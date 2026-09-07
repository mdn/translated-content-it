---
title: I tipi di input HTML5
slug: Learn_web_development/Extensions/Forms/HTML5_input_types
l10n:
  sourceCommit: 2066cc916dfdcbb782340bf0ce562b230e947cba
---

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/Basic_native_form_controls", "Learn_web_development/Extensions/Forms/Other_form_controls", "Learn_web_development/Extensions/Forms")}}

Nell'[articolo precedente](/it/docs/Learn_web_development/Extensions/Forms/Basic_native_form_controls) è stato esaminato l'elemento {{htmlelement("input")}}, trattando i valori originali dell'attributo `type` disponibili fin dai primi tempi di HTML. Ora verranno analizzate nel dettaglio le funzionalità di alcuni tipi di input aggiunti successivamente.

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
        Comprendere i valori più recenti dei tipi di input disponibili per creare controlli di modulo nativi e come implementarli usando HTML.
      </td>
    </tr>
  </tbody>
</table>

Poiché l'aspetto dei controlli dei moduli HTML può essere molto diverso dalle specifiche di un designer, gli sviluppatori web a volte creano controlli di modulo personalizzati. Questo argomento viene trattato in un tutorial avanzato: [Come creare widget di modulo personalizzati](/it/docs/Learn_web_development/Extensions/Forms/How_to_build_custom_form_controls).

## Campo indirizzo email

Questo tipo di campo viene impostato usando il valore `email` per l'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type):

```html hidden live-sample___email
<label for="email">Enter your email address:</label><br />
```

```html live-sample___email
<input type="email" id="email" name="email" />
```

{{EmbedLiveSample('email','100%','50')}}

Quando viene usato questo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type), il valore deve essere un indirizzo email per essere valido. Qualsiasi altro contenuto fa sì che il browser visualizzi un errore quando il modulo viene inviato. È possibile vederlo in azione nello screenshot seguente.

![Un input email non valido che mostra il messaggio "Please enter an email address."](email_address_invalid.png)

È possibile usare l'attributo [`multiple`](/it/docs/Web/HTML/Reference/Attributes/multiple) in combinazione con il tipo di input `email` per consentire l'inserimento di diversi indirizzi email separati da virgole nello stesso input:

```html
<input type="email" id="email" name="email" multiple />
```

Su alcuni dispositivi, in particolare dispositivi touch con tastiere dinamiche come gli smartphone, può essere presentato un tastierino virtuale diverso, più adatto all'inserimento di indirizzi email, che include il tasto `@`:

![Tastiera email di Firefox per Android, con il simbolo chiocciola visualizzato per impostazione predefinita.](fx-android-email-type-keyboard.jpg)

Questo è un altro valido motivo per usare questi tipi di input più recenti, migliorando l'esperienza utente per chi usa questi dispositivi.

### Validazione lato client

`email`, insieme ad altri tipi `input` più recenti, fornisce una validazione degli errori integrata _lato client_, eseguita dal browser prima che i dati vengano inviati al server. È un utile aiuto per guidare gli utenti nella compilazione corretta di un modulo e può far risparmiare tempo: è utile sapere immediatamente che i dati non sono corretti, invece di dover attendere un ciclo completo fino al server.

Tuttavia, _non deve essere considerata_ una misura di sicurezza esaustiva. Le applicazioni devono sempre eseguire controlli di sicurezza su tutti i dati inviati tramite modulo sul _lato server_, oltre che sul lato client, perché la validazione lato client è troppo facile da disattivare; pertanto, utenti malintenzionati possono comunque inviare facilmente dati errati al server. Leggere [Sicurezza dei siti web](/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Website_security) per avere un'idea di ciò che _potrebbe_ accadere; l'implementazione della validazione lato server va in parte oltre lo scopo di questo modulo, ma è opportuno tenerla presente.

