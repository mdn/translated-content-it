---
title: Cosa c'è nell'head? I metadati della pagina web
short-title: Metadati della pagina web
slug: Learn_web_development/Core/Structuring_content/Webpage_metadata
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Basic_HTML_syntax", "Learn_web_development/Core/Structuring_content/Headings_and_paragraphs", "Learn_web_development/Core/Structuring_content")}}

L'{{Glossary("Head", "head")}} di un documento HTML è la parte che non viene visualizzata nel browser quando la pagina viene caricata. Contiene informazioni sui metadati come il {{htmlelement("title")}} della pagina, collegamenti a {{Glossary("CSS", "CSS")}} (se scegli di stilizzare il tuo contenuto HTML con CSS), collegamenti a favicon personalizzati e altri metadati (dati sull'HTML, come l'autore e parole chiave importanti che descrivono il documento).

I browser web utilizzano le informazioni contenute nell'{{Glossary("Head", "head")}} per rendere correttamente il documento HTML. In questo articolo tratteremo tutto quanto sopra e altro ancora, per darti una buona base per lavorare con il markup.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con l'HTML, come coperto nella lezione precedente.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>L'HTML head e il suo scopo come contenitore dei metadati per il documento.</li>
          <li>Impostare la codifica dei caratteri e il titolo del documento.</li>
          <li>Fornire metadati per i motori di ricerca.</li>
          <li>Collegamento a icone per l'uso su browser e piattaforme mobili.</li>
          <li>Collegamento a fogli di stile e file di script.</li>
          <li>La necessità di impostare la lingua di un documento utilizzando l'attributo <code>lang</code> nel tag di apertura <code>&lt;html&gt;</code>.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Cos'è l'HTML head?

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

L'HTML head è il contenuto dell'elemento {{htmlelement("head")}}. A differenza dei contenuti dell'elemento {{htmlelement("body")}} (che vengono visualizzati sulla pagina quando caricati in un browser), il contenuto dell'head non viene mostrato sulla pagina. Invece, il compito dell'head è di contenere {{Glossary("Metadata", "metadati")}} sul documento. Nell'esempio sopra, l'head è abbastanza piccolo:

```html
<head>
  <meta charset="utf-8" />
  <title>My test page</title>
</head>
```

Tuttavia, nelle pagine più grandi, l'head può diventare piuttosto grande. Prova a visitare alcuni dei tuoi siti web preferiti e usa gli [strumenti per sviluppatori](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools) per controllare il contenuto dei loro head. L'obiettivo qui non è mostrarti come usare tutto ciò che può essere messo nell'head, ma piuttosto insegnarti come utilizzare gli elementi principali che vorrai includere nell'head e darti un po' di familiarità. Iniziamo.

## Aggiungere un titolo

Abbiamo già visto l'elemento {{htmlelement("title")}} in azione — questo può essere usato per aggiungere un titolo al documento. Tuttavia, potrebbe confondersi con l'elemento {{htmlelement("Heading_Elements", "h1")}}, che viene utilizzato per aggiungere un'intestazione di livello superiore al contenuto del tuo body — questo è anche a volte indicato come il titolo della pagina. Ma sono cose diverse!

- L'elemento {{htmlelement("Heading_Elements", "h1")}} appare sulla pagina quando viene caricata nel browser — generalmente dovrebbe essere usato una volta per pagina, per segnare il titolo del contenuto della tua pagina (il titolo della storia, o il titolo di una notizia, o qualsiasi altra cosa sia appropriata al tuo uso.)
- L'elemento {{htmlelement("title")}} è un metadato che rappresenta il titolo del documento HTML complessivo (non il contenuto del documento.)

### Apprendimento attivo: Ispezionare un esempio

1. Per iniziare questo apprendimento attivo, vogliamo che tu vada al nostro repository GitHub e scarichi una copia della nostra [pagina title-example.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/the-html-head/title-example.html). Per farlo, puoi

   1. Copiare e incollare il codice dalla pagina e incollarlo in un nuovo file di testo nel tuo editor di codice, quindi salvarlo in un posto appropriato.
   2. Premere il pulsante "Raw" sulla pagina di GitHub, che farà apparire il codice grezzo (possibilmente in una nuova scheda del browser). Successivamente, scegli il menu del browser _Salva Pagina Come…_ e scegli un posto appropriato per salvare il file.

