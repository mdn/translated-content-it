---
title: Metodi di layout legacy
slug: Learn_web_development/Core/CSS_layout/Legacy_Layout_Methods
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

I sistemi di griglia sono una funzionalità molto comune utilizzata nei layout CSS, e prima dell'introduzione del layout a griglia CSS, tendevano a essere implementati utilizzando i "float" o altre caratteristiche di layout. Si immagina il proprio layout come un numero fisso di colonne (ad esempio, 4, 6 o 12), e successivamente si posizionano le colonne di contenuto all'interno di queste colonne immaginarie. In questo articolo esploreremo come funzionano questi metodi più vecchi, in modo che tu possa capire come sono stati utilizzati se lavori su un progetto più vecchio.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Basi di HTML (studiare
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Introduzione a HTML</a
        >), e un'idea di come funziona il CSS (studiare
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Basi di styling CSS</a>.)
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere i concetti fondamentali dei sistemi di layout a griglia
        utilizzati prima che il layout a griglia CSS fosse disponibile nei browser.
      </td>
    </tr>
  </tbody>
</table>

## Sistemi di layout e griglie prima del layout a griglia CSS

Può sembrare sorprendente per chi proviene da un background di design che CSS non avesse un sistema di griglia integrato fino a tempi molto recenti, e invece sembravamo utilizzare una varietà di metodi subottimali per creare design a griglia. Ora ci riferiamo a questi come metodi "legacy".

Per nuovi progetti, nella maggior parte dei casi, il layout a griglia CSS sarà utilizzato in combinazione con uno o più altri metodi di layout moderni per formare la base di qualsiasi layout. Tuttavia, ti imbatterai occasionalmente in "sistemi di griglie" che utilizzano questi metodi legacy. Vale la pena capire come funzionano e perché sono diversi dal layout a griglia CSS.

Questa lezione spiegherà come funzionano i sistemi di griglie e i framework di griglie basati su float e flexbox. Dopo aver studiato il layout a griglia, probabilmente sarai sorpreso di quanto tutto ciò sembri complicato! Queste conoscenze ti saranno utili se devi creare codice di fallback per i browser che non supportano i metodi più recenti, oltre a permetterti di lavorare su progetti esistenti che utilizzano questo tipo di sistemi.

Vale la pena ricordare, mentre esploriamo questi sistemi, che nessuno di essi crea effettivamente una griglia nel modo in cui lo fa il layout a griglia CSS. Funzionano dando agli elementi una dimensione e spostandoli per allinearli in un modo che _sembra_ una griglia.

## Un layout a due colonne

Iniziamo con l'esempio più semplice possibile: un layout a due colonne. Puoi seguire creando un nuovo file `index.html` sul tuo computer, riempiendolo con un [template HTML semplice](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/getting-started/index.html) e inserendo il codice qui sotto nei punti appropriati. In fondo alla sezione puoi vedere un esempio dal vivo di come il codice finale dovrebbe apparire.

Prima di tutto, abbiamo bisogno di un po' di contenuto da mettere nelle nostre colonne. Sostituisci il contenuto attuale del corpo con il seguente:

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

Ciascuna delle colonne ha bisogno di un elemento esterno per contenere il proprio contenuto e permetterci di manipolarlo tutto in una volta. In questo caso d'esempio abbiamo scelto dei {{htmlelement("div")}}, ma potresti scegliere qualcosa di più semanticamente appropriato come {{htmlelement("article")}}, {{htmlelement("section")}}, e {{htmlelement("aside")}}, o qualsiasi altra cosa.

Ora per il CSS. Prima di tutto, applica il seguente stile al tuo HTML per fornire una configurazione di base:

```css
body {
  width: 90%;
  max-width: 900px;
  margin: 0 auto;
}
```

