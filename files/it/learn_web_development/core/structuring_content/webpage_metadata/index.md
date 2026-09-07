---
title: Cosa c'è nell'head? Metadati della pagina web
short-title: Metadati della pagina web
slug: Learn_web_development/Core/Structuring_content/Webpage_metadata
l10n:
  sourceCommit: 0d59135676db5a372b4dd692f0686e6bdfc13b51
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Basic_HTML_syntax", "Learn_web_development/Core/Structuring_content/Headings_and_paragraphs", "Learn_web_development/Core/Structuring_content")}}

L'{{Glossary("Head", "head")}} di un documento HTML è la parte che non viene visualizzata nel browser web quando la pagina viene caricata. Contiene informazioni sui metadati, come l'elemento {{htmlelement("title")}} della pagina, collegamenti a {{Glossary("CSS", "CSS")}} (se si sceglie di applicare stili ai contenuti HTML con CSS), collegamenti a favicon personalizzate e altri metadati (dati sull'HTML, come l'autore e parole chiave importanti che descrivono il documento).

I browser web utilizzano le informazioni contenute nell'{{Glossary("Head", "head")}} per eseguire correttamente il rendering del documento HTML. In questo articolo verranno trattati tutti gli aspetti sopra indicati e altro ancora, per fornire una buona base per lavorare con il markup.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Conoscenza di base di HTML, come trattato nella lezione precedente.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>L'head HTML e il suo scopo come contenitore di metadati per il documento.</li>
          <li>Impostare la codifica dei caratteri e il titolo del documento.</li>
          <li>Fornire metadati ai motori di ricerca.</li>
          <li>Collegare icone da utilizzare nei browser e nelle piattaforme mobili.</li>
          <li>Collegare fogli di stile e file di script.</li>
          <li>La necessità di impostare la lingua di un documento usando l'attributo <code>lang</code> nel tag di apertura <code>&lt;html&gt;</code>.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Che cos'è l'head HTML?

Rivediamo il semplice [documento HTML trattato nell'articolo precedente](/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax#anatomy_of_an_html_document):

```html
<!doctype html>
<html lang="en-US">
  <head>
    <meta charset="utf-8" />
    <title>My test page</title>
  </head>
  <body>
    <p>This is my page</p>
  </body>
</html>
```

L'head HTML è il contenuto dell'elemento {{htmlelement("head")}}. A differenza del contenuto dell'elemento {{htmlelement("body")}} (che viene visualizzato sulla pagina quando questa viene caricata in un browser), il contenuto dell'head non viene visualizzato sulla pagina. Il compito dell'head è invece contenere {{Glossary("Metadata", "metadati")}} sul documento. Nell'esempio precedente, l'head è piuttosto piccolo:

```html
<head>
  <meta charset="utf-8" />
  <title>My test page</title>
</head>
```

Nelle pagine più grandi, tuttavia, l'head può diventare piuttosto esteso. Provare a visitare alcuni siti web preferiti e usare gli [strumenti di sviluppo](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools) per controllare il contenuto del loro head. Lo scopo qui non è mostrare come utilizzare tutto ciò che può essere inserito nell'head, ma insegnare a usare gli elementi principali che si vorrà includere nell'head e acquisire familiarità con essi. Iniziamo.

## Aggiungere un titolo

Abbiamo già visto l'elemento {{htmlelement("title")}} in azione: può essere usato per aggiungere un titolo al documento. Tuttavia, può essere confuso con l'elemento {{htmlelement("Heading_Elements", "h1")}}, che viene utilizzato per aggiungere un'intestazione di livello superiore al contenuto del body: talvolta viene anche chiamato titolo della pagina. Ma sono cose diverse!

- L'elemento {{htmlelement("Heading_Elements", "h1")}} appare nella pagina quando questa viene caricata nel browser; in genere dovrebbe essere usato una volta per pagina, per contrassegnare il titolo del contenuto della pagina (il titolo di una storia, il titolo di una notizia o qualunque cosa sia appropriata al caso d'uso).
- L'elemento {{htmlelement("title")}} è un metadato che rappresenta il titolo dell'intero documento HTML (non il contenuto del documento).

