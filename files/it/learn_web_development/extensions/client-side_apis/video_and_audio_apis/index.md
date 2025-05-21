---
title: API video e audio
short-title: Video e audio
slug: Learn_web_development/Extensions/Client-side_APIs/Video_and_audio_APIs
l10n:
  sourceCommit: cd701f10306c8b0b9690532ff808df826818a04f
---

{{PreviousMenuNext("Learn_web_development/Extensions/Client-side_APIs/Introduction", "Learn_web_development/Extensions/Client-side_APIs/Drawing_graphics", "Learn_web_development/Extensions/Client-side_APIs")}}

HTML include elementi per incorporare media ricchi nei documenti — {{htmlelement("video")}} e {{htmlelement("audio")}} — che a loro volta dispongono di API per controllare la riproduzione, lo seeking, ecc. Questo articolo mostra come eseguire attività comuni come creare controlli di riproduzione personalizzati.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, in particolare le basi degli <a href="/it/docs/Learn_web_development/Core/Scripting/Object_basics">oggetti in JavaScript</a> e la copertura API di base come <a href="/it/docs/Learn_web_development/Core/Scripting/DOM_scripting">DOM scripting</a> e <a href="/it/docs/Learn_web_development/Core/Scripting/Network_requests">richieste di rete</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        <ul>
          <li>Cosa sono i codec e i diversi formati video e audio.</li>
          <li>Comprendere le funzionalità chiave associate ad audio e video — riproduzione, pausa, stop, seeking indietro e avanti, durata e tempo corrente.</li>
          <li>Utilizzare l'API <code>HTMLMediaElement</code> per costruire un lettore multimediale personalizzato di base, per una migliore accessibilità o una maggiore coerenza tra i browser.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Video e audio HTML

Gli elementi {{htmlelement("video")}} e {{htmlelement("audio")}} ci permettono di incorporare video e audio nelle pagine web. Come abbiamo mostrato in [Video e audio HTML](/it/docs/Learn_web_development/Core/Structuring_content/HTML_video_and_audio), un'implementazione tipica sembra così:

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

