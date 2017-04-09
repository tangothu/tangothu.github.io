---
layout: post
title: Python CSC108 - Exploring the Twitterverse
categories:
- assignment
tags:
- Python
- Data Structure
- dict
---

## Python assignment - CSC108 : Exploring the Twitterverse

Assignment 3: Exploring the Twitterverse

Purpose

The purpose of this assignment is to give you practise working with files, building and using dictionaries, designing functions, and writing unit tests.

Introduction: The Twitterverse

Twitter is a social networking website where users can post very short messages know as "tweets". Each Twitter user can choose to "follow" other users, which means that they see those users' tweets. A Twitter user sees the tweets of users they are "following", and their tweets are seen by their "followers" (the users who follow them).

All the "follow" connections define a network among Twitter users, and it's quite interesting to look for patterns in the connections. Tools like Twiangulate let you explore questions like "what connections do my two friends have in common?". In this assignment, you'll write a program that lets you ask questions (or "queries") about a Twitter dataset.

Any tool for exploring the Twitterverse must get its data from Twitter itself. Twitter provides an API to allow programmers to write programs that interact with Twitter and extract data from it. In general, an API is a module that defines functions for accessing underlying data and performing other tasks without having to know how that data is actually stored and retrieved.

To make this assignment more manageable for you, we will assume that the information we need has already been extracted from Twitter and stored in a file.

How to tackle this assignment

This is your first experience designing a program of this size. We are providing detailed advice to help you break the task down into manageable pieces.

Make sure your twitterverse_functions.py module runs without error before submitting. If you have syntax errors in your module, comment them out before submitting, or we will not be able to test your functions!

The Twitter Data File

A Twitter data file contains a series of one or more user profiles, one after the other. Each user profile has the following elements, in this order:
A line containing a non-blank, non-empty username. You may assume that usernames are unique, that is, a single username will not occur more than once in the file, and that usernames do not contain any whitespace.
A line for the user's actual name. If they did not provide a name, this line will be blank.
A line for the user's location, or a blank line if they did not provide one.
A line for the URL of a website, or a blank line if they did not provide one.
Zero or more lines for the user's bio, then a line with nothing but the keyword ENDBIO on it. This marks the end of the bio, and is not considered part of it. (You may assume that no bio has the string ENDBIO within it.) If the user did not provide a bio, the ENDBIO line will come immediately after the website line, with no blank line in between.
Zero or more lines each containing the username of someone that this user is following, then a line with the keyword END on it. (You may assume that no one has END as their username.) A user cannot be on his or her own following list. You may assume that every user on a following list has a user profile in the Twitter data file.
Notice that the keywords act as separators in this file. All of their letters are capitalized, and the keywords contain no punctuation.
Examples

Here is a sample user profile that might occur among many in a file:
tomCruise
Tom Cruise
Los Angeles, CA
http://www.tomcruise.com
Official TomCruise.com crew tweets. We love you guys!
Visit us at Facebook!
ENDBIO
katieH
NicoleKidman
END
The file data.txt is a smallish example of a complete Twitter data file (and was made by hand) and the file rdata.txt is a much larger example (and is made from real data extracted from Twitter). These should help you confirm your understanding of the file format and will also be useful in testing your program.
Cycles in the data

Although a user cannot be on their own following or followers lists, there can be "loops" (we call them "cycles") such as this: user A can be following B who is following A. This is the shortest possible cycle. Of course, cycles can be longer.

The Query File

Note that the word "query" just means "question". In computer science, we use it to mean a request for information. For this assignment, a query will be provided in a file. Below we will review the high level parts of the query, look at an example, and then describe the format of the query file.
Overview

A query has three components: a search specification, a filter specification, and a presentation specification.

The search specification describes how to generate a list of Twitter usernames, starting with an initial username (a list of length one) and then finding their followers or people they are following, then people that are those people's followers or who they are following, and so on. When processing the search specification, don't try to do anything to avoid cycles. For instance, if the search specification says to find the people who user A is following, and from there the people they are following, you could find yourself back at user A. Don't try to avoid that.

After processing the search specification, we have a list of Twitter usernames. Its length could be zero. For example, if the initial username is 'dianelynnhorton' and the search specification contains a single 'followers' keyword, then the length of the list will be zero if 'dianelynnhorton' has no followers.

The filter specification describes how to filter the list of usernames produced by the search specification. The filtering can be based on

whether or not they are following a particular user,
whether or not a particular user is their follower,
whether their name contains a particular string (case-insensitive), or
whether their location contains a particular string (case-insensitive).
After processing the filter specification, we have a possibly reduced list of usernames.
Once the search results have been found and filtered, the presentation specification describes how the output should be presented. It specifies on what basis the results should be sorted, and whether the results should be presented in a short or long format.

Example query

Here is an example query:

SEARCH
tomCruise
following
following
following
FILTER
following KatieH
location-includes USA
PRESENT
sort-by popularity
format long

The search specification in this particular query has four steps.
Start with a list containing the username to start the search from; ie. ['tomCruise']. Let's call that list L1.
The search keyword 'following' says to replace each username p in L1 with the usernames of the users who p is following. This yields a new list, L2.
For the next 'following' keyword, we start with L2 and repeat the same operation as in the previous step, yielding another list, L3.
For the final 'following' keyword, we start with L3 and repeat that operation one last time, yielding list L4.
Notice that each step yields a list of zero or more usernames that is the input to the next step. There should be no duplicates in the final results list. Duplicates should be removed after each step.
The Twitter data file diagram_data.txt contains the follower/following relationships as represented by this diagram. For those relationships, the search specification above would yield this list of usernames: ['i', 'j', 'h', 'k', 'tomCruise']. Make sure that you can see how the four lists, ending with this final one, are generated. Notice that the final list contains the users you can get to in three "steps" of the "following" relationship, starting from 'tomCruise'.

