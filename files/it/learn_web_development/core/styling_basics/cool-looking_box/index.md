---
title: "Sfida: Una scatola dall'aspetto interessante"
short-title: "Sfida: Stili per scatole eleganti"
slug: Learn_web_development/Core/Styling_basics/Cool-looking_box
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Fancy_letterheaded_paper", "Learn_web_development/Core/Text_styling", "Learn_web_development/Core/Styling_basics")}}

In questa valutazione, avrai l'opportunità di esercitarti ulteriormente nella creazione di scatole interessanti cercando di creare una scatola accattivante.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Prima di affrontare questa valutazione, dovresti aver già completato tutti gli articoli in questo modulo.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Verificare la comprensione del modello a scatola CSS e di altre caratteristiche relative alle scatole come bordi e sfondi.
      </td>
    </tr>
  </tbody>
</table>

## Punto di partenza

Per iniziare questa valutazione, dovresti:

- Salvare l'HTML e il CSS mostrati qui sotto come due file separati — `index.html` e `style.css` — in una nuova directory.

### HTML

```html
<!DOCTYPE html>
<html lang="en-US">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width" />
    <title>Cool box</title>
    <!-- your css link goes here -->
  </head>
  <body>
    <div>This is a cool box</div>
  </body>
</html>
```

### CSS

```css
html {
  font-family: sans-serif;
}

/* Your CSS below here */
```

In alternativa, potresti utilizzare un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/) o [Glitch](https://glitch.com/).
Potresti incollare l'HTML e riempiere il CSS in uno di questi editor online.

> [!NOTE]
> Se ti blocchi, puoi contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Descrizione del progetto

Il tuo compito è creare una scatola elegante e esplorare il divertimento che possiamo avere con il CSS.

### Compiti generali

- Applica il CSS all'HTML.

### Stilizzazione della scatola

Vorremmo che tu stilizzi il {{htmlelement("div")}} fornito, dandogli le seguenti caratteristiche:

- Una larghezza ragionevole per una scatola grande, diciamo circa 200 pixel.
- Un'altezza ragionevole per una scatola grande, centrando verticalmente il testo nel processo.
- Centrare la scatola orizzontalmente.
- Centrare il testo all'interno della scatola.
- Un leggero aumento della dimensione del font, a circa 17-18 pixel come stile calcolato. Utilizza i rem. Scrivi un commento su come hai calcolato il valore.
- Un colore base per il design. Dai alla scatola questo colore come colore di sfondo.
- Un colore contrastante per il testo e un'ombra di testo nera.
- Un raggio del bordo abbastanza sottile.
- Un bordo solido di 1 pixel con un colore simile al colore base, ma di una tonalità leggermente più scura.
- Un gradiente trasparente nero lineare verso l'angolo in basso a destra. Rendilo completamente trasparente all'inizio, gradevole fino a circa 0,2 di opacità al 30% lungo il percorso, e rimanendo dello stesso colore fino alla fine.
- Ombre multiple per le scatole. Dagli un'ombra standard per far sembrare la scatola leggermente sollevata dalla pagina. Le altre due dovrebbero essere ombre interne — un'ombra bianca semi-trasparente vicino all'angolo in alto a sinistra e un'ombra nera semi-trasparente vicino all'angolo in basso a destra — per aggiungere al bel look 3D rialzato della scatola.

## Suggerimenti e consigli

- Utilizza il [W3C CSS Validator](https://jigsaw.w3.org/css-validator/) per individuare e correggere gli errori nel tuo CSS.

## Esempio

Lo screenshot seguente mostra un esempio di come potrebbe apparire il design finale:

![Una grande scatola rossa con angoli arrotondati. Il testo bianco con ombra riporta 'questa è una scatola interessante'.](fancy-box2.png)

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Fancy_letterheaded_paper", "Learn_web_development/Core/Text_styling", "Learn_web_development/Core/Styling_basics")}}
