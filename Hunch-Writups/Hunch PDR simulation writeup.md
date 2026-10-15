### Appliances Decisions
---
__VyOS__
- Can use as router and a firewall, so one appliance can cover both routing, firewall, and vpn/ipsec
__OpenVSwitch__
- Allows for ethernet switching, and easy configuration for vlan
__VPCs__
- Low impact on simulation that allows for pinging and tracing for ip configuring without requireing full simulation or docker container so that many can be used
__Alpine docker nodes__
- Also low impact, but requires a little bit more for compute power(still very little) 
- Allows for much more configuration with package manager to be able to install need programs
__Nat node__
- GNS3 nat node allows simulated internet connection
__Delays__
- Using built in connections, able to simulate 1.28 second delay between earth and moon
### Topology
---
__Current GNS3 Topology:__
![GNS3 Topology Screenshot](current-topology.png)
As of now the moon side is not fully set up, however it will be a near mirror image of the earth side

__Topology Explaination:__
All traffic comes through VyOS router, which acts both as a router: VLANs, internet connection, dhcp assigning, routing connection to moon, etc. And as a firewall: general firewall capabilities, inter VLAN firewall, VPN/ipsec, etc.
After traffic passes router and firewall the core switch splits into different VLANS(eg. 10 for users, and 20 for lab)
After core switch each of the VLAN groups have their own switch that split the ethernet to route to each individual device

Above the router there is an internet node that allows devices in the topology to connect to internet incase they need to install packages
- The internet is only on the earth side the moon router will no matter what need to communicate with the earth so only being connected to earth router simulates 1.28 second delay from moon to earth
The router is also connected to the moon via cable that simulates delay and packet loss.

### Configurations
---
#### VyOS:
[Full Configuration Here](VyOS-configuration.txt)

__Interfaces__

| Interface | IP Address                              | MAC               | VRF     | MTU   | S/L | Description          |
| --------- | --------------------------------------- | ----------------- | ------- | ----- | --- | -------------------- |
| eth0      | -                                       | 0c:f4:c4:3f:00:00 | default | 1500  | u/u | EARTH-LAN VLAN TRUNK |
| eth0.10   | 10.10.10.1/24                           | 0c:f4:c4:3f:00:00 | default | 1500  | u/u | USERS                |
| eth0.20   | 10.10.20.1/24                           | 0c:f4:c4:3f:00:00 | default | 1500  | u/u | LABS                 |
| eth0.30   | 10.10.30.1/24                           | 0c:f4:c4:3f:00:00 | default | 1500  | u/u | IOT                  |
| eth0.40   | 10.10.40.1/24                           | 0c:f4:c4:3f:00:00 | default | 1500  | u/u |                      |
| eth0.50   | 10.10.50.1/24                           | 0c:f4:c4:3f:00:00 | default | 1500  | u/u |                      |
| eth0.60   | 10.10.60.1/24                           | 0c:f4:c4:3f:00:00 | default | 1500  | u/u |                      |
| eth0.70   | 10.10.70.1/24                           | 0c:f4:c4:3f:00:00 | default | 1500  | u/u |                      |
| eth0.80   | 10.10.80.1/24                           | 0c:f4:c4:3f:00:00 | default | 1500  | u/u |                      |
| eth0.90   | 10.10.90.1/24                           | 0c:f4:c4:3f:00:00 | default | 1500  | u/u |                      |
| eth0.100  | 10.10.100.1/24                          | 0c:f4:c4:3f:00:00 | default | 1500  | u/u | MGMT                 |
| eth1      | 10.30.0.1/30                            | 0c:f4:c4:3f:00:01 | default | 1500  | u/u | TRANSIT UP           |
| eth2      | dhcp(192.168.42.230/24)                 | 0c:f4:c4:3f:00:02 | default | 1500  | u/u | INTERNET-WAN         |
| lo        | 127.0.0.1/8<br>10.255.0.1/32<br>::1/128 | 00:00:00:00:00:00 | default | 65536 | u/u |                      |

