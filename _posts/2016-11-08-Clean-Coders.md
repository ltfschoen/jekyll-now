---
layout: post
title: Clean Coders (in progress!)
---

# Table of Contents
  * [Chapter 1 - Intro](#chapter-1)
  * [Chapter 2 - A-Frame Game](#chapter-2)

## Chapter 1 - Intro<a id="chapter-1"></a>

### 1960's vs 20xx computers

* Hardware changes over past ~50 years
- 10% of the cost
- 1% of the volume
- 8,000 times faster processing
- 2,000,000 times memory
- solid state storage (no rotating disk)
- 22 orders of magnitude better hardware overall
- sequence, selection and iteration still done mostly the same to prove correctness and ease reasoning/thinking about the program
- material manipulated virtually the same
- endless list of languages
- are there any new 'types' of languages to come?

### Types of programming languages
* group by syntax classes
* syntax and semantics components (i.e. block structures, object oriented)
* function name and args (prefix notation i.e. + x y)
* operations (infix notation i.e. x + y)
* operands upfront (postfix notation i.e. x y +)

### Semantic class of languages
* unstructured (Fortran, Cobal) - use of GO TO
* structured (C, Pascal) - discipline/constraint imposed upon direct transfer of control (i.e. upon use of GO TO) 
* object-oriented (OO) - (C with polymorphism) - OO paradigm is: 
- discipline imposed upon use of pointers to functions and upon indirect transfer of control
in exchange for benefit of ability to structure modules so source code dependencies oppose the flow of control
(normally in structured languages flow of control points in same direction as source code dependencies) 
- dependency management using polymorphism
- good OO programs use polymorphism so key dependencies are inverted to point against flow of control 
* pure functional - paradigm is (as described in book Structure and Interpretation of Computer Programs):
- discipline imposed upon assignment, as should not assign to variables, or change variables after initialised
- important due to multi-core problem of limited clock speed so now having to increase processor speed by adding more cores (failure of Moore's Law relating to processor speed)
- important due to seeking robust computing paradigm less subject to concurrent update problems 
to overcome concurrency issues due to use of multiple threads and processors
* modular programming - paradigm is:
- discipline of break-down monolithic files/programs into smaller modules
* aspect-oriented programming - not a paradigm:
- did not remove or constrain anything
- **added** new capabilities and features by increasing our expressiveness
BUT it failed
* graphical languages -
i.e. time is linear (algorithms occur as sequence of events) and executes in 1 Dimensional / Textual in program,
so natural to model as sequence of statements and we think of code being linear (a text),
even inside threads things happen one step at a time
Note: Electronics is graphical (lots happening at same time) 

Note: paradigms of each semantic class of language **add** nothing, they just impose constraints and **subtract**
 
### Domain specific languages (DSLs)
- proliferation of domains means more DSLs
- strive to use DSLs instead of GPLs
- how many GPLs should we have to get real work done??
- have we seen all possible GPLs? what benefits would new ones offer?
- last syntax revolution? (Forth, Prolog)

### Hybrid languages
- i.e. belongs to multiple semantic classes
- i.e. unconstrained languages keeping power that paradigms that constrain 

### Industries select notation of preference
i.e. C has become a defacto standard in Ruby, C#, etc
- Unify notation to reap benefits (as done in sciences)
- Overcomes corporate control of languages (organisations owning languages)
- Programmers gain control of mode of expression
- Choose characteristics i.e. garbage collected to overcome memory leaks and
not manual memory control (i.e. not Objective-C), polymorphism, 
compiles to virtual language not tied to hardware,
homoiconic and allow modify while running, syntax simplicity (so can focus on semantic issues),
polymorphism, dependency inversion, control how modules are dependent on each other,
modules where source code dependencies oppose transfer of control (polymorphism),
O-O (constraints on indirect transfer of control such as cannot use pointers to functions 
by instead dependency management solely using polymorphism),
language must run in virtual machine not just on certain platform hardware,
must be able to maintain access all virtual frameworks (i.e. ruby, python), 
structured paradigm without GO TO (i.e. not C or C++), fast (not like ruby), symbolic and abstract (not coupled to notion of hardware),
probably textual as in nature and 1 dimensional and proceeds in direction in time (not graphical),
homoiconic (i.e. able to manipulate code while running and construct more running programs) aka monkey patching / metaprogramming (code is the data and data is the code),
opinionated (i.e. type safety)

## TODO
* [Racket](http://racket-lang.org/)
* [Elixir](http://elixir-lang.org/)
* [Microsoft Visual Programming Language](https://msdn.microsoft.com/en-us/library/bb964572.aspx)

## Links
* [Clean Coders](https://cleancoders.com)
* [Clean Coder Blog](http://blog.cleancoder.com/)

## Chapter 2 - A-Frame Game<a id="chapter-2"></a>

### Objectives
* Game Goal: Take turns playing pieces on 3D lattice board intersects, first to connect 5 wins 
* Host Software:
- Location - On a device or in cloud, or on both devices
- Engine - knows board state (whose turn), 
- UI - listen for inputs from device 
* Client Software: Other device

* Step 1: 
- Create small usable version, check it works, user testing to validate target market receptive to ideas
- Simplify and build basic MVP on one device initially in 2D, later scale it to 3D, ensure decouple UI from communication system
- Minimum requirements: have board, place pieces, detect win/lose/tie, detect invalid if consecutive turns are both black or both white
- Tools: React, Test

* Step 2:
- Write first test (i.e. can create board)