---
title: "HTML: Creare il contenuto"
short-title: Creare il contenuto
slug: Learn_web_development/Getting_started/Your_first_website/Creating_the_content
l10n:
  sourceCommit: 6a5c619dfad295ca9a9d317a4088908cfd33e686
---

{{PreviousMenuNext("Learn_web_development/Getting_started/Your_first_website/What_will_your_website_look_like", "Learn_web_development/Getting_started/Your_first_website/Styling_the_content", "Learn_web_development/Getting_started/Your_first_website")}}

HTML (**H**yper**T**ext **M**arkup **L**anguage) è il codice utilizzato per strutturare una pagina web e il suo contenuto. Questo articolo fornisce una comprensione di base dell'HTML e della sua funzionalità, e mostra come creare il contenuto di base per il tuo primo sito web.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con il sistema operativo del computer, il software di base che si utilizzerà per creare un sito web e i sistemi di file.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>Lo scopo e la funzione dell'HTML.</li>
          <li>Le parti fondamentali della sintassi HTML — tag di apertura e chiusura, elementi, attributi, head, body.</li>
          <li>Elementi HTML comuni, inclusi paragrafi, intestazioni, immagini, liste e link.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Quindi, cos'è l'HTML?

HTML è un _linguaggio di markup_ che consiste in una serie di **{{Glossary("element", "elementi")}}** utilizzati per racchiudere il contenuto testuale per definire la sua struttura e farlo comportare in un certo modo.

Vediamo un esempio — i seguenti contenuti saranno tutti mostrati sulla stessa riga quando visualizzati su una pagina web, poiché non sono strutturati in alcun modo:

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

Questo HTML verrà reso nel modo seguente in un browser web:

{{EmbedLiveSample("basic-html", "100%", "140px")}}

Oltre a strutturare il testo, HTML ha molti altri usi — fare in modo che testo o immagini linkino ad altre pagine web, incorporare immagini o video, creare tabelle di dati, e così via.

## Creare il tuo primo documento HTML

Vediamo come gli elementi individuali si combinano per formare una pagina HTML. In questa sezione, creerai un file HTML di base e vedrai di cosa è composto.

1. All'interno della tua cartella `web-projects`, crea un'altra nuova cartella chiamata `first-website`.
2. All'interno di `first-website`, crea un nuovo file chiamato `index.html`, e inserisci il seguente codice nel file esattamente come mostrato:

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

