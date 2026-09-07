---
title: Layout a colonne multiple
slug: Learn_web_development/Core/CSS_layout/Multiple-column_Layout
l10n:
  sourceCommit: 1b7c3c1e03f14c3878e4d8518b0f1a89bedfdc9c
---

La specifica per il layout a colonne multiple fornisce un metodo per disporre i contenuti in colonne, come in un giornale. Questo articolo spiega come utilizzare questa funzionalità.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Nozioni di base di HTML (studiare
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Strutturare contenuti con HTML</a
        >) e un'idea di come funziona CSS (studiare
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Nozioni di base sullo stile CSS</a>.)
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare a creare layout a colonne multiple nelle pagine web, come quelli
        che si possono trovare in un giornale.
      </td>
    </tr>
  </tbody>
</table>

## Un esempio di base

Esploriamo come utilizzare il layout a colonne multiple — spesso chiamato _multicol_ — costruendo un esempio passo dopo passo. Per seguire, creare un nuovo file HTML sul sistema locale e aggiungervi il seguente contenuto:

```html
<!DOCTYPE html>
<html lang="en-US">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width" />
    <title>Multicol example</title>
    <style>
      body {
        width: 90%;
        max-width: 900px;
        margin: 2em auto;
        font:
          0.9em/1.2 Arial,
          Helvetica,
          sans-serif;
      }
    </style>
  </head>

  <body>
    <div class="container">
      <h1>Simple multicol example</h1>

      <p>
        Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla luctus
        aliquam dolor, eu lacinia lorem placerat vulputate. Duis felis orci,
        pulvinar id metus ut, rutrum luctus orci. Cras porttitor imperdiet nunc,
        at ultricies tellus laoreet sit amet. Sed auctor cursus massa at porta.
        Integer ligula ipsum, tristique sit amet orci vel, viverra egestas
        ligula. Curabitur vehicula tellus neque, ac ornare ex malesuada et. In
        vitae convallis lacus. Aliquam erat volutpat. Suspendisse ac imperdiet
        turpis. Aenean finibus sollicitudin eros pharetra congue. Duis ornare
        egestas augue ut luctus. Proin blandit quam nec lacus varius commodo et
        a urna. Ut id ornare felis, eget fermentum sapien.
      </p>

      <p>
        Nam vulputate diam nec tempor bibendum. Donec luctus augue eget
        malesuada ultrices. Phasellus turpis est, posuere sit amet dapibus ut,
        facilisis sed est. Nam id risus quis ante semper consectetur eget
        aliquam lorem. Vivamus tristique elit dolor, sed pretium metus suscipit
        vel. Mauris ultricies lectus sed lobortis finibus. Vivamus eu urna eget
        velit cursus viverra quis vestibulum sem. Aliquam tincidunt eget purus
        in interdum. Cum sociis natoque penatibus et magnis dis parturient
        montes, nascetur ridiculus mus.
      </p>
    </div>
  </body>
</html>
```

Di seguito sono disponibili vari esempi live che mostrano come dovrebbe apparire l'output renderizzato in ogni fase.

### Un layout a tre colonne

Il file di partenza contiene HTML molto semplice: un wrapper con una classe `container`, al cui interno si trovano un'intestazione e alcuni paragrafi.

L'elemento {{htmlelement("div")}} con una classe container diventerà il contenitore multicol. Si abilita multicol utilizzando una delle due proprietà: {{cssxref("column-count")}} o {{cssxref("column-width")}}. La proprietà `column-count` accetta un numero come valore e crea quel numero di colonne. Se si aggiunge il seguente CSS al foglio di stile e si ricarica la pagina, si otterranno tre colonne:

```css live-sample___column-count
.container {
  column-count: 3;
}
```

Le colonne create hanno larghezze flessibili: il browser calcola quanto spazio assegnare a ciascuna colonna.

```css hidden live-sample___column-count live-sample___column-width live-sample___column-styling live-sample___column-spanning
body {
  width: 90%;
  max-width: 900px;
  margin: 2em auto;
  font:
    0.9em/1.2 "Helvetica",
    "Arial",
    sans-serif;
}
```

