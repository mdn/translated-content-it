---
title: Come aggiungere un'ombra a un elemento
short-title: Aggiungere un'ombra a un elemento
slug: Learn_web_development/Howto/Solve_CSS_problems/Add_a_shadow
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

In questa guida può scoprire come aggiungere un'ombra a qualsiasi riquadro sulla sua pagina.

## Aggiungere ombre ai riquadri

Le ombre sono una caratteristica di design comune che può aiutare gli elementi a risaltare sulla sua pagina. In CSS, le ombre sui riquadri degli elementi sono create utilizzando la proprietà {{cssxref("box-shadow")}} (se vuole aggiungere un'ombra al testo stesso, deve utilizzare {{cssxref("text-shadow")}}).

La proprietà `box-shadow` accetta diversi valori:

- L'offset sull'asse x
- L'offset sull'asse y
- Un raggio di sfocatura
- Un raggio di espansione
- Un colore
- La keyword `inset`

Nell'esempio qui sotto abbiamo impostato gli assi X e Y a 5px, la sfocatura a 10px e l'espansione a 2px. Sto usando un nero semitrasparente come colore. Sperimenta con i diversi valori per vedere come cambiano l'ombra.

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
> In questo esempio non stiamo utilizzando `inset`, il che significa che l'ombra è l'ombra esterna predefinita con il riquadro sopra l'ombra. Le ombre interne appaiono all'interno del riquadro come se il contenuto fosse spinto indietro nella pagina.

## Vedi anche

- Il [Generatore di ombre per riquadri](/it/docs/Web/CSS/CSS_backgrounds_and_borders/Box-shadow_generator)
- [Imparare CSS: effetti di stile avanzati](/it/docs/Learn_web_development/Core/Styling_basics/Advanced_styling_effects)
