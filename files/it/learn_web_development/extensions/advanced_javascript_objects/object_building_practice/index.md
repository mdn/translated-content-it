---
title: Esercitazione sulla costruzione di oggetti
slug: Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_building_practice
l10n:
  sourceCommit: 2b4a2ad5d9ba084a9eaa2f9204102655e7b575c4
---

{{PreviousMenuNext("Learn_web_development/Extensions/Advanced_JavaScript_objects/Test_your_skills/Object-oriented_JavaScript", "Learn_web_development/Extensions/Advanced_JavaScript_objects/Adding_bouncing_balls_features", "Learn_web_development/Extensions/Advanced_JavaScript_objects")}}

Negli articoli precedenti abbiamo esaminato tutti i dettagli essenziali della teoria e della sintassi degli oggetti JavaScript, fornendo una solida base da cui iniziare. In questo articolo approfondiremo un esercizio pratico, offrendo ulteriore pratica nella costruzione di oggetti JavaScript personalizzati, con un risultato divertente e colorato.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con le basi di JavaScript
        (in particolare con le
        <a href="/it/docs/Learn_web_development/Core/Scripting/Object_basics">Basi degli oggetti</a>) e con i concetti di JavaScript orientato agli oggetti trattati nelle lezioni precedenti di questo modulo.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        Esercitarsi nell'uso di oggetti e tecniche orientate agli oggetti
        in un contesto reale.
      </td>
    </tr>
  </tbody>
</table>

## Facciamo rimbalzare alcune palline

In questo articolo scriveremo una classica demo di "palline rimbalzanti", per mostrare quanto possano essere utili gli oggetti in JavaScript. Le nostre piccole palline rimbalzeranno sullo schermo e cambieranno colore quando si toccano tra loro. L'esempio completato avrà un aspetto simile a questo:

![Schermata di una pagina web intitolata "Bouncing balls". Su uno schermo nero sono visibili 23 palline di vari colori pastello e dimensioni, con lunghe scie dietro di esse che ne indicano il movimento.](bouncing-balls.png)

Questo esempio utilizzerà la [Canvas API](/it/docs/Learn_web_development/Extensions/Client-side_APIs/Drawing_graphics) per disegnare le palline sullo schermo e l'API [`requestAnimationFrame`](/it/docs/Web/API/Window/requestAnimationFrame) per animare l'intera visualizzazione — non è necessaria alcuna conoscenza pregressa di queste API e, al termine di questo articolo, si spera che nasca l'interesse di esplorarle più a fondo. Durante il percorso useremo alcuni oggetti ingegnosi e mostreremo alcune tecniche utili, come far rimbalzare le palline contro i muri e verificare se si sono urtate (altrimenti noto come _rilevamento delle collisioni_).

## Per iniziare

Per prima cosa, creare copie locali dei file [`index.html`](https://github.com/mdn/learning-area/blob/main/javascript/oojs/bouncing-balls/index.html), [`style.css`](https://github.com/mdn/learning-area/blob/main/javascript/oojs/bouncing-balls/style.css) e [`main.js`](https://github.com/mdn/learning-area/blob/main/javascript/oojs/bouncing-balls/main.js). Questi contengono rispettivamente:

1. Un documento HTML molto semplice con un elemento {{HTMLElement("Heading_Elements", "h1")}}, un elemento {{HTMLElement("canvas")}} su cui disegnare le palline e gli elementi per applicare CSS e JavaScript all'HTML.
2. Alcuni stili molto semplici, che servono principalmente a definire lo stile e la posizione di `<h1>` e a eliminare eventuali barre di scorrimento o margini lungo il bordo della pagina, per ottenere un aspetto ordinato.
3. JavaScript che serve a configurare l'elemento `<canvas>` e a fornire una funzione generale che verrà utilizzata.

La prima parte dello script è la seguente:

```js
const canvas = document.querySelector("canvas");
const ctx = canvas.getContext("2d");

const width = (canvas.width = window.innerWidth);
const height = (canvas.height = window.innerHeight);
```

Questo script ottiene un riferimento all'elemento `<canvas>`, quindi chiama su di esso il metodo [`getContext()`](/it/docs/Web/API/HTMLCanvasElement/getContext) per fornire un contesto sul quale iniziare a disegnare. La costante risultante (`ctx`) è l'oggetto che rappresenta direttamente l'area di disegno del canvas e consente di disegnarvi forme 2D.

Successivamente, vengono impostate le costanti denominate `width` e `height`, nonché la larghezza e l'altezza dell'elemento canvas (rappresentate dalle proprietà `canvas.width` e `canvas.height`), affinché siano uguali alla larghezza e all'altezza della viewport del browser (l'area in cui viene visualizzata la pagina web — ottenibile dalle proprietà [`Window.innerWidth`](/it/docs/Web/API/Window/innerWidth) e [`Window.innerHeight`](/it/docs/Web/API/Window/innerHeight)).

