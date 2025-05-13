---
title: "HTML: Creazione del contenuto"
short-title: Creazione del contenuto
slug: Learn_web_development/Getting_started/Your_first_website/Creating_the_content
l10n:
  sourceCommit: 6a5c619dfad295ca9a9d317a4088908cfd33e686
---

{{PreviousMenuNext("Learn_web_development/Getting_started/Your_first_website/What_will_your_website_look_like", "Learn_web_development/Getting_started/Your_first_website/Styling_the_content", "Learn_web_development/Getting_started/Your_first_website")}}

HTML (**H**yper**T**ext **M**arkup **L**anguage) è il codice utilizzato per strutturare una pagina web e il suo contenuto. Questo articolo fornisce una comprensione di base di HTML e delle sue funzionalità, e mostra come creare il contenuto di base per il suo primo sito web.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con il sistema operativo del proprio computer, il software di base che si utilizza per costruire un sito web e i file system.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi formativi:</th>
      <td>
        <ul>
          <li>Lo scopo e la funzione di HTML.</li>
          <li>Le parti fondamentali della sintassi HTML — tag di apertura e chiusura, elementi, attributi, head, body.</li>
          <li>Elementi HTML comuni, tra cui paragrafi, intestazioni, immagini, elenchi e collegamenti.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Cos'è HTML?

HTML è un _linguaggio di markup_ costituito da una serie di **{{Glossary("element", "elementi")}}** utilizzati per avvolgere (o includere) il contenuto di testo per definire la sua struttura e farlo comportare in un certo modo.

Consideriamo un esempio — il seguente contenuto verrà mostrato tutto sulla stessa linea quando visualizzato su una pagina web, in quanto non è strutturato in alcun modo:

```plain
Instructions for life:
Eat
Sleep
Repeat
```

Se avvolgiamo questo contenuto con i seguenti elementi HTML, possiamo trasformare quella singola linea in un paragrafo ({{htmlelement("p")}}) e tre punti d'elenco ({{htmlelement("li")}}):

```html live-sample___basic-html
<p>Instructions for life:</p>

<ul>
  <li>Eat</li>
  <li>Sleep</li>
  <li>Repeat</li>
</ul>
```

Questo HTML verrà reso come segue in un browser web:

{{EmbedLiveSample("basic-html", "100%", "140px")}}

Oltre a strutturare il testo, HTML ha molti altri usi — rendere testo o immagini linkabili ad altre pagine web, incorporare immagini o video, creare tabelle di dati, e così via.

## Creare il suo primo documento HTML

Vediamo come elementi singoli sono combinati per formare una pagina HTML. In questa sezione, lei creerà un file HTML di base e darà un'occhiata a ciò di cui è composto.

1. All'interno della sua cartella `web-projects`, crei un'altra nuova cartella chiamata `first-website`.
2. All'interno di `first-website`, crei un nuovo file chiamato `index.html`, e inserisca il seguente codice nel file esattamente come mostrato:

```html
<!doctype html>
<html lang="en-US">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width" />
    <title>My test page</title>
  </head>
  <body>
    <img src="" alt="My test image" />
  </body>
</html>
```

Qui abbiamo quanto segue:

