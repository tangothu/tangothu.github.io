---
layout: post
title: Python CS116 Winter - Complete the function longest_subpalindrome
categories:
- assignment
tags:
- Python
- Data Structure
- accumulative recursion
---

### Question 3

- A palindrome is a string that is the same forwards and backwards. For example, "racecar", "anna", and
"aibohphobia" (the fear of long palindromes) are all palindromes, but "abc" and "sata" are not.
We say that a string is a "subpalindrome of s" if it is both a substring of s and a palindrome.
Complete the function longest_subpalindrome, that consumes a lowercase alphabetical strings of 
length at most 15, and produces the longest subpalindrome of s. If there are multiple
subpalindromes that are the same size, then return the one that comes first alphabetically.
For example,
• longest_subpalindrome("abaca") => "aba"
• longest_subpalindrome("anna") => "anna"
• longest_subpalindrome("sata") => "ata"
• longest_subpalindrome("abc") => "a"
Hints:
• You may use examples from class as helper functions.
• Use generative recursion to break up the problem into a number of smaller sub-problems.
• Your solution does not need to be efficient!
