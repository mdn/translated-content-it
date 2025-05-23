---
title: Supportare i browser più vecchi
slug: Learn_web_development/Core/CSS_layout/Supporting_Older_Browsers
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

I visitatori del tuo sito web possono includere utenti che utilizzano browser più vecchi o browser che non supportano le funzionalità CSS che hai implementato. Questo è uno scenario comune sul web, dove nuove funzionalità vengono continuamente aggiunte al CSS. I browser differiscono nel loro supporto per queste funzionalità perché diversi browser tendono a dare priorità all'implementazione di diverse funzionalità. Questo articolo spiega come, come sviluppatore web, puoi utilizzare tecniche web moderne per garantire che il tuo sito rimanga accessibile agli utenti con tecnologia più datata.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Basi di HTML (studia
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Introduzione a HTML</a
        >), e un'idea di come funziona il CSS (studia
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Basi di styling CSS</a>.)
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere come fornire supporto per i tuoi layout su browser più vecchi
        che potrebbero non supportare le funzionalità che vuoi usare.
      </td>
    </tr>
  </tbody>
</table>

## Qual è il panorama dei browser per il tuo sito?

Ogni sito web è diverso in termini di pubblico di destinazione. Prima di decidere un approccio, scopri il numero di visitatori che arrivano al tuo sito utilizzando browser più vecchi. Questo è semplice se stai aggiungendo o sostituendo un sito web esistente, poiché probabilmente hai analisi disponibili che possono dirti la tecnologia che i tuoi visitatori stanno usando. Se non hai statistiche o stai lanciando un sito completamente nuovo, allora siti come [Statcounter](https://gs.statcounter.com/) possono fornire statistiche rilevanti, che possono essere filtrate per località.

Dovresti anche considerare il tipo di dispositivi e il modo in cui le persone usano il tuo sito. Ad esempio, puoi aspettarti un uso superiore alla media del tuo sito su dispositivi mobili. Dai sempre priorità all'accessibilità e alle persone che usano tecnologia assistiva; per alcuni siti, questo potrebbe essere ancora più critico. Gli sviluppatori sono spesso molto preoccupati per l'esperienza dell'1% degli utenti, trascurando il numero molto maggiore che ha bisogni di accessibilità.

## Qual è il supporto per le funzionalità che vuoi usare?

{{Compat}}

La tabella sopra è inclusa in fondo a ogni pagina di funzionalità nella sezione "Compatibilità del browser". Dopo aver identificato i browser che i visitatori del tuo sito usano, puoi valutare qualsiasi tecnologia che vuoi usare rispetto a quanto bene è supportata tra i browser e quanto facilmente puoi fornire un'alternativa per i visitatori che non dispongono di quella tecnologia.

Su MDN, forniamo informazioni sulla compatibilità del browser su ogni pagina delle proprietà CSS. Queste informazioni sulla compatibilità, presentate in una tabella, includono un elenco dei principali browser insieme alle versioni che hanno iniziato a supportare la proprietà. I nomi dei browser occupano le intestazioni delle colonne. Ad esempio, dai un'occhiata alla tabella sopra o alla pagina per {{cssxref("grid-template-columns")}}, con particolare attenzione ai valori `subgrid` (recentemente supportato) e `masonry` (sperimentale e non supportato).

Queste tabelle di compatibilità del browser forniscono informazioni su quali browser sono compatibili con la tecnologia che stai cercando e la versione dalla quale il browser ha iniziato a supportare quella funzionalità. Le informazioni sulla compatibilità del browser e dei browser mobili sono visualizzate separatamente.

Un altro modo popolare per scoprire quanto bene una funzionalità è supportata è il sito web [Can I Use](https://caniuse.com/). Questo sito elenca la maggior parte delle funzionalità della Piattaforma Web con informazioni sul loro stato di supporto nei browser. Puoi visualizzare le statistiche d'uso per località — utile se lavori su un sito che ha utenti principalmente per una specifica area del mondo. Puoi anche collegare il tuo account Google Analytics per ottenere un'analisi basata sui dati degli utenti.

Comprendere la tecnologia che gli utenti hanno a causa del browser che stanno usando e il supporto incrociato dei browser per le funzionalità che potresti voler usare sul tuo sito web, ti mette in una buona posizione per prendere tutte le tue decisioni e sapere come supportare al meglio tutti i tuoi utenti.

## Il supporto delle funzionalità non significa aspetto identico

Un sito web non può avere lo stesso aspetto in tutti i browser. Alcuni dei tuoi utenti visualizzeranno il sito su un telefono e altri su uno schermo desktop grande. Allo stesso modo, alcuni dei tuoi utenti avranno una vecchia versione del browser, e altri l'ultimo browser. Alcuni dei tuoi utenti potrebbero ascoltare i tuoi contenuti letti da un lettore di schermo, mentre altri potrebbero aver bisogno di ingrandire la pagina per poterla leggere. Supportare tutti significa fornire una versione dei tuoi contenuti progettata in modo difensivo, in modo che apparirà fantastica sui browser moderni, ma sarà comunque utilizzabile a un livello di base per tutti gli utenti indipendentemente da come stanno accedendo ai tuoi contenuti.

Un livello base di supporto deriva dalla strutturazione corretta dei tuoi contenuti in modo che il flusso normale della tua pagina abbia senso. Per gli utenti con un piano dati limitato, i loro browser potrebbero non caricare immagini, font o persino il tuo CSS. Tuttavia, i contenuti dovrebbero essere presentati in modo tale da essere accessibili e leggibili anche quando questi elementi non sono completamente caricati. Un documento HTML ben strutturato dovrebbe sempre essere il tuo punto di partenza. Chiediti: _se rimuovi il tuo foglio di stile, i tuoi contenuti hanno ancora senso?_

Non ha senso commerciale trascorrere del tempo cercando di offrire a tutti un'esperienza identica del tuo sito web. Questo perché gli ambienti degli utenti possono variare ampiamente e sono al di là del tuo controllo. C'è un equilibrio che devi trovare tra una pagina HTML semplice e un sito web completamente funzionale. È utile testare una vista semplice, senza CSS del tuo sito per garantire che l'esperienza di ripiego del tuo sito sia accessibile. Questo ripiego potrebbe non essere mai visualizzato da persone che usano browser molto vecchi o limitati, ma potrebbe essere visualizzato dal tuo pubblico principale — utenti dei browser moderni — quando il loro browser o la connessione Internet fallisce temporaneamente. Il CSS semplifica la creazione di questi ripieghi. Pertanto, è meglio concentrarsi su ciò che puoi controllare, cioè, dedicare tempo a rendere il tuo sito [accessibile](/it/docs/Web/Accessibility), servendo così più utenti.

## Creare fallback in CSS

Le specifiche CSS contengono informazioni che spiegano cosa fa il browser quando due funzionalità simili, come i metodi di layout, vengono applicate allo stesso elemento. Ad esempio, definiscono cosa succede se un elemento è floatato ed è anche un elemento di griglia e parte di un contenitore griglia CSS. C'è anche una definizione di cosa succede quando un elemento ha sia le proprietà {{cssxref("margin-top")}} che {{cssxref("margin-block-start")}} impostate.

Quando un browser non riconosce una nuova funzionalità, ne scarta la dichiarazione come non valida [senza generare un errore](/it/docs/Web/CSS/CSS_syntax/Error_handling#css_parser_errors). Poiché i browser scartano le proprietà e i valori CSS che non supportano, i valori vecchi e nuovi possono coesistere nello stesso insieme di regole. Assicurati solo di dichiarare il valore vecchio prima di quello nuovo in modo che, una volta supportato, il valore nuovo sovrascriva quello vecchio (il fallback).

Ad esempio, la maggior parte dei browser supporta la sintassi a due valori della proprietà {{cssxref("display")}}. Se un browser non lo fa, utilizzerà la vecchia sintassi a singolo valore.

```css
.container {
  display: inline-flex;
  display: inline flex;
}
```

Allo stesso modo, questa [gestione degli errori](/it/docs/Web/CSS/CSS_syntax/Error_handling#vendor_prefixes) garantisce che i vecchi codici base CSS continuino a funzionare anche se le funzionalità vecchie {{Glossary("Vendor_Prefix", "con prefisso del venditore")}} non sono più supportate. Sebbene il prefisso del venditore non sia più comunemente usato, se devi includere una proprietà o un valore con prefisso, assicurati di dichiarare il valore con prefisso prima del valore standard in modo che, quando supportato, il valore nuovo sovrascriva il valore di fallback.

### Usare nuovi selettori

L'inclusione di nuovi selettori che non sono supportati in tutti i browser deve essere gestita con maggiore attenzione. Se un selettore in un elenco separato da virgole di [selettori è invalid](/it/docs/Learn_web_development/Extensions/Testing/HTML_and_CSS#selector_support), l'intero blocco di stile viene ignorato.

Se utilizzi [pseudo-elementi](/it/docs/Web/CSS/Pseudo-elements) con prefisso del venditore o nuove [pseudo-classi](/it/docs/Web/CSS/Pseudo-classes) che un browser potrebbe non supportare ancora, includi i valori con prefisso all'interno di un elenco di selettori [permissivi](/it/docs/Web/CSS/Selector_list#forgiving_selector_list) usando {{cssxref(":is", ":is()")}} o {{cssxref(":where", ":where()")}} in modo che l'intero blocco selettore non venga [invalidato e ignorato](/it/docs/Web/CSS/Selector_list#invalid_selector_list).

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

Nell'esempio sopra, il contenuto `.valid` sarà `sans-serif` ma non `red`.

## Interrogazioni sulle funzionalità

Le interrogazioni sulle funzionalità ti permettono di verificare se un browser supporta una particolare funzionalità CSS. Ciò significa che puoi scrivere del CSS per browser che non supportano una determinata funzionalità, poi controllare se il browser ha il supporto e, in tal caso, inserire le tue nuove funzionalità avanzate.

Possiamo aggiungere un'interrogazione sulle funzionalità per testare il supporto di `subgrid`, e fornire stili basati su tale supporto:

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

Le interrogazioni sulle funzionalità sono supportate in tutti i browser moderni. Scrivi prima il tuo CSS per le funzionalità completamente supportate, al di fuori di qualsiasi interrogazione sulle funzionalità. Una volta che il tuo sito è utilizzabile e accessibile a tutti gli utenti, aggiungi nuove funzionalità all'interno dei blocchi di interrogazione delle funzionalità. I browser che supportano la funzionalità interrogata possono quindi rendere il nuovo CSS all'interno del blocco di interrogazione delle funzionalità. Usa l'approccio di scrivere primo il CSS ben supportato, poi migliorare le funzionalità basandoti sul supporto.

## Testare browser più vecchi

Un modo è utilizzare uno strumento di test online come Sauce Labs, come dettagliato nel modulo [Collaudo](/it/docs/Learn_web_development/Extensions/Testing).

## Sommario

Ora hai la conoscenza per fornire fallback CSS per i browser più vecchi e testare con sicurezza le nuove funzionalità. Dovresti ora sentirti sicuro di utilizzare qualsiasi nuova tecnica che potrebbe emergere.

Ora che hai lavorato attraverso i nostri articoli sul layout CSS, è tempo di testare la tua comprensione con la nostra valutazione per il modulo: [Comprensione fondamentale del layout](/it/docs/Learn_web_development/Core/CSS_layout/Fundamental_Layout_Comprehension).

## Vedi anche

- [`@supports`](/it/docs/Web/CSS/@supports) at-rule
- [Regole at-rules CSS](/it/docs/Web/CSS/CSS_syntax/At-rule)
- [Usare le interrogazioni sulle funzionalità](/it/docs/Web/CSS/CSS_conditional_rules/Using_feature_queries)
- Modulo [Regole condizionali CSS](/it/docs/Web/CSS/CSS_conditional_rules)
