---
layout: post
title: "The SplFixedArray Class"
date: 2013-12-31 18:56
comments: true
categories: 
---

Another type of data structure that is unique to PHP is the SplFixedArray. This is a class that embodies much of the same functionality as an indexed array, but performs slightly different. 

Arrays in PHP, while functionally similar to arrays in other languages, like Ruby, more closely resemble hashes, as both indexed and associative arrays both use key => value pairs, where the key can be either an integer or a string. However, SplFixedArrays are not unlike arrays found within Ruby. Their indexes are fixed integers, and their performance is better on a large scale than an associative array.

An SplFixedArray has a fixed size (other arrays in PHP can be any size) that is determined in the  instantiation. Below, I've instantiated a new SplFixedArray (remember, it's a class!) and set four of the values.

```php
$cats = new SplFixedArray(4);
$cats[0] = "Maru";
$cats[1] = "Grumpy Cat";
$cats[2] = "Nala Cat";
$cats[3] = "Chairman Meow";
```
To increase the size of `$cats`:

```php
$cats->setSize(5);
```

The value of `$cats[4];` will be NULL until it's set. (Note that `setSize();` is a class function.)

The SplFixedArray is a great data structure that is more optimal in some cases, and for the Rubyists who love Ruby arrays, very familiar. :)