---
title: Float
slug: Learn_web_development/Core/CSS_layout/Floats
l10n:
  sourceCommit: 85fccefc8066bd49af4ddafc12c77f35265c7e2d
---

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Introduction", "Learn_web_development/Core/CSS_layout/Test_your_skills/Floats", "Learn_web_development/Core/CSS_layout")}}

Nata originariamente per far fluttuare immagini all'interno di blocchi di testo, la proprietà {{cssxref("float")}} è diventata uno degli strumenti più comunemente usati per creare layout a più colonne nelle pagine web. Con l'avvento di flexbox e grid, è ora tornata al suo scopo originale, come spiega questo articolo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Strutturare i contenuti con HTML</a
        >,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Nozioni di base sullo styling CSS</a>,
        <a href="/it/docs/Learn_web_development/Core/Text_styling/Fundamentals">Nozioni fondamentali sullo stile del testo e dei font</a>,
        familiarità con i <a href="/it/docs/Learn_web_development/Core/CSS_layout/Introduction">concetti fondamentali del layout CSS</a>.
      </td>
    </tr>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere lo scopo dei float: far fluttuare immagini all'interno di colonne di testo e altre tecniche come le iniziali ingrandite e i riquadri informativi fluttuanti inseriti nel testo.</li>
          <li>Comprendere che i float venivano usati per i layout a più colonne, ma che questo non è più necessario ora che sono disponibili strumenti migliori.</li>
          <li>Usare la proprietà <code>float</code> per creare float.</li>
          <li>Cancellare i float usando <code>clear</code> e il valore <code>display: flow-root</code>.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Il contesto dei float

La proprietà {{cssxref("float")}} è stata introdotta per consentire agli sviluppatori web di implementare layout che prevedono un'immagine fluttuante all'interno di una colonna di testo, con il testo che le scorre attorno a sinistra o a destra. Il tipo di impaginazione che si può trovare in un giornale.

Tuttavia, gli sviluppatori web si sono presto resi conto che è possibile far fluttuare qualsiasi elemento, non solo le immagini, quindi l'uso di float si è esteso, per esempio, a effetti di layout creativi come le [iniziali ingrandite](https://css-tricks.com/snippets/css/drop-caps/).

