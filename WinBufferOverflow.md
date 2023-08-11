## Starting

>LinOps -> ssh student@10.50.40.22 :: password

>WinOps -> 10.50.37.222 : password
		
		
>Jump Box -> ssh student@10.50.20.235 :: CHT8QBJ8TQBlv7S

	->T1: 192.168.28.111 ->
		PORT: 
			SSH Port: 2222
	
	->T2: 192.168.28.105
		PORT: 
			SSH Port: 2222
			
>Jump Box -> ssh student@10.50.20.235 :: CHT8QBJ8TQBlv7S

	>donovian_grey_host (111) ->

		>T3: 192.168.150.245
			PORT: 
				135/tcp  open  msrpc
				139/tcp  open  netbios-ssn
				445/tcp  open  microsoft-ds
				3389/tcp open  ms-wbt-server
				9999/tcp open  abyss

## Tunnels
11001 - 11099

11001 ->28.111 (2222 SSH)
11002 ->150.245 (3389 RDP)
11002 ->150.245 (9999 RDP)

xfreerdp /u:comrade /v:127.0.0.1 /dynamic-resolution +glyph-cache +clipboard
