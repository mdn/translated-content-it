---
title: Cos'è l'accessibilità?
slug: Learn_web_development/Howto/Design_and_accessibility/What_is_accessibility
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

Questo articolo introduce i concetti di base dietro l'accessibilità web.

<table class="standard-table">
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Nessuno.</td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Imparare cos'è l'accessibilità e perché è importante.</td>
    </tr>
  </tbody>
</table>

## Sommario

A causa di limitazioni fisiche o tecniche, forse i tuoi visitatori non possono vivere il tuo sito web come speravi. In questo articolo forniamo principi generali di accessibilità e spieghiamo alcune regole.

## Apprendimento attivo

_Non è ancora disponibile l'apprendimento attivo. [Per favore, considera di contribuire](/it/docs/MDN/Community/Getting_started)._

## Approfondimenti

### Accessibilità: principi generali

Potremmo associare l'accessibilità inizialmente a limitazioni negative. Questo edificio deve essere accessibile, quindi deve seguire queste normative per la larghezza delle porte, la dimensione dei bagni e il posizionamento degli ascensori.

Questo è un modo ristretto di pensare all'accessibilità. Considerala come un modo meraviglioso per potenziare le persone e servire più clienti. Cosa possono fare le persone in Brasile con il tuo sito web in inglese? Possono le persone con smartphone navigare in un sito pesante e disordinato progettato per un grande monitor desktop e una larghezza di banda illimitata? Andranno altrove. In generale, _dobbiamo pensare al nostro prodotto dal punto di vista di tutti i nostri clienti target e adattarci di conseguenza._ Da qui l'accessibilità.

### Accessibilità web

Nel contesto specifico del web, l'accessibilità significa che chiunque può trarre beneficio dal tuo contenuto, indipendentemente da disabilità, località, limitazioni tecniche o altre circostanze.

Consideriamo il video:

- Invalidità uditiva

  - : Come può una persona con udito compromesso beneficiare di un video? Devi fornire sottotitoli — o meglio ancora, una trascrizione completa del testo.

    Inoltre, assicurati che le persone possano regolare il volume per soddisfare le loro esigenze uniche.

- Invalidità visiva
  - : Ancora una volta, fornisci una trascrizione testuale che un utente possa consultare senza la necessità di riprodurre il video, e una descrizione audio (una voce fuori campo che descrive ciò che sta accadendo nel video).
- Capacità di pausa
  - : Gli utenti potrebbero avere difficoltà a comprendere qualcuno in un video. Permetti loro di mettere in pausa il video per leggere i sottotitoli o elaborare le informazioni.
- Capacità della tastiera
  - : Consenti all'utente di spostarsi con il tab dentro/fuori da un video, di riprodurlo e di metterlo in pausa senza restare intrappolato.

#### Elementi di base dell'accessibilità Web

Alcuni elementi necessari per l'accessibilità web di base includono:

- Ogni volta che il tuo sito ha bisogno di un'immagine per trasmettere un significato, includi un testo come alternativa per utenti con problemi visivi o connessioni lente.
- Assicurati che tutti gli utenti possano operare interfacce grafiche (come i menu a discesa) solo con una tastiera (ad esempio, con il tasto Tab e il tasto Invio).
- Fornisci un attributo che specifichi esplicitamente la lingua del tuo contenuto, in modo che i lettori di schermo leggano correttamente il tuo testo.
- Assicurati che un utente possa navigare verso tutti i widget su una pagina solo con la tastiera, senza restare intrappolato. (Almeno consenti loro di spostarsi con il tab dentro e fuori.)

E questo è solo l'inizio.

### Campioni di accessibilità

Dal 1999, il {{Glossary("W3C", "W3C")}} opera un gruppo di lavoro chiamato {{Glossary("WAI", "Iniziativa per l'accessibilità web")}} (WAI) che promuove l'accessibilità attraverso linee guida, materiale di supporto e risorse internazionali.

## Maggiori dettagli

Per favore riferisciti a:

- [Articolo di Wikipedia](https://en.wikipedia.org/wiki/Accessibility) sull'accessibilità
- [WAI (Iniziativa per l'accessibilità web del W3C)](https://www.w3.org/WAI/)

## Prossimi passi

L'accessibilità può influire sia sul design di un sito web che sulla struttura tecnica.

- Da un punto di vista del design, suggeriamo di apprendere come [progettare per tutti i tipi di utenti](/it/docs/Learn_web_development/Howto/Design_and_accessibility/Design_for_all_types_of_users).
- Se ti interessa di più il lato tecnico, potresti imparare come [inserire immagini nelle pagine web](/it/docs/Learn_web_development/Core/Structuring_content/HTML_images).
