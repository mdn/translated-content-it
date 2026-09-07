---
title: Corso intensivo sulla riga di comando
short-title: Riga di comando
slug: Learn_web_development/Getting_started/Environment_setup/Command_line
l10n:
  sourceCommit: 79f65d8322a4e55e9f3f4c91441c9188dbe670e0
---

{{PreviousMenuNext("Learn_web_development/Getting_started/Environment_setup/Dealing_with_files", "Learn_web_development/Getting_started/Your_first_website", "Learn_web_development/Getting_started/Environment_setup")}}

Nel processo di sviluppo sarà senza dubbio necessario eseguire alcuni comandi nel terminale (o sulla "riga di comando" — si tratta in pratica della stessa cosa). Questo articolo fornisce un'introduzione al terminale, ai comandi essenziali da inserire, a come concatenare i comandi e a come aggiungere strumenti di interfaccia a riga di comando (CLI) personalizzati.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con il sistema operativo del computer, il software di base che verrà utilizzato per creare un sito web e i file system.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Che cos'è la riga di comando e cosa consente di fare.</li>
          <li>Comprendere come accedere alla riga di comando su sistemi diversi.</li>
          <li>Conoscere le scorciatoie da tastiera di base (ad esempio la freccia su per accedere ai comandi precedenti, il tasto Tab per il completamento automatico).</li>
          <li>Conoscere i comandi di base (ad esempio <code>cd</code>, <code>ls</code>, <code>mkdir</code>, <code>touch</code>, <code>grep</code>, <code>cat</code>, <code>mv</code>, <code>cp</code>).</li>
          <li>Opzioni/flag dei comandi.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Benvenuti nel terminale

Il terminale è un'interfaccia testuale per eseguire programmi basati sul testo. Se si utilizza qualsiasi strumento per lo sviluppo web, è quasi certo che sarà necessario aprire la riga di comando ed eseguire alcuni comandi per utilizzare gli strumenti scelti (questi strumenti vengono spesso chiamati **strumenti CLI** — strumenti di interfaccia a riga di comando).

Un gran numero di strumenti può essere utilizzato digitando comandi nella riga di comando; molti sono preinstallati nel sistema e un numero enorme di altri può essere installato dai registri di pacchetti.
I registri di pacchetti sono come app store, ma (per lo più) per strumenti e software basati sulla riga di comando.
Più avanti in questo capitolo verrà illustrato come installare alcuni strumenti e nel capitolo successivo verranno approfonditi i registri di pacchetti.

Una delle principali critiche alla riga di comando è la grave mancanza di user experience.
Visualizzare la riga di comando per la prima volta può essere un'esperienza intimidatoria: uno schermo vuoto e un cursore lampeggiante, con pochissimo aiuto evidente su cosa fare.

In superficie, è tutt'altro che accogliente, ma consente di fare molte cose e, con un po' di guida e pratica, il suo utilizzo diventerà più facile!
Questo è il motivo per cui viene fornito questo capitolo: per aiutare a iniziare in questo ambiente apparentemente ostile.

### Da dove proviene il terminale?

