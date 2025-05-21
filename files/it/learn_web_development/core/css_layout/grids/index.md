---
title: Layout a griglia CSS
slug: Learn_web_development/Core/CSS_layout/Grids
l10n:
  sourceCommit: 2cdde962d935653e92c2b0e046ead20efbb26ec8
---

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Flexbox", "Learn_web_development/Core/CSS_layout/Responsive_design", "Learn_web_development/Core/CSS_layout")}}

Il layout a griglia CSS è un sistema di layout bidimensionale per il web. Ti permette di organizzare i contenuti in righe e colonne e offre molte funzionalità per semplificare la creazione di layout complessi. Questo articolo spiegherà tutto ciò che devi sapere per iniziare a utilizzare il layout a griglia.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Strutturare i contenuti con HTML</a
        >,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Nozioni di base sullo stile CSS</a>,
        <a href="/it/docs/Learn_web_development/Core/Text_styling/Fundamentals">Fondamenti di stili di testo e font</a>,
        familiarità con i <a href="/it/docs/Learn_web_development/Core/CSS_layout/Introduction">fondamentali concetti del layout CSS</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere lo scopo della CSS Grid — disporre in modo flessibile un insieme di elementi blocco o inline in due dimensioni.</li>
          <li>Comprendere la terminologia della griglia — righe, colonne, spazi e intercapedini.</li>
          <li>Comprendere cosa offre <code>display: grid</code> di default.</li>
          <li>Definire righe, colonne e spazi della griglia.</li>
          <li>Posizionamento degli elementi sulla griglia.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Cos'è il layout a griglia?

Una griglia è una raccolta di linee orizzontali e verticali che creano un modello contro il quale possiamo allineare i nostri elementi di design. Aiutano a creare layout in cui i nostri elementi non saltano o cambiano larghezza quando ci spostiamo da una pagina all'altra, fornendo maggiore consistenza nei nostri siti web.

Una griglia tipicamente avrà **colonne**, **righe** e poi spazi tra ciascuna riga e colonna. Gli spazi vengono comunemente indicati come **intercapedini**.

![Griglia CSS con parti etichettate come righe, colonne e intercapedini. Le righe sono i segmenti orizzontali della griglia e le colonne sono i segmenti verticali della griglia. Lo spazio tra due righe si chiama 'intercapedine di riga' e lo spazio tra due colonne si chiama 'intercapedine di colonna'.](grid.png)

## Creazione della griglia in CSS

Dopo aver deciso la griglia di cui il tuo design ha bisogno, puoi usare il layout a griglia CSS per crearla. Vedremo dapprima le caratteristiche di base del layout a griglia e poi esploreremo come creare un semplice sistema a griglia per il tuo progetto.
Il video seguente fornisce una bella spiegazione visiva sull'utilizzo della griglia CSS:

{{EmbedYouTube("KOvGeFUHAC0")}}

### Definire una griglia

Proviamo i layout a griglia, ecco un esempio con un contenitore che contiene alcuni elementi figli. Di default, questi elementi sono mostrati in un flusso normale, causando la visualizzazione uno sotto l'altro.

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

In modo simile a come definisci il flexbox, definisci un layout a griglia impostando il valore della proprietà {{cssxref("display")}} su `grid`. Come nel caso del flexbox, la proprietà `display: grid` trasforma tutti i discendenti diretti del contenitore in elementi della griglia. Abbiamo aggiunto il seguente CSS al file:

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

A differenza del flexbox, gli elementi non sembreranno immediatamente diversi. Dichiarando `display: grid` ottieni una griglia di una colonna, quindi i tuoi elementi continueranno a essere visualizzati uno sotto l'altro come avviene nel flusso normale.

Per vedere qualcosa che sembri più una griglia, dovremo aggiungere alcune colonne alla griglia. Aggiungiamo tre colonne di 200 pixel. Puoi utilizzare qualsiasi unità di lunghezza o percentuale per creare questi tracciati di colonne.

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

Dovresti vedere che gli elementi si sono disposti in modo che ci sia uno in ciascuna cella della griglia.

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

Qui cambiamo l'elenco dei tracciati alla seguente definizione, creando tre tracciati `1fr`:

