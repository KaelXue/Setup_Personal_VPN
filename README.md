# Setup_Personal_VPN
Build a server through Akamai and practice local and remote VPN based on WireGuard

This article is just notes, and does not involve very professional coding and principle analysis. This note only ensures that personal VPN can be configured smoothly. In addition, special thanks to Maxwell for full remote support.

**Local**  
Device:										**MacBook Pro M1**  
Virtual Machine Software:	**UTM**  
Virtual machine OS:				**Ubuntu22.04 ARM**  

**Remote**  
OS:												**Ubuntu22.04**

### Background
This is my first time using the Linux system in China. I learned that I need a domestic mirror source to use functions such as apt to install applications.   
However, since the device is a Mac M1 chip with an Arm architecture, there are not many compatible Ubuntu versions, making it more difficult to find the mirror source.  
Now that you have found a suitable mirror, why do you still need to build an overseas VPN?  
Because some sources URLs are no longer accessible! I tried to install OpenCV. Anyone who has installed it knows how troublesome it is. There are many libraries to install, and one or two of them cannot be found in the mirror source I configured.  
After configuring new mirror sources several times without success, I decided to build own VPN and use the original source to directly obtain the library.

## Get a remote server
Find a rental server website you like. If not, this link is for reference only([Akamai](https://www.cloud.linode.com)).  
**ADD PICTURE LATER**
Select the OS you want and the region where the server is located. Select Shared CPU below (since it is only used for pulling to the library, I only chose the cheapest one of $5 per month)  
You can choose the password, SSH, VPC, firewall and other configurations later.  
After the creation is completed, you will enter the following interface. Congratulations, you have completed the first step.  
**ADD PICTURE LATER**

## Accessing the Server via SSH
Recommended SSH software includes [Putty](https://www.putty.org/), [Termius](https://termius.com/download/macos), [Xshell](https://www.netsarang.com/en/xshell/), [Finalshell](https://github.com/flathub/com.hostbuf.FinalShell), etc.  
The screenshot above contains the server's SSH access instructions, just copy them (format: ssh root@xxx.xxx.xx.xx)(If the port number is required, the default value is 22)  
**Notice**: If the server is running and the command is entered correctly, but you cannot connect to the server, you can try to ping the server to test whether it is working. Using [ping](https://ping.pe/) or **ping** in terminal.  
**ADD ERROR & NORMAL LATER**

## Configuration WireGuard (Server)
Download WireGuard:
```
wget --no-check-certificate -O /opt/wireguard.sh https://raw.githubusercontent.com/teddysun/across/master/wireguard.sh
chmod 755 /opt/wireguard.sh
```
Install:
```
/opt/wireguard.sh -r
```
**Notice**: If an exception occurs or the command cannot be executed, use the following command instead.  
**ADD ERROR PICTURE**
```
/opt/wireguard.sh -s
```
Get client interface information:
```
cat /etc/wireguard/wg0_client
```
If the above instructions operate normally, you will get the following information:
**ADD INFOR**
This information includes public key, private key, address, port, DNS, etc.  
Check if the following configuration exists in sysctl.conf:
```
nano /etc/sysctl.conf
  net.ipv4.ip_forward=1
  net.ipv6.conf.all.forwarding=1
```
**Congratulations, you have completed all the configurations of the server.**  

## Configuration WireGuard (Client)
Install:
```
sudo apt install wireguard
```
Configure network information:
```
nano /etc/wireguard/wg0.conf
```
It doesn't matter if it is blank, just copy the information you just cat from the server and save it. like:
```
[Interface]
PrivateKey = xxxxxxxx
Address = xxxxx
DNS = xxxxxx

[Peer]
PublicKey = xxxxx
PresharedKey = xxxx
AllowedIPs = xxxxx
Endpoint = xxxxx
```
Enable the network configuration:
```
sudo wg-quick up wg0
```
**NOTICE**: The following error message may appear:  
**ADD RESOLVCONF**  
Just install it
```
sudo apt install resolvconf
```
At this point you have completed all the configurations. You can ping Google or other websites to test whether they can be accessed normally.

**PS**: You can add more than one machine as a client. To add multiple machines, use:
```
/opt/wireguard.sh -a
NEW_NAME
```