Si noti che `a@b` è un indirizzo email valido secondo i vincoli predefiniti forniti. Questo accade perché il tipo di input `email` consente per impostazione predefinita gli indirizzi email intranet. Per implementare un comportamento di validazione diverso, è possibile usare l'attributo [`pattern`](/it/docs/Web/HTML/Reference/Attributes/pattern). È anche possibile personalizzare i messaggi di errore. L'uso di queste funzionalità verrà trattato più avanti nell'articolo [Validazione dei moduli lato client](/it/docs/Learn_web_development/Extensions/Forms/Form_validation).

> [!NOTE]
> Se i dati inseriti non sono un indirizzo email, la pseudo-classe {{cssxref(':invalid')}} corrisponderà e la proprietà [`validityState.typeMismatch`](/it/docs/Web/API/ValidityState/typeMismatch) restituirà `true`.

## Campo di ricerca

I campi di ricerca sono pensati per creare caselle di ricerca nelle pagine e nelle applicazioni. Questo tipo di campo viene impostato usando il valore `search` per l'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type):

```html hidden
<label for="search">Enter a search term:</label><br />
```

```html
<input type="search" id="search" name="search" />
```

{{EmbedLiveSample('search field','100%','50')}}

La principale differenza tra un campo `text` e un campo `search` è il modo in cui il browser ne definisce l'aspetto. In alcuni browser, i campi `search` vengono visualizzati con angoli arrotondati. In alcuni browser viene visualizzata un'icona di cancellazione "Ⓧ", che svuota il campo da qualsiasi valore quando viene selezionata. Questa icona di cancellazione appare solo se il campo contiene un valore e, a eccezione di Safari, viene visualizzata solo quando il campo ha il focus. Inoltre, sui dispositivi con tastiere dinamiche, il tasto Invio della tastiera potrebbe riportare "**search**" oppure mostrare l'icona di una lente di ingrandimento.

Un'altra caratteristica degna di nota è che i valori di un campo `search` possono essere salvati e riutilizzati automaticamente per offrire il completamento automatico in più pagine dello stesso sito web; questo tende ad avvenire automaticamente nella maggior parte dei browser moderni.

## Campo numero di telefono

È possibile creare un campo speciale per compilare numeri di telefono usando `tel` come valore dell'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type):

```html hidden
<label for="tel">Enter a telephone number:</label><br />
```

```html
<input type="tel" id="tel" name="tel" />
```

{{EmbedLiveSample('phone number field','100%','50')}}

Quando viene utilizzato tramite un dispositivo touch con tastiera dinamica, la maggior parte dei dispositivi visualizza un tastierino numerico quando incontra `type="tel"`, rendendo questo tipo utile ogni volta che è utile un tastierino numerico e non solo per i numeri di telefono.

-![Tastiera email di Firefox per Android, con la e commerciale visualizzata per impostazione predefinita.](fx-android-tel-type-keyboard.jpg)

A causa dell'ampia varietà di formati di numero telefonico nel mondo, questo tipo di campo non impone alcun vincolo sul valore inserito da un utente, il che significa che può includere lettere e così via.

Come menzionato in precedenza, l'attributo [`pattern`](/it/docs/Web/HTML/Reference/Attributes/pattern) può essere utilizzato per imporre vincoli, come verrà illustrato in [Validazione dei moduli lato client](/it/docs/Learn_web_development/Extensions/Forms/Form_validation).

## Campo URL

È possibile creare un tipo speciale di campo per inserire URL usando il valore `url` per l'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type):

```html hidden
<label for="url">Enter a URL:</label><br />
```

```html
<input type="url" id="url" name="url" />
```

{{EmbedLiveSample('URL field','100%','50')}}

Aggiunge vincoli di validazione speciali al campo. Il browser segnalerà un errore se non viene inserito alcun protocollo, come `http:`, oppure se l'URL non è formato correttamente in altro modo. Sui dispositivi con tastiere dinamiche, la tastiera predefinita visualizza spesso alcuni o tutti i caratteri due punti, punto e barra in avanti come tasti predefiniti.

