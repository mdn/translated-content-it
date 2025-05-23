---
title: Convalida dei moduli lato client
slug: Learn_web_development/Extensions/Forms/Form_validation
l10n:
  sourceCommit: bccce51ad7f3fd5e5ff7e4231b6391a000c8faf6
---

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/UI_pseudo-classes", "Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data", "Learn_web_development/Extensions/Forms")}}

È importante assicurarsi che tutti i controlli del modulo richiesti siano compilati, nel formato corretto, prima di inviare i dati inseriti dall'utente al server. Questa **convalida dei moduli lato client** aiuta a garantire che i dati inseriti corrispondano ai requisiti stabiliti nei vari controlli del modulo.

Questo articolo guida attraverso i concetti base e gli esempi di convalida dei moduli lato client.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Alfabetizzazione informatica, una comprensione abbastanza buona di
        <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere cosa sia la convalida dei moduli lato client, perché sia importante,
        e come applicare varie tecniche per implementarla.
      </td>
    </tr>
  </tbody>
</table>

La convalida lato client è un controllo iniziale e una caratteristica importante di una buona esperienza utente; catturando i dati non validi lato client, l'utente può correggerli immediatamente. Se arrivano al server e vengono poi rifiutati, si provoca un ritardo significativo a causa di un viaggio di ritorno al server e poi di nuovo al lato client per comunicare all'utente di correggere i propri dati.

Tuttavia, la convalida lato client _non dovrebbe essere considerata_ una misura di sicurezza esaustiva! Le tue applicazioni devono sempre eseguire la convalida, compresi i controlli di sicurezza, su qualsiasi dato inviato tramite modulo anche lato _server_, oltre che lato client, perché la convalida lato client è troppo facile da aggirare, quindi utenti malintenzionati possono ancora facilmente inviare dati errati al tuo server.

> [!NOTE]
> Leggi [Sicurezza del sito web](/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Website_security) per avere un'idea di ciò che _potrebbe_ accadere; l'implementazione della convalida lato server è un po' al di fuori dell'ambito di questo modulo, ma dovresti tenerlo a mente.

## Che cos'è la convalida dei moduli?

Vai su qualsiasi sito popolare con un modulo di registrazione e noterai che essi forniscono feedback quando non inserisci i tuoi dati nel formato che si aspettano. Riceverai messaggi come:

- "Campo obbligatorio" (Non puoi lasciare questo campo vuoto).
- "Per favore inserisci il tuo numero di telefono nel formato xxx-xxxx" (È richiesto un formato dati specifico affinché sia considerato valido).
- "Per favore inserisci un indirizzo email valido" (i dati inseriti non sono nel formato corretto).
- "La tua password deve essere lunga tra 8 e 30 caratteri e contenere una lettera maiuscola, un simbolo e un numero." (È richiesto un formato dati molto specifico).

Questo è chiamato **convalida dei moduli**.
Quando inserisci i dati, il browser (e il web server) controlleranno che i dati siano nel formato corretto e all'interno dei vincoli stabiliti dall'applicazione. La convalida effettuata nel browser è chiamata convalida **lato client**, mentre la convalida effettuata sul server è chiamata convalida **lato server**. In questo capitolo ci concentriamo sulla convalida lato client.

Se l'informazione è formattata correttamente, l'applicazione consente l'invio dei dati al server e (di solito) la loro memorizzazione in un database; se l'informazione non è formattata correttamente, fornisce all'utente un messaggio di errore che spiega cosa deve essere corretto, e gli consente di riprovare.

Vogliamo rendere la compilazione dei moduli web il più facile possibile. Quindi, perché insistiamo nel convalidare i nostri moduli? Ci sono tre motivi principali:

- **Vogliamo ottenere i dati giusti, nel formato giusto.** Le nostre applicazioni non funzioneranno correttamente se i dati dei nostri utenti sono memorizzati nel formato sbagliato, sono errati o sono completamente omessi.
- **Vogliamo proteggere i dati dei nostri utenti.** Costringere i nostri utenti a inserire password sicure rende più facile proteggere le loro informazioni sull'account.
- **Vogliamo proteggere noi stessi.** Ci sono molti modi in cui utenti malintenzionati possono abusare di moduli non protetti per danneggiare l'applicazione. Vedi [Sicurezza del sito web](/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Website_security).

  > [!WARNING]
  > Non fidarti mai dei dati passati al tuo server dal client. Anche se il tuo modulo sta validando correttamente e prevenendo l'input malformato lato client, un utente malintenzionato può ancora modificare la richiesta di rete.

## Diversi tipi di convalida lato client

Esistono due diversi tipi di convalida lato client che incontrerai sul web:

- **Convalida dei moduli HTML**
  Gli attributi del modulo HTML possono definire quali controlli del modulo sono richiesti e in quale formato devono essere i dati inseriti dall'utente per essere validi.
- **Convalida dei moduli JavaScript**
  JavaScript è generalmente incluso per migliorare o personalizzare la convalida del modulo HTML.

La convalida lato client può essere realizzata con poco o senza JavaScript. La convalida HTML è più veloce di JavaScript, ma è meno personalizzabile. Si consiglia generalmente di iniziare i moduli utilizzando funzionalità HTML robuste, quindi migliorare l'esperienza utente con JavaScript secondo necessità.

## Uso della convalida integrata del modulo

Una delle caratteristiche più significative dei [controlli dei moduli](/it/docs/Learn_web_development/Extensions/Forms/HTML5_input_types) è la capacità di convalidare la maggior parte dei dati degli utenti senza fare affidamento su JavaScript. Questo viene fatto utilizzando attributi di convalida sugli elementi del modulo. Abbiamo visto molti di questi in precedenza nel corso, ma per riepilogare:

