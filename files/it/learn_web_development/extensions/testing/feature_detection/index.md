---
title: Implementazione della rilevazione delle funzionalità
short-title: Rilevazione delle funzionalità
slug: Learn_web_development/Extensions/Testing/Feature_detection
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Testing/HTML_and_CSS","Learn_web_development/Extensions/Testing/Automated_testing", "Learn_web_development/Extensions/Testing")}}

La rilevazione delle funzionalità implica determinare se un browser supporta un certo blocco di codice e eseguire codice diverso a seconda che lo supporti (o meno), in modo che il browser possa sempre fornire un'esperienza di funzionamento piuttosto che bloccarsi o generare errori in alcuni browser. Questo articolo dettaglia come scrivere la propria semplice rilevazione delle funzionalità, come utilizzare una libreria per accelerare l'implementazione e le funzionalità native per la rilevazione delle funzionalità come `@supports`.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con le basi del <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e delle lingue
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>; un'idea
        dei principi generali
        <a
          href="/it/docs/Learn_web_development/Extensions/Testing/Introduction"
          >del testing cross-browser</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere il concetto di rilevazione delle funzionalità e saper
        implementare soluzioni appropriate in CSS e JavaScript.
      </td>
    </tr>
  </tbody>
</table>

## Il concetto di rilevazione delle funzionalità

L'idea alla base della rilevazione delle funzionalità è che è possibile eseguire un test per determinare se una funzionalità è supportata nel browser corrente e quindi eseguire condizionalmente codice per fornire un'esperienza accettabile sia nei browser che _supportano_ la funzionalità, sia nei browser che _non la supportano_. Se non lo fai, i browser che non supportano le funzionalità che usi nel tuo codice potrebbero non visualizzare correttamente i tuoi siti o potrebbero non funzionare affatto, creando una cattiva esperienza utente.

Ricordiamo e guardiamo l'esempio su cui abbiamo toccato nel nostro articolo [Debugging JavaScript e gestione degli errori](/it/docs/Learn_web_development/Core/Scripting/Debugging_JavaScript#feature_detection) — l'[API Geolocation](/it/docs/Web/API/Geolocation_API) (che espone i dati di posizione disponibili per il dispositivo su cui il browser web è in esecuzione) ha il punto di ingresso principale per il suo utilizzo, una proprietà `geolocation` disponibile sull'oggetto globale [Navigator](/it/docs/Web/API/Navigator). Pertanto, puoi rilevare se il browser supporta la geolocalizzazione o meno utilizzando qualcosa di simile:

```js
if ("geolocation" in navigator) {
  navigator.geolocation.getCurrentPosition(function (position) {
    // show the location on a map, such as the Google Maps API
  });
} else {
  // Give the user a choice of static maps
}
```

