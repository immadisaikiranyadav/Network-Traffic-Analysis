# Evidence Notes

## Confirmed from supplied screenshots

- `network-traffic.pcap` was inspected with `capinfos` and tcpdump.
- One metadata view reports 450 packets over approximately 51.14 seconds.
- Another metadata view reports 1,653 packets over approximately 82.95 seconds.
- DNS queries include `gae2-spclient.spotify.com`.
- A DNS response includes a CNAME under `edge-web-gae2.dual-gslb.spotify.com`.
- Other DNS names are also visible, including Google-related names.
- ICMP echo requests and replies between `10.16.108.15` and `1.1.1.1` are visible.
- The ping command output reports four replies and 0% packet loss.
- TCP SYN attempts are visible toward `10.10.10.24:9997` and `192.168.1.105:1514`.
- Wireshark marks multiple TCP packets as retransmissions.

## Not established by the screenshots

- Whether the TCP attempts were authorized or unexpected.
- Why the TCP SYN packets were retransmitted.
- Whether the destinations were running services on the displayed ports.
- Whether any malicious activity occurred.
- The complete contents of the capture, because the original PCAP was not supplied.

## Handling note

The private IP addresses shown are retained because they are part of the lab evidence. Before publishing, ensure they do not identify a sensitive real-world internal network. If they are from a personal or organizational network, consider replacing the screenshots with sanitized copies.
