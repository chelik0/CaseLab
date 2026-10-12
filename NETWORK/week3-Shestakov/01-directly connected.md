## r-edge
### r-edge#show ip interface brief
Interface                  IP-Address      OK? Method Status                Protocol
Ethernet0/0                10.150.111.2    YES manual up                    up  
Ethernet0/1                10.150.111.9    YES manual up                    up  
Ethernet0/2                203.0.113.1     YES manual up                    up  
Ethernet0/3                198.51.100.1    YES manual up                    up  
### r-edge#show interfaces description
Interface                      Status         Protocol Description
Et0/0                          up             up       -=sw-core;e0/2=-
Et0/1                          up             up       -=r-branch;e0/1=-
Et0/2                          up             up       -=isp-a;e0/0=-
Et0/3                          up             up       -=isp-b;e0/0=-

## r-branch
### r-branch#show ip interface brief
Interface                  IP-Address      OK? Method Status                Protocol
Ethernet0/0                10.150.111.6    YES manual up                    up  
Ethernet0/1                10.150.111.10   YES manual up                    up  
Ethernet0/2                10.150.108.1    YES manual up                    up  
Ethernet0/3                unassigned      YES unset  administratively down down
### 
r-branch#show interfaces description
Interface                      Status         Protocol Description
Et0/0                          up             up       -=sw-core;e0/3=-
Et0/1                          up             up       -=r-edge;e0/1=-
Et0/2                          up             up       -=BR-PC1;access=-
Et0/3                          admin down     down

## isp-a
### isp-a#show ip interface brief
Interface                  IP-Address      OK? Method Status                Protocol
Ethernet0/0                203.0.113.2     YES manual up                    up  
Ethernet0/1                192.0.2.1       YES manual up                    up  
Ethernet0/2                unassigned      YES unset  administratively down down
Ethernet0/3                unassigned      YES unset  administratively down down
### isp-a#show interfaces description
Interface                      Status         Protocol Description
Et0/0                          up             up       -=r-edge;e0/2=-
Et0/1                          up             up       -=inet;e0/0=-
Et0/2                          admin down     down
Et0/3                          admin down     down

## isp-b
### isp-b#show ip interface brief
Interface                  IP-Address      OK? Method Status                Protocol
Ethernet0/0                198.51.100.2    YES manual up                    up  
Ethernet0/1                192.0.2.5       YES manual up                    up  
Ethernet0/2                unassigned      YES unset  administratively down down
Ethernet0/3                unassigned      YES unset  administratively down down
### isp-b#show interfaces description
Interface                      Status         Protocol Description
Et0/0                          up             up       -=r-edge;e0/3=-
Et0/1                          up             up       -=inet;e0/1=-
Et0/2                          admin down     down
Et0/3                          admin down     down

## inet
### inet#show ip interface brief
Interface                  IP-Address      OK? Method Status                Protocol
Ethernet0/0                192.0.2.2       YES manual up                    up  
Ethernet0/1                192.0.2.6       YES manual up                    up  
Ethernet0/2                unassigned      YES unset  administratively down down
Ethernet0/3                unassigned      YES unset  administratively down down
Loopback10                 192.0.2.100     YES manual up                    up  
Loopback20                 192.0.2.200     YES manual up                    up  
### inet#show interfaces description
Interface                      Status         Protocol Description
Et0/0                          up             up       -=isp-a;e0/1=-
Et0/1                          up             up       -=isp-b;e0/1=-
Et0/2                          admin down     down
Et0/3                          admin down     down
Lo10                           up             up       SERVICES



