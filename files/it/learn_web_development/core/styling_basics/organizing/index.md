---
title: Organizzare il tuo CSS
slug: Learn_web_development/Core/Styling_basics/Organizing
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Quando inizi a lavorare su fogli di stile più grandi e progetti importanti scoprirai che mantenere un file CSS enorme può essere una sfida. In questo articolo esamineremo brevemente alcune delle migliori pratiche per scrivere il tuo CSS in modo che sia facilmente mantenibile, e alcune delle soluzioni comunemente utilizzate per aiutare a migliorare la mantenibilità.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Installing_software">Software di base installato</a>, conoscenza di base di
        <a href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Dealing_with_files">lavorare con i file</a>, basi di HTML (studiare
        <a href="/it/docs/Learn_web_development/Core/Structuring_content">Introduzione all'HTML</a>), e un'idea di come funziona il CSS (studiare
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Basi di styling CSS</a>.)
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare alcuni consigli e migliori pratiche per organizzare i fogli di stile e scoprire alcune delle convenzioni di denominazione e degli strumenti comunemente usati per aiutare con l'organizzazione CSS e il lavoro di squadra.
      </td>
    </tr>
  </tbody>
</table>

## Suggerimenti per mantenere il tuo CSS ordinato

Ecco alcuni suggerimenti generali per mantenere i tuoi fogli di stile organizzati e ordinati.

### Il tuo progetto ha una guida allo stile del codice?

Se stai lavorando con un team su un progetto esistente, la prima cosa da verificare è se il progetto ha una guida allo stile esistente per il CSS. La guida dello stile del team dovrebbe sempre prevalere sulle tue preferenze personali. Spesso non esiste un modo giusto o sbagliato di fare le cose, ma la coerenza è importante.

Ad esempio, dai un'occhiata alle [linee guida CSS per gli esempi di codice MDN](/it/docs/MDN/Writing_guidelines/Code_style_guide/CSS).

### Mantieni la coerenza

Se hai la possibilità di stabilire le regole per il progetto o stai lavorando da solo, l'aspetto più importante è mantenere la coerenza. La coerenza può essere applicata in modi diversi, come utilizzare gli stessi convenzioni di denominazione per le classi, scegliere un metodo per descrivere i colori o mantenere un formato coerente. (Ad esempio, utilizzerai tabulazioni o spazi per indentare il tuo codice? Se spazi, quanti?)

Avere un insieme di regole che segui sempre riduce il carico mentale necessario quando scrivi CSS, poiché alcune decisioni sono già state prese.

### Formattare CSS leggibile

Ci sono un paio di modi in cui si vede formattato il CSS. Alcuni sviluppatori mettono tutte le regole su una singola riga, come qui:

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

Al CSS non interessa quale metodo utilizzi. Noi personalmente troviamo più leggibile avere ogni coppia di proprietà e valori su una nuova riga.

### Commenta il tuo CSS

Aggiungere commenti al tuo CSS aiuterà qualsiasi sviluppatore futuro a lavorare con il tuo file CSS e ti sarà utile quando tornerai al progetto dopo una pausa.

```css
/* This is a CSS comment
It can be broken onto multiple lines. */
```

Un buon suggerimento è aggiungere un blocco di commenti tra sezioni logiche del tuo foglio di stile per aiutarvi a individuare diverse sezioni rapidamente durante la scansione, o anche per darti qualcosa da cercare per saltare direttamente in quella parte del CSS. Se usi una stringa che non apparirà nel codice, puoi saltare da una sezione all'altra cercandola — di seguito abbiamo utilizzato `||`.

```css
/* || General styles */

/* … */

/* || Typography */

/* … */

/* || Header and Main Navigation */

/* … */
```

Non è necessario commentare ogni singola cosa nel tuo CSS, poiché gran parte di esso sarà autoesplicativo. Quello che dovresti commentare sono le cose per cui hai preso una decisione particolare per un motivo specifico.

Potresti aver utilizzato una proprietà CSS in modo specifico per aggirare incompatibilità con vecchi browser, ad esempio:

```css
.box {
  background-color: red; /* fallback for older browsers that don't support gradients */
  background-image: linear-gradient(to right, #ff0000, #aa0000);
}
```

Forse hai seguito un tutorial per ottenere qualcosa e il CSS non è molto autoesplicativo o riconoscibile. In tal caso, potresti aggiungere l'URL del tutorial nei commenti. Ti ringrazierai da solo quando tornerai a questo progetto tra un anno circa e potrai vagamente ricordare che c'era un ottimo tutorial su quella cosa, ma non riesci a ricordare da dove provenga.

