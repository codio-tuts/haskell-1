---
title: Getting Started with Haskell
files: []
editable: true
layout: 2-panels-tree

---
As you read the early chapters of this book, keep in mind that we will sometimes introduce ideas in restricted, simplified form. Haskell is a deep language, and presenting every aspect of a given subject all at once is likely to prove overwhelming. As we build a solid foundation in Haskell, we will expand upon these initial explanations.

##Your Haskell environment
Haskell is a language with many implementations, of which two are in wide use. Hugs is an interpreter that is primarily used for teaching. For real applications, the Glasgow Haskell Compiler (GHC) is much more popular. Compared to Hugs, GHC is more suited to “real work”: it compiles to native code, supports parallel execution, and provides useful performance analysis and debugging tools. For these reasons, GHC is the Haskell implementation that we will be using throughout this book. 6 comments

GHC has three main components

- ghc is an optimizing compiler that generates fast native code. No comments
- ghci is an interactive interpreter and debugger.
- runghc is a program for running Haskell programs as scripts, without needing to compile them first.

>How we refer to the components of GHC
When we discuss the GHC system as a whole, we will refer to it as GHC. If we are talking about a specific command, we will mention ghc, ghci, or runghc by name. 4 comments


---
title: Getting started with ghci
files: []
editable: true
layout: ""

---
The interactive interpreter for GHC is a program named **ghci**. It lets us enter and evaluate Haskell expressions, explore modules, and debug our code. If you are familiar with Python or Ruby, **ghci** is somewhat similar to python and irb, the interactive Python and Ruby interpreters.

>#####The ghci command has a narrow focus
We typically cannot copy some code out of a Haskell source file and paste it into ghci. This does not have a significant effect on debugging pieces of code, but it can initially be surprising if you are used to, say, the interactive Python interpreter.

##Running ghci
To run **ghci** open up a terminal window from the **Tools->Terminal** menu, or simply [click here](open_terminal).

When we run **ghci** from the terminal, it displays a startup banner, followed by a `Prelude>` prompt. Here, we're showing version 6.8.3 on a Linux box.

```bash
GHCi, version 6.8.3: http://www.haskell.org/ghc/  :? for help
Loading package base ... linking ... done.
Prelude>
```
The word Prelude in the prompt indicates that Prelude, a standard library of useful functions, is loaded and ready to use. When we load other modules or source files, they will show up in the prompt, too.

>#####Getting help
If you enter :? at the ghci prompt, it will print a long help message.

The Prelude module is sometimes referred to as “the standard prelude”, because its contents are defined by the Haskell 98 standard. Usually, it's simply shortened to “the prelude”. 

###About the ghci prompt
The prompt displayed by ghci changes frequently depending on what modules we have loaded. It can often grow long enough to leave little visual room on a single line for our input.

For brevity and consistency, we have replaced ghci's default prompts throughout this book with the prompt string ghci>.

If you want to do this youself, use ghci's `:set prompt` directive, as follows. 

```bash
Prelude> :set prompt "ghci> "
ghci>
```

The prelude is always implicitly available; we don't need to take any actions to use the types, values, or functions it defines. To use definitions from other modules, we must load them into ghci, using the :module command.

```bash
ghci> :module + Data.Ratio
```

We can now use the functionality of the Data.Ratio module, which lets us work with rational numbers (fractions).
---
title: Using ghci as a calculator
files: []
editable: true
layout: ""

---
In addition to providing a convenient interface for testing code fragments, ghci can function as a readily accessible desktop calculator. We can easily express any calculator operation in ghci and, as an added bonus, we can add more complex operations as we become more familiar with Haskell. Even using the interpreter in this simple way can help us to become more comfortable with how Haskell works. 

##Simple arithmetic
We can immediately start entering expressions, to see what ghci will do with them. Basic arithmetic works similarly to languages like C and Python: we write expressions in infix form, where an operator appears between its operands.

```bash
ghci> 2 + 2
4
ghci> 31337 * 101
3165037
ghci> 7.0 / 2.0
3.5
```

The infix style of writing an expression is just a convenience: we can also write an expression in prefix form, where the operator precedes its arguments. To do this, we must enclose the operator in parentheses.

```bash
ghci> 2 + 2
4
ghci> (+) 2 2
4
```

As the expressions above imply, Haskell has a notion of integers and floating point numbers. Integers can be arbitrarily large. Here, (^) provides integer exponentiation.

```haskell
ghci> 313 ^ 15
27112218957718876716220410905036741257
```


---
title: Writing negative numbers
files: []
editable: true
layout: ""

---
Haskell presents us with one peculiarity in how we must write numbers: it's often necessary to enclose a negative number in parentheses. This affects us as soon as we move beyond the simplest expressions.

We'll start by writing a negative number.

```haskell
ghci> -3
-3
```

The `-` above is a unary operator. In other words, we didn't write the single number “-3”; we wrote the number “3”, and applied the operator - to it. The - operator is Haskell's only unary operator, and we cannot mix it with infix operators.

```haskell
ghci> 2 + -3
<interactive>:1:0:
    precedence parsing error
        cannot mix `(+)' [infixl 6] and prefix `-' [infixl 6] in the same infix expression
        ```

If we want to use the unary minus near an infix operator, we must wrap the expression it applies to in parentheses.

```haskell
ghci> 2 + (-3)
-1
ghci> 3 + (-(13 * 37))
-478
```

This avoids a parsing ambiguity. When we apply a function in Haskell, we write the name of the function, followed by its argument, for example f 3. If we did not need to wrap a negative number in parentheses, we would have two profoundly different ways to read f-3: it could be either “apply the function f to the number -3”, or “subtract the number 3 from the variable f”.

Most of the time, we can omit white space (“blank” characters such as space and tab) from expressions, and Haskell will parse them as we intended. But not always. Here is an expression that works:

```haskell
ghci> 2*3
6
```

And here is one that seems similar to the problematic negative number example above, but results in a different error message.

```haskell
ghci> 2*-3
<interactive>:1:1: Not in scope: `*-'
```

Here, the Haskell implementation is reading *- as a single operator. Haskell lets us define new operators (a subject that we will return to later), but we haven't defined *-. Once again, a few parentheses get us and ghci looking at the expression in the same way.

```
ghci> 2*(-3)
-6
```

Compared to other languages, this unusual treatment of negative numbers might seem annoying, but it represents a reasoned trade-off. Haskell lets us define new operators at any time. This is not some kind of esoteric language feature; we will see quite a few user-defined operators in the chapters ahead. The language designers chose to accept a slightly cumbersome syntax for negative numbers in exchange for this expressive power.