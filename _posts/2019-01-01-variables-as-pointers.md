---
layout:     post
title:     Variables as Pointers 
date:       2019-01-01
summary:    In Ruby, variables are considered more like pointers than storage boxes.
categories: ruby basics
---

There is a well-worn analogy that variables are like little storage boxes that store stuff — like a number or a string. However, in Ruby, we have to rewire our thinking and instead consider a variable to be a pointer.

So what are we pointing at?

Well, in Ruby, everything is an object right? So, a string or an integer — both objects. An array or a hash, also objects. An object in Ruby has a unique identifier and we can find an object’s identifier by calling the #object_id method on it.

```
a = 21
a.object_id
43
```

What’s just happened is that we just pointed the variable a to the integer 21. Internally, Ruby stores 21 in memory and assigns the unique identifier 43 to it. Now, what happens when we assign a new variable, b, to a?

```
b = a
b.object_id
43
```

Yup, the variable ‘b’ simply points to the same object as variable ‘a’. So what happens if we change ‘a’? To find out, let’s assign ‘a’ to a new string:

```
a = “Hello”
=> "Hello"
a.object_id
=> 47426619353340
b
=> 21
b.object_id
43
```

In the above snippet, we reassign the variable ‘a’ to a new object — the string “Hello”. This new object has an entirely new and unique object_id. However, the variable ‘b’ still points to the original object — the number 21.

## Passing Variables as Parameters

Now, the above may appear a little laboured. Why is this so significant? Well, consider the following ruby program and have a think as to what is output by the program as it stands.

```
def greeting(param)
  param + " you!"
end

say_hello = "Hello"
greeting(say_hello)

p say_hello
```

Have a gold star if you said “Hello”. How do we explain what is going on here?

Well, first we initialize the variable ‘say_hello’ by pointing it at the object “Hello”. We then call the #greeting method and pass in ‘say_hello’ as a parameter. Remember that the variable points to an object that is the string “Hello”. When the method gets invoked, a new object is created that the new variable param points to. The object is the string “Hello you!”. However, we do not return this object (or anything else) from the method. We also have not permanently changed, or mutated, the object that the initial variable ‘say_hello’ is pointing to. So, when we print ‘say_hello’ we are simply printing out the original object.

## Mutating an Object

What happens when we do mutate the object?

Well, this is where the fun starts and also why methods that mutate objects, by convention, are usually appended with an exclamation point ‘!’. Consider the same program as above, except now we use the “shovel operator” (<<) to mutate the object:

```
def greeting(param)
  param << " you!"
end

say_hello = "Hello"
greeting(say_hello)

p say_hello
```

As you will no doubt now expect, the output will be “Hello you!” because the “shovel operator” mutates the original object. Remember that ‘param’ is simply a variable that is local in scope to the method but points to the same object as the variable ‘say_hello’ which was passed into the method as an argument (or parameter). So, when we call <<, this changes the object that param (and also ‘say_hello’) is pointing to and it is permanently changed.

## Variable Reassignment Inside a Method

When we pass a parameter into a method a new local variable is created, but we don’t necessarily do anything with that new variable. Consider the code below:

```
def greeting(param)
  param += " you!"
end

say_hello = "Hello"
greeting(say_hello)

p say_hello
```

What’s happening here is the variable ‘param’ is first pointed at the object “Hello” but is then reassigned by the += operator. We are effectively doing this:

```
param = param + “ you!”
```

_This is simple variable reassignment._

As you will have guessed by now, the output from the program will be “Hello”. This is because inside the method, although we create a new variable ‘param’ and point it at “Hello” and then reassign that variable using the += method, we do not return ‘param’ at the end of the method or mutate the original object. The program simply outputs the original object — the string “Hello”, thanks to the line:

```
p say_hello
```

To see what is going on with the above code as it is executed, we can simply print out ‘param’ at each stage in the method:

```
def greeting(param)
  p param
  param += " you!"
  p param
end

say_hello = "Hello"
greeting(say_hello)

p say_hello
```

When the above program runs, the output is:

```
"Hello"
"Hello you!"
"Hello"
```

The first “Hello” comes from printing ‘param’ the first time. ‘param’ is then reassigned to “Hello you!” and we print this out. The method then exits and, effectively ‘param’ disappears and the last action our program takes is to print ‘say_hello’, which produces the last “Hello” — ‘say_hello’ was never mutated.

Thanks for reading. I hope this helps clarify how variables work as pointers to objects and how, sometimes, we need to be careful about what we do inside methods with those variables.




