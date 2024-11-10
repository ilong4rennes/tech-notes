## Ruby Object Model

- Everything (numbers, strings, classes, methods) is an Object. Every object is an instance of a class.

- Classes: define the blueprint for objects. An object (or an instance) is created from a class. Classes themselves are objects, instances of the class `Class`.

- Modules: a way to group related methods, constants, and class variables that can be mixed into classes and objects. Unlike classes, you cannot create instances of modules.

- Object Class: At the top of Ruby's object hierarchy is the `Object` class. Almost every class in Ruby implicitly inherits from `Object`, making its methods available to virtually every object.

- BasicObject Class: `BasicObject` is the parent class of `Object`. It is the simplest object in Ruby, providing the minimal set of methods that every object needs. It's used when you want a lightweight class hierarchy free from the methods provided by `Object`.

- Single class inheritance: a class can inherit from one superclass

- Metaclass: Every object in Ruby has an eigenclass / metaclass. The class holds methods defined only on that object.

### Method Lookup Path

When you call a method, Ruby looks up the method in the following order:
- The object's eigenclass
- The class of the object
- Any modules included in the class of the object
- The superclass of the class, and any modules included in the superclass
- This process repeats up the inheritance chain until `BasicObject` is reached.

### Class Slides

![[Screen Shot 2024-03-12 at 03.26.27.png]]
- object `daffy` with instance variables `@name` and `@breed`
	- Instance variables are properties of individual instances of a class
- eigenclass for `daffy` with a method `talk` defined on it
	- Methods can be defined on the eigenclass

![[Screen Shot 2024-03-12 at 03.26.39.png]]
- `Duck` class with a class variable `@@count` and instance methods `talk` and `swim`
- eigenclass with a class method `self.active`
	- classes themselves have eigenclasses where class-level methods are defined

![[Screen Shot 2024-03-12 at 03.27.02.png]]
- method lookup path
	- start from instance `daffy`
	- `daffy`'s eigenclass
	- `Duck` instance methods
	- `Duck`'s eigenclass (for class methods)
	- `Object` class and its eigenclass

### Object Receives Messages

- Ruby objects receive messages through method calls. The `send` method is used to call a method by its name (symbol or string). For example, `"quack".send :upcase` sends the `:upcase` message to the string object `"quack"`, resulting in `"QUACK"`.
- The code demonstrates sending various messages (`:upcase`, `:+`, `:nil?`, `:class`) to different objects (String, Fixnum, Array, and custom objects) and receiving the expected outcomes.

### Object Responds to Messages

- The `respond_to?` method checks if an object can respond to a specific message (method call), highlighting Ruby's dynamic nature. For example, `"quack".respond_to? :length` checks if the string object can respond to the `length` method, which it can.

### Example of the Pen and the Scribe

#### Creating and Enhancing Objects

- An `Object.new` call creates a basic object, which is then enhanced by defining methods directly on it (such as `to_s` and `write`). This showcases Ruby's open classes and singleton methods, where you can add or modify methods of individual objects at runtime.
- The example demonstrates adding properties (`@ink_color`) and methods (`ink_color=`, `to_s`, `sparkly_write`, `html_write`) to the `pen` object, illustrating how objects can be dynamically modified to suit specific needs.

#### Interaction Between Objects

- A `Scribe` class is defined with an `initialize` method that takes a pen object, demonstrating dependency injection, where objects are created with their dependencies (e.g., a pen for a scribe).
- The `Scribe` class uses `attr_reader` and `attr_accessor` for creating getter and setter methods, simplifying access to instance variables.
- Interaction between the `scribe` and `pen` objects is shown through method calls, such as `write_everything`, where the `scribe` uses the `pen` to write notes.

#### Metaprogramming and Object Cloning

- Ruby's metaprogramming capabilities are illustrated through dynamic method definition (e.g., defining methods on the `pen` object and the `ezra` instance of `Scribe`).
- The clone method creates a shallow copy of an object, allowing for the creation of distinct objects with shared behavior but separate states (as shown with `pen2` being a clone of `pen`).

### Key Takeaways

- Ruby objects communicate through messages (method calls), and their behavior can be inspected or modified at runtime (`send`, `respond_to?`).
- Ruby supports dynamic modification of objects (open classes, singleton methods) and allows for sophisticated object interactions and dependencies.
- Metaprogramming in Ruby enables the dynamic definition of methods, enhancing the flexibility and expressiveness of the language.
- The example uses these concepts to create a narrative around a pen and a scribe, showcasing how Ruby's object model and its features can be applied in a creative and practical manner.

## Rollback









cannot delete amoxicillin
因为我们不能删除药物

rollback

error.add(:base)想不出加哪里的error

object option

calling method =  send messages

all classes to make objects are objects

arrays wiik ever tbe nuik.

difference be=w modules: 
class:an

write a method for  pen
def pen.write -->only pencil has this method

initializer: def initialize pen
getter and setter -- manually
attr_reader :list --就等于def list 这个list是immutable


speak hebrew is only the method of ezra
ezra is an object in scribes

you can add method to classes or to objects

the methods does not belong to daffy.
eigenclass
(ghost class that actually stores the methods)

.eat 
先去daffy eigenclass
然后duck instance
然后object
go right once then always go up to see who respond to this first. 
-method missing error已有的方法、、
run_find_by_method: 我们以为是find_name

find_by_username 并非我叫的而是active base

is_active_in_system --> module validations
line 9-10 valiations.rb

include as instance method
eg. is_active_in_sytem

How about class methods?


there are no methods in the object itself
the methods are kept in the eigenclass

daffy.swim
- first it goes to check daffy eigenclass
- then go straight up to duck
- then go to object
- then if we still cannot find it, then "method missing"

include validations in pet model
validations -- code from source code?

the pet class send the message (calling the method eg. .active)

want the methods actually in the eigenclass of the Duck

extend class methods


- class: an object, every objects stores methods in their hidden eigenclass
- eigenclass: you dont see this, automatically created these 

