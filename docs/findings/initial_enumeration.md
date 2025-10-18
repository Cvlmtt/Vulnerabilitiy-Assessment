# Findings — Initial Enumeration

**Scansione eseguita:** `nmap -Pn -sS -sV -O -T4 --reason -oA scans/initial_scan 192.168.56.1/24`  
**Data esecuzione:** 2025-10-18  
**File di riferimento:** `scans/initial_scan.nmap`, `scans/initial_scan.xml`, `scans/initial_scan.gnmap`.  
(Estratti e output completi sono disponibili nella cartella `scans/` del repository)

---

## 1. Sintesi generale
La scansione ha rilevato **5 host attivi** sulla subnet `192.168.56.0/24`. Dei cinque host, due mostrano un elenco dettagliato di porte/servizi (.105 e .106), mentre tre rispondono come host VirtualBox / infrastruttura di rete con porte tutte in stati ignorati/chiusi o fingerprint non univoci (.1, .100, .104). L'host .104 non viene riportato in quanto si tratta della VM Kali Linux utilizzata per la scansione. 

---

## 2. Tabella sintetica degli host (dati estratti da `scans/initial_scan.nmap`)

| IP | Hostname (se presente) | Porte / Servizi principali (da Nmap) | OS / note di fingerprint | Evidenze |
|-----|------------------------|---------------------------------------|--------------------------|----------|
| 192.168.56.1 | — | Tutte le 1000 porte scansionate in stati ignorati. Not shown: 846 closed, 147 filtered, 7 filtered (admin-prohibited). MAC: `0A:00:27:00:00:00`. | Too many fingerprints match — info OS non univoca. Network Distance: 1 hop. |
| 192.168.56.100 | — | Tutte le 1000 porte in stati ignorati (1000 filtered tcp). MAC: `08:00:27:A0:92:4B` (Oracle VirtualBox NIC). | Too many fingerprints match — probabilmente VM guest VirtualBox. Network Distance: 1 hop. |
| 192.168.56.105 | metasploitable-like host | **Numerose porte aperte:** 21/tcp (vsftpd 2.3.4), 22/tcp (OpenSSH 4.7p1), 23/tcp (telnetd), 25/tcp (Postfix), 53/tcp (BIND 9.4.2), 80/tcp (Apache 2.2.8), 111, 139, 445 (Samba smbd 3.x–4.x), 2049 (NFS), 2121 (ProFTPD 1.3.1), 3306 (MySQL 5.0.51a), 5432 (PostgreSQL 8.3.x), 5900 (VNC), 6000 (X11), 6667 (IRC UnrealIRCd), 8009 (AJP), 8180 (Tomcat). | Device type: general purpose. Running: Linux 2.6.x (OS CPE: cpe:/o:linux:linux_kernel:2.6). Network Distance: 1 hop. Service info: host names e servizi multipli (metasploitable.localdomain ecc.). |
| 192.168.56.106 | web-target / Apache | 80/tcp open — Apache httpd 2.4.58. (solo porta 80 mostrata come open). | Device type: general purpose|router. OS detection: Linux 4.x/5.x or MikroTik RouterOS 7.x (range di fingerprint multiple). Network Distance: 1 hop. |

---

## 3. Dettaglio host — 192.168.56.105
**File di riferimento:** `scans/initial_scan.nmap`, `scans/nuclei_results.txt`.

**Servizi:**
- 21/tcp open — vsftpd 2.3.4  
- 22/tcp open — OpenSSH 4.7p1 (protocol 2.0)  
- 23/tcp open — telnetd  
- 25/tcp open — Postfix smtpd  
- 53/tcp open — ISC BIND 9.4.2  
- 80/tcp open — Apache httpd 2.2.8 (Ubuntu, DAV/2)  
- 111/tcp open — rpcbind  
- 139/tcp open — Samba smbd (workgroup: WORKGROUP)  
- 445/tcp open — Samba smbd  
- 512/513/514 tcp — rexecd/login/shell (netkit)  
- 1099/tcp open — java-rmi (GNU Classpath grmiregistry)  
- 1524/tcp open — bindshell (Metasploitable root shell)  
- 2049/tcp open — NFS  
- 2121/tcp open — ProFTPD 1.3.1  
- 3306/tcp open — MySQL 5.0.51a-3ubuntu5  
- 5432/tcp open — PostgreSQL 8.3.x  
- 5900/tcp open — VNC (RFB 003.003)  
- 6000/tcp open — X11 (access denied)  
- 6667/tcp open — UnrealIRCd (IRC)  
- 8009/tcp open — ajp13 (Apache JServ)  
- 8180/tcp open — Apache Tomcat/Coyote JSP engine 1.1

