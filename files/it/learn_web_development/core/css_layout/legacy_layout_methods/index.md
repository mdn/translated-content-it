---
title: Metodi di layout legacy
slug: Learn_web_development/Core/CSS_layout/Legacy_Layout_Methods
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

I sistemi a griglia sono una caratteristica molto comune nei layout CSS, e prima del layout a griglia CSS, tendevano ad essere implementati usando i float o altre funzionalità di layout. Si immagina il layout come un numero fisso di colonne (ad esempio, 4, 6 o 12) e poi si adattano le colonne di contenuti all'interno di queste colonne immaginarie. In questo articolo esploreremo come funzionano questi metodi più vecchi, in modo da comprendere come venivano utilizzati se si lavora su un progetto più vecchio.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Nozioni di base su HTML (studiare
        <a href="/it/docs/Learn_web_development/Core/Structuring_content">Introduzione a HTML</a>), e un'idea di come funziona CSS (studiare
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Nozioni di base su CSS Styling</a>.)
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere i concetti fondamentali dietro i sistemi di layout a griglia utilizzati prima che il layout a griglia CSS fosse disponibile nei browser.
      </td>
    </tr>
  </tbody>
</table>

## Layout e sistemi a griglia prima del layout a griglia CSS

Può sembrare sorprendente per chi proviene da un background di design che CSS non avesse un sistema a griglia integrato fino a tempi molto recenti, e invece sembrava che si utilizzassero una varietà di metodi sub-ottimali per creare design a griglia. Ora ci riferiamo a questi come metodi "legacy".

Per nuovi progetti, nella maggior parte dei casi, il layout a griglia CSS sarà utilizzato in combinazione con uno o più metodi di layout moderni per formare la base per qualsiasi layout. Tuttavia, di tanto in tanto si incontreranno "sistemi a griglia" che utilizzano questi metodi legacy. Vale la pena comprendere come funzionano e perché sono diversi dal layout a griglia CSS.

Questa lezione spiegherà come funzionano i sistemi a griglia e i framework a griglia basati su float e flexbox. Dopo aver studiato il layout a griglia, probabilmente sarà sorprendente quanto tutto ciò sembri complicato! Questa conoscenza sarà utile se è necessario creare del codice di fallback per browser che non supportano i metodi più recenti, oltre a consentire di lavorare sui progetti esistenti che utilizzano questi tipi di sistemi.

Vale la pena tenere a mente, mentre esploriamo questi sistemi, che nessuno di loro crea effettivamente una griglia nel modo in cui il layout a griglia CSS crea una griglia. Funzionano fornendo agli elementi una dimensione e spingendoli intorno per allinearli in un modo che _sembra_ una griglia.

## Un layout a due colonne

Iniziamo con l'esempio più semplice possibile: un layout a due colonne. Può seguire creando un nuovo file `index.html` sul suo computer, riempendolo con un [template HTML semplice](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/getting-started/index.html) e inserendo il codice sottostante nei posti appropriati. Alla fine della sezione, può vedere un esempio live di come dovrebbe apparire il codice finale.

Prima di tutto, abbiamo bisogno di un po' di contenuto da inserire nelle nostre colonne. Sostituire ciò che è attualmente nel corpo con quanto segue:

```html
<h1>2 column layout example</h1>
<div>
  <h2>First column</h2>
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
</div>

<div>
  <h2>Second column</h2>
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

Ognuna delle colonne necessita di un elemento esterno per contenere il suo contenuto e permetterci di manipolarlo tutto in una volta. In questo caso esempio abbiamo scelto {{htmlelement("div")}}, ma potrebbe scegliere qualcosa di più semanticamente appropriato come {{htmlelement("article")}}, {{htmlelement("section")}}, e {{htmlelement("aside")}}, o qualsiasi altra cosa.

Ora per il CSS. Prima di tutto, applichi quanto segue al suo HTML per fornire alcune configurazioni di base:

```css
body {
  width: 90%;
  max-width: 900px;
  margin: 0 auto;
}
```

Il corpo sarà largo il 90% del viewport fino a quando non raggiungerà 900px di larghezza, nel qual caso rimarrà fisso a questa larghezza e si centrerà nel viewport. Per impostazione predefinita, i suoi figli (il {{htmlelement("Heading_Elements", "h1")}} e i due {{htmlelement("div")}}) si estenderanno per il 100% della larghezza del corpo. Se vogliamo che i due {{htmlelement("div")}} siano flottati uno accanto all'altro, dobbiamo impostare le loro larghezze per un totale del 100% della larghezza del loro elemento genitore o minore in modo che possano stare uno accanto all'altro. Aggiunga quanto segue alla fine del suo CSS:

```css
div:nth-of-type(1) {
  width: 48%;
}

