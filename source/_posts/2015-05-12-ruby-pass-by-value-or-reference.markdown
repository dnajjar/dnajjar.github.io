---
layout: post
title: "Ruby: Pass by Value or Reference?"
date: 2015-04-30 22:21:09 -0400
comments: true
categories: Flatiron School
---
##The confusing part

If you ask the internet whether Ruby is a pass-by-value or pass-by-reference language, it will resoundingly answer that ruby is pass by value. End of story. But if you listen carefully, you can hear a few muffled voices yelling that this is nowhere near the end of the story. The whole truth is that Ruby does things a little differently, and a precise answer would be that Ruby passes arguments by value, and values by reference. Don't worry, that made no sense to me the first 20 times I read it either. But first things first. What does it mean to pass by reference or value?

When you instantiate a variable, you're telling your computer that you want to store that variable somewhere in memory.
```
> str = "hello"
> str.object_id #=> 70219331102480 
```
In this case, we're creating a variable "str" that points to a location in physical memory where the string "hello" is being kept (the object_id references that location).

In Ruby, when you create a second variable and set it equal to the first, 
the second variable is now pointing at the same location in memory as the first.
```
> str2 = string
> str2.object_id #=> 70219331102480
```
str2 has the same object id as str, which tells us that they're both pointing at the same object in memory. Here's a good visualization from Stack Overflow



Keep this in mind and we'll come back to it soon. Let's switch gears a little bit and think about what happens to a variable when it gets passed to a method. In other words, how is it getting passed? What does the method know about its caller?

Programming languages can handle this in one of two ways: the argument can be passed directly into the method, in which case the method has full access to it. There is no difference between the argument outside the method and the argument inside of the method. This is called passing by reference.

Alternatively, the method can get its own copy of the argument which is identical to the first but points to a different location in memory. This is called passing by value (the method only has access to the value of the argument, not its reference in memory).

So far so good.

So why isn't this the end of the story? Because Ruby does things a little differently. When a variable calls a method in Ruby, the method is not given full access to that variable. In other words, the variable is not passed by reference. On the other hand, the method is not given its own separate copy, so the variable is not passed by value either. How is this possible? Because the method is given a copy of the variable pointing to the same location in memory. Remember the first thing we talked about? That's what's happening (now would be a good time to peek at the drawing). The argument being passed into the method is a copy of the original argument, but points to the same reference in memory.

This has a lot of interesting (and confusing ramifications). Let's look at some examples.
```
> def angry(string)
>  string = "SO ANGRY"
> end
> some_string = "warevs"
> angry(some_string) #=> "SO ANGRY"
> some_string #=> "warevs"
```
The method didn't modify the original string, even though it had access to it. How is this possible? Because the method was given a copy of the object, not the object itself. When the method reassigned its copy and told it to point somewhere new in memory, the original object remained blissfully unaware. Let's try another example.
```
> def please_leave(string)
>  string.replace("goodbye")
> end
> some_string = "hello"
> please_leave(some_string) #=> "goodbye"
> some_string #=> "goodbye"
```
Unlike the first example, the please_leave method changed the argument that got passed into it. WHAT? For the uninitiated, it might look like Ruby is an extremely unreliable language. After all, where would programming be without determinism? You may be able to guess what's coming next: Ruby's behavior is still completely deterministic.

##The Simple Truth
What it all comes down to is the return values of different methods. 
Again, what? It's pretty simple: some methods return new objects with new addresses in memory, and some methods modify their original callers and then return them.

Let's look at a method that returns an entirely new object than the one that called it.
```
> string = "hello"
> loud_string = string.upcase #=> "HELLO"
> string.object_id      #=> 70248691530340
> loud_string.object_id #=> 70248691495780
```
Notice that string and loud_string have different object ids. They are different objects, and are stored in different memory locations. 
Now let's look at a method that modifies its caller and then returns it:
```
> array = [1,2,3]
> new_array = array.push(4) #=> [1,2,3,4]
> array.object_id          => 70248691406240
> new_array.object_id      => 70248691406240
```
Notice that array and new_array have the same object ids, even though they are different arrays. The push method reached into the location where array was stored in memory, and changed its contents. It returned the same object, but only after it had modified it. Think of this like reaching into a box, modifying the contents, and then putting it back in its place. You haven't created a new box, just changed the contents of the existing one. (Just in case that wasn't clear, the location in memory is the box, the object is its contents).

##Wrapping it up
To summarize, a method's copy of its argument is pointing at the same location in memory as the argument itself. If the method is one that returns a new object, the original argument will remain unchanged outside the method. On the other hand, if the method is one that modifies its argument, all copies of the argument will now point at a location whose contents have been modified (think box again.)

How on earth are you supposed to know the return values of every method in Ruby? It will come with practice, and keep in mind that adding a ! to a method tells it to modify the original object instead of returning a new one.
```
> string = "hello"
> loud_string = string.upcase! #=> "HELLO"
> string.object_id      #=> 70248691530340
> loud_string.object_id #=> 70248691530340
```
Finally, some objects in Ruby have fixed ids, meaning that they will always point to the same location in memory. Integers, true, false and nil have their own special homes. The integer 5 will point to 11 on Monday, Tuesday and every other day of this week and every week. What this means is that a method will never be able to modify an argument if that argument is one of these special objects, because any modification will automatically be a reassignment. There is no way to reach into an integer's box and change the contents, because Ruby decided that nothing but that integer can ever be in that box.
```
> def change(num)
>  num+1
> end
> num = 5
> new_num = change(num) #=> 6
> num.object_id #=> 11
> new_num.object_id #=> 13
```