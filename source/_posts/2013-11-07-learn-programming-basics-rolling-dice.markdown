---
layout: post
title: "Learn Programming Basics Rolling Dice, Part 1"
date: 2013-11-07 19:34
comments: true
categories: ruby, coding
---
I started dabbling into the coding arts a few months ago (hence Code Gazer). I read Chris Pine’s *Learn to Program*. This is my attempt to elaborate on Pine’s Die Example to further understand the relationships between objects, classes, variables and methods.

**Pine Example:** Create a die that gives you a random number when you roll it.

```ruby
class Die         #create a class
  def roll        #create method
    1 + rand(6)     #method logic
  end
end
```

Everything is an object. Class is an object that allows us to make new classes of objects and defines how those classes behave. A method is the behavior of those objects.
A die is not a basic object in Ruby (i.e. range, integers, strings, arrays and hashes) so in this example, we need to create a new object. We can do that by creating a new class. To create a new class, we use a construct called `class` (which defines a class's behavior) and a class name, `Die`. The first letter of a class name is always capitalized.

<!-- more -->

After creating a class, we need to define its behavior. We can do that by making a method (behavior) that applies to the `class Die`: use the word `def` followed by the method name, `roll`. We define `roll` through the logic `1 + rand(6)` which says "return a random number between 1 and 6."

Simple enough? Let's go to the next example.

**Code Gazer Die Problem 1:** Create one die that gives you a random number and color when you roll it.

```ruby
class Die
  attr_accessor :num, :color

  @@colors = {1=>"yellow", 2=>"blue", 3=>"green", 4=>"black", 5=>"white", 6=>"red"}     #class variable hash

  def initialize
    @num = nil
    @color = nil
    roll
  end

  def roll
    @num = 1 + rand(6)                                                                  # instance variable
    @color = @@colors[@num]                                                             # hash key look-up
    puts "You rolled a #{@num} with color #{@color}"
  end
end
```

Let's take it one step at a time. What do we need?

**We need a die.**
To create an object, we need to create a new class: `class Die`.

**Each side of the die must have a number and color that correspond with each other.**
  We need a container (variable) that will hold our numbers and colors. A variable is a storage location containing values. Variables are very useful because we can use them as reference, allowing us to reuse those values at a later time. Here, we need to create a class variable because we want the `@@colors` to be available to all methods in the class `Die`. Notice the `@@` symbol before the word colors? That means colors is a class variable.

  Inside the variable, we need slots to hold our numbers and colors. A `Hash` is a perfect object for that. A hash variable is like a dictionary: you have a word (the key) and its corresponding definition (the value). Here, we created a `Hash` containing a set of colors (values) that correspond to each number in the die (keys). Like this:

```ruby
@@colors = {1=>"yellow", 2=>"blue", 3=>"green", 4=>"black", 5=>"white", 6=>"red"}
```
**When we create the object, die, it needs to roll.**
  Whenever we make a new object, the initialize method, `def initialize`, allows us to set up the object's initial state: we can provide default values to the attributes of the object (instance variables `@num` and `@color`). Everything we put in the initialize method (like `roll`) becomes a general/default rule and is automatically executed when you create a new object: `d = Die.new`.

**When we roll the die, we get a random color and number.**
  The die will roll automatically because it's being called inside the initialize method, but it doesn't give us a random color or number yet. We need to create a new method to define the behavior of the Die when it rolls: `def roll`.

  To see what color and number the die picks, we need to define variables we set up in the initialize method. We use the `@` symbol instead of `@@` because we want instance variables, not a class variable. Unlike a class variable (which is commonly available to the entire `class Die`; this means it's available for class methods (more on that another time) and instance methods, i.e. `roll`), each instance of a class has its own set of instance variables. So here, `@num` and `@color` applies only to the specific instance created via `Die.new`.

  We need some logic that allows us to get a random number between 1 - 6 every time the die rolls. We store the number picked in `@num` so we can reference back to `@@colors` to see which color corresponds with the number. To do that, we set `@color` as a key look-up on the `@@colors` hash.
```ruby
@num = 1 + rand(6)
@color = @@colors[@num]
```

**Did we forget something?**
  If we create an object, `d = Die.new`, the program has access to the random color and number, but the object itself can't retrieve variables. For example, `d.num` and `d.color` gives us:

```ruby
NoMethodError: undefined method 'num' for #<Die:0x007fdde294b778 @num=1, @color="yellow">
```

  We can see the object has `@num=1, @color="yellow"` (Ruby is calling the inspect method on the object, which irb is returning) in it, but we can't retrieve and use it. To do that, we can use a Ruby construct: `attr_accessor`. This construct is a shortcut given to us by the Ruby Gods to help us with our code crafting.

  `attr_accessor` provides two methods automatically for the developer: a 'getter' method and a 'setter' method. The sole purpose of the getter is to read/return the value of a particular instance variable. The setter, on the other hand, assigns/sets the value for a particular instance variable. When we write,

```ruby
attr_accessor :num, :color
```

  we're telling our program to build these methods behind the scenes:

```ruby
#getters
def num
  @num
end

def color
  @color
end

# setters
def num=(value)
  @num = value
end

def color=(value)
  @color = value
end
```


  I think we have everything we need. Let's run the program and see what we get:

```ruby
2.0.0p247 :049 > d = Die.new
You rolled a 6 with color red
 => #<Die:0x007fa7889321f0 @num=6, @color="red">
```

  Awesome. Let's try it a couple of times to see if it gives us a random color and number.

```ruby
d.roll => "You rolled a 4 with color black"
d.roll => "You rolled a 2 with color blue"
d.roll => "You rolled a 1 with color yellow"
```

  It works! Yay!

Phew! I hope I explained those concepts clearly and sufficiently. On my next post, I am going to change it up a bit and create two different dice, one colored and one numbered, so we can learn about class inheritance.

*Don't impose expectations onto things. Let things be. Abandon your hopes and fears. They're irrelevant.*
