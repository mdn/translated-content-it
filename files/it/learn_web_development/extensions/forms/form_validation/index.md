---
title: Validazione dei form lato client
slug: Learn_web_development/Extensions/Forms/Form_validation
l10n:
  sourceCommit: ce12c10364f35c64184dec44be85537b7e10d91f
---

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/UI_pseudo-classes", "Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data", "Learn_web_development/Extensions/Forms")}}

È importante assicurarsi che tutti i controlli obbligatori del form siano compilati, nel formato corretto, prima di inviare al server i dati del form inseriti dall'utente. Questa **validazione dei form lato client** aiuta a garantire che i dati inseriti soddisfino i requisiti stabiliti nei vari controlli del form.

Questo articolo guida attraverso i concetti di base e alcuni esempi di validazione dei form lato client.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Competenze informatiche di base, una ragionevole comprensione di
        <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere cos'è la validazione dei form lato client, perché è importante
        e come applicare varie tecniche per implementarla.
      </td>
    </tr>
  </tbody>
</table>

La validazione lato client è un controllo iniziale e una caratteristica importante di una buona esperienza utente; intercettando dati non validi lato client, l'utente può correggerli immediatamente.
Se i dati raggiungono il server e vengono poi rifiutati, si verifica un ritardo evidente dovuto al viaggio di andata e ritorno verso il server, necessario per comunicare all'utente che deve correggere i dati.

Tuttavia, la validazione lato client _non deve essere considerata_ una misura di sicurezza esaustiva. Le applicazioni devono sempre eseguire la validazione, inclusi i controlli di sicurezza, su tutti i dati inviati tramite form lato _server_, **oltre che** lato client, poiché la validazione lato client è troppo facile da aggirare e gli utenti malevoli possono comunque inviare facilmente dati non validi al server.

> [!NOTE]
> Leggere [Sicurezza dei siti web](/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Website_security) per farsi un'idea di ciò che _potrebbe_ accadere; l'implementazione della validazione lato server va in parte oltre l'ambito di questo modulo, ma è importante tenerla presente.

## Cos'è la validazione dei form?

Visitando un qualsiasi sito popolare con un form di registrazione, si noterà che fornisce un riscontro quando i dati non vengono inseriti nel formato previsto.
Vengono visualizzati messaggi come:

- "Questo campo è obbligatorio" (non è possibile lasciare vuoto questo campo).
- "Inserire il numero di telefono nel formato xxx-xxxx" (per essere considerato valido, è richiesto un formato di dati specifico).
- "Inserire un indirizzo email valido" (i dati inseriti non sono nel formato corretto).
- "La password deve contenere da 8 a 30 caratteri e includere una lettera maiuscola, un simbolo e un numero." (per i dati è richiesto un formato molto specifico).

Questa è chiamata **validazione dei form**.
Quando vengono inseriti dati, il browser (e il server web) verifica che siano nel formato corretto e rispettino i vincoli stabiliti dall'applicazione. La validazione eseguita nel browser è chiamata validazione **lato client**, mentre la validazione eseguita sul server è chiamata validazione **lato server**.
In questo capitolo ci concentriamo sulla validazione lato client.

Se le informazioni sono formattate correttamente, l'applicazione consente di inviare i dati al server e, di solito, di salvarli in un database; se le informazioni non sono formattate correttamente, fornisce all'utente un messaggio di errore che spiega cosa deve essere corretto e gli consente di riprovare.

L'obiettivo è rendere la compilazione dei form web il più semplice possibile. Perché, quindi, si insiste sulla validazione dei form?
Ci sono tre ragioni principali:

- **Si vogliono ottenere i dati corretti, nel formato corretto.** Le applicazioni non funzioneranno correttamente se i dati degli utenti vengono memorizzati nel formato errato, sono inesatti o vengono omessi del tutto.
- **Si vogliono proteggere i dati degli utenti.** Richiedere agli utenti di inserire password sicure facilita la protezione delle informazioni dei loro account.
- **Si vuole proteggere l'applicazione stessa.** Esistono molti modi in cui utenti malevoli possono utilizzare in modo improprio form non protetti per danneggiare l'applicazione. Vedere [Sicurezza dei siti web](/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Website_security).

  > [!WARNING]
  > Non considerare mai affidabili i dati trasmessi al server dal client. Anche se il form viene validato correttamente e impedisce input malformati lato client, un utente malevolo può comunque modificare la richiesta di rete.

## Diversi tipi di validazione lato client

Sul web si incontrano due diversi tipi di validazione lato client:

- **Validazione dei form HTML**
  Gli attributi dei form HTML possono definire quali controlli del form sono obbligatori e quale formato devono avere i dati inseriti dall'utente per essere validi.
- **Validazione dei form JavaScript**
  JavaScript viene generalmente incluso per migliorare o personalizzare la validazione dei form HTML.

La validazione lato client può essere ottenuta con poco o nessun JavaScript. La validazione HTML è più veloce di JavaScript, ma è meno personalizzabile rispetto alla validazione JavaScript. In genere si consiglia di iniziare i form utilizzando solide funzionalità HTML, quindi migliorare l'esperienza utente con JavaScript quando necessario.

## Utilizzare la validazione dei form integrata

Una delle caratteristiche più significative dei [controlli del form](/it/docs/Learn_web_development/Extensions/Forms/HTML5_input_types) è la capacità di validare la maggior parte dei dati utente senza fare affidamento su JavaScript.
Ciò avviene utilizzando gli attributi di validazione sugli elementi del form.
Molti di questi sono già stati visti in precedenza nel corso, ma ecco un riepilogo:

