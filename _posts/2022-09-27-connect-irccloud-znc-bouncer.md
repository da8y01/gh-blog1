---
layout: post
permalink: /connect-irccloud-znc-bouncer.html
published: true
date: 2022-09-27 13:09:42
tags: connect irc irccloud znc bouncer zncbouncer network client web mobile
title: "Connect the IRCCloud (web/mobile) client to a ZNC bouncer that is connected to some IRC network"
image: https://da8y01.github.io/gh-blog/assets/IRCloudZNC_LinkPreview.jpg
description: "Connect IRCCloud to ZNC bouncer"
---


<div style="text-align:center" markdown="1">
![IRCloudZNC_LinkPreview][IRCloudZNC_LinkPreview]{: width="70%"}
</div>

## Introduction
When trying to connect to [IRC][4] networks from different devices, there are many options for the client software/program to use.

In this case, there was a stable connection from web application client to the IRC networks through a connection to a pre-configured ([ZNC][2]) bouncer with its regular user registered and tested.

But from a mobile device, the setup/configuration could'nt satisfactorily/acceptably be made/done with any of the available apps.

The [IRCCloud mobile app][5] option worked just normal, but with the typical instability of the connection.

Then a configuration/setup for/with the [www.irccloud.com][3] Web application client to connect to a IRC (ZNC) bouncer was tried, to later reproduce it in a mobile device, and the result was the succesfull connection working/mode/done.

Next are the main instructions to achieve the configuration.


## Web
Appropiate configure/setup a ([ZNC][2]) bouncer instance and regular user in some VPS or server.

Test if the connection to the (ZNC) bouncer with the user credentials registered can be done/is succesfull; one way to do it is testing the login against the web administrative panel of the (ZNC) bouncer, typically entering the complete/correct URL (with port if needed/necessary) in the address bar of the web browser program/client, then fulfilling the user credentials and log in; again, if the login is successfull/can be done, then these would be the credentials/user-password/data to be utilized/used in the next steps of the setup/configuration.

Log in to the [www.irccloud.com][3] account to view the "Join a new network" page:
<div style="text-align:center" markdown="1">
![IRCloudZNC_JoinNetwork][IRCloudZNC_JoinNetwork]{: width="40%"}
</div>

Fulfill the fields with the (ZNC) bouncer data (HostName or IP, Port, NickName, etc.) used and tested before.

When connecting, the web client of IRCCloud would/will throw a message telling "Password required" plus a command to be entered with credentials to continue with the connection.
<div style="text-align:center" markdown="1">
![IRCloudZNC_PasswordRequired][IRCloudZNC_PasswordRequired]{: width="60%"}
</div>

For now (unless the process is automated in some way, or some "script" technique is used), one must manually type the correct command in the IRCCloud (web/mobile) client interface to be able to establish the connection between the IRCCloud (web/mobile) client and the IRC (ZNC) bouncer.

After entering the command, if everything that is needed/necessary works right/well, the IRCCloud (web/mobile) client must be connected to the more permanent/stable IRC (ZNC) bouncer server/connection/service.

From here, just to chat from this client software configured to use the bouncer.


## Mobile
Similar to the web client configuration/setup, once the [IRCCloud mobile app][5] is installed, just fulfill the form fields with the data of the pre-configured IRC (ZNC) bouncer.
<div style="text-align:center" markdown="1">
![IRCloudZNC_MobileServerSettings][IRCloudZNC_MobileServerSettings]{: width="40%"}
</div>

When the mobile app throws the "Password required" popup/modal/message, the analog/similar/correspondent/respective/right/correct command with credentials, as previously rehearsed/tested with the web application client, must be manually typed/entered (unless the process is automated in some way, or some "script" technique is used) to establish the connection between the mobile app client and the (ZNC) bouncer service/connection/server.
<div style="text-align:center" markdown="1">
![IRCloudZNC_MobilePasswordRequired][IRCloudZNC_MobilePasswordRequired]{: width="40%"}
</div>

If everything what is needed/necessary is fine, the mobile app client must be connected to the IRC network that was pre-configured in the (ZNC) bouncer service/server.
<div style="text-align:center" markdown="1">
![IRCloudZNC_MobileConnected][IRCloudZNC_MobileConnected]{: width="40%"}
</div>

That's all, from that it is just matter of type the right IRC server commands, to manage nicks, channels, memos and else related.


## References
* [https://www.irccloud.com/][1]{: target="_blank"}
* [https://play.google.com/store/apps/details?id=com.irccloud.android][5]{: target="_blank"}
* [https://wiki.znc.in/ZNC][2]{: target="_blank"}
* [https://en.wikipedia.org/wiki/Internet_Relay_Chat][4]{: target="_blank"}



[1]: https://www.irccloud.com/
[2]: https://wiki.znc.in/ZNC
[3]: https://www.irccloud.com/?/addNetwork
[4]: https://en.wikipedia.org/wiki/Internet_Relay_Chat
[5]: https://play.google.com/store/apps/details?id=com.irccloud.android

[IRCloudZNC_JoinNetwork]: {{ site.baseurl }}/assets/IRCloudZNC_JoinNetwork.png "IRCloudZNC Join Network"
[IRCloudZNC_PasswordRequired]: {{ site.baseurl }}/assets/IRCloudZNC_PasswordRequired.png "IRCloudZNC Password Required"
[IRCloudZNC_LinkPreview]: {{ site.baseurl }}/assets/IRCloudZNC_LinkPreview.jpg "IRCloudZNC Link Preview"
[IRCloudZNC_MobileServerSettings]: {{ site.baseurl }}/assets/IRCloudZNC_MobileServerSettings.jpg "IRCloudZNC Mobile Server Settings"
[IRCloudZNC_MobilePasswordRequired]: {{ site.baseurl }}/assets/IRCloudZNC_MobilePasswordRequired.jpg "IRCloudZNC Mobile Password Required"
[IRCloudZNC_MobileConnected]: {{ site.baseurl }}/assets/IRCloudZNC_MobileConnected.jpg "IRCloudZNC Mobile Connected"
