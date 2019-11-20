# Accessing a remote service - Solution

Considering the architecture proposed in this challange, SSH Tunnels will be used to provide the required communication.

Notes:
- Client A - configure SSH Client to forward traffic from a local specified port, through Server A, to the Server B
- Server A - configure SSH server listening for connections from Client A and to forward traffic to Sever B
- Server B - Available to offer http service to Client A

In order to perform these steps successfully is necessary a valid user in these systems  
**_If the browser tool installation is required, a privileged user is necessary_**

## **Creating SSH Tunnel using OpenSSH**

The following command must be used to create SSH Tunnel on Client A:  
_ssh -L 8001:localhost:8001 user@ipservera -N_

The following command must be used to create SSH Tunnel on Server A:  
_ssh -L 8001:ipserverb:8000 user@ipserverb -N_

What the command above does is:
- -L:  key to local port forwarding
- 8001: local port to be forwarded to the remote port
- localhost: address 
- ipserverb: address to forward traffic
- 8000: remote port to access the service running on Server B
- user@ipservera: the remote SSH server with access to Server B
- user@ipserverb: the remote SSH server running target application
- -N: do not execute remote commands, useful for just forwarding ports

In order to validate access simply use the url _http://127.0.0.1:8001_ on Client A  
In this case the tool used was lynx  

Installation command:  
**sudo yum install lynx**

What the command above does is:  
- YUM (Yellowdog Updater Modified) is an open source command-line as well as graphical based package management tool for RPM (RedHat Package Manager) based Linux systems. It allows easily install, update, remove or search software packages on the system.
- install: it will automatically find and install all required dependencies for package lynx
- lynx: is the name of desired package

## Possible outputs:
If the password is incorrect:  
>[sudo] password for user:  
> Sorry, try again.  

If the service is unavailable on Server B:  
>Looking up 127.0.0.1 first  
>Looking up 127.0.0.1:8001  
>Making HTTP connection to 127.0.0.1:8001  
>Sending HTTP request.  
>HTTP request sent; waiting for response.  
>Alert!: Unexpected network read error; connection aborted.  
>Can't Access `http://127.0.0.1:8001/'  
>Alert!: Unable to access document.  

Check if the ports entered when creating the tunnel are correct:  
>channel 3: open failed: connect failed: No route to host  
>channel 3: open failed: connect failed: Connection refused  
