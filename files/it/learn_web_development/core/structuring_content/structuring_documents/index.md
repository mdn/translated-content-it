---
title: Strutturare i documenti
slug: Learn_web_development/Core/Structuring_content/Structuring_documents
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Lists", "Learn_web_development/Core/Structuring_content/Advanced_text_features", "Learn_web_development/Core/Structuring_content")}}

Oltre a definire le singole parti della tua pagina (come "un paragrafo" o "un'immagine"), l'{{Glossary("HTML", "HTML")}} offre anche una serie di elementi di livello blocco usati per definire aree del tuo sito web (come "l'intestazione", "il menu di navigazione", "la colonna del contenuto principale"). Questo articolo esamina come pianificare una struttura basilare di un sito web e scrivere l'HTML per rappresentare tale struttura.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con l'HTML, come trattato in
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax">Sintassi di base HTML</a>. Semantica a livello di testo come <a href="/it/docs/Learn_web_development/Core/Structuring_content/Headings_and_paragraphs">intestazioni e paragrafi</a> e <a href="/it/docs/Learn_web_development/Core/Structuring_content/Lists">liste</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Gli elementi strutturali semantici comuni dell'HTML, ad esempio <code>&lt;main&gt;</code>, <code>&lt;section&gt;</code>, <code>&lt;article&gt;</code>, <code>&lt;header&gt;</code>, <code>&lt;nav&gt;</code>, e <code>&lt;footer&gt;</code>, e come usarli correttamente.</li>
          <li>La necessità di utilizzare elementi semantici in posti appropriati, anziché utilizzare solo elementi <code>&lt;div&gt;</code> ovunque sia richiesto un contenitore a livello blocco, e i benefici di ciò (come l'accessibilità migliorata).</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Sezioni base di un documento

Le pagine web possono avere e avranno un aspetto piuttosto diverso l'una dall'altra, ma tendono tutte a condividere componenti standard simili, a meno che la pagina non mostri un video o un gioco a schermo intero, non faccia parte di un progetto artistico, o sia semplicemente mal strutturata:

- header:
  - : Solitamente una grande striscia in cima con una grande intestazione, logo, e forse uno slogan. Di solito rimane lo stesso da una pagina all'altra di un sito web.
- barra di navigazione:
  - : Link alle sezioni principali del sito; solitamente rappresentati da pulsanti di menu, link o schede. Come l'intestazione, questo contenuto solitamente rimane coerente da una pagina web all'altra — avere una navigazione incoerente sul proprio sito web porterà solo a utenti confusi e frustrati. Molti web designer considerano la barra di navigazione come parte dell'intestazione piuttosto che un componente individuale, ma non è un requisito; infatti, alcuni sostengono che averli separati sia meglio per l'[accessibilità](/it/docs/Learn_web_development/Core/Accessibility), poiché i lettori di schermo possono leggere meglio le due funzioni se sono separate.
- contenuto principale:
  - : Una grande area al centro che contiene la maggior parte del contenuto unico di una determinata pagina web, per esempio, il video che vuoi guardare, o la storia principale che stai leggendo, o la mappa che vuoi visualizzare, o le notizie, ecc. Questa è la parte del sito web che sicuramente varierà di pagina in pagina!
- barra laterale:
  - : Alcune informazioni periferiche, link, citazioni, annunci, ecc. Solitamente, è contestuale a ciò che è contenuto nel contenuto principale (ad esempio su una pagina di articolo di notizie, la barra laterale potrebbe contenere la biografia dell'autore, o link ad articoli correlati) ma ci sono anche casi in cui si trovano alcuni elementi ricorrenti come un sistema di navigazione secondario.
- footer:
  - : Una striscia lungo il fondo della pagina che generalmente contiene le note a piè di pagina, avvisi di copyright, o informazioni di contatto. È un luogo per mettere informazioni comuni (come l'intestazione) ma di solito, quelle informazioni non sono critiche o sono secondarie al sito stesso. Il footer è talvolta usato anche per scopi di {{Glossary("SEO", "SEO")}}, fornendo dei link per l'accesso rapido al contenuto popolare.

Un "sito web tipico" potrebbe essere strutturato in questo modo:

![un esempio di struttura semplice di un sito web con un'intestazione principale, menu di navigazione, contenuto principale, barra laterale e footer.](sample-website.png)

> [!NOTE]
> L'immagine sopra illustra le sezioni principali di un documento, che puoi definire con HTML. Tuttavia, l'_aspetto_ della pagina mostrata qui — compreso il layout, i colori e i caratteri — si ottiene applicando [CSS](/it/docs/Learn_web_development/Core/Styling_basics) all'HTML.

## HTML per strutturare il contenuto

L'esempio mostrato sopra non è esteticamente bello, ma è perfettamente adatto per illustrare un esempio tipico di layout di un sito web. Alcuni siti web hanno più colonne, alcuni sono molto più complessi, ma hai capito l'idea. Con il CSS giusto, potresti usare praticamente qualsiasi elemento per avvolgere le diverse sezioni e farle apparire come desideri, ma come discusso prima, dobbiamo rispettare la semantica e **usare l'elemento giusto per il lavoro giusto**.

Questo perché la visuale non racconta tutta la storia. Usiamo colore e dimensione dei caratteri per attirare l'attenzione degli utenti vedenti sulle parti più utili del contenuto, come il menu di navigazione e i link correlati, ma che dire delle persone non vedenti, per esempio, che potrebbero non trovare molto utili concetti come "rosa" e "carattere grande"?

> **Note:** [Circa l'8% degli uomini e lo 0,5% delle donne](https://www.color-blindness.com/) sono daltonici; o, per dirla in un altro modo, all'incirca 1 uomo su 12 e 1 donna su 200. Le persone non vedenti e ipovedenti rappresentano circa il 4-5% della popolazione mondiale (nel 2015 c'erano [940 milioni di persone con un qualche grado di perdita della vista](https://en.wikipedia.org/wiki/Visual_impairment), mentre la popolazione totale era [circa 7,5 miliardi](https://en.wikipedia.org/wiki/World_human_population#/media/File:World_population_history.svg)).

Nel tuo codice HTML, puoi marcare sezioni di contenuto basandoti sulla loro _funzionalità_ — puoi usare elementi che rappresentano inequivocabilmente le sezioni di contenuto descritte sopra, e le tecnologie assistive come i lettori di schermo possono riconoscere quegli elementi e aiutare con attività come "trovare la navigazione principale" o "trovare il contenuto principale". Come abbiamo menzionato precedentemente nel corso, ci sono diverse [conseguenze nel non usare la corretta struttura degli elementi e semantica per il lavoro giusto](/it/docs/Learn_web_development/Core/Structuring_content/Headings_and_paragraphs#why_do_we_need_structure).

Per implementare tale marcatura semantica, l'HTML fornisce dei tag dedicati che puoi usare per rappresentare tali sezioni, per esempio:

- **header:** {{htmlelement("header")}}.
- **barra di navigazione:** {{htmlelement("nav")}}.
- **contenuto principale:** {{htmlelement("main")}}, con varie sottosezioni di contenuto rappresentate dagli elementi {{HTMLElement("article")}}, {{htmlelement("section")}}, e {{htmlelement("div")}}.
- **barra laterale:** {{htmlelement("aside")}}; spesso posizionata all'interno di {{htmlelement("main")}}.
- **footer:** {{htmlelement("footer")}}.

### Apprendimento attivo: esplorare il codice per il nostro esempio

Il nostro esempio visto sopra è rappresentato dal seguente codice (puoi anche [trovare l'esempio nel nostro repository GitHub](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/document_and_website_structure/index.html)). Ti invitiamo a guardare l'esempio sopra, e poi a esaminare l'elenco qui sotto per vedere quali parti compongono quale sezione del visivo.

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

Prenditi del tempo per esaminare il codice e capirlo — i commenti all'interno del codice dovrebbero anche aiutarti a comprenderlo. Non ti chiediamo di fare molto altro in questo articolo, perché la chiave per comprendere il layout di un documento è scrivere una solida struttura HTML, e poi disporla con il CSS. Aspetteremo finché non inizierai a studiare il layout CSS come parte del capitolo CSS.

## Elementi di layout HTML in dettaglio

È bene capire il significato generale di tutti gli elementi di sezione HTML nel dettaglio — questo è qualcosa su cui lavorerai gradualmente man mano che inizi a ottenere più esperienza nello sviluppo web. Puoi trovare molti dettagli leggendo la nostra [riferimento sugli elementi HTML](/it/docs/Web/HTML/Reference/Elements). Per ora, queste sono le definizioni principali che dovresti cercare di comprendere:

- {{HTMLElement('main')}} è per il contenuto _unico per questa pagina._ Usa `<main>` solo _una volta_ per pagina, e mettilo direttamente all'interno di {{HTMLElement('body')}}. Idealmente non dovrebbe essere annidato all'interno di altri elementi.
- {{HTMLElement('article')}} racchiude un blocco di contenuto correlato che ha senso da solo senza il resto della pagina (ad esempio, un singolo post del blog).
- {{HTMLElement('section')}} è simile a `<article>`, ma è più per raggruppare insieme una singola parte della pagina che costituisce un singolo pezzo di funzionalità (ad esempio, una mini mappa, o un gruppo di titoli di articoli e riassunti), o un tema. È considerata una buona pratica iniziare ogni sezione con una [intestazione](/it/docs/Learn_web_development/Core/Structuring_content/Headings_and_paragraphs); nota anche che puoi dividere le `<article>` in diverse `<section>`, o le `<section>` in diverse `<article>`, a seconda del contesto.
- {{HTMLElement('aside')}} contiene contenuto che non è direttamente correlato al contenuto principale ma può fornire ulteriori informazioni indirettamente correlate (voci di glossario, biografia dell'autore, link correlati, ecc.).
- {{HTMLElement('header')}} rappresenta un gruppo di contenuto introduttivo. Se è un figlio di {{HTMLElement('body')}} definisce l'intestazione globale di una pagina web, ma se è un figlio di un {{HTMLElement('article')}} o {{HTMLElement('section')}} definisce un'intestazione specifica per quella sezione (cerca di non confonderlo con [titoli e intestazioni](/it/docs/Learn_web_development/Core/Structuring_content/Webpage_metadata#adding_a_title)).
- {{HTMLElement('nav')}} contiene la funzionalità di navigazione principale per la pagina. Link secondari, ecc., non andrebbero nella navigazione.
- {{HTMLElement('footer')}} rappresenta un gruppo di contenuto finale per una pagina.

Ognuno degli elementi menzionati può essere cliccato per leggere l'articolo corrispondente nella sezione "Riferimento sugli elementi HTML", fornendo più dettagli su ciascuno.

### Involucri non semantici

A volte ti imbatterai in una situazione in cui non riesci a trovare un elemento semantico ideale per raggruppare alcuni elementi insieme o avvolgere del contenuto. Talvolta potresti voler semplicemente raggruppare un set di elementi insieme per influenzarli tutti come un'unica entità con qualche {{Glossary("CSS", "CSS")}} o {{Glossary("JavaScript", "JavaScript")}}. Per casi come questi, l'HTML fornisce gli elementi {{HTMLElement("div")}} e {{HTMLElement("span")}}. Dovresti usarli preferibilmente con un appropriato attributo [`class`](/it/docs/Web/HTML/Reference/Global_attributes/class), per fornire loro una sorta di etichetta in modo che possano essere facilmente bersagliati.

{{HTMLElement("span")}} è un elemento non semantico in linea, che dovresti usare solo se non riesci a pensare ad un miglior elemento di testo semantico per avvolgere il tuo contenuto, o non vuoi aggiungere alcun significato specifico. Per esempio:

```html
<p>
  The King walked drunkenly back to his room at 01:00, the beer doing nothing to
  aid him as he staggered through the door.
  <span class="editor-note">
    [Editor's note: At this point in the play, the lights should be down low].
  </span>
</p>
```

In questo caso, la nota dell'editore dovrebbe semplicemente fornire ulteriore direzione per il regista dell'opera; non dovrebbe avere un significato semantico aggiuntivo. Per gli utenti vedenti, il CSS potrebbe essere usato per distanziare leggermente la nota dal testo principale.

{{HTMLElement("div")}} è un elemento non semantico a livello di blocco, che dovresti usare solo se non riesci a pensare a un miglior elemento di blocco semantico da usare, o non vuoi aggiungere alcun significato specifico. Per esempio, immagina un widget di un carrello della spesa che potresti scegliere di aprire in qualsiasi momento durante la tua permanenza in un sito di e-commerce:

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

Questo non è propriamente un `<aside>`, poiché non si riferisce necessariamente al contenuto principale della pagina (lo vuoi visibile da ovunque). Non giustifica nemmeno particolarmente l'uso di un `<section>`, poiché non fa parte del contenuto principale della pagina. Quindi un `<div>` va bene in questo caso. Abbiamo incluso un'intestazione come indicatore per aiutare gli utenti dei lettori di schermo a trovarlo.

> [!WARNING]
> I Div sono così convenienti da usare che è facile abusarne. Poiché non hanno valore semantico, appesantiscono solo il tuo codice HTML. Fai attenzione ad usarli solo quando non c'è una soluzione semantica migliore e prova a ridurre il loro utilizzo al minimo altrimenti avrai difficoltà ad aggiornare e mantenere i tuoi documenti.

### Interruzioni di riga e regole orizzontali

Due elementi che utilizzerai occasionalmente e che vorrai conoscere sono {{htmlelement("br")}} e {{htmlelement("hr")}}.

#### \<br>: l'elemento di interruzione di riga

`<br>` crea un'interruzione di riga in un paragrafo; è l'unico modo per forzare una struttura rigida in una situazione in cui vuoi una serie di linee brevi fisse, come in un indirizzo postale o in una poesia. Per esempio:

```html
<p>
  There once was a man named O'Dell<br />
  Who loved to write HTML<br />
  But his structure was bad, his semantics were sad<br />
  and his markup didn't read very well.
</p>
```

Senza gli elementi `<br>`, il paragrafo verrebbe reso in un'unica lunga riga (come abbiamo detto in precedenza nel corso, [l'HTML ignora la maggior parte degli spazi bianchi](/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax#whitespace_in_html)); con gli elementi `<br>` nel codice, il markup viene reso così:

{{EmbedLiveSample('br_the_line_break_element', '100%', 150)}}

#### \<hr>: l'elemento di pausa tematica

Gli elementi `<hr>` creano una linea orizzontale nel documento che denota un cambiamento tematico nel testo (come un cambiamento di argomento o scena). Visivamente appare solo come una linea orizzontale. Come esempio:

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

Verrà reso come segue:

{{EmbedLiveSample('hr_the_thematic_break_element', '100%', '185px')}}

## Pianificare un sito web semplice

Una volta pianificata la struttura di una semplice pagina web, il passo logico successivo è cercare di capire quale contenuto vuoi inserire su un intero sito web, quali pagine ti servono e come dovrebbero essere organizzate e collegate tra loro per offrire la migliore esperienza utente possibile. Questo è chiamato {{Glossary("Information_architecture", "Architettura delle informazioni")}}. In un sito web grande e complesso, molta pianificazione può andare in questo processo, ma per un sito web semplice di poche pagine, questo può essere abbastanza semplice, e divertente!

1. Tieni presente che avrai alcuni elementi comuni alla maggior parte (se non a tutte) le pagine — come il menu di navigazione e il contenuto del footer. Se il tuo sito è per un'azienda, ad esempio, è una buona idea avere le tue informazioni di contatto disponibili nel footer su ogni pagina. Annota cosa vuoi avere come comune per ogni pagina.![le caratteristiche comuni del sito di viaggi da inserire su ogni pagina: titolo e logo, contatti, copyright, termini e condizioni, selettore di lingua, politica di accessibilità](common-features.png)
2. Successivamente, fai un disegno approssimativo di quella che potrebbe essere la struttura di ciascuna pagina (potrebbe assomigliare al nostro esempio di sito web semplice sopra). Annota cosa sarà ciascun blocco.![Un semplice diagramma di una struttura di esempio del sito, con un'intestazione, area di contenuto principale, due barre laterali opzionali e un footer](site-structure.png)
3. Ora, fai un brainstorming su tutto l'altro contenuto (non comune ad ogni pagina) che vuoi avere sul tuo sito web — scrivi un lungo elenco.![Un lungo elenco di tutte le caratteristiche che potremmo inserire sul nostro sito di viaggi, dalla ricerca, a offerte speciali e info specifiche per paese](feature-list.png)
4. Successivamente, prova a ordinare tutti questi elementi di contenuto in gruppi, per avere un'idea di quali parti potrebbero vivere insieme su diverse pagine. Questo è molto simile a una tecnica chiamata {{Glossary("Card_sorting", "Ordinamento delle schede")}}.![Gli elementi che dovrebbero apparire su un sito di vacanze ordinati in 5 categorie: Ricerca, Offerte speciali, Info specifiche per paese, Risultati di ricerca e Acquisti](card-sorting.png)
5. Ora prova a disegnare uno schema di sito approssimativo — crea un cerchio per ogni pagina sul tuo sito, e traccia delle linee per mostrare il flusso tipico tra le pagine. La homepage probabilmente sarà al centro e si collegherà a molte se non a tutte le altre; la maggior parte delle pagine in un sito piccolo dovrebbe essere disponibile dalla navigazione principale, anche se ci sono eccezioni. Potresti anche voler includere note su come potrebbero essere presentate le cose.![Una mappa del sito che mostra la homepage, pagina del paese, risultati della ricerca, pagina delle offerte speciali, pagamento e pagina di acquisto](site-map.png)

### Apprendimento attivo: crea il tuo schema del sito

Prova a realizzare l'esercizio sopra per un sito web di tua creazione. Di cosa ti piacerebbe realizzare un sito?

> [!NOTE]
> Salva il tuo lavoro da qualche parte; potresti averne bisogno più avanti.

## Sommario

A questo punto, dovresti avere un'idea migliore di come strutturare una pagina web/sito. Nel prossimo articolo di questo modulo, esamineremo alcune tecniche avanzate di testo.

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Lists", "Learn_web_development/Core/Structuring_content/Advanced_text_features", "Learn_web_development/Core/Structuring_content")}}
