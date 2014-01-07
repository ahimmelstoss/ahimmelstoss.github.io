---
layout: post
title: "JavaScript vs. Ruby"
date: 2013-11-17 14:49
comments: true
categories: 
---

After beginning JavaScript last Thursday, I've been thinking about the differences between this language and Ruby, whom I've gotten quite intimate with these last 7 weeks. Coming from Ruby, JavaScript isn't all that different, yet the syntax is unfamiliar and some logic differs slightly.

**`typeof` vs `class`**

To determine what type of value something is (a number, a fixnum, a string, an Object, an array, etc), in JavaScript you write `typeof` and then then the object; in Ruby, you append `.class` to the end of the object.

**`===` vs `==`**

In JavaScript, the triple equals sign helps determine if two objects are the same exact object (having the same `typeof` and the same value). For example:
`3 === 3` returns `true`
In Ruby, that is accomplished through the double equals. (`3 == 3` returns `true`)

JavaScript has its own double equals, which shouldn't be confused with Ruby's: double equals in JavaScript only determines if the values match. For example:
`"3" == 3` returns `true`

**`parseInt();` vs `.to_i`**

In JavaScript, to turn a string "10" into the integer 10, you write `parseInt("10");`. In Ruby, you write `"10".in_i`.

**Incrementing and Decrementing**

To increment or decrement by 1 in JavaScript, you can write `++` or `--` (for example, `var x = 2; x++;` returns `3`. In Ruby, it's `x += 1`. You can write this as well in JavaScript (which is useful if you want to increment with numbers other than 1), however, you have the handy `++` and `--` as well.

**Functions**

Functions in JavaScript are the same as methods in Ruby. The syntax of declaring a function vs. declaring a method is slightly different. In Ruby, it's :
```ruby
def hello
  puts "hello"
end
```
In JavaScript, the same logic can be written as:
```javascript
function hello() {
  console.log("hello");
}
```

**Objects**

Objects (similar to Ruby's hash), declared as variables, are a way of organizing key/value pairs. For example, `var cat = {color: "grey", name: "Spot", age: "12"};`
To access the value of an associated key, you can call that on the variable name:
`cat.name` returns `"Spot"`. You can also easily delete a key/value pair by writing `delete cat.name`. To declare a new key/value pair, write: `cat.gender = "male";`. This appends this new data point to the end of the Object.

**Arrays**

Both Ruby and JavaScript have Arrays, which are more of less logically the same; however, the Object Array functions (JavaScript) and the Class Array methods (Ruby) have different syntax.
For example, to find the index of an element in an array, in JavaScript you write `arrayName.indexOf("element");`. In Ruby, this is accomplished by `array_name.index("element")`

**`each`**

The method `each` in Ruby is a handy way of iterating through an array of elements in order to change or manipulate them. JavaScript as well as an `each` function that does the same thing; however, the syntax is slightly different:
```javascript
var arr = [1,2,3,4];
a.forEach(function(n) {
  console.log(n);
});
```
And in Ruby, the same is written as:
```ruby
arr = [1,2,3,4]

a.each {|n| puts n}

#or

a.each do |n|
    puts n
end
```

**Falsey Values**

One big difference between Ruby and JavaScript is the falsey-ness of certain values. Like in Ruby, null (in Ruby, it's nil) and false will return false, however, some truthful values in Ruby will return false in JavaScript, like `0`, empty strings (`""`), and `undefined`.


**Object Oriented Programming**

JavaScript, like Ruby, can be object-oriented, meaning that objects, their associated methods/functions, can be neatly contained. In JavaScript, a Prototype is much like Ruby's Class. To create a new Prototype in JavaScript:
```javascript
function Cat(name, age, gender){
  this.name = name;
  this.age = parseInt(age);
  this.gender = gender;
  this.meow = function(){
    console.log("meow, my name is "+this.name+"! I am "+this.age+" years old.");
  };
};

var maruCat = new Cat("Maru", "6", "male");
maruCat.meow(); //meow, my name is Maru! I am 6 years old.
```
The `maruCat` object is considered a new instance of the Cat prototype. Prototype is essentially the same as Class in Ruby. Note that `this` in JavaScript is the same concept as `self` in Ruby.
