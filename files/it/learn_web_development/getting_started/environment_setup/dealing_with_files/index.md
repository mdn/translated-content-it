---
title: Come gestire i file
slug: Learn_web_development/Getting_started/Environment_setup/Dealing_with_files
l10n:
  sourceCommit: be1922d62a0d31e4e3441db0e943aed8df736481
---

{{PreviousMenuNext("Learn_web_development/Getting_started/Environment_setup/Code_editors", "Learn_web_development/Getting_started/Environment_setup/Command_line", "Learn_web_development/Getting_started/Environment_setup")}}

Un sito web è composto da molti file: contenuti di testo, codice, fogli di stile, contenuti multimediali, e così via. Quando si costruisce un sito web, è necessario assemblare questi file in una struttura sensata sul computer locale, assicurarsi che possano comunicare tra loro e fare in modo che tutti i contenuti abbiano un aspetto corretto prima di caricarli su un server affinché il mondo li possa vedere. Questo articolo spiega come utilizzare l'interfaccia utente (UI) del file explorer del computer e impostare una struttura di file sensata per un sito web.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con il sistema operativo (OS) del proprio computer e con il software di base utilizzato per costruire un sito web.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Manipolazione di file e cartelle.</li>
          <li>Best practice per la nomenclatura.</li>
          <li>Struttura standard delle cartelle di un sito web.</li>
          <li>Gestione dei percorsi dei file.</li>
          <li>Gestione delle estensioni dei file.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Manipolazione di file e cartelle

Esistono molti modi diversi per creare e modificare i file e le cartelle contenuti sul computer. È possibile farlo tramite la riga di comando/terminale del computer utilizzando una serie di comandi testuali, di cui imparerai di più nel prossimo articolo. Tuttavia, molte persone trovano più facile iniziare a comprendere i sistemi di file in modo visivo, ed è di questo che parleremo qui. I sistemi operativi moderni (OSes) hanno un'interfaccia utente (UI) robusta per il sistema di file che puoi utilizzare per manipolare i file e le cartelle secondo necessità.

Su macOS, ad esempio, hai il programma Finder:

