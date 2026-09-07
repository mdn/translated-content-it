---
title: "Sfida: aggiungere funzionalità alla demo delle palline rimbalzanti"
short-title: "Sfida: funzionalità delle palline rimbalzanti"
slug: Learn_web_development/Extensions/Advanced_JavaScript_objects/Adding_bouncing_balls_features
l10n:
  sourceCommit: 2530db14de9ac226cf06f84540fa0101e804ca9b
---

{{PreviousMenu("Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_building_practice", "Learn_web_development/Extensions/Advanced_JavaScript_objects")}}

In questa sfida, occorre usare come punto di partenza la demo delle palline rimbalzanti dell'articolo precedente e aggiungervi nuove e interessanti funzionalità.

## Punto di partenza

Per iniziare questa sfida, creare una copia locale di [index-finished.html](https://github.com/mdn/learning-area/blob/main/javascript/oojs/bouncing-balls/index-finished.html), [style.css](https://github.com/mdn/learning-area/blob/main/javascript/oojs/bouncing-balls/style.css) e [main-finished.js](https://github.com/mdn/learning-area/blob/main/javascript/oojs/bouncing-balls/main-finished.js) del nostro ultimo articolo in una nuova directory sul computer locale.

In alternativa, è possibile usare un editor online come [CodePen](https://codepen.io/) o [JSFiddle](https://jsfiddle.net/).
Se l'editor online in uso non dispone di un pannello JavaScript separato, è possibile inserirlo inline in un elemento `<script>` all'interno della pagina HTML.

> [!NOTE]
> In caso di difficoltà, è possibile contattarci tramite uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Suggerimenti e consigli

Alcune indicazioni prima di iniziare.

- Questa sfida è piuttosto difficile. Leggere tutte le istruzioni prima di iniziare a programmare e affrontare ogni passaggio lentamente e con attenzione.
- Potrebbe essere una buona idea salvare una copia separata della demo dopo aver completato ogni fase, così da potervi fare riferimento in caso di problemi in seguito.

## Descrizione del progetto

La nostra demo delle palline rimbalzanti è divertente, ma ora vogliamo renderla un po' più interattiva aggiungendo un cerchio malvagio controllato dall'utente, che mangerà le palline se le cattura. Vogliamo inoltre mettere alla prova le capacità di creazione degli oggetti creando un oggetto `Shape()` generico da cui possano ereditare le palline e il cerchio malvagio. Infine, vogliamo aggiungere un contatore del punteggio per tenere traccia del numero di palline rimaste da catturare.

La seguente schermata offre un'idea dell'aspetto che dovrebbe avere il programma completato:

![Schermata della pagina demo delle palline rimbalzanti. Oltre alle palline colorate è visibile un cerchio con contorno bianco e sotto il titolo è visibile il testo "Ball count: 23".](bouncing-evil-circle.png)

Per avere un'idea più chiara, osservare l'[esempio completato](https://mdn.github.io/learning-area/javascript/oojs/assessment/) (senza sbirciare il codice sorgente!).

## Passaggi da completare

Le sezioni seguenti descrivono ciò che occorre fare.

### Creare una classe Shape

Prima di tutto, creare una nuova classe `Shape`. Questa contiene solo un costruttore. Il costruttore di `Shape` deve definire le proprietà `x`, `y`, `velX` e `velY` nello stesso modo in cui lo faceva originariamente il costruttore `Ball()`, ma non le proprietà `color` e `size`.

La classe `Ball` deve derivare da `Shape` usando `extends`. Il costruttore di `Ball` deve:

- accettare gli stessi argomenti di prima: `x`, `y`, `velX`, `velY`, `size` e `color`
- chiamare il costruttore di `Shape` usando `super()`, passando gli argomenti `x`, `y`, `velX` e `velY`
- inizializzare le proprie proprietà `color` e `size` a partire dai parametri ricevuti.

> [!NOTE]
> Assicurarsi di creare la classe `Shape` sopra la classe `Ball` esistente, altrimenti verrà visualizzato un errore simile a: "Uncaught ReferenceError: Cannot access 'Shape' before initialization"

Il costruttore di `Ball` deve definire una nuova proprietà denominata `exists`, usata per tenere traccia dell'esistenza delle palline nel programma (ovvero se non sono state mangiate dal cerchio malvagio). Deve essere un valore booleano (`true`/`false`), inizializzato a `true` nel costruttore.

Il metodo `collisionDetect()` della classe `Ball` richiede un piccolo aggiornamento. Una pallina deve essere considerata per il rilevamento delle collisioni solo se la proprietà `exists` è `true`. Pertanto, sostituire il codice esistente di `collisionDetect()` con il codice seguente:

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

Come spiegato sopra, l'unica aggiunta consiste nel verificare se la pallina esiste, usando `ball.exists` nella condizione `if`.

Le definizioni dei metodi `draw()` e `update()` di `Ball` possono rimanere esattamente uguali a prima.

A questo punto, provare a ricaricare il codice: dovrebbe funzionare esattamente come prima, con gli oggetti riprogettati.

### Definire EvilCircle

Ora è il momento di incontrare il cattivo: `EvilCircle()`! Il nostro gioco includerà un solo cerchio malvagio, ma verrà comunque definito usando un costruttore che eredita da `Shape()`, per fare pratica. In seguito potrebbe essere utile aggiungere un altro cerchio all'app controllabile da un altro giocatore, oppure avere più cerchi malvagi controllati dal computer. Probabilmente non si conquisterà il mondo con un solo cerchio malvagio, ma per questa sfida sarà sufficiente.

Creare una definizione per una classe `EvilCircle`. Deve ereditare da `Shape` usando `extends`.

#### Costruttore di EvilCircle

Il costruttore di `EvilCircle` deve:

- ricevere solo gli argomenti `x` e `y`
- passare gli argomenti `x` e `y` alla superclasse `Shape`, insieme a valori per `velX` e `velY` codificati direttamente a 20. Questo deve essere fatto con codice simile a `super(x, y, 20, 20);`
- impostare `color` su `white` e `size` su `10`.

Infine, il costruttore deve configurare il codice che consente all'utente di muovere il cerchio malvagio sullo schermo:

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

Questo aggiunge un listener dell'evento `keydown` all'oggetto `window`, in modo che quando viene premuto un tasto venga consultata la proprietà [`key`](/it/docs/Web/API/KeyboardEvent/key) dell'oggetto evento per determinare quale tasto è stato premuto. Se è uno dei quattro tasti specificati, il cerchio malvagio si sposterà a sinistra/destra/in alto/in basso.

### Definire metodi per EvilCircle

La classe `EvilCircle` deve avere tre metodi, come descritto di seguito.

#### draw()

Questo metodo ha lo stesso scopo del metodo `draw()` di `Ball`: disegna l'istanza dell'oggetto sul canvas. Il metodo `draw()` di `EvilCircle` funzionerà in modo molto simile, quindi è possibile iniziare copiando il metodo `draw()` di `Ball`. Apportare quindi le seguenti modifiche:

- Il cerchio malvagio non deve essere riempito, ma deve avere soltanto una linea esterna (stroke). Questo si può ottenere aggiornando [`fillStyle`](/it/docs/Web/API/CanvasRenderingContext2D/fillStyle) e [`fill()`](/it/docs/Web/API/CanvasRenderingContext2D/fill) rispettivamente in [`strokeStyle`](/it/docs/Web/API/CanvasRenderingContext2D/strokeStyle) e [`stroke()`](/it/docs/Web/API/CanvasRenderingContext2D/stroke).
- Si desidera inoltre rendere lo stroke un po' più spesso, in modo da vedere più facilmente il cerchio malvagio. Questo può essere ottenuto impostando un valore per [`lineWidth`](/it/docs/Web/API/CanvasRenderingContext2D/lineWidth) dopo la chiamata a [`beginPath()`](/it/docs/Web/API/CanvasRenderingContext2D/beginPath) (3 andrà bene).

#### checkBounds()

Questo metodo svolgerà la stessa funzione della prima parte del metodo `update()` di `Ball`: verificare se il cerchio malvagio sta per uscire dai bordi dello schermo e impedirglielo. Anche in questo caso è possibile copiare quasi interamente il metodo `update()` di `Ball`, ma occorre apportare alcune modifiche:

- Eliminare le ultime due righe: non vogliamo aggiornare automaticamente la posizione del cerchio malvagio a ogni frame, poiché verrà spostato in un altro modo, come mostrato più avanti.
- All'interno delle istruzioni `if ()`, se i test restituiscono true non si desidera aggiornare `velX`/`velY`; si desidera invece modificare il valore di `x`/`y` in modo che il cerchio malvagio venga riportato leggermente sullo schermo. Aggiungere o sottrarre, a seconda dei casi, la proprietà `size` del cerchio malvagio avrebbe senso.

#### collisionDetect()

Questo metodo agirà in modo molto simile al metodo `collisionDetect()` di `Ball`, quindi è possibile usarne una copia come base per questo nuovo metodo. Tuttavia, ci sono alcune differenze:

- Nell'istruzione `if` esterna, non è più necessario verificare se la pallina corrente nell'iterazione è la stessa pallina che esegue il controllo, poiché non è più una pallina: è il cerchio malvagio. Occorre invece verificare se la pallina controllata esiste (con quale proprietà è possibile farlo?). Se non esiste, è già stata mangiata dal cerchio malvagio, quindi non è necessario controllarla nuovamente.
- Nell'istruzione `if` interna, non si desidera più far cambiare colore agli oggetti quando viene rilevata una collisione; si desidera invece impostare le palline che entrano in collisione con il cerchio malvagio come non più esistenti (come si potrebbe fare?).

### Inserire il cerchio malvagio nel programma

Ora che il cerchio malvagio è stato definito, occorre farlo effettivamente apparire nella scena. Per farlo, apportare alcune modifiche alla funzione `loop()`.

- Prima di tutto, creare una nuova istanza dell'oggetto cerchio malvagio, specificando i parametri necessari. Questa operazione deve essere eseguita una sola volta, non a ogni iterazione del loop.
- Nel punto in cui si esegue il loop su ogni pallina e si chiamano le funzioni `draw()`, `update()` e `collisionDetect()` per ciascuna, fare in modo che queste funzioni vengano chiamate solo se la pallina corrente esiste.
- Chiamare i metodi `draw()`, `checkBounds()` e `collisionDetect()` dell'istanza del cerchio malvagio a ogni iterazione del loop.

### Implementare il contatore del punteggio

Per implementare il contatore del punteggio, seguire questi passaggi:

1. Nel file HTML, aggiungere un elemento {{HTMLElement("p")}} subito sotto l'elemento {{HTMLElement("Heading_Elements", "h1")}} contenente il testo "Ball count: ".
2. Nel file CSS, aggiungere la seguente regola in fondo:

   ```css
   p {
     position: absolute;
     margin: 0;
     top: 35px;
     right: 5px;
     color: #aaaaaa;
   }
   ```

3. Nel codice JavaScript, apportare i seguenti aggiornamenti:
   - Creare una variabile che memorizzi un riferimento al paragrafo.
   - Tenere traccia in qualche modo del numero di palline sullo schermo.
   - Incrementare il conteggio e visualizzare il numero aggiornato di palline ogni volta che una pallina viene aggiunta alla scena.
   - Decrementare il conteggio e visualizzare il numero aggiornato di palline ogni volta che il cerchio malvagio mangia una pallina, facendola cessare di esistere.

{{PreviousMenu("Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_building_practice", "Learn_web_development/Extensions/Advanced_JavaScript_objects")}}
