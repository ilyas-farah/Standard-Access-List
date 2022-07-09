# Standard-Access-List
-------------------
R1
----
int g0/0
ip address 192.168.1.1 255.255.255.0
no shutdownR1
int g0/2
ip address 10.1.1.1 255.255.255.0
no shutdown

Default Route
-------------
ip route 0.0.0.0 0.0.0.0 192.1.1.2

DCHP configuration
-------------------
R1
----
ip dhcp pool lan1
default-router 192.168.1.1
network 192.168.1.0 255.255.255.0
R1(dhcp-config)#exit
R1(dhcp-config)#default-router 10.1.1.1
network 10.1.1.0 255.255.255.0
end

R2
-----
int g0/1
ip address 8.8.8.1 255.255.255.0
R2(config-if)#no shutdown
int g0/0
ip address 192.1.1.2 255.255.255.252
no shutdown

NAT Configuration
------------------
R1
----
int g0/1
ip nat outside 
R1(config-if)#exit
int g0/0
ip nat inside 
exit
int g0/2
ip nat inside 
R1(config-if)#exit
access-list 3 permit 10.1.1.0 0.0.0.255
ip nat inside source list 3 interface g0/1 overload 
End 
