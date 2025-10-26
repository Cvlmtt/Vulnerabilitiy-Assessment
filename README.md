# 🛡️ Vulnerability Assessment e Hardening su Laboratorio Multi-Target

> **Stato del Progetto:** In Corso (Fase: Scansione Avanzata e Testing Controllato)
> **Output Principale:** [report/VA_Report_Final.pdf] (In fase di completamento)

---

## 🎯 Panoramica del Progetto

Questo repository documenta un progetto di **Vulnerability Assessment (VA)** completo, condotto su un laboratorio virtuale isolato. L'obiettivo è dimostrare competenze tecniche nell'identificazione, classificazione del rischio (tramite CVSS) e proposta di mitigazioni per vulnerabilità di rete e applicative.

Il progetto adotta un approccio **evidence-based**, in linea con le metodologie di sicurezza professionali (es. OWASP WSTG), garantendo la tracciabilità e la riproducibilità di ogni fase.

### Obiettivi Chiave

* Eseguire una **ricognizione di rete e applicativa** approfondita (Black-Box).
* Identificare, classificare (CVSS) e documentare le vulnerabilità (inclusi zero-day noti e configurazioni errate).
* Produrre un **Report Tecnico-Esecutivo** (PDF) che trasformi i risultati tecnici in raccomandazioni attuabili.
* Creare una **base di conoscenza** per futuri progetti di Penetration Testing, oltre che migliorare e ampliare la conoscenza di tools tecnici per attività di VA e PT.

---

## Stato del Progetto

| Fase | Step Dettagliato | Target(s) | Strumenti | Stato |
|------|------------------|-----------|-----------|-------|
| Preparazione | Allestimento laboratorio e Snapshot | Tutti | VirtualBox | ✅ Eseguito |
| Discovery & Enumeration | Scansione Iniziale | Tutti | Nmap | ✅ Eseguito |
| Vulnerability Scanning | Scanning Avanzato Servizi (CVE Mapping) | T.01 | Nmap --script=vulners,vuln | ✅ Eseguito |
|  | Scansione Approfondita (Web/Host) | T.02 | Nikto, Nuclei | 🟠 In Coda |
|  | Scansione con Vulnerability Scanner Commerciale (OpenVAS) | T.01, T.02 | OpenVAS / Greenbone | 🟠 In Coda |
| Triage & Risk Classification | Analisi e Correzione del Report (Aggiornamento Matrice di Rischio e Triage) | T.01, T.02 | Analisi Manuale, CVSS | ⏳ In Corso (T.01 Fatto) |
| Verification (Proof-of-Concept - PoC) | PoC Accesso Critico/Alto (RCE su vsFTPd, Bindshell, Credenziali DB) | T.01 | Metasploit, Telnet/Netcat | ❌ Non Eseguito |
|  | PoC Applicativo Web (SQL Injection, XSS su DVWA) | T.02 | OWASP ZAP, Burp Suite, SQLMap | ❌ Non Eseguito|
| Reporting & Remediation | Finalizzazione del Report Tecnico/Esecutivo e Proposta di Mitigazioni | N/A | Documentazione | ❌ Non Eseguito |

---

## ⚙️ Ambiente e Strumenti

### Architettura di Laboratorio

L'infrastruttura è costruita su **VirtualBox** con un host **Fedora Linux** e una rete **Host-Only** isolata (`vboxnet0`). Tutte le VM sono state inizializzate con snapshot "Golden Baseline".

| Ruolo | VM/SO | Rete | Note |
| :--- | :--- | :--- | :--- |
| **Attaccante** | Kali Linux | Host-Only | Piattaforma di testing principale. |
| **Target 1** | Metasploitable 2 (Linux 2.6.24) | Host-Only | Target deliberatamente vulnerabile. |
| **Target 2** | Ubuntu Server + DVWA | Host-Only | Target Web Application Testing. |

Informazioni in merito agli assets sono disponibili in `docs/host_info.md` e `docs/vm_info.md`

---

### Tool di Lavoro

| Fase | Tool Utilizzati (Finora) | Tool in Programma |
| :--- | :--- | :--- |
| **Discovery/Enum** | **Nmap**,**Nmap (Script Avanzati)** | OpenVAs |
| **Vulnerability/PoC** | N/A | Metasploit Framework, **OWASP ZAP** (Web App Scanning), **Burp Suite** (Manual Testing) |

--- 

## 🚨 Risultati Preliminari (Fase 1: Discovery)

Le scansioni iniziali hanno rivelato una **postura di sicurezza Critica** sui target, a causa dell'uso di software obsoleto e di configurazioni di default note.

| Target | IP | Tipo di Rischio Principale | Servizi Critici Esposti |
| :--- | :--- | :--- | :--- |
| **Metasploitable 2 (T.01)** | `192.168.56.105` | Vulnerabilità che consentono Remote Code Execution | VsFTPd, java-rmi, bindshell, UnrealIRCd |
| **Ubuntu + DVWA (T.02)** | `192.168.56.106` | (Analisi ancora da effettuare) |(Analisi ancora da effettuare) | 

**Evidenze di Alto Rischio già rilevate:**

- **Backdoor vsFTPd (CVE-2011-2523)**: dimostrata RCE con privilegi di root
- **RMI Registry RCE (CVE-2020-9761)**: configurazione di default che permette RCE
- **Bindshell**: configurazione insicura di una bindshell con permessi di root in ascolto  

---

## 📁 Struttura della Repository

La repository è strutturata per riflettere le fasi del progetto:

* **`report/`**: Contiene il report finale (PDF/DOCX) con la classificazione CVSS e le raccomandazioni.
* **`scans/`**: Log non elaborati e output grezzi da Nmap, Nikto, nuclei, e altri tools di scan.
* **`docs/`**: Informazioni di background sull'ambiente.
* **`findings/`**: Analisi dettagliata delle evidenze ottenute in ciascuna fase

---

## 🔒 Disclaimer

Tutte le attività di scansione e test sono state realizzate **esclusivamente a fini didattici e di ricerca** su sistemi controllati (di proprietà). Nessun sistema esterno o di terzi è stato coinvolto.

*Autore: Mattia Cavaliere*
*Licenza: [MIT License]*