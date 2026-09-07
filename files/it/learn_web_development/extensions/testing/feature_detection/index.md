---
title: Implementazione del rilevamento delle funzionalità
short-title: Rilevamento delle funzionalità
slug: Learn_web_development/Extensions/Testing/Feature_detection
l10n:
  sourceCommit: 2b4a2ad5d9ba084a9eaa2f9204102655e7b575c4
---

{{PreviousMenuNext("Learn_web_development/Extensions/Testing/HTML_and_CSS","Learn_web_development/Extensions/Testing/Automated_testing", "Learn_web_development/Extensions/Testing")}}

Il rilevamento delle funzionalità consiste nello stabilire se un browser supporta un determinato blocco di codice e nell'eseguire codice differente a seconda che lo supporti (o meno), in modo che il browser possa sempre offrire un'esperienza funzionante anziché bloccarsi o generare errori in alcuni browser. Questo articolo descrive come scrivere semplici test di rilevamento delle funzionalità, come usare una libreria per velocizzare l'implementazione e le funzionalità native per il rilevamento delle funzionalità, come `@supports`.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi fondamentali <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>; una conoscenza
        dei principi generali del
        <a
          href="/it/docs/Learn_web_development/Extensions/Testing/Introduction"
          >testing cross-browser</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere il concetto di rilevamento delle funzionalità ed essere in grado di
        implementare soluzioni appropriate in CSS e JavaScript.
      </td>
    </tr>
  </tbody>
</table>

## Il concetto di rilevamento delle funzionalità

L'idea alla base del rilevamento delle funzionalità è che sia possibile eseguire un test per determinare se una funzionalità è supportata dal browser corrente, quindi eseguire condizionalmente il codice per offrire un'esperienza accettabile sia nei browser che _supportano_ la funzionalità sia in quelli che _non la supportano_. Se ciò non viene fatto, i browser che non supportano le funzionalità usate nel codice potrebbero non visualizzare correttamente i siti o potrebbero non funzionare affatto, creando una pessima esperienza utente.