### Esaminare un esempio

1. In questo esercizio, iniziare visitando il nostro repository GitHub e scaricando una copia della pagina [title-example.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/the-html-head/title-example.html). A questo scopo, è possibile:
   1. Copiare e incollare il codice dalla pagina in un nuovo file di testo nell'editor di codice, quindi salvarlo in una posizione appropriata.
   2. Premere il pulsante "Raw" nella pagina GitHub, per visualizzare il codice non elaborato, eventualmente in una nuova scheda del browser. Quindi scegliere l'opzione _Salva pagina con nome…_ del browser e scegliere una posizione appropriata in cui salvare il file.

2. Ora aprire il file nel browser. Dovrebbe essere visualizzato qualcosa di simile:

   ![Una pagina web con il testo 'title' nella scheda della pagina del browser e il testo 'h1' come intestazione della pagina nel body del documento.](title-example.png)

   Ora dovrebbe essere del tutto evidente dove appare il contenuto di `<h1>` e dove appare il contenuto di `<title>`.

3. Provare anche ad aprire il codice nell'editor di codice, modificare il contenuto di questi elementi e quindi aggiornare la pagina nel browser. Sperimentare liberamente.

Il contenuto dell'elemento `<title>` viene utilizzato anche in altri modi. Ad esempio, se si prova ad aggiungere la pagina ai segnalibri (_Segnalibri > Aggiungi pagina ai segnalibri_ oppure l'icona a forma di stella nella barra degli URL in Firefox), il contenuto di `<title>` verrà inserito come nome suggerito per il segnalibro.

