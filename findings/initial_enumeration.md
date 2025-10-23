# Findings — Initial Enumeration

**Scansione eseguita:** `nmap -Pn -sS -sV -O -T4 -oA --reason scans/initial_scan 192.168.56.1/24`  
**Data esecuzione:** 2025-10-18  
**File di riferimento:** `scans/initial_scan.nmap`, `scans/initial_scan.xml`, `scans/initial_scan.gnmap`.  

---

## 1. Sintesi generale
La scansione ha rilevato **5 host attivi** sulla subnet `192.168.56.1/24`. Dei cinque host, due mostrano un elenco dettagliato di porte/servizi (.105 e .106), mentre tre rispondono come host VirtualBox / infrastruttura di rete con porte tutte in stati ignorati/chiusi o fingerprint non univoci (.1, .100, .104). L'host .104 non viene riportato in quanto si tratta della VM Kali Linux utilizzata per la scansione. 

---

## 2. Tabella sintetica degli host (dati estratti da `scans/initial_scan.nmap`)

| IP | Hostname (se presente) | Porte / Servizi principali | OS / note di fingerprint |
|-----|------------------------|---------------------------------------|--------------------------|
| 192.168.56.1 | — | Tutte le 1000 porte scansionate in stati ignorati. Not shown: 846 closed, 147 filtered, 7 filtered (admin-prohibited). | MAC: `0A:00:27:00:00:00`. Too many fingerprints match — info OS non univoca. Network Distance: 1 hop. |
| 192.168.56.100 | — | Tutte le 1000 porte in stati ignorati (1000 filtered tcp). | MAC: `08:00:27:A0:92:4B` (Oracle VirtualBox NIC). Too many fingerprints match — probabilmente VM guest VirtualBox. Network Distance: 1 hop. |
| 192.168.56.105 | metasploitable-like host | 21, 22, 23, 25, 53, 80, 111, 139, 445, 512, 513, 514, 1099, 1524, 2049, 2121, 3306, 5432, 5900, 6000, 6667, 8009, 8180 | MAC Address: `08:00:27:C0:9E:18` (PCS Systemtechnik/Oracle VirtualBox virtual NIC). Device type: general purpose. Running: Linux 2.6.x (OS CPE: cpe:/o:linux:linux_kernel:2.6). Network Distance: 1 hop. Service info: host names e servizi multipli (metasploitable.localdomain ecc.). |
| 192.168.56.106 | web-target / Apache | 80/tcp open — Apache httpd 2.4.58. (solo porta 80 mostrata come open). | MAC Address: `08:00:27:F1:C4:7C` (PCS Systemtechnik/Oracle VirtualBox virtual NIC). Device type: general purpose or router. OS detection: Linux 4.x/5.x or MikroTik RouterOS 7.x (range di fingerprint multiple). Network Distance: 1 hop. |

---

## 3. Dettaglio host — 192.168.56.105
**File di riferimento:** `scans/initial_scan.nmap`.

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

---

## 4. Dettaglio host — 192.168.56.106 (web server Apache)

**File di riferimento:** `scans/initial_scan.nmap`.

**Servizi:**
- 80/tcp open — Apache httpd 2.4.58

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
- `scans/initial_scan.xml` — XML Nmap per analisi strutturata.  

---

## 7. Note finali sul contenuto del file
Questo documento riassume fedelmente le informazioni ricavate dall'output `scans/initial_scan.nmap`. Per consultare le singole righe raw e i dettagli dei template trovati, aprire i file indicati nella cartella `scans/`.

---
