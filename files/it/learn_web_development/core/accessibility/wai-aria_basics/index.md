---
title: Nozioni di base su WAI-ARIA
short-title: WAI-ARIA
slug: Learn_web_development/Core/Accessibility/WAI-ARIA_basics
l10n:
  sourceCommit: a1ac64fa4da965d2a152f08221b1a9aed638fd16
---

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/CSS_and_JavaScript","Learn_web_development/Core/Accessibility/Multimedia", "Learn_web_development/Core/Accessibility")}}

A seguito dell'articolo precedente, a volte creare controlli UI complessi che coinvolgono HTML non semantico e contenuti dinamici aggiornati con JavaScript può risultare difficile. WAI-ARIA è una tecnologia che può aiutare in tali problemi aggiungendo ulteriori semantiche che i browser e le tecnologie assistive possono riconoscere e utilizzare per informare gli utenti su cosa sta accadendo. Qui mostreremo come utilizzarlo a un livello base per migliorare l'accessibilità.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Familiarità con <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e le migliori pratiche di accessibilità insegnate nelle lezioni precedenti nel modulo.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Lo scopo di WAI-ARIA — fornire semantica a HTML non semantico per far sì che gli utenti di tecnologie assistive possano comprendere le interfacce presentate.</li>
          <li>La sintassi di base — ruoli, proprietà e stati.</li>
          <li>Punti di riferimento e segnaletica.</li>
          <li>Miglioramento dell'accessibilità tramite tastiera.</li>
          <li>Annuncio degli aggiornamenti di contenuti dinamici con le regioni dal vivo.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Cos'è WAI-ARIA?

Iniziamo col vedere cos'è WAI-ARIA e cosa può fare per noi.

### Un insieme completamente nuovo di problemi

Man mano che le app web diventavano più complesse e dinamiche, iniziava ad emergere un nuovo insieme di funzionalità e problemi di accessibilità.

Ad esempio, HTML ha introdotto una serie di elementi semantici per definire funzionalità comuni della pagina ({{htmlelement("nav")}}, {{htmlelement("footer")}}, ecc.). Prima che queste fossero disponibili, gli sviluppatori utilizzavano {{htmlelement("div")}} con ID o classi, ad esempio `<div class="nav">`, ma ciò era problematico, poiché non c'era un modo semplice per trovare facilmente una funzione specifica della pagina come la navigazione principale in modo programmatico.

La soluzione iniziale era aggiungere uno o più link nascosti in cima alla pagina per collegarsi alla navigazione (o altro), ad esempio:

```html
<a href="#hidden" class="hidden">Skip to navigation</a>
```

Ma questo non è ancora molto preciso e può essere utilizzato solo quando il lettore dello schermo sta leggendo dall'inizio della pagina.

Un altro esempio è costituito dalle app che hanno iniziato a utilizzare controlli complessi come i selettori di data per scegliere date, slider per scegliere valori, ecc. HTML fornisce tipi di input speciali per renderizzare tali controlli:

```html
<input type="date" /> <input type="range" />
```

Questi originariamente non erano ben supportati ed era, e in una certa misura è ancora, difficile stilizzarli, portando designer e sviluppatori a optare per soluzioni personalizzate. Invece di utilizzare queste funzionalità native, alcuni sviluppatori fanno affidamento su librerie JavaScript che generano tali controlli come una serie di {{htmlelement("div")}} annidati che vengono poi stilizzati tramite CSS e controllati tramite JavaScript.

Il problema è che visivamente funzionano, ma i lettori di schermo non riescono a comprenderli affatto, e i loro utenti ricevono solo un insieme di elementi senza semantica per descrivere cosa significano.

### Introduzione a WAI-ARIA

