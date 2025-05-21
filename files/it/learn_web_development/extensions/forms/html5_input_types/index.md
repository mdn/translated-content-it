---
title: I tipi di input di HTML5
slug: Learn_web_development/Extensions/Forms/HTML5_input_types
l10n:
  sourceCommit: a1ac64fa4da965d2a152f08221b1a9aed638fd16
---

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/Basic_native_form_controls", "Learn_web_development/Extensions/Forms/Other_form_controls", "Learn_web_development/Extensions/Forms")}}

Nell'[articolo precedente](/it/docs/Learn_web_development/Extensions/Forms/Basic_native_form_controls) abbiamo esaminato l'elemento {{htmlelement("input")}}, coprendo i valori originali dell'attributo `type` disponibili fin dai primi giorni di HTML. Ora vedremo in dettaglio la funzionalità di alcuni tipi di input aggiunti successivamente.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Una comprensione di base
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >del HTML</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere i nuovi valori dei tipi di input disponibili per creare
        controlli form nativi e come implementarli utilizzando HTML.
      </td>
    </tr>
  </tbody>
</table>

Poiché l'aspetto dei controlli form HTML può essere molto diverso dalle specifiche di un designer, gli sviluppatori web a volte costruiscono i propri controlli personalizzati. Affrontiamo questo argomento in un tutorial avanzato: [Come realizzare widget per form personalizzati](/it/docs/Learn_web_development/Extensions/Forms/How_to_build_custom_form_controls).

## Campo indirizzo email

Questo tipo di campo viene impostato utilizzando il valore `email` per l'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type):

```html hidden live-sample___email
<label for="email">Enter your email address:</label><br />
```

```html live-sample___email
<input type="email" id="email" name="email" />
```

{{EmbedLiveSample('email','100%','50')}}

Quando viene utilizzato questo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type), il valore deve essere un indirizzo email per essere valido. Qualsiasi altro contenuto fa sì che il browser mostri un errore quando il form viene inviato. Puoi vedere questo in azione nello screenshot qui sotto.

![Un input email non valido mostra il messaggio "Per favore inserisci un indirizzo email."](email_address_invalid.png)

È possibile utilizzare l'attributo [`multiple`](/it/docs/Web/HTML/Reference/Attributes/multiple) in combinazione con il tipo di input `email` per consentire l'inserimento di più indirizzi email separati da virgole nello stesso input:

```html
<input type="email" id="email" name="email" multiple />
```

Su alcuni dispositivi — in particolare dispositivi touch con tastiere dinamiche come gli smartphone — potrebbe essere presentata una tastiera virtuale diversa che è più adatta per inserire indirizzi email, includendo il tasto `@`:

![Tastiera email di Firefox per Android, con il segno "@" visualizzato per impostazione predefinita.](fx-android-email-type-keyboard.jpg)

