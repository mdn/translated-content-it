---
title: Introduzione alla riga di comando
short-title: Riga di comando
slug: Learn_web_development/Getting_started/Environment_setup/Command_line
l10n:
  sourceCommit: 0915a5e602d475bd1a1a57d905f0bac1b7ed57b8
---

{{PreviousMenuNext("Learn_web_development/Getting_started/Environment_setup/Dealing_with_files", "Learn_web_development/Getting_started/Your_first_website", "Learn_web_development/Getting_started/Environment_setup")}}

Nel tuo processo di sviluppo, sarai inevitabilmente chiamato a eseguire alcuni comandi nel terminale (o sulla "riga di comando" — questi termini sono essenzialmente sinonimi). Questo articolo fornisce un'introduzione al terminale, ai comandi essenziali che dovrai digitare, a come concatenare i comandi tra loro e a come aggiungere i propri strumenti di interfaccia a riga di comando (CLI).

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con il sistema operativo del tuo computer, il software di base che utilizzerai per costruire un sito web e i sistemi di file.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Cos'è la riga di comando e cosa puoi fare con essa.</li>
          <li>Comprendere come accedere alla riga di comando su diversi sistemi.</li>
          <li>Conoscere le scorciatoie da tastiera di base (ad esempio, la freccia su per accedere ai comandi precedenti, tab per autocompletare).</li>
          <li>Conoscere i comandi di base (ad esempio <code>cd</code>, <code>ls</code>, <code>mkdir</code>, <code>touch</code>, <code>grep</code>, <code>cat</code>, <code>mv</code>, <code>cp</code>).</li>
          <li>Opzioni/flag dei comandi.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Benvenuto nel terminale

Il terminale è un'interfaccia testuale per eseguire programmi basati su testo. Se stai utilizzando strumenti per lo sviluppo web, è praticamente garantito che dovrai aprire la riga di comando ed eseguire alcuni comandi per utilizzare gli strumenti scelti (questi strumenti sono spesso denominati **strumenti CLI** — strumenti di interfaccia a riga di comando).

Un gran numero di strumenti possono essere utilizzati digitando comandi nella riga di comando; molti sono preinstallati sul sistema e un'infinità di altri sono installabili dai registri di pacchetti. I registri di pacchetti sono come negozi di app, ma (principalmente) per strumenti e software basati sulla riga di comando. Vedremo come installare alcuni strumenti più avanti in questo capitolo, e apprenderemo di più sui registri di pacchetti nel prossimo capitolo.

Una delle critiche più grandi alla riga di comando è che manca enormemente in termini di esperienza utente. La prima volta che si visualizza la riga di comando può sembrare un'esperienza scoraggiante: uno schermo vuoto e un cursore lampeggiante, con pochissimi aiuti evidenti disponibili su cosa fare.

A prima vista, possono sembrare poco accoglienti, ma ci sono molte cose che puoi fare con loro, e ti promettiamo che, con un po' di guida e pratica, utilizzarle diventerà più facile! Ecco perché forniamo questo capitolo — per aiutarti a iniziare in questo ambiente apparentemente poco amichevole.

### Da dove viene il terminale?

Il terminale ha origine intorno agli anni '50-'60 e la sua forma originale non somiglia affatto a ciò che usiamo oggi (per questo dovremmo essere grati). Puoi leggere un po' di storia nella voce di Wikipedia sul [Terminale del computer](https://en.wikipedia.org/wiki/Computer_terminal).

Da allora, il terminale è rimasto una caratteristica costante di tutti i sistemi operativi, dalle macchine desktop ai server nascosti nel cloud, ai microcomputer come il Raspberry PI Zero, e persino ai telefoni cellulari. Fornisce accesso diretto al file system sottostante del computer e a funzioni di basso livello, ed è quindi incredibilmente utile per eseguire rapidamente compiti complessi, se sai cosa stai facendo.

È anche utile per l'automazione — per esempio, per scrivere un comando che aggiorni istantaneamente i titoli di centinaia di file, diciamo da "ch01-xxxx.png" a "ch02-xxxx.png". Se aggiornassi i nomi dei file utilizzando il tuo programma GUI come Finder o Esplora file, ti ci vorrebbe molto tempo.

Comunque, il terminale non scomparirà presto.

### Come appare il terminale?

Di seguito puoi vedere alcuni dei diversi programmi che sono disponibili per accedere a un terminale.

Le immagini seguenti mostrano i prompt dei comandi disponibili in Windows — c'è una buona gamma di opzioni dal programma "cmd" al "powershell" — che può essere eseguito dal menu Start digitando il nome del programma.

