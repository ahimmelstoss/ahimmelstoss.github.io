---
layout: post
title: "Arrays in PHP"
date: 2013-12-23 18:42
comments: true
categories: 
---

In Ruby, arrays and hashes are two separate types of data structures. Arrays have elements with corresponding indexes, whereas hashes you can set you own key => value pairs. In PHP, arrays and hashes exist, but are not so mutually exclusive. There are two types of arrays, `indexed` and `associative`. Indexed arrays, much like their name suggests, are much like Ruby's arrays, and associative arrays operate much like hashes.

What's interesting about the value of an indexed array is that it resembles that of an associative array: the keys are automatically assigned for you as an index for the values.

```php
<?php
//an indexed array of cat breeds
$myArray = array("siamese", "scottish fold", "calico", "ragdoll");
/* → array(
  0 => 'siamese',
  1 => 'scottish fold',
  2 => 'calico',
  3 => 'ragdoll'
) */

//an associative array of one cat
$maruCat = array("name" => "Maru", "age" => 6, "breed" => "scottish fold", "country" => "Japanese");
/*  → array(
  'name' => 'Maru',
  'age' => 6,
  'breed' => 'scottish fold',
  'country' => 'Japanese'
) */
?>
```

Arrays in PHP can also be nested. Below, I've taken an exercise we did during the first few weeks at Flatiron that gives a nested array of data about some pigeons (`$rawPigeonData`) and transforms the structure of it into a more organized list with each pigeon's name being the key in the associative array.

```php
<?php
$rawPigeonData = array(
  "color" => array(
    "purple" => array("Theo", "Peter Jr.", "Lucky"),
    "grey" => array("Theo", "Peter Jr.", "Ms .K"),
    "white" => array("Queenie", "Andrew", "Ms .K", "Alex"),
    "brown" => array("Queenie", "Alex")
  ),
  "gender" => array(
    "male" => array("Alex", "Theo", "Peter Jr.", "Andrew", "Lucky"),
    "female" => array("Queenie", "Ms .K")
  ),
  "lives" => array(
    "Subway" => array("Theo", "Queenie"),
    "Central Park" => array("Alex", "Ms .K", "Lucky"),
    "Library" => array("Peter Jr."),
    "City Hall" => array("Andrew")
  )
);
?>
```
`$pigeonList` is the new associative array I'm going to create by iterating through `$rawPigeonData`:

```php
<?php
$pigeonList = array();

foreach ($rawPigeonData as $property => $valueArray) {
  foreach ($valueArray as $value => $pigeonNames) {
    foreach ($pigeonNames as $name) {
      if (isset($pigeonList[$name])) {
        if (isset($pigeonList[$name][$property])) {
          array_push($pigeonList[$name][$property], $value);
        } else {
          $pigeonList[$name][$property] = array($value);
        }
      } else {
        $pigeonList[$name] = array($property => array($value));
      }
    }
  }
}

/* The array looks like this once it's structured:
 → array(
  'Theo' => array(
    'color' => array(
      0 => 'purple',
      1 => 'grey'
    ),
    'gender' => array(
      0 => 'male'
    ),
    'lives' => array(
      0 => 'Subway'
    )
  ),
  'Peter Jr.' => array(
    'color' => array(
      0 => 'purple',
      1 => 'grey'
    ),
    'gender' => array(
      0 => 'male'
    ),
    'lives' => array(
      0 => 'Library'
    )
  ),
  'Lucky' => array(
    'color' => array(
      0 => 'purple'
    ),
    'gender' => array(
      0 => 'male'
    ),
    'lives' => array(
      0 => 'Central Park'
    )
  ),
  'Ms .K' => array(
    'color' => array(
      0 => 'grey',
      1 => 'white'
    ),
    'gender' => array(
      0 => 'female'
    ),
    'lives' => array(
      0 => 'Central Park'
    )
  ),
  'Queenie' => array(
    'color' => array(
      0 => 'white',
      1 => 'brown'
    ),
    'gender' => array(
      0 => 'female'
    ),
    'lives' => array(
      0 => 'Subway'
    )
  ),
  'Andrew' => array(
    'color' => array(
      0 => 'white'
    ),
    'gender' => array(
      0 => 'male'
    ),
    'lives' => array(
      0 => 'City Hall'
    )
  ),
  'Alex' => array(
    'color' => array(
      0 => 'white',
      1 => 'brown'
    ),
    'gender' => array(
      0 => 'male'
    ),
    'lives' => array(
      0 => 'Central Park'
    )
  )
)
*/
?>
```