> [!NOTE]
> Puoi trovare esempi dei tipi di input di base su [esempi di input di base](https://mdn.github.io/learning-area/html/forms/basic-input-examples/) (vedi anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/forms/basic-input-examples/index.html)).

Questo è un altro buon motivo per utilizzare questi nuovi tipi di input, migliorando l'esperienza utente per gli utenti di questi dispositivi.

### Validazione lato client

Come puoi vedere sopra, `email` — insieme ad altri tipi di `input` più recenti — fornisce una validazione _lato client_ degli errori incorporata, eseguita dal browser prima che i dati vengano inviati al server. È un aiuto utile per guidare gli utenti a compilare correttamente un form e può far risparmiare tempo: è utile sapere immediatamente che i dati non sono corretti, piuttosto che aspettare un viaggio di andata e ritorno al server.

Ma non _dovrebbe essere considerata_ una misura di sicurezza esaustiva! Le tue applicazioni dovrebbero sempre eseguire controlli di sicurezza su qualsiasi dato inviato tramite form _lato server_ così come lato client, poiché è troppo facile disattivare la validazione lato client, quindi gli utenti malintenzionati possono comunque inviare dati errati al tuo server. Leggi [Sicurezza del sito web](/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Website_security) per avere un'idea di cosa _potrebbe_ accadere; implementare la validazione lato server va oltre lo scopo di questo modulo, ma dovresti tenerlo presente.

Nota che `a@b` è un indirizzo email valido secondo i vincoli predefiniti forniti. Questo perché il tipo di input `email` consente di default gli indirizzi email intranet. Per implementare un comportamento di validazione diverso, puoi utilizzare l'attributo [`pattern`](/it/docs/Web/HTML/Reference/Attributes/pattern). Puoi anche personalizzare i messaggi di errore. Ne parleremo nell'articolo [Validazione dei form lato client](/it/docs/Learn_web_development/Extensions/Forms/Form_validation) più avanti.

> [!NOTE]
> Se i dati inseriti non sono un indirizzo email, la pseudo-classe {{cssxref(':invalid')}} sarà corrispondente, e la proprietà [`validityState.typeMismatch`](/it/docs/Web/API/ValidityState/typeMismatch) restituirà `true`.

## Campo di ricerca

I campi di ricerca sono pensati per essere utilizzati per creare caselle di ricerca su pagine e applicazioni. Questo tipo di campo viene impostato utilizzando il valore `search` per l'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type):

```html hidden
<label for="search">Enter a search term:</label><br />
```

```html
<input type="search" id="search" name="search" />
```

{{EmbedLiveSample('search field','100%','50')}}

La principale differenza tra un campo `text` e un campo `search` è l'aspetto stilistico che il browser applica. In alcuni browser, i campi `search` vengono resi con angoli arrotondati. In alcuni viene visualizzata un'icona "Ⓧ" di cancellazione, che pulisce il campo da qualsiasi valore quando viene cliccata. Questa icona di cancellazione appare solo se il campo ha un valore e, a parte Safari, viene visualizzata solo quando il campo è focalizzato. Inoltre, su dispositivi con tastiere dinamiche, il tasto invio della tastiera potrebbe leggere "**search**", o visualizzare un'icona a forma di lente d'ingrandimento.

Un'altra caratteristica degna di nota è che i valori di un campo `search` possono essere automaticamente salvati e riutilizzati per offrire l'auto-completamento su più pagine dello stesso sito web; ciò tende ad accadere automaticamente nella maggior parte dei moderni browser.

## Campo numero di telefono

Un campo speciale per l'inserimento di numeri di telefono può essere creato usando `tel` come valore dell'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type):

```html hidden
<label for="tel">Enter a telephone number:</label><br />
```

```html
<input type="tel" id="tel" name="tel" />
```

{{EmbedLiveSample('phone number field','100%','50')}}

Quando viene accesso tramite un dispositivo touch con tastiera dinamica, la maggior parte dei dispositivi mostrerà un tastierino numerico quando viene incontrato `type="tel"`, il che significa che questo tipo è utile ogni volta che è utile un tastierino numerico e non deve essere utilizzato solo per i numeri di telefono.

![Tastiera email di Firefox per Android, con il segno "&" visualizzato per impostazione predefinita.](fx-android-tel-type-keyboard.jpg)

A causa della grande varietà di formati di numeri di telefono in tutto il mondo, questo tipo di campo non impone alcun vincolo sul valore inserito da un utente (ciò significa che può includere lettere, ecc.).

Come abbiamo detto in precedenza, l'attributo [`pattern`](/it/docs/Web/HTML/Reference/Attributes/pattern) può essere utilizzato per imporre vincoli, che imparerai a conoscere in [Validazione dei form lato client](/it/docs/Learn_web_development/Extensions/Forms/Form_validation).

## Campo URL

Un tipo speciale di campo per l'inserimento di URL può essere creato utilizzando il valore `url` per l'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type):

```html hidden
<label for="url">Enter a URL:</label><br />
```

```html
<input type="url" id="url" name="url" />
```

{{EmbedLiveSample('URL field','100%','50')}}

Aggiunge vincoli di validazione speciali al campo. Il browser segnalerà un errore se non viene inserito alcun protocollo (come `http:`), o se l'URL è altrimenti malformato. Su dispositivi con tastiere dinamiche, la tastiera predefinita mostrerà spesso alcuni o tutti i due punti, il punto e la barra come tasti predefiniti.

> [!NOTE]
> Solo perché l'URL è ben formato non significa necessariamente che si riferisca a una posizione che esiste realmente!

## Campo numerico

I controlli per l'inserimento dei numeri possono essere creati con un {{HTMLElement("input")}} [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) di `number`. Questo controllo assomiglia a un campo di testo ma consente solo numeri a punto mobile e di solito fornisce pulsanti sotto forma di uno spinner per aumentare e diminuire il valore del controllo. Su dispositivi con tastiere dinamiche, generalmente viene visualizzata la tastiera numerica.

```html hidden live-sample___number
<label for="number">Enter a number:</label><br />
```

```html live-sample___number
<input type="number" id="number" name="number" />
```

{{EmbedLiveSample('number','100%','50')}}

