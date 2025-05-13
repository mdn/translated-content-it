---
title: Moduli e pulsanti in HTML
short-title: Moduli e pulsanti
slug: Learn_web_development/Core/Structuring_content/HTML_forms
l10n:
  sourceCommit: 874ad29df9150037acb8a4a3e7550a302c90a080
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Planet_data_table", "Learn_web_development/Core/Structuring_content/Debugging_HTML", "Learn_web_development/Core/Structuring_content")}}

I moduli e i pulsanti HTML sono strumenti potenti per interagire con gli utenti di un sito web. Più comunemente, essi forniscono agli utenti controlli per manipolare un'interfaccia utente (UI) o inserire dati quando richiesto.

In questo articolo, forniamo un'introduzione alle basi dei moduli e dei pulsanti. C'è molto altro da sapere — molti tipi di input e funzionalità dei moduli non sono menzionati — ma questo articolo le darà una solida base per la maggior parte dei casi. Può imparare gli usi avanzati o specializzati su base necessaria come parte dell'apprendimento costante che farà durante la sua carriera.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con HTML di base, come trattato in
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi di base HTML</a
        >. Semantica a livello di testo come <a href="/it/docs/Learn_web_development/Core/Structuring_content/Headings_and_paragraphs"
          >intestazioni e paragrafi</a
        > e <a href="/it/docs/Learn_web_development/Core/Structuring_content/Lists"
          >elenchi</a
        >. <a href="/it/docs/Learn_web_development/Core/Structuring_content/Structuring_documents"
          >HTML Strutturale</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Una comprensione che i moduli e i pulsanti sono gli strumenti principali per interagire con un sito web, insieme ai collegamenti.</li>
          <li>Diversi tipi di pulsanti.</li>
          <li>Tipi comuni di <code>&lt;input&gt;</code>.</li>
          <li>Attributi comuni come <code>name</code> e <code>value</code>.</li>
          <li>L'elemento <code>&lt;form&gt;</code> e le basi dell'invio dei moduli.</li>
          <li>Rendendo i moduli accessibili con etichette e semantica corretta.</li>
          <li>Altri tipi di controllo: <code>&lt;textarea&gt;</code>, <code>&lt;select&gt;</code> e <code>&lt;option&gt;</code>.</li>
          <li>Nozioni di base sulla convalida lato client.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Interagire con gli utenti

Finora nel corso, abbiamo visto un paio di modi in cui gli utenti possono interagire con il web:

- [Collegamenti](/it/docs/Learn_web_development/Core/Structuring_content/Creating_links) possono essere usati per navigare a diverse sezioni di contenuto, sia sulla stessa pagina che su una pagina diversa.
- Gli elementi [`<video>` e `<audio>`](/it/docs/Learn_web_development/Core/Structuring_content/HTML_video_and_audio) generalmente dispongono di controlli come play/pausa, avanzamento rapido, riavvolgimento, ecc., che permettono agli utenti di consumare contenuti multimediali secondo le loro preferenze.

Tuttavia, queste funzionalità tendono a facilitare interazioni unidirezionali, con gli utenti che consumano passivamente i contenuti. Questo va bene, ma il web è un'esperienza bidirezionale. Gli utenti del sito web impostano preferenze su come vogliono vivere i contenuti e i servizi. Ordinano taxi e richiedono richiamate. Forniscono feedback e fanno reclami. Comprano prodotti e li fanno consegnare a casa.

Per fornire questa esperienza bidirezionale, è necessario utilizzare pulsanti e moduli.

I pulsanti sono solitamente creati utilizzando elementi HTML {{htmlelement("button")}} (vengono anche talvolta creati utilizzando elementi {{htmlelement("input")}} con i loro attributi `type` impostati su un valore come `button` o `submit`). Questi pulsanti sono di uso generale — può collegarli per attivare qualsiasi funzionalità voglia, limitato solo dalla sua immaginazione e abilità di codifica.

I moduli sono creati utilizzando elementi come {{htmlelement("form")}}, {{htmlelement("label")}}, {{htmlelement("input")}}, e {{htmlelement("select")}}. Gli elementi del modulo possono essere utilizzati per creare controlli più complessi di quanto consentano i semplici pulsanti — ad esempio un menu a tendina contenente più opzioni che le permettono di scegliere tra diversi temi per un elemento dell'interfaccia utente.

