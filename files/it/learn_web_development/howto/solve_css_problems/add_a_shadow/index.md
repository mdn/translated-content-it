---
title: Come aggiungere un'ombra a un elemento
short-title: Aggiungere un'ombra a un elemento
slug: Learn_web_development/Howto/Solve_CSS_problems/Add_a_shadow
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

In questa guida puoi scoprire come aggiungere un'ombra a qualsiasi riquadro sulla tua pagina.

## Aggiungere ombre ai riquadri

Le ombre sono una caratteristica comune del design che può aiutare gli elementi a risaltare sulla tua pagina. In CSS, le ombre sui riquadri degli elementi sono create utilizzando la proprietà {{cssxref("box-shadow")}} (se vuoi aggiungere un'ombra al testo stesso, devi usare {{cssxref("text-shadow")}}).

La proprietà `box-shadow` prende un numero di valori:

- L'offset sull'asse x
- L'offset sull'asse y
- Un raggio di sfocatura
- Un raggio di diffusione
- Un colore
- La parola chiave `inset`

Nell'esempio qui sotto abbiamo impostato gli assi X e Y a 5px, la sfocatura a 10px e la diffusione a 2px. Sto usando un nero semi-trasparente come colore. Gioca con i diversi valori per vedere come cambiano l'ombra.

```html live-sample___box-shadow-button
<div class="wrapper">
  <button class="shadow">box-shadow</button>
</div>
```

```css hidden live-sample___box-shadow-button
.wrapper {
  height: 150px;
  display: flex;
  align-items: center;
  justify-content: center;
}

button {
  padding: 5px 10px;
  border: 0;
  border-radius: 5px;
  font-weight: bold;
  font-size: 140%;
  background-color: #db1f48;
  color: #fff;
}
```

```css live-sample___box-shadow-button
.shadow {
  box-shadow: 5px 5px 10px 2px rgb(0 0 0 / 0.8);
}
```

{{EmbedLiveSample("box-shadow-button")}}

> [!NOTE]
> Non stiamo usando `inset` in questo esempio, il che significa che l'ombra è l'ombra esterna predefinita con il riquadro sopra l'ombra. Le ombre inset appaiono all'interno del riquadro come se il contenuto fosse spinto indietro nella pagina.

## Vedi anche

- Il [Generatore di ombre per i riquadri](/it/docs/Web/CSS/CSS_backgrounds_and_borders/Box-shadow_generator)
- [Guida CSS: Effetti di stile avanzati](/it/docs/Learn_web_development/Core/Styling_basics/Advanced_styling_effects)