div:nth-of-type(2) {
  width: 48%;
}
```

Qui abbiamo impostato entrambi per essere il 48% della larghezza del loro genitore - questo totalizza il 96%, lasciandoci un 4% libero per fungere da canaletta tra le due colonne, dando un po' di spazio ai contenuti. Ora dobbiamo soltanto far flotare le colonne, in questo modo:

```css
div:nth-of-type(1) {
  width: 48%;
  float: left;
}

div:nth-of-type(2) {
  width: 48%;
  float: right;
}
```

Mettendo tutto insieme dovrebbe ottenere un risultato come il seguente:

{{ EmbedLiveSample('A_two_column_layout', '100%', 520) }}

Noterà qui che stiamo usando percentuali per tutte le larghezze - questa è una strategia piuttosto buona, poiché crea un layout liquido, uno che si adatta a diverse dimensioni di schermo e mantiene le stesse proporzioni per le larghezze delle colonne a dimensioni di schermo più piccole. Provi a regolare la larghezza della finestra del suo browser per vedere da solo. Questo è uno strumento prezioso per il web design responsivo.

> [!NOTE]
> Può vedere questo esempio in esecuzione su [0_two-column-layout.html](https://mdn.github.io/learning-area/css/css-layout/floats/0_two-column-layout.html) (vedere anche [il codice sorgente](https://github.com/mdn/learning-area/blob/main/css/css-layout/floats/0_two-column-layout.html)).

## Creare semplici framework a griglia legacy

La maggior parte dei framework legacy utilizza il comportamento della proprietà {{cssxref("float")}} per far galleggiare una colonna accanto ad un'altra per creare qualcosa che assomiglia a una griglia. Lavorare attraverso il processo di creazione di una griglia con i float mostra come funziona e introduce anche alcuni concetti più avanzati per costruire su ciò che ha appreso nella lezione su [float e pulizia](/it/docs/Learn_web_development/Core/CSS_layout/Floats).

Il tipo di framework a griglia più facile da creare è uno a larghezza fissa — dobbiamo solo calcolare quanto vogliamo che sia la larghezza totale del nostro design, quante colonne vogliamo, e quanto dovrebbero essere larghe le canalette e le colonne. Se invece decidessimo di disporre il nostro design su una griglia con colonne che crescono e si restringono in base alla larghezza del browser, dovremmo calcolare le larghezze percentuali per le colonne e le canalette tra di esse.

Nelle prossime sezioni esamineremo come creare entrambi i tipi. Creeremo una griglia a 12 colonne — una scelta molto comune che si vede essere molto adattabile a diverse situazioni dato che 12 è divisibile in maniera comoda per 6, 4, 3, e 2.

### Una semplice griglia a larghezza fissa

Per prima cosa, creiamo un sistema a griglia che utilizza colonne a larghezza fissa.

Inizi col fare una copia locale del nostro file di esempio [simple-grid.html](https://github.com/mdn/learning-area/blob/main/css/css-layout/grids/simple-grid.html), che contiene il seguente markup nel suo corpo.

```html
<div class="wrapper">
  <div class="row">
    <div class="col">1</div>
    <div class="col">2</div>
    <div class="col">3</div>
    <div class="col">4</div>
    <div class="col">5</div>
    <div class="col">6</div>
    <div class="col">7</div>
    <div class="col">8</div>
    <div class="col">9</div>
    <div class="col">10</div>
    <div class="col">11</div>
    <div class="col">12</div>
  </div>
  <div class="row">
    <div class="col span1">13</div>
    <div class="col span6">14</div>
    <div class="col span3">15</div>
    <div class="col span2">16</div>
  </div>
