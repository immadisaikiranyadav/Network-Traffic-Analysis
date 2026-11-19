# Network Traffic Investigation Report

## 1. Summary

This report documents a packet-capture and analysis exercise performed in Kali Linux using tcpdump and Wireshark. The available evidence includes terminal output and Wireshark screenshots. The original PCAP file was not provided for this report, so conclusions are restricted to the displayed evidence.

## 2. Scope

The analysis covered:
- DNS queries and responses, including Spotify-related names
- ICMP echo request/reply traffic involving `1.1.1.1`
- TCP SYN connection attempts and retransmission indications
- PCAP metadata and tcpdump-based packet inspection

## 3. Evidence and observations

### 3.1 Capture metadata

Two different metadata screenshots are present:
- Capture snapshot A: 450 packets, file size about 139 kB, duration about 51.14 seconds, average packet rate about 8 packets/second.
- Capture snapshot B: 1,653 packets, file size about 677 kB, duration about 82.95 seconds, average packet rate about 19 packets/second.

These values belong to separate capture snapshots. They must not be added together or presented as one capture.

### 3.2 DNS and Spotify

The Wireshark display filter `dns.qry.name contains "spotify"` displays Spotify-related DNS traffic. The visible query includes `gae2-spclient.spotify.com`; the response shows a CNAME to a hostname under `edge-web-gae2.dual-gslb.spotify.com`.

This is evidence of DNS name resolution associated with Spotify during the capture period. DNS visibility does not reveal the contents of Spotify sessions or the music being streamed. The screenshots also show other DNS lookups, so not all captured DNS activity is Spotify-related.

### 3.3 ICMP

The Wireshark filter `ip.addr == 1.1.1.1` shows ICMP echo requests from `10.16.108.15` to `1.1.1.1` and corresponding replies. The terminal screenshot of `ping -c 4 1.1.1.1` reports 4 transmitted, 4 received, and 0% packet loss.

This supports successful ICMP reachability to `1.1.1.1` during that test.

### 3.4 TCP SYN and retransmissions

The Wireshark screenshot using a SYN filter displays:
- `10.16.108.15` attempting a TCP connection to `10.10.10.24:9997`
- `10.16.108.117` attempting a TCP connection to `192.168.1.105:1514`
- Multiple packets marked `[TCP Retransmission]`

A SYN retransmission indicates that a SYN packet was sent again because the expected response was not observed within the retransmission interval. It does not, by itself, prove scanning, intrusion, or malicious activity. Possible explanations include an unavailable service, host/network filtering, routing problems, or an intentionally isolated lab endpoint.

The screenshots do not establish the destination ownership, intended service, or cause of the retransmissions. These require validation using the network topology, endpoint/service logs, and the original PCAP.

## 4. Useful display filters

```wireshark
dns
dns.qry.name contains "spotify"
tcp
tcp.flags.syn == 1 && tcp.flags.ack == 0
tcp.analysis.retransmission
icmp
ip.addr == 1.1.1.1
tcp.port == 443
```

## 5. Conclusion

The evidence confirms that the capture contains Spotify-related DNS resolution, DNS activity for other services, successful ICMP echo exchanges with `1.1.1.1`, and TCP SYN attempts with retransmission indications. No malicious activity can be confirmed from these screenshots alone.

## 6. Recommended follow-up

1. Preserve and add the original PCAP file.
2. Use `Statistics -> Conversations -> TCP` to identify connection duration, packet counts, and endpoints.
3. Follow each relevant TCP stream and inspect whether a SYN-ACK or RST is present.
4. Confirm whether `10.10.10.24:9997` and `192.168.1.105:1514` are expected lab services.
5. Compare the packet timeline with endpoint and service logs before classifying the activity.