I float sono stati comunemente utilizzati per creare interi layout di siti web con più colonne di informazioni fluttuanti, in modo che siano affiancate (il comportamento predefinito sarebbe quello di disporre le colonne una sotto l'altra nello stesso ordine in cui appaiono nel sorgente). Sono disponibili tecniche di layout più recenti e migliori. L'uso dei float in questo modo dovrebbe essere considerato una tecnica legacy.

In questo articolo ci concentreremo solo sugli usi appropriati dei float.

## Un esempio di float

Esploriamo l'uso dei float. Inizieremo con un esempio che prevede lo scorrimento di un blocco di testo attorno a un elemento. È possibile seguire creando un nuovo file `index.html` sul computer, riempiendolo con un [template HTML](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/getting-started/index.html) e inserendovi il codice seguente nelle posizioni appropriate. Alla fine della sezione, è possibile vedere un esempio live dell'aspetto che dovrebbe avere il codice finale.

Per prima cosa, inizieremo con dell'HTML. Aggiungere quanto segue al body HTML, rimuovendo tutto ciò che era presente prima:

```html
<h1>Float example</h1>

<div class="box">Float</div>

<p>
  Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla luctus aliquam
  dolor, eu lacinia lorem placerat vulputate. Duis felis orci, pulvinar id metus
  ut, rutrum luctus orci. Cras porttitor imperdiet nunc, at ultricies tellus
  laoreet sit amet.
</p>

<p>
  Sed auctor cursus massa at porta. Integer ligula ipsum, tristique sit amet
  orci vel, viverra egestas ligula. Curabitur vehicula tellus neque, ac ornare
  ex malesuada et. In vitae convallis lacus. Aliquam erat volutpat. Suspendisse
  ac imperdiet turpis. Aenean finibus sollicitudin eros pharetra congue. Duis
  ornare egestas augue ut luctus. Proin blandit quam nec lacus varius commodo et
  a urna. Ut id ornare felis, eget fermentum sapien.
</p>

<p>
  Nam vulputate diam nec tempor bibendum. Donec luctus augue eget malesuada
  ultrices. Phasellus turpis est, posuere sit amet dapibus ut, facilisis sed
  est. Nam id risus quis ante semper consectetur eget aliquam lorem. Vivamus
  tristique elit dolor, sed pretium metus suscipit vel. Mauris ultricies lectus
  sed lobortis finibus. Vivamus eu urna eget velit cursus viverra quis
  vestibulum sem. Aliquam tincidunt eget purus in interdum. Cum sociis natoque
  penatibus et magnis dis parturient montes, nascetur ridiculus mus.
</p>
```

Ora applicare il seguente CSS all'HTML (usando un elemento {{htmlelement("style")}} o un {{htmlelement("link")}} a un file `.css` separato: a scelta):

```css
body {
  width: 90%;
  max-width: 900px;
  margin: 0 auto;
  font:
    0.9em/1.2 "Helvetica",
    "Arial",
    sans-serif;
}

.box {
  width: 150px;
  height: 100px;
  border-radius: 5px;
  background-color: rgb(207 232 220);
  padding: 1em;
}
```

Salvando e aggiornando la pagina, verrà visualizzato qualcosa di molto simile a quanto previsto: il riquadro si trova sopra il testo, nel flusso normale.

### Far fluttuare il riquadro

Per far fluttuare il riquadro, aggiungere le proprietà {{cssxref("float")}} e {{cssxref("margin-right")}} alla regola `.box`:

```html hidden
<h1>Float example</h1>

<div class="box">Float</div>

<p>
  Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla luctus aliquam
  dolor, eu lacinia lorem placerat vulputate. Duis felis orci, pulvinar id metus
  ut, rutrum luctus orci. Cras porttitor imperdiet nunc, at ultricies tellus
  laoreet sit amet.
</p>

<p>
  Sed auctor cursus massa at porta. Integer ligula ipsum, tristique sit amet
  orci vel, viverra egestas ligula. Curabitur vehicula tellus neque, ac ornare
  ex malesuada et. In vitae convallis lacus. Aliquam erat volutpat. Suspendisse
  ac imperdiet turpis. Aenean finibus sollicitudin eros pharetra congue. Duis
  ornare egestas augue ut luctus. Proin blandit quam nec lacus varius commodo et
  a urna. Ut id ornare felis, eget fermentum sapien.
</p>

<p>
  Nam vulputate diam nec tempor bibendum. Donec luctus augue eget malesuada
  ultrices. Phasellus turpis est, posuere sit amet dapibus ut, facilisis sed
  est. Nam id risus quis ante semper consectetur eget aliquam lorem. Vivamus
  tristique elit dolor, sed pretium metus suscipit vel. Mauris ultricies lectus
  sed lobortis finibus. Vivamus eu urna eget velit cursus viverra quis
  vestibulum sem. Aliquam tincidunt eget purus in interdum. Cum sociis natoque
  penatibus et magnis dis parturient montes, nascetur ridiculus mus.
</p>
```

```css
.box {
  float: left;
  margin-right: 15px;
  width: 150px;
  height: 100px;
  border-radius: 5px;
  background-color: rgb(207 232 220);
  padding: 1em;
}
```

Salvando e aggiornando la pagina, verrà visualizzato qualcosa di simile al seguente:

{{EmbedLiveSample('Floating_the_box', '100%', 500)}}

Vediamo come funziona il float. L'elemento su cui è impostato il float (l'elemento {{htmlelement("div")}} in questo caso) viene rimosso dal normale flusso di layout del documento e posizionato sul lato sinistro del suo contenitore padre ({{htmlelement("body")}}, in questo caso). Qualsiasi contenuto che nel normale flusso di layout verrebbe dopo l'elemento fluttuante, ora scorrerà invece attorno a esso, riempiendo lo spazio sul suo lato destro fino all'altezza della parte superiore dell'elemento fluttuante. A quel punto si fermerà.

Far fluttuare il contenuto a destra ha esattamente lo stesso effetto, ma al contrario: l'elemento fluttuante si posizionerà a destra e il contenuto gli scorrerà attorno a sinistra. Provare a modificare il valore di float in `right` e a sostituire {{cssxref("margin-right")}} con {{cssxref("margin-left")}} nell'ultima ruleset per vedere il risultato.

