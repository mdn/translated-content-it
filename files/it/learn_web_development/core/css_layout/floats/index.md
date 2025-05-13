---
title: Float
slug: Learn_web_development/Core/CSS_layout/Floats
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Introduction", "Learn_web_development/Core/CSS_layout/Positioning", "Learn_web_development/Core/CSS_layout")}}

Originariamente pensata per far galleggiare le immagini all'interno dei blocchi di testo, la proprietà {{cssxref("float")}} è diventata uno degli strumenti più comunemente utilizzati per creare layout a più colonne sulle pagine web. Con l'avvento di flexbox e grid, è tornata al suo scopo originale, come spiega questo articolo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Strutturare il contenuto con HTML</a
        >,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Nozioni di base di CSS Styling</a>,
        <a href="/it/docs/Learn_web_development/Core/Text_styling/Fundamentals">Stili fondamentali per testi e font</a>,
        familiarità con i <a href="/it/docs/Learn_web_development/Core/CSS_layout/Introduction">concetti fondamentali del layout CSS</a>.
      </td>
    </tr>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere lo scopo dei float — per far galleggiare immagini all'interno di colonne di testo e altre tecniche come lettere iniziali in rilievo e box informativi flottanti.</li>
          <li>Comprendere che i float venivano utilizzati per layout a più colonne, ma ora ci sono strumenti migliori disponibili.</li>
          <li>Utilizzare la proprietà <code>float</code> per creare float.</li>
          <li>Chiarire i float usando <code>clear</code> e il valore <code>display: flow-root</code>.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Lo sfondo dei float

La proprietà {{cssxref("float")}} è stata introdotta per permettere agli sviluppatori web di implementare layout che coinvolgono un'immagine galleggiante all'interno di una colonna di testo, con il testo che si dispone attorno a sinistra o destra. Il tipo di cosa che si potrebbe vedere in un layout di quotidiani.

Ma gli sviluppatori web hanno rapidamente capito che si può far galleggiare qualsiasi cosa, non solo immagini, e l'uso del float si è ampliato, per esempio, per effetti di layout divertenti come le [lettere iniziali in rilievo](https://css-tricks.com/snippets/css/drop-caps/).

I float sono stati comunemente utilizzati per creare layout di siti web completi con più colonne di informazioni galleggianti in modo che si dispongano una accanto all'altra (il comportamento predefinito sarebbe che le colonne si dispongano una sotto l'altra nello stesso ordine in cui appaiono nel sorgente). Esistono tecniche di layout più recenti e migliori disponibili. Usare i float in questo modo dovrebbe essere considerato una tecnica legacy.

In questo articolo ci concentreremo solo sugli usi propri dei float.

## Un esempio di float

Esploriamo l'uso dei float. Inizieremo con un esempio che coinvolge un blocco di testo galleggiante attorno a un elemento. Può seguire questo esempio creando un nuovo file `index.html` sul suo computer, riempendolo con un [template HTML](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/getting-started/index.html) e inserendo il codice sottostante nei posti appropriati. In fondo alla sezione, può vedere un esempio dal vivo di come dovrebbe apparire il codice finale.

Per prima cosa, iniziamo con un po' di HTML. Aggiunga il seguente codice al corpo del suo HTML, rimuovendo qualsiasi cosa fosse presente prima:

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

Ora applichi il seguente CSS al suo HTML (usando un elemento {{htmlelement("style")}} o un {{htmlelement("link")}} a un file `.css` separato — a sua scelta):

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

Se salva e aggiorna, vedrà qualcosa di molto simile a ciò che ci si aspetterebbe: il box è sopra il testo, nel flusso normale.

### Far galleggiare il box

Per far galleggiare il box, aggiunga le proprietà {{cssxref("float")}} e {{cssxref("margin-right")}} alla regola `.box`:

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

Ora, se salva e aggiorna, vedrà qualcosa di simile al seguente:

{{EmbedLiveSample('Floating_the_box', '100%', 500)}}

Pensiamo a come funziona il float. L'elemento con il float impostato su di esso (l'elemento {{htmlelement("div")}} in questo caso) è rimosso dal flusso di layout normale del documento e viene attaccato al lato sinistro del suo contenitore genitore ({{htmlelement("body")}}, in questo caso). Qualsiasi contenuto che verrebbe sotto l'elemento flottante nel flusso di layout normale ora si disporrà attorno ad esso invece, riempiendo lo spazio sul lato destro di esso fino in cima all'elemento flottante. Lì, si fermerà.

