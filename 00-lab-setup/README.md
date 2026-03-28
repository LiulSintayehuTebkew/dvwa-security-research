# Lab Setup Documentation

## 1. Instal Virtualization Manager

First of all we need virtual machines to practice with different vulnerabilities and malwares. That is why we need to first download and install vmware which is virtualization manager where it manages your virtual machines to access the physical resources. You can use vmware to create, edit or even delete virtual machines. Alternatively someone can install VirualBox as a virtualization manager.

---

## 2. Install Virtual Machines

The second step is to install the actual virtual machines.

### a. Kali linux (attack machine)

This is the virtual machine I am using as attack machine. This simulates the attackers perspective in threat modeling. Even though there are other linux distribution for offensive security like parrotos, I am more comfortable with kali.

### b. Metasploitable2

This is a deliberately vulnerable machine where it has so many services running like web service, ssh, ftp and so many more with each their own vulnerabilities giving us a field to try and find weaknesses and develop exploiting and mitigating ideas.

---

## 3. Explore Already installed tools for security community

- **Wireshark** => for packet analysis  
- **Burp Suite** => for web security  
- **Nmap** => for network mapper and port scanner  
- **Nessus** => for vulnerability scanning  
- **Metasploit Framework** => for exploitation and post-exploitation  
- **Gobuster** => for web URL directory Brute forcing  

---

## 4. DVWA

Web based vulnerability repository where you can exploit using different attack vectors. From bruteforce attack to sql injection attack, from xss attack to file upload vulnerability attacks.
