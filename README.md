
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
