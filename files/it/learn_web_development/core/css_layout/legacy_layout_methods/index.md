---
title: Metodi di layout legacy
slug: Learn_web_development/Core/CSS_layout/Legacy_Layout_Methods
l10n:
  sourceCommit: f4c14731a1a157fc8d8f7357ac4d74d14a7d7fb5
---

I sistemi a griglia sono una funzionalità molto comune nei layout CSS e, prima del layout a griglia CSS, tendevano a essere implementati usando float o altre funzionalità di layout. Si immagina il layout come un determinato numero di colonne (ad esempio 4, 6 o 12), quindi si inseriscono le colonne di contenuto all'interno di queste colonne immaginarie. In questo articolo verrà illustrato il funzionamento di questi metodi meno recenti, per comprenderne l'uso quando si lavora su un progetto più datato.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Fondamenti di HTML (studiare
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Introduzione a HTML</a
        >) e un'idea di come funziona CSS (studiare
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Fondamenti dello stile CSS</a>.)
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere i concetti fondamentali alla base dei sistemi di layout a griglia
        usati prima che il layout a griglia CSS fosse disponibile nei browser.
      </td>
    </tr>
  </tbody>
</table>

## Layout e sistemi a griglia prima del layout a griglia CSS

Per chi proviene da un background di design, potrebbe sembrare sorprendente che CSS non abbia avuto un sistema a griglia integrato fino a tempi molto recenti e che, invece, sembrasse utilizzare una varietà di metodi non ottimali per creare design simili a griglie. Oggi ci riferiamo a questi come metodi "legacy".

Per i nuovi progetti, nella maggior parte dei casi il layout a griglia CSS verrà usato in combinazione con uno o più altri metodi di layout moderni per formare la base di qualsiasi layout. Tuttavia, occasionalmente si incontreranno "sistemi a griglia" che usano questi metodi legacy. Vale la pena comprendere come funzionano e perché sono diversi dal layout a griglia CSS.

Questa lezione spiegherà come funzionano i sistemi e i framework a griglia basati su float e flexbox. Dopo aver studiato il layout a griglia, probabilmente risulterà sorprendente quanto tutto questo sembri complicato. Questa conoscenza sarà utile quando occorre creare codice di fallback per browser che non supportano metodi più recenti, oltre a consentire di lavorare su progetti esistenti che usano questi tipi di sistemi.

È bene ricordare, mentre vengono esplorati questi sistemi, che nessuno di essi crea effettivamente una griglia nel modo in cui il layout a griglia CSS crea una griglia. Funzionano assegnando una dimensione agli elementi e spostandoli per allinearli in un modo che _sembra_ una griglia.

## Un layout a due colonne

Iniziamo con l'esempio più semplice possibile: un layout a due colonne. È possibile seguire creando un nuovo file `index.html` sul computer, riempiendolo con un [semplice modello HTML](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/getting-started/index.html) e inserendovi il codice seguente nei punti appropriati. Alla fine della sezione è disponibile un esempio live di come dovrebbe apparire il codice finale.

Prima di tutto, occorre del contenuto da inserire nelle colonne. Sostituire tutto ciò che si trova attualmente nel body con quanto segue:

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

Ciascuna colonna necessita di un elemento esterno che contenga il proprio contenuto e consenta di manipolarlo tutto insieme. In questo esempio sono stati scelti dei {{htmlelement("div")}}, ma si potrebbe scegliere qualcosa di semanticamente più appropriato, come {{htmlelement("article")}}, {{htmlelement("section")}}, {{htmlelement("aside")}} o altro.

Ora passiamo al CSS. Prima di tutto, applicare quanto segue all'HTML per fornire una configurazione di base:

```css
body {
  width: 90%;
  max-width: 900px;
  margin: 0 auto;
}
```

Il body avrà una larghezza pari al 90% della viewport fino a raggiungere i 900px; a quel punto rimarrà fisso a questa larghezza e verrà centrato nella viewport. Per impostazione predefinita, i suoi elementi figli ({{htmlelement("Heading_Elements", "h1")}} e i due {{htmlelement("div")}}) occuperanno il 100% della larghezza del body. Per far sì che i due {{htmlelement("div")}} siano affiancati tramite float, occorre impostare le loro larghezze affinché il totale sia pari o inferiore al 100% della larghezza dell'elemento padre, così da poter essere affiancati. Aggiungere quanto segue alla fine del CSS:

