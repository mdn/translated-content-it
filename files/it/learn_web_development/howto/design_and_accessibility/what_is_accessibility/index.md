---
title: Che cos'è l'accessibilità?
slug: Learn_web_development/Howto/Design_and_accessibility/What_is_accessibility
l10n:
  sourceCommit: f33de00c56ac53878eb2cb7cb5849df1f9ab8db7
---

Questo articolo introduce i concetti di base dell'accessibilità web.

<table class="standard-table">
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Nessuno.</td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Imparare che cos'è l'accessibilità e perché è importante.</td>
    </tr>
  </tbody>
</table>

## Riepilogo

A causa di limitazioni fisiche o tecniche, forse i visitatori non possono fruire del sito web nel modo previsto. In questo articolo vengono illustrati i principi generali dell'accessibilità e spiegate alcune regole.

## Approfondimento

### Accessibilità: principi generali

All'inizio si potrebbe associare l'accessibilità a limitazioni negative. Questo edificio deve essere accessibile, quindi deve rispettare queste norme sulla larghezza delle porte, sulle dimensioni dei servizi igienici e sul posizionamento degli ascensori.

Questo è un modo limitato di concepire l'accessibilità. È invece un modo straordinario per rendere le persone più autonome e servire più clienti. Che cosa possono fare le persone in Brasile con un sito web in inglese? Le persone con smartphone possono navigare un sito web pesante e affollato, progettato per un grande monitor desktop e una larghezza di banda illimitata? Andranno altrove. In generale, _è necessario pensare al prodotto dal punto di vista di tutti i clienti target e adattarlo di conseguenza._ Da qui l'accessibilità.

### Accessibilità web

Nel contesto specifico del web, l'accessibilità significa che chiunque può beneficiare dei contenuti, indipendentemente da disabilità, posizione geografica, limitazioni tecniche o altre circostanze.

Consideriamo i video:

- Disabilità uditiva
  - : Come può una persona con disabilità uditiva beneficiare di un video? È necessario fornire sottotitoli — o, ancora meglio, una trascrizione testuale completa.

    Inoltre, assicurarsi che le persone possano regolare il volume in base alle proprie esigenze.

- Disabilità visiva
  - : Anche in questo caso, fornire una trascrizione testuale che l'utente possa consultare senza dover riprodurre il video e un'audio-descrizione (una voce fuori campo che descrive ciò che accade nel video).
- Possibilità di mettere in pausa
  - : Gli utenti potrebbero avere difficoltà a comprendere qualcuno in un video. Consentire loro di mettere in pausa il video per leggere i sottotitoli o elaborare le informazioni.
- Possibilità di usare la tastiera
  - : Consentire all'utente di entrare e uscire da un video con il tasto Tab, riprodurlo e metterlo in pausa senza rimanervi intrappolato.

#### Le basi dell'accessibilità web

Alcune necessità per l'accessibilità web di base includono:

- Ogni volta che il sito necessita di un'immagine per trasmettere un significato, includere del testo come alternativa per gli utenti con disabilità visive o con connessioni lente.
- Assicurarsi che tutti gli utenti possano utilizzare le interfacce grafiche (come i menu espandibili) esclusivamente con la tastiera (ad esempio, con Tab e il tasto Invio).
- Fornire un attributo che specifichi esplicitamente la lingua dei contenuti, affinché gli screen reader leggano correttamente il testo.
- Assicurarsi che un utente possa navigare verso tutti i widget di una pagina esclusivamente con la tastiera, senza rimanere intrappolato. (Come minimo, consentire di entrare e uscire con Tab.)

E questo è solo l'inizio.

### Promotori dell'accessibilità

Dal 1999, il {{Glossary("W3C", "W3C")}} gestisce un gruppo di lavoro chiamato {{Glossary("WAI", "Web Accessibility Initiative")}} (WAI), che promuove l'accessibilità tramite linee guida, materiale di supporto e risorse internazionali.

## Maggiori dettagli

Consultare:

- [Articolo di Wikipedia](https://en.wikipedia.org/wiki/Accessibility) sull'accessibilità
- [WAI (Web Accessibility Initiative del W3C)](https://www.w3.org/WAI/)

## Passaggi successivi

L'accessibilità può influire sia sul design sia sulla struttura tecnica di un sito web.

- Dal punto di vista del design, si suggerisce di approfondire la [progettazione per tutti i tipi di utenti](/it/docs/Learn_web_development/Howto/Design_and_accessibility/Design_for_all_types_of_users).
- Se interessa maggiormente l'aspetto tecnico, è possibile imparare come [incorporare immagini nelle pagine web](/it/docs/Learn_web_development/Core/Structuring_content/HTML_images).
