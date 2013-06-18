# A C Developer's Utility Belt

Jealousy. Developing in other languages is just so *nice*.
Automated refactoring, dynamic reloading, garbage collection, exceptions, object-orientation, lambdas…
You give up a lot to work in C.

But it doesn't have to suck.
If you find yourself writing C, don't deny yourself the benefit of all the tools and disciplines that have worked so well for other languages.
These are things that help make software projects *in any language* successful, you shouldn't deny yourself the use of them.

But first, even before you start writing code, you need to know a few things.


### What are you making?
What form does your final product take?
This is generally a choice between distributing binaries and source code, and there are consequences to each.

Distributing binaries has some advantages.
First, if you want to keep your source code secret, this is the obvious choice.
It's difficult to recover source code from a compiled binary – so much so that you can think of an optimizing compiler as a sort of weak encryption tool.

Second, you have more control over the development environment – namely, you know the exact compiler(s) that your code needs to work with.

However, if you're distributing source code, you have access to a broader developer base. Complete strangers may chip in to fix a bug or add a feature.

Being open-source has subtler advantages, too.
One of my favorites is style-guide compliance; if my code isn't just shared with my colleagues, but with anyone (including my personal heroes), I take extra care to make sure it's readable, formatted correctly, and well-tested.

This is a decision that's usually driven by business goals rather than engineering or aesthetics.


### Which C?
You may not know that there are several flavors of C.
This is understandable; the core of the language has been stable almost since the first publication of *The C Programming Language* in 1978.

The one you're probably aware of is C89, which is the name given to the ISO adoption given to the 1983 ANSI standardization of the language.
This version is very close to the original specification of the language, and is the most commonly-supported dialect; it's hard to find a compiler that *won't* compile C89.

But it's missing some things, and the standard was revised in 1999 to include some very useful features that had been released in proprietary compilers and C++.
This dialect is called C99, and it's not supported as widely.

Your choice of distribution method will help inform this decision – if you're distributing source code, you may need compatibility with compilers that can't handle C99, for instance.


## Tactical tools
There's a standard set of tools that you'll be needing every day, no matter what your environment is.
I like to call these *tactical tools*, since they may change over the life of a project, without materially changing the goal  or content of the project itself.

### Compilers
Sadly, computers don't execute C.
At some point, someone or something is going to have to translate what you've written into binary instructions that will flow like water through the doped-silicon pathways of some stranger's computer.

Compilers are not at all a solved problem.
Rather than coalescing into a field with maybe three players (as desktop and mobile operating systems have), a thousand flowers have bloomed.
There's a compiler for every situation.
Many are free; some are very expensive.

The main thing here is to know which compilers your code will need to compile under.
This largely depends on your distribution strategy (see below). If your final product is built binaries, just pick a compiler and use it to the fullest.
If your project is open-source and needs to actually *build* on strangers' computers, you'll need to test under (at least) the three dominant players: gcc, clang, and MSVC.

If you need to support more than one of these, it's worth considering using a meta-build tool such as **CMake** or **gyp**.
These allow you to define your project in general terms (these are the source files, this is what the output name should be, etc.), and the tool can generate a variety of project files, and even build the system for you using the toolchain that's available.
There's effort involved in writing the meta-project file, but it can more than pay for itself by providing access to many different environments.


### Editors
One's choice of text editor is a very personal matter, but it must be said that some editors are better at C than others.

IDEs such as **Microsoft Visual Studio**, **Eclipse CDT** or **XCode** can be very useful.
They have extensive tools for graphical debugging, performance measurement, syntax-aware code navigation, and a host of other useful features.
These tools are designed around the edit-compile-run cycle, and common tasks are made very easy.
Their main drawback is their specificity; it's difficult to get Visual Studio to work with another toolchain, or to compile a binary for Linux, for example.

Raw-text powerhouses like **Emacs** and **Vim** are much better at raw text manipulation, with features like macro recording and ultimate customizability.
But since they aren't tied to a particular toolchain, it takes a bit of monkeying to get them to support the same kind of features that IDEs do.
The other side to this coin is their sheer flexibility; these editors allow you to customize nearly everything about their behavior, and they have rich ecosystems of plugins and extensions to prove it.

**TODO: finish this paragraph? Is it useful?** 
One great differentiator between these categories is symbol-based code navigation.

### Debuggers
**TODO**

### Checkers
**TODO: static**
**TODO: valgrind**

### Collaboration Tools

**TODO**


## Strategic Tools
Writing and running code is what you're going to be doing every day, but there are several important practices that will help you succeed over a longer time span.
I think of these as *strategic* because both their costs and benefits are felt in a different time-frame than their introduction.

### Unit Testing
Once you've written all that code, you probably want to make sure it does what you think it does.
Unit testing has become a standard part of software discipline.
While it's certainly an option to not have any automated testing at all, it's not a path commonly followed.

So you probably want unit tests.
The 

**TODO**

### Continuous Integration

**TODO**

### Dependency Management

**TODO**

### Marketing

## Learning
**TODO**

### CDecl
http://cdecl.org/

## Conclusion
**TODO**