+++
title = "Panorama Overrides"
date = "2023-09-26"
author = "Brian"
authorTwitter = "" #do not include @
cover = "img/override.png"
tags = ["paloalto", "firewalls", "panorama"]
keywords = ["panorama", "paloalto"]
description = "There is a classic ‘Ford’ vs ‘Chevy’ debate when it comes to Panorama and the utilization of policy within ‘Pre’ or ‘Post’ rules. At a high level, policy assigned within Panorama to ‘Pre’ rules will take precendence over a local firewall’s policy, while policy assigned within Panorama to ‘Post’ rules will follow after the local firewall’s policy."
showFullContent = false
readingTime = true
hideComments = true
color = "" #color from the theme settings
+++

There is a classic 'Ford' vs 'Chevy' debate when it comes to Panorama and the utilization of policy within 'Pre' or 'Post' rules. At a high level, policy assigned within Panorama to 'Pre' rules will take precendence over a local firewall's policy, while policy assigned within Panorama to 'Post' rules will follow after the local firewall's policy.

Outside of policy, sometimes you may need to perform an override on other areas of configuration. In this case, we had experienced an issue where we could access the local firewall's management interface over CLI only and the web GUI failed to respond. Attempts to restart the device management service `debug software restart process management-server` and an entire firewall reboot did not resolve the issue. In this case, we then needed to override the ACL on the management interface to allow for a direct, local connection to the firewall. The following commands were utilized in CLI to update the ACL:

```
configure
override deviceconfig system permitted-ip
set deviceconfig system permitted-ip IP_ADDRESS
commit
```

It turned out the issue was related to fragmentation due to how the ISP was causing fragmentation. A redunction of the MTU settings on the management interface resolved this issue through command `set deviceconfig system mtu VALUE`. 