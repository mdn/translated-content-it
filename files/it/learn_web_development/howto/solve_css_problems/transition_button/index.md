---
title: Come fare un'animazione di dissolvenza per un pulsante al passaggio del mouse
short-title: Dissolvenza di un pulsante al passaggio del mouse
slug: Learn_web_development/Howto/Solve_CSS_problems/Transition_button
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

In questa guida potrà scoprire come realizzare una transizione graduale tra due colori quando si passa il mouse su un pulsante.

Nel nostro esempio di pulsante, possiamo cambiare lo sfondo del nostro pulsante definendo un colore di sfondo diverso per la pseudo-classe dinamica `:hover`. Tuttavia, passare il mouse sul pulsante farà sì che il colore di sfondo cambi bruscamente al nuovo colore. Per creare un cambiamento più graduale tra i due, possiamo utilizzare le transizioni CSS.

## Utilizzo delle transizioni

Dopo aver aggiunto il colore desiderato per lo stato di hover, aggiunga la proprietà {{cssxref("transition")}} alle regole per il pulsante. Per una transizione semplice, il valore di `transition` è il nome della proprietà o delle proprietà a cui si desidera applicare questa transizione e il tempo che la transizione dovrebbe durare.

Per le pseudo-classi `:active` e `:focus`, la proprietà {{cssxref("transition")}} è impostata su none, in modo che il pulsante passi immediatamente allo stato attivo quando viene cliccato.

Nell'esempio, la transizione dura 1 secondo; può provare a modificare questo valore per vedere la differenza che comporta un cambiamento di velocità.

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
  color: #fff;
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
> La proprietà {{cssxref("transition")}} è una scorciatoia per {{cssxref("transition-delay")}}, {{cssxref("transition-duration")}}, {{cssxref("transition-property")}}, e {{cssxref("transition-timing-function")}}. Veda le pagine di queste proprietà su MDN per trovare modi di modificare le sue transizioni.

## Veda anche

- [Utilizzo delle transizioni CSS](/it/docs/Web/CSS/CSS_transitions/Using_CSS_transitions)
