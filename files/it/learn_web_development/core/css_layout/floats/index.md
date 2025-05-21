---
title: Floats
slug: Learn_web_development/Core/CSS_layout/Floats
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Introduction", "Learn_web_development/Core/CSS_layout/Positioning", "Learn_web_development/Core/CSS_layout")}}

Originariamente pensata per far fluttuare le immagini all'interno di blocchi di testo, la proprietà {{cssxref("float")}} è diventata uno degli strumenti più comunemente utilizzati per creare layout a più colonne sulle pagine web. Con l'avvento di flexbox e grid, è ora tornata al suo scopo originario, come spiega questo articolo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content">Strutturare i contenuti con HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Nozioni di base sullo styling CSS</a>,
        <a href="/it/docs/Learn_web_development/Core/Text_styling/Fundamentals">Fondamenti di stile del testo e dei font</a>,
        familiarità con i <a href="/it/docs/Learn_web_development/Core/CSS_layout/Introduction">concetti fondamentali del layout CSS</a>.
      </td>
    </tr>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere lo scopo dei floats — per far fluttuare immagini all'interno di colonne di testo e altre tecniche come capilettera e riquadri informativi intercalati.</li>
          <li>Capire che i floats venivano utilizzati per layout a più colonne, ma non è più così ora che sono disponibili strumenti migliori.</li>
          <li>Utilizzare la proprietà <code>float</code> per creare floats.</li>
          <li>Eliminare i floats usando <code>clear</code> e il valore <code>display: flow-root</code>.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Background dei floats

La proprietà {{cssxref("float")}} è stata introdotta per permettere agli sviluppatori web di implementare layout che coinvolgono un’immagine fluttuante all’interno di una colonna di testo, con il testo che la avvolge a destra o a sinistra. Il tipo di cosa che si può trovare in un layout di giornale.

Tuttavia, gli sviluppatori web si sono rapidamente resi conto che si può far fluttuare qualsiasi cosa, non solo le immagini, così l'uso del float si è ampliato, ad esempio, per effetti di layout divertenti come i [capilettera](https://css-tricks.com/snippets/css/drop-caps/).

I floats sono stati comunemente usati per creare interi layout di siti web con più colonne di informazioni fluttuanti affinché si trovassero una accanto all'altra (il comportamento predefinito sarebbe che le colonne si trovino una sotto l'altra nello stesso ordine in cui appaiono nel documento sorgente). Ora sono disponibili tecniche di layout più nuove e migliori. Usare i floats in questo modo dovrebbe essere considerato una tecnica legacy.

In questo articolo ci concentreremo solo sugli usi corretti dei floats.

## Un esempio di float

Esploriamo l'uso dei floats. Iniziamo con un esempio che coinvolge il fluttuare di un blocco di testo attorno a un elemento. Puoi seguire creando un nuovo file `index.html` sul tuo computer, riempiendolo con un [template HTML](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/getting-started/index.html) e inserendo il codice seguente nei punti appropriati. In fondo alla sezione, puoi vedere un esempio dal vivo di come dovrebbe apparire il codice finale.

Per prima cosa, inizieremo con un po' di HTML. Aggiungi il seguente codice al corpo HTML, rimuovendo tutto ciò che era al suo interno prima:

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

Ora applica il seguente CSS al tuo HTML (usando un elemento {{htmlelement("style")}} o un {{htmlelement("link")}} a un file `.css` separato — a tua scelta):