```css
div:nth-of-type(1) {
  width: 48%;
}

div:nth-of-type(2) {
  width: 48%;
}
```

Qui sono state impostate entrambe al 48% della larghezza del loro elemento padre: il totale è 96%, lasciando libero il 4% da usare come gutter tra le due colonne, per dare al contenuto un po' di spazio. Ora basta applicare float alle colonne, come segue:

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

Mettendo tutto insieme, si dovrebbe ottenere un risultato simile al seguente:

{{ EmbedLiveSample('A_two_column_layout', '100%', 520) }}

Si noti che per tutte le larghezze vengono usate percentuali: è una strategia piuttosto valida, perché crea un **layout fluido**, che si adatta a diverse dimensioni dello schermo e mantiene le stesse proporzioni per le larghezze delle colonne su schermi più piccoli. Provare a modificare la larghezza della finestra del browser per verificarlo. È uno strumento prezioso per il [responsive web design](/it/docs/Learn_web_development/Core/CSS_layout/Responsive_Design).

## Creazione di semplici framework a griglia legacy

La maggior parte dei framework legacy utilizza il comportamento della proprietà {{cssxref("float")}} per affiancare una colonna a un'altra e creare qualcosa che assomiglia a una griglia. Analizzare il processo di creazione di una griglia con i float mostra come funziona e introduce anche alcuni concetti più avanzati che si basano sugli argomenti appresi nella lezione su [float e clearing](/it/docs/Learn_web_development/Core/CSS_layout/Floats).

Il tipo di framework a griglia più semplice da creare è uno a larghezza fissa: occorre solo stabilire quale deve essere la larghezza totale del design, quante colonne sono necessarie e quanto devono essere larghi i gutter e le colonne. Se invece si decidesse di disporre il design su una griglia con colonne che crescono e si restringono in base alla larghezza del browser, sarebbe necessario calcolare larghezze percentuali per le colonne e per i gutter tra di esse.

Nelle sezioni successive verrà illustrata la creazione di entrambi. Verrà creata una griglia di 12 colonne, una scelta molto comune e considerata molto adattabile a diverse situazioni, poiché 12 è comodamente divisibile per 6, 4, 3 e 2.

### Una semplice griglia a larghezza fissa

Creiamo innanzitutto un sistema a griglia che usa colonne a larghezza fissa.

Iniziare creando un nuovo file HTML sul sistema locale e aggiungere il seguente markup nel suo `<body>`:

```html live-sample___basic-grid
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

L'obiettivo è trasformarlo in una griglia dimostrativa di due righe su una griglia a dodici colonne: la riga superiore dimostra la dimensione delle singole colonne, mentre la seconda riga mostra alcune aree di dimensioni diverse nella griglia.

![Griglia CSS con 16 elementi della griglia distribuiti su dodici colonne e due righe. La riga superiore ha 12 elementi della griglia di uguale larghezza in 12 colonne. La seconda riga presenta elementi della griglia di dimensioni diverse. L'elemento 13 occupa 1 colonna, l'elemento 14 ne occupa sei, il 15 ne occupa tre e il 16 ne occupa due.](simple-grid-finished.png)

Successivamente, applicare un foglio di stile all'HTML, usando un elemento {{htmlelement("style")}} oppure un file CSS esterno referenziato in un elemento {{htmlelement("link")}}.

Aggiungere il codice seguente al foglio di stile, che assegna al contenitore wrapper una larghezza di 980 pixel, con un padding di 20 pixel sul lato destro. Questo lascia 960 pixel per le larghezze complessive di colonne/gutter; in questo caso, il padding viene sottratto dalla larghezza totale del contenuto perché {{cssxref("box-sizing")}} è stato impostato su `border-box` per tutti gli elementi del sito (consultare [Il modello box CSS alternativo](/it/docs/Learn_web_development/Core/Styling_basics/Box_model#the_alternative_css_box_model) per ulteriori spiegazioni).

```css live-sample___basic-grid
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

Ora usare il contenitore di riga che avvolge ogni riga della griglia per separare una riga dall'altra tramite clearing. Aggiungere la regola seguente sotto la precedente:

