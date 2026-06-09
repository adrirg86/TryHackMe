### Neighbour-TryHackMe🏴󠁧󠁢󠁥󠁮󠁧󠁿

#### 1. First we're going to turn on the openvpn for connect to the machine.
```bash
sudo openvpn eu-west-3-adrirg737-regular-2.ovpn
```

<img width="961" height="236" alt="image" src="https://github.com/user-attachments/assets/aa22e9be-f7f1-4cc7-8547-357157559f38" />


#### 2. Then we are goin to prove the conexion with a ping.
```bash
ping -c3 10.129.190.217
```

<img width="966" height="244" alt="image" src="https://github.com/user-attachments/assets/8809ea5e-3ed2-4dc8-90b0-7d9bb8b17b42" />


#### 3. Then we're going to search the open ports with nmap.
```bash
nmap -sCV 10.129.190.217
```

<img width="956" height="466" alt="image" src="https://github.com/user-attachments/assets/43066b6f-20d3-4532-91af-b742b820f62d" />


#### 4. Then we goes to the website and i prove sql inyection that is disabled and i try the user guest and passwd guest.

<img width="962" height="353" alt="image" src="https://github.com/user-attachments/assets/1f3c009c-1b3f-4458-885d-cb34b1330f9a" />


#### 5. Now we do a gobuster for search the hidden directories.
```bash
 gobuster dir -u http://10.129.190.217/ \
                            -w /usr/share/seclists/Discovery/Web-Content/common.txt \
                            -x html,zip,php,txt,bak,sh,asp,aspx \
                            -b 404 -t 60
```

<img width="969" height="554" alt="image" src="https://github.com/user-attachments/assets/e9f430ca-556e-4cc8-bef6-d833025217a9" />


#### 6. We change the user guest to admin and we have the flag

<img width="953" height="278" alt="image" src="https://github.com/user-attachments/assets/84e93898-c1c8-4ea8-b463-6f3868479038" />


<img width="801" height="650" alt="image" src="https://github.com/user-attachments/assets/484cab5f-5a36-4c9a-88c4-c7c8b0cfca14" />












