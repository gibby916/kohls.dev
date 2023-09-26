+++
title = "GlobalProtect Enforcer Troubleshooting"
date = "2023-09-25T17:08:40-05:00"
author = "Brian"
authorTwitter = "" #do not include @
cover = "img/enforcer.png"
tags = ["paloalto", "firewalls", "globalprotect"]
keywords = ["globalprotect", "paloalto"]
description = "Palo Alto Enforcer Mode and Broadcast Traffic"
showFullContent = false
readingTime = false
hideComments = false
color = "" #color from the theme settings
+++

# Palo Alto GlobalProtect - Enforcer Mode

You may be familiar with the aft named 'Enforcer Mode' from GlobalProtect. This feature is enabled when you set the `Enforce GlobalProtect Connection for Network Access` to `Yes`. There are two additional fields which can optionally be enabled as well:

{{< image src="/img/app-configurations.png" alt="App Configurations" position="center" style="border-radius: 8px;" >}}

Currently, as the enforcement mode filtering is performed client side, there is no logging available outside of validating what IP / Subnets / domains have been whitelisted in the client configuration. When working with TCP/UDP based traffic, this is typically straight forward, and performing a client side PCAP with a packet capture tool such as Wireshark is a fallback. But what about broadcast traffic?

This is where I have found the vendor's documentation to be a bit lacking. Our customer had a need to communicate with a local network device while working remotely with no VPN connection established. Although the subnet was approved by the InfoSec team for 'whitelisting' within the enforcer mode's IP address list, the customer's application was still not functioning. At a high level, the application functioned by sending out a broadcast message which the remote device would respond with a UDP packet containing device information, allowing the remote device to be populated within the GUI of the customer's application. With enforcer mode DISABLED, a PCAP showed a broadcast was sent to the global broadcast address of 255.255.255.255, and a subsequent inbound UDP packet arrived from the remote device. With enforcer mode ENABLED, a PCAP never showed the outbound global broadcast making it to the ethernet adapter. Although it appears special exceptions are performed by the enforcer mode for special packets such as DHCP, this custom broadcast packet was dropped. 

To resolve this issue, we were required to add the global broadcast address, 255.255.255.255/32, to the enforcer mode's IP address list. Once this was performed, the customer's application properly populated the remote device within the GUI.