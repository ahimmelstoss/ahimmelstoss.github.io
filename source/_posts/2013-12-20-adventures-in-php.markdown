---
layout: post
title: "Adventures in PHP"
date: 2013-12-20 17:52
comments: true
categories: 
---

LAF ("Life after Flatiron") has been a mixture of emotions, but has primarily meant that I'm exploring new technologies, languages and stacks. I'm currently learning PHP, among other things, for my LAF job. I'm finding that PHP isn't very different from Ruby; they're both scripting languages, so logically very similar. Problems I'm familiar with in Ruby can be easily solved in PHP, just with a different syntax.

I happily breezed through Codecademy's <a href="http://www.codecademy.com/tracks/php">PHP Track</a>, at a pace that made me feel quite pleased with myself. :) Three months ago, the Ruby Track took more than twice as long to go through.

Once I became familiar with the basic syntax of PHP, I decided to expand my learning by writing some simple programs that I had previously done in Ruby.

First, naturally, was FizzBuzz, written procedurally. For my own clarity and practice, I pushed the fizzes, buzzes, and fizzbuzzes into an array:

```php
<?php 
  function fizzbuzz() {
    $myFizzBuzzes = array();
    foreach (range(1, 50) as $num) {
      if ($num % 3 == 0 && $num % 5 == 0){
        array_push($myFizzBuzzes, "FIZZBUZZ");
      } elseif ($num % 3 == 0) {
        array_push($myFizzBuzzes, "fizz");
      } elseif ($num % 5 == 0) {
        array_push($myFizzBuzzes, "buzz");
      }
    }
    return $myFizzBuzzes;
  }
?>
```

Next, practicing switch statements (like case statements in Ruby), I wrote a vowel checker program. I included some syntactic sugar with the `endswitch;`:

```php
<?php 
  function isVowel($letter) {
    switch(strtolower($letter)):
      case "a":
      case "e":
      case "i":
      case "o":
      case "u":
        return "'{$letter}' is a vowel.";
        break;
      default:
        return "'{$letter}' is not a vowel.";
    endswitch;
  }

  echo isVowel("A");  
?>
```

As a fan of object orientation (being an organizational freak, I guess it makes sense that I would love classes), I created a Triangle class which will return the type of triangle, given the sides. Additionally, I tried my hand at some Exception throwing, which throws an error if the sides do not make a valid triangle. Exceptions, like in Ruby, are classes that your custom class can inherit from.

```php
<?php
  class TriangleError extends Exception {}

  class Triangle {
    public $side1;
    public $side2;
    public $side3;

    public function __construct($side1, $side2, $side3) {
      $this->side1 = $side1;
      $this->side2 = $side2;
      $this->side3 = $side3;
      $this->validate();
    }

    public function validate() {
      if ($this->side1 <= 0 || $this->side2 <= 0 || $this->side3 <= 0) {
        throw new TriangleError("error; not valid triangle");
      } elseif ($this->side1 + $this->side2 <= $this->side3 ||
    $this->side1 + $this->side3 <= $this->side2 || $this->side2 + $this->side3 <= $this->side1) {
        throw new TriangleError("error; not valid triangle");
      }
    }

    public function kind() {
      if ($this->side1 == $this->side2 && $this->side2 == $this->side3) {
        return "equilateral";
      } elseif ($this->side1 == $this->side2 || $this->side2 == $this->side3 || $this->side1 == $this->side3) {
        return "isosceles";
      } else {
        return "scalene";
      }
    }
  }

  $myTriangle = new Triangle(6, -2, 7);
  echo $myTriangle->kind();
 ?>
```