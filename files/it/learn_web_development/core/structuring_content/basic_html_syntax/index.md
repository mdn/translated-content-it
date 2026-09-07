---
title: Sintassi HTML di base
slug: Learn_web_development/Core/Structuring_content/Basic_HTML_syntax
l10n:
  sourceCommit: d19dec85109590176f946fcceef48c787d578b1e
---

{{NextMenu("Learn_web_development/Core/Structuring_content/Webpage_metadata", "Learn_web_development/Core/Structuring_content")}}

In questo articolo vengono trattati i fondamenti di HTML, inclusi terminologia, sintassi e struttura. Lungo il percorso, saranno completate alcune sfide interattive per acquisire familiarità con la scrittura di HTML di base.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Installing_software">Software di base installato</a> e conoscenza di base del <a href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Dealing_with_files">lavoro con i file</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>L'anatomia di un elemento HTML — elemento, tag di apertura, contenuto, tag di chiusura, attributi.</li>
          <li>Il body HTML e il suo scopo come contenitore per il contenuto della pagina.</li>
          <li>Cosa sono i void element e in cosa differiscono dagli altri elementi.</li>
          <li>La necessità di un doctype all'inizio dei documenti HTML, incluso il suo scopo originale e il fatto che ora sia in qualche modo un artefatto storico.</li>
          <li>Comprendere che HTML deve essere annidato correttamente.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Cos'è HTML?

{{Glossary("HTML", "HTML")}} (HyperText Markup Language) è un _linguaggio di markup_ che comunica ai browser web come strutturare le pagine web visitate. HTML consiste in una serie di {{Glossary("Element", "elementi")}}, usati per racchiudere, avvolgere o _marcare_ diverse parti del contenuto affinché appaiano o si comportino in un determinato modo. I {{Glossary("Tag", "tag")}} di delimitazione possono trasformare il contenuto in un collegamento ipertestuale verso un'altra pagina, rendere le parole in corsivo e così via. Per esempio, si consideri la seguente riga di testo:

```plain
My cat is very grumpy
```

Si potrebbe specificare che questo testo è un paragrafo racchiudendolo nei tag di paragrafo ({{htmlelement("p")}}):

```html
<p>My cat is very grumpy</p>
```

Oppure, si potrebbe specificare che questo testo è un'intestazione di livello superiore racchiudendolo nei tag [`<h1>`](/it/docs/Web/HTML/Reference/Elements/Heading_Elements):

```html
<h1>My cat is very grumpy</h1>
```

HTML è contenuto all'interno di file di testo chiamati **documenti HTML**, o semplicemente **documenti**, con estensione `.html`. Mentre in precedenza si è parlato di pagine web, un documento HTML contiene il contenuto della pagina web e ne specifica la struttura.

Il file HTML più comune che si incontrerà è `index.html`, generalmente usato per contenere il contenuto della pagina iniziale di un sito web. È anche comune trovare sottocartelle contenenti i propri file `index.html`, per cui un sito web può avere più file index in posizioni differenti.

> [!NOTE]
> I tag in HTML non distinguono tra maiuscole e minuscole. Ciò significa che possono essere scritti in maiuscolo o in minuscolo. Per esempio, un tag {{htmlelement("title")}} potrebbe essere scritto come `<title>`, `<TITLE>`, `<Title>`, `<TiTlE>` e così via, e funzionerà comunque. Tuttavia, è buona pratica scrivere tutti i tag in minuscolo per coerenza e leggibilità.

## Anatomia di un elemento HTML

Esploriamo ulteriormente l'elemento paragrafo della sezione precedente:

![Un esempio di frammento di codice che dimostra la struttura di un elemento HTML.<p> My cat is very grumpy </p>.](grumpy-cat-small.png)

L'elemento completo è composto da:

