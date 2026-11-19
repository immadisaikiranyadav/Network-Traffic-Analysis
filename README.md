# Network Traffic Analysis with Wireshark and tcpdump

A hands-on network traffic analysis exercise using Kali Linux, Wireshark, and tcpdump. The work focused on capturing traffic, filtering protocols, examining DNS lookups associated with Spotify, checking ICMP connectivity, and reviewing TCP connection attempts.

> **Evidence scope:** This repository was prepared from the screenshots supplied for this exercise. The original `.pcap` capture file was not supplied with the screenshots, so packet-level findings are limited to what is visible in the evidence. Add the original PCAP if you want others to independently reproduce the analysis.

## Objectives

- Capture network traffic with `tcpdump`.
- Review a PCAP file in Wireshark.
- Apply display filters to isolate DNS, ICMP, and TCP packets.
- Identify DNS queries associated with Spotify.
- Inspect TCP SYN packets and retransmission indicators.
- Record observations without treating an indicator as proof of malicious activity.

## Environment and tools

- Operating system: Kali Linux (running in a virtual machine, based on the supplied screenshots)
- Packet capture: `tcpdump`
- Packet analysis: Wireshark
- PCAP inspection: `capinfos`
- Test traffic: `ping` and `nslookup`

## Project evidence

Screenshots are stored in [`screenshots/`](screenshots/).

| Evidence | What it demonstrates |
|---|---|
| [01 - Setup](screenshots/01-project-folder-and-capture-setup.png) | Project folders and capture preparation |
| [02 - Test traffic](screenshots/02-ping-and-dns-test-commands.png) | ICMP ping and DNS lookup commands |
| [03 - Capture summary](screenshots/03-tcpdump-capture-stop-summary.png) | tcpdump capture stopped and packet count reported |
| [04 - PCAP metadata](screenshots/04-pcap-metadata-1653-packets.png) | One capture metadata view: 1,653 packets, about 82.95 seconds |
| [05 - PCAP metadata](screenshots/05-pcap-metadata-450-packets.png) | Another capture metadata view: 450 packets, about 51.14 seconds |
| [06 - DNS overview](screenshots/06-dns-traffic-overview.png) | DNS packets visible in Wireshark |
| [07 - Spotify DNS](screenshots/07-spotify-dns-query-filter.png) | Spotify-related DNS query and response packets |
| [08 - TCP SYN](screenshots/08-tcp-syn-and-retransmissions.png) | TCP SYN packets and retransmission indications |
| [09 - ICMP](screenshots/09-icmp-traffic-to-1-1-1-1.png) | ICMP echo requests and replies involving 1.1.1.1 |
| [10 - DNS in tcpdump](screenshots/10-dns-queries-from-tcpdump.png) | DNS traffic inspected from the PCAP with tcpdump |
| [11 - ICMP in tcpdump](screenshots/11-icmp-request-reply-from-tcpdump.png) | ICMP requests and replies inspected with tcpdump |
| [12 - TCP in tcpdump](screenshots/12-tcp-traffic-from-tcpdump.png) | TCP traffic inspected from the PCAP |
| [13 - DNS packet details](screenshots/13-spotify-dns-packet-details.png) | Selected DNS packet details in Wireshark |
| [14 - Terminal inspection](screenshots/14-traffic-inspection-terminal.png) | Terminal-based inspection of captured TCP traffic |

## Key observations

1. **Capture metadata:** The screenshots show two PCAP metadata snapshots: one with 450 packets over approximately 51.14 seconds and another with 1,653 packets over approximately 82.95 seconds. These are separate capture states, not a combined total.
2. **Spotify name resolution:** Wireshark shows DNS queries for `gae2-spclient.spotify.com`. The visible response includes a CNAME referring to `edge-web-gae2.dual-gslb.spotify.com`. This supports that Spotify-related name resolution occurred during the captured activity.
3. **DNS traffic beyond Spotify:** The capture also contains DNS lookups for other names, including Google-related domains. Therefore, the PCAP represents general host activity during the test window, not exclusively Spotify traffic.
4. **ICMP connectivity:** The screenshots show echo requests from `10.16.108.15` to `1.1.1.1` and corresponding echo replies. The accompanying terminal output reports four packets transmitted, four received, and 0% packet loss for that ping test.
5. **TCP connection attempts:** The SYN filter output shows connection attempts from `10.16.108.15` to `10.10.10.24` on destination port `9997`, and from `10.16.108.117` to `192.168.1.105` on destination port `1514`. Several packets are labelled by Wireshark as TCP retransmissions.
6. **Investigation note:** Repeated SYN retransmissions can occur when a connection attempt is not acknowledged, for example because a destination is unavailable, filtered, or unreachable. The screenshots do not establish the cause, whether the traffic was expected, or whether it was malicious. These connections should be validated against the lab topology and expected services before drawing a security conclusion.

## Reproduction commands

The commands below are examples of the workflow shown in the screenshots. Replace `eth0` if your active interface has a different name.

```bash
# Identify interfaces and routes
ip a
ip route

# Create the project folders
mkdir -p ~/SOC-Projects/Network-Traffic-Analysis/{pcap,screenshots,reports}

# Capture traffic to a PCAP
sudo tcpdump -i eth0 -nn -s 0 -w ~/SOC-Projects/Network-Traffic-Analysis/pcap/network-traffic.pcap

# Generate simple test traffic in another terminal
ping -c 4 1.1.1.1
nslookup google.com

# Inspect the saved capture
capinfos ~/SOC-Projects/Network-Traffic-Analysis/pcap/network-traffic.pcap
sudo tcpdump -nn -r ~/SOC-Projects/Network-Traffic-Analysis/pcap/network-traffic.pcap
```

## Limitations and next steps

- The original PCAP file is not included yet. Screenshots are useful evidence, but a PCAP is needed for independent packet-level verification.
- The supplied screenshots show DNS, ICMP, and TCP analysis. They do not show a complete TCP three-way handshake for every connection.
- No confirmed malicious activity is established by this evidence.
- Next, add the original `.pcap` file, inspect the TCP conversations and retransmissions in context, record the expected lab services for ports `9997` and `1514`, and update the report with verified results.

## Conclusion

This exercise demonstrates basic packet capture and protocol-focused analysis with tcpdump and Wireshark. It documents Spotify-related DNS resolution, successful ICMP echo exchanges, and TCP SYN retransmission observations. The next stage is to validate the TCP connection attempts against the intended lab environment using the original PCAP and supporting system or service logs.
