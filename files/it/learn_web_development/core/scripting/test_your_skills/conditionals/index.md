---
title: "Metti alla prova le tue abilità: Condizionali"
short-title: Conditionals
slug: Learn_web_development/Core/Scripting/Test_your_skills/Conditionals
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Lo scopo di questo test di abilità è valutare se hai compreso il nostro articolo [Prendere decisioni nel tuo codice — condizionali](/it/docs/Learn_web_development/Core/Scripting/Conditionals).

> [!NOTE]
> Puoi provare le soluzioni scaricando il codice e inserendolo in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/) o [Glitch](https://glitch.com/).
> Se c'è un errore, verrà registrato nel pannello dei risultati sulla pagina o nella console JavaScript del browser per aiutarti.
>
> Se rimani bloccato, puoi contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Condizionali 1

In questo compito ti vengono forniti due variabili:

- `season` — contiene una stringa che indica qual è la stagione attuale.
- `response` — inizia non inizializzato, ma viene successivamente usato per memorizzare una risposta che verrà stampata nel pannello di output.

Vogliamo che tu crei un condizionale che verifichi se `season` contiene la stringa "summer", e in tal caso assegna una stringa a `response` che fornisca un messaggio appropriato all'utente sulla stagione. In caso contrario, dovrebbe assegnare una stringa generica a `response` che dice all'utente che non sappiamo quale sia la stagione.

Per concludere, dovresti poi aggiungere un altro test che verifichi se `season` contiene la stringa "winter" e di nuovo assegna una stringa appropriata a `response`.

Prova ad aggiornare il codice live qui sotto per ricreare l'esempio finito:

{{EmbedGHLiveSample("learning-area/javascript/building-blocks/tasks/conditionals/conditionals1.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/tasks/conditionals/conditionals1-download.html) per lavorare nel tuo editor o in un editor online.

## Condizionali 2

Per questo compito ti vengono date tre variabili:

- `machineActive` — contiene un indicatore di se la segreteria telefonica è accesa o meno (`true`/`false`)
- `score` — Contiene il tuo punteggio in un gioco immaginario. Questo punteggio viene inserito nella segreteria telefonica, che fornisce una risposta per indicare quanto bene hai fatto.
- `response` — inizia non inizializzato, ma viene successivamente usato per memorizzare una risposta che verrà stampata nel pannello di output.

Devi creare una struttura `if...else` che verifichi se la macchina è accesa e metta un messaggio nella variabile `response` se non lo è, dicendo all'utente di accendere la macchina.

All'interno del primo `if...else`, devi annidare un altro `if...else` che metta messaggi appropriati nella variabile `response` a seconda del valore di `score` — se la macchina è accesa. I diversi test condizionali (e le risposte risultanti) sono i seguenti:

- Punteggio inferiore a 0 o superiore a 100 — "Questo non è possibile, si è verificato un errore."
- Punteggio da 0 a 19 — "Quello era un punteggio terribile — totale fallimento!"
- Punteggio da 20 a 39 — "Sai alcune cose, ma è un punteggio piuttosto basso. Necessita di miglioramento."
- Punteggio da 40 a 69 — "Hai fatto un lavoro passabile, niente male!"
- Punteggio da 70 a 89 — "Quello è un ottimo punteggio, conosci davvero le tue cose."
- Punteggio da 90 a 100 — "Che punteggio straordinario! Hai imbrogliato? Sei per davvero?"

Prova ad aggiornare il codice live qui sotto per ricreare l'esempio finito. Dopo aver inserito il tuo codice, prova a cambiare `machineActive` in `true`, per vedere se funziona.
Si noti che, per l'ambito di questo esercizio, la dichiarazione `Your score is __` rimarrà sullo schermo indipendentemente dal valore di `machineActive`.

{{EmbedGHLiveSample("learning-area/javascript/building-blocks/tasks/conditionals/conditionals2.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/tasks/conditionals/conditionals2-download.html) per lavorare nel tuo editor o in un editor online.

## Condizionali 3

Per il compito finale ti vengono date quattro variabili:

- `machineActive` — contiene un indicatore di se la macchina di accesso è accesa o meno (`true`/`false`).
- `pwd` — Contiene la password di accesso dell'utente.
- `machineResult` — inizia non inizializzato, ma viene successivamente usato per memorizzare una risposta che verrà stampata nel pannello di output, informando l'utente se la macchina è accesa.
- `pwdResult` — inizia non inizializzato, ma viene successivamente usato per memorizzare una risposta che verrà stampata nel pannello di output, informando l'utente se il suo tentativo di accesso è stato effettuato con successo.

Vorremmo che tu creassi una struttura `if...else` che verifichi se la macchina è accesa e metta un messaggio nella variabile `machineResult` che dica all'utente se è accesa o spenta.

Se la macchina è accesa, vogliamo anche che venga eseguito un secondo condizionale che verifichi se `pwd` è uguale a `cheese`. In tal caso, dovrebbe assegnare una stringa a `pwdResult` dicendo all'utente che ha effettuato l'accesso con successo. Se no, dovrebbe assegnare una stringa diversa a `pwdResult` dicendo all'utente che il suo tentativo di accesso non è andato a buon fine. Vorremmo che tu lo facessi in una singola riga, usando qualcosa che non sia una struttura `if...else`.

Prova ad aggiornare il codice live qui sotto per ricreare l'esempio finito:

{{EmbedGHLiveSample("learning-area/javascript/building-blocks/tasks/conditionals/conditionals3.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/tasks/conditionals/conditionals3-download.html) per lavorare nel tuo editor o in un editor online.
