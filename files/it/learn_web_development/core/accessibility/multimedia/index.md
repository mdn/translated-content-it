---
title: Multimedia accessibile
slug: Learn_web_development/Core/Accessibility/Multimedia
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/WAI-ARIA_basics","Learn_web_development/Core/Accessibility/Mobile", "Learn_web_development/Core/Accessibility")}}

Un'altra categoria di contenuti che può creare problemi di accessibilità è il multimedia. Video, audio e contenuto visivo necessitano di alternative testuali appropriate affinché possano essere compresi dalle tecnologie assistive e dai loro utenti. Questo articolo mostra come fare.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Familiarità con <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e le migliori pratiche di accessibilità insegnate nelle lezioni precedenti del modulo.</td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>I problemi con i lettori multimediali nativi e come creare quelli personalizzati.</li>
          <li>Lo scopo delle trascrizioni audio e delle tracce di testo (sottotitoli, didascalie, ecc.) nel rendere accessibili i contenuti audio e video.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Multimedia e accessibilità

Finora in questo modulo abbiamo esaminato una varietà di contenuti e cosa deve essere fatto per garantirne l'accessibilità, dalla semplice testualità a tabelle di dati, immagini, controlli nativi come elementi di form e pulsanti, e persino strutture di markup più complesse (con attributi [WAI-ARIA](/it/docs/Learn_web_development/Core/Accessibility/WAI-ARIA_basics)).

Questo articolo, invece, si occupa di un'altra classe generale di contenuti che, a quanto pare, non è altrettanto facile da rendere accessibile — il multimedia. Immagini, tracce audio, video, elementi {{htmlelement("canvas")}}, ecc., non sono facilmente comprensibili dai lettori di schermo o navigabili tramite tastiera, e dobbiamo aiutarli.

Ma non disperare — qui ti aiuteremo a navigare attraverso le tecniche disponibili per rendere il multimedia più accessibile.

## Immagini semplici

