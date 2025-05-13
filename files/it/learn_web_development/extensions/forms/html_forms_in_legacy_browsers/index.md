---
title: Moduli HTML nei browser legacy
slug: Learn_web_development/Extensions/Forms/HTML_forms_in_legacy_browsers
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Tutti gli sviluppatori web imparano molto rapidamente (e talvolta dolorosamente) che il Web è un luogo molto difficile per loro. La nostra peggiore maledizione sono i browser legacy. Questo tempo fa significava "Internet Explorer", ma ci sono milioni di persone che usano dispositivi vecchi, specialmente telefoni cellulari, dove né il browser né il sistema operativo possono essere aggiornati.

Affrontare questa giungla fa parte del lavoro. Fortunatamente, ci sono alcuni trucchi da conoscere che possono aiutarla a risolvere la maggior parte dei problemi causati dai browser legacy. Se un browser non supporta un tipo di {{htmlelement('input')}} HTML, non fallisce: semplicemente utilizza il valore predefinito di `type=text`.

## Informarsi sui problemi

Per comprendere schemi comuni, è utile leggere la documentazione. Se sta leggendo questo su [MDN](/), è nel posto giusto per iniziare. Basta verificare il supporto degli elementi (o interfacce DOM) che vuole utilizzare. MDN ha tabelle di compatibilità disponibili per la maggior parte degli elementi, delle proprietà e delle API che possono essere usati in una pagina web.

