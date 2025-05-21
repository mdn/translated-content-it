---
title: "Sfida: Aggiungere funzionalità alla nostra demo di palle rimbalzanti"
short-title: "Sfida: Funzionalità delle palle rimbalzanti"
slug: Learn_web_development/Extensions/Advanced_JavaScript_objects/Adding_bouncing_balls_features
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenu("Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_building_practice", "Learn_web_development/Extensions/Advanced_JavaScript_objects")}}

In questa sfida, ci si aspetta che si utilizzi la demo delle palle rimbalzanti dell'articolo precedente come punto di partenza, e che vi si aggiungano alcune funzionalità nuove e interessanti.

## Punto di partenza

Per iniziare questa sfida, creare una copia locale di [index-finished.html](https://github.com/mdn/learning-area/blob/main/javascript/oojs/bouncing-balls/index-finished.html), [style.css](https://github.com/mdn/learning-area/blob/main/javascript/oojs/bouncing-balls/style.css), e [main-finished.js](https://github.com/mdn/learning-area/blob/main/javascript/oojs/bouncing-balls/main-finished.js) dal nostro ultimo articolo in una nuova directory sul tuo computer locale.

In alternativa, è possibile utilizzare un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/). Si potrebbero incollare HTML, CSS e JavaScript in uno di questi editor online. Se l'editor online che si sta utilizzando non ha un pannello JavaScript separato, si può inserire il codice inline in un elemento `<script>` all'interno della pagina HTML.

> [!NOTE]
> Se ci si blocca, si può contattare uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Suggerimenti e consigli

Ecco alcuni suggerimenti prima di iniziare.

- Questa sfida è piuttosto difficile. Leggi tutte le istruzioni prima di iniziare a codificare, e prendi ogni passaggio lentamente e con attenzione.
- Potrebbe essere una buona idea salvare una copia separata della demo dopo aver completato ogni fase funzionante, così da poterla consultare se ci si trova in difficoltà successivamente.

## Progetto

La nostra demo delle palle rimbalzanti è divertente, ma ora vogliamo renderla un po' più interattiva aggiungendo un cerchio malvagio controllato dall'utente, che mangerà le palle se le cattura. Vogliamo anche mettere alla prova le tue capacità di costruzione di oggetti creando un oggetto generico `Shape()` da cui le nostre palle e il cerchio malvagio possano ereditare. Infine, vogliamo aggiungere un contatore di punteggio per tracciare il numero di palle rimaste da catturare.

Lo screenshot seguente ti dà un'idea di come dovrebbe apparire il programma finito:

![Screenshot della pagina demo delle palle rimbalzanti. Un cerchio con contorno bianco è visibile in aggiunta alle palle colorate, e il testo "Ball count: 23" è visibile sotto il titolo.](bouncing-evil-circle.png)

Per darti un'idea più chiara, dai un'occhiata all'[esempio finito](https://mdn.github.io/learning-area/javascript/oojs/assessment/) (non sbirciare nel codice sorgente!)

## Passi da completare

Le sezioni seguenti descrivono cosa devi fare.

### Creare una classe Shape

Per prima cosa, crea una nuova classe `Shape`. Questa ha solo un costruttore. Il costruttore `Shape` dovrebbe definire le proprietà `x`, `y`, `velX` e `velY` nello stesso modo in cui faceva originariamente il costruttore `Ball()`, ma non le proprietà `color` e `size`.

La classe `Ball` dovrebbe essere derivata da `Shape` usando `extends`. Il costruttore per `Ball` dovrebbe:

- accettare gli stessi argomenti di prima: `x`, `y`, `velX`, `velY`, `size`, e `color`
- chiamare il costruttore `Shape` usando `super()`, passando gli argomenti `x`, `y`, `velX`, e `velY`
- inizializzare le proprie proprietà `color` e `size` dai parametri che riceve.

