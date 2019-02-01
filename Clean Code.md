Uncle Bob's Clean Code Training (via O'Reilly)
===============================

Ottinger's Rules for Naming
    The name of a variable should correspond to the size of the scope it's in
        So we can use `i` for indexing an iteration
        Uncle Bob says we can use short parameter names in short methods
            I disagree, because parameter names are used by developers outside of the method's scope
        Global variable names should be very long and specific
    The name of a function should correspond INVERSELY to the size of the scope it's in
        
        Private methods should be long
            Even longer if they're only called by private methods
        Unit tests should have them smallest possible scope - it should be a full sentence
Avoid situations where code will break if a misspelling is corrected
    Use `theClass` or `aClass` or `itsClass` instead of `klass`
The whole point of an interface is to not be 
    So starting interface names with `I` is dumb
Consider factory methods where constructors aren't clear
    `Complex.from_real(12.34)` vs `new Complex(12.34)`
Uncle Bob says the Patterns book is one of the most important
    He says to use the pattern name in class names
        I disagree STRONGLY
            Unless it's to distinguish from similar classes
Use names from the domain of the users, customers, and product owners


Uncle Bob's Step-Down rule: Every line in a function should be at the same level of abstraction
    Should be 1 level below the name of the function
    The called functions should be 1 level below the current function
Uncle Bob's first rule of functions: They should be small
Uncle Bob's second rule of functions: They should be smaller than that!
You can tell if a function is doing more than 1 thing if you can extract a function from it
Extract until you drop
    Surprisingly, you won't end up with too many little functions
        Because you have to come up with good names for what you extract
            The more you extract, the longer the names get
        Because you'll find other classes to extract
Order your methods so that the higher-level functions are higher in the file than the lower-level

Avoid switch statements
    It creates a fan-out problem
        Each case has an outgoing dependency
            This breaks independent deployability
    Especially type cases (where each case handles a different type)
        Use polymorphism instead
            Perhaps inheritance, perhaps an interface

Boolean arguments indicate that a function is doing more than 1 thing

Prefer exceptions to returning error codes

Comments should normally be a last resort, except:
    * Documentation of external API
    * Explanation of intent
    * Warnings about consequences
    * TODOs
    * Amplifying the importance of something

The newspaper metaphor
    * Banner at top
    * Synopsis paragraph
    * Details increase as you read further down
    * Scissor rule
    * Or as I call it, the pyramid (from learning how to write term papers)
Keep concepts that are related close to each other (vertically)

