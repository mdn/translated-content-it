---
title: Disegnare grafica
slug: Learn_web_development/Extensions/Client-side_APIs/Drawing_graphics
l10n:
  sourceCommit: f4c14731a1a157fc8d8f7357ac4d74d14a7d7fb5
---

{{PreviousMenuNext("Learn_web_development/Extensions/Client-side_APIs/Video_and_audio_APIs", "Learn_web_development/Extensions/Client-side_APIs/Client-side_storage", "Learn_web_development/Extensions/Client-side_APIs")}}

Il browser contiene strumenti molto potenti per la programmazione grafica, dal linguaggio Scalable Vector Graphics ([SVG](/it/docs/Web/SVG)) alle API per disegnare sugli elementi HTML {{htmlelement("canvas")}} (vedere [The Canvas API](/it/docs/Web/API/Canvas_API) e [WebGL](/it/docs/Web/API/WebGL_API)). Questo articolo fornisce un'introduzione a canvas e ulteriori risorse per approfondire l'argomento.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, in particolare con le <a href="/it/docs/Learn_web_development/Core/Scripting/Object_basics">basi degli oggetti JavaScript</a> e con API fondamentali quali il <a href="/it/docs/Learn_web_development/Core/Scripting/DOM_scripting">scripting del DOM</a> e le <a href="/it/docs/Learn_web_development/Core/Scripting/Network_requests">richieste di rete</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>I concetti e i casi d'uso resi possibili dalle API trattate in questa lezione.</li>
          <li>Sintassi e utilizzo di base di <code>&lt;canvas&gt;</code> e delle API associate.</li>
          <li>Utilizzo di timer e <code>requestAnimationFrame()</code> per impostare cicli di animazione.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Grafica sul Web

In origine il Web era costituito solo da testo, il che era piuttosto noioso; furono quindi introdotte le immagini, prima tramite l'elemento {{htmlelement("img")}} e successivamente tramite proprietà CSS come {{cssxref("background-image")}} e [SVG](/it/docs/Web/SVG).

Tuttavia, questo non era ancora sufficiente. Sebbene fosse possibile usare [CSS](/it/docs/Learn_web_development/Core/Styling_basics) e [JavaScript](/it/docs/Learn_web_development/Core/Scripting) per animare (e altrimenti manipolare) immagini vettoriali SVG, poiché sono rappresentate da markup, non esisteva ancora alcun modo per fare lo stesso con immagini bitmap e gli strumenti disponibili erano piuttosto limitati. Il Web non aveva ancora un modo per creare efficacemente animazioni, giochi, scene 3D e altri requisiti comunemente gestiti da linguaggi di livello più basso quali C++ o Java.

La situazione iniziò a migliorare quando i browser cominciarono a supportare l'elemento {{htmlelement("canvas")}} e la relativa [Canvas API](/it/docs/Web/API/Canvas_API) nel 2004. Come si vedrà di seguito, canvas fornisce alcuni strumenti utili per creare animazioni 2D, giochi, visualizzazioni di dati e altri tipi di applicazioni, soprattutto se combinato con alcune delle altre API fornite dalla piattaforma web, ma può essere difficile o impossibile renderlo accessibile.

L'esempio seguente mostra una semplice animazione di palline rimbalzanti 2D basata su canvas, incontrata originariamente nel modulo [Introduzione agli oggetti JavaScript](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_building_practice):

```html hidden live-sample___bouncing-balls
<h1>bouncing balls</h1>
<canvas></canvas>
```

```css hidden live-sample___bouncing-balls
html,
body {
  margin: 0;
}

html {
  font-family: "Helvetica Neue", "Helvetica", "Arial", sans-serif;
  height: 100%;
}

body {
  overflow: hidden;
  height: inherit;
}

h1 {
  font-size: 2rem;
  letter-spacing: -1px;
  position: absolute;
  margin: 0;
  top: -4px;
  right: 5px;

  color: transparent;
  text-shadow: 0 0 4px white;
}
```

```js hidden live-sample___bouncing-balls
// set up canvas

const canvas = document.querySelector("canvas");
const ctx = canvas.getContext("2d");

const width = (canvas.width = window.innerWidth);
const height = (canvas.height = window.innerHeight);

// function to generate random number

function random(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min;
}

// function to generate random RGB color value

function randomRGB() {
  return `rgb(${random(0, 255)} ${random(0, 255)} ${random(0, 255)})`;
}

const balls = [];

class Ball {
  constructor(x, y, velX, velY, color, size) {
    this.x = x;
    this.y = y;
    this.velX = velX;
    this.velY = velY;
    this.color = color;
    this.size = size;
  }

  draw() {
    ctx.beginPath();
    ctx.fillStyle = this.color;
    ctx.arc(this.x, this.y, this.size, 0, 2 * Math.PI);
    ctx.fill();
  }

  update() {
    if (this.x + this.size >= width) {
      this.velX = -Math.abs(this.velX);
    }

    if (this.x - this.size <= 0) {
      this.velX = Math.abs(this.velX);
    }

    if (this.y + this.size >= height) {
      this.velY = -Math.abs(this.velY);
    }

    if (this.y - this.size <= 0) {
      this.velY = Math.abs(this.velY);
    }

    this.x += this.velX;
    this.y += this.velY;
  }

  collisionDetect() {
    for (const ball of balls) {
      if (!(this === ball)) {
        const dx = this.x - ball.x;
        const dy = this.y - ball.y;
        const distance = Math.sqrt(dx * dx + dy * dy);

        if (distance < this.size + ball.size) {
          ball.color = this.color = randomRGB();
        }
      }
    }
  }
}

while (balls.length < 25) {
  const size = random(10, 20);
  const ball = new Ball(
    // ball position always drawn at least one ball width
    // away from the edge of the canvas, to avoid drawing errors
    random(0 + size, width - size),
    random(0 + size, height - size),
    random(-7, 7),
    random(-7, 7),
    randomRGB(),
    size,
  );

  balls.push(ball);
}

function loop() {
  ctx.fillStyle = "rgba(0, 0, 0, 0.25)";
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

{{EmbedLiveSample("bouncing-balls", '100%', 500)}}

Tra il 2006 e il 2007, Mozilla iniziò a lavorare su un'implementazione sperimentale di canvas 3D. Questa divenne [WebGL](/it/docs/Web/API/WebGL_API), che ottenne consenso tra i fornitori di browser e fu standardizzata intorno al 2009–2010. WebGL consente di creare vera grafica 3D nel browser web.

Questo articolo si concentrerà principalmente sul canvas 2D, poiché il codice WebGL puro è molto complesso. Verrà tuttavia mostrato come usare [una libreria WebGL per creare più facilmente una scena 3D](#webgl); è inoltre possibile trovare altrove un tutorial sul WebGL puro — vedere [Introduzione a WebGL](/it/docs/Web/API/WebGL_API/Tutorial/Getting_started_with_WebGL).

## Introduzione a `<canvas>`

Per creare una scena _2D_ o _3D_ in una pagina web, è necessario iniziare con un elemento HTML {{htmlelement("canvas")}}. Questo elemento viene utilizzato per definire l'area della pagina nella quale verrà disegnata l'immagine. È sufficiente includere l'elemento nella pagina:

```html
<canvas width="320" height="240"></canvas>
```

Questo creerà un canvas nella pagina di 320 per 240 pixel.

All'interno dei tag `<canvas>` dovrebbe essere inserito del contenuto di fallback. Questo dovrebbe descrivere il contenuto del canvas agli utenti di browser che non supportano canvas o agli utenti di lettori di schermo.

```html
<canvas width="320" height="240">
  <p>Description of the canvas for those unable to view it.</p>
