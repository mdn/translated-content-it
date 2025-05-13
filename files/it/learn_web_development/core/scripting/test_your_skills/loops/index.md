---
title: "Metti alla prova le tue abilità: Cicli"
short-title: Loops
slug: Learn_web_development/Core/Scripting/Test_your_skills/Loops
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

L'obiettivo di questo test di abilità è valutare se ha compreso il nostro articolo [Codice di loop](/it/docs/Learn_web_development/Core/Scripting/Loops).

> [!NOTE]
> Può provare le soluzioni scaricando il codice e inserendolo in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/) o [Glitch](https://glitch.com/).
>
> Se trova difficoltà, può contattarci attraverso uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Manipolazione del DOM: considerata utile

Alcune delle domande seguenti richiedono di scrivere del codice di manipolazione del {{Glossary("DOM", "DOM")}} per completarle — come creare nuovi elementi HTML, impostare il loro testo per eguagliare valori di stringa specifici e annidarli all'interno di elementi esistenti sulla pagina — tutto tramite JavaScript.

Non abbiamo ancora insegnato esplicitamente questo nel corso, ma avrà visto alcuni esempi che lo utilizzano e vorremmo che effettuasse alcune ricerche su quali API DOM siano necessarie per rispondere con successo alle domande. Un buon punto di partenza è il nostro tutorial [Introduzione al DOM scripting](/it/docs/Learn_web_development/Core/Scripting/DOM_scripting).

## Cicli 1

Nel nostro primo compito sui cicli vogliamo che inizi creando un semplice ciclo che attraversi tutti gli elementi dell'`myArray` fornito e li stampi sullo schermo all'interno di elementi di lista (ovvero elementi [`<li>`](/it/docs/Web/HTML/Reference/Elements/li)), che vengono aggiunti al `list` fornito.

> [!CALLOUT]
>
> [Scarichi il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/tasks/loops/loops1-download.html) per lavorare nel suo editor o in un editor online.

## Cicli 2

In questo prossimo compito, vogliamo che scriva un semplice programma che, dato un nome, cerchi un array di {{Glossary("Object", "oggetti")}} contenente nomi e numeri di telefono (`phonebook`) e, se trova il nome, visualizzi il nome e il numero di telefono nel paragrafo (`para`) e poi esca dal ciclo prima che si concluda.

Se non ha ancora letto sugli oggetti, non si preoccupi! Per ora, tutto ciò che deve sapere è come accedere a una coppia membro-valore. Può leggere sugli oggetti nel tutorial [Nozioni di base sugli oggetti JavaScript](/it/docs/Learn_web_development/Core/Scripting/Object_basics).

Le vengono fornite tre variabili con cui iniziare:

- `name` — contiene un nome da cercare
- `para` — contiene un riferimento a un paragrafo, che verrà usato per riferire i risultati
- `phonebook` — contiene le voci dell'elenco telefonico da cercare.

Dovrebbe usare un tipo di ciclo che non ha utilizzato nel compito precedente.

> [!CALLOUT]
>
> [Scarichi il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/tasks/loops/loops2-download.html) per lavorare nel suo editor o in un editor online.

## Cicli 3

In questo compito finale, le viene fornito quanto segue:

- `i` — inizia con un valore di 500; inteso per essere usato come iteratore.
- `para` — contiene un riferimento a un paragrafo, che verrà usato per riferire i risultati.
- `isPrime()` — una funzione che, quando viene passato un numero, restituisce `true` se il numero è un numero primo, e `false` se no.

Deve usare un ciclo per attraversare i numeri da 2 a 500 al contrario (1 non è considerato un numero primo), e eseguire la funzione `isPrime()` fornita su di essi. Per ogni numero che non è un numero primo, continui alla prossima iterazione del ciclo. Per ognuno che è un numero primo, lo aggiunga al `textContent` del paragrafo insieme a qualche tipo di separatore.

Dovrebbe usare un tipo di ciclo che non ha utilizzato nei due compiti precedenti.

> [!CALLOUT]
>
> [Scarichi il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/tasks/loops/loops3-download.html) per lavorare nel suo editor o in un editor online.
