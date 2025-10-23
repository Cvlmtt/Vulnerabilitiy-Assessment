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
* Creare una **base di conoscenza** per futuri progetti di Penetration Testing e Hardening.

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

## ⚙️ Ambiente e Strumenti

### Architettura di Laboratorio

L'infrastruttura è costruita su **VirtualBox** con un host **Fedora Linux** e una rete **Host-Only** isolata (`vboxnet0`). Tutte le VM sono state inizializzate con snapshot "Golden Baseline".

| Ruolo | VM/SO | Rete | Note |
| :--- | :--- | :--- | :--- |
| **Attaccante** | Kali Linux | Host-Only | Piattaforma di testing principale. |
| **Target 1** | Metasploitable 2 (Linux 2.6.24) | Host-Only | Target deliberatamente vulnerabile. |
| **Target 2** | Ubuntu Server + DVWA | Host-Only | Target Web Application Testing. |

Informazioni in merito agli assets sono disponibili in `docs/host_info.md` e `docs/vm_info.md`

### Tool di Lavoro

| Fase | Tool Utilizzati (Finora) | Tool in Programma |
| :--- | :--- | :--- |
| **Discovery/Enum** | **Nmap**,**Nmap (Script Avanzati)** | N/A |
| **Vulnerability/PoC** | N/A | CLI (psql, ftp, ssh), Metasploit Framework, **OWASP ZAP** (Web App Scanning), **Burp Suite** (Manual Testing) |

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