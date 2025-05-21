---
title: Introduzione alle API web
short-title: Introduction
slug: Learn_web_development/Extensions/Client-side_APIs/Introduction
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Extensions/Client-side_APIs/Video_and_audio_APIs", "Learn_web_development/Extensions/Client-side_APIs")}}

Per iniziare, esamineremo le API da un alto livello — cosa sono, come funzionano, come utilizzarle nel proprio codice e come sono strutturate? Vedremo anche quali sono le diverse principali classi di API e a quali utilizzi si prestano.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, specialmente le basi degli <a href="/it/docs/Learn_web_development/Core/Scripting/Object_basics">oggetti JavaScript</a> e la copertura delle API core come <a href="/it/docs/Learn_web_development/Core/Scripting/DOM_scripting">scripting DOM</a> e <a href="/it/docs/Learn_web_development/Core/Scripting/Network_requests">Richieste di rete</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Cosa sono le API web e cosa si può fare con esse.</li>
          <li>Come vengono usate le API.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Cosa sono le API?

Le Application Programming Interfaces (API) sono costrutti resi disponibili nei linguaggi di programmazione per permettere agli sviluppatori di creare funzionalità complesse più facilmente. Astraggono il codice più complesso da voi, fornendo una sintassi più semplice da utilizzare al suo posto.

Come esempio nel mondo reale, pensate alla fornitura di elettricità nella vostra casa, appartamento o altro luogo di abitazione. Se volete usare un elettrodomestico, lo collegate a una presa e funziona. Non cercate di connetterlo direttamente alla fonte di alimentazione, farlo sarebbe davvero inefficiente e, se non siete elettricisti, difficile e pericoloso.

![Due multiprese sono collegate a due diverse prese elettriche. Ogni multipresa ha uno slot di collegamento sulla sua parte superiore e frontale. Due spine sono collegate a ciascuna multipresa.](plug-socket.png)

