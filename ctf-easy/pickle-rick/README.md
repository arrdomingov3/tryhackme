![pickle rick ctf](./img/pickle%20rick.gif)

<!-- TOC -->

- [1.0 Room Information](#10-room-information)
- [2.0 Web Virtual Machine Setup](#20-web-virtual-machine-setup)
- [3.0 Attack Proper](#30-attack-proper)
  - [3.1 Scanning and Enumeration](#31-scanning-and-enumeration)

<!-- /TOC -->


# 1.0 Room Information

| 🏁 DETAILS: |  |
| ---               | ---                   |
| Room Title        | [Pickle Rick ](https://tryhackme.com/room/picklerick?ref=blog.tryhackme.com)            |
| Room Description  | A Rick and Morty CTF. Help turn Rick back into a human!   |
| Room Type         | Free Room. Anyone can deploy virtual machines in the room (without being subscribed)!           |
| Room Difficulty   | Easy                  |
| Tools Used        |           |
| Created by        | [tryhackme](https://tryhackme.com/p/tryhackme), [ar33zy](https://tryhackme.com/p/ar33zy), [arebel](https://tryhackme.com/p/arebel)  |

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
