---
layout: post
title: " Connecting the BeagleBone Black to the internet"
modified:
categories: blog
excerpt:
tags: []
image:
  feature:
date: 2014-02-27T08:08:50-04:00
---

I had a hard time connecting the BeagleBone Black to the internet while I was working from Fedora 19. The tutorials for other Linux distros were pretty straight forward. After having pondered over it for a day or two I finally managed to access internet from the Black. Special credits to [Gaurav Jain](http://www.linkedin.com/pub/gaurav-jain/1b/125/2b3) :D.

  
  
So here is what I did,      

SSH to your Black using *ssh root@192.168.7.2*. Once this is done run the following commands from the beaglebone shell.   

1.  root@beaglebone:/ # /sbin/route add default gw 192.168.7.1
2.  root@beaglebone:/ # echo "nameserver 8.8.8.8" >> /etc/resolv.conf

**On the host Linux Machine except Fedora:**  
    
1. sudo iptables -A POSTROUTING -t nat -j MASQUERADE  
    
	This instruction enables the masquerading option allowing any device in the subnet to act on behalf of the host machine. 
    
2. sudo echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward > /dev/null
    
    This instruction enables the ipv4 forwarding providing the beaglebone with access to the internet.

**On a Fedora machine instead of above two intsructions:**    

1. Open Firewall settings.    
  
2. Select zone as public.    
  
3. In the side tab panel select masquerading    
  
4. Enable Masquerade zones.    

**Switch back to the BeagleBone Black terminal**      
root@beaglebone:/ # ping google.com    
*A successful ping implies beaglebone black now  has internet access.*    

**Go ahead and update your Black!**       
	root@beaglebone:/ # opkg update    
*(opkg is the package manager for the BeagleBone Black.)*
  
  
Also here is one nice tutorial for getting a hands on with the BBB [BBB-Workshop by Anuj Deshpande](https://github.com/anujdeshpande/BBB-workshop)


  
