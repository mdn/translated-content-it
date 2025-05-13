---
title: Che cos'è l'accessibilità?
slug: Learn_web_development/Howto/Design_and_accessibility/What_is_accessibility
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

Questo articolo introduce i concetti di base che stanno alla base dell'accessibilità sul web.

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

A causa di limitazioni fisiche o tecniche, forse i suoi visitatori non possono vivere il suo sito web nel modo in cui sperava. In questo articolo forniamo i principi generali dell'accessibilità e spieghiamo alcune regole.

## Apprendimento attivo

_Non è ancora disponibile l'apprendimento attivo. [La preghiamo di considerare la possibilità di contribuire](/it/docs/MDN/Community/Getting_started)._

## Approfondimenti

### Accessibilità: principi generali

Potremmo associare inizialmente l'accessibilità a limitazioni negative. Questo edificio deve essere accessibile, quindi deve seguire queste normative per la larghezza delle porte, la dimensione dei bagni e il posizionamento degli ascensori.

Questo è un modo ristretto di pensare all'accessibilità. La pensi invece come un modo meraviglioso per dare potere alle persone e servire più clienti. Cosa possono fare le persone in Brasile con il suo sito web in inglese? Le persone con smartphone possono navigare su un sito web pesante e disordinato progettato per un grande monitor desktop e larghezza di banda illimitata? Andranno altrove. In generale, _dobbiamo pensare al nostro prodotto dal punto di vista di tutti i nostri clienti target e adattarci di conseguenza_. Da qui, l'importanza dell'accessibilità.

### Accessibilità sul web

Nel contesto specifico del web, l'accessibilità significa che chiunque può beneficiare del suo contenuto, indipendentemente da disabilità, localizzazione, limitazioni tecniche o altre circostanze.

Consideriamo il video:

- Disabilità uditive

  - : Come trae beneficio un non udente da un video? Deve fornire sottotitoli — o ancora meglio, una trascrizione completa del testo.

    Inoltre, si assicuri che le persone possano regolare il volume per soddisfare le loro esigenze specifiche.

- Disabilità visive
  - : Anche in questo caso, fornisca una trascrizione del testo che un utente può consultare senza bisogno di riprodurre il video, e una descrizione audio (una voce fuori campo che descrive ciò che sta avvenendo nel video).
- Capacità di pausa
  - : Gli utenti possono avere difficoltà a capire qualcuno in un video. Consentire loro di mettere in pausa il video per leggere i sottotitoli o elaborare le informazioni.
- Capacità della tastiera
  - : Consenta all'utente di navigare dentro/fuori un video, riprodurlo e metterlo in pausa senza rimanere intrappolato.

#### Le basi dell'accessibilità Web

Alcune necessità per una base di accessibilità Web includono:

- Ogni volta che il suo sito necessita di un'immagine per trasmettere un significato, includa del testo come alternativa per gli utenti ipovedenti o con connessioni lente.
- Si assicuri che tutti gli utenti possano operare interfacce grafiche (come i menu a discesa) esclusivamente con una tastiera (ad esempio, con il tasto Tab e il tasto Invio).
- Fornisca un attributo che specifichi esplicitamente la lingua del suo contenuto, in modo che i lettori di schermo leggano correttamente il suo testo.
- Si assicuri che un utente possa navigare a tutti i widget su una pagina esclusivamente con la tastiera, senza rimanere intrappolato. (Almeno, consenta loro di utilizzare il tasto Tab per entrare e uscire.)

E questo è solo l'inizio.

### Campioni dell'accessibilità

Dal 1999, il {{Glossary("W3C", "W3C")}} ha operato un gruppo di lavoro chiamato {{Glossary("WAI", "Iniziativa per l'accessibilità sul web")}} (WAI), promuovendo l'accessibilità attraverso linee guida, materiale di supporto e risorse internazionali.

## Ulteriori dettagli

Si riferisca a:

- [Articolo di Wikipedia](https://en.wikipedia.org/wiki/Accessibility) sull'accessibilità
- [WAI (Iniziativa per l'accessibilità del Web del W3C)](https://www.w3.org/WAI/)

## Prossimi passi

L'accessibilità può influire sia sulla progettazione che sulla struttura tecnica di un sito web.

- Da un punto di vista della progettazione, le suggeriamo di imparare a [progettare per tutti i tipi di utenti](/it/docs/Learn_web_development/Howto/Design_and_accessibility/Design_for_all_types_of_users).
- Se la interessa di più la parte tecnica, potrebbe imparare come [inserire immagini nelle pagine web](/it/docs/Learn_web_development/Core/Structuring_content/HTML_images).
