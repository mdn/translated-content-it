---
title: "HTML: Creare il contenuto"
short-title: Creare il contenuto
slug: Learn_web_development/Getting_started/Your_first_website/Creating_the_content
l10n:
  sourceCommit: 85fccefc8066bd49af4ddafc12c77f35265c7e2d
---

{{PreviousMenuNext("Learn_web_development/Getting_started/Your_first_website/What_will_your_website_look_like", "Learn_web_development/Getting_started/Your_first_website/Styling_the_content", "Learn_web_development/Getting_started/Your_first_website")}}

HTML (**H**yper**T**ext **M**arkup **L**anguage) è il codice usato per strutturare una pagina web e il relativo contenuto. Questo articolo fornisce una comprensione di base di HTML e delle sue funzionalità, e mostra come creare il contenuto di base per il primo sito web.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con il sistema operativo del computer, il software di base che verrà usato per creare un sito web e i file system.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Lo scopo e la funzione di HTML.</li>
          <li>Le parti di base della sintassi HTML — tag di apertura e chiusura, elementi, attributi, head, body.</li>
          <li>Elementi HTML comuni, inclusi paragrafi, titoli, immagini, elenchi e collegamenti.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Che cos'è HTML?

HTML è un _linguaggio di markup_ costituito da una serie di **{{Glossary("element", "elementi")}}** usati per racchiudere il contenuto testuale e definirne la struttura, facendolo comportare in un determinato modo.

Osserviamo un esempio: il contenuto seguente verrà mostrato tutto sulla stessa riga quando visualizzato in una pagina web, poiché non è strutturato in alcun modo:

```plain
Instructions for life:
Eat
Sleep
Repeat
```

Se racchiudiamo questo contenuto con i seguenti elementi HTML, possiamo trasformare quella singola riga in un paragrafo ({{htmlelement("p")}}) e tre punti elenco ({{htmlelement("li")}}):

```html live-sample___basic-html
<p>Instructions for life:</p>

<ul>
  <li>Eat</li>
  <li>Sleep</li>
  <li>Repeat</li>
</ul>
```

Questo HTML verrà visualizzato nel modo seguente in un browser web:

{{EmbedLiveSample("basic-html", "100%", "140px")}}

Oltre a strutturare il testo, HTML ha molti altri usi: rendere testo o immagini collegamenti ad altre pagine web, incorporare immagini o video, creare tabelle di dati e così via.

