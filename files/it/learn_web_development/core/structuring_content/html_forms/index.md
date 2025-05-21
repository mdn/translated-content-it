---
title: Form e pulsanti in HTML
short-title: Form e pulsanti
slug: Learn_web_development/Core/Structuring_content/HTML_forms
l10n:
  sourceCommit: 874ad29df9150037acb8a4a3e7550a302c90a080
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Planet_data_table", "Learn_web_development/Core/Structuring_content/Debugging_HTML", "Learn_web_development/Core/Structuring_content")}}

I form e i pulsanti HTML sono strumenti potenti per interagire con gli utenti di un sito web. Più comunemente, forniscono agli utenti i controlli per manipolare un'interfaccia utente (UI) o inserire dati quando richiesto.

In questo articolo, forniamo un'introduzione alle basi dei form e dei pulsanti. C'è molto altro da sapere — molti tipi di input e funzionalità dei form non sono menzionati — ma questo articolo ti darà una solida base per la maggior parte dei casi. Puoi imparare gli utilizzi avanzati o specializzati quando necessario, come parte dell'apprendimento continuo che farai nel corso della tua carriera.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con HTML, come coperto in
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi HTML di base</a
        >. Semantica a livello di testo come <a href="/it/docs/Learn_web_development/Core/Structuring_content/Headings_and_paragraphs"
          >titoli e paragrafi</a
        > e <a href="/it/docs/Learn_web_development/Core/Structuring_content/Lists"
          >liste</a
        >. <a href="/it/docs/Learn_web_development/Core/Structuring_content/Structuring_documents"
          >HTML strutturale</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi dell'apprendimento:</th>
      <td>
        <ul>
          <li>Apprezzare che i form e i pulsanti sono gli strumenti principali per gli utenti per interagire con un sito web, insieme ai link.</li>
          <li>Diversi tipi di pulsanti.</li>
          <li>Tipi comuni di <code>&lt;input&gt;</code>.</li>
          <li>Attributi comuni come <code>name</code> e <code>value</code>.</li>
          <li>L'elemento <code>&lt;form&gt;</code> e le basi dell'invio di un form.</li>
          <li>Rendere i form accessibili con etichette e semantica corretta.</li>
          <li>Altri tipi di controllo: <code>&lt;textarea&gt;</code>, <code>&lt;select&gt;</code>, e <code>&lt;option&gt;</code>.</li>
          <li>Le basi della validazione lato client.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Interazione con gli utenti

Finora nel corso, hai visto un paio di modi in cui gli utenti possono interagire con il web:

- [Link](/it/docs/Learn_web_development/Core/Structuring_content/Creating_links) possono essere usati per navigare a sezioni diverse di contenuto, sia sulla stessa pagina che su una pagina diversa.
- Gli elementi [`<video>` e `<audio>`](/it/docs/Learn_web_development/Core/Structuring_content/HTML_video_and_audio) generalmente dispongono di controlli come play/pausa, avanzamento veloce, riavvolgimento, etc., che consentono agli utenti di fruire del contenuto multimediale a loro piacimento.

Comunque, queste funzionalità tendono a facilitare interazioni unidirezionali, con gli utenti che consumano passivamente il contenuto. Questo va bene, ma il web è un'esperienza bidirezionale. Gli utenti del sito web impostano preferenze su come desiderano fruire del contenuto e dei servizi. Ordinano taxi e richiedono richiamate. Forniscono feedback e presentano reclami. Comprano prodotti e li fanno consegnare a casa.

Per offrire questa esperienza bidirezionale, devi utilizzare pulsanti e form.

I pulsanti sono generalmente creati utilizzando elementi HTML {{htmlelement("button")}} (a volte vengono creati anche utilizzando elementi {{htmlelement("input")}} con i loro attributi `type` impostati su un valore come `button` o `submit`). Questi pulsanti spingibili sono a scopo generale — puoi collegarli per attivare qualsiasi funzionalità desideri, limitata solo dalla tua fantasia e dalle tue abilità di programmazione.

I form sono creati utilizzando elementi come {{htmlelement("form")}}, {{htmlelement("label")}}, {{htmlelement("input")}}, e {{htmlelement("select")}}. Gli elementi del form possono essere utilizzati per creare controlli più complessi di quelli che semplici pulsanti consentono, ad esempio un menu a discesa contenente più opzioni che ti consentono di scegliere tra temi diversi per un elemento dell'interfaccia utente.