Tuttavia, cosa fondamentale, possono anche essere usati per creare moduli che gli utenti devono compilare quando devono inviare informazioni a un server del sito web. Pensi ai siti di e-commerce — quando vuole cercare un prodotto da comprare, utilizza un modulo per inserire i termini di ricerca. Quando vuole pagare alcuni articoli e finalizzare la consegna, utilizza un modulo per inserire il suo indirizzo postale e un altro modulo per inserire i dettagli della sua carta di credito.

Ci concentreremo principalmente su questo — uso degli elementi form in modo più tradizionale — in questo articolo. Noti che i pulsanti sono comunemente usati all'interno dei moduli, per inviare i dati inseriti al server.

Ora che abbiamo chiarito questa teoria importante, passiamo ad esplorare il codice e vedere come vengono implementati pulsanti e moduli.

## Pulsanti

Come accennato sopra, i pulsanti hanno un paio di usi principali sul web. Prima di tutto, sono usati per attivare funzionalità, il che è utile quando si creano controlli per l'interfaccia utente. Il pulsante più semplice è implementato utilizzando il seguente codice:

```html live-sample___basic-button
<button>Press me</button>
```

Viene reso come segue:

{{EmbedLiveSample("basic-button", "100%", "60")}}

Il testo che appare tra i tag `<button></button>` viene reso all'interno del pulsante, e il browser gli assegna uno stile di base affinché assomigli e si comporti come un pulsante di default. Fino a qui, tutto bene. Tuttavia, c'è un problema qui: il nostro pulsante solitario non farà nulla di utile da solo. Per renderlo utile, è necessario inserirlo all'interno di un modulo (che tratteremo più avanti) o aggiungere un po' di JavaScript.

Ad esempio, se applica il seguente JavaScript al pulsante sopra:

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

Le restituirà il seguente output — provi a cliccarlo:

{{EmbedLiveSample("basic-button-with-js", "100%", "60")}}

Non ci si aspetta che capisca come funziona il JavaScript per ora. Imparerà di più al riguardo più avanti nel corso.

Nella prossima sezione, vedrà una dimostrazione del secondo uso principale dei pulsanti — inviare moduli.

## L'anatomia di un modulo

Un modulo di base contiene tre cose:

- Un elemento {{htmlelement("form")}}, che racchiude tutto il contenuto del modulo. Qualsiasi controllo del modulo all'interno dei tag `<form></form>` fa parte dello stesso modulo, e i loro dati vengono inclusi quando il modulo viene inviato.
- Una o più coppie costituite ciascuna da un elemento {{htmlelement("label")}} e un elemento di controllo del modulo (di solito un elemento {{htmlelement("input")}}, ma ci sono anche altri tipi, ad esempio {{htmlelement("select")}}):
  - L'elemento di controllo del modulo permette all'utente di scegliere o inserire alcuni dati, che verranno inviati al server quando il modulo viene inviato.
  - L'elemento `<label>` fornisce un'etichetta identificativa associata al controllo del modulo che descrive i dati che devono essere inseriti in esso.
- Un elemento {{htmlelement("button")}}, utilizzato per inviare il modulo.

Guardiamo un esempio di base che include i tre elementi sopra. Questo modulo potrebbe essere utilizzato per chiedere il nome e l'email di un utente, per iscriverlo a una newsletter (non si preoccupi — non è collegato a nessun server, quindi attualmente non farà nulla).

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

Viene reso come segue:

{{EmbedLiveSample("form-anatomy", "100%", "200")}}

A causa del modo in cui funziona MDN, può inserire testo nei campi input, ma non vedrà il modulo inviare correttamente quando preme il pulsante. Per seguire le sezioni successive, copi il codice HTML sopra in un nuovo file HTML usando il suo [editor di codice](/it/docs/Learn_web_development/Getting_started/Environment_setup/Code_editors) e lo apra in una nuova scheda del browser.

### L'elemento `<form>`