Puoi rivedere cosa fanno tutte le funzionalità HTML nell'articolo linkato sopra; per i nostri scopi qui, l'attributo più interessante è [`controls`](/it/docs/Web/HTML/Reference/Elements/video#controls), che abilita il set di controlli di riproduzione predefinito. Se non lo specifichi, non ottieni controlli di riproduzione:

{{EmbedGHLiveSample("learning-area/html/multimedia-and-embedding/video-and-audio-content/multiple-video-formats-no-controls.html", '100%', 380)}}

Questo non è tanto utile immediatamente per la riproduzione video, ma ha dei vantaggi. Uno dei problemi principali con i controlli nativi del browser è che sono diversi in ogni browser — non molto buono per il supporto cross-browser! Un altro grande problema è che i controlli nativi nella maggior parte dei browser non sono molto accessibili tramite tastiera.

Puoi risolvere entrambi questi problemi nascondendo i controlli nativi (rimuovendo l'attributo `controls`), e programmando i tuoi con HTML, CSS e JavaScript. Nella sezione successiva, esamineremo gli strumenti di base a nostra disposizione per farlo.

## L'API HTMLMediaElement

Parte della specifica HTML, l'API [`HTMLMediaElement`](/it/docs/Web/API/HTMLMediaElement) fornisce funzionalità che ti permettono di controllare i lettori video e audio programmaticamente — per esempio [`HTMLMediaElement.play()`](/it/docs/Web/API/HTMLMediaElement/play), [`HTMLMediaElement.pause()`](/it/docs/Web/API/HTMLMediaElement/pause), ecc. Questa interfaccia è disponibile per entrambi gli elementi {{htmlelement("audio")}} e {{htmlelement("video")}}, poiché le funzionalità che si desidera implementare sono quasi identiche. Vediamo un esempio, aggiungendo funzionalità man mano.

Il nostro esempio finale apparirà (e funzionerà) come il seguente:

{{EmbedGHLiveSample("learning-area/javascript/apis/video-audio/finished/", '100%', 360)}}

### Per iniziare

Per iniziare con questo esempio, [scarica il nostro media-player-start.zip](https://github.com/mdn/learning-area/blob/main/javascript/apis/video-audio/start/media-player-start.zip) e decomprimilo in una nuova directory sul tuo hard drive. Se hai [scaricato il nostro repo di esempi](https://github.com/mdn/learning-area), lo troverai in `javascript/apis/video-audio/start/`.

A questo punto, se carichi l'HTML dovresti vedere un normale lettore video HTML, con i controlli nativi resi.

#### Esplorando l'HTML

Apri il file index HTML. Vedrai una serie di funzionalità; l'HTML è dominato dal lettore video e dai suoi controlli:

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

- L'intero lettore è avvolto in un elemento {{htmlelement("div")}}, in modo che possa essere interamente stilizzato come un'unità se necessario.
- L'elemento {{htmlelement("video")}} contiene due elementi {{htmlelement("source")}} in modo che diversi formati possano essere caricati a seconda del browser che visualizza il sito.
- L'HTML dei controlli è probabilmente il più interessante:

  - Abbiamo quattro {{htmlelement("button")}}: riproduci/pausa, fermo, riavvolgi e avanzamento veloce.
  - Ogni `<button>` ha un nome di `class`, un attributo `data-icon` per definire quale icona dovrebbe essere mostrata su ciascun pulsante (mostreremo come funziona questa parte nella sezione sottostante), e un attributo `aria-label` per fornire una descrizione comprensibile di ciascun pulsante, dato che non stiamo fornendo un'etichetta leggibile dall'utente all'interno dei tag. I contenuti degli attributi `aria-label` sono letti dai lettori di schermo quando i loro utenti si concentrano sugli elementi che li contengono.
  - C'è anche un timer {{htmlelement("div")}}, che segnalerà il tempo trascorso quando il video è in riproduzione. Giusto per divertimento, forniamo due meccanismi di segnalazione — un {{htmlelement("span")}} che contiene il tempo trascorso in minuti e secondi, e un ulteriore `<div>` che useremo per creare una barra orizzontale indicatrice che si allunga man mano che il tempo passa. Per avere un'idea di come apparirà il prodotto finito, [controlla la nostra versione finale](https://mdn.github.io/learning-area/javascript/apis/video-audio/finished/).

#### Esplorando il CSS

Ora apri il file CSS e guarda all'interno. Il CSS dell'esempio non è troppo complicato, ma evidenzieremo le parti più interessanti qui. Innanzitutto, nota lo stile `.controls`:

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

- Iniziamo con la {{cssxref("visibility")}} dei controlli personalizzati impostata su `hidden`. Nel nostro JavaScript successivamente, imposteremo i controlli su `visible` e rimuoveremo l'attributo `controls` dall'elemento `<video>`. Questo è così che, se il JavaScript non si carica per qualche motivo, gli utenti possono comunque utilizzare il video con i controlli nativi.
- Diamo ai controlli un'{{cssxref("opacity")}} di 0.5 di default, in modo che siano meno distrattivi quando stai cercando di guardare il video. Solo quando si passa il mouse/si focalizza sul lettore, i controlli appaiono a piena opacità.
- Disponiamo i pulsanti all'interno della barra dei controlli usando il flexbox ({{cssxref("display")}}: flex), per facilitare le cose.

Ora guardiamo le nostre icone dei pulsanti:

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

Prima di tutto, all'inizio del CSS usiamo un blocco {{cssxref("@font-face")}} per importare un font web personalizzato. Questo è un font di icone — tutti i caratteri dell'alfabeto equivalgono a icone comuni che potresti voler utilizzare in un'applicazione.

Successivamente, usiamo contenuti generati per visualizzare un'icona su ogni pulsante:

- Utilizziamo il selettore {{cssxref("::before")}} per visualizzare il contenuto prima di ogni elemento {{htmlelement("button")}}.
- Utilizziamo la proprietà {{cssxref("content")}} per impostare il contenuto da visualizzare in ogni caso in modo che sia uguale al contenuto dell'attributo [`data-icon`](/it/docs/Web/HTML/How_to/Use_data_attributes). Nel caso del nostro pulsante play, `data-icon` contiene una "P" maiuscola.
- Applichiamo il font web personalizzato ai nostri pulsanti utilizzando {{cssxref("font-family")}}. In questo font, "P" è effettivamente un'icona "riproduci", quindi il pulsante play ha un'icona "riproduci" visualizzata su di esso.

I font di icone sono molto belli per molti motivi — riducono le richieste HTTP perché non devi scaricare quelle icone come file immagine, grande scalabilità, e il fatto che puoi utilizzare le proprietà del testo per stilizzarle — come {{cssxref("color")}} e {{cssxref("text-shadow")}}.

Ultimo ma non meno importante, guardiamo al CSS del timer:

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

- Impostiamo l'elemento esterno `.timer` con `flex: 5`, in modo che occupi la maggior parte della larghezza della barra di controllo. Inoltre, gli diamo una {{cssxref("position", "position: relative")}}, in modo che possiamo posizionare gli elementi all'interno convenientemente secondo i suoi confini, e non i confini dell'elemento {{htmlelement("body")}}.
- L'`<div>` interno è posizionato assolutamente per sedersi direttamente sopra il `<div>` esterno. È anche dato una larghezza iniziale di 0, quindi non puoi vederlo affatto. Man mano che il video viene riprodotto, la larghezza verrà aumentata tramite JavaScript mentre il tempo passa.
- Anche lo `<span>` è posizionato assolutamente per sedersi vicino al lato sinistro della barra del timer.
- Diamo anche al nostro `<div>` interno e al `<span>` il giusto valore di {{cssxref("z-index")}} in modo che il timer venga visualizzato sopra, e il `<div>` interno sotto di esso. In questo modo, ci assicuriamo di poter vedere tutte le informazioni — una casella non sta oscurando un'altra.

### Implementare il JavaScript

Abbiamo già un'interfaccia HTML e CSS abbastanza completa; ora dobbiamo solo collegare tutti i pulsanti per far funzionare i controlli.

1. Crea un nuovo file JavaScript allo stesso livello della directory del tuo file index.html. Chiamalo `custom-player.js`.
2. All'inizio di questo file, inserisci il seguente codice:

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

   Qui stiamo creando delle costanti per tenere i riferimenti a tutti gli oggetti che vogliamo manipolare. Abbiamo tre gruppi:

   - L'elemento `<video>`, e la barra dei controlli.
   - I pulsanti play/pausa, stop, rewind, e fast forward.
   - L'involucro timer esterno `<div>`, il display digital timer `<span>`, e il `<div>` interno che diventa più largo mentre il tempo passa.

3. Successivamente, inserisci il seguente codice in fondo al tuo codice:

   ```js
   media.removeAttribute("controls");
   controls.style.visibility = "visible";
   ```

   Queste due righe rimuovono i controlli predefiniti del browser dal video e rendono visibili i controlli personalizzati.

#### Riproduzione e pausa del video

Implementiamo probabilmente il controllo più importante — il pulsante play/pausa.

1. Per prima cosa, aggiungi quanto segue in fondo al tuo codice, in modo che la funzione `playPauseMedia()` venga invocata quando si clicca sul pulsante play:

   ```js
   play.addEventListener("click", playPauseMedia);
   ```

2. Ora definiamo `playPauseMedia()` — aggiungi quanto segue, sempre in fondo al tuo codice:

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

   Qui utilizziamo un'istruzione [`if`](/it/docs/Web/JavaScript/Reference/Statements/if...else) per verificare se il video è in pausa. La proprietà [`HTMLMediaElement.paused`](/it/docs/Web/API/HTMLMediaElement/paused) restituisce true se il media è in pausa, il che avviene ogni volta che il video non è in riproduzione, inclusa quando è impostato a 0 durata dopo il primo caricamento. Se è in pausa, impostiamo il valore dell'attributo `data-icon` sul pulsante play a "u", che è un'icona "pausa", e invoca il metodo [`HTMLMediaElement.play()`](/it/docs/Web/API/HTMLMediaElement/play) per riprodurre il media.

   Al secondo clic, il pulsante verrà nuovamente commutato — l'icona "riproduci" verrà nuovamente mostrata, e il video verrà messo in pausa con [`HTMLMediaElement.pause()`](/it/docs/Web/API/HTMLMediaElement/pause).

#### Fermare il video

1. Successivamente, aggiungiamo funzionalità per gestire l'arresto del video. Aggiungi le seguenti righe [`addEventListener()`](/it/docs/Web/API/EventTarget/addEventListener) sotto la precedente che hai aggiunto:

   ```js
   stop.addEventListener("click", stopMedia);
   media.addEventListener("ended", stopMedia);
   ```

   L'evento [`click`](/it/docs/Web/API/Element/click_event) è ovvio — vogliamo fermare il video eseguendo la nostra funzione `stopMedia()` quando si clicca sul pulsante stop. Tuttavia, vogliamo anche fermare il video quando termina la riproduzione — questo è segnato dall'evento [`ended`](/it/docs/Web/API/HTMLMediaElement/ended_event) che viene attivato, quindi impostiamo anche un listener per eseguire la funzione quando questo evento viene attivato.

2. Successivamente, definiamo `stopMedia()` — aggiungi la seguente funzione sotto `playPauseMedia()`:

   ```js
   function stopMedia() {
     media.pause();
     media.currentTime = 0;
     play.setAttribute("data-icon", "P");
   }
   ```

   non c'è un metodo `stop()` sull'API HTMLMediaElement — l'equivalente è `pause()` il video e impostare la sua proprietà [`currentTime`](/it/docs/Web/API/HTMLMediaElement/currentTime) su 0. Impostare `currentTime` su un valore (in secondi) fa immediatamente saltare il media a quella posizione.

   Tutto quello che resta da fare è impostare l'icona visualizzata sull'icona "riproduci". Indipendentemente dal fatto che il video fosse in pausa o in riproduzione quando si preme il pulsante fermo, vuoi che sia pronto per essere riprodotto successivamente.

#### Seeking indietro e avanti

Ci sono molti modi in cui puoi implementare la funzionalità di riavvolgimento e avanzamento veloce; qui ti mostriamo un modo relativamente complesso per farlo, che non si rompe quando i diversi pulsanti vengono premuti in un ordine inaspettato.

1. Prima di tutto, aggiungi le seguenti due righe [`addEventListener()`](/it/docs/Web/API/EventTarget/addEventListener) sotto le precedenti:

   ```js
   rwd.addEventListener("click", mediaBackward);
   fwd.addEventListener("click", mediaForward);
   ```

2. Ora passiamo alle funzioni gestore degli eventi — aggiungi il seguente codice sotto le tue funzioni precedenti per definire `mediaBackward()` e `mediaForward()`:

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

   Noterai che prima di tutto inizializziamo due variabili — `intervalFwd` e `intervalRwd` — scoprirai a cosa servono più tardi.

   Vediamo `mediaBackward()` (la funzionalità di `mediaForward()` è esattamente la stessa, ma al contrario):

   1. Cancelliamo tutte le classi e gli intervalli impostati sulla funzionalità di avanzamento veloce — lo facciamo perché se premiamo il pulsante `rwd` dopo aver premuto il pulsante `fwd`, vogliamo cancellare qualsiasi funzionalità di avanzamento veloce e sostituirla con la funzionalità di riavvolgimento. Se provassimo a fare entrambe le cose contemporaneamente, il lettore si bloccherebbe.
   2. Usiamo un'istruzione `if` per verificare se la classe `active` è stata impostata sul pulsante `rwd`, indicando che è già stato premuto. La [`classList`](/it/docs/Web/API/Element/classList) è una proprietà piuttosto utile che esiste su ogni elemento — contiene un elenco di tutte le classi impostate sull'elemento, così come metodi per aggiungere/rimuovere classi, ecc. Utilizziamo il metodo `classList.contains()` per verificare se l'elenco contiene la classe `active`. Questo restituisce un risultato booleano `true`/`false`.
   3. Se `active` è stato impostato sul pulsante `rwd`, lo rimuoviamo utilizzando `classList.remove()`, cancelliamo l'intervallo che è stato impostato quando il pulsante è stato premuto per la prima volta (vedi sotto per una spiegazione più dettagliata), e utilizziamo [`HTMLMediaElement.play()`](/it/docs/Web/API/HTMLMediaElement/play) per annullare il riavvolgimento e avviare il video normalmente.
   4. Se non è ancora stato impostato, aggiungiamo la classe `active` al pulsante `rwd` utilizzando `classList.add()`, mettiamo il video in pausa utilizzando [`HTMLMediaElement.pause()`](/it/docs/Web/API/HTMLMediaElement/pause), quindi impostiamo la variabile `intervalRwd` per essere uguale a una chiamata [`setInterval()`](/it/docs/Web/API/Window/setInterval). Quando invocato, `setInterval()` crea un intervallo attivo, il che significa che esegue la funzione data come primo parametro ogni x millisecondi, dove x è il valore del 2º parametro. Quindi qui stiamo eseguendo la funzione `windBackward()` ogni 200 millisecondi — useremo questa funzione per riavvolgere costantemente il video. Per fermare un [`setInterval()`](/it/docs/Web/API/Window/setInterval) in esecuzione, è necessario chiamare [`clearInterval()`](/it/docs/Web/API/Window/clearInterval), dando il nome identificativo dell'intervallo da cancellare, che in questo caso è il nome della variabile `intervalRwd` (vedi la chiamata `clearInterval()` all'inizio della funzione).

3. Infine, dobbiamo definire le funzioni `windBackward()` e `windForward()` invocate nelle chiamate `setInterval()`. Aggiungi quanto segue sotto le tue due funzioni precedenti:

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

   Ancora una volta, esamineremo solo la prima di queste funzioni poiché funzionano quasi esattamente allo stesso modo, ma in senso opposto. In `windBackward()` facciamo quanto segue — considera che quando l'intervallo è attivo, questa funzione viene eseguita una volta ogni 200 millisecondi.

   1. Iniziamo con un'istruzione `if` che controlla se il tempo corrente è inferiore a 3 secondi, cioè se riavvolgendo di altri tre secondi si supererebbe l'inizio del video. Questo causerebbe un comportamento strano, quindi se questo è il caso fermiamo la riproduzione del video chiamando `stopMedia()`, rimuoviamo la classe `active` dal pulsante rewind, e cancelliamo l'intervallo `intervalRwd` per fermare la funzionalità di riavvolgimento. Se non facessimo quest'ultimo passaggio, il video continuerebbe a riavvolgersi per sempre.
   2. Se il tempo corrente non è entro 3 secondi dall'inizio del video, togliamo tre secondi dal tempo corrente eseguendo `media.currentTime -= 3`. Quindi di fatto, stiamo riavvolgendo il video di 3 secondi, una volta ogni 200 millisecondi.

#### Aggiornamento del tempo trascorso

L'ultimo pezzo del nostro lettore multimediale da implementare è la visualizzazione del tempo trascorso. Per farlo eseguiremo una funzione per aggiornare i time displays ogni volta che l'evento [`timeupdate`](/it/docs/Web/API/HTMLMediaElement/timeupdate_event) viene attivato sull'elemento `<video>`. La frequenza con cui questo evento viene attivato dipende dal tuo browser, potenza della CPU, ecc. ([vedi questo post su Stack Overflow](https://stackoverflow.com/questions/9678177/how-often-does-the-timeupdate-event-fire-for-an-html5-video)).

Aggiungi la seguente riga `addEventListener()` subito sotto le altre:

```js
media.addEventListener("timeupdate", setTime);
```

Ora per definire la funzione `setTime()`. Aggiungi quanto segue in fondo al tuo file:

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

1. Per prima cosa, calcoliamo il numero di minuti e secondi nel valore [`HTMLMediaElement.currentTime`](/it/docs/Web/API/HTMLMediaElement/currentTime).
2. Quindi inizializziamo altre due variabili — `minuteValue` e `secondValue`. Utilizziamo {{jsxref("String/padStart", "padStart()")}} per rendere ciascun valore lungo 2 caratteri, anche se il valore numerico è solo una cifra.
3. Il valore del tempo reale da visualizzare è impostato come `minuteValue` più un carattere due punti più `secondValue`.
4. Il valore [`Node.textContent`](/it/docs/Web/API/Node/textContent) del timer è impostato sul valore del tempo, quindi viene visualizzato nell'interfaccia utente.
5. La lunghezza che dovremmo impostare per il `<div>` interno viene calcolata prima calcolando la larghezza del `<div>` esterno (la proprietà [`clientWidth`](/it/docs/Web/API/Element/clientWidth) di qualsiasi elemento conterrà la sua lunghezza), e poi moltiplicandola per il tempo corrente [`HTMLMediaElement.currentTime`](/it/docs/Web/API/HTMLMediaElement/currentTime) diviso per la durata totale [`HTMLMediaElement.duration`](/it/docs/Web/API/HTMLMediaElement/duration) del media.
6. Impostiamo la larghezza del `<div>` interno per essere uguale alla lunghezza calcolata della barra, più "px", quindi verrà impostata su quel numero di pixel.

#### Risolvere play e pausa

C'è un problema rimanente da risolvere. Se i pulsanti play/pausa o stop vengono premuti mentre la funzionalità di riavvolgimento o avanzamento veloce è attiva, semplicemente non funzionano. Come possiamo risolverlo in modo che annullino la funzionalità del pulsante `rwd`/`fwd` e riproducano/arrestino il video come ci si aspetterebbe? Questo è relativamente facile da risolvere.

Prima di tutto, aggiungi le seguenti righe all'interno della funzione `stopMedia()` — qualsiasi punto andrà bene:

```js
rwd.classList.remove("active");
fwd.classList.remove("active");
clearInterval(intervalRwd);
clearInterval(intervalFwd);
```

Ora aggiungi di nuovo le stesse righe, all'inizio della funzione `playPauseMedia()` (appena prima dell'inizio dell'istruzione `if`).

A questo punto, potresti cancellare le righe equivalenti dalle funzioni `windBackward()` e `windForward()`, poiché tale funzionalità è stata implementata nella funzione `stopMedia()` invece.

Nota: Potresti anche migliorare ulteriormente l'efficienza del codice creando una funzione separata che esegue queste righe, quindi chiamandole ovunque necessario, piuttosto che ripetere le righe più volte nel codice. Ma lasceremo a te questa ottimizzazione.

## Sommario

Credo che abbiamo insegnato abbastanza in questo articolo. L'API [`HTMLMediaElement`](/it/docs/Web/API/HTMLMediaElement) rende disponibile una grande quantità di funzionalità per la creazione di semplici lettori video e audio, e questo è solo la punta dell'iceberg. Guarda la sezione "Vedi anche" qui sotto per link a funzionalità più complesse ed interessanti.

Ecco alcuni suggerimenti per modi in cui potresti migliorare l'esempio esistente che abbiamo costruito:

1. La visualizzazione del tempo attualmente si interrompe se il video dura un'ora o più (beh, non visualizzerà ore; solo minuti e secondi). Riesci a capire come modificare l'esempio per farlo visualizzare le ore?
2. Poiché gli elementi `<audio>` hanno la stessa funzionalità [`HTMLMediaElement`](/it/docs/Web/API/HTMLMediaElement) disponibile, potresti facilmente fare in modo che questo lettore funzioni anche per un elemento `<audio>`. Prova a farlo.
3. Riesci a trovare un modo per trasformare l'elemento `<div>` interno del timer in una vera barra di ricerca/scroller — cioè, quando clicchi da qualche parte sulla barra, salta a quella posizione relativa nella riproduzione del video? Come suggerimento, puoi trovare i valori X e Y dei lati sinistro/destro e superiore/inferiore dell'elemento tramite il metodo [`getBoundingClientRect()`](/it/docs/Web/API/Element/getBoundingClientRect), e puoi trovare le coordinate di un clic del mouse tramite l'oggetto evento dell'evento click, chiamato sull'oggetto [`Document`](/it/docs/Web/API/Document). Per esempio:

   ```js
   document.onclick = function (e) {
     console.log(e.x, e.y);
   };
   ```

## Vedi anche

- [`HTMLMediaElement`](/it/docs/Web/API/HTMLMediaElement)
- [Video e audio HTML](/it/docs/Learn_web_development/Core/Structuring_content/HTML_video_and_audio) — guida semplice a `<video>` e `<audio>` in HTML.
- [Consegna audio e video](/it/docs/Web/Media/Guides/Audio_and_video_delivery) — guida dettagliata alla consegna di media all'interno del browser, con molti consigli, trucchi e link ad altri tutorial più avanzati.
- [Manipolazione di audio e video](/it/docs/Web/Media/Guides/Audio_and_video_manipulation) — guida dettagliata alla manipolazione di audio e video, ad es. con [Canvas API](/it/docs/Web/API/Canvas_API), [Web Audio API](/it/docs/Web/API/Web_Audio_API) e altro.
- Pagine di riferimento di {{htmlelement("video")}} e {{htmlelement("audio")}}.
- [Guida ai tipi e formati di media sul web](/it/docs/Web/Media/Guides/Formats)

{{PreviousMenuNext("Learn_web_development/Extensions/Client-side_APIs/Introduction", "Learn_web_development/Extensions/Client-side_APIs/Drawing_graphics", "Learn_web_development/Extensions/Client-side_APIs")}}
