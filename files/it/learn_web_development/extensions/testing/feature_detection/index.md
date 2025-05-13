---
title: Implementazione della rilevazione delle funzionalità
short-title: Rilevazione delle funzionalità
slug: Learn_web_development/Extensions/Testing/Feature_detection
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Testing/HTML_and_CSS","Learn_web_development/Extensions/Testing/Automated_testing", "Learn_web_development/Extensions/Testing")}}

La rilevazione delle funzionalità implica determinare se un browser supporta un determinato blocco di codice e eseguire codice differente a seconda che lo supporti (o meno), in modo che il browser possa sempre fornire un'esperienza funzionale piuttosto che bloccarsi o generare errori in alcuni browser. Questo articolo descrive come scrivere una semplice rilevazione delle funzionalità, come utilizzare una libreria per velocizzare l'implementazione e le funzionalità native per la rilevazione delle funzionalità come `@supports`.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi fondamentali
        <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>; un'idea
        dei principi generali di
        <a
          href="/it/docs/Learn_web_development/Extensions/Testing/Introduction"
          >test cross-browser</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere il concetto di rilevazione delle funzionalità e essere in grado di
        implementare le soluzioni adeguate in CSS e JavaScript.
      </td>
    </tr>
  </tbody>
</table>

## Il concetto della rilevazione delle funzionalità

L'idea alla base della rilevazione delle funzionalità è che si può eseguire un test per determinare se una funzionalità è supportata nel browser corrente e quindi eseguire condizionalmente il codice per fornire un'esperienza accettabile sia nei browser che _supportano_ la funzionalità, sia nei browser che _non_ la supportano. Se non lo si fa, i browser che non supportano le funzionalità utilizzate nel codice potrebbero non visualizzare correttamente i siti o potrebbero fallire del tutto, creando una cattiva esperienza utente.

Facciamo un breve riassunto e guardiamo l'esempio che abbiamo toccato nel nostro articolo [Debugging e gestione degli errori in JavaScript](/it/docs/Learn_web_development/Core/Scripting/Debugging_JavaScript#feature_detection) — la [Geolocation API](/it/docs/Web/API/Geolocation_API) (che espone i dati disponibili sulla posizione per il dispositivo su cui il browser web è in esecuzione) ha il suo punto di ingresso principale in una proprietà `geolocation` disponibile sull'oggetto globale [Navigator](/it/docs/Web/API/Navigator). Pertanto, è possibile rilevare se il browser supporta la geolocalizzazione o meno utilizzando qualcosa di simile al seguente:

```js
if ("geolocation" in navigator) {
  navigator.geolocation.getCurrentPosition(function (position) {
    // show the location on a map, such as the Google Maps API
  });
} else {
  // Give the user a choice of static maps
}
```

Prima di procedere, vorremmo dire una cosa chiaramente — non confonda la rilevazione delle funzionalità con il **browser sniffing** (rilevamento di quale browser sta accedendo al sito) — questa è una pratica terribile che dovrebbe essere scoraggiata a tutti i costi. Veda [Rilevazione del browser utilizzando la stringa user agent (UA sniffing)](/it/docs/Web/HTTP/Guides/Browser_detection_using_the_user_agent) per ulteriori dettagli.

## Scrivere i propri test di rilevazione delle funzionalità

In questa sezione, esamineremo l'implementazione di test di rilevazione delle funzionalità propri, sia in CSS che in JavaScript.

### CSS

È possibile scrivere test per le funzionalità CSS testando l'esistenza di _[element.style.property](/it/docs/Web/API/HTMLElement/style)_ (ad esempio, `paragraph.style.rotate`) in JavaScript.

Un esempio classico potrebbe essere quello di testare il supporto per [Subgrid](/it/docs/Web/CSS/CSS_grid_layout/Subgrid) in un browser; per i browser che supportano il valore `subgrid` per i valori subgrid di [`grid-template-columns`](/it/docs/Web/CSS/grid-template-columns) e [`grid-template-rows`](/it/docs/Web/CSS/grid-template-rows), possiamo utilizzare subgrid nel nostro layout. Per i browser che non lo supportano, potremmo utilizzare la griglia regolare che funziona bene ma non è altrettanto accattivante.

