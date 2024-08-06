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
