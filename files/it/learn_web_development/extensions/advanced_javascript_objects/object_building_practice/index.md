---
title: Pratica di costruzione di oggetti
slug: Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_building_practice
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Advanced_JavaScript_objects/Classes_in_JavaScript", "Learn_web_development/Extensions/Advanced_JavaScript_objects/Adding_bouncing_balls_features", "Learn_web_development/Extensions/Advanced_JavaScript_objects")}}

Nei precedenti articoli abbiamo esaminato tutta la teoria essenziale sugli oggetti JavaScript e i dettagli della sintassi, fornendoti una solida base di partenza. In questo articolo ci immergeremo in un esercizio pratico, dandoti ancora più pratica nella costruzione di oggetti JavaScript personalizzati, con un risultato divertente e colorato.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con le basi di JavaScript
        (in particolare
        <a href="/it/docs/Learn_web_development/Core/Scripting/Object_basics">le basi degli oggetti</a>) e i concetti orientati agli oggetti JavaScript trattati nelle lezioni precedenti di questo modulo.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        Pratica l'uso di oggetti e tecniche orientate agli oggetti
        in un contesto reale.
      </td>
    </tr>
  </tbody>
</table>

## Facciamo rimbalzare alcune palline

In questo articolo scriveremo un classico demo di "palline rimbalzanti", per mostrarti quanto possano essere utili gli oggetti in JavaScript. Le nostre piccole palline rimbalzeranno sullo schermo e cambieranno colore quando si toccheranno. L'esempio finito assomiglierà un po' a questo:

![Screenshot di una pagina web intitolata "Palline rimbalzanti". 23 palline di vari colori pastello e dimensioni sono visibili su uno schermo nero con lunghe scie dietro che indicano il movimento.](bouncing-balls.png)

Questo esempio userà la [Canvas API](/it/docs/Learn_web_development/Extensions/Client-side_APIs/Drawing_graphics) per disegnare le palline sullo schermo, e l'API [`requestAnimationFrame`](/it/docs/Web/API/Window/requestAnimationFrame) per animare l'intero display — non è necessario avere una conoscenza preliminare di queste API, e speriamo che, al termine di questo articolo, sarai interessato a esplorarle ulteriormente. Durante il percorso, useremo alcuni oggetti intelligenti e ti mostreremo un paio di belle tecniche come far rimbalzare le palline sui muri e controllare se si sono colpite a vicenda (conosciuto altrimenti come _rilevamento delle collisioni_).

## Iniziare

