# A C Developer's Utility Belt

**Jealousy** is an emotion commonly associated with C.
Developing in other languages is just so *nice*.
Automated refactoring, dynamic reloading, garbage collection, exceptions, object-orientation, lambdas…
You give up a lot to work in C.

But it doesn't have to suck.
If you find yourself writing C, don't deny yourself the benefit of all the tools and disciplines that have worked so well for other languages.
These are things that help make software projects *in any language* successful, and you shouldn't deny yourself the use of them.

But first, even before you start writing code, you need to know a few things.


### What are you making?
What form does your final product take?
Is your software installed on strangers' computers, or does it run on a server under your control?
There are many ways to ask this question, but it boils down to whether what you're shipping is source code.

Distributing pre-built binaries has some advantages.
If you want to keep your source code secret, this is the obvious choice.
It's difficult to recover source code from a compiled binary – so much so that you can think of an optimizing compiler as a sort of weak encryption tool.

You also have more control over the development environment – you know the exact compiler(s) that your code needs to work with.
Having complete control over your toolchain lets you avoid cluttering your code up with compiler-compatibility workarounds, as well as not having to worry about testing with esoteric compilers.

However, if you're giving away your source code, you have access to a broader developer base.
Opening your code away has some obvious advantages, like having complete strangers chip in to fix a bug or add a feature.
But there are other, subtler advantages, like the goodwill generated by participating in the community surrounding your project, and the ability to recruit and interview more effectively.

One of my favorite subtle side-effects is style-guide compliance.
If my code isn't shared only with my colleagues, but with anyone (including my personal heroes), I take extra care to make sure it's readable, formatted correctly, and well-tested.

The choice of distribution strategy is usually informed by business goals and product style.
If you're producing installed software, it's common to keep the source code secret to avoid giving away your entire revenue stream.
But if you are building a service accessed over the Internet, having your code public may not matter when it comes to making money.


### Which C?
You may not know that there are several flavors of C.
This is understandable; the core of the language has been stable almost since the first publication of *The C Programming Language* in 1978.

The one you're probably aware of is C89, which is the name given to the ISO adoption given to the 1983 ANSI standardization of the language.
This version is very close to the original specification of the language, and is the most commonly-supported dialect; it's hard to find a compiler that *won't* compile C89.

But it's missing some things, and the standard was revised in 1999 to include some very useful features that had been released in proprietary compilers and C++.
This dialect is called C99, and it's not supported as widely.

Your choice of distribution method will help inform this decision – if you're distributing source code, you may need compatibility with compilers that can't handle C99, but if you have complete control over your toolchain, it's probably desirable to choose a set that supports the new features.
Happy developers are productive developers.


## Tactical tools
There's a standard set of tools that you'll be needing every day, no matter what your environment is.
I like to call these *tactical tools*, since they may change over the life of a project, without materially changing the goal  or content of the project itself.
These decisions are important, because there is always a cost to switching tools, but the cost isn't so high as to make the decision irreversible.

### Compilers
Sadly, computers don't execute C.
At some point, someone or something is going to have to translate what you've written into binary instructions that will flow like water through the doped-silicon pathways of some stranger's computer.

Compilers are not at all a solved problem.
Rather than coalescing into a field with maybe three players (as desktop and mobile operating systems have), a thousand flowers have bloomed.
Many compilers are free, some are very expensive, and it seems like a new one comes out every couple of years.

The main thing here is to know which compilers your code will need to work with.
This largely depends on your distribution strategy – if your final product is built binaries, just pick a compiler and use it to the fullest.
If your project is open-source and needs to *build* on strangers' computers, you'll need to test under the compilers your users are most likely to have.

If you need to support more than one compiler or development environment, it's worth considering using a meta-build tool such as **CMake** or **gyp**.
These allow you to define your project in general terms (these are the source files, this is what the output name should be, etc.), and the tool can generate a variety of project files, and even build the system for you using the toolchain that's available.
There's effort involved in writing and maintaining the meta-project file, but it can more than pay for itself by providing access to many different environments.
This approach also lets you avoid maintaining multiple tool-specific project files, and allows people working on the code to choose the environment that works best for them.


### Editors
One's choice of text editor is a very personal matter, but it must be said that some editors are better at C than others.

