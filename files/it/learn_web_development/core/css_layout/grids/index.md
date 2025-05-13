---
title: Layout a griglia CSS
slug: Learn_web_development/Core/CSS_layout/Grids
l10n:
  sourceCommit: 2cdde962d935653e92c2b0e046ead20efbb26ec8
---

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Flexbox", "Learn_web_development/Core/CSS_layout/Responsive_design", "Learn_web_development/Core/CSS_layout")}}

Il layout a griglia CSS è un sistema di layout bidimensionale per il web. Consente di organizzare contenuti in righe e colonne e offre molte funzionalità per semplificare la creazione di layout complessi. Questo articolo spiegherà tutto ciò che è necessario sapere per iniziare con il layout a griglia.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Strutturare i contenuti con HTML</a
        >,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Nozioni di base sullo stile CSS</a>,
        <a href="/it/docs/Learn_web_development/Core/Text_styling/Fundamentals">Nozioni fondamentali di stile di testo e font</a>,
        familiarità con <a href="/it/docs/Learn_web_development/Core/CSS_layout/Introduction">i concetti fondamentali del layout CSS</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere lo scopo di CSS Grid — disporre in modo flessibile un insieme di elementi blocco o inline in due dimensioni.</li>
          <li>Comprendere la terminologia della griglia — righe, colonne, spazi e canali.</li>
          <li>Comprendere cosa offre di predefinito <code>display: grid</code>.</li>
          <li>Definire righe, colonne e spazi della griglia.</li>
          <li>Posizionare elementi sulla griglia.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Cos'è il layout a griglia?

Una griglia è una raccolta di linee orizzontali e verticali che creano un modello contro cui possiamo allineare gli elementi del nostro design. Aiutano a creare layout nei quali i nostri elementi non saltano o cambiano larghezza quando si passa da una pagina all'altra, fornendo maggiore coerenza nei nostri siti web.

Una griglia avrà tipicamente **colonne**, **righe** e poi spazi tra ciascuna riga e colonna. Gli spazi sono comunemente chiamati **canali**.

![Griglia CSS con parti etichettate come righe, colonne e canali. Le righe sono i segmenti orizzontali della griglia e le colonne sono i segmenti verticali della griglia. Lo spazio tra due righe si chiama "canale di riga" e lo spazio tra 2 colonne si chiama "canale di colonna".](grid.png)

## Creare la tua griglia in CSS

Avendo deciso la griglia di cui il tuo design ha bisogno, puoi usare il layout a griglia CSS per crearlo. Esamineremo prima le caratteristiche di base del layout a griglia e poi esploreremo come creare un semplice sistema a griglia per il tuo progetto.
Il seguente video fornisce una spiegazione visiva sull'utilizzo di CSS grid:

{{EmbedYouTube("KOvGeFUHAC0")}}

### Definire una griglia

Proviamo i layout a griglia, ecco un esempio con un contenitore che ha alcuni elementi figlio. Di default, questi elementi sono visualizzati in un flusso normale, causando la loro apparizione uno sotto l'altro.

```html live-sample___simple-grid_0
<div class="container">
  <div>One</div>
  <div>Two</div>
  <div>Three</div>
  <div>Four</div>
  <div>Five</div>
  <div>Six</div>
  <div>Seven</div>
</div>
```

```css live-sample___simple-grid_0
body {
  font-family: sans-serif;
}
.container > div {
  border-radius: 5px;
  padding: 10px;
  background-color: rgb(207 232 220);
  border: 2px solid rgb(79 185 227);
}
```

{{EmbedLiveSample('simple-grid_0', '100%', "310") }}

In modo simile a come si definisce il flexbox, si definisce un layout a griglia impostando il valore della proprietà {{cssxref("display")}} su `grid`. Come nel caso del flexbox, la proprietà `display: grid` trasforma tutti i figli diretti del contenitore in elementi della griglia. Abbiamo aggiunto il seguente CSS al file:

```html hidden live-sample___simple-grid_1
<div class="container">
  <div>One</div>
  <div>Two</div>
  <div>Three</div>
  <div>Four</div>
  <div>Five</div>
  <div>Six</div>
  <div>Seven</div>
</div>
```

```css hidden live-sample___simple-grid_1
body {
  font-family: sans-serif;
}
.container > div {
  border-radius: 5px;
  padding: 10px;
  background-color: rgb(207 232 220);
  border: 2px solid rgb(79 185 227);
}
```

