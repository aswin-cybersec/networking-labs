# Lab 02 — DHCP Lab

## Objective
Understand how DHCP assigns IP addresses automatically,
simulate DHCP failure, and restore connectivity.

## Environment
- VM: Windows 10 (VirtualBox)
- Network: VirtualBox NAT Network (10.0.2.0/24)
- DHCP Server: 10.0.2.2 (VirtualBox built-in)

## Tools Used
- Command Prompt (Admin)
- ipconfig /release
- ipconfig /renew
- ipconfig /all
- Network Adapter Settings (TCP/IPv4)

## DORA Process
| Step | Name | What Happens |
|------|------|-------------|
| D | Discover | Device broadcasts "I need an IP address" |
| O | Offer | DHCP server replies "I offer you 10.0.2.3" |
| R | Request | Device says "I accept 10.0.2.3" |
| A | Acknowledge | Server confirms "10.0.2.3 is yours" |

## Steps Performed
| Step | Command/Tool | Finding |
|------|-------------|---------|
| 1 | ipconfig /all | DHCP active, lease 10 mins, server 10.0.2.2 |
| 2 | ipconfig /release | IP released, APIPA 169.254.181.88 assigned |
| 3 | ipconfig /renew | New IP 10.0.2.3 obtained from DHCP |
| 4 | Static IP set manually | DHCP disabled, IP 10.0.2.50 configured |
| 5 | IP conflict occurred | DHCP conflict error — IP already in use |
| 6 | VM restart | Conflict cleared, DHCP restored |

## Key Findings
- APIPA (169.254.x.x) appears when DHCP unreachable
- Static IP in DHCP range causes IP conflict
- VirtualBox DHCP gives 10-15 minute leases
- IP conflict fix: release, renew, or restart machine

## Lessons Learned
- 169.254.x.x always means DHCP failure
- DORA process happens on every network connection
- Never set static IP inside DHCP scope range
- ipconfig /release and /renew is first fix for DHCP issues
