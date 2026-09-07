---
title: Layout a griglia CSS
slug: Learn_web_development/Core/CSS_layout/Grids
l10n:
  sourceCommit: b5ee197a87ea18acbc4dd9544efa8c0e46253785
---

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Test_your_skills/Flexbox", "Learn_web_development/Core/CSS_layout/Test_your_skills/Grid", "Learn_web_development/Core/CSS_layout")}}

Il layout a griglia CSS è un sistema di layout bidimensionale per il web. Permette di organizzare il contenuto in righe e colonne e offre molte funzionalità per semplificare la creazione di layout complessi. Questo articolo spiega tutto ciò che serve per iniziare a usare il layout a griglia.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Strutturare contenuti con HTML</a
        >,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Basi dello styling CSS</a>,
        <a href="/it/docs/Learn_web_development/Core/Text_styling/Fundamentals">Fondamenti dello stile di testo e caratteri</a>,
        familiarità con i <a href="/it/docs/Learn_web_development/Core/CSS_layout/Introduction">concetti fondamentali del layout CSS</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere lo scopo di CSS Grid — disporre in modo flessibile un insieme di elementi block o inline in due dimensioni.</li>
          <li>Comprendere la terminologia delle griglie — righe, colonne, spazi e canaline.</li>
          <li>Comprendere cosa fornisce per impostazione predefinita <code>display: grid</code>.</li>
          <li>Definire righe, colonne e spazi della griglia.</li>
          <li>Posizionare gli elementi sulla griglia.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Che cos'è il layout a griglia?

Una griglia è un insieme di linee orizzontali e verticali che crea uno schema rispetto al quale è possibile allineare gli elementi del design. Aiuta a creare layout in cui gli elementi non si spostano né cambiano larghezza passando da una pagina all'altra, garantendo una maggiore coerenza nei siti web.

Una griglia dispone tipicamente di **colonne**, **righe** e spazi tra ogni riga e colonna. Questi spazi sono comunemente chiamati **canaline**.

![Griglia CSS con le parti etichettate come righe, colonne e canaline. Le righe sono i segmenti orizzontali della griglia e le colonne sono i segmenti verticali della griglia. Lo spazio tra due righe è chiamato "canalina di riga" e lo spazio tra 2 colonne è chiamato "canalina di colonna".](grid.png)

## Creare una griglia in CSS

Dopo aver scelto la griglia richiesta dal design, è possibile usare il layout a griglia CSS per crearla. Esamineremo prima le funzionalità di base del layout a griglia, quindi vedremo come creare un semplice sistema a griglia per il progetto.
Il video seguente offre una buona spiegazione visiva dell'uso di CSS grid:

{{EmbedYouTube("KOvGeFUHAC0")}}

### Definire una griglia

Proviamo i layout a griglia. Ecco un esempio con un contenitore che contiene alcuni elementi figli. Per impostazione predefinita, questi elementi sono visualizzati nel flusso normale, quindi appaiono uno sotto l'altro.

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

Analogamente a come si definisce flexbox, si definisce un layout a griglia impostando il valore della proprietà {{cssxref("display")}} su `grid`. Come nel caso di flexbox, la proprietà `display: grid` trasforma tutti i figli diretti del contenitore in elementi della griglia. Abbiamo aggiunto il seguente CSS al file:

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

A differenza di flexbox, gli elementi non avranno subito un aspetto diverso. Dichiarare `display: grid` fornisce una griglia a una colonna, quindi gli elementi continueranno a essere visualizzati uno sotto l'altro, come nel flusso normale.

Per vedere qualcosa che assomigli maggiormente a una griglia, sarà necessario aggiungere alcune colonne alla griglia. Aggiungiamo tre colonne di 200 pixel. Per creare queste tracce di colonna è possibile usare qualsiasi unità di lunghezza o percentuale.

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

Dovrebbe essere possibile vedere che gli elementi si sono riorganizzati, con uno in ogni cella della griglia.

{{EmbedLiveSample('simple-grid_2', '100%', "130") }}

## Riepilogo interattivo dei concetti di griglia

Il seguente contenuto incorporato di Scrimba<sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> offre una lezione interattiva sulle basi di CSS grid. Include inoltre un esempio di griglia live con cui è possibile interagire per vedere come funziona il codice.

<mdn-scrim-inline url="https://scrimba.com/learn-css-grid-c02k/~01" scrimtitle="La prima griglia"></mdn-scrim-inline>

