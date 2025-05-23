---
title: Nozioni di base su WAI-ARIA
short-title: WAI-ARIA
slug: Learn_web_development/Core/Accessibility/WAI-ARIA_basics
l10n:
  sourceCommit: f2dc3d5367203c860cf1a71ce0e972f018523849
---

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/CSS_and_JavaScript","Learn_web_development/Core/Accessibility/Multimedia", "Learn_web_development/Core/Accessibility")}}

Continuando dall'articolo precedente, a volte può essere difficile creare controlli UI complessi che coinvolgono HTML non semantico e contenuti aggiornati dinamicamente con JavaScript. WAI-ARIA è una tecnologia che può aiutare con tali problemi aggiungendo ulteriori semantiche che i browser e le tecnologie assistive possono riconoscere e utilizzare per informare gli utenti su ciò che sta accadendo. Qui mostreremo come usarlo a un livello base per migliorare l'accessibilità.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Familiarità con <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e le migliori pratiche di accessibilità come insegnato nelle lezioni precedenti del modulo.</a>.</td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Lo scopo di WAI-ARIA — fornire semantica a HTML altrimenti non semantico, in modo che gli utenti delle tecnologie assistive possano comprendere le interfacce che vengono loro presentate.</li>
          <li>La sintassi di base — ruoli, proprietà e stati.</li>
          <li>Punti di riferimento e segnaletica.</li>
          <li>Migliorare l'accessibilità tramite tastiera.</li>
          <li>Annunciare aggiornamenti dinamici del contenuto con regioni dal vivo.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Che cos'è WAI-ARIA?

Iniziamo esaminando cos'è WAI-ARIA e cosa può fare per noi.

### Un insieme completamente nuovo di problemi

Man mano che le app web sono diventate più complesse e dinamiche, è emerso un nuovo insieme di funzionalità e problemi di accessibilità.

Ad esempio, HTML ha introdotto un numero di elementi semantici per definire caratteristiche comuni della pagina ({{htmlelement("nav")}}, {{htmlelement("footer")}}, ecc.). Prima che fossero disponibili, gli sviluppatori utilizzavano {{htmlelement("div")}} con ID o classi, ad esempio `<div class="nav">`, ma questi erano problematici, in quanto non c'era un modo semplice per trovare facilmente una caratteristica specifica della pagina come la navigazione principale a livello programmatico.

La soluzione iniziale era quella di aggiungere uno o più link nascosti in cima alla pagina per collegarsi alla navigazione (o altro), ad esempio:

```html
<a href="#hidden" class="hidden">Skip to navigation</a>
```

Tuttavia, questo non è ancora molto preciso, e può essere utilizzato solo quando il lettore di schermo sta leggendo dall'alto della pagina.

Un altro esempio sono le app che hanno iniziato a includere controlli complessi come date picker per scegliere le date, slider per scegliere i valori, ecc. HTML fornisce tipi di input speciali per rendere tali controlli:

```html
<input type="date" /> <input type="range" />
```

Questi originariamente non erano ben supportati e, ed è stato, e in una certa misura lo è ancora, difficile da stilizzare, portando designer e sviluppatori a optare per soluzioni personalizzate. Invece di utilizzare queste funzionalità native, alcuni sviluppatori si affidano a librerie JavaScript che generano tali controlli come una serie di {{htmlelement("div")}} annidati che vengono quindi stilizzati utilizzando CSS e controllati utilizzando JavaScript.

Il problema qui è che visivamente funzionano, ma i lettori di schermo non riescono a capire affatto cosa siano, e i loro utenti si limitano a essere informati che possono vedere un miscuglio di elementi senza semantica per descrivere ciò che significano.

### Entra WAI-ARIA

