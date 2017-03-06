---
layout: post
title: Udacity Design of Computer Programs (in progress!)
---

# Table of Contents
  * [Chapter 1 - Design of Computer Programs](#chapter-1)
  
## Chapter 1 - Design of Computer Programs<a id="chapter-1"></a>

https://classroom.udacity.com/courses/cs212/lessons/48688918/concepts/486964890923

### Program Development Steps

* Requirement to build a program is given
* Understand problem
    * Example - Poker
        * Want Poker Program that returns highest ranking hand given a list of hands
* Specify specification
    * Hands List
        * Returns Hand belonging to a Player (i.e. 2 Pair)
        * Hand consists of Cards
            * Card has Rank (i.e. 9) and Suit (i.e. Clubs)
    * Hand Quality
        * Maps Hand to Hand Type
        * Hand Type comprises:
            * n-kind (i.e. 3 of a Rank or Suit)
            * straights (i.e. 5 of a Rank)
            * flush (i.e. 5 of a Suit)
    * Data: Hand, Card, Rank, Suit
    * Operations: Get Hand, Hand Quality
* Design solution using code style
    * Functional Style (Advanced) vs Sequential Style (Basic) https://github.com/ltfschoen/PythonTest/practice/square_sums.py
    * Design Representation
        * Hands

```
['JS', 'JD', ...]               # GOOD
[(11, 'S'), (11, 'D'), ...]     # GOOD
set(['JS', 'JD', ...]           # BAD - Not good if using multiple decks of cards as cannot have duplicates
"JS, JD, 2S ..."                # BAD - Unnecessary operations to manipulate
```

*
    * Create Prototype with Pseudocode 
    * Lexigraphic Ordering for Ranking with Tuples
    
```
# rank of hand, higher quantity of same, lower quantity of same
# i.e. KH KD KS KC 2D > 4H 4D 4S 4C 1C
(7, 13, 2) > (7, 4, 1)
```

*
    * Flush Hand example
        * Break the Ties - List of all cards in hard (highest first)
        * Tie breaker - If another player has same cards in different suit

```
# i.e. 10D, 8D, 7D, 5D, 3D
(5, [10, 8, 7, 5, 3])
```

*
    * Straight Hand example

```
# rank of hand, highest card of straight sequence
# i.e. JD, 10D, 9H, 8C, 7D
(4, 11)
```

*
    * Three of a Kind example
        * Break the Ties - List of all cards in hand
```
# rank of hand, rank of three of kind, list of cards to break ties
# i.e. 7D, 7H, 7C, 5D, 2H
(3, 7, [7, 7, 7, 5, 2])
```

*
    * Two of a Kind example
        * Break the Ties - List of all cards in hand
        
```
# rank of hand, higher two of kind, lower two of kind, 
# list of cards ordered highest to lowest to break any tied hands
# i.e. JD, JH, 3D, 3H, KD
(2, 11, 3, [13, 11, 11, 3, 3])
```
