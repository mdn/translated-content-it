---
title: Come rendere una scatola semi-trasparente
short-title: Rendere una scatola semi-trasparente
slug: Learn_web_development/Howto/Solve_CSS_problems/Make_box_transparent
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

Questa guida aiuterà a capire i modi per rendere una scatola semi-trasparente utilizzando CSS.

## Cambiare l'opacità della scatola e del contenuto

Se desideri che la scatola e tutti i suoi contenuti cambino opacità, allora la proprietà CSS {{cssxref("opacity")}} è lo strumento da utilizzare. L'opacità è l'opposto della trasparenza; pertanto `opacity: 1` è completamente opaco — non vedrai affatto attraverso la scatola.

Usare un valore di `0` renderebbe la scatola completamente trasparente, e valori compresi tra i due cambieranno l'opacità, con valori più alti che conferiscono meno trasparenza.

## Cambiare l'opacità solo del colore di sfondo

In molti casi vorrai rendere trasparente solo il colore di sfondo, mantenendo il testo e altri elementi completamente opachi. Per ottenere ciò, utilizza un valore [`<color>`](/it/docs/Web/CSS/color_value) che abbia un canale alpha, come `rgb()`. Come per l'`opacity`, un valore di `1` per il canale alpha rende il colore completamente opaco. Pertanto, `background-color: rgb(0 0 0 / 50%);` imposterà il colore di sfondo al 50% di opacità.

Prova a cambiare i valori di opacità e del canale alpha negli esempi seguenti per vedere più o meno l'immagine di sfondo dietro la scatola.

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
  border: 5px solid #000;
  border-radius: 0.5em;
  font-size: 140%;
  padding: 20px;
}
```

```css live-sample___opacity
.box1 {
  background-color: #000;
  color: #fff;
  opacity: 0.5;
}

.box2 {
  background-color: rgb(0 0 0 / 0.5);
  color: #fff;
}
```

{{EmbedLiveSample("opacity", "", "280px")}}

> [!NOTE]
> Assicurati che il tuo testo mantenga un contrasto sufficiente con lo sfondo nei casi in cui stai sovrapponendo un'immagine; altrimenti potresti rendere difficile la lettura del contenuto.

## Vedi anche

- [Applicare colori agli elementi HTML utilizzando CSS.](/it/docs/Web/CSS/CSS_colors/Applying_color)
