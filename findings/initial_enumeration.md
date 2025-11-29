# Findings — Initial Enumeration

**Scansione eseguita:** `nmap -Pn -sS -sV -O -T4 -oA --reason discovery 192.168.56.1/24`  
**Data esecuzione:** 2025-10-18  
**File di riferimento:** `scans/discovery.nmap`, `scans/discovery.xml`, `scans/discovery.gnmap`.  

---

## 1. Sintesi generale
La scansione ha rilevato tre host attivi sulla subnet `192.168.56.1/24`. Dei cinque host, due mostrano un elenco dettagliato di porte/servizi (.105 e .106), mentre tre rispondono come host VirtualBox / infrastruttura di rete con porte tutte in stati ignorati/chiusi o fingerprint non univoci (.1, .100, .104). L'host .104 non viene riportato in quanto si tratta della VM Kali Linux utilizzata per la scansione. 

---

## 2. Tabella sintetica degli host (dati estratti da `scans/discovery.nmap`)

| IP | Porte / Servizi principali | OS / note di fingerprint |
|----|----------------------------|--------------------------|
| 192.168.56.1 | ssh/22 (closed), dnsmasq/53 | MAC: `52:54:00:FB:8D:F4` (QEMU virtual nic). No exact OS matches for host. Network Distance: 1 hop. |
| 192.168.56.105 | 21, 22, 23, 25, 53, 80, 111, 139, 445, 512, 513, 514, 1099, 1524, 2049, 2121, 3306, 5432, 5900, 6000, 6667, 8009, 8180 | MAC Address: `52:54:00:C2:D5:7C` (QEMU virtual nic). Device type: general purpose. Running: Linux 2.6.x (OS CPE: cpe:/o:linux:linux_kernel:2.6). Network Distance: 1 hop. Service Info: Hosts:  metasploitable.localdomain, irc.Metasploitable.LAN; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel |
| 192.168.56.104 | All 1000 scanned ports on kali.labnet (192.168.56.104) are in ignored states. Not shown: 1000 closed tcp ports (reset) | Too many fingerprints match this host to give specific OS details
Network Distance: 0 hops |


---

## 3. Dettaglio host — 192.168.56.105
**File di riferimento:** `scans/discovery.nmap`.

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

## 4. Riferimenti evidenza
- `scans/discovery.nmap` — output Nmap (raw).   

---
