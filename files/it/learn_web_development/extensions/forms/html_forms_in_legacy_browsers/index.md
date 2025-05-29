---
title: Moduli HTML nei browser legacy
slug: Learn_web_development/Extensions/Forms/HTML_forms_in_legacy_browsers
l10n:
  sourceCommit: edb16c0a662d7e719efe67561389a7a087c1ace9
---

Tutti gli sviluppatori web imparano molto rapidamente (e talvolta dolorosamente) che il Web è un posto molto duro per loro. La nostra peggiore maledizione sono i browser legacy. In passato questo significava "Internet Explorer", ma ci sono milioni di persone che usano dispositivi vecchi, specialmente telefoni cellulari, dove né il browser né il sistema operativo possono essere aggiornati.

Affrontare questa giungla fa parte del lavoro. Fortunatamente, ci sono alcuni trucchi da conoscere che possono aiutare a risolvere la maggior parte dei problemi causati dai browser legacy. Se un browser non supporta un tipo di {{htmlelement('input')}} HTML, non fallisce: semplicemente utilizza il valore predefinito `type=text`.

## Scoprire i problemi

Per comprendere i modelli comuni, è utile leggere la documentazione. Se stai leggendo questo su [MDN](/), sei nel posto giusto per iniziare. Basta controllare il supporto degli elementi (o interfacce DOM) che si desidera utilizzare. MDN dispone di tabelle di compatibilità disponibili per la maggior parte degli elementi, proprietà e API utilizzabili in una pagina web.

Poiché i [moduli HTML](/it/docs/Learn_web_development/Extensions/Forms) implicano un'interazione complessa, c'è una regola importante: mantenere la semplicità, nota anche come "[principio KISS](https://en.wikipedia.org/wiki/KISS_principle)". Ci sono molti casi in cui vogliamo moduli che siano "più belli" o "con funzionalità avanzate", ma costruire moduli HTML efficienti non è una questione di design o tecnologia. Piuttosto, si tratta di semplicità, intuitività e facilità di interazione per l'utente. Il tutorial, [forms usability on UX For The Masses,](https://www.uxforthemasses.com/forms-usability/) lo spiega bene.

### La degradazione graduale è la migliore amica dello sviluppatore web

