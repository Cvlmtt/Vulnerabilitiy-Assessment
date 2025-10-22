Data di Redazione: 22 Ottobre 2025  
Autore: Mattia Cavaliere  
Ambito di Valutazione: Network & Web Application Security (Black-Box)  
Target: Metasploitable 2 e Ubuntu Server + DVWA  

# 1. Executive Summary
Il presente report documenta i risultati di un'attività di Vulnerability Assessment (VA) condotta su un laboratorio virtuale interno, composto da due macchine target deliberatamente vulnerabili, al fine di dimostrare competenza tecnica in ricognizione, analisi del rischio e mitigazione.

L'analisi preliminare ha rivelato una postura di sicurezza Estremamente Critica su entrambi i sistemi. Sono state identificate un totale di 11 vulnerabilità significative (rischio Alto o Critico), di cui 2 classificate come CRITICHE. Le criticità sono principalmente dovute all'utilizzo di software obsoleto (Metasploitable 2) e configurazioni di default insicure (DVWA).

Le vulnerabilità più gravi permettono a un attaccante non autenticato:
1. Accesso a file di sistema e potenziale RCE (Remote Code Execution) su Metasploitable 2.
2. Completa compromissione del database tramite credenziali di default.

Raccomandazione Immediata: Si raccomanda di procedere immediatamente alla verifica e mitigazione delle vulnerabilità Critiche e Alte, in particolare quelle relative ai servizi esposti su Metasploitable 2.

# 2. Overview e Metodologia
## 2.1 Scopo e Obiettivi
L'obiettivo primario di questo VA è stato quello di identificare, analizzare e classificare le vulnerabilità di sicurezza a livello di rete e applicativo presenti nel laboratorio virtuale, in un'ottica di miglioramento della sicurezza informatica. 

## 2.2 Ambiente di Laboratorio
La valutazione è stata condotta dal sistema attaccante (Kali Linux) sulla rete Host-Only `vboxnet0`.
Il laboratorio è composto dai seguenti assetts:


| Ruolo | Sistema Operativo | Applicazione / Impiego | Indirizzo IP |
|-------|-------------------|------------------------|--------------|
| Attaccante | Kali Linux | N/A | 192.168.56.104 |
| Target 1 | Metasploitable2 | Server Multi-Servizio | 192.168.56.105 |
| Target 2 | Ubuntu Server | Web Application Testing (DVWA) | 192.168.56.106 |
| Host | Fedora 42 (VirtualBox) | N/A | N/A |

## 2.3 Metodologia e strumenti
L'approccio utilizzato è di tipo Black-Box (senza conoscenze preliminari sull'infrastruttura interna, se non la gamma IP).
I passi eseguiti sono i seguenti:

| Fase | Strumenti utilizzati | Scopo |
|------|----------------------|-------|
| Discovery | nmap | Rilevmento di host, porte aperte e identificazione dei servizi/versioni |
| Scanning | Nuclei, nikto | Scansione automatizzata di vulnerabilità note (CVE) e configurazioni errate (Web) |
| Analysis | Manuale, CVSS v3.1 | Classificazione del rischio e analidi dei risultati |

### 2.3.1 Discovery
La fase di discovery è stata condotta tramite l'utilizzo del tool nmap sull'intera subnet configurata nel laboratorio (192.168.56.1/24). 
Per la fase di discovery è stato eseguito il seguente comando:

```Bash
nmap -Pn -sS -sV -O -T4 --reason -oA scans/initial_scan 192.168.56.1/24
```

Il risultato di tale comando è consultabile in `scans/nmap/initial_scan.*` in formato RAW (`.nmap`), XML e greppable (`.gnmap`). 

`Nmap` ha trovato 5 host attivi all'interno della subnet:
- 192.168.56.1 (il default gateway configurato da VirtualBox)
- 192.168.56.100 (server DHCP configurato da VirtualBox)
- 192.168.56.105 (Metasploitable2)
- 192.168.56.106 (Ubuntu Server)
- 192.168.56.104 (Kali Linux)

### 2.3.2 Scanning
Successivamente, la fase di scansione automatizzata è stata eseguita tramite i tools `Nuclei` e `Nikto`, rispettivamente con i seguenti comandi:

```Bash
nuclei -l target.txt -o scans/nuclei_results.txt
nikto -h http://192.168.56.106 -o scans/nikto_web-target-1.txt
```
*Nota: il file target.txt è consultabile nella directory `scans/target.txt`. Contiene gli indirizzi IP dei target che si vuole scansionare*

I risultati delle scansioni sono consultabili rispettivamente nei file `scans/nuclei_results.txt` e `scans/nikto_results.txt`. 

## Sintesi dei Risultati e Rischio
Sono state identificate 11 vulnerabilità significative, classificate secondo la loro gravità presunta o calcolata in base al potenziale impatto CVSS. 

| Livello di Gravità | Conteggio | Percentual |
|--------------------|-----------|------------|
| Critico | 2 | 18% |
| Alto | 4 | 36% |
| Medio | 3 | 27% |
| Basso/Info | 2 | 18% |
| Totale | 1 | 100% |

