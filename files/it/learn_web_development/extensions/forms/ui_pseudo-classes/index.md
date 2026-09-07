---
title: Pseudo-classi UI
slug: Learn_web_development/Extensions/Forms/UI_pseudo-classes
l10n:
  sourceCommit: 870fe25a3e6ed1a44222c52dd8a992b731c1a383
---

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/Customizable_select_listboxes", "Learn_web_development/Extensions/Forms/Form_validation", "Learn_web_development/Extensions/Forms")}}

Negli articoli precedenti, è stato trattato in modo generale lo styling di vari controlli dei moduli. Ciò includeva l'uso di alcune pseudo-classi, per esempio l'uso di `:checked` per selezionare una checkbox solo quando è selezionata. In questo articolo vengono esplorate le diverse pseudo-classi UI disponibili per applicare stili ai moduli in stati differenti.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Una conoscenza di base di
        <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, inclusa una conoscenza
        generale di
        <a
          href="/it/docs/Learn_web_development/Core/Styling_basics/Pseudo_classes_and_elements"
          >pseudo-classi e pseudo-elementi</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere quali parti dei moduli sono difficili da stilizzare e perché;
        imparare cosa si può fare per personalizzarle.
      </td>
    </tr>
  </tbody>
</table>

## Quali pseudo-classi sono disponibili?

Si potrebbe già avere familiarità con le seguenti pseudo-classi:

