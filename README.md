# Network Traffic Analysis with Wireshark and tcpdump

A hands-on network traffic analysis exercise using Kali Linux, Wireshark, and tcpdump. The work focused on capturing traffic, filtering protocols, examining DNS lookups associated with Spotify, checking ICMP connectivity, and reviewing TCP connection attempts.

*Evidence scope:* This repository includes both packet capture evidence (`network-traffic.pcap`) and Wireshark/terminal screenshots for packet-level verification and visual walkthroughs.

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

Screenshots are stored in the repository root (`01-*.png` through `14-*.png`), alongside `network-traffic.pcap`.

| Evidence | What it demonstrates |
| --- | --- |
| `01 - Setup` | Project folders and capture preparation |
| `02 - Test traffic` | ICMP ping and DNS lookup commands |
| `03 - Capture summary` | `tcpdump` capture stopped and packet count reported |
| `04 - PCAP metadata` | One capture metadata view: 1,653 packets, about 82.95 seconds |
| `05 - PCAP metadata` | Another capture metadata view: 450 packets, about 51.14 seconds |
| `06 - DNS overview` | DNS packets visible in Wireshark |
| `07 - Spotify DNS` | Spotify-related DNS query and response packets |
| `08 - TCP SYN` | TCP SYN packets and retransmission indications |
| `09 - ICMP` | ICMP echo requests and replies involving `1.1.1.1` |
| `10 - DNS in tcpdump` | DNS traffic inspected from the PCAP with `tcpdump` |
| `11 - ICMP in tcpdump` | ICMP requests and replies inspected with `tcpdump` |
| `12 - TCP in tcpdump` | TCP traffic inspected from the PCAP |
| `13 - DNS packet details` | Selected DNS packet details in Wireshark |
| `14 - Terminal inspection` | Terminal-based inspection of captured TCP traffic |

## Key observations

1. *Capture metadata:* The screenshots show two PCAP metadata snapshots: one with 450 packets over approximately 51.14 seconds and another with 1,653 packets over approximately 82.95 seconds. These are separate capture states, not a combined total.
2. *Spotify name resolution:* Wireshark shows DNS queries for `gae2-spclient.spotify.com`. The visible response includes a CNAME referring to `edge-web-gae2.dual-gslb.spotify.com`. This supports that Spotify-related name resolution occurred during the captured activity.
3. *DNS traffic beyond Spotify:* The capture also contains DNS lookups for other names, including Google-related domains. Therefore, the PCAP represents general host activity during the test window, not exclusively Spotify traffic.
4. *ICMP connectivity:* The screenshots show echo requests from `10.16.108.15` to `1.1.1.1` and corresponding echo replies. The accompanying terminal output reports four packets transmitted, four received, and 0% packet loss for that ping test.
5. *TCP connection attempts:* The SYN filter output shows connection attempts from `10.16.108.15` to `10.10.10.24` on destination port `9997` (commonly associated with Splunk forwarder traffic), and from `10.16.108.117` to `192.168.1.105` on destination port `1514` (commonly associated with Wazuh/OSSEC agent traffic). Several packets are labelled by Wireshark as TCP retransmissions.
6. *Investigation note:* Repeated SYN retransmissions can occur when a connection attempt is not acknowledged, for example because a destination is unavailable, filtered, or unreachable. The screenshots do not establish the cause, whether the traffic was expected, or whether it was malicious. These connections should be validated against the lab topology and expected services before drawing a security conclusion.

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
