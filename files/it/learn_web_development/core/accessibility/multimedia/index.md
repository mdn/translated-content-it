---
title: Multimedia accessibile
slug: Learn_web_development/Core/Accessibility/Multimedia
l10n:
  sourceCommit: ef78a9a3336c884fb3587e4ff833e64704296f01
---

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/Test_your_skills/WAI-ARIA","Learn_web_development/Core/Accessibility/Mobile", "Learn_web_development/Core/Accessibility")}}

Un'altra categoria di contenuti che può creare problemi di accessibilità è il multimedia. Ai contenuti video, audio e alle immagini occorre fornire adeguate alternative testuali affinché possano essere compresi dalle tecnologie assistive e dai relativi utenti. Questo articolo mostra come fare.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Conoscenza di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e delle migliori pratiche di accessibilità illustrate nelle lezioni precedenti del modulo.</a>.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>I problemi dei player multimediali nativi e come crearne di personalizzati.</li>
          <li>Lo scopo delle trascrizioni audio e delle tracce testuali (didascalie, sottotitoli e così via) per rendere accessibili i contenuti audio e video.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Multimedia e accessibilità

Finora, in questo modulo, sono stati esaminati diversi tipi di contenuti e ciò che deve essere fatto per garantirne l'accessibilità, dai semplici contenuti testuali alle tabelle di dati, alle immagini, ai controlli nativi come elementi dei moduli e pulsanti, fino a strutture di markup più complesse (con attributi [WAI-ARIA](/it/docs/Learn_web_development/Core/Accessibility/WAI-ARIA_basics)).

Questo articolo, invece, esamina un'altra classe generale di contenuti per la quale garantire l'accessibilità probabilmente non è altrettanto semplice: il multimedia. Immagini, tracce audio, video, elementi {{htmlelement("canvas")}} e così via non sono facilmente comprensibili dagli screen reader né navigabili tramite tastiera, quindi occorre fornire loro un aiuto.

Ma non disperare: qui vengono illustrate le tecniche disponibili per rendere il multimedia più accessibile.

## Immagini semplici

Le semplici alternative testuali per le immagini HTML sono già state trattate nell'articolo [HTML: una buona base per l'accessibilità](/it/docs/Learn_web_development/Core/Accessibility/HTML); è possibile consultarlo nuovamente per tutti i dettagli. In breve, occorre garantire che, ove possibile, il contenuto visivo disponga di un testo alternativo che gli screen reader possano rilevare e leggere ai propri utenti.

Per esempio:

```html
<img
  src="dinosaur.png"
  alt="A red Tyrannosaurus Rex: A two legged dinosaur standing upright like a human, with small arms, and a large head with lots of sharp teeth." />
```

## Controlli audio e video accessibili

L'implementazione di controlli per audio/video sul Web non dovrebbe essere un problema, giusto? Vediamo più nel dettaglio.

### Il problema dei controlli HTML nativi

