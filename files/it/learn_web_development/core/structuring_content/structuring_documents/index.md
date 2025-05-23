---
title: Strutturare i documenti
slug: Learn_web_development/Core/Structuring_content/Structuring_documents
l10n:
  sourceCommit: 0915a5e602d475bd1a1a57d905f0bac1b7ed57b8
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Lists", "Learn_web_development/Core/Structuring_content/Advanced_text_features", "Learn_web_development/Core/Structuring_content")}}

Oltre a definire parti individuali della pagina (come "un paragrafo" o "un'immagine"), {{Glossary("HTML", "HTML")}} offre anche una serie di elementi a livello di blocco utilizzati per definire le aree del sito web (come "l'intestazione", "il menu di navigazione", "la colonna del contenuto principale"). Questo articolo esplora come pianificare una struttura di base di un sito web e scrivere l'HTML per rappresentare questa struttura.

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
          >liste</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Gli elementi strutturali semantici comuni di HTML, ad esempio <code>&lt;main&gt;</code>, <code>&lt;section&gt;</code>, <code>&lt;article&gt;</code>, <code>&lt;header&gt;</code>, <code>&lt;nav&gt;</code>, e <code>&lt;footer&gt;</code>, e come usarli correttamente.</li>
          <li>La necessità di utilizzare elementi semantici nei posti appropriati, piuttosto che usare solo elementi <code>&lt;div&gt;</code> ovunque sia richiesto un contenitore a livello di blocco, e i vantaggi di ciò (come una migliore accessibilità).</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Sezioni di base di un documento

Le pagine web possono apparire diverse l'una dall'altra, ma tendono tutte a condividere componenti standard simili, a meno che la pagina non mostri un video o un gioco a schermo intero, faccia parte di un progetto artistico o sia semplicemente strutturata male:

- intestazione:
  - : Di solito una grande striscia in cima con un grande titolo, un logo e forse uno slogan. Questo solitamente rimane lo stesso da una pagina all'altra di un sito web.
- barra di navigazione:
  - : Link alle sezioni principali del sito; solitamente rappresentati da pulsanti di menu, link o schede. Come l'intestazione, questo contenuto solitamente rimane costante da una pagina web all'altra — avere una navigazione incoerente sul tuo sito web porterà solo a utenti confusi e frustrati. Molti web designer considerano la barra di navigazione parte dell'intestazione piuttosto che un componente individuale, ma non è un requisito; infatti, alcuni sostengono che separare le due sia meglio per l'[accessibilità](/it/docs/Learn_web_development/Core/Accessibility), poiché i lettori di schermo possono leggere meglio le due funzionalità se sono separate.
- contenuto principale:
  - : Una vasta area al centro che contiene la maggior parte del contenuto unico di una determinata pagina web, ad esempio, il video che vuoi guardare o l'articolo principale che stai leggendo, o la mappa che vuoi visualizzare, o i titoli delle notizie, ecc. Questa è la parte del sito web che sicuramente varierà da una pagina all'altra!