### Griglie flessibili con l'unità fr

Oltre a creare griglie usando lunghezze e percentuali, è possibile usare [`fr`](/it/docs/Web/CSS/Reference/Values/flex_value). L'unità `fr` rappresenta una frazione dello spazio disponibile nel contenitore della griglia, per dimensionare in modo flessibile righe e colonne della griglia.

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

Qui modifichiamo l'elenco delle tracce con la definizione seguente, creando tre tracce `1fr`:

```css live-sample___grid-fr-unit_0
.container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
}
```

{{EmbedLiveSample('grid-fr-unit_0', '100%', "130") }}

Ora sono disponibili tracce flessibili.
L'unità `fr` distribuisce lo spazio in modo proporzionale, quindi è possibile specificare diversi valori positivi per le tracce.
Modificare l'elenco delle tracce con la definizione seguente, creando una traccia `2fr` e due tracce `1fr`:

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

La prima traccia riceve `2fr` dello spazio disponibile e le altre due tracce ricevono `1fr`, rendendo più grande la prima traccia. È possibile combinare unità `fr` con unità di lunghezza fissa. In questo caso, viene prima utilizzato lo spazio necessario per le tracce fisse, prima di distribuire lo spazio rimanente alle altre tracce.

> [!NOTE]
> L'unità `fr` distribuisce lo spazio _disponibile_, non _tutto_ lo spazio. Pertanto, se una delle tracce contiene qualcosa di grande, ci sarà meno spazio libero da condividere.

### Spazi tra le tracce

Per creare spazi tra le tracce, si usano le proprietà:

- {{cssxref("column-gap")}} per gli spazi tra le colonne
- {{cssxref("row-gap")}} per gli spazi tra le righe
- {{cssxref("gap")}} come abbreviazione per entrambe

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

Qui aggiungiamo la proprietà `gap` per creare spazi di `20px` tra le tracce:

```css live-sample___grid-gap
.container {
  display: grid;
  grid-template-columns: 2fr 1fr 1fr;
  gap: 20px;
}
```

{{EmbedLiveSample('grid-gap', '100%', "180") }}

Questi spazi possono usare qualsiasi unità di lunghezza o percentuale, ma non un'unità `fr`.

### Ripetere elenchi di tracce

È possibile ripetere tutto o soltanto una sezione dell'elenco delle tracce usando la funzione CSS `repeat()`.
Qui modifichiamo l'elenco delle tracce come segue:

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

Ora si otterranno tre tracce `1fr`, proprio come prima. Il primo valore passato alla funzione `repeat()` specifica il numero di volte in cui ripetere l'elenco, mentre il secondo valore è un elenco di tracce, che può contenere una o più tracce da ripetere.

### Griglie implicite ed esplicite

Finora sono state specificate soltanto tracce di colonna, ma le righe vengono create automaticamente per contenere il contenuto. Questo concetto evidenzia la distinzione tra griglie esplicite e implicite.
Ecco ulteriori dettagli sulla differenza tra i due tipi di griglia:

- La **griglia esplicita** viene creata usando `grid-template-columns` o `grid-template-rows`.
- La **griglia implicita** estende la griglia esplicita definita quando il contenuto viene posizionato al di fuori di quella griglia, ad esempio nelle righe, tracciando linee della griglia aggiuntive.

Per impostazione predefinita, le tracce create nella griglia implicita sono dimensionate come `auto`, il che in generale significa che sono abbastanza grandi da contenere il loro contenuto. Per assegnare una dimensione alle tracce della griglia implicita, è possibile usare le proprietà {{cssxref("grid-auto-rows")}} e {{cssxref("grid-auto-columns")}}. Se si aggiunge `grid-auto-rows` con un valore di `100px` al CSS, sarà possibile vedere che le righe create hanno ora un'altezza di 100 pixel.

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

Le tracce alte 100 pixel non saranno molto utili se viene aggiunto a tali tracce un contenuto più alto di 100 pixel, poiché ciò causerebbe un overflow. Potrebbe essere meglio avere tracce alte _almeno_ 100 pixel, che possano comunque espandersi quando viene aggiunto altro contenuto. Un fatto abbastanza basilare del web è che non è mai possibile sapere davvero quanto sarà alto un elemento: contenuti aggiuntivi o dimensioni dei caratteri maggiori possono causare problemi ai design che tentano di essere perfetti al pixel in ogni dimensione.

