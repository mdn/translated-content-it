---
title: Titoli e paragrafi
slug: Learn_web_development/Core/Structuring_content/Headings_and_paragraphs
l10n:
  sourceCommit: 2066cc916dfdcbb782340bf0ce562b230e947cba
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Webpage_metadata", "Learn_web_development/Core/Structuring_content/Emphasis_and_importance", "Learn_web_development/Core/Structuring_content")}}

Uno dei compiti principali di HTML è dare struttura al testo affinché un browser possa visualizzare un documento HTML nel modo previsto da chi lo sviluppa. Questo articolo spiega come {{Glossary("HTML", "HTML")}} possa essere usato per fornire la struttura fondamentale di una pagina definendo titoli e paragrafi.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con HTML, come illustrato in
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi HTML di base</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>Come creare una buona struttura del documento con titoli e contenuti sotto tali titoli.</li>
          <li>Usare HTML semantico anziché HTML di presentazione e capire perché è importante.</li>
          <li>La necessità di usare i livelli dei titoli in modo logico, ovvero senza saltare livelli né usarli arbitrariamente per ottenere una determinata dimensione del carattere (questo è compito di CSS).</li>
          <li>Vantaggi SEO: ad esempio, le parole chiave sono valorizzate nei titoli.</li>
          <li>Vantaggi per l'accessibilità: le tecnologie assistive (AT), come gli screen reader, usano i titoli (e altri punti di riferimento) come segnali per navigare tra i contenuti. I documenti HTML sono molto difficili da usare per gli utenti di AT senza titoli.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Titoli e paragrafi

La maggior parte dei testi strutturati è composta da titoli e paragrafi, sia quando si legge un racconto, un giornale, un manuale universitario, una rivista e così via.

