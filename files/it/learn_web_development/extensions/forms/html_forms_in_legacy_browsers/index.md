---
title: Moduli HTML nei browser legacy
short-title: Moduli nei browser legacy
slug: Learn_web_development/Extensions/Forms/HTML_forms_in_legacy_browsers
l10n:
  sourceCommit: dc9d517589ac7b74bc205f49492b0450dfdb78de
---

Tutti gli sviluppatori web imparano molto rapidamente (e talvolta dolorosamente) che il Web è un ambiente molto ostile per loro. La peggiore maledizione sono i browser legacy. In passato questo significava "Internet Explorer", ma ci sono milioni di persone che usano dispositivi datati, in particolare telefoni cellulari, sui quali né il browser né il sistema operativo possono essere aggiornati.

Affrontare questa natura selvaggia fa parte del lavoro. Fortunatamente, esistono alcuni trucchi che possono aiutare a risolvere la maggior parte dei problemi causati dai browser legacy. Se un browser non supporta un tipo HTML {{htmlelement('input')}}, non fallisce: usa semplicemente il valore predefinito `type=text`.

## Conoscere i problemi

Per comprendere gli schemi comuni, è utile leggere la documentazione. Se questa pagina viene letta su [MDN](/), è il posto giusto da cui iniziare. Basta verificare il supporto degli elementi (o delle interfacce DOM) che si desidera usare. MDN dispone di tabelle di compatibilità per la maggior parte degli elementi, delle proprietà e delle API utilizzabili in una pagina web.

Poiché i [moduli HTML](/it/docs/Learn_web_development/Extensions/Forms) comportano interazioni complesse, esiste una regola importante: mantenere la semplicità, noto anche come "[principio KISS](https://en.wikipedia.org/wiki/KISS_principle)". Ci sono moltissimi casi in cui si desiderano moduli "più gradevoli" o "con funzionalità avanzate", ma creare moduli HTML efficienti non è una questione di design o tecnologia. Si tratta invece di semplicità, intuitività e facilità di interazione per l'utente. Il tutorial [usabilità dei moduli su UX For The Masses](https://www.uxforthemasses.com/forms-usability/) lo spiega bene.

### Il graceful degradation è il migliore alleato dello sviluppatore web

Il [graceful degradation e il progressive enhancement](https://www.sitepoint.com/progressive-enhancement-graceful-degradation-choice/) sono schemi di sviluppo che consentono di creare ottimi contenuti supportando contemporaneamente un'ampia gamma di browser. Quando si crea qualcosa per un browser moderno e si desidera assicurarsi che funzioni, in un modo o nell'altro, sui browser legacy, si sta applicando il graceful degradation.

Vediamo alcuni esempi relativi ai moduli HTML.

#### Tipi di input HTML

Tutti i tipi di input HTML sono utilizzabili in tutti i browser, anche quelli molto datati, perché il modo in cui degradano è altamente prevedibile. Se un browser non riconosce il valore dell'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) di un elemento {{HTMLElement("input")}}, effettuerà il fallback come se il valore fosse `text`.

```html
<label for="myColor">
  Pick a color
  <input type="color" id="myColor" name="color" />
</label>
```

<table class="no-markdown">
  <thead>
    <tr>
      <th>Supportato</th>
      <th>Non supportato</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        <img
          alt="Schermata dell'input color in Chrome per macOS"
          src="color-fallback-chrome.png"
        />
      </td>
      <td>
        <img
          alt="Schermata dell'input color in Firefox per macOS"
          src="color-fallback-firefox.png"
        />
      </td>
    </tr>
  </tbody>
</table>

#### Pulsanti del modulo

Esistono due modi per definire pulsanti nei moduli HTML:

- L'elemento {{HTMLElement("input")}} con l'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) impostato sui valori `button`, `submit`, `reset` o `image`
- L'elemento {{HTMLElement("button")}}

##### {{HTMLElement("input")}}

L'elemento {{HTMLElement("input")}} può rendere le cose un po' difficili se si desidera applicare del CSS usando il selettore dell'elemento:

```html
<input type="button" value="click me" />
```

Se si rimuove il bordo da tutti gli input, è possibile ripristinare l'aspetto predefinito per i soli pulsanti input con il valore CSS globale {{cssxref('revert')}}.

```css
input {
  /* This rule turns off the default rendering for the input types that have a border,
     including buttons defined with an input element */
  border: 1px solid #cccccc;
}
input[type="button"] {
  /* Revert the last border declaration */
  border: revert;
}
```

### Limitare lo stile nei browser legacy

Uno dei principali problemi dei moduli HTML nei browser legacy è la loro stilizzazione con CSS. Come illustrato altrove, è possibile dichiarare {{cssxref('appearance', 'appearance: none;')}} per rimuovere gli stili predefiniti e crearne di personalizzati. Tuttavia, i browser legacy hanno meno probabilità rispetto ai browser moderni di supportare le tecniche di stile illustrate in precedenza nel modulo. Se è necessario supportarli, potrebbe essere meglio lasciare i controlli del modulo senza stile nei browser legacy. Consultare la sezione successiva per indicazioni sul rilevamento del supporto per tipi di input specifici.

