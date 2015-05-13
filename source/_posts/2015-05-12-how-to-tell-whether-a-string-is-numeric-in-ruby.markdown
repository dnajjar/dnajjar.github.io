---
layout: post
title: "How to tell whether a string is numeric in Ruby"
date: 2015-04-17 22:15:50 -0400
comments: true
categories: Flatiron School
---
An issue that's come up for me several times in my three short weeks of learning Ruby is how to tell whether a string is numeric. In other words, given a string, is there some way to tell whether it's really just an integer masquerading as a string?

Why, you might ask, would an integer be masquerading as a string in the first place? Identity crises aside, let's say you build a program that asks the user for input. Let's use a jukebox command line app as an example. You list all the songs you can play for your user by name and number, and ask them what they'd like to hear. Your code might look something like this:

> user_input = gets.chomp
Let's say you want to support different kinds of input: your user has the option of giving you a song name, or of choosing a song by number. How your code responds to their input will have to depend on whether they entered a song name - in which case you would need to retrieve the song based on the string they entered - or a song number - in which case you would need to retrieve the song indexed at the number they entered. You might be thinking: why don't we just call user_input.class and know right away what we're dealing with? Unfortunately, it's not that simple. The "s" in "gets" stands for "string", so anything the user types gets converted into a string. But don't panic! There are several different ways to tell if your string is really just a number dressed up in fancy quotes. Let's look at 3 different ways to write an integer? method that returns true if our string is an integer in quotes, and false otherwise. All three can handle positive and negative integers, but only the first (regex way) can handle floats.

The Regular Expressions Way
```
> class String                                       
>   def integer?                              
>     /\A[-+]?\d+\z/ === self
>     end
>   end
```

This regular expression is giving the following instructions: go to the beginning of the string and look for 0 or 1 "+" or "-" signs, followed by one or more digits, followed by the end of the string. Let's look at some examples:

```
> "-3".integer? # => true
> "2".integer? # => true
> "one".integer? # => false 
> "ISWEARIMANUMBER".integer? # => false 
> "12345SURPRISE789".integer? # => false 
```

If you wanted to update your regular expression to handle floats, then these are the instructions you'd want to give: start at the beginning of the string, look for 0 or 1 "+" or "-" signs, followed by 1 or more integers, followed by 0 or 1 "."'s, followed by one or more integers, followed by the end of the string. Here's what that looks like:
```
/^[-+]?[0-9]+\.?[0-9]+$/
```
The Easy Way

I like this method best. It's simple and elegant, and takes advantage of the fact that you can call to_i on any string in Ruby, and get back "the result of interpreting leading characters in str as an integer" (from www.ruby-doc.org). What this means is that Ruby will look for an integer in your string, and once it finds one, discard any trailing characters. If it doesn't find any integers, it returns 0. So "1".to_i is 1, while "one".to_i is 0, and "99 red balloons".to_i is 99. Calling to_s on the result will return the original string if and only if it was an integer without any trailing characters.
```
> class String
>  def integer?
>    self.to_i.to_s == self
>  end
> end
```
Let's look at how this works:
```
> my_string = "1"
> result = my_string.to_i # => 1 
> result.to_s # => "1" 
> result == my_string # => true 
> my_string = "one"
> result = my_string.to_i # => 0
> result.to_s # => "0" 
> result == my_string # => false
```

Now let's use the integer? method

```
> "one".integer? # => false 
> "1".integer? #=> true 
> "99 red balloons".integer? # => false 
```
The only shortcoming of this method is that while you could write a regular expression to match decimals, calling to_i will always return an integer.

The Hard Way

Here's another way you could do it, though somewhat less intuitive. Calling Integer(str) with a string argument will do one of two things:

1- Return the string.to_i if string.to_i is not 0 
2- Throw an error if string.to_i is 0

We ultimately want our integer? method to return a boolean, but all Integer(str) does is either return an integer or throw an error. We can fix that by calling !!Integer(str) and putting that in a begin rescue block that returns false, so that our method returns true if Integer(str) returns an, integer and false if it throws an error. This works because integers are truthy in Ruby, meaning the return value of !int is false, and of !!int is true. Keep in mind that if we didn't specify a return value of false after the begin rescue block, it would default to nil.

```
> class String
>   def integer?
>    begin
>       !!Integer(self)
>     rescue ArgumentError, TypeError
>       false
>     end
>   end
>  end
> "one".integer? # => false
> "1".integer? # => true
> "1.1".integer # => false
```