```css live-sample___basic-grid
.row {
  clear: both;
}
```

Applicando questo clearing, non è necessario riempire completamente ogni riga con elementi che occupano tutte e dodici le colonne. Le righe rimarranno separate e non interferiranno tra loro.

I gutter tra le colonne sono larghi 20 pixel. Questi gutter vengono creati come margine sul lato sinistro di ogni colonna, inclusa la prima colonna, per compensare i 20 pixel di padding sul lato destro del contenitore. Ci sono quindi 12 gutter in totale: 12 x 20 = 240.

Occorre sottrarre questo valore dalla larghezza totale di 960 pixel, ottenendo 720 pixel per le colonne. Dividendo ora per 12, si ottiene che ogni colonna deve essere larga 60 pixel.

Il passo successivo consiste nel creare una regola per la classe `.col`, applicando float a sinistra, assegnandole un {{cssxref("margin-left")}} di 20 pixel per formare il gutter e una {{cssxref("width")}} di 60 pixel. Aggiungere la regola seguente alla fine del CSS:

```css live-sample___basic-grid
.col {
  float: left;
  margin-left: 20px;
  width: 60px;
  background: rgb(255 150 150);
}
```

La riga superiore di colonne singole verrà ora disposta ordinatamente come una griglia.

> [!NOTE]
> A ogni colonna è stato assegnato anche un colore rosso chiaro, in modo da mostrare esattamente quanto spazio occupa ciascuna.

Ai contenitori di layout che devono occupare più di una colonna devono essere assegnate classi speciali per adeguare i loro valori di {{cssxref("width")}} al numero richiesto di colonne, più i gutter intermedi. Occorre creare una classe aggiuntiva che permetta ai contenitori di occupare da 2 a 12 colonne. Ogni larghezza è il risultato della somma della larghezza delle colonne per quel numero di colonne più le larghezze dei gutter, che saranno sempre uno in meno del numero di colonne.

Aggiungere quanto segue alla fine del CSS:

```css live-sample___basic-grid
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

Dopo aver creato queste classi, è possibile disporre sulla griglia colonne di larghezze diverse. Provare a salvare e caricare la pagina nel browser per vedere gli effetti. Dovrebbe avere un aspetto simile al seguente esempio live:

{{embedlivesample("basic-grid", "100%", 100)}}

Provare a modificare le classi sugli elementi o anche ad aggiungere e rimuovere alcuni contenitori, per vedere come variare il layout. Ad esempio, si potrebbe fare in modo che la seconda riga appaia così:

```html
<div class="row">
  <div class="col span8">13</div>
  <div class="col span4">14</div>
</div>
```

Ora è disponibile un sistema a griglia funzionante: è possibile definire le righe e il numero di colonne in ogni riga, quindi riempire ciascun contenitore con il contenuto necessario. Ottimo!

### Creazione di una griglia fluida

La griglia funziona bene, ma ha una larghezza fissa; sarà stato notato che la griglia trabocca dalla pagina incorporata nell'esempio live precedente. Serve invece una griglia flessibile, o fluida, che cresca e si restringa insieme allo spazio disponibile nella {{Glossary("viewport", "viewport")}} del browser. Per ottenere questo risultato, è possibile trasformare le larghezze di riferimento in pixel in percentuali.

L'equazione che trasforma una larghezza fissa in una flessibile basata su percentuali è la seguente.

```plain
target / context = result
```

Per la larghezza della colonna, la **larghezza di destinazione** è 60 pixel e il **contesto** è il wrapper di 960 pixel. Per calcolare una percentuale si può usare quanto segue:

```plain
60 / 960 = 0.0625
```

Spostando quindi la virgola decimale di 2 posizioni, si ottiene una percentuale del 6,25%. Pertanto, nel CSS è possibile sostituire la larghezza della colonna di 60 pixel con 6,25%.

Occorre fare lo stesso con la larghezza del gutter:

```plain
20 / 960 = 0.02083333333
```

Quindi è necessario sostituire il {{cssxref("margin-left")}} di 20 pixel nella regola `.col` e il {{cssxref("padding-right")}} di 20 pixel in `.wrapper` con 2,08333333%.

#### Aggiornamento della griglia

Per iniziare questa sezione, creare una nuova copia della pagina di esempio precedente oppure ottenere una copia del codice dell'esempio live precedente da usare come punto di partenza (fare clic sul pulsante "Play" per visualizzare il codice completo nel playground MDN).

Aggiornare la seconda regola CSS, con il selettore `.wrapper`, come segue:

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

Oltre ad assegnare una {{cssxref("width")}} percentuale, è stata aggiunta anche una proprietà {{cssxref("max-width")}} per evitare che il layout diventi troppo largo.

Successivamente, aggiornare la quarta regola CSS, con il selettore `.col`, come segue:

```css
.col {
  float: left;
  margin-left: 2.08333333%;
  width: 6.25%;
  background: rgb(255 150 150);
}
```

Ora arriva la parte un po' più laboriosa: occorre aggiornare tutte le regole `.col.span` affinché usino percentuali anziché larghezze in pixel. Con una calcolatrice richiede un po' di tempo; per risparmiare lo sforzo, il lavoro è stato svolto di seguito.

Aggiornare il blocco finale delle regole CSS con quanto segue:

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

Ora salvare il codice e caricarlo in un browser, oppure consultare il seguente esempio live:

```css hidden live-sample___fluid-grid
* {
  box-sizing: border-box;
}