The final list generated by the search specification becomes the input to the filter specification. For our current example, the filter specification says that the list should be filtered in this way: a user should be kept only if they are following 'KatieH' and has a location that includes the string 'USA'. The presentation specification says to present the results in long format and to order the users according to their popularity.

Now let's look in detail at the exact format and meaning of a query.

Overall format of a query

The format of the query data file is as described below, in this order:

The keyword SEARCH alone on a line.
The search specification.
The keyword FILTER alone on a line.
The filter specification.
The keyword PRESENT alone on a line.
The presentation specification.
Notice that the keywords above all act as separators in this file and are in all capital letters. There are other keywords (such as following which can be part of a search specification) that are not separators; they are not capitalized.
Format and meaning of the search specification

The format of a search specification is as shown below, in this order:

A single username
Zero or more lines, in any order, each containing one of:
the keyword followers
the keyword following
Each step in the search specification involves modifying the current list of users by going through each user in the current list and finding their followers or who they are following. Importantly, at each step, we want to create the new results list by replacing each user from the current list with the users that they are following or who are their followers.

The possible search operations for a given user are:

followers operation: a list of all usernames for users who are following the given user (ie. that have the given username in the value list paired with the key 'following' in their data in the Twitterverse dictionary).
following operation: a list of all usernames for users who the given user is following (ie. the value list paired with the key 'following' in the data associated with the given user in the Twitterverse dictionary).
Note that, if the list of usernames is ['A'], and we perform a 'followers' or 'following' operation, 'A' will not be in the resulting list, since users cannot follow themselves. If 'A' is one of multiple usernames in the current list, then 'A' can appear in the results list, but only if one of the other usernames produces 'A' from the search operation.

The final list of results should not contain any duplicates. It is most efficient to remove duplicates after each operation is performed.

Format and meaning of the filter specification

The format of a filter specification is:

Zero or more lines, in any order, each containing one of the following:
the keyword name-includes, a space, and a string to match
the keyword location-includes, a space, and a string to match
the keyword follower, a space, and a string which is a valid username
the keyword following, a space, and a string which is a valid username
Each of the lines has exactly one blank within it (the space separating the keyword from the next string), and each keyword will appear at most once.
The filters are "additive" in that each filter step should be applied to the list that resulted from the previous filter step. That is, you start with the list from the search specification, filter it based on the first filter step to get a new list L1, filter L1 in the second filter step to get L2, and so on.

The 'following' and 'follower' filters will keep users in the list if they are following a particular user, or if a particular user is their follower. These filters are defined as:

The 'following' filter keeps only users who have the provided username in their 'following' list.
The 'follower' filter keeps only users who appear in the 'following' list for the provided username.
The filters that have 'includes' as part of the keyword will do a simple substring search on the string representing the relevant data for the user in the Twitterverse dictionary. This means that if the given string occurs anywhere in a user's name (for a 'name-includes filter') or a user's location (for a 'location-includes' filter), then that user is to be kept in the list. The substring search should be case-insensitive. For example, if the filter specifies users whose locations include the string "USA", then users with location "USA", "Modesto, California, USA", "kansas, usa" or "Musala, Bulgaria" would be kept, and users with location "United States of America" would be excluded. This is far from perfect, but don't try to improve on it.

Format and meaning of the presentation specification

The output from your program must include every user whose username is still in the list after the search and filter specifications have been processed.

The presentation specification describes how the output should be presented. It consists of these two lines, in order:

The keyword 'sort-by', then a space, then one of these keywords: 'username', 'name', 'popularity'
Either 'format short' or 'format long'.
The users must be listed in the order indicated by the 'sort-by' portion of the presentation specification. Output that is sorted by username or name should be in alphabetical order, so that strings starting with 'a' are at the beginning of the output. Although usernames are unique, names are not. If any users have the same name, sort them by username. When sorting users by popularity, the most popular users should come first. A user's popularity is defined to be the number of followers they have. (Not the number of people they are following!) If any users are tied for popularity, sort them by username.

If the presentation specification is for short format output, the output should consist of the usernames as a Python list of strings. If the presentation specification is for long format output, the output for each user should consist of a series of lines about the user, as shown in the example below. The long format output must include a line of ten dashes between each pair of users, as well as a line of dashes at the very top and very bottom of the output.

If there are no users to be presented (i.e., the 2nd parameter in get_present_string represents an empty list), then the long format output should just include two lines of dashes.

For the long format, the series of lines describing a single user should be formatted as shown below:

tomCruise
name: Tom Cruise
location: Los Angeles, CA
website: http://www.tomcruise.com
bio:
Official TomCruise.com crew tweets. We love you guys!
Visit us at Facebook!
following: ['katieH', 'NicoleKidman']
Notice that:
The name, location, and website text is reproduced exactly as it was in the Twitter data file, with the addition of the labels at the start of the line.
The bio begins on the line after the line containing 'bio:', unlike the other components that appear on the same line as their "tag".
The bio itself is also reproduced exactly as it was in the Twitter data file, including upper and lower case, newline characters, etc.
The "following" list is produced simply by converting the list of username strings to a string.
