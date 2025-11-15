# Target T.01 (192.168.56.105)
_Analisi e triage delle vulnerabilità emerse dalle scansioni di rete e sistema._

---

## 1. Identificazione del target
| Parametro | Valore |
|------------|--------|
| Host | 192.168.56.105 |
| Sistema Operativo | Linux Ubuntu 8.04 (Metasploitable 2) |
| Servizi Principali | FTP, SSH, Telnet, HTTP, SMB, MySQL, PostgreSQL |
| Ruolo | Macchina deliberatamente vulnerabile per test |

---

## 2. Strumenti di scansione utilizzati
| Strumento | Data scansione | Versione | Report | ID format |
|-----------|----------------|----------|--------|-----------|
| **Nmap** | 2025-10-22 | 7.95 | `scans/nmap/T01_advanced/T01_advanced_scan.nmap` | T01-NMXX |
| **OpenVAS** | 2025-11-03 | 26.2.1 | `scans/openvas/openvas_report_T01-T02.pdf` | T01-OXX |
| **Nuclei** | 2025-11-03 | 3.4.10 | `scans/web/nuclei/T01_nuclei.txt` | T01-NUXX |
| **Nikto** | 2025-11-03 | 2.5.1 | `scans/web/nikto/T01_nikto.txt` | T01-NIXX |

---

## 4. Vulnerabilità rilevate

## 4. Vulnerabilità rilevate

