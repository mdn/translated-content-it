---
title: Pratica di costruzione di oggetti
slug: Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_building_practice
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Advanced_JavaScript_objects/Classes_in_JavaScript", "Learn_web_development/Extensions/Advanced_JavaScript_objects/Adding_bouncing_balls_features", "Learn_web_development/Extensions/Advanced_JavaScript_objects")}}

Nei precedenti articoli abbiamo esaminato tutta la teoria essenziale degli oggetti in JavaScript e i dettagli di sintassi, fornendole una solida base di partenza. In questo articolo ci immergiamo in un esercizio pratico, offrendole ulteriore pratica nella costruzione di oggetti JavaScript personalizzati, con un risultato divertente e colorato.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con le basi di JavaScript
        (soprattutto
        <a href="/it/docs/Learn_web_development/Core/Scripting/Object_basics">Basi degli oggetti</a>) e concetti di JavaScript orientato agli oggetti trattati nelle lezioni precedenti di questo modulo.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        Praticare l'uso di oggetti e tecniche orientate agli oggetti in un contesto reale.
      </td>
    </tr>
  </tbody>
</table>

## Facciamo rimbalzare alcune palle

In questo articolo scriveremo un classico demo di "palle rimbalzanti", per mostrarle quanto possano essere utili gli oggetti in JavaScript. Le nostre piccole palle rimbalzeranno sullo schermo e cambieranno colore quando si toccano. L'esempio finale avrà un aspetto simile a questo:

![Screenshot di una pagina web intitolata "Palle rimbalzanti". 23 palle di vari colori pastello e dimensioni sono visibili su uno schermo nero con lunghe scie dietro di esse che indicano il movimento.](bouncing-balls.png)

Questo esempio utilizzerà le [API Canvas](/it/docs/Learn_web_development/Extensions/Client-side_APIs/Drawing_graphics) per disegnare le palle sullo schermo e l'API [`requestAnimationFrame`](/it/docs/Web/API/Window/requestAnimationFrame) per animare l'intero display — non ha bisogno di avere conoscenze precedenti su queste API, e speriamo che al termine di questo articolo sarà interessato a esplorarle ulteriormente. Lungo la strada, faremo uso di alcuni pratici oggetti e le mostreremo un paio di tecniche interessanti come far rimbalzare le palle dai muri e controllare se si sono colpite tra loro (altrimenti noto come _rilevamento delle collisioni_).

## Come iniziare

Per cominciare, faccia copie locali dei nostri file [`index.html`](https://github.com/mdn/learning-area/blob/main/javascript/oojs/bouncing-balls/index.html), [`style.css`](https://github.com/mdn/learning-area/blob/main/javascript/oojs/bouncing-balls/style.css) e [`main.js`](https://github.com/mdn/learning-area/blob/main/javascript/oojs/bouncing-balls/main.js). Questi contengono rispettivamente quanto segue:

1. Un documento HTML molto semplice con un elemento {{HTMLElement("Heading_Elements", "h1")}}, un elemento {{HTMLElement("canvas")}} su cui disegnare le nostre palle, ed elementi per applicare il nostro CSS e JavaScript al nostro HTML.
2. Alcuni stili molto semplici, che servono principalmente a stilizzare e posizionare il `<h1>`, e a eliminare eventuali barre di scorrimento o margini attorno al bordo della pagina (così che appaia bello e ordinato).
3. Un po' di JavaScript che serve a impostare l'elemento `<canvas>` e fornire una funzione generale che useremo.

La prima parte dello script appare cosi:

```js
const canvas = document.querySelector("canvas");
const ctx = canvas.getContext("2d");

const width = (canvas.width = window.innerWidth);
const height = (canvas.height = window.innerHeight);
```

Questo script ottiene un riferimento all'elemento `<canvas>`, quindi chiama il metodo [`getContext()`](/it/docs/Web/API/HTMLCanvasElement/getContext) su di esso per fornirci un contesto su cui possiamo iniziare a disegnare. La costante risultante (`ctx`) è l'oggetto che rappresenta direttamente l'area di disegno del canvas e ci permette di disegnare forme 2D su di esso.

Successivamente, impostiamo costanti chiamate `width` e `height`, e la larghezza e l'altezza dell'elemento canvas (rappresentati dalle proprietà `canvas.width` e `canvas.height`) per uguagliare la larghezza e l'altezza della finestra del browser (l'area su cui appare la pagina web — questo può essere ottenuto dalle proprietà [`Window.innerWidth`](/it/docs/Web/API/Window/innerWidth) e [`Window.innerHeight`](/it/docs/Web/API/Window/innerHeight)).

Si noti che stiamo concatenando più assegnazioni insieme, per impostare le variabili più rapidamente — va perfettamente bene.

