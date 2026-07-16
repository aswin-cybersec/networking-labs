# Lab 03 — Network Troubleshooting

## Objective
Use advanced command-line tools to trace network paths,
analyze active connections, inspect ARP tables, and
review routing logic.

## Environment
- VM: Windows 10 (VirtualBox)
- Network: VirtualBox NAT Network (10.0.2.0/24)

## Tools Used
- tracert
- netstat
- netstat -ano
- tasklist
- arp -a
- route print

## Steps Performed
| Step | Command | Finding |
|------|---------|---------|
| 1 | tracert google.com | Single hop due to VirtualBox NAT |
| 2 | tracert invalid domain | DNS resolution failure detected |
| 3 | netstat | 20+ active HTTPS connections found |
| 4 | netstat -ano | Mapped connections to PIDs |
| 5 | tasklist findstr PID | Identified msedgewebview2.exe |
| 6 | arp -a | Mapped IP-to-MAC for gateway and DHCP |
| 7 | route print | Reviewed local and default routing table |

## Key Findings
- tracert fails immediately if DNS cannot resolve domain
- CLOSE_WAIT connections indicate app not closing sockets
- ARP table can detect spoofing — watch gateway MAC changes
- route print confirms gateway routing for non-local traffic

## Lessons Learned
- netstat -ano + tasklist = standard suspicious process workflow
- tracert "unable to resolve" = DNS problem not network problem
- ARP spoofing detection starts with knowing gateway MAC
- This workflow is foundational for SOC malware detection