Le istanze video e audio HTML includono persino un insieme di controlli integrati che consentono di gestire il media immediatamente. Per esempio (vedere il [codice sorgente](https://github.com/mdn/learning-area/blob/main/accessibility/multimedia/native-controls.html) di `native-controls.html` e la [versione live](https://mdn.github.io/learning-area/accessibility/multimedia/native-controls.html)):

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

L'attributo `controls` fornisce pulsanti di riproduzione/pausa, una barra di avanzamento e così via: i controlli di base che ci si aspetta da un player multimediale. Ecco come appare in Firefox e Chrome:

![Schermata dei controlli video in Firefox](native-controls-firefox.png)

![Schermata dei controlli video in Chrome](native-controls-chrome.png)

Tuttavia, questi controlli presentano dei problemi:

- Non sono accessibili tramite tastiera nella maggior parte dei browser, ovvero non è possibile spostarsi con il tasto Tab tra i controlli all'interno del player nativo. Opera e Chrome offrono questa possibilità in una certa misura, ma non è comunque ideale.
- Browser diversi assegnano ai controlli nativi stili e funzionalità differenti, e non è possibile applicarvi stili; ciò significa che non possono essere adattati facilmente a una guida di stile del sito.

Per rimediare, è possibile creare controlli personalizzati. Vediamo come.

### Creazione di controlli audio e video personalizzati

Video e audio HTML condividono un'API, HTML Media Element, che consente di associare funzionalità personalizzate a pulsanti e altri controlli, entrambi definiti dallo sviluppatore.

Prendiamo l'esempio video precedente e aggiungiamo controlli personalizzati.

#### Configurazione di base

Innanzitutto, scaricare una copia dei file [custom-controls-start.html](https://github.com/mdn/learning-area/blob/main/accessibility/multimedia/custom-controls-start.html), [custom-controls.css](https://github.com/mdn/learning-area/blob/main/accessibility/multimedia/custom-controls.css), [rabbit320.mp4](https://raw.githubusercontent.com/mdn/learning-area/master/accessibility/multimedia/rabbit320.mp4) e [rabbit320.webm](https://raw.githubusercontent.com/mdn/learning-area/master/accessibility/multimedia/rabbit320.webm), quindi salvarli in una nuova directory sul disco rigido.

Creare un nuovo file denominato main.js e salvarlo nella stessa directory.

Prima di tutto, osserviamo l'HTML del player video:

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

#### Configurazione di base JavaScript

Sotto il video sono stati inseriti alcuni semplici pulsanti di controllo. Naturalmente, questi controlli non eseguiranno alcuna operazione per impostazione predefinita; per aggiungere funzionalità verrà usato JavaScript.

Per prima cosa, è necessario memorizzare riferimenti a ciascuno dei controlli: aggiungere il seguente codice all'inizio del file JavaScript:

```js
const playPauseBtn = document.querySelector(".play-pause");
const stopBtn = document.querySelector(".stop");
const rwdBtn = document.querySelector(".rwd");
const fwdBtn = document.querySelector(".fwd");
const timeLabel = document.querySelector(".time");
```

Successivamente, occorre ottenere un riferimento al player video/audio stesso: aggiungere questa riga sotto le righe precedenti:

```js
const player = document.querySelector("video");
```

Questo conserva un riferimento a un oggetto [`HTMLMediaElement`](/it/docs/Web/API/HTMLMediaElement), che dispone di varie proprietà e metodi utili utilizzabili per collegare funzionalità ai pulsanti.

Prima di passare alla creazione delle funzionalità dei pulsanti, rimuoviamo i controlli nativi affinché non interferiscano con quelli personalizzati. Aggiungere quanto segue, sempre alla fine del JavaScript:

```js
player.removeAttribute("controls");
```

Procedere in questo modo, anziché omettere semplicemente l'attributo `controls` fin dall'inizio, presenta il vantaggio che, se JavaScript non funziona per qualunque ragione, l'utente disporrà comunque di alcuni controlli.

#### Collegamento dei pulsanti

Per prima cosa, configuriamo il pulsante di riproduzione/pausa. È possibile farlo alternare tra riproduzione e pausa con una semplice funzione condizionale, come la seguente. Aggiungerla al codice, alla fine:

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

Successivamente, aggiungere questo codice alla fine, che controlla il pulsante di arresto:

```js
stopBtn.onclick = () => {
  player.pause();
  player.currentTime = 0;
  playPauseBtn.textContent = "Play";
};
```

Non è disponibile alcuna funzione `stop()` per gli [`HTMLMediaElement`](/it/docs/Web/API/HTMLMediaElement), quindi si usa invece `pause()` e, contemporaneamente, si imposta `currentTime` su 0.

Ora i pulsanti di riavvolgimento e avanzamento rapido: aggiungere i seguenti blocchi alla fine del codice:

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

Sono molto semplici: aggiungono o sottraggono 3 secondi a `currentTime` ogni volta che vengono selezionati. In un player video reale, probabilmente sarebbe preferibile una barra di ricerca più elaborata o qualcosa di simile.

Notare che viene anche verificato se `currentTime` è maggiore della `duration` totale del media oppure se il media non è in riproduzione quando viene premuto `fwdBtn`. Se una delle due condizioni è vera, il video viene arrestato per evitare che l'interfaccia utente si comporti in modo errato se si tenta di avanzare rapidamente quando il video non è in riproduzione oppure oltre la fine del video.

Infine, aggiungere il seguente codice alla fine per controllare la visualizzazione del tempo trascorso:

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

Ogni volta che il tempo viene aggiornato, una volta al secondo, viene eseguita questa funzione. Essa calcola il numero di minuti e secondi dal valore `currentTime` fornito, che è espresso in secondi, aggiunge uno 0 iniziale se il valore dei minuti o dei secondi è inferiore a 10, quindi crea il testo visualizzato e lo aggiunge all'etichetta del tempo.

#### Approfondimenti

Questo fornisce un'idea di base su come aggiungere funzionalità personalizzate alle istanze dei player video/audio. Per ulteriori informazioni su come aggiungere funzionalità più complesse ai player video/audio, vedere:

- [Distribuzione di audio e video](/it/docs/Web/Media/Guides/Audio_and_video_delivery)
- [Fondamenti dello stile dei player video](/it/docs/Web/Media/Guides/Audio_and_video_delivery/Video_player_styling_basics)
- [Creazione di un player video cross-browser](/it/docs/Web/Media/Guides/Audio_and_video_delivery/cross_browser_video_player)

È stato inoltre creato un esempio avanzato per mostrare come creare un sistema orientato agli oggetti che individua ogni player video e audio sulla pagina, indipendentemente dal loro numero, e vi aggiunge i controlli personalizzati. Vedere [custom-controls-oojs](https://mdn.github.io/learning-area/accessibility/multimedia/custom-controls-OOJS/) (vedere anche il [codice sorgente](https://github.com/mdn/learning-area/tree/main/accessibility/multimedia/custom-controls-OOJS)).

## Trascrizioni audio

Per fornire alle persone sorde l'accesso ai contenuti audio, occorre creare trascrizioni testuali. Queste possono essere incluse in qualche modo nella stessa pagina dell'audio oppure inserite in una pagina separata e collegate tramite link.

Per quanto riguarda la creazione effettiva della trascrizione, le opzioni sono:

- Servizi commerciali: è possibile pagare un professionista affinché realizzi la trascrizione; vedere, per esempio, aziende come [Scribie](https://scribie.com/), [Casting Words](https://castingwords.com/) o [Rev](https://www.rev.com/). Informarsi e chiedere consigli per assicurarsi di trovare un'azienda affidabile con cui sarà possibile lavorare efficacemente.
- Trascrizione da parte della comunità/dal basso/autonoma: se si fa parte di una comunità attiva o di un team sul posto di lavoro, è possibile chiedere aiuto per realizzare le trascrizioni. È anche possibile provare a farle autonomamente.
- Servizi automatizzati: sono disponibili servizi di IA, come [Trint](https://trint.com/). Caricare un file video/audio sul sito e questo lo trascriverà automaticamente. Su YouTube è possibile scegliere di generare didascalie/trascrizioni automatiche. A seconda della chiarezza dell'audio parlato, la qualità della trascrizione risultante varierà notevolmente.

Come per la maggior parte delle cose nella vita, generalmente si ottiene ciò per cui si paga; i diversi servizi varieranno per accuratezza e tempo necessario a produrre la trascrizione. Se si paga un'azienda affidabile o un servizio di IA per effettuare la trascrizione, probabilmente verrà eseguita rapidamente e con alta qualità. Se non si desidera pagare, è probabile che venga realizzata con qualità inferiore e/o più lentamente.

Non è accettabile pubblicare una risorsa audio promettendo di pubblicare la trascrizione in un secondo momento: tali promesse spesso non vengono mantenute, il che eroderà la fiducia tra il sito e i suoi utenti. Se l'audio presentato è, ad esempio, una riunione in presenza o un'esibizione parlata dal vivo, sarebbe accettabile prendere appunti durante l'evento, pubblicarli integralmente insieme all'audio e poi cercare aiuto per sistemarli in seguito.

### Esempi di trascrizioni

Se viene usato un servizio automatizzato, probabilmente sarà necessario utilizzare l'interfaccia utente fornita dallo strumento. Per esempio, osservare il video [Wait, ARIA Roles Have Categories?](https://www.youtube.com/watch?v=mwF-PpJOjMs) e selezionare il menu con tre punti (. . .) _> Mostra trascrizione_. La trascrizione verrà visualizzata in un pannello separato.

Se si crea un'interfaccia utente personalizzata per presentare l'audio e la trascrizione associata, è possibile farlo in qualsiasi modo, ma potrebbe avere senso includerla in un pannello mostrabile/nascondibile; vedere l'esempio [audio-transcript-ui](https://mdn.github.io/learning-area/accessibility/multimedia/audio-transcript-ui/) (vedere anche il [codice sorgente](https://github.com/mdn/learning-area/tree/main/accessibility/multimedia/audio-transcript-ui)).

### Audiodescrizioni

Nei casi in cui elementi visivi accompagnano l'audio, sarà necessario fornire delle audiodescrizioni per descrivere quel contenuto aggiuntivo.

In molti casi, ciò assumerà la forma di un video, nel qual caso è possibile implementare le didascalie utilizzando le tecniche descritte nella sezione successiva dell'articolo.

Tuttavia, esistono alcuni casi limite. Si potrebbe avere, per esempio, una registrazione audio di una riunione che fa riferimento a una risorsa di accompagnamento, come un foglio di calcolo o un grafico. In questi casi, occorre assicurarsi che le risorse vengano fornite insieme all'audio e alla trascrizione e collegarle specificamente nei punti in cui vengono citate nella trascrizione. Naturalmente, questo aiuterà tutti gli utenti, non soltanto le persone sorde.

> [!NOTE]
> Una trascrizione audio generalmente aiuta più gruppi di utenti. Oltre a offrire agli utenti sordi l'accesso alle informazioni contenute nell'audio, si pensi a un utente con una connessione a bassa larghezza di banda, per il quale scaricare l'audio sarebbe scomodo. Si pensi anche a un utente in un ambiente rumoroso come un pub o un bar, che cerca di accedere alle informazioni ma non riesce a sentirle a causa del rumore.

## Tracce testuali video

Per rendere i video accessibili alle persone sorde, alle persone con disabilità visive o ad altri gruppi di utenti, come coloro che dispongono di bassa larghezza di banda o non comprendono la lingua in cui è registrato il video, occorre includere tracce testuali insieme al contenuto video.

> [!NOTE]
> Le tracce testuali sono utili anche per potenzialmente qualsiasi utente, non solo per le persone con disabilità. Per esempio, alcuni utenti potrebbero non essere in grado di ascoltare l'audio perché si trovano in ambienti rumorosi, come un bar affollato durante la trasmissione di una partita sportiva, oppure potrebbero non voler disturbare altre persone se si trovano in un luogo silenzioso, come una biblioteca.

Non è un concetto nuovo: i servizi televisivi dispongono di sottotitoli per non udenti da molto tempo:

![Fotogramma di un cartone animato d'epoca con sottotitoli per non udenti: "Good work, Goldie. Keep it up!"](closed-captions.png)

Molti paesi offrono film in inglese con sottotitoli scritti nelle proprie lingue native e spesso sui DVD sono disponibili sottotitoli in diverse lingue, come mostrato qui sotto:

![Un film inglese con sottotitoli tedeschi: "Emo, warum erkennst du nicht die Schonheit dieses Ortes?"](subtitles_german.png)

Esistono diversi tipi di tracce testuali per scopi diversi. I principali che si incontreranno sono:

- Didascalie: a beneficio degli utenti sordi che non possono ascoltare la traccia audio; includono le parole pronunciate e informazioni contestuali, come chi ha pronunciato le parole, se le persone erano arrabbiate o tristi e quale atmosfera sta creando la musica in quel momento.
- Sottotitoli: includono traduzioni del dialogo audio, per gli utenti che non comprendono la lingua parlata.
- Descrizioni: includono descrizioni per persone con disabilità visive che non possono vedere il video, per esempio l'aspetto della scena.
- Titoli dei capitoli: marcatori di capitolo destinati ad aiutare l'utente a navigare nella risorsa multimediale.

### Implementazione delle tracce testuali per video HTML

Le tracce testuali da visualizzare con i video HTML devono essere scritte in WebVTT, un formato di testo contenente più stringhe di testo insieme a metadati, come il momento del video in cui si desidera visualizzare ogni stringa di testo, e persino informazioni limitate su stile e posizionamento. Queste stringhe di testo sono chiamate cue.

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

Per visualizzarlo insieme alla riproduzione multimediale HTML, occorre:

- Salvarlo come file .vtt in una posizione appropriata.
- Collegare il file .vtt con l'elemento {{htmlelement("track")}}. `<track>` deve essere inserito all'interno di `<audio>` o `<video>`, ma dopo tutti gli elementi `<source>`. Usare l'attributo [`kind`](/it/docs/Web/HTML/Reference/Elements/track#kind) per specificare se i cue sono sottotitoli, didascalie o descrizioni. Usare inoltre [`srclang`](/it/docs/Web/HTML/Reference/Elements/track#srclang) per indicare al browser in quale lingua sono stati scritti i sottotitoli.

Ecco un esempio:

```html
<video controls>
  <source src="example.mp4" type="video/mp4" />
  <source src="example.webm" type="video/webm" />
  <track kind="subtitles" src="subtitles_en.vtt" srclang="en" />
</video>
```

Questo produrrà un video con sottotitoli visualizzati. Per un'applicazione completa e il relativo codice sorgente, vedere [Aggiunta di didascalie e sottotitoli ai video HTML](/it/docs/Web/Media/Guides/Audio_and_video_delivery/Adding_captions_and_subtitles_to_HTML5_video). Questo esempio usa JavaScript per consentire agli utenti di scegliere tra diversi sottotitoli. Notare che per attivare i sottotitoli occorre premere il pulsante "CC" e selezionare un'opzione: English, Deutsch o Español.

> [!NOTE]
> Le tracce testuali e le trascrizioni aiutano anche con la {{Glossary("SEO", "SEO")}}, poiché i motori di ricerca prosperano particolarmente sui testi. Le tracce testuali consentono persino ai motori di ricerca di collegarsi direttamente a un punto specifico del video.

## Riepilogo

Questo capitolo ha fornito un riepilogo dei problemi di accessibilità relativi ai contenuti multimediali, insieme ad alcune soluzioni pratiche.

Non è sempre facile rendere accessibile il multimedia. Se, per esempio, si ha a che fare con un gioco 3D immersivo o un'app di realtà virtuale, è piuttosto difficile fornire alternative testuali per un'esperienza di questo tipo e si potrebbe sostenere che gli utenti con disabilità visive non rientrino realmente nel pubblico di destinazione di tali app.

È però possibile assicurarsi che un'app di questo tipo abbia un contrasto cromatico sufficientemente buono e una presentazione chiara, in modo che sia percepibile da persone con ipovisione o daltonismo, e renderla anche accessibile tramite tastiera. Ricordare che l'accessibilità consiste nel fare quanto più possibile, piuttosto che aspirare sempre al 100% di accessibilità, cosa spesso impossibile.

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/Test_your_skills/WAI-ARIA","Learn_web_development/Core/Accessibility/Mobile", "Learn_web_development/Core/Accessibility")}}
