Practical Object-Oriented Design Ruby (POODR)
=====================================

NOTE: I have the 1st edition


Chapter 5
---------


* It's not what an object /is/ that matters, it's what it /does/
* The design possibilities are infinite
	* Me: but design constraints are usually helpful
	* She later says to be intentional about duck typed interfaces
* Me: When you see a case statement, you're probably doing OOP wrong
* Dependencies: class names, method names, argument count and types
* Interesting that she introduced a named abstract interface (Preparer)
* Only note I took first time: pattern of passing self to collaborators
* Concrete code is easy to understand, but hard to extend
	* Abstract code is harder to understand at first, but easier to change
		* Me: a good name for the abstraction can *improve* understandability
* Polymorphism - different object types respond to the same message
* Disagree: ability to tolerate ambiguity about class/type of an object is the hallmark of a confident designer 
	* Agree more: Once you begin to treat your objects as if they are defined by their behavior rather than their class, you enter into a new realm of  expressive flexible design
* Depending on things with very stable interfaces is OK
	* Like builtin classes
	* Not as high a cost to leave those dependencies (and code looking at types) in, instead of implementing a duck type
* Wrong: can't determine nil at compile time
	* Crystal has proven this wrong


Chapter 6
---------

* Inheritance (classical)
    * Module/mixin inheritance will be covered in the following chapter
    * JavaScript has prototypical inheritance

* Inheritance is a mechanism for automatic message delegation
    * We also have SimpleDelegator to create a proxy object
    * We can, of course, manually delegate messages we receive
    * In Rails, we also have `delegate`

* I like how she starts the examples with a specific kind of bike
    * Instead of starting with a generic bike
        * Although the class is originally called `Bicycle`
    * Then shows an anti-pattern to extend to another type of bike
        * Smells:
            * If statement, checking the bike's type/style/category
            * Attributes that may or may not pertain, depending on the type of bike
                * Callers might be tempted to check the type before getting an attribute
            * Has more than 1 responsibility; contains things that might change for different reasons

* The `if` on the type of something identifies that there's something missing
    * Can be used to identify a missing duck type or a missing subtype/subclass
        * If the decision is based on the category of `self`, then it's probably a subclass trying to get found
        * If the decision is based on the class of something else, it's probably a duck type you need to find

* Use classical inheritance when:
    * There's a good amount of shared common behaviors
        * But also specialization

* Interesting that she used `super(args)` (p 115)
    * No reason not to just use `super` there
    * Also, sometimes it's not clear *when* `super` should be called from the overridden method in the subclass

* Two rules of inheritance:
    1. The objects being modeled must have a true generalized-specialized relationship
    2. Use the correct coding techniques

* Abstract class
    * Sole purpose is to be subclassed

* It's rarely a good idea to create an abstract class for a single concrete class
    * It's not necessarily clear yet what the "common" behaviors are
        * You might not ever have a need to find out (creating more subclasses)
        * You might find the *wrong* abstractions
    * If you can wait for a 3rd concrete class, that'll help even more
        * This advice is probably only appropriate for languages with duck typing (or maybe interfaces)
        * If the 3rd class will be arriving soon, it's better to put off extracting the superclass until then
            * Because it'll provide more information to find the right abstraction
                * And the cost of waiting won't be as high as having duplicated code around for a long time
            * You'll have more examples to see if each subclass needs to method or not
            * Helps identify the shared behaviors that constitute the (right) abstraction

* When splitting a class into a superclass and a subclass: (p 119-123)
    1. Move *everything* to the subclass
    2. Move abstract parts up to the superclass
        * This way you won't end up with concrete things in the superclass
        * It's easier to move things from the subclass to the superclass
            * TODO: We'll see why shortly (p 119)
            * Getting NoMethodError in a subclass is a very simple indicator
                * You need to promote the method up from the other subclass into the superclass

* Her mechanical refactoring requires 2 moves (Bike -> RoadBike -> Bike)
    * Refactoring is *supposed* to be mechanical

* I like that she always talks about the cost of (future) change (p 123, others)
    * And sets up strategies so that *when* you're wrong, the cost is cheap

* Template method pattern: (p 125-126)
    * Send messages to `self`
        * But give subclasses an opportunity to override the superclass's default implementation
            * To specialize the subclass's behavior
    * Separates parts of an algorithm that change form parts that don't change (p 160) (SPOILERS)
* Rule for using using template method pattern: (p 128)
    * Always include a default implementation in the superclass
        * Even if it's just `raise NotImplementedError`
        * Documents all the methods a subclass might want/need to override
            * Static documentation
            * Dynamic documentation (via an exception and backtrace)

* Hook methods reduce the coupling between subclass and superclass
    * Increases the tolerance for change
    * Subclass doesn't need to call `super`

* Inheritance's greatest strength is ease (low cost) of extension

* Typo I made: sibclass
    * This would be a funny/terrible abbreviation for a sibling class


Chapter 8 - Composition
---------

* Her definition of composition (the very first sentence):
	* Combining distinct parts into a complex whole ...
		* ... such that the whole becomes more than the sum of its parts.
	* Makes analogy with musical composition
* Foreshadowing: she'll make recommendations on how to choose between composition and inheritance
	* Both classical and modular inheritance