![Un esempio di prima pagina di un giornale, che mostra l'uso di un titolo di livello superiore, sottotitoli e paragrafi.](newspaper_small.jpg)

I contenuti strutturati rendono l'esperienza di lettura più semplice e piacevole.

In HTML, ogni paragrafo deve essere racchiuso in un elemento {{htmlelement("p")}}, in questo modo:

```html
<p>I am a paragraph, oh yes I am.</p>
```

Ogni titolo deve essere racchiuso in un elemento di intestazione:

```html
<h1>I am the title of the story.</h1>
```

Esistono sei elementi di intestazione: {{htmlelement("Heading_Elements", "h1")}}, {{htmlelement("Heading_Elements", "h2")}}, {{htmlelement("Heading_Elements", "h3")}}, {{htmlelement("Heading_Elements", "h4")}}, {{htmlelement("Heading_Elements", "h5")}} e {{htmlelement("Heading_Elements", "h6")}}. Ogni elemento rappresenta un diverso livello di contenuto nel documento: `<h1>` rappresenta il titolo principale, `<h2>` rappresenta i sottotitoli, `<h3>` rappresenta i sottosottotitoli e così via.

## Implementare una gerarchia strutturale

Ad esempio, in questo racconto, l'elemento `<h1>` rappresenta il titolo del racconto, gli elementi `<h2>` rappresentano il titolo di ogni capitolo e gli elementi `<h3>` rappresentano le sottosezioni di ogni capitolo:

```html
<h1>The Crushing Bore</h1>

<p>By Chris Mills</p>

<h2>Chapter 1: The dark night</h2>

<p>
  It was a dark night. Somewhere, an owl hooted. The rain lashed down on the…
</p>

<h2>Chapter 2: The eternal silence</h2>

<p>Our protagonist could not so much as a whisper out of the shadowy figure…</p>

<h3>The specter speaks</h3>

<p>
  Several more hours had passed, when all of a sudden the specter sat bolt
  upright and exclaimed, "Please have mercy on my soul!"
</p>
```

Il significato degli elementi coinvolti dipende interamente dalla scelta dello sviluppatore, purché la gerarchia abbia senso. Durante la creazione di queste strutture, occorre solo tenere presenti alcune buone pratiche:

- Preferibilmente, si dovrebbe usare un singolo `<h1>` per pagina: questo è il titolo di livello superiore e tutti gli altri si trovano sotto di esso nella gerarchia.
- Assicurarsi di usare i titoli nell'ordine corretto nella gerarchia. Non usare elementi `<h3>` per rappresentare sottotitoli, seguiti da elementi `<h2>` per rappresentare sottosottotitoli: non avrebbe senso e porterebbe a risultati strani.
- Dei sei livelli di intestazione disponibili, l'obiettivo dovrebbe essere usarne non più di tre per pagina, a meno che non sia necessario. I documenti con molti livelli, ad esempio con una gerarchia di titoli profonda, diventano difficili da gestire e da navigare. In questi casi, è consigliabile distribuire il contenuto su più pagine, se possibile.

## Perché serve una struttura?

Per rispondere a questa domanda, diamo un'occhiata a [text-start.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/html-text-formatting/text-start.html), una buona ricetta per l'hummus. Il corpo di questo documento contiene attualmente più parti di contenuto. Non sono contrassegnate in alcun modo, ma sono separate da interruzioni di riga, premendo Invio per andare alla riga successiva.

Tuttavia, quando si [apre il documento nel browser](https://mdn.github.io/learning-area/html/introduction-to-html/html-text-formatting/text-start.html), il testo appare come un unico grande blocco.

![Una pagina web che mostra una parete di testo non formattato, perché nella pagina non sono presenti elementi che ne strutturino il contenuto.](screen_shot_2017-03-29_at_09.20.35.png)

Questo accade perché non ci sono elementi che diano struttura al contenuto, quindi il browser non sa cosa sia un titolo e cosa sia un paragrafo. Inoltre:

- Gli utenti che guardano una pagina web tendono a scorrerla rapidamente per trovare contenuti rilevanti, spesso leggendo inizialmente solo i titoli. (Di solito [si trascorre pochissimo tempo su una pagina web](https://www.nngroup.com/articles/how-long-do-users-stay-on-web-pages/).) Se non riescono a vedere nulla di utile entro pochi secondi, probabilmente si frustreranno e andranno altrove.
- I motori di ricerca che indicizzano la pagina considerano il contenuto dei titoli come parole chiave importanti per influenzare il posizionamento della pagina nei risultati di ricerca. Senza titoli, la pagina avrà prestazioni scarse in termini di {{Glossary("SEO", "SEO")}} (Search Engine Optimization).
- Le persone con gravi disabilità visive spesso non leggono le pagine web, ma le ascoltano. Questo avviene tramite software chiamato [screen reader](https://en.wikipedia.org/wiki/Screen_reader). Questo software offre modi per accedere rapidamente a specifici contenuti testuali. Tra le varie tecniche utilizzate, fornisce una struttura del documento leggendo i titoli, consentendo agli utenti di trovare rapidamente le informazioni di cui hanno bisogno. Se i titoli non sono disponibili, saranno costretti ad ascoltare l'intero documento letto ad alta voce.
- Per applicare stili al contenuto con {{Glossary("CSS", "CSS")}}, oppure per eseguire operazioni interessanti con {{Glossary("JavaScript", "JavaScript")}}, sono necessari elementi che racchiudano il contenuto pertinente, affinché CSS/JavaScript possa selezionarlo in modo efficace.

Pertanto, è necessario fornire al contenuto un markup strutturale.

## Dare struttura al contenuto

Passiamo direttamente a una piccola sfida di codice per fare pratica con titoli e paragrafi HTML:

1. Fare clic su **"Play"** nel blocco di codice seguente per modificare l'esempio nel Playground MDN.
2. Racchiudere il testo appropriato all'inizio del contenuto all'interno di un elemento `<h1>` per trasformarlo in un titolo principale.
3. Ci sono due coppie di parole che devono essere racchiuse in elementi `<h2>` per trasformarle in titoli di secondo livello.
4. Racchiudere le frasi rimanenti in elementi `<p>` per trasformarle in paragrafi. Un elemento `<p>` deve trovarsi sotto ciascun elemento `<h2>`.

In caso di errore, è possibile cancellare il lavoro usando il pulsante _Reset_ nel Playground MDN. In caso di difficoltà, è possibile visualizzare la soluzione sotto il blocco di codice.

```html live-sample___headings_paragraphs
Favorite body parts The brain Lovely shape and color. Also does thinkin' stuff.
The feet Knobbly and ugly, but useful for getting about.
```

{{ EmbedLiveSample('headings_paragraphs', "100%", 60) }}

<details>
<summary>Fare clic qui per visualizzare la soluzione</summary>

L'elemento HTML completato dovrebbe avere questo aspetto:

```html
<h1>Favorite body parts</h1>

<h2>The brain</h2>

<p>Lovely shape and color. Also does thinkin' stuff.</p>

<h2>The feet</h2>

<p>Knobbly and ugly, but useful for getting about.</p>
```

</details>

## Perché servono la semantica?

La semantica viene usata ovunque intorno a noi: l'esperienza precedente permette di capire quale sia la funzione di un oggetto di uso quotidiano; quando si vede qualcosa, si sa quale sarà la sua funzione. Ad esempio, ci si aspetta che un semaforo rosso significhi "stop" e che un semaforo verde significhi "via". Le cose possono complicarsi molto rapidamente se viene applicata la semantica sbagliata. (Esistono Paesi in cui il rosso significa "via"? Speriamo di no.)

In modo simile, occorre assicurarsi di usare gli elementi corretti, assegnando al contenuto il significato, la funzione o l'aspetto corretto. In questo contesto, anche l'elemento `{{htmlelement("Heading_Elements", "&lt;h1>")}}` è un elemento semantico, che attribuisce al testo che racchiude il ruolo, o significato, di "titolo di livello superiore nella pagina".

```html
<h1>This is a top level heading</h1>
```

Per impostazione predefinita, il browser gli assegnerà una grande dimensione del carattere per farlo apparire come un titolo, sebbene sia possibile applicare stili CSS per renderlo simile a qualsiasi elemento desiderato. Ancora più importante, il suo valore semantico verrà usato in vari modi, ad esempio dai motori di ricerca e dagli screen reader, come menzionato sopra.

D'altra parte, è possibile far apparire qualsiasi elemento _come_ un titolo di livello superiore. Considerare quanto segue:

```html
<span style="font-size: 32px; margin: 21px 0; display: block;">
  Is this a top level heading?
</span>
```

Questo è un elemento {{htmlelement("span")}}. Non ha semantica. Viene usato per racchiudere contenuto quando si vuole applicarvi CSS, oppure eseguire qualcosa con JavaScript, senza attribuirgli alcun significato aggiuntivo. Questo argomento verrà approfondito più avanti nel corso. È stato applicato del CSS per farlo apparire come un titolo di livello superiore, ma poiché non ha valore semantico, non otterrà nessuno dei vantaggi aggiuntivi descritti sopra. È buona norma usare l'elemento HTML pertinente per lo scopo previsto.

## Riepilogo

Si conclude qui lo studio dei titoli e dei paragrafi HTML. Successivamente verranno esaminati altri aspetti dell'HTML semantico: dare enfasi alle parole.

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Webpage_metadata", "Learn_web_development/Core/Structuring_content/Emphasis_and_importance", "Learn_web_development/Core/Structuring_content")}}
