---
title: Strutturare i documenti
slug: Learn_web_development/Core/Structuring_content/Structuring_documents
l10n:
  sourceCommit: 2066cc916dfdcbb782340bf0ce562b230e947cba
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Marking_up_a_letter", "Learn_web_development/Core/Structuring_content/Creating_links", "Learn_web_development/Core/Structuring_content")}}

Oltre a definire singole parti della pagina (come "un paragrafo" o "un'immagine"), {{Glossary("HTML", "HTML")}} offre anche diversi elementi a livello di blocco usati per definire aree del sito web, come "l'intestazione", "il menu di navigazione" o "la colonna del contenuto principale". Questo articolo esamina come pianificare la struttura di base di un sito web e scrivere l'HTML per rappresentarla.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Conoscenza di base di HTML, come illustrato in
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi HTML di base</a
        >. Semantica a livello di testo, come <a href="/it/docs/Learn_web_development/Core/Structuring_content/Headings_and_paragraphs"
          >titoli e paragrafi</a
        > ed <a href="/it/docs/Learn_web_development/Core/Structuring_content/Lists"
          >elenchi</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>I comuni elementi strutturali semantici HTML, ad esempio <code>&lt;main&gt;</code>, <code>&lt;section&gt;</code>, <code>&lt;article&gt;</code>, <code>&lt;header&gt;</code>, <code>&lt;nav&gt;</code> e <code>&lt;footer&gt;</code>, e come usarli correttamente.</li>
          <li>La necessità di usare elementi semantici nei punti appropriati, anziché utilizzare semplicemente elementi <code>&lt;div&gt;</code> ovunque sia richiesto un contenitore a livello di blocco, e i vantaggi che ne derivano (come una migliore accessibilità).</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Sezioni di base di un documento

Le pagine web possono e saranno molto diverse tra loro, ma tendono tutte a condividere componenti standard simili, a meno che la pagina non visualizzi un video o un gioco a schermo intero, non faccia parte di un progetto artistico o non sia semplicemente strutturata male:

- intestazione:
  - : Di solito una grande fascia nella parte superiore con un titolo grande, un logo e forse uno slogan. In genere rimane uguale da una pagina del sito a un'altra.
- barra di navigazione:
  - : Collegamenti alle sezioni principali del sito; generalmente rappresentati da pulsanti di menu, collegamenti o schede. Come l'intestazione, questo contenuto di solito rimane coerente da una pagina web all'altra: una navigazione incoerente sul sito non farà che confondere e frustrare gli utenti. Molti web designer considerano la barra di navigazione parte dell'intestazione anziché un componente separato, ma non è un requisito; in effetti, alcuni sostengono anche che tenerle separate sia migliore per l'[accessibilità](/it/docs/Learn_web_development/Core/Accessibility), poiché gli screen reader possono leggere meglio le due funzionalità se sono distinte.
- contenuto principale:
  - : Un'ampia area al centro che contiene la maggior parte del contenuto unico di una determinata pagina web, ad esempio il video da guardare, l'articolo principale da leggere, la mappa da visualizzare, i titoli delle notizie e così via. Questa è la parte del sito web che varierà sicuramente da una pagina all'altra!
