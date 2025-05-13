---
title: Strutturare i documenti
slug: Learn_web_development/Core/Structuring_content/Structuring_documents
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Lists", "Learn_web_development/Core/Structuring_content/Advanced_text_features", "Learn_web_development/Core/Structuring_content")}}

Oltre a definire le singole parti della pagina (come "un paragrafo" o "un'immagine"), {{Glossary("HTML", "HTML")}} offre anche una serie di elementi a livello blocco utilizzati per definire le aree del tuo sito web (come "l'intestazione", "il menu di navigazione", "la colonna del contenuto principale"). Questo articolo esplora come pianificare una struttura di base per un sito web e scrivere l'HTML per rappresentare questa struttura.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con HTML, come trattato in
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
          <li>La necessità di utilizzare gli elementi semantici nei posti appropriati, piuttosto che utilizzare semplicemente gli elementi <code>&lt;div&gt;</code> ovunque sia richiesto un contenitore a livello blocco, e i vantaggi di ciò (come un miglioramento dell'accessibilità).</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Sezioni di base di un documento

Le pagine web possono e saranno piuttosto diverse l'una dall'altra, ma tendono tutte a condividere componenti standard simili, a meno che la pagina non visualizzi un video o un gioco a pieno schermo, non faccia parte di un progetto artistico o non sia semplicemente strutturata male:

- intestazione:
  - : Di solito una grande striscia in alto con un grande titolo, il logo e forse uno slogan. Questo di solito rimane lo stesso da una pagina all'altra di un sito web.
- barra di navigazione:
  - : Link alle sezioni principali del sito; di solito rappresentati da pulsanti di menu, link o schede. Come l'intestazione, questo contenuto di solito rimane coerente da una pagina web all'altra — avere una navigazione incoerente sul tuo sito web porterà solo a utenti confusi e frustrati. Molti designer considerano la barra di navigazione parte dell'intestazione piuttosto che un componente singolo, ma non è un requisito; infatti, alcuni sostengono che avere i due separati è meglio per l'[accessibilità](/it/docs/Learn_web_development/Core/Accessibility), poiché i lettori di schermo possono leggere meglio le due funzionalità se sono separate.
- contenuto principale:
  - : Un'ampia area centrale che contiene la maggior parte dei contenuti unici di una data pagina web, ad esempio, il video che desidera guardare, o l'articolo principale che sta leggendo, o la mappa che desidera visualizzare, o i titoli delle notizie, ecc. Questa è l'unica parte del sito web che sicuramente varierà da una pagina all'altra!
- barra laterale:
  - : Alcune informazioni periferiche, link, citazioni, annunci, ecc. Di solito, questa è contestuale a ciò che è contenuto nel contenuto principale (ad esempio, su una pagina di articolo di notizie, la barra laterale potrebbe contenere la biografia dell'autore o link a articoli correlati), ma ci sono anche casi in cui si trovano alcuni elementi ricorrenti come un sistema di navigazione secondario.
- piè di pagina:
  - : Una striscia in fondo alla pagina che generalmente contiene le note legali, gli avvisi sul copyright o le informazioni di contatto. È un luogo per mettere informazioni comuni (come l'intestazione) ma di solito tali informazioni non sono critiche o secondarie al sito web stesso. Il piè di pagina è talvolta utilizzato anche per {{Glossary("SEO", "SEO")}}, fornendo link per un accesso rapido ai contenuti popolari.

Una "tipica pagina web" potrebbe essere strutturata in questo modo:

![un esempio di struttura di sito web semplice con un'intestazione principale, un menu di navigazione, contenuto principale, una barra laterale e un piè di pagina.](sample-website.png)

> [!NOTE]
> L'immagine sopra illustra le sezioni principali di un documento, che puoi definire con HTML. Tuttavia, l'_aspetto_ della pagina mostrata qui — inclusi layout, colori e font — è ottenuto applicando il [CSS](/it/docs/Learn_web_development/Core/Styling_basics) all'HTML.

## HTML per strutturare il contenuto

L'esempio mostrato sopra non è elegante, ma è perfettamente adeguato per illustrare un esempio di layout tipico di un sito web. Alcuni siti web hanno più colonne, altri sono molto più complessi, ma hai capito l'idea. Con il giusto CSS, potresti usare praticamente qualsiasi elemento per racchiudere le diverse sezioni e ottenere l'aspetto desiderato, ma come discusso in precedenza, è importante rispettare la semantica e **usare l'elemento giusto per il lavoro giusto**.

Questo perché i visivi non raccontano l'intera storia. Utilizziamo colori e dimensioni dei font per attirare l'attenzione degli utenti vedenti verso le parti più utili del contenuto, come il menu di navigazione e i link correlati, ma che dire delle persone non vedenti, ad esempio, che potrebbero non trovare concetti come "rosa" e "font grande" molto utili?

> **Nota:** [Circa l'8% degli uomini e lo 0,5% delle donne](https://www.color-blindness.com/) sono daltonici; in altri termini, approssimativamente 1 uomo su 12 e 1 donna su 200. Le persone cieche e ipovedenti rappresentano circa il 4-5% della popolazione mondiale (nel 2015 c'erano [940 milioni di persone con un certo grado di perdita della vista](https://en.wikipedia.org/wiki/Visual_impairment), mentre la popolazione totale era [di circa 7,5 miliardi](https://en.wikipedia.org/wiki/World_human_population#/media/File:World_population_history.svg)).

Nel tuo codice HTML, puoi marcare sezioni di contenuto basandoti sulla loro _funzionalità_ — puoi usare elementi che rappresentano le sezioni di contenuto descritte sopra in modo inequivocabile, e le tecnologie assistive come i lettori di schermo possono riconoscere quegli elementi e aiutare con compiti come "trovare la navigazione principale", o "trovare il contenuto principale". Come abbiamo menzionato in precedenza nel corso, ci sono una serie di [conseguenze del non utilizzare la struttura e la semantica degli elementi giusti per il lavoro giusto](/it/docs/Learn_web_development/Core/Structuring_content/Headings_and_paragraphs#why_do_we_need_structure).

Per implementare tale markup semantico, HTML fornisce tag dedicati che puoi utilizzare per rappresentare tali sezioni, ad esempio:

- **intestazione:** {{htmlelement("header")}}.
- **barra di navigazione:** {{htmlelement("nav")}}.
- **contenuto principale:** {{htmlelement("main")}}, con varie sottosezioni di contenuto rappresentate da {{HTMLElement("article")}}, {{htmlelement("section")}}, e {{htmlelement("div")}}.
- **barra laterale:** {{htmlelement("aside")}}; spesso posizionata all'interno di {{htmlelement("main")}}.
- **piè di pagina:** {{htmlelement("footer")}}.

### Apprendimento attivo: esplorare il codice del nostro esempio

Il nostro esempio visto sopra è rappresentato dal seguente codice (puoi anche [trovare l'esempio nel nostro repository GitHub](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/document_and_website_structure/index.html)). Ci piacerebbe che tu guardassi l'esempio sopra e poi esaminassi l'elenco qui sotto per vedere quali parti compongono quale sezione del visivo.

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

Prenda un po' di tempo per esaminare il codice e comprenderlo — i commenti all'interno del codice dovrebbero aiutarti a comprenderlo. Non stiamo chiedendo di fare molto altro in questo articolo, perché la chiave per comprendere il layout del documento è scrivere una struttura HTML solida e poi disporla con CSS. Aspetteremo questo fino a quando inizierai a studiare il layout CSS come parte dell'argomento CSS.

## Elementi di layout HTML in dettaglio

È importante comprendere il significato complessivo di tutti gli elementi di sezione HTML nel dettaglio — questo è qualcosa su cui lavorerai gradualmente man mano che acquisirai più esperienza nello sviluppo web. Puoi trovare molti dettagli leggendo il nostro [Riferimento agli elementi HTML](/it/docs/Web/HTML/Reference/Elements). Per ora, queste sono le principali definizioni che dovresti cercare di comprendere:

- {{HTMLElement('main')}} è per contenuti _unici per questa pagina._ Usa `<main>` solo _una volta_ per pagina, e posizionalo direttamente all'interno di {{HTMLElement('body')}}. Idealmente non dovrebbe essere annidato all'interno di altri elementi.
- {{HTMLElement('article')}} racchiude un blocco di contenuti correlati che hanno senso da soli senza il resto della pagina (ad es., un singolo post di blog).
- {{HTMLElement('section')}} è simile a `<article>`, ma è più per raggruppare insieme una singola parte della pagina che costituisce un singolo pezzo di funzionalità (ad esempio, una mini mappa, o un insieme di titoli di articoli e riassunti), o un tema. Si considera buona pratica iniziare ogni sezione con un'[intestazione](/it/docs/Learn_web_development/Core/Structuring_content/Headings_and_paragraphs); nota anche che puoi suddividere `<article>` in diverse `<section>`, o `<section>` in diversi `<article>`, a seconda del contesto.
- {{HTMLElement('aside')}} contiene contenuti che non sono direttamente correlati al contenuto principale, ma possono fornire informazioni aggiuntive indirettamente collegate a questo (voci del glossario, biografia dell'autore, link correlati, ecc.).
- {{HTMLElement('header')}} rappresenta un gruppo di contenuti introduttivi. Se è un figlio di {{HTMLElement('body')}} definisce l'intestazione globale di una pagina web, ma se è un figlio di un {{HTMLElement('article')}} o {{HTMLElement('section')}} definisce una specifica intestazione per quella sezione (cerchi di non confondere questo con [titoli e intestazioni](/it/docs/Learn_web_development/Core/Structuring_content/Webpage_metadata#adding_a_title)).
- {{HTMLElement('nav')}} contiene la funzionalità di navigazione principale per la pagina. Link secondari, ecc., non andrebbero nella navigazione.
- {{HTMLElement('footer')}} rappresenta un gruppo di contenuti finali per una pagina.

Ciascuno degli elementi sopra menzionati può essere cliccato per leggere l'articolo corrispondente nella sezione "Riferimento agli elementi HTML", fornendo più dettagli su ciascuno.

### Involucri non semantici

A volte ci si imbatterà in una situazione in cui non è possibile trovare un elemento semantico ideale per raggruppare alcuni elementi insieme o racchiudere del contenuto. A volte si desidera semplicemente raggruppare un insieme di elementi insieme per influenzarli tutti come un'unica entità con del {{Glossary("CSS", "CSS")}} o {{Glossary("JavaScript", "JavaScript")}}. Per casi come questi, HTML fornisce gli elementi {{HTMLElement("div")}} e {{HTMLElement("span")}}. Dovresti usarli preferibilmente con un attributo [`class`](/it/docs/Web/HTML/Reference/Global_attributes/class) adatto, per fornire loro una sorta di etichetta in modo che possano essere facilmente mirati.

{{HTMLElement("span")}} è un elemento non semantico inline, che dovresti usare solo se non riesci a pensare a un elemento testuale semantico migliore da utilizzare per racchiudere il tuo contenuto, o non vuoi aggiungere un significato specifico. Ad esempio:

```html
<p>
  The King walked drunkenly back to his room at 01:00, the beer doing nothing to
  aid him as he staggered through the door.
  <span class="editor-note">
    [Editor's note: At this point in the play, the lights should be down low].
  </span>
</p>
```

In questo caso, la nota dell'editor è pensata per fornire semplicemente ulteriore direzione al direttore della commedia; non vuole avere un significato semantico aggiuntivo. Per gli utenti vedenti, CSS potrebbe essere utilizzato per distanziare leggermente la nota dal testo principale.

{{HTMLElement("div")}} è un elemento non semantico a livello di blocco, che dovresti usare solo se non riesci a pensare a un elemento di blocco semantico migliore da utilizzare, o non vuoi aggiungere un significato specifico. Ad esempio, immagina un widget del carrello acquisti che potresti scegliere di visualizzare in qualsiasi momento durante il tempo su un sito di e-commerce:

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

Questo non è davvero un `<aside>`, poiché non è necessariamente correlato al contenuto principale della pagina (lo si vuole visualizzare ovunque). Non merita neanche di utilizzare un `<section>`, poiché non fa parte del contenuto principale della pagina. Quindi un `<div>` va bene in questo caso. Abbiamo incluso un'intestazione per aiutare gli utenti di lettori di schermo a trovarlo.

> [!WARNING]
> I div sono così comodi da usare che è facile abusarne. Dato che non hanno valore semantico, ingombrano semplicemente il tuo codice HTML. Fai attenzione a usarli solo quando non c'è una soluzione semantica migliore e cerca di ridurne l'uso al minimo altrimenti avrai difficoltà a aggiornare e mantenere i tuoi documenti.

### Linee di interruzione e regole orizzontali

Due elementi che utilizzerai occasionalmente e che vorrai conoscere sono {{htmlelement("br")}} e {{htmlelement("hr")}}.

#### \<br>: l'elemento di interruzione di linea

`<br>` crea un'interruzione di linea in un paragrafo; è l'unico modo per forzare una struttura rigida in una situazione in cui si desidera una serie di brevi righe fisse, come in un indirizzo postale o una poesia. Ad esempio:

```html
<p>
  There once was a man named O'Dell<br />
  Who loved to write HTML<br />
  But his structure was bad, his semantics were sad<br />
  and his markup didn't read very well.
</p>
```

Senza gli elementi `<br>`, il paragrafo verrebbe semplicemente reso su una lunga riga (come abbiamo detto in precedenza nel corso, [HTML ignora la maggior parte degli spazi bianchi](/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax#whitespace_in_html)); con gli elementi `<br>` nel codice, il markup viene reso così:

{{EmbedLiveSample('br_the_line_break_element', '100%', 150)}}

#### \<hr>: l'elemento di interruzione tematica

Gli elementi `<hr>` creano una regola orizzontale nel documento che denota un cambio tematico nel testo (come un cambio di tema o scena). Visivamente appare come una linea orizzontale. Come esempio:

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

Verrà reso così:

{{EmbedLiveSample('hr_the_thematic_break_element', '100%', '185px')}}

## Pianificare un sito web semplice

Una volta pianificata la struttura di una semplice pagina web, il passo logico successivo è cercare di capire quale contenuto vuoi inserire in un intero sito web, quali pagine sono necessarie e come dovrebbero essere organizzate e collegate tra loro per offrire la migliore esperienza possibile all'utente. Questo è chiamato {{Glossary("Information_architecture", "Architettura dell'informazione")}}. In un sito web grande e complesso, molta pianificazione può essere necessaria per questo processo, ma per un semplice sito web di poche pagine, può essere abbastanza semplice e divertente!

1. Tieni presente che avrai alcuni elementi comuni alla maggior parte (se non a tutte) delle pagine — come il menu di navigazione e il contenuto del piè di pagina. Se il tuo sito è per un'azienda, ad esempio, è una buona idea avere le informazioni di contatto disponibili nel piè di pagina su ogni pagina. Prenda nota di ciò che vuoi avere comune a ogni pagina.![le caratteristiche comuni del sito di viaggi da inserire su ogni pagina: titolo e logo, contatti, copyright, condizioni d'uso, selezione della lingua, politica sull'accessibilità](common-features.png)
2. Quindi, disegna un abbozzo di come potrebbe apparire la struttura di ciascuna pagina (potrebbe sembrare il nostro sito semplice sopra). Nota cosa sarà ogni blocco.![Un semplice diagramma di una struttura di sito di esempio, con un'intestazione, area contenuto principale, due barre laterali opzionali e un piè di pagina](site-structure.png)
3. Ora, fai un brainstorming di tutti i contenuti (non comuni a ciascuna pagina) che vuoi avere sul tuo sito web — scrivi una lunga lista.![Un lungo elenco di tutte le funzionalità che potremmo inserire nel nostro sito di viaggi, dalla ricerca, alle offerte speciali e alle informazioni specifiche per paese](feature-list.png)
4. Successivamente, prova a ordinare tutti questi elementi di contenuto in gruppi, per darti un'idea delle parti che potrebbero vivere insieme su diverse pagine. Questo è molto simile a una tecnica chiamata {{Glossary("Card_sorting", "Card sorting")}}.![Gli elementi che dovrebbero apparire su un sito di vacanze ordinati in 5 categorie: Ricerca, Offerte speciali, Info specifiche per paese, Risultati di ricerca e Acquisti](card-sorting.png)
5. Ora prova a schizzare una mappa del sito approssimativa — crea una bolla per ogni pagina del tuo sito e traccia linee per mostrare il flusso di lavoro tipico tra le pagine. La homepage sarà probabilmente al centro e collegherà la maggior parte, se non tutte, delle altre; la maggior parte delle pagine in un sito piccolo dovrebbe essere accessibile dalla navigazione principale, anche se ci sono eccezioni. Potresti anche voler includere note su come potrebbero essere presentate le cose.![Una mappa del sito che mostra la homepage, la pagina del paese, i risultati di ricerca, la pagina delle offerte speciali, il checkout e la pagina di acquisto](site-map.png)

### Apprendimento attivo: crea la tua mappa del sito

Prova a eseguire l'esercizio sopra per un sito web di sua creazione. Di cosa le piacerebbe creare un sito?

> [!NOTE]
> Salvi il suo lavoro da qualche parte; potrebbe averne bisogno in seguito.

## Sommario

A questo punto, dovrebbe avere un'idea migliore su come strutturare una pagina web/sito. Nel prossimo articolo di questo modulo, esamineremo alcune tecniche avanzate di testo.

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Lists", "Learn_web_development/Core/Structuring_content/Advanced_text_features", "Learn_web_development/Core/Structuring_content")}}