__Firewall__
```txt
Rulesets Information  
  
---------------------------------  
ipv4 State Policy  
  
State          Packets    Bytes  Conditions  
-----------  ---------  -------  ----------------------------  
established         61     5721  ct state established  accept  
invalid              0        0  ct state invalid  
related              0        0  ct state related  accept  
  
---------------------------------  
ipv4 Firewall "forward filter"  
  
Rule     Action    Protocol      Packets    Bytes  Conditions  
-------  --------  ----------  ---------  -------  ----------------------------------------------------------------------------------  
30       accept    all                 0        0  ip daddr @N_LAB-IOT-ALLOWED ip saddr @N_IOT-ALLOWED-NET  accept  
31       accept    tcp                 0        0  ip daddr @N_LAB-IOT-ALLOWED tcp dport @P_HTTPS ip saddr @N_IOT-LIMITED-NET  accept  
35       accept    all                 0        0  ip daddr @N_IOT-NET ip saddr @N_LAB-IOT-ALLOWED  accept  
100      accept    all                 0        0  ip saddr @N_MGMT-NET  accept  
oifname "eth2"  accept  
ction "  
  
---------------------------------  
ipv6 State Policy  
  
State          Packets    Bytes  Conditions  
-----------  ---------  -------  ----------------------------  
established          0        0  ct state established  accept  
invalid              0        0  ct state invalid  
related              0        0  ct state related  accept
```

#### Core Switch
[Full Configuration Here](ovs-configuration.txt)
- VLAN configuration:
```txt
name                : eth11  
vlan_mode           : []  
tag                 : []  
trunks              : []  
  
name                : br0  
vlan_mode           : []  
tag                 : []  
trunks              : []  
  
name                : eth3  
vlan_mode           : []  
tag                 : 30  
trunks              : []  
  
name                : eth7  
vlan_mode           : []  
tag                 : []  
trunks              : []  
  
name                : eth6  
vlan_mode           : []  
tag                 : []  
trunks              : []  
  
name                : eth1  
vlan_mode           : []  
tag                 : 10  
trunks              : []  
  
name                : eth12  
vlan_mode           : []  
tag                 : []  
trunks              : []  
  
name                : eth10  
vlan_mode           : []  
tag                 : []  
trunks              : []  
  
name                : eth8  
vlan_mode           : []  
tag                 : []  
trunks              : []  
  
name                : eth5  
vlan_mode           : []  
tag                 : []  
trunks              : []  
  
name                : eth15  
vlan_mode           : []  
tag                 : []  
trunks              : []  
  
name                : br1  
vlan_mode           : []  
tag                 : []  
trunks              : []  
  
name                : eth9  
vlan_mode           : []  
tag                 : []  
trunks              : []  
  
name                : br3  
vlan_mode           : []  
tag                 : []  
trunks              : []  
  
name                : eth0  
vlan_mode           : []  
tag                 : []  
trunks              : [10, 20, 30, 40, 50, 60, 70, 80, 90, 100]  
  
name                : eth2  
vlan_mode           : []  
tag                 : 20  
trunks              : []  
  
name                : eth14  
vlan_mode           : []  
tag                 : []  
trunks              : []  
  
name                : br2  
vlan_mode           : []  
tag                 : []  
trunks              : []  
  
name                : eth4  
vlan_mode           : []  
tag                 : 100  
trunks              : []  
  
name                : eth13  
vlan_mode           : []  
tag                 : []  
trunks              : []
```

