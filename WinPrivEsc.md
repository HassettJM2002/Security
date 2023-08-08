
Jump Box:   ssh student@10.50.20.235 -X 
                        CHT8QBJ8TQBlv7S
                        JOHA-005-M

            LinOps:     ssh student@10.50.40.22 -X  

            WinBox:     ssh student@10.50.37.222
            
            Pivot: 	ssh -p 2222 comrade@192.168.28.105 -> comrade :: StudentReconPassword
            
            T1:	ssh comrade@192.168.28.5 -> comrade :: StudentPrivPassword
  
   c:\windows\system32\fortnite.exe
  
Lin : 14000 -> 
          
Pivot: 
	IPs:
		Int/Ext: 192.168.28.105/27
	Ports:
		2222 (SSH)

----------------------------------------------
T1: xfreerdp /u:comrade /v:192.168.28.5 /dynamic-resolution +glyph-cache +clipboard
	IPS:
		192.168.28.5
	Ports:
		135/tcp  open  msrpc
		139/tcp  open  netbios-ssn
		445/tcp  open  microsoft-ds
		3389/tcp open  ms-wbt-server
		5040/tcp  open  unknown
		5985/tcp  open  wsman
		5986/tcp  open  wsmans
		47001/tcp open  winrm
		49664/tcp open  unknown
		49665/tcp open  unknown
		49666/tcp open  unknown
		49667/tcp open  unknown
		49668/tcp open  unknown
		49669/tcp open  unknown
		49670/tcp open  unknown
		49671/tcp open  unknown
		
C:\MemoryStatus\service.exe		-> hijackmeplz.dll
