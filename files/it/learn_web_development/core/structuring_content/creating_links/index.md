---
title: Creazione dei collegamenti
slug: Learn_web_development/Core/Structuring_content/Creating_links
l10n:
  sourceCommit: 6a5c619dfad295ca9a9d317a4088908cfd33e686
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Advanced_text_features", "Learn_web_development/Core/Structuring_content/Marking_up_a_letter", "Learn_web_development/Core/Structuring_content")}}

I collegamenti (noti anche come hyperlink) sono davvero importanti — sono ciò che rende il Web _un web_.
Questo articolo mostra la sintassi necessaria per creare un collegamento e discute le migliori pratiche per i collegamenti.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con HTML, come trattato in
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi di base HTML</a
        >. Semantica a livello di testo come <a href="/it/docs/Learn_web_development/Core/Structuring_content/Headings_and_paragraphs"
          >intestazioni e paragrafi</a
        > e <a href="/it/docs/Learn_web_development/Core/Structuring_content/Lists"
          >elenchi</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere perché i collegamenti sono la caratteristica fondamentale del web. Non esistono web senza collegamenti.</li>
          <li>L'attributo <code>href</code>.</li>
          <li>Percorsi assoluti e relativi, e quando utilizzarli.</li>
          <li>Sintassi del percorso in dettaglio — slash, punto singolo e punto doppio.</li>
          <li>Stati del collegamento e perché sono importanti — <code>:hover</code>, <code>:focus</code>, <code>:visited</code>, e <code>:active</code>.</li>
          <li>Collegamenti inline e a livello di blocco.</li>
          <li>Comprendere i benefici della scrittura di un buon testo per i collegamenti, come una migliore accessibilità per gli utenti di lettori di schermo e potenziali effetti positivi sulla SEO.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Cos'è un hyperlink?

Gli hyperlink sono una delle innovazioni più entusiasmanti che il Web ha da offrire.
Sono stati una caratteristica del Web fin dall'inizio, e sono ciò che rende il Web _un web._
Gli hyperlink ci permettono di collegare documenti ad altri documenti o risorse, collegare a parti specifiche di documenti, o rendere le app disponibili a un indirizzo web.
Quasi qualsiasi contenuto web può essere convertito in un collegamento in modo che quando cliccato o attivato in altro modo, il browser web vada a un altro indirizzo web ({{Glossary("URL", "URL")}}).

