---
title: Nozioni di base su WAI-ARIA
short-title: WAI-ARIA
slug: Learn_web_development/Core/Accessibility/WAI-ARIA_basics
l10n:
  sourceCommit: 65692fd4d256d5647749b7c7005dcf53d425a533
---

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/Test_your_skills/CSS_and_JavaScript","Learn_web_development/Core/Accessibility/Test_your_skills/WAI-ARIA", "Learn_web_development/Core/Accessibility")}}

In seguito all'articolo precedente, talvolta può essere difficile creare controlli dell'interfaccia utente complessi che coinvolgono HTML non semantico e contenuti dinamici aggiornati da JavaScript. WAI-ARIA è una tecnologia che può aiutare a risolvere questi problemi aggiungendo ulteriore semantica che browser e tecnologie assistive possono riconoscere e utilizzare per informare gli utenti di ciò che sta accadendo. Qui verrà mostrato come usarla a un livello basilare per migliorare l'accessibilità.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Conoscenza di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e delle buone pratiche di accessibilità illustrate nelle lezioni precedenti del modulo</a>.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Lo scopo di WAI-ARIA — fornire semantica a HTML altrimenti non semantico, affinché gli utenti delle tecnologie assistive possano comprendere le interfacce presentate loro.</li>
          <li>La sintassi di base — ruoli, proprietà e stati.</li>
          <li>Landmark e indicazioni.</li>
          <li>Migliorare l'accessibilità tramite tastiera.</li>
          <li>Annunciare aggiornamenti di contenuti dinamici con live region.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Che cos'è WAI-ARIA?

Iniziamo esaminando che cos'è WAI-ARIA e cosa può fare.

### Un insieme completamente nuovo di problemi

Quando le app web hanno iniziato a diventare più complesse e dinamiche, ha iniziato ad apparire un nuovo insieme di funzionalità e problemi di accessibilità.

Ad esempio, HTML ha introdotto diversi elementi semantici per definire funzionalità comuni delle pagine ({{htmlelement("nav")}}, {{htmlelement("footer")}}, ecc.). Prima che questi fossero disponibili, gli sviluppatori utilizzavano {{htmlelement("div")}} con ID o classi, ad esempio `<div class="nav">`, ma ciò era problematico, poiché non esisteva un modo semplice per trovare programmaticamente una funzionalità specifica della pagina, come la navigazione principale.

La soluzione iniziale consisteva nell'aggiungere uno o più link nascosti nella parte superiore della pagina per collegarsi alla navigazione (o a qualsiasi altra destinazione), ad esempio:

```html
<a href="#hidden" class="hidden">Skip to navigation</a>
```

Ma questo non è ancora molto preciso e può essere utilizzato solo quando lo screen reader legge dall'inizio della pagina.

Come altro esempio, le app hanno iniziato a includere controlli complessi come selettori di date per scegliere date, cursori per scegliere valori e così via. HTML fornisce tipi di input speciali per renderizzare tali controlli:

```html
<input type="date" /> <input type="range" />
```

In origine non erano ben supportati e risultava, e lo è tuttora in misura minore, difficile applicare loro stili, portando designer e sviluppatori a scegliere soluzioni personalizzate. Invece di utilizzare queste funzionalità native, alcuni sviluppatori si affidano a librerie JavaScript che generano tali controlli come una serie di {{htmlelement("div")}} annidati, a cui vengono poi applicati stili mediante CSS e che vengono controllati tramite JavaScript.

Il problema è che visivamente funzionano, ma gli screen reader non riescono affatto a comprendere cosa siano e i loro utenti vengono solo informati di poter visualizzare un insieme confuso di elementi senza semantica che ne descriva il significato.

### WAI-ARIA entra in scena

