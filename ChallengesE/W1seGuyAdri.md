### W1seGuy-TryHackMe🏴󠁧󠁢󠁥󠁮󠁧󠁿

*Your friend told me you were wise, but I don't believe them. Can you prove me wrong?*

*When you are ready, click the Start Lab Machine button to fire up the Lab Machine. Please allow 3-5 minutes for the VM to start fully.*

*The server is listening on port 1337 via TCP. You can connect to it using Netcat or any other tool you prefer.*

#### 1. First we're going to prove the information with a ping.
```bash
ping -c3 10.129.148.39
```

<img width="962" height="270" alt="image" src="https://github.com/user-attachments/assets/2c0bc635-c116-47d9-87cd-a07f8b8a97fc" />


#### 2. Now we scan the open ports with nmap.
```bash
nmap -sCV 10.129.148.39
```

<img width="961" height="295" alt="image" src="https://github.com/user-attachments/assets/cd3ae59e-027d-4087-953c-ed131f1411b3" />


#### 3. We're going to execute the python
```bash
echo "HTB{bandera_de_prueba_local}" > flag.txt
python3 source-1705339805281.py
```

<img width="811" height="63" alt="image" src="https://github.com/user-attachments/assets/b3e670f7-75b3-4487-9b1c-b1129f471326" />


#### 4. Now we open another terminal to listen with `nc` in the port 1337 that the statement told us.
```bash
killall nc
fuser -k 1337/tcp
nc -lvp 1337
```

<img width="886" height="293" alt="image" src="https://github.com/user-attachments/assets/299c0adb-b431-4934-b26d-b713a9329553" />


#### 5. Now we're going to use the pwtools.

##### 5.1 Now we create an python to know the solution. `nano solucion.py` And we execute.
```python
from pwn import *
import string

# IP y puerto de la máquina W1se Guy de TryHackMe
IP_OBJETIVO = '10.129.148.39'  # <-- Asegúrate de que esta sea la IP actual de tu máquina de THM
PUERTO_OBJETIVO = 1337

context.log_level = 'error'
flag_start = "THM{" 
caracteres_posibles = string.ascii_letters + string.digits

print("[*] Atacando la máquina W1se Guy de TryHackMe...")

for posible_quinto_char in caracteres_posibles:
    try:
        io = remote(IP_OBJETIVO, PUERTO_OBJETIVO)
        
        # 1. Leer el hex
        io.recvuntil(b"flag 1: ")
        hex_cifrado = io.recvline().strip().decode()
        bytes_cifrados = bytes.fromhex(hex_cifrado)
        
        # 2. Reconstruir los primeros 4 caracteres de la clave con "THM{"
        clave_parcial = ""
        for i in range(4):
            clave_parcial += chr(bytes_cifrados[i] ^ ord(flag_start[i]))
        
        # 3. Formar la clave completa de 5 caracteres con el intento actual
        clave_intento = clave_parcial + posible_quinto_char
        
        # 4. Enviar la clave al servidor
        io.recvuntil(b"What is the encryption key? ")
        io.sendline(clave_intento.encode())
        
        # 5. Leer la respuesta
        respuesta = io.recvall(timeout=1).decode()
        
        # Si el servidor ve la clave correcta, te soltará la Flag 2 real
        if "Congrats" in respuesta or "flag 2" in respuesta:
            print(f"\n[+] ¡LLAVE ENCONTRADA!: {clave_intento}")
            print(f"[+] {respuesta}")
            break
            
        io.close()
    except Exception as e:
        continue

print("[*] Proceso completado.")
```

<img width="930" height="141" alt="image" src="https://github.com/user-attachments/assets/e95bad12-ade2-4d9f-89e1-d2ee4accc021" />

*We got it the second key*

##### 5.2 For the first key we're going to modify the script for instead of relying on hardcoded string matching, the script connects to the target server via a TCP socket (pwntools.remote). It reads the incoming stream and dynamically isolates the long hexadecimal string using a regex-like filter (string.hexdigits), converting it directly into raw encrypted bytes.
```python
from pwn import *
import string

# Configuración de la máquina W1se Guy
IP_OBJETIVO = '10.129.148.39'
PUERTO_OBJETIVO = 1337

context.log_level = 'error'

print("[*] Conectando para extraer la FLAG 1 real de forma matemática...")

try:
    # 1. Nos conectamos al servidor
    io = remote(IP_OBJETIVO, PUERTO_OBJETIVO, timeout=3)
    
    # 2. Leemos la línea que contiene el Hex y lo limpiamos
    linea = io.recvline().decode()
    partes = linea.split()
    hex_servidor = ""
    for p in partes:
        if len(p) > 30 and all(c in string.hexdigits for c in p.strip()):
            hex_servidor = p.strip()
            break

    if not hex_servidor:
        print("[-] No se pudo encontrar el código Hexadecimal en la respuesta del servidor.")
        io.close()
        exit()

    bytes_cifrados = bytes.fromhex(hex_servidor)

    # 3. Sacamos los primeros 4 caracteres de la clave usando "THM{"
    clave_4 = "".join(chr(bytes_cifrados[i] ^ ord("THM{"[i])) for i in range(4))

    # 4. Probamos el 5º carácter buscando cuál cierra la flag con '}'
    flag_1_real = None
    for c in string.ascii_letters + string.digits + string.punctuation:
        clave_intento = clave_4 + c
        
        # Desencriptamos aplicando XOR repetido con la clave de 5 chars
        descifrado = ""
        for i in range(len(bytes_cifrados)):
            descifrado += chr(bytes_cifrados[i] ^ ord(clave_intento[i % 5]))
        
        # Si el texto termina en '}', ¡esta es la bandera real del servidor!
        if descifrado.endswith("}"):
            flag_1_real = descifrado
            break

    if flag_1_real:
        print("\n==================================================")
        print(f"[+] FLAG 1 REAL ENCONTRADA: {flag_1_real}")
        print("==================================================")
    else:
        print("[-] No se pudo determinar el 5º carácter de forma exacta.")

    io.close()

except Exception as e:
    print(f"[-] Ocurrió un error: {e}")
```

<img width="804" height="150" alt="image" src="https://github.com/user-attachments/assets/689597a9-6459-4dce-a759-1058398cd46d" />


<img width="745" height="316" alt="image" src="https://github.com/user-attachments/assets/5542d1a4-41a5-4e91-b357-687b6c8465fd" />

