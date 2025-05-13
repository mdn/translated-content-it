---
title: I tipi di input di HTML5
slug: Learn_web_development/Extensions/Forms/HTML5_input_types
l10n:
  sourceCommit: a1ac64fa4da965d2a152f08221b1a9aed638fd16
---

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/Basic_native_form_controls", "Learn_web_development/Extensions/Forms/Other_form_controls", "Learn_web_development/Extensions/Forms")}}

Nell'[articolo precedente](/it/docs/Learn_web_development/Extensions/Forms/Basic_native_form_controls), abbiamo esaminato l'elemento {{htmlelement("input")}}, coprendo i valori originali dell'attributo `type` disponibili fin dai primi giorni di HTML. Ora esamineremo in dettaglio la funzionalità di alcuni tipi di input aggiunti successivamente.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Una comprensione di base
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >dell'HTML</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere i nuovi valori dei tipi di input disponibili per creare controlli di modulo nativi, e come implementarli usando l'HTML.
      </td>
    </tr>
  </tbody>
</table>

Poiché l'aspetto dei controlli del modulo HTML può essere molto diverso dalle specifiche del designer, gli sviluppatori web a volte costruiscono i propri controlli di modulo personalizzati. Trattiamo questo argomento in un tutorial avanzato: [Come creare widget di modulo personalizzati](/it/docs/Learn_web_development/Extensions/Forms/How_to_build_custom_form_controls).

## Campo indirizzo email

Questo tipo di campo viene impostato utilizzando il valore `email` per l'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type):

```html hidden live-sample___email
<label for="email">Enter your email address:</label><br />
```

```html live-sample___email
<input type="email" id="email" name="email" />
```

{{EmbedLiveSample('email','100%','50')}}

Quando questo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) è utilizzato, il valore deve essere un indirizzo email per essere valido. Qualsiasi altro contenuto causa la visualizzazione di un errore da parte del browser quando il modulo viene inviato. Può vedere questo in azione nello screenshot qui sotto.

![Un input email non valido che mostra il messaggio "Please enter an email address."](email_address_invalid.png)

Può usare l'attributo [`multiple`](/it/docs/Web/HTML/Reference/Attributes/multiple) in combinazione con il tipo di input `email` per consentire l'inserimento di diversi indirizzi email separati da virgole nello stesso input:

```html
<input type="email" id="email" name="email" multiple />
```

Su alcuni dispositivi — in particolare, dispositivi touch con tastiere dinamiche come gli smartphone — potrebbe essere presentata una tastiera virtuale diversa che è più adatta per inserire indirizzi email, incluso il tasto `@`:

![Tastiera email di Firefox per Android, con il simbolo della chiocciola visualizzato per impostazione predefinita.](fx-android-email-type-keyboard.jpg)

