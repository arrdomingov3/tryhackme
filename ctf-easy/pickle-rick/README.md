![pickle rick ctf](./img/pickle%20rick.gif)

<!-- TOC -->

- [1.0 Room Information](#10-room-information)
- [2.0 Web Virtual Machine Setup](#20-web-virtual-machine-setup)
- [3.0 Attack Proper](#30-attack-proper)
  - [3.1 Scanning and Enumeration](#31-scanning-and-enumeration)
  - [3.2 Deep-dive to hidden directories using GoBuster](#32-deep-dive-to-hidden-directories-using-gobuster)
  - [3.3 Exploring the vulnerability of the webapp](#33-exploring-the-vulnerability-of-the-webapp)

<!-- /TOC -->


# 1.0 Room Information

| 🏁 DETAILS: |  |
| ---               | ---                   |
| Room Title        | [Pickle Rick ](https://tryhackme.com/room/picklerick?ref=blog.tryhackme.com)            |
| Room Description  | A Rick and Morty CTF. Help turn Rick back into a human!   |
| Room Type         | Free Room. Anyone can deploy virtual machines in the room (without being subscribed)!           |
| Room Difficulty   | Easy                  |
| Tools Used        | •	`Nmap` - open-source utility use to discover devices on a network, map network architecture, find open ports, and identify system vulnerabilities. <br> •	`GoBuster` - tool used to brute-force and discover hidden files, directories, DNS subdomains, and virtual hosts. <br> •	`Wordlist` - are plain text files containing lists of words, phrases, or characters primarily used in cybersecurity for penetration testing, password cracking, security testing, and web directory enumeration.          |
| Created by        | [tryhackme](https://tryhackme.com/p/tryhackme), [ar33zy](https://tryhackme.com/p/ar33zy), [arebel](https://tryhackme.com/p/arebel)  |


> NOTE: Due to limited THM account capabilities, this pentesting is not accomplished in one-sitting. Hence, notice that the IP addresses of “Attacker machine” and “Target machine” is different on the screenshots.


# 2.0 Web Virtual Machine Setup

- Join the room.
- Setup the virtual environment by starting the “Attacker machine” and “Target machine”. Wait for 2-3 minutes for the machine to boot.

![Figure 1. Starting the attacker and target machine.](./img/1%20start.png)

*Figure 1. Starting the attacker and target machine.*

- Check the IP address of both attacker and target machine.

![Figure 2. IP address of attacker and targe machine.](./img/2%20ip%20address.png)

*Figure 2. IP address of attacker and target machine.*

- Go to the web VM and click “Terminal”.
- It is advisable to check the current IP address of the machine. Run the command “ifconfig”. The IP address must be the IP address of the attacker machine.
- Verify the connection between attacker machine and target machine by running the “ping” command.

# 3.0 Attack Proper

## 3.1 Scanning and Enumeration

- The very first step in penetration testing is to start by scanning the target machine for any open ports, this is a good rule of thumb. Open the terminal and enter “nmap \<target machine IP address\>”.

![Figure 3. Result of nmap scan.](./img/3%20nmap%20scan.png)

*Figure 3. Result of nmap scan.*

- From the nmap scan, port 22 is used for SSH protocol. SSH usually isn’t too vulnerable unless we have the username and password. So, we can ignore port 22 for now.
- Let us work on port 80, which is used for HTTP protocol where a website is being hosted. To begin the enumeration, open a browser and enter the IP address of the target machine in the URL. 

![Figure 4. Landing page of 10.144.135.80.](./img/4%20landing%20page.png)

*Figure 4. Landing page of 10.144.135.80.*

- First things first, check the source code of the webpage to check if there are any helpful clues. Right-click on the webpage and choose “Inspect”.

![Figure 5. Checking the source code.](./img/5%20source%20code.png)

*Figure 5. Checking the source code.*

- From the source code, there seems to be an interesting message about the username. Username: R1ckRul3s

![Figure 6. Username is R1ckRul3s.](./img/6%20username.png)

*Figure 6. Username is R1ckRul3s.*

## 3.2 Deep-dive to hidden directories using GoBuster

- Since we found the username, it’s time for more recon to find the password. 
- We will use gobuster and try to locate any directories that may have been hidden from us. Gobuster works together with wordlist. We will use a slightly bigger wordlist which is “directory-list-2.3-medium.txt”.
- Open the terminal and type “gobuster dir -u http://\<target machine IP address\> -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,sh,txt,cgi,html,css,js,py”.

> where:
>
> -u  --> URL
>
> -w --> path to the wordlist for brute forcing
>
> -x --> list of extensions to check for, if any. It is also possible to use “-x *”, this means look for all/any extensions it can find.

![Figure 7. Result of GoBuster command.](./img/7%20gobuster%20command.png)

*Figure 7. Result of GoBuster command.*

- Earlier we found a username but at this moment we still do not have a password. Most websites will have a robots.txt file that will tell a browser what is and isn't allowed to index. Let’s see if there is one for this site. Go to the browser and type "http://10.146.158.40/robots.txt".

![Figure 8. Seems to be a useful information which is Wubbalubbadubdub.](./img/8%20password.png)

*Figure 8. Seems to be a useful information which is Wubbalubbadubdub.*

- Let’s try to find out if what we’ve found is the password. Remember running gobuster command, there is a result of “login.php”. So, open the browser and go to “http://10.146.158.40/login.php”. Use these credentials:

> Username: R1ckRul3s
> 
> Password: Wubbalubbadubdub

![Figure 9. Login page.](./img/9%20login.png)

*Figure 9. Login page.*

- Looks like what we’ve found on “robots.txt” is the password of the login page. By obtaining the username and password of the login page, we were able to get logged in and we were brought to the command portal.

![Figure 10. Command portal.](./img/10%20portal.png)

*Figure 10. Command portal.*

## 3.3 Exploring the vulnerability of the webapp

- Since this page is called “Command Panel”, let’s enter some command and see what actually happens.
- Enter “ls” in the command panel and click execute.

![Figure 11. Entering “ls” command in command panel.](./img/11%20ls.png)

*Figure 11. Entering “ls” command in command panel.*

- We can see that some results are being returned. It would seem that we have a file where we can find our first ingredient.

![Figure 12. Result of “ls” command.](./img/12%20result%20ls.png)

*Figure 12. Result of “ls” command.*

- Based on the result, try to enter “cat Sup3rS3cretPickl3Ingred.txt”. 

![Figure 13. Opening one txt file.](./img/13%20txt%20file.png)

*Figure 13. Opening one txt file.*

- However, the result is not what we are expecting.

![Figure 14. Result of opening the txt file.](./img/14%20result%20txt%20file.png)

*Figure 14. Result of opening the txt file.*

- So, as a solution, go to browser and enter “http://\<ipAddress\>/Sup3rS3cretPickl3Ingred.txt”. 

> Note: We can also use an alternative command for “cat” which is “less”. This will give the same result. In the Command Panel, enter the command “less Sup3rS3cretPickl3Ingred.txt” to get the first ingredient/flag.

![Figure 15. First ingredient.](./img/15%20first%20ingredient.png)

*Figure 15. First ingredient.*


> ❓(Question 1/3) What is the first ingredient that Rick needs? 👉 mr. meeseek hair

- We are now one ingredient. It is time to find the other two, but how can we find it? Remember the “ls” command we run in the Command Panel, there is a file called “clue.txt”. Let’s open this and see if there is anything useful. Open the browser and type “http://\<ipAddress\>/clue.txt”.

![Figure 16. Opening clue.txt.](./img/16%20clue.png)

*Figure 16. Opening clue.txt.*

- Seems that the other ingredients are also in the file system, however we don’t know what it’s called. So, go to the Command Panel and execute “ls /home”. This will list out everything in our home directory.

![Figure 17. Result of running ls /home command.](./img/17%20ls%20home.png)

*Figure 17. Result of running ls /home command.*

- Let’s keep digging and execute “ls /home/rick”. Within the rick directory, we have a file called second ingredients.

![Figure 18. Second ingredients.](./img/18%20second%20ingredient.png)

*Figure 18. Second ingredients.*

- Since “cat” command is disabled in the Command Panel, Iet’s use the alternative command which is “less”. Execute “less '/home/rick/second ingredients'” command in the Command Panel.

![Figure 19. Using less command.](./img/19%20less%20com.png)

*Figure 19. Using less command.*

- Running “less” command worked. We found the second ingredient.

![Figure 20. Second ingredient is "1 jerry tear".](./img/20%20found%20second%20ing.png)

*Figure 20. Second ingredient is "1 jerry tear".*

> ❓(Question 2/3) What is the second ingredient in Rick’s potion? 👉 1 jerry tear

- We are now off to the third ingredients. Making an educated guess, let’s look in the root directory to see if the third ingredient was there. In the Command Panel, execute “sudo ls /root”.

![Figure 21. Command to see root directory.](./img/21%20root%20dir.png)

*Figure 21. Command to see root directory.*

- Running “sudo ls /root” command would potentially lead us to the third ingredients.

![Figure 22. Useful files in the root directory.](./img/22%20useful%20files.png)

*Figure 22. Useful files in the root directory.*

- •	Now that we have found the possible location of the third ingredients, we are going to use “less” command, but remember to use “sudo” command because we are in the root directory. So, execute the command “sudo less /root/3rd.txt".

![Figure 23. Checking the third ingredients.](./img/23%20check%20third.png)

*Figure 23. Checking the third ingredients.*

- Boom! There is the third and last ingredient in order to transform Rick back to human from a pickle!

![Figure 24. Third ingredients is "fleeb juice".](./img/24%20found%20third%20ing.png)

*Figure 24. Third ingredients is "fleeb juice".*

> ❓ (Question 3/3) What is the last and final ingredient? 👉 fleeb juice