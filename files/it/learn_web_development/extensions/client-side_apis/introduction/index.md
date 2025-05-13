---
title: Introduzione alle API web
short-title: Introduction
slug: Learn_web_development/Extensions/Client-side_APIs/Introduction
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Extensions/Client-side_APIs/Video_and_audio_APIs", "Learn_web_development/Extensions/Client-side_APIs")}}

Per prima cosa, inizieremo esaminando le API da un livello alto — cosa sono, come funzionano, come utilizzarle nel suo codice e come sono strutturate? Daremo anche un'occhiata a quali sono le diverse principali classi di API e a quale tipo di usi hanno.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, in particolare le <a href="/it/docs/Learn_web_development/Core/Scripting/Object_basics">basi degli oggetti in JavaScript</a> e la copertura delle API di base come il <a href="/it/docs/Learn_web_development/Core/Scripting/DOM_scripting">DOM scripting</a> e le <a href="/it/docs/Learn_web_development/Core/Scripting/Network_requests">richieste di rete</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Cosa sono le Web API e cosa si può fare con esse.</li>
          <li>Come vengono utilizzate le API.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Cosa sono le API?

Le interfacce di programmazione delle applicazioni (API) sono costrutti messi a disposizione nei linguaggi di programmazione per consentire agli sviluppatori di creare funzionalità complesse in modo più semplice. Astrazionano il codice più complesso, fornendo una sintassi più semplice da utilizzare al suo posto.

Come esempio nel mondo reale, pensi alla fornitura di elettricità nella sua casa, appartamento o altra abitazione. Se desidera utilizzare un apparecchio elettrico in casa, lo collega a una presa elettrica e funziona. Non cerca di collegarlo direttamente alla rete elettrica — farlo sarebbe davvero inefficiente e, se non è un elettricista, difficile e pericoloso da tentare.

![Due prese multi-plug sono collegate a due diverse prese elettriche. Ogni presa multi-plug ha un alloggio per la spina in cima e uno sul lato anteriore. Due spine sono inserite in ciascun alloggio multi-plug.](plug-socket.png)

_Sorgente dell'immagine: [Presa elettrica sovraccarica](https://www.flickr.com/photos/easy-pics/9518184890/in/photostream/lightbox/) di [The Clear Communication People](https://www.flickr.com/photos/easy-pics/), su Flickr._

Allo stesso modo, se si vuole ad esempio programmare della grafica 3D, è molto più semplice farlo utilizzando un'API scritta in un linguaggio di alto livello come JavaScript o Python, piuttosto che cercare di scrivere direttamente codice di basso livello (come C o C++) che controlla direttamente la GPU del computer o altre funzioni grafiche.

> [!NOTE]
> Veda anche la {{Glossary("API", "voce del glossario API")}} per una descrizione più dettagliata.

### Le API in JavaScript lato client

In particolare, JavaScript lato client ha molte API disponibili per esso — queste non fanno parte del linguaggio JavaScript stesso, ma sono costruite sopra il nucleo del linguaggio JavaScript, fornendole poteri extra da utilizzare nel suo codice JavaScript. Generalmente si suddividono in due categorie:

- **API del browser** sono integrate nel suo browser web e sono in grado di esporre dati dal browser e dall'ambiente circostante del computer e fare cose complesse e utili con essi. Ad esempio, la [Web Audio API](/it/docs/Web/API/Web_Audio_API) fornisce costrutti JavaScript per manipolare l'audio nel browser — prendere una traccia audio, alterarne il volume, applicarle effetti, ecc. In background, il browser sta effettivamente utilizzando del codice di livello inferiore complesso (ad es., C++ o Rust) per eseguire l'elaborazione audio effettiva. Ma ancora una volta, questa complessità viene astrazionata dall'API.

- **API di terze parti** non sono integrate nel browser per impostazione predefinita, e generalmente si deve recuperare il loro codice e le informazioni da qualche parte sul Web. Ad esempio, l'[API di Google Maps](https://developers.google.com/maps/documentation/javascript) consente di fare cose come visualizzare una mappa interattiva nel suo ufficio sul suo sito web. Fornisce un set speciale di costrutti che può utilizzare per interrogare il servizio di Google Maps e restituire informazioni specifiche.

