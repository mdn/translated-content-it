---
title: Gestire i file
slug: Learn_web_development/Getting_started/Environment_setup/Dealing_with_files
l10n:
  sourceCommit: 12fddfa7a8dfa7f4f30f7f55889b0e94a585d847
---

{{PreviousMenuNext("Learn_web_development/Getting_started/Environment_setup/Code_editors", "Learn_web_development/Getting_started/Environment_setup/Command_line", "Learn_web_development/Getting_started/Environment_setup")}}

Un sito web è composto da molti file: contenuti testuali, codice, fogli di stile, contenuti multimediali e così via. Quando si crea un sito web, è necessario organizzare questi file in una struttura sensata sul computer locale, assicurarsi che possano comunicare tra loro e fare in modo che tutti i contenuti abbiano l'aspetto desiderato prima di caricarli infine su un server affinché siano visibili al mondo. Questo articolo spiega come usare l'interfaccia utente (UI) dell'esplora file del computer e configurare una struttura di file sensata per un sito web.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Conoscenza di base del sistema operativo (OS) del computer e del software di base che verrà usato per creare un sito web.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Manipolazione di file e cartelle.</li>
          <li>Buone pratiche per la denominazione.</li>
          <li>Struttura standard delle cartelle di un sito web.</li>
          <li>Gestione dei percorsi dei file.</li>
          <li>Gestione delle estensioni dei file.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Manipolare file e cartelle

Esistono molti modi diversi per creare e modificare i file e le cartelle contenuti nel computer. È possibile farlo tramite la riga di comando/terminale del computer usando una serie di comandi testuali, di cui si parlerà più approfonditamente nel prossimo articolo. Tuttavia, molte persone trovano più semplice iniziare a conoscere i file system in modo visivo, ed è ciò di cui parleremo qui. I moderni sistemi operativi (OS) dispongono di una solida interfaccia utente (UI) per il file system, che può essere usata per manipolare file e cartelle secondo necessità.

Su macOS, per esempio, è disponibile il programma Finder:

