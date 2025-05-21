---
title: Disegno grafico
slug: Learn_web_development/Extensions/Client-side_APIs/Drawing_graphics
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Client-side_APIs/Video_and_audio_APIs", "Learn_web_development/Extensions/Client-side_APIs/Client-side_storage", "Learn_web_development/Extensions/Client-side_APIs")}}

Il browser contiene alcuni strumenti molto potenti per la programmazione grafica, dal linguaggio Scalable Vector Graphics ([SVG](/it/docs/Web/SVG)), alle API per disegnare sugli elementi HTML {{htmlelement("canvas")}}, (vedi [The Canvas API](/it/docs/Web/API/Canvas_API) e [WebGL](/it/docs/Web/API/WebGL_API)). Questo articolo fornisce un'introduzione a canvas e ulteriori risorse per permetterti di imparare di più.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, specialmente <a href="/it/docs/Learn_web_development/Core/Scripting/Object_basics">Nozioni di base sugli oggetti JavaScript</a> e la copertura delle API di base come <a href="/it/docs/Learn_web_development/Core/Scripting/DOM_scripting">DOM scripting</a> e <a href="/it/docs/Learn_web_development/Core/Scripting/Network_requests">Richieste di rete</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>I concetti e i casi d'uso abilitati dalle API trattate in questa lezione.</li>
          <li>Sintassi di base e uso di <code>&lt;canvas&gt;</code> e delle relative API.</li>
          <li>Utilizzare i timer e <code>requestAnimationFrame()</code> per impostare cicli di animazione.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Grafica sul Web

Il Web era originariamente solo testo, il che era molto noioso, quindi furono introdotte le immagini — prima tramite l'elemento {{htmlelement("img")}} e successivamente tramite proprietà CSS come {{cssxref("background-image")}}, e [SVG](/it/docs/Web/SVG).

Tuttavia, ciò non era ancora sufficiente. Mentre potevi utilizzare [CSS](/it/docs/Learn_web_development/Core/Styling_basics) e [JavaScript](/it/docs/Learn_web_development/Core/Scripting) per animare (e altrimenti manipolare) le immagini vettoriali SVG — poiché sono rappresentate da markup — non c'era ancora modo di fare lo stesso per le immagini bitmap, e gli strumenti disponibili erano piuttosto limitati. Il Web non aveva ancora modo di creare efficacemente animazioni, giochi, scene 3D e altre esigenze comunemente gestite con lingue di basso livello come C++ o Java.

La situazione iniziò a migliorare quando i browser iniziarono a supportare l'elemento {{htmlelement("canvas")}} e l'associata [Canvas API](/it/docs/Web/API/Canvas_API) nel 2004. Come vedrai sotto, canvas fornisce alcuni strumenti utili per creare animazioni 2D, giochi, visualizzazioni di dati e altri tipi di applicazioni, soprattutto se combinati con alcune delle altre API che la piattaforma web fornisce, ma può essere difficile o impossibile renderli accessibili.

L'esempio qui sotto mostra un'animazione semplice di palle rimbalzanti basata su canvas 2D che abbiamo incontrato originariamente nel nostro modulo [Introduzione agli oggetti JavaScript](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_building_practice):

{{EmbedGHLiveSample("learning-area/javascript/oojs/bouncing-balls/index-finished.html", '100%', 500)}}

Intorno al 2006–2007, Mozilla iniziò a lavorare su una implementazione tridimensionale sperimentale del canvas. Questo divenne [WebGL](/it/docs/Web/API/WebGL_API), che guadagnò terreno tra i venditori di browser, e fu standardizzato intorno al 2009–2010. WebGL ti permette di creare grafica 3D reale dentro al tuo browser web; l'esempio qui sotto mostra un semplice cubo WebGL che ruota:

{{EmbedGHLiveSample("learning-area/javascript/apis/drawing-graphics/threejs-cube/index.html", '100%', 500)}}

Questo articolo si concentrerà principalmente sul canvas 2D, poiché il codice WebGL grezzo è molto complesso. Tuttavia, mostreremo come utilizzare una libreria WebGL per creare una scena 3D più facilmente, e puoi trovare un tutorial che copre il WebGL grezzo altrove — vedi [Introduzione a WebGL](/it/docs/Web/API/WebGL_API/Tutorial/Getting_started_with_WebGL).

## Apprendimento attivo: Iniziare con un \<canvas>

Se vuoi creare una scena 2D _o_ 3D su una pagina web, devi iniziare con un elemento HTML {{htmlelement("canvas")}}. Questo elemento è utilizzato per definire l'area sulla pagina in cui l'immagine verrà disegnata. È semplice come includere l'elemento nella pagina:

```html
<canvas width="320" height="240"></canvas>
```

Questo creerà un canvas sulla pagina con una dimensione di 320 per 240 pixel.

Dovresti inserire del contenuto di fallback all'interno dei tag `<canvas>`. Questo dovrebbe descrivere il contenuto del canvas agli utenti di browser che non supportano il canvas, o agli utenti di screen reader.

```html
<canvas width="320" height="240">
  <p>Description of the canvas for those unable to view it.</p>
</canvas>
```

Il fallback dovrebbe fornire contenuti alternativi utili al contenuto del canvas. Per esempio, se stai rendendo un grafico di prezzi azionari che si aggiorna costantemente, il contenuto del fallback potrebbe essere un'immagine statica dell'ultimo grafico azionario, con testo `alt` che dice quali sono i prezzi in testo o un elenco di link alle singole pagine dei titoli.

> [!NOTE]
> Il contenuto del canvas non è accessibile agli screen reader. Includere del testo descrittivo come valore dell'attributo [`aria-label`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-label) direttamente sull'elemento canvas stesso o includere contenuto di fallback posizionato all'interno dei tag di apertura e chiusura `<canvas>`. Il contenuto del canvas non fa parte del DOM, ma il contenuto di fallback annidato sì.

### Creare e dimensionare il nostro canvas

Iniziamo creando il nostro canvas su cui disegneremo esperimenti futuri.

