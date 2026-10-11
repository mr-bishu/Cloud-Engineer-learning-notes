# Networking --- All-Day Study Notes

**Track:** Beginner Networking for AWS Cloud Engineering\
**Date:** 11 October 2026

## 1. What Is Computer Networking?

A computer network is a group of devices connected to exchange data and
share resources.

### Common network types

-   **PAN:** Personal Area Network; very short range.
-   **LAN:** Local Area Network; home, lab, or office.
-   **WLAN:** Wireless LAN using Wi-Fi.
-   **MAN:** Metropolitan Area Network; city-scale.
-   **WAN:** Wide Area Network; large geographic areas. The Internet is
    the best-known example.

### Basic terms

-   **Client:** Requests a service.
-   **Server:** Provides a service.
-   **Packet:** A unit of data sent across a network.
-   **Bandwidth:** Maximum data-carrying capacity.
-   **Latency:** Time taken for data to travel.
-   **Throughput:** Actual successful data transfer rate.
-   **Protocol:** Rules devices follow to communicate.

## 2. The OSI Model

Remember: **Please Do Not Throw Sausage Pizza Away**.

  -----------------------------------------------------------------------
  Layer             Name              Main purpose      Examples
  ----------------- ----------------- ----------------- -----------------
  7                 Application       Network services  HTTP, DNS, SMTP
                                      used by           
                                      applications      

  6                 Presentation      Data format,      TLS-related
                                      encryption,       presentation
                                      compression       functions

  5                 Session           Starts, manages,  Session control
                                      ends sessions     

  4                 Transport         End-to-end        TCP, UDP
                                      delivery and      
                                      ports             

  3                 Network           Logical           IPv4, IPv6,
                                      addressing and    routers
                                      routing           

  2                 Data Link         Frames and local  Ethernet, MAC,
                                      network delivery  switches

  1                 Physical          Signals, cables,  Fiber, copper,
                                      radio bits        radio
  -----------------------------------------------------------------------

The practical TCP/IP model is often described using four layers: Link,
Internet, Transport, and Application.

## 3. TCP vs UDP

-   **TCP:** Connection-oriented and reliable; uses acknowledgements and
    retransmission. Common for HTTPS, SSH, and many file transfers.
-   **UDP:** Connectionless with lower overhead; delivery is not
    guaranteed by UDP itself. Common for DNS queries, voice/video, and
    gaming.

## 4. IP Addresses

An IP address identifies a network interface logically.

-   **IPv4:** 32 bits, written as four decimal octets,
    e.g. `192.168.1.10`.
-   **IPv6:** 128 bits, written in hexadecimal groups,
    e.g. `2001:db8::10`.
-   **Private IPv4 ranges:**
    -   `10.0.0.0/8`
    -   `172.16.0.0/12` (172.16.0.0--172.31.255.255)
    -   `192.168.0.0/16`
-   **Loopback:** `127.0.0.1` is a common IPv4 loopback address.
-   **Link-local IPv4:** `169.254.0.0/16`.
-   **Public IP:** Routable on the public Internet, subject to routing
    and security controls.

## 5. Subnet Masks and CIDR

CIDR means **Classless Inter-Domain Routing**. `/24` means the first 24
bits are the network prefix.

Examples: - `192.168.1.0/24` → mask `255.255.255.0` - `10.0.0.0/16` →
mask `255.255.0.0` - `172.16.0.0/20` → mask `255.255.240.0` -
`192.168.1.64/26` → mask `255.255.255.192`

For IPv4: - Total addresses = `2^(32 − prefix length)`. - In a typical
subnet, usable host addresses = total addresses − 2 (network and
broadcast). - Special-purpose subnets and cloud providers can have
different usable-address rules.

### Common CIDR sizes

  -----------------------------------------------------------------------
  CIDR                   Total IPv4     Typical usable      Block size in
                          addresses              hosts      final octet\*
  -------------- ------------------ ------------------ ------------------
  /24                           256                254                256

  /25                           128                126                128

  /26                            64                 62                 64

  /27                            32                 30                 32

  /28                            16                 14                 16

  /29                             8                  6                  8

  /30                             4                  2                  4
  -----------------------------------------------------------------------

