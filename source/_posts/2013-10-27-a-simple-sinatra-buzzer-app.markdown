---
layout: post
title: "A Simple Sinatra Buzzer App"
date: 2013-10-27 23:03
comments: true
categories: 
---

This weekend, I wrote a simple door buzzer app that requires authentication before execution. The point of this is to be able to buzz open our downstairs doors from anywhere (particularly useful when hosting airbnb guests). My boyfriend did all of the methanical work: we got a raspberry pi (which we turned into a server to host the app) and a relay to connect that to the door buzzer (there was sautering and wires involved; I just watched). Through that, it's possible to trigger the buzzer from the command line of the raspberry pi. We (well, mostly my boyfriend; I, again, just really watched) wrote a simple C command to trigger the buzzer for 5 seconds, long enough for someone to get through both doors downstairs:

```
cd /sys/class/gpio
echo 2 > export
echo out > gpio2/direction
echo 0 > gpio2/value
sleep 5
echo 1 > gpio2/value
```


{% img /images/buzzer.jpg 400 400 buzzer %}

I approached this project not necessarily as a big challenge, but as a way to drill in the skills we've been working on this week into a cool, useful project. This is the second personal project I've worked on during school, but one that was most rewarding. To see all the components come together to make something that is truly useful was super awesome for me. I had been vaguely envisioning writing a door buzzer app for quite sometime, but I didn't have the skills yet to accomplish that. It was an empowering to realize I was at a point in my learning where I could build something that 4 weeks ago, I wouldn't have known how to even begin building.

This was an exercise in Sinatra, Ruby, ERB/HTML, SQLite/Sequel/migrations, and general project organization. From scratch I had to set of the program environment and config.ru files, which I can always use some practice with. I won't bore you with that part, but instead get to the meat of the application.

I have only two models in this application: `Buzzer` and `User`. 

`Buzzer` only knows how to do one thing right now, which is trigger the C command that in turn triggers the buzzer itself:

```ruby
def open_door
		system("/usr/local/sbin/open-buzzer")
	end
```

`User` is more advanced, because it interacts with the database, which I created with Sequel and migration methods. `User` could have used more advanced authentication methods, or even integrated the <a href="https://github.com/maxjustus/sinatra-authentication">sinatra-authentication</a> gem, but I thought it was best to keep it simple given the nature of the project.

The app requires you to log in in order to trigger the buzzer, but there's no way to sign up to that. Usernames and passwords can only be entered manually into the database. So therefore, `User`'s only tasks are to know if inputed usernames and passwords correspond with that is in the database:

```ruby
class User < Sequel::Model
	def username?(login_username)
		login_username == username
	end

	def password?(login_password)
		login_password == password
	end
end
```

Like many of my classmates, I'm a bit in love with Sinatra. I know nothing else to compare it to, so it's hard to give a solid argument why, but I love it for its simplicity and for its ability to teach me how websites are structured. `BuzzerApp` is rather simple, but it drives home web app structure for me, and gets the job done, which is great.

There are only three pages in the app: `login`, which authenticates the username and password input fields, `success` which lets you know you're in, and `failure`, which lets you know you don't have access. It's simple for a reason: there's no way for you to buzz in without entering a username and password, because a) there are no cookie sessions, and b) because an instance of `Buzzer` is only triggered through `/login`'s POST. The bulk of the app is within `login`: 

```ruby
class BuzzerApp < Sinatra::Base
	
	get '/' do 
		redirect '/login'
	end

	get '/login' do 
		erb :login
	end

	post '/login' do 
		login = User.find(username: params[:username])
		if !login.nil?  && login.username?(params[:username]) && login.password?(params[:password])
			buzz = Buzzer.new
			buzz.open_door
			redirect '/success'
		else
			redirect '/failure'
		end
	end

	get '/success' do 
		erb :success
	end

	get '/failure' do 
		erb :failure
	end
end
```
Below are the pages rendered by the .erb templates, which could use some styling, although the cats obviously make up for that:

{% img /images/login.png 600 600 login %}

{% img /images/success.png 600 600 success %}

{% img /images/failure.png 600 600 failure %}

Soon I hope to expand on this project to be able to receive output from the buzzer (to be notified when someone buzzes and then be able to respond with triggering the buzzer). Additionally, I am working on an email notification feature that will notify the administrator each time the buzzer is triggered. My takeaway from this, besides a super useful application, is that sometimes the best tools for a project are the most simple ones. Also, that Sinatra is awesome. :)
