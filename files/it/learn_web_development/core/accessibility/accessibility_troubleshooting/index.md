---
title: "Sfida: Risoluzione problemi di accessibilità"
short-title: "Sfida: Debugging A11y"
slug: Learn_web_development/Core/Accessibility/Accessibility_troubleshooting
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/Mobile","Learn_web_development/Core/Design_for_developers", "Learn_web_development/Core/Accessibility")}}

Nella sfida di questo modulo, vi presentiamo un semplice sito con una serie di problemi di accessibilità che dovete diagnosticare e correggere.

## Punto di partenza

Per iniziare questa sfida, dovresti scaricare lo [ZIP contenente i file che compongono l'esempio](https://raw.githubusercontent.com/mdn/learning-area/main/accessibility/assessment-start/assessment-files.zip). Decomprimi il contenuto in una nuova directory sul tuo computer locale.

In alternativa, potresti utilizzare un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/) o [Glitch](https://glitch.com/).

Il sito finale della sfida dovrebbe apparire così:

![Screenshot del sito finale della sfida con un buon contrasto colore. L'input di ricerca ha un testo segnaposto e un pulsante di invio che dice "go", ma nessuna etichetta visibile.](assessment-site-finished.png)

Vedrai alcune differenze/problemi con la visualizzazione dello stato iniziale della sfida — questo è principalmente dovuto alle differenze nel markup, che a loro volta causano alcuni problemi di stile poiché il CSS non viene applicato correttamente. Non preoccuparti — risolverai questi problemi nelle sezioni seguenti!

> [!NOTE]
> Se ti blocchi, puoi contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Breve progetto

Per questo progetto, ti viene presentato un sito fittizio sulla natura che visualizza un articolo "fattuale" sugli orsi. Come si presenta attualmente, ha una serie di problemi di accessibilità — il tuo compito è esplorare il sito esistente e correggerli al meglio delle tue capacità, rispondendo alle domande indicate di seguito.

### Colore

Il testo è difficile da leggere a causa dello schema di colori attuale. Puoi effettuare un test del contrasto di colore attuale (testo/sfondo), riportare i risultati del test e poi correggerlo cambiando i colori assegnati?

### HTML Semantico

1. Il contenuto non è ancora molto accessibile — segnala cosa succede quando provi a navigarlo usando un lettore di schermo.
2. Puoi aggiornare il testo dell'articolo per renderlo più facile da navigare per gli utenti di lettori di schermo?
3. La parte del menu di navigazione del sito (racchiusa in `<div class="nav"></div>`) potrebbe essere resa più accessibile inserendola in un corretto elemento semantico HTML. A quale dovrebbe essere aggiornata? Fai l'aggiornamento.

> [!NOTE]
> Dovrai aggiornare i selettori delle regole CSS che stilizzano i tag ai loro equivalenti corretti per le intestazioni semantiche. Una volta aggiunti gli elementi paragrafo, noterai che lo stile appare migliore.

### Le immagini

Le immagini sono attualmente inaccessibili agli utenti di lettori di schermo. Puoi correggerlo?

### Il lettore audio

1. Il lettore `<audio>` non è accessibile alle persone con problemi di udito (sordità): puoi aggiungere un'alternativa accessibile per questi utenti?
2. Il lettore `<audio>` non è accessibile per coloro che utilizzano browser più vecchi che non supportano l'audio HTML. Come puoi consentire loro di accedere comunque all'audio?

### I moduli

1. L'elemento `<input>` nel modulo di ricerca in alto potrebbe avere un'etichetta, ma non vogliamo aggiungere un'etichetta con testo visibile che potrebbe potenzialmente rovinare il design e non è veramente necessaria agli utenti vedenti. Come puoi aggiungere un'etichetta che sia visibile solo ai lettori di schermo?
2. I due elementi `<input>` nel modulo dei commenti hanno etichette di testo visibili, ma non sono chiaramente associati alle loro etichette — come fai a ottenere questo? Nota che dovrai aggiornare anche alcune delle regole CSS.

### Il controllo mostra/nascondi commento

Il pulsante di controllo mostra/nascondi commento non è al momento accessibile tramite tastiera. Puoi renderlo accessibile tramite tastiera, sia in termini di focalizzazione con il tasto tab, sia di attivazione con il tasto invio?

### La tabella

La tabella dei dati non è al momento molto accessibile — è difficile per gli utenti di lettori di schermo associare righe e colonne di dati, e la tabella non ha alcun tipo di sommario che chiarisca cosa mostra. Puoi aggiungere alcune caratteristiche al tuo HTML per risolvere questo problema?

### Altre considerazioni?

Puoi elencare due altre idee per miglioramenti che renderebbero il sito web più accessibile?

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/Mobile","Learn_web_development/Core/Design_for_developers", "Learn_web_development/Core/Accessibility")}}
