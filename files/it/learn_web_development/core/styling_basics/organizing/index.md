---
title: Organizzare il suo CSS
slug: Learn_web_development/Core/Styling_basics/Organizing
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Man mano che inizia a lavorare su fogli di stile più grandi e progetti importanti, scoprirà che mantenere un file CSS enorme può essere impegnativo. In questo articolo daremo uno sguardo breve ad alcune delle migliori pratiche per scrivere il suo CSS in modo che sia facilmente gestibile, e alcune delle soluzioni che troverà in uso da altri per migliorare la manutenibilità.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Installing_software"
          >Software di base installato</a
        >, conoscenza di base di
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Dealing_with_files"
          >lavorare con i file</a
        >, nozioni di base di HTML (studiare
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Introduzione a HTML</a
        >), e un'idea di come funziona CSS (studiare
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Nozioni di base su CSS Styling</a>.)
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare alcuni consigli e migliori pratiche per organizzare i fogli di stile, e scoprire alcune delle convenzioni di nomenclatura e degli strumenti comunemente in uso per aiutare con l'organizzazione del CSS e il lavoro di squadra.
      </td>
    </tr>
  </tbody>
</table>

## Consigli per mantenere il suo CSS ordinato

Ecco alcuni suggerimenti generali per mantenere i suoi fogli di stile organizzati e ordinati.

### Il suo progetto ha una guida di stile del codice?

Se sta lavorando con un team su un progetto esistente, la prima cosa da controllare è se il progetto ha una guida di stile CSS esistente. La guida di stile del team dovrebbe sempre prevalere sulle sue preferenze personali. Spesso non esiste un modo giusto o sbagliato di fare le cose, ma la coerenza è importante.

Per esempio, dia un'occhiata alle [linee guida CSS per gli esempi di codice MDN](/it/docs/MDN/Writing_guidelines/Code_style_guide/CSS).

### Mantenga la coerenza

Se le tocca impostare le regole per il progetto o sta lavorando da solo, allora la cosa più importante da fare è mantenere le cose coerenti. La coerenza può essere applicata in molti modi, come l'uso delle stesse convenzioni di denominazione per le classi, la scelta di un metodo per descrivere i colori, o il mantenimento di una formattazione coerente. (Ad esempio, userà tabulazioni o spazi per indentare il suo codice? Se usa spazi, quanti spazi?)

Avere un insieme di regole che segue sempre riduce la quantità di sforzo mentale necessario quando si scrive CSS, poiché alcune decisioni sono già state prese.

### Formattare CSS leggibile

Ci sono un paio di modi in cui vedrà formattato il CSS. Alcuni sviluppatori mettono tutte le regole su un'unica riga, come segue:

```css-nolint
.box {background-color: #567895; }
h2 {background-color: black; color: white; }
```

Altri sviluppatori preferiscono spezzare tutto su una nuova riga:

```css
.box {
  background-color: #567895;
}

h2 {
  background-color: black;
  color: white;
}
```

A CSS non importa quale lei utilizzi. Personalmente trovo che sia più leggibile avere ogni coppia di proprietà e valore su una nuova riga.

### Commentare il suo CSS

Aggiungere commenti al suo CSS aiuterà qualsiasi futuro sviluppatore a lavorare con il suo file CSS, ma aiuterà anche lei quando tornerà al progetto dopo una pausa.

```css
/* This is a CSS comment
It can be broken onto multiple lines. */
```

Un buon consiglio è aggiungere un blocco di commenti tra sezioni logiche nel suo foglio di stile, per aiutare a localizzare rapidamente diverse sezioni quando lo esamina, o persino per darle qualcosa da cercare per saltare direttamente a quella parte del CSS. Se utilizza una stringa che non apparirà nel codice, può saltare da una sezione all'altra cercandola — qui sotto abbiamo usato `||`.

```css
/* || General styles */

/* … */

/* || Typography */

/* … */

/* || Header and Main Navigation */

/* … */
```