Prima di procedere, vorremmo dire una cosa chiaramente — non confondere la rilevazione delle funzionalità con il **browser sniffing** (rilevamento di quale browser specifico sta accedendo al sito) — questa è una pratica terribile che dovrebbe essere scoraggiata a tutti i costi. Consulta [Rilevamento del browser utilizzando la stringa dell'utente (UA sniffing)](/it/docs/Web/HTTP/Guides/Browser_detection_using_the_user_agent) per ulteriori dettagli.

## Scrivere i propri test di rilevazione delle funzionalità

In questa sezione, guarderemo l'implementazione dei propri test di rilevazione delle funzionalità, sia in CSS che in JavaScript.

### CSS

È possibile scrivere test per funzionalità CSS testando l'esistenza di _[element.style.property](/it/docs/Web/API/HTMLElement/style)_ (ad esempio, `paragraph.style.rotate`) in JavaScript.

Un classico esempio potrebbe essere testare il supporto per [Subgrid](/it/docs/Web/CSS/CSS_grid_layout/Subgrid) in un browser; per i browser che supportano il valore `subgrid` per il valore subgrid per [`grid-template-columns`](/it/docs/Web/CSS/grid-template-columns) e [`grid-template-rows`](/it/docs/Web/CSS/grid-template-rows), possiamo utilizzare subgrid nel nostro layout. Per i browser che non lo supportano, possiamo usare una griglia normale che funziona bene ma non è altrettanto accattivante.

Usando questo come esempio, potremmo includere uno stylesheet subgrid se il valore è supportato e uno stylesheet di griglia normale se non lo è. Per farlo, potremmo includere due stylesheet nell'intestazione del nostro file HTML: uno per tutto lo stile, e uno che implementa il layout predefinito se subgrid non è supportato:

```html
<link href="basic-styling.css" rel="stylesheet" />
<link class="conditional" href="grid-layout.css" rel="stylesheet" />
```

Qui, `basic-styling.css` gestisce tutto lo stile che vogliamo dare a ogni browser. Abbiamo due file CSS aggiuntivi, `grid-layout.css` e `subgrid-layout.css`, che contengono il CSS che vogliamo applicare selettivamente ai browser a seconda dei loro livelli di supporto.

Usiamo JavaScript per testare il supporto per il valore subgrid, quindi aggiorniamo l`'href` del nostro stylesheet condizionale basato sul supporto del browser.

Possiamo aggiungere uno `<script></script>` al nostro documento, riempito con il seguente JavaScript:

```js
const conditional = document.querySelector(".conditional");
if (CSS.supports("grid-template-columns", "subgrid")) {
  conditional.setAttribute("href", "subgrid-layout.css");
}
```

Nella nostra dichiarazione condizionale, testiamo per vedere se la proprietà {{cssxref("grid-template-columns")}} supporta il valore `subgrid` utilizzando [`CSS.supports()`](/it/docs/Web/API/CSS/supports_static).

#### @supports

CSS ha un meccanismo nativo di rilevazione delle funzionalità: l'istruzione @supports di {{cssxref("@supports")}}. Funziona in modo simile alle [media queries](/it/docs/Web/CSS/CSS_media_queries) tranne che invece di applicare selettivamente CSS a seconda di una caratteristica media come una risoluzione, larghezza dello schermo o {{Glossary("aspect_ratio", "rapporto d'aspetto")}}, applica selettivamente CSS a seconda se una funzione CSS è supportata, simile a `CSS.supports()`.

Ad esempio, potremmo riscrivere il nostro esempio precedente per utilizzare `@supports`:

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

Questo blocco `@supports` applica la regola CSS all'interno solo se il browser corrente supporta la dichiarazione `grid-template-columns: subgrid;`. Per far funzionare una condizione con un valore, è necessario includere una dichiarazione completa (non solo un nome di proprietà) e NON includere il punto e virgola alla fine.

`@supports` ha anche logica `AND`, `OR`, e `NOT` disponibile — l'altro blocco applica il layout a griglia regolare se l'opzione subgrid non è disponibile:

```css
@supports not (grid-template-columns: subgrid) {
  /* rules in here */
}
```

Questo è più conveniente dell'esempio precedente — possiamo fare tutto il rilevamento delle funzionalità in CSS, senza bisogno di JavaScript, e possiamo gestire tutta la logica in un singolo file CSS, riducendo le richieste HTTP. Per questo motivo è il metodo preferito per determinare il supporto del browser per le funzionalità CSS.

### JavaScript

Abbiamo già visto un esempio di test di rilevazione delle funzionalità JavaScript prima. Generalmente, tali test vengono eseguiti attraverso uno dei pochi modelli comuni.

I modelli comuni per le funzionalità rilevabili includono:

- Membri di un oggetto

  - : Verifica se un particolare metodo o proprietà (in genere un punto di ingresso nell'API o altra funzione che stai rilevando) esiste nel suo `Object` genitore.

    Il nostro esempio precedente utilizzava questo modello per rilevare il supporto [Geolocation](/it/docs/Web/API/Geolocation_API) testando l'oggetto [`navigator`](/it/docs/Web/API/Navigator) per un membro `geolocation`:

    ```js
    if ("geolocation" in navigator) {
      // Access navigator.geolocation APIs
    }
    ```

- Proprietà di un elemento

  - : Crea un elemento in memoria usando [`Document.createElement()`](/it/docs/Web/API/Document/createElement) e poi verifica se una proprietà esiste su di esso.

    Questo esempio mostra un modo per rilevare il supporto per l'[API Canvas](/it/docs/Web/API/Canvas_API):

    ```js
    function supports_canvas() {
      return !!document.createElement("canvas").getContext;
    }

    if (supports_canvas()) {
      // Create and draw on canvas elements
    }
    ```

    > [!NOTE]
    > Il doppio `NOT` nell'esempio sopra (`!!`) è un modo per forzare che un valore di ritorno diventi un valore booleano "proprio", piuttosto che un valore {{Glossary("Truthy", "Truthy")}}/{{Glossary("Falsy", "Falsy")}} che potrebbe distorcere i risultati.

- Valori di ritorno specifici di un metodo su un elemento

  - : Crea un elemento in memoria usando [`Document.createElement()`](/it/docs/Web/API/Document/createElement) e poi verifica se un metodo esiste su di esso. Se esiste, controlla quale valore restituisce.

- Ritenzione del valore di proprietà assegnato da un elemento

  - : Crea un elemento in memoria usando [`Document.createElement()`](/it/docs/Web/API/Document/createElement), imposta una proprietà su un valore specifico, quindi verifica se il valore è stato mantenuto.

Tieni presente che alcune funzionalità sono, tuttavia, note per non essere rilevabili. In questi casi, sarà necessario utilizzare un approccio diverso, come utilizzare un {{Glossary("Polyfill", "polyfill")}}.

#### matchMedia

Volevamo anche menzionare la funzione JavaScript [`Window.matchMedia`](/it/docs/Web/API/Window/matchMedia) a questo punto. Questa è una proprietà che permette di eseguire test delle media query all'interno di JavaScript. Sembra così:

```js
if (window.matchMedia("(max-width: 480px)").matches) {
  // run JavaScript in here.
}
```

Ad esempio, la nostra demo [Snapshot](https://github.com/chrisdavidmills/snapshot) lo utilizza per applicare selettivamente la libreria JavaScript Brick e utilizzarla per gestire il layout dell'interfaccia utente, ma solo per il layout dello schermo piccolo (480px di larghezza o meno). Iniziamo utilizzando l'attributo `media` per applicare il CSS Brick alla pagina solo se la larghezza della pagina è 480px o meno:

```html
<link
  href="dist/brick.css"
  rel="stylesheet"
  media="all and (max-width: 480px)" />
```

Usiamo quindi `matchMedia()` nel JavaScript più volte, per eseguire le funzioni di navigazione di Brick solo se siamo nel layout dello schermo piccolo (nei layout a schermo più largo, tutto può essere visto contemporaneamente, quindi non abbiamo bisogno di navigare tra diverse visuali).

```js
if (window.matchMedia("(max-width: 480px)").matches) {
  deck.shuffleTo(1);
}
```

## Riepilogo

Questo articolo ha coperto la rilevazione delle funzionalità in un discreto dettaglio, passando attraverso i concetti principali e mostrandoti come implementare i tuoi test di rilevazione delle funzionalità.

Successivamente, inizieremo a esaminare i test automatizzati.

{{PreviousMenuNext("Learn_web_development/Extensions/Testing/HTML_and_CSS","Learn_web_development/Extensions/Testing/Automated_testing", "Learn_web_development/Extensions/Testing")}}
