---
title: Validazione dei moduli lato client
slug: Learn_web_development/Extensions/Forms/Form_validation
l10n:
  sourceCommit: a1ac64fa4da965d2a152f08221b1a9aed638fd16
---

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/UI_pseudo-classes", "Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data", "Learn_web_development/Extensions/Forms")}}

È importante assicurarsi che tutti i controlli dei moduli richiesti siano compilati, nel formato corretto, prima di inviare i dati inseriti dall'utente al server. Questa **validazione dei moduli lato client** contribuisce a garantire che i dati inseriti corrispondano ai requisiti stabiliti nei vari controlli del modulo.

Questo articolo guida attraverso concetti di base ed esempi di validazione dei moduli lato client.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Abilità informatiche di base, una comprensione ragionevole di
        <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere cosa sia la validazione dei moduli lato client, perché è importante,
        e come applicare varie tecniche per implementarla.
      </td>
    </tr>
  </tbody>
</table>

La validazione lato client è un controllo iniziale e una caratteristica importante di una buona esperienza utente; rilevando i dati non validi lato client, l'utente può correggerli immediatamente.
Se arriva al server e viene poi rifiutato, si verifica un notevole ritardo dovuto al ciclo di andata e ritorno al server e poi al lato client per informare l'utente di correggere i propri dati.

Tuttavia, la validazione lato client _non dovrebbe essere considerata_ una misura di sicurezza esaustiva! Le sue applicazioni dovrebbero sempre eseguire la validazione, inclusi i controlli di sicurezza, su tutti i dati inviati tramite modulo anche lato _server_ **oltre** che lato client, poiché la validazione lato client è troppo facile da bypassare, quindi gli utenti malintenzionati possono ancora facilmente inviare dati errati al suo server.

> [!NOTE]
> Legga [Sicurezza del sito web](/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Website_security) per farsi un'idea di ciò che _potrebbe_ accadere; implementare la validazione lato server è un argomento che va oltre lo scopo di questo modulo, ma dovrebbe tenerlo a mente.

## Cos'è la validazione del modulo?

Visiti qualsiasi sito popolare con un modulo di registrazione e noterà che fornisce feedback quando non inserisce i dati nel formato che si aspettano.
Riceverà messaggi come:

- "Questo campo è obbligatorio" (Non può lasciare questo campo vuoto).
- "Per favore inserisca il suo numero di telefono nel formato xxx-xxxx" (È richiesto uno specifico formato di dati affinché sia considerato valido).
- "Per favore inserisca un indirizzo email valido" (i dati inseriti non sono nel formato corretto).
- "La sua password deve essere lunga tra 8 e 30 caratteri e contenere una lettera maiuscola, un simbolo e un numero." (È richiesto un formato di dati molto specifico per i suoi dati).

Questo è chiamato **validazione del modulo**.
Quando inserisce dati, il browser (e il server web) controllerà per vedere se i dati sono nel formato corretto e all'interno dei vincoli stabiliti dall'applicazione. La validazione eseguita nel browser è detta validazione **lato client**, mentre quella eseguita sul server è detta validazione **lato server**.
In questo capitolo ci concentriamo sulla validazione lato client.

Se le informazioni sono correttamente formattate, l'applicazione consente di inviare i dati al server e (di solito) di salvarli in un database; se le informazioni non sono correttamente formattate, fornisce all'utente un messaggio di errore che spiega cosa deve essere corretto e permette loro di riprovare.

Vogliamo semplificare il più possibile la compilazione dei moduli web. Allora perché insistiamo nel validare i nostri moduli?
Ci sono tre ragioni principali:

- **Vogliamo ottenere i dati giusti, nel formato giusto.** Le nostre applicazioni non funzioneranno correttamente se i dati degli utenti sono memorizzati nel formato sbagliato, sono errati o sono omessi del tutto.
- **Vogliamo proteggere i dati dei nostri utenti**. Costringere i nostri utenti ad inserire password sicure rende più facile proteggere le informazioni sui loro account.
- **Vogliamo proteggere noi stessi**. Ci sono molti modi in cui gli utenti malintenzionati possono abusare di moduli non protetti per danneggiare l'applicazione. Veda [Sicurezza del sito web](/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Website_security).

  > [!WARNING]
  > Non si fidi mai dei dati inviati al suo server dal client. Anche se il suo modulo convalidi correttamente e impedisca l'input non valido lato client, un utente malintenzionato può ancora alterare la richiesta di rete.

## Diversi tipi di validazione lato client

Ci sono due tipi diversi di validazione lato client che incontrerà sul web:

- **Validazione del modulo HTML**
  Gli attributi del modulo HTML possono definire quali controlli del modulo sono richiesti e in quale formato devono essere i dati inseriti dall'utente affinché siano validi.
- **Validazione del modulo JavaScript**
  Di solito si include JavaScript per migliorare o personalizzare la validazione del modulo HTML.