- [`required`](/it/docs/Web/HTML/Reference/Attributes/required): Specifica se un campo del modulo deve essere compilato prima che il modulo possa essere inviato.
- [`minlength`](/it/docs/Web/HTML/Reference/Attributes/minlength) e [`maxlength`](/it/docs/Web/HTML/Reference/Attributes/maxlength): Specifica la lunghezza minima e massima dei dati testuali (stringhe).
- [`min`](/it/docs/Web/HTML/Reference/Attributes/min), [`max`](/it/docs/Web/HTML/Reference/Attributes/max), e [`step`](/it/docs/Web/HTML/Reference/Attributes/step): Specifica i valori minimo e massimo dei tipi di input numerici e l'incremento o passo per i valori, a partire dal minimo.
- [`type`](/it/docs/Web/HTML/Reference/Elements/input#input_types): Specifica se i dati devono essere un numero, un indirizzo email, o un altro tipo predefinito specifico.
- [`pattern`](/it/docs/Web/HTML/Reference/Attributes/pattern): Specifica una [espressione regolare](/it/docs/Web/JavaScript/Guide/Regular_expressions) che definisce un modello che i dati inseriti devono seguire.

Se i dati inseriti in un campo del modulo seguono tutte le regole specificate dagli attributi applicati al campo, sono considerati validi. In caso contrario, sono considerati non validi.

Quando un elemento è valido, le seguenti cose sono vere:

- L'elemento corrisponde alla pseudo-classe CSS {{cssxref(":valid")}}, che consente di applicare uno stile specifico agli elementi validi. Il controllo corrisponderà anche a {{cssxref(":user-valid")}} se l'utente ha interagito con il controllo, e può corrispondere ad altre pseudo-classi UI, come {{cssxref(":in-range")}}, a seconda del tipo di input e degli attributi.
- Se l'utente prova a inviare i dati, il browser invierà il modulo, a patto che non ci sia nient'altro che lo impedisca (es. JavaScript).

Quando un elemento è invalido, le seguenti cose sono vere:

- L'elemento corrisponde alla pseudo-classe CSS {{cssxref(":invalid")}}. Se l'utente ha interagito con il controllo, corrisponde anche alla pseudo-classe CSS {{cssxref(":user-invalid")}}. Altre pseudo-classi UI possono anche corrispondere, come {{cssxref(":out-of-range")}}, a seconda dell'errore. Queste consentono di applicare uno stile specifico agli elementi non validi.
- Se l'utente prova a inviare i dati, il browser bloccherà l'invio del modulo e mostrerà un messaggio di errore. Il messaggio di errore differirà a seconda del tipo di errore. L'API [Constraint Validation](#l'api_di_convalida_dei_vincoli) è descritta di seguito.

## Esempi di convalida del modulo integrata

In questa sezione, testeremo alcuni degli attributi di cui abbiamo discusso sopra.

### File di avvio semplice

Iniziamo con un esempio semplice: un input che ti consente di scegliere se preferisci una banana o una ciliegia. Questo esempio coinvolge un testo di base {{HTMLElement("input")}} con un {{htmlelement("label")}} associato e un {{htmlelement("button")}} di invio.

```html
<form>
  <label for="choose">Would you prefer a banana or cherry?</label>
  <input id="choose" name="i-like" />
  <button>Submit</button>
</form>
```

```css
input:invalid {
  border: 2px dashed red;
}

input:valid {
  border: 2px solid black;
}
```

{{EmbedLiveSample("Simple_start_file", "100%", 80)}}

