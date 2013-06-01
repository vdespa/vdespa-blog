# Firebird - Converting BLOB / BINARY column in STRING #

This article will try to resolve two problems. Viewing a BLOB inside a database administration tool like FlameRobin and / or using it inside a server side script, like PHP.

## Viewing a BLOB inside a database administration tool ##

You have a column which stores information as BLOB, but the information itself is text. With the following query you can display the contents of this field:

    SELECT      t1.*, 
  		CAST(SUBSTRING(blob1 FROM 1 FOR 32000) AS VARCHAR(32000)) AS myblobfield 
    FROM        t1

Please note that maximum length for VARCHAR is 32767 bytes and if the data gets truncated, the query may fail. Also if the BLOG is a multiline text, only the first line will be displayed.

Now I am not saying that this is the best method or something, it is just a solution that worked for me. If you know another one, a better, simpler way to do it, drop a message in the comment section below.

## Reading a BLOB field in PHP ##

PHP offers natively the possibility of getting the BLOB data as a string. 

For example `ibase_fetch_assoc ( resource $result [, int $fetch_flag = 0 ] )`  provides the `$fetch_flag` parameter which can be set to `IBASE_TEXT`, causing the function to return BLOB contents instead of BLOB ids.

Practical example:
		// Connect to database
		$dbh = ibase_connect($host, $username, $password);
		
		// Build query
		$query = "SELECT * FROM t1";
		
		// Get result
		$result = ibase_query($dbh, $query);
				
		// Loop through results
		while ($row = ibase_fetch_object($result, IBASE_TEXT)) 
		{
			var_dump($row);
		}

### Resources ###

How to convert BLOB to string? - http://www.firebirdfaq.org/faq250/

Firebird CAST() - http://www.firebirdsql.org/refdocs/langrefupd21-intfunc-cast.html

PHP ibase_fetch_assoc - http://www.php.net/manual/en/function.ibase-fetch-assoc.php

Advantages/disadvantages of BLOBs vs. VARCHARs - http://www.volny.cz/iprenosil/interbase/ip_ib_strings.htm
