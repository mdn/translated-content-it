---
title: Altri controlli dei form
slug: Learn_web_development/Extensions/Forms/Other_form_controls
l10n:
  sourceCommit: a1ac64fa4da965d2a152f08221b1a9aed638fd16
---

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/HTML5_input_types","Learn_web_development/Extensions/Forms/Styling_web_forms", "Learn_web_development/Extensions/Forms")}}

Esaminiamo ora in dettaglio la funzionalità degli elementi del form non-`<input>`, da altri tipi di controllo come liste a discesa e campi di testo multilinea, ad altre caratteristiche utili dei form come l'elemento {{htmlelement('output')}} (che abbiamo visto in azione nell'articolo precedente) e le barre di progresso.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Una comprensione base di
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >HTML</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere le caratteristiche dei form non-<code>&#x3C;input></code> e come
        implementarle usando HTML.
      </td>
    </tr>
  </tbody>
</table>

## Campi di testo multilinea

Un campo di testo multilinea è specificato utilizzando un elemento {{HTMLElement("textarea")}}, invece di utilizzare l'elemento {{HTMLElement("input")}}.

```html
<textarea cols="30" rows="8"></textarea>
```

Questo viene visualizzato così:

{{EmbedLiveSample("Multi-line_text_fields", 120, 160)}}

La principale differenza tra un `<textarea>` e un normale campo di testo a linea singola è che agli utenti è permesso includere interruzioni di linea (ossia, premendo invio) che verranno incluse quando i dati vengono inviati.

`<textarea>` necessita anche di un tag di chiusura; qualsiasi testo predefinito che si desidera contenere deve essere inserito tra i tag di apertura e chiusura. Al contrario, il {{HTMLElement("input")}} è un {{Glossary("void_element", "elemento vuoto")}} senza tag di chiusura — qualsiasi valore predefinito è inserito all'interno dell'attributo [`value`](/it/docs/Web/HTML/Reference/Elements/input#value).

Nota che, anche se puoi inserire qualsiasi cosa all'interno di un elemento `<textarea>` (inclusi altri elementi HTML, CSS e JavaScript), a causa della sua natura, viene reso tutto come se fosse contenuto di testo semplice. (Usare [`contenteditable`](/it/docs/Web/HTML/Reference/Global_attributes/contenteditable) su controlli non di form fornisce un'API per catturare contenuti HTML/"rich" invece che testo semplice).

Visivamente, il testo inserito si avvolge e il controllo forma è di default ridimensionabile. La maggior parte dei browser fornisce un manico di trascinamento che puoi trascinare per aumentare/diminuire la dimensione dell'area di testo.