</canvas>
```

Il fallback dovrebbe fornire contenuti alternativi utili rispetto al contenuto del canvas. Ad esempio, se si esegue il rendering di un grafico dei prezzi azionari in costante aggiornamento, il contenuto di fallback potrebbe essere un'immagine statica dell'ultimo grafico azionario, con testo `alt` che indichi i prezzi in forma testuale oppure un elenco di link alle singole pagine azionarie.

> [!NOTE]
> Il contenuto canvas non è accessibile ai lettori di schermo. Includere testo descrittivo come valore dell'attributo [`aria-label`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-label) direttamente sull'elemento canvas oppure includere contenuto di fallback inserito tra i tag di apertura e chiusura `<canvas>`. Il contenuto canvas non fa parte del DOM, mentre il contenuto di fallback annidato sì.

### Creare e dimensionare il canvas

Iniziamo creando un template canvas personale per realizzare esperimenti futuri.

1. Innanzitutto, creare una directory sul disco rigido locale denominata `canvas-template`.
2. Creare nella directory un nuovo file denominato `index.html` e salvarvi il seguente contenuto:

   ```html
   <!doctype html>
   <html lang="en-US">
     <head>
       <meta charset="utf-8" />
       <meta name="viewport" content="width=device-width" />
       <title>Canvas</title>
       <script src="script.js" defer></script>
       <link href="style.css" rel="stylesheet" />
     </head>
     <body>
       <canvas class="myCanvas">
         <p>Add suitable fallback here.</p>
       </canvas>
     </body>
   </html>
   ```

   ```html hidden live-sample___2-canvas-rectangles live-sample___3_canvas_paths live-sample___4-canvas-text live-sample___5-canvas-images live-sample___6-canvas-for-loop
   <canvas class="myCanvas">
     <p>Add suitable fallback here.</p>
   </canvas>
   ```

3. Creare nella directory un nuovo file denominato `style.css` e salvarvi la seguente regola CSS:

   ```css live-sample___2-canvas-rectangles live-sample___3_canvas_paths live-sample___4-canvas-text live-sample___5-canvas-images live-sample___6-canvas-for-loop live-sample___7-canvas-walking-animation
   body {
     margin: 0;
     overflow: hidden;
   }
   ```

4. Creare nella directory un nuovo file denominato `script.js`. Per il momento lasciare vuoto questo file.

5. Ora aprire `script.js` e aggiungere le seguenti righe JavaScript:

   ```js live-sample___2-canvas-rectangles live-sample___3_canvas_paths live-sample___4-canvas-text live-sample___5-canvas-images live-sample___6-canvas-for-loop live-sample___7-canvas-walking-animation
   const canvas = document.querySelector(".myCanvas");
   const width = (canvas.width = window.innerWidth);
   const height = (canvas.height = window.innerHeight);
   ```

   Qui è stato memorizzato un riferimento al canvas nella costante `canvas`. Nella seconda riga vengono impostate sia una nuova costante `width` sia la proprietà `width` del canvas uguali a [`Window.innerWidth`](/it/docs/Web/API/Window/innerWidth) (che fornisce la larghezza del viewport). Nella terza riga vengono impostate sia una nuova costante `height` sia la proprietà `height` del canvas uguali a [`Window.innerHeight`](/it/docs/Web/API/Window/innerHeight) (che fornisce l'altezza del viewport). Ora è disponibile un canvas che riempie l'intera larghezza e altezza della finestra del browser.

   Si noterà inoltre che vengono concatenate le assegnazioni con più segni di uguale: questo è consentito in JavaScript ed è una buona tecnica quando si vogliono rendere più variabili uguali allo stesso valore. Si desiderava rendere facilmente accessibili la larghezza e l'altezza del canvas nelle variabili width/height, poiché sono valori utili da avere disponibili in seguito, ad esempio per disegnare qualcosa esattamente a metà della larghezza del canvas.

> [!NOTE]
> In generale, la dimensione del canvas dovrebbe essere impostata usando attributi HTML o proprietà DOM, come spiegato sopra. Si potrebbe usare CSS, ma il problema è che il dimensionamento viene eseguito dopo il rendering del canvas e, proprio come qualsiasi altra immagine, il canvas potrebbe risultare pixelato o distorto.

### Ottenere il contesto canvas e la configurazione finale

Prima di poter considerare concluso il template canvas, è necessario fare un'ultima cosa. Per disegnare sul canvas occorre ottenere un riferimento speciale all'area di disegno, chiamato contesto. Ciò viene eseguito usando il metodo [`HTMLCanvasElement.getContext()`](/it/docs/Web/API/HTMLCanvasElement/getContext), che per l'utilizzo di base accetta come parametro una singola stringa che rappresenta il tipo di contesto da recuperare.

In questo caso si desidera un canvas 2D, quindi aggiungere la seguente riga JavaScript sotto le altre in `script.js`:

```js live-sample___2-canvas-rectangles live-sample___3_canvas_paths live-sample___4-canvas-text live-sample___5-canvas-images live-sample___6-canvas-for-loop live-sample___7-canvas-walking-animation
const ctx = canvas.getContext("2d");
```

> [!NOTE]
> Altri valori di contesto selezionabili includono `webgl` per WebGL, `webgpu` per WebGPU e così via, ma non saranno necessari in questo articolo.

Ecco fatto: il canvas è ora predisposto e pronto per disegnare. La variabile `ctx` contiene ora un oggetto [`CanvasRenderingContext2D`](/it/docs/Web/API/CanvasRenderingContext2D) e tutte le operazioni di disegno sul canvas comporteranno la manipolazione di questo oggetto.

Facciamo un'ultima cosa prima di proseguire. Coloreremo di nero lo sfondo del canvas per un primo assaggio della canvas API. Aggiungere le seguenti righe in fondo al JavaScript:

```js live-sample___2-canvas-rectangles live-sample___3_canvas_paths live-sample___4-canvas-text live-sample___5-canvas-images live-sample___6-canvas-for-loop
ctx.fillStyle = "black";
ctx.fillRect(0, 0, width, height);
```

Qui viene impostato un colore di riempimento usando la proprietà [`fillStyle`](/it/docs/Web/API/CanvasRenderingContext2D/fillStyle) del canvas (che accetta [valori di colore](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#color) proprio come le proprietà CSS), quindi viene disegnato un rettangolo che copre l'intera area del canvas con il metodo [`fillRect`](/it/docs/Web/API/CanvasRenderingContext2D/fillRect). I primi due parametri sono le coordinate dell'angolo superiore sinistro del rettangolo; gli ultimi due sono la larghezza e l'altezza con cui si desidera disegnare il rettangolo — ecco perché le variabili `width` e `height` sono utili.

Il template è completato ed è ora possibile proseguire.

## Fondamenti del canvas 2D

Come già detto, tutte le operazioni di disegno vengono eseguite manipolando un oggetto [`CanvasRenderingContext2D`](/it/docs/Web/API/CanvasRenderingContext2D) (in questo caso, `ctx`). Molte operazioni richiedono coordinate per individuare esattamente dove disegnare qualcosa: l'angolo superiore sinistro del canvas è il punto (0, 0), l'asse orizzontale (x) va da sinistra a destra e l'asse verticale (y) va dall'alto verso il basso.

![Carta millimetrata con piccoli quadrati che ne coprono l'area e un quadrato steelblue al centro. L'angolo superiore sinistro del canvas è il punto (0, 0) dell'asse x e dell'asse y del canvas. L'asse orizzontale (x) va da sinistra a destra e indica la larghezza, mentre l'asse verticale (y) va dall'alto verso il basso e indica l'altezza. L'angolo superiore sinistro del quadrato blu è etichettato come a una distanza di x unità dall'asse y e y unità dall'asse x.](canvas_default_grid.png)

Il disegno delle forme viene solitamente eseguito usando la primitiva della forma rettangolare oppure tracciando una linea lungo un determinato percorso e quindi riempiendo la forma. Di seguito verrà mostrato come fare entrambe le cose.

### Rettangoli semplici

Iniziamo con alcuni rettangoli semplici.

1. Prima di tutto, creare una copia della directory del template canvas appena realizzato.
2. Aggiungere le seguenti righe alla fine del file JavaScript:

   ```js live-sample___2-canvas-rectangles
   ctx.fillStyle = "red";
   ctx.fillRect(50, 50, 100, 150);
   ```

   Caricando l'HTML nel browser, dovrebbe apparire un rettangolo rosso sul canvas. Il suo angolo superiore sinistro dista 50 pixel dal bordo superiore e sinistro del canvas, come definito dai primi due parametri, ed è largo 100 pixel e alto 150 pixel, come definito dal terzo e quarto parametro.

3. Aggiungiamo un altro rettangolo: questa volta verde. Aggiungere quanto segue in fondo al JavaScript:

   ```js live-sample___2-canvas-rectangles
   ctx.fillStyle = "green";
   ctx.fillRect(75, 75, 100, 100);
   ```

   Salvare e aggiornare la pagina: sarà visibile il nuovo rettangolo. Questo solleva un punto importante: le operazioni grafiche, come il disegno di rettangoli, linee e così via, vengono eseguite nell'ordine in cui si verificano. È come dipingere una parete, dove ogni mano di pittura si sovrappone e può perfino nascondere ciò che si trova sotto. Non è possibile cambiare questo comportamento, quindi occorre pensare attentamente all'ordine in cui disegnare la grafica.

4. Si noti che è possibile disegnare grafica semitrasparente specificando un colore semitrasparente, ad esempio usando `rgb()`. Il "canale alfa" definisce il livello di trasparenza del colore. Più alto è il suo valore, più il colore nasconderà ciò che si trova dietro. Aggiungere quanto segue al codice:

   ```js live-sample___2-canvas-rectangles
   ctx.fillStyle = "rgb(255 0 255 / 75%)";
   ctx.fillRect(25, 100, 175, 50);
   ```

5. Provare ora a disegnare altri rettangoli personali; buon divertimento.

### Tratti e larghezze delle linee

Finora sono stati disegnati rettangoli pieni, ma è anche possibile disegnare rettangoli costituiti solo da contorni, chiamati **tratti** nella progettazione grafica. Per impostare il colore desiderato per il tratto, usare la proprietà [`strokeStyle`](/it/docs/Web/API/CanvasRenderingContext2D/strokeStyle); il disegno di un rettangolo con tratto si esegue usando [`strokeRect`](/it/docs/Web/API/CanvasRenderingContext2D/strokeRect).

1. Aggiungere quanto segue all'esempio precedente, sempre sotto le righe JavaScript precedenti:

   ```js
   ctx.strokeStyle = "white";
   ctx.strokeRect(25, 25, 175, 200);
   ```

2. La larghezza predefinita dei tratti è di 1 pixel; è possibile regolare il valore della proprietà [`lineWidth`](/it/docs/Web/API/CanvasRenderingContext2D/lineWidth) per modificarla. Questa accetta un numero che rappresenta il numero di pixel di larghezza del tratto. Aggiungere la seguente riga tra le due righe precedenti:

   ```js
   ctx.lineWidth = 5;
   ```

Ora il contorno bianco dovrebbe essere molto più spesso. Per ora è tutto. A questo punto l'esempio dovrebbe apparire così:

```js hidden live-sample___2-canvas-rectangles
ctx.strokeStyle = "white";
ctx.lineWidth = 5;
ctx.strokeRect(25, 25, 175, 200);
```

{{EmbedLiveSample("2-canvas-rectangles", '100%', 250)}}

È possibile premere il pulsante **Play** per aprire l'esempio in MDN Playground e modificare il codice sorgente.

### Disegnare percorsi

Per disegnare qualcosa di più complesso di un rettangolo, è necessario disegnare un percorso. In sostanza, ciò comporta la scrittura di codice per specificare esattamente quale percorso la penna dovrebbe seguire sul canvas per tracciare la forma desiderata. Canvas include funzioni per disegnare linee rette, cerchi, curve di Bézier e altro ancora.

Iniziare questa sezione creando una nuova copia del template canvas nella quale disegnare il nuovo esempio.

Verranno usati alcuni metodi e proprietà comuni in tutte le sezioni seguenti:

- [`beginPath()`](/it/docs/Web/API/CanvasRenderingContext2D/beginPath) — inizia a disegnare un percorso nel punto in cui si trova attualmente la penna sul canvas. In un nuovo canvas, la penna parte da (0, 0).
- [`moveTo()`](/it/docs/Web/API/CanvasRenderingContext2D/moveTo) — sposta la penna in un punto diverso del canvas, senza registrare o tracciare la linea; la penna "salta" nella nuova posizione.
- [`fill()`](/it/docs/Web/API/CanvasRenderingContext2D/fill) — disegna una forma piena riempiendo il percorso tracciato finora.
- [`stroke()`](/it/docs/Web/API/CanvasRenderingContext2D/stroke) — disegna una forma di contorno tracciando un tratto lungo il percorso disegnato finora.
- È inoltre possibile usare funzionalità come `lineWidth` e `fillStyle`/`strokeStyle` con i percorsi oltre che con i rettangoli.

Una tipica e semplice operazione di disegno di un percorso avrebbe un aspetto simile a questo:

```js
ctx.fillStyle = "red";
ctx.beginPath();
ctx.moveTo(50, 50);
// draw your path
ctx.fill();
```

#### Disegnare linee

Disegniamo un triangolo equilatero sul canvas.

1. Prima di tutto, aggiungere la seguente funzione helper in fondo al codice. Questa converte i valori in gradi in radianti, il che è utile perché ogni volta che è necessario fornire un valore angolare in JavaScript, questo sarà quasi sempre in radianti, mentre gli esseri umani pensano solitamente in gradi.

   ```js live-sample___3_canvas_paths
   function degToRad(degrees) {
     return (degrees * Math.PI) / 180;
   }
   ```

2. Successivamente, iniziare il percorso aggiungendo quanto segue sotto l'aggiunta precedente; qui viene impostato un colore per il triangolo, si inizia a disegnare un percorso e poi si sposta la penna a (50, 50) senza disegnare nulla. Da lì inizierà il disegno del triangolo.

   ```js live-sample___3_canvas_paths
   ctx.fillStyle = "red";
   ctx.beginPath();
   ctx.moveTo(50, 50);
   ```

3. Ora aggiungere le seguenti righe in fondo allo script:

   ```js live-sample___3_canvas_paths
   ctx.lineTo(150, 50);
   const triHeight = 50 * Math.tan(degToRad(60));
   ctx.lineTo(100, 50 + triHeight);
   ctx.lineTo(50, 50);
   ctx.fill();
   ```

   Vediamo l'operazione nell'ordine:

   Innanzitutto viene disegnata una linea fino a (150, 50): il percorso ora va 100 pixel verso destra lungo l'asse x.

   In secondo luogo, viene calcolata l'altezza del triangolo equilatero, usando un po' di semplice trigonometria. In sostanza, viene disegnato il triangolo rivolto verso il basso. Gli angoli in un triangolo equilatero sono sempre di 60 gradi; per calcolarne l'altezza è possibile dividerlo a metà in due triangoli rettangoli, ciascuno dei quali avrà angoli di 90 gradi, 60 gradi e 30 gradi. Per quanto riguarda i lati:
   - Il lato più lungo è chiamato **ipotenusa**.
   - Il lato adiacente all'angolo di 60 gradi è chiamato **adiacente** — e si sa che è di 50 pixel, poiché corrisponde a metà della linea appena disegnata.
   - Il lato opposto all'angolo di 60 gradi è chiamato **opposto**, che è l'altezza del triangolo da calcolare.

   ![Un triangolo equilatero rivolto verso il basso con angoli e lati etichettati. La linea orizzontale in alto è etichettata "adiacente". Una linea perpendicolare tratteggiata, dal centro della linea adiacente, etichettata "opposto", divide il triangolo creando due triangoli rettangoli uguali. Il lato destro del triangolo è etichettato come ipotenusa, poiché è l'ipotenusa del triangolo rettangolo formato dalla linea etichettata "opposto". Sebbene tutti e tre i lati del triangolo abbiano la stessa lunghezza, l'ipotenusa è il lato più lungo del triangolo rettangolo.](trigonometry.png)

   Una delle formule trigonometriche di base afferma che la lunghezza del lato adiacente moltiplicata per la tangente dell'angolo è uguale al lato opposto; da qui `50 * Math.tan(degToRad(60))`. Si usa la funzione `degToRad()` per convertire 60 gradi in radianti, poiché {{jsxref("Math.tan()")}} si aspetta un valore di input in radianti.

4. Con l'altezza calcolata, viene disegnata un'altra linea fino a `(100, 50 + triHeight)`. La coordinata X è semplice: deve trovarsi a metà tra i due valori X precedenti impostati. Il valore Y invece deve essere 50 più l'altezza del triangolo, poiché si sa che la parte superiore del triangolo si trova a 50 pixel dalla parte superiore del canvas.
5. La riga successiva disegna una linea di ritorno al punto iniziale del triangolo.
6. Infine, viene eseguito `ctx.fill()` per terminare il percorso e riempire la forma.

#### Disegnare cerchi

Vediamo ora come disegnare un cerchio in canvas. Questo viene eseguito usando il metodo [`arc()`](/it/docs/Web/API/CanvasRenderingContext2D/arc), che disegna tutto o parte di un cerchio in un punto specificato.

1. Aggiungiamo un arco al canvas: aggiungere quanto segue in fondo al codice:

   ```js live-sample___3_canvas_paths
   ctx.fillStyle = "blue";
   ctx.beginPath();
   ctx.arc(150, 106, 50, degToRad(0), degToRad(360), false);
   ctx.fill();
   ```

   `arc()` accetta sei parametri. I primi due specificano la posizione del centro dell'arco, rispettivamente X e Y. Il terzo è il raggio del cerchio, il quarto e il quinto sono gli angoli iniziale e finale ai quali disegnare il cerchio, quindi specificando 0 e 360 gradi si ottiene un cerchio completo, mentre il sesto parametro definisce se il cerchio deve essere disegnato in senso antiorario o orario (`false` corrisponde al senso orario).

   > [!NOTE]
   > 0 gradi si trova orizzontalmente verso destra.

2. Proviamo ad aggiungere un altro arco:

   ```js live-sample___3_canvas_paths
   ctx.fillStyle = "yellow";
   ctx.beginPath();
   ctx.arc(200, 106, 50, degToRad(-45), degToRad(45), true);
   ctx.lineTo(200, 106);
   ctx.fill();
   ```

   Il modello è molto simile, ma con due differenze:
   - L'ultimo parametro di `arc()` è stato impostato su `true`, il che significa che l'arco viene disegnato in senso antiorario. Ciò significa che, anche se l'arco è specificato come iniziante a -45 gradi e terminante a 45 gradi, l'arco viene disegnato intorno ai 270 gradi, non all'interno di questa porzione. Se si modificasse `true` in `false` e si eseguisse nuovamente il codice, verrebbe disegnata solo la fetta di 90 gradi del cerchio.
   - Prima di chiamare `fill()`, viene disegnata una linea fino al centro del cerchio. Questo produce il piacevole ritaglio in stile Pac-Man. Se si rimuovesse questa riga, provando a farlo, e si eseguisse nuovamente il codice, si otterrebbe solo un bordo del cerchio tagliato tra il punto iniziale e quello finale dell'arco. Questo illustra un altro punto importante del canvas: se si tenta di riempire un percorso incompleto, ossia non chiuso, il browser riempie una linea retta tra il punto iniziale e quello finale e quindi esegue il riempimento.

Per ora è tutto; l'esempio finale dovrebbe apparire così:

{{EmbedLiveSample("3_canvas_paths", '100%', 200)}}

È possibile premere il pulsante **Play** per aprire l'esempio in MDN Playground e modificare il codice sorgente.

> [!NOTE]
> Per scoprire di più sulle funzionalità avanzate di disegno dei percorsi, come le curve di Bézier, consultare il tutorial [Disegnare forme con canvas](/it/docs/Web/API/Canvas_API/Tutorial/Drawing_shapes).

### Testo

Canvas dispone anche di funzionalità per disegnare testo. Esploriamole brevemente. Iniziare creando un'altra nuova copia del template canvas nella quale disegnare il nuovo esempio.

Il testo viene disegnato usando due metodi:

- [`fillText()`](/it/docs/Web/API/CanvasRenderingContext2D/fillText) — disegna testo pieno.
- [`strokeText()`](/it/docs/Web/API/CanvasRenderingContext2D/strokeText) — disegna testo con contorno, o tratto.

Entrambi accettano tre proprietà nel loro utilizzo di base: la stringa di testo da disegnare e le coordinate X e Y del punto dal quale iniziare a disegnare il testo. Questo corrisponde all'angolo **inferiore sinistro** della **casella di testo**, letteralmente la casella che circonda il testo disegnato, il che potrebbe confondere poiché altre operazioni di disegno tendono a iniziare dall'angolo superiore sinistro. Tenerlo presente.

Esistono inoltre diverse proprietà che aiutano a controllare il rendering del testo, come [`font`](/it/docs/Web/API/CanvasRenderingContext2D/font), che consente di specificare famiglia di caratteri, dimensione e così via. Il suo valore usa la stessa sintassi della proprietà CSS {{cssxref("font")}}.

Il contenuto canvas non è accessibile ai lettori di schermo. Il testo dipinto sul canvas non è disponibile nel DOM, ma deve essere reso disponibile affinché sia accessibile. In questo esempio, il testo è incluso come valore di `aria-label`.

Provare ad aggiungere il seguente blocco in fondo al JavaScript:

```js live-sample___4-canvas-text
ctx.strokeStyle = "white";
ctx.lineWidth = 1;
ctx.font = "36px arial";
ctx.strokeText("Canvas text", 50, 50);

