---
title: Creare collegamenti
slug: Learn_web_development/Core/Structuring_content/Creating_links
l10n:
  sourceCommit: 0d59135676db5a372b4dd692f0686e6bdfc13b51
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Structuring_documents", "Learn_web_development/Core/Structuring_content/Test_your_skills/Links", "Learn_web_development/Core/Structuring_content")}}

I collegamenti (noti anche come hyperlink) sono davvero importanti: sono ciò che rende il Web _un web_.
Questo articolo mostra la sintassi necessaria per creare un collegamento e tratta le buone pratiche per i collegamenti.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con HTML, come illustrato in
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi HTML di base</a
        >. Semantica a livello di testo, come <a href="/it/docs/Learn_web_development/Core/Structuring_content/Headings_and_paragraphs"
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
          <li>Comprendere perché i collegamenti sono la caratteristica fondamentale del web. Non esiste web senza collegamenti.</li>
          <li>L'attributo <code>href</code>.</li>
          <li>Percorsi assoluti e relativi e quando usarli.</li>
          <li>Sintassi dei percorsi in dettaglio: barre, punto singolo e doppio punto.</li>
          <li>Stati dei collegamenti e perché sono importanti: <code>:hover</code>, <code>:focus</code>, <code>:visited</code> e <code>:active</code>.</li>
          <li>Collegamenti inline e a livello di blocco.</li>
          <li>Comprendere i vantaggi di scrivere un buon testo per i collegamenti, come una migliore accessibilità per gli utenti di screen reader e potenziali effetti positivi sulla SEO.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Che cos'è un hyperlink?

Gli hyperlink sono funzionalità di un documento HTML che, quando vengono selezionati o altrimenti attivati, fanno sì che il browser navighi verso altri documenti o risorse, talvolta verso parti specifiche di documenti.
Gli hyperlink sono una delle innovazioni più interessanti offerte dal Web.
Sono una caratteristica del Web fin dall'inizio e sono ciò che rende il Web _un web_.
Ogni risorsa sul web ha un indirizzo, noto come {{Glossary("URL", "URL")}} (Uniform Resource Locator), al quale puntano gli hyperlink.