body {
  width: 90%;
  max-width: 980px;
  margin: 0 auto;
}

.wrapper {
  padding-right: 2.08333333%;
}

.row {
  clear: both;
}

.col {
  float: left;
  margin-left: 2.08333333%;
  width: 6.25%;
  background: rgb(255, 150, 150);
}

/* Two column widths (12.5%) plus one gutter width (2.08333333%) */
.col.span2 {
  width: 14.58333333%;
}
/* Three column widths (18.75%) plus two gutter widths (4.1666666) */
.col.span3 {
  width: 22.91666666%;
}
/* And so on... */
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

{{embedlivesample("fluid-grid", "100%", 100)}}

Provare a modificare la larghezza della viewport: le larghezze delle colonne dovrebbero adattarsi in modo appropriato.

### Calcoli più semplici con la funzione calc()

È possibile usare la funzione {{cssxref("calc", "calc()")}} per eseguire i calcoli direttamente nel CSS: consente di inserire semplici equazioni matematiche nei valori CSS, per calcolare quale dovrebbe essere un valore. È particolarmente utile quando occorre eseguire calcoli complessi e può persino calcolare un'espressione che usa unità diverse, ad esempio: "si vuole che l'altezza di questo elemento sia sempre pari al 100% dell'altezza del suo elemento padre, meno 50px". Consultare [questo esempio tratto da un tutorial sulla MediaStream Recording API](/it/docs/Web/API/MediaStream_Recording_API/Using_the_MediaStream_Recording_API#keeping_the_interface_constrained_to_the_viewport_regardless_of_device_height_with_calc).

Comunque, torniamo alle griglie. Qualsiasi colonna che occupa più di una colonna della griglia ha una larghezza totale pari a 6,25% moltiplicato per il numero di colonne occupate, più 2,08333333% moltiplicato per il numero di gutter, che sarà sempre il numero di colonne meno 1. La funzione `calc()` consente di eseguire questo calcolo direttamente all'interno del valore della larghezza; per esempio, per qualsiasi elemento che occupi 4 colonne si può fare quanto segue:

```css
.col.span4 {
  width: calc((6.25% * 4) + (2.08333333% * 3));
}
```

Provare a sostituire il blocco finale di regole con il seguente, quindi ricaricarlo nel browser per verificare di ottenere lo stesso risultato:

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