```css live-sample___simple-grid_1
.container {
  display: grid;
}
```

{{EmbedLiveSample('simple-grid_1', '100%', "310") }}

A differenza del flexbox, gli elementi non sembreranno immediatamente diversi. Dichiarando `display: grid` si ottiene una griglia di una colonna, quindi i tuoi elementi continueranno a essere visualizzati uno sotto l'altro come fanno nel flusso normale.

Per vedere qualcosa che assomiglia più a una griglia, dovremo aggiungere alcune colonne alla griglia. Aggiungiamo tre colonne di 200 pixel. Puoi utilizzare qualsiasi unità di lunghezza o percentuale per creare queste tracce di colonna.

```html hidden live-sample___simple-grid_2
<div class="container">
  <div>One</div>
  <div>Two</div>
  <div>Three</div>
  <div>Four</div>
  <div>Five</div>
  <div>Six</div>
  <div>Seven</div>
</div>
```

```css hidden live-sample___simple-grid_2
body {
  font-family: sans-serif;
}
.container > div {
  border-radius: 5px;
  padding: 10px;
  background-color: rgb(207 232 220);
  border: 2px solid rgb(79 185 227);
}
```

```css live-sample___simple-grid_2
.container {
  display: grid;
  grid-template-columns: 200px 200px 200px;
}
```

Dovresti vedere che gli elementi si sono riorganizzati in modo che ce ne sia uno in ciascuna cella della griglia.

{{EmbedLiveSample('simple-grid_2', '100%', "130") }}

### Griglie flessibili con l'unità fr

Oltre a creare griglie utilizzando lunghezze e percentuali, possiamo usare [`fr`](/it/docs/Web/CSS/flex_value). L'unità `fr` rappresenta una frazione dello spazio disponibile nel contenitore della griglia per dimensionare in modo flessibile righe e colonne della griglia.

```html hidden live-sample___grid-fr-unit_0
<div class="container">
  <div>One</div>
  <div>Two</div>
  <div>Three</div>
  <div>Four</div>
  <div>Five</div>
  <div>Six</div>
  <div>Seven</div>
</div>
```

```css hidden live-sample___grid-fr-unit_0
body {
  font-family: sans-serif;
}
.container > div {
  border-radius: 5px;
  padding: 10px;
  background-color: rgb(207 232 220);
  border: 2px solid rgb(79 185 227);
}
```

Qui cambiamo l'elenco delle tracce alla seguente definizione, creando tre tracce `1fr`:

```css live-sample___grid-fr-unit_0
.container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
}
```

{{EmbedLiveSample('grid-fr-unit_0', '100%', "130") }}

Ora hai tracce flessibili.
L'unità `fr` distribuisce lo spazio proporzionalmente, quindi puoi specificare valori positivi diversi per le tue tracce.
Cambia l'elenco delle tracce con la seguente definizione, creando una traccia `2fr` e due tracce `1fr`:

```html hidden live-sample___grid-fr-unit_1
<div class="container">
  <div>One</div>
  <div>Two</div>
  <div>Three</div>
  <div>Four</div>
  <div>Five</div>
  <div>Six</div>
  <div>Seven</div>
</div>
```

```css hidden live-sample___grid-fr-unit_1
body {
  font-family: sans-serif;
}
.container > div {
  border-radius: 5px;
  padding: 10px;
  background-color: rgb(207 232 220);
  border: 2px solid rgb(79 185 227);
}
```

```css live-sample___grid-fr-unit_1
.container {
  display: grid;
  grid-template-columns: 2fr 1fr 1fr;
}
```

{{EmbedLiveSample('grid-fr-unit_1', '100%', "130") }}

La prima traccia ottiene `2fr` dello spazio disponibile mentre le altre due tracce ottengono `1fr`, rendendo la prima traccia più grande. Puoi mescolare unità `fr` con unità di lunghezza fissa. In questo caso, lo spazio necessario per le tracce fisse viene utilizzato per primo prima che lo spazio rimanente venga distribuito alle altre tracce.

> [!NOTE]
> L'unità `fr` distribuisce lo spazio _disponibile_, non _tutto_ lo spazio. Pertanto, se una delle tue tracce ha qualcosa di grande al suo interno, ci sarà meno spazio libero da condividere.

### Spazi tra le tracce

