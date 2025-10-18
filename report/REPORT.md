# Report — Vulnerability Assessment Lab

## Indice
1. Executive summary
2. Overview
   1. Inventory & Snapshots
3. Details
4. Findings
5. Recommended mitigations

# 1. Executive Summary

Il presente report documenta le attività di **Vulnerability Assessment** condotte in ambiente virtuale controllato, con l’obiettivo di individuare, analizzare e documentare vulnerabilità presenti in sistemi e servizi configurati intenzionalmente con differenti livelli di esposizione.  
Il progetto è stato sviluppato a scopo formativo e di ricerca, seguendo le buone pratiche di sicurezza informatica e mantenendo un approccio etico e metodico in ogni fase del lavoro.

L’ambiente di laboratorio è costituito da più macchine virtuali interconnesse in una rete isolata, simulando un’infrastruttura di produzione ridotta ma realistica.  
Le attività principali includono:

- Configurazione dell’ambiente di test e creazione di snapshot delle VM;
- Ricognizione e analisi dei servizi esposti tramite scansioni di rete (TCP/UDP);
- Valutazione delle vulnerabilità identificate con strumenti automatici e manuali;
- Documentazione dei risultati e proposta di mitigazioni tecniche.

Il progetto adotta un approccio **evidence-based**, in cui ogni fase viene registrata, documentata e supportata da file, log o screenshot presenti nel repository.  
Tale metodologia consente di garantire **ripetibilità, tracciabilità e trasparenza** dell’intero processo.

---

# 2. Overview

## 2.1 Scopo del laboratorio

L’obiettivo generale è realizzare un ambiente sicuro e controllato in cui poter studiare e sperimentare le fasi tipiche di un’attività di vulnerability assessment, utilizzando esclusivamente macchine e software di cui si dispone piena proprietà o controllo.  
Il laboratorio è stato progettato per riprodurre scenari comuni di esposizione a rischio — come server web vulnerabili, servizi di autenticazione deboli o configurazioni non sicure — al fine di analizzare le superfici d’attacco e comprendere le dinamiche di rilevamento e mitigazione.

## 2.2 Architettura e strumenti

- **Host principale:** sistema Fedora Linux con VirtualBox come hypervisor (dettagli in `HOST_INFO.md`);
- **Rete di laboratorio:** configurazione Host-Only per l’interconnessione tra VM, con NAT per l’accesso esterno se necessario;
- **Topologia:** un host attaccante (Kali Linux) e più target configurati con servizi vulnerabili o legacy;
- **Isolamento:** il laboratorio è completamente separato dalla rete reale, senza bridge verso connessioni esterne;
- **Strumenti principali:**
  - **Nmap** per la ricognizione e l’enumerazione dei servizi;
  - **Nikto** e **nuclei** per la scansione di vulnerabilità note su servizi web;
  - **Metasploit Framework** per il testing controllato di exploit;
  - **Wireshark** e **tcpdump** per la cattura e l’analisi del traffico;
  - **Script personalizzati** (bash/python) per automatizzare fasi di scanning e logging.

## 2.3 Metodo di lavoro

Il laboratorio segue un ciclo metodologico ispirato alle principali fasi del vulnerability assessment:

1. **Preparazione e baseline:** configurazione delle VM e snapshot iniziali;
2. **Ricognizione:** identificazione degli host e dei servizi attivi;
3. **Enumerazione:** raccolta di informazioni dettagliate su versioni, porte e protocolli;
4. **Valutazione delle vulnerabilità:** confronto con database CVE e test con tool automatici;
5. **Analisi e reporting:** classificazione dei rischi e documentazione dei risultati;
6. **Mitigazioni e verifica:** proposta di correzioni e test successivi (in fasi future).

Ogni step viene accompagnato da un commit descrittivo nel repository per garantire la tracciabilità temporale del lavoro svolto.

---

# 2.4 Inventory & Snapshot

Il laboratorio è composto da più macchine virtuali ospitate in VirtualBox.  
Per ogni VM è stato creato almeno uno **snapshot iniziale** (“golden snapshot”) che rappresenta lo stato stabile e documentato prima di qualsiasi attività di test.
Tutti gli snapshot sono elencati in dettaglio nel file `vm_snapshots.md`, che riporta anche hash e note tecniche specifiche.  
Questi snapshot costituiscono la **baseline verificabile** del laboratorio e possono essere ripristinati in qualsiasi momento per garantire l’integrità e la ripetibilità dei test.

---

## 2.5 Ricognizione — scansione iniziale (stato attuale)

**Data esecuzione:** 2025-10-18  
**Scopo:** identificare host attivi, porte aperte e servizi/versioni esposti sulla rete host-only del laboratorio.  
**Nota sui log di rete:** non è stata catturata una pcap in questa fase; tutti gli output prodotti dagli strumenti sono salvati nella cartella `scans/` del repository.

### Comandi eseguiti (principali)
- Scansione TCP+service/version detection: `nmap -Pn -sS -sV -O -T4 --reason -oA scans/initial_scan 192.168.56.106/24`
- Scansioni mirate web / vulnerabilità: 
  - `nikto -h http://192.168.56.106 -o scans/nikto_web-target-1.txt`
  - `nuclei -l target.txt -o scans/nuclei_results.txt`
Il file `target.txt` reperibile nella cartella `scans/` contiene la lista degli indirizzi delle macchine target. 


### File prodotti e posizione
Tutti i file prodotti dalla ricognizione sono commitati nella repo sotto `scans/`:
- `scans/initial_scan.xml` — output XML Nmap (importabile).
- `scans/initial_scan.nmap` — output Nmap leggibile.
- `scans/initial_scan.gnmap` — output grezzo per parsing.
- `scans/nikto_web-target-1.txt` — output Nikto.
- `scans/nuclei_results.txt` — output nuclei.

### Sintesi operativa (come leggere i risultati)
- Usare l'XML (`scans/initial_scan.xml`) per import in Metasploit o per analisi automatica.  
- Usare `scans/initial_scan.nmap` per una lettura rapida dei servizi e delle versioni.  
- I risultati principali (host attivi, porte aperte, servizi e versioni) sono stati sintetizzati in `findings/initial_enumeration.md`.


---

# ✅ Stato attuale

- Ambiente configurato e operativo  
- Snapshot iniziali completati e documentati
- Report in fase di completamento per la sezione tecnica (“Details”)  
- Nessuna attività di scanning ancora eseguita

---


