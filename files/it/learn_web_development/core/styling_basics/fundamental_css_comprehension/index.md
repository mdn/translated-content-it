---
title: "Sfida: Comprensione fondamentale di CSS"
short-title: "Sfida: Biglietto da visita"
slug: Learn_web_development/Core/Styling_basics/Fundamental_CSS_comprehension
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Debugging_CSS", "Learn_web_development/Core/Styling_basics/Fancy_letterheaded_paper", "Learn_web_development/Core/Styling_basics")}}

Questa sfida fornisce una serie di esercizi correlati che devono essere completati per creare il design finale — un biglietto da visita/carta giocatore/profilo social.

## Punto di partenza

Per iniziare questa sfida, dovrebbe:

- Scaricare il [file HTML per l'esercizio](https://github.com/mdn/learning-area/blob/main/css/introduction-to-css/fundamental-css-comprehension/index.html) e il [file immagine associato](https://github.com/mdn/learning-area/blob/main/css/introduction-to-css/fundamental-css-comprehension/chris.jpg), e salvarli in una nuova directory sul suo computer locale. Se desidera utilizzare un proprio file immagine e inserire il proprio nome, è il benvenuto — basta assicurarsi che l'immagine sia quadrata.
- Scaricare il [file di testo delle risorse CSS](https://github.com/mdn/learning-area/blob/main/css/introduction-to-css/fundamental-css-comprehension/style-resources.txt) — questo contiene una serie di selettori grezzi e settori di regole che dovrà studiare e combinare per rispondere a questa parte della sfida.

In alternativa, potrebbe usare un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
Potrebbe incollare l'HTML e compilare il CSS in uno di questi editor online, e usare [questo URL](https://mdn.github.io/learning-area/css/introduction-to-css/fundamental-css-comprehension/chris.jpg) per puntare l'elemento `<img>` al file immagine.

> [!NOTE]
> Se rimane bloccato, può contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Descrizione del progetto

Le è stato fornito del codice HTML grezzo e un'immagine, e deve scrivere il CSS necessario per trasformare tutto in un fantastico biglietto da visita online, che magari può fungere anche da carta giocatore o profilo social. Le sezioni seguenti descrivono cosa deve fare.

Impostazione di base:

- Prima di tutto, crei un nuovo file nella stessa directory dei file HTML e immagine. Lo chiami con qualcosa di veramente creativo come `style.css`.
- Colleghi il suo CSS al file HTML tramite un elemento `<link>`.
- I primi due settori di regole nel file di risorse CSS sono suoi, GRATIS! Dopo aver esultato per la sua buona sorte, le copi e incolli all'inizio del suo nuovo file CSS. Le usi per assicurarsi che il CSS sia applicato correttamente al suo HTML.
- Sopra le due regole, inserisca un commento CSS con del testo per indicare che si tratta di un set di stili generali per l'intera pagina. "Stili generali per la pagina" andrà bene. Aggiunga anche tre altri commenti alla fine del file CSS per indicare stili specifici per la configurazione del contenitore della carta, stili specifici per l'intestazione e il piè di pagina, e stili specifici per i contenuti principali del biglietto da visita. Da ora in avanti, gli stili successivi aggiunti al foglio di stile dovrebbero essere organizzati in un posto appropriato.

Gestione dei selettori e dei settori di regole forniti nel file di risorse CSS:

- Successivamente, vogliamo che esamini i quattro selettori e calcoli la specificità per ciascuno. La scriva da qualche parte dove possa essere trovata in seguito, ad esempio in un commento all'inizio del suo CSS.
- Ora è il momento di mettere il selettore giusto sul giusto settore di regole! Ha quattro coppie di selettore e settore di regole da abbinare nelle risorse CSS. Le faccia ora, e le aggiunga al suo file CSS. Deve:

  - Dare al contenitore principale della carta una larghezza/altezza fissa, colore di sfondo solido, bordo e border-radius (angoli arrotondati!), tra le altre cose.
  - Dare all'intestazione un gradiente di sfondo che va da più scuro a più chiaro, più angoli arrotondati che si adattano agli angoli arrotondati impostati sul contenitore principale della carta.
  - Dare al piè di pagina un gradiente di sfondo che va da più chiaro a più scuro, più angoli arrotondati che si adattano agli angoli arrotondati impostati sul contenitore principale della carta.
  - [Float](/it/docs/Learn_web_development/Core/CSS_layout/Floats) l'immagine a destra in modo che si attacchi al lato destro dei contenuti principali del biglietto da visita, e gli dia un'altezza massima del 100% (un trucco intelligente che assicura che crescerà/diminuirà per mantenersi alla stessa altezza del suo contenitore principale, indipendentemente dall'altezza che diventa.)

- Attenzione! Ci sono due errori nei settori di regole forniti. Utilizzando qualsiasi tecnica conosca, li rintracci e li corregga prima di procedere.

Nuovi settori di regole che deve scrivere:

- Scriva un settore di regole che indirizza sia l'intestazione della carta che il piè di pagina, dando loro entrambi un'altezza totale calcolata di 50px (compresi un'altezza del contenuto di 30px e un padding di 10px su tutti i lati). Ma lo esprima in `em`.
- Il margine predefinito applicato agli elementi `<h2>` e `<p>` dal browser interferirà con il nostro design, quindi scriva una regola che indirizza tutti questi elementi e imposti il loro margine a 0.
- Per impedire che l'immagine fuoriesca dal contenuto principale del biglietto da visita (l'elemento `<article>`), dobbiamo dargli un'altezza specifica. Imposti l'altezza dell'`<article>` a 120px, ma espressa in `em`. Gli dia anche un colore di sfondo nero semi-trasparente, risultando in una tonalità leggermente più scura che permette al colore rosso di sfondo di brillare un po' troppo.
- Scriva un settore di regole che dia all'`<h2>` una dimensione del font effettiva di 20px (ma espressa in `em`) e un'altezza di linea appropriata per posizionarla al centro della casella del contenuto dell'intestazione. Ricordi da prima che l'altezza della casella del contenuto dovrebbe essere 30px — questo le dà tutti i numeri necessari per calcolare l'altezza della linea.
- Scriva un settore di regole che dia al `<p>` all'interno del piè di pagina una dimensione del font effettiva di 15px (ma espressa in `em`) e un'altezza di linea appropriata per posizionarlo al centro della casella del contenuto del piè di pagina. Ricordi da prima che l'altezza della casella del contenuto dovrebbe essere 30px — questo le dà tutti i numeri necessari per calcolare l'altezza della linea.
- Come ultimo tocco, dia al paragrafo all'interno dell'`<article>` un valore di padding appropriato in modo che il suo bordo sinistro si allinei con l'`<h2>` e il paragrafo del piè di pagina, e imposti il suo colore per essere abbastanza chiaro così da essere facile da leggere.

> [!NOTE]
> Tenga presente che il secondo settore di regole imposta `font-size: 10px;` sull'elemento `<html>` — questo significa che per qualsiasi discendente di `<html>`, un em sarà uguale a 10px piuttosto che 16px come è per impostazione predefinita. (Questo è ovviamente, a condizione che i discendenti in questione non abbiano alcun antenato tra loro e `<html>` nella gerarchia che abbia un diverso `font-size` impostato su di essi. Questo potrebbe influenzare i valori di cui ha bisogno, anche se in questo semplice esempio questo non è un problema.)

Altre cose da considerare:

- Otterrà punti bonus se scrive il suo CSS per la massima leggibilità, con una dichiarazione separata su ciascuna riga.
- Dovrebbe includere `.card` all'inizio della catena dei selettori in tutte le sue regole, in modo che queste regole non interferiscano con lo stile di altri elementi se il biglietto da visita fosse messo su una pagina con un carico di altri contenuti.

## Suggerimenti e consigli

- Non è necessario modificare l'HTML in alcun modo, a eccezione dell'applicazione del CSS al suo HTML.
- Quando cerca di determinare il valore `em` necessario per rappresentare una certa lunghezza in pixel, pensi a quale dimensione del font di base ha l'elemento root (`<html>`) e a cosa deve essere moltiplicata per ottenere il valore desiderato. Questo le darà il suo valore em, almeno in un caso semplice come questo.

## Esempio

Lo screenshot seguente mostra un esempio di come dovrebbe apparire il design finale:

![Una vista del biglietto da visita finito, mostra un'intestazione e un piè di pagina rossi, e un pannello centrale più scuro che contiene i dettagli principali e l'immagine.](business-card.png)

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Debugging_CSS", "Learn_web_development/Core/Styling_basics/Fancy_letterheaded_paper", "Learn_web_development/Core/Styling_basics")}}
