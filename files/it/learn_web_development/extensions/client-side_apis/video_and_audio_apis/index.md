---
title: API per video e audio
short-title: Video e audio
slug: Learn_web_development/Extensions/Client-side_APIs/Video_and_audio_APIs
l10n:
  sourceCommit: cd701f10306c8b0b9690532ff808df826818a04f
---

{{PreviousMenuNext("Learn_web_development/Extensions/Client-side_APIs/Introduction", "Learn_web_development/Extensions/Client-side_APIs/Drawing_graphics", "Learn_web_development/Extensions/Client-side_APIs")}}

HTML include elementi per incorporare media ricchi di contenuti nei documenti — {{htmlelement("video")}} e {{htmlelement("audio")}} — che a loro volta hanno le proprie API per controllare la riproduzione, la ricerca, ecc. Questo articolo mostra come eseguire attività comuni come la creazione di controlli di riproduzione personalizzati.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, in particolare le <a href="/it/docs/Learn_web_development/Core/Scripting/Object_basics">basi degli oggetti JavaScript</a> e la copertura delle API di base come il <a href="/it/docs/Learn_web_development/Core/Scripting/DOM_scripting">DOM scripting</a> e le <a href="/it/docs/Learn_web_development/Core/Scripting/Network_requests">richieste di rete</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        <ul>
          <li>Cosa sono i codec e i diversi formati di video e audio.</li>
          <li>Comprendere le funzionalità chiave associate all'audio e al video — play, pausa, stop, ricerca indietro e in avanti, durata e tempo corrente.</li>
          <li>Utilizzare l'API <code>HTMLMediaElement</code> per costruire un lettore multimediale personalizzato di base, per una migliore accessibilità o maggiore coerenza tra i browser.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Video e audio HTML

Gli elementi {{htmlelement("video")}} e {{htmlelement("audio")}} ci permettono di incorporare video e audio nelle pagine web. Come abbiamo mostrato in [Video e audio HTML](/it/docs/Learn_web_development/Core/Structuring_content/HTML_video_and_audio), un'implementazione tipica appare così:

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

{{EmbedGHLiveSample("learning-area/html/multimedia-and-embedding/video-and-audio-content/multiple-video-formats.html", '100%', 380)}}