Con il tipo di input `number`, puoi vincolare i valori minimi e massimi consentiti impostando gli attributi [`min`](/it/docs/Web/HTML/Reference/Elements/input#min) e [`max`](/it/docs/Web/HTML/Reference/Elements/input#max).

È anche possibile utilizzare l'attributo `step` per impostare l'incremento di aumento e diminuzione causato dalla pressione dei pulsanti dello spinner. Per impostazione predefinita, il tipo di input numerico valida solo se il numero è un intero, poiché l'attributo [`step`](/it/docs/Web/HTML/Reference/Attributes/step) predefinito è `1`. Per consentire numeri decimali, specifica `step="any"` o un valore specifico, come `step="0.01"` per limitare i numeri a virgola mobile. Se omesso, poiché il valore step predefinito è `1`, solo i numeri interi sono validi.

Ecco alcuni esempi:

Questo esempio crea un controllo numerico il cui valore valido è limitato a un valore dispari compreso tra `1` e `10`. I pulsanti di aumento e diminuzione cambiano il valore di `2`, partendo dal valore `min`.

```html hidden live-sample___number2
<label for="number">Enter an odd number between 1 and 10:</label><br />
```

```html live-sample___number2
<input type="number" name="age" id="age" min="1" max="10" step="2" />
```

{{EmbedLiveSample('number2','100%','50')}}

Questo esempio crea un controllo numerico il cui valore è limitato a qualsiasi valore compreso tra `0` e `1` inclusi, e i cui pulsanti di aumento e diminuzione modificano il suo valore di `0.01`.

```html hidden live-sample___number3
<label for="number">Enter a number between 0 and 1, inclusive:</label><br />
```

```html live-sample___number3
<input type="number" name="change" id="pennies" min="0" max="1" step="0.01" />
```

{{EmbedLiveSample('number3','100%','50')}}

Il tipo di input `number` ha senso quando il range di valori validi è limitato, come l'età o l'altezza di una persona. Se il range è troppo ampio per avere senso con incrementi incrementali (come i codici postali USA, che vanno da `00001` a `99999`), il tipo `tel` potrebbe essere una migliore opzione; fornisce il tastierino numerico evitando la funzione dell'interfaccia utente dello spinner del numero.

## Controlli slider

Un altro modo per scegliere un numero è usare uno **slider**. Li vedi spesso su siti come siti di acquisti dove vuoi impostare un prezzo massimo per filtrare. Vediamo un esempio dal vivo per illustrare questo:

{{EmbedLiveSample('Slider controls','100%','50')}}

Dal punto di vista dell'uso, gli slider sono meno precisi dei campi di testo. Pertanto, vengono utilizzati per scegliere un numero il cui valore _preciso_ non è necessariamente importante.

Uno slider è creato usando l'elemento {{HTMLElement("input")}} con il suo attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) impostato sul valore `range`. Il cursore dello slider può essere mosso tramite mouse o touch, o con le frecce della tastiera.

È importante configurare correttamente il tuo slider. A tal fine, è altamente consigliato impostare gli attributi [`min`](/it/docs/Web/HTML/Reference/Attributes/min), [`max`](/it/docs/Web/HTML/Reference/Attributes/max) e [`step`](/it/docs/Web/HTML/Reference/Attributes/step) che impostano i valori minimo, massimo e di incremento rispettivamente.

Vediamo il codice dietro l'esempio sopra, così puoi vedere come è fatto. Innanzitutto, l'HTML di base:

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

Questo esempio crea uno slider il cui valore può essere compreso tra `50000` e `500000`, il quale incrementa/decrementa di 1000 alla volta. Gli abbiamo dato un valore predefinito di `250000`, usando l'attributo `value`.

Un problema con gli slider è che non offrono alcun tipo di feedback visivo sul valore attuale. Questo è il motivo per cui abbiamo incluso un elemento {{htmlelement("output")}} per contenere il valore attuale. Potresti visualizzare un valore di input o l'output di un calcolo all'interno di qualsiasi elemento, ma `<output>` è speciale — come `<label>` — e può prendere un attributo `for` che ti permette di associarlo all'elemento o agli elementi da cui proviene il valore di output.

Per visualizzare effettivamente il valore attuale, e aggiornarlo quando cambia, devi usare JavaScript, il che può essere fatto con poche istruzioni:

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

Qui memorizziamo i riferimenti all'input `range` e all'`output` in due variabili. Poi impostiamo immediatamente il [`textContent`](/it/docs/Web/API/Node/textContent) dell'`output` sul valore attuale dell'input. Infine, viene impostato un rilevatore di eventi per assicurarsi che ogni volta che lo slider del range viene spostato, il `textContent` dell'`output` venga aggiornato al nuovo valore.

## Selettori di data e ora

In generale, per una buona esperienza utente quando si raccolgono valori di data e ora, è importante fornire una UI di selezione del calendario. Queste permettono agli utenti di selezionare date senza dover cambiare contesto a un'applicazione calendario nativa o potenzialmente inserirle in formati diversi che sono difficili da analizzare. L'ultimo minuto del millennio precedente può essere espresso in modi diversi: `1999/12/31`, `23:59`, o `12/31/99T11:59PM`.

I controlli data HTML sono disponibili per gestire questo tipo specifico di dati, fornendo widget calendario e rendendo i dati uniformi.

Un controllo di data e ora è creato usando l'elemento {{HTMLElement("input")}} e un valore appropriato per l'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type), a seconda che tu desideri raccogliere date, orari o entrambi. Ecco un esempio dal vivo:

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

