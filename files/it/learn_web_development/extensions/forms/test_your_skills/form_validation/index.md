---
title: "Metti alla prova le tue abilità: Validazione dei moduli"
short-title: Validazione dei moduli
slug: Learn_web_development/Extensions/Forms/Test_your_skills/Form_validation
l10n:
  sourceCommit: 93f54b6e1fdfef1375233abb265f101bd6866f99
---

L'obiettivo di questo test di abilità è valutare se ha compreso il nostro articolo sulla [Validazione dei moduli lato client](/it/docs/Learn_web_development/Extensions/Forms/Form_validation).

> [!NOTE]
> Può provare le soluzioni negli editor interattivi su questa pagina o in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/) o [Glitch](https://glitch.com/).
>
> Se riscontra difficoltà, può contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Validazione del modulo 1

In questo compito, le forniamo un semplice modulo di richiesta di supporto e desideriamo che aggiunga alcune funzionalità di validazione:

1. Rendi tutti i campi di input obbligatori da completare prima che il modulo possa essere inviato.
2. Modifica il tipo dei campi "Email address" e "Phone number" per far sì che il browser applichi una validazione più specifica adatta ai dati richiesti.
3. Dai al campo "User name" una lunghezza richiesta compresa tra 5 e 20 caratteri, al campo "Phone number" una lunghezza massima di 15 caratteri, e al campo "Comment" una lunghezza massima di 200 caratteri.

Provi a inviare il suo modulo — dovrebbe rifiutarsi di inviare fino a quando non vengono rispettati i vincoli sopra indicati e fornire messaggi di errore adeguati. Per aiutarla, potrebbe considerare di aggiungere qualche semplice CSS per mostrare quando un campo del modulo è valido o non valido.

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/html/forms/tasks/form-validation/form-validation1-download.html) per lavorare nel suo editor o in un editor online.

## Validazione del modulo 2

Ora vogliamo che prenda lo stesso modulo che ha visto nel compito precedente (utilizzi la sua risposta precedente se lo desidera) e aggiunga una convalida di modello più specifica ai primi tre campi utilizzando le espressioni regolari.

1. Tutti i nomi utente nella nostra applicazione consistono in una singola lettera, seguita da un punto, seguita da tre o più lettere o numeri. Tutte le lettere devono essere minuscole.
2. Tutti gli indirizzi email dei nostri utenti consistono in una o più lettere (maiuscole o minuscole) o numeri, seguiti da "@bigcorp.eu".
3. Rimuova la validazione della lunghezza dal campo del numero di telefono se presente e configuri in modo che accetti 10 cifre — o 10 cifre di fila, o un modello di tre cifre, tre cifre, poi quattro cifre, separate da spazi, trattini o punti.

> [!NOTE]
> Le espressioni regolari sono davvero impegnative se è nuovo a loro, ma non disperi — provi a vedere dove riesce ad arrivare; non c'è vergogna nel chiedere aiuto. Può trovare tutto ciò di cui ha bisogno per rispondere a queste domande nella nostra [guida alle espressioni regolari](/it/docs/Web/JavaScript/Guide/Regular_expressions) e cercando su [Stack Overflow](https://stackoverflow.com/).

Ancora una volta, per aiutarla potrebbe considerare di aggiungere qualche semplice CSS per mostrare quando un campo del modulo è valido o non valido.

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/html/forms/tasks/form-validation/form-validation2-download.html) per lavorare nel suo editor o in un editor online.

## Validazione del modulo 3

Nel nostro ultimo compito di questo set, le forniamo un esempio simile a quello che ha visto nell'articolo di accompagnamento — un input di inserimento indirizzo email. Desideriamo che usi l'API di validazione dei vincoli, oltre ad alcuni attributi di validazione dei moduli, per programmare messaggi di errore personalizzati.

1. Faccia in modo che l'input sia obbligatorio da compilare e gli dia una lunghezza minima di 10 caratteri.
2. Aggiunga un listener di eventi che verifichi se il valore immesso è un indirizzo email e se è abbastanza lungo. Se non sembra un indirizzo email o è troppo corto, fornisca all'utente messaggi di errore personalizzati appropriati.

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/html/forms/tasks/form-validation/form-validation3-download.html) per lavorare nel suo editor o in un editor online.