> [!NOTE]
> [HTML tags](https://scrimba.com/frontend-path-c0j/~0g?via=mdn) di Scrimba <sup>[_MDN learning partner_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> è una lezione interattiva che permette di esercitarsi con le basi di HTML, inclusi i titoli.

## Creare il primo documento HTML

Vediamo come i singoli elementi vengono combinati per formare una pagina HTML. In questa sezione verrà creato un file HTML di base e verranno analizzate le parti che lo compongono.

1. Nella cartella `web-projects`, creare un'altra cartella chiamata `first-website`.
2. All'interno di `first-website`, creare un nuovo file chiamato `index.html` e inserire nel file il codice seguente esattamente come mostrato:

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

Qui sono presenti i seguenti elementi:

- `<!doctype html>`: Il {{Glossary("Doctype", "doctype")}} è un preambolo obbligatorio. In tempi remoti, quando HTML era giovane (intorno al 1991/92), i doctype erano pensati per agire come collegamenti a un insieme di regole che la pagina HTML doveva seguire per essere considerata un buon HTML; questo poteva includere il controllo automatico degli errori e altre funzioni utili. Oggi, tuttavia, non fanno molto e sono necessari principalmente per assicurarsi che il documento si comporti correttamente. Per ora è tutto ciò che occorre sapere.
- `<html></html>`: L'elemento {{htmlelement("html")}} racchiude tutto il contenuto dell'intera pagina ed è talvolta noto come **elemento radice**. Include inoltre l'{{Glossary("Attribute", "attributo")}} `lang`, che imposta la lingua principale del documento.
- `<head></head>`: L'elemento {{htmlelement("head")}} agisce come contenitore per tutti gli elementi che si desidera includere nella pagina HTML ma che _non_ sono il contenuto mostrato ai visitatori della pagina. Include elementi come {{Glossary("keyword", "parole chiave")}} e una descrizione della pagina da mostrare nei risultati di ricerca, {{Glossary("CSS", "CSS")}} per applicare stili al contenuto, dichiarazioni del set di caratteri e altro ancora.
- `<meta charset="utf-8">`: Questo elemento imposta il set di caratteri che il documento deve usare su {{Glossary("UTF-8", "UTF-8")}}, che include la maggior parte dei caratteri della stragrande maggioranza delle lingue scritte. In sostanza, ora può gestire qualsiasi contenuto testuale che vi si potrebbe inserire. Non c'è motivo per non impostarlo e può aiutare a evitare alcuni problemi in seguito.
- `<meta name="viewport" content="width=device-width">`: Questo [elemento viewport](/it/docs/Web/CSS/Guides/CSSOM_view/Viewport_concepts#mobile_viewports) assicura che la pagina venga visualizzata alla larghezza del viewport del browser, impedendo ai browser mobili di visualizzare pagine più larghe del viewport per poi ridurle.
- `<title></title>`: L'elemento {{htmlelement("title")}} imposta il titolo della pagina, ovvero il titolo visualizzato nella scheda del browser in cui è caricata la pagina. Viene anche usato per descrivere la pagina quando viene aggiunta ai segnalibri o ai preferiti.
- `<body></body>`: L'elemento {{htmlelement("body")}} contiene _tutto_ il contenuto che si desidera mostrare agli utenti web quando visitano la pagina, che si tratti di testo, immagini, video, giochi, tracce audio riproducibili o qualsiasi altra cosa. Al momento contiene soltanto un singolo elemento `<img>`, ma verrà aggiunto altro contenuto in seguito.

> [!NOTE]
> La maggior parte degli elementi HTML consiste in un **tag di apertura** (per esempio, `<body>`), seguito dal contenuto dell'elemento e quindi da un **tag di chiusura** (per esempio, `</body>`). Alcuni elementi HTML hanno anche degli **attributi**, che contengono impostazioni o informazioni aggiuntive sull'elemento — vedere, per esempio, `charset`, `name` e `src` nell'esempio di codice.

## Incorporare immagini

Passiamo ora all'elemento {{htmlelement("img")}}:

```html
<img src="" alt="My test image" />
```

Questo incorpora un'immagine nella pagina nella posizione in cui appare. Lo fa tramite l'attributo `src` (source), che contiene il percorso del file immagine da incorporare.

È stato incluso anche un attributo `alt` (alternative). Nell'[attributo `alt`](/it/docs/Web/HTML/Reference/Elements/img#authoring_meaningful_alternate_descriptions), si specifica un testo descrittivo per gli utenti che non possono vedere l'immagine, possibilmente per uno dei seguenti motivi:

1. Hanno una disabilità visiva. Gli utenti con disabilità visive significative usano spesso strumenti chiamati screen reader, che leggono loro ad alta voce il testo alternativo.
2. Qualcosa è andato storto e l'immagine non viene visualizzata. Se l'attributo `src` non contiene un percorso valido a un'immagine, verrà visualizzato al suo posto il testo alternativo:

   ![Le parole: my test image](alt-text-example.png)

Il testo alternativo scritto dovrebbe fornire al lettore informazioni sufficienti per avere una buona idea di ciò che l'immagine comunica. In questo esempio, il testo attuale, "My test image", non è adeguato perché non trasmette informazioni descrittive sull'immagine. Un'alternativa molto migliore per il logo di Firefox sarebbe "The Firefox logo: a flaming fox surrounding the Earth."

> [!NOTE]
> Elementi come `<img>` non hanno contenuto né tag di chiusura e sono quindi chiamati elementi **vuoti** (o **{{Glossary("void_element", "void")}}**). Talvolta vengono scritti con una **barra finale** alla fine del loro singolo tag (`<img />`), ma questa è facoltativa.

Ora visualizziamo l'immagine.

1. All'interno della cartella `first-website`, creare una nuova cartella chiamata `images` e inserire in questa cartella l'immagine scelta nell'esempio precedente.
2. Nel valore dell'attributo `src` del tag `<img>`, inserire il percorso dell'immagine. L'immagine si trova in una cartella chiamata `images`, che si trova nella stessa directory del file `index.html`; pertanto il percorso sarà `images/` più il nome dell'immagine. Per esempio, se l'immagine si chiama `firefox-icon.png`, l'attributo `src` sarà così: `src="images/firefox-icon.png"`.
3. Sostituire il valore dell'attributo `alt` — `My test image` — con un testo che descriva meglio l'immagine.
4. Aprire il file `index.html` in un browser web. L'immagine dovrebbe essere visualizzata. In caso contrario, confrontare l'elemento `<img>` con il nostro codice; assicurarsi che non manchi alcuna parte della sintassi, come le virgolette. Assicurarsi che il nome del file immagine sia corretto.

Se l'immagine è molto grande e quindi non entra nello schermo, non occorre preoccuparsi. Questo problema verrà risolto nel prossimo articolo.

> [!NOTE]
> Per ulteriori informazioni sull'uso di un attributo `alt` per le immagini in varie situazioni, consultare il nostro [tutorial sui contenuti multimediali accessibili](/it/docs/Learn_web_development/Core/Accessibility/Multimedia) e [An alt Decision Tree](https://www.w3.org/WAI/tutorials/images/decision-tree/).

## Eseguire il markup del testo

Questa sezione descrive alcuni elementi HTML essenziali da usare per eseguire il markup del testo.

> [!NOTE]
> [The basics of semantic HTML](https://scrimba.com/the-frontend-developer-career-path-c0j/~0xid?via=mdn) di Scrimba <sup>[_MDN learning partner_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> è una lezione interattiva che fornisce una descrizione utile di HTML, con particolare enfasi sul motivo per cui il suo aspetto _semantico_ è importante.

### Titoli

Gli elementi titolo consentono di specificare che determinate parti del contenuto sono titoli o sottotitoli. Come un libro ha il titolo principale, i titoli dei capitoli e i sottotitoli, anche un documento HTML può averli. HTML contiene 6 livelli di titolo, {{htmlelement("Heading_Elements", "&lt;h1&gt;–&lt;h6&gt;")}}, sebbene normalmente ne vengano usati al massimo solo da 3 a 4:

```html
<!-- 4 heading levels: -->
<h1>My main title</h1>
<h2>My top level heading</h2>
<h3>My subheading</h3>
<h4>My sub-subheading</h4>
```

> [!NOTE]
> Qualsiasi contenuto HTML compreso tra `<!--` e `-->` è un **commento HTML**. Il browser ignora i commenti durante il rendering del codice. In altre parole, non sono visibili sulla pagina, ma solo nel codice. I commenti HTML permettono di aggiungere note sul codice o sulla logica, che potrebbero essere utili ad altre persone che lavorano sullo stesso codice oppure quando vi si ritorna dopo 6 mesi e non si ricorda più cosa è stato fatto.

Aggiungere il titolo della pagina alla pagina HTML appena sopra l'elemento {{htmlelement("img")}}, racchiuso nei tag `<h1> ... </h1>`. Salvare il file e visualizzarlo in un browser per osservare l'effetto.

### Paragrafi

Gli elementi paragrafo {{htmlelement("p")}} servono a contenere paragrafi di testo; verranno usati spesso per eseguire il markup del normale contenuto testuale:

```html
<p>This is a single paragraph</p>
```

Aggiungere il testo di esempio dell'articolo precedente in uno o più paragrafi, posizionati direttamente sotto l'elemento {{htmlelement("img")}}. Salvare il file e visualizzare la pagina in un browser.

### Elenchi

Gran parte del contenuto del web è costituito da elenchi e HTML dispone di elementi speciali per essi. Il markup degli elenchi consiste sempre di almeno 2 elementi. I tipi di elenco più comuni sono gli elenchi ordinati e non ordinati:

1. Gli **elenchi non ordinati** sono destinati agli elenchi in cui l'ordine degli elementi non è importante, come una lista della spesa. Sono racchiusi in un elemento {{htmlelement("ul")}}.
2. Gli **elenchi ordinati** sono destinati agli elenchi in cui l'ordine degli elementi è importante, come un elenco di istruzioni di cucina in una ricetta. Sono racchiusi in un elemento {{htmlelement("ol")}}.

Ogni elemento all'interno degli elenchi viene inserito in un elemento {{htmlelement("li")}} (list item).

Per esempio, se si volesse trasformare parte del seguente frammento di paragrafo in un elenco:

```html
<p>
  At Mozilla, we're a global community of technologists, thinkers, and builders
  working together…
</p>
```

Il markup potrebbe essere modificato in questo modo:

```html
<p>At Mozilla, we're a global community of</p>

<ul>
  <li>technologists</li>
  <li>thinkers</li>
  <li>builders</li>
</ul>

<p>working together…</p>
```

Provare ad aggiungere un elenco ordinato o non ordinato alla pagina di esempio e visualizzare il risultato in un browser.

## Creare collegamenti

I collegamenti sono molto importanti: sono ciò che rende il web un web! Per aggiungere un collegamento, è necessario usare un elemento {{htmlelement("a")}}, dove "a" è l'abbreviazione di "anchor". Per trasformare in un collegamento del testo all'interno di un paragrafo, seguire questi passaggi:

1. Scegliere del testo. In questo caso è stato scelto il testo "Mozilla Manifesto".
2. Racchiudere il testo in un elemento {{htmlelement("a")}}, come mostrato di seguito:

   ```html
   <a>Mozilla Manifesto</a>
   ```

3. Assegnare all'elemento {{htmlelement("a")}} un attributo `href`, come mostrato di seguito:

   ```html
   <a href="">Mozilla Manifesto</a>
   ```

4. Inserire nel valore di questo attributo l'indirizzo web verso cui deve puntare il collegamento:

   ```html
   <a href="https://www.mozilla.org/en-US/about/manifesto/">
     Mozilla Manifesto
   </a>
   ```

Potrebbero verificarsi risultati imprevisti se si omette la parte `https://` o `http://`, chiamata _protocol_, all'inizio dell'indirizzo web. Dopo aver creato un collegamento, selezionarlo per verificare che porti alla destinazione desiderata.

> [!NOTE]
> `href` potrebbe inizialmente sembrare una scelta piuttosto oscura per il nome di un attributo. È l'abbreviazione di _**h**ypertext **ref**erence_.

Aggiungere ora un collegamento alla pagina, se non è già stato fatto.

## Conclusione

Seguendo tutte le istruzioni di questo articolo, si dovrebbe ottenere una pagina simile a quella mostrata di seguito (è anche possibile [visualizzarla qui](https://mdn.github.io/beginner-html-site/)):

![Screenshot di una pagina web che mostra un logo di Firefox, un titolo con scritto Mozilla is cool e due paragrafi di testo segnaposto](finished-test-page-small.png)

In caso di difficoltà, è sempre possibile confrontare il proprio lavoro con il nostro [codice dell'esempio completato](https://github.com/mdn/beginner-html-site/blob/main/index.html) su GitHub.

Qui è stata appena scalfita la superficie di HTML. Si imparerà molto di più nel modulo Core [Strutturare il contenuto con HTML](/it/docs/Learn_web_development/Core/Structuring_content), più avanti nel corso.

## Vedere anche

- [Imparare HTML e CSS](https://scrimba.com/learn-html-and-css-c0p?via=mdn), Scrimba <sup>[_MDN learning partner_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>
  - : Il corso _Learn HTML and CSS_ di [Scrimba](https://scrimba.com?via=mdn) insegna HTML e CSS attraverso la creazione e la distribuzione di cinque fantastici progetti, con lezioni interattive divertenti e sfide tenute da insegnanti esperti.

{{PreviousMenuNext("Learn_web_development/Getting_started/Your_first_website/What_will_your_website_look_like", "Learn_web_development/Getting_started/Your_first_website/Styling_the_content", "Learn_web_development/Getting_started/Your_first_website")}}
