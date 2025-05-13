---
title: "Metti alla prova le sue capacità: Condizionali"
short-title: Conditionals
slug: Learn_web_development/Core/Scripting/Test_your_skills/Conditionals
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

L'obiettivo di questo test di abilità è valutare se ha compreso il nostro articolo [Prendere decisioni nel suo codice — condizionali](/it/docs/Learn_web_development/Core/Scripting/Conditionals).

> [!NOTE]
> Può provare le soluzioni scaricando il codice e inserendolo in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/) o [Glitch](https://glitch.com/).
> Se c'è un errore, verrà registrato nel pannello dei risultati sulla pagina o nella console JavaScript del browser per aiutarla.
>
> Se si blocca, può contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Condizionali 1

In questo compito le vengono fornite due variabili:

- `season` — contiene una stringa che indica quale sia la stagione corrente.
- `response` — inizia non inizializzata, ma viene successivamente utilizzata per memorizzare una risposta che verrà stampata nel pannello di output.

Desideriamo che crei un condizionale che verifichi se `season` contiene la stringa "summer", e, in tal caso, assegni una stringa a `response` che fornisca all'utente un messaggio appropriato sulla stagione. In caso contrario, dovrebbe assegnare a `response` una stringa generica che dice all'utente che non sappiamo che stagione sia.

Per finire, dovrebbe poi aggiungere un altro test che verifichi se `season` contiene la stringa "winter", e nuovamente assegni una stringa appropriata a `response`.

Provi a aggiornare il codice live qui sotto per ricreare l'esempio finito:

{{EmbedGHLiveSample("learning-area/javascript/building-blocks/tasks/conditionals/conditionals1.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarichi il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/tasks/conditionals/conditionals1-download.html) per lavorare nel suo editor o in un editor online.

## Condizionali 2

Per questo compito le vengono fornite tre variabili:

- `machineActive` — contiene un indicatore del fatto che la segreteria telefonica sia accesa o meno (`true`/`false`)
- `score` — Contiene il suo punteggio in un gioco immaginario. Questo punteggio viene fornito alla segreteria telefonica, che fornisce una risposta per indicare come ha fatto.
- `response` — inizia non inizializzata, ma viene successivamente utilizzata per memorizzare una risposta che verrà stampata nel pannello di output.

Deve creare una struttura `if...else` che verifichi se la macchina è accesa e inserisca un messaggio nella variabile `response` se non lo è, dicendo all'utente di accenderla.

All'interno del primo `if...else`, deve nidificare un altro `if...else` che inserisca messaggi appropriati nella variabile `response` a seconda del valore di `score` — se la macchina è accesa. I diversi test condizionali (e le risposte risultanti) sono i seguenti:

- Punteggio inferiore a 0 o superiore a 100 — "Questo non è possibile, si è verificato un errore."
- Punteggio da 0 a 19 — "È stato un punteggio terribile — fallimento totale!"
- Punteggio da 20 a 39 — "Conosce alcune cose, ma è un punteggio piuttosto scarso. Serve miglioramento."
- Punteggio da 40 a 69 — "Ha fatto un lavoro passabile, niente male!"
- Punteggio da 70 a 89 — "È un ottimo punteggio, conosce davvero le sue cose."
- Punteggio da 90 a 100 — "Che punteggio incredibile! Ha barato? È reale?"

Provi a aggiornare il codice live qui sotto per ricreare l'esempio finito. Dopo aver inserito il suo codice, provi a cambiare `machineActive` in `true`, per vedere se funziona.
Si ricordi che, per lo scopo di questo esercizio, l'affermazione `Il suo punteggio è __` rimarrà sullo schermo indipendentemente dal valore di `machineActive`.

{{EmbedGHLiveSample("learning-area/javascript/building-blocks/tasks/conditionals/conditionals2.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarichi il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/tasks/conditionals/conditionals2-download.html) per lavorare nel suo editor o in un editor online.

## Condizionali 3

Per l'ultimo compito le vengono fornite quattro variabili:

- `machineActive` — contiene un indicatore del fatto che la macchina di accesso sia accesa o meno (`true`/`false`).
- `pwd` — Contiene la password di accesso dell'utente.
- `machineResult` — inizia non inizializzata, ma viene successivamente utilizzata per memorizzare una risposta che verrà stampata nel pannello di output, informando l'utente se la macchina è accesa.
- `pwdResult` — inizia non inizializzata, ma viene successivamente utilizzata per memorizzare una risposta che verrà stampata nel pannello di output, informando l'utente se il tentativo di accesso è stato corretto.

Vogliamo che crei una struttura `if...else` che verifichi se la macchina è accesa e inserisca un messaggio nella variabile `machineResult` che dica all'utente se è accesa o spenta.

Se la macchina è accesa, vogliamo anche che venga eseguito un secondo condizionale che verifichi se `pwd` è uguale a `cheese`. In tal caso, dovrebbe assegnare a `pwdResult` una stringa che dice all'utente che l'accesso è avvenuto con successo. In caso contrario, dovrebbe assegnare a `pwdResult` una stringa diversa che informa l'utente che il tentativo di accesso non è andato a buon fine. Vogliamo che questo sia fatto in un'unica riga, utilizzando qualcosa che non sia una struttura `if...else`.

Provi a aggiornare il codice live qui sotto per ricreare l'esempio finito:

{{EmbedGHLiveSample("learning-area/javascript/building-blocks/tasks/conditionals/conditionals3.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarichi il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/tasks/conditionals/conditionals3-download.html) per lavorare nel suo editor o in un editor online.