> [!NOTE]
> Un URL può puntare a file HTML, file di testo, immagini, documenti di testo, file video e audio o qualsiasi altra cosa presente sul Web.
> Se il browser web non sa come visualizzare o gestire il file, chiederà se si desidera aprire il file (nel qual caso il compito di aprire o gestire il file viene affidato a un'app nativa adatta sul dispositivo) oppure scaricare il file (nel qual caso sarà possibile provare a gestirlo in seguito).

Per esempio, la home page della BBC contiene molti collegamenti che puntano non solo a numerosi articoli di notizie, ma anche a diverse aree del sito (funzionalità di navigazione), alle pagine di accesso/registrazione (strumenti utente) e altro ancora.

![pagina iniziale di bbc.co.uk, che mostra molti articoli di notizie e la funzionalità del menu di navigazione](updated-bbc-website.png)

## Anatomia di un collegamento

Un collegamento di base viene creato racchiudendo il testo o altro contenuto all'interno di un elemento {{htmlelement("a")}} e usando l'attributo [`href`](/it/docs/Web/HTML/Reference/Elements/a#href), noto anche come **Hypertext Reference** o **target**, che contiene l'indirizzo web.

```html
<p>
  I'm creating a link to
  <a href="https://www.mozilla.org/en-US/">the Mozilla homepage</a>.
</p>
```

Questo produce il seguente risultato:

Sto creando un collegamento alla [home page di Mozilla](https://www.mozilla.org/en-US/).

> [!NOTE]
> Lo scrim [Anchor tags](https://scrimba.com/learn-html-and-css-c0p/~0a?via=mdn) di Scrimba <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> offre una dimostrazione interattiva di come creare collegamenti usando HTML, oltre a una sfida per creare collegamenti personalizzati.

### Collegamenti a livello di blocco

Come già accennato, quasi ogni contenuto può diventare un collegamento, persino gli {{Glossary("Block/CSS", "elementi a livello di blocco")}}.
Per rendere un elemento titolo un collegamento, racchiuderlo in un elemento anchor (`<a>`), come mostrato nel seguente frammento di codice:

```html
<a href="https://developer.mozilla.org/en-US/">
  <h1>MDN Web Docs</h1>
</a>
<p>
  Documenting web technologies, including CSS, HTML, and JavaScript, since 2005.
</p>
```

Questo trasforma il titolo in un collegamento:
{{EmbedLiveSample('Block level links', '100%', 150)}}

### Collegamenti sulle immagini

Per trasformare un'immagine in un collegamento, racchiudere l'elemento {{htmlelement("img")}} con un elemento {{htmlelement("a")}}. L'esempio seguente usa un percorso relativo per fare riferimento a un file di immagine SVG archiviato localmente.

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
> In un prossimo articolo verranno illustrate ulteriori informazioni sull'uso delle immagini sul Web.

### Aggiungere informazioni di supporto con l'attributo title

Potrebbe anche essere opportuno aggiungere un attributo `title` ai collegamenti.
Il titolo contiene informazioni aggiuntive sul collegamento, ad esempio quale tipo di informazioni contiene la pagina o aspetti di cui tenere conto sul sito web.

```html
<p>
  I'm creating a link to
  <a
    href="https://www.mozilla.org/en-US/"
    title="The best place to find more information about Mozilla's mission and how to contribute">
    the Mozilla homepage</a
  >.
</p>
```

Questo produce il seguente risultato e, passando il puntatore sul collegamento, il titolo viene visualizzato come tooltip:

{{EmbedLiveSample('Adding supporting information with the title attribute', '100%', 150)}}

> [!NOTE]
> Il titolo di un collegamento viene mostrato solo al passaggio del mouse, quindi le persone che si affidano ai controlli da tastiera o ai touchscreen per navigare nelle pagine web avranno difficoltà ad accedere alle informazioni del titolo.
> Se le informazioni di un titolo sono realmente importanti per l'usabilità della pagina, devono essere presentate in un modo accessibile a tutti gli utenti, ad esempio inserendole nel testo normale.

### Creare esempi di collegamenti personalizzati

Bene, ora tocca a te!

1. Fai clic su **"Play"** nel blocco di codice seguente per modificare l'esempio nel MDN Playground, oppure crea una copia del nostro [template introduttivo](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/getting-started/index.html) e copia il codice seguente al suo interno.
2. Collega i testi "Red squirrel" e "Eastern gray squirrel" alle pagine di Wikipedia che descrivono le specie corrispondenti. Assegna a ciascun collegamento un attributo `title` uguale al nome scientifico della specie.
3. Collega il testo "Wikipedia Squirrel page" alla pagina principale di Wikipedia sugli scoiattoli.

In caso di errore, è possibile cancellare il lavoro usando il pulsante _Reset_ nel MDN Playground. Se si rimane bloccati, è possibile vedere la soluzione sotto il blocco di codice.

```html live-sample___links-1
<h1>Squirrels</h1>

<p>
  Squirrels are commonly thought of as tree-dwelling mammals, but the squirrel
  family extends far beyond that to include ground-dwelling rodents such as
  chipmunks and prairie dogs, and flying squirrels.
</p>

<p>Two of the most common and best-known squirrel species are the:</p>

<ul>
  <li>Red squirrel</li>
  <li>Eastern gray squirrel</li>
</ul>

<p>
  For a good starting point on squirrel information, see the Wikipedia Squirrel
  page.
</p>
```

{{ EmbedLiveSample('links-1', "100%", 280) }}

<details>
<summary>Fai clic qui per mostrare la soluzione</summary>

L'HTML completato dovrebbe essere simile al seguente:

```html
<h1>Squirrels</h1>

<p>
  Squirrels are commonly thought of as tree-dwelling mammals, but the squirrel
  family extends far beyond that to include ground-dwelling rodents such as
  chipmunks and prairie dogs, and flying squirrels.
</p>

<p>Two of the most common and best-known squirrel species are the:</p>

<ul>
  <li>
    <a
      href="https://en.wikipedia.org/wiki/Red_squirrel"
      title="Sciurus vulgaris">
      Red squirrel
    </a>
  </li>
  <li>
    <a
      href="https://en.wikipedia.org/wiki/Eastern_gray_squirrel"
      title="Sciurus carolinensis">
      Eastern gray squirrel
    </a>
  </li>
</ul>

<p>
  For a good starting point on squirrel information, see the
  <a href="https://en.wikipedia.org/wiki/Squirrel">Wikipedia Squirrel page</a>.
</p>
```

</details>

## Breve introduzione a URL e percorsi

Le destinazioni dei collegamenti sono URL. Un URL, o Uniform Resource Locator, è una stringa di testo che definisce dove si trova qualcosa sul Web. Per esempio, la home page inglese di Mozilla si trova in `https://www.mozilla.org/en-US/`.

Un [web server](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_web_server) riceve richieste per URL e risponde con la risorsa appropriata. La maggior parte delle risorse è archiviata come file nel file system del server, quindi gli URL per tali risorse assomigliano spesso a percorsi di file.

> [!NOTE]
> I percorsi di file e gli URL non sono la stessa cosa, ma per ora ne parleremo come se lo fossero per facilitare la comprensione. Le differenze saranno approfondite nella sezione [come vengono tradotti gli URL in percorsi di file?](#how_do_urls_translate_into_file_paths).

Osserviamo un esempio di struttura di directory su un server:

![Una semplice struttura di directory. La directory padre è chiamata creating-hyperlinks e contiene due file chiamati index.html e contacts.html, e due directory chiamate projects e pdfs, che contengono rispettivamente un file index.html e un file project-brief.pdf](simple-directory.png)

La **root** di questa struttura di directory è chiamata `creating-hyperlinks`. Quando si lavora localmente con un sito web, esiste una directory che contiene l'intero sito. All'interno della **root** sono presenti un file `index.html` e un file `contacts.html`. In un sito web reale, `index.html` sarebbe la home page o landing page (una pagina web che funge da punto di ingresso per un sito web o per una particolare sezione di un sito web).

All'interno della root sono presenti anche due directory: `pdfs` e `projects`. Entrambe contengono un singolo file: rispettivamente un PDF (`project-brief.pdf`) e un file `index.html`. È possibile avere più file `index.html` in un progetto, purché si trovino in posizioni diverse del file system. Il secondo `index.html` potrebbe essere la landing page principale per le informazioni relative ai progetti.

Osserviamo alcuni esempi di collegamenti tra diversi file in questa struttura di directory per dimostrare i diversi tipi di percorso.

### Stessa directory

Se si desidera includere un hyperlink all'interno del file `index.html` di livello superiore che punta a `contacts.html`, è possibile specificare il percorso semplicemente come nome del file da collegare, perché si trova nella stessa directory del file corrente. L'URL da usare è `contacts.html`:

```html
<p>
  Want to contact a specific staff member? Find details on our
  <a href="contacts.html">contacts page</a>.
</p>
```

È anche possibile iniziare un percorso a un file nella stessa directory usando un singolo punto seguito da una barra: `./`. L'esempio seguente è equivalente al precedente, ma alcune persone preferiscono includere comunque `./` perché ritengono che fornisca maggiore chiarezza:

```html
<p>
  Want to contact a specific staff member? Find details on our
  <a href="./contacts.html">contacts page</a>.
</p>
```

> [!NOTE]
> Esistono alcuni casi in cui includere `./` nel percorso fa differenza, ad esempio quando si specificano percorsi per le importazioni di [moduli JavaScript](/it/docs/Web/JavaScript/Guide/Modules), ma non è necessario preoccuparsene per i collegamenti HTML e CSS statici.

### Scendere nelle sottodirectory

Se si desidera includere un hyperlink all'interno del file `index.html` di livello superiore che punta a `projects/index.html`, è necessario scendere nella directory `projects` prima di indicare il file da collegare. Ciò si ottiene specificando il nome della directory, quindi una barra, quindi il nome del file. L'URL utilizzabile è `projects/index.html`:

```html
<p>Visit my <a href="projects/index.html">project homepage</a>.</p>
```

### Risalire nelle directory padre

Se si desidera includere un hyperlink all'interno di `projects/index.html` che punta a `pdfs/project-brief.pdf`, occorre risalire di un livello nella directory, quindi scendere nuovamente nella directory `pdfs`. Per risalire di una directory, si usano due punti: `..`; quindi l'URL è `../pdfs/project-brief.pdf`:

```html
<p>A link to my <a href="../pdfs/project-brief.pdf">project brief</a>.</p>
```

> [!NOTE]
> Se necessario, è possibile combinare più occorrenze di queste funzionalità in percorsi complessi, ad esempio: `../../../complex/path/to/my/file.html`.

### Collegare relativamente alla directory root

Gli URL precedenti funzionano, ma occorre tenere presente che spostando il file contenente il collegamento o il file collegato, il collegamento si interrompe.

Se si desidera creare un collegamento a una posizione specifica che non si interrompa spostando il file contenente il collegamento, è possibile farlo inserendo una singola barra all'inizio del percorso: ciò indica che il percorso inizia nella directory root del sito. Per esempio, il collegamento precedente all'interno di `projects/index.html` potrebbe essere riscritto come:

```html
<p>A link to my <a href="/pdfs/project-brief.pdf">project brief</a>.</p>
```

Ora il percorso inizierà sempre dalla directory root (`creating-hyperlinks`), raggiungerà la directory `pdfs` e troverà il file `project-brief.pdf`. Questo continuerà a funzionare anche se il file contenente il collegamento viene spostato in una posizione diversa, ad esempio `a/b/c/d/e/index.html`.

Se si sposta il file collegato `project-brief.pdf` in una posizione diversa, il collegamento continuerà invece a interrompersi.

Due termini che si incontrano sul web sono **percorso assoluto** e **percorso relativo**.

- Percorso assoluto: punta a una posizione definita dalla sua posizione assoluta nel sito (o altrove sul web). Per esempio, è possibile creare un collegamento assoluto che punti sempre alla stessa posizione rispetto alla directory root del sito usando la singola barra all'inizio del percorso, come visto in precedenza: `/pdfs/project-brief.pdf`.
- Percorso relativo: punta a una posizione _relativa_ al file dal quale si sta creando il collegamento. Nell'esempio precedente, è stato usato `projects/index.html` per creare un collegamento relativo tra il file corrente e un file chiamato `index.html` che si trova in una sottodirectory `projects`. Spostando il file corrente in una posizione diversa, il percorso resterebbe relativo a quel file, ma punterebbe a una posizione assoluta diversa.

Questi termini non vengono sempre usati in modo coerente. Per esempio, `/pdfs/project-brief.pdf` è assoluto rispetto alla posizione del file corrente, ma relativo al [nome di dominio](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_domain_name). Un URL che include il nome di dominio completo, come `https://example.com/pdfs/project-brief.pdf`, è assoluto rispetto all'intero web.

### Collegare con URL completi

È possibile specificare un URL completo come percorso, che punterà sempre alla stessa posizione sul web, indipendentemente da dove viene usato. Per esempio:

```html
<a href="https://www.example.com/projects/">projects</a>
```

Questo collegamento punterà sempre a `https://www.example.com/projects/`, anche se il sito viene spostato su un dominio diverso.

### Collegamenti interni ed esterni

Quando un collegamento punta a una risorsa sul _proprio_ sito, viene definito **collegamento interno**. Quando un collegamento punta a una risorsa su un sito _diverso_, viene chiamato **collegamento esterno**.

Quando si specifica un collegamento esterno, è sempre necessario includere l'URL completo come percorso, per esempio:

```html
<a href="https://www.some-other-site.com">projects</a>
```

Non è possibile fare riferimento a una posizione su un sito diverso con un percorso come `/pdfs/project-brief.pdf` o `projects/index.html`, poiché entrambi sono relativi a una posizione sul proprio sito e il browser necessita del nome di dominio del sito web per poterlo trovare.

Quando si specifica un collegamento interno, è possibile usare un percorso relativo o assoluto, oppure un URL completo. Nell'esempio, questi collegamenti sono equivalenti:

```html
<a href="https://www.example.com/projects/">projects</a>

<a href="projects">projects</a>
```

Si consiglia il secondo senza il nome di dominio completo, per ragioni di portabilità. Come già detto, specificando `https://www.example.com/projects/`, il collegamento punterà sempre a `https://www.example.com/projects/`. Se il sito web viene poi spostato su un dominio diverso, ad esempio `another-example.com`, tutti i collegamenti con URL completi dovranno essere modificati. Specificando percorsi come `/projects`, essi continueranno a funzionare, poiché sono ancora relativi alla struttura delle directory.

### Frammenti di documento

È possibile creare un collegamento a una parte specifica di un documento HTML, nota come **frammento di documento**, invece che solo alla parte superiore del documento.
Gli elementi con un attributo [`id`](/it/docs/Web/HTML/Reference/Global_attributes/id) nel documento creano automaticamente un frammento di documento al quale è possibile collegarsi.

Il caso d'uso più tipico è il collegamento a un titolo specifico, che appare così:

```html
<h2 id="mailing_address">Mailing address</h2>
```

Per collegarsi a quello specifico `id`, includerlo alla fine del percorso, preceduto da un simbolo hash/cancelletto (`#`), per esempio:

```html
<p>
  Want to write us a letter? Use our
  <a href="contacts.html#mailing_address">mailing address</a>.
</p>
```

È persino possibile usare da solo il riferimento al frammento di documento per collegarsi a _un'altra parte del documento corrente_:

```html
<p>
  The <a href="#mailing_address">company mailing address</a> can be found at the
  bottom of this page.
</p>
```

### Come vengono tradotti gli URL in percorsi di file?

Tutte le destinazioni dei collegamenti viste finora sono _URL_, che vengono elaborati da un web server per trovare la risorsa pertinente.
**Nessun contenuto web può vedere direttamente il file system del server.**

L'esempio di server osservato finora crea un {{Glossary("SSG", "sito web statico")}}.
Il server prende semplicemente la parte [pathname](/it/docs/Web/API/URL/pathname) dell'URL e cerca direttamente il file corrispondente nel proprio file system.

> [!NOTE]
> Molti server generano il contenuto per un URL al momento, anziché recuperarlo da un file statico. Se si usa un [web framework](/it/docs/Learn_web_development/Core/Frameworks_libraries), anche la directory del codice sorgente potrebbe essere molto diversa da ciò che viene distribuito sul server. Quando si lavora con un sito web personale, è necessario comprendere gli strumenti di build e la configurazione del server per sapere come gli URL vengono mappati ai file sorgente.

Se si avvia un web server (vedere [Come configurare un server di test locale?](/it/docs/Learn_web_development/Howto/Tools_and_setup/set_up_a_local_testing_server)) usando come root la directory del sito di esempio e il {{Glossary("domain_name", "nome di dominio")}} del sito web è impostato su `example.com`, il file `pdfs/project-brief.pdf` sarà disponibile in `https://www.example.com/pdfs/project-brief.pdf`.

Tutti i collegamenti vengono risolti rispetto all'URL del documento corrente, quindi:

- Per tutte le pagine del dominio `https://example.com`, un collegamento a `/pdfs/project-brief.pdf` crea sempre un collegamento a `https://www.example.com/pdfs/project-brief.pdf`, il cui pathname è `/pdfs/project-brief.pdf`. Il server cerca la directory `pdfs` nella directory root, quindi cerca il file `project-brief.pdf` all'interno di tale directory.
- Un collegamento a `projects/index.html` creerebbe un collegamento a `https://www.example.com/projects/index.html`, ma solo quando incluso in un file nella directory root, come il file `index.html` di livello superiore o `contacts.html`. Se venisse incluso, per esempio, in un file HTML in `pdfs/index.html`, punterebbe a `https://www.example.com/pdfs/projects/index.html`, il cui percorso è `/pdfs/projects/index.html`, che non esiste; si finirebbe quindi con un collegamento interrotto.

#### La pagina `index.html` predefinita

Quando si fa riferimento a un file `index.html`, in genere non è necessario includere `index.html` nell'URL/percorso, poiché i web server cercano una landing page predefinita chiamata `index.html` quando non viene specificato un nome file.

Riprendendo l'esempio di percorso `projects/index.html`, è possibile scrivere semplicemente il percorso come `projects`, creando così un collegamento a `https://www.example.com/projects/index.html`. Durante la navigazione alla pagina, è possibile scrivere l'URL come `https://www.example.com/projects/` e si raggiungerà comunque il posto corretto.

> [!NOTE]
> La barra finale (`/`) alla fine dell'URL è importante. In sua presenza, un collegamento relativo a `contacts.html` all'interno di `projects/index.html` verrà risolto in `https://www.example.com/projects/contacts.html` (che si trova nella stessa directory). Senza di essa, l'URL verrebbe trattato come un file e il collegamento relativo verrebbe risolto in `https://www.example.com/contacts.html` (che si trova una directory più in alto).
>
> [Diversi web server gestiscono un URL come `https://www.example.com/projects` in modi diversi](https://github.com/slorber/trailing-slash-guide): alcuni reindirizzano automaticamente all'URL con una barra finale, mentre altri servono lo stesso `index.html` senza reindirizzare. Quest'ultimo comportamento può interrompere i collegamenti relativi.

## Buone pratiche per i collegamenti

Esistono alcune buone pratiche da seguire durante la scrittura dei collegamenti. Vediamole ora.

### Usare una formulazione chiara per i collegamenti

È facile inserire collegamenti in una pagina. Ma non basta. Occorre rendere i collegamenti _accessibili_ a tutti i lettori, indipendentemente dal loro contesto attuale e dagli strumenti che preferiscono. Per esempio:

- Gli utenti di screen reader preferiscono spostarsi da un collegamento all'altro nella pagina e leggere i collegamenti fuori contesto.
- I motori di ricerca usano il testo dei collegamenti per indicizzare i file di destinazione, quindi è una buona idea includere parole chiave nel testo del collegamento per descrivere efficacemente ciò a cui si collega.
- I lettori visivi scorrono la pagina invece di leggere ogni parola e gli occhi sono attratti dalle caratteristiche della pagina che risaltano, come i collegamenti. Troveranno utile un testo descrittivo per i collegamenti.

Osserviamo un esempio specifico:

Testo del collegamento **buono**: [Scarica Firefox](https://www.firefox.com/en-US/?redirect_source=firefox-com)

```html example-good
<p><a href="https://www.firefox.com/en-US/">Download Firefox</a></p>
```

<!-- markdownlint-disable descriptive-link-text -->

Testo del collegamento **cattivo**: [Fai clic qui](https://www.firefox.com/en-US/) per scaricare Firefox

```html example-bad
<p>
  <a href="https://www.firefox.com/en-US/">Click here</a> to download Firefox
</p>
```

<!-- markdownlint-enable descriptive-link-text -->

Altri suggerimenti:

- Non ripetere l'URL come parte del testo del collegamento: gli URL hanno un brutto aspetto e suonano ancora peggio quando uno screen reader li legge lettera per lettera.
- Non dire "collegamento" o "collega a" nel testo del collegamento: è solo rumore. Gli screen reader comunicano alle persone che è presente un collegamento.
  Anche gli utenti visivi sapranno che è presente un collegamento, perché i collegamenti sono generalmente stilizzati con un colore diverso e sottolineati (questa convenzione generalmente non dovrebbe essere infranta, poiché gli utenti vi sono abituati).
- Mantenere il testo del collegamento il più breve possibile: è utile perché gli screen reader devono interpretare l'intero testo del collegamento.
- Ridurre al minimo i casi in cui più copie dello stesso testo sono collegate a luoghi diversi.
  Questo può causare problemi agli utenti di screen reader, se è presente un elenco di collegamenti fuori contesto etichettati "fai clic qui", "fai clic qui", "fai clic qui".

### Collegare a risorse non HTML: lasciare indicazioni chiare

Quando si collega a una risorsa che non verrà aperta nella pagina corrente come una "navigazione normale", occorre aggiungere al testo del collegamento una formulazione chiara su ciò che sta per accadere. Per esempio, se si sta scaricando o trasmettendo in streaming una risorsa, oppure se il collegamento aprirà un popup o produrrà un altro effetto potenzialmente inatteso, ciò deve essere indicato nel testo. Questo è importante per gli utenti con connessioni a bassa larghezza di banda, che potrebbero voler evitare di scaricare risorse di molti megabyte. Aiuta anche a impostare le aspettative degli utenti di screen reader, che altrimenti potrebbero non essere consapevoli di ciò che sta accadendo.

Osserviamo alcuni esempi, per vedere quale tipo di testo può essere usato:

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

### Usare l'attributo download quando si collega a un download

Quando si collega a una risorsa da scaricare anziché aprire nel browser, è possibile usare l'attributo `download` per fornire un nome file predefinito per il salvataggio. Ecco un esempio con un collegamento per scaricare l'ultima versione di Firefox per Windows:

```html
<a
  href="https://download.mozilla.org/?product=firefox-latest-ssl&os=win64&lang=en-US"
  download="firefox-latest-64bit-installer.exe">
  Download Latest Firefox for Windows (64-bit) (English, US)
</a>
```

### Quando aprire collegamenti in una nuova scheda

Per impostazione predefinita, i collegamenti si aprono nella stessa scheda della pagina in cui si trovano, consentendo all'utente di tornare alla pagina precedente usando il pulsante Indietro del browser. Tuttavia, molti siti, incluso MDN, scelgono di aprire determinati collegamenti, soprattutto i collegamenti esterni, in una nuova scheda. Ciò avviene impostando l'attributo [`target`](/it/docs/Web/HTML/Reference/Elements/a#target) su `"_blank"`.

```html
Firefox is developed by the
<a href="https://www.mozilla.org/en-US/" target="_blank">Mozilla Foundation</a>.
```

La scelta di aprire o meno i collegamenti in una nuova scheda dovrebbe essere una decisione consapevole, basata su considerazioni di progettazione dell'esperienza utente. Ecco alcuni aspetti da considerare:

- L'apertura dei collegamenti in una nuova scheda presenta i due documenti simultaneamente, utile per un'esperienza di navigazione "parallela". D'altra parte, i collegamenti che si aprono nella stessa scheda assomigliano maggiormente a una continuazione della pagina corrente.
- L'apertura dei collegamenti in una nuova scheda può disorientare gli utenti abituati a usare il pulsante Indietro.
- Anche quando i collegamenti vengono aperti nella stessa scheda per impostazione predefinita, gli utenti possono comunque scegliere di aprirli in una nuova scheda usando scorciatoie da tastiera o opzioni del menu contestuale. Al contrario, i collegamenti che si aprono in una nuova scheda sono difficili da aprire nella stessa scheda.
- Gli utenti di screen reader possono essere confusi dai collegamenti che si aprono in una nuova scheda, poiché potrebbero non rendersi conto che la nuova scheda è stata aperta e potrebbero perdere il contesto relativo alla propria posizione nella pagina.

Un approccio comune consiste nell'aprire i collegamenti esterni in nuove schede e i collegamenti interni nella stessa scheda.
Alcuni progettisti preferiscono aprire tutti i collegamenti nella stessa scheda.
Se si aprono collegamenti in nuove schede, è consigliabile fornire indicazioni per tali collegamenti, ad esempio un'icona accanto al testo del collegamento.

## Creare un menu di navigazione

Per questo esercizio, occorre collegare alcune pagine tramite un menu di navigazione per creare un sito web multipagina. Questo è uno dei modi comuni in cui viene creato un sito web: la stessa struttura di pagina viene usata in ogni pagina, incluso lo stesso menu di navigazione, quindi quando si fa clic sui collegamenti si ha l'impressione di rimanere nello stesso posto mentre viene visualizzato contenuto diverso.

Occorre creare copie locali delle quattro pagine seguenti, tutte nella stessa directory. Per un elenco completo dei file, vedere la directory [navigation-menu-start](https://github.com/mdn/learning-area/tree/main/html/introduction-to-html/navigation-menu-start):

- [index.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/navigation-menu-start/index.html)
- [projects.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/navigation-menu-start/projects.html)
- [pictures.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/navigation-menu-start/pictures.html)
- [social.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/navigation-menu-start/social.html)

Occorre:

1. Aggiungere un elenco non ordinato nel punto indicato di una pagina che includa i nomi delle pagine da collegare.
   Un menu di navigazione è di solito soltanto un elenco di collegamenti, quindi semanticamente va bene.
2. Trasformare ogni nome di pagina in un collegamento a quella pagina.
3. Copiare il menu di navigazione in ciascuna pagina.
4. In ogni pagina, rimuovere soltanto il collegamento alla pagina stessa: è confuso e non necessario che una pagina includa un collegamento a se stessa.
   Inoltre, l'assenza di un collegamento funge da buon promemoria visivo della pagina in cui ci si trova.

L'esempio completato dovrebbe essere simile alla pagina seguente:

![Un esempio di semplice menu di navigazione HTML, con voci di menu home, immagini, progetti e social](navigation-example.png)

> [!NOTE]
> Se si rimane bloccati o non si è sicuri di aver fatto correttamente, è possibile controllare la directory [navigation-menu-marked-up](https://github.com/mdn/learning-area/tree/main/html/introduction-to-html/navigation-menu-marked-up) per vedere la risposta corretta.

## Collegamenti email

È possibile creare collegamenti o pulsanti che, quando selezionati, aprono un nuovo messaggio email in uscita anziché collegarsi a una risorsa o pagina.
Ciò avviene usando l'elemento {{HTMLElement("a")}} e lo schema URL `mailto:`.

Nella forma più semplice e più comunemente usata, un collegamento `mailto:` indica l'indirizzo email del destinatario previsto. Per esempio:

```html
<a href="mailto:nowhere@mozilla.org">Send email to nowhere</a>
```

Il risultato è un collegamento simile a questo: [Invia email a nowhere](mailto:nowhere@mozilla.org).

In realtà, l'indirizzo email è facoltativo. Se viene omesso e [`href`](/it/docs/Web/HTML/Reference/Elements/a#href) è "mailto:", il client email dell'utente aprirà una nuova finestra per email in uscita senza indirizzo di destinazione.
Questo è spesso utile come collegamento "Condividi", sul quale gli utenti possono fare clic per inviare un'email a un indirizzo di loro scelta.

### Specificare i dettagli

Oltre all'indirizzo email, è possibile fornire altre informazioni. Infatti, è possibile aggiungere all'URL `mailto` fornito qualsiasi campo header email standard.
I più comunemente usati sono "subject", "cc" e "body" (che non è un vero campo header, ma consente di specificare un breve messaggio di contenuto per la nuova email).
Ogni campo e il relativo valore vengono specificati come termine di query.

Ecco un esempio che include cc, bcc, subject e body:

```html
<a
  href="mailto:nowhere@mozilla.org?cc=name2@rapidtables.com&bcc=name3@rapidtables.com&subject=The%20subject%20of%20the%20email&body=The%20body%20of%20the%20email">
  Send mail with cc, bcc, subject and body
</a>
```

> [!NOTE]
> I valori di ciascun campo devono essere codificati come URL, con i caratteri non stampabili (caratteri invisibili come tabulazioni, ritorni a capo e interruzioni di pagina) e gli spazi sottoposti a {{Glossary("Percent-encoding", "escape percentuale")}}.
> Notare inoltre l'uso del punto interrogativo (`?`) per separare l'URL principale dai valori dei campi e delle e commerciali (&) per separare ogni campo nell'URL `mailto:`.
> Questa è la notazione standard per le query URL.
> Leggere [Il metodo GET](/it/docs/Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data#the_get_method) per comprendere per cosa viene più comunemente usata la notazione per le query URL.

Ecco alcuni altri esempi di URL `mailto`:

- <mailto:>
- <mailto:nowhere@mozilla.org>
- <mailto:nowhere@mozilla.org,nobody@mozilla.org>
- <mailto:nowhere@mozilla.org?cc=nobody@mozilla.org>
- <mailto:nowhere@mozilla.org?cc=nobody@mozilla.org&subject=This%20is%20the%20subject>

## Riepilogo

Per ora è tutto sui collegamenti! I collegamenti verranno ripresi più avanti nel corso, quando inizierà l'analisi del loro styling. Successivamente verranno proposti alcuni test per verificare quanto bene sono state comprese e memorizzate le informazioni fornite sui collegamenti.

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Structuring_documents", "Learn_web_development/Core/Structuring_content/Test_your_skills/Links", "Learn_web_development/Core/Structuring_content")}}