- `<!doctype html>`: Il {{Glossary("Doctype", "doctype")}} è un preambolo richiesto. Nei tempi antichi, quando l'HTML era giovane (intorno al 1991/92), i doctype erano destinati ad agire come collegamenti a un insieme di regole che la pagina HTML doveva seguire per essere considerata buon HTML, il che poteva significare controllo degli errori automatico e altre cose utili. Tuttavia, al giorno d'oggi, non fanno molto e sono fondamentalmente necessari solo per assicurarsi che il documento si comporti correttamente. Questo è tutto quello che devi sapere per ora.
- `<html></html>`: L'elemento {{htmlelement("html")}} racchiude tutto il contenuto dell'intera pagina ed è a volte noto come **elemento radice**. Include anche l'attributo `lang` {{Glossary("Attribute", "attribute")}}, che imposta la lingua principale del documento.
- `<head></head>`: L'elemento {{htmlelement("head")}} funge da contenitore per tutto quello che vuoi includere nella pagina HTML che _non è_ il contenuto che mostri ai visitatori della tua pagina. Questo include cose come {{Glossary("keyword", "parole chiave")}} e una descrizione della pagina che vuoi far apparire nei risultati di ricerca, {{Glossary("CSS", "CSS")}} per stilizzare il contenuto, dichiarazioni di set di caratteri, e altro.
- `<meta charset="utf-8">`: Questo elemento imposta il set di caratteri che il documento dovrebbe utilizzare su {{Glossary("UTF-8", "UTF-8")}}, che include la maggior parte dei caratteri della stragrande maggioranza delle lingue scritte. Essenzialmente, ora può gestire qualsiasi contenuto testuale tu possa inserirvi. Non c'è motivo per non impostarlo, e può aiutare a evitare alcuni problemi in seguito.
- `<meta name="viewport" content="width=device-width">`: Questo [elemento viewport](/it/docs/Web/CSS/CSSOM_view/Viewport_concepts#mobile_viewports) assicura che la pagina si renderizzi alla larghezza del viewport del browser, evitando che i browser mobili rendano le pagine più ampie del viewport e poi le ridimensionino.
- `<title></title>`: L'elemento {{htmlelement("title")}} imposta il titolo della tua pagina, che è il titolo che appare nella scheda del browser in cui la pagina è caricata. Viene anche usato per descrivere la pagina quando la si aggiunge ai segnalibri/preferiti.
- `<body></body>`: L'elemento {{htmlelement("body")}} contiene _tutto_ il contenuto che vuoi mostrare agli utenti web quando visitano la tua pagina, che sia testo, immagini, video, giochi, brani audio riproducibili, o qualsiasi altra cosa. Al momento contiene solo un singolo elemento `<img>`, ma aggiungeremo più contenuti più avanti.

> [!NOTE]
> La maggior parte degli elementi HTML consiste di un **tag di apertura** (ad esempio, `<body>`), seguito dal contenuto dell'elemento, seguito da un **tag di chiusura** (ad esempio, `</body>`). Alcuni elementi HTML hanno anche **attributi**, che contengono impostazioni o informazioni extra sull'elemento — vedi ad esempio `charset`, `name`, e `src` nel nostro esempio di codice.

## Incorporare immagini

Focalizziamoci sull'elemento {{htmlelement("img")}}:

```html
<img src="" alt="My test image" />
```

Questo incorpora un'immagine nella nostra pagina nella posizione in cui appare. Lo fa tramite l'attributo `src` (source), che contiene il percorso al file immagine che vogliamo incorporare.

Abbiamo anche incluso un attributo `alt` (alternativo). Nell'attributo [`alt`](/it/docs/Web/HTML/Reference/Elements/img#authoring_meaningful_alternate_descriptions), specifichi del testo descrittivo per gli utenti che non possono vedere l'immagine, possibilmente per i seguenti motivi:

1. Sono ipovedenti. Gli utenti con significativi problemi alla vista spesso usano strumenti chiamati screen reader per leggere loro il testo alternativo.
2. Qualcosa è andato storto, causando il mancato caricamento dell'immagine. Se l'attributo `src` non contiene un percorso valido a un'immagine, verrà mostrato il testo alternativo:

   ![Le parole: la mia immagine di prova](alt-text-example.png)

Il testo alternativo che scrivi dovrebbe fornire al lettore abbastanza informazioni per avere una buona idea di cosa l'immagine trasmette. In questo esempio, il nostro attuale testo "La mia immagine di prova" non è buono perché non trasmette informazioni descrittive sull'immagine. Un'alternativa molto migliore per il nostro logo di Firefox sarebbe "Il logo di Firefox: una volpe fiammeggiante che avvolge la Terra."

> [!NOTE]
> Elementi come `<img>` non hanno contenuto o tag di chiusura, e sono quindi chiamati elementi **vuoti** (o **{{Glossary("void_element", "void")}}**). A volte sono scritti con uno **slash finale** alla fine del loro singolo tag (`<img />`), ma è opzionale.

Passiamo a far vedere la tua immagine ora.

1. All'interno della cartella `first-website`, crea una nuova cartella chiamata `images` e metti l'immagine che hai scelto nell'esempio precedente all'interno di questa cartella.
2. Nel valore dell'attributo `src` dell'elemento `<img>`, inserisci il percorso alla tua immagine. Si trova all'interno di una cartella chiamata `images`, che è all'interno della stessa directory del tuo file `index.html`, quindi il percorso sarà `images/` più il nome della tua immagine. Per esempio, se la tua immagine si chiama `firefox-icon.png`, il tuo attributo `src` sarà simile a questo: `src="images/firefox-icon.png"`.
3. Sostituisci il valore dell'attributo `alt` — `La mia immagine di prova` — con del testo che descriva meglio la tua immagine.
4. Apri il tuo file `index.html` in un browser web. Dovresti vedere la tua immagine visualizzata. Se no, confronta il tuo elemento `<img>` con il nostro codice; assicurati che non manchi nessuna parte della sintassi, come i doppi apici. Assicurati che il nome del file dell'immagine sia corretto.

Se l'immagine è davvero grande e quindi non si adatta allo schermo, non preoccuparti. Risolveremo questo problema nel prossimo articolo.

> [!NOTE]
> Puoi scoprire di più sull'uso di un attributo `alt` per le immagini in varie situazioni nel nostro [tutorial su multimedialità accessibile](/it/docs/Learn_web_development/Core/Accessibility/Multimedia) e [Un albero decisionale per alt](https://www.w3.org/WAI/tutorials/images/decision-tree/).

## Contrassegnare il testo

Questa sezione coprirà alcuni degli elementi HTML essenziali che userai per contrassegnare il testo.

> [!NOTE]
> [Le basi dell'HTML semantico di Scrimba](https://scrimba.com/the-frontend-developer-career-path-c0j/~0xid?via=mdn) <sup>[_Partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> è una lezione interattiva che fornisce una descrizione utile dell'HTML, con particolare enfasi sul perché l'aspetto _semantico_ di esso sia importante.

### Intestazioni

Gli elementi di intestazione ti permettono di specificare che certe parti del tuo contenuto sono intestazioni — o sottointestazioni. Allo stesso modo in cui un libro ha il titolo principale, i titoli dei capitoli e i sottotitoli, anche un documento HTML può farlo. HTML contiene 6 livelli di intestazione, {{htmlelement("Heading_Elements", "&lt;h1&gt;–&lt;h6&gt;")}}, sebbene comunemente ne utilizzerai solo da 3 a 4 al massimo:

```html
<!-- 4 heading levels: -->
<h1>My main title</h1>
<h2>My top level heading</h2>
<h3>My subheading</h3>
<h4>My sub-subheading</h4>
```

> [!NOTE]
> Tutto ciò che si trova in HTML tra `<!--` e `-->` è un **commento HTML**. Il browser ignora i commenti mentre rende il codice. In altre parole, non sono visibili sulla pagina — solo nel codice. I commenti HTML sono un modo per te di aggiungere note sul tuo codice o logica, che potrebbero essere utili ad altri che lavorano sullo stesso codice, o a te, se ci torni dopo 6 mesi e non riesci a ricordare cosa hai fatto.

Ora prova ad aggiungere un titolo principale appropriato alla tua pagina HTML appena sopra il tuo elemento {{htmlelement("img")}}. Salva il file e visualizzalo in un browser per vedere l'effetto.

### Paragrafi

Gli elementi di paragrafo {{htmlelement("p")}} sono per contenere paragrafi di testo; li userai frequentemente quando contrassegnerai contenuti testuali regolari:

```html
<p>This is a single paragraph</p>
```

Aggiungi il tuo testo di esempio dall'articolo precedente in uno o più paragrafi, collocati direttamente sotto il tuo elemento {{htmlelement("img")}}. Salvalo e guarda la tua pagina in un browser.

### Liste

Gran parte del contenuto del web è costituito da liste e HTML ha elementi speciali per queste. Il contrassegnare le liste consiste sempre di almeno 2 elementi. I tipi di lista più comuni sono liste ordinate e non ordinate:

1. **Liste non ordinate** sono per liste in cui l'ordine degli elementi non è importante, come una lista della spesa. Queste sono racchiuse in un elemento {{htmlelement("ul")}}.
2. **Liste ordinate** sono per liste in cui l'ordine degli elementi è importante, come un elenco di istruzioni di cottura in una ricetta. Queste sono racchiuse in un elemento {{htmlelement("ol")}}.

Ogni elemento all'interno delle liste è inserito in un elemento {{htmlelement("li")}} (elemento lista).

Per esempio, se volessimo trasformare parte del seguente frammento di paragrafo in una lista:

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

Prova ad aggiungere una lista ordinata o non ordinata alla tua pagina di esempio e visualizza il risultato in un browser.

## Creare link

I link sono molto importanti — sono ciò che rende il web una rete! Per aggiungere un link, abbiamo bisogno di usare un elemento {{htmlelement("a")}}, "a" sta per "anchor" (ancora). Per trasformare il testo all'interno del tuo paragrafo in un link, segui questi passaggi:

1. Scegli del testo. Abbiamo scelto il testo "Mozilla Manifesto".
2. Racchiudi il testo in un elemento {{htmlelement("a")}}, come mostrato di seguito:

   ```html
   <a>Mozilla Manifesto</a>
   ```

3. Dai all'elemento {{htmlelement("a")}} un attributo `href`, come mostrato di seguito:

   ```html
   <a href="">Mozilla Manifesto</a>
   ```

4. Compila il valore di questo attributo con l'indirizzo web a cui vuoi che punti il link:

   ```html
   <a href="https://www.mozilla.org/en-US/about/manifesto/">
     Mozilla Manifesto
   </a>
   ```

Potresti ottenere risultati imprevisti se ometti `https://` o `http://`, chiamati _protocollo_, all'inizio dell'indirizzo web. Dopo aver creato un link, fai clic su di esso per assicurarti che ti invii dove volevi.

> **Nota:** `href` potrebbe sembrare una scelta piuttosto oscura per un nome di attributo all'inizio. Sta per _**h**ypertext **ref**erence_.

Aggiungi un link alla tua pagina ora, se non lo hai già fatto.

## Conclusione

Se hai seguito tutte le istruzioni di questo articolo, dovresti finire con una pagina che appare come quella sotto (puoi anche [visualizzarla qui](https://mdn.github.io/beginner-html-site/)):

![Una schermata di pagina web che mostra un logo di Firefox, un'intestazione che dice Mozilla è fantastico, e due paragrafi di testo fittizio](finished-test-page-small.png)

Se ti blocchi, puoi sempre confrontare il tuo lavoro con il nostro [esempio di codice finale](https://github.com/mdn/beginner-html-site/blob/main/index.html) su GitHub.

Qui, abbiamo solo veramente scalfito la superficie dell'HTML. Imparerai molto di più nel nostro modulo Core [Strutturare contenuti con HTML](/it/docs/Learn_web_development/Core/Structuring_content) più avanti nel corso.

## Vedi anche

- [Impara HTML e CSS](https://scrimba.com/learn-html-and-css-c0p?via=mdn), Scrimba <sup>[_Partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>
  - : Il corso _Impara HTML e CSS_ di [Scrimba](https://scrimba.com?via=mdn) ti insegna HTML e CSS attraverso la costruzione e il deployment di cinque progetti fantastici, con lezioni interattive e sfide divertenti impartite da insegnanti esperti.

{{PreviousMenuNext("Learn_web_development/Getting_started/Your_first_website/What_will_your_website_look_like", "Learn_web_development/Getting_started/Your_first_website/Styling_the_content", "Learn_web_development/Getting_started/Your_first_website")}}
