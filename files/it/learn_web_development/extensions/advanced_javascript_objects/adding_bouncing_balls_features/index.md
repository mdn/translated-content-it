---
title: "Sfida: Aggiunta di funzionalità alla nostra demo di palline rimbalzanti"
short-title: "Sfida: Funzionalità delle palline rimbalzanti"
slug: Learn_web_development/Extensions/Advanced_JavaScript_objects/Adding_bouncing_balls_features
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenu("Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_building_practice", "Learn_web_development/Extensions/Advanced_JavaScript_objects")}}

In questa sfida, ci aspettiamo che utilizzi la demo delle palline rimbalzanti dell'articolo precedente come punto di partenza e aggiunga alcune nuove ed interessanti funzionalità.

## Punto di partenza

Per iniziare questa sfida, faccia una copia locale di [index-finished.html](https://github.com/mdn/learning-area/blob/main/javascript/oojs/bouncing-balls/index-finished.html), [style.css](https://github.com/mdn/learning-area/blob/main/javascript/oojs/bouncing-balls/style.css), e [main-finished.js](https://github.com/mdn/learning-area/blob/main/javascript/oojs/bouncing-balls/main-finished.js) dal nostro ultimo articolo in una nuova directory nel suo computer locale.

In alternativa, potrebbe usare un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/). Potrebbe incollare HTML, CSS e JavaScript in uno di questi editor online. Se l'editor online che sta usando non ha un pannello JavaScript separato, si senta libero di inserirlo inline in un elemento `<script>` all'interno della pagina HTML.

> [!NOTE]
> Se si blocca, può contattarci attraverso uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Suggerimenti e consigli

Alcuni suggerimenti prima di iniziare.

- Questa sfida è abbastanza difficile. Legga tutte le istruzioni prima di iniziare a programmare e affronti ogni passaggio lentamente e con attenzione.
- Potrebbe essere una buona idea salvare una copia separata della demo dopo aver fatto funzionare ogni fase, in modo da poter fare riferimento a essa se si trova in difficoltà più avanti.

## Breve del progetto

La nostra demo delle palline rimbalzanti è divertente, ma ora vogliamo renderla un po' più interattiva aggiungendo un cerchio malvagio controllato dall'utente, che mangerà le palline se le cattura. Vogliamo anche testare le sue abilità di costruzione di oggetti creando un oggetto generico `Shape()` da cui le nostre palline e il cerchio malvagio possano ereditare. Infine, vogliamo aggiungere un contatore dei punti per tracciare il numero di palline rimaste da catturare.

Lo screenshot seguente le dà un'idea di come dovrebbe apparire il programma finito:

![Screenshot della pagina demo delle palline rimbalzanti. Un cerchio con contorno bianco è visibile oltre alle palline colorate, e il testo "Ball count: 23" è visibile sotto il titolo.](bouncing-evil-circle.png)

Per darle un'idea più chiara, dia un'occhiata all'[esempio finito](https://mdn.github.io/learning-area/javascript/oojs/assessment/) (eviti di sbirciare il codice sorgente!)

## Passi da completare

Le sezioni seguenti descrivono cosa deve fare.

### Creare una classe `Shape`

Innanzitutto, crei una nuova classe `Shape`. Questa ha solo un costruttore. Il costruttore di `Shape` dovrebbe definire le proprietà `x`, `y`, `velX`, e `velY` nello stesso modo in cui il costruttore `Ball()` le definiva originariamente, ma non le proprietà `color` e `size`.

La classe `Ball` dovrebbe derivare da `Shape` usando `extends`. Il costruttore per `Ball` dovrebbe:

- prendere gli stessi argomenti di prima: `x`, `y`, `velX`, `velY`, `size`, e `color`
- chiamare il costruttore `Shape` usando `super()`, passando gli argomenti `x`, `y`, `velX`, e `velY`
- inizializzare le sue proprie proprietà `color` e `size` dai parametri che le vengono dati.

> [!NOTE]
> Assicurarsi di creare la classe `Shape` sopra l'esistente classe `Ball`, altrimenti otterrà un errore come: "Uncaught ReferenceError: Cannot access 'Shape' before initialization."

