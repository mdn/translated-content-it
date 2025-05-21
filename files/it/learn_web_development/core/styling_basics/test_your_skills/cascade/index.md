---
title: "Metti alla prova le tue competenze: Il Cascade"
short-title: Cascade
slug: Learn_web_development/Core/Styling_basics/Test_your_skills/Cascade
l10n:
  sourceCommit: e632b14fd430eac85406827dd5412630cf545e8b
---

Lo scopo di questo test di abilità è valutare se si comprendano i valori delle proprietà universali per [controllare l'ereditarietà in CSS](/it/docs/Learn_web_development/Core/Styling_basics/Handling_conflicts).

> [!NOTE]
> Clicca su **"Play"** nei blocchi di codice qui sotto per modificare gli esempi nel Playground di MDN.
> Puoi anche copiare il codice (clicca sull'icona degli appunti) e incollarlo in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
> Se ti blocchi, puoi contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Attività 1

In questa attività, vogliamo che si utilizzi uno dei valori speciali esaminati nella sezione [controllare l'ereditarietà](/it/docs/Learn_web_development/Core/Styling_basics/Handling_conflicts#controlling_inheritance). Devi scrivere una dichiarazione in una nuova regola che reimposta il colore di sfondo sul bianco, senza usare un valore di colore effettivo.

Il risultato finale dovrebbe apparire come l'immagine qui sotto:

![Collegamenti giallo quasi invisibili su uno sfondo bianco.](mdn-cascade.png)

Prova ad aggiornare il codice qui sotto per ricreare l'esempio finale:

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
<summary>Clicca qui per mostrare la soluzione</summary>

Una possibile soluzione è la seguente:

```css
#outer #inner a {
  background-color: inherit;
}
```

Ci sono due cose che devi fare in questa attività. Prima, scrivi un selettore per l'elemento `a` che sia più specifico del selettore utilizzato per rendere lo sfondo polvere blu. In questa soluzione, questo è ottenuto usando il selettore `id`, che ha una specificità molto alta.

Poi devi ricordare che ci sono valori di parola chiave speciali per tutte le proprietà. In questo caso, usando `inherit` si imposta il colore di sfondo per essere lo stesso dell'elemento padre.

</details>

## Attività 2

In questa attività, vogliamo che si manipoli l'ordine dei livelli del cascade per colorare i collegamenti `rebeccapurple`. Non modificare la dichiarazione `lightgreen`!

Questa attività è un obiettivo avanzato — richiede conoscenze sui livelli del cascade, che non abbiamo coperto nell'articolo [Gestione dei conflitti](/it/docs/Learn_web_development/Core/Styling_basics/Handling_conflicts). È possibile trovare le informazioni necessarie per tentare questa attività in [Livelli del cascade > Determinare la precedenza basata sull'ordine dei livelli](/it/docs/Learn_web_development/Core/Styling_basics/Cascade_layers#determining_the_precedence_based_on_the_order_of_layers).

Il risultato finale dovrebbe apparire come l'immagine qui sotto:

![Collegamenti giallo quasi invisibili su uno sfondo bianco.](mdn-cascade.png)

Prova ad aggiornare il codice qui sotto per ricreare l'esempio finale:

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
<summary>Clicca qui per mostrare la soluzione</summary>

Una possibile soluzione è la seguente:

```css
@layer yellow, green, purple;
```

C'è una cosa che devi fare in questa attività: cambiare l'ordine di precedenza in modo che la dichiarazione per il colore desiderato sia nell'ultimo livello dichiarato, come mostra questa soluzione.

Devi ricordare che gli stili normali non stratificati hanno precedenza sugli stili normali stratificati. Ma, se tutti gli stili sono all'interno di livelli — come nel caso di questa attività — gli stili nei livelli dichiarati più tardi hanno precedenza sugli stili dichiarati nei livelli precedenti. Spostare il livello viola alla fine significa che ha precedenza sui livelli verde e giallo.

</details>

## Vedi anche

- [Nozioni di base sullo styling CSS](/it/docs/Learn_web_development/Core/Styling_basics)
