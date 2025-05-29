---
title: Nozioni di base su WAI-ARIA
short-title: WAI-ARIA
slug: Learn_web_development/Core/Accessibility/WAI-ARIA_basics
l10n:
  sourceCommit: edb16c0a662d7e719efe67561389a7a087c1ace9
---

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/CSS_and_JavaScript","Learn_web_development/Core/Accessibility/Multimedia", "Learn_web_development/Core/Accessibility")}}

A seguito dell'articolo precedente, a volte creare controlli dell'interfaccia utente complessi che coinvolgono HTML non semantico e contenuti aggiornati dinamicamente in JavaScript può essere difficile. WAI-ARIA è una tecnologia che può aiutare con tali problemi aggiungendo ulteriori semantiche che i browser e le tecnologie assistive possono riconoscere e utilizzare per informare gli utenti su cosa sta succedendo. Qui mostreremo come usarlo a un livello base per migliorare l'accessibilità.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Familiarità con <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e le pratiche migliori di accessibilità come insegnato nelle lezioni precedenti del modulo.</a>.</td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>Scopo di WAI-ARIA — fornire semantica a HTML altrimenti non semantico, affinché gli utenti di AT possano comprendere le interfacce presentate loro.</li>
          <li>La sintassi di base — ruoli, proprietà e stati.</li>
          <li>Riferimenti e punti di riferimento.</li>
          <li>Miglioramento dell'accessibilità da tastiera.</li>
          <li>Annuncio degli aggiornamenti dei contenuti dinamici con regioni live.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Che cos'è WAI-ARIA?

Iniziamo osservando cos'è WAI-ARIA e cosa può fare per noi.

### Un insieme completamente nuovo di problemi

Man mano che le applicazioni web hanno iniziato a diventare più complesse e dinamiche, è iniziato a emergere un nuovo insieme di funzionalità e problemi di accessibilità.

Ad esempio, l'HTML ha introdotto una serie di elementi semantici per definire funzionalità comuni della pagina ({{htmlelement("nav")}}, {{htmlelement("footer")}}, ecc.). Prima che questi fossero disponibili, gli sviluppatori utilizzavano {{htmlelement("div")}} con ID o classi, ad esempio `<div class="nav">`, ma questi erano problematici, poiché non c'era un modo facile per trovare facilmente una specifica funzionalità della pagina come la navigazione principale in modo programmato.

La soluzione iniziale è stata quella di aggiungere uno o più link nascosti in cima alla pagina per collegarsi alla navigazione (o altro), ad esempio:

```html
<a href="#hidden" class="hidden">Skip to navigation</a>
```

Ma questo non è ancora molto preciso e può essere utilizzato solo quando il lettore dello schermo sta leggendo dalla cima della pagina.

Un altro esempio riguarda le app che hanno iniziato a presentare controlli complessi come i selettori di date per scegliere le date, i cursori per scegliere i valori, ecc. HTML fornisce tipi speciali di input per visualizzare tali controlli:

```html
<input type="date" /> <input type="range" />
```

Questi inizialmente non erano ben supportati ed era, e in parte è ancora, difficile stilizzarli, portando designer e sviluppatori a optare per soluzioni personalizzate. Invece di utilizzare queste funzionalità native, alcuni sviluppatori si affidano a librerie JavaScript che generano tali controlli come una serie di {{htmlelement("div")}} annidati che vengono poi stilizzati utilizzando CSS e controllati utilizzando JavaScript.

Il problema qui è che visivamente funzionano, ma i lettori dello schermo non riescono a capire di cosa si tratta e i loro utenti ricevono solo la notifica che possono vedere un insieme di elementi senza semantica che descrivano il loro significato.

### Entra in scena WAI-ARIA