![Una finestra della riga di comando cmd di Windows e una finestra di PowerShell di Windows](win-terminals.png)

E di seguito, puoi vedere l'applicazione terminale di macOS.

![Un terminale macOS di base](mac-terminal.png)

### Come si accede al terminale?

Molti sviluppatori oggi stanno utilizzando strumenti basati su Unix (ad esempio, il terminale e gli strumenti che puoi accedere tramite esso). Molti tutorial e strumenti presenti oggi sul web supportano (e purtroppo assumono) sistemi basati su Unix, ma non preoccuparti — sono disponibili sulla maggior parte dei sistemi. In questa sezione, vedremo come ottenere l'accesso al terminale sul sistema scelto.

#### Linux/Unix

Come accennato sopra, i sistemi Linux/Unix hanno un terminale disponibile di default, elencato tra le tue applicazioni.

#### macOS

macOS ha un sistema chiamato Darwin che si trova sotto l'interfaccia grafica. Darwin è un sistema simile a Unix, che fornisce il terminale e l'accesso agli strumenti di basso livello. Darwin di macOS è in gran parte equivalente a Unix, certamente abbastanza buono da non causarci preoccupazioni mentre lavoriamo su questo articolo.

Il terminale è disponibile su macOS in `Applicazioni/Utility/Terminale`.

#### Windows

Come con alcuni altri strumenti di programmazione, utilizzare il terminale (o riga di comando) su Windows non è stato tradizionalmente semplice o facile come su altri sistemi operativi. Ma le cose stanno migliorando.

Windows ha tradizionalmente avuto un programma simile a un terminale chiamato `cmd` ("prompt dei comandi") per molto tempo, ma questo non è equivalente ai comandi Unix, ed è equivalente al vecchio prompt DOS di Windows.

Esistono programmi migliori per fornire un'esperienza di terminale su Windows, come PowerShell ([vedi qui per trovare installatori](https://github.com/PowerShell/PowerShell)) e Gitbash (che viene fornito come parte del [git per Windows](https://gitforwindows.org/) toolkit).

Tuttavia, la migliore opzione per Windows al giorno d'oggi è il Windows Subsystem for Linux (WSL) — un livello di compatibilità per eseguire sistemi operativi Linux direttamente dall'interno di Windows 10, consentendoti di eseguire un "vero terminale" direttamente su Windows, senza bisogno di una macchina virtuale.

