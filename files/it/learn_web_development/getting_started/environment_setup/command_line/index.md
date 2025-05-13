---
title: Corso intensivo sulla riga di comando
short-title: Riga di comando
slug: Learn_web_development/Getting_started/Environment_setup/Command_line
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Getting_started/Environment_setup/Dealing_with_files", "Learn_web_development/Getting_started/Your_first_website", "Learn_web_development/Getting_started/Environment_setup")}}

Nel suo processo di sviluppo, sarà inevitabilmente richiesto di eseguire alcuni comandi nel terminale (o sulla "riga di comando" — sono effettivamente la stessa cosa). Questo articolo fornisce un'introduzione al terminale, ai comandi essenziali che dovrà inserire, come concatenare comandi insieme e come aggiungere i propri strumenti di interfaccia a riga di comando (CLI).

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con il sistema operativo del suo computer, il software base che userà per costruire un sito web e i file system.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Cos'è la riga di comando e cosa si può fare con essa.</li>
          <li>Capire come accedere alla riga di comando su diversi sistemi.</li>
          <li>Conoscere le scorciatoie da tastiera di base (per esempio la freccia verso l'alto per accedere ai comandi precedenti, il tab per l'autocompletamento).</li>
          <li>Conoscere i comandi di base (per esempio <code>cd</code>, <code>ls</code>, <code>mkdir</code>, <code>touch</code>, <code>grep</code>, <code>cat</code>, <code>mv</code>, <code>cp</code>).</li>
          <li>Opzioni/flag dei comandi.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Benvenuti al terminale

Il terminale è un'interfaccia testuale per eseguire programmi basati su testo. Se sta eseguendo strumenti per lo sviluppo web, c'è una possibilità quasi garantita che dovrà aprire la riga di comando ed eseguire alcuni comandi per usare gli strumenti scelti (spesso vedrà tali strumenti definiti come **strumenti CLI** — strumenti di interfaccia a riga di comando).

Un gran numero di strumenti può essere usato digitando comandi nella riga di comando; molti sono preinstallati sul suo sistema e un enorme numero di altri sono installabili dai registri dei pacchetti. I registri dei pacchetti sono come app store, ma (principalmente) per strumenti e software basati su riga di comando. Vedremo come installare alcuni strumenti più avanti in questo capitolo, e impareremo di più sui registri dei pacchetti nel prossimo capitolo.

Una delle critiche più grandi alla riga di comando è che manca notevolmente in termini di esperienza utente. Vedere la riga di comando per la prima volta può essere un'esperienza intimidatoria: uno schermo vuoto e un cursore lampeggiante, con pochissimo aiuto evidente su cosa fare.

In superficie, possono sembrare poco accoglienti, ma c'è molto che si può fare con loro, e le promettiamo che, con un po' di guida e pratica, usarli diventerà più facile! È per questo che stiamo fornendo questo capitolo — per aiutarla ad iniziare in questo ambiente apparentemente ostile.

### Da dove viene il terminale?

Il terminale ha origine negli anni 1950-60 e la sua forma originale davvero non somiglia a ciò che usiamo oggi (per cui dovremmo esserne grati). Può leggere un po' della storia sulla voce di Wikipedia relativa al [Terminale del computer](https://en.wikipedia.org/wiki/Computer_terminal).

Da allora, il terminale è rimasto una caratteristica costante di tutti i sistemi operativi — dai desktop ai server nascosti nel cloud, ai microcomputer come il Raspberry PI Zero, e persino ai telefoni cellulari. Fornisce l'accesso diretto al file system sottostante del computer e alle funzionalità di basso livello, ed è quindi incredibilmente utile per svolgere compiti complessi rapidamente, se sa cosa sta facendo.

È anche utile per l'automazione — per esempio, per scrivere un comando per aggiornare i titoli di centinaia di file istantaneamente, diciamo da "ch01-xxxx.png" a "ch02-xxxx.png". Se avesse aggiornato i nomi dei file utilizzando il suo finder o l'app GUI explorer, ci vorrebbe molto tempo.

Comunque, il terminale non andrà via tanto presto.

### Come appare il terminale?

Di seguito può vedere alcune delle diverse varianti di programmi disponibili per accedere a un terminale.

Le immagini seguenti mostrano i prompt dei comandi disponibili in Windows — c'è una buona gamma di opzioni dal programma "cmd" a "powershell" — che possono essere eseguite dal menu start digitando il nome del programma.

![Una finestra della riga di comando di Windows vaniglia e una finestra di PowerShell di Windows](win-terminals.png)

