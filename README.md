# TryHackMe

![image](https://user-images.githubusercontent.com/64806211/127160623-e5ab7c23-2ef3-488d-9eb4-ab02a33457f8.png)



### **Pickle Rick**


![1](https://user-images.githubusercontent.com/64806211/127137486-7e02d153-6524-4075-bc2a-19f00ba29862.png)


Introduction 


Hello fellow Hackers! Another day with another CTF machine for my tryhackme writup series. A Rick and Morty CTF. We need to help Rick to turn back into a human!. This Rick and Morty themed challenge requires you to exploit a webserver to find 3 ingredients that will help Rick make his potion to transform himself back into a human from a pickle. 



![2](https://user-images.githubusercontent.com/64806211/127138177-2160039a-506e-4587-97c6-e119a389954b.gif)


help!!!




After hitting the deploy button we now have our IP address (before starting, check whether the IP is live by pinging ).


**#Enum/Recon**

I have used Nmap to check for open ports and services.

Command used: nmap -A -sV <machine IP>
  
  
  ![image](https://user-images.githubusercontent.com/64806211/127140547-1030aa7b-83b2-49b0-9619-764eba783fe4.png)

  You can access this machine from this url: https://tryhackme.com/room/picklerick






From the nmap scan result we came to know that two ports are open and they are, 22/tcp ssh and 80/tcp http. Let’s check out port 80 on the browser.


Well, seems like Rick is in danger!! In the webpage, I couldn’t find any clue but when I viewed the page source, I got the username: R1ckRul3s 


![image](https://user-images.githubusercontent.com/64806211/127140711-cc23660f-37f1-4267-9267-ad745d8d53af.png)


Since we got the username, let’s start looking for password using brute force techniques. First, I did the directory brute forcing with my favorite tool Gobuster and got /robots.txt with status: 200.
  
  
  
  
  command used: gobuster dir -u <url> -w /usr/share/dirb/wordlists/common.txt 
  
  
  
  ![image](https://user-images.githubusercontent.com/64806211/127141466-2e46a17a-289e-4591-888c-ccc3b0d34f47.png)

  
  When I checked in my browser, I think I got the password!!
  
  
  
  ![image](https://user-images.githubusercontent.com/64806211/127141490-c7bbb2ea-ba4c-42be-8934-ee78ad9cd99a.png)

  
  
  password: Wubbalubbadubdub

  
  
  ![image](https://user-images.githubusercontent.com/64806211/127141536-a7a8ff54-b9d8-4d75-8e00-e094f5b936ad.png)

  
  
  With the collected login credentials, I tried to connect to the server via SSH and the permission was denied.
  
  
  
  ![image](https://user-images.githubusercontent.com/64806211/127141587-296e2f34-1616-4860-a7fc-af96d870d197.png)

  
  
  ![image](https://user-images.githubusercontent.com/64806211/127141601-23919337-ee55-4764-8306-0898773212be.png)

  
  
  Well at this point I felt pretty stupid as rick said and then realized that enumeration is the key. So, I looked around in /assets in my browser and this is what I got…A big nothing except gifs and images and nothing interesting.
  
  
  ![image](https://user-images.githubusercontent.com/64806211/127141647-a2441745-c83f-47da-aac6-391d5a2ea6bb.png)

  
  
  Now I tried with Nikto tool to get even more results and observed that there is /login.php. 
  
  
  command used: nikto -h <machine IP> 
  
  
  ![image](https://user-images.githubusercontent.com/64806211/127141704-444503fa-9e5e-4a8b-81e9-f12eeeae28ad.png)
 
  
  
  
  I just tried it and bingo! I got the login page. 
  
  
  ![image](https://user-images.githubusercontent.com/64806211/127141745-359c8b38-809a-4893-897f-9a2b10ae19e1.png)
 
  
  
  
  Login Credentials 
  
  
  
  username: R1ckRul3s 
  
  
  password: Wubbalubbadubdub 
  
  
  
  ![image](https://user-images.githubusercontent.com/64806211/127141809-d4eba9e4-177c-4ec7-bd5f-9a4b4c6b325e.png)
 
  
  
  
  **#Exploit**
  
  
   Now, we should execute some linux commands get the ingredients flags. 
  
  
  command used: ls -la 
  
  
  ![image](https://user-images.githubusercontent.com/64806211/127141932-f9404915-126a-4738-b7dd-6a159b6fd579.png)
 
  
  
  
  We got the .txt file. If we use cat command, we won’t get the flag because the command is disabled. 

  
  
  ![image](https://user-images.githubusercontent.com/64806211/127141964-ded8748a-b69c-485a-9b4c-31c714be95ca.png)

  
  
  So, I used less command instead of cat and got the first flag. 
  
  
  command used: less Sup3rS3cretPickl3Ingred.txt 
  
  
  
  mr. meeseek hair

 
  
  ![image](https://user-images.githubusercontent.com/64806211/127142065-f6b77d8a-5dca-47b9-9eae-de70b11c2265.png)

  
  
  For the second flag the command used: less /home/rick/’second ingredients’ 
  
  
  1 jerry tear 
  
  
  
  ![image](https://user-images.githubusercontent.com/64806211/127142115-ae3729b4-d82e-43e2-87c9-b17b6bb9e8dd.png)
 
  
  Now it’s time for 3rd and the last flag. To get this, I just checked the user permission by typing sudo -l and we can see that there is no restrictions and the existing user can run commands as sudo. 
  
  
  
  
  
  ![image](https://user-images.githubusercontent.com/64806211/127142158-ac0ea3a1-0103-4450-ae6c-77fb0ac8f6c2.png)
 
  
  
  
  for the 3rd flag, the command used: sudo less /root/3rd.txt
  
  
  
  
  3rd ingredients: fleeb juice
  
  
  ![image](https://user-images.githubusercontent.com/64806211/127142222-512b73c5-75ac-4035-80c0-936fadca77ae.png)
 
  
  
  
  
  ![image](https://user-images.githubusercontent.com/64806211/127142246-d4c8e8ee-6e62-446c-a21d-3b7e10223a0e.png)
  
  
  
  
  Finally!!! all the three flags were captured and the task is completed successfully. Thanks for reading and hope you enjoyed too. as I always mention in my every blog, suggestions are always welcome and open for discussion so that we can discuss about other methods to complete the same task (exchanging ideas).  
  
  
  
  
                         **Happy Hacking….**
