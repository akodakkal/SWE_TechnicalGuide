---
title: Remote Computing
keywords: 
tags: [Remote_Computing.md]
sidebar: kratos_workshops
summary: 
---
# Remote Computing
Some additional tools, which are presented here, are required to [execute the CFD simulation remotely](Remote_Computing.html){:target="_blank"} from the cluster of the Chair. 

___
## VPN
It is necessary to establish a vpn connection to the LRZ server of TUM. To connect to this server, the **eduVPN** client is recommended. [Download the client](https://www.eduvpn.org/client-apps/){:target="_blank"} and chose the link according to your operating system (Windows and Linux are both available). Download the **.exe** file, run it and follow the installation steps. After opening eduVPN, you can log in with your TUM credentials in order to establish a connection.

___
## Secure Shell (SSH) Client 
The next step after being connected to the TUM LRZ server via VPN is to establish an SSH connection with the Chair's cluster. A recommended SSH client is [Git Bash](https://gitforwindows.org/){:target="_blank"}. The way to establish connection and commands in Git are more similar to Linux. Download the **.exe** file, run it and follow the installation steps. 

Other popular SSH clients are:
- [PuTTY](https://www.putty.org/){:target="_blank"}
- [Open SSH](https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse?tabs=gui){:target="_blank"} 
- [VSCode](https://code.visualstudio.com/docs/remote/ssh){:target="_blank"} 

The other steps regarding [Remote Computing](../Remote_Computing.html){:target="_blank"} are explained in a seperate part of the Hitchhiker Guide.