Per iniziare, fai una copia del [file `fruit-start.html` trovato su GitHub](https://github.com/mdn/learning-area/blob/main/html/forms/form-validation/fruit-start.html) in una nuova directory sul tuo hard disk.

### L'attributo required

Una funzione comune di convalida HTML è l'attributo [`required`](/it/docs/Web/HTML/Reference/Attributes/required). Aggiungi questo attributo a un input per rendere un elemento obbligatorio. Quando questo attributo è impostato, l'elemento corrisponde alla pseudo-classe UI {{cssxref(':required')}} e il modulo non verrà inviato, visualizzando un messaggio di errore all'invio, se l'input è vuoto. Mentre vuoto, l'input sarà anche considerato invalido, corrispondendo alla pseudo-classe UI {{cssxref(':invalid')}}.

Se un qualsiasi pulsante radio in un gruppo con lo stesso nome ha l'attributo `required`, uno dei pulsanti radio in quel gruppo deve essere selezionato affinché il gruppo sia valido; il pulsante selezionato non deve necessariamente essere quello con l'attributo impostato.

> [!NOTE]
> Richiedi solo che gli utenti inseriscano i dati di cui hai bisogno: per esempio, è davvero necessario sapere il genere o il titolo di qualcuno?

Aggiungi un attributo `required` al tuo input, come mostrato di seguito.

```html
<form>
  <label for="choose">Would you prefer a banana or cherry? (required)</label>
  <input id="choose" name="i-like" required />
  <button>Submit</button>
</form>
```

Abbiamo aggiunto "(required)" al {{htmlelement("label")}} per informare l'utente che l'{{htmlelement("input")}} è richiesto. Indicare all'utente quando i campi del modulo sono richiesti non è solo una buona esperienza utente, ma è richiesto dalle linee guida WCAG per l'[accessibilità](/it/docs/Learn_web_development/Core/Accessibility).

Includiamo stili CSS che sono applicati in base al fatto che l'elemento sia richiesto, valido o invalido:

```css
input:invalid {
  border: 2px dashed red;
}

input:invalid:required {
  background-image: linear-gradient(to right, pink, lightgreen);
}

input:valid {
  border: 2px solid black;
}
```

Questo CSS fa sì che l'input abbia un bordo tratteggiato rosso quando è invalido e un bordo nero solido più sottile quando è valido. Abbiamo anche aggiunto un gradiente di sfondo quando l'input è richiesto _e_ invalido. Prova il nuovo comportamento nell'esempio qui sotto:

{{EmbedLiveSample("The_required_attribute", "100%", 80)}}

Prova a inviare il modulo dall'[esempio live `required`](https://mdn.github.io/learning-area/html/forms/form-validation/fruit-required.html) senza un valore. Nota come l'input non valido ottiene il focus, appare un messaggio di errore predefinito ("Per favore compila questo campo"), e il modulo è impedito dall'essere inviato. Puoi anche vedere il [codice sorgente su GitHub](https://github.com/mdn/learning-area/blob/main/html/forms/form-validation/fruit-required.html).

### Convalida rispetto a un'espressione regolare

Un'altra utile funzione di convalida è l'attributo [`pattern`](/it/docs/Web/HTML/Reference/Attributes/pattern), che si aspetta un [Espressione Regolare](/it/docs/Web/JavaScript/Guide/Regular_expressions) come suo valore. Un'espressione regolare (regexp) è un modello che può essere usato per confrontare combinazioni di caratteri in stringhe di testo, quindi le regex sono ideali per la convalida dei moduli e servono a una varietà di altri usi in JavaScript.

Le regex sono piuttosto complesse e non intendiamo insegnartele in modo esaustivo in questo articolo. Di seguito sono riportati alcuni esempi per darti un'idea di base di come funzionano.

- `a` — Corrisponde a un carattere che è `a` (non `b`, non `aa`, e così via).
- `abc` — Corrisponde a `a`, seguito da `b`, seguito da `c`.
- `ab?c` — Corrisponde a `a`, facoltativamente seguito da un singolo `b`, seguito da `c`. (`ac` o `abc`)
- `ab*c` — Corrisponde a `a`, facoltativamente seguito da qualsiasi numero di `b`, seguito da `c`. (`ac`, `abc`, `abbbbbc`, e così via).
- `a|b` — Corrisponde a un carattere che è `a` o `b`.
- `abc|xyz` — Corrisponde esattamente a `abc` o esattamente a `xyz` (ma non `abcxyz` o `a` o `y`, e così via).

Ci sono molte altre possibilità che non copriamo qui. Per un elenco completo e molti esempi, consulta la nostra documentazione sulle [Espressioni regolari](/it/docs/Web/JavaScript/Guide/Regular_expressions).

Implementiamo un esempio. Aggiorna il tuo HTML per aggiungere un attributo [`pattern`](/it/docs/Web/HTML/Reference/Attributes/pattern) come questo:

```html
<form>
  <label for="choose">Would you prefer a banana or a cherry?</label>
  <input id="choose" name="i-like" required pattern="[Bb]anana|[Cc]herry" />
  <button>Submit</button>
</form>
```

```css hidden
input:invalid {
  border: 2px dashed red;
}

input:valid {
  border: 2px solid black;
}
```

Questo ci dà il seguente aggiornamento — provalo:

{{EmbedLiveSample("Validating_against_a_regular_expression", "100%", 80)}}

Puoi trovare questo esempio live su GitHub come [fruit-pattern.html](https://mdn.github.io/learning-area/html/forms/form-validation/fruit-pattern.html) insieme al [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/forms/form-validation/fruit-pattern.html).

In questo esempio, il {{HTMLElement("input")}} accetta uno dei quattro possibili valori: le stringhe "banana", "Banana", "cherry", o "Cherry". Le espressioni regolari sono case-sensitive, ma abbiamo fatto in modo che supportino versioni con la prima lettera maiuscola e minuscola usando un modello extra "Aa" all'interno di parentesi quadre.

A questo punto, prova a modificare il valore all'interno dell'attributo [`pattern`](/it/docs/Web/HTML/Reference/Attributes/pattern) per eguagliare alcuni degli esempi che hai visto in precedenza, e guarda come ciò influisce sui valori che puoi inserire per rendere valido il valore dell'input. Prova a scrivere alcuni dei tuoi e verifica come procede. Fai in modo che siano correlati alla frutta, se possibile, in modo che i tuoi esempi abbiano senso!

Se un valore non vuoto del {{HTMLElement("input")}} non corrisponde al modello dell'espressione regolare, l'`input` corrisponderà alla pseudo-classe {{cssxref(':invalid')}}. Se vuoto, e l'elemento non è richiesto, non è considerato invalido.

Alcuni tipi di elemento {{HTMLElement("input")}} non hanno bisogno di un attributo [`pattern`](/it/docs/Web/HTML/Reference/Attributes/pattern) per essere convalidati rispetto a un'espressione regolare. Per esempio, specificare il tipo `email` convalida il valore degli input rispetto a un modello di indirizzo email ben formato o un modello che corrisponde a un elenco separato da virgole di indirizzi email se ha l'attributo [`multiple`](/it/docs/Web/HTML/Reference/Attributes/multiple).

> [!NOTE]
> L'elemento {{HTMLElement("textarea")}} non supporta l'attributo [`pattern`](/it/docs/Web/HTML/Reference/Attributes/pattern).

### Limitazione della lunghezza delle tue voci

Puoi limitare la lunghezza del carattere di tutti i campi di testo creati da {{HTMLElement("input")}} o {{HTMLElement("textarea")}} utilizzando gli attributi [`minlength`](/it/docs/Web/HTML/Reference/Attributes/minlength) e [`maxlength`](/it/docs/Web/HTML/Reference/Attributes/maxlength). Un campo è invalido se ha un valore e quel valore ha meno caratteri del valore [`minlength`](/it/docs/Web/HTML/Reference/Attributes/minlength) o più del valore [`maxlength`](/it/docs/Web/HTML/Reference/Attributes/maxlength).

I browser spesso non consentono all'utente di digitare un valore più lungo del previsto nei campi di testo. Una migliore esperienza utente rispetto all'uso solo di `maxlength` consiste nel fornire anche un feedback sul conteggio dei caratteri in modo accessibile e consentire all'utente di modificare i suoi contenuti per adattarli alla dimensione. Un esempio di questo è il limite di caratteri quando si pubblica sui social media. JavaScript, inclusi [soluzioni che usano `maxlength`](https://github.com/mimo84/bootstrap-maxlength), può essere usato per fornire questo.

> [!NOTE]
> I vincoli di lunghezza non vengono mai riportati se il valore viene impostato programmaticamente. Sono riportati solo per l'input fornito dall'utente.

### Limitazione dei valori delle tue voci

Per i campi numerici, inclusi [`<input type="number">`](/it/docs/Web/HTML/Reference/Elements/input/number) e i vari tipi di input di data, gli attributi [`min`](/it/docs/Web/HTML/Reference/Attributes/min) e [`max`](/it/docs/Web/HTML/Reference/Attributes/max) possono essere usati per fornire un intervallo di valori validi. Se il campo contiene un valore al di fuori di questo intervallo, sarà invalido.

Vediamo un altro esempio. Crea una nuova copia del file [fruit-start.html](https://github.com/mdn/learning-area/blob/main/html/forms/form-validation/fruit-start.html).

Ora elimina i contenuti dell'elemento `<body>`, e sostituiscili con i seguenti:

```html
<form>
  <div>
    <label for="choose">Would you prefer a banana or a cherry?</label>
    <input
      type="text"
      id="choose"
      name="i-like"
      required
      minlength="6"
      maxlength="6" />
  </div>
  <div>
    <label for="number">How many would you like?</label>
    <input type="number" id="number" name="amount" value="1" min="1" max="10" />
  </div>
  <div>
    <button>Submit</button>
  </div>
</form>
```

- Qui vedrai che abbiamo dato al campo `text` un `minlength` e `maxlength` di sei, che è la stessa lunghezza di banana e ciliegia.
- Abbiamo anche dato al campo `number` un `min` di uno e un `max` di dieci. I numeri inseriti al di fuori di questo intervallo appariranno come non validi; gli utenti non potranno utilizzare le frecce di incremento/decremento per spostare il valore al di fuori di questo intervallo. Se l'utente inserisce manualmente un numero al di fuori di questo intervallo, i dati sono non validi. Il numero non è richiesto, quindi rimuovere il valore risulterà in un valore valido.

```css hidden
input:invalid {
  border: 2px dashed red;
}

input:valid {
  border: 2px solid black;
}

div {
  margin-bottom: 10px;
}
```

Ecco l'esempio in esecuzione live:

{{EmbedLiveSample("Constraining_the_values_of_your_entries", "100%", 100)}}

Prova questo [esempio live su GitHub](https://mdn.github.io/learning-area/html/forms/form-validation/fruit-length.html) e visualizza il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/forms/form-validation/fruit-length.html).

I tipi di input numerici, come `number`, `range` e `date`, possono anche prendere l'attributo [`step`](/it/docs/Web/HTML/Reference/Attributes/step). Questo attributo specifica quale incremento il valore aumenterà o diminuirà quando vengono utilizzati i controlli di input (come i pulsanti su e giù del numero o lo scorrimento del cursore dei range). L'attributo `step` è omesso nel nostro esempio, quindi il valore predefinito è `1`. Questo significa che i numeri in virgola mobile, come 3.2, appariranno anche come non validi.

### Esempio completo

Ecco un esempio completo per mostrare l'utilizzo delle funzionalità di convalida integrate di HTML. Innanzitutto, un po' di HTML:

```html
<form>
  <fieldset>
    <legend>
      Do you have a driver's license?<span aria-label="required">*</span>
    </legend>
    <input type="radio" required name="driver" id="r1" value="yes" /><label
      for="r1"
      >Yes</label
    >
    <input type="radio" required name="driver" id="r2" value="no" /><label
      for="r2"
      >No</label
    >
  </fieldset>
  <p>
    <label for="n1">How old are you?</label>
    <input type="number" min="12" max="120" step="1" id="n1" name="age" />
  </p>
  <p>
    <label for="t1"
      >What's your favorite fruit?<span aria-label="required">*</span></label
    >
    <input
      type="text"
      id="t1"
      name="fruit"
      list="l1"
      required
      pattern="[Bb]anana|[Cc]herry|[Aa]pple|[Ss]trawberry|[Ll]emon|[Oo]range" />
    <datalist id="l1">
      <option>Banana</option>
      <option>Cherry</option>
      <option>Apple</option>
      <option>Strawberry</option>
      <option>Lemon</option>
      <option>Orange</option>
    </datalist>
  </p>
  <p>
    <label for="t2">What's your email address?</label>
    <input type="email" id="t2" name="email" />
  </p>
  <p>
    <label for="t3">Leave a short message</label>
    <textarea id="t3" name="msg" maxlength="140" rows="5"></textarea>
  </p>
  <p>
    <button>Submit</button>
  </p>
</form>
```

E ora un po' di CSS per stilizzare l'HTML:

```css
form {
  font: 1em sans-serif;
  max-width: 320px;
}

p > label {
  display: block;
}

input[type="text"],
input[type="email"],
input[type="number"],
textarea,
fieldset {
  width: 100%;
  border: 1px solid #333;
  box-sizing: border-box;
}

input:invalid {
  box-shadow: 0 0 5px 1px red;
}

input:focus:invalid {
  box-shadow: none;
}
```

Ciò si traduce nel seguente aspetto:

{{EmbedLiveSample("Full_example", "100%", 420)}}

Questo [esempio completo è live su GitHub](https://mdn.github.io/learning-area/html/forms/form-validation/full-example.html) insieme al [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/forms/form-validation/full-example.html).

Consulta [Attributi correlati alla convalida](/it/docs/Web/HTML/Guides/Constraint_validation#validation-related_attributes) per un elenco completo di attributi che possono essere utilizzati per vincolare i valori di input e i tipi di input che li supportano.

## Convalida dei moduli utilizzando JavaScript

Se desideri cambiare il testo dei messaggi di errore nativi, è necessario JavaScript. In questa sezione esamineremo i diversi modi per farlo.

### L'API di convalida dei vincoli

L'API di Convalida dei Vincoli consiste in un insieme di metodi e proprietà disponibili sulle seguenti interfacce DOM degli elementi del modulo:

- [`HTMLButtonElement`](/it/docs/Web/API/HTMLButtonElement) (rappresenta un elemento [`<button>`](/it/docs/Web/HTML/Reference/Elements/button))
- [`HTMLFieldSetElement`](/it/docs/Web/API/HTMLFieldSetElement) (rappresenta un elemento [`<fieldset>`](/it/docs/Web/HTML/Reference/Elements/fieldset))
- [`HTMLInputElement`](/it/docs/Web/API/HTMLInputElement) (rappresenta un elemento [`<input>`](/it/docs/Web/HTML/Reference/Elements/input))
- [`HTMLOutputElement`](/it/docs/Web/API/HTMLOutputElement) (rappresenta un elemento [`<output>`](/it/docs/Web/HTML/Reference/Elements/output))
- [`HTMLSelectElement`](/it/docs/Web/API/HTMLSelectElement) (rappresenta un elemento [`<select>`](/it/docs/Web/HTML/Reference/Elements/select))
- [`HTMLTextAreaElement`](/it/docs/Web/API/HTMLTextAreaElement) (rappresenta un elemento [`<textarea>`](/it/docs/Web/HTML/Reference/Elements/textarea))

L'API di Convalida dei Vincoli rende disponibili le seguenti proprietà sugli elementi sopra.

- `validationMessage`: Restituisce un messaggio localizzato che descrive i vincoli di convalida che il controllo non soddisfa (se presenti). Se il controllo non è un candidato per la convalida dei vincoli (`willValidate` è `false`) o il valore dell'elemento soddisfa i suoi vincoli (è valido), questo restituirà una stringa vuota.
- `validity`: Restituisce un oggetto `ValidityState` che contiene diverse proprietà che descrivono lo stato di validità dell'elemento. Puoi trovare tutti i dettagli completi di tutte le proprietà disponibili nella pagina di riferimento [`ValidityState`](/it/docs/Web/API/ValidityState); qui sotto sono elencate alcune delle più comuni:

  - [`patternMismatch`](/it/docs/Web/API/ValidityState/patternMismatch): Restituisce `true` se il valore non corrisponde al [`pattern`](/it/docs/Web/HTML/Reference/Elements/input#pattern) specificato, e `false` se lo fa. Se vero, l'elemento corrisponde alla pseudo-classe CSS {{cssxref(":invalid")}}.
  - [`tooLong`](/it/docs/Web/API/ValidityState/tooLong): Restituisce `true` se il valore è più lungo della lunghezza massima specificata dall'attributo [`maxlength`](/it/docs/Web/HTML/Reference/Elements/input#maxlength), o `false` se è più corto o uguale al massimo. Se vero, l'elemento corrisponde alla pseudo-classe CSS {{cssxref(":invalid")}}.
  - [`tooShort`](/it/docs/Web/API/ValidityState/tooShort): Restituisce `true` se il valore è più corto della lunghezza minima specificata dall'attributo [`minlength`](/it/docs/Web/HTML/Reference/Elements/input#minlength), o `false` se è più grande o uguale al minimo. Se vero, l'elemento corrisponde alla pseudo-classe CSS {{cssxref(":invalid")}}.
  - [`rangeOverflow`](/it/docs/Web/API/ValidityState/rangeOverflow): Restituisce `true` se il valore è maggiore del massimo specificato dall'attributo [`max`](/it/docs/Web/HTML/Reference/Elements/input#max), o `false` se è minore o uguale al massimo. Se vero, l'elemento corrisponde alle pseudo-classi CSS {{cssxref(":invalid")}} e {{cssxref(":out-of-range")}}.
  - [`rangeUnderflow`](/it/docs/Web/API/ValidityState/rangeUnderflow): Restituisce `true` se il valore è minore del minimo specificato dall'attributo [`min`](/it/docs/Web/HTML/Reference/Elements/input#min), o `false` se è più grande o uguale al minimo. Se vero, l'elemento corrisponde alle pseudo-classi CSS {{cssxref(":invalid")}} e {{cssxref(":out-of-range")}}.
  - [`typeMismatch`](/it/docs/Web/API/ValidityState/typeMismatch): Restituisce `true` se il valore non è nella sintassi richiesta (quando [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) è `email` o `url`), o `false` se la sintassi è corretta. Se `true`, l'elemento corrisponde alla pseudo-classe CSS {{cssxref(":invalid")}}.
  - `valid`: Restituisce `true` se l'elemento soddisfa tutti i suoi vincoli di convalida, ed è quindi considerato valido, o `false` se fallisce in uno qualsiasi dei vincoli. Se vero, l'elemento corrisponde alla pseudo-classe CSS {{cssxref(":valid")}}; altrimenti alla pseudo-classe CSS {{cssxref(":invalid")}}.
  - `valueMissing`: Restituisce `true` se l'elemento ha un attributo [`required`](/it/docs/Web/HTML/Reference/Elements/input#required), ma nessun valore, o `false` altrimenti. Se vero, l'elemento corrisponde alla pseudo-classe CSS {{cssxref(":invalid")}}.

- `willValidate`: Restituisce `true` se l'elemento verrà convalidato quando il modulo viene inviato; `false` altrimenti.

L'API di Convalida dei Vincoli rende disponibili anche i seguenti metodi sugli elementi sopra e sull'elemento [`form`](/it/docs/Web/HTML/Reference/Elements/form).

- `checkValidity()`: Restituisce `true` se il valore dell'elemento non presenta problemi di validità; `false` altrimenti. Se l'elemento è invalido, questo metodo attiva anche un [`evento invalid`](/it/docs/Web/API/HTMLInputElement/invalid_event) sull'elemento.
- `reportValidity()`: Segnala il/i campo/i invalido/i utilizzando eventi. Questo metodo è utile in combinazione con `preventDefault()` in un gestore di eventi `onSubmit`.
- `setCustomValidity(message)`: Aggiunge un messaggio di errore personalizzato all'elemento; se imposti un messaggio di errore personalizzato, l'elemento è considerato invalido e viene visualizzato l'errore specificato. Questo ti consente di utilizzare il codice JavaScript per stabilire un errore di convalida diverso da quelli offerti dai vincoli di convalida HTML standard. Il messaggio viene mostrato all'utente quando si segnala il problema.

#### Implementare un messaggio di errore personalizzato

Come hai visto negli esempi di vincoli di convalida HTML precedenti, ogni volta che un utente prova a inviare un modulo invalido, il browser mostra un messaggio di errore. Il modo in cui questo messaggio è visualizzato dipende dal browser.

Questi messaggi automatici hanno due svantaggi:

- Non c'è un modo standard per cambiare il loro aspetto con il CSS.
- Dipendono dalla localizzazione del browser, il che significa che puoi avere una pagina in una lingua ma un messaggio di errore visualizzato in un'altra lingua, come si vede nel seguente screenshot di Firefox.

![Esempio di un messaggio di errore con Firefox in francese su una pagina in inglese](error-firefox-win7.png)

Personalizzare questi messaggi di errore è uno dei casi d'uso più comuni dell'API di Convalida dei Vincoli. Lavoriamo attraverso un esempio di come farlo.

Inizieremo con qualche HTML (sentiti libero di metterlo in un file HTML vuoto; usa una copia fresca di [fruit-start.html](https://github.com/mdn/learning-area/blob/main/html/forms/form-validation/fruit-start.html) come base, se ti va):

```html
<form>
  <label for="mail">
    I would like you to provide me with an email address:
  </label>
  <input type="email" id="mail" name="mail" />
  <button>Submit</button>
</form>
```

Aggiungi il seguente JavaScript alla pagina:

```js
const email = document.getElementById("mail");

email.addEventListener("input", (event) => {
  if (email.validity.typeMismatch) {
    email.setCustomValidity("I am expecting an email address!");
  } else {
    email.setCustomValidity("");
  }
});
```

Qui memorizziamo un riferimento al campo email, poi aggiungiamo un gestore di eventi ad esso che esegue il codice contenuto ogni volta che il valore all'interno dell'input viene modificato.

All'interno del codice contenuto, verifichiamo se la proprietà `validity.typeMismatch` dell'input email restituisce `true`, il che significa che il valore contenuto non corrisponde al modello per un indirizzo email ben formato. Se è così, chiamiamo il metodo [`setCustomValidity()`](/it/docs/Web/API/HTMLInputElement/setCustomValidity) con un messaggio personalizzato. Questo rende l'input invalido, così che quando provi a inviare il modulo, l'invio fallisce e viene mostrato il messaggio di errore personalizzato.

Se la proprietà `validity.typeMismatch` restituisce `false`, chiamiamo il metodo `setCustomValidity()` con una stringa vuota. Questo rende valido l'input, quindi il modulo verrà inviato. Durante la convalida, se un qualsiasi controllo del modulo ha un `customError` che non è la stringa vuota, l'invio del modulo è bloccato.

Puoi provarlo di seguito:

{{EmbedGHLiveSample("learning-area/html/forms/form-validation/custom-error-message.html", '100%', 120)}}

Puoi trovare questo esempio live su GitHub come [custom-error-message.html](https://mdn.github.io/learning-area/html/forms/form-validation/custom-error-message.html), insieme al [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/forms/form-validation/custom-error-message.html).

#### Estendere la convalida del modulo integrata

L'esempio precedente ha mostrato come puoi aggiungere un messaggio personalizzato per un particolare tipo di errore (`validity.typeMismatch`). È anche possibile utilizzare tutta la convalida integrata del modulo HTML, e poi aggiungerle utilizzando `setCustomValidity()`.

Qui dimostriamo come puoi estendere la convalida del modulo integrata per accettare solo indirizzi con il dominio `@example.com`. Iniziamo con il modulo HTML {{htmlelement("form")}} qui sotto.

```html
<form>
  <label for="mail">Email address (@example.com only):</label>
  <input type="email" id="mail" />
  <button>Submit</button>
</form>
```

Il codice di convalida è mostrato di seguito. In caso di qualsiasi nuovo input, il codice resetta prima il messaggio di validità personalizzato chiamando `setCustomValidity("")`. Quindi utilizza `email.validity.valid` per verificare se l'indirizzo inserito è invalido e, in tal caso, ritorna dal gestore di eventi. Questo assicura che tutti i controlli di validità integrati vengano eseguiti mentre il testo inserito non è un indirizzo email valido.

Una volta che l'indirizzo email è valido, il codice aggiunge un vincolo personalizzato, chiamando `setCustomValidity()` con un messaggio di errore se l'indirizzo non termina con `@example.com`.

```js
const email = document.getElementById("mail");

email.addEventListener("input", (event) => {
  // Validate with the built-in constraints
  email.setCustomValidity("");
  if (!email.validity.valid) {
    return;
  }

  // Extend with a custom constraints
  if (!email.value.endsWith("@example.com")) {
    email.setCustomValidity("Please enter an email address of @example.com");
  }
});
```

Puoi provare questo esempio nella pagina al {{LiveSampleLink('Extending_built-in_form_validation', 'Live sample demo link')}}. Prova a inviare un indirizzo email non valido, un indirizzo email valido che non termina in `@example.com`, e uno che termina in `@example.com`.

#### Un esempio più dettagliato

Ora che abbiamo visto un esempio davvero basilare, vediamo come possiamo usare questa API per costruire una convalida personalizzata leggermente più complessa.

Prima, l'HTML. Ancora una volta, sentiti libero di costruirlo insieme a noi:

```html
<form novalidate>
  <p>
    <label for="mail">
      <span>Please enter an email address:</span>
      <input type="email" id="mail" name="mail" required minlength="8" />
      <span class="error" aria-live="polite"></span>
    </label>
  </p>
  <button>Submit</button>
</form>
```

Questo modulo utilizza l'attributo [`novalidate`](/it/docs/Web/HTML/Reference/Elements/form#novalidate) per disattivare la validazione automatica del browser. Impostare l'attributo `novalidate` sul modulo impedisce al modulo di mostrare i propri messaggi di errore a bolle, e ci permette invece di visualizzare i messaggi di errore personalizzati nel DOM in qualche modo a nostra scelta. Tuttavia, questo non disabilita il supporto per l'API di convalida dei vincoli né l'applicazione delle pseudo-classi CSS come {{cssxref(":valid")}}, ecc. Ciò significa che anche se il browser non controlla automaticamente la validità del modulo prima di inviarne i dati, puoi ancora farlo tu stesso e stilare il modulo di conseguenza.

Il nostro input da convalidare è un [`<input type="email">`](/it/docs/Web/HTML/Reference/Elements/input/email), che è `required`, e ha una `minlength` di 8 caratteri. Controlliamo questi con il nostro codice, e mostriamo un messaggio di errore personalizzato per ciascuno.

Miriamo a mostrare i messaggi di errore all'interno di un elemento `<span>`. L'attributo [`aria-live`](/it/docs/Web/Accessibility/ARIA/Guides/Live_regions) è impostato su quel `<span>` per assicurarsi che il nostro messaggio di errore personalizzato venga presentato a tutti, compreso il leggere per gli utenti di screen reader.

Ora un po' di CSS di base per migliorare leggermente l'aspetto del modulo e fornire un feedback visivo quando i dati di input sono invalidi:

```css
body {
  font: 1em sans-serif;
  width: 200px;
  padding: 0;
  margin: 0 auto;
}

p * {
  display: block;
}

input[type="email"] {
  appearance: none;

  width: 100%;
  border: 1px solid #333;
  margin: 0;

  font-family: inherit;
  font-size: 90%;

  box-sizing: border-box;
}

/* invalid fields */
input:invalid {
  border-color: #900;
  background-color: #fdd;
}

input:focus:invalid {
  outline: none;
}

/* error message styles */
.error {
  width: 100%;
  padding: 0;

  font-size: 80%;
  color: white;
  background-color: #900;
  border-radius: 0 0 5px 5px;

  box-sizing: border-box;
}

.error.active {
  padding: 0.3em;
}
```

Ora diamo uno sguardo al JavaScript che implementa la convalida degli errori personalizzata. Ci sono molti modi per selezionare un nodo DOM; qui otteniamo il modulo stesso e la casella di input email, nonché l'elemento span in cui inseriremo il messaggio di errore.

Utilizzando i gestori di eventi, controlliamo se i campi del modulo sono validi ogni volta che l'utente digita qualcosa. Se c'è un errore, lo mostriamo. Se non c'è errore, rimuoviamo qualsiasi messaggio di errore.

```js
const form = document.querySelector("form");
const email = document.getElementById("mail");
const emailError = document.querySelector("#mail + span.error");

email.addEventListener("input", (event) => {
  if (email.validity.valid) {
    emailError.textContent = ""; // Remove the message content
    emailError.className = "error"; // Removes the `active` class
  } else {
    // If there is still an error, show the correct error
    showError();
  }
});

form.addEventListener("submit", (event) => {
  // if the email field is invalid
  if (!email.validity.valid) {
    // display an appropriate error message
    showError();
    // prevent form submission
    event.preventDefault();
  }
});

function showError() {
  if (email.validity.valueMissing) {
    // If empty
    emailError.textContent = "You need to enter an email address.";
  } else if (email.validity.typeMismatch) {
    // If it's not an email address,
    emailError.textContent = "Entered value needs to be an email address.";
  } else if (email.validity.tooShort) {
    // If the value is too short,
    emailError.textContent = `Email should be at least ${email.minLength} characters; you entered ${email.value.length}.`;
  }
  // Add the `active` class
  emailError.className = "error active";
}
```

Ogni volta che cambiamo il valore dell'input, controlliamo se contiene dati validi. Se è così, rimuoviamo qualsiasi messaggio di errore mostrato. Se i dati non sono validi, eseguiamo `showError()` per mostrare l'errore appropriato.

Ogni volta che proviamo a inviare il modulo, controlliamo di nuovo se i dati sono validi. Se lo sono, lasciamo che il modulo venga inviato. Se no, eseguiamo `showError()` per mostrare l'errore appropriato e fermiamo l'invio del modulo con [`preventDefault()`](/it/docs/Web/API/Event/preventDefault).

La funzione `showError()` utilizza varie proprietà dell'oggetto `validity` dell'input per determinare quale sia l'errore, e quindi visualizza un messaggio di errore appropriato.

Ecco il risultato live:

{{EmbedGHLiveSample("learning-area/html/forms/form-validation/detailed-custom-validation.html", '100%', 150)}}

Puoi trovare questo esempio live su GitHub come [detailed-custom-validation.html](https://mdn.github.io/learning-area/html/forms/form-validation/detailed-custom-validation.html) insieme al [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/forms/form-validation/detailed-custom-validation.html).

L'API di convalida dei vincoli ti offre uno strumento potente per gestire la convalida dei moduli, permettendoti di avere un enorme controllo sull'interfaccia utente al di là di ciò che puoi fare solo con HTML e CSS.

### Convalida dei moduli senza un'API integrata

In alcuni casi, come i [controlli personalizzati](/it/docs/Learn_web_development/Extensions/Forms/How_to_build_custom_form_controls), non sarai in grado o non vorrai utilizzare l'API di convalida dei vincoli. Puoi ancora utilizzare JavaScript per convalidare il tuo modulo, ma dovrai semplicemente scriverne uno tuo.

Per convalidare un modulo, chiediti alcune domande:

- Che tipo di convalida dovrei eseguire?
  - : È necessario determinare come convalidare i tuoi dati: operazioni di stringa, conversione dei tipi, espressioni regolari, e così via. Sta a te.
- Cosa dovrei fare se il modulo non è valido?
  - : Questo è chiaramente un problema di UI. Devi decidere come si comporterà il modulo. Il modulo invia comunque i dati? Dovresti evidenziare i campi che sono in errore? Dovresti visualizzare messaggi di errore?
- Come posso aiutare l'utente a correggere i dati non validi?

  - : Per ridurre la frustrazione dell'utente, è molto importante fornire quante più informazioni utili possibile per guidarlo nella correzione degli input. Dovresti offrire suggerimenti preventivi in modo che sappiano cosa ci si aspetta, così come chiari messaggi di errore. Se vuoi approfondire i requisiti dell'interfaccia utente della convalida dei moduli, ecco alcuni articoli utili che dovresti leggere:

    - [Aiutare gli utenti a inserire i dati giusti nei moduli](https://web.dev/learn/forms/form-fields)
    - [Convalida degli input](https://www.w3.org/WAI/tutorials/forms/validation/)
    - [Come segnalare errori nei moduli: 10 linee guida di progettazione](https://www.nngroup.com/articles/errors-forms-design-guidelines/)

#### Un esempio che non utilizza l'API di convalida dei vincoli

Per illustrare ciò, il seguente è una versione semplificata dell'esempio precedente senza l'API di Convalida dei Vincoli.

L'HTML è quasi lo stesso; abbiamo solo rimosso le funzionalità di validazione HTML.

```html
<form>
  <p>
    <label for="mail">
      <span>Please enter an email address:</span>
    </label>
    <input type="text" id="mail" name="mail" />
    <span id="error" aria-live="polite"></span>
  </p>
  <button>Submit</button>
</form>
```

Allo stesso modo, il CSS non necessita di cambiare molto; abbiamo semplicemente trasformato la pseudo-classe CSS {{cssxref(":invalid")}} in una classe reale ed evitato di utilizzare il selettore di attributo.

```css
body {
  font: 1em sans-serif;
  width: 200px;
  padding: 0;
  margin: 0 auto;
}

form {
  max-width: 200px;
}

p * {
  display: block;
}

input {
  appearance: none;
  width: 100%;
  border: 1px solid #333;
  margin: 0;

  font-family: inherit;
  font-size: 90%;

  box-sizing: border-box;
}

/* invalid fields */
input.invalid {
  border: 2px solid #900;
  background-color: #fdd;
}

input:focus.invalid {
  outline: none;
  /* make sure keyboard-only users see a change when focusing */
  border-style: dashed;
}

/* error messages */
#error {
  width: 100%;
  font-size: 80%;
  color: white;
  background-color: #900;
  border-radius: 0 0 5px 5px;
  box-sizing: border-box;
}

.active {
  padding: 0.3rem;
}
```

Le grandi modifiche sono nel codice JavaScript, che deve fare molto più lavoro pesante.

```js
const form = document.querySelector("form");
const email = document.getElementById("mail");
const error = document.getElementById("error");

// Regular expression for email validation as per HTML specification
const emailRegExp = /^[\w.!#$%&'*+/=?^`{|}~-]+@[a-z\d-]+(?:\.[a-z\d-]+)*$/i;

// Check if the email is valid
const isValidEmail = () => {
  const validity = email.value.length !== 0 && emailRegExp.test(email.value);
  return validity;
};

// Update email input class based on validity
const setEmailClass = (isValid) => {
  email.className = isValid ? "valid" : "invalid";
};

// Update error message and visibility
const updateError = (isValidInput) => {
  if (isValidInput) {
    error.textContent = "";
    error.removeAttribute("class");
  } else {
    error.textContent = "I expect an email, darling!";
    error.setAttribute("class", "active");
  }
};

// Initialize email validity on page load
const initializeValidation = () => {
  const emailInput = isValidEmail();
  setEmailClass(emailInput);
};

// Handle input event to update email validity
const handleInput = () => {
  const emailInput = isValidEmail();
  setEmailClass(emailInput);
  updateError(emailInput);
};

// Handle form submission to show error if email is invalid
const handleSubmit = (event) => {
  event.preventDefault();

  const emailInput = isValidEmail();
  setEmailClass(emailInput);
  updateError(emailInput);
};

// Now we can rebuild our validation constraint
// Because we do not rely on CSS pseudo-class, we have to
// explicitly set the valid/invalid class on our email field
window.addEventListener("load", initializeValidation);
// This defines what happens when the user types in the field
email.addEventListener("input", handleInput);
// This defines what happens when the user tries to submit the data
form.addEventListener("submit", handleSubmit);
```

Il risultato appare così:

{{EmbedLiveSample("An_example_that_doesnt_use_the_constraint_validation_API", "100%", 150)}}

Come puoi vedere, non è così difficile costruire un sistema di validazione da solo. La parte difficile è renderlo abbastanza generico da utilizzare sia in modo cross-platform che su qualsiasi modulo potresti creare. Ci sono molte librerie disponibili per eseguire la convalida dei moduli, come [Validate.js](https://rickharrison.github.io/validate.js/).

## Metti alla prova le tue abilità!

Hai raggiunto la fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare alcuni ulteriori test per verificare di aver conservato queste informazioni prima di procedere — vedi [Metti alla prova le tue abilità: Convalida dei moduli](/it/docs/Learn_web_development/Extensions/Forms/Test_your_skills/Form_validation).

## Sommario

La convalida dei moduli lato client a volte richiede JavaScript se si desidera personalizzare lo stile e i messaggi di errore, ma richiede _sempre_ di pensare attentamente all'utente. Ricorda sempre di aiutare i tuoi utenti a correggere i dati che forniscono. A tal fine, assicurati di:

- Mostrare messaggi di errore espliciti.
- Essere permissivo riguardo al formato dell'input.
- Indicare esattamente dove si verifica l'errore, specialmente nei moduli di grandi dimensioni.

Una volta che hai verificato che il modulo è compilato correttamente, il modulo può essere inviato. Copriremo il [che invio dei dati del modulo](/it/docs/Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data) successivamente.

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/UI_pseudo-classes", "Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data", "Learn_web_development/Extensions/Forms")}}