**Osservazioni dai tool nuclei:**  
- Evidenze di `phpmyadmin-panel`, `phpinfo-files` e paths sensibili (es. `/phpMyAdmin/`, `/phpinfo.php`) che indicano presenza di pannelli e pagine informative.  
- Numerose issue critiche/alte legate a PostgreSQL (default logins, file reads, list databases, list users, version detect) con credenziali/database predefinite rilevate (es. user/password `postgres`), e hash password estratto segnalato.  
- Rilevata presenza di SMB (Samba 3.0.20-Debian) e SMBv1 supportato.  
- Rilevata la CVE-2012-1823 (LFI/allow_url_include style) e CVE-2020-1938 (AJP ghost / tomcat ajp critical) riferita alla porta 8009.  
(Elenco completo e linee dei template nuclei sono nel file `scans/nuclei_results.txt`).

**Note su rischio / contesto:** host fortemente vulnerabile (profili tipici di Metasploitable). Le evidenze nuclei mostrano credenziali e letture file sensibili su PostgreSQL; AJP e versioni datate di servizi web sono presenti.

---

## 4. Dettaglio host — 192.168.56.106 (web server Apache)

**File di riferimento:** `scans/initial_scan.nmap`, `scans/nikto_web-target-1.txt`, `scans/nuclei_results.txt`.

**Servizi:**
- 80/tcp open — Apache httpd 2.4.58

**Risultati Nikto (sintesi):**  
Dal file `nikto_web-target-1.txt` sono emerse le seguenti problematiche principali su `http://192.168.56.106`:  
- Mancanza di header di sicurezza rilevanti: X-Frame-Options, X-Content-Type-Options, Content-Security-Policy (e altri).  
- Directory indexing abilitato su più path (diverse varianti di path testing mostrano listing).  
- OPTIONS mostra metodi consentiti: GET, POST, OPTIONS, HEAD.  
(Dettagli completi e righe raw sono in `scans/nikto_web-target-1.txt`).

**Risultati nuclei (sintesi):**  
- Rilevamento generico di WAF/Apache generico (waf-detect: apachegeneric) riportato sia per .106 che per .105.

**Note su rischio / contesto:** l’host appare un web server aggiornato (Apache 2.4.58) ma con configurazione HTTP che manca di header di sicurezza e permette directory listing: condizioni che facilitano ricognizione e possibile scoperta di risorse sensibili.

---

## 5. Host infrastrutturali — 192.168.56.1, 192.168.56.100, 192.168.56.104

**Sintesi (da Nmap):**
- `192.168.56.1` — tutte le porte nello stato ignorato; MAC `0A:00:27:00:00:00`. Fingerprint OS non univoco. Probabile ruolo di infrastruttura/bridge Host-Only.
- `192.168.56.100` — tutte le porte filtered; MAC `08:00:27:A0:92:4B` (VirtualBox NIC). Probabile VM guest non esposta o con firewall che filtra le porte.
- `192.168.56.104` — tutte le porte chiuse; Network Distance: 0 hops (probabile entry locale/host). Fingerprint non univoco.

(Estratti completi nel file `scans/initial_scan.nmap`).

---

## 6. Riferimenti evidenza
- `scans/initial_scan.nmap` — output Nmap (raw).  
- `scans/initial_scan.xml` — (se presente) XML Nmap per analisi strutturata.  
- `scans/nikto_web-target-1.txt` — output completo Nikto per 192.168.56.106. :contentReference[oaicite:11]{index=11}  
- `scans/nuclei_results.txt` — output nuclei (template hits) con riferimenti a CVE e possibili exploit su 192.168.56.105 e 192.168.56.106. :contentReference[oaicite:12]{index=12}

---

## 7. Note finali sul contenuto del file
Questo documento riassume fedelmente le informazioni ricavate dall'output `scans/initial_scan.nmap` e dai file di scan automatici (Nikto, Nuclei). Per consultare le singole righe raw e i dettagli dei template trovati, aprire i file indicati nella cartella `scans/`.

---
