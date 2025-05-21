---
title: Creare collegamenti
slug: Learn_web_development/Core/Structuring_content/Creating_links
l10n:
  sourceCommit: e488eba036b2fee56444fd579c3759ef45ff2ca8
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Advanced_text_features", "Learn_web_development/Core/Structuring_content/Marking_up_a_letter", "Learn_web_development/Core/Structuring_content")}}

I link (conosciuti anche come hyperlink) sono veramente importanti — sono ciò che rende il Web _una rete_.
Questo articolo mostra la sintassi richiesta per creare un link e discute le migliori pratiche relative ai link.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con le nozioni di base di HTML, come trattato in
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi di base di HTML</a
        >. Semantica a livello di testo come <a href="/it/docs/Learn_web_development/Core/Structuring_content/Headings_and_paragraphs"
          >titoli e paragrafi</a
        > e <a href="/it/docs/Learn_web_development/Core/Structuring_content/Lists"
          >liste</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere perché i link sono la caratteristica fondamentale del web. Non c'è web senza link.</li>
          <li>L'attributo <code>href</code>.</li>
          <li>Percorsi assoluti e relativi e quando utilizzarli.</li>
          <li>Sintassi dei percorsi in dettaglio — slash, singolo punto e doppio punto.</li>
          <li>Stati del link e perché sono importanti — <code>:hover</code>, <code>:focus</code>, <code>:visited</code> e <code>:active</code>.</li>
          <li>Collegamenti a livello inline e block.</li>
          <li>Comprendere i vantaggi di scrivere un buon testo per i link, come una migliore accessibilità per gli utenti di screen reader e possibili effetti positivi per la SEO.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Che cos'è un hyperlink?

Gli hyperlink sono una delle innovazioni più entusiasmanti che il Web ha da offrire.
Sono una caratteristica del Web fin dall'inizio e sono ciò che rende il Web _una rete._
Gli hyperlink ci consentono di collegare documenti ad altri documenti o risorse, collegarsi a parti specifiche dei documenti o rendere le app disponibili a un indirizzo web.
Quasi qualsiasi contenuto web può essere convertito in un link in modo che, al clic o in un altro modo attivato, il browser web si sposti a un altro indirizzo web ({{Glossary("URL", "URL")}}).