</div>
```

L'obiettivo è trasformarlo in una griglia dimostrativa di due righe su una griglia di dodici colonne — la riga superiore dimostra la dimensione delle singole colonne, la seconda riga diverse aree di dimensioni sulla griglia.

![Griglia CSS con 16 elementi della griglia distribuiti su dodici colonne e due righe. La prima riga ha 12 elementi della griglia di uguale larghezza in 12 colonne. La seconda riga ha elementi della griglia di diverse dimensioni. L'elemento 13 si estende su 1 colonna, l'elemento 14 si estende su sei colonne, il 15 su tre, e il 16 su due.](simple-grid-finished.png)

Nell'elemento {{htmlelement("style")}}, aggiunge il seguente codice, che dà al contenitore wrapper una larghezza di 980 pixel, con un padding sul lato destro di 20 pixel. Questo ci lascia con 960 pixel per le larghezze totali colonne/canalette — in questo caso, il padding è sottratto dalla larghezza totale del contenuto perché abbiamo impostato {{cssxref("box-sizing")}} su `border-box` su tutti gli elementi nel sito (vedere [Il modello di scatola alternativo CSS](/it/docs/Learn_web_development/Core/Styling_basics/Box_model#the_alternative_css_box_model) per ulteriori spiegazioni).

```css
* {
  box-sizing: border-box;
}

body {
  width: 980px;
  margin: 0 auto;
}

.wrapper {
  padding-right: 20px;
}
```

Ora usi il contenitore di riga che è avvolto attorno a ogni riga della griglia per pulire una riga dall'altra. Aggiunge la seguente regola sotto la precedente:

```css
.row {
  clear: both;
}
```

Applicare questa pulizia significa che non è necessario riempire completamente ogni riga con elementi per raggiungere le dodici colonne complete. Le righe rimarranno separate e non interferiranno tra loro.

Le canalette tra le colonne sono larghe 20 pixel. Creiamo queste canalette come un margine sul lato sinistro di ogni colonna — compresa la prima colonna, per bilanciare i 20 pixel di padding sul lato destro del contenitore. Quindi abbiamo in totale 12 canalette — 12 x 20 = 240.

Dobbiamo sottrarlo dalla nostra larghezza totale di 960 pixel, lasciandoci 720 pixel per le nostre colonne. Se ora dividiamo questo per 12, sappiamo che ogni colonna dovrebbe essere larga 60 pixel.

Il nostro passaggio successivo è creare una regola per la classe `.col`, facendola galleggiare a sinistra, dandole un {{cssxref("margin-left")}} di 20 pixel per formare la canaletta, e una {{cssxref("width")}} di 60 pixel. Aggiunge la seguente regola alla fine del suo CSS:

```css
.col {
  float: left;
  margin-left: 20px;
  width: 60px;
  background: rgb(255 150 150);
}
```

La riga superiore di colonne singole ora si disporrà ordinatamente come una griglia.

> [!NOTE]
> Abbiamo anche dato a ogni colonna un colore rosso chiaro in modo da poter vedere esattamente quanto spazio occupi ciascuna.

I contenitori di layout che vogliamo estendere oltre una colonna richiedono classi speciali per regolare i loro valori di {{cssxref("width")}} al numero di colonne richieste (più canalette tra di esse). Abbiamo bisogno di creare una classe aggiuntiva per consentire ai contenitori di estendersi da 2 a 12 colonne. Ogni larghezza è il risultato di sommare la larghezza delle colonne di quel numero di colonne più le larghezze delle canalette, che saranno sempre uno in meno rispetto al numero di colonne.

Aggiunga quanto segue alla fine del suo CSS:

```css
/* Two column widths (120px) plus one gutter width (20px) */
.col.span2 {
  width: 140px;
}
/* Three column widths (180px) plus two gutter widths (40px) */
.col.span3 {
  width: 220px;
}
/* And so on… */
.col.span4 {
  width: 300px;
}
.col.span5 {
  width: 380px;
}
.col.span6 {
  width: 460px;
}
.col.span7 {
  width: 540px;
}
.col.span8 {
  width: 620px;
}
.col.span9 {
  width: 700px;
}
.col.span10 {
  width: 780px;
}
.col.span11 {
  width: 860px;
}
.col.span12 {
  width: 940px;
}
```

Con queste classi create, possiamo ora disporre colonne di diverse larghezze sulla griglia. Provi a salvare e caricare la pagina nel suo browser per vedere gli effetti.

> [!NOTE]
> Se sta avendo difficoltà a far funzionare l'esempio sopra, provi a confrontarlo con la nostra [versione finale](https://github.com/mdn/learning-area/blob/main/css/css-layout/grids/simple-grid-finished.html) su GitHub (vedi anche [in esecuzione live](https://mdn.github.io/learning-area/css/css-layout/grids/simple-grid-finished.html)).

Provi a modificare le classi sui suoi elementi o anche ad aggiungere e rimuovere alcuni contenitori, per vedere come può variare il layout. Ad esempio, potrebbe far sembrare la seconda riga così:

```html
<div class="row">
  <div class="col span8">13</div>
  <div class="col span4">14</div>
