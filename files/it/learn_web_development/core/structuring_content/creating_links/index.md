---
title: Creare collegamenti
slug: Learn_web_development/Core/Structuring_content/Creating_links
l10n:
  sourceCommit: 0915a5e602d475bd1a1a57d905f0bac1b7ed57b8
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Advanced_text_features", "Learn_web_development/Core/Structuring_content/Marking_up_a_letter", "Learn_web_development/Core/Structuring_content")}}

I link (noti anche come collegamenti ipertestuali) sono davvero importanti: sono ciò che fa del Web una _rete_.
Questo articolo mostra la sintassi necessaria per creare un link e discute le migliori pratiche sui link.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Conoscenza di base di HTML, come trattato in
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
          <li>Comprendere perché i link sono la caratteristica fondamentale del web. Non c'è web senza link.</li>
          <li>L'attributo <code>href</code>.</li>
          <li>Percorsi assoluti e relativi, e quando utilizzarli.</li>
          <li>Sintassi dei percorsi nel dettaglio — slash, punto singolo e doppio punto.</li>
          <li>Stati del link e perché sono importanti — <code>:hover</code>, <code>:focus</code>, <code>:visited</code>, e <code>:active</code>.</li>
          <li>Link a livello inline e block.</li>
          <li>Comprendere i benefici di scrivere testi di link validi, come una migliore accessibilità per gli utenti di screen reader e potenziali effetti positivi sulla SEO.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Che cos'è un collegamento ipertestuale?

I collegamenti ipertestuali sono una delle innovazioni più entusiasmanti che il Web ha da offrire.
Sono una caratteristica del Web sin dall'inizio e sono ciò che fa del Web _una rete_.
I collegamenti ipertestuali ci permettono di collegare documenti ad altri documenti o risorse, collegare a parti specifiche dei documenti, o rendere disponibili app a un indirizzo web.
Quasi qualsiasi contenuto web può essere convertito in un link in modo che, quando cliccato o altrimenti attivato, il browser web vada a un altro indirizzo web ({{Glossary("URL", "URL")}}).

> [!NOTE]
> Un URL può puntare a file HTML, file di testo, immagini, documenti di testo, file video e audio, o qualsiasi altra cosa che esiste sul Web.
> Se il browser web non sa come visualizzare o gestire il file, chiederà se si desidera aprire il file (in tal caso l'onere di aprire o gestire il file passa a un'app nativa adatta sul dispositivo) o scaricare il file (in tal caso si potrà provare a gestirlo in seguito).

