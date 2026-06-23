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
1. Create a temp working directory with `mktemp -d` and copy the file there.
2. Convert the hexdump back to binary: `xxd -r data.txt > data_bin`.
3. Check the file type: `file data_bin`.
4. Decompress accordingly, repeat until ASCII text.

## Decompression commands used
- gzip: `mv file file.gz && gunzip file.gz`
- bzip2: `mv file file.bz2 && bunzip2 file.bz2`  
- tar: `tar xf file`

## What I learned
- `file` reads magic bytes to determine the real file type — 
  file extensions can lie, which matters in security contexts.
- `&&` runs the second command only if the first succeeded.
- `xxd -r` reverses a hexdump back to binary.

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

4. Get the key:
``` cmd
  cat /etc/bandit_pass/bandit14
```

## What I learned
- SSH can authenticate with a private key instead of a password — no password prompt appears at all.
- The `-i` flag specifies which private key file to use.
- Private keys are just text files and can be copy-pasted between terminals.
- `chmod 600` was not needed on Windows, but on Linux a private key must have restricted permissions or SSH refuses to use it.

# Bandit Level 14 → Level 15

## Concept
To get the password to the next level, the password from the current level needs to be sent to port 30000 on localhost.

## Process
1. use the command netcat to connect to port 30000 on localhost.
``` cmd
nc localhost 30000
```
2. Paste the Password in and get a return message.
3. To get the passwort of current level.
``` cmd
cat /etc/bandit_pass/bandit14
```

## What I learned
- `nc` (Netcat) opens a direct TCP connection to a host and port.
- `localhost` refers to the current machine itself.
- Services can listen on specific ports and respond to input
  here port 30000 accepted the password and returned the next one.

# Bandit Level 15 → Level 16

## Concept
To get the password to the next level, the password from the current level needs to be sent to port 30001 on localhost usind SSL/TLS encription.

## Process
1. Establish an SSL/TLS connection to the target service:
``` cmd
openssl s_client -connect localhost:30001
```
2. After the TLS handshake is completed, enter the password from the current level and press Enter.

## What I learned
- How to use openssl s_client to establish SSL/TLS connections from the command line.
- The difference between plain-text communication and encrypted communication.
- That Transport Layer Security (TLS) provides encryption and helps protect data during transmission.

# Bandit Level 16 → Level 17

## Concept
The credentials for the next level can be retrieved by submitting the password of the current level to a port on localhost in the range 31000 to 32000. First find out which of these ports have a server listening on them. Then find out which of those speak SSL/TLS and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.

## Process
1. Find out what ports are listening.
``` cmd
   nmap localhost -p 31000-32000
```
2. Test each open port with openssl to find which ones speak SSL/TLS:
   openssl s_client -connect localhost:<port> -ign_eof
   Ports that echo your input back are echo servers — ignore them.
   The correct port returns credentials.
   
4. Because of KEYUPDATE you need to use -ign_eof Flag
``` cmd
   openssl s_client -connect localhost:(the port) -ign_eof
```
4. Get the private key do the steps in lv 13 and connect to next level

## What I learned
- `nmap` can scan multiple ports and give information about different ports
- Two ports where working but one was an echo server which only gave your input back and the other was a real server which gives answer
- The -ign_eof flag prevents openssl s_client from automatically closing the connection when it reaches the end of the input stream, ensuring the connection stays open long enough to receive the server's full response

# Bandit Level 17 → Level 18

## Concept
There are 2 files in the homedirectory: passwords.old and passwords.new. The password for the next level is in passwords.new and is the only line that has been changed between passwords.old and passwords.new

## Process
1. List the files in the home directory using `ls`
2. Compare both files with `diff passwords.old passwords.new`
3. `<` shows content from passwords.old `>` shows content from passwords.new

## What I learned
- diff is used to compare two files line by line and highlight differences
- The symbols <` and `>` indicate the source of each changed line
- Simple comparison tools are often enough to detect changes without additional filtering tools like `grep`

# Bandit Level 18 → Level 19

## Concept
When connecting via SSH as bandit18, the session immediately terminates after login.
Despite this restriction, SSH still allows the execution of non-interactive remote commands. The goal is to retrieve the password for the next level using this behavior.

## Process
1. Attempt a normal SSH login:
2. 