### Tests
---
__Ping router:__
- User pcs:
```txt
  
VPCS> show ip  
  
NAME        : VPCS[1]  
IP/MASK     : 10.10.10.10/24  
GATEWAY     : 10.10.10.1  
DNS         :    
MAC         : 00:50:79:66:68:00  
LPORT       : 20036  
RHOST:PORT  : 127.0.0.1:20037  
MTU         : 1500

VPCS> ping 10.10.10.1  
  
84 bytes from 10.10.10.1 icmp_seq=1 ttl=64 time=2.297 ms  
84 bytes from 10.10.10.1 icmp_seq=2 ttl=64 time=2.077 ms  
84 bytes from 10.10.10.1 icmp_seq=3 ttl=64 time=2.769 ms  
84 bytes from 10.10.10.1 icmp_seq=4 ttl=64 time=2.362 ms  
84 bytes from 10.10.10.1 icmp_seq=5 ttl=64 time=3.220 ms  
  
VPCS> trace 10.10.10.1  
trace to 10.10.10.1, 8 hops max, press Ctrl+C to stop  
1   *10.10.10.1   1.929 ms (ICMP type:3, code:3, Destination port unreachable)
```
- Lab
```txt
iface eth0 inet static  
       address 10.10.20.10  
       netmask 255.255.255.0  
       gateway 10.10.20.1  
       up echo nameserver 10.10.20.1 > /etc/resolv.conf
       
/ # ping 10.10.20.1  
PING 10.10.20.1 (10.10.20.1): 56 data bytes  
64 bytes from 10.10.20.1: seq=0 ttl=64 time=6.143 ms  
64 bytes from 10.10.20.1: seq=1 ttl=64 time=5.458 ms  
64 bytes from 10.10.20.1: seq=2 ttl=64 time=5.676 ms  
64 bytes from 10.10.20.1: seq=3 ttl=64 time=4.651 ms  
64 bytes from 10.10.20.1: seq=4 ttl=64 time=7.462 ms  
^C  
--- 10.10.20.1 ping statistics ---  
5 packets transmitted, 5 packets received, 0% packet loss  
round-trip min/avg/max = 4.651/5.878/7.462 ms  
/ # traceroute 10.10.20.1  
traceroute to 10.10.20.1 (10.10.20.1), 30 hops max, 60 byte packets  
1  10.10.20.1 (10.10.20.1)  6.567 ms  6.757 ms  6.719 ms
```
- IoT
```txt
IOT-allowed> show ip 

NAME        : IOT-allowed[1]
IP/MASK     : 10.10.30.11/24
GATEWAY     : 10.10.30.1
DNS         : 
MAC         : 00:50:79:66:68:02
LPORT       : 20034
RHOST:PORT  : 127.0.0.1:20035
MTU         : 1500

IOT-allowed> ping 10.10.30.1

84 bytes from 10.10.30.1 icmp_seq=1 ttl=64 time=6.481 ms
84 bytes from 10.10.30.1 icmp_seq=2 ttl=64 time=5.069 ms
84 bytes from 10.10.30.1 icmp_seq=3 ttl=64 time=5.941 ms
84 bytes from 10.10.30.1 icmp_seq=4 ttl=64 time=4.610 ms
84 bytes from 10.10.30.1 icmp_seq=5 ttl=64 time=6.805 ms

IOT-allowed> trace 10.10.30.1
trace to 10.10.30.1, 8 hops max, press Ctrl+C to stop
 1   *10.10.30.1   7.562 ms (ICMP type:3, code:3, Destination port unreachable)
```