[WAI-ARIA](https://www.w3.org/TR/wai-aria/) (Web Accessibility Initiative - Accessible Rich Internet Applications) è una specifica scritta dal W3C, che definisce un insieme di attributi HTML aggiuntivi che possono essere applicati agli elementi per fornire semantica aggiuntiva e migliorare l'accessibilità ovunque manchi. Ci sono tre caratteristiche principali definite nella specifica:

- [Ruoli](/it/docs/Web/Accessibility/ARIA/Reference/Roles)
  - : Questi definiscono cosa un elemento è o fa. Molti di questi sono chiamati ruoli di punto di riferimento, che in gran parte duplicano il valore semantico degli elementi strutturali, come `role="navigation"` ({{htmlelement("nav")}}), `role="banner"` (document {{htmlelement("header")}}), `role="complementary"` ({{htmlelement("aside")}}) o, `role="search"` ({{htmlelement("search")}}). Alcuni altri ruoli descrivono diverse strutture di pagina che non hanno elementi che corrispondono a tali ruoli, come `role="tablist"`, e `role="tabpanel"`, che si trovano comunemente nelle UI.
- Proprietà
  - : Queste definiscono le proprietà degli elementi, che possono essere utilizzate per dare loro un significato oppure semantica extra. Ad esempio, `aria-required="true"` specifica che un input di un modulo deve essere compilato per essere valido, mentre `aria-labelledby="label"` permette di assegnare un ID a un elemento, poi fare riferimento a esso come l'etichetta per qualsiasi altra cosa sulla pagina, inclusi più elementi, il che non è possibile utilizzando `<label for="input">`. Ad esempio, è possibile utilizzare `aria-labelledby` per specificare che una descrizione di tasto contenuta in un {{htmlelement("div")}} è l'etichetta per più celle di una tabella, oppure potrebbe essere utilizzato come alternativa al testo alternativo di un'immagine — specificare informazioni esistenti sulla pagina come testo alt di un'immagine, piuttosto che doverle ripetere all'interno dell'attributo `alt`. È possibile vedere un esempio di questo nella [Guida](https://en.wikipedia.org/wiki/Guide) al [Text alternatives](/it/docs/Learn_web_development/Core/Accessibility/HTML#text_alternatives).
- Stati
  - : Proprietà speciali che definiscono le condizioni attuali degli elementi, come `aria-disabled="true"`, che specifica a un lettore di schermo che un input di un modulo è attualmente disabilitato. Gli stati differiscono dalle proprietà nel fatto che le proprietà non cambiano durante il ciclo di vita di un'app, mentre gli stati possono cambiare, generalmente in modo programmatico tramite JavaScript.

Un punto importante sugli attributi WAI-ARIA è che non influenzano nulla sulla pagina web, tranne le informazioni esposte dalle API di accessibilità del browser (dove i lettori di schermo prendono le loro informazioni). WAI-ARIA non influisce sulla struttura della pagina web, sul DOM, ecc., sebbene gli attributi possano essere utili per selezionare degli elementi tramite CSS.

> [!NOTE]
> È possibile trovare un elenco utile di tutti i ruoli ARIA e i loro usi, con link a ulteriori informazioni, nella specifica WAI-ARIA — vedere [Definition of Roles](https://www.w3.org/TR/wai-aria-1.1/#role_definitions) su questo sito — vedere [Ruoli ARIA](/it/docs/Web/Accessibility/ARIA/Reference/Roles).
>
> La specifica contiene anche un elenco di tutte le proprietà e gli stati, con link a ulteriori informazioni — vedere [Definitions of States and Properties (tutti gli attributi `aria-*`)](https://www.w3.org/TR/wai-aria-1.1/#state_prop_def).

### Dove è supportato WAI-ARIA?

Questa non è una domanda facile a cui rispondere. È difficile trovare una risorsa conclusiva che dichiari quali caratteristiche di WAI-ARIA sono supportate, e dove, perché:

1. Ci sono molte caratteristiche nella specifica WAI-ARIA.
2. Ci sono molte combinazioni di sistemi operativi, browser e lettori di schermo da considerare.

Questo ultimo punto è fondamentale — Per utilizzare un lettore di schermo in primo luogo, il tuo sistema operativo deve eseguire browser che abbiano le necessarie API di accessibilità attive per esporre le informazioni di cui i lettori di schermo hanno bisogno per svolgere il loro lavoro. La maggior parte dei sistemi operativi popolari ha uno o due browser in cui i lettori di schermo possono lavorare. Il Paciello Group ha un post abbastanza aggiornato che fornisce dati su questo — vedere [Rough Guide: browsers, operating systems and screen reader support updated](https://www.tpgi.com/rough-guide-browsers-operating-systems-and-screen-reader-support-updated/).

Successivamente, è necessario preoccuparsi se i browser in questione supportano le funzionalità ARIA e le espongono tramite le loro API, ma anche se i lettori di schermo riconoscono tali informazioni e le presentano ai loro utenti in modo utile.

1. Il supporto del browser è quasi universale.
2. Il supporto dei lettori di schermo per le funzionalità ARIA non è ancora a questo livello, ma i lettori di schermo più popolari ci stanno arrivando. Puoi avere un'idea dei livelli di supporto guardando l'articolo di Powermapper [WAI-ARIA Screen reader compatibility](https://www.powermapper.com/tests/screen-readers/aria/).

In questo articolo, non tenteremo di coprire tutte le funzionalità di WAI-ARIA, e i loro dettagli di supporto esatti. Invece, copriremo le funzionalità WAI-ARIA più critiche che devi conoscere; se non menzioniamo dettagli di supporto, puoi presumere che la funzionalità sia ben supportata. Metteremo in chiaro tutte le eccezioni a questo.

> [!NOTE]
> Alcune librerie JavaScript supportano WAI-ARIA, il che significa che quando generano funzionalità UI come controlli di modulo complessi, aggiungono attributi ARIA per migliorare l'accessibilità di tali funzionalità. Se stai cercando una soluzione JavaScript di terze parti per lo sviluppo rapido di UI, dovresti sicuramente considerare l'accessibilità dei suoi widget UI come un fattore importante quando fai la tua scelta. Buoni esempi sono jQuery UI (vedere [About jQuery UI: Deep accessibility support](https://jqueryui.com/about/#deep-accessibility-support)), [ExtJS](https://www.sencha.com/products/extjs/), e [Dojo/Dijit](https://dojotoolkit.org/reference-guide/1.10/dijit/a11y/statement.html).

### Quando dovresti usare WAI-ARIA?

Abbiamo parlato di alcuni dei problemi che hanno portato alla creazione di WAI-ARIA in precedenza, ma essenzialmente, ci sono quattro aree principali in cui WAI-ARIA è utile:

- Segnalazioni/Punti di riferimento
  - : I valori dell'attributo [`role`](/it/docs/Web/Accessibility/ARIA/Reference/Roles) di ARIA possono agire come punti di riferimento che replicano la semantica degli elementi HTML (ad esempio, {{htmlelement("nav")}}), oppure vanno oltre la semantica HTML per fornire segnaletica a diverse aree funzionali, ad esempio `search`, `tablist`, `tab`, `listbox`, ecc.
- Aggiornamenti di contenuti dinamici
  - : I lettori di schermo tendono ad avere difficoltà nel riportare contenuti che cambiano costantemente; con ARIA possiamo usare `aria-live` per informare gli utenti di lettori di schermo quando un'area di contenuto viene aggiornata in modo dinamico: ad esempio, da JavaScript nella pagina [che preleva nuovo contenuto dal server e aggiorna il DOM](/it/docs/Learn_web_development/Core/Scripting/Network_requests).
- Migliorare l'accessibilità tramite tastiera
  - : Vi sono elementi HTML integrati che hanno accessibilità tramite tastiera nativa; quando vengono utilizzati altri elementi insieme a JavaScript per simulare interazioni simili, l'accessibilità tramite tastiera e la rendicontazione dei lettori di schermo ne risente. Laddove ciò non è evitabile, WAI-ARIA fornisce un mezzo per consentire ad altri elementi di ricevere il focus (utilizzando `tabindex`).
- Accessibilità dei controlli non semantici
  - : Quando una serie di `<div>` annidati insieme a CSS/JavaScript viene utilizzata per creare una funzionalità UI complessa, oppure un controllo nativo viene notevolmente migliorato/cambiato tramite JavaScript, l'accessibilità può peggiorare — gli utenti dei lettori di schermo avranno difficoltà a capire cosa fa la funzionalità se non ci sono semantiche o altri indizi. In queste situazioni, ARIA può aiutare a fornire ciò che manca con una combinazione di ruoli come `button`, `listbox`, o `tablist`, e proprietà come `aria-required` o `aria-posinset` per fornire ulteriori indizi sulle funzionalità.

#### Dovresti utilizzare WAI-ARIA solo quando ne hai bisogno!

Utilizzare gli elementi HTML corretti fornisce implicitamente i ruoli necessari e dovresti _sempre_ usare [funzionalità HTML native](/it/docs/Learn_web_development/Core/Accessibility/HTML) per fornire le semantiche richieste dai lettori di schermo per informare i loro utenti su cosa sta accadendo. A volte ciò non è possibile, sia perché hai un controllo limitato sul codice, sia perché stai creando qualcosa di complesso che non ha un elemento HTML facile da implementare. In questi casi, WAI-ARIA può essere uno strumento prezioso per migliorare l'accessibilità.

Ma ancora una volta, usalo solo quando necessario!

> [!NOTE]
> Inoltre, cerca di assicurarti di testare il tuo sito con una varietà di _utenti reali_ — persone non disabili, persone che utilizzano lettori di schermo, persone che utilizzano la navigazione tramite tastiera, ecc. Avranno maggiori intuizioni di te su quanto bene funzioni.

## Implementazioni pratiche di WAI-ARIA

Nella sezione successiva, esamineremo le quattro aree in maggior dettaglio, insieme a esempi pratici. Prima di continuare, dovresti impostare un ambiente di test con un lettore di schermo, in modo da poter testare alcuni degli esempi mentre procedi.

Vedi la nostra sezione su [testare i lettori di schermo](/it/docs/Learn_web_development/Core/Accessibility/Tooling#screen_readers) per ulteriori informazioni.

### Segnalazioni/Punti di riferimento

WAI-ARIA aggiunge l'[attributo `role`](https://www.w3.org/TR/wai-aria-1.1/#role_definitions) ai browser, che consente di aggiungere un valore semantico extra agli elementi sul tuo sito dove necessario. La prima area principale in cui questo è utile è fornire informazioni per i lettori di schermo in modo che i loro utenti possano trovare elementi comuni della pagina. Questo esempio ha la seguente struttura:

```html live-sample___aria-website-no-roles
<header>
  <h1>Header</h1>

  <!-- Even is it's not mandatory, it's common practice to put the main navigation menu within the main header -->

  <nav>
    <ul>
      <li><a href="#">Home</a></li>
      <li><a href="#">Team</a></li>
      <li><a href="#">Projects</a></li>
      <li><a href="#">Contact</a></li>
    </ul>

    <!-- A Search form is another common non-linear way to navigate through a website. -->

    <form>
      <input type="search" name="q" placeholder="Search query" />
      <input type="submit" value="Go!" />
    </form>
  </nav>
</header>

<!-- Here is our page's main content -->
<main>
  <!-- It contains an article -->
  <article>
    <h2>Article heading</h2>

    <p>
      Lorem ipsum dolor sit amet, consectetur adipisicing elit. Donec a diam
      lectus. Set sit amet ipsum mauris. Maecenas congue ligula as quam viverra
      nec consectetur ant hendrerit. Donec et mollis dolor. Praesent et diam
      eget libero egestas mattis sit amet vitae augue. Nam tincidunt congue
      enim, ut porta lorem lacinia consectetur.
    </p>

    <h3>subsection</h3>

    <p>
      Donec ut librero sed accu vehicula ultricies a non tortor. Lorem ipsum
      dolor sit amet, consectetur adipisicing elit. Aenean ut gravida lorem. Ut
      turpis felis, pulvinar a semper sed, adipiscing id dolor.
    </p>
  </article>

  <!-- the aside content can also be nested within the main content -->
  <aside>
    <h2>Related</h2>

    <ul>
      <li><a href="#">Oh I do like to be beside the seaside</a></li>
      <li><a href="#">Oh I do like to be beside the sea</a></li>
      <li><a href="#">Although in the North of England</a></li>
      <li><a href="#">It never stops raining</a></li>
      <li><a href="#">Oh well...</a></li>
    </ul>
  </aside>
</main>

<!-- And here is our main footer that is used across all the pages of our website -->

<footer>
  <p>©Copyright 2050 by nobody. All rights reversed.</p>
</footer>
```

```css hidden live-sample___aria-website-no-roles
/* || General setup */

html,
body {
  margin: 0;
  padding: 0;
}

html {
  font-size: 10px;
  background-color: #a9a9a9;
}

body {
  width: max(70vw, 90%);
  margin: 0 auto;
  padding: 0 10px;
  display: flex;
  flex-direction: column;
}

/* || typography */

h1,
h2,
h3 {
  font-family: "Sonsie One", cursive;
  color: #2a2a2a;
}

p,
input,
li {
  font-family: "Open Sans Condensed", sans-serif;
  color: #2a2a2a;
}

h1 {
  font-size: 4rem;
  text-align: center;
  color: white;
  text-shadow: 2px 2px 10px black;
}

h2 {
  font-size: 3rem;
  text-align: center;
}

h3 {
  font-size: 2.2rem;
}

p,
li {
  font-size: 1.6rem;
  line-height: 1.5;
}

/* || header layout */

header {
  margin-bottom: 10px;
}

nav,
article,
aside,
footer {
  background-color: white;
  padding: 1%;
}

nav {
  background-color: ff80ff;
  display: flex;
  gap: 2vw;
  @media (width <= 650px) {
    flex-direction: column;
  }
}

nav ul {
  padding: 0;
  list-style-type: none;
  flex: 2;
  display: flex;
  gap: 2vw;
}

nav li {
  display: inline;
  text-align: center;
}

nav a {
  display: inline-block;
  font-size: 2rem;
  text-transform: uppercase;
  text-decoration: none;
  color: black;
}

nav form {
  flex: 1;
  display: flex;
  align-items: center;
  height: 100%;
}

input {
  font-size: 1.6rem;
  height: 32px;
}

input[type="search"] {
  flex: 3;
}

input[type="submit"] {
  flex: 1;
  margin-left: 1rem;
  background: #333;
  border: 0;
  color: white;
}

/* || main layout */

main {
  display: flex;
  gap: 2vw;
  @media (width <= 650px) {
    flex-direction: column;
  }
}

article {
  flex: 4;
}

aside {
  flex: 1;
  background-color: #ff80ff;
}

aside li {
  padding-bottom: 10px;
}

footer {
  margin-top: 10px;
}
```

{{EmbedLiveSample("aria-website-no-roles", "100", "850")}}

Se provi a testare l'esempio con un lettore di schermo in un browser moderno, otterrai già delle informazioni utili. Ad esempio, VoiceOver ti dà quanto segue:

- Sul `<header>` — "banner, 2 elementi" (contiene un'intestazione e il `<nav>`).
- Sul `<nav>` — "navigazione 2 elementi" (contiene un elenco e un modulo).
- Sul `<main>` — "principale 2 elementi" (contiene un articolo e una sezione laterale).
- Sul `<aside>` — "complementare 2 elementi" (contiene un'intestazione e un elenco).
- Sul campo di ricerca del modulo — "Query di ricerca, inserzione all'inizio del testo".
- Sul `<footer>` — "footer 1 elemento".

Se vai al menu dei punti di riferimento di VoiceOver (accessibile usando VoiceOver key + U e poi i tasti cursore per sfogliare le voci del menu), vedrai che la maggior parte degli elementi è elencata in modo che possano essere accessibili rapidamente.

![Menu di VoiceOver del Mac per una rapida accessibilità. Intestazione punti di riferimento e elenco di punti di riferimento inclusi banner, navigazione, principale e complementare.](landmarks-list.png)

Tuttavia, potremmo fare meglio qui. Il modulo di ricerca è un punto di riferimento davvero importante che le persone vorranno trovare, ma non è elencato nel menu dei punti di riferimento o considerato come un punto di riferimento notevole oltre al vero input chiamato come input di ricerca (`<input type="search">`).

Potremmo migliorarlo usando il ruolo ARIA `role="search"`, ma usare l'elemento {{htmlelement("search")}} implicitamente attribuisce quel ruolo al modulo.

```html live-sample___aria-website-roles
<header>
  <h1>Header</h1>

  <!-- Even is it's not mandatory, it's common practice to put the main navigation menu within the main header -->

  <nav>
    <ul>
      <li><a href="#">Home</a></li>
      <li><a href="#">Our team</a></li>
      <li><a href="#">Projects</a></li>
      <li><a href="#">Contact</a></li>
    </ul>

    <!-- A Search form is another common non-linear way to navigate through a website. -->

    <search>
      <form>
        <input
          type="search"
          name="q"
          placeholder="Search query"
          aria-label="Search through site content" />
        <input type="submit" value="Go!" />
      </form>
    </search>
  </nav>
</header>

<!-- Here is our page's main content -->
<main>
  <!-- It contains an article -->
  <article>
    <h2>Article heading</h2>

    <p>
      Lorem ipsum dolor sit amet, consectetur adipisicing elit. Donec a diam
      lectus. Set sit amet ipsum mauris. Maecenas congue ligula as quam viverra
      nec consectetur ant hendrerit. Donec et mollis dolor. Praesent et diam
      eget libero egestas mattis sit amet vitae augue. Nam tincidunt congue
      enim, ut porta lorem lacinia consectetur.
    </p>

    <h3>subsection</h3>

    <p>
      Donec ut librero sed accu vehicula ultricies a non tortor. Lorem ipsum
      dolor sit amet, consectetur adipisicing elit. Aenean ut gravida lorem. Ut
      turpis felis, pulvinar a semper sed, adipiscing id dolor.
    </p>

    <p>
      Pelientesque auctor nisi id magna consequat sagittis. Curabitur dapibus,
      enim sit amet elit pharetra tincidunt feugiat nist imperdiet. Ut convallis
      libero in urna ultrices accumsan. Donec sed odio eros.
    </p>
  </article>

  <!-- the aside content can also be nested within the main content -->
  <aside>
    <h2>Related</h2>
    <ul>
      <li><a href="#">Oh I do like to be beside the seaside</a></li>
      <li><a href="#">Oh I do like to be beside the sea</a></li>
      <li><a href="#">Although in the North of England</a></li>
      <li><a href="#">It never stops raining</a></li>
      <li><a href="#">Oh well...</a></li>
    </ul>
  </aside>
</main>

<!-- And here is our main footer that is used across all the pages of our website -->

<footer>
  <p>©Copyright 2050 by nobody. All rights reversed.</p>
</footer>
```

```css hidden live-sample___aria-website-roles
/* || General setup */

html,
body {
  margin: 0;
  padding: 0;
}

html {
  font-size: 10px;
  background-color: #a9a9a9;
}

body {
  width: max(70vw, 90%);
  margin: 0 auto;
  padding: 0 10px;
  display: flex;
  flex-direction: column;
}

/* || typography */

h1,
h2,
h3 {
  font-family: "Sonsie One", cursive;
  color: #2a2a2a;
}

p,
input,
li {
  font-family: "Open Sans Condensed", sans-serif;
  color: #2a2a2a;
}

h1 {
  font-size: 4rem;
  text-align: center;
  color: white;
  text-shadow: 2px 2px 10px black;
}

h2 {
  font-size: 3rem;
  text-align: center;
}

h3 {
  font-size: 2.2rem;
}

p,
li {
  font-size: 1.6rem;
  line-height: 1.5;
}

/* || header layout */

header {
  margin-bottom: 10px;
}

nav,
article,
aside,
footer {
  background-color: white;
  padding: 1%;
}

nav {
  background-color: ff80ff;
  display: flex;
  gap: 2vw;
  @media (width <= 650px) {
    flex-direction: column;
  }
}

nav ul {
  padding: 0;
  list-style-type: none;
  flex: 2;
  display: flex;
  gap: 2vw;
}

nav li {
  display: inline;
  text-align: center;
}

nav a {
  display: inline-block;
  font-size: 2rem;
  text-transform: uppercase;
  text-decoration: none;
  color: black;
}

nav form {
  flex: 1;
  display: flex;
  align-items: center;
  height: 100%;
}

input {
  font-size: 1.6rem;
  height: 32px;
}

input[type="search"] {
  flex: 3;
}

input[type="submit"] {
  flex: 1;
  margin-left: 1rem;
  background: #333;
  border: 0;
  color: white;
}

/* || main layout */

main {
  display: flex;
  gap: 2vw;
  @media (width <= 650px) {
    flex-direction: column;
  }
}

article {
  flex: 4;
}

aside {
  flex: 1;
  background-color: #ff80ff;
}

aside li {
  padding-bottom: 10px;
}

footer {
  margin-top: 10px;
}
```

{{EmbedLiveSample("aria-website-roles", "100", "850")}}

Soprattutto, abbiamo utilizzato HTML semantico che conferisce significato e ruoli alla struttura della pagina senza aggiungere inutili attributi [`role`](/it/docs/Web/Accessibility/ARIA/Reference/Roles) alla nostra struttura HTML, che ha una struttura simile a questa:

```html
<header>
  <h1>…</h1>
  <nav>
    <ul>
      …
    </ul>
    <search>
      <form>
        <!-- search form -->
      </form>
    </search>
  </nav>
</header>

<main>
  <article>…</article>
  <aside>…</aside>
</main>

<footer>…</footer>
```

Abbiamo anche fornito una funzionalità bonus in questo esempio — l'elemento {{htmlelement("input")}} è stato assegnato l'attributo [`aria-label`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-label), che dà una descrizione da leggere da un lettore di schermo, anche se non abbiamo incluso un elemento {{htmlelement("label")}}. In casi come questi, è molto utile — un modulo di ricerca come questo è una funzionalità comune e facilmente riconoscibile, e aggiungere un'etichetta visiva rovinerebbe il design della pagina.

```html
<input
  type="search"
  name="q"
  placeholder="Search query"
  aria-label="Search through site content" />
```

Ora se usiamo VoiceOver per esaminare questo esempio, otteniamo alcuni miglioramenti:

- Il modulo di ricerca è chiamato come un elemento separato, sia durante la navigazione nella pagina sia nel menu dei punti di riferimento.
- Il testo dell'etichetta contenuta nell'attributo `aria-label` viene letto quando l'input del modulo è evidenziato.

Se hai bisogno di supportare browser più vecchi come IE8; vale la pena includere ruoli ARIA a tale scopo. E se per qualche motivo il tuo sito è costruito utilizzando solo `<div>`, dovresti sicuramente includere i ruoli ARIA per fornire queste semantiche molto necessarie!

Vedrai molte altre informazioni su queste semantiche e il potere delle proprietà/attributi ARIA qui sotto, specialmente nella sezione [Accessibilità dei controlli non semantici](#accessibilità_dei_controlli_non_semantici). Per ora, però, vediamo come ARIA può aiutare con gli aggiornamenti dei contenuti dinamici.

### Aggiornamenti dei contenuti dinamici

I contenuti caricati nel DOM possono essere facilmente accessibili utilizzando un lettore di schermo, da contenuti testuali a testo alternativo allegato alle immagini. I siti web statici tradizionali con contenuti principalmente testuali sono quindi facili da rendere accessibili per le persone con disabilità visive.

Il problema è che le app web moderne spesso non sono solo testi statici — spesso aggiornano parti della pagina prelevando nuovo contenuto dal server (in questo esempio stiamo usando un array statico di citazioni) e aggiornando il DOM. Queste sono talvolta chiamate **regioni dal vivo**.

```html live-sample___aria-no-live
<section>
  <h1>Random quote</h1>
  <blockquote>
    <p></p>
  </blockquote>
</section>
```

```css live-sample___aria-no-live
html {
  font-family: sans-serif;
}

h1 {
  letter-spacing: 2px;
}

p {
  line-height: 1.6;
}

section {
  padding: 10px;
  width: calc(100% - 20px);
  background: #666;
  text-shadow: 1px 1px 1px black;
  color: white;
  min-height: 160px;
}
```

```js live-sample___aria-no-live
let quotes = [
  {
    quote:
      "Every child is an artist. The problem is how to remain an artist once he grows up.",
    author: "Pablo Picasso",
  },
  {
    quote:
      "You can never cross the ocean until you have the courage to lose sight of the shore.",
    author: "Christopher Columbus",
  },
  {
    quote:
      "I love deadlines. I love the whooshing noise they make as they go by.",
    author: "Douglas Adams",
  },
];
```

```js live-sample___aria-no-live
const quotePara = document.querySelector("section p");

window.setInterval(showQuote, 10000);

function showQuote() {
  let random = Math.floor(Math.random() * quotes.length);
  quotePara.textContent = `${quotes[random].quote} -- ${quotes[random].author}`;
}
```

{{EmbedLiveSample("aria-no-live", "100", "180")}}

Questo funziona bene, ma non è buono per l'accessibilità — l'aggiornamento del contenuto non viene rilevato dai lettori di schermo, quindi i loro utenti non saprebbero cosa sta succedendo. Questo è un esempio abbastanza banale, ma immagina se stessi creando una UI complessa con molti contenuti costantemente aggiornati, come una chat room, o un'interfaccia di un gioco strategico, o una visualizzazione di un carrello dello shopping live — sarebbe impossibile usare l'app in modo efficace senza qualche tipo di modo per allertare l'utente sugli aggiornamenti.

Fortunatamente, WAI-ARIA fornisce un meccanismo utile per fornire questi avvisi — la proprietà [`aria-live`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-live). L'applicazione di questa a un elemento fa in modo che i lettori di schermo leggano il contenuto aggiornato. Quanto urgentemente il contenuto è letto dipende dal valore dell'attributo:

- `off`
  - : Il valore predefinito. Gli aggiornamenti non dovrebbero essere annunciati.
- `polite`
  - : Gli aggiornamenti dovrebbero essere annunciati solo se l'utente è inattivo.
- `assertive`
  - : Gli aggiornamenti dovrebbero essere annunciati all'utente il prima possibile.

Qui aggiorniamo il tag di apertura `<section>` come segue:

```html
<section aria-live="assertive">…</section>
```

Questo farà sì che un lettore di schermo legga il contenuto man mano che viene aggiornato.

C'è un'altra considerazione qui — solo il pezzo di testo che si aggiorna viene letto. Potrebbe essere bello se leggessimo sempre anche l'intestazione, così l'utente può ricordare cosa viene letto. Per fare ciò, possiamo aggiungere la proprietà [`aria-atomic`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-atomic) alla sezione. Aggiorna nuovamente il tuo tag di apertura `<section>`, in questo modo:

```html
<section aria-live="assertive" aria-atomic="true">…</section>
```

L'attributo `aria-atomic="true"` indica ai lettori di schermo di leggere tutto il contenuto dell'elemento come un'unità atomica, non solo i pezzi che sono stati aggiornati.

```html live-sample___aria-live
<section aria-live="assertive" aria-atomic="true">
  <h1>Random quote</h1>
  <blockquote>
    <p></p>
  </blockquote>
</section>
```

```css live-sample___aria-live
html {
  font-family: sans-serif;
}

h1 {
  letter-spacing: 2px;
}

p {
  line-height: 1.6;
}

section {
  padding: 10px;
  width: calc(100% - 20px);
  background: #666;
  text-shadow: 1px 1px 1px black;
  color: white;
  min-height: 160px;
}
```

```js live-sample___aria-live
let quotes = [
  {
    quote:
      "Every child is an artist. The problem is how to remain an artist once he grows up.",
    author: "Pablo Picasso",
  },
  {
    quote:
      "You can never cross the ocean until you have the courage to lose sight of the shore.",
    author: "Christopher Columbus",
  },
  {
    quote:
      "I love deadlines. I love the whooshing noise they make as they go by.",
    author: "Douglas Adams",
  },
];
```

```js live-sample___aria-live
const quotePara = document.querySelector("section p");

window.setInterval(showQuote, 10000);

function showQuote() {
  let random = Math.floor(Math.random() * quotes.length);
  quotePara.textContent = `${quotes[random].quote} -- ${quotes[random].author}`;
}
```

{{EmbedLiveSample("aria-live", "100", "180")}}

> [!NOTE]
> La proprietà [`aria-relevant`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-relevant) è anche molto utile per controllare ciò che viene letto quando una regione dal vivo viene aggiornata. Puoi, ad esempio, fare in modo che siano letti solo i contenuti aggiunti o rimossi.

### Miglioramento dell'accessibilità tramite tastiera

Come discusso in diversi punti del modulo, uno dei punti di forza chiave di HTML rispetto all'accessibilità è l'accessibilità tramite tastiera integrata per funzionalità come pulsanti, controlli di modulo e collegamenti. In generale, puoi usare il tasto tab per muoverti tra i controlli, il tasto Invio/Ritorna per selezionare o attivare controlli, e occasionalmente altri controlli come necessario (ad esempio i tasti cursore su e giù per spostarti tra le opzioni in una `<select>` box).

Tuttavia, a volte finirai con scrivere codice che utilizza elementi non semantici come pulsanti (o altri tipi di controllo), o utilizza controlli focalizzabili per uno scopo non del tutto corretto. Potresti provare a correggere del codice errato che hai ereditato, o potresti costruire un qualche tipo di widget complesso che lo richiede.

In termini di rendere il codice non focalizzabile, focalizzabile, WAI-ARIA estende l'attributo `tabindex` con alcuni nuovi valori:

- `tabindex="0"` — come indicato sopra, questo valore consente agli elementi che normalmente non sono omologabili di diventare omologabili. Questo è il valore più utile di `tabindex`.
- `tabindex="-1"` — questo permette ad elementi che normalmente non sono omologabili di ricevere il focus in modo programmatico, ad esempio, tramite JavaScript, o come target di link.

Abbiamo discusso questo in dettaglio maggiore e mostrato un'implementazione tipica nel nostro articolo sull'accessibilità HTML — vedere [Ricostruire l'accessibilità tramite tastiera](/it/docs/Learn_web_development/Core/Accessibility/HTML#building_keyboard_accessibility_back_in).

### Accessibilità dei controlli non semantici

Questo segue la sezione precedente — quando una serie di `<div>` annidati insieme a CSS/JavaScript è utilizzata per creare una funzionalità UI complessa, o un controllo nativo è notevolmente migliorato/cambiato tramite JavaScript, non solo l'accessibilità tramite tastiera può soffrire, ma anche gli utenti dei lettori di schermo troveranno difficile capire cosa la funzionalità fa se non ci sono semantiche o altri indizi. In tali situazioni, ARIA può aiutare a fornire quelle semantiche mancanti.

#### Validazione dei moduli e avvisi di errore

Innanzitutto, rivediamo l'esempio del modulo di cui abbiamo parlato nel nostro articolo sull'accessibilità CSS e JavaScript (leggi [Mantenere l'ostruibilità](/it/docs/Learn_web_development/Core/Accessibility/CSS_and_JavaScript#keeping_it_unobtrusive) per un riepilogo completo). Alla fine di questa sezione, abbiamo mostrato che abbiamo incluso alcuni attributi ARIA sulla casella dei messaggi di errore che visualizzano eventuali errori di validazione quando provi a inviare il modulo:

```html
<div class="errors" role="alert" aria-relevant="all">
  <ul></ul>
</div>
```

- [`role="alert"`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/alert_role) trasforma automaticamente l'elemento a cui è applicato in una regione dal vivo, quindi le modifiche sono lette; identifica anche semanticamente come un messaggio di avviso (informazioni importanti nel tempo/attinenti al contesto) e rappresenta un modo migliore e più accessibile di fornire un avviso a un utente (le finestre di dialogo modali come le chiamate [`alert()`](/it/docs/Web/API/Window/alert) hanno una serie di problemi di accessibilità; vedere [Popup Windows](https://webaim.org/techniques/javascript/other#popups) da WebAIM).
- Un valore [`aria-relevant`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-relevant) di `all` istruisce il lettore di schermo a leggere i contenuti dell'elenco degli errori quando vengono apportate modifiche a esso — i.e., quando gli errori sono aggiunti o rimossi. Questo è utile perché l'utente vorrà sapere quali errori rimangono, non solo cosa è stato aggiunto o rimosso dall'elenco.

Potremmo andare oltre con l'uso di ARIA e fornire un po' più di assistenza alla validazione. Che ne dici di indicare se i campi sono richiesti in primo luogo, e quale dovrebbe essere l'intervallo dell'età?

1. A questo punto, prendi una copia del nostro [`form-validation.html`](https://github.com/mdn/learning-area/blob/main/accessibility/css/form-validation.html) e [`validation.js`](https://github.com/mdn/learning-area/blob/main/accessibility/css/validation.js) file, e salvali in una directory locale.
2. Aprili entrambi in un editor di testo e dai un'occhiata a come funziona il codice.
3. Innanzitutto, aggiungi un paragrafo appena sopra il tag di apertura `<form>`, come quello qui sotto, e contrassegna entrambe le `<label>` del modulo con un asterisco. Questo è normalmente il modo in cui contrassegniamo i campi obbligatori per gli utenti vedenti.

   ```html
   <p>Fields marked with an asterisk (*) are required.</p>
   ```

4. Questo ha senso visivamente, ma non è così facile da capire per gli utenti di lettori di schermo. Fortunatamente, WAI-ARIA fornisce l'attributo [`aria-required`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-required) per dare ai lettori di schermo indizi che dovrebbero informare gli utenti che gli input del modulo devono essere compilati. Aggiorna gli elementi `<input>` in questo modo:

   ```html
   <input type="text" name="name" id="name" aria-required="true" />

   <input type="number" name="age" id="age" aria-required="true" />
   ```

5. Se salvi ora l'esempio e lo testi con un lettore di schermo, dovresti sentire qualcosa come "Immetti il tuo nome asterisco, richiesto, modifica testo".
6. Potrebbe anche essere utile se diamo agli utenti di lettori di schermo e agli utenti vedenti un'idea di quale dovrebbe essere il valore dell'età. Questo è spesso presentato come tooltip o segnaposto all'interno del campo del modulo. WAI-ARIA include le proprietà [`aria-valuemin`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-valuemin) e [`aria-valuemax`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-valuemax) per specificare valori minimi e massimi, e i lettori di schermo supportano gli attributi nativi `min` e `max`. Un'altra funzionalità ben supportata è l'attributo HTML `placeholder`, che può contenere un messaggio che è mostrato nell'input quando non viene inserito nessun valore, ed è letto da alcuni lettori di schermo. Aggiorna il tuo input numerico in questo modo:

   ```html
   <label for="age">Your age:</label>
   <input
     type="number"
     name="age"
     id="age"
     placeholder="Enter 1 to 150"
     required
     aria-required="true" />
   ```

Include sempre un {{HTMLelement('label')}} per ogni input. Mentre alcuni lettori di schermo annunciano il testo di segnaposto, la maggior parte non lo fa. Sostituzioni accettabili per fornire controlli modulo con un nome accessibile includono [`aria-label`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-label) e [`aria-labelledby`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-labelledby). Ma l'elemento `<label>` con un attributo `for` è il metodo preferito poiché fornisce usabilità per tutti gli utenti, inclusi gli utenti di mouse.

> [!NOTE]
> Puoi vedere l'esempio finale dal vivo su [`form-validation-updated.html`](https://mdn.github.io/learning-area/accessibility/aria/form-validation-updated.html).

WAI-ARIA abilita anche alcune tecniche avanzate di etichettatura dei moduli, oltre al classico elemento {{htmlelement("label")}}. Abbiamo già parlato dell'uso della proprietà [`aria-label`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-label) per fornire un'etichetta dove non vogliamo che l'etichetta sia visibile agli utenti vedenti (vedere la sezione [Segnalazioni/Punti di riferimento](#signpostslandmarks) sopra). Alcune altre tecniche di etichettatura utilizzano altre proprietà come [`aria-labelledby`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-labelledby) se vuoi designare un elemento non `<label>` come etichetta o etichettare più input di modulo con la stessa etichetta, e [`aria-describedby`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-describedby), se vuoi associare altre informazioni a un input di modulo e farle leggere. Vedi l'articolo [Guida avanzata alle etichette del modulo di WebAIM](https://webaim.org/techniques/forms/advanced) per ulteriori dettagli.

Ci sono molte altre proprietà e stati utili, per indicare lo stato degli elementi del modulo. Ad esempio, `aria-disabled="true"` può essere utilizzato per indicare che un campo di un modulo è disabilitato. Molti browser salteranno i campi di modulo disabilitati che porta a non farli leggere dai lettori di schermo. In alcuni casi, un elemento disabilitato sarà percepito, quindi è una buona idea includere questo attributo per far sapere al lettore di schermo che un controllo del modulo disabilitato è infatti disabilitato.

Se lo stato disabilitato di un input è probabile che cambi, allora è anche una buona idea indicare quando accade, e qual è il risultato. Ad esempio, nel nostro demo [`form-validation-checkbox-disabled.html`](https://mdn.github.io/learning-area/accessibility/aria/form-validation-checkbox-disabled.html), c'è una casella di controllo quando attivata, abilita un altro input del modulo per consentire un'ulteriore informazione da immettere. Abbiamo impostato una regione dal vivo nascosta:

```html
<p class="hidden-alert" aria-live="assertive"></p>
```

che è nascosta alla vista utilizzando il posizionamento assoluto. Quando questa viene selezionata/deselezionata, aggiorniamo il testo all'interno della regione dal vivo nascosta per dire agli utenti di lettori di schermo qual è il risultato della selezione di questa casella di controllo, nonché aggiornando lo stato `aria-disabled`, e alcuni indicatori visivi anche:

```js
function toggleMusician(bool) {
  const instrument = formItems[formItems.length - 1];
  if (bool) {
    instrument.input.disabled = false;
    instrument.label.style.color = "#000";
    instrument.input.setAttribute("aria-disabled", "false");
    hiddenAlert.textContent =
      "Instruments played field now enabled; use it to tell us what you play.";
  } else {
    instrument.input.disabled = true;
    instrument.label.style.color = "#999";
    instrument.input.setAttribute("aria-disabled", "true");
    instrument.input.removeAttribute("aria-label");
    hiddenAlert.textContent = "Instruments played field now disabled.";
  }
}
```

#### Descrivere pulsanti non semantici come pulsanti

Un paio di volte in questo corso abbiamo menzionato l'accessibilità nativa (e i problemi di accessibilità che stanno dietro all'uso di altri elementi per emulare) pulsanti, link o elementi di modulo (vedere [Usa controlli UI semantici dove possibile](/it/docs/Learn_web_development/Core/Accessibility/HTML#use_semantic_ui_controls_where_possible) nell'articolo sull'accessibilità HTML, e [Migliorare l'accessibilità tramite tastiera](#miglioramento_dell'accessibilità_tramite_tastiera) sopra). Fondamentalmente, puoi ricostruire l'accessibilità tramite tastiera senza troppi problemi in molti casi, utilizzando `tabindex` e un po' di JavaScript.

Ma che dire dei lettori di schermo? Continuano a non vedere gli elementi come pulsanti. Se testiamo il nostro esempio [`fake-div-buttons.html`](https://mdn.github.io/learning-area/tools-testing/cross-browser-testing/accessibility/fake-div-buttons.html) in un lettore di schermo, i nostri pulsanti falsi verranno riportati con frasi come "Click me!, gruppo", il che è ovviamente confondente.

Possiamo risolvere questo problema utilizzando un ruolo WAI-ARIA. Crea una copia locale di [`fake-div-buttons.html`](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/accessibility/fake-div-buttons.html), e aggiungi [`role="button"`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/button_role) a ciascun `<div>` del pulsante, ad esempio:

```html
<div data-message="This is from the first button" tabindex="0" role="button">
  Click me!
</div>
```

Ora quando provi questo con un lettore di schermo, vedrai i pulsanti riportati con frasi come "Click me!, pulsante". Anche se questo è molto meglio, devi comunque aggiungere tutte le funzionalità native che gli utenti si aspettano, come la gestione degli eventi <kbd>enter</kbd> e click, come spiegato nella documentazione del [`ruolo`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/button_role) `button`.

> [!NOTE]
> Tuttavia, non dimenticare che usare l'elemento semantico corretto dove possibile è sempre meglio. Se vuoi creare un pulsante, e puoi usare un elemento {{htmlelement("button")}}, dovresti usare un elemento {{htmlelement("button")}}!

#### Guidare gli utenti attraverso widget complessi

Ci sono un'intera serie di [ruoli](/it/docs/Web/Accessibility/ARIA/Reference/Roles) in grado di identificare strutture di elementi non semantici come funzionalità UI comuni che vanno oltre ciò che è disponibile in HTML standard, ad esempio [`combobox`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/combobox_role), [`slider`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/slider_role), [`tabpanel`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/tabpanel_role), [`tree`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/tree_role). Puoi vedere diversi esempi utili nella [biblioteca di codice dell'università Deque](https://dequeuniversity.com/library/) per avere un'idea di come tali controlli possano essere resi accessibili.

Passiamo attraverso un esempio nostro. Torniamo alla nostra semplice interfaccia tabulata posizionata in modo assoluto (vedi [Nascondere le cose](/it/docs/Learn_web_development/Core/Accessibility/CSS_and_JavaScript#hiding_things) nel nostro articolo sull'accessibilità CSS e JavaScript), che puoi trovare su [Esempio di casella di informazioni tabulata](/it/docs/Learn_web_development/Core/CSS_layout/Practical_positioning_examples#a_tabbed_info-box).

```html live-sample___aria-tabbed-info-box
<section class="info-box">
  <div role="tablist" class="manual">
    <button
      id="tab-1"
      type="button"
      role="tab"
      aria-selected="true"
      aria-controls="tabpanel-1">
      <span>Tab 1</span>
    </button>
    <button
      id="tab-2"
      type="button"
      role="tab"
      aria-selected="false"
      aria-controls="tabpanel-2"
      tabindex="-1">
      <span>Tab 2</span>
    </button>
    <button
      id="tab-3"
      type="button"
      role="tab"
      aria-selected="false"
      aria-controls="tabpanel-3"
      tabindex="-1">
      <span>Tab 3</span>
    </button>
  </div>
  <div class="panels">
    <article id="tabpanel-1" role="tabpanel" aria-labelledby="tab-1">
      <h2>The first tab</h2>
      <p>This is the content for tab one and is just a paragraph.</p>
    </article>
    <article
      id="tabpanel-2"
      role="tabpanel"
      aria-labelledby="tab-2"
      class="is-hidden">
      <h2>The second tab</h2>
      <p>This is the content for tab two and is just a paragraph.</p>
    </article>
    <article
      id="tabpanel-3"
      role="tabpanel"
      aria-labelledby="tab-3"
      class="is-hidden">
      <h2>The third tab</h2>
      <p>This is the content for tab three and is a paragraph and a list.</p>
      <ul>
        <li>Cat</li>
        <li>Dog</li>
        <li>Horse</li>
      </ul>
    </article>
  </div>
</section>
```

```css live-sample___aria-tabbed-info-box
/* General setup */

html {
  font-family: sans-serif;
}

* {
  box-sizing: border-box;
}

body {
  margin: 0;
}

/* info-box setup */

.info-box {
  width: 452px;
  height: 250px;
  margin: 1.25rem auto 0;
}

/* styling info-box tabs */

.info-box [role="tablist"] {
  min-width: 100%;
  display: flex;
}

.info-box [role="tab"] {
  border: none;
  background: white;
  padding: 0 1rem 0 1rem;
  line-height: 3rem;
  color: #b60000;
  font-weight: bold;
  outline: none;
}

.info-box [role="tab"]:focus span,
.info-box [role="tab"]:hover span {
  outline: 1px solid blue;
  outline-offset: 6px;
  border-radius: 4px;
}

.info-box [role="tab"][aria-selected="true"] {
  background-color: #b60000;
  color: white;
}

/* styling info-box panels */

.info-box .panels {
  height: 200px;
  clear: both;
  position: relative;
}

.info-box [role="tabpanel"] {
  color: white;
  position: absolute;
  padding: 0.8rem 1.2rem;
  height: 200px;
  width: 100%;
  top: 0;
  background-color: #b60000;
  left: 0;
}

.info-box [role="tabpanel"].is-hidden {
  display: none;
}
```

```js live-sample___aria-tabbed-info-box
class TabsManual {
  constructor(groupNode) {
    this.tablistNode = groupNode;

    this.tabs = [];

    this.firstTab = null;
    this.lastTab = null;

    this.tabs = Array.from(this.tablistNode.querySelectorAll("[role=tab]"));
    this.tabpanels = [];

    for (let i = 0; i < this.tabs.length; i += 1) {
      const tab = this.tabs[i];
      const tabpanel = document.getElementById(
        tab.getAttribute("aria-controls"),
      );

      tab.tabIndex = -1;
      tab.setAttribute("aria-selected", "false");
      this.tabpanels.push(tabpanel);

      tab.addEventListener("keydown", this.onKeydown.bind(this));
      tab.addEventListener("click", this.onClick.bind(this));

      if (!this.firstTab) {
        this.firstTab = tab;
      }
      this.lastTab = tab;
    }

    this.setSelectedTab(this.firstTab);
  }

  setSelectedTab(currentTab) {
    for (let i = 0; i < this.tabs.length; i += 1) {
      const tab = this.tabs[i];
      if (currentTab === tab) {
        tab.setAttribute("aria-selected", "true");
        tab.removeAttribute("tabindex");
        this.tabpanels[i].classList.remove("is-hidden");
      } else {
        tab.setAttribute("aria-selected", "false");
        tab.tabIndex = -1;
        this.tabpanels[i].classList.add("is-hidden");
      }
    }
  }

  moveFocusToTab(currentTab) {
    currentTab.focus();
  }

  moveFocusToPreviousTab(currentTab) {
    let index;

    if (currentTab === this.firstTab) {
      this.moveFocusToTab(this.lastTab);
    } else {
      index = this.tabs.indexOf(currentTab);
      this.moveFocusToTab(this.tabs[index - 1]);
    }
  }

  moveFocusToNextTab(currentTab) {
    let index;

    if (currentTab === this.lastTab) {
      this.moveFocusToTab(this.firstTab);
    } else {
      index = this.tabs.indexOf(currentTab);
      this.moveFocusToTab(this.tabs[index + 1]);
    }
  }

  /* EVENT HANDLERS */

  onKeydown(event) {
    const tgt = event.currentTarget;
    let flag = false;

    switch (event.key) {
      case "ArrowLeft":
        this.moveFocusToPreviousTab(tgt);
        flag = true;
        break;

      case "ArrowRight":
        this.moveFocusToNextTab(tgt);
        flag = true;
        break;

      case "Home":
        this.moveFocusToTab(this.firstTab);
        flag = true;
        break;

      case "End":
        this.moveFocusToTab(this.lastTab);
        flag = true;
        break;

      default:
        break;
    }

    if (flag) {
      event.stopPropagation();
      event.preventDefault();
    }
  }

  // Since this example uses buttons for the tabs, the click onr also is activated
  // with the space and enter keys
  onClick(event) {
    this.setSelectedTab(event.currentTarget);
  }
}

// Initialize tablist

window.addEventListener("load", function () {
  const tablists = document.querySelectorAll("[role=tablist].manual");
  for (let i = 0; i < tablists.length; i++) {
    new TabsManual(tablists[i]);
  }
});
```

{{EmbedLiveSample("aria-tabbed-info-box", "100", "270")}}

In questo esempio abbiamo usato una combinazione di elementi semantici, ruoli aria e attributi aria. Il primo di questi è che abbiamo usato un elemento {{htmlelement("button")}} come _tab_, il che significa che il tab può essere selezionato tramite un clic del mouse o tramite tastiera usando spazio o invio.

Le funzionalità ARIA utilizzate includono:

- Nuovi ruoli — [`tablist`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/tablist_role), [`tab`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/tab_role), [`tabpanel`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/tabpanel_role)
  - : Questi identificano le aree rilevanti dell'interfaccia tabulata — il contenitore per le tab, le tab stesse, e i relativi pannelli delle tab.
- [`aria-selected`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-selected)
  - : Definisce quale tab è attualmente selezionata. Man mano che diverse tab sono selezionate dall'utente, il valore di questo attributo sulle diverse tab viene aggiornato tramite JavaScript.
- `tabindex="-1"`
  - : `tabindex="-1"` rimuove l'elemento dall'ordine del tab. Poiché stiamo utilizzando JavaScript per permettere all'utente di controllare le tab tramite tastiera o mouse, non vogliamo che l'utente possa usare il tasto tab per navigare tra i pulsanti.
- [`aria-labelledby`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-labelledby)
  - : Questo attributo identifica un elemento (tramite il suo `id`) che etichetta l'elemento, in questo esempio l`<article>` è etichettato dalla tab corrispondente o `<button>`.
- [`aria-controls`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-controls)
  - : Questo attributo identifica un elemento (tramite il suo `id`) che è controllato dall'elemento, in questo esempio l`<article>` è controllato dalla tab corrispondente o `<button>`.

Avremmo potuto usare `aria-hidden` per nascondere il contenuto dei pannelli delle tab dalle tecnologie assistive ma se quel contenuto conteneva contenuto focalizzabile, come collegamenti, l'utente sarebbe ancora in grado di focalizzarsi su quel contenuto anche quando aria-hidden=true è impostato per i pannelli non attivi. In questo esempio abbiamo applicato `class="is-hidden"` ai pannelli delle tab che corrispondono alle tab con `aria-selected="false"` e usando CSS per `display: none;` che impedisce di focalizzarsi sul contenuto nascosto.

Nei nostri test, questa nuova struttura ha servito a migliorare le cose globalmente. I `<button>` sono ora riconosciuti come tab (ad esempio, "tab" viene detto dal lettore di schermo), la tab selezionata è indicata da "selezionata" letta insieme al nome della tab e qualsiasi contenuto non mostrato non può essere focalizzato. L'utente può anche navigare tra le tab con la tastiera o il mouse.

## Testa le tue competenze!

Hai raggiunto la fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare ulteriori test per verificare di aver trattenuto queste informazioni prima di continuare — vedi [Testa le tue competenze: WAI-ARIA](/it/docs/Learn_web_development/Core/Accessibility/Test_your_skills/WAI-ARIA).

## Sommario

Questo articolo non ha coperto tutto ciò che è disponibile in WAI-ARIA, ma dovrebbe averti fornito abbastanza informazioni per comprendere come utilizzarlo, e sapere alcuni dei modelli più comuni che incontrerai che lo richiedono.

## Vedi anche

- [Stati e proprietà con Aria](/it/docs/Web/Accessibility/ARIA/Reference/Attributes): Tutti gli attributi `aria-*`
- [Ruoli WAI-ARIA](/it/docs/Web/Accessibility/ARIA/Reference/Roles): Categorie di ruoli ARIA e ruoli trattati su MDN
- [ARIA in HTML](https://www.w3.org/TR/html-aria/) su W3C: Una specifica che definisce, per ogni caratteristica HTML, le semantiche di accessibilità (ARIA) applicate implicitamente su di essa dal browser e le funzionalità WAI-ARIA che puoi impostare se sono necessari extra semantiche
- [Libreria di codice dell'università Deque](https://dequeuniversity.com/library/): Una libreria di esempi davvero utili e pratici che mostrano controlli UI complessi resi accessibili utilizzando le funzionalità di WAI-ARIA
- [Pratiche di scrittura WAI-ARIA](https://www.w3.org/WAI/ARIA/apg/) su W3C: Un modello di design molto dettagliato del W3C che spiega come implementare diversi tipi di controllo UI complesso mentre li rende accessibili utilizzando le funzionalità WAI-ARIA

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/CSS_and_JavaScript","Learn_web_development/Core/Accessibility/Multimedia", "Learn_web_development/Core/Accessibility")}}
