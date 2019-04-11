Structure and Interpretation of Computer Programs (SICP)
=================================================

Lecture 5A - Assignment, state, and side effects
----------

* Corresponds to Sections 3.1 and 3.2 in the text

* Only add a feature to a language to provide another way to decompose problems
    * Decompose them into a different set of pieces, that you could not before
* Uses a `!` to indicate things "like" assignment
* Pure FP (without `set!`) can do all the computations at once
    * There's now time involved - time before and time after a variable is set
* Using `set!` in a function makes it not a mathematical function
* Using `set!` in a function means the substitution model no longer works
    * Have to use the environment model instead
* interesting: `let` basically works by defining and executing a function
    `(let ((v1 e1) (v2 e2)) e3) ===== ((define (v1 v2) e3) e1 e2)`
* Environment model
    * Bound variable
        * A variable _v_ is *bound* in an expression _e_
          if the *meaning* of _e_ does NOT change
          when we change all occurrences of _v_ in _e_ to a different variable name
        * Like in math, `f(x) = x + 1`, `x` is bound to the `f`
            * Because `f(y) = y + 1` would *mean* the same thing
        * It doesn't matter what names you use for function parameters
    * Free variable
        * A variable _v_ is *free* in an expression _e_
          if the *meaning* of _e_ DOES change
          when we change all occurrences of _v_ in _e_ to a different variable name
        * Note that "variable" here also includes names of functions, methods, constants, etc.
    * Scope
        * The scope of a variable is where it has a bound value
    * Formal parameter list
        * AKA bound variable list (of a lambda)
        * The named variables within a lambda expression, that are bound to the lambda expression
    * Environment
        * A way of doing substitutions virtually
        * Represents a place where you store substitutions that you haven't done yet
        * Can be a table or a function (like a map)
        * Built from a sequence of frames
    * Frame
        * Piece of an environment
            * Chained to other frames (in parent-child relation) to create the full environment
                * A value in the child frame shadows the value of the same name in the parent frame
        * Corresponds to a scope
    * Procedure
        * Combination of code and an environment
        * Used to capture values of free variables (i.e. those from enclosing scopes)
    * Rule 1 (evaluating procedures):
        * When calling a procedure,
          a frame is constructed, binding the formal parameters to the actual arguments;
          then evaluate the body of the procedure, in the context of the new environment;
          that new environment is the procedure's enclosing environment, with the new frame as its child
    * Rule 2 (construction of procedures):
        * When a lambda expression is evaluated,
          a procedure is created,
          containing the code and the environment containing the lambda
        * I.e., when a lambda is evaluated, look in enclosing scopes to find the values of the free variables
* Is there some definition of object here that he's using that pre-dated OOP?
    * He did mention OOP
* Don't use assignment when you don't have to

Lecture 3B
----------

* "In order to make a system that's robust, it has to be insensitive to small changes."
    * "A small change in the problem should lead to only a small change in the solution."