![Uno screenshot del browser con la home page del browser Firefox aperta. Ci sono API integrate nel browser per impostazione predefinita. Le API di terze parti non sono integrate nel browser per impostazione predefinita. Il loro codice e le informazioni devono essere recuperati da qualche parte sul web per utilizzarle.](browser.png)

### Relazione tra JavaScript, API e altri strumenti JavaScript

In precedenza abbiamo parlato di cosa sono le API JavaScript lato client e di come si relazionano al linguaggio JavaScript. Riepiloghiamo questo per renderlo più chiaro, e menzioniamo anche dove si inseriscono gli altri strumenti JavaScript:

- JavaScript — Un linguaggio di scripting di alto livello integrato nei browser che le consente di implementare funzionalità su pagine/app web. Noti che JavaScript è disponibile anche in altri ambienti di programmazione, come [Node](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction).

- API del browser — costrutti integrati nel browser che si trovano sopra il linguaggio JavaScript e le consentono di implementare funzionalità in modo più semplice.

- API di terze parti — costrutti integrati nelle piattaforme di terze parti (ad esempio, Disqus, Facebook) che le consentono di utilizzare alcune delle funzionalità di quelle piattaforme nelle sue pagine web (ad esempio, visualizzare i suoi commenti di Disqus su una pagina web).

- Librerie JavaScript — Solitamente uno o più file JavaScript contenenti [funzioni personalizzate](/it/docs/Learn_web_development/Core/Scripting/Functions) che può allegare alla sua pagina web per velocizzare o abilitare la scrittura di funzionalità comuni. Esempi includono jQuery, Mootools e React.

- Framework JavaScript — Il passo successivo rispetto alle librerie, i framework JavaScript (ad es., Angular ed Ember) tendono ad essere pacchetti di HTML, CSS, JavaScript e altre tecnologie che installa e poi utilizza per scrivere un'intera applicazione web da zero. La differenza principale tra una libreria e un framework è l'"Inversione del Controllo". Quando richiama un metodo da una libreria, lo sviluppatore è in controllo. Con un framework, il controllo è invertito: il framework richiama il codice dello sviluppatore.

## Cosa possono fare le API?

Ci sono un numero enorme di API disponibili nei browser moderni che le consentono di fare una grande varietà di cose nel suo codice. Può vedere questo dando un'occhiata alla [pagina dell'indice delle API di MDN](/it/docs/Web/API).

### API comuni del browser

In particolare, le categorie più comuni di API del browser che utilizzerà (e che copriremo in questo modulo in maggiore dettaglio) sono:

- **API per la manipolazione dei documenti** caricati nel browser. L'esempio più ovvio è l'[API DOM (Document Object Model)](/it/docs/Web/API/Document_Object_Model), che le consente di manipolare HTML e CSS — creare, rimuovere e cambiare HTML, applicare dinamicamente nuovi stili alla pagina, ecc. Ogni volta che vede una finestra popup apparire su una pagina o qualche nuovo contenuto visualizzato, ad esempio, quello è il DOM in azione. Scopri di più su questi tipi di API nell'[introduzione al DOM scripting](/it/docs/Learn_web_development/Core/Scripting/DOM_scripting).

- **API che recuperano dati dal server** per aggiornare piccole sezioni di una pagina web da sole sono molto comunemente utilizzate. Questo dettaglio apparentemente piccolo ha avuto un grande impatto sulle prestazioni e sul comportamento dei siti — se ha solo bisogno di aggiornare un elenco di stock o un elenco di nuove storie disponibili, farlo istantaneamente senza dover ricaricare l'intera pagina dal server può rendere il sito o l'app molto più reattiva e "rapida". L'API principale utilizzata per questo è l'[API Fetch](/it/docs/Web/API/Fetch_API), anche se il codice più vecchio potrebbe ancora utilizzare l'API [`XMLHttpRequest`](/it/docs/Web/API/XMLHttpRequest). Potrebbe anche incontrare il termine **AJAX**, che descrive questa tecnica. Scopra di più su tali API nella [realizzazione di richieste di rete con JavaScript](/it/docs/Learn_web_development/Core/Scripting/Network_requests).