IDEs such as **Microsoft Visual Studio**, **Eclipse CDT** or **XCode** can be very useful.
They have extensive tools for graphical debugging, performance measurement, syntax-aware code navigation, and a host of other useful features.
These tools are designed around the edit-compile-run cycle, and common tasks are made very easy.
Their main drawback is their specificity; it's difficult to get Visual Studio to work with another toolchain, or to compile a binary for Linux, for example.

Raw-text powerhouses like **Emacs** and **Vim** are much better at raw text manipulation, with features like macro recording and ultimate customizability.
But since they aren't tied to a particular toolchain, it takes a bit of monkeying to get them to support the same kind of features that IDEs do.
The other side to this coin is sheer flexibility; these editors allow you to customize nearly everything about their behavior, and they have rich ecosystems of plugins and extensions to prove it.

**TODO: finish this paragraph? Is it useful?** 
One great differentiator between these categories is symbol-based code navigation.

### Debuggers
If you're using an IDE, you already have a debugger.
But if you're using a raw-text editor, you get to choose one.

Many compiler toolchains come with a debugger (gcc comes with **gdb**, clang with **lldb**, etc.).
There is a steep learning curve with CLI-based debugging tools, but climbing it can be well worth your time.
As with Vim or Emacs, you gain intimate understanding and muscle-memory speed when you choose a tool designed for use with only a keyboard.

Another approach is to use a meta-project generator to create a project file for an IDE.
Most of your day-to-day editing can be done with your favorite editor, and when you need the specialized tools included with Xcode, for example, you simply generate a project file.
Using a meta-build tool, even if you're only planning on building with one compiler, can make it possible to use the best tool for each job, rather than being stuck with the tools you chose on day one.


### Checkers
Automated checking tools can catch many serious bugs before they even go into source control.
These come in two flavors: static and dynamic.

Static tools work by analyzing the source code without actually running it.
You're actually running one of these already – compilers are pretty good at finding type errors, and not terribly bad at detecting certain other kinds of error.
There are others that are free; **scan-build** is included with clang (and is also available from inside Xcode), and Eclipse CDT includes **CODAN**, for example.
Commercial tools also exist, such as **PC-Lint** and **PVS-Studio**.
The main drawback of static analysis tools is their pedantry.
Often these tools will find problems that aren't worth the effort to fix, or false positives.
It takes some effort to tune the tool to your team's particular coding style and requirements.

Dynamic tools work by analyzing data about your program while it's running.
Usually these are focused on watching memory allocations, CPU usage, or I/O, and they are sometimes called profilers.
Xcode ships with several excellent tools, and certain versions of Visual Studio do as well.
**Valgrind** is the king of the hill in the open source world, and includes an extensive set of debugging and profiling tools.
These tools can save you many hours of debugging when (not if) memory corruption errors happen.


### Collaboration Tools
Working with others is essential to having a successful project.
For a simple project with a small team, you can probably get by with face-to-face communication and a pad of paper, but more complex situations demand more sophisticated tools.

One thing you'll need, without exception, is a version control system (VCS).
Choosing one seems fairly arbitrary at first, but the capabilities of your tool will shape the kinds of actions you can take later on.
I highly recommend using one of the newest generation, so-called "distributed" version control tools, like **Git** or **Mercurial**.
Your team may grumble if they're not used to them, but within a few months they probably won't be able to imagine going back to a centralized system.
But if you must, please don't use RCS or CVS; there's no reason to choose them over something like **Subversion**.

You'll also need to be able to communicate with your team, both asynchronously and in real-time.
Asynchronous tools include things like bug trackers, discussion boards, wikis, and even the API documentation extracted from code comments.
Think of these as places where decisions have URLs, so you can refer to them later.
Each kind of tool has its place, but your choice of which to use will be informed by the way your team works together.

You'll also need synchronous communication for rapid feedback.
If your team consists of one person, or work in the same physical space, this problem is already solved; humans are great at sharing ideas in an environment such as this.
But if neither of these facts is 100% true, you'll need some software to back you up.
This is generally called *chat software*, and includes things like **IRC**, **Campfire**, and **HipChat**.

If all of this is overwhelming, well, good.
Human communication is a complex problem, not easily solved by software, and there have been many failed attempts at being the *one true way*.
However, if you want a curated set of collaboration tools, a bundle of decisions that have already been made for you and wrapped up with a bow, you can find that too.
Toolsets like **GitHub** have been crafted with a team workflow in mind, and will work very well if you stick to their constraints.
Other packages, such as products from Atlassian or Microsoft, can be tailored to your specific needs.


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