---
title: Moduli e pulsanti in HTML
short-title: Moduli e pulsanti
slug: Learn_web_development/Core/Structuring_content/HTML_forms
l10n:
  sourceCommit: 7d93b0f639e37e9340ed707e3cb7f9a75c1b3048
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Planet_data_table", "Learn_web_development/Core/Structuring_content/Test_your_skills/Forms_and_buttons", "Learn_web_development/Core/Structuring_content")}}

I moduli HTML e i pulsanti sono strumenti potenti per interagire con gli utenti di un sito web. Nella maggior parte dei casi, forniscono agli utenti controlli per manipolare un'interfaccia utente (UI) o inserire dati quando necessario.

In questo articolo viene fornita un'introduzione alle basi dei moduli e dei pulsanti. C'è molto altro da sapere — molti tipi di input e funzionalità dei moduli non vengono menzionati — ma questo articolo fornirà una solida base per la maggior parte dei casi. È possibile apprendere gli utilizzi avanzati o specializzati secondo necessità, nell'ambito dell'apprendimento continuo che accompagnerà l'intera carriera.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con HTML, come illustrato in
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi HTML di base</a
        >. Semantica a livello di testo, come <a href="/it/docs/Learn_web_development/Core/Structuring_content/Headings_and_paragraphs"
          >intestazioni e paragrafi</a
        > e <a href="/it/docs/Learn_web_development/Core/Structuring_content/Lists"
          >elenchi</a
        >. <a href="/it/docs/Learn_web_development/Core/Structuring_content/Structuring_documents"
          >HTML strutturale</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere che i moduli e i pulsanti sono gli strumenti principali, insieme ai link, con cui gli utenti interagiscono con un sito web.</li>
          <li>Diversi tipi di pulsante.</li>
          <li>Tipi <code>&lt;input&gt;</code> comuni.</li>
          <li>Attributi comuni quali <code>name</code> e <code>value</code>.</li>
          <li>L'elemento <code>&lt;form&gt;</code> e le basi dell'invio dei moduli.</li>
          <li>Rendere accessibili i moduli con etichette e semantica corretta.</li>
          <li>Altri tipi di controllo: <code>&lt;textarea&gt;</code>, <code>&lt;select&gt;</code> e <code>&lt;option&gt;</code>.</li>
          <li>Basi della validazione lato client.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Interagire con gli utenti

Finora, nel corso sono stati illustrati alcuni modi in cui gli utenti possono interagire con il web:

- I [link](/it/docs/Learn_web_development/Core/Structuring_content/Creating_links) possono essere utilizzati per navigare verso diverse sezioni di contenuto, nella stessa pagina oppure in una pagina diversa.
- Gli elementi [`<video>` e `<audio>`](/it/docs/Learn_web_development/Core/Structuring_content/HTML_video_and_audio) generalmente includono controlli quali riproduzione/pausa, avanzamento rapido, riavvolgimento e così via, che consentono agli utenti di fruire dei contenuti multimediali come desiderano.

Tuttavia, queste funzionalità tendono a facilitare interazioni a senso unico, in cui gli utenti consumano passivamente i contenuti. Va bene così, ma il web è un'esperienza a doppio senso. Gli utenti di un sito web impostano preferenze su come desiderano fruire di contenuti e servizi. Ordinano taxi e richiedono di essere richiamati. Forniscono feedback e presentano reclami. Acquistano prodotti e li ricevono a domicilio.

Per offrire questa esperienza a doppio senso, è necessario utilizzare pulsanti e moduli.

I pulsanti vengono solitamente creati usando gli elementi HTML {{htmlelement("button")}} (talvolta vengono creati anche usando elementi {{htmlelement("input")}} con i rispettivi attributi `type` impostati su un valore come `button` o `submit`). Questi pulsanti sono di uso generale — è possibile collegarli per attivare qualsiasi funzionalità desiderata, limitati soltanto dall'immaginazione e dalle competenze di programmazione.

I moduli vengono creati utilizzando elementi quali {{htmlelement("form")}}, {{htmlelement("label")}}, {{htmlelement("input")}} e {{htmlelement("select")}}. Gli elementi dei moduli possono essere utilizzati per creare controlli più complessi di quelli consentiti dai semplici pulsanti — per esempio, un menu a discesa contenente più opzioni che permettono di scegliere tra diversi temi per un elemento dell'interfaccia utente.

