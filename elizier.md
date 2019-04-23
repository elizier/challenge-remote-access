This Challenge talks about  "SSH connection over HTTP Proxy" using a distinguish port(i.e: 8000).
As talked with interviewer and according the problem announces, the challenge is not about give the full solution, but expose the skill of management and generate a strategy of how-to solve the question. So i decided to talk about the main problem and give two ways to solve and won't talk about all verbose messages wich of each command can generate.

# SUBJECT and 2 Solutions Approach:

A proxy is an intermediary that forwards requests from clients to other servers.  Performance improvement, load balancing, security or access control are some reasons proxies are used.  
Here, we just want, starting at "Client A", pass through (or JUMP, as you well understand) the proxy server "Server A" and reach a service on target server "Server B".
For this moment will explaine 2, but not finals, solutions ways:
- SOCKS Proxy Via an Intermediate Host
- Port Forwarding Via an Intermediate Host


### SOCKS PROXY explanation

It is possible to connect via an intermediate machine using a SOCKS proxy,  that are currently supported by OpenSSH.  SOCKS allows transparent traversal of a firewall or other barrier by an application. Dynamic application-level port forwarding allows the outgoing port to be allocated on the fly thus creating a proxy at the TCP session level.  


## SOCKS Proxy Via a Single Intermediate Host

If you want to open a SOCKS proxy via an intermediate host, it is possible:


`$ ssh -L 8000:localhost:8000 user1@serverA.com -t ssh -D 8000 user2@serverB.com`


In this example, the client will see a SOCKS proxy on port 8000 on the local host, which is actually a connection to ''serverA'' and the traffic will ultimately enter and leave the net through ''serverB''.  Port 8000 on the local host connects to port 8000 on ''serverA'' which is a SOCKS proxy to ''serverB''.  The port numbers can be chosen to be whatever is needed, but forwarding privileged ports still requires root privileges.

## Port Forwarding Via an Intermediate Host

Tunneling, also called port forwarding, is when a port on one machine mapped to a connection to a port on another machine.  In that way remote services can be accessed as if they were local.  Or in the case of reverse port forwarding, vice verse.  Forwarding can be done directly from one machine to another or via a machine in the middle.  

Below we are setting up a tunnel from the ''localhost'' to ''serverB'', which is behind a proxy server, ''serverA''.  The tunnel will be via ''serverA'' which is publicly accessible and also has access to ''serverB''.


`$ ssh -L 8000:machineB.com:80 serverA.com`


Next connecting to the tunnel will actually connect to the second host, ''machineB''.


`$ ssh -p 8000 user@localhost`


# Conclusion

Basically, what we need to do is :
I can can only access a remote server named ‘serverB’ via ssh by first login into an intermediary server called ‘ServerA’. First, login to server:  

`$ ssh -D8000 user1@serverA`  

Next, I must ssh through the intermediary system as follows:  
`$ ssh -D 8000 user2@serverB`

We can optimize these solutions by making local scripts that holds users servers and ports and customize the ~/.ssh/config file.

### As a DevOps watcher, these kind of solutions is to keep it simple but has to be optimized at the end and encrypted 'cause in release environments its opens serious backdoor problems.

That's all folks.

Elizier Santos 