> [!NOTE]
> Un URL può puntare a file HTML, file di testo, immagini, documenti di testo, file video e audio, o qualsiasi altra cosa che vive sul Web.
> Se il browser web non sa come visualizzare o gestire il file, chiederà se si desidera aprire il file (in tal caso il compito di aprire o gestire il file viene passato a un'app nativa adatta sul dispositivo) o scaricare il file (in tal caso si può provare a gestirlo successivamente).

Ad esempio, la homepage della BBC contiene molti collegamenti che puntano non solo a diverse storie di notizie, ma anche a diverse aree del sito (funzionalità di navigazione), pagine di login o registrazione (strumenti per l'utente), e altro ancora.

![pagina principale di bbc.co.uk, che mostra molti articoli di notizie, e la funzionalità del menu di navigazione](updated-bbc-website.png)

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

Come accennato prima, quasi qualsiasi contenuto può essere trasformato in un collegamento, anche {{Glossary("Block/CSS", "elementi a livello di blocco")}}.
Se si desidera trasformare un elemento di intestazione in un collegamento, allora avvolgerlo in un elemento ancora (`<a>`) come mostrato nel seguente frammento di codice:

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

### Collegamenti con immagini

Per trasformare un'immagine in un collegamento, avvolgere l'elemento {{htmlelement("img")}} con un elemento {{htmlelement("a")}}. L'esempio seguente utilizza un percorso relativo per fare riferimento a un file immagine SVG archiviato localmente.

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
> Scoprirete di più sull'uso delle immagini sul Web in un articolo futuro.

### Aggiungere informazioni di supporto con l'attributo title

Un altro attributo che potrebbe voler aggiungere ai propri collegamenti è `title`.
Il titolo contiene informazioni aggiuntive sul collegamento, come il tipo di informazioni che la pagina contiene o le cose da tenere presenti sul sito web.

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

Questo ci dà il seguente risultato e passando il mouse sul collegamento viene visualizzato il titolo come tooltip:

{{EmbedLiveSample('Adding supporting information with the title attribute', '100%', 150)}}

> [!NOTE]
> Un titolo del collegamento viene rivelato solo al passaggio del mouse, il che significa che le persone che si affidano ai controlli della tastiera o agli schermi touchscreen per navigare nelle pagine web avranno difficoltà ad accedere alle informazioni del titolo.
> Se le informazioni di un titolo sono davvero importanti per l'usabilità della pagina, allora dovresti presentarle in un modo che sia accessibile a tutti gli utenti, ad esempio mettendole nel testo normale.

### Apprendimento attivo: creare il proprio esempio di collegamento

Crei un documento HTML utilizzando il suo editor di codice locale e il nostro [modello di avvio](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/getting-started/index.html).

- All'interno del corpo HTML, aggiunga uno o più paragrafi o altri tipi di contenuto che già conosce.
- Trasformi parte del contenuto in collegamenti.
- Includa attributi di titolo.

## Un breve primo piano su URL e percorsi

Per comprendere appieno i target dei collegamenti, è necessario comprendere URL e percorsi dei file. Questa sezione fornisce le informazioni necessarie per ottenere ciò.

Un URL, o Localizzatore di Risorse Uniforme, è una stringa di testo che definisce dove si trova qualcosa sul Web. Ad esempio, la homepage inglese di Mozilla si trova all'indirizzo `https://www.mozilla.org/en-US/`.

Gli URL utilizzano i percorsi per trovare i file. I percorsi specificano dove si trova il file di interesse nel file system. Diamo un'occhiata a un esempio di struttura di directory, vedere la struttura della directory `creating-hyperlinks` mostrata di seguito:

![Una semplice struttura di directory. La directory principale si chiama creating-hyperlinks e contiene due file chiamati index.html e contacts.html, e due directory chiamate projects e pdfs, che contengono rispettivamente un file index.html e un file project-brief.pdf](simple-directory.png)

La **radice** di questa struttura di directory si chiama `creating-hyperlinks`. Quando si lavora localmente con un sito web, si avrà una directory che contiene l'intero sito. All'interno della **radice**, abbiamo un file `index.html` e un `contacts.html`. In un vero sito web, `index.html` sarebbe la nostra homepage o landing page (una pagina web che serve come punto di ingresso per un sito web o una particolare sezione di un sito web.).

Ci sono anche due directory all'interno della nostra radice — `pdfs` e `projects`. Ognuna di queste ha un singolo file al suo interno — un PDF (`project-brief.pdf`) e un file `index.html`, rispettivamente. Nota che è possibile avere due file `index.html` in un unico progetto, purché siano in posizioni diverse nel file system. Il secondo `index.html` sarebbe forse la pagina principale di destinazione per le informazioni relative al progetto.

Diamo un'occhiata ad alcuni esempi di collegamenti tra alcuni file diversi in questa struttura di directory per dimostrare diversi tipi di collegamento:

- **Stessa directory**: Se si desidera includere un hyperlink all'interno di `index.html` (il livello superiore `index.html`) che punti a `contacts.html`, si specifica il nome del file a cui si desidera collegarsi, perché è nella stessa directory del file corrente. L'URL che si utilizzerebbe è `contacts.html`:

  ```html
  <p>
    Want to contact a specific staff member? Find details on our
    <a href="contacts.html">contacts page</a>.
  </p>
  ```

- **Scendendo nelle sottodirectory**: Se si desidera includere un hyperlink all'interno di `index.html` (il livello superiore `index.html`) che punti a `projects/index.html`, è necessario scendere nella directory `projects` prima di indicare il file a cui si desidera collegarsi. Questo viene fatto specificando il nome della directory, quindi una barra in avanti, quindi il nome del file. L'URL che si utilizzerebbe è `projects/index.html`:

  ```html
  <p>Visit my <a href="projects/index.html">project homepage</a>.</p>
  ```

- **Risali nelle directory principali**: Se si desidera includere un hyperlink all'interno di `projects/index.html` che punti a `pdfs/project-brief.pdf`, è necessario salire di un livello di directory, quindi tornare giù nella directory `pdfs`. Per risalire una directory, usi due punti — `..` — quindi l'URL che userebbe è `../pdfs/project-brief.pdf`:

  ```html
  <p>A link to my <a href="../pdfs/project-brief.pdf">project brief</a>.</p>
  ```

> [!NOTE]
> Può combinare più istanze di queste caratteristiche in URL complessi, se necessario, ad esempio: `../../../complex/path/to/my/file.html`.

### Frammenti di documenti

È possibile collegarsi a una parte specifica di un documento HTML, nota come **frammento di documento**, piuttosto che solo alla parte superiore del documento. Per fare ciò, è necessario assegnare un attributo [`id`](/it/docs/Web/HTML/Reference/Global_attributes/id) all'elemento a cui si vuole collegarsi. Di solito ha senso collegarsi a una specifica intestazione, quindi assomiglierebbe a qualcosa di simile al seguente:

```html
<h2 id="Mailing_address">Mailing address</h2>
```

Quindi, per collegarsi a quel specifico `id`, lo si includerebbe alla fine dell'URL, preceduto da un simbolo di hash/numero (`#`), ad esempio:

```html
<p>
  Want to write us a letter? Use our
  <a href="contacts.html#Mailing_address">mailing address</a>.
</p>
```

È possibile anche utilizzare il riferimento al frammento di documento da solo per collegarsi a _un'altra parte del documento corrente_:

```html
<p>
  The <a href="#Mailing_address">company mailing address</a> can be found at the
  bottom of this page.
</p>
```

### URL assoluti versus URL relativi

Due termini che incontrerai sul web sono **URL assoluto** e **URL relativo**:

**URL assoluto**: Punta a una posizione definita dalla sua posizione assoluta sul web, incluso il {{Glossary("protocol", "protocollo")}} e il {{Glossary("domain_name", "nome di dominio")}}. Ad esempio, se una pagina `index.html` è caricata in una directory chiamata `projects` che si trova all'interno della **radice** di un server web, e il dominio del sito è `https://www.example.com`, la pagina sarebbe disponibile all'indirizzo `https://www.example.com/projects/index.html` (o anche solo `https://www.example.com/projects/`, poiché la maggior parte dei server web cerca semplicemente una pagina di destinazione come `index.html` da caricare se non specificata nell'URL.)

Un URL assoluto punterà sempre alla stessa posizione, indipendentemente da dove viene utilizzato.

**URL relativo**: Punta a una posizione che è _relativa_ al file da cui ti stai collegando, più simile a quanto abbiamo visto nella sezione precedente. Ad esempio, se volessimo collegarci dal nostro file di esempio a `https://www.example.com/projects/index.html` a un file PDF nella stessa directory, l'URL sarebbe semplicemente il nome del file — `project-brief.pdf` — nessuna informazione extra necessaria. Se il PDF fosse disponibile in una sottodirectory all'interno di `projects` chiamata `pdfs`, il collegamento relativo sarebbe `pdfs/project-brief.pdf` (l'URL assoluto equivalente sarebbe `https://www.example.com/projects/pdfs/project-brief.pdf`.)

Un URL relativo punterà a luoghi diversi a seconda della posizione effettiva del file da cui si riferisce — ad esempio, se spostassimo il nostro file `index.html` fuori dalla directory `projects` e nella **radice** del sito web (il livello superiore, non in alcuna directory), il collegamento relativo all'URL `pdfs/project-brief.pdf` al suo interno ora punterebbe a un file situato in `https://www.example.com/pdfs/project-brief.pdf`, non a un file situato in `https://www.example.com/projects/pdfs/project-brief.pdf`.

Naturalmente, la posizione del file `project-brief.pdf` e della cartella `pdfs` non cambierà improvvisamente perché ha spostato il file `index.html` — questo renderebbe il suo collegamento puntare al luogo sbagliato, quindi non funzionerebbe se cliccato. Deve fare attenzione!

## Migliori pratiche per i collegamenti

Esistono alcune migliori pratiche da seguire quando si scrivono collegamenti. Vediamo di cosa si tratta ora.

### Utilizzare un linguaggio chiaro per i collegamenti

È facile inserire collegamenti sulla sua pagina. Questo non basta. Dobbiamo rendere i nostri collegamenti _accessibili_ a tutti i lettori, indipendentemente dal loro contesto attuale e dagli strumenti che preferiscono. Ad esempio:

- Gli utenti di lettori di schermo amano saltare da un collegamento all'altro nella pagina e leggere i collegamenti fuori contesto.
- I motori di ricerca usano il testo dei collegamenti per indicizzare i file di destinazione, quindi è una buona idea includere parole chiave nel testo del collegamento per descrivere efficacemente cosa è collegato.
- I lettori visivamente sfogliano la pagina piuttosto che leggere ogni parola, e i loro occhi saranno attratti dalle caratteristiche della pagina che si distinguono, come i collegamenti. Troveranno utile del testo descrittivo per i collegamenti.

Vediamo un esempio specifico:

**Buon** testo del collegamento: [Scarica Firefox](https://www.mozilla.org/en-US/firefox/new/?redirect_source=firefox-com)

```html example-good
<p><a href="https://www.mozilla.org/en-US/firefox/new/">Download Firefox</a></p>
```

**Cattivo** testo del collegamento: [Clicca qui](https://www.mozilla.org/en-US/firefox/new/) per scaricare Firefox

```html example-bad
<p>
  <a href="https://www.mozilla.org/en-US/firefox/new/">Click here</a> to
  download Firefox
</p>
```

Altri consigli:

- Non ripetere l'URL come parte del testo del collegamento — gli URL hanno un aspetto brutto e suonano ancora più brutti quando un lettore di schermo li legge lettera per lettera.
- Non dire "collegamento" o "si collega a" nel testo del collegamento — è solo rumore. I lettori di schermo dicono alle persone che c'è un collegamento.
  Anche gli utenti visivi sapranno che c'è un collegamento, poiché generalmente i collegamenti sono stilizzati in un colore diverso e sottolineati (questa convenzione generalmente non dovrebbe essere rotta, poiché gli utenti sono abituati).
- Mantenga il testo del suo collegamento il più breve possibile — questo è utile perché i lettori di schermo devono interpretare tutto il testo del collegamento.
- Minimizzare le istanze in cui più copie dello stesso testo sono collegate a luoghi diversi.
  Questo può causare problemi per gli utenti di lettori di schermo, se c'è un elenco di collegamenti fuori contesto che sono etichettati "clicca qui", "clicca qui", "clicca qui".

### Collegarsi a risorse non HTML — lasciare chiari indicatori

Quando si collega a una risorsa che non verrà aperta nella pagina corrente come una "navigazione normale", dovrebbe aggiungere testo chiaro al testo del collegamento su ciò che accadrà. Ad esempio, se sta scaricando o trasmettendo in streaming una risorsa, o se il collegamento aprirà un popup o eseguirà qualche altro effetto potenzialmente inaspettato, questo dovrebbe essere indicato nel testo. Questo è importante per gli utenti con connessioni a bassa larghezza di banda, che potrebbero voler evitare di scaricare risorse di diversi megabyte. Aiuta anche a creare aspettative per gli utenti di lettori di schermo, che potrebbero non essere consapevoli di ciò che sta accadendo altrimenti.

Diamo un'occhiata ad alcuni esempi, per vedere che tipo di testo può essere utilizzato qui:

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

### Usare l'attributo download quando si collega a uno scaricamento

Quando si collega a una risorsa che deve essere scaricata piuttosto che aperta nel browser, è possibile utilizzare l'attributo `download` per fornire un nome file predefinito per il salvataggio. Ecco un esempio con un collegamento per lo scaricamento dell'ultima versione di Firefox per Windows:

```html
<a
  href="https://download.mozilla.org/?product=firefox-latest-ssl&os=win64&lang=en-US"
  download="firefox-latest-64bit-installer.exe">
  Download Latest Firefox for Windows (64-bit) (English, US)
</a>
```

### Quando aprire i collegamenti in una nuova scheda

I collegamenti per impostazione predefinita si aprono nella stessa scheda della pagina su cui si trovano, il che consente all'utente di tornare alla pagina precedente utilizzando il pulsante indietro del browser. Tuttavia, molti siti (incluso MDN) scelgono di aprire alcuni collegamenti, specialmente i collegamenti esterni, in una nuova scheda. Questo è reso possibile impostando l'attributo [`target`](/it/docs/Web/HTML/Reference/Elements/a#target) su `"_blank"`.

```html
Firefox is developed by the
<a href="https://www.mozilla.org/en-US/" target="_blank">Mozilla Foundation</a>.
```

Aprire o meno i collegamenti in una nuova scheda dovrebbe essere una decisione consapevole, basata su considerazioni di esperienza utente. Ecco alcune cose a cui pensare:

- Aprire collegamenti in una nuova scheda presenta i due documenti contemporaneamente, il che è utile per un'esperienza di navigazione "parallela". D'altra parte, i collegamenti che si aprono nella stessa scheda sono più una continuazione della pagina corrente.
- Aprire collegamenti in una nuova scheda può essere disorientante per gli utenti abituati a usare il pulsante indietro.
- Anche quando i collegamenti sono aperti di default nella stessa scheda, gli utenti possono comunque scegliere di aprirli in una nuova scheda, utilizzando scorciatoie da tastiera o opzioni dal menu contestuale. D'altra parte, i collegamenti che si aprono in una nuova scheda sono difficili da aprire nella stessa scheda.
- Gli utenti di lettori di schermo possono essere confusi da collegamenti che si aprono in una nuova scheda, poiché potrebbero non rendersi conto che la nuova scheda è stata aperta, e potrebbero perdere il contesto sulla loro posizione nella pagina.

Un approccio comune consiste nell'aprire i collegamenti esterni in nuove schede e i collegamenti interni nella stessa scheda.
Alcuni designer preferiscono aprire tutti i collegamenti nella stessa scheda.
Se si aprono collegamenti in nuove schede, è consigliabile fornire indicazioni per questi collegamenti, ad esempio un'icona accanto al testo del collegamento.

## Apprendimento attivo: creare un menu di navigazione

Per questo esercizio, le chiediamo di collegare alcune pagine con un menu di navigazione per creare un sito web a più pagine. Questo è un modo comune in cui un sito web viene creato — la stessa struttura della pagina viene utilizzata per ogni pagina, incluso lo stesso menu di navigazione, quindi quando i collegamenti vengono cliccati, dà l'impressione che stia rimanendo nello stesso luogo, e viene portato su un contenuto diverso.

Dovrà fare copie locali delle seguenti quattro pagine, tutte nella stessa directory. Per un elenco completo dei file, veda la directory [navigation-menu-start](https://github.com/mdn/learning-area/tree/main/html/introduction-to-html/navigation-menu-start):

- [index.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/navigation-menu-start/index.html)
- [projects.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/navigation-menu-start/projects.html)
- [pictures.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/navigation-menu-start/pictures.html)
- [social.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/navigation-menu-start/social.html)

Dovrebbe:

1. Aggiungere un elenco non ordinato nel luogo indicato su una pagina che include i nomi delle pagine a cui collegarsi.
   Un menu di navigazione è generalmente solo un elenco di collegamenti, quindi questo è semanticamente corretto.
2. Cambiare ogni nome di pagina in un collegamento a quella pagina.
3. Copiare il menu di navigazione su ogni pagina.
4. Su ogni pagina, rimuova solo il collegamento a quella stessa pagina — è confuso e non necessario che una pagina includa un collegamento a se stessa.
   E, la mancanza di un collegamento agisce come un buon promemoria visivo di quale pagina si trova attualmente.

L'esempio finito dovrebbe apparire simile alla seguente pagina:

![Un esempio di un semplice menu di navigazione HTML, con elementi del menu home, immagini, progetti e social](navigation-example.png)

> [!NOTE]
> Se si blocca o non è sicuro di aver fatto bene, può controllare la directory [navigation-menu-marked-up](https://github.com/mdn/learning-area/tree/main/html/introduction-to-html/navigation-menu-marked-up) per vedere la risposta corretta.

## Collegamenti email

È possibile creare collegamenti o pulsanti che, quando cliccati, aprono un nuovo messaggio email in uscita anziché collegarsi a una risorsa o pagina.
Questo è fatto utilizzando l'elemento {{HTMLElement("a")}} e lo schema URL `mailto:`.

Nella sua forma più semplice e comunemente usata, un collegamento `mailto:` indica l'indirizzo email del destinatario previsto. Ad esempio:

```html
<a href="mailto:nowhere@mozilla.org">Send email to nowhere</a>
```

Questo risulta in un collegamento che appare così: [Invia email a nowhere](mailto:nowhere@mozilla.org).

In effetti, l'indirizzo email è opzionale. Se lo omette e il suo [`href`](/it/docs/Web/HTML/Reference/Elements/a#href) è "mailto:", si aprirà una nuova finestra di email in uscita dal client di posta elettronica dell'utente senza indirizzo di destinazione.
Questo è spesso utile come collegamenti "Condividi" che gli utenti possono cliccare per inviare un'email a un indirizzo a loro scelta.

### Specificare i dettagli

Oltre all'indirizzo email, può fornire altre informazioni. In effetti, qualsiasi campo intestazione email standard può essere aggiunto all'URL `mailto` che fornisce.
I più comunemente usati sono "subject", "cc", e "body" (che non è un vero campo intestazione, ma le permette di specificare un breve messaggio di contenuto per la nuova email).
Ogni campo e il suo valore è specificato come termine di query.

Ecco un esempio che include un cc, bcc, subject e body:

```html
<a
  href="mailto:nowhere@mozilla.org?cc=name2@rapidtables.com&bcc=name3@rapidtables.com&subject=The%20subject%20of%20the%20email&body=The%20body%20of%20the%20email">
  Send mail with cc, bcc, subject and body
</a>
```

> [!NOTE]
> I valori di ciascun campo devono essere codificati nell'URL con caratteri non stampabili (caratteri invisibili come tabulazioni, ritorni a capo, e interruzioni di pagina) e spazi {{Glossary("Percent-encoding", "percentualmente codificati")}}.
> Inoltre, si noti l'uso del punto interrogativo (`?`) per separare l'URL principale dai valori dei campi, e le e commerciali (&) per separare ciascun campo nell'URL `mailto:`.
> Questa è la notazione di query URL standard.
> Legga [Il metodo GET](/it/docs/Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data#the_get_method) per capire per cosa è comunemente usata la notazione di query URL.

Ecco alcuni altri URL `mailto` di esempio:

- <mailto:>
- <mailto:nowhere@mozilla.org>
- <mailto:nowhere@mozilla.org,nobody@mozilla.org>
- <mailto:nowhere@mozilla.org?cc=nobody@mozilla.org>
- <mailto:nowhere@mozilla.org?cc=nobody@mozilla.org&subject=Questo%20è%20il%20soggetto>

## Metti alla prova le tue abilità!

Ha raggiunto la fine di questo articolo, ma può ricordare le informazioni più importanti? Può trovare alcuni ulteriori test per verificare di aver assimilato queste informazioni prima di andare avanti — veda [Test your skills: Links](/it/docs/Learn_web_development/Core/Structuring_content/Test_your_skills/Links).

## Sommario

Questo è tutto per quanto riguarda i collegamenti, almeno per ora! Tornerà ai collegamenti più avanti nel corso quando inizierà a guardarli dal punto di vista stilistico. Il prossimo passo per HTML, lavorerà attraverso un paio di sfide che metteranno alla prova la sua comprensione degli argomenti trattati finora.

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Advanced_text_features", "Learn_web_development/Core/Structuring_content/Marking_up_a_letter", "Learn_web_development/Core/Structuring_content")}}
