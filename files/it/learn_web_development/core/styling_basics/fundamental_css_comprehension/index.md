---
title: "Sfida: Comprensione Fondamentale di CSS"
short-title: "Sfida: Biglietto da visita"
slug: Learn_web_development/Core/Styling_basics/Fundamental_CSS_comprehension
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Debugging_CSS", "Learn_web_development/Core/Styling_basics/Fancy_letterheaded_paper", "Learn_web_development/Core/Styling_basics")}}

Questa sfida fornisce una serie di esercizi correlati che devono essere completati per creare il design finale — un biglietto da visita/carta del giocatore/profilo social.

## Punto di partenza

Per iniziare questa sfida, dovresti:

- Scaricare il [file HTML per l'esercizio](https://github.com/mdn/learning-area/blob/main/css/introduction-to-css/fundamental-css-comprehension/index.html) e il [file immagine associato](https://github.com/mdn/learning-area/blob/main/css/introduction-to-css/fundamental-css-comprehension/chris.jpg) e salvarli in una nuova directory nel tuo computer locale. Se desideri utilizzare una tua immagine e inserire il tuo nome, puoi farlo — assicurati solo che l'immagine sia quadrata.
- Scaricare il [file di risorse CSS](https://github.com/mdn/learning-area/blob/main/css/introduction-to-css/fundamental-css-comprehension/style-resources.txt) — contiene un insieme di selettori grezzi e ruleset che dovrai studiare e combinare per completare parte di questa sfida.

In alternativa, potresti utilizzare un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/). Potresti incollare l'HTML e inserire il CSS in uno di questi editor online e usare [questo URL](https://mdn.github.io/learning-area/css/introduction-to-css/fundamental-css-comprehension/chris.jpg) per puntare l'elemento `<img>` al file immagine.

> [!NOTE]
> Se ti blocchi, puoi contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Descrizione del progetto

Ti sono stati forniti alcuni HTML grezzi e un'immagine, e devi scrivere il CSS necessario per stilizzare questo in un accattivante biglietto da visita online, che può forse essere anche una carta del giocatore o un profilo social. Le sezioni seguenti descrivono ciò che devi fare.

Impostazione di base:

- Prima di tutto, crea un nuovo file nella stessa directory dei tuoi file HTML e immagine. Chiamalo qualcosa di veramente fantasioso come `style.css`.
- Collega il tuo CSS al tuo file HTML tramite un elemento `<link>`.
- I primi due ruleset nel file di risorse CSS sono tuoi, GRATIS! Dopo aver finito di gioire per la tua buona fortuna, copiali e incollali nella parte superiore del tuo nuovo file CSS. Usali come test per assicurarti che il tuo CSS sia applicato correttamente al tuo HTML.
- Sopra i due ruleset, aggiungi un commento CSS con del testo all'interno per indicare che si tratta di un insieme di stili generali per l'intera pagina. "Stili generali della pagina" potrebbe andare bene. Aggiungi anche altri tre commenti alla fine del file CSS per indicare stili specifici per l'impostazione del contenitore del biglietto, stili specifici per l'intestazione e il piè di pagina, e stili specifici per i contenuti principali del biglietto da visita. Da ora in poi, gli stili aggiunti al foglio di stile dovrebbero essere organizzati in un posto appropriato.

Gestione dei selettori e ruleset forniti nel file risorse CSS:

- Successivamente, ti chiediamo di guardare i quattro selettori e calcolare la specificità per ciascuno di essi. Scrivi questi valori da qualche parte dove possano essere trovati in seguito, ad esempio in un commento in cima al tuo CSS.
- Ora è il momento di mettere il selettore giusto sul giusto insieme di regole! Hai quattro coppie di selettore e ruleset da abbinare nelle tue risorse CSS. Fai questo ora, e aggiungili al tuo file CSS. Devi:

  - Dare al contenitore principale del biglietto una larghezza/altezza fissa, un colore di sfondo solido, un bordo e un raggio di bordatura (angoli arrotondati!), tra le altre cose.
  - Dare all'intestazione un gradiente di sfondo che va da più scuro a più chiaro, più angoli arrotondati che si adattano agli angoli arrotondati impostati sul contenitore principale del biglietto.
  - Dare il piè di pagina un gradiente di sfondo che va da più chiaro a più scuro, più angoli arrotondati che si adattano agli angoli arrotondati impostati sul contenitore principale del biglietto.
  - [Float](/it/docs/Learn_web_development/Core/CSS_layout/Floats) l'immagine a destra in modo che aderisca al lato destro dei contenuti principali del biglietto da visita, e darle un'altezza massima del 100% (un trucco intelligente che assicura che cresca/rimpicciolisca per rimanere alla stessa altezza del suo contenitore principale, indipendentemente da quale altezza diventi.)

- Attenzione! Ci sono due errori nei ruleset forniti. Usando qualsiasi tecnica che conosci, individua questi errori e correggili prima di procedere.

Nuovi ruleset che devi scrivere:

- Scrivi un ruleset che indirizza sia l'intestazione del biglietto, sia il piè di pagina, dando ad entrambi un'altezza totale calcolata di 50px (inclusa un'altezza del contenuto di 30px e padding di 10px su tutti i lati). Ma esprimilo in `em`.
- Il margine predefinito applicato agli elementi `<h2>` e `<p>` dal browser interferirà con il nostro design, quindi scrivi una regola che indirizza tutti questi elementi e imposta il loro margine a 0.
- Per evitare che l'immagine fuoriesca dai contenuti principali del biglietto da visita (l'elemento `<article>`), dobbiamo darle un'altezza specifica. Imposta l'altezza dell'`<article>` a 120px, ma espressa in `em`. Dai anche un colore di sfondo di nero semitrasparente, risultando in una tonalità leggermente più scura che consente al colore rosso di sfondo di emergere un po' troppo.
- Scrivi un ruleset che dà all'`<h2>` una dimensione del font effettiva di 20px (ma espressa in `em`) e un'altezza della linea appropriata per posizionarla al centro della casella del contenuto dell'intestazione. Ricordati da prima che l'altezza della casella del contenuto dovrebbe essere di 30px — questo ti dà tutti i numeri necessari per calcolare l'altezza della linea.
- Scrivi un ruleset che dà al `<p>` all'interno del piè di pagina una dimensione del font effettiva di 15px (ma espressa in `em`) e un'altezza della linea appropriata per posizionarla al centro della casella del contenuto del piè di pagina. Ricordati da prima che l'altezza della casella del contenuto dovrebbe essere di 30px — questo ti dà tutti i numeri necessari per calcolare l'altezza della linea.
- Come ultimo tocco, dai al paragrafo all'interno dell'`<article>` un valore di padding appropriato in modo che il suo bordo sinistro sia allineato con l'`<h2>` e il paragrafo del piè di pagina, e imposta il suo colore per essere abbastanza chiaro in modo che sia facile da leggere.

> [!NOTE]
> Tieni presente che il secondo ruleset imposta `font-size: 10px;` sull'elemento `<html>` — questo significa che per qualsiasi discendente di `<html>`, un em sarà uguale a 10px anziché 16px come avviene di default. (Questo è, naturalmente, a patto che i discendenti in questione non abbiano nessun antenato che si trova tra loro e `<html>` nella gerarchia con un diverso `font-size` impostato. Ciò potrebbe influire sui valori di cui hai bisogno, anche se in questo semplice esempio ciò non è un problema.)

Altre cose da considerare:

- Otterrai punti bonus se scrivi il tuo CSS per la massima leggibilità, con una dichiarazione separata per ogni riga.
- Dovresti includere `.card` all'inizio della catena dei selettori in tutte le tue regole, in modo che queste regole non interferiscano con lo stile di qualsiasi altro elemento se il biglietto da visita dovesse essere inserito in una pagina con un carico di altri contenuti.

## Suggerimenti e consigli

- Non è necessario modificare l'HTML in nessun modo, eccetto per applicare il CSS al tuo HTML.
- Quando provi a calcolare il valore `em` di cui hai bisogno per rappresentare una certa lunghezza in pixel, pensa a quale dimensione base del font ha l'elemento root (`<html>`) e a cosa deve essere moltiplicata per ottenere il valore desiderato. Questo ti darà il tuo valore em, almeno in un caso semplice come questo.

## Esempio

Il seguente screenshot mostra un esempio di come dovrebbe apparire il design finito:

![Una vista del biglietto da visita finito, mostra un'intestazione e un piè di pagina rossi, e un pannello centrale più scuro contenente i dettagli principali e l'immagine.](business-card.png)

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Debugging_CSS", "Learn_web_development/Core/Styling_basics/Fancy_letterheaded_paper", "Learn_web_development/Core/Styling_basics")}}
