---
title: API per video e audio
short-title: Video e audio
slug: Learn_web_development/Extensions/Client-side_APIs/Video_and_audio_APIs
l10n:
  sourceCommit: 4c58f4735f986a91bee1b77e336143630df727a2
---

{{PreviousMenuNext("Learn_web_development/Extensions/Client-side_APIs/Introduction", "Learn_web_development/Extensions/Client-side_APIs/Drawing_graphics", "Learn_web_development/Extensions/Client-side_APIs")}}

HTML include elementi per incorporare contenuti multimediali avanzati nei documenti — {{htmlelement("video")}} e {{htmlelement("audio")}} — che a loro volta dispongono di API proprie per controllare la riproduzione, lo spostamento nella timeline e così via. Questo articolo mostra come svolgere attività comuni, come creare controlli di riproduzione personalizzati.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Conoscenza di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, in particolare delle <a href="/it/docs/Learn_web_development/Core/Scripting/Object_basics">basi degli oggetti JavaScript</a> e delle API fondamentali, come lo <a href="/it/docs/Learn_web_development/Core/Scripting/DOM_scripting">scripting del DOM</a> e le <a href="/it/docs/Learn_web_development/Core/Scripting/Network_requests">richieste di rete</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        <ul>
          <li>Comprendere cosa sono i codec e i diversi formati video e audio.</li>
          <li>Comprendere le funzionalità principali associate ad audio e video — riproduzione, pausa, arresto, spostamento indietro e avanti, durata e tempo corrente.</li>
          <li>Usare l'API <code>HTMLMediaElement</code> per creare un lettore multimediale personalizzato di base, per una migliore accessibilità o maggiore coerenza tra browser.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Video e audio HTML

Gli elementi {{htmlelement("video")}} e {{htmlelement("audio")}} consentono di incorporare video e audio nelle pagine web. Come mostrato in [Video e audio HTML](/it/docs/Learn_web_development/Core/Structuring_content/HTML_video_and_audio), un'implementazione tipica ha questo aspetto:

```html
<video controls>
  <source src="rabbit320.mp4" type="video/mp4" />
  <source src="rabbit320.webm" type="video/webm" />
  <p>
    Your browser doesn't support HTML video. Here is a
    <a href="rabbit320.mp4">link to the video</a> instead.
  </p>
</video>
```

Questo crea un lettore video nel browser come segue:

```html hidden live-sample___multiple-formats
<h1>Below is a video that will play in all modern browsers</h1>

<video controls>
  <source
    src="https://mdn.github.io/learning-area/html/multimedia-and-embedding/video-and-audio-content/rabbit320.mp4"
    type="video/mp4" />
  <source
    src="https://mdn.github.io/learning-area/html/multimedia-and-embedding/video-and-audio-content/rabbit320.webm"
    type="video/webm" />
</video>
```

{{EmbedLiveSample("multiple-formats", '100%', 380)}}