__Inter VLAN pinging(Including Firewall Restrictions):__
- Personal computers cannot access either lab or IoT
```txt
VPCS> ping 10.10.20.10  
  
10.10.20.10 icmp_seq=1 timeout  
10.10.20.10 icmp_seq=2 timeout  
^C  
VPCS> trace 10.10.20.10  
trace to 10.10.20.10, 8 hops max, press Ctrl+C to stop  
1   10.10.10.1   4.360 ms  1.713 ms  2.624 ms  
2     *  *  *  
3     *  *  *  
^C 4     *  *  
  
VPCS> ping 10.10.30.10  
  
10.10.30.10 icmp_seq=1 timeout  
10.10.30.10 icmp_seq=2 timeout  
^C  
VPCS> trace 10.10.30.10  
trace to 10.10.30.10, 8 hops max, press Ctrl+C to stop  
1   10.10.10.1   3.109 ms  2.394 ms  2.517 ms  
2     *  *  *  
3     *  *  *  
^C 4
```
- General lab pcs cannot acces IoT
```txt
iface eth0 inet static  
       address 10.10.20.10  
       netmask 255.255.255.0  
       gateway 10.10.20.1  
       up echo nameserver 10.10.20.1 > /etc/resolv.conf

/ # hostname  
Lab-Device-1  
/ # ping 10.10.30.10  
PING 10.10.30.10 (10.10.30.10): 56 data bytes  
^C  
--- 10.10.30.10 ping statistics ---  
5 packets transmitted, 0 packets received, 100% packet loss  
/ # traceroute 10.10.30.10  
traceroute to 10.10.30.10 (10.10.30.10), 30 hops max, 60 byte packets  
1  10.10.20.1 (10.10.20.1)  7.808 ms  9.577 ms  9.651 ms  
2  * * *  
3  * * *  
4  * * *  
5  * * *  
6  * * *
```
- However allowed ones are able
```txt
iface eth0 inet static  
       address 10.10.20.11  
       netmask 255.255.255.0  
       gateway 10.10.20.1  
       up echo nameserver 10.10.20.1 > /etc/resolv.conf

/ # hostname  
Lab-Device-2  
/ # ping 10.10.30.10  
PING 10.10.30.10 (10.10.30.10): 56 data bytes  
64 bytes from 10.10.30.10: seq=0 ttl=63 time=30.080 ms  
64 bytes from 10.10.30.10: seq=1 ttl=63 time=12.908 ms  
64 bytes from 10.10.30.10: seq=2 ttl=63 time=10.998 ms  
64 bytes from 10.10.30.10: seq=3 ttl=63 time=10.934 ms  
^C  
--- 10.10.30.10 ping statistics ---  
4 packets transmitted, 4 packets received, 0% packet loss  
round-trip min/avg/max = 10.934/16.230/30.080 ms  
/ # traceroute 10.10.30.10  
traceroute to 10.10.30.10 (10.10.30.10), 30 hops max, 46 byte packets  
1  10.10.20.1 (10.10.20.1)  4.929 ms  5.546 ms  4.982 ms  
2  10.10.30.10 (10.10.30.10)  11.397 ms  14.180 ms  14.750 ms
```
- Some IoT devices cannot access rest of network
```txt
iface eth0 inet static  
       address 10.10.30.10  
       netmask 255.255.255.0  
       gateway 10.10.30.1  
       up echo nameserver 192.168.0.1 > /etc/resolv.conf
       
/ # hostname  
Iot-sensor  
/ # ping 10.10.20.11  
PING 10.10.20.11 (10.10.20.11): 56 data bytes  
^C  
--- 10.10.20.11 ping statistics ---  
5 packets transmitted, 0 packets received, 100% packet loss
/ # traceroute 10.10.20.11  
traceroute to 10.10.20.11 (10.10.20.11), 30 hops max, 46 byte packets  
1  10.10.30.1 (10.10.30.1)  6.036 ms  6.750 ms  5.608 ms  
2  *  *  *  
3  *  *  *  
4^C
```
- However some can(But only specific lab devices)
```txt
  
IOT-allowed> show ip  
  
NAME        : IOT-allowed[1]  
IP/MASK     : 10.10.30.11/24  
GATEWAY     : 10.10.30.1  
DNS         :    
MAC         : 00:50:79:66:68:02  
LPORT       : 20034  
RHOST:PORT  : 127.0.0.1:20035  
MTU         : 1500  
  
IOT-allowed> ping 10.10.20.11  
  
84 bytes from 10.10.20.11 icmp_seq=1 ttl=63 time=13.187 ms  
84 bytes from 10.10.20.11 icmp_seq=2 ttl=63 time=12.179 ms  
84 bytes from 10.10.20.11 icmp_seq=3 ttl=63 time=11.038 ms  
^C  
IOT-allowed> trace 10.10.20.11  
trace to 10.10.20.11, 8 hops max, press Ctrl+C to stop  
1   10.10.30.1   8.238 ms  5.849 ms  4.637 ms  
2   *10.10.20.11   11.725 ms (ICMP type:3, code:3, Destination port unreachable)  
  
IOT-allowed> ping 10.10.20.10  
  
10.10.20.10 icmp_seq=1 timeout  
10.10.20.10 icmp_seq=2 timeout  
^C  
IOT-allowed> ping 10.10.10.10  
  
10.10.10.10 icmp_seq=1 timeout  
10.10.10.10 icmp_seq=2 timeout
```
- And some can only access devices through a specifc port(eg. 443 for https)
```
SENDER:
iface eth0 inet static  
       address 10.10.30.12  
       netmask 255.255.255.0  
       gateway 10.10.30.1  
       up echo nameserver 10.10.30.1 > /etc/resolv.conf
       
/ # hostname  
IOT-limited  
/ # echo "this should send" | nc 10.10.20.11 443  
^Cpunt!  
  
/ # echo "but this should not" | nc 10.10.20.11 80


RECIEVER:
iface eth0 inet static  
       address 10.10.20.11  
       netmask 255.255.255.0  
       gateway 10.10.20.1  
       up echo nameserver 10.10.20.1 > /etc/resolv.conf
       

/ # hostname  
Lab-Device-2
/ # nc -l -p 443  
this should send  
^Cpunt!

/ # nc -l -p 80  
^Cpunt!
```
*As you can see only the message on port 443 sent, and the message on port 80 did not go through as it was prevented by firewall*