> [!NOTE]
> Il fatto che l'URL sia ben formato non significa necessariamente che faccia riferimento a una posizione realmente esistente.

## Campo numerico

I controlli per l'inserimento di numeri possono essere creati con un {{HTMLElement("input")}} [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) di `number`. Questo controllo ha l'aspetto di un campo di testo, ma consente solo numeri in virgola mobile e solitamente fornisce pulsanti sotto forma di spinner per aumentare e diminuire il valore del controllo. Sui dispositivi con tastiere dinamiche viene generalmente visualizzata la tastiera numerica.

```html hidden live-sample___number
<label for="number">Enter a number:</label><br />
```

```html live-sample___number
<input type="number" id="number" name="number" />
```

{{EmbedLiveSample('number','100%','50')}}

Con il tipo di input `number`, è possibile vincolare i valori minimi e massimi consentiti impostando gli attributi [`min`](/it/docs/Web/HTML/Reference/Elements/input#min) e [`max`](/it/docs/Web/HTML/Reference/Elements/input#max).

È inoltre possibile usare l'attributo `step` per impostare l'incremento e il decremento causati dalla pressione dei pulsanti dello spinner. Per impostazione predefinita, il tipo di input number valida soltanto se il numero è un intero, poiché l'attributo [`step`](/it/docs/Web/HTML/Reference/Attributes/step) è impostato su `1`. Per consentire numeri in virgola mobile, specificare `step="any"` o un valore specifico, come `step="0.01"` per limitare la parte decimale. Se omesso, poiché il valore predefinito di `step` è `1`, sono validi solo numeri interi.

Vediamo alcuni esempi:

Questo esempio crea un controllo numerico il cui valore valido è limitato a un valore dispari compreso tra `1` e `10`. I pulsanti di aumento e diminuzione modificano il valore di `2`, iniziando dal valore `min`.

```html hidden live-sample___number2
<label for="number">Enter an odd number between 1 and 10:</label><br />
```

```html live-sample___number2
<input type="number" name="age" id="age" min="1" max="10" step="2" />
```

{{EmbedLiveSample('number2','100%','50')}}

Questo esempio crea un controllo numerico il cui valore è limitato a qualsiasi valore compreso tra `0` e `1`, inclusi, e i cui pulsanti di aumento e diminuzione ne modificano il valore di `0.01`.

```html hidden live-sample___number3
<label for="number">Enter a number between 0 and 1, inclusive:</label><br />
```

```html live-sample___number3
<input type="number" name="change" id="pennies" min="0" max="1" step="0.01" />
```

{{EmbedLiveSample('number3','100%','50')}}

Il tipo di input `number` ha senso quando l'intervallo dei valori validi è limitato, come l'età o l'altezza di una persona. Se l'intervallo è troppo ampio perché gli incrementi graduali abbiano senso, come i codici ZIP degli Stati Uniti, che vanno da `00001` a `99999`, il tipo `tel` potrebbe essere un'opzione migliore; fornisce il tastierino numerico rinunciando alla funzionalità dell'interfaccia dello spinner del numero.

## Controlli slider

Un altro modo per scegliere un numero è usare uno **slider**. Questi controlli si trovano spesso su siti come quelli di e-commerce, quando si desidera impostare un prezzo massimo per filtrare i prodotti. Vediamo un esempio dal vivo per illustrarlo:

{{EmbedLiveSample('Slider controls','100%','80')}}

Dal punto di vista dell'uso, gli slider sono meno precisi dei campi di testo. Pertanto, vengono utilizzati per scegliere un numero il cui valore _esatto_ non è necessariamente importante.

Uno slider viene creato usando {{HTMLElement("input")}} con l'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) impostato sul valore `range`. Il cursore dello slider può essere spostato tramite mouse o tocco, oppure con le frecce del tastierino.

È importante configurare correttamente lo slider. A tale scopo, è vivamente consigliato impostare gli attributi [`min`](/it/docs/Web/HTML/Reference/Attributes/min), [`max`](/it/docs/Web/HTML/Reference/Attributes/max) e [`step`](/it/docs/Web/HTML/Reference/Attributes/step), che impostano rispettivamente i valori minimo, massimo e di incremento.

Vediamo il codice alla base dell'esempio precedente, per mostrare come viene realizzato. Prima di tutto, l'HTML di base:

```html
<label for="price">Choose a maximum house price: </label>
<input
  type="range"
  name="price"
  id="price"
  min="50000"
  max="500000"
  step="1000"
  value="250000" />
<output class="price-output" for="price"></output>
```

Questo esempio crea uno slider il cui valore può variare tra `50000` e `500000`, aumentando/diminuendo di 1000 alla volta. Gli è stato assegnato un valore predefinito di `250000`, usando l'attributo `value`.

Un problema degli slider è che non offrono alcun tipo di feedback visivo sul valore corrente. Per questo motivo è stato incluso un elemento {{htmlelement("output")}} per contenere il valore corrente. È possibile visualizzare un valore di input o l'output di un calcolo all'interno di qualsiasi elemento, ma `<output>` è speciale, come `<label>`, e può accettare un attributo `for` che consente di associarlo all'elemento o agli elementi da cui proviene il valore di output.

Per visualizzare effettivamente il valore corrente e aggiornarlo quando cambia, è necessario usare JavaScript, operazione realizzabile con alcune istruzioni:

```js
const price = document.querySelector("#price");
const output = document.querySelector(".price-output");

output.textContent = price.value;

price.addEventListener("input", () => {
  output.textContent = price.value;
});
```

```css hidden
body {
  text-align: center;
}
label,
output {
  display: block;
}
```

Qui vengono memorizzati i riferimenti all'input `range` e all'elemento `output` in due variabili. Quindi, viene impostato immediatamente il [`textContent`](/it/docs/Web/API/Node/textContent) di `output` al `value` corrente dell'input. Infine, viene impostato un event listener per garantire che, ogni volta che lo slider range viene spostato, il `textContent` di `output` venga aggiornato al nuovo valore.

## Selettori di data e ora

In generale, per offrire una buona esperienza utente durante la raccolta di valori di data e ora, è importante fornire un'interfaccia di selezione del calendario. Queste consentono agli utenti di selezionare date senza dover passare a un'applicazione di calendario nativa o inserirle potenzialmente in formati diversi difficili da analizzare. L'ultimo minuto del millennio precedente può essere espresso nei seguenti modi diversi: `1999/12/31`, `23:59` oppure `12/31/99T11:59PM`.

I controlli data HTML sono disponibili per gestire questo specifico tipo di dati, fornendo widget calendario e rendendo i dati uniformi.

Un controllo di data e ora viene creato usando l'elemento {{HTMLElement("input")}} e un valore appropriato per l'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type), a seconda che si desideri raccogliere date, orari o entrambi. Ecco un esempio dal vivo:

