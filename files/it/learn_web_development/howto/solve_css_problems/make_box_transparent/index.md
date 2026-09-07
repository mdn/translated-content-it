---
title: Come rendere una casella semi-trasparente
short-title: Rendere una casella semi-trasparente
slug: Learn_web_development/Howto/Solve_CSS_problems/Make_box_transparent
l10n:
  sourceCommit: 2b4a2ad5d9ba084a9eaa2f9204102655e7b575c4
---

Questa guida aiuta a comprendere i modi per rendere una casella semi-trasparente usando CSS.

## Modificare l'opacità della casella e del contenuto

Se si desidera modificare l'opacità della casella e di tutto il suo contenuto, la proprietà CSS {{cssxref("opacity")}} è lo strumento da usare. L'opacità è l'opposto della trasparenza; pertanto `opacity: 1` indica una completa opacità: non sarà possibile vedere attraverso la casella.

L'uso del valore `0` renderebbe la casella completamente trasparente, mentre i valori compresi tra i due modificano l'opacità; valori più alti producono una minore trasparenza.

## Modificare solo l'opacità del colore di sfondo

In molti casi si desidera rendere parzialmente trasparente soltanto il colore di sfondo, mantenendo il testo e gli altri elementi completamente opachi. Per ottenere questo risultato, usare un valore {{cssxref("&lt;color&gt;")}} con un canale alfa, come `rgb()`. Come per `opacity`, un valore di `1` per il canale alfa rende il colore completamente opaco. Pertanto, `background-color: rgb(0 0 0 / 50%);` imposta il colore di sfondo al 50% di opacità.

Provare a modificare i valori di opacità e del canale alfa negli esempi seguenti per visualizzare una porzione maggiore o minore dell'immagine di sfondo dietro la casella.

```html live-sample___opacity
<div class="wrapper">
  <div class="box box1">This box uses opacity</div>
  <div class="box box2">
    This box has a background color with an alpha channel
  </div>
</div>
```

```css hidden live-sample___opacity
body {
  font-family: sans-serif;
}

.wrapper {
  height: 200px;
  display: flex;
  gap: 20px;
  background-image: url("https://mdn.github.io/shared-assets/images/examples/balloon.jpg");
  background-repeat: no-repeat;
  background-size: cover;
  padding: 20px;
}

.box {
  flex: 1;
  border: 5px solid black;
  border-radius: 0.5em;
  font-size: 140%;
  padding: 20px;
}
```

```css live-sample___opacity
.box1 {
  background-color: black;
  color: white;
  opacity: 0.5;
}

.box2 {
  background-color: rgb(0 0 0 / 0.5);
  color: white;
}
```

{{EmbedLiveSample("opacity", "", "280px")}}

> [!NOTE]
> Assicurarsi che il testo mantenga un contrasto sufficiente con lo sfondo quando viene sovrapposto a un'immagine; in caso contrario, il contenuto potrebbe risultare difficile da leggere.

## Vedere anche

- [Applicare colori agli elementi HTML usando CSS.](/it/docs/Web/CSS/Guides/Colors/Applying_color)