Tuttavia, soprattutto, possono essere utilizzati anche per creare moduli che gli utenti compilano quando devono inviare informazioni a un server web. Si pensi ai siti di e-commerce — quando si desidera cercare un prodotto da acquistare, si utilizza un modulo per inserire i termini di ricerca. Quando si desidera pagare alcuni articoli e finalizzare la consegna, si utilizza un modulo per inserire l'indirizzo postale e un altro modulo per inserire i dati della carta di credito.

In questo articolo ci concentreremo principalmente su questo utilizzo più tradizionale degli elementi dei moduli. Si noti che i pulsanti vengono comunemente utilizzati anche all'interno dei moduli per inviare al server i dati inseriti.

Dopo questa importante teoria, è il momento di esplorare il codice e vedere come vengono implementati pulsanti e moduli.

## Pulsanti

Come accennato sopra, i pulsanti hanno alcuni utilizzi principali sul web. Prima di tutto, vengono utilizzati per attivare funzionalità, il che è utile durante la creazione di controlli dell'interfaccia utente. Il pulsante più semplice viene implementato usando il codice seguente:

```html live-sample___basic-button
<button>Press me</button>
```

Il risultato visualizzato è il seguente:

{{EmbedLiveSample("basic-button", "100%", "60")}}

Il testo visualizzato tra i tag `<button></button>` viene renderizzato all'interno del pulsante e il browser gli applica uno stile di base, quindi per impostazione predefinita avrà l'aspetto e il comportamento di un pulsante. Fin qui tutto bene. Tuttavia, c'è un problema: il pulsante da solo non farà nulla di utile. Per fare in modo che svolga qualcosa di utile, occorre inserirlo in un modulo (che verrà trattato più avanti) oppure aggiungere del JavaScript.

Per esempio, applicando il seguente JavaScript al pulsante precedente:

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

si otterrebbe il seguente risultato — provare a fare clic sul pulsante:

{{EmbedLiveSample("basic-button-with-js", "100%", "60")}}

Per ora non è necessario comprendere come funziona il JavaScript. Se ne saprà di più più avanti nel corso.

Nella sezione successiva verrà mostrata una dimostrazione del secondo utilizzo principale dei pulsanti: l'invio dei moduli.

## L'anatomia di un modulo

Un modulo di base contiene tre elementi:

- Un elemento {{htmlelement("form")}}, che racchiude tutti gli altri contenuti del modulo. Tutti i controlli del modulo all'interno dei tag `<form></form>` fanno parte dello stesso modulo e i relativi dati vengono inclusi quando il modulo viene inviato.
- Una o più coppie, ciascuna composta da un elemento {{htmlelement("label")}} e un elemento di controllo del modulo (solitamente un elemento {{htmlelement("input")}}, ma esistono anche altri tipi, per esempio {{htmlelement("select")}}):
  - L'elemento di controllo del modulo consente all'utente di scegliere o inserire dati, che verranno inviati al server quando il modulo viene inviato.
  - L'elemento `<label>` fornisce un'etichetta identificativa associata al controllo del modulo, che descrive i dati da inserire.
- Un elemento {{htmlelement("button")}}, utilizzato per inviare il modulo.

Vediamo un esempio di base che include i tre elementi precedenti. Questo modulo potrebbe essere utilizzato per richiedere il nome e l'email di un utente, per iscriverlo a una newsletter (nessuna preoccupazione — non è collegato a nessun server, quindi al momento non farà nulla).

```html live-sample___form-anatomy
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>First form</title>
  </head>
  <body>
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
  </body>
</html>
```

Il risultato visualizzato è il seguente:

{{EmbedLiveSample("form-anatomy", "100%", "200", , , , , "allow-forms")}}

Facendo immediatamente clic su "Sign me up!", verrà visualizzato un errore di validazione perché non sono stati inseriti dati. Se si compilano i campi con un nome e un indirizzo email, quindi si fa clic su "Sign me up!", verrà visualizzato un messaggio di errore `404`.

Il motivo verrà spiegato più avanti. Prima di proseguire, copiare il precedente elenco di codice HTML in un nuovo file HTML utilizzando il proprio [editor di codice](/it/docs/Learn_web_development/Getting_started/Environment_setup/Code_editors) e aprirlo in una nuova scheda del browser.

