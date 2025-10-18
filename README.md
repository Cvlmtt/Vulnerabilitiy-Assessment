# Vulnerability Assessment Lab Project

Questo repository documenta il progetto personale di **Vulnerability Assessment** svolto in ambiente controllato, con l’obiettivo di analizzare, testare e documentare la sicurezza di un insieme di macchine virtuali configurate appositamente per simulare un’infrastruttura di rete reale.

Il progetto è stato interamente eseguito in ambiente locale, senza alcun collegamento o impatto su sistemi esterni.  
Tutte le attività — scansioni, test di vulnerabilità e simulazioni di exploit — sono state svolte **in modo etico**, nel rispetto delle buone pratiche di sicurezza informatica e con macchine di mia esclusiva proprietà o configurate a fini didattici.

---

## 🎯 Obiettivi del progetto

- Creare un **laboratorio virtuale** completo per lo studio di tecniche di scanning e penetration testing.  
- Documentare le fasi di **ricognizione, analisi e testing** di sicurezza delle VM.  
- Produrre **evidenze verificabili** (snapshot, log, report di scansione) che dimostrino l’attività svolta.  
- Mantenere una **tracciabilità chiara** del lavoro attraverso commit descrittivi e versionamento dei file chiave.

---

## ⚙️ Ambiente di lavoro

- **Hypervisor:** VirtualBox  
- **VM principali:** Kali Linux (attaccante), Metasploitable 2, Ubuntu server + DVWA  
- **Rete:** configurazione Host-Only + NAT, isolata dal traffico reale  
- **Strumenti principali:** Nmap, Nikto, Metasploit Framework, Wireshark, tcpdump, nuclei  

Tutti i sistemi sono stati inizialmente salvati come snapshot “golden”, documentati nel file `vm_snapshots.md`.

---

## 🧾 Metodo di lavoro

1. **Configurazione e snapshot iniziali**  
   Ogni VM viene preparata, configurata e salvata come snapshot di riferimento.

2. **Scansione e enumerazione**  
   Vengono eseguite scansioni di rete (TCP/UDP) per identificare host, servizi e versioni.

3. **Analisi vulnerabilità**  
   Si applicano strumenti di assessment (Nmap NSE, Nikto, nuclei) per individuare potenziali CVE.

4. **Testing controllato**  
   Eventuali exploit o tentativi di privilege escalation vengono condotti in ambiente isolato.

5. **Documentazione**  
   Tutte le prove vengono registrate in file testuali o screenshot, con commit dedicati che ne riportano la data e la finalità.

---

## 🧱 Filosofia del repository

Questa repository non è pensata per la riproduzione integrale del laboratorio, ma come **documentazione verificabile** del percorso tecnico svolto.  
Ogni commit rappresenta un’azione concreta — una scansione, una modifica, un’analisi o un report — e serve a fornire **traccia cronologica e trasparente** dell’evoluzione del progetto.

---

## 📅 Stato del progetto

- ✅ Configurazione ambiente completata  
- ✅ Snapshot iniziali registrati (`vm_snapshots.md`)  
- 🔜 Scansioni di rete iniziali (fase di ricognizione)

---

## 🔒 Disclaimer

Tutte le attività descritte sono state realizzate **esclusivamente a fini didattici e di ricerca** su sistemi controllati.  
Non è stato in alcun modo coinvolto o testato alcun sistema di rete esterno o appartenente a terzi.

---

© 2025 – Progetto di Vulnerability Assessment  
Autore: Mattia Cavaliere
