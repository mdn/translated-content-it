---
title: Controlli nativi di base dei moduli
slug: Learn_web_development/Extensions/Forms/Basic_native_form_controls
l10n:
  sourceCommit: c9f3d85f24d7839c9fe36a68d8042d088d906147
---

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/How_to_structure_a_web_form", "Learn_web_development/Extensions/Forms/HTML5_input_types", "Learn_web_development/Extensions/Forms")}}

Nell'[articolo precedente](/it/docs/Learn_web_development/Extensions/Forms/How_to_structure_a_web_form), è stato creato il markup di un esempio di modulo web funzionale, introducendo alcuni controlli dei moduli ed elementi strutturali comuni e concentrandosi sulle best practice di accessibilità. Successivamente, verranno esaminate in dettaglio le funzionalità dei diversi controlli dei moduli, o widget, studiando tutte le diverse opzioni disponibili per raccogliere differenti tipi di dati. In questo articolo specifico verrà esaminato il set originale di controlli dei moduli, disponibile in tutti i browser fin dai primi giorni del web.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Una conoscenza di base di
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >HTML</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere in dettaglio il set originale di widget nativi dei moduli
        disponibili nei browser per raccogliere dati e come implementarli
        usando HTML.
      </td>
    </tr>
  </tbody>
</table>

Sono già stati incontrati alcuni elementi dei moduli, inclusi {{HTMLelement('form')}}, {{HTMLelement('fieldset')}}, {{HTMLelement('legend')}}, {{HTMLelement('textarea')}}, {{HTMLelement('label')}}, {{HTMLelement('button')}} e {{HTMLelement('input')}}. Questo articolo tratta:

- I tipi di input comuni {{HTMLelement('input/button', 'button')}}, {{HTMLelement('input/checkbox', 'checkbox')}}, {{HTMLelement('input/file', 'file')}}, {{HTMLelement('input/hidden', 'hidden')}}, {{HTMLelement('input/image', 'image')}}, {{HTMLelement('input/password', 'password')}}, {{HTMLelement('input/radio', 'radio')}}, {{HTMLelement('input/reset', 'reset')}}, {{HTMLelement('input/submit', 'submit')}} e {{HTMLelement('input/text', 'text')}}.
- Alcuni attributi comuni a tutti i controlli dei moduli.