_Sorgente immagine: [Presa elettrica sovraccarica](https://www.flickr.com/photos/easy-pics/9518184890/in/photostream/lightbox/) di [The Clear Communication People](https://www.flickr.com/photos/easy-pics/), su Flickr._

Allo stesso modo, se volete, per esempio, programmare qualche grafica 3D, è molto più semplice farlo utilizzando un'API scritta in un linguaggio di livello superiore come JavaScript o Python, piuttosto che tentare di scrivere codice di basso livello (come C o C++) che controlli direttamente la GPU o altre funzioni grafiche del computer.

> [!NOTE]
> Vedere anche l'{{Glossary("API", "entry del glossario sulle API")}} per una descrizione più dettagliata.

### API in JavaScript lato client

Il JavaScript lato client, in particolare, ha molte API disponibili — queste non fanno parte del linguaggio JavaScript stesso, piuttosto sono costruite sopra il linguaggio base JavaScript, fornendovi superpoteri extra da utilizzare nel vostro codice JavaScript. Generalmente si suddividono in due categorie:

- **API del browser** sono integrate nel vostro browser web e sono in grado di esporre dati dal browser e dall'ambiente informatico circostante, e fare cose complesse utili con esso. Per esempio, la [Web Audio API](/it/docs/Web/API/Web_Audio_API) fornisce costrutti JavaScript per manipolare l'audio nel browser — prendere una traccia audio, modificarne il volume, applicargli effetti, ecc. In sottofondo, il browser sta effettivamente utilizzando del codice complesso di livello inferiore (ad es., C++ o Rust) per fare il reale processamento audio. Ma ancora una volta, questa complessità è astratta da voi dall'API.
- **API di terze parti** non sono integrate nel browser di default e generalmente è necessario recuperare il loro codice e informazioni da qualche parte sul Web. Per esempio, la [Google Maps API](https://developers.google.com/maps/documentation/javascript) vi consente di fare cose come visualizzare una mappa interattiva del vostro ufficio sul vostro sito web. Fornisce un insieme speciale di costrutti che potete usare per interrogare il servizio Google Maps e restituire informazioni specifiche.

![Uno screenshot del browser con la home page del browser Firefox aperta. Ci sono API integrate nel browser di default. Le API di terze parti non sono integrate nel browser di default. Il loro codice e le informazioni devono essere recuperati da qualche parte sul web per utilizzarli.](browser.png)

### Relazione tra JavaScript, API e altri strumenti JavaScript

Quindi, sopra abbiamo parlato di cosa sono le API JavaScript lato client e come si relazionano al linguaggio JavaScript. Riassumiamo questo per renderlo più chiaro, e menzioniamo anche dove si inquadrano altri strumenti JavaScript:

- JavaScript — Un linguaggio di scripting di alto livello integrato nei browser che permette di implementare funzionalità su pagine/app web. Nota che JavaScript è disponibile anche in altri ambienti di programmazione, come [Node](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction).
- API del browser — costrutti integrati nel browser che si appoggiano al linguaggio JavaScript e vi permettono di implementare funzionalità in modo più semplice.
- API di terze parti — costrutti integrati in piattaforme di terze parti (es. Disqus, Facebook) che vi permettono di usare alcune delle funzionalità di quelle piattaforme nelle vostre pagine web (ad esempio, visualizzare i vostri commenti Disqus su una pagina web).
- Librerie JavaScript — Di solito uno o più file JavaScript contenenti [funzioni personalizzate](/it/docs/Learn_web_development/Core/Scripting/Functions) che potete allegare alla vostra pagina web per velocizzare o abilitare l'implementazione di funzionalità comuni. Esempi includono jQuery, Mootools e React.
- Framework JavaScript — Il passo successivo rispetto alle librerie, i framework JavaScript (es., Angular e Ember) tendono ad essere pacchetti di HTML, CSS, JavaScript e altre tecnologie che installate e poi usate per scrivere un'intera applicazione web da zero. La differenza chiave tra una libreria e un framework è "Inversione di Controllo". Quando si chiama un metodo da una libreria, lo sviluppatore è in controllo. Con un framework, il controllo è invertito: il framework chiama il codice dello sviluppatore.

## Cosa possono fare le API?

Ci sono un numero enorme di API disponibili nei browser moderni che permettono di fare una vasta gamma di cose nel vostro codice. Potete vedere questo dando un'occhiata alla [pagina dell'indice delle API di MDN](/it/docs/Web/API).

### API del browser comuni

In particolare, le categorie più comuni di API del browser che utilizzerete (e che copriremo in questo modulo in modo più dettagliato) sono:

- **API per manipolare documenti** caricati nel browser. L'esempio più ovvio è l'[API DOM (Document Object Model)](/it/docs/Web/API/Document_Object_Model), che permette di manipolare HTML e CSS — creare, rimuovere e modificare HTML, applicare dinamicamente nuovi stili alla vostra pagina, ecc. Ogni volta che vedete una finestra popup apparire su una pagina o qualche nuovo contenuto visualizzato, per esempio, è il DOM in azione. Scoprite di più su questo tipo di API nell'[introduzione allo scripting DOM](/it/docs/Learn_web_development/Core/Scripting/DOM_scripting).
- **API che raccolgono dati dal server** per aggiornare piccole sezioni di una pagina web in autonomia sono molto comunemente usate. Questo dettaglio apparentemente piccolo ha avuto un enorme impatto sulle prestazioni e sul comportamento dei siti — se avete solo bisogno di aggiornare un elenco di azioni o un elenco di nuove storie disponibili, farlo istantaneamente senza dover ricaricare l'intera pagina dal server può far apparire il sito o l'app molto più reattivo e "scattante". L'API principale usata per questo è la [Fetch API](/it/docs/Web/API/Fetch_API), anche se il codice più vecchio potrebbe ancora usare l'API [`XMLHttpRequest`](/it/docs/Web/API/XMLHttpRequest). Potreste anche incontrare il termine **AJAX**, che descrive questa tecnica. Scopri di più su tali API in [Fare richieste di rete con JavaScript](/it/docs/Learn_web_development/Core/Scripting/Network_requests).
- **API per disegnare e manipolare grafica** sono ampiamente supportate nei browser — le più popolari sono [Canvas](/it/docs/Web/API/Canvas_API) e [WebGL](/it/docs/Web/API/WebGL_API), che vi permettono di aggiornare programmaticamente i dati dei pixel contenuti in un elemento HTML {{htmlelement("canvas")}} per creare scene 2D e 3D. Per esempio, potreste disegnare forme come rettangoli o cerchi, importare un'immagine sulla canvas e applicarvi un filtro come seppia o scala di grigi usando l'API Canvas, o creare una scena 3D complessa con illuminazione e texture usando WebGL. Tali API sono spesso combinate con le API per creare loop di animazione (come [`window.requestAnimationFrame()`](/it/docs/Web/API/Window/requestAnimationFrame)) e altre per creare scene in continuo aggiornamento come cartoni animati e giochi.
- **[API Audio e Video](/it/docs/Web/Media/Guides/Audio_and_video_delivery)** come [`HTMLMediaElement`](/it/docs/Web/API/HTMLMediaElement), la [Web Audio API](/it/docs/Web/API/Web_Audio_API) e [WebRTC](/it/docs/Web/API/WebRTC_API) vi permettono di fare cose davvero interessanti con i multimedia come creare controlli UI personalizzati per riprodurre audio e video, visualizzare tracce di testo come sottotitoli e didascalie insieme ai vostri video, acquisire video dalla vostra webcam per essere manipolato tramite una canvas (vedi sopra) o visualizzato sul computer di qualcun altro in una web conference, o aggiungere effetti alle tracce audio (come guadagno, distorsione, panning, ecc.).
- **API dei dispositivi** vi permettono di interagire con l'hardware del dispositivo: per esempio, accedere al GPS del dispositivo per trovare la posizione dell'utente usando l'API [Geolocation](/it/docs/Web/API/Geolocation_API).
- **API di archiviazione lato client** permettono di memorizzare dati sul lato client, così potete creare un'app che salvi il suo stato tra i carichi di pagina, e forse funzioni anche quando il dispositivo è offline. Ci sono diverse opzioni disponibili, ad esempio, l'archiviazione semplice nome/valore con l'API [Web Storage](/it/docs/Web/API/Web_Storage_API), e l'archiviazione in database più complessa con l'API [IndexedDB](/it/docs/Web/API/IndexedDB_API).

### API di terze parti comuni

Le API di terze parti sono disponibili in una vasta varietà; alcune delle più popolari che potreste voler usare prima o poi sono:

- API di mappe, come [Mapquest](https://developer.mapquest.com/) e la [Google Maps API](https://developers.google.com/maps/), che vi permettono di fare ogni sorta di cose con le mappe nelle vostre pagine web.
- La [suite di API di Facebook](https://developers.facebook.com/docs/), che vi permette di utilizzare varie parti dell'ecosistema di Facebook per beneficiare la vostra app, come fornire l'accesso all'app tramite il login di Facebook, accettare pagamenti in-app, avviare campagne pubblicitarie mirate, ecc.
- Le [API di Telegram](https://core.telegram.org/api), che vi permettono di incorporare contenuti dai canali Telegram sul vostro sito web, oltre a fornire supporto per i bot.
- L'[API di YouTube](https://developers.google.com/youtube/), che vi permette di incorporare video YouTube sul vostro sito, cercare su YouTube, costruire playlist e altro ancora.
- L'[API di Pinterest](https://developers.pinterest.com/), che fornisce strumenti per gestire bacheche e pin di Pinterest da includere nel vostro sito web.
- L'[API di Twilio](https://www.twilio.com/docs), che fornisce un framework per costruire funzionalità di chiamata voce e video nella vostra app, inviare SMS/MMS dalle vostre app e altro ancora.
- L'[API di Disqus](https://disqus.com/api/docs/), che fornisce una piattaforma di commenti che può essere integrata nel vostro sito.
- L'[API di Mastodon](https://docs.joinmastodon.org/api/), che vi permette di manipolare programmativamente funzionalità del social network Mastodon.
- L'[API IFTTT](https://ifttt.com/developers), che permette di integrare più API attraverso una piattaforma.

## Come funzionano le API?

Le diverse API JavaScript funzionano in modi leggermente diversi, ma generalmente hanno caratteristiche comuni e temi simili nel loro funzionamento.

### Sono basate su oggetti

Il vostro codice interagisce con le API usando uno o più [oggetti JavaScript](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects), che servono come contenitori per i dati che l'API utilizza (contenuti nelle proprietà degli oggetti) e per la funzionalità che l'API rende disponibile (contenuta nei metodi degli oggetti).

> [!NOTE]
> Se non siete già familiari con il funzionamento degli oggetti, dovreste tornare indietro e lavorare attraverso il nostro modulo sugli [oggetti JavaScript](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects) prima di continuare.

Ritorniamo all'esempio della Web Audio API — questa è un'API abbastanza complessa, che consiste in un numero di oggetti. I più ovvi sono:

- [`AudioContext`](/it/docs/Web/API/AudioContext), che rappresenta un [grafo audio](/it/docs/Web/API/Web_Audio_API/Basic_concepts_behind_Web_Audio_API#audio_graphs) che può essere usato per manipolare l'audio che suona all'interno del browser, e ha un numero di metodi e proprietà disponibili per manipolare quell'audio.
- [`MediaElementAudioSourceNode`](/it/docs/Web/API/MediaElementAudioSourceNode), che rappresenta un elemento {{htmlelement("audio")}} contenente suono che volete riprodurre e manipolare all'interno del contesto audio.
- [`AudioDestinationNode`](/it/docs/Web/API/AudioDestinationNode), che rappresenta la destinazione dell'audio, cioè il dispositivo sul vostro computer che lo riprodurrà effettivamente — di solito le vostre casse o cuffie.

Quindi, come interagiscono questi oggetti? Se guardate il nostro [esempio semplice di audio web](https://github.com/mdn/learning-area/blob/main/javascript/apis/introduction/web-audio/index.html) ([vedi anche in vivo](https://mdn.github.io/learning-area/javascript/apis/introduction/web-audio/)), vedrete prima il seguente HTML:

```html
<audio src="outfoxing.mp3"></audio>

<button class="paused">Play</button>
<br />
<input type="range" min="0" max="1" step="0.01" value="1" class="volume" />
```

Per prima cosa, includiamo un elemento `<audio>` con il quale incorporiamo un MP3 nella pagina. Non includiamo controlli del browser predefiniti. Successivamente, includiamo un {{htmlelement("button")}} che useremo per riprodurre e fermare la musica, e un elemento {{htmlelement("input")}} di tipo range, che useremo per regolare il volume della traccia mentre suona.

Successivamente, diamo un'occhiata al JavaScript per questo esempio.

Iniziamo creando un'istanza di `AudioContext` all'interno della quale manipolare la nostra traccia:

```js
const audioCtx = new AudioContext();
```

Successivamente creiamo costanti che memorizzano i riferimenti ai nostri elementi `<audio>`, `<button>` e `<input>`, e usiamo il metodo [`AudioContext.createMediaElementSource()`](/it/docs/Web/API/AudioContext/createMediaElementSource) per creare un `MediaElementAudioSourceNode` che rappresenta la fonte del nostro audio — l'elemento `<audio>` da cui si riprodurrà:

```js
const audioElement = document.querySelector("audio");
const playBtn = document.querySelector("button");
const volumeSlider = document.querySelector(".volume");

const audioSource = audioCtx.createMediaElementSource(audioElement);
```

Successivamente includiamo un paio di gestori di eventi che servono per alternare tra play e pausa quando si preme il pulsante e reimpostare la visualizzazione all'inizio quando la canzone ha finito di suonare:

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
> Alcuni di voi potrebbero notare che i metodi `play()` e `pause()` usati per riprodurre e mettere in pausa la traccia non fanno parte della Web Audio API; fanno parte dell'API [`HTMLMediaElement`](/it/docs/Web/API/HTMLMediaElement), che è diversa ma strettamente correlata.

Poi, creiamo un oggetto [`GainNode`](/it/docs/Web/API/GainNode) usando il metodo [`AudioContext.createGain()`](/it/docs/Web/API/BaseAudioContext/createGain), che può essere usato per regolare il volume dell'audio che passa attraverso di esso, e creiamo un altro gestore di eventi che cambia il valore del guadagno (volume) del grafo audio ogni volta che il valore dello slider cambia:

```js
// volume
const gainNode = audioCtx.createGain();

volumeSlider.addEventListener("input", () => {
  gainNode.gain.value = volumeSlider.value;
});
```

L'ultima cosa da fare per far funzionare tutto è connettere i diversi nodi nel grafo audio, cosa che viene fatta usando il metodo [`AudioNode.connect()`](/it/docs/Web/API/AudioNode/connect) disponibile su ogni tipo di nodo:

```js
audioSource.connect(gainNode).connect(audioCtx.destination);
```

L'audio inizia nella sorgente, viene poi collegato al gain node in modo che il volume dell'audio possa essere regolato. Il gain node è poi collegato al nodo di destinazione in modo che il suono possa essere riprodotto sul vostro computer (la proprietà [`AudioContext.destination`](/it/docs/Web/API/BaseAudioContext/destination) rappresenta qualunque sia il nodo di destinazione audio predefinito disponibile sull'hardware del vostro computer, ad esempio, le vostre casse).

### Hanno punti di ingresso riconoscibili

Quando si utilizza un'API, è importante sapere dove si trova il punto di ingresso dell'API. Nella Web Audio API, questo è piuttosto semplice — è l'oggetto [`AudioContext`](/it/docs/Web/API/AudioContext), che deve essere utilizzato per qualsiasi manipolazione audio.

L'API Document Object Model (DOM) ha anche un punto di ingresso semplice — le sue funzionalità tendono a trovarsi legate all'oggetto [`Document`](/it/docs/Web/API/Document), o a un'istanza di un elemento HTML che volete influenzare in qualche modo, ad esempio:

```js
const em = document.createElement("em"); // create a new em element
const para = document.querySelector("p"); // reference an existing p element
em.textContent = "Hello there!"; // give em some text content
para.appendChild(em); // embed em inside para
```

L'[API Canvas](/it/docs/Web/API/Canvas_API) si basa anche sull'ottenimento di un oggetto contesto da usare per manipolare le cose, anche se in questo caso si tratta di un contesto grafico piuttosto che un contesto audio. Il suo oggetto contesto è creato ottenendo un riferimento all'elemento {{htmlelement("canvas")}} su cui volete disegnare, e poi chiamando il suo metodo [`HTMLCanvasElement.getContext()`](/it/docs/Web/API/HTMLCanvasElement/getContext):

```js
const canvas = document.querySelector("canvas");
const ctx = canvas.getContext("2d");
```

Qualsiasi cosa vogliate fare sulla canvas viene poi realizzata chiamando le proprietà e i metodi dell'oggetto contesto (che è un'istanza di [`CanvasRenderingContext2D`](/it/docs/Web/API/CanvasRenderingContext2D)), ad esempio:

```js
Ball.prototype.draw = function () {
  ctx.beginPath();
  ctx.fillStyle = this.color;
  ctx.arc(this.x, this.y, this.size, 0, 2 * Math.PI);
  ctx.fill();
};
```

> [!NOTE]
> Potete vedere questo codice in azione nella nostra [demo delle palline rimbalzanti](https://github.com/mdn/learning-area/blob/main/javascript/apis/introduction/bouncing-balls.html) (vedete anche [in esecuzione live](https://mdn.github.io/learning-area/javascript/apis/introduction/bouncing-balls.html)).

### Utilizzano spesso eventi per gestire i cambiamenti di stato

Abbiamo già discusso di eventi all'inizio del corso nel nostro articolo [Introduzione agli eventi](/it/docs/Learn_web_development/Core/Scripting/Events), che esamina in dettaglio cosa sono gli eventi web lato client e come vengono usati nel vostro codice. Se non siete già familiari con il funzionamento degli eventi delle API web lato client, dovreste leggere questo articolo prima di continuare.

Alcune API web non contengono eventi, ma la maggior parte ne contiene almeno qualcuno. Le proprietà gestore che ci permettono di eseguire funzioni quando gli eventi si attivano sono generalmente elencate nel nostro materiale di riferimento in sezioni separate "Gestori di eventi".

Abbiamo già visto un numero di gestori di eventi in uso nel nostro esempio di Web Audio API sopra:

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

Le funzionalità WebAPI sono soggette alle stesse considerazioni di sicurezza di JavaScript e di altre tecnologie web (ad esempio la [same-origin policy](/it/docs/Web/Security/Same-origin_policy)), ma a volte hanno meccanismi di sicurezza aggiuntivi in atto. Ad esempio, alcune delle funzionalità più recenti delle WebAPI funzionano solo su pagine servite tramite HTTPS a causa della trasmissione di dati potenzialmente sensibili (esempi includono i [Service Workers](/it/docs/Web/API/Service_Worker_API) e [Push](/it/docs/Web/API/Push_API)).

Inoltre, alcune WebAPI richiedono il permesso di essere abilitate dall'utente una volta effettuate le chiamate ad esse nel vostro codice. Ad esempio, l'API [Notifications](/it/docs/Web/API/Notifications_API) chiede il permesso usando una finestra di dialogo a comparsa:

![Uno screenshot del pop-up di notifica fornito dall'API di Notifiche del browser. Il sito 'mdn.github.io' sta chiedendo permessi per inviare notifiche all'agente utente con una X per chiudere la finestra di dialogo e un menu a tendina di opzioni con 'ricevi sempre notifiche' selezionata di default.](notification-permission.png)

Le API Web Audio e [`HTMLMediaElement`](/it/docs/Web/API/HTMLMediaElement) sono soggette a un meccanismo di sicurezza chiamato [autoplay policy](/it/docs/Web/API/Web_Audio_API/Best_practices#autoplay_policy) — questo fondamentalmente significa che non è possibile riprodurre audio automaticamente quando una pagina viene caricata — è necessario consentire agli utenti di iniziare la riproduzione audio tramite un controllo come un pulsante. Questo viene fatto perché l'audio in riproduzione automatica è di solito davvero fastidioso, e non dovremmo veramente sottoporre i nostri utenti a esso.

> [!NOTE]
> A seconda di quanto sia stretto il browser, tali meccanismi di sicurezza potrebbero persino impedire il funzionamento dell'esempio localmente, cioè, se si carica il file di esempio locale nel browser invece di eseguirlo da un server web. Al momento della scrittura, il nostro esempio di Web Audio API non funzionava localmente su Google Chrome — abbiamo dovuto caricarlo su GitHub prima che funzionasse.

## Sommario

A questo punto, dovreste avere una buona idea di cosa sono le API, come funzionano e cosa potete fare con esse nel vostro codice JavaScript. Probabilmente siete entusiasti di iniziare a fare davvero alcune cose divertenti con API specifiche, quindi procediamo! Il prossimo passo, esamineremo le API video e audio.

{{NextMenu("Learn_web_development/Extensions/Client-side_APIs/Video_and_audio_APIs", "Learn_web_development/Extensions/Client-side_APIs")}}