```html hidden live-sample___column-count live-sample___column-width live-sample___column-styling
<div class="container">
  <h1>Simple multicol example</h1>

  <p>
    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla luctus
    aliquam dolor, eu lacinia lorem placerat vulputate. Duis felis orci,
    pulvinar id metus ut, rutrum luctus orci. Cras porttitor imperdiet nunc, at
    ultricies tellus laoreet sit amet. Sed auctor cursus massa at porta. Integer
    ligula ipsum, tristique sit amet orci vel, viverra egestas ligula. Curabitur
    vehicula tellus neque, ac ornare ex malesuada et. In vitae convallis lacus.
    Aliquam erat volutpat. Suspendisse ac imperdiet turpis. Aenean finibus
    sollicitudin eros pharetra congue. Duis ornare egestas augue ut luctus.
    Proin blandit quam nec lacus varius commodo et a urna. Ut id ornare felis,
    eget fermentum sapien.
  </p>

  <p>
    Nam vulputate diam nec tempor bibendum. Donec luctus augue eget malesuada
    ultrices. Phasellus turpis est, posuere sit amet dapibus ut, facilisis sed
    est. Nam id risus quis ante semper consectetur eget aliquam lorem. Vivamus
    tristique elit dolor, sed pretium metus suscipit vel. Mauris ultricies
    lectus sed lobortis finibus. Vivamus eu urna eget velit cursus viverra quis
    vestibulum sem. Aliquam tincidunt eget purus in interdum. Cum sociis natoque
    penatibus et magnis dis parturient montes, nascetur ridiculus mus.
  </p>
</div>
```

{{ EmbedLiveSample('column-count', '100%', 400) }}

### Impostare column-width

Modificare il CSS per utilizzare `column-width` come segue:

```css live-sample___column-width
.container {
  column-width: 200px;
}
```

Il browser ora creerà il maggior numero possibile di colonne della dimensione specificata; lo spazio rimanente verrà quindi condiviso tra le colonne esistenti. Ciò significa che non si otterrà esattamente la larghezza specificata, a meno che il contenitore non sia esattamente divisibile per tale larghezza.

{{ EmbedLiveSample('column-width', '100%', 400) }}

## Applicare stili alle colonne

Le colonne create da multicol non possono essere stilizzate singolarmente. Non è possibile rendere una colonna più grande delle altre o modificare il colore di sfondo o del testo di una singola colonna. Esistono due possibilità per modificare la visualizzazione delle colonne:

- Modificare la dimensione dello spazio tra le colonne utilizzando {{cssxref("column-gap")}}.
- Aggiungere una linea tra le colonne con {{cssxref("column-rule")}}.

Utilizzando l'esempio precedente, modificare la dimensione dello spazio aggiungendo una proprietà `column-gap`. È possibile sperimentare valori diversi: la proprietà accetta qualsiasi unità di lunghezza.

Ora aggiungere una linea tra le colonne con `column-rule`. In modo simile alla proprietà {{cssxref("border")}} incontrata nelle lezioni precedenti, `column-rule` è una scorciatoia per {{cssxref("column-rule-color")}}, {{cssxref("column-rule-style")}} e {{cssxref("column-rule-width")}}, e accetta gli stessi valori di `border`.

```css live-sample___column-styling live-sample___column-spanning
.container {
  column-count: 3;
  column-gap: 20px;
  column-rule: 4px dotted rgb(79 185 227);
}
```

Provare ad aggiungere linee di stili e colori diversi.

{{ EmbedLiveSample('column-styling', '100%', 400) }}

Un aspetto da notare è che la linea non occupa una larghezza propria. Si trova nello spazio creato con `column-gap`. Per aumentare lo spazio su entrambi i lati della linea, sarà necessario incrementare la dimensione di `column-gap`.

## Estendere gli elementi sulle colonne

È possibile fare in modo che un elemento si estenda su tutte le colonne. In questo caso, il contenuto si interrompe nel punto in cui viene introdotto l'elemento esteso e poi continua sotto l'elemento, creando un nuovo insieme di colonne. Per fare in modo che un elemento si estenda su tutte le colonne, specificare il valore `all` per la proprietà {{cssxref("column-span")}}.

> [!NOTE]
> Non è possibile fare in modo che un elemento si estenda solo su _alcune_ colonne. La proprietà può avere solo i valori `none` (che è il valore predefinito) o `all`.