> [!NOTE]
> Assicurati di creare la classe `Shape` sopra l'esistente classe `Ball`, altrimenti si incontreranno errori come: "Uncaught ReferenceError: Cannot access 'Shape' before initialization"

Il costruttore di `Ball` dovrebbe definire una nuova proprietà chiamata `exists`, che viene utilizzata per tracciare se le palle esistono nel programma (non sono state mangiate dal cerchio malvagio). Questa dovrebbe essere un booleano (`true`/`false`), inizializzato a `true` nel costruttore.

Il metodo `collisionDetect()` della classe `Ball` necessita di un piccolo aggiornamento. Una palla deve essere considerata per il rilevamento delle collisioni solo se la proprietà `exists` è `true`. Quindi, sostituire il codice esistente di `collisionDetect()` con il seguente codice:

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

Come discusso sopra, l'unica aggiunta è controllare se la palla esiste — usando `ball.exists` nel condizionale `if`.

Le definizioni dei metodi `draw()` e `update()` della palla dovrebbero rimanere esattamente come erano prima.

A questo punto, prova a ricaricare il codice — dovrebbe funzionare esattamente come prima, con i nostri oggetti ridisegnati.

### Definire EvilCircle

Ora è il momento di incontrare il cattivo — il `EvilCircle()`! Il nostro gioco coinvolgerà solo un cerchio malvagio, ma lo definiremo comunque usando un costruttore che eredita da `Shape()`, per darti un po' di pratica. Potresti voler aggiungere un altro cerchio all'app in seguito che può essere controllato da un altro giocatore, o avere diversi cerchi malvagi controllati dal computer. Probabilmente non conquisterai il mondo con un solo cerchio malvagio, ma sarà sufficiente per questa sfida.

Crea una definizione per una classe `EvilCircle`. Dovrebbe ereditare da `Shape` usando `extends`.

#### Costruttore di EvilCircle

Il costruttore per `EvilCircle` dovrebbe:

- ricevere solo gli argomenti `x`, `y`
- passare gli argomenti `x`, `y` alla superclasse `Shape` insieme ai valori per `velX` e `velY` fissati a 20. Dovresti farlo con codice come `super(x, y, 20, 20);`
- impostare `color` a `white` e `size` a `10`.

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

Questo aggiunge un event listener `keydown` all'oggetto `window` in modo che, quando un tasto viene premuto, la proprietà [`key`](/it/docs/Web/API/KeyboardEvent/key) dell'oggetto evento viene consultata per vedere quale tasto è stato premuto. Se è uno dei quattro tasti specificati, allora il cerchio malvagio si muoverà a sinistra/destra/su/giù.

### Definire i metodi per EvilCircle

La classe `EvilCircle` dovrebbe avere tre metodi, descritti di seguito.

#### draw()

Questo metodo ha lo stesso scopo del metodo `draw()` per `Ball`: disegna l'istanza dell'oggetto sul canvas. Il metodo `draw()` per `EvilCircle` funzionerà in modo molto simile, quindi si può iniziare copiando il metodo `draw()` per `Ball`. Dovresti poi apportare le seguenti modifiche:

- Vogliamo che il cerchio malvagio non sia riempito, ma piuttosto abbia solo un bordo esterno (stroke). Puoi ottenere ciò aggiornando [`fillStyle`](/it/docs/Web/API/CanvasRenderingContext2D/fillStyle) e [`fill()`](/it/docs/Web/API/CanvasRenderingContext2D/fill) a [`strokeStyle`](/it/docs/Web/API/CanvasRenderingContext2D/strokeStyle) e [`stroke()`](/it/docs/Web/API/CanvasRenderingContext2D/stroke) rispettivamente.
- Vogliamo anche rendere il contorno un po' più spesso, in modo da poter vedere il cerchio malvagio più facilmente. Questo può essere ottenuto impostando un valore per [`lineWidth`](/it/docs/Web/API/CanvasRenderingContext2D/lineWidth) da qualche parte dopo la chiamata a [`beginPath()`](/it/docs/Web/API/CanvasRenderingContext2D/beginPath) (3 andrà bene).

