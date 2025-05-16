---
title: Controlli nativi di base per i moduli
slug: Learn_web_development/Extensions/Forms/Basic_native_form_controls
l10n:
  sourceCommit: c05ef6211441aedb359d4020518ac152aa92db9e
---

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/How_to_structure_a_web_form", "Learn_web_development/Extensions/Forms/HTML5_input_types", "Learn_web_development/Extensions/Forms")}}

Nell'[articolo precedente](/it/docs/Learn_web_development/Extensions/Forms/How_to_structure_a_web_form), abbiamo marcato un esempio di modulo web funzionale, introducendo alcuni controlli di modulo ed elementi strutturali comuni, concentrandoci sulle migliori pratiche per l'accessibilità. Successivamente, vedremo in dettaglio la funzionalità dei diversi controlli di modulo, o widget, studiando tutte le opzioni disponibili per raccogliere diversi tipi di dati. In questo particolare articolo, esamineremo il set originale di controlli di modulo, disponibili in tutti i browser sin dai primi giorni del web.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Una comprensione di base di
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >HTML</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere in dettaglio il set originale di widget di modulo nativi
        disponibili nei browser per la raccolta dati e come implementarli
        usando HTML.
      </td>
    </tr>
  </tbody>
</table>

Ha già incontrato alcuni elementi di modulo, inclusi {{HTMLelement('form')}}, {{HTMLelement('fieldset')}}, {{HTMLelement('legend')}}, {{HTMLelement('textarea')}}, {{HTMLelement('label')}}, {{HTMLelement('button')}}, e {{HTMLelement('input')}}. Questo articolo copre:

- I tipi di input comuni {{HTMLelement('input/button', 'button')}}, {{HTMLelement('input/checkbox', 'checkbox')}}, {{HTMLelement('input/file', 'file')}}, {{HTMLelement('input/hidden', 'hidden')}}, {{HTMLelement('input/image', 'image')}}, {{HTMLelement('input/password', 'password')}}, {{HTMLelement('input/radio', 'radio')}}, {{HTMLelement('input/reset', 'reset')}}, {{HTMLelement('input/submit', 'submit')}}, e {{HTMLelement('input/text', 'text')}}.
- Alcuni attributi comuni a tutti i controlli di modulo.