- [`required`](/it/docs/Web/HTML/Reference/Attributes/required): specifica se un campo del form deve essere compilato prima che il form possa essere inviato.
- [`minlength`](/it/docs/Web/HTML/Reference/Attributes/minlength) e [`maxlength`](/it/docs/Web/HTML/Reference/Attributes/maxlength): specificano la lunghezza minima e massima dei dati testuali (stringhe).
- [`min`](/it/docs/Web/HTML/Reference/Attributes/min), [`max`](/it/docs/Web/HTML/Reference/Attributes/max) e [`step`](/it/docs/Web/HTML/Reference/Attributes/step): specificano i valori minimo e massimo dei tipi di input numerici e l'incremento, o passo, per i valori, a partire dal minimo.
- [`type`](/it/docs/Web/HTML/Reference/Elements/input#input_types): specifica se i dati devono essere un numero, un indirizzo email o un altro tipo predefinito specifico.
- [`pattern`](/it/docs/Web/HTML/Reference/Attributes/pattern): specifica un'[espressione regolare](/it/docs/Web/JavaScript/Guide/Regular_expressions) che definisce un modello che i dati inseriti devono seguire.

Se i dati inseriti in un campo del form seguono tutte le regole specificate dagli attributi applicati al campo, sono considerati validi. In caso contrario, sono considerati non validi.

Quando un elemento è valido, sono vere le seguenti affermazioni:

- L'elemento corrisponde alla pseudo-classe CSS {{cssxref(":valid")}}, che consente di applicare uno stile specifico agli elementi validi. Il controllo corrisponderà anche a {{cssxref(":user-valid")}} se l'utente ha interagito con il controllo e potrebbe corrispondere ad altre pseudo-classi UI, come {{cssxref(":in-range")}}, a seconda del tipo di input e degli attributi.
- Se l'utente tenta di inviare i dati, il browser invierà il form, a condizione che non vi sia altro a impedirlo (ad esempio, JavaScript).

Quando un elemento non è valido, sono vere le seguenti affermazioni:

- L'elemento corrisponde alla pseudo-classe CSS {{cssxref(":invalid")}}. Se l'utente ha interagito con il controllo, corrisponde anche alla pseudo-classe CSS {{cssxref(":user-invalid")}}. Possono inoltre corrispondere altre pseudo-classi UI, come {{cssxref(":out-of-range")}}, a seconda dell'errore. Queste consentono di applicare uno stile specifico agli elementi non validi.
- Se l'utente tenta di inviare i dati, il browser bloccherà l'invio del form e visualizzerà un messaggio di errore. Il messaggio di errore sarà diverso a seconda del tipo di errore. La [Constraint Validation API](#la_constraint_validation_api) è descritta più avanti.

## Esempi di validazione dei form integrata

In questa sezione verranno provati alcuni degli attributi trattati in precedenza.

### File iniziale di base

Iniziamo con un esempio di base: un input che consente di scegliere se si preferisce una banana o una ciliegia.
Questo esempio comprende un {{HTMLElement("input")}} di testo con un {{htmlelement("label")}} associato e un {{htmlelement("button")}} di invio.

```html live-sample___simple-start-file
<!doctype html>
<html lang="en-US">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width" />
    <title>Favorite fruit start</title>
    <style>
      input:invalid {
        border: 2px dashed red;
      }

      input:valid {
        border: 2px solid black;
      }
    </style>
  </head>

  <body>
    <form>
      <label for="choose">Would you prefer a banana or a cherry?</label>
      <input id="choose" name="i_like" />
      <button>Submit</button>
    </form>
  </body>
</html>
```

{{EmbedLiveSample("simple-start-file", "100%", 80)}}

Per iniziare, creare una copia del precedente elenco HTML in un nuovo file `index.html`. Salvarlo in una nuova directory sul disco rigido.

### L'attributo required

Una comune funzionalità di validazione HTML è l'attributo [`required`](/it/docs/Web/HTML/Reference/Attributes/required).
Aggiungere questo attributo a un input per rendere obbligatorio un elemento.
Quando questo attributo è impostato, l'elemento corrisponde alla pseudo-classe UI {{cssxref(':required')}} e il form non verrà inviato, mostrando un messaggio di errore al momento dell'invio, se l'input è vuoto.
Quando è vuoto, l'input sarà inoltre considerato non valido e corrisponderà alla pseudo-classe UI {{cssxref(':invalid')}}.

Se un qualsiasi radio button in un gruppo con lo stesso nome ha l'attributo `required`, è necessario selezionare uno dei radio button di quel gruppo affinché il gruppo sia valido; il radio button selezionato non deve necessariamente essere quello su cui è impostato l'attributo.

> [!NOTE]
> Richiedere agli utenti solo i dati necessari: per esempio, è davvero necessario conoscere il genere o il titolo di una persona?

Aggiungere un attributo `required` all'input, come mostrato di seguito.

```html live-sample___the-required-attribute
<form>
  <label for="choose">Would you prefer a banana or cherry? *</label>
  <input id="choose" name="i-like" required />
  <button>Submit</button>
</form>
```

> [!NOTE]
> Una pratica comune consiste nel mettere un asterisco, o un altro contrassegno, dopo le etichette dei controlli obbligatori del form, in modo che risaltino per gli utenti vedenti. Indicare all'utente quando i campi del form sono obbligatori non è solo una buona esperienza utente, ma è richiesto dalle linee guida WCAG sull'[accessibilità](/it/docs/Learn_web_development/Core/Accessibility).

Includiamo stili CSS applicati in base al fatto che l'elemento sia obbligatorio, valido e non valido:

```css live-sample___the-required-attribute
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

```js hidden live-sample___the-required-attribute live-sample___validate-regular-expression live-sample___constraining-values
const form = document.querySelector("form");
form.addEventListener("submit", (e) => {
  e.preventDefault();
});
```

Questo CSS fa sì che l'input abbia un bordo rosso tratteggiato quando non è valido e un bordo nero continuo più discreto quando è valido.
È stato inoltre aggiunto un gradiente di sfondo quando l'input è obbligatorio _e_ non valido. Provare il nuovo comportamento nell'esempio seguente:

{{EmbedLiveSample("the-required-attribute", "100%", 80, , , , , "allow-forms")}}

Provare a inviare il form senza un valore. Notare come l'input non valido riceva il focus e venga visualizzato un messaggio di errore predefinito ("Compilare questo campo"). Viene inoltre impedito l'invio del form, anche se, quando viene inserito un valore, l'invio del form viene comunque impedito per evitare un errore dovuto al modo in cui MDN gestisce i form incorporati.

### Validazione rispetto a un'espressione regolare

Un'altra utile funzionalità di validazione è l'attributo [`pattern`](/it/docs/Web/HTML/Reference/Attributes/pattern), che si aspetta un'[espressione regolare](/it/docs/Web/JavaScript/Guide/Regular_expressions) come valore.
Un'espressione regolare (regexp) è un modello che può essere utilizzato per trovare combinazioni di caratteri nelle stringhe di testo; pertanto, le regexp sono ideali per la validazione dei form e hanno molti altri usi in JavaScript.

Le regexp sono piuttosto complesse e questo articolo non intende insegnarle in modo esaustivo.
Di seguito sono riportati alcuni esempi per fornire un'idea di base del loro funzionamento.

- `a` — Corrisponde a un carattere che è `a` (non `b`, non `aa` e così via).
- `abc` — Corrisponde a `a`, seguita da `b`, seguita da `c`.
- `ab?c` — Corrisponde a `a`, seguita facoltativamente da una singola `b`, seguita da `c`. (`ac` o `abc`)
- `ab*c` — Corrisponde a `a`, seguita facoltativamente da un qualsiasi numero di `b`, seguita da `c`. (`ac`, `abc`, `abbbbbc` e così via).
- `a|b` — Corrisponde a un carattere che è `a` o `b`.
- `abc|xyz` — Corrisponde esattamente a `abc` o esattamente a `xyz` (ma non a `abcxyz`, né a `a` né a `y` e così via).

Esistono molte altre possibilità che non vengono trattate qui.
Per un elenco completo e molti esempi, consultare la documentazione sulle [espressioni regolari](/it/docs/Web/JavaScript/Guide/Regular_expressions).

Implementiamo un esempio.
Aggiornare l'HTML aggiungendo un attributo [`pattern`](/it/docs/Web/HTML/Reference/Attributes/pattern) in questo modo:

```html live-sample___validate-regular-expression
<form>
  <label for="choose">Would you prefer a banana or a cherry? *</label>
  <input id="choose" name="i-like" required pattern="[Bb]anana|[Cc]herry" />
  <button>Submit</button>
</form>
```

```css hidden live-sample___validate-regular-expression
input:invalid {
  border: 2px dashed red;
}

input:valid {
  border: 2px solid black;
}
```

Questo produce il seguente aggiornamento: provarlo:

{{EmbedLiveSample("validate-regular-expression", "100%", 80, , , , , "allow-forms")}}

È inoltre possibile premere il pulsante **Play** per aprire l'esempio in MDN Playground e modificarne il codice sorgente.

In questo esempio, l'elemento {{HTMLElement("input")}} accetta uno di quattro possibili valori: le stringhe "banana", "Banana", "cherry" o "Cherry". Le espressioni regolari distinguono tra maiuscole e minuscole, ma sono state rese compatibili sia con le versioni maiuscole iniziali sia con quelle minuscole utilizzando un ulteriore modello "Aa" annidato tra parentesi quadre.

A questo punto, provare a modificare il valore all'interno dell'attributo [`pattern`](/it/docs/Web/HTML/Reference/Attributes/pattern) affinché corrisponda ad alcuni degli esempi visti in precedenza, e osservare come ciò influenzi i valori che è possibile inserire per rendere valido il valore dell'input.
Provare a scriverne alcuni personali e vedere come funziona.
Renderli relativi alla frutta, ove possibile, in modo che gli esempi abbiano senso.

Se un valore non vuoto di {{HTMLElement("input")}} non corrisponde al modello dell'espressione regolare, l'`input` corrisponderà alla pseudo-classe {{cssxref(':invalid')}}. Se è vuoto e l'elemento non è obbligatorio, non è considerato non valido.

Alcuni tipi di elemento {{HTMLElement("input")}} non necessitano di un attributo [`pattern`](/it/docs/Web/HTML/Reference/Attributes/pattern) per essere validati rispetto a un'espressione regolare. Ad esempio, specificare il tipo `email` valida il valore dell'input rispetto a un modello di indirizzo email ben formato, oppure a un modello corrispondente a un elenco di indirizzi email separati da virgole se possiede l'attributo [`multiple`](/it/docs/Web/HTML/Reference/Attributes/multiple).

> [!NOTE]
> L'elemento {{HTMLElement("textarea")}} non supporta l'attributo [`pattern`](/it/docs/Web/HTML/Reference/Attributes/pattern).

### Vincolare la lunghezza degli inserimenti

È possibile vincolare la lunghezza in caratteri di tutti i campi di testo creati da {{HTMLElement("input")}} o {{HTMLElement("textarea")}} utilizzando gli attributi [`minlength`](/it/docs/Web/HTML/Reference/Attributes/minlength) e [`maxlength`](/it/docs/Web/HTML/Reference/Attributes/maxlength).
Un campo non è valido se ha un valore e tale valore contiene meno caratteri del valore di [`minlength`](/it/docs/Web/HTML/Reference/Attributes/minlength) o più caratteri del valore di [`maxlength`](/it/docs/Web/HTML/Reference/Attributes/maxlength).

I browser spesso non consentono all'utente di digitare nei campi di testo un valore più lungo di quello previsto. Un'esperienza utente migliore rispetto al solo utilizzo di `maxlength` consiste anche nel fornire in modo accessibile un riscontro sul conteggio dei caratteri e consentire all'utente di ridurre il proprio contenuto alla dimensione richiesta.
Un esempio è il limite di caratteri durante la pubblicazione sui social media. Per fornire questa funzionalità è possibile usare JavaScript, incluse [soluzioni che utilizzano `maxlength`](https://github.com/mimo84/bootstrap-maxlength).

> [!NOTE]
> I vincoli di lunghezza non vengono mai segnalati se il valore è impostato programmaticamente. Vengono segnalati solo per l'input fornito dall'utente.

### Vincolare i valori degli inserimenti

Per i campi numerici, inclusi [`<input type="number">`](/it/docs/Web/HTML/Reference/Elements/input/number) e i vari tipi di input per le date, gli attributi [`min`](/it/docs/Web/HTML/Reference/Attributes/min) e [`max`](/it/docs/Web/HTML/Reference/Attributes/max) possono essere utilizzati per fornire un intervallo di valori validi.
Se il campo contiene un valore al di fuori di questo intervallo, non sarà valido.

Vediamo un altro esempio.
Creare una nuova copia del [file iniziale di base](#file_iniziale_di_base) e salvarla nella stessa directory come `index2.html`.

Ora eliminare il contenuto dell'elemento `<body>` e sostituirlo con quanto segue:

```html live-sample___constraining-values
<form>
  <div>
    <label for="choose">Would you prefer a banana or a cherry? *</label>
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

- Qui si può vedere che al campo `text` sono stati assegnati un `minlength` e un `maxlength` di sei, che corrisponde alla stessa lunghezza di banana e cherry.
- Al campo `number` sono stati inoltre assegnati un `min` di uno e un `max` di dieci.
  I numeri inseriti al di fuori di questo intervallo verranno mostrati come non validi; gli utenti non potranno usare le frecce di incremento/decremento per spostare il valore al di fuori di questo intervallo.
  Se l'utente inserisce manualmente un numero al di fuori di questo intervallo, i dati non sono validi.
  Il numero non è obbligatorio, quindi rimuovere il valore produrrà un valore valido.

```css hidden live-sample___constraining-values
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

{{EmbedLiveSample("constraining-values", "100%", 100)}}

È inoltre possibile premere il pulsante **Play** per aprire l'esempio in MDN Playground e modificarne il codice sorgente.

I tipi di input numerici, come `number`, `range` e `date`, possono anche accettare l'attributo [`step`](/it/docs/Web/HTML/Reference/Attributes/step). Questo attributo specifica di quale incremento il valore aumenterà o diminuirà quando vengono utilizzati i controlli di input, come i pulsanti numerici su e giù o lo scorrimento del cursore dell'intervallo. Nell'esempio l'attributo `step` è omesso, quindi il valore predefinito è `1`. Ciò significa che anche i numeri decimali, come 3.2, verranno mostrati come non validi.

### Esempio completo

Ecco un esempio completo che mostra l'utilizzo delle funzionalità di validazione integrate in HTML.
Prima, un po' di HTML:

```html
<form>
  <p>Please complete all required (*) fields.</p>
  <fieldset>
    <legend>Do you have a driver's license? *</legend>
    <input type="radio" required name="driver" id="r1" value="yes" />
    <label for="r1">Yes</label>
    <input type="radio" required name="driver" id="r2" value="no" />
    <label for="r2">No</label>
  </fieldset>
  <p>
    <label for="n1">How old are you?</label>
    <input type="number" min="12" max="120" step="1" id="n1" name="age" />
  </p>
  <p>
    <label for="t1">What's your favorite fruit? *</label>
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

E ora del CSS per definire lo stile dell'HTML:

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
  border: 1px solid #333333;
  box-sizing: border-box;
}

input:invalid {
  box-shadow: 0 0 5px 1px red;
}

input:focus:invalid {
  box-shadow: none;
}
```

Il risultato viene visualizzato come segue:

{{EmbedLiveSample("Full_example", "100%", 420)}}

È inoltre possibile premere il pulsante **Play** per aprire l'esempio in MDN Playground e modificarne il codice sorgente.

Vedere [Attributi relativi alla validazione](/it/docs/Web/HTML/Guides/Constraint_validation#validation-related_attributes) per un elenco completo degli attributi che possono essere utilizzati per vincolare i valori di input e dei tipi di input che li supportano.

## Validare i form usando JavaScript

Se si desidera modificare il testo dei messaggi di errore nativi, è necessario JavaScript.
In questa sezione verranno esaminati i diversi modi per farlo.

### La Constraint Validation API

La Constraint Validation API è costituita da un insieme di metodi e proprietà disponibili sulle seguenti interfacce DOM degli elementi del form:

- [`HTMLButtonElement`](/it/docs/Web/API/HTMLButtonElement) (rappresenta un elemento [`<button>`](/it/docs/Web/HTML/Reference/Elements/button))
- [`HTMLFieldSetElement`](/it/docs/Web/API/HTMLFieldSetElement) (rappresenta un elemento [`<fieldset>`](/it/docs/Web/HTML/Reference/Elements/fieldset))
- [`HTMLInputElement`](/it/docs/Web/API/HTMLInputElement) (rappresenta un elemento [`<input>`](/it/docs/Web/HTML/Reference/Elements/input))
- [`HTMLOutputElement`](/it/docs/Web/API/HTMLOutputElement) (rappresenta un elemento [`<output>`](/it/docs/Web/HTML/Reference/Elements/output))
- [`HTMLSelectElement`](/it/docs/Web/API/HTMLSelectElement) (rappresenta un elemento [`<select>`](/it/docs/Web/HTML/Reference/Elements/select))
- [`HTMLTextAreaElement`](/it/docs/Web/API/HTMLTextAreaElement) (rappresenta un elemento [`<textarea>`](/it/docs/Web/HTML/Reference/Elements/textarea))

La Constraint Validation API rende disponibili le seguenti proprietà sugli elementi indicati sopra.

- `validationMessage`: restituisce un messaggio localizzato che descrive i vincoli di validazione non soddisfatti dal controllo, se presenti. Se il controllo non è un candidato per la validazione dei vincoli (`willValidate` è `false`) o il valore dell'elemento soddisfa i relativi vincoli, ovvero è valido, verrà restituita una stringa vuota.
- `validity`: restituisce un oggetto `ValidityState` che contiene diverse proprietà che descrivono lo stato di validità dell'elemento. I dettagli completi di tutte le proprietà disponibili sono disponibili nella pagina di riferimento [`ValidityState`](/it/docs/Web/API/ValidityState); di seguito sono elencate alcune delle più comuni:
  - [`patternMismatch`](/it/docs/Web/API/ValidityState/patternMismatch): restituisce `true` se il valore non corrisponde al [`pattern`](/it/docs/Web/HTML/Reference/Elements/input#pattern) specificato, e `false` se corrisponde. Se è true, l'elemento corrisponde alla pseudo-classe CSS {{cssxref(":invalid")}}.
  - [`tooLong`](/it/docs/Web/API/ValidityState/tooLong): restituisce `true` se il valore è più lungo della lunghezza massima specificata dall'attributo [`maxlength`](/it/docs/Web/HTML/Reference/Elements/input#maxlength), oppure `false` se è più corto o uguale al massimo. Se è true, l'elemento corrisponde alla pseudo-classe CSS {{cssxref(":invalid")}}.
  - [`tooShort`](/it/docs/Web/API/ValidityState/tooShort): restituisce `true` se il valore è più corto della lunghezza minima specificata dall'attributo [`minlength`](/it/docs/Web/HTML/Reference/Elements/input#minlength), oppure `false` se è maggiore o uguale al minimo. Se è true, l'elemento corrisponde alla pseudo-classe CSS {{cssxref(":invalid")}}.
  - [`rangeOverflow`](/it/docs/Web/API/ValidityState/rangeOverflow): restituisce `true` se il valore è maggiore del massimo specificato dall'attributo [`max`](/it/docs/Web/HTML/Reference/Elements/input#max), oppure `false` se è minore o uguale al massimo. Se è true, l'elemento corrisponde alle pseudo-classi CSS {{cssxref(":invalid")}} e {{cssxref(":out-of-range")}}.
  - [`rangeUnderflow`](/it/docs/Web/API/ValidityState/rangeUnderflow): restituisce `true` se il valore è minore del minimo specificato dall'attributo [`min`](/it/docs/Web/HTML/Reference/Elements/input#min), oppure `false` se è maggiore o uguale al minimo. Se è true, l'elemento corrisponde alle pseudo-classi CSS {{cssxref(":invalid")}} e {{cssxref(":out-of-range")}}.
  - [`typeMismatch`](/it/docs/Web/API/ValidityState/typeMismatch): restituisce `true` se il valore non è nella sintassi richiesta, quando [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) è `email` o `url`, oppure `false` se la sintassi è corretta. Se è `true`, l'elemento corrisponde alla pseudo-classe CSS {{cssxref(":invalid")}}.
  - `valid`: restituisce `true` se l'elemento soddisfa tutti i propri vincoli di validazione ed è quindi considerato valido, oppure `false` se non soddisfa un qualsiasi vincolo. Se è true, l'elemento corrisponde alla pseudo-classe CSS {{cssxref(":valid")}}; altrimenti alla pseudo-classe CSS {{cssxref(":invalid")}}.
  - `valueMissing`: restituisce `true` se l'elemento ha un attributo [`required`](/it/docs/Web/HTML/Reference/Elements/input#required), ma non ha alcun valore, oppure `false` negli altri casi. Se è true, l'elemento corrisponde alla pseudo-classe CSS {{cssxref(":invalid")}}.

- `willValidate`: restituisce `true` se l'elemento verrà validato quando il form viene inviato; altrimenti `false`.

La Constraint Validation API rende disponibili anche i seguenti metodi sugli elementi indicati sopra e sull'elemento [`form`](/it/docs/Web/HTML/Reference/Elements/form).

- `checkValidity()`: restituisce `true` se il valore dell'elemento non presenta problemi di validità; altrimenti `false`. Se l'elemento non è valido, questo metodo genera anche un [evento `invalid`](/it/docs/Web/API/HTMLInputElement/invalid_event) sull'elemento.
- `reportValidity()`: segnala i campi non validi mediante eventi. Questo metodo è utile in combinazione con `preventDefault()` in un event handler `onSubmit`.
- `setCustomValidity(message)`: aggiunge un messaggio di errore personalizzato all'elemento; se viene impostato un messaggio di errore personalizzato, l'elemento è considerato non valido e viene visualizzato l'errore specificato. Ciò consente di utilizzare codice JavaScript per stabilire un errore di validazione diverso da quelli offerti dai vincoli standard di validazione HTML. Il messaggio viene mostrato all'utente quando viene segnalato il problema.

#### Implementare un messaggio di errore personalizzato

Come visto in precedenza negli esempi dei vincoli di validazione HTML, ogni volta che un utente tenta di inviare un form non valido, il browser visualizza un messaggio di errore. Il modo in cui questo messaggio viene visualizzato dipende dal browser.

Questi messaggi automatici presentano due svantaggi:

- Non esiste un modo standard per modificarne l'aspetto con CSS.
- Dipendono dalla lingua del browser, il che significa che è possibile avere una pagina in una lingua e un messaggio di errore visualizzato in un'altra lingua, come mostrato nella seguente schermata di Firefox.

![Esempio di messaggio di errore con Firefox in francese in una pagina inglese](error-firefox-win7.png)

La personalizzazione di questi messaggi di errore è uno dei casi d'uso più comuni della Constraint Validation API.
Vediamo un esempio di come farlo.

Si inizierà con un po' di HTML. Se si vuole, è possibile inserirlo in un'altra copia del file [iniziale di base](#file_iniziale_di_base):

```html
<form>
  <label for="mail">
    I would like you to provide me with an email address:
  </label>
  <input type="email" id="mail" name="mail" />
  <button>Submit</button>
</form>
```

Aggiungere il seguente JavaScript alla pagina:

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

Qui viene memorizzato un riferimento all'input email, quindi viene aggiunto un event listener che esegue il codice contenuto ogni volta che viene modificato il valore all'interno dell'input.

Nel codice contenuto, viene verificato se la proprietà `validity.typeMismatch` dell'input email restituisce `true`, il che significa che il valore contenuto non corrisponde al modello di un indirizzo email ben formato. In tal caso, viene chiamato il metodo [`setCustomValidity()`](/it/docs/Web/API/HTMLInputElement/setCustomValidity) con un messaggio personalizzato. Questo rende l'input non valido, quindi quando si tenta di inviare il form, l'invio non riesce e viene visualizzato il messaggio di errore personalizzato.

Se la proprietà `validity.typeMismatch` restituisce `false`, viene chiamato il metodo `setCustomValidity()` con una stringa vuota. Questo rende l'input valido, quindi il form verrà inviato. Durante la validazione, se un controllo del form ha un `customError` che non è una stringa vuota, l'invio del form viene bloccato.

È possibile provarlo qui sotto, premendo il pulsante **Play** per eseguire l'esempio in MDN Playground e modificarne il codice sorgente:

```html hidden live-sample___custom-error-message
<form>
  <label for="mail"
    >I would like you to provide me with an email address:</label
  >
  <input type="email" id="mail" name="mail" />
  <button>Submit</button>
</form>
```

```css hidden live-sample___custom-error-message
input:invalid {
  border: 2px dashed red;
}

input:valid {
  border: 2px solid black;
}
form {
  margin: 3rem 0;
}
```

```js hidden live-sample___custom-error-message
const email = document.getElementById("mail");

email.addEventListener("input", (event) => {
  if (email.validity.typeMismatch) {
    email.setCustomValidity("I am expecting an email address!");
  } else {
    email.setCustomValidity("");
  }
});

const form = document.querySelector("form");
form.addEventListener("submit", (e) => {
  e.preventDefault();
});
```

{{EmbedLiveSample("custom-error-message", "100%", 120, , , , , "allow-forms")}}

#### Estendere la validazione dei form integrata

L'esempio precedente ha mostrato come aggiungere un messaggio personalizzato per un particolare tipo di errore (`validity.typeMismatch`).
È inoltre possibile utilizzare tutta la validazione dei form integrata e poi estenderla usando `setCustomValidity()`.

Qui viene mostrato come estendere la validazione integrata di [`<input type="email">`](/it/docs/Web/HTML/Reference/Elements/input/email) per accettare solo indirizzi con il dominio `@example.com`.
Si inizia con il {{htmlelement("form")}} HTML seguente.

```html
<form>
  <label for="mail">Email address (@example.com only):</label>
  <input type="email" id="mail" />
  <button>Submit</button>
</form>
```

Il codice di validazione è mostrato di seguito.
In caso di nuovo input, il codice prima reimposta il messaggio di validità personalizzato chiamando `setCustomValidity("")`.
Quindi utilizza `email.validity.valid` per verificare se l'indirizzo inserito non è valido e, in tal caso, esce dall'event handler.
Ciò garantisce che tutti i normali controlli di validazione integrati vengano eseguiti mentre il testo inserito non è un indirizzo email valido.

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

Provare a inviare un indirizzo email non valido, un indirizzo email valido che non termina con `@example.com` e uno che termina con `@example.com`.

{{EmbedLiveSample("extending built-in form validation", "", 200, , , , , "allow-forms")}}

#### Un esempio più dettagliato

Ora che è stato visto un esempio davvero basilare, vediamo come utilizzare questa API per creare una validazione personalizzata leggermente più complessa.

Prima, l'HTML. Anche in questo caso, è possibile costruirlo insieme a noi:

```html
<form novalidate>
  <p>
    <label for="mail">
      <span>Please enter an email address *:</span>
      <input type="email" id="mail" name="mail" required minlength="8" />
      <span class="error" aria-live="polite"></span>
    </label>
  </p>
  <button>Submit</button>
</form>
```

Questo form utilizza l'attributo [`novalidate`](/it/docs/Web/HTML/Reference/Elements/form#novalidate) per disattivare la validazione automatica del browser. L'impostazione dell'attributo `novalidate` sul form impedisce al form di mostrare i propri messaggi di errore a fumetto e consente invece di visualizzare i messaggi di errore personalizzati nel DOM nel modo che si preferisce.
Tuttavia, ciò non disabilita il supporto per la Constraint Validation API né l'applicazione di pseudo-classi CSS come {{cssxref(":valid")}}, ecc.
Ciò significa che, anche se il browser non verifica automaticamente la validità del form prima di inviarne i dati, è comunque possibile farlo e applicare lo stile al form di conseguenza.

L'input da validare è un [`<input type="email">`](/it/docs/Web/HTML/Reference/Elements/input/email), che è `required` e ha un `minlength` di 8 caratteri. Verifichiamo questi aspetti con il nostro codice e mostriamo un messaggio di errore personalizzato per ciascuno.

L'obiettivo è mostrare i messaggi di errore all'interno di un elemento `<span>`.
L'attributo [`aria-live`](/it/docs/Web/Accessibility/ARIA/Guides/Live_regions) è impostato su tale `<span>` per garantire che il messaggio di errore personalizzato venga presentato a tutti, inclusa la lettura ad alta voce agli utenti di screen reader.

Passiamo ora a un CSS di base per migliorare leggermente l'aspetto del form e fornire un riscontro visivo quando i dati dell'input non sono validi:

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
  border: 1px solid #333333;
  margin: 0;

  font-family: inherit;
  font-size: 90%;

  box-sizing: border-box;
}

/* invalid fields */
input:invalid {
  border-color: #990000;
  background-color: #ffdddd;
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
  background-color: #990000;
  border-radius: 0 0 5px 5px;

  box-sizing: border-box;
}

.error.active {
  padding: 0.3em;
}
```

Ora esaminiamo il JavaScript che implementa la validazione degli errori personalizzata.
Esistono molti modi per selezionare un nodo DOM; qui vengono ottenuti il form stesso, la casella di input email e l'elemento span in cui verrà inserito il messaggio di errore.

Usando gli event handler, viene verificato se i campi del form sono validi ogni volta che l'utente digita qualcosa. Se c'è un errore, viene mostrato. Se non c'è alcun errore, viene rimosso qualsiasi messaggio di errore.

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

Ogni volta che viene modificato il valore dell'input, viene verificato se contiene dati validi. In caso affermativo, viene rimosso qualsiasi messaggio di errore visualizzato. Se i dati non sono validi, viene eseguita `showError()` per mostrare l'errore appropriato.

Ogni volta che si tenta di inviare il form, viene nuovamente verificato se i dati sono validi. In caso affermativo, il form viene inviato. In caso contrario, viene eseguita `showError()` per mostrare l'errore appropriato e viene impedito l'invio del form con [`preventDefault()`](/it/docs/Web/API/Event/preventDefault).

La funzione `showError()` utilizza varie proprietà dell'oggetto `validity` dell'input per determinare l'errore, quindi visualizza un messaggio di errore appropriato.

Ecco il risultato dal vivo: premere il pulsante **Play** per eseguire l'esempio in MDN Playground e modificarne il codice sorgente.

```html hidden live-sample___detailed-custom-validation
<form novalidate>
  <p>
    <label for="mail">
      <span>Please enter an email address *:</span>
      <input type="email" id="mail" name="mail" required minlength="8" />
      <span class="error" aria-live="polite"></span>
    </label>
  </p>
  <button>Submit</button>
</form>
```

```css hidden live-sample___detailed-custom-validation
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
  -webkit-appearance: none;
  appearance: none;

  width: 100%;
  border: 1px solid #333333;
  margin: 0;

  font-family: inherit;
  font-size: 90%;

  box-sizing: border-box;
}

/* This is our style for the invalid fields */
input:invalid {
  border-color: #990000;
  background-color: #ffdddd;
}

input:focus:invalid {
  outline: none;
}

/* This is the style of our error messages */
.error {
  width: 100%;
  padding: 0;

  font-size: 80%;
  color: white;
  background-color: #990000;
  border-radius: 0 0 5px 5px;

  box-sizing: border-box;
}

.error.active {
  padding: 0.3em;
}
```

```js hidden live-sample___detailed-custom-validation
// There are many ways to pick a DOM node; here we get the form itself and the email
// input box, as well as the span element into which we will place the error message.
const form = document.getElementsByTagName("form")[0];

const email = document.getElementById("mail");
const emailError = document.querySelector("#mail + span.error");

email.addEventListener("input", (event) => {
  // Each time the user types something, we check if the
  // form fields are valid.

  if (email.validity.valid) {
    // In case there is an error message visible, if the field
    // is valid, we remove the error message.
    emailError.innerHTML = ""; // Reset the content of the message
    emailError.className = "error"; // Reset the visual state of the message
  } else {
    // If there is still an error, show the correct error
    showError();
  }
});

form.addEventListener("submit", (event) => {
  // if the form contains valid data, we let it submit

  if (!email.validity.valid) {
    // If it isn't, we display an appropriate error message
    showError();
    // Then we prevent the form from being sent by canceling the event
    event.preventDefault();
  }
});

function showError() {
  if (email.validity.valueMissing) {
    // If the field is empty
    // display the following error message.
    emailError.textContent = "You need to enter an email address.";
  } else if (email.validity.typeMismatch) {
    // If the field doesn't contain an email address
    // display the following error message.
    emailError.textContent = "Entered value needs to be an email address.";
  } else if (email.validity.tooShort) {
    // If the data is too short
    // display the following error message.
    emailError.textContent = `Email should be at least ${email.minLength} characters; you entered ${email.value.length}.`;
  }

  // Set the styling appropriately
  emailError.className = "error active";
}

form.addEventListener("submit", (e) => {
  e.preventDefault();
});
```

{{EmbedLiveSample("detailed-custom-validation", "100%", 150, , , , , "allow-forms")}}

La Constraint Validation API fornisce uno strumento potente per gestire la validazione dei form, offrendo un enorme controllo sull'interfaccia utente, ben oltre ciò che è possibile ottenere con HTML e CSS da soli.

### Validare i form senza un'API integrata

In alcuni casi, come i [controlli personalizzati](/it/docs/Learn_web_development/Extensions/Forms/How_to_build_custom_form_controls), non sarà possibile o non si vorrà utilizzare la Constraint Validation API. È comunque possibile usare JavaScript per validare il form, ma sarà necessario scrivere la propria soluzione.

Per validare un form, è necessario porsi alcune domande:

- Che tipo di validazione deve essere eseguita?
  - : È necessario determinare come validare i dati: operazioni sulle stringhe, conversione dei tipi, espressioni regolari e così via. La scelta dipende dall'implementazione.
- Cosa deve accadere se il form non viene validato?
  - : Questa è chiaramente una questione di UI. È necessario decidere come si comporterà il form. Il form deve inviare comunque i dati?
    Devono essere evidenziati i campi che contengono errori?
    Devono essere visualizzati messaggi di errore?
- Come si può aiutare l'utente a correggere i dati non validi?
  - : Per ridurre la frustrazione dell'utente, è molto importante fornire quante più informazioni utili possibile per guidarlo nella correzione degli input.
    È opportuno offrire suggerimenti in anticipo, affinché sia chiaro cosa ci si aspetta, oltre a messaggi di errore chiari.
    Per approfondire i requisiti della UI per la validazione dei form, ecco alcuni articoli utili da leggere:
    - [Aiutare gli utenti a inserire i dati corretti nei form](https://web.dev/learn/forms/form-fields)
    - [Validare l'input](https://www.w3.org/WAI/tutorials/forms/validation/)
    - [Come segnalare gli errori nei form: 10 linee guida di progettazione](https://www.nngroup.com/articles/errors-forms-design-guidelines/)

#### Un esempio che non usa la Constraint Validation API

Per illustrare questo concetto, quanto segue è una versione semplificata dell'esempio precedente senza la Constraint Validation API.

L'HTML è quasi identico; sono state semplicemente rimosse le funzionalità di validazione HTML.

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

Allo stesso modo, il CSS non deve cambiare molto; la pseudo-classe CSS {{cssxref(":invalid")}} è stata semplicemente trasformata in una classe reale ed è stato evitato l'uso del selettore di attributo.

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
  border: 1px solid #333333;
  margin: 0;

  font-family: inherit;
  font-size: 90%;

  box-sizing: border-box;
}

/* invalid fields */
input.invalid {
  border: 2px solid #990000;
  background-color: #ffdddd;
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
  background-color: #990000;
  border-radius: 0 0 5px 5px;
  box-sizing: border-box;
}

.active {
  padding: 0.3rem;
}
```

I cambiamenti più importanti sono nel codice JavaScript, che deve svolgere molto più lavoro.

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
const updateError = (isValid) => {
  if (isValid) {
    error.textContent = "";
    error.removeAttribute("class");
  } else {
    error.textContent = "I expect an email, darling!";
    error.setAttribute("class", "active");
  }
};

// Handle input event to update email validity
const handleInput = () => {
  const validity = isValidEmail();
  setEmailClass(validity);
  updateError(validity);
};

// Handle form submission to show error if email is invalid
const handleSubmit = (event) => {
  event.preventDefault();

  const validity = isValidEmail();
  setEmailClass(validity);
  updateError(validity);
};

// Now we can rebuild our validation constraint
// Because we do not rely on CSS pseudo-class, we have to
// explicitly set the valid/invalid class on our email field
const validity = isValidEmail();
setEmailClass(validity);
// This defines what happens when the user types in the field
email.addEventListener("input", handleInput);
// This defines what happens when the user tries to submit the data
form.addEventListener("submit", handleSubmit);
```

Il risultato appare così:

{{EmbedLiveSample("An_example_that_doesnt_use_the_constraint_validation_API", "100%", 150)}}

Come si può vedere, non è così difficile creare autonomamente un sistema di validazione. La parte difficile consiste nel renderlo sufficientemente generico da poter essere utilizzato sia multipiattaforma sia con qualsiasi form si possa creare. Sono disponibili molte librerie per eseguire la validazione dei form, come [Validate.js](https://rickharrison.github.io/validate.js/).

## Riepilogo

La validazione dei form lato client talvolta richiede JavaScript se si desidera personalizzare lo stile e i messaggi di errore, ma richiede _sempre_ di riflettere attentamente sull'utente.
Ricordare sempre di aiutare gli utenti a correggere i dati forniti. A tal fine, assicurarsi di:

- Visualizzare messaggi di errore espliciti.
- Essere permissivi riguardo al formato di input.
- Indicare esattamente dove si verifica l'errore, specialmente nei form di grandi dimensioni.

Dopo aver verificato che il form sia compilato correttamente, può essere inviato.
Successivamente verrà trattato l'[invio dei dati del form](/it/docs/Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data).

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/UI_pseudo-classes", "Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data", "Learn_web_development/Extensions/Forms")}}
