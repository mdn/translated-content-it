---
title: Gestione dei file
slug: Learn_web_development/Getting_started/Environment_setup/Dealing_with_files
l10n:
  sourceCommit: be1922d62a0d31e4e3441db0e943aed8df736481
---

{{PreviousMenuNext("Learn_web_development/Getting_started/Environment_setup/Code_editors", "Learn_web_development/Getting_started/Environment_setup/Command_line", "Learn_web_development/Getting_started/Environment_setup")}}

Un sito web è composto da molti file: contenuti testuali, codice, fogli di stile, contenuti multimediali, e così via. Quando costruisce un sito web, bisogna assemblare questi file in una struttura sensata sul proprio computer locale, assicurarsi che possano comunicare tra loro e far sì che tutto il contenuto appaia corretto prima di metterlo su un server per essere visibile al mondo. Questo articolo spiega come utilizzare l'interfaccia utente (UI) dell'esploratore di file del proprio computer e impostare una struttura di file sensata per un sito web.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con il sistema operativo (OS) del proprio computer e con il software di base che si utilizzerà per costruire un sito web.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Manipolazione di file e cartelle.</li>
          <li>Migliori pratiche di denominazione.</li>
          <li>Struttura standard delle cartelle di un sito web.</li>
          <li>Gestione dei percorsi dei file</li>
          <li>Gestione delle estensioni dei file.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Manipolare file e cartelle

Ci sono molti modi diversi per creare e modificare i file e le cartelle contenuti sul tuo computer. Può farlo tramite la riga di comando/terminale del computer utilizzando una serie di comandi di testo, che imparerà meglio nel prossimo articolo. Tuttavia, molte persone trovano più facile iniziare a comprendere i sistemi di file visivamente, e di questo parleremo qui. I moderni sistemi operativi (OS) hanno un'interfaccia utente (UI) del file system robusta che può utilizzare per manipolare file e cartelle come necessario.

Su macOS, ad esempio, ha il programma Finder:

