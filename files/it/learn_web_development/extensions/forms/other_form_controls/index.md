---
title: Altri controlli del modulo
slug: Learn_web_development/Extensions/Forms/Other_form_controls
l10n:
  sourceCommit: a1ac64fa4da965d2a152f08221b1a9aed638fd16
---

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/HTML5_input_types","Learn_web_development/Extensions/Forms/Styling_web_forms", "Learn_web_development/Extensions/Forms")}}

Ora esamineremo nel dettaglio la funzionalità degli elementi del modulo non-`<input>`, da altri tipi di controlli come le liste a discesa e i campi di testo multilinea, ad altre utili funzionalità del modulo come l'elemento {{htmlelement('output')}} (che abbiamo visto in azione nell'articolo precedente) e le barre di avanzamento.

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
        Comprendere le funzionalità del modulo non-<code>&#x3C;input></code> e come
        implementarle utilizzando HTML.
      </td>
    </tr>
  </tbody>
</table>

## Campi di testo multilinea

Un campo di testo multilinea è specificato utilizzando un elemento {{HTMLElement("textarea")}}, invece di utilizzare l'elemento {{HTMLElement("input")}}.

```html
<textarea cols="30" rows="8"></textarea>
```

Questo verrà reso così:

{{EmbedLiveSample("Multi-line_text_fields", 120, 160)}}

La principale differenza tra un `<textarea>` e un normale campo di testo a riga singola è che agli utenti è consentito includere interruzioni di riga dure (ad esempio, premendo invio) che saranno incluse quando i dati vengono inviati.

