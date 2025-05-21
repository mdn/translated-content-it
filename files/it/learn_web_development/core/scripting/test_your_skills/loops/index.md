---
title: "Metti alla prova le tue abilità: Cicli"
short-title: Loops
slug: Learn_web_development/Core/Scripting/Test_your_skills/Loops
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Lo scopo di questo test di abilità è valutare se hai compreso il nostro articolo [Codice di cicli](/it/docs/Learn_web_development/Core/Scripting/Loops).

> [!NOTE]
> Puoi provare le soluzioni scaricando il codice e mettendolo in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
>
> Se incontri difficoltà, puoi contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Manipolazione del DOM: considerata utile

Alcune delle domande qui sotto richiedono di scrivere del codice di manipolazione del {{Glossary("DOM", "DOM")}} per completarle — come creare nuovi elementi HTML, impostare i loro contenuti di testo per uguagliare specifici valori stringa, e nidificarli all'interno di elementi esistenti sulla pagina — tutto tramite JavaScript.

Non abbiamo ancora spiegato esplicitamente questo nel corso, ma avrai visto alcuni esempi che ne fanno uso, e vorremmo che facessi alcune ricerche su quali API del DOM ti servono per rispondere con successo alle domande. Un buon punto di partenza è il nostro tutorial [Introduzione alla script del DOM](/it/docs/Learn_web_development/Core/Scripting/DOM_scripting).

## Cicli 1

Nel nostro primo compito sui cicli vogliamo che inizi creando un semplice ciclo che attraversi tutti gli elementi nel `myArray` fornito e li stampi sullo schermo all'interno di elementi di lista (ovvero, [`<li>`](/it/docs/Web/HTML/Reference/Elements/li)), che sono aggiunti alla `list` fornita.

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/tasks/loops/loops1-download.html) per lavorare nel tuo editor personale o in un editor online.

## Cicli 2

In questo prossimo compito, vogliamo che scrivi un programma semplice che, dato un nome, cerchi in un array di {{Glossary("Object", "oggetti")}} contenenti nomi e numeri di telefono (`phonebook`) e, se trova il nome, inserisca il nome e il numero di telefono nel paragrafo (`para`) e poi esca dal ciclo prima che abbia terminato il suo corso.

Se non hai ancora letto sugli oggetti, non preoccuparti! Per ora, tutto ciò che devi sapere è come accedere a una coppia membro-valore. Puoi informarti sugli oggetti nel tutorial [Concetti base sugli oggetti in JavaScript](/it/docs/Learn_web_development/Core/Scripting/Object_basics).

Ti vengono forniti tre variabili per iniziare:

- `name` — contiene un nome da cercare
- `para` — contiene un riferimento a un paragrafo, che sarà usato per riportare i risultati
- `phonebook` - contiene le voci della rubrica da cercare.

Dovresti usare un tipo di ciclo che non hai usato nel compito precedente.

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/tasks/loops/loops2-download.html) per lavorare nel tuo editor personale o in un editor online.

## Cicli 3

In questo compito finale, ti viene fornito quanto segue:

- `i` — parte con un valore di 500; inteso per essere usato come iteratore.
- `para` — contiene un riferimento a un paragrafo, che sarà usato per riportare i risultati.
- `isPrime()` — una funzione che, quando viene passata un numero, restituisce `true` se il numero è un numero primo, e `false` se non lo è.

Devi utilizzare un ciclo per esaminare i numeri da 2 a 500 all'indietro (1 non è considerato un numero primo), ed eseguire la funzione `isPrime()` fornita su di essi. Per ogni numero che non è primo, continua alla prossima iterazione del ciclo. Per ognuno che è un numero primo, aggiungilo al `textContent` del paragrafo insieme a un qualche tipo di separatore.

Dovresti usare un tipo di ciclo che non hai usato nei due compiti precedenti.

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/tasks/loops/loops3-download.html) per lavorare nel tuo editor personale o in un editor online.