- {{cssxref(":hover")}}: seleziona un elemento solo quando il puntatore del mouse passa sopra di esso.
- {{cssxref(":focus")}}: seleziona un elemento solo quando ha il focus (ovvero quando viene raggiunto tramite il tasto Tab della tastiera).
- {{cssxref(":active")}}: seleziona un elemento solo quando viene attivato (ovvero mentre viene fatto clic su di esso oppure, nel caso di un'attivazione da tastiera, quando viene tenuto premuto il tasto <kbd>Return</kbd> / <kbd>Enter</kbd>).

I [selettori CSS](/it/docs/Web/CSS/Guides/Selectors) forniscono diverse altre pseudo-classi correlate ai moduli HTML. Queste offrono varie utili condizioni di selezione che possono essere sfruttate. Verranno esaminate più nel dettaglio nelle sezioni seguenti, ma in breve, quelle principali trattate saranno:

- {{cssxref(':required')}} e {{cssxref(':optional')}}: selezionano gli elementi che possono essere obbligatori (per esempio, elementi che supportano l'attributo HTML [`required`](/it/docs/Web/HTML/Reference/Attributes/required)), a seconda che siano obbligatori o facoltativi.
- {{cssxref(":valid")}} e {{cssxref(":invalid")}}, nonché {{cssxref(":in-range")}} e {{cssxref(":out-of-range")}}: selezionano i controlli dei moduli validi/non validi in base ai vincoli di convalida del modulo impostati su di essi, oppure con dati all'interno/all'esterno dell'intervallo.
- {{cssxref(":enabled")}} e {{cssxref(":disabled")}}, nonché {{cssxref(":read-only")}} e {{cssxref(":read-write")}}: selezionano gli elementi che possono essere disabilitati (per esempio, elementi che supportano l'attributo HTML [`disabled`](/it/docs/Web/HTML/Reference/Attributes/disabled)), in base al fatto che siano attualmente abilitati o disabilitati, e i controlli del modulo in lettura-scrittura o sola lettura (per esempio, elementi con l'attributo [`readonly`](/it/docs/Web/HTML/Reference/Attributes/readonly) impostato).
- {{cssxref(":checked")}}, {{cssxref(":indeterminate")}} e {{cssxref(":default")}}: selezionano rispettivamente checkbox e radio button selezionati, in uno stato indeterminato (né selezionati né deselezionati) e l'opzione predefinita selezionata al caricamento della pagina (per esempio, un [`<input type="checkbox">`](/it/docs/Web/HTML/Reference/Elements/input/checkbox) con l'attributo [`checked`](/it/docs/Web/HTML/Reference/Elements/input#checked) impostato, oppure un elemento [`<option>`](/it/docs/Web/HTML/Reference/Elements/option) con l'attributo [`selected`](/it/docs/Web/HTML/Reference/Elements/option#selected) impostato).

Ne esistono molte altre, ma quelle elencate sopra sono le più evidentemente utili. Alcune sono pensate per risolvere problemi di nicchia molto specifici. Le pseudo-classi UI elencate sopra dispongono di un eccellente supporto dei browser, ma naturalmente è necessario testare attentamente le implementazioni dei moduli per assicurarsi che funzionino per il pubblico di destinazione.

> [!NOTE]
> Diverse delle pseudo-classi trattate qui riguardano lo styling dei controlli del modulo in base al loro stato di convalida (i loro dati sono validi o no?). Il prossimo articolo — [Convalida dei moduli lato client](/it/docs/Learn_web_development/Extensions/Forms/Form_validation) — illustrerà molto più dettagliatamente come impostare e controllare i vincoli di convalida, ma per ora la convalida del modulo verrà mantenuta semplice, per non complicare le cose.

## Applicare stili agli input in base al fatto che siano obbligatori o meno

Uno dei concetti più basilari relativi alla convalida dei moduli lato client è stabilire se un input del modulo è obbligatorio (deve essere compilato prima di poter inviare il modulo) oppure facoltativo.

Gli elementi {{htmlelement('input')}}, {{htmlelement('select')}} e {{htmlelement('textarea')}} dispongono dell'attributo `required` che, quando impostato, indica che quel controllo deve essere compilato prima che il modulo possa essere inviato correttamente.
Per esempio, il nome e il cognome sono obbligatori nel modulo seguente, mentre l'indirizzo email è facoltativo:

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

È possibile associare questi due stati usando le pseudo-classi {{cssxref(':required')}} e {{cssxref(':optional')}}. Per esempio, se si applica il seguente CSS all'HTML precedente:

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
  box-shadow: inset 1px 1px 3px #cccccc;
  border-radius: 5px;
}

input:hover,
input:focus {
  background-color: #eeeeee;
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

I controlli obbligatori hanno un bordo continuo, mentre il controllo facoltativo ha un bordo tratteggiato.
Si può anche provare a inviare il modulo senza compilarlo, per vedere i messaggi di errore della convalida lato client forniti dai browser per impostazione predefinita:

{{EmbedLiveSample("optional-required-styles", , "400px", , , , , "allow-forms")}}

In generale, si dovrebbe evitare di distinguere gli elementi "obbligatori" da quelli "facoltativi" nei moduli usando soltanto il colore, poiché ciò non è ottimale per le persone daltoniche:

```css example-bad
input:required {
  border: 2px solid red;
}

input:optional {
  border: 2px solid green;
}
```

La convenzione standard sul web per indicare lo stato obbligatorio è un asterisco (`*`), oppure la parola "required" associata ai rispettivi controlli.
Nella sezione successiva verrà esaminato un esempio migliore per indicare i campi obbligatori mediante `:required` e contenuto generato.

> [!NOTE]
> Probabilmente non sarà necessario usare spesso la pseudo-classe `:optional`. I controlli dei moduli sono facoltativi per impostazione predefinita, quindi è possibile applicare lo stile facoltativo come impostazione predefinita e aggiungere stili aggiuntivi per i controlli obbligatori.

> [!NOTE]
> Se un radio button in un gruppo di radio button con lo stesso nome ha l'attributo `required` impostato, tutti i radio button saranno non validi finché non ne viene selezionato uno, ma solo quello a cui è assegnato l'attributo corrisponderà effettivamente a {{cssxref(':required')}}.

## Uso del contenuto generato con le pseudo-classi

Negli articoli precedenti è già stato visto l'uso del [contenuto generato](/it/docs/Web/CSS/Guides/Generated_content), ma questo è un buon momento per descriverlo con maggior dettaglio.

L'idea è che sia possibile usare gli pseudo-elementi {{cssxref("::before")}} e {{cssxref("::after")}} insieme alla proprietà {{cssxref("content")}} per far apparire un blocco di contenuto prima o dopo l'elemento interessato. Il blocco di contenuto non viene aggiunto al DOM, quindi potrebbe essere invisibile ad alcuni screen reader. Poiché è uno pseudo-elemento, può essere selezionato con gli stili nello stesso modo di qualsiasi nodo effettivo del DOM.

Questo è molto utile quando si desidera aggiungere un indicatore visivo a un elemento, ad esempio un'etichetta o un'icona, mentre sono disponibili anche indicatori alternativi per garantire l'accessibilità a tutti gli utenti. Per esempio, è possibile usare il contenuto generato per gestire il posizionamento e l'animazione del cerchio interno di un radio button personalizzato quando viene selezionato:

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

Questo è molto utile: gli screen reader informano già gli utenti quando un radio button o una checkbox che incontrano è selezionato, quindi non è opportuno che leggano un altro elemento DOM che indica la selezione, poiché potrebbe essere fonte di confusione. Un indicatore puramente visivo risolve questo problema.

Non tutti i tipi di `<input>` supportano l'inserimento di contenuto generato. Tutti i tipi di input che mostrano testo dinamico, come `text`, `password` o `button`, non visualizzano il contenuto generato. Altri, tra cui `range`, `color`, `checkbox` e così via, visualizzano il contenuto generato.

Tornando all'esempio obbligatorio/facoltativo precedente, questa volta non verrà modificato l'aspetto dell'input stesso: verrà usato contenuto generato per aggiungere un'etichetta indicativa.

Prima di tutto, verrà aggiunto un paragrafo all'inizio del modulo per spiegare cosa cercare:

```html
<p>Required fields are labeled with "required".</p>
```

Gli utenti di screen reader sentiranno leggere "required" come informazione aggiuntiva quando raggiungono ciascun input obbligatorio, mentre gli utenti vedenti vedranno l'etichetta.

Come già detto, gli input di testo non supportano il contenuto generato, quindi viene aggiunto uno [`<span>`](/it/docs/Web/HTML/Reference/Elements/span) vuoto a cui agganciare il contenuto generato:

```html
<div>
  <label for="fname">First name: </label>
  <input id="fname" name="fname" type="text" required />
  <span></span>
</div>
```

Il problema immediato era che lo span andava su una nuova riga sotto l'input, poiché sia l'input sia l'etichetta sono impostati con `width: 100%`. Per risolvere il problema, al `<div>` genitore viene applicato lo stile per trasformarlo in un contenitore flex, indicando anche di disporre il contenuto su nuove righe se diventa troppo lungo:

```css
fieldset > div {
  margin-bottom: 20px;
  display: flex;
  flex-flow: row wrap;
}
```

L'effetto è che l'etichetta e l'input si trovano su righe separate poiché entrambi sono `width: 100%`, ma lo `<span>` ha una larghezza pari a `0`, pertanto può trovarsi sulla stessa riga dell'input.

Ora passiamo al contenuto generato. Viene creato usando questo CSS:

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

Lo `<span>` viene impostato su `position: relative` affinché il contenuto generato possa essere impostato su `position: absolute` e posizionato relativamente allo `<span>` anziché al `<body>` (ai fini del posizionamento, il contenuto generato si comporta come se fosse un nodo figlio dell'elemento su cui viene generato).

Al contenuto generato viene quindi assegnato il testo "required", ovvero ciò che si desiderava mostrare nell'etichetta, e viene stilizzato e posizionato come desiderato. Il risultato è visibile qui sotto (premere il pulsante **Play** per eseguire l'esempio in MDN Playground e modificare il codice sorgente).

```html hidden live-sample___required-optional-generated
<form>
  <fieldset>
    <legend>Feedback form</legend>

    <p>Required fields are labeled with "required".</p>
    <div>
      <label for="fname">First name: </label>
      <input id="fname" name="fname" type="text" required />
      <span></span>
    </div>
    <div>
      <label for="lname">Last name: </label>
      <input id="lname" name="lname" type="text" required />
      <span></span>
    </div>
    <div>
      <label for="email"
        >Email address (include if you want a response):
      </label>
      <input id="email" name="email" type="email" />
      <span></span>
    </div>
    <div><button>Submit</button></div>
  </fieldset>
</form>
```

```css hidden live-sample___required-optional-generated
@import "https://fonts.googleapis.com/css2?family=Josefin+Sans:ital,wght@0,100..700;1,100..700&display=swap";

body {
  font-family: "Josefin Sans", sans-serif;
  margin: 20px auto;
  max-width: 460px;
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
  font-family: inherit;
  font-size: 100%;
  margin: 0;
  box-sizing: border-box;
  width: 100%;
  padding: 5px;
  height: 30px;
}

input {
  box-shadow: inset 1px 1px 3px #cccccc;
  border-radius: 5px;
}

input:hover,
input:focus {
  background-color: #eeeeee;
}

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

button {
  width: 60%;
  margin: 0 auto;
}
```

```js hidden live-sample___required-optional-generated
const form = document.querySelector("form");
form.addEventListener("submit", (e) => {
  e.preventDefault();
});
```

{{EmbedLiveSample("required-optional-generated", "100%", 430, , , , , "allow-forms")}}

## Applicare stili ai controlli in base alla validità dei dati

L'altro concetto fondamentale molto importante nella convalida dei moduli è stabilire se i dati di un controllo del modulo sono validi o meno (nel caso di dati numerici, si può anche parlare di dati all'interno o all'esterno dell'intervallo). I controlli dei moduli con [vincoli di validazione](/it/docs/Web/HTML/Guides/Constraint_validation) possono essere selezionati in base a questi stati.

### :valid e :invalid

È possibile selezionare i controlli dei moduli usando le pseudo-classi {{cssxref(":valid")}} e {{cssxref(":invalid")}}. Alcuni punti da tenere presenti:

- I controlli senza convalida dei vincoli saranno sempre validi e quindi corrisponderanno a `:valid`.
- I controlli con `required` impostato che non hanno un valore sono considerati non validi: corrisponderanno a `:invalid` e `:required`.
- I controlli con convalida integrata, come `<input type="email">` o `<input type="url">`, corrispondono a `:invalid` quando i dati inseriti non rispettano il modello previsto (ma sono validi quando sono vuoti).
- I controlli il cui valore corrente è al di fuori dei limiti dell'intervallo specificati dagli attributi [`min`](/it/docs/Web/HTML/Reference/Elements/input#min) e [`max`](/it/docs/Web/HTML/Reference/Elements/input#max) corrispondono a `:invalid`, ma corrispondono anche a {{cssxref(":out-of-range")}}, come verrà illustrato più avanti.
- Esistono altri modi per fare in modo che un elemento corrisponda a `:valid`/`:invalid`, come verrà illustrato nell'articolo [Convalida dei moduli lato client](/it/docs/Learn_web_development/Extensions/Forms/Form_validation). Per ora, tuttavia, si manterrà tutto semplice.

Esaminiamo un esempio di `:valid`/`:invalid`.

Come nell'esempio precedente, sono presenti ulteriori `<span>` su cui generare contenuto, che verranno usati per fornire indicatori di dati validi/non validi:

```html
<div>
  <label for="fname">First name: </label>
  <input id="fname" name="fname" type="text" required />
  <span></span>
</div>
```

Per fornire questi indicatori, viene usato il seguente CSS:

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

Come prima, gli `<span>` vengono impostati su `position: relative` affinché il contenuto generato possa essere posizionato relativamente a essi. Viene quindi posizionato in modo assoluto contenuto generato differente a seconda che i dati del modulo siano validi o non validi: rispettivamente un segno di spunta verde o una croce rossa. Per aggiungere un po' di urgenza ai dati non validi, agli input viene inoltre assegnato un bordo rosso spesso quando non sono validi.

> [!NOTE]
> È stato usato `::before` per aggiungere queste etichette, poiché `::after` era già utilizzato per le etichette "required".

Si può provare qui sotto (premere il pulsante **Play** per eseguire l'esempio in MDN Playground e modificare il codice sorgente):

```html hidden live-sample___valid-invalid
<form>
  <fieldset>
    <legend>Feedback form</legend>

    <p>Required fields are labeled with "required".</p>
    <div>
      <label for="fname">First name: </label>
      <input id="fname" name="fname" type="text" required />
      <span></span>
    </div>
    <div>
      <label for="lname">Last name: </label>
      <input id="lname" name="lname" type="text" required />
      <span></span>
    </div>
    <div>
      <label for="email"
        >Email address (include if you want a response):
      </label>
      <input id="email" name="email" type="email" />
      <span></span>
    </div>
    <div><button>Submit</button></div>
  </fieldset>
</form>
```

```css hidden live-sample___valid-invalid
@import "https://fonts.googleapis.com/css2?family=Josefin+Sans:ital,wght@0,100..700;1,100..700&display=swap";

body {
  font-family: "Josefin Sans", sans-serif;
  margin: 20px auto;
  max-width: 460px;
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
  font-family: inherit;
  font-size: 100%;
  margin: 0;
  box-sizing: border-box;
  width: 100%;
  padding: 5px;
  height: 30px;
}

input {
  box-shadow: inset 1px 1px 3px #cccccc;
  border-radius: 5px;
}

input:hover,
input:focus {
  background-color: #eeeeee;
}

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

button {
  width: 60%;
  margin: 0 auto;
}
```

```js hidden live-sample___valid-invalid
const form = document.querySelector("form");
form.addEventListener("submit", (e) => {
  e.preventDefault();
});
```

{{EmbedLiveSample("valid-invalid", "100%", 430, , , , , "allow-forms")}}

Si noti come gli input di testo obbligatori non siano validi quando sono vuoti, ma diventino validi quando contengono qualcosa. L'input email, invece, è valido quando è vuoto, poiché non è obbligatorio, ma non è valido quando contiene qualcosa che non è un indirizzo email corretto.

### Dati all'interno e all'esterno dell'intervallo

Come accennato sopra, vi sono altre due pseudo-classi correlate da considerare: {{cssxref(":in-range")}} e {{cssxref(":out-of-range")}}. Queste corrispondono agli input numerici per i quali i limiti dell'intervallo sono specificati tramite [`min`](/it/docs/Web/HTML/Reference/Elements/input#min) e [`max`](/it/docs/Web/HTML/Reference/Elements/input#max), quando i dati sono rispettivamente all'interno o all'esterno dell'intervallo specificato.

> [!NOTE]
> I tipi di input numerici sono `date`, `month`, `week`, `time`, `datetime-local`, `number` e `range`.

Vale la pena notare che gli input i cui dati sono all'interno dell'intervallo corrisponderanno anche alla pseudo-classe `:valid`, mentre gli input i cui dati sono al di fuori dell'intervallo corrisponderanno anche alla pseudo-classe `:invalid`. Perché avere entrambe? La questione riguarda soprattutto la semantica: fuori dall'intervallo è un tipo più specifico di comunicazione di non validità, quindi potrebbe essere opportuno fornire un messaggio diverso per gli input fuori intervallo, che sarà più utile agli utenti rispetto alla semplice indicazione "non valido". Potrebbe persino essere opportuno fornire entrambi.

Vediamo un esempio che fa esattamente questo, basandosi sull'esempio precedente per fornire messaggi fuori intervallo per gli input numerici, oltre a indicare se sono obbligatori.

L'input numerico ha questo aspetto:

```html
<div>
  <label for="age">Age (must be 12+): </label>
  <input id="age" name="age" type="number" min="12" max="120" required />
  <span></span>
</div>
```

Il CSS ha questo aspetto:

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

La situazione è simile a quella dell'esempio precedente con `:required`, tranne per il fatto che qui le dichiarazioni applicabili a qualsiasi contenuto `::after` sono state separate in una regola distinta, e al contenuto `::after` separato per gli stati `:required` e `:out-of-range` sono stati assegnati contenuto e styling propri. Si può provare qui (premere il pulsante **Play** per eseguire l'esempio in MDN Playground e modificare il codice sorgente):

```html hidden live-sample___out-of-range
<form>
  <fieldset>
    <legend>Feedback form</legend>

    <p>Required fields are labeled with "required".</p>
    <div>
      <label for="name">Name: </label>
      <input id="name" name="name" type="text" required />
      <span></span>
    </div>
    <div>
      <label for="age">Age (must be 12+): </label>
      <input id="age" name="age" type="number" min="12" max="120" required />
      <span></span>
    </div>
    <div>
      <label for="email"
        >Email address (include if you want a response):
      </label>
      <input id="email" name="email" type="email" />
      <span></span>
    </div>
    <div><button>Submit</button></div>
  </fieldset>
</form>
```

```css hidden live-sample___out-of-range
@import "https://fonts.googleapis.com/css2?family=Josefin+Sans:ital,wght@0,100..700;1,100..700&display=swap";

body {
  font-family: "Josefin Sans", sans-serif;
  margin: 20px auto;
  max-width: 460px;
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
  font-family: inherit;
  font-size: 100%;
  margin: 0;
  box-sizing: border-box;
  width: 100%;
  padding: 5px;
  height: 30px;
}

input {
  box-shadow: inset 1px 1px 3px #cccccc;
  border-radius: 5px;
}

input:hover,
input:focus {
  background-color: #eeeeee;
}

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
  content: "required";
  left: -70px;
}

input:out-of-range + span::after {
  color: white;
  background-color: red;
  width: 155px;
  content: "Outside allowable value range";
  left: -182px;
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

button {
  width: 60%;
  margin: 0 auto;
}
```

```js hidden live-sample___out-of-range
const form = document.querySelector("form");
form.addEventListener("submit", (e) => {
  e.preventDefault();
});
```

{{EmbedLiveSample("out-of-range", "100%", 430, , , , , "allow-forms")}}

È possibile che l'input numerico sia contemporaneamente obbligatorio e fuori intervallo; cosa succede in questo caso? Poiché la regola `:out-of-range` appare più avanti nel codice sorgente rispetto alla regola `:required`, entrano in gioco le [regole della cascata](/it/docs/Learn_web_development/Core/Styling_basics/Handling_conflicts#understanding_the_cascade) e viene mostrato il messaggio fuori intervallo.

Questo funziona piuttosto bene: quando la pagina viene caricata per la prima volta, viene mostrato "Required", insieme a una croce e a un bordo rossi. Dopo aver inserito un'età valida, ovvero nell'intervallo 12-120, l'input diventa valido. Tuttavia, modificando poi l'età con un valore fuori intervallo, il messaggio "Outside allowable value range" appare al posto di "Required".

> [!NOTE]
> Per inserire un valore non valido/fuori intervallo, è necessario dare effettivamente il focus al modulo e digitarlo usando la tastiera. I pulsanti dello spinner non consentono di incrementare o decrementare il valore oltre l'intervallo consentito.

## Applicare stili agli input abilitati e disabilitati, e in sola lettura e lettura-scrittura

Un elemento abilitato è un elemento che può essere attivato: può essere selezionato, ricevere clic, ricevere testo e così via. Al contrario, non è possibile interagire in alcun modo con un elemento disabilitato e i suoi dati non vengono nemmeno inviati al server.

Questi due stati possono essere selezionati usando {{cssxref(":enabled")}} e {{cssxref(":disabled")}}. Perché gli input disabilitati sono utili? Talvolta, se alcuni dati non si applicano a un determinato utente, potrebbe non essere desiderabile inviarli nemmeno quando il modulo viene inviato. Un esempio classico è un modulo di spedizione: comunemente viene chiesto se si desidera usare lo stesso indirizzo per la fatturazione e la spedizione; in tal caso, è possibile inviare un solo indirizzo al server e disabilitare semplicemente i campi dell'indirizzo di fatturazione.

Vediamo un esempio che fa esattamente questo. Prima di tutto, l'HTML è un semplice modulo che contiene input di testo, oltre a una checkbox per attivare e disattivare la disabilitazione dell'indirizzo di fatturazione. I campi dell'indirizzo di fatturazione sono disabilitati per impostazione predefinita.

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
  background: #eeeeee;
  border: 1px solid #cccccc;
}

label:has(+ :disabled) {
  color: #aaaaaa;
}
```

Gli input da disabilitare sono stati selezionati direttamente usando `input[type="text"]:disabled`, ma si desiderava anche rendere grigie le etichette di testo corrispondenti. Poiché le etichette si trovano subito prima dei rispettivi input, sono state selezionate usando la pseudo-classe {{cssxref(":has")}}.

Infine, è stato usato JavaScript per attivare o disattivare la disabilitazione dei campi dell'indirizzo di fatturazione:

```js
function toggleBilling() {
  // Select the billing text fields
  const billingItems = document.querySelectorAll('#billing input[type="text"]');

  // Toggle the billing text fields
  for (const item of billingItems) {
    item.disabled = !item.disabled;
  }
}

// Attach `change` event listener to checkbox
document
  .getElementById("billing-checkbox")
  .addEventListener("change", toggleBilling);
```

Viene usato l'[evento `change`](/it/docs/Web/API/HTMLElement/change_event) per consentire all'utente di abilitare/disabilitare i campi di fatturazione e alternare lo styling delle etichette associate.

L'esempio in azione è visibile qui sotto (premere il pulsante **Play** per eseguire l'esempio in MDN Playground e modificare il codice sorgente):

```html hidden live-sample___enabled-disabled-shipping
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
      <label for="name" class="billing-label">Name: </label>
      <input id="name" name="name" type="text" disabled required />
    </div>
    <div>
      <label for="address2" class="billing-label">Address: </label>
      <input id="address2" name="address2" type="text" disabled required />
    </div>
    <div>
      <label for="zip-code2" class="billing-label">Zip/postal code: </label>
      <input id="zip-code2" name="zip-code2" type="text" disabled required />
    </div>
  </fieldset>

  <div><button>Submit</button></div>
</form>
```

```css hidden live-sample___enabled-disabled-shipping
@import "https://fonts.googleapis.com/css2?family=Josefin+Sans:ital,wght@0,100..700;1,100..700&display=swap";

body {
  font-family: "Josefin Sans", sans-serif;
  margin: 20px auto;
  max-width: 460px;
}

fieldset {
  padding: 10px 30px 0;
  margin-bottom: 20px;
}

legend {
  color: white;
  background: black;
  padding: 5px 10px;
}

fieldset > div {
  margin-bottom: 20px;
  display: flex;
}

button,
label,
input[type="text"] {
  display: block;
  font-family: inherit;
  font-size: 100%;
  margin: 0;
  box-sizing: border-box;
  width: 100%;
  padding: 5px;
  height: 30px;
}

input {
  box-shadow: inset 1px 1px 3px #cccccc;
  border-radius: 5px;
}

input:hover,
input:focus {
  background-color: #eeeeee;
}

input[type="text"]:disabled {
  background: #eeeeee;
  border: 1px solid #cccccc;
}

label:has(+ :disabled) {
  color: #aaaaaa;
}

button {
  width: 60%;
  margin: 0 auto;
}
```

```js hidden live-sample___enabled-disabled-shipping
function toggleBilling() {
  // Select the billing text fields
  const billingItems = document.querySelectorAll('#billing input[type="text"]');

  // Toggle the billing text fields
  for (const item of billingItems) {
    item.disabled = !item.disabled;
  }
}

// Attach `change` event listener to checkbox
document
  .getElementById("billing-checkbox")
  .addEventListener("change", toggleBilling);

const form = document.querySelector("form");
form.addEventListener("submit", (e) => {
  e.preventDefault();
});
```

{{EmbedLiveSample("enabled-disabled-shipping", "100%", 580, , , , , "allow-forms")}}

### Sola lettura e lettura-scrittura

In modo simile a `:disabled` e `:enabled`, le pseudo-classi `:read-only` e `:read-write` selezionano due stati tra i quali gli input del modulo possono alternarsi. Come per gli input disabilitati, l'utente non può modificare gli input in sola lettura. Tuttavia, a differenza degli input disabilitati, i valori degli input in sola lettura verranno inviati al server. Lettura-scrittura significa che possono essere modificati: è il loro stato predefinito.

Un input viene impostato in sola lettura usando l'attributo `readonly`. Per esempio, si immagini una pagina di conferma in cui lo sviluppatore ha inviato a questa pagina i dettagli compilati nelle pagine precedenti, con l'obiettivo di consentire all'utente di controllarli tutti in un unico posto, aggiungere eventuali dati finali necessari e quindi confermare l'ordine tramite invio. A questo punto, tutti i dati finali del modulo possono essere inviati al server in un'unica operazione.

Vediamo come potrebbe essere un modulo.

Un frammento dell'HTML è il seguente: si noti l'attributo `readonly`:

```html
<div>
  <label for="name">Name: </label>
  <input id="name" name="name" type="text" value="Mr Soft" readonly />
</div>
```

Provando l'esempio dal vivo, si potrà vedere che il gruppo superiore di elementi del modulo non è modificabile; tuttavia, i valori vengono inviati quando il modulo viene inoltrato. I controlli del modulo sono stati stilizzati mediante le pseudo-classi `:read-only` e `:read-write`, in questo modo:

```css
input:read-only,
textarea:read-only {
  border: 0;
  box-shadow: none;
  background-color: white;
}

textarea:read-write {
  box-shadow: inset 1px 1px 3px #cccccc;
  border-radius: 5px;
}
```

L'esempio completo è il seguente (premere il pulsante **Play** per eseguire l'esempio in MDN Playground e modificare il codice sorgente):

```html hidden live-sample___readonly-confirmation
<form>
  <fieldset>
    <legend>Check shipping details</legend>
    <div>
      <label for="name">Name: </label>
      <input id="name" name="name" type="text" value="Mr Soft" readonly />
    </div>
    <div>
      <label for="address">Address: </label>
      <textarea id="address" name="address" readonly>
23 Elastic Way,
Viscous,
Bright Ridge,
CA
</textarea>
    </div>
    <div>
      <label for="zip-code">Zip/postal code: </label>
      <input id="zip-code" name="zip-code" type="text" value="94708" readonly />
    </div>
  </fieldset>

  <fieldset>
    <legend>Final instructions</legend>
    <div>
      <label for="sms-confirm">Send confirmation by SMS?</label>
      <input id="sms-confirm" name="sms-confirm" type="checkbox" />
    </div>
    <div>
      <label for="instructions">Any special instructions?</label>
      <textarea id="instructions" name="instructions"></textarea>
    </div>
  </fieldset>

  <div><button type="button">Amend details</button></div>
  <div><button type="submit">Submit</button></div>
</form>
```

```css hidden live-sample___readonly-confirmation
@import "https://fonts.googleapis.com/css2?family=Josefin+Sans:ital,wght@0,100..700;1,100..700&display=swap";

body {
  font-family: "Josefin Sans", sans-serif;
  margin: 20px auto;
  max-width: 460px;
}

fieldset {
  padding: 10px 30px 0;
  margin-bottom: 20px;
}

legend {
  color: white;
  background: black;
  padding: 5px 10px;
}

fieldset > div {
  margin-bottom: 20px;
  display: flex;
  justify-content: space-between;
}

button,
label,
input[type="text"],
textarea {
  display: block;
  font-family: inherit;
  font-size: 100%;
  margin: 0;
  box-sizing: border-box;
  padding: 5px;
  height: 30px;
}

input[type="text"],
textarea {
  width: 50%;
}

textarea {
  height: 110px;
  resize: none;
}

label {
  width: 40%;
}

input:hover,
input:focus,
textarea:hover,
textarea:focus {
  background-color: #eeeeee;
}

button {
  width: 60%;
  margin: 20px auto;
}

input:read-only,
textarea:read-only {
  border: 0;
  box-shadow: none;
  background-color: white;
}

textarea:read-write {
  box-shadow: inset 1px 1px 3px #cccccc;
  border-radius: 5px;
}
```

```js hidden live-sample___readonly-confirmation
const form = document.querySelector("form");
form.addEventListener("submit", (e) => {
  e.preventDefault();
});
```

{{EmbedLiveSample("readonly-confirmation", "100%", 660, , , , , "allow-forms")}}

> [!NOTE]
> `:enabled` e `:read-write` sono altre due pseudo-classi che probabilmente verranno usate raramente, dato che descrivono gli stati predefiniti degli elementi input.

## Stati di radio button e checkbox: selezionato, predefinito, indeterminato

Come visto negli articoli precedenti del modulo, i {{HTMLElement("input/radio", "radio button")}} e le {{HTMLElement("input/checkbox", "checkbox")}} possono essere selezionati o deselezionati. Tuttavia, vi sono anche un paio di altri stati da considerare:

- {{cssxref(":default")}}: corrisponde a radio button/checkbox selezionati per impostazione predefinita, al caricamento della pagina (ovvero impostando su di essi l'attributo `checked`). Questi corrispondono alla pseudo-classe {{cssxref(":default")}}, anche se l'utente li deseleziona.
- {{cssxref(":indeterminate")}}: quando radio button/checkbox non sono né selezionati né deselezionati, sono considerati _indeterminati_ e corrispondono alla pseudo-classe {{cssxref(":indeterminate")}}. Maggiori informazioni sul significato di questo stato sono riportate di seguito.

### :checked

Quando sono selezionati, corrispondono alla pseudo-classe {{cssxref(":checked")}}.

L'uso più comune consiste nell'aggiungere uno stile differente alla checkbox o al radio button quando è selezionato, nei casi in cui lo stile predefinito di sistema è stato rimosso con [`appearance: none;`](/it/docs/Web/CSS/Reference/Properties/appearance) e si desidera ricostruire manualmente gli stili. Esempi di questo sono stati visti nell'articolo precedente, nella sezione [Applicare stili a checkbox e radio button usando `appearance`](/it/docs/Learn_web_development/Extensions/Forms/Advanced_form_styling#styling_checkboxes_and_radio_buttons_using_appearance).

Come riepilogo, il codice `:checked` dell'esempio Styled radio buttons è questo:

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

Si può provare qui (premere il pulsante **Play** per eseguire l'esempio in MDN Playground e modificare il codice sorgente):

```html hidden live-sample___radios-styled
<form>
  <fieldset>
    <legend>Choose your favorite fruit</legend>
    <p>
      <label>
        <input type="radio" name="fruit" value="cherry" />
        Cherry
      </label>
    </p>
    <p>
      <label>
        <input type="radio" name="fruit" value="banana" />
        Banana
      </label>
    </p>
    <p>
      <label>
        <input type="radio" name="fruit" value="strawberry" />
        Strawberry
      </label>
    </p>
  </fieldset>
</form>
```

```css hidden live-sample___radios-styled
input[type="radio"] {
  appearance: none;
}

input[type="radio"] {
  width: 20px;
  height: 20px;
  border-radius: 10px;
  border: 2px solid gray;
  /* Adjusts the position of the checkboxes on the text baseline */
  vertical-align: -2px;
  outline: none;
}

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

{{EmbedLiveSample("radios-styled", "100%", 200, , , , , "allow-forms")}}

In sostanza, lo styling del "cerchio interno" del radio button viene creato usando lo pseudo-elemento `::before`, ma impostando su di esso una {{cssxref("transform")}} `scale(0)`. Viene quindi usata una {{cssxref("transition")}} per animare piacevolmente la comparsa del contenuto generato sull'input quando il radio button viene selezionato. Il vantaggio dell'uso di una transform anziché di una transizione di {{cssxref("width")}}/{{cssxref("height")}} è che si può usare {{cssxref("transform-origin")}} per farlo crescere dal centro del cerchio, anziché farlo sembrare crescere dall'angolo del cerchio; inoltre, non si verifica alcun salto poiché nessun valore delle proprietà del box model viene aggiornato.

### :default e :indeterminate

Come indicato sopra, la pseudo-classe {{cssxref(":default")}} corrisponde a radio button/checkbox selezionati per impostazione predefinita al caricamento della pagina, anche quando vengono deselezionati. Questo potrebbe essere utile per aggiungere un indicatore a un elenco di opzioni e ricordare all'utente quali erano le opzioni predefinite, o iniziali, nel caso in cui desideri reimpostare le proprie scelte.

Inoltre, i radio button/checkbox menzionati sopra corrisponderanno alla pseudo-classe {{cssxref(":indeterminate")}} quando si trovano in uno stato in cui non sono né selezionati né deselezionati. Ma cosa significa? Gli elementi indeterminati includono:

- input {{HTMLElement("input/radio")}}, quando tutti i radio button in un gruppo con lo stesso nome sono deselezionati
- input {{HTMLElement("input/checkbox")}} la cui proprietà `indeterminate` è impostata su `true` tramite JavaScript
- elementi {{HTMLElement("progress")}} privi di valore.

Probabilmente questo non verrà usato molto spesso. Un caso d'uso potrebbe essere un indicatore per dire agli utenti che devono selezionare un radio button prima di procedere.

Esaminiamo un paio di versioni modificate dell'esempio precedente, che ricordano all'utente quale fosse l'opzione predefinita e applicano stili alle etichette dei radio button quando sono indeterminati. Entrambe presentano la seguente struttura HTML per gli input:

```html
<p>
  <input type="radio" name="fruit" value="cherry" id="cherry" />
  <label for="cherry">Cherry</label>
  <span></span>
</p>
```

Per l'esempio `:default`, l'attributo `checked` è stato aggiunto all'input del radio button centrale, quindi sarà selezionato per impostazione predefinita al caricamento. Viene quindi stilizzato con il seguente CSS:

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

Questo fornisce una piccola etichetta "Default" sull'elemento originariamente selezionato al caricamento della pagina. Si noti che qui viene usato il combinatore di fratelli successivi (`~`) anziché il combinatore del fratello successivo (`+`): è necessario farlo perché lo `<span>` non si trova immediatamente dopo l'`<input>` nell'ordine sorgente.

Il risultato dal vivo è visibile qui sotto (premere il pulsante **Play** per eseguire l'esempio in MDN Playground e modificare il codice sorgente):

```html hidden live-sample___radios-checked-default
<form>
  <fieldset>
    <legend>Choose your favorite fruit</legend>
    <p>
      <input type="radio" name="fruit" value="cherry" id="cherry" />
      <label for="cherry">Cherry</label>
      <span></span>
    </p>
    <p>
      <input type="radio" name="fruit" value="banana" id="banana" checked />
      <label for="banana">Banana</label>
      <span></span>
    </p>
    <p>
      <input type="radio" name="fruit" value="strawberry" id="strawberry" />
      <label for="strawberry">Strawberry</label>
      <span></span>
    </p>
  </fieldset>
</form>
```

```css hidden live-sample___radios-checked-default
@import "https://fonts.googleapis.com/css2?family=Josefin+Sans:ital,wght@0,100..700;1,100..700&display=swap";

body {
  font-family: "Josefin Sans", sans-serif;
}

input[type="radio"] {
  -webkit-appearance: none;
  appearance: none;
}

input[type="radio"] {
  width: 20px;
  height: 20px;
  border-radius: 10px;
  border: 2px solid gray;
  /* Adjusts the position of the checkboxes on the text baseline */
  vertical-align: -2px;
  outline: none;
}

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

{{EmbedLiveSample("radios-checked-default", "100%", 200, , , , , "allow-forms")}}

Per l'esempio `:indeterminate`, non è presente alcun radio button selezionato per impostazione predefinita: questo è importante; se ce ne fosse stato uno, non vi sarebbe stato alcuno stato indeterminato da stilizzare. I radio button indeterminati vengono stilizzati con il seguente CSS:

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

Questo crea un divertente piccolo contorno animato sui radio button, che dovrebbe indicare la necessità di selezionarne uno.

Il risultato dal vivo è visibile qui sotto (premere il pulsante **Play** per eseguire l'esempio in MDN Playground e modificare il codice sorgente):

```html hidden live-sample___radios-checked-indeterminate
<form>
  <fieldset>
    <legend>Choose your favorite fruit</legend>
    <p>
      <input type="radio" name="fruit" value="cherry" id="cherry" />
      <label for="cherry">Cherry</label>
      <span></span>
    </p>
    <p>
      <input type="radio" name="fruit" value="banana" id="banana" />
      <label for="banana">Banana</label>
      <span></span>
    </p>
    <p>
      <input type="radio" name="fruit" value="strawberry" id="strawberry" />
      <label for="strawberry">Strawberry</label>
      <span></span>
    </p>
  </fieldset>
</form>
```

```css hidden live-sample___radios-checked-indeterminate
@import "https://fonts.googleapis.com/css2?family=Josefin+Sans:ital,wght@0,100..700;1,100..700&display=swap";

body {
  font-family: "Josefin Sans", sans-serif;
}

input[type="radio"] {
  -webkit-appearance: none;
  appearance: none;
}

input[type="radio"] {
  width: 20px;
  height: 20px;
  border-radius: 10px;
  border: 2px solid gray;
  /* Adjusts the position of the checkboxes on the text baseline */
  vertical-align: -2px;
  outline: none;
}

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

input[type="radio"]:indeterminate {
  border: 2px solid red;
  animation: 0.4s linear infinite alternate border-pulse;
}

@keyframes border-pulse {
  from {
    border: 2px solid red;
  }

  to {
    border: 6px solid red;
  }
}
```

{{EmbedLiveSample("radios-checked-indeterminate", "100%", 200, , , , , "allow-forms")}}

> [!NOTE]
> Nella pagina di riferimento di [`<input type="checkbox">`](/it/docs/Web/HTML/Reference/Elements/input/checkbox) è disponibile un [interessante esempio che coinvolge gli stati `indeterminate`](/it/docs/Web/HTML/Reference/Elements/input/checkbox#indeterminate_state_checkboxes).

## Altre pseudo-classi

Esistono diverse altre pseudo-classi di interesse e qui non c'è spazio per descriverle tutte dettagliatamente. Vediamone alcune che meritano di essere approfondite.

- La pseudo-classe {{cssxref(":focus-within")}} corrisponde a un elemento che ha ricevuto il focus oppure che _contiene_ un elemento che ha ricevuto il focus. È utile se si desidera evidenziare in qualche modo un intero modulo quando un input al suo interno riceve il focus.
- La pseudo-classe {{cssxref(":focus-visible")}} corrisponde agli elementi con focus che hanno ricevuto il focus tramite interazione con la tastiera, anziché tramite tocco o mouse; è utile se si desidera mostrare uno stile differente per il focus da tastiera rispetto al focus del mouse, o di altro tipo.
- La pseudo-classe {{cssxref(":placeholder-shown")}} corrisponde agli elementi {{htmlelement('input')}} e {{htmlelement('textarea')}} che visualizzano il proprio placeholder, ovvero il contenuto dell'attributo [`placeholder`](/it/docs/Web/HTML/Reference/Elements/input#placeholder), perché il valore dell'elemento è vuoto.

Anche le seguenti sono interessanti, ma al momento non sono ben supportate nei browser:

- La pseudo-classe {{cssxref(":blank")}} seleziona i controlli dei moduli vuoti. {{cssxref(":empty")}} corrisponde anch'essa a elementi privi di figli, come {{HTMLElement("input")}}, ma è più generale: corrisponde anche ad altri {{Glossary("void_element", "elementi void")}} come {{HTMLElement("br")}} e {{HTMLElement("hr")}}. `:empty` ha un supporto ragionevole nei browser; la specifica della pseudo-classe `:blank` non è ancora terminata, pertanto non è ancora supportata in alcun browser.
- La pseudo-classe {{cssxref(":user-invalid")}}, quando supportata, sarà simile a {{cssxref(":invalid")}}, ma con una migliore esperienza utente. Se il valore è valido quando l'input riceve il focus, l'elemento può corrispondere a `:invalid` mentre l'utente inserisce dati se il valore è temporaneamente non valido, ma corrisponderà a `:user-invalid` solo quando l'elemento perde il focus. Se il valore era originariamente non valido, corrisponderà sia a `:invalid` sia a `:user-invalid` per tutta la durata del focus. In modo simile a `:invalid`, smetterà di corrispondere a `:user-invalid` se il valore diventa valido.

## Riepilogo

Questo conclude l'analisi delle pseudo-classi UI relative agli input dei moduli. È consigliabile continuare a sperimentare con esse e creare alcuni divertenti stili per moduli. Successivamente si passerà a qualcosa di diverso: la [convalida dei moduli lato client](/it/docs/Learn_web_development/Extensions/Forms/Form_validation).

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/Customizable_select_listboxes", "Learn_web_development/Extensions/Forms/Form_validation", "Learn_web_development/Extensions/Forms")}}