La validazione lato client può essere realizzata con un minimo, o senza JavaScript. La validazione HTML è più veloce di JavaScript, ma è meno personalizzabile della validazione JavaScript. Generalmente si raccomanda di iniziare i suoi moduli utilizzando robuste funzionalità HTML, poi migliorare l'esperienza utente con JavaScript come necessario.

## Utilizzo della validazione incorporata del modulo

Una delle caratteristiche più significative dei [controlli del modulo](/it/docs/Learn_web_development/Extensions/Forms/HTML5_input_types) è la capacità di validare la maggior parte dei dati inseriti dagli utenti senza fare affidamento su JavaScript.
Questo si fa utilizzando attributi di validazione sugli elementi del modulo.
Abbiamo visto molti di questi in precedenza nel corso, ma per riepilogare:

- [`required`](/it/docs/Web/HTML/Reference/Attributes/required): Specifica se un campo del modulo deve essere compilato prima che il modulo possa essere inviato.
- [`minlength`](/it/docs/Web/HTML/Reference/Attributes/minlength) e [`maxlength`](/it/docs/Web/HTML/Reference/Attributes/maxlength): Specificano la lunghezza minima e massima dei dati testuali (stringhe).
- [`min`](/it/docs/Web/HTML/Reference/Attributes/min), [`max`](/it/docs/Web/HTML/Reference/Attributes/max), e [`step`](/it/docs/Web/HTML/Reference/Attributes/step): Specificano i valori minimo e massimo dei tipi di input numerici, e l'incremento, o passo, per i valori, a partire dal minimo.
- [`type`](/it/docs/Web/HTML/Reference/Elements/input#input_types): Specifica se i dati devono essere un numero, un indirizzo email, o qualche altro tipo predefinito specifico.
- [`pattern`](/it/docs/Web/HTML/Reference/Attributes/pattern): Specifica un [espressione regolare](/it/docs/Web/JavaScript/Guide/Regular_expressions) che definisce un modello che i dati inseriti devono seguire.

Se i dati inseriti in un campo del modulo seguono tutte le regole specificate dagli attributi applicati al campo, sono considerati validi. In caso contrario, sono considerati non validi.

Quando un elemento è valido, le seguenti cose sono vere:

- L'elemento corrisponde alla pseudo-classe CSS {{cssxref(":valid")}}, che le consente di applicare uno stile specifico agli elementi validi. Il controllo corrisponderà anche a {{cssxref(":user-valid")}} se l'utente ha interagito con il controllo, e potrebbe corrispondere ad altre pseudo-classi UI, come {{cssxref(":in-range")}}, a seconda del tipo e degli attributi dell'input.
- Se l'utente tenta di inviare i dati, il browser invierà il modulo, a condizione che non ci sia nient'altro che lo impedisca (es. JavaScript).

Quando un elemento è non valido, le seguenti cose sono vere:

- L'elemento corrisponde alla pseudo-classe CSS {{cssxref(":invalid")}}. Se l'utente ha interagito con il controllo, corrisponde anche alla pseudo-classe CSS {{cssxref(":user-invalid")}}. Altre pseudo-classi UI possono anch'esse corrispondere, come {{cssxref(":out-of-range")}}, a seconda dell'errore. Queste le permettono di applicare uno stile specifico agli elementi non validi.
- Se l'utente tenta di inviare i dati, il browser bloccherà l'invio del modulo e visualizzerà un messaggio di errore. Il messaggio di errore differirà a seconda del tipo di errore. L'[API di validazione dei vincoli](#l'api_di_validazione_dei_vincoli) è descritta di seguito.

## Esempi di validazione incorporata del modulo

In questa sezione, testeremo alcuni degli attributi di cui abbiamo discusso sopra.

### File di inizio semplice

Iniziamo con un esempio semplice: un input che le permette di scegliere se preferisce una banana o una ciliegia.
Questo esempio coinvolge un semplice {{HTMLElement("input")}} di testo con un {{htmlelement("label")}} associato e un {{htmlelement("button")}} di invio.

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

