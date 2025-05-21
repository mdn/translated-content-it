---
title: Controlli di base nativi del modulo
slug: Learn_web_development/Extensions/Forms/Basic_native_form_controls
l10n:
  sourceCommit: c05ef6211441aedb359d4020518ac152aa92db9e
---

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/How_to_structure_a_web_form", "Learn_web_development/Extensions/Forms/HTML5_input_types", "Learn_web_development/Extensions/Forms")}}

Nell'[articolo precedente](/it/docs/Learn_web_development/Extensions/Forms/How_to_structure_a_web_form), abbiamo creato un esempio di modulo web funzionale, introducendo alcuni controlli del modulo ed elementi strutturali comuni, concentrandoci sulle migliori pratiche di accessibilità. Successivamente, esamineremo in dettaglio la funzionalità dei diversi controlli del modulo, o widget, studiando tutte le diverse opzioni disponibili per raccogliere diversi tipi di dati. In questo articolo specifico, esamineremo il set originale di controlli del modulo, disponibile in tutti i browser sin dai primi giorni del web.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Una conoscenza di base
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >dell'HTML</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere in dettaglio il set originale di widget nativi del modulo
        disponibili nei browser per raccogliere dati, e come implementarli
        usando HTML.
      </td>
    </tr>
  </tbody>
</table>

Hai già incontrato alcuni elementi del modulo, inclusi {{HTMLelement('form')}}, {{HTMLelement('fieldset')}}, {{HTMLelement('legend')}}, {{HTMLelement('textarea')}}, {{HTMLelement('label')}}, {{HTMLelement('button')}} e {{HTMLelement('input')}}. Questo articolo copre:

- I tipi di input comuni {{HTMLelement('input/button', 'button')}}, {{HTMLelement('input/checkbox', 'checkbox')}}, {{HTMLelement('input/file', 'file')}}, {{HTMLelement('input/hidden', 'hidden')}}, {{HTMLelement('input/image', 'image')}}, {{HTMLelement('input/password', 'password')}}, {{HTMLelement('input/radio', 'radio')}}, {{HTMLelement('input/reset', 'reset')}}, {{HTMLelement('input/submit', 'submit')}}, e {{HTMLelement('input/text', 'text')}}.
- Alcuni degli attributi comuni a tutti i controlli del modulo.