[WAI-ARIA](https://www.w3.org/TR/wai-aria/) (Web Accessibility Initiative - Accessible Rich Internet Applications) è una specifica scritta dal W3C, che definisce un insieme di attributi HTML aggiuntivi che possono essere applicati agli elementi per fornire semantica aggiuntiva e migliorare l'accessibilità ovunque manchi. Ci sono tre caratteristiche principali definite nella specifica:

- [Ruoli](/it/docs/Web/Accessibility/ARIA/Reference/Roles)
  - : Questi definiscono cosa è o fa un elemento. Molti di questi sono i cosiddetti ruoli di punto di riferimento, che duplicano in gran parte il valore semantico degli elementi strutturali, come `role="navigation"` ({{htmlelement("nav")}}), `role="banner"` (document {{htmlelement("header")}}), `role="complementary"` ({{htmlelement("aside")}}) o , `role="search"` ({{htmlelement("search")}}). Alcuni altri ruoli descrivono diverse strutture di pagina che non hanno elementi che corrispondono a tali ruoli, come `role="tablist"`, e `role="tabpanel"`, che si trovano comunemente nelle interfacce utente.
- Proprietà
  - : Queste definiscono proprietà degli elementi, che possono essere utilizzate per dare loro un significato o una semantica extra. Ad esempio, `aria-required="true"` specifica che un input di modulo deve essere compilato per essere valido, mentre `aria-labelledby="label"` consente di mettere un ID su un elemento, quindi riferirlo come etichetta per qualsiasi altra cosa sulla pagina, inclusi più elementi, cosa non possibile usando `<label for="input">`. Ad esempio, potrebbe essere utilizzato `aria-labelledby` per specificare che una descrizione chiave contenuta in un {{htmlelement("div")}} è l'etichetta per più celle di tabella, o potrebbe essere utilizzato come alternativa al testo alternativo delle immagini — specificare informazioni esistenti sulla pagina come testo alt di un'immagine, piuttosto che doverlo ripetere all'interno dell'attributo `alt`. Puoi vedere un esempio di ciò in [Testi alternativi](/it/docs/Learn_web_development/Core/Accessibility/HTML#text_alternatives).
- Stati
  - : Proprietà speciali che definiscono le condizioni correnti degli elementi, come `aria-disabled="true"`, che specifica a un lettore di schermo che un input di modulo è attualmente disabilitato. Gli stati differiscono dalle proprietà in quanto le proprietà non cambiano nel ciclo di vita di un'app, mentre gli stati possono cambiare, generalmente a livello programmatico tramite JavaScript.

Un punto importante riguarda gli attributi WAI-ARIA: essi non influenzano nulla della pagina web, a parte le informazioni esposte dalle API di accessibilità del browser (dove i lettori di schermo ottengono le loro informazioni). WAI-ARIA non influisce sulla struttura della pagina web, sul DOM, ecc., sebbene gli attributi possano essere utili per selezionare elementi tramite CSS.

> [!NOTE]
> Puoi trovare un utile elenco di tutti i ruoli ARIA e dei loro usi, con link a ulteriori informazioni, nella specifica WAI-ARIA — consulta [Definizione dei ruoli](https://www.w3.org/TR/wai-aria-1.1/#role_definitions) — su questo sito — consulta [Ruoli ARIA](/it/docs/Web/Accessibility/ARIA/Reference/Roles).
>
> La specifica contiene anche un elenco di tutte le proprietà e gli stati, con link a ulteriori informazioni — consulta [Definizioni degli stati e delle proprietà (tutti gli attributi `aria-*`)](https://www.w3.org/TR/wai-aria-1.1/#state_prop_def).

### Dove è supportato WAI-ARIA?

Questa non è una domanda facile a cui rispondere. È difficile trovare una risorsa definitiva che dichiari quali funzionalità di WAI-ARIA sono supportate e dove, perché:

1. Ci sono molte funzionalità nella specifica WAI-ARIA.
2. Ci sono molte combinazioni di sistemi operativi, browser e lettori di schermo da considerare.

Questo ultimo punto è cruciale — Per utilizzare un lettore di schermo in primo luogo, il tuo sistema operativo deve eseguire browser che abbiano le API di accessibilità necessarie per esporre le informazioni di cui i lettori di schermo hanno bisogno per svolgere il loro compito. La maggior parte dei sistemi operativi popolari ha uno o due browser che i lettori di schermo possono utilizzare. Paciello Group ha un post abbastanza aggiornato che fornisce dati a riguardo — consulta [Rough Guide: browsers, operating systems and screen reader support updated](https://www.tpgi.com/rough-guide-browsers-operating-systems-and-screen-reader-support-updated/).

Successivamente, devi preoccuparti di se i browser in questione supportano le funzionalità ARIA e le espongono tramite le loro API, ma anche se i lettori di schermo riconoscono tali informazioni e le presentano ai loro utenti in modo utile.

1. Il supporto nei browser è quasi universale.
2. Il supporto dei lettori di schermo per le funzionalità ARIA non è ancora a questo livello, ma i lettori di schermo più popolari stanno arrivandoci. Puoi farti un’idea dei livelli di supporto guardando l'articolo [WAI-ARIA Screen reader compatibility](https://www.powermapper.com/tests/screen-readers/aria/) di Powermapper.

In questo articolo, non tenteremo di coprire ogni funzione WAI-ARIA e i suoi dettagli di supporto esatti. Invece, copriremo le caratteristiche WAI-ARIA più critiche che devi conoscere; se non menzioniamo alcun dettaglio di supporto, puoi assumere che la funzione è ben supportata. Menzioneremo chiaramente eventuali eccezioni a questo.

> [!NOTE]
> Alcune librerie JavaScript supportano WAI-ARIA, il che significa che quando generano funzioni UI come controlli di modulo complessi, aggiungono attributi ARIA per migliorare l'accessibilità di tali funzioni. Se stai cercando una soluzione JavaScript di terze parti per uno sviluppo rapido delle interfacce utente, dovresti sicuramente considerare l'accessibilità dei suoi widget UI come un fattore importante nella tua scelta. Buoni esempi sono jQuery UI (vedi [About jQuery UI: Deep accessibility support](https://jqueryui.com/about/#deep-accessibility-support)), [ExtJS](https://www.sencha.com/products/extjs/), e [Dojo/Dijit](https://dojotoolkit.org/reference-guide/1.10/dijit/a11y/statement.html).

### Quando dovresti usare WAI-ARIA?

Abbiamo parlato di alcuni dei problemi che hanno indotto la creazione di WAI-ARIA in precedenza, ma essenzialmente, ci sono quattro aree principali in cui WAI-ARIA è utile:

- Segnaletica/Punti di riferimento
  - : I valori dell'attributo [`role`](/it/docs/Web/Accessibility/ARIA/Reference/Roles) di ARIA possono fungere da punti di riferimento che replicano la semantica degli elementi HTML (es. {{htmlelement("nav")}}), o vanno oltre la semantica HTML per fornire segnaletica alle diverse aree funzionali, ad esempio `search`, `tablist`, `tab`, `listbox`, ecc.
- Aggiornamenti dinamici del contenuto
  - : I lettori di schermo tendono a incontrare difficoltà nel riportare contenuti che cambiano costantemente; con ARIA possiamo usare `aria-live` per informare gli utenti di lettori di schermo quando un'area di contenuto viene aggiornata dinamicamente: ad esempio, da JavaScript nella pagina [che recupera nuovi contenuti dal server e aggiorna il DOM](/it/docs/Learn_web_development/Core/Scripting/Network_requests).
- Miglioramento dell'accessibilità tramite tastiera
  - : Ci sono elementi HTML integrati che hanno nativamente accessibilità tramite tastiera; quando gli altri elementi vengono usati insieme a JavaScript per simulare interazioni simili, l'accessibilità tramite tastiera e il reporting del lettore di schermo ne soffrono come risultato. Dove questo è inevitabile, WAI-ARIA offre un mezzo per permettere ad altri elementi di ricevere il focus (usando `tabindex`).
- Accessibilità di controlli non semantici
  - : Quando una serie di `<div>` annidati insieme a CSS/JavaScript viene utilizzata per creare una funzione UI complessa, o un controllo nativo è notevolmente migliorato/modificato tramite JavaScript, l'accessibilità può risentirne — gli utenti di lettori di schermo troveranno difficile capire cosa fa la funzione se non ci sono semantiche o altri indizi. In queste situazioni, ARIA può aiutare a fornire quello che manca con una combinazione di ruoli come `button`, `listbox`, o `tablist`, e proprietà come `aria-required` o `aria-posinset` per fornire ulteriori indizi sulla funzionalità.

#### Dovresti usare WAI-ARIA solo quando è necessario!

Utilizzando gli elementi HTML corretti si ottengono implicitamente i ruoli necessari e si dovrebbe _sempre_ usare [le funzionalità HTML native](/it/docs/Learn_web_development/Core/Accessibility/HTML) per fornire la semantica richiesta dai lettori di schermo per dire ai loro utenti cosa sta succedendo. A volte questo non è possibile, sia perché hai un controllo limitato sul codice, sia perché stai creando qualcosa di complesso che non ha un elemento HTML facile da implementare. In tali casi, WAI-ARIA può essere uno strumento di miglioramento dell'accessibilità prezioso.

Ma ancora, usalo solo quando è necessario!

> [!NOTE]
> Inoltre, cerca di assicurarti di testare il tuo sito con una varietà di _utenti reali_ — persone non disabili, persone che usano lettori di schermo, persone che usano la navigazione tramite tastiera, ecc. Avranno intuizioni migliori di te su quanto bene funzioni.

## Implementazioni pratiche di WAI-ARIA

Nella sezione successiva, esamineremo le quattro aree in maggior dettaglio, insieme a esempi pratici. Prima di continuare, dovresti mettere in atto una configurazione per testare i lettori di schermo, in modo da poter testare alcuni degli esempi mentre procedi.

Consulta la nostra sezione su [testare i lettori di schermo](/it/docs/Learn_web_development/Core/Accessibility/Tooling#screen_readers) per maggiori informazioni.

### Segnaletica/Punti di riferimento

WAI-ARIA aggiunge l'attributo [`role`](https://www.w3.org/TR/wai-aria-1.1/#role_definitions) ai browser, che ti permette di aggiungere valore semantico extra agli elementi sul tuo sito ovunque siano necessari. La prima area maggiore in cui questo è utile è fornendo informazioni ai lettori di schermo affinché i loro utenti possano trovare elementi comuni della pagina. Questo esempio ha la struttura seguente:

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

Se provi a testare l'esempio con un lettore di schermo in un browser moderno, otterrai già alcune informazioni utili. Ad esempio, VoiceOver ti fornisce il seguente:

- Sull'elemento `<header>` — "banner, 2 elementi" (contiene un'intestazione e il `<nav>`).
- Sull'elemento `<nav>` — "navigazione 2 elementi" (contiene un elenco e un modulo).
- Sull'elemento `<main>` — "principale 2 elementi" (contiene un articolo e un aside).
- Sull'elemento `<aside>` — "complementare 2 elementi" (contiene un'intestazione e un elenco).
- Sull'input del modulo di ricerca — "Query di ricerca, inserimento all'inizio del testo".
- Sull'elemento `<footer>` — "footer 1 elemento".

Se apri il menu dei punti di riferimento di VoiceOver (accessibile usando il tasto VoiceOver + U e poi utilizzando i tasti cursore per scorrere tra le scelte del menu), vedrai che la maggior parte degli elementi è elencata in modo da poter essere acceduta rapidamente.

![Menu di VoiceOver sul Mac per un'accessibilità rapida. Intestazione dei punti di riferimento e elenco dei punti di riferimento inclusi banner, navigazione, principale e complementare.](landmarks-list.png)

Tuttavia, potremmo fare meglio qui. Il modulo di ricerca è un punto di riferimento veramente importante che le persone vorranno trovare, ma non è elencato nel menu dei punti di riferimento né trattato come un punto di riferimento noto al di là dell'input stesso chiamato come un input di ricerca (`<input type="search">`).

Potremmo migliorarlo utilizzando il ruolo ARIA `role="search"`, ma usando l'elemento {{htmlelement("search")}} si ottiene implicitamente quel ruolo nel modulo.

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

La cosa più importante è che abbiamo utilizzato HTML semantico che dà significato e ruoli alla struttura della pagina senza aggiungere attributi di [`role`](/it/docs/Web/Accessibility/ARIA/Reference/Roles) non necessari alla nostra struttura HTML, che ha una struttura come questa:

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

Ti abbiamo anche dato una funzione bonus in questo esempio — l'elemento {{htmlelement("input")}} è stato fornito dell'attributo [`aria-label`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-label), che offre una descrizione dell'etichetta da leggere da un lettore di schermo, anche se non abbiamo incluso un elemento {{htmlelement("label")}}. In casi come questi, questo è molto utile — un modulo di ricerca come questo è una funzione molto comune e facilmente riconoscibile, e aggiungere un'etichetta visiva rovinerebbe il design della pagina.

```html
<input
  type="search"
  name="q"
  placeholder="Search query"
  aria-label="Search through site content" />
```

Ora, se utilizzi VoiceOver per guardare questo esempio, ottieni alcuni miglioramenti:

- Il modulo di ricerca viene indicato come un elemento separato, sia durante la navigazione della pagina, sia nel menu dei Punti di riferimento.
- Il testo dell'etichetta contenuto nell'attributo `aria-label` viene letto quando l'input del modulo è evidenziato.

Se hai bisogno di supportare browser più vecchi come IE8, vale la pena includere i ruoli ARIA a tale scopo. E se per qualche motivo il tuo sito è costruito usando solo `<div>`, dovresti sicuramente includere i ruoli ARIA per fornire queste tanto necessarie semantiche!

Vedrai molto di più su queste semantiche e sulla potenza delle proprietà/attributi ARIA qui sotto, specialmente nella sezione [Accessibilità di controlli non semantici](#accessibilità_di_controlli_non-semantici). Per ora, però, vediamo come ARIA può aiutare con gli aggiornamenti dinamici del contenuto.

### Aggiornamenti dinamici del contenuto

Il contenuto caricato nel DOM può essere facilmente accessibile usando un lettore di schermo, dal contenuto testuale ai testi alternativi allegati alle immagini. I siti web tradizionali statici con contenuti ampiamente testuali sono quindi facili da rendere accessibili per le persone con disabilità visive.

Il problema è che le moderne app web spesso non sono solo testo statico — spesso aggiornano parti della pagina recuperando nuovi contenuti dal server (in questo esempio stiamo utilizzando un array statico di citazioni) e aggiornando il DOM. Queste sono talvolta chiamate **regioni dal vivo**.

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

Questo funziona bene, ma non è buono per l'accessibilità — l'aggiornamento del contenuto non viene rilevato dai lettori di schermo, quindi i loro utenti non saprebbero cosa sta succedendo. Questo è un esempio piuttosto banale, ma immagina solo se stessi creando un'interfaccia utente complessa con molti contenuti che si aggiornano costantemente, come una chat room, o un'interfaccia utente di un gioco di strategia, o un display di carrello della spesa che si aggiorna in tempo reale — sarebbe impossibile utilizzare l'app in modo efficace senza un qualche tipo di modo per avvisare l'utente degli aggiornamenti.

WAI-ARIA, fortunatamente, fornisce un meccanismo utile per fornire questi avvisi — la proprietà [`aria-live`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-live). Applicandola a un elemento si fa sì che i lettori di schermo leggano il contenuto aggiornato. Quanto urgentemente il contenuto viene letto dipende dal valore dell'attributo:

- `off`
  - : L'impostazione predefinita. Gli aggiornamenti non dovrebbero essere annunciati.
- `polite`
  - : Gli aggiornamenti dovrebbero essere annunciati solo se l'utente è inattivo.
- `assertive`
  - : Gli aggiornamenti dovrebbero essere annunciati all'utente appena possibile.

Qui aggiorniamo il tag di apertura `<section>` come segue:

```html
<section aria-live="assertive">…</section>
```

Questo farà sì che un lettore di schermo legga il contenuto mentre viene aggiornato.

C'è una considerazione aggiuntiva qui — solo il pezzo di testo che si aggiorna viene letto. Sarebbe bello se si leggesse sempre anche l'intestazione, in modo che l'utente possa ricordare cosa viene letto. Per fare questo, possiamo aggiungere la proprietà [`aria-atomic`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-atomic) alla sezione. Aggiorna nuovamente il tuo tag di apertura `<section>`, così:

```html
<section aria-live="assertive" aria-atomic="true">…</section>
```

L'attributo `aria-atomic="true"` dice ai lettori di schermo di leggere l'intero contenuto dell'elemento come un'unica unità atomica, non solo i pezzi che sono stati aggiornati.

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
> La proprietà [`aria-relevant`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-relevant) è anche piuttosto utile per controllare cosa viene letto quando una regione dal vivo viene aggiornata. Puoi ad esempio far sì che solo le aggiunte o rimozioni di contenuto vengano lette.

### Migliorare l'accessibilità tramite tastiera

Come discusso in altri punti del modulo, uno dei punti di forza principali di HTML rispetto all'accessibilità è l'accessibilità tramite tastiera integrata delle funzioni come pulsanti, controlli di modulo e link. In generale, puoi usare il tasto tab per spostarti tra i controlli, il tasto Invio/Return per selezionare o attivare i controlli, e occasionalmente altri controlli come necessario (ad esempio la freccia su e giù per spostarsi tra le opzioni in un box `<select>`).

Tuttavia, a volte dovrai scrivere codice che utilizza elementi non semantici come pulsanti (o altri tipi di controllo), o utilizza controlli focalizzabili per scopi non proprio corretti. Potresti cercare di correggere del codice non buono che hai ereditato, o potresti costruire un tipo di widget complesso che lo richiede.

In termini di rendere focalizzabile il codice non focalizzabile, WAI-ARIA estende l'attributo `tabindex` con alcuni nuovi valori:

- `tabindex="0"` — come indicato sopra, questo valore consente agli elementi che normalmente non sono tababili di diventare tababili. Questo è il valore più utile di `tabindex`.
- `tabindex="-1"` — questo consente agli elementi normalmente non tababili di ricevere il focus a livello programmatico, ad esempio tramite JavaScript, o come target di link.

Abbiamo discusso questo in modo più dettagliato e mostrato un'implementazione tipica nel nostro articolo sull'accessibilità HTML — vedi [Costruire nuovamente l'accessibilità tramite tastiera](/it/docs/Learn_web_development/Core/Accessibility/HTML#costruire_nuovamente_l'accessibilità_tramite_tastiera).

### Accessibilità di controlli non semantici

Questo segue dalla sezione precedente — quando una serie di `<div>` annidati insieme a CSS/JavaScript viene utilizzata per creare una funzione UI complessa, o un controllo nativo è notevolmente migliorato/modificato tramite JavaScript, non solo l'accessibilità tramite tastiera può soffrirne, ma gli utenti di lettori di schermo troveranno difficile capire cosa fa la funzione se non ci sono semantiche o altri indizi. In tali situazioni, ARIA può aiutare a fornire quelle semantiche mancanti.

#### Validazione del modulo e avvisi di errore

Prima di tutto, rivisitiamo l'esempio di modulo che abbiamo visto per la prima volta nel nostro articolo sull'accessibilità CSS e JavaScript (leggi [Mantenerlo discreto](/it/docs/Learn_web_development/Core/Accessibility/CSS_and_JavaScript#mantenerlo_discreto) per un riassunto completo). Alla fine di questa sezione, abbiamo mostrato di aver incluso alcuni attributi ARIA sulla casella del messaggio di errore che visualizza eventuali errori di validazione quando tenti di inviare il modulo:

```html
<div class="errors" role="alert" aria-relevant="all">
  <ul></ul>
</div>
```

- [`role="alert"`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/alert_role) trasforma automaticamente l'elemento a cui è applicato in una regione dal vivo, quindi i cambiamenti verranno letti; identifica anche semanticamente come un messaggio di avviso (informazioni importanti sensibili al tempo/contenuto), e rappresenta un modo migliore, più accessibile, di fornire un avviso all'utente (le finestre di dialogo modale come le chiamate [`alert()`](/it/docs/Web/API/Window/alert) hanno alcuni problemi di accessibilità; vedi [Finestra di Popup](https://webaim.org/techniques/javascript/other#popups) da WebAIM).
- Un valore [`aria-relevant`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-relevant) di `all` istruisce il lettore di schermo a leggere il contenuto dell'elenco di errori quando vengono apportate modifiche ad esso — ad esempio, quando vengono aggiunti o rimossi errori. Questo è utile perché l'utente vorrà sapere quali errori rimangono, non solo cosa è stato aggiunto o rimosso dall'elenco.

Potremmo andare oltre con il nostro utilizzo di ARIA, e fornire un maggiore aiuto nella validazione. Che ne dici di indicare se i campi sono richiesti in primo luogo, e quale dovrebbe essere l'intervallo dell'età?

1. A questo punto, prendi una copia dei nostri file [`form-validation.html`](https://github.com/mdn/learning-area/blob/main/accessibility/css/form-validation.html) e [`validation.js`](https://github.com/mdn/learning-area/blob/main/accessibility/css/validation.js), e salvali in una directory locale.
2. Aprili entrambi in un editor di testo e dai un'occhiata a come funziona il codice.
3. Prima di tutto, aggiungi un paragrafo appena sopra l'apertura del tag `<form>`, come quello qui sotto, e marca entrambe le etichette del modulo `<label>` con un asterisco. Questo è normalmente il modo in cui contrassegniamo i campi richiesti per gli utenti vedenti.

   ```html
   <p>Fields marked with an asterisk (*) are required.</p>
   ```

4. Questo ha senso visivo, ma non è così facile da capire per gli utenti di lettori di schermo. Fortunatamente, WAI-ARIA fornisce l'attributo [`aria-required`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-required) per dare indicazioni ai lettori di schermo che dovrebbero informare gli utenti che gli input del modulo devono essere compilati. Aggiorna gli elementi `<input>` così:

   ```html
   <input type="text" name="name" id="name" aria-required="true" />

   <input type="number" name="age" id="age" aria-required="true" />
   ```

5. Se salvi l'esempio ora e lo testi con un lettore di schermo, dovresti sentire qualcosa come "Inserisci il tuo nome stella, richiesto, modifica testo".
6. Potrebbe anche essere utile fornire agli utenti dei lettori di schermo e agli utenti vedenti un'idea di quale dovrebbe essere il valore dell'età. Questo viene spesso presentato come un tooltip o un placeholder all'interno del campo di modulo. WAI-ARIA include le proprietà [`aria-valuemin`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-valuemin) e [`aria-valuemax`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-valuemax) per specificare valori min e max, e i lettori di schermo supportano gli attributi nativi `min` e `max`. Un'altra funzionalità ben supportata è l'attributo HTML `placeholder`, che può contenere un messaggio che viene mostrato nell'input quando non viene inserito alcun valore e viene letto da alcuni lettori di schermo. Aggiorna il tuo input numerico in questo modo:

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

Includi sempre un {{HTMLelement('label')}} per ogni input. Mentre alcuni lettori di schermo annunciano il testo del placeholder, la maggior parte non lo fa. Sostituzioni accettabili per fornire ai controlli del modulo un nome accessibile includono [`aria-label`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-label) e [`aria-labelledby`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-labelledby). Ma l'elemento `<label>` con un attributo `for` è il metodo preferito in quanto fornisce usabilità per tutti gli utenti, inclusi gli utenti del mouse.

> [!NOTE]
> Puoi vedere l'esempio finito dal vivo su [`form-validation-updated.html`](https://mdn.github.io/learning-area/accessibility/aria/form-validation-updated.html).

WAI-ARIA consente anche alcune tecniche avanzate di etichettatura dei moduli, oltre al classico elemento {{htmlelement("label")}}. Abbiamo già parlato dell'uso della proprietà [`aria-label`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-label) per fornire un'etichetta dove non vogliamo che l'etichetta sia visibile agli utenti vedenti (vedi la sezione [Segnaletica/Punti di riferimento](#signpostslandmarks) sopra). Alcune altre tecniche di etichettatura usano altre proprietà come [`aria-labelledby`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-labelledby) se vuoi designare un elemento non-`<label>` come etichetta o etichettare molteplici input di modulo con la stessa etichetta, e [`aria-describedby`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-describedby), se vuoi associare altre informazioni a un input di modulo e farle leggere. Vedi l'articolo [Tecniche Avanzate di Etichettatura dei Moduli](https://webaim.org/techniques/forms/advanced) di WebAIM per ulteriori dettagli.

Ci sono molte altre proprietà e stati utili too, per indicare lo stato degli elementi di modulo. Ad esempio, `aria-disabled="true"` può essere usato per indicare che un campo del modulo è disabilitato. Molti browser ignoreranno i campi del modulo disabilitati, il che porta a non farli leggere dai lettori di schermo. In alcuni casi, un elemento disabilitato sarà percepito, quindi è una buona idea includere questo attributo per far sapere al lettore di schermo che un controllo del modulo disabilitato è effettivamente disabilitato.

Se è probabile che lo stato disabilitato di un input cambi, allora è anche una buona idea indicare quando avviene, e quale sia il risultato. Ad esempio, nel nostro demo [`form-validation-checkbox-disabled.html`](https://mdn.github.io/learning-area/accessibility/aria/form-validation-checkbox-disabled.html), c'è un checkbox che, quando viene spuntato, abilita un altro input del modulo per consentire di inserire ulteriori informazioni. Abbiamo impostato una regione dal vivo nascosta:

```html
<p class="hidden-alert" aria-live="assertive"></p>
```

che è nascosta dalla vista usando il posizionamento assoluto. Quando viene spuntata/non spuntata, aggiorniamo il testo all'interno della regione dal vivo nascosta per dire agli utenti dei lettori di schermo qual è il risultato dello spuntamento di questo checkbox, oltre ad aggiornare lo stato `aria-disabled` e alcuni indicatori visivi.

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

Alcune volte in questo corso, abbiamo menzionato l'accessibilità nativa (e i problemi di accessibilità dietro l'utilizzo di altri elementi per simulare) di pulsanti, link, o elementi di modulo (vedi [Usa controlli UI semantici ove possibile](/it/docs/Learn_web_development/Core/Accessibility/HTML#use_semantic_ui_controls_where_possible) nell'articolo sull'accessibilità HTML, e [Migliorare l'accessibilità tramite tastiera](#migliorare_l'accessibilità_tramite_tastiera), sopra). Fondamentalmente, puoi aggiungere l'accessibilità tramite tastiera senza troppi problemi in molti casi, usando `tabindex` e un po' di JavaScript.

Ma per quanto riguarda i lettori di schermo? Loro non vedranno ancora gli elementi come pulsanti. Se testiamo il nostro esempio di [`fake-div-buttons.html`](https://mdn.github.io/learning-area/tools-testing/cross-browser-testing/accessibility/fake-div-buttons.html) in un lettore di schermo, i nostri finti pulsanti verranno riportati con frasi come "Click me!, group", il che ovviamente è confondente.

Possiamo risolvere questo problema utilizzando un ruolo WAI-ARIA. Fai una copia locale di [`fake-div-buttons.html`](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/accessibility/fake-div-buttons.html), e aggiungi [`role="button"`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/button_role) ad ogni pulsante `<div>`, ad esempio:

```html
<div data-message="This is from the first button" tabindex="0" role="button">
  Click me!
</div>
```

Ora, quando provi questo usando un lettore di schermo, avrai i pulsanti riportati con frasi come "Click me!, button". Mentre è molto meglio, devi ancora aggiungere tutte le funzionalità native dei pulsanti che gli utenti si aspetterebbero, come gestire gli eventi <kbd>enter</kbd> e click, come spiegato nella documentazione del [`ruolo button`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/button_role).

> [!NOTE]
> Non dimenticare comunque che utilizzare l'elemento semantico corretto ove possibile è sempre meglio. Se vuoi creare un pulsante, e puoi usare un elemento {{htmlelement("button")}}, dovresti usare un elemento {{htmlelement("button")}}!

#### Guidare gli utenti attraverso widget complessi

Ci sono una serie di altri [ruoli](/it/docs/Web/Accessibility/ARIA/Reference/Roles) che possono identificare strutture di elementi non semantici come caratteristiche comuni dell'interfaccia utente che vanno oltre quello disponibile nell'HTML standard, ad esempio [`combobox`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/combobox_role), [`slider`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/slider_role), [`tabpanel`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/tabpanel_role), [`tree`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/tree_role). Puoi vedere diversi esempi utili nella [libreria di codice dell'università Deque](https://dequeuniversity.com/library/) per darti un'idea di come tali controlli possano essere resi accessibili.

Esaminiamo un esempio nostro. Torniamo alla nostra semplice interfaccia a schede posizionata in modo assoluto (vedi [Nascondere le cose](/it/docs/Learn_web_development/Core/Accessibility/CSS_and_JavaScript#nascondere_le_cose) nel nostro articolo sull'accessibilità CSS e JavaScript), che puoi trovare nell'[esempio dell'Infobox a schede](/it/docs/Learn/CSS/Building/alto).

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

      this.firstTab ??= tab;
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

In questo esempio abbiamo utilizzato una combinazione di elementi semantici, ruoli aria e attributi aria. Il primo di questi è l'utilizzo di un elemento {{htmlelement("button")}} come una _scheda_, il che significa che la scheda può essere selezionata tramite un clic del mouse o tramite la tastiera usando spazio o invio.

Le funzionalità ARIA utilizzate includono:

- Nuovi ruoli — [`tablist`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/tablist_role), [`tab`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/tab_role), [`tabpanel`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/tabpanel_role)
  - : Questi identificano le aree importanti dell'interfaccia a schede — il contenitore per le schede, le schede stesse e i tabpanels corrispondenti.
- [`aria-selected`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-selected)
  - : Definisce quale scheda è attualmente selezionata. Poiché diverse schede sono selezionate dall'utente, il valore di questo attributo sulle diverse schede viene aggiornato tramite JavaScript.
- `tabindex="-1"`
  - : `tabindex="-1"` rimuove l'elemento dall'ordine di tabulazione. Poiché stiamo utilizzando JavaScript per consentire all'utente di controllare le schede tramite tastiera o mouse non vogliamo che l'utente possa usare il tasto tab per navigare ai pulsanti.
- [`aria-labelledby`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-labelledby)
  - : Questo attributo identifica un elemento (mediante il suo `id`) che etichetta l'elemento, in questo esempio l'articolo `<article>` è etichettato dalla scheda corrispondente o `<button>`.
- [`aria-controls`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-controls)
  - : Questo attributo identifica un elemento (mediante il suo `id`) che è controllato dall'elemento, in questo esempio l'articolo `<article>` è controllato dalla scheda corrispondente o `<button>`.

Potremmo aver utilizzato `aria-hidden` per nascondere il contenuto dei tabpanels dalle tecnologie assistive ma se tale contenuto conteneva contenuti focalizzabili, come i link, l'utente sarebbe comunque in grado di tabulare a quel contenuto anche quando `aria-hidden=true` è impostato per i pannelli non attivi. In questo esempio abbiamo applicato `class="is-hidden"` ai tabpanels che corrispondono alle schede con `aria-selected="false"` e utilizzando il CSS per `display: none;` che impedisce il tabulatore al contenuto nascosto.

Nei nostri test, questa nuova struttura ha servito a migliorare le cose complessivamente. I `<button>` sono ora riconosciuti come schede (ad esempio, "tab" è pronunciato dal lettore di schermo), la scheda selezionata è indicata da "selezionato" che viene letto con il nome della scheda e qualsiasi contenuto che non viene mostrato non può essere tabulato. L'utente può anche navigare tra le schede con tastiera o mouse.

## Metti alla prova le tue abilità!

Hai raggiunto la fine di questo articolo, ma puoi ricordare le informazioni più importanti? Puoi trovare alcuni ulteriori test per verificare se hai conservato queste informazioni prima di andare avanti — vedi [Metti alla prova le tue abilità: WAI-ARIA](/it/docs/Learn_web_development/Core/Accessibility/Test_your_skills/WAI-ARIA).

## Sommario

Questo articolo non ha certo coperto tutto ciò che è disponibile in WAI-ARIA, ma dovrebbe averti dato abbastanza informazioni per capire come usarlo, e sapere quali sono i modelli più comuni che incontrerai che lo richiedono.

## Vedi anche

- [Stati e proprietà Aria](/it/docs/Web/Accessibility/ARIA/Reference/Attributes): Tutti gli attributi `aria-*`
- [Ruoli WAI-ARIA](/it/docs/Web/Accessibility/ARIA/Reference/Roles): Categorie di ruoli ARIA e i ruoli trattati su MDN
- [ARIA in HTML](https://www.w3.org/TR/html-aria/) su W3C: Una specifica che definisce, per ogni caratteristica HTML, le semantiche di accessibilità (ARIA) applicate implicitamente su di essa dal browser e le funzionalità WAI-ARIA che puoi impostare su di essa se sono richieste semantiche extra
- [Libreria di codice università Deque](https://dequeuniversity.com/library/): Una libreria di esempi davvero utili e pratici che mostrano controlli UI complessi resi accessibili utilizzando le funzionalità WAI-ARIA
- [Linee guida sull'autore WAI-ARIA](https://www.w3.org/WAI/ARIA/apg/) su W3C: Un pattern di design molto dettagliato dal W3C, che spiega come implementare diversi tipi di controllo UI complesso rendendoli accessibili utilizzando le funzionalità WAI-ARIA

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/CSS_and_JavaScript","Learn_web_development/Core/Accessibility/Multimedia", "Learn_web_development/Core/Accessibility")}}