Si noti che più assegnazioni vengono concatenate insieme per impostare più rapidamente tutte le variabili: è perfettamente corretto.

Sono poi presenti due funzioni di supporto:

```js
function random(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min;
}

function randomRGB() {
  return `rgb(${random(0, 255)} ${random(0, 255)} ${random(0, 255)})`;
}
```

La funzione `random()` accetta due numeri come argomenti e restituisce un numero casuale nell'intervallo compreso tra essi. La funzione `randomRGB()` genera un colore casuale rappresentato come stringa {{cssxref("color_value/rgb")}}.

## Modellare una pallina nel programma

Il programma includerà molte palline che rimbalzano sullo schermo. Poiché queste palline si comporteranno tutte nello stesso modo, è sensato rappresentarle con un oggetto. Iniziare aggiungendo la seguente definizione di classe alla fine del codice.

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

Per ora questa classe contiene soltanto un costruttore, nel quale è possibile inizializzare le proprietà necessarie a ogni pallina per funzionare nel programma:

- coordinate `x` e `y` — le coordinate orizzontale e verticale in cui la pallina inizia sullo schermo. Possono variare da 0, nell'angolo superiore sinistro, fino alla larghezza e all'altezza della viewport del browser, nell'angolo inferiore destro.
- velocità orizzontale e verticale (`velX` e `velY`) — a ogni pallina viene assegnata una velocità orizzontale e verticale; in pratica, questi valori vengono aggiunti regolarmente ai valori delle coordinate `x`/`y` durante l'animazione delle palline, per spostarle di questa quantità a ogni frame.
- `color` — ogni pallina riceve un colore.
- `size` — ogni pallina riceve una dimensione, ovvero il suo raggio in pixel.

Questo gestisce le proprietà, ma che dire dei metodi? Occorre che le palline facciano effettivamente qualcosa nel programma.

### Disegnare la pallina

Per prima cosa, aggiungere il seguente metodo `draw()` alla classe `Ball`:

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

Usando questa funzione, è possibile indicare alla pallina di disegnarsi sullo schermo, chiamando una serie di membri del contesto canvas 2D definito in precedenza (`ctx`). Il contesto è come la carta e ora occorre comandare la penna affinché disegni qualcosa su di essa:

- Innanzitutto, si usa [`beginPath()`](/it/docs/Web/API/CanvasRenderingContext2D/beginPath) per dichiarare che si desidera disegnare una forma sulla carta.
- Successivamente, si usa [`fillStyle`](/it/docs/Web/API/CanvasRenderingContext2D/fillStyle) per definire il colore desiderato per la forma: viene impostato sulla proprietà `color` della pallina.
- Quindi, si usa il metodo [`arc()`](/it/docs/Web/API/CanvasRenderingContext2D/arc) per tracciare una forma ad arco sulla carta. I suoi parametri sono:
  - La posizione `x` e `y` del centro dell'arco: vengono specificate le proprietà `x` e `y` della pallina.
  - Il raggio dell'arco: in questo caso, la proprietà `size` della pallina.
  - Gli ultimi due parametri specificano il numero di gradi iniziale e finale della circonferenza tra cui viene disegnato l'arco. Qui vengono specificati 0 gradi e `2 * PI`, equivalenti a 360 gradi in radianti (purtroppo, è necessario specificarlo in radianti). Questo produce un cerchio completo. Specificando soltanto `1 * PI`, si otterrebbe un semicerchio, ovvero 180 gradi.

- Infine, si usa il metodo [`fill()`](/it/docs/Web/API/CanvasRenderingContext2D/fill), che sostanzialmente dichiara: "termina il disegno del percorso iniziato con `beginPath()` e riempi l'area che occupa con il colore specificato in precedenza in `fillStyle`."

È già possibile iniziare a testare l'oggetto.

1. Salvare il codice scritto finora e caricare il file HTML in un browser.
2. Aprire la console JavaScript del browser, quindi aggiornare la pagina affinché la dimensione del canvas cambi in base alla viewport visibile più piccola che rimane quando la console è aperta.
3. Digitare quanto segue per creare una nuova istanza della pallina:

   ```js
   const testBall = new Ball(50, 100, 4, 4, "blue", 10);
   ```

4. Provare a chiamarne i membri:

   ```js
   testBall.x;
   testBall.size;
   testBall.color;
   testBall.draw();
   ```

5. Quando viene inserita l'ultima riga, la pallina dovrebbe disegnarsi in un punto del canvas.