```css live-sample___grid-fr-unit_0
.container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
}
```

{{EmbedLiveSample('grid-fr-unit_0', '100%', "130") }}

Ora hai tracciati flessibili.
L'unità `fr` distribuisce lo spazio in modo proporzionale, quindi puoi specificare valori diversi per i tuoi tracciati.
Cambia il tuo elenco di tracciati alla seguente definizione, creando un tracciato `2fr` e due tracciati `1fr`:

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

Il primo tracciato ottiene `2fr` dello spazio disponibile e gli altri due tracciati ottengono `1fr`, rendendo il primo tracciato più grande. Puoi mescolare unità `fr` con unità di lunghezza fissa. In questo caso, lo spazio necessario per i tracciati fissi viene utilizzato prima che lo spazio rimanente venga distribuito agli altri tracciati.

> [!NOTE]
> L'unità `fr` distribuisce lo spazio _disponibile_, non tutto lo spazio. Pertanto, se uno dei tuoi tracciati contiene qualcosa di grande al suo interno, ci sarà meno spazio libero da condividere.

### Spazi tra i tracciati

Per creare spazi tra i tracciati, utilizziamo le proprietà:

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

Qui aggiungiamo la proprietà `gap` per creare spazi tra i tracciati con un valore di `20px`:

```css live-sample___grid-gap
.container {
  display: grid;
  grid-template-columns: 2fr 1fr 1fr;
  gap: 20px;
}
```

{{EmbedLiveSample('grid-gap', '100%', "180") }}

Questi spazi possono essere qualsiasi unità di lunghezza o percentuale, ma non un'unità `fr`.

### Ripetere le liste di tracciati

Puoi ripetere tutta o solo una parte dell'elenco dei tuoi tracciati usando la funzione CSS `repeat()`.
Qui cambiamo l'elenco dei tracciati come segue:

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

Avrai ora tre tracciati `1fr` proprio come prima. Il primo valore passato alla funzione `repeat()` specifica il numero di volte in cui si desidera ripetere l'elenco, mentre il secondo valore è un elenco di tracciati, che può essere uno o più tracciati che desideri ripetere.

### Griglie implicite ed esplicite

Fino a questo punto, abbiamo specificato solo tracciati di colonne, ma le righe sono create automaticamente per contenere il contenuto. Questo concetto mette in evidenza la distinzione tra griglie esplicite e implicite.
Ecco qualcosa in più sulla differenza tra i due tipi di griglie:

- **Griglia esplicita** è creata usando `grid-template-columns` o `grid-template-rows`.
- **Griglia implicita** estende la griglia esplicita definita quando il contenuto è posizionato fuori da quella griglia, come nelle righe, aggiungendo ulteriori linee di griglia.

Per impostazione predefinita, i tracciati creati nella griglia implicita sono di dimensioni `auto`, il che in generale significa che sono abbastanza grandi da contenere il loro contenuto. Se desideri assegnare una dimensione ai tracciati della griglia implicita, puoi usare le proprietà {{cssxref("grid-auto-rows")}} e {{cssxref("grid-auto-columns")}}. Se aggiungi `grid-auto-rows` con un valore di `100px` al tuo CSS, vedrai che quelle righe create sono ora alte 100 pixel.

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

I nostri tracciati alti 100 pixel non saranno molto utili se inseriamo contenuti in quei tracciati più alti di 100 pixel, nel qual caso causerebbero un overflow. Potrebbe essere meglio avere tracciati che sono _almeno_ alti 100 pixel e possono comunque espandersi se viene aggiunto più contenuto. Un fatto abbastanza basilare sul web è che non sai mai veramente quanto sarà alto qualcosa — contenuti aggiuntivi o dimensioni del font più grandi possono causare problemi con progetti che tentano di essere perfetti al pixel in ogni dimensione.

La funzione {{cssxref("minmax", "minmax()")}} ci consente di impostare una dimensione minima e massima per un tracciato, ad esempio, `minmax(100px, auto)`. La dimensione minima è di 100 pixel, ma la massima è `auto`, che si espanderà per ospitare più contenuto. Qui cambiamo `grid-auto-rows` per usare un valore `minmax()`:

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