Può rivedere cosa fanno tutte le funzionalità HTML nell'articolo linkato sopra; per i nostri scopi qui, l'attributo più interessante è [`controls`](/it/docs/Web/HTML/Reference/Elements/video#controls), che abilita l'insieme predefinito di controlli di riproduzione. Se non si specifica questo attributo, non si ottengono controlli di riproduzione:

{{EmbedGHLiveSample("learning-area/html/multimedia-and-embedding/video-and-audio-content/multiple-video-formats-no-controls.html", '100%', 380)}}

Questo non è immediatamente utile per la riproduzione video, ma ha dei vantaggi. Un grosso problema con i controlli nativi del browser è che sono diversi in ogni browser — non molto buono per il supporto multipiattaforma! Un altro grande problema è che i controlli nativi nella maggior parte dei browser non sono molto accessibili tramite tastiera.

Può risolvere entrambi questi problemi nascondendo i controlli nativi (rimuovendo l'attributo `controls`) e programmando i propri con HTML, CSS e JavaScript. Nella sezione successiva, vedremo gli strumenti di base che abbiamo a disposizione per farlo.

## L'API HTMLMediaElement

Parte della specifica HTML, l'API [`HTMLMediaElement`](/it/docs/Web/API/HTMLMediaElement) fornisce funzionalità che permettono di controllare i lettori video e audio in modo programmatico — ad esempio [`HTMLMediaElement.play()`](/it/docs/Web/API/HTMLMediaElement/play), [`HTMLMediaElement.pause()`](/it/docs/Web/API/HTMLMediaElement/pause), ecc. Questa interfaccia è disponibile per entrambi gli elementi {{htmlelement("audio")}} e {{htmlelement("video")}}, poiché le caratteristiche che si vorranno implementare sono quasi identiche. Esaminiamo un esempio, aggiungendo funzionalità man mano che procediamo.

Il nostro esempio finale apparirà (e funzionerà) in modo simile al seguente:

{{EmbedGHLiveSample("learning-area/javascript/apis/video-audio/finished/", '100%', 360)}}

### Per iniziare

Per iniziare con questo esempio, [scarichi il nostro media-player-start.zip](https://github.com/mdn/learning-area/blob/main/javascript/apis/video-audio/start/media-player-start.zip) e lo decomprima in una nuova directory sul suo disco rigido. Se ha [scaricato il nostro repository di esempi](https://github.com/mdn/learning-area), lo troverà in `javascript/apis/video-audio/start/`.

A questo punto, se carica l'HTML dovrebbe vedere un lettore video HTML perfettamente normale, con i controlli nativi resi.

#### Esplorare l'HTML

Apra il file HTML index. Vedrà un numero di funzionalità; l'HTML è dominato dal lettore video e dai suoi controlli:

```html
<div class="player">
  <video controls>
    <source src="video/sintel-short.mp4" type="video/mp4" />
    <source src="video/sintel-short.webm" type="video/webm" />
    <!-- fallback content here -->
  </video>
  <div class="controls">
    <button class="play" data-icon="P" aria-label="play pause toggle"></button>
    <button class="stop" data-icon="S" aria-label="stop"></button>
    <div class="timer">
      <div></div>
      <span aria-label="timer">00:00</span>
    </div>
    <button class="rwd" data-icon="B" aria-label="rewind"></button>
    <button class="fwd" data-icon="F" aria-label="fast forward"></button>
  </div>
</div>
```

- L'intero lettore è avvolto in un elemento {{htmlelement("div")}}, in modo che possa essere tutto stilizzato come un'unica unità, se necessario.
- L'elemento {{htmlelement("video")}} contiene due elementi {{htmlelement("source")}} in modo che formati diversi possano essere caricati a seconda del browser che visualizza il sito.
- L'HTML dei controlli è probabilmente la parte più interessante:

  - Abbiamo quattro {{htmlelement("button")}} — play/pause, stop, rewind e fast forward.
  - Ogni `<button>` ha un nome `class`, un attributo `data-icon` per definire quale icona dovrebbe essere mostrata su ciascun pulsante (mostreremo come funziona nella sezione sottostante), e un attributo `aria-label` per fornire una descrizione comprensibile di ciascun pulsante, dato che non forniamo un'etichetta leggibile dall'uomo all'interno dei tag. I contenuti degli attributi `aria-label` vengono letti dai lettori di schermo quando i loro utenti si concentrano sugli elementi che li contengono.
  - C'è anche un timer {{htmlelement("div")}}, che segnalerà il tempo trascorso quando il video è in riproduzione. Giusto per divertimento, stiamo fornendo due meccanismi di segnalazione — uno {{htmlelement("span")}} contenente il tempo trascorso in minuti e secondi, e un ulteriore `<div>` che useremo per creare una barra indicatrice orizzontale che si allunga man mano che il tempo trascorre. Per avere un'idea di come apparirà il prodotto finale, [veda la nostra versione finale](https://mdn.github.io/learning-area/javascript/apis/video-audio/finished/).

#### Esplorare il CSS

Ora apra il file CSS e dia un'occhiata all'interno. Il CSS per l'esempio non è troppo complicato, ma evidenzieremo qui le parti più interessanti. Prima di tutto, noti lo stile `.controls`:

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

- Cominciamo con la {{cssxref("visibility")}} dei controlli personalizzati impostata su `hidden`. Nel nostro JavaScript in seguito, imposteremo i controlli su `visible` e rimuoveremo l'attributo `controls` dall'elemento `<video>`. Questo è così che, se il JavaScript non si carica per qualche motivo, gli utenti possono ancora usare il video con i controlli nativi.
- Diamo ai controlli un'{{cssxref("opacity")}} di 0,5 di default, in modo che siano meno distrattivi quando si sta cercando di guardare il video. Solo quando si passa con il mouse o si mette a fuoco il lettore, i controlli appaiono con opacità completa.
- Disponiamo i pulsanti all'interno della barra di controllo usando flexbox ({{cssxref("display")}}: flex), per facilitare le cose.

Successivamente, esaminiamo le icone dei pulsanti:

```css
@font-face {
  font-family: "HeydingsControlsRegular";
  src: url("fonts/heydings_controls-webfont.eot");
  src:
    url("fonts/heydings_controls-webfont.eot?#iefix")
      format("embedded-opentype"),
    url("fonts/heydings_controls-webfont.woff") format("woff"),
    url("fonts/heydings_controls-webfont.ttf") format("truetype");
  font-weight: normal;
  font-style: normal;
}

button:before {
  font-family: HeydingsControlsRegular;
  font-size: 20px;
  position: relative;
  content: attr(data-icon);
  color: #aaa;
  text-shadow: 1px 1px 0px black;
}
```

Innanzitutto, nella parte superiore del CSS usiamo un blocco {{cssxref("@font-face")}} per importare un font web personalizzato. Questo è un font icona — tutti i caratteri dell'alfabeto corrispondono a icone comuni che si potrebbero voler usare in un'applicazione.

Successivamente, utilizziamo contenuti generati per visualizzare un'icona su ciascun pulsante:

- Usiamo il selettore {{cssxref("::before")}} per visualizzare il contenuto prima di ogni elemento {{htmlelement("button")}}.
- Usiamo la proprietà {{cssxref("content")}} per impostare il contenuto da visualizzare in ogni caso come uguale al contenuto dell'attributo [`data-icon`](/it/docs/Web/HTML/How_to/Use_data_attributes). Nel caso del nostro pulsante play, `data-icon` contiene una "P" maiuscola.
- Applichiamo il font web personalizzato ai nostri pulsanti usando {{cssxref("font-family")}}. In questo font, "P" è in realtà un'icona "play", quindi il pulsante play ha un'icona "play" visualizzata su di esso.

I font icona sono molto interessanti per molti motivi — ridurre le richieste HTTP poiché non è necessario scaricare quelle icone come file immagine, grande scalabilità, e il fatto che è possibile utilizzare le proprietà di testo per stilizzarle — come {{cssxref("color")}} e {{cssxref("text-shadow")}}.

Ultimo ma non meno importante, esaminiamo il CSS per il timer:

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

- Impostiamo l'elemento `.timer` esterno su `flex: 5`, in modo che occupi la maggior parte della larghezza della barra di controllo. Gli diamo anche {{cssxref("position", "position: relative")}}, così possiamo posizionare comodamente gli elementi al suo interno in base ai suoi limiti, e non ai limiti dell'elemento {{htmlelement("body")}}.
- Il `<div>` interno è posizionato in modo assoluto per sedersi direttamente sopra il `<div>` esterno. Viene anche dato una larghezza iniziale di 0, quindi non si può vedere affatto. Man mano che il video viene riprodotto, la larghezza verrà aumentata tramite JavaScript man mano che il video avanza.
- Anche lo `<span>` è posizionato in modo assoluto per sedersi vicino al lato sinistro della barra del timer.
- Diamo anche al nostro `<div>` interno e allo `<span>` la giusta quantità di {{cssxref("z-index")}}, in modo che il timer venga visualizzato sopra, e il `<div>` interno sotto di esso. In questo modo, ci assicuriamo che tutte le informazioni siano visibili — una casella non ne oscura un'altra.

### Implementare il JavaScript

Abbiamo già un'interfaccia HTML e CSS abbastanza completa; ora dobbiamo solo collegare tutti i pulsanti per far funzionare i controlli.

1. Crei un nuovo file JavaScript allo stesso livello della directory del file index.html. Lo chiami `custom-player.js`.
2. Nella parte superiore di questo file, inserisca il seguente codice:

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

   Qui stiamo creando costanti per mantenere i riferimenti a tutti gli oggetti che vogliamo manipolare. Abbiamo tre gruppi:

   - L'elemento `<video>`, e la barra dei controlli.
   - I pulsanti play/pause, stop, rewind e fast forward.
   - L'`<div>` del wrapper esterno del timer, il display di lettura digitale del timer `<span>`, e l'`<div>` interno che si allarga man mano che il tempo trascorre.

3. Successivamente, inserisca il seguente codice in fondo al suo codice:

   ```js
   media.removeAttribute("controls");
   controls.style.visibility = "visible";
   ```

   Queste due righe rimuovono i controlli del browser predefiniti dal video e rendono visibili i controlli personalizzati.

#### Riproduzione e pausa del video

Implementiamo probabilmente il controllo più importante — il pulsante play/pause.

1. Prima di tutto, aggiunga il seguente codice in fondo al suo codice, in modo che la funzione `playPauseMedia()` venga chiamata quando si fa clic sul pulsante play:

   ```js
   play.addEventListener("click", playPauseMedia);
   ```

2. Ora definiamo `playPauseMedia()` — aggiunga il seguente codice di nuovo in fondo al suo codice:

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

   Qui usiamo un'istruzione [`if`](/it/docs/Web/JavaScript/Reference/Statements/if...else) per verificare se il video è in pausa. La proprietà [`HTMLMediaElement.paused`](/it/docs/Web/API/HTMLMediaElement/paused) restituisce true se il media è in pausa, il che succede ogni volta che il video non è in riproduzione, incluso quando è impostato a 0 di durata dopo il primo caricamento. Se è in pausa, impostiamo il valore dell'attributo `data-icon` sul pulsante play a "u", che è un'icona "pausa", e invochiamo il metodo [`HTMLMediaElement.play()`](/it/docs/Web/API/HTMLMediaElement/play) per riprodurre il media.

   Al secondo clic, il pulsante verrà nuovamente commutato — l'icona "play" verrà nuovamente mostrata e il video verrà messo in pausa con [`HTMLMediaElement.pause()`](/it/docs/Web/API/HTMLMediaElement/pause).

#### Fermare il video

1. Successivamente, aggiungiamo funzionalità per gestire l'arresto del video. Aggiunga le seguenti righe [`addEventListener()`](/it/docs/Web/API/EventTarget/addEventListener) sotto quella precedente che ha aggiunto:

   ```js
   stop.addEventListener("click", stopMedia);
   media.addEventListener("ended", stopMedia);
   ```

   L'evento [`click`](/it/docs/Web/API/Element/click_event) è ovvio — vogliamo fermare il video eseguendo la nostra funzione `stopMedia()` quando viene cliccato il pulsante di stop. Tuttavia, vogliamo anche fermare il video quando termina la riproduzione — questo è segnato dal generarsi dell'evento [`ended`](/it/docs/Web/API/HTMLMediaElement/ended_event), quindi impostiamo anche un ascoltatore per eseguire la funzione anche al generarsi di questo evento.

2. Successivamente, definiamo `stopMedia()` — aggiunga la seguente funzione sotto `playPauseMedia()`:

   ```js
   function stopMedia() {
     media.pause();
     media.currentTime = 0;
     play.setAttribute("data-icon", "P");
   }
   ```

   Non esiste un metodo `stop()` nell'API HTMLMediaElement — l'equivalente è `pause()` il video e impostare la sua proprietà [`currentTime`](/it/docs/Web/API/HTMLMediaElement/currentTime) a 0. Impostare `currentTime` su un valore (in secondi) sposta immediatamente il media a quella posizione.

   Non resta altro da fare dopo questo se non impostare l'icona visualizzata sull'icona "play". Indipendentemente dal fatto che il video fosse fermo o in riproduzione quando si preme il pulsante di stop, si vuole che sia pronto a riprodurre in seguito.

#### Ricerca avanti e indietro

Ci sono molti modi per implementare la funzionalità di rewind e fast forward; qui stiamo mostrando un modo relativamente complesso di farlo, che non si rompe quando i diversi pulsanti vengono premuti in un ordine inaspettato.

1. Prima di tutto, aggiunga le due seguenti righe [`addEventListener()`](/it/docs/Web/API/EventTarget/addEventListener) sotto quelle precedenti:

   ```js
   rwd.addEventListener("click", mediaBackward);
   fwd.addEventListener("click", mediaForward);
   ```

2. Ora passiamo alle funzioni gestori degli eventi — aggiunga il seguente codice sotto le sue funzioni precedenti per definire `mediaBackward()` e `mediaForward()`:

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

   Noterà che per prima cosa, inizializziamo due variabili — `intervalFwd` e `intervalRwd` — scoprirà a cosa servono più avanti.

   Esaminiamo `mediaBackward()` (la funzionalità per `mediaForward()` è esattamente la stessa, ma al contrario):

   1. Cancelliamo tutte le classi e gli intervalli che sono impostati sulla funzionalità di fast forward — lo facciamo perché se premiamo il tasto `rwd` dopo aver premuto il tasto `fwd`, vogliamo annullare qualsiasi funzionalità di fast forward e sostituirlo con la funzionalità di rewind. Se provassimo a fare entrambe le cose contemporaneamente, il lettore si romperebbe.
   2. Usiamo un'istruzione `if` per verificare se la classe `active` è stata impostata sul pulsante `rwd`, indicando che è già stata premuta. Il [`classList`](/it/docs/Web/API/Element/classList) è una proprietà piuttosto utile che esiste su ogni elemento — contiene un elenco di tutte le classi impostate sull'elemento, nonché metodi per aggiungere/rimuovere classi, ecc. Usiamo il metodo `classList.contains()` per verificare se l'elenco contiene la classe `active`. Questo restituisce un risultato booleano `true`/`false`.
   3. Se `active` è stato impostato sul pulsante `rwd`, lo rimuoviamo utilizzando `classList.remove()`, cancelliamo l'intervallo che è stato impostato quando il pulsante è stato premuto per la prima volta (veda sotto per un'ulteriore spiegazione) e usiamo [`HTMLMediaElement.play()`](/it/docs/Web/API/HTMLMediaElement/play) per annullare il rewind e avviare la riproduzione normale del video.
   4. Se non è stato ancora impostato, aggiungiamo la classe `active` al pulsante `rwd` utilizzando `classList.add()`, mettiamo in pausa il video usando [`HTMLMediaElement.pause()`](/it/docs/Web/API/HTMLMediaElement/pause), poi impostiamo la variabile `intervalRwd` uguale a una chiamata a [`setInterval()`](/it/docs/Web/API/Window/setInterval). Quando invocato, `setInterval()` crea un'intervallo attivo, cioè esegue la funzione data come primo parametro ogni x millisecondi, dove x è il valore del secondo parametro. Quindi qui stiamo eseguendo la funzione `windBackward()` ogni 200 millisecondi — useremo questa funzione per riavvolgere il video costantemente. Per fermare l'esecuzione di un [`setInterval()`](/it/docs/Web/API/Window/setInterval), bisogna chiamare [`clearInterval()`](/it/docs/Web/API/Window/clearInterval), dandogli il nome identificativo dell'intervallo da cancellare, che in questo caso è il nome della variabile `intervalRwd` (veda la chiamata `clearInterval()` sopra nella funzione).

3. Infine, dobbiamo definire le funzioni `windBackward()` e `windForward()` invocate nelle chiamate `setInterval()`. Aggiunga il seguente codice sotto le due funzioni precedenti:

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

   Ancora una volta, descriviamo solo la prima di queste funzioni poiché lavorano quasi identicamente, ma al contrario l'una dall'altra. In `windBackward()` facciamo quanto segue — tenga presente che quando l'intervallo è attivo, questa funzione viene eseguita una volta ogni 200 millisecondi.

   1. Iniziamo con un'istruzione `if` che verifica se il tempo corrente è inferiore a 3 secondi, cioè il riavvolgimento di altri tre secondi lo riporterebbe oltre l'inizio del video. Questo causerebbe comportamenti strani, quindi se questo è il caso, stoppiamo la riproduzione del video chiamando `stopMedia()`, rimuoviamo la classe `active` dal pulsante rewind e cancelliamo l'intervallo `intervalRwd` per fermare la funzionalità di rewind. Se non facessimo questo ultimo passaggio, il video continuerebbe a riavvolgersi all'infinito.
   2. Se il tempo corrente non è entro 3 secondi dall'inizio del video, rimuoviamo tre secondi dal tempo corrente eseguendo `media.currentTime -= 3`. Quindi, in effetti, stiamo riavvolgendo il video di 3 secondi, una volta ogni 200 millisecondi.

#### Aggiornare il tempo trascorso

L'ultimo pezzo del nostro lettore multimediale da implementare sono i display del tempo trascorso. Per farlo eseguiremo una funzione per aggiornare i display del tempo ogni volta che l'evento [`timeupdate`](/it/docs/Web/API/HTMLMediaElement/timeupdate_event) viene emesso sull'elemento `<video>`. La frequenza con cui questo evento viene emesso dipende dal suo browser, dalla potenza della CPU, ecc. ([veda questo post su Stack Overflow](https://stackoverflow.com/questions/9678177/how-often-does-the-timeupdate-event-fire-for-an-html5-video)).

Aggiunga la seguente riga `addEventListener()` appena sotto le altre:

```js
media.addEventListener("timeupdate", setTime);
```

Ora per definire la funzione `setTime()`. Aggiunga il seguente codice in fondo al suo file:

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

Questa è una funzione piuttosto lunga, quindi andiamo attraverso di essa passo per passo:

1. Prima di tutto, calcoliamo quanti minuti e secondi ci sono nel valore [`HTMLMediaElement.currentTime`](/it/docs/Web/API/HTMLMediaElement/currentTime).
2. Poi inizializziamo altre due variabili — `minuteValue` e `secondValue`. Utilizziamo {{jsxref("String/padStart", "padStart()")}} per rendere ciascun valore lungo 2 caratteri, anche se il valore numerico è solo una cifra.
3. Il valore di tempo effettivo da visualizzare è impostato come `minuteValue` più un carattere di due punti più `secondValue`.
4. Il valore [`Node.textContent`](/it/docs/Web/API/Node/textContent) del timer è impostato sul valore del tempo, quindi viene visualizzato nell'interfaccia.
5. La lunghezza che dovremmo impostare per il `<div>` interno viene calcolata prima calcolando la larghezza del `<div>` esterno (la proprietà [`clientWidth`](/it/docs/Web/API/Element/clientWidth) di un qualsiasi elemento conterrà la sua lunghezza), e poi moltiplicandola per il [`HTMLMediaElement.currentTime`](/it/docs/Web/API/HTMLMediaElement/currentTime) diviso per la [`HTMLMediaElement.duration`](/it/docs/Web/API/HTMLMediaElement/duration) totale del media.
6. Impostiamo la larghezza del `<div>` interno per essere pari alla lunghezza calcolata della barra, più "px", in modo che venga impostata a quel numero di pixel.

#### Correggere la riproduzione e la pausa

C'è un problema rimasto da risolvere. Se i pulsanti di play/pause o stop vengono premuti mentre la funzionalità di rewind o fast forward è attiva, semplicemente non funzionano. Come possiamo fare in modo che annullino la funzionalità dei pulsanti `rwd`/`fwd` e riproducano/sospendano il video come ci si aspetterebbe? Questo è abbastanza facile da risolvere.

Prima di tutto, aggiunga le seguenti righe all'interno della funzione `stopMedia()` — ovunque andrà bene:

```js
rwd.classList.remove("active");
fwd.classList.remove("active");
clearInterval(intervalRwd);
clearInterval(intervalFwd);
```

Ora aggiunga di nuovo le stesse righe, all'inizio della funzione `playPauseMedia()` (giusto prima dell'inizio dell'istruzione `if`).

A questo punto, potrebbe eliminare le righe equivalenti dalle funzioni `windBackward()` e `windForward()`, poiché quella funzionalità è stata implementata nella funzione `stopMedia()` invece.

Nota: Potrebbe anche migliorare ulteriormente l'efficienza del codice creando una funzione separata che esegue queste righe, quindi chiamandola ovunque sia necessaria, piuttosto che ripetere le righe più volte nel codice. Ma lasciamo questo compito a lei.

## Sommario

Penso che abbiamo insegnato abbastanza in questo articolo. L'API [`HTMLMediaElement`](/it/docs/Web/API/HTMLMediaElement) mette a disposizione una ricchezza di funzionalità per creare semplici lettori video e audio, e questo è solo la punta dell'iceberg. Veda la sezione "Vedere anche" qui sotto per collegamenti a funzionalità più complesse e interessanti.

Ecco alcuni suggerimenti per modi in cui potrebbe migliorare l'esempio esistente che abbiamo costruito:

1. Il display del tempo attualmente si rompe se il video dura un'ora o più (beh, non mostrerà ore; solo minuti e secondi). Può capire come cambiare l'esempio per farlo visualizzare le ore?
2. Poiché gli elementi `<audio>` hanno la stessa funzionalità [`HTMLMediaElement`](/it/docs/Web/API/HTMLMediaElement) disponibile, potrebbe facilmente far funzionare questo lettore per un elemento `<audio>` anche. Provi a farlo.
3. Può trovare un modo per trasformare l'elemento `<div>` interno del timer in una vera barra di ricerca/scrolling — cioè, quando clicca da qualche parte sulla barra, salta a quella posizione relativa nella riproduzione del video? Come suggerimento, può scoprire i valori X e Y dei lati sinistro/destro e superiore/inferiore dell'elemento tramite il metodo [`getBoundingClientRect()`](/it/docs/Web/API/Element/getBoundingClientRect), e può trovare le coordinate di un clic del mouse tramite l'oggetto evento dell'evento del clic, chiamato sull'oggetto [`Document`](/it/docs/Web/API/Document). Ad esempio:

   ```js
   document.onclick = function (e) {
     console.log(e.x, e.y);
   };
   ```

## Vedere anche

- [`HTMLMediaElement`](/it/docs/Web/API/HTMLMediaElement)
- [Video e audio HTML](/it/docs/Learn_web_development/Core/Structuring_content/HTML_video_and_audio) — guida semplice agli elementi `<video>` e `<audio>` HTML.
- [Consegna di audio e video](/it/docs/Web/Media/Guides/Audio_and_video_delivery) — guida dettagliata per consegnare i media all'interno del browser, con molti suggerimenti, trucchi e collegamenti a tutorial più avanzati.
- [Manipolazione di audio e video](/it/docs/Web/Media/Guides/Audio_and_video_manipulation) — guida dettagliata alla manipolazione di audio e video, ad esempio con [Canvas API](/it/docs/Web/API/Canvas_API), [Web Audio API](/it/docs/Web/API/Web_Audio_API), e altro ancora.
- Pagine di riferimento {{htmlelement("video")}} e {{htmlelement("audio")}}.
- [Guida ai tipi e formati di media sul web](/it/docs/Web/Media/Guides/Formats)

{{PreviousMenuNext("Learn_web_development/Extensions/Client-side_APIs/Introduction", "Learn_web_development/Extensions/Client-side_APIs/Drawing_graphics", "Learn_web_development/Extensions/Client-side_APIs")}}