### L'elemento `<form>`

Come già detto, l'elemento {{htmlelement("form")}} funge da contenitore esterno per il modulo, raggruppando tutti i controlli del modulo al suo interno. Quando viene premuto `<button>`, tutti i dati rappresentati dai controlli del modulo verranno inviati al server. L'elemento `<form>` può accettare molti attributi, ma i due più importanti, inclusi nel nostro esempio, sono i seguenti:

- `action`: contiene un percorso alla pagina a cui si desidera inviare i dati del modulo inviato affinché vengano elaborati. Più avanti, dopo l'invio del modulo, verrà visualizzato `/submit_page` nell'URL. Verrà inoltre ricevuta una risposta di errore {{HTTPStatus("404")}} perché la pagina non esiste realmente, ma per ora va bene così.
- `method`: specifica il [metodo](/it/docs/Web/HTTP/Reference/Methods) di trasmissione dei dati da utilizzare per inviare i dati del modulo al server. Per ora non occorre preoccuparsi troppo di questo; il valore `get` fa sì che i dati vengano inviati come parametri aggiunti alla fine dell'URL.

#### Verificare i dati inviati

1. Andare all'esempio nella scheda separata e provare a inserire il nome "Bob" e l'indirizzo email "bob@bob.com".
2. Premere `<button>`.

Gli attributi `action` e `method` fanno sì che i dati del modulo vengano inviati in un URL simile al seguente:

```plain
/some/url/submit_page?name=Bob&email=bob%40bob.com
```

#### Strutturare i moduli

All'interno di un elemento `<form>` è possibile includere qualsiasi elemento HTML per strutturare gli elementi del modulo e fornire contenitori a cui applicare CSS per lo stile e così via.

Nel nostro esempio è stato incluso un [elemento di intestazione](/it/docs/Web/HTML/Reference/Elements/Heading_Elements) (`<h2>`) per descrivere lo scopo del modulo.

Ogni coppia input/label e il pulsante di invio sono stati inoltre inseriti all'interno di un {{htmlelement("p")}} separato, in modo che ciascuno appaia su una riga distinta. Questi elementi sono tutti inline per impostazione predefinita, il che significa che, se non fosse stato fatto, sarebbero tutti sulla stessa riga.

Questo è un modello comune per strutturare i moduli. Alcune persone utilizzano elementi `<p>` per separare gli elementi del modulo, altre utilizzano elementi {{htmlelement("div")}}, {{htmlelement("section")}} o persino {{htmlelement("li")}}. Non è particolarmente importante, purché gli elementi utilizzati abbiano senso dal punto di vista semantico. Per esempio, ha senso dividere gruppi di elementi del modulo in paragrafi o sezioni di contenuto distinti, oppure persino in elementi di un elenco. Avrebbe meno senso rappresentarli come [citazioni in blocco](/it/docs/Web/HTML/Reference/Elements/blockquote), [contenuti complementari](/it/docs/Web/HTML/Reference/Elements/aside) o [indirizzi](/it/docs/Web/HTML/Reference/Elements/address).

Esiste un elemento specializzato per raggruppare gli elementi dei moduli, chiamato {{htmlelement("fieldset")}}. È utile in determinate circostanze, come nei moduli complessi e quando si raggruppano più checkbox e radio button. Più avanti verranno esaminati alcuni esempi di `<fieldset>`.

### Elementi `<input>`

Gli elementi {{htmlelement("input")}} rappresentano i diversi dati inseriti nel modulo. Studiamo uno degli esempi del nostro modulo di base:

```html
<input type="text" name="name" id="name" required />
```

Gli attributi sono i seguenti:

- `type`: specifica il tipo di controllo del modulo da creare. Esistono molti tipi diversi di controlli del modulo, dai semplici campi di testo di vario tipo ai radio button, alle checkbox e altro ancora. Il tipo `text` renderizza un campo di testo di base che può accettare qualsiasi valore.
- `name`: specifica un nome per il dato. Quando il modulo viene inviato, i dati vengono trasmessi in coppie nome/valore. In ogni caso, il nome è uguale al valore di questo attributo `name`, mentre il valore è uguale al testo inserito nel campo di testo.
- `id`: specifica un ID che può essere utilizzato per identificare l'elemento. In questo caso viene utilizzato per associare il controllo del modulo al relativo `<label>`.
- `required`: specifica che è necessario inserire un valore nell'elemento del modulo prima che il modulo possa essere inviato. Dovrebbe essere impostato solo sugli input obbligatori, non sui campi facoltativi.

