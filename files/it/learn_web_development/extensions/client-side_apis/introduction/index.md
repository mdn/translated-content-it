---
title: Introduzione alle API web
short-title: Introduction
slug: Learn_web_development/Extensions/Client-side_APIs/Introduction
l10n:
  sourceCommit: f4c14731a1a157fc8d8f7357ac4d74d14a7d7fb5
---

{{NextMenu("Learn_web_development/Extensions/Client-side_APIs/Video_and_audio_APIs", "Learn_web_development/Extensions/Client-side_APIs")}}

Per iniziare, esamineremo le API a livello generale: cosa sono, come funzionano, come usarle nel codice e come sono strutturate? Vedremo inoltre quali sono le diverse categorie principali di API e quali tipi di utilizzo consentono.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, in particolare con le <a href="/it/docs/Learn_web_development/Core/Scripting/Object_basics">basi degli oggetti JavaScript</a> e con le API fondamentali, come lo <a href="/it/docs/Learn_web_development/Core/Scripting/DOM_scripting">scripting DOM</a> e le <a href="/it/docs/Learn_web_development/Core/Scripting/Network_requests">richieste di rete</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Cosa sono le Web API e cosa è possibile fare con esse.</li>
          <li>Come vengono usate le API.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Cosa sono le API?

Le Application Programming Interfaces (API) sono costrutti messi a disposizione nei linguaggi di programmazione per consentire agli sviluppatori di creare più facilmente funzionalità complesse. Astraggono il codice più complesso, fornendo al suo posto una sintassi più semplice da usare.

Come esempio del mondo reale, si pensi alla fornitura di elettricità in una casa, un appartamento o un'altra abitazione. Per utilizzare un elettrodomestico, lo si collega a una presa elettrica e funziona. Non si cerca di cablarlo direttamente all'alimentazione elettrica: farlo sarebbe molto inefficiente e, senza essere elettricisti, difficile e pericoloso.

![Due ciabatte elettriche sono collegate a due diverse prese di corrente. Ogni ciabatta ha uno slot per spina nella parte superiore e sul lato anteriore. Due spine sono collegate a ciascuna ciabatta.](plug-socket.png)

_Fonte dell'immagine: [Presa elettrica sovraccarica](https://www.flickr.com/photos/easy-pics/9518184890/in/photostream/lightbox/) di [The Clear Communication People](https://www.flickr.com/photos/easy-pics/), su Flickr._

Allo stesso modo, per esempio, per programmare della grafica 3D è molto più semplice utilizzare un'API scritta in un linguaggio di alto livello come JavaScript o Python, anziché tentare di scrivere direttamente codice di basso livello (come C o C++) che controlli direttamente la GPU del computer o altre funzioni grafiche.

> [!NOTE]
> Vedere anche la {{Glossary("API", "voce del glossario relativa alle API")}} per ulteriori informazioni.

### API in JavaScript lato client

JavaScript lato client, in particolare, dispone di molte API: queste non fanno parte del linguaggio JavaScript stesso, ma sono costruite sopra il linguaggio JavaScript di base, fornendo superpoteri aggiuntivi da utilizzare nel codice JavaScript. In genere rientrano in due categorie:

- Le **API del browser** sono integrate nel browser web e possono esporre dati provenienti dal browser e dall'ambiente del computer circostante, oltre a eseguire operazioni complesse utili con essi. Per esempio, la [Web Audio API](/it/docs/Web/API/Web_Audio_API) fornisce costrutti JavaScript per manipolare l'audio nel browser: acquisire una traccia audio, modificarne il volume, applicarvi effetti e così via. In background, il browser usa effettivamente codice di basso livello complesso (ad esempio C++ o Rust) per eseguire l'elaborazione audio. Questa complessità viene però nascosta dall'API.
- Le **API di terze parti** non sono integrate nel browser per impostazione predefinita e in genere è necessario recuperare il loro codice e le loro informazioni da qualche parte sul Web. Per esempio, la [Google Maps API](https://developers.google.com/maps/documentation/javascript) consente di visualizzare sul sito web una mappa interattiva che mostra il proprio ufficio. Fornisce un insieme speciale di costrutti che permettono di interrogare il servizio Google Maps e restituire informazioni specifiche.

![Una schermata del browser con aperta la pagina iniziale di Firefox. Alcune API sono integrate nel browser per impostazione predefinita. Le API di terze parti non sono integrate nel browser per impostazione predefinita. Per utilizzarle, il loro codice e le loro informazioni devono essere recuperati da qualche parte sul Web.](browser.png)

### Relazione tra JavaScript, API e altri strumenti JavaScript

Abbiamo quindi parlato di cosa sono le API JavaScript lato client e di come si relazionano al linguaggio JavaScript. Riepiloghiamo per renderlo più chiaro e indichiamo anche dove si collocano gli altri strumenti JavaScript:

- JavaScript: un linguaggio di scripting di alto livello integrato nei browser che consente di implementare funzionalità nelle pagine web e nelle app. JavaScript è disponibile anche in altri ambienti di programmazione, come [Node](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction).
- API del browser: costrutti integrati nel browser che si basano sul linguaggio JavaScript e consentono di implementare più facilmente funzionalità.
- API di terze parti: costrutti integrati in piattaforme di terze parti (ad esempio Disqus, Facebook) che consentono di utilizzare alcune funzionalità di tali piattaforme nelle proprie pagine web, per esempio visualizzare i commenti Disqus in una pagina web.
- Librerie JavaScript: in genere uno o più file JavaScript contenenti [funzioni personalizzate](/it/docs/Learn_web_development/Core/Scripting/Functions) che possono essere associati alla pagina web per accelerare o rendere possibile la scrittura di funzionalità comuni. Alcuni esempi sono jQuery, Mootools e React.
- Framework JavaScript: un livello superiore rispetto alle librerie; i framework JavaScript (ad esempio Angular ed Ember) tendono a essere pacchetti di HTML, CSS, JavaScript e altre tecnologie che vengono installati e poi utilizzati per scrivere un'intera applicazione web da zero. La differenza principale tra una libreria e un framework è l'"inversione del controllo". Quando viene chiamato un metodo di una libreria, lo sviluppatore mantiene il controllo. Con un framework, il controllo è invertito: il framework chiama il codice dello sviluppatore.

## Cosa possono fare le API?

Nei browser moderni è disponibile un enorme numero di API che consentono di fare una grande varietà di operazioni nel codice. È possibile verificarlo consultando la [pagina dell'indice delle API di MDN](/it/docs/Web/API).

### API comuni del browser

In particolare, le categorie più comuni di API del browser che verranno utilizzate, e che saranno trattate più dettagliatamente in questo modulo, sono:

- **API per manipolare i documenti** caricati nel browser. L'esempio più evidente è la [DOM (Document Object Model) API](/it/docs/Web/API/Document_Object_Model), che consente di manipolare HTML e CSS: creare, rimuovere e modificare HTML, applicare dinamicamente nuovi stili alla pagina e così via. Ogni volta che appare una finestra popup in una pagina o viene visualizzato del nuovo contenuto, per esempio, è il DOM in azione. Per saperne di più su questi tipi di API, consultare l'[introduzione allo scripting DOM](/it/docs/Learn_web_development/Core/Scripting/DOM_scripting).
- **API che recuperano dati dal server** per aggiornare autonomamente piccole sezioni di una pagina web sono usate molto comunemente. Questo dettaglio apparentemente piccolo ha avuto un enorme impatto sulle prestazioni e sul comportamento dei siti: se è necessario aggiornare soltanto una quotazione azionaria o un elenco di nuove notizie disponibili, farlo istantaneamente senza dover ricaricare l'intera pagina dal server può rendere il sito o l'app molto più reattivo e scattante. L'API principale usata a questo scopo è la [Fetch API](/it/docs/Web/API/Fetch_API), anche se il codice meno recente potrebbe ancora usare l'API [`XMLHttpRequest`](/it/docs/Web/API/XMLHttpRequest). Si potrebbe inoltre incontrare il termine **AJAX**, che descrive questa tecnica. Per saperne di più su tali API, consultare [Effettuare richieste di rete con JavaScript](/it/docs/Learn_web_development/Core/Scripting/Network_requests).
- **API per disegnare e manipolare la grafica** sono ampiamente supportate nei browser; le più popolari sono [Canvas](/it/docs/Web/API/Canvas_API) e [WebGL](/it/docs/Web/API/WebGL_API), che consentono di aggiornare programmaticamente i dati dei pixel contenuti in un elemento HTML {{htmlelement("canvas")}} per creare scene 2D e 3D. Per esempio, con la Canvas API è possibile disegnare forme come rettangoli o cerchi, importare un'immagine sul canvas e applicarvi un filtro come seppia o scala di grigi, oppure creare una complessa scena 3D con illuminazione e texture usando WebGL. Tali API sono spesso combinate con API per creare cicli di animazione, come [`window.requestAnimationFrame()`](/it/docs/Web/API/Window/requestAnimationFrame), e altre, per realizzare scene in continuo aggiornamento come cartoni animati e giochi.
- Le **[API audio e video](/it/docs/Web/Media/Guides/Audio_and_video_delivery)**, come [`HTMLMediaElement`](/it/docs/Web/API/HTMLMediaElement), la [Web Audio API](/it/docs/Web/API/Web_Audio_API) e [WebRTC](/it/docs/Web/API/WebRTC_API), consentono di fare operazioni molto interessanti con contenuti multimediali, come creare controlli UI personalizzati per riprodurre audio e video, visualizzare tracce di testo quali didascalie e sottotitoli insieme ai video, acquisire video dalla webcam per manipolarlo tramite un canvas (vedere sopra) o visualizzarlo sul computer di un'altra persona durante una conferenza web, oppure aggiungere effetti alle tracce audio, come guadagno, distorsione, panoramica e così via.
- Le **API dei dispositivi** consentono di interagire con l'hardware del dispositivo: per esempio, accedere al GPS del dispositivo per individuare la posizione dell'utente mediante la [Geolocation API](/it/docs/Web/API/Geolocation_API).
- Le **API di archiviazione lato client** consentono di memorizzare dati lato client, in modo da poter creare un'app che salvi il proprio stato tra i caricamenti di pagina e che possa persino funzionare quando il dispositivo è offline. Sono disponibili diverse opzioni, ad esempio un semplice archivio nome/valore con la [Web Storage API](/it/docs/Web/API/Web_Storage_API) e un archivio database più complesso con la [IndexedDB API](/it/docs/Web/API/IndexedDB_API).

### API di terze parti comuni

Le API di terze parti sono molto varie; alcune tra le più popolari che probabilmente verranno utilizzate prima o poi sono:

- API per mappe, come [Mapquest](https://developer.mapquest.com/) e la [Google Maps API](https://developers.google.com/maps/), che consentono di fare ogni tipo di operazione con le mappe nelle pagine web.
- La [suite di API Facebook](https://developers.facebook.com/docs/), che consente di usare varie parti dell'ecosistema Facebook a vantaggio dell'app, per esempio fornendo l'accesso all'app tramite Facebook login, accettando pagamenti in-app, avviando campagne pubblicitarie mirate e così via.
- Le [API Telegram](https://core.telegram.org/api), che consentono di incorporare contenuti dai canali Telegram nel sito web, oltre a fornire supporto per i bot.
- La [YouTube API](https://developers.google.com/youtube/), che consente di incorporare video YouTube nel sito, effettuare ricerche su YouTube, creare playlist e altro ancora.
- La [Pinterest API](https://developers.pinterest.com/), che fornisce strumenti per gestire bacheche e pin Pinterest e includerli nel sito web.
- La [Twilio API](https://www.twilio.com/docs), che fornisce un framework per creare funzionalità di chiamate vocali e video nell'app, inviare SMS/MMS dalle app e altro ancora.
- La [Disqus API](https://disqus.com/api/docs/), che fornisce una piattaforma di commenti integrabile nel sito.
- La [Mastodon API](https://docs.joinmastodon.org/api/), che consente di manipolare programmaticamente le funzionalità del social network Mastodon.
- La [IFTTT API](https://ifttt.com/developers), che consente di integrare più API tramite un'unica piattaforma.

## Come funzionano le API?

Le diverse API JavaScript funzionano in modi leggermente diversi, ma in generale condividono caratteristiche comuni e modalità di funzionamento simili.

### Sono basate su oggetti

Il codice interagisce con le API usando uno o più [oggetti JavaScript](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects), che fungono da contenitori per i dati usati dall'API, contenuti nelle proprietà degli oggetti, e per le funzionalità messe a disposizione dall'API, contenute nei metodi degli oggetti.

> [!NOTE]
> Se non si ha già familiarità con il funzionamento degli oggetti, è necessario tornare al modulo sugli [oggetti JavaScript](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects) prima di proseguire.

Torniamo all'esempio della Web Audio API: si tratta di un'API abbastanza complessa, composta da diversi oggetti. Quelli più evidenti sono:

- [`AudioContext`](/it/docs/Web/API/AudioContext), che rappresenta un [grafo audio](/it/docs/Web/API/Web_Audio_API/Basic_concepts_behind_Web_Audio_API#audio_graphs) utilizzabile per manipolare l'audio riprodotto nel browser e dispone di vari metodi e proprietà per manipolare quell'audio.
- [`MediaElementAudioSourceNode`](/it/docs/Web/API/MediaElementAudioSourceNode), che rappresenta un elemento {{htmlelement("audio")}} contenente il suono da riprodurre e manipolare nel contesto audio.
- [`AudioDestinationNode`](/it/docs/Web/API/AudioDestinationNode), che rappresenta la destinazione dell'audio, cioè il dispositivo del computer che lo riprodurrà effettivamente, in genere altoparlanti o cuffie.

Come interagiscono questi oggetti? Osservando il nostro [semplice esempio di web audio](https://github.com/mdn/learning-area/blob/main/javascript/apis/introduction/web-audio/index.html) ([visualizzabile anche in esecuzione](https://mdn.github.io/learning-area/javascript/apis/introduction/web-audio/)), si vedrà anzitutto il seguente HTML:

```html
<audio src="outfoxing.mp3"></audio>

<button class="paused">Play</button>
<br />
<input type="range" min="0" max="1" step="0.01" value="1" class="volume" />
```

Innanzitutto, viene incluso un elemento `<audio>` con cui incorporare un MP3 nella pagina. Non vengono inclusi controlli predefiniti del browser. Successivamente, viene incluso un elemento {{htmlelement("button")}} da utilizzare per riprodurre e interrompere la musica, e un elemento {{htmlelement("input")}} di tipo range, da utilizzare per regolare il volume della traccia durante la riproduzione.

Vediamo ora il JavaScript di questo esempio.

Si inizia creando un'istanza `AudioContext` al cui interno manipolare la traccia:

```js
const audioCtx = new AudioContext();
```

Successivamente, vengono create costanti che memorizzano riferimenti agli elementi `<audio>`, `<button>` e `<input>`, e viene usato il metodo [`AudioContext.createMediaElementSource()`](/it/docs/Web/API/AudioContext/createMediaElementSource) per creare un `MediaElementAudioSourceNode` che rappresenta la sorgente dell'audio, l'elemento `<audio>` da cui verrà riprodotto:

```js
const audioElement = document.querySelector("audio");
const playBtn = document.querySelector("button");
const volumeSlider = document.querySelector(".volume");

const audioSource = audioCtx.createMediaElementSource(audioElement);
```

Successivamente, vengono inclusi un paio di gestori di eventi che consentono di alternare riproduzione e pausa quando viene premuto il pulsante e di riportare la visualizzazione all'inizio quando la riproduzione del brano è terminata:

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
> Alcuni potrebbero notare che i metodi `play()` e `pause()` usati per riprodurre e mettere in pausa la traccia non fanno parte della Web Audio API; fanno parte dell'API [`HTMLMediaElement`](/it/docs/Web/API/HTMLMediaElement), diversa ma strettamente correlata.

Successivamente, viene creato un oggetto [`GainNode`](/it/docs/Web/API/GainNode) usando il metodo [`AudioContext.createGain()`](/it/docs/Web/API/BaseAudioContext/createGain), che può essere usato per regolare il volume dell'audio che lo attraversa, e viene creato un altro gestore di eventi che modifica il valore di gain, ovvero il volume, del grafo audio ogni volta che cambia il valore dello slider:

```js
// volume
const gainNode = audioCtx.createGain();

volumeSlider.addEventListener("input", () => {
  gainNode.gain.value = volumeSlider.value;
});
```

L'ultima operazione necessaria affinché tutto funzioni è collegare i diversi nodi nel grafo audio, usando il metodo [`AudioNode.connect()`](/it/docs/Web/API/AudioNode/connect) disponibile per ogni tipo di nodo:

```js
audioSource.connect(gainNode).connect(audioCtx.destination);
```

L'audio inizia nella sorgente, che viene poi collegata al nodo di gain affinché sia possibile regolarne il volume. Il nodo di gain viene quindi collegato al nodo di destinazione affinché il suono possa essere riprodotto sul computer. La proprietà [`AudioContext.destination`](/it/docs/Web/API/BaseAudioContext/destination) rappresenta qualunque [`AudioDestinationNode`](/it/docs/Web/API/AudioDestinationNode) predefinito sia disponibile nell'hardware del computer, per esempio gli altoparlanti.

### Hanno punti di ingresso riconoscibili

Quando si usa un'API, è necessario assicurarsi di conoscere il punto di ingresso dell'API. Nella Web Audio API questo è piuttosto semplice: è l'oggetto [`AudioContext`](/it/docs/Web/API/AudioContext), che deve essere usato per eseguire qualsiasi manipolazione audio.

Anche l'API Document Object Model (DOM) ha un punto di ingresso semplice: le sue funzionalità sono generalmente disponibili dall'oggetto [`Document`](/it/docs/Web/API/Document) o da un'istanza di un elemento HTML che si desidera modificare in qualche modo, per esempio:

```js
const em = document.createElement("em"); // create a new em element
const para = document.querySelector("p"); // reference an existing p element
em.textContent = "Hello there!"; // give em some text content
para.appendChild(em); // embed em inside para
```

Anche la [Canvas API](/it/docs/Web/API/Canvas_API) richiede di ottenere un oggetto contesto per manipolare gli elementi, sebbene in questo caso si tratti di un contesto grafico anziché di un contesto audio. Il suo oggetto contesto viene creato ottenendo un riferimento all'elemento {{htmlelement("canvas")}} su cui si desidera disegnare, quindi chiamando il relativo metodo [`HTMLCanvasElement.getContext()`](/it/docs/Web/API/HTMLCanvasElement/getContext):

```js
const canvas = document.querySelector("canvas");
const ctx = canvas.getContext("2d");
```

Qualsiasi operazione da eseguire sul canvas viene quindi realizzata chiamando proprietà e metodi dell'oggetto contesto, che è un'istanza di [`CanvasRenderingContext2D`](/it/docs/Web/API/CanvasRenderingContext2D), per esempio:

```js
Ball.prototype.draw = function () {
  ctx.beginPath();
  ctx.fillStyle = this.color;
  ctx.arc(this.x, this.y, this.size, 0, 2 * Math.PI);
  ctx.fill();
};
```

> [!NOTE]
> È possibile vedere questo codice in azione nella nostra [demo delle palline rimbalzanti](https://github.com/mdn/learning-area/blob/main/javascript/apis/introduction/bouncing-balls.html) e [in esecuzione](https://mdn.github.io/learning-area/javascript/apis/introduction/bouncing-balls.html).

### Spesso usano eventi per gestire i cambiamenti di stato

Gli eventi sono già stati trattati in precedenza nel corso, nell'articolo [Introduzione agli eventi](/it/docs/Learn_web_development/Core/Scripting/Events), che esamina in dettaglio cosa sono gli eventi web lato client e come vengono usati nel codice. Se non si ha già familiarità con il funzionamento degli eventi delle API web lato client, leggere prima questo articolo.

Alcune API web non contengono eventi, ma la maggior parte ne contiene almeno alcuni. Le proprietà dei gestori che consentono di eseguire funzioni quando si attivano gli eventi sono generalmente elencate nel materiale di riferimento in sezioni separate "Event handlers".

Nell'esempio della Web Audio API precedente sono già stati visti diversi gestori di eventi:

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

### Dispongono di meccanismi di sicurezza aggiuntivi, quando appropriato

Le funzionalità delle Web API sono soggette alle stesse considerazioni di sicurezza di JavaScript e delle altre tecnologie web, per esempio la [same-origin policy](/it/docs/Web/Security/Defenses/Same-origin_policy), ma talvolta sono dotate di meccanismi di sicurezza aggiuntivi. Per esempio, alcune delle Web API più moderne funzionano soltanto su pagine servite tramite HTTPS, poiché trasmettono dati potenzialmente sensibili. Alcuni esempi includono [Service Workers](/it/docs/Web/API/Service_Worker_API) e [Push](/it/docs/Web/API/Push_API).

Inoltre, alcune Web API richiedono all'utente l'autorizzazione per essere abilitate quando vengono effettuate chiamate a esse nel codice. Per esempio, la [Notifications API](/it/docs/Web/API/Notifications_API) richiede l'autorizzazione usando una finestra di dialogo popup:

![Una schermata della finestra di dialogo popup per le notifiche fornita dalla Notifications API del browser. Il sito web 'mdn.github.io' richiede l'autorizzazione per inviare notifiche allo user agent, con una X per chiudere la finestra di dialogo e un menu a discesa di opzioni in cui 'ricevi sempre notifiche' è selezionato per impostazione predefinita.](notification-permission.png)

Le API Web Audio e [`HTMLMediaElement`](/it/docs/Web/API/HTMLMediaElement) sono soggette a un meccanismo di sicurezza chiamato [autoplay policy](/it/docs/Web/API/Web_Audio_API/Best_practices#autoplay_policy): in sostanza, significa che non è possibile riprodurre automaticamente l'audio al caricamento di una pagina; occorre consentire agli utenti di avviare la riproduzione audio tramite un controllo come un pulsante. Questo avviene perché l'audio riprodotto automaticamente è solitamente molto fastidioso e non dovrebbe essere imposto agli utenti.

> [!NOTE]
> A seconda di quanto sia restrittivo il browser, tali meccanismi di sicurezza potrebbero persino impedire il funzionamento dell'esempio in locale, cioè se il file di esempio locale viene caricato nel browser anziché eseguito da un server web. Al momento della stesura, il nostro esempio della Web Audio API non funzionava localmente in Google Chrome: è stato necessario caricarlo su GitHub affinché funzionasse.

## Riepilogo

A questo punto dovrebbe essere chiaro cosa sono le API, come funzionano e cosa è possibile fare con esse nel codice JavaScript. Probabilmente è arrivato il momento di iniziare a fare operazioni interessanti con API specifiche: procediamo. Successivamente, verranno esaminate le API video e audio.

{{NextMenu("Learn_web_development/Extensions/Client-side_APIs/Video_and_audio_APIs", "Learn_web_development/Extensions/Client-side_APIs")}}