### Crea sezioni logiche nel tuo foglio di stile

È una buona idea avere tutti gli stili comuni per primi nel foglio di stile. Questo significa tutti gli stili che si applicheranno generalmente a meno che non faccia qualcosa di specifico con quell'elemento. In genere avrai regole impostate per:

- `body`
- `p`
- `h1`, `h2`, `h3`, `h4`, `h5`
- `ul` e `ol`
- Le proprietà del `table`
- Link

In questa sezione del foglio di stile stiamo fornendo stili predefiniti per il tipo sul sito, impostando uno stile predefinito per tabelle dati e elenchi, e così via.

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

Dopo questa sezione, potremmo definire alcune classi di utilità, ad esempio, una classe che rimuove lo stile di elenco predefinito per elenchi che verranno visualizzati come elementi flex o in qualche altro modo. Se hai alcune scelte stilistiche che sai che vorrai applicare a molti elementi diversi, puoi metterle in questa sezione.

```css
/* || UTILITIES */

.no-bullets {
  list-style: none;
  margin: 0;
  padding: 0;
}

/* … */
```

Poi possiamo aggiungere tutto ciò che viene utilizzato a livello di sito. Potrebbe trattarsi di cose come l'impaginazione base della pagina, lo stile di navigazione, e così via.

```css
/* SITEWIDE */

.main-nav {
  /* … */
}

.logo {
  /* … */
}
```

Infine, includeremo CSS per cose specifiche, suddivise per contesto, pagina o anche componente in cui vengono utilizzate.

```css
/* || STORE PAGES */

.product-listing {
  /* … */
}

.product-box {
  /* … */
}
```

Ordinando le cose in questo modo, avremo almeno un'idea in quale parte del foglio di stile cercheremo qualcosa che vogliamo cambiare.

### Evita selettori troppo specifici

Se crei selettori molto specifici, spesso ti troverai a dover duplicare parti del tuo CSS per applicare le stesse regole a un altro elemento. Ad esempio, potresti avere qualcosa come il selettore qui sotto, che applica la regola a un `<p>` con una classe di `box` all'interno di un `<article>` con una classe di `main`.

```css
article.main p.box {
  border: 1px solid #ccc;
}
```

Se successivamente volessi applicare le stesse regole a qualcosa fuori da `main`, o a qualcosa di diverso da un `<p>`, dovresti aggiungere un altro selettore a queste regole o creare un nuovo set di regole. Invece, potresti usare il selettore `.box` per applicare la tua regola a qualsiasi elemento che abbia la classe `box`:

```css
.box {
  border: 1px solid #ccc;
}
```

Ci saranno momenti in cui rendere qualcosa di più specifico ha senso; tuttavia, questo sarà generalmente un'eccezione piuttosto che la pratica usuale.

### Dividi fogli di stile grandi in più piccoli

Nel caso in cui tu abbia stili molto diversi per parti distinte del sito, potresti voler avere un foglio di stile che includa tutte le regole globali, nonché alcuni fogli di stile più piccoli che includano le regole specifiche necessarie per quelle sezioni. Puoi collegarti a più fogli di stile da una sola pagina, e le normali regole del cascade si applicano, con le regole nei fogli di stile collegati più tardi che vengono dopo le regole nei fogli di stile collegati prima.

Ad esempio, potremmo avere un negozio online come parte del sito, con molto CSS utilizzato solo per stilizzare le liste di prodotti e i moduli necessari per il negozio. Avrebbe senso avere quelle cose in un foglio di stile diverso, collegato solo alle pagine del negozio.

Questo può rendere più facile mantenere il tuo CSS organizzato, e significa anche che se più persone stanno lavorando sul CSS, avrai meno situazioni in cui due persone devono lavorare sullo stesso foglio di stile contemporaneamente, portando a conflitti nel controllo del codice sorgente.

## Altri strumenti che possono aiutare

Il CSS di per sé non offre molto in termini di organizzazione integrata; pertanto, il livello di coerenza nel tuo CSS dipenderà in gran parte da te. La comunità del web ha sviluppato vari strumenti e approcci che possono aiutarti a gestire progetti CSS più ampi. Dato che è probabile che incontri questi strumenti quando lavori con altre persone, e dato che sono spesso di aiuto in generale, abbiamo incluso una breve guida su alcuni di essi.

### Metodologie CSS