</div>
```

Ora che ha un sistema di griglia funzionante, può definire le righe e il numero di colonne in ogni riga, quindi riempire ogni contenitore con il contenuto richiesto. Ottimo!

### Creare una griglia fluida

La nostra griglia funziona bene, ma ha una larghezza fissa. Vogliamo davvero una griglia flessibile (fluida) che si espanderà e si restringerà con lo spazio disponibile nel {{Glossary("viewport", "viewport")}} del browser. Per ottenere ciò possiamo trasformare le larghezze dei pixel di riferimento in percentuali.

L'equazione che trasforma una larghezza fissa in una flessibile basata su percentuali è la seguente.

```plain
target / context = result
```

Per la nostra larghezza della colonna, la **larghezza target** è di 60 pixel e il **contesto** è il wrapper da 960 pixel. Possiamo usare quanto segue per calcolare una percentuale.

```plain
60 / 960 = 0.0625
```

Poi spostiamo il punto decimale 2 posizioni, dandoci una percentuale del 6.25%. Quindi, nel nostro CSS possiamo sostituire la larghezza della colonna di 60 pixel con il 6.25%.

Dobbiamo fare lo stesso con la larghezza della nostra canaletta:

```plain
20 / 960 = 0.02083333333
```

Quindi dobbiamo sostituire i 20 pixel di {{cssxref("margin-left")}} sulla nostra regola `.col` e i 20 pixel di {{cssxref("padding-right")}} su `.wrapper` con il 2.08333333%.

#### Aggiornare la nostra griglia

Per iniziare in questa sezione, faccia una nuova copia della sua precedente pagina di esempio, o faccia una copia locale del nostro codice [simple-grid-finished.html](https://github.com/mdn/learning-area/blob/main/css/css-layout/grids/simple-grid-finished.html) da usare come punto di partenza.

Aggiorni la seconda regola CSS (con il selettore `.wrapper`) come segue:

```css
body {
  width: 90%;
  max-width: 980px;
  margin: 0 auto;
}