2. Ora apri il file nel tuo browser. Dovresti vedere qualcosa di simile:

   ![Una pagina web con testo 'title' nella scheda della pagina del browser e testo 'h1' come intestazione della pagina nel corpo del documento.](title-example.png)

   Ora dovrebbe essere completamente ovvio dove appare il contenuto di `<h1>` e dove appare il contenuto di `<title>`!

3. Dovresti anche provare ad aprire il codice nel tuo editor di codice, modificare il contenuto di questi elementi, quindi aggiornare la pagina nel browser. Divertiti un po' con esso.

Anche il contenuto dell'elemento `<title>` viene utilizzato in altri modi. Per esempio, se provi a salvare la pagina nei segnalibri (_Segnalibri > Aggiungi pagina ai segnalibri_ o l'icona della stella nella barra degli URL in Firefox), vedrai che il contenuto di `<title>` viene riempito come nome suggerito del segnalibro.

![Una pagina web che viene aggiunta ai segnalibri in Firefox. Il nome del segnalibro è stato automaticamente riempito con il contenuto dell'elemento 'title'](bookmark-example.png)

Il contenuto di `<title>` viene inoltre utilizzato nei risultati di ricerca, come vedrai di seguito.

## Metadati: l'elemento `<meta>`

I metadati sono dati che descrivono dati, e l'HTML ha un modo "ufficiale" di aggiungere metadati a un documento — l'elemento {{htmlelement("meta")}}. Naturalmente, anche l'altro materiale di cui parliamo in questo articolo potrebbe essere considerato come metadati. Ci sono molti tipi diversi di elementi `<meta>` che possono essere inclusi nell`<head>` della tua pagina, ma non tenteremo di spiegarli tutti in questa fase, poiché sarebbe solo troppo confuso. Invece, spiegheremo alcune cose che potresti vedere comunemente, solo per darti un'idea.

### Specificare la codifica dei caratteri del documento

Nell'esempio che abbiamo visto sopra, questa riga era inclusa:

```html
<meta charset="utf-8" />
```

Questo elemento specifica la codifica dei caratteri del documento — il set di caratteri che il documento è autorizzato a utilizzare. `utf-8` è un set di caratteri universale che include praticamente qualsiasi carattere di qualsiasi lingua umana. Questo significa che la tua pagina web sarà in grado di gestire la visualizzazione di qualsiasi lingua; pertanto è una buona idea impostarlo su ogni pagina web che crei! Per esempio, la tua pagina potrebbe gestire sia l'inglese che il giapponese senza problemi:

![Una pagina web contenente caratteri in inglese e giapponese, con la codifica dei caratteri impostata su universale, o utf-8. Entrambe le lingue si displayano correttamente.](correct-encoding.png)

Se imposti la tua codifica dei caratteri su `ISO-8859-1`, per esempio (il set di caratteri per l'alfabeto latino), il rendering della tua pagina potrebbe apparire tutto confuso:

![Una pagina web contenente caratteri in inglese e giapponese, con la codifica dei caratteri impostata su latin. I caratteri giapponesi non si visualizzano correttamente.](bad-encoding.png)

> [!NOTE]
> Alcuni browser (come Chrome) correggono automaticamente le codifiche errate, quindi a seconda del browser che utilizzi, potresti non vedere questo problema. Dovresti comunque impostare una codifica di `utf-8` sulla tua pagina per evitare qualsiasi potenziale problema in altri browser.

### Apprendimento attivo: Sperimenta con la codifica dei caratteri

Per provare, rivisita il semplice template HTML che hai ottenuto nella sezione precedente su `<title>` (la [pagina title-example.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/the-html-head/title-example.html)), prova a cambiare il valore del meta charset in `ISO-8859-1`, e aggiungi il giapponese alla tua pagina. Questo è il codice che abbiamo usato:

```html
<p>Japanese example: ご飯が熱い。</p>
```

### Aggiungere un autore e una descrizione

Molti elementi `<meta>` includono attributi `name` e `content`:

- `name` specifica il tipo di elemento meta che è; che tipo di informazioni contiene.
- `content` specifica il contenuto meta effettivo.

Due di tali elementi meta che è utile includere sulla tua pagina definiscono l'autore della pagina e forniscono una descrizione concisa della pagina. Guardiamo un esempio:

```html
<meta name="author" content="Chris Mills" />
<meta
  name="description"
  content="The MDN Web Docs Learning Area aims to provide
complete beginners to the Web with all they need to know to get
started with developing websites and applications." />
```

Specificare un autore è utile in molti modi: è utile essere in grado di capire chi ha scritto la pagina, se hai domande sul contenuto e desideri contattarlo. Alcuni sistemi di gestione dei contenuti hanno funzionalità per estrarre automaticamente le informazioni sull'autore della pagina e renderle disponibili per tali scopi.

Specificare una descrizione che includa parole chiave relative al contenuto della tua pagina è utile poiché ha il potenziale di far apparire la tua pagina più in alto nelle ricerche rilevanti effettuate nei motori di ricerca (tali attività sono chiamate {{Glossary("SEO", "Search Engine Optimization")}}, o {{Glossary("SEO", "SEO")}}.)

### Apprendimento attivo: Utilizzo della descrizione nei motori di ricerca

La descrizione viene anche utilizzata nelle pagine dei risultati dei motori di ricerca. Facciamo un esercizio per esplorarlo

1. Vai alla [pagina principale di The Mozilla Developer Network](/en-US/).
2. Visualizza il sorgente della pagina (clic destro sulla pagina, scegli _Visualizza sorgente pagina_ dal menu contestuale.)
3. Trova il tag meta della descrizione. Sembrerà qualcosa del genere (anche se potrebbe cambiare nel tempo):

   ```html
   <meta
     name="description"
     content="The MDN Web Docs site
     provides information about Open Web technologies
     including HTML, CSS, and APIs for both websites and
     progressive web apps." />
   ```

4. Ora cerca "MDN Web Docs" nel tuo motore di ricerca preferito (abbiamo usato Google.) Noterai il `<meta>` della descrizione e il contenuto dell'elemento `<title>` utilizzato nel risultato di ricerca — decisamente utile avere!

   ![Un risultato di ricerca Yahoo per "Mozilla Developer Network"](mdn-search-result.png)

> [!NOTE]
> In Google, vedrai alcune sottopagine rilevanti di MDN Web Docs elencate sotto il link principale della home page — questi sono chiamati sitelinks, e sono configurabili negli [strumenti per webmaster di Google](https://search.google.com/search-console/about?hl=en) — un modo per migliorare i risultati di ricerca del tuo sito nel motore di ricerca di Google.

> [!NOTE]
> Molte funzionalità di `<meta>` non vengono più utilizzate. Per esempio, l'elemento `<meta>` delle parole chiave (`<meta name="keywords" content="riempi, con, le, tue, parole, chiave, qui">`) — che dovrebbe fornire parole chiave per consentire ai motori di ricerca di determinare la rilevanza di quella pagina per diversi termini di ricerca — viene ignorato dai motori di ricerca, perché gli spammer riempivano semplicemente l'elenco delle parole chiave con centinaia di termini, distorcendo i risultati.

### Altri tipi di metadati

Viaggiando sul web, troverai anche altri tipi di metadati. Molte delle funzionalità che vedrai sui siti web sono creazioni proprietarie progettate per fornire a determinati siti (come i siti di social networking) informazioni specifiche che possono utilizzare.

Per esempio, [Open Graph Data](https://ogp.me/) è un protocollo di metadati che Facebook ha inventato per fornire metadati più ricchi per i siti web. Nel codice sorgente di MDN Web Docs, troverai questo:

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

Un effetto di ciò è che quando colleghi MDN Web Docs su Facebook, il link appare insieme a un'immagine e una descrizione: un'esperienza più ricca per gli utenti.

![Dati del protocollo open graph dalla homepage di MDN come visualizzati su Facebook, mostrando un'immagine, titolo e descrizione.](facebook-output.png)

## Aggiungere icone personalizzate al tuo sito

Per arricchire ulteriormente il design del tuo sito, puoi aggiungere riferimenti a icone personalizzate nei tuoi metadati, e queste verranno visualizzate in determinati contesti. Quella più comunemente usata è la **favicon** (abbreviazione di "favorites icon", riferendosi al suo uso nelle liste "favorites" o segnalibri nei browser).

La semplice favicon esiste da molti anni. È la prima icona di questo tipo: un'icona quadrata di 16 pixel utilizzata in più posizioni. Potresti vedere (a seconda del browser) le favicon visualizzate nella scheda del browser contenente ciascuna pagina aperta, e accanto alle pagine nei segnalibri nel pannello dei segnalibri.

Una favicon può essere aggiunta alla tua pagina da:

1. Salvandola nella stessa directory della pagina indice del sito, salvata in formato `.ico` (la maggior parte supporta anche le favicon in formati più comuni come `.gif` o `.png`)
2. Aggiungendo la seguente riga nel blocco {{HTMLElement("head")}} del tuo HTML per farvi riferimento:

   ```html
   <link rel="icon" href="favicon.ico" type="image/x-icon" />
   ```

Ecco un esempio di una favicon in un pannello dei segnalibri:

![Il pannello dei segnalibri di Firefox, mostrando un esempio di segnalibro con una favicon visualizzata accanto ad esso.](bookmark-favicon.png)

Potresti anche avere bisogno di diverse icone per diversi contesti. Per esempio, troverai questo nel codice sorgente della homepage di MDN Web Docs:

```html
<link rel="icon" href="/favicon-48x48.[some hex hash].png" />
<link rel="apple-touch-icon" href="/apple-touch-icon.[some hex hash].png" />
```

Questo è un modo per far sì che il sito mostri un'icona quando salvato nella schermata iniziale di un dispositivo Apple. Potresti persino voler fornire icone diverse per diversi dispositivi, per assicurarti che l'icona sia visibile su tutti i dispositivi. Per esempio:

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

I commenti spiegano a cosa è destinata ciascuna icona — questi elementi coprono cose come fornire un'icona ad alta risoluzione da usare quando il sito web è salvato nella schermata iniziale di un iPad.

Non preoccuparti troppo di implementare tutti questi tipi di icone adesso — questa è una funzionalità piuttosto avanzata, e non ci si aspetta che tu abbia conoscenza di questo per progredire nel corso. Lo scopo principale qui è farti sapere cosa sono tali cose, nel caso tu ti imbatta in esse mentre navighi nei codici sorgenti di altri siti web. Se vuoi saperne di più su tutti questi valori e come sceglierli, leggi la pagina di riferimento dell'elemento {{HTMLElement("link")}}.

> [!NOTE]
> Se il tuo sito utilizza una Content Security Policy (CSP) per aumentare la sua sicurezza, la policy si applica alla favicon. Se riscontri problemi con il caricamento della favicon, verifica che l'intestazione {{HTTPHeader("Content-Security-Policy")}} del' `img-src` directive](/it/docs/Web/HTTP/Reference/Headers/Content-Security-Policy/img-src) non stia impedendo l'accesso ad essa.

## Applicare CSS e JavaScript a HTML

Quasi tutti i siti web che utilizzerai oggi impiegheranno {{Glossary("CSS", "CSS")}} per rendere il loro aspetto accattivante e {{Glossary("JavaScript", "JavaScript")}} per alimentare la funzionalità interattiva, come lettori video, mappe, giochi e altro. Questi sono più comunemente applicati a una pagina web utilizzando l'elemento {{htmlelement("link")}} e l'elemento {{htmlelement("script")}}, rispettivamente.

- L'elemento {{htmlelement("link")}} dovrebbe sempre andare nell'head del tuo documento. Questo prende due attributi, `rel="stylesheet"`, che indica che è il foglio di stile del documento, e `href`, che contiene il percorso del file del foglio di stile:

  ```html
  <link rel="stylesheet" href="my-css-file.css" />
  ```

- L'elemento {{htmlelement("script")}} dovrebbe anche andare nell'head e dovrebbe includere un attributo `src` contenente il percorso del JavaScript che vuoi caricare, e `defer` (un [attributo booleano](/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax#boolean_attributes)), che istruisce il browser a caricare il JavaScript dopo che la pagina ha finito di analizzare l'HTML. L'attributo `defer` è utile poiché garantisce che l'HTML sia tutto caricato prima che il JavaScript venga eseguito in modo da evitare errori dovuti al tentativo di JavaScript di accedere a un elemento HTML che non esiste ancora sulla pagina. Ci sono [diversi modi](/it/docs/Learn_web_development/Core/Scripting/What_is_JavaScript#script_loading_strategies) per gestire il caricamento del JavaScript sulla tua pagina, ma questo è il più affidabile da usare per i browser moderni.

  ```html
  <script src="my-js-file.js" defer></script>
  ```

  > [!NOTE]
  > L'elemento `<script>` potrebbe sembrare un {{Glossary("void_element", "elemento vuoto")}}, ma non lo è, e quindi necessita di un tag di chiusura. Invece di puntare a un file di script esterno, puoi anche scegliere di inserire il tuo script all'interno dell'elemento `<script>`.

### Apprendimento attivo: applicare CSS e JavaScript a una pagina

1. Per iniziare questo apprendimento attivo, prendi una copia del nostro file [meta-example.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/the-html-head/meta-example.html), [script.js](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/the-html-head/script.js) e [style.css](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/the-html-head/style.css), e salvali sul tuo computer nella stessa directory. Assicurati che siano salvati con i nomi e le estensioni di file corretti.
2. Apri il file HTML sia nel tuo browser che nel tuo editor di testo.
3. Seguendo le informazioni fornite sopra, aggiungi elementi {{htmlelement("link")}} e {{htmlelement("script")}} al tuo HTML, in modo che i tuoi CSS e JavaScript vengano applicati al tuo HTML.

Se fatto correttamente, quando salverai il tuo HTML e aggiornerai il tuo browser, dovresti essere in grado di vedere che le cose sono cambiate:

![Esempio che mostra una pagina con CSS e JavaScript applicati ad essa. Il CSS ha fatto diventare la pagina verde, mentre il JavaScript ha aggiunto un elenco dinamico alla pagina.](js-and-css.png)

- Il JavaScript ha aggiunto un elenco vuoto alla pagina. Ora, quando clicchi da qualche parte fuori dall'elenco, apparirà una finestra di dialogo che ti chiederà di inserire qualche testo per un nuovo elemento di lista. Quando premi il pulsante OK, verrà aggiunto un nuovo elemento all'elenco contenente il testo. Quando clicchi su un elemento esistente dell'elenco, apparirà una finestra di dialogo che ti permetterà di cambiare il testo dell'elemento.
- Il CSS ha causato che lo sfondo diventasse verde e il testo diventasse più grande. Ha anche stilizzato parte del contenuto che il JavaScript ha aggiunto alla pagina (la barra rossa con il bordo nero è lo stile che il CSS ha aggiunto all'elenco generato da JS.)

> [!NOTE]
> Se incontri difficoltà in questo esercizio e non riesci a far applicare il CSS/JS, prova a controllare la nostra pagina di esempio [css-and-js.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/the-html-head/css-and-js.html).

## Impostare la lingua principale del documento

Infine, vale la pena menzionare che puoi (e dovresti davvero) impostare la lingua della tua pagina. Questo può essere fatto aggiungendo l'[attributo lang](/it/docs/Web/HTML/Reference/Global_attributes/lang) al tag di apertura HTML (come visto nel [meta-example.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/the-html-head/meta-example.html) e mostrato qui sotto.)

```html
<html lang="en-US">
  …
</html>
```

Questo è utile in molti modi. Il tuo documento HTML sarà indicizzato in modo più efficace dai motori di ricerca se la sua lingua è impostata (permettendo che appaia correttamente nei risultati specifici per lingua, per esempio), ed è utile per persone con disabilità visive che utilizzano lettori di schermo (per esempio, la parola "six" esiste sia in francese che in inglese, ma viene pronunciata diversamente).

Puoi anche impostare sottosezioni del tuo documento per essere riconosciute come diverse lingue. Per esempio, possiamo impostare la nostra sezione in giapponese per essere riconosciuta come giapponese, in questo modo:

```html
<p>Japanese example: <span lang="ja">ご飯が熱い。</span>.</p>
```

Questi codici sono definiti dallo standard [ISO 639-1](https://en.wikipedia.org/wiki/ISO_639-1). Puoi trovare ulteriori informazioni sui tag delle lingue in [Language tags in HTML and XML](https://www.w3.org/International/articles/language-tags/).

## Sommario

Questo segna la fine del nostro rapido tour dell'HTML head — c'è molto di più che puoi fare qui, ma un tour esaustivo sarebbe noioso e confuso in questa fase, e volevamo solo darti un'idea delle cose più comuni che troverai lì per ora! Nel prossimo articolo, esamineremo [Intestazioni e paragrafi in HTML](/it/docs/Learn_web_development/Core/Structuring_content/Headings_and_paragraphs).

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Basic_HTML_syntax", "Learn_web_development/Core/Structuring_content/Headings_and_paragraphs", "Learn_web_development/Core/Structuring_content")}}
