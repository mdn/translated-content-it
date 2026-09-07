---
title: Organizzare il CSS
slug: Learn_web_development/Core/Styling_basics/Organizing
l10n:
  sourceCommit: 85fccefc8066bd49af4ddafc12c77f35265c7e2d
---

Quando si inizia a lavorare su fogli di stile più grandi e progetti di grandi dimensioni, si scopre che mantenere un enorme file CSS può essere impegnativo. In questo articolo verranno esaminate brevemente alcune buone pratiche per scrivere CSS facilmente manutenibile e alcune delle soluzioni utilizzate da altri per migliorare la manutenibilità.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Installing_software"
          >Software di base installato</a
        >, conoscenza di base del
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Dealing_with_files"
          >lavoro con i file</a
        >, basi di HTML (studiare
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Introduzione a HTML</a
        >) e un'idea di come funziona CSS (studiare
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Basi dello stile CSS</a>.)
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare alcuni suggerimenti e buone pratiche per organizzare i fogli di stile
        e conoscere alcune convenzioni di denominazione e strumenti di uso comune
        utili per l'organizzazione del CSS e il lavoro in team.
      </td>
    </tr>
  </tbody>
</table>

## Suggerimenti per mantenere ordinato il CSS

Ecco alcuni suggerimenti generali su come mantenere organizzati e ordinati i fogli di stile.

### Il progetto dispone di una guida allo stile del codice?

Se si lavora in un team su un progetto esistente, la prima cosa da verificare è se il progetto dispone già di una guida allo stile per il CSS. La guida allo stile del team dovrebbe sempre prevalere sulle preferenze personali. Spesso non esiste un modo giusto o sbagliato di fare le cose, ma la coerenza è importante.

Per esempio, consultare le [linee guida CSS per gli esempi di codice di MDN](/it/docs/MDN/Writing_guidelines/Code_style_guide/CSS).

### Mantenere la coerenza

Se si possono stabilire le regole per il progetto o si lavora da soli, la cosa più importante è mantenere la coerenza. La coerenza può essere applicata in molti modi, ad esempio usando le stesse convenzioni di denominazione per le classi, scegliendo un unico metodo per descrivere i colori o mantenendo una formattazione coerente. Ad esempio, verranno utilizzate tabulazioni o spazi per rientrare il codice? Se si usano spazi, quanti?

Avere un insieme di regole da seguire sempre riduce il carico mentale necessario per scrivere CSS, poiché alcune decisioni sono già state prese.

### Formattare CSS leggibile

Esistono un paio di modi in cui si può trovare CSS formattato. Alcuni sviluppatori inseriscono tutte le regole su una sola riga, in questo modo:

```css-nolint
.box {background-color: #567895; }
h2 {background-color: black; color: white; }
```

Altri sviluppatori preferiscono inserire ogni elemento su una nuova riga:

```css
.box {
  background-color: #567895;
}

h2 {
  background-color: black;
  color: white;
}
```

Per CSS non fa differenza quale metodo venga usato. Riteniamo personalmente più leggibile avere ogni coppia proprietà-valore su una nuova riga.

### Commentare il CSS

Aggiungere commenti al CSS aiuterà qualsiasi futuro sviluppatore a lavorare con il file CSS, ma sarà utile anche quando si tornerà al progetto dopo una pausa.

```css
/* This is a CSS comment
It can be broken onto multiple lines. */
```

Un buon suggerimento è aggiungere anche un blocco di commenti tra le sezioni logiche del foglio di stile, per individuare rapidamente le diverse sezioni durante la scansione del file o persino per avere qualcosa da cercare per passare direttamente a quella parte del CSS. Se si utilizza una stringa che non comparirà nel codice, è possibile passare da una sezione all'altra cercandola: qui sotto è stato usato `||`.

```css
/* || General styles */

/* … */

/* || Typography */

/* … */

/* || Header and Main Navigation */

/* … */
```

Non è necessario commentare ogni singolo elemento del CSS, poiché gran parte di esso sarà autoesplicativa. È opportuno commentare le parti in cui è stata presa una decisione specifica per un determinato motivo.

Potrebbe essere stata usata una proprietà CSS in un modo specifico per aggirare incompatibilità con browser meno recenti, ad esempio:

```css
.box {
  background-color: red; /* fallback for older browsers that don't support gradients */
  background-image: linear-gradient(to right, red, #aa0000);
}
```

Forse è stato seguito un tutorial per ottenere qualcosa e il CSS non è molto autoesplicativo o riconoscibile. In questo caso, è possibile aggiungere l'URL del tutorial nei commenti. Sarà utile quando si tornerà a questo progetto tra circa un anno e si ricorderà vagamente che esisteva un ottimo tutorial su quella cosa, ma non da dove proveniva.

