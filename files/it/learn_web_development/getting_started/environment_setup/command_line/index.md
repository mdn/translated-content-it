---
title: Corso intensivo sulla linea di comando
short-title: Linea di comando
slug: Learn_web_development/Getting_started/Environment_setup/Command_line
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Getting_started/Environment_setup/Dealing_with_files", "Learn_web_development/Getting_started/Your_first_website", "Learn_web_development/Getting_started/Environment_setup")}}

Nel tuo processo di sviluppo, sarai sicuramente tenuto a eseguire alcuni comandi nel terminale (o sulla "linea di comando" — sono effettivamente la stessa cosa). Questo articolo fornisce un'introduzione al terminale, ai comandi essenziali che devi inserire, a come concatenare i comandi insieme e a come aggiungere i tuoi strumenti per l'interfaccia della linea di comando (CLI).

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con il sistema operativo del tuo computer, il software di base che utilizzerai per costruire un sito web e i file system.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Cos'è la linea di comando e cosa puoi fare con essa.</li>
          <li>Capire come accedere alla linea di comando su diversi sistemi.</li>
          <li>Conoscere le scorciatoie da tastiera di base (ad esempio freccia su per accedere ai comandi precedenti, tab per l'autocompletamento).</li>
          <li>Conoscere i comandi di base (ad esempio <code>cd</code>, <code>ls</code>, <code>mkdir</code>, <code>touch</code>, <code>grep</code>, <code>cat</code>, <code>mv</code>, <code>cp</code>).</li>
          <li>Opzioni/flag dei comandi.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Benvenuto nel terminale

Il terminale è un'interfaccia testuale per eseguire programmi basati su testo. Se stai eseguendo qualsiasi strumento per lo sviluppo web, c'è una quasi certezza che dovrai aprire la linea di comando ed eseguire alcuni comandi per usare gli strumenti scelti (spesso vedrai tali strumenti indicati come strumenti **CLI** — strumenti dell'interfaccia della linea di comando).

Un gran numero di strumenti può essere utilizzato digitando comandi nella linea di comando; molti sono preinstallati sul tuo sistema e un'enorme quantità di altri può essere installata dai registri dei pacchetti.
I registri dei pacchetti sono come app store, ma (principalmente) per strumenti e software basati sulla linea di comando.
Vedremo come installare alcuni strumenti più avanti in questo capitolo, e impareremo di più sui registri dei pacchetti nel prossimo capitolo.

Una delle maggiori critiche alla linea di comando è che manca enormemente di esperienza utente.
Visualizzando la linea di comando per la prima volta può essere un'esperienza scoraggiante: uno schermo vuoto e un cursore lampeggiante, con pochissimo aiuto evidente su cosa fare.

In superficie, non sono affatto accoglienti, ma c'è molto che puoi fare con loro, e promettiamo che, con un po' di guida e pratica, usarle diventerà più facile!
Questo è il motivo per cui forniamo questo capitolo - per aiutarti a iniziare in questo ambiente apparentemente ostile.

### Da dove viene il terminale?

Il terminale ha origine intorno agli anni 1950-60 e la sua forma originale non assomiglia davvero a ciò che usiamo oggi (di questo dovremmo essere grati). Puoi leggere un po' della storia nell'entrata di Wikipedia per [Terminale del computer](https://en.wikipedia.org/wiki/Computer_terminal).

Da allora, il terminale è rimasto una caratteristica costante di tutti i sistemi operativi — dai computer desktop ai server nascosti nel cloud, ai microcomputer come il Raspberry PI Zero e persino ai telefoni cellulari. Offre l'accesso diretto al file system sottostante del computer e alle funzionalità a basso livello, ed è quindi incredibilmente utile per eseguire rapidamente compiti complessi, se sai cosa stai facendo.

È anche utile per l'automazione — ad esempio, per scrivere un comando che aggiorni i titoli di centinaia di file istantaneamente, diciamo da "ch01-xxxx.png" a "ch02-xxxx.png". Se aggiornassi i nomi dei file utilizzando il tuo finder o l'app GUI explorer, ci metteresti molto tempo.

Comunque, il terminale non scomparirà tanto presto.

### Come appare il terminale?

Di seguito puoi vedere alcune delle diverse versioni di programmi disponibili che possono portarti a un terminale.

Le immagini successive mostrano i prompt dei comandi disponibili in Windows — c'è una buona gamma di opzioni dal programma "cmd" a "powershell" — che possono essere eseguiti dal menu di avvio digitando il nome del programma.

![Una finestra di cmd windows vanilla, e una finestra powershell windows](win-terminals.png)

E di seguito, puoi vedere l'applicazione terminale di macOS.

![Un terminale macOS vanilla di base](mac-terminal.png)

### Come si accede al terminale?

Oggi molti sviluppatori utilizzano strumenti basati su Unix (es. il terminale e gli strumenti a cui puoi accedere). Molti tutorial e strumenti che esistono oggi sul web supportano (e purtroppo presumono) i sistemi basati su Unix, ma non preoccuparti — sono disponibili sulla maggior parte dei sistemi. In questa sezione, esamineremo come ottenere accesso al terminale sul sistema scelto.

#### Linux/Unix

Come accennato sopra, i sistemi Linux/Unix hanno un terminale disponibile di default, elencato tra le tue Applicazioni.

#### macOS

macOS ha un sistema chiamato Darwin che si trova sotto l'interfaccia grafica utente. Darwin è un sistema simile a Unix, che fornisce il terminale e l'accesso agli strumenti a basso livello. macOS Darwin ha principalmente parità con Unix, certamente abbastanza buona da non causare preoccupazioni mentre lavoriamo a questo articolo.

Il terminale è disponibile su macOS in `Applications/Utilities/Terminal`.

#### Windows

Come con alcuni altri strumenti di programmazione, usare il terminale (o linea di comando) su Windows non è stato tradizionalmente semplice o facile come su altri sistemi operativi. Ma le cose stanno migliorando.

Tradizionalmente Windows ha avuto il suo programma simile a un terminale chiamato `cmd` (il "prompt dei comandi") per molto tempo, ma questo non ha parità con i comandi Unix, ed è equivalente al vecchio prompt DOS di Windows.

Esistono programmi migliori per fornire un'esperienza terminale su Windows, come PowerShell ([vedi qui per trovare gli installer](https://github.com/PowerShell/PowerShell)), e Gitbash (che fa parte del toolkit [git per Windows](https://gitforwindows.org/)).

Tuttavia, l'opzione migliore per Windows al giorno d'oggi è il Sottosistema Windows per Linux (WSL) — un livello di compatibilità per l'esecuzione di sistemi operativi Linux direttamente da Windows 10, permettendoti di eseguire un "vero terminale" direttamente su Windows, senza bisogno di una macchina virtuale.

Questo può essere installato direttamente dallo store Windows gratuitamente. Puoi trovare tutta la documentazione di cui hai bisogno nella [Documentazione del Sottosistema Windows per Linux](https://learn.microsoft.com/en-us/windows/wsl/).

![uno screenshot della documentazione del Sottosistema Windows per Linux](wsl.png)

In termini di quale opzione scegliere su Windows, consigliamo vivamente di provare a installare il WSL. Potresti rimanere con il prompt dei comandi predefinito (`cmd`), e molti strumenti funzioneranno bene, ma troverai tutto più semplice se hai una maggiore parità con gli strumenti Unix.

#### Nota a margine: qual è la differenza tra un comando e un terminale?

Generalmente, troverai questi due termini usati in modo intercambiabile. Tecnicamente, un terminale è un software che avvia e si connette a una shell. Una shell è la tua sessione e ambiente di sessione (dove cose come il prompt e le scorciatoie possono essere personalizzate). La linea di comando è la linea letterale in cui inserisci i comandi e il cursore lampeggia.

### Devi usare il terminale?

Sebbene ci sia una grande varietà di strumenti disponibili dalla linea di comando, se stai usando strumenti come [Visual Studio Code](https://code.visualstudio.com/) c'è anche una massa di estensioni che possono essere usate come proxy per usare i comandi del terminale senza dover usare direttamente il terminale. Tuttavia, non troverai un'estensione dell'editor di codice per tutto ciò che vuoi fare — dovrai acquisire un po' di esperienza con il terminale alla fine.

## Comandi di terminale di base integrati

Abbastanza parole — iniziamo a guardare alcuni comandi del terminale! Di fabbrica, ecco solo alcune delle cose che la linea di comando può fare, insieme ai nomi degli strumenti pertinenti in ciascun caso:

- Navigare nel file system del tuo computer insieme alle attività di base come creare, copiare, rinominare e eliminare:

  - Muoversi nella struttura delle directory (cartelle): `cd`
  - Creare directory: `mkdir`
  - Creare file (e modificare i loro metadati): `touch`
  - Copiare file o directory: `cp`
  - Spostare file o directory: `mv`
  - Eliminare file o directory: `rm`

- Scaricare file trovati a URL specifici: `curl`
- Cercare frammenti di testo all'interno di corpi di testo più grandi: `grep`
- Visualizzare il contenuto di un file pagina per pagina: `less`, `cat`
- Manipolare e trasformare flussi di testo (ad esempio cambiando tutte le istanze di `<div>` in un file HTML in `<article>`): `awk`, `tr`, `sed`

> [!NOTE]
> Ci sono un certo numero di buoni tutorial sul web che vanno molto più in profondità nella linea di comando — questa è solo una breve introduzione!

Passiamo avanti e guardiamo a usare alcuni di questi strumenti sulla linea di comando. Prima di andare oltre, apri il tuo programma terminale!

### Navigazione sulla linea di comando

Quando visiti la linea di comando dovrai inevitabilmente navigare verso una determinata directory per "fare qualcosa". Tutti i sistemi operativi (supponendo un'impostazione predefinita) lanceranno il loro programma terminale nella tua directory _Home_, e da lì è probabile che tu voglia spostarti in un altro posto.

> [!NOTE]
> "Directory" è il termine tecnico per ciò che abbiamo chiamato "cartella" nell'articolo precedente. Quando si guarda alla struttura dei file all'interno di un'interfaccia utente (UI), il termine "cartella" ha più senso, poiché le icone utilizzate sembrano vecchi raccoglitori fisici. Tuttavia, si tende a sentire anche il termine "directory" spesso, specialmente quando si parla di manipolare file usando la linea di comando. Ci sono sfumature, ma i due termini fondamentalmente significano la stessa cosa.

Il comando `cd` ti permette di Cambiare Directory. Tecnicamente, cd non è un programma ma un built-in. Questo significa che il tuo sistema operativo lo fornisce di fabbrica, e anche che non puoi eliminarlo accidentalmente — grazie al cielo! Non devi preoccuparti troppo del fatto che un comando sia built-in o meno, ma tieni presente che i built-in appaiono su tutti i sistemi basati su Unix.

Per cambiare directory, digiti `cd` nel tuo terminale, seguito dalla directory in cui vuoi spostarti. Supponendo che la directory sia all'interno della tua home directory, puoi usare `cd Desktop` (vedi gli screenshot sotto).

![risultati del comando cd Desktop eseguito in una varietà di terminali windows - la posizione del terminale si sposta nel desktop](win-terminals-cd.png)

Prova a digitare questo nel terminale del tuo sistema:

```bash
cd Desktop
```

Se vuoi tornare alla directory precedente, puoi usare due punti:

```bash
cd ..
```

> [!NOTE]
> Una scorciatoia di terminale molto utile è usare il tasto <kbd>tab</kbd> per completare automaticamente i nomi che sai essere presenti, piuttosto che dover digitare l'intera cosa. Ad esempio, dopo aver digitato i due comandi sopra, prova a digitare `cd D` e a premere <kbd>tab</kbd> — dovrebbe completare automaticamente il nome della directory `Desktop` per te, a condizione che sia presente nella directory corrente. Tieni a mente questo mentre vai avanti.

Se la directory che vuoi raggiungere è nidificata in profondità, devi conoscere il percorso per arrivarci. Questo di solito diventa più facile man mano che ti familiarizzi con la struttura del tuo file system, ma se non sei sicuro del percorso di solito puoi capirlo con una combinazione del comando `ls` (vedi sotto) e cliccando intorno nella finestra del tuo Explorer/Finder per vedere dove si trova una directory, rispetto a dove sei attualmente.

Ad esempio, se volessi andare in una directory chiamata `src`, situata all'interno di una directory chiamata `project`, situata sul _Desktop_, potresti digitare questi tre comandi per arrivarci dalla tua directory _Home_:

```bash
cd Desktop
cd project
cd src
```

Ma questo è uno spreco di tempo — invece, puoi digitare un solo comando, con i diversi elementi nel percorso separati da slash in avanti, proprio come fai quando specifichi percorsi a immagini o altri asset in CSS, HTML o JavaScript:

```bash
cd Desktop/project/src
```

Nota che includere uno slash iniziale nel tuo percorso rende il percorso assoluto, ad esempio `/Users/tuo-nome-utente/Desktop`. Ommettendo lo slash iniziale come abbiamo fatto sopra, il percorso diventa relativo alla tua directory di lavoro attuale. Questo è esattamente lo stesso che vedresti con gli URL nel tuo browser web. Uno slash iniziale significa "alla radice del sito web", mentre omettere lo slash significa "l'URL è relativo alla mia pagina attuale".

> [!NOTE]
> Su Windows, usi i backslash invece degli slash in avanti, ad es., `cd Desktop\project\src` — questo può sembrare davvero strano, ma se sei interessato al perché, [guarda questo clip su YouTube](https://www.youtube.com/watch?v=5T3IJfBfBmI) con una spiegazione di uno degli ingegneri principali di Microsoft.

### Elencare i contenuti della directory

Un altro comando Unix integrato è `ls` (abbreviazione di lista), che elenca i contenuti della directory in cui ti trovi attualmente. Nota che questo non funzionerà se stai usando il prompt dei comandi predefinito di Windows (`cmd`) — l'equivalente lì è `dir`.

Prova a eseguirlo ora nel tuo terminale:

```bash
ls
```

Questo ti fornisce un elenco dei file e delle directory nella tua directory di lavoro attuale, ma le informazioni sono davvero basilari — ottieni solo il nome di ciascun elemento presente, non se è un file o una directory, o qualsiasi altra cosa. Fortunatamente, una piccola modifica all'uso del comando può fornirti molte più informazioni.

### Introduzione alle opzioni dei comandi

La maggior parte dei comandi del terminale ha opzioni — sono modificatori che aggiungi alla fine di un comando, che lo fanno comportare in modo leggermente diverso. Questi consistono solitamente in uno spazio dopo il nome del comando, seguito da un trattino, seguito da una o più lettere.

Ad esempio, provalo e vedi cosa ottieni:

```bash
ls -l
```

Nel caso di `ls`, l'opzione `-l` (_trattino elle_) ti offre un elenco con un file o directory su ciascuna linea e molte più informazioni mostrate. Le directory possono essere identificate cercando una lettera "d" sul lato sinistro delle linee. Quelli sono quelli in cui possiamo fare `cd`.

Di seguito c'è uno screenshot con un terminale macOS "vanilla" in alto e un terminale personalizzato con alcune icone extra e colori per mantenerlo vivace — entrambi che mostrano i risultati dell'esecuzione di `ls -l`:

![Un terminale macOS vanilla e un terminale macOS personalizzato più colorato, che mostra un elenco di file - il risultato dell'esecuzione del comando ls -l](mac-terminals-ls.png)

> [!NOTE]
> Per scoprire esattamente quali opzioni ha ogni comando disponibili, puoi guardare la sua [man page](https://en.wikipedia.org/wiki/Man_page). Questo si fa digitando il comando `man`, seguito dal nome del comando che vuoi cercare, ad esempio `man ls`. Questo aprirà la man page nel visualizzatore di file di testo predefinito del terminale (ad esempio, [`less`](<https://en.wikipedia.org/wiki/Less_(Unix)>) nel mio terminale), e dovresti essere in grado di scorrere la pagina utilizzando i tasti freccia o meccanismo similare. La man page elenca tutte le opzioni in grande dettaglio, il che può essere un po' intimidatorio all'inizio, ma almeno sai che è lì se ne hai bisogno. Una volta terminata la lettura della man page, devi uscire usando il comando di uscita del tuo visualizzatore di testo ("q" in `less`; potrebbe essere necessario cercare sul web per trovarlo se non è ovvio).

> [!NOTE]
> Per eseguire un comando con più opzioni contemporaneamente, di solito puoi metterle tutte in una singola stringa dopo il carattere trattino, ad esempio `ls -lah`, o `ls -ltrh`. Prova a guardare la man page di `ls` per capire cosa fanno queste opzioni extra!

Ora che abbiamo discusso di due comandi fondamentali, fai un po' di esplorazione nella tua directory e vedi se riesci a navigare da un posto all'altro.

### Creare, copiare, spostare, rimuovere

Ci sono una serie di altri comandi di utilità di base che probabilmente finirai per utilizzare molto mentre lavori con il terminale. Sono abbastanza semplici, quindi non li spiegheremo tutti nel dettaglio come i due precedenti.

Gioca con loro in una directory di test che hai creato da qualche parte in modo da non eliminare accidentalmente qualcosa di importante, usando i comandi di esempio sotto per guida:

- `mkdir` — questo crea una nuova directory all'interno della directory corrente in cui ti trovi, con il nome che fornisci dopo il nome del comando. Ad esempio, `mkdir my-awesome-website` creerà una nuova directory chiamata `my-awesome-website`.
- `rmdir` — rimuove la directory nominata, ma solo se è vuota. Ad esempio, `rmdir my-awesome-website` rimuoverà la directory che abbiamo creato sopra. Se vuoi rimuovere una directory che non è vuota (e rimuovere anche tutto ciò che contiene), allora puoi usare `rm -r` invece (vedi sotto), ma questo è pericoloso. Assicurati che non ci sia nulla che potresti aver bisogno nella directory più tardi, poiché sarà andato per sempre.
- `touch` — crea un nuovo file vuoto, all'interno della directory corrente. Ad esempio, `touch mdn-example.md` crea un nuovo file vuoto chiamato `mdn-example.md`.
- `mv` — sposta un file dalla prima posizione del file specificato alla seconda posizione specificata del file, ad esempio `mv mdn-example.md mdn-example.txt` (le posizioni sono scritte come percorsi di file). Questo comando sposta un file chiamato `mdn-example.md` nella directory corrente a un file chiamato `mdn-example.txt` nella directory corrente. Tecnicamente il file viene spostato, ma da un punto di vista pratico, questo comando sta effettivamente rinominando il file.
- `cp` — simile nell'uso a `mv`, `cp` crea una copia del file nella prima posizione specificata, nella seconda posizione specificata. Ad esempio, `cp mdn-example.txt mdn-example.txt.bak` crea una copia di `mdn-example.txt` chiamata `mdn-example.txt.bak` (puoi ovviamente chiamarla qualcosa di diverso se lo desideri).
- `rm` — rimuove il file specificato. Ad esempio, `rm mdn-example.txt` elimina un singolo file chiamato `mdn-example.txt`. Nota che questa eliminazione è permanente e non può essere annullata tramite il cestino che potresti avere sulla tua interfaccia utente desktop.

> [!NOTE]
> Molti comandi del terminale ti permettono di usare gli asterischi come caratteri "wild card", intendendo "qualsiasi sequenza di caratteri". Questo ti permette di eseguire un'operazione contro un potenzialmente ampio numero di file in una sola volta, tutti quelli che corrispondono al modello specificato. Come esempio, `rm mdn-*` eliminerebbe tutti i file che iniziano con `mdn-`. `rm mdn-*.bak` eliminerebbe tutti i file che iniziano con `mdn-` e finiscono con `.bak`.

## Terminale — considerato dannoso?

Abbiamo accennato questo prima, ma per essere chiari — devi essere prudente con il terminale. I comandi semplici non comportano troppi pericoli, ma man mano che inizi a mettere insieme comandi più complessi, devi pensare attentamente a cosa farà il comando e provare a testarli prima di eseguirli finalmente nella directory desiderata.

Diciamo che avevi 1000 file di testo in una directory e volevi esaminarli tutti ed eliminare solo quelli che hanno una certa sottostringa nel nome del file. Se non stai attento, potresti finire per eliminare qualcosa di importante, perdendoti un sacco del tuo lavoro nel processo. Una buona abitudine da adottare è di scrivere il tuo comando del terminale in un editor di testo, capire come pensi dovrebbe apparire, e poi fare una copia di backup della tua directory e provare a eseguire il comando su quella prima, per testarlo.

Un altro buon suggerimento — se non ti senti a tuo agio a provare i comandi del terminale sul tuo computer, un posto sicuro per provarli è su [Glitch.com](https://glitch.com/). Oltre a essere un ottimo posto per provare il codice di sviluppo web, i progetti offrono anche accesso a un terminale, quindi puoi eseguire tutti questi comandi direttamente in quel terminale, sapendo che non romperai il tuo computer.

![uno screenshot doppio che mostra la home page di glitch.com e l'emulatore terminale di glitch](glitch.png)

Una grande risorsa per ottenere una panoramica rapida di specifici comandi del terminale è [tldr.sh](https://tldr.sh/). Questo è un servizio di documentazione guidato dalla comunità, simile a MDN, ma specifico per i comandi del terminale.

Nella prossima sezione aumentiamo un po' il livello (o parecchi livelli in effetti) e vediamo come possiamo collegare strumenti insieme sulla linea di comando per vedere davvero come il terminale può essere vantaggioso rispetto alla normale interfaccia utente desktop.

## Collegare i comandi insieme con pipe

Il terminale esprime tutto il suo potenziale quando inizi a concatenare i comandi utilizzando il simbolo `|` (pipe). Vediamo un rapido esempio di cosa significa.

Abbiamo già guardato a `ls`, che stampa i contenuti della directory corrente:

```bash
ls
```

Ma e se volessimo contare rapidamente il numero di file e directory all'interno della directory corrente? `ls` non può farlo da solo.

C'è un altro strumento Unix disponibile chiamato `wc`. Questo conta il numero di parole, righe, caratteri o byte di qualunque cosa sia immessa dentro di esso. Questo può essere un file di testo — l'esempio sotto outputta il numero di righe in `myfile.txt`:

```bash
wc -l myfile.txt
```

Ma può anche contare il numero di righe di qualunque output sia **pipeato** dentro di esso. Ad esempio, il comando sotto conta il numero di righe stampate dal comando `ls` (cosa che stamperebbe normalmente nel terminale se eseguito da solo) e outputta quel conteggio nel terminale invece:

```bash
ls | wc -l
```

Poiché `ls` stampa ciascun file o directory su una propria linea, questo ci dà effettivamente un conteggio di directory e file.

Quindi cosa sta succedendo qui? Una filosofia generale degli strumenti della linea di comando (Unix) è che stampano testo nel terminale (chiamato anche "stampa sull'uscita standard" o `STDOUT`). Molti comandi possono anche leggere contenuto da input in streaming (conosciuto come "input standard" o `STDIN`).

L'operatore pipe può _collegare_ questi input e output insieme, permettendoci di costruire operazioni sempre più complesse per soddisfare le nostre esigenze — l'output da un comando può diventare l'input per il comando successivo. In questo caso, `ls` normalmente stamperebbe il suo output a `STDOUT`, ma invece l'output di `ls` viene pipeato in `wc`, che prende quell'output come input, contando il numero di righe che contiene, e stampa quel conteggio su `STDOUT` invece.

## Un esempio leggermente più complesso

Passiamo a qualcosa di un po' più complicato.

Proveremo prima a recuperare il contenuto della pagina "fetch" di MDN usando il comando `curl` (che può essere usato per richiedere contenuto dagli URL), da `https://developer.mozilla.org/it/docs/Web/API/WindowOrWorkerGlobalScope/fetch`.
Prova ora:

```bash
curl https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/fetch
```

Non otterrai un output perché la pagina è stata reindirizzata (a [/Web/API/fetch](/it/docs/Web/API/Window/fetch)).
Dobbiamo esplicitamente dire a `curl` di seguire i reindirizzamenti usando il flag `-L`.

Guardiamo anche gli header che `developer.mozilla.org` ritorna usando il flag `-I` di `curl`, e stampiamo tutti i reindirizzamenti di posizione che invia al terminale, pipeando l'output di `curl` in `grep` (chiederemo a `grep` di restituire tutte le righe che contengono la parola "location").

Prova a eseguire il seguente comando (vedrai che c'è solo un reindirizzamento prima di raggiungere la pagina finale):

```bash
curl https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/fetch -L -I | grep location
```

Il tuo output dovrebbe apparire in questo modo (`curl` prima stamperà alcuni contatori di download e cose simili):

```bash
location: /en-US/docs/Web/API/Window/fetch
```

Anche se artificioso, potremmo prendere questo risultato un po' più avanti e trasformare i contenuti della riga `location:`, aggiungendo l'origine base all'inizio di ognuno in modo da ottenere URL completi stampati.
Per farlo, aggiungeremo `awk` al mix (che è un linguaggio di programmazione simile a JavaScript o Ruby o Python, solo molto più vecchio!).

Prova a eseguire questo:

```bash
curl https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/fetch -L -I | grep location | awk '{ print "https://developer.mozilla.org" $2 }'
```

Il tuo output finale dovrebbe apparire in questo modo:

```bash
https://developer.mozilla.org/en-US/docs/Web/API/Window/fetch
```

Combinando questi comandi abbiamo personalizzato l'output per mostrare gli URL completi attraverso cui il server Mozilla ci sta reindirizzando quando richiediamo l'URL `/docs/Web/API/WindowOrWorkerGlobalScope/fetch`.
Conoscere il tuo sistema si dimostrerà utile negli anni a venire — impara come funzionano questi strumenti e come possono diventare parte del tuo toolkit per risolvere problemi di nicchia.

## Aggiunta di potenziamenti

Ora che abbiamo dato un'occhiata ad alcuni dei comandi integrati di cui il tuo sistema è dotato, vediamo come possiamo installare uno strumento CLI di terze parti e utilizzarlo.

L'enorme ecosistema di strumenti installabili per lo sviluppo web frontend è attualmente presente per lo più su [npm](https://www.npmjs.com/), un servizio di hosting di pacchetti di proprietà privata che funziona a stretto contatto con Node.js.
Questo sta lentamente crescendo — puoi aspettarti di vedere più fornitori di pacchetti man mano che il tempo passa.

[Installare Node.js](https://nodejs.org/en/) installa anche lo strumento della linea di comando npm (e un tool supplementare centrato su npm chiamato npx), che offre un gateway per installare strumenti aggiuntivi della linea di comando. Node.js e npm funzionano allo stesso modo su tutti i sistemi: macOS, Windows e Linux.

Installa npm sul tuo sistema ora, andando all'URL sopra e scaricando ed eseguendo un installatore di Node.js adatto al tuo sistema operativo. Se richiesto, assicurati di includere npm come parte dell'installazione.

![l'installatore di Node.js su Windows, che mostra l'opzione di includere npm](npm-install-option.png)

Ancora una volta useremo [Prettier](https://prettier.io/) come esempio qui. Abbiamo mostrato come installarlo come estensione di VS Code nel nostro articolo sui [editori di codice](/it/docs/Learn_web_development/Getting_started/Environment_setup/Code_editors#enhancing_your_code_editor_with_extensions). Qui ti mostreremo come installarlo come strumento della linea di comando.

> [!NOTE]
> Prettier è un formattatore di codice opinato che ha solo "poche opzioni". Meno opzioni tende a significare più semplice. Dato che gli strumenti possono talvolta diventare fuori controllo in termini di complessità, "poche opzioni" può essere molto attraente.

### Dove installare i nostri strumenti CLI?

Prima di immergerci nell'installazione di Prettier, c'è una domanda a cui rispondere — "dove dovremmo installarlo?"

Con `npm` abbiamo la scelta di installare strumenti globalmente — così possiamo accedervi ovunque — o localmente nella directory del progetto corrente.

Ci sono pro e contro in entrambi i modi — e le seguenti liste di pro e contro per l'installazione globale sono lontane dall'essere esaustive.

**Pro dell'installazione globale:**

- Accessibile ovunque nel tuo terminale
- Installare una sola volta
- Usa meno spazio su disco
- Sempre la stessa versione
- Si sente come qualsiasi altro comando unix

**Contro dell'installazione globale:**

- Potrebbe non essere compatibile con il codice del tuo progetto
- Altri sviluppatori nel tuo team non avranno accesso a questi strumenti, ad esempio se stai condividendo il codice con uno strumento come git.
- Collegato al punto precedente, rende il codice del progetto più difficile da replicare (se installi i tuoi strumenti localmente, possono essere impostati come dipendenze e installati con <code>npm install</code>).

Anche se la lista dei _contro_ è più breve, l'impatto negativo dell'installazione globale è potenzialmente molto più grande dei benefici.
Qui installeremo localmente, ma sentiti libero di installare globalmente una volta che comprendi i rischi relativi.

### Installazione di Prettier

Prettier è uno strumento di formattazione del codice opinato per gli sviluppatori frontend, concentrandosi sui linguaggi basati su JavaScript e aggiungendo supporto per HTML, CSS, SCSS, JSON e altro.

Prettier può:

- Salvarti dal sovraccarico cognitivo di ottenere lo stile coerente manualmente in tutti i tuoi file di codice; Prettier può farlo automaticamente per te.
- Aiutare i nuovi arrivati allo sviluppo web a formattare il loro codice in modo conforme alle migliori pratiche.
- Essere installato su qualsiasi sistema operativo e persino come parte diretta degli strumenti del progetto, assicurando che colleghi e amici che lavorano sul tuo codice utilizzino lo stile di codice che usi tu.
- Essere configurato per funzionare al salvataggio, mentre digiti, o anche prima di pubblicare il tuo codice (con altri strumenti che vedremo più avanti nel modulo).

Per questo articolo, installeremo Prettier localmente, come suggerito nella [guida all'installazione di Prettier](https://prettier.io/docs/install.html).

Una volta installato node, apri il terminale ed esegui il seguente comando per installare Prettier (spiegheremo cosa fa `--save-dev` nel prossimo articolo):

```bash
npm install --save-dev prettier
```

Ora puoi eseguire il file in locale usando lo strumento [npx](https://docs.npmjs.com/cli/commands/npx/).
Esegui il comando senza argomenti, come molti altri comandi, offrirà informazioni sull'uso e aiuto.
Prova ora:

```bash
npx prettier
```

Il tuo output dovrebbe apparire simile a questo:

```bash
Usage: prettier [options] [file/glob ...]

By default, output is written to stdout.
Stdin is read if it is piped to Prettier and no files are given.

…
```

Vale sempre la pena almeno sfogliare le informazioni sull'uso, anche se sono lunghe.
Ti aiuterà a comprendere meglio come si intende utilizzare lo strumento.

> [!NOTE]
> Se non hai installato Prettier localmente per primo, eseguendo `npx prettier` scaricherai ed eseguirai l'ultima versione di Prettier tutto in un colpo solo _solo per quel comando_.
> Anche se questo potrebbe sembrare fantastico, le nuove versioni di Prettier potrebbero modificare leggermente l'output.
> Vuoi installarlo localmente in modo da fissare la versione di Prettier che stai utilizzando per la formattazione fino a quando non sei pronto a cambiarla.

### Giocare con Prettier

Facciamo un rapido gioco con Prettier, in modo che tu possa vedere come funziona.

Innanzitutto, crea una nuova directory da qualche parte nel tuo file system che sia facile da trovare. Magari una directory chiamata `prettier-test` sul tuo `Desktop`.

Ora salva il seguente codice in un nuovo file chiamato `index.js`, all'interno della tua directory di test:

```js-nolint
const myObj = {
a:1,b:{c:2}}
function printMe(obj){console.log(obj.b.c)}
printMe(myObj)
```

Possiamo eseguire Prettier su una base di codice per controllare se il nostro codice deve essere regolato. `cd` nella tua directory, e prova a eseguire questo comando:

```bash
npx prettier --check index.js
```

Dovresti ottenere un output del tipo:

```bash
Checking formatting...
index.js
Code style issues found in the above file(s). Forgot to run Prettier?
```

Quindi, ci sono alcuni stili di codice che possono essere corretti. Nessun problema. Aggiungendo l'opzione `--write` al comando `prettier` li sistemerà, lasciandoci concentrati sul scrivere effettivamente del codice utile.

Ora prova a eseguire questa versione del comando:

```bash
npx prettier --write index.js
```

Otterrai un output come questo

```bash
Checking formatting...
index.js
Code style issues fixed in the above file(s).
```

Ma cosa più importante, se guardi indietro al tuo file JavaScript, scoprirai che è stato riformattato in qualcosa come questo:

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

A seconda del tuo flusso di lavoro (o del flusso di lavoro che scegli), puoi renderlo una parte automatizzata del tuo processo. L'automazione è davvero dove gli strumenti eccellono; la nostra preferenza personale è il tipo di automazione che "accade e basta" senza dover configurare nulla.

Con Prettier ci sono un certo numero di modi in cui l'automazione può essere ottenuta e anche se sono oltre lo scopo di questo articolo, ci sono alcune risorse eccellenti online per aiutare (alcune delle quali sono state collegate). Puoi richiamare Prettier:

- Prima di compromettere il tuo codice in un repository git usando [Husky](https://github.com/typicode/husky).
- Ogni volta che premi "salva" nel tuo editor di codice, sia esso [VS Code](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode), o [Sublime Text](https://packagecontrol.io/packages/JsPrettier).
- Come parte dei controlli di integrazione continua usando strumenti come [GitHub Actions](https://github.com/features/actions).

La nostra preferenza personale è la seconda — mentre si utilizza ad esempio VS Code, Prettier si attiva e pulisce qualsiasi formattazione debba fare ogni volta che premiamo salva. Puoi trovare molte più informazioni sull'uso di Prettier in modi diversi nei [documenti di Prettier](https://prettier.io/docs/).

## Altri strumenti con cui giocare

Se vuoi giocare con alcuni altri strumenti, ecco una breve lista che è divertente da provare:

- [`bat`](https://github.com/sharkdp/bat) — Un `cat` "più bello" (`cat` è usato per stampare il contenuto dei file).
- [`prettyping`](https://denilson.sa.nom.br/prettyping/) — `ping` sulla linea di comando, ma visualizzato (`ping` è uno strumento utile per verificare se un server risponde).
- [`htop`](https://htop.dev/) — Un visualizzatore di processi, utile quando qualcosa sta facendo comportare il tuo ventilatore della CPU come un motore a reazione e vuoi identificare il programma colpevole.
- [`tldr`](https://tldr.sh/#installation) — menzionato in precedenza in questo capitolo, ma disponibile come strumento della linea di comando.

Nota che alcune delle suggestioni sopra potrebbero richiedere di essere installate utilizzando npm, come abbiamo fatto con Prettier.

## Riepilogo

Questo ci porta alla fine del nostro tour introduttivo del terminale/linea di comando e al modulo di configurazione dell'ambiente. Successivamente, ti faremo lavorare sulla costruzione del tuo primo semplice sito web, così puoi farti un'idea di com'è lo sviluppo web.

{{PreviousMenuNext("Learn_web_development/Getting_started/Environment_setup/Dealing_with_files", "Learn_web_development/Getting_started/Your_first_website", "Learn_web_development/Getting_started/Environment_setup")}}
