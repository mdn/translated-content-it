---
title: Come fare una scatola semitrasparente
short-title: Rendere una scatola semitrasparente
slug: Learn_web_development/Howto/Solve_CSS_problems/Make_box_transparent
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

Questa guida vi aiuterà a comprendere i modi per rendere una scatola semitrasparente usando CSS.

## Cambiare l'opacità della scatola e del contenuto

Se desidera che la scatola e tutti i contenuti della scatola cambino opacità, allora la proprietà CSS {{cssxref("opacity")}} è lo strumento da usare. L'opacità è l'opposto della trasparenza; pertanto `opacity: 1` è completamente opaco: non vedrà attraverso la scatola per nulla.

Utilizzare un valore di `0` renderebbe la scatola completamente trasparente, e valori intermedi modificheranno l'opacità, con valori più alti che danno meno trasparenza.

## Cambiare l'opacità solo del colore di sfondo

In molti casi si vorrà solo rendere parzialmente trasparente il colore di sfondo stesso, mantenendo il testo e altri elementi completamente opachi. Per ottenere questo, si utilizza un valore [`<color>`](/it/docs/Web/CSS/color_value) che ha un canale alfa, come `rgb()`. Come con `opacity`, un valore di `1` per il canale alfa rende il colore completamente opaco. Pertanto, `background-color: rgb(0 0 0 / 50%);` imposterà il colore di sfondo al 50% di opacità.

Provi a cambiare i valori di opacità e del canale alfa negli esempi qui sotto per vedere più o meno dell'immagine di sfondo dietro la scatola.

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
> Si assicuri che il testo mantenga abbastanza contrasto con lo sfondo nei casi in cui si sovrappone un'immagine; altrimenti potrebbe rendere il contenuto difficile da leggere.

## Vedere anche

- [Applicare il colore agli elementi HTML usando CSS.](/it/docs/Web/CSS/CSS_colors/Applying_color)