ctx.fillStyle = "red";
ctx.font = "48px georgia";
ctx.fillText("Canvas text", 50, 150);

canvas.setAttribute("aria-label", "Canvas text");
```

Qui vengono disegnate due righe di testo, una con contorno e l'altra con tratto. L'esempio dovrebbe apparire così:

{{EmbedLiveSample("4-canvas-text", '100%', 180)}}

Premere il pulsante **Play** per aprire l'esempio in MDN Playground e modificare il codice sorgente. Provare a sperimentare per vedere cosa si riesce a creare. Maggiori informazioni sulle opzioni disponibili per il testo canvas sono disponibili in [Disegnare testo](/it/docs/Web/API/Canvas_API/Tutorial/Drawing_text).

### Disegnare immagini sul canvas

È possibile eseguire il rendering di immagini esterne sul canvas. Possono essere immagini semplici, fotogrammi di video o il contenuto di altri canvas. Per il momento verrà esaminato solo il caso dell'uso di alcune immagini semplici sul canvas.

1. Come in precedenza, creare un'altra nuova copia del template canvas nella quale disegnare il nuovo esempio.

   Le immagini vengono disegnate sul canvas usando il metodo [`drawImage()`](/it/docs/Web/API/CanvasRenderingContext2D/drawImage). La versione più semplice accetta tre parametri: un riferimento all'immagine di cui eseguire il rendering e le coordinate X e Y dell'angolo superiore sinistro dell'immagine.

2. Iniziamo ottenendo una sorgente immagine da incorporare nel canvas. Aggiungere le seguenti righe in fondo al JavaScript:

   ```js live-sample___5-canvas-images
   const image = new Image();
   image.src =
     "https://mdn.github.io/shared-assets/images/examples/fx-nightly-512.png";
   ```

   Qui viene creato un nuovo oggetto [`HTMLImageElement`](/it/docs/Web/API/HTMLImageElement) usando il costruttore [`Image()`](/it/docs/Web/API/HTMLImageElement/Image). L'oggetto restituito è dello stesso tipo di quello restituito quando si ottiene un riferimento a un elemento {{htmlelement("img")}} esistente. Viene quindi impostato il suo attributo [`src`](/it/docs/Web/HTML/Reference/Elements/img#src) affinché sia uguale all'immagine del logo Firefox. A questo punto, il browser inizia a caricare l'immagine.

3. Ora si potrebbe provare a incorporare l'immagine usando `drawImage()`, ma è necessario assicurarsi che il file immagine sia stato caricato prima, altrimenti il codice non funzionerà. Questo può essere ottenuto usando l'evento `load`, che verrà attivato solo quando l'immagine avrà terminato il caricamento. Aggiungere il seguente blocco sotto quello precedente:

   ```js
   image.addEventListener("load", () => ctx.drawImage(image, 20, 20));
   ```

   Caricando ora l'esempio nel browser, dovrebbe essere visibile l'immagine incorporata nel canvas, sebbene piuttosto grande.

4. Ma c'è di più. E se si volesse visualizzare solo una parte dell'immagine o ridimensionarla? Entrambe le operazioni sono possibili con la versione più complessa di `drawImage()`. Aggiornare la riga `ctx.drawImage()` come segue:

   ```js
   ctx.drawImage(image, 0, 0, 512, 512, 50, 40, 185, 185);
   ```

   ```js hidden live-sample___5-canvas-images
   image.addEventListener("load", () =>
     ctx.drawImage(image, 0, 0, 512, 512, 50, 40, 185, 185),
   );
   ```

   - Il primo parametro è il riferimento all'immagine, come in precedenza.
   - I parametri 2 e 3 definiscono le coordinate dell'angolo superiore sinistro dell'area da ritagliare dall'immagine caricata, rispetto all'angolo superiore sinistro dell'immagine stessa. Non verrà disegnato nulla a sinistra del primo parametro o sopra il secondo.
   - I parametri 4 e 5 definiscono la larghezza e l'altezza dell'area da ritagliare dall'immagine originale caricata.
   - I parametri 6 e 7 definiscono le coordinate alle quali disegnare l'angolo superiore sinistro della porzione ritagliata dell'immagine, rispetto all'angolo superiore sinistro del canvas.
   - I parametri 8 e 9 definiscono la larghezza e l'altezza con cui disegnare l'area ritagliata dell'immagine. In questo caso sono state specificate le stesse dimensioni della fetta originale, ma sarebbe possibile ridimensionarla specificando valori diversi.

5. Quando l'immagine viene aggiornata in modo significativo, anche la descrizione deve essere aggiornata.

   ```js live-sample___5-canvas-images
   canvas.setAttribute("aria-label", "Firefox Logo");
   ```

L'esempio finale dovrebbe apparire così:

{{EmbedLiveSample("5-canvas-images", '100%', 260)}}

Premere il pulsante **Play** per aprire l'esempio in MDN Playground e modificare il codice sorgente.

## Cicli e animazioni

Finora sono stati trattati alcuni utilizzi molto basilari del canvas 2D, ma non si sperimenterà davvero tutta la potenza del canvas a meno che non venga aggiornato o animato in qualche modo. Dopotutto, canvas fornisce immagini controllabili tramite script. Se non si intende modificare nulla, tanto vale usare immagini statiche e risparmiarsi tutto il lavoro.

### Creare un ciclo

Sperimentare con i cicli in canvas è piuttosto divertente: è possibile eseguire comandi canvas all'interno di un ciclo [`for`](/it/docs/Web/JavaScript/Reference/Statements/for), o di un altro tipo, proprio come qualsiasi altro codice JavaScript.

Costruiamo un esempio.

1. Creare un'altra nuova copia del template canvas.
2. Aggiungere la seguente riga in fondo al JavaScript. Questa contiene un nuovo metodo, [`translate()`](/it/docs/Web/API/CanvasRenderingContext2D/translate), che sposta il punto di origine del canvas:

   ```js live-sample___6-canvas-for-loop
   ctx.translate(width / 2, height / 2);
   ```

   Questo fa sì che l'origine delle coordinate (0, 0) venga spostata al centro del canvas, anziché trovarsi nell'angolo superiore sinistro. È molto utile in molte situazioni, come questa, nella quale si desidera che il disegno venga realizzato rispetto al centro del canvas.

3. Ora aggiungere il seguente codice in fondo al JavaScript:

   ```js live-sample___6-canvas-for-loop
   function degToRad(degrees) {
     return (degrees * Math.PI) / 180;
   }

   function rand(min, max) {
     return Math.floor(Math.random() * (max - min + 1)) + min;
   }

   let length = 250;
   let moveOffset = 20;
   ```

   Qui viene implementata la stessa funzione `degToRad()` vista nell'esempio del triangolo precedente, una funzione `rand()` che restituisce un numero casuale tra limiti inferiore e superiore dati e le variabili `length` e `moveOffset`, sulle quali si scoprirà di più in seguito.

4. L'idea è di disegnare qualcosa sul canvas all'interno del ciclo `for` e iterare a ogni passaggio per creare qualcosa di interessante. Aggiungere il seguente codice all'interno del ciclo `for`:

   ```js live-sample___6-canvas-for-loop
   for (let i = 0; i < length; i++) {
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
   }
   ```

   Quindi, a ogni iterazione:
   - Si imposta `fillStyle` su una tonalità di viola leggermente trasparente, che cambia ogni volta in base al valore di `length`. Come si vedrà in seguito, la lunghezza diminuisce ogni volta che viene eseguito il ciclo, quindi l'effetto è che il colore diventa più luminoso con ogni triangolo successivo disegnato.
   - Si inizia il percorso.
   - Si sposta la penna a una coordinata di `(moveOffset, moveOffset)`. Questa variabile definisce di quanto spostarsi ogni volta che viene disegnato un nuovo triangolo.
   - Si disegna una linea fino a una coordinata di `(moveOffset+length, moveOffset)`. Questo disegna una linea di lunghezza `length` parallela all'asse X.
   - Si calcola l'altezza del triangolo, come in precedenza.
   - Si disegna una linea fino all'angolo rivolto verso il basso del triangolo, quindi una linea di ritorno all'inizio del triangolo.
   - Si chiama `fill()` per riempire il triangolo.
   - Si aggiornano le variabili che descrivono la sequenza di triangoli, per prepararsi a disegnare quello successivo. Il valore `length` viene diminuito di 1, così i triangoli diventano più piccoli ogni volta; `moveOffset` viene aumentato di una piccola quantità affinché ogni triangolo successivo sia leggermente più distante, e viene usata un'altra nuova funzione, [`rotate()`](/it/docs/Web/API/CanvasRenderingContext2D/rotate), che consente di ruotare l'intero canvas. Il canvas viene ruotato di 5 gradi prima di disegnare il triangolo successivo.

Ecco fatto. L'esempio finale dovrebbe apparire così:

{{EmbedLiveSample("6-canvas-for-loop", '100%', 550)}}

Premere il pulsante **Play** per aprire l'esempio in MDN Playground e modificare il codice sorgente. Si incoraggia a sperimentare con l'esempio e renderlo personale. Ad esempio:

- Disegnare rettangoli o archi invece di triangoli, oppure persino incorporare immagini.
- Sperimentare con i valori `length` e `moveOffset`.
- Introdurre alcuni numeri casuali usando la funzione `rand()` inclusa sopra ma non utilizzata.

### Animazioni

L'esempio di ciclo creato sopra era divertente, ma per qualunque applicazione canvas seria, come giochi e visualizzazioni in tempo reale, è davvero necessario un ciclo costante che continui a essere eseguito. Se si pensa al canvas come a un film, è necessario che il display si aggiorni a ogni fotogramma per mostrare la vista aggiornata, con una frequenza di aggiornamento ideale di 60 fotogrammi al secondo affinché il movimento appaia gradevole e fluido all'occhio umano.

Esistono alcune funzioni JavaScript che consentono di eseguire funzioni ripetutamente, diverse volte al secondo; la migliore per gli scopi di questo caso è [`window.requestAnimationFrame()`](/it/docs/Web/API/Window/requestAnimationFrame). Accetta un parametro: il nome della funzione da eseguire per ciascun fotogramma. La prossima volta che il browser sarà pronto ad aggiornare lo schermo, la funzione verrà chiamata. Se questa funzione disegna il nuovo aggiornamento dell'animazione e poi chiama di nuovo `requestAnimationFrame()` poco prima della fine della funzione, il ciclo di animazione continuerà a essere eseguito. Il ciclo termina quando si smette di chiamare `requestAnimationFrame()` oppure se si chiama [`window.cancelAnimationFrame()`](/it/docs/Web/API/Window/cancelAnimationFrame) dopo aver chiamato `requestAnimationFrame()` ma prima che il fotogramma venga chiamato.

> [!NOTE]
> È buona pratica chiamare `cancelAnimationFrame()` dal codice principale quando si termina di usare l'animazione, per garantire che non vi siano ancora aggiornamenti in attesa di essere eseguiti.

Il browser gestisce dettagli complessi, come l'esecuzione dell'animazione a una velocità costante e il non spreco di risorse nell'animazione di elementi che non possono essere visti.

Per vedere come funziona, diamo una rapida occhiata di nuovo al nostro [esempio delle palline rimbalzanti](#frame_bouncing-balls). Il codice del ciclo che mantiene tutto in movimento ha questo aspetto:

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

La funzione `loop()` viene eseguita una volta in fondo al codice per avviare il ciclo e disegnare il primo fotogramma dell'animazione; la funzione `loop()` si occupa poi di chiamare `requestAnimationFrame(loop)` per eseguire il fotogramma successivo dell'animazione, ripetutamente.

Si noti che a ogni fotogramma il canvas viene completamente svuotato e tutto viene ridisegnato. Per ogni pallina presente, questa viene disegnata, la sua posizione viene aggiornata e viene verificato se collide con altre palline. Una volta disegnata una grafica su un canvas, non è possibile manipolare quella grafica singolarmente come si può fare con gli elementi DOM. Non è possibile spostare ciascuna pallina nel canvas perché, una volta disegnata, essa diventa parte del canvas e non è un elemento o oggetto individuale accessibile. Occorre invece cancellare e ridisegnare, cancellando l'intero fotogramma e ridisegnando tutto oppure disponendo di codice che sappia esattamente quali parti devono essere cancellate e che cancelli e ridisegni solo l'area minima necessaria del canvas.

L'ottimizzazione dell'animazione della grafica è un'intera specializzazione della programmazione, con molte tecniche ingegnose disponibili. Tuttavia, queste vanno oltre ciò che serve per l'esempio.

In generale, il processo di creazione di un'animazione canvas comprende i passaggi seguenti:

1. Cancellare il contenuto del canvas, ad esempio con [`fillRect()`](/it/docs/Web/API/CanvasRenderingContext2D/fillRect) o [`clearRect()`](/it/docs/Web/API/CanvasRenderingContext2D/clearRect).
2. Salvare lo stato, se necessario, usando [`save()`](/it/docs/Web/API/CanvasRenderingContext2D/save). Questo è necessario quando si desidera salvare impostazioni aggiornate sul canvas prima di proseguire ed è utile per applicazioni più avanzate.
3. Disegnare la grafica da animare.
4. Ripristinare le impostazioni salvate al passaggio 2 usando [`restore()`](/it/docs/Web/API/CanvasRenderingContext2D/restore).
5. Chiamare `requestAnimationFrame()` per pianificare il disegno del fotogramma successivo dell'animazione.

> [!NOTE]
> `save()` e `restore()` non verranno trattati qui, ma sono ben spiegati nel tutorial [Trasformazioni](/it/docs/Web/API/Canvas_API/Tutorial/Transformations), e in quelli che lo seguono.

### Animazione di un oggetto che cammina

Creiamo ora una semplice animazione personale: verrà animato un oggetto in movimento attraverso lo schermo usando uno sprite sheet.

1. Creare un'altra nuova copia del template canvas e aprirla nell'editor di codice.

2. Aggiornare l'HTML di fallback affinché rifletta l'immagine:

   ```html live-sample___7-canvas-walking-animation
   <canvas class="myCanvas">
     <p>A cat walking.</p>
   </canvas>
   ```

3. Questa volta non verrà colorato di nero lo sfondo. Dopo aver acquisito la variabile `ctx`, dipingere invece lo sfondo in grigio chiaro:

   ```js live-sample___7-canvas-walking-animation
   ctx.fillStyle = "#e5e6e9";
   ctx.fillRect(0, 0, width, height);
   ```

4. In fondo al JavaScript, aggiungere la seguente riga per posizionare nuovamente l'origine delle coordinate al centro del canvas:

   ```js live-sample___7-canvas-walking-animation
   ctx.translate(width / 2, height / 2);
   ```

5. Creiamo ora un nuovo oggetto [`HTMLImageElement`](/it/docs/Web/API/HTMLImageElement), impostiamo il suo [`src`](/it/docs/Web/API/HTMLImageElement/src) sull'immagine da caricare e aggiungiamo un gestore dell'evento `onload` che farà attivare la funzione `draw()` quando l'immagine verrà caricata:

   ```js live-sample___7-canvas-walking-animation
   const image = new Image();
   image.src =
     "https://developer.mozilla.org/shared-assets/images/examples/web-animations/cat_sprite.png";
   image.onload = draw;
   ```

6. Ora aggiungiamo alcune variabili per tenere traccia della posizione nella quale disegnare lo sprite sullo schermo e del numero dello sprite da visualizzare.

   ```js live-sample___7-canvas-walking-animation
   let spriteIndex = 0;
   let posX = 0;
   const spriteWidth = 300;
   const spriteHeight = 150;
   const totalSprites = 12;
   ```

   L'immagine sprite è stata creata e condivisa per gentile concessione di [Rachel Nabors](https://nearestnabors.com/), per il suo lavoro di documentazione sulla [Web Animations API](/it/docs/Web/API/Web_Animations_API). Ha questo aspetto:

   ![Uno sprite sheet con tre colonne, ciascuna contenente una sequenza di immagini di un gatto nero che si muove verso sinistra a ritmi diversi. Ogni sprite è largo 300 pixel e alto 150 pixel.](/shared-assets/images/examples/web-animations/cat_sprite.png)

   Presenta tre colonne. Ogni colonna è una sequenza che rappresenta il gatto mentre si muove a un ritmo diverso: camminando, trotterellando e galoppando. Ogni sequenza contiene 12 o 13 sprite, ciascuno largo 300 pixel e alto 150 pixel. Verrà usata la sequenza di camminata più a sinistra, che contiene 12 sprite. Per visualizzare ogni sprite in modo pulito, sarà necessario usare `drawImage()` per ritagliare una singola immagine sprite dallo spritesheet e visualizzare solo quella parte, come fatto sopra con il logo Firefox. Le coordinate X e Y della fetta dovranno essere rispettivamente un multiplo di `spriteWidth` e `spriteHeight`; poiché viene usata la sequenza più a sinistra, la coordinata X è sempre 0. La dimensione della fetta sarà sempre `spriteWidth` per `spriteHeight`.

7. Inseriamo ora una funzione `draw()` vuota in fondo al codice, pronta per essere riempita con del codice:

   ```js
   function draw() {}
   ```

   ```js-nolint hidden live-sample___7-canvas-walking-animation
   function draw() {
   ```

8. Il resto del codice di questa sezione va inserito dentro `draw()`. Innanzitutto, aggiungere la seguente riga, che cancella il canvas per prepararsi a disegnare ciascun fotogramma. Si noti che è necessario specificare l'angolo superiore sinistro del rettangolo come `-(width / 2), -(height / 2)` perché in precedenza la posizione dell'origine è stata specificata come `width/2, height/2`.

   ```js live-sample___7-canvas-walking-animation
   ctx.fillRect(-(width / 2), -(height / 2), width, height);
   ```

9. Successivamente, disegneremo l'immagine usando la versione a 9 parametri di drawImage. Aggiungere quanto segue:

   ```js live-sample___7-canvas-walking-animation
   ctx.drawImage(
     image,
     0,
     spriteIndex * spriteHeight,
     spriteWidth,
     spriteHeight,
     0 + posX,
     -spriteHeight / 2,
     spriteWidth,
     spriteHeight,
   );
   ```

   Come si può vedere:
   - Viene specificato `image` come immagine da incorporare.
   - I parametri 2 e 3 specificano l'angolo superiore sinistro della fetta da ritagliare dall'immagine sorgente, con il valore X pari a 0, per la colonna più a sinistra, e il valore Y che scorre tra multipli di `spriteHeight`. È possibile sostituire il valore X con `spriteWidth` o `2 * spriteWidth` per selezionare le altre colonne.
   - I parametri 4 e 5 specificano la dimensione della fetta da ritagliare: `spriteWidth` e `spriteHeight`.
   - I parametri 6 e 7 specificano l'angolo superiore sinistro della casella nella quale disegnare la fetta sul canvas. La posizione X è 0 + `posX`, il che significa che è possibile alterare la posizione di disegno modificando il valore `posX`. La posizione Y è `-spriteHeight / 2`, il che significa che l'immagine sarà centrata verticalmente sul canvas.
   - I parametri 8 e 9 specificano la dimensione dell'immagine sul canvas. Si desidera mantenere la dimensione originale, quindi vengono specificati `spriteWidth` e `spriteHeight` come larghezza e altezza.

10. Ora verrà modificato il valore `spriteIndex` dopo ogni disegno, o meglio, dopo alcuni di essi. Aggiungere il seguente blocco in fondo alla funzione `draw()`:

    ```js live-sample___7-canvas-walking-animation
    if (posX % 11 === 0) {
      if (spriteIndex === totalSprites - 1) {
        spriteIndex = 0;
      } else {
        spriteIndex++;
      }
    }
    ```

    L'intero blocco è racchiuso in `if (posX % 11 === 0) { }`. Viene usato l'operatore modulo (`%`), noto anche come [operatore resto](/it/docs/Web/JavaScript/Reference/Operators/Remainder), per verificare se il valore `posX` può essere diviso esattamente per 11 senza resto. In tal caso, si passa allo sprite successivo incrementando `spriteIndex`, tornando a 0 dopo l'ultimo. Questo significa effettivamente che lo sprite viene aggiornato solo ogni 11° fotogramma, ovvero approssimativamente circa 6 fotogrammi al secondo, dato che `requestAnimationFrame()` viene chiamato fino a 60 fotogrammi al secondo se possibile. La frequenza dei fotogrammi viene deliberatamente rallentata perché ci sono solo 12 sprite disponibili e, visualizzandone uno ogni sessantesimo di secondo, l'oggetto si muoverebbe troppo velocemente.

    All'interno del blocco esterno viene usata un'istruzione [`if...else`](/it/docs/Web/JavaScript/Reference/Statements/if...else) per verificare se il valore `spriteIndex` corrisponde all'ultimo. Se viene già mostrato l'ultimo sprite, `spriteIndex` viene reimpostato a 0; in caso contrario, viene semplicemente incrementato di 1.

11. Successivamente è necessario stabilire come modificare il valore `posX` a ogni fotogramma. Aggiungere il seguente blocco di codice subito sotto l'ultimo.

    ```js live-sample___7-canvas-walking-animation
    if (posX < -width / 2 - spriteWidth) {
      const newStartPos = width / 2;
      posX = Math.ceil(newStartPos);
    } else {
      posX -= 2;
    }
    ```

    Viene usata un'altra istruzione `if...else` per verificare se il valore di `posX` è diventato inferiore a `-width/2 - spriteWidth`, il che significa che il gatto è uscito dal bordo sinistro dello schermo. In tal caso, viene calcolata una posizione che collocherebbe il gatto appena a destra del lato destro dello schermo.

    Se il gatto non è ancora uscito dal bordo dello schermo, `posX` viene diminuito di 2. Questo lo farà spostare leggermente verso sinistra la prossima volta che verrà disegnato.

12. Infine, è necessario creare il ciclo di animazione chiamando [`requestAnimationFrame()`](/it/docs/Web/API/Window/requestAnimationFrame) in fondo alla funzione `draw()`:

    ```js live-sample___7-canvas-walking-animation
    window.requestAnimationFrame(draw);
    ```

```js-nolint hidden live-sample___7-canvas-walking-animation
}
```

Ecco fatto. L'esempio finale dovrebbe apparire così:

{{EmbedLiveSample("7-canvas-walking-animation", '100%', 260)}}

È possibile premere il pulsante **Play** per aprire l'esempio in MDN Playground e modificare il codice sorgente.

### Una semplice applicazione di disegno

Come esempio finale di animazione, viene mostrata una semplicissima applicazione di disegno, per illustrare come il ciclo di animazione possa essere combinato con l'input dell'utente, in questo caso il movimento del mouse. Non verrà illustrata la sua costruzione passo per passo; saranno esplorate soltanto le parti più interessanti del codice.

```html hidden live-sample___8-canvas-drawing-app
<div class="toolbar">
  <input type="color" aria-label="select pen color" value="#ff0000" />
  <div>
    <input
      type="range"
      min="2"
      max="50"
      value="30"
      aria-label="select pen size" /><span class="output">30</span>
  </div>
  <button>Clear canvas</button>
