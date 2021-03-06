# Object Inheritance

## Learning Goals
- Describe objects in terms of _is-a_ relationships
- Use _inheritance_ to indicate a hierarchy of ownership or lineage between classes
- Explore overriding method definitions in _subclasses_
- Use `super` to invoke the behavior of a _base class_ in a _subclass_

## Video Lesson

<iframe src="https://adaacademy.hosted.panopto.com/Panopto/Pages/Embed.aspx?id=55d8bacd-3f1d-4494-925d-ac3f015a5e0c&autoplay=false&offerviewer=true&showtitle=true&showbrand=false&start=0&interactivity=all" height="405" width="720" style="border: 1px solid #464646;" allowfullscreen allow="autoplay"></iframe>

## Object Inheritance: _is-a_
_Inheritance_ is used to model the behaviors of one class after another. We would want to do this when two objects have a clear relationship. Often the parent class, or _base class_, is more general while the child, or _subclass_, is more specific.

For example, we could have a `Property` class that describes the general behaviors of properties which could be used in a real estate listing site like Redfin or Zillow. This is a _base class_ or _superclass_. We could create `Apartment` and `Condo` classes that model more specific behaviors and additional information. An `Apartment` is very different than a `Condo` or a `House`, but they are all properties which can be listed. It could make sense then, to declare them _subclasses_ of `Property`.

A way to describe this is by using the _is-a_ phrase: an `Apartment` _is-a_ `Property`.  In Ruby, we use the `<` symbol to denote that the class on the left of the `<` symbol is inheriting behaviors from the class on the right side of the `<` symbol:

```ruby
class Property
  attr_reader :id, :street, :city, :state, :zip

  def initialize(id, street, city, state, zip)
    @id = id
    @street = street
    @city = city
    @state = state
    @zip = zip
  end

  def mailing_address
    return "#{street}\n#{city}, #{state} #{zip}"
  end
end

# Apartment is-a Property
# Apartment inherits from Property
class Apartment < Property
end
```

The code above creates an is-a relationship between `Apartment` and `Property`.  The relationship diagramed below gives any instance of `Apartment` properties like `id` and the `mailing_address` method.

<img src="images/inheritance-basic.png" alt="Inheritance Diagram" height="500"/>

## Inheriting Methods
So what do we get when we inherit? We get __methods__ and instance variables from the parent class. The  `Property` class has an instance method called `mailing_address` which returns the mailing address of the property. Since all properties have a `mailing_address` method, our _subclasses_ will _inherit_ this method. So now all `Apartment` instances have a `mailing_address` method.

```ruby
generic_property = Property.new(12345, '123 St Charles Place', 'New York', 'NY', 12080)
generic_property.mailing_address #=> "123 St Charles Place"
                                  #  "New York, NY 12080"

fun_place = Apartment.new(56789, '111 Boardwalk', 'New York', 'NY', 12070)
fun_place.mailing_address #=> "111 Boardwalk"
                          #   "New York, NY 12070"
```

### **Exercise**
Create a `Shape` class with a `color` attribute and a `describe` method. `describe` should say in a sentence what color the shape is. 
Create a `Circle` class as a child.
Verify that both `Shape` and `Circle` have a functional `color` attribute and `describe` method (by using pry/irb).

## Super & Initialize

Notice the Apartment class is inheriting the `mailing_address` method from Property, and it's also inheriting the constructor and all of the reader methods.

If I wanted to emulate this behavior while explicitly writing a constructor in the Apartment class, I can update Apartment class like so:

```ruby
class Property
  attr_reader :id, :street, :city, :state, :zip

  def initialize(id, street, city, state, zip)
    @id = id
    @street = street
    @city = city
    @state = state
    @zip = zip
  end

  def mailing_address
    return "#{street}\n#{city}, #{state} #{zip}"
  end
end

class Apartment < Property

  def initialize(id, street, unit, city, state, zip)
    super(id, street, city, state, zip)
  end
end
```
This is functionally doing the same thing as not defining a constructor in `Apartment` but helps us see more about how inheritance works in ruby.

In the above example notice the `super` keyword in the 1st line of the `Apartment` class's `initialize` method.

`super` is a term used to refer to the parent class's version of the same method, so the line inside of the Apartment class's constructor is calling the Property class's constructor. `Property`'s `initialize` method is called and `@id`, `@address` etc are pass as the parameters.

There's something funky going on here though. The `super` method call is not actually initializing an instance of a `Property`. It is executing this code on behalf of the `Apartment` that's being initialized. It's in practicality more like Ruby is copying and pasting the code from `Property` into `Apartment`.

![inheritance initialize](images/inheritance-initialize.png)

## Adding attributes to a subclass

We have seen how inheritance allows a subclass to "inherit" the methods from it's superclass.  We can also update the `initialize` method in the subclass and use it to give our subclass additional attributes.

An apartment mailing address is different than that of a generic `Property` in that it must have a unit number.  So we can give `Apartment` objects a unit number attribute.

```ruby
class Property
  attr_reader :id, :street, :city, :state, :zip

  def initialize(id, street, city, state, zip)
    @id = id
    @street = street
    @city = city
    @state = state
    @zip = zip
  end

  def mailing_address
    return "#{street}\n#{city}, #{state} #{zip}"
  end
end

class Apartment < Property
  attr_reader :unit

  def initialize(id, street, unit, city, state, zip)
    super(id, street, city, state, zip)
    @unit = unit
  end
end
```