Come abbiamo detto in precedenza, l'elemento {{htmlelement("form")}} funge da involucro esterno per il modulo, raggruppando insieme tutti i controlli del modulo all'interno di esso. Quando il `<button>` viene premuto, tutti i dati rappresentati dai controlli del modulo verranno inviati al server. L'elemento `<form>` può assumere molti attributi, ma i due più importanti, che abbiamo incluso qui, sono i seguenti:

- `action`: Contiene un percorso alla pagina a cui vogliamo inviare i dati del modulo inviati, per essere elaborati. Più avanti, dopo l'invio del modulo, vedrà `/submit_page` incluso nell'URL. Riceverà anche un errore di risposta {{HTTPStatus("404")}} perché la pagina non esiste realmente, ma va bene per ora.
- `method`: Specifica il [metodo](/it/docs/Web/HTTP/Reference/Methods) di trasmissione dei dati che vuole utilizzare per inviare i dati del modulo al server. Non si preoccupi troppo di questo per ora; il valore `get` causa l'invio dei dati come parametri allegati alla fine dell'URL.

> [!CALLOUT]
>
> **Provi**
>
> Vada all'esempio nella scheda separata, provi a inserire un nome di "Bob" e un indirizzo email di "bob@bob.com".
>
> I due attributi sopra causano l'invio dei dati del modulo in un URL lungo le seguenti linee:
>
> `/some/url/submit_page?name=Bob&email=bob%40bob.com`

#### Strutturare i moduli

Può includere qualsiasi elemento HTML desideri all'interno di un elemento `<form>` per strutturare gli elementi del modulo stessi e fornire contenitori da targetizzare con CSS per lo styling, ecc.

Nel nostro esempio, abbiamo incluso un [elemento di intestazione](/it/docs/Web/HTML/Reference/Elements/Heading_Elements) (`<h2>`) per descrivere lo scopo del modulo.

Abbiamo anche inserito ogni coppia input/etichetta e il pulsante di invio all'interno di un separato {{htmlelement("p")}}, in modo che ciascuno appaia su una linea separata. Questi elementi sono tutti inline di default, il che significa che se non lo facessimo, apparirebbero tutti sulla stessa linea.

Questo è un modello comune per la strutturazione dei moduli. Alcuni usano elementi `<p>` per separare i loro elementi del modulo, altri usano {{htmlelement("div")}}, {{htmlelement("section")}}, o anche elementi {{htmlelement("li")}}. Non importa molto, purché gli elementi utilizzati abbiano senso semantico. Ad esempio, ha senso dividere i gruppi di elementi del modulo in paragrafi separati o sezioni di contenuto, o anche elementi in una lista. Ha meno senso rappresentarli come [blockquotes](/it/docs/Web/HTML/Reference/Elements/blockquote), [asides](/it/docs/Web/HTML/Reference/Elements/aside), o [address](/it/docs/Web/HTML/Reference/Elements/address).

C'è un elemento specializzato per raggruppare insieme gli elementi del modulo chiamato {{htmlelement("fieldset")}}. Questo è utile in determinate circostanze, come nei moduli complessi, e quando si raggruppano più checkbox e pulsanti radio. Vedremo un paio di esempi di `<fieldset>` più avanti.

### Elementi `<input>`

Gli elementi {{htmlelement("input")}} rappresentano i diversi elementi di dati inseriti nel modulo. Studiamo uno degli esempi dal nostro modulo di base:

```html
<input type="text" name="name" id="name" required />
```

Gli attributi sono i seguenti:

- `type`: Specifica il tipo di controllo del modulo da creare. Ci sono molti diversi tipi di controlli del modulo, dai semplici campi di testo di diverso tipo ai pulsanti radio, checkbox e altro ancora. Il tipo `text` rende un campo di testo di base che può accettare qualsiasi valore.
- `name`: Specifica un nome per l'elemento di dati. Quando il modulo viene inviato, i dati vengono inviati in coppie nome/valore. In ciascun caso, il nome è uguale a questo valore di attributo `name`, e il valore è uguale al testo inserito nel campo di testo.
- `id`: Specifica un ID che può essere utilizzato per identificare l'elemento. In questo caso, viene utilizzato per associare il controllo del modulo con il suo `<label>`.
- `required`: Specifica che è necessario immettere un valore nell'elemento del modulo prima che il modulo possa essere inviato. Questo dovrebbe essere impostato solo sui campi che richiede, non sui campi opzionali.