### Visualizzare il float

Anche se è possibile aggiungere un margine al float per allontanare il testo, non è possibile aggiungere un margine al testo per allontanarlo dal float. Questo perché un elemento fluttuante viene rimosso dal flusso normale e i riquadri degli elementi successivi si estendono effettivamente dietro il float. È possibile osservarlo apportando alcune modifiche all'esempio.

Aggiungere una classe `special` al primo paragrafo di testo, quello immediatamente successivo al riquadro fluttuante, quindi aggiungere le seguenti regole al CSS. Queste assegneranno un colore di sfondo al paragrafo successivo.

```css
.special {
  background-color: rgb(148 255 172);
  padding: 10px;
  color: purple;
}
```

Per rendere l'effetto più facile da vedere, modificare `margin-right` sul float in `margin`, in modo da ottenere spazio tutto attorno al float. Sarà possibile vedere lo sfondo del paragrafo che passa proprio sotto il riquadro fluttuante, come nell'esempio seguente.

```html hidden
<h1>Float example</h1>

<div class="box">Float</div>

<p class="special">
  Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla luctus aliquam
  dolor, eu lacinia lorem placerat vulputate. Duis felis orci, pulvinar id metus
  ut, rutrum luctus orci. Cras porttitor imperdiet nunc, at ultricies tellus
  laoreet sit amet.
</p>

<p>
  Sed auctor cursus massa at porta. Integer ligula ipsum, tristique sit amet
  orci vel, viverra egestas ligula. Curabitur vehicula tellus neque, ac ornare
  ex malesuada et. In vitae convallis lacus. Aliquam erat volutpat. Suspendisse
  ac imperdiet turpis. Aenean finibus sollicitudin eros pharetra congue. Duis
  ornare egestas augue ut luctus. Proin blandit quam nec lacus varius commodo et
  a urna. Ut id ornare felis, eget fermentum sapien.
</p>

<p>
  Nam vulputate diam nec tempor bibendum. Donec luctus augue eget malesuada
  ultrices. Phasellus turpis est, posuere sit amet dapibus ut, facilisis sed
  est. Nam id risus quis ante semper consectetur eget aliquam lorem. Vivamus
  tristique elit dolor, sed pretium metus suscipit vel. Mauris ultricies lectus
  sed lobortis finibus. Vivamus eu urna eget velit cursus viverra quis
  vestibulum sem. Aliquam tincidunt eget purus in interdum. Cum sociis natoque
  penatibus et magnis dis parturient montes, nascetur ridiculus mus.
</p>
```

```css hidden
body {
  width: 90%;
  max-width: 900px;
  margin: 0 auto;
  font:
    0.9em/1.2 "Helvetica",
    "Arial",
    sans-serif;
}

.box {
  float: left;
  margin: 15px;
  width: 150px;
  height: 150px;
  border-radius: 5px;
  background-color: rgb(207 232 220);
  padding: 1em;
}
```

{{EmbedLiveSample('Visualizing_the_float', '100%', 500)}}