La funzione {{cssxref("minmax", "minmax()")}} permette di impostare una dimensione minima e massima per una traccia, ad esempio `minmax(100px, auto)`. La dimensione minima è di 100 pixel, ma la massima è `auto`, che si espanderà per adattarsi a ulteriore contenuto. Qui modifichiamo `grid-auto-rows` per usare un valore `minmax()`:

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

Aggiungendo contenuto aggiuntivo, sarà possibile vedere che la traccia si espande per consentirne l'adattamento. Si noti che l'espansione avviene lungo la riga.

### Tante colonne quante ne possono entrare

È possibile combinare alcune delle nozioni apprese sugli elenchi di tracce, sulla notazione di ripetizione e su {{cssxref("minmax", "minmax()")}} per creare un modello utile. A volte è utile poter chiedere a CSS grid di creare il maggior numero possibile di colonne che entrino nel contenitore. Questo si fa impostando il valore di `grid-template-columns` usando la funzione {{cssxref("repeat", "repeat()")}}, ma invece di passare un numero, si passa la parola chiave [`auto-fit`](/it/docs/Web/CSS/Reference/Values/repeat#auto-fit). Per il secondo parametro della funzione si usa `minmax()` con un valore minimo uguale alla dimensione minima desiderata per la traccia e un massimo di `1fr`.

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

Questo funziona perché la griglia crea quante più colonne da 230 pixel possano entrare nel contenitore, quindi condivide lo spazio rimanente tra tutte le colonne. Il massimo è `1fr` che, come già noto, distribuisce lo spazio uniformemente tra le tracce.

## Posizionamento basato sulle linee

Passiamo ora dalla creazione di una griglia al posizionamento degli elementi sulla griglia. La griglia contiene sempre linee: queste sono numerate a partire da 1 e dipendono dalla [modalità di scrittura](/it/docs/Web/CSS/Guides/Writing_modes) del documento. Ad esempio, la linea di colonna 1 in inglese (scritto da sinistra a destra) si trova sul lato sinistro della griglia e la linea di riga 1 in alto, mentre in arabo (scritto da destra a sinistra), la linea di colonna 1 si trova sul lato destro.

Per posizionare gli elementi lungo queste linee, è possibile specificare le linee iniziale e finale dell'area della griglia in cui deve essere posizionato un elemento. A questo scopo si possono usare quattro proprietà:

- {{cssxref("grid-column-start")}}
- {{cssxref("grid-column-end")}}
- {{cssxref("grid-row-start")}}
- {{cssxref("grid-row-end")}}

Queste proprietà accettano numeri di linea come valori, quindi è possibile specificare, ad esempio, che un elemento debba iniziare alla linea 1 e terminare alla linea 3.
In alternativa, si possono anche usare proprietà abbreviate che permettono di specificare contemporaneamente le linee iniziale e finale, separate da una barra `/`:

- {{cssxref("grid-column")}} è l'abbreviazione di `grid-column-start` e `grid-column-end`
- {{cssxref("grid-row")}} è l'abbreviazione di `grid-row-start` e `grid-row-end`

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

Senza un posizionamento definito, è possibile vedere che il _posizionamento automatico_ colloca ciascun elemento nella propria cella della griglia. L'elemento {{htmlelement("header")}} occupa `1fr` (un quarto) e l'elemento {{htmlelement("main")}} occupa `3fr` (tre quarti).

{{EmbedLiveSample('grid-placement_0', '100%', "230") }}

Disponiamo tutti gli elementi del sito usando le linee della griglia. Aggiungere le seguenti regole alla fine del CSS:

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

Ora {{htmlelement("header")}} e {{htmlelement("footer")}} sono impostati su `1 / 3`, ossia iniziano alla linea `1` e terminano alla linea `3`.

{{EmbedLiveSample('grid-placement_1', '100%', "230") }}

> [!NOTE]
> È anche possibile usare il valore `-1` per selezionare la linea finale di colonna o riga, quindi contare verso l'interno a partire dalla fine usando valori negativi. Si noti inoltre che le linee vengono sempre contate dai bordi della griglia esplicita, non dalla {{Glossary("Grid", "griglia implicita")}}.

## Posizionamento con grid-template-areas

Un modo alternativo per disporre gli elementi sulla griglia consiste nell'usare la proprietà {{cssxref("grid-template-areas")}} e assegnare un nome ai vari elementi del design.

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

Qui usiamo la proprietà {{CSSXRef("grid-template-areas")}} per definire come sono disposte le 3 righe. La prima riga ha un valore di `header header`, la seconda di `sidebar content` e la terza di `footer footer`. Usiamo poi la proprietà {{CSSXRef("grid-area")}} per definire dove vengono posizionati gli elementi in `grid-template-areas`.

{{EmbedLiveSample('grid-placement_2', '100%', "230") }}

Le regole per `grid-template-areas` sono le seguenti:

- È necessario riempire ogni cella della griglia.
- Per estendersi su due celle, ripetere il nome.
- Per lasciare una cella vuota, usare un `.` (punto).
- Le aree devono essere rettangolari: ad esempio, non può esistere un'area a forma di L.
- Le aree non possono essere ripetute in posizioni diverse.

È possibile sperimentare con il layout, modificando il footer affinché si trovi solo sotto l'articolo e facendo estendere la sidebar fino in fondo. Questo è un ottimo modo per descrivere un layout perché, osservando il CSS, è chiaro esattamente cosa accade.

## Nidificare griglie e subgrid

È possibile nidificare una griglia all'interno di un'altra griglia, creando una ["subgrid"](/it/docs/Web/CSS/Guides/Grid_layout/Subgrid).
È possibile farlo impostando la proprietà `display: grid` su un elemento nella griglia padre.

Espandiamo l'esempio precedente aggiungendo un contenitore per gli articoli e usando una griglia nidificata per controllare il layout di più articoli.
Sebbene nella griglia nidificata si usi solo una colonna, è possibile definire la divisione delle righe in un rapporto 4:3:3 usando la proprietà `grid-template-rows`.
Questo approccio consente di creare un layout in cui un articolo nella parte superiore della pagina viene visualizzato in grande, mentre gli altri hanno un layout più piccolo, simile a un'anteprima.

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

Per semplificare il lavoro con i layout nelle griglie nidificate, è possibile usare `subgrid` sulle proprietà `grid-template-rows` e `grid-template-columns`. Ciò consente di sfruttare le tracce definite nella griglia padre.

Nell'esempio seguente, viene usato il [posizionamento basato sulle linee](#posizionamento_basato_sulle_linee), consentendo alla griglia nidificata di estendersi su più colonne e righe della griglia padre.
È stato aggiunto `subgrid` per ereditare le tracce di colonna della griglia padre, aggiungendo al contempo un layout differente per le righe nella griglia nidificata.

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

## Framework per griglie

Sono disponibili numerosi framework per griglie: sistemi CSS predefiniti che
offrono funzionalità quali griglie a 12 o 16 colonne, classi di utilità per spaziatura e allineamento, e
design responsive tramite breakpoint.

La buona notizia è che probabilmente non saranno necessarie soluzioni proprietarie per creare layout basati su griglie: tutti i browser moderni supportano lo standard CSS grid.

L'esempio seguente mostra una versione semplificata dell'aspetto che potrebbe avere tale codice. Include un contenitore con una griglia a 12 colonne definita, usando `grid-template-columns: repeat(12, 1fr);`, e lo stesso markup usato nei due esempi precedenti. Ora è possibile usare il posizionamento basato sulle linee per collocare il contenuto nella griglia a 12 colonne.

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

Usando l'[ispettore della griglia di Firefox](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/how_to/examine_grid_layouts/index.html) per sovrapporre le linee della griglia al design, è possibile osservare come funziona la griglia a 12 colonne.

![Una griglia a 12 colonne sovrapposta al design.](learn-grids-inspector.png)

## Riepilogo

In questa panoramica sono state esaminate le principali funzionalità del layout a griglia CSS. A questo punto dovrebbe essere possibile iniziare a usarlo nei propri design.

Nel prossimo articolo verranno proposti alcuni test che consentono di verificare quanto bene siano state comprese e memorizzate tutte queste informazioni.

## Vedi anche

- [Layout a griglia CSS](/it/docs/Web/CSS/Guides/Grid_layout)
  - : La pagina principale del modulo di layout a griglia CSS, contenente molte altre risorse.
- [Guida al layout a griglia CSS](https://css-tricks.com/complete-guide-css-grid-layout/)
  - : Una guida visiva su CSS-Tricks (2021).
- [Grid Garden](https://cssgridgarden.com/)
  - : Un gioco educativo per imparare e comprendere meglio le basi delle griglie su cssgridgarden.com.

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Test_your_skills/Flexbox", "Learn_web_development/Core/CSS_layout/Test_your_skills/Grid", "Learn_web_development/Core/CSS_layout")}}