### Creare sezioni logiche nel foglio di stile

È una buona idea inserire per primo nel foglio di stile tutto lo stile comune. Ciò significa tutti gli stili che verranno generalmente applicati, a meno che non venga fatto qualcosa di speciale con quell'elemento. In genere saranno configurate regole per:

- `body`
- `p`
- `h1`, `h2`, `h3`, `h4`, `h5`
- `ul` e `ol`
- Le proprietà di `table`
- Collegamenti

In questa sezione del foglio di stile viene fornito lo stile predefinito per il testo del sito, viene impostato uno stile predefinito per tabelle di dati ed elenchi e così via.

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

Dopo questa sezione, si potrebbero definire alcune classi di utilità, ad esempio una classe che rimuove lo stile predefinito degli elenchi per gli elenchi che verranno visualizzati come elementi flex o in altro modo. Se esistono alcune scelte di stile che si sa di voler applicare a molti elementi diversi, possono essere inserite in questa sezione.

```css
/* || UTILITIES */

.no-bullets {
  list-style: none;
  margin: 0;
  padding: 0;
}

/* … */
```

Si può quindi aggiungere tutto ciò che viene usato nell'intero sito. Potrebbero essere elementi come il layout di base della pagina, l'intestazione, lo stile della navigazione e così via.

```css
/* SITEWIDE */

.main-nav {
  /* … */
}

.logo {
  /* … */
}
```

Infine, verrà incluso il CSS per elementi specifici, suddiviso in base al contesto, alla pagina o persino al componente in cui viene utilizzato.

```css
/* || STORE PAGES */

.product-listing {
  /* … */
}

.product-box {
  /* … */
}
```

Ordinando le cose in questo modo, si ha almeno un'idea di quale parte del foglio di stile cercare per trovare qualcosa che si desidera modificare.

### Evitare selettori eccessivamente specifici

Se vengono creati selettori molto specifici, spesso sarà necessario duplicare parti del CSS per applicare le stesse regole a un altro elemento. Ad esempio, potrebbe esserci un selettore come quello riportato di seguito, che applica la regola a un `<p>` con classe `box` all'interno di un `<article>` con classe `main`.

```css
article.main p.box {
  border: 1px solid #cccccc;
}
```

Se poi si volessero applicare le stesse regole a qualcosa al di fuori di `main`, o a qualcosa di diverso da un `<p>`, sarebbe necessario aggiungere un altro selettore a queste regole o creare un insieme di regole completamente nuovo. Invece, si potrebbe usare il selettore `.box` per applicare la regola a qualsiasi elemento che abbia la classe `box`:

```css
.box {
  border: 1px solid #cccccc;
}
```

Ci saranno casi in cui rendere qualcosa più specifico ha senso; tuttavia, in genere questa sarà un'eccezione anziché la pratica abituale.

### Suddividere i fogli di stile grandi in più fogli più piccoli

Nei casi in cui esistono stili molto diversi per parti distinte del sito, potrebbe essere opportuno avere un foglio di stile che includa tutte le regole globali, oltre ad alcuni fogli di stile più piccoli che includano le regole specifiche necessarie per tali sezioni. È possibile collegare più fogli di stile da una pagina e si applicano le normali regole della cascata, con le regole nei fogli di stile collegati successivamente che vengono dopo le regole nei fogli di stile collegati in precedenza.

Ad esempio, potrebbe esserci un negozio online come parte del sito, con molto CSS usato solo per lo stile degli elenchi di prodotti e dei moduli necessari al negozio. Avrebbe senso inserire tali elementi in un foglio di stile diverso, collegato solo nelle pagine del negozio.

Questo può rendere più semplice mantenere organizzato il CSS e significa anche che, se più persone lavorano sul CSS, ci saranno meno situazioni in cui due persone devono lavorare contemporaneamente sullo stesso foglio di stile, causando conflitti nel controllo del codice sorgente.

## Altri strumenti che possono essere utili

CSS in sé non offre molto in termini di organizzazione integrata; pertanto, il livello di coerenza del CSS dipenderà in gran parte dallo sviluppatore. La comunità web ha sviluppato vari strumenti e approcci che possono aiutare a gestire progetti CSS più grandi. Poiché è probabile incontrare questi strumenti quando si lavora con altre persone e poiché sono spesso utili in generale, è inclusa una breve guida ad alcuni di essi.

### Metodologie CSS