Il terminale ha origine intorno agli anni 1950-60 e la sua forma originale non assomiglia affatto a quella usata oggi (e per questo bisogna essere grati). È possibile leggere parte della sua storia nella voce di Wikipedia [Computer Terminal](https://en.wikipedia.org/wiki/Computer_terminal).

Da allora, il terminale è rimasto una funzionalità costante di tutti i sistemi operativi: dalle macchine desktop ai server nascosti nel cloud, ai microcomputer come Raspberry PI Zero e persino ai telefoni cellulari. Fornisce accesso diretto al file system sottostante del computer e alle funzionalità di basso livello, risultando quindi incredibilmente utile per eseguire rapidamente attività complesse, se si sa cosa si sta facendo.

È utile anche per l'automazione, ad esempio per scrivere un comando che aggiorni istantaneamente i titoli di centinaia di file, ad esempio da "ch01-xxxx.png" a "ch02-xxxx.png". Aggiornare i nomi dei file usando l'app GUI finder o explorer richiederebbe molto tempo.

In ogni caso, il terminale non scomparirà presto.

### Che aspetto ha il terminale?

Di seguito sono disponibili alcuni dei diversi tipi di programmi che consentono di accedere a un terminale.

Le immagini successive mostrano i prompt dei comandi disponibili in Windows: è presente una buona gamma di opzioni, dal programma "cmd" a "powershell", che possono essere eseguiti dal menu Start digitando il nome del programma.

![Una finestra della riga di comando cmd standard di Windows e una finestra Windows PowerShell](win-terminals.png)

Di seguito è possibile vedere l'applicazione terminale di macOS.

![Un terminale macOS standard di base](mac-terminal.png)

### Come si accede al terminale?

Oggi molti sviluppatori utilizzano strumenti basati su Unix, ad esempio il terminale e gli strumenti accessibili tramite esso. Molti tutorial e strumenti presenti oggi sul web supportano (e purtroppo presuppongono) sistemi basati su Unix, ma non c'è da preoccuparsi: sono disponibili nella maggior parte dei sistemi. In questa sezione verrà illustrato come accedere al terminale nel sistema scelto.

#### Linux/Unix

Come accennato sopra, i sistemi Linux/Unix dispongono di un terminale disponibile per impostazione predefinita, elencato tra le Applicazioni.

#### macOS

macOS dispone di un sistema chiamato Darwin, che si trova al di sotto dell'interfaccia grafica utente. Darwin è un sistema simile a Unix, che fornisce il terminale e l'accesso agli strumenti di basso livello. Darwin di macOS è per lo più equivalente a Unix, certamente abbastanza da non causare problemi durante lo studio di questo articolo.

Il terminale è disponibile su macOS in `Applications/Utilities/Terminal`.

#### Windows

Come per altri strumenti di programmazione, l'uso del terminale (o della riga di comando) su Windows tradizionalmente non è stato semplice o facile quanto su altri sistemi operativi. Tuttavia, le cose stanno migliorando.

Windows ha tradizionalmente avuto da molto tempo un proprio programma simile a un terminale chiamato `cmd` ("il prompt dei comandi"), ma questo non è equivalente ai comandi Unix ed equivale al prompt DOS di Windows in stile precedente.

Esistono programmi migliori per fornire un'esperienza di terminale su Windows, come PowerShell ([vedere qui per trovare i programmi di installazione](https://github.com/PowerShell/PowerShell)) e Git Bash, incluso nel set di strumenti [git per Windows](https://gitforwindows.org/).

Tuttavia, l'opzione migliore per Windows oggi è Windows Subsystem for Linux (WSL), un livello di compatibilità per eseguire sistemi operativi Linux direttamente all'interno di Windows 10, consentendo di eseguire un "vero terminale" direttamente su Windows senza necessità di una macchina virtuale.

Può essere installato gratuitamente direttamente dal Windows Store. Tutta la documentazione necessaria è disponibile nella [documentazione di Windows Subsystem for Linux](https://learn.microsoft.com/en-us/windows/wsl/).

![uno screenshot della documentazione di Windows Subsystem for Linux](wsl.png)

Per quanto riguarda l'opzione da scegliere su Windows, è fortemente consigliato provare a installare WSL. È possibile continuare a usare il prompt dei comandi predefinito (`cmd`) e molti strumenti funzioneranno correttamente, ma tutto risulterà più semplice con una maggiore equivalenza agli strumenti Unix.

#### Nota a margine: qual è la differenza tra riga di comando e terminale?

In generale, questi due termini vengono usati in modo intercambiabile. Tecnicamente, un terminale è un software che avvia e si connette a una shell. Una shell è la sessione e l'ambiente della sessione, in cui elementi come il prompt e le scorciatoie possono essere personalizzati. La riga di comando è la riga letterale in cui vengono inseriti i comandi e il cursore lampeggia.

### È obbligatorio usare il terminale?

Sebbene dalla riga di comando sia disponibile una grande quantità di strumenti, se si utilizzano strumenti come [Visual Studio Code](https://code.visualstudio.com/) esiste anche un gran numero di estensioni che possono fungere da tramite per utilizzare comandi del terminale senza dover usare direttamente il terminale. Tuttavia, non esiste un'estensione dell'editor di codice per tutto ciò che si desidera fare: prima o poi sarà necessario acquisire esperienza con il terminale.

## Comandi di base integrati nel terminale

Basta parlare: iniziamo a esaminare alcuni comandi del terminale! Pronti all'uso, ecco solo alcune delle operazioni che la riga di comando può eseguire, insieme ai nomi degli strumenti pertinenti in ogni caso:

- Esplorare il file system del computer insieme ad attività di base come creare, copiare, rinominare ed eliminare:
  - Spostarsi nella struttura di directory (cartelle): `cd`
  - Creare directory: `mkdir`
  - Creare file, modificandone anche i metadati: `touch`
  - Copiare file o directory: `cp`
  - Spostare file o directory: `mv`
  - Eliminare file o directory: `rm`

- Scaricare file disponibili a URL specifici: `curl`
- Cercare frammenti di testo all'interno di corpi di testo più grandi: `grep`
- Visualizzare il contenuto di un file pagina per pagina: `less`, `cat`
- Manipolare e trasformare flussi di testo, ad esempio modificando tutte le occorrenze di `<div>` in un file HTML in `<article>`: `awk`, `tr`, `sed`

> [!NOTE]
> Sul web sono disponibili numerosi ottimi tutorial che approfondiscono molto di più la riga di comando: questa è solo una breve introduzione.

Andiamo avanti e vediamo come usare alcuni di questi strumenti dalla riga di comando. Prima di proseguire, aprire il programma terminale.

### Navigazione nella riga di comando

Quando si utilizza la riga di comando, sarà inevitabilmente necessario raggiungere una particolare directory per "fare qualcosa". Tutti i sistemi operativi, assumendo una configurazione predefinita, avviano il programma terminale nella directory _Home_ e da lì probabilmente sarà necessario spostarsi altrove.

> [!NOTE]
> "Directory" è il termine tecnico per ciò che nell'articolo precedente è stato chiamato "cartella". Quando si osserva la struttura dei file in un'interfaccia utente (UI), il termine "cartella" ha più senso, poiché le icone utilizzate assomigliano alle vecchie cartelle fisiche per l'archiviazione. Tuttavia, si sentirà spesso anche il termine "directory", soprattutto quando si parla di manipolare file tramite la riga di comando. Esistono alcune sfumature, ma i due termini significano sostanzialmente la stessa cosa.

Il comando `cd` consente di Change Directory. Tecnicamente, `cd` non è un programma ma un comando integrato. Ciò significa che il sistema operativo lo fornisce pronto all'uso e che non può essere eliminato accidentalmente — per fortuna! Non è necessario preoccuparsi troppo di stabilire se un comando sia integrato o meno, ma occorre tenere presente che i comandi integrati sono presenti in tutti i sistemi basati su Unix.

1. Per cambiare directory, digitare `cd` nel terminale, seguito dalla directory in cui ci si vuole spostare. Supponendo che la directory si trovi all'interno della directory home, è possibile usare `cd Desktop` (vedere gli screenshot seguenti).

   ![risultati dell'esecuzione del comando cd Desktop in vari terminali Windows: il percorso del terminale si sposta nel desktop](win-terminals-cd.png)

2. Provare a digitare questo nel terminale del sistema:

   ```bash
   cd Desktop
   ```

3. Per risalire alla directory precedente, è possibile usare due punti. Digitare ora questo:

   ```bash
   cd ..
   ```

> [!NOTE]
> Una scorciatoia del terminale molto utile consiste nell'usare il tasto <kbd>tab</kbd> per completare automaticamente i nomi che si sa essere presenti, anziché digitare l'intero nome. Ad esempio, dopo avere digitato i due comandi precedenti, provare a digitare `cd D` e premere <kbd>tab</kbd>: il nome della directory `Desktop` dovrebbe essere completato automaticamente, purché sia presente nella directory corrente. Tenerlo presente man mano che si procede.

Se la directory da raggiungere è annidata in profondità, è necessario conoscerne il percorso. Questo di solito diventa più facile man mano che si acquisisce familiarità con la struttura del file system, ma se non si è sicuri del percorso, spesso è possibile determinarlo con una combinazione del comando `ls` (vedere sotto) e facendo clic nella finestra Explorer/Finder per vedere dove si trova una directory rispetto alla posizione corrente.

Ad esempio, se si volesse raggiungere una directory chiamata `src`, situata all'interno di una directory chiamata `project`, situata sul _Desktop_, sarebbe possibile digitare questi tre comandi per raggiungerla dalla directory _Home_:

```bash
cd Desktop
cd project
cd src
```

Ma è una perdita di tempo: si può invece digitare un solo comando, con i diversi elementi del percorso separati da barre oblique, proprio come quando si specificano percorsi a immagini o altre risorse nel codice CSS, HTML o JavaScript:

```bash
cd Desktop/project/src
```

Si noti che includere una barra iniziale nel percorso rende il percorso assoluto, ad esempio `/Users/your-user-name/Desktop`. Omettere la barra iniziale, come fatto sopra, rende il percorso relativo alla directory di lavoro corrente. Questo è esattamente ciò che avviene con gli URL nel browser web. Una barra iniziale significa "alla radice del sito web", mentre omettere la barra significa "l'URL è relativo alla pagina corrente".

> [!NOTE]
> In Windows vengono usate barre rovesciate anziché barre oblique, ad esempio `cd Desktop\project\src`. Può sembrare davvero strano, ma per approfondire il motivo, è possibile [guardare questo video su YouTube](https://www.youtube.com/watch?v=5T3IJfBfBmI), con una spiegazione di uno dei Principal Engineer di Microsoft.

### Elencare il contenuto di una directory

Un altro comando Unix integrato è `ls` (abbreviazione di list), che elenca il contenuto della directory in cui ci si trova. Si noti che questo non funziona se si utilizza il prompt dei comandi Windows predefinito (`cmd`): l'equivalente è `dir`.

Provare ora a eseguire questo nel terminale:

```bash
ls
```

Questo restituisce un elenco dei file e delle directory nella directory di lavoro corrente, ma le informazioni sono molto basilari: si ottiene solo il nome di ogni elemento presente, non se si tratta di un file o di una directory, né altro. Fortunatamente, una piccola modifica all'uso del comando può fornire molte più informazioni.

### Introduzione alle opzioni dei comandi

La maggior parte dei comandi del terminale ha opzioni: sono modificatori aggiunti alla fine di un comando, che ne modificano leggermente il comportamento. Di solito consistono in uno spazio dopo il nome del comando, seguito da un trattino e da una o più lettere.

Ad esempio, provare questo e vedere il risultato:

```bash
ls -l
```

Nel caso di `ls`, l'opzione `-l` (_trattino elle_) fornisce un elenco con un file o una directory per riga e molte più informazioni. Le directory possono essere identificate cercando la lettera "d" all'estrema sinistra delle righe. Sono quelle nelle quali è possibile usare `cd`.

Di seguito è presente uno screenshot con un terminale macOS "standard" nella parte superiore e un terminale personalizzato con alcune icone e colori aggiuntivi per renderlo più vivace: entrambi mostrano i risultati dell'esecuzione di `ls -l`:

![Un terminale macOS standard e un terminale macOS personalizzato più colorato, che mostrano un elenco di file: il risultato dell'esecuzione del comando ls -l](mac-terminals-ls.png)

> [!NOTE]
> Per scoprire esattamente quali opzioni sono disponibili per ciascun comando, è possibile consultare la relativa [man page](https://en.wikipedia.org/wiki/Man_page). Per farlo, digitare il comando `man` seguito dal nome del comando da cercare, ad esempio `man ls`. Questo aprirà la man page nel visualizzatore di file di testo predefinito del terminale, ad esempio [`less`](<https://en.wikipedia.org/wiki/Less_(Unix)>) nel terminale dell'autore, e dovrebbe quindi essere possibile scorrere la pagina usando i tasti freccia o un meccanismo simile. La man page elenca tutte le opzioni in grande dettaglio, il che può risultare un po' intimidatorio all'inizio, ma almeno si sa che è disponibile in caso di necessità. Una volta terminata la consultazione della man page, è necessario chiuderla usando il comando di uscita del visualizzatore di testo ("q" in `less`; potrebbe essere necessario cercarlo sul web se non è evidente).

> [!NOTE]
> Per eseguire un comando con più opzioni contemporaneamente, di solito è possibile inserirle tutte in un'unica stringa dopo il carattere trattino, ad esempio `ls -lah` oppure `ls -ltrh`. Provare a consultare la man page di `ls` per capire cosa fanno queste opzioni aggiuntive.

Ora che sono stati esaminati due comandi fondamentali, esplorare un po' la directory e vedere se è possibile navigare da un punto all'altro.

### Creare, copiare, spostare, rimuovere

Esistono vari altri comandi di utilità di base che probabilmente verranno usati molto durante il lavoro con il terminale. Sono piuttosto semplici, quindi non verranno spiegati tutti nel dettaglio quanto i due precedenti.

Provare a usarli in una directory di test creata in una posizione sicura, per evitare di eliminare accidentalmente qualcosa di importante, usando come guida i comandi di esempio seguenti:

- `mkdir`: crea una nuova directory all'interno della directory corrente, con il nome fornito dopo il nome del comando. Ad esempio, `mkdir my-awesome-website` creerà una nuova directory chiamata `my-awesome-website`.
- `rmdir`: rimuove la directory indicata, ma solo se è vuota. Ad esempio, `rmdir my-awesome-website` rimuoverà la directory creata sopra. Se si desidera rimuovere una directory non vuota, rimuovendo anche tutto ciò che contiene, è possibile usare invece `rm -r` (vedere sotto), ma è pericoloso. Assicurarsi che non vi sia nulla nella directory che potrebbe servire in seguito, poiché verrà eliminato definitivamente.
- `touch`: crea un nuovo file vuoto nella directory corrente. Ad esempio, `touch mdn-example.md` crea un nuovo file vuoto chiamato `mdn-example.md`.
- `mv`: sposta un file dalla prima posizione di file specificata alla seconda posizione di file specificata, ad esempio `mv mdn-example.md mdn-example.txt` (le posizioni sono scritte come percorsi di file). Questo comando sposta un file chiamato `mdn-example.md` nella directory corrente in un file chiamato `mdn-example.txt` nella directory corrente. Tecnicamente il file viene spostato, ma dal punto di vista pratico questo comando rinomina il file.
- `cp`: simile nell'uso a `mv`, `cp` crea una copia del file nella prima posizione specificata, nella seconda posizione specificata. Ad esempio, `cp mdn-example.txt mdn-example.txt.bak` crea una copia di `mdn-example.txt` chiamata `mdn-example.txt.bak` (naturalmente può essere chiamata diversamente).
- `rm`: rimuove il file specificato. Ad esempio, `rm mdn-example.txt` elimina un singolo file chiamato `mdn-example.txt`. Si noti che questa eliminazione è permanente e non può essere annullata tramite il cestino eventualmente presente nell'interfaccia utente desktop.

> [!NOTE]
> Molti comandi del terminale consentono di usare gli asterischi come caratteri "wildcard", nel senso di "qualsiasi sequenza di caratteri". Questo permette di eseguire un'operazione su un numero potenzialmente grande di file contemporaneamente, purché tutti corrispondano al modello specificato. Ad esempio, `rm mdn-*` eliminerebbe tutti i file che iniziano con `mdn-`. `rm mdn-*.bak` eliminerebbe tutti i file che iniziano con `mdn-` e terminano con `.bak`.

## Terminale: considerato dannoso?

È stato accennato in precedenza, ma per essere chiari: bisogna fare attenzione con il terminale. I comandi semplici non comportano molti rischi, ma quando si iniziano a mettere insieme comandi più complessi, è necessario riflettere attentamente su cosa farà il comando e provare prima a testarlo prima di eseguirlo infine nella directory prevista.

Supponiamo di avere 1000 file di testo in una directory e di volerli esaminare tutti per eliminare solo quelli che contengono una determinata sottostringa nel nome file. Se non si fa attenzione, si potrebbe finire per eliminare qualcosa di importante, perdendo molto lavoro nel processo.
Una buona abitudine consiste nello scrivere il comando del terminale in un editor di testo, capire quale dovrebbe essere la sua forma corretta, quindi creare una copia di backup della directory ed eseguire prima il comando su quella per testarlo.

Se non ci si sente a proprio agio nel provare i comandi del terminale sul proprio computer, sono disponibili terminali online ospitati che offrono luoghi sicuri in cui esercitarsi a inserire comandi, senza rischiare di danneggiare il proprio computer:

- Il partner di apprendimento [Scrimba](https://scrimba.com/home?via=mdn) include un terminale per inserire comandi nel proprio ambiente di apprendimento. Un ottimo punto in cui vederlo in azione è il corso [Command Line Basics](https://scrimba.com/command-line-basics-c08b87ogl0/~05hu?via=mdn) <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>, che fornisce anche una divertente introduzione interattiva alla navigazione nell'albero dei file e alla manipolazione di file e directory tramite il terminale.
- Il [playground della riga di comando](https://sandbox.bio/playgrounds/terminal) su sandbox.bio è un ottimo posto per provare comandi del terminale e acquisire familiarità con le interfacce a riga di comando e shell comuni come Bash.

Un'ottima risorsa per ottenere una panoramica rapida di comandi specifici del terminale è [tldr.sh](https://tldr.sh/). Si tratta di un servizio di documentazione gestito dalla comunità, simile a MDN, ma dedicato ai comandi del terminale.

Nella prossima sezione si aumenterà il livello di difficoltà, anche di parecchi livelli, e si vedrà come connettere insieme gli strumenti nella riga di comando per capire davvero in che modo il terminale può essere vantaggioso rispetto alla normale interfaccia utente desktop.

## Connettere i comandi con le pipe

Il terminale dà il meglio di sé quando si iniziano a concatenare i comandi usando il simbolo `|` (pipe). Vediamo un esempio molto rapido di cosa significa.

È già stato esaminato `ls`, che restituisce il contenuto della directory corrente:

```bash
ls
```

Ma cosa succede se si desidera contare rapidamente il numero di file e directory nella directory corrente? `ls` non può farlo da solo.

È disponibile un altro strumento Unix chiamato `wc`. Questo conta il numero di parole, righe, caratteri o byte di qualunque input gli venga fornito. Può trattarsi di un file di testo: l'esempio seguente restituisce il numero di righe in `myfile.txt`:

```bash
wc -l myfile.txt
```

Ma può anche contare il numero di righe di qualsiasi output che viene inviato tramite **pipe**. Ad esempio, il comando seguente conta il numero di righe restituite dal comando `ls` — ciò che normalmente stamperebbe nel terminale se eseguito da solo — e stampa invece quel conteggio nel terminale:

```bash
ls | wc -l
```

Poiché `ls` stampa ogni file o directory su una riga separata, questo fornisce di fatto un conteggio di directory e file.

Quindi, cosa sta accadendo? Una filosofia generale degli strumenti della riga di comando Unix è che stampano testo nel terminale, detta anche "stampa nell'output standard" o `STDOUT`. Molti comandi possono anche leggere contenuti da input in streaming, detto "input standard" o `STDIN`.

L'operatore pipe può _connettere_ questi input e output, consentendo di costruire operazioni sempre più complesse in base alle necessità: l'output di un comando può diventare l'input del comando successivo. In questo caso, `ls` normalmente stamperebbe il proprio output su `STDOUT`, ma invece l'output di `ls` viene inviato tramite pipe a `wc`, che riceve quell'output come input, conta il numero di righe che contiene e stampa invece quel conteggio su `STDOUT`.

## Un esempio leggermente più complesso

Esaminiamo qualcosa di un po' più complicato.

1. Per prima cosa si proverà a recuperare il contenuto della pagina "fetch" di MDN usando il comando `curl`, che può essere usato per richiedere contenuto dagli URL, da `https://developer.mozilla.org/it/docs/Web/API/WindowOrWorkerGlobalScope/fetch`. Provarlo ora:

   ```bash
   curl https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/fetch
   ```

   Non si otterrà un output perché la pagina è stata reindirizzata a [/Web/API/fetch](/it/docs/Web/API/Window/fetch). È necessario indicare esplicitamente a `curl` di seguire i reindirizzamenti usando il flag `-L`.

2. Esaminiamo anche gli header restituiti da `developer.mozilla.org` usando il flag `-I` di `curl` e stampiamo tutti i reindirizzamenti di posizione inviati al terminale, inviando tramite pipe l'output di `curl` a `grep` (verrà chiesto a `grep` di restituire tutte le righe che contengono la parola "location"). Provare a eseguire quanto segue: si vedrà che è presente un solo reindirizzamento prima di raggiungere la pagina finale.

   ```bash
   curl https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/fetch -L -I | grep location
   ```

   L'output dovrebbe essere simile al seguente (`curl` stamperà prima alcuni contatori di download e simili):

   ```bash
   location: /en-US/docs/Web/API/Window/fetch
   ```

3. Sebbene artificioso, è possibile sviluppare ulteriormente questo risultato e trasformare il contenuto della riga `location:`, aggiungendo l'origine di base all'inizio di ciascuna riga per ottenere URL completi stampati. A questo scopo verrà aggiunto `awk`, un linguaggio di programmazione simile a JavaScript, Ruby o Python, solo molto più vecchio. Provare a eseguire questo:

   ```bash
   curl https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/fetch -L -I | grep location | awk '{ print "https://developer.mozilla.org" $2 }'
   ```

L'output finale dovrebbe essere simile al seguente:

```bash
https://developer.mozilla.org/en-US/docs/Web/API/Window/fetch
```

Combinando questi comandi, l'output è stato personalizzato per mostrare gli URL completi attraverso i quali il server Mozilla reindirizza quando viene richiesta l'URL `/docs/Web/API/WindowOrWorkerGlobalScope/fetch`.
Conoscere bene il proprio sistema si rivelerà utile negli anni a venire: imparare come funzionano questi strumenti dedicati a un singolo compito e come possono entrare a far parte del proprio toolkit per risolvere problemi specifici.

## Aggiungere potenziamenti

Dopo avere esaminato alcuni dei comandi integrati con cui il sistema è dotato, vediamo come installare uno strumento CLI di terze parti e usarlo.

Il vasto ecosistema di strumenti installabili per lo sviluppo web front-end esiste attualmente soprattutto all'interno di [npm](https://www.npmjs.com/), un servizio privato di hosting di pacchetti che lavora a stretto contatto con Node.js.
Questo ecosistema si sta lentamente espandendo: nel tempo è possibile aspettarsi di vedere più fornitori di pacchetti.

[Installare Node.js](https://nodejs.org/en/) installa anche lo strumento della riga di comando npm e uno strumento supplementare incentrato su npm chiamato npx, che offre un accesso per installare ulteriori strumenti della riga di comando. Node.js e npm funzionano allo stesso modo su tutti i sistemi: macOS, Windows e Linux.

Installare ora npm sul sistema, andando all'URL precedente e scaricando ed eseguendo un programma di installazione Node.js adatto al sistema operativo. Se richiesto, assicurarsi di includere npm come parte dell'installazione.

![il programma di installazione di Node.js su Windows, che mostra l'opzione per includere npm](npm-install-option.png)

Verrà usato nuovamente [Prettier](https://prettier.io/) come esempio. È stato mostrato come installarlo come estensione di VS Code nell'articolo [Editor di codice](/it/docs/Learn_web_development/Getting_started/Environment_setup/Code_editors#enhancing_your_code_editor_with_extensions). Qui verrà mostrato come installarlo come strumento da riga di comando.

> [!NOTE]
> Prettier è un formattatore di codice con opinioni precise che ha solo "poche opzioni". Meno opzioni tendono a significare maggiore semplicità. Poiché gli strumenti possono talvolta diventare eccessivamente complessi, "poche opzioni" può essere molto interessante.

### Dove installare gli strumenti CLI?

Prima di installare Prettier, occorre rispondere a una domanda: "dove installarlo?"

Con `npm` è possibile scegliere di installare gli strumenti globalmente, così da potervi accedere ovunque, oppure localmente nella directory del progetto corrente.

Entrambe le modalità hanno vantaggi e svantaggi, e i seguenti elenchi di pro e contro dell'installazione globale sono tutt'altro che esaustivi.

**Vantaggi dell'installazione globale:**

- Accessibile ovunque nel terminale
- Installazione necessaria una sola volta
- Utilizza meno spazio su disco
- Sempre la stessa versione
- Sembra un qualsiasi altro comando Unix

**Svantaggi dell'installazione globale:**

- Potrebbe non essere compatibile con la codebase del progetto
- Gli altri sviluppatori del team non avranno accesso a questi strumenti, ad esempio se la codebase viene condivisa tramite uno strumento come git.
- Collegato al punto precedente, rende più difficile replicare il codice del progetto. Se gli strumenti vengono installati localmente, possono essere configurati come dipendenze e installati con <code>npm install</code>.

Sebbene l'elenco degli _svantaggi_ sia più breve, l'impatto negativo dell'installazione globale è potenzialmente molto maggiore dei benefici.
Qui l'installazione verrà effettuata localmente, ma è possibile installare globalmente una volta compresi i rischi relativi.

### Installare Prettier

Prettier è uno strumento di formattazione del codice con opinioni precise per gli sviluppatori front-end, incentrato sui linguaggi basati su JavaScript e con supporto per HTML, CSS, SCSS, JSON e altro ancora.

Prettier può:

- Eliminare il sovraccarico cognitivo della coerenza manuale dello stile in tutti i file di codice; Prettier può farlo automaticamente.
- Aiutare chi è alle prime armi con lo sviluppo web a formattare il proprio codice secondo le best practice.
- Essere installato su qualsiasi sistema operativo e persino direttamente come parte degli strumenti del progetto, assicurando che colleghi e amici che lavorano sul codice usino lo stesso stile di codice.
- Essere configurato per essere eseguito al salvataggio, durante la digitazione o persino prima di pubblicare il codice, con strumenti aggiuntivi che verranno esaminati più avanti nel modulo.

Per questo articolo, Prettier verrà installato localmente, come suggerito nella [guida all'installazione di Prettier](https://prettier.io/docs/install.html).

1. Dopo aver installato Node, aprire il terminale ed eseguire il comando seguente per installare Prettier. Il significato di `--save-dev` verrà spiegato nel prossimo articolo.

   ```bash
   npm install --save-dev prettier
   ```

2. Ora è possibile eseguire il file localmente usando lo strumento [npx](https://docs.npmjs.com/cli/commands/npx/). Eseguire il comando senza argomenti, come per molti altri comandi, fornirà informazioni sull'uso e sulla guida. Provarlo ora:

   ```bash
   npx prettier
   ```

L'output dovrebbe essere simile al seguente:

```bash
Usage: prettier [options] [file/glob ...]

By default, output is written to stdout.
Stdin is read if it is piped to Prettier and no files are given.

…
```

Vale sempre la pena almeno scorrere le informazioni sull'uso, anche quando sono lunghe.
Aiuteranno a comprendere meglio come lo strumento è progettato per essere usato.

> [!NOTE]
> Se Prettier non è stato prima installato localmente, l'esecuzione di `npx prettier` scaricherà ed eseguirà l'ultima versione di Prettier in un'unica operazione _solo per quel comando_.
> Anche se può sembrare ottimo, le nuove versioni di Prettier potrebbero modificare leggermente l'output.
> Si desidera installarlo localmente per fissare la versione di Prettier usata per la formattazione finché non si è pronti a cambiarla.

### Provare Prettier

Facciamo una rapida prova di Prettier, per vedere come funziona.

1. Prima di tutto, creare una nuova directory in un punto del file system facile da trovare. Ad esempio, una directory chiamata `prettier-test` sul `Desktop`.

2. Ora salvare il codice seguente in un nuovo file chiamato `index.js`, all'interno della directory di test:

   ```js-nolint
   const myObj = {
   a:1,b:{c:2}}
   function printMe(obj){console.log(obj.b.c)}
   printMe(myObj)
   ```

3. È possibile eseguire Prettier su una codebase solo per verificare se il codice necessita di modifiche. Usare `cd` per entrare nella directory e provare a eseguire questo comando:

   ```bash
   npx prettier --check index.js
   ```

   Si dovrebbe ottenere un output simile a:

   ```bash
   Checking formatting...
   index.js
   Code style issues found in the above file(s). Forgot to run Prettier?
   ```

4. Quindi, sono presenti alcuni stili di codice che possono essere corretti. Nessun problema. Aggiungere l'opzione `--write` al comando `prettier` correggerà questi aspetti, lasciando concentrare sull'effettiva scrittura di codice utile. Ora provare a eseguire questa versione del comando:

   ```bash
   npx prettier --write index.js
   ```

   Si otterrà un output simile a questo:

   ```bash
   Checking formatting...
   index.js
   Code style issues fixed in the above file(s).
   ```

   Ma, cosa più importante, se si controlla nuovamente il file JavaScript, si vedrà che è stato riformattato in modo simile a questo:

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

A seconda del flusso di lavoro utilizzato o scelto, questo può diventare una parte automatizzata del processo. L'automazione è davvero il campo in cui gli strumenti eccellono; la preferenza personale è il tipo di automazione che "avviene e basta", senza dover configurare nulla.

Con Prettier esistono vari modi per ottenere l'automazione e, sebbene vadano oltre lo scopo di questo articolo, online sono disponibili ottime risorse di supporto, alcune delle quali sono state collegate. È possibile invocare Prettier:

- Prima di effettuare il commit del codice in un repository git usando [Husky](https://github.com/typicode/husky).
- Ogni volta che si preme "salva" nell'editor di codice, sia [VS Code](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode) sia [Sublime Text](https://packagecontrol.io/packages/JsPrettier).
- Come parte dei controlli di {{Glossary("continuous_integration", "integrazione continua")}} usando strumenti come [GitHub Actions](https://github.com/features/actions).

La preferenza personale è la seconda opzione: durante l'uso di VS Code, ad esempio, Prettier interviene e corregge la formattazione necessaria ogni volta che si preme salva. Molte più informazioni sull'uso di Prettier in modi diversi sono disponibili nella [documentazione di Prettier](https://prettier.io/docs/).

## Altri strumenti da provare

Per provare qualche altro strumento, ecco un breve elenco di strumenti divertenti da testare:

- [`bat`](https://github.com/sharkdp/bat): un `cat` "migliore" (`cat` viene usato per stampare il contenuto dei file).
- [`prettyping`](https://denilson.sa.nom.br/prettyping/): `ping` nella riga di comando, ma visualizzato (`ping` è uno strumento utile per verificare se un server risponde).
- [`htop`](https://htop.dev/): un visualizzatore di processi, utile quando qualcosa fa comportare la ventola della CPU come un motore a reazione e si vuole identificare il programma responsabile.
- [`tldr`](https://tldr.sh/#installation): menzionato prima in questo capitolo, ma disponibile come strumento da riga di comando.

Si noti che alcuni dei suggerimenti precedenti potrebbero richiedere l'installazione tramite npm, come fatto con Prettier.

## Riepilogo

Questo conclude il tour introduttivo del terminale/della riga di comando e il modulo di configurazione dell'ambiente. Successivamente, si inizierà a lavorare alla creazione del primo semplice sito web, per avere un'idea di come sia lo sviluppo web.

{{PreviousMenuNext("Learn_web_development/Getting_started/Environment_setup/Dealing_with_files", "Learn_web_development/Getting_started/Your_first_website", "Learn_web_development/Getting_started/Environment_setup")}}