Se aggiungi contenuti extra, vedrai che il tracciato si espande per permettergli di adattarsi. Nota che l'espansione avviene lungo tutta la riga.

### Tante colonne quante ne entrano

Possiamo combinare alcune delle lezioni che abbiamo appreso sull'elenco dei tracciati, la notazione di ripetizione e {{cssxref("minmax", "minmax()")}} per creare un pattern utile. A volte è utile poter chiedere alla griglia di creare quante più colonne possibile che possono entrare nel contenitore. Facciamo questo impostando il valore di `grid-template-columns` usando la funzione {{cssxref("repeat", "repeat()")}}, ma invece di passare un numero, passiamo la parola chiave [`auto-fit`](/it/docs/Web/CSS/repeat#auto-fit). Per il secondo parametro della funzione usiamo `minmax()` con un valore minimo pari alla dimensione minima del tracciato che vorremmo avere e un massimo di `1fr`.

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

Questo funziona perché la griglia sta creando quante più colonne da 230 pixel possono entrare nel contenitore, quindi condivide qualsiasi spazio rimasto tra tutte le colonne. Il massimo è `1fr`, che, come già sappiamo, distribuisce lo spazio in modo uniforme tra i tracciati.

## Posizionamento basato sulle linee

Passiamo ora dalla creazione di una griglia al posizionamento degli elementi sulla griglia. La nostra griglia ha sempre delle linee — queste sono numerate a partire da 1 e si riferiscono alla [modalità di scrittura](/it/docs/Web/CSS/CSS_writing_modes) del documento. Ad esempio, la linea di colonna 1 in inglese (scritto da sinistra a destra) sarebbe sul lato sinistro della griglia e la linea di riga 1 in alto, mentre in arabo (scritto da destra a sinistra), la linea di colonna 1 sarebbe sul lato destro.

Per posizionare gli elementi lungo queste linee, possiamo specificare le linee di inizio e fine dell'area della griglia in cui un elemento dovrebbe essere posizionato. Ci sono quattro proprietà che possiamo usare per farlo:

- {{cssxref("grid-column-start")}}
- {{cssxref("grid-column-end")}}
- {{cssxref("grid-row-start")}}
- {{cssxref("grid-row-end")}}

Queste proprietà accettano numeri di linea come loro valori, così possiamo specificare che un elemento dovrebbe iniziare sulla linea 1 e finire sulla linea 3, per esempio.
In alternativa, puoi anche usare proprietà abbreviate che ti consentono di specificare contemporaneamente le linee di inizio e fine, separate da una barra inclinata `/`:

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

Senza il posizionamento definito, puoi vedere che il _posizionamento automatico_ sta piazzando ogni elemento nella propria cella nella griglia. L'{{htmlelement("header")}} sta occupando `1fr` (un quarto) e l'{{htmlelement("main")}} sta occupando `3fr` (tre quarti).

{{EmbedLiveSample('grid-placement_0', '100%', "230") }}

Disponiamo ora tutti gli elementi per il nostro sito usando le linee di griglia. Aggiungi le seguenti regole in fondo al tuo CSS:

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
> Puoi anche usare il valore `-1` per mirare alla linea di colonna o riga finale, quindi contare verso l'interno dalla fine usando valori negativi. Nota anche che le linee contano sempre dai bordi della griglia esplicita, non dalla {{Glossary("Grid", "griglia implicita")}}.

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

Qui stiamo usando la proprietà {{CSSXRef("grid-template-areas")}} per definire come sono disposti i 3 righi. Il primo rigo ha un valore di `header header`, il secondo `sidebar content` e il terzo `footer footer`. Stiamo quindi usando la proprietà {{CSSXRef("grid-area")}} per definire dove sono posizionati gli elementi nelle `grid-template-areas`.

{{EmbedLiveSample('grid-placement_2', '100%', "230") }}

Le regole per `grid-template-areas` sono le seguenti:

- Devi avere ogni cella della griglia riempita.
- Per estendersi su due celle, ripeti il nome.
- Per lasciare una cella vuota, usa un `.` (punto).
- Le aree devono essere rettangolari — ad esempio, non puoi avere un'area a forma di L.
- Le aree non possono essere ripetute in posizioni diverse.