- **API per disegnare e manipolare grafica** sono ampiamente supportate nei browser — le più popolari sono [Canvas](/it/docs/Web/API/Canvas_API) e [WebGL](/it/docs/Web/API/WebGL_API), che le consentono di aggiornare programmaticamente i dati dei pixel contenuti in un elemento HTML {{htmlelement("canvas")}} per creare scene 2D e 3D. Ad esempio, potrebbe disegnare forme come rettangoli o cerchi, importare un'immagine sul canvas e applicarle un filtro come seppia o scala di grigi utilizzando l'API Canvas, o creare una scena 3D complessa con illuminazione e texture utilizzando WebGL. Tali API sono spesso combinate con API per creare loop di animazione (come [`window.requestAnimationFrame()`](/it/docs/Web/API/Window/requestAnimationFrame)) e altre per creare scene in costante aggiornamento come cartoni animati e giochi.

- **[API Audio e Video](/it/docs/Web/Media/Guides/Audio_and_video_delivery)** come [`HTMLMediaElement`](/it/docs/Web/API/HTMLMediaElement), la [Web Audio API](/it/docs/Web/API/Web_Audio_API) e [WebRTC](/it/docs/Web/API/WebRTC_API) le consentono di fare cose davvero interessanti con i multimedia, come creare controlli UI personalizzati per riprodurre audio e video, visualizzare tracce di testo come didascalie e sottotitoli insieme ai suoi video, prendere video dalla sua webcam per manipolarlo tramite un canvas (vedi sopra) o visualizzarlo sul computer di qualcun altro in una conferenza web, o aggiungere effetti alle tracce audio (come amplificazione, distorsione, panning, ecc.).

- **API dei dispositivi** le consentono di interagire con l'hardware del dispositivo: ad esempio, accedere al GPS del dispositivo per trovare la posizione dell'utente utilizzando l'[API di Geolocalizzazione](/it/docs/Web/API/Geolocation_API).

- **API di archiviazione lato client** le consentono di archiviare dati sul lato client, in modo che possa creare un'app che salverà il suo stato tra i caricamenti della pagina e forse funzionerà anche quando il dispositivo è offline. Sono disponibili diverse opzioni, ad es., archiviazione semplice nome/valore con la [Web Storage API](/it/docs/Web/API/Web_Storage_API) e archiviazione in database più complessa con la [IndexedDB API](/it/docs/Web/API/IndexedDB_API).

### API comuni di terze parti

Le API di terze parti sono disponibili in una grande varietà; alcune delle più popolari che probabilmente utilizzerà prima o poi sono:

- API di mappe, come [Mapquest](https://developer.mapquest.com/) e l'[API di Google Maps](https://developers.google.com/maps/), che le consentono di fare di tutto con le mappe sulle sue pagine web.

- La [suite di API di Facebook](https://developers.facebook.com/docs/), che le consente di utilizzare varie parti dell'ecosistema Facebook per beneficiare della sua app, come fornendo l'accesso all'app tramite login Facebook, accettando pagamenti in-app, avviando campagne pubblicitarie mirate, ecc.

- L'[API di Telegram](https://core.telegram.org/api), che le consente di incorporare contenuti dai canali Telegram sul suo sito web, oltre a fornire supporto per i bot.

- L'[API di YouTube](https://developers.google.com/youtube/), che le consente di incorporare video di YouTube sul suo sito, cercare su YouTube, creare playlist e altro.

- L'[API di Pinterest](https://developers.pinterest.com/), che fornisce strumenti per gestire bacheche e pin di Pinterest da includere nel suo sito.

- L'[API di Twilio](https://www.twilio.com/docs), che fornisce una struttura per integrare funzionalità di chiamate vocali e video nella sua app, inviare SMS/MMS dalle sue app e altro ancora.

- L'[API di Disqus](https://disqus.com/api/docs/), che fornisce una piattaforma di commenti che può essere integrata nel suo sito.

- L'[API di Mastodon](https://docs.joinmastodon.org/api/), che le consente di manipolare in modo programmatico le caratteristiche del social network Mastodon.

- L'[API di IFTTT](https://ifttt.com/developers), che le permette di integrare molteplici API attraverso un'unica piattaforma.

## Come funzionano le API?

Le diverse API JavaScript funzionano in modi leggermente diversi, ma generalmente hanno caratteristiche comuni e temi simili su come funzionano.

### Sono basate su oggetti

Il suo codice interagisce con le API utilizzando uno o più [oggetti JavaScript](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects), che servono come contenitori per i dati utilizzati dall'API (contenuti nelle proprietà degli oggetti) e la funzionalità messa a disposizione dall'API (contenuti nei metodi degli oggetti).

> [!NOTE]
> Se non è già familiare con il funzionamento degli oggetti, dovrebbe andare a lavorare attraverso il nostro modulo [oggetti JavaScript](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects) prima di continuare.

Torniamo all'esempio della Web Audio API — questa è una API abbastanza complessa, che consiste in una serie di oggetti. I più evidenti sono:

- [`AudioContext`](/it/docs/Web/API/AudioContext), che rappresenta un [audio grafo](/it/docs/Web/API/Web_Audio_API/Basic_concepts_behind_Web_Audio_API#audio_graphs) che può essere utilizzato per manipolare l'audio che viene riprodotto nel browser, e ha un numero di metodi e proprietà disponibili per manipolare quell'audio.

- [`MediaElementAudioSourceNode`](/it/docs/Web/API/MediaElementAudioSourceNode), che rappresenta un elemento {{htmlelement("audio")}} contenente il suono che desidera riprodurre e manipolare all'interno del contesto audio.

- [`AudioDestinationNode`](/it/docs/Web/API/AudioDestinationNode), che rappresenta la destinazione dell'audio, cioè il dispositivo sul suo computer che lo riprodurrà effettivamente — solitamente i suoi altoparlanti o le cuffie.

Come interagiscono questi oggetti? Se guarda il nostro [esempio semplice di audio web](https://github.com/mdn/learning-area/blob/main/javascript/apis/introduction/web-audio/index.html) ([veda anche in tempo reale](https://mdn.github.io/learning-area/javascript/apis/introduction/web-audio/)), vedrà prima di tutto il seguente HTML:

```html
<audio src="outfoxing.mp3"></audio>

<button class="paused">Play</button>
<br />
<input type="range" min="0" max="1" step="0.01" value="1" class="volume" />
```

Includiamo, per prima cosa, un elemento `<audio>` con cui incorporiamo un MP3 nella pagina. Non includiamo alcun controllo del browser predefinito. Successivamente, includiamo un {{htmlelement("button")}} che utilizzeremo per riprodurre e interrompere la musica, e un elemento {{htmlelement("input")}} di tipo range, che utilizzeremo per regolare il volume del brano mentre è in riproduzione.

Successivamente, diamo un'occhiata al JavaScript per questo esempio.

Iniziamo creando un'istanza di `AudioContext` all'interno della quale manipolare il nostro brano:

```js
const audioCtx = new AudioContext();
```

Successivamente, creiamo costanti che memorizzano i riferimenti ai nostri elementi `<audio>`, `<button>` e `<input>`, e utilizziamo il metodo [`AudioContext.createMediaElementSource()`](/it/docs/Web/API/AudioContext/createMediaElementSource) per creare un `MediaElementAudioSourceNode` che rappresenta la fonte del nostro audio — l'elemento `<audio>` che verrà riprodotto da:

```js
const audioElement = document.querySelector("audio");
const playBtn = document.querySelector("button");
const volumeSlider = document.querySelector(".volume");

const audioSource = audioCtx.createMediaElementSource(audioElement);
```

Successivamente includiamo un paio di gestori di eventi che servono per alternare tra riproduci e pausa quando il pulsante viene premuto e ripristinare la visualizzazione all'inizio quando il brano è finito di essere riprodotto:

```js
// play/pause audio
playBtn.addEventListener("click", () => {
  // check if context is in suspended state (autoplay policy)
  if (audioCtx.state === "suspended") {
    audioCtx.resume();
  }

  // if track is stopped, play it
  if (playBtn.getAttribute("class") === "paused") {
    audioElement.play();
    playBtn.setAttribute("class", "playing");
    playBtn.textContent = "Pause";
    // if track is playing, stop it
  } else if (playBtn.getAttribute("class") === "playing") {
    audioElement.pause();
    playBtn.setAttribute("class", "paused");
    playBtn.textContent = "Play";
  }
});

// if track ends
audioElement.addEventListener("ended", () => {
  playBtn.setAttribute("class", "paused");
  playBtn.textContent = "Play";
});
```

> [!NOTE]
> Alcuni di voi potrebbero notare che i metodi `play()` e `pause()` utilizzati per riprodurre e interrompere il brano non fanno parte della Web Audio API; fanno parte dell'API [`HTMLMediaElement`](/it/docs/Web/API/HTMLMediaElement), che è diversa ma strettamente correlata.

Successivamente, creiamo un oggetto [`GainNode`](/it/docs/Web/API/GainNode) utilizzando il metodo [`AudioContext.createGain()`](/it/docs/Web/API/BaseAudioContext/createGain), che può essere utilizzato per regolare il volume dell’audio che viene trasmesso attraverso di esso, e creiamo un altro gestore di eventi che cambia il valore del guadagno (volume) del grafo audio ogni volta che il valore del cursore viene cambiato:

```js
// volume
const gainNode = audioCtx.createGain();

volumeSlider.addEventListener("input", () => {
  gainNode.gain.value = volumeSlider.value;
});
```

L'ultima cosa da fare per far funzionare questo è collegare i diversi nodi nel grafo audio, cosa che viene fatta utilizzando il metodo [`AudioNode.connect()`](/it/docs/Web/API/AudioNode/connect) disponibile per ogni tipo di nodo:

```js
audioSource.connect(gainNode).connect(audioCtx.destination);
```

L’audio inizia dalla sorgente, che è poi collegata al nodo di guadagno in modo che il volume dell’audio possa essere regolato. Il nodo di guadagno è poi collegato al nodo di destinazione in modo che il suono possa essere riprodotto sul suo computer (la proprietà [`AudioContext.destination`](/it/docs/Web/API/BaseAudioContext/destination) rappresenta qualsiasi sia il default [`AudioDestinationNode`](/it/docs/Web/API/AudioDestinationNode) disponibile sull'hardware del suo computer, ad es., i suoi altoparlanti).

### Hanno punti di ingresso riconoscibili

Quando utilizza un'API, deve assicurarsi di sapere dove si trova il punto di ingresso per l'API. Nella Web Audio API, questo è abbastanza semplice — è l'oggetto [`AudioContext`](/it/docs/Web/API/AudioContext), che deve essere utilizzato per eseguire qualsiasi manipolazione dell’audio.

Anche l'API Document Object Model (DOM) ha un punto di ingresso semplice — le sue funzionalità tendono ad essere trovate agganciate all'oggetto [`Document`](/it/docs/Web/API/Document), o un'istanza di un elemento HTML che desidera influenzare in qualche modo, ad esempio:

```js
const em = document.createElement("em"); // create a new em element
const para = document.querySelector("p"); // reference an existing p element
em.textContent = "Hello there!"; // give em some text content
para.appendChild(em); // embed em inside para
```

Anche l'[API Canvas](/it/docs/Web/API/Canvas_API) si basa sull'ottenimento di un oggetto contesto da utilizzare per manipolare le cose, anche se in questo caso è un contesto grafico piuttosto che un contesto audio. Il suo oggetto contesto è creato ottenendo un riferimento all'elemento {{htmlelement("canvas")}} su cui desidera disegnare, e quindi chiamando il metodo [`HTMLCanvasElement.getContext()`](/it/docs/Web/API/HTMLCanvasElement/getContext):

```js
const canvas = document.querySelector("canvas");
const ctx = canvas.getContext("2d");
```

Qualsiasi cosa vogliamo fare con il canvas è poi ottenuta chiamando le proprietà e i metodi dell'oggetto contesto (che è un'istanza di [`CanvasRenderingContext2D`](/it/docs/Web/API/CanvasRenderingContext2D)), ad esempio:

```js
Ball.prototype.draw = function () {
  ctx.beginPath();
  ctx.fillStyle = this.color;
  ctx.arc(this.x, this.y, this.size, 0, 2 * Math.PI);
  ctx.fill();
};
```

> [!NOTE]
> Può vedere questo codice in azione nella nostra demo [palle rimbalzanti](https://github.com/mdn/learning-area/blob/main/javascript/apis/introduction/bouncing-balls.html) (veda anche [esecuzione dal vivo](https://mdn.github.io/learning-area/javascript/apis/introduction/bouncing-balls.html)).

### Usano spesso eventi per gestire i cambiamenti di stato

Abbiamo già discusso di eventi in precedenza nel corso nel nostro articolo [Introduzione agli eventi](/it/docs/Learn_web_development/Core/Scripting/Events), che analizza in dettaglio cosa sono gli eventi web lato client e come vengono utilizzati nel suo codice. Se non è già familiare con il funzionamento degli eventi dell'API web lato client, dovrebbe leggere questo articolo prima di continuare.

Alcune API web non contengono eventi, ma la maggior parte ne contiene almeno alcuni. Le proprietà del gestore che ci consentono di eseguire funzioni quando si verificano eventi sono generalmente elencate nel nostro materiale di riferimento in sezioni separate "Gestori di eventi".

Abbiamo già visto un certo numero di gestori di eventi in uso nel nostro esempio di Web Audio API sopra:

```js
// play/pause audio
playBtn.addEventListener("click", () => {
  // check if context is in suspended state (autoplay policy)
  if (audioCtx.state === "suspended") {
    audioCtx.resume();
  }

  // if track is stopped, play it
  if (playBtn.getAttribute("class") === "paused") {
    audioElement.play();
    playBtn.setAttribute("class", "playing");
    playBtn.textContent = "Pause";
    // if track is playing, stop it
  } else if (playBtn.getAttribute("class") === "playing") {
    audioElement.pause();
    playBtn.setAttribute("class", "paused");
    playBtn.textContent = "Play";
  }
});

// if track ends
audioElement.addEventListener("ended", () => {
  playBtn.setAttribute("class", "paused");
  playBtn.textContent = "Play";
});
```

### Hanno meccanismi di sicurezza aggiuntivi dove appropriato

Le funzionalità delle WebAPI sono soggette alle stesse considerazioni di sicurezza di JavaScript e altre tecnologie web (ad esempio la [politica dello stesso origine](/it/docs/Web/Security/Same-origin_policy)), ma a volte hanno meccanismi di sicurezza aggiuntivi in atto. Ad esempio, alcune delle WebAPI più moderne funzionano solo su pagine servite su HTTPS a causa della trasmissione di dati potenzialmente sensibili (esempi includono i [Service Workers](/it/docs/Web/API/Service_Worker_API) e [Push](/it/docs/Web/API/Push_API)).

Inoltre, alcune WebAPI richiedono il permesso per essere abilitate dall'utente una volta che le chiamate a esse vengono effettuate nel suo codice. Ad esempio, l'[API di Notifiche](/it/docs/Web/API/Notifications_API) chiede il permesso utilizzando una finestra di dialogo popup:

![Uno screenshot della finestra di dialogo popup di notifiche fornita dall'API di Notifiche del browser. Il sito web 'mdn.github.io' chiede il permesso di inviare notifiche all'agent dell'utente con un X per chiudere la finestra di dialogo e un menu a discesa di opzioni con 'ricevere sempre notifiche' selezionato per default.](notification-permission.png)

La Web Audio e le API [`HTMLMediaElement`](/it/docs/Web/API/HTMLMediaElement) sono soggette a un meccanismo di sicurezza chiamato [politica di autoplay](/it/docs/Web/API/Web_Audio_API/Best_practices#autoplay_policy) — questo significa essenzialmente che non può riprodurre automaticamente l'audio quando una pagina viene caricata — deve consentire agli utenti di avviare la riproduzione audio tramite un controllo come un pulsante. Questo viene fatto perché l'audio autoplay è di solito davvero fastidioso e non dovremmo realmente sottoporre i nostri utenti a esso.

> [!NOTE]
> A seconda di quanto è rigoroso il browser, tali meccanismi di sicurezza potrebbero persino impedire che l'esempio funzioni localmente, cioè, se carica il file di esempio locale nel suo browser anziché eseguirlo da un server web. Al momento della scrittura, il nostro esempio di API Web Audio non funzionerebbe localmente su Google Chrome — abbiamo dovuto caricarlo su GitHub prima che funzionasse.

## Sintesi

A questo punto, dovrebbe avere un'idea chiara di cosa siano le API, come funzionano e cosa può fare con loro nel suo codice JavaScript. Probabilmente sarà entusiasta di iniziare a fare effettivamente delle cose divertenti con specifiche API, quindi andiamo! Il prossimo passo sarà esplorare le API video e audio.

{{NextMenu("Learn_web_development/Extensions/Client-side_APIs/Video_and_audio_APIs", "Learn_web_development/Extensions/Client-side_APIs")}}
