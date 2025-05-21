---
title: Pseudo-classi UI
slug: Learn_web_development/Extensions/Forms/UI_pseudo-classes
l10n:
  sourceCommit: a1ac64fa4da965d2a152f08221b1a9aed638fd16
---

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/Customizable_select", "Learn_web_development/Extensions/Forms/Form_validation", "Learn_web_development/Extensions/Forms")}}

Negli articoli precedenti, abbiamo trattato la stilizzazione di vari controlli del modulo in modo generale. Questo includeva l'uso di alcune pseudo-classi, ad esempio, usando `:checked` per selezionare una casella solo quando è selezionata. In questo articolo, esploriamo le diverse pseudo-classi UI disponibili per stilizzare i moduli in differenti stati.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Comprensione di base di
        <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, incluso la conoscenza generale delle
        <a
          href="/it/docs/Learn_web_development/Core/Styling_basics/Pseudo_classes_and_elements"
          >pseudo-classi e pseudo-elementi</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere quali parti dei moduli sono difficili da stilizzare e perché; apprendere
        cosa si può fare per personalizzarle.
      </td>
    </tr>
  </tbody>
</table>

## Quali pseudo-classi abbiamo disponibili?

Le pseudo-classi originali (da [CSS 2.1](https://www.w3.org/TR/CSS21/selector.html#dynamic-pseudo-classes)) rilevanti per i moduli sono:

- {{cssxref(":hover")}}: Seleziona un elemento solo quando è sovrastato da un puntatore del mouse.
- {{cssxref(":focus")}}: Seleziona un elemento solo quando è focalizzato (ad esempio, tabulato sulla tastiera).
- {{cssxref(":active")}}: seleziona un elemento solo quando è attivato (ad esempio, mentre viene cliccato, o quando il tasto <kbd>Return</kbd> / <kbd>Enter</kbd> è premuto nel caso di un'attivazione tramite tastiera).

Queste pseudo-classi di base dovrebbero esserti ormai familiari. [Selettori CSS](/it/docs/Web/CSS/CSS_selectors) forniscono numerose altre pseudo-classi relative ai moduli HTML. Queste offrono diverse condizioni utili di targeting che puoi sfruttare. Le discuteremo in dettaglio nelle sezioni successive, ma brevemente, le principali che esamineremo sono:

- {{cssxref(':required')}} e {{cssxref(':optional')}}: Target elementi che possono essere richiesti (ad esempio, elementi che supportano l'attributo HTML [`required`](/it/docs/Web/HTML/Reference/Attributes/required)), basandosi sul fatto che siano richiesti o opzionali.
- {{cssxref(":valid")}} e {{cssxref(":invalid")}}, e {{cssxref(":in-range")}} e {{cssxref(":out-of-range")}}: Target controlli dei moduli che sono validi/invalidi secondo i vincoli di validazione del modulo impostati su di essi, o in-intervallo/fuori-intervallo.
- {{cssxref(":enabled")}} e {{cssxref(":disabled")}}, e {{cssxref(":read-only")}} e {{cssxref(":read-write")}}: Target elementi che possono essere disabilitati (ad esempio, elementi che supportano l'attributo HTML [`disabled`](/it/docs/Web/HTML/Reference/Attributes/disabled)), basandosi sul fatto che siano attualmente abilitati o disabilitati, e controlli del modulo in modalità lettura-scrittura o solo lettura (ad esempio, elementi con l'attributo HTML [`readonly`](/it/docs/Web/HTML/Reference/Attributes/readonly) impostato).
- {{cssxref(":checked")}}, {{cssxref(":indeterminate")}}, e {{cssxref(":default")}}: Rispettivamente, target caselle di controllo e pulsanti di opzione che sono selezionati, in uno stato indeterminato (né selezionati né non selezionati), e l'opzione selezionata per impostazione predefinita quando la pagina viene caricata (ad esempio, un [`<input type="checkbox">`](/it/docs/Web/HTML/Reference/Elements/input/checkbox) con l'attributo [`checked`](/it/docs/Web/HTML/Reference/Elements/input#checked) impostato, o un elemento [`<option>`](/it/docs/Web/HTML/Reference/Elements/option) con l'attributo [`selected`](/it/docs/Web/HTML/Reference/Elements/option#selected) impostato).

Ci sono molte altre pseudo-classi, ma quelle sopra elencate sono quelle più ovviamente utili. Alcune di esse sono destinate a risolvere problemi molto specifici di nicchia. Le pseudo-classi UI sopra elencate sono supportate in modo eccellente dai browser, ma ovviamente, è necessario testare attentamente le implementazioni dei moduli per garantire che funzionino per il pubblico di destinazione.

> [!NOTE]
> Alcune delle pseudo-classi discusse qui sono interessate alla stilizzazione dei controlli del modulo basati sul loro stato di validazione (i loro dati sono validi o no?) Imparerai molto di più sull'impostazione e il controllo dei vincoli di validazione nel nostro prossimo articolo — [Validazione del modulo lato client](/it/docs/Learn_web_development/Extensions/Forms/Form_validation) — ma per ora manterremo le cose semplici riguardo alla validazione del modulo, per non creare confusione.

## Stilizzazione degli input basata sul fatto che siano richiesti o meno

Uno dei concetti più basilari riguardo alla validazione del modulo lato client è se un input del modulo sia richiesto (deve essere compilato prima che il modulo possa essere inviato) oppure opzionale.

Gli elementi {{htmlelement('input')}}, {{htmlelement('select')}}, e {{htmlelement('textarea')}} hanno disponibile l'attributo `required` il quale, quando impostato, significa che è necessario riempire quel controllo prima che il modulo venga inviato con successo.
Ad esempio, il nome e il cognome sono richiesti nel modulo qui sotto, ma l'indirizzo email è opzionale:

```html live-sample___optional-required-styles
<form>
  <fieldset>
    <legend>Feedback form</legend>
    <div>
      <label for="fname">First name: </label>
      <input id="fname" name="fname" type="text" required />
    </div>
    <div>
      <label for="lname">Last name: </label>
      <input id="lname" name="lname" type="text" required />
    </div>
    <div>
      <label for="email"> Email address (if you want a response): </label>
      <input id="email" name="email" type="email" />
    </div>
    <div><button>Submit</button></div>
  </fieldset>
</form>
```

Puoi abbinare questi due stati utilizzando le pseudo-classi {{cssxref(':required')}} e {{cssxref(':optional')}}. Ad esempio, se applichiamo il seguente CSS all'HTML sopra:

```css hidden live-sample___optional-required-styles
body {
  font-family: sans-serif;
  margin: 20px auto;
  max-width: 70%;
}

fieldset {
  padding: 10px 30px 0;
}

legend {
  color: white;
  background: black;
  padding: 5px 10px;
}

fieldset > div {
  margin-bottom: 20px;
  display: flex;
  flex-flow: row wrap;
}

button,
label,
input {
  display: block;
  font-size: 100%;
  box-sizing: border-box;
  width: 100%;
  padding: 5px;
}

input {
  box-shadow: inset 1px 1px 3px #ccc;
  border-radius: 5px;
}

input:hover,
input:focus {
  background-color: #eee;
}

button {
  width: 60%;
  margin: 0 auto;
}
```

```css live-sample___optional-required-styles
input:required {
  border: 2px solid;
}

input:optional {
  border: 2px dashed;
}
```

I controlli richiesti hanno un bordo solido, e il controllo opzionale ha un bordo tratteggiato.
Puoi anche provare a inviare il modulo senza compilarlo, per vedere i messaggi di errore di validazione lato client che i browser ti danno per impostazione predefinita:

{{EmbedLiveSample("optional-required-styles", , "400px", , , , , "allow-forms")}}

In generale, dovresti evitare di stilizzare gli elementi 'richiesti' rispetto a quelli 'opzionali' nei moduli usando solo il colore, perché questo non è ottimo per le persone daltoniche:

```css example-bad
input:required {
  border: 2px solid red;
}

input:optional {
  border: 2px solid green;
}
```

La convenzione standard sul web per lo stato richiesto è un asterisco (`*`), oppure la parola "necessario" associata ai rispettivi controlli.
Nella prossima sezione, vedremo un esempio migliore di indicazione dei campi richiesti usando `:required` e contenuto generato.

> [!NOTE]
> Probabilmente non troverai te stesso a utilizzare molto spesso la pseudo-classe `:optional`. I controlli del modulo sono opzionali per impostazione predefinita, quindi potresti semplicemente fare il tuo stile opzionale per impostazione predefinita, e aggiungere stili in cima per i controlli richiesti.

> [!NOTE]
> Se un pulsante radio in un gruppo di pulsanti radio con lo stesso nome ha l'attributo `required` impostato, tutti i pulsanti radio saranno invalidi fino a quando non ne sarà selezionato uno, ma solo quello con l'attributo assegnato corrisponderà a {{cssxref(':required')}}.

## Uso del contenuto generato con le pseudo-classi

Negli articoli precedenti, abbiamo visto l'uso di [contenuto generato](/it/docs/Web/CSS/CSS_generated_content), ma abbiamo pensato che questo fosse un buon momento per parlarne in modo più dettagliato.

L'idea è che possiamo usare gli pseudo-elementi [`::before`](/it/docs/Web/CSS/::before) e [`::after`](/it/docs/Web/CSS/::after) insieme alla proprietà [`content`](/it/docs/Web/CSS/content) per far apparire un pezzo di contenuto prima o dopo l'elemento interessato. Il pezzo di contenuto non viene aggiunto al DOM, quindi potrebbe essere invisibile a alcuni screen reader. Poiché è uno pseudo-elemento, può essere targettizzato con stili nello stesso modo in cui qualsiasi nodo effettivo del DOM può.

Questo è davvero utile quando vuoi aggiungere un indicatore visivo a un elemento, come un'etichetta o un'icona, quando sono disponibili indicatori alternativi per garantire l'accessibilità a tutti gli utenti. Ad esempio, nel nostro [esempio di pulsanti di opzione personalizzati](https://mdn.github.io/learning-area/html/forms/styling-examples/radios-styled.html), usiamo il contenuto generato per gestire il posizionamento e l'animazione del cerchio interno del pulsante di opzione personalizzato quando un pulsante di opzione è selezionato:

```css
input[type="radio"]::before {
  display: block;
  content: " ";
  width: 10px;
  height: 10px;
  border-radius: 6px;
  background-color: red;
  font-size: 1.2em;
  transform: translate(3px, 3px) scale(0);
  transform-origin: center;
  transition: all 0.3s ease-in;
}

input[type="radio"]:checked::before {
  transform: translate(3px, 3px) scale(1);
  transition: all 0.3s cubic-bezier(0.25, 0.25, 0.56, 2);
}
```

Questo è davvero utile — gli screen reader già fanno sapere ai loro utenti quando un pulsante di opzione o una casella di controllo che incontrano è selezionata, quindi non vuoi che leggano un altro elemento del DOM che indica la selezione — ciò potrebbe essere confuso. Avere un indicatore puramente visivo risolve questo problema.

Non tutti i tipi di `<input>` supportano il contenuto generato inserito su di essi. Tutti i tipi di input che mostrano testo dinamico al loro interno, come `text`, `password`, o `button`, non mostrano contenuto generato. Altri, inclusi `range`, `color`, `checkbox`, ecc., mostrano contenuto generato.

Tornando al nostro esempio richiesto/opzionale di prima, questa volta non altereremo l'aspetto dell'input stesso — useremo il contenuto generato per aggiungere un'etichetta indicativa ([vedi il live qui](https://mdn.github.io/learning-area/html/forms/pseudo-classes/required-optional-generated.html), e vedere il [codice sorgente qui](https://github.com/mdn/learning-area/blob/main/html/forms/pseudo-classes/required-optional-generated.html)).

Prima di tutto, aggiungeremo un paragrafo in cima al modulo per dire cosa stai cercando:

```html
<p>Required fields are labeled with "required".</p>
```

Gli utenti di screen reader sentiranno "necessario" come un'informazione extra quando raggiungono ogni input richiesto, mentre gli utenti vedenti otterranno la nostra etichetta.

Come menzionato in precedenza, gli input di testo non supportano contenuti generati, quindi aggiungiamo un `<span>` vuoto per ospitare il contenuto generato:

```html
<div>
  <label for="fname">First name: </label>
  <input id="fname" name="fname" type="text" required />
  <span></span>
</div>
```

Il problema immediato con questo era che lo span cadeva su una nuova riga sotto l'input perché l'input e l'etichetta sono entrambi impostati con `width: 100%`. Per risolvere questo stile, stiliamo il div genitore affinché diventi un contenitore flessibile, ma anche per dire a esso di avvolgere i suoi contenuti su nuove righe se il contenuto diventa troppo lungo:

```css
fieldset > div {
  margin-bottom: 20px;
  display: flex;
  flex-flow: row wrap;
}
```

L'effetto che ha è che l'etichetta e l'input siedono su righe separate perché entrambi sono `width: 100%`, ma lo `<span>` ha una larghezza di `0` quindi può sedere sulla stessa riga dell'input.

Ora passiamo al contenuto generato. Lo creiamo usando questo CSS:

```css
input + span {
  position: relative;
}

input:required + span::after {
  font-size: 0.7rem;
  position: absolute;
  content: "required";
  color: white;
  background-color: black;
  padding: 5px 10px;
  top: -26px;
  left: -70px;
}
```

Impostiamo lo `<span>` su `position: relative` in modo da poter posizionare il contenuto generato su `position: absolute` e posizionarlo in relazione allo `<span>` piuttosto che al `<body>` (Il contenuto generato si comporta come se fosse un nodo figlio dell'elemento su cui viene generato, ai fini del posizionamento).

Poi diamo al contenuto generato il contenuto "richiesto", che è quello che volevamo che la nostra etichetta dicesse, e lo stiliamo e posizioniamo come vogliamo. Il risultato è visibile di seguito.

{{EmbedGHLiveSample("learning-area/html/forms/pseudo-classes/required-optional-generated.html", '100%', 430)}}

## Stilizzazione dei controlli basata sul fatto che i dati siano validi

L'altro concetto davvero importante e fondamentale nella validazione del modulo è se i dati di un controllo del modulo sono validi o meno (nel caso di dati numerici, possiamo anche parlare di dati in intervallo e fuori intervallo). I controlli del modulo con [limitazioni di vincolo](/it/docs/Web/HTML/Guides/Constraint_validation) possono essere targetizzati in base a questi stati.

### :valid e :invalid

Puoi targetizzare i controlli del modulo usando le pseudo-classi {{cssxref(":valid")}} e {{cssxref(":invalid")}}. Alcuni punti che vale la pena considerare:

- I controlli senza validazione dei vincoli saranno sempre validi e quindi abbinati con `:valid`.
- I controlli con `required` impostato su di loro che non hanno valore sono considerati non validi — saranno abbinati con `:invalid` e `:required`.
- I controlli con validazione incorporata, come `<input type="email">` o `<input type="url">` sono (abbinati con) `:invalid` quando i dati inseriti in essi non corrispondono al modello che stanno cercando (ma sono validi quando vuoti).
- I controlli il cui valore corrente è al di fuori dei limiti di intervallo specificati dagli attributi [`min`](/it/docs/Web/HTML/Reference/Elements/input#min) e [`max`](/it/docs/Web/HTML/Reference/Elements/input#max) sono (abbinati con) `:invalid`, ma anche abbinati a {{cssxref(":out-of-range")}}, come vedrai più avanti.
- Ci sono altri modi per fare in modo che un elemento sia abbinato da `:valid`/`:invalid`, come vedrai nell'articolo [Validazione del modulo lato client](/it/docs/Learn_web_development/Extensions/Forms/Form_validation). Ma per ora manterremo le cose semplici.

Entriamo e guardiamo un esempio di `:valid`/`:invalid` (vedi [valid-invalid.html](https://mdn.github.io/learning-area/html/forms/pseudo-classes/valid-invalid.html) per la versione live, e controlla anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/forms/pseudo-classes/valid-invalid.html)).

Come nell'esempio precedente, abbiamo extra `<span>` per generare contenuto su, che useremo per fornire indicatori di dati validi/invalidi:

```html
<div>
  <label for="fname">First name: </label>
  <input id="fname" name="fname" type="text" required />
  <span></span>
</div>
```

Per fornire questi indicatori, usiamo il seguente CSS:

```css
input + span {
  position: relative;
}

input + span::before {
  position: absolute;
  right: -20px;
  top: 5px;
}

input:invalid {
  border: 2px solid red;
}

input:invalid + span::before {
  content: "✖";
  color: red;
}

input:valid + span::before {
  content: "✓";
  color: green;
}
```

Come prima, impostiamo gli `<span>` su `position: relative` in modo da poter posizionare il contenuto generato in relazione ad essi. Poi posizioniamo assolutamente contenuti generati diversi a seconda che i dati dei moduli siano validi o non validi — un segno di spunta verde o una croce rossa, rispettivamente. Per aggiungere un po' di urgenza in più ai dati non validi, abbiamo anche dato agli input un bordo spesso rosso quando sono invalidi.

> [!NOTE]
> Abbiamo usato `::before` per aggiungere queste etichette, poiché stavamo già usando `::after` per le etichette "necessarie".

Puoi provarlo sotto:

{{EmbedGHLiveSample("learning-area/html/forms/pseudo-classes/valid-invalid.html", '100%', 430)}}

Nota come gli input di testo richiesti siano invalidi quando vuoti, ma validi quando hanno qualcosa compilato. L'input email invece è valido quando vuoto, poiché non è richiesto, ma non valido quando contiene qualcosa che non è un indirizzo email corretto.

### Dati in intervallo e fuori intervallo

Come accennato sopra, ci sono altre due pseudo-classi correlate da considerare — {{cssxref(":in-range")}} e {{cssxref(":out-of-range")}}. Queste abbinano input numerici in cui i limiti di intervallo sono specificati dagli attributi [`min`](/it/docs/Web/HTML/Reference/Elements/input#min) e [`max`](/it/docs/Web/HTML/Reference/Elements/input#max), quando i loro dati sono all'interno o all'esterno dell'intervallo specificato, rispettivamente.

> [!NOTE]
> I tipi di input numerici sono `date`, `month`, `week`, `time`, `datetime-local`, `number`, e `range`.

Vale la pena notare che gli input i cui dati sono in intervallo saranno anche abbinati dalla pseudo-classe `:valid` e gli input i cui dati sono fuori intervallo saranno anche abbinati dalla pseudo-classe `:invalid`. Allora perché avere entrambi? La questione è davvero una di semantica — fuori intervallo è un tipo più specifico di comunicazione non valida, quindi potresti voler fornire un messaggio diverso per input fuori intervallo, che sarà più utile per gli utenti piuttosto che dire semplicemente "non valido". Potresti anche voler fornire entrambi.

Diamo un'occhiata a un esempio che fa esattamente questo. La nostra demo [out-of-range.html](https://mdn.github.io/learning-area/html/forms/pseudo-classes/out-of-range.html) (vedi anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/forms/pseudo-classes/out-of-range.html)) si costruisce sull'esempio precedente per fornire messaggi fuori intervallo per gli input numerici, oltre a dire se sono richiesti.

L'input numerico appare così:

```html
<div>
  <label for="age">Age (must be 12+): </label>
  <input id="age" name="age" type="number" min="12" max="120" required />
  <span></span>
</div>
```

E il CSS appare così:

```css
input + span {
  position: relative;
}

input + span::after {
  font-size: 0.7rem;
  position: absolute;
  padding: 5px 10px;
  top: -26px;
}

input:required + span::after {
  color: white;
  background-color: black;
  content: "Required";
  left: -70px;
}

input:out-of-range + span::after {
  color: white;
  background-color: red;
  width: 155px;
  content: "Outside allowable value range";
  left: -182px;
}
```

Questa è una storia simile a quella che avevamo prima nell'esempio `:required`, tranne che qui abbiamo diviso le dichiarazioni che si applicano a qualsiasi contenuto `::after` in una regola separata, e dato il contentuto `::after` separato per gli stati `:required` e `:out-of-range` il proprio contenuto e stile. Puoi provarlo qui:

{{EmbedGHLiveSample("learning-area/html/forms/pseudo-classes/out-of-range.html", '100%', 430)}}

È possibile che l'input numerico sia sia richiesto sia fuori intervallo allo stesso tempo, quindi cosa succede allora? Poiché la regola `:out-of-range` appare più tardi nel codice sorgente rispetto alla regola `:required`, entrano in gioco le [regole della cascata](/it/docs/Learn_web_development/Core/Styling_basics/Handling_conflicts#understanding_the_cascade), e viene mostrato il messaggio di fuori intervallo.

Questo funziona abbastanza bene — quando la pagina viene caricata per la prima volta, viene mostrato "Richiesto", insieme a una croce rossa e un bordo. Quando hai digitato un'età valida (cioè, nell'intervallo di 12-120), l'input diventa valido. Se tuttavia poi cambi l'inserimento dell'età in uno che è fuori dall'intervallo, appare il messaggio "Al di fuori dell'intervallo di valori consentito" al posto di "Richiesto".

> [!NOTE]
> Per inserire un valore non valido/fuori intervallo, dovrai effettivamente focalizzare il modulo e digitarlo utilizzando la tastiera. I pulsanti spinner non ti permetteranno di incrementare/decrementare al di fuori dell'intervallo consentito.

## Stilizzazione degli input abilitati e disabilitati, e in modalità di sola lettura e lettura-scrittura

Un elemento abilitato è un elemento che può essere attivato; può essere selezionato, cliccato, digitato, ecc. Un elemento disabilitato invece non può essere interagito in alcun modo e i suoi dati non sono nemmeno inviati al server.

Questi due stati possono essere targetizzati utilizzando {{cssxref(":enabled")}} e {{cssxref(":disabled")}}. Perché gli input disabilitati sono utili? Beh, a volte se alcuni dati non si applicano a un determinato utente, potresti non voler nemmeno inviare quei dati quando inviano il modulo. Un esempio classico è un modulo di spedizione — comunemente verrà chiesto se si desidera utilizzare lo stesso indirizzo per fatturazione e spedizione; in tal caso, puoi semplicemente inviare un singolo indirizzo al server, e potresti anche solo disabilitare i campi dell'indirizzo di fatturazione.

Diamo un'occhiata a un esempio che fa proprio questo. Prima di tutto, l'HTML è un modulo semplice contenente input di testo, oltre a una casella di controllo per alternare l'attivazione/disattivazione dell'indirizzo di fatturazione. I campi dell'indirizzo di fatturazione sono disabilitati per impostazione predefinita.

```html
<form>
  <fieldset id="shipping">
    <legend>Shipping address</legend>
    <div>
      <label for="name1">Name: </label>
      <input id="name1" name="name1" type="text" required />
    </div>
    <div>
      <label for="address1">Address: </label>
      <input id="address1" name="address1" type="text" required />
    </div>
    <div>
      <label for="zip-code1">Zip/postal code: </label>
      <input id="zip-code1" name="zip-code1" type="text" required />
    </div>
  </fieldset>
  <fieldset id="billing">
    <legend>Billing address</legend>
    <div>
      <label for="billing-checkbox">Same as shipping address:</label>
      <input type="checkbox" id="billing-checkbox" checked />
    </div>
    <div>
      <label for="name" class="billing-label disabled-label">Name: </label>
      <input id="name" name="name" type="text" disabled required />
    </div>
    <div>
      <label for="address2" class="billing-label disabled-label">
        Address:
      </label>
      <input id="address2" name="address2" type="text" disabled required />
    </div>
    <div>
      <label for="zip-code2" class="billing-label disabled-label">
        Zip/postal code:
      </label>
      <input id="zip-code2" name="zip-code2" type="text" disabled required />
    </div>
  </fieldset>

  <div><button>Submit</button></div>
</form>
```

Ora passiamo al CSS. Le parti più rilevanti di questo esempio sono le seguenti:

```css
input[type="text"]:disabled {
  background: #eee;
  border: 1px solid #ccc;
}

label:has(+ :disabled) {
  color: #aaa;
}
```

Abbiamo selettivamente scelto gli input che vogliamo disabilitare utilizzando `input[type="text"]:disabled`, ma volevamo anche grigiare le etichette di testo corrispondenti. Poiché le etichette sono proprio prima dei loro input, le abbiamo selezionate utilizzando la pseudo-classe [`:has`](/it/docs/Web/CSS/:has).

Infine, abbiamo utilizzato un po' di JavaScript per alternare l'attivazione/disattivazione dei campi dell'indirizzo di fatturazione:

```js
// Wait for the page to finish loading
document.addEventListener(
  "DOMContentLoaded",
  () => {
    // Attach `change` event listener to checkbox
    document
      .getElementById("billing-checkbox")
      .addEventListener("change", toggleBilling);
  },
  false,
);

function toggleBilling() {
  // Select the billing text fields
  const billingItems = document.querySelectorAll('#billing input[type="text"]');

  // Toggle the billing text fields
  for (let i = 0; i < billingItems.length; i++) {
    billingItems[i].disabled = !billingItems[i].disabled;
  }
}
```

Usa l'[`evento change`](/it/docs/Web/API/HTMLElement/change_event) per permettere all'utente di abilitare/disabilitare i campi di fatturazione e alternare lo stile delle etichette associate.

Puoi vedere l'esempio in azione qui sotto (vedi anche il [live qui](https://mdn.github.io/learning-area/html/forms/pseudo-classes/enabled-disabled-shipping.html), e vedi il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/forms/pseudo-classes/enabled-disabled-shipping.html)):

{{EmbedGHLiveSample("learning-area/html/forms/pseudo-classes/enabled-disabled-shipping.html", '100%', 600)}}

### Sola lettura e lettura-scrittura

In modo simile a `:disabled` e `:enabled`, le pseudo-classi `:read-only` e `:read-write` targetizzano due stati tra cui gli input del modulo alternano. Come con gli input disabilitati, l'utente non può modificare gli input di sola lettura. Tuttavia, a differenza degli input disabilitati, i valori degli input di sola lettura saranno inviati al server. La lettura-scrittura significa che possono essere modificati — il loro stato predefinito.

Un'input è impostato su sola lettura utilizzando l'attributo `readonly`. Ad esempio, immagina una pagina di conferma dove lo sviluppatore ha inviato i dettagli compilati nelle pagine precedenti a questa pagina, con l'obiettivo di ottenere l'utente a controllarli tutti in un unico luogo, aggiungere eventuali dati finali necessari e poi confermare l'ordine inviando. A questo punto, tutti i dati finali del modulo possono essere inviati al server in un colpo solo.

Diamo un'occhiata a come potrebbe apparire un modulo (vedi [readonly-confirmation.html](https://mdn.github.io/learning-area/html/forms/pseudo-classes/readonly-confirmation.html) per l'esempio vivo; vedi anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/forms/pseudo-classes/readonly-confirmation.html)).

Un frammento dell'HTML è il seguente — nota l'attributo readonly:

```html
<div>
  <label for="name">Name: </label>
  <input id="name" name="name" type="text" value="Mr Soft" readonly />
</div>
```

Se provi l'esempio live, vedrai che il primo set di elementi del modulo non è modificabile, tuttavia, i valori sono inviati quando il modulo è completato. Abbiamo stilizzato i controlli del modulo utilizzando le pseudo-classi `:read-only` e `:read-write`, in questo modo:

```css
input:read-only,
textarea:read-only {
  border: 0;
  box-shadow: none;
  background-color: white;
}

textarea:read-write {
  box-shadow: inset 1px 1px 3px #ccc;
  border-radius: 5px;
}
```

L'esempio completo appare così:

{{EmbedGHLiveSample("learning-area/html/forms/pseudo-classes/readonly-confirmation.html", '100%', 660)}}

> **Nota:** `:enabled` e `:read-write` sono altre due pseudo-classi che probabilmente userai raramente, dato che descrivono gli stati di default degli elementi di input.

## Stati dei radio e delle caselle di controllo — checked, default, indeterminate

Come abbiamo visto negli articoli precedenti del modulo, {{HTMLElement("input/radio", "pulsanti radio")}} e {{HTMLElement("input/checkbox", "caselle di controllo")}} possono essere selezionati o deselezionati. Ma ci sono anche un paio di altri stati da considerare:

- {{cssxref(":default")}}: Abbina radio/caselle di controllo che sono selezionate per impostazione predefinita, al caricamento della pagina (cioè impostando l'attributo `checked` su di esse). Queste corrispondono alla pseudo-classe {{cssxref(":default")}}, anche se l'utente le deseleziona.
- {{cssxref(":indeterminate")}}: Quando radio/caselle di controllo non sono né selezionate né de-selezionate, sono considerate _indeterminate_ e corrisponderanno alla pseudo-classe {{cssxref(":indeterminate")}}. Maggiori dettagli su cosa questo significa di seguito.

### :checked

Quando selezionate, saranno corrisposte dalla pseudo-classe {{cssxref(":checked")}}.

L'uso più comune di questo è aggiungere uno stile diverso alla casella di controllo o al pulsante di opzione quando è selezionato, nei casi in cui hai rimosso lo stile predefinito del sistema con [`appearance: none;`](/it/docs/Web/CSS/appearance) e vuoi ricostruire gli stili da solo. Abbiamo visto esempi di questo nell'articolo precedente quando abbiamo parlato di [Utilizzo di `appearance: none` su radio/caselle](/it/docs/Learn_web_development/Extensions/Forms/Advanced_form_styling#using_appearance_none_on_radioscheckboxes).

Come promemoria, il codice `:checked` dal nostro esempio [Styled radio buttons](https://mdn.github.io/learning-area/html/forms/styling-examples/radios-styled.html) appare così:

```css
input[type="radio"]::before {
  display: block;
  content: " ";
  width: 10px;
  height: 10px;
  border-radius: 6px;
  background-color: red;
  font-size: 1.2em;
  transform: translate(3px, 3px) scale(0);
  transform-origin: center;
  transition: all 0.3s ease-in;
}

input[type="radio"]:checked::before {
  transform: translate(3px, 3px) scale(1);
  transition: all 0.3s cubic-bezier(0.25, 0.25, 0.56, 2);
}
```

Puoi provarlo qui:

{{EmbedGHLiveSample("learning-area/html/forms/styling-examples/radios-styled.html", '100%', 200)}}

Fondamentalmente, costruiamo lo stile per il "cerchio interno" di un pulsante radio usando lo pseudo-elemento `::before`, ma aggiungiamo una `scale(0)` [`transform`](/it/docs/Web/CSS/transform) su di esso. Usiamo quindi una [`transition`](/it/docs/Web/CSS/transition) per far animare il contenuto generato sull'etichetta in modo elegante quando il radio è selezionato/contrassegnato. Il vantaggio di usare un transform piuttosto che transitional [`width`](/it/docs/Web/CSS/width)/[`height`](/it/docs/Web/CSS/height) è che puoi usare [`transform-origin`](/it/docs/Web/CSS/transform-origin) per farlo crescere dal centro del cerchio, piuttosto che apparire come se crescesse dall'angolo del cerchio, e non c'è alcun comportamento di salto poiché nessun valore di proprietà del modello di casella è aggiornato.

### :default e :indeterminate

Come menzionato sopra, la pseudo-classe {{cssxref(":default")}} abbina radio/caselle di controllo selezionate per impostazione predefinita, al caricamento della pagina, anche quando deselezionate. Questo potrebbe essere utile per aggiungere un indicatore a una lista di opzioni per ricordare al'utente quali erano le impostazioni predefinite (o le opzioni iniziali), nel caso in cui vogliano ripristinare le loro scelte.

Inoltre, le radio/caselle di controllo menzionate sopra saranno abbinate dalla pseudo-classe {{cssxref(":indeterminate")}} quando sono in uno stato in cui non sono né selezionate né de-selezionate. Ma cosa significa questo? Gli elementi che sono indeterminati includono:

- {{HTMLElement("input/radio")}} inputs, quando tutti i pulsanti radio in un gruppo omonimo sono deselezionati
- {{HTMLElement("input/checkbox")}} inputs la cui proprietà `indeterminate` è impostata su `true` tramite JavaScript
- Elementi {{HTMLElement("progress")}} che non hanno valore.

Questo non è qualcosa che probabilmente utilizzerai molto spesso. Un caso d'uso potrebbe essere un indicatore per informare gli utenti che devono davvero selezionare un pulsante radio prima di procedere.

Diamo un'occhiata a un paio di versioni modificate dell'esempio precedente che ricordano all'utente quale fosse l'opzione predefinita e stilano le etichette dei pulsanti radio quando indeterminate. Entrambi hanno la seguente struttura HTML per gli input:

```html
<p>
  <input type="radio" name="fruit" value="cherry" id="cherry" />
  <label for="cherry">Cherry</label>
  <span></span>
</p>
```

Per l'esempio `:default`, abbiamo aggiunto l'attributo `checked` all'input del pulsante radio centrale, quindi sarà selezionato per impostazione predefinita quando caricato. Abbiamo quindi stilizzato questo con il seguente CSS:

```css
input ~ span {
  position: relative;
}

input:default ~ span::after {
  font-size: 0.7rem;
  position: absolute;
  content: "Default";
  color: white;
  background-color: black;
  padding: 5px 10px;
  right: -65px;
  top: -3px;
}
```

Questo fornisce una piccola etichetta "Default" sull'elemento che è stato originariamente selezionato quando la pagina è stata caricata. Nota qui stiamo usando il combinatore di successivo-con-fratello (`~`) piuttosto che il combinatore di subito fratello (`+`) — dobbiamo farlo perché lo `<span>` non viene subito dopo l'`<input>` nell'ordine del codice.

Vedi il risultato live qui sotto:

{{EmbedGHLiveSample("learning-area/html/forms/pseudo-classes/radios-checked-default.html", '100%', 200)}}

> [!NOTE]
> Puoi anche trovare l'esempio live su GitHub in [radios-checked-default.html](https://mdn.github.io/learning-area/html/forms/pseudo-classes/radios-checked-default.html) (vedi anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/forms/pseudo-classes/radios-checked-default.html).)

Per l'esempio `:indeterminate`, non abbiamo alcun pulsante radio selezionato per impostazione predefinita — questo è importante — se ci fosse, allora non ci sarebbe alcuno stato indeterminato da stilizzare. Stiliamo i pulsanti radio indeterminati con il seguente CSS:

```css
input[type="radio"]:indeterminate {
  outline: 2px solid red;
  animation: 0.4s linear infinite alternate outline-pulse;
}

@keyframes outline-pulse {
  from {
    outline: 2px solid red;
  }

  to {
    outline: 6px solid red;
  }
}
```

Questo crea un piccolo outline animato divertente sui pulsanti radio, che speriamo indichi che devi selezionare uno di essi!

Vedi il risultato live qui sotto:

{{EmbedGHLiveSample("learning-area/html/forms/pseudo-classes/radios-checked-indeterminate.html", '100%', 200)}}

> [!NOTE]
> Puoi anche trovare l'esempio live su GitHub in [radios-checked-indeterminate.html](https://mdn.github.io/learning-area/html/forms/pseudo-classes/radios-checked-indeterminate.html) (vedi anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/forms/pseudo-classes/radios-checked-indeterminate.html).)

> [!NOTE]
> Puoi trovare un [esempio interessante che coinvolge stati `indeterminate`](/it/docs/Web/HTML/Reference/Elements/input/checkbox#indeterminate_state_checkboxes) sulla pagina di riferimento [`<input type="checkbox">`](/it/docs/Web/HTML/Reference/Elements/input/checkbox).

## Altre pseudo-classi

Ci sono un certo numero di altre pseudo-classi di interesse, e non abbiamo spazio per scrivere di tutte in dettaglio qui. Parliamo di alcune altre che dovresti prendere il tempo di investigare.

- La pseudo-classe {{cssxref(":focus-within")}} abbina un elemento che ha ricevuto il focus o _contiene_ un elemento che ha ricevuto il focus. Questo è utile se vuoi che un intero modulo si evidenzi in qualche modo quando un input all'interno di esso è focalizzato.
- La pseudo-classe {{cssxref(":focus-visible")}} abbina elementi focalizzati che hanno ricevuto focus tramite interazione con la tastiera (piuttosto che tocco o mouse) — utile se vuoi mostrare uno stile diverso per il focus tramite tastiera rispetto al mouse (o altro) focus.
- La pseudo-classe {{cssxref(":placeholder-shown")}} abbina elementi {{htmlelement('input')}} e {{htmlelement('textarea')}} che hanno il loro placeholder mostrato (cioè, i contenuti dell'attributo [`placeholder`](/it/docs/Web/HTML/Reference/Elements/input#placeholder)) perché il valore dell'elemento è vuoto.

Le seguenti sono anche interessanti, ma finora non ben supportate nei browser:

- La pseudo-classe {{cssxref(":blank")}} seleziona controlli di modulo vuoti. Anche {{cssxref(":empty")}} abbina elementi che non hanno figli, come {{HTMLElement("input")}}, ma è più generale — abbina anche altri {{Glossary("void_element", "elementi vuoti")}} come {{HTMLElement("br")}} e {{HTMLElement("hr")}}. `:empty` ha un supporto browser ragionevole; la specifica della pseudo-classe `:blank` non è ancora finita, quindi non è ancora supportata in alcun browser.
- La pseudo-classe [`:user-invalid`](/it/docs/Web/CSS/:user-invalid), quando supportata, sarà simile a {{cssxref(":invalid")}}, ma con una migliore esperienza utente. Se il valore è valido quando l'input riceve il focus, l'elemento può abbinare `:invalid` man mano che l'utente inserisce dati se il valore è temporaneamente non valido, ma corrisponderà solo a `:user-invalid` quando l'elemento perde focus. Se il valore era originariamente non valido, corrisponderà sia a `:invalid` sia a `:user-invalid` per tutta la durata del focus. In modo simile a `:invalid`, smetterà di corrispondere a `:user-invalid` se il valore diventa valido.

## Metti alla prova le tue abilità!

Sei arrivato alla fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare alcuni ulteriori test per verificare di aver trattenuto queste informazioni prima di procedere — guarda [Metti alla prova le tue abilità: Stilizzazione avanzata](/it/docs/Learn_web_development/Extensions/Forms/Test_your_skills/Advanced_styling).

## Sommario

Questo completa il nostro sguardo alle pseudo-classi UI che riguardano gli input dei moduli. Continua a giocarci e crea alcuni stili di modulo divertenti! Prossimamente, passeremo a qualcosa di diverso — [validazione del modulo lato client](/it/docs/Learn_web_development/Extensions/Forms/Form_validation).

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/Customizable_select", "Learn_web_development/Extensions/Forms/Form_validation", "Learn_web_development/Extensions/Forms")}}