Questo può essere installato direttamente dallo store di Windows gratuitamente. Puoi trovare tutta la documentazione di cui hai bisogno nella [Documentazione del Windows Subsystem for Linux](https://learn.microsoft.com/en-us/windows/wsl/).

![uno screenshot della documentazione del Windows Subsystem for Linux](wsl.png)

In termini di quale opzione scegliere su Windows, raccomandiamo fortemente di provare a installare il WSL. Potresti restare con il prompt dei comandi predefinito (`cmd`), e molti strumenti funzioneranno bene, ma troverai tutto più facile se hai una maggiore parità con gli strumenti Unix.

#### Nota a margine: qual è la differenza tra riga di comando e terminale?

In generale, troverai questi due termini usati in modo intercambiabile. Tecnicamente, un terminale è un software che avvia e si connette a una shell. Una shell è la tua sessione e ambiente di sessione (dove cose come il prompt e le scorciatoie possono essere personalizzate). La riga di comando è la linea letterale dove inserisci i comandi e il cursore lampeggia.

### Devi usare il terminale?

Anche se c'è una grande ricchezza di strumenti disponibili dalla riga di comando, se stai usando strumenti come [Visual Studio Code](https://code.visualstudio.com/) c'è anche una massa di estensioni che possono essere utilizzate come proxy per usare comandi del terminale senza bisogno di usare direttamente il terminale. Tuttavia, non troverai un'estensione per editor di codice per tutto ciò che vuoi fare — dovrai acquisire un po' di esperienza con il terminale alla fine.

## Comandi di terminale di base integrati

Abbastanza parole — iniziamo a guardare alcuni comandi del terminale! Subito fuori dalla scatola, ecco solo alcune delle cose che la riga di comando può fare, insieme ai nomi degli strumenti rilevanti in ogni caso:

- Navigare il sistema di file del tuo computer insieme a compiti di base come creare, copiare, rinominare ed eliminare:

  - Muoversi nella struttura delle directory (cartelle): `cd`
  - Creare directory: `mkdir`
  - Creare file (e modificarne i metadati): `touch`
  - Copiare file o directory: `cp`
  - Spostare file o directory: `mv`
  - Eliminare file o directory: `rm`

- Scaricare file trovati a URL specifici: `curl`
- Cercare frammenti di testo dentro corpi di testo più grandi: `grep`
- Visualizzare il contenuto di un file pagina per pagina: `less`, `cat`
- Manipolare e trasformare flussi di testo (ad esempio cambiando tutte le istanze di `<div>` in un file HTML in `<article>`): `awk`, `tr`, `sed`

> [!NOTE]
> Ci sono una serie di buoni tutorial sul web che approfondiscono molto di più la riga di comando — questa è solo una breve introduzione!

Andiamo avanti e vediamo come usare alcuni di questi strumenti sulla riga di comando. Prima di andare oltre, apri il tuo programma di terminale!

### Navigazione sulla riga di comando

Quando visiti la riga di comando inevitabilmente dovrai navigare verso una particolare directory per "fare qualcosa". Tutti i sistemi operativi (supponendo una configurazione predefinita) avvieranno il loro programma terminale nella tua directory _Home_, e da lì probabilmente vorrai spostarti in un altro posto.

> [!NOTE]
> "Directory" è il termine tecnico per ciò che abbiamo chiamato "cartella" nell'articolo precedente. Quando si guarda alla struttura dei file all'interno di un'interfaccia utente (UI), il termine "cartella" ha più senso, poiché le icone usate assomigliano a cartelle fisiche di archiviazione vecchio stile. Tuttavia, tende ad essere usato spesso anche il termine "directory", soprattutto quando si parla di manipolare file utilizzando la riga di comando. Ci sono sfumature, ma i due termini essenzialmente significano la stessa cosa.

Il comando `cd` ti permette di cambiare directory. Tecnicamente, cd non è un programma ma un built-in. Questo significa che il tuo sistema operativo lo fornisce subito e significa anche che non puoi accidentalmente eliminarlo — per fortuna! Non devi preoccuparti troppo se un comando è built-in o no, ma tieni presente che i built-in appaiono su tutti i sistemi basati su Unix.

Per cambiare la directory, digita `cd` nel tuo terminale, seguito dalla directory in cui vuoi muoverti. Supponendo che la directory sia all'interno della tua directory home, puoi usare `cd Desktop` (vedi gli screenshot sotto).

![risultati del comando cd Desktop eseguito in una varietà di terminali Windows - la posizione del terminale si sposta sul desktop](win-terminals-cd.png)

Prova a digitare questo nel terminale del tuo sistema:

```bash
cd Desktop
```

Se vuoi risalire alla directory precedente, puoi usare due punti:

```bash
cd ..
```

> [!NOTE]
> Una scorciatoia del terminale molto utile è usare il tasto <kbd>tab</kbd> per autocompletare i nomi che sai essere presenti, piuttosto che dover digitare tutto. Ad esempio, dopo aver digitato i due comandi sopra, prova a digitare `cd D` e premere <kbd>tab</kbd> — dovrebbe completare automaticamente il nome della directory `Desktop` per te, a condizione che sia presente nella directory corrente. Tieni questo a mente mentre procedi.

Se la directory che vuoi visitare è annidata in profondità, devi conoscere il percorso per arrivarci. Questo di solito diventa più facile man mano che acquisisci familiarità con la struttura del tuo file system, ma se non sei sicuro del percorso, puoi generalmente risolverlo con una combinazione del comando `ls` (vedi sotto) e cliccando intorno nella finestra di Esplora/Finder per vedere dove si trova una directory, rispetto a dove sei attualmente.

Ad esempio, se volessi andare in una directory chiamata `src`, situata dentro una directory chiamata `project`, situata su _Desktop_, potresti digitare questi tre comandi per arrivarci dalla tua directory _Home_:

```bash
cd Desktop
cd project
cd src
```

Ma questo è uno spreco di tempo — invece, puoi digitare un solo comando, con i diversi elementi nel percorso separati da barre avanti, proprio come fai quando specifichi i percorsi per immagini o altri asset nel codice CSS, HTML o JavaScript:

```bash
cd Desktop/project/src
```

Nota che includere una barra iniziale nel tuo percorso rende il percorso assoluto, ad esempio `/Users/tuo-nome-utente/Desktop`. Omettere la barra iniziale, come abbiamo fatto sopra, rende il percorso relativo alla tua directory corrente. Questo è esattamente lo stesso che vedresti con gli URL nel tuo browser web. Una barra iniziale significa "alla radice del sito web", mentre omettere la barra significa "l'URL è relativo alla mia pagina corrente".

> [!NOTE]
> Su Windows, utilizzi le barre retro invece delle barre avanti, ad esempio, `cd Desktop\project\src` — questo può sembrare davvero strano, ma se sei interessato a sapere perché, [guarda questo clip su YouTube](https://www.youtube.com/watch?v=5T3IJfBfBmI) con una spiegazione di uno dei principali ingegneri di Microsoft.

### Elencare il contenuto della directory

Un altro comando Unix integrato è `ls` (abbreviazione di lista), che elenca il contenuto della directory in cui ti trovi attualmente. Nota che questo non funzionerà se stai usando il prompt dei comandi predefinito di Windows (`cmd`) — l'equivalente lì è `dir`.

Prova a eseguirlo subito nel tuo terminale:

```bash
ls
```

Questo ti dà un elenco dei file e delle directory nella tua directory di lavoro corrente, ma l'informazione è davvero essenziale — ottieni solo il nome di ogni elemento presente, non se è un file o una directory, o qualsiasi altra cosa. Fortunatamente, una piccola modifica all'uso del comando può darti molte più informazioni.

### Introduzione alle opzioni dei comandi

La maggior parte dei comandi del terminale ha opzioni — sono dei modificatori che aggiungi alla fine di un comando, che lo fanno comportare in modo leggermente diverso. Questi solitamente consistono in uno spazio dopo il nome del comando, seguito da un trattino, seguito da una o più lettere.

Ad esempio, prova a eseguire questo comando e vedi cosa ottieni:

```bash
ls -l
```

Nel caso di `ls`, l'opzione `-l` (_trattino ell_) ti dà un elenco con un file o directory per ciascuna riga, e molte più informazioni mostrate. Le directory possono essere identificate cercando una lettera "d" sul lato sinistro delle righe. Quelle sono quelle in cui possiamo entrare con `cd`.

Sotto trovi uno screenshot con un terminale macOS "vanilla" in alto, e un terminale personalizzato con alcune icone e colori extra per mantenerlo vivace — entrambi mostrano i risultati dell'esecuzione di `ls -l`:

![Un terminale macOS standard e un terminale macOS più colorato e personalizzato, mostrando un elenco di file - il risultato di eseguire il comando ls -l](mac-terminals-ls.png)

> [!NOTE]
> Per scoprire esattamente quali opzioni ciascun comando ha disponibili, puoi guardare la sua [pagina man](https://en.wikipedia.org/wiki/Man_page). Questo viene fatto digitando il comando `man`, seguito dal nome del comando che vuoi cercare, ad esempio `man ls`. Questo aprirà la pagina man nel visualizzatore di file di testo predefinito del terminale (ad esempio, [`less`](<https://en.wikipedia.org/wiki/Less_(Unix)>) nel mio terminale), e dovresti quindi essere in grado di scorrere la pagina usando i tasti freccia, o un meccanismo simile. La pagina man elenca tutte le opzioni in grande dettaglio, il che può essere un po' intimidatorio all'inizio, ma almeno sai che è lì se ne hai bisogno. Una volta che hai finito di guardare la pagina man, devi uscire usando il comando di uscita del visualizzatore di testo ("q" in `less`; potresti dover cercare sul web per trovarlo se non è ovvio).

> [!NOTE]
> Per eseguire un comando con più opzioni contemporaneamente, di solito puoi metterle tutte in una stringa singola dopo il carattere trattino, ad esempio `ls -lah`, o `ls -ltrh`. Prova a guardare la pagina man di `ls` per capire cosa fanno queste opzioni extra!

Ora che abbiamo discusso due comandi fondamentali, esplora un po' la tua directory e vedi se riesci a navigare da un posto all'altro.

### Creazione, copia, spostamento, rimozione

Ci sono diversi altri comandi di utilità di base che probabilmente finirai per usare abbastanza frequentemente mentre lavori con il terminale. Sono abbastanza semplici, quindi non li spiegheremo tutti in dettaglio come i due precedenti.

Gioca con loro in una directory di prova che hai creato da qualche parte in modo da non eliminare accidentalmente qualcosa di importante, usando i comandi di esempio qui sotto come guida:

- `mkdir` — crea una nuova directory all'interno della directory corrente in cui ti trovi, con il nome che fornisci dopo il nome del comando. Ad esempio, `mkdir my-awesome-website` farà una nuova directory chiamata `my-awesome-website`.
- `rmdir` — rimuove la directory nominata, ma solo se è vuota. Ad esempio, `rmdir my-awesome-website` rimuoverà la directory abbiamo creato sopra. Se vuoi rimuovere una directory che non è vuota (e rimuovere anche tutto ciò che contiene), allora puoi usare `rm -r` invece (vedi sotto), ma questo è pericoloso. Assicurati che non ci sia niente di cui potresti avere bisogno all'interno della directory in seguito, poiché andrà perduto per sempre.
- `touch` — crea un nuovo file vuoto, all'interno della directory corrente. Ad esempio, `touch mdn-example.md` crea un nuovo file vuoto chiamato `mdn-example.md`.
- `mv` — sposta un file dalla prima posizione specificata alla seconda posizione specificata, ad esempio `mv mdn-example.md mdn-example.txt` (le posizioni sono scritte come percorsi di file). Questo comando sposta un file chiamato `mdn-example.md` nella directory corrente a un file chiamato `mdn-example.txt` nella directory corrente. Tecnicamente il file viene spostato, ma da un punto di vista pratico, questo comando sta effettivamente rinominando il file.
- `cp` — simile nell'uso a `mv`, `cp` crea una copia del file nella prima posizione specificata, nella seconda posizione specificata. Ad esempio, `cp mdn-example.txt mdn-example.txt.bak` crea una copia `mdn-example.txt` chiamata `mdn-example.txt.bak` (puoi naturalmente chiamarla qualcosa di diverso se lo desideri).
- `rm` — rimuove il file specificato. Ad esempio, `rm mdn-example.txt` elimina un singolo file chiamato `mdn-example.txt`. Nota che questa cancellazione è permanente e non può essere annullata tramite il cestino che potresti avere sulla tua interfaccia utente desktop.

> [!NOTE]
> Molti comandi del terminale ti consentono di usare asterischi come caratteri "wild card", che significano "qualsiasi sequenza di caratteri". Questo ti permette di eseguire un'operazione contro un numero potenzialmente grande di file alla volta, tutti i quali corrispondono al modello specificato. Come esempio, `rm mdn-*` cancellerebbe tutti i file che iniziano con `mdn-`. `rm mdn-*.bak` cancellerebbe tutti i file che iniziano con `mdn-` e terminano con `.bak`.

## Terminale — considerato dannoso?

Abbiamo accennato prima a questo, ma per essere chiari — devi essere prudente con il terminale. I comandi semplici non comportano troppi pericoli, ma man mano che inizi a mettere insieme comandi più complessi, devi pensare attentamente a cosa farà il comando e provare a testarli prima di eseguirli effettivamente nella directory voluta.

Diciamo che hai 1000 file di testo in una directory e vuoi passarli tutti e cancellare solo quelli che hanno una certa sottostringa all'interno del nome file. Se non stai attento, potresti finire per eliminare qualcosa di importante, perdendo così un sacco di lavoro nel processo. Una buona abitudine è scrivere il tuo comando del terminale in un editor di testo, capire come pensi debba apparire, quindi fare una copia di backup della tua directory e provare a eseguire il comando su quella prima, per testarlo.

Se non ti senti a tuo agio a provare comandi del terminale sul tuo stesso computer, ci sono terminali ospitati online disponibili che forniscono posti sicuri per esercitarsi a inserire comandi, senza rischiare di rompere il proprio computer:

- Il nostro partner di apprendimento, [Scrimba](https://scrimba.com/home?via=mdn), offre un terminale per l'inserimento di comandi nel loro ambiente di apprendimento. Un ottimo posto per vedere questo in azione è il loro corso [Command Line Basics](https://scrimba.com/command-line-basics-c08b87ogl0/~05hu?via=mdn) <sup>[_MDN learning partner_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>, che fornisce anche un'introduzione interattiva e divertente alla navigazione nell'albero dei file e alla manipolazione di file e directory tramite il terminale.
- [Glitch.com](https://glitch.com/) è un ottimo posto per provare il codice di sviluppo web e i progetti Glitch includono anche un terminale per eseguire comandi.

Una grande risorsa per ottenere una rapida panoramica dei comandi specifici del terminale è [tldr.sh](https://tldr.sh/). Questo è un servizio di documentazione guidato dalla comunità, simile a MDN, ma specifico per i comandi del terminale.

Nella prossima sezione, facciamo un passo avanti (o diversi passi in effetti) e vediamo come possiamo collegare gli strumenti sulla riga di comando per vedere davvero come il terminale può essere vantaggioso rispetto all'interfaccia utente desktop regolare.

## Collegare i comandi insieme con le pipe

Il terminale dà il meglio di sé quando inizi a concatenare i comandi insieme usando il simbolo `|` (pipe). Vediamo un esempio molto rapido di cosa significa questo.

Abbiamo già visto `ls`, che visualizza il contenuto della directory corrente:

```bash
ls
```

Ma cosa succede se volessimo contare rapidamente il numero di file e directory all'interno della directory corrente? `ls` non può farlo da solo.

Esiste un altro strumento Unix chiamato `wc`. Questo conta il numero di parole, righe, caratteri o byte di qualunque cosa venga passata come input. Questo può essere un file di testo — l'esempio sotto riporta il numero di righe in `myfile.txt`:

```bash
wc -l myfile.txt
```

Ma può anche contare il numero di righe di qualunque output venga **pipato** in esso. Ad esempio, il comando sottostante conta il numero di righe visualizzate dal comando `ls` (ciò che visualizzerebbe normalmente nel terminale se eseguito da solo) e visualizza quel conteggio nel terminale invece:

```bash
ls | wc -l
```

Poiché `ls` stampa ciascun file o directory sulla propria riga, ciò ci dà effettivamente un conteggio di directory e file.

Allora, cosa sta succedendo qui? Una filosofia generale degli strumenti della linea di comando (unix) è che stampano testo nel terminale (chiamato anche "stampa sull'output standard" o `STDOUT`). Un buon numero di comandi può anche leggere contenuti da input stream (conosciuti come "input standard" o `STDIN`).

L'operatore pipe può _collegare_ questi entrate e uscite insieme, permettendoci di costruire operazioni sempre più complesse per soddisfare le nostre esigenze — l'output di un comando può diventare l'input del comando successivo. In questo caso, `ls` normalmente stamperebbe il suo output su `STDOUT`, ma invece l'output di `ls` viene pipato in `wc`, che prende quell'output come input, conta il numero di linee che contiene, e stampa quel conteggio su `STDOUT` al suo posto.

## Un esempio leggermente più complesso

Vediamo qualcosa di un po' più complicato.

Prima proveremo a recuperare il contenuto della pagina "fetch" di MDN usando il comando `curl` (che può essere usato per richiedere contenuti da URL), da `https://developer.mozilla.org/it/docs/Web/API/WindowOrWorkerGlobalScope/fetch`. Provalo ora:

```bash
curl https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/fetch
```

Non otterrai un output perché la pagina è stata reindirizzata (a [/Web/API/fetch](/it/docs/Web/API/Window/fetch)). Dobbiamo esplicitamente dire a `curl` di seguire i reindirizzamenti usando il flag `-L`.

Diamos anche un'occhiata alle intestazioni che `developer.mozilla.org` ritorna usando il flag `-I` di `curl`, e stampiamo tutti i reindirizzamenti alla posizione che invia al terminale, indirizzando l'output di `curl` in `grep` (chiederemo a `grep` di restituire tutte le righe che contengono la parola "location").

Prova a eseguire il seguente (vedrai che c'è solo un reindirizzamento prima di raggiungere la pagina finale):

```bash
curl https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/fetch -L -I | grep location
```

Il tuo output dovrebbe apparire come questo (`curl` visualizzerà per primo alcuni contatori di download e simili):

```bash
location: /en-US/docs/Web/API/Window/fetch
```

Sebbene artificioso, potremmo sviluppare ulteriormente questo risultato trasformando i contenuti della riga `location:`, aggiungendo l'origine di base all'inizio di ciascuno di essi in modo che otteniamo URL completi stampati fuori. Per questo, aggiungeremo `awk` al mix (che è un linguaggio di programmazione simile a JavaScript o Ruby o Python, solo molto più vecchio!).

Prova a eseguire questo:

```bash
curl https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/fetch -L -I | grep location | awk '{ print "https://developer.mozilla.org" $2 }'
```

Il tuo output finale dovrebbe apparire come questo:

```bash
https://developer.mozilla.org/en-US/docs/Web/API/Window/fetch
```

Combinando questi comandi abbiamo personalizzato l'output per mostrare gli URL completi attraverso cui il server Mozilla sta reindirizzando quando richiediamo l'URL `/docs/Web/API/WindowOrWorkerGlobalScope/fetch`. Conoscere il tuo sistema si rivelerà utile negli anni a venire — impara come questi strumenti monouso funzionano e come possono diventare parte della tua cassetta degli attrezzi per risolvere problemi di nicchia.

## Potenziare

Ora che abbiamo visto alcuni dei comandi integrati con cui è equipaggiato il tuo sistema, vediamo come possiamo installare uno strumento CLI di terze parti e farne uso.

Il vasto ecosistema di strumenti installabili per lo sviluppo web frontend attualmente esiste principalmente all'interno [npm](https://www.npmjs.com/), un servizio privato di hosting di pacchetti che lavora a stretto contatto con Node.js. Questo si sta lentamente espandendo — ci si può aspettare di vedere più fornitori di pacchetti man mano che il tempo passa.

[Installare Node.js](https://nodejs.org/en/) installa anche il comando npm a riga di comando (e uno strumento supplementare orientato a npm chiamato npx), che offre un gateway per l'installazione di ulteriori strumenti a riga di comando. Node.js e npm funzionano allo stesso modo su tutti i sistemi: macOS, Windows e Linux.

Installa npm sul tuo sistema ora, andando all'URL sopra e scaricando ed eseguendo un installer Node.js appropriato per il tuo sistema operativo. Se richiesto, assicurati di includere npm come parte dell'installazione.

![l'installer di Node.js su Windows, mostrando l'opzione per includere npm](npm-install-option.png)

Useremo di nuovo [Prettier](https://prettier.io/) come esempio qui. Abbiamo mostrato come installarlo come estensione per VS Code nel nostro articolo [Editor di codice](/it/docs/Learn_web_development/Getting_started/Environment_setup/Code_editors#enhancing_your_code_editor_with_extensions). Qui ti mostreremo come installarlo come strumento a riga di comando.

> [!NOTE]
> Prettier è un formattatore di codice opinato che ha solo "alcune opzioni". Meno opzioni tende a significare più semplice. Considerato che gli strumenti possono a volte diventare fuori controllo in termini di complessità, "poche opzioni" può essere molto allettante.

### Dove installare i nostri strumenti CLI?

Prima di procedere con l'installazione di Prettier, c'è una domanda da rispondere — "dove dovremmo installarlo?"

Con `npm` abbiamo la scelta di installare gli strumenti globalmente — in modo che possano essere accessibili ovunque — o localmente alla directory del progetto corrente.

Ci sono pro e contro in entrambi i modi — e le seguenti liste di pro e contro per l'installazione globale non sono affatto esaustive.

**Pro dell'installazione globale:**

- Accessibile ovunque nel tuo terminale
- Installazione una sola volta
- Consuma meno spazio su disco
- Sempre la stessa versione
- Sembra come qualsiasi altro comando unix

**Contro dell'installazione globale:**

- Potrebbe non essere compatibile con il codice del tuo progetto
- Altri sviluppatori nel tuo team non avranno accesso a questi strumenti, ad esempio se stai condividendo il codice tramite uno strumento come git.
- Riferito al punto precedente, rende più difficile replicare il codice del progetto (se installi gli strumenti localmente, possono essere configurati come dipendenze e installati con <code>npm install</code>).

Anche se l'elenco dei _contro_ è più breve, l'impatto negativo dell'installazione globale è potenzialmente molto più grande rispetto ai benefici. Qui installeremo localmente, ma sentiti libero di installare globalmente una volta che comprendi i rischi relativi.

### Installazione di Prettier

Prettier è uno strumento di formattazione del codice opinato per sviluppatori frontend, concentrandosi sui linguaggi basati su JavaScript e aggiungendo supporto per HTML, CSS, SCSS, JSON e altro ancora.

Prettier può:

- Risparmiare il sovraccarico cognitivo di mantenere manualmente lo stile coerente attraverso tutti i tuoi file di codice; Prettier può farlo automaticamente per te.
- Aiutare i nuovi arrivati nello sviluppo web a formattare il loro codice nel modo migliore.
- Essere installato su qualsiasi sistema operativo e persino come parte integrante degli strumenti di progetto, garantendo che colleghi e amici che lavorano sul tuo codice utilizzino lo stile di codice che stai utilizzando.
- Essere configurato per essere eseguito al salvataggio, mentre scrivi o anche prima di pubblicare il tuo codice (con strumenti aggiuntivi che vedremo più avanti nel modulo).

Per questo articolo, installeremo Prettier localmente, come suggerito nella [guida all'installazione di Prettier](https://prettier.io/docs/install.html).

Dopo aver installato node, apri il terminale ed esegui il seguente comando per installare Prettier (spiegheremo cosa fa `--save-dev` nel prossimo articolo):

```bash
npm install --save-dev prettier
```

Puoi ora eseguire il file localmente usando lo strumento [npx](https://docs.npmjs.com/cli/commands/npx/). Eseguire il comando senza argomenti, come per molti altri comandi, offrirà informazioni di utilizzo e di aiuto. Prova ora:

```bash
npx prettier
```

Il tuo output dovrebbe apparire come segue:

```bash
Usage: prettier [options] [file/glob ...]

By default, output is written to stdout.
Stdin is read if it is piped to Prettier and no files are given.

…
```

Vale sempre la pena almeno scorrere rapidamente le informazioni sull'uso, anche se sono lunghe. Ti aiuterà a capire meglio come lo strumento è pensato per essere usato.

> [!NOTE]
> Se non hai installato Prettier localmente per primo, allora eseguendo `npx prettier` scaricherai ed eseguirai l'ultima versione di Prettier tutto in una volta _solo per quel comando_. Sebbene possa sembrare fantastico, nuove versioni di Prettier possono leggermente modificare l'output. Vuoi installarlo localmente in modo da fissare la versione di Prettier che stai usando per la formattazione fino a quando sei pronto per cambiarla.

### Giocare con Prettier

Facciamo una rapida prova con Prettier, in modo che tu possa vedere come funziona.

Prima di tutto, crea una nuova directory da qualche parte nel tuo sistema di file che sia facile da trovare. Forse una directory chiamata `prettier-test` sul tuo `Desktop`.

Ora salva il seguente codice in un nuovo file chiamato `index.js`, all'interno della tua directory di prova:

```js-nolint
const myObj = {
a:1,b:{c:2}}
function printMe(obj){console.log(obj.b.c)}
printMe(myObj)
```

Possiamo eseguire Prettier su una base di codice per verificare semplicemente se il nostro codice necessita di aggiustamenti. `cd` nella tua directory e prova ad eseguire questo comando:

```bash
npx prettier --check index.js
```

Dovresti ottenere un output simile a:

```bash
Checking formatting...
index.js
Code style issues found in the above file(s). Forgot to run Prettier?
```

Quindi, ci sono alcuni stili di codice che possono essere sistemati. Nessun problema. Aggiungendo l'opzione `--write` al comando `prettier` risolverà questi problemi, lasciandoci concentrare sulla scrittura di codice utile.

Ora prova a eseguire questa versione del comando:

```bash
npx prettier --write index.js
```

Otterrai un output del tipo

```bash
Checking formatting...
index.js
Code style issues fixed in the above file(s).
```

Ma più importante, se guardi al tuo file JavaScript troverai che è stato riformattato in qualcosa di simile a questo:

```js
const myObj = {
  a: 1,
  b: { c: 2 },
};
function printMe(obj) {
  console.log(obj.b.c);
}
printMe(myObj);
```

A seconda del tuo flusso di lavoro (o del flusso di lavoro che scegli), puoi far sì che questo diventi una parte automatizzata del tuo processo. L'automazione è davvero dove gli strumenti eccellono; la nostra preferenza personale è il tipo di automazione che "accade solo" senza bisogno di configurare alcunché.

Con Prettier ci sono diversi modi in cui l'automazione può essere realizzata e anche se sono al di fuori dell'ambito di questo articolo, ci sono eccellenti risorse online da consultare (alcune delle quali sono state collegate). Puoi invocare Prettier:

- Prima di commettere il tuo codice in un repository git usando [Husky](https://github.com/typicode/husky).
- Ogni volta che premi "salva" nel tuo editor di codice, sia esso [VS Code](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode), o [Sublime Text](https://packagecontrol.io/packages/JsPrettier).
- Come parte dei controlli di integrazione continua utilizzando strumenti come [GitHub Actions](https://github.com/features/actions).

La nostra preferenza personale è la seconda — mentre usiamo ad esempio VS Code, Prettier entra in funzione e ripulisce qualsiasi formattazione necessiti di essere effettuata ogni volta che premiamo salva. Puoi trovare molte più informazioni su come usare Prettier in modi diversi nella [documentazione di Prettier](https://prettier.io/docs/).

## Altri strumenti da provare

Se vuoi giocare con qualche altro strumento, ecco un breve elenco che è divertente provare:

- [`bat`](https://github.com/sharkdp/bat) — Un `cat` "più carino" (`cat` viene usato per stampare il contenuto dei file).
- [`prettyping`](https://denilson.sa.nom.br/prettyping/) — `ping` sulla riga di comando, ma visualizzato (`ping` è uno strumento utile per verificare se un server risponde).
- [`htop`](https://htop.dev/) — Un visualizzatore di processi, utile quando qualcosa sta facendo il tuo ventilatore della CPU comportarsi come un motore a reazione e vuoi identificare il programma responsabile.
- [`tldr`](https://tldr.sh/#installation) — menzionato prima in questo capitolo, ma disponibile come strumento della riga di comando.

Nota che alcune delle suggestioni sopra potrebbero necessitare di essere installate usando npm, come abbiamo fatto con Prettier.

## Riepilogo

Questo ci porta alla fine del nostro tour introduttivo del terminale/riga di comando e al modulo di configurazione dell'ambiente. La prossima volta, ti faremo lavorare sulla costruzione del tuo primo semplice sito web, in modo da ottenere un'idea di cosa significa sviluppo web.

{{PreviousMenuNext("Learn_web_development/Getting_started/Environment_setup/Dealing_with_files", "Learn_web_development/Getting_started/Your_first_website", "Learn_web_development/Getting_started/Environment_setup")}}
