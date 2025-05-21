---
title: Moduli HTML nei browser legacy
slug: Learn_web_development/Extensions/Forms/HTML_forms_in_legacy_browsers
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Tutti gli sviluppatori web apprendono molto rapidamente (e a volte dolorosamente) che il Web è un luogo molto ostile per loro. La nostra peggior maledizione sono i browser legacy. Questo significava un tempo "Internet Explorer", ma ci sono milioni di persone che utilizzano dispositivi vecchi, specialmente telefoni cellulari, in cui né il browser né il sistema operativo possono essere aggiornati.

Affrontare questa giungla fa parte del lavoro. Fortunatamente, ci sono alcuni trucchi da conoscere che possono aiutarti a risolvere la maggior parte dei problemi causati dai browser legacy. Se un browser non supporta un tipo di {{htmlelement('input')}} HTML, non fallisce: semplicemente utilizza il valore predefinito `type=text`.

## Scopri i problemi

Per comprendere i modelli comuni, è utile leggere la documentazione. Se stai leggendo questo su [MDN](/), sei nel posto giusto per iniziare. Basta controllare il supporto degli elementi (o interfacce DOM) che desideri utilizzare. MDN ha tabelle di compatibilità disponibili per la maggior parte degli elementi, delle proprietà e delle API che possono essere utilizzate in una pagina web.

