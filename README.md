# otw-bandit-writeups
My solutions and notes for OverTheWire Bandit wargame

# Bandit Level 7 → Level 8

## Level Goal
The password for the next level is stored in the file data.txt next to the word millionth

## Command used
grep "millionth" data.txt


## What I learned
- grep can be used to search specific patterns within files
- Syntax: grep [Option] Pattern [FIle]
- Grep is usefull to search for certain words/patterns within a document.

# Bandit Level 8 → Level 9

## Level Goal
The password for the next level is stored in the file data.txt and is the only line of text that occurs only once

## Command used
sort data.txt | uniq -u

## What I learned
- "|" is a Pipe and can be used to connect two Commands.
- sort sorts the document so repeated lines are in a Block.
- uniq can be used to filter out duplicated lines
- Pipe was in this scenario important since uniq only compares line with the line immediately preceding it which is why sort is necessary.


# Bandit Level 9 → Level 10

## Level Goal
The password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several ‘=’ characters.

## Command used
strings data.txt | grep "=="

## What I learned
- strings can be used to filter out binary and only shows readable characters
- binary files contain non-readable characters, strings extracts only ASCII text with 4+ printable characters


# Bandit Level 10 → Level 11

## Level Goal
The password for the next level is stored in the file data.txt, which contains base64 encoded data

## Command used
base64 data.txt -d

## What I learned 
- Base64 converts binary data into ASCII string format
- primary purpose is data integrity during transmition over systems

# Level Goal
The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions

## Command used