```css hidden live-sample___fluid-grid-calc
* {
  box-sizing: border-box;
}

body {
  width: 90%;
  max-width: 980px;
  margin: 0 auto;
}

.wrapper {
  padding-right: 2.08333333%;
}

.row {
  clear: both;
}

.col {
  float: left;
  margin-left: 2.08333333%;
  width: 6.25%;
  background: rgb(255, 150, 150);
}

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

Questo produce il seguente risultato finale:

{{embedlivesample("fluid-grid-calc", "100%", "100")}}

### Sistemi a griglia semantici e "non semantici"

L'aggiunta di classi al markup per definire il layout significa che contenuto e markup diventano legati alla presentazione visiva. Talvolta si sente descrivere questo uso delle classi CSS come "non semantico", poiché descrive l'aspetto del contenuto, anziché un uso semantico delle classi che descrive il contenuto. Questo è il caso delle classi `span2`, `span3` e così via.

Non è l'unico approccio possibile. Si potrebbe invece decidere la griglia e poi aggiungere le informazioni sulle dimensioni alle regole per le classi semantiche esistenti. Ad esempio, se fosse presente un {{htmlelement("div")}} con una classe `content` che deve occupare 8 colonne, si potrebbe copiare la larghezza dalla classe `span8`, ottenendo una regola come questa:

```css
.content {
  width: calc((6.25% * 8) + (2.08333333% * 7));
}
```

> [!NOTE]
> Usando un preprocessore come [Sass](https://sass-lang.com/), si potrebbe creare un semplice mixin per inserire quel valore automaticamente.

### Abilitazione dei contenitori con offset nella griglia

La griglia creata funziona bene finché tutti i contenitori devono iniziare a filo con il lato sinistro della griglia. Se si volesse lasciare uno spazio vuoto di una colonna prima del primo contenitore, oppure tra contenitori, sarebbe necessario creare una classe di offset per aggiungere un margine sinistro al sito e spostarlo visivamente lungo la griglia. Altri calcoli!

Proviamo.

Partire dal codice precedente esistente oppure usare il codice dell'esempio live precedente (premere il pulsante "Play" per visualizzare il codice completo nel playground MDN).

Creare una classe nel CSS che sposti un elemento contenitore della larghezza di una colonna. Aggiungere quanto segue alla fine del CSS:

```css
.offset-by-one {
  margin-left: calc(6.25% + (2.08333333% * 2));
}
```

Oppure, se si preferisce calcolare le percentuali manualmente, usare questo:

```css
.offset-by-one {
  margin-left: 10.41666666%;
}
```

Ora è possibile aggiungere questa classe a qualsiasi contenitore davanti al quale si voglia lasciare uno spazio vuoto largo una colonna sul lato sinistro. Ad esempio, se nell'HTML è presente quanto segue:

```html
<div class="col span6">14</div>
```

Provare a sostituirlo con:

```html
<div class="col span5 offset-by-one">14</div>
```

> [!NOTE]
> Si noti che è necessario ridurre il numero di colonne occupate, per fare spazio all'offset.

```html hidden live-sample___fluid-grid-offset
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
    <div class="col span5 offset-by-one">14</div>
    <div class="col span3">15</div>
    <div class="col span2">16</div>
  </div>
</div>
```

```css hidden live-sample___fluid-grid-offset
* {
  box-sizing: border-box;
}

body {
  width: 90%;
  max-width: 980px;
  margin: 0 auto;
}

.wrapper {
  padding-right: 2.08333333%;
}

.row {
  clear: both;
}

.col {
  float: left;
  margin-left: 2.08333333%;
  width: 6.25%;
  background: rgb(255, 150, 150);
}

/* Two column widths (12.5%) plus one gutter width (2.08333333%) */
.col.span2 {
  width: 14.58333333%;
}
/* Three column widths (18.75%) plus two gutter widths (4.1666666) */
.col.span3 {
  width: 22.91666666%;
}
/* And so on... */
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