Utilizzando questo come esempio, potremmo includere un foglio di stile subgrid se il valore è supportato e un foglio di stile della griglia regolare in caso contrario. Per fare ciò, potremmo includere due fogli di stile nell'intestazione del nostro file HTML: uno per tutta la formattazione e uno che implementa il layout predefinito se subgrid non è supportato:

```html
<link href="basic-styling.css" rel="stylesheet" />
<link class="conditional" href="grid-layout.css" rel="stylesheet" />
```

Qui, `basic-styling.css` gestisce tutta la formattazione che vogliamo dare a ogni browser. Abbiamo due ulteriori file CSS, `grid-layout.css` e `subgrid-layout.css`, che contengono il CSS che vogliamo applicare selettivamente ai browser in base ai loro livelli di supporto.

Utilizziamo JavaScript per testare il supporto per il valore subgrid, quindi aggiorniamo il `href` del nostro foglio di stile condizionale in base al supporto del browser.

Possiamo aggiungere un `<script></script>` al nostro documento, riempito con il seguente JavaScript

```js
const conditional = document.querySelector(".conditional");
if (CSS.supports("grid-template-columns", "subgrid")) {
  conditional.setAttribute("href", "subgrid-layout.css");
}
```

Nel nostro istruzione condizionale, testiamo se la proprietà {{cssxref("grid-template-columns")}} supporta il valore `subgrid` usando [`CSS.supports()`](/it/docs/Web/API/CSS/supports_static).

#### @supports

Il CSS ha un meccanismo nativo di rilevazione delle funzionalità: l'at-rule {{cssxref("@supports")}}. Questo funziona in modo simile alle [query multimediali](/it/docs/Web/CSS/CSS_media_queries) tranne per il fatto che invece di applicare selettivamente il CSS a seconda di una caratteristica multimediale come una risoluzione, una larghezza dello schermo o un {{Glossary("aspect_ratio", "rapporto di aspetto")}}, applica selettivamente il CSS a seconda che una funzionalità CSS sia supportata, simile a `CSS.supports()`.

Ad esempio, potremmo riscrivere il nostro esempio precedente usando `@supports`:

```css
@supports (grid-template-columns: subgrid) {
  main {
    display: grid;
    grid-template-columns: repeat(9, 1fr);
    grid-template-rows: repeat(4, minmax(100px, auto));
  }

  .item {
    display: grid;
    grid-column: 2 / 7;
    grid-row: 2 / 4;
    grid-template-columns: subgrid;
    grid-template-rows: repeat(3, 80px);
  }

  .subitem {
    grid-column: 3 / 6;
    grid-row: 1 / 3;
  }
}
```

Questo blocco at-rule applica la regola CSS al suo interno solo se il browser corrente supporta la dichiarazione `grid-template-columns: subgrid;`. Perché una condizione con un valore funzioni, è necessario includere una dichiarazione completa (non solo un nome di proprietà) e NON includere il punto e virgola alla fine.

`@supports` dispone anche della logica `AND`, `OR`, e `NOT` — l'altro blocco applica il layout della griglia regolare se l'opzione subgrid non è disponibile:

```css
@supports not (grid-template-columns: subgrid) {
  /* rules in here */
}
```

Questo è più conveniente rispetto all'esempio precedente — possiamo fare tutte le nostre rilevazioni delle funzionalità in CSS, nessun JavaScript richiesto, e possiamo gestire tutta la logica in un singolo file CSS, riducendo le richieste HTTP. Per questo motivo è il metodo preferito per determinare il supporto del browser per le funzionalità CSS.

### JavaScript

Abbiamo già visto un esempio di un test di rilevazione delle funzionalità JavaScript in precedenza. In generale, tali test vengono eseguiti tramite uno dei pochi schemi comuni.

Schemi comuni per le funzionalità rilevabili includono:

