---
title: Supporto dei browser meno recenti
slug: Learn_web_development/Core/CSS_layout/Supporting_Older_Browsers
l10n:
  sourceCommit: fdc1b80c73b20a04748a9fdf68645d0e6b4d96e1
---

Tra i visitatori di un sito web possono esserci utenti che usano browser meno recenti oppure browser che non supportano le funzionalità CSS implementate. Si tratta di uno scenario comune sul web, dove nuove funzionalità vengono aggiunte continuamente a CSS. I browser differiscono nel supporto di queste funzionalità perché tendono a dare priorità all'implementazione di funzionalità diverse. Questo articolo spiega come, in qualità di sviluppatore web, sia possibile utilizzare moderne tecniche web per garantire che il sito web rimanga accessibile agli utenti con tecnologie meno recenti.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Nozioni di base di HTML (studiare
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Introduzione a HTML</a
        >) e un'idea del funzionamento di CSS (studiare
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Nozioni di base sullo stile CSS</a>.)
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere come fornire supporto per i layout nei browser meno recenti
        che potrebbero non supportare le funzionalità che si desidera utilizzare.
      </td>
    </tr>
  </tbody>
</table>

## Qual è il panorama dei browser per il sito?

Ogni sito web è diverso in termini di pubblico di destinazione. Prima di decidere un approccio, occorre scoprire quanti visitatori raggiungono il sito usando browser meno recenti. Questo è semplice se si sta aggiungendo contenuto a un sito web esistente o lo si sta sostituendo, poiché probabilmente sono disponibili dati analitici che possono indicare la tecnologia usata dai visitatori. Se non si dispone di dati analitici o si sta lanciando un sito completamente nuovo, siti come [Statcounter](https://gs.statcounter.com/) possono fornire statistiche pertinenti, filtrabili per località.

Occorre inoltre considerare il tipo di dispositivi e il modo in cui le persone usano il sito. Per esempio, è possibile aspettarsi un utilizzo del sito web superiore alla media sui dispositivi mobili. Dare sempre priorità all'accessibilità e alle persone che usano tecnologie assistive; per alcuni siti, questo aspetto può essere ancora più critico. Gli sviluppatori spesso si preoccupano molto dell'esperienza dell'1% degli utenti, trascurando però il numero molto maggiore di persone con esigenze di accessibilità.

## Qual è il supporto per le funzionalità che si desidera utilizzare?

{{Compat}}

La tabella precedente è inclusa in fondo a ogni pagina relativa a una funzionalità, nella sezione "Compatibilità del browser". Dopo aver identificato i browser usati dai visitatori del sito, è possibile valutare ogni tecnologia che si desidera usare in base al suo supporto nei vari browser e alla facilità con cui si può fornire un'alternativa ai visitatori che non dispongono di tale tecnologia.

Su MDN, vengono fornite informazioni sulla compatibilità del browser in ogni pagina relativa alle proprietà CSS. Queste informazioni sulla compatibilità, presentate in una tabella, includono un elenco dei principali browser insieme alle versioni che hanno iniziato a supportare la proprietà. Vedere le pagine [`flex-flow`](/it/docs/Web/CSS/Reference/Properties/flex-flow#browser_compatibility) e [`background-color`](/it/docs/Web/CSS/Reference/Properties/background-color#browser_compatibility) per alcuni esempi.

Queste tabelle di compatibilità del browser forniscono informazioni sui browser compatibili con la tecnologia cercata e sulla versione dalla quale il browser ha iniziato a supportare tale funzionalità. Le informazioni sulla compatibilità dei browser desktop e dei browser per telefoni cellulari sono visualizzate separatamente.

Un altro modo diffuso per scoprire quanto bene sia supportata una funzionalità è il sito web [Can I Use](https://caniuse.com/). Questo sito elenca la maggior parte delle funzionalità della piattaforma web con informazioni sul loro stato di supporto nei browser. È possibile visualizzare statistiche di utilizzo per località, utile se si lavora a un sito con utenti concentrati soprattutto in una specifica area del mondo. È persino possibile collegare il proprio account Google Analytics per ottenere analisi basate sui dati degli utenti.

Comprendere la tecnologia disponibile agli utenti in base al browser che usano e il supporto multipiattaforma per le funzionalità che si potrebbero voler usare sul sito web consente di prendere decisioni informate e di sapere come supportare al meglio tutti gli utenti.

## Il supporto di una funzionalità non implica un aspetto identico

Non è possibile che un sito web abbia lo stesso aspetto in tutti i browser. Alcuni utenti visualizzeranno il sito su un telefono, altri su un grande schermo desktop. Analogamente, alcuni utenti avranno una versione meno recente del browser e altri il browser più aggiornato. Alcuni utenti potrebbero ascoltare i contenuti letti da uno screen reader, mentre altri potrebbero dover ingrandire la pagina per poterli leggere. Supportare tutti significa fornire una versione dei contenuti progettata in modo robusto, che risulti eccellente nei browser moderni, ma rimanga utilizzabile a un livello di base per tutti gli utenti, indipendentemente dal modo in cui accedono ai contenuti.

Un livello di supporto di base deriva da una buona struttura dei contenuti, in modo che il flusso normale della pagina abbia senso. Per gli utenti con un piano dati limitato, i browser potrebbero non caricare immagini, font o persino CSS. Tuttavia, il contenuto dovrebbe essere presentato in modo da essere accessibile e leggibile anche quando questi elementi non sono completamente caricati. Un documento HTML ben strutturato dovrebbe sempre essere il punto di partenza. Occorre chiedersi: _se si rimuove il foglio di stile, il contenuto ha ancora senso?_

Dal punto di vista commerciale, non ha senso dedicare tempo a cercare di offrire a tutti un'esperienza identica del sito web. Questo perché gli ambienti degli utenti possono variare enormemente e non sono sotto il controllo dello sviluppatore. Occorre trovare un equilibrio tra una semplice pagina HTML e un sito web ricco di funzionalità. È utile testare una visualizzazione semplice del sito, priva di CSS, per garantire che l'esperienza di fallback sia accessibile. Questo fallback potrebbe non essere mai visualizzato dalle persone che usano browser molto vecchi o limitati, ma potrebbe essere visualizzato dal principale pubblico di destinazione, ovvero gli utenti di browser moderni, quando il browser o la connessione Internet non funzionano temporaneamente. CSS semplifica la creazione di questi fallback. Pertanto, è preferibile concentrarsi su ciò che è possibile controllare, ovvero dedicare tempo a rendere il sito [accessibile](/it/docs/Web/Accessibility), servendo così più utenti.

## Creazione di fallback in CSS

Le specifiche CSS contengono informazioni che spiegano cosa fa il browser quando due funzionalità simili, come metodi di layout, vengono applicate allo stesso elemento. Per esempio, definiscono cosa accade se un elemento è flottante ed è anche un elemento griglia che fa parte di un contenitore griglia CSS. Esiste anche una definizione di ciò che accade quando un elemento ha impostate entrambe le proprietà {{cssxref("margin-top")}} e {{cssxref("margin-block-start")}}.

Quando un browser non riconosce una nuova funzionalità, scarta la dichiarazione come non valida [senza generare un errore](/it/docs/Web/CSS/Guides/Syntax/Error_handling#css_parser_errors). Poiché i browser scartano le proprietà e i valori CSS che non supportano, valori vecchi e nuovi possono coesistere nello stesso ruleset. Basta assicurarsi di dichiarare il vecchio valore prima di quello nuovo, in modo che, quando supportato, il nuovo valore sovrascriva il vecchio valore (il fallback).

Per esempio, la maggior parte dei browser supporta la sintassi a due valori della proprietà {{cssxref("display")}}. Se un browser non la supporta, userà la sintassi meno recente a valore singolo.

```css
.container {
  display: inline-flex;
  display: inline flex;
}
```

Analogamente, questa [gestione degli errori](/it/docs/Web/CSS/Guides/Syntax/Error_handling#vendor_prefixes) garantisce che le vecchie codebase CSS continuino a funzionare anche se le funzionalità legacy {{Glossary("Vendor_Prefix", "con prefisso del fornitore")}} non sono più supportate. Sebbene l'uso dei prefissi del fornitore non sia più comune, se è necessario includere una proprietà o un valore con prefisso del fornitore, assicurarsi di dichiarare il valore con prefisso prima del valore standard, in modo che, quando supportato, il nuovo valore sovrascriva il valore di fallback.

### Utilizzo di nuovi selettori

L'inclusione di nuovi selettori non supportati da tutti i browser richiede maggiore attenzione. Se un selettore in un elenco di [selettori separati da virgole non è valido](/it/docs/Learn_web_development/Extensions/Testing/HTML_and_CSS#selector_support), l'intero blocco di stile viene ignorato.

Se si usano [pseudo-elementi](/it/docs/Web/CSS/Reference/Selectors/Pseudo-elements) con prefisso del fornitore o nuove [pseudo-classi](/it/docs/Web/CSS/Reference/Selectors/Pseudo-classes) che un browser potrebbe non supportare ancora, includere i valori con prefisso all'interno di un [elenco di selettori permissivo](/it/docs/Web/CSS/Reference/Selectors/Selector_list#forgiving_selector_list) usando {{cssxref(":is", ":is()")}} o {{cssxref(":where", ":where()")}}, in modo che l'intero blocco di selettori non venga [invalidato e ignorato](/it/docs/Web/CSS/Reference/Selectors/Selector_list#invalid_selector_list).

```css
:is(:-prefix-mistake, :unsupported-pseudo),
.valid {
  font-family: sans-serif;
}
:-prefix-mistake,
:unsupported-pseudo,
.valid {
  color: red;
}
```

Nell'esempio precedente, il contenuto `.valid` avrà `sans-serif`, ma non `red`.

## Query sulle funzionalità

Le query sulle funzionalità consentono di verificare se un browser supporta una particolare funzionalità CSS. Questo significa che è possibile scrivere del CSS per i browser che non supportano una determinata funzionalità, quindi verificare se il browser la supporta e, in caso affermativo, aggiungere le nuove funzionalità avanzate.

È possibile aggiungere una query sulle funzionalità per verificare il supporto di `subgrid` e fornire stili in base a tale supporto:

```css
* {
  box-sizing: border-box;
}

.wrapper {
  background-color: palegoldenrod;
  padding: 10px;
  max-width: 400px;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
}

.item {
  border-radius: 5px;
  background-color: rgb(207 232 220);
}

@supports (grid-template-rows: subgrid) {
  .wrapper {
    grid-template-rows: subgrid;
    gap: 10px;
    background-color: lightblue;
    text-align: center;
  }
}
```

```html
<div class="wrapper">
  <div class="item">Item One</div>
  <div class="item">Item Two</div>
  <div class="item">Item Three</div>
  <div class="item">Item Four</div>
  <div class="item">Item Five</div>
  <div class="item">Item Six</div>
</div>
```

{{ EmbedLiveSample('Feature_queries', '100%', '200') }}

Le query sulle funzionalità sono supportate in tutti i browser moderni. Scrivere prima il CSS per le funzionalità pienamente supportate, al di fuori di qualsiasi query sulle funzionalità. Una volta che il sito è utilizzabile e accessibile a tutti gli utenti, aggiungere nuove funzionalità all'interno dei blocchi di query sulle funzionalità. I browser che supportano la funzionalità verificata potranno quindi eseguire il rendering del CSS più recente contenuto nel blocco della query sulle funzionalità. Adottare l'approccio di scrivere prima CSS ampiamente supportato, quindi migliorare le funzionalità in base al supporto.

## Test dei browser meno recenti

Un modo consiste nell'utilizzare uno strumento di test online come Sauce Labs, come illustrato nel modulo [Test](/it/docs/Learn_web_development/Extensions/Testing).

## Riepilogo

Ora sono disponibili le conoscenze per fornire CSS di fallback per i browser meno recenti e testare con sicurezza le nuove funzionalità. Si dovrebbe ora avere fiducia nell'utilizzare qualsiasi nuova tecnica che possa emergere.

Dopo aver esaminato gli articoli sul layout CSS, è il momento di verificare la propria comprensione con la valutazione del modulo: [Comprensione dei fondamenti del layout](/it/docs/Learn_web_development/Core/CSS_layout/Fundamental_Layout_Comprehension).

## Vedi anche

- At-rule {{cssxref("@supports")}}
- [At-rule CSS](/it/docs/Web/CSS/Guides/Syntax/At-rules)
- [Utilizzo delle query sulle funzionalità](/it/docs/Web/CSS/Guides/Conditional_rules/Using_feature_queries)
- Modulo sulle [regole condizionali CSS](/it/docs/Web/CSS/Guides/Conditional_rules)
