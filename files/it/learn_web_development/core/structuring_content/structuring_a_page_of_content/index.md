---
title: "Sfida: Strutturare una pagina di contenuti"
short-title: "Sfida: Sito di birdwatching"
slug: Learn_web_development/Core/Structuring_content/Structuring_a_page_of_content
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Marking_up_a_letter", "Learn_web_development/Core/Structuring_content/HTML_images", "Learn_web_development/Core/Structuring_content")}}

Strutturare una pagina di contenuti pronta per essere stilizzata utilizzando CSS è una competenza molto importante da padroneggiare, quindi in questa sfida sarai testato sulla tua capacità di pensare a come una pagina potrebbe risultare, e scegliere la semantica strutturale adeguata su cui costruire un layout.

## Punto di partenza

Per iniziare questa sfida, dovresti andare e prendere lo [zip contenente tutte le risorse iniziali](https://raw.githubusercontent.com/mdn/learning-area/main/html/introduction-to-html/structuring-a-page-of-content-start/assets.zip).

Il file zip contiene:

- L'HTML a cui devi aggiungere il markup strutturale.
- CSS per stilizzare il tuo markup.
- Immagini che sono utilizzate nella pagina.

Crea l'esempio sul tuo computer locale, oppure utilizza un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).

> [!NOTE]
> Se ti blocchi, puoi contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Brief del progetto

Per questo progetto, il tuo compito è prendere il contenuto per la homepage di un sito di birdwatching e aggiungere elementi strutturali in modo che possa essere applicato un layout alla pagina. Deve avere:

- Un'intestazione che si estende sull'intera larghezza del sito contenente il titolo principale della pagina, il logo del sito, e il menu di navigazione. Il titolo e il logo appaiono affiancati una volta applicato lo stile, e la navigazione appare sotto questi due elementi.
- Un'area di contenuto principale contenente due colonne — un blocco principale per contenere il testo di benvenuto e una barra laterale per contenere le miniature delle immagini.
- Un piè di pagina contenente informazioni sul diritto d'autore e crediti.

Devi aggiungere un wrapper adatto per:

- L'intestazione
- Il menu di navigazione
- Il contenuto principale
- Il testo di benvenuto
- La barra laterale delle immagini
- Il piè di pagina

Dovresti anche:

- Applicare il CSS fornito alla pagina aggiungendo un altro elemento {{htmlelement("link")}} appena sotto quello esistente fornito all'inizio.

## Suggerimenti e consigli

- Usa il [W3C Nu HTML Checker](https://validator.w3.org/nu/) per individuare errori non intenzionali nel tuo HTML, CSS, e SVG — errori che potresti altrimenti aver trascurato — in modo che tu possa correggerli.
- Non è necessario conoscere alcun CSS per affrontare questa sfida; devi solo inserire il CSS fornito all'interno di un elemento HTML.
- Il CSS fornito è progettato in modo che, quando vengono aggiunti i corretti elementi strutturali al markup, questi appariranno verdi nella pagina renderizzata.
- Se ti trovi bloccato e non riesci a immaginare quali elementi mettere dove, disegna un semplice diagramma a blocchi del layout della pagina e scrivi su di esso gli elementi che pensi dovrebbero avvolgere ogni blocco. Questo è estremamente utile.

## Esempio

La seguente screenshot mostra un esempio di come potrebbe apparire la homepage dopo essere stata marcata.

![L'esempio finito per la sfida; una semplice pagina web sul birdwatching, inclusa un'intestazione "Birdwatching", foto di uccelli, e un messaggio di benvenuto](example-page.png)

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Marking_up_a_letter", "Learn_web_development/Core/Structuring_content/HTML_images", "Learn_web_development/Core/Structuring_content")}}