Dovrebbe essere consapevole del fatto che alcuni tipi di input di solito non ottengono i loro valori da testo inserito in un campo. Ad esempio, [`<input type="color">`](/it/docs/Web/HTML/Reference/Elements/input/color) restituisce un widget selettore di colori da cui scegliere un colore, mentre [`<input type="radio">`](/it/docs/Web/HTML/Reference/Elements/input/radio) restituisce un controllo a pulsante radio che può essere selezionato o meno.

Nel caso dei pulsanti radio, è generalmente necessario fornire il valore che sarebbe inviato se selezionato all'interno di uno specifico attributo `value`. Noti che può specificare un attributo `value` sui tipi di input come `text` e `color` — l'effetto è che il valore viene precompilato nel campo del modulo quando viene visualizzato per la prima volta.

> [!CALLOUT]
>
> **Provi**
>
> 1. Di nuovo, vada all'esempio caricato in una scheda separata e provi a inviare il modulo senza alcun valore inserito in nessuno dei campi. Vedrà un messaggio di errore comparire accanto al campo "Nome" dicendo qualcosa come "Compili questo campo" (varierà tra i diversi browser). Questo è l'attributo `required` — e la convalida del modulo lato client del browser — in azione.
> 2. Ora provi a inviare il modulo con un nome valido inserito nel primo campo, ma un valore che non è un indirizzo email valido nel secondo campo (qualcosa come "aaaa" andrà bene). Questa volta vedrà un messaggio di errore comparire accanto al campo "Email" dicendo qualcosa come "Inserisca un indirizzo email".
> 3. Per questo esercizio, dovrà modificare il codice del modulo. Può farlo aprendo l'esempio locale creato nel suo editor di testo, modificarlo lì e salvarlo. Provi a modificare il modulo per includere `value="Bob"` sul primo input. Quando ricarica il codice, vedrà che il primo campo ha un valore di "Bob" inserito di default.

#### Input di campo di testo specializzati

Il secondo esercizio sopra solleva un punto interessante. Il secondo campo input richiede specificamente un indirizzo email, e convalida i valori inseriti di conseguenza. Se guarda di nuovo il codice del modulo, vedrà il perché — il secondo `<input>` ha un `type` di `email`. Ci sono diversi tipi di input di campo di testo specializzati progettati per gestire tipi specifici di dati — [`<input type="number">`](/it/docs/Web/HTML/Reference/Elements/input/number), [`<input type="password">`](/it/docs/Web/HTML/Reference/Elements/input/password), [`<input type="tel">`](/it/docs/Web/HTML/Reference/Elements/input/tel), ecc.

> [!CALLOUT]
>
> **Provi**
>
> Segua alcuni dei link sopra per scoprire per cosa sono utilizzati questi tipi di input. Dia un'occhiata al nostro riferimento [`<input>`](/it/docs/Web/HTML/Reference/Elements/input) e veda se riesce a trovare altri tipi di input di campo di testo specializzati.

### Elementi `<label>`

Come abbiamo detto sopra, gli elementi {{htmlelement("label")}} forniscono etichette identificative associate ai controlli del modulo che descrivono i dati che devono essere inseriti in essi. Può inserire qualsiasi contenuto di testo desideri negli elementi `<label>`, ma essi dovrebbero descrivere accuratamente quali dati il controllo del modulo associato si aspetta. L'associazione viene creata assegnando al controllo del modulo un attributo `id`, quindi assegnando all'elemento `<label>` un attributo `for` con lo stesso valore dell'`id` del controllo.

Ad esempio:

```html
<label for="name">Name (required):</label>
<input type="text" name="name" id="name" required />
```

Gli elementi `<label>` sono importanti per diverse ragioni, soprattutto perché:

- Quando gli utenti ipovedenti utilizzano un lettore di schermo per aiutarli a leggere e interagire con il contenuto della pagina web, il lettore di schermo leggerà il testo dell'etichetta associata quando ciascun controllo viene incontrato. Questo rende più facile per gli utenti capire quali contenuti devono essere inseriti in ciascun controllo.
- Le consentono di focalizzare gli elementi del modulo cliccando sul loro testo di etichetta oltre che sui controlli. Ciò è particolarmente utile per gli utenti di telefoni cellulari, dove può essere difficile selezionare accuratamente un elemento del modulo con il dito su uno schermo touch. Ingrandire l'area di "hit" è utile in tali circostanze.

#### Etichette di modulo esplicite e implicite

Lo stile dell'etichetta del modulo che ha visto sopra è chiamato **etichetta di modulo esplicita** — l'associazione tra controllo ed etichetta viene fatta esplicitamente tramite gli attributi `id` e `for`. Può anche implementare un'etichetta di modulo **implicita** annidando il controllo all'interno dell'etichetta, come segue:

```html
<label>
  Name (required):
  <input type="text" name="name" required />
</label>
```

L'annidamento crea un'associazione implicita tra controllo ed etichetta, e non ha più bisogno degli attributi `id` e `for`.

Entrambi gli approcci vanno bene, ma consiglieremmo di usare l'approccio dell'etichetta esplicita. Questo perché l'associazione esplicita è generalmente più facile da identificare e comprendere, specialmente man mano che il suo codice HTML diventa più complesso. Inoltre, i lettori di schermo (e altre tecnologie assistive) non gestiscono sempre correttamente le etichette implicite.

