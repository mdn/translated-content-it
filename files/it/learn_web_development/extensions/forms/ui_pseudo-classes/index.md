---
title: Pseudo-classi UI
slug: Learn_web_development/Extensions/Forms/UI_pseudo-classes
l10n:
  sourceCommit: b5437b737639d6952d18b95ebd1045ed73e4bfa7
---

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/Customizable_select", "Learn_web_development/Extensions/Forms/Form_validation", "Learn_web_development/Extensions/Forms")}}

Negli articoli precedenti, abbiamo trattato la stilizzazione di vari controlli dei moduli in modo generale. Questo ha incluso l'uso di alcune pseudo-classi, ad esempio utilizzando `:checked` per targetizzare una checkbox solo quando è selezionata. In questo articolo, esploriamo le diverse pseudo-classi UI disponibili per stilizzare i moduli in diversi stati.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Una comprensione di base di
        <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, inclusa la conoscenza generale di
        <a
          href="/it/docs/Learn_web_development/Core/Styling_basics/Pseudo_classes_and_elements"
          >pseudo-classi e pseudo-elementi</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Capire quali parti dei moduli sono difficili da stilizzare, e perché; imparare
        cosa si può fare per personalizzarli.
      </td>
    </tr>
  </tbody>
</table>

## Quali pseudo-classi abbiamo a disposizione?

