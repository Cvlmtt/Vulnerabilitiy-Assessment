# Host environment - Sintesi

**Scopo:** informazioni di contesto sull'host che ospita le VM del laboratorio.
Questo file contiene solo dati di carattere tecnico e non sensibili.

- Host OS: Fedora Linux 42                     # esempio - cambia se diverso
- Hypervisor: VirtualBox 7.1.12
- Versione kernel: 6.6.0                         # opzionale
- CPU: Intel i7-9750H (6 cores)                  # opzionale, solo se rilevante
- RAM fisica disponibile: 32 GB
- Disco: SSD 1 TB (path VMs: `/home/utente/VirtualBox VMs/`)
- Rete host: Host-Only `vboxnet0` (10.0.0.1), NAT per internet
- Note: macchina isolata dalla rete aziendale; laboratorio eseguito in host locale.