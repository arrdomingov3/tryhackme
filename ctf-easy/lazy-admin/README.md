![lazy admin ctf](./img/lazy-admin.gif)


<!-- TOC -->

- [1.0 Room Information](#10-room-information)
- [2.0 Web Virtual Machine Setup](#20-web-virtual-machine-setup)
- [3.0 Attack Proper](#30-attack-proper)
  - [3.1 Enumeration](#31-enumeration)
  - [3.2 Exploitation](#32-exploitation)
  - [3.3 Privilege Escalation](#33-privilege-escalation)

<!-- /TOC -->


# 1.0 Room Information

| 🏁 DETAILS: |  |
| ---               | ---                   |
| Room Title        | [LazyAdmin](https://tryhackme.com/room/lazyadmin)            |
| Room Description  | This room is about web directory discovery and CMS exploitation. This is an easy linux machine to practice.   |
| Room Type         | Free Room. Anyone can deploy virtual machines in the room (without being subscribed)!           |
| Room Difficulty   | Easy                  |
| Tools Used        | nmap, gobuster, wordlist           |
| Created by        | [MrSeth6797](https://tryhackme.com/p/MrSeth6797)  |


`
NOTE: Due to limited THM account capabilities, this pentesting is not accomplished in one-sitting. Attacker machine is only good for 60-minutes and will be reactivated after 24-hours. Hence, notice that the IP addresses of “Attacker machine” and “Target machine” is different on the screenshots.
`

# 2.0 Web Virtual Machine Setup

- Join the room.
- Setup the virtual environment by starting the “Attacker machine” and “Target machine”. Wait for 2-3 minutes for the machine to boot.

![Figure 1. Starting the attacker and target machine](./img/1%20starting%20the%20attacker%20and%20target%20machine.png)

*Figure 1. Starting the attacker and target machine*

- Check the IP address of both attacker and target machine.

![Figure 2. IP address of attacker machine is 10.144.191.97, while the IP address of target machine is 10.144.181.37.](./img/2%20ip%20address%20of%20attacker%20machine.png)

*Figure 2. IP address of attacker machine is 10.144.191.97, while the IP address of target machine is 10.144.181.37.*

- Go to the web VM and click “Terminal”.
- It is advisable to check the current IP address of the machine. Run the command “ifconfig”. The IP address must be the IP address of the attacker machine.

![Figure 3. Verifying the IP address of "Attacker machine", which is 10.144.191.97.](./img/3%20verifying%20the%20ip%20address.png)

*Figure 3. Verifying the IP address of "Attacker machine", which is 10.144.191.97.*

- Verify the connection between attacker machine and target machine by running the command “ping 10.144.181.37”.

![Figure 4. Successful ping to the target machine.](./img/4%20successful%20ping%20to%20the%20target%20machine.png)

*Figure 4. Successful ping to the target machine.*

# 3.0 Attack Proper

## 3.1 Enumeration
- Run nmap scan to check for open ports and services in the target machine and gather more information about the relevant attack vectors. Run the command “nmap 10.144.181.37”.

![Figure 5. From the nmap scan result, port 22 (ssh) and port 80 (http) are open.](./img/5%20from%20the%20nmap%20scan%20result.png)

*Figure 5. From the nmap scan result, port 22 (ssh) and port 80 (http) are open.*

- Let's check port 80 (http) which is the port for insecure web servers. In the web VM, open a browser and type the IP address of the target machine in the URL.

![Figure 6. After entering the IP address of the target machine in the browser, it is just a landing page of regular application with no malicious options.](./img/6%20after%20entering%20the%20ip%20address.png)

*Figure 6. After entering the IP address of the target machine in the browser, it is just a landing page of regular application with no malicious options.*

- Use Gobuster, which is a tool used to brute-force and discover hidden files, directories, DNS subdomains, and virtual hosts. We’re going to use the common.txt wordlist, which can be found on Kali by default at /usr/share/wordlists/dirb. The command is:
  
> gobuster dir -u http://\<remote-ip\> -w \<path-to-common-directory-list\>
> 
> where:
> 
> -u --> url
> 
> -w --> wordlist

- Run the gobuster command “gobuster dir -u http://10.144.151.246 -w /usr/share/wordlists/dirb/common.txt”.

![Figure 7. Gobuster result](./img/7%20gobuster%20result.png)

*Figure 7. Gobuster result.*

> `Status 403` is forbidden code, which indicates that the server successfully understands the request but completely refuses to authorize it. 
> 
> `Status 301` is an HTTP response that means a requested web page or file has been permanently moved to a new URL. 
> 
> `Status 200` is OK status code, which means that a client's request was successfully received, understood, and processed by the server.

- Base on the Gobuster result, we’ve already checked the index.html (landing page) and now let’s check what’s in the /content directory.

![Figure 8. Content page.](./img/8%20content%20page.png)

*Figure 8. Content page.*

- There isn’t much on content page. It is either still under construction or planning stage. Therefore, let’s run Gobuster again. This time, include the content subdomain let’s use a slightly bigger wordlist (/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt).

![Figure 9. Result of running Gobuster with the content subdomain.](./img/9%20result%20of%20gobuster%20with%20content%20subdomain.png)

*Figure 9. Result of running Gobuster with the content subdomain.*

- With the result of the Gobuster, let’s also find any CVEs for SweetRice. In the terminal, run “searchsploit sweetrice”.

![Figure 10. Searching CVE using terminal.](./img/10%20searching%20cve%20using%20terminal.png)

*Figure 10. Searching CVE using terminal.*

- As an alternative, open a browser and go to “exploit-db.com” and search for “SweetRice”.

![Figure 11. SweetRice result in exploit-db.com.](./img/11%20sweetrice%20result%20in%20exploitdb.png)

*Figure 11. SweetRice result in exploit-db.com.*

- Looks like there are couple of options. Le’ts disregard the old versions of SweetRice and focus on the latest version which is 1.5.1. This time, we are very particular to the “Backup Disclosure”.

![Figure 12. Focus on Backup Disclosure.](./img/12%20focus%20on%20backup%20disclosure.png)

*Figure 12. Focus on Backup Disclosure.*

- Click “Backup Disclosure” to see the details of SweetRice vulnerability.

![Figure 13. Hint to retrieve the password.](./img/13%20hint%20to%20retrieve%20password.png)

*Figure 13. Hint to retrieve the password.*


## 3.2 Exploitation


## 3.3 Privilege Escalation