E di seguito, può vedere l'applicazione terminale su macOS.

![Un terminale di base vaniglia macOS](mac-terminal.png)

### Come si accede al terminale?

Oggi molti sviluppatori utilizzano strumenti basati su Unix (es. il terminale, e gli strumenti a cui può accedere tramite esso). Molti tutorial e strumenti che esistono oggi sul web supportano (e purtroppo assumono) sistemi basati su Unix, ma non si preoccupi — sono disponibili sulla maggior parte dei sistemi. In questa sezione, vedremo come ottenere accesso al terminale sul suo sistema scelto.

#### Linux/Unix

Come accennato sopra, i sistemi Linux/Unix hanno un terminale disponibile per impostazione predefinita, elencato tra le sue applicazioni.

#### macOS

macOS ha un sistema chiamato Darwin che si trova sotto l'interfaccia grafica utente. Darwin è un sistema simile a Unix, che fornisce il terminale e l'accesso agli strumenti di basso livello. macOS Darwin ha principalmente parità con Unix, abbastanza buona da non causarci preoccupazioni mentre lavoriamo attraverso questo articolo.

Il terminale è disponibile su macOS in `Applications/Utilities/Terminal`.

#### Windows

Come con altri strumenti di programmazione, utilizzare il terminale (o riga di comando) su Windows tradizionalmente non è stato così semplice o facile come sugli altri sistemi operativi. Ma le cose stanno migliorando.

Tradizionalmente Windows ha avuto un proprio programma simile a un terminale chiamato `cmd` ("il prompt dei comandi") per molto tempo, ma questo non ha parità con i comandi Unix, ed è equivalente al vecchio prompt DOS stile Windows.

Esistono programmi migliori per fornire un'esperienza terminale su Windows, come PowerShell ([vedere qui per trovare installer](https://github.com/PowerShell/PowerShell)), e Gitbash (che viene fornito come parte del [git per Windows](https://gitforwindows.org/) strumento).

Tuttavia, la migliore opzione per Windows ai giorni nostri è il Sottosistema Windows per Linux (WSL) — un livello di compatibilità per eseguire sistemi operativi Linux direttamente dall'interno di Windows 10, consentendole di eseguire un "vero terminale" direttamente su Windows, senza bisogno di una macchina virtuale.