Poiché [i moduli HTML](/it/docs/Learn_web_development/Extensions/Forms) comportano un'interazione complessa, c'è una regola importante: mantenerli semplici, nota anche come "[principio KISS](https://en.wikipedia.org/wiki/KISS_principle)". Ci sono molti casi in cui vogliamo moduli che sono "più belli" o "con funzioni avanzate", ma costruire moduli HTML efficienti non è una questione di design o tecnologia. Piuttosto, riguarda la semplicità, l'intuitività e la facilità di interazione per l'utente. Il tutorial, [usabilità dei moduli su UX For The Masses,](https://www.uxforthemasses.com/forms-usability/) lo spiega bene.

### Il degrado graduale è il miglior amico dello sviluppatore web

[Degrado graduale e miglioramento progressivo](https://www.sitepoint.com/progressive-enhancement-graceful-degradation-choice/) sono schemi di sviluppo che le permettono di realizzare grandi cose supportando allo stesso tempo un'ampia gamma di browser. Quando costruisce qualcosa per un browser moderno e vuole essere sicuro che funzioni, in un modo o nell'altro, nei browser legacy, sta eseguendo un degrado graduale.

Vediamo alcuni esempi relativi ai moduli HTML.

#### Tipi di input HTML

Tutti i tipi di input HTML possono essere utilizzati in tutti i browser, anche quelli antichi, perché il modo in cui degradano è altamente prevedibile. Se un browser non conosce il valore dell'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) di un elemento {{HTMLElement("input")}}, ricadrà come se il valore fosse `text`.

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
          alt="Schermata dell'input di colore su Chrome per Mac OSX"
          src="color-fallback-chrome.png"
        />
      </td>
      <td>
        <img
          alt="Schermata dell'input di colore su Firefox per Mac OSX"
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

L'elemento {{HTMLElement("input")}} può rendere le cose un po' difficili se desidera applicare del CSS utilizzando il selettore dell'elemento:

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

Veda il valore globale CSS {{cssxref('revert')}} per ulteriori informazioni.

### Lasciare perdere il CSS

Uno dei grandi problemi con i moduli HTML è lo stile dei widget del modulo con CSS. L'aspetto dei controlli di modulo è specifico del browser e del sistema operativo. Ad esempio, l'input di tipo colore appare diverso nei browser Safari, Chrome e Firefox, ma il widget del selettore di colore è lo stesso in tutti i browser su un dispositivo poiché si apre il selettore di colore nativo del sistema operativo.

In generale, è una buona idea non alterare l'aspetto predefinito del controllo del modulo perché alterare il valore di una proprietà CSS può alterare alcuni tipi di input ma non altri. Ad esempio, se dichiara `input { font-size: 2rem; }`, impatterà su `number`, `date` e `text`, ma non su `color` o `range`. Se altera una proprietà, ciò può influenzare l'aspetto del widget in modi inaspettati. Ad esempio, `[value] { background-color: #ccc; }` potrebbe essere usato per mirare a ogni {{HTMLElement("input")}} con un attributo `value`, ma cambiare il colore di sfondo o il raggio del bordo su un {{HTMLElement("meter")}} porterà probabilmente a risultati inaspettati che differiscono tra i browser. Può dichiarare {{cssxref('appearance', 'appearance: none;')}} per rimuovere gli stili del browser, ma questo generalmente sconfigge lo scopo: poiché si perde tutto lo stile, rimuovendo l'aspetto predefinito a cui i suoi visitatori sono abituati.

Per riassumere, quando si tratta di stilizzare i widget di controllo del modulo, gli effetti collaterali dello stilarli con CSS possono essere imprevedibili. Quindi, non lo faccia. Anche se è ancora possibile fare alcuni aggiustamenti sugli elementi di testo (come dimensionamento o colore del carattere), ci sono sempre effetti collaterali. L'approccio migliore rimane quello di non stilizzare affatto i widget del modulo HTML. Ma può comunque applicare stili a tutti gli elementi circostanti. E, se deve alterare gli stili predefiniti dei suoi widget di modulo, definisca una guida di stile per garantire la coerenza tra tutti i suoi controlli del modulo in modo che l'esperienza utente non venga distrutta. Può anche indagare su alcune tecniche ardue come [ricostruire i widget con JavaScript](/it/docs/Learn_web_development/Extensions/Forms/How_to_build_custom_form_controls). Ma, in quel caso, non esiti a [addebiti al suo cliente per tale follia](https://www.smashingmagazine.com/2011/11/but-the-client-wants-ie-6-support/).

## Rilevamento delle funzionalità e polyfill

CSS e JavaScript sono tecnologie fantastiche, ma è importante assicurarsi di non rompere i browser legacy. Prima di usare funzionalità che non sono completamente supportate nei browser che sta puntando, dovrebbe rilevare le funzionalità:

### Rilevamento delle funzionalità CSS

Prima di stilizzare un widget di controllo del modulo sostituito, può controllare se il browser supporta le funzionalità che intende usare con {{cssxref('@supports')}}:

```css
@supports (appearance: none) {
  input[type="search"] {
    appearance: none;
    /* restyle the search input */
  }
}
```

La proprietà {{cssxref('appearance')}} può essere utilizzata per visualizzare un elemento utilizzando lo stile nativo della piattaforma, o, come si fa con il valore di `none`, rimuovere lo stile predefinito basato sulla piattaforma nativa.

### JavaScript non invadente

Uno dei maggiori problemi è la disponibilità delle API. Per quella ragione, è considerata una buona pratica lavorare con JavaScript "non invadente". È un pattern di sviluppo che definisce due requisiti:

- Una separazione rigorosa tra struttura e comportamenti.
- Se il codice si rompe, il contenuto e le funzionalità di base devono rimanere accessibili e usabili.

[I principi di JavaScript non invadente](https://www.w3.org/wiki/The_principles_of_unobtrusive_JavaScript) (originariamente scritti da Peter-Paul Koch per Dev.Opera.com) descrivono molto bene queste idee.

### Prestare attenzione alle prestazioni

Anche se alcuni polyfill sono molto attenti alle prestazioni, il caricamento di script aggiuntivi può influenzare le prestazioni della sua applicazione. Questo è particolarmente critico con i browser legacy; molti di loro hanno un motore JavaScript molto lento che può rendere l'esecuzione di tutti i suoi polyfill dolorosa per l'utente. Le prestazioni sono un argomento a sé, ma i browser legacy sono molto sensibili ad esso: fondamentalmente, sono lenti e più polyfill hanno bisogno, più JavaScript devono elaborare. Quindi sono doppiamente penalizzati rispetto ai browser moderni. Testi il suo codice con i browser legacy per vedere come si comportano effettivamente. A volte, abbandonare alcune funzionalità porta a una migliore esperienza utente rispetto ad avere esattamente la stessa funzionalità in tutti i browser. Come ultimo promemoria, pensi sempre agli utenti finali.

## Conclusione

Come può vedere, considerare l'aspetto predefinito dei controlli dei moduli del browser e del sistema operativo è importante. Ci sono molte tecniche per gestire questi problemi; tuttavia, padroneggiarle tutte va oltre lo scopo di questo articolo. La premessa di base è considerare se alterare l'implementazione predefinita vale il lavoro prima di intraprendere la sfida.

Se legge tutti gli articoli di questa [guida ai moduli HTML](/it/docs/Learn_web_development/Extensions/Forms), dovrebbe ora sentirsi a suo agio nell'utilizzare i moduli. Se scopre nuove tecniche o suggerimenti, la preghiamo di contribuire a migliorare la guida.