#### checkBounds()

Questo metodo farà la stessa cosa della prima parte del metodo `update()` per `Ball` — controllare se il cerchio malvagio sta per uscire dallo schermo, e impedirlo. Anche in questo caso, puoi semplicemente copiare il metodo `update()` per `Ball`, ma ci sono alcune modifiche che dovresti fare:

- Elimina le ultime due righe — non vogliamo aggiornare automaticamente la posizione del cerchio malvagio a ogni frame, perché lo muoveremo in un altro modo, come vedrai di seguito.
- All'interno delle istruzioni `if ()`, se i test restituiscono vero non vogliamo aggiornare `velX`/`velY`; vogliamo invece cambiare il valore di `x`/`y` in modo che il cerchio malvagio rimbalzi leggermente indietro sullo schermo. Aggiungere o sottrarre (secondo necessità) la proprietà `size` del cerchio malvagio avrebbe senso.

#### collisionDetect()

Questo metodo agirà in modo molto simile al metodo `collisionDetect()` per `Ball`, quindi puoi usare una copia di quello come base per questo nuovo metodo. Ma ci sono un paio di differenze:

- Nella dichiarazione `if` esterna, non hai più bisogno di controllare se la palla corrente nell'iterazione è la stessa palla che sta eseguendo il controllo — perché non è più una palla, è il cerchio malvagio! Invece, devi fare un test per vedere se la palla che stai controllando esiste (con quale proprietà puoi fare questo?). Se non esiste, è già stata mangiata dal cerchio malvagio, quindi non c'è bisogno di controllarla di nuovo.
- Nella dichiarazione `if` interna, non vuoi più far cambiare colore agli oggetti quando viene rilevata una collisione — invece, vuoi far sì che le palle che collidono con il cerchio malvagio non esistano più (di nuovo, come pensi che potresti fare questo?).

### Inserire il cerchio malvagio nel programma

Ora che abbiamo definito il cerchio malvagio, dobbiamo effettivamente farlo apparire nella nostra scena. Per farlo, è necessario effettuare alcune modifiche alla funzione `loop()`.

- Prima di tutto, crea una nuova istanza dell'oggetto cerchio malvagio (specificando i parametri necessari). Devi farlo solo una volta, non a ogni iterazione del loop.
- Nel punto in cui si passa attraverso ogni palla e si chiamano le funzioni `draw()`, `update()`, e `collisionDetect()` per ciascuna, fai in modo che queste funzioni siano chiamate solo se la palla attuale esiste.
- Chiama i metodi `draw()`, `checkBounds()`, e `collisionDetect()` dell'istanza del cerchio malvagio a ogni iterazione del loop.

### Implementare il contatore del punteggio

Per implementare il contatore del punteggio, segui i passaggi seguenti:

1. Nel tuo file HTML, aggiungi un elemento {{HTMLElement("p")}} appena sotto l'elemento {{HTMLElement("Heading_Elements", "h1")}} contenente il testo "Ball count: ".
2. Nel tuo file CSS, aggiungi la seguente regola alla fine:

   ```css
   p {
     position: absolute;
     margin: 0;
     top: 35px;
     right: 5px;
     color: #aaa;
   }
   ```

3. Nel tuo JavaScript, fai le seguenti modifiche:

   - Crea una variabile che memorizzi un riferimento al paragrafo.
   - Tieni il conteggio del numero di palle sullo schermo in qualche modo.
   - Incrementa il conteggio e visualizza il numero aggiornato di palle ogni volta che una palla viene aggiunta alla scena.
   - Decrementa il conteggio e visualizza il numero aggiornato di palle ogni volta che il cerchio malvagio mangia una palla (fa sì che non esista più).

{{PreviousMenu("Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_building_practice", "Learn_web_development/Extensions/Advanced_JavaScript_objects")}}