Questo può essere installato direttamente dallo store di Windows gratuitamente. Può trovare tutta la documentazione di cui ha bisogno nella [Documentazione del Sottosistema Windows per Linux](https://learn.microsoft.com/en-us/windows/wsl/).

![uno screenshot della documentazione del Sottosistema Windows per Linux](wsl.png)

In termini di quale opzione scegliere su Windows, le consigliamo vivamente di provare a installare WSL. Potrebbe attenersi al prompt dei comandi predefinito (`cmd`), e molti strumenti funzioneranno bene, ma troverà tutto più facile se avrà una migliore parità con gli strumenti Unix.

#### Nota a margine: qual è la differenza tra una riga di comando e un terminale?

Generalmente, troverà questi due termini usati in modo intercambiabile. Tecnicamente, un terminale è un software che avvia e si connette a una shell. Una shell è la sua sessione e ambiente di sessione (dove cose come il prompt e le scorciatoie potrebbero essere personalizzate). La riga di comando è la riga letterale dove inserisce i comandi e il cursore lampeggia.

### Deve usare il terminale?

Anche se c'è una grande ricchezza di strumenti disponibili dalla riga di comando, se sta utilizzando strumenti come [Visual Studio Code](https://code.visualstudio.com/) c'è anche una massa di estensioni che possono essere usate come proxy per usare i comandi terminali senza dover usare direttamente il terminale. Tuttavia, non troverà un'estensione del codice editor per tutto ciò che vuole fare — dovrà acquisire un'esperienza con il terminale eventualmente.

## Comandi di base integrati nel terminale

Abbiamo parlato abbastanza — iniziamo a esaminare alcuni comandi di terminale! Fuori dagli schemi, ecco alcune delle cose che la riga di comando può fare, insieme ai nomi degli strumenti rilevanti in ciascun caso:

- Navigare nel file system del suo computer insieme a compiti di base come creare, copiare, rinominare e cancellare:

  - Muoversi nella struttura delle sue directory (cartelle): `cd`
  - Creare directory: `mkdir`
  - Creare file (e modificare i loro metadati): `touch`
  - Copiare file o directory: `cp`
  - Spostare file o directory: `mv`
  - Cancellare file o directory: `rm`

- Scaricare file trovati a URL specifici: `curl`
- Cercare frammenti di testo dentro corpi di testo più grandi: `grep`
- Visualizzare il contenuto di un file pagina per pagina: `less`, `cat`
- Manipolare e trasformare flussi di testo (per esempio cambiando tutte le istanze di `<div>` in un file HTML a `<article>`): `awk`, `tr`, `sed`

> [!NOTE]
> Ci sono un certo numero di buoni tutorial sul web che approfondiscono molto di più la riga di comando — questo è solo un'introduzione breve!

Passiamo avanti e guardiamo a utilizzare alcuni di questi strumenti sulla riga di comando. Prima di andare oltre, apra il suo programma terminale!

### Navigazione sulla riga di comando

Quando visita la riga di comando inevitabilmente avrà bisogno di navigare in una particolare directory per "fare qualcosa". Tutti i sistemi operativi (supponendo una configurazione predefinita) avvieranno il loro programma terminale nella sua directory _Home_, e da lì probabilmente vorrà spostarsi in un altro posto.

> [!NOTE]
> "Directory" è il termine tecnico per ciò che abbiamo chiamato "cartella" nell'articolo precedente. Quando si guarda alla struttura dei file all'interno di un'interfaccia utente (UI), il termine "cartella" ha più senso, poiché le icone usate sembrano vecchie cartelle di archiviazione fisica scolastica. Tuttavia, tende ad ascoltare il termine "directory" usato frequentemente anche, specialmente quando si parla di manipolare file usando la riga di comando. Ci sono sfumature, ma i due termini fondamentalmente significano la stessa cosa.

Il comando `cd` le consente di Cambiare Directory. Tecnicamente, cd non è un programma ma un built-in. Questo significa che il suo sistema operativo lo fornisce fin dall'inizio, e anche che non può eliminarlo accidentalmente — per fortuna! Non deve preoccuparsi troppo se un comando è integrato o meno, ma tenga presente che i built-in appaiono su tutti i sistemi basati su Unix.

Per cambiare la directory, digiti `cd` nel suo terminale, seguito dalla directory a cui vuole spostarsi. Supponendo che la directory sia all'interno della sua directory home, può usare `cd Desktop` (vedere gli screenshot di seguito).

![risultati del comando cd Desktop eseguito in una varietà di terminali Windows - la posizione del terminale si sposta nel desktop](win-terminals-cd.png)

Provi a digitare questo nel terminale del suo sistema:

```bash
cd Desktop
```

Se vuole risalire alla directory precedente, può usare due punti:

```bash
cd ..
```

> [!NOTE]
> Una scorciatoia da tastiera del terminale molto utile è usare il tasto <kbd>tab</kbd> per completare automaticamente nomi che sa essere presenti, piuttosto che dover digitare l'intero elemento. Per esempio, dopo aver digitato i due comandi sopra, provi a digitare `cd D` e premere <kbd>tab</kbd> — dovrebbe completare automaticamente il nome della directory `Desktop` per lei, a condizione che sia presente nella directory corrente. Tenga questo presente mentre avanza.

Se la directory a cui vuole accedere è nidificata in profondità, deve conoscere il percorso per raggiungerla. Questo di solito diventa più facile man mano che si familiarizza con la struttura del suo file system, ma se non è sicuro del percorso può di solito capirlo con una combinazione del comando `ls` (vedere sotto), e cliccando in giro nella sua finestra Explorer/Finder per vedere dove si trova una directory, rispetto a dove si trova attualmente.

Per esempio, se volesse andare in una directory chiamata `src`, situata all'interno di una directory chiamata `project`, situata sul _Desktop_, potrebbe digitare questi tre comandi per arrivarci dalla sua directory _Home_:

```bash
cd Desktop
cd project
cd src
```

Ma questo è una perdita di tempo — invece, può digitare un solo comando, con gli elementi diversi nel percorso separati da barre, proprio come fa quando specifica percorsi a immagini o altri asset in CSS, HTML o JavaScript codice:

```bash
cd Desktop/project/src
```

Si noti che includere una barra iniziale nel suo percorso rende il percorso assoluto, ad esempio `/Users/il-suo-nome-utente/Desktop`. Omettere la barra iniziale come abbiamo fatto sopra rende il percorso relativo alla sua directory di lavoro attuale. Questo è esattamente lo stesso che vedrebbe con gli URL nel suo browser web. Una barra iniziale significa "alla radice del sito web", mentre omettere la barra significa "l'URL è relativo al mio attuale pagina".

> [!NOTE]
> Su Windows, si usano le barre rovesciate anziché le barre, ad esempio, `cd Desktop\project\src` — questo può sembrare veramente strano, ma se è interessato al perché, [guardi questo video su YouTube](https://www.youtube.com/watch?v=5T3IJfBfBmI) con una spiegazione di uno dei Principal engineer di Microsoft.

### Elencare il contenuto della directory

Un altro comando Unix integrato è `ls` (abbreviazione di list), che elenca i contenuti della directory in cui si trova attualmente. Si noti che questo non funzionerà se si utilizza il prompt dei comandi predefinito di Windows (`cmd`) — l'equivalente lì è `dir`.

Provi a eseguire questo ora nel suo terminale:

```bash
ls
```

Questo le dà una lista dei file e delle directory nella sua directory di lavoro attuale, ma l'informazione è davvero di base — ottiene solo il nome di ciascun elemento presente, non se è un file o una directory, o qualsiasi altra cosa. Fortunatamente, una piccola modifica all'uso del comando può darle molte più informazioni.

### Introduzione delle opzioni dei comandi

La maggior parte dei comandi terminali ha opzioni — questi sono modificatori che aggiunge alla fine di un comando, che lo fanno comportare in un modo leggermente diverso. Questi di solito consistono in uno spazio dopo il nome del comando, seguito da un trattino, seguito da una o più lettere.

Ad esempio, provi a eseguire questo comando e vedere cosa ottiene:

```bash
ls -l
```

Nel caso di `ls`, l'opzione `-l` (_trattino ell_) le fornisce un elenco con un file o una directory su ciascuna riga, e molte più informazioni mostrato. Le directory possono essere identificate guardando una lettera "d" sul lato molto sinistro delle righe. Quelle sono quelle in cui possiamo entrare con `cd`.

Di seguito è riportato uno screenshot con un terminale "vaniglia" macOS in alto, e un terminale personalizzato con alcune icone extra e colori per mantenerlo vivace — entrambi mostrano i risultati di eseguire `ls -l`:

![Un terminale macOS vaniglia e un terminale macOS personalizzato più colorato, mostrando un elenco di file - il risultato dell'esecuzione del comando ls -l](mac-terminals-ls.png)

> [!NOTE]
> Per scoprire esattamente quali opzioni ha ogni comando disponibili, può guardare sua [pagina del manuale](https://en.wikipedia.org/wiki/Man_page). Questo viene fatto digitando il comando `man`, seguito dal nome del comando che vuole guardare, ad esempio `man ls`. Questo aprirà la pagina del manuale nel visualizzatore del file di testo predefinito del terminale (ad esempio, [`less`](<https://en.wikipedia.org/wiki/Less_(Unix)>) nel mio terminale), e dovrebbe quindi essere in grado di scorrere la pagina usando i tasti freccia, o qualche meccanismo simile. La pagina del manuale elenca tutte le opzioni in dettaglio, che può essere un po' intimidatorio all'inizio, ma almeno sa che è lì se ne ha bisogno. Una volta che ha finito di scorrere la pagina del manuale, deve uscire da essa usando il comando di uscita del suo visualizzatore di testo ("q" in `less`; potrebbe dover cercare sul web per trovarlo se non è ovvio).

> [!NOTE]
> Per eseguire un comando con più opzioni contemporaneamente, può di solito metterle tutte in una stringa singola dopo il carattere trattino, ad esempio `ls -lah`, o `ls -ltrh`. Provi a guardare la pagina del manuale di `ls` per capire cosa fanno queste opzioni extra!

Ora che abbiamo discusso due comandi fondamentali, faccia un giro nella sua directory e veda se riesce a navigare da un posto all'altro.

### Creare, copiare, spostare, rimuovere

Ci sono un certo numero di altri comandi di utilità di base che probabilmente finirà per usare molto mentre lavora con il terminale. Sono abbastanza semplici, quindi non li spiegheremo tutti nei dettagli come i precedenti.

Provi a giocarci in una directory di test che ha creato da qualche parte per non cancellare accidentalmente qualcosa di importante, usando i comandi esemplificativi sotto per guida:

- `mkdir` — questa crea una nuova directory all'interno della directory attuale in cui si trova, con il nome che fornisce dopo il nome del comando. Per esempio, `mkdir my-awesome-website` creerà una nuova directory chiamata `my-awesome-website`.
- `rmdir` — rimuove la directory nominata, ma solo se è vuota. Per esempio, `rmdir my-awesome-website` rimuoverà la directory che abbiamo creato sopra. Se vuole rimuovere una directory che non è vuota (e anche rimuovere tutto ciò che contiene), allora può usare `rm -r` invece (vedere sotto), ma questo è pericoloso. Assicurarsi che non ci sia niente che potrebbe servirle all'interno della directory più tardi, poiché sarà andata per sempre.
- `touch` — crea un nuovo file vuoto, all'interno della directory corrente. Per esempio, `touch mdn-example.md` creerà un nuovo file vuoto chiamato `mdn-example.md`.
- `mv` — sposta un file dalla prima posizione di percorso specificata alla seconda posizione di percorso specificata, per esempio `mv mdn-example.md mdn-example.txt` (le posizioni sono scritte come percorsi file). Questo comando sposta un file chiamato `mdn-example.md` nella directory corrente a un file chiamato `mdn-example.txt` nella directory corrente. Tecnicamente il file viene spostato, ma da un punto di vista pratico, questo comando sta effettivamente rinominando il file.
- `cp` — simile nell'uso a `mv`, `cp` crea una copia del file nella prima posizione specificata, nella seconda posizione specificata. Per esempio, `cp mdn-example.txt mdn-example.txt.bak` crea una copia di `mdn-example.txt` chiamata `mdn-example.txt.bak` (può naturalmente chiamarlo qualcosa di diverso se lo desidera).
- `rm` — rimuove il file specificato. Per esempio, `rm mdn-example.txt` elimina un singolo file chiamato `mdn-example.txt`. Si noti che questa cancellazione è permanente e non può essere annullata tramite il cestino che potrebbe avere sulla sua interfaccia utente desktop.

> [!NOTE]
> Molti comandi terminali le consentono di usare asterischi come caratteri "jolly", che significano "qualsiasi sequenza di caratteri". Questo le consente di eseguire un'operazione contro un numero potenzialmente grande di file contemporaneamente, tutti corrispondenti al pattern specificato. Come esempio, `rm mdn-*` cancellerebbe tutti i file che iniziano con `mdn-`. `rm mdn-*.bak` cancellerebbe tutti i file che iniziano con `mdn-` e finiscono con `.bak`.

## Terminale — considerato dannoso?

Abbiamo accennato a questo prima, ma per essere chiari — deve fare attenzione con il terminale. I comandi semplici non comportano troppi pericoli, ma mentre inizia a mettere insieme comandi più complessi, deve pensare attentamente a cosa farà il comando, e provare a testarli prima di eseguirli finalmente nella directory intenduta.

Diciamo che ha 1000 file di testo in una directory, e vuole passarli tutti e solo cancellare quelli che hanno una certa sottostringa nel nome del file. Se non è attento, allora potrebbe finire per cancellare qualcosa di importante, perdendo un carico del suo lavoro nel processo. Una buona abitudine da acquisire è scrivere il suo comando terminale in un editor di testo, capire come pensa che debba sembrare, e poi fare una copia di backup della sua directory e provare a eseguire il comando su quella prima, per testarlo.

Un altro buon consiglio — se non è a suo agio con il provare comandi terminali sulla sua macchina, un buon posto sicuro per provarli è su [Glitch.com](https://glitch.com/). Insieme a essere un ottimo posto per provare codice di sviluppo web, i progetti forniscono anche accesso a un terminale, in modo che possa eseguire tutti questi comandi direttamente in quel terminale, sapendo con sicurezza che non romperà la sua macchina.

![uno screenshot doppio che mostra la home page di glitch.com e l'emulatore terminale di glitch](glitch.png)

Una grande risorsa per ottenere una panoramica rapida di comandi terminali specifici è [tldr.sh](https://tldr.sh/). Questo è un servizio di documentazione guidato dalla comunità, simile a MDN, ma specifico ai comandi terminali.

Nella prossima sezione facciamo un passo avanti (o diversi passi in effetti) e vediamo come possiamo connettere strumenti insieme sulla riga di comando per vedere davvero come il terminale possa essere vantaggioso rispetto alla regolare interfaccia utente desktop.

## Collegare i comandi insieme con le pipe

Il terminale raggiunge davvero il proprio massimo quando inizia a concatenare comandi insieme utilizzando il simbolo `|` (pipe). Vediamo un esempio molto rapido di cosa ciò significhi.

Abbiamo già esaminato `ls`, che visualizza il contenuto della directory corrente:

```bash
ls
```

Ma cosa succede se volessimo contare rapidamente il numero di file e directory all'interno della directory corrente? `ls` non può farlo da solo.

C'è un altro strumento Unix disponibile chiamato `wc`. Conta il numero di parole, righe, caratteri o byte di qualunque input riceve. Questo può essere un file di testo — l'esempio sotto visualizza il numero di righe in `myfile.txt`:

```bash
wc -l myfile.txt
```

Ma può anche contare il numero di righe di qualunque output viene **pipe** in esso. Per esempio, il comando sotto conta il numero di linee visualizzate dal comando `ls` (quello che normalmente stamperebbe nel terminale se eseguito da solo) e visualizza quel conteggio invece nel terminale:

```bash
ls | wc -l
```

Dato che `ls` stampa ciascun file o directory sulla propria riga, questo ci dà effettivamente un conteggio di directory e file.

Cosa sta succedendo qui? Una filosofia generale degli strumenti della linea di comando (unix) è che stampano testo a terminal (anche indicato come "stampa su standard output" o `STDOUT`). Un buon numero di comandi può anche leggere contenuto da input in streaming (noto come "standard input" o `STDIN`).

L'operatore pipe può _connettere_ questi input e output insieme, consentendoci di costruire operazioni sempre più complesse per soddisfare le nostre esigenze — l'output da un comando può diventare l'input al comando successivo. In questo caso, `ls` normalmente stamperebbe il suo output su `STDOUT`, ma invece l'output di `ls` viene pipeato in `wc`, che prende quell'output come un input, contando il numero di linee che contiene, e stampa quel conteggio a `STDOUT` invece.

## Un esempio leggermente più complesso

Vediamo qualcosa di un po' più complicato.

Prima proveremo a recuperare il contenuto della pagina "fetch" di MDN usando il comando `curl` (che può essere usato per richiedere contenuti da URL), da `https://developer.mozilla.org/it/docs/Web/API/WindowOrWorkerGlobalScope/fetch`. Provi ora:

```bash
curl https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/fetch
```

Non otterrà alcun output perché la pagina è stata reindirizzata (a [/Web/API/fetch](/it/docs/Web/API/Window/fetch)). Dobbiamo dire esplicitamente a `curl` di seguire i reindirizzamenti usando il flag `-L`.

Vediamo anche gli header che `developer.mozilla.org` restituisce usando il flag `-I` di `curl`, e stampiamo tutti i reindirizzamenti di posizione che invia al terminale, inserendo l'output di `curl` in `grep` (chiederemo a `grep` di restituire tutte le righe che contengono la parola "location").

Provi a eseguire quanto segue (vedrà che c'è solo un reindirizzamento prima di raggiungere la pagina finale):

```bash
curl https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/fetch -L -I | grep location
```

Il suo output dovrebbe apparire in questo modo (`curl` prima stamperà alcuni contatori di download e simili):

```bash
location: /en-US/docs/Web/API/Window/fetch
```

Anche se forzato, potremmo portare ulteriormente questo risultato e trasformare i contenuti della riga `location:`, aggiungendo l'origine base all'inizio di ciascuno di modo da ottenere URL completi stampati. Per questo, aggiungeremo `awk` al mix (che è un linguaggio di programmazione simile a JavaScript o Ruby o Python, solo molto più vecchio!).

Provi a eseguire questo:

```bash
curl https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/fetch -L -I | grep location | awk '{ print "https://developer.mozilla.org" $2 }'
```

Il suo output finale dovrebbe apparire in questo modo:

```bash
https://developer.mozilla.org/en-US/docs/Web/API/Window/fetch
```

Combinando questi comandi abbiamo personalizzato l'output per mostrare gli URL completi attraverso cui il server Mozilla si reindirizza quando richiediamo l'URL `/docs/Web/API/WindowOrWorkerGlobalScope/fetch`. Conoscere il suo sistema si rivelerà utile negli anni a venire — impari come questi strumenti a servizio singolo funzionano e come possono diventare parte della sua cassetta degli attrezzi per risolvere problemi di nicchia.

## Aggiungere miglioramenti

Ora che abbiamo dato un'occhiata ad alcuni dei comandi integrati con cui il suo sistema è attrezzato, vediamo come possiamo installare uno strumento CLI di terze parti e farne uso.

L'ampio ecosistema di strumenti installabili per lo sviluppo web front-end esiste attualmente principalmente all'interno di [npm](https://www.npmjs.com/), un servizio di hosting privato di pacchetti che lavora a stretto contatto con Node.js. Questo sta lentamente espandendosi — può aspettarsi di vedere più fornitori di pacchetti man mano che il tempo avanza.

[Installare Node.js](https://nodejs.org/en/) installa anche lo strumento a riga di comando npm (e uno strumento supplementare incentrato su npm chiamato npx), che offre un gateway per installare strumenti di riga di comando aggiuntivi. Node.js e npm funzionano allo stesso modo su tutti i sistemi: macOS, Windows e Linux.

Installare npm sul suo sistema ora, andando all'URL sopra e scaricando ed eseguendo un installer Node.js appropriato al suo sistema operativo. Se richiesto, assicurarsi di includere npm come parte dell'installazione.

![l'installer di Node.js su Windows, mostrando l'opzione per includere npm](npm-install-option.png)

Utilizzeremo di nuovo [Prettier](https://prettier.io/) come esempio qui. Abbiamo mostrato come installarlo come estensione VS Code nel nostro articolo [Editor di codice](/it/docs/Learn_web_development/Getting_started/Environment_setup/Code_editors#enhancing_your_code_editor_with_extensions). Qui mostreremo come installarlo come strumento a riga di comando.

> [!NOTE]
> Prettier è un formattatore di codice di opinione che ha solo "poche opzioni". Meno opzioni tendono a significare più semplice. Dato che gli strumenti a volte possono sfuggire di mano in termini di complessità, "poche opzioni" possono essere molto attraenti.

### Dove installare i nostri strumenti CLI?

Prima di immergerci nell'installazione di Prettier, c'è una questione da rispondere — "dove dovremmo installarlo?"

Con `npm` abbiamo la possibilità di installare strumenti globalmente — così possiamo accedervi ovunque — o localmente nella directory del progetto corrente.

Ci sono pro e contro per ciascuno — e le seguenti liste di pro e contro per l'installazione globale sono tutt'altro che esaustive.

**Pro dell'installazione globale:**

- Accessibile ovunque nel suo terminale
- Installare solo una volta
- Utilizza meno spazio su disco
- Sempre la stessa versione
- Sembra come qualsiasi altro comando unix

**Contro dell'installazione globale:**

- Potrebbe non essere compatibile con il codice del suo progetto
- Altri sviluppatori nel suo team non avranno accesso a questi strumenti, per esempio se condivide il codice del progetto tramite un strumento come git.
- Relativamente al punto precedente, rende il codice del progetto più difficile da replicare (se installa i suoi strumenti localmente, possono essere impostati come dipendenze e installati con <code>npm install</code>).

Anche se la lista dei _contro_ è più corta, l'impatto negativo dell'installazione globale è potenzialmente molto più grande dei benefici. Qui installeremo localmente, ma si senta libero di installare globalmente una volta che comprende i rischi relativi.

### Installazione di Prettier

Prettier è uno strumento di formattazione di codice di opinione per sviluppatori front-end, incentrato sui linguaggi basati su JavaScript e aggiungendo supporto per HTML, CSS, SCSS, JSON, e altro.

Prettier può:

- Salvare il carico cognitivo di ottenere lo stile coerente manualmente su tutti i suoi file di codice; Prettier può farlo automaticamente per lei.
- Aiutare chi è nuovo allo sviluppo web a formattare il loro codice in modo appropriato alle migliori pratiche.
- Essere installato su qualsiasi sistema operativo e anche come parte diretta degli strumenti del progetto, assicurandosi che colleghi e amici che lavorano sul suo codice utilizzino lo stile di codice che sta utilizzando.
- Essere configurato per eseguire alla salva, mentre digita, o anche prima di pubblicare il suo codice (con strumenti aggiuntivi che vedremo più avanti nel modulo).

Per questo articolo, installeremo Prettier localmente, come suggerito nella [guida all'installazione di Prettier](https://prettier.io/docs/install.html).

Una volta installato node, aprire il terminale e eseguire il comando seguente per installare Prettier (spiegheremo cosa fa `--save-dev` nell'articolo successivo):

```bash
npm install --save-dev prettier
```

Ora può eseguire il file localmente utilizzando lo strumento [npx](https://docs.npmjs.com/cli/commands/npx/). Eseguire il comando senza argomenti, come con molti altri comandi, offrirà informazioni sull'uso e sull'aiuto. Provi ora:

```bash
npx prettier
```

Il suo output dovrebbe apparire in questo modo:

```bash
Usage: prettier [options] [file/glob ...]

By default, output is written to stdout.
Stdin is read if it is piped to Prettier and no files are given.

…
```

Vale sempre la pena di almeno sbirciare tra le informazioni sull'uso, anche se sono lunghe. Le aiuterà a capire meglio come lo strumento è inteso per essere usato.

> [!NOTE]
> Se non ha prima installato Prettier localmente, eseguire `npx prettier` scaricherà ed eseguirà l'ultima versione di Prettier tutto in una volta _solo per quel comando_. Anche se questo potrebbe sembrare fantastico, nuove versioni di Prettier potrebbero modificare leggermente l'output. Vuole installarlo localmente in modo che stia fissando la versione di Prettier che sta usando per la formattazione fino a quando non è pronto a cambiarlo.

### Giocare con Prettier

Facciamo una rapida prova con Prettier, così può vedere come funziona.

Innanzitutto, crei una nuova directory da qualche parte sul suo file system che sia facile da trovare. Magari una directory chiamata `prettier-test` sul suo `Desktop`.

Ora salvi il seguente codice in un nuovo file chiamato `index.js`, all'interno della sua directory di test:

```js-nolint
const myObj = {
a:1,b:{c:2}}
function printMe(obj){console.log(obj.b.c)}
printMe(myObj)
```

Possiamo eseguire Prettier contro una base di codice solo per vedere se il nostro codice desidera delle correzioni. `cd` nella sua directory, e provi a eseguire questo comando:

```bash
npx prettier --check index.js
```

Dovrebbe ottenere un output simile a:

```bash
Checking formatting...
index.js
Code style issues found in the above file(s). Forgot to run Prettier?
```

Quindi, ci sono alcuni stili di codice che possono essere corretti. Nessun problema. Aggiungendo l'opzione `--write` al comando `prettier` li correggerà, lasciandoci concentrare su effettivamente scrivere codice utile.

Ora provi a eseguire questa versione del comando:

```bash
npx prettier --write index.js
```

Otterrà un output simile a questo

```bash
Checking formatting...
index.js
Code style issues fixed in the above file(s).
```

Ma più importante, se guarda di nuovo il suo file JavaScript vedrà che è stato riformattato in qualcosa di simile a questo:

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

A seconda del flusso di lavoro (o del flusso di lavoro che sceglie) può rendere questo una parte automatica del suo processo. L'automazione è veramente dove gli strumenti eccellono; la nostra preferenza personale è il tipo di automazione che "accade e basta" senza dover configurare nulla.

Con Prettier ci sono diversi modi in cui l'automazione può essere realizzata e anche se sono al di fuori dell'ambito di questo articolo, ci sono ottime risorse online per aiutare (alcune delle quali sono state collegate). Può invocare Prettier:

- Prima di impegnare il suo codice in un repository git usando [Husky](https://github.com/typicode/husky).
- Ogni volta che preme "salva" nel suo editor di codice, sia esso [VS Code](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode), o [Sublime Text](https://packagecontrol.io/packages/JsPrettier).
- Come parte dei controlli di integrazione continua utilizzando strumenti come [GitHub Actions](https://github.com/features/actions).

La nostra preferenza personale è il secondo — mentre usiamo ad esempio VS Code, Prettier si attiva e pulisce qualsiasi formattazione che necessita di fare ogni volta che premiamo salva. Può trovare molte più informazioni su come usare Prettier in diversi modi nei [documenti di Prettier](https://prettier.io/docs/).

## Altri strumenti con cui giocare

Se vuole giocare con alcuni altri strumenti, ecco una breve lista che sono divertenti da provare:

- [`bat`](https://github.com/sharkdp/bat) — Un `cat` "più carino" (`cat` è utilizzato per stampare i contenuti dei file).
- [`prettyping`](https://denilson.sa.nom.br/prettyping/) — `ping` su riga di comando, ma visualizzato (`ping` è uno strumento utile per verificare se un server sta rispondendo).
- [`htop`](https://htop.dev/) — Un visualizzatore di processi, utile per quando qualcosa sta facendo comportare la sua ventola CPU come un motore a reazione e vuole identificare il programma colpevole.
- [`tldr`](https://tldr.sh/#installation) — menzionato in precedenza in questo capitolo, ma disponibile come strumento a riga di comando.

Si noti che alcuni dei suggerimenti sopra possono aver bisogno di essere installati utilizzando npm, come abbiamo fatto con Prettier.

## Sommario

Questo ci porta alla fine del nostro tour introduttivo del terminale/riga di comando, e del modulo di Configurazione dell'ambiente. Il prossimo passo, la faremo lavorare alla costruzione del suo primo semplice sito web, in modo da farsi un'idea di cosa significhi lo sviluppo web.

{{PreviousMenuNext("Learn_web_development/Getting_started/Environment_setup/Dealing_with_files", "Learn_web_development/Getting_started/Your_first_website", "Learn_web_development/Getting_started/Environment_setup")}}