> [!NOTE]
> Tratteremo ulteriori controlli di modulo più potenti nei prossimi due articoli. Se desidera un riferimento più avanzato, dovrebbe consultare il nostro [riferimento agli elementi dei moduli HTML](/it/docs/Web/HTML/Reference/Elements#forms), e in particolare il nostro ampio riferimento ai tipi di [`<input>`](/it/docs/Web/HTML/Reference/Elements/input).

## Campi di input di testo

I campi di testo {{htmlelement("input")}} sono i widget di modulo più basilari. Sono un modo molto conveniente per consentire all'utente di inserire qualsiasi tipo di dato, e abbiamo già visto alcuni semplici esempi.

> [!NOTE]
> I campi di testo HTML nei moduli sono controlli di input di testo semplice. Questo significa che non può usarli per eseguire l'editing di testo avanzato (grassetto, corsivo, ecc.). Tutti gli editor di testo avanzato che incontrerà sono widget personalizzati creati con HTML, CSS e JavaScript.

Tutti i controlli di testo di base condividono alcuni comportamenti comuni:

- Possono essere contrassegnati come [`readonly`](/it/docs/Web/HTML/Reference/Elements/input#readonly) (l'utente non può modificare il valore dell'input ma viene comunque inviato con il resto dei dati del modulo) o [`disabled`](/it/docs/Web/HTML/Reference/Elements/input#disabled) (il valore dell'input non può essere modificato e non viene mai inviato con il resto dei dati del modulo).
- Possono avere un [`placeholder`](/it/docs/Web/HTML/Reference/Elements/input#placeholder); si tratta del testo che appare all'interno della casella di input di testo che dovrebbe essere usato per descrivere brevemente lo scopo della casella.
- Possono essere limitati in [`size`](/it/docs/Web/HTML/Reference/Attributes/size) (la dimensione fisica della casella) e [`maxlength`](/it/docs/Web/HTML/Reference/Attributes/maxlength) (il numero massimo di caratteri che possono essere inseriti nella casella).
- Possono beneficiare del controllo ortografico (usando l'attributo [`spellcheck`](/it/docs/Web/HTML/Reference/Global_attributes/spellcheck)).

> [!NOTE]
> L'elemento {{htmlelement("input")}} è unico tra gli elementi HTML perché può assumere molte forme a seconda del valore dell'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type). È usato per creare la maggior parte dei tipi di widget di modulo, inclusi campi di testo su una sola linea, controlli di tempo e data, controlli senza input di testo come checkbox, radio button e picker di colore, e pulsanti.

### Campi di testo su una sola linea

Un campo di testo su una sola linea è creato usando un elemento {{HTMLElement("input")}} il cui valore dell'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) è impostato su `text`, o omettendo completamente l'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) (`text` è il valore predefinito). Il valore `text` per questo attributo è anche il valore di fallback se il valore specificato per l'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) è sconosciuto al browser (ad esempio se specifica `type="color"` e il browser non supporta i picker di colori nativi).

> [!NOTE]
> Può trovare esempi di tutti i tipi di campi di testo su una sola linea su GitHub a [single-line-text-fields.html](https://github.com/mdn/learning-area/blob/main/html/forms/native-form-widgets/single-line-text-fields.html) ([vedere anche in diretta](https://mdn.github.io/learning-area/html/forms/native-form-widgets/single-line-text-fields.html)).

Ecco un esempio di base di un campo di testo su una sola linea:

```html
<input type="text" id="comment" name="comment" value="I'm a text field" />
```

I campi di testo su una sola linea hanno solo una vera limitazione: se digita testo con interruzioni di linea, il browser rimuove queste interruzioni di linea prima di inviare i dati al server.

Lo screenshot seguente mostra un input di testo negli stati predefinito, focalizzato e disabilitato. La maggior parte dei browser indica lo stato focalizzato usando un anello di messa a fuoco intorno al controllo e lo stato disabilitato usando testo grigio o un controllo sbiadito/semi-trasparente.

![Screenshot degli stati predefinito, focalizzato e disabilitato dell'input di testo in Chrome su macOS](disabled.png)

Gli screenshot utilizzati in questo documento sono stati presi nel browser Chrome su macOS. Potrebbero esserci variazioni minori in questi campi/pulsanti tra diversi browser, ma la tecnica di evidenziazione di base rimane simile.

> [!NOTE]
> Discuteremo i valori per l'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) che impongono vincoli di validazione specifici, inclusi i tipi di input colore, email e url, nel prossimo articolo, [I tipi di input HTML5](/it/docs/Learn_web_development/Extensions/Forms/HTML5_input_types).

#### Campo Password

Uno dei tipi di input originali era il tipo di campo di testo `password`:

```html
<input type="password" id="pwd" name="pwd" />
```

Lo screenshot seguente mostra il campo di input Password in cui ogni carattere inserito è mostrato come un punto.

![Campo Password in chrome 115 su macOS](password.png)

Il valore `password` non aggiunge alcun vincolo speciale al testo inserito, ma oscura il valore inserito nel campo (ad es., con punti o asterischi) in modo che non possa essere facilmente letto da altri.

Tenga presente che questa è solo una funzione dell'interfaccia utente; a meno che il modulo non venga inviato in modo sicuro, sarà inviato in chiaro, il che è dannoso per la sicurezza — una parte malintenzionata potrebbe intercettare i dati e rubare password, dettagli delle carte di credito o qualsiasi altra cosa inviata. Il modo migliore per proteggere gli utenti da questo è ospitare qualsiasi pagina che coinvolga moduli su una connessione sicura (cioè, situata a un indirizzo `https://`), in modo che i dati siano crittografati prima dell'invio.

I browser riconoscono le implicazioni di sicurezza dell'invio di dati di modulo su una connessione insicura e hanno avvertimenti per scoraggiare gli utenti dall'usare moduli insicuri. Per maggiori informazioni su cosa implementa Firefox, vedi [Password non sicure](/it/docs/Web/Security/Insecure_passwords).

### Contenuto nascosto

Un altro controllo di testo originale è il tipo di input `hidden`. Questo viene utilizzato per creare un controllo di modulo invisibile all'utente, ma viene comunque inviato al server insieme al resto dei dati del modulo una volta inviato — ad esempio potrebbe voler inviare un timestamp al server che indica quando un ordine è stato effettuato. Poiché è nascosto, l'utente non può vederlo né modificarlo intenzionalmente, non riceverà mai il focus, e neppure un lettore di schermo lo noterà.

```html
<input type="hidden" id="timestamp" name="timestamp" value="1286705410" />
```

Se crea un tale elemento, è necessario impostare i suoi attributi `name` e `value`. Il valore può essere impostato dinamicamente tramite JavaScript. Il tipo di input `hidden` non dovrebbe avere un'etichetta associata.

Altri tipi di input di testo, come {{HTMLElement("input/search", "search")}}, {{HTMLElement("input/url", "url")}}, e {{HTMLElement("input/tel", "tel")}}, saranno trattati nel prossimo tutorial, [tipi di input HTML5](/it/docs/Learn_web_development/Extensions/Forms/HTML5_input_types).

## Elementi controllabili: checkbox e radio button

Gli elementi controllabili sono controlli il cui stato può essere modificato facendo clic su di essi o sulle etichette associate. Ci sono due tipi di elementi controllabili: la checkbox e il radio button. Entrambi utilizzano l'attributo [`checked`](/it/docs/Web/HTML/Reference/Elements/input/checkbox#checked) per indicare se il widget è selezionato di default o no.

Vale la pena notare che questi widget non si comportano esattamente come altri widget di modulo. Per la maggior parte dei widget di modulo, una volta che il modulo è inviato tutti i widget che hanno un attributo [`name`](/it/docs/Web/HTML/Reference/Elements/input#name) sono inviati, anche se non è stato compilato alcun valore. Nel caso di elementi controllabili, i loro valori sono inviati solo se sono selezionati. Se non sono selezionati, non viene inviato nulla, nemmeno il loro nome. Se sono selezionati ma non hanno valore, il nome viene inviato con un valore di _on_.

> [!NOTE]
> Può trovare gli esempi di questa sezione su GitHub come [checkable-items.html](https://github.com/mdn/learning-area/blob/main/html/forms/native-form-widgets/checkable-items.html) ([vedere anche in diretta](https://mdn.github.io/learning-area/html/forms/native-form-widgets/checkable-items.html)).

Per la massima usabilità/accessibilità, si consiglia di circondare ogni elenco di elementi correlati in un {{htmlelement("fieldset")}}, con una {{htmlelement("legend")}} che fornisca una descrizione generale dell'elenco. Ogni coppia individuale di elementi {{htmlelement("label")}}/{{htmlelement("input")}} dovrebbe essere contenuta nel proprio elemento di elenco (o simile). L'etichetta associata è generalmente posizionata immediatamente prima o dopo il radio button o la checkbox, con le istruzioni per il gruppo di radio button o checkbox che sono generalmente il contenuto della {{htmlelement("legend")}}. Vedere gli esempi sopra linkati per esempi strutturali.

### Checkbox

Una checkbox è creata usando l'elemento {{HTMLElement("input")}} con un attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) impostato sul valore {{HTMLElement("input/checkbox", "checkbox")}}.

```html
<input type="checkbox" id="questionOne" name="subscribe" value="yes" checked />
```

Gli elementi checkbox correlati dovrebbero usare lo stesso attributo [`name`](/it/docs/Web/HTML/Reference/Elements/input#name). Includere l'attributo [`checked`](/it/docs/Web/HTML/Reference/Elements/input/checkbox#checked) fa sì che la checkbox sia selezionata automaticamente quando la pagina viene caricata. Facendo clic sulla checkbox o sulla sua etichetta associata si alterna la selezione della checkbox.

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

Lo screenshot seguente mostra checkbox negli stati predefinito, focalizzato e disabilitato. Le checkbox negli stati predefinito e disabilitato appaiono selezionate, mentre nello stato focalizzato, la checkbox è deselezionata, con un anello di messa a fuoco intorno ad essa.

![Checkbox negli stati predefinito, focalizzato e disabilitato in chrome 115 su macOS](checkboxes.png)

> [!NOTE]
> Qualsiasi checkbox e radio button con l'attributo [`checked`](/it/docs/Web/HTML/Reference/Elements/input/checkbox#checked) al caricamento corrispondono alla pseudo-classe {{cssxref(':default')}}, anche se non sono più selezionati. Qualsiasi attualmente selezionato corrisponde alla pseudo-classe {{cssxref(':checked')}}.

A causa della natura on-off delle checkbox, la checkbox è considerata un pulsante di commutazione, con molti sviluppatori e designer che espandono lo stile predefinito delle checkbox per creare pulsanti che sembrano interruttori di commutazione. Può [vedere un esempio in azione qui](https://mdn.github.io/learning-area/html/forms/toggle-switch-example/) (vedere anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/forms/toggle-switch-example/index.html)).

### Radio button

Un radio button è creato usando l'elemento {{HTMLElement("input")}} con il suo attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) impostato sul valore `radio`.

```html
<input type="radio" id="soup" name="meal" value="soup" checked />
```

Diversi radio button possono essere collegati fra loro. Se condividono lo stesso valore per il loro attributo [`name`](/it/docs/Web/HTML/Reference/Elements/input#name), saranno considerati nello stesso gruppo di pulsanti. Solo un pulsante in un dato gruppo può essere selezionato alla volta; questo significa che quando uno di essi è selezionato, tutti gli altri vengono automaticamente deselezionati. Quando il modulo è inviato, solo il valore del radio button selezionato è inviato. Se nessuno di essi è selezionato, l'intero gruppo di radio button è considerato in uno stato sconosciuto e nessun valore è inviato con il modulo. Una volta che uno dei radio button in un gruppo con lo stesso nome è selezionato, non è possibile per l'utente deselezionare tutti i pulsanti senza ripristinare il modulo.

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

Lo screenshot seguente mostra radio button predefiniti e disabilitati nello stato selezionato, insieme a un radio button focalizzato nello stato deselezionato.

![Radio button predefiniti, focalizzati e disabilitati in chrome 115 su macOS](radios.png)

## Pulsanti effettivi

Il radio button non è in realtà un pulsante, nonostante il suo nome; passiamo a esaminare i pulsanti effettivi! Ci sono tre tipi di input che producono pulsanti:

- `submit`
  - : Invia i dati del modulo al server. Per gli elementi {{HTMLElement("button")}}, omettere l'attributo `type` (o un valore non valido di `type`) risulta in un pulsante di invio.
- `reset`
  - : Ripristina tutti i widget del modulo ai loro valori predefiniti.
- `button`
  - : Pulsanti che non hanno effetti automatici ma possono essere personalizzati usando codice JavaScript.

Abbiamo anche l'elemento {{htmlelement("button")}} stesso. Questo può assumere un attributo `type` con valore `submit`, `reset`, o `button` per imitare il comportamento dei tre tipi di `<input>` menzionati sopra. La principale differenza tra i due è che gli elementi `<button>` effettivi sono molto più facili da stilizzare.

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
> Anche il tipo di input `image` si rende come un pulsante. Ne parleremo più avanti.

> [!NOTE]
> Può trovare gli esempi di questa sezione su GitHub come [button-examples.html](https://github.com/mdn/learning-area/blob/main/html/forms/native-form-widgets/button-examples.html) ([vedere anche in diretta](https://mdn.github.io/learning-area/html/forms/native-form-widgets/button-examples.html)).

Di seguito può trovare esempi di ciascun tipo di pulsante `<input>`, insieme al tipo `<button>` equivalente.

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

### anonymous

```html
<button type="button">This is an <strong>anonymous button</strong></button>

<input type="button" value="This is an anonymous button" />
```

I pulsanti si comportano sempre allo stesso modo indipendentemente dal fatto che usi un elemento {{HTMLElement("button")}} o un elemento {{HTMLElement("input")}}. Come può vedere dagli esempi, tuttavia, gli elementi {{HTMLElement("button")}} permettono di usare l'HTML nel loro contenuto, che è inserito tra i tag `<button>` di apertura e chiusura. Gli elementi {{HTMLElement("input")}} d'altra parte sono {{Glossary("void_element", "elementi vuoti")}}; il loro contenuto visualizzato è inserito all'interno dell'attributo `value`, e quindi accetta solo testo semplice come contenuto.

Lo screenshot seguente mostra un pulsante negli stati predefinito, focalizzato e disabilitato. Nello stato focalizzato, c'è un anello di messa a fuoco intorno al pulsante e nello stato disabilitato, il pulsante è grigio.

![Stati di pulsante predefinito, focalizzato e disabilitato in chrome 115 su macOS](buttons.png)

### Pulsante immagine

Il controllo **pulsante immagine** è reso esattamente come un elemento {{HTMLElement("img")}}, tranne che quando l'utente fa clic su di esso, si comporta come un pulsante di invio.

Un pulsante immagine è creato usando un elemento {{HTMLElement("input")}} con il suo attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) impostato sul valore `image`. Questo elemento supporta esattamente lo stesso set di attributi dell'elemento {{HTMLElement("img")}}, più tutti gli attributi supportati da altri pulsanti di modulo.

```html
<input type="image" alt="Click me!" src="my-img.png" width="80" height="30" />
```

Se il pulsante immagine è usato per inviare il modulo, questo controllo non invia il suo valore — invece, le coordinate X e Y del clic sull'immagine sono inviate (le coordinate sono relative all'immagine, il che significa che l'angolo superiore sinistro dell'immagine rappresenta la coordinata (0, 0)). Le coordinate sono inviate come due coppie chiave/valore:

- La chiave del valore X è il valore dell'attributo [`name`](/it/docs/Web/HTML/Reference/Elements/input#name) seguito dalla stringa "_.x_".
- La chiave del valore Y è il valore dell'attributo [`name`](/it/docs/Web/HTML/Reference/Elements/input#name) seguito dalla stringa "_.y_".

Quindi, ad esempio, quando fa clic sull'immagine alla coordinata (123, 456) e viene inviato tramite il metodo `get`, vedrà i valori aggiunti all'URL come segue:

```url
http://foo.com?pos.x=123&pos.y=456
```

Questo è un modo molto conveniente per costruire una "mappa attiva". Come questi valori vengono inviati e recuperati è descritto nell'articolo [Inviando dati di modulo](/it/docs/Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data).

## Selettore di file

C'è un ultimo tipo `<input>` che ci è pervenuto nei primi giorni dell'HTML: il tipo di input file. I moduli sono in grado di inviare file a un server (questa azione specifica è anche dettagliata nell'articolo [Inviando dati di modulo](/it/docs/Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data)). Il widget selettore di file può essere usato per scegliere uno o più file da inviare.

Per creare un [widget selettore di file](/it/docs/Web/HTML/Reference/Elements/input/file), usa l'elemento {{HTMLElement("input")}} con il suo attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) impostato su `file`. I tipi di file che sono accettati possono essere limitati usando l'attributo [`accept`](/it/docs/Web/HTML/Reference/Elements/input#accept). Inoltre, se desidera permettere all'utente di selezionare più di un file, può farlo aggiungendo l'attributo [`multiple`](/it/docs/Web/HTML/Reference/Elements/input#multiple).

### Esempio

In questo esempio, viene creato un selettore di file che richiede file immagine grafici. In questo caso, all'utente è permesso di selezionare più file.

```html
<input type="file" name="file" id="file" accept="image/*" multiple />
```

Su alcuni dispositivi mobili, il selettore di file può accedere a foto, video e audio catturati direttamente dalla fotocamera e dal microfono del dispositivo aggiungendo informazioni di cattura all'attributo `accept` in questo modo:

```html
<input type="file" accept="image/*;capture=camera" />
<input type="file" accept="video/*;capture=camcorder" />
<input type="file" accept="audio/*;capture=microphone" />
```

Lo screenshot seguente mostra il widget selettore di file negli stati predefiniti, focalizzati e disabilitati quando nessun file è selezionato.

![Widget selettore di file negli stati predefiniti, focalizzati e disabilitati in chrome 115 su macOS](filepickers.png)

## Attributi comuni

Molti degli elementi utilizzati per definire i controlli di modulo hanno alcuni degli attributi specifici. Tuttavia, c'è un set di attributi comuni a tutti gli elementi del modulo. Ha già incontrato alcuni di questi, ma di seguito è riportato un elenco di quegli attributi comuni, per il suo riferimento:

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
        se non c'è un elemento contenitore con l'attributo <code>disabled</code> impostato, allora l'elemento è abilitato.
      </td>
    </tr>
    <tr>
      <td>
        <code><a href="/it/docs/Web/HTML/Reference/Elements/input#form">form</a></code>
      </td>
      <td></td>
      <td>
        L'elemento <code>&#x3C;form></code> a cui il widget è associato, usato se non è annidato all'interno di quel modulo.
        Il valore dell'attributo deve essere l'attributo <code>id</code> di un elemento {{HTMLElement("form")}} nello stesso documento.
        Questo consente di associare un controllo di modulo con un modulo al di fuori del quale è, anche se è all'interno di un altro elemento modulo.
      </td>
    </tr>
    <tr>
      <td>
        <code><a href="/it/docs/Web/HTML/Reference/Elements/input#name">name</a></code>
      </td>
      <td></td>
      <td>Il nome dell'elemento; questo è inviato con i dati del modulo.</td>
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

## Metti alla prova le sue competenze!

Ha raggiunto la fine di questo articolo, ma è in grado di ricordare le informazioni più importanti? Può trovare ulteriori test per verificare di aver mantenuto queste informazioni prima di andare avanti — vedere [Metti alla prova le tue competenze: Controlli di base](/it/docs/Learn_web_development/Extensions/Forms/Test_your_skills/Basic_controls).

## Riassunto

Questo articolo ha coperto i tipi di input più vecchi — il set originale introdotto nei primi giorni dell'HTML che è ben supportato in tutti i browser. Nella sezione successiva, esamineremo i valori più moderni dell'attributo `type`.

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/How_to_structure_a_web_form", "Learn_web_development/Extensions/Forms/HTML5_input_types", "Learn_web_development/Extensions/Forms")}}