```css
body {
  width: 90%;
  max-width: 900px;
  margin: 0 auto;
  font:
    0.9em/1.2 Arial,
    Helvetica,
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

Se salvi e aggiorni, vedrai qualcosa di molto simile a quello che ti aspetteresti: la casella si trova sopra il testo, nel flusso normale.

### Fluttuazione della casella

Per far fluttuare la casella, aggiungi le proprietà {{cssxref("float")}} e {{cssxref("margin-right")}} alla regola `.box`:

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

Ora se salvi e aggiorni vedrai qualcosa di simile al seguente:

{{EmbedLiveSample('Floating_the_box', '100%', 500)}}

Pensiamo a come funziona il float. L'elemento su cui è impostato il float (l'elemento {{htmlelement("div")}} in questo caso) è tolto dal normale flusso di layout del documento e attaccato al lato sinistro del suo contenitore genitore ({{htmlelement("body")}}, in questo caso). Qualsiasi contenuto che sarebbe venuto sotto l'elemento fluttuante nel normale flusso di layout ora si avvolgerà attorno ad esso, riempiendo lo spazio alla sua destra fino al bordo superiore dell'elemento fluttuante. Lì, si fermerà.

Far fluttuare il contenuto a destra ha esattamente lo stesso effetto, ma al contrario: l'elemento fluttuante si attaccherà a destra e il contenuto si avvolgerà attorno a esso a sinistra. Prova a cambiare il valore float in `right` e sostituisci {{cssxref("margin-right")}} con {{cssxref("margin-left")}} nell'ultimo set di regole per vedere quale sarà il risultato.

### Visualizzazione del float

Anche se possiamo aggiungere un margine al float per allontanare il testo, non possiamo aggiungere un margine al testo per allontanarlo dal float. Questo perché un elemento fluttuante è tolto dal flusso normale e le caselle degli elementi successivi si estendono effettivamente dietro il float. Puoi vedere questo effetto apportando alcune modifiche al tuo esempio.

Aggiungi una classe `special` al primo paragrafo di testo, quello immediatamente successivo alla casella fluttuante, quindi nel tuo CSS aggiungi le seguenti regole. Queste daranno al nostro paragrafo successivo un colore di sfondo.

```css
.special {
  background-color: rgb(148 255 172);
  padding: 10px;
  color: purple;
}
```

Per rendere l'effetto più facile da vedere, cambia il `margin-right` sul tuo float in `margin` in modo che ci sia spazio tutto intorno al float. Sarai in grado di vedere il colore di sfondo sul paragrafo che scorre direttamente sotto la casella fluttuante, come nell'esempio sottostante.

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
    0.9em/1.2 Arial,
    Helvetica,
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

Le [line boxes](/it/docs/Web/CSS/CSS_display/Visual_formatting_model#line_boxes) del nostro elemento successivo sono state accorciate in modo che il testo si avvolga attorno al float, ma a causa del float rimosso dal flusso normale, la casella attorno al paragrafo rimane comunque a piena larghezza.

## Eliminare i floats

Abbiamo visto che un float è rimosso dal flusso normale e che altri elementi si visualizzeranno accanto ad esso. Se vogliamo impedire che l'elemento successivo si sposti verso l'alto, dobbiamo _eliminarlo_; questo è realizzato con la proprietà {{cssxref("clear")}}.

Nel tuo HTML dall'esempio precedente, aggiungi una classe `cleared` al secondo paragrafo sotto l'elemento fluttuante. Poi aggiungi il seguente codice CSS:

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
    0.9em/1.2 Arial,
    Helvetica,
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

Dovresti vedere che il secondo paragrafo ora elimina l'elemento fluttuante e non si allinea più accanto ad esso. La proprietà `clear` accetta i seguenti valori:

- `left`: Elimina gli elementi fluttuanti a sinistra.
- `right`: Elimina gli elementi fluttuanti a destra.
- `both`: Elimina qualsiasi elemento fluttuante, a sinistra o a destra.

## Eliminare le caselle attorno a un float

Ora sai come eliminare qualcosa che segue un elemento fluttuante, ma vediamo cosa succede se hai un float alto e un paragrafo corto, con una casella attorno a _entrambi_ gli elementi.

### Il problema

Modifica il tuo documento in modo che il primo paragrafo e la casella fluttuante siano avvolti congiuntamente da un {{htmlelement("div")}}, che ha una classe `wrapper`.

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

Nel tuo CSS, aggiungi la seguente regola per la classe `.wrapper` e poi ricarica la pagina:

```css live-sample___the_problem
.wrapper {
  background-color: rgb(148 255 172);
  padding: 10px;
  color: purple;
}
```

Inoltre, rimuovi la classe `.cleared` originale:

```css
.cleared {
  clear: left;
}
```

Vedrai che, proprio come nell'esempio in cui abbiamo messo un colore di sfondo sul paragrafo, il colore di sfondo scorre dietro il float.

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
    0.9em/1.2 Arial,
    Helvetica,
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

Ancora una volta, questo accade perché il float è stato tolto dal flusso normale. Potresti aspettarti che avvolgendo insieme la casella fluttuante e il testo del primo paragrafo che si avvolge intorno al float, il contenuto successivo sia eliminato dalla casella. Ma non è così.

### display: flow-root

Per risolvere questo problema, si utilizza il valore `flow-root` della proprietà `display`. Questo esiste solo per risolvere questo particolare problema senza usare trucchi — non ci saranno conseguenze indesiderate quando lo usi.

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
    0.9em/1.2 Arial,
    Helvetica,
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

## Metti alla prova le tue abilità!

Sei arrivato alla fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare alcuni ulteriori test per verificare che hai conservato queste informazioni prima di passare oltre — vedi [Testa le tue abilità: Floats](/it/docs/Learn_web_development/Core/CSS_layout/Test_your_skills/Floats).

## Riepilogo

Questo è tutto ciò che devi sapere sui floats. Successivamente, esploreremo il posizionamento in dettaglio.

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Introduction", "Learn_web_development/Core/CSS_layout/Positioning", "Learn_web_development/Core/CSS_layout")}}
