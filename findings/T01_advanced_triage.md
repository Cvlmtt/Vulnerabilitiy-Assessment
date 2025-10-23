# Findings — Dettaglio Scansione Avanzata (T.01 - 192.168.56.105)

**Scansione Eseguita:** `nmap -sV --script=vulners,vuln -oA VA_lab/scans/metasploitable_nmap_script 192.168.56.105`  
**Data Esecuzione:** 2025-10-22  
**File di Riferimento:** `scans/nmap/T01_advanced/T01_advanced_scan.*`

---

## 1. Riepilogo Vulnerabilità Critiche e Alte (T.01)

| ID Finding | Porta | Servizio | Versione | Vulnerabilità Chiave (CVE/Issue) | Gravità (CVSS) | PoC (Prossimo Step) |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **V-C01** | 21/tcp | vsftpd | 2.3.4 | **Backdoor vsFTPd (CVE-2011-2523)**. Dimostrata RCE con privilegi di root. | **CRITICA** (CVSS v3.x) | Metasploit |
| **V-C02** | 1099/tcp | java-rmi | GNU Classpath grmiregistry | **RMI Registry RCE** (Insecure Default Configuration, **CVE-2020-9761**). Permette RCE. | **CRITICA** (CVSS v3.x) | Metasploit |
| **V-C03** | 1524/tcp | bindshell | Metasploitable root shell | **Configurazione Insecure:** Shell di root in ascolto. Accesso diretto. | **CRITICA** | Netcat/Telnet |
| **V-A04** | 6667/tcp | UnrealIRCd | N/A | **UnrealIRCd Backdoor** (**CVE-2010-2075**). Permette RCE (Riferimento 2010/Jun/277). | **ALTA** (CVSS v2.0) | Metasploit |
| **V-A05** | 5432/tcp | postgresql | 8.3.0 - 8.3.7 | **Vulnerabilità SSL/TLS Multiple** (POODLE, CCS Injection, DH Weakness, **CVE-2014-3566**). | **MEDIA** (CVSS v.20) | Test Credenziali di Default |
| **V-A06** | 80/tcp | Apache httpd | 2.2.8 | **Slowloris DoS (CVE-2007-6750)** / Potenziali link a SQLi. | **MEDIA** (CVSS v2.0) | N/A (Focus RCE/Accesso) |
| **V-A07** | 22/tcp, 23/tcp | ssh, telnet | OpenSSH 4.7p1, Linux telnetd | **Accesso Credenziali Deboli**. Servizi legacy/obsoleti. | **ALTA** | Tentativo di Brute-force/Default |

---