```html hidden live-sample___date1
<label for="party">Choose a date and time for your party:</label>
<input type="datetime-local" id="party" name="bday" />
<span class="validity"></span>
```

```css hidden live-sample___date1
input:invalid + span::after {
  content: " ✖";
}

input:valid + span::after {
  content: " ✓";
}
```

{{EmbedLiveSample('date1','100%','50')}}

Vediamo brevemente i diversi tipi disponibili. Si noti che l'uso di questi tipi è piuttosto complesso, specialmente considerando il supporto dei browser (vedere sotto); per tutti i dettagli, seguire i link alle pagine di riferimento per ciascun tipo, inclusi esempi dettagliati.

### `date`

[`<input type="date">`](/it/docs/Web/HTML/Reference/Elements/input/date) crea un widget per visualizzare e selezionare una data, ovvero anno, mese e giorno, senza orario.

```html hidden
<label for="date">Enter the date:</label><br />
```

```html
<input type="date" name="date" id="date" />
```

{{EmbedLiveSample('date','100%','50')}}

### `datetime-local`

[`<input type="datetime-local">`](/it/docs/Web/HTML/Reference/Elements/input/datetime-local) crea un widget per visualizzare e selezionare una data con orario, senza informazioni su uno specifico fuso orario.

```html hidden
<label for="month">Enter the date and time:</label><br />
```

