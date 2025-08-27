Download this file from given link below:
[https://github.com/Emperor-Shah/Networking-Labs/blob/main/eigrp%20telnet%20lab.pkt]
1. Lab Objective
The primary objective of this lab is to configure EIGRP (Enhanced Interior Gateway Routing Protocol) on multiple routers so they can exchange network information. Additionally, remote access has been configured using Telnet for network administration.

2. Network Topology
The network topology for this lab is shown below. It includes four routers, two switched and four Pc's connected with appropriate IP addressing.

3. Router 1 (R-1) Configuration
This is the startup configuration for Router 1, showing the complete setup for EIGRP routing and Telnet access.

version 12.2
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption
!
hostname R-1
!
!
!
enable password 1234
!
!
ip dhcp excluded-address 192.168.10.193
!
ip dhcp pool mypool
 network 192.168.10.192 255.255.255.224
 default-router 192.168.10.193
!
!
!
no ip cef
no ipv6 cef
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
interface FastEthernet0/0
 ip address 192.168.10.193 255.255.255.224
 duplex auto
 speed auto
!
interface FastEthernet0/1
 no ip address
 duplex auto
 speed auto
 shutdown
!
interface Serial0/0
 ip address 15.0.230.245 255.255.255.252
 clock rate 2000000
!
interface Serial0/1
 ip address 12.1.116.237 255.255.255.252
 clock rate 2000000
!
router eigrp 10
 network 12.1.116.236 0.0.0.3
 network 15.0.230.244 0.0.0.3
 network 192.168.10.192 0.0.0.31
 auto-summary
!
ip classless
!
ip flow-export version 9
!
!
!
!
!
!
!
!
line con 0
!
line aux 0
!
line vty 0 3
 password cyber
 login
 transport input telnet
line vty 4
 login
!
!
!
end
4. Verification
The following command outputs confirm that the EIGRP configuration and Telnet access are working correctly.

show ip protocols
This output shows that EIGRP Autonomous System 10 is active and that auto-summary has been enabled.

Routing Protocol is "eigrp  10 "
  Outgoing update filter list for all interfaces is not set
  Incoming update filter list for all interfaces is not set
  Default networks flagged in outgoing updates
  Default networks accepted from incoming updates
  EIGRP metric weight K1=1, K2=0, K3=1, K4=0, K5=0
  EIGRP maximum hopcount 100
  EIGRP maximum metric variance 1
Redistributing: eigrp 10
  Automatic network summarization is in effect
  Automatic address summarization:
    12.0.0.0/8 for FastEthernet0/0, Serial0/0
      Summarizing with metric 2169856
    15.0.0.0/8 for FastEthernet0/0, Serial0/1
      Summarizing with metric 2169856
    192.168.10.0/24 for Serial0/0, Serial0/1
      Summarizing with metric 28160
  Maximum path: 4
  Routing for Networks:
     12.1.116.236/30
     15.0.230.244/30
     192.168.10.192/27
  Routing Information Sources:
    Gateway         Distance      Last Update
    12.1.116.238    90            7821
    15.0.230.246    90            8202
  Distance: internal 90 external 170
show ip route
The routing table confirms that R-1 has learned routes to other networks through EIGRP, such as 13.0.0.0/8 and 14.0.0.0/8. The "D" code indicates these are EIGRP routes.

Codes: C - connected, S - static, I - IGRP, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route

Gateway of last resort is not set

     12.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
D        12.0.0.0/8 is a summary, 00:10:01, Null0
C        12.1.116.236/30 is directly connected, Serial0/1
D    13.0.0.0/8 [90/2681856] via 12.1.116.238, 00:09:54, Serial0/1
D    14.0.0.0/8 [90/2681856] via 15.0.230.246, 00:09:53, Serial0/0
     15.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
D        15.0.0.0/8 is a summary, 00:10:01, Null0
C        15.0.230.244/30 is directly connected, Serial0/0
     192.168.10.0/24 is variably subnetted, 2 subnets, 2 masks
D        192.168.10.0/24 is a summary, 00:10:01, Null0
C        192.168.10.192/27 is directly connected, FastEthernet0/0
D    192.168.20.0/24 [90/2684416] via 12.1.116.238, 00:09:54, Serial0/1
                     [90/2684416] via 15.0.230.246, 00:09:52, Serial0/0 R-1(config)#
show ip interface brief
This output confirms that all relevant interfaces are correctly configured with IP addresses and are up.

Interface              IP-Address      OK? Method Status                Protocol
FastEthernet0/0        192.168.10.193  YES manual up                    up
FastEthernet0/1        unassigned      YES unset  administratively down down
Serial0/0              15.0.230.245    YES manual up                    up
Serial0/1              12.1.116.237    YES manual up                    up