![Una pagina web aggiunta ai segnalibri in Firefox. Il nome del segnalibro è stato compilato automaticamente con il contenuto dell'elemento 'title'](bookmark-example.png)

Il contenuto di `<title>` viene utilizzato anche nei risultati di ricerca, come verrà mostrato di seguito.

## Metadati: l'elemento `<meta>`

I metadati sono dati che descrivono dati e HTML dispone di un modo "ufficiale" per aggiungere metadati a un documento: l'elemento {{htmlelement("meta")}}. Naturalmente, anche gli altri elementi trattati in questo articolo possono essere considerati metadati. Esistono molti tipi diversi di elementi `<meta>` che possono essere inclusi nell'`<head>` della pagina, ma in questa fase non verranno spiegati tutti, poiché ciò potrebbe risultare troppo confuso. Verranno invece spiegati alcuni elementi che si incontrano comunemente, giusto per fornire un'idea.

### Specificare la codifica dei caratteri del documento

Nell'esempio visto sopra era inclusa questa riga:

```html
<meta charset="utf-8" />
```

Questo elemento specifica la codifica dei caratteri del documento, ovvero il set di caratteri che il documento può usare. `utf-8` è un set di caratteri universale che include praticamente qualsiasi carattere di qualsiasi lingua umana. Ciò significa che la pagina web sarà in grado di gestire la visualizzazione di qualsiasi lingua; è quindi una buona idea impostarlo in ogni pagina web creata. Ad esempio, la pagina potrebbe gestire senza problemi sia l'inglese sia il giapponese:

![Una pagina web contenente caratteri inglesi e giapponesi, con la codifica dei caratteri impostata su universale, ovvero utf-8. Entrambe le lingue vengono visualizzate correttamente.](correct-encoding.png)

Se si imposta la codifica dei caratteri su `ISO-8859-1`, ad esempio (il set di caratteri per l'alfabeto latino), il rendering della pagina potrebbe apparire completamente alterato:

![Una pagina web contenente caratteri inglesi e giapponesi, con la codifica dei caratteri impostata su latino. I caratteri giapponesi non vengono visualizzati correttamente.](bad-encoding.png)

> [!NOTE]
> Alcuni browser, come Chrome, correggono automaticamente le codifiche errate; pertanto, a seconda del browser utilizzato, questo problema potrebbe non essere visibile. È comunque necessario impostare la codifica `utf-8` nella pagina per evitare potenziali problemi in altri browser.

### Sperimentare con la codifica dei caratteri

Per provarlo, riprendere il semplice modello HTML ottenuto nella sezione precedente su `<title>` (la pagina [title-example.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/the-html-head/title-example.html)), provare a modificare il valore di meta charset in `ISO-8859-1` e aggiungere il testo giapponese alla pagina. Questo è il codice utilizzato:

```html
<p>Japanese example: ご飯が熱い。</p>
```

### Aggiungere un autore e una descrizione

Molti elementi `<meta>` includono gli attributi `name` e `content`:

- `name` specifica il tipo di elemento meta; ovvero quale tipo di informazioni contiene.
- `content` specifica il contenuto meta effettivo.

Due elementi meta di questo tipo, utili da includere nella pagina, definiscono l'autore della pagina e forniscono una descrizione concisa della pagina. Vediamo un esempio:

```html
<meta name="author" content="Chris Mills" />
<meta
  name="description"
  content="The MDN Web Docs Learning Area aims to provide
complete beginners to the Web with all they need to know to get
started with developing websites and applications." />
```

Specificare un autore è vantaggioso sotto molti aspetti: è utile poter capire chi ha scritto la pagina, nel caso vi siano domande sul contenuto e si desideri contattare l'autore. Alcuni sistemi di gestione dei contenuti dispongono di funzionalità per estrarre automaticamente le informazioni sull'autore della pagina e renderle disponibili per tali scopi.

Specificare una descrizione che includa parole chiave relative al contenuto della pagina è utile perché può far apparire la pagina più in alto nelle ricerche pertinenti effettuate nei motori di ricerca (tali attività sono denominate {{Glossary("SEO", "Search Engine Optimization")}}, o {{Glossary("SEO", "SEO")}}).

### Esplorare l'uso della descrizione nei motori di ricerca

La descrizione viene utilizzata anche nelle pagine dei risultati dei motori di ricerca. Seguiamo un esercizio per esplorare questo aspetto:

1. Visitare la [pagina iniziale di Mozilla Developer Network](/en-US/).
2. Visualizzare il sorgente della pagina (fare clic con il pulsante destro del mouse sulla pagina e scegliere _Visualizza sorgente pagina_ dal menu contestuale).
3. Trovare il tag meta della descrizione. Sarà simile a questo, anche se potrebbe cambiare nel tempo:

   ```html
   <meta
     name="description"
     content="The MDN Web Docs site
     provides information about Open Web technologies
     including HTML, CSS, and APIs for both websites and
     progressive web apps." />
   ```

4. Ora cercare "MDN Web Docs" nel motore di ricerca preferito (è stato usato Google). Si noterà che il contenuto degli elementi `<meta>` della descrizione e `<title>` viene utilizzato nel risultato di ricerca: decisamente utile.

   ![Un risultato di ricerca Yahoo per "Mozilla Developer Network"](mdn-search-result.png)

> [!NOTE]
> In Google, sotto il collegamento principale alla home page verranno visualizzate alcune sottopagine pertinenti di MDN Web Docs: sono chiamate sitelink e possono essere configurate negli [strumenti per webmaster di Google](https://search.google.com/search-console/about?hl=en), un modo per migliorare i risultati di ricerca del sito nel motore di ricerca Google.

> [!NOTE]
> Molte funzionalità di `<meta>` non vengono più utilizzate. Ad esempio, l'elemento `<meta>` per le parole chiave (`<meta name="keywords" content="fill, in, your, keywords, here">`), che dovrebbe fornire parole chiave affinché i motori di ricerca determinino la pertinenza della pagina rispetto a diversi termini di ricerca, viene ignorato dai motori di ricerca perché gli spammer riempivano l'elenco di parole chiave con centinaia di termini, distorcendo i risultati.

### Altri tipi di metadati

Navigando sul web, si incontreranno anche altri tipi di metadati. Molte delle funzionalità presenti nei siti web sono creazioni proprietarie progettate per fornire a determinati siti, come i social network, informazioni specifiche che possono utilizzare.

Ad esempio, [Open Graph Data](https://ogp.me/) è un protocollo di metadati inventato da Facebook per fornire metadati più ricchi ai siti web. Nel codice sorgente di MDN Web Docs si trova quanto segue:

```html
<meta
  property="og:image"
  content="https://developer.mozilla.org/mdn-social-share.png" />
<meta
  property="og:description"
  content="The Mozilla Developer Network (MDN) provides
information about Open Web technologies including HTML, CSS, and APIs for both websites
and HTML Apps." />
<meta property="og:title" content="Mozilla Developer Network" />
```

Un effetto di questo è che, quando viene inserito un collegamento a MDN Web Docs su Facebook, il link appare insieme a un'immagine e a una descrizione: un'esperienza più ricca per gli utenti.

![Dati del protocollo Open Graph dalla home page di MDN visualizzati su Facebook, che mostrano un'immagine, un titolo e una descrizione.](facebook-output.png)

## Aggiungere icone personalizzate al sito

Per arricchire ulteriormente il design del sito, è possibile aggiungere riferimenti a icone personalizzate nei metadati, che verranno visualizzate in determinati contesti. La più comunemente utilizzata è la **favicon** (abbreviazione di "favorites icon", in riferimento al suo utilizzo negli elenchi di "preferiti" o "segnalibri" nei browser).

La modesta favicon esiste da molti anni. È la prima icona di questo tipo: un'icona quadrata di 16 pixel utilizzata in più posizioni. A seconda del browser, è possibile vedere le favicon visualizzate nella scheda del browser che contiene ogni pagina aperta e accanto alle pagine aggiunte ai segnalibri nel pannello dei segnalibri.

È possibile aggiungere una favicon alla pagina:

1. Salvandola in un formato supportato, come `.ico`, `.gif` o `.png`, in un punto qualsiasi della struttura delle cartelle del sito web.
2. Aggiungendo un elemento {{htmlelement("link")}} nel blocco {{HTMLElement("head")}} dell'HTML, che fa riferimento al percorso del file favicon:

   ```html
   <link rel="icon" href="/favicon.ico" type="image/x-icon" />
   ```

> [!NOTE]
> In questo esempio, il percorso del file favicon inizia con `/`, che significa "cercare il file nella directory di livello superiore, o _root_, del sito". Questo percorso potrebbe trovarsi in un punto diverso nel codice sorgente, a seconda del sistema utilizzato per creare il sito: i framework web di solito riservano una cartella speciale ai file nella root del sito, come `static` o `public`.
>
> Per ora non preoccuparsi troppo delle complessità dei percorsi dei file; verranno approfonditi in seguito (consultare [Una rapida introduzione agli URL e ai percorsi](/it/docs/Learn_web_development/Core/Structuring_content/Creating_links#a_quick_primer_on_urls_and_paths) per ulteriori informazioni).
>
> Oggigiorno la maggior parte dei browser e delle applicazioni software utilizza automaticamente come favicon un file `favicon.ico` trovato nella root del sito, quindi molti siti non includono nemmeno l'elemento `<link>`. Un elemento esplicito è comunque utile nel caso in cui si desideri collocare il file favicon altrove.

Ecco un esempio di favicon in un pannello dei segnalibri:

![Il pannello dei segnalibri di Firefox, che mostra un esempio aggiunto ai segnalibri con una favicon visualizzata accanto.](bookmark-favicon.png)

Potrebbe anche essere opportuno includere icone diverse per contesti diversi. Ad esempio:

```html
<link rel="icon" href="/favicon-48x48.[some hex hash].png" />
<link rel="apple-touch-icon" href="/apple-touch-icon.[some hex hash].png" />
```

Questo è un modo per fare in modo che il sito mostri un'icona quando viene salvato nella schermata iniziale di un dispositivo Apple. Si potrebbero persino fornire icone diverse per dispositivi diversi, per garantire che l'icona abbia un buon aspetto su tutti i dispositivi. Ad esempio:

```html
<!-- iPad Pro with high-resolution Retina display: -->
<link
  rel="apple-touch-icon"
  sizes="167x167"
  href="/apple-touch-icon-167x167.png" />
<!-- 3x resolution iPhone: -->
<link
  rel="apple-touch-icon"
  sizes="180x180"
  href="/apple-touch-icon-180x180.png" />
<!-- non-Retina iPad, iPad mini, etc.: -->
<link
  rel="apple-touch-icon"
  sizes="152x152"
  href="/apple-touch-icon-152x152.png" />
<!-- 2x resolution iPhone and other devices: -->
<link rel="apple-touch-icon" href="/apple-touch-icon-120x120.png" />
<!-- basic favicon -->
<link rel="icon" href="/favicon.ico" />
```

I commenti spiegano per cosa viene utilizzata ogni icona: questi elementi coprono aspetti come la fornitura di un'icona ad alta risoluzione da usare quando il sito web viene salvato nella schermata iniziale di un iPad.

Per ora non preoccuparsi troppo di implementare tutti questi tipi di icone: si tratta di una funzionalità piuttosto avanzata e non è necessario conoscerla per proseguire nel corso. Lo scopo principale è far sapere cosa sono questi elementi, nel caso in cui vengano incontrati durante l'esplorazione del codice sorgente di altri siti web. Per approfondire tutti questi valori e come sceglierli, leggere la pagina di riferimento dell'elemento {{HTMLElement("link")}}.

## Applicare CSS e JavaScript a HTML

Quasi tutti i siti web moderni utilizzano {{Glossary("CSS", "CSS")}} per migliorarne l'aspetto e {{Glossary("JavaScript", "JavaScript")}} per implementare funzionalità interattive, come lettori video, mappe, giochi e altro. Questi vengono applicati più comunemente a una pagina web utilizzando rispettivamente l'elemento {{htmlelement("link")}} e l'elemento {{htmlelement("script")}}.

- L'elemento {{htmlelement("link")}} deve sempre essere inserito nell'head del documento. Richiede due attributi: `rel="stylesheet"`, che indica che si tratta del foglio di stile del documento, e `href`, che contiene il percorso del file del foglio di stile:

  ```html
  <link rel="stylesheet" href="my-css-file.css" />
  ```

- L'elemento {{htmlelement("script")}} deve anch'esso essere inserito nell'head e deve includere un attributo `src` contenente il percorso del JavaScript da caricare e `defer` (un [attributo booleano](/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax#boolean_attributes)), che indica al browser di caricare il JavaScript dopo che la pagina ha terminato di analizzare l'HTML. L'attributo `defer` è utile perché garantisce che tutto l'HTML sia caricato prima dell'esecuzione di JavaScript, evitando errori dovuti al tentativo di JavaScript di accedere a un elemento HTML che non esiste ancora nella pagina. Esistono [diversi modi](/it/docs/Learn_web_development/Core/Scripting/What_is_JavaScript#script_loading_strategies) per gestire il caricamento di JavaScript nella pagina, ma questo è il più affidabile da usare nei browser moderni.

  ```html
  <script src="my-js-file.js" defer></script>
  ```

  > [!NOTE]
  > L'elemento `<script>` potrebbe sembrare un {{Glossary("void_element", "elemento vuoto")}}, ma non lo è e pertanto necessita di un tag di chiusura. Invece di puntare a un file di script esterno, è anche possibile inserire lo script all'interno dell'elemento `<script>`.

### Tocca a te: applicare CSS e JavaScript a una pagina

1. Per iniziare questo esercizio, ottenere una copia dei file [meta-example.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/the-html-head/meta-example.html), [script.js](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/the-html-head/script.js) e [style.css](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/the-html-head/style.css), quindi salvarli sul computer locale nella stessa directory. Assicurarsi che vengano salvati con i nomi e le estensioni di file corretti.
2. Aprire il file HTML sia nel browser sia nell'editor di testo.
3. Seguendo le informazioni fornite sopra, aggiungere elementi {{htmlelement("link")}} e {{htmlelement("script")}} all'HTML, in modo che CSS e JavaScript vengano applicati all'HTML.

Se eseguito correttamente, dopo aver salvato l'HTML e aggiornato il browser dovrebbe essere possibile vedere che alcuni aspetti sono cambiati:

![Esempio che mostra una pagina con CSS e JavaScript applicati. Il CSS ha reso verde la pagina, mentre JavaScript ha aggiunto un elenco dinamico alla pagina.](js-and-css.png)

- JavaScript ha aggiunto un elenco vuoto alla pagina. Ora, facendo clic in un punto qualsiasi al di fuori dell'elenco, verrà visualizzata una finestra di dialogo che chiede di inserire del testo per un nuovo elemento dell'elenco. Premendo il pulsante OK, verrà aggiunto all'elenco un nuovo elemento contenente il testo. Facendo clic su un elemento dell'elenco esistente, verrà visualizzata una finestra di dialogo che consente di modificare il testo dell'elemento.
- CSS ha reso verde lo sfondo e più grande il testo. Ha inoltre applicato stili ad alcuni dei contenuti aggiunti da JavaScript alla pagina (la barra rossa con il bordo nero è lo stile CSS applicato all'elenco generato da JS).

> [!NOTE]
> Se si rimane bloccati in questo esercizio e non si riesce ad applicare CSS/JS, provare a consultare la nostra pagina di esempio [css-and-js.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/the-html-head/css-and-js.html).

## Impostare la lingua principale del documento

Infine, vale la pena ricordare che è possibile, e in effetti necessario, impostare la lingua della pagina. Questo può essere fatto aggiungendo l'[attributo lang](/it/docs/Web/HTML/Reference/Global_attributes/lang) al tag HTML di apertura, come mostrato in [meta-example.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/the-html-head/meta-example.html) e di seguito.

```html
<html lang="en-US">
  …
</html>
```

Questo è utile sotto molti aspetti. Il documento HTML verrà indicizzato più efficacemente dai motori di ricerca se la sua lingua è impostata, permettendo ad esempio che venga visualizzato correttamente nei risultati specifici per lingua; ed è utile per le persone con disabilità visive che usano gli screen reader. Ad esempio, la parola "six" esiste sia in francese sia in inglese, ma viene pronunciata in modo diverso.

È inoltre possibile impostare sottosezioni del documento affinché vengano riconosciute come lingue diverse. Ad esempio, è possibile impostare la sezione in lingua giapponese affinché venga riconosciuta come giapponese, in questo modo:

```html
<p>Japanese example: <span lang="ja">ご飯が熱い。</span>.</p>
```

Questi codici sono definiti dallo standard [ISO 639-1](https://en.wikipedia.org/wiki/ISO_639-1). Ulteriori informazioni sono disponibili in [Language tags in HTML and XML](https://www.w3.org/International/articles/language-tags/).

## Riepilogo

Si conclude qui questo rapido tour dell'head HTML: ci sono molte altre cose che è possibile fare al suo interno, ma un'esplorazione esaustiva sarebbe noiosa e confusa in questa fase, e per ora lo scopo era solo fornire un'idea degli elementi più comuni che vi si trovano. Nel prossimo articolo verranno trattati [Intestazioni e paragrafi in HTML](/it/docs/Learn_web_development/Core/Structuring_content/Headings_and_paragraphs).

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Basic_HTML_syntax", "Learn_web_development/Core/Structuring_content/Headings_and_paragraphs", "Learn_web_development/Core/Structuring_content")}}