> [!NOTE]
> Tratteremo controlli di modulo aggiuntivi e più potenti nei prossimi due articoli. Se desideri una referenza più avanzata, dovresti consultare la nostra [referenza degli elementi dei moduli HTML](/it/docs/Web/HTML/Reference/Elements#forms), e in particolare la nostra estensiva [referenza sui tipi di `<input>`](/it/docs/Web/HTML/Reference/Elements/input).

## Campi di input testo

I campi di testo {{htmlelement("input")}} sono i widget di modulo più basilari. Sono un modo molto conveniente per consentire all'utente di inserire qualsiasi tipo di dato, e abbiamo già visto alcuni semplici esempi.

> [!NOTE]
> I campi di testo dei moduli HTML sono controlli di input di testo semplice. Questo significa che non puoi usarli per eseguire l'editing di testo ricco (grassetto, corsivo, ecc.). Tutti gli editor di testo ricco che incontrerai sono widget personalizzati creati con HTML, CSS, e JavaScript.

Tutti i controlli di testo di base condividono alcuni comportamenti comuni:

- Possono essere contrassegnati come [`readonly`](/it/docs/Web/HTML/Reference/Elements/input#readonly) (l'utente non può modificare il valore dell'input ma viene comunque inviato con il resto dei dati del modulo) o [`disabled`](/it/docs/Web/HTML/Reference/Elements/input#disabled) (il valore dell'input non può essere modificato e non viene mai inviato con il resto dei dati del modulo).
- Possono avere un [`placeholder`](/it/docs/Web/HTML/Reference/Elements/input#placeholder); questo è il testo che appare all'interno della casella di input di testo che dovrebbe essere usato per descrivere brevemente lo scopo della casella.
- Possono essere limitati nella [`size`](/it/docs/Web/HTML/Reference/Attributes/size) (la dimensione fisica della casella) e [`maxlength`](/it/docs/Web/HTML/Reference/Attributes/maxlength) (il numero massimo di caratteri che possono essere inseriti nella casella).
- Possono beneficiare del controllo ortografico (usando l'attributo [`spellcheck`](/it/docs/Web/HTML/Reference/Global_attributes/spellcheck)).

> [!NOTE]
> L'elemento {{htmlelement("input")}} è unico tra gli elementi HTML perché può assumere molte forme a seconda del valore dell'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) che possiede. È usato per creare la maggior parte dei tipi di widget del modulo inclusi i campi di testo a linea singola, i controlli di tempo e data, i controlli senza input di testo come le caselle di spunta, i bottoni di scelta, i selettori di colore, e i bottoni.

### Campi di testo a linea singola

Un campo di testo a linea singola è creato usando un elemento {{HTMLElement("input")}} il cui valore dell'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) è impostato su `text`, o omettendo completamente l'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) (`text` è il valore predefinito). Il valore `text` per questo attributo è anche il valore di default se il valore specificato per l'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) è sconosciuto dal browser (ad esempio se si specifica `type="color"` e il browser non supporta i selettori di colore nativi).

> [!NOTE]
> Puoi trovare esempi di tutti i tipi di campo di testo a linea singola su GitHub in [single-line-text-fields.html](https://github.com/mdn/learning-area/blob/main/html/forms/native-form-widgets/single-line-text-fields.html) ([vedi anche il live](https://mdn.github.io/learning-area/html/forms/native-form-widgets/single-line-text-fields.html)).

Ecco un esempio basilare di un campo di testo a linea singola:

```html
<input type="text" id="comment" name="comment" value="I'm a text field" />
```

I campi di testo a linea singola hanno solo una vera limitazione: se scrivi un testo con interruzioni di riga, il browser rimuove queste interruzioni prima di inviare i dati al server.

Lo screenshot qui sotto mostra un input di testo negli stati di default, focalizzato e disabilitato. La maggior parte dei browser indica lo stato di focalizzazione usando un bordo di messa a fuoco attorno al controllo e lo stato di disabilitazione usando testo grigio o un controllo sbiadito/semitrasparente.

![Screenshot dell'input di testo negli stati di default, focalizzato e disabilitato in Chrome su macOS](disabled.png)

Gli screenshot utilizzati in questo documento sono stati presi nel browser Chrome su macOS. Ci possono essere variazioni minori in questi campi/bottoni tra diversi browser, ma la tecnica di evidenziazione di base rimane simile.

> [!NOTE]
> Discutiamo dei valori per l'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) che impongono vincoli di validazione specifici inclusi i tipi di input di colore, email e url, nel prossimo articolo, [I tipi di input HTML5](/it/docs/Learn_web_development/Extensions/Forms/HTML5_input_types).

#### Campo password

Uno dei tipi di input originali era il tipo di campo testo `password`:

```html
<input type="password" id="pwd" name="pwd" />
```

Lo screenshot seguente mostra il campo di input Password in cui ciascun carattere immesso è mostrato come un punto.

![Campo password in chrome 115 su macOS](password.png)

Il valore `password` non aggiunge alcun vincolo speciale al testo inserito, ma oscura il valore inserito nel campo (ad esempio con punti o asterischi) in modo che non possa essere facilmente letto da altri.

Tieni presente che questo è solo un elemento dell'interfaccia utente; a meno che tu non invii il tuo modulo in modo sicuro, verrà inviato in testo semplice, il che è dannoso per la sicurezza — una parte malintenzionata potrebbe intercettare i tuoi dati e rubare password, dettagli delle carte di credito, o qualsiasi altra cosa tu abbia inviato. Il modo migliore per proteggere gli utenti da questo è ospitare qualsiasi pagina che comporta moduli su una connessione sicura (cioè situata a un indirizzo `https://`), in modo che i dati siano crittografati prima di essere inviati.

I browser riconoscono le implicazioni di sicurezza dell'invio di dati del modulo su una connessione non sicura e hanno avvisi per scoraggiare gli utenti dall'usare moduli non sicuri. Per ulteriori informazioni su ciò che Firefox implementa, vedere [Password non sicure](/it/docs/Web/Security/Insecure_passwords).

### Contenuto nascosto

Un altro controllo di testo originale è il tipo di input `hidden`. Questo viene usato per creare un controllo del modulo invisibile all'utente, ma che viene comunque inviato al server insieme al resto dei dati del modulo una volta inviato — ad esempio potresti voler inviare un timestamp al server indicando quando è stato effettuato un ordine. Poiché è nascosto, l'utente non può vederlo né modificarlo intenzionalmente, non riceverà mai la focalizzazione e un lettore di schermo non lo noterà nemmeno.

```html
<input type="hidden" id="timestamp" name="timestamp" value="1286705410" />
```

Se si crea un tale elemento, è necessario impostare gli attributi `name` e `value`. Il valore può essere impostato dinamicamente tramite JavaScript. Il tipo di input `hidden` non dovrebbe avere un'etichetta associata.

Altri tipi di input di testo, come {{HTMLElement("input/search", "search")}}, {{HTMLElement("input/url", "url")}}, e {{HTMLElement("input/tel", "tel")}}, verranno trattati nel prossimo tutorial, [Tipi di input HTML5](/it/docs/Learn_web_development/Extensions/Forms/HTML5_input_types).

## Elementi selezionabili: caselle di controllo e pulsanti di scelta

Gli elementi selezionabili sono controlli il cui stato può essere modificato facendo clic su di essi o sulle loro etichette associate. Ci sono due tipi di elementi selezionabili: la casella di controllo e il pulsante di scelta. Entrambi utilizzano l'attributo [`checked`](/it/docs/Web/HTML/Reference/Elements/input/checkbox#checked) per indicare se il widget è selezionato di default o meno.

Vale la pena notare che questi widget non si comportano esattamente come altri widget del modulo. Per la maggior parte dei widget del modulo, una volta che il modulo è stato inviato, tutti i widget che hanno un attributo [`name`](/it/docs/Web/HTML/Reference/Elements/input#name) vengono inviati, anche se nessun valore è stato compilato. Nel caso degli elementi selezionabili, i loro valori vengono inviati solo se sono selezionati. Se non sono selezionati, non viene inviato nulla, nemmeno il loro nome. Se sono selezionati ma non hanno valore, il nome viene inviato con un valore di _on_.

> [!NOTE]
> Puoi trovare gli esempi di questa sezione su GitHub come [checkable-items.html](https://github.com/mdn/learning-area/blob/main/html/forms/native-form-widgets/checkable-items.html) ([vedi anche il live](https://mdn.github.io/learning-area/html/forms/native-form-widgets/checkable-items.html)).

Per massima usabilità/accessibilità, si consiglia di circondare ciascun elenco di elementi correlati in un {{htmlelement("fieldset")}}, con un {{htmlelement("legend")}} che fornisce una descrizione generale dell'elenco. Ciascuna coppia individuale di elementi {{htmlelement("label")}}/{{htmlelement("input")}} dovrebbe essere contenuta nel proprio elemento di elenco (o simile). L'etichetta associata {{htmlelement('label')}} è generalmente posizionata immediatamente prima o dopo il pulsante di scelta o la casella di controllo, con le istruzioni per il gruppo di pulsanti di scelta o caselle di controllo che sono generalmente il contenuto del {{htmlelement("legend")}}. Vedi gli esempi collegati sopra per esempi strutturali.

### Casella di controllo

Una casella di controllo è creata usando l'elemento {{HTMLElement("input")}} con un attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) impostato sul valore {{HTMLElement("input/checkbox", "checkbox")}}.

```html
<input type="checkbox" id="questionOne" name="subscribe" value="yes" checked />
```

Gli elementi correlati della casella di controllo dovrebbero usare lo stesso attributo [`name`](/it/docs/Web/HTML/Reference/Elements/input#name). Includere l'attributo [`checked`](/it/docs/Web/HTML/Reference/Elements/input/checkbox#checked) rende la casella di controllo selezionata automaticamente quando la pagina viene caricata. Facendo clic sulla casella di controllo o sull'etichetta associata, la casella di controllo viene alternata acceso e spento.

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

Lo screenshot seguente mostra le caselle di controllo negli stati di default, focalizzato e disabilitato. Le caselle di controllo negli stati di default e disabilitato appaiono selezionate, mentre nello stato focalizzato la casella di controllo è deselezionata, con un bordo di messa a fuoco attorno.

![Casi di default, focalizzati e disabilitati delle caselle di controllo in chrome 115 su macOS](checkboxes.png)

> [!NOTE]
> Qualsiasi casella di controllo e pulsante di scelta con l'attributo [`checked`](/it/docs/Web/HTML/Reference/Elements/input/checkbox#checked) al caricamento corrisponde alla pseudo-classe {{cssxref(':default')}}, anche se non sono più selezionati. Qualsiasi selezionato corrisponde alla pseudo-classe {{cssxref(':checked')}}.

Data la natura on-off delle caselle di controllo, la casella di controllo è considerata un pulsante di commutazione, con molti sviluppatori e designer che espandono lo stile di default della casella di controllo per creare bottoni che assomigliano a interruttori a levetta. Puoi [vedere un esempio in azione qui](https://mdn.github.io/learning-area/html/forms/toggle-switch-example/) (vedi anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/forms/toggle-switch-example/index.html)).

### Pulsante di scelta

Un pulsante di scelta è creato usando l'elemento {{HTMLElement("input")}} con il suo attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) impostato sul valore `radio`:

```html
<input type="radio" id="soup" name="meal" value="soup" checked />
```

Diversi pulsanti di scelta possono essere collegati insieme. Se condividono lo stesso valore per il loro attributo [`name`](/it/docs/Web/HTML/Reference/Elements/input#name), saranno considerati come facenti parte dello stesso gruppo di pulsanti. Solo un pulsante in un determinato gruppo può essere selezionato alla volta; questo significa che quando uno di essi è selezionato, tutti gli altri vengono automaticamente deselezionati. Quando il modulo viene inviato, solo il valore del pulsante di scelta selezionato viene inviato. Se nessuno di essi è selezionato, l'intero gruppo di pulsanti di scelta è considerato in uno stato sconosciuto e nessun valore viene inviato con il modulo. Una volta che uno dei pulsanti di scelta in un gruppo con lo stesso nome è selezionato, non è possibile per l'utente deselezionare tutti i pulsanti senza resettare il modulo.

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

Lo screenshot seguente mostra i pulsanti di scelta predefiniti e disabilitati nello stato selezionato, insieme a un pulsante di scelta focalizzato nello stato non selezionato.

![Stati predefiniti, focalizzati e disabilitati dei pulsanti di scelta in chrome 115 su macOS](radios.png)

## Bottoni effettivi

Il pulsante di scelta in realtà non è un pulsante, nonostante il suo nome; passiamo oltre e diamo un'occhiata ai bottoni effettivi! Ci sono tre tipi di input che producono bottoni:

- `submit`
  - : Invia i dati del modulo al server. Per gli elementi {{HTMLElement("button")}}, omettere l'attributo `type` (o un valore non valido di `type`) risulta in un bottone di invio.
- `reset`
  - : Reimposta tutti i widget del modulo ai loro valori predefiniti.
- `button`
  - : Bottoni che non hanno effetto automatico ma possono essere personalizzati usando il codice JavaScript.

Poi abbiamo anche l'elemento {{htmlelement("button")}} stesso. Questo può prendere un attributo `type` del valore `submit`, `reset`, o `button` per imitare il comportamento dei tre tipi `<input>` menzionati sopra. La principale differenza tra i due è che gli elementi `<button>` effettivi sono molto più facili da stilizzare.

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
> Anche il tipo di input `image` si rappresenta come un bottone. Lo tratteremo anche più avanti.

> [!NOTE]
> Puoi trovare gli esempi di questa sezione su GitHub come [button-examples.html](https://github.com/mdn/learning-area/blob/main/html/forms/native-form-widgets/button-examples.html) ([vedi anche il live](https://mdn.github.io/learning-area/html/forms/native-form-widgets/button-examples.html)).

Sotto puoi trovare esempi di ciascun tipo di bottone `<input>`, insieme al tipo `<button>` equivalente.

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

### Anonymous

```html
<button type="button">This is an <strong>anonymous button</strong></button>

<input type="button" value="This is an anonymous button" />
```

I bottoni si comportano sempre allo stesso modo sia che tu usi un elemento {{HTMLElement("button")}} o un elemento {{HTMLElement("input")}}. Tuttavia, come puoi vedere dagli esempi, gli elementi {{HTMLElement("button")}} ti permettono di usare HTML nel loro contenuto, che è inserito tra i tag di apertura e chiusura `<button>`. Gli elementi {{HTMLElement("input")}}, d'altra parte, sono {{Glossary("void_element", "elementi vuoti")}}; il loro contenuto visualizzato è inserito nell'attributo `value`, e quindi accetta solo testo semplice come contenuto.

Lo screenshot seguente mostra un bottone negli stati predefiniti, focalizzati e disabilitati. Nel stato focalizzato, c'è un bordo di messa a fuoco attorno al bottone, e nello stato disabilitato, il bottone è grigio.

![Stati di default, focalizzato e disabilitato del bottone in chrome 115 su macOS](buttons.png)

### Bottone immagine

Il controllo **bottone immagine** viene rappresentato esattamente come un elemento {{HTMLElement("img")}}, eccetto che quando l'utente ci clicca sopra, si comporta come un bottone di invio.

Un bottone immagine è creato usando un elemento {{HTMLElement("input")}} con il suo attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) impostato sul valore `image`. Questo elemento supporta esattamente lo stesso set di attributi dell'elemento {{HTMLElement("img")}}, più tutti gli attributi supportati dagli altri bottoni del modulo.

```html
<input type="image" alt="Click me!" src="my-img.png" width="80" height="30" />
```

Se il bottone immagine è usato per inviare il modulo, questo controllo non invia il suo valore — invece, vengono inviate le coordinate X e Y del clic sull'immagine (le coordinate sono relative all'immagine, il che significa che l'angolo superiore sinistro dell'immagine rappresenta la coordinata (0, 0)). Le coordinate sono inviate come due coppie chiave/valore:

- La chiave del valore X è il valore dell'attributo [`name`](/it/docs/Web/HTML/Reference/Elements/input#name) seguito dalla stringa "_.x_".
- La chiave del valore Y è il valore dell'attributo [`name`](/it/docs/Web/HTML/Reference/Elements/input#name) seguito dalla stringa "_.y_".

Quindi, ad esempio, quando fai clic sull'immagine alla coordinata (123, 456) e questa invia tramite il metodo `get`, vedrai i valori aggiunti all'URL come segue:

```url
http://foo.com?pos.x=123&pos.y=456
```

Questo è un modo molto conveniente per costruire una "mappa attiva". Come questi valori vengono inviati e recuperati è dettagliato nell'articolo [Invio dei dati del modulo](/it/docs/Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data).

## Selettore di file

C'è un ultimo tipo `<input>` che ci è stato trasmesso nei primi HTML: il tipo di input file. I moduli sono in grado di inviare file a un server (questa azione specifica è anche dettagliata nell'articolo [Invio e recupero dei dati del modulo](/it/docs/Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data)). Il widget selettore di file può essere usato per scegliere uno o più file da inviare.

Per creare un [widget selettore di file](/it/docs/Web/HTML/Reference/Elements/input/file), usi l'elemento {{HTMLElement("input")}} con il suo attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) impostato su `file`. I tipi di file accettati possono essere limitati usando l'attributo [`accept`](/it/docs/Web/HTML/Reference/Elements/input#accept). Inoltre, se desideri consentire all'utente di scegliere più di un file, puoi farlo aggiungendo l'attributo [`multiple`](/it/docs/Web/HTML/Reference/Elements/input#multiple).

### Esempio

In questo esempio, viene creato un selettore di file che richiede file di immagini grafiche. In questo caso, l'utente è autorizzato a selezionare più file.

```html
<input type="file" name="file" id="file" accept="image/*" multiple />
```

Su alcuni dispositivi mobili, il selettore di file può accedere a foto, video e audio catturati direttamente dalla fotocamera e dal microfono del dispositivo aggiungendo informazioni di cattura all'attributo `accept` in questo modo:

```html
<input type="file" accept="image/*;capture=camera" />
<input type="file" accept="video/*;capture=camcorder" />
<input type="file" accept="audio/*;capture=microphone" />
```

Lo screenshot seguente mostra il widget selettore di file negli stati di default, focalizzato e disabilitato quando non è selezionato alcun file.

![Widget selettore di file negli stati di default, focalizzato e disabilitato in chrome 115 su macOS](filepickers.png)

## Attributi comuni

Molti degli elementi utilizzati per definire i controlli del modulo hanno alcuni loro specifici attributi. Tuttavia, c'è un insieme di attributi comune a tutti gli elementi del modulo. Hai già incontrato alcuni di questi, ma sotto c'è un elenco di quegli attributi comuni per riferimento:

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
        Questo attributo Booleano ti consente di specificare che l'elemento dovrebbe avere automaticamente il focus di input quando la pagina si carica.
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
        Se questo attributo non è specificato, l'elemento eredita la sua impostazione dall'elemento contenitore, ad esempio, {{HTMLElement("fieldset")}};
        se non c'è nessun elemento contenitore con l'attributo <code>disabled</code> impostato, allora l'elemento è abilitato.
      </td>
    </tr>
    <tr>
      <td>
        <code><a href="/it/docs/Web/HTML/Reference/Elements/input#form">form</a></code>
      </td>
      <td></td>
      <td>
        L'elemento <code>&#x3C;form></code> al quale è associato il widget, utilizzato se non è annidato all'interno di quel form.
        Il valore dell'attributo deve essere l'attributo <code>id</code> di un elemento {{HTMLElement("form")}} nel medesimo documento.
        Questo ti permette di associare un controllo di un modulo con un modulo al di fuori del suo contenitore, anche se è dentro un altro elemento form.
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

## Metti alla prova le tue abilità!

Hai raggiunto la fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare alcuni altri test per verificare che tu abbia trattenuto queste informazioni prima di andare avanti — vedi [Metti alla prova le tue abilità: Controlli di base](/it/docs/Learn_web_development/Extensions/Forms/Test_your_skills/Basic_controls).

## Sintesi

Questo articolo ha trattato i tipi di input più vecchi — il set originale introdotto nei primi giorni dell'HTML che è ben supportato in tutti i browser. Nella prossima sezione, daremo uno sguardo ai valori più moderni dell'attributo `type`.

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/How_to_structure_a_web_form", "Learn_web_development/Extensions/Forms/HTML5_input_types", "Learn_web_development/Extensions/Forms")}}
