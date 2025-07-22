# Section 1 - Network Connectivity
## Finding a Proxy
A proxy may be defined in various places, such as:
**Linux**

**Windows**

### Identifying what proxy to use
Most network segments, and by extension, the servers in them, require a proxy for egress.  **Firstly, ask the admin you're talking to, to see if they know!**

**If they do not know, you will need to work with them to identify it!!!!** 

### Checking DNS is Resolving
Once you think you know what proxy to use (or even uses an FQDN for your instance), you will want to check you can actually resolve the FQDNs.
  **Linux**
    
    dig <<FQDN>>
    
  OR
  
    host <<FQDN>>
  
  OR
  
    resolvectl query <<FQDN>>

  **Windows**
    
    Resolve-DnsName -Name <<FQDN>> -Verbose
    
  OR 

    ns lookup <<FQDN>>
    

## Reaching the Proxy
### Checking Reachability to the Proxy
  **Linux**

    echo > /dev/tcp/<<FQDN|IP>>/<<Port>> && echo "Port is open" || echo "Port is closed"

  OR
  
    curl -Is http://<<FQDN|IP>>:<<Port>>| head -n 1
*N.B. Reachability would be shown as a HTTP response (even if it's a `400` response) a failed to connect to proxy would show a blank line*


  **Windows**
  
    Test-NetConnection -ComputerName <<FQDN|IP>> -port <<Port>>


## Checking Connextivity Through the Proxy
The velociraptor server hosts it's server public key at `/server.pem` which makes an excellent target for something to test downloads.
  **Linux**

    curl -x http://<<Proxy_FQDN|IP>>:<<Port>> https://<<Veloci_Server_FQDN|IP>>:<<Agent_Port-typically-8000>>/server.pem -k

 or

    wget --no-check-certificate -qO- -e use_proxy=yes -e http_proxy=<<Proxy_FQDN|IP>>:<<Port>> https:// https://<<Veloci_Server_FQDN|IP>>:<<Agent_Port-typically-8000>>/server.pem 


 **Windows**

    Invoke-WebRequest -Uri https:/<<Veloci_Server_FQDN|IP>>:<<Agent_Port-typically-8000>>/server.pem -Proxy http://<<Proxy_FQDN|IP>>:<<Port>>

## Setting the Proxy
You can set a proxy in the agent configuration either pre- or post-installation by defining a YAML field called `proxy` inside of `Client`

***N.B.** if you do this post installation, you need to restart the service*

Configuration file is stored:
* (Windows): `C:\Program Files\Velociraptor\client.config.yaml`
* (Linux): `/etc/velociraptor/client.config.yaml`