Il corpo sarà largo il 90% del viewport fino a raggiungere i 900px di larghezza, nel qual caso rimarrà fisso a questa larghezza e si centrerà nel viewport. Per impostazione predefinita, i suoi figli (l'elemento {{htmlelement("Heading_Elements", "h1")}} e i due {{htmlelement("div")}}) occuperanno il 100% della larghezza del corpo. Se vogliamo che i due {{htmlelement("div")}} siano affiancati, dobbiamo impostare le loro larghezze in modo che totaleggino il 100% della larghezza dell'elemento padre o meno, in modo che possano adattarsi uno accanto all'altro. Aggiungi il seguente codice nella parte inferiore del tuo CSS:

```css
div:nth-of-type(1) {
  width: 48%;
}

div:nth-of-type(2) {
  width: 48%;
}
```

Qui abbiamo impostato entrambi al 48% della larghezza del loro genitore, per un totale del 96%, lasciando un 4% come margine tra le due colonne, dando al contenuto un po' di spazio per respirare. Ora dobbiamo solo far galleggiare le colonne, come segue:

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

Mettere tutto questo insieme dovrebbe darci un risultato come questo:

{{ EmbedLiveSample('A_two_column_layout', '100%', 520) }}

Noterai qui che stiamo usando percentuali per tutte le larghezze: questa è una buona strategia, in quanto crea un **layout liquido**, uno che si adatta a diverse dimensioni di schermo e mantiene le stesse proporzioni per le larghezze delle colonne a dimensioni di schermo più piccole. Prova a regolare la larghezza della finestra del browser per vedere di persona. Questo è uno strumento prezioso per il design web responsive.

> [!NOTE]
> Puoi vedere questo esempio in esecuzione su [0_two-column-layout.html](https://mdn.github.io/learning-area/css/css-layout/floats/0_two-column-layout.html) (vedi anche [il codice sorgente](https://github.com/mdn/learning-area/blob/main/css/css-layout/floats/0_two-column-layout.html)).

## Creare framework a griglia legacy semplici

La maggior parte dei framework legacy utilizza il comportamento della proprietà {{cssxref("float")}} per far galleggiare una colonna accanto all'altra per creare qualcosa che sembri una griglia. Attraversare il processo di creazione di una griglia con float ti mostra come funziona questo processo e introduce anche concetti più avanzati per costruire sulle cose che hai imparato nella lezione su [float e clearing](/it/docs/Learn_web_development/Core/CSS_layout/Floats).

Il tipo più facile di framework a griglia da creare è uno a larghezza fissa: dobbiamo solo determinare di quanto vogliamo che sia la larghezza totale del nostro design, quante colonne vogliamo e quanto dovrebbero essere larghi i margini e le colonne. Se decidessimo invece di creare il nostro design su una griglia con colonne che si espandono e si comprimono in base alla larghezza del browser, dovremmo calcolare le larghezze in percentuale per le colonne e i margini tra di esse.

Nelle prossime sezioni vedremo come creare entrambi. Creeremo una griglia a 12 colonne, una scelta molto comune considerata molto adattabile a varie situazioni, dato che 12 è facilmente divisibile per 6, 4, 3, e 2.

### Una griglia a larghezza fissa semplice

Iniziamo creando un sistema di griglia che utilizza colonne a larghezza fissa.

Inizia facendo una copia locale del nostro file [simple-grid.html](https://github.com/mdn/learning-area/blob/main/css/css-layout/grids/simple-grid.html), che contiene il seguente markup nel suo corpo.

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

L'obiettivo è trasformarlo in una griglia dimostrativa di due righe su una griglia a dodici colonne: la riga superiore dimostra la dimensione delle singole colonne, la seconda riga alcune aree di diverse dimensioni sulla griglia.

![Griglia CSS con 16 elementi a griglia distribuiti su dodici colonne e due righe. La riga superiore ha 12 elementi a griglia di larghezza uguale in 12 colonne. La seconda riga ha elementi a griglia di dimensioni diverse. L'elemento 13 si estende su 1 colonna, l'elemento 14 su sei colonne, il 15 su tre e il 16 su due.](simple-grid-finished.png)

Nell'elemento {{htmlelement("style")}}, aggiungi il seguente codice, che fornisce al contenitore wrapper una larghezza di 980 pixel, con un padding di 20 pixel sul lato destro. Questo ci lascia con 960 pixel per le larghezze totali delle colonne/margini: in questo caso, il padding è sottratto dalla larghezza totale del contenuto perché abbiamo impostato {{cssxref("box-sizing")}} su `border-box` su tutti gli elementi del sito (vedi [Il modello di box CSS alternativo](/it/docs/Learn_web_development/Core/Styling_basics/Box_model#the_alternative_css_box_model) per ulteriori spiegazioni).

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

Ora usa il contenitore di righe avvolto attorno a ciascuna riga della griglia per separare una riga dall'altra. Aggiungi la seguente regola sotto quella precedente:

```css
.row {
  clear: both;
}
```

Applicare questo clearing significa che non abbiamo bisogno di riempire completamente ciascuna riga con elementi per fare le dodici colonne complete. Le righe rimarranno separate e non interferiranno l'una con l'altra.

I margini tra le colonne sono larghi 20 pixel. Creiamo questi margini come un margine sul lato sinistro di ciascuna colonna, inclusa la prima colonna, per bilanciare i 20 pixel di padding sul lato destro del contenitore. Quindi abbiamo 12 margini in totale: 12 x 20 = 240.

Dobbiamo sottrarre ciò dalla nostra larghezza totale di 960 pixel, dandoci 720 pixel per le nostre colonne. Se ora lo dividiamo per 12, sappiamo che ciascuna colonna dovrebbe essere larga 60 pixel.

Il nostro passo successivo è creare una regola per la classe `.col`, facendola galleggiare a sinistra, dandole un {{cssxref("margin-left")}} di 20 pixel per formare il margine, e una {{cssxref("width")}} di 60 pixel. Aggiungi la seguente regola nella parte inferiore del tuo CSS:

```css
.col {
  float: left;
  margin-left: 20px;
  width: 60px;
  background: rgb(255 150 150);
}
```

La riga superiore di colonne singole si disporrà ora ordinatamente come una griglia.

> [!NOTE]
> Abbiamo anche dato a ogni colonna un colore rosso chiaro in modo da poter vedere esattamente quanto spazio occupa ciascuna.

I contenitori di layout che vogliamo che si estendano su più di una colonna devono ricevere classi speciali per regolare i loro valori di {{cssxref("width")}} al numero necessario di colonne (più margini tra di esse). Dobbiamo creare una classe aggiuntiva per consentire ai contenitori di estendersi da 2 a 12 colonne. Ogni larghezza è il risultato della somma della larghezza delle colonne di quel numero di colonne più le larghezze dei margini, che saranno sempre uno in meno rispetto al numero di colonne.

Aggiungi quanto segue alla fine del tuo CSS:

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

Con queste classi create, ora possiamo disporre colonne di larghezza diversa sulla griglia. Prova a salvare e caricare la pagina nel tuo browser per vedere gli effetti.

> [!NOTE]
> Se hai difficoltà a far funzionare l'esempio sopra, prova a confrontarlo con la nostra [versione finale](https://github.com/mdn/learning-area/blob/main/css/css-layout/grids/simple-grid-finished.html) su GitHub (vedi anche [come funziona dal vivo](https://mdn.github.io/learning-area/css/css-layout/grids/simple-grid-finished.html)).

Prova a modificare le classi sui tuoi elementi o addirittura ad aggiungere e rimuovere alcuni contenitori, per vedere come puoi variare il layout. Ad esempio, potresti far sembrare la seconda riga così:

```html
<div class="row">
  <div class="col span8">13</div>
  <div class="col span4">14</div>
</div>
```

Ora che hai un sistema di griglie funzionante, puoi definire le righe e il numero di colonne in ciascuna riga, quindi riempire ogni contenitore con il tuo contenuto richiesto. Fantastico!

### Creare una griglia fluida

La nostra griglia funziona bene, ma ha una larghezza fissa. Vogliamo davvero una griglia flessibile (fluida) che cresca e si restringa con lo spazio disponibile nel {{Glossary("viewport", "viewport")}} del browser. Per ottenere questo possiamo trasformare le larghezze in pixel di riferimento in percentuali.

L'equazione che trasforma una larghezza fissa in una a base di percentuale flessibile è la seguente.

```plain
target / context = result
```

Per la nostra larghezza delle colonne, il nostro **target width** è 60 pixel e il nostro **contesto** è il wrapper di 960 pixel. Possiamo utilizzare il seguente per calcolare una percentuale.

```plain
60 / 960 = 0.0625
```

Spostiamo quindi il punto decimale di 2 posizioni dandoci una percentuale del 6.25%. Quindi, nel nostro CSS possiamo sostituire la larghezza di 60 pixel della colonna con il 6.25%.

Dobbiamo fare lo stesso con la larghezza del margine:

```plain
20 / 960 = 0.02083333333
```

Quindi dobbiamo sostituire il {{cssxref("margin-left")}} di 20 pixel sulla nostra regola `.col` e il {{cssxref("padding-right")}} di 20 pixel su `.wrapper` con il 2.08333333%.

#### Aggiornare la nostra griglia

Per iniziare in questa sezione, fai una nuova copia della tua pagina d'esempio precedente, oppure fai una copia locale del nostro codice [simple-grid-finished.html](https://github.com/mdn/learning-area/blob/main/css/css-layout/grids/simple-grid-finished.html) da utilizzare come punto di partenza.

Aggiorna la seconda regola CSS (con il selettore `.wrapper`) come segue:

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

Non solo gli abbiamo dato una {{cssxref("width")}} percentuale, abbiamo anche aggiunto una proprietà {{cssxref("max-width")}} per impedire che il layout diventi troppo largo.

Successivamente, aggiorna la quarta regola CSS (con il selettore `.col`) in questo modo:

```css
.col {
  float: left;
  margin-left: 2.08333333%;
  width: 6.25%;
  background: rgb(255 150 150);
}
```

Ora viene la parte leggermente più laboriosa: dobbiamo aggiornare tutte le nostre regole `.col.span` per usare percentuali anziché larghezze in pixel. Ciò richiede un po' di tempo con una calcolatrice; per risparmiarti un po' di sforzo, lo abbiamo già fatto per te qui sotto.

Aggiorna il blocco inferiore di regole CSS con quanto segue:

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

Ora salva il tuo codice, caricalo in un browser e prova a cambiare la larghezza del viewport: dovresti vedere le larghezze delle colonne adattarsi bene di conseguenza.

> [!NOTE]
> Se hai difficoltà a far funzionare l'esempio sopra, prova a confrontarlo con la nostra [versione finale su GitHub](https://github.com/mdn/learning-area/blob/main/css/css-layout/grids/fluid-grid.html) (vedi anche [come funziona dal vivo](https://mdn.github.io/learning-area/css/css-layout/grids/fluid-grid.html)).

### Calcoli più facili usando la funzione calc()

Potresti utilizzare la funzione {{cssxref("calc", "calc()")}} per fare i calcoli direttamente all'interno del tuo CSS: questo ti permette di inserire equazioni matematiche semplici nei valori CSS, per calcolare quale dovrebbe essere un valore. È particolarmente utile quando ci sono calcoli complessi da fare, e puoi persino calcolare un valore utilizzando unità diverse, ad esempio "Voglio che l'altezza di questo elemento sia sempre il 100% dell'altezza del suo genitore, meno 50px". Vedi [questo esempio da un tutorial sull'API di registrazione MediaStream](/it/docs/Web/API/MediaStream_Recording_API/Using_the_MediaStream_Recording_API#keeping_the_interface_constrained_to_the_viewport_regardless_of_device_height_with_calc).

Comunque, torniamo ai nostri grid! Qualsiasi colonna che si estende su più colonne della nostra griglia ha una larghezza totale del 6.25% moltiplicata per il numero di colonne su cui si estende più il 2.08333333% moltiplicato per il numero di margini (che sarà sempre il numero di colonne meno 1). La funzione `calc()` ci consente di fare questo calcolo direttamente nel valore di width, quindi per qualsiasi elemento che si estende su 4 colonne possiamo fare questo, ad esempio:

```css
.col.span4 {
  width: calc((6.25% * 4) + (2.08333333% * 3));
}
```

Prova a sostituire il tuo blocco inferiore di regole con il seguente, quindi ricaricarlo nel browser per vedere se ottieni lo stesso risultato:

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
> Puoi vedere la nostra versione finale in [fluid-grid-calc.html](https://github.com/mdn/learning-area/blob/main/css/css-layout/grids/fluid-grid-calc.html) (vedi anche [come funziona dal vivo](https://mdn.github.io/learning-area/css/css-layout/grids/fluid-grid-calc.html)).

### Sistemi di griglia semantici contro "non semantici"

Aggiungere classi al markup per definire il layout significa che il tuo contenuto e il markup diventano legati alla tua presentazione visiva. Sentirai, a volte, descrivere questo utilizzo delle classi CSS come "non semantico", cioè descrive l'aspetto del contenuto piuttosto che un utilizzo semantico delle classi che descrive il contenuto stesso. Questo è il caso con le nostre classi `span2`, `span3`, ecc.

Questi non sono l'unico approccio. Potresti invece decidere la tua griglia e poi aggiungere le informazioni sulla dimensione alle regole per le classi semantiche esistenti. Ad esempio, se hai un {{htmlelement("div")}} con una classe `content` che vuoi che si estenda su 8 colonne, potresti copiare la larghezza dalla classe `span8`, dandoti una regola come questa:

```css
.content {
  width: calc((6.25% * 8) + (2.08333333% * 7));
}
```

> [!NOTE]
> Se dovessi utilizzare un preprocessore come [Sass](https://sass-lang.com/), potresti creare un mixin semplice per inserire quel valore per te.

### Abilitare i contenitori di offset nella nostra griglia

La griglia che abbiamo creato funziona bene fintanto che vogliamo che tutti i contenitori inizino allineati al lato sinistro della griglia. Se volessimo lasciare uno spazio vuoto di una colonna prima del primo contenitore, o tra i contenitori, dovremmo creare una classe di offset per aggiungere un margine sinistro al nostro sito per spingerlo visivamente attraverso la griglia. Più matematica!

Proviamo questo.

Inizia con il tuo codice precedente, oppure usa il nostro file [fluid-grid.html](https://github.com/mdn/learning-area/blob/main/css/css-layout/grids/fluid-grid.html) come punto di partenza.

Creiamo una classe nel nostro CSS che sposterà un elemento contenitore di una larghezza di una colonna. Aggiungi il seguente codice alla fine del tuo CSS:

```css
.offset-by-one {
  margin-left: calc(6.25% + (2.08333333% * 2));
}
```

Oppure se preferisci calcolare le percentuali da solo, usa questo:

```css
.offset-by-one {
  margin-left: 10.41666666%;
}
```

Ora puoi aggiungere questa classe a qualsiasi contenitore per lasciare uno spazio vuoto largo una colonna sul lato sinistro di esso. Ad esempio, se hai questo nel tuo HTML:

```html
<div class="col span6">14</div>
```

Prova a sostituirlo con

```html
<div class="col span5 offset-by-one">14</div>
```

> [!NOTE]
> Nota che devi ridurre il numero di colonne coperte, per fare spazio all'offset!

Prova a caricare e aggiornare per vedere la differenza, o controlla il nostro esempio [fluid-grid-offset.html](https://github.com/mdn/learning-area/blob/main/css/css-layout/grids/fluid-grid-offset.html) (vedi anche [come funziona dal vivo](https://mdn.github.io/learning-area/css/css-layout/grids/fluid-grid-offset.html)). L'esempio finale dovrebbe apparire così:

![La griglia ha 2 righe. La prima riga ha 12 elementi a griglia di larghezza uguale e la seconda riga ha 4 elementi di larghezze diverse. L'elemento 13 si estende su 1 colonna, l'elemento 14 su cinque colonne, il 15 su tre e il 16 su due. L'elemento 14 ha applicata la classe 'offset-by-one', che significa che inizia nella terza colonna, anziché nella seconda, lasciando uno spazio vuoto largo una colonna nella seconda-colonna della seconda-riga.](offset-grid-finished.png)

> [!NOTE]
> Come ulteriore esercizio, puoi implementare una classe `	offset-by-two`?

### Limitazioni della griglia con float

Quando si utilizza un sistema come questo, è necessario assicurarsi che le larghezze totali siano corrette e che non si includano elementi in una riga che si estendono su più colonne di quanto la riga possa contenere. A causa del modo in cui funzionano i float, se il numero di colonne della griglia diventa troppo largo per la griglia, gli elementi alla fine scenderanno alla linea successiva, rompendo la griglia.

Inoltre, tieni presente che se il contenuto degli elementi diventa più grande delle righe che occupano, queste usciranno fuori e appariranno in disordine.

La maggiore limitazione di questo sistema è che è essenzialmente monodimensionale. Ci stiamo occupando di colonne e di far estendere gli elementi attraverso colonne, ma non attraverso righe. È molto difficile con questi metodi di layout più vecchi controllare l'altezza degli elementi senza impostare esplicitamente un'altezza, e questo è anche un approccio molto inflessibile: funziona solo se puoi garantire che il tuo contenuto avrà un'altezza certa.

## Griglie Flexbox?

Se hai letto il nostro articolo precedente su [flexbox](/it/docs/Learn_web_development/Core/CSS_layout/Flexbox), potresti pensare che flexbox sia la soluzione ideale per creare un sistema a griglia. Ci sono molti sistemi di griglia basati su flexbox disponibili e flexbox può risolvere molti dei problemi che abbiamo già scoperto nella creazione della nostra griglia sopra.

Tuttavia, flexbox non è mai stato progettato come sistema di griglia e presenta un nuovo set di sfide quando utilizzato come tale. Come semplice esempio di questo, possiamo prendere lo stesso markup d'esempio che abbiamo usato sopra e utilizzare il seguente CSS per stilizzare le classi `wrapper`, `row`, e `col`:

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

Puoi provare a fare queste sostituzioni nel tuo esempio, o guardare il nostro esempio [flexbox-grid.html](https://github.com/mdn/learning-area/blob/main/css/css-layout/grids/flexbox-grid.html) (vedi anche [come funziona dal vivo](https://mdn.github.io/learning-area/css/css-layout/grids/flexbox-grid.html)).

Qui stiamo trasformando ogni riga in un contenitore flessibile. Con una griglia basata su flexbox abbiamo ancora bisogno di righe per permetterci di avere elementi che non sommano al 100%. Impostiamo quel contenitore su `display: flex`.

Su `.col` impostiamo il primo valore della proprietà {{cssxref("flex")}} ({{cssxref("flex-grow")}}) su 1 in modo che i nostri elementi possano crescere, il secondo valore ({{cssxref("flex-shrink")}}) su 1 in modo che gli elementi possano ridursi, e il terzo valore ({{cssxref("flex-basis")}}) su `auto`. Poiché il nostro elemento ha una `{{cssxref("width")}}` impostata, `auto` utilizzerà quella larghezza come valore `flex-basis`.

Sulla riga superiore otteniamo dodici scatole ordinate sulla griglia e crescono e si riducono in modo uguale man mano che cambiamo la larghezza del viewport. Sulla riga successiva, tuttavia, abbiamo solo quattro elementi e questi crescono e si riducono da quella base di 60px. Con solo quattro di loro, possono crescere molto più degli elementi nella riga sopra, il risultato è che occupano tutti la stessa larghezza nella seconda riga.

![La griglia ha due righe. Ogni riga è un contenitore flessibile. La prima riga ha dodici elementi flex di larghezza uguale. La seconda riga ha quattro elementi flex di larghezza uguale.](flexbox-grid-incomplete.png)

Per correggere questo, dobbiamo ancora includere le nostre classi `span` per fornire una larghezza che sostituirà il valore utilizzato da `flex-basis` per quell'elemento.

Non rispettano inoltre la griglia utilizzata dagli elementi sopra perché non ne sanno nulla.

Flexbox è **monodimensionale** per design. Si occupa di una singola dimensione, quella di una riga o di una colonna. Non possiamo creare una griglia rigida per colonne e righe, ciò significa che se vogliamo utilizzare flexbox per la nostra griglia, dobbiamo ancora calcolare percentuali come per il layout con float.

Nel tuo progetto potresti comunque scegliere di utilizzare una 'griglia' flexbox a causa delle capacità di allineamento e distribuzione degli spazi aggiuntive che fornisce rispetto ai float. Dovresti, tuttavia, essere consapevole che stai ancora utilizzando uno strumento per qualcosa di diverso da quello per cui è stato progettato. Quindi potresti sentirti come se ti stesse costringendo a saltare attraverso cerchi aggiuntivi per ottenere il risultato desiderato.

## Sistemi di griglia di terze parti

Ora che comprendiamo la matematica dietro i nostri calcoli delle griglie, siamo in una buona posizione per esaminare alcuni dei sistemi di griglia di terze parti comunemente utilizzati. Se cerchi "framework a griglia CSS" sul web, troverai una vasta lista di opzioni tra cui scegliere. I framework popolari come [Bootstrap](https://getbootstrap.com/) e [Foundation](https://get.foundation/) includono un sistema di griglia. Ci sono anche sistemi di griglia standalone, sviluppati utilizzando CSS o usando preprocessori.

Diamo un'occhiata a uno di questi sistemi standalone poiché dimostra tecniche comuni per lavorare con un framework a griglia. La griglia che useremo fa parte di Skeleton, un semplice framework CSS.

Per iniziare, visita il sito web [Skeleton](http://getskeleton.com/) e scegli "Download" per scaricare il file ZIP. Scompatta questo e copia i file skeleton.css e normalize.css in una nuova directory.

Fai una copia del nostro file [html-skeleton.html](https://github.com/mdn/learning-area/blob/main/css/css-layout/grids/html-skeleton.html) e salvalo nella stessa directory dei CSS skeleton e normalize.

Includi i CSS skeleton e normalize nella pagina HTML aggiungendo quanto segue nella sua intestazione:

```html
<link href="normalize.css" rel="stylesheet" />
<link href="skeleton.css" rel="stylesheet" />
```

Skeleton include più di un sistema di griglia: contiene anche CSS per la tipografia e altri elementi della pagina che puoi usare come punto di partenza. Lasceremo questi ai valori predefiniti per ora, comunque: è la griglia che ci interessa qui.

> **Nota:** [Normalize](https://necolas.github.io/normalize.css/) è una piccola e utile libreria CSS scritta da Nicolas Gallagher, che esegue automaticamente alcune correzioni di layout di base utili e rende lo stile elementare predefinito più coerente tra i browser.

Useremo un HTML simile al nostro esempio precedente. Aggiungi quanto segue all'interno del corpo del tuo HTML:

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

Per iniziare a utilizzare Skeleton, dobbiamo dare al wrapper {{htmlelement("div")}} una classe `container`, questo è già incluso nel nostro HTML. Ciò centra il contenuto con una larghezza massima di 960 pixel. Puoi vedere come le scatole ora non diventano mai più larghe di 960 pixel.

Puoi controllare nel file skeleton.css per vedere il CSS che viene utilizzato quando applichiamo questa classe. Il `<div>` è centrato utilizzando margini sinistri e destri `auto`, e viene applicato un padding di 20 pixel a sinistra e a destra. Skeleton imposta anche la proprietà {{cssxref("box-sizing")}} su `border-box` proprio come abbiamo fatto prima, quindi i padding e i bordi di questo elemento saranno inclusi nella larghezza totale.

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

Gli elementi possono essere parte della griglia solo se sono all'interno di una riga, quindi come nel nostro esempio precedente, abbiamo bisogno di un'ulteriore `<div>` o altro elemento con una classe `row` nidificato tra gli elementi `<div>` del contenuto e il `<div>` del contenitore. Lo abbiamo già fatto anche noi.

Ora diamo un'occhiata alle scatole contenitore. Skeleton si basa su una griglia a 12 colonne. Le scatole sulla linea superiore devono tutte avere classi `one column` per farle coprire una sola colonna.

Aggiungile ora, come mostrato nel seguente frammento:

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

Successivamente, dai ai contenitori della seconda riga classi che spiegano il numero di colonne che dovrebbero coprire, come segue:

```html
<div class="row">
  <div class="one column">13</div>
  <div class="six columns">14</div>
  <div class="three columns">15</div>
  <div class="two columns">16</div>
</div>
```

Prova a salvare il tuo file HTML e a caricarlo nel tuo browser per vedere l'effetto.

> [!NOTE]
> Se hai difficoltà a far funzionare questo esempio, prova ad allargare la finestra che stai usando per visualizzarlo (la griglia non sarà visualizzata come descritto qui se la finestra è troppo stretta). Se non funziona, prova a confrontarlo con il nostro file [html-skeleton-finished.html](https://github.com/mdn/learning-area/blob/main/css/css-layout/grids/html-skeleton-finished.html) (vedi anche [come funziona dal vivo](https://mdn.github.io/learning-area/css/css-layout/grids/html-skeleton-finished.html)).

Se guardi nel file skeleton.css, puoi vedere come funziona. Ad esempio, Skeleton ha definito quanto segue per stilizzare gli elementi a cui sono state aggiunte classi "three columns".

```css
.three.columns {
  width: 22%;
}
```

Tutto ciò che Skeleton (o qualsiasi altro framework a griglia) sta facendo è impostare classi predefinite che puoi usare aggiungendole al tuo markup. È esattamente come se facessi tu stesso il lavoro di calcolo di queste percentuali.

Come puoi vedere, dobbiamo scrivere molto poco CSS utilizzando Skeleton. Gestisce tutto il floating per noi quando aggiungiamo classi al nostro markup. È questa capacità di trasferire la responsabilità del layout a qualcos'altro che ha reso una scelta convincente l'uso di un framework per un sistema di griglia! Tuttavia, al giorno d'oggi, con il layout a griglia CSS, molti sviluppatori si stanno allontanando da questi framework per utilizzare la griglia nativa integrata che CSS fornisce.

## Sommario

Ora capisci come vengono creati vari sistemi di griglia, il che sarà utile per lavorare con siti più vecchi e per comprendere la differenza tra la griglia nativa del layout a griglia CSS e questi sistemi più antichi.