Abbiamo già trattato le semplici alternative testuali per le immagini HTML nel nostro articolo [HTML: Una buona base per l'accessibilità](/it/docs/Learn_web_development/Core/Accessibility/HTML) — puoi fare riferimento a quell'articolo per i dettagli completi. In breve, dovresti assicurarti che, dove possibile, il contenuto visivo abbia un testo alternativo disponibile per i lettori di schermo per raccoglierlo e leggerlo agli utenti.

Per esempio:

```html
<img
  src="dinosaur.png"
  alt="A red Tyrannosaurus Rex: A two legged dinosaur standing upright like a human, with small arms, and a large head with lots of sharp teeth." />
```

## Controlli audio e video accessibili

Implementare controlli per audio/video basato su web non dovrebbe essere un problema, giusto? Scopriamo.

### Il problema con i controlli HTML nativi

Le istanze video e audio HTML vengono persino con un set di controlli integrati che permettono di controllare il media immediatamente. Ad esempio (vedi il `native-controls.html` nel [codice sorgente](https://github.com/mdn/learning-area/blob/main/accessibility/multimedia/native-controls.html) e [live](https://mdn.github.io/learning-area/accessibility/multimedia/native-controls.html)):

```html
<audio controls>
  <source src="viper.mp3" type="audio/mp3" />
  <source src="viper.ogg" type="audio/ogg" />
  <p>
    Your browser doesn't support HTML audio. Here is a
    <a href="viper.mp3">link to the audio</a> instead.
  </p>
</audio>

<br />

<video controls>
  <source src="rabbit320.mp4" type="video/mp4" />
  <source src="rabbit320.webm" type="video/webm" />
  <p>
    Your browser doesn't support HTML video. Here is a
    <a href="rabbit320.mp4">link to the video</a> instead.
  </p>
</video>
```

L'attributo controls fornisce pulsanti di riproduzione/pausa, una barra di ricerca, ecc. — i controlli di base che ci si aspetterebbe da un lettore multimediale. Appare così in Firefox e Chrome:

![Screenshot dei controlli video in Firefox](native-controls-firefox.png)

![Screenshot dei controlli video in Chrome](native-controls-chrome.png)

Tuttavia, ci sono problemi con questi controlli:

- Non sono accessibili tramite tastiera nella maggior parte dei browser, ovvero non è possibile passare tra i controlli all'interno del lettore nativo. Opera e Chrome lo forniscono in parte, ma non è ancora ideale.
- Diversi browser offrono diversi stili e funzionalità per i controlli nativi, e non possono essere stilizzati, il che significa che non possono essere facilmente adattati alla guida di stile di un sito.

Per rimediare a ciò, possiamo creare i nostri controlli personalizzati. Diamo un'occhiata a come fare.

### Creare controlli audio e video personalizzati

Video e audio HTML condividono un'API — HTML Media Element — che consente di mappare la funzionalità personalizzata su pulsanti e altri controlli — entrambi definiti da te.

Prendiamo l'esempio del video sopra e aggiungiamo controlli personalizzati.

#### Configurazione di base

Prima di tutto, scarica una copia dei nostri file [custom-controls-start.html](https://github.com/mdn/learning-area/blob/main/accessibility/multimedia/custom-controls-start.html), [custom-controls.css](https://github.com/mdn/learning-area/blob/main/accessibility/multimedia/custom-controls.css), [rabbit320.mp4](https://raw.githubusercontent.com/mdn/learning-area/master/accessibility/multimedia/rabbit320.mp4) e [rabbit320.webm](https://raw.githubusercontent.com/mdn/learning-area/master/accessibility/multimedia/rabbit320.webm) e salvali in una nuova directory sul tuo hard disk.

Crea un nuovo file chiamato main.js e salvalo nella stessa directory.

Prima di tutto, diamo un'occhiata all'HTML per il lettore video nel file HTML:

```html
<section class="player">
  <video controls>
    <source src="rabbit320.mp4" type="video/mp4" />
    <source src="rabbit320.webm" type="video/webm" />
    <p>
      Your browser doesn't support HTML video. Here is a
      <a href="rabbit320.mp4">link to the video</a> instead.
    </p>
  </video>

  <div class="controls">
    <button class="play-pause">Play</button>
    <button class="stop">Stop</button>
    <button class="rwd">Rwd</button>
    <button class="fwd">Fwd</button>
    <div class="time">00:00</div>
  </div>
</section>
```

#### Configurazione di base in JavaScript

Abbiamo inserito alcuni semplici pulsanti di controllo sotto il nostro video. Questi controlli naturalmente non faranno nulla di default; per aggiungere funzionalità, useremo JavaScript.

Innanzitutto, dobbiamo memorizzare riferimenti a ciascuno dei controlli — aggiungi il seguente codice all'inizio del tuo file JavaScript:

```js
const playPauseBtn = document.querySelector(".play-pause");
const stopBtn = document.querySelector(".stop");
const rwdBtn = document.querySelector(".rwd");
const fwdBtn = document.querySelector(".fwd");
const timeLabel = document.querySelector(".time");
```

Successivamente, dobbiamo ottenere un riferimento al lettore video/audio stesso — aggiungi questa linea sotto le precedenti:

```js
const player = document.querySelector("video");
```

Questo contiene un riferimento a un oggetto [`HTMLMediaElement`](/it/docs/Web/API/HTMLMediaElement), che ha diverse proprietà e metodi utili disponibili che possono essere utilizzati per connettere la funzionalità ai nostri pulsanti.

Prima di passare alla creazione della funzionalità dei pulsanti, rimuoviamo i controlli nativi in modo che non interferiscano con i nostri controlli personalizzati. Aggiungi il seguente codice, sempre in fondo al JavaScript:

```js
player.removeAttribute("controls");
```

Fare in questo modo anziché non includere del tutto l'attributo controls ha il vantaggio che, se il nostro JavaScript fallisce per qualsiasi motivo, l'utente avrà ancora alcuni controlli disponibili.

#### Connettere i nostri pulsanti

Prima di tutto, imposteremo il pulsante di riproduzione/pausa. Possiamo far sì che intervalli tra play e pause con una semplice funzione condizionale, come la seguente. Aggiungila al tuo codice, in fondo:

```js
playPauseBtn.onclick = () => {
  if (player.paused) {
    player.play();
    playPauseBtn.textContent = "Pause";
  } else {
    player.pause();
    playPauseBtn.textContent = "Play";
  }
};
```

Successivamente, aggiungi questo codice in fondo, che controlla il pulsante di stop:

```js
stopBtn.onclick = () => {
  player.pause();
  player.currentTime = 0;
  playPauseBtn.textContent = "Play";
};
```

Non esiste una funzione `stop()` su [`HTMLMediaElement`](/it/docs/Web/API/HTMLMediaElement), quindi invece mettiamo in `pause()` e, allo stesso tempo, impostiamo il `currentTime` a 0.

Successivamente, i nostri pulsanti di riavvolgimento e avanzamento rapido — aggiungi i seguenti blocchi in fondo al tuo codice:

```js
rwdBtn.onclick = () => {
  player.currentTime -= 3;
};

fwdBtn.onclick = () => {
  player.currentTime += 3;
  if (player.currentTime >= player.duration || player.paused) {
    player.pause();
    player.currentTime = 0;
    playPauseBtn.textContent = "Play";
  }
};
```

Questi sono molto semplici, basta aggiungere o sottrarre 3 secondi al `currentTime` ogni volta che vengono cliccati. In un lettore video reale, probabilmente si vorrebbe una barra di ricerca più elaborata, o simili.

Nota che controlliamo anche se il `currentTime` è più del `duration` totale dei media o se i media non sono in riproduzione quando viene premuto il `fwdBtn`. Se una delle condizioni è vera, fermiamo il video per evitare che l'interfaccia utente vada in errore se si tenta di avanzare rapidamente quando il video non è in riproduzione o oltre la fine del video.

Infine, aggiungi il seguente codice alla fine, per controllare la visualizzazione del tempo trascorso:

```js
player.ontimeupdate = () => {
  const minutes = Math.floor(player.currentTime / 60);
  const seconds = Math.floor(player.currentTime - minutes * 60);
  const minuteValue = minutes < 10 ? `0${minutes}` : minutes;
  const secondValue = seconds < 10 ? `0${seconds}` : seconds;

  const mediaTime = `${minuteValue}:${secondValue}`;
  timeLabel.textContent = mediaTime;
};
```

Ogni volta che il tempo si aggiorna (una volta al secondo), attiviamo questa funzione. Calcola il numero di minuti e secondi dal valore `currentTime` fornito (che è in secondi), aggiunge uno 0 iniziale se il valore dei minuti o dei secondi è inferiore a 10, e quindi crea la visualizzazione e la aggiunge all'etichetta temporale.

#### Ulteriori letture

Questo ti dà un'idea di base su come aggiungere funzionalità personalizzate al lettore video/audio. Per ulteriori informazioni su come aggiungere funzionalità più complesse ai lettori video/audio, vedi:

- [Distribuzione di audio e video](/it/docs/Web/Media/Guides/Audio_and_video_delivery)
- [Basi sulla stilizzazione del lettore video](/it/docs/Web/Media/Guides/Audio_and_video_delivery/Video_player_styling_basics)
- [Creare un lettore video cross-browser](/it/docs/Web/Media/Guides/Audio_and_video_delivery/cross_browser_video_player)

Abbiamo anche creato un esempio avanzato per mostrare come si potrebbe creare un sistema orientato agli oggetti che trova ogni lettore video e audio sulla pagina (indipendentemente dal numero) e aggiunge i nostri controlli personalizzati. Vedi [custom-controls-oojs](https://mdn.github.io/learning-area/accessibility/multimedia/custom-controls-OOJS/) (vedi anche il [codice sorgente](https://github.com/mdn/learning-area/tree/main/accessibility/multimedia/custom-controls-OOJS)).

## Trascrizioni audio

Per fornire alle persone non udenti accesso ai contenuti audio, è necessario creare trascrizioni di testo. Queste possono essere incluse sulla stessa pagina dell'audio in qualche modo o incluse su una pagina separata e collegate.

In realtà, per creare la trascrizione, le opzioni sono:

- Servizi commerciali — Si potrebbe pagare un professionista per fare la trascrizione, vedi ad esempio aziende come [Scribie](https://scribie.com/), [Casting Words](https://castingwords.com/) o [Rev](https://www.rev.com/). Guardati intorno e chiedi consigli per assicurarti di trovare un'azienda rispettabile con cui puoi lavorare efficacemente.
- Trascrizione comunitaria/autoprodotta — Se fai parte di una comunità attiva o di un team nel tuo posto di lavoro, potresti chiedere loro aiuto per fare le trascrizioni. Potresti persino provare a farle tu stesso.
- Servizi automatizzati — Esistono servizi di AI disponibili, come [Trint](https://trint.com/) o [Transcribear](https://transcribear.com/). Carica un file video/audio sul sito, e lo trascrive automaticamente per te. Su YouTube, puoi scegliere di generare sottotitoli/trascrizioni automatiche. A seconda di quanto chiaro sia l'audio parlato, la qualità della trascrizione risultante varierà notevolmente.

Come per la maggior parte delle cose della vita, si tende a ottenere ciò per cui si paga; i diversi servizi varieranno in accuratezza e tempo necessario per produrre la trascrizione. Se paghi un'azienda o un servizio di AI rispettabile per fare la trascrizione, probabilmente verrà eseguita rapidamente e di alta qualità. Se non vuoi pagare, è probabile che venga eseguita a una qualità inferiore e/o lentamente.

Non è corretto pubblicare una risorsa audio promettendo di pubblicare la trascrizione più tardi — tali promesse spesso non vengono mantenute, il che danneggerà la fiducia tra te e i tuoi utenti. Se l'audio che presenti è qualcosa come un incontro faccia a faccia o una performance dal vivo, sarebbe accettabile prendere appunti durante la performance, pubblicarli per intero insieme all'audio, poi cercare aiuto per ripulire gli appunti successivamente.

### Esempi di trascrizione

Se utilizzi un servizio automatico, allora probabilmente dovrai utilizzare l'interfaccia utente fornita dallo strumento. Ad esempio, dai un'occhiata al nostro video [Wait, ARIA Roles Have Categories?](https://www.youtube.com/watch?v=mwF-PpJOjMs) e seleziona il menu a tre punti (...), _> Mostra Trascrizione_. Vedrai la trascrizione apparire in un pannello separato.

Se stai creando un'interfaccia utente personalizzata per presentare il tuo audio e la trascrizione associata, puoi farlo come preferisci, ma potrebbe avere senso includerlo in un pannello mostrabile/nascondibile; vedi il nostro esempio [audio-transcript-ui](https://mdn.github.io/learning-area/accessibility/multimedia/audio-transcript-ui/) (vedi anche il [codice sorgente](https://github.com/mdn/learning-area/tree/main/accessibility/multimedia/audio-transcript-ui)).

### Descrizioni audio

In occasioni in cui ci sono elementi visivi che accompagnano il tuo audio, dovrai fornire descrizioni audio di qualche tipo per descrivere quel contenuto extra.

In molti casi, questo prenderà la forma di un video, nel qual caso puoi implementare sottotitoli utilizzando le tecniche descritte nella sezione successiva dell'articolo.

Tuttavia, ci sono alcuni casi limite. Potresti, ad esempio, avere una registrazione audio di un incontro che fa riferimento a una risorsa accompagnante come un foglio di calcolo o un grafico. In tali casi, dovresti assicurarti che le risorse siano fornite insieme all'audio + trascrizione e specificamente collegarle nei punti in cui sono menzionate nella trascrizione. Questo naturalmente aiuterà tutti gli utenti, non solo le persone non udenti.

> [!NOTE]
> Una trascrizione audio aiuterà in generale più gruppi di utenti. Oltre a dare agli utenti non udenti accesso alle informazioni contenute nell'audio, pensa all'utente con una connessione a banda bassa, per il quale sarebbe scomodo scaricare l'audio. Pensa anche a un utente in un ambiente rumoroso come un pub o un bar, che sta cercando di accedere alle informazioni ma non può sentirle per via del rumore.

## Tracce di testo video

Per rendere accessibile un video a persone non udenti, ipovedenti, o altri gruppi di utenti (come quelli con bassa larghezza di banda, o che non comprendono la lingua in cui il video è registrato), è necessario includere tracce di testo insieme al contenuto video.

> [!NOTE]
> Le tracce di testo sono utili anche per qualsiasi utente, non solo per quelli con disabilità. Ad esempio, alcuni utenti potrebbero non essere in grado di ascoltare l'audio perché sono in ambienti rumorosi (come un bar affollato durante una partita sportiva) o potrebbero non voler disturbare gli altri se si trovano in un luogo tranquillo (come una biblioteca).

Questo non è un concetto nuovo — i servizi televisivi hanno avuto la sottotitolazione chiusa disponibile da molto tempo:

![Fotogramma di un cartone animato d'epoca con sottotitoli chiusi "Ben fatto, Goldie. Continua così!"](closed-captions.png)

Molti paesi offrono film in inglese con sottotitoli nella propria lingua madre, e diversi sottotitoli in lingua sono spesso disponibili nei DVD, come mostrato di seguito:

![Un film in inglese con sottotitoli in tedesco "Emo, warum erkennst du nicht die Schonheit dieses Ortes?"](subtitles_german.png)

Esistono diversi tipi di tracce di testo per diverse finalità. I principali che incontrerai sono:

- Didattiche — Presenti per il beneficio degli utenti non udenti che non possono sentire la traccia audio, includendo le parole pronunciate, e informazioni contestuali come chi ha parlato, se le persone erano arrabbiate o tristi, e quale umore la musica sta creando attualmente.
- Sottotitoli — Includono traduzioni del dialogo audio, per utenti che non comprendono la lingua parlata.
- Descrizioni — Includono descrizioni per persone ipovedenti che non possono vedere il video, per esempio, come appare la scena.
- Titoli di capitolo — Indicatori di capitolo destinati ad aiutare l'utente a navigare nella risorsa multimediale.

### Implementare tracce di testo per video HTML

Le tracce di testo per la riproduzione con video HTML devono essere scritte in WebVTT, un formato di testo contenente più stringhe di testo insieme a metadata come il momento nel video in cui si desidera che ciascuna stringa di testo venga visualizzata, e anche informazioni di stile/posizionamento limitate. Queste stringhe di testo sono chiamate cue.

Un tipico file WebVTT sarà simile a questo:

```plain
WEBVTT

1
00:00:22.230 --> 00:00:24.606
This is the first subtitle.

2
00:00:30.739 --> 00:00:34.074
This is the second.

…
```

Per farlo visualizzare insieme alla riproduzione del media HTML, è necessario:

- Salvarlo come file .vtt in un luogo sensato.
- Collegarsi al file .vtt con l'elemento {{htmlelement("track")}}. `<track>` dovrebbe essere posizionato all'interno di `<audio>` o `<video>`, ma dopo tutti gli elementi `<source>`. Usa l'attributo [`kind`](/it/docs/Web/HTML/Reference/Elements/track#kind) per specificare se i cue sono sottotitoli, didascalie o descrizioni. Inoltre, usa [`srclang`](/it/docs/Web/HTML/Reference/Elements/track#srclang) per dire al browser in quale lingua hai scritto i sottotitoli.

Ecco un esempio:

```html
<video controls>
  <source src="example.mp4" type="video/mp4" />
  <source src="example.webm" type="video/webm" />
  <track kind="subtitles" src="subtitles_en.vtt" srclang="en" />
</video>
```

Questo risulterà in un video che ha sottotitoli visualizzati, un po' così:

![Lettore video con controlli standard come play, stop, volume e sottotitoli attivi e disattivi. Il video in riproduzione mostra una scena di un uomo che tiene un'arma simile a una lancia, e una didascalia recita "Esta hoja tiene pasado oscuro."](video-player-with-captions.png)

Per maggiori dettagli, vedi [Aggiungere didascalie e sottotitoli al video HTML](/it/docs/Web/Media/Guides/Audio_and_video_delivery/Adding_captions_and_subtitles_to_HTML5_video). Puoi trovare [l'esempio](https://iandevlin.github.io/mdn/video-player-with-captions/) che accompagna questo articolo su GitHub, scritto da Ian Devlin (vedi anche il [codice sorgente](https://github.com/iandevlin/iandevlin.github.io/tree/master/mdn/video-player-with-captions)). Questo esempio utilizza JavaScript per consentire agli utenti di scegliere tra diversi sottotitoli. Nota che per attivare i sottotitoli, devi premere il pulsante "CC" e selezionare un'opzione — English, Deutsch, o Español.

> [!NOTE]
> Le tracce di testo e le trascrizioni ti aiutano anche con la {{Glossary("SEO", "SEO")}}, poiché i motori di ricerca prosperano soprattutto sul testo. Le tracce di testo consentono persino ai motori di ricerca di collegarsi direttamente a un punto a metà del video.

## Riepilogo

Questo capitolo ha fornito un riepilogo delle preoccupazioni di accessibilità per i contenuti multimediali, insieme ad alcune soluzioni pratiche.

Non è sempre facile rendere accessibile il multimedia. Se, per esempio, stai affrontando un gioco 3D immersivo o un'app di realtà virtuale, è piuttosto difficile fornire alternative testuali per un'esperienza del genere, e potresti sostenere che gli utenti ipovedenti non sono veramente nel target di tali app.

Puoi comunque assicurarti che tale app abbia un contrasto di colore abbastanza buono e una presentazione chiara in modo da risultare percepibile per chi ha poca vista/ciechi ai colori, e anche renderla accessibile tramite tastiera. Ricorda che l'accessibilità riguarda il fare il massimo possibile, piuttosto che cercare il 100% di accessibilità tutto il tempo, cosa che è spesso impossibile.

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/WAI-ARIA_basics","Learn_web_development/Core/Accessibility/Mobile", "Learn_web_development/Core/Accessibility")}}