È possibile rivedere il funzionamento di tutte le funzionalità HTML nell'articolo collegato sopra; per gli scopi di questo articolo, l'attributo più interessante è [`controls`](/it/docs/Web/HTML/Reference/Elements/video#controls), che abilita l'insieme predefinito di controlli per la riproduzione. Se non viene specificato, non si ottengono controlli di riproduzione:

```html hidden live-sample___multiple-formats-no-controls
<h1>Below is a video that will play in all modern browsers</h1>

<video>
  <source
    src="https://mdn.github.io/learning-area/html/multimedia-and-embedding/video-and-audio-content/rabbit320.mp4"
    type="video/mp4" />
  <source
    src="https://mdn.github.io/learning-area/html/multimedia-and-embedding/video-and-audio-content/rabbit320.webm"
    type="video/webm" />
</video>
```

{{EmbedLiveSample("multiple-formats-no-controls", '100%', 380)}}

Questo non è altrettanto immediatamente utile per la riproduzione video, ma presenta dei vantaggi. Un problema importante dei controlli nativi del browser è che sono diversi in ogni browser, il che non è ideale per il supporto cross-browser. Un altro problema importante è che i controlli nativi della maggior parte dei browser non sono molto accessibili tramite tastiera.

Entrambi i problemi possono essere risolti nascondendo i controlli nativi, rimuovendo l'attributo `controls`, e programmando controlli propri con HTML, CSS e JavaScript. Nella sezione successiva verranno esaminati gli strumenti di base disponibili per farlo.

## L'API HTMLMediaElement

Parte della specifica HTML, l'API [`HTMLMediaElement`](/it/docs/Web/API/HTMLMediaElement) fornisce funzionalità che consentono di controllare programmaticamente i lettori video e audio, ad esempio [`HTMLMediaElement.play()`](/it/docs/Web/API/HTMLMediaElement/play), [`HTMLMediaElement.pause()`](/it/docs/Web/API/HTMLMediaElement/pause) e così via. Questa interfaccia è disponibile sia per gli elementi {{htmlelement("audio")}} sia per {{htmlelement("video")}}, poiché le funzionalità da implementare sono pressoché identiche. Esaminiamo un esempio, aggiungendo funzionalità man mano.

L'esempio completato avrà un aspetto, e un funzionamento, simile al seguente:

```html hidden live-sample___custom-video-player
<div class="player">
  <video controls>
    <source src="/shared-assets/videos/sintel-short.mp4" type="video/mp4" />
    <source src="/shared-assets/videos/sintel-short.webm" type="video/webm" />
  </video>
  <div class="controls">
    <button class="play" data-icon="P" aria-label="play pause toggle"></button>
    <button class="stop" data-icon="S" aria-label="stop"></button>
    <div class="timer">
      <div></div>
      <span>00:00</span>
    </div>
    <button class="rwd" data-icon="B" aria-label="rewind"></button>
    <button class="fwd" data-icon="F" aria-label="fast forward"></button>
  </div>
</div>
<p>
  Sintel &copy; copyright Blender Foundation |
  <a href="https://studio.blender.org/films/sintel/"
    >studio.blender.org/films/sintel/</a
  >.
</p>
```

```css hidden live-sample___custom-video-player
body {
  overflow: hidden;
}

@font-face {
  font-family: "HeydingsControlsRegular";
  src: url("https://mdn.github.io/learning-area/javascript/apis/video-audio/finished/fonts/heydings_controls-webfont.eot");
  src:
    url("https://mdn.github.io/learning-area/javascript/apis/video-audio/finished/fonts/heydings_controls-webfont.eot?#iefix")
      format("embedded-opentype"),
    url("https://mdn.github.io/learning-area/javascript/apis/video-audio/finished/fonts/heydings_controls-webfont.woff")
      format("woff"),
    url("https://mdn.github.io/learning-area/javascript/apis/video-audio/finished/fonts/heydings_controls-webfont.ttf")
      format("truetype");
  font-weight: normal;
  font-style: normal;
}

video {
  border: 1px solid black;
}

p {
  position: absolute;
  top: 310px;
}

.player {
  position: absolute;
}

.controls {
  visibility: hidden;
  opacity: 0.5;
  width: 400px;
  border-radius: 10px;
  position: absolute;
  bottom: 20px;
  left: 50%;
  margin-left: -200px;
  background-color: black;
  box-shadow: 3px 3px 5px black;
  transition: 1s all;
  display: flex;
}

.player:hover .controls,
.player:focus-within .controls {
  opacity: 1;
}

button,
.controls {
  background: linear-gradient(to bottom, #222222, #666666);
}

button::before {
  font-family: "HeydingsControlsRegular";
  font-size: 20px;
  position: relative;
  content: attr(data-icon);
  color: #aaaaaa;
  text-shadow: 1px 1px 0px black;
}

.play::before {
  font-size: 22px;
}

button,
.timer {
  height: 38px;
  line-height: 19px;
  box-shadow: inset 0 -5px 25px #0000004d;
  border-right: 1px solid #333333;
}

button {
  position: relative;
  border: 0;
  flex: 1;
  outline: none;
}

.play {
  border-radius: 10px 0 0 10px;
}

.fwd {
  border-radius: 0 10px 10px 0;
}

.timer {
  line-height: 38px;
  font-size: 10px;
  font-family: monospace;
  text-shadow: 1px 1px 0px black;
  color: white;
  flex: 5;
  position: relative;
}

.timer div {
  position: absolute;
  background-color: rgb(255 255 255 / 20%);
  left: 0;
  top: 0;
  width: 0;
  height: 38px;
  z-index: 2;
}

.timer span {
  position: absolute;
  z-index: 3;
  left: 19px;
}

button:hover,
button:focus {
  box-shadow: inset 1px 1px 2px black;
}

button:active {
  box-shadow: inset 3px 3px 2px black;
}

.active::before {
  color: red;
}
```

```js hidden live-sample___custom-video-player
const media = document.querySelector("video");
const controls = document.querySelector(".controls");

const play = document.querySelector(".play");
const stop = document.querySelector(".stop");
const rwd = document.querySelector(".rwd");
const fwd = document.querySelector(".fwd");

const timerWrapper = document.querySelector(".timer");
const timer = document.querySelector(".timer span");
const timerBar = document.querySelector(".timer div");

media.removeAttribute("controls");
controls.style.visibility = "visible";

play.addEventListener("click", playPauseMedia);
stop.addEventListener("click", stopMedia);
media.addEventListener("ended", stopMedia);
rwd.addEventListener("click", mediaBackward);
fwd.addEventListener("click", mediaForward);
media.addEventListener("timeupdate", setTime);

let intervalFwd;
let intervalRwd;

function playPauseMedia() {
  rwd.classList.remove("active");
  fwd.classList.remove("active");
  clearInterval(intervalRwd);
  clearInterval(intervalFwd);
  if (media.paused) {
    play.setAttribute("data-icon", "u");
    media.play();
  } else {
    play.setAttribute("data-icon", "P");
    media.pause();
  }
}

function stopMedia() {
  rwd.classList.remove("active");
  fwd.classList.remove("active");
  media.pause();
  media.currentTime = 0;
  clearInterval(intervalRwd);
  clearInterval(intervalFwd);
  play.setAttribute("data-icon", "P");
}

function mediaBackward() {
  clearInterval(intervalFwd);
  fwd.classList.remove("active");

  if (rwd.classList.contains("active")) {
    rwd.classList.remove("active");
    clearInterval(intervalRwd);
    media.play();
  } else {
    rwd.classList.add("active");
    media.pause();
    intervalRwd = setInterval(windBackward, 200);
  }
}

function mediaForward() {
  clearInterval(intervalRwd);
  rwd.classList.remove("active");

  if (fwd.classList.contains("active")) {
    fwd.classList.remove("active");
    clearInterval(intervalFwd);
    media.play();
  } else {
    fwd.classList.add("active");
    media.pause();
    intervalFwd = setInterval(windForward, 200);
  }
}

function windBackward() {
  if (media.currentTime <= 3) {
    rwd.classList.remove("active");
    clearInterval(intervalRwd);
    stopMedia();
  } else {
    media.currentTime -= 3;
  }
}

function windForward() {
  if (media.currentTime >= media.duration - 3) {
    fwd.classList.remove("active");
    clearInterval(intervalFwd);
    stopMedia();
  } else {
    media.currentTime += 3;
  }
}

function setTime() {
  const minutes = Math.floor(media.currentTime / 60);
  const seconds = Math.floor(media.currentTime - minutes * 60);

  const minuteValue = minutes.toString().padStart(2, "0");
  const secondValue = seconds.toString().padStart(2, "0");

  const mediaTime = `${minuteValue}:${secondValue}`;
  timer.textContent = mediaTime;

  const barLength =
    timerWrapper.clientWidth * (media.currentTime / media.duration);
  timerBar.style.width = `${barLength}px`;
}
```

{{EmbedLiveSample("custom-video-player", '100%', 360)}}

### Per iniziare

Per iniziare con questo esempio, seguire questi passaggi:

1. Creare una nuova directory sul disco rigido chiamata `custom-video-player`.
2. Creare al suo interno un nuovo file chiamato `index.html` e riempirlo con il seguente contenuto:

   ```html
   <!doctype html>
   <html lang="en-gb">
     <head>
       <meta charset="utf-8" />
       <meta name="viewport" content="width=device-width" />
       <title>Video player example</title>
       <link rel="stylesheet" type="text/css" href="style.css" />
     </head>
     <body>
       <div class="player">
         <video controls>
           <source
             src="/shared-assets/videos/sintel-short.mp4"
             type="video/mp4" />
           <source
             src="/shared-assets/videos/sintel-short.webm"
             type="video/webm" />
         </video>
         <div class="controls">
           <button
             class="play"
             data-icon="P"
             aria-label="play pause toggle"></button>
           <button class="stop" data-icon="S" aria-label="stop"></button>
           <div class="timer">
             <div></div>
             <span>00:00</span>
           </div>
           <button class="rwd" data-icon="B" aria-label="rewind"></button>
           <button class="fwd" data-icon="F" aria-label="fast forward"></button>
         </div>
       </div>
       <p>
         Sintel &copy; copyright Blender Foundation |
         <a href="https://studio.blender.org/films/sintel/"
           >studio.blender.org/films/sintel/</a
         >.
       </p>
       <script src="custom-player.js"></script>
     </body>
   </html>
   ```

3. Creare un altro nuovo file al suo interno chiamato `style.css` e riempirlo con il seguente contenuto:

   ```css
   @font-face {
     font-family: "HeydingsControlsRegular";
     src: url("https://mdn.github.io/learning-area/javascript/apis/video-audio/finished/fonts/heydings_controls-webfont.eot");
     src:
       url("https://mdn.github.io/learning-area/javascript/apis/video-audio/finished/fonts/heydings_controls-webfont.eot?#iefix")
         format("embedded-opentype"),
       url("https://mdn.github.io/learning-area/javascript/apis/video-audio/finished/fonts/heydings_controls-webfont.woff")
         format("woff"),
       url("https://mdn.github.io/learning-area/javascript/apis/video-audio/finished/fonts/heydings_controls-webfont.ttf")
         format("truetype");
     font-weight: normal;
     font-style: normal;
   }

   video {
     border: 1px solid black;
   }

   p {
     position: absolute;
     top: 310px;
   }

   .player {
     position: absolute;
   }

   .controls {
     visibility: hidden;
     opacity: 0.5;
     width: 400px;
     border-radius: 10px;
     position: absolute;
     bottom: 20px;
     left: 50%;
     margin-left: -200px;
     background-color: black;
     box-shadow: 3px 3px 5px black;
     transition: 1s all;
     display: flex;
   }

   .player:hover .controls,
   .player:focus-within .controls {
     opacity: 1;
   }

   button,
   .controls {
     background: linear-gradient(to bottom, #222222, #666666);
   }

   button::before {
     font-family: "HeydingsControlsRegular";
     font-size: 20px;
     position: relative;
     content: attr(data-icon);
     color: #aaaaaa;
     text-shadow: 1px 1px 0px black;
   }

   .play::before {
     font-size: 22px;
   }

   button,
   .timer {
     height: 38px;
     line-height: 19px;
     box-shadow: inset 0 -5px 25px #0000004d;
     border-right: 1px solid #333333;
   }

   button {
     position: relative;
     border: 0;
     flex: 1;
     outline: none;
   }

   .play {
     border-radius: 10px 0 0 10px;
   }

   .fwd {
     border-radius: 0 10px 10px 0;
   }

   .timer {
     line-height: 38px;
     font-size: 10px;
     font-family: monospace;
     text-shadow: 1px 1px 0px black;
     color: white;
     flex: 5;
     position: relative;
   }

   .timer div {
     position: absolute;
     background-color: rgb(255 255 255 / 20%);
     left: 0;
     top: 0;
     width: 0;
     height: 38px;
     z-index: 2;
   }

   .timer span {
     position: absolute;
     z-index: 3;
     left: 19px;
   }

   button:hover,
   button:focus {
     box-shadow: inset 1px 1px 2px black;
   }

   button:active {
     box-shadow: inset 3px 3px 2px black;
   }

   .active::before {
     color: red;
   }
   ```

4. Creare un altro nuovo file nella directory chiamato `custom-player.js`. Per ora lasciarlo vuoto.

A questo punto, caricando l'HTML dovrebbe essere visibile un normale lettore video HTML, con i controlli nativi visualizzati.

#### Esplorare l'HTML

Aprire il file indice HTML. Saranno visibili diverse funzionalità; l'HTML è dominato dal lettore video e dai relativi controlli:

- L'intero lettore è racchiuso in un elemento {{htmlelement("div")}}, in modo che possa essere stilizzato come un'unica unità, se necessario.
- L'elemento {{htmlelement("video")}} contiene due elementi {{htmlelement("source")}}, affinché possano essere caricati formati diversi a seconda del browser che visualizza il sito.
- L'HTML dei controlli è probabilmente la parte più interessante:
  - Sono presenti quattro {{htmlelement("button")}}: riproduzione/pausa, arresto, riavvolgimento e avanzamento rapido.
  - Ogni `<button>` ha un nome `class`, un attributo `data-icon` per definire quale icona debba essere visualizzata su ciascun pulsante, verrà mostrato come funziona nella sezione seguente, e un attributo `aria-label` per fornire una descrizione comprensibile di ciascun pulsante, poiché non viene fornita un'etichetta leggibile all'interno dei tag. Il contenuto degli attributi `aria-label` viene letto dagli screen reader quando gli utenti mettono lo stato attivo sugli elementi che li contengono.
  - È presente anche un {{htmlelement("div")}} per il timer, che riporterà il tempo trascorso durante la riproduzione del video. Per divertimento, vengono forniti due meccanismi di visualizzazione: uno {{htmlelement("span")}} contenente il tempo trascorso in minuti e secondi e un ulteriore `<div>` che verrà usato per creare una barra indicatrice orizzontale che si allunga con il passare del tempo.

#### Esplorare il CSS

Ora aprire il file CSS e osservarne il contenuto. Il CSS dell'esempio non è troppo complicato, ma qui verranno evidenziate le parti più interessanti. Prima di tutto, notare lo stile di `.controls`:

```css
.controls {
  visibility: hidden;
  opacity: 0.5;
  width: 400px;
  border-radius: 10px;
  position: absolute;
  bottom: 20px;
  left: 50%;
  margin-left: -200px;
  background-color: black;
  box-shadow: 3px 3px 5px black;
  transition: 1s all;
  display: flex;
}

.player:hover .controls,
.player:focus-within .controls {
  opacity: 1;
}
```

- Inizialmente, la {{cssxref("visibility")}} dei controlli personalizzati è impostata su `hidden`. Successivamente, nel JavaScript, i controlli verranno impostati su `visible` e l'attributo `controls` verrà rimosso dall'elemento `<video>`. In questo modo, se JavaScript non viene caricato per qualche motivo, gli utenti possono comunque utilizzare il video con i controlli nativi.
- Ai controlli viene assegnata una {{cssxref("opacity")}} predefinita di `0.5`, affinché risultino meno distraenti durante la visione del video. Solo passando il puntatore sopra il lettore o portandovi lo stato attivo, i controlli appaiono con opacità completa.
- I pulsanti all'interno della barra di controllo vengono disposti usando flexbox ({{cssxref("display")}}: flex), per semplificare il layout.

Successivamente, osserviamo le icone dei pulsanti:

```css
@font-face {
  font-family: "HeydingsControlsRegular";
  src: url("https://mdn.github.io/learning-area/javascript/apis/video-audio/finished/fonts/heydings_controls-webfont.eot");
  src:
    url("https://mdn.github.io/learning-area/javascript/apis/video-audio/finished/fonts/heydings_controls-webfont.eot?#iefix")
      format("embedded-opentype"),
    url("https://mdn.github.io/learning-area/javascript/apis/video-audio/finished/fonts/heydings_controls-webfont.woff")
      format("woff"),
    url("https://mdn.github.io/learning-area/javascript/apis/video-audio/finished/fonts/heydings_controls-webfont.ttf")
      format("truetype");
  font-weight: normal;
  font-style: normal;
}

button::before {
  font-family: "HeydingsControlsRegular";
  font-size: 20px;
  position: relative;
  content: attr(data-icon);
  color: #aaaaaa;
  text-shadow: 1px 1px 0px black;
}
```

Prima di tutto, nella parte superiore del CSS viene usato un blocco {{cssxref("@font-face")}} per importare un font web personalizzato. Si tratta di un font di icone: tutti i caratteri dell'alfabeto corrispondono a icone comuni che potrebbero essere utilizzate in un'applicazione.

Successivamente, viene usato contenuto generato per visualizzare un'icona su ciascun pulsante:

- Viene usato il selettore {{cssxref("::before")}} per visualizzare il contenuto prima di ogni elemento {{htmlelement("button")}}.
- Viene usata la proprietà {{cssxref("content")}} per impostare il contenuto da visualizzare in ogni caso in modo che sia uguale al contenuto dell'attributo [`data-icon`](/it/docs/Web/HTML/How_to/Use_data_attributes). Nel caso del pulsante di riproduzione, `data-icon` contiene una "P" maiuscola.
- Il font web personalizzato viene applicato ai pulsanti usando {{cssxref("font-family")}}. In questo font, "P" è in realtà un'icona di riproduzione, quindi il pulsante di riproduzione mostra un'icona di riproduzione.

I font di icone sono utili per molti motivi: riducono le richieste HTTP perché non è necessario scaricare quelle icone come file immagine, offrono un'eccellente scalabilità e possono essere stilizzati usando proprietà di testo, come {{cssxref("color")}} e {{cssxref("text-shadow")}}.

Infine, osserviamo il CSS per il timer:

```css
.timer {
  line-height: 38px;
  font-size: 10px;
  font-family: monospace;
  text-shadow: 1px 1px 0px black;
  color: white;
  flex: 5;
  position: relative;
}

.timer div {
  position: absolute;
  background-color: rgb(255 255 255 / 20%);
  left: 0;
  top: 0;
  width: 0;
  height: 38px;
  z-index: 2;
}

.timer span {
  position: absolute;
  z-index: 3;
  left: 19px;
}
```

- L'elemento esterno `.timer` è impostato con `flex: 5`, in modo che occupi gran parte della larghezza della barra dei controlli. Viene inoltre assegnato {{cssxref("position", "position: relative")}}, in modo da poter posizionare comodamente gli elementi al suo interno rispetto ai suoi limiti, anziché ai limiti dell'elemento {{htmlelement("body")}}.
- Il `<div>` interno è posizionato in modo assoluto per trovarsi direttamente sopra il `<div>` esterno. Gli viene inoltre assegnata una larghezza iniziale pari a 0, quindi non è visibile. Durante la riproduzione del video, la larghezza verrà aumentata tramite JavaScript con il trascorrere del video.
- Anche lo `<span>` è posizionato in modo assoluto, vicino al lato sinistro della barra del timer.
- Al `<div>` interno e allo `<span>` viene inoltre assegnato il valore corretto di {{cssxref("z-index")}}, in modo che il timer venga visualizzato sopra e il `<div>` interno sotto. In questo modo, tutte le informazioni restano visibili e un riquadro non ne oscura un altro.

### Implementare JavaScript

L'interfaccia HTML e CSS è già piuttosto completa; ora occorre soltanto collegare tutti i pulsanti affinché i controlli funzionino.

1. All'inizio del file `custom-player.js`, inserire il seguente codice:

   ```js
   const media = document.querySelector("video");
   const controls = document.querySelector(".controls");

   const play = document.querySelector(".play");
   const stop = document.querySelector(".stop");
   const rwd = document.querySelector(".rwd");
   const fwd = document.querySelector(".fwd");

   const timerWrapper = document.querySelector(".timer");
   const timer = document.querySelector(".timer span");
   const timerBar = document.querySelector(".timer div");
   ```

   Qui vengono create costanti per contenere riferimenti a tutti gli oggetti da manipolare. Sono presenti tre gruppi:
   - L'elemento `<video>` e la barra dei controlli.
   - I pulsanti riproduzione/pausa, arresto, riavvolgimento e avanzamento rapido.
   - Il `<div>` contenitore esterno del timer, lo `<span>` con la visualizzazione digitale del timer e il `<div>` interno che si allarga con il trascorrere del tempo.

2. Successivamente, inserire quanto segue alla fine del codice:

   ```js
   media.removeAttribute("controls");
   controls.style.visibility = "visible";
   ```

   Queste due righe rimuovono i controlli predefiniti del browser dal video e rendono visibili i controlli personalizzati.

#### Riprodurre e mettere in pausa il video

Implementiamo probabilmente il controllo più importante: il pulsante riproduzione/pausa.

1. Prima di tutto, aggiungere quanto segue alla fine del codice, affinché la funzione `playPauseMedia()` venga invocata quando viene fatto clic sul pulsante di riproduzione:

   ```js
   play.addEventListener("click", playPauseMedia);
   ```

2. Ora definiamo `playPauseMedia()`: aggiungere quanto segue, ancora una volta alla fine del codice:

   ```js
   function playPauseMedia() {
     if (media.paused) {
       play.setAttribute("data-icon", "u");
       media.play();
     } else {
       play.setAttribute("data-icon", "P");
       media.pause();
     }
   }
   ```

   Qui viene usata un'istruzione [`if`](/it/docs/Web/JavaScript/Reference/Statements/if...else) per verificare se il video è in pausa. La proprietà [`HTMLMediaElement.paused`](/it/docs/Web/API/HTMLMediaElement/paused) restituisce true se il contenuto multimediale è in pausa, ovvero ogni volta che il video non è in riproduzione, incluso quando è impostato alla durata 0 dopo il primo caricamento. Se è in pausa, il valore dell'attributo `data-icon` sul pulsante di riproduzione viene impostato su "u", che corrisponde a un'icona di pausa, e viene invocato il metodo [`HTMLMediaElement.play()`](/it/docs/Web/API/HTMLMediaElement/play) per riprodurre il contenuto multimediale.

   Al secondo clic, il pulsante verrà nuovamente commutato: verrà mostrata di nuovo l'icona di riproduzione e il video verrà messo in pausa con [`HTMLMediaElement.pause()`](/it/docs/Web/API/HTMLMediaElement/pause).

#### Arrestare il video

1. Successivamente, aggiungiamo la funzionalità per gestire l'arresto del video. Aggiungere le seguenti righe [`addEventListener()`](/it/docs/Web/API/EventTarget/addEventListener) sotto quella aggiunta in precedenza:

   ```js
   stop.addEventListener("click", stopMedia);
   media.addEventListener("ended", stopMedia);
   ```

   L'evento [`click`](/it/docs/Web/API/Element/click_event) è evidente: si desidera arrestare il video eseguendo la funzione `stopMedia()` quando viene fatto clic sul pulsante di arresto. Tuttavia, si desidera arrestare il video anche al termine della riproduzione: questo è indicato dall'attivazione dell'evento [`ended`](/it/docs/Web/API/HTMLMediaElement/ended_event), quindi viene impostato un listener per eseguire la funzione anche quando tale evento si verifica.

2. Successivamente, definiamo `stopMedia()`: aggiungere la seguente funzione sotto `playPauseMedia()`:

   ```js
   function stopMedia() {
     media.pause();
     media.currentTime = 0;
     play.setAttribute("data-icon", "P");
   }
   ```

   Nell'API HTMLMediaElement non esiste un metodo `stop()`: l'equivalente consiste nel mettere in `pause()` il video e impostare la proprietà [`currentTime`](/it/docs/Web/API/HTMLMediaElement/currentTime) a 0. Impostare `currentTime` su un valore, in secondi, sposta immediatamente il contenuto multimediale a quella posizione.

   Dopo questo, resta soltanto da impostare l'icona visualizzata sull'icona di riproduzione. Indipendentemente dal fatto che il video fosse in pausa o in riproduzione quando viene premuto il pulsante di arresto, deve essere pronto per la riproduzione successiva.

#### Spostarsi indietro e avanti

Esistono molti modi per implementare la funzionalità di riavvolgimento e avanzamento rapido; qui viene mostrato un metodo relativamente complesso, che non si interrompe quando i diversi pulsanti vengono premuti in un ordine imprevisto.

1. Prima di tutto, aggiungere le seguenti due righe [`addEventListener()`](/it/docs/Web/API/EventTarget/addEventListener) sotto le precedenti:

   ```js
   rwd.addEventListener("click", mediaBackward);
   fwd.addEventListener("click", mediaForward);
   ```

2. Ora passiamo alle funzioni di gestione degli eventi: aggiungere il seguente codice sotto le funzioni precedenti per definire `mediaBackward()` e `mediaForward()`:

   ```js
   let intervalFwd;
   let intervalRwd;

   function mediaBackward() {
     clearInterval(intervalFwd);
     fwd.classList.remove("active");

     if (rwd.classList.contains("active")) {
       rwd.classList.remove("active");
       clearInterval(intervalRwd);
       media.play();
     } else {
       rwd.classList.add("active");
       media.pause();
       intervalRwd = setInterval(windBackward, 200);
     }
   }

   function mediaForward() {
     clearInterval(intervalRwd);
     rwd.classList.remove("active");

     if (fwd.classList.contains("active")) {
       fwd.classList.remove("active");
       clearInterval(intervalFwd);
       media.play();
     } else {
       fwd.classList.add("active");
       media.pause();
       intervalFwd = setInterval(windForward, 200);
     }
   }
   ```

   Si noterà che inizialmente vengono inizializzate due variabili, `intervalFwd` e `intervalRwd`; il loro scopo sarà illustrato più avanti.

   Esaminiamo `mediaBackward()`; la funzionalità di `mediaForward()` è esattamente identica, ma inversa:
   1. Vengono rimosse tutte le classi e gli intervalli impostati sulla funzionalità di avanzamento rapido. Questo viene fatto perché, se si preme il pulsante `rwd` dopo aver premuto il pulsante `fwd`, si desidera annullare qualsiasi funzionalità di avanzamento rapido e sostituirla con la funzionalità di riavvolgimento. Se si tentasse di eseguire entrambe le operazioni contemporaneamente, il lettore non funzionerebbe correttamente.
   2. Viene usata un'istruzione `if` per verificare se la classe `active` è stata impostata sul pulsante `rwd`, indicando che è già stato premuto. [`classList`](/it/docs/Web/API/Element/classList) è una proprietà molto utile disponibile su ogni elemento: contiene un elenco di tutte le classi impostate sull'elemento, nonché metodi per aggiungere o rimuovere classi e così via. Viene usato il metodo `classList.contains()` per verificare se l'elenco contiene la classe `active`. Questo restituisce un risultato booleano `true`/`false`.
   3. Se `active` è stata impostata sul pulsante `rwd`, viene rimossa usando `classList.remove()`, viene cancellato l'intervallo impostato quando il pulsante è stato premuto la prima volta, come spiegato di seguito, e viene usato [`HTMLMediaElement.play()`](/it/docs/Web/API/HTMLMediaElement/play) per annullare il riavvolgimento e avviare normalmente la riproduzione del video.
   4. Se non è ancora stata impostata, la classe `active` viene aggiunta al pulsante `rwd` usando `classList.add()`, il video viene messo in pausa usando [`HTMLMediaElement.pause()`](/it/docs/Web/API/HTMLMediaElement/pause), quindi la variabile `intervalRwd` viene impostata uguale a una chiamata [`setInterval()`](/it/docs/Web/API/Window/setInterval). Quando viene invocata, `setInterval()` crea un intervallo attivo, ovvero esegue la funzione fornita come primo parametro ogni x millisecondi, dove x è il valore del secondo parametro. Qui viene quindi eseguita la funzione `windBackward()` ogni 200 millisecondi: questa funzione verrà usata per riavvolgere costantemente il video. Per interrompere l'esecuzione di [`setInterval()`](/it/docs/Web/API/Window/setInterval), occorre chiamare [`clearInterval()`](/it/docs/Web/API/Window/clearInterval), fornendogli il nome identificativo dell'intervallo da cancellare, che in questo caso è il nome della variabile `intervalRwd`, come nella chiamata `clearInterval()` precedente nella funzione.

3. Infine, occorre definire le funzioni `windBackward()` e `windForward()` invocate nelle chiamate `setInterval()`. Aggiungere quanto segue sotto le due funzioni precedenti:

   ```js
   function windBackward() {
     if (media.currentTime <= 3) {
       rwd.classList.remove("active");
       clearInterval(intervalRwd);
       stopMedia();
     } else {
       media.currentTime -= 3;
     }
   }

   function windForward() {
     if (media.currentTime >= media.duration - 3) {
       fwd.classList.remove("active");
       clearInterval(intervalFwd);
       stopMedia();
     } else {
       media.currentTime += 3;
     }
   }
   ```

   Anche qui verrà esaminata soltanto la prima di queste funzioni, poiché funzionano in modo quasi identico ma inverso. In `windBackward()` avviene quanto segue: tenere presente che, quando l'intervallo è attivo, questa funzione viene eseguita una volta ogni 200 millisecondi.
   1. Si inizia con un'istruzione `if` che verifica se il tempo corrente è inferiore a 3 secondi, cioè se il riavvolgimento di altri tre secondi lo porterebbe oltre l'inizio del video. Ciò causerebbe un comportamento anomalo, quindi in questo caso si interrompe la riproduzione del video chiamando `stopMedia()`, si rimuove la classe `active` dal pulsante di riavvolgimento e si cancella l'intervallo `intervalRwd` per interrompere la funzionalità di riavvolgimento. Senza quest'ultimo passaggio, il video continuerebbe semplicemente a riavvolgersi per sempre.
   2. Se il tempo corrente non si trova entro 3 secondi dall'inizio del video, vengono sottratti tre secondi dal tempo corrente eseguendo `media.currentTime -= 3`. In pratica, il video viene riavvolto di 3 secondi una volta ogni 200 millisecondi.

#### Aggiornare il tempo trascorso

L'ultima parte del lettore multimediale da implementare è la visualizzazione del tempo trascorso. Per farlo, verrà eseguita una funzione che aggiorna le visualizzazioni del tempo ogni volta che viene attivato l'evento [`timeupdate`](/it/docs/Web/API/HTMLMediaElement/timeupdate_event) sull'elemento `<video>`. La frequenza di attivazione di questo evento dipende dal browser, dalla potenza della CPU e così via ([consultare questo post su Stack Overflow](https://stackoverflow.com/questions/9678177/how-often-does-the-timeupdate-event-fire-for-an-html5-video)).

1. Aggiungere la seguente riga `addEventListener()` subito sotto le altre:

   ```js
   media.addEventListener("timeupdate", setTime);
   ```

2. Ora definiamo la funzione `setTime()`. Aggiungere quanto segue alla fine del file:

   ```js
   function setTime() {
     const minutes = Math.floor(media.currentTime / 60);
     const seconds = Math.floor(media.currentTime - minutes * 60);

     const minuteValue = minutes.toString().padStart(2, "0");
     const secondValue = seconds.toString().padStart(2, "0");

     const mediaTime = `${minuteValue}:${secondValue}`;
     timer.textContent = mediaTime;

     const barLength =
       timerWrapper.clientWidth * (media.currentTime / media.duration);
     timerBar.style.width = `${barLength}px`;
   }
   ```

Questa è una funzione piuttosto lunga, quindi esaminiamola passo dopo passo:

1. Prima di tutto, viene calcolato il numero di minuti e secondi nel valore [`HTMLMediaElement.currentTime`](/it/docs/Web/API/HTMLMediaElement/currentTime).
2. Vengono quindi inizializzate altre due variabili: `minuteValue` e `secondValue`. Viene usato {{jsxref("String/padStart", "padStart()")}} per rendere ogni valore lungo 2 caratteri, anche se il valore numerico è composto da una sola cifra.
3. Il valore effettivo del tempo da visualizzare viene impostato come `minuteValue` più il carattere due punti più `secondValue`.
4. Il valore [`Node.textContent`](/it/docs/Web/API/Node/textContent) del timer viene impostato sul valore temporale, affinché venga visualizzato nell'interfaccia utente.
5. La lunghezza da impostare per il `<div>` interno viene calcolata prima determinando la larghezza del `<div>` esterno, la proprietà [`clientWidth`](/it/docs/Web/API/Element/clientWidth) di qualsiasi elemento contiene la sua larghezza, e poi moltiplicandola per [`HTMLMediaElement.currentTime`](/it/docs/Web/API/HTMLMediaElement/currentTime) diviso per la [`HTMLMediaElement.duration`](/it/docs/Web/API/HTMLMediaElement/duration) totale del contenuto multimediale.
6. La larghezza del `<div>` interno viene impostata uguale alla lunghezza calcolata della barra, più "px", in modo che venga impostata a quel numero di pixel.

#### Correggere riproduzione e pausa

Resta un problema da correggere. Se vengono premuti i pulsanti riproduzione/pausa o arresto mentre è attiva la funzionalità di riavvolgimento o avanzamento rapido, semplicemente non funzionano. Come è possibile correggere il comportamento affinché annullino la funzionalità del pulsante `rwd`/`fwd` e riproducano o arrestino il video come previsto? La correzione è piuttosto semplice.

1. Prima di tutto, aggiungere le seguenti righe all'interno della funzione `stopMedia()`: possono essere inserite ovunque.

   ```js
   rwd.classList.remove("active");
   fwd.classList.remove("active");
   clearInterval(intervalRwd);
   clearInterval(intervalFwd);
   ```

2. Ora aggiungere di nuovo le stesse righe, all'inizio della funzione `playPauseMedia()` appena prima dell'inizio dell'istruzione `if`.

3. A questo punto, è possibile eliminare le righe equivalenti dalle funzioni `windBackward()` e `windForward()`, poiché questa funzionalità è stata invece implementata nella funzione `stopMedia()`.

> [!NOTE]
> L'efficienza del codice potrebbe essere ulteriormente migliorata creando una funzione separata che esegua queste righe e chiamandola ovunque sia necessario, anziché ripetere le righe più volte nel codice. Ma questo esercizio viene lasciato al lettore.

## Riepilogo

Questo articolo ha fornito informazioni sufficienti. L'API [`HTMLMediaElement`](/it/docs/Web/API/HTMLMediaElement) mette a disposizione un'ampia gamma di funzionalità per creare semplici lettori video e audio, e questa è solo la punta dell'iceberg. Consultare la sezione "Vedere anche" qui sotto per collegamenti a funzionalità più complesse e interessanti.

Ecco alcuni suggerimenti su come migliorare l'esempio esistente:

1. La visualizzazione del tempo attualmente non funziona se il video dura un'ora o più, poiché non mostra le ore, ma solo minuti e secondi. È possibile capire come modificare l'esempio affinché visualizzi anche le ore?
2. Poiché gli elementi `<audio>` dispongono delle stesse funzionalità [`HTMLMediaElement`](/it/docs/Web/API/HTMLMediaElement), è possibile fare in modo che questo lettore funzioni facilmente anche per un elemento `<audio>`. Provare a farlo.
3. È possibile trovare un modo per trasformare l'elemento `<div>` interno del timer in una vera barra di ricerca/scorrimento, ovvero affinché, facendo clic in un punto della barra, si salti a quella posizione relativa nella riproduzione del video? Come suggerimento, è possibile ottenere i valori X e Y dei lati sinistro/destro e superiore/inferiore dell'elemento tramite il metodo [`getBoundingClientRect()`](/it/docs/Web/API/Element/getBoundingClientRect), mentre le coordinate di un clic del mouse possono essere ottenute tramite l'oggetto evento dell'evento di clic, chiamato sull'oggetto [`Document`](/it/docs/Web/API/Document). Ad esempio:

   ```js
   document.onclick = function (e) {
     console.log(e.x, e.y);
   };
   ```

## Vedere anche

- [`HTMLMediaElement`](/it/docs/Web/API/HTMLMediaElement)
- [Video e audio HTML](/it/docs/Learn_web_development/Core/Structuring_content/HTML_video_and_audio) — guida semplice a `<video>` e `<audio>` HTML.
- [Distribuzione di audio e video](/it/docs/Web/Media/Guides/Audio_and_video_delivery) — guida dettagliata alla distribuzione di contenuti multimediali nel browser, con molti suggerimenti, tecniche e collegamenti a ulteriori tutorial più avanzati.
- [Manipolazione di audio e video](/it/docs/Web/Media/Guides/Audio_and_video_manipulation) — guida dettagliata alla manipolazione di audio e video, ad esempio con [Canvas API](/it/docs/Web/API/Canvas_API), [Web Audio API](/it/docs/Web/API/Web_Audio_API) e altro.
- Pagine di riferimento per {{htmlelement("video")}} e {{htmlelement("audio")}}.
- [Guida ai tipi e ai formati multimediali sul web](/it/docs/Web/Media/Guides/Formats)

{{PreviousMenuNext("Learn_web_development/Extensions/Client-side_APIs/Introduction", "Learn_web_development/Extensions/Client-side_APIs/Drawing_graphics", "Learn_web_development/Extensions/Client-side_APIs")}}