Il costruttore di `Ball` dovrebbe definire una nuova proprietà chiamata `exists`, che viene usata per tracciare se le palline esistono nel programma (non sono state mangiate dal cerchio malvagio). Questa dovrebbe essere un booleano (`true`/`false`), inizializzata su `true` nel costruttore.

Il metodo `collisionDetect()` della classe `Ball` necessita di un piccolo aggiornamento. Una pallina deve essere considerata per il rilevamento delle collisioni solo se la proprietà `exists` è `true`. Quindi, sostituisca il codice esistente di `collisionDetect()` con il seguente codice:

```js
class Ball {
  // …
  collisionDetect() {
    for (const ball of balls) {
      if (!(this === ball) && ball.exists) {
        const dx = this.x - ball.x;
        const dy = this.y - ball.y;
        const distance = Math.sqrt(dx * dx + dy * dy);

        if (distance < this.size + ball.size) {
          ball.color = this.color = randomRGB();
        }
      }
    }
  }
  // …
}
```

Come discusso sopra, l'unica aggiunta è controllare se la pallina esiste — utilizzando `ball.exists` nel condizionale `if`.

Le definizioni dei metodi `draw()` e `update()` della pallina dovrebbero poter rimanere esattamente le stesse di come erano prima.

A questo punto, provi a ricaricare il codice — dovrebbe funzionare esattamente allo stesso modo di prima, con i nostri oggetti riprogettati.

### Definire EvilCircle

Ora è il momento di incontrare il cattivo — `EvilCircle()`! Il nostro gioco avrà solo un cerchio malvagio, ma lo definiremo comunque usando un costruttore che eredita da `Shape()`, per darle un po' di pratica. Potrebbe voler aggiungere un altro cerchio all'app più avanti che possa essere controllato da un altro giocatore, o avere diversi cerchi malvagi controllati dal computer. Probabilmente non conquisterà il mondo con un solo cerchio malvagio, ma andrà bene per questa sfida.

Crei una definizione per una classe `EvilCircle`. Dovrebbe ereditare da `Shape` usando `extends`.

#### Costruttore di EvilCircle

Il costruttore per `EvilCircle` dovrebbe:

- essere passato solo argomenti `x`, `y`
- passare gli argomenti `x`, `y` alla superclasse `Shape` insieme a valori per `velX` e `velY` hardcoded a 20. Dovrebbe farlo con un codice come `super(x, y, 20, 20);`
- impostare `color` su `white` e `size` su `10`.

Infine, il costruttore dovrebbe impostare il codice che permette all'utente di muovere il cerchio malvagio sullo schermo:

```js
window.addEventListener("keydown", (e) => {
  switch (e.key) {
    case "a":
      this.x -= this.velX;
      break;
    case "d":
      this.x += this.velX;
      break;
    case "w":
      this.y -= this.velY;
      break;
    case "s":
      this.y += this.velY;
      break;
  }
});
```

Questo aggiunge un listener per l'evento `keydown` all'oggetto `window`, in modo che quando viene premuto un tasto, la proprietà [`key`](/it/docs/Web/API/KeyboardEvent/key) dell'oggetto evento viene consultata per vedere quale tasto è stato premuto. Se è uno dei quattro tasti specificati, allora il cerchio malvagio si muoverà a sinistra/destra/su/giù.

### Definire metodi per EvilCircle

La classe `EvilCircle` dovrebbe avere tre metodi, come descritto di seguito.

#### draw()

Questo metodo ha lo stesso scopo del metodo `draw()` per `Ball`: disegna l'istanza dell'oggetto sul canvas. Il metodo `draw()` per `EvilCircle` funzionerà in modo molto simile, quindi può iniziare copiando il metodo `draw()` per `Ball`. Dovrebbe poi apportare le seguenti modifiche:

- Vogliamo che il cerchio malvagio non sia riempito, ma piuttosto abbia solo una linea esterna (stroke). Può ottenerlo aggiornando [`fillStyle`](/it/docs/Web/API/CanvasRenderingContext2D/fillStyle) e [`fill()`](/it/docs/Web/API/CanvasRenderingContext2D/fill) rispettivamente a [`strokeStyle`](/it/docs/Web/API/CanvasRenderingContext2D/strokeStyle) e [`stroke()`](/it/docs/Web/API/CanvasRenderingContext2D/stroke).
- Vogliamo anche rendere lo stroke un po' più spesso, in modo che si possa vedere il cerchio malvagio un po' più facilmente. Questo può essere ottenuto impostando un valore per [`lineWidth`](/it/docs/Web/API/CanvasRenderingContext2D/lineWidth) da qualche parte dopo la chiamata a [`beginPath()`](/it/docs/Web/API/CanvasRenderingContext2D/beginPath) (3 andranno bene).