Poi abbiamo due funzioni di supporto:

```js
function random(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min;
}

function randomRGB() {
  return `rgb(${random(0, 255)} ${random(0, 255)} ${random(0, 255)})`;
}
```

La funzione `random()` prende due numeri come argomenti e restituisce un numero casuale nell'intervallo tra i due. La funzione `randomRGB()` genera un colore casuale rappresentato come stringa [`rgb()`](/it/docs/Web/CSS/color_value/rgb).

## Modellizzare una palla nel nostro programma

Il nostro programma presenterà molte palle che rimbalzano sullo schermo. Poiché queste palle si comporteranno tutte allo stesso modo, ha senso rappresentarle con un oggetto. Iniziamo aggiungendo la seguente definizione di classe alla fine del nostro codice.

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

Finora questa classe contiene solo un costruttore, nel quale possiamo inizializzare le proprietà di cui ogni palla ha bisogno per funzionare nel nostro programma:

- coordinate `x` e `y` — le coordinate orizzontali e verticali dove la palla inizia sullo schermo. Questo può variare tra 0 (angolo in alto a sinistra) e la larghezza e altezza della finestra del browser (angolo in basso a destra).
- velocità orizzontale e verticale (`velX` e `velY`) — a ogni palla viene data una velocità orizzontale e verticale; in termini reali questi valori vengono aggiunti regolarmente ai valori delle coordinate `x`/`y` quando animiamo le palle, per spostarle di tanto su ogni fotogramma.
- `color` — a ogni palla viene dato un colore.
- `size` — a ogni palla è assegnata una dimensione — questo è il suo raggio, in pixel.

Questo gestisce le proprietà, ma per quanto riguarda i metodi? Vogliamo che le nostre palle facciano effettivamente qualcosa nel nostro programma.

### Disegnare la palla

Per prima cosa, aggiunga il seguente metodo `draw()` alla classe `Ball`:

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

Utilizzando questa funzione, possiamo dire alla palla di disegnarsi sullo schermo, chiamando una serie di membri del contesto 2D del canvas che abbiamo definito prima (`ctx`). Il contesto è come la carta, e ora vogliamo comandare alla nostra penna di disegnare qualcosa su di essa:

- Prima, usiamo [`beginPath()`](/it/docs/Web/API/CanvasRenderingContext2D/beginPath) per dichiarare che vogliamo disegnare una forma sulla carta.
- Successivamente, usiamo [`fillStyle`](/it/docs/Web/API/CanvasRenderingContext2D/fillStyle) per definire di che colore vogliamo che sia la forma — la impostiamo sulla nostra proprietà `color` della palla.
- Successivamente, usiamo il metodo [`arc()`](/it/docs/Web/API/CanvasRenderingContext2D/arc) per tracciare una forma ad arco sulla carta. I suoi parametri sono:

  - La posizione `x` e `y` del centro dell'arco — stiamo specificando le proprietà `x` e `y` della palla.
  - Il raggio dell'arco — in questo caso, la proprietà `size` della palla.
  - Gli ultimi due parametri specificano il numero di gradi di inizio e fine intorno al cerchio tra cui l'arco è disegnato. Qui specifichiamo 0 gradi e `2 * PI`, che è l'equivalente di 360 gradi in radianti (fastidiosamente, è necessario specificarlo in radianti). Questo ci dà un cerchio completo. Se avesse specificato solo `1 * PI`, otterrebbe un semicircolo (180 gradi).

- Infine, usiamo il metodo [`fill()`](/it/docs/Web/API/CanvasRenderingContext2D/fill), che sostanzialmente indica "termina il disegno del percorso che abbiamo iniziato con `beginPath()`, e riempi l'area che occupa con il colore che abbiamo specificato precedentemente in `fillStyle`."

Può già iniziare a testare il suo oggetto.

1. Salvi il codice finora e carichi il file HTML in un browser.
2. Apra la console JavaScript del browser e poi ricarichi la pagina in modo che la dimensione del canvas cambi con la visualizzazione più piccola rimasta quando la console viene aperta.
3. Digiti quanto segue per creare una nuova istanza di palla:

   ```js
   const testBall = new Ball(50, 100, 4, 4, "blue", 10);
   ```

4. Provi a chiamare i suoi membri:

   ```js
   testBall.x;
   testBall.size;
   testBall.color;
   testBall.draw();
   ```

5. Quando inserirà l'ultima riga, vedrà la palla disegnarsi da qualche parte sul canvas.

### Aggiornare i dati della palla

Possiamo disegnare la palla in posizione, ma per effettivamente muovere la palla, abbiamo bisogno di una funzione di aggiornamento di qualche tipo. Aggiunga il seguente codice all'interno della definizione della classe `Ball`:

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

