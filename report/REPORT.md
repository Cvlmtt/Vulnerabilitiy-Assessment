Data inizio VA: 15 Ottobre 2025
Data di Redazione: 22 Ottobre 2025  
Autore: Mattia Cavaliere  
Ambito di Valutazione: Network & Web Application Security   
Target: Metasploitable 2 (T.01) e Ubuntu Server + DVWA (T.02)  

# 1. Executive Summary
Il presente report documenta i risultati di un'attività di Vulnerability Assessment (VA) condotta su un laboratorio virtuale interno, composto da due macchine target deliberatamente vulnerabili, al fine di dimostrare competenza tecnica in ricognizione, analisi del rischio e mitigazione.

L'analisi preliminare ha rivelato una postura di sicurezza Estremamente Critica su entrambi i sistemi. Sono state identificate un totale di 11 vulnerabilità significative (rischio Alto o Critico), di cui 2 classificate come CRITICHE. Le criticità sono principalmente dovute all'utilizzo di software obsoleto (T.01) e configurazioni di default insicure (T.02).

Le vulnerabilità più gravi permettono a un attaccante non autenticato:
1. Accesso a file di sistema e potenziale RCE (Remote Code Execution) su T.01.
2. Completa compromissione del database tramite credenziali di default T.02.

Raccomandazione Immediata: Si raccomanda di procedere immediatamente alla verifica e mitigazione delle vulnerabilità Critiche e Alte, in particolare quelle relative ai servizi esposti su T.01 tramite aggiornamento software dei servizi offerti da T.01.

# 2. Overview e Metodologia
## 2.1 Scopo e Obiettivi
L'obiettivo primario di questo VA è stato quello di identificare, analizzare e classificare le vulnerabilità di sicurezza a livello di rete e applicativo presenti nel laboratorio virtuale, in un'ottica di miglioramento della sicurezza informatica. 

## 2.2 Ambiente di Laboratorio
La valutazione è stata condotta dal sistema attaccante (Kali Linux) sulla rete Host-Only `vboxnet0`.
Il laboratorio è composto dai seguenti assetts:


| Ruolo | Sistema Operativo | Applicazione / Impiego | Indirizzo IP | Identificativo |
|-------|-------------------|------------------------|--------------|----------------|
| Attaccante | Kali Linux | N/A | 192.168.56.104 | A.01 |
| Target 1 | Metasploitable2 | Server Multi-Servizio | 192.168.56.105 | T.01 |
| Target 2 | Ubuntu Server | Web Application Testing (DVWA) | 192.168.56.106 | T.02 |
| Host | Fedora 42 (VirtualBox) | N/A | N/A | N/A |

## 2.3 Metodologia e strumenti
L'approccio utilizzato è di tipo Black-Box (senza conoscenze preliminari sull'infrastruttura interna, se non la gamma IP).
I passi eseguiti sono i seguenti:

| Fase | Strumenti utilizzati | Scopo |
|------|----------------------|-------|
| Discovery | nmap v7.95| Rilevmento di host, porte aperte e identificazione dei servizi/versioni |
| Scanning | nmpa script `vulners`, `vuln`| Scansione automatizzata di vulnerabilità note (CVE) e configurazioni errate (Web) |
| Analysis | Manuale, CVSS v3.1 | Classificazione del rischio e analidi dei risultati |

### 2.3.1 Discovery
La fase di discovery è stata condotta tramite l'utilizzo del tool nmap sull'intera subnet configurata nel laboratorio (192.168.56.1/24). 
Per la fase di discovery è stato eseguito il seguente comando:

```Bash
nmap -Pn -sS -sV -O -T4 --reason -oA scans/discovery 192.168.56.1/24
```

Il risultato di tale comando è consultabile in `scans/nmap/discovery.*` in formato RAW (`.nmap`), XML e greppable (`.gnmap`). 

`Nmap` ha trovato 5 host attivi all'interno della subnet:
- 192.168.56.1 (il default gateway configurato da VirtualBox)
- 192.168.56.100 (server DHCP configurato da VirtualBox)
- 192.168.56.105 (T.01)
- 192.168.56.106 (T.02)
- 192.168.56.104 (A.01)

### 2.3.2 Scanning
La fase di scanning è stata divisa in due parti:
1. **Scanning avanzato (T.01 - 192.168.56.105)**: scan eseguito con scripts `nmap` (`vulners` e `vuln`). I risultati hanno rivelato la presenza di 3 vulnerabilità RCE (Remote Code Execution) classificate come critiche in base a CVSS. Il triage dettagliato è disponibile in `findings/T01_advanced_triage.md`
2. **Scanning applicativo (T.02 - 192.168.56.106)**: In attesa di esecuzione

# 3. Sintesi dei Risultati