- **Il tag di apertura:** consiste nel nome dell'elemento (in questo esempio, _p_ per paragraph), racchiuso tra parentesi angolari di apertura e chiusura. Questo tag di apertura indica dove l'elemento inizia o comincia ad avere effetto. In questo esempio, precede l'inizio del testo del paragrafo.
- **Il contenuto:** è il contenuto dell'elemento. In questo esempio, è il testo del paragrafo — "My cat is very grumpy".
- **Il tag di chiusura:** è uguale al tag di apertura, tranne per il fatto che include una barra in avanti prima del nome dell'elemento. Indica dove termina l'elemento. Omettere un tag di chiusura è un errore comune tra i principianti che può produrre risultati insoliti.

> [!NOTE]
> Visita lo scrim [HTML tags](https://scrimba.com/learn-html-and-css-c0p/~02?via=mdn) <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> del partner di apprendimento Scrimba per una spiegazione interattiva dei tag HTML.

### Creare il primo elemento HTML

Ecco un po' di pratica nella scrittura di elementi HTML:

1. Fare clic su **"Play"** nel blocco di codice sottostante per modificare l'esempio nel Playground MDN.
2. Racchiudere la riga di testo con i tag `<em>` e `</em>`. Per _aprire l'elemento_, inserire il tag di apertura (`<em>`) all'inizio della riga. Per _chiudere l'elemento_, inserire il tag di chiusura (`</em>`) alla fine della riga. Così facendo, il testo renderizzato verrà formattato in corsivo.
3. Per sperimentare ulteriormente, provare a cercare altri [elementi HTML](/it/docs/Web/HTML/Reference/Elements) e applicarli all'esempio di testo.

In caso di errore, è possibile cancellare il lavoro usando il pulsante _Reset_ nel Playground MDN. Se si rimane bloccati, è possibile visualizzare la soluzione sotto il blocco di codice.

```html live-sample___basic_html_1
This is my text.
```

{{ EmbedLiveSample('basic_html_1', "100%", 60) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

La riga HTML completata dovrebbe apparire così:

```html
<em>This is my text.</em>
```

</details>

### Annidare elementi

Gli elementi possono essere inseriti all'interno di altri elementi. Questa operazione è chiamata _annidamento_. Se si volesse indicare che il gatto è **molto** scontroso, si potrebbe racchiudere la parola _molto_ in un elemento {{htmlelement("strong")}}, che applica una formattazione del testo forte:

```html
<p>My cat is <strong>very</strong> grumpy.</p>
```

In questo blocco di codice, il testo "My cat is very grumpy." è interamente definito come paragrafo. La parola "very" è inoltre definita come di forte importanza.

Esiste un modo corretto e uno sbagliato di effettuare l'annidamento. Nel blocco di codice precedente, viene aperto prima l'elemento `<p>`, quindi viene aperto l'elemento `<strong>`. Per un annidamento corretto, viene chiuso prima l'elemento `<strong>`, quindi viene chiuso l'elemento `<p>`.

Il seguente è un esempio del modo _sbagliato_ di effettuare l'annidamento:

```html-nolint example-bad
<p>My cat is <strong>very grumpy.</p></strong>
```

I **tag devono aprirsi e chiudersi in modo da trovarsi uno all'interno o all'esterno dell'altro**. Poiché gli elementi si sovrappongono nel blocco di codice precedente, il browser deve indovinare l'intenzione. Questo tipo di supposizione può produrre risultati imprevisti.

### Void element

Non tutti gli elementi seguono lo schema di un tag di apertura, contenuto e poi tag di chiusura. Alcuni elementi consistono in un singolo tag, generalmente usato per inserire/incorporare qualcosa nel documento. Tali elementi sono chiamati {{Glossary("void_element", "void element")}}, ovvero "elementi che non possono contenere altro contenuto HTML".

Per esempio, l'elemento {{htmlelement("br")}} inserisce un'interruzione di riga in una riga di testo, facendo sì che il testo vada a capo su più righe:

```html live-sample___void-example
<p>
  This is a single paragraph, but we are going to <br />break it onto two lines.
</p>
```

Il rendering è il seguente:

{{ EmbedLiveSample('void-example', "100%", 100) }}

> [!NOTE]
> In alcuni esempi HTML, sarà presente un `/` aggiunto alla fine del tag di un void element, per esempio `<br />`. Si tratta di uno stile di sintassi di markup differente, che non è errato, ma questa "barra di chiusura" non è necessaria.

## Attributi

Gli elementi possono anche avere attributi. Gli attributi appaiono così:

![Tag di paragrafo con l'attributo 'class="editor-note"' evidenziato](grumpy-cat-attribute-small.png)

Gli attributi contengono informazioni aggiuntive sull'elemento che non fanno parte del suo contenuto. L'attributo **`class`** fornisce un nome identificativo che può essere usato per selezionare l'elemento con stili (CSS) o informazioni di scripting (JavaScript).

Un attributo dovrebbe avere:

- Uno spazio tra l'attributo e il nome dell'elemento. Quando un elemento ha più di un attributo, anche gli attributi dovrebbero essere separati da spazi.
- Il nome dell'attributo, seguito da un segno di uguale (`=`).
- Un valore dell'attributo, racchiuso tra virgolette di apertura e chiusura.

### Aggiungere attributi a un elemento

Ora è di nuovo il momento di esercitarsi. In questa sezione verrà esplorato l'elemento {{htmlelement("img")}}, usato per visualizzare un'immagine nella pagina. L'elemento `<img>` può accettare diversi attributi, inclusi:

- `src`: un attributo **obbligatorio** che specifica l'{{Glossary("URL", "URL")}} (indirizzo web) dell'immagine. Per esempio: `src="https://mdn.github.io/shared-assets/images/examples/fx-nightly-512.png"`.
- `alt`: specifica una descrizione testuale dell'immagine per le persone che non possono vederla. Per esempio: `alt="The Firefox Nightly icon"`. Questo attributo non è tecnicamente obbligatorio, ma è consigliabile fornire una descrizione testuale per tutte le immagini che trasmettono significato, invece di essere puramente decorative.
- `width`: specifica la larghezza dell'immagine in pixel. Per esempio: `width="300"`.
- `height`: specifica l'altezza dell'immagine in pixel. Per esempio: `height="300"`.

Seguire i passaggi seguenti per completare l'attività:

1. Fare clic su **"Play"** nel blocco di codice sottostante per modificare l'esempio nel Playground MDN.
2. Trovare online un'immagine preferita, fare clic destro su di essa e premere _Copia indirizzo immagine/collegamento_. In alternativa, copiare l'URL dell'immagine riportato sopra.
3. Tornando al Playground MDN, aggiungere l'attributo `src` all'elemento `<img>` e impostarne il valore all'URL del passaggio 2.
4. Impostare l'attributo `alt` a una descrizione appropriata dell'immagine.
5. Impostare l'attributo `width` a un valore come `300`, in modo da poter vedere meglio l'immagine nel pannello di output. Regolare il valore se necessario.

In caso di errore, è possibile cancellare il lavoro usando il pulsante _Reset_ nel Playground MDN. Se si rimane bloccati, è possibile visualizzare la soluzione sotto il blocco di codice.

```html live-sample___basic_html_2
<img />
```

{{ EmbedLiveSample('basic_html_2', "100%", 60) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

L'elemento HTML completato dovrebbe apparire più o meno così:

```html
<img
  src="https://mdn.github.io/shared-assets/images/examples/fx-nightly-512.png"
  alt="A description of the image"
  width="300" />
```

</details>

### Attributi booleani

Talvolta si vedranno attributi HTML scritti senza valori. Questi sono chiamati {{Glossary("Boolean/HTML", "attributi booleani")}}. Quando viene aggiunto un attributo booleano, il suo valore è impostato su `true`, indipendentemente dal valore che gli viene assegnato, anche se non viene assegnato alcun valore. Se un attributo non è incluso in un tag HTML, il suo valore è impostato su `false`.

Per esempio, si consideri l'attributo [`disabled`](/it/docs/Web/HTML/Reference/Elements/input#disabled), che può essere assegnato agli elementi {{htmlelement("input")}} dei moduli per impedire all'utente di immettervi dati. Per esempio:

```html live-sample___boolean-example
<label for="first-input">This input is disabled</label>
<input id="first-input" type="text" disabled="disabled" />
<br />
```

Come abbreviazione, è accettabile scrivere l'attributo `disabled` senza un valore:

```html live-sample___boolean-example
<label for="second-input">This input is also disabled</label>
<input id="second-input" type="text" disabled />
<br />
```

Come riferimento, viene fornito anche un elemento `<input>` non disabilitato per poter confrontare le differenze. Si noti come gli input `disabled` appaiano leggermente in grigio nel rendering seguente:

```html live-sample___boolean-example
<label for="third-input">This input isn't disabled; you can type into it</label>
<input id="third-input" type="text" />
```

I frammenti HTML precedenti vengono renderizzati così:

{{ EmbedLiveSample('boolean-example', "100%", 100) }}

> [!NOTE]
> Gli elementi {{htmlelement("label")}} inclusi nel codice precedente forniscono un modo per associare etichette descrittive agli elementi dei moduli. Sono stati inclusi perché è una buona pratica e per fornire una certa separazione tra gli input del modulo.

### Omettere le virgolette attorno ai valori degli attributi

In determinate circostanze, è consentito omettere le virgolette attorno ai valori degli attributi. Tuttavia, poiché questo può danneggiare il markup in altre circostanze, è consigliabile **includere sempre** le virgolette. Esploriamo il motivo.

L'elemento nel frammento di codice seguente, {{htmlelement("a")}}, è chiamato **ancora**. Le ancore racchiudono testo e lo trasformano in collegamenti. L'attributo `href` specifica l'URL a cui punta il collegamento. È possibile omettere le virgolette attorno al valore dell'attributo `href` mostrato sotto senza conseguenze negative, perché non contiene spazi:

```html
<a href=https://www.mozilla.org/>favorite website</a>
```

Tuttavia, omettendo le virgolette dai valori degli attributi _con_ spazi, si incontrano rapidamente dei problemi. Si consideri l'attributo `title` mostrato sotto, che fornisce una descrizione della pagina collegata ("The Mozilla homepage") che dovrebbe apparire come tooltip quando il puntatore del mouse passa sopra il collegamento.

```html-nolint example-bad live-sample___bad-no-quotes
<a href=https://www.mozilla.org/ title=The Mozilla homepage>favorite website</a>
```

Poiché le virgolette non sono incluse attorno al valore dell'attributo `title`, il browser lo interpreta come tre attributi: un attributo `title` con il valore `The` e due attributi booleani — `Mozilla` e `homepage`. Ovviamente, questo non è ciò che era previsto. Se si utilizza un dispositivo con puntatore del mouse, è possibile provare a passare sopra il collegamento per visualizzare il tooltip del titolo: verrà mostrato "The" anziché l'atteso "The Mozilla homepage".

{{ EmbedLiveSample('bad-no-quotes', 700, 100) }}

Includere sempre le virgolette attorno ai valori degli attributi. Evita errori e comportamenti indesiderati e rende il codice più leggibile.

### Virgolette singole o doppie?

In questo articolo, tutti i valori degli attributi sono stati racchiusi tra virgolette doppie. Tuttavia, in alcuni codici HTML potrebbero essere usate virgolette singole. È una questione di stile ed è possibile scegliere liberamente quale si preferisce. Entrambe queste righe sono equivalenti:

```html-nolint
<a href='https://www.example.com'>A link to my example.</a>

<a href="https://www.example.com">A link to my example.</a>
```

Assicurarsi di non mescolare virgolette singole e doppie. L'esempio seguente mescola le virgolette, causando errori perché, dal punto di vista del browser, il valore dell'attributo `href` non è stato terminato:

```html-nolint example-bad
<a href="https://www.example.com'>A link to my example.</a>
```

Usando un tipo di virgolette, è possibile includere l'altro tipo _all'interno_ dei valori degli attributi. Questo funziona correttamente:

```html
<a href="https://www.example.com" title="Isn't this fun?">
  A link to my example.
</a>
```

Per usare virgolette all'interno di altre virgolette dello stesso tipo, singole o doppie, è possibile usare i [riferimenti ai caratteri](#riferimenti-ai-caratteri-inclusi-caratteri-speciali-in-html). Per esempio, questo non funzionerà:

```html-nolint example-bad
<a href="https://www.example.com" title="An "interesting" reference">A link to my example.</a>
```

È invece necessario fare così:

```html-nolint
<a href="https://www.example.com" title="An &quot;interesting&quot; reference">A link to my example.</a>
```

## Anatomia di un documento HTML

I singoli elementi HTML non sono molto utili da soli. Esaminiamo ora come i singoli elementi si combinano per formare un'intera pagina HTML.

L'esempio seguente è una pagina web completa molto semplice:

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

Le parti di questo esempio sono le seguenti:

1. `<!doctype html>`: il doctype. Quando HTML era agli inizi, nel 1991-1992, i doctype dovevano agire come collegamenti a un insieme di regole che la pagina HTML doveva seguire per essere considerata buon HTML. I doctype avevano un aspetto simile a questo:

   ```html
   <!doctype html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
   ```

   Oggi, il doctype è un artefatto storico che deve essere incluso affinché tutto il resto funzioni correttamente. `<!doctype html>` è la stringa di caratteri più breve che conta come doctype valido e dovrebbe essere inclusa all'inizio di tutte le pagine web. Questo è tutto ciò che occorre sapere.

2. `<html></html>`: l'elemento {{htmlelement("html")}}. Questo elemento racchiude tutto il contenuto della pagina. Talvolta è noto come elemento radice.
3. `<head></head>`: l'elemento {{htmlelement("head")}}. Questo elemento agisce come contenitore per informazioni sulla pagina che _non_ fanno parte del contenuto che gli utenti vedranno. Possono includere parole chiave e una descrizione della pagina da mostrare nei risultati di ricerca, CSS per definire lo stile del contenuto, dichiarazioni del set di caratteri e altro. Nel prossimo articolo verrà approfondita la sezione head della pagina.
4. `<meta charset="utf-8">`: un elemento {{htmlelement("meta")}}. Questo elemento rappresenta metadati che descrivono la pagina. L'attributo [`charset`](/it/docs/Web/HTML/Reference/Elements/meta#charset) specifica la codifica dei caratteri che verrà usata dal documento. UTF-8 include la maggior parte dei caratteri della stragrande maggioranza delle lingue scritte umane, il che significa che la pagina sarà in grado di visualizzare correttamente lingue diverse. Non esiste alcun motivo per non impostarla e può aiutare a evitare alcuni problemi in seguito.
5. `<title></title>`: l'elemento {{htmlelement("title")}}. Imposta il titolo della pagina, che appare nella scheda del browser in cui è caricata la pagina. Il titolo della pagina viene usato anche per descrivere la pagina quando viene aggiunta ai segnalibri.
6. `<body></body>`: l'elemento {{htmlelement("body")}}. Contiene _tutto_ il contenuto visualizzato nella pagina, inclusi testo, immagini, video, giochi, tracce audio riproducibili e così via.

### Aggiungere alcune funzionalità a un documento HTML

A questo punto, è utile esercitarsi a scrivere contenuti HTML leggermente più consistenti. Per farlo, ci sono un paio di opzioni: creare l'HTML sul computer locale oppure usare il Playground MDN come negli esempi precedenti.

#### Configurazione dell'esempio

- Per farlo sul computer locale:
  1. Copiare l'esempio di pagina HTML riportato nella sezione precedente e incollarlo in un nuovo file nell'editor di codice. È inoltre possibile trovare questo [modello HTML di base](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/getting-started/index.html) nel repository GitHub di MDN.
  2. Apportare alla pagina le modifiche indicate nelle [istruzioni](#istruzioni-dellesempio).
  3. Salvare il file come `index.html`, quindi caricarlo in una nuova scheda del browser per vedere i risultati.
- Per farlo nel Playground MDN, fare clic su **"Play"** nel pannello di output sottostante per modificare l'esempio, quindi seguire le [istruzioni](#istruzioni-dellesempio). In caso di errore, è possibile cancellare il lavoro usando il pulsante _Reset_ nel Playground MDN.

```html hidden live-sample___basic_html_3
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

{{ EmbedLiveSample('basic_html_3', "100%", 60) }}

#### Istruzioni dell'esempio

Ecco le istruzioni da seguire:

1. Subito sotto il tag di apertura dell'elemento {{htmlelement("body")}}, aggiungere un titolo principale per il documento. Dovrebbe essere racchiuso dai tag di apertura e chiusura `<h1></h1>`.
2. Modificare il contenuto del paragrafo per includere un testo su un argomento ritenuto interessante.
3. Evidenziare in grassetto le parole importanti racchiudendole in un elemento {{htmlelement("strong")}}.
4. Aggiungere due collegamenti al paragrafo. Questo si ottiene usando l'elemento {{htmlelement("a")}}.
5. Aggiungere un'immagine al documento sotto il paragrafo, come [spiegato in precedenza](#aggiungere-attributi-a-un-elemento). Se è troppo grande per essere visualizzata, aggiungere un attributo `width` per ridurne le dimensioni.

Se si rimane bloccati, è possibile visualizzare qui una possibile soluzione:

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il contenuto del body dell'elemento HTML completato dovrebbe apparire più o meno così:

```html
<h1>Some music</h1>
<p>
  I really enjoy <strong>playing the drums</strong>. One of my favorite drummers
  is Neal Peart, who used to play in the band
  <a href="https://en.wikipedia.org/wiki/Rush_%28band%29">Rush</a>. My favorite
  Rush album is currently
  <a href="https://www.deezer.com/album/942295">Moving Pictures</a>.
</p>
<img
  src="https://www.cygnus-x1.net/links/rush/images/albums/sectors/sector2-movingpictures-cover-s.jpg"
  alt="Rush Moving Pictures album cover"
  width="300" />
```

</details>

## Spazi bianchi in HTML

Negli esempi precedenti sono stati inclusi molti spazi bianchi nel codice. Nella maggior parte dei casi sono completamente facoltativi e vengono inclusi principalmente per rendere il codice più leggibile. Per esempio, questi due frammenti di codice sono equivalenti:

```html-nolint live-sample___whitespace-example
<p id="noWhitespace">Dogs are silly.</p>

<p id="whitespace">Dogs
    are
        silly.</p>
```

Entrambi hanno esattamente lo stesso rendering:

{{ EmbedLiveSample('whitespace-example', 700, 100) }}

In quasi tutti gli elementi, con eccezioni come {{htmlelement("pre")}}, indipendentemente dalla quantità di spazi bianchi usata nel contenuto dell'elemento HTML, il parser HTML riduce ogni sequenza di spazi bianchi a un singolo spazio durante il rendering del codice.

La scelta di uno stile preferito di formattazione del codice dipende dallo sviluppatore. È comune assegnare a ogni elemento annidato un rientro di due spazi maggiore rispetto a quello dell'elemento che lo contiene; questo è lo stile usato su MDN.

Per esempio:

```html
<section>
  <div>
    <p>A paragraph of content.</p>
  </div>
</section>
```

## Riferimenti ai caratteri: includere caratteri speciali in HTML

In HTML, i caratteri `<`, `>`, `"`, `'` e `&` sono caratteri speciali. Fanno parte della sintassi HTML stessa. Come è possibile quindi includere questi caratteri speciali nel testo? Per esempio, come si può usare una e commerciale letterale o un segno di minore nel contenuto senza che venga interpretato come codice?

Questo viene fatto con i {{Glossary("character_reference", "riferimenti ai caratteri")}}. Si tratta di codici speciali che rappresentano caratteri, da usare proprio in queste circostanze. Ogni riferimento a un carattere inizia con una e commerciale (&) e termina con un punto e virgola (;).

| Carattere letterale | Riferimento al carattere equivalente |
| ------------------- | ------------------------------------ |
| <                   | `&lt;`                               |
| >                   | `&gt;`                               |
| "                   | `&quot;`                             |
| '                   | `&apos;`                             |
| &                   | `&amp;`                              |

I riferimenti ai caratteri sono abbastanza facili da ricordare perché il testo usato è un'abbreviazione del nome del carattere: per esempio, "lt" = "less than", "quot" = "quotation" e "amp" = "ampersand". Per ulteriori informazioni sui riferimenti alle entità, vedere [List of XML and HTML character entity references](https://en.wikipedia.org/wiki/List_of_XML_and_HTML_character_entity_references) (Wikipedia).

Nell'esempio seguente sono presenti due paragrafi:

```html-nolint live-sample___entity-ref-example
<p>In HTML, you define a paragraph using the <p> element.</p>

<p>In HTML, you define a paragraph using the &lt;p&gt; element.</p>
```

Il rendering è il seguente:

{{ EmbedLiveSample('entity-ref-example', 700, 150) }}

Si può notare che il primo paragrafo non viene interpretato correttamente, perché il browser ha interpretato la seconda istanza di `<p>` come l'inizio di un nuovo paragrafo. Il secondo paragrafo viene renderizzato correttamente perché le parentesi angolari del contenuto "&lt;p&gt;" sono rappresentate da riferimenti ai caratteri.

> [!NOTE]
> Non è necessario usare riferimenti alle entità per altri simboli, poiché i browser moderni gestiscono correttamente i simboli effettivi purché la [codifica dei caratteri HTML sia impostata su UTF-8](/it/docs/Learn_web_development/Core/Structuring_content/Webpage_metadata#specifying_your_documents_character_encoding).

## Commenti HTML

HTML dispone di un meccanismo per scrivere commenti nel codice. I browser ignorano i commenti, pertanto sono invisibili all'utente. Lo scopo dei commenti è consentire l'inclusione di note nel codice per spiegare come funziona. Questo è molto utile quando si torna a una base di codice dopo un periodo sufficientemente lungo da non ricordarla più, oppure quando un'altra persona inizia a lavorarci senza averla mai vista prima.

Per scrivere un commento HTML, racchiuderlo tra i marcatori speciali `<!--` e `-->`, come mostrato di seguito:

```html live-sample___comment-example
<p>I'm not inside a comment</p>

<!-- <p>I am!</p> -->
```

Il rendering di questo codice è il seguente:

{{ EmbedLiveSample('comment-example', 700, 100) }}

Nell'output live viene visualizzato solo il primo paragrafo; la seconda riga non viene renderizzata perché è un commento HTML.

## Riepilogo

È stata raggiunta la fine dell'articolo. Ci auguriamo che questa panoramica delle basi di HTML sia stata utile.

A questo punto, dovrebbe essere chiaro l'aspetto di HTML e il suo funzionamento a livello di base. Dovrebbe inoltre essere possibile scrivere alcuni elementi e attributi. Gli articoli successivi di questo modulo approfondiscono alcuni degli argomenti introdotti qui e presentano ulteriori argomenti.

> [!NOTE]
> Man mano che si apprende di più su HTML, è consigliabile imparare anche le basi di [CSS](/it/docs/Learn_web_development/Core/Styling_basics), il linguaggio usato per definire lo stile delle pagine web, per esempio modificando colori, font e spaziatura. HTML e CSS vengono usati insieme nella maggior parte delle pagine web e impararli contemporaneamente può essere efficace.

{{NextMenu("Learn_web_development/Core/Structuring_content/Webpage_metadata", "Learn_web_development/Core/Structuring_content")}}