### Aggiornare i dati della pallina

È possibile disegnare la pallina in una posizione, ma per spostarla effettivamente serve una sorta di funzione di aggiornamento. Aggiungere il seguente codice all'interno della definizione della classe `Ball`:

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

Le prime quattro parti della funzione verificano se la pallina ha raggiunto il bordo del canvas. In tal caso, invertono la polarità della velocità pertinente per far viaggiare la pallina nella direzione opposta. Per esempio, se la pallina si stava muovendo verso l'alto (`velY` negativo), allora la velocità verticale viene modificata affinché inizi invece a muoversi verso il basso (`velY` positivo).

Nei quattro casi, viene verificato se:

- la coordinata `x` è maggiore della larghezza del canvas (la pallina sta uscendo dal bordo destro);
- la coordinata `x` è minore di 0 (la pallina sta uscendo dal bordo sinistro);
- la coordinata `y` è maggiore dell'altezza del canvas (la pallina sta uscendo dal bordo inferiore);
- la coordinata `y` è minore di 0 (la pallina sta uscendo dal bordo superiore).

In ogni caso, il calcolo include la `size` della pallina perché le coordinate `x`/`y` si trovano al centro della pallina, mentre si desidera che sia il bordo della pallina a rimbalzare contro il perimetro: non si vuole che la pallina esca per metà dallo schermo prima di iniziare a rimbalzare.

Le ultime due righe aggiungono il valore `velX` alla coordinata `x` e il valore `velY` alla coordinata `y`: in effetti, la pallina viene spostata ogni volta che questo metodo viene chiamato.

Questo basta per ora; passiamo all'animazione.

## Animare la pallina

Ora rendiamo il tutto più divertente. Si inizierà ad aggiungere palline al canvas e ad animarle.

Per prima cosa, occorre creare un luogo in cui memorizzare tutte le palline e poi popolarlo. Il seguente codice svolgerà questo compito: aggiungerlo ora alla fine del codice.

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

Il ciclo `while` crea una nuova istanza di `Ball()` usando valori casuali generati dalle funzioni `random()` e `randomRGB()`, quindi esegue `push()` alla fine dell'array delle palline, ma solo finché il numero di palline nell'array è inferiore a 25. Pertanto, quando l'array contiene 25 palline, non verranno più aggiunte palline. È possibile variare il numero in `balls.length < 25` per ottenere più o meno palline nell'array. A seconda della potenza di elaborazione del computer/browser, specificare diverse migliaia di palline potrebbe rallentare notevolmente l'animazione.

Successivamente, aggiungere quanto segue alla fine del codice:

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

Tutti i programmi che animano elementi generalmente includono un ciclo di animazione, che serve ad aggiornare le informazioni nel programma e quindi a eseguire il rendering della visualizzazione risultante a ogni frame dell'animazione; questa è la base della maggior parte dei giochi e di altri programmi simili. La funzione `loop()` svolge quanto segue:

- Imposta il colore di riempimento del canvas su nero semitrasparente, quindi disegna un rettangolo di quel colore lungo l'intera larghezza e altezza del canvas, usando `fillRect()` (i quattro parametri forniscono una coordinata iniziale e una larghezza e altezza per il rettangolo disegnato). Questo serve a coprire il disegno del frame precedente prima di disegnare quello successivo. Senza questa operazione, invece di palline in movimento si vedrebbero soltanto lunghi serpenti che si snodano nel canvas. Il colore di riempimento è impostato su semitrasparente, `rgb(0 0 0 / 25%)`, per consentire ai frame precedenti di trasparire leggermente, producendo le piccole scie dietro le palline mentre si muovono. Cambiando 0.25 in 1, non sarebbero più visibili. Provare a variare questo numero per osservare l'effetto prodotto.
- Esegue un ciclo su tutte le palline nell'array `balls`, quindi esegue le funzioni `draw()` e `update()` di ogni pallina per disegnarla sullo schermo e applicare gli aggiornamenti necessari a posizione e velocità in vista del frame successivo.
- Esegue nuovamente la funzione usando il metodo `requestAnimationFrame()`: quando questo metodo viene eseguito ripetutamente e gli viene passato lo stesso nome di funzione, esegue tale funzione un determinato numero di volte al secondo per creare un'animazione fluida. In genere viene eseguito ricorsivamente, ossia la funzione chiama sé stessa ogni volta che viene eseguita, continuando a ripetersi.

Infine, aggiungere la riga seguente alla fine del codice: è necessario chiamare la funzione una volta per avviare l'animazione.

```js
loop();
```

Questo è tutto per le basi: provare a salvare e aggiornare la pagina per testare le palline rimbalzanti.

## Aggiungere il rilevamento delle collisioni