.wrapper {
  padding-right: 2.08333333%;
}
```

Non solo gli abbiamo dato una {{cssxref("width")}} percentuale, ma abbiamo anche aggiunto una proprietà {{cssxref("max-width")}} per evitare che il layout diventi troppo largo.

Successivamente, aggiorni la quarta regola CSS (con il selettore `.col`):

```css
.col {
  float: left;
  margin-left: 2.08333333%;
  width: 6.25%;
  background: rgb(255 150 150);
}
```

Ora arriva la parte leggermente più laboriosa — dobbiamo aggiornare tutte le nostre regole `.col.span` per usare percentuali invece di larghezze in pixel. Questo richiede un po' di tempo con una calcolatrice; per risparmiarle un po' di fatica, lo abbiamo fatto per lei qui sotto.

Aggiorni il blocco inferiore delle regole CSS con il seguente:

```css
/* Two column widths (12.5%) plus one gutter width (2.08333333%) */
.col.span2 {
  width: 14.58333333%;
}
/* Three column widths (18.75%) plus two gutter widths (4.1666666) */
.col.span3 {
  width: 22.91666666%;
}
/* And so on… */
.col.span4 {
  width: 31.24999999%;
}
.col.span5 {
  width: 39.58333332%;
}
.col.span6 {
  width: 47.91666665%;
}
.col.span7 {
  width: 56.24999998%;
}
.col.span8 {
  width: 64.58333331%;
}
.col.span9 {
  width: 72.91666664%;
}
.col.span10 {
  width: 81.24999997%;
}
.col.span11 {
  width: 89.5833333%;
}
.col.span12 {
  width: 97.91666663%;
}
```

Ora salvi il suo codice, lo carichi in un browser e provi a modificare la larghezza del viewport — dovrebbe vedere le larghezze delle colonne adeguarsi piacevolmente alle necessità.

> [!NOTE]
> Se sta avendo difficoltà a far funzionare l'esempio sopra, provi a confrontarlo con la nostra [versione finale su GitHub](https://github.com/mdn/learning-area/blob/main/css/css-layout/grids/fluid-grid.html) (vedi anche [in esecuzione live](https://mdn.github.io/learning-area/css/css-layout/grids/fluid-grid.html)).

### Calcoli più facili usando la funzione calc()

Può usare la funzione {{cssxref("calc", "calc()")}} per fare matematica direttamente nel suo CSS — questo le consente di inserire semplici equazioni matematiche nei valori CSS, per calcolare quale dovrebbe essere un valore. È particolarmente utile quando ci sono calcoli complessi da fare, e può persino calcolare un calcolo che utilizza unità diverse, ad esempio "Voglio che l'altezza di questo elemento sia sempre il 100% dell'altezza del suo genitore, meno 50px". Vedere [questo esempio da un tutorial sull'API MediaStream Recording](/it/docs/Web/API/MediaStream_Recording_API/Using_the_MediaStream_Recording_API#keeping_the_interface_constrained_to_the_viewport_regardless_of_device_height_with_calc).

Ad ogni modo, torniamo alle nostre griglie! Qualsiasi colonna che si estende su più di una colonna della nostra griglia ha una larghezza totale del 6.25% moltiplicata per il numero di colonne estese più il 2.08333333% moltiplicato per il numero di canalette (che sarà sempre uguale al numero di colonne meno 1). `calc()` ci permette di fare questo calcolo proprio all'interno del valore di larghezza, quindi per qualsiasi elemento che si estende su 4 colonne possiamo fare questo, ad esempio:

```css
.col.span4 {
  width: calc((6.25% * 4) + (2.08333333% * 3));
}
```

Provi a sostituire il suo blocco inferiore di regole con il seguente, poi lo ricarichi nel browser per vedere se ottiene lo stesso risultato:

```css
.col.span2 {
  width: calc((6.25% * 2) + 2.08333333%);
}
.col.span3 {
  width: calc((6.25% * 3) + (2.08333333% * 2));
}
.col.span4 {
  width: calc((6.25% * 4) + (2.08333333% * 3));
}
.col.span5 {
  width: calc((6.25% * 5) + (2.08333333% * 4));
}
.col.span6 {
  width: calc((6.25% * 6) + (2.08333333% * 5));
}
.col.span7 {
  width: calc((6.25% * 7) + (2.08333333% * 6));
}
.col.span8 {
  width: calc((6.25% * 8) + (2.08333333% * 7));
}
.col.span9 {
  width: calc((6.25% * 9) + (2.08333333% * 8));
}
.col.span10 {
  width: calc((6.25% * 10) + (2.08333333% * 9));
}
.col.span11 {
  width: calc((6.25% * 11) + (2.08333333% * 10));
}
.col.span12 {
  width: calc((6.25% * 12) + (2.08333333% * 11));
}
```

> [!NOTE]
> Può vedere la nostra versione finale in [fluid-grid-calc.html](https://github.com/mdn/learning-area/blob/main/css/css-layout/grids/fluid-grid-calc.html) (anche [vederla live](https://mdn.github.io/learning-area/css/css-layout/grids/fluid-grid-calc.html)).

### Sistemi a griglia semantici versus "unsemantic"

Aggiungere classi al suo markup per definire il layout significa che il suo contenuto e il markup diventano legati alla sua presentazione visiva. A volte sentirà questa pratica descritta come "unsemantic" — descrivendo come appare il contenuto — piuttosto che un uso semantico delle classi che descrive il contenuto. Questo è il caso delle nostre classi `span2`, `span3`, ecc.

Queste non sono l'unico approccio. Potrebbe invece decidere sulla sua griglia e poi aggiungere le informazioni di dimensione alle regole per le classi semantiche esistenti. Ad esempio, se avesse un {{htmlelement("div")}} con una classe `content` su di esso che vuole estendere su 8 colonne, potrebbe copiare attraverso la larghezza dalla classe `span8`, dandole una regola come così:

```css
.content {
  width: calc((6.25% * 8) + (2.08333333% * 7));
}
```

> [!NOTE]
> Se utilizzerà un preprocessore come [Sass](https://sass-lang.com/), potrebbe creare un semplice mixin per inserire quel valore per lei.

### Abilitare i contenitori offset nella nostra griglia

La griglia che abbiamo creato funziona bene finché vogliamo che tutti i contenitori iniziano a filo con il lato sinistro della griglia. Se volessimo lasciare uno spazio di colonna vuoto prima del primo contenitore — o tra i contenitori — avremmo bisogno di creare una classe offset per aggiungere un margine sinistro al nostro sito per spingerlo attraverso la griglia visivamente. Più matematica!

Proviamo questo.

Inizi con il suo codice precedente, o usi il nostro file [fluid-grid.html](https://github.com/mdn/learning-area/blob/main/css/css-layout/grids/fluid-grid.html) come punto di partenza.

Creiamo una classe nel nostro CSS che compenserà un elemento del contenitore di una larghezza di colonna. Aggiunga quanto segue alla fine del suo CSS:

```css
.offset-by-one {
  margin-left: calc(6.25% + (2.08333333% * 2));
}
```

Oppure, se preferisce calcolare le percentuali da solo, usi questo:

```css
.offset-by-one {
  margin-left: 10.41666666%;
}
```

Può ora aggiungere questa classe a qualsiasi contenitore vuole lasciare uno spazio vuoto di una colonna sul lato sinistro di esso. Ad esempio, se ha questo nel suo HTML:

```html
<div class="col span6">14</div>
```

Provi a sostituirlo con:

```html
<div class="col span5 offset-by-one">14</div>
```

> [!NOTE]
> Noti che deve ridurre il numero di colonne estese, per fare spazio all'offset!

Provi a caricare e aggiornare per vedere la differenza, o controlli il nostro esempio [fluid-grid-offset.html](https://github.com/mdn/learning-area/blob/main/css/css-layout/grids/fluid-grid-offset.html) (vedi anche [eseguito live](https://mdn.github.io/learning-area/css/css-layout/grids/fluid-grid-offset.html)). L'esempio finito dovrebbe apparire così:

![La griglia ha 2 righe. La prima riga ha 12 elementi di griglia di uguale larghezza e la seconda riga ha 4 elementi di larghezze diverse. L'elemento 13 si estende su 1 colonna, l'elemento 14 si estende su cinque colonne, il 15 su tre, e il 16 su due. L'elemento 14 ha applicata la classe "offset-by-one", il che significa che inizia nella 3ª colonna, piuttosto che nella seconda, lasciando un spazio vuoto largo una colonna nella seconda riga seconda colonna.](offset-grid-finished.png)

> [!NOTE]
> Come esercizio extra, può implementare una classe `offset-by-two`.

### Limitazioni delle griglie flottanti

Quando si utilizza un sistema come questo, è necessario fare attenzione affinché le larghezze totali si sommino correttamente e che non includa elementi in una riga che si estendono oltre le colonne che la riga può contenere. A causa del modo in cui funzionano i flottanti, se il numero di colonne della griglia diventa troppo ampio per la griglia, gli elementi alla fine scenderanno alla riga successiva, rompendo la griglia.

Inoltre, tenga presente che se il contenuto degli elementi diventa più largo delle righe che occupano, traboccherà e apparirà un disastro.

La limitazione più grande di questo sistema è che è essenzialmente unidimensionale. Stiamo trattando colonne e estendendo elementi attraverso colonne, ma non righe. È molto difficile con questi metodi di layout più vecchi controllare l'altezza degli elementi senza impostare esplicitamente un'altezza, e questo è un approccio molto inflessibile anche — funziona solo se può garantire che il suo contenuto avrà una certa altezza.

## Griglie Flexbox?

Se ha letto il nostro articolo precedente su [flexbox](/it/docs/Learn_web_development/Core/CSS_layout/Flexbox), potrebbe pensare che flexbox sia la soluzione ideale per creare un sistema a griglia. Ci sono molti sistemi a griglia basati su flexbox disponibili e flexbox può risolvere molti dei problemi che abbiamo già scoperto creando la nostra griglia sopra.

Tuttavia, flexbox non è mai stato progettato come un sistema a griglia e pone un nuovo set di sfide se usato come tale. Come esempio semplice di questo, possiamo prendere lo stesso markup di esempio che abbiamo usato sopra e usare il seguente CSS per stilizzare le classi `wrapper`, `row`, e `col`:

```css
body {
  width: 90%;
  max-width: 980px;
  margin: 0 auto;
}

