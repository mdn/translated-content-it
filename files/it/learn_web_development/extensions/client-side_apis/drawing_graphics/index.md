---
title: Disegnare grafica
slug: Learn_web_development/Extensions/Client-side_APIs/Drawing_graphics
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Client-side_APIs/Video_and_audio_APIs", "Learn_web_development/Extensions/Client-side_APIs/Client-side_storage", "Learn_web_development/Extensions/Client-side_APIs")}}

Il browser contiene alcuni strumenti molto potenti per la programmazione grafica, dal linguaggio Scalable Vector Graphics ([SVG](/it/docs/Web/SVG)), alle API per disegnare sugli elementi HTML {{htmlelement("canvas")}}, (vedi [The Canvas API](/it/docs/Web/API/Canvas_API) e [WebGL](/it/docs/Web/API/WebGL_API)). Questo articolo fornisce un'introduzione al `canvas`, e ulteriori risorse per permetterle di saperne di più.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, specialmente le <a href="/it/docs/Learn_web_development/Core/Scripting/Object_basics">basi degli oggetti JavaScript</a> e la copertura delle API di base come <a href="/it/docs/Learn_web_development/Core/Scripting/DOM_scripting">DOM scripting</a> e <a href="/it/docs/Learn_web_development/Core/Scripting/Network_requests">richieste di rete</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>I concetti e i casi d'uso abilitati dalle API trattate in questa lezione.</li>
          <li>La sintassi di base e l'uso di <code>&lt;canvas&gt;</code> e delle API associate.</li>
          <li>Uso di timer e <code>requestAnimationFrame()</code> per impostare cicli di animazione.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Grafica sul Web

Il Web era originariamente solo testo, il che era molto noioso, quindi furono introdotte le immagini — prima attraverso l'elemento {{htmlelement("img")}} e successivamente attraverso proprietà CSS come {{cssxref("background-image")}}, e [SVG](/it/docs/Web/SVG).

Tuttavia, questo non era ancora sufficiente. Anche se poteva usare [CSS](/it/docs/Learn_web_development/Core/Styling_basics) e [JavaScript](/it/docs/Learn_web_development/Core/Scripting) per animare (e altrimenti manipolare) immagini vettoriali SVG — poiché sono rappresentate da markup — ancora non c'era modo di fare lo stesso per le immagini bitmap, e gli strumenti disponibili erano piuttosto limitati. Il Web non aveva ancora un modo efficace per creare animazioni, giochi, scene 3D, e altri requisiti comunemente gestiti da linguaggi di livello inferiore come C++ o Java.

La situazione iniziò a migliorare quando i browser iniziarono a supportare l'elemento {{htmlelement("canvas")}} e l'associata [Canvas API](/it/docs/Web/API/Canvas_API) nel 2004. Come vedrà sotto, `canvas` fornisce alcuni strumenti utili per creare animazioni 2D, giochi, visualizzazioni di dati e altri tipi di applicazioni, specialmente quando combinato con alcune delle altre API che la piattaforma web fornisce, ma può essere difficile o impossibile renderle accessibili.

L'esempio sotto mostra una semplice animazione di palle rimbalzanti basata su `canvas` 2D che abbiamo incontrato originariamente nel nostro modulo [Introduzione agli oggetti JavaScript](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_building_practice):

{{EmbedGHLiveSample("learning-area/javascript/oojs/bouncing-balls/index-finished.html", '100%', 500)}}

Intorno al 2006–2007, Mozilla iniziò a lavorare su un'implementazione sperimentale di `canvas` 3D. Questa divenne [WebGL](/it/docs/Web/API/WebGL_API), che guadagnò consensi tra i produttori di browser, e fu standardizzato intorno al 2009–2010. WebGL le consente di creare grafica 3D reale all'interno del suo browser web; l'esempio sotto mostra un semplice cubo WebGL rotante:

{{EmbedGHLiveSample("learning-area/javascript/apis/drawing-graphics/threejs-cube/index.html", '100%', 500)}}

Questo articolo si concentrerà principalmente sul `canvas` 2D, poiché il codice WebGL grezzo è molto complesso. Tuttavia, mostreremo come usare una libreria WebGL per creare una scena 3D in modo più semplice, e può trovare un tutorial che copre WebGL grezzo altrove — veda [Iniziare con WebGL](/it/docs/Web/API/WebGL_API/Tutorial/Getting_started_with_WebGL).

## Apprendimento attivo: Iniziare con un \<canvas>

Se desidera creare una scena 2D _o_ 3D su una pagina web, deve iniziare con un elemento HTML {{htmlelement("canvas")}}. Questo elemento viene usato per definire l'area della pagina in cui verrà disegnata l'immagine. È semplice come includere l'elemento sulla pagina:

```html
<canvas width="320" height="240"></canvas>
```

Questo creerà un `canvas` sulla pagina di dimensioni 320 per 240 pixel.

Dovrebbe mettere del contenuto di ripiego all'interno dei tag `<canvas>`. Questo dovrebbe descrivere il contenuto del `canvas` agli utenti di browser che non supportano il `canvas`, o agli utenti di screen reader.

```html
<canvas width="320" height="240">
  <p>Description of the canvas for those unable to view it.</p>
</canvas>
```

Il contenuto di ripiego dovrebbe fornire contenuto alternativo utile a quello del `canvas`. Per esempio, se sta rendendo un grafico in costante aggiornamento dei prezzi delle azioni, il contenuto di ripiego potrebbe essere un'immagine statica dell'ultimo grafico delle azioni, con testo `alt` che dice quali sono i prezzi testualmente o un elenco di link a singole pagine di azioni.

> [!NOTE]
> Il contenuto del `canvas` non è accessibile agli screen reader. Includa testo descrittivo come valore dell'attributo [`aria-label`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-label) direttamente sull'elemento `canvas` stesso o includa contenuto di ripiego posizionato all'interno dei tag `<canvas>` di apertura e chiusura. Il contenuto del `canvas` non fa parte del DOM, ma il contenuto di ripiego annidato sì.

### Creare e dimensionare il nostro canvas

Iniziamo creando il nostro `canvas` su cui disegnare gli esperimenti futuri.

