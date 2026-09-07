---
title: Come aggiungere un'ombra a un elemento
short-title: Aggiungere un'ombra a un elemento
slug: Learn_web_development/Howto/Solve_CSS_problems/Add_a_shadow
l10n:
  sourceCommit: 85fccefc8066bd49af4ddafc12c77f35265c7e2d
---

In questa guida è possibile scoprire come aggiungere un'ombra a qualsiasi riquadro della pagina.

## Aggiungere ombre ai riquadri

Le ombre sono una caratteristica di progettazione comune che può aiutare gli elementi a risaltare nella pagina. In CSS, le ombre sui riquadri degli elementi vengono create utilizzando la proprietà {{cssxref("box-shadow")}} (per aggiungere un'ombra al testo stesso, è necessario usare {{cssxref("text-shadow")}}).

La proprietà `box-shadow` accetta diversi valori:

- Lo spostamento sull'asse x
- Lo spostamento sull'asse y
- Un raggio di sfocatura
- Un raggio di espansione
- Un colore
- La parola chiave `inset`

Nell'esempio seguente, gli assi X e Y sono impostati a 5px, la sfocatura a 10px e l'espansione a 2px. Come colore viene utilizzato un nero semitrasparente. È possibile provare i diversi valori per osservare come modificano l'ombra.

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
  color: white;
}
```

```css live-sample___box-shadow-button
.shadow {
  box-shadow: 5px 5px 10px 2px rgb(0 0 0 / 0.8);
}
```

{{EmbedLiveSample("box-shadow-button")}}

> [!NOTE]
> In questo esempio non viene utilizzato `inset`; ciò significa che l'ombra è la drop shadow predefinita, con il riquadro sopra l'ombra. Le ombre inset appaiono all'interno del riquadro, come se il contenuto fosse spinto indietro nella pagina.

## Vedi anche

- Il [generatore di box shadow](/it/docs/Web/CSS/Guides/Backgrounds_and_borders/Box-shadow_generator)
- [Imparare CSS: effetti di stile avanzati](/it/docs/Learn_web_development/Core/Styling_basics/Advanced_styling_effects)