.wrapper {
  padding-right: 2.08333333%;
}

.row {
  display: flex;
}

.col {
  margin-left: 2.08333333%;
  margin-bottom: 1em;
  width: 6.25%;
  flex: 1 1 auto;
  background: rgb(255 150 150);
}
```

Può provare a fare queste sostituzioni nel suo esempio, o guardare il nostro codice di esempio [flexbox-grid.html](https://github.com/mdn/learning-area/blob/main/css/css-layout/grids/flexbox-grid.html) (vedi anche [eseguito live](https://mdn.github.io/learning-area/css/css-layout/grids/flexbox-grid.html)).

Qui stiamo trasformando ciascuna riga in un contenitore flex. Con una griglia basata su flexbox abbiamo ancora bisogno di righe per consentirci di avere elementi che sommano meno del 100%. Impostiamo quel contenitore su `display: flex`.

Su `.col` impostiamo il primo valore della proprietà {{cssxref("flex")}} ({{cssxref("flex-grow")}}) su 1 in modo che i nostri elementi possano crescere, il secondo valore ({{cssxref("flex-shrink")}}) su 1 in modo che gli elementi possano restringersi, e il terzo valore ({{cssxref("flex-basis")}}) su `auto`. Poiché il nostro elemento ha una {{cssxref("width")}} impostata, `auto` userà quella larghezza come valore di `flex-basis`.

Sulla riga superiore otteniamo dodici caselle ordinatamente disposte nella griglia e crescono e si restringono equamente man mano che modifichiamo la larghezza del viewport. Nella riga successiva, però, abbiamo solo quattro elementi e anche questi crescono e si restringono da quel punto di partenza di 60px. Con solo quattro di loro possono crescere molto più degli elementi nella riga sopra, con il risultato che tutti occupano la stessa larghezza nella seconda riga.

![La griglia ha due righe. Ogni riga è un contenitore flex. La prima riga ha dodici elementi flex di uguale larghezza. La seconda riga ha quattro elementi flex di uguale larghezza.](flexbox-grid-incomplete.png)

Per risolvere questo problema dobbiamo ancora includere le nostre classi `span` per fornire una larghezza che sostituirà il valore usato da `flex-basis` per quell'elemento.

Non rispettano nemmeno la griglia degli elementi superiori perché non ne sanno nulla.

Flexbox è **unidimensionale** per design. Gestisce una singola dimensione, quella di una riga o di una colonna. Non possiamo creare una griglia rigorosa per colonne e righe, il che significa che se dobbiamo usare flexbox per la nostra griglia, dobbiamo ancora calcolare le percentuali come per il layout flottante.

Nel suo progetto potrebbe ancora scegliere di utilizzare una "griglia" flexbox per le capacità aggiuntive di allineamento e distribuzione dello spazio che flexbox fornisce rispetto ai float. Tuttavia, dovrebbe essere consapevole che sta ancora usando uno strumento per qualcosa di diverso da ciò per cui è stato progettato. Potrebbe quindi sentirsi che la faccia passare attraverso ulteriori passaggi per ottenere il risultato finale desiderato.

## Sistemi a griglia di terze parti

Ora che comprendiamo la matematica dietro i nostri calcoli di griglia, siamo in una buona posizione per esaminare alcuni dei sistemi a griglia di terze parti di uso comune. Se cerca "framework a griglia CSS" sul web, troverà una lista enorme di opzioni tra cui scegliere. Framework popolari come [Bootstrap](https://getbootstrap.com/) e [Foundation](https://get.foundation/) includono un sistema a griglia. Ci sono anche sistemi a griglia standalone, sviluppati utilizzando CSS o preprocessori.

Diamo un'occhiata a uno di questi sistemi standalone poiché dimostra tecniche comuni per lavorare con un framework a griglia. La griglia che useremo è parte di Skeleton, un semplice framework CSS.

Per iniziare, visiti il sito web di [Skeleton](http://getskeleton.com/), e scelga "Download" per scaricare il file ZIP. Scomprima questo e copi i file skeleton.css e normalize.css in una nuova directory.

Faccia una copia del nostro file [html-skeleton.html](https://github.com/mdn/learning-area/blob/main/css/css-layout/grids/html-skeleton.html) e lo salvi nella stessa directory dei CSS di skeleton e normalize.

Includa i CSS di skeleton e normalize nella pagina HTML, aggiungendo quanto segue nella head:

```html
<link href="normalize.css" rel="stylesheet" />
<link href="skeleton.css" rel="stylesheet" />
```

Skeleton include più di un sistema a griglia — contiene anche CSS per la tipografia e altri elementi di pagina che può usare come punto di partenza. Per ora, li lasceremo ai valori predefiniti — è la griglia che ci interessa qui.

> **Nota:** [Normalize](https://necolas.github.io/normalize.css/) è una piccola libreria CSS molto utile scritta da Nicolas Gallagher, che effettua automaticamente alcune correzioni di layout di base utili e rende più coerente lo stile predefinito degli elementi tra i browser.

Useremo un HTML simile al nostro esempio precedente. Aggiunga quanto segue nel corpo del suo HTML:

```html
<div class="container">
  <div class="row">
    <div class="col">1</div>
    <div class="col">2</div>
    <div class="col">3</div>
    <div class="col">4</div>
    <div class="col">5</div>
    <div class="col">6</div>
    <div class="col">7</div>
    <div class="col">8</div>
    <div class="col">9</div>
    <div class="col">10</div>
    <div class="col">11</div>
    <div class="col">12</div>
  </div>
  <div class="row">
    <div class="col">13</div>
    <div class="col">14</div>
    <div class="col">15</div>
    <div class="col">16</div>
  </div>
