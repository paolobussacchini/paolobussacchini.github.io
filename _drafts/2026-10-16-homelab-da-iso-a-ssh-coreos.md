---
layout: post
title: "Homelab: da ISO a SSH su Fedora CoreOS"
date: 2026-10-16
categories: [homelab]
tags: [coreos, virtualbox, networking, ssh]
---

Primo pezzo pubblico del lab: far partire una VM **Fedora CoreOS**, applicarci una config **Ignition**, ed entrarci in **SSH** - senza password in console.

Racconto bottoms-up, come l'ho capito io.

## 1. Il PC vero e il PC finto

Il laptop Windows è l'unico pezzo di ferro. **VirtualBox** crea dentro una stanza: CPU, RAM, disco e scheda di rete *virtuali*. La VM crede di essere un computer normale.

```text
PC Windows
  └── VirtualBox
        └── VM (disco all'inizio vuoto)
```

## 2. Cos'è un'ISO (e quale scaricare)

Un file `.iso` è un "CD in un file". Lo monti sulla VM e booti da lì.

**Live** = l'OS gira in RAM e serve a *installare* sul disco, non è ancora il sistema definitivo.

Errore istruttivo: ho scaricato **Fedora Workstation** (desktop con installer grafico). Per questo lab serve **Fedora CoreOS Live** (appliance + `coreos-installer` + Ignition). Non è "quale numero di Fedora", è **quale prodotto**.

## 3. La ricetta: Butane -> Ignition

- **Butane** (`.bu`): YAML leggibile - utente, chiave SSH pubblica, cartelle, hostname.
- **Ignition** (`.ign`): JSON per la macchina, applicato **una volta** al primo boot.
- Il tool `butane` traduce A -> B.

Poi servi il `.ign` con un HTTP server minimo sul PC e, dalla Live, installi con qualcosa del tipo:

```bash
coreos-installer install <disco> \
  --ignition-url http://<host>:8000/config.ign \
  --insecure-ignition
```

Dettaglio che mi ha bloccato: su PowerShell il redirect `>` aveva prodotto un JSON in **UTF-16**. Il download "funzionava", il parse no. Serve **UTF-8**.

Il campo `version: 1.5.0` nel Butane è la versione dello *schema* del file, non la release di Fedora.

## 4. Rete: perché `127.0.0.1:2222`

Con la scheda in **NAT**, la VM ha un IP interno (tipico `10.0.2.15`) e la LAN non la vede. Sul PC apri un **port-forward**:

```text
PowerShell  ->  127.0.0.1:2222  ->  (VirtualBox)  ->  VM:22 (sshd)
```

Bridge sulla Wi-Fi, sul mio setup, non mi ha dato un IP raggiungibile. NAT + forward è bastato.

## 5. SSH: lucchetto e chiave

- Pubblica -> in Butane / sulla VM (`authorized_keys`)
- Privata -> solo sul PC (`-i`)

La console chiede password: su CoreOS lab spesso **non ce n'è**. Normale. Entri così:

```powershell
ssh -i $env:USERPROFILE\.ssh\<chiave> -p 2222 core@127.0.0.1
```

Senza `-i` / `-p` ottieni facilmente `Permission denied (publickey)`.

Checkpoint: hostname atteso, interfaccia su `10.0.2.15`, utente `core`.

## Cosa viene dopo

Installare **k3s** pinnato, poi Helm e una PVC di prova per capire *dove* i dati vivono sul disco del nodo. Poi rendere l'installazione ripetibile dalla ricetta Butane.

---

*Appunti sanitizzati da un lab personale. Niente chiavi, secret o dettagli interni di lavoro.*
