---
layout: post
title: "Arrays in PHP, Continued"
date: 2013-12-27 12:38
comments: true
categories: 
---

In continuing to learn about arrays and data structures in PHP, I've been playing around with a few cool functions.

`array_column();` is a handy method in PHP5.5 that returns all of the values of a single column, given the key.

Let's say I had the following associated array:

```php
$catCelebs = array(
    array(
        'name' => 'Maru',
        'breed' => 'Scottish Fold',
        'age' => 6
    ),
    array(
        'name' => 'Nala Cat',
        'breed' => 'Siamese mix',
        'age' => 3
    ),
    array(
        'name' => 'Lil Bub',
        'breed' => 'Domestic Shorthair',
        'age' => 2
    ),
    array(
        'name' => 'Tardar Sauce',
        'breed' => 'Siamese',
        'age' => 1
    )
);
```

If I wanted all of the cat's names, I would write:
```php
$catNames = array_column($catCelebs, 'name');
```

`array_key_exists();` is a function that checks if an array contains a given key, returning true or false.

From `$catCelebs` above, if I wanted to check if age was an included column, I would write:
```php
if (array_key_exists('age', $catCelebs)) {
  echo "We know the ages of the celebrity cats!";
}
```

Similar to this are two other useful functions that allow you to search arrays: `in_array();` searches an array for a value and returns true or false if it exists, and `array_search();` looks for a value and returns its key if it exists, and false otherwise.

`array_unique();`, much like the name suggests, will remove duplicates from a given array, the return of that being a new array. This function does not change the original array, so reassignment would be necessary.