* Interesting that she names a class `Parts`
	* Plural, and meant to contain more objects
	* Wasn't sure what to think of this decision
		* Not one I've made very often, if at all
		* I think it's the wrong abstraction
			* A bicycle *is-a* collection of parts; it's doesn't *have-a* collection of parts
			* A bicycle is more than the sum of its parts
				* I don't think the collection of parts is more than the sum of its parts
	* Allows different types of bikes to have collections of different kinds of parts
* I don't like how she refactors `Bicycle` before writing `Parts`
	* Violates one of the prime refactoring rules
		* Always make sure your tests remain green during refactoring
		* Important for TCR (test && commit || refactor)
	* I suspect she does a better job of this in /99 Bottles of OOP/
* Example of subclassing `Parts` from `Array`
	* Ruby doesn't have a good way of saying "create another object of my class"
		* Or maybe it's more of an issue with the standard library
			* TODO: Look into possibly fixing this
				* Array, Hash, String
		* Even if it did, what if I wanted to combine an Array and a subclass
			* Which should we choose then?
				* Probably whichever is the receiver of the method
* I hate Ruby's `Forwardable` and `def_delegators`
	* Odd to `extend` instead of `include`
	* Order of arguments to `def_delegators` feels wrong
	* I much prefer Rails' `delegate`
	* But I like how she made it an `Enumerable`
* I hate that she created an entire `PartsFactory` module
	* The build method feels like it should be a factory method on `Parts`
		* It's intimately connected to that class
			* Despite the attempt to use dependency inversion
				* Prematurely - we don't have any other classes that could be used in their place
		* Or just a different initializer
			* She says to *only* use the factory to create the objects
				* But doesn't enforce that
				* That could be accomplished just by using the initializer
			* In Ruby, `new` is basically the "default" factory method
* Note that while `Bicycle` *has-a* `Parts`, and `Parts` *has-a* set of `Part` objects ...
	* ... `Bicycle` treats its `parts` as anything that fills the role
		* Maybe this is why she uses `parts_class` in the factory
	* ... and `Parts` treats each of its `parts` as anything that fills the role
		* So she used an OpenStruct instead of a `Part` class
* I also don't like that `RoadBike` and `MountainBike` no longer encapsulate their component parts
	* Instead, the component parts are left in configuration objects *outside* of any encapsulation
	* We could still do the config objects, but *inside* of an encapsulating class
		* Or some DSL
		* Or template method pattern
* Aggregation
	* Closely related to (or a  special kind of) composition
		* The items composing the whole have an individual life outside of the composed object
		* A *has-many* relationship
			* And a *belongs-to* relationship from the other point of view
* Costs and benefits of composition and inheritance
	* I love that she gives the costs and benefits of each option
		* Because often it's not completely cut and dry
	* Inheritance benefits:
		* Adding something small high in the inheritance chain can have a big impact
		* Easy to extend
	* Inheritance costs:
		* Might paint yourself into a corner if you chose the wrong abstraction
		* Making *changes* high up in the hierarchy has far-reaching effects
		* Building on wrong abstractions has higher costs
		* Issue of "where is this defined?"
			* Especially in deep hierarchies with lots of methods
	* Composition benefits:
		* Encourages smaller objects
			* Which in turn encourages adhering to SRP
		* More flexibility, because components only have to fit the interface
			* Eg. her choice to use an OpenStruct instead of a Part class
	* Composition costs:
		* Can create complexity
			* Might be a lot of "moving pieces"
			* Combined operation of the whole might not be apparent
		* Manual delegation
			* No way to share code that might require identical delegations
		* Probably harder to test
			* Do we mock the components?
* Don't write frameworks that require users to subclass your objects (original note)
	* They may already have their own hierarchy
		* So the won't be able to subclass your class
	* ActiveRecord breaks this rule
	* You'll note I've taken that to heart
		* My ActiveRecord-Repository Entity and Repository modules
		* My Includable-ActiveRecord gem
* Composition leads to smaller objects with clearer interfaces (original note)
* Choosing between composition and inheritance
	* Guidance from the "grand masters":
		* "Inheritance is specialization."
			* Bertrand Meyer, /Touch of Class/
		* "Inheritance is best suited to adding functionality to existing classes when you will use most of the old code and add relatively small amounts of new code."
			* Gamma et al, /Design Patterns/
		* "Use composition when the behavior is more than the sum of its parts."
			* Grady Booch, /Object-Oriented Design and Analysis/
	* Lean towards composition
		* Has far fewer built-in dependencies
	* Use inheritance for "is-a" relationships
		* Obvious specialization hierarchies
	* Use duck types for "behaves-like-a" relationships
		* Additional roles not one of the core responsibilities of an object
			* Printable, listable, persistable, ...
			* The roles are wide-spread among disparate types of objects
	* Use composition for "has-a" relationships
		* Has behaviors that are separate from and in addition to the behavior of its parts
* Keys to improving your design skills:
	* Attempt to use the various techniques
	* Learn from your mistakes
	* Refactor mercilessly


* Thought experiment: what if our language didn't have any inheritance?
	* We'd only have composition
	* Could we still do OOP?
		* Probably - we could even have inheritance still
			* We'd have an explicit "parent" object for each object
			* We'd have to manually delegate everything to that "parent"
				* Rails-style `delegate` would mitigate this a *lot*
			* Might actually be better in some ways
				* Some say explicit is better than implicit
				* Solves the multiple inheritance issues
				* Mitigates issue of "where is this defined?"
			* Downsides
				* Wouldn't guranteed that subclasses implement the patents interface
					* So Liskov Substitution Principle could be violates easily
					* But isn't that just duck typing?


