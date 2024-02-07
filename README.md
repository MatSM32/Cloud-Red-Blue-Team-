# Cloud-Red/Blue-Team
# Description 
To do this you will need to set up three vms using any hypervisors of your choice. For me I created them in AWS, if you would like to do the same follow this tutorial from this youtuber: https://youtu.be/2cMkpLoKUj0?si=d9a7h05a51ojcyJU. In this project, you will be using Kali Linux as an attacker to do a dictionary attack using Hydra and using Nmap to do scans to find what ports are open. With the security tools VM, we will be using Splunk and Nessus. Then with the Windows VM we will be using it to do attacks on.
# Utilities Used
| Splunk 
| Hydra
# Environments Used 
| Windows 10
| Kali Linux
| Ubuntu
# Attack/Defense Walkthrough:
Launch Kali Linux: 
![lin](https://github.com/MatSM32/Cloud-Red-Blue-Team-/assets/150560131/e9c8f4a7-9528-4a70-a7db-b9fe5bef6e55)
Right click and open Terminal and do sudo apt-get install hydra:
![image](https://github.com/MatSM32/Cloud-Red-Blue-Team-/assets/150560131/63015b3a-b0e1-4f05-bd3f-bb7b8dff095f)

Now on your vulnerable machine make sure that your firewall wall is disabled and WAF inbound rule for rdp is letting any ip address access your machine. Next go to command prompt and do ipconfig to find the ip address:
![image](https://github.com/MatSM32/Cloud-Red-Blue-Team-/assets/150560131/a43bf0f3-9eae-429f-8b77-c6ecde171a90)

Switch back to Kali Link with the Ipv4 address and now do a nmap scan:
![image](https://github.com/MatSM32/Cloud-Red-Blue-Team-/assets/150560131/4d618236-7e7d-4590-bc0a-134bffbda1e8)
As you can see there are many open ports due to the firewall configuration. Here we are looking to see if the rdp port is open. If you didnt know the port number for rdp is 3389, so yes it is open.

Next make a directory by using mkdir BFA and then go into it by doing cd BFA
![image](https://github.com/MatSM32/Cloud-Red-Blue-Team-/assets/150560131/6eee9f62-85f0-46ff-ade1-dde81436dd8e)
Now create two text files called users.txt and pass.txt. For example nano users.txt. Then use chat.gpt to generate random usernames and passwords and then paste it into the file. Press control + x, then press y, and then enter. Then check to see if your files have the usernames or passwords by doing cat users.txt or pass.txt
![image](https://github.com/MatSM32/Cloud-Red-Blue-Team-/assets/150560131/7cf5dff1-5a39-4742-8bde-4508c62b9599)

To perform the dictionary attack type this command:
![image](https://github.com/MatSM32/Cloud-Red-Blue-Team-/assets/150560131/80117edf-7d14-4d77-81a7-2828425c39a1)
The -o found_credntials.txt is the text file that will have the user and pass if it is found after the dictionary attack.

Now go to Ubuntu and start up Splunk and use this query if you want to see how many attempts were made and the location it came from:
![image](https://github.com/MatSM32/Cloud-Red-Blue-Team-/assets/150560131/9d91b7ac-f1e2-4450-bfcb-9fd97138379c)

If you want to graph it on a Map then use this query instead:

![image](https://github.com/MatSM32/Cloud-Red-Blue-Team-/assets/150560131/640c349f-44a9-4078-bbcc-1dd3f5d51faa)

There are multiple ways to defend against this you can use a NIDS to detect and block ip addresses that are not allowed, make a rule in the firewall that only allows certain ip addresses to access rdp, make an alert in your SIEM to be able to identify a dictionary attack. Im only going to be showing how to make an alert and only making a firewall rule that will ony allow certain ip addresses.

Make an alert by pressing save as and make sure you have a query in the search bar:
![image](https://github.com/MatSM32/Cloud-Red-Blue-Team-/assets/150560131/80caa27d-cbd2-4ff1-9c79-033c6d82f297)

Make a scheduled alert like this:
![image](https://github.com/MatSM32/Cloud-Red-Blue-Team-/assets/150560131/a959ff33-5e86-4a2d-ad67-db5989928fdf)
![image](https://github.com/MatSM32/Cloud-Red-Blue-Team-/assets/150560131/11b8271d-fcdf-40cf-b4c1-5fb122608194)