Non deve commentare ogni singola cosa nel suo CSS, poiché molte di esse saranno autoesplicative. Ciò che dovrebbe commentare sono le cose in cui ha preso una decisione particolare per una ragione.

Potrebbe aver utilizzato una proprietà CSS in modo specifico per superare incompatibilità con vecchi browser, per esempio:

```css
.box {
  background-color: red; /* fallback for older browsers that don't support gradients */
  background-image: linear-gradient(to right, #ff0000, #aa0000);
}
```

Forse ha seguito un tutorial per ottenere qualcosa, e il CSS non è molto autoesplicativo o riconoscibile. In tal caso, potrebbe aggiungere l'URL del tutorial ai commenti. Si ringrazierà quando tornerà su questo progetto tra un anno o giù di lì e potrà ricordare vagamente che c'era un ottimo tutorial su quella cosa, ma non potrà ricordare da dove proviene.

### Creare sezioni logiche nel suo stylesheet

È una buona idea avere tutto lo styling comune all'inizio del foglio di stile. Questo significa tutti gli stili che si applicheranno generalmente a meno che non si faccia qualcosa di particolare con quel elemento. Tipicamente si avranno regole impostate per:

- `body`
- `p`
- `h1`, `h2`, `h3`, `h4`, `h5`
- `ul` e `ol`
- Le proprietà del `table`
- Collegamenti

In questa sezione del foglio di stile stiamo fornendo degli stili di default per il tipo sul sito, impostando uno stile di default per le tabelle dati e le liste e così via.

```css
/* || GENERAL STYLES */

body {
  /* … */
}

h1,
h2,
h3,
h4 {
  /* … */
}

ul {
  /* … */
}

blockquote {
  /* … */
}
```

Dopo questa sezione, potremmo definire alcune classi di utilità, per esempio, una classe che rimuove lo stile predefinito delle liste che vogliamo visualizzare come elementi flessibili o in qualche altro modo. Se ha alcune scelte di stile che sa che vorrà applicare a molti elementi diversi, possono essere messe in questa sezione.

```css
/* || UTILITIES */

.no-bullets {
  list-style: none;
  margin: 0;
  padding: 0;
}

/* … */
```

Successivamente possiamo aggiungere tutto ciò che viene utilizzato a livello di sito. Potrebbe trattarsi di elementi come il layout base della pagina, l'intestazione, lo stile di navigazione, e così via.

```css
/* SITEWIDE */

.main-nav {
  /* … */
}

.logo {
  /* … */
}
```

Infine, includeremo CSS per cose specifiche, suddivise per contesto, pagina o persino componente in cui vengono utilizzate.

```css
/* || STORE PAGES */

.product-listing {
  /* … */
}

.product-box {
  /* … */
}
```

Ordinando le cose in questo modo, avremo almeno un'idea di quale parte del foglio di stile stiamo cercando qualcosa che vogliamo cambiare.

### Evitare selettori eccessivamente specifici

Se crea selettori molto specifici, spesso scoprirà che ha bisogno di duplicare parti del suo CSS per applicare le stesse regole a un altro elemento. Ad esempio, potrebbe avere qualcosa come il selettore qui sotto, che applica la regola a un `<p>` con una classe `box` all'interno di un `<article>` con una classe `main`.

```css
article.main p.box {
  border: 1px solid #ccc;
}
```

Se poi volesse applicare le stesse regole a qualcosa al di fuori di `main`, o a qualcosa di diverso da un `<p>`, dovrebbe aggiungere un altro selettore a queste regole o creare un intero nuovo set di regole. Invece, potrebbe usare il selettore `.box` per applicare la sua regola a qualsiasi elemento che ha la classe `box`:

```css
.box {
  border: 1px solid #ccc;
}
```

Ci saranno momenti in cui ha senso rendere qualcosa più specifico; tuttavia, questo sarà generalmente un'eccezione piuttosto che la pratica usuale.