Occorre sapere che alcuni tipi di input solitamente non ottengono i valori dal testo inserito in un campo. Per esempio, [`<input type="color">`](/it/docs/Web/HTML/Reference/Elements/input/color) renderizza un widget di selezione del colore da cui scegliere un colore, mentre [`<input type="radio">`](/it/docs/Web/HTML/Reference/Elements/input/radio) renderizza un controllo radio button che può essere selezionato oppure no.

Nel caso dei radio button, generalmente è necessario fornire il valore che verrebbe inviato se fosse selezionato all'interno di un attributo `value` specifico. Si noti che è possibile specificare un attributo `value` su tipi di input come `text` e `color` — l'effetto è che il valore viene precompilato nel campo del modulo quando viene renderizzato per la prima volta.

#### Attributi `required` e `value` in azione

1. Tornare all'esempio caricato in una scheda separata e provare a inviare il modulo senza inserire un valore in nessuno dei campi. Verrà visualizzato un messaggio di errore accanto al campo "Name" con un testo simile a "Please fill in this field" (varierà tra browser diversi). Questo è l'attributo `required` — e la validazione predefinita dei moduli lato client del browser — in azione.
2. Ora provare a inviare il modulo con un nome valido nel primo campo, ma un valore che non sia un indirizzo email valido nel secondo campo (qualcosa come "aaaa" andrà bene). Questa volta verrà visualizzato un messaggio di errore accanto al campo "Email" con un testo simile a "Please enter an email address".
3. Provare a modificare il modulo per includere `value="Bob"` nel primo input. Ricaricando il codice, si vedrà che il primo campo contiene per impostazione predefinita il valore "Bob".

#### Input specializzati per campi di testo

Il secondo esercizio precedente solleva un punto interessante. Il secondo campo di input si aspetta specificamente un indirizzo email e convalida i valori inseriti come tali. Osservando nuovamente il codice del modulo, è possibile capire perché: il secondo `<input>` ha un `type` pari a `email`.

Esistono diversi tipi di input specializzati per campi di testo progettati per gestire tipi specifici di dati, come [`<input type="number">`](/it/docs/Web/HTML/Reference/Elements/input/number), [`<input type="password">`](/it/docs/Web/HTML/Reference/Elements/input/password), [`<input type="tel">`](/it/docs/Web/HTML/Reference/Elements/input/tel), [`<input type="url">`](/it/docs/Web/HTML/Reference/Elements/input/url) e così via.

Seguire alcuni dei link precedenti per scoprire a cosa servono questi tipi di input. Consultare il riferimento di [`<input>`](/it/docs/Web/HTML/Reference/Elements/input) e vedere se è possibile trovare altri tipi di input specializzati per campi di testo.

### Elementi `<label>`

Come detto sopra, gli elementi {{htmlelement("label")}} forniscono etichette identificative associate ai controlli dei moduli che descrivono i dati da inserire. Negli elementi `<label>` è possibile inserire qualsiasi contenuto testuale, ma dovrebbe descrivere accuratamente quali dati si aspetta il controllo del modulo associato. L'associazione viene creata assegnando al controllo del modulo un attributo `id`, quindi assegnando all'elemento `<label>` un attributo `for` con lo stesso valore dell'`id` del controllo.

Per esempio:

```html
<label for="name">Name (required):</label>
<input type="text" name="name" id="name" required />
```

Gli elementi `<label>` sono importanti per diversi motivi, in particolare perché:

- Quando gli utenti ipovedenti utilizzano uno screen reader per aiutarli a leggere e interagire con il contenuto della pagina web, lo screen reader leggerà il testo dell'etichetta associata quando incontra ciascun controllo. Questo rende più facile per gli utenti comprendere quali contenuti devono essere inseriti in ciascun controllo.
- Consentono di mettere a fuoco gli elementi del modulo facendo clic sul testo dell'etichetta, oltre che sui controlli stessi. Questo è particolarmente utile per gli utenti di telefoni cellulari, per i quali può essere difficile selezionare accuratamente un elemento del modulo con il dito su uno schermo touch. Rendere più ampia l'**area di attivazione** è utile in tali circostanze.