Per creare spazi tra le tracce, usiamo le proprietà:

- {{cssxref("column-gap")}} per gli spazi tra le colonne
- {{cssxref("row-gap")}} per gli spazi tra le righe
- {{cssxref("gap")}} come scorciatoia per entrambi

```html hidden live-sample___grid-gap
<div class="container">
  <div>One</div>
  <div>Two</div>
  <div>Three</div>
  <div>Four</div>
  <div>Five</div>
  <div>Six</div>
  <div>Seven</div>
</div>
```

```css hidden live-sample___grid-gap
body {
  font-family: sans-serif;
}
.container > div {
  border-radius: 5px;
  padding: 10px;
  background-color: rgb(207 232 220);
  border: 2px solid rgb(79 185 227);
}
```

Qui aggiungiamo la proprietà `gap` per creare spazi tra le tracce con un valore di `20px`:

```css live-sample___grid-gap
.container {
  display: grid;
  grid-template-columns: 2fr 1fr 1fr;
  gap: 20px;
}
```

{{EmbedLiveSample('grid-gap', '100%', "180") }}

Questi spazi possono essere qualsiasi unità di lunghezza o percentuale, ma non un'unità `fr`.

### Ripetizione degli elenchi di tracce

Puoi ripetere tutto o solo una sezione della tua lista di tracce usando la funzione CSS `repeat()`.
Qui cambiamo la lista delle tracce nella seguente:

```html hidden live-sample___grid-repeat
<div class="container">
  <div>One</div>
  <div>Two</div>
  <div>Three</div>
  <div>Four</div>
  <div>Five</div>
  <div>Six</div>
  <div>Seven</div>
</div>
```

```css hidden live-sample___grid-repeat
body {
  font-family: sans-serif;
}
.container > div {
  border-radius: 5px;
  padding: 10px;
  background-color: rgb(207 232 220);
  border: 2px solid rgb(79 185 227);
}
```

```css live-sample___grid-repeat
.container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
}
```

{{EmbedLiveSample('grid-repeat', '100%', "180") }}

Ora avrai tre tracce `1fr` proprio come prima. Il primo valore passato alla funzione `repeat()` specifica il numero di volte in cui vuoi che l'elenco si ripeta, mentre il secondo valore è una lista di tracce, che può essere una o più tracce che vuoi ripetere.

### Griglie implicite ed esplicite

Finora, abbiamo specificato solo tracce di colonna, ma le righe vengono create automaticamente per contenere i contenuti. Questo concetto evidenzia la differenza tra griglie esplicite e implicite.
Ecco un po' di più sulla differenza tra i due tipi di griglie:

- **Griglia esplicita** viene creata usando `grid-template-columns` o `grid-template-rows`.
- **Griglia implicita** estende la griglia esplicita definita quando il contenuto viene collocato al di fuori di quella griglia, ad esempio nelle righe tracciando linee di griglia aggiuntive.

Per impostazione predefinita, le tracce create nella griglia implicita sono dimensionate automaticamente, il che in generale significa che sono abbastanza grandi da contenere il loro contenuto. Se desideri dare alle tracce di griglia implicite una dimensione, puoi usare le proprietà {{cssxref("grid-auto-rows")}} e {{cssxref("grid-auto-columns")}}. Se aggiungi `grid-auto-rows` con un valore di `100px` al tuo CSS, vedrai che quelle righe create ora sono alte 100 pixel.

```html hidden live-sample___grid-auto
<div class="container">
  <div>One</div>
  <div>Two</div>
  <div>Three</div>
  <div>Four</div>
  <div>Five</div>
  <div>Six</div>
  <div>Seven</div>
</div>
```

```css hidden live-sample___grid-auto
body {
  font-family: sans-serif;
}
.container > div {
  border-radius: 5px;
  padding: 10px;
  background-color: rgb(207 232 220);
  border: 2px solid rgb(79 185 227);
}
```

```css live-sample___grid-auto
.container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-auto-rows: 100px;
  gap: 20px;
}
```

{{EmbedLiveSample('grid-auto', '100%', "350") }}

### La funzione minmax()