I [line box](/it/docs/Web/CSS/Guides/Display/Visual_formatting_model#line_boxes) dell'elemento successivo sono stati accorciati affinché il testo scorra attorno al float, ma poiché il float è stato rimosso dal flusso normale, il riquadro attorno al paragrafo rimane comunque a larghezza intera.

## Cancellare i float

Abbiamo visto che un float viene rimosso dal flusso normale e che gli altri elementi verranno visualizzati accanto a esso. Se si vuole impedire che l'elemento successivo risalga, è necessario _cancellarlo_; ciò si ottiene con la proprietà {{cssxref("clear")}}.

Nell'HTML dell'esempio precedente, aggiungere una classe `cleared` al secondo paragrafo sotto l'elemento fluttuante. Quindi aggiungere quanto segue al CSS:

```css
.cleared {
  clear: left;
}
```

```html hidden
<h1>Float example</h1>

<div class="box">Float</div>

<p class="special">
  Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla luctus aliquam
  dolor, eu lacinia lorem placerat vulputate. Duis felis orci, pulvinar id metus
  ut, rutrum luctus orci. Cras porttitor imperdiet nunc, at ultricies tellus
  laoreet sit amet.
</p>

<p class="cleared">
  Sed auctor cursus massa at porta. Integer ligula ipsum, tristique sit amet
  orci vel, viverra egestas ligula. Curabitur vehicula tellus neque, ac ornare
  ex malesuada et. In vitae convallis lacus. Aliquam erat volutpat. Suspendisse
  ac imperdiet turpis. Aenean finibus sollicitudin eros pharetra congue. Duis
  ornare egestas augue ut luctus. Proin blandit quam nec lacus varius commodo et
  a urna. Ut id ornare felis, eget fermentum sapien.
</p>

<p>
  Nam vulputate diam nec tempor bibendum. Donec luctus augue eget malesuada
  ultrices. Phasellus turpis est, posuere sit amet dapibus ut, facilisis sed
  est. Nam id risus quis ante semper consectetur eget aliquam lorem. Vivamus
  tristique elit dolor, sed pretium metus suscipit vel. Mauris ultricies lectus
  sed lobortis finibus. Vivamus eu urna eget velit cursus viverra quis
  vestibulum sem. Aliquam tincidunt eget purus in interdum. Cum sociis natoque
  penatibus et magnis dis parturient montes, nascetur ridiculus mus.
</p>
```

```css hidden
body {
  width: 90%;
  max-width: 900px;
  margin: 0 auto;
  font:
    0.9em/1.2 "Helvetica",
    "Arial",
    sans-serif;
}

.box {
  float: left;
  margin: 15px;
  width: 150px;
  height: 150px;
  border-radius: 5px;
  background-color: rgb(207 232 220);
  padding: 1em;
}

.special {
  background-color: rgb(148 255 172);
  padding: 10px;
  color: purple;
}

.cleared {
  clear: left;
}
```

{{EmbedLiveSample('Clearing_floats', '100%', 600)}}

Si dovrebbe vedere che il secondo paragrafo ora cancella l'elemento fluttuante e non si posiziona più accanto a esso. La proprietà `clear` accetta i seguenti valori:

- `left`: cancella gli elementi fluttuanti a sinistra.
- `right`: cancella gli elementi fluttuanti a destra.
- `both`: cancella tutti gli elementi fluttuanti, a sinistra o a destra.

## Cancellare i riquadri che racchiudono un float

Ora è noto come cancellare un elemento che segue un elemento fluttuante, ma vediamo cosa succede se si ha un float alto e un paragrafo breve, con un riquadro che contiene _entrambi_ gli elementi.

### Il problema

Modificare il documento in modo che il primo paragrafo e il riquadro fluttuante siano racchiusi insieme in un {{htmlelement("div")}}, che ha una classe `wrapper`.

```html live-sample___the_problem
<div class="wrapper">
  <div class="box">Float1</div>

  <p>
    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla luctus
    aliquam dolor, eu lacinia lorem placerat vulputate. Duis felis orci,
    pulvinar id metus ut, rutrum luctus orci. Cras porttitor imperdiet nunc, at
    ultricies tellus laoreet sit amet.
  </p>
</div>
```

Nel CSS, aggiungere la seguente regola per la classe `.wrapper`, quindi ricaricare la pagina:

```css live-sample___the_problem
.wrapper {
  background-color: rgb(148 255 172);
  padding: 10px;
  color: purple;
}
```

Inoltre, rimuovere la classe `.cleared` originale:

```css
.cleared {
  clear: left;
}
```

Si vedrà che, proprio come nell'esempio in cui è stato applicato un colore di sfondo al paragrafo, il colore di sfondo passa dietro il float.

```html hidden live-sample___the_problem
<p>
  Sed auctor cursus massa at porta. Integer ligula ipsum, tristique sit amet
  orci vel, viverra egestas ligula. Curabitur vehicula tellus neque, ac ornare
  ex malesuada et. In vitae convallis lacus. Aliquam erat volutpat. Suspendisse
  ac imperdiet turpis. Aenean finibus sollicitudin eros pharetra congue. Duis
  ornare egestas augue ut luctus. Proin blandit quam nec lacus varius commodo et
  a urna. Ut id ornare felis, eget fermentum sapien.
</p>

<p>
  Nam vulputate diam nec tempor bibendum. Donec luctus augue eget malesuada
  ultrices. Phasellus turpis est, posuere sit amet dapibus ut, facilisis sed
  est. Nam id risus quis ante semper consectetur eget aliquam lorem. Vivamus
  tristique elit dolor, sed pretium metus suscipit vel. Mauris ultricies lectus
  sed lobortis finibus. Vivamus eu urna eget velit cursus viverra quis
  vestibulum sem. Aliquam tincidunt eget purus in interdum. Cum sociis natoque
  penatibus et magnis dis parturient montes, nascetur ridiculus mus.
</p>
```

```css hidden live-sample___the_problem
body {
  width: 90%;
  max-width: 900px;
  margin: 0 auto;
  font:
    0.9em/1.2 "Helvetica",
    "Arial",
    sans-serif;
}

.box {
  float: left;
  margin: 15px;
  width: 150px;
  height: 150px;
  border-radius: 5px;
  background-color: rgb(207 232 220);
  padding: 1em;
  color: black;
}
```

{{EmbedLiveSample('the_problem', '100%', 600)}}

Ancora una volta, ciò accade perché il float è stato rimosso dal flusso normale. Si potrebbe prevedere che, racchiudendo insieme il riquadro fluttuante e il testo del primo paragrafo che scorre attorno al float, il contenuto successivo venga separato dal riquadro. Ma non è così.

### display: flow-root

Per risolvere questo problema, usare il valore `flow-root` della proprietà `display`. Questo valore esiste esclusivamente per risolvere questo particolare problema senza usare hack: il suo utilizzo non produrrà conseguenze indesiderate.

```css
.wrapper {
  background-color: rgb(148 255 172);
  padding: 10px;
  color: purple;
  display: flow-root;
}
```

```html hidden
<h1>Float example</h1>
<div class="wrapper">
  <div class="box">Float</div>

  <p>
    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla luctus
    aliquam dolor, eu lacinia lorem placerat vulputate. Duis felis orci,
    pulvinar id metus ut, rutrum luctus orci. Cras porttitor imperdiet nunc, at
    ultricies tellus laoreet sit amet.
  </p>
</div>
<p class="cleared">
  Sed auctor cursus massa at porta. Integer ligula ipsum, tristique sit amet
  orci vel, viverra egestas ligula. Curabitur vehicula tellus neque, ac ornare
  ex malesuada et. In vitae convallis lacus. Aliquam erat volutpat. Suspendisse
  ac imperdiet turpis. Aenean finibus sollicitudin eros pharetra congue. Duis
  ornare egestas augue ut luctus. Proin blandit quam nec lacus varius commodo et
  a urna. Ut id ornare felis, eget fermentum sapien.
</p>

<p>
  Nam vulputate diam nec tempor bibendum. Donec luctus augue eget malesuada
  ultrices. Phasellus turpis est, posuere sit amet dapibus ut, facilisis sed
  est. Nam id risus quis ante semper consectetur eget aliquam lorem. Vivamus
  tristique elit dolor, sed pretium metus suscipit vel. Mauris ultricies lectus
  sed lobortis finibus. Vivamus eu urna eget velit cursus viverra quis
  vestibulum sem. Aliquam tincidunt eget purus in interdum. Cum sociis natoque
  penatibus et magnis dis parturient montes, nascetur ridiculus mus.
</p>
```

```css hidden
body {
  width: 90%;
  max-width: 900px;
  margin: 0 auto;
  font:
    0.9em/1.2 "Helvetica",
    "Arial",
    sans-serif;
}

.box {
  float: left;
  margin: 15px;
  width: 150px;
  height: 150px;
  border-radius: 5px;
  background-color: rgb(207 232 220);
  padding: 1em;
  color: black;
}
```

{{EmbedLiveSample('display_flow-root', '100%', 600)}}

## Riepilogo

Questo è tutto ciò che occorre sapere sui float. Nel prossimo articolo verranno proposti alcuni test che permetteranno di verificare quanto bene siano state comprese e memorizzate tutte queste informazioni.

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Introduction", "Learn_web_development/Core/CSS_layout/Test_your_skills/Floats", "Learn_web_development/Core/CSS_layout")}}
