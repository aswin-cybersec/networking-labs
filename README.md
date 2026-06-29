
## Lab 1 - DNS Troubleshooting

### Objective
Understand DNS resolution, simulate a DNS failure, 
and fix it using Windows built-in tools.

### Environment
- VM: Windows 10 (VirtualBox)
- Network: VirtualBox NAT Network (10.0.2.0/24)
- DNS Tested: ISP DNS (103.199.160.80) and Google DNS (8.8.8.8)

### Tools Used
- Command Prompt
- nslookup
- ipconfig /flushdns
- ipconfig /displaydns
- Network Adapter Settings (TCP/IPv4)

### Steps Performed

| Step | Command/Tool | Finding |
|------|-------------|---------|
| 1 | ipconfig /all | DNS: 103.199.160.80 (ISP DNS) |
| 2 | nslookup google.com | DNS resolving correctly |
| 3 | nslookup youtube/facebook/8.8.8.8 | All resolved + reverse lookup confirmed |
| 4 | Set fake DNS (100.100.100.1) | DNS failure simulated — request timed out |
| 5 | Set Google DNS (8.8.8.8) | DNS restored — google.com resolved |
| 6 | ipconfig /flushdns | DNS cache cleared successfully |
| 7 | ipconfig /displaydns | Cache verified empty after flush |

### Key Findings
- Fake DNS IP causes complete resolution failure
- ISP DNS and Google DNS return different IPs for same domain
  — this is normal (load balancing)
- DNS failure looks like internet failure to end users
- /flushdns fixes stale cache issues after DNS changes

### Lessons Learned
- Always run nslookup first when user says internet is not working
- DNS failure ≠ internet failure — ping 8.8.8.8 to confirm connectivity
- Google DNS (8.8.8.8 / 8.8.4.4) is the standard manual fix
- ipconfig /flushdns resolves cached record issues instantly

## Lab 2 - DHCP Lab

### Objective
Understand how DHCP assigns IP addresses automatically,
simulate DHCP failure, and restore connectivity using
ipconfig commands.

### Environment
- VM: Windows 10 (VirtualBox)
- Network: VirtualBox NAT Network (10.0.2.0/24)
- DHCP Server: 10.0.2.2 (VirtualBox built-in)

### Tools Used
- Command Prompt (Admin)
- ipconfig /release
- ipconfig /renew
- ipconfig /all
- Network Adapter Settings (TCP/IPv4)

### DORA Process
| Step | Name | What happens |
|------|------|-------------|
| D | Discover | Device broadcasts "I need an IP address" |
| O | Offer | DHCP server replies "I offer you 10.0.2.3" |
| R | Request | Device says "I accept 10.0.2.3" |
| A | Acknowledge | Server confirms "10.0.2.3 is yours" |

### Steps Performed
| Step | Command/Tool | Finding |
|------|-------------|---------|
| 1 | ipconfig /all | DHCP active, lease 10 mins, server 10.0.2.2 |
| 2 | ipconfig /release | IP released, APIPA 169.254.181.88 assigned |
| 3 | ipconfig /renew | New IP 10.0.2.3 obtained from DHCP |
| 4 | Static IP set manually | DHCP disabled, IP 10.0.2.50 configured |
| 5 | IP conflict occurred | DHCP conflict error — IP already in use |
| 6 | VM restart | Conflict cleared, DHCP restored |

### Key Findings
- APIPA (169.254.x.x) appears when DHCP server
  is unreachable
- Static IP in DHCP range causes IP conflict
- VirtualBox DHCP gives 10-15 minute leases
- IP conflict fix: release, renew, or restart machine

### Lessons Learned
- 169.254.x.x always means DHCP failure
- DORA process happens automatically on every
  network connection
- Never set static IP inside DHCP scope range
- ipconfig /release and /renew is first fix for
  any DHCP related complaint