Le nostre tracce alte 100 pixel non saranno molto utili se aggiungiamo contenuto in quelle tracce che è più alto di 100 pixel, nel qual caso causerebbe un overflow. Potrebbe essere meglio avere tracce che siano _almeno_ alte 100 pixel e possano ancora espandersi se viene aggiunto più contenuto. Un fatto abbastanza basilare sul web è che non sai mai realmente quanto qualcosa sarà alto: contenuti aggiuntivi o dimensioni dei font più grandi possono causare problemi con design che tentano di essere perfetti in ogni dimensione.

La funzione {{cssxref("minmax", "minmax()")}} ci consente di impostare una dimensione minima e massima per una traccia, ad esempio, `minmax(100px, auto)`. La dimensione minima è di 100 pixel, ma la dimensione massima è `auto`, che si espanderà per accogliere più contenuti. Qui cambiamo `grid-auto-rows` per utilizzare un valore `minmax()`:

```html hidden live-sample___grid-minmax_0
<div class="container">
  <div>One</div>
  <div>Two</div>
  <div>Three</div>
  <div>Four<br />More content</div>
  <div>Five</div>
  <div>Six</div>
  <div>Seven</div>
</div>
```

```css hidden live-sample___grid-minmax_0
body {
  font-family: sans-serif;
}
.container > div {
  border-radius: 5px;
  padding: 10px;
  background-color: rgb(207 232 220);
  border: 2px solid rgb(79 185 227);
}
```

```css live-sample___grid-minmax_0
.container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-auto-rows: minmax(50px, auto);
  gap: 20px;
}
```

{{EmbedLiveSample('grid-minmax_0', '100%', "210") }}

Se aggiungi contenuto extra, vedrai che la traccia si espande per consentirgli di adattarsi. Nota che l'espansione avviene lungo tutta la riga.

### Tante colonne quanto basta

