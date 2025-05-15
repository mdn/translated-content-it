---
title: Come costruire controlli di modulo personalizzati
slug: Learn_web_development/Extensions/Forms/How_to_build_custom_form_controls
l10n:
  sourceCommit: eaec5c4226ac64696a95314a7bce995165a4d124
---

Ci sono alcuni casi in cui i controlli modulo HTML nativi disponibili potrebbero sembrare non sufficienti. Ad esempio, se ha bisogno di [eseguire uno stile avanzato](/it/docs/Learn_web_development/Extensions/Forms/Advanced_form_styling) su alcuni controlli come l'elemento {{HTMLElement("select")}}, o se desidera fornire comportamenti personalizzati, potrebbe considerare di costruire i propri controlli.

In questo articolo, discuteremo come costruire un controllo personalizzato. A tal fine, lavoreremo con un esempio: ricostruire l'elemento {{HTMLElement("select")}}. Discuteremo anche se, quando e in che modo costruire il proprio controllo abbia senso e cosa considerare quando è necessario costruire un controllo.

> [!NOTE]
> Ci concentreremo sulla costruzione del controllo, non su come rendere il codice generico e riutilizzabile; ciò comporterebbe del codice JavaScript non banale e manipolazione del DOM in un contesto sconosciuto, e questo esula dallo scopo di questo articolo.

## Progettazione, struttura e semantica

Prima di costruire un controllo personalizzato, dovrebbe iniziare a capire esattamente cosa desidera. Questo le farà risparmiare del tempo prezioso. In particolare, è importante definire chiaramente tutti gli stati del suo controllo. Per fare ciò, è utile iniziare con un controllo esistente i cui stati e comportamenti siano ben noti, in modo da poterli imitare il più possibile.

Nel nostro esempio, ricostruiremo l'elemento {{HTMLElement("select")}}. Ecco il risultato che vogliamo ottenere:

![I tre stati di una casella di selezione](custom-select.png)

Questo screenshot mostra i tre stati principali del nostro controllo: lo stato normale (a sinistra); lo stato attivo (al centro) e lo stato aperto (a destra).

In termini di comportamento, stiamo ricreando un elemento HTML nativo. Pertanto, dovrebbe avere gli stessi comportamenti e semantiche dell'elemento HTML nativo. Richiediamo che il nostro controllo sia utilizzabile sia con un mouse che con una tastiera, e comprensibile per un lettore di schermo, proprio come qualsiasi controllo nativo. Cominciamo definendo come il controllo raggiunge ciascuno stato:

**Il controllo è nello stato normale quando:**

- la pagina viene caricata.
- il controllo era attivo e l'utente fa clic in qualsiasi punto al di fuori di esso.
- il controllo era attivo e l'utente muove il focus su un altro controllo usando la tastiera (ad esempio, il tasto <kbd>Tab</kbd>).

**Il controllo è nello stato attivo quando:**

- l'utente fa clic su di esso o lo tocca su uno schermo tattile.
- l'utente preme il tasto tab e il controllo guadagna il focus.
- il controllo era nello stato aperto e l'utente fa clic su di esso.

**Il controllo è nello stato aperto quando:**

- il controllo è in uno stato diverso dall'aperto e l'utente fa clic su di esso.

Una volta che sappiamo come cambiare stati, è importante definire come cambiare il valore del controllo:

**Il valore cambia quando:**

- l'utente fa clic su un'opzione quando il controllo è nello stato aperto.
- l'utente preme i tasti freccia su o giù quando il controllo è nello stato attivo.

**Il valore non cambia quando:**

- l'utente preme il tasto freccia su quando l'opzione selezionata è la prima.
- l'utente preme il tasto freccia giù quando l'opzione selezionata è l'ultima.

Infine, definiamo come si comporteranno le opzioni del controllo:

- Quando il controllo è aperto, l'opzione selezionata viene evidenziata.
- Quando il mouse è su un'opzione, l'opzione è evidenziata e l'opzione precedentemente evidenziata ritorna al suo stato normale.

Ai fini del nostro esempio, ci fermeremo a questo; tuttavia, se è un lettore attento, noterà che mancano alcuni comportamenti. Ad esempio, cosa pensa accadrà se l'utente preme il tasto tab mentre il controllo è nello stato aperto? La risposta è _niente_. OK, il comportamento corretto sembra ovvio, ma il fatto è, poiché non è definito nelle nostre specifiche, è molto facile trascurare questo comportamento. Questo è particolarmente vero in un ambiente di squadra dove le persone che progettano il comportamento del controllo sono diverse da quelle che lo implementano.

Un altro esempio interessante: cosa accadrà se l'utente preme i tasti freccia su o giù mentre il controllo è nello stato aperto? Questo è un po' più difficile. Se si considera che lo stato attivo e lo stato aperto sono completamente diversi, la risposta è ancora "non accadrà nulla" poiché non abbiamo definito nessuna interazione della tastiera per lo stato aperto. D'altra parte, se si considera che lo stato attivo e lo stato aperto si sovrappongono un po', il valore può cambiare, ma l'opzione non sarà sicuramente evidenziata di conseguenza, ancora una volta perché non abbiamo definito nessuna interazione della tastiera sulle opzioni quando il controllo è nello stato aperto (abbiamo solo definito cosa dovrebbe accadere quando il controllo è aperto, ma nulla dopo).

Dobbiamo pensare un po' più avanti: che dire del tasto escape? Premere il tasto <kbd>Esc</kbd> chiude un select aperto. Ricorda, se desidera fornire la stessa funzionalità dell'esistente {{htmlelement('select')}} nativo, dovrebbe comportarsi esattamente allo stesso modo del select per tutti gli utenti, dalla tastiera al mouse al touchscreen al lettore di schermo, e qualsiasi altro dispositivo di input.

Nel nostro esempio, le specifiche mancanti sono ovvie, quindi le gestiremo, ma può essere un vero problema per nuovi controlli esotici. Quando si tratta di elementi standardizzati, di cui il {{htmlelement('select')}} è uno, gli autori della specifica hanno passato un'enorme quantità di tempo a specificare tutte le interazioni per ogni scenario d'uso per ogni dispositivo di input. Creare nuovi controlli non è così facile, soprattutto se sta creando qualcosa che non è stato fatto prima, e quindi nessuno ha la minima idea di quali siano i comportamenti e le interazioni previsti. Almeno il select è stato fatto prima, quindi sappiamo come dovrebbe comportarsi!

Progettare nuove interazioni è generalmente un'opzione solo per grandi attori del settore che hanno abbastanza portata affinché un'interazione da loro creata possa diventare uno standard. Ad esempio, Apple ha introdotto il click wheel con l'iPod nel 2001. Avevano la quota di mercato per introdurre con successo un modo completamente nuovo di interagire con un dispositivo, qualcosa che la maggior parte delle compagnie di dispositivi non può fare.