Se è necessario modificare gli stili predefiniti dei widget del modulo nei browser legacy, definire una guida di stile per garantire coerenza tra tutti i controlli del modulo, in modo che l'esperienza utente non venga compromessa. È anche possibile valutare tecniche complesse come la [ricostruzione dei widget con JavaScript](/it/docs/Learn_web_development/Extensions/Forms/How_to_build_custom_form_controls), ma potrebbe richiedere più lavoro di quanto valga.

## Rilevamento delle funzionalità e polyfill

CSS e JavaScript sono tecnologie eccezionali, ma è importante assicurarsi di non compromettere i browser legacy. Prima di usare funzionalità non completamente supportate dai browser a cui ci si rivolge, è opportuno rilevarne il supporto.

### Rilevamento delle funzionalità CSS

Prima di stilizzare un widget di controllo del modulo sostituito, è possibile verificare se il browser supporta le funzionalità che si intende usare con {{cssxref('@supports')}}:

```css
@supports (appearance: none) {
  input[type="search"] {
    appearance: none;
    /* restyle the search input */
  }
}
```

La proprietà {{cssxref('appearance')}} può essere usata per visualizzare un elemento con lo stile nativo della piattaforma oppure, come avviene con il valore `none`, per rimuovere lo stile predefinito basato sulla piattaforma nativa.

### Rilevamento JavaScript degli input dei moduli

È possibile usare JavaScript per rilevare se un particolare tipo di input è supportato. Questo si basa sul fatto menzionato in precedenza: ogni tipo di input effettua il fallback a `<input type="text">` nei browser che non lo supportano.

Definire una funzione di test. La prima riga del corpo della funzione deve creare un elemento `<input>` di test. Successivamente, impostare il suo attributo `type` sul tipo che si desidera testare. Infine, testare il valore dell'attributo `type`. Nei browser che non supportano quel tipo di input, l'ultima riga non avrà effetto e `type` verrà restituito come `text`. Nella riga seguente viene invertito il valore restituito usando l'operatore di negazione (`!`) perché, se `type` non è `text`, il tipo è supportato e quindi si desidera restituire `true`. La funzione completa è la seguente:

```js
function testDatetimeLocalSupport() {
  const testInput = document.createElement("input");
  testInput.setAttribute("type", "datetime-local");
  return testInput.type !== "text";
}
```

L'esempio precedente mostra l'idea di base alla base di tali test. Tuttavia, anziché reinventare la ruota, è opportuno usare una libreria di rilevamento delle funzionalità per gestire questi test.

In base ai risultati del test, si potrebbe ad esempio scegliere di usare JavaScript per creare una sostituzione personalizzata per il tipo non supportato oppure di non applicare un foglio di stile che stilizzi il tipo non supportato, poiché si desidera fornire stili predefiniti semplici ai browser legacy.

### JavaScript non intrusivo

Uno dei maggiori problemi è la disponibilità delle API. Per questo motivo, è considerata una buona pratica lavorare con JavaScript "non intrusivo". Si tratta di uno schema di sviluppo che definisce due requisiti:

- Una separazione rigorosa tra struttura e comportamenti.
- Se il codice non funziona, il contenuto e le funzionalità di base devono rimanere accessibili e utilizzabili.

[I principi del JavaScript non intrusivo](https://www.w3.org/wiki/The_principles_of_unobtrusive_JavaScript) (originariamente scritti da Peter-Paul Koch per dev.opera.com) descrivono molto bene queste idee.

### Prestare attenzione alle prestazioni

Anche se alcuni polyfill prestano molta attenzione alle prestazioni, il caricamento di script aggiuntivi può influire sulle prestazioni dell'applicazione. Questo è particolarmente critico con i browser legacy; molti di essi hanno un motore JavaScript molto lento che può rendere l'esecuzione di tutti i polyfill gravosa per l'utente. Le prestazioni sono un argomento a sé stante, ma i browser legacy sono molto sensibili a esse: in sostanza, sono lenti e più polyfill richiedono, più JavaScript devono elaborare. Risultano quindi doppiamente penalizzati rispetto ai browser moderni. Testare il codice con i browser legacy per verificare come si comporta effettivamente. Talvolta, eliminare alcune funzionalità porta a un'esperienza utente migliore rispetto ad avere esattamente le stesse funzionalità in tutti i browser. Come ultimo promemoria, occorre sempre pensare agli utenti finali.

## Conclusione

Come si può vedere, considerare l'aspetto predefinito dei controlli del modulo del browser e del sistema operativo è importante. Esistono molte tecniche per gestire questi problemi; tuttavia, padroneggiarle tutte va oltre lo scopo di questo articolo. Il presupposto fondamentale è valutare se modificare l'implementazione predefinita valga il lavoro necessario prima di affrontare questa sfida.

Dopo aver letto tutti gli articoli di questa [guida ai moduli HTML](/it/docs/Learn_web_development/Extensions/Forms), dovrebbe essere possibile usare i moduli con sicurezza. Se vengono scoperte nuove tecniche o suggerimenti, contribuire a migliorare la guida.