Può leggere di più sulle migliori pratiche di etichettatura dei moduli in [HTML Inputs and Labels: A Love Story](https://css-tricks.com/html-inputs-and-labels-a-love-story/), csstricks.com (2021).

### L'elemento `<button>`

Quando un elemento {{htmlelement("button")}} è incluso all'interno di un elemento `<form>`, il suo comportamento predefinito è di inviare il modulo, a condizione che non ci siano dati non validi presenti che causino il blocco dell'invio dalla convalida del modulo lato client. Ha già visto questo comportamento giocando con il nostro esempio di modulo di base sopra.

Ci sono altri comportamenti del pulsante che possono essere specificati tramite l'attributo `type` dell'elemento `<button>`:

- `<button type="submit">` dichiara esplicitamente che un pulsante dovrebbe comportarsi come un pulsante di invio. Non è mai realmente necessario dichiararlo, a meno che per qualche motivo non stia includendo altri pulsanti all'interno del suo `<form>`, e voglia rendere chiaro quale sia il pulsante di invio. Questo sarà molto raro.
- `<button type="reset">` crea un *pulsante di reset* — questo elimina immediatamente tutti i dati dal modulo, ripristinandolo al suo stato iniziale. **Non usi i pulsanti di reset** — erano popolari nei primi giorni del web, ma sono solitamente più fastidiosi che utili. La maggior parte delle persone ha sperimentato il riempimento di un lungo modulo per poi cliccare accidentalmente il pulsante di reset invece del pulsante di invio, il che significa che devono ricominciare da capo.
- `<button type="button">` crea un pulsante con lo stesso comportamento dei pulsanti specificati al di fuori degli elementi `<form>`. Come abbiamo visto in precedenza, non fanno assolutamente nulla di default, e serve il JavaScript per dare loro funzionalità.

> [!NOTE]
> Può anche creare i tipi di pulsanti sopra utilizzando un elemento `<input>` con gli stessi valori `type` specificati — [`<input type="submit">`](/it/docs/Web/HTML/Reference/Elements/input/submit), [`<input type="reset">`](/it/docs/Web/HTML/Reference/Elements/input/reset), e [`<input type="button">`](/it/docs/Web/HTML/Reference/Elements/input/button). Tuttavia, questi hanno molti svantaggi rispetto ai loro omologhi `<button>`. Dovrebbe preferire l'uso di `<button>`.

## Una nota sull'accessibilità

Abbiamo già parlato dell'importanza delle etichette dei moduli per l'accessibilità, ma volevamo anche includere alcuni commenti sull'importanza generale dell'uso degli elementi semantici corretti per creare moduli (ad esempio, usi un `<button>` per inviare il suo modulo, e non un `<div>` programmato per comportarsi come un `<button>`). È perfettamente possibile utilizzare una combinazione di CSS e JavaScript per fare in modo che praticamente qualsiasi elemento HTML assomigli e si comporti come un elemento del modulo. Gli sviluppatori di solito fanno questo per ragioni di design — alcuni controlli del modulo sono difficili da stilizzare.

Tuttavia, quando lo fa, rende la vita più difficile per sé stesso e per gli utenti. Il browser fornisce diverse funzionalità di `<button>` e controllo del modulo di default, senza necessità di JavaScript o altro codice extra, per rendere i moduli più utilizzabili per tutti gli utenti.

Ad esempio:

- Gli elementi semantici sono compresi dalle tecnologie assistive come i lettori di schermo, che comunicano il loro significato agli utenti che non possono vederli.
- I controlli dei moduli e i pulsanti sono accessibili da tastiera di default. Nell'esempio precedente, provi a muoversi avanti e indietro tra gli elementi del modulo usando <kbd>Tab</kbd> e <kbd>Shift</kbd> + <kbd>Tab</kbd> (chiamato "tabbing").
- Noti anche come fare tabbing tra gli elementi del modulo faccia sì che il elemento focalizzato venga evidenziato usando un contorno blu (chiamato **focus outline**). Questa è una funzionalità importante per gli utenti da tastiera per sapere dove si trovano attualmente nel modulo.

Se non utilizza gli elementi semantici corretti per implementare i suoi moduli, dovrà reimplementare tutte queste funzionalità, altrimenti gli elementi del modulo non si comporteranno come gli utenti si aspettano e sembreranno quindi rotti. Tutto si somma.

## Altri tipi di controllo

Ci sono molti altri tipi di controllo che può utilizzare per raccogliere dati in un modulo. Guardiamo un esempio leggermente più complesso, e poi esploreremo e spiegheremo.

> [!NOTE]
> In questo esempio, assumiamo che l'utente sia già registrato e loggato, quindi non è necessario raccogliere dati come nome ed email.

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

Viene reso come segue:

{{EmbedLiveSample("form-other-controls", "100%", "500")}}

Consigliamo di aprire questo esempio in una scheda separata del browser mentre lavora attraverso le prossime sezioni, in cui guardiamo ciascun tipo di controllo a turno. Per ottenere questo, copi il codice in un file HTML utilizzando il suo editor di codice e lo apra in una scheda del browser.

> [!CALLOUT]
>
> **Provi**
>
> Prima di andare avanti, giochi con i diversi controlli del modulo, selezioni alcuni valori e provi a inviare il modulo.

### Pulsanti radio

I pulsanti "Scegli tipo di camera d'albergo" sono implementati utilizzando i controlli [`<input type="radio">`](/it/docs/Web/HTML/Reference/Elements/input/radio). Questi vengono riprodotti come un set di controlli a pulsante in cui solo uno del set può essere selezionato in qualsiasi momento — non può selezionarne più di uno alla volta. Sono chiamati così dai pulsanti trovati sulle radio vecchio stile, dove si preme un pulsante e quello selezionato precedentemente poppa di nuovo fuori.

Il nostro esempio di codice appare così:

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

I tipi di input `radio` funzionano principalmente allo stesso modo dei tipi di input `text`, ma con alcune differenze:

- Gli attributi `name` per ciascun set di pulsanti radio devono contenere lo stesso valore, per associarli insieme come un solo set. Se contengono valori diversi, saranno effettivamente set separati, con valori diversi all'invio.
- Deve includere un attributo `value` contenente il valore da inviare per ciascun pulsante radio. Il valore inviato sarà una coppia nome/valore, ma il nome sarà sempre lo stesso, ad esempio `hotel=economy` o `hotel=superior`.
- L'etichetta `<label>` per ciascun pulsante radio deve descrivere quella particolare scelta di valore, piuttosto che il valore complessivo che sta selezionando. Il modo preferito per fornire una descrizione della scelta complessiva del valore è racchiuderli in un {{htmlelement("fieldset")}}, che prende un elemento {{htmlelement("legend")}} come figlio che contiene la descrizione.

> [!NOTE]
> Oltre alla struttura e all'etichettatura dei moduli, i fieldset hanno altri usi, come [disabilitare](#disabilitare_controlli_modulo) un intero set di controlli come unità singola.

Vale anche la pena notare che abbiamo applicato l'attributo `checked` al primo pulsante radio — questo fa sì che sia selezionato quando la pagina viene caricata per la prima volta. È così che giustifichiamo la necessità di segnare il valore del tipo di camera d'albergo come "richiesto" — un'opzione sarà sempre selezionata, e non è possibile deselezionare un pulsante radio senza selezionarne un altro.

> [!CALLOUT]
>
> **Provi**
>
> Provi a rimuovere l'attributo `checked` dal primo pulsante radio, salvi, quindi ricarichi, per vedere l'effetto che ha. Ripristini prima di andare avanti.

#### Disabilitare controlli del modulo

Nell'esempio dei pulsanti radio, noterà che il terzo pulsante radio ha l'attributo `disabled` impostato su di esso. Questo fa sì che il controllo reso sia grigio e non selezionabile. Questo è utile in molte situazioni in cui un'opzione è normalmente disponibile, solo non in questo momento. Ad esempio, un prodotto potrebbe essere esaurito, o come nel caso del nostro esempio, tutte le suite attico sono prenotate!

Può impostare l'attributo `disabled` su qualsiasi controllo del modulo, inclusi gli elementi `<button>`. Gli elementi `<fieldset>` possono anche accettare l'attributo `disabled` — questo fa sì che ogni controllo del modulo all'interno del fieldset sia disabilitato.

> [!CALLOUT]
>
> **Provi**
>
> Provi a impostare l'attributo `disabled` sui due elementi `<fieldset>`, salvi, quindi ricarichi, per vedere l'effetto che ha. Li rimuova di nuovo prima di andare avanti.

### Checkbox

I nostri selettori "classi da frequentare" sono implementati usando i controlli [`<input type="checkbox">`](/it/docs/Web/HTML/Reference/Elements/input/checkbox). Questi rendono come un set di checkbox in stato on/off. A differenza dei pulsanti radio, può selezionarne più di uno alla volta.

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

Come può vedere dai frammenti di codice, i pulsanti radio e i checkbox sono implementati in modo molto simile (possono anche prendere attributi `checked` per renderli preselezionati quando la pagina viene caricata). Si comportano anche in modo abbastanza simile, tranne che i pulsanti radio le permettono di scegliere zero o un elemento tra molti, e i checkbox le permettono di scegliere zero o più elementi tra molti.

La differenza principale (a parte il valore del `type`!) è che ciascun checkbox ha un valore di `name` diverso, e generalmente non viene dato alcun attributo `value`. Sul piano del comportamento, questo significa che rappresentano valori di dati diversi, mentre un set di pulsanti radio rappresenta solo uno. All'invio, ciascun valore viene inviato con un valore di `on` se il checkbox era selezionato — `yoga=on`, `balloon=on`, ecc.

> [!NOTE]
> È possibile cambiare il valore inviato per un checkbox assegnandogli un attributo `value`, ad esempio: `<input type="checkbox" id="yoga" name="yoga" value="yes" />` risulterebbe in `yoga=yes` inviato se selezionato. Tuttavia, non c'è molto senso farlo. Un checkbox viene inviato o meno con un singolo valore se selezionato, o non viene inviato affatto. Non importa davvero quale valore viene inviato al server.

### Menu a tendina

I menu a tendina, ad esempio il controllo di selezione "Come sta arrivando qui" nel nostro esempio, sono implementati non con un tipo `<input>`, ma con gli elementi {{htmlelement("select")}} e {{htmlelement("option")}}:

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

L'elemento `<select>` racchiude tutte le diverse scelte di valore. È dove si imposta l'attributo `id` che associa il controllo con la sua etichetta, e l'attributo `name` che imposta il nome dell'elemento dati da inviare.

Ciascun valore possibile per l'elemento dati è rappresentato da un elemento `<option>`, annidato all'interno dell'elemento `<select>`. Ogni elemento `<option>` può prendere un attributo `value`, che specifica il valore da inviare se quell'opzione è scelta dalla lista a tendina. Se non specifica un `value`, viene utilizzato il testo all'interno dei tag `<option></option>` come valore.

> [!NOTE]
> Se desidera che una specifica opzione venga selezionata al caricamento della pagina, può aggiungere un attributo `selected` all'elemento `<option>` rilevante.

### Campi di input testo multi-linea

I campi di input testo multi-linea sono creati utilizzando elementi {{htmlelement("textarea")}}:

```html
<label for="comments">Any other comments:</label>
<textarea id="comments" name="comments" rows="5" cols="33"></textarea>
```

Si comportano allo stesso modo degli elementi `<input type="text">`, tranne che consentono di inserire più righe di testo. L'attributo `rows` specifica il numero di righe che l'area di testo avrà di default, mentre l'attributo `cols` specifica il numero di colonne che avrà di default. Se non sono specificati, i valori utilizzati sono `cols="20"` e `rows="2"`.

> [!CALLOUT]
>
> **Provi**
>
> La maggior parte dei browser rende le aree di testo con un manico di trascinamento in un angolo, che può essere usato per ridimensionarlo. Provi a utilizzare questo per ridimensionare l'area di testo nel nostro demo.

## Validazione dei moduli

In precedenza, abbiamo visto alcune delle nozioni di base sulla validazione dei moduli lato client fornita dal browser. L'attributo `required` è utilizzato per specificare che un campo deve essere compilato prima che il modulo possa essere inviato; verifica anche che il tipo di valore corretto sia inserito per tipi di valore specifici come indirizzi email, URL, numeri, ecc. La validazione è importante per due motivi principali:

- Assicurarsi che i dati siano inviati nel formato corretto in modo che non causino errori nella sua applicazione.
- Assicurarsi che i dati non causino problemi di sicurezza. Le persone male intenzionate sanno come inviare dati formattati specificamente in modo che, su applicazioni insicure, possa eseguire comandi per eliminare database o prendere il controllo di un sistema.

La validazione dei moduli è un tema enorme che è oltre lo scopo di questo articolo, quindi la lasceremo qui per ora. Solo tenga a mente che ci sono due tipi di validazione dei moduli:

- La validazione lato client, che avviene nel browser, implementata usando una combinazione di attributi di validazione del modulo (come `required`) e JavaScript. La validazione lato client è utile per dare agli utenti suggerimenti immediati quando hanno inserito dati errati, ma non è così efficace nel bloccare i dati malevoli dall'arrivare. È troppo facile disattivare JavaScript o alterare il codice lato client in modo che la validazione non funzioni più.
- La validazione lato server, che avviene sul server, implementata usando qualsiasi linguaggio il server stia utilizzando. Messaggi mal formati possono essere inviati a un server per caso o di proposito. La saggezza convenzionale è di assicurarsi che il suo server non si fidi di niente che un client stia inviando per evitare bug o problemi di sicurezza causati da messaggi mal formati. La validazione lato server è ottima per bloccare messaggi malevoli, poiché è più difficile manomettere il codice che gira sul server. La validazione lato server non è così buona a dare agli utenti suggerimenti sui dati errati perché i dati devono andare al server per essere validati, poi il risultato deve essere inviato indietro al client prima che l'utente possa essere notificato.

In breve, non decida tra usare la validazione lato client o lato server - avrà bisogno di entrambi. Ha bisogno della validazione lato client per dare agli utenti feedback sui loro input e della validazione lato server per assicurarsi che i messaggi siano in un formato che il suo server può gestire in sicurezza. Se vuole iniziare a imparare di più sulla validazione, un buon punto di partenza è [Validazione dei moduli lato client](/it/docs/Learn_web_development/Extensions/Forms/Form_validation).

## Riepilogo

Questo è tutto per ora. C'è molto altro da sapere sui moduli, ma per ora le abbiamo dato abbastanza comprensione per andare avanti nei suoi studi.

Prossimamente, daremo un'occhiata a come eseguire il debug dei problemi nel codice HTML.

## Vedi anche

- [Moduli web — Lavorare con i dati utente](/it/docs/Learn_web_development/Extensions/Forms)

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Planet_data_table", "Learn_web_development/Core/Structuring_content/Debugging_HTML", "Learn_web_development/Core/Structuring_content")}}