.offset-by-one {
  margin-left: 10.41666666%;
}
```

Provare a caricare e aggiornare la pagina per vedere la differenza, oppure consultare l'esempio live completato:

{{embedlivesample("fluid-grid-offset", "100%","100")}}

> [!NOTE]
> Come esercizio aggiuntivo, è possibile implementare una classe `offset-by-two`?

### Limiti delle griglie con float

Quando si usa un sistema come questo, è necessario fare attenzione che le larghezze totali siano sommate correttamente e non includere in una riga elementi che occupano più colonne di quante la riga possa contenere. A causa del funzionamento dei float, se il numero di colonne della griglia diventa troppo ampio per la griglia, gli elementi finali scenderanno alla riga successiva, interrompendo la griglia.

Inoltre, se il contenuto degli elementi diventa più largo delle righe che occupano, traboccherà e avrà un aspetto disordinato.

Il limite maggiore di questo sistema è che è essenzialmente unidimensionale. Si lavora con colonne e con elementi che occupano più colonne, ma non con le righe. Con questi metodi di layout più vecchi, è molto difficile controllare l'altezza degli elementi senza impostare esplicitamente un'altezza; anche questo è un approccio molto poco flessibile, perché funziona solo se è possibile garantire che il contenuto avrà una certa altezza.

## Griglie flexbox?

Dopo aver letto l'articolo precedente su [flexbox](/it/docs/Learn_web_development/Core/CSS_layout/Flexbox), potrebbe sembrare che flexbox sia la soluzione ideale per creare un sistema a griglia. Sono disponibili molti sistemi a griglia basati su flexbox e flexbox può risolvere molti dei problemi già emersi nella creazione della griglia precedente.

Tuttavia, flexbox non è mai stato progettato come sistema a griglia e pone una nuova serie di sfide quando viene usato come tale. Come semplice esempio, è possibile prendere lo stesso markup di esempio usato sopra e utilizzare il CSS seguente per applicare lo stile alle classi `wrapper`, `row` e `col`:

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

```html hidden live-sample___fluid-grid live-sample___fluid-grid-calc live-sample___flexbox-grid
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

```css hidden live-sample___flexbox-grid
* {
  box-sizing: border-box;
}

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
  background: rgb(255, 150, 150);
}

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

Questo produce sostanzialmente lo stesso risultato di prima:

{{embedlivesample("flexbox-grid", "100%","100")}}

Qui ogni riga viene trasformata in un contenitore flex. Con una griglia basata su flexbox servono comunque le righe, per consentire elementi la cui somma sia inferiore a `100%`. Il contenitore viene impostato su `display: flex`.

Su `.col`, il primo valore della proprietà {{cssxref("flex")}} ({{cssxref("flex-grow")}}) viene impostato a 1 affinché gli elementi possano crescere, il secondo valore ({{cssxref("flex-shrink")}}) a 1 affinché gli elementi possano restringersi e il terzo valore ({{cssxref("flex-basis")}}) a `auto`. Poiché l'elemento ha una {{cssxref("width")}} impostata, `auto` userà tale larghezza come valore di `flex-basis`.

Occorre ancora includere le classi `span` sulle colonne che devono occupare un numero specifico di righe, fornendo una larghezza che sostituirà il valore usato da `flex-basis` per tali elementi.

Questo sistema non rispetta la griglia usata per contenere gli elementi, perché non ne sa nulla. Flexbox è **unidimensionale** per progettazione. Gestisce una singola dimensione, quella di una riga o di una colonna. Non è possibile creare una griglia rigorosa per colonne e righe; ciò significa che, per usare flexbox per una griglia, occorre comunque calcolare le percentuali come nel layout con float.

In un progetto si potrebbe comunque scegliere di usare una "griglia" flexbox grazie alle ulteriori capacità di allineamento e distribuzione dello spazio che flexbox offre rispetto ai float. Bisogna però essere consapevoli che si sta ancora usando uno strumento per uno scopo diverso da quello per cui è stato progettato. Potrebbe quindi sembrare necessario superare ulteriori ostacoli per ottenere il risultato finale desiderato.

## Sistemi a griglia di terze parti

Ora che sono compresi i calcoli alla base delle griglie, è possibile esaminare alcuni sistemi a griglia di terze parti di uso comune. Cercando "CSS grid framework" sul Web si troverà un enorme elenco di opzioni tra cui scegliere. Framework popolari come [Bootstrap](https://getbootstrap.com/) e [Foundation](https://get.foundation/) includono un sistema a griglia. Esistono anche sistemi a griglia indipendenti, sviluppati usando CSS o preprocessori.

Esaminiamo uno di questi sistemi indipendenti, poiché dimostra tecniche comuni per lavorare con un framework a griglia. La griglia che verrà usata fa parte di Skeleton, un semplice framework CSS.

Per iniziare, visitare il [sito web di Skeleton](http://getskeleton.com/) e scegliere "Download" per scaricare il file ZIP. Decomprimere il file e copiare i file skeleton.css e normalize.css contenuti al suo interno in una nuova directory.

Creare un nuovo file HTML con un `<body>` vuoto nella stessa directory dei file CSS skeleton e normalize.

Includere Skeleton e normalize CSS nella pagina HTML aggiungendo quanto segue al relativo head:

```html
<link href="normalize.css" rel="stylesheet" />
<link href="skeleton.css" rel="stylesheet" />
```

Skeleton include più di un sistema a griglia: contiene anche CSS per la tipografia e altri elementi della pagina, utilizzabili come punto di partenza. Per ora verranno lasciati ai valori predefiniti; qui interessa soprattutto la griglia.

> [!NOTE]
> [Normalize](https://necolas.github.io/normalize.css/) è una piccola e utilissima libreria CSS scritta da Nicolas Gallagher, che applica automaticamente alcune utili correzioni di layout di base e rende lo stile predefinito degli elementi più coerente tra i browser.

Verrà usato HTML simile a quello dell'esempio precedente. Aggiungere quanto segue nel body HTML:

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

Per iniziare a usare Skeleton, occorre assegnare al wrapper {{htmlelement("div")}} una classe `container`; questa è già inclusa nell'HTML. Ciò centra il contenuto con una larghezza massima di 960 pixel. Si può notare come i riquadri non diventino mai più larghi di 960 pixel.

È possibile esaminare il file skeleton.css per vedere il CSS usato quando viene applicata questa classe. Il `<div>` viene centrato usando margini sinistro e destro `auto` e viene applicato un padding di 20 pixel a sinistra e a destra. Skeleton imposta inoltre la proprietà {{cssxref("box-sizing")}} su `border-box`, come fatto in precedenza, quindi il padding e i bordi di questo elemento saranno inclusi nella larghezza totale.

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

Gli elementi possono fare parte della griglia soltanto se si trovano all'interno di una riga, quindi, come nell'esempio precedente, è necessario un ulteriore `<div>` o altro elemento con una classe `row` annidato tra gli elementi `<div>` del contenuto e il `<div>` contenitore. Anche questo è già stato fatto.

Ora disponiamo i riquadri contenitore. Skeleton si basa su una griglia di 12 colonne. Tutti i riquadri della riga superiore necessitano delle classi `one column` per occupare una colonna.

Aggiungerle ora, come mostrato nel frammento seguente:

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

Successivamente, assegnare ai contenitori della seconda riga classi che indicano il numero di colonne che devono occupare, come segue:

```html
<div class="row">
  <div class="one column">13</div>
  <div class="six columns">14</div>
  <div class="three columns">15</div>
  <div class="two columns">16</div>
