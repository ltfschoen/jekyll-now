---
layout: post
title: Code Interview (in progress!)
---

# Table of Contents
  * [Chapter 1 - Cracking the Code Interview](#chapter-1)

## Chapter 1 - Cracking the Code Interview <a id="chapter-1"></a>

## TODO

* https://www.udacity.com/course/technical-interview--ud513

### My Questions

### Structure of Answers

* General
    * SAR - Situation, Action, Response (result)

### Their Questions

* General
    * Company-specific
        * Passion for Company, Position, and Tech
            * Q1
                * Their Question:
                    * Why do you want to work for ABC company?
                * Answer Criteria:
                    * Passion for tech
                * Answer:
                    * **Experience** using ABC software S1
                    * **Like/Fan of** how ABC's S1 does __
                    * Say interested in upcoming projects and
                    their clients where you look forward to
                    helping achieve deadlines
                    * Say interested in fitting into their team
                    and if they play __?
                    * Assure them you are:
                        * Competent Unsupervised at
                        Problem Solving and in Teams
                        (without constant hand-holding)
                        * Dependable, Reliable team member
                        * Productive
                        * Positively influence team productivity
                        * Smart worker who will make their lives
                        easier (who can handle unpredictable
                        pressure, such as even in interview
                        and on real projects)
                        * Asset who takes initiative
                        (not burden or distraction)
                    * Example:
                        * Recently **specific usage** of S1-1 to
                        learn __ (i.e. game programming).
                        Its APIs are excellent
                * My Questions
                    * Genuine Questions (actually want to know answer)
                        * Qty Hours coding per day?
                        * Qty Meetings per week?
                        * Ratio of Dev, Testers, Product Mgrs?
                        * Project Planning approach?
                        * Ask about their current tech stack
                        and if they are changing it. Familiarise with
                        alternatives and praise all options
                        * Ask about their different departments
                        you'd be getting to know, IT, Business Analysts,
                        Design, Product Management, Sales, Customers,
                        Marketing
                        * Ask about what opportunities to write
                        documentation (i.e. Agile Project Plan,
                        Requirements Definition)
                        * Ask about career opportunities, and
                        company's growth plans
                    * Insightful Questions (just to demonstrate
                    deep knowledge of tech)
                        * Ask how improving S1 further?
                        * Ask their thoughts on your **recommendation** to
                        improving it by ___
                    * Passion Questions (demonstrate passion for tech)
                        * Scalability interested. What opportunities
                        to learnt about it
                        * I'm not familiar with tech X, but sounds like
                        interesting solution. Could you tell me how it
                        works

    * Applicant-specific
        * Tell about yourself
        * Tell about projects
    * Behavioural Preparation Grid
        * Most Challenging (i.e. tech, interaction)
            * 1. **Dynamic JS Plugin + pure functions Jasmine testing**
            At Oomph, built new Action Hotspots feature for
            Adobe InDesign Plugin in JavaScript and ExtendScript
            using bridge design pattern
            to dynamically generate DOM
            and exported pure functions from modules to set up Jasmine
            tests and debugged in Chromium
        * Most Learned
            * 1. **SPA in RoR** At Constant, built Angular.js SPA
            within existing RoR app
            * 2. **Pre-Live CI check Dev/Prod CMS**
            At Bauer, importance of pre-live validation in
            CI pipeline, which may point to different CMS from
            development, and prevent publishing changes that
            may prevent entire homepage from loading and present error
            page if production CMS not setup, instead of speaking to
            client to confirm acceptable to load page partially
            without the missing data
        * Most Interesting
            * 1. **Search Autocomplete Google Maps API Filter**
            At Arace, built UI event search form filter
            with geolocation autocomplete that syncronised Google Maps
            UI map markers with form fields that queried the Active
            Network API of sporting events using JavaScript, jQuery,
            and Ruby on Rails
        * Hardest Bug
            * 1. **Multi-User/Multi-Phase AWS S3 Images and Video**
            At Oomph, fixing AWS S3 images and videos
            integration in Ruby on Rails app for different
            users authentication levels and publication phase
            in workflow
        * Most Enjoyed
            * 1. **Different Tests** - Understanding and integrating unit tests,
            integration tests, e2e acceptance tests, and
        * Hardest Conflict (i.e. influencing people)
            * 1. **Helpers vs Maintainability** Refactoring duplicate code in existing middleware into
            separate helper functions rejected with justification
            being more files to maintain
        * Weakness
            * 1. **Attention to detail. Deadline vs Quality**
            traditionally slower to execute as not always
            had someone else checking my work,
            but with code reviewers, unit, integration, e2e
            acceptance tests, and browserstack, now have
            more confidence to accept faster delivery
            focusing on passing critical tests workflow
            for peace of mind
            to achieve deadlines in fast paced enviro

* Technical - TODO
    * Design
        * Scalable Systems (large scale) (i.e. Google, Amazon, Yahoo)
            * System Design (Yahoo always does)
                * Examples
                    * Design a large distributed cache
            * Memory Limits
        * Object-Oriented Design (i.e. Google, Amazon)
        * Databases (i.e. Yahoo)
        * Architecture (i.e. Yahoo)
    * Coding
        * Bit Manipulation (i.e. Google)
        * Algorithms
            * Approach
                * Ask Interviewer "good questions" to close-out ambiguities
                and to simplify problem
                    * **Approach: Misc**
                        * **Example** Design an algorithm to sort a list TODO
                            * Ask:
                                * What assume specific input Data Types?
                                i.e. array, linked list
                                * What assume the Data type to specifically hold?
                                i.e. numbers (int, floats), chars, strings
                                * Where does the data come from?
                                i.e. confidential, public
                                * What does the data represent?
                                i.e. ids, ages
                                * What is the range of data provided?
                                (i.e. ages 25-35)
                                * How should the output be sorted/ordered?
                                * How large is the data set (i.e. qty of people)
                    * i.e. Performance
                        * **Example** Given laggy response loading images, how debug?
                            * Identify the complexity (running time/space of algorithms)
                            such as loops with many iterations. Then depending on the
                            application:
                            If small qty of iterations choose readability even though
                             may be more expensive operation (i.e.
                            string `concat`, instead of array of strings then `join`)
                            If large qty of iterations then optimise (i.e. `join` instead
                            of `concat`
                    * **Approach: Examplify**
                        * **Example** Given a time, calculate angle
                        b/w hr and min hands
                            * Step 1: Sample input of time
                            * Step 2: Draw sketch
                            ...
                    * **Approach: Pattern Matching**
                        * **Example** Given a rotated sorted array
                        (it can be rotated circularly) [3,4,5,6,1,2].
                            * How find specific smallest element in array at any point
                            (i.e. binary search)?
                                * Observation: Array is sorted and rotated
                                * Observation: Array sorted in increasing order,
                                then resets and increases again.
                                * Observation: Minimum element is "reset" point
                                * **Answer**
                                    * **Binary Search**
                                        * Iteratively compare two elements
                                        (i.e. first and middle element)
                                        and if LEFT < RIGHT then range does not
                                        contain reset point
                                        * [Link to my answer to copy/paste in JSBin](https://gist.github.com/ltfschoen/209c2a50781a639850b651bb927ee2df)

                * Identify Pattern of given "real-world" Problem
                    * Examplify
                        * Think of example inputs
                        * Draw sketch
                    * Pattern Matching
                        * Think of problem the algorithm is similar to
                        and modify solution by building algorithm for problem
                * Consider Space and Time complexities, including
                whether can improve Time Efficiency
                by reducing Space Efficiency for a problem
                * Consider Risks (i.e. design integration issues:
                    * i.e. creating modified binary search tree design
                    may impact time for insert, find, delete
                * Notify Mitigation Measures (i.e. trade-offs)
                based on issues identified
                * Identify Algorithm that solves the Problem Pattern
                * Identify any problems with the algorithm
                * Write Algorithm code in pseudo-code on paper/white-board
                (start top-left of board and do not rush)
                first (ensure tell interviewer its pseudo-code to outline
                thoughts first before following up with real code)
                    * Use Data Structures and Object-Oriented Design generously
                    (i.e. create class to represent resource)
                * Write code
                * Test code and carefully
                    * Consider Edge Cases:
                        * Extreme cases: 9, negative, null, max
                         (lots of data, no data)
                        * User error
                        * Normal case
                * Identify any Bugs. Deep thinking on how to fix
                carefully (not random fixes)
                * Optimise for Edge Case scenarios
            * Examples
                * Merge Sort implementation
    * Misc
        * Polymorphism
        * Heaps
        * Virtual Machines
        * Data Structures
        * Discrete Maths
        * Compromises
            * Deadline vs Code Quality
                * Meeting deadline trumps code and algorithm purity
    * Cheat sheet https://gist.github.com/TSiege/cbb0507082bb18ff7e4b
    * Links
        * http://interactivepython.org/courselib/static/pythonds/SortSearch/TheBinarySearch.html
        * https://medium.com/@nandodrw/merge-sort-javascript-and-ecmascript-2015-es6-30a30671da35#.c2gpq35qu

### Algorithms MUST-KNOW

* Data Structures
    * Linked Lists
        * Space & Time Complexity
        * Implementation Example
    * Binary Trees
        * Space & Time Complexity
        * Implementation Example
    * Tries
        * Space & Time Complexity
        * Implementation Example
    * Stacks
        * Space & Time Complexity
        * Implementation Example
    * Queues
        * Space & Time Complexity
        * Implementation Example
    * Vectors / ArrayLists
        * Space & Time Complexity
        * Implementation Example
    * Hash Tables
        * Space & Time Complexity
        * Implementation Example

* Algorithms
    * Breadth-First Search
        * Space & Time Complexity
        * Implementation Example
    * Depth-First Search
        * Space & Time Complexity
        * Implementation Example
    * Binary Search
        * Space & Time Complexity
        * Implementation Example
    * Merge Sort
        * Space & Time Complexity
        * Implementation Example
    * Quick Sort
        * Space & Time Complexity
        * Implementation Example
    * Tree Insert, Find
        * Space & Time Complexity
        * Implementation Example

* References
    * Visualisations and code examples:
        * http://codepen.io/pjcbm9/pen/MaKEYd
        * http://codepen.io/ltfschoen/pen/evLNMb
        * https://codepen.io/mbielanczuk/pen/votiD


* Concepts
    * Bit Manipulation
        * Space & Time Complexity
        * Implementation Example
    * Singleton Design Pattern
        * Space & Time Complexity
        * Implementation Example
    * Factory Design Pattern
        * Space & Time Complexity
        * Implementation Example
    * Memory (Stack vs Heap)
        * Space & Time Complexity
        * Implementation Example
    * Recursion
        * Space & Time Complexity
        * Implementation Example
    * Big-O Time
        * Space & Time Complexity
        * Implementation Example
    * Polymorphism and Virtual Tables (Vtable)
        * **Example** Flexibility in inheritance by calling
        derived class function
        through base class object (Polymorphism) with pointers
        in an interface (VTable).
        Cross references achieved
        by declaring pointer
        in base class structure holding derived class object,
        and declare pointer in derived
        class structure holding base class object
* Languages
    * Lexer (Lexing Tokens), Parser, Abstract Syntax Tree (AST)
        * **Dfn** Parser transforms code by inspecting its characters
        into an AST, which is a structured in-memory representation
        of the "abstract" semantics (what should happen when run code)
        of the program (without
        caring about exact characters). Lexer (aka Tokenizer) operates on char
        input stream (each Token containing properties: Type and Value)
        and returns stream object with same interface
            * References http://lisperator.net/pltut/parser/
        * Implementation Example
            * Reverse Polish Notation https://en.wikipedia.org/wiki/Reverse_Polish_notation
            * Use recursion and parsing
    * Virtual Table (vtable)
        * **Dfn** Table of pointers to functions


### Assessment Criteria

* Analytical Ability
* Coding
* Experience
* Knowledge of Company
    * Familiarisation with Company Systems
    * Familiarisation with Team Systems
* Communication
    * STAR speech pattern structure
    * Avoid rambling, be concise, clarify if they would like
    you to continue explaining your thoughts and ideas,
    or if they are satisfied with the answer
* Ability to make Interviewer an Enthusiastic Endorser of you
* Do you like talking with them?
* Perceived to be smart and professional

### Avoid Red Flag Perceptions

* Arrogance
    * Avoid by being specific with just the facts (i.e. I implemented X,
    the most challenging component because of Y)
* Don't blabber
    * i.e. I did X, would you like me to go into more details?
* Poor coding
* Do not ask about Flexible working hours
* Do not mention how soon can take vacation time

### Unique Interview Processes in Different Companies

* Phone
    * Write code and read aloud over phone (i.e. Amazon)
    * Broad questions exploring tech areas familiar with (i.e. Amazon)
* Face-to-face
    * Coding on a white-board (i.e. Microsoft)
        * Write/communicate all thoughts clearly
    * Multiple Interviewers (one raises bar of question difficulty)
    * Multiple Interviews

### Unique Focus of Interview Questions in Different Companies

### Following Up

* One week after Interviews follow up with Hiring Manager

### CV

* Assume recruiter will only glance at CV for 20 seconds
* Avoid polluting/diluting CV with hobbies
* Limit content to most impressive and relevant items
* Only show relevant jobs history, not full history
* Highlight accomplishments
(i.e. increased/reduced x by y% by implementing z, leading to p)
showing what you did, how you did it, and what measurable results
* List side-projects (2-4 most significant), incl.
language/tech, individual or team project, if "Course Project"
or "Independent Project", or open source
(shows passion, initiative, work ethic)
* Highlight developer-specific high tech software experience, and
language experience:
    * i.e. Languages: JavaScript (expert), Ruby on Rails (proficient),
    Python (proficient)
    * i.e. Technical Software: Unix (expert), IntelliJ (proficient)
* Personal Information: Include work authorisation / visa status,
do not include nationality

### Resources

* CLR's Algorithms textbook


up to page 23 of book