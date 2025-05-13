---
title: Supporto per i browser meno recenti
slug: Learn_web_development/Core/CSS_layout/Supporting_Older_Browsers
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

I visitatori del suo sito web potrebbero includere utenti che utilizzano browser meno recenti o che utilizzano browser che non supportano le funzionalità CSS che ha implementato. Questo è uno scenario comune sul web, dove nuove funzionalità vengono continuamente aggiunte a CSS. I browser differiscono nel loro supporto per queste funzionalità perché diversi browser tendono a dare priorità all'implementazione di funzionalità differenti. Questo articolo spiega come lei, come sviluppatore web, può utilizzare tecniche web moderne per garantire che il suo sito web rimanga accessibile agli utenti con tecnologie meno recenti.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Nozioni di base su HTML (studiare
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Introduzione a HTML</a
        >), e un'idea di come funziona CSS (studiare
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Nozioni di base sulla stile CSS</a>.)
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Capire come fornire supporto per i suoi layout su browser meno recenti che potrebbero non supportare le funzionalità che desidera utilizzare.
      </td>
    </tr>
  </tbody>
</table>

## Qual è il panorama dei browser per il suo sito?

Ogni sito web è diverso in termini di pubblico di riferimento. Prima di decidere un approccio, scopra il numero di visitatori che accedono al suo sito utilizzando browser meno recenti. Questo è semplice se sta aggiungendo o sostituendo un sito esistente, dato che probabilmente dispone di analisi disponibili che possono dirle la tecnologia che i suoi visitatori utilizzano. Se non ha analisi o sta lanciando un sito completamente nuovo, siti come [Statcounter](https://gs.statcounter.com/) possono fornire statistiche pertinenti, che possono essere filtrate per posizione.

Dovrebbe anche considerare il tipo di dispositivi e il modo in cui le persone utilizzano il suo sito. Ad esempio, può aspettarsi un utilizzo superiore alla media del suo sito web su dispositivi mobili. Dia sempre priorità all'accessibilità e alle persone che utilizzano tecnologie assistive; per alcuni siti, questo potrebbe essere ancora più critico. Gli sviluppatori sono spesso molto preoccupati per l'esperienza dell'1% degli utenti, trascurando il numero molto maggiore di persone che hanno esigenze di accessibilità.

## Qual è il supporto per le funzionalità che vuole utilizzare?

{{Compat}}

La tabella sopra è inclusa in fondo a ogni pagina delle funzionalità nella sezione "Compatibilità del browser". Dopo aver identificato i browser utilizzati dai suoi visitatori, può valutare qualsiasi tecnologia che desidera utilizzare rapportandola a quanto bene è supportata tra i vari browser e quanto facilmente può fornire un'alternativa per i visitatori che non dispongono di quella tecnologia.

Su MDN, forniamo informazioni sulla compatibilità del browser in ogni pagina delle proprietà CSS. Queste informazioni di compatibilità, presentate in una tabella, includono un elenco dei principali browser insieme alle versioni che hanno iniziato a supportare la proprietà. I nomi dei browser occupano le intestazioni delle colonne. Ad esempio, dia un'occhiata alla tabella sopra o alla pagina per {{cssxref("grid-template-columns")}}, con particolare attenzione ai valori `subgrid` (recentemente supportati) e `masonry` (sperimentali e non supportati).

Queste tabelle di compatibilità dei browser forniscono informazioni su quali browser sono compatibili con la tecnologia che sta cercando e dalla versione in cui il browser ha iniziato a supportare quella funzionalità. Le informazioni sulla compatibilità dei browser e dei browser mobili sono visualizzate separatamente.

Un altro modo popolare per scoprire quanto bene è supportata una funzionalità è il sito [Can I Use](https://caniuse.com/). Questo sito elenca la maggior parte delle funzionalità della Piattaforma Web con informazioni sul loro stato di supporto nei browser. Può visualizzare le statistiche di utilizzo per localizzazione — utile se lavora su un sito che ha utenti prevalentemente in una specifica area del mondo. Può anche collegare il suo account Google Analytics per ottenere un'analisi basata sui dati degli utenti.

Comprendere la tecnologia che i suoi utenti hanno in base al browser che stanno utilizzando e il supporto cross-browser per le funzionalità che potrebbe voler utilizzare sul suo sito web, la mette in una buona posizione per prendere tutte le sue decisioni e sapere come supportare meglio tutti i suoi utenti.

## Il supporto delle funzionalità non significa aspetto identico

Un sito web non può apparire identico in tutti i browser. Alcuni dei suoi utenti visualizzeranno il sito su un telefono e altri su uno schermo desktop grande. Allo stesso modo, alcuni dei suoi utenti avranno una versione di browser vecchia, e altri l'ultimo browser. Alcuni dei suoi utenti potrebbero ascoltare il suo contenuto letto da un lettore di schermo, mentre altri potrebbero aver bisogno di ingrandire la pagina per poterla leggere. Supportare tutti significa offrire una versione del suo contenuto progettata in modo difensivo, in modo che appaia fantastico sui browser moderni, ma sia comunque utilizzabile a un livello base per tutti gli utenti, indipendentemente da come stanno accedendo al suo contenuto.

Un livello base di supporto deriva dalla strutturazione bene del suo contenuto in modo tale che il flusso normale della sua pagina abbia senso. Per gli utenti con un piano dati limitato, i loro browser potrebbero non caricare immagini, font o anche il suo CSS. Tuttavia, il contenuto dovrebbe essere presentato in modo tale da essere accessibile e leggibile anche quando questi elementi non sono completamente caricati. Un documento HTML ben strutturato dovrebbe essere sempre il suo punto di partenza. Chieda a se stesso: _se rimuovesse il suo foglio di stile, il suo contenuto avrebbe ancora senso?_

Non ha senso commerciale spendere tempo cercando di dare a tutti un'esperienza identica del suo sito web. Questo perché gli ambienti degli utenti possono variare ampiamente e sono al di fuori del suo controllo. Deve trovare un equilibrio tra una pagina HTML semplice e un sito web con tutte le funzionalità. È utile testare una vista semplice, senza CSS del suo sito per garantire che l'esperienza di fallback del suo sito sia accessibile. Questo fallback potrebbe non essere mai visualizzato dalle persone che utilizzano browser molto vecchi o limitati, ma potrebbe essere visualizzato dal suo pubblico principale — utenti di browser moderni — quando il loro browser o connessione Internet si interrompe temporaneamente. CSS semplifica la creazione di questi fallback. Pertanto, è meglio concentrarsi su ciò che può controllare, cioè, dedicare il tempo a rendere il suo sito [accessibile](/it/docs/Web/Accessibility), servendo così più utenti.

## Creare fallback in CSS

Le specifiche CSS contengono informazioni che spiegano cosa fa il browser quando vengono applicate due funzionalità simili, come i metodi di layout, allo stesso elemento. Ad esempio, definiscono cosa succede se un elemento è flottante ed è anche un elemento di griglia e parte di un contenitore di griglia CSS. C'è anche una definizione per cosa succede quando un elemento ha impostate sia le proprietà {{cssxref("margin-top")}} che {{cssxref("margin-block-start")}}.

Quando un browser non riconosce una nuova funzionalità, scarta la dichiarazione come non valida [senza generare un errore](/it/docs/Web/CSS/CSS_syntax/Error_handling#css_parser_errors). Poiché i browser scartano proprietà e valori CSS che non supportano, sia i vecchi che i nuovi valori possono coesistere nello stesso set di regole. Assicurarsi solo di dichiarare il vecchio valore prima del nuovo valore in modo che, quando supportato, il nuovo valore sovrascriva il vecchio (il fallback).

Ad esempio, la maggior parte dei browser supporta la sintassi a due valori della proprietà {{cssxref("display")}}. Se un browser non lo supporta, utilizzerà la sintassi a un valore singolo più vecchia.

```css
.container {
  display: inline-flex;
  display: inline flex;
}
```

Allo stesso modo, questa [gestione degli errori](/it/docs/Web/CSS/CSS_syntax/Error_handling#vendor_prefixes) garantisce che i vecchi basi di codice CSS continuino a funzionare anche se le funzionalità con prefisso del fornitore {{Glossary("Vendor_Prefix", "vendor-prefixed")}} non sono più supportate. Sebbene il prefisso del fornitore non sia più comunemente usato, se deve includere una proprietà o un valore con prefisso del fornitore, assicurarsi di dichiarare il valore con prefisso prima del valore standard in modo che, quando supportato, il nuovo valore sovrascriva il valore di fallback.

### Uso di nuovi selettori

Includere nuovi selettori non supportati su tutti i browser deve essere gestito con maggiore attenzione. Se un selettore in un elenco separato da virgola di [selettori è non valido](/it/docs/Learn_web_development/Extensions/Testing/HTML_and_CSS#selector_support), l'intero blocco di stile viene ignorato.

Se si utilizzano [pseudo-elementi](/it/docs/Web/CSS/Pseudo-elements) con prefisso del fornitore o nuove [pseudo-classi](/it/docs/Web/CSS/Pseudo-classes) che un browser potrebbe non supportare ancora, includere i valori con prefisso all'interno di un [elenco di selettori tollerante](/it/docs/Web/CSS/Selector_list#forgiving_selector_list) usando {{cssxref(":is", ":is()")}} o {{cssxref(":where", ":where()")}} in modo che l'intero blocco di selettori non venga [invalidato e ignorato](/it/docs/Web/CSS/Selector_list#invalid_selector_list).

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

Nell'esempio sopra, il contenuto di `.valid` sarà `sans-serif` ma non `red`.

## Feature queries

Le feature queries consentono di verificare se un browser supporta una particolare funzionalità CSS. Questo significa che lei può scrivere del CSS per i browser che non supportano una certa funzionalità, quindi controllare se il browser ha supporto e, in tal caso, introdurre le sue nuove funzionalità avanzate.

Possiamo aggiungere una feature query per testare il supporto `subgrid`, e fornire stili basati su tale supporto:

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

Le feature queries sono supportate in tutti i browser moderni. Scriva il suo CSS per le funzionalità completamente supportate prima, fuori da qualsiasi feature query. Una volta che il suo sito è utilizzabile e accessibile a tutti gli utenti, aggiunga nuove funzionalità all'interno dei blocchi di feature query. I browser che supportano la funzionalità interrogata possono quindi rendere il nuovo CSS all'interno del blocco di feature query. Usi l'approccio di scrivere prima CSS ben supportato, quindi migliorare le funzionalità basandosi sul supporto.

## Testare i browser meno recenti

Un modo è utilizzare uno strumento di test online come Sauce Labs, come dettagliato nel modulo di [Testing](/it/docs/Learn_web_development/Extensions/Testing).

## Sommario

Ora ha la conoscenza per fornire CSS di fallback per i browser meno recenti e testare con sicurezza per nuove funzionalità. Dovrebbe ora sentirsi sicuro di utilizzare qualsiasi nuova tecnica che potrebbe emergere.

Ora che ha lavorato attraverso i nostri articoli sul layout CSS, è tempo di testare la sua comprensione con la nostra valutazione per il modulo: [Comprehensione fondamentale del layout](/it/docs/Learn_web_development/Core/CSS_layout/Fundamental_Layout_Comprehension).

## Veda anche

- [`@supports`](/it/docs/Web/CSS/@supports) regola at
- [Regole at CSS](/it/docs/Web/CSS/CSS_syntax/At-rule)
- [Uso delle feature queries](/it/docs/Web/CSS/CSS_conditional_rules/Using_feature_queries)
- [Modulo di regole condizionali CSS](/it/docs/Web/CSS/CSS_conditional_rules)
