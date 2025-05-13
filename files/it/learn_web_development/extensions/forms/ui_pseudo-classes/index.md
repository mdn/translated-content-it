---
title: Pseudo-classi UI
slug: Learn_web_development/Extensions/Forms/UI_pseudo-classes
l10n:
  sourceCommit: a1ac64fa4da965d2a152f08221b1a9aed638fd16
---

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/Customizable_select", "Learn_web_development/Extensions/Forms/Form_validation", "Learn_web_development/Extensions/Forms")}}

Negli articoli precedenti, abbiamo trattato la stilizzazione di vari controlli dei moduli in generale. Questo includeva l'uso di alcune pseudo-classi, ad esempio, utilizzando `:checked` per selezionare una casella di controllo solo quando è selezionata. In questo articolo, esploriamo le diverse pseudo-classi UI disponibili per stilizzare i moduli in diversi stati.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Una comprensione di base di
        <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, compresa una conoscenza generale delle
        <a
          href="/it/docs/Learn_web_development/Core/Styling_basics/Pseudo_classes_and_elements"
          >pseudo-classi e pseudo-elementi</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere quali parti dei moduli sono difficili da stilizzare, e perché; imparare
        cosa si può fare per personalizzarle.
      </td>
    </tr>
  </tbody>
</table>

## Quali pseudo-classi abbiamo a disposizione?

