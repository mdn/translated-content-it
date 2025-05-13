---
title: Cosa c'è nell'head? Metadati della pagina web
short-title: Metadati della pagina web
slug: Learn_web_development/Core/Structuring_content/Webpage_metadata
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Basic_HTML_syntax", "Learn_web_development/Core/Structuring_content/Headings_and_paragraphs", "Learn_web_development/Core/Structuring_content")}}

L'{{Glossary("Head", "head")}} di un documento HTML è la parte che non viene visualizzata nel browser web quando la pagina viene caricata. Contiene informazioni sui metadati come il {{htmlelement("title")}} della pagina, collegamenti a {{Glossary("CSS", "CSS")}} (se decide di stilizzare il suo contenuto HTML con CSS), collegamenti a favicon personalizzate e altri metadati (dati sull'HTML, come l'autore, e parole chiave importanti che descrivono il documento).

I browser web utilizzano le informazioni contenute nell'{{Glossary("Head", "head")}} per rendere correttamente il documento HTML. In questo articolo copriremo tutto quanto sopra e altro ancora, al fine di darle una buona base per lavorare con il markup.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con HTML di base, come trattato nella lezione precedente.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>L'head HTML e il suo scopo come contenitore di metadati per il documento.</li>
          <li>Impostazione della codifica dei caratteri e del titolo del documento.</li>
          <li>Fornire metadati per i motori di ricerca.</li>
          <li>Collegamento a icone per l'uso su browser e piattaforme mobili.</li>
          <li>Collegamento a fogli di stile e file di script.</li>
          <li>La necessità di impostare la lingua di un documento utilizzando l'attributo <code>lang</code> nel tag di apertura <code>&lt;html&gt;</code>.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Che cos'è l'head HTML?

Rivediamo il semplice [documento HTML che abbiamo trattato nell'articolo precedente](/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax#anatomy_of_an_html_document):

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

L'head HTML è il contenuto dell'elemento {{htmlelement("head")}}. A differenza del contenuto dell'elemento {{htmlelement("body")}} (che viene visualizzato sulla pagina quando è caricata in un browser), il contenuto dell'head non viene visualizzato sulla pagina. Invece, il compito dell'head è contenere {{Glossary("Metadata", "metadati")}} sul documento. Nell'esempio sopra, l'head è piuttosto piccolo:

```html
<head>
  <meta charset="utf-8" />
  <title>My test page</title>
</head>
```

Tuttavia, in pagine più grandi, l'head può diventare piuttosto grande. Provi a visitare alcuni dei suoi siti web preferiti e utilizzi gli [strumenti per sviluppatori](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools) per controllare i loro contenuti di head. Il nostro obiettivo qui non è mostrarle come utilizzare tutto ciò che può essere messo nell'head, ma piuttosto insegnarle come utilizzare i principali elementi che vorrà includere nell'head e darle un po' di familiarità. Iniziamo.

## Aggiungere un titolo

Abbiamo già visto l'elemento {{htmlelement("title")}} in azione — questo può essere usato per aggiungere un titolo al documento. Questo però può essere confuso con l'elemento {{htmlelement("Heading_Elements", "h1")}}, che è usato per aggiungere un'intestazione di livello superiore al contenuto del suo corpo — questo è anche talvolta indicato come il titolo della pagina. Ma sono cose diverse!

- L'elemento {{htmlelement("Heading_Elements", "h1")}} appare sulla pagina quando viene caricata nel browser – in genere questo dovrebbe essere usato una volta per pagina, per marcare il titolo del suo contenuto della pagina (il titolo della storia, o il titolo della notizia, o qualsiasi cosa sia appropriata per il suo uso).
- L'elemento {{htmlelement("title")}} è un metadato che rappresenta il titolo dell'intero documento HTML (non il contenuto del documento).

### Apprendimento attivo: Ispezionare un esempio

1. Per iniziare questo apprendimento attivo, vorremmo che lei vada nel nostro repository di GitHub e scarichi una copia della nostra [pagina title-example.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/the-html-head/title-example.html). Per fare questo, o

   1. Copi e incolli il codice dalla pagina in un nuovo file di testo nel suo editor di codice, poi lo salvi in un posto sensato.
   2. Premi il pulsante "Raw" sulla pagina di GitHub, che fa apparire il codice grezzo (possibilmente in una nuova scheda del browser). Successivamente, scelga il menu _Salva pagina con nome..._ del suo browser e scelga un luogo appropriato per salvare il file.

2. Ora apra il file nel suo browser. Dovrebbe vedere qualcosa di simile:

   ![Una pagina web con testo 'title' nella scheda della pagina del browser e testo 'h1' come intestazione della pagina nel corpo del documento.](title-example.png)

   Ora dovrebbe essere completamente ovvio dove appaiono il contenuto `<h1>` e dove appaiono il contenuto `<title>`!

3. Dovrebbe anche provare ad aprire il codice nel suo editor di codice, modificare il contenuto di questi elementi, per poi ricaricare la pagina nel suo browser. Si diverta con esso.

Il contenuto dell'elemento `<title>` è anche utilizzato in altri modi. Per esempio, se prova a creare un segnalibro della pagina (_Segnalibri > Aggiungi pagina ai segnalibri_ o l'icona a stella nella barra URL in Firefox), vedrà il contenuto `<title>` compilato come nome suggerito per il segnalibro.

