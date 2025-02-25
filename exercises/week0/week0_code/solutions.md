# How I caught this week's flags

Url to exercises: https://hack.redrocket.club/course/OS-Week0/problems/
* exercise1_GPUPOWER: http://vuln.redrocket.club:8002/?
* exercise2_Hashed: http://vuln.redrocket.club:8000/?
* exercise3_JSS: http://vuln.redrocket.club:8888/?
* exercise3_JSS-file: http://redrocket.club/exercise3_JSS.js
* exercise4_NiceCMS-normal_link: http://hack.hctf.fun:37366
* exercise4_NiceCMS-link_to_admin.php: http://hack.hctf.fun:37366/admin.php
* exercise5_OhCanada-normal_link: http://vuln.redrocket.club:8004
* exercise5_OhCanada-modified_link: http://vuln.redrocket.club:8004/docs/document_"
* exercise6_REGEX: http://vuln.redrocket.club:4444/?
* exercise8_SessionOne: http://vuln.redrocket.club:1920/
* exercise9_SessionTwo: http://vuln.redrocket.club:1921/

### Exercise 1 GPUPOWER
* The flag was in a file flag.txt
* you see the name of the file on the php website and the path (there was none)
* it was accessable by just addin /flag.txt to the url 

### Exercise 2 Hashed
* there is a vulnerability when using md5 hashes for passwords
* if you are looking for a pw stating with "0e..." (only numbers following) 
* and you are using php == to compare your pw to the input
* then you need to be aware of this: https://owasp.org/www-pdf-archive/PHPMagicTricks-TypeJuggling.pdf

### Exercise 3 JSS
* you were given some jss-code 
* after understanding it completely 
* you will see that the password is generated with a lot of effort 
* but it is always the same
* so you just copy the script, execute it in your ide and print the generated pw to the console
* then you can enter the pw in the webapp and get the flag

### Exercise 4 Nice CMS
* you are given the webapp's url and the admin script
* the admin script contains the flag so this is the script you need to call 
* you need to understand that you are redirected in the if condition
* but redirecting is a mechanism that you can avoid 
* e.g. curl automatically does not redirect you 
* you can get the flag by calling the admin script and setting the -v option in curl
* the flag is hidden in the body 

### Exercise 5 OhCanada 
* you can access all documents via changing the url (counting up)
* you can check all url if they contain the flag 
* however in this case the flag was in one of the documents
* so you need to download all of them and check the content (preview was sufficient)

### Exercise 6 Regex
* this was a regex problem
* you need to set a parameter uid so that a regex is fulfilled 
* but the parameter cannot be 000 which fulfills the regex and would get you the password
* but there is an if that says it cannot be "000"
* so you need to be smarter than the regex and insert something that passes 
* the regex condition and is !="000"
* so basically 000 with something attached 
* the beginning of a line in regex is ^
* the ending of a line in regex is $
* this was used here: you can tell the url to end a line with "%0A"
* So you enter 000%0A
* and pass the regex and your string is !=0 
* and get the flag 
* regex cheat sheet: https://www.rexegg.com/regex-quickstart.html

### Exercise Session One
* do a normal curl to see what data is there
* find cookie user=shortencoded thing
* base64 was used for encoding and you can find a decoder for that on the internet
* decoded word was guest so encode the word "admin"
* set encoded admin word as user cookie using curl -b 
* flag is found in the body 

### Exercise Session two
* James starts at 0
* James start at -1219
* Alonso ends at 1219
* Alsonso ends at -1 
* so we have 1219 numbers from -1219 to -1
* and 1219 numbers from 1 to 1219
* write a script to try out all session_ids
* look for each session_id in the text for "flag{"  