Per cominciare, crea copie locali dei nostri file [`index.html`](https://github.com/mdn/learning-area/blob/main/javascript/oojs/bouncing-balls/index.html), [`style.css`](https://github.com/mdn/learning-area/blob/main/javascript/oojs/bouncing-balls/style.css), e [`main.js`](https://github.com/mdn/learning-area/blob/main/javascript/oojs/bouncing-balls/main.js). Questi contengono rispettivamente:

1. Un documento HTML molto semplice con un elemento {{HTMLElement("Heading_Elements", "h1")}}, un elemento {{HTMLElement("canvas")}} per disegnare le nostre palline, ed elementi per applicare il nostro CSS e JavaScript al nostro HTML.
2. Alcuni stili molto semplici, che servono principalmente per stilizzare e posizionare l'`<h1>`, e rimuovere eventuali barre di scorrimento o margini attorno ai bordi della pagina (in modo che si presenti in modo carino e ordinato).
3. Un po' di JavaScript che serve per impostare l'elemento `<canvas>` e fornire una funzione generale che useremo.

La prima parte dello script è la seguente:

```js
const canvas = document.querySelector("canvas");
const ctx = canvas.getContext("2d");

const width = (canvas.width = window.innerWidth);
const height = (canvas.height = window.innerHeight);
```

Questo script ottiene un riferimento all'elemento `<canvas>`, quindi chiama il metodo [`getContext()`](/it/docs/Web/API/HTMLCanvasElement/getContext) su di esso per darci un contesto su cui possiamo iniziare a disegnare. La costante risultante (`ctx`) è l'oggetto che rappresenta direttamente l'area di disegno del canvas e ci consente di disegnarci forme 2D.

Successivamente, impostiamo le costanti chiamate `width` e `height`, e la larghezza e l'altezza dell'elemento canvas (rappresentate dalle proprietà `canvas.width` e `canvas.height`) per corrispondere alla larghezza e altezza del viewport del browser (l'area su cui appare la pagina web — che può essere ottenuta dalle proprietà [`Window.innerWidth`](/it/docs/Web/API/Window/innerWidth) e [`Window.innerHeight`](/it/docs/Web/API/Window/innerHeight)).

Nota che stiamo concatenando più assegnazioni insieme, per ottenere le variabili impostate più rapidamente — questo è perfettamente lecito.

Poi abbiamo due funzioni di supporto:

```js
function random(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min;
}

function randomRGB() {
  return `rgb(${random(0, 255)} ${random(0, 255)} ${random(0, 255)})`;
}
```

La funzione `random()` prende due numeri come argomenti e restituisce un numero casuale nell'intervallo tra i due. La funzione `randomRGB()` genera un colore casuale rappresentato come una stringa [`rgb()`](/it/docs/Web/CSS/color_value/rgb).

## Modellare una pallina nel nostro programma

Il nostro programma presenterà molte palline che rimbalzano sullo schermo. Poiché queste palline si comporteranno tutte allo stesso modo, ha senso rappresentarle con un oggetto. Iniziamo aggiungendo la seguente definizione di classe alla fine del nostro codice.

```js
class Ball {
  constructor(x, y, velX, velY, color, size) {
    this.x = x;
    this.y = y;
    this.velX = velX;
    this.velY = velY;
    this.color = color;
    this.size = size;
  }
}
```

Finora questa classe contiene solo un costruttore, nel quale possiamo inizializzare le proprietà che ciascuna pallina necessita per funzionare nel nostro programma:

- Coordinate `x` e `y` — le coordinate orizzontali e verticali dove la pallina inizia sullo schermo. Questo può variare tra 0 (angolo in alto a sinistra) fino alla larghezza e altezza del viewport del browser (angolo in basso a destra).
- Velocità orizzontale e verticale (`velX` e `velY`) — a ciascuna pallina è assegnata una velocità orizzontale e verticale; in termini reali questi valori sono regolarmente aggiunti ai valori di coordinate `x`/`y` quando animiamo le palline, per spostarle di questa quantità a ogni frame.
- `color` — a ciascuna pallina viene assegnato un colore.
- `size` — a ciascuna pallina viene assegnata una dimensione — questo è il suo raggio, in pixel.

Questo gestisce le proprietà, ma che dire dei metodi? Vogliamo far fare qualcosa alle nostre palline nel nostro programma.

### Disegnare la pallina

Prima aggiungi il seguente metodo `draw()` alla classe `Ball`:

```js
class Ball {
  // …
  draw() {
    ctx.beginPath();
    ctx.fillStyle = this.color;
    ctx.arc(this.x, this.y, this.size, 0, 2 * Math.PI);
    ctx.fill();
  }
}
```

Utilizzando questa funzione, possiamo dire alla pallina di disegnarsi sullo schermo, chiamando una serie di membri del contesto canvas 2D che abbiamo definito in precedenza (`ctx`). Il contesto è come la carta, e ora vogliamo comandare la nostra penna per disegnare qualcosa su di essa:

- Per prima cosa, usiamo [`beginPath()`](/it/docs/Web/API/CanvasRenderingContext2D/beginPath) per dichiarare che vogliamo disegnare una forma sulla carta.
- Successivamente, usiamo [`fillStyle`](/it/docs/Web/API/CanvasRenderingContext2D/fillStyle) per definire quale colore vogliamo che abbia la forma — lo impostiamo sulla proprietà `color` della nostra pallina.
- Quindi, usiamo il metodo [`arc()`](/it/docs/Web/API/CanvasRenderingContext2D/arc) per tracciare una forma ad arco sulla carta. I suoi parametri sono:

  - La posizione `x` e `y` del centro dell'arco — stiamo specificando le proprietà `x` e `y` della pallina.
  - Il raggio dell'arco — in questo caso, la proprietà `size` della pallina.
  - Gli ultimi due parametri specificano l'inizio e la fine del numero di gradi attorno al cerchio che l'arco è disegnato. Qui specifichiamo 0 gradi e `2 * PI`, che è l'equivalente di 360 gradi in radianti (sfortunatamente, devi specificarlo in radianti). Ciò ci dà un cerchio completo. Se avessi specificato solo `1 * PI`, otterresti un semicerchio (180 gradi).

- Infine, usiamo il metodo [`fill()`](/it/docs/Web/API/CanvasRenderingContext2D/fill), che in pratica dichiara "finisci di disegnare il percorso che abbiamo iniziato con `beginPath()`, e riempi l'area che occupa con il colore che abbiamo specificato in precedenza in `fillStyle`."

Puoi già iniziare a testare il tuo oggetto.

1. Salva il codice finora e carica il file HTML in un browser.
2. Apri la console JavaScript del browser e poi aggiorna la pagina in modo che la dimensione del canvas cambi nella piccola finestra visibile che rimane quando si apre la console.
3. Digita quanto segue per creare una nuova istanza di pallina:

   ```js
   const testBall = new Ball(50, 100, 4, 4, "blue", 10);
   ```

4. Prova a chiamare i suoi membri:

   ```js
   testBall.x;
   testBall.size;
   testBall.color;
   testBall.draw();
   ```

5. Quando inserisci l'ultima riga, dovresti vedere la pallina disegnarsi da qualche parte sul canvas.

### Aggiornare i dati della pallina

Possiamo disegnare la pallina in posizione, ma per spostare effettivamente la pallina, abbiamo bisogno di una funzione di aggiornamento di qualche tipo. Aggiungi il seguente codice all'interno della definizione della classe `Ball`:

```js
class Ball {
  // …
  update() {
    if (this.x + this.size >= width) {
      this.velX = -this.velX;
    }

    if (this.x - this.size <= 0) {
      this.velX = -this.velX;
    }

    if (this.y + this.size >= height) {
      this.velY = -this.velY;
    }

    if (this.y - this.size <= 0) {
      this.velY = -this.velY;
    }

    this.x += this.velX;
    this.y += this.velY;
  }
}
```

Le prime quattro parti della funzione controllano se la pallina ha raggiunto il bordo del canvas. Se lo ha fatto, invertiamo la polarità della velocità rilevante per far viaggiare la pallina nella direzione opposta. Quindi, ad esempio, se la pallina stava viaggiando verso l'alto (`velY` negativo), la velocità verticale viene cambiata in modo che inizi a viaggiare verso il basso (positivo `velY`).

Nei quattro casi, stiamo controllando:

- se la coordinata `x` è maggiore della larghezza del canvas (la pallina sta andando oltre il bordo destro).
- se la coordinata `x` è minore di 0 (la pallina sta andando oltre il bordo sinistro).
- se la coordinata `y` è maggiore dell'altezza del canvas (la pallina sta andando oltre il bordo inferiore).
- se la coordinata `y` è minore di 0 (la pallina sta andando oltre il bordo superiore).

In ciascun caso, includiamo la `size` della pallina nel calcolo perché le coordinate `x`/`y` sono al centro della pallina, ma vogliamo che il bordo della pallina rimbalzi sul perimetro — non vogliamo che la pallina vada a metà fuori dallo schermo prima che inizi a rimbalzare indietro.

Le ultime due righe aggiungono il valore `velX` alla coordinata `x`, e il valore `velY` alla coordinata `y` — la pallina viene effettivamente spostata ogni volta che questo metodo viene chiamato.

Questo è tutto per ora; procediamo con un po' di animazione!

## Animare la pallina

Ora rendiamo tutto più divertente. Ora inizieremo ad aggiungere palline al canvas e ad animarle.

Per prima cosa, dobbiamo creare un posto dove memorizzare tutte le nostre palline e poi popolarlo. Il seguente codice farà questo compito — aggiungilo alla fine del tuo codice ora:

```js
const balls = [];

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
```

Il ciclo `while` crea una nuova istanza del nostro `Ball()` utilizzando valori casuali generati con le nostre funzioni `random()` e `randomRGB()`, quindi lo `push()` alla fine del nostro array di palline, ma solo mentre il numero di palline nell'array è inferiore a 25. Quindi quando abbiamo 25 palline nell'array, non verranno aggiunte altre palline. Puoi provare a variare il numero in `balls.length < 25` per ottenere più o meno palline nell'array. A seconda di quanta potenza di elaborazione ha il tuo computer/browser, specificare diverse migliaia di palline potrebbe rallentare notevolmente l'animazione!

Successivamente, aggiungi il seguente codice alla fine del tuo codice:

```js
function loop() {
  ctx.fillStyle = "rgb(0 0 0 / 25%)";
  ctx.fillRect(0, 0, width, height);

  for (const ball of balls) {
    ball.draw();
    ball.update();
  }

  requestAnimationFrame(loop);
}
```

Tutti i programmi che animano cose generalmente coinvolgono un ciclo di animazione, che serve per aggiornare le informazioni nel programma e poi renderizzare la vista risultante su ciascun frame dell'animazione; questa è la base per la maggior parte dei giochi e di altri programmi simili. La nostra funzione `loop()` fa quanto segue:

- Imposta il colore di riempimento del canvas su nero semi-trasparente, quindi disegna un rettangolo del colore su tutta la larghezza e altezza del canvas, utilizzando `fillRect()` (i quattro parametri forniscono una coordinata iniziale e una larghezza e un'altezza per il rettangolo disegnato). Questo serve a coprire il disegno del frame precedente prima che il prossimo venga disegnato. Se non lo fai, vedrai solo lunghe serpenti strisciare sul canvas invece di palline che si muovono! Il colore del riempimento è impostato su semi-trasparente, `rgb(0 0 0 / 25%)`, per consentire ai frame precedenti di trasparire leggermente, producendo le piccole scie dietro le palline mentre si muovono. Se cambi `0.25` in `1`, non le vedrai affatto più. Prova a variare questo numero per vedere l'effetto che ha.
- Cicla attraverso tutte le palline nell'array `balls` e esegue la funzione `draw()` e `update()` di ciascuna pallina per disegnarne ciascuna sullo schermo, quindi eseguire gli aggiornamenti necessari alla posizione e velocità in tempo per il prossimo frame.
- Esegue nuovamente la funzione utilizzando il metodo `requestAnimationFrame()` — quando questo metodo viene eseguito ripetutamente e viene passato lo stesso nome di funzione, esegue quella funzione un numero impostato di volte al secondo per creare un'animazione fluida. Generalmente questo viene fatto ricorsivamente — il che significa che la funzione chiama se stessa ogni volta che viene eseguita, quindi viene eseguita più e più volte.

Infine, aggiungi la seguente riga alla fine del tuo codice — dobbiamo chiamare la funzione una volta per avviare l'animazione.

```js
loop();
```

Questo è tutto per le basi — prova a salvare e aggiornare per testare le tue palline rimbalzanti!

## Aggiungere il rilevamento delle collisioni

Ora per un po' di divertimento, aggiungiamo un po' di rilevamento delle collisioni al nostro programma, in modo che le nostre palline sappiano quando hanno colpito un'altra pallina.

Prima, aggiungi la seguente definizione di metodo alla tua classe `Ball`.

```js
class Ball {
  // …
  collisionDetect() {
    for (const ball of balls) {
      if (this !== ball) {
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
```

Questo metodo è un po' complesso, quindi non preoccuparti se non capisci esattamente come funziona per ora. Segue una spiegazione:

- Per ciascuna pallina, dobbiamo controllare ogni altra pallina per vedere se ha colliso con la pallina corrente. Per fare ciò, avviamo un altro ciclo `for...of` per scorrere tutte le palline nell'array `balls[]`.
- Immediatamente all'interno del ciclo for, usiamo un'istruzione `if` per verificare se la pallina corrente che si sta scorrendo è la stessa pallina di quella che stiamo attualmente controllando. Non vogliamo controllare se una pallina ha colliso con se stessa! Per fare ciò, verifichiamo se la pallina corrente (cioè la pallina il cui metodo collisionDetect è invocato) è la stessa della pallina nel ciclo (cioè la pallina a cui si fa riferimento dall'attuale iterazione del ciclo for nel metodo collisionDetect). Poi usiamo `!` per negare il controllo, in modo che il codice all'interno dell'istruzione `if` si esegua solo se non sono la stessa.
- Poi usiamo un algoritmo comune per controllare la collisione di due cerchi. In pratica stiamo controllando se le aree dei due cerchi si sovrappongono. Questo è spiegato più approfonditamente in [Rilevamento delle collisioni 2D](/it/docs/Games/Techniques/2D_collision_detection).
- Se viene rilevata una collisione, il codice all'interno dell'istruzione `if` interna viene eseguito. In questo caso, impostiamo solo la proprietà `color` di entrambi i cerchi su un nuovo colore casuale. Avremmo potuto fare qualcosa di molto più complesso, come far rimbalzare le palline l'una dall'altra realisticamente, ma sarebbe stato molto più complesso da implementare. Per tali simulazioni fisiche, gli sviluppatori tendono a usare librerie di giochi o fisica come [PhysicsJS](https://wellcaffeinated.net/PhysicsJS/), [matter.js](https://brm.io/matter-js/), [Phaser](https://phaser.io/), ecc.

Devi anche chiamare questo metodo in ogni frame dell'animazione. Aggiorna la tua funzione `loop()` per chiamare `ball.collisionDetect()` dopo `ball.update()`:

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
```

Salva e aggiorna nuovamente il demo, e vedrai le tue palline cambiare colore quando si collidono!

> [!NOTE]
> Se hai problemi a far funzionare questo esempio, prova a confrontare il tuo codice JavaScript con la nostra [versione finita](https://github.com/mdn/learning-area/blob/main/javascript/oojs/bouncing-balls/main-finished.js) (vedilo anche [eseguire dal vivo](https://mdn.github.io/learning-area/javascript/oojs/bouncing-balls/index-finished.html)).

## Riepilogo

Speriamo che ti sia divertito a scrivere il tuo esempio reale di palline rimbalzanti casuali, utilizzando varie tecniche di oggetti e orientamento agli oggetti da tutto il modulo! Questo dovrebbe averti dato una pratica utile nell'usare gli oggetti e un buon contesto reale.

Questo è tutto per le lezioni sugli oggetti — tutto ciò che rimane ora è per te testare le tue abilità nella sfida del modulo.

## Vedi anche

- [Tutorial Canvas](/it/docs/Web/API/Canvas_API/Tutorial) — un tutorial introduttivo su canvas 2D.
- [requestAnimationFrame()](/it/docs/Web/API/Window/requestAnimationFrame)
- [Rilevamento delle collisioni 2D](/it/docs/Games/Techniques/2D_collision_detection)
- [Rilevamento delle collisioni 3D](/it/docs/Games/Techniques/3D_collision_detection)
- [Gioco breakout 2D usando JavaScript puro](/it/docs/Games/Tutorials/2D_Breakout_game_pure_JavaScript) — un ottimo tutorial per principianti che mostra come costruire un gioco 2D.
- [Gioco breakout 2D usando Phaser](/it/docs/Games/Tutorials/2D_breakout_game_Phaser) — spiega le basi della costruzione di un gioco 2D utilizzando una libreria di giochi JavaScript.

{{PreviousMenuNext("Learn_web_development/Extensions/Advanced_JavaScript_objects/Classes_in_JavaScript", "Learn_web_development/Extensions/Advanced_JavaScript_objects/Adding_bouncing_balls_features", "Learn_web_development/Extensions/Advanced_JavaScript_objects")}}