[WAI-ARIA](https://www.w3.org/TR/wai-aria/) (Web Accessibility Initiative - Accessible Rich Internet Applications) è una specifica scritta dal W3C, che definisce un insieme di attributi HTML aggiuntivi che possono essere applicati agli elementi per fornire ulteriori semantica e migliorare l'accessibilità ovunque essa sia carente. Ci sono tre caratteristiche principali definite nella specifica:

- [Ruoli](/it/docs/Web/Accessibility/ARIA/Reference/Roles)
  - : Questi definiscono cosa è o cosa fa un elemento. Molti di questi sono i cosiddetti ruoli di riferimento, che duplicano in gran parte il valore semantico degli elementi strutturali, come `role="navigation"` ({{htmlelement("nav")}}), `role="banner"` (documento {{htmlelement("header")}}), `role="complementary"` ({{htmlelement("aside")}}) o, `role="search"` ({{htmlelement("search")}}). Altri ruoli descrivono diverse strutture di pagina che non hanno elementi che corrispondono a quei ruoli, come `role="tablist"`, e `role="tabpanel"`, che si trovano comunemente nelle interfacce utente.
- Proprietà
  - : Queste definiscono le proprietà degli elementi, che possono essere utilizzate per dare loro significato o semantica extra. Ad esempio, `aria-required="true"` specifica che un input del modulo deve essere compilato per essere valido, mentre `aria-labelledby="label"` consente di mettere un ID su un elemento, quindi fare riferimento ad esso come etichetta per qualsiasi altra cosa sulla pagina, incluso più elementi, cosa non possibile utilizzando `<label for="input">`. Ad esempio, si potrebbe usare `aria-labelledby` per specificare che una descrizione chiave contenuta in un {{htmlelement("div")}} è l'etichetta per più celle di tabella, o potrebbe essere utilizzata come alternativa al testo alternativo per l'immagine — specificare le informazioni esistenti sulla pagina come testo alternativo di un'immagine piuttosto che doverle ripetere all'interno dell'attributo `alt`. È possibile vedere un esempio di questo in [Alternative testuali](/it/docs/Learn_web_development/Core/Accessibility/HTML#text_alternatives).
- Stati
  - : Proprietà speciali che definiscono le condizioni attuali degli elementi, ad esempio `aria-disabled="true"`, che specifica ad un lettore dello schermo che un input del modulo è attualmente disabilitato. Gli stati differiscono dalle proprietà in quanto le proprietà non cambiano durante il ciclo di vita di un'app, mentre gli stati possono cambiare, generalmente in modo programmato tramite JavaScript.

Un punto importante sugli attributi WAI-ARIA è che non influenzano nulla della pagina web, tranne le informazioni esposte dalle API di accessibilità del browser (da dove i lettori dello schermo ottengono le loro informazioni). WAI-ARIA non influenza la struttura della pagina web, il DOM, ecc., anche se gli attributi possono essere utili per selezionare elementi tramite CSS.

> [!NOTE]
> Puoi trovare un elenco utile di tutti i ruoli ARIA e i loro usi, con link a ulteriori informazioni, nella specifica WAI-ARIA — vedi [Definizione dei ruoli](https://www.w3.org/TR/wai-aria-1.1/#role_definitions) — su questo sito — vedi [Ruoli ARIA](/it/docs/Web/Accessibility/ARIA/Reference/Roles).
>
> La specifica contiene anche un elenco di tutte le proprietà e gli stati, con link a ulteriori informazioni — vedi [Definizioni di stati e proprietà (tutti gli attributi `aria-*`)](https://www.w3.org/TR/wai-aria-1.1/#state_prop_def).

### Dove è supportato WAI-ARIA?

Questa non è una domanda facile a cui rispondere. È difficile trovare una risorsa conclusiva che indichi quali caratteristiche di WAI-ARIA sono supportate e dove, perché:

1. Ci sono molte caratteristiche nella specifica WAI-ARIA.
2. Ci sono molte combinazioni di sistemi operativi, browser e lettori dello schermo da considerare.

Quest'ultimo punto è fondamentale — Per utilizzare un lettore dello schermo in primo luogo, il tuo sistema operativo deve eseguire browser che abbiano le necessarie API di accessibilità per esporre le informazioni necessarie ai lettori dello schermo per svolgere il loro lavoro. La maggior parte dei sistemi operativi popolari ha uno o due browser con cui i lettori dello schermo possono lavorare. Il Paciello Group ha un post piuttosto aggiornato che fornisce dati in quest'area — vedi [Guida approssimativa: browser, sistemi operativi e supporto dei lettori di schermo aggiornati](https://www.tpgi.com/rough-guide-browsers-operating-systems-and-screen-reader-support-updated/).

Successivamente, devi preoccuparti del fatto che i browser in questione supportano le funzionalità ARIA e le espongono tramite le loro API, ma anche se i lettori dello schermo riconoscono quelle informazioni e le presentano ai loro utenti in un modo utile.

1. Il supporto del browser è quasi universale.
2. Il supporto dei lettori dello schermo per le funzionalità ARIA non è ancora a questo livello, ma i lettori dello schermo più popolari ci stanno arrivando. Puoi farti un'idea dei livelli di supporto guardando l'articolo [Compatibilità dei lettori di schermo WAI-ARIA](https://www.powermapper.com/tests/screen-readers/aria/) di Powermapper.

In questo articolo, non tenteremo di coprire ogni funzionalità di WAI-ARIA e i suoi dettagli esatti di supporto. Invece, copriremo le funzionalità WAI-ARIA più critiche che devi conoscere; se non menzioniamo alcun dettaglio di supporto, puoi presumere che la funzionalità sia ben supportata. Segnaleremo chiaramente eventuali eccezioni a questo.

> [!NOTE]
> Alcune librerie JavaScript supportano WAI-ARIA, il che significa che quando generano funzionalità dell'interfaccia utente come controlli di modulo complessi, aggiungono attributi ARIA per migliorare l'accessibilità di quelle funzionalità. Se stai cercando una soluzione JavaScript di terze parti per uno sviluppo rapido dell'interfaccia utente, dovresti sicuramente considerare l'accessibilità dei suoi widget UI come un fattore importante nella scelta. Buoni esempi sono jQuery UI (vedi [Informazioni su jQuery UI: Supporto per l'accessibilità profonda](https://jqueryui.com/about/#deep-accessibility-support)), [ExtJS](https://www.sencha.com/products/extjs/) e [Dojo/Dijit](https://dojotoolkit.org/reference-guide/1.10/dijit/a11y/statement.html).

### Quando dovrebbe essere usato WAI-ARIA?

Abbiamo parlato di alcuni dei problemi che hanno spinto alla creazione di WAI-ARIA in precedenza, ma essenzialmente, ci sono quattro aree principali in cui WAI-ARIA è utile:

- Riferimenti/Punti di riferimento
  - : I valori degli attributi [`role`](/it/docs/Web/Accessibility/ARIA/Reference/Roles) di ARIA possono fungere da punti di riferimento che replicano la semantica degli elementi HTML (ad es., {{htmlelement("nav")}}), o vanno oltre la semantica HTML per fornire riferimenti a diverse aree funzionali, ad esempio, `search`, `tablist`, `tab`, `listbox`, ecc.
- Aggiornamenti dinamici dei contenuti
  - : I lettori dello schermo tendono a riscontrare difficoltà nel riportare contenuti in costante cambiamento; con ARIA possiamo utilizzare `aria-live` per informare gli utenti di lettori dello schermo quando un'area di contenuto viene aggiornata dinamicamente: ad esempio, usando JavaScript nella pagina per [recuperare nuove informazioni dal server e aggiornare il DOM](/it/docs/Learn_web_development/Core/Scripting/Network_requests).
- Miglioramento dell'accessibilità da tastiera
  - : Ci sono elementi HTML incorporati che hanno un'accessibilità da tastiera nativa; quando vengono utilizzati altri elementi insieme a JavaScript per simulare interazioni simili, l'accessibilità da tastiera e il reporting del lettore dello schermo ne risentono. Dove ciò è inevitabile, WAI-ARIA fornisce un mezzo per consentire ad altri elementi di ricevere il focus (usando `tabindex`).
- Accessibilità dei controlli non semantici
  - : Quando una serie di `<div>` annidati insieme a CSS/JavaScript viene utilizzata per creare una funzionalità UI complessa, o un controllo nativo viene notevolmente migliorato/modificato tramite JavaScript, l'accessibilità può risentirne — gli utenti di lettori dello schermo troveranno difficile capire cosa fa la funzionalità se non ci sono semantiche o altri indizi. In queste situazioni, ARIA può aiutare a fornire ciò che manca con una combinazione di ruoli come `button`, `listbox`, o `tablist`, e proprietà come `aria-required` o `aria-posinset` per fornire ulteriori indizi sulla funzionalità.

#### Dovresti usare WAI-ARIA solo quando ne hai bisogno!

Utilizzare gli elementi HTML corretti implicita fornisce i ruoli necessari e dovresti _sempre_ usare [funzionalità HTML native](/it/docs/Learn_web_development/Core/Accessibility/HTML) per fornire la semantica richiesta dai lettori dello schermo per informare i loro utenti su cosa sta succedendo. A volte ciò non è possibile, o perché hai un controllo limitato sul codice, o perché stai creando qualcosa di complesso che non ha un elemento HTML facile da implementare. In tali casi, WAI-ARIA può essere uno strumento prezioso per migliorare l'accessibilità.

Ma di nuovo, usalo solo quando necessario!

> [!NOTE]
> Cerca anche di assicurarti di testare il tuo sito con una varietà di utenti _reali_ — persone senza disabilità, persone che utilizzano lettori di schermo, persone che utilizzano la navigazione da tastiera, ecc. Avranno intuizioni migliori di te su come funziona bene.

## Implementazioni pratiche di WAI-ARIA

Nella sezione successiva, esamineremo le quattro aree in maggior dettaglio, con esempi pratici. Prima di continuare, dovresti predisporre una configurazione di test per lettori di schermo, in modo da poter testare alcuni degli esempi man mano che prosegui.

Consulta la nostra sezione sui [test dei lettori di schermo](/it/docs/Learn_web_development/Core/Accessibility/Tooling#screen_readers) per ulteriori informazioni.

### Riferimenti/Punti di riferimento

WAI-ARIA aggiunge l'[attributo `role`](https://www.w3.org/TR/wai-aria-1.1/#role_definitions) ai browser, che consente di aggiungere valore semantico extra agli elementi del sito ovunque siano necessari. La prima area principale in cui questo è utile è fornire informazioni ai lettori di schermo in modo che i loro utenti possano trovare elementi comuni della pagina. Questo esempio ha la seguente struttura:

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
  background-color: #ff80ff;
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

Se provi a testare l'esempio con un lettore di schermo in un browser moderno, otterrai già alcune informazioni utili. Ad esempio, VoiceOver ti darà quanto segue:

- Sul `<header>` — "banner, 2 elementi" (contiene un titolo e il `<nav>`).
- Sul `<nav>` — "navigazione 2 elementi" (contiene un elenco e un modulo).
- Sul `<main>` — "main 2 elementi" (contiene un articolo e un aside).
- Sul `<aside>` — "complementare 2 elementi" (contiene un titolo e un elenco).
- Sull'input form di ricerca — "Query di ricerca, inserimento all'inizio del testo".
- Sul `<footer>` — "footer 1 elemento".

Se vai al menu di riferimenti di VoiceOver (accessibile utilizzando il tasto VoiceOver + U e quindi utilizzando i tasti cursore per scorrere le scelte del menu), vedrai che la maggior parte degli elementi è elencata in modo che possano essere accessibili rapidamente.

![Menu VoiceOver del Mac per un'accessibilità rapida. Intestazione dei punti di riferimento e elenco di punti di riferimento, tra cui banner, navigazione, principale e complemento.](landmarks-list.png)

Tuttavia, possiamo fare di meglio qui. Il modulo di ricerca è un punto di riferimento davvero importante che le persone vorranno trovare, ma non è elencato nel menu punti di riferimenti o trattato come un punto di riferimento significativo oltre al fatto che l'input stesso venga chiamato input di ricerca (`<input type="search">`).

Potremmo migliorarlo utilizzando il `role="search"` di ARIA, ma usare l'elemento {{htmlelement("search")}} implicitamente conferisce quel ruolo al form.

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
  background-color: #ff80ff;
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

La cosa più importante è che abbiamo utilizzato HTML semantico che dà significato ai ruoli della struttura della pagina senza aggiungere attributi [`role`](/it/docs/Web/Accessibility/ARIA/Reference/Roles) non necessari alla nostra struttura HTML, che ha una struttura del genere:

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

Ti abbiamo anche fornito una funzione bonus in questo esempio — l'elemento {{htmlelement("input")}} è stato dotato dell'attributo [`aria-label`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-label), che gli dà un'etichetta descrittiva da leggere da un lettore di schermo, anche se non abbiamo incluso un elemento {{htmlelement("label")}}. In casi come questi, è molto utile — un form di ricerca come questo è una caratteristica molto comune e facilmente riconoscibile, e l'aggiunta di un'etichetta visiva rovinerebbe il design della pagina.

```html
<input
  type="search"
  name="q"
  placeholder="Search query"
  aria-label="Search through site content" />
```

Ora se usiamo VoiceOver per guardare questo esempio, otteniamo dei miglioramenti:

- Il modulo di ricerca è chiamato come un elemento separato, sia quando si naviga attraverso la pagina, sia nel menu Punti di riferimento.
- Il testo dell'etichetta contenuto nell'attributo `aria-label` viene letto quando l'input del modulo è evidenziato.

Se hai bisogno di supportare vecchi browser come IE8, vale la pena di includere ruoli ARIA per quel motivo. E se per qualche motivo il tuo sito è costruito usando solo `<div>`, dovresti sicuramente includere i ruoli ARIA per fornire queste semantiche tanto necessarie!

Vedrai molto di più su queste semantiche e il potere delle proprietà/attributi ARIA più avanti, specialmente nella sezione [Accessibilità dei controlli non semantici](#accessibilità_dei_controlli_non_semantici). Per ora, però, vediamo come ARIA può aiutare con gli aggiornamenti dei contenuti dinamici.

### Aggiornamenti dinamici dei contenuti

I contenuti caricati nel DOM possono essere facilmente accessibili tramite un lettore di schermo, dal contenuto testuale al testo alternativo allegato alle immagini. Pertanto, i siti web statici tradizionali con contenuti in gran parte testuali sono facili da rendere accessibili per le persone con disabilità visive.

Il problema è che le moderne app web non sono spesso solo testo statico — spesso aggiornano parti della pagina recuperando nuovi contenuti dal server (in questo esempio stiamo usando un array statico di citazioni) e aggiornando il DOM. Questi sono talvolta chiamati **regioni live**.

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

Questo funziona, ma non è buono per l'accessibilità — l'aggiornamento dei contenuti non viene rilevato dai lettori di schermo, quindi i loro utenti non saprebbero cosa sta succedendo. Questo è un esempio piuttosto banale, ma immagina se stessi creando un'interfaccia utente complessa con molti contenuti in costante aggiornamento, come una chat room, o un'interfaccia utente di un gioco di strategia, o un display di carrello della spesa che si aggiorna in tempo reale — sarebbe impossibile usare l'app in modo efficace senza un modo per avvisare l'utente degli aggiornamenti.

Fortunatamente, WAI-ARIA fornisce un meccanismo utile per fornire questi avvisi — la proprietà [`aria-live`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-live). Applicandola a un elemento, i lettori di schermo leggeranno automaticamente il contenuto che viene aggiornato. Quanto il contenuto viene letto con urgenza dipende dal valore dell'attributo:

- `off`
  - : Il valore predefinito. Gli aggiornamenti non dovrebbero essere annunciati.
- `polite`
  - : Gli aggiornamenti dovrebbero essere annunciati solo se l'utente è inattivo.
- `assertive`
  - : Gli aggiornamenti dovrebbero essere annunciati all'utente il prima possibile.

Qui aggiorniamo il tag di apertura di `<section>` come segue:

```html
<section aria-live="assertive">…</section>
```

Questo farà sì che un lettore di schermo legga automaticamente il contenuto man mano che viene aggiornato.

C'è un'altra considerazione qui: solo la parte di testo che viene aggiornata viene letta. Potrebbe essere utile se leggessimo sempre anche il titolo, in modo che l'utente possa ricordare cosa viene letto. Per fare questo, possiamo aggiungere la proprietà [`aria-atomic`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-atomic) alla sezione. Aggiorna il tag di apertura di `<section>` ancora una volta, in questo modo:

```html
<section aria-live="assertive" aria-atomic="true">…</section>
```

L'attributo `aria-atomic="true"` dice ai lettori di schermo di leggere l'intero contenuto dell'elemento come un'unità atomica, non solo i bit che sono stati aggiornati.

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
> La proprietà [`aria-relevant`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-relevant) è anche abbastanza utile per controllare cosa viene letto quando una regione live viene aggiornata. Si può, ad esempio, far leggere solo le aggiunte o le rimozioni di contenuto.

### Miglioramento dell'accessibilità da tastiera

Come discusso in diversi altri luoghi del modulo, una delle principali forze di HTML rispetto all'accessibilità è l'accessibilità da tastiera incorporata in funzionalità come pulsanti, controlli di modulo e collegamenti. Generalmente, puoi usare il tasto tab per spostarti tra i controlli, il tasto Invio/Return per selezionare o attivare i controlli, e occasionalmente altri controlli come necessario (ad esempio i tasti cursore su e giù per muoversi tra le opzioni in una casella `<select>`).

Tuttavia, a volte finirai col dover scrivere codice che utilizza elementi non semantici come pulsanti (o altri tipi di controllo), o utilizza controlli focalizzabili per uno scopo non del tutto corretto. Potresti dover correggere del codice mal scritto che hai ereditato, o potresti costruire un widget complesso che lo richiede.

In termini di rendere il codice non focalizzabile focalizzabile, WAI-ARIA estende l'attributo `tabindex` con alcuni nuovi valori:

- `tabindex="0"` — come indicato sopra, questo valore consente agli elementi che normalmente non sono tappabili di diventare tappabili. Questo è il valore più utile di `tabindex`.
- `tabindex="-1"` — questo consente a elementi che normalmente non sono tappabili di ricevere il focus in modo programmato, ad esempio, tramite JavaScript, o come obiettivo di collegamenti.

Abbiamo discusso questo in modo più dettagliato e mostrato un'implementazione tipica già nel nostro articolo sull'accessibilità HTML — vedi [Ricostruire l'accessibilità da tastiera](/it/docs/Learn_web_development/Core/Accessibility/HTML#building_keyboard_accessibility_back_in).

### Accessibilità dei controlli non semantici

Questo segue dalla sezione precedente — quando una serie di `<div>` annidati insieme a CSS/JavaScript viene utilizzata per creare una funzionalità UI complessa, o un controllo nativo viene migliorato/cambiato in modo significativo tramite JavaScript, non solo l'accessibilità da tastiera può risentirne, ma gli utenti di lettori di schermo troveranno difficile capire cosa fa la funzionalità se non ci sono semantiche o altri indizi. In tali situazioni, ARIA può aiutare a fornire quelle semantiche mancanti.

#### Validazione dei moduli e avvisi di errore

Innanzitutto, riprendiamo l'esempio del modulo che abbiamo esaminato nel nostro articolo sull'accessibilità CSS e JavaScript (leggi [Mantenere discreta](/it/docs/Learn_web_development/Core/Accessibility/CSS_and_JavaScript#keeping_it_unobtrusive) per un pieno riepilogo). Alla fine di questa sezione, abbiamo mostrato che abbiamo incluso alcuni attributi ARIA sulla casella di messaggio di errore che mostra errori di validazione quando si tenta di inviare il modulo:

```html
<div class="errors" role="alert" aria-relevant="all">
  <ul></ul>
</div>
```

- [`role="alert"`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/alert_role) trasforma automaticamente l'elemento a cui è applicato in una regione live, quindi i cambiamenti ad esso vengono letti; identificalo anche semanticamente come un messaggio di avviso (informazioni importanti nel tempo/contesto), e rappresenta un modo migliore e più accessibile di fornire un avviso a un utente (finestre di dialogo modali come chiamate [`alert()`](/it/docs/Web/API/Window/alert) hanno una serie di problemi di accessibilità; vedi [Finestre popup](https://webaim.org/techniques/javascript/other#popups) di WebAIM).
- Un valore [`aria-relevant`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-relevant) di `all` istruisce il lettore di schermo a leggere i contenuti dell'elenco di errori quando vengono apportate modifiche ad esso — cioè, quando vengono aggiunti o rimossi errori. Questo è utile perché l'utente vorrà sapere quali errori rimangono, non solo cosa è stato aggiunto o rimosso dall'elenco.

Potremmo andare oltre con l'uso delle ARIA e fornire ulteriore aiuto per la validazione. Che ne dici di indicare se i campi devono essere compilati in primo luogo e quale gamma dovrebbe avere l'età?

1. A questo punto, prendi una copia dei nostri file [`form-validation.html`](https://github.com/mdn/learning-area/blob/main/accessibility/css/form-validation.html) e [`validation.js`](https://github.com/mdn/learning-area/blob/main/accessibility/css/validation.js) e salvali in una directory locale.
2. Aprili entrambi in un editor di testo e dai un'occhiata a come funziona il codice.
3. Innanzitutto, aggiungi un paragrafo appena sopra il tag di apertura `<form>`, come quello qui sotto, e marca entrambi i `<label>` con un asterisco. Questo è normalmente come marchiamo i campi richiesti per gli utenti vedenti.

   ```html
   <p>Fields marked with an asterisk (*) are required.</p>
   ```

4. Questo ha senso visivamente, ma non è facile da capire per gli utenti di lettori di schermo. Fortunatamente, WAI-ARIA fornisce l'attributo [`aria-required`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-required) per dare suggerimenti ai lettori di schermo che devono dire agli utenti che gli input del modulo devono essere compilati. Aggiorna gli elementi `<input>` in questo modo:

   ```html
   <input type="text" name="name" id="name" aria-required="true" />

   <input type="number" name="age" id="age" aria-required="true" />
   ```

5. Se salvi l'esempio ora e lo testi con un lettore di schermo, dovresti sentire qualcosa del tipo "Immetti il tuo nome stella, richiesto, modifica testo".
6. Potrebbe anche essere utile se forniamo agli utenti di lettori di schermo e agli utenti vedenti un'idea di quale dovrebbe essere il valore dell'età. Questo viene spesso presentato come un suggerimento o un segnaposto all'interno del campo modulo. WAI-ARIA include proprietà [`aria-valuemin`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-valuemin) e [`aria-valuemax`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-valuemax) per specificare i valori min e max, e i lettori di schermo supportano gli attributi nativi `min` e `max`. Un'altra funzionalità ben supportata è l'attributo `placeholder` HTML, che può contenere un messaggio che viene mostrato nell'input quando non è inserito alcun valore e viene letto da alcuni lettori di schermo. Aggiorna il numero dell'input in questo modo:

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

Includi sempre un {{HTMLelement('label')}} per ogni input. Mentre alcuni lettori di schermo annunciano il testo del segnaposto, la maggior parte non lo fa. Sostituzioni accettabili per fornire controlli di modulo con un nome accessibile includono [`aria-label`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-label) e [`aria-labelledby`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-labelledby). Ma l'elemento `<label>` con un attributo `for` è il metodo preferito poiché offre usabilità per tutti gli utenti, inclusi gli utenti del mouse.

> [!NOTE]
> Puoi vedere l'esempio completato dal vivo su [`form-validation-updated.html`](https://mdn.github.io/learning-area/accessibility/aria/form-validation-updated.html).

WAI-ARIA consente anche alcune tecniche avanzate di etichettatura dei moduli, oltre all'elemento {{htmlelement("label")}} classico. Abbiamo già parlato di utilizzare la proprietà [`aria-label`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-label) per fornire un'etichetta dove non vogliamo che l'etichetta sia visibile agli utenti vedenti (vedi la sezione [Riferimenti/Punti di riferimento](#signpostslandmarks), sopra). Alcune altre tecniche di etichettatura utilizzano altre proprietà come [`aria-labelledby`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-labelledby) se vuoi designare un elemento non-`<label>` come etichetta o etichettare più campi di input con la stessa etichetta, e [`aria-describedby`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-describedby), se vuoi associare altre informazioni a un input del modulo e farle leggere anche. Vedi l'[articolo di etichettatura avanzata dei moduli](https://webaim.org/techniques/forms/advanced) di WebAIM per ulteriori dettagli.

Ci sono molte altre proprietà e stati utili anche per indicare lo stato degli elementi del modulo. Ad esempio, `aria-disabled="true"` può essere usato per indicare che un campo del modulo è disabilitato. Molti browser passeranno oltre i campi del modulo disabilitati, il che porta al fatto che non vengono letti dai lettori di schermo. In alcuni casi, un elemento disabilitato verrà percepito, quindi è una buona idea includere questo attributo per informare il lettore di schermo che un controllo del modulo disabilitato è effettivamente disabilitato.

Se lo stato disabilitato di un input è destinato a cambiare, allora è anche una buona idea indicare quando accade e quale è il risultato. Ad esempio, nel nostro demo [`form-validation-checkbox-disabled.html`](https://mdn.github.io/learning-area/accessibility/aria/form-validation-checkbox-disabled.html), c'è una casella di verifica che, quando selezionata, abilita un altro input del modulo per consentire l'inserimento di ulteriori informazioni. Abbiamo impostato una regione live nascosta:

```html
<p class="hidden-alert" aria-live="assertive"></p>
```

che è nascosta dalla vista utilizzando il posizionamento assoluto. Quando questo è selezionato/deselezionato, aggiorniamo il testo all'interno della regione live nascosta per dire agli utenti di lettori di schermo quale è il risultato della selezione di questa casella di verifica, oltre ad aggiornare lo stato `aria-disabled`, e alcuni indicatori visivi anche:

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

#### Descrivere i pulsanti non semantici come pulsanti

Diverse volte in questo corso, abbiamo menzionato l'accessibilità nativa di (e i problemi di accessibilità derivanti dall'uso di altri elementi per simulare) pulsanti, collegamenti o elementi del modulo (vedi [Usare controlli UI semantici dove possibile](/it/docs/Learn_web_development/Core/Accessibility/HTML#use_semantic_ui_controls_where_possible) nell'articolo sull'accessibilità HTML, e [Miglioramento dell'accessibilità da tastiera](#miglioramento_dell'accessibilità_da_tastiera), sopra). Fondamentalmente, puoi aggiungere l'accessibilità da tastiera senza troppi problemi in molti casi, utilizzando `tabindex` e un po' di JavaScript.

Ma che dire dei lettori di schermo? Non vedranno comunque gli elementi come pulsanti. Se testiamo il nostro esempio [`fake-div-buttons.html`](https://mdn.github.io/learning-area/tools-testing/cross-browser-testing/accessibility/fake-div-buttons.html) in un lettore di schermo, i nostri finti pulsanti verranno riportati con frasi tipo "Fai clic su di me!, gruppo", il che è ovviamente confuso.

Possiamo risolvere questo problema utilizzando un ruolo WAI-ARIA. Fai una copia locale di [`fake-div-buttons.html`](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/accessibility/fake-div-buttons.html), e aggiungi [`role="button"`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/button_role) a ciascun `<div>` del pulsante, ad esempio:

```html
<div data-message="This is from the first button" tabindex="0" role="button">
  Click me!
</div>
```

Ora quando provi questo usando un lettore di schermo, i pulsanti verranno riportati con frasi come "Fai clic su di me!, pulsante". Anche se questo è molto meglio, devi ancora aggiungere tutte le funzionalità native che gli utenti si aspettano, come la gestione degli eventi <kbd>enter</kbd> e click, come spiegato nella [documentazione del ruolo `button`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/button_role).

> [!NOTE]
> Non dimenticare tuttavia che l'uso del corretto elemento semantico dove possibile è sempre meglio. Se vuoi creare un pulsante e puoi utilizzare un elemento {{htmlelement("button")}}, dovresti utilizzare un elemento {{htmlelement("button")}}!

#### Guidare gli utenti attraverso widget complessi

Esiste un intero insieme di altri [ruoli](/it/docs/Web/Accessibility/ARIA/Reference/Roles) che possono identificare strutture di elementi non semantici come funzionalità comuni dell'interfaccia utente che vanno oltre ciò che è disponibile nell'HTML standard, ad esempio [`combobox`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/combobox_role), [`slider`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/slider_role), [`tabpanel`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/tabpanel_role), [`tree`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/tree_role). Puoi vedere diversi esempi utili nella [libreria di codice Deque university](https://dequeuniversity.com/library/) per farti un'idea di come tali controlli possono essere resi accessibili.

Passiamo attraverso un nostro esempio. Torneremo alla nostra semplice interfaccia a schede con posizionamento assoluto (vedi [Nascondere cose](/it/docs/Learn_web_development/Core/Accessibility/CSS_and_JavaScript#hiding_things) nel nostro articolo sull'accessibilità CSS e JavaScript), che puoi trovare all'esempio [Scheda informativa](/it/docs/Learn_web_development/Core/CSS_layout/Practical_positioning_examples#a_tabbed_info-box).

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

    for (const tab of this.tabs) {
      const tabpanel = document.getElementById(
        tab.getAttribute("aria-controls"),
      );

      tab.tabIndex = -1;
      tab.setAttribute("aria-selected", "false");
      this.tabpanels.push(tabpanel);

      tab.addEventListener("keydown", this.onKeydown.bind(this));
      tab.addEventListener("click", this.onClick.bind(this));

      this.firstTab ??= tab;
      this.lastTab = tab;
    }

    this.setSelectedTab(this.firstTab);
  }

  setSelectedTab(currentTab) {
    for (const tab of this.tabs.length) {
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

window.addEventListener("load", () => {
  const tablists = document.querySelectorAll("[role=tablist].manual");
  for (const tablist of tablists) {
    new TabsManual(tablist);
  }
});
```

{{EmbedLiveSample("aria-tabbed-info-box", "100", "270")}}

In questo esempio abbiamo usato una combinazione di elementi semantici, ruoli aria e attributi aria. Il primo di questi è che abbiamo usato un elemento {{htmlelement("button")}} come _tab_, questo significa che la scheda può essere selezionata tramite un clic del mouse o tramite la tastiera usando barra spaziatrice o invio.

Le funzionalità ARIA usate includono:

- Nuovi ruoli — [`tablist`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/tablist_role), [`tab`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/tab_role), [`tabpanel`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/tabpanel_role)
  - : Questi identificano le aree importanti dell'interfaccia a schede — il contenitore per le schede, le schede stesse e i pannelli di schede corrispondenti.
- [`aria-selected`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-selected)
  - : Definisce quale scheda è attualmente selezionata. Poiché le diverse schede vengono selezionate dall'utente, il valore di questo attributo sulle diverse schede viene aggiornato tramite JavaScript.
- `tabindex="-1"`
  - : `tabindex="-1"` toglie l'elemento dall'ordine dei tab. Poiché stiamo usando JavaScript per consentire all'utente di controllare le schede tramite tastiera o mouse, non vogliamo che l'utente possa utilizzare il tasto tab per navigare sui pulsanti.
- [`aria-labelledby`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-labelledby)
  - : Questo attributo identifica un elemento (dal suo `id`) che etichetta l'elemento, in questo esempio l`<article>` è etichettato dalla scheda corrispondente o `<button>`.
- [`aria-controls`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-controls)
  - : Questo attributo identifica un elemento (dal suo `id`) che è controllato dall'elemento, in questo esempio l`<article>` è controllato dalla scheda corrispondente o `<button>`.

Avremmo potuto usare `aria-hidden` per nascondere il contenuto dei pannelli delle schede dalle tecnologie assistive, ma se quel contenuto conteneva contenuti focalizzabili, come collegamenti, l'utente sarebbe comunque in grado di interagire con quel contenuto anche quando aria-hidden=true è impostato per i pannelli non attivi. In questo esempio abbiamo applicato `class="is-hidden"` ai pannelli delle schede che corrispondono alle schede con `aria-selected="false"` e usiamo CSS per `display: none;` che impedisce che il contenuto nascosto venga tabulato.

Nei nostri test, questa nuova struttura ha effettivamente migliorato le cose. I `<button>` ora sono riconosciuti come schede (ad esempio, "scheda" è pronunciato dal lettore di schermo), la scheda selezionata è indicata da "selezionata" che viene letta insieme al nome della scheda e qualsiasi contenuto che non è mostrato non può essere tabulato. L'utente può anche navigare tra le schede con tastiera o mouse.

## Metti alla prova le tue competenze!

Sei arrivato alla fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare alcuni ulteriori test per verificare di aver assimilato queste informazioni prima di procedere — vedi [Metti alla prova le tue competenze: WAI-ARIA](/it/docs/Learn_web_development/Core/Accessibility/Test_your_skills/WAI-ARIA).

## Sommario

Questo articolo non ha affatto coperto tutto ciò che è disponibile in WAI-ARIA, ma dovrebbe averti fornito abbastanza informazioni per capire come usarlo e conoscere alcuni dei modelli più comuni che incontrerai che richiedono il suo uso.

## Vedi anche

- [Stati e proprietà Aria](/it/docs/Web/Accessibility/ARIA/Reference/Attributes): Tutti gli attributi `aria-*`
- [Ruoli WAI-ARIA](/it/docs/Web/Accessibility/ARIA/Reference/Roles): Categorie di ruoli ARIA e ruoli coperti su MDN
- [ARIA in HTML](https://www.w3.org/TR/html-aria/) su W3C: Una specifica che definisce, per ciascuna funzionalità HTML, le semantiche di accessibilità (ARIA) applicate implicitamente ad essa dal browser e le funzionalità WAI-ARIA che puoi impostare su di essa se sono richieste semantiche extra
- [Libreria di codice Deque university](https://dequeuniversity.com/library/): Una libreria di esempi davvero utili e pratici che mostrano controlli di UI complessi resi accessibili utilizzando funzionalità WAI-ARIA
- [Pratiche di authoring WAI-ARIA](https://www.w3.org/WAI/ARIA/apg/) su W3C: Un modello di design molto dettagliato dal W3C, che spiega come implementare diversi tipi di controllo UI complesso rendendoli accessibili utilizzando funzionalità WAI-ARIA

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/CSS_and_JavaScript","Learn_web_development/Core/Accessibility/Multimedia", "Learn_web_development/Core/Accessibility")}}
