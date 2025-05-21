---
title: "Metti alla prova le tue competenze: Validazione dei moduli"
short-title: Validazione dei moduli
slug: Learn_web_development/Extensions/Forms/Test_your_skills/Form_validation
l10n:
  sourceCommit: 93f54b6e1fdfef1375233abb265f101bd6866f99
---

L'obiettivo di questo test di competenza è valutare se hai compreso il nostro articolo sulla [Validazione del modulo lato client](/it/docs/Learn_web_development/Extensions/Forms/Form_validation).

> [!NOTE]
> Puoi provare le soluzioni negli editor interattivi su questa pagina o in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/) o [Glitch](https://glitch.com/).
>
> Se ti trovi in difficoltà, puoi contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Validazione dei moduli 1

In questo compito, ti forniamo un semplice modulo di richiesta di supporto e vogliamo che tu aggiunga alcune funzionalità di validazione:

1. Rendi tutti i campi di input obbligatori da completare prima che il modulo possa essere inviato.
2. Modifica il tipo dei campi "Email address" e "Phone number" per far sì che il browser applichi una validazione più specifica adatta ai dati richiesti.
3. Dai al campo "User name" una lunghezza richiesta compresa tra 5 e 20 caratteri, un massimo di 15 caratteri al campo "Phone number" e un massimo di 200 caratteri al campo "Comment".

Prova a inviare il tuo modulo — dovrebbe rifiutare di inviare finché i vincoli sopra indicati non vengono rispettati e dare messaggi di errore adeguati. Per aiutarti, potresti considerare di aggiungere del semplice CSS per mostrare quando un campo del modulo è valido o non valido.

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/html/forms/tasks/form-validation/form-validation1-download.html) per lavorare nel tuo editor o in un editor online.

## Validazione dei moduli 2

Ora vogliamo che tu prenda lo stesso modulo visto nel compito precedente (usa la tua risposta precedente se lo desideri) e aggiungi una validazione di pattern più specifica ai primi tre campi usando le espressioni regolari.

1. Tutti i nomi utente nella nostra applicazione consistono in una singola lettera, seguita da un punto, seguita da tre o più lettere o numeri. Tutte le lettere devono essere minuscole.
2. Tutti gli indirizzi email per i nostri utenti consistono in una o più lettere (maiuscole o minuscole) o numeri, seguiti da "@bigcorp.eu".
3. Rimuovi la validazione della lunghezza dal campo del numero di telefono, se presente, e impostalo in modo che accetti 10 cifre — o 10 cifre di fila, o un pattern di tre cifre, tre cifre, quindi quattro cifre, separate da spazi, trattini o punti.

> [!NOTE]
> Le espressioni regolari sono davvero impegnative se sei nuovo a esse, ma non disperare — prova a vedere fin dove arrivi; non c'è vergogna a chiedere aiuto. Puoi trovare tutto ciò di cui hai bisogno per rispondere a queste domande nella nostra [riferimento sulle espressioni regolari](/it/docs/Web/JavaScript/Guide/Regular_expressions), e cercando su [Stack Overflow](https://stackoverflow.com/).

Ancora una volta, per aiutarti potresti voler considerare di aggiungere del semplice CSS per mostrare quando un campo del modulo è valido o non valido.

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/html/forms/tasks/form-validation/form-validation2-download.html) per lavorare nel tuo editor o in un editor online.

## Validazione dei moduli 3

Nel nostro compito finale per questo set, ti forniamo un esempio simile a quello che hai visto nell'articolo di accompagnamento — un input per inserire un indirizzo email. Vorremmo che tu utilizzassi l'API di validazione dei vincoli, oltre ad alcuni attributi di validazione del modulo, per programmare dei messaggi di errore personalizzati.

1. Rendi l'input obbligatorio da compilare e dagli una lunghezza minima di 10 caratteri.
2. Aggiungi un listener di eventi che verifichi se il valore immesso è un indirizzo email e se è abbastanza lungo. Se non sembra un indirizzo email o è troppo corto, fornisci all'utente dei messaggi di errore personalizzati appropriati.

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/html/forms/tasks/form-validation/form-validation3-download.html) per lavorare nel tuo editor o in un editor online.