Tuttavia, in modo cruciale, possono anche essere utilizzati per creare form che gli utenti compilano quando devono inviare informazioni al server di un sito web. Pensa ai siti di e-commerce — quando vuoi cercare un prodotto da acquistare, utilizzi un form per inserire i termini di ricerca. Quando vuoi pagare per alcuni articoli e finalizzare la consegna, utilizzi un form per inserire il tuo indirizzo postale e un altro form per inserire i dettagli della tua carta di credito.

Ci concentreremo principalmente su questo uso — più tradizionale — degli elementi del form in questo articolo. Nota che i pulsanti sono spesso utilizzati all'interno dei form, per inviare i dati inseriti al server.

Con questa importante teoria fuori dal tavolo, passiamo ad esplorare il codice e vedere come i pulsanti e i form vengono implementati.

## Pulsanti

Come accennato sopra, i pulsanti hanno un paio di usi principali sul web. Prima di tutto, sono utilizzati per attivare funzioni, il che è utile quando si creano controlli UI. Il pulsante più semplice è implementato utilizzando il seguente codice:

```html live-sample___basic-button
<button>Press me</button>
```

Questo è reso come segue:

{{EmbedLiveSample("basic-button", "100%", "60")}}

Il testo che appare tra i tag `<button></button>` viene mostrato all'interno del pulsante e riceve un po' di stile di base dal browser, quindi sembrerà e si comporterà come un pulsante per impostazione predefinita. Fin qui tutto bene. Tuttavia, c'è un problema qui — il nostro pulsante solitario non farà nulla di utile da solo. Per farlo fare qualcosa di utile, devi metterlo all'interno di un form (che copriremo più avanti), o aggiungere un po' di JavaScript.

Ad esempio, se applichi il seguente JavaScript al pulsante sopra:

```html hidden live-sample___basic-button-with-js
<button>Press me</button>
```

```js live-sample___basic-button-with-js
const btn = document.querySelector("button");
btn.addEventListener("click", () => {
  btn.textContent = "YOU CLICKED ME!! ❤️";
  setTimeout(() => {
    btn.textContent = "Press me";
  }, 1000);
});
```

Ottieni il seguente output — prova a cliccarci sopra:

{{EmbedLiveSample("basic-button-with-js", "100%", "60")}}

Non ci si aspetta che tu capisca come funziona il JavaScript per ora. Imparerai di più su di esso più avanti nel corso.

Nella prossima sezione, vedrai una dimostrazione del secondo uso principale dei pulsanti — inviare form.

## L'anatomia di un form

Un form di base contiene tre elementi:

- Un elemento {{htmlelement("form")}}, che racchiude tutti gli altri contenuti del form. Qualsiasi controllo del form all'interno dei tag `<form></form>` fa parte dello stesso form e i loro dati vengono inclusi quando il form viene inviato.
- Una o più coppie costituite ciascuna da un elemento {{htmlelement("label")}} e un elemento di controllo del form (solitamente un elemento {{htmlelement("input")}}, ma ci sono anche altri tipi come {{htmlelement("select")}}):
  - L'elemento di controllo del form consente all'utente di scegliere o inserire alcuni dati, che verranno inviati al server quando il form viene inviato.
  - L'elemento `<label>` fornisce un'etichetta identificativa associata al controllo del form, descrivendo i dati che dovrebbero essere inseriti.
- Un elemento {{htmlelement("button")}}, utilizzato per inviare il form.

Esaminiamo un esempio di base che include i tre elementi sopra menzionati. Questo form potrebbe essere utilizzato per chiedere il nome e l'email di un utente, per iscriverlo a una newsletter (non preoccuparti — non è collegato a nessun server, quindi attualmente non farà nulla).

```html live-sample___form-anatomy
<form action="./submit_page" method="get">
  <h2>Subscribe to our newsletter</h2>
  <p>
    <label for="name">Name (required):</label>
    <input type="text" name="name" id="name" required />
  </p>
  <p>
    <label for="email">Email (required):</label>
    <input type="email" name="email" id="email" required />
  </p>
  <p>
    <button>Sign me up!</button>
  </p>
</form>
```

Questo è reso come segue:

{{EmbedLiveSample("form-anatomy", "100%", "200")}}

Grazie al modo in cui funziona MDN, puoi inserire testo nei campi di input, ma non vedrai il form inviare correttamente quando premi il pulsante. Per seguire le prossime sezioni, copia il codice HTML sopra in un nuovo file HTML utilizzando il tuo [editor di codice](/it/docs/Learn_web_development/Getting_started/Environment_setup/Code_editors) e aprilo in un nuovo tab del browser.

### L'elemento `<form>`

Come abbiamo detto in precedenza, l'elemento {{htmlelement("form")}} funge da wrapper esterno per il form, raggruppando insieme tutti i controlli del form al suo interno. Quando il `<button>` viene premuto, tutti i dati rappresentati dai controlli del form verranno inviati al server. L'elemento `<form>` può accettare molti attributi, ma i due più importanti, che abbiamo incluso qui, sono i seguenti:

