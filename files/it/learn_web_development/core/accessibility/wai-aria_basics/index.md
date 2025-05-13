---
title: Nozioni di base su WAI-ARIA
short-title: WAI-ARIA
slug: Learn_web_development/Core/Accessibility/WAI-ARIA_basics
l10n:
  sourceCommit: a1ac64fa4da965d2a152f08221b1a9aed638fd16
---

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/CSS_and_JavaScript","Learn_web_development/Core/Accessibility/Multimedia", "Learn_web_development/Core/Accessibility")}}

Continuando dall'articolo precedente, a volte può essere difficile creare controlli complessi dell'interfaccia utente che coinvolgono HTML non semantico e contenuti aggiornati dinamicamente in JavaScript. WAI-ARIA è una tecnologia che può aiutare con tali problemi aggiungendo ulteriori semantiche che i browser e le tecnologie assistive possono riconoscere e utilizzare per informare gli utenti su cosa sta succedendo. Qui mostreremo come usarla a un livello base per migliorare l'accessibilità.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Familiarità con <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e le migliori pratiche di accessibilità insegnate nelle lezioni precedenti del modulo.</a>.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Lo scopo di WAI-ARIA — fornire semantica a HTML altrimenti non semantico, in modo che gli utenti di tecnologia assistiva possano comprendere le interfacce presentate loro.</li>
          <li>La sintassi di base — ruoli, proprietà e stati.</li>
          <li>Punti di riferimento e segnaletica.</li>
          <li>Migliorare l'accessibilità tramite tastiera.</li>
          <li>Annunciare aggiornamenti di contenuti dinamici con regioni live.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Cos'è WAI-ARIA?

Iniziamo esaminando cos'è WAI-ARIA e cosa può fare per noi.

### Un intero nuovo set di problemi

Con il rendersi sempre più complessi e dinamici delle applicazioni web, sono emerse nuove funzionalità e problemi di accessibilità.

Ad esempio, HTML ha introdotto una serie di elementi semantici per definire funzioni comuni della pagina ({{htmlelement("nav")}}, {{htmlelement("footer")}}, ecc.). Prima che questi fossero disponibili, gli sviluppatori usavano {{htmlelement("div")}}s con ID o classi, per esempio, `<div class="nav">`, ma questi erano problematici, poiché non c'era modo semplice di trovare facilmente una funzione specifica della pagina come la navigazione principale in modo programmatico.

La soluzione iniziale era aggiungere uno o più link nascosti nella parte superiore della pagina per collegarsi alla navigazione (o altro), per esempio:

```html
<a href="#hidden" class="hidden">Skip to navigation</a>
```

Ma questo non è ancora molto preciso e può essere utilizzato solo quando il lettore dello schermo sta leggendo dall'inizio della pagina.

Un altro esempio è costituito da applicazioni che iniziano a presentare controlli complessi come selezionatori di date per scegliere le date, cursori per scegliere i valori, ecc. HTML fornisce tipi di input speciali per visualizzare tali controlli:

```html
<input type="date" /> <input type="range" />
```

Questi inizialmente non erano ben supportati e, come ancora in parte lo sono, era ed è ancora difficile stilizzarli, portando designer e sviluppatori a optare per soluzioni personalizzate. Invece di utilizzare queste funzionalità native, alcuni sviluppatori si affidano a librerie JavaScript che generano tali controlli come una serie di {{htmlelement("div")}} annidati che vengono poi stilizzati utilizzando CSS e controllati utilizzando JavaScript.

Il problema qui è che visivamente funzionano, ma i lettori dello schermo non riescono a capire affatto cosa siano, e i loro utenti vengono informati che vedono un miscuglio di elementi senza alcuna semantica per descrivere cosa significano.

### Introduzione a WAI-ARIA