`<textarea>` richiede anche un tag di chiusura; qualsiasi testo predefinito che si desidera contenere dovrebbe essere inserito tra i tag di apertura e chiusura. Al contrario, la {{HTMLElement("input")}} è un {{Glossary("void_element", "elemento void")}} senza tag di chiusura — qualsiasi valore predefinito viene inserito all'interno dell'attributo [`value`](/it/docs/Web/HTML/Reference/Elements/input#value).

Si noti che, anche se si può mettere qualsiasi cosa dentro un elemento `<textarea>` (inclusi altri elementi HTML, CSS e JavaScript), per sua natura, tutto è reso come se fosse contenuto di testo semplice. (Utilizzando [`contenteditable`](/it/docs/Web/HTML/Reference/Global_attributes/contenteditable) su controlli non di modulo fornisce un'API per acquisire contenuti HTML/"ricchi" invece di testo semplice).

Visivamente, il testo inserito si avvolge e il controllo del modulo è di default ridimensionabile. La maggior parte dei browser fornisce un'impugnatura da trascinare per aumentare/ridurre le dimensioni dell'area di testo.

È possibile trovare un esempio di utilizzo del textarea nell'[esempio](https://mdn.github.io/learning-area/html/forms/your-first-HTML-form/first-form-styled.html) che abbiamo messo insieme nel primo articolo di questo.

### Controllo del rendering multilinea

{{htmlelement("textarea")}} accetta tre attributi per controllare il suo rendering su più righe:

- [`cols`](/it/docs/Web/HTML/Reference/Elements/textarea#cols)
  - : Specifica la larghezza visibile (colonne) del controllo di testo, misurata in larghezze medie di caratteri. Questo è in effetti la larghezza iniziale, poiché può essere cambiata ridimensionando il `<textarea>`, e sovrascritta usando CSS. Il valore predefinito, se non specificato, è 20.
- [`rows`](/it/docs/Web/HTML/Reference/Elements/textarea#rows)
  - : Specifica il numero di righe di testo visibili per il controllo. Questo è in effetti l'altezza iniziale, poiché può essere cambiata ridimensionando il `<textarea>`, e sovrascritta usando CSS. Il valore predefinito, se non specificato, è 2.
- [`wrap`](/it/docs/Web/HTML/Reference/Elements/textarea#wrap)
  - : Specifica come il controllo avvolge il testo. I valori sono `soft` (il valore predefinito), il che significa che il testo inviato non è avvolto ma il testo reso dal browser è avvolto; `hard` (l'attributo `cols` deve essere specificato quando si utilizza questo valore), il che significa che sia il testo inviato sia quello reso sono avvolti, e `off`, che ferma l'avvolgimento.

### Controllo del ridimensionamento del textarea

La possibilità di ridimensionare un `<textarea>` è controllata con la proprietà CSS `resize`. I suoi possibili valori sono:

- `both`: Il valore predefinito — consente il ridimensionamento orizzontale e verticale.
- `horizontal`: Consente il ridimensionamento solo orizzontale.
- `vertical`: Consente il ridimensionamento solo verticale.
- `none`: Non consente il ridimensionamento.
- `block` e `inline`: Valori sperimentali che consentono il ridimensionamento solo nella direzione `block` o `inline` (questo varia a seconda della direzione del testo; legga [Gestione di direzioni di testo differenti](/it/docs/Learn_web_development/Core/Styling_basics/Handling_different_text_directions) se desidera saperne di più).

Gioca con l'esempio interattivo in cima alla pagina di riferimento {{cssxref("resize")}} per una dimostrazione di come funzionano.

## Controlli a discesa

I controlli a discesa sono un modo semplice per consentire agli utenti di selezionare tra molte opzioni senza occupare molto spazio nell'interfaccia utente. HTML ha due tipi di controlli a discesa: il **select box** e la **casella di completamento automatico**. L'interazione è la stessa in entrambi i tipi di controlli a discesa — dopo che il controllo è stato attivato, il browser visualizza un elenco di valori da cui l'utente può selezionare.

> [!NOTE]
> Può trovare esempi di tutti i tipi di casella a discesa su GitHub in [drop-down-content.html](https://github.com/mdn/learning-area/blob/main/html/forms/native-form-widgets/drop-down-content.html) ([vedi anche dal vivo](https://mdn.github.io/learning-area/html/forms/native-form-widgets/drop-down-content.html)).

### Select box

Una semplice select box è creata con un elemento {{HTMLElement("select")}} con uno o più elementi {{HTMLElement("option")}} come suoi figli, ciascuno dei quali specifica uno dei suoi possibili valori.

#### Esempio di base

```html
<select id="simple" name="simple">
  <option>Banana</option>
  <option selected>Cherry</option>
  <option>Lemon</option>
</select>
```

{{EmbedLiveSample("Basic_example", 120, 120)}}

Se necessario, il valore predefinito per la select box può essere impostato utilizzando l'attributo [`selected`](/it/docs/Web/HTML/Reference/Elements/option#selected) sull'elemento {{HTMLElement("option")}} desiderato — quest'opzione è quindi preselezionata al caricamento della pagina.

#### Utilizzo di optgroup

Gli elementi {{HTMLElement("option")}} possono essere nidificati all'interno di elementi {{HTMLElement("optgroup")}} per creare gruppi di valori visivamente associati:

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

Sull'elemento {{HTMLElement("optgroup")}}, il valore dell'attributo [`label`](/it/docs/Web/HTML/Reference/Elements/optgroup#label) è visualizzato prima dei valori delle opzioni annidate. Il browser di solito li distingue visivamente dalle opzioni (ad esempio, rendendoli in grassetto e a un diverso livello di nidificazione) in modo che siano meno propensi a essere confusi per opzioni effettive.

#### Utilizzo dell'attributo value

Se un elemento {{HTMLElement("option")}} ha un attributo `value` esplicito impostato su di esso, quel valore viene inviato quando il modulo viene inviato con quell'opzione selezionata. Se l'attributo `value` è omesso, come nei precedenti esempi, il contenuto dell'elemento {{HTMLElement("option")}} viene utilizzato come valore. Quindi gli attributi `value` non sono necessari, ma potresti trovare un motivo per voler inviare un valore abbreviato o diverso al server rispetto a quanto visualmente mostrato nella select box.

Ad esempio:

```html
<select id="simple" name="simple">
  <option value="banana">Big, beautiful yellow banana</option>
  <option value="cherry">Succulent, juicy cherry</option>
  <option value="lemon">Sharp, powerful lemon</option>
</select>
```

Di default, l'altezza della select box è sufficiente per visualizzare un singolo valore. L'attributo opzionale [`size`](/it/docs/Web/HTML/Reference/Attributes/size) fornisce un controllo su quanti sono i valori visibili quando non ha il focus.

### Select box a scelta multipla

Di default, una select box consente a un utente di selezionare solo un valore. Aggiungendo l'attributo [`multiple`](/it/docs/Web/HTML/Reference/Elements/select#multiple) all'elemento {{HTMLElement("select")}}, si può consentire agli utenti di selezionare più valori. Gli utenti possono selezionare più valori utilizzando il meccanismo predefinito fornito dal sistema operativo (ad esempio, sul desktop, più valori possono essere selezionati cliccando tenendo premuti i tasti <kbd>Cmd</kbd>/<kbd>Ctrl</kbd>).

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
> Nel caso delle select box a scelta multipla, si noterà che la select box non visualizza più i valori come contenuto a discesa — invece, tutti i valori vengono visualizzati contemporaneamente in un elenco, con l'attributo opzionale [`size`](/it/docs/Web/HTML/Reference/Attributes/size) che determina l'altezza del widget.

> [!NOTE]
> Tutti i browser che supportano l'elemento {{HTMLElement("select")}} supportano anche l'attributo [`multiple`](/it/docs/Web/HTML/Reference/Elements/select#multiple).

### Casella di completamento automatico

Si possono fornire valori suggeriti e completati automaticamente per i widget del modulo usando l'elemento {{HTMLElement("datalist")}} con elementi figli {{HTMLElement("option")}} per specificare i valori da visualizzare. Il `<datalist>` deve essere dato un `id`.

L'elenco dei dati è poi legato a un elemento {{htmlelement("input")}} (ad esempio, un tipo di input `text` o `email`) usando l'attributo [`list`](/it/docs/Web/HTML/Reference/Elements/input#list), il cui valore è l'`id` della lista di dati da collegare.

Una volta che una lista di dati è affiliata a un widget del modulo, le sue opzioni vengono utilizzate per completare automaticamente il testo inserito dall'utente; in genere, questo viene presentato all'utente come un elenco a discesa che elenca possibili corrispondenze per ciò che è stato digitato nel campo di input.

#### Esempio di base

Vediamo un esempio.

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

#### Usi meno ovvi del datalist

Secondo [la specifica HTML](https://html.spec.whatwg.org/multipage/input.html#attr-input-list), l'attributo [`list`](/it/docs/Web/HTML/Reference/Elements/input#list) e l'elemento {{HTMLElement("datalist")}} possono essere usati con qualsiasi tipo di widget che richiede un input dell'utente. Questo porta ad alcuni usi di esso che potrebbero sembrare non ovvi.

Ad esempio, nei browser che supportano `{{htmlelement("datalist")}}` sui tipi di input `range`, verrà visualizzato un piccolo segno di spunta sopra l'intervallo per ciascun valore di `{{htmlelement("option")}}` della lista di dati. Puoi vedere un esempio di implementazione [nella pagina di riferimento `<input type="range">`](/it/docs/Web/HTML/Reference/Elements/input/range#adding_tick_marks).

E i browser che supportano i {{htmlelement('datalist')}} e gli [`<input type="color">`](/it/docs/Web/HTML/Reference/Elements/input/color) dovrebbero visualizzare una palette di colori personalizzata come predefinita, pur rendendo disponibile l'intera palette di colori.

In questo caso, i diversi browser si comportano diversamente di caso in caso, quindi considera tali usi come potenziamenti progressivi e assicurati che degradino con grazia.

## Altre funzionalità del modulo

Ci sono alcune altre funzionalità del modulo che non sono così ovvie come quelle che abbiamo già menzionato, ma ancora utili in alcune situazioni, quindi abbiamo pensato che valesse la pena dar loro una breve menzione.

> [!NOTE]
> Può trovare gli esempi di questa sezione su GitHub come [other-examples.html](https://github.com/mdn/learning-area/blob/main/html/forms/native-form-widgets/other-examples.html) ([vedi anche dal vivo](https://mdn.github.io/learning-area/html/forms/native-form-widgets/other-examples.html)).

### Meters e barre di avanzamento

I meters e le barre di avanzamento sono rappresentazioni visive di valori numerici. Il supporto per {{HTMLElement("progress")}} e {{HTMLElement("meter")}} è disponibile in tutti i browser moderni.

#### Meter

Una barra meter rappresenta un valore fisso in un intervallo delimitato dai valori [`max`](/it/docs/Web/HTML/Reference/Elements/meter#max) e [`min`](/it/docs/Web/HTML/Reference/Elements/meter#min). Questo valore è visivamente reso come una barra e per sapere come appare, confrontiamo il valore con un altro set di valori:

- I valori [`low`](/it/docs/Web/HTML/Reference/Elements/meter#low) e [`high`](/it/docs/Web/HTML/Reference/Elements/meter#high) dividono l'intervallo nelle seguenti tre parti:

  - La parte inferiore dell'intervallo è tra i valori [`min`](/it/docs/Web/HTML/Reference/Elements/meter#min) e [`low`](/it/docs/Web/HTML/Reference/Elements/meter#low), inclusiva.
  - La parte media dell'intervallo è tra i valori [`low`](/it/docs/Web/HTML/Reference/Elements/meter#low) e [`high`](/it/docs/Web/HTML/Reference/Elements/meter#high), esclusiva.
  - La parte superiore dell'intervallo è tra i valori [`high`](/it/docs/Web/HTML/Reference/Elements/meter#high) e [`max`](/it/docs/Web/HTML/Reference/Elements/meter#max), inclusiva.

- Il valore [`optimum`](/it/docs/Web/HTML/Reference/Elements/meter#optimum) definisce il valore ottimale per l'elemento {{HTMLElement("meter")}}. In combinazione con il valore [`low`](/it/docs/Web/HTML/Reference/Elements/meter#low) e il valore [`high`](/it/docs/Web/HTML/Reference/Elements/meter#high), definisce quale parte dell'intervallo è preferita:

  - Se il valore [`optimum`](/it/docs/Web/HTML/Reference/Elements/meter#optimum) è nella parte inferiore dell'intervallo, la parte inferiore è considerata come la parte preferita, la parte media è considerata come la parte media, e la parte superiore è considerata come la parte peggiore.
  - Se il valore [`optimum`](/it/docs/Web/HTML/Reference/Elements/meter#optimum) è nella parte media dell'intervallo, la parte inferiore è considerata come parte media, la parte media è considerata come parte preferita, e anche la parte superiore è considerata come media.
  - Se il valore [`optimum`](/it/docs/Web/HTML/Reference/Elements/meter#optimum) è nella parte superiore dell'intervallo, la parte inferiore è considerata come la parte peggiore, la parte media è considerata come parte media e la parte superiore è considerata come parte preferita.

Tutti i browser che implementano l'elemento {{HTMLElement("meter")}} usano quei valori per cambiare il colore della barra del meter:

- Se il valore corrente è nella parte preferita dell'intervallo, la barra è verde.
- Se il valore corrente è nella parte media dell'intervallo, la barra è gialla.
- Se il valore corrente è nella parte peggiore dell'intervallo, la barra è rossa.

Una tale barra è creata usando l'elemento {{HTMLElement("meter")}}. Questo è per implementare qualsiasi tipo di meter; ad esempio, una barra che mostra lo spazio totale utilizzato su un disco, che diventa rossa quando inizia a riempirsi.

```html
<meter min="0" max="100" value="75" low="33" high="66" optimum="0">75</meter>
```

{{EmbedLiveSample("Meter", 120, 120)}}

Il contenuto all'interno dell'elemento {{HTMLElement("meter")}} è una soluzione alternativa per i browser che non supportano l'elemento e per le tecnologie assistive per verbalizzarlo.

#### Progress

Una barra di avanzamento rappresenta un valore che cambia nel tempo fino a un valore massimo specificato dall'attributo [`max`](/it/docs/Web/HTML/Reference/Elements/progress#max). Una tale barra è creata usando un elemento {{ HTMLElement("progress")}}.

```html
<progress max="100" value="75">75/100</progress>
```

{{EmbedLiveSample("Progress", 120, 120)}}

Questo è per implementare qualsiasi cosa richieda la segnalazione del progresso, come la percentuale di file totali scaricati, o il numero di domande compilate in un questionario.

Il contenuto all'interno dell'elemento {{HTMLElement("progress")}} è una soluzione alternativa per i browser che non supportano l'elemento e per i lettori di schermo per verbalizzarlo.

## Metti alla prova le sue abilità!

Hai raggiunto la fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Può trovare ulteriori test per verificare di aver assimilato queste informazioni prima di procedere — vedi [Testa le tue abilità: Altri controlli](/it/docs/Learn_web_development/Extensions/Forms/Test_your_skills/Other_controls).

## Riepilogo

Come ha visto negli ultimi articoli, ci sono molti tipi di controlli del modulo. Non c'è bisogno di ricordare tutti questi dettagli subito, e può tornare a questi articoli il numero di volte che desidera per verificare i dettagli.

Ora che ha una comprensione dell'HTML dietro i diversi controlli del modulo disponibili, daremo un'occhiata a [come stilizzarli](/it/docs/Learn_web_development/Extensions/Forms/Styling_web_forms).

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/HTML5_input_types","Learn_web_development/Extensions/Forms/Styling_web_forms", "Learn_web_development/Extensions/Forms")}}