Le pseudo-classi originali (da [CSS 2.1](https://www.w3.org/TR/CSS21/selector.html#dynamic-pseudo-classes)) che sono rilevanti per i moduli sono:

- {{cssxref(":hover")}}: Seleziona un elemento solo quando è sorvolato dal puntatore del mouse.
- {{cssxref(":focus")}}: Seleziona un elemento solo quando è focalizzato (cioè quando è stato selezionato tramite la tastiera).
- {{cssxref(":active")}}: Seleziona un elemento solo quando viene attivato (cioè mentre viene cliccato, o quando il tasto <kbd>Return</kbd> / <kbd>Enter</kbd> è premuto nel caso di un'attivazione tramite tastiera).

Queste pseudo-classi di base dovrebbero esservi ormai familiari. [Selettori CSS](/it/docs/Web/CSS/CSS_selectors) forniscono diverse altre pseudo-classi relative ai moduli HTML. Questi offrono diverse condizioni utili di targeting che si possono sfruttare. Ne parleremo più dettagliatamente nelle sezioni seguenti, ma brevemente, le principali che esamineremo sono:

- {{cssxref(':required')}} e {{cssxref(':optional')}}: Seleziona elementi che possono essere necessari (ad esempio, elementi che supportano l'attributo HTML [`required`](/it/docs/Web/HTML/Reference/Attributes/required)), in base al fatto che siano obbligatori o facoltativi.
- {{cssxref(":valid")}} e {{cssxref(":invalid")}}, e {{cssxref(":in-range")}} e {{cssxref(":out-of-range")}}: Seleziona i controlli che sono validi/invalidi secondo i vincoli di convalida impostati su di loro, o nella gamma/oltre la gamma.
- {{cssxref(":enabled")}} e {{cssxref(":disabled")}}, e {{cssxref(":read-only")}} e {{cssxref(":read-write")}}: Seleziona elementi che possono essere disabilitati (ad esempio, elementi che supportano l'attributo HTML [`disabled`](/it/docs/Web/HTML/Reference/Attributes/disabled)), in base al fatto che siano attualmente abilitati o disabilitati, e controlli di scrittura-lettura o di sola lettura (ad esempio, elementi con l'attributo HTML [`readonly`](/it/docs/Web/HTML/Reference/Attributes/readonly) impostato).
- {{cssxref(":checked")}}, {{cssxref(":indeterminate")}}, e {{cssxref(":default")}}: Rispettivamente seleziona le caselle di controllo e i pulsanti radio che sono selezionati, in uno stato indeterminato (né selezionati né non selezionati), e l'opzione selezionata per impostazione predefinita quando la pagina viene caricata (ad esempio, un [`<input type="checkbox">`](/it/docs/Web/HTML/Reference/Elements/input/checkbox) con l'attributo [`checked`](/it/docs/Web/HTML/Reference/Elements/input#checked) impostato, o un elemento [`<option>`](/it/docs/Web/HTML/Reference/Elements/option) con l'attributo [`selected`](/it/docs/Web/HTML/Reference/Elements/option#selected) impostato).

Ci sono molte altre, ma quelle elencate sopra sono le più ovviamente utili. Alcune di esse sono destinate a risolvere problemi di nicchia molto specifici. Le pseudo-classi UI elencate sopra hanno un ottimo supporto da parte dei browser, ma naturalmente, è consigliato testare attentamente la loro implementazione nei moduli per garantire che funzionino per il suo pubblico di destinazione.

> [!NOTE]
> Un numero di pseudo-classi discusse qui si occupano di stilizzare i controlli dei moduli in base al loro stato di convalida (i loro dati sono validi, o no?) Imparerà molto di più sui vincoli di convalida e di controllo nel nostro prossimo articolo — [Validazione del modulo lato client](/it/docs/Learn_web_development/Extensions/Forms/Form_validation) — ma per ora terremo le cose semplici riguardo alla convalida del modulo, in modo da non creare confusione.

## Stilizzare gli input in base a se sono obbligatori o meno

Uno dei concetti più basilari riguardanti la convalida dei moduli lato client è se un input del modulo è obbligatorio (deve essere compilato prima che il modulo possa essere inviato) o facoltativo.

Gli elementi {{htmlelement('input')}}, {{htmlelement('select')}}, e {{htmlelement('textarea')}} hanno un attributo `required` disponibile che, quando impostato, significa che dovrà compilare quel controllo prima che il modulo sia inviato con successo. Ad esempio, il nome e il cognome sono obbligatori nel modulo sottostante, ma l'indirizzo email è facoltativo:

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

Può confrontare questi due stati usando le pseudo-classi {{cssxref(':required')}} e {{cssxref(':optional')}}. Ad esempio, se applichiamo il seguente CSS all'HTML sopra:

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

I controlli obbligatori hanno un bordo solido, e il controllo facoltativo ha un bordo tratteggiato.
Può anche provare a inviare il modulo senza compilarlo, per vedere i messaggi di errore di convalida lato client che i browser le forniscono di default:

{{EmbedLiveSample("optional-required-styles", , "400px", , , , , "allow-forms")}}

In generale, si dovrebbe evitare di stilizzare elementi 'obbligatori' contro 'facoltativi' nei moduli usando solo colori, perché questo non è ottimale per le persone daltoniche:

```css example-bad
input:required {
  border: 2px solid red;
}

input:optional {
  border: 2px solid green;
}
```

La convenzione standard sul web per lo stato obbligatorio è un asterisco (`*`), o la parola "obbligatorio" associata ai rispettivi controlli.
Nella prossima sezione, esamineremo un esempio migliore di indicazione dei campi obbligatori usando `:required` e contenuto generato.

> [!NOTE]
> Probabilmente non troverà spesso l'occasione di usare la pseudo-classe `:optional`. I controlli dei moduli sono facoltativi di default, quindi potrebbe semplicemente fare il suo styling facoltativo di default e aggiungere stili sopra per i controlli obbligatori.

> [!NOTE]
> Se un pulsante radio in un gruppo di pulsanti radio con lo stesso nome ha l'attributo `required` impostato, tutti i pulsanti radio saranno invalidi fino a quando uno di essi non sarà selezionato, ma solo quello con l'attributo assegnato verrà effettivamente selezionato con {{cssxref(':required')}}.

## Usare contenuto generato con pseudo-classi

In articoli precedenti, abbiamo visto l'uso del [contenuto generato](/it/docs/Web/CSS/CSS_generated_content), ma ora pensiamo che sia un buon momento per parlarne un po' più dettagliatamente.

L'idea è che possiamo usare gli pseudo-elementi [`::before`](/it/docs/Web/CSS/::before) e [`::after`](/it/docs/Web/CSS/::after) insieme alla proprietà [`content`](/it/docs/Web/CSS/content) per far apparire un pezzo di contenuto prima o dopo l'elemento interessato. Il pezzo di contenuto non viene aggiunto al DOM, quindi potrebbe essere invisibile ad alcuni screen reader. Poiché è un pseudo-elemento, può essere selezionato con stili nello stesso modo in cui può esserlo qualsiasi nodo reale del DOM.

Questo è davvero utile quando si vuole aggiungere un indicatore visivo a un elemento, come un'etichetta o un'icona, quando sono disponibili indicatori alternativi per garantire l'accessibilità a tutti gli utenti. Ad esempio, nel nostro [esempio di pulsanti radio personalizzati](https://mdn.github.io/learning-area/html/forms/styling-examples/radios-styled.html), usiamo contenuto generato per gestire il posizionamento e l'animazione del cerchio interno del pulsante radio personalizzato quando un pulsante radio è selezionato:

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

Questo è davvero utile — gli screen reader già avvisano i loro utenti quando un pulsante radio o una casella di controllo che incontrano è selezionata, quindi non vuole che leggano anche un altro elemento DOM che indica la selezione — potrebbe essere confusivo. Avere un indicatore puramente visivo risolve questo problema.

Non tutti i tipi di `<input>` supportano l'aggiunta di contenuto generato. Tutti i tipi di input che mostrano testo dinamico al loro interno, come `text`, `password`, o `button`, non mostrano contenuto generato. Altri, inclusi `range`, `color`, `checkbox`, ecc., mostrano contenuto generato.

Tornando al nostro esempio precedente di obbligatorio/facoltativo, questa volta non altereremo l'aspetto dell'input stesso — useremo contenuto generato per aggiungere un'etichetta indicante ([vedi qui dal vivo](https://mdn.github.io/learning-area/html/forms/pseudo-classes/required-optional-generated.html), e vedi il [codice sorgente qui](https://github.com/mdn/learning-area/blob/main/html/forms/pseudo-classes/required-optional-generated.html)).

Per primo, aggiungeremo un paragrafo in cima al modulo per spiegare cosa sta cercando:

```html
<p>Required fields are labeled with "required".</p>
```

Gli utenti con screen reader sentiranno "obbligatorio" letto come un'informazione aggiuntiva quando arriverà a ciascun input obbligatorio, mentre gli utenti vedenti vedranno la nostra etichetta.

Come già accennato, gli input di testo non supportano contenuto generato, quindi aggiungiamo uno [`<span>`](/it/docs/Web/HTML/Reference/Elements/span) vuoto per appendere il contenuto generato:

```html
<div>
  <label for="fname">First name: </label>
  <input id="fname" name="fname" type="text" required />
  <span></span>
</div>
```

Il problema immediato con questo era che lo span cadeva su una nuova linea sotto l'input perché sia l'input sia l'etichetta erano entrambi impostati con `width: 100%`. Per correggere questo, stilizziamo il tag genitore `<div>` per diventare un contenitore flessibile, ma gli diciamo anche di avvolgere i suoi contenuti su nuove linee se il contenuto diventa troppo lungo:

```css
fieldset > div {
  margin-bottom: 20px;
  display: flex;
  flex-flow: row wrap;
}
```

L'effetto che ha è che l'etichetta e l'input si trovano su linee separate perché sono entrambi `width: 100%`, ma lo `<span>` ha una larghezza di `0` quindi può stare sulla stessa linea dell'input.

Passando ora al contenuto generato. Lo creiamo usando questo CSS:

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

Impostiamo lo `<span>` su `position: relative` in modo che possiamo posizionare il contenuto generato su `position: absolute` e posizionarlo in relazione allo `<span>` piuttosto che al `<body>` (Il contenuto generato agisce come se fosse un nodo figlio dell'elemento su cui è generato, ai fini del posizionamento).

Poi diamo al contenuto generato il contenuto "obbligatorio", che è ciò che volevamo che la nostra etichetta dicesse, e lo stiliamo e lo posizioniamo come desideriamo. Il risultato è visto sotto.

{{EmbedGHLiveSample("learning-area/html/forms/pseudo-classes/required-optional-generated.html", '100%', 430)}}

## Stilizzare i controlli in base alla validità dei loro dati

L'altro concetto davvero importante e fondamentale nella convalida dei moduli è se i dati di un controllo del modulo sono validi o no (nel caso di dati numerici, possiamo anche parlare di dati nella gamma e fuori dalla gamma). I controlli dei moduli con [limitazioni di vincolo](/it/docs/Web/HTML/Guides/Constraint_validation) possono essere selezionati in base a questi stati.

### :valid e :invalid

Può selezionare i controlli dei moduli usando le pseudo-classi {{cssxref(":valid")}} e {{cssxref(":invalid")}}. Alcuni punti da tenere a mente:

- I controlli senza convalida di vincoli saranno sempre validi, e quindi abbinati con `:valid`.
- I controlli con `required` impostato su di essi che non hanno un valore vengono considerati non validi — saranno abbinati con `:invalid` e `:required`.
- I controlli con convalida incorporata, come `<input type="email">` o `<input type="url">` sono (abbinati con) `:invalid` quando i dati inseriti in essi non corrispondono al pattern che cercano (ma sono validi quando vuoti).
- I controlli il cui attuale valore è al di fuori dei limiti di gamma specificati dagli attributi [`min`](/it/docs/Web/HTML/Reference/Elements/input#min) e [`max`](/it/docs/Web/HTML/Reference/Elements/input#max) sono (abbinati con) `:invalid`, ma anche abbinati da {{cssxref(":out-of-range")}}, come vedrà più avanti.
- Ci sono alcuni altri modi per fare in modo che un elemento sia abbinato da `:valid`/`:invalid`, come vedrà nell'articolo [Validazione del modulo lato client](/it/docs/Learn_web_development/Extensions/Forms/Form_validation). Ma per ora terremo le cose semplici.

Vediamo un esempio di `:valid`/`:invalid` (veda [valid-invalid.html](https://mdn.github.io/learning-area/html/forms/pseudo-classes/valid-invalid.html) per la versione dal vivo, e anche controlli il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/forms/pseudo-classes/valid-invalid.html)).

Come nell'esempio precedente, abbiamo extra `<span>`s per generare contenuto su cui useremo per fornire indicatori di dati validi/invalidi:

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

Come prima, impostiamo gli `<span>`s su `position: relative` in modo che possiamo posizionare il contenuto generato in relazione a loro. Poi posizioniamo assolutamente contenuto generato differente a seconda che i dati del modulo siano validi o non validi — una spunta verde o una croce rossa, rispettivamente. Per aggiungere un po' di urgenza extra ai dati non validi, abbiamo anche dato agli input un bordo rosso spesso quando invalidi.

> [!NOTE]
> Abbiamo usato `::before` per aggiungere queste etichette, poiché stavamo già usando `::after` per le etichette "obbligatorie".

Può provarlo qui sotto:

{{EmbedGHLiveSample("learning-area/html/forms/pseudo-classes/valid-invalid.html", '100%', 430)}}

Noti come i test di testo obbligatori sono invalidi quando vuoti, ma validi quando hanno qualcosa di riempito. L'input email, d'altra parte, è valido quando vuoto, poiché non è obbligatorio, ma non valido quando contiene qualcosa che non è un indirizzo email corretto.

### Dati nella gamma e fuori dalla gamma

Come accennato in precedenza, ci sono altre due pseudo-classi correlate da considerare — {{cssxref(":in-range")}} e {{cssxref(":out-of-range")}}. Queste selezionano input numerici dove i limiti di gamma sono specificati dagli attributi [`min`](/it/docs/Web/HTML/Reference/Elements/input#min) e [`max`](/it/docs/Web/HTML/Reference/Elements/input#max), quando i loro dati sono all'interno o fuori dalla gamma specificata, rispettivamente.

> [!NOTE]
> I tipi di input numerici sono `date`, `month`, `week`, `time`, `datetime-local`, `number`, e `range`.

Vale la pena notare che gli input i cui dati sono nella gamma saranno anche abbinati dalla pseudo-classe `:valid` e gli input i cui dati sono fuori dalla gamma saranno anche abbinati dalla pseudo-classe `:invalid`. Quindi perché avere entrambi? La questione è davvero una di semantica — fuori dalla gamma è un tipo più specifico di comunicazione di invalidità, quindi potrebbe voler fornire un messaggio diverso per gli input fuori gamma, che sarà più utile per gli utenti rispetto a semplicemente "non valido". Potrebbe persino voler fornire entrambi.

Diamo un'occhiata a un esempio che fa esattamente questo. La nostra demo [out-of-range.html](https://mdn.github.io/learning-area/html/forms/pseudo-classes/out-of-range.html) (veda anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/forms/pseudo-classes/out-of-range.html)) costruisce sull'esempio precedente per fornire messaggi fuori gamma per gli input numerici, oltre a dire se sono obbligatori.

L'input numerico sembra così:

```html
<div>
  <label for="age">Age (must be 12+): </label>
  <input id="age" name="age" type="number" min="12" max="120" required />
  <span></span>
</div>
```

E il CSS sembra così:

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

Questa è una storia simile a quella che avevamo prima nell'esempio `:required`, tranne che qui abbiamo suddiviso le dichiarazioni che si applicano a qualsiasi contenuto `::after` in una regola separata, e abbiamo dato i contenuti `::after` separati per gli stati `:required` e `:out-of-range` i loro contenuti e stili. Può provarlo qui:

{{EmbedGHLiveSample("learning-area/html/forms/pseudo-classes/out-of-range.html", '100%', 430)}}

È possibile per l'input di numero essere sia obbligatorio che fuori gamma allo stesso tempo, quindi cosa succede allora? Poiché la regola `:out-of-range` appare più tardi nel codice sorgente rispetto alla regola `:required`, le [regole della cascata](/it/docs/Learn_web_development/Core/Styling_basics/Handling_conflicts#understanding_the_cascade) entrano in gioco, e viene mostrato il messaggio fuori gamma.

Funziona abbastanza bene — quando la pagina viene caricata per la prima volta, viene mostrato "Obbligatorio", insieme a una croce rossa e un bordo. Quando ha digitato un'età valida (cioè nell'intervallo di 12-120), l'input diventa valido. Se tuttavia cambia l'età in un valore non valido, il messaggio "Fuori dalla gamma consentita" quindi appare al posto di "Obbligatorio".

> [!NOTE]
> Per inserire un valore non valido/fuori gamma, dovrà effettivamente focalizzare il modulo e digitarlo usando la tastiera. I pulsanti di incremento non le consentiranno di aumentare/diminuire il valore fuori dalla gamma consentita.

## Stilizzare gli input abilitati e disabilitati, e di sola lettura e di lettura e scrittura

Un elemento abilitato è un elemento che può essere attivato; può essere selezionato, cliccato, digitato, ecc. Un elemento disabilitato, d'altra parte, non può essere interagito in alcun modo, e i suoi dati non vengono neanche inviati al server.

Questi due stati possono essere selezionati usando {{cssxref(":enabled")}} e {{cssxref(":disabled")}}. Perché sono utili gli input disabilitati? Beh, a volte, se alcuni dati non si applicano a un certo utente, potrebbe non voler neanche inviare quei dati quando inviano il modulo. Un esempio classico è un modulo di spedizione — comunemente verrà chiesto se si desidera usare lo stesso indirizzo per la fatturazione e la spedizione; se è così, può semplicemente inviare un solo indirizzo al server e, magari, disabilitare i campi dell'indirizzo di fatturazione.

Diamo un'occhiata a un esempio che fa proprio questo. Prima di tutto, l'HTML è un modulo semplice che contiene input di testo, più una casella di controllo per attivare/disattivare la disabilitazione dell'indirizzo di fatturazione. I campi dell'indirizzo di fatturazione sono disabilitati per default.

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

Passiamo ora al CSS. Le parti più rilevanti di questo esempio sono le seguenti:

```css
input[type="text"]:disabled {
  background: #eee;
  border: 1px solid #ccc;
}

label:has(+ :disabled) {
  color: #aaa;
}
```

Abbiamo selezionato direttamente gli input che volevamo disabilitare usando `input[type="text"]:disabled`, ma volevamo anche in grigio anche le etichette di testo corrispondenti. Poiché le etichette sono proprio prima dei loro input, le abbiamo selezionate usando la pseudo-classe [`:has`](/it/docs/Web/CSS/:has).

Infine, abbiamo usato un po' di JavaScript per attivare/disattivare la disabilitazione dei campi dell'indirizzo di fatturazione:

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

Usa l'[evento `change`](/it/docs/Web/API/HTMLElement/change_event) per consentire all'utente di abilitare/disabilitare i campi di fatturazione e attivare/disattivare lo stile delle etichette associate.

Può vedere l'esempio in azione qui sotto (veda anche [questa versione live](https://mdn.github.io/learning-area/html/forms/pseudo-classes/enabled-disabled-shipping.html), e veda il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/forms/pseudo-classes/enabled-disabled-shipping.html)):

{{EmbedGHLiveSample("learning-area/html/forms/pseudo-classes/enabled-disabled-shipping.html", '100%', 600)}}

### Sola lettura e lettura e scrittura

In modo simile a `:disabled` e `:enabled`, le pseudo-classi `:read-only` e `:read-write` selezionano due stati che gli input del modulo alternano. Come gli input disabilitati, l'utente non può modificare gli input di sola lettura. Tuttavia, a differenza degli input disabilitati, i valori degli input di sola lettura saranno inviati al server. La lettura e scrittura significa che possono essere modificati — il loro stato predefinito.

Un input è impostato su sola lettura utilizzando l'attributo `readonly`. Come esempio, immagini una pagina di conferma in cui lo sviluppatore ha inviato i dettagli compilati su pagine precedenti a questa pagina, con l'obiettivo di far controllare l'utente tutti in un unico luogo, aggiungere eventuali dati finali che sono necessari, e quindi confermare l'ordine inviando. A questo punto, tutti i dati finali del modulo possono essere inviati al server in una sola volta.

Vediamo come potrebbe apparire un modulo (veda [readonly-confirmation.html](https://mdn.github.io/learning-area/html/forms/pseudo-classes/readonly-confirmation.html) per l'esempio dal vivo; veda anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/forms/pseudo-classes/readonly-confirmation.html)).

Un frammento dell'HTML è il seguente — noti l'attributo readonly:

```html
<div>
  <label for="name">Name: </label>
  <input id="name" name="name" type="text" value="Mr Soft" readonly />
</div>
```

Se prova l'esempio dal vivo, vedrà che il set superiore di elementi del modulo non è modificabile, comunque, i valori sono inviati quando si invia il modulo. Abbiamo stilizzato i controlli del modulo usando le pseudo-classi `:read-only` e `:read-write`, in questo modo:

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

L'esempio completo sembra così:

{{EmbedGHLiveSample("learning-area/html/forms/pseudo-classes/readonly-confirmation.html", '100%', 660)}}

> **Nota:** `:enabled` e `:read-write` sono due pseudo-classi che probabilmente non userà spesso, dato che descrivono gli stati predefiniti degli elementi di input.

## Stati dei radio e checkbox — selezionato, predefinito, indeterminato

Come abbiamo visto in articoli precedenti del modulo, {{HTMLElement("input/radio", "pulsanti radio")}} e {{HTMLElement("input/checkbox", "checkbox")}} possono essere selezionati o non selezionati. Ma ci sono un paio di altri stati da considerare anche:

- {{cssxref(":default")}}: Seleziona radio/checkbox che sono selezionati di default, al caricamento della pagina (cioè impostando l'attributo `checked` su di essi). Questi abbinano la pseudo-classe {{cssxref(":default")}}, anche se l'utente li deseleziona.
- {{cssxref(":indeterminate")}}: Quando radio/checkbox non sono né selezionati né deselezionati, sono considerati _indeterminati_ e saranno abbinati alla pseudo-classe {{cssxref(":indeterminate")}}. Più su cosa significa questo sotto.

### :checked

Quando selezionati, saranno abbinati alla pseudo-classe {{cssxref(":checked")}}.

L'uso più comune di questo è aggiungere uno stile diverso alla casella di verifica o pulsante radio quando è selezionato, nei casi in cui hai rimosso lo stile predefinito del sistema con [`appearance: none;`](/it/docs/Web/CSS/appearance) e desideri ricostruire gli stili da solo. Abbiamo visto esempi di questo nell'articolo precedente, quando abbiamo parlato di [Usare `appearance: none` su radio/checkbox](/it/docs/Learn_web_development/Extensions/Forms/Advanced_form_styling#using_appearance_none_on_radioscheckboxes).

Come riassunto, il codice `:checked` dal nostro esempio di [Pulsanti radio stilizzati](https://mdn.github.io/learning-area/html/forms/styling-examples/radios-styled.html) appare in questo modo:

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

Può provarlo qui:

{{EmbedGHLiveSample("learning-area/html/forms/styling-examples/radios-styled.html", '100%', 200)}}

Fondamentalmente, costruiamo lo stile per il "cerchio interno" di un pulsante radio usando lo pseudo-elemento `::before`, ma impostiamo un `scale(0)` [`transform`](/it/docs/Web/CSS/transform) su di esso. Usiamo quindi un [`transition`](/it/docs/Web/CSS/transition) per far sì che il contenuto generato sull'etichetta si animi dolcemente nella vista quando il radio è selezionato/controllato. Il vantaggio di usare un trasform invece che una transizione [`width`](/it/docs/Web/CSS/width)/[`height`](/it/docs/Web/CSS/height) è che può usare [`transform-origin`](/it/docs/Web/CSS/transform-origin) per farlo crescere dal centro del cerchio, piuttosto che farlo apparire crescere dall'angolo del cerchio, e non ci sono comportamenti di salto poiché nessun valore delle proprietà del modello di scatola viene aggiornato.

### :default e :indeterminate

Come accennato sopra, la pseudo-classe {{cssxref(":default")}} abbina radio/checkbox che sono selezionati di default, al caricamento della pagina, anche quando deselezionati. Questo potrebbe essere utile per aggiungere un indicatore a un elenco di opzioni per ricordare all'utente quali erano le opzioni predefinite (o iniziali), nel caso in cui desiderano azzerare le loro scelte.

Inoltre, i radio/checkbox menzionati sopra saranno abbinati dalla pseudo-classe {{cssxref(":indeterminate")}} quando si trovano in uno stato in cui non sono né selezionati né deselezionati. Ma cosa significa questo? Gli elementi che sono indeterminati includono:

- input {{HTMLElement("input/radio")}}, quando tutti i pulsanti radio in un gruppo con lo stesso nome sono deselezionati
- input {{HTMLElement("input/checkbox")}} la cui proprietà `indeterminate` è impostata su `true` tramite JavaScript
- elementi {{HTMLElement("progress")}} che non hanno un valore.

Questo non è qualcosa che probabilmente userà molto spesso. Un caso d'uso potrebbe essere un indicatore per dire agli utenti che hanno davvero bisogno di selezionare un pulsante radio prima di procedere oltre.

Diamo un'occhiata a un paio di versioni modificate dell'esempio precedente che ricordano all'utente quale era l'opzione di default, e stilizzano le etichette dei pulsanti radio quando sono indeterminate. Entrambe hanno la seguente struttura HTML per gli input:

```html
<p>
  <input type="radio" name="fruit" value="cherry" id="cherry" />
  <label for="cherry">Cherry</label>
  <span></span>
</p>
```

Per l'esempio `:default`, abbiamo aggiunto l'attributo `checked` al pulsante radio di mezzo, quindi sarà selezionato di default quando caricato. Abbiamo quindi stilizzato questo con il seguente CSS:

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

Questo fornisce una piccola etichetta "Predefinito" sull'elemento originariamente selezionato quando la pagina è stata caricata. Noti qui che stiamo usando il combinatore successore di filiale (`~`) piuttosto che il combinatore di prossimo-filiale (`+`) — dobbiamo farlo perché lo `<span>` non viene subito dopo il `<input>` nell'ordine sorgente.

Veda il risultato dal vivo qui sotto:

{{EmbedGHLiveSample("learning-area/html/forms/pseudo-classes/radios-checked-default.html", '100%', 200)}}

> [!NOTE]
> Può anche trovare esempio dal vivo su GitHub a [radios-checked-default.html](https://mdn.github.io/learning-area/html/forms/pseudo-classes/radios-checked-default.html) (vedi anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/forms/pseudo-classes/radios-checked-default.html).)

Per l'esempio `:indeterminate`, non abbiamo nessun pulsante radio selezionato di default — questo è importante — se ci fosse, allora non ci sarebbe stato alcuno stato indeterminato da stilizzare. Stilizziamo i pulsanti radio indeterminati con il seguente CSS:

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

Questo crea una divertente piccola outline animata sui pulsanti radio, che speriamo indicano che ne deve selezionare uno!

Veda il risultato dal vivo qui sotto:

{{EmbedGHLiveSample("learning-area/html/forms/pseudo-classes/radios-checked-indeterminate.html", '100%', 200)}}

> [!NOTE]
> Può anche trovare esempio dal vivo su GitHub a [radios-checked-indeterminate.html](https://mdn.github.io/learning-area/html/forms/pseudo-classes/radios-checked-indeterminate.html) (vedi anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/forms/pseudo-classes/radios-checked-indeterminate.html).)

> [!NOTE]
> Può trovare un [esempio interessante che coinvolge stati `indeterminate`](/it/docs/Web/HTML/Reference/Elements/input/checkbox#indeterminate_state_checkboxes) sulla pagina di riferimento [`<input type="checkbox">`](/it/docs/Web/HTML/Reference/Elements/input/checkbox).

## Altre pseudo-classi

Ci sono un numero di altre pseudo-classi di interesse e non abbiamo spazio per scriverne tutte in dettaglio qui. Parliamo di alcune altre che dovrebbe prendersi il tempo di investigare:

- La pseudo-classe {{cssxref(":focus-within")}} seleziona un elemento che ha ricevuto il focus o _contiene_ un elemento che ha ricevuto il focus. Questo è utile se desidera che un intero modulo evidenzi in qualche modo quando un input al suo interno è focalizzato.
- La pseudo-classe {{cssxref(":focus-visible")}} seleziona elementi focalizzati che hanno ricevuto il focus tramite interazione da tastiera (piuttosto che touch o mouse) — utile se desidera mostrare uno stile diverso per il focus da tastiera rispetto al focus mouse (o altro).
- La pseudo-classe {{cssxref(":placeholder-shown")}} seleziona elementi {{htmlelement('input')}} e {{htmlelement('textarea')}} che mostrano il loro placeholder (cioè il contenuto dell'attributo [`placeholder`](/it/docs/Web/HTML/Reference/Elements/input#placeholder)) perché il valore dell'elemento è vuoto.

I seguenti sono anche interessanti, ma non sono ancora ben supportati nei browser:

- La pseudo-classe {{cssxref(":blank")}} seleziona i controlli del modulo vuoti. {{cssxref(":empty")}} abbina anche elementi che non hanno figli, come {{HTMLElement("input")}}, ma è più generale — abbina anche altri {{Glossary("void_element", "elementi vuoti")}} come {{HTMLElement("br")}} e {{HTMLElement("hr")}}. `:empty` ha un supporto ragionevole nei browser; la pseudo-classe `:blank` non è ancora finita nelle specifiche, quindi non è ancora supportata in alcun browser.
- La pseudo-classe [`:user-invalid`](/it/docs/Web/CSS/:user-invalid), quando supportata, sarà simile a {{cssxref(":invalid")}}, ma con un'esperienza utente migliore. Se il valore è valido quando l'input riceve focus, l'elemento può abbinare `:invalid` mentre l'utente inserisce i dati se il valore è temporaneamente non valido, ma sarà solo abbinato da `:user-invalid` quando l'elemento perde focus. Se il valore era originariamente non valido, verrà abbinato sia da `:invalid` che da `:user-invalid` per tutta la durata del focus. In modo simile a `:invalid`, smetterà di abbinare `:user-invalid` se il valore diventa valido.

## Verifichi le sue abilità!

Ha raggiunto la fine di questo articolo, ma ricorda le informazioni più importanti? Può trovare alcuni ulteriori test per verificare di averle memorizzate prima di andare avanti — veda [Testa le tue abilità: Stile Avanzato](/it/docs/Learn_web_development/Extensions/Forms/Test_your_skills/Advanced_styling).

## Riassunto

Questo completa il nostro esame delle pseudo-classi UI relative agli input dei moduli. Continui a giocarci e crei alcuni stili di modulo divertenti! Successivamente, passeremo a qualcosa di diverso — [validazione del modulo lato client](/it/docs/Learn_web_development/Extensions/Forms/Form_validation).

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/Customizable_select", "Learn_web_development/Extensions/Forms/Form_validation", "Learn_web_development/Extensions/Forms")}}