- Pinging from/to the moon takes extra time
```txt
NAME        : VPCS[1]  
IP/MASK     : 10.20.10.10/24  
GATEWAY     : 10.20.10.1  
DNS         :    
MAC         : 00:50:79:66:68:01  
LPORT       : 20038  
RHOST:PORT  : 127.0.0.1:20039  
MTU         : 1500

VPCS> ping 10.30.0.1 -w 5000  
  
84 bytes from 10.30.0.1 icmp_seq=1 ttl=63 time=2565.467 ms  
84 bytes from 10.30.0.1 icmp_seq=2 ttl=63 time=2564.436 ms  
84 bytes from 10.30.0.1 icmp_seq=3 ttl=63 time=3174.030 ms  
84 bytes from 10.30.0.1 icmp_seq=4 ttl=63 time=2564.706 ms  
84 bytes from 10.30.0.1 icmp_seq=5 ttl=63 time=3280.386 ms
```
*It takes over 1280 x 2 or 2560 ms each direction as it takes 1280 ms fore each packet sent to and from the router on earth*

- Devices are able to access the internet, including using dns
```txt
iface eth0 inet static  
       address 10.10.20.11  
       netmask 255.255.255.0  
       gateway 10.10.20.1  
       up echo nameserver 10.10.20.1 > /etc/resolv.conf

/ # traceroute google.com  
traceroute to google.com (74.125.69.100), 30 hops max, 46 byte packets  
1  10.10.20.1 (10.10.20.1)  3.884 ms  4.611 ms  5.098 ms  
2  192.168.42.1 (192.168.42.1)  6.256 ms  6.699 ms  6.607 ms  
3  172.16.166.2 (172.16.166.2)  5.771 ms  7.208 ms  5.428 ms  
4  192.168.50.1 (192.168.50.1)  12.948 ms  8.433 ms  6.596 ms  
5  10.60.143.34 (10.60.143.34)  21.498 ms  18.005 ms  10.60.143.35 (10.60.143.35)  16.798 ms  
6  po-58-rur501.denver.co.denver.comcast.net (69.139.228.25)  28.096 ms  po-58-rur502.denver.co.denver.comcast.net (69.139.228.65)  18.463 ms  po-58-rur501.denver.co.denver.comcast.net (69.139.228.25)  
 17.191 ms  
7  po-2-rur501.denver.co.denver.comcast.net (24.124.155.173)  20.495 ms  po-500-xar01.denver.co.denver.comcast.net (24.124.155.161)  18.463 ms  po-2-rur501.denver.co.denver.comcast.net (24.124.155.173  
)  20.199 ms  
8  be-306-arsc1.denver.co.denver.comcast.net (24.124.155.129)  18.052 ms  po-500-xar01.denver.co.denver.comcast.net (24.124.155.161)  20.836 ms  be-306-arsc1.denver.co.denver.comcast.net (24.124.155.1  
29)  19.724 ms  
9  be-306-arsc1.denver.co.denver.comcast.net (24.124.155.129)  20.377 ms  be-36021-cs02.1601milehigh.co.ibone.comcast.net (96.110.43.245)  20.288 ms  be-306-arsc1.denver.co.denver.comcast.net (24.124.  
155.129)  21.584 ms  
10  be-3411-pe11.910fifteenth.co.ibone.comcast.net (96.110.33.126)  19.709 ms  *  be-36011-cs01.1601milehigh.co.ibone.comcast.net (96.110.43.241)  20.697 ms  
11  *  be-3411-pe11.910fifteenth.co.ibone.comcast.net (96.110.33.126)  21.063 ms  *  
12  *  *  *  
13  192.178.246.238 (192.178.246.238)  20.761 ms  108.170.250.218 (108.170.250.218)  25.055 ms  142.251.192.105 (142.251.192.105)  24.858 ms  
14  172.253.51.122 (172.253.51.122)  24.689 ms  209.85.142.170 (209.85.142.170)  23.769 ms  *  
15  192.178.246.238 (192.178.246.238)  30.468 ms  172.253.51.78 (172.253.51.78)  25.241 ms  172.253.51.118 (172.253.51.118)  20.206 ms  
16  172.253.51.116 (172.253.51.116)  19.962 ms  192.178.81.82 (192.178.81.82)  43.116 ms  108.170.252.160 (108.170.252.160)  42.025 ms  
17  192.178.81.82 (192.178.81.82)  42.204 ms  172.253.77.122 (172.253.77.122)  41.852 ms  44.447 ms  
18  192.178.81.82 (192.178.81.82)  50.291 ms  209.85.250.65 (209.85.250.65)  79.977 ms  142.251.255.221 (142.251.255.221)  130.938 ms  
19  216.239.49.123 (216.239.49.123)  43.167 ms  192.178.44.179 (192.178.44.179)  37.549 ms  *  
20  74.125.253.171 (74.125.253.171)  46.891 ms  *  *  
21  *  *  *  
22  *  *  *  
23  *  *  *  
24  *  *  *  
25  *  *  *  
26  *  *  *  
27  *  *  *  
28  *  *  *  
29  iq-in-f100.1e100.net (74.125.69.100)  42.523 ms  *  *  
/ # ping google.com  
PING google.com (142.250.125.102): 56 data bytes  
64 bytes from 142.250.125.102: seq=0 ttl=126 time=43.318 ms  
64 bytes from 142.250.125.102: seq=1 ttl=126 time=41.484 ms  
64 bytes from 142.250.125.102: seq=2 ttl=126 time=42.002 ms  
64 bytes from 142.250.125.102: seq=3 ttl=126 time=45.040 ms  
^C  
--- google.com ping statistics ---  
4 packets transmitted, 4 packets received, 0% packet loss  
round-trip min/avg/max = 41.484/42.961/45.040 ms
```