</div>
```

Per iniziare a utilizzare Skeleton dobbiamo assegnare al wrapper {{htmlelement("div")}} una classe di `container` — questa è già inclusa nel nostro HTML. Questo centra il contenuto con una larghezza massima di 960 pixel. Può vedere come le caselle ora non diventano mai più larghe di 960 pixel.

Può dare un'occhiata nel file skeleton.css per vedere il CSS che viene usato quando applichiamo questa classe. Il `<div>` è centrato usando margini sinistro e destro `auto`, e un padding di 20 pixel è applicato a sinistra e a destra. Skeleton imposta anche la proprietà {{cssxref("box-sizing")}} su `border-box` come abbiamo fatto in precedenza, quindi il padding e i bordi di questo elemento saranno inclusi nella larghezza totale.

```css
.container {
  position: relative;
  width: 100%;
  max-width: 960px;
  margin: 0 auto;
  padding: 0 20px;
  box-sizing: border-box;
}
```

Gli elementi possono essere parte della griglia solo se sono all'interno di una riga, quindi come nel nostro esempio precedente abbiamo bisogno di un `<div>` aggiuntivo o altro elemento con una classe di `row` annidato tra gli elementi `<div>` di contenuto e il `<div>` del contenitore. Questo l'abbiamo già fatto.

Ora disponiamo le caselle del contenitore. Skeleton è basato su una griglia a 12 colonne. Le caselle della riga superiore hanno tutte bisogno di classi di `one column` per farle estendere su una colonna.

Aggiunga questi ora, come mostrato nel seguente snippet:

```html
<div class="container">
  <div class="row">
    <div class="one column">1</div>
    <div class="one column">2</div>
    <div class="one column">3</div>
    /* and so on */
  </div>
