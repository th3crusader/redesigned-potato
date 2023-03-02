# redesigned-potato
Task 2 - Add IP to correct interfaces and host devices

Task 3 - Config Routers
       - To get each interface to communicate with it's neighbor
Task 4 - VLAN configure trunks & native. Add IP address to default gateway
Task 5 - LACP use ?
Task 6 - Spanning tree portfast
Task 7 - ip route 0.0.0.0 ::/ [port]
Task 8 - router ospf 10 [if lost use ?]
        - default-information originate
        - router id
        - network
        - passive for the LANS

Task 9 - 
ip dhcp exclude 1st & 2nd
ip dhcp exclude default gateway
ip dhcp pool [name]
ip dhcp network
ip dhcp default-router [default gateway]
ip dhcp dns server [if needed]


Task 10 - IPV6 addressing [ add ipv6 to correct device/interface]
        - Static default route on r2 [::/ (interface)]

Task 11 - you should know how to do the first step !!? will give you the cmds!!
        switchport port security ?
Task 12 - TEST connections
Task 13 - copy run tftp IP address

Basic Configuration on Router
hostname - h
banner   - ban m #xxxx#
  *passwords*
    - line con 0 & vty line 0 15
    - enable sec
encrypt password - service password encryption
copy run tftp

Basic Switch Configuration
   *interfaces*
    - Description
    - Default Gateway

SSH
name the shit - domain n
  *vty line 0 15*
    - transport input ssh 
    - login local
username __ secret __
crypto key gen rsa
copy run tftp

Interfaces v4/v6
ip add x.x.x.x subnet
desc 
no shut
ipv6 add
ipv6 add fe80:: link-local
desc
no shut

Inter VLAN Routing only on switches
  *Create / Name VLANs*
    - Vlan # -> name
      *switchport modes*
        - switchport mode access vlan #
        - switchport mode trunk 
        - switchport trunk native vlan #

Router on a Stick(peg-leg)
  *Sub-interfaces*
    - interface g0/0/.#
    - encapsulation dot1q # (native) |do this before ip add|
    - ip add x.x.x.x
    - interface g0/0 (main interface) -> no shut

EtherChannels
Int Range     - int range fa0/1-2
shutdown
switchport mode trunk
switchport nonegotiate

Channel Group - channel-group # mode ____
  (Modes - passive, active, desirable -- ? shows PAgP or LACP)
spanning-tree vlan # root primary

DHCP
Exclude IP (Net ID, BRO, Default) - ip exclude-add
Name DHCP - ip dhcp pool ____
  *Config DHCP Pool*
    - network        -> netid subnet
    - default-router -> router IP
    - dns-server     -> server IP
    - domain-name    -> domain name
Replay INT - ip helper-add

Static Routes
  *Identify Type Connection (Recursive / Directly Connected)*
    - Recursive          - next hop ip
    - Directly Connected - will be a interface se0/0 *(Never g0/0)*
  *Identify Type of Router*
                     (destination, destination subnet, next interface ip)
    - Default            - (ipv4) 0.0.0.0 0.0.0.0 exit int OR ip add     (ipv6) ::/0 exit or ipadd
    - LAN                - (ipv4) LanID LanSubnet exit/ipadd             (ipv6) Lanip/prefix exit/ipadd
    - Floating           - (all you got to do is add admin distance) ->  # at end of ip routes
    - Host               - (ipv4) Host IP 255.255.255.255 exit/ip        (ipv6) Hostip/128 exit/ip

OSPF
Create OSPF with process ID - router ospf #
Router ID                   - router-id x.x.x.x
Net Wild Card               - net id subnet area # *(Opposite Subnet)*
Make lan connection Passive - passive int |interlace not used|

Port Security
Shut down ports - int range -> shutdown
Sticky Mac      - switchport port-security mac-add sticky
Max Mac         - switchport port-security maximum
Violation Mode  - switchport port-security violation ____

Extras
Port Fast   - spanning-tree portfast
DNS Disable - no ip domain-lookup
clockrate 128000
default originate
.