![L'applicazione Finder di macOS, che mostra il contenuto di una tipica cartella Home](finder.png)

Windows dispone invece di Esplora file:

![L'applicazione Esplora file di Windows, che mostra il contenuto di una tipica cartella Home](file-explorer.png)

> [!NOTE]
> Questa guida è stata scritta usando Windows 11 e macOS 15. Potrebbe essere in uso una versione diversa dell'OS, o un OS completamente diverso, nel qual caso l'esperienza sarà differente. Sul web sono disponibili molte guide sull'uso di base degli OS: si consiglia di cercare sul web informazioni relative al proprio OS specifico.

### Struttura di base

La maggior parte dei sistemi operativi moderni dispone di una cartella `Users`, che contiene una cartella per ogni account utente presente nel sistema, nota anche come cartella _Home_ dell'utente. Questa è generalmente rappresentata da un'icona a forma di casa per renderla più facile da trovare. A sua volta, la cartella _Home_ contiene altre importanti cartelle (e file) standard rilevanti per quello specifico utente, come _Documents_, _Music_ e così via. Nel computer sono presenti anche molti altri file e cartelle, ma per il momento non è necessario preoccuparsi di essi.

Per impostazione predefinita, l'utente attualmente connesso potrà accedere solo alla propria cartella _Home_.

I file dei progetti relativi al proprio lavoro dovrebbero essere creati in una posizione all'interno della cartella _Home_, magari all'interno di _Documents_. Questo ha senso, poiché i file delle pagine web sono spesso chiamati _documenti_.

> [!WARNING]
> Se si iniziano a creare e modificare file in altre parti del sistema, per esempio nelle aree che controllano il sistema operativo o applicazioni importanti, si potrebbe danneggiare qualcosa. Limitarsi a creare e modificare file nella cartella _Home_ finché non si sa cosa si sta facendo.

### Creare una cartella

Creiamo una nuova cartella in cui archiviare tutti i progetti web.

1. Nell'interfaccia utente del file system, fare clic sulla cartella _Home_, quindi fare doppio clic sulla cartella _Documents_.
2. Creare in questa posizione una nuova cartella chiamata `web-projects`:
   1. In Windows, è possibile farlo selezionando il pulsante _Nuovo_ nella finestra di Esplora file e selezionando _Cartella_ (oppure premendo <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>N</kbd>), digitando `web-projects` come nome della nuova icona della cartella visualizzata e premendo <kbd>Invio</kbd>/<kbd>Return</kbd>.
   2. In macOS, è possibile farlo selezionando _File_ > _Nuova cartella_ nel menu Finder (oppure premendo <kbd>Cmd</kbd> + <kbd>Shift</kbd> + <kbd>N</kbd>) — verrà visualizzata una nuova cartella chiamata _cartella senza titolo_. Fare clic sul nome della cartella per iniziare a modificarlo, digitare `web-projects` e premere <kbd>Invio</kbd>/<kbd>Return</kbd>.

In caso di errore di digitazione, è possibile modificare il nome della cartella per correggerlo (funziona anche con i file):

- In Windows, fare clic con il pulsante destro del mouse sulla cartella, selezionare _Rinomina_ dal menu, quindi modificarla. Alcune versioni di Windows mostrano inizialmente un menu semplificato: potrebbe essere necessario fare clic con il pulsante destro del mouse, selezionare _Mostra altre opzioni_, quindi selezionare _Rinomina_.
- In macOS, fare clic sul nome della cartella/selezionarlo per modificarlo.

### Aprire una cartella di progetto e creare file in VS Code

Benché sia possibile creare file di testo nell'interfaccia utente del file system dell'OS, in genere è più semplice e meno soggetto a errori crearli nell'editor di codice. VS Code dispone infatti di un proprio esplora file che consente di creare tutte le cartelle e i file necessari per i progetti web.

Perché, dunque, creare una cartella tramite l'interfaccia utente del file system dell'OS? Perché VS Code deve ricevere una cartella iniziale di primo livello.

È inoltre utile comprendere almeno in parte come è strutturato il file system dell'OS. Questo diventerà più utile quando si inizieranno a usare strumenti più complessi in seguito.

Apriamo ora la cartella `web-projects` in VS Code:

1. Aprire VS Code.
2. Selezionare _File_ > _Open Folder..._ dal menu.
   > [!NOTE]
   > Per chi usa la tastiera, è possibile eseguire il comando _Open Folder_ in Windows tenendo premuto il tasto <kbd>Ctrl</kbd> e premendo <kbd>K</kbd>, quindi <kbd>O</kbd>. Il modo più semplice per gli utenti macOS consiste nell'aprire la _Command Palette_ con <kbd>Cmd</kbd> + <kbd>Shift</kbd> + <kbd>P</kbd>, digitare "Open Folder" per filtrare l'elenco dei comandi, usare i tasti freccia per spostarsi fino a _File: Open Folder_, quindi premere <kbd>Invio</kbd>.
3. Verrà visualizzata una versione ridotta dell'interfaccia utente del file system dell'OS. Usarla per trovare la cartella `web-projects`, selezionarla, quindi premere il pulsante _Select Folder_.
4. Verrà visualizzata una finestra di dialogo intitolata _Do you trust the authors of the files in this folder?_. Leggerla attentamente per comprenderne il significato. Al momento, l'unica persona che creerà file in questa cartella è l'utente stesso, pertanto è possibile fare clic su _Yes, I trust the authors_.

La cartella `web-projects` dovrebbe ora essere aperta nel riquadro _EXPLORER_ di VS Code, come mostrato di seguito:

![Il pannello Explorer di VS Code, che mostra una cartella vuota chiamata web-projects](vs-code-explorer.png)

> [!WARNING]
> Ancora una volta, per il momento assicurarsi di modificare solo i propri file nella cartella _Home_, per evitare di causare problemi al sistema.

#### Una nota sulla navigazione da tastiera in VS Code

VS Code, pur non essendo perfetto, dispone di un ampio insieme di scorciatoie da tastiera. In questo articolo sono state incluse quelle utili dove possibile, ma elenchi più completi sono disponibili nel documento di VS Code [Keyboard Shortcuts Reference](https://code.visualstudio.com/docs/configure/keybindings).

In generale, per navigare in VS Code tramite tastiera, è possibile premere il tasto <kbd>Tab</kbd> per spostarsi tra le diverse aree della UI (<kbd>Shift</kbd> + <kbd>Tab</kbd> sposta alla precedente posizione con focus). Se in una posizione con focus tramite tab sono presenti più pulsanti, è possibile usare i tasti freccia per spostarsi tra essi.

Se si sta modificando un file, il tasto tab non navigherà nella UI: aggiungerà invece caratteri di tabulazione al file. Per uscire dal file in modifica e passare al riquadro _EXPLORER_, è possibile premere <kbd>Cmd</kbd> + <kbd>Shift</kbd> + <kbd>E</kbd> su macOS, oppure <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>E</kbd> su Windows.

Per tornare al riquadro dell'editor di file e iniziare a spostarsi tra i diversi file aperti in schede differenti, tenere premuto il tasto <kbd>Ctrl</kbd> e usare <kbd>Tab</kbd> e <kbd>Shift</kbd> + <kbd>Tab</kbd> per spostarsi su e giù nell'elenco delle schede aperte, sia su macOS sia su Windows. Una volta evidenziato il file da modificare, rilasciare i tasti per passare a quella scheda.

#### Creare un file

Da qui è possibile creare nuovi file e cartelle usando i pulsanti corrispondenti nella parte superiore del riquadro _EXPLORER_.

1. Creare un nuovo file facendo clic sull'icona _New File..._ (oppure spostarsi su di essa con <kbd>Tab</kbd> e premere <kbd>Invio</kbd>/<kbd>Return</kbd>).
2. Inserire il nome del file come "index.html" nella casella di testo visualizzata e premere <kbd>Invio</kbd>/<kbd>Return</kbd>.

> [!NOTE]
> Non usare i pulsanti nella parte superiore della scheda _Welcome_ per creare file e cartelle, poiché funzionano in modo leggermente diverso. È possibile chiudere la scheda _Welcome_, poiché non è necessaria. Per farlo, fare clic sulla "x" sul lato destro della scheda oppure premere <kbd>Cmd</kbd> + <kbd>W</kbd> su macOS (<kbd>Ctrl</kbd> + <kbd>W</kbd> su Windows).

A questo punto, tornare all'interfaccia utente del file system dell'OS, entrare nella cartella `web-projects` facendo doppio clic su di essa e dovrebbe essere visibile anche il file `index.html`. VS Code usa il file system sottostante dell'OS, non un proprio strano file system.

### Spostare index.html nella propria sottocartella

È possibile creare cartelle all'interno di altre cartelle, chiamate _sottocartelle_, con tutti i livelli di profondità desiderati. È anche possibile spostare file e cartelle all'interno di altre cartelle trascinandoli e rilasciandoli sopra la cartella desiderata.

Esploriamo questa funzionalità e, nel processo, spostiamo il file `index.html` nella propria sottocartella. Non è opportuno lasciarlo direttamente nella cartella principale `web-projects`.

1. Creare una nuova cartella all'interno di `web-projects`, usando il pulsante _New Folder..._ nel riquadro _EXPLORER_ di VS Code.
2. Chiamarla `test-site`.
3. Ora dovrebbe essere possibile trascinare il file `index.html` e rilasciarlo sulla cartella `test-site` per spostarlo al suo interno.
   > [!NOTE]
   > Per chi usa la tastiera, è possibile farlo seguendo questi passaggi:
   >
   > 1. Usare i tasti freccia su e giù per spostare il contorno del focus sul file `index.html`.
   > 2. Premere <kbd>Cmd</kbd> + <kbd>X</kbd> su macOS (<kbd>Ctrl</kbd> + <kbd>X</kbd> su Windows) per selezionare il file da spostare.
   > 3. Usare i tasti freccia per spostare il contorno del focus sulla cartella.
   > 4. Premere <kbd>Cmd</kbd> + <kbd>V</kbd> su macOS (<kbd>Ctrl</kbd> + <kbd>V</kbd> su Windows) per spostare il file nella cartella.

Ci sarebbero molte altre informazioni da includere sull'uso delle UI del file system dell'OS e di VS Code, ma lo spazio è limitato, quindi per ora ci fermeremo qui. Sono state fornite informazioni sufficienti per iniziare; si consiglia di cercare sul web informazioni su come eseguire altre operazioni con file e cartelle.

Passiamo ora a una breve discussione sulla struttura di un sito web.

## Quale struttura dovrebbe avere un sito web?

Quando si lavora localmente sui siti web, ovvero sul computer, è necessario mantenere tutti i file correlati di ciascun sito in un'unica cartella. A loro volta, tutte le cartelle dei siti web dovrebbero essere conservate in una cartella centrale, in modo da poterle trovare facilmente.

In precedenza nell'articolo, è stato chiesto di creare una cartella centrale chiamata `web-projects` per archiviare tutti i progetti di siti web. È stata inoltre creata una sottocartella chiamata `test-site` con all'interno un file `index.html` vuoto.

Aggiungiamo altre componenti a `test-site` per dimostrare una struttura tipica di un sito web; nel prossimo modulo verrà creato un esempio di sito web completo al suo interno. Gli elementi più comuni presenti in qualsiasi progetto di sito web sono un file HTML indice e cartelle per immagini, file di stile e file di script:

1. **`index.html`**: questo file conterrà generalmente il contenuto della homepage, ovvero il testo e le immagini che le persone vedono quando visitano per la prima volta il sito.
2. **Cartella `images`**: questa cartella conterrà tutte le immagini usate nel sito.
3. **Cartella `styles`**: questa cartella conterrà il codice CSS usato per definire lo stile dei contenuti, per esempio impostando i colori del testo e dello sfondo.
4. **Cartella `scripts`**: questa cartella conterrà tutto il codice JavaScript usato per aggiungere funzionalità interattive al sito, per esempio definendo cosa accade quando si fa clic sui pulsanti.

Il file `index.html` dovrebbe già trovarsi all'interno di `test-site`. Creare ora al suo interno le cartelle `images`, `styles` e `scripts`.

## Nomi dei file

Un nome file è generalmente composto da due parti: il **nome** e l'**estensione**. Consideriamo il file creato sopra, `index.html`:

- In questo caso, il nome è `index`. I nomi dei file possono generalmente contenere tutti i caratteri desiderati, anche se i diversi sistemi informatici applicano varie restrizioni ai caratteri utilizzabili. È meglio limitarsi a numeri e lettere, almeno all'inizio. Inoltre, i sistemi possono attribuire un significato speciale a determinati nomi o parti dei nomi: come già detto, i file `index` tendono a essere riconosciuti come file della homepage principale di un sito web.
- L'estensione del file identifica il tipo di file e viene usata dai sistemi informatici per identificare il tipo di contenuto previsto nel file, il programma da usare per aprirlo e così via. In questo caso, l'estensione è `.html`, che significa che il file dovrebbe contenere testo semplice e, più specificamente, codice HTML. Grazie all'estensione, il computer sa che, quando si prova ad aprire il file, deve usare l'editor di testo predefinito, che dovrebbe essere VS Code se sono state seguite tutte le istruzioni fino a questo punto.

Non è così in tutti i casi, ma la maggior parte dei file necessita di un'estensione per essere gestita correttamente. Rimuovere o modificare l'estensione di un file può causare errori, quindi non dovrebbe essere alterata a meno di sapere realmente cosa si sta facendo.

> [!NOTE]
> È possibile inserire più di un punto nel nome di un file, per esempio `my.cats.html`. In questi casi, si presume che l'ultimo punto indichi l'inizio dell'estensione del file.

Nei computer Windows, potrebbe essere difficile visualizzare le estensioni di alcuni file perché Windows dispone di un'opzione chiamata **Nascondi le estensioni per i tipi di file conosciuti** attivata per impostazione predefinita. È possibile disattivarla accedendo a Esplora file, selezionando l'opzione **Opzioni cartella…**, deselezionando la casella di controllo **Nascondi le estensioni per i tipi di file conosciuti**, quindi facendo clic su **OK**. Per informazioni più specifiche relative alla propria versione di Windows, è possibile eseguire una ricerca sul web.

### Buone pratiche per la denominazione dei file

Seguendo questo corso, si noterà che viene sempre chiesto di assegnare a cartelle e file nomi interamente in minuscolo, senza spazi. Ignorare questo consiglio può causare problemi in molti modi; alcuni dei più comuni sono i seguenti:

1. Molti sistemi informatici, inclusa la maggior parte dei server web, distinguono tra maiuscole e minuscole. Per esempio, se un'immagine viene inserita nel sito web in `test-site/images/MyImage.jpg` e poi, in un file diverso, si prova a fare riferimento all'immagine con `test-site/images/myimage.jpg`, potrebbe non funzionare.
2. Quando si invocano comandi nella riga di comando, è necessario racchiudere tra virgolette i nomi dei file contenenti spazi; altrimenti, verranno interpretati come due elementi separati.
3. Alcuni linguaggi di programmazione, per esempio Python, non funzionano bene con gli spazi nei nomi dei file in determinate circostanze, ad esempio se questi file sono moduli da importare.
4. I nomi dei file vengono comunemente associati ad indirizzi web/URL. Se, per esempio, nella cartella radice del server è presente un file chiamato <code>my&nbsp;file.html</code>, generalmente sarà accessibile a un URL come `https://example.com/my%20file.html`. I server web di solito sostituiscono gli spazi nei nomi dei file con `%20` (perché gli URL usano la {{Glossary("Percent-encoding", "percent-encoding")}}), il che può creare bug sottili con alcuni sistemi se questi presumono che i nomi dei file e gli URL corrispondano perfettamente.

Al posto degli spazi, molti sviluppatori usano un carattere separatore, come un trattino (`-`), anziché uno spazio; per esempio `my-file.html` invece di <code>my&nbsp;file.html</code>. Questa è una buona pratica.

È preferibile abituarsi a scrivere i nomi delle cartelle e dei file in minuscolo, senza spazi e con le parole separate da trattini, almeno finché non si sa cosa si sta facendo. In questo modo, si incontreranno meno problemi in futuro.

> [!NOTE]
> Ulteriori buone pratiche per i nomi di file e gli URL sono disponibili in [URL structure best practices for Google](https://developers.google.com/search/docs/crawling-indexing/url-structure).

## Percorsi dei file

Per fare riferimento a un file da un altro, è necessario fornire un percorso del file: in pratica, un itinerario che consente a un file di sapere dove si trova un altro. Per esempio, quando si crea una pagina web contenente un'immagine, il codice della pagina web deve contenere un percorso del file che indichi la posizione dell'immagine da visualizzare.

Esaminiamo un esempio di base. Per il momento potrebbe non essere chiaro il significato di tutto questo, ma va bene così.

1. Cercare sul web un'immagine di proprio gradimento, per esempio usando un servizio come [Google Images](https://www.google.com/imghp), e scaricarla. In alternativa, è possibile usare la nostra [immagine dell'icona di Firefox](https://raw.githubusercontent.com/mdn/beginner-html-site/refs/heads/main/images/firefox-icon.png) per questo esempio.
2. Inserire l'immagine nella cartella _images_.
3. Assicurarsi che il file dell'immagine abbia un nome breve e semplice, senza spazi. Per esempio, `firefox-icon.png` va bene e `cat.jpg` va bene, ma `efregre^%^£$£@%$^&YTJgfbgfdgt54656756_ertgrth-rtgtfghhyj.png` non va bene. Assicurarsi inoltre di mantenere l'estensione del file.

Ora verrà aggiunto contenuto al file `index.html` per consentirgli di individuare e visualizzare il file dell'immagine.

1. Aprire `index.html` in VS Code e inserire nel file il seguente contenuto esattamente come mostrato di seguito. Si tratta di HTML, il linguaggio usato per definire e strutturare il contenuto delle pagine web. Se ne apprenderà molto di più a breve.

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

2. La riga `<img src="" alt="My test image">` è il codice HTML che inserisce un'immagine nella pagina. È necessario indicare all'HTML dove si trova l'immagine. L'immagine si trova nella cartella _images_, che è nella stessa cartella di `index.html`. Per percorrere la struttura dei file da `index.html` all'immagine, il percorso del file necessario è `images/your-image-filename`. Per esempio, se l'immagine si chiamasse `firefox-icon.png`, il percorso del file sarebbe `images/firefox-icon.png`.
3. Inserire il percorso del file nel codice HTML tra le virgolette doppie di `src=""`.
4. Salvare il file HTML, quindi caricarlo nel browser web. È possibile farlo facendo <kbd>Ctrl</kbd>+clic/clic con il pulsante destro del mouse sul file HTML, quindi scegliendo _Open With_ e selezionando un browser web dal sottomenu risultante. In alternativa, è possibile aprire l'interfaccia utente del file system e una finestra del browser web nella stessa schermata, quindi trascinare e rilasciare il file HTML sopra la finestra del browser web.

Dovrebbe essere visualizzata una pagina web di base che mostra l'immagine.

![Uno screenshot del nostro sito web di base che mostra solo il logo di Firefox, una volpe fiammeggiante che avvolge il mondo](website-screenshot.png)

### Regole generali per i percorsi dei file

- Per collegarsi a un file di destinazione nella stessa cartella del file HTML che lo invoca, usare semplicemente il nome del file, per esempio `my-image.jpg`.
- Per fare riferimento a un file in una sottocartella, scrivere il nome della cartella davanti al percorso, seguito da una barra, per esempio `subfolder/my-image.jpg`.
- Per collegarsi a un file di destinazione nella cartella **sopra** il file HTML che lo invoca, scrivere due punti. Per esempio, se `index.html` si trovasse in una sottocartella di `test-site` e `my-image.jpg` si trovasse in `test-site`, sarebbe possibile fare riferimento a `my-image.jpg` da `index.html` usando `../my-image.jpg`.
- È possibile combinare questi elementi a piacere, per esempio `../subfolder/another-subfolder/my-image.jpg`.

> [!NOTE]
> Il file system di Windows tende a usare barre rovesciate anziché barre normali, per esempio `C:\Windows`. Questo non conta in HTML: anche sviluppando il sito web su Windows, nel codice è necessario usare comunque barre normali.

{{PreviousMenuNext("Learn_web_development/Getting_started/Environment_setup/Code_editors", "Learn_web_development/Getting_started/Environment_setup/Command_line", "Learn_web_development/Getting_started/Environment_setup")}}
