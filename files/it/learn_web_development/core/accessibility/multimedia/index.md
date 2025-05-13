---
title: Multimedia accessibile
slug: Learn_web_development/Core/Accessibility/Multimedia
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/WAI-ARIA_basics","Learn_web_development/Core/Accessibility/Mobile", "Learn_web_development/Core/Accessibility")}}

Un'altra categoria di contenuti che può creare problemi di accessibilità è il multimedia. Video, audio e contenuti di immagini devono avere alternative testuali adeguate in modo che possano essere comprese dalle tecnologie assistive e dai loro utenti. Questo articolo spiega come fare.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Familiarità con <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e le migliori pratiche di accessibilità come insegnato nelle lezioni precedenti di questo modulo.</td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>I problemi con i lettori multimediali nativi e come crearne di personalizzati.</li>
          <li>Lo scopo delle trascrizioni audio e delle tracce di testo (didascalie, sottotitoli, ecc.) nel rendere accessibili i contenuti audio e video.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Multimedia e accessibilità

Finora, in questo modulo abbiamo esaminato una varietà di contenuti e cosa è necessario fare per garantirne l'accessibilità, spaziando da semplici contenuti testuali a tabelle di dati, immagini, controlli nativi come elementi dei moduli e pulsanti, e anche strutture di markup più complesse (con attributi [WAI-ARIA](/it/docs/Learn_web_development/Core/Accessibility/WAI-ARIA_basics)).

Questo articolo, invece, esamina un'altra generale categoria di contenuti per cui non è altrettanto semplice garantire l'accessibilità — il multimedia. Immagini, tracce audio, video, elementi {{htmlelement("canvas")}}, ecc., non sono facilmente comprensibili dai lettori di schermo o navigabili tramite tastiera, e dobbiamo fornire loro un supporto.

Ma non disperi — qui l'aiuteremo a padroneggiare le tecniche disponibili per rendere il multimedia più accessibile.

## Immagini semplici