> [!NOTE]
> Può trovare esempi dei tipi di input di base su [esempi di input di base](https://mdn.github.io/learning-area/html/forms/basic-input-examples/) (veda anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/forms/basic-input-examples/index.html)).

Questo è un altro buon motivo per utilizzare questi tipi di input più recenti, migliorando l'esperienza utente per gli utenti di questi dispositivi.

### Validazione lato client

Come può vedere sopra, `email` — insieme ad altri tipi di `input` più recenti — fornisce la validazione degli errori _lato client_ incorporata, eseguita dal browser prima che i dati vengano inviati al server. È un aiuto utile per guidare gli utenti a compilare accuratamente un modulo e può risparmiare tempo: sapere immediatamente che i dati non sono corretti è utile, piuttosto che dover aspettare un viaggio di andata e ritorno al server.

Ma _non dovrebbe essere considerato_ una misura di sicurezza esaustiva! Le sue applicazioni dovrebbero sempre eseguire controlli di sicurezza su qualsiasi dato inviato tramite modulo sul _lato server_ così come sul lato client, perché la validazione lato client è troppo facile da disabilitare, quindi gli utenti malintenzionati possono ancora inviare facilmente dati errati al suo server. Legga [Sicurezza del sito web](/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Website_security) per avere un'idea di cosa _potrebbe_ accadere; implementare la validazione lato server è un po' oltre lo scopo di questo modulo, ma dovrebbe tenerlo a mente.

Noti che `a@b` è un indirizzo email valido secondo i vincoli forniti di default. Questo perché il tipo di input `email` consente per impostazione predefinita indirizzi email di intranet. Per implementare un comportamento di validazione diverso, può utilizzare l'attributo [`pattern`](/it/docs/Web/HTML/Reference/Attributes/pattern). Può anche personalizzare i messaggi di errore. Parleremo su come utilizzare queste funzionalità nell'articolo [Validazione dei moduli lato client](/it/docs/Learn_web_development/Extensions/Forms/Form_validation) in seguito.

> [!NOTE]
> Se i dati inseriti non sono un indirizzo email, la pseudo-classe {{cssxref(':invalid')}} corrisponderà e la proprietà [`validityState.typeMismatch`](/it/docs/Web/API/ValidityState/typeMismatch) restituirà `true`.

## Campo di ricerca

I campi di ricerca sono destinati a essere utilizzati per creare caselle di ricerca su pagine e app. Questo tipo di campo viene impostato utilizzando il valore `search` per l'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type):

```html hidden
<label for="search">Enter a search term:</label><br />
```

```html
<input type="search" id="search" name="search" />
```

{{EmbedLiveSample('search field','100%','50')}}

La principale differenza tra un campo `text` e un campo `search` è come il browser ne stila l'aspetto. In alcuni browser, i campi di `search` vengono resi con angoli arrotondati. In alcuni browser, viene visualizzata un'icona di cancellazione "Ⓧ", che cancella il campo di qualsiasi valore quando viene cliccata. Questa icona di cancellazione appare solo se il campo ha un valore e, ad eccezione di Safari, viene visualizzata solo quando il campo è focalizzato. Inoltre, sui dispositivi con tastiere dinamiche, il tasto invio della tastiera può leggere "**search**", o visualizzare un'icona di lente di ingrandimento.

Un'altra caratteristica degna di nota è che i valori di un campo `search` possono essere automaticamente salvati e riutilizzati per offrire l'auto-completamento su più pagine dello stesso sito web; questo tende a verificarsi automaticamente nella maggior parte dei browser moderni.

## Campo numero di telefono

Un campo speciale per riempire i numeri di telefono può essere creato usando `tel` come valore dell'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type):

```html hidden
<label for="tel">Enter a telephone number:</label><br />
```

```html
<input type="tel" id="tel" name="tel" />
```

{{EmbedLiveSample('phone number field','100%','50')}}

Quando viene utilizzato da un dispositivo touch con tastiera dinamica, la maggior parte dei dispositivi visualizzerà una tastiera numerica quando si incontra `type="tel"`, ciò significa che questo tipo è utile ogni volta che una tastiera numerica è utile, e non deve necessariamente essere utilizzato solo per i numeri di telefono.

-![Tastiera email di Firefox per Android, con e commerciale visualizzato per default.](fx-android-tel-type-keyboard.jpg)

A causa della vasta varietà di formati di numeri di telefono in tutto il mondo, questo tipo di campo non impone alcun vincolo sul valore inserito da un utente (questo significa che può includere lettere, ecc.).

Come accennato in precedenza, l'attributo [`pattern`](/it/docs/Web/HTML/Reference/Attributes/pattern) può essere utilizzato per imporre vincoli, che imparerà su [Validazione dei moduli lato client](/it/docs/Learn_web_development/Extensions/Forms/Form_validation).

## Campo URL

Un tipo speciale di campo per immettere URL può essere creato utilizzando il valore `url` per l'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type):

```html hidden
<label for="url">Enter a URL:</label><br />
```

```html
<input type="url" id="url" name="url" />
```

{{EmbedLiveSample('URL field','100%','50')}}

Aggiunge vincoli di validazione speciali al campo. Il browser segnalerà un errore se non viene inserito alcun protocollo (come `http:`), o se l'URL è altrimenti malformato. Sui dispositivi con tastiere dinamiche, la tastiera predefinita spesso visualizzerà alcuni o tutti i due punti, punto e barra come tasti predefiniti.

> [!NOTE]
> Solo perché l'URL è ben formato non significa necessariamente che si riferisca a una posizione che esiste effettivamente!

## Campo numerico

I controlli per l'inserimento di numeri possono essere creati con un {{HTMLElement("input")}} [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) di `number`. Questo controllo assomiglia a un campo di testo ma consente solo numeri a virgola mobile, e solitamente fornisce pulsanti sotto forma di uno spinner per aumentare e ridurre il valore del controllo. Sui dispositivi con tastiere dinamiche, generalmente viene visualizzata la tastiera numerica.

```html hidden live-sample___number
<label for="number">Enter a number:</label><br />
```

```html live-sample___number
<input type="number" id="number" name="number" />
```

{{EmbedLiveSample('number','100%','50')}}

Con il tipo di input `number`, può limitare i valori minimi e massimi consentiti impostando gli attributi [`min`](/it/docs/Web/HTML/Reference/Elements/input#min) e [`max`](/it/docs/Web/HTML/Reference/Elements/input#max).

Può anche utilizzare l'attributo `step` per impostare l'incremento e il decremento causati dalla pressione dei pulsanti dello spinner. Per default, il tipo di input number si valida solo se il numero è un intero, poiché l'attributo [`step`](/it/docs/Web/HTML/Reference/Attributes/step) di default è `1`. Per consentire numeri a virgola mobile, specifichi `step="any"` o un valore specifico, come `step="0.01"` per limitare il punto mobile. Se omesso, poiché il valore `step` di default è `1`, sono validi solo numeri interi.

Vediamo alcuni esempi:

Questo esempio crea un controllo numerico il cui valore valido è limitato a un valore dispari compreso tra `1` e `10`. I pulsanti di incremento e decremento cambiano il valore di `2`, a partire dal valore `min`.

```html hidden live-sample___number2
<label for="number">Enter an odd number between 1 and 10:</label><br />
```

```html live-sample___number2
<input type="number" name="age" id="age" min="1" max="10" step="2" />
```

{{EmbedLiveSample('number2','100%','50')}}

Questo esempio crea un controllo numerico il cui valore è limitato a qualsiasi valore compreso tra `0` e `1` inclusi, e i cui pulsanti di incremento e decremento cambiano il suo valore di `0.01`.

```html hidden live-sample___number3
<label for="number">Enter a number between 0 and 1, inclusive:</label><br />
```

```html live-sample___number3
<input type="number" name="change" id="pennies" min="0" max="1" step="0.01" />
```

{{EmbedLiveSample('number3','100%','50')}}

Il tipo di input `number` è sensato quando l'intervallo di valori validi è limitato, come l'età o l'altezza di una persona. Se l'intervallo è troppo ampio perché gli incrementi incrementali abbiano senso (come i codici postali degli USA, che variano da `00001` a `99999`), il tipo `tel` potrebbe essere un'opzione migliore; fornisce la tastiera numerica evitandone la funzione UI dello spinner.

## Controlli slider

Un altro modo per scegliere un numero è utilizzare uno **slider**. Vede questi abbastanza spesso su siti come siti di shopping dove vuole impostare un prezzo massimo della proprietà da filtrare. Vediamo un esempio live per illustrare questo:

{{EmbedLiveSample('Slider controls','100%','50')}}

A livello di utilizzo, gli slider sono meno accurati dei campi di testo. Pertanto, vengono usati per scegliere un numero il cui valore _preciso_ non è necessariamente importante.

Uno slider è creato utilizzando l'{{HTMLElement("input")}} con il suo attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) impostato sul valore `range`. Il cursore dello slider può essere mosso via mouse o touch, o con le frecce della tastiera.

È importante configurare correttamente il suo slider. A tal fine, è altamente consigliato impostare gli attributi [`min`](/it/docs/Web/HTML/Reference/Attributes/min), [`max`](/it/docs/Web/HTML/Reference/Attributes/max), e [`step`](/it/docs/Web/HTML/Reference/Attributes/step) che impostano i valori minimo, massimo, e incrementale, rispettivamente.

Vediamo il codice dietro l'esempio precedente, così può vedere come è fatto. Prima di tutto, l'HTML di base:

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

Questo esempio crea un cursore il cui valore può variare tra `50000` e `500000`, che incrementa/decrementa di 1000 alla volta. Gli abbiamo assegnato un valore di default di `250000`, usando l'attributo `value`.

Un problema con gli slider è che non offrono alcun tipo di feedback visivo sul valore corrente. Questo è il motivo per cui abbiamo incluso un elemento {{htmlelement("output")}} per contenere il valore corrente. Potrebbe visualizzare un valore di input o l'output di un calcolo all'interno di qualsiasi elemento, ma `<output>` è speciale — come `<label>` — e può prendere un attributo `for` che le permette di associarlo all'elenco o agli elementi da cui è provenuto il valore dell'output.

Per visualizzare effettivamente il valore corrente, e aggiornarlo come cambia, deve utilizzare JavaScript, che può essere realizzato con alcune istruzioni:

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

Qui memorizza riferimenti al`range` input e all'`output` in due variabili. Poi imposta immediatamente il [`textContent`](/it/docs/Web/API/Node/textContent) dell'`output` al valore corrente dell'input. Infine, viene impostato un event listener per assicurarsi che ogni volta che il cursore del range viene spostato, il `textContent` dell'`output` sia aggiornato al nuovo valore.

## Selettori di data e ora

In generale, per una buona esperienza utente quando si raccolgono valori di data e ora, è importante fornire un'interfaccia utente di selezione del calendario. Queste consentono agli utenti di selezionare le date senza dover passare a un'applicazione calendario nativa o potenzialmente inserirle in formati diversi che sono difficili da analizzare. L'ultimo minuto del secolo precedente può essere espresso nei seguenti modi diversi: `1999/12/31`, `23:59`, o `12/31/99T11:59PM`.

I controlli di data HTML sono disponibili per gestire questo tipo specifico di dati, fornendo widget di calendario e uniformando i dati.

Un controllo di data e orario è creato usando l'elemento {{HTMLElement("input")}} e un valore appropriato per l'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type), a seconda se si desidera raccogliere date, orari, o entrambi. Ecco un esempio live:

```html hidden live-sample___date1
<label for="party">Choose a date and time for your party:</label>
<input type="datetime-local" id="party" name="bday" />
<span class="validity"></span>
```

```css hidden live-sample___date1
input:invalid + span:after {
  content: " ✖";
}

input:valid + span:after {
  content: " ✓";
}
```

{{EmbedLiveSample('date1','100%','50')}}

Vediamo in breve i diversi tipi disponibili. Si noti che l'utilizzo di questi tipi è abbastanza complesso, specialmente considerando il supporto dei browser (veda sotto); per trovare tutti i dettagli, segua i link sotto alle pagine di riferimento per ciascun tipo, compresi esempi dettagliati.

### `date`

[`<input type="date">`](/it/docs/Web/HTML/Reference/Elements/input/date) crea un widget per visualizzare e scegliere una data (anno, mese e giorno, senza ora).

```html hidden
<label for="date">Enter the date:</label><br />
```

```html
<input type="date" name="date" id="date" />
```

{{EmbedLiveSample('date','100%','50')}}

### `datetime-local`

[`<input type="datetime-local">`](/it/docs/Web/HTML/Reference/Elements/input/datetime-local) crea un widget per visualizzare e scegliere una data con orario senza informazioni sul fuso orario specifico.

```html hidden
<label for="month">Enter the date and time:</label><br />
```

```html
<input type="datetime-local" name="datetime" id="datetime" />
```

{{EmbedLiveSample('datetime-local','100%','50')}}

### `month`

[`<input type="month">`](/it/docs/Web/HTML/Reference/Elements/input/month) crea un widget per visualizzare e scegliere un mese con un anno.

```html hidden
<label for="month">Enter the month:</label><br />
```

```html
<input type="month" name="month" id="month" />
```

{{EmbedLiveSample('month','100%','50')}}

### `time`

[`<input type="time">`](/it/docs/Web/HTML/Reference/Elements/input/time) crea un widget per visualizzare e scegliere un valore di tempo. Anche se il tempo può _visualizzare_ nel formato 12 ore, il _valore restituito_ è nel formato 24 ore.

```html hidden
<label for="time">Enter a time:</label><br />
```

```html
<input type="time" name="time" id="time" />
```

{{EmbedLiveSample('time','100%','50')}}

### `week`

[`<input type="week">`](/it/docs/Web/HTML/Reference/Elements/input/week) crea un widget per visualizzare e scegliere un numero di settimana e il suo anno.

Le settimane iniziano il lunedì e terminano la domenica. Inoltre, la prima settimana dell'anno 1 contiene il primo giovedì di quell'anno — il che può non includere il primo giorno dell'anno, o può includere gli ultimi giorni dell'anno precedente.

```html hidden
<label for="week">Enter the week:</label><br />
```

```html
<input type="week" name="week" id="week" />
```

{{EmbedLiveSample('week','100%','50')}}

### Limitare i valori di data/ora

Tutti i controlli di data e ora possono essere limitati usando gli attributi [`min`](/it/docs/Web/HTML/Reference/Attributes/min) e [`max`](/it/docs/Web/HTML/Reference/Attributes/max), con ulteriori limitazioni possibili tramite l'attributo [`step`](/it/docs/Web/HTML/Reference/Attributes/step) (il cui valore varia a seconda del tipo di input).

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

I colori sono sempre un po' difficili da gestire. Ci sono molti modi per esprimerli: valori RGB (decimali o esadecimali), valori HSL, parole chiave, e così via.

Un controllo `color` può essere creato usando l'elemento {{HTMLElement("input")}} con il suo attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) impostato al valore `color`:

```html hidden
<label for="color">Pick a color:</label><br />
```

```html
<input type="color" name="color" id="color" />
```

{{EmbedLiveSample('Color picker control','100%','50')}}

Facendo clic su un controllo di colore, generalmente viene visualizzata la funzionalità di selezione del colore predefinita del sistema operativo per consentire la scelta. Il valore restituito è sempre un colore esadecimale a 6 cifre in minuscolo.

## Verifica le tue abilità!

Ha raggiunto la fine di questo articolo, ma può ricordare le informazioni più importanti? Può trovare ulteriori test per verificare che abbia mantenuto queste informazioni prima di procedere — veda [Verifica le tue abilità: controlli HTML5](/it/docs/Learn_web_development/Extensions/Forms/Test_your_skills/Input_types).

## Sommario

Questo ci porta alla fine del nostro tour dei tipi di input dei moduli HTML5. Ci sono alcuni altri tipi di controllo che non possono essere facilmente raggruppati a causa dei loro comportamenti molto specifici, ma sono ancora essenziali da conoscere. Li trattiamo nell'articolo successivo.

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/Basic_native_form_controls", "Learn_web_development/Extensions/Forms/Other_form_controls", "Learn_web_development/Extensions/Forms")}}
