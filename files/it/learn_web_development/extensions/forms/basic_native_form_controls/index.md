---
title: Controlli di modulo base nativi
slug: Learn_web_development/Extensions/Forms/Basic_native_form_controls
l10n:
  sourceCommit: a1ac64fa4da965d2a152f08221b1a9aed638fd16
---

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/How_to_structure_a_web_form", "Learn_web_development/Extensions/Forms/HTML5_input_types", "Learn_web_development/Extensions/Forms")}}

Nell'[articolo precedente](/it/docs/Learn_web_development/Extensions/Forms/How_to_structure_a_web_form), abbiamo strutturato un esempio di modulo web funzionale, introducendo alcuni controlli di modulo ed elementi strutturali comuni, concentrandoci sulle migliori pratiche di accessibilità. Successivamente, esamineremo in dettaglio la funzionalità dei diversi controlli di modulo, o widget, studiando tutte le diverse opzioni disponibili per raccogliere diversi tipi di dati. In questo articolo in particolare, esamineremo l'insieme originale di controlli di modulo, disponibili in tutti i browser fin dai primi giorni del web.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Una conoscenza di base di
        <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere in dettaglio l'insieme originale di widget di modulo nativi disponibili nei browser per la raccolta dati, e come implementarli utilizzando HTML.
      </td>
    </tr>
  </tbody>
</table>

Ha già incontrato alcuni elementi di modulo, inclusi {{HTMLelement('form')}}, {{HTMLelement('fieldset')}}, {{HTMLelement('legend')}}, {{HTMLelement('textarea')}}, {{HTMLelement('label')}}, {{HTMLelement('button')}}, e {{HTMLelement('input')}}. Questo articolo copre:

- I tipi comuni di input {{HTMLelement('input/button', 'button')}}, {{HTMLelement('input/checkbox', 'checkbox')}}, {{HTMLelement('input/file', 'file')}}, {{HTMLelement('input/hidden', 'hidden')}}, {{HTMLelement('input/image', 'image')}}, {{HTMLelement('input/password', 'password')}}, {{HTMLelement('input/radio', 'radio')}}, {{HTMLelement('input/reset', 'reset')}}, {{HTMLelement('input/submit', 'submit')}}, e {{HTMLelement('input/text', 'text')}}.
- Alcuni degli attributi comuni a tutti i controlli di modulo.