Invece di dover elaborare le tue regole per scrivere CSS, potresti trarre vantaggio dall'adozione di uno degli approcci già progettati dalla comunità e testati su molti progetti. Queste metodologie sono essenzialmente guide per la codifica CSS che prendono un approccio molto strutturato alla scrittura e organizzazione del CSS. Tipicamente tendono a rendere il CSS più verboso di quanto avresti fatto se avessi scritto e ottimizzato ogni selettore a un insieme di regole personalizzato per quel progetto.

Tuttavia, ottieni molta struttura adottandone una. Poiché molti di questi sistemi sono ampiamente utilizzati, altri sviluppatori sono più inclini a comprendere l'approccio che stai usando e a scrivere il loro CSS nello stesso modo, piuttosto che dover elaborare la tua metodologia personale da zero.

#### OOCSS

La maggior parte degli approcci che incontrerai deve qualcosa al concetto di Object Oriented CSS (OOCSS), un approccio reso popolare dal [lavoro di Nicole Sullivan](https://github.com/stubbornella/oocss/wiki). L'idea fondamentale di OOCSS è separare il tuo CSS in oggetti riutilizzabili, che possono essere utilizzati ovunque necessari sul sito. L'esempio standard di OOCSS è il modello descritto come [The Media Object](/it/docs/Web/CSS/Layout_cookbook/Media_objects). Questo è un modello con un'immagine a dimensione fissa, un video o un altro elemento su un lato, e contenuto flessibile sull'altro. È un modello che vediamo su molti siti web per commenti, elenchi, e così via.

Se non stai seguendo un approccio OOCSS potresti creare un CSS personalizzato per i diversi luoghi in cui viene utilizzato questo modello, ad esempio, creando due classi, una chiamata `comment` con un mucchio di regole per le parti componenti, e un'altra chiamata `list-item` con quasi le stesse regole della classe `comment` tranne alcune piccole differenze. Le differenze tra questi due componenti sono che l'elenco ha un bordo inferiore, e le immagini nei commenti hanno un bordo mentre le immagini dell'elenco no.

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

In OOCSS, creeresti un modello chiamato `media` che avrebbe tutto il CSS comune per entrambi i modelli — una classe base per cose che sono generalmente la forma del media object. Poi aggiungeremmo una classe aggiuntiva per gestire quelle piccole differenze, estendendo così quello stile in modi specifici.

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

Nel tuo HTML, il commento avrebbe bisogno di entrambe le classi `media` e `comment` applicate:

```html
<div class="media comment">
  <img src="" alt="" />
  <div class="content"></div>
</div>
```

L'elemento dell'elenco avrebbe `media` e `list-item` applicate:

```html
<ul>
  <li class="media list-item">
    <img src="" alt="" />
    <div class="content"></div>
  </li>
</ul>
```

Il lavoro che Nicole Sullivan ha fatto nel descrivere e promuovere questo approccio significa che anche le persone che non seguono strettamente un approccio OOCSS oggi riutilizzeranno generalmente il CSS in questo modo — è entrato nella nostra comprensione come un buon modo per affrontare le cose in generale.

#### BEM

BEM sta per Block Element Modifier. In BEM un blocco è un'entità autonoma come un pulsante, un menu o un logo. Un elemento è qualcosa come un elemento della lista o un titolo che è legato al blocco in cui si trova. Un modificatore è un flag su un blocco o elemento che cambia lo stile o comportamento. Sarai in grado di riconoscere codice che utilizza BEM grazie all'uso estensivo di trattini e underscore nelle classi CSS. Ad esempio, guarda le classi applicate a questo HTML dalla pagina su [BEM Naming conventions](https://getbem.com/naming/):

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

BEM è ampiamente utilizzato in progetti web più grandi e molte persone scrivono il loro CSS in questo modo. È probabile che incontrerai esempi, anche in tutorial, che utilizzano la sintassi BEM, senza menzionare perché il CSS è strutturato in quel modo.

Leggi di più su questo sistema [BEM 101](https://css-tricks.com/bem-101/) su CSS Tricks.

#### Altri sistemi comuni

Ci sono un gran numero di questi sistemi in uso. Altri approcci popolari includono [Scalable and Modular Architecture for CSS (SMACSS)](https://smacss.com/), creato da Jonathan Snook, [ITCSS](https://itcss.io/) di Harry Roberts, e [Atomizer CSS (ACSS)](https://acss-io.github.io/atomizer/), originariamente creato da Yahoo!. Se ti imbatti in un progetto che utilizza uno di questi approcci, allora il vantaggio è che sarai in grado di cercare e trovare molti articoli e guide che ti aiuteranno a comprendere come codificare nello stesso stile.

Lo svantaggio dell'utilizzo di un sistema simile è che possono sembrare eccessivamente complessi, specialmente per progetti più piccoli.

### Sistemi di build per CSS

Un altro modo per organizzare il CSS è sfruttare alcuni degli strumenti disponibili per gli sviluppatori front-end, che ti permettono di adottare un approccio leggermente più programmatico nella scrittura del CSS. Ci sono diversi strumenti, che chiamiamo _pre-processori_ e _post-processori_. Un pre-processore elabora i tuoi file grezzi e li trasforma in un foglio di stile, mentre un post-processore prende il foglio di stile finito e fa qualcosa — forse ottimizzarlo in modo che si carichi più velocemente.

Utilizzare uno qualsiasi di questi strumenti richiederà che il tuo ambiente di sviluppo sia in grado di eseguire gli script che gestiscono la pre- e post-elaborazione. Molti editor di codice possono farlo per te, o puoi installare strumenti da linea comandi per aiutare.

Il pre-processore più popolare è [Sass](https://sass-lang.com/). Questo non è un tutorial su Sass, quindi spiegherò brevemente alcune delle cose che Sass può fare, davvero utile in termini di organizzazione anche se non usi nessuna delle altre funzionalità di Sass. Se vuoi saperne di più su Sass, inizia con l'articolo [Sass basics](https://sass-lang.com/guide/), poi passa alla loro altra documentazione.

#### Definizione di variabili

Il CSS ora ha nativamente le [proprietà personalizzate](/it/docs/Web/CSS/CSS_cascading_variables/Using_CSS_custom_properties), rendendo questa funzione sempre meno importante. Tuttavia, uno dei motivi per cui potresti utilizzare Sass è essere in grado di definire tutti i colori e i font utilizzati in un progetto come impostazioni, quindi usare quella variabile in tutto il progetto. Questo significa che se ti accorgi di aver usato la tonalità sbagliata di blu, devi cambiarla in un solo posto.

Se abbiamo creato una variabile chiamata `$base-color`, come nella prima riga qui sotto, potremmo poi usarla nel foglio di stile ovunque richieda quel colore.

```scss
$base-color: #c6538c;

.alert {
  border: 1px solid $base-color;
}
```

Una volta compilato in CSS, finiresti con il seguente CSS nel foglio di stile finale.

```css
.alert {
  border: 1px solid #c6538c;
}
```

#### Compilare fogli di stile componenti

Ho menzionato sopra che un modo per organizzare il CSS è dividere i fogli di stile in fogli di stile più piccoli. Quando si utilizza Sass puoi portare questo a un altro livello e avere molti fogli di stile molto piccoli — fino ad arrivare al punto di avere un foglio di stile separato per ogni componente. Utilizzando la funzionalità inclusa in Sass (parziali), questi possono tutti essere compilati insieme in uno o un piccolo numero di fogli di stile da collegare effettivamente al tuo sito web.

Quindi, ad esempio, con i [parziali](https://sass-lang.com/documentation/at-rules/use/#partials), potresti avere diversi file stile all'interno di una directory, ad esempio `foundation/_code.scss`, `foundation/_lists.scss`, `foundation/_footer.scss`, `foundation/_links.scss`, ecc. Potresti quindi usare la regola `@use` di Sass per caricarli in altri fogli di stile:

```scss
// foundation/_index.scss
@use "code";
@use "lists";
@use "footer";
@use "links";
```

Se i parziali sono tutti caricati in un file indice, come implicato sopra, puoi quindi caricare quell'intera directory in un altro foglio di stile in un colpo solo:

```scss
// style.scss
@use "foundation";
```

> [!NOTE]
> Un modo semplice per provare Sass è usare [CodePen](https://codepen.io/) — puoi abilitare Sass per il tuo CSS nelle Impostazioni di un Pen, e CodePen eseguirà quindi il parser Sass per te, in modo che tu possa vedere la pagina risultante con il CSS regolare applicato. A volte troverai che i tutorial CSS hanno usato Sass piuttosto che CSS semplice nei loro demo su CodePen, quindi è utile sapere un po' su di esso.

#### Post-elaborazione per l'ottimizzazione

Se sei preoccupato per l'aumento delle dimensioni dei tuoi fogli di stile, ad esempio aggiungendo molti commenti aggiuntivi e spazi bianchi, allora un passo di post-elaborazione potrebbe essere quello di ottimizzare il CSS rimuovendo tutto ciò che è non necessario nella versione di produzione. Un esempio di soluzione post-processore per fare questo sarebbe [cssnano](https://cssnano.github.io/cssnano/).
