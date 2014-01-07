---
layout: post
title: "Forms in PHP"
date: 2013-12-26 20:37
comments: true
categories: 
---

Today I continued in my exercise in familiarizing myself with PHP's syntax. I rewrote an old program I did two months ago in Ruby, a <a href="http://ahimmelstoss.github.io/blog/2013/10/17/roman-numerals-converter-explained/">Roman Numerals Converter</a>, drilling in classes, array iterations, and few new methods and tricks (like the `end();` method that gives you the last element in an array).

I decided to turn this converter into something interactive, using good ol' HTML forms. One of the cool things about PHP is that you can get a dynamic website up and going pretty quick, without a framework in place, so making a converter where a user can enter a number and get back the Roman numeral for that was relatively quick and clean. In fact, rendering HTML forms with PHP is not unlike ERB (embedded Ruby).

In a .php file, you set up your HTML as you like, and if your logic isn't in the file (I think best practices is to keep logic and rendering separate), you be sure to `include();` the file you wish to use. Be sure to `include();` above where you want to work with it on the page. 

I'll admit that my time in Rails led me to momentarily forget how to write a traditional form. Mine is relatively simple:

```html
  <form action="romanconverter.php" method="get">
    Number: <input type="text" name="number">
    <input type="submit" value="convert!">
  </form>
```
Next comes the rendering of that get request. PHP makes this relatively easy:

```php
  <?php if (!isset($_GET["number"])): ?>
  <?php elseif (isset($_GET["number"]) && is_numeric($_GET["number"]) && $_GET["number"] >= 1): ?> 
    <p>Your Number: <?php echo htmlentities($_GET["number"], ENT_QUOTES); ?></p>
    <?php 
    $myRoman = new Roman($_GET["number"]); 
    $mynumber = $myRoman->toRoman();
    ?>
    <p>Roman Number: <?php echo htmlentities($mynumber, ENT_QUOTES); ?> </p>
  <?php else: ?>
    <p>Error! Please enter a valid number.</p>
  <?php endif; ?>
```
I included some validation on the `$_GET` to make sure that something is in the input (`isset();`), the input is a number (`is_numeric();`), and that it is greater than 0, as the Roman numerals system does not have a zero. Encapsulated is the calling of a new instance of the `Roman` class, and the calling of a function within that class converts the number (in PHP, the syntax for calling a class function is different than calling a regular one: `$myRoman->toRoman();`). The `<?php  ?>` brackets around the if, elseif, else, and endif are just one way of embedding PHP into a page (you can instead `echo` your HTML tags and text within one big PHP block); I think this looks cleaner.

In the lines that render the number, for example: `<p>Roman Number: <?php echo htmlentities($mynumber, ENT_QUOTES); ?> </p>`, `htmlentities();` is a best practices function within PHP that makes sure the dynamic data you're rendering is escaped and always rendered as HTML. The `ENT_QUOTES` included in the function's parameters is a flag that let's PHP know how to handle quotes: in this case it will convert double and single quotes.

One cool thing about PHP is the `$_GET` variable; this is called a `superglobal` array. There is also `$_POST`, `$_REQUEST`, `$_SERVER`, `$_ENV`, `$_SESSION`, and `$_COOKIE`. These superglobal arrays are very similar to `params[]` found in Ruby on Rails and Sinatra. These arrays are where PHP stores values sent into a script from an HTML from the GET, POST, etc methods. Whereas `params[]` can hold data from any type of action, the differentiation between get, post, etc gives some added security.

If you want to play with my Roman Numerals Converter, it's <a href="http://www.amandahimmelstoss.com/romanconverter.php">here</a>!