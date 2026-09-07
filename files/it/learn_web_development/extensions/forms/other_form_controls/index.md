---
title: Altri controlli dei moduli
slug: Learn_web_development/Extensions/Forms/Other_form_controls
l10n:
  sourceCommit: 2066cc916dfdcbb782340bf0ce562b230e947cba
---

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/HTML5_input_types","Learn_web_development/Extensions/Forms/Styling_web_forms", "Learn_web_development/Extensions/Forms")}}

Ora esamineremo nel dettaglio la funzionalità degli elementi di modulo diversi da `<input>`, da altri tipi di controlli come gli elenchi a discesa e i campi di testo su più righe, ad altre utili funzionalità dei moduli come l'elemento {{htmlelement('output')}} (che abbiamo visto in azione nell'articolo precedente) e le barre di avanzamento.

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
        Comprendere le funzionalità dei moduli diverse da <code>&#x3C;input></code> e
        come implementarle usando HTML.
      </td>
    </tr>
  </tbody>
</table>

## Campi di testo su più righe

Un campo di testo su più righe viene specificato usando un elemento {{HTMLElement("textarea")}}, anziché l'elemento {{HTMLElement("input")}}.

```html
<textarea cols="30" rows="8"></textarea>
```

Il rendering è il seguente:

{{EmbedLiveSample("Multi-line_text_fields", 120, 160)}}

La differenza principale tra un `<textarea>` e un normale campo di testo a riga singola è che gli utenti possono includere interruzioni di riga esplicite (ovvero, premendo Invio), che verranno incluse quando i dati vengono inviati.

`<textarea>` richiede anche un tag di chiusura; qualsiasi testo predefinito che deve contenere va inserito tra il tag di apertura e quello di chiusura. Al contrario, {{HTMLElement("input")}} è un {{Glossary("void_element", "elemento void")}} senza tag di chiusura: qualsiasi valore predefinito viene inserito nell'attributo [`value`](/it/docs/Web/HTML/Reference/Elements/input#value).

Si noti che, anche se è possibile inserire qualsiasi contenuto all'interno di un elemento `<textarea>` (inclusi altri elementi HTML, CSS e JavaScript), per sua natura viene interamente visualizzato come se fosse contenuto di testo semplice. (L'uso di [`contenteditable`](/it/docs/Web/HTML/Reference/Global_attributes/contenteditable) su controlli non appartenenti ai moduli fornisce un'API per acquisire contenuto HTML/"ricco" anziché testo semplice.)

Visivamente, il testo inserito va a capo e il controllo del modulo è ridimensionabile per impostazione predefinita. La maggior parte dei browser fornisce una maniglia di trascinamento che può essere trascinata per aumentare o diminuire le dimensioni dell'area di testo.

### Controllare il rendering su più righe

{{htmlelement("textarea")}} accetta tre attributi per controllarne il rendering su più righe:

