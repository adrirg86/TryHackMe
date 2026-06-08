### TakeOver - THM🏴󠁧󠁢󠁥󠁮󠁧󠁿


#### 1. First we're going to write on /etc/hosts the target IP.
```bash
nano /etc/hosts
```

<img width="894" height="800" alt="image" src="https://github.com/user-attachments/assets/d9464c6f-e6d9-4754-9da6-212b7e217bf0" />


#### 2. Then we need to prove the conexion with a ping 
```bash
ping -c3 10.129.182.213
```

<img width="961" height="493" alt="image" src="https://github.com/user-attachments/assets/d1218a60-5e6b-4998-8de1-3904a6ce0cb3" />


####3. We're going to search the open ports with nmap.
```bash
 nmap -sCV 10.129.182.213
```

<img width="966" height="826" alt="image" src="https://github.com/user-attachments/assets/4e0310f2-d3f0-4073-84bf-4bf582db10b0" />



#### 4. Now we do a ffuz of hidden ports
```bash
ffuf -w /usr/share/wordlists/dirb/common.txt -u "http://10.129.182.213" -H "Host: FUZZ.futurevera.thm" -fw 1
```

<img width="984" height="726" alt="image" src="https://github.com/user-attachments/assets/5457d6c7-2f63-4727-a495-47a20b11e8fb" />


#### 5. We add the new website for /etc/hosts
```bash
echo 10.129.182.213 portal.future.thm >> /etc/hosts
```

<img width="887" height="82" alt="image" src="https://github.com/user-attachments/assets/40dee881-aef3-4dfb-8a9d-d0ee7b503221" />


#### 6. Now we ffuz the https.
```bash
ffuf -w /usr/share/wordlists/dirb/common.txt -u "https://10.129.182.213" -H "Host: FUZZ.futurevera.thm" -fw 1511
```

<img width="1069" height="521" alt="image" src="https://github.com/user-attachments/assets/b4eeace2-7d33-43d9-a87a-3b2a315ab946" />


#### 7. We add the new websites to /etc/hosts
```bash
echo 10.129.182.213 blog.futurevera.thm >> /etc/hosts
echo 10.129.182.213 support.futurevera.thm >> /etc/hosts
```

<img width="830" height="247" alt="image" src="https://github.com/user-attachments/assets/dc7e2f69-7c29-4de0-b3fe-b8c65115867f" />


#### 8. We found a interesnt dns and we're going to add to the hosts.


<img width="1382" height="864" alt="image" src="https://github.com/user-attachments/assets/0a6be5e4-c1c5-47d4-a527-5bc12a62e066" />


#### 8.2 Then we added to the hosts.
```bash
echo 10.129.182.213 secrethelpdesk934752.support.futurevera.thm >> /etc/hosts
```

<img width="924" height="196" alt="image" src="https://github.com/user-attachments/assets/4859f46b-8157-4d26-b930-532b182e5ee2" />


#### 9. We go into the website and we go it the flag.


<img width="1343" height="671" alt="image" src="https://github.com/user-attachments/assets/5679a98c-6e02-4d58-975a-29ab3023e914" />