Le pseudo-classi originali (da [CSS 2.1](https://www.w3.org/TR/CSS21/selector.html#dynamic-pseudo-classes)) che sono rilevanti per i moduli sono:

- {{cssxref(":hover")}}: Seleziona un elemento solo quando è sovrastato dal puntatore del mouse.
- {{cssxref(":focus")}}: Seleziona un elemento solo quando è in focus (cioè, selezionato tramite il tab dalla tastiera).
- {{cssxref(":active")}}: seleziona un elemento solo quando è attivato (cioè, mentre viene cliccato, o quando il tasto <kbd>Return</kbd> / <kbd>Enter</kbd> è premuto nel caso di un'attivazione da tastiera).

Queste pseudo-classi di base dovrebbero esserti ormai familiari. [I selettori CSS](/it/docs/Web/CSS/CSS_selectors) offrono diverse altre pseudo-classi relative ai moduli HTML. Queste forniscono diverse condizioni utili di targetizzazione di cui puoi avvantaggiarti. Ne discuteremo in maggior dettaglio nelle sezioni seguenti, ma brevemente, le principali che esamineremo sono:

- {{cssxref(':required')}} e {{cssxref(':optional')}}: Mirare elementi che possono essere richiesti (ad esempio, elementi che supportano l'attributo HTML [`required`](/it/docs/Web/HTML/Reference/Attributes/required)), basandosi sul fatto che siano richiesti o opzionali.
- {{cssxref(":valid")}} e {{cssxref(":invalid")}}, e {{cssxref(":in-range")}} e {{cssxref(":out-of-range")}}: Mirare controlli di modulo che sono validi/invalidi secondo i vincoli di convalida del modulo imposti, o dentro/fuori dal range.
- {{cssxref(":enabled")}} e {{cssxref(":disabled")}}, e {{cssxref(":read-only")}} e {{cssxref(":read-write")}}: Mirare elementi che possono essere disabilitati (ad esempio, elementi che supportano l'attributo HTML [`disabled`](/it/docs/Web/HTML/Reference/Attributes/disabled)), in base al fatto che siano attualmente abilitati o disabilitati, e controlli di modulo in lettura/scrittura o sola lettura (ad esempio, elementi con l'attributo HTML [`readonly`](/it/docs/Web/HTML/Reference/Attributes/readonly) impostato).
- {{cssxref(":checked")}}, {{cssxref(":indeterminate")}}, e {{cssxref(":default")}}: Rispettiamente, mirare checkboxes e pulsanti radio che sono selezionati, in uno stato indeterminato (né selezionati né non selezionati), e l'opzione predefinita selezionata quando la pagina viene caricata (ad esempio, un [`<input type="checkbox">`](/it/docs/Web/HTML/Reference/Elements/input/checkbox) con l'attributo [`checked`](/it/docs/Web/HTML/Reference/Elements/input#checked) impostato, o un elemento [`<option>`](/it/docs/Web/HTML/Reference/Elements/option) con l'attributo [`selected`](/it/docs/Web/HTML/Reference/Elements/option#selected) impostato).

Ce ne sono molti altri, ma quelli elencati sopra sono i più ovviamente utili. Alcuni di essi sono progettati per risolvere problemi di nicchia molto specifici. Le pseudo-classi UI elencate sopra hanno un eccellente supporto del browser, ma ovviamente, dovresti testare attentamente le tue implementazioni dei moduli per garantire che funzionino per il tuo pubblico di riferimento.

> [!NOTE]
> Un certo numero di pseudo-classi discusse qui sono interessate alla stilizzazione dei controlli di modulo in base al loro stato di convalida (i loro dati sono validi o no?). Imparerai molto di più sulla definizione e il controllo dei vincoli di convalida nel nostro prossimo articolo — [Validazione del modulo lato client](/it/docs/Learn_web_development/Extensions/Forms/Form_validation) — ma per ora terremo le cose semplici riguardo alla convalida dei moduli, così da non confondere le cose.

## Stilizzazione degli input in base al fatto che siano richiesti o meno

Uno dei concetti più fondamentali riguardanti la convalida dei moduli lato client è se un input di un modulo sia richiesto (deve essere compilato prima che il modulo possa essere inviato) o opzionale.

Gli elementi {{htmlelement('input')}}, {{htmlelement('select')}}, e {{htmlelement('textarea')}} hanno un attributo `required` disponibile che, quando impostato, significa che devi compilare quel controllo prima che il modulo venga inviato con successo.
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

Puoi corrispondere questi due stati usando le pseudo-classi {{cssxref(':required')}} e {{cssxref(':optional')}}. Ad esempio, se applichiamo il seguente CSS all'HTML sopra:

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
Puoi anche provare a inviare il modulo senza compilarlo, per vedere i messaggi di errore di convalida lato client che i browser ti forniscono di default:

{{EmbedLiveSample("optional-required-styles", , "400px", , , , , "allow-forms")}}

In generale, dovresti evitare di stilizzare elementi 'richiesti' rispetto a 'opzionali' nei moduli usando solo il colore, perché questo non è ottimo per le persone daltoniche:

```css example-bad
input:required {
  border: 2px solid red;
}

input:optional {
  border: 2px solid green;
}
```

La convenzione standard sul web per lo stato richiesto è un asterisco (`*`), o la parola "richiesto" associata ai rispettivi controlli.
Nella sezione successiva, vedremo un esempio migliore di indicazione dei campi richiesti utilizzando `:required` e contenuto generato.

> [!NOTE]
> Probabilmente non ti troverai ad usare spesso la pseudo-classe `:optional`. I controlli di modulo sono opzionali per default, quindi potresti semplicemente fare la tua stilizzazione opzionale di default, e aggiungere stili sopra per i controlli richiesti.

> [!NOTE]
> Se un pulsante radio di un gruppo di pulsanti radio con lo stesso nome ha l'attributo `required` impostato, tutti i pulsanti radio saranno invalidi fino a quando non ne viene selezionato uno, ma solo quello a cui l'attributo è assegnato corrisponderà effettivamente a {{cssxref(':required')}}.

## Uso del contenuto generato con pseudo-classi

Negli articoli precedenti, abbiamo visto l'uso del [contenuto generato](/it/docs/Web/CSS/CSS_generated_content), ma abbiamo pensato che ora sarebbe un buon momento per parlarne in modo più dettagliato.

L'idea è che possiamo usare gli pseudo-elementi [`::before`](/it/docs/Web/CSS/::before) e [`::after`](/it/docs/Web/CSS/::after) insieme alla proprietà [`content`](/it/docs/Web/CSS/content) per fare apparire un pezzo di contenuto prima o dopo l'elemento interessato. Il pezzo di contenuto non viene aggiunto al DOM, quindi può essere invisibile ad alcuni screen reader. Siccome è uno pseudo-elemento, può essere targetizzato con stili nello stesso modo in cui qualsiasi nodo DOM effettivo può esserlo.

Questo è davvero utile quando vuoi aggiungere un indicatore visivo a un elemento, come un'etichetta o un'icona, quando sono disponibili indicatori alternativi per garantire l'accessibilità a tutti gli utenti. Ad esempio, nel nostro [esempio di pulsanti radio personalizzati](https://mdn.github.io/learning-area/html/forms/styling-examples/radios-styled.html), usiamo il contenuto generato per gestire il posizionamento e l'animazione del cerchio interno del pulsante radio personalizzato quando un pulsante radio è selezionato:

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

Questo è davvero utile — i lettori di schermo già informano i loro utenti quando un pulsante radio o una checkbox che incontrano è selezionato/spuntato, quindi non vuoi che leggano un altro elemento DOM che indica la selezione — potrebbe confondere. Avere un indicatore puramente visivo risolve questo problema.

Non tutti i tipi di `<input>` supportano la presenza di contenuto generato su di essi. Tutti i tipi di input che mostrano testo dinamico in essi, come `text`, `password`, o `button`, non mostrano contenuto generato. Altri, inclusi `range`, `color`, `checkbox`, ecc., mostrano contenuto generato.

Tornando al nostro esempio richiesto/opzionale di prima, questa volta non altereremo l'aspetto dell'input stesso — useremo il contenuto generato per aggiungere un'etichetta indicativa ([vedi l'esempio in azione qui](https://mdn.github.io/learning-area/html/forms/pseudo-classes/required-optional-generated.html), e vedi il [codice sorgente qui](https://github.com/mdn/learning-area/blob/main/html/forms/pseudo-classes/required-optional-generated.html)).

Per prima cosa, aggiungeremo un paragrafo in cima al modulo per dire cosa stai cercando:

```html
<p>Required fields are labeled with "required".</p>
```

Gli utenti di lettori di schermo sentiranno "required" letto come un'informazione aggiuntiva quando arrivano a ciascun input richiesto, mentre gli utenti vedenti vedranno la nostra etichetta.

Come menzionato in precedenza, i text input non supportano il contenuto generato, quindi aggiungiamo uno [`<span>`](/it/docs/Web/HTML/Reference/Elements/span) vuoto su cui attaccare il contenuto generato:

```html
<div>
  <label for="fname">First name: </label>
  <input id="fname" name="fname" type="text" required />
  <span></span>
</div>
```

Il problema immediato con questo era che lo span stava cadendo su una nuova linea sotto l'input perché l'input e l'etichetta sono entrambi impostati con `width: 100%`. Per risolvere questo stile stile il `<div>` genitore per diventare un contenitore flessibile, ma anche indicargli di avvolgere i suoi contenuti su nuove righe se i contenuti diventano troppo lunghi:

```css
fieldset > div {
  margin-bottom: 20px;
  display: flex;
  flex-flow: row wrap;
}
```

L'effetto è che l'etichetta e l'input sedano su linee separate perché entrambi hanno `width: 100%`, ma lo `<span>` ha una larghezza di `0` quindi può sedersi sulla stessa linea dell'input.

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

Impostiamo lo `<span>` su `position: relative` in modo da poter impostare il contenuto generato su `position: absolute` e posizionarlo in relazione allo `<span>` piuttosto che al `<body>` (Il contenuto generato agisce come se fosse un nodo figlio dell'elemento su cui è generato, per quanto riguarda il posizionamento).

Poi diamo al contenuto generato il contenuto "required", che è quello che volevamo che la nostra etichetta dicesse, e lo stiliamo e lo posizioniamo come vogliamo. Il risultato è visibile qui sotto.

{{EmbedGHLiveSample("learning-area/html/forms/pseudo-classes/required-optional-generated.html", '100%', 430)}}

## Stilizzazione dei controlli in base al fatto che i loro dati siano validi

L'altro concetto davvero importante, fondamentale nella convalida dei moduli è se i dati di un controllo del modulo siano validi o meno (nel caso di dati numerici, possiamo anche parlare di dati dentro o fuori dal range). I controlli di modulo con [limitazioni di vincolo](/it/docs/Web/HTML/Guides/Constraint_validation) possono essere targetizzati basandosi su questi stati.

### :valid e :invalid

Puoi targetizzare i controlli di modulo usando le pseudo-classi {{cssxref(":valid")}} e {{cssxref(":invalid")}}. Alcuni punti che vale la pena tenere a mente:

- I controlli senza convalida del vincolo saranno sempre validi, e quindi corrisponderanno a `:valid`.
- I controlli con `required` impostato su di essi che non hanno un valore sono considerati invalidi — saranno corrisposti a `:invalid` e `:required`.
- I controlli con convalida incorporata, come `<input type="email">` o `<input type="url">` sono (corrisposti a) `:invalid` quando i dati immessi in essi non corrispondono al pattern che stanno cercando (ma sono validi quando vuoti).
- I controlli il cui valore corrente è al di fuori dei limiti di range specificati dagli attributi [`min`](/it/docs/Web/HTML/Reference/Elements/input#min) e [`max`](/it/docs/Web/HTML/Reference/Elements/input#max) sono (corrisposti a) `:invalid`, ma anche corrisposti a {{cssxref(":out-of-range")}}, come vedrai più avanti.
- Ci sono alcuni altri modi per fare in modo che un elemento venga corrisposto da `:valid`/`:invalid`, come vedrai nell'articolo [Validazione del modulo lato client](/it/docs/Learn_web_development/Extensions/Forms/Form_validation). Ma per ora terremo le cose semplici.

Andiamo a vedere un esempio di `:valid`/`:invalid` (vedi [valid-invalid.html](https://mdn.github.io/learning-area/html/forms/pseudo-classes/valid-invalid.html) per la versione live, e controlla anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/forms/pseudo-classes/valid-invalid.html)).

Come nell'esempio precedente, abbiamo aggiunto `<span>` extra per generare contenuto su, che useremo per fornire indicatori di dati validi/invalidi:

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

Come prima, impostiamo gli `<span>` su `position: relative` in modo da poter posizionare il contenuto generato in relazione ad essi. Poi posizioniamo in modo assoluto contenuti generati diversi a seconda del fatto che i dati del modulo siano validi o invalidi — rispettivamente un check verde o una croce rossa. Per aggiungere un po' di urgenza extra ai dati invalidi, abbiamo anche dato agli input un bordo rosso spesso quando sono invalidi.

> [!NOTE]
> Abbiamo usato `::before` per aggiungere queste etichette, poiché stavamo già usando `::after` per le etichette "required".

Puoi provarlo qui sotto:

{{EmbedGHLiveSample("learning-area/html/forms/pseudo-classes/valid-invalid.html", '100%', 430)}}

Nota come i text input richiesti sono invalidi quando vuoti, ma validi quando hanno qualcosa compilato. L'input email invece è valido quando vuoto, poiché non è richiesto, ma invalido quando contiene qualcosa che non è un indirizzo email corretto.

### Dati dentro al range e fuori dal range

Come accennato sopra, ci sono altre due pseudo-classi correlate da considerare — {{cssxref(":in-range")}} e {{cssxref(":out-of-range")}}. Queste corrispondono a input numerici dove i limiti di range sono specificati dagli attributi [`min`](/it/docs/Web/HTML/Reference/Elements/input#min) e [`max`](/it/docs/Web/HTML/Reference/Elements/input#max), quando i loro dati sono all'interno o all'esterno del range specificato, rispettivamente.

> [!NOTE]
> I tipi di input numerici sono `date`, `month`, `week`, `time`, `datetime-local`, `number`, e `range`.

Vale la pena notare che gli input i cui dati sono dentro al range saranno anche corrisposti dalla pseudo-classe `:valid` e gli input i cui dati sono fuori dal range saranno anche corrisposti dalla pseudo-classe `:invalid`. Perché avere entrambi? Il problema è davvero uno di semantica — fuori dal range è un tipo più specifico di comunicazione invalida, quindi potresti voler fornire un messaggio diverso per gli input fuori dal range, che sarà più utile per gli utenti rispetto a dire solo "invalido". Potresti anche voler fornire entrambi.

Diamo un'occhiata a un esempio che fa esattamente questo. La nostra demo [out-of-range.html](https://mdn.github.io/learning-area/html/forms/pseudo-classes/out-of-range.html) (vedi anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/forms/pseudo-classes/out-of-range.html)) si basa sull'esempio precedente per fornire messaggi di fuori dal range per gli input numerici, oltre a dire se sono richiesti.

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

Questa è una storia simile a quella dell'esempio `:required`, tranne per il fatto che qui abbiamo separato le dichiarazioni che si applicano a qualsiasi contenuto `::after` in una regola separata, e abbiamo dato al contenuto `::after` separato per gli stati `:required` e `:out-of-range` il loro contenuto e stile. Puoi provarlo qui:

{{EmbedGHLiveSample("learning-area/html/forms/pseudo-classes/out-of-range.html", '100%', 430)}}

È possibile che l'input del numero sia sia richiesto che fuori dal range allo stesso tempo, quindi cosa succede allora? Poiché la regola `:out-of-range` appare successivamente nel codice sorgente rispetto alla regola `:required`, si attivano le [regole di cascade](/it/docs/Learn_web_development/Core/Styling_basics/Handling_conflicts#understanding_the_cascade), e viene mostrato il messaggio di fuori dal range.

Questo funziona molto bene — quando la pagina si carica per la prima volta, "Required" viene mostrato, insieme a una croce rossa e un bordo. Quando hai digitato un'età valida (cioè, nel range di 12-120), l'input diventa valido. Se tuttavia, cambi l'ingresso dell'età in uno che è fuori dal range, il messaggio "Outside allowable value range" appare al posto di "Required".

> [!NOTE]
> Per inserire un valore invalido/fuori dal range, dovrai effettivamente focalizzare il modulo e digitarlo usando la tastiera. I pulsanti di spin non ti permetteranno di incrementare/decrementare il valore al di fuori del range consentito.

## Stilizzazione di input abilitati e disabilitati, e solo lettura e lettura/scrittura

Un elemento abilitato è un elemento che può essere attivato; può essere selezionato, cliccato, digitato, ecc. Un elemento disabilitato invece non può essere interagito in alcun modo, e i suoi dati non sono nemmeno inviati al server.

Questi due stati possono essere targetizzati usando {{cssxref(":enabled")}} e {{cssxref(":disabled")}}. Perché gli input disabilitati sono utili? Beh, a volte se alcuni dati non si applicano a un determinato utente, potresti non voler nemmeno inviare quei dati quando invia il modulo. Un esempio classico è un modulo di spedizione — comunemente ti viene chiesto se vuoi usare lo stesso indirizzo per la fatturazione e la spedizione; in tal caso, puoi semplicemente inviare un singolo indirizzo al server, e potresti anche disabilitare i campi dell'indirizzo di fatturazione.

Diamo un'occhiata a un esempio che fa proprio questo. Prima di tutto, l'HTML è un semplice modulo contenente text input, più una checkbox per attivare e disattivare la disabilitazione dell'indirizzo di fatturazione. I campi dell'indirizzo di fatturazione sono disabilitati per default.

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

Abbiamo direttamente selezionato gli input che vogliamo disabilitare usando `input[type="text"]:disabled`, ma volevamo anche ingrigire le corrispondenti etichette di testo. Poiché le etichette sono subito prima dei loro input, le abbiamo selezionate usando la pseudo-classe [`:has`](/it/docs/Web/CSS/:has).

Ora infine, abbiamo usato un po' di JavaScript per attivare e disattivare la disabilitazione dei campi dell'indirizzo di fatturazione:

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
  for (const item of billingItems) {
    item.disabled = !item.disabled;
  }
}
```

Usa l'[`evento change`](/it/docs/Web/API/HTMLElement/change_event) per permettere all'utente di abilitare/disabilitare i campi di fatturazione, e attivare lo styling delle etichette associate.

Puoi vedere l'esempio in azione qui sotto (vedi anche l'esempio live qui](https://mdn.github.io/learning-area/html/forms/pseudo-classes/enabled-disabled-shipping.html), e vedere il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/forms/pseudo-classes/enabled-disabled-shipping.html)):

{{EmbedGHLiveSample("learning-area/html/forms/pseudo-classes/enabled-disabled-shipping.html", '100%', 600)}}

### Solo lettura e lettura/scrittura

In modo simile a `:disabled` e `:enabled`, le pseudo-classi `:read-only` e `:read-write` targetizzano due stati tra cui gli input del modulo alternano. Come con gli input disabilitati, l'utente non può modificare gli input di sola lettura. Tuttavia, a differenza degli input disabilitati, i valori degli input di sola lettura saranno inviati al server. Lettura/scrittura significa che possono essere modificati — il loro stato predefinito.

Un input è impostato su sola lettura usando l'attributo `readonly`. Come esempio, immagina una pagina di conferma in cui lo sviluppatore ha inviato i dettagli compilati nelle pagine precedenti a questa pagina, con l'obiettivo di farli controllare tutti in un unico posto dall'utente, aggiungere eventuali dati finali necessari, e quindi confermare l'ordine inviando. A questo punto, tutti i dati del modulo finale possono essere inviati al server in un colpo solo.

Vediamo come potrebbe apparire un modulo (vedi [readonly-confirmation.html](https://mdn.github.io/learning-area/html/forms/pseudo-classes/readonly-confirmation.html) per l'esempio live; vedi anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/forms/pseudo-classes/readonly-confirmation.html)).

Un frammento dell'HTML è il seguente — nota l'attributo readonly:

```html
<div>
  <label for="name">Name: </label>
  <input id="name" name="name" type="text" value="Mr Soft" readonly />
</div>
```

Se provi l'esempio live, vedrai che il set superiore di elementi del modulo non è modificabile, tuttavia, i valori vengono inviati quando il modulo viene inviato. Abbiamo stilizzato i controlli del modulo usando le pseudo-classi `:read-only` e `:read-write`, in questo modo:

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

> **Nota:** `:enabled` e `:read-write` sono due altre pseudo-classi che probabilmente utilizzerai raramente, dato che descrivono gli stati predefiniti degli elementi input.

## Stati di radio e checkbox — checked, default, indeterminate

Come abbiamo visto negli articoli precedenti nel modulo, {{HTMLElement("input/radio", "pulsanti radio")}} e {{HTMLElement("input/checkbox", "checkbox")}} possono essere selezionati o non selezionati. Ma ci sono altri stati da considerare:

- {{cssxref(":default")}}: Corrisponde ai radio/checkbox che sono selezionati per default, al caricamento della pagina (cioè, impostando l'attributo `checked` su di essi). Questi corrispondono alla pseudo-classe {{cssxref(":default")}}, anche se l'utente li deseleziona.
- {{cssxref(":indeterminate")}}: Quando i radio/checkbox non sono né selezionati né deselezionati, sono considerati _indeterminate_ e corrisponderanno alla pseudo-classe {{cssxref(":indeterminate")}}. Più avanti su cosa significa questo.

### :checked

Quando selezionati, saranno corrisposti dalla pseudo-classe {{cssxref(":checked")}}.

L'uso più comune di questo è aggiungere uno stile diverso alla checkbox o pulsante radio quando è selezionato, nei casi in cui hai rimosso lo stile predefinito di sistema con [`appearance: none;`](/it/docs/Web/CSS/appearance) e vuoi ricostruire lo stile da solo. Abbiamo visto esempi di questo nell'articolo precedente quando abbiamo parlato di [Utilizzare `appearance: none` sui radio/checkbox](/it/docs/Learn_web_development/Extensions/Forms/Advanced_form_styling#using_appearance_none_on_radioscheckboxes).

Come riepilogo, il codice `:checked` dal nostro esempio di [Styled radio buttons](https://mdn.github.io/learning-area/html/forms/styling-examples/radios-styled.html) appare così:

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

Fondamentalmente, costruiamo lo stile per il "circolo interno" di un pulsante radio usando lo pseudo-elemento `::before`, ma impostiamo una [`transform`](/it/docs/Web/CSS/transform) `scale(0)` su di esso. Usiamo poi una [`transition`](/it/docs/Web/CSS/transition) per far sì che il contenuto generato sull'etichetta venga animato in modo gradevole quando il radio viene selezionato/spuntato. Il vantaggio di usare una trasformazione piuttosto che una transizione su [`width`](/it/docs/Web/CSS/width)/[`height`](/it/docs/Web/CSS/height) è che puoi usare [`transform-origin`](/it/docs/Web/CSS/transform-origin) per farlo crescere dal centro del cerchio, piuttosto che farlo apparire come se crescesse dall'angolo del cerchio, e non c'è comportamento di salto dato che nessun modello di proprietà delle caselle viene aggiornato.

### :default e :indeterminate

Come menzionato sopra, la pseudo-classe {{cssxref(":default")}} corrisponde ai radio/checkbox che sono selezionati per default, al caricamento della pagina, anche quando non selezionati. Questo potrebbe essere utile per aggiungere un indicatore a una lista di opzioni per ricordare all'utente quali erano i predefiniti (o le opzioni iniziali), nel caso vogliano resettare le loro scelte.

Inoltre, i radio/checkbox sopra menzionati saranno corrisposti dalla pseudo-classe {{cssxref(":indeterminate")}} quando sono in uno stato in cui non sono né selezionati né deselezionati. Ma cosa significa? Gli elementi che sono indeterminati includono:

- Input {{HTMLElement("input/radio")}}, quando tutti i pulsanti radio in un gruppo con lo stesso nome sono non selezionati
- Input {{HTMLElement("input/checkbox")}} la cui proprietà `indeterminate` è impostata su `true` tramite JavaScript
- Elementi {{HTMLElement("progress")}} che non hanno valore.

Questo non è qualcosa che probabilmente userai molto spesso. Un caso d'uso potrebbe essere un indicatore per dire agli utenti che devono davvero selezionare un pulsante radio prima di procedere.

Vediamo un paio di versioni modificate dell'esempio precedente che ricordano all'utente quale era l'opzione predefinita, e stilizzano le etichette dei pulsanti radio quando indeterminate. Entrambi questi hanno la seguente struttura HTML per gli input:

```html
<p>
  <input type="radio" name="fruit" value="cherry" id="cherry" />
  <label for="cherry">Cherry</label>
  <span></span>
</p>
```

Per l'esempio `:default`, abbiamo aggiunto l'attributo `checked` al pulsante radio centrale, quindi sarà selezionato per default quando caricato. Lo stiliamo con il seguente CSS:

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

Questo fornisce una piccola etichetta "Default" sull'elemento che era originariamente selezionato quando la pagina è stata caricata. Nota qui stiamo usando il combinatore di fratello successivo (`~`) piuttosto che il combinatore di fratello successivo (`+`) — dobbiamo fare questo perché lo `<span>` non viene subito dopo l'`<input>` nell'ordine del sorgente.

Vedi il risultato live qui sotto:

{{EmbedGHLiveSample("learning-area/html/forms/pseudo-classes/radios-checked-default.html", '100%', 200)}}

> [!NOTE]
> Puoi anche trovare l'esempio live su GitHub su [radios-checked-default.html](https://mdn.github.io/learning-area/html/forms/pseudo-classes/radios-checked-default.html) (vedi anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/forms/pseudo-classes/radios-checked-default.html).)

Per l'esempio `:indeterminate`, non abbiamo nessun pulsante radio selezionato per default — questo è importante — se ci fosse, allora non ci sarebbe stato nessuno stato indeterminato da stilizzare. Stilizziamo i pulsanti radio indeterminate con il seguente CSS:

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

Questo crea un piccolo contorno animato sui pulsanti radio, che indicano si spera che devi selezionarne uno!

Vedi il risultato live qui sotto:

{{EmbedGHLiveSample("learning-area/html/forms/pseudo-classes/radios-checked-indeterminate.html", '100%', 200)}}

> [!NOTE]
> Puoi anche trovare l'esempio live su GitHub su [radios-checked-indeterminate.html](https://mdn.github.io/learning-area/html/forms/pseudo-classes/radios-checked-indeterminate.html) (vedi anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/forms/pseudo-classes/radios-checked-indeterminate.html).)

> [!NOTE]
> Puoi trovare un [esempio interessante che coinvolge stati `indeterminate`](/it/docs/Web/HTML/Reference/Elements/input/checkbox#indeterminate_state_checkboxes) sulla pagina di riferimento [`<input type="checkbox">`](/it/docs/Web/HTML/Reference/Elements/input/checkbox).

## Altre pseudo-classi

Ci sono un numero di altre pseudo-classi di interesse, e non abbiamo spazio per scriverne tutte dettagliatamente qui. Parliamo di alcune altre che dovresti prendere tempo per esplorare.

- La pseudo-classe {{cssxref(":focus-within")}} corrisponde a un elemento che ha ricevuto focus o _contiene_ un elemento che ha ricevuto focus. Questo è utile se vuoi che un intero modulo si evidenzi in qualche modo quando un input all'interno di esso è focalizzato.
- La pseudo-classe {{cssxref(":focus-visible")}} corrisponde a elementi focalizzati che hanno ricevuto focus tramite interazione con la tastiera (piuttosto che touch o mouse) — utile se vuoi mostrare uno stile diverso per il focus da tastiera rispetto al mouse (o altro) focus.
- La pseudo-classe {{cssxref(":placeholder-shown")}} corrisponde a elementi {{htmlelement('input')}} e {{htmlelement('textarea')}} che hanno il loro placeholder visibile (cioè, i contenuti dell'attributo [`placeholder`](/it/docs/Web/HTML/Reference/Elements/input#placeholder)) perché il valore dell'elemento è vuoto.

I seguenti sono anche interessanti, ma per ora non hanno un buon supporto nei browser:

- La pseudo-classe {{cssxref(":blank")}} seleziona i controlli di modulo vuoti. {{cssxref(":empty")}} corrisponde anche a elementi che non hanno figli, come {{HTMLElement("input")}}, ma è più generale — corrisponde anche ad altri {{Glossary("void_element", "elementi vuoti")}} come {{HTMLElement("br")}} e {{HTMLElement("hr")}}. `:empty` ha un ragionevole supporto nei browser; la pseudo-classe `:blank` non è ancora finita nella sua specifica, quindi non è ancora supportata in nessun browser.
- La pseudo-classe [`:user-invalid`](/it/docs/Web/CSS/:user-invalid), quando supportata, sarà simile a {{cssxref(":invalid")}}, ma con una migliore esperienza utente. Se il valore è valido quando l'input riceve il focus, l'elemento può corrispondere a `:invalid` mentre l'utente inserisce dati se il valore è temporaneamente invalido, ma corrisponderà solo a `:user-invalid` quando l'elemento perde il focus. Se il valore era originariamente invalido, corrisponderà a entrambi `:invalid` e `:user-invalid` per la durata dell'intero focus. In modo simile a `:invalid`, smetterà di corrispondere a `:user-invalid` se il valore diventa valido.

## Testa le tue abilità!

Sei giunto alla fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare alcuni ulteriori test per verificare di aver trattenuto queste informazioni prima di passare avanti — vedi [Testa le tue abilità: Stilizzazione avanzata](/it/docs/Learn_web_development/Extensions/Forms/Test_your_skills/Advanced_styling).

## Riepilogo

Questo completa il nostro sguardo alle pseudo-classi UI che riguardano gli input dei moduli. Continua a giocarci e crea alcuni stili di modulo divertenti! A seguire, passeremo a qualcosa di diverso — [validazione del modulo lato client](/it/docs/Learn_web_development/Extensions/Forms/Form_validation).

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/Customizable_select", "Learn_web_development/Extensions/Forms/Form_validation", "Learn_web_development/Extensions/Forms")}}