Riepiloghiamo e osserviamo l'esempio affrontato nell'articolo sul [debugging JavaScript e la gestione degli errori](/it/docs/Learn_web_development/Core/Scripting/Debugging_JavaScript#feature_detection): la [Geolocation API](/it/docs/Web/API/Geolocation_API) (che espone i dati di posizione disponibili per il dispositivo su cui è in esecuzione il browser web) ha come punto di ingresso principale per il suo utilizzo una proprietà `geolocation` disponibile sull'oggetto globale [Navigator](/it/docs/Web/API/Navigator). Pertanto, è possibile rilevare se il browser supporta o meno la geolocalizzazione usando qualcosa di simile al seguente:

```js
if ("geolocation" in navigator) {
  navigator.geolocation.getCurrentPosition((position) => {
    // show the location on a map, such as the Google Maps API
  });
} else {
  // Give the user a choice of static maps
}
```

Prima di proseguire, è importante chiarire una cosa: non confondere il rilevamento delle funzionalità con il **browser sniffing** (rilevare quale browser specifico sta accedendo al sito): si tratta di una pratica pessima che dovrebbe essere scoraggiata a ogni costo. Per ulteriori dettagli, vedere [Rilevamento del browser tramite la stringa user agent (UA sniffing)](/it/docs/Web/HTTP/Guides/Browser_detection_using_the_user_agent).

## Scrivere test di rilevamento delle funzionalità personalizzati

In questa sezione verrà esaminata l'implementazione di test di rilevamento delle funzionalità personalizzati, sia in CSS sia in JavaScript.

### CSS

È possibile scrivere test per le funzionalità CSS verificando l'esistenza di _[element.style.property](/it/docs/Web/API/HTMLElement/style)_ (ad esempio, `paragraph.style.rotate`) in JavaScript.

Un esempio classico potrebbe essere il test del supporto per [Subgrid](/it/docs/Web/CSS/Guides/Grid_layout/Subgrid) in un browser; per i browser che supportano il valore `subgrid` per {{cssxref("grid-template-columns")}} e {{cssxref("grid-template-rows")}}, è possibile usare subgrid nel layout. Per i browser che non lo supportano, si potrebbe usare una normale grid, che funziona bene ma ha un aspetto meno accattivante.

Usando questo come esempio, si potrebbe includere un foglio di stile subgrid se il valore è supportato e un foglio di stile con una normale grid in caso contrario. Per farlo, si potrebbero includere due fogli di stile nella sezione head del file HTML: uno per tutti gli stili e uno che implementa il layout predefinito se subgrid non è supportato:

```html
<link href="basic-styling.css" rel="stylesheet" />
<link class="conditional" href="grid-layout.css" rel="stylesheet" />
```

In questo caso, `basic-styling.css` gestisce tutti gli stili da applicare a ogni browser. Sono presenti due file CSS aggiuntivi, `grid-layout.css` e `subgrid-layout.css`, che contengono il CSS da applicare selettivamente ai browser in base al loro livello di supporto.

Si usa JavaScript per testare il supporto del valore subgrid, quindi si aggiorna l'`href` del foglio di stile condizionale in base al supporto del browser.

È possibile aggiungere un `<script></script>` al documento, contenente il seguente JavaScript:

```js
const conditional = document.querySelector(".conditional");
if (CSS.supports("grid-template-columns", "subgrid")) {
  conditional.setAttribute("href", "subgrid-layout.css");
}
```

Nell'istruzione condizionale, viene verificato se la proprietà {{cssxref("grid-template-columns")}} supporta il valore `subgrid` mediante [`CSS.supports()`](/it/docs/Web/API/CSS/supports_static).

#### @supports

CSS dispone di un meccanismo nativo per il rilevamento delle funzionalità: l'at-rule {{cssxref("@supports")}}. Funziona in modo simile alle [media query](/it/docs/Web/CSS/Guides/Media_queries), ma anziché applicare selettivamente il CSS in base a una funzionalità media come risoluzione, larghezza dello schermo o {{Glossary("aspect_ratio", "rapporto d'aspetto")}}, applica selettivamente il CSS in base al supporto di una funzionalità CSS, in modo simile a `CSS.supports()`.

Ad esempio, l'esempio precedente potrebbe essere riscritto usando `@supports`:

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

Questo blocco di at-rule applica la regola CSS contenuta solo se il browser corrente supporta la dichiarazione `grid-template-columns: subgrid;`. Affinché una condizione con un valore funzioni, è necessario includere una dichiarazione completa, non solo il nome di una proprietà, e NON includere il punto e virgola finale.

`@supports` mette inoltre a disposizione la logica `AND`, `OR` e `NOT`: l'altro blocco applica il normale layout grid se l'opzione subgrid non è disponibile:

```css
@supports not (grid-template-columns: subgrid) {
  /* rules in here */
}
```

Questo approccio è più pratico dell'esempio precedente: è possibile eseguire tutto il rilevamento delle funzionalità in CSS, senza JavaScript, e gestire tutta la logica in un singolo file CSS, riducendo le richieste HTTP. Per questo motivo è il metodo preferito per determinare il supporto del browser per le funzionalità CSS.

### JavaScript

In precedenza è già stato osservato un esempio di test di rilevamento delle funzionalità in JavaScript. In generale, questi test vengono eseguiti tramite alcuni modelli comuni.

I modelli comuni per le funzionalità rilevabili includono:

- Membri di un oggetto
  - : Verificare se un determinato metodo o proprietà, in genere un punto di ingresso per l'utilizzo dell'API o di un'altra funzionalità da rilevare, esiste nel relativo `Object` padre.

    L'esempio precedente ha usato questo modello per rilevare il supporto di [Geolocation](/it/docs/Web/API/Geolocation_API), testando l'oggetto [`navigator`](/it/docs/Web/API/Navigator) per un membro `geolocation`:

    ```js
    if ("geolocation" in navigator) {
      // Access navigator.geolocation APIs
    }
    ```

- Proprietà di un elemento
  - : Creare un elemento in memoria usando [`Document.createElement()`](/it/docs/Web/API/Document/createElement), quindi verificare se una proprietà esiste su di esso.

    Questo esempio mostra un modo per rilevare il supporto di [Canvas API](/it/docs/Web/API/Canvas_API):

    ```js
    function supportsCanvas() {
      return !!document.createElement("canvas").getContext;
    }

    if (supportsCanvas()) {
      // Create and draw on canvas elements
    }
    ```

    > [!NOTE]
    > Il doppio `NOT` nell'esempio precedente (`!!`) è un modo per forzare un valore di ritorno a diventare un valore booleano "corretto", anziché un valore {{Glossary("Truthy", "Truthy")}}/{{Glossary("Falsy", "Falsy")}} che potrebbe alterare i risultati.

- Valori di ritorno specifici di un metodo su un elemento
  - : Creare un elemento in memoria usando [`Document.createElement()`](/it/docs/Web/API/Document/createElement), quindi verificare se esiste un metodo su di esso. Se esiste, verificare quale valore restituisce.

- Mantenimento del valore assegnato a una proprietà da parte di un elemento
  - : Creare un elemento in memoria usando [`Document.createElement()`](/it/docs/Web/API/Document/createElement), impostare una proprietà su un valore specifico, quindi verificare se il valore viene mantenuto.

Tenere presente che alcune funzionalità sono tuttavia note per non essere rilevabili. In questi casi, sarà necessario usare un approccio differente, ad esempio un {{Glossary("Polyfill", "polyfill")}}.

#### matchMedia

A questo punto è utile menzionare anche la funzionalità JavaScript [`Window.matchMedia`](/it/docs/Web/API/Window/matchMedia). Si tratta di una proprietà che permette di eseguire test di media query all'interno di JavaScript. Ha questo aspetto:

```js
if (window.matchMedia("(width <= 480px)").matches) {
  // run JavaScript in here.
}
```

Come esempio, la demo [Snapshot](https://github.com/chrisdavidmills/snapshot) usa questa funzionalità per applicare selettivamente la libreria JavaScript Brick e usarla per gestire il layout dell'interfaccia utente, ma solo per il layout degli schermi piccoli, larghi 480px o meno. Innanzitutto viene usato l'attributo `media` per applicare il CSS Brick alla pagina solo se la larghezza della pagina è pari o inferiore a 480px:

```html
<link href="dist/brick.css" rel="stylesheet" media="(width <= 480px)" />
```

Successivamente, `matchMedia()` viene usato più volte nel JavaScript per eseguire le funzioni di navigazione Brick solo nel layout per schermi piccoli. Nei layout per schermi più larghi, tutto è visibile contemporaneamente, quindi non è necessario navigare tra viste differenti.

```js
if (window.matchMedia("(width <= 480px)").matches) {
  deck.shuffleTo(1);
}
```

## Riepilogo

Questo articolo ha trattato il rilevamento delle funzionalità con un livello ragionevole di dettaglio, esaminando i concetti principali e mostrando come implementare test di rilevamento delle funzionalità personalizzati.

Successivamente verrà esaminato il testing automatizzato.

{{PreviousMenuNext("Learn_web_development/Extensions/Testing/HTML_and_CSS","Learn_web_development/Extensions/Testing/Automated_testing", "Learn_web_development/Extensions/Testing")}}