| ID | Porta/Servizio | Gravità | Descrizione della Vulnerabilità | CVSS | Rilevato Da | CVE/Riferimenti |
|:---|:---------------|:--------|:--------------------------------|:-----|:------------|:----------------|
| **T01-001** | generale | Critica | Sistema Operativo Ubuntu Linux 8.04 a Fine Ciclo di Vita (EOL). | 10.0 | OpenVAS | N/A |
| **T01-002** | 513/tcp (login) | Critica | Servizio rlogin consente l'accesso come root senza password. | 10.0 | OpenVAS | N/A |
| **T01-003** | 512/tcp (exec) | Critica | "Servizio rexec in esecuzione |  invia credenziali (username/password) in chiaro." | 10.0 | OpenVAS | CVE-1999-0618 |
| **T01-004** | 80/tcp (http) | Critica | TWiki XSS e Command Execution. Versione obsoleta (01.Feb.2003) vulnerabile all'esecuzione di codice e XSS. | 10.0 | OpenVAS | CVE-2008-5304, CVE-2008-5305 |
| **T01-005** | 1524/tcp (bindshell) | Critica | Backdoor Rilevata (Ingreslock) / Metasploitable root shell. Conferma l'esecuzione di comandi come uid=0(root). | 10.0 | OpenVAS & Nmap Scripts | N/A |
| **T01-006** | 21/tcp (ftp) | Critica | **vsftpd 2.3.4 backdoor**. Sfruttabile per eseguire comandi arbitrari come root. | 9.8 | OpenVAS, Nmap Scripts & Nuclei | CVE-2011-2523, CVE-2021-30047 |
| **T01-NU01** | **80/tcp (http)** | Critica | **PHP RCE (Remote Code Execution)** su versione obsoleta (configurazione CGI). Nuclei conferma **CVE-2012-1823**. | 9.8 | OpenVAS & Nuclei | CVE-2012-1823 |
| **T01-007** | 6200/tcp (vsftpd backdoor) | Critica | Backdoor vsftpd che apre una shell separata su questa porta. | 9.8 | OpenVAS | CVE-2011-2523 |
| **T01-008** | 3306/tcp (mysql) | Critica | "Credenziali di default/deboli: Accesso riuscito come utente ""root"" con password vuota." | 9.8 | OpenVAS & Nuclei | N/A |
| **T01-010** | 8009/tcp (ajp13) | Critica | Apache Tomcat AJP RCE Vulnerability (Ghostcat). Permette la lettura di file sensibili. | 9.8 | OpenVAS & Nuclei | CVE-2020-1938 |
| **T01-011** | 3632/tcp (distcc) | Critica | DistCC RCE Vulnerability. Confermata l'esecuzione di comandi (id come daemon). | 9.3 | OpenVAS & Nuclei | CVE-2004-2687 |
| **T01-012** | 5900/tcp (vnc) | Critica | "VNC Brute Force Login. Accesso riuscito con password di default/debole: ""password""." | 9.0 | OpenVAS | N/A |
| **T01-013** | 5432/tcp (postgresql) | Critica | "Credenziali di default/deboli: Accesso riuscito con utente ""postgres"" e password ""postgres"". Nuclei conferma lettura hash e file. " | 9.0 | OpenVAS & Nuclei | N/A |
| **T01-NU02** | **8180/tcp (http)** | Alta | **Apache Tomcat Console Esposta/Credenziali di Default.** Nuclei rileva il pannello di gestione. | 8.8 | Nuclei & Nmap Scripts | N/A |
| **T01-014** | 6667/tcp (irc) | Alta | UnrealIRCd backdoor. Versione trojanizzata di UnrealIRCd. | N/A | OpenVAS & Nmap Scripts | N/A |
| **T01-NI01** | **80/tcp (http)** | Alta | **Esposizione di File di Configurazione (es. `/#wp-config.php#`)** contenente credenziali. | 7.8 | Nikto | N/A |
| **T01-015** | 21/tcp, 2121/tcp | Alta | "Credenziali di default/deboli su FTP (vsftpd/ProFTPD). Accesso riuscito con credenziali comuni (e.g., msfadmin:msfadmin)." | 7.5 | OpenVAS & Nuclei | N/A |
| **T01-016** | 1099/tcp (java-rmi) | Alta | Java RMI Server Insecure Default Configuration RCE. Permette a un attaccante non autenticato di eseguire codice arbitrario. | 7.5 | OpenVAS & Nmap Scripts | CVE-2011-3556 |
| **T01-017** | 514/tcp (shell) | Alta | rsh Unencrypted Cleartext Login. Trasmissione di credenziali in chiaro. | 7.5 | OpenVAS | CVE-1999-0651 |
| **T01-018** | 80/tcp (http) | Alta | Metodi HTTP pericolosi (PUT e DELETE) abilitati sulla directory /dav/ (WebDAV). | 7.5 | OpenVAS | N/A |
| **T01-019** | 5432/tcp (postgresql) | Alta | SSL/TLS: OpenSSL CCS Injection (Man in the Middle Security Bypass). | 7.4 | OpenVAS & Nmap Scripts | CVE-2014-0224 |
| **T01-020** | 80/tcp (http) | Alta | "Rilevata potenziale SQL Injection su numerosi endpoint (e.g. |  /dav/ |  /mutillidae/...)." | N/A | OpenVAS & Nmap Scripts | N/A |
| **T01-021** | 80/tcp (http) | Media | TWiki Cross-Site Request Forgery (CSRF). | 6.8 | OpenVAS | CVE-2009-4898 |
| **T01-022** | 25/tcp (smtp) | Media | STARTTLS Implementation Plaintext Arbitrary Command Injection. | 6.8 | OpenVAS | CVE-2011-0411, etc. |
| **T01-023** | 80/tcp (http) | Media | HTTP Debugging Methods (**TRACE**) abilitato. Espone il server a XST. | 6.8 | OpenVAS & Nikto | CVE-2003-1567, etc. |
| **T01-024** | 21/tcp (ftp) | Media | Login FTP Anonimo Abilitato. | 6.4 | OpenVAS & Nuclei | CVE-1999-0497 |
| **T01-025** | 80/tcp (http) | Media | Apache HTTP Server (v2.2.8) 'httpOnly' Cookie Information Disclosure. | 6.3 | OpenVAS | CVE-2012-0053 |
| **T01-026** | 25/tcp, 5432/tcp | Media | SSL/TLS: Certificato con chiave RSA a 1024 bit (Insufficient RSA key size). | 6.3 | OpenVAS | N/A |
| **T01-027** | 80/tcp (http) | Media | TWiki XSS Vulnerability. | 6.1 | OpenVAS | CVE-2018-20212 |
| **T01-028** | 80/tcp (http) | Media | jQuery XSS Vulnerability (v1.3.2). | 6.1 | OpenVAS | CVE-2012-6708 |
| **T01-029** | 445/tcp (netbios-ssn) | Media | Samba RCE Vulnerability (Samba < 3.0.25rc3). | 6.0 | OpenVAS | CVE-2007-2447 |
| **T01-030** | 25/tcp, 5432/tcp | Media | SSL/TLS: Certificato Scaduto. Scaduto il 2010-04-16. | 6.0 | OpenVAS | N/A |
| **T01-031** | 80/tcp (http) | Media | TWiki CSRF Vulnerability. | 6.0 | OpenVAS | CVE-2009-1339 |
| **T01-032** | 25/tcp, 5432/tcp | Media | SSL/TLS: Rilevato uso dei protocolli deprecati SSLv2 e SSLv3. | 5.9 | OpenVAS | CVE-2014-3566,CVE-2016-0800 |
| **T01-033** | 5432/tcp (postgresql) | Media | "SSL/TLS: Report di Cipher Suites Deboli (e.g. |  TLS_RSA_WITH_RC4_128_SHA)." | 5.9 | OpenVAS | CVE-2015-2808 |
| **T01-034** | 22/tcp (ssh) | Media | Supporto per Weak Key Exchange (KEX) Algorithm(s). | 5.3 | OpenVAS | N/A |
| **T01-035** | 22/tcp (ssh) | Media | Supporto per Weak Host Key Algorithm(s): ssh-dss. | 5.3 | OpenVAS | N/A |
| **T01-036** | 22/tcp (ssh) | Media | Supporto per Weak Encryption Algorithm(s). | 5.3 | OpenVAS | N/A |
| **T01-037** | 80/tcp (http) | Media | **phpinfo() Output Disclosure**. File come /phpinfo.php espongono informazioni sensibili. | 5.3 | OpenVAS & Nikto | N/A |
| **T01-038** | 25/tcp | Media | "Mailserver risponde alle richieste VRFY e EXPN (VRFY root risponde 252 2.0.0 root)." | 5.0 | OpenVAS | N/A |
| **T01-039** | 80/tcp (http) | Media | Directory Listing Abilitato su /doc. | 5.0 | OpenVAS & Nikto | CVE-1999-0678 |
| **T01-040** | 80/tcp (http) | Media | QWikiwiki directory traversal vulnerability (/mutillidae/). | 5.0 | OpenVAS | CVE-2005-0283 |
| **T01-041** | 80/tcp (http) | Media | awiki Multiple LFI Vulnerabilities (/mutillidae/). | 5.0 | OpenVAS | N/A |
| **T01-042** | 25/tcp, 5432/tcp | Media | "SSL/TLS: Renegotiation DoS Vulnerability." | 5.0 | OpenVAS | CVE-2011-1473, CVE-2011-5094 |
| **T01-043** | 25/tcp, 5432/tcp | Media | SSL/TLS: Diffie-Hellman Key Exchange Insufficient DH Group Strength. | 5.0 | OpenVAS & Nmap Scripts | N/A |
| **T01-044** | 23/tcp (telnet) | Media | Telnet Unencrypted Cleartext Login. | 4.8 | OpenVAS | N/A |
| **T01-045** | 21/tcp, 2121/tcp | Media | FTP Unencrypted Cleartext Login. | 4.8 | OpenVAS | N/A |
| **T01-046** | 5900/tcp (vnc) | Media | VNC Server Unencrypted Data Transmission. | 4.8 | OpenVAS | N/A |
| **T01-047** | 80/tcp (http) | Media | "Cleartext Transmission of Sensitive Information via HTTP. Campi password in login form." | 4.8 | OpenVAS | N/A |
| **T01-048** | 25/tcp | Media | SSL/TLS: RSA EXPORT Downgrade Issue (FREAK). | 4.3 | OpenVAS | CVE-2015-0204 |
| **T01-049** | 25/tcp, 5432/tcp | Media | SSL/TLS: Deprecated TLSv1.0 Protocol Detection. | 4.3 | OpenVAS | CVE-2011-3389. CVE-2015-0204 |
| **T01-050** | 80/tcp (http) | Media | phpMyAdmin 'error.php' Cross Site Scripting. | 4.3 | OpenVAS | CVE-2010-4480 |
| **T01-051** | 80/tcp (http) | Media | jQuery XSS Vulnerability (v1.3.2). | 4.3 | OpenVAS | CVE-2011-4969 |
| **T01-052** | 25/tcp, 5432/tcp | Media | SSL/TLS: Certificato firmato con algoritmo di firma debole. | 4.0 | OpenVAS | N/A |
| **T01-NI02** | 80/tcp (http) | Media | Directory Listing Abilitato su /icons/. Espone l'elenco dei programmi. | 5.0 | Nikto | N/A |
| **T01-NI03** | 80/tcp (http) | Bassa | Header `X-Frame-Options` e `X-Content-Type-Options` mancanti. | N/A | Nikto | N/A |
| **T01-NU03** | 80/tcp (http) | Bassa | Missing Security Headers (es. `Strict-Transport-Security`, `Content-Security-Policy`). | N/A | Nuclei | N/A |
| **T01-NI01** | **80/tcp (http)** | **Critica** | **Esposizione di File di Configurazione:** Trovato file `#wp-config.php#` (Potenziale esposizione di credenziali). | 9.8 | Nikto | N/A |
| **T01-NU04** | **80/tcp (http)** | **Alta** | **Exposed phpMyAdmin Interface:** Pannello di gestione database esposto pubblicamente. | 7.5 | Nuclei & Nikto | N/A |