- Membri di un oggetto

  - : Controllare se un particolare metodo o proprietà (tipicamente un punto di ingresso nell'utilizzo dell'API o di un'altra funzionalità che si sta rilevando) esiste nel relativo `Object` padre.

    Il nostro esempio precedente ha utilizzato questo schema per rilevare il supporto per la [Geolocation](/it/docs/Web/API/Geolocation_API) testando l'oggetto [`navigator`](/it/docs/Web/API/Navigator) per un membro `geolocation`:

    ```js
    if ("geolocation" in navigator) {
      // Access navigator.geolocation APIs
    }
    ```

- Proprietà di un elemento

  - : Creare un elemento in memoria utilizzando [`Document.createElement()`](/it/docs/Web/API/Document/createElement) e quindi controllare se una proprietà esiste su di esso.

    Questo esempio mostra un modo per rilevare il supporto per la [Canvas API](/it/docs/Web/API/Canvas_API):

    ```js
    function supports_canvas() {
      return !!document.createElement("canvas").getContext;
    }

    if (supports_canvas()) {
      // Create and draw on canvas elements
    }
    ```

    > [!NOTE]
    > Il doppio `NOT` nell'esempio sopra (`!!`) è un modo per forzare un valore di ritorno a diventare un valore booleano "proprio", piuttosto che un valore {{Glossary("Truthy", "Truthy")}}/{{Glossary("Falsy", "Falsy")}} che potrebbe distorcere i risultati.

- Valori di ritorno specifici di un metodo su un elemento

  - : Creare un elemento in memoria utilizzando [`Document.createElement()`](/it/docs/Web/API/Document/createElement) e quindi controllare se un metodo esiste su di esso. Se esiste, controllare quale valore restituisce.

- Ritenzione del valore assegnato dalla proprietà di un elemento

  - : Creare un elemento in memoria utilizzando [`Document.createElement()`](/it/docs/Web/API/Document/createElement), impostare una proprietà su un valore specifico, quindi controllare se il valore è mantenuto.

Tenga presente che alcune funzionalità sono, tuttavia, note per essere non rilevabili. In questi casi, sarà necessario utilizzare un approccio diverso, come l'utilizzo di un {{Glossary("Polyfill", "polyfill")}}.

#### matchMedia

Vogliamo anche menzionare la funzionalità JavaScript [`Window.matchMedia`](/it/docs/Web/API/Window/matchMedia) a questo punto. Questa è una proprietà che consente di eseguire test di query multimediali all'interno di JavaScript. Appare così:

```js
if (window.matchMedia("(max-width: 480px)").matches) {
  // run JavaScript in here.
}
```

Ad esempio, la nostra demo [Snapshot](https://github.com/chrisdavidmills/snapshot) ne fa uso per applicare selettivamente la libreria JavaScript Brick e usarla per gestire il layout dell'interfaccia utente, ma solo per il layout dello schermo piccolo (480px di larghezza o meno). Per prima cosa utilizziamo l'attributo `media` per applicare il CSS di Brick alla pagina solo se la larghezza della pagina è di 480px o meno:

```html
<link
  href="dist/brick.css"
  rel="stylesheet"
  media="all and (max-width: 480px)" />
```

Poi utilizziamo `matchMedia()` nel JavaScript diverse volte, per eseguire solo le funzioni di navigazione Brick se siamo nel layout dello schermo piccolo (nei layout a schermo più ampio, tutto può essere visto insieme, quindi non è necessario navigare tra diverse viste).

```js
if (window.matchMedia("(max-width: 480px)").matches) {
  deck.shuffleTo(1);
}
```

## Riepilogo

Questo articolo ha trattato la rilevazione delle funzionalità in un buon livello di dettaglio, passando attraverso i concetti principali e mostrando come implementare i propri test di rilevazione delle funzionalità.

Successivamente, inizieremo a esaminare i test automatizzati.

{{PreviousMenuNext("Learn_web_development/Extensions/Testing/HTML_and_CSS","Learn_web_development/Extensions/Testing/Automated_testing", "Learn_web_development/Extensions/Testing")}}
