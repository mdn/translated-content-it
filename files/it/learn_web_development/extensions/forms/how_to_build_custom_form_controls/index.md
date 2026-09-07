---
title: Come creare controlli di modulo personalizzati
short-title: Controlli di modulo personalizzati
slug: Learn_web_development/Extensions/Forms/How_to_build_custom_form_controls
l10n:
  sourceCommit: c9f3d85f24d7839c9fe36a68d8042d088d906147
---

Esistono alcuni casi in cui i controlli di modulo HTML nativi disponibili possono sembrare insufficienti. Ad esempio, se è necessario [applicare stili avanzati](/it/docs/Learn_web_development/Extensions/Forms/Advanced_form_styling) ad alcuni controlli come l'elemento {{HTMLElement("select")}}, oppure se si desidera fornire comportamenti personalizzati, si può prendere in considerazione la creazione di controlli propri.

In questo articolo verrà illustrato come creare un controllo personalizzato. A questo scopo, verrà usato un esempio: ricreare l'elemento {{HTMLElement("select")}}. Verrà inoltre discusso come, quando e se abbia senso creare un controllo proprio, e cosa considerare quando la creazione di un controllo è un requisito.

> [!NOTE]
> L'attenzione sarà rivolta alla creazione del controllo, non a come rendere il codice generico e riutilizzabile; ciò comporterebbe codice JavaScript e manipolazione del DOM non banali in un contesto sconosciuto, e non rientra nell'ambito di questo articolo.

## Progettazione, struttura e semantica

Prima di creare un controllo personalizzato, occorre iniziare definendo con precisione ciò che si desidera. Questo consentirà di risparmiare tempo prezioso. In particolare, è importante definire chiaramente tutti gli stati del controllo. A tale scopo, è utile iniziare da un controllo esistente i cui stati e comportamenti siano ben noti, in modo da riprodurli il più possibile.

Nel nostro esempio, verrà ricreato l'elemento {{HTMLElement("select")}}. Ecco il risultato che si desidera ottenere:

![I tre stati di una casella select](custom-select.png)

Questa schermata mostra i tre stati principali del controllo: lo stato normale (a sinistra), lo stato attivo (al centro) e lo stato aperto (a destra).

In termini di comportamento, si sta ricreando un elemento HTML nativo. Pertanto, dovrebbe avere gli stessi comportamenti e la stessa semantica dell'elemento HTML nativo. Il controllo deve poter essere usato con il mouse e con la tastiera, oltre a essere comprensibile da un lettore di schermo, proprio come qualsiasi controllo nativo. Iniziamo definendo come il controllo raggiunge ogni stato:

**Il controllo si trova nel suo stato normale quando:**

- la pagina viene caricata;
- il controllo era attivo e l'utente fa clic in qualunque punto esterno a esso;
- il controllo era attivo e l'utente sposta il focus su un altro controllo usando la tastiera, ad esempio con il tasto <kbd>Tab</kbd>.

**Il controllo si trova nel suo stato attivo quando:**

- l'utente vi fa clic sopra o lo tocca su uno schermo touch;
- l'utente preme il tasto Tab e il controllo riceve il focus;
- il controllo si trovava nello stato aperto e l'utente vi fa clic sopra.

**Il controllo si trova nel suo stato aperto quando:**

- il controllo si trova in qualunque stato diverso da quello aperto e l'utente vi fa clic sopra.

Una volta noto come modificare gli stati, è importante definire come modificare il valore del controllo:

**Il valore cambia quando:**

- l'utente fa clic su un'opzione mentre il controllo si trova nello stato aperto;
- l'utente preme i tasti freccia su o freccia giù mentre il controllo si trova nello stato attivo.

**Il valore non cambia quando:**

- l'utente preme il tasto freccia su quando è selezionata la prima opzione;
- l'utente preme il tasto freccia giù quando è selezionata l'ultima opzione.

Infine, definiamo come si comporteranno le opzioni del controllo:

- Quando il controllo viene aperto, l'opzione selezionata viene evidenziata.
- Quando il mouse passa sopra un'opzione, l'opzione viene evidenziata e l'opzione precedentemente evidenziata torna allo stato normale.

Ai fini dell'esempio, ci fermeremo qui; tuttavia, chi legge con attenzione noterà che mancano alcuni comportamenti. Ad esempio, cosa accade se l'utente preme il tasto Tab mentre il controllo si trova nello stato aperto? La risposta è _nulla_. Bene, il comportamento corretto sembra ovvio, ma il fatto è che, poiché non è definito nelle specifiche, è molto facile trascurarlo. Questo è particolarmente vero in un ambiente di lavoro di gruppo, quando le persone che progettano il comportamento del controllo sono diverse da quelle che lo implementano.