[WAI-ARIA](https://www.w3.org/TR/wai-aria/) (Web Accessibility Initiative - Accessible Rich Internet Applications) è una specifica scritta dal W3C, che definisce un insieme di ulteriori attributi HTML che possono essere applicati agli elementi per fornire ulteriori semantiche e migliorare l'accessibilità laddove manca. Nella specifica sono definite tre caratteristiche principali:

- [Ruoli](/it/docs/Web/Accessibility/ARIA/Reference/Roles)
  - : Questi definiscono cosa sia o faccia un elemento. Molti di questi sono i cosiddetti ruoli di riferimento, che duplicano in gran parte il valore semantico degli elementi strutturali, come `role="navigation"` ({{htmlelement("nav")}}), `role="banner"` (documento {{htmlelement("header")}}), `role="complementary"` ({{htmlelement("aside")}}) o `role="search"` ({{htmlelement("search")}}). Altri ruoli descrivono diverse strutture di pagina che non hanno elementi abbinati a quei ruoli, come `role="tablist"` e `role="tabpanel"`, che si trovano comunemente nelle interfacce utente.
- Proprietà
  - : Queste definiscono le proprietà degli elementi, che possono essere utilizzate per assegnare loro un significato o una semantica extra. Ad esempio, `aria-required="true"` specifica che un input del modulo deve essere compilato per essere valido, mentre `aria-labelledby="label"` consente di mettere un ID su un elemento, quindi fare riferimento ad esso come l'etichetta per qualsiasi altra cosa sulla pagina, compresi più elementi, cosa che non è possibile usando `<label for="input">`. Come esempio, si potrebbe usare `aria-labelledby` per specificare che una descrizione chiave contenuta in un {{htmlelement("div")}} è l'etichetta per più celle di tabella, oppure usarla come alternativa al testo alternativo dell'immagine — specificare informazioni esistenti sulla pagina come testo alternativo di un'immagine, piuttosto che doverle ripetere all'interno dell'attributo `alt`. Può vedere un esempio di questo in [Testo alternativo](/it/docs/Learn_web_development/Core/Accessibility/HTML#text_alternatives).
- Stati
  - : Proprietà speciali che definiscono le condizioni correnti degli elementi, come `aria-disabled="true"`, che specifica a un lettore dello schermo che un input di un modulo è attualmente disabilitato. Gli stati differiscono dalle proprietà in quanto le proprietà non cambiano durante il ciclo di vita di un'app, mentre gli stati possono cambiare, generalmente in modo programmatico tramite JavaScript.

Un punto importante sugli attributi WAI-ARIA è che non influenzano nulla della pagina web, tranne che per le informazioni esposte dalle API di accessibilità del browser (da cui i lettori dello schermo ottengono le loro informazioni). WAI-ARIA non influenza la struttura della pagina web, il DOM, ecc., anche se gli attributi possono essere utili per selezionare elementi tramite CSS.

> [!NOTE]
> È possibile trovare un elenco utile di tutti i ruoli ARIA e i loro usi, con link a ulteriori informazioni, nella specifica WAI-ARIA: vedere [Definizione dei Ruoli](https://www.w3.org/TR/wai-aria-1.1/#role_definitions) su questo sito — vedere [Ruoli ARIA](/it/docs/Web/Accessibility/ARIA/Reference/Roles).
>
> La specifica contiene anche un elenco di tutte le proprietà e gli stati, con link a ulteriori informazioni — vedere [Definizioni di Stati e Proprietà (tutti gli attributi `aria-*`)](https://www.w3.org/TR/wai-aria-1.1/#state_prop_def).

### Dove è supportato WAI-ARIA?

Non è facile rispondere a questa domanda. È difficile trovare una risorsa conclusiva che dichiari quali funzioni di WAI-ARIA siano supportate e dove, perché:

1. Ci sono molte funzioni nella specifica WAI-ARIA.
2. Ci sono molte combinazioni di sistemi operativi, browser e lettori dello schermo da considerare.

Questo ultimo punto è fondamentale: per utilizzare un lettore dello schermo in primo luogo, il suo sistema operativo deve eseguire browser che abbiano le API di accessibilità necessarie per esporre le informazioni di cui i lettori dello schermo hanno bisogno per svolgere il loro lavoro. La maggior parte degli OS popolari hanno uno o due browser che possono funzionare con lettori dello schermo. Il Paciello Group ha un post abbastanza aggiornato che fornisce dati a riguardo - vedi [Rough Guide: browsers, operating systems and screen reader support updated](https://www.tpgi.com/rough-guide-browsers-operating-systems-and-screen-reader-support-updated/).

Successivamente, deve preoccuparsi se i browser in questione supportano le funzioni ARIA e le espongono tramite le loro API, ma anche se i lettori dello schermo riconoscono tali informazioni e le presentano ai loro utenti in modo utile.

1. Il supporto del browser è quasi universale.
2. Il supporto del lettore dello schermo per le funzioni ARIA non è ancora a questo livello, ma i lettori dello schermo più popolari ci stanno arrivando. Può avere un'idea dei livelli di supporto guardando l'articolo di Powermapper sulla [compatibilità con il lettore dello schermo WAI-ARIA](https://www.powermapper.com/tests/screen-readers/aria/).

In questo articolo, non tenteremo di coprire ciascuna funzione WAI-ARIA e i suoi dettagli di supporto esatti. Invece, copriremo le funzioni WAI-ARIA più critiche che deve conoscere; se non menzioniamo i dettagli di supporto, può assumere che la funzione sia ben supportata. Indicheremo chiaramente eventuali eccezioni a questo.

> [!NOTE]
> Alcune librerie JavaScript supportano WAI-ARIA, il che significa che quando generano funzionalità dell'interfaccia utente come controlli di modulo complessi, aggiungono attributi ARIA per migliorare l'accessibilità di tali funzionalità. Se sta cercando una soluzione JavaScript di terze parti per lo sviluppo rapido dell'interfaccia utente, dovrebbe sicuramente considerare l'accessibilità dei suoi widget dell'interfaccia utente come un fattore importante nella scelta. Buoni esempi sono jQuery UI (vedi [Informazioni su jQuery UI: Supporto approfondito dell'accessibilità](https://jqueryui.com/about/#deep-accessibility-support)), [ExtJS](https://www.sencha.com/products/extjs/), e [Dojo/Dijit](https://dojotoolkit.org/reference-guide/1.10/dijit/a11y/statement.html).

### Quando dovrebbe usare WAI-ARIA?

Abbiamo parlato di alcuni dei problemi che hanno portato alla creazione di WAI-ARIA in precedenza, ma essenzialmente, ci sono quattro aree principali in cui WAI-ARIA è utile:

- Segnaletica/Punti di Riferimento
  - : I valori dell'attributo [`role`](/it/docs/Web/Accessibility/ARIA/Reference/Roles) di ARIA possono fungere da punti di riferimento che replicano la semantica degli elementi HTML (ad esempio, {{htmlelement("nav")}}), o vanno oltre la semantica HTML per fornire segnaletica a varie aree funzionali, per esempio, `search`, `tablist`, `tab`, `listbox`, ecc.
- Aggiornamenti dinamici dei contenuti
  - : I lettori dello schermo tendono ad avere difficoltà nel segnalare contenuti che cambiano costantemente; con ARIA possiamo utilizzare `aria-live` per informare gli utenti del lettore dello schermo quando un'area di contenuto viene aggiornata in modo dinamico: per esempio, tramite JavaScript nella pagina [recuperando nuovi contenuti dal server e aggiornando il DOM](/it/docs/Learn_web_development/Core/Scripting/Network_requests).
- Miglioramento dell'accessibilità tramite tastiera
  - : Ci sono elementi HTML intrinseci che hanno un'accessibilità da tastiera nativa; quando altri elementi vengono utilizzati insieme a JavaScript per simulare interazioni simili, l'accessibilità tramite tastiera e il reporting del lettore dello schermo ne risentono. Dove ciò è inevitabile, WAI-ARIA fornisce un mezzo per permettere ad altri elementi di ricevere il focus (usando `tabindex`).
- Accessibilità dei controlli non semantici
  - : Quando si utilizza una serie di `<div>` annidati insieme a CSS/JavaScript per creare una funzione dell'interfaccia utente complessa, o un controllo nativo è notevolmente migliorato/cambiato tramite JavaScript, l'accessibilità può risentirne — gli utenti del lettore dello schermo troveranno difficile capire cosa fa la funzione se non ci sono semantiche o altri indizi. In queste situazioni, ARIA può aiutare a fornire ciò che manca con una combinazione di ruoli come `button`, `listbox`, o `tablist`, e proprietà come `aria-required` o `aria-posinset` per fornire ulteriori indizi sulla funzionalità.

#### Deve usare WAI-ARIA solo quando è necessario!

L'uso degli elementi HTML corretti implica implicitamente i ruoli necessari e dovrebbe _sempre_ utilizzare [funzionalità HTML native](/it/docs/Learn_web_development/Core/Accessibility/HTML) per fornire le semantiche richieste dai lettori dello schermo per dire ai loro utenti cosa sta succedendo. A volte, questo non è possibile, sia perché ha un controllo limitato sul codice, sia perché sta creando qualcosa di complesso che non ha un elemento HTML facile per implementarlo. In tali casi, WAI-ARIA può essere uno strumento prezioso per migliorare l'accessibilità.

Ma ancora, usalo solo quando è necessario!

> [!NOTE]
> Inoltre, provi a assicurarsi di testare il suo sito con una varietà di utenti _reali_ — persone non disabili, persone che utilizzano lettori dello schermo, persone che utilizzano la navigazione tramite tastiera, ecc. Avranno intuizioni migliori rispetto a lei su quanto bene funzioni.

## Implementazioni pratiche di WAI-ARIA

Nella sezione successiva, esamineremo le quattro aree in modo più dettagliato, insieme ad esempi pratici. Prima di continuare, dovrebbe mettere in atto un setup di test per lettori dello schermo, in modo da poter testare alcuni degli esempi mentre procede.

Vedi la nostra sezione sui [test dei lettori dello schermo](/it/docs/Learn_web_development/Core/Accessibility/Tooling#screen_readers) per maggiori informazioni.

### Segnaletica/Punti di Riferimento

WAI-ARIA aggiunge [`l'attributo role`](https://www.w3.org/TR/wai-aria-1.1/#role_definitions) ai browser, che permette di aggiungere valore semantico extra agli elementi del suo sito dovunque ne abbiano necessità. La prima area principale in cui ciò è utile è fornire informazioni per i lettori dello schermo in modo che i loro utenti possano trovare elementi di pagina comuni. Questo esempio ha la seguente struttura:

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

Se prova a testare l'esempio con un lettore dello schermo in un browser moderno, otterrà già alcune informazioni utili. Ad esempio, VoiceOver le darà quanto segue:

- Sull'elemento `<header>` — "banner, 2 oggetti" (contiene un heading e il `<nav>`).
- Sull'elemento `<nav>` — "navigazione 2 oggetti" (contiene un elenco e un modulo).
- Sull'elemento `<main>` — "principale 2 oggetti" (contiene un articolo e un aside).
- Sull'elemento `<aside>` — "complementare 2 oggetti" (contiene un heading e un elenco).
- Sull'input del form di ricerca — "Query di ricerca, inserimento all'inizio del testo".
- Sull'elemento `<footer>` — "footer 1 oggetto".

Se va al menu dei punti di riferimento di VoiceOver (accessibile usando il tasto VoiceOver + U e poi usando i tasti freccia per scorrere le scelte del menu), vedrà che la maggior parte degli elementi è elencata ordinatamente in modo che possano essere accessibili rapidamente.

![Menu VoiceOver di Mac per accessibilità rapida. Intestazione dei punti di riferimento e elenco dei punti di riferimento inclusi banner, navigazione, main e complementare.](landmarks-list.png)

Tuttavia, potremmo fare meglio qui. Il form di ricerca è un punto di riferimento molto importante che le persone vorranno trovare, ma non è elencato nel menu dei punti di riferimento né trattato come un punto di riferimento notevole oltre al fatto che l'input effettivo viene chiamato come input di ricerca (`<input type="search">`).

Potremmo migliorarlo usando il `role="search"` di ARIA, ma usando l'elemento {{htmlelement("search")}} si conferisce implicitamente tale ruolo al modulo.

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

La cosa più importante, abbiamo utilizzato HTML semantico che dà significato e ruoli alla struttura della pagina senza aggiungere inutili attributi [`role`](/it/docs/Web/Accessibility/ARIA/Reference/Roles) alla nostra struttura HTML, che ha una struttura come questa:

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

Ti abbiamo anche dato una funzione bonus in questo esempio — l'elemento {{htmlelement("input")}} è stato dotato dell'attributo [`aria-label`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-label), che gli assegna un'etichetta descrittiva da leggere dal lettore dello schermo, anche se non abbiamo incluso un elemento {{htmlelement("label")}}. In casi come questi, ciò è molto utile — un modulo di ricerca come questo è una funzione molto comune ed è facilmente riconoscibile, e aggiungere un'etichetta visiva rovinerebbe il design della pagina.

```html
<input
  type="search"
  name="q"
  placeholder="Search query"
  aria-label="Search through site content" />
```

Ora, se usiamo VoiceOver per guardare questo esempio, otteniamo alcuni miglioramenti:

- Il form di ricerca è chiamato come un elemento separato, sia quando si naviga attraverso la pagina, sia nel menu dei punti di riferimento.
- Il testo dell'etichetta contenuto nell'attributo `aria-label` viene letto quando l'input del modulo è evidenziato.

Se c'è bisogno di supportare browser più vecchi come IE8; è opportuno includere i ruoli ARIA a tale scopo. E se per qualsiasi motivo il suo sito è costruito usando solo `<div>`, dovrebbe sicuramente includere i ruoli ARIA per fornire queste semantiche tanto necessarie!

Vedrà molto di più su queste semantiche e il potere delle proprietà/attributi ARIA di seguito, specialmente nella sezione [Accessibilità dei controlli non semantici](#accessibilit%C3%A0_dei_controlli_non_semantici). Ma per ora, vediamo come ARIA può aiutare con gli aggiornamenti dinamici dei contenuti.

### Aggiornamenti dinamici dei contenuti

Il contenuto caricato nel DOM può essere facilmente accessibile usando un lettore dello schermo, dai contenuti testuali ai testi alternativi allegati alle immagini. Pertanto, i siti web statici tradizionali con contenuti in gran parte di testo sono facili da rendere accessibili per le persone con disabilità visive.

Il problema è che le moderne app web spesso non sono solo testo statico — spesso aggiornano parti della pagina recuperando nuovi contenuti dal server (in questo esempio stiamo usando un array statico di citazioni) e aggiornando il DOM. Questi a volte sono indicati come **regioni live**.

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

Questo funziona bene, ma non è buono per l'accessibilità — l'aggiornamento dei contenuti non viene rilevato dai lettori dello schermo, quindi i loro utenti non saprebbero cosa sta succedendo. Questo è un esempio piuttosto banale, ma immaginiamo solo se stesse creando un'interfaccia utente complessa con molti contenuti che si aggiornano costantemente, come una chat room, o un'interfaccia di un gioco di strategia, o un display del carrello della spesa che si aggiorna in tempo reale — sarebbe impossibile utilizzare l'app in modo efficace senza qualche tipo di modo di informare l'utente degli aggiornamenti.

Fortunatamente, WAI-ARIA fornisce un meccanismo utile per fornire questi avvisi — la proprietà [`aria-live`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-live). Applicandolo a un elemento, si fa sì che i lettori dello schermo leggano il contenuto che viene aggiornato. L'urgenza con cui il contenuto viene letto dipende dal valore dell'attributo:

- `off`
  - : Il valore predefinito. Gli aggiornamenti non dovrebbero essere annunciati.
- `polite`
  - : Gli aggiornamenti dovrebbero essere annunciati solo se l'utente è inattivo.
- `assertive`
  - : Gli aggiornamenti dovrebbero essere annunciati all'utente il più presto possibile.

Qui aggiorniamo il tag di apertura `<section>` come segue:

```html
<section aria-live="assertive">…</section>
```

Questo farà sì che un lettore dello schermo legga il contenuto man mano che viene aggiornato.

C'è un'ulteriore considerazione qui — solo il pezzo di testo che viene aggiornato viene letto. Potrebbe essere bello se leggessimo sempre anche l'intestazione, così l'utente può ricordare cosa viene letto. Per fare ciò, possiamo aggiungere la proprietà [`aria-atomic`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-atomic) alla sezione. Aggiorni il tag di apertura `<section>` di nuovo, in questo modo:

```html
<section aria-live="assertive" aria-atomic="true">…</section>
```

L'attributo `aria-atomic="true"` dice ai lettori dello schermo di leggere l'intero contenuto dell'elemento come un'unità atomica, non solo le parti che sono state aggiornate.

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
> La proprietà [`aria-relevant`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-relevant) è anche molto utile per controllare ciò che viene letto quando una regione live viene aggiornata. Può ad esempio far leggere solo le aggiunte o le rimozioni di contenuto.

### Miglioramento dell'accessibilità tramite tastiera

Come discusso in qualche altro punto del modulo, uno dei punti di forza chiave di HTML rispetto all'accessibilità è l'accessibilità tramite tastiera integrata in funzionalità come pulsanti, controlli di modulo e link. Generalmente, è possibile utilizzare il tasto tab per spostarsi tra i controlli, il tasto Invio/Return per selezionare o attivare i controlli e occasionalmente altri controlli come necessario (ad esempio il cursore su e giù per spostarsi tra le opzioni in un box `<select>`).

Tuttavia, a volte finirà per dover scrivere un codice che utilizza elementi non semantici come pulsanti (o altri tipi di controllo), o utilizza controlli focalizzabili per scopi non esattamente corretti. Potrebbe cercare di correggere del codice errato che ha ereditato, o potrebbe costruire un tipo di widget complesso che lo richiede.

In termini di rendere focalizzabile il codice non focalizzabile, WAI-ARIA estende l'attributo `tabindex` con alcuni nuovi valori:

- `tabindex="0"` — come indicato sopra, questo valore permette agli elementi che normalmente non sono tabulabili di diventare tabulabili. Questo è il valore più utile di `tabindex`.
- `tabindex="-1"` — questo consente agli elementi che normalmente non sono tabulabili di ricevere il focus in modo programmatico, ad esempio tramite JavaScript, o come obiettivo di link.

Abbiamo discusso questo in modo più dettagliato e mostrato una tipica implementazione nel nostro articolo sull'accessibilità dell'HTML — vedi [Ricostituzione dell'accessibilità tramite tastiera](/it/docs/Learn_web_development/Core/Accessibility/HTML#building_keyboard_accessibility_back_in).

### Accessibilità dei controlli non semantici

Questo segue la sezione precedente — quando si utilizza una serie di `<div>` annidati insieme a CSS/JavaScript per creare una funzione dell'interfaccia utente complessa, o si migliora/cambia notevolmente un controllo nativo tramite JavaScript, non solo l'accessibilità tramite tastiera può risentirne, ma gli utenti del lettore dello schermo troveranno difficile capire cosa fa la funzione se non ci sono semantiche o altri indizi. In tali situazioni, ARIA può aiutare a fornire quelle semantiche mancanti.

#### Validazione del modulo e avvisi di errore

Prima di tutto, ripassiamo l'esempio di modulo che abbiamo analizzato per la prima volta nel nostro articolo sull'accessibilità di CSS e JavaScript (legga [Mantenendo discreto](/it/docs/Learn_web_development/Core/Accessibility/CSS_and_JavaScript#keeping_it_unobtrusive) per un riepilogo completo). Alla fine di questa sezione, abbiamo mostrato di aver incluso alcuni attributi ARIA sulla casella del messaggio di errore che visualizza eventuali errori di validazione quando si prova a inviare il modulo:

```html
<div class="errors" role="alert" aria-relevant="all">
  <ul></ul>
</div>
```

- [`role="alert"`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/alert_role) trasforma automaticamente l'elemento a cui è applicato in una regione live, quindi i cambiamenti su di esso vengono letti; identifica anche semanticamente come un messaggio di avviso (informazioni importanti sensibili al tempo/contesto) e rappresenta un modo migliore e più accessibile di fornire un avviso a un utente (le finestre di dialogo modali come le chiamate [`alert()`](/it/docs/Web/API/Window/alert) hanno una serie di problemi di accessibilità; vedi [Popup Windows](https://webaim.org/techniques/javascript/other#popups) di WebAIM).
- Un valore di [`aria-relevant`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-relevant) di `all` istruisce il lettore dello schermo a leggere il contenuto dell'elenco degli errori quando vengono apportate modifiche ad esso — cioè, quando gli errori vengono aggiunti o rimossi. Questo è utile perché l'utente vorrà sapere quali errori rimangono, non solo cosa è stato aggiunto o rimosso dall'elenco.

Potremmo andare oltre con l'uso di ARIA e fornire qualche ulteriore aiuto per la validazione. Che ne dice di indicare se i campi sono richiesti in primo luogo e quale range dovrebbe essere l'età?

1. A questo punto, prenda una copia dei nostri file [`form-validation.html`](https://github.com/mdn/learning-area/blob/main/accessibility/css/form-validation.html) e [`validation.js`](https://github.com/mdn/learning-area/blob/main/accessibility/css/validation.js), e salvali in una directory locale.
2. Aprili entrambi in un editor di testo e osserva come funziona il codice.
3. Prima di tutto, aggiungi un paragrafo appena sopra il tag di apertura `<form>`, come quello qui sotto, e segni entrambi i `<label>` del modulo con un asterisco. Questo è normalmente come segniamo i campi obbligatori per gli utenti vedenti.

   ```html
   <p>Fields marked with an asterisk (*) are required.</p>
   ```

4. Questo ha senso visivamente, ma non è così facile da capire per gli utenti del lettore di schermo. Fortunatamente, WAI-ARIA fornisce l'attributo [`aria-required`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-required) per dare suggerimenti ai lettori di schermo che dovrebbero dire agli utenti che gli input del modulo devono essere riempiti. Aggiorni gli elementi `<input>` in questo modo:

   ```html
   <input type="text" name="name" id="name" aria-required="true" />

   <input type="number" name="age" id="age" aria-required="true" />
   ```

5. Se salva l'esempio ora e lo testa con un lettore di schermo, dovrebbe sentire qualcosa come "Inserisci il tuo nome asterisco, richiesto, modifica testo".
6. Potrebbe essere utile anche se dessimo agli utenti del lettore di schermo e agli utenti vedenti un'idea di quale dovrebbe essere il valore dell'età. Questo viene spesso presentato come tooltip o placeholder all'interno del campo di modulo. WAI-ARIA include le proprietà [`aria-valuemin`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-valuemin) e [`aria-valuemax`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-valuemax) per specificare i valori minimo e massimo, e i lettori di schermo supportano gli attributi nativi `min` e `max`. Un'altra funzione ben supportata è l'attributo HTML `placeholder`, che può contenere un messaggio mostrato nell'input quando non è stato inserito alcun valore ed è letto da alcuni lettori di schermo. Aggiorni il suo input numerico in questo modo:

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

Includa sempre un {{HTMLelement('label')}} per ogni input. Anche se alcuni lettori di schermo annunciano il testo placeholder, molti non lo fanno. Sostituzioni accettabili per fornire ai controlli del modulo un nome accessibile includono [`aria-label`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-label) e [`aria-labelledby`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-labelledby). Ma l'elemento `<label>` con un attributo `for` è il metodo preferito poiché offre usabilità a tutti gli utenti, inclusi gli utenti del mouse.

> [!NOTE]
> Può vedere l'esempio finito dal vivo in [`form-validation-updated.html`](https://mdn.github.io/learning-area/accessibility/aria/form-validation-updated.html).

WAI-ARIA consente anche alcune tecniche avanzate di etichettatura dei moduli, oltre all'elemento classico {{htmlelement("label")}}. Abbiamo già parlato dell'uso della proprietà [`aria-label`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-label) per fornire un'etichetta dove non vogliamo che sia visibile agli utenti vedenti (vedi la sezione [Punti di riferimento/Segnaletica](#signpostslandmarks), sopra). Alcune altre tecniche di etichettatura utilizzano altre proprietà come [`aria-labelledby`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-labelledby) se vuole designare un elemento non-`<label>` come etichetta o etichettare più input di modulo con la stessa etichetta, e [`aria-describedby`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-describedby), se desidera associare altre informazioni a un input di modulo e farle leggere anche. Vedi l'articolo avanzato di etichettatura dei moduli di [WebAIM](https://webaim.org/techniques/forms/advanced) per ulteriori dettagli.

Ci sono molte altre proprietà e stati utili anche, per indicare lo stato degli elementi del modulo. Ad esempio, `aria-disabled="true"` può essere usato per indicare che un campo del modulo è disabilitato. Molti browser salteranno i campi del modulo disabilitati e questo porta a far sì che non vengano letti dagli screen reader. In alcuni casi, un elemento disabilitato verrà percepito, quindi è una buona idea includere questo attributo per informare il lettore di schermo che un controllo di modulo disabilitato è effettivamente disabilitato.

Se lo stato disabilitato di un input è probabile che cambi, allora è anche una buona idea indicare quando succede, e quale sia il risultato. Ad esempio, nel nostro demo [`form-validation-checkbox-disabled.html`](https://mdn.github.io/learning-area/accessibility/aria/form-validation-checkbox-disabled.html), c'è una casella di controllo che quando selezionata, abilita un altro input di modulo per consentire l'inserimento di ulteriori informazioni. Abbiamo impostato una regione live nascosta:

```html
<p class="hidden-alert" aria-live="assertive"></p>
```

che è nascosta alla vista mediante posizionamento assoluto. Quando questo è selezionato/non selezionato, aggiorniamo il testo all'interno della regione live nascosta per dire agli utenti del lettore di schermo quale sia il risultato del selezionare questa casella di controllo, oltre ad aggiornare lo stato `aria-disabled`, e alcuni indicatori visivi.

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

Abbiamo menzionato un paio di volte in questo corso l'accessibilità nativa (e i problemi di accessibilità legati all'utilizzo di altri elementi per simulare) di pulsanti, collegamenti o elementi di modulo (vedi [Usare controlli di interfaccia utente semantici dove possibile](/it/docs/Learn_web_development/Core/Accessibility/HTML#use_semantic_ui_controls_where_possible) nell'articolo sull'accessibilità HTML, e [Miglioramento dell'accessibilità tramite tastiera](#miglioramento_dell'accessibilità_tramite_tastiera), sopra). Fondamentalmente, puoi aggiungere nuovamente l'accessibilità da tastiera senza troppi problemi in molti casi, utilizzando `tabindex` e un po' di JavaScript.

Ma cosa succede ai lettori di schermo? Ancora non vedranno gli elementi come pulsanti. Se testiamo il nostro esempio [`fake-div-buttons.html`](https://mdn.github.io/learning-area/tools-testing/cross-browser-testing/accessibility/fake-div-buttons.html) con un lettore di schermo, i nostri falsi pulsanti saranno segnalati usando frasi come "Click me!, group", che è ovviamente confuso.

Possiamo risolvere questo usando un ruolo WAI-ARIA. Faccia una copia locale di [`fake-div-buttons.html`](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/accessibility/fake-div-buttons.html), e aggiunga [`role="button"`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/button_role) a ciascun `<div>` del pulsante, per esempio:

```html
<div data-message="This is from the first button" tabindex="0" role="button">
  Click me!
</div>
```

Ora, quando prova questo usando un lettore di schermo, i pulsanti saranno segnalati usando frasi come "Click me!, button". Sebbene sia molto meglio, deve ancora aggiungere tutte le funzionalità native dei pulsanti che gli utenti si aspettano, come la gestione degli eventi <kbd>enter</kbd> e click, come spiegato nella [documentazione del ruolo `button`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/button_role).

> [!NOTE]
> Tuttavia, non dimentichi che usare l'elemento semantico corretto dove possibile è sempre meglio. Se vuole creare un pulsante, e può usare un elemento {{htmlelement("button")}}, dovrebbe usare un elemento {{htmlelement("button")}}!

#### Guidare gli utenti attraverso widget complessi

Ci sono una miriade di altri [ruoli](/it/docs/Web/Accessibility/ARIA/Reference/Roles) che possono identificare strutture di elementi non semantici come funzionalità dell'interfaccia utente comuni che vanno oltre quello che è disponibile in HTML standard, per esempio [`combobox`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/combobox_role), [`slider`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/slider_role), [`tabpanel`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/tabpanel_role), [`tree`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/tree_role). Può vedere diversi esempi utili nella [Deque university code library](https://dequeuniversity.com/library/) per farle un'idea di come tali controlli possano essere resi accessibili.

Esaminiamo un esempio nostro. Torneremo alla nostra semplice interfaccia a schede posizionata in modo assoluto (vedi [Nascondere le cose](/it/docs/Learn_web_development/Core/Accessibility/CSS_and_JavaScript#hiding_things) nel nostro articolo sull'accessibilità CSS e JavaScript), che può trovare nell'[esempio di scatola di informazioni a schede](/it/docs/Learn_web_development/Core/CSS_layout/Practical_positioning_examples#a_tabbed_info-box).

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

In questo esempio abbiamo utilizzato una combinazione di elementi semantici, ruoli ARIA e attributi ARIA. Il primo di questi è che abbiamo utilizzato un elemento {{htmlelement("button")}} come _tab_, il che significa che la scheda può essere selezionata tramite un clic del mouse o tramite la tastiera usando spazio o invio.

Le funzionalità ARIA utilizzate includono:

- Nuovi ruoli — [`tablist`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/tablist_role), [`tab`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/tab_role), [`tabpanel`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/tabpanel_role)
  - : Questi identificano le aree importanti della struttura delle schede — il contenitore per le schede, le schede stesse e i pannelli di scheda corrispondenti.
- [`aria-selected`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-selected)
  - : Definisce quale scheda è attualmente selezionata. Man mano che diverse schede vengono selezionate dall'utente, il valore di questo attributo sulle diverse schede viene aggiornato tramite JavaScript.
- `tabindex="-1"`
  - : `tabindex="-1"` esclude l'elemento dall'ordine di tabulazione. Poiché stiamo usando JavaScript per consentire all'utente di controllare le schede tramite tastiera o mouse, non vogliamo che l'utente sia in grado di usare il tasto tab per navigare fino ai pulsanti.
- [`aria-labelledby`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-labelledby)
  - : Questo attributo identifica un elemento (dal suo `id`) che etichetta l'elemento, in questo esempio l'`<article>` è etichettato dalla scheda corrispondente o `<button>`.
- [`aria-controls`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-controls)
  - : Questo attributo identifica un elemento (dal suo `id`) che è controllato dall'elemento, in questo esempio l'`<article>` è controllato dalla scheda o `<button>` corrispondente.

Avremmo potuto usare `aria-hidden` per nascondere il contenuto dei pannelli delle schede dalle tecnologie assistive, ma se quel contenuto conteneva contenuti focalizzabili, come collegamenti, l'utente sarebbe comunque in grado di tabulare su quel contenuto anche quando `aria-hidden=true` è impostato per i pannelli non attivi. In questo esempio abbiamo applicato `class="is-hidden"` ai pannelli delle schede che corrispondono alle schede con `aria-selected="false"` e usato CSS per `display: none;`, che impedisce che il contenuto nascosto venga tabulato.

Nei nostri test, questa nuova struttura ha servito a migliorare le cose nel complesso. I `<button>` sono ora riconosciuti come schede (ad esempio, "tab" è pronunciato dal lettore di schermo), la scheda selezionata è indicata da "selected" che viene letta con il nome della scheda e qualsiasi contenuto non mostrato non può essere tabulato. L'utente può anche navigare tra le schede con la tastiera o il mouse.

## Metti alla prova le tue capacità!

Hai raggiunto la fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Può trovare alcuni ulteriori test per verificare se ha trattenuto queste informazioni prima di procedere — vedi [Metti alla prova le tue capacità: WAI-ARIA](/it/docs/Learn_web_development/Core/Accessibility/Test_your_skills/WAI-ARIA).

## Riassunto

Questo articolo non ha coperto tutto ciò che è disponibile in WAI-ARIA, ma avrebbe dovuto fornirle abbastanza informazioni per capire come usarlo e conoscere alcuni dei modelli più comuni che incontrerà che lo richiedono.

## Vedi anche

- [Stati e proprietà Aria](/it/docs/Web/Accessibility/ARIA/Reference/Attributes): Tutti gli attributi `aria-*`
- [Ruoli WAI-ARIA](/it/docs/Web/Accessibility/ARIA/Reference/Roles): Categorie di ruoli ARIA e ruoli trattati su MDN
- [ARIA in HTML](https://www.w3.org/TR/html-aria/) su W3C: Una specifica che definisce, per ogni funzionalità HTML, le semantiche di accessibilità (ARIA) applicate implicitamente su di essa dal browser e le funzionalità WAI-ARIA che può impostare su di essa se sono richieste semantiche extra
- [Biblioteca del codice universitario Deque](https://dequeuniversity.com/library/): Una libreria di esempi davvero utili e pratici che mostrano controlli dell'interfaccia utente complessi resi accessibili utilizzando le funzionalità WAI-ARIA
- [Pratiche di redazione WAI-ARIA](https://www.w3.org/WAI/ARIA/apg/) su W3C: Un modello di progettazione molto dettagliato del W3C, che spiega come implementare diversi tipi di controllo dell'interfaccia utente complessa migliorandone l'accessibilità usando le funzionalità WAI-ARIA

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/CSS_and_JavaScript","Learn_web_development/Core/Accessibility/Multimedia", "Learn_web_development/Core/Accessibility")}}
