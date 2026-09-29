# WinRE BIOS Bypass for Clean OS Installation

> **Descrizione:** Un metodo pratico per aggirare i blocchi del BIOS/UEFI aziendale sfruttando l'Ambiente di Ripristino di Windows (WinRE) e Diskpart per forzare un'installazione pulita del sistema operativo.

## Il Problema
Nei sistemi aziendali dismessi o di seconda mano, è frequente incontrare un blocco crittografico del BIOS che impedisce:
- L'accesso al menu di configurazione hardware (F2).
- La selezione manuale del dispositivo di boot (F12).
- L'avvio di dispositivi USB (spesso disabilitati per policy di sicurezza interne).

Senza la Master Password (o l'accesso a programmatori hardware), risulta apparentemente impossibile avviare un drive di installazione esterno per formattare la macchina.

## La Soluzione: Sfruttare WinRE e Diskpart
Poiché l'Ambiente di Ripristino di Windows (WinRE) nativo possiede i privilegi necessari per caricare i driver USB durante le operazioni diagnostiche, è possibile richiamare un prompt dei comandi con privilegi di sistema per lanciare manualmente l'eseguibile di installazione direttamente dal supporto esterno, aggirando le restrizioni del firmware.

## Procedura Passo-Passo

### 1. Accesso all'Avvio Avanzato
- Inserire il supporto di installazione USB preparato (es. tramite Rufus).
- Dal sistema operativo della macchina bloccata, tenere premuto `Maiusc` e cliccare su **Riavvia il sistema**.

### 2. Apertura del Terminale di Sistema
- Nella schermata blu delle opzioni avanzate, navigare in: `Risoluzione dei problemi` > `Opzioni avanzate` > `Prompt dei comandi`.

### 3. Individuazione del Volume USB
- Nel terminale, lanciare lo strumento di gestione dischi digitando in sequenza (premendo Invio dopo ogni comando):
  `diskpart`
  `list volume`
  `exit`
- Identificare la lettera assegnata alla partizione principale della chiavetta USB contenente i file di Windows.

### 4. Esecuzione dell'Installer
- Spostarsi nell'unità corretta (sostituendo `F:` con la lettera individuata):
  `F:`
  `dir`
- Verificare la presenza del file `setup.exe` nell'elenco.
- Lanciare l'installazione scavalcando il BIOS digitando:
  `setup.exe`

### 5. Pulizia del Disco
- Una volta avviato il setup grafico, selezionare l'installazione **Personalizzata**.
- Eliminare **tutte** le partizioni esistenti (Sistema, Ripristino, MSR, Primaria) per radere al suolo le vecchie partizioni OEM e i dati del sistema precedente.
- Installare il nuovo OS sullo "Spazio non allocato" risultante.

## Considerazioni di Sicurezza
Questa procedura dimostra come l'accesso fisico e a livello di OS possa scavalcare le difese del perimetro firmware. Questo approccio non rimuove fisicamente la password del BIOS memorizzata sulla scheda madre, ma permette di riottenere il pieno controllo dello storage e del sistema operativo su macchine legittimamente acquisite.
