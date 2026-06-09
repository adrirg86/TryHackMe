### Corridor-TryHackMe🏴󠁧󠁢󠁥󠁮󠁧󠁿

#### 1. First we're going to prove the conexion with a ping.
```bash
 ping -c3 10.129.186.78
```

<img width="959" height="159" alt="image" src="https://github.com/user-attachments/assets/660d1575-bbe2-4c3a-a1af-9e9f1013374b" />


#### 2. Now we are going to search the open ports at the machine.
```bash
 nmap -sCV 10.129.186.78
```

<img width="963" height="251" alt="image" src="https://github.com/user-attachments/assets/650a6848-2c73-498e-a507-15b321dc5c02" />

#### 3. We see that the port 80 is open in a http, we look in.

<img width="957" height="868" alt="image" src="https://github.com/user-attachments/assets/15fa87d7-bd59-4dc9-85b8-e35ea6931830" />


#### 4. We're going to do a ferobxbuster for search the hidden directories.
```bash
feroxbuster -u http://10.129.186.78 -w /usr/share/wordlists/dirb/common.txt
```

<img width="918" height="602" alt="image" src="https://github.com/user-attachments/assets/001e98bf-f077-45e3-b973-d47e00c4ed73" />

#### 5. Now we collect all the hashes
```bash
curl http://10.129.186.78 | grep 'alt' | cut -d '"' -f4 > hash.txt
```

<img width="911" height="185" alt="image" src="https://github.com/user-attachments/assets/bac5b921-1d10-45a9-95a4-304d742c3b5b" />


#### 6. WE crack the hash with john.
```bash
john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```

<img width="918" height="446" alt="image" src="https://github.com/user-attachments/assets/0d4d9de4-8361-4819-80a8-6961cc794d68" />


#### 7. The vulnerability is at -n 14 or 0.
```bash
echo -n 14 | md5sum
echo -n 0 | md5sum
```

<img width="820" height="136" alt="image" src="https://github.com/user-attachments/assets/9108bfbd-c9a8-4cfc-9ecd-616ab37accbc" />


#### 7.A The first doesn't found.

<img width="998" height="328" alt="image" src="https://github.com/user-attachments/assets/cf6a744b-1a1e-4d0a-a1ec-0d1ef78c363c" />


#### 7.B The second is the key.

<img width="993" height="530" alt="image" src="https://github.com/user-attachments/assets/25216c1a-ca38-45f6-a6a5-7ae5323bd9b0" />


<img width="823" height="489" alt="image" src="https://github.com/user-attachments/assets/08be1315-f56f-4082-8702-df6eb6540510" />