![L'applicazione Finder di macOS, che mostra il contenuto di una tipica cartella Home](finder.png)

Mentre Windows ha il File Explorer:

![L'applicazione File Explorer di Windows, che mostra il contenuto di una tipica cartella Home](file-explorer.png)

> [!NOTE]
> Questa guida è stata scritta utilizzando Windows 11 e macOS 15. Potresti utilizzare una versione diversa del sistema operativo, o un altro completamente diverso, nel qual caso l'esperienza potrebbe differire leggermente. Ci sono molte guide sul web sull'uso di base dei sistemi operativi - incoraggiamo a cercare informazioni specifiche sul tuo sistema operativo.

### Struttura base

La maggior parte dei moderni sistemi operativi ha una cartella `Users`, che contiene una cartella per ciascun account utente esistente sul sistema, nota anche come cartella _Home_ dell'utente. Questa è di solito rappresentata da un'icona a forma di casa per facilitarne l'individuazione. A sua volta, la cartella _Home_ conterrà altre cartelle standard importanti (e file) rilevanti per quell'utente in particolare, come _Documents_, _Music_, ecc. Ci sono molti altri file e cartelle sul computer, ma non preoccuparti di quelli per ora.

L'utente attualmente connesso potrà accedere solo alla propria cartella _Home_ per impostazione predefinita.

Dovresti creare i file di progetto relativi al tuo lavoro in qualche posizione all'interno della tua cartella _Home_, forse all'interno di _Documents_. Questo ha senso, dato che i file delle pagine web sono spesso indicati come _documenti_.

> [!WARNING]
> Se inizi a creare e modificare file in altre aree del sistema (ad esempio, aree che controllano il sistema operativo o applicazioni importanti), potresti rompere qualcosa. Per il momento, concentrati nel creare e modificare file all'interno della tua cartella _Home_.

### Creazione di una cartella

Creiamo una nuova cartella per memorizzare tutti i nostri progetti web.

1. Nell'interfaccia utente del sistema di file, clicca sulla tua cartella _Home_, poi fai doppio clic sulla cartella _Documents_.
2. Crea una nuova cartella in questa posizione chiamata `web-projects`:
   1. Su Windows, questo può essere fatto selezionando il pulsante _Nuovo_ nella finestra del File Explorer e selezionando _Folder_ (o premendo <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>N</kbd>), digitando `web-projects` come nome della nuova icona di cartella che appare e premendo <kbd>Enter</kbd>/<kbd>Return</kbd>.
   2. Su macOS, questo può essere fatto selezionando _File_ > _Nuova cartella_ nel menu del Finder (o premendo <kbd>Cmd</kbd> + <kbd>Shift</kbd> + <kbd>N</kbd>) — vedrai apparire una nuova cartella chiamata _untitled folder_. Clicca sul nome della cartella per iniziare a modificarlo, digita `web-projects` e premi <kbd>Enter</kbd>/<kbd>Return</kbd>.

Se commetti un errore di battitura, puoi modificare il nome della cartella per correggerlo (questo vale anche per i file):

- Su Windows, fai clic con il tasto destro sulla cartella, seleziona _Rinomina_ dal menu, poi modificala. Alcune versioni di Windows mostrano inizialmente un menu semplificato — potresti dover fare clic con il tasto destro, poi selezionare _Mostra più opzioni_, poi selezionare _Rinomina_!
- Su macOS, fai clic/seleziona il nome della cartella per modificarlo.

### Apertura di una cartella di progetto e creazione di file in VS Code

Sebbene sia possibile creare file di testo all'interno dell'interfaccia utente del sistema operativo, in generale è più facile e meno soggetto a errori crearli all'interno del tuo editor di codice. Infatti, VS Code ha il proprio file explorer che ti permette di creare tutte le cartelle e i file di cui hai bisogno per i tuoi progetti web.

Quindi perché ti abbiamo fatto creare una cartella utilizzando l'interfaccia utente del sistema operativo? Perché VS Code deve essere indicato inizialmente verso una cartella di livello superiore!

È anche utile comprendere un po' di come è strutturato il sistema di file del tuo sistema operativo. Questo diventerà più utile man mano che inizi a usare strumenti più complessi.

Apriamo ora la cartella `web-projects` in VS Code:

1. Apri VS Code.
2. Seleziona _File_ > _Apri cartella..._ dal menu.
   > [!NOTE]
   > Se sei un utente da tastiera, puoi eseguire il comando _Apri cartella_ su Windows tenendo premuto il tasto <kbd>Ctrl</kbd> e premendo <kbd>K</kbd> poi <kbd>O</kbd>. Il modo più semplice per un utente macOS di fare ciò è aprire il _Command Palette_ con <kbd>Cmd</kbd> + <kbd>Shift</kbd> + <kbd>P</kbd>, digitare "Apri cartella" per filtrare l'elenco dei comandi, utilizzare i tasti cursore per spostarsi verso il basso fino a _File: Apri cartella_, quindi premere <kbd>Enter</kbd>.
3. Apparirà una versione ridotta dell'interfaccia utente del sistema di file del sistema operativo. Usala per trovare la tua cartella `web-projects`, selezionala, poi premi il pulsante _Seleziona cartella_.
4. Ti verrà presentato un dialogo intitolato _Ti fidi degli autori dei file in questa cartella?_ Leggilo attentamente per capire di cosa si tratta. Al momento, sei l'unica persona che creerà file in questa cartella, quindi puoi cliccare _Sì, mi fido degli autori_.

Dovresti vedere la tua cartella `web-projects` aperta nel pannello _EXPLORER_ di VS Code, come mostrato sotto:

![Il pannello Explorer di VS Code, mostrando una cartella vuota chiamata web-projects](vs-code-explorer.png)

> [!WARNING]
> Di nuovo, assicurati di concentrarti sull'editing dei tuoi file all'interno della cartella _Home_, per evitare di causare problemi al tuo sistema.

#### Un approfondimento sulla navigazione da tastiera in VS Code

VS Code, pur non essendo perfetto, ha un ampio set di scorciatoie da tastiera. In tutto questo articolo abbiamo cercato di includere le più utili dove possibile, ma puoi trovare elenchi più completi nel [Riferimento delle scorciatoie da tastiera di VS Code](https://code.visualstudio.com/docs/configure/keybindings).

In generale, se desideri navigare all'interno di VS Code tramite tastiera, puoi premere il tasto <kbd>Tab</kbd> per spostarti nelle diverse aree dell'interfaccia utente (<kbd>Shift</kbd> + <kbd>Tab</kbd> ti porterà alla posizione del focus precedente). Se ci sono più pulsanti in una posizione di focus della tab, puoi usare i tasti cursore per muoverti tra di essi.

Se stai attualmente modificando un file, il tasto tab non navigherà all'interno dell'interfaccia utente — aggiungerà i caratteri tab nel file. Per spostarti dal file che stai modificando al pannello _EXPLORER_, puoi premere <kbd>Cmd</kbd> + <kbd>Shift</kbd> + <kbd>E</kbd> su macOS, o <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>E</kbd> su Windows.

Per tornare al pannello dell'editor di file e iniziare a muoverti tra i diversi file aperti in schede diverse, tieni premuto il tasto <kbd>Ctrl</kbd> e usa <kbd>Tab</kbd> e <kbd>Shift</kbd> + <kbd>Tab</kbd> per muoverti su e giù nella lista delle schede aperte (sia su macOS che su Windows). Una volta che hai evidenziato il file che desideri modificare, rilascia i tasti per passare a quella scheda.

#### Creare un file

Da qui, puoi creare nuovi file e cartelle utilizzando i pulsanti pertinenti nella parte superiore del pannello _EXPLORER_.

1. Crea un nuovo file facendo clic sull'icona _Nuovo file..._ (o <kbd>Tab</kbd> per selezionarla e premi <kbd>Enter</kbd>/<kbd>Return</kbd>).
2. Inserisci il nome del file come "index.html" nella casella di testo che appare e premi <kbd>Enter</kbd>/<kbd>Return</kbd>.

> [!NOTE]
> Non utilizzare i pulsanti nella parte superiore della scheda _Welcome_ per creare file e cartelle, poiché funzionano in modo leggermente diverso. Infatti, puoi chiudere la scheda _Welcome_, poiché non ne hai bisogno. Fai questo cliccando la "x" sul lato destro della scheda, o premendo <kbd>Cmd</kbd> + <kbd>W</kbd> su macOS (<kbd>Ctrl</kbd> + <kbd>W</kbd> su Windows).

A questo punto, torna all'interfaccia utente del sistema di file del tuo sistema operativo, vai nella tua cartella `web-projects` facendo doppio clic su di essa e dovresti vedere il tuo file `index.html` lì dentro. VS Code utilizza il sistema di file sottostante del sistema operativo, non utilizza un sistema di file strano di propria creazione.

### Spostare index.html nella sua cartella secondaria

Puoi creare delle cartelle all'interno di altre cartelle (chiamate _sotto-cartelle_) per quante sue vuoi. Puoi anche spostare file (e cartelle) all'interno di altre cartelle trascinandoli e rilasciandoli sopra quella cartella.

Esploriamo questo aspetto e nel processo, spostiamo il nostro file `index.html` all'interno della sua sotto-cartella. Non vogliamo davvero che sia all'interno della cartella principale `web-projects`.

1. Crea una nuova cartella all'interno di `web-projects`, utilizzando il pulsante _Nuova cartella..._ del pannello _EXPLORER_ di VS Code.
2. Nominala `test-site`.
3. Ora dovresti essere in grado di trascinare il file `index.html` e rilasciarlo sopra la cartella `test-site` per spostare il file all'interno della cartella.
   > [!NOTE]
   > Se sei un utilizzatore della tastiera, puoi farlo seguendo questi passaggi:
   >
   > 1. Usa i tasti freccia su e giù per spostare il contorno di focus sul file `index.html`.
   > 2. Premi <kbd>Cmd</kbd> + <kbd>X</kbd> su macOS (<kbd>Ctrl</kbd> + <kbd>X</kbd> su Windows) per selezionare il file per lo spostamento.
   > 3. Usa i tasti freccia per spostare il contorno di focus sulla cartella.
   > 4. Premi <kbd>Cmd</kbd> + <kbd>V</kbd> su macOS (<kbd>Ctrl</kbd> + <kbd>V</kbd> su Windows) per spostare il file in quella cartella.

Ci sarebbe molto altro da includere su come usare le UI dei sistemi di file del sistema operativo e VS Code, ma abbiamo spazio limitato, quindi ci fermeremo qui per ora. Ti abbiamo fornito abbastanza informazioni per iniziare, e ti incoraggiamo a cercare sul web informazioni su come fare altre cose con file e cartelle.

Passiamo ad una breve discussione sulla struttura di un sito web.

## Quale struttura dovrebbe avere un sito web?

Quando lavori localmente (sul tuo computer) su siti web, dovresti mantenere tutti i file correlati a ciascun sito in una singola cartella. A sua volta, dovresti mantenere tutte le tue cartelle di siti web in una cartella centrale unica, in modo che siano facili da trovare.

All'inizio dell'articolo, ti abbiamo istruito a creare una cartella centrale chiamata `web-projects` per memorizzare tutti i progetti dei tuoi siti web. Ti abbiamo anche fatto creare una sottocartella chiamata `test-site` con un file `index.html` vuoto al suo interno.

Aggiungiamo alcune caratteristiche all'interno di `test-site` per dimostrare una tipica struttura di un sito web; nel prossimo modulo, ti faremo costruire un esempio di sito web completo al suo interno. Le cose più comuni che ogni progetto di sito web conterrà sono un file HTML index e cartelle per contenere immagini, file di stile e file di script:

1. **`index.html`**: Questo file conterrà generalmente il contenuto della tua homepage, cioè il testo e le immagini che le persone vedono quando visitano per la prima volta il tuo sito.
2. **cartella `images`**: Questa cartella conterrà tutte le immagini che utilizzi nel tuo sito.
3. **cartella `styles`**: Questa cartella conterrà il codice CSS usato per stilizzare i tuoi contenuti (ad esempio, impostare i colori del testo e di sfondo).
4. **cartella `scripts`**: Questa cartella conterrà tutto il codice JavaScript utilizzato per aggiungere funzionalità interattive al tuo sito (ad esempio, definire cosa succede quando si cliccano i pulsanti).

> [!CALLOUT]
>
> **Provalo**
>
> Dovresti già avere un file `index.html` all'interno di `test-site`. Crea ora le cartelle `images`, `styles` e `scripts` al suo interno.

## Nomi dei file

Generalmente ci sono due parti nel nome di un file — il **nome** e l'**estensione**. Prendiamo il file creato sopra — `index.html`:

- Il nome in questo caso è `index`. I nomi dei file possono generalmente contenere qualsiasi carattere tu voglia, anche se diversi sistemi informatici avranno diverse limitazioni sui caratteri utilizzabili. È meglio limitarsi a numeri e lettere, almeno all'inizio. Inoltre, i sistemi possono dare significati speciali a determinati nomi o parti di nomi — come abbiamo già detto, i file `index` tendono a essere riconosciuti come il principale file di homepage di un sito web.
- L'estensione del file identifica il tipo di file con cui abbiamo a che fare, ed è utilizzata dai sistemi informatici per identificare che tipo di contenuto aspettarsi nel file, quale programma dovrebbe usare per aprire il file, ecc. in questo caso, l'estensione è `.html`, il che significa che il file dovrebbe contenere testo semplice e, più specificamente, codice HTML. A causa dell'estensione, il tuo computer sa che quando cerchi di aprire il file dovrebbe aprirlo usando l'editor di testo predefinito, che dovrebbe essere VS Code se hai seguito tutte le nostre istruzioni finora.

Non è vero in tutti i casi, ma la maggior parte dei file necessita di un'estensione per essere gestita correttamente. Rimuovere o cambiare l'estensione del file è probabile che causi errori, quindi non dovresti modificarla a meno che tu non sappia davvero cosa stai facendo.

> [!NOTE]
> È possibile mettere più di un punto in un nome di file, per esempio `my.cats.html`. In tali casi, si presume che l'ultimo punto sia l'inizio dell'estensione del file.

Sui computer Windows, potresti avere problemi a vedere le estensioni di alcuni file, perché Windows ha un'opzione chiamata **Nascondi estensioni per tipi di file conosciuti** attivata di default. Puoi disattivarla andando su File Explorer, selezionando l'opzione **Opzioni cartella...**, deselezionando la casella di controllo **Nascondi estensioni per tipi di file conosciuti**, quindi cliccando su **OK**. Per informazioni più specifiche sulla tua versione di Windows, puoi cercare sul web.

### Best practice per la nomenclatura dei file

Seguendo questo corso, noterai che ti chiederemo sempre di nominare cartelle e file completamente in minuscolo senza spazi. Esistono molti modi in cui l'uso di spazi nei nomi di file e cartelle crea problemi — alcuni dei più comuni sono i seguenti:

<!-- cSpell:ignore myimage -->

1. Molti sistemi informatici, inclusi la maggior parte dei server web, fanno distinzione tra maiuscole e minuscole. Quindi, per esempio, se metti un'immagine sul tuo sito web in `test-site/images/MyImage.jpg` e poi in un altro file cerchi di fare riferimento all'immagine con `test-site/images/myimage.jpg`, potrebbe non funzionare.
2. Quando invochi comandi sulla riga di comando, devi mettere le virgolette attorno ai nomi dei file con spazi al loro interno, altrimenti verranno interpretati come due elementi separati.
3. Alcuni linguaggi di programmazione (come Python) non funzionano bene con spazi nei nomi di file in alcune circostanze (ad esempio, se questi file sono moduli da importare).
4. I nomi dei file generalmente corrispondono agli indirizzi web/URL. Se, per esempio, hai un file chiamato `my file.html` nella cartella radice del tuo server, generalmente sarà accessibile a un URL come `https://example.com/my%20file.html`. I server web solitamente sostituiscono gli spazi nei nomi dei file con `%20` (perché gli URL sono {{Glossary("Percent-encoding", "percentualmente codificati")}}), il che può creare bug sottili con alcuni sistemi se assumono che i nomi di file e gli URL combacino perfettamente.

Invece degli spazi, molti sviluppatori utilizzano un carattere separatore come un trattino (`-`) anziché uno spazio — ad esempio `my-file.html` anziché `my file.html`. Questa è una buona pratica.

È meglio abituarsi a scrivere i nomi delle cartelle e dei file in minuscolo senza spazi e con le parole separate da trattini, almeno finché non si sa cosa si sta facendo. In questo modo, incontrerai meno problemi in futuro.

> [!NOTE]
> Puoi trovare ulteriori best practice per i nomi di file e URL nelle [Best practices per la struttura degli URL di Google](https://developers.google.com/search/docs/crawling-indexing/url-structure).

## Percorsi dei file

Per fare riferimento a un file da un altro, devi fornire un percorso del file — fondamentalmente una rotta, in modo che un file sappia dove si trova un altro file. Per esempio, quando si crea una pagina web che contiene un'immagine, il codice della pagina web dovrà contenere un percorso di file che indica la posizione dell'immagine che si desidera visualizzare.

Esaminiamo un esempio di base di questo. Potresti non capire cosa significhi tutto questo per ora, ma va bene.

1. Cerca sul web un'immagine che ti piace (ad esempio, utilizzando un servizio come [Google Immagini](https://www.google.com/imghp)) e scaricala. In alternativa, puoi semplicemente prendere la nostra [immagine dell'icona di Firefox](https://raw.githubusercontent.com/mdn/beginner-html-site/refs/heads/main/images/firefox-icon.png) da utilizzare per questo esempio.
2. Metti l'immagine all'interno della tua cartella _images_.
3. Assicurati che il file immagine si chiami qualcosa di breve e semplice, senza spazi al suo interno. Ad esempio, `firefox-icon.png` è buono, e `cat.jpg` è buono, ma `efregre^%^£$£@%$^&YTJgfbgfdgt54656756_ertgrth-rtgtfghhyj.png` non è buono. Assicurati anche di mantenere l'estensione del file.

Ora aggiungeremo contenuto al file `index.html` per permettergli di individuare il file immagine e visualizzarlo.

1. Apri il tuo `index.html` in VS Code e inserisci il seguente contenuto nel file esattamente come mostrato sotto. Questo è HTML, il linguaggio che usiamo per definire e strutturare il contenuto delle pagine web. Imparerai molto di più su questo molto presto!

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

2. La riga `<img src="" alt="My test image">` è il codice HTML che inserisce un'immagine nella pagina. Dobbiamo dire all'HTML dove si trova l'immagine. L'immagine è all'interno della cartella _images_, che si trova nella stessa cartella di `index.html`. Per scendere nella struttura dei file da `index.html` alla nostra immagine, il percorso del file necessario è `images/nome-tuo-file-immagine`. Ad esempio, se la tua immagine si chiama `firefox-icon.png`, il percorso del file sarebbe `images/firefox-icon.png`.
3. Inserisci il percorso del file nel tuo codice HTML tra le virgolette doppie di `src=""`.
4. Salva il tuo file HTML, poi caricalo nel tuo browser web. Puoi farlo facendo <kbd>Ctrl</kbd>/clic destro sul file HTML, poi scegliendo _Apri con_ e selezionando un browser web dal sottomenu risultante. Potresti anche aprire l'interfaccia utente del sistema di file e una finestra del browser web sullo stesso schermo, e trascinare e rilasciare il file HTML sulla finestra del browser web.

Dovresti vedere una pagina web di base che visualizza la tua immagine!

![Uno screenshot del nostro sito web di base che mostra solo il logo di Firefox - una volpe fiammeggiante che avvolge il mondo](website-screenshot.png)

### Regole generali per i percorsi dei file

- Per collegarti a un file di destinazione nella stessa cartella del file HTML di invocazione, usa solo il nome del file, ad esempio `my-image.jpg`.
- Per fare riferimento a un file in una sottocartella, scrivi il nome della cartella davanti al percorso, più una barra in avanti, ad esempio `subfolder/my-image.jpg`.
- Per collegarti a un file di destinazione nella cartella **sopra** al file HTML di invocazione, scrivi due punti. Ad esempio, se `index.html` era all'interno di una sottocartella di `test-site` e `my-image.jpg` era all'interno di `test-site`, potresti fare riferimento a `my-image.jpg` da `index.html` usando `../my-image.jpg`.
- Puoi combinare questi come vuoi, per esempio `../subfolder/another-subfolder/my-image.jpg`.

> [!NOTE]
> Il sistema di file di Windows tende a usare barre all'indietro, non barre in avanti, es., `C:\Windows`. Questo non importa in HTML — anche se stai sviluppando il tuo sito web su Windows, dovresti comunque usare barre in avanti nel tuo codice.

{{PreviousMenuNext("Learn_web_development/Getting_started/Environment_setup/Code_editors", "Learn_web_development/Getting_started/Environment_setup/Command_line", "Learn_web_development/Getting_started/Environment_setup")}}
