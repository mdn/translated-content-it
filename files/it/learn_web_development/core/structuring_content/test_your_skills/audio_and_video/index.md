---
title: "Metti alla prova le tue competenze: audio e video"
short-title: "Test: audio e video"
slug: Learn_web_development/Core/Structuring_content/Test_your_skills/Audio_and_video
l10n:
  sourceCommit: 2066cc916dfdcbb782340bf0ce562b230e947cba
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/HTML_video_and_audio", "Learn_web_development/Core/Structuring_content/Splash_page", "Learn_web_development/Core/Structuring_content")}}

L'obiettivo di questo test sulle competenze è aiutare a valutare se si è compreso come [incorporare contenuti video e audio in HTML](/it/docs/Learn_web_development/Core/Structuring_content/HTML_video_and_audio).

> [!NOTE]
> Per ottenere aiuto, leggere la nostra guida sull'uso di [Metti alla prova le tue competenze](/it/docs/Learn_web_development#test_your_skills). È anche possibile contattarci tramite uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Audio e video 1

In questa attività, l'obiettivo è incorporare un file audio nella pagina.

Per completare questa attività:

1. Aggiungere il percorso del file audio a un attributo appropriato per incorporarlo nella pagina. Il file audio si chiama `audio.mp3` ed è disponibile al percorso `https://github.com/mdn/learning-area/raw/refs/heads/main/html/multimedia-and-embedding/tasks/media-embed/media/audio.mp3`.
2. Aggiungere un attributo per fare in modo che i browser visualizzino alcuni controlli predefiniti.

Il punto di partenza dell'attività è simile a questo:

{{ EmbedLiveSample('audio-1', "100%", 150) }}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___audio-1
<h1>Basic audio embed</h1>

<audio></audio>
```

<!-- Stili condivisi -->

```css hidden live-sample___video-1 live-sample___audio-1 live-sample___video-1-finished live-sample___audio-1-finished
body {
  background-color: white;
  color: #333333;
  font:
    1em / 1.4 "Helvetica Neue",
    "Helvetica",
    "Arial",
    sans-serif;
  padding: 1em;
  margin: 0;
}

* {
  box-sizing: border-box;
}

audio,
video {
  border: 1px solid black;
}
```

Il contenuto aggiornato dovrebbe avere questo aspetto:

{{ EmbedLiveSample('audio-1-finished', "100%", 180) }}

<details>
<summary>Fare clic qui per visualizzare la soluzione</summary>

L'HTML completato dovrebbe avere questo aspetto:

```html live-sample___audio-1-finished
<h1>Basic audio embed</h1>

<audio
  controls
  src="https://github.com/mdn/learning-area/raw/refs/heads/main/html/multimedia-and-embedding/tasks/media-embed/media/audio.mp3"></audio>
```

</details>

## Audio e video 2

In questa attività, l'obiettivo è effettuare il markup di un player video leggermente più complesso, con più sorgenti, sottotitoli e altre funzionalità.

Per completare questa attività:

1. Aggiungere un attributo per fare in modo che i browser visualizzino alcuni controlli predefiniti.
2. Aggiungere più sorgenti, contenenti i percorsi ai file video. I file si chiamano `video.mp4` e `video.webm` e sono disponibili ai seguenti percorsi:
   1. `https://github.com/mdn/learning-area/raw/refs/heads/main/html/multimedia-and-embedding/tasks/media-embed/media/video.mp4`
   2. `https://github.com/mdn/learning-area/raw/refs/heads/main/html/multimedia-and-embedding/tasks/media-embed/media/video.webm`
3. Comunicare in anticipo al browser a quali formati video puntano le sorgenti, affinché possa scegliere in modo informato quale scaricare.
4. Assegnare al `<video>` una larghezza e un'altezza uguali alle sue dimensioni intrinseche (320 per 240 pixel).
5. Fare in modo che il video sia disattivato per impostazione predefinita.
6. Visualizzare le tracce di testo contenute nel file `https://raw.githubusercontent.com/mdn/learning-area/refs/heads/main/html/multimedia-and-embedding/tasks/media-embed/media/subtitles_en.vtt` durante la riproduzione del video. È necessario impostare esplicitamente il tipo come sottotitoli e la lingua dei sottotitoli come inglese.
7. Assicurarsi che chi legge possa identificare la lingua dei sottotitoli quando utilizza i controlli predefiniti.

Il punto di partenza dell'attività è simile a questo:

{{EmbedLiveSample('video-1', "100%", 300)}}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___video-1
<h1>Video embed</h1>

<video></video>
```

Il contenuto aggiornato dovrebbe avere questo aspetto:

{{EmbedLiveSample('video-1-finished', "100%", 380)}}

<details>
<summary>Fare clic qui per visualizzare la soluzione</summary>

L'HTML completato dovrebbe avere questo aspetto:

```html live-sample___video-1-finished
<h1>Video embed</h1>

<video controls width="320" height="240" muted>
  <source
    src="https://github.com/mdn/learning-area/raw/refs/heads/main/html/multimedia-and-embedding/tasks/media-embed/media/video.mp4"
    type="video/mp4" />
  <source
    src="https://github.com/mdn/learning-area/raw/refs/heads/main/html/multimedia-and-embedding/tasks/media-embed/media/video.webm"
    type="video/webm" />
  <track
    kind="subtitles"
    src="https://raw.githubusercontent.com/mdn/learning-area/refs/heads/main/html/multimedia-and-embedding/tasks/media-embed/media/subtitles_en.vtt"
    srclang="en"
    label="English" />
</video>
```

</details>

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/HTML_video_and_audio", "Learn_web_development/Core/Structuring_content/Splash_page", "Learn_web_development/Core/Structuring_content")}}
