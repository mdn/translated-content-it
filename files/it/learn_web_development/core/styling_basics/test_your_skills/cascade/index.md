---
title: "Metti alla prova le tue abilità: La cascata"
short-title: Cascade
slug: Learn_web_development/Core/Styling_basics/Test_your_skills/Cascade
l10n:
  sourceCommit: e632b14fd430eac85406827dd5412630cf545e8b
---

Lo scopo di questo test è valutare se comprende i valori universali delle proprietà per il [controllo dell'ereditarietà in CSS](/it/docs/Learn_web_development/Core/Styling_basics/Handling_conflicts).

> [!NOTE]
> Clicchi su **"Play"** nei blocchi di codice qui sotto per modificare gli esempi nel MDN Playground.
> Può anche copiare il codice (clicchi sull'icona della clipboard) e incollarlo in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
> Se si blocca, può contattarci attraverso uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Attività 1

In questa attività, vogliamo che usi uno dei valori speciali discussi nella sezione [controllo dell'ereditarietà](/it/docs/Learn_web_development/Core/Styling_basics/Handling_conflicts#controlling_inheritance). Scriva una dichiarazione in una nuova regola che reimposterà il colore di sfondo su bianco, senza usare un valore di colore reale.

Il suo risultato finale dovrebbe assomigliare all'immagine qui sotto:

![Link gialli appena visibili su uno sfondo bianco.](mdn-cascade.png)

Provi a aggiornare il codice qui sotto per ricreare l'esempio finale:

```html live-sample___cascade
<div class="container" id="outer">
  <div class="container" id="inner">
    <ul>
      <li class="nav"><a href="#">One</a></li>
      <li class="nav"><a href="#">Two</a></li>
    </ul>
  </div>
</div>
```

```css live-sample___cascade
#outer div ul .nav a {
  background-color: powderblue;
  padding: 5px;
  display: inline-block;
  margin-bottom: 10px;
}

div div li a {
  color: rebeccapurple;
}
```

{{EmbedLiveSample("cascade")}}

<details>
<summary>Clicchi qui per mostrare la soluzione</summary>

Una soluzione possibile è la seguente:

```css
#outer #inner a {
  background-color: inherit;
}
```

Ci sono due cose da fare in questa attività. Prima, scriva un selettore per l'elemento `a` che sia più specifico del selettore usato per rendere lo sfondo azzurro polvere. In questa soluzione, ciò è ottenuto utilizzando il selettore `id`, che ha una specificità molto alta.

Poi deve ricordare che ci sono valori di parola chiave speciali per tutte le proprietà. In questo caso, utilizzando `inherit` si imposta il colore dello sfondo per essere lo stesso del suo elemento padre.

</details>

## Attività 2

In questa attività, vogliamo che manipoli l'ordine del livello di cascata per colorare i link `rebeccapurple`. Non modifichi la dichiarazione `lightgreen`!

Questa attività è un obiettivo avanzato — richiede la conoscenza dei livelli di cascata, che non abbiamo trattato nell'articolo [Gestire i conflitti](/it/docs/Learn_web_development/Core/Styling_basics/Handling_conflicts). Può trovare le informazioni necessarie per tentare questa attività in [Livelli di cascata > Determinazione della precedenza basata sull'ordine dei livelli](/it/docs/Learn_web_development/Core/Styling_basics/Cascade_layers#determining_the_precedence_based_on_the_order_of_layers).

Il suo risultato finale dovrebbe assomigliare all'immagine qui sotto:

![Link gialli appena visibili su uno sfondo bianco.](mdn-cascade.png)

Provi a aggiornare il codice qui sotto per ricreare l'esempio finale:

```html live-sample___cascade-layer
<div class="container" id="outer">
  <div class="container" id="inner">
    <ul>
      <li class="nav"><a href="#">One</a></li>
      <li class="nav"><a href="#">Two</a></li>
    </ul>
  </div>
</div>
```

```css live-sample___cascade-layer
@layer yellow, purple, green;

@layer yellow {
  #outer div ul .nav a {
    padding: 5px;
    display: inline-block;
    margin-bottom: 10px;
  }
}
@layer purple {
  div div li a {
    color: rebeccapurple;
  }
}
@layer green {
  a {
    color: lightgreen;
  }
}
```

{{EmbedLiveSample("cascade-layer")}}

<details>
<summary>Clicchi qui per mostrare la soluzione</summary>

Una soluzione possibile è la seguente:

```css
@layer yellow, green, purple;
```

C'è una cosa che deve fare in questa attività: cambiare l'ordine di precedenza in modo che la dichiarazione per il colore desiderato sia nell'ultimo livello dichiarato, come questa soluzione mostra.

Deve ricordare che gli stili normali non stratificati hanno precedenza sugli stili normali stratificati. Ma, se tutti gli stili sono all'interno dei livelli — come nel caso di questa attività — gli stili nei livelli dichiarati successivamente hanno precedenza sugli stili dichiarati nei livelli precedenti. Spostare il livello viola alla fine significa che ha precedenza sui livelli verde e giallo.

</details>

## Vedi anche

- [Nozioni di base sulla stilizzazione CSS](/it/docs/Learn_web_development/Core/Styling_basics)
