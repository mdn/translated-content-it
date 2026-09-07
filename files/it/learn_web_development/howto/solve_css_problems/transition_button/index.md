---
title: Come sfumare un pulsante al passaggio del mouse
short-title: Sfumare un pulsante al passaggio del mouse
slug: Learn_web_development/Howto/Solve_CSS_problems/Transition_button
l10n:
  sourceCommit: 85fccefc8066bd49af4ddafc12c77f35265c7e2d
---

In questa guida viene spiegato come creare una delicata sfumatura tra due colori quando si passa il mouse sopra un pulsante.

Nel nostro esempio di pulsante, è possibile modificare lo sfondo del pulsante definendo un colore di sfondo diverso per la pseudo-classe dinamica `:hover`. Tuttavia, passando il mouse sopra il pulsante, `background-color` passerà istantaneamente al nuovo colore. Per creare un cambiamento più delicato tra i due colori, è possibile usare le transizioni CSS.

## Uso delle transizioni

Dopo aver aggiunto il colore desiderato per lo stato al passaggio del mouse, aggiungere la proprietà {{cssxref("transition")}} alle regole del pulsante. Per una transizione semplice, il valore di `transition` è il nome della proprietà o delle proprietà a cui si desidera applicare questa transizione e la durata della transizione.

Per le pseudo-classi `:active` e `:focus`, la proprietà {{cssxref("transition")}} viene impostata su `none`, in modo che il pulsante passi istantaneamente allo stato attivo quando viene fatto clic.

Nell'esempio la transizione dura 1 secondo; è possibile provare a modificare questo valore per osservare la differenza prodotta da un cambiamento di velocità.

```html live-sample___transition-button
<div class="wrapper">
  <button class="fade">Hover over me</button>
</div>
```

```css hidden live-sample___transition-button
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
  cursor: pointer;
}

.fade:focus,
.fade:active {
  background-color: black;
}
```

```css live-sample___transition-button
.fade {
  background-color: #db1f48;
  color: white;
  transition: background-color 1s;
}

.fade:hover {
  background-color: #004369;
}

.fade:focus,
.fade:active {
  background-color: black;
  transition: none;
}
```

{{EmbedLiveSample("transition-button")}}

> [!NOTE]
> La proprietà {{cssxref("transition")}} è una forma abbreviata per {{cssxref("transition-delay")}}, {{cssxref("transition-duration")}}, {{cssxref("transition-property")}} e {{cssxref("transition-timing-function")}}. Consultare le pagine di queste proprietà su MDN per scoprire come regolare le transizioni.

## Vedere anche

- [Uso delle transizioni CSS](/it/docs/Web/CSS/Guides/Transitions/Using)