Puoi trovare un esempio di utilizzo dell'area di testo nell'[esempio](https://mdn.github.io/learning-area/html/forms/your-first-HTML-form/first-form-styled.html) che abbiamo messo insieme nel primo articolo di questo.

### Controllo del rendering su più linee

{{htmlelement("textarea")}} accetta tre attributi per controllare il rendering su più linee:

- [`cols`](/it/docs/Web/HTML/Reference/Elements/textarea#cols)
  - : Specifica la larghezza visibile (colonne) del controllo di testo, misurata in larghezze medie di caratteri. Questo è effettivamente la larghezza iniziale, poiché può essere modificata ridimensionando il `<textarea>`, e sovrascritta utilizzando CSS. Il valore predefinito se nessuno è specificato è 20.
- [`rows`](/it/docs/Web/HTML/Reference/Elements/textarea#rows)
  - : Specifica il numero di righe di testo visibili per il controllo. Questo è effettivamente l'altezza iniziale, poiché può essere modificata ridimensionando il `<textarea>`, e sovrascritta utilizzando CSS. Il valore predefinito se nessuno è specificato è 2.
- [`wrap`](/it/docs/Web/HTML/Reference/Elements/textarea#wrap)
  - : Specifica come il controllo avvolge il testo. I valori sono `soft` (il valore predefinito), il che significa che il testo inviato non è avvolto ma il testo reso dal browser è avvolto; `hard` (l'attributo `cols` deve essere specificato quando si utilizza questo valore), il che significa che sia il testo inviato che quello reso sono avvolti, e `off`, che interrompe l'avvolgimento.

### Controllo della ridimensionabilità di textarea

La possibilità di ridimensionare un `<textarea>` è controllata con la proprietà CSS `resize`. I suoi valori possibili sono:

- `both`: Il predefinito — consente il ridimensionamento orizzontale e verticale.
- `horizontal`: Consente il ridimensionamento solo orizzontale.
- `vertical`: Consente il ridimensionamento solo verticale.
- `none`: Non consente alcun ridimensionamento.
- `block` e `inline`: Valori sperimentali che consentono il ridimensionamento nella direzione `block` o `inline` solo (questo varia a seconda della direzionalità del tuo testo; leggi [Gestione delle diverse direzioni del testo](/it/docs/Learn_web_development/Core/Styling_basics/Handling_different_text_directions) se vuoi saperne di più.)

Gioca con l'esempio interattivo nella parte superiore della pagina di riferimento {{cssxref("resize")}} per una dimostrazione di come funzionano.

## Controlli a discesa

I controlli a discesa sono un modo semplice per permettere agli utenti di selezionare tra molte opzioni senza occupare molto spazio nell'interfaccia utente. L'HTML ha due tipi di controlli a discesa: il **box di selezione** e il **box di completamento automatico**. L'interazione è la stessa in entrambi i tipi di controlli a discesa — dopo l'attivazione del controllo, il browser visualizza un elenco di valori tra cui l'utente può selezionare.

> [!NOTE]
> Puoi trovare esempi di tutti i tipi di box a discesa su GitHub in [drop-down-content.html](https://github.com/mdn/learning-area/blob/main/html/forms/native-form-widgets/drop-down-content.html) ([vedi anche in tempo reale](https://mdn.github.io/learning-area/html/forms/native-form-widgets/drop-down-content.html)).

### Box di selezione

Un semplice box di selezione è creato con un elemento {{HTMLElement("select")}} con uno o più elementi {{HTMLElement("option")}} come suoi figli, ciascuno dei quali specifica uno dei suoi possibili valori.

#### Esempio base

```html
<select id="simple" name="simple">
  <option>Banana</option>
  <option selected>Cherry</option>
  <option>Lemon</option>
</select>
```

{{EmbedLiveSample("Basic_example", 120, 120)}}

Se richiesto, il valore predefinito per il box di selezione può essere impostato usando l'attributo [`selected`](/it/docs/Web/HTML/Reference/Elements/option#selected) sull'elemento {{HTMLElement("option")}} desiderato — questa opzione è quindi preselezionata quando la pagina si carica.

#### Uso di optgroup

Gli elementi {{HTMLElement("option")}} possono essere annidati all'interno di elementi {{HTMLElement("optgroup")}} per creare gruppi di valori visivamente associati:

```html
<select id="groups" name="groups">
  <optgroup label="fruits">
    <option>Banana</option>
    <option selected>Cherry</option>
    <option>Lemon</option>
  </optgroup>
  <optgroup label="vegetables">
    <option>Carrot</option>
    <option>Eggplant</option>
    <option>Potato</option>
  </optgroup>
</select>
```

{{EmbedLiveSample("Using_optgroup", 120, 120)}}

Sull'elemento {{HTMLElement("optgroup")}}, il valore dell'attributo [`label`](/it/docs/Web/HTML/Reference/Elements/optgroup#label) è visualizzato prima dei valori delle opzioni annidate. Il browser di solito le distingue visivamente dalle opzioni (ossia, mettendole in grassetto e a un diverso livello di annidamento) in modo che sia meno probabile che vengano confuse per opzioni effettive.

#### Uso dell'attributo value

Se un elemento {{HTMLElement("option")}} ha un attributo `value` esplicito impostato su di esso, quel valore viene inviato quando il form viene inviato con quell'opzione selezionata. Se l'attributo `value` è omesso, come negli esempi sopra, il contenuto dell'elemento {{HTMLElement("option")}} è usato come valore. Quindi gli attributi `value` non sono necessari, ma potresti trovare un motivo per voler inviare un valore ridotto o diverso al server rispetto a quello che è visivamente mostrato nel box di selezione.

Per esempio:

```html
<select id="simple" name="simple">
  <option value="banana">Big, beautiful yellow banana</option>
  <option value="cherry">Succulent, juicy cherry</option>
  <option value="lemon">Sharp, powerful lemon</option>
</select>
```

Per impostazione predefinita, l'altezza del box di selezione è sufficiente a mostrare un singolo valore. L'attributo [`size`](/it/docs/Web/HTML/Reference/Attributes/size) opzionale fornisce controllo su quante opzioni sono visibili quando il selettore non è a fuoco.

### Box di selezione a scelta multipla

Per impostazione predefinita, un box di selezione consente a un utente di selezionare solo un valore. Aggiungendo l'attributo [`multiple`](/it/docs/Web/HTML/Reference/Elements/select#multiple) all'elemento {{HTMLElement("select")}}, puoi permettere agli utenti di selezionare diversi valori. Gli utenti possono selezionare valori multipli usando il meccanismo predefinito fornito dal sistema operativo (ad esempio, sul desktop, possono essere cliccati valori multipli tenendo premuti i tasti <kbd>Cmd</kbd>/<kbd>Ctrl</kbd>).

```html
<select id="multi" name="multi" multiple size="2">
  <optgroup label="fruits">
    <option>Banana</option>
    <option selected>Cherry</option>
    <option>Lemon</option>
  </optgroup>
  <optgroup label="vegetables">
    <option>Carrot</option>
    <option>Eggplant</option>
    <option>Potato</option>
  </optgroup>
</select>
```

{{EmbedLiveSample("Multiple_choice_select_box", 120, 120)}}

> [!NOTE]
> Nel caso di box di selezione a scelta multipla, noterai che il box di selezione non visualizza più i valori come contenuto a discesa — invece tutti i valori sono visualizzati contemporaneamente in un elenco, con l'attributo [`size`](/it/docs/Web/HTML/Reference/Attributes/size) opzionale che determina l'altezza del widget.

> [!NOTE]
> Tutti i browser che supportano l'elemento {{HTMLElement("select")}} supportano anche l'attributo [`multiple`](/it/docs/Web/HTML/Reference/Elements/select#multiple).

### Box di completamento automatico

Puoi fornire valori suggeriti e automaticamente completati per i widget formato usando l'elemento {{HTMLElement("datalist")}} con elementi figlio {{HTMLElement("option")}} per specificare i valori da visualizzare. Il `<datalist>` deve essere assegnato un `id`.

Il data list è poi legato a un elemento {{htmlelement("input")}} (ad esempio, un tipo di input `text` o `email`) usando l'attributo [`list`](/it/docs/Web/HTML/Reference/Elements/input#list), il cui valore è l'`id` del data list da legare.

Una volta che un data list è associato a un widget di un form, le sue opzioni sono utilizzate per completare automaticamente il testo inserito dall'utente; tipicamente, questo è presentato all'utente come un box a discesa che elenca i possibili corrispondenti a ciò che hanno digitato nell'input.

#### Esempio base

Osserviamo un esempio.

```html
<label for="myFruit">What's your favorite fruit?</label>
<input type="text" name="myFruit" id="myFruit" list="mySuggestion" />
<datalist id="mySuggestion">
  <option>Apple</option>
  <option>Banana</option>
  <option>Blackberry</option>
  <option>Blueberry</option>
  <option>Lemon</option>
  <option>Lychee</option>
  <option>Peach</option>
  <option>Pear</option>
</datalist>
```

{{EmbedLiveSample("Basic_example_2", 120, 120)}}

#### Usi meno ovvi di datalist

Secondo [la specifica HTML](https://html.spec.whatwg.org/multipage/input.html#attr-input-list), l'attributo [`list`](/it/docs/Web/HTML/Reference/Elements/input#list) e l'elemento {{HTMLElement("datalist")}} possono essere usati con qualsiasi tipo di widget che richieda un input utente. Questo porta a degli usi di esso che potrebbero sembrare un po' non ovvi.

Ad esempio, nei browser che supportano `{{htmlelement("datalist")}}` sui tipi di input `range`, un piccolo segno di spunta sarà visualizzato sopra il range per ciascun valore `{{htmlelement("option")}}` del datalist. Puoi vedere un'implementazione [esempio di questo nella pagina di riferimento `<input type="range">`](/it/docs/Web/HTML/Reference/Elements/input/range#adding_tick_marks).

E i browser che supportano {{htmlelement('datalist')}}s e [`<input type="color">`](/it/docs/Web/HTML/Reference/Elements/input/color) dovrebbero visualizzare una tavolozza di colori personalizzata come predefinita, mentre mantengono comunque disponibile l'intera tavolozza dei colori.

In questo caso, i diversi browser si comportano in modo diverso da caso a caso, quindi considera tali utilizzi come un miglioramento progressivo, e assicurati che degradano con grazia.

## Altre caratteristiche del form

Ci sono alcune altre caratteristiche del form che non sono così ovvie come quelle che abbiamo già menzionato, ma comunque utili in alcune situazioni, quindi abbiamo pensato che valesse la pena dar loro un breve accenno.

> [!NOTE]
> Puoi trovare gli esempi di questa sezione su GitHub come [other-examples.html](https://github.com/mdn/learning-area/blob/main/html/forms/native-form-widgets/other-examples.html) ([vedi anche in tempo reale](https://mdn.github.io/learning-area/html/forms/native-form-widgets/other-examples.html)).

### Metri e barre di progresso

Metri e barre di progresso sono rappresentazioni visive di valori numerici. Il supporto per {{HTMLElement("progress")}} e {{HTMLElement("meter")}} è disponibile in tutti i browser moderni.

#### Meter

Un'asta del metro rappresenta un valore fisso in un intervallo delimitato dai valori [`max`](/it/docs/Web/HTML/Reference/Elements/meter#max) e [`min`](/it/docs/Web/HTML/Reference/Elements/meter#min). Questo valore è reso visualmente come un'asta, e per sapere come appare questa asta, confrontiamo il valore con altri valori impostati:

- I valori [`low`](/it/docs/Web/HTML/Reference/Elements/meter#low) e [`high`](/it/docs/Web/HTML/Reference/Elements/meter#high) dividono l'intervallo nei seguenti tre parti:

  - La parte inferiore dell'intervallo è compresa tra i valori [`min`](/it/docs/Web/HTML/Reference/Elements/meter#min) e [`low`](/it/docs/Web/HTML/Reference/Elements/meter#low), inclusi.
  - La parte media dell'intervallo è compresa tra i valori [`low`](/it/docs/Web/HTML/Reference/Elements/meter#low) e [`high`](/it/docs/Web/HTML/Reference/Elements/meter#high), esclusi.
  - La parte superiore dell'intervallo è compresa tra i valori [`high`](/it/docs/Web/HTML/Reference/Elements/meter#high) e [`max`](/it/docs/Web/HTML/Reference/Elements/meter#max), inclusi.

- Il valore [`optimum`](/it/docs/Web/HTML/Reference/Elements/meter#optimum) definisce il valore ottimale per l'elemento {{HTMLElement("meter")}}. In congiunzione con i valori [`low`](/it/docs/Web/HTML/Reference/Elements/meter#low) e [`high`](/it/docs/Web/HTML/Reference/Elements/meter#high), definisce quale parte dell'intervallo è preferita:

  - Se il valore [`optimum`](/it/docs/Web/HTML/Reference/Elements/meter#optimum) è nella parte inferiore dell'intervallo, la gamma inferiore è considerata la parte preferita, la gamma media è considerata la parte media e la gamma superiore è considerata quella peggiore.
  - Se il valore [`optimum`](/it/docs/Web/HTML/Reference/Elements/meter#optimum) è nella parte media dell'intervallo, la gamma inferiore è considerata una parte media, la gamma media è considerata la parte preferita e la gamma superiore è considerata media.
  - Se il valore [`optimum`](/it/docs/Web/HTML/Reference/Elements/meter#optimum) è nella parte superiore dell'intervallo, la gamma inferiore è considerata la parte peggiore, la gamma media è considerata quella media e la gamma superiore è considerata la parte preferita.

Tutti i browser che implementano l'elemento {{HTMLElement("meter")}} utilizzano questi valori per cambiare il colore dell'asta del metro:

- Se il valore corrente è nella parte preferita dell'intervallo, l'asta è verde.
- Se il valore corrente è nella parte media dell'intervallo, l'asta è gialla.
- Se il valore corrente è nella parte peggiore dell'intervallo, l'asta è rossa.

Tale asta è creata utilizzando l'elemento {{HTMLElement("meter")}}. Questo è per implementare qualsiasi tipo di metro; ad esempio, un'asta che mostri lo spazio totale utilizzato su un disco, che diventa rossa quando inizia a riempirsi.

```html
<meter min="0" max="100" value="75" low="33" high="66" optimum="0">75</meter>
```

{{EmbedLiveSample("Meter", 120, 120)}}

Il contenuto all'interno dell'elemento {{HTMLElement("meter")}} è un'alternativa per i browser che non supportano l'elemento e per le tecnologie assistive a vocalizzarlo.

#### Progress

Una barra di progresso rappresenta un valore che cambia nel tempo fino a un valore massimo specificato dall'attributo [`max`](/it/docs/Web/HTML/Reference/Elements/progress#max). Tale barra è creata usando un elemento {{HTMLElement("progress")}}.

```html
<progress max="100" value="75">75/100</progress>
```

{{EmbedLiveSample("Progress", 120, 120)}}

Questo è per implementare qualsiasi cosa richieda una segnalazione di progresso, come la percentuale di file totali scaricati, o il numero di domande compilate su un questionario.

Il contenuto all'interno dell'elemento {{HTMLElement("progress")}} è un'alternativa per i browser che non supportano l'elemento e per gli screen reader a vocalizzarlo.

## Metti alla prova le tue abilità!

Hai raggiunto la fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare alcuni ulteriori test per verificare che tu abbia conservato queste informazioni prima di procedere — vedi [Metti alla prova le tue abilità: Altri controlli](/it/docs/Learn_web_development/Extensions/Forms/Test_your_skills/Other_controls).

## Sommario

Come avrai visto negli ultimi articoli, ci sono molti tipi di controlli per i form. Non devi ricordare tutti questi dettagli in una volta sola, e puoi tornare a questi articoli tutte le volte che vuoi per controllare i dettagli.

Ora che hai compreso l'HTML dietro i diversi controlli dei form disponibili, daremo uno sguardo a [Come stilizzarli](/it/docs/Learn_web_development/Extensions/Forms/Styling_web_forms).

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/HTML5_input_types","Learn_web_development/Extensions/Forms/Styling_web_forms", "Learn_web_development/Extensions/Forms")}}