Abbiamo già trattato le semplici alternative testuali per le immagini HTML nel nostro articolo [HTML: Una buona base per l'accessibilità](/it/docs/Learn_web_development/Core/Accessibility/HTML) — può fare riferimento a quell'articolo per i dettagli completi. In breve, dovrebbe assicurarsi che, dove possibile, il contenuto visivo abbia un testo alternativo disponibile per i lettori di schermo così da poterlo leggere ai loro utenti.

Per esempio:

```html
<img
  src="dinosaur.png"
  alt="A red Tyrannosaurus Rex: A two legged dinosaur standing upright like a human, with small arms, and a large head with lots of sharp teeth." />
```

## Controlli audio e video accessibili

Implementare controlli per audio/video basati sul web non dovrebbe essere un problema, giusto? Esaminiamo.

### Il problema con i controlli HTML nativi

Le istanze di video e audio HTML offrono anche un set di controlli integrati che le consentono di controllare il media direttamente. Per esempio (vedere il codice sorgente `native-controls.html` [source code](https://github.com/mdn/learning-area/blob/main/accessibility/multimedia/native-controls.html) e [dal vivo](https://mdn.github.io/learning-area/accessibility/multimedia/native-controls.html)):

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

L'attributo controls fornisce pulsanti play/pausa, barra di scorrimento, ecc. — i controlli di base che ci si aspetterebbe da un lettore multimediale. Assomiglia a questo in Firefox e Chrome:

![Schermata dei Controlli Video in Firefox](native-controls-firefox.png)

![Schermata dei Controlli Video in Chrome](native-controls-chrome.png)

Tuttavia, ci sono problemi con questi controlli:

- Non sono accessibili da tastiera nella maggior parte dei browser, cioè non si può navigare tra i controlli all'interno del lettore nativo. Opera e Chrome offrono questo in una certa misura, ma non è ancora ideale.
- I diversi browser presentano diversi stili e funzionalità per i controlli nativi, e questi non sono stilizzabili, il che significa che non possono facilmente seguire una guida di stile del sito.

Per rimediare a questo, possiamo creare i nostri controlli personalizzati. Vediamo come.

### Creare controlli audio e video personalizzati

Video e audio HTML condividono un'API — HTML Media Element — che le consente di mappare funzioni personalizzate a pulsanti e altri controlli — entrambi definiti da lei.

Prendiamo l'esempio di video sopra e aggiungiamo controlli personalizzati.

#### Configurazione di base

Per prima cosa, prenda una copia dei nostri file [custom-controls-start.html](https://github.com/mdn/learning-area/blob/main/accessibility/multimedia/custom-controls-start.html), [custom-controls.css](https://github.com/mdn/learning-area/blob/main/accessibility/multimedia/custom-controls.css), [rabbit320.mp4](https://raw.githubusercontent.com/mdn/learning-area/master/accessibility/multimedia/rabbit320.mp4), and [rabbit320.webm](https://raw.githubusercontent.com/mdn/learning-area/master/accessibility/multimedia/rabbit320.webm) e li salvi in una nuova directory sul suo hard drive.

Crei un nuovo file chiamato main.js e lo salvi nella stessa directory.

Iniziamo guardando l'HTML per il lettore video, nell'HTML:

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

Abbiamo inserito alcuni semplici pulsanti di controllo sotto il nostro video. Questi controlli naturalmente non faranno nulla per impostazione predefinita; per aggiungere funzionalità, useremo JavaScript.

Prima di tutto, dobbiamo memorizzare i riferimenti a ciascuno dei controlli — aggiunga il seguente codice all'inizio del file JavaScript:

```js
const playPauseBtn = document.querySelector(".play-pause");
const stopBtn = document.querySelector(".stop");
const rwdBtn = document.querySelector(".rwd");
const fwdBtn = document.querySelector(".fwd");
const timeLabel = document.querySelector(".time");
```

Successivamente, dobbiamo ottenere un riferimento al lettore video/audio stesso — aggiunga questa linea sotto le righe precedenti:

```js
const player = document.querySelector("video");
```

Questo contiene un riferimento a un oggetto [`HTMLMediaElement`](/it/docs/Web/API/HTMLMediaElement), che ha diverse proprietà e metodi utili disponibili che possono essere usati per collegare le funzionalità ai nostri pulsanti.

Prima di passare alla creazione della funzionalità dei nostri pulsanti, rimuoviamo i controlli nativi in modo che non interferiscano con i nostri controlli personalizzati. Aggiunga il seguente codice, nuovamente in fondo al suo JavaScript:

```js
player.removeAttribute("controls");
```

Facendo in questo modo piuttosto che non includere l'attributo controls in primo luogo, si ha il vantaggio che se il nostro JavaScript non funziona per qualsiasi motivo, l'utente avrà comunque alcuni controlli disponibili.

#### Collegare i nostri pulsanti

Per prima cosa, configuriamo il pulsante play/pausa. Possiamo farlo alternare tra play e pausa con una semplice funzione condizionale, come la seguente. La aggiunga al suo codice, in fondo:

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

Successivamente, aggiunge questo codice alla fine, che controlla il pulsante stop:

```js
stopBtn.onclick = () => {
  player.pause();
  player.currentTime = 0;
  playPauseBtn.textContent = "Play";
};
```

Non esiste una funzione `stop()` disponibile su [`HTMLMediaElement`](/it/docs/Web/API/HTMLMediaElement), quindi lo `pause()`, e allo stesso tempo imposta il `currentTime` a 0.

Passiamo ai pulsanti di riavvolgimento e avanzamento rapido — aggiunge i seguenti blocchi alla fine del suo codice:

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

Sono molto semplici, aggiungendo o sottraendo 3 secondi al `currentTime` ogni volta che vengono cliccati. In un vero lettore video, probabilmente vorrebbe una barra di scorrimento più elaborata o qualcosa di simile.

Si noti che controlliamo anche se il `currentTime` è maggiore della `duration` totale del media o se il media non è in riproduzione quando viene premuto il `fwdBtn`. Se una delle due condizioni è vera, fermiamo il video per evitare che l'interfaccia utente si incasini se si cerca di avanzare rapidamente quando il video non è in riproduzione o si cerca di andare oltre la fine del video.

Infine, aggiunga il seguente codice alla fine per controllare la visualizzazione del tempo trascorso:

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

Ogni volta che il tempo viene aggiornato (una volta al secondo), attiviamo questa funzione. Calcola il numero di minuti e secondi dal valore `currentTime` dato (che è in secondi), aggiunge uno 0 iniziale se il valore dei minuti o dei secondi è inferiore a 10, e quindi crea la visualizzazione e la aggiunge all'etichetta del tempo.

#### Ulteriori letture

Questo le dà un'idea di base su come aggiungere funzionalità personalizzate alle istanze dei lettori video/audio. Per ulteriori informazioni su come aggiungere funzionalità più complesse ai lettori video/audio, vedere:

- [Distribuzione audio e video](/it/docs/Web/Media/Guides/Audio_and_video_delivery)
- [Nozioni di base sullo stile del lettore video](/it/docs/Web/Media/Guides/Audio_and_video_delivery/Video_player_styling_basics)
- [Creare un lettore video compatibile tra browser](/it/docs/Web/Media/Guides/Audio_and_video_delivery/cross_browser_video_player)

Abbiamo anche creato un esempio avanzato per mostrare come potrebbe creare un sistema orientato agli oggetti che trova ogni lettore video e audio sulla pagina (non importa quanti ce ne siano) e aggiunge i nostri controlli personalizzati. Vedere [custom-controls-oojs](https://mdn.github.io/learning-area/accessibility/multimedia/custom-controls-OOJS/) (veda anche il [codice sorgente](https://github.com/mdn/learning-area/tree/main/accessibility/multimedia/custom-controls-OOJS)).

## Trascrizioni audio

Per fornire alle persone sorde l'accesso ai contenuti audio, è necessario creare trascrizioni testuali. Queste possono essere incluse nella stessa pagina con l'audio in qualche modo o incluse su una pagina separata e collegate ad essa.

Quando si tratta di creare effettivamente la trascrizione, le sue opzioni sono:

- Servizi commerciali — Potrebbe pagare un professionista per fare la trascrizione, vedere ad esempio aziende come [Scribie](https://scribie.com/), [Casting Words](https://castingwords.com/), o [Rev](https://www.rev.com/). Guardi intorno e chieda consiglio per assicurarsi di trovare una società rispettabile con cui sarà in grado di lavorare efficacemente.
- Trascrizione comunitaria/di base/personale — Se fa parte di una comunità attiva o di un team sul posto di lavoro, potrebbe chiedere loro aiuto per fare le traduzioni. Potrebbe persino provare a farle da solo.
- Servizi automatizzati — Ci sono servizi di intelligenza artificiale disponibili, come [Trint](https://trint.com/) o [Transcribear](https://transcribear.com/). Carichi un file video/audio sul sito, e questo lo trascriverà automaticamente per lei. Su YouTube, può scegliere di generare didascalie/trascrizioni automatiche. A seconda di quanto chiaro è l'audio parlato, la qualità della trascrizione risultante varierà notevolmente.

Come con la maggior parte delle cose nella vita, tende a ottenere ciò per cui paga; i diversi servizi varieranno in accuratezza e tempo necessario per produrre la trascrizione. Se paga una società rispettabile o un servizio di intelligenza artificiale per fare la trascrizione, probabilmente lo farà rapidamente e ad alta qualità. Se non vuole pagare per questo, probabilmente lo farà a una qualità inferiore e/o lentamente.

Non è accettabile pubblicare una risorsa audio ma promettere di pubblicare la trascrizione in seguito — tali promesse spesso non vengono mantenute, il che eroderà la fiducia tra lei e i suoi utenti. Se l'audio che sta presentando è qualcosa come una riunione faccia a faccia o una performance parlata dal vivo, sarebbe accettabile prendere appunti durante la performance, pubblicarli per intero insieme all'audio, quindi cercare aiuto nel pulire gli appunti in seguito.

### Esempi di trascrizione

Se utilizza un servizio automatizzato, probabilmente dovrà utilizzare l'interfaccia utente che lo strumento fornisce. Ad esempio, dia un'occhiata al nostro video [Wait, ARIA Roles Have Categories?](https://www.youtube.com/watch?v=mwF-PpJOjMs) e selezioni il menu con tre punti (. . .) _> Mostra trascrizione_. Vedrà la trascrizione comparire in un pannello separato.

Se sta creando la sua interfaccia utente per presentare il suo audio e la trascrizione associata, può farlo come preferisce, ma potrebbe essere sensato includerlo in un pannello che possa essere mostrato/nascosto; veda il nostro esempio [audio-transcript-ui](https://mdn.github.io/learning-area/accessibility/multimedia/audio-transcript-ui/) (veda anche il [codice sorgente](https://github.com/mdn/learning-area/tree/main/accessibility/multimedia/audio-transcript-ui)).

### Descrizioni audio

In occasioni in cui i visivi accompagnano il suo audio, dovrà fornire descrizioni audio di qualche tipo per descrivere quel contenuto aggiuntivo.

In molti casi, questo prenderà la forma di un video, nel qual caso può implementare le didascalie utilizzando le tecniche descritte nella sezione successiva dell'articolo.

Tuttavia, ci sono alcuni casi limite. Potrebbe, per esempio, avere una registrazione audio di una riunione che fa riferimento a una risorsa allegata come un foglio di calcolo o un grafico. In tali casi, dovrebbe cercare di assicurarsi che le risorse siano fornite insieme all'audio + trascrizione, e specificamente collegarsi ad esse nei punti in cui vengono citate nella trascrizione. Questo ovviamente aiuterà tutti gli utenti, non solo le persone sorde.

> [!NOTE]
> Una trascrizione audio aiuterà in generale più gruppi di utenti. Oltre a dare accesso agli utenti sordi alle informazioni contenute nell'audio, pensi a un utente con una connessione a bassa larghezza di banda, che troverebbe scomodo scaricare l'audio. Pensi anche a un utente in un ambiente rumoroso come un pub o bar, che sta cercando di accedere alle informazioni ma non può sentirle sopra il rumore.

## Tracce di testo video

Per rendere il video accessibile ai sordi, ai non vedenti o ad altri gruppi di utenti (come quelli con bassa larghezza di banda, o che non comprendono la lingua in cui è registrato il video), deve includere tracce di testo insieme al suo contenuto video.

> [!NOTE]
> Le tracce di testo sono utili anche per potenzialmente qualsiasi utente, non solo per quelli con disabilità. Ad esempio, alcuni utenti potrebbero non essere in grado di ascoltare l'audio perché si trovano in ambienti rumorosi (come un bar affollato quando viene mostrata una partita sportiva) o potrebbero non voler disturbare gli altri se si trovano in un luogo tranquillo (come una biblioteca).

Non è un concetto nuovo — i servizi televisivi offrono sottotitoli disponibili da parecchio tempo:

![Frame di un cartone animato d'epoca con didascalie chiuse "Good work, Goldie. Keep it up!"](closed-captions.png)

Molti paesi offrono film in inglese con sottotitoli scritti nelle loro lingue native, e diversi sottotitoli in lingua sono spesso disponibili sui DVD, come mostrato di seguito:

![Un film in inglese con sottotitoli in tedesco "Emo, warum erkennst du nicht die Schonheit dieses Ortes?"](subtitles_german.png)

Esistono diversi tipi di tracce di testo per diversi scopi. I principali che incontrerà sono:

- Didascalie — Destinate a beneficio degli utenti sordi che non possono ascoltare la traccia audio, comprese le parole pronunciate, e le informazioni contestuali come chi ha parlato le parole, se le persone erano arrabbiate o tristi, e quale umore sta creando attualmente la musica.
- Sottotitoli — Includono traduzioni del dialogo audio, per utenti che non comprendono la lingua parlata.
- Descrizioni — Queste includono descrizioni per i non vedenti che non possono vedere il video, ad esempio, com'è la scena.
- Titoli dei capitoli — Segnaposto per capitoli intesi ad aiutare l'utente a navigare nella risorsa media.

### Implementazione di tracce di testo video HTML

Le tracce di testo per la visualizzazione con video HTML devono essere scritte in WebVTT, un formato di testo contenente più stringhe di testo insieme ai metadati come il momento nel video in cui si desidera che ciascuna stringa venga visualizzata, e persino informazioni di stile/posizionamento limitate. Queste stringhe di testo sono chiamate cue.

Un file WebVTT tipico avrà l'aspetto di questo:

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

Per far sì che questo venga visualizzato insieme alla riproduzione del media HTML, deve:

- Salvare come un file .vtt in un luogo sensato.
- Collegarlo al file .vtt con l'elemento {{htmlelement("track")}}. `<track>` dovrebbe essere posizionato all'interno di `<audio>` o `<video>`, ma dopo tutti gli elementi `<source>`. Utilizzi l'attributo [`kind`](/it/docs/Web/HTML/Reference/Elements/track#kind) per specificare se i cue sono sottotitoli, didascalie, o descrizioni. Inoltre, utilizzi [`srclang`](/it/docs/Web/HTML/Reference/Elements/track#srclang) per indicare al browser in quale lingua ha scritto i sottotitoli.

Ecco un esempio:

```html
<video controls>
  <source src="example.mp4" type="video/mp4" />
  <source src="example.webm" type="video/webm" />
  <track kind="subtitles" src="subtitles_en.vtt" srclang="en" />
</video>
```

Questo risulterà in un video che ha sottotitoli visualizzati, in modo simile a questo:

![Lettore video con controlli standard come play, stop, volume e didascalie accese e spente. Il video in riproduzione mostra una scena di un uomo con in mano un'arma simile a una lancia, e una didascalia recita "Esta hoja tiene pasado oscuro."](video-player-with-captions.png)

Per ulteriori dettagli, veda [Aggiungere didascalie e sottotitoli al video HTML](/it/docs/Web/Media/Guides/Audio_and_video_delivery/Adding_captions_and_subtitles_to_HTML5_video). Può trovare [l'esempio](https://iandevlin.github.io/mdn/video-player-with-captions/) che accompagna questo articolo su GitHub, scritto da Ian Devlin (veda anche il [codice sorgente](https://github.com/iandevlin/iandevlin.github.io/tree/master/mdn/video-player-with-captions)). Questo esempio utilizza JavaScript per permettere agli utenti di scegliere tra diversi sottotitoli. Si noti che per attivare i sottotitoli, è necessario premere il pulsante "CC" e selezionare un'opzione — English, Deutsch o Español.

> [!NOTE]
> Le tracce di testo e le trascrizioni aiutano anche con {{Glossary("SEO", "SEO")}}, poiché i motori di ricerca attraversano particolarmente il testo. Le tracce di testo permettono persino ai motori di ricerca di collegarsi direttamente a una parte del video.

## Riepilogo

Questo capitolo ha fornito un riassunto delle considerazioni di accessibilità per i contenuti multimediali, insieme ad alcune soluzioni pratiche.

Non è sempre facile rendere accessibile il multimedia. Se ad esempio sta trattando con un gioco 3D immersivo o un'app di realtà virtuale, è abbastanza difficile fornire alternative testuali per un'esperienza del genere, e potrebbe sostenere che gli utenti non vedenti non sono davvero nel target di tali app.

Può comunque assicurarsi che un'app del genere abbia un contrasto di colore abbastanza buono e una presentazione chiara in modo che sia percepibile per chi ha una visione debole/cieco ai colori, e anche renderla accessibile tramite tastiera. Ricordi che l'accessibilità riguarda il fare il massimo possibile, piuttosto che cercare di raggiungere una completa accessibilità al 100% ogni volta, cosa che spesso è impossibile.

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/WAI-ARIA_basics","Learn_web_development/Core/Accessibility/Mobile", "Learn_web_development/Core/Accessibility")}}
