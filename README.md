# DHCP Starvation Attack: Detection & Remediation
[cite_start]**Project Lead:** Group 4 (Isaiah, Panashe, LaVonte) [cite: 17, 18]

## 1. Threat Overview
[cite_start]A DHCP Starvation attack is a malicious flood that weaponizes the DHCP protocol to create a network-wide denial of service[cite: 24]. [cite_start]The attack floods a server with fake `DHCPDISCOVER` packets using spoofed MAC addresses[cite: 25].

[cite_start]**The Goal:** Exhaust all available IP addresses in the pool, denying service to legitimate clients[cite: 26].

### Actor Objectives
* [cite_start]**Credential Harvesting:** Stealing user credentials[cite: 30].
* [cite_start]**Man-in-the-Middle (MITM):** Deploying rogue DHCP servers to intercept traffic[cite: 32].
* [cite_start]**Business Disruption:** Causing costly downtime and productivity losses[cite: 63].

## 2. Technical Analysis & Evidence
[cite_start]During our capture, we observed a massive volume of traffic that matches the signature of a starvation attack[cite: 37].

* [cite_start]**Total DHCP Discover Packets:** 318,076 out of 318,333 total packets[cite: 52].
* [cite_start]**Source:** `0.0.0.0` (Client has no IP yet)[cite: 39, 40].
* [cite_start]**Destination:** `255.255.255.255` (Broadcast to the entire local network)[cite: 41, 42].

### Identified Spoofed MAC Addresses
* [cite_start]`18:dc:d3:53:cb:0e` [cite: 54]
* [cite_start]`a4:a2:38:1d:82:88` [cite: 55]

![Network Traffic Stats](assets/Screenshot%202026-03-12%20124942.png)

## 3. Detection Methodology (Wireshark)
[cite_start]To pinpoint the flood of fake DHCP requests, use the following specialized filters[cite: 50]:

* [cite_start]`bootp.type == 1`: Filters for `DHCPDISCOVER` packets (BOOTP Request)[cite: 48].
* [cite_start]`ip.src == 0.0.0.0 && ip.dst == 255.255.255.255`: Identifies packets originating from unknown sources broadcast to the network[cite: 49].

![Filter Evidence](assets/Screenshot%202026-03-12%20124953.png)

## 4. Remediation Strategy: DHCP Snooping
[cite_start]The primary defense against these risks is implementing **DHCP Snooping** on network switches[cite: 66, 67].

1. [cite_start]**Track Messages:** Monitor all DHCP traffic flowing through the switch[cite: 15].
2. [cite_start]**Build Binding Table:** Create a database of valid IP-to-MAC-to-port mappings[cite: 15].
3. [cite_start]**Block Unauthorized Traffic:** Drop messages from untrusted ports or with spoofed MACs[cite: 15].
4. [cite_start]**Prevent Rogue Servers:** Stop attackers from introducing fake DHCP servers[cite: 15].

[cite_start]**Complementary Measure:** Implement **Port Security** to limit the number of MAC addresses allowed per port to prevent mass spoofing[cite: 69].