Ad esempio, la homepage della BBC contiene molti link che puntano non solo a molteplici storie di cronaca, ma anche a diverse aree del sito (funzionalità di navigazione), pagine di login/registrazione (strumenti per l'utente) e altro.

![pagina principale di bbc.co.uk, che mostra molti articoli di notizie e una funzionalità del menu di navigazione](updated-bbc-website.png)

## Anatomia di un link

Un link di base è creato incapsulando il testo o altro contenuto all'interno di un elemento {{htmlelement("a")}} e utilizzando l'attributo [`href`](/it/docs/Web/HTML/Reference/Elements/a#href), noto anche come **Hypertext Reference**, o **target**, che contiene l'indirizzo web.

```html
<p>
  I'm creating a link to
  <a href="https://www.mozilla.org/en-US/">the Mozilla homepage</a>.
</p>
```

Questo ci fornisce il seguente risultato:

Sto creando un link alla [homepage di Mozilla](https://www.mozilla.org/en-US/).

> [!CALLOUT]
>
> **Provalo**
>
> Lo scrim di Scrimba sui [tag di ancoraggio](https://scrimba.com/learn-html-and-css-c0p/~0a?via=mdn) <sup>[_partner di apprendimento di MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> offre una dimostrazione interattiva di come creare link usando HTML, oltre a una sfida per creare i tuoi link.

### Link a livello di blocco

Come accennato in precedenza, quasi qualsiasi contenuto può essere trasformato in un link, persino gli [elementi a livello di blocco](/it/docs/Glossary/Block/CSS).
Se si desidera rendere un'intestazione un link, allora incapsularla in un elemento ancoraggio (`<a>`) come mostrato nel seguente esempio di codice:

```html
<a href="https://developer.mozilla.org/en-US/">
  <h1>MDN Web Docs</h1>
</a>
<p>
  Documenting web technologies, including CSS, HTML, and JavaScript, since 2005.
</p>
```

Questo trasforma l'intestazione in un link:
{{EmbedLiveSample('Link a livello di blocco', '100%', 150)}}

### Link alle immagini

Per trasformare un'immagine in un link, incapsulare l'elemento {{htmlelement("img")}} con un elemento {{htmlelement("a")}}. L'esempio seguente utilizza un percorso relativo per riferirsi a un file immagine SVG archiviato localmente.

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

Questo trasforma il logo MDN in un link:
{{EmbedLiveSample('Link alle immagini', '100%', 150)}}

> [!NOTE]
> Troverai maggiori informazioni sull'uso delle immagini sul Web in un articolo futuro.

### Aggiungere informazioni di supporto con l'attributo title

Un altro attributo che potrebbe essere utile aggiungere ai tuoi link è `title`.
Il titolo contiene informazioni aggiuntive sul link, come il tipo di informazioni che la pagina contiene, o cose da sapere sul sito web.

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

Questo ci fornisce il seguente risultato e passando il mouse sul link viene visualizzato il titolo come tooltip:

{{EmbedLiveSample('Aggiungere informazioni di supporto con l'attributo title', '100%', 150)}}

> [!NOTE]
> Un titolo di link viene rivelato solo al passaggio del mouse, il che significa che le persone che si affidano ai controlli da tastiera o ai touchscreen per navigare nelle pagine web avranno difficoltà ad accedere alle informazioni del titolo.
> Se le informazioni di un titolo sono davvero importanti per l'usabilità della pagina, allora si dovrebbe presentarle in un modo accessibile a tutti gli utenti, ad esempio mettendole nel testo regolare.

### Apprendimento attivo: creare un tuo esempio di link

Crea un documento HTML utilizzando il tuo editor di codice locale e il nostro [template iniziale](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/getting-started/index.html).

- All'interno del corpo HTML, aggiungi uno o più paragrafi o altri tipi di contenuto che già conosci.
- Trasforma parte del contenuto in link.
- Includi attributi title.

## Una breve introduzione a URL e percorsi

Per comprendere appieno i target dei link, è necessario comprendere gli URL e i percorsi dei file. Questa sezione ti fornisce le informazioni di cui hai bisogno per raggiungere questo obiettivo.

Un URL, o Uniform Resource Locator, è una stringa di testo che definisce dove si trova qualcosa sul Web. Ad esempio, la homepage in lingua inglese di Mozilla si trova all'indirizzo `https://www.mozilla.org/en-US/`.

Gli URL utilizzano i percorsi per trovare i file. I percorsi specificano dove si trova nel filesystem il file di interesse. Esaminiamo un esempio di struttura di directory, vedi la struttura della directory `creating-hyperlinks` mostrata di seguito:

![Una semplice struttura di directory. La directory principale si chiama creating-hyperlinks e contiene due file chiamati index.html e contacts.html, e due directory chiamate projects e pdfs, contenenti rispettivamente un file index.html e un file project-brief.pdf](simple-directory.png)

La **radice** di questa struttura di directory si chiama `creating-hyperlinks`. Quando si lavora localmente con un sito web, si ha una directory che contiene l'intero sito. All'interno della **radice**, abbiamo un file `index.html` e un `contacts.html`. In un sito web reale, `index.html` sarebbe la nostra home page o landing page (una pagina web che serve come punto di ingresso per un sito web o una particolare sezione di un sito web).

Ci sono anche due directory all'interno della nostra radice — `pdfs` e `projects`. Queste contengono ciascuna un singolo file, un PDF (`project-brief.pdf`) e un file `index.html`, rispettivamente. Nota che puoi avere due file `index.html` in un progetto, purché siano in diverse posizioni del filesystem. Il secondo `index.html` sarebbe forse la pagina principale per le informazioni relative al progetto.

Esaminiamo alcuni esempi di link tra file diversi in questa struttura di directory per dimostrare diversi tipi di link:

- **Stessa directory**: Se si desidera includere un collegamento ipertestuale all'interno di `index.html` (il `index.html` di livello superiore) puntando a `contacts.html`, basta specificare il nome del file a cui si vuole collegare, perché è nella stessa directory del file corrente. L'URL che useresti è `contacts.html`:

  ```html
  <p>
    Want to contact a specific staff member? Find details on our
    <a href="contacts.html">contacts page</a>.
  </p>
  ```

- **Scendere in sottodirectory**: Se si desidera includere un collegamento ipertestuale all'interno di `index.html` (il `index.html` di livello superiore) puntando a `projects/index.html`, occorre scendere nella directory `projects` prima di indicare il file a cui si vuole collegare.
  Questo si fa specificando il nome della directory, poi uno slash, poi il nome del file. L'URL che useresti è `projects/index.html`:

  ```html
  <p>Visit my <a href="projects/index.html">project homepage</a>.</p>
  ```

- **Risalire in directory genitore**: Se si desidera includere un collegamento ipertestuale all'interno di `projects/index.html` puntando a `pdfs/project-brief.pdf`, è necessario risalire di un livello di directory, poi scendere nella directory `pdfs`.
  Per risalire una directory, usa due punti — `..` — quindi l'URL che useresti è `../pdfs/project-brief.pdf`:

  ```html
  <p>A link to my <a href="../pdfs/project-brief.pdf">project brief</a>.</p>
  ```

> [!NOTE]
> Puoi combinare più istanze di queste funzionalità in URL complessi, se necessario, ad esempio: `../../../complex/path/to/my/file.html`.

### Frammenti di documento

È possibile collegarsi a una parte specifica di un documento HTML, nota come **frammento di documento**, piuttosto che soltanto in cima al documento.
Per fare ciò, devi prima assegnare un attributo [`id`](/it/docs/Web/HTML/Reference/Global_attributes/id) all'elemento a cui vuoi collegarti.
Di solito ha senso collegarsi a un'intestazione specifica, quindi si presenterebbe un qualcosa come il seguente:

```html
<h2 id="Mailing_address">Mailing address</h2>
```

Quindi, per collegarti a quello specifico `id`, lo includeresti alla fine dell'URL, preceduto dal simbolo hash/libra (`#`), ad esempio:

```html
<p>
  Want to write us a letter? Use our
  <a href="contacts.html#Mailing_address">mailing address</a>.
</p>
```

Puoi persino usare il riferimento al frammento di documento da solo per collegarti a _un'altra parte del documento corrente_:

```html
<p>
  The <a href="#Mailing_address">company mailing address</a> can be found at the
  bottom of this page.
</p>
```

### URL assoluti rispetto a URL relativi

Due termini che incontrerai sul web sono URL **assoluto** e URL **relativo**:

**URL assoluto**: Punta a una posizione definita dalla sua posizione assoluta sul web, inclusi {{Glossary("protocol", "protocollo")}} e {{Glossary("domain_name", "nome di dominio")}}.
Ad esempio, se una pagina `index.html` è caricata in una directory chiamata `projects` che si trova all'interno della **radice** di un server web, e il dominio del sito web è `https://www.example.com`, la pagina sarebbe disponibile a `https://www.example.com/projects/index.html` (o anche solo `https://www.example.com/projects/`, dato che la maggior parte dei server web tende a cercare una pagina di atterraggio come `index.html` da caricare se non è specificata nell'URL.)

Un URL assoluto punterà sempre alla stessa posizione, indipendentemente da dove viene utilizzato.

**URL relativo**: Punta a una posizione che è _relativa_ al file da cui stai collegando, più simile a quello che abbiamo visto nella sezione precedente.
Ad esempio, se volessimo collegarci dal nostro file di esempio a `https://www.example.com/projects/index.html` a un file PDF nella stessa directory, l'URL sarebbe semplicemente il nome del file — `project-brief.pdf` — nessuna informazione extra necessaria. Se il PDF fosse disponibile in una sottodirectory all'interno di `projects` chiamata `pdfs`, il link relativo sarebbe `pdfs/project-brief.pdf` (l'URL assoluto equivalente sarebbe `https://www.example.com/projects/pdfs/project-brief.pdf`.)

Un URL relativo punterà a posizioni diverse a seconda della posizione reale del file da cui si fa riferimento — ad esempio se spostassimo il nostro file `index.html` fuori dalla directory `projects` e nella **radice** del sito web (livello superiore, non in alcuna directory), il link relativo `pdfs/project-brief.pdf` al suo interno ora punterebbe a un file situato a `https://www.example.com/pdfs/project-brief.pdf`, non a un file situato a `https://www.example.com/projects/pdfs/project-brief.pdf`.

Ovviamente, la posizione del file `project-brief.pdf` e della cartella `pdfs` non cambierà improvvisamente perché hai spostato il file `index.html` — questo farebbe sì che il tuo link punti al posto sbagliato, quindi non funzionerebbe se cliccato. Deve prestare attenzione!

## Migliori pratiche sui link

Ci sono alcune migliori pratiche da seguire quando si scrivono link. Esaminiamole ora.

### Usa un testo del link chiaro

È facile inserire link nella tua pagina. Ma questo non è sufficiente. Dobbiamo rendere i nostri link _accessibili_ a tutti i lettori, indipendentemente dalla loro situazione attuale e dagli strumenti che preferiscono. Ad esempio:

- Gli utenti di screen reader amano muoversi rapidamente tra i link sulla pagina, e leggere link fuori contesto.
- I motori di ricerca usano il testo del link per indicizzare i file target, quindi è una buona idea includere parole chiave nel tuo testo di link per descrivere in modo efficace ciò a cui ti stai collegando.
- I lettori visivi scorrono la pagina piuttosto che leggere ogni parola, e i loro occhi saranno attratti da caratteristiche della pagina che risaltano, come i link. Troveranno utile un testo del link descrittivo.

Esaminiamo un esempio specifico:

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

- Non ripetere l'URL come parte del testo del link — gli URL appaiono brutti e suonano ancora più brutti quando uno screen reader li legge lettera per lettera.
- Non dire "link" o "link a" nel testo del link — è solo rumore. Gli screen reader dicono già alle persone che c'è un link.
  Anche gli utenti visivi sapranno che c'è un link, perché generalmente i link sono stilizzati in un colore diverso e sottolineati (questa convenzione generalmente non dovrebbe essere interrotta, poiché gli utenti sono abituati).
- Mantieni il tuo testo del link il più breve possibile — questo è utile perché gli screen reader devono interpretare l'intero testo del link.
- Minimizza le occorrenze in cui copie multiple dello stesso testo sono collegate a posti diversi.
  Questo può causare problemi per gli utenti di screen reader, se c'è una lista di link fuori contesto che sono etichettati "clicca qui", "clicca qui", "clicca qui".

### Collegare a risorse non-HTML — lasciare chiari cartelli indicatori

Quando ci si collega a una risorsa che non verrà aperta nella pagina corrente come una "navigazione normale", si dovrebbe aggiungere un chiaro testo sul link su ciò che sta per accadere. Ad esempio, se si sta scaricando o streaming una risorsa, o se il link aprirà un popup o eseguirà qualche altro effetto potenzialmente inaspettato, questo dovrebbe essere detto nel testo. Questo è importante per gli utenti con connessioni a bassa larghezza di banda, che potrebbero voler evitare di scaricare risorse di diversi megabyte. Aiuta anche a impostare le aspettative per gli utenti di screen reader, che altrimenti potrebbero non sapere cosa sta accadendo.

Vediamo alcuni esempi, per capire che tipo di testo può essere usato qui:

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

### Usa l'attributo download quando ci si collega a un download

Quando ci si collega a una risorsa da scaricare piuttosto che da aprire nel browser, è possibile utilizzare l'attributo `download` per fornire un nome di file di salvataggio predefinito. Ecco un esempio di un link di download all'ultima versione di Firefox per Windows:

```html
<a
  href="https://download.mozilla.org/?product=firefox-latest-ssl&os=win64&lang=en-US"
  download="firefox-latest-64bit-installer.exe">
  Download Latest Firefox for Windows (64-bit) (English, US)
</a>
```

### Quando aprire link in una nuova scheda

I link per impostazione predefinita si aprono nella stessa scheda della pagina in cui si trovano, il che consente all'utente di tornare alla pagina precedente utilizzando il pulsante indietro del browser. Tuttavia, molti siti (incluso MDN) scelgono di aprire alcuni link, specialmente i link esterni, in una nuova scheda. Questo si fa impostando l'attributo [`target`](/it/docs/Web/HTML/Reference/Elements/a#target) su `"_blank"`.

```html
Firefox is developed by the
<a href="https://www.mozilla.org/en-US/" target="_blank">Mozilla Foundation</a>.
```

Sia che si aprano o meno i link in una nuova scheda, dovrebbe essere una decisione consapevole, basata su considerazioni di design dell'esperienza utente. Ecco alcune cose a cui pensare:

- Aprire link in una nuova scheda presenta i due documenti contemporaneamente, il che è utile per un'esperienza di navigazione "parallela". Al contrario, i link che si aprono nella stessa scheda sono più simili a un proseguimento della pagina corrente.
- Aprire link in una nuova scheda può confondere gli utenti abituati a usare il pulsante indietro.
- Anche quando i link vengono aperti per impostazione predefinita nella stessa scheda, gli utenti possono comunque scegliere di aprirli in una nuova scheda, utilizzando scorciatoie da tastiera o opzioni del menu contestuale. Al contrario, i link che si aprono in una nuova scheda sono difficili da aprire nella stessa scheda.
- Gli utenti di screen reader potrebbero sentirsi disorientati da link che si aprono in una nuova scheda, in quanto potrebbero non rendersi conto che la nuova scheda si è aperta, e potrebbero perdere il contesto sulla loro posizione nella pagina.

Un approccio comune è aprire link esterni in nuove schede e link interni nella stessa scheda.
Alcuni designer preferiscono aprire tutti i link nella stessa scheda.
Se si aprono link in nuove schede, è consigliabile fornire segnali per questi link, come un'icona accanto al testo del link.

## Apprendimento attivo: creare un menu di navigazione

Per questo esercizio, ti chiediamo di collegare alcune pagine insieme con un menu di navigazione per creare un sito web multipagina. Questo è un modo comune in cui viene creato un sito web — la stessa struttura di pagina è utilizzata su ogni pagina, inclusa lo stesso menu di navigazione, quindi quando i link vengono cliccati dà l'impressione di rimanere nello stesso posto e visualizzare contenuti diversi.

Dovrai fare copie locali delle seguenti quattro pagine, tutte nella stessa directory. Per un elenco completo dei file, vedi la directory [navigation-menu-start](https://github.com/mdn/learning-area/tree/main/html/introduction-to-html/navigation-menu-start):

- [index.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/navigation-menu-start/index.html)
- [projects.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/navigation-menu-start/projects.html)
- [pictures.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/navigation-menu-start/pictures.html)
- [social.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/navigation-menu-start/social.html)

Dovresti:

1. Aggiungere un elenco non ordinato nel posto indicato su una pagina che includa i nomi delle pagine a cui collegarsi.
   Un menu di navigazione è solitamente solo un elenco di link, quindi questo è semanticamente corretto.
2. Trasformare ogni nome di pagina in un link a quella pagina.
3. Copiare il menu di navigazione su ogni pagina.
4. Su ogni pagina, rimuovere solo il link a quella stessa pagina — è confusivo e inutile per una pagina includere un link a sé stessa.
   Inoltre, l'assenza di un link funge da buon promemoria visivo per indicare quale pagina si sta attualmente visualizzando.

L'esempio finito dovrebbe sembrare simile alla seguente pagina:

![Un esempio di un semplice menu di navigazione HTML, con elementi menu home, pictures, projects e social](navigation-example.png)

> [!NOTE]
> Se ti blocchi o non sei sicuro di aver fatto giusto, puoi controllare la directory [navigation-menu-marked-up](https://github.com/mdn/learning-area/tree/main/html/introduction-to-html/navigation-menu-marked-up) per vedere la risposta corretta.

## Collegamenti email

È possibile creare link o pulsanti che, se cliccati, aprono un nuovo messaggio email in uscita, piuttosto che collegarsi a una risorsa o pagina.
Questo è fatto usando l'elemento {{HTMLElement("a")}} e lo schema URL `mailto:`.

Nella sua forma più basilare e comunemente usata, un link `mailto:` indica l'indirizzo email del destinatario previsto. Ad esempio:

```html
<a href="mailto:nowhere@mozilla.org">Send email to nowhere</a>
```

Questo produce un link che appare così: [Invia email a nowhere](mailto:nowhere@mozilla.org).

In realtà, l'indirizzo email è facoltativo. Se lo ometti e il tuo [`href`](/it/docs/Web/HTML/Reference/Elements/a#href) è "mailto:", verrà aperta una nuova finestra di email in uscita dal client email dell'utente senza indirizzo di destinazione.
Questo è spesso utile come collegamenti "Condividi" che gli utenti possono cliccare per inviare un'email a un indirizzo a loro scelta.

### Specificare dettagli

Oltre all'indirizzo email, puoi fornire altre informazioni. Infatti, qualsiasi campo standard dell'intestazione email può essere aggiunto all'URL `mailto` che fornisci.
I più comunemente usati sono "subject", "cc", e "body" (che non è un vero campo di intestazione, ma ti consente di specificare un breve messaggio di contenuto per la nuova email).
Ogni campo e il suo valore sono specificati come un termine della query.

Ecco un esempio che include un cc, bcc, subject e body:

```html
<a
  href="mailto:nowhere@mozilla.org?cc=name2@rapidtables.com&bcc=name3@rapidtables.com&subject=The%20subject%20of%20the%20email&body=The%20body%20of%20the%20email">
  Send mail with cc, bcc, subject and body
</a>
```

> [!NOTE]
> I valori di ciascun campo devono essere codificati in modo URL con caratteri non stampabili (caratteri invisibili come tab, ritorno a capo e interruzioni di pagina) e spazi {{Glossary("Percent-encoding", "percentualmente codificati")}}.
> Nota anche l'uso del punto interrogativo (`?`) per separare l'URL principale dai valori dei campi, e gli e commerciali (&) per separare ogni campo nell'URL `mailto:`.
> Questa è l'annotazione standard delle query URL.
> Leggi [Il metodo GET](/it/docs/Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data#the_get_method) per capire cosa viene più comunemente usato nell'annotazione delle query URL.

Ecco alcuni altri esempi di URL `mailto`:

- <mailto:>
- <mailto:nowhere@mozilla.org>
- <mailto:nowhere@mozilla.org,nobody@mozilla.org>
- <mailto:nowhere@mozilla.org?cc=nobody@mozilla.org>
- <mailto:nowhere@mozilla.org?cc=nobody@mozilla.org&subject=This%20is%20the%20subject>

## Metti alla prova le tue abilità!

Hai raggiunto la fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare alcuni test ulteriori per verificare che tu abbia assimilato queste informazioni prima di proseguire — vedi [Metti alla prova le tue abilità: Link](/it/docs/Learn_web_development/Core/Structuring_content/Test_your_skills/Links).

## Riepilogo

Questo è tutto per i link, per ora! Tornerai sui link più avanti nel corso quando inizierai a guardarne la stilizzazione. Prossimamente per HTML, affronterai un paio di sfide che metteranno alla prova la tua comprensione degli argomenti che hai trattato finora.

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Advanced_text_features", "Learn_web_development/Core/Structuring_content/Marking_up_a_letter", "Learn_web_development/Core/Structuring_content")}}