Per cominciare, faccia una copia del [file `fruit-start.html` trovato su GitHub](https://github.com/mdn/learning-area/blob/main/html/forms/form-validation/fruit-start.html) in una nuova directory sul suo disco rigido.

### L'attributo required

Una caratteristica comune di validazione HTML è l'attributo [`required`](/it/docs/Web/HTML/Reference/Attributes/required).
Aggiunga questo attributo a un input per rendere un elemento obbligatorio.
Quando questo attributo è impostato, l'elemento corrisponde alla pseudo-classe UI {{cssxref(':required')}}, e il modulo non verrà inviato, visualizzando un messaggio di errore in caso di invio, se l'input è vuoto.
Finché è vuoto, l'input verrà anche considerato non valido, corrispondendo alla pseudo-classe UI {{cssxref(':invalid')}}.

Se un qualsiasi pulsante di scelta, in un gruppo con lo stesso nome, ha l'attributo `required`, uno dei pulsanti di scelta in quel gruppo deve essere selezionato affinché il gruppo sia valido; il pulsante selezionato non deve essere necessariamente quello con l'attributo impostato.

> [!NOTE]
> Richieda agli utenti di inserire solo i dati di cui ha bisogno: ad esempio, è davvero necessario sapere il sesso o il titolo di una persona?

Aggiunga un attributo `required` al suo input, come mostrato di seguito.

```html
<form>
  <label for="choose">Would you prefer a banana or cherry? (required)</label>
  <input id="choose" name="i-like" required />
  <button>Submit</button>
</form>
```

Abbiamo aggiunto "(obbligatorio)" al {{htmlelement("label")}} per informare l'utente che l'{{htmlelement("input")}} è richiesto. Indicare all'utente quando i campi del modulo sono obbligatori non è solo una buona esperienza utente, ma è richiesto dalle linee guida WCAG [accessibilità](/it/docs/Learn_web_development/Core/Accessibility).

Includiamo stili CSS che vengono applicati a seconda se l'elemento è richiesto, valido, e non valido:

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

Questo CSS fa sì che l'input abbia un bordo tratteggiato rosso quando è non valido e un bordo nero solido più sottile quando è valido.
Abbiamo anche aggiunto un gradiente di sfondo quando l'input è richiesto _e_ non valido. Provi il nuovo comportamento nell'esempio qui sotto:

{{EmbedLiveSample("The_required_attribute", "100%", 80)}}

Provi ad inviare il modulo dall'[esempio live `required`](https://mdn.github.io/learning-area/html/forms/form-validation/fruit-required.html) senza un valore. Noti come l'input non valido ottenga il focus, appaia un messaggio di errore predefinito ("Si prega di compilare questo campo") e il modulo sia impedito dall'essere inviato. Può anche vedere il [codice sorgente su GitHub](https://github.com/mdn/learning-area/blob/main/html/forms/form-validation/fruit-required.html).

### Validazione contro un'espressione regolare

Un'altra utile caratteristica di validazione è l'attributo [`pattern`](/it/docs/Web/HTML/Reference/Attributes/pattern), che si aspetta un [espressione regolare](/it/docs/Web/JavaScript/Guide/Regular_expressions) come valore.
Un'espressione regolare (regexp) è un modello che può essere utilizzato per confrontare combinazioni di caratteri in stringhe di testo, quindi le regexp sono ideali per la validazione dei moduli e servono a una varietà di altri usi in JavaScript.

Le regexp sono piuttosto complesse, e non intendiamo insegnarle in modo esaustivo in questo articolo.
Di seguito sono riportati alcuni esempi per darLe un'idea di base di come funzionano.

- `a` — Corrisponde a un carattere che è `a` (non `b`, non `aa`, e così via).
- `abc` — Corrisponde a `a`, seguito da `b`, seguito da `c`.
- `ab?c` — Corrisponde a `a`, eventualmente seguito da un unico `b`, seguito da `c`. (`ac` o `abc`)
- `ab*c` — Corrisponde a `a`, eventualmente seguito da qualsiasi numero di `b`, seguito da `c`. (`ac`, `abc`, `abbbbbc`, e così via).
- `a|b` — Corrisponde a un carattere che è `a` o `b`.
- `abc|xyz` — Corrisponde esattamente a `abc` o esattamente a `xyz` (ma non `abcxyz` o `a` o `y`, e così via).

Ci sono molte più possibilità che non copriamo qui.
Per un elenco completo e molti esempi, consulti la nostra documentazione sulle [Espressioni regolari](/it/docs/Web/JavaScript/Guide/Regular_expressions).

Realizziamo un esempio.
Aggiorni il suo HTML per aggiungere un attributo [`pattern`](/it/docs/Web/HTML/Reference/Attributes/pattern) come questo:

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

Ciò ci fornisce il seguente aggiornamento — provi:

{{EmbedLiveSample("Validating_against_a_regular_expression", "100%", 80)}}

Può trovare questo [esempio live su GitHub](https://mdn.github.io/learning-area/html/forms/form-validation/fruit-pattern.html) insieme al [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/forms/form-validation/fruit-pattern.html).

In questo esempio, l'elemento {{HTMLElement("input")}} accetta uno dei quattro valori possibili: le stringhe "banana", "Banana", "cherry", o "Cherry". Le espressioni regolari sono sensibili alle maiuscole e minuscole, ma abbiamo fatto in modo che supporti le versioni con lettera maiuscola e minuscola utilizzando un modello extra "Aa" annidato all'interno delle parentesi quadrate.

A questo punto, provi a cambiare il valore all'interno dell'attributo [`pattern`](/it/docs/Web/HTML/Reference/Attributes/pattern) per farlo corrispondere ad alcuni degli esempi visti in precedenza e osservi come influisce sui valori che può inserire per rendere valido il valore dell'input.
Provi a scrivere alcuni dei suoi e veda come va.
Faccia in modo che siano legati alla frutta, se possibile, in modo che i suoi esempi abbiano senso!

Se un valore non vuoto dell'elemento {{HTMLElement("input")}} non corrisponde al modello dell'espressione regolare, l'`input` corrisponderà alla pseudo-classe {{cssxref(':invalid')}}. Se vuoto, e l'elemento non è richiesto, non è considerato non valido.

Alcuni tipi di elementi {{HTMLElement("input")}} non hanno bisogno di un attributo [`pattern`](/it/docs/Web/HTML/Reference/Attributes/pattern) per essere validati contro un'espressione regolare. Ad esempio, specificare il tipo `email` valida il valore degli input rispetto a un modello di indirizzo email ben formato o un modello che corrisponde a un elenco separato da virgole di indirizzi email se ha l'attributo [`multiple`](/it/docs/Web/HTML/Reference/Attributes/multiple).

> [!NOTE]
> L'elemento {{HTMLElement("textarea")}} non supporta l'attributo [`pattern`](/it/docs/Web/HTML/Reference/Attributes/pattern).

### Limitare la lunghezza delle voci

Può limitare la lunghezza in caratteri di tutti i campi di testo creati da {{HTMLElement("input")}} o {{HTMLElement("textarea")}} utilizzando gli attributi [`minlength`](/it/docs/Web/HTML/Reference/Attributes/minlength) e [`maxlength`](/it/docs/Web/HTML/Reference/Attributes/maxlength).
Un campo è non valido se ha un valore e quel valore ha meno caratteri del valore [`minlength`](/it/docs/Web/HTML/Reference/Attributes/minlength) o più del valore [`maxlength`](/it/docs/Web/HTML/Reference/Attributes/maxlength).

I browser spesso non permettono all'utente di digitare un valore più lungo di quanto ci si aspetti nei campi di testo. Una migliore esperienza utente rispetto soltanto all'utilizzo di `maxlength` è fornire anche un feedback sul conteggio dei caratteri in modo accessibile e permettere all'utente di modificare il loro contenuto per ridurlo alle dimensioni.
Un esempio di questo è il limite di caratteri quando si pubblicano sui social media. JavaScript, compresi [soluzioni che utilizzano `maxlength`](https://github.com/mimo84/bootstrap-maxlength), può essere utilizzato per fornire questo.

> [!NOTE]
> I vincoli di lunghezza non vengono mai segnalati se il valore viene impostato programmaticamente. Vengono segnalati solo per input forniti dall'utente.

### Limitare i valori delle voci

Per i campi numerici, compresi [`<input type="number">`](/it/docs/Web/HTML/Reference/Elements/input/number) e i vari tipi di input per data, gli attributi [`min`](/it/docs/Web/HTML/Reference/Attributes/min) e [`max`](/it/docs/Web/HTML/Reference/Attributes/max) possono essere utilizzati per fornire un intervallo di valori validi.
Se il campo contiene un valore al di fuori di questo intervallo, sarà non valido.

Esaminiamo un altro esempio.
Crei una nuova copia del file [fruit-start.html](https://github.com/mdn/learning-area/blob/main/html/forms/form-validation/fruit-start.html).

Ora elimini il contenuto dell'elemento `<body>`, e lo sostituisca con quanto segue:

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

- Qui vedrà che abbiamo dato al campo di testo un `minlength` e un `maxlength` di sei, che è la stessa lunghezza di banana e cherry.
- Abbiamo anche dato al campo `number` un `min` di uno e un `max` di dieci.
  I numeri inseriti al di fuori di questo intervallo saranno mostrati come non validi; gli utenti non potranno utilizzare le frecce di incremento/decremento per spostare il valore al di fuori di questo intervallo.
  Se l'utente inserisce manualmente un numero al di fuori di questo intervallo, i dati sono non validi.
  Il numero non è richiesto, quindi rimuovere il valore risulterà in un valore valido.

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

Ecco l'esempio in esecuzione:

{{EmbedLiveSample("Constraining_the_values_of_your_entries", "100%", 100)}}

Provi questo [esempio live su GitHub](https://mdn.github.io/learning-area/html/forms/form-validation/fruit-length.html) e veda il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/forms/form-validation/fruit-length.html).

I tipi di input numerici, come `number`, `range` e `date`, possono anche accettare l'attributo [`step`](/it/docs/Web/HTML/Reference/Attributes/step). Questo attributo specifica quale incremento il valore aumenterà o diminuirà quando i controlli dell'input vengono utilizzati (come i pulsanti numerici su e giù, o far scorrere il pollice del range). L'attributo `step` è omesso nel nostro esempio, quindi il valore predefinito è `1`. Ciò significa che i numeri decimali, come 3.2, verranno anche mostrati come non validi.

### Esempio completo

Qui è presente un esempio completo per mostrare l'utilizzo delle caratteristiche di validazione integrate di HTML.
Prima di tutto, un po' di HTML:

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

Questo viene visualizzato come segue:

{{EmbedLiveSample("Full_example", "100%", 420)}}

Questo [esempio completo è live su GitHub](https://mdn.github.io/learning-area/html/forms/form-validation/full-example.html) insieme al [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/forms/form-validation/full-example.html).

Veda [Attributi relativi alla validazione](/it/docs/Web/HTML/Guides/Constraint_validation#validation-related_attributes) per un elenco completo degli attributi che possono essere utilizzati per limitare i valori di input e i tipi di input che li supportano.

## Validazione dei moduli con JavaScript

Se vuole cambiare il testo dei messaggi di errore nativi, è necessario JavaScript.
In questa sezione vedremo i diversi modi per farlo.

### L'API di validazione dei vincoli

L'API di validazione dei vincoli consiste in un set di metodi e proprietà disponibili sulle seguenti interfacce DOM degli elementi del modulo:

- [`HTMLButtonElement`](/it/docs/Web/API/HTMLButtonElement) (rappresenta un elemento [`<button>`](/it/docs/Web/HTML/Reference/Elements/button))
- [`HTMLFieldSetElement`](/it/docs/Web/API/HTMLFieldSetElement) (rappresenta un elemento [`<fieldset>`](/it/docs/Web/HTML/Reference/Elements/fieldset))
- [`HTMLInputElement`](/it/docs/Web/API/HTMLInputElement) (rappresenta un elemento [`<input>`](/it/docs/Web/HTML/Reference/Elements/input))
- [`HTMLOutputElement`](/it/docs/Web/API/HTMLOutputElement) (rappresenta un elemento [`<output>`](/it/docs/Web/HTML/Reference/Elements/output))
- [`HTMLSelectElement`](/it/docs/Web/API/HTMLSelectElement) (rappresenta un elemento [`<select>`](/it/docs/Web/HTML/Reference/Elements/select))
- [`HTMLTextAreaElement`](/it/docs/Web/API/HTMLTextAreaElement) (rappresenta un elemento [`<textarea>`](/it/docs/Web/HTML/Reference/Elements/textarea))

L'API di validazione dei vincoli rende disponibili le seguenti proprietà sui suddetti elementi.

- `validationMessage`: Restituisce un messaggio localizzato che descrive i vincoli di validazione che il controllo non soddisfa (se presenti). Se il controllo non è un candidato per la validazione dei vincoli (`willValidate` è `false`) o il valore dell'elemento soddisfa i suoi vincoli (è valido), questo restituirà una stringa vuota.
- `validity`: Restituisce un oggetto `ValidityState` che contiene diverse proprietà che descrivono lo stato di validità dell'elemento. Può trovare i dettagli completi di tutte le proprietà disponibili nella pagina di riferimento [`ValidityState`](/it/docs/Web/API/ValidityState); di seguito è elencato un elenco di alcuni dei più comuni:

  - [`patternMismatch`](/it/docs/Web/API/ValidityState/patternMismatch): Restituisce `true` se il valore non corrisponde al modello specificato in [`pattern`](/it/docs/Web/HTML/Reference/Elements/input#pattern), e `false` se corrisponde. Se vero, l'elemento corrisponde alla pseudo-classe CSS {{cssxref(":invalid")}}.
  - [`tooLong`](/it/docs/Web/API/ValidityState/tooLong): Restituisce `true` se il valore è più lungo della lunghezza massima specificata dall'attributo [`maxlength`](/it/docs/Web/HTML/Reference/Elements/input#maxlength), o `false` se è più corto o uguale alla lunghezza massima. Se vero, l'elemento corrisponde alla pseudo-classe CSS {{cssxref(":invalid")}}.
  - [`tooShort`](/it/docs/Web/API/ValidityState/tooShort): Restituisce `true` se il valore è più corto della lunghezza minima specificata dall'attributo [`minlength`](/it/docs/Web/HTML/Reference/Elements/input#minlength), o `false` se è maggiore o uguale alla lunghezza minima. Se vero, l'elemento corrisponde alla pseudo-classe CSS {{cssxref(":invalid")}}.
  - [`rangeOverflow`](/it/docs/Web/API/ValidityState/rangeOverflow): Restituisce `true` se il valore è maggiore del massimo specificato dall'attributo [`max`](/it/docs/Web/HTML/Reference/Elements/input#max), o `false` se è minore o uguale al massimo. Se vero, l'elemento corrisponde alle pseudo-class CSS {{cssxref(":invalid")}} e {{cssxref(":out-of-range")}}.
  - [`rangeUnderflow`](/it/docs/Web/API/ValidityState/rangeUnderflow): Restituisce `true` se il valore è minore del minimo specificato dall'attributo [`min`](/it/docs/Web/HTML/Reference/Elements/input#min), o `false` se è maggiore o uguale al minimo. Se vero, l'elemento corrisponde alle pseudo-class CSS {{cssxref(":invalid")}} e {{cssxref(":out-of-range")}}.
  - [`typeMismatch`](/it/docs/Web/API/ValidityState/typeMismatch): Restituisce `true` se il valore non è nella sintassi richiesta (quando [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) è `email` o `url`), o `false` se la sintassi è corretta. Se `true`, l'elemento corrisponde alla pseudo-classe CSS {{cssxref(":invalid")}}.
  - `valid`: Restituisce `true` se l'elemento soddisfa tutti i suoi vincoli di validazione, ed è quindi considerato valido, o `false` se non soddisfa alcun vincolo. Se vero, l'elemento corrisponde alla pseudo-classe CSS {{cssxref(":valid")}}; altrimenti alla pseudo-classe CSS {{cssxref(":invalid")}}.
  - `valueMissing`: Restituisce `true` se l'elemento ha un attributo [`required`](/it/docs/Web/HTML/Reference/Elements/input#required), ma nessun valore, o `false` altrimenti. Se vero, l'elemento corrisponde alla pseudo-classe CSS {{cssxref(":invalid")}}.

- `willValidate`: Restituisce `true` se l'elemento sarà validato quando il modulo sarà inviato; `false` altrimenti.

L'API di validazione dei vincoli rende inoltre disponibili i seguenti metodi sui suddetti elementi e sull'elemento [`form`](/it/docs/Web/HTML/Reference/Elements/form).

- `checkValidity()`: Restituisce `true` se il valore dell'elemento non presenta problemi di validità; `false` altrimenti. Se l'elemento è non valido, questo metodo genera anche un [`evento non valido`](/it/docs/Web/API/HTMLInputElement/invalid_event) sull'elemento.
- `reportValidity()`: Segnala il campo (o i campi) non valido(i) usando eventi. Questo metodo è utile in combinazione con `preventDefault()` in un gestore di eventi `onSubmit`.
- `setCustomValidity(message)`: Aggiunge un messaggio di errore personalizzato all'elemento; se imposta un messaggio di errore personalizzato, l'elemento è considerato non valido e viene visualizzato l'errore specificato. Questo Le consente di utilizzare il codice JavaScript per stabilire un fallimento di validazione diverso da quelli offerti dai vincoli di validazione HTML standard. Il messaggio è mostrato all'utente quando viene segnalato il problema.

#### Implementazione di un messaggio di errore personalizzato

Come ha visto negli esempi di vincoli di validazione HTML precedenti, ogni volta che un utente tenta di inviare un modulo non valido, il browser visualizza un messaggio di errore. Il modo in cui questo messaggio viene visualizzato dipende dal browser.

Questi messaggi automatizzati hanno due svantaggi:

- Non c'è un modo standard per cambiare il loro aspetto con CSS.
- Dipendono dalla localizzazione del browser, il che significa che può avere una pagina in una lingua ma un messaggio di errore visualizzato in un'altra lingua, come si vede nel seguente screenshot di Firefox.

![Esempio di un messaggio di errore con Firefox in francese su una pagina in inglese](error-firefox-win7.png)

Personalizzare questi messaggi di errore è uno degli usi più comuni dell'API di validazione dei vincoli.
Lavoriamo attraverso un esempio di come farlo.

Inizieremo con un po' di HTML (si senta libero di metterlo in un file HTML vuoto; usi una copia fresca di [fruit-start.html](https://github.com/mdn/learning-area/blob/main/html/forms/form-validation/fruit-start.html) come base, se vuole):

```html
<form>
  <label for="mail">
    I would like you to provide me with an email address:
  </label>
  <input type="email" id="mail" name="mail" />
  <button>Submit</button>
</form>
```

Aggiunga il seguente JavaScript alla pagina:

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

Qui memorizziamo un riferimento all'input email, quindi aggiungiamo un gestore di eventi che esegue il codice contenuto ogni volta che il valore all'interno dell'input viene modificato.

All'interno del codice contenuto, controlliamo se la proprietà `validity.typeMismatch` dell'input email restituisce `true`, il che significa che il valore contenuto non corrisponde al modello per un indirizzo email ben formato. Se è così, chiamiamo il metodo [`setCustomValidity()`](/it/docs/Web/API/HTMLInputElement/setCustomValidity) con un messaggio personalizzato. Questo rende l'input non valido, in modo che quando si tenta di inviare il modulo, l'invio fallisce e il messaggio di errore personalizzato viene visualizzato.

Se la proprietà `validity.typeMismatch` restituisce `false`, chiamiamo il metodo `setCustomValidity()` con una stringa vuota. Questo rende l'input valido, quindi il modulo sarà inviato. Durante la validazione, se un controllo del modulo ha un `customError` che non è una stringa vuota, l'invio del modulo viene bloccato.

Può provarlo qui sotto:

{{EmbedGHLiveSample("learning-area/html/forms/form-validation/custom-error-message.html", '100%', 120)}}

Può trovare questo esempio live su GitHub come [custom-error-message.html](https://mdn.github.io/learning-area/html/forms/form-validation/custom-error-message.html), insieme al [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/forms/form-validation/custom-error-message.html).

#### Estendere la validazione incorporata del modulo

L'esempio precedente ha mostrato come può aggiungere un messaggio personalizzato per un tipo particolare di errore (`validity.typeMismatch`).
È anche possibile utilizzare tutta la validazione incorporata del modulo e poi aggiungervi utilizzando `setCustomValidity()`.

Qui dimostriamo come può estendere la validazione integrata di [`<input type="email">`](/it/docs/Web/HTML/Reference/Elements/input/email) per accettare solo indirizzi con il dominio `@example.com`.
Iniziamo con il modulo HTML {{htmlelement("form")}} qui sotto.

```html
<form>
  <label for="mail">Email address (@example.com only):</label>
  <input type="email" id="mail" />
  <button>Submit</button>
</form>
```

Il codice di validazione è mostrato di seguito.
In caso di nuovo input, il codice ripristina prima di tutto il messaggio di validità personalizzato chiamando `setCustomValidity("")`.
Poi, utilizza `email.validity.valid` per verificare se l'indirizzo inserito è non valido e, in caso affermativo, esce dal gestore dell'evento.
Questo garantisce che tutti i normali controlli di validazione incorporati vengano eseguiti mentre il testo inserito non è un indirizzo email valido.

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

Può provare questo esempio nella pagina al {{LiveSampleLink('Extending_built-in_form_validation', 'Link demo esempio live')}}.
Provi a inviare un indirizzo email non valido, un indirizzo email valido che non termina con `@example.com`, e uno che termina con `@example.com`.

#### Un esempio più dettagliato

Ora che abbiamo visto un esempio veramente basilare, vediamo come possiamo utilizzare quest'API per costruire una validazione personalizzata leggermente più complessa.

Prima, l'HTML. Ancora una volta, si senta libero di costruirlo insieme a noi:

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

Questo modulo utilizza l'attributo [`novalidate`](/it/docs/Web/HTML/Reference/Elements/form#novalidate) per disattivare la validazione automatica del browser. Impostare l'attributo `novalidate` sul modulo impedisce al modulo di mostrare le proprie bolle di messaggio di errore, e ci permette di visualizzare invece i messaggi di errore personalizzati nel DOM in qualche modo a nostra scelta.
Tuttavia, questo non disabilita il supporto per l'API di validazione dei vincoli né l'applicazione delle pseudo-class CSS come {{cssxref(":valid")}}, ecc.
Ciò significa che anche se il browser non controlla automaticamente la validità del modulo prima di inviarne i dati, può comunque farlo da solo e stilizzare il modulo di conseguenza.

Il nostro input da validare è un [`<input type="email">`](/it/docs/Web/HTML/Reference/Elements/input/email), che è `required`, e ha un `minlength` di 8 caratteri. Controlliamo questi utilizzando il nostro codice, e mostriamo un messaggio di errore personalizzato per ciascuno.

Miriamo a mostrare i messaggi di errore all'interno di un elemento `<span>`.
L'attributo [`aria-live`](/it/docs/Web/Accessibility/ARIA/Guides/Live_regions) è impostato su quell'elemento `<span>` per garantire che il nostro messaggio di errore personalizzato sarà presentato a tutti, incluso che venga letto per gli utenti di screen reader.

Passiamo ora a un po' di CSS di base per migliorare l'aspetto del modulo leggermente e fornire un feedback visivo quando i dati di input sono non validi:

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

Ora diamo un'occhiata al JavaScript che implementa la validazione personalizzata degli errori.
Ci sono molti modi per scegliere un nodo DOM; qui otteniamo il modulo stesso e la casella di input email, così come l'elemento span in cui inseriremo il messaggio di errore.

Utilizzando gestori di eventi, controlliamo se i campi del modulo sono validi ogni volta che l'utente digita qualcosa. Se c'è un errore, lo mostriamo. Se non c'è errore, rimuoviamo qualsiasi messaggio di errore.

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

Ogni volta che cambiamo il valore dell'input, controlliamo per vedere se contiene dati validi. Se ha successo, rimuoviamo qualsiasi messaggio di errore mostrato. Se i dati non sono validi, eseguiamo `showError()` per mostrare l'errore appropriato.

Ogni volta che proviamo a inviare il modulo, controlliamo nuovamente se i dati sono validi. Se è così, permettiamo l'invio del modulo. In caso contrario, eseguiamo `showError()` per mostrare l'errore appropriato e fermiamo l'invio del modulo con [`preventDefault()`](/it/docs/Web/API/Event/preventDefault).

La funzione `showError()` utilizza varie proprietà dell'oggetto `validity` dell'input per determinare qual è l'errore, quindi visualizza un messaggio di errore appropriato.

Ecco il risultato live:

{{EmbedGHLiveSample("learning-area/html/forms/form-validation/detailed-custom-validation.html", '100%', 150)}}

Può trovare questo esempio live su GitHub come [detailed-custom-validation.html](https://mdn.github.io/learning-area/html/forms/form-validation/detailed-custom-validation.html) insieme al [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/forms/form-validation/detailed-custom-validation.html).

L'API di validazione dei vincoli Le offre uno strumento potente per gestire la validazione dei moduli, consentendoLe di avere un controllo enorme sull'interfaccia utente, oltre ciò che può fare solo con HTML e CSS.

### Validazione dei moduli senza un'API integrata

In alcuni casi, come i [controlli personalizzati](/it/docs/Learn_web_development/Extensions/Forms/How_to_build_custom_form_controls), non sarà in grado di o non vorrà usare l'API di validazione dei vincoli. È ancora in grado di usare JavaScript per validare il suo modulo, ma dovrà semplicemente scriverlo da solo.

Per validare un modulo, si ponga alcune domande:

- Che tipo di validazione dovrei eseguire?
  - Deve determinare come validare i suoi dati: operazioni su stringhe, conversioni di tipo, espressioni regolari, e così via. Sta a Lei.
- Cosa dovrei fare se il modulo non convalida?
  - Questo è chiaramente una questione di UI. Deve decidere come si comporterà il modulo. Il modulo invia comunque i dati?
    Dovrebbe evidenziare i campi che sono in errore?
    Dovrebbe visualizzare messaggi di errore?
- Come posso aiutare l'utente a correggere i dati non validi?

  - : Al fine di ridurre la frustrazione dell'utente, è molto importante fornire quante più informazioni utili possibili per guidarli nella correzione dei loro input.
    Dovrebbe offrire suggerimenti iniziali in modo che sappiano cosa ci si aspetta, così come messaggi di errore chiari.
    Se vuole approfondire i requisiti dell'interfaccia utente di validazione del modulo, qui ci sono alcuni articoli utili che dovrebbe leggere:

    - [Aiutare gli utenti a inserire i dati corretti nei moduli](https://web.dev/learn/forms/form-fields)
    - [Validazione degli input](https://www.w3.org/WAI/tutorials/forms/validation/)
    - [Come segnalare errori nei moduli: 10 linee guida per il design](https://www.nngroup.com/articles/errors-forms-design-guidelines/)

#### Un esempio che non utilizza l'API di validazione dei vincoli

Per illustrare questo, il seguente è una versione semplificata dell'esempio precedente senza l'API di validazione dei vincoli.

L'HTML è quasi lo stesso; abbiamo solo rimosso le caratteristiche di validazione HTML.

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

Similmente, il CSS non ha bisogno di cambiare molto; abbiamo semplicemente trasformato la pseudo-classe CSS {{cssxref(":invalid")}} in una vera classe ed evitato di usare il selettore di attributo.

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

I grandi cambiamenti sono nel codice JavaScript, che deve fare molto di più.

```js
const form = document.querySelector("form");
const email = document.getElementById("mail");
const error = document.getElementById("error");

// Regular expression for email validation as per HTML specification
const emailRegExp =
  /^[a-zA-Z0-9.!#$%&'*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/;

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

Il risultato appare in questo modo:

{{EmbedLiveSample("An_example_that_doesnt_use_the_constraint_validation_API", "100%", 150)}}

Come può vedere, non è così difficile costruire un sistema di validazione da solo. La parte difficile è renderlo abbastanza generico da poterlo usare su qualsiasi piattaforma e su qualsiasi modulo potrebbe creare. Ci sono molte librerie disponibili per eseguire la validazione del modulo, come [Validate.js](https://rickharrison.github.io/validate.js/).

## Metta alla prova le sue capacità!

Ha raggiunto la fine di questo articolo, ma può ricordare le informazioni più importanti? Può trovare ulteriori test per verificare di aver assimilato queste informazioni prima di procedere — veda [Metti alla prova le tue capacità: Validazione del modulo](/it/docs/Learn_web_development/Extensions/Forms/Test_your_skills/Form_validation).

## Riepilogo

La validazione dei moduli lato client a volte richiede JavaScript se si vogliono personalizzare lo stile e i messaggi di errore, ma richiede _sempre_ di pensare attentamente all'utente.
Ricordi sempre di aiutare i suoi utenti a correggere i dati che forniscono. A tal fine, sia sicuro di:

- Visualizzare messaggi di errore espliciti.
- Essere permissivo riguardo il formato di input.
- Indicare esattamente dove si verifica l'errore, specialmente su moduli grandi.

Una volta verificato che il modulo sia compilato correttamente, il modulo può essere inviato.
Copriamo [l'invio dei dati del modulo](/it/docs/Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data) nel prossimo articolo.

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/UI_pseudo-classes", "Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data", "Learn_web_development/Extensions/Forms")}}