\*The block-size column applies when the subnet boundary is in the last
octet. For larger prefixes such as `/20`, the boundary is in an earlier
octet.

### Quick block-size method

When the subnet mask boundary is in an octet: **Block size = 256 − mask
value in that octet.**

Example: `/26` has mask `255.255.255.192`, so block size =
`256 − 192 = 64`. The subnet network starts in multiples of 64: `.0`,
`.64`, `.128`, `.192`.

For `192.168.1.70/26`: - Network address: `192.168.1.64` - Broadcast
address: `192.168.1.127` - Typical usable range:
`192.168.1.65–192.168.1.126` - Total addresses: 64; typical usable
hosts: 62

For `10.0.5.130/25`: - `/25` block size = 128 in the last octet. -
Network: `10.0.5.128` - Broadcast: `10.0.5.255` - Typical usable range:
`10.0.5.129–10.0.5.254`

## 6. Network, Host, and Broadcast Addresses

For a typical IPv4 subnet: - **Network address:** Identifies the subnet;
host bits are all zero. - **Broadcast address:** Sends to all hosts in
that IPv4 subnet; host bits are all one. - **Usable host addresses:**
Usually between network and broadcast addresses.

Example: `192.168.10.0/24` - Network: `192.168.10.0` - First typical
host: `192.168.10.1` - Last typical host: `192.168.10.254` - Broadcast:
`192.168.10.255`

## 7. AWS VPC and Subnets

-   **Amazon VPC:** A logically isolated virtual network in AWS.
-   **VPC CIDR:** Address range assigned to the VPC.
-   **Subnet:** A smaller IP range inside a VPC, associated with one
    Availability Zone.
-   **Route table:** Rules that determine where network traffic goes.
-   **Internet Gateway (IGW):** Enables eligible resources with public
    IPs and suitable routes/security rules to communicate with the
    Internet.
-   **NAT Gateway:** Lets resources in private subnets initiate
    supported outbound IPv4 connections without allowing unsolicited
    inbound Internet connections.
-   **Security Group:** Stateful virtual firewall associated with
    network interfaces/resources.
-   **Network ACL (NACL):** Stateless subnet-level traffic filter.
-   **VPC Peering / Transit Gateway:** Options for connecting VPC
    networks.

### AWS subnet address reservation

AWS reserves **five IPv4 addresses in each subnet**: 1. Network address
2. VPC router 3. DNS 4. Reserved for future use 5. Last address in the
subnet range

Therefore, for an AWS IPv4 subnet: **AWS usable addresses = total
addresses − 5.**

Examples: - `/24`: 256 total, 251 AWS-usable addresses. - `/26`: 64
total, 59 AWS-usable addresses. - `/28`: 16 total, 11 AWS-usable
addresses.

AWS has service-specific minimums and special cases; check current AWS
documentation before designing production networks.

### Public vs private subnet

A subnet is commonly called **public** when its route table has a route
to an Internet Gateway. A subnet is commonly called **private** when it
does not route directly to an Internet Gateway. Public IP assignment and
security rules also matter; a route alone does not make every resource
reachable.

Typical design: - Public subnet: load balancer or bastion host, where
needed. - Private application subnet: application servers. - Private
data subnet: databases with no direct public exposure.

## 8. Ports and Common Protocols

     Port Protocol/service   Typical use
  ------- ------------------ ------------------------------
    20/21 FTP                Legacy file transfer
       22 SSH                Secure remote administration
       25 SMTP               Mail transfer
       53 DNS                Name resolution over UDP/TCP
    67/68 DHCP               IPv4 address configuration
       80 HTTP               Web traffic without TLS
      123 NTP                Time synchronisation
      143 IMAP               Email access
      443 HTTPS              Web traffic protected by TLS
     3389 RDP                Windows remote desktop

Open only the ports required. Avoid exposing administrative ports such
as SSH or RDP to the entire Internet.