### Suddividere grandi fogli di stile in più fogli più piccoli

Nei casi in cui ha stili molto diversi per parti distinte del sito, potrebbe voler avere un foglio di stile che include tutte le regole globali, oltre ad alcuni fogli di stile più piccoli che includono le regole specifiche necessarie per quelle sezioni. Può collegare più fogli di stile da una pagina, e si applicano le normali regole della cascata, con le regole nei fogli di stile collegati successivamente che vengono dopo le regole nei fogli di stile collegati in precedenza.

Ad esempio, potremmo avere un negozio online come parte del sito, con molto CSS utilizzato solo per lo stile delle liste di prodotti e dei moduli necessari per il negozio. Avrebbe senso avere quelle cose in un diverso foglio di stile, collegato solo alle pagine del negozio.

Questo può rendere più semplice mantenere organizzato il suo CSS, e significa anche che se più persone stanno lavorando sul CSS, avrà meno situazioni in cui due persone devono lavorare sullo stesso foglio di stile contemporaneamente, portando a conflitti nel controllo della sorgente.

## Altri strumenti che possono aiutare

CSS stesso non ha molto in termini di organizzazione integrata; quindi, il livello di coerenza nel suo CSS dipenderà in gran parte da lei. La comunità del web ha sviluppato vari strumenti e approcci che possono aiutarla a gestire progetti CSS più grandi. Poiché probabilmente incontrerà questi aiuti lavorando con altre persone, e poiché spesso sono di aiuto in generale, abbiamo incluso una breve guida su alcuni di essi.

### Metodologie CSS

Invece di dover ideare le sue regole per scrivere CSS, potrebbe beneficiare dell'adozione di uno degli approcci già progettati dalla comunità e testati su molti progetti. Queste metodologie sono essenzialmente guide di codifica CSS che adottano un approccio molto strutturato per scrivere e organizzare CSS. Tipicamente tendono a produrre CSS più verboso di quanto avrebbe fatto se avesse scritto e ottimizzato ogni selettore per un set di regole personalizzato per quel progetto.

Tuttavia, guadagna molta struttura adottandone uno. Poiché molti di questi sistemi sono ampiamente utilizzati, altri sviluppatori sono più propensi a capire l'approccio che sta usando e a scrivere il loro CSS nello stesso modo, piuttosto che dover elaborare la sua metodologia personale da zero.

#### OOCSS

