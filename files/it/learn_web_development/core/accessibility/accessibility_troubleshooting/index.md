---
title: "Sfida: Risoluzione dei problemi di accessibilità"
short-title: "Sfida: Debugging A11y"
slug: Learn_web_development/Core/Accessibility/Accessibility_troubleshooting
l10n:
  sourceCommit: 1b7c3c1e03f14c3878e4d8518b0f1a89bedfdc9c
---

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/Mobile","Learn_web_development/Core/Design_for_developers", "Learn_web_development/Core/Accessibility")}}

Nella sfida di questo modulo, viene presentato un semplice sito con diversi problemi di accessibilità da diagnosticare e risolvere.

## Punto di partenza

Per iniziare questa sfida, scaricare il [file ZIP contenente i file che compongono l'esempio](https://raw.githubusercontent.com/mdn/learning-area/main/accessibility/assessment-start/assessment-files.zip). Decomprimere il contenuto in una nuova directory sul computer locale.

In alternativa, è possibile usare un editor online come [CodePen](https://codepen.io/) o [JSFiddle](https://jsfiddle.net/).

> [!NOTE]
> Se si rimane bloccati, è possibile contattarci tramite uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Descrizione del progetto

Per questo progetto, viene presentato un sito fittizio dedicato alla natura che mostra un articolo "informativo" sugli orsi. Nello stato attuale, presenta diversi problemi di accessibilità: il compito consiste nell'esplorare il sito esistente e risolverli al meglio delle proprie capacità, rispondendo alle domande riportate di seguito.

### Colore

Il testo è difficile da leggere a causa dell'attuale combinazione di colori. È possibile eseguire un test dell'attuale contrasto dei colori (testo/sfondo), riportarne i risultati e poi risolvere il problema modificando i colori assegnati?

### HTML semantico

1. Il contenuto non è ancora molto accessibile: descrivere cosa succede quando si prova a navigarlo usando uno screen reader.
2. È possibile aggiornare il testo dell'articolo per renderlo più semplice da navigare per gli utenti di screen reader?
3. La parte del sito relativa al menu di navigazione (racchiusa in `<div class="nav"></div>`) potrebbe essere resa più accessibile inserendola in un elemento HTML semantico appropriato. Quale elemento dovrebbe essere usato? Apportare l'aggiornamento.

> [!NOTE]
> Sarà necessario aggiornare i selettori delle regole CSS che applicano lo stile ai tag, sostituendoli con gli equivalenti appropriati per le intestazioni semantiche. Dopo aver aggiunto gli elementi paragrafo, lo stile risulterà migliore.

### Le immagini

Le immagini non sono attualmente accessibili agli utenti di screen reader. È possibile risolvere questo problema?

### Il lettore audio

1. Il lettore `<audio>` non è accessibile alle persone con disabilità uditive (sorde): è possibile aggiungere un qualche tipo di alternativa accessibile per questi utenti?
2. Il lettore `<audio>` non è accessibile a chi usa browser meno recenti che non supportano l'audio HTML. Come è possibile consentire loro di accedere comunque all'audio?

### I moduli

1. L'elemento `<input>` nel modulo di ricerca in alto potrebbe avere un'etichetta, ma non si desidera aggiungere un'etichetta testuale visibile che potrebbe compromettere il design e non è realmente necessaria agli utenti vedenti. Come è possibile aggiungere un'etichetta accessibile solo agli screen reader?
2. I due elementi `<input>` nel modulo dei commenti hanno etichette di testo visibili, ma non sono associati in modo inequivocabile alle rispettive etichette: come si ottiene questa associazione? Si noti che sarà necessario aggiornare anche alcune regole CSS.

### Il controllo per mostrare/nascondere i commenti

Il pulsante di controllo per mostrare/nascondere i commenti non è attualmente accessibile tramite tastiera. È possibile renderlo accessibile tramite tastiera, sia per quanto riguarda la messa a fuoco con il tasto Tab sia per l'attivazione con il tasto Invio?

### La tabella

La tabella di dati non è attualmente molto accessibile: per gli utenti di screen reader è difficile associare tra loro righe e colonne di dati, e la tabella non dispone nemmeno di un riepilogo che chiarisca cosa mostra. È possibile aggiungere alcune funzionalità all'HTML per risolvere questo problema?

### Altre considerazioni?

È possibile elencare altre due idee di miglioramento che renderebbero il sito web più accessibile?

## Esempio

Il sito della sfida completato dovrebbe avere un aspetto simile a questo:

![Screenshot del sito della sfida completato con un buon contrasto dei colori. L'input di ricerca contiene testo segnaposto e un pulsante di invio con la scritta "go", ma nessuna etichetta visibile.](assessment-site-finished.png)

<details>
<summary>Fare clic qui per la soluzione</summary>

Consultare il [codice dell'esempio completato](https://github.com/mdn/learning-area/tree/main/accessibility/assessment-finished).

</details>

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/Mobile","Learn_web_development/Core/Design_for_developers", "Learn_web_development/Core/Accessibility")}}