- `<!doctype html>`: Il {{Glossary("Doctype", "doctype")}} è un preambolo richiesto. Nella nebbia del tempo, quando HTML era giovane (intorno al 1991/92), i doctype erano pensati per fungere da collegamenti a un insieme di regole che la pagina HTML doveva seguire per essere considerata buon HTML, il che poteva significare un controllo automatico degli errori e altre cose utili. Tuttavia, oggi, non fanno molto e sono sostanzialmente necessari solo per assicurarsi che il documento si comporti correttamente. Questo è tutto ciò che deve sapere per ora.
- `<html></html>`: L'elemento {{htmlelement("html")}} avvolge tutto il contenuto dell'intera pagina ed è talvolta noto come **elemento radice**. Include anche l'attributo `lang`, che imposta la lingua principale del documento.
- `<head></head>`: L'elemento {{htmlelement("head")}} funge da contenitore per tutto ciò che si desidera includere nella pagina HTML che _non_ è il contenuto che si sta mostrando ai visitatori della pagina. Questo include cose come {{Glossary("keyword", "parole chiave")}} e una descrizione della pagina che si desidera appaia nei risultati di ricerca, {{Glossary("CSS", "CSS")}} per stilizzare il contenuto, dichiarazioni del set di caratteri, e altro.
- `<meta charset="utf-8">`: Questo elemento imposta il set di caratteri che il documento dovrebbe utilizzare a {{Glossary("UTF-8", "UTF-8")}}, che include la maggior parte dei caratteri della stragrande maggioranza delle lingue scritte. Essenzialmente, ora può gestire qualsiasi contenuto testuale che si potrebbe inserirvi. Non c'è motivo per non impostarlo, e può aiutare a evitare alcuni problemi in futuro.
- `<meta name="viewport" content="width=device-width">`: Questo [elemento viewport](/it/docs/Web/CSS/CSSOM_view/Viewport_concepts#mobile_viewports) assicura che la pagina venga resa alla larghezza del tavolozza del browser, evitando che i browser mobili rendano le pagine più larghe del tavolozza e poi le riducano.
- `<title></title>`: L'elemento {{htmlelement("title")}} imposta il titolo della sua pagina, che è il titolo che appare nella scheda del browser in cui la pagina è caricata. Viene utilizzato anche per descrivere la pagina quando la segna come segnalibro/preferita.
- `<body></body>`: L'elemento {{htmlelement("body")}} contiene _tutto_ il contenuto che si desidera mostrare agli utenti web quando visitano la sua pagina, che sia testo, immagini, video, giochi, tracce audio riproducibili, o qualsiasi altro. Al momento contiene solo un singolo elemento `<img>`, ma aggiungeremo più contenuto più avanti.

> [!NOTE]
> La maggior parte degli elementi HTML consiste in un **tag di apertura** (per esempio, `<body>`), seguito dal contenuto dell'elemento, seguito da un **tag di chiusura** (per esempio, `</body>`). Alcuni elementi HTML hanno anche **attributi**, che contengono impostazioni extra o informazioni sull'elemento — si vedano per esempio `charset`, `name`, e `src` nel nostro esempio di codice.

## Incorporare immagini

Concentriamoci sull'elemento {{htmlelement("img")}}:

```html
<img src="" alt="My test image" />
```

Questo incorpora un'immagine nella nostra pagina nella posizione in cui appare. Lo fa tramite l'attributo `src` (sorgente), che contiene il percorso del file immagine che vogliamo incorporare.

Abbiamo anche incluso un attributo `alt` (alternativo). Nell'[`attributo alt`](/it/docs/Web/HTML/Reference/Elements/img#authoring_meaningful_alternate_descriptions), si specifica il testo descrittivo per gli utenti che non possono vedere l'immagine, possibilmente a causa dei seguenti motivi:

1. Sono non vedenti. Gli utenti con gravi deficit visivi spesso utilizzano strumenti chiamati screen reader per leggere loro il testo alternativo.
2. Qualcosa è andato storto, causando la mancata visualizzazione dell'immagine. Se l'attributo `src` non contiene un percorso valido per un'immagine, al suo posto verrà visualizzato il testo alternativo:

   ![Le parole: la mia immagine di prova](alt-text-example.png)

Il testo alternativo che si scrive dovrebbe fornire al lettore informazioni sufficienti per avere una buona idea di cosa l'immagine trasmette. In questo esempio, il nostro attuale testo "La mia immagine di prova" non è buono perché non trasmette informazioni descrittive sull'immagine. Una alternativa molto migliore per il nostro logo di Firefox sarebbe "Il logo di Firefox: una volpe infuocata che circonda la Terra."

> [!NOTE]
> Elementi come `<img>` non hanno contenuto o un tag di chiusura, quindi sono chiamati elementi **vuoti** (o **{{Glossary("void_element", "void")}}**). Talvolta sono scritti con una **barra finale** alla fine del loro singolo tag (`<img />`), ma questo è opzionale.

Facciamo ora in modo che la sua immagine venga visualizzata correttamente.

1. All'interno della cartella `first-website`, crei una nuova cartella chiamata `images`, e metta l'immagine scelta nell'esempio precedente all'interno di questa cartella.
2. All'interno del valore dell'attributo `src` del tag `<img>`, inserisca il percorso dell'immagine. È all'interno di una cartella chiamata `images`, che si trova nella stessa directory del suo file `index.html`, pertanto il percorso sarà `images/` più il nome del file immagine. Ad esempio, se l'immagine si chiama `firefox-icon.png`, il suo attributo `src` sarà come questo: `src="images/firefox-icon.png"`.
3. Sostituisca il valore dell'attributo `alt` — `La mia immagine di prova` — con un testo che descriva meglio la sua immagine.
4. Apra il suo file `index.html` all'interno di un browser web. Dovrebbe vedere la sua immagine visualizzata. In caso contrario, controlli il suo elemento `<img>` rispetto al nostro codice; assicuri che non le manchi alcuna parte della sintassi, come per esempio i segni di citazione. Assicuri inoltre che il nome del file immagine sia corretto.

Se l'immagine è veramente grande e quindi non si adatta allo schermo, non si preoccupi. Risolveremo questo problema nel prossimo articolo.

> [!NOTE]
> Scoprire di più sull'utilizzo di un attributo `alt` per le immagini in varie situazioni nel nostro [tutorial sui multimedia accessibili](/it/docs/Learn_web_development/Core/Accessibility/Multimedia) e [Un albero decisionale per l'alt](https://www.w3.org/WAI/tutorials/images/decision-tree/).

## Marcatura del testo

Questa sezione coprirà alcuni elementi HTML essenziali che lei utilizzerà per la marcatura del testo.

> [!NOTE]
> La [basics of semantic HTML](https://scrimba.com/the-frontend-developer-career-path-c0j/~0xid?via=mdn) <sup>[_MDN learning partner_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> di Scrimba è una lezione interattiva che fornisce una descrizione utile dell'HTML, con particolare enfasi su perché l'aspetto _semantico_ sia importante.

### Intestazioni

Gli elementi di intestazione consentono di specificare che alcune parti del contenuto sono intestazioni — o sottointestazioni. Nello stesso modo in cui un libro ha il titolo principale, i titoli dei capitoli e i sottotitoli, un documento HTML può fare lo stesso. HTML contiene 6 livelli di intestazione, {{htmlelement("Heading_Elements", "&lt;h1&gt;–&lt;h6&gt;")}}, anche se comunemente ne utilizzerà solo 3 o 4 al massimo:

```html
<!-- 4 heading levels: -->
<h1>My main title</h1>
<h2>My top level heading</h2>
<h3>My subheading</h3>
<h4>My sub-subheading</h4>
```

> [!NOTE]
> Qualsiasi cosa in HTML tra `<!--` e `-->` è un **commento HTML**. Il browser ignora i commenti nel momento in cui rende il codice. In altre parole, non sono visibili sulla pagina — solo nel codice. I commenti HTML sono un modo per aggiungere note sul suo codice o sulla logica, che potrebbero essere utili ad altri che lavorano sullo stesso codice, o a lei, se tornasse indietro a guardarlo dopo 6 mesi e non ricordasse cosa ha fatto.

Ora provi ad aggiungere un titolo principale appropriato alla sua pagina HTML appena sopra l’elemento {{htmlelement("img")}}. Salvi il file e lo visualizzi in un browser per vedere l’effetto.

### Paragrafi

Gli elementi di paragrafo {{htmlelement("p")}} servono per contenere paragrafi di testo; li utilizzerà frequentemente quando si occupa di contrassegnare contenuto testuale regolare:

```html
<p>This is a single paragraph</p>
```

Aggiunga al suo testo di esempio dell'articolo precedente in uno o più paragrafi, posizionati direttamente sotto l'elemento {{htmlelement("img")}}. Lo salvi e osservi la sua pagina in un browser.

### Elenchi

Gran parte del contenuto del web è costituito da elenchi e HTML ha elementi speciali per questi. La marcatura degli elenchi consiste sempre in almeno 2 elementi. I tipi di elenchi più comuni sono elenchi ordinati e non ordinati:

1. **Elenchi non ordinati** sono per elenchi in cui l'ordine degli elementi non importa, come una lista della spesa. Questi sono avvolti in un elemento {{htmlelement("ul")}}.
2. **Elenchi ordinati** sono per elenchi in cui l'ordine degli elementi è importante, come una serie di istruzioni di cottura in una ricetta. Questi sono avvolti in un elemento {{htmlelement("ol")}}.

Ogni elemento all'interno degli elenchi è messo all'interno di un elemento {{htmlelement("li")}} (elemento di elenco).

Per esempio, se volessimo trasformare parte del seguente frammento di paragrafo in un elenco:

```html
<p>
  At Mozilla, we're a global community of technologists, thinkers, and builders
  working together…
</p>
```

Potremmo modificare il markup in questo modo:

```html
<p>At Mozilla, we're a global community of</p>

<ul>
  <li>technologists</li>
  <li>thinkers</li>
  <li>builders</li>
</ul>

<p>working together…</p>
```

Provi ad aggiungere un elenco ordinato o non ordinato alla sua pagina di esempio e visualizzi il risultato in un browser.

## Creare collegamenti

I collegamenti sono molto importanti — sono ciò che forma la rete! Per aggiungere un collegamento, dobbiamo utilizzare un elemento {{htmlelement("a")}}, "a" essendo l'abbreviazione di "ancora". Per trasformare il testo all'interno del suo paragrafo in un collegamento, segua questi passaggi:

1. Scelga del testo. Abbiamo scelto il testo "Mozilla Manifesto".
2. Avvolga il testo in un elemento {{htmlelement("a")}}, come mostrato di seguito:

   ```html
   <a>Mozilla Manifesto</a>
   ```

3. Dia all'elemento {{htmlelement("a")}} un attributo `href`, come mostrato di seguito:

   ```html
   <a href="">Mozilla Manifesto</a>
   ```

4. Riempia il valore di questo attributo con l'indirizzo web a cui si vuole che il collegamento punti:

   ```html
   <a href="https://www.mozilla.org/en-US/about/manifesto/">
     Mozilla Manifesto
   </a>
   ```

Potrebbe ottenere risultati inaspettati se omette la parte `https://` o `http://`, chiamata _protocollo_, all'inizio dell'indirizzo web. Dopo aver creato un collegamento, lo clicchi per assicurarsi che la mandi dove voleva.

> **Nota:** `href` potrebbe sembrare una scelta piuttosto oscura per un nome di attributo all'inizio. Sta per _**h**ypertext **ref**erence_.

Aggiunga un collegamento alla sua pagina ora, se non l'ha già fatto.

## Conclusione

Se ha seguito tutte le istruzioni in questo articolo, dovrebbe finire con una pagina simile a quella mostrata di seguito (può anche [visualizzarla qui](https://mdn.github.io/beginner-html-site/)):

![Uno screenshot di una pagina web che mostra un logo di Firefox, un'intestazione che dice Mozilla è cool, e due paragrafi di testo riempitivo](finished-test-page-small.png)

Se dovesse incontrare difficoltà, può sempre confrontare il suo lavoro con il nostro [esempio di codice completo](https://github.com/mdn/beginner-html-site/blob/main/index.html) su GitHub.

Qui, abbiamo solo toccato la superficie di HTML. Imparerà molto di più nel nostro modulo [Strutturare il contenuto con HTML](/it/docs/Learn_web_development/Core/Structuring_content) più avanti nel corso.

## Vedere anche

- [Imparare HTML e CSS](https://scrimba.com/learn-html-and-css-c0p?via=mdn), Scrimba <sup>[_MDN learning partner_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>
  - : Il corso _Learn HTML and CSS_ di [Scrimba](https://scrimba.com?via=mdn) insegna HTML e CSS attraverso la costruzione e la distribuzione di cinque progetti fantastici, con lezioni e sfide interattive e divertenti insegnate da insegnanti esperti.

{{PreviousMenuNext("Learn_web_development/Getting_started/Your_first_website/What_will_your_website_look_like", "Learn_web_development/Getting_started/Your_first_website/Styling_the_content", "Learn_web_development/Getting_started/Your_first_website")}}