</div>

<canvas class="myCanvas">
  <p>Add suitable fallback here.</p>
</canvas>
```

```css hidden live-sample___8-canvas-drawing-app
body {
  margin: 0;
  overflow: hidden;
  background: #cccccc;
}

.toolbar {
  height: 75px;
  background: #cccccc;
  padding: 5px 20px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.toolbar div {
  margin: 0 20px;
  flex: 3;
}

input[type="color"],
button {
  flex: 1;
}

input[type="range"] {
  width: calc(100% - 20px);
}

output {
  width: 20px;
}

span {
  position: relative;
  bottom: 5px;
}
```

```js hidden live-sample___8-canvas-drawing-app
const canvas = document.querySelector(".myCanvas");
const width = (canvas.width = window.innerWidth);
const height = (canvas.height = window.innerHeight - 85);
const ctx = canvas.getContext("2d");

ctx.fillStyle = "black";
ctx.fillRect(0, 0, width, height);

const colorPicker = document.querySelector('input[type="color"]');
const sizePicker = document.querySelector('input[type="range"]');
const output = document.querySelector(".output");
const clearBtn = document.querySelector("button");

// covert degrees to radians
function degToRad(degrees) {
  return (degrees * Math.PI) / 180;
}

// update sizePicker output value

sizePicker.addEventListener(
  "input",
  () => (output.textContent = sizePicker.value),
);
```

È possibile sperimentare con l'esempio dal vivo qui sotto; è inoltre possibile fare clic sul pulsante **Play** per aprirlo in MDN Playground, dove è possibile modificare il codice sorgente:

{{EmbedLiveSample("8-canvas-drawing-app", '100%', 600)}}

Vediamo le parti più interessanti. Innanzitutto, vengono tenute traccia delle coordinate X e Y del mouse e del fatto che sia premuto o meno tramite tre variabili: `curX`, `curY` e `pressed`. Quando il mouse si muove, viene attivata una funzione impostata come gestore dell'evento `onmousemove`, che acquisisce i valori X e Y correnti. Vengono inoltre usati i gestori degli eventi `onmousedown` e `onmouseup` per modificare il valore di `pressed` in `true` quando viene premuto il pulsante del mouse e di nuovo in `false` quando viene rilasciato.

```js live-sample___8-canvas-drawing-app
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

Quando viene premuto il pulsante "Clear canvas", viene eseguita una semplice funzione che cancella l'intero canvas riportandolo al nero, come visto in precedenza:

```js live-sample___8-canvas-drawing-app
clearBtn.addEventListener("click", () => {
  ctx.fillStyle = "black";
  ctx.fillRect(0, 0, width, height);
});
```

Questa volta il ciclo di disegno è piuttosto semplice: se pressed è `true`, viene disegnato un cerchio con uno stile di riempimento uguale al valore nel selettore di colore e un raggio uguale al valore impostato nell'input range. Il cerchio deve essere disegnato 85 pixel sopra il punto in cui è stato misurato, poiché la misura verticale viene presa dalla parte superiore del viewport, ma il cerchio viene disegnato rispetto alla parte superiore del canvas, che inizia sotto la barra degli strumenti alta 85 pixel. Se venisse disegnato usando solo `curY` come coordinata y, apparirebbe 85 pixel più in basso rispetto alla posizione del mouse.

```js live-sample___8-canvas-drawing-app
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

Tutti i tipi {{htmlelement("input")}} sono ben supportati. Se un browser non supporta un tipo di input, ricorrerà a semplici campi di testo.

## WebGL

È ora il momento di lasciare il 2D e dare una rapida occhiata al canvas 3D. Il contenuto canvas 3D viene specificato usando la [WebGL API](/it/docs/Web/API/WebGL_API), che è un'API completamente separata dalla canvas API 2D, anche se entrambe eseguono il rendering su elementi {{htmlelement("canvas")}}.

WebGL si basa su {{Glossary("OpenGL", "OpenGL")}}, Open Graphics Library, e consente di comunicare direttamente con la {{Glossary("GPU", "GPU")}} del computer. Pertanto, scrivere WebGL puro è più simile a linguaggi di basso livello come C++ che al normale JavaScript: è piuttosto complesso, ma incredibilmente potente.

### Usare una libreria

A causa della sua complessità, la maggior parte delle persone scrive codice grafico 3D usando una libreria JavaScript di terze parti, come [Three.js](/it/docs/Games/Techniques/3D_on_the_web/Building_up_a_basic_demo_with_Three.js), [PlayCanvas](/it/docs/Games/Techniques/3D_on_the_web/Building_up_a_basic_demo_with_PlayCanvas) o [Babylon.js](/it/docs/Games/Techniques/3D_on_the_web/Building_up_a_basic_demo_with_Babylon.js). La maggior parte di queste funziona in modo simile, fornendo funzionalità per creare forme primitive e personalizzate, posizionare telecamere di visualizzazione e luci, coprire superfici con texture e altro. Gestiscono WebGL al posto dello sviluppatore, consentendo di lavorare a un livello più alto.

Sì, usarne una significa imparare un'altra API nuova, in questo caso di terze parti, ma sono molto più semplici rispetto alla scrittura di WebGL puro.

### Un cubo rotante

Vediamo un esempio di come creare qualcosa con una libreria WebGL. Verrà scelta [Three.js](/it/docs/Games/Techniques/3D_on_the_web/Building_up_a_basic_demo_with_Three.js), poiché è una delle più popolari. In questo tutorial verrà creato un cubo 3D rotante.

1. Per iniziare, creare una nuova cartella sul disco rigido locale denominata `webgl-cube`.
2. Al suo interno, creare un nuovo file denominato `index.html` e aggiungervi il seguente contenuto:

   ```html
   <!doctype html>
   <html lang="en-US">
     <head>
       <meta charset="utf-8" />
       <meta name="viewport" content="width=device-width" />

       <title>Three.js basic cube example</title>

       <script src="https://cdn.jsdelivr.net/npm/three-js@79.0.0/three.min.js"></script>
       <script src="script.js" defer></script>
       <link href="style.css" rel="stylesheet" />
     </head>

     <body></body>
   </html>
   ```

   ```html hidden live-sample___9-webgl-cube
   <script src="https://cdn.jsdelivr.net/npm/three-js@79.0.0/three.min.js"></script>
   ```

3. Successivamente, creare un altro nuovo file denominato `script.js`, ancora nella stessa cartella. Per il momento lasciarlo vuoto.
4. Ora creare un altro nuovo file denominato `style.css`, ancora nella stessa cartella, e aggiungervi il seguente contenuto:

   ```css live-sample___9-webgl-cube
   html,
   body {
     margin: 0;
   }

   body {
     overflow: hidden;
   }
   ```

5. `three.js` è incluso nella pagina, come fa il primo elemento `<script>` nell'HTML, quindi ora è possibile iniziare a scrivere in `script.js` JavaScript che lo utilizza. Iniziamo creando una nuova scena: aggiungere quanto segue nel file `script.js`:

   ```js live-sample___9-webgl-cube
   const scene = new THREE.Scene();
   ```

   Il costruttore [`Scene()`](https://threejs.org/docs/index.html#api/en/scenes/Scene) crea una nuova scena, che rappresenta l'intero mondo 3D che si sta cercando di visualizzare.

6. Successivamente è necessaria una **telecamera** per poter vedere la scena. Nella terminologia delle immagini 3D, la telecamera rappresenta la posizione dell'osservatore nel mondo. Per creare una telecamera, aggiungere le righe seguenti:

   ```js live-sample___9-webgl-cube
   const camera = new THREE.PerspectiveCamera(
     75,
     window.innerWidth / window.innerHeight,
     0.1,
     1000,
   );
   camera.position.z = 5;
   ```

   Il costruttore [`PerspectiveCamera()`](https://threejs.org/docs/index.html#api/en/cameras/PerspectiveCamera) accetta quattro argomenti:
   - Il campo visivo: quanto deve essere ampia, in gradi, l'area davanti alla telecamera visibile sullo schermo.
   - Il {{Glossary("aspect_ratio", "rapporto d'aspetto")}}: di solito è il rapporto tra la larghezza della scena divisa per l'altezza della scena. L'utilizzo di un altro valore distorcerà la scena, il che potrebbe essere desiderato, ma solitamente non lo è.
   - Il piano vicino: quanto gli oggetti possono essere vicini alla telecamera prima che si smetta di eseguirne il rendering sullo schermo. Si pensi a quando si avvicina sempre di più la punta di un dito allo spazio tra gli occhi: a un certo punto non è più possibile vederla.
   - Il piano lontano: quanto possono essere lontane le cose dalla telecamera prima che non vengano più renderizzate.

   Viene inoltre impostata la posizione della telecamera a 5 unità di distanza lungo l'asse Z che, come in CSS, esce dallo schermo verso l'osservatore.

7. Il terzo ingrediente fondamentale è un renderer. Si tratta di un oggetto che esegue il rendering di una determinata scena così come viene osservata attraverso una determinata telecamera. Per ora ne verrà creato uno usando il costruttore [`WebGLRenderer()`](https://threejs.org/docs/index.html#api/en/renderers/WebGLRenderer), ma non verrà usato fino a più tardi. Aggiungere le seguenti righe:

   ```js live-sample___9-webgl-cube
   const renderer = new THREE.WebGLRenderer();
   renderer.setSize(window.innerWidth, window.innerHeight);
   document.body.appendChild(renderer.domElement);
   ```

   La prima riga crea un nuovo renderer, la seconda riga imposta la dimensione alla quale il renderer disegnerà la visuale della telecamera e la terza riga aggiunge l'elemento {{htmlelement("canvas")}} creato dal renderer all'elemento {{htmlelement("body")}} del documento. Ora tutto ciò che il renderer disegna verrà visualizzato nella finestra.

8. Successivamente, si desidera creare il cubo da visualizzare sul canvas. Aggiungere il seguente blocco di codice in fondo al JavaScript:

   ```js live-sample___9-webgl-cube
   let cube;

   const loader = new THREE.TextureLoader();

   loader.load(
     "https://mdn.github.io/shared-assets/images/examples/learn/metal003.png",
     (texture) => {
       texture.wrapS = THREE.RepeatWrapping;
       texture.wrapT = THREE.RepeatWrapping;
       texture.repeat.set(2, 2);

       const geometry = new THREE.BoxGeometry(2.4, 2.4, 2.4);
       const material = new THREE.MeshLambertMaterial({ map: texture });
       cube = new THREE.Mesh(geometry, material);
       scene.add(cube);

       draw();
     },
   );
   ```

   Qui c'è un po' di più da assimilare, quindi analizziamolo per fasi:
   - Innanzitutto viene creata una variabile globale `cube`, in modo da poter accedere al cubo da qualsiasi punto del codice.
   - Successivamente viene creato un nuovo oggetto [`TextureLoader`](https://threejs.org/docs/index.html#api/en/loaders/TextureLoader), quindi viene chiamato `load()` su di esso. In questo caso `load()` accetta due parametri, sebbene possa accettarne di più: la texture da caricare, un PNG, e una funzione che verrà eseguita quando la texture sarà stata caricata.
   - All'interno di questa funzione vengono usate le proprietà dell'oggetto [`texture`](https://threejs.org/docs/index.html#api/en/textures/Texture) per specificare che si desidera una ripetizione 2 × 2 dell'immagine avvolta su tutti i lati del cubo. Successivamente vengono creati un nuovo oggetto [`BoxGeometry`](https://threejs.org/docs/index.html#api/en/geometries/BoxGeometry) e un nuovo oggetto [`MeshLambertMaterial`](https://threejs.org/docs/index.html#api/en/materials/MeshLambertMaterial), riunendoli in una [`Mesh`](https://threejs.org/docs/index.html#api/en/objects/Mesh) per creare il cubo. Un oggetto richiede tipicamente una geometria, cioè quale forma abbia, e un materiale, cioè l'aspetto della sua superficie.
   - Infine viene aggiunto il cubo alla scena e viene chiamata la funzione `draw()` per avviare l'animazione.

9. Prima di definire `draw()`, verranno aggiunte un paio di luci alla scena per ravvivare un po' l'ambiente. Aggiungere i seguenti blocchi:

   ```js live-sample___9-webgl-cube
   const light = new THREE.AmbientLight("white"); // soft white light
   scene.add(light);

   const spotLight = new THREE.SpotLight("white");
   spotLight.position.set(100, 1000, 1000);
   spotLight.castShadow = true;
   scene.add(spotLight);
   ```

   Un oggetto [`AmbientLight`](https://threejs.org/docs/index.html#api/en/lights/AmbientLight) è un tipo di luce soffusa che illumina leggermente l'intera scena, come il sole quando si è all'aperto. L'oggetto [`SpotLight`](https://threejs.org/docs/index.html#api/en/lights/SpotLight), invece, è un fascio di luce direzionale, più simile a una torcia o a un faretto.

10. Infine, aggiungere la funzione `draw()` in fondo al codice:

    ```js live-sample___9-webgl-cube
    function draw() {
      cube.rotation.x += 0.01;
      cube.rotation.y += 0.01;
      renderer.render(scene, camera);

      requestAnimationFrame(draw);
    }
    ```

    Questo è abbastanza intuitivo: a ogni fotogramma, il cubo viene ruotato leggermente sui suoi assi X e Y, quindi viene eseguito il rendering della scena vista dalla telecamera e infine viene chiamato `requestAnimationFrame()` per pianificare il disegno del fotogramma successivo.

Il prodotto finito dovrebbe apparire così:

{{EmbedLiveSample("9-webgl-cube", "100%", 500)}}

> [!NOTE]
> Nel repository GitHub è possibile trovare anche un altro interessante esempio di cubo 3D: [Three.js Video Cube](https://github.com/mdn/learning-area/tree/main/javascript/apis/drawing-graphics/threejs-video-cube) ([visualizzabile anche dal vivo](https://mdn.github.io/learning-area/javascript/apis/drawing-graphics/threejs-video-cube/)). Questo usa [`getUserMedia()`](/it/docs/Web/API/MediaDevices/getUserMedia) per acquisire un flusso video dalla webcam del computer e proiettarlo sul lato del cubo come texture.

## Riepilogo

A questo punto dovrebbe essere disponibile un'idea utile dei fondamenti della programmazione grafica con Canvas e WebGL, di ciò che è possibile fare con queste API e di dove trovare ulteriori informazioni. Buon divertimento!

## Vedere anche

Qui sono stati trattati solo i fondamenti essenziali del canvas: c'è molto altro da imparare. Gli articoli seguenti consentiranno di approfondire.

- [Tutorial Canvas](/it/docs/Web/API/Canvas_API/Tutorial) — Una serie di tutorial molto dettagliata che spiega ciò che è necessario sapere sul canvas 2D in modo molto più approfondito di quanto trattato qui. Lettura essenziale.
- [Tutorial WebGL](/it/docs/Web/API/WebGL_API/Tutorial) — Una serie che insegna le basi della programmazione WebGL pura.
- [Creare una demo di base con Three.js](/it/docs/Games/Techniques/3D_on_the_web/Building_up_a_basic_demo_with_Three.js) — Tutorial di base su Three.js. Sono disponibili anche guide equivalenti per [PlayCanvas](/it/docs/Games/Techniques/3D_on_the_web/Building_up_a_basic_demo_with_PlayCanvas) e [Babylon.js](/it/docs/Games/Techniques/3D_on_the_web/Building_up_a_basic_demo_with_Babylon.js).
- [Sviluppo di giochi](/it/docs/Games) — La pagina di destinazione di MDN per lo sviluppo di giochi web. Qui sono disponibili tutorial e tecniche molto utili relativi al canvas 2D e 3D; consultare le opzioni del menu Techniques and Tutorials.

## Esempi

- [Violent theremin](https://github.com/mdn/webaudio-examples/tree/main/violent-theremin) — Usa la Web Audio API per generare suono e canvas per generare una piacevole visualizzazione di accompagnamento.
- [Voice change-o-matic](https://github.com/mdn/webaudio-examples/tree/main/voice-change-o-matic) — Usa un canvas per visualizzare dati audio in tempo reale dalla Web Audio API.

{{PreviousMenuNext("Learn_web_development/Extensions/Client-side_APIs/Video_and_audio_APIs", "Learn_web_development/Extensions/Client-side_APIs/Client-side_storage", "Learn_web_development/Extensions/Client-side_APIs")}}