```html
<input type="datetime-local" name="datetime" id="datetime" />
```

{{EmbedLiveSample('datetime-local','100%','50')}}

### `month`

[`<input type="month">`](/it/docs/Web/HTML/Reference/Elements/input/month) crea un widget per visualizzare e selezionare un mese con un anno.

```html hidden
<label for="month">Enter the month:</label><br />
```

```html
<input type="month" name="month" id="month" />
```

{{EmbedLiveSample('month','100%','50')}}

### `time`

[`<input type="time">`](/it/docs/Web/HTML/Reference/Elements/input/time) crea un widget per visualizzare e selezionare un valore di orario. Sebbene l'orario possa _essere visualizzato_ nel formato di 12 ore, il _valore restituito_ è nel formato di 24 ore.

```html hidden
<label for="time">Enter a time:</label><br />
```

```html
<input type="time" name="time" id="time" />
```

{{EmbedLiveSample('time','100%','50')}}

### `week`

[`<input type="week">`](/it/docs/Web/HTML/Reference/Elements/input/week) crea un widget per visualizzare e selezionare un numero di settimana e il relativo anno.

Le settimane iniziano il lunedì e terminano la domenica. Inoltre, la settimana 1 di ogni anno contiene il primo giovedì di quell'anno, che potrebbe non includere il primo giorno dell'anno oppure potrebbe includere gli ultimi giorni dell'anno precedente.

```html hidden
<label for="week">Enter the week:</label><br />
```

```html
<input type="week" name="week" id="week" />
```

{{EmbedLiveSample('week','100%','50')}}

### Vincolare i valori di data/ora

Tutti i controlli di data e ora possono essere vincolati usando gli attributi [`min`](/it/docs/Web/HTML/Reference/Attributes/min) e [`max`](/it/docs/Web/HTML/Reference/Attributes/max), con ulteriori vincoli possibili tramite l'attributo [`step`](/it/docs/Web/HTML/Reference/Attributes/step), il cui valore varia in base al tipo di input.

```html
<label for="myDate">When are you available this summer?</label><br />
<input
  type="date"
  name="myDate"
  min="2025-06-01"
  max="2025-08-31"
  step="7"
  id="myDate" />
```

{{EmbedLiveSample('constraining date/time values','100%','50')}}

## Controllo selettore di colore

I colori sono sempre un po' difficili da gestire. Esistono molti modi per esprimerli: valori RGB, decimali o esadecimali, valori HSL, parole chiave e così via.

Un controllo `color` può essere creato usando l'elemento {{HTMLElement("input")}} con l'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) impostato sul valore `color`:

```html hidden
<label for="color">Pick a color:</label><br />
```

```html
<input type="color" name="color" id="color" />
```

{{EmbedLiveSample('Color picker control','100%','50')}}

La selezione di un controllo di colore visualizza generalmente la funzionalità predefinita del sistema operativo per la selezione dei colori. Il valore restituito è sempre un colore esadecimale in minuscolo a 6 cifre.

## Riepilogo

Si conclude qui l'esplorazione dei tipi di input dei moduli HTML5. Esistono alcuni altri tipi di controllo che non possono essere raggruppati facilmente a causa dei loro comportamenti molto specifici, ma che è comunque essenziale conoscere. Verranno trattati nel prossimo articolo.

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/Basic_native_form_controls", "Learn_web_development/Extensions/Forms/Other_form_controls", "Learn_web_development/Extensions/Forms")}}