È meglio non inventare nuove interazioni utente. Per qualsiasi interazione che aggiunge, è fondamentale passare del tempo nella fase di progettazione; se definisce un comportamento in modo errato, o dimentica di definirne uno, sarà molto difficile ridefinirlo una volta che gli utenti si saranno abituati. Se ha dubbi, chieda il parere degli altri, e se ha il budget per farlo, non esiti a [eseguire test utente](https://en.wikipedia.org/wiki/Usability_testing). Questo processo si chiama Design UX. Se desidera saperne di più su questo argomento, dovrebbe consultare le seguenti risorse utili:

- [UXMatters.com](https://www.uxmatters.com/)
- [La sezione UX Design di SmashingMagazine](https://www.smashingmagazine.com/)

> [!NOTE]
> Inoltre, nella maggior parte dei sistemi c'è un modo per aprire l'elemento {{HTMLElement("select")}} con la tastiera per guardare tutte le scelte disponibili (questo è lo stesso che cliccare l'elemento {{HTMLElement("select")}} con un mouse). Questo è ottenuto con <kbd>Alt</kbd> + <kbd>Giù</kbd> su Windows. Non abbiamo implementato questo nel nostro esempio, ma sarebbe facile farlo, poiché il meccanismo è già stato implementato per l'evento `click`.

## Definire la struttura HTML e (alcune) semantiche

Ora che la funzionalità di base del controllo è stata decisa, è il momento di iniziare a costruirlo. Il primo passo è definire la sua struttura HTML e dargli qualche semantica di base. Ecco cosa ci serve per ricostruire un elemento {{HTMLElement("select")}}:

```html
<!-- This is our main container for our control.
     The tabindex attribute is what allows the user to focus on the control.
     We'll see later that it's better to set it through JavaScript. -->
<div class="select" tabindex="0">
  <!-- This container will be used to display the current value of the control -->
  <span class="value">Cherry</span>

  <!-- This container will contain all the options available for our control.
       Because it's a list, it makes sense to use the ul element. -->
  <ul class="optList">
    <!-- Each option only contains the value to be displayed, we'll see later
         how to handle the real value that will be sent with the form data -->
    <li class="option">Cherry</li>
    <li class="option">Lemon</li>
    <li class="option">Banana</li>
    <li class="option">Strawberry</li>
    <li class="option">Apple</li>
  </ul>
</div>
```

Noti l'uso dei nomi delle classi; questi identificano ciascuna parte rilevante indipendentemente dagli elementi HTML sottostanti effettivamente usati. Questo è importante per assicurarsi di non legare il nostro CSS e JavaScript a una struttura HTML forte, in modo da poter apportare modifiche all'implementazione in seguito senza rompere il codice che usa il controllo. Ad esempio, e se desidera implementare l'equivalente dell'elemento {{HTMLElement("optgroup")}} in seguito?

I nomi delle classi, tuttavia, non forniscono alcun valore semantico. In questo stato attuale, l'utente del lettore di schermo "vede" solo un elenco non ordinato. Aggiungeremo in un secondo momento delle semantiche ARIA.

## Creare l'aspetto e la sensazione utilizzando CSS

Ora che abbiamo una struttura, possiamo iniziare a progettare il nostro controllo. L'intero punto di costruire questo controllo personalizzato è essere in grado di stilizzarlo esattamente come vogliamo. A tal fine, divideremo il nostro lavoro CSS in due parti: la prima parte saranno le regole CSS assolutamente necessarie per far comportare il nostro controllo come un elemento {{HTMLElement("select")}}, e la seconda parte consisterà negli stili fantasiosi usati per farlo apparire come vogliamo.

### Stili richiesti

Gli stili richiesti sono quelli necessari per gestire i tre stati del nostro controllo.

```css
.select {
  /* This will create a positioning context for the list of options;
     adding this to `.select:focus-within` will be a better option when fully supported
  */
  position: relative;

  /* This will make our control become part of the text flow and sizable at the same time */
  display: inline-block;
}
```

Abbiamo bisogno di una classe extra `active` per definire l'aspetto e la sensazione del nostro controllo quando è nello stato attivo. Poiché il nostro controllo è focalizzabile, raddoppiamo questo stile personalizzato con la pseudo-classe {{cssxref(":focus")}} per assicurarci che si comporteranno allo stesso modo.

```css
.select.active,
.select:focus {
  outline-color: transparent;

  /* This box-shadow property is not exactly required, however it's imperative to ensure
     active state is visible, especially to keyboard users, that we use it as a default value. */
  box-shadow: 0 0 3px 1px #227755;
}
```

Ora, gestiamo l'elenco delle opzioni:

```css
/* The .select selector here helps to make sure we only select
   element inside our control. */
.select .optList {
  /* This will make sure our list of options will be displayed below the value
     and out of the HTML flow */
  position: absolute;
  top: 100%;
  left: 0;
}
```

Abbiamo bisogno di una classe extra per gestire quando l'elenco delle opzioni è nascosto. Questo è necessario per gestire le differenze tra lo stato attivo e lo stato aperto che non corrispondono esattamente.

```css
.select .optList.hidden {
  /* This is a simple way to hide the list in an accessible way;
     we will talk more about accessibility in the end */
  max-height: 0;
  visibility: hidden;
}
```

> [!NOTE]
> Avremmo anche potuto utilizzare `transform: scale(1, 0)` per dare all'elenco delle opzioni nessuna altezza e larghezza completa.

### Abbellimento

Ora che abbiamo in atto la funzionalità di base, il divertimento può iniziare. Quanto segue è solo un esempio di ciò che è possibile, e corrisponderà allo screenshot all'inizio di questo articolo. Tuttavia, dovrebbe sentirsi libero di sperimentare e vedere cosa può inventare.

```css
.select {
  /* The computations are made assuming 1em equals 16px which is the default value in most browsers.
     If you are lost with px to em conversion, try https://nekocalc.com/px-to-em-converter */
  font-size: 0.625em; /* this (10px) is the new font size context for em value in this context */
  font-family: Verdana, Arial, sans-serif;

  box-sizing: border-box;

  /* We need extra room for the down arrow we will add */
  padding: 0.1em 2.5em 0.2em 0.5em;
  width: 10em; /* 100px */

  border: 0.2em solid #000;
  border-radius: 0.4em;
  box-shadow: 0 0.1em 0.2em rgb(0 0 0 / 45%);

  /* The first declaration is for browsers that do not support linear gradients. */
  background: #f0f0f0;
  background: linear-gradient(0deg, #e3e3e3, #fcfcfc 50%, #f0f0f0);
}

.select .value {
  /* Because the value can be wider than our control, we have to make sure it will not
     change the control's width. If the content overflows, we display an ellipsis */
  display: inline-block;
  width: 100%;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
  vertical-align: top;
}
```

Non abbiamo bisogno di un elemento extra per progettare la freccia in basso; invece, stiamo usando il pseudo-elemento {{cssxref("::after")}}. Potrebbe anche essere implementato usando una semplice immagine di sfondo sulla classe `select`.

```css
.select::after {
  content: "▼"; /* We use the unicode character U+25BC; make sure to set a charset meta tag */
  position: absolute;
  z-index: 1; /* This will be important to keep the arrow from overlapping the list of options */
  top: 0;
  right: 0;

  box-sizing: border-box;

  height: 100%;
  width: 2em;
  padding-top: 0.1em;

  border-left: 0.2em solid #000;
  border-radius: 0 0.1em 0.1em 0;

  background-color: #000;
  color: #fff;
  text-align: center;
}
```

Successivamente, stiliamo l'elenco delle opzioni:

```css
.select .optList {
  z-index: 2; /* We explicitly said the list of options will always be on top of the down arrow */

  /* this will reset the default style of the ul element */
  list-style: none;
  margin: 0;
  padding: 0;

  box-sizing: border-box;

  /* If the values are smaller than the control, the list of options
     will be as wide as the control itself */
  min-width: 100%;

  /* In case the list is too long, its content will overflow vertically
     (which will add a vertical scrollbar automatically) but never horizontally
     (because we haven't set a width, the list will adjust its width automatically.
     If it can't, the content will be truncated) */
  max-height: 10em; /* 100px */
  overflow-y: auto;
  overflow-x: hidden;

  border: 0.2em solid #000;
  border-top-width: 0.1em;
  border-radius: 0 0 0.4em 0.4em;

  box-shadow: 0 0.2em 0.4em rgb(0 0 0 / 40%);
  background: #f0f0f0;
}
```

Per le opzioni, dobbiamo aggiungere una classe `highlight` per poter identificare il valore che l'utente sceglierà (o ha scelto).

```css
.select .option {
  padding: 0.2em 0.3em; /* 2px 3px */
}

.select .highlight {
  background: #000;
  color: #ffffff;
}
```

Ecco il risultato con i nostri tre stati ([controlla il codice sorgente qui](/it/docs/Learn_web_development/Extensions/Forms/How_to_build_custom_form_controls/Example_1)):

#### Stato di base

```html hidden
<div class="select">
  <span class="value">Cherry</span>
  <ul class="optList hidden">
    <li class="option">Cherry</li>
    <li class="option">Lemon</li>
    <li class="option">Banana</li>
    <li class="option">Strawberry</li>
    <li class="option">Apple</li>
  </ul>
</div>
```

```css hidden
.select {
  position: relative;
  display: inline-block;
}

.select.active,
.select:focus {
  box-shadow: 0 0 3px 1px #227755;
  outline-color: transparent;
}

.select .optList {
  position: absolute;
  top: 100%;
  left: 0;
}

.select .optList.hidden {
  max-height: 0;
  visibility: hidden;
}

.select {
  font-size: 0.625em; /* 10px */
  font-family: Verdana, Arial, sans-serif;

  box-sizing: border-box;

  padding: 0.1em 2.5em 0.2em 0.5em; /* 1px 25px 2px 5px */
  width: 10em; /* 100px */

  border: 0.2em solid #000; /* 2px */
  border-radius: 0.4em; /* 4px */

  box-shadow: 0 0.1em 0.2em rgb(0 0 0 / 45%); /* 0 1px 2px */

  background: #f0f0f0;
  background: linear-gradient(0deg, #e3e3e3, #fcfcfc 50%, #f0f0f0);
}

.select .value {
  display: inline-block;
  width: 100%;
  overflow: hidden;

  white-space: nowrap;
  text-overflow: ellipsis;
  vertical-align: top;
}

.select::after {
  content: "▼";
  position: absolute;
  z-index: 1;
  height: 100%;
  width: 2em; /* 20px */
  top: 0;
  right: 0;

  padding-top: 0.1em;

  box-sizing: border-box;

  text-align: center;

  border-left: 0.2em solid #000;
  border-radius: 0 0.1em 0.1em 0;

  background-color: #000;
  color: #fff;
}

.select .optList {
  z-index: 2;

  list-style: none;
  margin: 0;
  padding: 0;

  background: #f0f0f0;
  border: 0.2em solid #000;
  border-top-width: 0.1em;
  border-radius: 0 0 0.4em 0.4em;

  box-shadow: 0 0.2em 0.4em rgb(0 0 0 / 40%);

  box-sizing: border-box;

  min-width: 100%;
  max-height: 10em; /* 100px */
  overflow-y: auto;
  overflow-x: hidden;
}

.select .option {
  padding: 0.2em 0.3em;
}

.select .highlight {
  background: #000;
  color: #ffffff;
}
```

{{EmbedLiveSample("Basic_state",120,130)}}

#### Stato attivo

```html hidden
<div class="select active">
  <span class="value">Cherry</span>
  <ul class="optList hidden">
    <li class="option">Cherry</li>
    <li class="option">Lemon</li>
    <li class="option">Banana</li>
    <li class="option">Strawberry</li>
    <li class="option">Apple</li>
  </ul>
</div>
```

```css hidden
.select {
  position: relative;
  display: inline-block;
}

.select.active,
.select:focus {
  box-shadow: 0 0 3px 1px #227755;
  outline-color: transparent;
}

.select .optList {
  position: absolute;
  top: 100%;
  left: 0;
}

.select .optList.hidden {
  max-height: 0;
  visibility: hidden;
}

.select {
  font-size: 0.625em; /* 10px */
  font-family: Verdana, Arial, sans-serif;

  box-sizing: border-box;

  padding: 0.1em 2.5em 0.2em 0.5em; /* 1px 25px 2px 5px */
  width: 10em; /* 100px */

  border: 0.2em solid #000; /* 2px */
  border-radius: 0.4em; /* 4px */

  box-shadow: 0 0.1em 0.2em rgb(0 0 0 / 45%); /* 0 1px 2px */

  background: #f0f0f0;
  background: linear-gradient(0deg, #e3e3e3, #fcfcfc 50%, #f0f0f0);
}

.select .value {
  display: inline-block;
  width: 100%;
  overflow: hidden;

  white-space: nowrap;
  text-overflow: ellipsis;
  vertical-align: top;
}

.select::after {
  content: "▼";
  position: absolute;
  z-index: 1;
  height: 100%;
  width: 2em; /* 20px */
  top: 0;
  right: 0;

  padding-top: 0.1em;

  box-sizing: border-box;

  text-align: center;

  border-left: 0.2em solid #000;
  border-radius: 0 0.1em 0.1em 0;

  background-color: #000;
  color: #fff;
}

.select .optList {
  z-index: 2;

  list-style: none;
  margin: 0;
  padding: 0;

  background: #f0f0f0;
  border: 0.2em solid #000;
  border-top-width: 0.1em;
  border-radius: 0 0 0.4em 0.4em;

  box-shadow: 0 0.2em 0.4em rgb(0 0 0 / 40%);

  box-sizing: border-box;

  min-width: 100%;
  max-height: 10em; /* 100px */
  overflow-y: auto;
  overflow-x: hidden;
}

.select .option {
  padding: 0.2em 0.3em;
}

.select .highlight {
  background: #000;
  color: #ffffff;
}
```

{{EmbedLiveSample("Active_state",120,130)}}

#### Stato aperto

```html hidden
<div class="select active">
  <span class="value">Cherry</span>
  <ul class="optList">
    <li class="option highlight">Cherry</li>
    <li class="option">Lemon</li>
    <li class="option">Banana</li>
    <li class="option">Strawberry</li>
    <li class="option">Apple</li>
  </ul>
</div>
```

```css hidden
.select {
  position: relative;
  display: inline-block;
}

.select.active,
.select:focus {
  box-shadow: 0 0 3px 1px #227755;
  outline-color: transparent;
}

.select .optList {
  position: absolute;
  top: 100%;
  left: 0;
}

.select .optList.hidden {
  max-height: 0;
  visibility: hidden;
}

.select {
  font-size: 0.625em; /* 10px */
  font-family: Verdana, Arial, sans-serif;

  box-sizing: border-box;

  padding: 0.1em 2.5em 0.2em 0.5em; /* 1px 25px 2px 5px */
  width: 10em; /* 100px */

  border: 0.2em solid #000; /* 2px */
  border-radius: 0.4em; /* 4px */

  box-shadow: 0 0.1em 0.2em rgb(0 0 0 / 45%); /* 0 1px 2px */

  background: #f0f0f0;
  background: linear-gradient(0deg, #e3e3e3, #fcfcfc 50%, #f0f0f0);
}

.select .value {
  display: inline-block;
  width: 100%;
  overflow: hidden;

  white-space: nowrap;
  text-overflow: ellipsis;
  vertical-align: top;
}

.select::after {
  content: "▼";
  position: absolute;
  z-index: 1;
  height: 100%;
  width: 2em; /* 20px */
  top: 0;
  right: 0;

  padding-top: 0.1em;

  box-sizing: border-box;

  text-align: center;

  border-left: 0.2em solid #000;
  border-radius: 0 0.1em 0.1em 0;

  background-color: #000;
  color: #fff;
}

.select .optList {
  z-index: 2;

  list-style: none;
  margin: 0;
  padding: 0;

  background: #f0f0f0;
  border: 0.2em solid #000;
  border-top-width: 0.1em;
  border-radius: 0 0 0.4em 0.4em;

  box-shadow: 0 0.2em 0.4em rgb(0 0 0 / 40%);

  box-sizing: border-box;

  min-width: 100%;
  max-height: 10em; /* 100px */
  overflow-y: auto;
  overflow-x: hidden;
}

.select .option {
  padding: 0.2em 0.3em;
}

.select .highlight {
  background: #000;
  color: #fff;
}
```

{{EmbedLiveSample("Open_state",120,130)}}

## Dare vita al suo controllo con JavaScript

Ora che il nostro design e la nostra struttura sono pronti, possiamo scrivere il codice JavaScript per far funzionare effettivamente il controllo.

> [!WARNING]
> Quanto segue è codice educativo, non codice di produzione, e non dovrebbe essere utilizzato così com'è. Non è a prova di futuro né funzionerà su browser legacy. Ha anche parti ridondanti che dovrebbero essere ottimizzate nel codice di produzione.

### Perché non funziona?

Prima di iniziare, è importante ricordare che **JavaScript nel browser è una tecnologia inaffidabile**. I controlli personalizzati si basano su JavaScript per collegare tutto. Tuttavia, ci sono casi in cui JavaScript non è in grado di essere eseguito nel browser:

- L'utente ha disabilitato JavaScript: Questo è inusuale; pochissime persone disabilitano JavaScript al giorno d'oggi.
- Lo script non è stato caricato: Questo è uno dei casi più comuni, soprattutto nel mondo mobile dove la rete non è molto affidabile.
- Lo script è difettoso: Dovrebbe sempre considerare questa possibilità.
- Lo script confligge con uno script di terze parti: Questo può accadere con script di tracciamento o qualsiasi bookmarklet utilizzato dall'utente.
- Lo script confligge con, o è influenzato da, un'estensione del browser (come l'estensione [NoScript](https://addons.mozilla.org/fr/firefox/addon/noscript/) di Firefox o l'estensione [ScriptBlock](https://chromewebstore.google.com/detail/scriptblock/hcdjknjpbnhdoabbngpmfekaecnpajba) di Chrome).
- L'utente sta utilizzando un browser legacy, e una delle funzionalità di cui ha bisogno non è supportata: Questo accadrà frequentemente quando utilizza API all'avanguardia.
- L'utente sta interagendo con il contenuto prima che il JavaScript sia stato completamente scaricato, analizzato ed eseguito.

A causa di questi rischi, è davvero importante considerare seriamente cosa accadrà se il suo JavaScript non funziona. Discuteremo le opzioni da considerare e copriremo le basi nel nostro esempio (una discussione completa su come risolvere questo problema per tutti gli scenari richiederebbe un libro). Ricorda solo che è vitale rendere il suo script generico e riutilizzabile.

Nel nostro esempio, se il nostro codice JavaScript non viene eseguito, torneremo a visualizzare un normale elemento {{HTMLElement("select")}}. Includiamo il nostro controllo e l'elemento {{HTMLElement("select")}}; quale viene visualizzato dipende dalla classe dell'elemento body, con la classe dell'elemento body che viene aggiornata dallo script che fa funzionare il controllo, quando viene caricato con successo.

Per ottenere questo, abbiamo bisogno di due cose:

Primo, dobbiamo aggiungere un regolare elemento {{HTMLElement("select")}} prima di ogni istanza del nostro controllo personalizzato. C'è un vantaggio ad avere questo "extra" select anche se il nostro JavaScript funziona come sperato: utilizzeremo questo select per inviare dati dal nostro controllo personalizzato insieme al resto dei dati del nostro modulo. Ne discuteremo più in dettaglio successivamente.

```html
<body class="no-widget">
  <form>
    <select name="myFruit">
      <option>Cherry</option>
      <option>Lemon</option>
      <option>Banana</option>
      <option>Strawberry</option>
      <option>Apple</option>
    </select>

    <div class="select">
      <span class="value">Cherry</span>
      <ul class="optList hidden">
        <li class="option">Cherry</li>
        <li class="option">Lemon</li>
        <li class="option">Banana</li>
        <li class="option">Strawberry</li>
        <li class="option">Apple</li>
      </ul>
    </div>
  </form>
</body>
```

Secondo, abbiamo bisogno di due nuove classi per permetterci di nascondere l'elemento non necessario: nascondiamo visivamente il controllo personalizzato se il nostro script non è in esecuzione, oppure l'elemento "reale" {{HTMLElement("select")}} se è in esecuzione. Si noti che, per impostazione predefinita, il nostro codice HTML nasconde il nostro controllo personalizzato.

```css
.widget select,
.no-widget .select {
  /* This CSS selector basically says:
     - either we have set the body class to "widget" and thus we hide the actual <select> element
     - or we have not changed the body class, therefore the body class is still "no-widget",
       so the elements whose class is "select" must be hidden */
  position: absolute;
  left: -5000em;
  height: 0;
  overflow: hidden;
}
```

Questo CSS nasconde visivamente uno degli elementi, ma è ancora disponibile per i lettori di schermo.

Ora ci serve un interruttore JavaScript per determinare se lo script è in esecuzione o no. Questo interruttore è un paio di righe: se al momento del caricamento della pagina il nostro script è in esecuzione, rimuoverà la classe `no-widget` e aggiungerà la classe `widget`, scambiando così la visibilità dell'elemento {{HTMLElement("select")}} e del controllo personalizzato.

```js
window.addEventListener("load", () => {
  document.body.classList.remove("no-widget");
  document.body.classList.add("widget");
});
```

#### Senza JS

Controlla il [codice sorgente completo](/it/docs/Learn_web_development/Extensions/Forms/How_to_build_custom_form_controls/Example_2#no_js).

```html hidden
<form class="no-widget">
  <select name="myFruit">
    <option>Cherry</option>
    <option>Lemon</option>
    <option>Banana</option>
    <option>Strawberry</option>
    <option>Apple</option>
  </select>

  <div class="select">
    <span class="value">Cherry</span>
    <ul class="optList hidden">
      <li class="option">Cherry</li>
      <li class="option">Lemon</li>
      <li class="option">Banana</li>
      <li class="option">Strawberry</li>
      <li class="option">Apple</li>
    </ul>
  </div>
</form>
```

```css hidden
.widget select,
.no-widget .select {
  position: absolute;
  left: -5000em;
  height: 0;
  overflow: hidden;
}
```

{{EmbedLiveSample("Without_JS",120,130)}}

#### Con JS

Controlla il [codice sorgente completo](/it/docs/Learn_web_development/Extensions/Forms/How_to_build_custom_form_controls/Example_2#js).

```html hidden
<form class="no-widget">
  <select name="myFruit">
    <option>Cherry</option>
    <option>Lemon</option>
    <option>Banana</option>
    <option>Strawberry</option>
    <option>Apple</option>
  </select>

  <div class="select">
    <span class="value">Cherry</span>
    <ul class="optList hidden">
      <li class="option">Cherry</li>
      <li class="option">Lemon</li>
      <li class="option">Banana</li>
      <li class="option">Strawberry</li>
      <li class="option">Apple</li>
    </ul>
  </div>
</form>
```

```css hidden
.widget select,
.no-widget .select {
  position: absolute;
  left: -5000em;
  height: 0;
  overflow: hidden;
}

.select {
  position: relative;
  display: inline-block;
}

.select.active,
.select:focus {
  box-shadow: 0 0 3px 1px #227755;
  outline-color: transparent;
}

.select .optList {
  position: absolute;
  top: 100%;
  left: 0;
}

.select .optList.hidden {
  max-height: 0;
  visibility: hidden;
}

.select {
  font-size: 0.625em; /* 10px */
  font-family: Verdana, Arial, sans-serif;

  box-sizing: border-box;

  padding: 0.1em 2.5em 0.2em 0.5em; /* 1px 25px 2px 5px */
  width: 10em; /* 100px */

  border: 0.2em solid #000; /* 2px */
  border-radius: 0.4em; /* 4px */

  box-shadow: 0 0.1em 0.2em rgb(0 0 0 / 45%); /* 0 1px 2px */

  background: #f0f0f0;
  background: linear-gradient(0deg, #e3e3e3, #fcfcfc 50%, #f0f0f0);
}

.select .value {
  display: inline-block;
  width: 100%;
  overflow: hidden;

  white-space: nowrap;
  text-overflow: ellipsis;
  vertical-align: top;
}

.select::after {
  content: "▼";
  position: absolute;
  z-index: 1;
  height: 100%;
  width: 2em; /* 20px */
  top: 0;
  right: 0;

  padding-top: 0.1em;

  box-sizing: border-box;

  text-align: center;

  border-left: 0.2em solid #000;
  border-radius: 0 0.1em 0.1em 0;

  background-color: #000;
  color: #fff;
}

.select .optList {
  z-index: 2;

  list-style: none;
  margin: 0;
  padding: 0;

  background: #f0f0f0;
  border: 0.2em solid #000;
  border-top-width: 0.1em;
  border-radius: 0 0 0.4em 0.4em;

  box-shadow: 0 0.2em 0.4em rgb(0 0 0 / 40%);

  box-sizing: border-box;

  min-width: 100%;
  max-height: 10em; /* 100px */
  overflow-y: auto;
  overflow-x: hidden;
}

.select .option {
  padding: 0.2em 0.3em;
}

.select .highlight {
  background: #000;
  color: #ffffff;
}
```

```js hidden
window.addEventListener("load", () => {
  const form = document.querySelector("form");

  form.classList.remove("no-widget");
  form.classList.add("widget");
});
```

{{EmbedLiveSample("With_JS",120,130)}}

> [!NOTE]
> Se vuole davvero rendere il suo codice generico e riutilizzabile, invece di fare uno switch di classe è molto meglio aggiungere solo la classe widget per nascondere gli elementi {{HTMLElement("select")}}, e aggiungere dinamicamente l'albero DOM che rappresenta il controllo personalizzato dopo ogni elemento {{HTMLElement("select")}} nella pagina.

### Come rendere il lavoro più facile

Nel codice che stiamo per costruire, useremo le API JavaScript standard e DOM per fare tutto il lavoro di cui abbiamo bisogno. Le funzionalità che pianifichiamo di utilizzare sono le seguenti:

1. [`classList`](/it/docs/Web/API/Element/classList)
2. [`addEventListener()`](/it/docs/Web/API/EventTarget/addEventListener)
3. [`NodeList.forEach()`](/it/docs/Web/API/NodeList/forEach)
4. [`querySelector()`](/it/docs/Web/API/Element/querySelector) e [`querySelectorAll()`](/it/docs/Web/API/Element/querySelectorAll)

### Costruzione dei callback degli eventi

Le basi sono fatte. Possiamo ora iniziare a definire tutte le funzioni che verranno utilizzate ogni volta che l'utente interagisce con il nostro controllo.

```js
// This function will be used each time we want to deactivate a custom control
// It takes one parameter
// select : the DOM node with the `select` class to deactivate
function deactivateSelect(select) {
  // If the control is not active there is nothing to do
  if (!select.classList.contains("active")) return;

  // We need to get the list of options for the custom control
  const optList = select.querySelector(".optList");

  // We close the list of option
  optList.classList.add("hidden");

  // and we deactivate the custom control itself
  select.classList.remove("active");
}

// This function will be used each time the user wants to activate the control
// (which, in turn, will deactivate other select controls)
// It takes two parameters:
// select : the DOM node with the `select` class to activate
// selectList : the list of all the DOM nodes with the `select` class
function activeSelect(select, selectList) {
  // If the control is already active there is nothing to do
  if (select.classList.contains("active")) return;

  // We have to turn off the active state on all custom controls
  // Because the deactivateSelect function fulfills all the requirements of the
  // forEach callback function, we use it directly without using an intermediate
  // anonymous function.
  selectList.forEach(deactivateSelect);

  // And we turn on the active state for this specific control
  select.classList.add("active");
}

// This function will be used each time the user wants to open/closed the list of options
// It takes one parameter:
// select : the DOM node with the list to toggle
function toggleOptList(select) {
  // The list is kept from the control
  const optList = select.querySelector(".optList");

  // We change the class of the list to show/hide it
  optList.classList.toggle("hidden");
}

// This function will be used each time we need to highlight an option
// It takes two parameters:
// select : the DOM node with the `select` class containing the option to highlight
// option : the DOM node with the `option` class to highlight
function highlightOption(select, option) {
  // We get the list of all option available for our custom select element
  const optionList = select.querySelectorAll(".option");

  // We remove the highlight from all options
  optionList.forEach((other) => {
    other.classList.remove("highlight");
  });

  // We highlight the right option
  option.classList.add("highlight");
}
```

Ha bisogno di queste per gestire i vari stati del controllo personalizzato.

Successivamente, leghiamo queste funzioni agli eventi appropriati:

```js
// We handle the event binding when the document is loaded.
window.addEventListener("load", () => {
  const selectList = document.querySelectorAll(".select");

  // Each custom control needs to be initialized
  selectList.forEach((select) => {
    // as well as all its `option` elements
    const optionList = select.querySelectorAll(".option");

    // Each time a user hovers their mouse over an option, we highlight the given option
    optionList.forEach((option) => {
      option.addEventListener("mouseover", () => {
        // Note: the `select` and `option` variable are closures
        // available in the scope of our function call.
        highlightOption(select, option);
      });
    });

    // Each times the user clicks on or taps a custom select element
    select.addEventListener("click", (event) => {
      // Note: the `select` variable is a closure
      // available in the scope of our function call.

      // We toggle the visibility of the list of options
      toggleOptList(select);
    });

    // In case the control gains focus
    // The control gains the focus each time the user clicks on it or each time
    // they use the tabulation key to access the control
    select.addEventListener("focus", (event) => {
      // Note: the `select` and `selectList` variable are closures
      // available in the scope of our function call.

      // We activate the control
      activeSelect(select, selectList);
    });

    // In case the control loses focus
    select.addEventListener("blur", (event) => {
      // Note: the `select` variable is a closure
      // available in the scope of our function call.

      // We deactivate the control
      deactivateSelect(select);
    });

    // Loose focus if the user hits `esc`
    select.addEventListener("keyup", (event) => {
      // deactivate on keyup of `esc`
      if (event.key === "Escape") {
        deactivateSelect(select);
      }
    });
  });
});
```

In questo momento, il nostro controllo cambierà stato secondo il nostro design, ma il suo valore non viene ancora aggiornato. Ce ne occuperemo in seguito.

#### Esempio dal vivo

Controlla il [codice sorgente completo](/it/docs/Learn_web_development/Extensions/Forms/How_to_build_custom_form_controls/Example_3).

```html hidden
<form class="no-widget">
  <select name="myFruit" tabindex="-1">
    <option>Cherry</option>
    <option>Lemon</option>
    <option>Banana</option>
    <option>Strawberry</option>
    <option>Apple</option>
  </select>

  <div class="select" tabindex="0">
    <span class="value">Cherry</span>
    <ul class="optList hidden">
      <li class="option">Cherry</li>
      <li class="option">Lemon</li>
      <li class="option">Banana</li>
      <li class="option">Strawberry</li>
      <li class="option">Apple</li>
    </ul>
  </div>
</form>
```

```css hidden
.widget select,
.no-widget .select {
  position: absolute;
  left: -5000em;
  height: 0;
  overflow: hidden;
}

.select {
  position: relative;
  display: inline-block;
}

.select.active,
.select:focus {
  box-shadow: 0 0 3px 1px #227755;
  outline-color: transparent;
}

.select .optList {
  position: absolute;
  top: 100%;
  left: 0;
}

.select .optList.hidden {
  max-height: 0;
  visibility: hidden;
}

.select {
  font-size: 0.625em; /* 10px */
  font-family: Verdana, Arial, sans-serif;

  box-sizing: border-box;

  padding: 0.1em 2.5em 0.2em 0.5em; /* 1px 25px 2px 5px */
  width: 10em; /* 100px */

  border: 0.2em solid #000; /* 2px */
  border-radius: 0.4em; /* 4px */

  box-shadow: 0 0.1em 0.2em rgb(0 0 0 / 45%); /* 0 1px 2px */

  background: #f0f0f0;
  background: linear-gradient(0deg, #e3e3e3, #fcfcfc 50%, #f0f0f0);
}

.select .value {
  display: inline-block;
  width: 100%;
  overflow: hidden;

  white-space: nowrap;
  text-overflow: ellipsis;
  vertical-align: top;
}

.select::after {
  content: "▼";
  position: absolute;
  z-index: 1;
  height: 100%;
  width: 2em; /* 20px */
  top: 0;
  right: 0;

  padding-top: 0.1em;

  box-sizing: border-box;

  text-align: center;

  border-left: 0.2em solid #000;
  border-radius: 0 0.1em 0.1em 0;

  background-color: #000;
  color: #fff;
}

.select .optList {
  z-index: 2;

  list-style: none;
  margin: 0;
  padding: 0;

  background: #f0f0f0;
  border: 0.2em solid #000;
  border-top-width: 0.1em;
  border-radius: 0 0 0.4em 0.4em;

  box-shadow: 0 0.2em 0.4em rgb(0 0 0 / 40%);

  box-sizing: border-box;

  min-width: 100%;
  max-height: 10em; /* 100px */
  overflow-y: auto;
  overflow-x: hidden;
}

.select .option {
  padding: 0.2em 0.3em;
}

.select .highlight {
  background: #000;
  color: #ffffff;
}
```

```js hidden
function deactivateSelect(select) {
  if (!select.classList.contains("active")) return;

  const optList = select.querySelector(".optList");

  optList.classList.add("hidden");
  select.classList.remove("active");
}

function activeSelect(select, selectList) {
  if (select.classList.contains("active")) return;

  selectList.forEach(deactivateSelect);
  select.classList.add("active");
}

function toggleOptList(select, show) {
  const optList = select.querySelector(".optList");

  optList.classList.toggle("hidden");
}

function highlightOption(select, option) {
  const optionList = select.querySelectorAll(".option");

  optionList.forEach((other) => {
    other.classList.remove("highlight");
  });

  option.classList.add("highlight");
}

window.addEventListener("load", () => {
  const form = document.querySelector("form");

  form.classList.remove("no-widget");
  form.classList.add("widget");
});

window.addEventListener("load", () => {
  const selectList = document.querySelectorAll(".select");

  selectList.forEach((select) => {
    const optionList = select.querySelectorAll(".option");

    optionList.forEach((option) => {
      option.addEventListener("mouseover", () => {
        highlightOption(select, option);
      });
    });

    select.addEventListener(
      "click",
      (event) => {
        toggleOptList(select);
      },
      false,
    );

    select.addEventListener("focus", (event) => {
      activeSelect(select, selectList);
    });

    select.addEventListener("blur", (event) => {
      deactivateSelect(select);
    });

    select.addEventListener("keyup", (event) => {
      if (event.key === "Escape") {
        deactivateSelect(select);
      }
    });
  });
});
```

{{EmbedLiveSample("Live_example",120,130)}}

### Gestione del valore del controllo

Ora che il nostro controllo sta funzionando, dobbiamo aggiungere codice per aggiornare il suo valore secondo l'input dell'utente e renderlo possibile da inviare insieme ai dati del modulo.

Il modo più semplice per farlo è usare un controllo nativo sotto il cofano. Tale controllo terrà traccia del valore con tutti i controlli integrati forniti dal browser, e il valore verrà inviato come al solito quando un modulo viene inviato. Non ha senso reinventare la ruota quando possiamo far fare tutto questo a noi.

Come visto in precedenza, utilizziamo già un controllo select nativo come fallback per motivi di accessibilità; possiamo sincronizzare il suo valore con quello del nostro controllo personalizzato:

```js
// This function updates the displayed value and synchronizes it with the native control.
// It takes two parameters:
// select : the DOM node with the class `select` containing the value to update
// index  : the index of the value to be selected
function updateValue(select, index) {
  // We need to get the native control for the given custom control
  // In our example, that native control is a sibling of the custom control
  const nativeWidget = select.previousElementSibling;

  // We also need to get the value placeholder of our custom control
  const value = select.querySelector(".value");

  // And we need the whole list of options
  const optionList = select.querySelectorAll(".option");

  // We set the selected index to the index of our choice
  nativeWidget.selectedIndex = index;

  // We update the value placeholder accordingly
  value.textContent = optionList[index].textContent;

  // And we highlight the corresponding option of our custom control
  highlightOption(select, optionList[index]);
}

// This function returns the current selected index in the native control
// It takes one parameter:
// select : the DOM node with the class `select` related to the native control
function getIndex(select) {
  // We need to access the native control for the given custom control
  // In our example, that native control is a sibling of the custom control
  const nativeWidget = select.previousElementSibling;

  return nativeWidget.selectedIndex;
}
```

Con queste due funzioni, possiamo legare i controlli nativi a quelli personalizzati:

```js
// We handle event binding when the document is loaded.
window.addEventListener("load", () => {
  const selectList = document.querySelectorAll(".select");

  // Each custom control needs to be initialized
  selectList.forEach((select) => {
    const optionList = select.querySelectorAll(".option");
    const selectedIndex = getIndex(select);

    // We make our custom control focusable
    select.tabIndex = 0;

    // We make the native control no longer focusable
    select.previousElementSibling.tabIndex = -1;

    // We make sure that the default selected value is correctly displayed
    updateValue(select, selectedIndex);

    // Each time a user clicks on an option, we update the value accordingly
    optionList.forEach((option, index) => {
      option.addEventListener("click", (event) => {
        updateValue(select, index);
      });
    });

    // Each time a user uses their keyboard on a focused control, we update the value accordingly
    select.addEventListener("keyup", (event) => {
      let index = getIndex(select);
      // When the user hits the Escape key, deactivate the custom control
      if (event.key === "Escape") {
        deactivateSelect(select);
      }

      // When the user hits the down arrow, we jump to the next option
      if (event.key === "ArrowDown" && index < optionList.length - 1) {
        index++;
        // Prevent the default action of the ArrowDown key press.
        // Without this, the page would scroll down when the ArrowDown key is pressed.
        event.preventDefault();
      }

      // When the user hits the up arrow, we jump to the previous option
      if (event.key === "ArrowUp" && index > 0) {
        index--;
        // Prevent the default action of the ArrowUp key press.
        event.preventDefault();
      }
      if (event.key === "Enter" || event.key === " ") {
        // If Enter or Space is pressed, toggle the option list
        toggleOptList(select);
      }

      updateValue(select, index);
    });
  });
});
```

Nel codice sopra, vale la pena notare l'uso della proprietà [`tabIndex`](/it/docs/Web/API/HTMLElement/tabIndex). Usare questa proprietà è necessario per garantire che il controllo nativo non guadagnerà mai il focus, e per assicurarci che il nostro controllo personalizzato guadagni il focus quando l'utente utilizza la propria tastiera o mouse.

Con questo, abbiamo finito!

#### Esempio dal vivo

Controlla il [codice sorgente qui](/it/docs/Learn_web_development/Extensions/Forms/How_to_build_custom_form_controls/Example_4).

```html hidden
<form class="no-widget">
  <select name="myFruit">
    <option>Cherry</option>
    <option>Lemon</option>
    <option>Banana</option>
    <option>Strawberry</option>
    <option>Apple</option>
  </select>

  <div class="select">
    <span class="value">Cherry</span>
    <ul class="optList hidden">
      <li class="option">Cherry</li>
      <li class="option">Lemon</li>
      <li class="option">Banana</li>
      <li class="option">Strawberry</li>
      <li class="option">Apple</li>
    </ul>
  </div>
</form>
```

```css hidden
.widget select,
.no-widget .select {
  position: absolute;
  left: -5000em;
  height: 0;
  overflow: hidden;
}

.select {
  position: relative;
  display: inline-block;
}

.select.active,
.select:focus {
  box-shadow: 0 0 3px 1px #227755;
  outline-color: transparent;
}

.select .optList {
  position: absolute;
  top: 100%;
  left: 0;
}

.select .optList.hidden {
  max-height: 0;
  visibility: hidden;
}

.select {
  font-size: 0.625em; /* 10px */
  font-family: Verdana, Arial, sans-serif;

  box-sizing: border-box;

  padding: 0.1em 2.5em 0.2em 0.5em; /* 1px 25px 2px 5px */
  width: 10em; /* 100px */

  border: 0.2em solid #000; /* 2px */
  border-radius: 0.4em; /* 4px */

  box-shadow: 0 0.1em 0.2em rgb(0 0 0 / 45%); /* 0 1px 2px */

  background: #f0f0f0;
  background: linear-gradient(0deg, #e3e3e3, #fcfcfc 50%, #f0f0f0);
}

.select .value {
  display: inline-block;
  width: 100%;
  overflow: hidden;

  white-space: nowrap;
  text-overflow: ellipsis;
  vertical-align: top;
}

.select::after {
  content: "▼";
  position: absolute;
  z-index: 1;
  height: 100%;
  width: 2em; /* 20px */
  top: 0;
  right: 0;

  padding-top: 0.1em;

  box-sizing: border-box;

  text-align: center;

  border-left: 0.2em solid #000;
  border-radius: 0 0.1em 0.1em 0;

  background-color: #000;
  color: #fff;
}

.select .optList {
  z-index: 2;

  list-style: none;
  margin: 0;
  padding: 0;

  background: #f0f0f0;
  border: 0.2em solid #000;
  border-top-width: 0.1em;
  border-radius: 0 0 0.4em 0.4em;

  box-shadow: 0 0.2em 0.4em rgb(0 0 0 / 40%);

  box-sizing: border-box;

  min-width: 100%;
  max-height: 10em; /* 100px */
  overflow-y: auto;
  overflow-x: hidden;
}

.select .option {
  padding: 0.2em 0.3em;
}

.select .highlight {
  background: #000;
  color: #ffffff;
}
```

```js hidden
function deactivateSelect(select) {
  if (!select.classList.contains("active")) return;

  const optList = select.querySelector(".optList");

  optList.classList.add("hidden");
  select.classList.remove("active");
}

function activeSelect(select, selectList) {
  if (select.classList.contains("active")) return;

  selectList.forEach(deactivateSelect);
  select.classList.add("active");
}

function toggleOptList(select, show) {
  const optList = select.querySelector(".optList");

  optList.classList.toggle("hidden");
}

function highlightOption(select, option) {
  const optionList = select.querySelectorAll(".option");

  optionList.forEach((other) => {
    other.classList.remove("highlight");
  });

  option.classList.add("highlight");
}

function updateValue(select, index) {
  const nativeWidget = select.previousElementSibling;
  const value = select.querySelector(".value");
  const optionList = select.querySelectorAll(".option");

  nativeWidget.selectedIndex = index;
  value.textContent = optionList[index].textContent;
  highlightOption(select, optionList[index]);
}

function getIndex(select) {
  const nativeWidget = select.previousElementSibling;

  return nativeWidget.selectedIndex;
}

window.addEventListener("load", () => {
  const form = document.querySelector("form");

  form.classList.remove("no-widget");
  form.classList.add("widget");
});

window.addEventListener("load", () => {
  const selectList = document.querySelectorAll(".select");

  selectList.forEach((select) => {
    const optionList = select.querySelectorAll(".option");

    optionList.forEach((option) => {
      option.addEventListener("mouseover", () => {
        highlightOption(select, option);
      });
    });

    select.addEventListener("click", (event) => {
      toggleOptList(select);
    });

    select.addEventListener("focus", (event) => {
      activeSelect(select, selectList);
    });

    select.addEventListener("blur", (event) => {
      deactivateSelect(select);
    });
  });
});

window.addEventListener("load", () => {
  const selectList = document.querySelectorAll(".select");

  selectList.forEach((select) => {
    const optionList = select.querySelectorAll(".option");
    const selectedIndex = getIndex(select);

    select.tabIndex = 0;
    select.previousElementSibling.tabIndex = -1;

    updateValue(select, selectedIndex);

    optionList.forEach((option, index) => {
      option.addEventListener("click", (event) => {
        updateValue(select, index);
      });
    });

    select.addEventListener("keyup", (event) => {
      let index = getIndex(select);

      if (event.key === "Escape") {
        deactivateSelect(select);
      }
      if (event.key === "ArrowDown" && index < optionList.length - 1) {
        index++;
      }
      if (event.key === "ArrowUp" && index > 0) {
        index--;
      }

      updateValue(select, index);
    });
  });
});
```

{{EmbedLiveSample("live_example_2",120,130)}}

Ma aspetti un secondo, abbiamo veramente finito?

## Renderlo accessibile

Abbiamo costruito qualcosa che funziona e anche se siamo lontani da una casella di selezione completamente funzionale, funziona bene. Ma quello che abbiamo fatto non è altro che manipolare il DOM. Non ha una vera semantica, e anche se sembra una casella di selezione, dal punto di vista del browser non lo è, quindi le tecnologie assistive non saranno in grado di capire che è una casella di selezione. In breve, questa bella nuova casella di selezione non è accessibile!

Fortunatamente, c'è una soluzione e si chiama [ARIA](/it/docs/Web/Accessibility/ARIA). ARIA sta per "Accessible Rich Internet Application", ed è [una specifica del W3C](https://www.w3.org/TR/wai-aria/) progettata specificamente per ciò che stiamo facendo qui: rendere le applicazioni web ei controlli personalizzati accessibili. È fondamentalmente un insieme di attributi che estendono l'HTML in modo da poter descrivere meglio i ruoli, gli stati e le proprietà come se l'elemento che abbiamo appena ideato fosse l'elemento nativo che cerca di passare. Usare questi attributi può essere fatto modificando il markup HTML. Aggiorniamo anche gli attributi ARIA tramite JavaScript man mano che l'utente aggiorna il valore selezionato.

### L'attributo `role`

L'attributo chiave utilizzato da [ARIA](/it/docs/Web/Accessibility/ARIA) è l'attributo [`role`](/it/docs/Web/Accessibility/ARIA/Guides/Techniques). L'attributo [`role`](/it/docs/Web/Accessibility/ARIA/Guides/Techniques) accetta un valore che definisce a cosa serve un elemento. Ogni ruolo definisce i propri requisiti e comportamenti. Nel nostro esempio, utilizzeremo il ruolo [`listbox`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/listbox_role). È un "ruolo composito", il che significa che gli elementi con quel ruolo si aspettano di avere figli, ciascuno con un ruolo specifico (in questo caso, almeno un figlio con il ruolo `option`).

Vale anche la pena notare che ARIA definisce ruoli applicati per impostazione predefinita al markup HTML standard. Ad esempio, l'elemento {{HTMLElement("table")}} corrisponde al ruolo `grid`, e l'elemento {{HTMLElement("ul")}} corrisponde al ruolo `list`. Poiché usiamo un elemento {{HTMLElement("ul")}}, vogliamo assicurarci che il ruolo `listbox` del nostro controllo sostituisca il ruolo `list` dell'elemento {{HTMLElement("ul")}}. A tal fine, utilizzeremo il ruolo `presentation`. Questo ruolo è progettato per farci indicare che un elemento non ha un significato speciale ed è usato solo per presentare informazioni. Lo applicheremo al nostro elemento {{HTMLElement("ul")}}.

Per supportare il ruolo [`listbox`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/listbox_role), dobbiamo solo aggiornare il nostro HTML in questo modo:

```html
<!-- We add the role="listbox" attribute to our top element -->
<div class="select" role="listbox">
  <span class="value">Cherry</span>
  <!-- We also add the role="presentation" to the ul element -->
  <ul class="optList" role="presentation">
    <!-- And we add the role="option" attribute to all the li elements -->
    <li role="option" class="option">Cherry</li>
    <li role="option" class="option">Lemon</li>
    <li role="option" class="option">Banana</li>
    <li role="option" class="option">Strawberry</li>
    <li role="option" class="option">Apple</li>
  </ul>
</div>
```

> [!NOTE]
> Includere sia l'attributo `role` che un attributo `class` non è necessario. Invece di usare `.option` utilizzi i [selettori di attributo](/it/docs/Web/CSS/Attribute_selectors) `[role="option"]` nel suo CSS.

### L'attributo `aria-selected`

Usare l'attributo [`role`](/it/docs/Web/Accessibility/ARIA/Guides/Techniques) non è sufficiente. [ARIA](/it/docs/Web/Accessibility/ARIA) fornisce anche molti stati e attributi di proprietà. Più e meglio li usa, meglio il suo controllo sarà compreso dalle tecnologie assistive. Nel nostro caso, limiteremo il nostro utilizzo a un attributo: `aria-selected`.

L'attributo `aria-selected` è usato per indicare quale opzione è attualmente selezionata; ciò consente alle tecnologie assistive di informare l'utente su quale sia la selezione corrente. Lo useremo dinamicamente con JavaScript per contrassegnare l'opzione selezionata ogni volta che l'utente ne sceglie una. A tal fine, dobbiamo rivedere la nostra funzione `updateValue()`:

```js
function updateValue(select, index) {
  const nativeWidget = select.previousElementSibling;
  const value = select.querySelector(".value");
  const optionList = select.querySelectorAll('[role="option"]');

  // We make sure that all the options are not selected
  optionList.forEach((other) => {
    other.setAttribute("aria-selected", "false");
  });

  // We make sure the chosen option is selected
  optionList[index].setAttribute("aria-selected", "true");

  nativeWidget.selectedIndex = index;
  value.textContent = optionList[index].textContent;
  highlightOption(select, optionList[index]);
}
```

Potrebbe sembrare più semplice lasciare che un lettore di schermo si concentri sul select fuori schermo e ignori il nostro stilizzato, ma questa non è una soluzione accessibile. I lettori di schermo non sono limitati a persone non vedenti; persone con bassa visione e anche vista perfetta li usano. Per questo motivo, non si può far concentrare il lettore di schermo su un elemento fuori schermo.

Di seguito è riportato il risultato finale di tutte queste modifiche (avrà un'idea migliore provandolo con una tecnologia assistiva come [NVDA](https://www.nvaccess.org/) o [VoiceOver](https://www.apple.com/accessibility/features/?vision)).

#### Esempio dal vivo

Controlla il [codice sorgente completo qui](/it/docs/Learn_web_development/Extensions/Forms/How_to_build_custom_form_controls/Example_5).

```html hidden
<form class="no-widget">
  <select name="myFruit">
    <option>Cherry</option>
    <option>Lemon</option>
    <option>Banana</option>
    <option>Strawberry</option>
    <option>Apple</option>
  </select>

  <div class="select" role="listbox">
    <span class="value">Cherry</span>
    <ul class="optList hidden" role="presentation">
      <li class="option" role="option" aria-selected="true">Cherry</li>
      <li class="option" role="option">Lemon</li>
      <li class="option" role="option">Banana</li>
      <li class="option" role="option">Strawberry</li>
      <li class="option" role="option">Apple</li>
    </ul>
  </div>
</form>
```

```css hidden
.widget select,
.no-widget .select {
  position: absolute;
  left: -5000em;
  height: 0;
  overflow: hidden;
}

.select {
  position: relative;
  display: inline-block;
}

.select.active,
.select:focus {
  box-shadow: 0 0 3px 1px #227755;
  outline-color: transparent;
}

.select .optList {
  position: absolute;
  top: 100%;
  left: 0;
}

.select .optList.hidden {
  max-height: 0;
  visibility: hidden;
}

.select {
  font-size: 0.625em; /* 10px */
  font-family: Verdana, Arial, sans-serif;

  box-sizing: border-box;

  padding: 0.1em 2.5em 0.2em 0.5em; /* 1px 25px 2px 5px */
  width: 10em; /* 100px */

  border: 0.2em solid #000; /* 2px */
  border-radius: 0.4em; /* 4px */

  box-shadow: 0 0.1em 0.2em rgb(0 0 0 / 45%); /* 0 1px 2px */

  background: #f0f0f0;
  background: linear-gradient(0deg, #e3e3e3, #fcfcfc 50%, #f0f0f0);
}

.select .value {
  display: inline-block;
  width: 100%;
  overflow: hidden;

  white-space: nowrap;
  text-overflow: ellipsis;
  vertical-align: top;
}

.select::after {
  content: "▼";
  position: absolute;
  z-index: 1;
  height: 100%;
  width: 2em; /* 20px */
  top: 0;
  right: 0;

  padding-top: 0.1em;

  box-sizing: border-box;

  text-align: center;

  border-left: 0.2em solid #000;
  border-radius: 0 0.1em 0.1em 0;

  background-color: #000;
  color: #fff;
}

.select .optList {
  z-index: 2;

  list-style: none;
  margin: 0;
  padding: 0;

  background: #f0f0f0;
  border: 0.2em solid #000;
  border-top-width: 0.1em;
  border-radius: 0 0 0.4em 0.4em;

  box-shadow: 0 0.2em 0.4em rgb(0 0 0 / 40%);

  box-sizing: border-box;

  min-width: 100%;
  max-height: 10em; /* 100px */
  overflow-y: auto;
  overflow-x: hidden;
}

.select .option {
  padding: 0.2em 0.3em;
}

.select .highlight {
  background: #000;
  color: #ffffff;
}
```

```js hidden
function deactivateSelect(select) {
  if (!select.classList.contains("active")) return;

  const optList = select.querySelector(".optList");

  optList.classList.add("hidden");
  select.classList.remove("active");
}

function activeSelect(select, selectList) {
  if (select.classList.contains("active")) return;

  selectList.forEach(deactivateSelect);
  select.classList.add("active");
}

function toggleOptList(select, show) {
  const optList = select.querySelector(".optList");

  optList.classList.toggle("hidden");
}

function highlightOption(select, option) {
  const optionList = select.querySelectorAll(".option");

  optionList.forEach((other) => {
    other.classList.remove("highlight");
  });

  option.classList.add("highlight");
}

function updateValue(select, index) {
  const nativeWidget = select.previousElementSibling;
  const value = select.querySelector(".value");
  const optionList = select.querySelectorAll(".option");

  optionList.forEach((other) => {
    other.setAttribute("aria-selected", "false");
  });

  optionList[index].setAttribute("aria-selected", "true");

  nativeWidget.selectedIndex = index;
  value.textContent = optionList[index].textContent;
  highlightOption(select, optionList[index]);
}

function getIndex(select) {
  const nativeWidget = select.previousElementSibling;

  return nativeWidget.selectedIndex;
}

window.addEventListener("load", () => {
  const form = document.querySelector("form");

  form.classList.remove("no-widget");
  form.classList.add("widget");
});

window.addEventListener("load", () => {
  const selectList = document.querySelectorAll(".select");

  selectList.forEach((select) => {
    const optionList = select.querySelectorAll(".option");
    const selectedIndex = getIndex(select);

    select.tabIndex = 0;
    select.previousElementSibling.tabIndex = -1;

    updateValue(select, selectedIndex);

    optionList.forEach((option, index) => {
      option.addEventListener("mouseover", () => {
        highlightOption(select, option);
      });

      option.addEventListener("click", (event) => {
        updateValue(select, index);
      });
    });

    select.addEventListener("click", (event) => {
      toggleOptList(select);
    });

    select.addEventListener("focus", (event) => {
      activeSelect(select, selectList);
    });

    select.addEventListener("blur", (event) => {
      deactivateSelect(select);
    });

    select.addEventListener("keyup", (event) => {
      let index = getIndex(select);

      if (event.key === "Escape") {
        deactivateSelect(select);
      }
      if (event.key === "ArrowDown" && index < optionList.length - 1) {
        index++;
      }
      if (event.key === "ArrowUp" && index > 0) {
        index--;
      }

      updateValue(select, index);
    });
  });
});
```

{{EmbedLiveSample("live_example_3",120,130)}}

Se desidera andare avanti, il codice in questo esempio necessita di qualche miglioramento prima che diventi generico e riutilizzabile. Questo è un esercizio che può provare a eseguire. Due suggerimenti per aiutarla in questo: il primo argomento per tutte le nostre funzioni è lo stesso, il che significa che quelle funzioni hanno bisogno dello stesso contesto. Costruire un oggetto per condividere quel contesto sarebbe saggio.

## Un approccio alternativo: Utilizzo dei pulsanti radio

Nell'esempio sopra, abbiamo reinventato un elemento {{htmlelement('select')}} utilizzando HTML non semantico, CSS e JavaScript. Questo select stava selezionando un'opzione tra un numero limitato di opzioni, che è la stessa funzionalità di un gruppo omonimo di pulsanti {{htmlelement('input/radio', 'radio')}}.

Potremmo quindi reinventare questo utilizzando invece i pulsanti radio; diamo un'occhiata a questa opzione.

Possiamo iniziare con un elenco non ordinato completamente semantico e accessibile di pulsanti {{htmlelement('input/radio','radio')}} con un {{htmlelement('label')}} associato, etichettando l'intero gruppo con una coppia {{htmlelement('fieldset')}} e {{htmlelement('legend')}} semanticamente appropriata.

```html
<fieldset>
  <legend>Pick a fruit</legend>
  <ul class="styledSelect">
    <li>
      <input
        type="radio"
        name="fruit"
        value="Cherry"
        id="fruitCherry"
        checked />
      <label for="fruitCherry">Cherry</label>
    </li>
    <li>
      <input type="radio" name="fruit" value="Lemon" id="fruitLemon" />
      <label for="fruitLemon">Lemon</label>
    </li>
    <li>
      <input type="radio" name="fruit" value="Banana" id="fruitBanana" />
      <label for="fruitBanana">Banana</label>
    </li>
    <li>
      <input
        type="radio"
        name="fruit"
        value="Strawberry"
        id="fruitStrawberry" />
      <label for="fruitStrawberry">Strawberry</label>
    </li>
    <li>
      <input type="radio" name="fruit" value="Apple" id="fruitApple" />
      <label for="fruitApple">Apple</label>
    </li>
  </ul>
</fieldset>
```

Faremo un po' di styling dell'elenco dei pulsanti radio (non del legend/fieldset) per farlo sembrare un po' come l'esempio precedente, solo per mostrare che può essere fatto:

```css
.styledSelect {
  display: inline-block;
  padding: 0;
}
.styledSelect li {
  list-style-type: none;
  padding: 0;
  display: flex;
}
.styledSelect [type="radio"] {
  position: absolute;
  left: -100vw;
  top: -100vh;
}
.styledSelect label {
  margin: 0;
  line-height: 2;
  padding: 0 0 0 4px;
}
.styledSelect:not(:focus-within) input:not(:checked) + label {
  height: 0;
  outline-color: transparent;
  overflow: hidden;
}
.styledSelect:not(:focus-within) input:checked + label {
  border: 0.2em solid #000;
  border-radius: 0.4em;
  box-shadow: 0 0.1em 0.2em rgb(0 0 0 / 45%);
}
.styledSelect:not(:focus-within) input:checked + label::after {
  content: "▼";
  background: black;
  float: right;
  color: white;
  padding: 0 4px;
  margin: 0 -4px 0 4px;
}
.styledSelect:focus-within {
  border: 0.2em solid #000;
  border-radius: 0.4em;
  box-shadow: 0 0.1em 0.2em rgb(0 0 0 / 45%);
}
.styledSelect:focus-within input:checked + label {
  background-color: #333;
  color: #fff;
  width: 100%;
}
```

Senza JavaScript, e con solo un po' di CSS, possiamo stilizzare l'elenco dei pulsanti radio per visualizzare solo l'elemento selezionato. Quando il focus è all'interno del `<ul>` nel `<fieldset>`, l'elenco si apre e le frecce su e giù (e sinistra e destra) funzionano per selezionare gli elementi precedente e successivo. Provi:

{{EmbedLiveSample("An_alternative_approach_Using_radio_buttons",200,240)}}

Questo funziona, in una certa misura, senza JavaScript. Abbiamo creato un controllo simile al nostro controllo personalizzato, che funziona anche se il JavaScript fallisce. Sembra una grande soluzione, giusto? Beh, non al 100%. Funziona con la tastiera, ma non come previsto con un clic del mouse. Probabilmente ha più senso usare gli standard web come base per i controlli personalizzati invece di fare affidamento sui framework per creare elementi senza semantica nativa. Tuttavia, il nostro controllo non ha la stessa funzionalità che un `<select>` ha nativamente.

Dal lato positivo, questo controllo è completamente accessibile a un lettore di schermo e completamente navigabile tramite la tastiera. Tuttavia, questo controllo non è un sostituto di un {{htmlelement('select')}}. C'è funzionalità che differisce e/o manca. Ad esempio, tutte e quattro le frecce navigano tra le opzioni, ma cliccando la freccia giù quando l'utente è sull'ultimo pulsante lo porta al primo pulsante; non si ferma in cima e in fondo all'elenco delle opzioni come fa un `<select>`.

Lasceremo l'aggiunta di questa funzionalità mancante come un esercizio per il lettore.

## Conclusione

Abbiamo visto tutte le basi della costruzione di un controllo modulo personalizzato, ma come può vedere non è banale. Prima di creare il proprio controllo personalizzato, consideri se l'HTML fornisce elementi alternativi che possono essere usati per supportare adeguatamente i suoi requisiti. Se ha bisogno di creare un controllo personalizzato, spesso è più facile fare affidamento su librerie esterne invece di costruire il proprio. Ma, se crea il proprio, modifichi gli elementi esistenti, o usi un framework per implementare un controllo preconfezionato, ricordi che creare un controllo di modulo utilizzabile e accessibile è più complicato di quanto sembri.

Ecco alcune librerie che dovrebbe considerare prima di scrivere il proprio codice:

- [jQuery UI](https://jqueryui.com/)
- [AXE accessible custom select dropdowns](https://www.webaxe.org/accessible-custom-select-dropdowns/)
- [msDropDown](https://github.com/marghoobsuleman/ms-Dropdown)

Se crea controlli alternativi tramite pulsanti radio, il suo JavaScript o con una libreria di terze parti, assicuri che sia accessibile e a prova di futuro; cioè, deve essere in grado di funzionare meglio con una varietà di browser la cui compatibilità con gli standard web che usano varia. Buon divertimento!