Far galleggiare il contenuto a destra ha esattamente lo stesso effetto, ma al contrario: l'elemento flottante si attaccherà a destra, e il contenuto si disporrà attorno ad esso a sinistra. Provi a cambiare il valore del float in `right` e sostituire {{cssxref("margin-right")}} con {{cssxref("margin-left")}} nell'ultima regola per vedere quale sia il risultato.

### Visualizzare il float

Mentre possiamo aggiungere un margine al float per spingere il testo lontano, non possiamo aggiungere un margine al testo per allontanarlo dal float. Questo perché un elemento flottante è tolto dal flusso normale e le box degli elementi successivi effettivamente corrono dietro al float. Può vedere questo facendo alcune modifiche al suo esempio.

Aggiunga una classe `special` al primo paragrafo di testo, quello immediatamente successivo al box flottante, quindi nel suo CSS aggiunga le seguenti regole. Queste daranno al nostro paragrafo successivo un colore di sfondo.

```css
.special {
  background-color: rgb(148 255 172);
  padding: 10px;
  color: purple;
}
```

Per rendere l'effetto più facile da vedere, cambi il `margin-right` del suo float in `margin` in modo da avere spazio tutto attorno al float. Sarà in grado di vedere lo sfondo sul paragrafo che corre proprio sotto il box flottante, come nell'esempio sottostante.

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

Le [line boxes](/it/docs/Web/CSS/CSS_display/Visual_formatting_model#line_boxes) del nostro elemento successivo sono state accorciate in modo che il testo si disponga attorno al float, ma a causa del float che è stato rimosso dal flusso normale, la box attorno al paragrafo rimane comunque a piena larghezza.

## Chiarire i float

Abbiamo visto che un float è rimosso dal flusso normale e che gli altri elementi si dispongono accanto ad esso. Se vogliamo fermare l'elemento successivo dal muoversi verso l'alto, dobbiamo _chiarirlo_; questo si ottiene con la proprietà {{cssxref("clear")}}.

Nel suo HTML dell'esempio precedente, aggiunga una classe `cleared` al secondo paragrafo sotto l'elemento flottante. Poi aggiunga il seguente al suo CSS:

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

Dovrebbe vedere che il secondo paragrafo ora chiarisce l'elemento flottante e non si affianca più ad esso. La proprietà `clear` accetta i seguenti valori:

- `left`: Elimina gli elementi fluttuati a sinistra.
- `right`: Elimina gli elementi fluttuati a destra.
- `both`: Elimina qualsiasi elemento fluttuato, a sinistra o a destra.

## Chiarire box attorno a un float

Ora sa come chiarire qualcosa che segue un elemento flottante, ma vediamo cosa succede se ha un float alto e un paragrafo breve, con una box attorno a _entrambi_ gli elementi.

### Il problema

Modifichi il suo documento in modo che il primo paragrafo e il box flottante siano avvolti insieme con un {{htmlelement("div")}}, che ha una classe di `wrapper`.

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

Nel suo CSS, aggiunga la seguente regola per la classe `.wrapper` e poi ricarichi la pagina:

```css live-sample___the_problem
.wrapper {
  background-color: rgb(148 255 172);
  padding: 10px;
  color: purple;
}
```

Inoltre, rimuova la classe originale `.cleared`:

```css
.cleared {
  clear: left;
}
```

Vedrà che, proprio come nell'esempio in cui abbiamo messo un colore di sfondo sul paragrafo, il colore di sfondo corre dietro il float.

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

Ancora una volta, ciò accade perché il float è stato tolto dal flusso normale. Potrebbe aspettarsi che, avvolgendo il box flottante e il testo del primo paragrafo che si dispone attorno al float insieme, il contenuto successivo sarà chiarito dal box. Ma non è questo il caso.

### display: flow-root

Per risolvere questo problema, si può utilizzare il valore `flow-root` della proprietà `display`. Questo esiste solo per risolvere questo particolare problema senza usare hack — non ci saranno conseguenze indesiderate quando lo si utilizza.

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

## Testi le sue abilità!

Ha raggiunto la fine di questo articolo, ma riesce a ricordare le informazioni più importanti? Può trovare ulteriori test per verificare di aver trattenuto queste informazioni prima di procedere — veda [Test your skills: Floats](/it/docs/Learn_web_development/Core/CSS_layout/Test_your_skills/Floats).

## Sommario

Questo è tutto ciò che deve sapere sui float. Successivamente, esploreremo il posizionamento in dettaglio.

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Introduction", "Learn_web_development/Core/CSS_layout/Positioning", "Learn_web_development/Core/CSS_layout")}}