Vediamo brevemente i diversi tipi disponibili. Nota che l'uso di questi tipi è abbastanza complesso, soprattutto considerando il supporto del browser (vedi sotto); per trovare i dettagli completi, segui i link sottostanti alle pagine di riferimento per ciascun tipo, inclusi esempi dettagliati.

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

[`<input type="datetime-local">`](/it/docs/Web/HTML/Reference/Elements/input/datetime-local) crea un widget per visualizzare e scegliere una data con ora senza informazioni specifiche sul fuso orario.

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

[`<input type="time">`](/it/docs/Web/HTML/Reference/Elements/input/time) crea un widget per visualizzare e scegliere un valore di ora. Mentre l'orario può _essere visualizzato_ nel formato a 12 ore, il _valore restituito_ è nel formato a 24 ore.

```html hidden
<label for="time">Enter a time:</label><br />
```

```html
<input type="time" name="time" id="time" />
```

{{EmbedLiveSample('time','100%','50')}}

### `week`

[`<input type="week">`](/it/docs/Web/HTML/Reference/Elements/input/week) crea un widget per visualizzare e scegliere un numero di settimana e il suo anno.

Le settimane iniziano il lunedì e terminano la domenica. Inoltre, la prima settimana 1 di ogni anno contiene il primo giovedì di quell'anno — il quale potrebbe non includere il primo giorno dell'anno, o potrebbe includere gli ultimi giorni dell'anno precedente.

```html hidden
<label for="week">Enter the week:</label><br />
```

```html
<input type="week" name="week" id="week" />
```

{{EmbedLiveSample('week','100%','50')}}

### Limitazione dei valori di data/ora

Tutti i controlli data e ora possono essere limitati utilizzando gli attributi [`min`](/it/docs/Web/HTML/Reference/Attributes/min) e [`max`](/it/docs/Web/HTML/Reference/Attributes/max), con una ulteriore limitazione possibile tramite l'attributo [`step`](/it/docs/Web/HTML/Reference/Attributes/step) (il cui valore varia a seconda del tipo di input).

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

## Controllo selezione colore

I colori sono sempre un po' difficili da gestire. Ci sono molti modi per esprimerli: valori RGB (decimali o esadecimali), valori HSL, parole chiave, e così via.

Un controllo `color` può essere creato utilizzando l'elemento {{HTMLElement("input")}} con il suo attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) impostato sul valore `color`:

```html hidden
<label for="color">Pick a color:</label><br />
```

```html
<input type="color" name="color" id="color" />
```

{{EmbedLiveSample('Color picker control','100%','50')}}

Facendo clic su un controllo colore si visualizza generalmente la funzionalità di selezione colore predefinita del sistema operativo per consentirti di scegliere. Il valore restituito è sempre un colore esadecimale a 6 valori in minuscolo.

## Metti alla prova le tue abilità!

Hai raggiunto la fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare ulteriori test per verificare che tu abbia mantenuto queste informazioni prima di procedere — vedi [Metti alla prova le tue abilità: controlli HTML5](/it/docs/Learn_web_development/Extensions/Forms/Test_your_skills/Input_types).

## Sommario

Questo ci porta alla fine del nostro tour dei tipi di input del form HTML5. Ci sono alcuni altri tipi di controllo che non possono essere facilmente raggruppati a causa dei loro comportamenti molto specifici ma che sono ancora essenziali da conoscere. Trattiamo quelli nell'articolo successivo.

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/Basic_native_form_controls", "Learn_web_development/Extensions/Forms/Other_form_controls", "Learn_web_development/Extensions/Forms")}}