![Una pagina web in cui si sta aggiungendo un segnalibro in Firefox. Il nome del segnalibro è stato automaticamente compilato con il contenuto dell'elemento 'title'](bookmark-example.png)

Il contenuto `<title>` è anche utilizzato nei risultati di ricerca, come vedrà qui sotto.

## Metadati: l'elemento `<meta>`

I metadati sono dati che descrivono dati, e l'HTML ha un modo "ufficiale" di aggiungere metadati a un documento — l'elemento {{htmlelement("meta")}}. Ovviamente, anche le altre cose di cui stiamo parlando in questo articolo potrebbero essere considerate metadati. Ci sono molti tipi diversi di elementi `<meta>` che possono essere inclusi nell`<head>` della sua pagina, ma non proveremo a spiegarli tutti in questa fase, poiché risulterebbe troppo confuso. Invece, spiegheremo alcune cose che potrebbe vedere comunemente, solo per darle un'idea.

### Specificare la codifica dei caratteri del suo documento

Nell'esempio che abbiamo visto sopra, era inclusa questa riga:

```html
<meta charset="utf-8" />
```

Questo elemento specifica la codifica dei caratteri del documento — l'insieme di caratteri che il documento è autorizzato a usare. `utf-8` è un insieme di caratteri universale che include praticamente qualsiasi carattere di qualsiasi lingua umana. Ciò significa che la sua pagina web sarà in grado di gestire la visualizzazione di qualsiasi lingua; è quindi una buona idea impostarla su ogni pagina web che crea! Per esempio, la sua pagina potrebbe gestire l'inglese e il giapponese senza problemi:

![Una pagina web contenente caratteri inglesi e giapponesi, con la codifica dei caratteri impostata su universale, o utf-8. Entrambe le lingue si visualizzano correttamente.](correct-encoding.png)

Se imposta la sua codifica dei caratteri su `ISO-8859-1`, per esempio (l'insieme di caratteri per l'alfabeto latino), la sua resa della pagina potrebbe apparire tutta incasinata:

![Una pagina web contenente caratteri inglesi e giapponesi, con la codifica dei caratteri impostata su latino. I caratteri giapponesi non vengono visualizzati correttamente.](bad-encoding.png)

> [!NOTE]
> Alcuni browser (come Chrome) correggono automaticamente codifiche errate, quindi a seconda del browser che utilizza, potrebbe non vedere questo problema. Dovrebbe comunque impostare una codifica `utf-8` sulla sua pagina per evitare eventuali problemi in altri browser.

### Apprendimento attivo: Sperimentare con la codifica dei caratteri

Per provare questo, riveda il semplice modello HTML che ha ottenuto nella sezione precedente su `<title>` (la [pagina title-example.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/the-html-head/title-example.html)), provi a cambiare il valore meta charset in `ISO-8859-1`, e aggiunga il giapponese alla sua pagina. Questo è il codice che abbiamo usato:

```html
<p>Japanese example: ご飯が熱い。</p>
```

### Aggiungere un autore e una descrizione

Molti elementi `<meta>` includono attributi `name` e `content`:

- `name` specifica il tipo di elemento meta che è; che tipo di informazioni contiene.
- `content` specifica il reale contenuto meta.

Due di tali elementi meta che sono utili da includere nella sua pagina definiscono l'autore della pagina e forniscono una concisa descrizione della pagina. Vediamo un esempio:

```html
<meta name="author" content="Chris Mills" />
<meta
  name="description"
  content="The MDN Web Docs Learning Area aims to provide
complete beginners to the Web with all they need to know to get
started with developing websites and applications." />
```

Specificare un autore è vantaggioso in molti modi: è utile poter capire chi ha scritto la pagina, nel caso abbia domande sul contenuto e desideri contattarlo. Alcuni sistemi di gestione del contenuto hanno strumenti per estrarre automaticamente le informazioni sull'autore della pagina e renderle disponibile per tali scopi.

Specificare una descrizione che includa le parole chiave relative al contenuto della sua pagina è utile in quanto ha il potenziale di far apparire la sua pagina più in alto nelle ricerche pertinenti effettuate nei motori di ricerca (tali attività sono chiamate {{Glossary("SEO", "ottimizzazione per i motori di ricerca")}}, o {{Glossary("SEO", "SEO")}}).

### Apprendimento attivo: Uso della descrizione nei motori di ricerca

La descrizione è anche utilizzata nelle pagine dei risultati dei motori di ricerca. Facciamo un esercizio per esplorarlo.

1. Vada alla [pagina principale di The Mozilla Developer Network](/en-US/).
2. Visualizzi il codice sorgente della pagina (faccia clic destro sulla pagina, scelga _Visualizza sorgente della pagina_ dal menu contestuale.)
3. Trovi il tag meta description. Apparirà qualcosa di simile a questo (anche se potrà cambiare nel tempo):

   ```html
   <meta
     name="description"
     content="The MDN Web Docs site
     provides information about Open Web technologies
     including HTML, CSS, and APIs for both websites and
     progressive web apps." />
   ```

4. Ora cerchi "MDN Web Docs" nel suo motore di ricerca preferito (abbiamo usato Google). Noterà i contenuti degli elementi `<meta>` e `<title>` usati nel risultato di ricerca — sicuramente vale la pena averli!

   ![Un risultato di ricerca Yahoo per "Mozilla Developer Network"](mdn-search-result.png)

> [!NOTE]
> In Google, vedrà alcune sottopagine rilevanti di MDN Web Docs elencate sotto il link principale della homepage — questi sono chiamati sitelink, e sono configurabili nei [strumenti per webmaster di Google](https://search.google.com/search-console/about?hl=en) — un modo per migliorare i risultati di ricerca del suo sito nel motore di ricerca Google.

> [!NOTE]
> Molte funzionalità `<meta>` semplicemente non vengono più usate. Ad esempio, l'elemento `<meta>` delle parole chiave (`<meta name="keywords" content="fill, in, your, keywords, here">`) — che dovrebbe fornire parole chiave per i motori di ricerca per determinare la rilevanza di quella pagina per diversi termini di ricerca — è ignorato dai motori di ricerca, poiché gli spammer stavano semplicemente riempiendo la lista delle parole chiave con centinaia di parole chiave, influenzando i risultati.

### Altri tipi di metadati

Mentre naviga sul web, troverà anche altri tipi di metadati. Molte delle funzionalità che vedrà sui siti web sono creazioni proprietarie progettate per fornire a determinati siti (come i siti di social network) informazioni specifiche che possono utilizzare.

Ad esempio, [Open Graph Data](https://ogp.me/) è un protocollo di metadati che Facebook ha inventato per fornire metadati più ricchi per i siti web. Nel codice sorgente di MDN Web Docs, troverà questo:

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

Uno degli effetti di questo è che quando lei collega MDN Web Docs su Facebook, il link appare insieme a un'immagine e una descrizione: un'esperienza più ricca per gli utenti.

![Dati del protocollo Open Graph dalla homepage di MDN visualizzati su Facebook, mostrando un'immagine, un titolo e una descrizione.](facebook-output.png)

## Aggiungere icone personalizzate al suo sito

Per arricchire ulteriormente il design del suo sito, può aggiungere riferimenti a icone personalizzate nei suoi metadati, e queste verranno visualizzate in determinati contesti. La più comunemente usata di queste è la **favicon** (abbreviazione di "favorites icon", riferendosi al suo uso negli elenchi "favorites" o "bookmarks" nei browser).

La modesta favicon esiste da molti anni. È la prima icona di questo tipo: un'icona quadrata di 16 pixel usata in più posti. Potrà vedere (a seconda del browser) le favicon visualizzate nella scheda del browser contenente ogni pagina aperta, e accanto alle pagine salvate nei segnalibri nel pannello dei segnalibri.

Una favicon può essere aggiunta alla sua pagina:

1. Salvandola nella stessa directory della pagina index del sito, salvata in formato `.ico` (la maggior parte supporta anche favicon in formati più comuni come `.gif` o `.png`)
2. Aggiungendo la seguente riga nel blocco {{HTMLElement("head")}} del suo HTML per fare riferimento ad essa:

   ```html
   <link rel="icon" href="favicon.ico" type="image/x-icon" />
   ```

Ecco un esempio di una favicon nel pannello dei segnalibri:

![Il pannello dei segnalibri di Firefox, che mostra un esempio salvato con una favicon visualizzata accanto.](bookmark-favicon.png)

Potrebbe anche aver bisogno di icone diverse per contesti diversi. Ad esempio, troverà quanto segue nel codice sorgente della homepage di MDN Web Docs:

```html
<link rel="icon" href="/favicon-48x48.[some hex hash].png" />
<link rel="apple-touch-icon" href="/apple-touch-icon.[some hex hash].png" />
```

Questo è un modo per far mostrare al sito un'icona quando viene salvato sulla schermata home di un dispositivo Apple. Potrebbe anche voler fornire icone diverse per dispositivi diversi, per assicurarsi che l'icona appaia bene su tutti i dispositivi. Ad esempio:

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

I commenti spiegano a cosa serve ogni icona — questi elementi coprono cose come fornire un'icona ad alta risoluzione da usare quando il sito web è salvato sulla schermata home di un iPad.

Non si preoccupi troppo di implementare tutti questi tipi di icona in questo momento — questa è una caratteristica piuttosto avanzata, e non ci si aspetterà che abbia conoscenza di questo per progredire nel corso. Lo scopo principale qui è farle sapere cosa sono queste cose, nel caso lei le incontri mentre naviga nel codice sorgente di altri siti web. Se desidera approfondire tutti questi valori e come sceglierli, legga la pagina di riferimento dell'elemento {{HTMLElement("link")}}.

> [!NOTE]
> Se il suo sito utilizza una Content Security Policy (CSP) per incrementarne la sicurezza, la policy si applica alla favicon. Se si verificano problemi con la favicon che non viene caricata, verifichi che l'intestazione {{HTTPHeader("Content-Security-Policy")}} non stia impedendo l'accesso ad essa attraverso la direttiva [`img-src`](/it/docs/Web/HTTP/Reference/Headers/Content-Security-Policy/img-src).

## Applicare CSS e JavaScript a HTML

Quasi tutti i siti web che utilizzerà oggi impiegheranno {{Glossary("CSS", "CSS")}} per renderli accattivanti e {{Glossary("JavaScript", "JavaScript")}} per alimentare funzionalità interattive, come lettori video, mappe, giochi e molto altro. Questi sono più comunemente applicati a una pagina web utilizzando l'elemento {{htmlelement("link")}} e l'elemento {{htmlelement("script")}}, rispettivamente.

- L'elemento {{htmlelement("link")}} dovrebbe sempre andare all'interno dell'head del suo documento. Questo prende due attributi, `rel="stylesheet"`, che indica che è il foglio di stile del documento, e `href`, che contiene il percorso al file del foglio di stile:

  ```html
  <link rel="stylesheet" href="my-css-file.css" />
  ```

- L'elemento {{htmlelement("script")}} dovrebbe anch'esso andare nell'head, e dovrebbe includere un attributo `src` che contiene il percorso al JavaScript che si vuole caricare, e `defer` (un [attributo booleano](/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax#boolean_attributes)), che istruisce il browser a caricare il JavaScript dopo che la pagina ha finito di analizzare l'HTML. L'attributo `defer` è utile in quanto garantisce che l'HTML sia tutto caricato prima che il JavaScript venga eseguito, così da non ottenere errori a causa del JavaScript che cerca di accedere a un elemento HTML che ancora non esiste sulla pagina. Ci sono [diversi modi](/it/docs/Learn_web_development/Core/Scripting/What_is_JavaScript#script_loading_strategies) per gestire il caricamento del JavaScript sulla sua pagina, ma questo è il metodo più affidabile da usare per i browser moderni.

  ```html
  <script src="my-js-file.js" defer></script>
  ```

> [!NOTE]
> L'elemento `<script>` potrebbe sembrare un {{Glossary("void_element", "elemento void")}}, ma non lo è, e quindi necessita di un tag di chiusura. Invece di puntare a un file di script esterno, può anche scegliere di inserire il suo script all'interno dell'elemento `<script>`.

### Apprendimento attivo: applicare CSS e JavaScript a una pagina

1. Per iniziare questo apprendimento attivo, prenda una copia dei nostri file [meta-example.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/the-html-head/meta-example.html), [script.js](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/the-html-head/script.js) e [style.css](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/the-html-head/style.css), e li salvi sul suo computer locale nella stessa directory. Si assicuri che siano salvati con i nomi e le estensioni di file corretti.
2. Apra il file HTML sia nel suo browser sia nel suo editor di testo.
3. Seguendo le informazioni date sopra, aggiunga gli elementi {{htmlelement("link")}} e {{htmlelement("script")}} al suo HTML, così che il suo CSS e JavaScript vengano applicati al suo HTML.

Se fatto correttamente, quando salva il suo HTML e aggiorna il suo browser dovrebbe vedere che le cose sono cambiate:

![Esempio che mostra una pagina con CSS e JavaScript applicati ad essa. Il CSS ha fatto diventare la pagina verde, mentre il JavaScript ha aggiunto una lista dinamica alla pagina.](js-and-css.png)

- Il JavaScript ha aggiunto una lista vuota alla pagina. Ora, quando clicca ovunque fuori dalla lista, apparirà una finestra di dialogo che le chiederà di inserire del testo per un nuovo elemento della lista. Quando preme il pulsante OK, verrà aggiunto un nuovo elemento alla lista contenente il testo. Quando clicca su un elemento esistente della lista, apparirà una finestra di dialogo permettendole di cambiare il testo dell'elemento.
- Il CSS ha fatto diventare lo sfondo verde e ha reso il testo più grande. Ha anche stilizzato parte del contenuto che il JavaScript ha aggiunto alla pagina (la barra rossa con il bordo nero è la stilizzazione aggiunta dal CSS alla lista generata con JS.)

> [!NOTE]
> Se si blocca in questo esercizio e non riesce a far applicare il CSS/JS, provi a controllare la nostra pagina di esempio [css-and-js.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/the-html-head/css-and-js.html).

## Impostare la lingua principale del documento

Infine, vale la pena menzionare che può (e dovrebbe realmente) impostare la lingua della sua pagina. Questo può essere fatto aggiungendo l'attributo [lang](/it/docs/Web/HTML/Reference/Global_attributes/lang) al tag HTML di apertura (come visto nel [meta-example.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/the-html-head/meta-example.html) e mostrato qui sotto.)

```html
<html lang="en-US">
  …
</html>
```

Questo è utile in molti modi. Il suo documento HTML verrà indicizzato più efficacemente dai motori di ricerca se la sua lingua è impostata (permettendogli di apparire correttamente nei risultati specifici della lingua, ad esempio), ed è utile per le persone con incapacità visive che utilizzano lettori di schermo (ad esempio, la parola "six" esiste sia in francese che in inglese, ma viene pronunciata in modo diverso).

Può anche impostare sezioni del suo documento per essere riconosciute come lingue diverse. Ad esempio, potremmo impostare la nostra sezione in giapponese per essere riconosciuta come giapponese, così:

```html
<p>Japanese example: <span lang="ja">ご飯が熱い。</span>.</p>
```

Questi codici sono definiti dallo standard [ISO 639-1](https://en.wikipedia.org/wiki/ISO_639-1). Può trovare ulteriori informazioni in [Language tags in HTML and XML](https://www.w3.org/International/articles/language-tags/).

## Riassunto

Questo segna la fine del nostro rapido tour dell'HTML head — c'è molto di più che può fare qui dentro, ma un tour esauriente sarebbe noioso e confuso in questa fase, e volevamo solo darle un'idea delle cose più comuni che troverà lì per ora! Nel prossimo articolo, daremo uno sguardo a [Intestazioni e paragrafi in HTML](/it/docs/Learn_web_development/Core/Structuring_content/Headings_and_paragraphs).

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Basic_HTML_syntax", "Learn_web_development/Core/Structuring_content/Headings_and_paragraphs", "Learn_web_development/Core/Structuring_content")}}