- [`cols`](/it/docs/Web/HTML/Reference/Elements/textarea#cols)
  - : Specifica la larghezza visibile (in colonne) del controllo di testo, misurata in larghezze medie dei caratteri. Si tratta in effetti della larghezza iniziale, poiché può essere modificata ridimensionando il `<textarea>` e sovrascritta tramite CSS. Il valore predefinito, se non ne viene specificato alcuno, è 20.
- [`rows`](/it/docs/Web/HTML/Reference/Elements/textarea#rows)
  - : Specifica il numero di righe di testo visibili per il controllo. Si tratta in effetti dell'altezza iniziale, poiché può essere modificata ridimensionando il `<textarea>` e sovrascritta tramite CSS. Il valore predefinito, se non ne viene specificato alcuno, è 2.
- [`wrap`](/it/docs/Web/HTML/Reference/Elements/textarea#wrap)
  - : Specifica come il controllo manda a capo il testo. I valori sono `soft` (il valore predefinito), che indica che il testo inviato non viene mandato a capo ma il testo renderizzato dal browser sì; `hard` (quando si usa questo valore è necessario specificare l'attributo `cols`), che indica che sia il testo inviato sia quello renderizzato vengono mandati a capo; e `off`, che disabilita l'andata a capo.

### Controllare la ridimensionabilità di textarea

La possibilità di ridimensionare un `<textarea>` è controllata dalla proprietà CSS `resize`. I valori possibili sono:

- `both`: il valore predefinito, consente il ridimensionamento orizzontale e verticale.
- `horizontal`: consente il ridimensionamento solo orizzontale.
- `vertical`: consente il ridimensionamento solo verticale.
- `none`: non consente alcun ridimensionamento.
- `block` e `inline`: valori sperimentali che consentono il ridimensionamento solo nella direzione `block` o `inline` (questo varia in base alla direzionalità del testo; per ulteriori informazioni, leggere [Gestire diverse direzioni del testo](/it/docs/Learn_web_development/Core/Styling_basics/Handling_different_text_directions)).

Per una dimostrazione del loro funzionamento, sperimentare con l'esempio interattivo nella parte superiore della pagina di riferimento di {{cssxref("resize")}}.

## Controlli a discesa

I controlli a discesa sono un modo semplice per consentire agli utenti di selezionare tra molte opzioni senza occupare molto spazio nell'interfaccia utente. HTML dispone di due tipi di controlli a discesa: la **casella di selezione** e la **casella di completamento automatico**. L'interazione è la stessa in entrambi i tipi di controlli a discesa: dopo l'attivazione del controllo, il browser visualizza un elenco di valori da cui l'utente può selezionare.

### Casella di selezione

Una semplice casella di selezione viene creata con un elemento {{HTMLElement("select")}} con uno o più elementi {{HTMLElement("option")}} come elementi figli, ciascuno dei quali specifica uno dei possibili valori.

#### Esempio di base

```html
<select id="simple" name="simple">
  <option>Banana</option>
  <option selected>Cherry</option>
  <option>Lemon</option>
</select>
```

{{EmbedLiveSample("Basic_example", 120, 120)}}

Se necessario, il valore predefinito della casella di selezione può essere impostato usando l'attributo [`selected`](/it/docs/Web/HTML/Reference/Elements/option#selected) sull'elemento {{HTMLElement("option")}} desiderato: questa opzione viene quindi preselezionata al caricamento della pagina.

#### Usare optgroup

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

Nell'elemento {{HTMLElement("optgroup")}}, il valore dell'attributo [`label`](/it/docs/Web/HTML/Reference/Elements/optgroup#label) viene visualizzato prima dei valori delle opzioni annidate. Il browser di solito li distingue visivamente dalle opzioni (ad esempio, in grassetto e a un diverso livello di annidamento), così che sia meno probabile confonderli con opzioni effettive.

#### Usare l'attributo value

Se un elemento {{HTMLElement("option")}} dispone di un attributo `value` esplicito, quel valore viene inviato quando il modulo viene inviato con tale opzione selezionata. Se l'attributo `value` viene omesso, come negli esempi precedenti, viene usato come valore il contenuto dell'elemento {{HTMLElement("option")}}. Gli attributi `value` non sono quindi necessari, ma potrebbe essere opportuno inviare al server un valore abbreviato o diverso da quello mostrato visivamente nella casella di selezione.

Ad esempio:

```html
<select id="simple" name="simple">
  <option value="banana">Big, beautiful yellow banana</option>
  <option value="cherry">Succulent, juicy cherry</option>
  <option value="lemon">Sharp, powerful lemon</option>
</select>
```

Per impostazione predefinita, l'altezza della casella di selezione è sufficiente per visualizzare un singolo valore. L'attributo facoltativo [`size`](/it/docs/Web/HTML/Reference/Attributes/size) consente di controllare quante opzioni sono visibili quando il controllo select non ha il focus.

### Casella di selezione a scelta multipla

Per impostazione predefinita, una casella di selezione consente all'utente di selezionare un solo valore. Aggiungendo l'attributo [`multiple`](/it/docs/Web/HTML/Reference/Elements/select#multiple) all'elemento {{HTMLElement("select")}}, è possibile consentire agli utenti di selezionare più valori. Gli utenti possono selezionare più valori usando il meccanismo predefinito fornito dal sistema operativo (ad esempio, su desktop è possibile fare clic su più valori tenendo premuti i tasti <kbd>Cmd</kbd>/<kbd>Ctrl</kbd>).

```html
<select id="multi" name="multi" multiple size="3">
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
> Nel caso delle caselle di selezione a scelta multipla, si noterà che la casella non visualizza più i valori come contenuto a discesa: al contrario, tutti i valori vengono visualizzati contemporaneamente in un elenco e l'attributo facoltativo [`size`](/it/docs/Web/HTML/Reference/Attributes/size) determina l'altezza del widget.

> [!NOTE]
> Tutti i browser che supportano l'elemento {{HTMLElement("select")}} supportano anche l'attributo [`multiple`](/it/docs/Web/HTML/Reference/Elements/select#multiple).

### Casella di completamento automatico

È possibile fornire valori suggeriti e completati automaticamente per i widget dei moduli usando l'elemento {{HTMLElement("datalist")}} con elementi {{HTMLElement("option")}} figli per specificare i valori da visualizzare. A `<datalist>` deve essere assegnato un `id`.

L'elenco di dati viene quindi associato a un elemento {{htmlelement("input")}} (ad esempio, un tipo di input `text` o `email`) usando l'attributo [`list`](/it/docs/Web/HTML/Reference/Elements/input#list), il cui valore è l'`id` dell'elenco di dati da associare.

Una volta che un elenco di dati è associato a un widget del modulo, le sue opzioni vengono usate per completare automaticamente il testo inserito dall'utente; in genere, questo viene presentato all'utente come una casella a discesa che elenca le possibili corrispondenze per quanto digitato nell'input.

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

#### Usi meno evidenti di datalist

Secondo [la specifica HTML](https://html.spec.whatwg.org/multipage/input.html#attr-input-list), l'attributo [`list`](/it/docs/Web/HTML/Reference/Elements/input#list) e l'elemento {{HTMLElement("datalist")}} possono essere usati con qualsiasi tipo di widget che richieda un input dell'utente. Questo porta ad alcuni utilizzi che potrebbero sembrare poco ovvi.

Ad esempio, nei browser che supportano `{{htmlelement("datalist")}}` sui tipi di input `range`, viene visualizzato un piccolo segno di graduazione sopra l'intervallo per ogni valore `{{htmlelement("option")}}` del datalist. È possibile vedere un'[esempio di implementazione nella pagina di riferimento di `<input type="range">`](/it/docs/Web/HTML/Reference/Elements/input/range#adding_tick_marks).

Inoltre, i browser che supportano {{htmlelement('datalist')}} e [`<input type="color">`](/it/docs/Web/HTML/Reference/Elements/input/color) dovrebbero visualizzare per impostazione predefinita una tavolozza di colori personalizzata, mantenendo comunque disponibile la tavolozza completa.

In questo caso, browser diversi si comportano in modo diverso a seconda della situazione, pertanto è consigliabile considerare tali utilizzi come miglioramento progressivo e assicurarsi che degradino in modo appropriato.

## Altre funzionalità dei moduli

Esistono alcune altre funzionalità dei moduli non così evidenti come quelle già menzionate, ma comunque utili in alcune situazioni; per questo riteniamo che valga la pena citarle brevemente.

### Indicatori e barre di avanzamento

Gli indicatori e le barre di avanzamento (creati usando gli elementi {{HTMLElement("meter")}} e {{HTMLElement("progress")}}) sono rappresentazioni visive di valori numerici.

#### Indicatore

Una barra indicatrice rappresenta un valore fisso in un intervallo delimitato dai valori [`max`](/it/docs/Web/HTML/Reference/Elements/meter#max) e [`min`](/it/docs/Web/HTML/Reference/Elements/meter#min). Questo valore viene renderizzato visivamente come una barra e, per sapere quale aspetto avrà questa barra, il valore viene confrontato con altri valori impostati:

- I valori [`low`](/it/docs/Web/HTML/Reference/Elements/meter#low) e [`high`](/it/docs/Web/HTML/Reference/Elements/meter#high) dividono l'intervallo nelle tre parti seguenti:
  - La parte inferiore dell'intervallo è compresa tra i valori [`min`](/it/docs/Web/HTML/Reference/Elements/meter#min) e [`low`](/it/docs/Web/HTML/Reference/Elements/meter#low), inclusi.
  - La parte centrale dell'intervallo è compresa tra i valori [`low`](/it/docs/Web/HTML/Reference/Elements/meter#low) e [`high`](/it/docs/Web/HTML/Reference/Elements/meter#high), esclusi.
  - La parte superiore dell'intervallo è compresa tra i valori [`high`](/it/docs/Web/HTML/Reference/Elements/meter#high) e [`max`](/it/docs/Web/HTML/Reference/Elements/meter#max), inclusi.

- Il valore [`optimum`](/it/docs/Web/HTML/Reference/Elements/meter#optimum) definisce il valore ottimale per l'elemento {{HTMLElement("meter")}}. In combinazione con i valori [`low`](/it/docs/Web/HTML/Reference/Elements/meter#low) e [`high`](/it/docs/Web/HTML/Reference/Elements/meter#high), definisce quale parte dell'intervallo è preferibile:
  - Se il valore [`optimum`](/it/docs/Web/HTML/Reference/Elements/meter#optimum) si trova nella parte inferiore dell'intervallo, l'intervallo inferiore è considerato la parte preferita, l'intervallo centrale è considerato la parte media e l'intervallo superiore è considerato la parte peggiore.
  - Se il valore [`optimum`](/it/docs/Web/HTML/Reference/Elements/meter#optimum) si trova nella parte centrale dell'intervallo, l'intervallo inferiore è considerato una parte media, l'intervallo centrale è considerato la parte preferita e anche l'intervallo superiore è considerato medio.
  - Se il valore [`optimum`](/it/docs/Web/HTML/Reference/Elements/meter#optimum) si trova nella parte superiore dell'intervallo, l'intervallo inferiore è considerato la parte peggiore, l'intervallo centrale è considerato la parte media e l'intervallo superiore è considerato la parte preferita.

Tutti i browser che implementano l'elemento {{HTMLElement("meter")}} usano questi valori per modificare il colore della barra dell'indicatore:

- Se il valore corrente si trova nella parte preferita dell'intervallo, la barra è verde.
- Se il valore corrente si trova nella parte media dell'intervallo, la barra è gialla.
- Se il valore corrente si trova nella parte peggiore dell'intervallo, la barra è rossa.

Una barra di questo tipo viene creata usando l'elemento {{HTMLElement("meter")}}. Serve a implementare qualsiasi tipo di indicatore; ad esempio, una barra che mostra lo spazio totale usato su un disco e che diventa rossa quando il disco inizia a riempirsi.

```html
<meter min="0" max="100" value="75" low="33" high="66" optimum="0">75</meter>
```

{{EmbedLiveSample("Meter", 120, 120)}}

Il contenuto all'interno dell'elemento {{HTMLElement("meter")}} è un fallback per i browser che non supportano l'elemento e per consentire alle tecnologie assistive di vocalizzarlo.

#### Avanzamento

Una barra di avanzamento rappresenta un valore che cambia nel tempo fino a un valore massimo specificato dall'attributo [`max`](/it/docs/Web/HTML/Reference/Elements/progress#max). Una barra di questo tipo viene creata usando un elemento {{ HTMLElement("progress")}}.

```html
<progress max="100" value="75">75/100</progress>
```

{{EmbedLiveSample("Progress", 120, 120)}}

Serve a implementare qualsiasi elemento che richieda la segnalazione dell'avanzamento, come la percentuale dei file totali scaricati o il numero di domande compilate in un questionario.

Il contenuto all'interno dell'elemento {{HTMLElement("progress")}} è un fallback per i browser che non supportano l'elemento e per consentire agli screen reader di vocalizzarlo.

## Riepilogo

Come si è visto negli ultimi articoli, esistono molti tipi di controlli dei moduli. Non è necessario ricordare tutti questi dettagli immediatamente: è possibile tornare a questi articoli tutte le volte che si desidera per controllare i dettagli.

Ora che è stata acquisita una comprensione dell'HTML alla base dei diversi controlli dei moduli disponibili, esamineremo [come applicare loro lo stile](/it/docs/Learn_web_development/Extensions/Forms/Styling_web_forms).

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/HTML5_input_types","Learn_web_development/Extensions/Forms/Styling_web_forms", "Learn_web_development/Extensions/Forms")}}
