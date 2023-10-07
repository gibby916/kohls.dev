+++
title = "Windows Single-Sign-On (SSO) & GlobalProtect"
date = "2023-10-06"
author = "Brian"
authorTwitter = "" #do not include @
cover = "img/credential-provider.png"
tags = ["paloalto", "firewalls", "globalprotect"]
keywords = ["globalprotect", "paloalto"]
description = ""
showFullContent = false
readingTime = true
hideComments = true
color = "" #color from the theme settings
+++

# Palo Alto GlobalProtect - Windows SSO

Familiar with Windows credential providers? When using the default configuration of GlobalProtect, single-sign-on, or SSO, is enabled by default. This allows for the entering of a password into the GlobalProtect credential provider which will pass the authentication over to Windows. If your environment is leveraging password based authentication, this would likely simplify the VPN authentication process.

If your environment does not leverage password based authentication and instead leverages an alternate authentication method (e.g. Hello for Business, Yubikey / Certificate based authentication, etc) then this default option is no longer your friend. In this case, setting a registry value for `use-sso` to `no` along with updating the portal configuration within each agent profile for the `Use Single Sign-on (Windows)` to `No` will be beneficial. With this configuration, GlobalProtect is no longer added as a Windows credential provider and you can continue to leverage your existing configuration.

{{< image src="/img/single-sign-on.png" alt="Single Sign On" position="center" style="border-radius: 8px;" >}}

We experienced issues when installing GlobalProtect without this option enabled where customers were entering their H4B PIN or Yubikey PIN into the GlobalProtect credential provider which was looking for a password. This caused several accounts to be locked out and numerous help desk tickets to be created.