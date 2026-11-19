# Wireshark Display Filter Reference

Use these in Wireshark's display-filter bar. Display filters change which packets are shown; they do not remove packets from the capture.

| Filter | Purpose |
|---|---|
| `dns` | Show DNS packets |
| `dns.qry.name contains "spotify"` | Show DNS queries whose name contains `spotify` |
| `tcp` | Show TCP packets |
| `udp` | Show UDP packets |
| `icmp` | Show ICMP packets |
| `ip.addr == 1.1.1.1` | Show IPv4 packets to or from `1.1.1.1` |
| `tcp.port == 443` | Show TCP packets with source or destination port 443 |
| `tcp.flags.syn == 1 && tcp.flags.ack == 0` | Show initial TCP SYN packets |
| `tcp.analysis.retransmission` | Show packets Wireshark identifies as TCP retransmissions |
| `tcp.flags.reset == 1` | Show TCP reset packets |
| `tls` | Show packets dissected as TLS |

## Notes

- A DNS query filter only shows names visible in DNS packets. Cached DNS entries or encrypted DNS can limit visibility.
- TLS application data is encrypted. A packet capture normally cannot reveal Spotify credentials, account details, or audio content.
- A SYN packet is a connection attempt, not proof of a successful connection.
- A retransmission is an indicator to investigate, not proof of an attack.