[Graceful degradation e progressive enhancement](https://www.sitepoint.com/progressive-enhancement-graceful-degradation-choice/) sono schemi di sviluppo che permettono di costruire grandi cose supportando una vasta gamma di browser allo stesso tempo. Quando si costruisce qualcosa per un browser moderno e si vuole essere sicuri che funzioni, in un modo o nell'altro, sui browser legacy, si sta eseguendo una graceful degradation.

Vediamo alcuni esempi relativi ai moduli HTML.

#### Tipi di input HTML

Tutti i tipi di input HTML sono utilizzabili in tutti i browser, anche quelli antichi, perché il modo in cui si degradano è altamente prevedibile. Se un browser non conosce il valore dell'attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) di un elemento {{HTMLElement("input")}}, ricadrà come se il valore fosse `text`.

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
          alt="Screenshot del colore input su Chrome per Mac OSX"
          src="color-fallback-chrome.png"
        />
      </td>
      <td>
        <img
          alt="Screenshot del colore input su Firefox per Mac OSX"
          src="color-fallback-firefox.png"
        />
      </td>
    </tr>
  </tbody>
</table>

#### Pulsanti del modulo

Ci sono due modi per definire i pulsanti all'interno dei moduli HTML:

- L'elemento {{HTMLElement("input")}} con il suo attributo [`type`](/it/docs/Web/HTML/Reference/Elements/input#type) impostato ai valori `button`, `submit`, `reset` o `image`
- L'elemento {{HTMLElement("button")}}

##### {{HTMLElement("input")}}

L'elemento {{HTMLElement("input")}} può rendere le cose un po' difficili se si vuole applicare del CSS usando il selettore dell'elemento:

```html
<input type="button" value="click me" />
```

Se rimuoviamo il bordo su tutti gli input, possiamo ripristinare l'aspetto predefinito solo per i pulsanti input con il valore CSS globale {{cssxref('revert')}}.

```css
input {
  /* This rule turns off the default rendering for the input types that have a border,
     including buttons defined with an input element */
  border: 1px solid #ccc;
}
input[type="button"] {
  /* Revert the last border declaration */
  border: revert;
}
```

### Lasciar perdere il CSS

Uno dei grandi problemi con i moduli HTML è lo stile dei widget del modulo con il CSS. L'aspetto dei controlli del modulo è specifico per il browser e il sistema operativo. Per esempio, l'input di tipo colore appare diverso nei browser Safari, Chrome e Firefox, ma il widget di selezione del colore è lo stesso in tutti i browser su un dispositivo poiché si apre il selettore di colori nativo del sistema operativo.

È generalmente una buona idea non alterare l'aspetto predefinito dei controlli del modulo perché alterare il valore di una proprietà CSS può alterare alcuni tipi di input ma non altri. Per esempio, se dichiari `input { font-size: 2rem; }`, avrà un impatto su `number`, `date` e `text`, ma non su `color` o `range`. Se alteri una proprietà, ciò potrebbe influenzare l'aspetto del widget in modi inaspettati. Per esempio, `[value] { background-color: #ccc; }` può essere stato utilizzato per mirare a ogni {{HTMLElement("input")}} con un attributo `value`, ma cambiare il colore di sfondo o il raggio del bordo su un {{HTMLElement("meter")}} porterà probabilmente a risultati inaspettati che differiscono tra i browser. Puoi dichiarare {{cssxref('appearance', 'appearance: none;')}} per rimuovere gli stili del browser, ma questo generalmente vanifica l'obiettivo: poiché perdi tutto lo stile, rimuovi l'aspetto predefinito a cui i tuoi visitatori sono abituati.

In sintesi, quando si tratta di formattare i widget di controllo del modulo, gli effetti collaterali di stilarli con il CSS possono essere imprevedibili. Quindi non farlo. Anche se è ancora possibile fare alcuni aggiustamenti sugli elementi di testo (come dimensioni o colore del font), ci sono sempre effetti collaterali. Il miglior approccio resta il non stilare affatto i widget del modulo HTML. Ma puoi ancora applicare stili a tutti gli elementi circostanti. E, se devi alterare gli stili predefiniti dei tuoi widget del modulo, definisci una guida allo stile per garantire coerenza tra tutti i tuoi controlli del modulo in modo che l'esperienza dell'utente non sia distrutta. Puoi anche esplorare tecniche difficili come [ricostruire i widget con JavaScript](/it/docs/Learn_web_development/Extensions/Forms/How_to_build_custom_form_controls). Ma in tal caso, non esitare a [fare pagare al tuo cliente tale follia](https://www.smashingmagazine.com/2011/11/but-the-client-wants-ie-6-support/).

## Rilevamento delle funzionalità e polyfill

CSS e JavaScript sono tecnologie straordinarie, ma è importante assicurarsi di non interrompere i browser legacy. Prima di utilizzare funzionalità che non sono completamente supportate nei browser a cui ti stai rivolgendo, dovresti rilevare le funzionalità:

### Rilevamento delle funzionalità CSS

Prima di stilare un widget di controllo del modulo sostituito, puoi verificare se il browser supporta le funzionalità che intendi utilizzare {{cssxref('@supports')}}:

```css
@supports (appearance: none) {
  input[type="search"] {
    appearance: none;
    /* restyle the search input */
  }
}
```

La proprietà {{cssxref('appearance')}} può essere utilizzata per visualizzare un elemento utilizzando lo stile nativo della piattaforma o, come viene fatto con il valore `none`, rimuovere lo stile predefinito basato su nativo della piattaforma.

### JavaScript non intrusivo

Uno dei maggiori problemi è la disponibilità delle API. Per questo motivo, è considerata una buona pratica lavorare con JavaScript "non intrusivo". Si tratta di uno schema di sviluppo che definisce due requisiti:

- Una rigorosa separazione tra struttura e comportamenti.
- Se il codice si rompe, il contenuto e le funzionalità di base devono rimanere accessibili e utilizzabili.

[I principi del JavaScript non intrusivo](https://www.w3.org/wiki/The_principles_of_unobtrusive_JavaScript) (originariamente scritto da Peter-Paul Koch per Dev.Opera.com) descrivono molto bene queste idee.

### Presta attenzione alle prestazioni

Anche se alcuni polyfill sono molto attenti alle prestazioni, il caricamento di script aggiuntivi può influenzare le prestazioni dell'applicazione. Questo è particolarmente critico con i browser legacy; molti di loro hanno un motore JavaScript molto lento che può rendere l'esecuzione di tutti i tuoi polyfill dolorosa per l'utente. Le prestazioni sono un argomento a sé stante, ma i browser legacy sono molto sensibili ad esse: fondamentalmente, sono lenti e più polyfill richiedono, più JavaScript devono elaborare. Quindi sono doppiamente gravati rispetto ai browser moderni. Testa il tuo codice con i browser legacy per vedere come si comportano realmente. A volte, eliminare alcune funzionalità porta a una migliore esperienza utente che avere esattamente la stessa funzionalità in tutti i browser. Come ultimo promemoria, pensa sempre agli utenti finali.

## Conclusione

Come puoi vedere, considerare l'aspetto predefinito dei controlli del modulo per browser e sistema operativo è importante. Ci sono molte tecniche per affrontare questi problemi; tuttavia padroneggiarle tutte va oltre lo scopo di questo articolo. La premessa di base è considerare se alterare l'implementazione predefinita vale il lavoro prima di imbarcarsi nella sfida.

Se hai letto tutti gli articoli di questa [Guida ai moduli HTML](/it/docs/Learn_web_development/Extensions/Forms), dovresti ora sentirti a tuo agio nell'usare i moduli. Se scopri nuove tecniche o suggerimenti, per favore aiuta a migliorare la guida.
