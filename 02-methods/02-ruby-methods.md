# Ruby Methods

<iframe src="https://adaacademy.hosted.panopto.com/Panopto/Pages/Embed.aspx?pid=8af308aa-aaff-40ed-aef9-ac210061e3e0&autoplay=false&offerviewer=true&showtitle=true&showbrand=false&start=0&interactivity=all" height="405" width="720" style="border: 1px solid #464646;" allowfullscreen allow="autoplay"></iframe>

## Learning Goals

  - Learn why we need methods!
  - Learn to write our own methods!
  - Understand a _method signature_!
  - Figure out the difference between _parameters_ and _arguments_.
  - Identify method signatures for methods we have used.
  - See the difference between using `return` and not using it.

## Methods

We have seen and used methods before, but we haven't created our own method definitions yet. Let's change that.

### Creating methods: Why?

Methods allow us to assign a _name_ and _structure_ to sections of code. We create methods when we identify these sections of code that we may want to reuse and execute independently from other sections of code.  This lets us write sections of code and _reuse_ those segments over and over again, reducing the amount of code we need to write.  Methods are a key tool in the mantra "DRY" (Do Not Repeat Yourself) that many developers adopt.


### Creating methods: Signatures and Blocks

To create a method, you must first understand it's _method signature_. A _method signature_ is comprised of 2 parts: the __name__ of the method, and the __arguments__ it is expecting. Once you have defined those two things, you can create a `def` (method definition) statement.

#### Example 1

```ruby
def say_hello # <= that's the method signature
  puts "hello world!" # <= that's the body
end
```

In this example, `say_hello` is the method __name__. This method has no __parameters__ (more on that shortly), so the _method signature_ is just `say_hello`. The purpose of this method is to print out a string to the user. As we learned earlier, we want to _call_ or _invoke_ a method in order to execute its body:

```ruby
say_hello
# => hello world!
```

### Method Arguments (and parameters)

Okay, let's get this out of the way. Most folks use _argument_ and _parameter_ interchangeably. That's fine and it almost never matters. However, as best as I understand, here's the difference:

__In Ruby, _parameters_ are variables used to refer to pieces of data provided as input; _arguments_ **are** those pieces of data.__ The _parameters_ are defined in the _method signature_, and the _arguments_ are the specific values given to the method when it is _invoked_. Like I said, most of the time, this distinction isn't important.

Once we have created a method, we may want to use an _parameter_ to provide additional data or context to our method. Think back to our `Math.sqrt(9)` example. The `9` in the `()` is a _argument_. Looking at this, I can hazard an educated guess that the _method signature_ of `sqrt` looks something like `def sqrt(number)` (name and one parameter).

To add a parameter to the `say_hello` method, we need to change the __method signature__:

#### Example 2

```ruby
def say_hello(name) # <= method signature with a name and one parameter
  puts "welcome #{name}! hello world!" # <= the block
end

say_hello("Ada") # <= invoking the method with one argument
# => welcome Ada! hello world!
```

In this updated method, `name` is a variable with the value for the argument passed in. When we called the method, the `name` variable was substitued out for the value of string `"Ada"` since that was the value we passed in.

#### Example 3

```ruby
def add(num_one, num_two)
  return num_one + num_two
end
```

In this example, `add` is the method __name__. This method has two __parameters__ so the _method signature_ is `add(num_one, num_two)`. The purpose of this method is to __return__ the result of adding two variables together. We want to _call_ or _invoke_ this method to execute its body:
```ruby
add(1, 2)
# => 3
```

In this example method _call_, the variable `num_one` is substituted with value `1` and the variable `num_two` is substitued with value `2`.

#### Example 4

__Question: When do we need to use the `return` keyword?__

Let's see what the difference is between using `puts` and using `return.`

First, let's assign a variable to the _result_ of the method call.
```ruby
def add_with_return(num_one, num_two)
  return num_one + num_two
end

result = add_with_return(2, 2)
puts result
# => 4
```

Next, let's change the method and assign the variable in the same way.
```ruby
def add_with_puts(num_one, num_two)
  puts num_one + num_two
end

result = add_with_puts(2, 2)
puts result
# => nil
```

**Methods can often be referred to as _messages_. When we send a message, we expect a reply.**


## Summary

Writing Ruby methods allows us to assign a name and structure to segments of code.  Those segments can then be called whenever those instructions need to be executed.  When calling methods we can pass data in via _arguments_.  In the method signature the variables which are assigned the value of any arguments are called _parameters_.  The combination of a method name and parameters defines a method's _signature_.  A method's signature distinguishes it from other methods available.  So it can be important for them to be unique and meaningful.

**All** Ruby methods return a value, we can explicitly indicate the returned value with the `return` keyword which will cause the method to execute and result in the given value, or lacking a `return` statement, a Ruby method returns the value of the last line of code executed in the method, called an _implicit return_.  At Ada we generally ask students to include an explicit `return` statement when writing methods as it's a bit more readable.