Invece di dover inventare regole personali per scrivere CSS, può essere utile adottare uno degli approcci già progettati dalla comunità e testati su molti progetti. Queste metodologie sono essenzialmente guide per la scrittura di codice CSS che adottano un approccio molto strutturato alla scrittura e all'organizzazione del CSS. In genere tendono a rendere il CSS più verboso rispetto a quanto sarebbe se ogni selettore venisse scritto e ottimizzato secondo un insieme di regole personalizzato per quel progetto.

Tuttavia, adottandone una si ottiene molta struttura. Poiché molti di questi sistemi sono ampiamente utilizzati, è più probabile che altri sviluppatori comprendano l'approccio usato e siano in grado di scrivere il proprio CSS nello stesso modo, anziché dover ricostruire da zero una metodologia personale.

#### OOCSS

La maggior parte degli approcci che si incontreranno deve qualcosa al concetto di Object Oriented CSS (OOCSS), un approccio reso popolare dal [lavoro di Nicole Sullivan](https://github.com/stubbornella/oocss/wiki). L'idea di base di OOCSS è separare il CSS in oggetti riutilizzabili, che possono essere usati ovunque sia necessario nel sito. L'esempio standard di OOCSS è il pattern descritto come [The Media Object](/it/docs/Web/CSS/How_to/Layout_cookbook/Media_objects). Si tratta di un pattern con un'immagine, un video o un altro elemento di dimensione fissa su un lato e contenuto flessibile sull'altro. È un pattern presente in molti siti web per commenti, elenchi e così via.

Se non si adotta un approccio OOCSS, si potrebbe creare un CSS personalizzato per i diversi punti in cui viene usato questo pattern, ad esempio creando due classi: una chiamata `comment` con un insieme di regole per le parti del componente e un'altra chiamata `list-item` con quasi le stesse regole della classe `comment`, a eccezione di alcune piccole differenze. Le differenze tra questi due componenti sono che l'elemento dell'elenco ha un bordo inferiore e le immagini nei commenti hanno un bordo, mentre le immagini degli elementi dell'elenco non lo hanno.

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

In OOCSS, verrebbe creato un pattern chiamato `media` che avrebbe tutto il CSS comune a entrambi i pattern: una classe di base per gli elementi che hanno generalmente la forma del media object. Verrebbe quindi aggiunta una classe aggiuntiva per gestire quelle piccole differenze, estendendo così lo stile in modi specifici.

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

Nell'HTML, il commento dovrebbe avere applicate entrambe le classi `media` e `comment`:

```html
<div class="media comment">
  <img src="" alt="" />
  <div class="content"></div>
</div>
```

All'elemento dell'elenco verrebbero applicate `media` e `list-item`:

```html
<ul>
  <li class="media list-item">
    <img src="" alt="" />
    <div class="content"></div>
  </li>
</ul>
```

Il lavoro svolto da Nicole Sullivan nella descrizione e promozione di questo approccio fa sì che anche le persone che oggi non seguono strettamente un approccio OOCSS in genere riutilizzino il CSS in questo modo: è entrato nella nostra comprensione come un buon modo di affrontare le cose in generale.

#### BEM

BEM significa Block Element Modifier. In BEM, un blocco è un'entità autonoma, come un pulsante, un menu o un logo. Un elemento è qualcosa come una voce di elenco o un titolo legato al blocco in cui si trova. Un modificatore è un indicatore su un blocco o un elemento che modifica lo stile o il comportamento. È possibile riconoscere il codice che usa BEM dall'ampio uso di trattini e trattini bassi nelle classi CSS. Ad esempio, osservare le classi applicate a questo HTML dalla pagina sulle [convenzioni di denominazione BEM](https://getbem.com/naming/):

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

Le classi aggiuntive sono simili a quelle usate nell'esempio OOCSS; tuttavia, usano le rigide convenzioni di denominazione di BEM.

BEM è ampiamente utilizzato nei grandi progetti web e molte persone scrivono il proprio CSS in questo modo. È probabile incontrare esempi, anche nei tutorial, che usano la sintassi BEM senza indicare perché il CSS sia strutturato in questo modo.

Per approfondire questo sistema, leggere [BEM 101](https://css-tricks.com/bem-101/) su CSS Tricks.

#### Altri sistemi comuni

Sono in uso molti di questi sistemi. Altri approcci popolari includono [Scalable and Modular Architecture for CSS (SMACSS)](https://smacss.com/), creato da Jonathan Snook, [ITCSS](https://itcss.io/) di Harry Roberts e [Atomizer CSS (ACSS)](https://acss-io.github.io/atomizer/), originariamente creato da Yahoo!. Se si incontra un progetto che usa uno di questi approcci, il vantaggio è che sarà possibile cercare e trovare molti articoli e guide utili per comprendere come scrivere codice nello stesso stile.

Lo svantaggio dell'uso di un sistema simile è che può sembrare eccessivamente complesso, specialmente per progetti più piccoli.

### Sistemi di build per CSS

Un altro modo per organizzare il CSS consiste nello sfruttare alcuni degli strumenti disponibili per gli sviluppatori front-end, che consentono di adottare un approccio leggermente più programmatico alla scrittura del CSS. Esistono vari strumenti, chiamati _pre-processors_ e _post-processors_. Un pre-processor elabora i file grezzi e li trasforma in un foglio di stile, mentre un post-processor prende il foglio di stile finito e fa qualcosa con esso, magari per ottimizzarlo affinché venga caricato più velocemente.

L'uso di uno qualsiasi di questi strumenti richiederà che l'ambiente di sviluppo sia in grado di eseguire gli script che effettuano il pre- e il post-processing. Molti editor di codice possono farlo, oppure è possibile installare strumenti da riga di comando utili allo scopo.

Il pre-processor più popolare è [Sass](https://sass-lang.com/). Questo non è un tutorial su Sass, quindi verranno spiegate brevemente alcune delle cose che Sass può fare, davvero utili in termini di organizzazione anche se non vengono usate altre funzionalità di Sass. Per approfondire Sass, iniziare dall'articolo [Sass basics](https://sass-lang.com/guide/), quindi consultare l'altra documentazione.

#### Definire variabili

CSS dispone ora di [proprietà personalizzate](/it/docs/Web/CSS/Guides/Cascading_variables/Using_custom_properties) native, rendendo questa funzionalità sempre meno importante. Tuttavia, uno dei motivi per usare Sass potrebbe essere la possibilità di definire tutti i colori e i font usati in un progetto come impostazioni, per poi usare tale variabile in tutto il progetto. Ciò significa che, se ci si accorge di aver usato la tonalità di blu sbagliata, sarà necessario modificarla in un solo punto.

Se venisse creata una variabile chiamata `$base-color`, come nella prima riga qui sotto, sarebbe poi possibile usarla ovunque nel foglio di stile fosse richiesto quel colore.

```scss
$base-color: #c6538c;

.alert {
  border: 1px solid $base-color;
}
```

Una volta compilato in CSS, si otterrebbe il seguente CSS nel foglio di stile finale.

```css
.alert {
  border: 1px solid #c6538c;
}
```

#### Compilare i fogli di stile dei componenti

In precedenza è stato menzionato che un modo per organizzare il CSS consiste nel suddividere i fogli di stile in fogli più piccoli. Quando si usa Sass, è possibile portare questo concetto a un livello superiore e avere molti fogli di stile molto piccoli, persino un foglio di stile separato per ogni componente. Usando le funzionalità incluse in Sass (partials), questi possono essere tutti compilati insieme in uno o in un piccolo numero di fogli di stile da collegare effettivamente al sito web.

Ad esempio, con le [partials](https://sass-lang.com/documentation/at-rules/use/#partials), si potrebbero avere diversi file di stile in una directory, come `foundation/_code.scss`, `foundation/_lists.scss`, `foundation/_footer.scss`, `foundation/_links.scss` e così via. Si potrebbe quindi usare la regola Sass `@use` per caricarli in altri fogli di stile:

```scss
// foundation/_index.scss
@use "code";
@use "lists";
@use "footer";
@use "links";
```

Se tutte le partials vengono caricate in un file indice, come indicato sopra, è possibile quindi caricare l'intera directory in un altro foglio di stile in una sola volta:

```scss
// style.scss
@use "foundation";
```

> [!NOTE]
> Un modo semplice per provare Sass è usare [CodePen](https://codepen.io/): è possibile abilitare Sass per il CSS nelle impostazioni di un Pen e CodePen eseguirà quindi il parser Sass per permettere di visualizzare la pagina web risultante con CSS regolare applicato. Talvolta i tutorial CSS usano Sass anziché CSS semplice nelle demo CodePen, quindi è utile conoscerne almeno le basi.

#### Post-processing per l'ottimizzazione

Se si è preoccupati di aumentare le dimensioni dei fogli di stile, ad esempio aggiungendo molti commenti e spazi vuoti, un passaggio di post-processing potrebbe consistere nell'ottimizzare il CSS rimuovendo tutto ciò che non è necessario nella versione di produzione. Un esempio di soluzione post-processor per farlo è [cssnano](https://cssnano.github.io/cssnano/).
