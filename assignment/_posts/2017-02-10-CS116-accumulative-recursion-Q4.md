---
layout: post
title: Python CS116 Winter - Use accumulative recursion to complete the function find_group
categories:
- assignment
tags:
- Python
- Data Structure
- accumulative recursion
---


### Question 4

- Consider the following scenario, in which there are n different “people”, each represented by a distinct
natural number labelled from 0 to n–1.
Each person has been asked to "choose" a single person that they would prefer to have as a group
member for some project. These choices are recorded in a list L of length n > 1, where the ith
element of L contains the choice for person i.

For example, if L = [2, 0, 1, 4, 0], then (see picture):
• person 0 has chosen person 2
• person 1 has chosen person 0
• person 2 has chosen person 1
• person 3 has chosen person 4
• person 4 has chosen person 0
and note that people are not allowed to choose themselves.
Now we want to automate the task of finding the smallest "group" that contains a given person.
However, we will only consider a group to be "good" if it has at least two people, and everyone in the
group chose someone else from the same group.
For instance, in the above example, the group of people [0, 1, 2] is good because all of these
people chose others from the same group. However, the group [3, 4] is not good because person
4 chose person 0, and person 0 is not in the group. In order for the group to be good, each person's
choice must also be (recursively) added to the same group, and therefore [0, 1, 2, 3, 4] would
be the smallest good group that includes person 3.
Your task is to use any kind of recursion to complete the function find_group, that consumes some
list L of choices, and an initial person i — according to all of the above conditions — and then
produces a sorted list of people that represents a good group containing person i. If there is a good
group that contains person i, then your function should produce that group in sorted order. If multiple
groups are good, then produce the shortest one.
For example,
• find_group([2, 0, 1, 4, 0], 0) => [0, 1, 2]
• find_group([2, 0, 1, 4, 0], 4) => [0, 1, 2, 4]
• find_group([2, 0, 1, 4, 0], 3) => [0, 1, 2, 3, 4]


The first example finds the group [0, 1, 2], which is good and contains person 0. The second
example finds the group [0, 1, 2, 4], since including person 4 means we must include person 0,
which in turns includes person 2, and then finally person 1. The third example is [0, 1, 2, 3, 4],
since everyone can be put into one group containing all 5 people, which is the smallest good group that
contains person 3.
Hints:
• Work through some examples first, by hand, in order to see the pattern. Try drawing pictures to
get some intuition. There is an elegant solution!

