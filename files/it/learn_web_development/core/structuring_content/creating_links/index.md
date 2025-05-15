---
title: Creare collegamenti
slug: Learn_web_development/Core/Structuring_content/Creating_links
l10n:
  sourceCommit: e488eba036b2fee56444fd579c3759ef45ff2ca8
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Advanced_text_features", "Learn_web_development/Core/Structuring_content/Marking_up_a_letter", "Learn_web_development/Core/Structuring_content")}}

I collegamenti (noti anche come collegamenti ipertestuali) sono davvero importanti — sono ciò che rende il Web _una rete_.
Questo articolo mostra la sintassi necessaria per creare un collegamento e discute le migliori pratiche per i collegamenti.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con HTML, come trattato in
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi HTML di base</a
        >. Semantica a livello di testo come <a href="/it/docs/Learn_web_development/Core/Structuring_content/Headings_and_paragraphs"
          >intestazioni e paragrafi</a
        > e <a href="/it/docs/Learn_web_development/Core/Structuring_content/Lists"
          >elenchi</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere perché i collegamenti sono la caratteristica fondamentale del web. Non esiste un web senza collegamenti.</li>
          <li>L'attributo <code>href</code>.</li>
          <li>Percorsi assoluti e relativi, e quando usarli.</li>
          <li>Sintassi dei percorsi in dettaglio: barre, punto singolo, e doppio punto.</li>
          <li>Stati del collegamento e perché sono importanti — <code>:hover</code>, <code>:focus</code>, <code>:visited</code>, e <code>:active</code>.</li>
          <li>Collegamenti a livello in linea e di blocco.</li>
          <li>Comprendere i vantaggi di scrivere buon testo per i collegamenti, come una migliore accessibilità per gli utenti di lettori di schermo, e potenziali effetti positivi per la SEO.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Cos'è un collegamento ipertestuale?

I collegamenti ipertestuali sono una delle innovazioni più entusiasmanti che il Web ha da offrire.
Sono stati una caratteristica del Web sin dall'inizio e sono ciò che rende il Web _una rete._
I collegamenti ipertestuali ci consentono di collegare documenti ad altri documenti o risorse, collegare a parti specifiche di documenti, o rendere disponibili app a un indirizzo web.
Quasi qualsiasi contenuto web può essere convertito in un collegamento in modo che quando viene cliccato o attivato in altro modo, il browser web si rechi a un altro indirizzo web ({{Glossary("URL", "URL")}}).

