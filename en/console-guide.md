## Network > VPC > Console Guide

This document describes what is required to deal with VPCs in the console.

## VPC

AS a VPC may have many subnets, a sufficiently large network must be configured when divided subnets are used. VPC network can be described by using [CIDR Notation](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing).  All VPCs must be located in the three address areas as below, to be able to configure a [Private Network](https://en.wikipedia.org/wiki/Private_network) and link-local addresses cannot be used. Furthermore, the network area must be larger than 24bit-256 at the least. 

### Private Network 

RFC1918 | IP Address Area | Available Number of Addresses 
-------- | ---------- | -----------------
24bit block | 10.0.0.0/8 | 16,777,216
20bit block | 172.16.0.0/12 | 1,047,576
16bit block | 192.168.0.0/16 | 65,536

### Link-Local Addresses 

Cannot use 65,536 IP addresses that are included to 169.254.0.0/16. 

### Examples 

Example | Availability 
-------- | ---------- 
10.0.0.0/8 | Available. 
10.0.0.0/16 | Available. 
10.0.0.0/24 | Available. 
10.0.0.0/28 | Unavailable. Range is too short. 
172.16.0.0/16 | Available. 
172.16.0.0/8 | Unavailable. Out of available range. 
192.168.0.0/16 | Available. Specified as default range. 
192.168.0.0/24 | Available. 
192.253.0.0/24 | Unavailable. Out of available range. 

<br>


By using initial compute and network products, items as below are to be automatically configured. 

Item | Name | Summary 
-------- | ---------- | --------------
VPC | Default Network | Creates one VPC within the range of 192.168.0.0/16. 
Subnet | Default Network | Creates one subnet within the range of 192.168.0.0/24. 
Routing Table | vpc-[id] | Creates one routing table named after a part of VPC ID. 
Internet Gateway | ig-[id] | Creates one internet gateway named after a part of routing table ID. 
Security Group | default | Creates a security group named default. 

To add VPC, not as an initial configuration, configure items as below: 

Item | Name | Summary 
-------- | ---------- | --------------
VPC | As specified | Creates one VPC within the specified range. 
Subnet | - | Not created. 
Routing Table | vpc-[id] | Creates one routing table named after a part of VPC ID. 
Internet Gateway | - | Not created: create additionally and connect. 
Security Group | - | Not created additionally. 

Here is the quota for VPC and each item.  

Item | Max 
-------- | ---------- 
VPC | 3
Subnet | 10 per VPC 
Internet Gateway | 3 
Floating IP | Unlimited 
Routing Table | 10 per VPC 
Route | 10 per Routing Table 
Peering | Unlimited 



> [Note]
VPCs can be deleted only when subnets are deleted altogether, and in such case, delete along with subnets, routing tables, and internet gateways. 

* VPC is safe from traffic as it is completed isolated from other VPCs .

* Direct access of VPC from the internet is unavailable as it is a private network.  

* All elements within VPC cannot use VLAN. 

* For those traffic beyond  a region, local communication is not provided.  

* All instances within a VPC cannot access the internet without internet gateway. 

* Excessively transferred "Broadcast, Multicast, Unknown Unicast" may be blocked without prior notice. 


## Subnets 

A  VPC can be subdivided into subnets that are composed of many networks. However, a subnet must be included within the range of a VPC address, with its length the same or shorter.  For example, in the case of 192.168.0.0/16, a total of 65536 IP addresses are available between 192.168.0.0 and 192.168.255.255. In addition, the smallest subnet is 28 bits and any configuration cannot be smaller than that. Subnets also adopt CIDR display, just like VPC.  

When a subnet is created, its gateway IP address is automatically designated and it cannot be modified. It is also automatically registered to a routing table included to the VPC. 

> [Note]
To delete a subnet, it should be empty with no instances or load balancers included. Also, make sure that the routing table which the subnet is connected to does not have  routes to that subnet.

* Assigns one IP address to a newly-created instance from a designated subnet (called a fixed IP).

* Applies the IP address to the instance when it is booted, via DHCP.  

* Cannot modify the address range of a subnet. 

* Cannot redundantly create or overlap the range for different subnets within a same VPC.

* The range of subnets may be redundant or wrapped over for different VPCs. 

* MAC addresses that are not assigned to an instance may be blocked from the network; VPN service running on instances may not operate. 

* When many subnets are connected to an instance, an appropriate routing setting is required within OS of the instance. 

* Two subnets within a same VPC are not completely isolated; apply security groups to protect instances. 

* Subnets support local communication through different areas of availability. Local communication shall not be charged.  


## Internet Gateway

An internet gateway can be connected to a routing table. VPCs comprised of private networks cannot be connected externally, and may access the internet through internet gateway. Each instance, to be connected to an internet, must set "default gateway" as the gateway address of the subnet, and TOAST does the job automatically. To create an internet gateway, an external network is required and TOAST is operating only one "public_network" as of now.  

* An internet gateway address is automatically assigned when an instance is created or VPC requires an internet access, and it cannot be modified. 

* Cannot access an instance with an internet gateway address. 

* Blocks all traffic inflow via internet gateway address.

* Charges by usage, when an instance connected to the internet triggers traffic to the internet direction.

* Do not charge for local communication between instances. 

### Guide for Restarting Internet Gateways for Server Maintenance 

TOAST updates software of the Internet gateway server on a regular basis to enhance security and stability of its infrastructure services. 

Internet gateways that are running on a target server for maintenance must be restarted and migrated to the server which is completed with maintenance.  

To restart Internet gateways, use the **! Restart** button which is created next to each Internet gateway name. 

Go to the project where your Internet gateway specified as maintenance target is located. 

1. Any Internet gateway that has the **! Restart** button before its name requires maintenance. 
    <image-001.png>
    Put the mouse cursor on the **! Restart** button to find maintenance schedule details. 
    <image-002.png>

2. Select a target Internet gateway and click the **! Restart** button next to its name. 
    It is advised to perform maintenance during time when impact on service is limited, since instances of the Internet gateway are disconnected from the Internet until restarting is completed. 
    However, instances that are connected to floating IPs are not influenced by restarting of Internet gateways. 

3. Click **OK** onto the window asking of restarting Internet gateway. 
    <image-003.png>

4. Wait until the Internet gateway status turns green and the **! Restart** button disappears.
    If the status does not change or the **! Restart** button remains, press 'Refresh'.  
    <image-004.png>

The Internet gateway becomes inoperable while restarting is underway.
Unless restarting Internet gateway is normally completed, it shall be automatically reported to the administrator, and you'll be contacted by TOAST.  

## Floating IP 

When an instance is created, an IP address is assigned from a designated subnet. The IP address is certainly included to VPC, as it is the subnet address, and we call it Fixed IP. As it is a private network address, an access from the internet is not available. Therefore, to access the internet directly from outside, an accessible IP address from outside is required. Floating IP allows direct access to an instance from the internet, by linking the address to the instance 1:1. For more details, refer to [Overview]. To create a floating IP, an external network must be selected and TOAST currently operates only one "public network".   

> [Note] To connect a floating IP to an instance, a subnet including the instance must be connected to a routing table,  <br>
> and the routing table must be connected to the internet through an internet gateway, so as to perform "connection". 

* Charges for a floating IP begin from the moment it is created, and charging shall continue before it is deleted (which is irrelevant to instance connection).  

* A connection to a floating IP does not mean a change of a fixed IP to a floating IP. 

* Traffic occurred to the internet direction shall be charged. 

* Two instances, within a same VPC, communicating via floating IP shall be charged by the usage.  

> [Note] Two instances, within a same VPC, communicating locally via fixed IP shall not be charged. 

### 여러 개의 네트워크 인터페이스를 갖는 인스턴스에 플로팅 IP 연결하기

여러 개의 네트워크 인터페이스를 갖는 인스턴스는 각 네트워크 인터페이스마다 플로팅 IP를 연결할 수 있습니다. 그러나 첫 번째를 제외한 나머지에 네트워크 인터페이스에 연결한 플로팅 IP로 인스턴스에 접속하기 위해서는 인스턴스의 Routing Rule 설정이 필요합니다.

**TOAST에서 제공하는 공용 Linux 이미지 배포 버전 `2018.12.27` 이상**으로 생성한 인스턴스는 부팅 시 Routing Rule을 자동으로 설정하여 각각의 네트워크 인터페이스에 연결된 모든 플로팅 IP를 통해 접근이 가능합니다. 
* 인스턴스에 접속 후, 다음과 같이 Routing Rule 설정 여부를 확인할 수 있습니다.
    ```shell
    $ ip rule
    0:      from all lookup local
    100:    from { eth0의 IP 주소 } lookup 1
    200:    from { eth1의 IP 주소 } lookup 2
    300:    from { eth2의 IP 주소 } lookup 3
    ...
    32766:  from all lookup main
    32767:  from all lookup default
    ```
    위와 같이 `ip rule` 명령을 실행했을 때 각 네트워크 인터페이스 별 Routing Rule 설정이 되어 있다면 모든 플로팅 IP를 통해 인스턴스에 접근이 가능합니다.

이 외의 이미지로 생성한 인스턴스는, 다음과 같이 인스턴스 내에 Routing Rule을 설정하여 인스턴스에 연결된 모든 플로팅 IP를 통해 접근하도록 할 수 있습니다.

* 첫 번째 네트워크 인터페이스(eth0)에 연결된 플로팅 IP를 통해 인스턴스에 접속한 후, 플로팅 IP를 연결하여 접속하려는 나머지 네트워크 인터페이스들에 대해 다음과 같은 명령을 실행합니다.
  ```shell
  ip rule add from {네트워크 인터페이스 IP 주소}/32 table {테이블 번호} priority {우선순위}
  ip route add default via {네트워크 인터페이스의 Default 게이트웨이 주소} table {테이블 번호}
  ip route add {네트워크 인터페이스의 서브넷 CIDR} dev {네트워크 인터페이스 이름} table {테이블 번호}
  ```

	* 예로, 인스턴스가 갖는 네트워크 인터페이스 정보가 다음과 같다고 할 때
      ```shell
      1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN
          link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
          inet 127.0.0.1/8 scope host lo
             valid_lft forever preferred_lft forever
          inet6 ::1/128 scope host
             valid_lft forever preferred_lft forever
      2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1454 qdisc pfifo_fast state UP qlen 1000
          link/ether fa:16:3e:8d:71:d6 brd ff:ff:ff:ff:ff:ff
          inet 192.168.100.132/24 brd 192.168.100.255 scope global dynamic eth0
             valid_lft 86379sec preferred_lft 86379sec
          inet6 fe80::f816:3eff:fe8d:71d6/64 scope link
             valid_lft forever preferred_lft forever
      3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1454 qdisc pfifo_fast state UP qlen 1000
          link/ether fa:16:3e:06:96:2f brd ff:ff:ff:ff:ff:ff
          inet 172.16.0.37/24 brd 172.16.0.255 scope global dynamic eth1
             valid_lft 86381sec preferred_lft 86381sec
          inet6 fe80::f816:3eff:fe06:962f/64 scope link
             valid_lft forever preferred_lft forever
      4: eth2: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1454 qdisc pfifo_fast state UP qlen 1000
          link/ether fa:16:3e:06:ac:10 brd ff:ff:ff:ff:ff:ff
          inet 10.254.0.90/24 brd 10.254.0.255 scope global dynamic eth2
             valid_lft 86386sec preferred_lft 86386sec
          inet6 fe80::f816:3eff:fe06:ac10/64 scope link
             valid_lft forever preferred_lft forever
      ```
        `eth1`, `eth2` 에 대해 플로팅 IP로 접속하기 위해 아래와 같은 명령을 통해 Routing Rule을 설정합니다.

      ```shell
      # eth1의 플로팅 IP 접속을 위한 Routing Rule 설정
      ip rule add from 172.16.0.37/32 table 2 priority 200
      ip route add default via 172.16.0.1 table 2
      ip route add 172.16.0.0/24 dev eth1 table 2

      # eth2의 플로팅 IP 접속을 위한 Routing Rule 설정
      ip rule add from 10.254.0.90/32 table 3 priority 300
      ip route add default via 10.254.0.1 table 3
      ip route add 10.254.0.0/24 dev eth2 table 3
      ```
      명령 실행 후 다음과 같이 설정된 Routing Rule을 확인할 수 있습니다.

      ```shell 
      $ ip rule													
      0:	from all lookup local
      200:	from 172.16.0.37 lookup 2 	
      300:	from 10.254.0.90 lookup 3 	
      32766:	from all lookup main
      32767:	from all lookup default

      $ ip route show table 2					
      default via 172.16.0.1 dev eth1
      172.16.0.0/24 dev eth1  scope link

      $ ip route show table 3
      default via 10.254.0.1 dev eth2
      10.254.0.0/24 dev eth2  scope link
      ```

* 위 Routing Rule 설정은 인스턴스를 재부팅하면 초기화 되므로, 인스턴스 재부팅 시 Routing Rule이 자동으로 설정되도록 설정하는 것이 좋습니다.

## Security Group 

Security group is used to protect instances from other traffic. In general, "Positive Security Model" is applied to allow specified traffic, while blocking the others. 

With a service initiated, a default security group is created, blocking all inbound traffic. As a result, services like 'ping' or 'ssh' become available only by setting rules that are required. Same can be applied both to external access using floating IP and internal access using private IP. 

Many security groups can be set for an instance. When additional security group is created, with many rules added and set for an instance, all rules set for security groups shall be applied to the instance.   

For instance, if a security group called 'CONN' has rules named 'Ingress TCP PORT 22' and 'Ingress TCP PORT 23', while a security group called 'WEB' has rules named 'Ingress TCP PORT 80' and 'Ingress TCP PORT 8080', the two security groups, 'CONN' and 'WEB', can be assigned to an instance and make the four rules all available.    


| Item        | Description                                                  |
| ----------- | ------------------------------------------------------------ |
| Flow        | Ingress refers to an inbound flow to an instance. Egress refers to an outbound flow from an instance. |
| Ether Type  | Version of EtherType IP. IPv4 or IPv6 can be specified.      |
| IP Protocol | A particular protocol or all can be specified.               |
| Port Range  | For L4 protocol, the port range can be specified.            |
| Remote      | Range of a security group or IP address can be specified. If the flow of a rule is 'Egress', the destination is remote. <br>If it is 'Ingress', the departure is remote. Traffic departure and destination is compared depending on the flow of a rule. <br>When a security group is specified, see if the IP belongs to instances of the specified security group. <br>When IP address or range is specified by selecting CIDR, see if the IP address or range is set. |

Since a security group runs by 'stateful', sessions that are once connected with a rule are allowed even without rule of the opposite flow. 

For example, if the first packet of TCP 80 which directs toward an instance, has passed by the 'Ingress TCP PORT 80' rule, the transfer packet which starts from TCP 80 port of an instance shall not be blocked.  

Nevertheless, if a session is closed due to failure of incoming packet which is appropriate for a particular period rule, packets of the opposite direction are also blocked.   

Default security group has rules for all outbound traffic from an instance. Unless the rules are removed, all sessions starting from instances are allowed. 

* Specifying a range is more effective than adding rules one by one.

* Adding more rules may degrade performance. 

* Traffic under inappropriate session status may be blocked. 

* Asymmetric traffic that has different inflow from outflow may be blocked. 

* Rules not on the list may be defined to use. For more details, refer to [Well-known port](https://en.wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers). 




## Routing Table 

A routing table is created along with VPC, and is also deleted along with a deletion of VPC. Many of them can be created within a VPC, and they may be deleted if not a default routing table. Subnets must be connected to at least one routing table, and multiple routing tables cannot share an internet gateway.   

When a list of routing table is designated, summary information is displayed on the screen of details, and more routes may be added by using "Routes". 

> [Note]
Adding more routes is available, only when available areas within VPC are designated. Otherwise, message will show failure. 

* Gateways of subnets included to a routing table are automatically added.  

* “Default routing table” cannot be deleted. It shall be deleted along with VPC on its deletion.

* Gateways of subnets and internet cannot be deleted from the list of routers. 

* Disconnecting a routing table from the internet gateway will disconnect the internet. 



## Peering 

Peering refers to connecting two different VPCs. In general, VPCs cannot communicate as their network areas are different, and may be linked via floating IPs but charged further depending on the network usage. Hence, peering is provided to communicate two VPCs 

> [Note] Peering connects two different VPCs; does not support a connection to a third VPC through another VPC. That is, for A <-> B <-> C, A and C cannot be linked.  

* Cannot use overlapped IP address areas of two VPCs.<br>When IP address areas of two VPCs to create peering are redundant, peering fails. 
* Size of an IP address area does not matter and communication with unlinked subnets to "default routing table" is unavailable. 
* Peering is charged from the moment it is created. 
* There's no quota limits for a peering, but a subnet quota is required. 
