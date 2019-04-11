Practical Object-Oriented Design Ruby (POODR)
=====================================

NOTE: I have the 1st edition


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