> [!NOTE]
> I controlli dei moduli aggiuntivi e più potenti vengono trattati nei due articoli successivi. Per un riferimento più avanzato, consultare il [riferimento degli elementi dei moduli HTML](/it/docs/Web/HTML/Reference/Elements#forms) e, in particolare, l'ampio riferimento ai [tipi di `<input>`](/it/docs/Web/HTML/Reference/Elements/input).

## Campi di input di testo

I campi di {{htmlelement("input")}} di testo sono i widget dei moduli più basilari. Rappresentano un modo molto pratico per consentire all'utente di inserire qualsiasi tipo di dati e ne sono già stati illustrati alcuni semplici esempi.

> [!NOTE]
> I campi di testo dei moduli HTML sono semplici controlli di input di testo semplice. Ciò significa che non possono essere usati per eseguire modifiche di testo avanzate (grassetto, corsivo e così via). Tutti gli editor di testo avanzati che si incontrano sono widget personalizzati creati con HTML, CSS e JavaScript.

Tutti i controlli di testo di base condividono alcuni comportamenti comuni:

- Possono essere contrassegnati come [`readonly`](/it/docs/Web/HTML/Reference/Elements/input#readonly) (l'utente non può modificare il valore di input, ma questo viene comunque inviato insieme al resto dei dati del modulo) oppure [`disabled`](/it/docs/Web/HTML/Reference/Elements/input#disabled) (il valore di input non può essere modificato e non viene mai inviato insieme al resto dei dati del modulo).
- Possono avere un [`placeholder`](/it/docs/Web/HTML/Reference/Elements/input#placeholder); si tratta del testo che appare all'interno della casella di input di testo e che dovrebbe essere usato per descrivere brevemente lo scopo della casella.
- Possono essere limitati tramite [`size`](/it/docs/Web/HTML/Reference/Attributes/size) (la dimensione fisica della casella) e [`maxlength`](/it/docs/Web/HTML/Reference/Attributes/maxlength) (il numero massimo di caratteri che possono essere inseriti nella casella).
- Possono usufruire del controllo ortografico (usando l'attributo [`spellcheck`](/it/docs/Web/HTML/Reference/Global_attributes/spellcheck)).

> [!NOTE]
> L'elemento {{htmlelement("input")}} è unico tra gli elementi HTML perché può assumere molte forme in base al valore del suo attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type). Viene usato per creare la maggior parte dei tipi di widget dei moduli, inclusi campi di testo a riga singola, controlli per ora e data, controlli senza input di testo come checkbox, radio button e selettori di colore, nonché pulsanti.

### Campi di testo a riga singola

Un campo di testo a riga singola viene creato usando un elemento {{HTMLElement("input")}} il cui valore dell'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) è impostato su [`text`](/it/docs/Web/HTML/Reference/Elements/input/text), oppure omettendo completamente l'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) (`text` è il valore predefinito). Il valore `text` per questo attributo è anche il valore di fallback se il valore specificato per l'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) non è noto al browser (ad esempio, se viene specificato `type="color"` e il browser non supporta selettori di colore nativi).

Ecco un esempio di base di campo di testo a riga singola:

```html live-sample___single-line
<input type="text" id="comment" name="comment" value="I'm a text field" />
```

Viene visualizzato in questo modo:

{{embedlivesample("single-line", "100%", "80")}}

I campi di testo a riga singola hanno una sola vera limitazione: se viene digitato testo con interruzioni di riga, il browser rimuove tali interruzioni prima di inviare i dati al server.

La schermata seguente mostra un input di testo negli stati predefinito, attivo e disabilitato. La maggior parte dei browser indica lo stato attivo usando un anello di focus attorno al controllo e lo stato disabilitato usando testo grigio o un controllo attenuato/semiopaco.

![Schermata dell'input di testo negli stati predefinito, attivo e disabilitato in Chrome su macOS](disabled.png)

Le schermate usate in questo documento sono state acquisite nel browser Chrome su macOS. Potrebbero esserci lievi variazioni in questi campi/pulsanti tra browser diversi, ma la tecnica di evidenziazione di base rimane simile.

> [!NOTE]
> I valori dell'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) che applicano vincoli di convalida specifici, inclusi i tipi di input color, email e url, vengono trattati nel prossimo articolo, [I tipi di input HTML5](/it/docs/Learn_web_development/Extensions/Forms/HTML5_input_types).

#### Campo password

Uno dei tipi di input originali era il tipo di campo di testo [`password`](/it/docs/Web/HTML/Reference/Elements/input/password):

```html live-sample___password
<input type="password" id="pwd" name="pwd" />
```

Viene visualizzato in modo simile al campo di testo di base a riga singola:

{{embedlivesample("password", "100%", "80")}}

Tuttavia, provare a digitare nel campo: ogni carattere inserito verrà visualizzato come un punto.

Il valore `password` non aggiunge vincoli speciali al testo inserito, ma oscura il valore immesso nel campo in modo che non possa essere letto facilmente da altri.

Tenere presente che questa è soltanto una funzionalità dell'interfaccia utente; a meno che il modulo non venga inviato in modo sicuro, sarà trasmesso in testo semplice, il che è dannoso per la sicurezza: una parte malevola potrebbe intercettare i dati e rubare password, dettagli delle carte di credito o qualsiasi altra informazione inviata. Il modo migliore per proteggere gli utenti consiste nell'ospitare tutte le pagine che includono moduli su una connessione sicura (ossia a un indirizzo `https://`), affinché i dati siano crittografati prima dell'invio.

I browser riconoscono le implicazioni di sicurezza dell'invio di dati dei moduli tramite una connessione non sicura e dispongono di avvisi per scoraggiare gli utenti dall'uso di moduli non sicuri.

### Contenuto nascosto

Un altro controllo di testo originale è il tipo di input [`hidden`](/it/docs/Web/HTML/Reference/Elements/input/hidden). Viene usato per creare un controllo del modulo invisibile all'utente, ma comunque inviato al server insieme al resto dei dati del modulo una volta inviato il modulo stesso; ad esempio, potrebbe essere necessario inviare al server un timestamp che indichi quando è stato effettuato un ordine. Poiché è nascosto, l'utente non può vedere né modificare intenzionalmente il valore, non riceverà mai il focus e nemmeno uno screen reader lo rileverà.

```html
<input type="hidden" id="timestamp" name="timestamp" value="1286705410" />
```

Se viene creato un elemento di questo tipo, è necessario impostarne gli attributi `name` e `value`. Il valore può essere impostato dinamicamente tramite JavaScript. Il tipo di input `hidden` non dovrebbe avere un label associato.

Altri tipi di input di testo, come {{HTMLElement("input/search", "search")}}, {{HTMLElement("input/url", "url")}} e {{HTMLElement("input/tel", "tel")}}, verranno trattati nel prossimo tutorial, [Tipi di input HTML5](/it/docs/Learn_web_development/Extensions/Forms/HTML5_input_types).

## Elementi selezionabili: checkbox e radio button

Gli elementi selezionabili sono controlli il cui stato può essere modificato facendo clic su di essi o sulle relative label associate. Esistono due tipi di elementi selezionabili: la checkbox e il radio button. Entrambi usano l'attributo [`checked`](/it/docs/Web/HTML/Reference/Elements/input/checkbox#checked) per indicare se il widget è selezionato per impostazione predefinita o meno.

Vale la pena notare che questi widget non si comportano esattamente come gli altri widget dei moduli. Per la maggior parte dei widget dei moduli, una volta inviato il modulo vengono inviati tutti i widget che hanno un attributo [`name`](/it/docs/Web/HTML/Reference/Elements/input#name), anche se non è stato compilato alcun valore. Nel caso degli elementi selezionabili, i relativi valori vengono inviati solo se sono selezionati. Se non sono selezionati, non viene inviato nulla, nemmeno il loro nome. Se sono selezionati ma non hanno valore, il nome viene inviato con valore _on._

Per la massima usabilità/accessibilità, è consigliabile racchiudere ogni elenco di elementi correlati in un {{htmlelement("fieldset")}}, con un {{htmlelement("legend")}} che fornisca una descrizione generale dell'elenco. Ogni singola coppia di elementi {{htmlelement("label")}}/{{htmlelement("input")}} dovrebbe essere contenuta nel proprio elemento di elenco (o simile). Il {{htmlelement('label')}} associato viene in genere posizionato immediatamente prima o dopo il radio button o la checkbox, mentre le istruzioni per il gruppo di radio button o checkbox sono generalmente il contenuto del {{htmlelement("legend")}}.

### Checkbox

Una checkbox viene creata usando l'elemento {{HTMLElement("input")}} con un attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) impostato sul valore [`checkbox`](/it/docs/Web/HTML/Reference/Elements/input/checkbox).

```html
<input type="checkbox" id="questionOne" name="subscribe" value="yes" checked />
```

Gli elementi checkbox correlati dovrebbero usare lo stesso attributo [`name`](/it/docs/Web/HTML/Reference/Elements/input#name). L'inclusione dell'attributo [`checked`](/it/docs/Web/HTML/Reference/Elements/input/checkbox#checked) rende la checkbox selezionata automaticamente al caricamento della pagina. Facendo clic sulla checkbox o sulla relativa label associata, la checkbox viene attivata e disattivata.

```html live-sample___checkbox
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

Questo esempio viene visualizzato come segue:

{{embedlivesample("checkbox", "100%", "150")}}

La schermata seguente mostra checkbox negli stati predefinito, attivo e disabilitato. Le checkbox negli stati predefinito e disabilitato appaiono selezionate, mentre nello stato attivo la checkbox non è selezionata ed è circondata da un anello di focus.

![Checkbox predefinite, attive e disabilitate in Chrome 115 su macOS](checkboxes.png)

> [!NOTE]
> Tutte le checkbox e i radio button con l'attributo [`checked`](/it/docs/Web/HTML/Reference/Elements/input/checkbox#checked) al caricamento corrispondono alla pseudo-classe {{cssxref(':default')}}, anche se non sono più selezionati. Tutti quelli attualmente selezionati corrispondono alla pseudo-classe {{cssxref(':checked')}}.

A causa della natura attivo-disattivo delle checkbox, la checkbox è considerata un pulsante di attivazione, e molti sviluppatori e designer ampliano lo stile predefinito delle checkbox per creare pulsanti simili a interruttori. È possibile [vedere qui un esempio in azione](https://mdn.github.io/learning-area/html/forms/toggle-switch-example/) (vedere anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/forms/toggle-switch-example/index.html)).

### Radio button

Un radio button viene creato usando l'elemento {{HTMLElement("input")}} con il relativo attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) impostato sul valore [`radio`](/it/docs/Web/HTML/Reference/Elements/input/radio):

```html
<input type="radio" id="soup" name="meal" value="soup" checked />
```

È possibile collegare tra loro più radio button. Se condividono lo stesso valore per il loro attributo [`name`](/it/docs/Web/HTML/Reference/Elements/input#name), verranno considerati appartenenti allo stesso gruppo di pulsanti. Può essere selezionato solo un pulsante di un dato gruppo alla volta; ciò significa che, quando uno viene selezionato, tutti gli altri vengono automaticamente deselezionati. Quando il modulo viene inviato, viene inviato solo il valore del radio button selezionato. Se nessuno è selezionato, l'intero gruppo di radio button viene considerato in uno stato sconosciuto e non viene inviato alcun valore con il modulo. Una volta selezionato uno dei radio button in un gruppo di pulsanti con lo stesso nome, non è possibile per l'utente deselezionare tutti i pulsanti senza reimpostare il modulo.

```html live-sample___radio
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

Questo esempio viene visualizzato come segue:

{{embedlivesample("radio", "100%", "150")}}

La schermata seguente mostra radio button predefiniti e disabilitati nello stato selezionato, insieme a un radio button attivo nello stato non selezionato.

![Radio button predefiniti, attivi e disabilitati in Chrome 115 su macOS](radios.png)

## Pulsanti effettivi

Il radio button non è realmente un pulsante, nonostante il nome; passiamo quindi ai pulsanti effettivi. Esistono tre tipi di input che producono pulsanti:

- [`submit`](/it/docs/Web/HTML/Reference/Elements/input/submit)
  - : Invia i dati del modulo al server. Per gli elementi {{HTMLElement("button")}}, l'omissione dell'attributo `type` (o un valore non valido di `type`) genera un pulsante di invio.
- [`reset`](/it/docs/Web/HTML/Reference/Elements/input/reset)
  - : Reimposta tutti i widget del modulo ai loro valori predefiniti.
- [`button`](/it/docs/Web/HTML/Reference/Elements/input/button)
  - : Pulsanti che non hanno alcun effetto automatico, ma possono essere personalizzati usando codice JavaScript.

Vi è poi l'elemento {{htmlelement("button")}} stesso. Può avere un attributo `type` con valore `submit`, `reset` o `button` per imitare il comportamento dei tre tipi di `<input>` menzionati sopra. La differenza principale tra i due è che gli elementi `<button>` effettivi sono molto più facili da stilizzare.

```html live-sample___actual_buttons_ex
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
```

{{ EmbedLiveSample('actual_buttons_ex', '500', '250') }}

> [!NOTE]
> Anche il tipo di input `image` viene visualizzato come pulsante. Verrà trattato più avanti.

Di seguito sono disponibili esempi di ogni tipo di `<input>` pulsante, insieme al tipo `<button>` equivalente. Ogni coppia è stata racchiusa in un elemento {{htmlelement("div")}} per separarla su una nuova riga.

- Pulsante di invio:

  ```html live-sample___buttons
  <div>
    <button type="submit">This is a <strong>submit button</strong></button>

    <input type="submit" value="This is a submit button" />
  </div>
  ```

- Pulsante di reimpostazione:

  ```html live-sample___buttons
  <div>
    <button type="reset">This is a <strong>reset button</strong></button>

    <input type="reset" value="This is a reset button" />
  </div>
  ```

- Pulsante anonimo:

  ```html live-sample___buttons
  <div>
    <button type="button">This is an <strong>anonymous button</strong></button>

    <input type="button" value="This is an anonymous button" />
  </div>
  ```

Questi esempi vengono visualizzati come segue:

{{embedlivesample("buttons", "100%", "150")}}

I pulsanti si comportano sempre allo stesso modo, sia che venga usato un elemento {{HTMLElement("button")}} sia un elemento {{HTMLElement("input")}}. Tuttavia, come si può vedere dagli esempi, gli elementi {{HTMLElement("button")}} consentono di usare HTML nel proprio contenuto, che viene inserito tra i tag `<button>` di apertura e chiusura. Gli elementi {{HTMLElement("input")}}, invece, sono {{Glossary("void_element", "elementi void")}}; il relativo contenuto visualizzato viene inserito nell'attributo `value` e, pertanto, accetta solo testo semplice come contenuto.

La schermata seguente mostra un pulsante negli stati predefinito, attivo e disabilitato. Nello stato attivo è presente un anello di focus attorno al pulsante, mentre nello stato disabilitato il pulsante è grigio.

![Stati del pulsante predefinito, attivo e disabilitato in Chrome 115 su macOS](buttons.png)

### Pulsante immagine

Il controllo **pulsante immagine** viene visualizzato esattamente come un elemento {{HTMLElement("img")}}, tranne per il fatto che quando l'utente fa clic su di esso si comporta come un pulsante di invio.

Un pulsante immagine viene creato usando un elemento {{HTMLElement("input")}} con l'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) impostato sul valore [`image`](/it/docs/Web/HTML/Reference/Elements/input/image). Questo elemento supporta esattamente lo stesso insieme di attributi dell'elemento {{HTMLElement("img")}}, oltre a tutti gli attributi supportati dagli altri pulsanti dei moduli.

```html
<input type="image" alt="Click me!" src="my-img.png" width="80" height="30" />
```

Se il pulsante immagine viene usato per inviare il modulo, questo controllo non invia il proprio valore; vengono invece inviate le coordinate X e Y del clic sull'immagine (le coordinate sono relative all'immagine, ossia l'angolo superiore sinistro dell'immagine rappresenta la coordinata (0, 0)). Le coordinate vengono inviate come due coppie chiave/valore:

- La chiave del valore X è il valore dell'attributo [`name`](/it/docs/Web/HTML/Reference/Elements/input#name) seguito dalla stringa "_.x_".
- La chiave del valore Y è il valore dell'attributo [`name`](/it/docs/Web/HTML/Reference/Elements/input#name) seguito dalla stringa "_.y_".

Quindi, ad esempio, facendo clic sull'immagine alla coordinata (123, 456) e inviando tramite il metodo `get`, i valori verranno aggiunti all'URL come segue:

```url
https://example.com?pos.x=123&pos.y=456
```

Questo è un modo molto pratico per creare una "mappa sensibile". Il modo in cui questi valori vengono inviati e recuperati è descritto in dettaglio nell'articolo [Invio dei dati del modulo](/it/docs/Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data).

## Selettore di file

Esiste un ultimo tipo di `<input>` arrivato con le prime versioni di HTML: il tipo di input file. I moduli sono in grado di inviare file a un server (questa azione specifica è illustrata anch'essa nell'articolo [Invio dei dati del modulo](/it/docs/Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data)). Il widget selettore di file può essere usato per scegliere uno o più file da inviare.

Per creare un [widget selettore di file](/it/docs/Web/HTML/Reference/Elements/input/file), si usa l'elemento {{HTMLElement("input")}} con il relativo attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) impostato su `file`. I tipi di file accettati possono essere limitati usando l'attributo [`accept`](/it/docs/Web/HTML/Reference/Elements/input#accept). Inoltre, se si desidera consentire all'utente di scegliere più di un file, è possibile farlo aggiungendo l'attributo [`multiple`](/it/docs/Web/HTML/Reference/Elements/input#multiple).

### Esempio

In questo esempio viene creato un selettore di file che richiede file di immagini grafiche. In questo caso l'utente può selezionare più file.

```html
<input type="file" name="file" id="file" accept="image/*" multiple />
```

Su alcuni dispositivi mobili, il selettore di file può accedere a foto, video e audio acquisiti direttamente dalla fotocamera e dal microfono del dispositivo aggiungendo informazioni di acquisizione all'attributo `accept`, come segue:

```html
<input type="file" accept="image/*;capture=camera" />
<input type="file" accept="video/*;capture=camcorder" />
<input type="file" accept="audio/*;capture=microphone" />
```

La schermata seguente mostra il widget selettore di file negli stati predefinito, attivo e disabilitato quando non è selezionato alcun file.

![Widget selettore di file negli stati predefinito, attivo e disabilitato in Chrome 115 su macOS](filepickers.png)

## Attributi comuni

Molti degli elementi usati per definire i controlli dei moduli hanno alcuni attributi specifici propri. Tuttavia, esiste un insieme di attributi comune a tutti gli elementi dei moduli. Alcuni di questi sono già stati incontrati, ma di seguito è riportato un elenco di tali attributi comuni come riferimento:

<table class="no-markdown">
  <thead>
    <tr>
      <th scope="col">Nome dell'attributo</th>
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
        Questo attributo Boolean consente di specificare che l'elemento debba ricevere automaticamente il focus di input al caricamento della pagina.
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
        Questo attributo Boolean indica che l'utente non può interagire con l'elemento.
        Se questo attributo non viene specificato, l'elemento eredita la propria impostazione dall'elemento contenitore, ad esempio {{HTMLElement("fieldset")}};
        se non esiste un elemento contenitore con l'attributo <code>disabled</code> impostato, l'elemento è abilitato.
      </td>
    </tr>
    <tr>
      <td>
        <code><a href="/it/docs/Web/HTML/Reference/Elements/input#form">form</a></code>
      </td>
      <td></td>
      <td>
        L'elemento <code>&#x3C;form></code> a cui il widget è associato, usato se non è annidato all'interno di tale modulo.
        Il valore dell'attributo deve essere l'attributo <code>id</code> di un elemento {{HTMLElement("form")}} nello stesso documento.
        Ciò consente di associare un controllo del modulo a un modulo esterno a esso, anche se si trova all'interno di un elemento modulo diverso.
      </td>
    </tr>
    <tr>
      <td>
        <code><a href="/it/docs/Web/HTML/Reference/Elements/input#name">name</a></code>
      </td>
      <td></td>
      <td>Il nome dell'elemento; viene inviato con i dati del modulo.</td>
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

## Riepilogo

Questo articolo ha trattato i tipi di input più vecchi, ovvero il set originale introdotto nei primi giorni di HTML e ben supportato in tutti i browser. Nella prossima sezione verranno esaminati i valori più moderni dell'attributo `type`.

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/How_to_structure_a_web_form", "Learn_web_development/Extensions/Forms/HTML5_input_types", "Learn_web_development/Extensions/Forms")}}
