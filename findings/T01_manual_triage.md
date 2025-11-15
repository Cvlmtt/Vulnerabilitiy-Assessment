# FTP Service (Port 21)

## Vulnerabilità confermate

### 1. vstfpd 2.3.4 Backdoor (CVE-2011-2523)
Severity: Critical
Tool: Nmap, OpenVAS
Description: versione di vsftpd nota con backdoor che permette l'apertura di una shell remota
Evidenza:
- Nmap: rilevata versione (2.3.4) con backdoor - CVE-2011-2523
- OpenVAS: Conferma CVE-2011-2523
Check: confermata manualmente
![vsftpd backdoor exploit execution](img/vsftpd_backdoor_exploit.png)

### 2. Anonymous login enable
Severity: High
Tool: Nuclei
Description: Possibilità di acceso anonimo ad FTP con username="anonymous" e password="somepass"
Check: `ftp IP_TARGET` and `username=anonymous; password=SOME_PASSWORD`

### Vulnerabilità escluse
- DoS vsftpd 3.0.3: versione non corrispondente
- CVE-2021-1419: vulnerabilità SSH non FTP

### 3. Weak/Know login credentials
Severity: Medium
Tool: OpenVAS
Description: Possibilità di accesso ad FTP con credenziali deboli o note (msfadmin:masfadmin)
Check: `ftp msfadmin@IP_TARGET` and `passoword=msfadmin`

# SSH Service (Port 22)

## Vulnerabilità confermate

### 1. Outdated and weak SSH cryptographic configuration
Severity: Medium
Tool: Nuclei, OpenVAS
Description: il server supporta algoritmi di cifratura e MAC obsoleti come `diffie-hellman-group1-sha1`, `hmac-sha1`, `cbc-mode` 
Check: `nmap --script ssh2-enum-algos -p22 192.168.56.105`

### 2. Weak Authentication Configuration
Severity: Low/Medium
Tool: Nuclei
Description: Il server SSH accetta autenticazione via password, potenzialmente soggetto a brute-force
Check: testare credenziali comuni o default

### Informational
- SSH Version Disclosure  
- SSH Auth Methods Enumeration