</div>
```

Successivamente, dia ai contenitori sulla seconda riga classi che spiegano il numero di colonne che dovrebbero occupare, come così:

```html
<div class="row">
  <div class="one column">13</div>
  <div class="six columns">14</div>
  <div class="three columns">15</div>
  <div class="two columns">16</div>
</div>
```

Provi a salvare il suo file HTML e caricarlo nel suo browser per vedere l'effetto.

> [!NOTE]
> Se sta avendo difficoltà a far funzionare questo esempio, provi ad allargare la finestra che sta usando per visualizzarlo (la griglia non sarà visualizzata come descritto qui se la finestra è troppo stretta). Se non funziona, provi a confrontarlo con il nostro file [html-skeleton-finished.html](https://github.com/mdn/learning-area/blob/main/css/css-layout/grids/html-skeleton-finished.html) (vedi anche [in esecuzione live](https://mdn.github.io/learning-area/css/css-layout/grids/html-skeleton-finished.html)).

Se guarda nel file skeleton.css può vedere come funziona. Ad esempio, Skeleton ha definito quanto segue per stilizzare elementi con classi "three columns" aggiunte a essi.

```css
.three.columns {
  width: 22%;
}
```

Tutto ciò che Skeleton (o qualsiasi altro framework a griglia) sta facendo è impostare classi predefinite che può usare aggiungendole al suo markup. È esattamente come se avesse fatto lei stesso il lavoro di calcolare queste percentuali.

Come può vedere, dobbiamo scrivere pochissimo CSS quando usiamo Skeleton. Gestisce tutti i flottamenti per noi quando aggiunge classi al nostro markup. È questa capacità di trasferire la responsabilità del layout a qualcos'altro che ha reso allettante la scelta di utilizzare un framework per un sistema a griglia! Tuttavia, al giorno d'oggi, con il layout a griglia CSS, molti sviluppatori stanno passando da questi framework per utilizzare la griglia nativa integrata che CSS fornisce.

## Sommario

Ora comprende come vengono creati vari sistemi a griglia, il che sarà utile nel lavorare con siti più vecchi e nel comprendere la differenza tra la griglia nativa del layout a griglia CSS e questi sistemi più vecchi.