- `action`: Contiene un percorso alla pagina a cui vogliamo inviare i dati del form inviato per essere elaborati. Più avanti, dopo che avrai inviato il form, vedrai `/submit_page` incluso nell'URL. Riceverai anche una risposta di errore {{HTTPStatus("404")}} perché la pagina in realtà non esiste, ma va bene per ora.
- `method`: Specifica il [metodo](/it/docs/Web/HTTP/Reference/Methods) di trasmissione dei dati che vuoi usare per inviare i dati del form al server. Non preoccuparti troppo di questo per ora; il valore `get` fa in modo che i dati vengano inviati come parametri allegati alla fine dell'URL.

> [!CALLOUT]
>
> **Provalo**
>
> Vai al esempio nel tab separato, prova a inserire un nome come "Bob" e un indirizzo email come "bob@bob.com".
>
> I due attributi sopra causano l'invio dei dati del form in un URL lungo le seguenti linee:
>
> `/some/url/submit_page?name=Bob&email=bob%40bob.com`

#### Strutturare i form

Puoi includere qualsiasi elemento HTML desideri all'interno di un elemento `<form>` per strutturare gli elementi del form stessi e fornire contenitori su cui puntare con CSS per lo styling, ecc.

Nel nostro esempio, abbiamo incluso un [elemento di intestazione](/it/docs/Web/HTML/Reference/Elements/Heading_Elements) (`<h2>`) per descrivere lo scopo del form.

Abbiamo anche messo ogni coppia input/label e il pulsante di invio all'interno di un separato {{htmlelement("p")}}, in modo che ognuno appaia su una linea separata. Questi elementi sono tutti inline per impostazione predefinita, il che significa che se non avessimo fatto questo, sarebbero tutti sulla stessa riga.

Questo è un modello comune per strutturare i form. Alcune persone usano elementi `<p>` per separare i loro elementi di form, alcune usano {{htmlelement("div")}}, {{htmlelement("section")}}, o addirittura elementi {{htmlelement("li")}}. Non importa molto, purché gli elementi utilizzati abbiano senso semanticamente. Ad esempio, ha senso dividere i gruppi di elementi di form in paragrafi separati o sezioni di contenuto o anche elementi in una lista. Sarebbe meno sensato rappresentarli come [citazioni di blocco](/it/docs/Web/HTML/Reference/Elements/blockquote), [asides](/it/docs/Web/HTML/Reference/Elements/aside), o [indirizzi](/it/docs/Web/HTML/Reference/Elements/address).

Esiste un elemento specializzato per raggruppare gli elementi del form insieme chiamato {{htmlelement("fieldset")}}. Questo è utile in alcune circostanze, come nei form complessi, e quando si raggruppano insieme più checkbox e radio button. Esamineremo un paio di esempi di `<fieldset>` più avanti.

### Elementi `<input>`

Gli elementi {{htmlelement("input")}} rappresentano i diversi elementi di dati inseriti nel form. Studiamo uno degli esempi dal nostro form di base:

```html
<input type="text" name="name" id="name" required />
```

Gli attributi sono i seguenti:

- `type`: Specifica il tipo di controllo del form da creare. Ci sono molti tipi diversi di controlli del form, da campi di testo semplici di diversi tipi a radio button, checkbox e altro. Il tipo `text` rende un campo di testo di base che può accettare qualsiasi valore.
- `name`: Specifica un nome per l'elemento di dati. Quando il form viene inviato, i dati vengono inviati in coppie nome/valore. In ciascun caso, il nome è uguale a questo valore di attributo `name`, e il valore è uguale al testo inserito nel campo di testo.
- `id`: Specifica un ID che può essere usato per identificare l'elemento. In questo caso, viene usato per associare il controllo del form con il suo `<label>`.
- `required`: Specifica che deve essere inserito un valore nel form element prima che il form possa essere inviato. Questo dovrebbe essere impostato solo su input che richiedi, non su campi facoltativi.

Dovresti essere consapevole che alcuni tipi di input di solito non ottengono i loro valori dal testo inserito in un campo. Ad esempio, un [`<input type="color">`](/it/docs/Web/HTML/Reference/Elements/input/color) rende un widget di selezione del colore da cui scegliere un colore, mentre un [`<input type="radio">`](/it/docs/Web/HTML/Reference/Elements/input/radio) rende un controllo del radiobutton che può essere selezionato, o meno.