Possiamo combinare alcune delle lezioni apprese sulla lista delle tracce, la notazione di ripetizione e {{cssxref("minmax", "minmax()")}} per creare un modello utile. A volte è utile poter chiedere alla griglia di creare quante colonne possano adattarsi nel contenitore. Lo facciamo impostando il valore di `grid-template-columns` utilizzando la funzione {{cssxref("repeat", "repeat()")}}, ma invece di passare un numero, passiamo la parola chiave [`auto-fit`](/it/docs/Web/CSS/repeat#auto-fit). Per il secondo parametro della funzione usiamo `minmax()` con un valore minimo uguale alla dimensione minima della traccia che vorremmo avere e un massimo di `1fr`.

```html hidden live-sample___grid-minmax_1
<div class="container">
  <div>One</div>
  <div>Two</div>
  <div>Three</div>
  <div>Four<br />More content</div>
  <div>Five</div>
  <div>Six</div>
  <div>Seven</div>
</div>
```

```css hidden live-sample___grid-minmax_1
body {
  font-family: sans-serif;
}
.container > div {
  border-radius: 5px;
  padding: 10px;
  background-color: rgb(207 232 220);
  border: 2px solid rgb(79 185 227);
}
```

```css live-sample___grid-minmax_1
.container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(230px, 1fr));
  grid-auto-rows: minmax(50px, auto);
  gap: 20px;
}
```

{{EmbedLiveSample('grid-minmax_1', '100%', "210") }}

Funziona perché la griglia sta creando quante più colonne da 230 pixel possano adattarsi nel contenitore, quindi distribuendo lo spazio rimanente tra tutte le colonne. Il massimo è `1fr` che, come già sappiamo, distribuisce spazio equamente tra le tracce.

## Posizionamento basato sulle linee

Passiamo ora dalla creazione di una griglia al posizionamento degli oggetti sulla griglia. La nostra griglia ha sempre linee — queste sono numerate inizialmente a 1 e si riferiscono al [modo di scrittura](/it/docs/Web/CSS/CSS_writing_modes) del documento. Ad esempio, la linea di colonna 1 in inglese (scritto da sinistra a destra) si troverebbe sul lato sinistro della griglia e la linea di riga 1 in alto, mentre in arabo (scritto da destra a sinistra), la linea di colonna 1 si troverebbe sul lato destro.

Per posizionare gli elementi lungo queste linee, possiamo specificare le linee di inizio e fine dell'area griglia dove un elemento dovrebbe essere posizionato. Ci sono quattro proprietà che possiamo usare per fare questo:

- {{cssxref("grid-column-start")}}
- {{cssxref("grid-column-end")}}
- {{cssxref("grid-row-start")}}
- {{cssxref("grid-row-end")}}

Queste proprietà accettano numeri di linea come loro valori, quindi possiamo specificare che un elemento dovrebbe iniziare sulla linea 1 e finire sulla linea 3, ad esempio.
In alternativa, è possibile utilizzare anche proprietà abbreviate che ti consentono di specificare le linee di inizio e fine simultaneamente, separate da una barra `/`:

- {{cssxref("grid-column")}} abbreviazione per `grid-column-start` e `grid-column-end`
- {{cssxref("grid-row")}} abbreviazione per `grid-row-start` e `grid-row-end`

```html live-sample___grid-placement_0
<div class="container">
  <header>Header</header>
  <main>
    <h1>Main</h1>
    <p>Main content…</p>
  </main>
  <aside>
    <h2>Aside</h2>
    <p>Related content</p>
  </aside>
  <footer>footer</footer>
</div>
```

```css live-sample___grid-placement_0
.container {
  font-family: sans-serif;
  display: grid;
  grid-template-columns: 1fr 3fr;
  gap: 20px;
}
header,
footer {
  border-radius: 5px;
  padding: 10px;
  background-color: rebeccapurple;
  color: whitesmoke;
  text-align: center;
}
aside {
  border-right: 1px solid rebeccapurple;
}
```

Senza la posizione definita, puoi vedere che il _posizionamento automatico_ sta collocando ciascun elemento nella propria cella nella griglia. L'{{htmlelement("header")}} occupa `1fr` (un quarto) e l'{{htmlelement("main")}} occupa `3fr` (tre quarti).

{{EmbedLiveSample('grid-placement_0', '100%', "230") }}

Disponiamo tutti gli elementi per il nostro sito utilizzando le linee della griglia. Aggiungi le seguenti regole alla fine del tuo CSS:

```html hidden live-sample___grid-placement_1
<div class="container">
  <header>Header</header>
  <main>
    <h1>Main</h1>
    <p>Main content…</p>
  </main>
  <aside>
    <h2>Aside</h2>
    <p>Related content</p>
  </aside>
  <footer>footer</footer>
</div>
```

```css hidden live-sample___grid-placement_1
.container {
  font-family: sans-serif;
  display: grid;
  grid-template-columns: 1fr 3fr;
  gap: 20px;
}
header,
footer {
  border-radius: 5px;
  padding: 10px;
  background-color: rebeccapurple;
  color: whitesmoke;
  text-align: center;
}
aside {
  border-right: 1px solid rebeccapurple;
}
```

```css live-sample___grid-placement_1
header {
  grid-column: 1 / 3;
  grid-row: 1;
}
main {
  grid-column: 2;
  grid-row: 2;
}
aside {
  grid-column: 1;
  grid-row: 2;
}
footer {
  grid-column: 1 / 3;
  grid-row: 3;
}
```

Ora l'{{htmlelement("header")}} e l'{{htmlelement("footer")}} sono impostati su `1 / 3`, il che significa che iniziano alla linea `1` e finiscono alla linea `3`.

{{EmbedLiveSample('grid-placement_1', '100%', "230") }}

> [!NOTE]
> È possibile utilizzare anche il valore `-1` per mirare alla linea della colonna o della riga finale, quindi contare verso l'interno dall'estremità usando valori negativi. Nota anche che le linee contano sempre dai bordi della griglia esplicita, non dalla {{Glossary("Grid", "griglia implicita")}}.

## Posizionamento con grid-template-areas

Un modo alternativo per disporre gli elementi sulla tua griglia è utilizzare la proprietà {{cssxref("grid-template-areas")}} e assegnare un nome ai vari elementi del tuo design.

```html hidden live-sample___grid-placement_2
<div class="container">
  <header>Header</header>
  <main>
    <h1>Main</h1>
    <p>Main content…</p>
  </main>
  <aside>
    <h2>Aside</h2>
    <p>Related content</p>
  </aside>
  <footer>footer</footer>
</div>
```

```css hidden live-sample___grid-placement_2
.container {
  font-family: sans-serif;
  display: grid;
  grid-template-columns: 1fr 3fr;
  gap: 20px;
}
header,
footer {
  border-radius: 5px;
  padding: 10px;
  background-color: rebeccapurple;
  color: whitesmoke;
  text-align: center;
}
aside {
  border-right: 1px solid rebeccapurple;
}
```

```css live-sample___grid-placement_2
.container {
  display: grid;
  grid-template-areas:
    "header header"
    "sidebar content"
    "footer footer";
  grid-template-columns: 1fr 3fr;
  gap: 20px;
}
header {
  grid-area: header;
}
main {
  grid-area: content;
}
aside {
  grid-area: sidebar;
}
footer {
  grid-area: footer;
}
```

Qui stiamo usando la proprietà {{CSSXRef("grid-template-areas")}} per definire come sono disposte le 3 righe. La prima riga ha un valore di `header header`, la seconda `sidebar content` e la terza `footer footer`. Stiamo quindi usando la proprietà {{CSSXRef("grid-area")}} per definire dove gli elementi sono posizionati nelle `grid-template-areas`.

{{EmbedLiveSample('grid-placement_2', '100%', "230") }}

Le regole per `grid-template-areas` sono le seguenti:

- Devi riempire ogni cella della griglia.
- Per fare span su due celle, ripeti il nome.
- Per lasciare una cella vuota, usa un `.` (punto).
- Le aree devono essere rettangolari — ad esempio, non puoi avere un'area a forma di L.
- Le aree non possono essere ripetute in posizioni diverse.

Puoi sperimentare ulteriormente con il nostro layout, cambiando il footer in modo che si trovi solo sotto l'articolo e la sidebar per estendersi in basso. Questo è un modo molto chiaro per descrivere un layout perché è evidente solo guardando il CSS per sapere esattamente cosa sta accadendo.

## Annidare griglie e subgrid

È possibile annidare una griglia all'interno di un'altra griglia, creando una ["subgrid"](/it/docs/Web/CSS/CSS_grid_layout/Subgrid).
Puoi farlo impostando la proprietà `display: grid` su un elemento della griglia padre.

Espandiamo il precedente esempio aggiungendo un contenitore per gli articoli e usando una griglia annidata per controllare il layout di più articoli.
Anche se stiamo usando una sola colonna nella griglia annidata, possiamo definire le righe da dividere in un rapporto di 4:3:3 usando la proprietà `grid-template-rows`.
Questo approccio ci consente di creare un layout in cui un articolo nella parte superiore della pagina ha una visualizzazione grande, mentre gli altri hanno un layout più piccolo simile a un'anteprima.

```html hidden live-sample___nesting-grids
<div class="container">
  <header>Header</header>
  <main>
    <article>
      <h1>Article one</h1>
      <p>Content…</p>
    </article>
    <article>
      <h1>Article two</h1>
      <p>Content…</p>
    </article>
    <article>
      <h1>Article three</h1>
      <p>Content…</p>
    </article>
  </main>
  <aside>
    <h2>Aside</h2>
    <p>Related content</p>
  </aside>
  <footer>footer</footer>
</div>
```

```css hidden live-sample___nesting-grids
.container {
  font-family: sans-serif;
  display: grid;
  grid-template-columns: 1fr 3fr;
  gap: 20px;
}
header,
footer {
  border-radius: 5px;
  padding: 10px;
  background-color: rebeccapurple;
  color: whitesmoke;
  text-align: center;
}
header {
  grid-area: header;
}
aside {
  border-right: 1px solid rebeccapurple;
  grid-area: sidebar;
}
footer {
  grid-area: footer;
}
.container {
  display: grid;
  grid-template-areas:
    "header header"
    "sidebar content"
    "footer footer";
  grid-template-columns: 1fr 3fr;
  gap: 20px;
}
```

```css live-sample___nesting-grids
main {
  grid-area: content;
  display: grid;
  grid-template-rows: 4fr 3fr 3fr;
  gap: inherit;
}
article {
  padding: 10px;
  border: 2px solid rebeccapurple;
  border-radius: 5px;
}
```

{{EmbedLiveSample('nesting-grids', '100%', 560)}}

Per rendere più facile lavorare con layout in griglie annidate, puoi usare `subgrid` sulle proprietà `grid-template-rows` e `grid-template-columns`. Questo ti permette di sfruttare le tracce definite nella griglia genitore.

Nel seguente esempio, stiamo usando il [posizionamento basato sulle linee](#posizionamento_basato_sulle_linee), consentendo alla griglia annidata di coprire più colonne e righe della griglia padre.
Abbiamo aggiunto `subgrid` per ereditare le colonne della griglia del genitore, mentre aggiungiamo un layout diverso per le righe all'interno della griglia annidata.

```html hidden live-sample___subgrid
<div class="container">
  <div>One</div>
  <div>Two</div>
  <div>Three</div>
  <div>Four</div>
  <div class="subgrid">
    <div>Five</div>
    <div>Six</div>
    <div>Seven</div>
    <div>Eight</div>
  </div>
  <div>Nine</div>
  <div>Ten</div>
</div>
```

```css hidden live-sample___subgrid
.container {
  font-family: sans-serif;
}
.container div {
  border-radius: 5px;
  padding: 10px;
  background-color: rebeccapurple;
  border: 1px solid white;
  color: white;
}
```

```css live-sample___subgrid
.container {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-template-rows: repeat(1, 1fr);
  gap: 10px;
}
.subgrid {
  grid-column: 1 / 4;
  grid-row: 2 / 4;
  display: grid;
  gap: inherit;
  grid-template-columns: subgrid;
  grid-template-rows: 2fr 1fr;
}
```

{{ EmbedLiveSample('subgrid', '100%', 200) }}

## Framework a griglia

Esistono numerosi framework di griglie disponibili, che offrono una griglia a 12 o 16 colonne, per aiutare a disporre i tuoi contenuti.
La buona notizia è che probabilmente non avrai bisogno di framework di terze parti per aiutarti a creare layout basati sulla griglia — la funzionalità della griglia è già inclusa nelle specifiche ed è supportata dalla maggior parte dei browser moderni.

Questo ha un contenitore con una griglia a 12 colonne definita, usando `grid-template-columns: repeat(12, 1fr);`, e lo stesso markup usato nei due esempi precedenti. Possiamo ora usare il posizionamento basato sulle linee per disporre il nostro contenuto nella griglia a 12 colonne.

```html hidden live-sample___grid-frameworks
<div class="container">
  <header>Header</header>
  <main>
    <h1>Main</h1>
    <p>Main content…</p>
  </main>
  <aside>
    <h2>Aside</h2>
    <p>Related content</p>
  </aside>
  <footer>footer</footer>
</div>
```

```css hidden live-sample___grid-frameworks
.container {
  font-family: sans-serif;
}

header,
footer {
  border-radius: 5px;
  padding: 10px;
  background-color: rebeccapurple;
  color: whitesmoke;
  text-align: center;
}
aside {
  border-right: 1px solid rebeccapurple;
}
```

```css live-sample___grid-frameworks
.container {
  font-family: sans-serif;
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  gap: 20px;
}
header {
  grid-column: 1 / 13;
  grid-row: 1;
}
main {
  grid-column: 4 / 13;
  grid-row: 2;
}
aside {
  grid-column: 1 / 4;
  grid-row: 2;
}
footer {
  grid-column: 1 / 13;
  grid-row: 3;
}
```

{{EmbedLiveSample('grid-frameworks', '100%', "230") }}

Se utilizzi l'[ispettore di griglia di Firefox](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/how_to/examine_grid_layouts/index.html) per sovrapporre le linee della griglia al tuo design, puoi vedere come funziona la nostra griglia a 12 colonne.

![Una griglia a 12 colonne sovrapposta al nostro design.](learn-grids-inspector.png)

## Metti alla prova le tue competenze!

Hai raggiunto la fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare ulteriori test per verificare di aver assimilato queste informazioni prima di passare oltre — vedi [Metti alla prova le tue competenze: Grid](/it/docs/Learn_web_development/Core/CSS_layout/Test_your_skills/Grid).

## Riassunto

In quest'overview, abbiamo esplorato le caratteristiche principali del layout a griglia CSS. Dovrebbe essere in grado di iniziare a usarlo nei suoi progetti. Prossimamente, esamineremo il design reattivo.

## Vedi anche

- [Layout a griglia CSS](/it/docs/Web/CSS/CSS_grid_layout#guides)
  - : Pagina principale del modulo CSS Grid Layout, contenente molte risorse aggiuntive
- [Guida completa alla griglia CSS](https://css-tricks.com/snippets/css/complete-guide-grid/)
  - : Una guida visiva su CSS-Tricks (2023).
- [Grid Garden](https://cssgridgarden.com/)
  - : Un gioco educativo per apprendere e comprendere meglio le basi della griglia su cssgridgarden.com.

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Flexbox", "Learn_web_development/Core/CSS_layout/Responsive_design", "Learn_web_development/Core/CSS_layout")}}
