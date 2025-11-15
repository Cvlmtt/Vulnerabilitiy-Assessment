# Target T.02 (192.168.56.106)
_Analisi e triage delle vulnerabilità emerse dalle scansioni web, applicative e di host._

---

## 1. Identificazione del target
| Parametro | Valore |
|:----------|:--------|
| Host | 192.168.56.106 |
| Sistema Operativo | Linux Ubuntu 24.04.3 TLS |
| Servizi Principali | HTTP (Apache 2.4.58) |
| Ruolo | Web Application Testing (DVWA) |

---

## 2. Strumenti di scansione utilizzati
| Strumento | Data scansione | Versione | Scopo | Report | ID format |
|:----------|:---------------|:----------|:-------|:--------|:-----------|
| **Nmap** | 2025-10-22 | 7.95 | Identificazione porte, servizi, OS detection (scansione iniziale). | `scans/nmap/discovery.nmap` | N/A |
| **OpenVAS** | 2025-11-03 | 26.2.1 | Scansione Cross-Validation e classificazione del rischio host/rete. | `scans/openvas/openvas_report_T01-T02.pdf` | T02-OVxx |
| **Nikto** | N/A | 2.5.0 | Rilevamento di misconfigurazioni del web server, directory esposte e weak file names. | `scans/web/T02_nikto.txt` | T02-NIxx |
| **Nuclei** | N/A | 3.x | Testing rapido e mirato di CVE specifiche e configurazioni web (Template Matching). | `scans/web/T02_nuclei.txt` | T02-NUxx |
| **OWASP ZAP** | 2025-11-04 | 2.16.1 | Dynamic Application Security Testing (DAST) su DVWA per difetti di codice e policy mancanti. | `scans/ZAP/T02/2025-11-04-ZAP-Report-complete.pdf` | T02-ZAxx |

---

## 4. Vulnerabilità rilevate

## 4. Vulnerabilità rilevate

| ID | Porta/Servizio | Gravità | Descrizione della Vulnerabilità | CVSS (Manuale) | Rilevato Da | CVE/Riferimenti |
|:---|:---------------|:--------|:--------------------------------|:-----|:------------|:----------------|
| **T02-AL01** | 80/tcp (DVWA) | **CRITICA** | **SQL Injection (Iniezione Database)**. Vulnerabilità intrinseca di DVWA che permette l'estrazione di dati sensibili e la completa compromissione del DB. | 9.8 | **Manuale (Logica Applicativa)** | CWE-89 |
| **T02-AL02** | 80/tcp (DVWA) | **ALTA** | **Cross-Site Scripting (XSS)**. Vulnerabilità di DVWA (sia Reflected che Stored) che permette il furto di sessioni o l'esecuzione di codice client-side. | 7.5 | **Manuale (Logica Applicativa)** | CWE-79 |
| **T02-OV01** | 80/tcp (http) | Media | **Source Control Management (SCM) Files Accessible (.git)**. Esposizione di metadati Git nella directory /dvwa/. | 5.0 | OpenVAS | N/A |
| **T02-OV02** | 80/tcp (http) | Media | **Trasmissione Cleartext di Informazioni Sensibili (HTTP)**. Credenziali di login (password) trasmesse in chiaro. | 4.8 | OpenVAS & ZAP | CWE-319 |
| **T02-ZA01** | 80/tcp (http) | Media | **Directory Browsing / Listing Abilitato**. Esposizione di struttura e file su directory web. | 5.0 | ZAP, OpenVAS, Nikto | CWE-548 |
| **T02-ZA02** | 80/tcp (http) | Media | **Mancanza di Anti-clickjacking Header (X-Frame-Options)**. Rischio di attacchi ClickJacking. | N/A | ZAP, Nikto | CWE-1021 |
| **T02-ZA03** | 80/tcp (http) | Bassa | **X-Content-Type-Options Header Missing**. Rischio di MIME-sniffing. | N/A | ZAP, Nikto | N/A |

---

## 3. Prossimo Step: Verification (PoC)

Con questa correzione, la tua prossima azione deve essere la **Fase 4: Verification (Proof-of-Concept)**.

1.  **Obiettivo Primario:** Eseguire PoC per **T02-AL01 (SQL Injection)** e **T02-AL02 (XSS)**. Questo è l'unico modo per confermare la vera criticità del finding e superare i limiti della classificazione automatica degli scanner.
2.  **Tool:** Utilizza **Burp Suite** o **SQLMap** per la verifica attiva.