Aggiungere la seguente regola al CSS, sotto le precedenti:

```css live-sample___column-spanning
h2 {
  column-span: all;
  background-color: rgb(79 185 227);
  color: white;
  padding: 0.5em;
}
```

Ora aggiungere un'intestazione di secondo livello tra il primo e il secondo paragrafo:

```html
<h2>Spanning subhead</h2>
```

```html hidden live-sample___column-spanning
<div class="container">
  <h1>Simple multicol example</h1>

  <p>
    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla luctus
    aliquam dolor, eu lacinia lorem placerat vulputate. Duis felis orci,
    pulvinar id metus ut, rutrum luctus orci. Cras porttitor imperdiet nunc, at
    ultricies tellus laoreet sit amet. Sed auctor cursus massa at porta. Integer
    ligula ipsum, tristique sit amet orci vel, viverra egestas ligula.
  </p>

  <h2>Spanning subhead</h2>

  <p>
    Curabitur vehicula tellus neque, ac ornare ex malesuada et. In vitae
    convallis lacus. Aliquam erat volutpat. Suspendisse ac imperdiet turpis.
    Aenean finibus sollicitudin eros pharetra congue. Duis ornare egestas augue
    ut luctus. Proin blandit quam nec lacus varius commodo et a urna. Ut id
    ornare felis, eget fermentum sapien.
  </p>

  <p>
    Nam vulputate diam nec tempor bibendum. Donec luctus augue eget malesuada
    ultrices. Phasellus turpis est, posuere sit amet dapibus ut, facilisis sed
    est. Nam id risus quis ante semper consectetur eget aliquam lorem. Vivamus
    tristique elit dolor, sed pretium metus suscipit vel. Mauris ultricies
    lectus sed lobortis finibus. Vivamus eu urna eget velit cursus viverra quis
    vestibulum sem. Aliquam tincidunt eget purus in interdum. Cum sociis natoque
    penatibus et magnis dis parturient montes, nascetur ridiculus mus.
  </p>
</div>
```

Il codice renderizzato dovrebbe ora apparire così:

{{ EmbedLiveSample('column-spanning', '100%', 550) }}

## Colonne e frammentazione

Il contenuto di un layout a colonne multiple viene frammentato. Si comporta essenzialmente allo stesso modo dei contenuti nei media paginati, ad esempio quando si stampa una pagina web. Quando il contenuto viene trasformato in un contenitore multicol, viene frammentato in colonne. Per poterlo fare, il contenuto deve _interrompersi_.

### Riquadri frammentati

Talvolta questa interruzione avviene in punti che producono una scarsa esperienza di lettura. Nell'esempio seguente, multicol viene utilizzato per disporre una serie di riquadri, ciascuno dei quali contiene un'intestazione e del testo. L'intestazione viene separata dal testo se le colonne si frammentano tra i due.

```css hidden live-sample___fragmented-boxes live-sample___fragmented-boxes-fixed
body {
  width: 90%;
  max-width: 900px;
  margin: 2em auto;
  font:
    0.9em/1.2 "Helvetica",
    "Arial",
    sans-serif;
}
```

