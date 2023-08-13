## Scanning Methods

##  (OPTIONAL) If you only have /bin/sh
	run 'bash' to get bash shell

### (On the first box) Ping Sweeps ( On a target computer that can see network )
	Run it on every network, re run it on every new box to find the box
	```shell
	whereis ping 
	for i in {1..254}; do (/bin/ping -c 1 192.168.1.$i | grep "bytes from" &); done
	```
### NMAP Scans (On machines that are up)
	Scan a box
	```shell
		nmap <ip>
	```
	
	if they have http
	```shell
	nmap -v -sT -T5 --script http-enum.nse <ip>
	```
	
### Transfering Data
	scp <source> <destination>
	
	On recieving 
		nc -lvp [n] <recv. port> > out.file
	
	On Sending end
		nc -w 3 [dest. ip] <recv. port> < out.file