Poiché i [moduli HTML](/it/docs/Learn_web_development/Extensions/Forms) coinvolgono un'interazione complessa, c'è una regola importante: mantienilo semplice, anche noto come il "[principio KISS](https://en.wikipedia.org/wiki/KISS_principle)". Ci sono molti casi in cui vogliamo moduli che siano "più belli" o "con funzionalità avanzate", ma costruire moduli HTML efficienti non è una questione di design o tecnologia. Piuttosto, riguarda la semplicità, l'intuitività e la facilità di interazione dell'utente. Il tutorial, [forms usability on UX For The Masses,](https://www.uxforthemasses.com/forms-usability/) lo spiega bene.

### La degradazione graduale è la migliore amica dello sviluppatore web

[Degradazione graduale e miglioramento progressivo](https://www.sitepoint.com/progressive-enhancement-graceful-degradation-choice/) sono modelli di sviluppo che ti permettono di creare cose eccezionali supportando una vasta gamma di browser contemporaneamente. Quando costruisci qualcosa per un browser moderno e vuoi essere sicuro che funzionerà, in un modo o nell'altro, nei browser legacy, stai eseguendo una degradazione graduale.

Vediamo alcuni esempi relativi ai moduli HTML.

#### Tipi di input HTML

Tutti i tipi di input HTML sono utilizzabili in tutti i browser, anche in quelli antichi, perché il modo in cui si degradano è altamente prevedibile. Se un browser non conosce il valore dell'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) di un elemento {{HTMLElement("input")}}, tornerà come se il valore fosse `text`.

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
          alt="Screenshot dell'input color in Chrome per Mac OSX"
          src="color-fallback-chrome.png"
        />
      </td>
      <td>
        <img
          alt="Screenshot dell'input color in Firefox per Mac OSX"
          src="color-fallback-firefox.png"
        />
      </td>
    </tr>
  </tbody>
</table>

#### Pulsanti del modulo

Ci sono due modi per definire i pulsanti all'interno dei moduli HTML:

- L'elemento {{HTMLElement("input")}} con il suo attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) impostato sui valori `button`, `submit`, `reset` o `image`
- L'elemento {{HTMLElement("button")}}

##### {{HTMLElement("input")}}

L'elemento {{HTMLElement("input")}} può rendere le cose un po' difficili se desideri applicare dei CSS utilizzando il selettore dell'elemento:

```html
<input type="button" value="click me" />
```

Se rimuoviamo il bordo su tutti gli input, possiamo ripristinare l'aspetto predefinito solo sui pulsanti di input?

```css
input {
  /* This rule turns off the default rendering for the input types that have a border,
     including buttons defined with an input element */
  border: 1px solid #ccc;
}
input[type="button"] {
  /* This does NOT restore the default rendering */
  border: none;
}
input[type="button"] {
  /* These don't either! Actually there is no standard way to do it in any browser */
  border: auto;
  border: initial;
}
input[type="button"] {
  /* This will come the closest to restoring default rendering. */
  border: revert;
}
```

Consulta il valore globale CSS {{cssxref('revert')}} per ulteriori informazioni.

### Lascia perdere il CSS

Uno dei grandi problemi con i moduli HTML è la stilizzazione dei widget dei moduli con CSS. L'aspetto dei controlli dei moduli è specifico del browser e del sistema operativo. Ad esempio, l'input di tipo colore appare diverso nei browser Safari, Chrome e Firefox, ma il widget del selettore di colori è lo stesso in tutti i browser su un dispositivo poiché apre il selettore di colori nativo del sistema operativo.

In generale, è una buona idea non alterare l'aspetto predefinito dei controlli dei moduli, poiché alterare un valore di una proprietà CSS potrebbe alterare alcuni tipi di input ma non altri. Ad esempio, se dichiari `input { font-size: 2rem; }`, influenzerà `number`, `date`, e `text`, ma non `color` o `range`. Se alteri una proprietà, potresti influenzare l'aspetto del widget in modi inaspettati. Ad esempio, `[value] { background-color: #ccc; }` potrebbe essere stato usato per prendere di mira ogni {{HTMLElement("input")}} con un attributo `value`, ma cambiare il colore di sfondo o il raggio del bordo su un {{HTMLElement("meter")}} porterà probabilmente a risultati inattesi che differiscono tra i browser. È possibile dichiarare {{cssxref('appearance', 'appearance: none;')}} per rimuovere gli stili del browser, ma generalmente questo vanifica lo scopo: poiché perdi tutto lo stile, eliminando l'aspetto e la sensazione predefiniti a cui i tuoi visitatori sono abituati.

Per riassumere, quando si tratta di stilizzare i widget di controllo dei moduli, gli effetti collaterali di stilizzarli con CSS possono essere imprevedibili. Quindi non farlo. Anche se è ancora possibile effettuare alcuni aggiustamenti sugli elementi di testo (come dimensionamento o colore del carattere), ci saranno sempre effetti collaterali. Il miglior approccio rimane quello di non stilizzare affatto i widget dei moduli HTML. Ma è possibile applicare comunque stili a tutti gli elementi circostanti. E, se devi alterare gli stili predefiniti dei tuoi widget di modulo, definisci una guida di stile per garantire coerenza tra tutti i tuoi controlli di modulo in modo che l'esperienza utente non venga compromessa. Puoi anche indagare su alcune tecniche difficili come [ricostruire i widget con JavaScript](/it/docs/Learn_web_development/Extensions/Forms/How_to_build_custom_form_controls). Ma in tal caso, non esitare a [far pagare al tuo cliente per tali follie](https://www.smashingmagazine.com/2011/11/but-the-client-wants-ie-6-support/).

## Rilevamento delle funzionalità e polyfills

CSS e JavaScript sono tecnologie fantastiche, ma è importante assicurarsi di non interrompere i browser legacy. Prima di utilizzare funzionalità che non sono completamente supportate nei browser a cui miri, dovresti rilevare le funzionalità:

### Rilevamento delle funzionalità CSS

Prima di stilizzare un widget di controllo del modulo sostituito, puoi verificare se il browser supporta le funzionalità che intendi utilizzare con {{cssxref('@supports')}}:

```css
@supports (appearance: none) {
  input[type="search"] {
    appearance: none;
    /* restyle the search input */
  }
}
```

La proprietà {{cssxref('appearance')}} può essere utilizzata per visualizzare un elemento utilizzando lo stile nativo della piattaforma o, come viene fatto con il valore `none`, per rimuovere lo stile nativo della piattaforma predefinito.

### JavaScript non invasivo

Uno dei più grandi problemi è la disponibilità delle API. Per questo motivo, è considerata la migliore pratica lavorare con JavaScript "non invasivo". È un modello di sviluppo che definisce due requisiti:

- Una separazione rigorosa tra struttura e comportamenti.
- Se il codice si rompe, il contenuto e le funzionalità di base devono rimanere accessibili e utilizzabili.

[I principi del JavaScript non invasivo](https://www.w3.org/wiki/The_principles_of_unobtrusive_JavaScript) (originariamente scritti da Peter-Paul Koch per Dev.Opera.com) descrivono molto bene queste idee.

### Presta attenzione alle prestazioni

Anche se alcuni polyfill sono molto concentrati sulle prestazioni, il caricamento di script aggiuntivi può influenzare le prestazioni della tua applicazione. Questo è particolarmente critico con i browser legacy; molti di loro hanno un motore JavaScript molto lento che può rendere dolorosa per l'utente l'esecuzione di tutti i tuoi polyfill. Le prestazioni sono un argomento a sé stante, ma i browser legacy sono molto sensibili ad esse: fondamentalmente, sono lenti e più polyfill richiedono, più JavaScript devono elaborare. Quindi sono doppiamente gravati rispetto ai browser moderni. Testa il tuo codice con i browser legacy per vedere come si comportano effettivamente. A volte, rinunciare ad alcune funzionalità porta a una migliore esperienza dell'utente piuttosto che avere esattamente la stessa funzionalità in tutti i browser. Come ultimo promemoria, pensa sempre agli utenti finali.

## Conclusione

Come puoi vedere, prendere in considerazione l'aspetto predefinito dei controlli del modulo nei browser e nei sistemi operativi è importante. Ci sono molte tecniche per affrontare questi problemi; tuttavia, padroneggiarle tutte va al di là dello scopo di questo articolo. La premessa di base è considerare se alterare l'implementazione predefinita valga il lavoro prima di intraprendere la sfida.

Se leggi tutti gli articoli di questa [Guida ai moduli HTML](/it/docs/Learn_web_development/Extensions/Forms), dovresti ora sentirti a tuo agio nell'usare i moduli. Se scopri nuove tecniche o suggerimenti, ti invitiamo a contribuire a migliorare la guida.