## 9. DNS, DHCP, ARP, and NAT

-   **DNS:** Resolves domain names such as `example.com` to IP
    addresses.
-   **DHCP:** Automatically provides IP configuration, including
    address, mask, gateway, and DNS.
-   **ARP:** Resolves an IPv4 address to a MAC address on a local
    network.
-   **NAT:** Translates IP addresses; commonly used to share public IPv4
    connectivity.

## 10. Switching and Routing

-   **Switch:** Forwards Ethernet frames within a LAN using MAC
    addresses.
-   **Router:** Forwards packets between IP networks using routing
    tables.
-   **Default gateway:** Router/interface used when the destination is
    outside the local subnet.
-   **Static route:** Manually configured route.
-   **Dynamic routing:** Routers learn routes using protocols such as
    OSPF or BGP.

## 11. Troubleshooting Commands

Linux:

``` bash
ip addr
ip route
ping 8.8.8.8
ping example.com
traceroute example.com
tracepath example.com
dig example.com
nslookup example.com
ss -tulpen
curl -I https://example.com
```

Windows:

``` powershell
ipconfig /all
ping 8.8.8.8
tracert example.com
nslookup example.com
netstat -ano
```

Troubleshooting order: 1. Check link/Wi-Fi and interface status. 2.
Check IP address and subnet mask. 3. Check default gateway and routing.
4. Ping the gateway, if ICMP is allowed. 5. Test an external IP address.
6. Test DNS using a hostname. 7. Check firewall rules, security groups,
NACLs, and service ports. 8. Review logs and test the application.

A failed ping alone does not prove a host is down; ICMP may be blocked.

## 12. Beginner AWS Networking Checklist

-   [ ] Explain IP address, subnet mask, gateway, DNS, TCP, and UDP.
-   [ ] Convert `/24`, `/25`, `/26`, `/27`, and `/28` to address counts.
-   [ ] Find network and broadcast addresses.
-   [ ] Calculate typical usable ranges.
-   [ ] Remember AWS reserves five IPv4 addresses per subnet.
-   [ ] Explain VPC, subnet, route table, IGW, NAT Gateway, SG, and
    NACL.
-   [ ] Explain public vs private subnet routing.
-   [ ] Troubleshoot using `ip`, `ping`, `traceroute`, `dig`, and
    `curl`.

## 13. Practice Questions

Try answering before checking the answer key.

1.  How many total IPv4 addresses are in a `/27`?
2.  What is the block size for `/26` when the boundary is in the last
    octet?
3.  Find the network and broadcast addresses for `192.168.1.100/26`.
4.  Find the typical usable range for `10.0.0.0/25`.
5.  How many AWS-usable IPv4 addresses are in a `/26` subnet?
6.  Which AWS component provides a route between a VPC and the public
    Internet?
7.  Which is stateful: a Security Group or a Network ACL?
8.  Which command checks DNS resolution on Linux?

### Answer key

1.  32
2.  64
3.  Network `192.168.1.64`; broadcast `192.168.1.127`
4.  `10.0.0.1–10.0.0.126`
5.  59
6.  Internet Gateway (with suitable route and other configuration)
7.  Security Group
8.  `dig` or `nslookup`

## 14. A Reliable Study Routine

-   **Hour 1:** Networking basics, OSI/TCP-IP, TCP vs UDP.
-   **Hour 2:** IPv4, private ranges, CIDR, subnet masks.
-   **Hour 3:** Block sizes, network/broadcast addresses, usable ranges.
-   **Hour 4:** AWS VPCs, subnets, route tables, IGW, NAT Gateway.
-   **Hour 5:** Security Groups, NACLs, ports, DNS, DHCP.
-   **Hour 6:** Practice commands and troubleshoot sample scenarios.
-   **Hour 7:** Answer the practice questions and explain each answer
    aloud.

**Tip:** Do subnet calculations by hand first. Then verify them with a
subnet calculator. In AWS, always check both the CIDR range and AWS's
five reserved IPv4 addresses.
