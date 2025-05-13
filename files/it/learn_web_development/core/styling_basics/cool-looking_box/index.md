---
title: "Sfida: Una scatola dall'aspetto fantastico"
short-title: "Sfida: Stili per scatole eleganti"
slug: Learn_web_development/Core/Styling_basics/Cool-looking_box
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Fancy_letterheaded_paper", "Learn_web_development/Core/Text_styling", "Learn_web_development/Core/Styling_basics")}}

In questa valutazione, praticherà ulteriormente la creazione di scatole dall'aspetto accattivante cercando di creare una scatola attraente.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Prima di provare questa valutazione dovrebbe aver già completato
        tutti gli articoli di questo modulo.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Testare la comprensione del modello CSS box e altre caratteristiche
        relative alle scatole, come bordi e sfondi.
      </td>
    </tr>
  </tbody>
</table>

## Punto di partenza

Per iniziare questa valutazione, dovrebbe:

- Salvare l'HTML e il CSS mostrati di seguito come due file separati — `index.html` e `style.css` — in una nuova directory.

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

In alternativa, può utilizzare un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/) o [Glitch](https://glitch.com/).
Può incollare l'HTML e completare il CSS in uno di questi editor online.

> [!NOTE]
> Se si trova in difficoltà, può contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Descrizione del progetto

Il suo compito è quello di creare una scatola elegante e esplorare quanto ci si possa divertire con CSS.

### Compiti generali

- Applicare il CSS all'HTML.

### Stile della scatola

Vorremmo che stilizzasse il {{htmlelement("div")}} fornito, dandogli quanto segue:

- Una larghezza ragionevole per una scatola grande, diciamo circa 200 pixel.
- Un'altezza ragionevole per una scatola grande, centrando verticalmente il testo nel processo.
- Centrare la scatola orizzontalmente.
- Centrare il testo all'interno della scatola.
- Un leggero aumento della dimensione del carattere, a circa 17-18 pixel nello stile calcolato. Utilizzare rem. Scrivere un commento su come ha calcolato il valore.
- Un colore di base per il design. Assegnare questo colore come colore di sfondo della scatola.
- Un colore contrastante per il testo e un'ombra del testo nera.
- Un raggio del bordo abbastanza sottile.
- Un bordo solido di 1 pixel con un colore simile al colore di base, ma con una tonalità leggermente più scura.
- Un gradiente nero lineare semi-trasparente che va verso l'angolo in basso a destra. Renderlo completamente trasparente all'inizio, passando a un'opacità di circa 0.2 entro il 30%, mantenendo lo stesso colore fino alla fine.
- Ombre multiple sulla scatola. Dare una ombra standard per far sembrare che la scatola sia leggermente sollevata dalla pagina. Le altre due dovrebbero essere ombre incassate — un'ombra bianca semi-trasparente vicino all'angolo in alto a sinistra e un'ombra nera semi-trasparente vicino all'angolo in basso a destra — per migliorare l'effetto 3D sollevato della scatola.

## Suggerimenti e consigli

- Utilizzare il [Validator CSS W3C](https://jigsaw.w3.org/css-validator/) per individuare e correggere errori nel suo CSS.

## Esempio

Lo screenshot seguente mostra un esempio di come potrebbe apparire il design finito:

![Un grande riquadro rosso con angoli arrotondati. Il testo bianco con ombra riporta 'questa è una scatola fantastica'.](fancy-box2.png)

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Fancy_letterheaded_paper", "Learn_web_development/Core/Text_styling", "Learn_web_development/Core/Styling_basics")}}