> [!NOTE]
> Un URL può puntare a file HTML, file di testo, immagini, documenti di testo, file video e audio o qualsiasi altra cosa che vive sul Web.
> Se il browser web non sa come visualizzare o gestire il file, ti chiederà se vuoi aprire il file (in tal caso il compito di aprire o gestire il file viene passato a un'app nativa adatta sul dispositivo) o scaricare il file (in tal caso puoi provare a gestirlo successivamente).

Ad esempio, la homepage della BBC contiene molti link che puntano non solo a più storie di cronaca, ma anche a diverse aree del sito (funzionalità di navigazione), pagine di login/registrazione (strumenti per l'utente) e altro ancora.

![pagina iniziale di bbc.co.uk, che mostra molti articoli di cronaca e la funzionalità del menu di navigazione](updated-bbc-website.png)

## Anatomia di un link

Un link di base viene creato racchiudendo il testo o altro contenuto all'interno di un elemento {{htmlelement("a")}} e utilizzando l'attributo [`href`](/it/docs/Web/HTML/Reference/Elements/a#href), noto anche come **Hypertext Reference**, o **target**, che contiene l'indirizzo web.

```html
<p>
  I'm creating a link to
  <a href="https://www.mozilla.org/en-US/">the Mozilla homepage</a>.
</p>
```

Questo ci dà il seguente risultato:

Sto creando un link alla [homepage di Mozilla](https://www.mozilla.org/en-US/).

### Link a livello di blocco

Come menzionato prima, quasi qualsiasi contenuto può essere trasformato in un link, anche gli {{Glossary("Block/CSS", "elementi a livello di blocco")}}.
Se vuoi trasformare un elemento intestazione in un link, racchiudilo in un elemento ancora (`<a>`) come mostrato nel seguente snippet di codice:

```html
<a href="https://developer.mozilla.org/en-US/">
  <h1>MDN Web Docs</h1>
</a>
<p>
  Documenting web technologies, including CSS, HTML, and JavaScript, since 2005.
</p>
```

Questo trasforma l'intestazione in un link:
{{EmbedLiveSample('Block level links', '100%', 150)}}

### Link immagine

Per trasformare un'immagine in un link, racchiudi l'elemento {{htmlelement("img")}} con un elemento {{htmlelement("a")}}. L'esempio qui sotto utilizza un percorso relativo per fare riferimento a un file immagine SVG memorizzato localmente.

```css hidden
img {
  height: 100px;
  width: 150px;
  border: 1px solid gray;
}
```

```html
<a href="https://developer.mozilla.org/en-US/">
  <img src="mdn_logo.svg" alt="MDN Web Docs" />
</a>
```

Questo rende il logo MDN un link:
{{EmbedLiveSample('Image links', '100%', 150)}}

> [!NOTE]
> Scoprirai di più sull'uso delle immagini sul Web in un articolo futuro.

### Aggiungere informazioni di supporto con l'attributo "title"

Un altro attributo che potresti voler aggiungere ai tuoi link è `title`.
Il title contiene informazioni aggiuntive sul link, come quale tipo di informazioni contiene la pagina o cose di cui essere consapevoli sul sito web.

```html-nolint
<p>
  I'm creating a link to
  <a
    href="https://www.mozilla.org/en-US/"
    title="The best place to find more information about Mozilla's
          mission and how to contribute">
    the Mozilla homepage</a>.
</p>
```

Questo ci dà il seguente risultato e spostando il mouse sul link viene visualizzato il titolo come tooltip:

{{EmbedLiveSample('Adding supporting information with the title attribute', '100%', 150)}}

> [!NOTE]
> Un titolo del link viene rivelato solo al passaggio del mouse, il che significa che le persone che si affidano ai controlli della tastiera o ai touchscreen per navigare nelle pagine web avranno difficoltà ad accedere alle informazioni del titolo.
> Se le informazioni di un titolo sono davvero importanti per l'usabilità della pagina, allora dovresti presentarle in un modo che sia accessibile a tutti gli utenti, ad esempio inserendole nel testo normale.

### Apprendimento attivo: creare il tuo esempio di link

Crea un documento HTML utilizzando il tuo editor di codice locale e il nostro [modello di inizio](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/getting-started/index.html).

- All'interno del corpo HTML, aggiungi uno o più paragrafi o altri tipi di contenuto che già conosci.
- Trasforma parte del contenuto in link.
- Includi gli attributi title.

## Una rapida introduzione agli URL e ai percorsi

Per comprendere appieno i target dei link, devi comprendere gli URL e i percorsi dei file. Questa sezione fornisce le informazioni necessarie per ottenere questo.

Un URL, o Uniform Resource Locator, è una stringa di testo che definisce dove si trova qualcosa sul Web. Ad esempio, la homepage in inglese di Mozilla si trova a `https://www.mozilla.org/en-US/`.

Gli URL utilizzano i percorsi per trovare i file. I percorsi specificano dove si trova il file di interesse nel file system. Vediamo un esempio di struttura di directory, si veda la struttura della directory `creating-hyperlinks` mostrata di seguito:

![Una semplice struttura di directory. La directory principale si chiama creating-hyperlinks e contiene due file chiamati index.html e contacts.html, e due directory chiamate projects e pdfs, che contengono rispettivamente un file index.html e un file project-brief.pdf](simple-directory.png)

La **radice** di questa struttura di directory si chiama `creating-hyperlinks`. Quando si lavora localmente con un sito web, si avrà una directory che contiene l'intero sito. All'interno della **radice**, abbiamo un file `index.html` e un `contacts.html`. In un sito web reale, `index.html` sarebbe la nostra pagina iniziale o pagina di destinazione (una pagina web che funge da punto di accesso per un sito web o una particolare sezione di un sito web).

Ci sono anche due directory all'interno della nostra radice — `pdfs` e `projects`. Queste hanno ciascuna un unico file all'interno di esse — un PDF (`project-brief.pdf`) e un file `index.html`, rispettivamente. Si noti che è possibile avere due file `index.html` in un progetto, a condizione che siano in posizioni diverse nel file system. Il secondo `index.html` sarebbe forse la pagina di destinazione principale per le informazioni relative ai progetti.

Vediamo alcuni esempi di collegamenti tra alcuni file diversi in questa struttura di directory per dimostrare diversi tipi di link:

- **Stessa directory**: Se si desidera includere un hyperlink all'interno di `index.html` (il livello superiore `index.html`) puntando a `contacts.html`, si specificherà il nome del file a cui si desidera collegarsi, poiché si trova nella stessa directory del file corrente. L'URL che utilizzeresti è `contacts.html`:

  ```html
  <p>
    Want to contact a specific staff member? Find details on our
    <a href="contacts.html">contacts page</a>.
  </p>
  ```

- **Spostarsi verso il basso in sottodirectory**: Se si desidera includere un hyperlink all'interno di `index.html` (il livello superiore `index.html`) puntando a `projects/index.html`, occorre scendere nella directory `projects` prima di indicare il file a cui si desidera collegarsi.
  Questo si fa specificando il nome della directory, poi uno slash in avanti, poi il nome del file. L'URL che utilizzeresti è `projects/index.html`:

  ```html
  <p>Visit my <a href="projects/index.html">project homepage</a>.</p>
  ```

- **Risalire a directory superiori**: Se vuoi includere un hyperlink all'interno di `projects/index.html` puntando a `pdfs/project-brief.pdf`, dovresti risalire di un livello di directory, quindi scendere di nuovo nella directory `pdfs`.
  Per risalire a una directory, usa due punti — `..` — quindi l'URL che utilizzeresti è `../pdfs/project-brief.pdf`:

  ```html
  <p>A link to my <a href="../pdfs/project-brief.pdf">project brief</a>.</p>
  ```

> [!NOTE]
> Puoi combinare più istanze di queste caratteristiche in URL complessi, se necessario, ad esempio: `../../../complex/path/to/my/file.html`.

### Frammenti di documento

È possibile collegarsi a una parte specifica di un documento HTML, noto come **frammento di documento**, piuttosto che solo alla parte superiore del documento.
Per fare questo devi prima assegnare un attributo [`id`](/it/docs/Web/HTML/Reference/Global_attributes/id) all'elemento a cui desideri collegarti.
Di solito ha senso collegarsi a un'intestazione specifica, quindi questo si presenterebbe nel modo seguente:

```html
<h2 id="Mailing_address">Mailing address</h2>
```

Poi, per collegarti a quello specifico `id`, lo includeresti alla fine dell'URL, preceduto da un simbolo di hash (cancelletto) (`#`), ad esempio:

```html
<p>
  Want to write us a letter? Use our
  <a href="contacts.html#Mailing_address">mailing address</a>.
</p>
```

Puoi anche usare il riferimento al frammento di documento da solo per collegarti a _un'altra parte del documento corrente_:

```html
<p>
  The <a href="#Mailing_address">company mailing address</a> can be found at the
  bottom of this page.
</p>
```

### URL assoluti rispetto a URL relativi

Due termini che incontrerai sul web sono **URL assoluto** e **URL relativo**:

**URL assoluto**: Punta a una posizione definita dalla sua posizione assoluta sul web, inclusi {{Glossary("protocol", "protocollo")}} e {{Glossary("domain_name", "nome di dominio")}}.
Ad esempio, se una pagina `index.html` è caricata in una directory chiamata `projects` che si trova all'interno della **radice** di un server web, e il dominio del sito web è `https://www.example.com`, la pagina sarebbe disponibile a `https://www.example.com/projects/index.html` (o anche solo `https://www.example.com/projects/`, poiché la maggior parte dei server web cerca semplicemente una pagina di destinazione come `index.html` da caricare se non specificata nell'URL.)

Un URL assoluto punterà sempre alla stessa posizione, indipendentemente da dove viene utilizzato.

**URL relativo**: Punta a una posizione che è _relativa_ al file da cui ti stai collegando, più come abbiamo visto nella sezione precedente.
Ad esempio, se volessimo collegarci dal nostro file di esempio a `https://www.example.com/projects/index.html` a un file PDF nella stessa directory, l'URL sarebbe solo il nome del file — `project-brief.pdf` — nessuna informazione aggiuntiva necessaria. Se il PDF fosse disponibile in una sottodirectory all'interno di `projects` chiamata `pdfs`, il link relativo sarebbe `pdfs/project-brief.pdf` (l'URL assoluto equivalente sarebbe `https://www.example.com/projects/pdfs/project-brief.pdf`.)

Un URL relativo punterà a luoghi diversi a seconda della posizione effettiva del file da cui ti riferisci — ad esempio se spostassimo il nostro file `index.html` fuori dalla directory `projects` e nella **radice** del sito web (il livello superiore, non in alcuna directory), il link relativo `pdfs/project-brief.pdf` al suo interno ora punterebbe a un file situato a `https://www.example.com/pdfs/project-brief.pdf`, non a un file situato a `https://www.example.com/projects/pdfs/project-brief.pdf`.

Ovviamente, la posizione del file `project-brief.pdf` e della cartella `pdfs` non cambierà improvvisamente perché hai spostato il file `index.html` — questo renderebbe il tuo link puntare al luogo sbagliato, quindi non funzionerebbe se cliccato. Devi fare attenzione!

## Migliori pratiche per i link

Ci sono alcune migliori pratiche da seguire quando si scrivono link. Vediamo queste ora.

### Utilizzare testo del link chiaro

È facile inserire link sulla tua pagina. Questo non è sufficiente. Dobbiamo rendere i nostri link _accessibili_ a tutti i lettori, indipendentemente dal loro contesto attuale e dagli strumenti che preferiscono. Ad esempio:

- Gli utenti di screen reader amano saltare da un link all'altro sulla pagina e leggere i link fuori contesto.
- I motori di ricerca utilizzano il testo del link per indicizzare i file target, quindi è una buona idea includere parole chiave nel testo del link per descrivere efficacemente cosa si sta collegando.
- I lettori visivi scorrono la pagina piuttosto che leggere ogni parola, e i loro occhi verranno attratti dalle caratteristiche della pagina che si distinguono, come i link. Troveranno utile il testo del link descrittivo.

Vediamo un esempio specifico:

Testo del link **buono**: [Scarica Firefox](https://www.mozilla.org/en-US/firefox/new/?redirect_source=firefox-com)

```html example-good
<p><a href="https://www.mozilla.org/en-US/firefox/new/">Download Firefox</a></p>
```

<!-- markdownlint-disable descriptive-link-text -->

Testo del link **cattivo**: [Clicca qui](https://www.mozilla.org/en-US/firefox/new/) per scaricare Firefox

```html example-bad
<p>
  <a href="https://www.mozilla.org/en-US/firefox/new/">Click here</a> to
  download Firefox
</p>
```

<!-- markdownlint-enable descriptive-link-text -->

Altri suggerimenti:

- Non ripetere l'URL come parte del testo del link — gli URL sembrano brutti e suonano ancora peggio quando un lettore di schermo li legge lettera per lettera.
- Non dire "link" o "collegamenti a" nel testo del link — sono solo rumore. Gli screen reader informano le persone che c'è un link.
  Gli utenti visivi sapranno anche che c'è un link, perché generalmente i link sono stilizzati in un colore diverso e sottolineati (questa convenzione generalmente non dovrebbe essere infranta, poiché gli utenti ci sono abituati).
- Mantieni il tuo testo del link il più breve possibile — questo è utile perché i lettori di schermo devono interpretare l'intero testo del link.
- Minimizza i casi in cui copie multiple dello stesso testo sono collegate a luoghi diversi.
  Questo può causare problemi agli utenti di screen reader, se c'è una lista di link fuori contesto che sono etichettati "clicca qui", "clicca qui", "clicca qui".

### Collegarsi a risorse non HTML — lasciare chiari segnaposti

Quando colleghi a una risorsa che non verrà aperta nella pagina corrente come una "navigazione normale", dovresti aggiungere un testo chiaro nel testo del link su ciò che sta per accadere. Ad esempio, se stai scaricando o trasmettendo una risorsa, o se il link aprirà una finestra popup o eseguirà qualche altro effetto potenzialmente inaspettato, questo deve essere affermato nel testo. Questo è importante per gli utenti con connessioni a bassa larghezza di banda che potrebbero voler evitare il download di asset di molti megabyte. Aiuta anche a stabilire aspettative per gli utenti di screen reader, che altrimenti potrebbero non essere consapevoli di ciò che sta accadendo.

Vediamo alcuni esempi, per vedere che tipo di testo può essere utilizzato qui:

```html
<p>
  <a href="/large-report.pdf" download>
    Download the sales report (PDF, 10MB)
  </a>
</p>

<p>
  <a href="https://www.example.com/video-stream/" target="_blank">
    Watch the video (stream opens in separate tab, HD quality)
  </a>
</p>
```

### Utilizzare l'attributo download quando si collega a un download

Quando stai collegando a una risorsa da scaricare piuttosto che da aprire nel browser, puoi utilizzare l'attributo `download` per fornire un nome file di salvataggio predefinito. Ecco un esempio con un link di download alla versione Windows più recente di Firefox:

```html
<a
  href="https://download.mozilla.org/?product=firefox-latest-ssl&os=win64&lang=en-US"
  download="firefox-latest-64bit-installer.exe">
  Download Latest Firefox for Windows (64-bit) (English, US)
</a>
```

### Quando aprire i link in una nuova scheda

I link per impostazione predefinita si aprono nella stessa scheda della pagina su cui si trovano, il che permette all'utente di navigare indietro alla pagina precedente utilizzando il pulsante indietro del browser. Tuttavia, molti siti (incluso MDN) scelgono di aprire certi link, specialmente quelli esterni, in una nuova scheda. Questo viene fatto impostando l'attributo [`target`](/it/docs/Web/HTML/Reference/Elements/a#target) su `"_blank"`.

```html
Firefox is developed by the
<a href="https://www.mozilla.org/en-US/" target="_blank">Mozilla Foundation</a>.
```

Aprire o no i link in una nuova scheda dovrebbe essere una decisione consapevole, basata sulle considerazioni di design dell'esperienza utente. Ecco alcune cose da considerare:

- Aprire i link in una nuova scheda presenta i due documenti simultaneamente, il che è utile per un'esperienza di navigazione "parallela". Dall'altro lato, link che si aprono nella stessa scheda sono più come una continuazione della pagina corrente.
- Aprire i link in una nuova scheda può essere disorientante per gli utenti abituati a utilizzare il pulsante indietro.
- Anche quando i link si aprono di default nella stessa scheda, gli utenti possono comunque scegliere di aprirli in una nuova scheda, utilizzando scorciatoie da tastiera o opzioni del menu contestuale. Dall'altro lato, link che si aprono in una nuova scheda sono difficili da aprire nella stessa scheda.
- Gli utenti di screen reader potrebbero essere confusi da link che si aprono in una nuova scheda, poiché potrebbero non rendersi conto che la nuova scheda si è aperta e potrebbero perdere il contesto sulla loro posizione nella pagina.

Un approccio comune è aprire i link esterni in nuove schede e i link interni nella stessa scheda.
Alcuni designer preferiscono aprire tutti i link nella stessa scheda.
Se apri link in nuove schede, si raccomanda di fornire indizi per questi link, come un'icona accanto al testo del link.

## Apprendimento attivo: creare un menu di navigazione

Per questo esercizio, vorremmo che collegassi alcune pagine insieme con un menu di navigazione per creare un sito web a più pagine. Questo è uno dei modi comuni in cui viene creato un sito web — la stessa struttura di pagina è utilizzata su ogni pagina, incluso lo stesso menu di navigazione, quindi quando vengono cliccati i link, dà l'impressione che si rimanga nello stesso luogo, e venga visualizzato un contenuto diverso.

Dovrai fare copie locali delle seguenti quattro pagine, tutte nella stessa directory. Per un elenco completo dei file, vedi la directory [navigation-menu-start](https://github.com/mdn/learning-area/tree/main/html/introduction-to-html/navigation-menu-start):

- [index.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/navigation-menu-start/index.html)
- [projects.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/navigation-menu-start/projects.html)
- [pictures.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/navigation-menu-start/pictures.html)
- [social.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/navigation-menu-start/social.html)

Dovresti:

1. Aggiungere un elenco non ordinato nel punto indicato su una pagina che include i nomi delle pagine a cui collegarsi.
   Un menu di navigazione è di solito solo un elenco di link, quindi questo è semanticamente corretto.
2. Trasforma ciascun nome di pagina in un link a quella pagina.
3. Copia il menu di navigazione su ciascuna pagina.
4. Su ciascuna pagina, rimuovi solo il link a quella stessa pagina — è confuso e non necessario che una pagina includa un link a se stessa.
   E, la mancanza di un link funge da buon promemoria visivo di quale pagina sei attualmente.

L'esempio finito dovrebbe apparire simile alla seguente pagina:

![Un esempio di un semplice menu di navigazione HTML, con le voci di menu home, pictures, projects e social](navigation-example.png)

> [!NOTE]
> Se rimani bloccato, o non sei sicuro di aver fatto tutto correttamente, puoi controllare la [directory navigation-menu-marked-up](https://github.com/mdn/learning-area/tree/main/html/introduction-to-html/navigation-menu-marked-up) per vedere la risposta corretta.

## Link email

È possibile creare link o pulsanti che, quando cliccati, aprono un nuovo messaggio email in uscita piuttosto che collegarsi a una risorsa o pagina.
Questo viene fatto utilizzando l'elemento {{HTMLElement("a")}} e lo schema URL `mailto:`.

Nella sua forma più semplice e comunemente usata, un link `mailto:` indica l'indirizzo email del destinatario previsto. Ad esempio:

```html
<a href="mailto:nowhere@mozilla.org">Send email to nowhere</a>
```

Questo risulta in un link che appare così: [Invia email a nowhere](mailto:nowhere@mozilla.org).

In effetti, l'indirizzo email è facoltativo. Se lo ometti e il tuo [`href`](/it/docs/Web/HTML/Reference/Elements/a#href) è "mailto:", una nuova finestra email in uscita verrà aperta dal client email dell'utente senza indirizzo di destinazione.
Questo è spesso utile come "link di condivisione" che gli utenti possono cliccare per inviare un'email a un indirizzo a loro scelta.

### Specificare dettagli

Oltre all'indirizzo email, puoi fornire altre informazioni. In effetti, qualsiasi campo di intestazione di email standard può essere aggiunto all'URL `mailto` che fornisci.
Quelli più comunemente usati sono "subject", "cc", e "body" (che non è un vero e proprio campo di intestazione, ma permette di specificare un breve messaggio di contenuto per la nuova email).
Ogni campo e il suo valore sono specificati come un termine di query.

Ecco un esempio che include un cc, bcc, subject e body:

```html
<a
  href="mailto:nowhere@mozilla.org?cc=name2@rapidtables.com&bcc=name3@rapidtables.com&subject=The%20subject%20of%20the%20email&body=The%20body%20of%20the%20email">
  Send mail with cc, bcc, subject and body
</a>
```

> [!NOTE]
> I valori di ciascun campo devono essere codificati in URL con caratteri non stampabili (caratteri invisibili come tabulazioni, ritorni a capo e interruzioni di pagina) e spazi {{Glossary("Percent-encoding", "percentualmente codificati")}}.
> Nota anche l'uso del punto interrogativo (`?`) per separare l'URL principale dai valori dei campi, e le e-commerciali (&) per separare ciascun campo nell'URL `mailto:`.
> Questa è la notazione standard delle query URL.
> Leggi [Il metodo GET](/it/docs/Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data#the_get_method) per comprendere per cosa è più comunemente usata la notazione delle query URL.

Ecco alcuni altri esempi di URL `mailto`:

- <mailto:>
- <mailto:nowhere@mozilla.org>
- <mailto:nowhere@mozilla.org,nobody@mozilla.org>
- <mailto:nowhere@mozilla.org?cc=nobody@mozilla.org>
- <mailto:nowhere@mozilla.org?cc=nobody@mozilla.org&subject=This%20is%20the%20subject>

## Metti alla prova le tue abilità!

Hai raggiunto la fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare ulteriori test per verificare di aver trattenuto queste informazioni prima di procedere — vedi [Metti alla prova le tue abilità: Link](/it/docs/Learn_web_development/Core/Structuring_content/Test_your_skills/Links).

## Sommario

Questo è tutto per i link, per ora comunque! Tornerai ai link più avanti nel corso quando inizierai a guardare come stilizzarli. Il prossimo passo per HTML, affronterai un paio di sfide che metteranno alla prova la tua comprensione degli argomenti trattati finora.

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Advanced_text_features", "Learn_web_development/Core/Structuring_content/Marking_up_a_letter", "Learn_web_development/Core/Structuring_content")}}