![L'applicazione Finder su macOS, che mostra il contenuto di una tipica cartella Home](finder.png)

Mentre Windows ha l'Esplora File:

![L'applicazione Esplora File su Windows, che mostra il contenuto di una tipica cartella Home](file-explorer.png)

> [!NOTE]
> Questa guida è stata scritta utilizzando Windows 11 e macOS 15. Potrebbe utilizzare una versione diversa del sistema operativo o un sistema operativo completamente diverso, nel qual caso l'esperienza potrebbe differire leggermente. Ci sono molte guide sul web sull'uso base del sistema operativo — la invitiamo a cercare sul web informazioni sul suo particolare sistema operativo.

### Struttura base

La maggior parte dei moderni sistemi operativi ha una cartella `Users`, che contiene una cartella per ciascun account utente esistente sul sistema, conosciuta anche come la cartella _Home_ dell'utente. Questa di solito è rappresentata da un'icona a forma di casa per renderla più facile da trovare. A sua volta, la cartella _Home_ conterrà altre cartelle standard importanti (e file) rilevanti per quell'utente in particolare, come _Documents_, _Music_, ecc. Ci sono parecchi altri file e cartelle sul suo computer, ma per ora non si preoccupi di quelli.

L'utente attualmente connesso, per impostazione predefinita, potrà accedere solo alla propria cartella _Home_.

Dovrebbe creare file di progetto relativi al suo lavoro da qualche parte all'interno della cartella _Home_, magari all'interno di _Documents_. Questo ha senso, dato che i file delle pagine web sono spesso riferiti come _documenti_.

> [!WARNING]
> Se inizia a creare e modificare file in altre parti del sistema (per esempio, aree che controllano il sistema operativo o applicazioni importanti), potrebbe rompere qualcosa. Per il momento, limiti la creazione e modifica dei file all'interno della cartella _Home_.

### Creare una cartella

Creiamo una nuova cartella per conservare tutti i nostri progetti web.

1. Nella sua UI del file system, clicchi sulla cartella _Home_, poi faccia doppio clic sulla cartella _Documents_.
2. Crei una nuova cartella in questa posizione chiamata `web-projects`:
   1. Su Windows, può farlo selezionando il pulsante _New_ nella finestra di Esplora File e selezionando _Folder_ (o premendo <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>N</kbd>), digitando `web-projects` come nome per la nuova icona di cartella che appare, e premendo <kbd>Enter</kbd>/<kbd>Return</kbd>.
   2. Su macOS, può farlo selezionando _File_ > _New Folder_ nel menu Finder (o premendo <kbd>Cmd</kbd> + <kbd>Shift</kbd> + <kbd>N</kbd>) — vedrà apparire una nuova cartella chiamata _untitled folder_. Clicchi sul nome della cartella per iniziare a modificarlo, digiti `web-projects`, e prema <kbd>Enter</kbd>/<kbd>Return</kbd>.

Se commette un errore di battitura, può modificare il nome della cartella per correggerlo (questo funziona anche con i file):

- Su Windows, clicchi con il tasto destro sulla cartella, selezioni _Rename_ dal menu, poi lo modifichi. Alcune versioni di Windows hanno un menu semplificato mostrato inizialmente — potrebbe dover cliccare con il tasto destro, poi selezionare _Show more options_, quindi selezionare _Rename_!
- Su macOS, clicchi su/selezioni il nome della cartella per modificarlo.

### Aprire una cartella di progetto e creare file in VS Code

Anche se può creare file di testo all'interno dell'UI del file system del sistema operativo, è generalmente più facile e meno predisposto agli errori crearli all'interno del suo editor di codice. Infatti, VS Code ha il proprio esploratore di file che le consente di creare tutte le cartelle e i file di cui ha bisogno per i suoi progetti web.

Quindi, perché l'abbiamo fatta passare per la difficoltà di creare una cartella tramite l'UI del file system del sistema operativo? Perché VS Code deve essere indicato su una cartella di livello superiore iniziale!

È anche utile capire un po' come è strutturato il suo file system del sistema operativo. Questo diventerà più utile quando si inizierà a utilizzare strumenti più complessi più avanti.

Apriamo ora la cartella `web-projects` in VS Code:

1. Apra VS Code.
2. Selezioni _File_ > _Open Folder..._ dal menu.
   > [!NOTE]
   > Se è un utente da tastiera, può eseguire il comando _Open Folder_ su Windows tenendo premuto il tasto <kbd>Ctrl</kbd> e premendo <kbd>K</kbd> poi <kbd>O</kbd>. Il modo più semplice per un utente macOS di farlo è aprire il _Command Palette_ con <kbd>Cmd</kbd> + <kbd>Shift</kbd> + <kbd>P</kbd>, digitare "Open Folder" per filtrare l'elenco dei comandi, utilizzare i tasti cursore per muoversi verso il basso fino a _File: Open Folder_, quindi premere <kbd>Enter</kbd>.
3. Apparirà una versione mini dell'UI del file system del sistema operativo. Usi questa per trovare la sua cartella `web-projects`, selezionarla, quindi premere il pulsante _Select Folder_.
4. Verrà presentata una finestra di dialogo intitolata _Do you trust the authors of the files in this folder?_. Legga attentamente per capire di cosa si tratta. Al momento, è l'unica persona che creerà i file in questa cartella, quindi può cliccare _Yes, I trust the authors_.

Dovrebbe vedere la cartella `web-projects` aperta nel riquadro _EXPLORER_ di VS Code, come mostrato di seguito:

![Il pannello Explora di VS Code, che mostra una cartella vuota chiamata web-projects](vs-code-explorer.png)

> [!WARNING]
> Ancora una volta, si assicuri di attenersi alla modifica dei propri file all'interno della cartella _Home_ per ora, per evitare di causare qualsiasi problema con il sistema.

#### Un'annotazione sulla navigazione tramite tastiera in VS Code

VS Code, pur non essendo perfetto sotto molti aspetti, ha una serie estesa di scorciatoie da tastiera. Durante questo articolo abbiamo cercato di includere quelle utili dove possibile, ma può trovare elenchi più completi nello [Riferimento alle scorciatoie da tastiera di VS Code](https://code.visualstudio.com/docs/configure/keybindings).

In generale, se si vuole navigare in VS Code tramite tastiera, può premere il tasto <kbd>Tab</kbd> per spostarsi nelle diverse aree dell'interfaccia (<kbd>Shift</kbd> + <kbd>Tab</kbd> la riporterà su una posizione di tabulazione precedente). Se ci sono più pulsanti in una posizione di tabulazione, può usare i tasti cursore per muoversi tra di essi.

Se sta attualmente modificando un file, il tasto tab non navigherà nell'interfaccia — aggiungerà invece caratteri di tabulazione nel file. Per uscire dal file che sta modificando e passare al riquadro _EXPLORER_, può premere <kbd>Cmd</kbd> + <kbd>Shift</kbd> + <kbd>E</kbd> su macOS, o <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>E</kbd> su Windows.

Per tornare al pannello dell'editor dei file e iniziare a muoversi tra i diversi file aperti in schede diverse, tenga premuto il tasto <kbd>Ctrl</kbd> e usi <kbd>Tab</kbd> e <kbd>Shift</kbd> + <kbd>Tab</kbd> per muoversi su e giù nell'elenco delle schede aperte (sia su macOS che su Windows). Una volta evidenziato il file che si desidera modificare, rilasci i tasti per passare a quella scheda.

#### Creando un file

Da qui, può creare nuovi file e cartelle usando i pulsanti rilevanti nella parte superiore del riquadro _EXPLORER_.

1. Crei un nuovo file cliccando sull'icona _New File..._ (o <kbd>Tab</kbd> su esso e premi <kbd>Enter</kbd>/<kbd>Return</kbd>).
2. Inserisca il nome del file come "index.html" nella casella di inserimento testo che appare e premi <kbd>Enter</kbd>/<kbd>Return</kbd>.

> [!NOTE]
> Non usi i pulsanti nella parte superiore della scheda _Welcome_ per creare file e cartelle, perché funzionano in modo un po' diverso. Infatti, può chiudere la scheda _Welcome_, dato che non le serve. Lo faccia cliccando la "x" nella parte destra della scheda, o premendo <kbd>Cmd</kbd> + <kbd>W</kbd> su macOS (<kbd>Ctrl</kbd> + <kbd>W</kbd> su Windows).

A questo punto, torni alla UI del file system del suo sistema operativo, entri nella cartella `web-projects` facendo doppio clic su di essa, e dovrebbe vedere lì anche il suo file `index.html`. VS Code utilizza il file system sottostante del sistema operativo, non usa un sistema di file strano proprio.

### Spostare index.html nella sua sottocartella

Può creare cartelle all'interno di altre cartelle (chiamate _sottocartelle_) tutti i livelli in profondità che desidera. Può anche spostare file (e cartelle) all'interno di altre cartelle trascinandoli e rilasciandoli sopra quella cartella.

Esploriamolo, e nel processo, spostiamo il nostro file `index.html` all'interno della sua sottocartella. Non vogliamo davvero che sia seduto all'interno della cartella principale `web-projects`.

1. Crei una nuova cartella all'interno di `web-projects`, utilizzando il pulsante _New Folder..._ del riquadro _EXPLORER_ di VS Code.
2. Le attribuisca il nome `test-site`.
3. Ora dovrebbe essere in grado di trascinare il file `index.html` e rilasciarlo sopra la cartella `test-site` per spostare il file all'interno della cartella.
   > [!NOTE]
   > Se è un utente da tastiera, può farlo seguendo questi passaggi:
   >
   > 1. Usi i tasti freccia su e giù per spostare il contorno del focus sul file `index.html`.
   > 2. Prema <kbd>Cmd</kbd> + <kbd>X</kbd> su macOS (<kbd>Ctrl</kbd> + <kbd>X</kbd> su Windows) per selezionare il file per il trasferimento.
   > 3. Usi i tasti freccia per spostare il contorno del focus sulla cartella.
   > 4. Prema <kbd>Cmd</kbd> + <kbd>V</kbd> su macOS (<kbd>Ctrl</kbd> + <kbd>V</kbd> su Windows) per spostare il file in quella cartella.

C'è molto altro che potremmo includere sull'utilizzo delle UI del file system del sistema operativo e di VS Code, ma abbiamo spazio limitato, quindi ci fermeremo qui per ora. Questo le ha fornito abbastanza informazioni per iniziare, e le consigliamo di cercare sul web informazioni su come fare altre operazioni con file e cartelle.

Passiamo ora a una breve discussione sulla struttura di un sito web.

## Che struttura dovrebbe avere un sito web?

Quando lavora sui siti web localmente (sul suo computer), dovrebbe mantenere tutti i file correlati a ciascun sito in una singola cartella. A sua volta, dovrebbe tenere tutte le sue cartelle del sito web in una cartella centrale, in modo che siano tutte facili da trovare.

Prima nell'articolo, l'abbiamo istruita a creare una cartella centrale chiamata `web-projects` per conservare tutti i suoi progetti di siti web. Le abbiamo anche fatto creare una sottocartella chiamata `test-site` con un file `index.html` vuoto all'interno.

Aggiungiamo alcune altre funzionalità all'interno di `test-site` per dimostrare una tipica struttura di un sito web; nel prossimo modulo, le chiederemo di costruire un esempio di sito web completo al suo interno. Le cose più comuni che qualsiasi progetto di sito web conterrà sono un file di indice HTML e cartelle per contenere immagini, file di stile, e file script:

1. **`index.html`**: Questo file generalmente conterrà i contenuti della sua homepage, cioè il testo e le immagini che le persone vedono quando visitano per la prima volta il suo sito.
2. **Cartella `images`**: Questa cartella conterrà tutte le immagini che utilizza sul suo sito.
3. **Cartella `styles`**: Questa cartella conterrà il codice CSS utilizzato per stilizzare il suo contenuto (ad esempio, impostando i colori del testo e dello sfondo).
4. **Cartella `scripts`**: Questa cartella conterrà tutto il codice JavaScript utilizzato per aggiungere funzionalità interattive al suo sito (ad esempio, definendo cosa succede quando vengono cliccati i pulsanti).

> [!CALLOUT]
>
> **Provi**
>
> Dovrebbe già avere un file `index.html` dentro `test-site`. Crei ora le cartelle `images`, `styles`, e `scripts` al suo interno.

## Nome dei file

Ci sono generalmente due parti in un nome file — il **nome** e l'**estensione**. Prenda il file che abbiamo creato sopra — `index.html`:

- Il nome in questo caso è `index`. I nomi dei file generalmente possono contenere qualsiasi carattere si desideri, sebbene i diversi sistemi informatici avranno varie restrizioni sui caratteri che possono essere utilizzati. È meglio attenersi a numeri e lettere, almeno all'inizio. Inoltre, i sistemi possono dare un significato speciale a determinati nomi o parti di nomi — come abbiamo già detto, i file `index` tendono ad essere riconosciuti come il file principale della homepage di un sito web.
- L'estensione del file identifica il tipo di file di cui stiamo trattando ed è utilizzata dai sistemi informatici per identificare quale tipo di contenuto si può aspettare nel file, quale programma dovrebbe usare per aprire il file, ecc. in questo caso, l'estensione è `.html`, che significa che il file dovrebbe contenere testo semplice, e più specificamente, codice HTML. Grazie all'estensione, il suo computer sa che quando tenta di aprire il file dovrebbe aprirlo utilizzando il suo editor di testo predefinito, che dovrebbe essere VS Code se ha seguito tutte le nostre istruzioni fino ad ora.

Non è vero in tutti i casi, ma la maggior parte dei file necessita di un'estensione per essere gestiti correttamente. Rimuovere o cambiare l'estensione del file è probabile che causi errori, quindi non dovrebbe alterarla a meno di sapere veramente cosa sta facendo.

> [!NOTE]
> È possibile mettere più di un punto in un nome file, ad esempio `my.cats.html`. In tali casi, l'ultimo punto è considerato l'inizio dell'estensione del file.

Sui computer Windows, potrebbe avere difficoltà a vedere le estensioni di alcuni file perché Windows ha un'opzione chiamata **Nascondi estensioni per i tipi di file conosciuti** abilitata per impostazione predefinita. Può disattivarlo andando su Esplora File, selezionando l'opzione **Opzioni cartella…**, deselezionando la casella **Nascondi estensioni per i tipi di file conosciuti**, quindi cliccando su **OK**. Per informazioni più specifiche riguardo alla sua versione di Windows, può fare una ricerca sul web.

### Migliori pratiche per nominare i file

Mentre segue questo corso, noterà che le chiediamo sempre di nominare cartelle e file completamente in minuscolo senza spazi. Ci sono molti modi in cui l'uso di spazi in nomi di file e cartelle crea problemi — alcuni tra i più comuni sono i seguenti:

<!-- cSpell:ignore myimage -->

1. Molti sistemi informatici, inclusi la maggior parte dei server web, sono sensibili alla distinzione tra maiuscole e minuscole. Quindi ad esempio, se inserisce un'immagine sul suo sito web in `test-site/images/MyImage.jpg` e poi in un file diverso tenta di fare riferimento all'immagine con `test-site/images/myimage.jpg`, potrebbe non funzionare.
2. Quando esegue comandi sulla riga di comando, deve mettere virgolette intorno ai nomi di file con spazi, altrimenti verranno interpretati come due elementi separati.
3. Alcuni linguaggi di programmazione (ad esempio, Python) non funzionano bene con spazi nei nomi di file in certe situazioni (ad esempio, se questi file sono moduli da importare).
4. I nomi dei file comunemente si mappano agli indirizzi web/URL. Se, ad esempio, ha un file chiamato `my file.html` nella cartella radice del suo server, generalmente sarà accessibile a un URL come `https://example.com/my%20file.html`. I server web di solito sostituiscono gli spazi nei nomi dei file con `%20` (perché gli URL sono {{Glossary("Percent-encoding", "percent-encoded")}}), il che può creare bug sottili con alcuni sistemi se assumono che i nomi dei file e gli URL corrispondano perfettamente.

Invece degli spazi, molti sviluppatori utilizzano un carattere separatore come un trattino (`-`) piuttosto che uno spazio — ad esempio `my-file.html` piuttosto che `my file.html`. Questa è una buona pratica.

È meglio abituarsi a scrivere i nomi delle sue cartelle e file in minuscolo senza spazi e con le parole separate dai trattini, almeno finché non sa cosa sta facendo. In questo modo, incontrerà meno problemi in futuro.

> [!NOTE]
> Può trovare altre migliori pratiche per i nomi dei file e gli URL in [URL structure best practices for Google](https://developers.google.com/search/docs/crawling-indexing/url-structure).

## Percorsi dei file

Per fare riferimento a un file da un altro, deve fornire un percorso del file — fondamentalmente un itinerario, in modo che un file sappia dove si trova un altro. Ad esempio, quando crea una pagina web che contiene un'immagine, il codice della sua pagina web dovrà contenere un percorso del file che indica la posizione dell'immagine che desidera visualizzare.

Vediamo un esempio base di questo. Potrebbe non capire cosa tutto questo significhi per ora, ma va bene.

1. Cerchi sul web un'immagine che le piace (ad esempio, utilizzando un servizio come [Google Images](https://www.google.com/imghp)) e la scarichi. In alternativa, può semplicemente prendere la nostra [icona dell'immagine di Firefox](https://raw.githubusercontent.com/mdn/beginner-html-site/refs/heads/main/images/firefox-icon.png) da utilizzare per questo esempio.
2. Metta l'immagine all'interno della cartella _images_.
3. Si assicuri che il file dell'immagine abbia un nome breve e semplice senza spazi. Ad esempio, `firefox-icon.png` va bene, e `cat.jpg` va bene, ma `efregre^%^££@%$^&YTJgfbgfdgt54656756_ertgrth-rtgtfghhyj.png` non va bene. Inoltre, si assicuri di preservare l'estensione del file.

Ora aggiungeremo contenuti al file `index.html` per permettergli di localizzare il file dell'immagine e visualizzarlo.

1. Apra il suo `index.html` in VS Code e inserisca il seguente contenuto nel file esattamente come mostrato sotto. Questo è HTML, il linguaggio che usiamo per definire e strutturare il contenuto della pagina web. Imparerà molto di più su questo molto presto!

   ```html
   <!doctype html>
   <html lang="en-US">
     <head>
       <meta charset="utf-8" />
       <meta name="viewport" content="width=device-width" />
       <title>My test page</title>
     </head>
     <body>
       <img src="" alt="My test image" />
     </body>
   </html>
   ```

2. La riga `<img src="" alt="My test image">` è il codice HTML che inserisce un'immagine nella pagina. Dobbiamo dire all'HTML dove si trova l'immagine. L'immagine è all'interno della cartella _images_, che si trova nella stessa cartella di `index.html`. Per scendere attraverso la struttura dei file da `index.html` alla nostra immagine, il percorso del file di cui abbiamo bisogno è `images/il-nome-del-suo-file-immagine`. Ad esempio, se la sua immagine si chiama `firefox-icon.png`, il percorso del file sarebbe `images/firefox-icon.png`.
3. Inserisca il percorso del file nel suo codice HTML tra le virgolette doppie di `src=""`.
4. Salvi il suo file HTML, e lo carichi nel suo browser web. Può fare questo cliccando con il tasto destro/Ctrl click sul file HTML, poi scegliendo _Open With_ e selezionando un browser web dal sottomenu risultante. Potrebbe anche aprire il suo UI del file system e una finestra del browser web sullo stesso schermo, e trascinare e rilasciare il file HTML sopra la finestra del browser web.

Dovrebbe vedere una pagina web base che visualizza la sua immagine!

![Uno screenshot del nostro sito web base che mostra solo il logo di Firefox - una volpe in fiamme che avvolge il mondo](website-screenshot.png)

### Regole generali per i percorsi dei file

- Per collegare un file target nella stessa cartella del file HTML invocante, usi solo il nome del file, ad esempio `my-image.jpg`.
- Per fare riferimento a un file in una sottocartella, scriva il nome della cartella davanti al percorso, più una barra in avanti, ad esempio `subfolder/my-image.jpg`.
- Per collegare un file target nella cartella **sopra** il file HTML invocante, scriva due punti. Quindi, ad esempio, se `index.html` fosse in una sottocartella di `test-site` e `my-image.jpg` fossi dentro `test-site`, potrebbe fare riferimento a `my-image.jpg` da `index.html` usando `../my-image.jpg`.
- Può combinare tutto questo quanto desidera, ad esempio `../subfolder/another-subfolder/my-image.jpg`.

> [!NOTE]
> Il file system di Windows tende a usare le barre rovesciate, non le barre in avanti, ad esempio, `C:\Windows`. Questo non importa in HTML — anche se sta sviluppando il suo sito web su Windows, dovrebbe comunque usare le barre in avanti nel suo codice.

{{PreviousMenuNext("Learn_web_development/Getting_started/Environment_setup/Code_editors", "Learn_web_development/Getting_started/Environment_setup/Command_line", "Learn_web_development/Getting_started/Environment_setup")}}