Le prime quattro parti di questa funzione controllano se la palla ha raggiunto il bordo del canvas. Se lo ha fatto, invertiamo la polarità della velocità rilevante per fare in modo che la palla viaggi nella direzione opposta. Così, per esempio, se la palla stava viaggiando verso l'alto (velocità verticale negativa `velY`), allora la velocità verticale viene cambiata in modo che inizi a viaggiare verso il basso (velocità verticale positiva `velY`).

Nei quattro casi, stiamo verificando:

- se la coordinata `x` è maggiore della larghezza del canvas (la palla sta uscendo dal bordo destro).
- se la coordinata `x` è minore di 0 (la palla sta uscendo dal bordo sinistro).
- se la coordinata `y` è maggiore dell'altezza del canvas (la palla sta uscendo dal bordo inferiore).
- se la coordinata `y` è minore di 0 (la palla sta uscendo dal bordo superiore).

In ogni caso, includiamo la `size` della palla nel calcolo perché le coordinate `x`/`y` sono al centro della palla, ma vogliamo che il bordo della palla rimbalzi perimetrale — non vogliamo che la palla vada a metà fuori dallo schermo prima di iniziare a rimbalzare indietro.

Le ultime due righe aggiungono il valore `velX` alla coordinata `x`, e il valore `velY` alla coordinata `y` — la palla viene effettivamente spostata ogni volta che questo metodo viene chiamato.

Questo è sufficiente per ora; procediamo con un po' di animazione!

## Animare la palla

Ora rendiamo la cosa divertente. Stiamo per iniziare ad aggiungere palle sul canvas e ad animarle.

Prima, dobbiamo creare un posto per memorizzare tutte le nostre palle e poi popolarlo. Il seguente codice farà questo lavoro — lo aggiunga ora alla fine del suo codice:

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

Il ciclo `while` crea una nuova istanza della nostra `Ball()` utilizzando valori casuali generati con le nostre funzioni `random()` e `randomRGB()`, poi la `push()` alla fine del nostro array di palle, finché il numero di palle nell'array è inferiore a 25. Quindi, quando abbiamo 25 palle nell'array, non ne saranno aggiunte altre. Può provare a variare il numero in `balls.length < 25` per ottenere più o meno palle nell'array. A seconda di quanta potenza di elaborazione ha il suo computer/browser, specificare diverse migliaia di palle potrebbe rallentare l'animazione piuttosto notevolmente!

Successivamente, aggiunga il seguente codice alla fine del suo:

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

Tutti i programmi che animano cose generalmente coinvolgono un ciclo di animazione, che serve ad aggiornare le informazioni nel programma e poi a renderizzare la vista risultante in ogni fotogramma dell'animazione; questa è la base per la maggior parte dei giochi e di altri programmi simili. La nostra funzione `loop()` fa quanto segue:

- Imposta il colore di riempimento del canvas su nero semi-trasparente, quindi disegna un rettangolo del colore attraverso tutta la larghezza e l'altezza del canvas, usando `fillRect()` (i quattro parametri forniscono una coordinata di inizio e una larghezza e altezza per il rettangolo disegnato). Questo serve a coprire il disegno del fotogramma precedente prima che venga disegnato il successivo. Se non fa questo, vedrà solo lunghe scie muoversi nel canvas invece di palle in movimento! Il colore del riempimento è impostato su semi-trasparente, `rgb(0 0 0 / 25%)`, per permettere che i fotogrammi precedenti brillino leggermente, producendo i piccoli tracciati dietro le palle mentre si muovono. Se ha cambiato 0.25 a 1, non le vedrebbe affatto più. Provi a variare questo numero per vedere l'effetto che ha.
- Passa in rassegna tutte le palle nell'array `balls`, e esegue la funzione `draw()` e `update()` di ciascuna palla per disegnarne ciascuna sullo schermo, quindi esegue gli aggiornamenti necessari alla posizione e alla velocità in tempo per il fotogramma successivo.
- Esegue la funzione di nuovo usando il metodo `requestAnimationFrame()` — quando questo metodo viene eseguito ripetutamente e passato lo stesso nome della funzione, esegue quella funzione un determinato numero di volte al secondo per creare un'animazione fluida. Questo è generalmente fatto in modo ricorsivo — il che significa che la funzione chiama se stessa ogni volta che viene eseguita, quindi viene eseguita ancora e ancora.

Infine, aggiunga la seguente riga alla fine del suo codice — dobbiamo chiamare la funzione una volta per avviare l'animazione.

```js
loop();
```

Questo è tutto per quanto riguarda le basi — provi a salvare e aggiornare per testare le sue palle rimbalzanti!