```html live-sample___fragmented-boxes live-sample___fragmented-boxes-fixed
<div class="container">
  <div class="card">
    <h2>I am the heading</h2>
    <p>
      Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla luctus
      aliquam dolor, eu lacinia lorem placerat vulputate. Duis felis orci,
      pulvinar id metus ut, rutrum luctus orci. Cras porttitor imperdiet nunc,
      at ultricies tellus laoreet sit amet. Sed auctor cursus massa at porta.
      Integer ligula ipsum, tristique sit amet orci vel, viverra egestas ligula.
    </p>
  </div>

  <div class="card">
    <h2>I am the heading</h2>
    <p>
      Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla luctus
      aliquam dolor, eu lacinia lorem placerat vulputate. Duis felis orci,
      pulvinar id metus ut, rutrum luctus orci. Cras porttitor imperdiet nunc,
      at ultricies tellus laoreet sit amet. Sed auctor cursus massa at porta.
      Integer ligula ipsum, tristique sit amet orci vel, viverra egestas ligula.
    </p>
  </div>

  <div class="card">
    <h2>I am the heading</h2>
    <p>
      Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla luctus
      aliquam dolor, eu lacinia lorem placerat vulputate. Duis felis orci,
      pulvinar id metus ut, rutrum luctus orci. Cras porttitor imperdiet nunc,
      at ultricies tellus laoreet sit amet. Sed auctor cursus massa at porta.
      Integer ligula ipsum, tristique sit amet orci vel, viverra egestas ligula.
    </p>
  </div>
  <div class="card">
    <h2>I am the heading</h2>
    <p>
      Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla luctus
      aliquam dolor, eu lacinia lorem placerat vulputate. Duis felis orci,
      pulvinar id metus ut, rutrum luctus orci. Cras porttitor imperdiet nunc,
      at ultricies tellus laoreet sit amet. Sed auctor cursus massa at porta.
      Integer ligula ipsum, tristique sit amet orci vel, viverra egestas ligula.
    </p>
  </div>

  <div class="card">
    <h2>I am the heading</h2>
    <p>
      Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla luctus
      aliquam dolor, eu lacinia lorem placerat vulputate. Duis felis orci,
      pulvinar id metus ut, rutrum luctus orci. Cras porttitor imperdiet nunc,
      at ultricies tellus laoreet sit amet. Sed auctor cursus massa at porta.
      Integer ligula ipsum, tristique sit amet orci vel, viverra egestas ligula.
    </p>
  </div>

  <div class="card">
    <h2>I am the heading</h2>
    <p>
      Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla luctus
      aliquam dolor, eu lacinia lorem placerat vulputate. Duis felis orci,
      pulvinar id metus ut, rutrum luctus orci. Cras porttitor imperdiet nunc,
      at ultricies tellus laoreet sit amet. Sed auctor cursus massa at porta.
      Integer ligula ipsum, tristique sit amet orci vel, viverra egestas ligula.
    </p>
  </div>

  <div class="card">
    <h2>I am the heading</h2>
    <p>
      Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla luctus
      aliquam dolor, eu lacinia lorem placerat vulputate. Duis felis orci,
      pulvinar id metus ut, rutrum luctus orci. Cras porttitor imperdiet nunc,
      at ultricies tellus laoreet sit amet. Sed auctor cursus massa at porta.
      Integer ligula ipsum, tristique sit amet orci vel, viverra egestas ligula.
    </p>
  </div>
</div>
```

```css live-sample___fragmented-boxes live-sample___fragmented-boxes-fixed
.container {
  column-width: 250px;
  column-gap: 1em;
}

.card {
  background-color: rgb(207 232 220);
  border: 2px solid rgb(79 185 227);
  padding: 10px;
  margin-bottom: 1em;
}
```

{{ EmbedLiveSample('fragmented-boxes', '100%', 1000) }}

### Impostare break-inside

Per controllare questo comportamento, è possibile utilizzare proprietà della specifica [CSS Fragmentation](/it/docs/Web/CSS/Guides/Fragmentation). Questa specifica fornisce proprietà per controllare l'interruzione dei contenuti in multicol e nei media paginati. Ad esempio, aggiungendo la proprietà {{cssxref("break-inside")}} con valore `avoid` alle regole per `.card`. Questo è il contenitore dell'intestazione e del testo, quindi non si desidera che venga frammentato.

```css live-sample___fragmented-boxes-fixed
.card {
  break-inside: avoid;
  background-color: rgb(207 232 220);
  border: 2px solid rgb(79 185 227);
  padding: 10px;
  margin-bottom: 1em;
}
```

L'aggiunta di questa proprietà fa sì che i riquadri rimangano interi: ora non vengono _frammentati_ tra le colonne.

{{ EmbedLiveSample('fragmented-boxes-fixed', '100%', 1100) }}

## Riepilogo

Ora sono note le funzionalità di base del layout a colonne multiple, un altro strumento a disposizione quando si sceglie un metodo di layout per i design in fase di realizzazione.

## Vedi anche

- [CSS Fragmentation](/it/docs/Web/CSS/Guides/Fragmentation)
- [Utilizzare layout a colonne multiple](/it/docs/Web/CSS/Guides/Multicol_layout/Using)
