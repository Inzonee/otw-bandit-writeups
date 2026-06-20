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
- 'strings' can be used to filter out binary and only shows readable characters
- binary files contain non-readable characters, strings extracts only ASCII text with 4+ printable characters


# Bandit Level 10 → Level 11

## Level Goal
The password for the next level is stored in the file data.txt, which contains base64 encoded data

## Command used
base64 data.txt -d

## What I learned 
- Base64 converts binary data into ASCII string format
- primary purpose is data integrity during transmition over systems

# Bandit Level 12 → Level 13

## Concept
A file was compressed multiple times with different tools (gzip, bzip2, tar),
then converted to a hexdump with xxd. The goal is to reverse all of that
step by step until you reach plaintext.

## Process
1. Create a temp working directory with `mktemp -d` and copy the file there
2. Convert the hexdump back to binary: `xxd -r data.txt > data_bin`
3. Check the file type: `file data_bin`
4. Decompress accordingly, repeat until ASCII text

## Decompression commands used
- gzip: `mv file file.gz && gunzip file.gz`
- bzip2: `mv file file.bz2 && bunzip2 file.bz2`  
- tar: `tar xf file`

## What I learned
- `file` reads magic bytes to determine the real file type — 
  file extensions can lie, which matters in security contexts
- `&&` runs the second command only if the first succeeded
- `xxd -r` reverses a hexdump back to binary
- Never `cat` a binary file — it corrupts your terminal output

# Bandit Level 13 → 14

## Concept
No password this time. A private SSH key is provided instead, which can be used to authenticate as bandit14. The actual password is stored in `/etc/bandit_pass/bandit14` but is only readable by bandit14 — so the key is your only way in.

## Process

1. Display the private key on the server:
``` bash
   cat /home/bandit13/sshkey.private
```

2. Save it locally on Windows:
``` cmd
   notepad bandit14.key
```
   Paste everything including the header and footer:
```
   -----BEGIN RSA PRIVATE KEY-----
   ...
   -----END RSA PRIVATE KEY-----
```

3. Connect using the key:
``` cmd
   ssh -i C:\Users\USERNAME\bandit14.key bandit14@bandit.labs.overthewire.org -p 2220
```

## What I learned
- SSH can authenticate with a private key instead of a password — no password prompt appears at all
- The `-i` flag specifies which private key file to use
- Private keys are just text files and can be copy-pasted between terminals
- `chmod 600` was not needed on Windows, but on Linux a private key must have restricted permissions or SSH refuses to use it