## Aggiungere il rilevamento delle collisioni

Ora per un po' di divertimento, aggiungiamo il rilevamento delle collisioni al nostro programma, in modo che le nostre palle sappiano quando hanno colpito un'altra palla.

Per prima cosa, aggiunga la seguente definizione di metodo alla sua classe `Ball`.

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

Questo metodo è un po' complesso, quindi non si preoccupi se per ora non comprende esattamente come funzioni. Segue una spiegazione:

- Per ogni palla, dobbiamo controllare ogni altra palla per vedere se ha colliso con quella corrente. Per farlo, avviamo un altro ciclo `for...of` per passare attraverso tutte le palle nell'array `balls[]`.
- Immediatamente all'interno del ciclo for, usiamo un'istruzione `if` per controllare se la palla corrente che stiamo scorrendo è la stessa palla di quella che stiamo attualmente controllando. Non vogliamo controllare se una palla ha colliso con se stessa! Per farlo, controlliamo se la palla attuale (cioè la palla il cui metodo collisionDetect è in corso di esecuzione) è la stessa della palla del ciclo (cioè la palla a cui si riferisce l'iterazione corrente del ciclo for nel metodo collisionDetect). Poi usiamo `!` per negare il controllo, in modo che il codice all'interno dell'istruzione `if` venga eseguito solo se NON sono lo stesso.
- Poi usiamo un algoritmo comune per controllare la collisione di due cerchi. In pratica stiamo verificando se l'area di uno dei due cerchi si sovrappone. Questo è spiegato più in dettaglio nel [rilevamento delle collisioni 2D](/it/docs/Games/Techniques/2D_collision_detection).
- Se viene rilevata una collisione, viene eseguito il codice all'interno dell'istruzione `if` interna. In questo caso, modifichiamo soltanto la proprietà `color` di entrambi i cerchi in un nuovo colore casuale. Potremmo aver fatto qualcosa di molto più complesso, come fare in modo che le palle rimbalzassero l'una contro l'altra realistamene, ma sarebbe stato molto più complesso da implementare. Per tali simulazioni fisiche, gli sviluppatori tendono a usare librerie di giochi o di fisica come [PhysicsJS](https://wellcaffeinated.net/PhysicsJS/), [matter.js](https://brm.io/matter-js/), [Phaser](https://phaser.io/), ecc.

Deve anche chiamare questo metodo in ogni fotogramma dell'animazione. Aggiorni la sua funzione `loop()` per chiamare `ball.collisionDetect()` dopo `ball.update()`:

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

Salvi e aggiorni di nuovo la demo, e vedrà le sue palle cambiare colore quando si collidono!

> [!NOTE]
> Se ha problemi a far funzionare questo esempio, provi a comparare il suo codice JavaScript con la nostra [versione finale](https://github.com/mdn/learning-area/blob/main/javascript/oojs/bouncing-balls/main-finished.js) (veda anche il [funzionamento dal vivo](https://mdn.github.io/learning-area/javascript/oojs/bouncing-balls/index-finished.html)).

## Riepilogo

Speriamo che si sia divertito a scrivere il suo esempio reale di palle rimbalzanti casuali, usando varie tecniche orientate agli oggetti da tutto il modulo! Questo dovrebbe averle dato una pratica utile nell'uso degli oggetti, e un buon contesto reale.

Questo è tutto per le lezioni sugli oggetti — resta solo per lei mettere alla prova le sue abilità nella sfida del modulo.

## Vedere anche

- [Tutorial Canvas](/it/docs/Web/API/Canvas_API/Tutorial) — un tutorial per principianti su canvas 2D.
- [requestAnimationFrame()](/it/docs/Web/API/Window/requestAnimationFrame)
- [Rilevamento delle collisioni 2D](/it/docs/Games/Techniques/2D_collision_detection)
- [Rilevamento delle collisioni 3D](/it/docs/Games/Techniques/3D_collision_detection)
- [Gioco 2D breakout usando JavaScript puro](/it/docs/Games/Tutorials/2D_Breakout_game_pure_JavaScript) — un ottimo tutorial per principianti che mostra come costruire un gioco 2D.
- [Gioco 2D breakout usando Phaser](/it/docs/Games/Tutorials/2D_breakout_game_Phaser) — spiega le basi su come costruire un gioco 2D usando una libreria di giochi JavaScript.

{{PreviousMenuNext("Learn_web_development/Extensions/Advanced_JavaScript_objects/Classes_in_JavaScript", "Learn_web_development/Extensions/Advanced_JavaScript_objects/Adding_bouncing_balls_features", "Learn_web_development/Extensions/Advanced_JavaScript_objects")}}