Puoi giocare con il nostro layout, modificando il footer in modo che si trovi solo sotto l'articolo e la sidebar si estenda per tutta la lunghezza. Questo è un modo molto bello di descrivere un layout perché è chiaro, semplicemente guardando il CSS, sapere esattamente cosa sta succedendo.

## Nidificazione delle griglie e subgriglia

È possibile nidificare una griglia all'interno di un'altra griglia, creando una ["subgriglia"](/it/docs/Web/CSS/CSS_grid_layout/Subgrid).
Puoi farlo impostando il `display: grid` su un elemento nella griglia principale.

Ampliamo l'esempio precedente aggiungendo un contenitore per articoli e usando una griglia nidificata per controllare il layout di più articoli.
Mentre stiamo usando solo una colonna nella griglia nidificata, possiamo definire le righe da dividere in un rapporto 4:3:3 usando la proprietà `grid-template-rows`.
Questo approccio ci consente di creare un layout in cui un articolo in cima alla pagina ha una visualizzazione grande, mentre gli altri hanno un layout più piccolo, simile a un'anteprima.

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

Per facilitare il lavoro con layout in griglie nidificate, puoi usare `subgrid` sulle proprietà `grid-template-rows` e `grid-template-columns`. Questo ti permette di sfruttare i tracciati definiti nella griglia principale.

Nel seguente esempio, stiamo usando il [posizionamento basato sulle linee](#posizionamento_basato_sulle_linee), permettendo alla griglia nidificata di estendersi su più colonne e righe della griglia principale.
Abbiamo aggiunto `subgrid` per ereditare i tracciati delle colonne della griglia principale, aggiungendo un layout differente per le righe all'interno della griglia nidificata.

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

## Frameworks a griglia

Sono disponibili numerosi frameworks a griglia, che offrono una griglia di 12 o 16 colonne, per aiutare a disporre i tuoi contenuti.
La buona notizia è che probabilmente non avrai bisogno di framework di terze parti per aiutarti a creare layout basati su griglia — la funzionalità di griglia è già inclusa nella specifica ed è supportata dalla maggior parte dei browser moderni.

Questo ha un contenitore con una griglia definita di 12 colonne, usando `grid-template-columns: repeat(12, 1fr);`, e lo stesso markup usato negli ultimi due esempi. Possiamo ora usare il posizionamento basato sulle linee per posizionare i nostri contenuti sulla griglia di 12 colonne.

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

Se usi l'[ispettore di griglie di Firefox](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/how_to/examine_grid_layouts/index.html) per sovrapporre le linee della griglia sul tuo design, puoi vedere come funziona la nostra griglia a 12 colonne.

![Una griglia di 12 colonne sovrapposta al nostro design.](learn-grids-inspector.png)

## Metti alla prova le tue abilità!

Sei arrivato alla fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare alcuni ulteriori test per verificare che tu abbia trattenuto queste informazioni prima di passare oltre — vedi [Test your skills: Grid](/it/docs/Learn_web_development/Core/CSS_layout/Test_your_skills/Grid).

## Sommario

In questa panoramica, abbiamo fatto un tour delle principali caratteristiche del layout a griglia CSS. Dovresti essere in grado di iniziare a usarlo nei tuoi progetti. Nel prossimo argomento parleremo del design responsivo.

## Vedi anche

- [Layout a griglia CSS](/it/docs/Web/CSS/CSS_grid_layout#guides)
  - : Pagina principale del modulo CSS Grid Layout, contenente molte risorse aggiuntive
- [Una guida completa alla griglia CSS](https://css-tricks.com/snippets/css/complete-guide-grid/)
  - : Una guida visiva su CSS-Tricks (2023).
- [Grid Garden](https://cssgridgarden.com/)
  - : Un gioco educativo per imparare e comprendere meglio i concetti di base della griglia su cssgridgarden.com.

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Flexbox", "Learn_web_development/Core/CSS_layout/Responsive_design", "Learn_web_development/Core/CSS_layout")}}