> [!NOTE]
> Un URL può puntare a file HTML, file di testo, immagini, documenti di testo, file video e audio, o qualsiasi altra cosa che esista sul Web.
> Se il browser web non sa come visualizzare o gestire il file, le chiederà se vuole aprire il file (nel qual caso il compito di aprire o gestire il file viene passato a un'applicazione nativa adatta sul dispositivo) o scaricare il file (nel qual caso potrà provare a gestirlo più tardi).

Ad esempio, la homepage della BBC contiene molti collegamenti che puntano non solo a molteplici notizie, ma anche a diverse aree del sito (funzionalità di navigazione), pagine di accesso/registrazione (strumenti per l'utente), e altro ancora.

![prima pagina di bbc.co.uk, mostrando molti articoli di notizie e funzionalità del menu di navigazione](updated-bbc-website.png)

## Anatomia di un collegamento

Un collegamento di base viene creato racchiudendo il testo o altro contenuto all'interno di un elemento {{htmlelement("a")}} e utilizzando l'attributo [`href`](/it/docs/Web/HTML/Reference/Elements/a#href), noto anche come **Hypertext Reference**, o **target**, che contiene l'indirizzo web.

```html
<p>
  I'm creating a link to
  <a href="https://www.mozilla.org/en-US/">the Mozilla homepage</a>.
</p>
```

Questo ci dà il seguente risultato:

Sto creando un collegamento alla [homepage di Mozilla](https://www.mozilla.org/en-US/).

### Collegamenti a livello di blocco

Come detto in precedenza, quasi qualsiasi contenuto può essere trasformato in un collegamento, anche gli {{Glossary("Block/CSS", "elementi a livello di blocco")}}.
Se vuole rendere un elemento di intestazione un collegamento, racchiuda l'intestazione in un elemento ancora (`<a>`) come mostrato nel seguente frammento di codice:

```html
<a href="https://developer.mozilla.org/en-US/">
  <h1>MDN Web Docs</h1>
</a>
<p>
  Documenting web technologies, including CSS, HTML, and JavaScript, since 2005.
</p>
```

Questo trasforma l'intestazione in un collegamento:
{{EmbedLiveSample('Block level links', '100%', 150)}}

### Collegamenti di immagine

Per trasformare un'immagine in un collegamento, racchiuda l'elemento {{htmlelement("img")}} con un elemento {{htmlelement("a")}}. L'esempio seguente utilizza un percorso relativo per fare riferimento a un file di immagine SVG salvato localmente.

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

Questo rende il logo MDN un collegamento:
{{EmbedLiveSample('Image links', '100%', 150)}}

> [!NOTE]
> Scoprirà di più sull'utilizzo delle immagini sul Web in un articolo futuro.

### Aggiungere informazioni di supporto con l'attributo title

Un altro attributo che potrebbe voler aggiungere ai suoi collegamenti è `title`.
Il title contiene informazioni aggiuntive sul collegamento, come il tipo di informazioni che la pagina contiene, o cose da sapere sul sito web.

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

Questo ci dà il seguente risultato e, passando il mouse sopra il collegamento, visualizza il titolo come un tooltip:

{{EmbedLiveSample('Adding supporting information with the title attribute', '100%', 150)}}

> [!NOTE]
> Un titolo di collegamento viene rivelato solo al passaggio del mouse, il che significa che le persone che si affidano a controlli tramite tastiera o schermi tattili per navigare le pagine web avranno difficoltà ad accedere alle informazioni del titolo.
> Se le informazioni del titolo sono veramente importanti per l'utilizzo della pagina, allora dovrebbe presentarle in un modo che sia accessibile a tutti gli utenti, ad esempio inserendole nel testo regolare.

### Apprendimento attivo: creare il proprio esempio di collegamento

Crei un documento HTML utilizzando il suo editor di codice locale e il nostro [modello introduttivo](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/getting-started/index.html).

- All'interno del corpo HTML, aggiunga uno o più paragrafi o altri tipi di contenuto che già conosce.
- Cambi alcuni dei contenuti in collegamenti.
- Includa attributi title.

## Un breve ripasso su URL e percorsi

Per comprendere appieno i target dei collegamenti, è necessario comprendere gli URL e i percorsi dei file. Questa sezione le fornisce le informazioni necessarie per raggiungere questo scopo.

Un URL, o Uniform Resource Locator, è una stringa di testo che definisce dove qualcosa si trova sul Web. Ad esempio, la homepage in inglese di Mozilla si trova a `https://www.mozilla.org/en-US/`.

Gli URL utilizzano i percorsi per trovare i file. I percorsi specificano dove si trova il file di suo interesse nel filesystem. Esaminiamo un esempio di struttura di directory, veda la struttura della directory `creating-hyperlinks` mostrata di seguito:

![Una semplice struttura di directory. La directory principale è chiamata creating-hyperlinks e contiene due file chiamati index.html e contacts.html e due directory chiamate projects e pdfs, che contengono rispettivamente un file index.html e un file project-brief.pdf](simple-directory.png)

La **radice** di questa struttura di directory è chiamata `creating-hyperlinks`. Quando lavora localmente con un sito web, avrà una directory che contiene l'intero sito. All'interno della **radice**, abbiamo un file `index.html` e un `contacts.html`. In un sito web reale, `index.html` sarebbe la nostra homepage o pagina di atterraggio (una pagina web che serve come punto di ingresso per un sito web o una sezione particolare di un sito web).

Ci sono anche due directory all'interno della nostra radice — `pdfs` e `projects`. Ciascuna di queste ha un solo file al loro interno — un PDF (`project-brief.pdf`) e un file `index.html`, rispettivamente. Si noti che è possibile avere due file `index.html` in un progetto, fintanto che sono in posizioni diverse nel filesystem. Il secondo `index.html` sarebbe forse la pagina di atterraggio principale per le informazioni relative al progetto.

Esaminiamo alcuni esempi di collegamenti tra alcuni file diversi in questa struttura della directory per dimostrare diversi tipi di collegamento:

- **Stessa directory**: Se volesse includere un collegamento ipertestuale all'interno di `index.html` (il livello superiore `index.html`) che punta a `contacts.html`, dovrebbe specificare il nome del file a cui vuole collegarsi, perché si trova nella stessa directory del file corrente. L'URL che utilizzerebbe è `contacts.html`:

  ```html
  <p>
    Want to contact a specific staff member? Find details on our
    <a href="contacts.html">contacts page</a>.
  </p>
  ```

- **Spostarsi verso il basso nelle sottodirectory**: Se volesse includere un collegamento ipertestuale all'interno di `index.html` (il livello superiore `index.html`) che punta a `projects/index.html`, dovrebbe scendere nella directory `projects` prima di indicare il file a cui vuole collegarsi.
  Questo viene fatto specificando il nome della directory, poi una barra, poi il nome del file. L'URL che utilizzerebbe è `projects/index.html`:

  ```html
  <p>Visit my <a href="projects/index.html">project homepage</a>.</p>
  ```

- **Risale nelle directory padre**: Se volesse includere un collegamento ipertestuale all'interno di `projects/index.html` che punta a `pdfs/project-brief.pdf`, dovrebbe risalire di un livello di directory, poi scendere nella directory `pdfs`.
  Per risalire di una directory, utilizzi due punti — `..` — quindi l'URL che utilizzerebbe è `../pdfs/project-brief.pdf`:

  ```html
  <p>A link to my <a href="../pdfs/project-brief.pdf">project brief</a>.</p>
  ```

> [!NOTE]
> È possibile combinare più istanze di queste funzionalità in URL complessi, se necessario, ad esempio: `../../../complex/path/to/my/file.html`.

### Frammenti di documento

È possibile collegarsi a una parte specifica di un documento HTML, nota come **frammento di documento**, piuttosto che solo alla parte superiore del documento.
Per fare questo, prima deve assegnare un attributo [`id`](/it/docs/Web/HTML/Reference/Global_attributes/id) all'elemento a cui vuole collegarsi.
Normalmente ha senso collegarsi a un'intestazione specifica, quindi questo sarebbe simile a quanto segue:

```html
<h2 id="Mailing_address">Mailing address</h2>
```

Poi, per collegarsi a quello specifico `id`, lo includerebbe alla fine dell'URL, preceduto da un simbolo hash/#:

```html
<p>
  Want to write us a letter? Use our
  <a href="contacts.html#Mailing_address">mailing address</a>.
</p>
```

Può anche usare il riferimento al frammento di documento da solo per collegarsi a _un'altra parte del documento corrente_:

```html
<p>
  The <a href="#Mailing_address">company mailing address</a> can be found at the
  bottom of this page.
</p>
```

### URL assoluti rispetto agli URL relativi

Due termini che incontrerà sul web sono **URL assoluto** e **URL relativo:**

**URL assoluto**: Punta a una posizione definita dalla sua posizione assoluta sul web, includendo il {{Glossary("protocol", "protocollo")}} e il {{Glossary("domain_name", "nome di dominio")}}.
Ad esempio, se una pagina `index.html` è caricata in una directory chiamata `projects` che si trova all'interno della **radice** di un server web, e il dominio del sito web è `https://www.example.com`, la pagina sarebbe disponibile a `https://www.example.com/projects/index.html` (o anche solo `https://www.example.com/projects/`, dato che la maggior parte dei server web cerca solo una pagina di atterraggio come `index.html` da caricare se non è specificata nell'URL).

Un URL assoluto punterà sempre alla stessa posizione, indipendentemente da dove viene utilizzato.

**URL relativo**: Punta a una posizione che è _relativa_ al file da cui si sta collegando, simile a quanto visto nella sezione precedente.
Ad esempio, se volessimo collegarci dal nostro file di esempio a `https://www.example.com/projects/index.html` verso un file PDF nella stessa directory, l'URL sarebbe solo il nome del file — `project-brief.pdf` — nessuna informazione aggiuntiva necessaria. Se il PDF era disponibile in una sottodirectory all'interno di `projects` chiamata `pdfs`, il collegamento relativo sarebbe `pdfs/project-brief.pdf` (l'equivalente URL assoluto sarebbe `https://www.example.com/projects/pdfs/project-brief.pdf`).

Un URL relativo punterà a posti diversi a seconda della posizione effettiva del file da cui si fa riferimento — ad esempio se abbiamo spostato il nostro file `index.html` fuori dalla directory `projects` e nella **radice** del sito web (il livello superiore, non in nessuna directory), il collegamento relativo `pdfs/project-brief.pdf` al suo interno ora punterebbe a un file situato a `https://www.example.com/pdfs/project-brief.pdf`, non a un file situato a `https://www.example.com/projects/pdfs/project-brief.pdf`.

Ovviamente, la posizione del file `project-brief.pdf` e della cartella `pdfs` non cambierà improvvisamente perché ha spostato il file `index.html` — questo farebbe sì che il suo collegamento punti al posto sbagliato, quindi non funzionerebbe se cliccato. Deve fare attenzione!

## Le migliori pratiche per i collegamenti

Esistono alcune migliori pratiche da seguire quando si scrivono collegamenti. Esaminiamole ora.

### Utilizzare un testo chiaro per i collegamenti

È facile inserire collegamenti sulla sua pagina. Non basta. Dobbiamo rendere i nostri collegamenti _accessibili_ a tutti i lettori, indipendentemente dal loro contesto attuale e dagli strumenti che preferiscono. Ad esempio:

- Gli utenti di lettori di schermo amano saltare da un collegamento all'altro sulla pagina, leggendo i collegamenti fuori contesto.
- I motori di ricerca utilizzano il testo del collegamento per indicizzare i file di destinazione, quindi è una buona idea includere parole chiave nel testo del collegamento per descrivere efficacemente ciò a cui si sta collegando.
- I lettori visivi scorrono la pagina piuttosto che leggere ogni parola e i loro occhi saranno attratti da funzionalità della pagina che si distinguono, come i collegamenti. Troveranno utile un testo descrittivo per i collegamenti.

Esaminiamo un esempio specifico:

**Buon** testo del collegamento: [Scarica Firefox](https://www.mozilla.org/en-US/firefox/new/?redirect_source=firefox-com)

```html example-good
<p><a href="https://www.mozilla.org/en-US/firefox/new/">Download Firefox</a></p>
```

<!-- markdownlint-disable descriptive-link-text -->

**Cattivo** testo del collegamento: [Clicchi qui](https://www.mozilla.org/en-US/firefox/new/) per scaricare Firefox

```html example-bad
<p>
  <a href="https://www.mozilla.org/en-US/firefox/new/">Click here</a> to
  download Firefox
</p>
```

<!-- markdownlint-enable descriptive-link-text -->

Altri suggerimenti:

- Non ripeta l'URL come parte del testo del collegamento — gli URL appaiono brutti e suonano anche peggio quando un lettore di schermo li legge lettera per lettera.
- Non dica "collegamento" o "collega a" nel testo del collegamento — è solo rumore. I lettori di schermo informano le persone che c'è un collegamento.
  Gli utenti visuali sapranno anche che c'è un collegamento, perché i collegamenti sono generalmente stilizzati in un colore diverso e sottolineati (questa convenzione generalmente non dovrebbe essere infranta, poiché gli utenti sono abituati a essa).
- Mantenga il testo del collegamento il più breve possibile — questo è utile perché i lettori di schermo devono interpretare l'intero testo del collegamento.
- Minimizzare le istanze in cui più copie dello stesso testo sono collegate a posti diversi.
  Questo può causare problemi agli utenti di lettori di schermo, se c'è un elenco di collegamenti fuori contesto etichettati come "clicca qui", "clicca qui", "clicca qui".

### Collegamento a risorse non HTML — lasciare segnali chiari

Quando ci si collega a una risorsa che non verrà aperta nella pagina corrente come una "navigazione normale", si dovrebbe aggiungere un testo chiaro al testo del collegamento su ciò che sta per accadere. Ad esempio, se sta scaricando o trasmettendo una risorsa, o se il collegamento aprirà un popup o eseguirà qualche altro effetto potenzialmente inaspettato, questo dovrebbe essere indicato nel testo. Questo è importante per gli utenti su connessioni a banda bassa, che potrebbero voler evitare di scaricare asset di molti megabyte. Aiuta anche a creare aspettative per gli utenti di lettori di schermo, che altrimenti potrebbero non essere consapevoli di ciò che sta accadendo.

Esaminiamo alcuni esempi per vedere che tipo di testo può essere utilizzato qui:

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

Quando si collega a una risorsa che deve essere scaricata piuttosto che aperta nel browser, può utilizzare l'attributo `download` per fornire un nome file predefinito per il salvataggio. Ecco un esempio con un collegamento per il download dell'ultima versione di Windows di Firefox:

```html
<a
  href="https://download.mozilla.org/?product=firefox-latest-ssl&os=win64&lang=en-US"
  download="firefox-latest-64bit-installer.exe">
  Download Latest Firefox for Windows (64-bit) (English, US)
</a>
```

### Quando aprire i collegamenti in una nuova scheda

I collegamenti per impostazione predefinita si aprono nella stessa scheda della pagina su cui si trovano, il che consente all'utente di tornare alla pagina precedente utilizzando il pulsante "indietro" del browser. Tuttavia, molti siti (incluso MDN) scelgono di aprire alcuni collegamenti, specialmente i collegamenti esterni, in una nuova scheda. Ciò viene fatto impostando l'attributo [`target`](/it/docs/Web/HTML/Reference/Elements/a#target) su `"_blank"`.

```html
Firefox is developed by the
<a href="https://www.mozilla.org/en-US/" target="_blank">Mozilla Foundation</a>.
```

Se aprire o meno i collegamenti in una nuova scheda dovrebbe essere una decisione consapevole, basata su considerazioni di progettazione dell'esperienza utente. Ecco alcune cose da considerare:

- L'apertura di collegamenti in una nuova scheda presenta i due documenti contemporaneamente, il che è utile per un'esperienza di navigazione "parallela". D'altra parte, i collegamenti che si aprono nella stessa scheda sono più simili a una continuazione della pagina attuale.
- L'apertura di collegamenti in una nuova scheda può essere disorientante per gli utenti abituati a usare il pulsante "indietro".
- Anche quando i collegamenti vengono aperti nella stessa scheda per impostazione predefinita, gli utenti possono comunque scegliere di aprirli in una nuova scheda, utilizzando scorciatoie da tastiera o opzioni del menu contestuale. D'altra parte, i collegamenti che si aprono in una nuova scheda sono difficili da aprire nella stessa scheda.
- Gli utenti di lettori di schermo possono essere confusi dai collegamenti che si aprono in una nuova scheda, poiché potrebbero non rendersi conto che è stata aperta una nuova scheda e potrebbero perdere il contesto della loro posizione sulla pagina.

Un approccio comune è aprire collegamenti esterni in nuove schede e collegamenti interni nella stessa scheda.
Alcuni designer preferiscono aprire tutti i collegamenti nella stessa scheda.
Se apre collegamenti in nuove schede, si raccomanda di fornire indizi per questi collegamenti, come un'icona accanto al testo del collegamento.

## Apprendimento attivo: creare un menu di navigazione

Per questo esercizio, vorremmo che collegasse alcune pagine insieme con un menu di navigazione per creare un sito web a più pagine. Questo è un modo comune in cui viene creato un sito web — la stessa struttura di pagina viene utilizzata su ogni pagina, incluso lo stesso menu di navigazione, quindi quando i collegamenti vengono cliccati dà l'impressione di rimanere nello stesso posto, e diverso contenuto viene mostrato.

Dovrà fare copie locali delle seguenti quattro pagine, tutte nella stessa directory. Per un elenco completo dei file, veda la directory [navigation-menu-start](https://github.com/mdn/learning-area/tree/main/html/introduction-to-html/navigation-menu-start):

- [index.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/navigation-menu-start/index.html)
- [projects.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/navigation-menu-start/projects.html)
- [pictures.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/navigation-menu-start/pictures.html)
- [social.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/navigation-menu-start/social.html)

Dovrebbe:

1. Aggiungere un elenco non ordinato nel punto indicato su una pagina che includa i nomi delle pagine a cui collegarsi.
   Un menu di navigazione è solitamente solo un elenco di collegamenti, quindi questo è semanticamente corretto.
2. Cambiare ogni nome di pagina in un collegamento a quella pagina.
3. Copiare il menu di navigazione su ogni pagina.
4. Su ogni pagina, rimuovere solo il collegamento a quella stessa pagina — è confuso e non necessario che una pagina includa un collegamento a se stessa.
   E, la mancanza di un collegamento funge da buon promemoria visivo di quale pagina sta attualmente visualizzando.

L'esempio finale dovrebbe apparire simile alla seguente pagina:

![Un esempio di un semplice menu di navigazione HTML, con voci di menu home, pictures, projects, e social](navigation-example.png)

> [!NOTE]
> Se si blocca, o non è sicuro se ha fatto bene, può controllare la directory [navigation-menu-marked-up](https://github.com/mdn/learning-area/tree/main/html/introduction-to-html/navigation-menu-marked-up) per assicurarsi della risposta corretta.

## Collegamenti email

È possibile creare collegamenti o pulsanti che, quando vengono cliccati, aprono un nuovo messaggio email in uscita piuttosto che collegarsi a una risorsa o pagina.
Ciò viene fatto utilizzando l'elemento {{HTMLElement("a")}} e lo schema URL `mailto:`.

Nella sua forma più semplice e comunemente utilizzata, un collegamento `mailto:` indica l'indirizzo email del destinatario previsto. Ad esempio:

```html
<a href="mailto:nowhere@mozilla.org">Send email to nowhere</a>
```

Ciò si traduce in un collegamento che sembra così: [Invia email a nowhere](mailto:nowhere@mozilla.org).

In realtà, l'indirizzo email è facoltativo. Se lo omette e il suo [`href`](/it/docs/Web/HTML/Reference/Elements/a#href) è "mailto:", verrà aperta una nuova finestra email in uscita dal client email dell'utente senza un indirizzo di destinazione.
Questo è spesso utile come "Collegamenti di condivisione" su cui gli utenti possono cliccare per inviare un'email a un indirizzo di loro scelta.

### Specificare dettagli

Oltre all'indirizzo email, può fornire altre informazioni. In effetti, qualsiasi campo di intestazione email standard può essere aggiunto all'URL `mailto` che fornisce.
I campi più comunemente usati sono "subject", "cc" e "body" (che non è un vero campo di intestazione, ma le consente di specificare un breve messaggio di contenuto per la nuova email).
Ogni campo e il suo valore sono specificati come una voce di query.

Ecco un esempio che include un cc, bcc, subject e body:

```html
<a
  href="mailto:nowhere@mozilla.org?cc=name2@rapidtables.com&bcc=name3@rapidtables.com&subject=The%20subject%20of%20the%20email&body=The%20body%20of%20the%20email">
  Send mail with cc, bcc, subject and body
</a>
```

> [!NOTE]
> I valori di ciascun campo devono essere codificati per URL con caratteri non stampabili (caratteri invisibili come tab, ritorni a capo e interruzioni di pagina) e gli spazi {{Glossary("Percent-encoding", "escapes percentuali")}}.
> Noti anche l'uso del punto di domanda (`?`) per separare l'URL principale dai valori dei campi e degli e commerciali (&) per separare ogni campo nell'URL `mailto:`.
> Questa è la notazione standard delle query URL.
> Legga [Il metodo GET](/it/docs/Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data#the_get_method) per capire per cosa è più comunemente usata la notazione delle query URL.

Ecco alcuni altri URL `mailto` di esempio:

- <mailto:>
- <mailto:nowhere@mozilla.org>
- <mailto:nowhere@mozilla.org,nobody@mozilla.org>
- <mailto:nowhere@mozilla.org?cc=nobody@mozilla.org>
- <mailto:nowhere@mozilla.org?cc=nobody@mozilla.org&subject=This%20is%20the%20subject>

## Test delle vostre competenze!

È arrivato alla fine di questo articolo, ma riesce a ricordare le informazioni più importanti? Può trovare ulteriori test per verificare che abbia mantenuto queste informazioni prima di procedere — vedere [Test delle sue competenze: Collegamenti](/it/docs/Learn_web_development/Core/Structuring_content/Test_your_skills/Links).

## Sintesi

Questo è tutto per quanto riguarda i collegamenti, almeno per ora! Tornerà ai collegamenti più avanti nel corso quando inizierà a guardare come stilizzarli. Il prossimo passo per quanto riguarda HTML sarà affrontare un paio di sfide che testeranno la sua comprensione degli argomenti trattati finora.

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Advanced_text_features", "Learn_web_development/Core/Structuring_content/Marking_up_a_letter", "Learn_web_development/Core/Structuring_content")}}