#### Etichette di modulo esplicite e implicite

Lo stile di etichetta del modulo visto sopra è chiamato **etichetta di modulo esplicita** — l'associazione tra controllo ed etichetta viene creata esplicitamente tramite gli attributi `id` e `for`. È inoltre possibile implementare un'**etichetta di modulo implicita** annidando il controllo all'interno dell'etichetta, in questo modo:

```html
<label>
  Name (required):
  <input type="text" name="name" required />
</label>
```

L'annidamento crea un'associazione implicita tra controllo ed etichetta e gli attributi `id` e `for` non sono più necessari.

Entrambi gli approcci sono validi, ma è consigliabile utilizzare l'approccio di etichettatura esplicita. Questo perché l'associazione esplicita è solitamente più facile da identificare e comprendere, soprattutto quando il codice HTML diventa più complesso. Inoltre, gli screen reader (e altre tecnologie assistive) non sempre gestiscono correttamente le etichette implicite.

Per ulteriori informazioni sulle migliori pratiche per le etichette dei moduli, vedere [HTML Inputs and Labels: A Love Story](https://css-tricks.com/html-inputs-and-labels-a-love-story/), css-tricks.com (2021).

### L'elemento `<button>`

Quando un elemento {{htmlelement("button")}} è incluso all'interno di un elemento `<form>`, il suo comportamento predefinito è inviare il modulo, a condizione che non siano presenti dati non validi che causino il blocco dell'invio da parte della validazione dei moduli lato client. Questo comportamento è già stato osservato sperimentando con l'esempio di modulo di base precedente.

Altri comportamenti dei pulsanti possono essere specificati tramite l'attributo `type` dell'elemento `<button>`:

- `<button type="submit">` dichiara esplicitamente che un pulsante deve comportarsi come un pulsante di invio. Non è realmente necessario dichiararlo, a meno che, per qualche ragione, non si includano altri pulsanti all'interno di `<form>` e si voglia rendere chiaro quale sia il pulsante di invio. Questo accadrà molto raramente.
- `<button type="reset">` crea un _pulsante di ripristino_ — elimina immediatamente tutti i dati dal modulo, riportandolo allo stato iniziale. **Non usare pulsanti di ripristino** — erano popolari nei primi tempi del web, ma di solito sono più fastidiosi che utili. Molte persone hanno compilato un lungo modulo per poi fare accidentalmente clic sul pulsante di ripristino invece che su quello di invio, dovendo quindi ricominciare da capo.
- `<button type="button">` crea un pulsante con lo stesso comportamento dei pulsanti specificati al di fuori degli elementi `<form>`. Come visto in precedenza, per impostazione predefinita non fanno assolutamente nulla e occorre JavaScript per fornire loro funzionalità.

Sebbene sia possibile creare questi tipi di pulsante utilizzando un elemento `<input>` con gli stessi valori `type` — come [`<input type="submit">`](/it/docs/Web/HTML/Reference/Elements/input/submit), [`<input type="reset">`](/it/docs/Web/HTML/Reference/Elements/input/reset) e [`<input type="button">`](/it/docs/Web/HTML/Reference/Elements/input/button) — presentano molti svantaggi rispetto alle rispettive controparti `<button>`. È quindi preferibile utilizzare `<button>`.

> [!NOTE]
> Scrimba<sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> offre una lezione gratuita — [The very basics of forms](https://scrimba.com/learn-responsive-web-design-c029/~031?via=mdn) — che fornisce un utile riepilogo interattivo delle basi dei moduli trattate precedentemente in questo articolo.

## Una nota sull'accessibilità

È già stata discussa l'importanza delle etichette dei moduli per l'accessibilità, ma è utile aggiungere alcune osservazioni sull'importanza generale di utilizzare gli elementi semantici corretti per creare moduli (per esempio, utilizzare un `<button>` per inviare un modulo e non un `<div>` programmato per comportarsi come un `<button>`). È perfettamente possibile utilizzare una combinazione di CSS e JavaScript per fare in modo che praticamente qualsiasi elemento HTML abbia l'aspetto e il comportamento di un elemento di modulo. Gli sviluppatori solitamente lo fanno per motivi di design — alcuni controlli dei moduli sono difficili da stilizzare.

Tuttavia, quando si procede in questo modo, la vita diventa più difficile sia per lo sviluppatore sia per gli utenti. Il browser fornisce per impostazione predefinita diverse funzionalità dei controlli `<button>` e dei moduli, senza richiedere JavaScript o altro codice aggiuntivo, per rendere i moduli più utilizzabili da tutti gli utenti.

Per esempio:

- Gli elementi semantici vengono compresi dalle tecnologie assistive, quali gli screen reader, che comunicano il loro significato agli utenti che non possono vederli.
- I controlli dei moduli e i pulsanti sono accessibili tramite tastiera per impostazione predefinita. Nell'esempio precedente, provare a spostarsi in avanti e indietro tra gli elementi del modulo utilizzando <kbd>Tab</kbd> e <kbd>Shift</kbd> + <kbd>Tab</kbd> (operazione chiamata "tabbing").
- Si noti inoltre che lo spostamento tramite tab tra gli elementi del modulo fa sì che l'elemento attivo venga evidenziato con un contorno blu (chiamato **contorno di focus**). Questa è una funzionalità importante affinché gli utenti della tastiera sappiano dove si trovano nel modulo.

Se non si utilizzano gli elementi semantici corretti per implementare i moduli, gli elementi del modulo non si comporteranno come gli utenti si aspettano e sembreranno non funzionanti. Sarà necessario reimplementare autonomamente tutte queste funzionalità, con ulteriore lavoro.

## Altri tipi di controllo

Esistono molti altri tipi di controllo che è possibile utilizzare per raccogliere dati in un modulo. Vediamo un esempio leggermente più complesso, quindi lo esploreremo e spiegheremo.

> [!NOTE]
> In questo esempio, si presume che l'utente sia già registrato e abbia effettuato l'accesso, quindi non è necessario raccogliere dati quali nome ed email.

```html live-sample___form-other-controls
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>Second form</title>
  </head>
  <body>
    <form action="./payment_page" method="get">
      <h2>Register for the meetup</h2>
      <fieldset>
        <legend>Choose hotel room type:</legend>
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
  </body>
</html>
```

Il risultato visualizzato è il seguente:

{{EmbedLiveSample("form-other-controls", "100%", "500", , , , , "allow-forms")}}

Si consiglia di aprire questo esempio in una scheda separata del browser mentre si procede con le prossime sezioni, nelle quali verrà esaminato ciascun tipo di controllo. Per farlo, copiare il codice in un file HTML utilizzando l'editor di codice e aprirlo in una scheda del browser.

Prima di proseguire, sperimentare con i diversi controlli del modulo nella copia locale e selezionare alcuni valori. Provare a inviare il modulo e osservare l'aspetto dei dati inviati nell'URL.

### Radio button

I pulsanti "Choose hotel room type" sono implementati utilizzando controlli [`<input type="radio">`](/it/docs/Web/HTML/Reference/Elements/input/radio). Vengono renderizzati come un insieme di controlli a pulsante di cui può essere selezionato solo uno alla volta — non è possibile selezionarne più di uno contemporaneamente. Prendono il nome dai pulsanti presenti nelle vecchie radio, in cui premendo un pulsante quello selezionato in precedenza tornava fuori.

Il codice di esempio ha il seguente aspetto:

```html
<fieldset>
  <legend>Choose hotel room type:</legend>
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

I tipi di input `radio` funzionano per lo più come i tipi di input `text`, ma con alcune differenze:

- Gli attributi `name` per ciascun gruppo di radio button devono contenere lo stesso valore, per associarli in un unico gruppo. Se contengono valori diversi, saranno di fatto gruppi separati, con valori diversi al momento dell'invio.
- È necessario includere un attributo `value` contenente il valore da inviare per ciascun radio button. Il valore inviato sarà una coppia nome/valore, ma il nome sarà sempre lo stesso, per esempio `hotel=economy` oppure `hotel=superior`.
- Il `<label>` di ciascun radio button deve descrivere quella particolare scelta di valore, anziché il valore complessivo che si sta selezionando. Il modo preferito per descrivere la scelta di valore complessiva è racchiuderli in un {{htmlelement("fieldset")}}, che accetta come figlio un elemento {{htmlelement("legend")}} contenente la descrizione.

> [!NOTE]
> Oltre a strutturare ed etichettare i moduli, i fieldset hanno altri utilizzi, come [disabilitare](#disabilitare_i_controlli_dei_moduli) un intero gruppo di controlli come una singola unità.

Vale inoltre la pena notare che al primo radio button è stato applicato l'attributo `checked` — ciò fa sì che venga selezionato al primo caricamento della pagina. Questo significa che sarà sempre selezionata un'opzione e che non è possibile deselezionare un radio button senza selezionarne un altro.

Provare a rimuovere l'attributo `checked` dal primo radio button, salvare e ricaricare per osservare l'effetto. Reinserirlo prima di proseguire.

#### Disabilitare i controlli dei moduli

Nell'esempio dei radio button, si noterà che il terzo radio button ha l'attributo `disabled` impostato. Ciò fa sì che il controllo renderizzato sia disattivato e non selezionabile. È utile in molte situazioni in cui un'opzione è normalmente disponibile, ma non in quel momento. Per esempio, un prodotto potrebbe essere esaurito oppure, come nel caso del nostro esempio, tutte le suite attico potrebbero essere prenotate.

L'attributo `disabled` può essere impostato su qualsiasi controllo del modulo, inclusi gli elementi `<button>`. Anche gli elementi `<fieldset>` possono accettare l'attributo `disabled` — ciò fa sì che ogni controllo del modulo all'interno del fieldset venga disabilitato.

Provare a impostare l'attributo `disabled` sui due elementi `<fieldset>`, salvare e ricaricare per osservare l'effetto. Rimuoverli nuovamente prima di proseguire.

### Checkbox

I selettori "classes to attend" sono implementati utilizzando controlli [`<input type="checkbox">`](/it/docs/Web/HTML/Reference/Elements/input/checkbox). Vengono renderizzati come un insieme di checkbox con stato attivo/disattivo. A differenza dei radio button, è possibile selezionarne più di una alla volta.

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

Come si può vedere dagli snippet di codice, radio button e checkbox vengono implementati in modo molto simile (possono inoltre accettare attributi `checked` per essere renderizzati preselezionati al caricamento della pagina). Si comportano inoltre in modo abbastanza simile, con la differenza che i radio button consentono di scegliere zero o un elemento tra molti, mentre le checkbox consentono di scegliere zero o più elementi tra molti.

La differenza principale, oltre al valore `type`, è che ogni checkbox ha un valore `name` diverso e generalmente non riceve attributi `value`. Dal punto di vista del comportamento, questo significa che rappresentano valori di dati diversi, mentre un gruppo di radio button rappresenta un solo valore. Al momento dell'invio, ogni valore viene inviato con un valore di `on` se la checkbox era selezionata — `yoga=on`, `balloon=on` e così via.

> [!NOTE]
> È possibile modificare il valore inviato per una checkbox assegnandole un attributo `value`; per esempio: `<input type="checkbox" id="yoga" name="yoga" value="yes" />` determinerebbe l'invio di `yoga=yes` se selezionata.

### Menu a discesa

I menu a discesa, per esempio il controllo di selezione "How are you getting here" nel nostro esempio, non sono implementati con un tipo `<input>`, ma con gli elementi {{htmlelement("select")}} e {{htmlelement("option")}}:

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

L'elemento `<select>` racchiude tutte le diverse scelte di valore. È qui che si imposta l'attributo `id` che associa il controllo alla sua etichetta e l'attributo `name` che imposta il nome del dato da inviare.

Ogni possibile valore per il dato è rappresentato da un elemento `<option>`, annidato all'interno dell'elemento `<select>`. Ogni elemento `<option>` può accettare un attributo `value`, che specifica il valore da inviare se quell'opzione viene scelta dall'elenco a discesa. Se non viene specificato un `value`, viene utilizzato come valore il testo all'interno dei tag `<option></option>`.

È inoltre possibile dividere le opzioni all'interno di un menu a discesa `<select>` in più sottogruppi utilizzando l'elemento {{htmlelement("optgroup")}}. Consultare la pagina di riferimento di questo elemento per scoprire come.

> [!NOTE]
> Per avere un'opzione specifica selezionata al caricamento della pagina, è possibile aggiungere un attributo `selected` all'elemento `<option>` pertinente.

### Campi di input di testo su più righe

I campi di input di testo su più righe vengono creati utilizzando elementi {{htmlelement("textarea")}}:

```html
<label for="comments">Any other comments:</label>
<textarea id="comments" name="comments" rows="5" cols="33"></textarea>
```

Si comportano nello stesso modo degli elementi `<input type="text">`, tranne per il fatto che consentono l'inserimento di più righe di testo. L'attributo `rows` specifica il numero di righe di altezza predefinito dell'area di testo, mentre l'attributo `cols` specifica il numero di colonne di larghezza predefinito dell'area di testo. Se non vengono specificati, i valori utilizzati sono `cols="20"` e `rows="2"`.

La maggior parte dei browser renderizza le aree di testo con una maniglia di trascinamento in un angolo, che può essere utilizzata per ridimensionarle. Provare a usarla per ridimensionare l'area di testo nella demo.

## Validazione dei moduli

In precedenza sono state esaminate alcune delle funzionalità di base per la validazione dei moduli lato client fornite dal browser. L'attributo `required` viene utilizzato per specificare che un campo deve essere compilato prima che il modulo possa essere inviato; verifica inoltre che venga inserito il tipo di valore corretto per specifici tipi di valore, come indirizzi email, URL, numeri e così via. La validazione è importante per due ragioni principali:

- Assicurarsi che i dati vengano inviati nel formato corretto, in modo che non causino errori nell'applicazione.
- Assicurarsi che i dati non causino problemi di sicurezza. I malintenzionati sanno come inviare dati formattati appositamente affinché, nelle applicazioni non sicure, possano eseguire comandi per eliminare database o assumere il controllo di un sistema.

La validazione dei moduli è un argomento vasto che va oltre lo scopo di questo articolo, quindi per ora ci fermeremo qui. È sufficiente tenere presente che esistono due tipi di validazione dei moduli:

- La validazione lato client, che avviene nel browser, viene implementata utilizzando una combinazione di attributi di validazione dei moduli (come `required`) e JavaScript. La validazione lato client è utile per fornire agli utenti suggerimenti immediati quando hanno inserito dati errati, ma non è altrettanto efficace nel bloccare il passaggio di dati dannosi. È troppo facile disattivare JavaScript o modificare il codice lato client affinché la validazione non funzioni più.
- La validazione lato server, che avviene sul server, viene implementata utilizzando qualsiasi linguaggio stia usando il server. Messaggi malformati possono essere inviati a un server per errore o intenzionalmente. La prassi consolidata è assicurarsi che il server non consideri attendibile nulla di ciò che invia un client, per evitare bug o problemi di sicurezza causati da messaggi malformati. La validazione lato server è ottima per bloccare messaggi dannosi, poiché è più difficile manomettere il codice in esecuzione sul server. La validazione lato server non è altrettanto efficace nel fornire agli utenti suggerimenti sui dati errati, perché i dati devono essere inviati al server per essere validati, quindi il risultato deve essere rimandato al client prima che l'utente possa essere avvisato.

In breve, non bisogna scegliere tra validazione lato client e lato server: servono entrambe. È necessaria la validazione lato client per fornire agli utenti feedback sui dati inseriti e la validazione lato server per assicurarsi che i messaggi abbiano un formato che il server possa gestire in sicurezza. Per iniziare a saperne di più sulla validazione, un buon punto di partenza è [Validazione dei moduli lato client](/it/docs/Learn_web_development/Extensions/Forms/Form_validation).

## Riepilogo

Per ora è tutto. C'è ancora molto da sapere sui moduli, ma a questo punto è stata fornita una comprensione sufficiente per proseguire negli studi.

Successivamente verranno proposti alcuni test che consentono di verificare quanto siano state comprese e memorizzate le informazioni fornite sui moduli HTML.

## Vedi anche

- [Moduli web — Lavorare con i dati degli utenti](/it/docs/Learn_web_development/Extensions/Forms)

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Planet_data_table", "Learn_web_development/Core/Structuring_content/Test_your_skills/Forms_and_buttons", "Learn_web_development/Core/Structuring_content")}}