La maggior parte degli approcci che incontrerà deve qualcosa al concetto di Object Oriented CSS (OOCSS), un approccio reso popolare dal [lavoro di Nicole Sullivan](https://github.com/stubbornella/oocss/wiki). L'idea di base di OOCSS è separare il suo CSS in oggetti riutilizzabili, che possono essere utilizzati ovunque ne abbia bisogno nel suo sito. L'esempio standard di OOCSS è il pattern descritto come [The Media Object](/it/docs/Web/CSS/Layout_cookbook/Media_objects). Questo è un modello con un'immagine di dimensioni fisse, un video o un altro elemento da un lato e contenuti flessibili dall'altro. È un modello che vediamo dappertutto sui siti web per commenti, liste, e così via.

Se non sta adottando un approccio OOCSS potrebbe creare un CSS personalizzato per i diversi luoghi in cui viene usato questo pattern, ad esempio, creando due classi, una chiamata `comment` con un sacco di regole per le parti del componente, e un'altra chiamata `list-item` con quasi le stesse regole della classe `comment` tranne per alcune piccole differenze. Le differenze tra questi due componenti sono che l'elemento della lista ha un bordo inferiore e le immagini nei commenti hanno un bordo mentre le immagini dell'elemento della lista no.

```css
.comment {
  display: grid;
  grid-template-columns: 1fr 3fr;
}

.comment img {
  border: 1px solid grey;
}

.comment .content {
  font-size: 0.8rem;
}

.list-item {
  display: grid;
  grid-template-columns: 1fr 3fr;
  border-bottom: 1px solid grey;
}

.list-item .content {
  font-size: 0.8rem;
}
```

In OOCSS, creerebbe un pattern chiamato `media` che avrebbe tutto il CSS comune per entrambi i pattern — una classe base per cose che sono generalmente la forma del media object. Poi aggiungeremmo una classe aggiuntiva per gestire quelle piccole differenze, estendendo così quello stile in modi specifici.

```css
.media {
  display: grid;
  grid-template-columns: 1fr 3fr;
}

.media .content {
  font-size: 0.8rem;
}

.comment img {
  border: 1px solid grey;
}

.list-item {
  border-bottom: 1px solid grey;
}
```

Nel suo HTML, il commento avrebbe bisogno di entrambe le classi `media` e `comment` applicate:

```html
<div class="media comment">
  <img src="" alt="" />
  <div class="content"></div>
</div>
```

L'elemento della lista avrebbe `media` e `list-item` applicate:

```html
<ul>
  <li class="media list-item">
    <img src="" alt="" />
    <div class="content"></div>
  </li>
</ul>
```

Il lavoro che Nicole Sullivan ha fatto nel descrivere e promuovere questo approccio significa che anche le persone che non stanno seguendo strettamente un approccio OOCSS oggi riutilizzeranno generalmente il CSS in questo modo — è entrato nella nostra comprensione come un buon modo di affrontare le cose in generale.

#### BEM

BEM sta per Block Element Modifier. In BEM un blocco è un'entità autonoma come un pulsante, un menu o un logo. Un elemento è qualcosa come un elemento di lista o un titolo che è legato al blocco in cui si trova. Un modificatore è un flag su un blocco o elemento che cambia lo stile o il comportamento. Sarà in grado di riconoscere il codice che utilizza BEM per l'uso estensivo di trattini e sottolineature nelle classi CSS. Per esempio, guardi le classi applicate a questo HTML dalla pagina su [Convenzioni di denominazione BEM](https://getbem.com/naming/):

```html
<form class="form form--theme-xmas form--simple">
  <label class="label form__label" for="inputId"></label>
  <input class="form__input" type="text" id="inputId" />

  <input
    class="form__submit form__submit--disabled"
    type="submit"
    value="Submit" />
</form>
```

Le classi aggiuntive sono simili a quelle utilizzate nell'esempio OOCSS; tuttavia, usano le convenzioni di denominazione rigorose di BEM.

BEM è ampiamente utilizzato nei progetti web più grandi e molte persone scrivono il loro CSS in questo modo. È probabile che incontrerà esempi, anche in tutorial, che usano la sintassi BEM, senza menzionare perché il CSS è strutturato in un certo modo.

Legga di più su questo sistema [BEM 101](https://css-tricks.com/bem-101/) su CSS Tricks.

#### Altri sistemi comuni

Esiste un gran numero di questi sistemi in uso. Altri approcci popolari includono [Scalable and Modular Architecture for CSS (SMACSS)](https://smacss.com/), creato da Jonathan Snook, [ITCSS](https://itcss.io/) di Harry Roberts, e [Atomizer CSS (ACSS)](https://acss-io.github.io/atomizer/), originariamente creato da Yahoo!. Se si imbatte in un progetto che utilizza uno di questi approcci, allora il vantaggio è che potrà cercare e trovare molti articoli e guide per aiutarla a capire come codificare nello stesso stile.

Lo svantaggio di utilizzare un sistema del genere è che possono sembrare eccessivamente complessi, specialmente per i progetti più piccoli.

### Sistemi di build per CSS

Un altro modo per organizzare il CSS è sfruttare alcuni degli strumenti disponibili per gli sviluppatori front-end, che permettono di adottare un approccio leggermente più programmatico alla scrittura del CSS. Ci sono una serie di strumenti, che chiamiamo _pre-processori_ e _post-processori_. Un pre-processore esamina i suoi file grezzi e li trasforma in un foglio di stile, mentre un post-processore prende il foglio di stile finito e fa qualcosa ad esso — forse per ottimizzarlo in modo che si carichi più velocemente.

Usare uno qualsiasi di questi strumenti richiederà che il suo ambiente di sviluppo sia in grado di eseguire gli script che fanno il pre- e post-processing. Molti editor di codice possono farlo per lei, o può installare strumenti da riga di comando per aiutare.

Il pre-processore più popolare è [Sass](https://sass-lang.com/). Questo non è un tutorial su Sass, quindi spiegherò brevemente un paio delle cose che Sass può fare, che sono davvero utili in termini di organizzazione anche se non usa nessuna delle altre caratteristiche di Sass. Se vuole imparare molto di più su Sass, inizi con l'articolo [Sass basics](https://sass-lang.com/guide/), poi passi alla loro altra documentazione.

#### Definire variabili

CSS ora ha nativi [custom properties](/it/docs/Web/CSS/CSS_cascading_variables/Using_CSS_custom_properties), rendendo questa funzione sempre meno importante. Tuttavia, uno dei motivi per cui potrebbe usare Sass è per essere in grado di definire tutti i colori e i caratteri usati in un progetto come impostazioni, poi usare quella variabile in tutto il progetto. Questo significa che se si rende conto di avere usato la tonalità sbagliata di blu, deve solo cambiarla in un posto.

Se creiamo una variabile chiamata `$base-color`, come nella prima riga qui sotto, possiamo poi usarla nel foglio di stile ovunque sia richiesto quel colore.

```scss
$base-color: #c6538c;

.alert {
  border: 1px solid $base-color;
}
```

Una volta compilato in CSS, otterrebbe il seguente CSS nel foglio di stile finale.

```css
.alert {
  border: 1px solid #c6538c;
}
```

#### Compilare fogli di stile dei componenti

Ho menzionato sopra che un modo per organizzare il CSS è suddividere i fogli di stile in fogli di stile più piccoli. Quando si usa Sass è possibile portare questo ad un altro livello e avere moltissimi fogli di stile molto piccoli — anche fino al punto di avere un foglio di stile separato per ogni componente. Usando la funzionalità inclusa in Sass (partials), questi possono essere tutti compilati insieme in uno o un piccolo numero di fogli di stile da collegare effettivamente al suo sito web.

Quindi, per esempio, con [partials](https://sass-lang.com/documentation/at-rules/use/#partials), potrebbe avere molti file di stile all'interno di una directory, ad esempio `foundation/_code.scss`, `foundation/_lists.scss`, `foundation/_footer.scss`, `foundation/_links.scss`, ecc. Potrebbe poi usare la regola `@use` di Sass per caricarli in altri fogli di stile:

```scss
// foundation/_index.scss
@use "code";
@use "lists";
@use "footer";
@use "links";
```

Se i partials sono tutti caricati in un file indice, come suggerisce sopra, può quindi caricare quell'intera directory in un altro foglio di stile in un colpo solo:

```scss
// style.scss
@use "foundation";
```

> [!NOTE]
> Un modo semplice per provare Sass è usare [CodePen](https://codepen.io/) — può abilitare Sass per il suo CSS nelle Impostazioni di una Penna, e CodePen eseguirà poi il parser Sass per lei in modo che possa vedere la pagina risultante con il CSS regolare applicato. A volte troverà che i tutorial CSS hanno usato Sass piuttosto che plain CSS nei loro demo su CodePen, quindi è utile sapere qualcosa a riguardo.

#### Post-elaborazione per l'ottimizzazione

Se è preoccupato di aggiungere dimensione ai suoi fogli di stile, per esempio, aggiungendo molti commenti e spazi bianchi aggiuntivi, allora un passo di post-elaborazione potrebbe essere ottimizzare il CSS eliminando tutto ciò che è superfluo nella versione di produzione. Un esempio di una soluzione di post-processore per fare questo potrebbe essere [cssnano](https://cssnano.github.io/cssnano/).