- barra laterale:
  - : Informazioni periferiche, collegamenti, citazioni, pubblicità e così via. In genere sono contestuali a quanto contenuto nel contenuto principale (ad esempio, nella pagina di un articolo di notizie, la barra laterale potrebbe contenere la biografia dell'autore o collegamenti ad articoli correlati), ma esistono anche casi in cui sono presenti elementi ricorrenti come un sistema di navigazione secondario.
- piè di pagina:
  - : Una fascia nella parte inferiore della pagina che generalmente contiene note in caratteri piccoli, avvisi di copyright o informazioni di contatto. È un luogo in cui inserire informazioni comuni (come l'intestazione), ma di solito tali informazioni non sono critiche o sono secondarie rispetto al sito web stesso. Il piè di pagina viene talvolta usato anche per scopi di {{Glossary("SEO", "SEO")}}, fornendo collegamenti per accedere rapidamente ai contenuti più popolari.

Un "sito web tipico" potrebbe essere strutturato in questo modo:

![un semplice esempio di struttura di sito web con titolo principale, menu di navigazione, contenuto principale, barra laterale e piè di pagina.](sample-website.png)

> [!NOTE]
> L'immagine precedente illustra le sezioni principali di un documento, che possono essere definite con HTML. Tuttavia, l'_aspetto_ della pagina mostrata qui — inclusi layout, colori e font — si ottiene applicando [CSS](/it/docs/Learn_web_development/Core/Styling_basics) all'HTML.

## HTML per strutturare il contenuto

L'esempio mostrato sopra non è particolarmente bello, ma è perfettamente adeguato per illustrare un esempio di layout tipico di un sito web. Alcuni siti web hanno più colonne, altri sono molto più complessi, ma il concetto è chiaro. Con il CSS appropriato, si potrebbero usare praticamente tutti gli elementi per racchiudere le diverse sezioni e ottenere l'aspetto desiderato, ma, come discusso in precedenza, occorre rispettare la semantica e **usare l'elemento giusto per il compito giusto**.

Questo perché gli aspetti visivi non raccontano tutta la storia. Si usano colore e dimensione del font per attirare l'attenzione degli utenti vedenti sulle parti più utili del contenuto, come il menu di navigazione e i collegamenti correlati, ma che dire, ad esempio, delle persone ipovedenti, per le quali concetti come "rosa" e "font grande" potrebbero non essere molto utili?

> [!NOTE]
> [Circa l'8% degli uomini e lo 0,5% delle donne](https://www.color-blindness.com/) ha un deficit della visione dei colori; oppure, detto in un altro modo, approssimativamente 1 uomo su 12 e 1 donna su 200. Le persone cieche e ipovedenti rappresentano circa il 4-5% della popolazione mondiale (nel 2015 c'erano [940 milioni di persone con un certo grado di perdita della vista](https://en.wikipedia.org/wiki/Visual_impairment), mentre la popolazione totale era di [circa 7,5 miliardi](https://en.wikipedia.org/wiki/World_human_population#/media/File:World_population_history.svg)).

Nel codice HTML, è possibile effettuare il markup delle sezioni di contenuto in base alla loro _funzionalità_: si possono usare elementi che rappresentano in modo inequivocabile le sezioni di contenuto descritte sopra e le tecnologie assistive, come gli screen reader, possono riconoscere tali elementi e aiutare in attività come "trova la navigazione principale" o "trova il contenuto principale". Come già menzionato nel corso, esistono diverse [conseguenze derivanti dal mancato uso della giusta struttura di elementi e della semantica appropriata](/it/docs/Learn_web_development/Core/Structuring_content/Headings_and_paragraphs#why_do_we_need_structure).

Per implementare questo markup semantico, HTML fornisce tag dedicati che possono essere usati per rappresentare tali sezioni, ad esempio:

- **intestazione:** {{htmlelement("header")}}.
- **barra di navigazione:** {{htmlelement("nav")}}.
- **contenuto principale:** {{htmlelement("main")}}, con varie sottosezioni di contenuto rappresentate dagli elementi {{HTMLElement("article")}}, {{htmlelement("section")}} e {{htmlelement("div")}}.
- **barra laterale:** {{htmlelement("aside")}}; spesso inserita all'interno di {{htmlelement("main")}}.
- **piè di pagina:** {{htmlelement("footer")}}.

### Esplorare il codice dell'esempio

L'esempio visto sopra è rappresentato dal seguente codice (è anche possibile [trovare il codice nel repository GitHub](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/document_and_website_structure/index.html) e [visualizzare l'esempio dal vivo](https://mdn.github.io/learning-area/html/introduction-to-html/document_and_website_structure/)). Si invita a osservare l'elenco seguente per vedere quali parti compongono ciascuna sezione dell'output visivo.

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

      <!-- A Search form: another common non-linear
           way to navigate through a site. -->

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

Dedica un po' di tempo a esaminare e comprendere il codice: anche i commenti al suo interno dovrebbero aiutare a capirlo. In questo articolo non viene richiesto di fare molto altro, perché la chiave per comprendere il layout dei documenti è scrivere una solida struttura HTML e poi disporla con CSS. Questo verrà affrontato quando inizierà lo studio del layout CSS nell'ambito dell'argomento CSS.

## Elementi HTML di layout in maggiore dettaglio

È utile comprendere nel dettaglio il significato generale di tutti gli elementi HTML di sezionamento: questo è un aspetto su cui si lavorerà gradualmente acquisendo maggiore esperienza nello sviluppo web. Molti dettagli sono disponibili consultando il [riferimento degli elementi HTML](/it/docs/Web/HTML/Reference/Elements). Per ora, queste sono le definizioni principali che occorre cercare di comprendere:

- {{HTMLElement('main')}} serve per il contenuto _unico di questa pagina._ Usare `<main>` solo _una volta_ per pagina e inserirlo direttamente all'interno di {{HTMLElement('body')}}. Idealmente, non dovrebbe essere annidato all'interno di altri elementi.
- {{HTMLElement('article')}} racchiude un blocco di contenuto correlato che ha senso da solo, senza il resto della pagina (ad esempio, un singolo post di blog).
- {{HTMLElement('section')}} è simile a `<article>`, ma serve maggiormente a raggruppare una singola parte della pagina che costituisce una sola funzionalità (come una mini mappa o un insieme di titoli e riepiloghi di articoli), oppure un tema. È considerata una buona pratica iniziare ogni sezione con un [titolo](/it/docs/Learn_web_development/Core/Structuring_content/Headings_and_paragraphs); inoltre, è possibile suddividere gli `<article>` in diverse `<section>`, oppure le `<section>` in diversi `<article>`, a seconda del contesto.
- {{HTMLElement('aside')}} contiene contenuto non direttamente correlato al contenuto principale, ma che può fornire informazioni aggiuntive indirettamente correlate a esso (voci di glossario, biografia dell'autore, collegamenti correlati e così via).
- {{HTMLElement('header')}} rappresenta un gruppo di contenuti introduttivi. Se è figlio di {{HTMLElement('body')}}, definisce l'intestazione globale di una pagina web, mentre se è figlio di un {{HTMLElement('article')}} o di {{HTMLElement('section')}}, definisce un'intestazione specifica per quella sezione (da non confondere con [titoli e intestazioni](/it/docs/Learn_web_development/Core/Structuring_content/Webpage_metadata#adding_a_title)).
- {{HTMLElement('nav')}} contiene la funzionalità di navigazione principale della pagina. I collegamenti secondari e simili non dovrebbero essere inclusi nella navigazione.
- {{HTMLElement('footer')}} rappresenta un gruppo di contenuti conclusivi per una pagina.

Ciascuno degli elementi menzionati può essere selezionato per leggere l'articolo corrispondente nella sezione "Riferimento degli elementi HTML", che fornisce maggiori dettagli su ogni elemento.

### Wrapper non semantici

Talvolta si incontrerà una situazione in cui non è possibile trovare un elemento semantico ideale per raggruppare alcuni elementi o racchiudere del contenuto. A volte potrebbe essere necessario semplicemente raggruppare un insieme di elementi per applicare a tutti, come singola entità, del {{Glossary("CSS", "CSS")}} o del {{Glossary("JavaScript", "JavaScript")}}. Per casi come questi, HTML fornisce gli elementi {{HTMLElement("div")}} e {{HTMLElement("span")}}. È preferibile usarli con un attributo [`class`](/it/docs/Web/HTML/Reference/Global_attributes/class) appropriato, per fornire loro un qualche tipo di etichetta che permetta di individuarli facilmente.

{{HTMLElement("span")}} è un elemento inline non semantico, da usare solo se non viene in mente un elemento di testo semantico migliore per racchiudere il contenuto, o se non si vuole aggiungere alcun significato specifico. Ad esempio:

```html
<p>
  The King walked drunkenly back to his room at 01:00, the beer doing nothing to
  aid him as he staggered through the door.
  <span class="editor-note">
    [Editor's note: At this point in the play, the lights should be down low].
  </span>
</p>
```

In questo caso, la nota dell'editor serve semplicemente a fornire indicazioni aggiuntive al regista dell'opera; non deve avere un significato semantico aggiuntivo. Per gli utenti vedenti, probabilmente si userebbe CSS per distanziare leggermente la nota dal testo principale.

{{HTMLElement("div")}} è un elemento a livello di blocco non semantico, da usare solo se non viene in mente un elemento di blocco semantico migliore, o se non si vuole aggiungere alcun significato specifico. Ad esempio, si immagini un widget del carrello degli acquisti che può essere aperto in qualsiasi momento durante la navigazione in un sito di e-commerce:

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

Non si tratta propriamente di un `<aside>`, poiché non è necessariamente correlato al contenuto principale della pagina (deve essere visualizzabile ovunque). Non giustifica nemmeno in modo particolare l'uso di un `<section>`, poiché non fa parte del contenuto principale della pagina. In questo caso, quindi, un `<div>` va bene. È stato incluso un titolo come indicatore per aiutare gli utenti di screen reader a trovarlo.

> [!WARNING]
> I `div` sono così pratici da usare che è facile abusarne. Poiché non hanno alcun valore semantico, si limitano a ingombrare il codice HTML. Occorre usarli solo quando non esiste una soluzione semantica migliore e cercare di ridurne l'uso al minimo; in caso contrario, aggiornare e mantenere i documenti diventerà difficile.

> [!NOTE]
> Il tutorial interattivo [Semantic HTML](https://scrimba.com/learn-accessible-web-design-c031/~0b?via=mdn) di Scrimba <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> fornisce un utile ripasso del markup semantico e dei motivi per usarlo, oltre a una sfida che verifica la capacità di migliorare una codebase HTML con elementi semantici.

### Interruzioni di riga e righe orizzontali

Due elementi che verranno usati occasionalmente e che è utile conoscere sono {{htmlelement("br")}} e {{htmlelement("hr")}}.

#### \<br>: l'elemento per l'interruzione di riga

`<br>` crea un'interruzione di riga in un paragrafo; è l'unico modo per imporre una struttura rigida in una situazione in cui è necessaria una serie di brevi righe fisse, come in un indirizzo postale o in una poesia. Ad esempio:

```html
<p>
  There once was a man named O'Dell<br />
  Who loved to write HTML<br />
  But his structure was bad, his semantics were sad<br />
  and his markup didn't read very well.
</p>
```

Senza gli elementi `<br>`, il paragrafo verrebbe semplicemente reso come un'unica lunga riga (come già detto in precedenza nel corso, [HTML ignora la maggior parte degli spazi bianchi](/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax#whitespace_in_html)); con gli elementi `<br>` nel codice, il markup viene reso in questo modo:

{{EmbedLiveSample('br_the_line_break_element', '100%', 150)}}

#### \<hr>: l'elemento di separazione tematica

Gli elementi `<hr>` creano una riga orizzontale nel documento che indica un cambiamento tematico nel testo (come un cambio di argomento o di scena). Visivamente appare semplicemente come una linea orizzontale. Ad esempio:

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

Verrebbe reso in questo modo:

{{EmbedLiveSample('hr_the_thematic_break_element', '100%', '185px')}}

## Strutturare un sito web di base

La fase successiva, dopo aver pianificato la struttura di una singola pagina web, consiste nel pianificare la struttura di un intero sito web multipagina, incluso il modo in cui le pagine devono essere organizzate e collegate tra loro per offrire la migliore esperienza utente possibile. Questo processo viene chiamato {{Glossary("Information_architecture", "architettura dell'informazione")}}.

In un sito web grande e complesso, questo processo può richiedere molta pianificazione, ma per un sito web di base con poche pagine può essere un esercizio rapido e divertente.

Il processo potrebbe essere il seguente:

1. Ci saranno alcuni elementi comuni alla maggior parte (se non a tutte) le pagine, come il menu di navigazione e il contenuto del piè di pagina. Se il sito è dedicato a un'azienda, ad esempio, è una buona idea rendere disponibili le informazioni di contatto nel piè di pagina di ogni pagina. Annotare ciò che si desidera avere in comune in ogni pagina. Ad esempio:
   - Intestazione:
     - Titolo e logo
     - Selettore della lingua del sito
   - Menu di navigazione
   - Piè di pagina:
     - Avviso di copyright
     - Collegamento a termini e condizioni, recapiti e informativa sull'accessibilità

2. Successivamente, disegnare uno schizzo approssimativo di come potrebbe apparire la struttura di ogni pagina (potrebbe assomigliare al semplice sito web mostrato sopra). Annotare cosa rappresenterà ogni blocco.![Un semplice diagramma della struttura di un sito di esempio, con intestazione, area del contenuto principale, due barre laterali opzionali e piè di pagina](/shared-assets/images/diagrams/learn/structuring-documents/site-structure.svg)
3. Ora, raccogliere idee su tutti gli altri contenuti (non comuni a ogni pagina) da inserire nel sito web. Ad esempio:
   - Voli
   - Alloggi
   - Trasporti
   - Cose da fare
   - Offerte speciali
   - Pacchetti vacanza popolari, ad esempio vacanze al sole in inverno, sci
   - Risultati della ricerca
   - Recensioni
   - Requisiti per visto/ingresso
   - Valuta
   - Lingue e cultura
   - Acquista vacanze

4. Successivamente, provare a ordinare tutti questi elementi di contenuto in gruppi, per avere un'idea di quali parti potrebbero trovarsi insieme in pagine diverse. Questo è molto simile a una tecnica chiamata {{Glossary("Card_sorting", "ordinamento delle schede")}}.
   - Ricerca
     - Voli
     - Alloggi
     - Trasporti
     - Cose da fare
   - Offerte speciali
     - Vacanze popolari
     - Vacanze al sole in inverno
     - Sci
   - Risultati della ricerca
     - Recensioni
     - Informazioni specifiche per paese
       - Requisiti per visto/ingresso
       - Valuta
       - Lingue e cultura
   - Acquista vacanze

5. Ora provare a disegnare una sitemap approssimativa: creare un riquadro per ogni pagina del sito e tracciare linee per mostrare il flusso di lavoro tipico tra le pagine. La homepage sarà probabilmente in alto o al centro e si collegherà alla maggior parte, se non a tutte, delle altre pagine. La maggior parte delle pagine di un sito piccolo dovrebbe essere disponibile dalla navigazione principale, anche se esistono eccezioni. Potrebbe inoltre essere utile includere note su come i contenuti potrebbero essere presentati.![Una mappa del sito che mostra la homepage, la pagina del paese, i risultati della ricerca, la pagina delle offerte speciali e il flusso di checkout e acquisto](/shared-assets/images/diagrams/learn/structuring-documents/site-map.svg)

Provare a svolgere l'esercizio precedente per un sito web di propria creazione. Su quale argomento dovrebbe essere il sito? Come obiettivo aggiuntivo, usare le conoscenze HTML acquisite finora per creare alcune pagine del sito. Si può usare il [modello HTML di base](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/getting-started/index.html) come punto di partenza.

## Riepilogo

A questo punto, dovrebbe essere più chiaro come strutturare una pagina web o un sito web. Nel prossimo articolo di questo modulo verrà illustrato come creare collegamenti ipertestuali, una delle funzionalità fondamentali del web.

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Marking_up_a_letter", "Learn_web_development/Core/Structuring_content/Creating_links", "Learn_web_development/Core/Structuring_content")}}