> [!NOTE]
> Trattiamo controlli di modulo aggiuntivi e più potenti nei due articoli successivi. Se desidera un riferimento più avanzato, dovrebbe consultare il nostro [Riferimento degli elementi dei moduli HTML](/it/docs/Web/HTML/Reference/Elements#forms), e in particolare il nostro ampio riferimento sui tipi di [`<input>`](/it/docs/Web/HTML/Reference/Elements/input).

## Campi di input testuali

I campi {{htmlelement("input")}} testuali sono i widget di modulo più basilari. Sono un modo molto conveniente per permettere all'utente di inserire qualsiasi tipo di dato, e abbiamo già visto alcuni semplici esempi.

> [!NOTE]
> I campi testuali dei moduli HTML sono semplici controlli di input di testo semplice. Ciò significa che non può usarli per eseguire l'editing di testo arricchito (grassetto, corsivo, ecc.). Tutti gli editor di testo arricchito che incontrerà sono widget personalizzati creati con HTML, CSS e JavaScript.

Tutti i controlli di testo di base condividono alcuni comportamenti comuni:

- Possono essere contrassegnati come [`readonly`](/it/docs/Web/HTML/Reference/Elements/input#readonly) (l'utente non può modificare il valore di input ma viene comunque inviato con il resto dei dati del modulo) o [`disabled`](/it/docs/Web/HTML/Reference/Elements/input#disabled) (il valore di input non può essere modificato e non viene mai inviato con il resto dei dati del modulo).
- Possono avere un [`placeholder`](/it/docs/Web/HTML/Reference/Elements/input#placeholder); questo è il testo che appare all'interno della casella di input testuale e che dovrebbe essere utilizzato per descrivere brevemente lo scopo della casella.
- Possono essere vincolati in [`size`](/it/docs/Web/HTML/Reference/Attributes/size) (la dimensione fisica della casella) e [`maxlength`](/it/docs/Web/HTML/Reference/Attributes/maxlength) (il numero massimo di caratteri che possono essere inseriti nella casella).
- Possono beneficiare del controllo ortografico (utilizzando l'attributo [`spellcheck`](/it/docs/Web/HTML/Reference/Global_attributes/spellcheck)).

> [!NOTE]
> L'elemento {{htmlelement("input")}} è unico tra gli elementi HTML perché può assumere molte forme a seconda del valore del suo attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type). Viene utilizzato per creare la maggior parte dei tipi di widget di modulo, inclusi campi di testo a riga singola, controlli di tempo e data, controlli senza input di testo come caselle di controllo, pulsanti di opzione e selettori di colori, e pulsanti.

### Campi di testo a riga singola

Un campo di testo a riga singola è creato utilizzando un elemento {{HTMLElement("input")}} il cui valore dell'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) è impostato su `text`, o omettendo del tutto l'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) (`text` è il valore predefinito). Il valore `text` per questo attributo è anche il valore di ripiego se il valore specificato per l'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) è sconosciuto al browser (ad esempio se specifica `type="color"` e il browser non supporta i selettori di colori nativi).

> [!NOTE]
> Può trovare esempi di tutti i tipi di campo di testo a riga singola su GitHub alla pagina [single-line-text-fields.html](https://github.com/mdn/learning-area/blob/main/html/forms/native-form-widgets/single-line-text-fields.html) ([vedi anche live](https://mdn.github.io/learning-area/html/forms/native-form-widgets/single-line-text-fields.html)).

Ecco un esempio base di campo di testo a riga singola:

```html
<input type="text" id="comment" name="comment" value="I'm a text field" />
```

I campi di testo a riga singola hanno solo un vero vincolo: se scrive del testo con interruzioni di riga, il browser rimuove quelle interruzioni di riga prima di inviare i dati al server.

Lo screenshot sotto mostra un input di testo negli stati di default, messa a fuoco e disabilitato. La maggior parte dei browser indica lo stato di messa a fuoco utilizzando un anello di messa a fuoco attorno al controllo e lo stato disabilitato utilizzando testo grigio o un controllo sbiadito/semi-opaco.

![Screenshot degli stati di default, focale e disabilitato dell'input di testo in Chrome su macOS](disabled.png)

Gli screenshot utilizzati in questo documento sono stati presi nel browser Chrome su macOS. Potrebbero esserci piccole variazioni in questi campi/pulsanti in diversi browser, ma la tecnica di evidenziazione di base rimane simile.

> [!NOTE]
> Discutiamo dei valori per l'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) che impongono vincoli di validazione specifici, inclusi i tipi di input color, email e url, nel prossimo articolo, [I tipi di input HTML5](/it/docs/Learn_web_development/Extensions/Forms/HTML5_input_types).

#### Campo password

Uno dei tipi di input originali era il campo di testo `password`:

```html
<input type="password" id="pwd" name="pwd" />
```

Lo screenshot seguente mostra un campo di input del tipo Password in cui ogni carattere inserito è mostrato come un punto.

![Campo password in chrome 115 su macOS](password.png)

Il valore `password` non aggiunge alcun vincolo speciale al testo inserito, ma oscura il valore inserito nel campo (ad es., con punti o asterischi) in modo che non possa essere facilmente letto da altri.

Tenga presente che questo è solo un aspetto dell'interfaccia utente; a meno che non invii il suo modulo in modo sicuro, verrà inviato in testo semplice, il che è dannoso per la sicurezza: una parte malevola potrebbe intercettare i suoi dati e rubare password, dati delle carte di credito o qualsiasi altra cosa lei abbia inviato. Il miglior modo per proteggere gli utenti da ciò è ospitare qualsiasi pagina che coinvolga moduli su una connessione sicura (cioè, situata a un indirizzo `https://`), così i dati sono crittografati prima di essere inviati.

I browser riconoscono le implicazioni di sicurezza dell'invio di dati dei moduli su una connessione non sicura e hanno avvisi per dissuadere gli utenti dall'usare moduli non sicuri. Per ulteriori informazioni su ciò che implementa Firefox, veda [Password non sicure](/it/docs/Web/Security/Insecure_passwords).

### Contenuto nascosto

Un altro controllo di testo originale è l'input di tipo `hidden`. Questo viene utilizzato per creare un controllo di modulo che è invisibile all'utente, ma che viene comunque inviato al server insieme al resto dei dati del modulo una volta inviato — ad esempio, potrebbe voler inviare al server un timestamp che indica quando un ordine è stato effettuato. Poiché è nascosto, l'utente non può vederlo né modificarlo intenzionalmente, non riceverà mai il focus, e nemmeno un screen reader lo noterà.

```html
<input type="hidden" id="timestamp" name="timestamp" value="1286705410" />
```

Se crea un tale elemento, è necessario impostare i suoi attributi `name` e `value`. Il valore può essere impostato dinamicamente tramite JavaScript. L'input di tipo `hidden` non dovrebbe avere un'etichetta associata.

Altri tipi di input testuale, come {{HTMLElement("input/search", "search")}}, {{HTMLElement("input/url", "url")}}, e {{HTMLElement("input/tel", "tel")}}, saranno trattati nel prossimo tutorial, [Tipo di input HTML5](/it/docs/Learn_web_development/Extensions/Forms/HTML5_input_types).

## Elementi selezionabili: caselle di controllo e pulsanti di opzione

Gli elementi selezionabili sono controlli il cui stato può essere modificato facendo clic su di essi o sulle loro etichette associate. Ci sono due tipi di elementi selezionabili: la casella di controllo e il pulsante di opzione. Entrambi usano l'attributo [`checked`](/it/docs/Web/HTML/Reference/Elements/input/checkbox#checked) per indicare se il widget è selezionato per impostazione predefinita o meno.

Vale la pena notare che questi widget non si comportano esattamente come altri widget di modulo. Per la maggior parte dei widget di modulo, una volta che il modulo è stato inviato, tutti i widget che hanno un attributo [`name`](/it/docs/Web/HTML/Reference/Elements/input#name) sono inviati, anche se non è stato compilato alcun valore. Nel caso degli elementi selezionabili, i loro valori vengono inviati solo se sono selezionati. Se non sono selezionati, niente viene inviato, nemmeno il loro nome. Se sono selezionati ma non hanno valore, il nome viene inviato con un valore di _on._

> [!NOTE]
> Può trovare gli esempi di questa sezione su GitHub come [checkable-items.html](https://github.com/mdn/learning-area/blob/main/html/forms/native-form-widgets/checkable-items.html) ([vedi anche live](https://mdn.github.io/learning-area/html/forms/native-form-widgets/checkable-items.html)).

Per la massima usabilità/accessibilità, è consigliato racchiudere ciascun elenco di elementi correlati in un {{htmlelement("fieldset")}}, con un {{htmlelement("legend")}} che fornisce una descrizione complessiva dell'elenco. Ogni coppia individuale di elementi {{htmlelement("label")}}/{{htmlelement("input")}} dovrebbe essere contenuta nel proprio elemento list item (o simile). L'etichetta associata è generalmente posizionata immediatamente prima o dopo il pulsante di opzione o la casella di controllo, con le istruzioni per il gruppo di pulsante di opzione o casella di controllo che sono generalmente il contenuto del {{htmlelement("legend")}}. Veda gli esempi collegati sopra per esempi strutturali.

### Casella di controllo

Una casella di controllo è creata usando l'elemento {{HTMLElement("input")}} con un attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) impostato al valore {{HTMLElement("input/checkbox", "checkbox")}}.

```html
<input type="checkbox" id="questionOne" name="subscribe" value="yes" checked />
```

Gli elementi di una casella di controllo correlata dovrebbero utilizzare lo stesso attributo [`name`](/it/docs/Web/HTML/Reference/Elements/input#name). Includere l'attributo [`checked`](/it/docs/Web/HTML/Reference/Elements/input/checkbox#checked) fa sì che la casella di controllo sia selezionata automaticamente quando la pagina viene caricata. Facendo clic sulla casella di controllo o sulla sua etichetta associata si attiva e disattiva la casella di controllo.

```html
<fieldset>
  <legend>Choose all the vegetables you like to eat</legend>
  <ul>
    <li>
      <label for="carrots">Carrots</label>
      <input
        type="checkbox"
        id="carrots"
        name="vegetable"
        value="carrots"
        checked />
    </li>
    <li>
      <label for="peas">Peas</label>
      <input type="checkbox" id="peas" name="vegetable" value="peas" />
    </li>
    <li>
      <label for="cabbage">Cabbage</label>
      <input type="checkbox" id="cabbage" name="vegetable" value="cabbage" />
    </li>
  </ul>
</fieldset>
```

Lo screenshot seguente mostra caselle di controllo negli stati di default, foco e disabilitato. Le caselle di controllo negli stati di default e disabilitato appaiono selezionate, mentre nello stato di focale, la casella di controllo non è selezionata, con un anello di messa a fuoco attorno.

![Stati della casella di controllo di default, foco e disabilitata in chrome 115 su macOS](checkboxes.png)

> [!NOTE]
> Qualsiasi casella di controllo e pulsante di opzione con l'attributo [`checked`](/it/docs/Web/HTML/Reference/Elements/input/checkbox#checked) al carico corrisponde alla pseudo-classe {{cssxref(':default')}}, anche se non sono più selezionati. Qualsiasi che è attualmente selezionato corrisponde alla pseudo-classe {{cssxref(':checked')}}.

A causa della natura on-off delle caselle di controllo, la casella di controllo è considerata un pulsante di attivazione, con molti sviluppatori e designer che ampliano lo stile predefinito della casella di controllo per creare pulsanti che sembrano interruttori a levetta. Può [vedere un esempio in azione qui](https://mdn.github.io/learning-area/html/forms/toggle-switch-example/) (veda anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/forms/toggle-switch-example/index.html)).

### Pulsante di opzione

Un pulsante di opzione è creato usando l'elemento {{HTMLElement("input")}} con il suo attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) impostato al valore `radio`:

```html
<input type="radio" id="soup" name="meal" value="soup" checked />
```

Diversi pulsanti di opzione possono essere legati insieme. Se condividono lo stesso valore per il loro attributo [`name`](/it/docs/Web/HTML/Reference/Elements/input#name), saranno considerati appartenere allo stesso gruppo di pulsanti. Solo un pulsante in un dato gruppo può essere selezionato alla volta; ciò significa che quando uno di loro è selezionato, tutti gli altri vengono deselezionati automaticamente. Quando il modulo viene inviato, solo il valore del pulsante di opzione selezionato viene inviato. Se nessuno di loro è selezionato, l'intero gruppo di pulsanti di opzione è considerato in uno stato sconosciuto e non viene inviato alcun valore con il modulo. Una volta che uno dei pulsanti di opzione in un gruppo di pulsanti con lo stesso nome è selezionato, non è possibile per l'utente deselezionare tutti i pulsanti senza resettare il modulo.

```html
<fieldset>
  <legend>What is your favorite meal?</legend>
  <ul>
    <li>
      <label for="soup">Soup</label>
      <input type="radio" id="soup" name="meal" value="soup" checked />
    </li>
    <li>
      <label for="curry">Curry</label>
      <input type="radio" id="curry" name="meal" value="curry" />
    </li>
    <li>
      <label for="pizza">Pizza</label>
      <input type="radio" id="pizza" name="meal" value="pizza" />
    </li>
  </ul>
</fieldset>
```

Lo screenshot seguente mostra pulsanti di opzione predefiniti e disabilitati nello stato selezionato, insieme a un pulsante di opzione focalizzato nello stato non selezionato.

![Pulsanti di opzione di default, foco e disabilitati in chrome 115 su macOS](radios.png)

## Bottoni veri e propri

Il pulsante di opzione in realtà non è un pulsante, nonostante il suo nome; passiamo avanti e esaminiamo i bottoni veri e propri! Ci sono tre tipi di input che producono bottoni:

- `submit`
  - : Invia i dati del modulo al server. Per gli elementi {{HTMLElement("button")}}, omettere l'attributo `type` (o un valore di `type` non valido) risulta in un pulsante di invio.
- `reset`
  - : Ripristina tutti i widget del modulo ai loro valori predefiniti.
- `button`
  - : Pulsanti che non hanno effetto automatico ma possono essere personalizzati usando il codice JavaScript.

Poi abbiamo anche l'elemento {{htmlelement("button")}} stesso. Questo può prendere un attributo `type` con valore `submit`, `reset`, o `button` per imitare il comportamento dei tre tipi `<input>` menzionati sopra. La differenza principale tra i due è che gli elementi `<button>` effettivi sono molto più facili da stilizzare.

```html
<input type="submit" value="Submit this form" />
<input type="reset" value="Reset this form" />
<input type="button" value="Do Nothing without JavaScript" />

<button type="submit">Submit this form</button>
<button type="reset">Reset this form</button>
<button type="button">Do Nothing without JavaScript</button>
```

```html hidden
<div class="button-demo">
  <p>Using &lt;input></p>
  <p>
    <input type="submit" value="Submit this form" />
    <input type="reset" value="Reset this form" />
    <input type="button" value="Do Nothing without JavaScript" />
  </p>
  <p>Using &lt;button></p>
  <p>
    <button type="submit">Submit this form</button>
    <button type="reset">Reset this form</button>
    <button type="button">Do Nothing without JavaScript</button>
  </p>
</div>
```

```css hidden
button,
input {
  display: none;
}
.button-demo button,
.button-demo input {
  all: revert;
}
```

{{ EmbedLiveSample('Actual_buttons', '500', '250') }}

> [!NOTE]
> Il tipo di input `image` si presenta anche come un pulsante. Lo copriremo più avanti.

> [!NOTE]
> Può trovare gli esempi di questa sezione su GitHub come [button-examples.html](https://github.com/mdn/learning-area/blob/main/html/forms/native-form-widgets/button-examples.html) ([vedi anche live](https://mdn.github.io/learning-area/html/forms/native-form-widgets/button-examples.html)).

Qui sotto può trovare esempi di ciascun tipo di `<input>` per i bottoni, insieme al tipo `<button>` equivalente.

### submit

```html
<button type="submit">This is a <strong>submit button</strong></button>

<input type="submit" value="This is a submit button" />
```

### reset

```html
<button type="reset">This is a <strong>reset button</strong></button>

<input type="reset" value="This is a reset button" />
```

### anonimo

```html
<button type="button">This is an <strong>anonymous button</strong></button>

<input type="button" value="This is an anonymous button" />
```

I bottoni si comportano sempre allo stesso modo, sia che si utilizzi un elemento {{HTMLElement("button")}} o un elemento {{HTMLElement("input")}}. Come può vedere dagli esempi, tuttavia, gli elementi {{HTMLElement("button")}} consentono di usare HTML nel loro contenuto, che è inserito tra i tag di apertura e chiusura `<button>`. Gli elementi {{HTMLElement("input")}} invece sono {{Glossary("void_element", "elementi void")}}; il loro contenuto visualizzato è inserito nell'attributo `value`, e quindi accetta solo testo semplice come contenuto.

Lo screenshot seguente mostra un bottone negli stati di default, foco, e disabilitato. Nello stato di foco, c'è un anello di messa a fuoco attorno al bottone, e nello stato disabilitato, il bottone è grigiato.

![Stati del bottone di default, focus e disabilitato in chrome 115 su macOS](buttons.png)

### Pulsante immagine

Il controllo **pulsante immagine** è reso esattamente come un elemento {{HTMLElement("img")}}, tranne per il fatto che quando l'utente fa clic su di esso, si comporta come un pulsante di invio.

Un pulsante immagine è creato usando un elemento {{HTMLElement("input")}} con il suo attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) impostato al valore `image`. Questo elemento supporta esattamente lo stesso insieme di attributi dell'elemento {{HTMLElement("img")}}, oltre a tutti gli attributi supportati da altri pulsanti di modulo.

```html
<input type="image" alt="Click me!" src="my-img.png" width="80" height="30" />
```

Se il pulsante immagine viene utilizzato per inviare il modulo, questo controllo non invia il suo valore — invece, vengono inviati le coordinate X e Y del clic sull'immagine (le coordinate sono relative all'immagine, il che significa che l'angolo in alto a sinistra dell'immagine rappresenta la coordinata (0, 0)). Le coordinate vengono inviate come due coppie chiave/valore:

- La chiave del valore X è il valore dell'attributo [`name`](/it/docs/Web/HTML/Reference/Elements/input#name) seguito dalla stringa "_.x_".
- La chiave del valore Y è il valore dell'attributo [`name`](/it/docs/Web/HTML/Reference/Elements/input#name) seguito dalla stringa "_.y_".

Quindi, ad esempio, quando fa clic sull'immagine alla coordinata (123, 456) e la invia tramite il metodo `get`, vedrà i valori aggiunti all'URL come segue:

```url
http://foo.com?pos.x=123&pos.y=456
```

Questo è un modo molto conveniente per costruire una "mappa interattiva". Come questi valori vengono inviati e recuperati è dettagliato nell'articolo [Invio di dati di modulo](/it/docs/Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data).

## Selettore di file

C'è un ultimo tipo `<input>` che ci è arrivato nei primi tempi di HTML: il tipo di input file. I moduli sono in grado di inviare file a un server (questa azione specifica è anche dettagliata nell'articolo [Invio di dati di modulo](/it/docs/Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data)). Il widget selettore di file può essere utilizzato per scegliere uno o più file da inviare.

Per creare un [widget selettore di file](/it/docs/Web/HTML/Reference/Elements/input/file), usa l'elemento {{HTMLElement("input")}} con il suo attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) impostato su `file`. I tipi di file che sono accettati possono essere vincolati utilizzando l'attributo [`accept`](/it/docs/Web/HTML/Reference/Elements/input#accept). Inoltre, se desidera consentire all'utente di scegliere più di un file, può farlo aggiungendo l'attributo [`multiple`](/it/docs/Web/HTML/Reference/Elements/input#multiple).

### Esempio

In questo esempio, viene creato un selettore di file che richiede file di immagini grafiche. In questo caso, è consentito all'utente di selezionare più file.

```html
<input type="file" name="file" id="file" accept="image/*" multiple />
```

Su alcuni dispositivi mobili, il selettore di file può accedere a foto, video e audio catturati direttamente dalla fotocamera e dal microfono del dispositivo aggiungendo informazioni di acquisizione all'attributo `accept` in questo modo:

```html
<input type="file" accept="image/*;capture=camera" />
<input type="file" accept="video/*;capture=camcorder" />
<input type="file" accept="audio/*;capture=microphone" />
```

Lo screenshot seguente mostra il widget selettore di file negli stati di default, focus e disabilitato quando non è selezionato nessun file.

![Widget selettore di file negli stati di default, focus e disabilitato in chrome 115 su macOS](filepickers.png)

## Attributi comuni

Molti degli elementi utilizzati per definire controlli di modulo hanno alcuni dei propri attributi specifici. Tuttavia, c'è un insieme di attributi comune a tutti gli elementi dei moduli. Ha già incontrato alcuni di questi, ma di seguito è riportato un elenco di quegli attributi comuni, per il suo riferimento:

<table class="no-markdown">
  <thead>
    <tr>
      <th scope="col">Nome attributo</th>
      <th scope="col">Valore predefinito</th>
      <th scope="col">Descrizione</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        <code
          ><a href="/it/docs/Web/HTML/Reference/Global_attributes/autofocus"
            >autofocus</a
          ></code
        >
      </td>
      <td>false</td>
      <td>
        Questo attributo Booleano consente di specificare che l'elemento dovrebbe automaticamente avere il focus di input quando la pagina viene caricata.
        Solo un elemento associato a un modulo in un documento può avere questo attributo specificato.
      </td>
    </tr>
    <tr>
      <td>
        <code
          ><a href="/it/docs/Web/HTML/Reference/Attributes/disabled">disabled</a></code
        >
      </td>
      <td>false</td>
      <td>
        Questo attributo Booleano indica che l'utente non può interagire con l'elemento.
        Se questo attributo non è specificato, l'elemento eredita le sue impostazioni dall'elemento contenitore, ad esempio, {{HTMLElement("fieldset")}};
        se non c'è un elemento contenitore con l'attributo <code>disabled</code> impostato, quindi l'elemento è abilitato.
      </td>
    </tr>
    <tr>
      <td>
        <code><a href="/it/docs/Web/HTML/Reference/Elements/input#form">form</a></code>
      </td>
      <td></td>
      <td>
        L'elemento <code>&#x3C;form></code> a cui il widget è associato, utilizzato se non è nidificato all'interno di quel modulo.
        Il valore dell'attributo deve essere l'attributo <code>id</code> di un elemento {{HTMLElement("form")}} nello stesso documento.
        Questo permette di associare un controllo di modulo a un modulo al di fuori di esso, anche se è all'interno di un altro elemento form.
      </td>
    </tr>
    <tr>
      <td>
        <code><a href="/it/docs/Web/HTML/Reference/Elements/input#name">name</a></code>
      </td>
      <td></td>
      <td>Il nome dell'elemento; questo viene inviato con i dati del modulo.</td>
    </tr>
    <tr>
      <td>
        <code><a href="/it/docs/Web/HTML/Reference/Elements/input#value">value</a></code>
      </td>
      <td></td>
      <td>Il valore iniziale dell'elemento.</td>
    </tr>
  </tbody>
</table>

## Metti alla prova le sue capacità!

Ha raggiunto la fine di questo articolo, ma riesce a ricordare le informazioni più importanti? Può trovare ulteriori test per verificare se ha assimilato queste informazioni prima di andare avanti — veda [Metti alla prova le sue capacità: Controlli di base](/it/docs/Learn_web_development/Extensions/Forms/Test_your_skills/Basic_controls).

## Sommario

Questo articolo ha trattato i vecchi tipi di input — l'insieme originale introdotto nei primi giorni di HTML che è ben supportato in tutti i browser. Nella prossima sezione, esamineremo i valori più moderni dell'attributo `type`.

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/How_to_structure_a_web_form", "Learn_web_development/Extensions/Forms/HTML5_input_types", "Learn_web_development/Extensions/Forms")}}