</div>
```

Provare a salvare il file HTML e caricarlo nel browser per vedere l'effetto.

> [!NOTE]
> Se si riscontrano difficoltà nel far funzionare questo esempio, provare ad allargare la finestra usata per visualizzarlo; la griglia non verrà mostrata come descritto qui se la finestra è troppo stretta. Se questo non funziona, provare a confrontarlo con il file [html-skeleton-finished.html](https://github.com/mdn/learning-area/blob/main/css/css-layout/legacy/html-skeleton-finished.html) (disponibile anche [in esecuzione live](https://mdn.github.io/learning-area/css/css-layout/legacy/html-skeleton-finished.html)).

Osservando il file skeleton.css, è possibile vedere come funziona. Ad esempio, Skeleton definisce quanto segue per applicare lo stile agli elementi a cui sono state aggiunte classi "three columns".

```css
.three.columns {
  width: 22%;
}
```

Tutto ciò che fa Skeleton, o qualsiasi altro framework a griglia, è configurare classi predefinite che possono essere usate aggiungendole al markup. È esattamente lo stesso che svolgere il lavoro di calcolare queste percentuali manualmente.

Come si può vedere, quando si usa Skeleton è necessario scrivere pochissimo CSS. Il framework gestisce tutti i float quando si aggiungono classi al markup. Questa possibilità di affidare a qualcos'altro la responsabilità del layout ha reso l'uso di un framework per un sistema a griglia una scelta convincente. Oggi, tuttavia, con il layout a griglia CSS, molti sviluppatori si stanno allontanando da questi framework per usare la griglia nativa integrata offerta da CSS.

## Riepilogo

Ora sono stati compresi i diversi modi in cui vengono creati i sistemi a griglia. Questo sarà utile per lavorare con siti più vecchi e per comprendere la differenza tra la griglia nativa del layout a griglia CSS e questi sistemi meno recenti.
