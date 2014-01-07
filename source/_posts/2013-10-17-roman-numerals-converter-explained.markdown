---
layout: post
title: "Roman Numerals Converter Explained"
date: 2013-10-17 21:40
comments: true
categories: 
---

I'm going to spend this post explaining a roman numeral converter I made for one of our morning todo assignments. I struggled with this assignment, so I thought the best way to really drive it home would be to explain it.

First, within the `Integer` class, I defined as a constant variable a hash of that is representative of all of the roman numeral characters within their counting system. I cheated a little bit and included instances of 4 and 9, because those are a bit complicated.

```ruby
  NUMERALS = 
   { 
    1 => "I",
    4 => "IV",
    5 => "V",
    9 => "IX",
    10 => "X",
    40 => "XL",
    50 => "L",
    90 => "XC",
    100 => "C",
    400 => "CD",
    500 => "D",
    900 => "CM",
    1000 => "M" 
  }
```

The instance of the `Integer` class method is a number, which I called in the initialize method. 

```ruby
def initialize(number)
  @number = number
end
```

Once I got that out of the way, I began to think about the logic of the roman numerals. Generally, it's addition and substraction. The number 40, for example is representated as XL. L is 50 and X is 10. 50(L) - 10(X) is 40. Or, the number 13 is XIIII. 10(X) + 1(I) +1(I) +1(I) is 11. The relationship is one of addition and subtraction when you are trying to represent a number that is not a singular character within the roman numeral library.

How to execute this relationship through code is the complicated part. I am at that point in my learning where I know what I want to accomplish with code, but the execution doesn't come completely naturally. If I compare it to language learning, I am in the recognition stage and still somewhat fumbling with the recollection.

I executed this converter with two methods. The first method, `lower_key`, takes an instance number as an argument, and returns the largest number below it which has an associated roman numeral. `to_roman` uses the return of `lower_key` to help generate the roman numeral associated with the instance of `number`. I'll discuss `lower_key` first.

First, I create a new array, `numbers_array`, and I `.collect` all of the numbers in my `NUMERALS` hash and sort them:

```ruby
numbers_array = NUMERALS.collect {|k,v| k}.sort
```

This gives me an easy array of the numbers to work with when I compare them to `number`. 

Before I mentioned that the logic of roman numerals is that we're adding roman numeral characters together to make a number. In order to do that, we need to know the number right below it to work on that. To do this, I iterated over my new `numbers_array` with `.each_with_index`. In `numbers_array` the number (I called it `decimal`) below `number` can be found through first determining the number above it. Then, using its `index`, I can access the number below. Then, returning the `.last` in `numbers_array` accounts for if `number` is greater than the largest number in `NUMERALS`, like 3000.

```ruby
numbers_array.each_with_index do |decimal, index|
  if decimal > number
    return numbers_array[index - 1]
  end
end
return numbers_array.last
```

Onto the `to_roman` method. First, I confirmed that `number` is `self`. I then set a variable `roman_numeral = ""`; I will be pushing into this string below. 

The bulk of the method in enclosed in a `while` loop. If `NUMERALS` includes `number`, set that `NUMERALS[number]` as `roman_numeral`. This takes care of instances of `number` that are simple and already defined in `NUMERALS`, like 5(V). For more complicated instances, we loop beyond this. 

Let's take a more complicated number like 51(LI) as an example to follow through. It doesn't apply yet to the `if` block, but instead is passed into the `else` first. `lower_key(51)` is called, which returns 50. "L" is added into `roman_numeral` (`roman_numeral += NUMERALS[50]`). Then the remainder of that (`number -= 50`) is 1, which is passed up and looped through. `NUMERALS` has that as a key, where the value is "I", and that is pushed into `roman_numeral`, right behind the "L" already in there from before. "LI" is returned.

```ruby
while number > 0
	if NUMERALS.has_key?(number)
	  roman_numeral << NUMERALS[number]
	  return roman_numeral
	else
	  roman_numeral += NUMERALS[lower_key(number)]
	  number -= lower_key(number)
	end
end
```

Below is the entire program.


```ruby
class Integer

  NUMERALS = 
   { 
    1 => "I",
    4 => "IV",
    5 => "V",
    9 => "IX",
    10 => "X",
    40 => "XL",
    50 => "L",
    90 => "XC",
    100 => "C",
    400 => "CD",
    500 => "D",
    900 => "CM",
    1000 => "M" 
  }

  def initialize(number)
    @number = number
  end

  def lower_key(number)
    numbers_array = NUMERALS.collect {|decimal, roman| decimal}.sort
    numbers_array.each_with_index do |decimal, index|
      if decimal > number
        return numbers_array[index - 1]
      end
    end
    return numbers_array.last
  end

  def to_roman
    number = self
    roman_numeral= ""
    while number > 0
      if NUMERALS.has_key?(number)
        roman_numeral << NUMERALS[number]
        return roman_numeral
      else
        roman_numeral += NUMERALS[lower_key(number)]
        number -= lower_key(number)
      end
    end
  end
end
```