Ora, per rendere il tutto più divertente, aggiungiamo il rilevamento delle collisioni al programma, in modo che le palline sappiano quando hanno colpito un'altra pallina.

Per prima cosa, aggiungere la seguente definizione di metodo alla classe `Ball`.

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

Questo metodo è leggermente complesso, quindi non preoccupatevi se per ora non è chiaro esattamente come funziona. Di seguito è riportata una spiegazione:

- Per ogni pallina, occorre controllare tutte le altre palline per vedere se si è verificata una collisione con quella corrente. A tale scopo, viene avviato un altro ciclo `for...of` per scorrere tutte le palline nell'array `balls[]`.
- Subito all'interno del ciclo for, viene utilizzata un'istruzione `if` per verificare se la pallina corrente attraversata dal ciclo è la stessa pallina che si sta controllando. Non si vuole controllare se una pallina è entrata in collisione con sé stessa. Per farlo, viene verificato se la pallina corrente, ossia la pallina il cui metodo collisionDetect viene invocato, è uguale alla pallina del ciclo, ossia quella a cui fa riferimento l'iterazione corrente del ciclo for nel metodo collisionDetect. Viene quindi usato `!` per negare il controllo, in modo che il codice all'interno dell'istruzione `if` venga eseguito solo se non sono **la stessa** pallina.
- Viene quindi usato un algoritmo comune per controllare la collisione di due cerchi. In sostanza, viene verificato se le aree dei due cerchi si sovrappongono. Questo è spiegato più dettagliatamente in [Rilevamento delle collisioni 2D](/it/docs/Games/Techniques/2D_collision_detection).
- Se viene rilevata una collisione, viene eseguito il codice all'interno dell'istruzione `if` interna. In questo caso, viene impostata soltanto la proprietà `color` di entrambi i cerchi su un nuovo colore casuale. Si sarebbe potuto fare qualcosa di molto più complesso, come far rimbalzare realisticamente le palline l'una contro l'altra, ma sarebbe stato molto più complesso da implementare. Per simulazioni fisiche di questo tipo, gli sviluppatori tendono a utilizzare librerie per giochi o fisica come [PhysicsJS](https://wellcaffeinated.net/PhysicsJS/), [matter.js](https://brm.io/matter-js/), [Phaser](https://phaser.io/) e così via.

È inoltre necessario chiamare questo metodo in ogni frame dell'animazione. Aggiornare la funzione `loop()` per chiamare `ball.collisionDetect()` dopo `ball.update()`:

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

Salvare e aggiornare nuovamente la demo: le palline cambieranno colore quando entrano in collisione.

> [!NOTE]
> In caso di problemi nel far funzionare questo esempio, provare a confrontare il codice JavaScript con la nostra [versione completata](https://github.com/mdn/learning-area/blob/main/javascript/oojs/bouncing-balls/main-finished.js) (consultare anche la versione [in esecuzione](https://mdn.github.io/learning-area/javascript/oojs/bouncing-balls/index-finished.html)).

## Riepilogo

Si spera che la scrittura di questo esempio reale di palline casuali rimbalzanti sia stata divertente, utilizzando varie tecniche relative agli oggetti e alla programmazione orientata agli oggetti presenti nel modulo. Questo dovrebbe aver fornito pratica utile nell'uso degli oggetti e un buon contesto reale.

Questo è tutto per le lezioni sugli oggetti: ora resta soltanto da mettere alla prova le proprie competenze nella sfida del modulo.

## Vedi anche

- [Tutorial Canvas](/it/docs/Web/API/Canvas_API/Tutorial) — un tutorial introduttivo sul canvas 2D.
- [requestAnimationFrame()](/it/docs/Web/API/Window/requestAnimationFrame)
- [Rilevamento delle collisioni 2D](/it/docs/Games/Techniques/2D_collision_detection)
- [Rilevamento delle collisioni 3D](/it/docs/Games/Techniques/3D_collision_detection)
- [Gioco breakout 2D con JavaScript puro](/it/docs/Games/Tutorials/2D_Breakout_game_pure_JavaScript) — un ottimo tutorial introduttivo che mostra come creare un gioco 2D.
- [Gioco breakout 2D con Phaser](/it/docs/Games/Tutorials/2D_breakout_game_Phaser) — spiega le basi della creazione di un gioco 2D usando una libreria JavaScript per giochi.

{{PreviousMenuNext("Learn_web_development/Extensions/Advanced_JavaScript_objects/Test_your_skills/Object-oriented_JavaScript", "Learn_web_development/Extensions/Advanced_JavaScript_objects/Adding_bouncing_balls_features", "Learn_web_development/Extensions/Advanced_JavaScript_objects")}}
