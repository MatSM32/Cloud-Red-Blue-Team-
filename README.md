# Cloud-Red/Blue-Team
# Description 
To do this you will need to set up three VMs using any hypervisors of your choice. For me I created them in AWS, if you would like to do the same, follow this tutorial from this YouTuber: https://youtu.be/2cMkpLoKUj0?si=d9a7h05a51ojcyJU. In this project, you will be utilizing Kali Linux as an attacker to do a dictionary attack using Hydra and using Nmap to do scans to find what ports are open. With the security tools VM, you will be using Splunk to do correlation searches and Nessus to do vulnerability scans. Then with the Windows VM you will be using it as a vulnerable machine.
# Utilities Used
| Splunk 
| Hydra
| Nessus 
# Environments Used 
| Windows 10
| Kali Linux
| Ubuntu
# Attack/Defense Walkthrough:
Launch Kali Linux: 
![lin](https://github.com/MatSM32/Cloud-Red-Blue-Team-/assets/150560131/e9c8f4a7-9528-4a70-a7db-b9fe5bef6e55)
Right-click and open Terminal and do sudo apt-get install hydra:
![image](https://github.com/MatSM32/Cloud-Red-Blue-Team-/assets/150560131/63015b3a-b0e1-4f05-bd3f-bb7b8dff095f)

Now on your vulnerable machine make sure that your firewall wall is disabled and WAF inbound rule for rdp is letting any ip address access your machine. Next, go to command prompt and do ipconfig to find the ip address:
![image](https://github.com/MatSM32/Cloud-Red-Blue-Team-/assets/150560131/a43bf0f3-9eae-429f-8b77-c6ecde171a90)

Switch back to Kali Link with the Ipv4 address and now do a Nmap scan (doing this is active footprinting so if the vulnerable machince was connected to a NIDS it will detect you doing this scan):
![image](https://github.com/MatSM32/Cloud-Red-Blue-Team-/assets/150560131/4d618236-7e7d-4590-bc0a-134bffbda1e8)
As you can see there are many open ports due to the firewall configuration. Here we are looking to see if the rdp port is open. If you didnt know the port number for rdp is 3389, so yes it is open.

Next, make a directory by using mkdir BFA and then go into it by doing cd BFA
![image](https://github.com/MatSM32/Cloud-Red-Blue-Team-/assets/150560131/6eee9f62-85f0-46ff-ade1-dde81436dd8e)
Now create two text files called users.txt and pass.txt. For example type nano users.txt, then use chat.gpt to generate random usernames and passwords and paste it into the file then press control + x, then press y, and then enter which saves the passwords or usernames in the file. Then check to see if your files have the usernames or passwords by doing cat users.txt or pass.txt
![image](https://github.com/MatSM32/Cloud-Red-Blue-Team-/assets/150560131/7cf5dff1-5a39-4742-8bde-4508c62b9599)

To perform the dictionary attack type this command:
![image](https://github.com/MatSM32/Cloud-Red-Blue-Team-/assets/150560131/80117edf-7d14-4d77-81a7-2828425c39a1)
The -o found_credntials.txt is the text file that will have the user and pass if it is found after the dictionary attack.

Now go to Ubuntu (Security Tool Machine) start-up Splunk and use this query if you want to see how many attempts were made and the location they came from(make sure that you include your index name because my index name might be different from yours). While doing the dictionary attack, I noticed that there were at least two attempts from Poland and six from Lithuiana:
![image](https://github.com/MatSM32/Cloud-Red-Blue-Team-/assets/150560131/9d91b7ac-f1e2-4450-bfcb-9fd97138379c)

If you want to graph it on a Map then use this query instead:

![image](https://github.com/MatSM32/Cloud-Red-Blue-Team-/assets/150560131/640c349f-44a9-4078-bbcc-1dd3f5d51faa)

There are multiple ways to defend against this. I used Mitre att&ck to help me figure out ways to defend against this type of brute force attack. So from what Mitre att&ck showed, you can use a NIDS to detect and block IP addresses that are not allowed, make a rule in the firewall that only allows certain ip addresses to access RDP, make an alert in your SIEM to be able to identify a failed login attempts, and also to do a vulnerability scan to see if your machine has any weakness that can be exploited. I'm only going to be showing how to make an alert, making a firewall rule that will only allow your IP addresses and also you should make a complex password if you are not using one.

Make an alert by pressing save-as and make sure you have a query in the search bar:
![image](https://github.com/MatSM32/Cloud-Red-Blue-Team-/assets/150560131/80caa27d-cbd2-4ff1-9c79-033c6d82f297)

Make a scheduled alert like this, which will alert when failed logins are greater than 5:
![image](https://github.com/MatSM32/Cloud-Red-Blue-Team-/assets/150560131/a959ff33-5e86-4a2d-ad67-db5989928fdf)
![image](https://github.com/MatSM32/Cloud-Red-Blue-Team-/assets/150560131/11b8271d-fcdf-40cf-b4c1-5fb122608194)

So I went back to AWS and then went to the security group and only allowed my IP address:
![image](https://github.com/MatSM32/Cloud-Red-Blue-Team-/assets/150560131/094d4c43-3c9a-4ca1-bb53-2f3ad66a4fda)

Next open up Nessus, click on create a scan and click Basic Network Scan. Enter a name for the scan, then for target put the ip address of the vulnerable machine:
![image](https://github.com/MatSM32/Cloud-Red-Blue-Team-/assets/150560131/73e6abe9-4edd-498f-adca-fbafc3707cf8)

Then click credential scan and enter the username and password for the vulnerable machine. A credential scan is a more in-depth scan so it will take at least 10-15 minutes:
![image](https://github.com/MatSM32/Cloud-Red-Blue-Team-/assets/150560131/e4d6e0fb-671e-4555-9791-6cebac218808)

These are vulnerabilities that appeared in the scan:

![image](https://github.com/MatSM32/Cloud-Red-Blue-Team-/assets/150560131/a2806146-a346-408f-b4aa-fd9e93ace708)

I chose the TLS vulnerability and it gives you information on the vulnerability and what you should do to mitigate it: 
![image](https://github.com/MatSM32/Cloud-Red-Blue-Team-/assets/150560131/471159b3-f1d3-4c21-8c41-d95c6ceeb0b5)













