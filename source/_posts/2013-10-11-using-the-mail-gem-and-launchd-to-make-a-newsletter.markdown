---
layout: post
title: "Using the Mail Gem and LaunchD to Make a Newsletter"
date: 2013-10-11 16:03
comments: true
categories: 
---

I had a lot of fun learning to scrape, because I saw the potential to use that to automate some of my online activities. I decided to create a scrape of a blog I like to check every week (when a certain column I like is usually published). With that, I wanted the scraped data (generated into HTML) to be emailed to me once a week at a certain time (after the column is published).

The first step requires the Ruby Mail Gem. It's pretty simple. First:

`gem install mail`

Then, at the top of my scrape program I required it: 

```ruby
require 'mail'
```

Next, within my program, I set the mail options, which I figured out from the <a href="https://github.com/mikel/mail">gem documentation</a> and some googling about gmail configurations:

```ruby
mail_options = { :address => "gmail-smtp-in.l.google.com",
                 :port => 25 }
```

Then, I set the mail defaults, which sets the method of deliver to `:smtp` and my set `mail_options` above: 

```ruby
Mail.defaults do
  delivery_method :smtp, mail_options
end
```

After that, I entered the delivery options. As far as I could tell, you can only send from one gmail from another, not any other email server. Additionally, because gmail is particular, if I send an email to myself externally like this, it won't show up in the inbox, but the sent messages. To bypass this, I sent the email from my junk email address that I keep for purposes such as this. In the subject line, I used string interpolation to make the subject be the title of the article. In the `text_part`, I set the body to say something like "this email is not supported in plain text". This message will never be seen in gmail; it's for mail clients that don't support HTML. The real body of the email is in the `html_part`. I set the `content_type` to 'text/html; charset=UTF-8', and the body the interpolation of the article content, from my scrape.

```ruby
Mail.deliver do
  to 'my email address'
  from 'my junk email address'
  subject "#{article_title}"

  text_part do
    body "this email is not supported in plain text"
  end

  html_part do
    content_type 'text/html; charset=UTF-8'
    body "#{article_itself}"
  end
end
```

Running my program from the command line, it sends the email!

I then wanted to automate this even further by getting the email sent to me only once a week, at a certain time. Some research lead me to a feature within Mac OSX that automatically runs programs upon certain parameters. This is the same feature that automatically launches certain programs upon rebooting the machine. It's called launchd or launchctl. 

I figured this out, again, through some googling. <a href="http://stackoverflow.com/questions/132955/how-do-i-set-a-task-to-run-every-so-often">This helpful comment</a> on Stack Overflow gives a good explanation on how to use <a href="https://developer.apple.com/library/mac/documentation/macosx/conceptual/bpsystemstartup/chapters/CreatingLaunchdJobs.html#//apple_ref/doc/uid/TP40001762-104142">Launchd</a> to run jobs.

It requires making a `.plist` file in `~/Library/LaunchAgents`. When I went into this folder, I found a file already in there that launches Spotify upon rebooting (mystery solved on that). Before deleting that file, I copied the template to make my file, `com.myname.myprogram.plist`.

```ruby
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
 <key>Label</key>
 <string>com.myname.myprogram</string>
 <key>StartCalendarInterval</key>
 <dict>
  <key>Hour</key>
  <integer>12</integer>
  <key>Minute</key>
  <integer>0</integer>
  <key>Weekday</key>
  <integer>4</integer>
 </dict>
 <key>ProgramArguments</key>
 <array>
  <string>/Users/username/.rvm/rubies/ruby-1.9.3-p448/bin/ruby</string>
  <string>scrape program's location</string>
 </array>
</dict>
</plist>
```
The important part of this was setting `StartCalendarInterval` to the day, hour, and minute. This tells the computer to run the program (in the `ProgramArguments`) when indicated. Within the `ProgramArguments` I indicated the location of ruby, and the location of my program.

To test to make sure the `.plist` file works, not just at the time I indicated, I ran this:
`launchctl start com.myname.myprogram`

Waiting in my inbox moments later was my newsletter. :)