[WAI-ARIA](https://w3c.github.io/aria/) (Web Accessibility Initiative - Accessible Rich Internet Applications) è una specifica scritta dal W3C, che definisce un insieme di attributi HTML aggiuntivi applicabili agli elementi per fornire semantica aggiuntiva e migliorare l'accessibilità laddove risulta carente. Nella specifica sono definite tre funzionalità principali:

- [Ruoli](/it/docs/Web/Accessibility/ARIA/Reference/Roles)
  - : Definiscono che cos'è o che cosa fa un elemento. Molti di questi sono i cosiddetti ruoli landmark, che duplicano in gran parte il valore semantico degli elementi strutturali, come `role="navigation"` ({{htmlelement("nav")}}), `role="banner"` ({{htmlelement("header")}} del documento), `role="complementary"` ({{htmlelement("aside")}}) oppure `role="search"` ({{htmlelement("search")}}). Alcuni altri ruoli descrivono diverse strutture di pagina per le quali non esistono elementi corrispondenti, come `role="tablist"` e `role="tabpanel"`, che si trovano comunemente nelle interfacce utente.
- Proprietà
  - : Definiscono proprietà degli elementi, utilizzabili per attribuire loro ulteriore significato o semantica. Ad esempio, `aria-required="true"` specifica che un input di un modulo deve essere compilato per essere valido, mentre `aria-labelledby="label"` permette di inserire un ID su un elemento e quindi farvi riferimento come etichetta per qualsiasi altro elemento della pagina, inclusi più elementi, cosa che non è possibile usando `<label for="input">`. Ad esempio, è possibile usare `aria-labelledby` per specificare che una descrizione di chiave contenuta in un {{htmlelement("div")}} è l'etichetta per più celle di una tabella, oppure come alternativa al testo alternativo di un'immagine — specificare informazioni già presenti nella pagina come testo alternativo dell'immagine, anziché doverle ripetere nell'attributo `alt`. È possibile vedere un esempio in [Alternative testuali](/it/docs/Learn_web_development/Core/Accessibility/HTML#text_alternatives).
- Stati
  - : Proprietà speciali che definiscono le condizioni correnti degli elementi, come `aria-disabled="true"`, che specifica a uno screen reader che un input di un modulo è attualmente disabilitato. Gli stati differiscono dalle proprietà perché queste ultime non cambiano durante il ciclo di vita di un'app, mentre gli stati possono cambiare, generalmente in modo programmatico tramite JavaScript.

Un aspetto importante degli attributi WAI-ARIA è che non influenzano nulla della pagina web, tranne le informazioni esposte dalle API di accessibilità del browser, da cui gli screen reader ottengono le proprie informazioni. WAI-ARIA non influenza la struttura della pagina web, il DOM e così via, sebbene gli attributi possano essere utili per selezionare elementi tramite CSS.

> [!NOTE]
> È possibile trovare un elenco utile di tutti i ruoli ARIA e dei relativi utilizzi, con link a ulteriori informazioni, nella specifica WAI-ARIA — vedere [Definition of Roles](https://w3c.github.io/aria/#role_definitions) — e su questo sito — vedere [Ruoli ARIA](/it/docs/Web/Accessibility/ARIA/Reference/Roles).
>
> La specifica contiene inoltre un elenco di tutte le proprietà e gli stati, con link a ulteriori informazioni — vedere [Definitions of States and Properties (all `aria-*` attributes)](https://w3c.github.io/aria/#state_prop_def).

## Dove è supportato WAI-ARIA?

Non è una domanda facile a cui rispondere. È difficile trovare una risorsa conclusiva che indichi quali funzionalità di WAI-ARIA siano supportate e dove, perché:

1. La specifica WAI-ARIA contiene molte funzionalità.
2. Esistono molte combinazioni di sistemi operativi, browser e screen reader da considerare.

Quest'ultimo punto è fondamentale — per utilizzare uno screen reader, il sistema operativo deve innanzitutto eseguire browser dotati delle API di accessibilità necessarie per esporre le informazioni di cui gli screen reader hanno bisogno per svolgere il proprio lavoro. I sistemi operativi più diffusi dispongono di uno o due browser con cui gli screen reader possono funzionare.

Successivamente, occorre preoccuparsi se i browser in questione supportino le funzionalità ARIA e le espongano attraverso le loro API, ma anche se gli screen reader riconoscano tali informazioni e le presentino ai propri utenti in modo utile.

1. Il supporto dei browser è quasi universale.
2. Il supporto degli screen reader per le funzionalità ARIA non è ancora a questo livello, ma gli screen reader più diffusi si stanno avvicinando. È possibile farsi un'idea dei livelli di supporto consultando l'articolo di Powermapper [WAI-ARIA Screen reader compatibility](https://www.powermapper.com/tests/screen-readers/aria/).

In questo articolo non si tenterà di trattare ogni funzionalità WAI-ARIA e i relativi dettagli precisi sul supporto. Verranno invece trattate le funzionalità WAI-ARIA più importanti da conoscere; se non vengono menzionati dettagli sul supporto, si può presumere che la funzionalità sia ben supportata. Eventuali eccezioni verranno indicate chiaramente.

> [!NOTE]
> Alcune librerie JavaScript supportano WAI-ARIA, il che significa che, quando generano funzionalità dell'interfaccia utente come controlli complessi dei moduli, aggiungono attributi ARIA per migliorarne l'accessibilità. Se si cerca una soluzione JavaScript di terze parti per lo sviluppo rapido di interfacce utente, l'accessibilità dei relativi widget dell'interfaccia utente dovrebbe essere considerata un fattore importante nella scelta. Buoni esempi sono jQuery UI (vedere [About jQuery UI: Deep accessibility support](https://jqueryui.com/about/#deep-accessibility-support)), [ExtJS](https://www.sencha.com/products/extjs/) e [Dojo/Dijit](https://dojotoolkit.org/reference-guide/1.10/dijit/a11y/statement.html).

## Quando usare WAI-ARIA?

In precedenza sono stati descritti alcuni dei problemi che hanno portato alla creazione di WAI-ARIA, ma essenzialmente esistono quattro aree principali in cui WAI-ARIA è utile:

- Indicazioni/Landmark
  - : I valori dell'attributo [`role`](/it/docs/Web/Accessibility/ARIA/Reference/Roles) di ARIA possono agire come landmark che replicano la semantica degli elementi HTML (ad esempio, {{htmlelement("nav")}}) oppure vanno oltre la semantica HTML per fornire indicazioni verso diverse aree funzionali, ad esempio `search`, `tablist`, `tab`, `listbox` e così via.
- Aggiornamenti di contenuti dinamici
  - : Gli screen reader tendono ad avere difficoltà nel segnalare contenuti in costante cambiamento; con ARIA è possibile usare `aria-live` per informare gli utenti di screen reader quando un'area di contenuto viene aggiornata dinamicamente: ad esempio, tramite JavaScript nella pagina che [recupera nuovi contenuti dal server e aggiorna il DOM](/it/docs/Learn_web_development/Core/Scripting/Network_requests).
- Migliorare l'accessibilità tramite tastiera
  - : Esistono elementi HTML integrati dotati di accessibilità nativa tramite tastiera; quando vengono utilizzati altri elementi insieme a JavaScript per simulare interazioni simili, l'accessibilità tramite tastiera e la segnalazione da parte degli screen reader ne risentono. Dove ciò è inevitabile, WAI-ARIA offre un mezzo per consentire ad altri elementi di ricevere il focus, usando `tabindex`.
- Accessibilità di controlli non semantici
  - : Quando una serie di `<div>` annidati insieme a CSS/JavaScript viene utilizzata per creare una funzionalità complessa dell'interfaccia utente, oppure un controllo nativo viene notevolmente migliorato/modificato tramite JavaScript, l'accessibilità può risentirne — gli utenti di screen reader avranno difficoltà a capire che cosa faccia la funzionalità se non esistono semantica o altri indizi. In queste situazioni, ARIA può aiutare a fornire ciò che manca con una combinazione di ruoli quali `button`, `listbox` o `tablist`, e proprietà quali `aria-required` o `aria-posinset`, per fornire ulteriori indizi sulla funzionalità.

Nella sezione successiva verranno esaminate più in dettaglio le quattro aree principali descritte in precedenza, insieme a esempi. Prima di continuare, è opportuno predisporre un ambiente di test con screen reader, così da poter testare alcuni esempi durante la lettura. Per maggiori informazioni, consultare la sezione sul [test degli screen reader](/it/docs/Learn_web_development/Core/Accessibility/Tooling#screen_readers).

> [!CALLOUT]
>
> **WAI-ARIA dovrebbe essere utilizzata solo quando necessario!**
>
> Usare gli elementi HTML corretti fornisce implicitamente i ruoli necessari e si dovrebbero _sempre_ utilizzare le [funzionalità HTML native](/it/docs/Learn_web_development/Core/Accessibility/HTML) per fornire la semantica necessaria agli screen reader affinché possano comunicare ai propri utenti ciò che sta accadendo. Talvolta ciò non è possibile, perché si ha un controllo limitato sul codice oppure perché si sta creando qualcosa di complesso per cui non esiste un elemento HTML semplice da implementare. In questi casi, WAI-ARIA può essere uno strumento prezioso per migliorare l'accessibilità.
>
> Tuttavia, ancora una volta, va utilizzata solo quando necessario!
>
> Inoltre, occorre assicurarsi di testare il sito con una varietà di utenti _reali_ — persone senza disabilità, persone che usano screen reader, persone che usano la navigazione tramite tastiera e così via. Queste persone avranno una comprensione migliore di quanto bene funzioni.

## Indicazioni/Landmark

WAI-ARIA aggiunge ai browser l'[attributo `role`](https://w3c.github.io/aria/#role_definitions), che permette di aggiungere valore semantico aggiuntivo agli elementi del sito ovunque sia necessario. La prima area importante in cui ciò risulta utile consiste nel fornire informazioni agli screen reader affinché i loro utenti possano trovare gli elementi comuni della pagina. Questo esempio ha la seguente struttura:

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
  background-color: darkgrey;
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
  background: #333333;
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

Provando a testare l'esempio con uno screen reader in un browser moderno, si ottengono già alcune informazioni utili. Ad esempio, VoiceOver fornisce quanto segue:

- Sull'elemento `<header>` — "banner, 2 elementi" (contiene un'intestazione e `<nav>`).
- Sull'elemento `<nav>` — "navigazione, 2 elementi" (contiene un elenco e un modulo).
- Sull'elemento `<main>` — "principale, 2 elementi" (contiene un articolo e un aside).
- Sull'elemento `<aside>` — "complementare, 2 elementi" (contiene un'intestazione e un elenco).
- Sull'input del modulo di ricerca — "Query di ricerca, inserimento all'inizio del testo".
- Sull'elemento `<footer>` — "footer, 1 elemento".

Se si accede al menu dei landmark di VoiceOver, tramite il tasto VoiceOver + U e quindi usando i tasti cursore per scorrere le opzioni del menu, si può vedere che la maggior parte degli elementi è elencata correttamente, quindi accessibile rapidamente.

![Menu di VoiceOver su Mac per l'accessibilità rapida. Intestazione Landmark e elenco Landmark che include banner, navigazione, principale e complementare.](landmarks-list.png)

Tuttavia, è possibile fare di meglio. Il modulo di ricerca è un landmark davvero importante che gli utenti vorranno trovare, ma non è elencato nel menu dei landmark né viene trattato come un landmark rilevante oltre al fatto che l'input effettivo viene identificato come input di ricerca (`<input type="search">`).

Per contrassegnare il modulo come landmark, è possibile avvolgerlo con l'elemento {{htmlelement("search")}} oppure assegnargli ARIA `role="search"`. Come regola generale, usare la semantica HTML quando possibile e usare ARIA solo quando non esiste un equivalente HTML.

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
  background-color: darkgrey;
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
  background: #333333;
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

Soprattutto, è stato utilizzato HTML semantico che attribuisce significato e ruoli alla struttura della pagina senza aggiungere attributi [`role`](/it/docs/Web/Accessibility/ARIA/Reference/Roles) non necessari alla struttura HTML, la quale ha una struttura come questa:

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

In questo esempio è stata aggiunta anche una funzionalità aggiuntiva — all'elemento {{htmlelement("input")}} è stato assegnato l'attributo [`aria-label`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-label), che gli fornisce un'etichetta descrittiva da leggere tramite screen reader, anche se non è stato incluso un elemento {{htmlelement("label")}}. In casi come questo è molto utile — un modulo di ricerca come questo è una funzionalità molto comune e facilmente riconoscibile, e l'aggiunta di un'etichetta visiva rovinerebbe il design della pagina.

```html
<input
  type="search"
  name="q"
  placeholder="Search query"
  aria-label="Search through site content" />
```

Ora, usando VoiceOver per osservare questo esempio, si ottengono alcuni miglioramenti:

- Il modulo di ricerca viene identificato come elemento separato, sia durante l'esplorazione della pagina sia nel menu Landmark.
- Il testo dell'etichetta contenuto nell'attributo `aria-label` viene letto quando l'input del modulo è evidenziato.

Se è necessario supportare browser più vecchi come IE8, vale la pena includere ruoli ARIA a tale scopo. E se per qualche ragione il sito è costruito utilizzando solo `<div>`, è decisamente opportuno includere i ruoli ARIA per fornire questa semantica tanto necessaria.

Di seguito verranno illustrate ulteriormente queste semantiche e la potenza delle proprietà/attributi ARIA, specialmente nella sezione [Accessibilità di controlli non semantici](#accessibilità_di_controlli_non_semantici). Per ora, vediamo come ARIA possa aiutare con gli aggiornamenti di contenuti dinamici.

## Aggiornamenti di contenuti dinamici

I contenuti caricati nel DOM possono essere facilmente raggiunti tramite screen reader, dai contenuti testuali al testo alternativo associato alle immagini. I siti web statici tradizionali, con contenuti in gran parte testuali, sono quindi facili da rendere accessibili alle persone con disabilità visive.

Il problema è che le app web moderne spesso non sono costituite solo da testo statico — spesso aggiornano parti della pagina recuperando nuovi contenuti dal server, in questo esempio viene utilizzato un array statico di citazioni, e aggiornando il DOM. Queste aree vengono talvolta definite **live region**.

Vediamo un esempio — un generatore casuale di citazioni:

```html live-sample___aria-no-live
<section>
  <h1>Random quote generator</h1>
  <button>Start giving me quotes</button>
  <blockquote>
    <p></p>
  </blockquote>
</section>
```

```css hidden live-sample___aria-no-live live-sample___aria-live
* {
  box-sizing: border-box;
}

html {
  font-family: sans-serif;
}

html,
body {
  height: 100%;
}

h1 {
  letter-spacing: 2px;
}

p {
  line-height: 1.6;
}

section {
  height: 100%;
  padding: 10px;
  background: #666666;
  text-shadow: 1px 1px 1px black;
  color: white;
}
```

```js live-sample___aria-no-live live-sample___aria-live
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

```js live-sample___aria-no-live live-sample___aria-live
const quotePara = document.querySelector("section p");
const btn = document.querySelector("button");

btn.addEventListener("click", () => {
  function showQuote() {
    let random = Math.floor(Math.random() * quotes.length);
    quotePara.textContent = `${quotes[random].quote} -- ${quotes[random].author}`;
  }

  showQuote();
  btn.disabled = true;
  window.setInterval(showQuote, 5000);
});
```

{{EmbedLiveSample("aria-no-live", "100", "220")}}

Funziona correttamente, ma non è positivo per l'accessibilità — l'aggiornamento del contenuto non viene rilevato dagli screen reader, quindi i loro utenti non saprebbero cosa sta accadendo. Questo è un esempio abbastanza banale, ma basta immaginare di creare un'interfaccia utente complessa con molti contenuti in costante aggiornamento, come una chat, l'interfaccia di un gioco strategico oppure la visualizzazione di un carrello della spesa aggiornato in tempo reale — sarebbe impossibile usare l'app in modo efficace senza un modo per avvisare l'utente degli aggiornamenti.

Fortunatamente, WAI-ARIA fornisce un meccanismo utile per fornire questi avvisi — la proprietà [`aria-live`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-live). Applicarla a un elemento fa sì che gli screen reader leggano il contenuto aggiornato. L'urgenza con cui il contenuto viene letto dipende dal valore dell'attributo:

- `off`
  - : Il valore predefinito. Gli aggiornamenti non devono essere annunciati.
- `polite`
  - : Gli aggiornamenti devono essere annunciati solo se l'utente è inattivo.
- `assertive`
  - : Gli aggiornamenti devono essere annunciati all'utente non appena possibile.

Qui viene aggiornato il tag di apertura `<blockquote>` come segue:

```html
<blockquote aria-live="assertive">…</blockquote>
```

Questo farà sì che uno screen reader legga il contenuto durante l'aggiornamento: provare a testare la versione live aggiornata:

```html hidden live-sample___aria-live
<section>
  <h1>Random quote generator</h1>
  <button>Start giving me quotes</button>
  <blockquote aria-live="assertive">
    <p></p>
  </blockquote>
</section>
```

{{EmbedLiveSample("aria-live", "100", "220")}}

> [!NOTE]
> Esistono altre proprietà ARIA correlate a `aria-live` che vale la pena conoscere:
>
> - La proprietà [`aria-atomic`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-atomic), quando impostata su `true`, indica agli screen reader di leggere l'intero contenuto dell'elemento come un'unica unità atomica, non soltanto le parti aggiornate. È utile quando viene aggiornato solo il contenuto di una sezione, ma si vuole che l'intestazione venga letta ogni volta che qualcosa cambia, per ricordare all'utente il suo contenuto.
> - La proprietà [`aria-relevant`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-relevant) è utile per controllare ciò che viene letto quando una live region viene aggiornata. Ad esempio, è possibile fare in modo che vengano lette solo le aggiunte o le rimozioni di contenuto.

## Migliorare l'accessibilità tramite tastiera

Come discusso in alcuni altri punti del modulo, uno dei punti di forza principali di HTML rispetto all'accessibilità è l'accessibilità tramite tastiera integrata in funzionalità quali pulsanti, controlli dei moduli e link. In generale, è possibile usare il tasto Tab per spostarsi tra i controlli, il tasto Invio/Return per selezionare o attivare i controlli e occasionalmente altri controlli secondo necessità, ad esempio i cursori su e giù per spostarsi tra le opzioni in una casella `<select>`.

Tuttavia, talvolta sarà necessario scrivere codice che utilizza elementi non semantici come pulsanti, o altri tipi di controllo, oppure che utilizza controlli focalizzabili per uno scopo non del tutto appropriato. Si potrebbe cercare di correggere codice ereditato non corretto oppure costruire un qualche tipo di widget complesso che lo richiede.

Per rendere focalizzabile codice non focalizzabile, WAI-ARIA estende l'attributo `tabindex` con alcuni nuovi valori:

- `tabindex="0"` — come indicato sopra, questo valore consente agli elementi che normalmente non sono raggiungibili tramite Tab di diventarlo. È il valore di `tabindex` più utile.
- `tabindex="-1"` — consente a elementi normalmente non raggiungibili tramite Tab di ricevere il focus programmaticamente, ad esempio tramite JavaScript o come destinazione di link.

Questo argomento è stato discusso più in dettaglio e ne è stata mostrata un'implementazione tipica nell'articolo sull'accessibilità HTML — vedere [Ripristinare l'accessibilità tramite tastiera](/it/docs/Learn_web_development/Core/Accessibility/HTML#building_keyboard_accessibility_back_in).

## Accessibilità di controlli non semantici

Questo prosegue dalla sezione precedente — quando una serie di `<div>` annidati insieme a CSS/JavaScript viene utilizzata per creare una funzionalità complessa dell'interfaccia utente, oppure un controllo nativo viene notevolmente migliorato/modificato tramite JavaScript, non solo l'accessibilità tramite tastiera può risentirne, ma gli utenti di screen reader avranno difficoltà a capire che cosa faccia la funzionalità se non esistono semantica o altri indizi. In tali situazioni, ARIA può aiutare a fornire la semantica mancante.

### Convalida dei moduli e avvisi di errore

Innanzitutto, riesaminiamo l'esempio del modulo visto inizialmente nell'articolo sull'accessibilità con CSS e JavaScript, leggere [Mantenerlo non invasivo](/it/docs/Learn_web_development/Core/Accessibility/CSS_and_JavaScript#keeping_it_unobtrusive) per un riepilogo completo. Alla fine di questa sezione è stato mostrato che sono stati inclusi alcuni attributi ARIA nel riquadro del messaggio di errore che visualizza eventuali errori di convalida quando si tenta di inviare il modulo:

```html
<div class="errors" role="alert" aria-relevant="all">
  <ul></ul>
</div>
```

- [`role="alert"`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/alert_role) trasforma automaticamente l'elemento a cui viene applicato in una live region, quindi le sue modifiche vengono lette; inoltre lo identifica semanticamente come messaggio di avviso, ovvero informazioni importanti sensibili al tempo o al contesto, e rappresenta un modo migliore e più accessibile per inviare un avviso a un utente. Le finestre di dialogo modali come le chiamate [`alert()`](/it/docs/Web/API/Window/alert) presentano diversi problemi di accessibilità; vedere [Popup Windows](https://webaim.org/techniques/javascript/other#popups) di WebAIM.
- Un valore [`aria-relevant`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-relevant) pari a `all` istruisce lo screen reader a leggere il contenuto dell'elenco degli errori ogni volta che vi vengono apportate modifiche, ovvero quando gli errori vengono aggiunti o rimossi. Ciò è utile perché l'utente vorrà sapere quali errori rimangono, non solo ciò che è stato aggiunto o rimosso dall'elenco.

L'uso di ARIA potrebbe essere esteso ulteriormente per fornire altro aiuto alla convalida. Che ne dire di indicare se i campi sono obbligatori e quale intervallo dovrebbe avere l'età?

1. A questo punto, creare una copia dei file [`form-validation.html`](https://github.com/mdn/learning-area/blob/main/accessibility/css/form-validation.html) e [`validation.js`](https://github.com/mdn/learning-area/blob/main/accessibility/css/validation.js), quindi salvarli in una directory locale.
2. Aprirli entrambi in un editor di testo e osservare come funziona il codice.
3. Innanzitutto, aggiungere un paragrafo appena sopra il tag di apertura `<form>`, come quello riportato di seguito, e contrassegnare entrambe le `<label>` del modulo con un asterisco. Questo è normalmente il modo in cui vengono indicati i campi obbligatori agli utenti vedenti.

   ```html
   <p>Fields marked with an asterisk (*) are required.</p>
   ```

4. Ciò ha senso visivamente, ma non è altrettanto facile da comprendere per gli utenti di screen reader. Fortunatamente, WAI-ARIA fornisce l'attributo [`aria-required`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-required) per suggerire agli screen reader che devono comunicare agli utenti che gli input del modulo devono essere compilati. Aggiornare gli elementi `<input>` come segue:

   ```html
   <input type="text" name="name" id="name" aria-required="true" />

   <input type="number" name="age" id="age" aria-required="true" />
   ```

5. Salvando ora l'esempio e testandolo con uno screen reader, dovrebbe essere pronunciato qualcosa come "Inserisci il tuo nome asterisco, obbligatorio, modifica testo".
6. Potrebbe inoltre essere utile dare agli utenti di screen reader e a quelli vedenti un'idea di quale dovrebbe essere il valore dell'età. Questo viene spesso presentato come tooltip o segnaposto all'interno del campo del modulo. WAI-ARIA include le proprietà [`aria-valuemin`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-valuemin) e [`aria-valuemax`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-valuemax) per specificare valori minimi e massimi e gli screen reader supportano gli attributi nativi `min` e `max`. Un'altra funzionalità ben supportata è l'attributo HTML `placeholder`, che può contenere un messaggio mostrato nell'input quando non è inserito alcun valore e letto da alcuni screen reader. Aggiornare l'input numerico in questo modo:

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

Includere sempre un elemento {{HTMLelement('label')}} per ogni input. Sebbene alcuni screen reader annuncino il testo segnaposto, la maggior parte non lo fa. Sostituzioni accettabili per fornire ai controlli dei moduli un nome accessibile includono [`aria-label`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-label) e [`aria-labelledby`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-labelledby). Tuttavia, l'elemento `<label>` con un attributo `for` è il metodo preferito, poiché offre usabilità a tutti gli utenti, inclusi quelli che usano il mouse.

> [!NOTE]
> È possibile vedere l'esempio completo in diretta su [`form-validation-updated.html`](https://mdn.github.io/learning-area/accessibility/aria/form-validation-updated.html).

WAI-ARIA consente inoltre alcune tecniche avanzate per l'etichettatura dei moduli, oltre al classico elemento {{htmlelement("label")}}. Si è già parlato dell'uso della proprietà [`aria-label`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-label) per fornire un'etichetta quando non si desidera che sia visibile agli utenti vedenti, vedere la sezione [Indicazioni/Landmark](#signpostslandmarks) sopra. Altre tecniche di etichettatura utilizzano altre proprietà, come [`aria-labelledby`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-labelledby) se si desidera designare un elemento diverso da `<label>` come etichetta o etichettare più input del modulo con la stessa etichetta, e [`aria-describedby`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-describedby), se si desidera associare altre informazioni a un input del modulo e farle leggere anch'esse. Per ulteriori dettagli, vedere l'articolo [Advanced Form Labeling di WebAIM](https://webaim.org/techniques/forms/advanced).

Esistono anche molte altre proprietà e stati utili per indicare lo stato degli elementi dei moduli. Ad esempio, `aria-disabled="true"` può essere utilizzato per indicare che un campo del modulo è disabilitato. Molti browser saltano i campi disabilitati dei moduli, facendo sì che non vengano letti dagli screen reader. In alcuni casi, un elemento disabilitato viene percepito, quindi è una buona idea includere questo attributo per informare lo screen reader che un controllo del modulo disabilitato è effettivamente disabilitato.

Se è probabile che lo stato disabilitato di un input cambi, è inoltre una buona idea indicare quando accade e quale sia il risultato. Ad esempio, nella demo [`form-validation-checkbox-disabled.html`](https://mdn.github.io/learning-area/accessibility/aria/form-validation-checkbox-disabled.html), è presente una casella di controllo che, se selezionata, abilita un altro input del modulo per consentire l'inserimento di ulteriori informazioni. È stata inoltre configurata una live region nascosta alla vista mediante posizionamento assoluto:

```html
<p class="hidden-alert" aria-live="assertive"></p>
```

Quando la casella di controllo viene selezionata/deselezionata, viene aggiornato il testo all'interno della live region nascosta per comunicare agli utenti di screen reader il risultato della selezione di questa casella, oltre ad aggiornare lo stato `aria-disabled` e alcuni indicatori visivi:

```js
function toggleMusician(bool) {
  const instrument = formItems[formItems.length - 1];
  if (bool) {
    instrument.input.disabled = false;
    instrument.label.style.color = "black";
    instrument.input.setAttribute("aria-disabled", "false");
    hiddenAlert.textContent =
      "Instruments played field now enabled; use it to tell us what you play.";
  } else {
    instrument.input.disabled = true;
    instrument.label.style.color = "#999999";
    instrument.input.setAttribute("aria-disabled", "true");
    instrument.input.removeAttribute("aria-label");
    hiddenAlert.textContent = "Instruments played field now disabled.";
  }
}
```

### Descrivere pulsanti non semantici come pulsanti

In più occasioni durante questo corso è stata menzionata l'accessibilità nativa dei pulsanti, dei link e degli elementi dei moduli, nonché i problemi di accessibilità derivanti dall'uso di altri elementi per simularli; vedere [Usare controlli dell'interfaccia utente semantici quando possibile](/it/docs/Learn_web_development/Core/Accessibility/HTML#use_semantic_ui_controls_where_possible) nell'articolo sull'accessibilità HTML e [Migliorare l'accessibilità tramite tastiera](#migliorare_l'accessibilità_tramite_tastiera) sopra. In sostanza, in molti casi è possibile ripristinare l'accessibilità tramite tastiera senza troppe difficoltà usando `tabindex` e un po' di JavaScript.

Ma per quanto riguarda gli screen reader? Continuerebbero a non percepire gli elementi come pulsanti. Se si testa l'esempio [`fake-div-buttons.html`](https://mdn.github.io/learning-area/tools-testing/cross-browser-testing/accessibility/fake-div-buttons.html) in uno screen reader, i pulsanti simulati saranno segnalati con frasi come "Click me!, gruppo", cosa che risulta ovviamente confusa.

È possibile risolvere questo problema tramite un ruolo WAI-ARIA. Creare una copia locale di [`fake-div-buttons.html`](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/accessibility/fake-div-buttons.html) e aggiungere [`role="button"`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/button_role) a ogni `<div>` che rappresenta un pulsante, ad esempio:

```html
<div data-message="This is from the first button" tabindex="0" role="button">
  Click me!
</div>
```

Ora, provando questo esempio con uno screen reader, i pulsanti verranno segnalati con frasi come "Click me!, pulsante". Sebbene sia molto meglio, è comunque necessario aggiungere tutte le funzionalità native dei pulsanti che gli utenti si aspettano, come la gestione degli eventi <kbd>enter</kbd> e click, come spiegato nella [documentazione del ruolo `button`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/button_role).

> [!NOTE]
> Non bisogna tuttavia dimenticare che usare l'elemento semantico corretto quando possibile è sempre meglio. Se si desidera creare un pulsante e si può usare un elemento {{htmlelement("button")}}, si dovrebbe usare un elemento {{htmlelement("button")}}!

### Guidare gli utenti attraverso widget complessi

Esiste un'ampia varietà di altri [ruoli](/it/docs/Web/Accessibility/ARIA/Reference/Roles) che possono identificare strutture di elementi non semantici come comuni funzionalità dell'interfaccia utente che vanno oltre ciò che è disponibile nell'HTML standard, ad esempio [`combobox`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/combobox_role), [`slider`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/slider_role), [`tabpanel`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/tabpanel_role), [`tree`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/tree_role). Per avere un'idea di come rendere accessibili tali controlli, è possibile vedere diversi esempi utili nella [libreria di codice di Deque University](https://dequeuniversity.com/library/).

È inoltre possibile trovare diversi esempi live nella documentazione sui [ruoli WAI-ARIA](/it/docs/Web/Accessibility/ARIA/Reference/Roles). Vedere, ad esempio, l'[esempio del ruolo ARIA `tab`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/tab_role#example), che spiega come implementare un'interfaccia accessibile a schede.

## Riepilogo

Questo articolo non ha affatto trattato tutto ciò che è disponibile in WAI-ARIA, ma dovrebbe aver fornito informazioni sufficienti per comprenderne l'uso e conoscere alcuni dei pattern più comuni che ne richiedono l'utilizzo.

Nel prossimo articolo verranno proposti alcuni test per verificare quanto bene siano state comprese e memorizzate tutte queste informazioni.

## Vedere anche

- [Stati e proprietà ARIA](/it/docs/Web/Accessibility/ARIA/Reference/Attributes): tutti gli attributi `aria-*`
- [Ruoli WAI-ARIA](/it/docs/Web/Accessibility/ARIA/Reference/Roles): categorie di ruoli ARIA e ruoli trattati su MDN
- [ARIA in HTML](https://w3c.github.io/html-aria/) sul W3C: una specifica che definisce, per ogni funzionalità HTML, la semantica di accessibilità ARIA applicata implicitamente dal browser e le funzionalità WAI-ARIA che è possibile impostare qualora sia necessaria semantica aggiuntiva
- [Libreria di codice di Deque University](https://dequeuniversity.com/library/): una libreria di esempi molto utili e pratici che mostrano controlli complessi dell'interfaccia utente resi accessibili tramite funzionalità WAI-ARIA
- [Pratiche di authoring WAI-ARIA](https://www.w3.org/WAI/ARIA/apg/) sul W3C: un pattern di progettazione molto dettagliato del W3C, che spiega come implementare diversi tipi di controlli complessi dell'interfaccia utente rendendoli accessibili tramite funzionalità WAI-ARIA

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/Test_your_skills/CSS_and_JavaScript","Learn_web_development/Core/Accessibility/Test_your_skills/WAI-ARIA", "Learn_web_development/Core/Accessibility")}}