Note: Now that we are adding a new attribute to the `Apartment` class, we are required to call super if we want the parent constructor to execute (which is all the time).

`super` **must** be the first line in a subclass' `initialize` method.  If `super` is called without an argument, it will use the parameters from the subclass' `initialize` method.

### **Exercise**
Update the `Circle` class to have a `radius` attribute.
Verify that `radius` can now be assigned and read on an instance of `Circle` (by using pry/irb)

## Overriding Methods

What happens when our classes have different functionality for the same idea? We can _override_ methods from _base classes_ in their _subclasses_.  We want the mailing addresses of `Apartment` objects to include the unit number so we can override the `mailing_address` method.

```ruby
class Apartment < Property
  attr_reader :unit

  def initialize(id, street, unit, city, state, zip)
    super(id, street, city, state, zip)
    @unit = unit
  end

  def mailing_address
    return "Unit: #{unit}\n#{street}\n#{city}, #{state} #{zip}"
  end
end

generic_property = Property.new(12345, '123 St Charles Place', 'New York', 'NY', 12080)
generic_property.mailing_address #=> "12345: 123 St Charles Place"

fun_place = Apartment.new(56789, '111 Boardwalk', '212B', 'New York', 'NY', 12070)
fun_place.mailing_address #=> "Unit: 212B"
                  # "111 Boardwalk"
                  # "New York, NY 12070"
```

In this case, we override the functionality that the _base class_ (`Property`) provides with more specific functionality in the _subclass_ (`Apartment`).

### Using `super` to invoke behavior in _base classes_.

It's also possible to use the behavior defined in the _base class_ alongside specialized behavior in the _child class_. By using the keyword `super`, we can invoke the code in the _base class_ at any point in the _overriding_ method __and__ still be capable of using the overridden code.  In other words the overriding method can do the same action as the parent class **and** more.  This can help us DRY up our code significantly.

```ruby
class Apartment < Property
  ...
  def mailing_address
    return "Unit: #{unit}\n#{super}"
  end
end

generic_property = Property.new(12345, "123 St Charles Place", "New York", 'NY', 12080)
generic_property.mailing_address #=> "123 St Charles Place"
                         # "New York, NY 12080"

fun_place = Apartment.new(56789, '111 Boardwalk', '212B', 'New York', 'NY', 12070)
fun_place.mailing_address #=> "Unit: 212B"
                  # "111 Boardwalk"
                  # "New York, NY 12070"
```

In this case, we utilize the behavior that the _base class_ (`Property`) provides but augment it with behavior specific to the _subclass_ (`Apartment`).

![super in method calls](images/inheritance-super-method.png)

### **Exercise** 
Override the `describe` method in `Circle` to describe `radius` as well as `color`. (You should be able to do this using `super` to keep your code DRY.)

## A Second Child
Because we have created a parent `Property` class that defines attributes and behavior shared by all properties, we can create the `House` class by writing very little code. 

```ruby
class House < Property
  attr_reader :roof_type

  def initialize(id, street, city, state, zip, roof_type)
    super(id, street, city, state, zip)
    @roof_type = roof_type
  end
end

fun_place = House.new(56789, '100 Park Place', 'New York', 'NY', 12077, 'Shake')
fun_place.mailing_address #=> "100 Park Place"
                  # "New York, NY 12077"
fun_place.roof_type #=> "Shake"
```

### **Exercise**
Add `Rectangle` as a child of `Shape`

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->
<!-- Replace everything in square brackets [] and remove brackets  -->

### !challenge

* type: short-answer
* id: 47cba0e0-857c-4597-889d-edecf70a55d1
* title: Instead of radius, what attribute(s) should Rectangle have?
<!-- * points: [1] (optional, the number of points for scoring as a checkpoint) -->
* topics: oop, classes

##### !question

Instead of radius, what attribute(s) should Rectangle have?

##### !end-question

##### !placeholder

Rectangle attributes

##### !end-placeholder

##### !answer

/.+/

##### !end-answer

<!-- other optional sections -->
<!-- !hint - !end-hint (markdown, users can see after a failed attempt) -->
<!-- !rubric - !end-rubric (markdown, instructors can see while scoring a checkpoint) -->
##### !explanation

length and width

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->  

## Summary

Inheritance allows classes to gain or "inherit" the methods and attributes from another class. It's important to name that the example in this lesson would not be a good example if `Apartment` were the only child class of `Property` we made. It is the fact that many child classes can inherit the same behavior from one parent that makes inheritance so powerful.

Inheritance is a key concept in Object Oriented Programming.  While it can be powerful, but it is also easy to misuse. Inheritance is good to use where many classes are clearly an extension or variation of the parent class, so each class gaining methods and attributes **"is-a"** specialized version of the parent class.  Be very selective in your use of inheritance as composition is more often the most effective strategy.

## Resources
- [Understanding Inheritance in Ruby](http://culttt.com/2015/06/24/understanding-inheritance-in-ruby/)
- [Ruby the Hard Way inheritance vs composition](https://learnrubythehardway.org/book/ex44.html)
- [Youtube Video Intro to class inheritance](https://www.youtube.com/watch?v=hrCKyNFSaos)