#### checkBounds()

Questo metodo farà la stessa cosa della prima parte del metodo `update()` per `Ball` — controllare se il cerchio malvagio sta per uscire dal bordo dello schermo e impedirlo. Anche in questo caso, può principalmente copiare il metodo `update()` per `Ball`, ma ci sono alcune modifiche che dovrebbe fare:

- Si sbarazzi delle ultime due righe — non vogliamo aggiornare automaticamente la posizione del cerchio malvagio a ogni frame, perché lo muoveremo in un altro modo, come vedrà sotto.
- All'interno delle dichiarazioni `if ()`, se i test ritornano vero non vogliamo aggiornare `velX`/`velY`; vogliamo invece cambiare il valore di `x`/`y` in modo che il cerchio malvagio venga rimbalzato leggermente sullo schermo. L'aggiunta o la sottrazione (a seconda dei casi) della proprietà `size` del cerchio malvagio avrebbe senso.

#### collisionDetect()

Questo metodo funzionerà in modo molto simile al metodo `collisionDetect()` per `Ball`, quindi può usarne una copia come base di questo nuovo metodo. Ma ci sono alcune differenze:

- Nella dichiarazione `if` esterna, non è più necessario controllare se la pallina corrente nell'iterazione è la stessa della pallina che sta effettuando il controllo — perché non è più una pallina, è il cerchio malvagio! Invece, deve effettuare un test per vedere se la pallina controllata esiste (con quale proprietà potrebbe fare ciò?). Se non esiste, è già stata mangiata dal cerchio malvagio, quindi non c'è bisogno di controllarla di nuovo.
- Nella dichiarazione `if` interna, non vuole più fare in modo che gli oggetti cambino colore quando viene rilevata una collisione — invece, vuole impostare tutte le palline che collidono con il cerchio malvagio in modo che non esistano più (di nuovo, come pensa di farlo?).

### Inserire il cerchio malvagio nel programma

Ora che abbiamo definito il cerchio malvagio, dobbiamo effettivamente farlo apparire nella nostra scena. Per farlo, deve apportare alcune modifiche alla funzione `loop()`.

- Innanzitutto, crei una nuova istanza dell'oggetto cerchio malvagio (specificando i parametri necessari). Deve farlo solo una volta, non a ogni iterazione del ciclo.
- Nel punto in cui scorre attraverso ogni pallina e chiama le funzioni `draw()`, `update()`, e `collisionDetect()` per ciascuna, faccia in modo che queste funzioni siano chiamate solo se la pallina corrente esiste.
- Chiami i metodi `draw()`, `checkBounds()`, e `collisionDetect()` dell'istanza del cerchio malvagio a ogni iterazione del ciclo.

### Implementare il contatore del punteggio

Per implementare il contatore del punteggio, segua questi passi:

1. Nel suo file HTML, aggiunga un elemento {{HTMLElement("p")}} appena sotto l'elemento {{HTMLElement("Heading_Elements", "h1")}} contenente il testo "Ball count: ".
2. Nel suo file CSS, aggiunga la seguente regola in fondo:

   ```css
   p {
     position: absolute;
     margin: 0;
     top: 35px;
     right: 5px;
     color: #aaa;
   }
   ```

3. Nel suo JavaScript, faccia i seguenti aggiornamenti:

   - Crei una variabile che memorizzi un riferimento al paragrafo.
   - Mantenga in qualche modo un conteggio del numero di palline sullo schermo.
   - Incrementi il conteggio e visualizzi il numero aggiornato di palline ogni volta che una pallina viene aggiunta alla scena.
   - Decrementi il conteggio e visualizzi il numero aggiornato di palline ogni volta che il cerchio malvagio mangia una pallina (facendola cessare di esistere).

{{PreviousMenu("Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_building_practice", "Learn_web_development/Extensions/Advanced_JavaScript_objects")}}