1. Per prima cosa faccia una copia locale della directory [0_canvas_start](https://github.com/mdn/learning-area/tree/main/javascript/apis/drawing-graphics/getting-started/0_canvas_start). Contiene tre file:
   - "index.html"
   - "script.js"
   - "style.css"
2. Apra "index.html", e aggiunga il seguente codice, appena sotto il tag di apertura {{htmlelement("body")}}:

   ```html
   <canvas class="myCanvas">
     <p>Add suitable fallback here.</p>
   </canvas>
   ```

   Abbiamo aggiunto una `class` all'elemento `<canvas>` in modo che sarà più facile selezionarlo se abbiamo più `canvas` sulla pagina, ma abbiamo rimosso gli attributi `width` e `height` per ora (potrebbe aggiungerli di nuovo se volesse, ma li imposteremo usando JavaScript in una sezione successiva). I `canvas` senza larghezza e altezza esplicite predefinite sono larghi 300 pixel e alti 150 pixel.

3. Ora apra "script.js" e aggiunga le seguenti righe di JavaScript:

   ```js
   const canvas = document.querySelector(".myCanvas");
   const width = (canvas.width = window.innerWidth);
   const height = (canvas.height = window.innerHeight);
   ```

   Qui abbiamo memorizzato un riferimento al `canvas` nella costante `canvas`. Nella seconda riga impostiamo sia una nuova costante `width` che la proprietà `width` del `canvas` uguale a [`Window.innerWidth`](/it/docs/Web/API/Window/innerWidth) (che ci dà la larghezza del viewport). Nella terza riga impostiamo sia una nuova costante `height` che la proprietà `height` del `canvas` uguale a [`Window.innerHeight`](/it/docs/Web/API/Window/innerHeight) (che ci dà l'altezza del viewport). Quindi ora abbiamo un `canvas` che riempie tutta la larghezza e altezza della finestra del browser!

   Vedrà anche che stiamo concatenando assegnazioni insieme con più segni di uguale — questo è consentito in JavaScript, ed è una buona tecnica se vuole rendere più variabili tutte uguali allo stesso valore. Volevamo rendere le variabili width e height del `canvas` facilmente accessibili nelle variabili width/height, poiché sono valori utili da avere disponibili per dopo (per esempio, se vuole disegnare qualcosa esattamente a metà della larghezza del `canvas`).

> [!NOTE]
> Dovrebbe generalmente impostare la dimensione dell'immagine usando attributi HTML o proprietà DOM, come spiegato sopra. Potrebbe usare CSS, ma il problema in questo caso è che le dimensioni vengono calcolate dopo che il `canvas` è stato reso, e proprio come qualsiasi altra immagine (il `canvas` renderizzato è solo un'immagine), l'immagine potrebbe diventare pixelata/distorta.

### Ottenere il contesto del `canvas` e impostazione finale

Dobbiamo fare un'ultima cosa prima di poter considerare completato il nostro modello di `canvas`. Per disegnare sul `canvas` dobbiamo ottenere un riferimento speciale all'area di disegno chiamato contesto. Questo viene fatto usando il metodo [`HTMLCanvasElement.getContext()`](/it/docs/Web/API/HTMLCanvasElement/getContext), che per uso di base prende una singola stringa come parametro che rappresenta il tipo di contesto che si vuole ottenere.

In questo caso vogliamo un `canvas` 2d, quindi aggiunga la seguente riga JavaScript sotto le altre in "script.js":

```js
const ctx = canvas.getContext("2d");
```

> [!NOTE]
> Altri valori del contesto che potrebbe scegliere includono `webgl` per WebGL, `webgl2` per WebGL 2, ecc., ma non avremo bisogno di quelli in questo articolo.

Ecco fatto — il nostro `canvas` è ora pronto per essere disegnato! La variabile `ctx` contiene ora un oggetto [`CanvasRenderingContext2D`](/it/docs/Web/API/CanvasRenderingContext2D), e tutte le operazioni di disegno sul `canvas` coinvolgeranno la manipolazione di questo oggetto.

Facciamo un'ultima cosa prima di andare avanti. Coloreremo lo sfondo del `canvas` di nero per darle un primo assaggio delle API del `canvas`. Aggiunga le seguenti righe alla fine del suo JavaScript:

```js
ctx.fillStyle = "rgb(0 0 0)";
ctx.fillRect(0, 0, width, height);
```

Qui stiamo impostando un colore di riempimento usando la proprietà [`fillStyle`](/it/docs/Web/API/CanvasRenderingContext2D/fillStyle) del `canvas` (questa prende valori di [colore](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#color) proprio come fanno le proprietà CSS), quindi disegnando un rettangolo che copre tutta l'area del `canvas` con il metodo [`fillRect`](/it/docs/Web/API/CanvasRenderingContext2D/fillRect) (i primi due parametri sono le coordinate dell'angolo in alto a sinistra del rettangolo; gli ultimi due sono la larghezza e l'altezza che desidera che il rettangolo disegnato abbia — le abbiamo detto che le variabili width e height sarebbero state utili)!

OK, il nostro modello è fatto ed è ora di andare avanti.

## 2D canvas basics

Come detto sopra, tutte le operazioni di disegno sono fatte manipolando un oggetto [`CanvasRenderingContext2D`](/it/docs/Web/API/CanvasRenderingContext2D) (nel nostro caso, `ctx`). Molte operazioni devono essere fornite di coordinate per individuare esattamente dove disegnare qualcosa — il punto in alto a sinistra del `canvas` è il punto (0, 0), l'asse orizzontale (x) si estende da sinistra a destra, e l'asse verticale (y) si estende dall'alto al basso.

![Carta millimetrata con piccoli quadrati che coprono l'area con un quadrato acciaio blu al centro. L'angolo in alto a sinistra del canvas è il punto (0, 0) dell'asse x e y del canvas. L'asse orizzontale (x) si estende da sinistra a destra indicando la larghezza, e l'asse verticale (y) si estende dall'alto al basso indicando l'altezza. L'angolo in alto a sinistra del quadrato blu è etichettato come distante x unità dall'asse y e y unità dall'asse x.](canvas_default_grid.png)

Disegnare forme tende ad essere fatto usando il primitivo forma rettangolare, o tracciando una linea lungo un certo percorso e poi riempiendo la forma. Di seguito mostreremo come fare entrambi.

### Rettangoli semplici

Iniziamo con alcuni rettangoli semplici.

1. Prima di tutto, faccia una copia del suo nuovo modello di `canvas` codificato (o faccia una copia locale della directory [1_canvas_template](https://github.com/mdn/learning-area/tree/main/javascript/apis/drawing-graphics/getting-started/1_canvas_template) se non ha seguito i passaggi sopra).
2. Successivamente, aggiunga le seguenti righe alla fine del suo JavaScript:

   ```js
   ctx.fillStyle = "rgb(255 0 0)";
   ctx.fillRect(50, 50, 100, 150);
   ```

   Se salva e aggiorna, dovrebbe vedere un rettangolo rosso apparire sul suo `canvas`. Il suo angolo in alto a sinistra è a 50 pixel dalla parte superiore e sinistra del bordo del `canvas` (come definito dai primi due parametri), ed è largo 100 pixel e alto 150 pixel (come definito dal terzo e quarto parametro).

3. Aggiungiamo un altro rettangolo nel mix — questa volta uno verde. Aggiunga quanto segue in fondo al suo JavaScript:

   ```js
   ctx.fillStyle = "rgb(0 255 0)";
   ctx.fillRect(75, 75, 100, 100);
   ```

   Salvi e aggiorni, e vedrà il suo nuovo rettangolo. Questo solleva un punto importante: le operazioni grafiche come disegnare rettangoli, linee, ecc. sono eseguite nell'ordine in cui si verificano. Pensandoci come dipingere un muro, dove ogni strato di pittura si sovrappone e può anche nascondere ciò che c'è sotto. Non si può fare nulla per cambiare questo, quindi deve pensare attentamente all'ordine in cui disegna i grafici.

4. Nota che può disegnare grafici semi-trasparenti specificando un colore semi-trasparente, per esempio usando `rgb()`. Il "canale alfa" definisce la quantità di trasparenza del colore. Più alto è il suo valore, più coprirà ciò che c'è dietro. Aggiunga quanto segue al suo codice:

   ```js
   ctx.fillStyle = "rgb(255 0 255 / 75%)";
   ctx.fillRect(25, 100, 175, 50);
   ```

5. Ora provi a disegnare altri rettangoli per conto suo; si diverta!

### Contorni e larghezze di linea

Finora abbiamo visto come disegnare rettangoli pieni, ma può anche disegnare rettangoli che sono solo contorni (chiamati **strokes** nel design grafico). Per impostare il colore desiderato per il suo contorno, usa la proprietà [`strokeStyle`](/it/docs/Web/API/CanvasRenderingContext2D/strokeStyle); disegnare un rettangolo con contorno viene fatto usando [`strokeRect`](/it/docs/Web/API/CanvasRenderingContext2D/strokeRect).

1. Aggiunga quanto segue all'esempio precedente, sempre sotto le precedenti righe JavaScript:

   ```js
   ctx.strokeStyle = "rgb(255 255 255)";
   ctx.strokeRect(25, 25, 175, 200);
   ```

2. La larghezza predefinita dei contorni è di 1 pixel; può regolare il valore della proprietà [`lineWidth`](/it/docs/Web/API/CanvasRenderingContext2D/lineWidth) per cambiarlo (prende un numero che rappresenta il numero di pixel di larghezza del contorno). Aggiunga la seguente riga tra le due precedenti righe:

   ```js
   ctx.lineWidth = 5;
   ```

Ora dovrebbe vedere che il suo contorno bianco è diventato molto più spesso! Questo è tutto per ora. A questo punto il suo esempio dovrebbe apparire così:

{{EmbedGHLiveSample("learning-area/javascript/apis/drawing-graphics/getting-started/2_canvas_rectangles/index.html", '100%', 250)}}

> [!NOTE]
> Il codice finito è disponibile su GitHub come [2_canvas_rectangles](https://github.com/mdn/learning-area/tree/main/javascript/apis/drawing-graphics/getting-started/2_canvas_rectangles).

### Disegnare percorsi

Se desidera disegnare qualcosa di più complesso di un rettangolo, deve disegnare un percorso. Praticamente, ciò implica scrivere del codice per specificare esattamente quale percorso il penna dovrebbe percorrere sul suo `canvas` per tracciare la forma che desidera disegnare. Canvas include funzioni per disegnare linee rette, cerchi, curve di Bézier, e altro.

Iniziamo questa sezione facendo una copia fresca del nostro modello di `canvas` ([1_canvas_template](https://github.com/mdn/learning-area/tree/main/javascript/apis/drawing-graphics/getting-started/1_canvas_template)), su cui disegnare il nuovo esempio.

Useremo alcuni metodi e proprietà comuni in tutte le sezioni seguenti:

- [`beginPath()`](/it/docs/Web/API/CanvasRenderingContext2D/beginPath) — inizia a disegnare un percorso nel punto in cui la penna si trova attualmente sul `canvas`. Su un nuovo `canvas`, la penna inizia su (0, 0).
- [`moveTo()`](/it/docs/Web/API/CanvasRenderingContext2D/moveTo) — sposta la penna su un diverso punto sul `canvas`, senza registrare o tracciare la linea; la penna "salta" alla nuova posizione.
- [`fill()`](/it/docs/Web/API/CanvasRenderingContext2D/fill) — disegna una forma riempita riempiendo il percorso tracciato finora.
- [`stroke()`](/it/docs/Web/API/CanvasRenderingContext2D/stroke) — disegna una forma di contorno disegnando un contorno lungo il percorso tracciato finora.
- Potrà anche usare funzionalità come `lineWidth` e `fillStyle`/`strokeStyle` con i percorsi oltre ai rettangoli.

Una tipica, semplice operazione di disegno di percorsi apparirebbe più o meno così:

```js
ctx.fillStyle = "rgb(255 0 0)";
ctx.beginPath();
ctx.moveTo(50, 50);
// draw your path
ctx.fill();
```

#### Disegnare linee

Disegneremo un triangolo equilatero sul `canvas`.

1. Prima di tutto, aggiunga la seguente funzione di aiuto alla fine del suo codice. Questa converte i valori in gradi in radianti, il che è utile perché ogni volta che deve fornire un valore d'angolo in JavaScript, sarà quasi sempre in radianti, ma gli umani di solito pensano in gradi.

   ```js
   function degToRad(degrees) {
     return (degrees * Math.PI) / 180;
   }
   ```

2. Ora inizi il suo percorso aggiungendo quanto segue sotto la sua precedente aggiunta; qui impostiamo un colore per il nostro triangolo, iniziamo a disegnare un percorso, e poi spostiamo la penna su (50, 50) senza disegnare nulla. È qui che inizieremo a disegnare il nostro triangolo.

   ```js
   ctx.fillStyle = "rgb(255 0 0)";
   ctx.beginPath();
   ctx.moveTo(50, 50);
   ```

3. Ora aggiunga le seguenti linee alla fine del suo script:

   ```js
   ctx.lineTo(150, 50);
   const triHeight = 50 * Math.tan(degToRad(60));
   ctx.lineTo(100, 50 + triHeight);
   ctx.lineTo(50, 50);
   ctx.fill();
   ```

   Vediamo questo in ordine:

   Prima disegniamo una linea fino a (150, 50) — il nostro percorso ora va 100 pixel a destra lungo l'asse x.

   In secondo luogo, calcoliamo l'altezza del nostro triangolo equilatero, usando un po' di trigonometria di base. Praticamente, stiamo disegnando il triangolo puntando verso il basso. Gli angoli in un triangolo equilatero sono sempre di 60 gradi; per calcolare l'altezza possiamo dividerlo a metà in due triangoli rettangoli, che avranno ciascuno angoli di 90 gradi, 60 gradi, e 30 gradi. In termini di lati:

   - Il lato più lungo è chiamato **ipotenusa**
   - Il lato accanto all'angolo di 60 gradi è chiamato **adiacente** — che sappiamo essere 50 pixel, poiché è la metà della linea appena tracciata.
   - Il lato opposto all'angolo di 60 gradi è chiamato **opposto**, che è l'altezza del triangolo che vogliamo calcolare.

   ![Un triangolo equilatero orientato verso il basso con angoli e lati etichettati. La linea orizzontale in alto è etichettata 'adiacente'. Una linea tratteggiata perpendicolare, dal centro della linea adiacente, etichettata 'opposto', divide il triangolo creando due triangoli rettangoli uguali. Il lato destro del triangolo è etichettato 'ipotenusa', poiché è l'ipotenusa del triangolo rettangolo formato dalla linea etichettata 'opposto'. mentre tutti e tre i lati del triangolo sono della stessa lunghezza, l'ipotenusa è il lato più lungo del triangolo rettangolo.](trigonometry.png)

   Una delle formule trigonometriche di base afferma che la lunghezza dell'adiacente moltiplicata per il tangente dell'angolo è uguale all'opposto, quindi otteniamo `50 * Math.tan(degToRad(60))`. Usiamo la nostra funzione `degToRad()` per convertire 60 gradi in radianti, poiché {{jsxref("Math.tan()")}} si aspetta un valore di input in radianti.

4. Con l'altezza calcolata, disegniamo un'altra linea fino a `(100, 50 + triHeight)`. La coordinata X è semplice; deve essere a metà strada tra i precedenti due valori X impostati. Il valore Y invece deve essere 50 più l'altezza del triangolo, poiché sappiamo che la parte superiore del triangolo è a 50 pixel dalla parte superiore del `canvas`.
5. La linea successiva traccia una linea fino al punto di partenza del triangolo.
6. Infine, eseguiamo `ctx.fill()` per terminare il percorso e riempire la forma.

#### Disegnare cerchi

Ora vediamo come disegnare un cerchio su `canvas`. Questo viene eseguito usando il metodo [`arc()`](/it/docs/Web/API/CanvasRenderingContext2D/arc), che disegna tutto o parte di un cerchio in un punto specificato.

1. Aggiungiamo un arco al nostro `canvas` — aggiunga il seguente alla fine del suo codice:

   ```js
   ctx.fillStyle = "rgb(0 0 255)";
   ctx.beginPath();
   ctx.arc(150, 106, 50, degToRad(0), degToRad(360), false);
   ctx.fill();
   ```

   `arc()` prende sei parametri. I primi due specificano la posizione del centro dell'arco (X e Y, rispettivamente). Il terzo è il raggio del cerchio, il quarto e il quinto sono gli angoli iniziale e finale in cui disegnare il cerchio (quindi specificando 0 e 360 gradi otteniamo un cerchio completo), e il sesto parametro definisce se il cerchio dovrebbe essere disegnato in senso antiorario (`false` è orario).

   > [!NOTE]
   > 0 gradi è orizzontalmente a destra.

2. Aggiungiamo un altro arco:

   ```js
   ctx.fillStyle = "yellow";
   ctx.beginPath();
   ctx.arc(200, 106, 50, degToRad(-45), degToRad(45), true);
   ctx.lineTo(200, 106);
   ctx.fill();
   ```

   Il modello qui è molto simile, ma con due differenze:

   - Abbiamo impostato l'ultimo parametro di `arc()` su `true`, il che significa che l'arco è disegnato in senso antiorario, il che significa che anche se l'arco è specificato come iniziando a -45 gradi e terminando a 45 gradi, disegniamo l'arco attorno ai 270 gradi non all'interno di questa porzione. Se dovesse cambiare `true` con `false` e poi eseguire di nuovo il codice, solo il segmento di 90 gradi del cerchio verrebbe disegnato.
   - Prima di chiamare `fill()`, tracciamo una linea fino al centro del cerchio. Ciò significa che otteniamo il piuttosto piacevole ritaglio in stile Pac-Man. Se rimuovesse questa linea (lo provi!) quindi ripetesse il codice, otterrebbe solo un bordo del cerchio ritagliato tra il punto iniziale e finale dell'arco. Ciò illustra un altro punto importante del `canvas` — se si tenta di riempire un percorso incompleto (cioè uno che non è chiuso), il browser riempie una linea dritta tra il punto iniziale e finale e quindi lo riempie.

Questo è tutto per ora; il suo esempio finale dovrebbe apparire così:

{{EmbedGHLiveSample("learning-area/javascript/apis/drawing-graphics/getting-started/3_canvas_paths/index.html", '100%', 200)}}

> [!NOTE]
> Il codice finito è disponibile su GitHub come [3_canvas_paths](https://github.com/mdn/learning-area/tree/main/javascript/apis/drawing-graphics/getting-started/3_canvas_paths).

> [!NOTE]
> Per saperne di più sulle funzionalità avanzate di disegno di percorsi come le curve di Bézier, dia un'occhiata al nostro tutorial [Disegnare forme con canvas](/it/docs/Web/API/Canvas_API/Tutorial/Drawing_shapes).

### Testo

Canvas ha anche funzionalità per disegnare testo. Esploriamole brevemente. Inizi con fare un'altra copia fresca del nostro modello di `canvas` ([1_canvas_template](https://github.com/mdn/learning-area/tree/main/javascript/apis/drawing-graphics/getting-started/1_canvas_template)) su cui disegnare il nuovo esempio.

Il testo viene disegnato usando due metodi:

- [`fillText()`](/it/docs/Web/API/CanvasRenderingContext2D/fillText) — disegna testo riempito.
- [`strokeText()`](/it/docs/Web/API/CanvasRenderingContext2D/strokeText) — disegna testo contornato (stroke).

Entrambi questi prendono tre proprietà nel loro utilizzo di base: la stringa di testo da disegnare e le coordinate X e Y del punto in cui iniziare a disegnare il testo. Ciò risulta essere l'angolo inferiore sinistro del box di testo (letteralmente, il box che circonda il testo ti disegni), che potrebbe confonderla dato che altre operazioni di disegno tendono a iniziare dall'angolo superiore sinistro — tenga questo a mente.

Ci sono anche un certo numero di proprietà per aiutare a controllare il rendering del testo come [`font`](/it/docs/Web/API/CanvasRenderingContext2D/font), che le permette di specificare la famiglia di font, la dimensione, ecc. Prende come valore la stessa sintassi della proprietà CSS {{cssxref("font")}}.

Il contenuto del `canvas` non è accessibile agli screen reader. Il testo dipinto sul `canvas` non è disponibile al DOM, ma deve essere reso disponibile per essere accessibile. In questo esempio, includiamo il testo come valore per `aria-label`.

Provi ad aggiungere il seguente blocco alla fine del suo JavaScript:

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

Qui disegniamo due linee di testo, una contornata e l'altra stroke. L'esempio finale dovrebbe apparire così:

{{EmbedGHLiveSample("learning-area/javascript/apis/drawing-graphics/getting-started/4_canvas_text/index.html", '100%', 180)}}

> [!NOTE]
> Il codice finito è disponibile su GitHub come [4_canvas_text](https://github.com/mdn/learning-area/tree/main/javascript/apis/drawing-graphics/getting-started/4_canvas_text).

Si diverta a vedere cosa può inventarsi! Può trovare ulteriori informazioni sulle opzioni disponibili per il testo su `canvas` in [Disegnare testo](/it/docs/Web/API/Canvas_API/Tutorial/Drawing_text).

### Disegnare immagini su canvas

È possibile rendere sul suo `canvas` immagini esterne. Queste possono essere semplici immagini, fotogrammi di video, o il contenuto di altri `canvas`. Al momento osserveremo solo il caso di usare alcune semplici immagini sul nostro `canvas`.

1. Come prima, faccia un'altra nuova copia del nostro modello di `canvas` ([1_canvas_template](https://github.com/mdn/learning-area/tree/main/javascript/apis/drawing-graphics/getting-started/1_canvas_template)) su cui disegnare il nuovo esempio.

   Le immagini vengono disegnate su `canvas` usando il metodo [`drawImage()`](/it/docs/Web/API/CanvasRenderingContext2D/drawImage). La versione più semplice prende tre parametri — un riferimento all'immagine che si vuole visualizzare, e le coordinate X e Y dell'angolo in alto a sinistra dell'immagine.

2. Iniziamo ottenere una fonte immagine da incorporare nel nostro `canvas`. Aggiunga le seguenti righe alla fine del suo JavaScript:

   ```js
   const image = new Image();
   image.src = "firefox.png";
   ```

   Qui creiamo un nuovo oggetto [`HTMLImageElement`](/it/docs/Web/API/HTMLImageElement) usando il costruttore [`Image()`](/it/docs/Web/API/HTMLImageElement/Image). L'oggetto restituito è dello stesso tipo di quello che viene restituito quando si ottiene un riferimento ad un elemento esistente {{htmlelement("img")}}. Poi impostiamo il suo attributo [`src`](/it/docs/Web/HTML/Reference/Elements/img#src) per essere uguale alla nostra immagine del logo di Firefox. A questo punto, il browser inizia a caricare l'immagine.

3. Potremmo ora provare a incorporare l'immagine usando `drawImage()`, ma dobbiamo assicurarci che il file immagine sia stato caricato prima, altrimenti il codice fallirà. Possiamo ottenere ciò usando l'evento `load`, che verrà attivato solo quando l'immagine avrà finito di caricarsi. Aggiunga il seguente blocco sotto quello precedente:

   ```js
   image.addEventListener("load", () => ctx.drawImage(image, 20, 20));
   ```

   Se carica il suo esempio nel browser ora, dovrebbe vedere l'immagine incorporata nel `canvas`.

4. Ma c'è di più! E se volessimo visualizzare solo una parte dell'immagine, o ridimensionarla? Possiamo fare entrambe le cose con la versione più complessa di `drawImage()`. Aggiorni la sua riga `ctx.drawImage()` in questo modo:

   ```js
   ctx.drawImage(image, 20, 20, 185, 175, 50, 50, 185, 175);
   ```

   - Il primo parametro è il riferimento all'immagine, come prima.
   - I parametri 2 e 3 definiscono le coordinate dell'angolo in alto a sinistra dell'area che si desidera ritagliare dall'immagine caricata, relative all'angolo in alto a sinistra dell'immagine stessa. Niente a sinistra del primo parametro o sopra il secondo sarà disegnato.
   - I parametri 4 e 5 definiscono la larghezza e l'altezza dell'area che vogliamo ritagliare dall'immagine originale caricata.
   - I parametri 6 e 7 definiscono le coordinate in cui si vuole disegnare l'angolo in alto a sinistra della porzione ritagliata dell'immagine, relative all'angolo in alto a sinistra del `canvas`.
   - I parametri 8 e 9 definiscono la larghezza e l'altezza su cui disegnare l'area ritagliata dell'immagine. In questo caso, abbiamo specificato le stesse dimensioni della fetta originale, ma potrebbe ridimensionarlo specificando valori diversi.

5. Quando l'immagine viene significativamente aggiornata, la {{Glossary("accessible_description", "descrizione accessibile")}} deve anche essere aggiornata.

   ```js
   canvas.setAttribute("aria-label", "Firefox Logo");
   ```

L'esempio finale dovrebbe apparire così:

{{EmbedGHLiveSample("learning-area/javascript/apis/drawing-graphics/getting-started/5_canvas_images/index.html", '100%', 260)}}

> [!NOTE]
> Il codice finito è disponibile su GitHub come [5_canvas_images](https://github.com/mdn/learning-area/tree/main/javascript/apis/drawing-graphics/getting-started/5_canvas_images).

## Cicli e animazioni

Finora abbiamo coperto alcuni utilizzi molto basilari di `canvas` 2D, ma veramente non sperimenterà tutta la potenza del `canvas` a meno che non lo aggiorni o animi in qualche modo. Dopotutto, il `canvas` fornisce immagini scriptabili! Se non ha intenzione di cambiare nulla, allora potrebbe altrettanto bene usare immagini statiche e risparmiarsi tutto il lavoro.

### Creare un ciclo

Giocare con cicli in `canvas` è piuttosto divertente — può eseguire comandi `canvas` all'interno di un ciclo [`for`](/it/docs/Web/JavaScript/Reference/Statements/for) (o altro tipo di) proprio come qualsiasi altro codice JavaScript.

Costruiamo un esempio.

1. Faccia un'altra nuova copia del nostro modello di `canvas` ([1_canvas_template](https://github.com/mdn/learning-area/tree/main/javascript/apis/drawing-graphics/getting-started/1_canvas_template)) e lo apra nel suo editor di codice.
2. Aggiunga la seguente riga alla fine del suo JavaScript. Questa contiene un nuovo metodo, [`translate()`](/it/docs/Web/API/CanvasRenderingContext2D/translate), che sposta il punto di origine del `canvas`:

   ```js
   ctx.translate(width / 2, height / 2);
   ```

   Questo sposta l'origine delle coordinate (0, 0) verso il centro del `canvas`, piuttosto che essere nell'angolo superiore sinistro. Questo è molto utile in molte situazioni, come questa, in cui vogliamo che il nostro design sia disegnato rispetto al centro del `canvas`.

3. Ora aggiunga il seguente codice alla fine del JavaScript:

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

   Qui stiamo implementando la stessa funzione `degToRad()` che abbiamo visto nell'esempio del triangolo sopra, una funzione `rand()` che restituisce un numero casuale tra confini inferiori e superiori dati, variabili `length` e `moveOffset` (scopriremo di più su di esse più tardi), e un ciclo `for` vuoto.

4. L'idea qui è che disegneremo qualcosa sul `canvas` all'interno del ciclo `for`, e itereremo su di esso ogni volta per creare qualcosa di interessante. Aggiunga il seguente codice all'interno del suo ciclo `for`:

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

   Quindi, ad ogni iterazione, noi:

   - Imposta il `filStyle` per essere una sfumatura di viola leggermente trasparente, che cambia ogni volta in base al valore di `length`. Come vedrà più tardi la lunghezza diventa più piccola ogni volta che il ciclo viene eseguito, quindi l'effetto qui è che il colore diventa più luminoso ad ogni triangolo disegnato.
   - Inizia il percorso.
   - Sposta la penna su una coordinata di `(moveOffset, moveOffset)`; Questa variabile definisce di quanto vogliamo spostarci ogni volta che disegniamo un nuovo triangolo.
   - Disegna una linea su una coordinata di `(moveOffset+length, moveOffset)`. Questo disegna una linea di lunghezza `length` parallela all'asse X.
   - Calcola l'altezza del triangolo, come prima.
   - Disegna una linea fino all'angolo in basso del triangolo, quindi disegna una linea di nuovo al punto di partenza del triangolo.
   - Chiama `fill()` per riempire il triangolo.
   - Aggiorniamo le variabili che descrivono la sequenza di triangoli in modo da essere pronti a disegnare il prossimo. Decrementiamo il valore `length` di 1, quindi i triangoli diventano più piccoli ogni volta; aumentiamo `moveOffset` di una piccola quantità in modo che ogni successivo triangolo sia leggermente più lontano, e usiamo un'altra nuova funzione, [`rotate()`](/it/docs/Web/API/CanvasRenderingContext2D/rotate), che ci permette di ruotare l'intero `canvas`! Lo ruotiamo di 5 gradi prima di disegnare il prossimo triangolo.

È tutto! L'esempio finale dovrebbe apparire così:

{{EmbedGHLiveSample("learning-area/javascript/apis/drawing-graphics/loops_animation/6_canvas_for_loop/index.html", '100%', 550)}}

A questo punto, ci piacerebbe incoraggiarla a giocare con l'esempio e personalizzarlo! Per esempio:

- Disegna rettangoli o archi invece di triangoli, o anche incorpora immagini.
- Giochi con i valori `length` e `moveOffset`.
- Introduca alcuni numeri casuali usando quella funzione `rand()` che abbiamo incluso sopra ma non usato.

> [!NOTE]
> Il codice finito è disponibile su GitHub come [6_canvas_for_loop](https://github.com/mdn/learning-area/tree/main/javascript/apis/drawing-graphics/loops_animation/6_canvas_for_loop).

### Animazioni

L'esempio di ciclo che abbiamo costruito sopra era divertente, ma in realtà è necessario un ciclo costante che continui e continui per qualsiasi applicazione seria di `canvas` (come giochi e visualizzazioni in tempo reale). Se pensa al suo `canvas` come a un film, vuole davvero che il display si aggiorni a ogni fotogramma per mostrare la vista aggiornata, con un aggiornamento ideale di 60 fotogrammi al secondo in modo che il movimento appaia bello e fluido all'occhio umano.

Ci sono alcune funzioni JavaScript che le permetteranno di eseguire funzioni ripetutamente, diverse volte al secondo, la migliore per i nostri scopi qui è [`window.requestAnimationFrame()`](/it/docs/Web/API/Window/requestAnimationFrame). Prende un parametro: il nome della funzione che si desidera eseguire per ogni fotogramma. La prossima volta che il browser è pronto ad aggiornare lo schermo, la sua funzione verrà chiamata. Se quella funzione disegna il nuovo aggiornamento alla sua animazione, quindi chiama di nuovo `requestAnimationFrame()` appena prima della fine della funzione, il ciclo di animazione continuerà a funzionare. Il ciclo termina quando smette di chiamare `requestAnimationFrame()` o se chiama [`window.cancelAnimationFrame()`](/it/docs/Web/API/Window/cancelAnimationFrame) dopo aver chiamato `requestAnimationFrame()` ma prima che il frame venga chiamato.

> [!NOTE]
> È buona pratica chiamare `cancelAnimationFrame()` dal suo codice principale quando ha finito di usare l'animazione, per assicurarsi che nessun aggiornamento sia ancora in attesa di essere eseguito.

Il browser elabora dettagli complessi come fare in modo che l'animazione venga eseguita a una velocità costante, e non sprecare risorse per animare cose che non possono essere viste.

Per vedere come funziona, diamo un'altra occhiata al nostro esempio delle Palle Rimbalzanti ([vedi live](https://mdn.github.io/learning-area/javascript/oojs/bouncing-balls/index-finished.html), e vedi anche [il codice sorgente](https://github.com/mdn/learning-area/tree/main/javascript/oojs/bouncing-balls)). Il codice per il ciclo che tiene tutto in movimento appare così:

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

Eseguiamo la funzione `loop()` una volta in fondo al codice per iniziare il ciclo, disegnando il primo frame dell'animazione; la funzione `loop()` poi si incarica di chiamare `requestAnimationFrame(loop)` per eseguire il prossimo frame dell'animazione, ancora e ancora.

Nota che a ogni frame stiamo cancellando completamente il `canvas` e ridisegnando tutto. Per ogni pallina presente, la disegniamo, aggiorniamo la sua posizione, e verifichiamo se sta collidendo con altre palline. Una volta che ha disegnato un grafico su un `canvas`, non c'è modo di manipolare quel grafico individualmente come può fare con elementi del DOM. Non può spostare ogni pallina intorno sul `canvas`, perché una volta che è disegnata, fa parte del `canvas`, e non è un elemento o oggetto accessibile individualmente. Invece, ha bisogno di cancellare e ridisegnare, o cancellando l'intero frame e ridisegnando tutto, o avendo un codice che sa esattamente quali parti devono essere cancellate e solo cancella e ridisegna la minima area del `canvas` necessaria.

Ottimizzare l'animazione di grafici è una specialità della programmazione, con molti tecniche intelligenti disponibili. Quelle sono al di là di ciò che necessitiamo per il nostro esempio, comunque!

In generale, il processo di fare un'animazione su `canvas` coinvolge i seguenti passi:

1. Cancellare il contenuto del `canvas` (ad es., con [`fillRect()`](/it/docs/Web/API/CanvasRenderingContext2D/fillRect) o [`clearRect()`](/it/docs/Web/API/CanvasRenderingContext2D/clearRect)).
2. Salvare lo stato (se necessario) usando [`save()`](/it/docs/Web/API/CanvasRenderingContext2D/save) — questo è necessario quando desidera salvare le impostazioni che ha aggiornato sul `canvas` prima di continuare, il che è utile per applicazioni più avanzate.
3. Disegnare i grafici che sta animando.
4. Ripristinare le impostazioni salvate nel passo 2, usando [`restore()`](/it/docs/Web/API/CanvasRenderingContext2D/restore)
5. Chiamare `requestAnimationFrame()` per programmare il disegno del prossimo frame dell'animazione.

> [!NOTE]
> Non faremo copertura di `save()` e `restore()` qui, ma sono spiegati bene nel nostro tutorial [Trasformazioni](/it/docs/Web/API/Canvas_API/Tutorial/Transformations) (e quelli che lo seguono).

### Una semplice animazione di caratteri

Ora creiamo la nostra semplice animazione — faremo attraversare lo schermo a un personaggio di un certo gioco per computer rétro piuttosto fantastico.

1. Faccia un'altra nuova copia del nostro modello di `canvas` ([1_canvas_template](https://github.com/mdn/learning-area/tree/main/javascript/apis/drawing-graphics/getting-started/1_canvas_template)) e lo apra nel suo editor di codice.

2. Aggiorni l'HTML interno per riflettere l'immagine:

   ```html
   <canvas class="myCanvas">
     <p>A man walking.</p>
   </canvas>
   ```

3. Alla fine del JavaScript, aggiunga la seguente riga per far nuovamente posizionare l'origine delle coordinate nel mezzo del `canvas`:

   ```js
   ctx.translate(width / 2, height / 2);
   ```

4. Ora creiamo un nuovo oggetto [`HTMLImageElement`](/it/docs/Web/API/HTMLImageElement), impostiamo il suo [`src`](/it/docs/Web/HTML/Reference/Elements/img#src) all'immagine che vogliamo caricare, e aggiungiamo un gestore di eventi `onload` che farà in modo che la funzione `draw()` si attivi quando l'immagine è caricata:

   ```js
   const image = new Image();
   image.src = "walk-right.png";
   image.onload = draw;
   ```

5. Ora aggiungeremo alcune variabili per tenere traccia della posizione in cui lo sprite deve essere disegnato sullo schermo, e del numero dello sprite che vogliamo visualizzare.

   ```js
   let sprite = 0;
   let posX = 0;
   ```

   Spieghiamo l'immagine sprite (che abbiamo preso rispettosamente in prestito dal CodePen di Mike Thomas [Walking cycle usando CSS animation](https://codepen.io/mikethomas/pen/kQjKLW)). L'immagine appare così:

   ![Una sprite sheet con sei immagini sprite di un personaggio pixelato che assomiglia a una persona che cammina dal loro lato destro in diversi istanti di un singolo passo avanti. Il personaggio ha una maglietta bianca con bottoni celesti, pantaloni neri e scarpe nere. Ogni sprite è largo 102 pixel e alto 148 pixel.](walk-right.png)

   Contiene sei sprite che compongono l'intera sequenza di camminata — ciascuno è largo 102 pixel e alto 148 pixel. Per visualizzare ciascun sprite pulito dovremo usare `drawImage()` per tagliare un'unica immagine sprite dal `spritesheet` e visualizzare solo quella parte, come abbiamo fatto sopra col logo di Firefox. La coordinata X della fetta dovrà essere un multiplo di 102, e la coordinata Y sarà sempre 0. La dimensione della fetta sarà sempre 102 per 148 pixel.

6. Ora inseriamo una funzione `draw()` vuota alla fine del codice, pronta per essere riempita con del codice:

   ```js
   function draw() {}
   ```

7. Il resto del codice in questa sezione va all'interno di `draw()`. Per prima cosa, aggiunga la seguente riga, che cancella il `canvas` per prepararlo a disegnare ogni frame. Noti che dobbiamo specificare l'angolo in alto a sinistra del rettangolo come `-(width/2), -(height/2)` perché abbiamo specificato la posizione dell'origine come `width/2, height/2` in precedenza.

   ```js
   ctx.fillRect(-(width / 2), -(height / 2), width, height);
   ```

8. Successivamente, disegneremo la nostra immagine usando `drawImage` — la versione a 9 parametri. Aggiunga quanto segue:

   ```js
   ctx.drawImage(image, sprite * 102, 0, 102, 148, 0 + posX, -74, 102, 148);
   ```

   Come può vedere:

   - Specifichiamo `image` come immagine da incorporare.
   - I parametri 2 e 3 specificano l'angolo in alto a sinistra della fetta da ritagliare dall'immagine sorgente, con il valore X come `sprite` moltiplicato per 102 (dove `sprite` è il numero dello sprite tra 0 e 5) e il valore Y sempre 0.
   - I parametri 4 e 5 specificano le dimensioni della fetta da ritagliare — 102 pixel per 148 pixel.
   - I parametri 6 e 7 specificano l'angolo in alto a sinistra della casella in cui disegnare la fetta sul `canvas` — la posizione X è 0 + `posX`, il che significa che possiamo alterare la posizione del disegno alterando il valore `posX`.
   - I parametri 8 e 9 specificano le dimensioni dell'immagine sul `canvas`. Vogliamo solo mantenerne le dimensioni originale, quindi specifichiamo 102 e 148 come larghezza e altezza.

9. Ora altereremo il valore `sprite` dopo ogni disegno — beh, dopo alcuni di loro comunque. Aggiunga il seguente blocco alla fine della funzione `draw()`:

   ```js
   if (posX % 13 === 0) {
     if (sprite === 5) {
       sprite = 0;
     } else {
       sprite++;
     }
   }
   ```

   Stiamo racchiudendo l'intero blocco in `if (posX % 13 === 0) { }`. Usamo l'operatore modulo (`%`) (noto anche come [operatore di resto](/it/docs/Web/JavaScript/Reference/Operators/Remainder)) per controllare se il valore `posX` può essere esattamente diviso per 13 senza resto. Se sì, passiamo al prossimo sprite incrementando `sprite` (tornando a 0 dopo che abbiamo terminato con lo sprite #5). Questo significa effettivamente che stiamo aggiornando solo lo sprite su ogni tredicesimo frame, o approssimativamente circa 5 frame al secondo (`requestAnimationFrame()` ci chiama a fino 60 frame al secondo se possibile). Stiamo rallentando deliberatamente la velocità di fotogramma perché abbiamo solo sei sprite con cui lavorare, e se ne visualizziamo uno ogni 60esimo di secondo, il nostro personaggio si muoverà troppo velocemente!

   All'interno del blocco più esterno usiamo un'istruzione [`if...else`](/it/docs/Web/JavaScript/Reference/Statements/if...else) per controllare se il valore `sprite` è a 5 (lo sprite finale, dato che i numeri degli sprite vanno da 0 a 5). Se stiamo già mostrando l'ultimo sprite, ripristiniamo `sprite` a 0; se no, lo incrementiamo semplicemente di 1.

10. Successivamente dobbiamo capire come cambiare il valore `posX` su ogni frame — aggiunga il seguente blocco di codice appena sotto il suo ultimo.

    ```js
    if (posX > width / 2) {
      let newStartPos = -(width / 2 + 102);
      posX = Math.ceil(newStartPos);
      console.log(posX);
    } else {
      posX += 2;
    }
    ```

    Stiamo usando un altro `if...else` per vedere se il valore di `posX` è diventato maggiore di `width/2`, il che significa che il nostro personaggio è uscito dal lato destro dello schermo. Se è così, calcoliamo una posizione che metterebbe il personaggio appena a sinistra del lato sinistro dello schermo.

    Se il nostro personaggio non è ancora uscito dal lato dello schermo, incrementiamo `posX` di 2. Questo lo farà andare un po' a destra la prossima volta che lo disegniamo.

11. Infine, dobbiamo far sì che il ciclo di animazione si ripeta chiamando [`requestAnimationFrame()`](/it/docs/Web/API/Window/requestAnimationFrame) alla fine della funzione `draw()`:

    ```js
    window.requestAnimationFrame(draw);
    ```

Ecco fatto! L'esempio finale dovrebbe apparire così:

{{EmbedGHLiveSample("learning-area/javascript/apis/drawing-graphics/loops_animation/7_canvas_walking_animation/index.html", '100%', 260)}}

> [!NOTE]
> Il codice finito è disponibile su GitHub come [7_canvas_walking_animation](https://github.com/mdn/learning-area/tree/main/javascript/apis/drawing-graphics/loops_animation/7_canvas_walking_animation).

### Una semplice applicazione di disegno

Come esempio finale di animazione, vorremmo mostrarle una semplicissima applicazione di disegno, per illustrare come il ciclo d'animazione può essere combinato con l'input dell'utente (come il movimento del mouse, in questo caso). Non la faremo costruire passo per passo questo; esploreremo solo le parti più interessanti del codice.

L'esempio può essere trovato su GitHub come [8_canvas_drawing_app](https://github.com/mdn/learning-area/tree/main/javascript/apis/drawing-graphics/loops_animation/8_canvas_drawing_app), e può giocarci live sotto:

{{EmbedGHLiveSample("learning-area/javascript/apis/drawing-graphics/loops_animation/8_canvas_drawing_app/index.html", '100%', 600)}}

Vediamo le parti più interessanti. Prima di tutto, teniamo traccia delle coordinate X e Y del mouse e se viene cliccato o no con tre variabili: `curX`, `curY`, e `pressed`. Quando il mouse si muove, eseguiamo una funzione impostata come gestore dell'evento `onmousemove`, che cattura i valori correnti di X e Y. Usiamo anche i gestori degli eventi `onmousedown` e `onmouseup` per cambiare il valore di `pressed` a `true` quando il bottone del mouse è premuto, e di nuovo su `false` quando viene rilasciato.

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

Quando il pulsante "Cancella canvas" viene premuto, eseguiamo una semplice funzione che cancella tutto il `canvas` tornandolo al nero, nello stesso modo in cui abbiamo visto prima:

```js
clearBtn.addEventListener("click", () => {
  ctx.fillStyle = "rgb(0 0 0)";
  ctx.fillRect(0, 0, width, height);
});
```

Il ciclo di disegno è piuttosto semplice questa volta — se `pressed` è `true`, disegniamo un cerchio con uno stile di riempimento uguale al valore nel selettore di colori, e un raggio uguale al valore impostato nell'input range. Dobbiamo disegnare il cerchio 85 pixel sopra il punto da cui lo misuriamo, perché la misura verticale è presa dalla parte superiore della viewport, ma stiamo disegnando il cerchio rispetto alla parte superiore del `canvas`, che inizia sotto la barra degli strumenti alta 85 pixel. Se lo disegnassimo solo con `curY` come coordinata y, apparirebbe 85 pixel più in basso della posizione del mouse.

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

Tutti i tipi di {{htmlelement("input")}} sono ben supportati. Se un browser non supporta un tipo di input, si ripiega su una casella di testo semplice.

## WebGL

È ora il momento di lasciare il 2D alle spalle, e dare un'occhiata veloce al `canvas` 3D. I contenuti 3D su `canvas` vengono specificati usando l'[API WebGL](/it/docs/Web/API/WebGL_API), che è un'API completamente separata dall'API `canvas` 2D, anche se entrambi rendono su elementi {{htmlelement("canvas")}}.

WebGL si basa su {{Glossary("OpenGL", "OpenGL")}} (Open Graphics Library), e le permette di comunicare direttamente con la {{Glossary("GPU", "GPU")}} del computer. In quanto tale, scrivere WebGL grezzo è più vicino ai linguaggi a basso livello come C++ rispetto al normale JavaScript; è piuttosto complesso ma incredibilmente potente.

### Usare una libreria

A causa della sua complessità, la maggior parte delle persone scrive codice grafico 3D usando una libreria JavaScript di terze parti come [Three.js](/it/docs/Games/Techniques/3D_on_the_web/Building_up_a_basic_demo_with_Three.js), [PlayCanvas](/it/docs/Games/Techniques/3D_on_the_web/Building_up_a_basic_demo_with_PlayCanvas), o [Babylon.js](/it/docs/Games/Techniques/3D_on_the_web/Building_up_a_basic_demo_with_Babylon.js). La maggior parte di queste funziona in modo simile, fornendo funzionalità per creare forme primitive e personalizzate, posizionare telecamere di visualizzazione e illuminazione, coprire superfici con texture, e altro. Gestiscono il WebGL per lei, permettendole di lavorare su un livello più alto.

Sì, usarne una di queste significa imparare un'altra nuova API (una di terze parti, in questo caso), ma sono molto più semplici che codificare WebGL grezzo.

### Ricreare il nostro cubo

Vediamo un esempio di come creare qualcosa con una libreria WebGL. Sceglieremo [Three.js](/it/docs/Games/Techniques/3D_on_the_web/Building_up_a_basic_demo_with_Three.js), in quanto è una delle più popolari. In questo tutorial ricreeremo il cubo rotante 3D che abbiamo visto prima.

1. Per cominciare, faccia una copia locale di [threejs-cube/index.html](https://github.com/mdn/learning-area/blob/main/javascript/apis/drawing-graphics/threejs-cube/index.html) in una nuova cartella, quindi salvi una copia di [metal003.png](https://github.com/mdn/learning-area/blob/main/javascript/apis/drawing-graphics/threejs-cube/metal003.png) nella stessa cartella. Questa è l'immagine che useremo come texture di superficie per il cubo più tardi.
2. Successivamente, crei un nuovo file chiamato `script.js`, di nuovo nella stessa cartella di prima.
3. Ora deve avere la libreria Three.js installata. Può seguire i passaggi di impostazione dell'ambiente descritti in [Costruire un demo di base con Three.js](/it/docs/Games/Techniques/3D_on_the_web/Building_up_a_basic_demo_with_Three.js) in modo da far funzionare Three.js come previsto.
4. Ora che abbiamo allegato `three.js` alla nostra pagina, possiamo iniziare a scrivere JavaScript che ne faccia uso all'interno di `script.js`. Cominciamo creando una nuova scena — aggiunga il seguente nel suo file `script.js`:

   ```js
   const scene = new THREE.Scene();
   ```

   Il costruttore [`Scene()`](https://threejs.org/docs/index.html#api/en/scenes/Scene) crea una nuova scena, che rappresenta tutto il mondo 3D che stiamo cercando di visualizzare.

5. Successivamente, abbiamo bisogno di una **camera** in modo da poter vedere la scena. In termini di immagini 3D, la camera rappresenta la posizione di visualizzazione nel mondo. Per creare una camera, aggiunga le seguenti righe dopo:

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

   - Il campo visivo: Quanto è ampia l'area di fronte alla camera che dovrebbe essere visibile sullo schermo, in gradi.
   - Il {{Glossary("aspect_ratio", "rapporto d'aspetto")}}: Di solito, questo è il rapporto della larghezza della scena diviso per l'altezza della scena. Usare un altro valore distorcerà la scena (il che potrebbe essere ciò che vuole, ma di solito non è così).
   - Il piano vicino: Quanto vicini possono essere gli oggetti alla camera prima di smettere di renderizzarli sullo schermo. Pensate a quando avvicina la punta del suo dito alle spazio tra i suoi occhi, alla fine non riesce più a vederlo.
   - Il piano lontano: Quanto lontano sono le cose dalla camera prima che non vengano più renderizzate.

   Impostiamo anche la posizione della camera per essere a 5 unità di distanza sull'asse Z, che, come in CSS, è fuori dallo schermo verso di lei, l'osservatore.

6. Il terzo ingrediente vitale è un renderer. Questo è un oggetto che renderizza una data scena, come visto attraverso una data camera. Creeremo uno per ora usando il costruttore [`WebGLRenderer()`](https://threejs.org/docs/index.html#api/en/renderers/WebGLRenderer), ma non lo useremo finché non sarà più tardi. Aggiunga le seguenti righe dopo:

   ```js
   const renderer = new THREE.WebGLRenderer();
   renderer.setSize(window.innerWidth, window.innerHeight);
   document.body.appendChild(renderer.domElement);
   ```

   La prima riga crea un nuovo renderer, la seconda riga imposta la dimensione a cui il renderer disegnerà la vista della camera, e la terza riga appende l'elemento {{htmlelement("canvas")}} creato dal renderer al {{htmlelement("body")}} del documento. Ora tutto ciò che il renderer disegna verrà visualizzato nella nostra finestra.

7. Successivamente, vogliamo creare il cubo che vedremo sul `canvas`. Aggiunga il seguente blocco di codice alla fine del suo JavaScript:

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

   C'è un po' da assorbire qui, quindi vediamolo per gradi:

   - Per prima cosa, creiamo una variabile globale `cube` in modo da poter accedere al nostro cubo da qualsiasi punto del codice.
   - Successivamente, creiamo un nuovo oggetto [`TextureLoader`](https://threejs.org/docs/index.html#api/en/loaders/TextureLoader), quindi chiamiamo `load()` su di esso. `load()` prende due parametri in questo caso (anche se può prenderne di più): la texture che vogliamo caricare (la nostra PNG), e una funzione che verrà eseguita quando la texture sarà caricata.
   - All'interno di questa funzione usiamo le proprietà dell'oggetto [`texture`](https://threejs.org/docs/index.html#api/en/textures/Texture) per specificare che vogliamo una ripetizione 2 x 2 dell'immagine avvolta su tutti i lati del cubo. Successivamente, creiamo un nuovo oggetto [`BoxGeometry`](https://threejs.org/docs/index.html#api/en/geometries/BoxGeometry) e un nuovo oggetto [`MeshLambertMaterial`](https://threejs.org/docs/index.html#api/en/materials/MeshLambertMaterial), e li uniamo in un [`Mesh`](https://threejs.org/docs/index.html#api/en/objects/Mesh) per creare il nostro cubo. Un oggetto richiede tipicamente una geometria (che forma ha) e un materiale (che aspetto ha la sua superficie).
   - Infine, aggiungiamo il nostro cubo alla scena, quindi chiamiamo la nostra funzione `draw()` per dare il via all'animazione.

8. Prima di arrivare a definire `draw()`, aggiungeremo un paio di luci alla scena, per vivacizzare le cose un po'; aggiunga i seguenti blocchi dopo:

   ```js
   const light = new THREE.AmbientLight("rgb(255 255 255)"); // soft white light
   scene.add(light);

   const spotLight = new THREE.SpotLight("rgb(255 255 255)");
   spotLight.position.set(100, 1000, 1000);
   spotLight.castShadow = true;
   scene.add(spotLight);
   ```

   Un oggetto [`AmbientLight`](https://threejs.org/docs/index.html#api/en/lights/AmbientLight) è un tipo di luce soffusa che illumina un po' l'intera scena, come il sole quando si è fuori. L'oggetto [`SpotLight`](https://threejs.org/docs/index.html#api/en/lights/SpotLight), d'altra parte, è un fascio di luce direzionale, più come una torcia (o un riflettore, in effetti).

9. Infine, aggiungiamo la nostra funzione `draw()` alla fine del codice:

   ```js
   function draw() {
     cube.rotation.x += 0.01;
     cube.rotation.y += 0.01;
     renderer.render(scene, camera);

     requestAnimationFrame(draw);
   }
   ```

   Questo è abbastanza intuitivo; su ogni frame, ruotiamo leggermente il nostro cubo sui suoi assi X e Y, quindi renderizziamo la scena come vista dalla nostra camera, e infine chiamiamo `requestAnimationFrame()` per programmare il disegno del prossimo frame.

Diamo un'altra occhiata a come dovrebbe apparire il prodotto finito:

{{EmbedGHLiveSample("learning-area/javascript/apis/drawing-graphics/threejs-cube/index.html", '100%', 500)}}

Può [trovare il codice finito su GitHub](https://github.com/mdn/learning-area/tree/main/javascript/apis/drawing-graphics/threejs-cube).

> [!NOTE]
> Nel nostro repository GitHub può trovare anche un altro interessante esempio di cubo 3D — [Three.js Video Cube](https://github.com/mdn/learning-area/tree/main/javascript/apis/drawing-graphics/threejs-video-cube) ([vedi anche live](https://mdn.github.io/learning-area/javascript/apis/drawing-graphics/threejs-video-cube/)). Questo usa [`getUserMedia()`](/it/docs/Web/API/MediaDevices/getUserMedia) per prendere un flusso video da una webcam e proiettarlo sul lato del cubo come texture!

## Sommario

A questo punto, dovrebbe avere un'idea utile dei concetti basilari della programmazione grafica usando `Canvas` e `WebGL` e cosa può fare con queste API, oltre ad avere una buona idea di dove andare per ulteriori informazioni. Si diverta!

## Vedi anche

Qui abbiamo coperto solo le basi reali di `canvas` — c'è molto di più da imparare! Gli articoli sotto la porteranno più lontano.

- [Tutorial su Canvas](/it/docs/Web/API/Canvas_API/Tutorial) — Una serie di tutorial molto dettagliati che spiegano cosa dovrebbe sapere sul `canvas` 2D in modo molto più dettagliato rispetto a quanto coperto qui. Lettura essenziale.
- [Tutorial su WebGL](/it/docs/Web/API/WebGL_API/Tutorial) — Una serie che insegna i concetti basilari della programmazione WebGL grezza.
- [Costruire un demo di base con Three.js](/it/docs/Games/Techniques/3D_on_the_web/Building_up_a_basic_demo_with_Three.js) — tutorial base su Three.js. Abbiamo anche guide equivalenti per [PlayCanvas](/it/docs/Games/Techniques/3D_on_the_web/Building_up_a_basic_demo_with_PlayCanvas) o [Babylon.js](/it/docs/Games/Techniques/3D_on_the_web/Building_up_a_basic_demo_with_Babylon.js).
- [Sviluppo di giochi](/it/docs/Games) — la pagina di atterraggio per lo sviluppo di giochi web su MDN. Ci sono alcuni tutorial e tecniche davvero utili disponibili qui relativi ai `canvas` 2D e 3D — veda le opzioni di menu Tecniche e Tutorial.

## Esempi

- [Violent theremin](https://github.com/mdn/webaudio-examples/tree/main/violent-theremin) — Usa il Web Audio API per generare suoni, e `canvas` per generare una bella visualizzazione da accompagnare.
- [Voice change-o-matic](https://github.com/mdn/webaudio-examples/tree/main/voice-change-o-matic) — Usa un `canvas` per visualizzare dati audio in tempo reale dal Web Audio API.

{{PreviousMenuNext("Learn_web_development/Extensions/Client-side_APIs/Video_and_audio_APIs", "Learn_web_development/Extensions/Client-side_APIs/Client-side_storage", "Learn_web_development/Extensions/Client-side_APIs")}}