Un altro esempio interessante: cosa accade se l'utente preme i tasti freccia su o freccia giù mentre il controllo si trova nello stato aperto? Questo caso è un po' più complesso. Se si considera che lo stato attivo e lo stato aperto siano completamente diversi, la risposta è ancora «non accadrà nulla», poiché non è stata definita alcuna interazione tramite tastiera per lo stato aperto. D'altra parte, se si considera che lo stato attivo e lo stato aperto si sovrappongano in parte, il valore potrebbe cambiare, ma l'opzione non verrebbe certamente evidenziata di conseguenza, ancora una volta perché non è stata definita alcuna interazione tramite tastiera sulle opzioni quando il controllo si trova nello stato aperto. È stato definito soltanto ciò che deve accadere quando il controllo viene aperto, ma non ciò che accade dopo.

Occorre riflettere ulteriormente: che dire del tasto Esc? La pressione del tasto <kbd>Esc</kbd> chiude un select aperto. Ricorda: se si desidera fornire la stessa funzionalità dell'elemento nativo {{htmlelement('select')}}, questo deve comportarsi esattamente come il select per tutti gli utenti, dalla tastiera al mouse, dal touch al lettore di schermo e con qualsiasi altro dispositivo di input.

Nel nostro esempio, le specifiche mancanti sono evidenti e verranno quindi gestite, ma per controlli nuovi ed esotici questo può costituire un vero problema. Per quanto riguarda gli elementi standardizzati, tra i quali figura {{htmlelement('select')}}, gli autori delle specifiche hanno dedicato una quantità enorme di tempo a specificare tutte le interazioni per ogni caso d'uso e per ogni dispositivo di input. Creare nuovi controlli non è così semplice, soprattutto se si sta creando qualcosa che non è mai stato realizzato prima e, pertanto, nessuno ha la minima idea dei comportamenti e delle interazioni attesi. Almeno il select esiste già, quindi si sa come dovrebbe comportarsi.

La progettazione di nuove interazioni è generalmente un'opzione solo per i grandi attori del settore che dispongono di una diffusione sufficiente perché un'interazione da loro creata possa diventare uno standard. Ad esempio, Apple ha introdotto la rotella di scorrimento con l'iPod nel 2001. Aveva una quota di mercato tale da poter introdurre con successo un modo completamente nuovo di interagire con un dispositivo, cosa che la maggior parte delle aziende produttrici di dispositivi non può fare.

È preferibile non inventare nuove interazioni utente. Per ogni interazione aggiunta, è fondamentale dedicare tempo alla fase di progettazione; se un comportamento viene definito male o ci si dimentica di definirlo, sarà molto difficile ridefinirlo una volta che gli utenti si saranno abituati. In caso di dubbi, è opportuno chiedere l'opinione di altre persone e, se il budget lo consente, non esitare a [eseguire test con gli utenti](https://en.wikipedia.org/wiki/Usability_testing). Questo processo è chiamato UX Design. Per approfondire l'argomento, è possibile consultare le seguenti risorse utili:

- [UXMatters.com](https://www.uxmatters.com/)
- [La sezione UX Design di SmashingMagazine](https://www.smashingmagazine.com/)

> [!NOTE]
> Inoltre, nella maggior parte dei sistemi esiste un modo per aprire l'elemento {{HTMLElement("select")}} con la tastiera e visualizzare tutte le scelte disponibili, equivalente a fare clic sull'elemento {{HTMLElement("select")}} con il mouse. In Windows ciò si ottiene con <kbd>Alt</kbd> + <kbd>Freccia giù</kbd>. Questo comportamento non è stato implementato nell'esempio, ma sarebbe facile farlo, poiché il meccanismo è già stato implementato per l'evento `click`.

## Definizione della struttura HTML e di parte della semantica

Ora che è stata decisa la funzionalità di base del controllo, è il momento di iniziare a crearlo. Il primo passaggio consiste nel definirne la struttura HTML e attribuirgli una semantica di base. Ecco ciò che serve per ricreare un elemento {{HTMLElement("select")}}:

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

Si noti l'uso dei nomi delle classi: identificano ogni parte rilevante indipendentemente dagli elementi HTML effettivamente usati. Questo è importante per assicurarsi di non vincolare CSS e JavaScript a una rigida struttura HTML, così da poter apportare modifiche all'implementazione in seguito senza interrompere il codice che usa il controllo. Ad esempio, cosa accadrebbe se in seguito si volesse implementare l'equivalente dell'elemento {{HTMLElement("optgroup")}}?

I nomi delle classi, tuttavia, non forniscono alcun valore semantico. Nello stato attuale, l'utente di un lettore di schermo «vede» soltanto un elenco non ordinato. Tra poco verrà aggiunta la semantica ARIA.

## Creazione dell'aspetto mediante CSS

Ora che esiste una struttura, è possibile iniziare a progettare il controllo. Lo scopo principale della creazione di questo controllo personalizzato è poterlo stilizzare esattamente come desiderato. A tale scopo, il lavoro CSS verrà diviso in due parti: la prima sarà costituita dalle regole CSS assolutamente necessarie per far comportare il controllo come un elemento {{HTMLElement("select")}}, mentre la seconda consisterà negli stili più elaborati usati per conferirgli l'aspetto desiderato.

### Stili necessari

Gli stili necessari sono quelli richiesti per gestire i tre stati del controllo.

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

È necessaria una classe aggiuntiva, `active`, per definire l'aspetto del controllo quando si trova nello stato attivo. Poiché il controllo può ricevere il focus, questo stile personalizzato viene duplicato con la pseudo-classe {{cssxref(":focus")}} per assicurarsi che si comportino allo stesso modo.

```css
.select.active,
.select:focus {
  outline-color: transparent;

  /* This box-shadow property is not exactly required, however it's imperative to ensure
     active state is visible, especially to keyboard users, that we use it as a default value. */
  box-shadow: 0 0 3px 1px #227755;
}
```

Ora gestiamo l'elenco delle opzioni:

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

È necessaria una classe aggiuntiva per gestire il caso in cui l'elenco delle opzioni è nascosto. Ciò è necessario per gestire le differenze tra lo stato attivo e lo stato aperto, che non corrispondono esattamente.

```css
.select .optList.hidden {
  /* This is a simple way to hide the list in an accessible way;
     we will talk more about accessibility in the end */
  max-height: 0;
  visibility: hidden;
}
```

> [!NOTE]
> Si sarebbe potuto usare anche `transform: scale(1, 0)` per assegnare all'elenco delle opzioni altezza zero e larghezza completa.

### Abbellimento

Ora che la funzionalità di base è disponibile, può iniziare la parte più divertente. Quanto segue è soltanto un esempio di ciò che è possibile fare e corrisponderà alla schermata mostrata all'inizio di questo articolo. È comunque possibile sperimentare liberamente e vedere cosa si riesce a ottenere.

```css
.select {
  /* The computations are made assuming 1em equals 16px which is the default value in most browsers.
     If you are lost with px to em conversion, try https://nekocalc.com/px-to-em-converter */
  font-size: 0.625em; /* this (10px) is the new font size context for em value in this context */
  font-family: "Verdana", "Arial", sans-serif;

  box-sizing: border-box;

  /* We need extra room for the down arrow we will add */
  padding: 0.1em 2.5em 0.2em 0.5em;
  width: 10em; /* 100px */

  border: 0.2em solid black;
  border-radius: 0.4em;
  box-shadow: 0 0.1em 0.2em rgb(0 0 0 / 45%);

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

Non è necessario un elemento aggiuntivo per progettare la freccia verso il basso; viene invece usato il pseudo-elemento {{cssxref("::after")}}. Potrebbe essere implementato anche usando una semplice immagine di sfondo sulla classe `select`.

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

  border-left: 0.2em solid black;
  border-radius: 0 0.1em 0.1em 0;

  background-color: black;
  color: white;
  text-align: center;
}
```

Successivamente, stilizziamo l'elenco delle opzioni:

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

  border: 0.2em solid black;
  border-top-width: 0.1em;
  border-radius: 0 0 0.4em 0.4em;

  box-shadow: 0 0.2em 0.4em rgb(0 0 0 / 40%);
  background: #f0f0f0;
}
```

Per le opzioni, occorre aggiungere una classe `highlight` per poter identificare il valore che l'utente selezionerà, oppure ha selezionato.

```css
.select .option {
  padding: 0.2em 0.3em; /* 2px 3px */
}

.select .highlight {
  background: black;
  color: white;
}
```

Ecco quindi il risultato con i tre stati ([consulta qui il codice sorgente](/it/docs/Learn_web_development/Extensions/Forms/How_to_build_custom_form_controls/Example_1)):

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
  font-family: "Verdana", "Arial", sans-serif;

  box-sizing: border-box;

  padding: 0.1em 2.5em 0.2em 0.5em; /* 1px 25px 2px 5px */
  width: 10em; /* 100px */

  border: 0.2em solid black; /* 2px */
  border-radius: 0.4em; /* 4px */

  box-shadow: 0 0.1em 0.2em rgb(0 0 0 / 45%); /* 0 1px 2px */

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

  border-left: 0.2em solid black;
  border-radius: 0 0.1em 0.1em 0;

  background-color: black;
  color: white;
}

.select .optList {
  z-index: 2;

  list-style: none;
  margin: 0;
  padding: 0;

  background: #f0f0f0;
  border: 0.2em solid black;
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
  background: black;
  color: white;
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
  font-family: "Verdana", "Arial", sans-serif;

  box-sizing: border-box;

  padding: 0.1em 2.5em 0.2em 0.5em; /* 1px 25px 2px 5px */
  width: 10em; /* 100px */

  border: 0.2em solid black; /* 2px */
  border-radius: 0.4em; /* 4px */

  box-shadow: 0 0.1em 0.2em rgb(0 0 0 / 45%); /* 0 1px 2px */

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

  border-left: 0.2em solid black;
  border-radius: 0 0.1em 0.1em 0;

  background-color: black;
  color: white;
}

.select .optList {
  z-index: 2;

  list-style: none;
  margin: 0;
  padding: 0;

  background: #f0f0f0;
  border: 0.2em solid black;
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
  background: black;
  color: white;
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
  font-family: "Verdana", "Arial", sans-serif;

  box-sizing: border-box;

  padding: 0.1em 2.5em 0.2em 0.5em; /* 1px 25px 2px 5px */
  width: 10em; /* 100px */

  border: 0.2em solid black; /* 2px */
  border-radius: 0.4em; /* 4px */

  box-shadow: 0 0.1em 0.2em rgb(0 0 0 / 45%); /* 0 1px 2px */

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

  border-left: 0.2em solid black;
  border-radius: 0 0.1em 0.1em 0;

  background-color: black;
  color: white;
}

.select .optList {
  z-index: 2;

  list-style: none;
  margin: 0;
  padding: 0;

  background: #f0f0f0;
  border: 0.2em solid black;
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
  background: black;
  color: white;
}
```

{{EmbedLiveSample("Open_state",120,130)}}

## Dare vita al controllo con JavaScript

Ora che progettazione e struttura sono pronte, è possibile scrivere il codice JavaScript per rendere effettivamente funzionante il controllo.

> [!WARNING]
> Il seguente codice è a scopo didattico, non è codice di produzione e non dovrebbe essere usato così com'è. Non è a prova di futuro e non funzionerà nei browser legacy. Inoltre, contiene parti ridondanti che dovrebbero essere ottimizzate nel codice di produzione.

### Perché non funziona?

Prima di iniziare, è importante ricordare che **JavaScript nel browser è una tecnologia inaffidabile**. I controlli personalizzati dipendono da JavaScript per collegare insieme tutti gli elementi. Tuttavia, esistono casi in cui JavaScript non riesce a essere eseguito nel browser:

- L'utente ha disattivato JavaScript: è insolito; al giorno d'oggi pochissime persone disattivano JavaScript.
- Lo script non è stato caricato: è uno dei casi più comuni, specialmente nel mondo mobile, dove la rete non è molto affidabile.
- Lo script contiene bug: questa possibilità dovrebbe essere sempre considerata.
- Lo script entra in conflitto con uno script di terze parti: ciò può accadere con script di tracciamento o con bookmarklet usati dall'utente.
- Lo script entra in conflitto con, oppure è influenzato da, un'estensione del browser, come l'estensione [NoScript](https://addons.mozilla.org/fr/firefox/addon/noscript/) di Firefox o l'estensione [ScriptBlock](https://chromewebstore.google.com/detail/scriptblock/hcdjknjpbnhdoabbngpmfekaecnpajba) di Chrome.
- L'utente utilizza un browser legacy e una delle funzionalità richieste non è supportata: ciò accadrà frequentemente quando si usano API all'avanguardia.
- L'utente interagisce con il contenuto prima che JavaScript sia stato completamente scaricato, analizzato ed eseguito.

A causa di questi rischi, è davvero importante considerare seriamente cosa accadrà se JavaScript non funziona. Verranno discusse opzioni da considerare e trattate le basi nell'esempio; una discussione completa su come risolvere questo problema per tutti gli scenari richiederebbe un libro. Basta ricordare che è fondamentale rendere lo script generico e riutilizzabile.

Nel nostro esempio, se il codice JavaScript non è in esecuzione, verrà mostrato un elemento {{HTMLElement("select")}} standard. Vengono inclusi il controllo personalizzato e l'elemento {{HTMLElement("select")}}; quale dei due viene mostrato dipende dalla classe dell'elemento body, aggiornata dallo script che rende funzionante il controllo quando viene caricato correttamente.

Per ottenere questo risultato sono necessarie due cose:

Innanzitutto, è necessario aggiungere un normale elemento {{HTMLElement("select")}} prima di ogni istanza del controllo personalizzato. Esiste un vantaggio nell'avere questo select «aggiuntivo» anche se JavaScript funziona come previsto: questo select verrà usato per inviare i dati del controllo personalizzato insieme al resto dei dati del modulo. Questo aspetto verrà approfondito in seguito.

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

In secondo luogo, sono necessarie due nuove classi per nascondere l'elemento non necessario: il controllo personalizzato viene nascosto visivamente se lo script non è in esecuzione, oppure viene nascosto l'elemento {{HTMLElement("select")}} «reale» se lo script è in esecuzione. Si noti che, per impostazione predefinita, il codice HTML nasconde il controllo personalizzato.

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

Questo CSS nasconde visivamente uno degli elementi, ma lo mantiene comunque disponibile per i lettori di schermo.

Ora serve un selettore JavaScript per determinare se lo script è in esecuzione o meno. Questo selettore consiste in un paio di righe: se al caricamento della pagina lo script è in esecuzione, rimuoverà la classe `no-widget` e aggiungerà la classe `widget`, scambiando così la visibilità dell'elemento {{HTMLElement("select")}} e del controllo personalizzato.

```js
document.body.classList.remove("no-widget");
document.body.classList.add("widget");
```

#### Senza JS

Consulta il [codice sorgente completo](/it/docs/Learn_web_development/Extensions/Forms/How_to_build_custom_form_controls/Example_2#no_js).

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

Consulta il [codice sorgente completo](/it/docs/Learn_web_development/Extensions/Forms/How_to_build_custom_form_controls/Example_2#js).

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
  font-family: "Verdana", "Arial", sans-serif;

  box-sizing: border-box;

  padding: 0.1em 2.5em 0.2em 0.5em; /* 1px 25px 2px 5px */
  width: 10em; /* 100px */

  border: 0.2em solid black; /* 2px */
  border-radius: 0.4em; /* 4px */

  box-shadow: 0 0.1em 0.2em rgb(0 0 0 / 45%); /* 0 1px 2px */

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

  border-left: 0.2em solid black;
  border-radius: 0 0.1em 0.1em 0;

  background-color: black;
  color: white;
}

.select .optList {
  z-index: 2;

  list-style: none;
  margin: 0;
  padding: 0;

  background: #f0f0f0;
  border: 0.2em solid black;
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
  background: black;
  color: white;
}
```

```js hidden
const form = document.querySelector("form");

form.classList.remove("no-widget");
form.classList.add("widget");
```

{{EmbedLiveSample("With_JS",120,130)}}

> [!NOTE]
> Se si desidera davvero rendere il codice generico e riutilizzabile, anziché effettuare uno scambio di classi è molto meglio aggiungere semplicemente la classe widget per nascondere gli elementi {{HTMLElement("select")}} e aggiungere dinamicamente l'albero DOM che rappresenta il controllo personalizzato dopo ogni elemento {{HTMLElement("select")}} nella pagina.

### Semplificare il lavoro

Nel codice che sta per essere creato, verranno usate le API JavaScript e DOM standard per svolgere tutto il lavoro necessario. Le funzionalità che si intende usare sono le seguenti:

1. [`classList`](/it/docs/Web/API/Element/classList)
2. [`addEventListener()`](/it/docs/Web/API/EventTarget/addEventListener)
3. [`NodeList.forEach()`](/it/docs/Web/API/NodeList/forEach)
4. [`querySelector()`](/it/docs/Web/API/Element/querySelector) e [`querySelectorAll()`](/it/docs/Web/API/Element/querySelectorAll)

### Creazione delle callback degli eventi

La base è pronta. Ora è possibile iniziare a definire tutte le funzioni che verranno usate ogni volta che l'utente interagisce con il controllo.

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

Queste funzioni sono necessarie per gestire i vari stati del controllo personalizzato.

Successivamente, si associano queste funzioni agli eventi appropriati:

```js
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

  // Lose focus if the user hits `esc`
  select.addEventListener("keyup", (event) => {
    // deactivate on keyup of `esc`
    if (event.key === "Escape") {
      deactivateSelect(select);
    }
  });
});
```

A questo punto, il controllo cambierà stato in base alla progettazione, ma il suo valore non viene ancora aggiornato. Questo verrà gestito ora.

#### Esempio live

Consulta il [codice sorgente completo](/it/docs/Learn_web_development/Extensions/Forms/How_to_build_custom_form_controls/Example_3).

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
  font-family: "Verdana", "Arial", sans-serif;

  box-sizing: border-box;

  padding: 0.1em 2.5em 0.2em 0.5em; /* 1px 25px 2px 5px */
  width: 10em; /* 100px */

  border: 0.2em solid black; /* 2px */
  border-radius: 0.4em; /* 4px */

  box-shadow: 0 0.1em 0.2em rgb(0 0 0 / 45%); /* 0 1px 2px */

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

  border-left: 0.2em solid black;
  border-radius: 0 0.1em 0.1em 0;

  background-color: black;
  color: white;
}

.select .optList {
  z-index: 2;

  list-style: none;
  margin: 0;
  padding: 0;

  background: #f0f0f0;
  border: 0.2em solid black;
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
  background: black;
  color: white;
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

const form = document.querySelector("form");

form.classList.remove("no-widget");
form.classList.add("widget");

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

  select.addEventListener("keyup", (event) => {
    if (event.key === "Escape") {
      deactivateSelect(select);
    }
  });
});
```

{{EmbedLiveSample("Live_example",120,130)}}

### Gestione del valore del controllo

Ora che il controllo funziona, è necessario aggiungere codice per aggiornare il suo valore in base all'input dell'utente e rendere possibile l'invio del valore insieme ai dati del modulo.

Il modo più semplice per farlo consiste nell'usare un controllo nativo internamente. Tale controllo terrà traccia del valore usando tutti i controlli integrati forniti dal browser e il valore verrà inviato normalmente quando viene inviato un modulo. Non ha senso reinventare la ruota quando tutto questo può essere fatto automaticamente.

Come visto in precedenza, viene già usato un controllo select nativo come fallback per motivi di accessibilità; il suo valore può essere sincronizzato con quello del controllo personalizzato:

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

Con queste due funzioni, è possibile associare i controlli nativi a quelli personalizzati:

```js
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
```

Nel codice precedente, vale la pena notare l'uso della proprietà [`tabIndex`](/it/docs/Web/API/HTMLElement/tabIndex). L'uso di questa proprietà è necessario per assicurarsi che il controllo nativo non riceva mai il focus e che il controllo personalizzato riceva il focus quando l'utente usa tastiera o mouse.

Con questo, il lavoro è concluso.

#### Esempio live

Consulta il [codice sorgente qui](/it/docs/Learn_web_development/Extensions/Forms/How_to_build_custom_form_controls/Example_4).

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
  font-family: "Verdana", "Arial", sans-serif;

  box-sizing: border-box;

  padding: 0.1em 2.5em 0.2em 0.5em; /* 1px 25px 2px 5px */
  width: 10em; /* 100px */

  border: 0.2em solid black; /* 2px */
  border-radius: 0.4em; /* 4px */

  box-shadow: 0 0.1em 0.2em rgb(0 0 0 / 45%); /* 0 1px 2px */

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

  border-left: 0.2em solid black;
  border-radius: 0 0.1em 0.1em 0;

  background-color: black;
  color: white;
}

