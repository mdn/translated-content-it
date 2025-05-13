---
title: "Sfida: Risoluzione dei problemi di accessibilità"
short-title: "Sfida: Debugging A11y"
slug: Learn_web_development/Core/Accessibility/Accessibility_troubleshooting
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/Mobile","Learn_web_development/Core/Design_for_developers", "Learn_web_development/Core/Accessibility")}}

Nella sfida per questo modulo, le presentiamo un sito semplice con diversi problemi di accessibilità che deve diagnosticare e correggere.

## Punto di partenza

Per iniziare questa sfida, dovrebbe scaricare il [ZIP contenente i file che compongono l'esempio](https://raw.githubusercontent.com/mdn/learning-area/main/accessibility/assessment-start/assessment-files.zip). Decomprimere il contenuto in una nuova directory da qualche parte sul suo computer locale.

In alternativa, potrebbe utilizzare un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/) o [Glitch](https://glitch.com/).

Il sito finito della sfida dovrebbe apparire così:

![Screenshot del sito della sfida finita con un buon contrasto di colori. L'input di ricerca ha un testo segnaposto e un pulsante di invio che dice go, ma nessuna etichetta visibile.](assessment-site-finished.png)

Noterà alcune differenze/problematiche nella visualizzazione dello stato iniziale della sfida — questo è dovuto principalmente alle differenze nel markup, che a loro volta causano alcuni problemi di stile poiché il CSS non è applicato correttamente. Non si preoccupi — risolverà questi problemi nelle sezioni successive!

> [!NOTE]
> Se si blocca, può contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Breve progetto

Per questo progetto, le viene presentato un sito fittizio sulla natura che visualizza un articolo "fattuale" sugli orsi. Così com'è, presenta diversi problemi di accessibilità — il suo compito è quello di esplorare il sito esistente e correggerli nel miglior modo possibile, rispondendo alle domande elencate di seguito.

### Colore

Il testo è difficile da leggere a causa dell'attuale schema di colori. Può eseguire un test del contrasto attuale dei colori (testo/sfondo), riportare i risultati del test e quindi correggerlo cambiando i colori assegnati?

### HTML semantico

1. Il contenuto non è ancora molto accessibile — riporti cosa succede quando prova a navigarlo utilizzando un lettore di schermo.
2. Può aggiornare il testo dell'articolo per renderlo più facile da navigare per gli utenti del lettore di schermo?
3. La parte del menu di navigazione del sito (inclusa in `<div class="nav"></div>`) potrebbe essere resa più accessibile mettendola in un elemento semantico HTML appropriato. A quale elemento dovrebbe essere aggiornata? Effettui l'aggiornamento.

> [!NOTE]
> Dovrà aggiornare i selettori della regola CSS che stilizzano i tag ai loro equivalenti corretti per le intestazioni semantiche. Una volta aggiunti elementi di paragrafo, noterà che lo stile apparirà migliore.

### Le immagini

Le immagini sono attualmente inaccessibili agli utenti del lettore di schermo. Può risolvere questo problema?

### Il player audio

1. Il `<audio>` player non è accessibile alle persone con disabilità uditiva (sorde) — può aggiungere una sorta di alternativa accessibile per questi utenti?
2. Il `<audio>` player non è accessibile a coloro che utilizzano browser più vecchi che non supportano l'audio HTML. Come può permettere loro di accedere comunque all'audio?

### I form

1. L'elemento `<input>` nel form di ricerca in cima potrebbe aver bisogno di un'etichetta, ma non vogliamo aggiungere un'etichetta testuale visibile che potenzialmente rovinerebbe il design e non è davvero necessaria per gli utenti vedenti. Come può aggiungere un'etichetta accessibile solo ai lettori di schermo?
2. I due elementi `<input>` nel form dei commenti hanno etichette di testo visibili, ma non sono inequivocabilmente associati con le loro etichette — come può ottenere questo? Nota che dovrà aggiornare anche alcune delle regole CSS.

### Il controllo mostra/nascondi commento

Il pulsante di controllo mostra/nascondi commento non è attualmente accessibile tramite tastiera. Può renderlo accessibile tramite tastiera, sia in termini di metterlo a fuoco usando il tasto tab, sia attivandolo usando il tasto invio?

### La tabella

La tabella dei dati non è attualmente molto accessibile — è difficile per gli utenti del lettore di schermo associare le righe e le colonne dei dati insieme, e la tabella non ha alcun tipo di riepilogo che chiarisca cosa mostra. Può aggiungere alcune caratteristiche al suo HTML per risolvere questo problema?

### Altre considerazioni?

Può elencare altre due idee per miglioramenti che renderebbero il sito web più accessibile?

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/Mobile","Learn_web_development/Core/Design_for_developers", "Learn_web_development/Core/Accessibility")}}
