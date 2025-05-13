---
title: "Metti alla prova le tue abilità: Multimedia e incorporamento"
short-title: Audio e video
slug: Learn_web_development/Core/Structuring_content/Test_your_skills/Multimedia_and_embedding
l10n:
  sourceCommit: 93f54b6e1fdfef1375233abb265f101bd6866f99
---

L'obiettivo di questo test di abilità è valutare se comprende come [incorporare contenuti video e audio in HTML](/it/docs/Learn_web_development/Core/Structuring_content/HTML_video_and_audio).

> [!NOTE]
> Può provare soluzioni negli editor interattivi su questa pagina o in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
>
> Se rimane bloccato, può contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Compito 1

In questo compito, vogliamo che lei incorpori un file audio sulla pagina. Deve:

- Aggiungere il percorso al file audio a un attributo appropriato per incorporarlo nella pagina. L'audio si chiama `audio.mp3` e si trova in una cartella all'interno della cartella corrente chiamata `media`.
- Aggiungere un attributo per far visualizzare ai browser alcuni controlli predefiniti.
- Aggiungere del testo di riserva appropriato per i browser che non supportano `<audio>`.

Provi a aggiornare il codice live qui sotto per ricreare l'esempio finito:

{{EmbedGHLiveSample("learning-area/html/multimedia-and-embedding/tasks/media-embed/mediaembed1.html", '100%', 700)}}

> [!CALLOUT]
>
> [Scarichi il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/html/multimedia-and-embedding/tasks/media-embed/mediaembed1-download.html) per lavorare nel suo editor o in un editor online.

## Compito 2

In questo compito, vogliamo che lei strutturi un lettore video leggermente più complesso, con più sorgenti, sottotitoli e altre caratteristiche. Deve:

- Aggiungere un attributo per fare in modo che i browser visualizzino alcuni controlli predefiniti.
- Aggiungere del testo di riserva appropriato per i browser che non supportano `<video>`.
- Aggiungere più sorgenti, contenenti i percorsi ai file video. I file si chiamano `video.mp4` e `video.webm` e si trovano in una cartella all'interno della cartella corrente chiamata `media`.
- Far sapere al browser in anticipo quali formati video puntano le sorgenti, in modo che possa scegliere in modo informato quale scaricare in anticipo.
- Dare al `<video>` una larghezza e un'altezza pari alla sua dimensione intrinseca (320 per 240 pixel).
- Rendere il video muto per impostazione predefinita.
- Visualizzare le tracce di testo contenute nella cartella `media`, in un file chiamato `subtitles_en.vtt`, quando il video è in riproduzione. Deve impostare esplicitamente il tipo come sottotitoli e la lingua dei sottotitoli in inglese.
- Assicurarsi che i lettori possano identificare la lingua dei sottotitoli quando utilizzano i controlli predefiniti.

Provi ad aggiornare il codice live qui sotto per ricreare l'esempio finito:

{{EmbedGHLiveSample("learning-area/html/multimedia-and-embedding/tasks/media-embed/mediaembed2.html", '100%', 700)}}

> [!CALLOUT]
>
> [Scarichi il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/html/multimedia-and-embedding/tasks/media-embed/mediaembed2-download.html) per lavorare nel suo editor o in un editor online.
