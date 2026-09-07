---
title: "Metti alla prova le tue competenze: la cascata"
short-title: "Test: la cascata"
slug: Learn_web_development/Core/Styling_basics/Test_your_skills/Cascade
l10n:
  sourceCommit: a623d4459e2aa00d17dc0fd6b6bc44f56c589950
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Handling_conflicts", "Learn_web_development/Core/Styling_basics/Fixing_blog_styles", "Learn_web_development/Core/Styling_basics")}}

L'obiettivo di questo test sulle competenze è aiutare a valutare se si comprendono i valori universali delle proprietà per [controllare l'ereditarietà in CSS](/it/docs/Learn_web_development/Core/Styling_basics/Handling_conflicts).

> [!NOTE]
> Per ottenere aiuto, leggere la nostra guida all'uso di [Metti alla prova le tue competenze](/it/docs/Learn_web_development#test_your_skills). È anche possibile contattarci attraverso uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Cascata 1

In questa attività, si deve usare uno dei valori speciali esaminati nella sezione [controllare l'ereditarietà](/it/docs/Learn_web_development/Core/Styling_basics/Handling_conflicts#controlling_inheritance).

Per completare l'attività, scrivere una dichiarazione in una nuova regola che reimposti il colore di sfondo su bianco, senza usare un valore di colore effettivo.

Il punto di partenza dell'attività è il seguente:

{{EmbedLiveSample("cascade1-start", "100%", "110px")}}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___cascade1-start live-sample___cascade1-finish
<div class="container" id="outer">
  <div class="container" id="inner">
    <ul>
      <li class="nav"><a href="#">One</a></li>
      <li class="nav"><a href="#">Two</a></li>
    </ul>
  </div>
</div>
```

```css live-sample___cascade1-start live-sample___cascade1-finish
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

Lo stile aggiornato dovrebbe apparire così:

{{EmbedLiveSample("cascade1-finish", "100%", "110px")}}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Una possibile soluzione è la seguente:

```css live-sample___cascade1-finish
#outer #inner a {
  background-color: inherit;
}
```

In questa attività ci sono due cose da fare. Innanzitutto, scrivere un selettore per l'elemento `a` che sia più specifico del selettore usato per impostare lo sfondo su powderblue. In questa soluzione, ciò viene ottenuto usando il selettore `id`, che ha una specificità molto elevata.

Quindi occorre ricordare che esistono valori di parole chiave speciali per tutte le proprietà. In questo caso, l'uso di `inherit` imposta nuovamente il colore di sfondo affinché sia uguale a quello dell'elemento genitore.

</details>

## Cascata 2

Per completare questa attività, manipolare l'ordine dei layer della cascata per colorare i link `rebeccapurple`. Non modificare la dichiarazione `lightgreen`!

Questa attività è un obiettivo aggiuntivo: richiede la conoscenza dei layer della cascata, che non sono stati trattati nell'articolo [Gestire i conflitti](/it/docs/Learn_web_development/Core/Styling_basics/Handling_conflicts). Le informazioni necessarie per tentare questa attività sono disponibili in [Layer della cascata > Determinare la precedenza in base all'ordine dei layer](/it/docs/Learn_web_development/Core/Styling_basics/Cascade_layers#determining_the_precedence_based_on_the_order_of_layers).

Il punto di partenza dell'attività è il seguente:

{{EmbedLiveSample("cascade2-start", "100%", "110px")}}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___cascade2-start live-sample___cascade2-finish
<div class="container" id="outer">
  <div class="container" id="inner">
    <ul>
      <li class="nav"><a href="#">One</a></li>
      <li class="nav"><a href="#">Two</a></li>
    </ul>
  </div>
</div>
```

```css live-sample___cascade2-start
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

Lo stile aggiornato dovrebbe apparire così:

{{EmbedLiveSample("cascade2-finish", "100%", "110px")}}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Una possibile soluzione è la seguente:

```css live-sample___cascade2-finish
@layer yellow, green, purple;
```

```css hidden live-sample___cascade2-finish
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

In questa attività c'è una cosa da fare: modificare l'ordine di precedenza affinché la dichiarazione per il colore desiderato si trovi nell'ultimo layer dichiarato, come mostra questa soluzione.

Occorre ricordare che gli stili normali senza layer hanno precedenza sugli stili normali nei layer. Tuttavia, se tutti gli stili sono all'interno di layer, come nel caso di questa attività, gli stili nei layer dichiarati successivamente hanno precedenza sugli stili dichiarati nei layer precedenti. Spostare il layer viola alla fine significa che ha precedenza sui layer verde e giallo.

</details>

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Handling_conflicts", "Learn_web_development/Core/Styling_basics/Fixing_blog_styles", "Learn_web_development/Core/Styling_basics")}}