.select .optList {
  z-index: 2;

  list-style: none;
  margin: 0;
  padding: 0;

  background: #f0f0f0;
  border: 0.2em solid black;
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
  background: black;
  color: white;
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

const form = document.querySelector("form");

form.classList.remove("no-widget");
form.classList.add("widget");

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
```

{{EmbedLiveSample("live_example_2",120,130)}}

Ma aspetta un attimo: è davvero finita?

## Renderlo accessibile

È stato creato qualcosa che funziona e, anche se è ancora lontano dall'essere una casella select completa, funziona bene. Tuttavia, ciò che è stato fatto non è altro che manipolare il DOM. Non dispone di vera semantica e, anche se sembra una casella select, dal punto di vista del browser non lo è, quindi le tecnologie assistive non saranno in grado di capire che si tratta di una casella select. In breve, questa nuova e graziosa casella select non è accessibile.

Fortunatamente, esiste una soluzione chiamata [ARIA](/it/docs/Web/Accessibility/ARIA). ARIA significa "Accessible Rich Internet Application" ed è una [specifica W3C](https://w3c.github.io/aria/) progettata appositamente per ciò che si sta facendo qui: rendere accessibili le applicazioni web e i controlli personalizzati. Si tratta essenzialmente di un insieme di attributi che estendono HTML, consentendo di descrivere meglio ruoli, stati e proprietà come se l'elemento appena creato fosse l'elemento nativo che tenta di imitare. L'uso di questi attributi può essere effettuato modificando il markup HTML. Gli attributi ARIA vengono inoltre aggiornati tramite JavaScript quando l'utente aggiorna il valore selezionato.

### L'attributo `role`

L'attributo chiave usato da [ARIA](/it/docs/Web/Accessibility/ARIA) è l'attributo [`role`](/it/docs/Web/Accessibility/ARIA/Guides/Techniques). L'attributo [`role`](/it/docs/Web/Accessibility/ARIA/Guides/Techniques) accetta un valore che definisce a cosa serve un elemento. Ogni ruolo definisce i propri requisiti e comportamenti. Nel nostro esempio verrà usato il ruolo [`listbox`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/listbox_role). Si tratta di un «ruolo composito», il che significa che gli elementi con quel ruolo prevedono di avere elementi figli, ciascuno con un ruolo specifico, in questo caso almeno un figlio con il ruolo `option`.

Vale inoltre la pena notare che ARIA definisce ruoli applicati per impostazione predefinita al markup HTML standard. Ad esempio, l'elemento {{HTMLElement("table")}} corrisponde al ruolo `grid` e l'elemento {{HTMLElement("ul")}} corrisponde al ruolo `list`. Poiché viene usato un elemento {{HTMLElement("ul")}}, è necessario assicurarsi che il ruolo `listbox` del controllo prevalga sul ruolo `list` dell'elemento {{HTMLElement("ul")}}. A tale scopo, verrà usato il ruolo `presentation`. Questo ruolo è progettato per indicare che un elemento non ha significato particolare ed è usato esclusivamente per presentare informazioni. Verrà applicato all'elemento {{HTMLElement("ul")}}.

Per supportare il ruolo [`listbox`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/listbox_role), è sufficiente aggiornare l'HTML nel seguente modo:

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
> Non è necessario includere sia l'attributo `role` sia un attributo `class`. Invece di usare `.option`, usare i [selettori di attributo](/it/docs/Web/CSS/Reference/Selectors/Attribute_selectors) `[role="option"]` nel CSS.

### L'attributo `aria-selected`

L'uso dell'attributo [`role`](/it/docs/Web/Accessibility/ARIA/Guides/Techniques) non è sufficiente. [ARIA](/it/docs/Web/Accessibility/ARIA) fornisce anche molti attributi di stato e proprietà. Più questi vengono usati, e meglio vengono usati, più il controllo sarà comprensibile alle tecnologie assistive. In questo caso, l'uso sarà limitato a un attributo: `aria-selected`.

L'attributo `aria-selected` viene usato per contrassegnare quale opzione è attualmente selezionata; ciò consente alle tecnologie assistive di informare l'utente sulla selezione corrente. Verrà usato dinamicamente con JavaScript per contrassegnare l'opzione selezionata ogni volta che l'utente ne sceglie una. A tale scopo, è necessario rivedere la funzione `updateValue()`:

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

Potrebbe essere sembrato più semplice lasciare che un lettore di schermo mettesse il focus sul select fuori schermo e ignorasse quello stilizzato, ma questa non è una soluzione accessibile. I lettori di schermo non sono limitati alle persone non vedenti; li usano anche persone ipovedenti e perfino persone con una vista perfetta. Per questo motivo, non è possibile far mettere al lettore di schermo il focus su un elemento fuori schermo.

Di seguito è mostrato il risultato finale di tutte queste modifiche. Per comprenderlo meglio, è possibile provarlo con una tecnologia assistiva come [NVDA](https://www.nvaccess.org/) o [VoiceOver](https://www.apple.com/accessibility/features/?vision).

#### Esempio live

Consulta il [codice sorgente completo qui](/it/docs/Learn_web_development/Extensions/Forms/How_to_build_custom_form_controls/Example_5).

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
  font-family: "Verdana", "Arial", sans-serif;

  box-sizing: border-box;

  padding: 0.1em 2.5em 0.2em 0.5em; /* 1px 25px 2px 5px */
  width: 10em; /* 100px */

  border: 0.2em solid black; /* 2px */
  border-radius: 0.4em; /* 4px */

  box-shadow: 0 0.1em 0.2em rgb(0 0 0 / 45%); /* 0 1px 2px */

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

  border-left: 0.2em solid black;
  border-radius: 0 0.1em 0.1em 0;

  background-color: black;
  color: white;
}

.select .optList {
  z-index: 2;

  list-style: none;
  margin: 0;
  padding: 0;

  background: #f0f0f0;
  border: 0.2em solid black;
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
  background: black;
  color: white;
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

const form = document.querySelector("form");

form.classList.remove("no-widget");
form.classList.add("widget");

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
```

{{EmbedLiveSample("live_example_3",120,130)}}

Per proseguire, il codice di questo esempio necessita di alcuni miglioramenti prima di diventare generico e riutilizzabile. Questo è un esercizio che è possibile provare a svolgere. Due suggerimenti utili: il primo argomento di tutte le funzioni è lo stesso, il che significa che tali funzioni necessitano dello stesso contesto. Sarebbe opportuno creare un oggetto per condividere quel contesto.

## Un approccio alternativo: usare pulsanti radio

Nell'esempio precedente, è stato ricreato un elemento {{htmlelement('select')}} usando HTML non semantico, CSS e JavaScript. Questo select permetteva di selezionare un'opzione da un numero limitato di opzioni, la stessa funzionalità di un gruppo di pulsanti {{htmlelement('input/radio', 'radio')}} con lo stesso nome.

Si potrebbe quindi ricrearlo invece usando pulsanti radio; esaminiamo questa possibilità.

Si può iniziare con un elenco non ordinato completamente semantico e accessibile di pulsanti {{htmlelement('input/radio','radio')}} con un {{htmlelement('label')}} associato, etichettando l'intero gruppo con una coppia semanticamente appropriata di {{htmlelement('fieldset')}} e {{htmlelement('legend')}}.

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

Verrà applicato un po' di stile all'elenco dei pulsanti radio, ma non a legend/fieldset, per farlo assomigliare in qualche modo all'esempio precedente, solo per dimostrare che è possibile:

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
  padding-left: 4px;
}
.styledSelect:not(:focus-within) input:not(:checked) + label {
  height: 0;
  outline-color: transparent;
  overflow: hidden;
}
.styledSelect:not(:focus-within) input:checked + label {
  border: 0.2em solid black;
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
  border: 0.2em solid black;
  border-radius: 0.4em;
  box-shadow: 0 0.1em 0.2em rgb(0 0 0 / 45%);
}
.styledSelect:focus-within input:checked + label {
  background-color: #333333;
  color: white;
  width: 100%;
}
```

Senza JavaScript e con solo una piccola quantità di CSS, è possibile stilizzare l'elenco di pulsanti radio affinché mostri soltanto l'elemento selezionato. Quando il focus si trova all'interno di `<ul>` nel `<fieldset>`, l'elenco si apre e le frecce su e giù, nonché sinistra e destra, consentono di selezionare gli elementi precedenti e successivi. Provalo:

{{EmbedLiveSample("An_alternative_approach_Using_radio_buttons",200,240)}}

Questo funziona, in una certa misura, senza JavaScript. È stato creato un controllo simile al controllo personalizzato, che funziona anche se JavaScript non riesce a essere eseguito. Sembra un'ottima soluzione, giusto? Beh, non al 100%. Funziona con la tastiera, ma non come previsto con un clic del mouse. Probabilmente ha più senso usare gli standard web come base per i controlli personalizzati invece di fare affidamento su framework per creare elementi senza semantica nativa. Tuttavia, il controllo non possiede la stessa funzionalità che un `<select>` offre nativamente.

Dal lato positivo, questo controllo è completamente accessibile a un lettore di schermo e pienamente navigabile tramite tastiera. Tuttavia, non è un sostituto di {{htmlelement('select')}}. Alcune funzionalità sono diverse e/o mancanti. Ad esempio, tutte e quattro le frecce consentono di navigare tra le opzioni, ma facendo clic sulla freccia giù quando l'utente si trova sull'ultimo pulsante, questo viene portato al primo pulsante; non si ferma all'inizio e alla fine dell'elenco di opzioni come fa un `<select>`.

L'aggiunta di questa funzionalità mancante viene lasciata come esercizio per chi legge.

## Conclusione

Sono state viste tutte le basi per creare un controllo di modulo personalizzato, ma come si può osservare non è un'operazione banale. Prima di creare un controllo personalizzato, occorre considerare se HTML fornisca elementi alternativi che possano supportare adeguatamente i requisiti. Se è davvero necessario creare un controllo personalizzato, spesso è più semplice affidarsi a librerie di terze parti invece di crearne uno da zero. Tuttavia, se si decide di crearne uno, modificare elementi esistenti oppure usare un framework per implementare un controllo preconfezionato, occorre ricordare che creare un controllo di modulo usabile e accessibile è più complicato di quanto sembri.

Ecco alcune librerie da considerare prima di scrivere codice personalizzato:

- [jQuery UI](https://jqueryui.com/)
- [AXE accessible custom select dropdowns](https://www.webaxe.org/accessible-custom-select-dropdowns/)
- [msDropDown](https://github.com/marghoobsuleman/ms-Dropdown)

Se vengono creati controlli alternativi tramite pulsanti radio, JavaScript personalizzato o una libreria di terze parti, assicurarsi che siano accessibili e a prova di funzionalità; devono cioè poter funzionare correttamente con una varietà di browser la cui compatibilità con gli standard web usati può variare. Buon divertimento!
