## Linux Exploition

### Important Files To Look Into
	cat
		/etc/shadow
	
### Escalate Privs Using Sudo / SUID / SGID / etc.
	X = 4000 (SUID) : 2000 (SGID) : 6000 (Either)
	find / -type f -perm /X -ls 2>/dev/null
		
	sudo -l, find perms and check gtfo bins and create persistence that way	
		
### Find Writeable Files and Directories
	find / -type f -writable -o -type d -writable 2>/dev/null

### '.' in path, can run command in whatever dir there in but create our own command
	. in path, will run in current directory
	if user with dot is in tmp for example, and we have /tmp/ls script
	when the user runs ls, it will run the /tmp/ls script we made before it runs
	/bin/ls
	If elavated user, can do bigger things

### Create Persistence
	System vs user CRON
	Persistance can be gotten with cron

	Kernel Modules Backdoors
	find /var/spool/cron/crontabs /etc/cron* -writable -ls


### Linux MSFConsole Exploit / Script ( Biffer Overflow )
	1) Run file that is vulnerable and see if you it can be buffer overflowed, check for
	segmentation fault (core dumped)
	2) 

### Misc