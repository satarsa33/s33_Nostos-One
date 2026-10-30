# S33 - Nostos One

Sintetizzatore **modulare polifonico** che gira nel browser, pensato per essere suonato con una **tastiera MIDI**.
Ha due facce sullo stesso motore:

- la **Lavagna**: i moduli (oscillatori, filtri, inviluppi, effetti…) si trascinano, si collegano con i cavi e si ascoltano subito;
- il **Pannello**: un frontale pulito con manopole, drawbar, arpeggiatore e monitor, che agisce sugli stessi moduli.

È una **PWA** in un solo file HTML, senza dipendenze né build: si apre, si installa come app e funziona anche senza rete.

> Progetto personale della famiglia S33, nato come esperimento didattico e di divertimento. Il nome viene da *nostos*, il "ritorno a casa" da cui nasce la parola nostalgia: propone suoni del passato. Nessuna garanzia: vedi [Limiti noti](#limiti-noti).

---

## Avvio rapido

### Provarlo in locale

I service worker e il MIDI richiedono `http://localhost` o `https://`, quindi non aprire il file con doppio clic: servi la cartella.

```bash
cd s33-nostos-one
python3 -m http.server 8000
# poi apri http://localhost:8000 in Chrome
```

### Pubblicarlo su GitHub Pages

1. Crea un repository e carica **il contenuto** di questa cartella (`index.html`, `sw.js`, `manifest.webmanifest`, `icons/`, `README.md`).
2. *Settings → Pages → Deploy from a branch →* `main` / `(root)`.
3. Apri l'indirizzo `https://<utente>.github.io/<repository>/`.

I percorsi sono tutti relativi, quindi funziona anche in una sottocartella.

### Installarlo come app

In **Chrome** (consigliato) apri l'indirizzo e scegli *Installa S33 - Nostos One* dalla barra degli indirizzi (o dal menu ⋮). Sul Mac compare come applicazione a sé, con la sua icona.

### Suonare

1. Collega la tastiera MIDI **prima** di aprire la pagina (o ricarica dopo averla collegata).
2. Premi **Avvia audio** (il browser richiede un gesto dell'utente) e concedi l'accesso MIDI.
3. Scegli un preset dal menu e suona.

Senza tastiera MIDI puoi usare la tastiera sullo schermo oppure i tasti del computer: `A W S E D F T G Y H U J K` corrispondono a una ottava da Do centrale.

| | Chrome / Edge | Safari | Firefox |
|---|---|---|---|
| Audio e interfaccia | sì | sì | sì |
| Ingresso MIDI (Web MIDI) | **sì** | no | limitato |

---

## Cosa c'è dentro

### Moduli

Tastiera (nota e velocity), Oscillatore (sinusoide, triangolo, sega, quadra; ottava, semitono, fine), **Osc sync** (hard sync con rapporto modulabile), **FM** (rapporto e indice, con ingresso per modulare l'indice), Rumore, LFO, Mixer a 3 ingressi, **Filtro a scala** (24 dB/ott., risonante, con saturazione), **Passa-alto**, ADSR, VCA, Rotella mod, Delay (anche **sincronizzato al tempo**), Riverbero, Chorus, Uscita.

I moduli a sinistra della linea tratteggiata sono **per voce** (copiati 8 volte); quelli a destra sono **master** (una sola volta). La posizione del modulo decide a quale zona appartiene.

### Polifonia

1, 4 o 8 voci con assegnazione e "furto" della voce più vecchia, glide, pitch bend e rotella mod (CC 1).

### Lavagna

- trascina da un cerchio all'altro per collegare; clic su un modulo o su un cavo per modificarlo;
- doppio clic su un cavo selezionato per eliminarlo;
- i cavi con una quantità (per esempio inviluppo → filtro) hanno la loro manopola nel pannello del modulo.

### Pannello

Il pannello si adatta al preset con un **modello** dedicato (Minimoog, FM, Organo, Percussioni, Pad, Prophet, Farfisa, Hammond, Rhodes, Pianoforte) oppure in modalità **Automatica**, che mostra le manopole di tutti i moduli. Comprende:

- **Arpeggiatore** con modi su / giù / su-giù / casuale / ordine, divisioni anche in terzine, 1–4 ottave, gate, swing, latch e tempo in BPM;
- **Dinamica del tocco** da velocity MIDI (vedi sotto);
- monitor di **preset**, **oscilloscopio** e **spettro**, ridimensionabili;
- tastiera sullo schermo con rotelle pitch e mod, e display dell'ottava.

### Dinamica del tocco

La velocity MIDI agisce su **volume** e **brillantezza** (apre filtri e indice FM). Si può:

- attivare e disattivare per tutto il synth (**DINAMICA**);
- escludere per il singolo preset (**QUESTO PRESET**): organi e string machine partono già senza, come gli strumenti originali;
- regolare **curva** (morbida / normale / dura), **volume** e **brillantezza**.

Sulla Lavagna il modulo Tastiera ha anche un'uscita `velocity` per collegarla a qualunque parametro.

### Vesti grafiche

Dal menu **Veste**: Automatica (segue chiaro/scuro del sistema), Moog avorio, Prophet nero, Grafite, Neon, Blueprint. Le manopole hanno un aspetto in rilievo che cambia con la veste.

### Preset (46)

| Gruppo | Preset |
|---|---|
| **Iconici** | Patch di partenza, Basso Moog, Pad caldo, Archi, Supersaw, Aria, Abisso, Ottoni, Lead solista, Pizzicato, Campana FM, Organo, Soffio e colpo, **Shine On - P III**, **Shine On - P I**, Arpeggio notturno, Cassa sintetica, Rullante, Hi-hat chiuso, Hi-hat aperto, Piatto, Piano elettrico FM |
| **Prophet-5** | One of My Turns (intro), sync lead (Run Like Hell) |
| **String machine** | ARP String Ensemble – violini, celli e viole |
| **Organi** | Hammond C3 888000000 (Leslie lento / veloce), Farfisa Compact (Flute-Oboe-Flute 4′, Booster) |
| **Piani elettrici** | Fender Rhodes Mark I, Suitcase (tremolo) |
| **Arpeggiatore** | Pluck secco, Staccato digitale, Basso sequencer, Flauto morbido, Velluto, Kalimba, Campanelle FM, Metallo FM, Acciaio (sync) |
| **Pianoforte** | Piano a coda, verticale, morbido (feltro), brillante (pop), con pedale (ambiente) |

I preset che richiamano brani o strumenti reali sono **ricostruzioni a orecchio e da descrizioni trovate online**, non copie: valgono come punto di partenza da rifinire (vedi [Fonti](#fonti-dei-preset)).

### Salvare, esportare, importare

- **Salva come…** conserva il tuo preset nel browser;
- **Esporta** / **Esporta miei** scarica uno o più preset in un file `.json`;
- **Importa** carica un file: viene controllato e normalizzato (moduli e porte sconosciuti, valori fuori scala, anelli e cavi non validi vengono rifiutati o riportati nei limiti).

Il file contiene solo dati, mai codice.

---

## Struttura della cartella

```
index.html               l'intera applicazione (HTML, CSS e JavaScript)
manifest.webmanifest     nome, icone e colori dell'app
sw.js                    service worker: funzionamento offline
icons/                   icone (192, 512, maskable, Apple touch, favicon, SVG sorgente)
README.md                questo file
```

### Aggiornare l'app

Il service worker risponde dalla cache e si aggiorna in silenzio quando c'è rete: dopo aver pubblicato una modifica, la versione nuova compare **alla seconda apertura**. Per forzare il rinnovo cambia `VERSIONE` in `sw.js`.

### Dati salvati nel browser (`localStorage`)

`synth-lavagna-v3` (patch corrente), `synth-user-presets`, `synth-mon-w` (larghezza dei monitor), `synth-skin` (veste), `synth-vel` (dinamica).

---

## Limiti noti

- **Non è stato valutato all'ascolto da chi l'ha scritto**: la struttura, l'importazione, l'offline e il comportamento dell'audio sono stati verificati con test automatici, ma i suoni vanno giudicati e ritoccati a orecchio.
- Niente phaser, spring reverb e Leslie veri (il Leslie dei preset Hammond è approssimato con tremolo e chorus); le string machine non hanno il phaser dell'originale.
- Il pianoforte e il Rhodes sono **sintesi FM**, non campioni: suonano "da piano sintetico".
- Ingressi MIDI gestiti: note con velocity, pitch bend, CC 1. **Non** gestiti: pedale di sustain, aftertouch, program change.
- Il mixer ha 3 ingressi (se ne servono di più si concatenano due mixer).
- Il filtro a scala usa un AudioWorklet; se non è disponibile il browser ripiega su un filtro più semplice.
- Il MIDI funziona solo in Chrome e in Edge e solo in HTTPS o su `localhost`.

## Idee per il futuro

- pacchetto desktop con **Electron** o **Tauri**;
- pedale di sustain e aftertouch;
- altri moduli (phaser, waveshaper, sequencer a passi).

---

## Fonti dei preset

Le impostazioni dei preset ispirati a strumenti reali derivano da descrizioni pubbliche, non da dati ufficiali:

- [Pink Floyd – One of My Turns, intro (matrixsynth)](https://www.matrixsynth.com/2008/09/pink-floyd-one-of-my-turns-intro.html)
- [Sound On Sound – ricreare il suono delle vecchie string machine](https://www.soundonsound.com/sound-advice/q-how-do-re-create-sound-those-old-string-synths)
- [Farfisa Compact (combo-organ.com)](https://www.combo-organ.com/Farfisa/Compact/)
- [Hammond organ (Wikipedia)](https://en.wikipedia.org/wiki/Hammond_organ)
- [I sintetizzatori dei Pink Floyd (gearnews)](https://gearnews.com/synths-of-pink-floyd-synth/)

Il carattere del titolo è **Oswald** (SIL Open Font License), incorporato nel file.

Moog, Minimoog, Sequential, Prophet-5, ARP, Solina, Hammond, Farfisa, Fender, Rhodes e gli altri nomi citati appartengono ai rispettivi proprietari e sono usati solo per indicare il carattere sonoro a cui un preset si ispira. Nessun legame con i marchi citati.

## Uso

Progetto a uso personale. Se lo pubblichi, scegli tu la licenza da aggiungere.