- barra laterale:
  - : Alcune informazioni periferiche, link, citazioni, annunci, ecc. Solitamente, questo è contestuale a ciò che è contenuto nel contenuto principale (ad esempio, su una pagina di un articolo di notizie, la barra laterale potrebbe contenere la biografia dell'autore o link ad articoli correlati) ma ci sono anche casi in cui troverai degli elementi ricorrenti come un sistema di navigazione secondario.
- piè di pagina:
  - : Una striscia in fondo alla pagina che generalmente contiene notizie in piccola stampa, annotazioni sul copyright o informazioni di contatto. È un luogo dove mettere informazioni comuni (come l'intestazione) ma solitamente tali informazioni non sono critiche o secondarie rispetto al sito web stesso. Il piè di pagina è anche talvolta utilizzato per scopi di {{Glossary("SEO", "SEO")}}, fornendo link per un accesso rapido a contenuti popolari.

Un "sito web tipico" potrebbe essere strutturato in questo modo:

![un esempio di struttura semplice di un sito web che presenta un'intestazione principale, un menu di navigazione, contenuto principale, barra laterale e piè di pagina.](sample-website.png)

> [!NOTE]
> L'immagine sopra mostra le sezioni principali di un documento, che puoi definire con HTML. Tuttavia, l'_aspetto_ della pagina mostrata qui — incluso il layout, i colori e i caratteri — è ottenuto applicando [CSS](/it/docs/Learn_web_development/Core/Styling_basics) all'HTML.

## HTML per strutturare il contenuto

L'esempio mostrato sopra non è esteticamente bello, ma è perfettamente adeguato per illustrare un esempio tipico di layout di un sito web. Alcuni siti web hanno più colonne, altri sono molto più complessi, ma hai capito l'idea. Con il giusto CSS, potresti usare praticamente qualsiasi elemento per avvolgere le diverse sezioni e ottenere l'aspetto che desideri, ma come discusso in precedenza, dobbiamo rispettare la semantica e **usare l'elemento giusto per il lavoro giusto**.

Questo perché l'aspetto visivo non racconta tutta la storia. Usiamo il colore e la dimensione del carattere per attirare l'attenzione degli utenti vedenti sulle parti più utili del contenuto, come il menu di navigazione e i link correlati, ma che dire degli utenti ipovedenti, per esempio, che potrebbero non trovare concetti come "rosa" e "carattere grande" molto utili?

> **Nota:** [Circa l'8% degli uomini e lo 0,5% delle donne](https://www.color-blindness.com/) sono daltonici; o, per dirla in un altro modo, circa 1 uomo su 12 e 1 donna su 200. I non vedenti e le persone ipovedenti rappresentano circa il 4-5% della popolazione mondiale (nel 2015 c'erano [940 milioni di persone con qualche grado di perdita visiva](https://it.wikipedia.org/wiki/Disabilità_visiva), mentre la popolazione totale era di [circa 7,5 miliardi](https://it.wikipedia.org/wiki/Popolazione_mondiale)).

Nel tuo codice HTML, puoi contrassegnare sezioni di contenuto in base alla loro _funzionalità_ — puoi usare elementi che rappresentano le sezioni di contenuto sopra descritte in modo inequivocabile, e le tecnologie assistive come i lettori di schermo possono riconoscere quegli elementi e aiutare con compiti come "trovare la navigazione principale" o "trovare il contenuto principale". Come accennato in precedenza nel corso, ci sono una serie di [conseguenze dell'uso scorretto della struttura degli elementi e della semantica giusta per il lavoro giusto](/it/docs/Learn_web_development/Core/Structuring_content/Headings_and_paragraphs#why_do_we_need_structure).

Per implementare tale marcatura semantica, HTML fornisce tag dedicati che puoi utilizzare per rappresentare tali sezioni, ad esempio:

- **intestazione:** {{htmlelement("header")}}.
- **barra di navigazione:** {{htmlelement("nav")}}.
- **contenuto principale:** {{htmlelement("main")}}, con varie sottosezioni di contenuto rappresentate dagli elementi {{HTMLElement("article")}}, {{htmlelement("section")}}, e {{htmlelement("div")}}.
- **barra laterale:** {{htmlelement("aside")}}; spesso posizionata all'interno di {{htmlelement("main")}}.
- **piè di pagina:** {{htmlelement("footer")}}.

### Apprendimento attivo: esplorare il codice per il nostro esempio

Il nostro esempio visto sopra è rappresentato dal seguente codice (puoi anche [trovare l'esempio nel nostro repository GitHub](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/document_and_website_structure/index.html)). Ti invitiamo a guardare l'esempio sopra, e poi guardare l'elenco sottostante per vedere quali parti compongono quale sezione del visuale.

```html
<!doctype html>
<html lang="en-US">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width" />

    <title>My page title</title>
    <link
      href="https://fonts.googleapis.com/css?family=Open+Sans+Condensed:300|Sonsie+One"
      rel="stylesheet" />
    <link rel="stylesheet" href="style.css" />
  </head>

  <body>
    <!-- The main header used across all the pages of our website -->

    <header>
      <h1>Header</h1>
    </header>

    <nav>
      <ul>
        <li><a href="#">Home</a></li>
        <li><a href="#">Our team</a></li>
        <li><a href="#">Projects</a></li>
        <li><a href="#">Contact</a></li>
      </ul>

      <!-- A Search form: another common non-linear way to navigate through a site. -->

      <form>
        <input type="search" name="q" placeholder="Search query" />
        <input type="submit" value="Go!" />
      </form>
    </nav>

    <!-- Our page's main content -->
    <main>
      <!-- An article -->
      <article>
        <h2>Article heading</h2>

        <p>
          Lorem ipsum dolor sit amet, consectetur adipisicing elit. Donec a diam
          lectus. Set sit amet ipsum mauris. Maecenas congue ligula as quam
          viverra nec consectetur ant hendrerit. Donec et mollis dolor. Praesent
          et diam eget libero egestas mattis sit amet vitae augue. Nam tincidunt
          congue enim, ut porta lorem lacinia consectetur.
        </p>

        <section>
          <h3>Subsection</h3>

          <p>
            Donec ut librero sed accu vehicula ultricies a non tortor. Lorem
            ipsum dolor sit amet, consectetur adipisicing elit. Aenean ut
            gravida lorem. Ut turpis felis, pulvinar a semper sed, adipiscing id
            dolor.
          </p>

          <p>
            Pelientesque auctor nisi id magna consequat sagittis. Curabitur
            dapibus, enim sit amet elit pharetra tincidunt feugiat nist
            imperdiet. Ut convallis libero in urna ultrices accumsan. Donec sed
            odio eros.
          </p>
        </section>

        <section>
          <h3>Another subsection</h3>

          <p>
            Donec viverra mi quis quam pulvinar at malesuada arcu rhoncus. Cum
            soclis natoque penatibus et manis dis parturient montes, nascetur
            ridiculus mus. In rutrum accumsan ultricies. Mauris vitae nisi at
            sem facilisis semper ac in est.
          </p>

          <p>
            Vivamus fermentum semper porta. Nunc diam velit, adipscing ut
            tristique vitae sagittis vel odio. Maecenas convallis ullamcorper
            ultricied. Curabitur ornare, ligula semper consectetur sagittis,
            nisi diam iaculis velit, is fringille sem nunc vet mi.
          </p>
        </section>
      </article>

      <!-- the aside content can also be nested within the main content -->
      <aside>
        <h2>Related</h2>

        <ul>
          <li><a href="#">Oh I do like to be beside the seaside</a></li>
          <li><a href="#">Oh I do like to be beside the sea</a></li>
          <li><a href="#">Although in the North of England</a></li>
          <li><a href="#">It never stops raining</a></li>
          <li><a href="#">Oh well…</a></li>
        </ul>
      </aside>
    </main>

    <!-- The footer that is used across all the pages of our website -->

    <footer>
      <p>©Copyright 2050 by nobody. All rights reversed.</p>
    </footer>
  </body>
</html>
```

Prenditi del tempo per esaminare il codice e comprenderlo — i commenti all'interno del codice dovrebbero anche aiutarti a comprenderlo. Non ti chiediamo di fare molto altro in questo articolo, perché la chiave per comprendere il layout di un documento è scrivere una struttura HTML solida, e quindi strutturarlo con CSS. Aspetteremo fino a quando non inizierai a studiare il layout CSS come parte dell'argomento CSS.

## Elementi di layout HTML in dettaglio

È utile comprendere il significato complessivo di tutti gli elementi di sezione HTML in dettaglio — questo è qualcosa su cui lavorerai gradualmente man mano che inizi ad acquisire più esperienza con lo sviluppo web. Puoi trovare molti dettagli leggendo il nostro [riferimento sugli elementi HTML](/it/docs/Web/HTML/Reference/Elements). Per ora, queste sono le principali definizioni che dovresti cercare di comprendere:

- {{HTMLElement('main')}} è per contenuto _unico per questa pagina._ Usa `<main>` solo _una volta_ per pagina, e posizionarlo direttamente all'interno di {{HTMLElement('body')}}. Idealmente non dovrebbe essere nidificato entro altri elementi.
- {{HTMLElement('article')}} racchiude un blocco di contenuto correlato che ha senso da solo senza il resto della pagina (ad esempio, un singolo post di blog).
- {{HTMLElement('section')}} è simile a `<article>`, ma è più per raggruppare assieme una singola parte della pagina che costituisce un singolo pezzo di funzionalità (ad esempio, una mini mappa, o un set di titoli e riepiloghi di articoli), o un tema. È considerata una buona pratica iniziare ciascuna sezione con una [intestazione](/it/docs/Learn_web_development/Core/Structuring_content/Headings_and_paragraphs); nota anche che puoi suddividere gli `<article>` in differenti `<section>`, o le `<section>` in differenti `<article>`, a seconda del contesto.
- {{HTMLElement('aside')}} contiene contenuto che non è direttamente correlato al contenuto principale ma può fornire informazioni aggiuntive indirettamente correlate ad esso (voci di glossario, biografia dell'autore, link correlati, ecc.).
- {{HTMLElement('header')}} rappresenta un gruppo di contenuti introduttivi. Se è figlio di {{HTMLElement('body')}} definisce l'intestazione globale di una pagina web, ma se è figlio di un {{HTMLElement('article')}} o {{HTMLElement('section')}} definisce un'intestazione specifica per quella sezione (cerca di non confonderlo con [titoli e intestazioni](/it/docs/Learn_web_development/Core/Structuring_content/Webpage_metadata#adding_a_title)).
- {{HTMLElement('nav')}} contiene la principale funzionalità di navigazione per la pagina. Link secondari, ecc., non andrebbero nella navigazione.
- {{HTMLElement('footer')}} rappresenta un gruppo di contenuti finali per una pagina.

Ciascuno degli elementi sopra menzionati può essere cliccato per leggere l'articolo corrispondente nella sezione di riferimento "elemento HTML", fornendo maggiori dettagli su ciascuno.

### Wrapper non semantici

A volte ti imbatterai in una situazione in cui non riesci a trovare un elemento semantico ideale per raggruppare alcuni elementi insieme o avvolgere del contenuto. A volte potresti voler semplicemente raggruppare un insieme di elementi per influenzarli tutti come una singola entità con qualche {{Glossary("CSS", "CSS")}} o {{Glossary("JavaScript", "JavaScript")}}. Per casi come questi, HTML fornisce gli elementi {{HTMLElement("div")}} e {{HTMLElement("span")}}. Dovresti usarli preferibilmente con un attributo [`class`](/it/docs/Web/HTML/Reference/Global_attributes/class) adatto, per fornire loro un'etichetta che possa essere facilmente mirata.

{{HTMLElement("span")}} è un elemento inline non semantico, che dovresti utilizzare solo se non riesci a pensare a un elemento di testo semantico migliore per avvolgere il tuo contenuto, o se non vuoi aggiungere un significato specifico. Per esempio:

```html
<p>
  The King walked drunkenly back to his room at 01:00, the beer doing nothing to
  aid him as he staggered through the door.
  <span class="editor-note">
    [Editor's note: At this point in the play, the lights should be down low].
  </span>
</p>
```

In questo caso, la nota dell'editore dovrebbe semplicemente fornire una direzione extra per il regista del dramma; non è volta ad avere un significato semantico extra. Per gli utenti vedenti, CSS potrebbe essere utilizzato per distanziare leggermente la nota dal testo principale.

{{HTMLElement("div")}} è un elemento a livello di blocco non semantico, che dovresti utilizzare solo se non riesci a pensare a un migliore elemento di blocco semantico da utilizzare, o non vuoi aggiungere un significato specifico. Per esempio, immagina un widget del carrello della spesa che potresti scegliere di aprire in qualsiasi momento durante il tuo tempo su un sito di e-commerce:

```html-nolint
<div class="shopping-cart">
  <h2>Shopping cart</h2>
  <ul>
    <li>
      <p>
        <a href=""><strong>Silver earrings</strong></a>: $99.95.
      </p>
      <img src="../products/3333-0985/thumb.png" alt="Silver earrings" />
    </li>
    <li>…</li>
  </ul>
  <p>Total cost: $237.89</p>
</div>
```

Questo non è realmente un `<aside>`, poiché non si relaziona necessariamente con il contenuto principale della pagina (vuoi che sia visibile ovunque). Non giustifica nemmeno particolarmente l'uso di un `<section>`, poiché non è parte del contenuto principale della pagina. Quindi un `<div>` va bene in questo caso. Abbiamo incluso un'intestazione come segnalibro per aiutare gli utenti di screen reader a trovarlo.

> [!WARNING]
> I `div` sono così convenienti da utilizzare che è facile usarli troppo. Poiché non hanno un valore semantico, semplicemente affollano il tuo codice HTML. Fai attenzione a usarli solo quando non c'è una soluzione semantica migliore e cerca di ridurne l'uso al minimo altrimenti avrai difficoltà ad aggiornare e mantenere i tuoi documenti.

> [!CALLOUT]
>
> **Provalo**
>
> Il tutorial interattivo [HTML semantico](https://scrimba.com/learn-accessible-web-design-c031/~0b?via=mdn) <sup>[_Partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> di Scrimba fornisce un utile riepilogo della marcatura semantica e perché dovresti usarla, oltre a una sfida che testa la tua capacità di migliorare un codice HTML con elementi semantici.

### Interruzioni di riga e regole orizzontali

Due elementi che userai occasionalmente e che vorrai conoscere sono {{htmlelement("br")}} e {{htmlelement("hr")}}.

#### \<br>: l'elemento line break

`<br>` crea un'interruzione di riga in un paragrafo; è l'unico modo per forzare una struttura rigida in una situazione in cui desideri una serie di righe brevi fisse, come in un indirizzo postale o una poesia. Per esempio:

```html
<p>
  There once was a man named O'Dell<br />
  Who loved to write HTML<br />
  But his structure was bad, his semantics were sad<br />
  and his markup didn't read very well.
</p>
```

Senza gli elementi `<br>`, il paragrafo sarebbe semplicemente reso in una lunga riga (come abbiamo detto in precedenza nel corso, [l'HTML ignora la maggior parte degli spazi bianchi](/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax#whitespace_in_html)); con gli elementi `<br>` nel codice, il markup si presenta così:

{{EmbedLiveSample('br_the_line_break_element', '100%', 150)}}

#### \<hr>: l'elemento thematic break

Gli elementi `<hr>` creano una regola orizzontale nel documento che denota un cambiamento tematico nel testo (come un cambiamento di argomento o scena). Visivamente appare semplicemente come una linea orizzontale. Come esempio:

```html
<p>
  Ron was backed into a corner by the marauding netherbeasts. Scared, but
  determined to protect his friends, he raised his wand and prepared to do
  battle, hoping that his distress call had made it through.
</p>
<hr />
<p>
  Meanwhile, Harry was sitting at home, staring at his royalty statement and
  pondering when the next spin off series would come out, when an enchanted
  distress letter flew through his window and landed in his lap. He read it
  hazily and sighed; "better get back to work then", he mused.
</p>
```

Sarebbe reso così:

{{EmbedLiveSample('hr_the_thematic_break_element', '100%', '185px')}}

## Pianificare un sito web semplice

Una volta pianificata la struttura di una semplice pagina web, il passo logico successivo è cercare di capire quale contenuto si desidera inserire in un intero sito web, quali pagine sono necessarie e come dovrebbero essere organizzate e collegate tra loro per offrire la migliore esperienza utente possibile. Questo si chiama {{Glossary("Information_architecture", "Architettura dell'informazione")}}. In un sito web grande e complesso, in questo processo può entrare molta pianificazione, ma per un sito semplice composto da poche pagine, questo può essere piuttosto semplice e divertente!

1. Tieni presente che avrai alcuni elementi comuni alla maggior parte (se non a tutte) delle pagine — come il menu di navigazione e il contenuto del piè di pagina. Se il tuo sito è per un'azienda, ad esempio, è una buona idea avere le tue informazioni di contatto disponibili nel piè di pagina su ciascuna pagina. Annota cosa desideri avere in comune a ogni pagina.![le caratteristiche comuni del sito di viaggi da mettere su ogni pagina: titolo e logo, contatto, copyright, termini e condizioni, selettore della lingua, politica di accessibilità](common-features.png)
2. Successivamente, disegna uno schizzo approssimativo di come potresti voler che la struttura di ciascuna pagina appaia (potrebbe sembrare come il nostro semplice sito web sopra). Annotare cos'è ciascun blocco.![Un semplice diagramma di una struttura del sito di esempio, con un'intestazione, un'area contenuto principale, due barre laterali opzionali e un piè di pagina](site-structure.png)
3. Ora, cerca di ordinare tutti gli altri (non comuni a ogni pagina) contenuti che vuoi avere sul tuo sito web — scrivi una lunga lista.![Un lungo elenco di tutte le funzionalità che potremmo inserire nel nostro sito di viaggi, dalla ricerca, alle offerte speciali e informazioni specifiche sul paese](feature-list.png)
4. Successivamente, cerca di ordinare tutti questi elementi di contenuto in gruppi, per avere un'idea di quali parti potrebbero vivere insieme su diverse pagine. Questo è molto simile a una tecnica chiamata {{Glossary("Card_sorting", "Card sorting")}}.![Gli elementi che dovrebbero apparire su un sito per le vacanze ordinati in 5 categorie: Cerca, Offerte speciali, Info specifiche del paese, Risultati della ricerca e Acquista le cose](card-sorting.png)
5. Ora prova a disegnare una mappa del sito approssimativa — crea una bolla per ciascuna pagina del tuo sito e traccia linee per mostrare il flusso di lavoro tipico tra le pagine. La homepage sarà probabilmente al centro e collegherà la maggior parte, se non tutte, delle altre; la maggior parte delle pagine in un sito piccolo dovrebbe essere disponibile dalla navigazione principale, sebbene ci siano eccezioni. Potresti anche voler includere annotazioni su come potrebbero essere presentate le cose.![Una mappa del sito che mostra la homepage, pagina del paese, risultati di ricerca, pagina delle offerte speciali, cassa e pagina di acquisto](site-map.png)

### Apprendimento attivo: crea la tua mappa del sito

Prova a svolgere l'esercizio sopra per un sito web di tua creazione. Di cosa ti piacerebbe realizzare un sito?

> [!NOTE]
> Salva il tuo lavoro da qualche parte; potresti averne bisogno più avanti.

## Riepilogo

A questo punto, dovresti avere un'idea migliore di come strutturare una pagina/ un sito web. Nel prossimo articolo di questo modulo, esamineremo alcune tecniche di testo avanzate.

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Lists", "Learn_web_development/Core/Structuring_content/Advanced_text_features", "Learn_web_development/Core/Structuring_content")}}