Nel caso dei radio button, generalmente devi fornire il valore che verrebbe inviato se è selezionato all'interno di un attributo `value` specifico. Nota che _puoi_ specificare un attributo `value` su tipi di input come `text` e `color` — l'effetto è che il valore è pre-compilato nel campo del form quando viene mostrato per la prima volta.

> [!CALLOUT]
>
> **Provalo**
>
> 1. Ancora una volta, vai all'esempio che hai caricato in un unico tab separato e prova a inviare il form senza inserire alcun valore in nessuno dei due campi. Vedrai apparire un messaggio di errore accanto al campo "Nome", che dice qualcosa come "Per favore compila questo campo" (varierà tra i diversi browser). Questo è l'attributo `required` — e la validazione del form lato client predefinita del browser — in azione.
> 2. Ora prova a inviare il form con un nome valido inserito nel primo campo, ma un valore che non è un indirizzo email valido nel secondo campo (qualcosa come "aaaa" andrà bene). Questa volta vedrai apparire un messaggio di errore accanto al campo "Email" che dice qualcosa come "Per favore inserisci un indirizzo email".
> 3. Per questo esercizio, sarà necessario modificare il codice del form. Puoi farlo aprendo il tuo esempio locale che hai creato nel tuo editor di testo, modificandolo lì e salvandolo. Prova a modificare il form per includere `value="Bob"` nel primo input. Quando ricarichi il codice, vedrai che nel primo campo è inserito di default un valore di "Bob".

#### Input dei campi di testo specializzati

Il secondo esercizio sopra solleva un punto interessante. Il secondo campo di input si aspetta specificamente un indirizzo email e convalida i valori inseriti come tali. Se guardi di nuovo il codice del form, vedrai perché — il secondo `<input>` ha un `type` di `email`. Ci sono diversi tipi di campi di testo specializzati progettati per gestire tipi specifici di dati — [`<input type="number">`](/it/docs/Web/HTML/Reference/Elements/input/number), [`<input type="password">`](/it/docs/Web/HTML/Reference/Elements/input/password), [`<input type="tel">`](/it/docs/Web/HTML/Reference/Elements/input/tel), ecc.

> [!CALLOUT]
>
> **Provalo**
>
> Segui alcuni dei link sopra per scoprire a cosa servono questi tipi di input. Dai un'occhiata alla nostra [referenza di `<input>`](/it/docs/Web/HTML/Reference/Elements/input) e vedi se riesci a trovare altri tipi di campi di testo specializzati.

### Elementi `<label>`

Come abbiamo detto sopra, gli elementi {{htmlelement("label")}} forniscono etichette identificative associate ai controlli del form che descrivono i dati che dovrebbero essere inseriti in essi. Puoi mettere qualsiasi contenuto di testo desideri negli elementi `<label>`, ma dovrebbero descrivere accuratamente quali dati il controllo del form associato si aspetta. L'associazione è creata assegnando al controllo del form un attributo `id`, quindi fornendo all'elemento `<label>` un attributo `for` con lo stesso valore dell'`id` del controllo.

Ad esempio:

```html
<label for="name">Name (required):</label>
<input type="text" name="name" id="name" required />
```

Gli elementi `<label>` sono importanti per diversi motivi, in particolare:

- Quando utenti non vedenti utilizzano un lettore di schermo per aiutarli a leggere e interagire con il contenuto delle pagine web, il lettore di schermo leggerà il testo dell'etichetta associata quando ciascun controllo viene incontrato. Questo rende più facile per gli utenti capire quali contenuti dovrebbero essere inseriti in ciascun controllo.
- Consentono di focalizzare gli elementi del form facendo clic sul loro testo dell'etichetta oltre che sui controlli. Questo è particolarmente utile per gli utenti di telefoni cellulari, dove può essere difficile selezionare accuratamente un elemento del form con il dito su uno schermo touch. Rendere la **area di clic** più grande è utile in tali circostanze.

#### Etichette di form esplicite e implicite

Lo stile delle etichette di form che hai visto sopra è chiamato **etichetta di form esplicita** — l'associazione tra controllo e etichetta è fatta esplicitamente tramite gli attributi `id` e `for`. Puoi anche implementare un'etichetta di form **implicita** annidando il controllo all'interno dell'etichetta, in questo modo:

```html
<label>
  Name (required):
  <input type="text" name="name" required />
</label>
```

L'annidamento crea un'associazione implicita tra controllo e etichetta, e non hai più bisogno degli attributi `id` e `for`.

Qualunque approccio va bene, ma consiglieremmo di utilizzare l'approccio di etichettatura esplicita. Questo perché l'associazione esplicita è di solito più facile da identificare e capire, specialmente quando il tuo codice HTML diventa più complesso. Inoltre, i lettori di schermo (e altre tecnologie assistive) a volte non gestiscono correttamente le etichette implicite.

