# otw-bandit-writeups
My solutions and notes for OverTheWire Bandit wargame

# Bandit Level 7 → Level 8

## Level Goal
The password for the next level is stored in the file data.txt next to the word millionth

## Command used
grep "millionth" data.txt


## What I learned
-grep can be used to search specific patterns within files
-Syntax: grep [Option] Pattern [FIle]
-Grep is usefull to search for certain words/patterns within a document.

# Bandit Level 8 → Level 9

## The password for the next level is stored in the file data.txt and is the only line of text that occurs only once

## Comman used
sort data.txt | uniq -u

## What I leaned
-"|" is a Pipe and can be used to connect two Commands.
-sort sorts the document so repeated lines are in a Block.
-uniq can be used tu filter out duplicated lines
-Pipe was in this scenario important scince uniq only compares line with the line immediatly preceding it which is why sort is neccesary.