1. Per prima cosa, crea una copia locale della directory [0_canvas_start](https://github.com/mdn/learning-area/tree/main/javascript/apis/drawing-graphics/getting-started/0_canvas_start). Contiene tre file:
   - "index.html"
   - "script.js"
   - "style.css"
2. Apri "index.html", e aggiungi il seguente codice subito sotto il tag di apertura {{htmlelement("body")}}:

   ```html
   <canvas class="myCanvas">
     <p>Add suitable fallback here.</p>
   </canvas>
   ```

   Abbiamo aggiunto una `class` all'elemento `<canvas>` in modo che sarà più facile selezionarlo se avremo molteplici canvases sulla pagina, ma per ora abbiamo rimosso gli attributi `width` e `height` (puoi aggiungerli nuovamente se vuoi, ma li imposteremo usando JavaScript in una sezione successiva). I canvases senza una larghezza e altezza esplicita predefiniscono 300 pixel di larghezza per 150 pixel di altezza.

3. Ora apri "script.js" e aggiungi le seguenti righe di JavaScript:

   ```js
   const canvas = document.querySelector(".myCanvas");
   const width = (canvas.width = window.innerWidth);
   const height = (canvas.height = window.innerHeight);
   ```

   Qui abbiamo memorizzato un riferimento al canvas nella costante `canvas`. Nella seconda riga, impostiamo sia una nuova costante `width` che la proprietà `width` del canvas uguale a [`Window.innerWidth`](/it/docs/Web/API/Window/innerWidth) (che ci fornisce la larghezza della viewport). Nella terza riga, impostiamo sia una nuova costante `height` che la proprietà `height` del canvas uguale a [`Window.innerHeight`](/it/docs/Web/API/Window/innerHeight) (che ci fornisce l'altezza della viewport). Ora abbiamo un canvas che riempie l'intera larghezza e altezza della finestra del browser!

   Vedrai anche che stiamo concatenando assegnazioni insieme con multipli segni di uguale — questo è permesso in JavaScript, ed è una buona tecnica se vuoi far sì che molteplici variabili siano tutte uguali allo stesso valore. Volevamo rendere la larghezza del canvas e l'altezza facilmente accessibili nelle variabili width/height, poiché sono valori utili da avere disponibili per dopo (per esempio, se vuoi disegnare qualcosa esattamente a metà della larghezza del canvas).

> [!NOTE]
> Dovresti generalmente impostare la dimensione dell'immagine usando attributi HTML o proprietà DOM, come spiegato sopra. Potresti usare CSS, ma il problema è che il dimensionamento viene fatto dopo che il canvas è stato eseguito il rendering, e come qualsiasi altra immagine (il canvas eseguito il rendering è solo un'immagine), l'immagine potrebbe diventare pixelata/deformata.

### Ottenere il contesto del canvas e impostazione finale

Abbiamo bisogno di fare un'ultima cosa prima di poter considerare il nostro modello di canvas finito. Per disegnare sul canvas, dobbiamo ottenere un riferimento speciale all'area di disegno chiamata contesto. Questo viene fatto utilizzando il metodo [`HTMLCanvasElement.getContext()`](/it/docs/Web/API/HTMLCanvasElement/getContext), che per un utilizzo di base prende una singola stringa come parametro che rappresenta il tipo di contesto che vuoi recuperare.

In questo caso vogliamo un canvas 2d, quindi aggiungi la seguente riga di JavaScript sotto le altre in "script.js":

```js
const ctx = canvas.getContext("2d");
```

> [!NOTE]
> Altri valori di contesto che potresti scegliere includono `webgl` per WebGL, `webgl2` per WebGL 2, ecc., ma non ci serviranno in questo articolo.

Quindi questo è tutto — il nostro canvas è ora pronto per essere disegnato! La variabile `ctx` ora contiene un oggetto [`CanvasRenderingContext2D`](/it/docs/Web/API/CanvasRenderingContext2D), e tutte le operazioni di disegno sul canvas coinvolgeranno la manipolazione di questo oggetto.

Facciamo un'ultima cosa prima di procedere. Coloreremo il background del canvas di nero per darti un primo assaggio dell'API del canvas. Aggiungi le seguenti righe in fondo al tuo JavaScript:

```js
ctx.fillStyle = "rgb(0 0 0)";
ctx.fillRect(0, 0, width, height);
```

Qui stiamo impostando un colore di riempimento usando la proprietà [`fillStyle`](/it/docs/Web/API/CanvasRenderingContext2D/fillStyle) del canvas (questo prende [valori colore](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#color) proprio come fanno le proprietà CSS), poi disegniamo un rettangolo che copre l'intera area del canvas con il metodo [`fillRect`](/it/docs/Web/API/CanvasRenderingContext2D/fillRect) (i primi due parametri sono le coordinate dell'angolo in alto a sinistra del rettangolo; gli ultimi due sono la larghezza e l'altezza che vuoi che il rettangolo sia disegnato — ti abbiamo detto che quelle variabili di larghezza e altezza sarebbero state utili)!

OK, il nostro template è fatto ed è ora di andare avanti.

## Basi del canvas 2D

Come abbiamo detto sopra, tutte le operazioni di disegno vengono eseguite manipolando un oggetto [`CanvasRenderingContext2D`](/it/docs/Web/API/CanvasRenderingContext2D) (nel nostro caso, `ctx`). Molte operazioni devono essere fornite di coordinate per individuare esattamente dove disegnare qualcosa — il lato in alto a sinistra del canvas è il punto (0, 0), l'asse orizzontale (x) va da sinistra a destra e l'asse verticale (y) va dall'alto verso il basso.

![Carta millimetrata in cui piccoli quadrati coprono l'area con un quadrato blu acciaio al centro. L'angolo in alto a sinistra del canvas è il punto (0, 0) degli assi x e y del canvas. L'asse orizzontale (x) va da sinistra a destra denotando la larghezza, e l'asse verticale (y) va dall'alto verso il basso denotando l'altezza. L'angolo in alto a sinistra del quadrato blu è etichettato come una distanza di x unità dall'asse y e y unità dall'asse x.](canvas_default_grid.png)

Disegnare forme tende a essere fatto usando il primitivo della forma rettangolare, o tracciando una linea lungo un certo percorso e poi riempiendo la forma. Di seguito mostreremo come fare entrambi.

### Rettangoli semplici

Cominciamo con alcuni semplici rettangoli.

1. Prima di tutto, prendi una copia del tuo nuovo template canvas codificato (o fai una copia locale della directory [1_canvas_template](https://github.com/mdn/learning-area/tree/main/javascript/apis/drawing-graphics/getting-started/1_canvas_template) se non hai seguito i passi sopra).
2. Successivamente, aggiungi le seguenti righe alla fine del tuo JavaScript:

   ```js
   ctx.fillStyle = "rgb(255 0 0)";
   ctx.fillRect(50, 50, 100, 150);
   ```

   Se salvi e aggiorni, dovresti vedere che un rettangolo rosso è apparso sul tuo canvas. Il suo angolo in alto a sinistra si trova a 50 pixel di distanza dal bordo superiore e dal bordo sinistro del canvas (come definito dai primi due parametri), ed è largo 100 pixel e alto 150 pixel (come definito dal terzo e quarto parametro).

3. Aggiungiamo un altro rettangolo nel mix — uno verde questa volta. Aggiungi il seguente alla fine del tuo JavaScript:

   ```js
   ctx.fillStyle = "rgb(0 255 0)";
   ctx.fillRect(75, 75, 100, 100);
   ```

   Salva e aggiorna, e vedrai il tuo nuovo rettangolo. Questo solleva un punto importante: le operazioni grafiche come disegnare rettangoli, linee e così via vengono eseguite nell'ordine in cui si verificano. Pensa ad esso come dipingere un muro, dove ogni mano di vernice si sovrappone e può persino nascondere ciò che c'è sotto. Non puoi fare nulla per cambiare questo, quindi devi pensare attentamente all'ordine in cui disegni la grafica.

4. Nota che puoi disegnare grafiche semi-trasparenti specificando un colore semi-trasparente, ad esempio utilizzando `rgb()`. Il "canale alfa" definisce la quantità di trasparenza che il colore ha. Maggiore è il suo valore, più esso coprirà quello che c'è dietro. Aggiungi il seguente al tuo codice:

   ```js
   ctx.fillStyle = "rgb(255 0 255 / 75%)";
   ctx.fillRect(25, 100, 175, 50);
   ```

5. Ora prova a disegnare alcuni altri rettangoli di tua scelta; divertiti!

### Contorni e spessori di linea

Finora abbiamo visto disegnare rettangoli riempiti, ma puoi anche disegnare rettangoli che sono solo contorni (chiamati **strokes** nel design grafico). Per impostare il colore che vuoi per il tuo contorno, usi la proprietà [`strokeStyle`](/it/docs/Web/API/CanvasRenderingContext2D/strokeStyle); disegnare un rettangolo di contorno viene fatto usando [`strokeRect`](/it/docs/Web/API/CanvasRenderingContext2D/strokeRect).

1. Aggiungi il seguente al precedente esempio, di nuovo sotto le precedenti righe di JavaScript:

   ```js
   ctx.strokeStyle = "rgb(255 255 255)";
   ctx.strokeRect(25, 25, 175, 200);
   ```

2. La larghezza predefinita dei contorni è di 1 pixel; puoi regolare il valore di [`lineWidth`](/it/docs/Web/API/CanvasRenderingContext2D/lineWidth) per cambiarlo (prende un numero che rappresenta il numero di pixel di larghezza del contorno). Aggiungi la seguente riga tra le due righe precedenti:

   ```js
   ctx.lineWidth = 5;
   ```

Ora dovresti vedere che il tuo contorno bianco è diventato molto più spesso! Questo è tutto per ora. A questo punto il tuo esempio dovrebbe assomigliare a questo:

{{EmbedGHLiveSample("learning-area/javascript/apis/drawing-graphics/getting-started/2_canvas_rectangles/index.html", '100%', 250)}}

> [!NOTE]
> Il codice finito è disponibile su GitHub come [2_canvas_rectangles](https://github.com/mdn/learning-area/tree/main/javascript/apis/drawing-graphics/getting-started/2_canvas_rectangles).

### Disegnare percorsi

Se vuoi disegnare qualcosa di più complesso di un rettangolo, devi disegnare un percorso. Fondamentalmente, questo coinvolge la scrittura di codice per specificare esattamente quale percorso la penna dovrebbe seguire sul tuo canvas per tracciare la forma che vuoi disegnare. Canvas include funzioni per disegnare linee rette, cerchi, curve di Bézier e altro.

Iniziamo la sezione facendo una nuova copia del nostro template canvas ([1_canvas_template](https://github.com/mdn/learning-area/tree/main/javascript/apis/drawing-graphics/getting-started/1_canvas_template)), in cui disegnare il nuovo esempio.

Utilizzeremo alcuni metodi e proprietà comuni in tutte le sezioni seguenti:

- [`beginPath()`](/it/docs/Web/API/CanvasRenderingContext2D/beginPath) — inizia a disegnare un percorso nel punto in cui la penna si trova attualmente sul canvas. Su un nuovo canvas, la penna parte da (0, 0).
- [`moveTo()`](/it/docs/Web/API/CanvasRenderingContext2D/moveTo) — sposta la penna in un altro punto sul canvas, senza registrare o tracciare la linea; la penna "salta" alla nuova posizione.
- [`fill()`](/it/docs/Web/API/CanvasRenderingContext2D/fill) — disegna una forma riempita riempiendo nel percorso che hai tracciato finora.
- [`stroke()`](/it/docs/Web/API/CanvasRenderingContext2D/stroke) — disegna una forma di contorno disegnando un contorno lungo il percorso che hai tracciato finora.
- Puoi anche usare funzionalità come `lineWidth` e `fillStyle`/`strokeStyle` con percorsi oltre che con rettangoli.

Un'operazione tipica e semplice di disegno di un percorso potrebbe assomigliare a così:

```js
ctx.fillStyle = "rgb(255 0 0)";
ctx.beginPath();
ctx.moveTo(50, 50);
// draw your path
ctx.fill();
```

#### Disegnare linee

Disegniamo un triangolo equilatero sul canvas.

1. Innanzitutto, aggiungi la seguente funzione helper in fondo al tuo codice. Questo converte i valori di grado in radianti, il che è utile perché ogni volta che devi fornire un valore di angolo in JavaScript, sarà quasi sempre in radianti, ma gli esseri umani di solito pensano in gradi.

   ```js
   function degToRad(degrees) {
     return (degrees * Math.PI) / 180;
   }
   ```

2. Successivamente, inizia il tuo percorso aggiungendo il seguente sotto alla tua aggiunta precedente; qui impostiamo un colore per il nostro triangolo, iniziamo a disegnare un percorso, e poi spostiamo la penna a (50, 50) senza disegnare nulla. È lì che inizieremo a disegnare il nostro triangolo.

   ```js
   ctx.fillStyle = "rgb(255 0 0)";
   ctx.beginPath();
   ctx.moveTo(50, 50);
   ```

3. Ora aggiungi le seguenti righe in fondo al tuo script:

   ```js
   ctx.lineTo(150, 50);
   const triHeight = 50 * Math.tan(degToRad(60));
   ctx.lineTo(100, 50 + triHeight);
   ctx.lineTo(50, 50);
   ctx.fill();
   ```

   Esaminiamolo in ordine:

   Prima disegniamo una linea fino a (150, 50) — il nostro percorso ora va di 100 pixel a destra lungo l'asse x.

   In secondo luogo, calcoliamo l'altezza del nostro triangolo equilatero, utilizzando un po' di trigonometria semplice. Fondamentalmente, stiamo disegnando il triangolo puntato verso il basso. Gli angoli in un triangolo equilatero sono sempre 60 gradi; per calcolare l'altezza possiamo dividerlo a metà formando due triangoli rettangoli, che avranno ciascuno angoli di 90 gradi, 60 gradi e 30 gradi. In termini di lati:

   - Il lato più lungo è chiamato **ipotenusa**
   - Il lato accanto all'angolo di 60 gradi è chiamato **adiacente** — che sappiamo essere 50 pixel, poiché è la metà della linea che abbiamo appena disegnato.
   - Il lato opposto all'angolo di 60 gradi è chiamato **opposto**, che è l'altezza del triangolo che vogliamo calcolare.

   ![Un triangolo equilatero che punta verso il basso con angoli e lati etichettati. La linea orizzontale in alto è etichettata come 'adiacente'. Una linea punteggiata perpendicolare, dal centro della linea adiacente, etichettata 'opposta', divide il triangolo creando due triangoli rettangoli uguali. Il lato destro del triangolo è etichettato come ipotenusa, poiché è l'ipotenusa del triangolo rettangolo formato dalla linea etichettata 'opposta'. Anche se tutti i tre lati del triangolo sono di uguale lunghezza, l'ipotenusa è il lato più lungo del triangolo rettangolo.](trigonometry.png)

   Una delle formule trigonometriche di base afferma che la lunghezza dell'adiacente moltiplicata per la tangente dell'angolo è uguale all'opposto, quindi giungiamo a `50 * Math.tan(degToRad(60))`. Utilizziamo la nostra funzione `degToRad()` per convertire 60 gradi in radianti, poiché {{jsxref("Math.tan()")}} si aspetta un valore di input in radianti.

4. Con l'altezza calcolata, disegniamo un'altra linea a `(100, 50 + triHeight)`. La coordinata X è semplice; deve essere il punto medio tra i due valori X precedenti che abbiamo impostato. Il valore Y invece deve essere 50 più l'altezza del triangolo, in quanto sappiamo che la parte superiore del triangolo si trova a 50 pixel dalla parte superiore del canvas.
5. La riga successiva disegna una linea che ritorna al punto di partenza del triangolo.
6. Infine, eseguiamo `ctx.fill()` per terminare il percorso e riempire la forma.

#### Disegnare cerchi

Ora vediamo come disegnare un cerchio su canvas. Questo viene realizzato utilizzando il metodo [`arc()`](/it/docs/Web/API/CanvasRenderingContext2D/arc), che disegna tutto o parte di un cerchio in un punto specificato.

1. Aggiungiamo un arco al nostro canvas — aggiungi il seguente in fondo al tuo codice:

   ```js
   ctx.fillStyle = "rgb(0 0 255)";
   ctx.beginPath();
   ctx.arc(150, 106, 50, degToRad(0), degToRad(360), false);
   ctx.fill();
   ```

   `arc()` prende sei parametri. I primi due specificano la posizione del centro dell'arco (X e Y, rispettivamente). Il terzo è il raggio del cerchio, il quarto e il quinto sono gli angoli di inizio e di fine in cui disegnare il cerchio (quindi specificando 0 e 360 gradi ci dà un cerchio completo), e il sesto parametro definisce se il circolo deve essere disegnato in senso antiorario o orario (`false` è orario).

   > [!NOTE]
   > 0 gradi è orizzontalmente a destra.

2. Proviamo ad aggiungere un altro arco:

   ```js
   ctx.fillStyle = "yellow";
   ctx.beginPath();
   ctx.arc(200, 106, 50, degToRad(-45), degToRad(45), true);
   ctx.lineTo(200, 106);
   ctx.fill();
   ```

   Il modello qui è molto simile, ma con due differenze:

   - Abbiamo impostato l'ultimo parametro di `arc()` su `true`, il che significa che l'arco viene disegnato in senso antiorario, il che significa che anche se l'arco è specificato per iniziare a -45 gradi e terminare a 45 gradi, disegniamo l'arco attorno ai 270 gradi fuori dalla porzione specificata. Se dovessi cambiare `true` in `false` e quindi eseguire nuovamente il codice, verrebbe disegnata solo la fetta di 90 gradi del cerchio.
   - Prima di chiamare `fill()`, disegniamo una linea al centro del cerchio. Questo significa che otteniamo il piuttosto carino ritaglio in stile Pac-Man che viene renderizzato. Se rimuovessi questa linea (provaci!) e quindi eseguissi nuovamente il codice, otterresti solo un bordo del cerchio tagliato tra il punto di inizio e di fine dell'arco. Questo illustra un altro importante punto del canvas — se provi a riempire un percorso incompleto (cioè, uno che non è chiuso), il browser riempie una linea retta tra i punti di inizio e di fine e quindi lo riempie.

Questo è tutto per ora; il tuo esempio finale dovrebbe assomigliare a questo:

{{EmbedGHLiveSample("learning-area/javascript/apis/drawing-graphics/getting-started/3_canvas_paths/index.html", '100%', 200)}}

> [!NOTE]
> Il codice finito è disponibile su GitHub come [3_canvas_paths](https://github.com/mdn/learning-area/tree/main/javascript/apis/drawing-graphics/getting-started/3_canvas_paths).

> [!NOTE]
> Per saperne di più su funzionalità avanzate di disegno percorsi come le curve di Bézier, dai un'occhiata al nostro tutorial [Disegnare forme con canvas](/it/docs/Web/API/Canvas_API/Tutorial/Drawing_shapes).

### Testo

Canvas ha anche funzionalità per disegnare testo. Esploriamo brevemente queste. Inizia a fare un'altra copia fresca del nostro template canvas ([1_canvas_template](https://github.com/mdn/learning-area/tree/main/javascript/apis/drawing-graphics/getting-started/1_canvas_template)) in cui disegnare il nuovo esempio.

Il testo viene disegnato utilizzando due metodi:

- [`fillText()`](/it/docs/Web/API/CanvasRenderingContext2D/fillText) — disegna testo riempito.
- [`strokeText()`](/it/docs/Web/API/CanvasRenderingContext2D/strokeText) — disegna testo di contorno (stroke).

Entrambi questi metodi prendono tre proprietà nel loro utilizzo base: la stringa di testo da disegnare e le coordinate X e Y del punto per iniziare a disegnare il testo. Questo si traduce nell'angolo **in basso a sinistra** della **casella di testo** (letteralmente, la casella che circonda il testo che disegni), il che potrebbe confonderti dato che altre operazioni di disegno tendono a partire dall'angolo in alto a sinistra — tienilo presente.

Ci sono anche un certo numero di proprietà per aiutare a controllare il rendering del testo come [`font`](/it/docs/Web/API/CanvasRenderingContext2D/font), che ti permette di specificare la famiglia di caratteri, la dimensione, ecc. Prende come valore la stessa sintassi della proprietà CSS {{cssxref("font")}}.

Il contenuto del canvas non è accessibile agli screen reader. Il testo dipinto sul canvas non è disponibile nel DOM, ma deve essere reso disponibile per essere accessibile. In questo esempio, includiamo il testo come valore per `aria-label`.

Prova ad aggiungere il seguente blocco in fondo al tuo JavaScript:

```js
ctx.strokeStyle = "white";
ctx.lineWidth = 1;
ctx.font = "36px arial";
ctx.strokeText("Canvas text", 50, 50);

ctx.fillStyle = "red";
ctx.font = "48px georgia";
ctx.fillText("Canvas text", 50, 150);

canvas.setAttribute("aria-label", "Canvas text");
```

Qui disegniamo due righe di testo, una outline e l'altra stroke. L'esempio finale dovrebbe somigliare a così:

{{EmbedGHLiveSample("learning-area/javascript/apis/drawing-graphics/getting-started/4_canvas_text/index.html", '100%', 180)}}

> [!NOTE]
> Il codice finito è disponibile su GitHub come [4_canvas_text](https://github.com/mdn/learning-area/tree/main/javascript/apis/drawing-graphics/getting-started/4_canvas_text).

Prova a giocare e vedere cosa puoi creare! Puoi trovare più informazioni sulle opzioni disponibili per il testo su canvas a [Drawing text](/it/docs/Web/API/Canvas_API/Tutorial/Drawing_text).

### Disegnare immagini su canvas

È possibile renderizzare immagini esterne sul tuo canvas. Queste possono essere immagini semplici, frame di video, o il contenuto di altri canvas. Per il momento guarderemo solo il caso di utilizzare alcune immagini semplici sul nostro canvas.

1. Come prima, fai un'altra copia fresca del nostro template canvas ([1_canvas_template](https://github.com/mdn/learning-area/tree/main/javascript/apis/drawing-graphics/getting-started/1_canvas_template)) in cui disegnare il nuovo esempio.

   Le immagini vengono disegnate sul canvas utilizzando il metodo [`drawImage()`](/it/docs/Web/API/CanvasRenderingContext2D/drawImage). La versione più semplice prende tre parametri — un riferimento all'immagine che vuoi renderizzare, e le coordinate X e Y dell'angolo in alto a sinistra dell'immagine.

2. Iniziamo recuperando una sorgente di immagini da incorporare nel nostro canvas. Aggiungi le seguenti righe in fondo al tuo JavaScript:

   ```js
   const image = new Image();
   image.src = "firefox.png";
   ```

   Qui creiamo un nuovo oggetto [`HTMLImageElement`](/it/docs/Web/API/HTMLImageElement) utilizzando il costruttore [`Image()`](/it/docs/Web/API/HTMLImageElement/Image). L'oggetto restituito è dello stesso tipo di quello che viene restituito quando ottieni un riferimento a un elemento esistente {{htmlelement("img")}}. Poi impostiamo il suo attributo [`src`](/it/docs/Web/HTML/Reference/Elements/img#src) per essere uguale alla nostra immagine del logo di Firefox. A questo punto, il browser inizia a caricare l'immagine.

3. Potremmo ora provare a incorporare l'immagine usando `drawImage()`, ma dobbiamo assicurarci che il file immagine sia stato caricato prima, altrimenti il codice fallirà. Possiamo raggiungere questo risultato usando l'evento `load`, che verrà emesso solo quando l'immagine ha finito di caricarsi. Aggiungi il seguente blocco sotto quello precedente:

   ```js
   image.addEventListener("load", () => ctx.drawImage(image, 20, 20));
   ```

   Se carichi il tuo esempio nel browser ora, dovresti vedere l'immagine incorporata nel canvas.

4. Ma c'è di più! E se volessimo visualizzare solo una parte dell'immagine o ridimensionarla? Possiamo fare entrambi con la versione più complessa di `drawImage()`. Aggiorna la tua riga `ctx.drawImage()` in questo modo:

   ```js
   ctx.drawImage(image, 20, 20, 185, 175, 50, 50, 185, 175);
   ```

   - Il primo parametro è il riferimento all'immagine, come prima.
   - I parametri 2 e 3 definiscono le coordinate dell'angolo in alto a sinistra dell'area che vuoi ritagliare dall'immagine caricata, relativo all'angolo in alto a sinistra dell'immagine stessa. Nulla a sinistra del primo parametro o sopra il secondo verrà disegnato.
   - I parametri 4 e 5 definiscono la larghezza e l'altezza dell'area che vogliamo ritagliare dall'immagine originale che abbiamo caricato.
   - I parametri 6 e 7 definiscono le coordinate a cui vuoi disegnare l'angolo in alto a sinistra della porzione ritagliata dell'immagine, relativo all'angolo in alto a sinistra del canvas.
   - I parametri 8 e 9 definiscono la larghezza e l'altezza per disegnare l'area ritagliata dell'immagine. In questo caso, abbiamo specificato le stesse dimensioni della fetta originale, ma potresti ridimensionarla specificando valori diversi.

5. Quando l'immagine viene aggiornata in modo significativo, la {{Glossary("accessible_description", "descrizione accessibile")}} deve essere anche aggiornata.

   ```js
   canvas.setAttribute("aria-label", "Firefox Logo");
   ```

L'esempio finale dovrebbe assomigliare a così:

{{EmbedGHLiveSample("learning-area/javascript/apis/drawing-graphics/getting-started/5_canvas_images/index.html", '100%', 260)}}

> [!NOTE]
> Il codice finito è disponibile su GitHub come [5_canvas_images](https://github.com/mdn/learning-area/tree/main/javascript/apis/drawing-graphics/getting-started/5_canvas_images).

## Cicli e animazioni

Fino a ora abbiamo coperto alcuni usi molto basici del canvas 2D, ma veramente non sperimenterai tutta la potenza del canvas a meno che non lo aggiorni o animi in qualche modo. Dopotutto, il canvas fornisce immagini scriptabili! Se non hai intenzione di cambiare nulla, potresti semplicemente utilizzare immagini statiche e risparmiarti tutto il lavoro.

### Creare un ciclo

Giocare con cicli in canvas è piuttosto divertente — puoi eseguire comandi di canvas all'interno di un ciclo [`for`](/it/docs/Web/JavaScript/Reference/Statements/for) (o di altro tipo) proprio come qualsiasi altro codice JavaScript.

Costruiamo un esempio.

1. Fai un'altra copia fresca del nostro template canvas ([1_canvas_template](https://github.com/mdn/learning-area/tree/main/javascript/apis/drawing-graphics/getting-started/1_canvas_template)) e aprilo nel tuo editor di codice.
2. Aggiungi la seguente riga in fondo al tuo JavaScript. Questo contiene un nuovo metodo, [`translate()`](/it/docs/Web/API/CanvasRenderingContext2D/translate), che sposta il punto di origine del canvas:

   ```js
   ctx.translate(width / 2, height / 2);
   ```

   Questo provoca lo spostamento dell'origine delle coordinate (0, 0) al centro del canvas, anziché essere nell'angolo in alto a sinistra. Questo è molto utile in molte situazioni, come questa, dove vogliamo che il nostro design sia disegnato in relazione al centro del canvas.

3. Ora aggiungi il seguente codice in fondo al JavaScript:

   ```js
   function degToRad(degrees) {
     return (degrees * Math.PI) / 180;
   }

   function rand(min, max) {
     return Math.floor(Math.random() * (max - min + 1)) + min;
   }

   let length = 250;
   let moveOffset = 20;

   for (let i = 0; i < length; i++) {}
   ```

   Qui stiamo implementando la stessa funzione `degToRad()` che abbiamo visto nell'esempio del triangolo sopra, una funzione `rand()` che restituisce un numero casuale tra limiti inferiori e superiori dati, variabili `length` e `moveOffset` (di cui scopriremo di più più tardi), e un ciclo `for` vuoto.

4. L'idea qui è che disegneremo qualcosa sul canvas all'interno del ciclo `for`, e lo itereremo ogni volta così possiamo creare qualcosa di interessante. Aggiungi il seguente codice dentro il tuo ciclo `for`:

   ```js
   ctx.fillStyle = `rgb(${255 - length} 0 ${255 - length} / 90%)`;
   ctx.beginPath();
   ctx.moveTo(moveOffset, moveOffset);
   ctx.lineTo(moveOffset + length, moveOffset);
   const triHeight = (length / 2) * Math.tan(degToRad(60));
   ctx.lineTo(moveOffset + length / 2, moveOffset + triHeight);
   ctx.lineTo(moveOffset, moveOffset);
   ctx.fill();

   length--;
   moveOffset += 0.7;
   ctx.rotate(degToRad(5));
   ```

   Quindi a ogni iterazione, noi:

   - Impostiamo `fillStyle` per essere una sfumatura di viola leggermente trasparente, che cambia ogni volta in base al valore di `length`. Come vedrai successivamente la lunghezza diventa più piccola ogni volta che il loop viene eseguito, quindi l'effetto qui è che il colore diventa più brillante con ogni triangolo successivo disegnato.
   - Iniziamo il percorso.
   - Spostiamo la penna in una coordinata di `(moveOffset, moveOffset)`; questa variabile definisce di quanto vogliamo spostarci ogni volta che disegniamo un nuovo triangolo.
   - Disegniamo una linea a una coordinata di `(moveOffset+length, moveOffset)`. Questo disegna una linea di lunghezza `length` parallela all'asse X.
   - Calcoliamo l'altezza del triangolo, come prima.
   - Disegniamo una linea fino all'angolo inferiore che punta del triangolo, poi disegniamo una linea per tornare all'inizio del triangolo.
   - Chiamiamo `fill()` per riempire il triangolo.
   - Aggiorniamo le variabili che descrivono la sequenza dei triangoli, così possiamo essere pronti per disegnare il prossimo. Diminuiamo il valore di `length` di 1, così i triangoli diventano più piccoli ogni volta; incrementiamo `moveOffset` di una piccola quantità così ogni triangolo successivo è leggermente più lontano, e utilizziamo un'altra nuova funzione, [`rotate()`](/it/docs/Web/API/CanvasRenderingContext2D/rotate), che ci permette di ruotare l'intero canvas! Lo ruotiamo di 5 gradi prima di disegnare il prossimo triangolo.

Questo è tutto! L'esempio finale dovrebbe assomigliare a così:

{{EmbedGHLiveSample("learning-area/javascript/apis/drawing-graphics/loops_animation/6_canvas_for_loop/index.html", '100%', 550)}}

A questo punto, ti incoraggiamo a giocare con l'esempio e renderlo tuo! Per esempio:

- Disegna rettangoli o archi invece di triangoli, o addirittura incorpora immagini.
- Gioca con i valori di `length` e `moveOffset`.
- Introduci alcuni numeri casuali usando quella funzione `rand()` che abbiamo incluso sopra ma non usato.

> [!NOTE]
> Il codice finito è disponibile su GitHub come [6_canvas_for_loop](https://github.com/mdn/learning-area/tree/main/javascript/apis/drawing-graphics/loops_animation/6_canvas_for_loop).

### Animazioni

Il ciclo di esempio che abbiamo costruito sopra era divertente, ma davvero hai bisogno di un ciclo costante che continua e continua per qualsiasi applicazione di canvas seria (come giochi e visualizzazioni in tempo reale). Se pensi al tuo canvas come a un film, vuoi veramente che il display si aggiorni a ogni frame per mostrare la vista aggiornata, con un ideale tasso di refresh di 60 frame al secondo in modo che il movimento appaia bello e fluido all'occhio umano.

Ci sono alcune funzioni JavaScript che ti permetteranno di eseguire funzioni ripetutamente, diverse volte al secondo, la migliore per i nostri scopi qui è [`window.requestAnimationFrame()`](/it/docs/Web/API/Window/requestAnimationFrame). Prende un parametro — il nome della funzione che vuoi eseguire per ogni frame. La prossima volta che il browser è pronto ad aggiornare lo schermo, la tua funzione verrà chiamata. Se quella funzione disegna il nuovo aggiornamento alla tua animazione, poi chiama `requestAnimationFrame()` di nuovo appena prima della fine della funzione, il ciclo dell'animazione continuerà a funzionare. Il ciclo termina quando smetti di chiamare `requestAnimationFrame()` o se chiami [`window.cancelAnimationFrame()`](/it/docs/Web/API/Window/cancelAnimationFrame) dopo aver chiamato `requestAnimationFrame()` ma prima che venga chiamato il frame.

> [!NOTE]
> È buona pratica chiamare `cancelAnimationFrame()` dal tuo codice principale quando hai finito di usare l'animazione, per assicurarti che nessun aggiornamento sia ancora in attesa di essere eseguito.

Il browser si occupa di dettagli complessi come far funzionare l'animazione a una velocità costante, e non sprecare risorse animando cose che non possono essere viste.

Per vedere come funziona, diamo un'occhiata al nostro esempio di palle rimbalzanti ([vedi dal vivo](https://mdn.github.io/learning-area/javascript/oojs/bouncing-balls/index-finished.html), e anche vedi [il codice sorgente](https://github.com/mdn/learning-area/tree/main/javascript/oojs/bouncing-balls)). Il codice per il ciclo che tiene tutto in movimento sembra così:

```js
function loop() {
  ctx.fillStyle = "rgb(0 0 0 / 25%)";
  ctx.fillRect(0, 0, width, height);

  for (const ball of balls) {
    ball.draw();
    ball.update();
    ball.collisionDetect();
  }

  requestAnimationFrame(loop);
}

loop();
```

Eseguiamo la funzione `loop()` una volta in fondo al codice per iniziare il ciclo, disegnando il primo frame dell'animazione; la funzione `loop()` poi prende il controllo della chiamata `requestAnimationFrame(loop)` per eseguire il prossimo frame dell'animazione, ancora e ancora.

Nota che su ogni frame stiamo completamente cancellando il canvas e ridisegnando tutto. Per ogni palla presente la disegniamo, aggiorniamo la sua posizione, e controlliamo se si sta scontrando con altre palle. Una volta che hai disegnato una grafica su un canvas, non c'è modo di manipolare quella grafica individualmente come puoi con gli elementi DOM. Non puoi muovere ogni palla intorno al canvas, perché una volta che è stata disegnata, è parte del canvas e non è un elemento o oggetto accessibile individualmente. Invece, devi cancellare e ridisegnare, o cancellando l'intero frame e ridisegnando tutto, o avendo del codice che sa esattamente quali parti devono essere cancellate e solo cancella e ridisegna l'area minima del canvas necessaria.

Ottimizzare l'animazione di grafiche è una specialità intera nella programmazione, con molte tecniche intelligenti disponibili. Quelle sono al di là di ciò che ci serve per il nostro esempio, comunque!

In generale, il processo di fare un'animazione su canvas coinvolge i seguenti passi:

1. Cancella il contenuto del canvas (ad esempio, con [`fillRect()`](/it/docs/Web/API/CanvasRenderingContext2D/fillRect) o [`clearRect()`](/it/docs/Web/API/CanvasRenderingContext2D/clearRect)).
2. Salva lo stato (se necessario) usando [`save()`](/it/docs/Web/API/CanvasRenderingContext2D/save) — questo è necessario quando vuoi salvare le impostazioni che hai aggiornato sul canvas prima di continuare, il che è utile per applicazioni più avanzate.
3. Disegna le grafiche che stai animando.
4. Ripristina le impostazioni che hai salvato nel passaggio 2, usando [`restore()`](/it/docs/Web/API/CanvasRenderingContext2D/restore)
5. Chiama `requestAnimationFrame()` per programmare il disegno del prossimo frame dell'animazione.

> [!NOTE]
> Non copriremo `save()` e `restore()` qui, ma sono spiegati bene nel nostro tutorial [Transformations](/it/docs/Web/API/Canvas_API/Tutorial/Transformations) (e quelli che seguono).

### Una semplice animazione di un personaggio

Ora creiamo la nostra semplice animazione — faremo camminare un personaggio di un certo gioco di computer retro piuttosto fantastico sullo schermo.

1. Fai un'altra copia fresca del nostro template canvas ([1_canvas_template](https://github.com/mdn/learning-area/tree/main/javascript/apis/drawing-graphics/getting-started/1_canvas_template)) e aprilo nel tuo editor di codice.

2. Aggiorna l'HTML interno per riflettere l'immagine:

   ```html
   <canvas class="myCanvas">
     <p>A man walking.</p>
   </canvas>
   ```

3. Aggiungi alla fine del JavaScript la seguente riga per fare ancora una volta in modo che l'origine delle coordinate si sieda nel mezzo del canvas:

   ```js
   ctx.translate(width / 2, height / 2);
   ```

4. Ora creiamo un nuovo oggetto [`HTMLImageElement`](/it/docs/Web/API/HTMLImageElement), impostiamo il suo [`src`](/it/docs/Web/HTML/Reference/Elements/img#src) sull'immagine che vogliamo caricare, e aggiungiamo un gestore di evento `onload` che farà scattare la funzione `draw()` quando l'immagine sarà carica:

   ```js
   const image = new Image();
   image.src = "walk-right.png";
   image.onload = draw;
   ```

5. Ora aggiungeremo alcune variabili per tenere traccia della posizione in cui vogliamo disegnare lo sprite sullo schermo, e del numero dello sprite che vogliamo mostrare.

   ```js
   let sprite = 0;
   let posX = 0;
   ```

   Spieghiamo l'immagine del foglio di sprite (che abbiamo preso in prestito rispettosamente dal [Walking cycle using CSS animation](https://codepen.io/mikethomas/pen/kQjKLW) di Mike Thomas su CodePen). L'immagine appare così:

   ![Un foglio di sprite con sei immagini sprite di un personaggio pixelato simile a una persona che cammina dal loro lato destro in diversi momenti di un solo passo in avanti. Il personaggio ha una camicia bianca con bottoni azzurri, pantaloni neri e scarpe nere. Ogni sprite è largo 102 pixel e alto 148 pixel.](walk-right.png)

   Contiene sei sprite che compongono l'intera sequenza di camminata — ciascuno è largo 102 pixel e alto 148 pixel. Per visualizzare ogni sprite in modo pulito, dovremo usare `drawImage()` per tagliare un'immagine sprite singola dal foglio di sprite e mostrare solo quella parte, come abbiamo fatto sopra con il logo di Firefox. La coordinata X della fetta dovrà essere un multiplo di 102, e la coordinata Y sarà sempre 0. La dimensione della fetta sarà sempre 102 per 148 pixel.

6. Ora inseriamo una funzione `draw()` vuota in fondo al codice, pronta per essere riempita con un po' di codice:

   ```js
   function draw() {}
   ```

7. Il resto del codice in questa sezione va all'interno di `draw()`. Per prima cosa, aggiungi la seguente riga, che cancella il canvas per preparare il disegno di ogni frame. Nota che dobbiamo specificare l'angolo in alto a sinistra del rettangolo come `-(width/2), -(height/2)` perché abbiamo specificato la posizione dell'origine come `width/2, height/2` in precedenza.

   ```js
   ctx.fillRect(-(width / 2), -(height / 2), width, height);
   ```

8. Successivamente, disegneremo la nostra immagine usando drawImage — la versione a 9 parametri. Aggiungi il seguente:

   ```js
   ctx.drawImage(image, sprite * 102, 0, 102, 148, 0 + posX, -74, 102, 148);
   ```

   Come puoi vedere:

   - Specifichiamo `image` come l'immagine da incorporare.
   - I parametri 2 e 3 specificano l'angolo in alto a sinistra della fetta da tagliare dall'originale immagine, con il valore X come `sprite` moltiplicato per 102 (dove `sprite` è il numero dello sprite tra 0 e 5) e il valore Y sempre 0.
   - I parametri 4 e 5 specificano la dimensione della fetta da tagliare — 102 pixel per 148 pixel.
   - I parametri 6 e 7 specificano l'angolo in alto a sinistra della casella in cui disegnare la fetta sul canvas — la posizione X è 0 + `posX`, il che significa che possiamo alterare la posizione di disegno alterando il valore `posX`.
   - I parametri 8 e 9 specificano la dimensione dell'immagine sul canvas. Vogliamo solo mantenere la sua dimensione originale, quindi specifichiamo 102 e 148 come larghezza e altezza.

9. Ora cambieremo il valore `sprite` dopo ogni disegno — beh, dopo alcuni di essi almeno. Aggiungi il seguente blocco in fondo alla funzione `draw()`:

   ```js
   if (posX % 13 === 0) {
     if (sprite === 5) {
       sprite = 0;
     } else {
       sprite++;
     }
   }
   ```

   Stiamo avvolgendo l'intero blocco in `if (posX % 13 === 0) { }`. Usiamo l'operatore modulo (`%`) (conosciuto anche come [remainder operator](/it/docs/Web/JavaScript/Reference/Operators/Remainder)) per controllare se il valore di `posX` può essere esattamente diviso per 13 senza resto. Se lo è, passiamo allo sprite successivo incrementando `sprite` (tornando a 0 dopo che abbiamo finito con lo sprite #5). Questo significa effettivamente che stiamo aggiornando lo sprite solo ogni 13° frame, o approssimativamente circa 5 frame al secondo (`requestAnimationFrame()` ci chiama fino a 60 frame al secondo se possibile). Stiamo deliberatamente rallentando il frame rate perché abbiamo solo sei sprite con cui lavorare, e se ne mostriamo uno ogni 60° di secondo, il nostro personaggio si muoverà decisamente troppo velocemente!

   All'interno del blocco esterno usiamo un'istruzione [`if...else`](/it/docs/Web/JavaScript/Reference/Statements/if...else) per controllare se il valore di `sprite` è a 5 (l'ultimo sprite, dato che i numeri dello sprite vanno da 0 a 5). Se stiamo già mostrando l'ultimo sprite, reimpostiamo `sprite` a 0; se no incrementiamo semplicemente di 1.

10. Successivamente, dobbiamo capire come cambiare il valore `posX` su ogni frame — aggiungi il seguente blocco di codice appena sotto l'ultimo.

    ```js
    if (posX > width / 2) {
      let newStartPos = -(width / 2 + 102);
      posX = Math.ceil(newStartPos);
      console.log(posX);
    } else {
      posX += 2;
    }
    ```

    Stiamo usando un'altra istruzione `if...else` per vedere se il valore di `posX` è diventato maggiore di `width/2`, il che significa che il nostro personaggio ha camminato fuori dal bordo destro dello schermo. Se è così, calcoliamo una posizione che metterebbe il personaggio appena a sinistra del lato sinistro dello schermo.

    Se il nostro personaggio non è ancora uscito dal bordo dello schermo, incrementiamo `posX` di 2. Questo lo farà muovere leggermente verso destra la prossima volta che lo disegneremo.

11. Infine, dobbiamo far sì che il ciclo dell'animazione continui chiamando [`requestAnimationFrame()`](/it/docs/Web/API/Window/requestAnimationFrame) in fondo alla funzione `draw()`:

    ```js
    window.requestAnimationFrame(draw);
    ```

Questo è tutto! L'esempio finale dovrebbe sembrare così:

{{EmbedGHLiveSample("learning-area/javascript/apis/drawing-graphics/loops_animation/7_canvas_walking_animation/index.html", '100%', 260)}}

> [!NOTE]
> Il codice finito è disponibile su GitHub come [7_canvas_walking_animation](https://github.com/mdn/learning-area/tree/main/javascript/apis/drawing-graphics/loops_animation/7_canvas_walking_animation).

### Una semplice applicazione di disegno

Come ultimo esempio di animazione, vogliamo mostrarti un'applicazione di disegno molto semplice, per illustrare come il ciclo di animazione possa essere combinato con input utente (come il movimento del mouse, in questo caso). Non ti faremo eseguire questo passaggio; esploreremo solo le parti più interessanti del codice.

L'esempio può essere trovato su GitHub come [8_canvas_drawing_app](https://github.com/mdn/learning-area/tree/main/javascript/apis/drawing-graphics/loops_animation/8_canvas_drawing_app), e puoi giocare con esso dal vivo qui sotto:

{{EmbedGHLiveSample("learning-area/javascript/apis/drawing-graphics/loops_animation/8_canvas_drawing_app/index.html", '100%', 600)}}

Vediamo le parti più interessanti. Innanzitutto, teniamo traccia delle coordinate X e Y del mouse e se viene cliccato o no con tre variabili: `curX`, `curY` e `pressed`. Quando il mouse si sposta, eseguiamo una funzione impostata come gestore di evento `onmousemove`, che cattura i valori X e Y correnti. Usiamo anche i gestori di evento `onmousedown` e `onmouseup` per cambiare il valore di `pressed` in `true` quando il pulsante del mouse viene premuto, e di nuovo in `false` quando viene rilasciato.

```js
let curX;
let curY;
let pressed = false;

// update mouse pointer coordinates
document.addEventListener("mousemove", (e) => {
  curX = e.pageX;
  curY = e.pageY;
});

canvas.addEventListener("mousedown", () => (pressed = true));

canvas.addEventListener("mouseup", () => (pressed = false));
```

Quando il pulsante "Clear canvas" viene premuto, eseguiamo una semplice funzione che cancella l'intero canvas tornando a nero, nello stesso modo che abbiamo visto prima:

```js
clearBtn.addEventListener("click", () => {
  ctx.fillStyle = "rgb(0 0 0)";
  ctx.fillRect(0, 0, width, height);
});
```

Il ciclo di disegno è piuttosto semplice questa volta — se pressed è `true`, disegniamo un cerchio con uno stile di riempimento uguale al valore nel selettore del colore, e un raggio uguale al valore impostato nel range input. Dobbiamo disegnare il cerchio 85 pixel sopra rispetto a dove lo abbiamo misurato, perché la misura verticale è presa dalla parte superiore della finestra del viewport, ma stiamo disegnando il cerchio relativo alla parte superiore del canvas, che inizia sotto la barra degli strumenti alta 85 pixel. Se lo disegnassimo con solo `curY` come coordinata y, apparirebbe 85 pixel più in basso rispetto alla posizione del mouse.

```js
function draw() {
  if (pressed) {
    ctx.fillStyle = colorPicker.value;
    ctx.beginPath();
    ctx.arc(
      curX,
      curY - 85,
      sizePicker.value,
      degToRad(0),
      degToRad(360),
      false,
    );
    ctx.fill();
  }

  requestAnimationFrame(draw);
}

draw();
```

Tutti i tipi di {{htmlelement("input")}} sono ben supportati. Se un browser non supporta un tipo di input, farà il fallback a semplici campi di testo.

## WebGL

È giunto il momento di lasciare indietro il 2D e dare una breve occhiata al canvas 3D. Il contenuto canvas 3D è specificato utilizzando l'[API WebGL](/it/docs/Web/API/WebGL_API), che è un'API completamente separata dall'API canvas 2D, anche se entrambi eseguono il rendering sugli elementi {{htmlelement("canvas")}}.

WebGL si basa su {{Glossary("OpenGL", "OpenGL")}} (Open Graphics Library), e ti permette di comunicare direttamente con il {{Glossary("GPU", "GPU")}} del computer. Come tale, scrivere WebGL grezzo è più vicino ai linguaggi a basso livello come C++ rispetto al normale JavaScript; è piuttosto complesso ma incredibilmente potente.

### Utilizzo di una libreria

A causa della sua complessità, la maggior parte delle persone scrive il codice grafico 3D utilizzando una libreria JavaScript di terze parti come [Three.js](/it/docs/Games/Techniques/3D_on_the_web/Building_up_a_basic_demo_with_Three.js), [PlayCanvas](/it/docs/Games/Techniques/3D_on_the_web/Building_up_a_basic_demo_with_PlayCanvas), o [Babylon.js](/it/docs/Games/Techniques/3D_on_the_web/Building_up_a_basic_demo_with_Babylon.js). La maggior parte di queste funziona in modo simile, fornendo funzionalità per creare forme primitive e personalizzate, posizionare telecamere e luci di visualizzazione, coprire superfici con texture e altro ancora. Gestiscono il WebGL per te, permettendoti di lavorare a un livello più alto.

Sì, utilizzare una di queste significa imparare un'altra nuova API (in questo caso, una di terze parti), ma sono molto più semplici che codificare in WebGL grezzo.

### Ricreare il nostro cubo

Vediamo un esempio di come creare qualcosa con una libreria WebGL. Sceglieremo [Three.js](/it/docs/Games/Techniques/3D_on_the_web/Building_up_a_basic_demo_with_Three.js), poiché è una delle più popolari. In questo tutorial creeremo il cubo rotante 3D che abbiamo visto prima.

1. Per iniziare, fai una copia locale di [threejs-cube/index.html](https://github.com/mdn/learning-area/blob/main/javascript/apis/drawing-graphics/threejs-cube/index.html) in una nuova cartella, poi salva una copia di [metal003.png](https://github.com/mdn/learning-area/blob/main/javascript/apis/drawing-graphics/threejs-cube/metal003.png) nella stessa cartella. Questa è l'immagine che useremo come texture di superficie per il cubo più avanti.
2. Successivamente, crea un nuovo file chiamato `script.js`, di nuovo nella stessa cartella di prima.
3. Successivamente, devi avere la libreria Three.js installata. Puoi seguire i passaggi di configurazione dell'ambiente descritti in [Costruire un demo di base con Three.js](/it/docs/Games/Techniques/3D_on_the_web/Building_up_a_basic_demo_with_Three.js) in modo da avere Three.js funzionante come previsto.
4. Ora abbiamo `three.js` allegata alla nostra pagina, possiamo iniziare a scrivere JavaScript che la utilizza in `script.js`. Iniziamo creando una nuova scena — aggiungi il seguente nel tuo file `script.js`:

   ```js
   const scene = new THREE.Scene();
   ```

   Il costruttore [`Scene()`](https://threejs.org/docs/index.html#api/en/scenes/Scene) crea una nuova scena, che rappresenta l'intero mondo 3D che stiamo cercando di visualizzare.

5. Successivamente, abbiamo bisogno di una **camera** per poter vedere la scena. In termini di immagini 3D, la telecamera rappresenta la posizione del visualizzatore nel mondo. Per creare una telecamera, aggiungi le seguenti righe successivamente:

   ```js
   const camera = new THREE.PerspectiveCamera(
     75,
     window.innerWidth / window.innerHeight,
     0.1,
     1000,
   );
   camera.position.z = 5;
   ```

   Il costruttore [`PerspectiveCamera()`](https://threejs.org/docs/index.html#api/en/cameras/PerspectiveCamera) prende quattro argomenti:

   - Il campo visivo: Quanto ampio l'area davanti alla telecamera dovrebbe essere visibile sullo schermo, in gradi.
   - Il {{Glossary("aspect_ratio", "rapporto d'aspetto")}}: Di solito, questo è il rapporto della larghezza della scena diviso per l'altezza della scena. Usando un altro valore si distorce la scena (cosa che potrebbe essere ciò che vuoi, ma di solito non lo è).
   - Il piano vicino: Quanto vicino alla telecamera possono essere gli oggetti prima che smettiamo di renderizzarli sullo schermo. Pensa a come quando avvicini il tuo polpastrello sempre più vicino allo spazio tra i tuoi occhi, alla fine non puoi più vederlo.
   - Il piano lontano: Quanto distanti sono le cose dalla telecamera prima che non vengano più renderizzate.

   Impostiamo anche la posizione della camera di 5 unità di distanza fuori dall'asse Z, che, come in CSS, è fuori dallo schermo verso di te, il visualizzatore.

6. Il terzo ingrediente vitale è un renderer. Questo è un oggetto che renderizza una data scena, come vista attraverso una data telecamera. Ne creeremo uno per ora utilizzando il costruttore [`WebGLRenderer()`](https://threejs.org/docs/index.html#api/en/renderers/WebGLRenderer), ma non lo useremo ancora. Aggiungi le seguenti righe successivamente:

   ```js
   const renderer = new THREE.WebGLRenderer();
   renderer.setSize(window.innerWidth, window.innerHeight);
   document.body.appendChild(renderer.domElement);
   ```

   La prima riga crea un nuovo renderer, la seconda riga imposta le dimensioni alle quali il renderer disegnerà la vista della camera, e la terza riga aggiunge l'elemento {{htmlelement("canvas")}} creato dal renderer al {{htmlelement("body")}} del documento. Ora qualsiasi cosa il renderer disegna verrà visualizzata nella nostra finestra.

7. Successivamente, vogliamo creare il cubo che visualizzeremo sul canvas. Aggiungi il seguente pezzo di codice in fondo al tuo JavaScript:

   ```js
   let cube;

   const loader = new THREE.TextureLoader();

   loader.load("metal003.png", (texture) => {
     texture.wrapS = THREE.RepeatWrapping;
     texture.wrapT = THREE.RepeatWrapping;
     texture.repeat.set(2, 2);

     const geometry = new THREE.BoxGeometry(2.4, 2.4, 2.4);
     const material = new THREE.MeshLambertMaterial({ map: texture });
     cube = new THREE.Mesh(geometry, material);
     scene.add(cube);

     draw();
   });
   ```

   C'è un po' di più da assorbire qui, quindi esaminiamolo in fasi:

   - In primo luogo, creiamo una variabile globale `cube` in modo tale che possiamo accedere al nostro cubo da qualsiasi parte del codice.
   - Successivamente, creiamo un nuovo oggetto [`TextureLoader`](https://threejs.org/docs/index.html#api/en/loaders/TextureLoader), poi chiamiamo `load()` su di esso. `load()` prende due parametri in questo caso (anche se può prenderne di più): la texture che vogliamo caricare (il nostro PNG), e una funzione che verrà eseguita quando la texture è stata caricata.
   - All'interno di questa funzione usiamo le proprietà dell'oggetto [`texture`](https://threejs.org/docs/index.html#api/en/textures/Texture) per specificare che vogliamo una ripetizione 2 x 2 dell'immagine intorno a tutti i lati del cubo. Successivamente, creiamo un nuovo oggetto [`BoxGeometry`](https://threejs.org/docs/index.html#api/en/geometries/BoxGeometry) e un nuovo oggetto [`MeshLambertMaterial`](https://threejs.org/docs/index.html#api/en/materials/MeshLambertMaterial), e li uniamo in un [`Mesh`](https://threejs.org/docs/index.html#api/en/objects/Mesh) per creare il nostro cubo. Un oggetto richiede tipicamente una geometria (di quale forma è) e un materiale (che aspetto ha la sua superficie).
   - Infine, aggiungiamo il nostro cubo alla scena, poi chiamiamo la nostra funzione `draw()` per avviare l'animazione.

8. Prima di arrivare a definire `draw()`, aggiungeremo un paio di luci alla scena, per vivacizzare un po' le cose; aggiungi i seguenti blocchi successivamente:

   ```js
   const light = new THREE.AmbientLight("rgb(255 255 255)"); // soft white light
   scene.add(light);

   const spotLight = new THREE.SpotLight("rgb(255 255 255)");
   spotLight.position.set(100, 1000, 1000);
   spotLight.castShadow = true;
   scene.add(spotLight);
   ```

   Un oggetto [`AmbientLight`](https://threejs.org/docs/index.html#api/en/lights/AmbientLight) è un tipo di luce morbida che illumina un po' tutta la scena, come il sole quando si è all'aperto. L'oggetto [`SpotLight`](https://threejs.org/docs/index.html#api/en/lights/SpotLight), d'altra parte, è un fascio di luce direzionale, più simile a una torcia elettrica (o un proiettore, in effetti).

9. Infine, aggiungiamo la nostra funzione `draw()` in fondo al codice:

   ```js
   function draw() {
     cube.rotation.x += 0.01;
     cube.rotation.y += 0.01;
     renderer.render(scene, camera);

     requestAnimationFrame(draw);
   }
   ```

   Questo è abbastanza intuitivo; su ogni frame, giriamo leggermente il nostro cubo sui suoi assi X e Y, poi eseguiamo il rendering della scena come visualizzata dalla nostra telecamera, poi infine chiamiamo `requestAnimationFrame()` per programmare il disegno del nostro prossimo frame.

Diamo ancora un'occhiata a come dovrebbe apparire il prodotto finito:

{{EmbedGHLiveSample("learning-area/javascript/apis/drawing-graphics/threejs-cube/index.html", '100%', 500)}}

Puoi [trovare il codice finito su GitHub](https://github.com/mdn/learning-area/tree/main/javascript/apis/drawing-graphics/threejs-cube).

> [!NOTE]
> Nel nostro repo GitHub puoi anche trovare un altro interessante esempio di cubo 3D — [Three.js Video Cube](https://github.com/mdn/learning-area/tree/main/javascript/apis/drawing-graphics/threejs-video-cube) (vedi anche [dal vivo](https://mdn.github.io/learning-area/javascript/apis/drawing-graphics/threejs-video-cube/)). Questo usa [`getUserMedia()`](/it/docs/Web/API/MediaDevices/getUserMedia) per prendere un flusso video dalla webcam di un computer e proiettarlo su un lato del cubo come texture!

## Sommario

A questo punto, dovresti avere un'idea utile dei fondamenti della programmazione grafica usando Canvas e WebGL e cosa puoi fare con queste API, oltre a una buona idea di dove andare per ulteriori informazioni. Divertiti!

## Vedi anche

Qui abbiamo trattato solo le basi reali del canvas — c'è molto altro da imparare! Gli articoli sottostanti ti porteranno ulteriormente.

- [Canvas tutorial](/it/docs/Web/API/Canvas_API/Tutorial) — Una serie di tutorial molto dettagliata che spiega ciò che dovresti sapere sul canvas 2D in molto più dettaglio rispetto a quanto trattato qui. Lettura essenziale.
- [WebGL tutorial](/it/docs/Web/API/WebGL_API/Tutorial) — Una serie che insegna le basi della programmazione WebGL grezza.
- [Costruire un demo di base con Three.js](/it/docs/Games/Techniques/3D_on_the_web/Building_up_a_basic_demo_with_Three.js) — Tutorial base di Three.js. Abbiamo anche guide equivalenti per [PlayCanvas](/it/docs/Games/Techniques/3D_on_the_web/Building_up_a_basic_demo_with_PlayCanvas) o [Babylon.js](/it/docs/Games/Techniques/3D_on_the_web/Building_up_a_basic_demo_with_Babylon.js).
- [Sviluppo di giochi](/it/docs/Games) — La pagina di destinazione per lo sviluppo di giochi web su MDN. Ci sono alcuni tutorial e tecniche davvero utili disponibili qui legati al canvas 2D e 3D — vedi le opzioni del menu Techniques and Tutorials.

## Esempi

- [Violent theremin](https://github.com/mdn/webaudio-examples/tree/main/violent-theremin) — Utilizza l'API Web Audio per generare suoni e il canvas per generare una bella visualizzazione da accompagnarlo.
- [Voice change-o-matic](https://github.com/mdn/webaudio-examples/tree/main/voice-change-o-matic) — Utilizza un canvas per visualizzare dati audio in tempo reale dall'API Web Audio.

{{PreviousMenuNext("Learn_web_development/Extensions/Client-side_APIs/Video_and_audio_APIs", "Learn_web_development/Extensions/Client-side_APIs/Client-side_storage", "Learn_web_development/Extensions/Client-side_APIs")}}