Puoi leggere di più sulle migliori pratiche di etichettatura dei form in [HTML Inputs and Labels: A Love Story](https://css-tricks.com/html-inputs-and-labels-a-love-story/), csstricks.com (2021).

### L'elemento `<button>`

Quando un elemento {{htmlelement("button")}} è incluso all'interno di un elemento `<form>`, il suo comportamento predefinito è che invierà il form, a condizione che non ci siano dati non validi presenti che bloccano l'invio a causa della validazione del form lato client. Hai già visto questo comportamento quando hai giocato con il nostro esempio di form di base sopra.

Ci sono altri comportamenti dei pulsanti che possono essere specificati tramite l'attributo `type` dell'elemento `<button>`:

- `<button type="submit">` dichiara esplicitamente che un pulsante dovrebbe comportarsi come un pulsante di invio. Non è mai realmente necessario dichiararlo, a meno che per qualche motivo tu stia includendo altri pulsanti all'interno del tuo `<form>`, e vuoi chiarire quale sia il pulsante di invio. Questo sarà molto raro.
- `<button type="reset">` crea un _pulsante di reset_ — questo elimina immediatamente tutti i dati dal form, reimpostandolo nello stato iniziale. **Non utilizzare i pulsanti di reset** — erano popolari nei primi giorni del web, ma sono di solito più fastidiosi che utili. La maggior parte delle persone ha sperimentato la compilazione di un lungo form solo per cliccare accidentalmente il pulsante di reset invece del pulsante di invio, il che significa che devono ricominciare da capo.
- `<button type="button">` crea un pulsante con lo stesso comportamento dei pulsanti specificati al di fuori degli elementi `<form>`. Come abbiamo visto in precedenza, non fanno assolutamente nulla per impostazione predefinita, e è necessario JavaScript per dare loro funzionalità.

> [!NOTE]
> Puoi anche creare i tipi di pulsanti sopra usando un elemento `<input>` con gli stessi valori di `type` specificati — [`<input type="submit">`](/it/docs/Web/HTML/Reference/Elements/input/submit), [`<input type="reset">`](/it/docs/Web/HTML/Reference/Elements/input/reset), e [`<input type="button">`](/it/docs/Web/HTML/Reference/Elements/input/button). Tuttavia, questi hanno molti svantaggi rispetto ai loro equivalenti `<button>`. Dovresti usare `<button>` invece.

## Una nota sull'accessibilità

Abbiamo già parlato dell'importanza delle etichette dei form per l'accessibilità, ma vogliamo includere anche qualche commento sull'importanza generale di utilizzare gli elementi semantici corretti per creare form (ad esempio, usa un `<button>` per inviare il tuo form, e non un `<div>` programmato per comportarsi come un `<button>`). È perfettamente possibile usare una combinazione di CSS e JavaScript per far apparire e comportarsi praticamente qualsiasi elemento HTML come un elemento di form. Gli sviluppatori di solito fanno questo per motivi di design — alcuni controlli del form sono difficili da stilare.

Tuttavia, quando fai questo, rendi la vita più difficile a te stesso e ai tuoi utenti. Il browser fornisce diverse funzionalità per i `<button>` e i controlli di form per impostazione predefinita, senza bisogno di JavaScript o altro codice extra, per rendere i form più utilizzabili per tutti gli utenti.

Ad esempio:

- Gli elementi semantici sono compresi dalla tecnologia assistiva come i lettori di schermo, che comunicano il loro significato agli utenti che non possono vederli.
- I controlli di form e i pulsanti sono accessibili da tastiera per impostazione predefinita. Nell'esempio precedente, provare a spostarsi avanti e indietro tra gli elementi del form usando <kbd>Tab</kbd> e <kbd>Shift</kbd> + <kbd>Tab</kbd> (chiamato "tabbing").
- Nota anche come il tabbing tra gli elementi del form causa l'evidenziazione dell'elemento focalizzato con un contorno blu (chiamato **contorno di focus**). Questa è una funzionalità importante per gli utenti da tastiera per sapere dove si trovano attualmente nel form.

Se non utilizzi gli elementi semantici corretti per implementare i tuoi form, dovrai reimplementare tutte queste funzionalità, altrimenti i tuoi elementi del form non si comporteranno come gli utenti si aspettano, e quindi sembreranno rotti. Si somma tutto.

## Altri tipi di controllo

Ci sono molti altri tipi di controllo che puoi usare per raccogliere dati in un form. Vediamo un esempio leggermente più complesso, e poi lo esploriamo e lo spieghiamo.

> [!NOTE]
> In questo esempio, si suppone che l'utente sia già registrato e abbia effettuato l'accesso, quindi non è necessario raccogliere dettagli come nome e email.

```html live-sample___form-other-controls
<form action="./payment_page" method="get">
  <h2>Register for the meetup</h2>
  <fieldset>
    <legend>Choose hotel room type (required):</legend>
    <div>
      <input
        type="radio"
        id="hotelChoice1"
        name="hotel"
        value="economy"
        checked />
      <label for="hotelChoice1">Economy (+$0)</label>

      <input type="radio" id="hotelChoice2" name="hotel" value="superior" />
      <label for="hotelChoice2">Superior (+$50)</label>

      <input
        type="radio"
        id="hotelChoice3"
        name="hotel"
        value="penthouse"
        disabled />
      <label for="hotelChoice3">Penthouse (+$150)</label>
    </div>
  </fieldset>
  <fieldset>
    <legend>Choose classes to attend:</legend>
    <div>
      <input type="checkbox" id="yoga" name="yoga" />
      <label for="yoga">Yoga (+$10)</label>

      <input type="checkbox" id="coffee" name="coffee" />
      <label for="coffee">Coffee roasting (+$20)</label>

      <input type="checkbox" id="balloon" name="balloon" />
      <label for="balloon">Balloon animal art (+$5)</label>
    </div>
  </fieldset>
  <p>
    <label for="transport">How are you getting here:</label>
    <select name="transport" id="transport">
      <option value="">--Please choose an option--</option>
      <option value="plane">Plane</option>
      <option value="bike">Bike</option>
      <option value="walk">Walk</option>
      <option value="bus">Bus</option>
      <option value="train">Train</option>
      <option value="jetPack">Jet pack</option>
    </select>
  </p>
  <p>
    <label for="comments">Any other comments:</label>
    <textarea id="comments" name="comments" rows="5" cols="33"></textarea>
  </p>
  <p>
    <button>Continue to payment</button>
  </p>
</form>
```

Questo è reso come segue:

{{EmbedLiveSample("form-other-controls", "100%", "500")}}

Ti consigliamo di aprire questo esempio in un tab separato del browser mentre lavori alle prossime sezioni, in cui esamineremo ciascun tipo di controllo a turno. Per fare questo, copia il codice in un file HTML usando il tuo editor di codice e aprilo in un tab del browser.

> [!CALLOUT]
>
> **Provalo**
>
> Prima di procedere, gioca con i diversi controlli del form, seleziona alcuni valori e prova a inviare il form.

### Radio button

I pulsanti "Scegli il tipo di camera dell'hotel" sono implementati usando i controlli [`<input type="radio">`](/it/docs/Web/HTML/Reference/Elements/input/radio). Questi vengono resi come un insieme di controlli a pulsante premibili in cui solo uno dell'insieme può essere selezionato in un dato momento — non puoi selezionare più di uno contemporaneamente. Sono chiamati così per i pulsanti presenti sulle radio vecchio stile, dove premi un pulsante e quello precedentemente selezionato si solleva di nuovo.

Il nostro codice di esempio sembra così:

```html
<fieldset>
  <legend>Choose hotel room type (required):</legend>
  <div>
    <input
      type="radio"
      id="hotelChoice1"
      name="hotel"
      value="economy"
      checked />
    <label for="hotelChoice1">Economy (+$0)</label>

    <input type="radio" id="hotelChoice2" name="hotel" value="superior" />
    <label for="hotelChoice2">Superior (+$50)</label>

    <input
      type="radio"
      id="hotelChoice3"
      name="hotel"
      value="penthouse"
      disabled />
    <label for="hotelChoice3">Penthouse (+$150)</label>
  </div>
</fieldset>
```

I tipi di input `radio` funzionano per lo più allo stesso modo dei tipi di input `text`, ma con alcune differenze:

- Gli attributi `name` per ciascun insieme di radio button devono contenere lo stesso valore, per associarli insieme come un unico set. Se contengono valori diversi, saranno effettivamente set separati, con valori diversi all'invio.
- Devi includere un attributo `value` contenente il valore da inviare per ciascun radio button. Il valore inviato sarà una coppia nome/valore, ma il nome sarà sempre lo stesso, ad esempio `hotel=economy` o `hotel=superior`.
- L'etichetta `<label>` per ciascun radio button deve descrivere quella particolare scelta di valore, piuttosto che il valore complessivo che stai selezionando. Il modo preferito per fornire una descrizione della scelta di valore complessiva è racchiuderli in un elemento {{htmlelement("fieldset")}}, che accetta un elemento {{htmlelement("legend")}} come figlio che contiene la descrizione.

> [!NOTE]
> Oltre a strutturare e etichettare i form, i fieldset hanno altri usi, come [disabilitare](#disabilitare_i_controlli_del_form) un intero set di controlli come un'unità singola.

Vale anche la pena notare che abbiamo applicato l'attributo `checked` al primo radio button — questo fa sì che sia selezionato quando la pagina viene caricata per la prima volta. Questo è il modo in cui giustifichiamo il contrassegnare il valore del tipo di camera dell'hotel come "required" — un'opzione sarà sempre selezionata e non puoi deselezionare un radio button senza selezionare un altro.

> [!CALLOUT]
>
> **Provalo**
>
> Prova a rimuovere l'attributo `checked` dal primo radio button, salva, poi ricarica, per vedere l'effetto che ha. Rimettilo prima di procedere.

#### Disabilitare i controlli del form

Nell'esempio del radio button, noterai che il terzo radio button ha impostato l'attributo `disabled`. Questo fa sì che il controllo renderizzato sia grigiato e non selezionabile. Questo è utile in molte situazioni in cui un'opzione è normalmente disponibile, ma non in questo momento. Ad esempio, un prodotto potrebbe essere esaurito, o come nel caso del nostro esempio, le suite attico sono tutte prenotate!

Puoi impostare l'attributo `disabled` su qualsiasi controllo del form, compreso gli elementi `<button>`. Gli elementi `<fieldset>` possono anche accettare l'attributo `disabled` — questo fa sì che ogni controllo all'interno del fieldset sia disabilitato.

> [!CALLOUT]
>
> **Provalo**
>
> Prova a impostare l'attributo `disabled` sui due elementi `<fieldset>`, salva, poi ricarica, per vedere l'effetto che ha. Rimuovili di nuovo prima di procedere.

### Checkbox

I nostri selettori "classi da frequentare" sono implementati usando i controlli [`<input type="checkbox">`](/it/docs/Web/HTML/Reference/Elements/input/checkbox). Questi vengono resi come un insieme di checkbox di stato acceso/spento. A differenza dei radio button, puoi selezionare più di uno contemporaneamente.

```html
<fieldset>
  <legend>Choose classes to attend:</legend>
  <div>
    <input type="checkbox" id="yoga" name="yoga" />
    <label for="yoga">Yoga (+$10)</label>

    <input type="checkbox" id="coffee" name="coffee" />
    <label for="coffee">Coffee roasting (+$20)</label>

    <input type="checkbox" id="balloon" name="balloon" />
    <label for="balloon">Balloon animal art (+$5)</label>
  </div>
</fieldset>
```

Come puoi vedere dagli spezzoni di codice, i radio button e i checkbox sono implementati in modo molto simile (possono anche prendere attributi `checked` per renderli preselezionati quando la pagina viene caricata). Si comportano anche in modo abbastanza simile, tranne per il fatto che i radio button ti permettono di scegliere zero o uno elementi su molti, e i checkbox ti permettono di scegliere zero o più elementi su molti.

La principale differenza (a parte il valore di `type`!) è che ciascun checkbox ha un diverso valore `name`, e generalmente non vengono dati attributi `value`. Comportativamente, questo significa che rappresentano diversi valori di dati, mentre un set di radio button rappresenta solo uno. All'invio, ciascun valore viene inviato con un valore di `on` se il checkbox era selezionato — `yoga=on`, `balloon=on`, ecc.

> [!NOTE]
> È possibile cambiare il valore inviato per un checkbox assegnandogli un attributo `value`, ad esempio: `<input type="checkbox" id="yoga" name="yoga" value="yes" />` risulterebbe in `yoga=yes` inviato se selezionato. Tuttavia, non c'è molto scopo nel farlo. Un checkbox viene inviato con un singolo valore se selezionato, oppure non viene inviato affatto. Non importa realmente quale valore venga inviato al server.

### Menu a discesa

I menu a discesa, ad esempio il controllo di selezione "Come stai arrivando qui" nel nostro esempio, sono implementati non con un tipo `<input>`, ma con gli elementi {{htmlelement("select")}} e {{htmlelement("option")}}:

```html
<label for="transport">How are you getting here:</label>
<select name="transport" id="transport">
  <option value="">--Please choose an option--</option>
  <option value="plane">Plane</option>
  <option value="bike">Bike</option>
  <option value="walk">Walk</option>
  <option value="bus">Bus</option>
  <option value="train">Train</option>
  <option value="jetPack">Jet pack</option>
</select>
```

L'elemento `<select>` racchiude tutte le diverse scelte di valore. È dove imposti l'attributo `id` che associa il controllo alla sua etichetta, e l'attributo `name` che imposta il nome dell'elemento di dati da inviare.

Ciascun valore possibile per l'elemento di dati è rappresentato da un elemento `<option>`, annidato all'interno dell'elemento `<select>`. Ogni elemento `<option>` può prendere un attributo `value`, che specifica il valore da inviare se quell'opzione è scelta dall'elenco a discesa. Se non specifichi un `value`, il testo all'interno dei tag `<option></option>` è usato come valore.

> [!NOTE]
> Se vuoi avere una specifica opzione selezionata al momento del caricamento della pagina, puoi aggiungere un attributo `selected` al relativo elemento `<option>`.

### Campi di input di testo multilinea

I campi di input di testo multilinea sono creati usando elementi {{htmlelement("textarea")}}:

```html
<label for="comments">Any other comments:</label>
<textarea id="comments" name="comments" rows="5" cols="33"></textarea>
```

Si comportano nello stesso modo degli elementi `<input type="text">`, tranne per il fatto che consentono di inserire più righe di testo. L'attributo `rows` specifica il numero di righe di altezza che l'area di testo avrà per impostazione predefinita, mentre l'attributo `cols` specifica il numero di colonne di larghezza che l'area di testo avrà per impostazione predefinita. Se non sono specificati, i valori utilizzati sono `cols="20"` e `rows="2"`.

> [!CALLOUT]
>
> **Provalo**
>
> La maggior parte dei browser rende le aree di testo con una maniglia di trascinamento in un angolo, che può essere usata per ridimensionarla. Prova a usare questa per ridimensionare l'area di testo nel nostro demo.

## Validazione dei form

In precedenza, abbiamo esaminato alcune delle validazioni di base dei form lato client fornite dal browser. L'attributo `required` è usato per specificare che un campo deve essere compilato prima che il form possa essere inviato; verifica anche che il tipo di valore corretto sia inserito per tipi di valore specifici come indirizzi email, URL, numeri, ecc. La validazione è importante per due main motivi:

- Assicurarsi che i dati siano inviati nel formato corretto in modo che non causino errori nella tua applicazione.
- Assicurarsi che i dati non causino problemi di sicurezza. Le persone malintenzionate sanno come inviare dati formattati specificamente in modo che, su applicazioni non sicure, possano eseguire comandi per eliminare database o prendere controllo di un sistema.

La validazione dei form è un argomento enorme che va oltre l'ambito di questo articolo, quindi lo lasceremo qui per ora. Tieni presente solo che ci sono due tipi di validazione dei form:

- Validazione lato client, che avviene nel browser, implementata utilizzando una combinazione di attributi di validazione dei form (come `required`) e JavaScript. La validazione lato client è utile per dare agli utenti suggerimenti istantanei quando hanno inserito dati errati, ma non è così efficace nel fermare i dati dannosi. È troppo facile disattivare JavaScript o alterare il codice lato client in modo che la validazione non funzioni più.
- Validazione lato server, che avviene sul server, implementata utilizzando qualsiasi linguaggio il server stia utilizzando. Messaggi mal formati possono essere inviati a un server per errore o di proposito. La saggezza convenzionale è di assicurarsi che il tuo server non si fidi mai di ciò che un client sta inviando per evitare bug o problemi di sicurezza causati da messaggi mal formati. La validazione lato server è ottima per fermare i messaggi dannosi, poiché è più difficile manomettere il codice in esecuzione sul server. La validazione lato server non è così buona nel fornire suggerimenti agli utenti about dati errati perché i dati devono andare al server per essere convalidati, quindi il risultato deve essere inviato di nuovo al client prima che l'utente possa essere notificato.

In breve, non decidere tra l'uso della validazione lato client o lato server - avrai bisogno di entrambi. Hai bisogno della validazione lato client per fornire feedback agli utenti sul loro input e della validazione lato server per assicurarti che i messaggi siano in un formato che il server possa gestire in modo sicuro. Se vuoi iniziare a saperne di più sulla validazione, un buon punto di partenza è [Validazione dei form lato client](/it/docs/Learn_web_development/Extensions/Forms/Form_validation).

## Riepilogo

Questo è tutto per ora. C'è molto altro da sapere sui form, ma per ora, ti abbiamo dato abbastanza conoscenza per andare avanti nei tuoi studi.

Nel prossimo capitolo, esamineremo come eseguire il debug dei problemi nel tuo codice HTML.

## Vedi anche

- [Moduli web — Lavorare con i dati degli utenti](/it/docs/Learn_web_development/Extensions/Forms)

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Planet_data_table", "Learn_web_development/Core/Structuring_content/Debugging_HTML", "Learn_web_development/Core/Structuring_content")}}
