---
title: Prestazioni percepite
slug: Learn_web_development/Extensions/Performance/Perceived_performance
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/what_is_web_performance", "Learn_web_development/Extensions/Performance/Measuring_performance", "Learn_web_development/Extensions/Performance")}}

**{{Glossary("Perceived_performance", "Prestazioni percepite")}}** è una misura soggettiva delle prestazioni, della reattività e dell'affidabilità di un sito web. In altre parole, quanto velocemente un sito web sembra agli utenti. È più difficile da quantificare e misurare rispetto alla velocità effettiva di funzionamento, ma forse ancor più importante.

Questo articolo fornisce una breve introduzione ai fattori che influenzano le prestazioni percepite, insieme a una serie di strumenti per valutare e migliorare la percezione.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Installing_software"
          >Software di base installato</a
        >, e conoscenza di base delle
        <a href="/it/docs/Learn_web_development/Getting_started/Your_first_website"
          >tecnologie web lato client</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Acquisire familiarità di base con la percezione delle prestazioni web da parte degli utenti.</td>
    </tr>
  </tbody>
</table>

## Panoramica

La percezione di quanto rapidamente (e senza intoppi) le pagine si caricano e rispondono all'interazione dell'utente è ancora più importante del tempo effettivo richiesto per recuperare le risorse. Anche se potrebbe non essere possibile far funzionare il sito fisicamente più veloce, è possibile migliorare quanto _velocemente_ appare per i suoi utenti.

Una buona regola generale per migliorare le prestazioni percepite è che è di solito meglio fornire una risposta rapida e aggiornamenti regolari di stato piuttosto che far aspettare gli utenti fino a quando un'operazione non sia completata interamente (prima di fornire qualsiasi informazione). Ad esempio, quando si carica una pagina, è meglio visualizzare il testo appena arriva piuttosto che attendere tutte le immagini e altre risorse. Anche se il contenuto non è stato completamente scaricato, l'utente può vedere che qualcosa sta accadendo e può iniziare a interagire con il contenuto.

> [!NOTE]
> Il tempo sembra passare più rapidamente per gli utenti che sono attivamente impegnati, distratti o intrattenuti, rispetto a quelli che stanno aspettando passivamente che accada qualcosa. Dove possibile, coinvolga e informi attivamente gli utenti che stanno aspettando il completamento di un'attività.

In modo simile, è meglio mostrare un'animazione di "caricamento" non appena un utente clicca su un link per eseguire un'operazione di lunga durata. Anche se questo non cambia il tempo necessario per completare l'operazione, il sito sembra più reattivo e l'utente sa che sta lavorando su qualcosa di utile.

## Metriche di performance

Non esiste una singola metrica o test che possa essere eseguito su un sito per valutare come un utente "si sente". Tuttavia, ci sono diverse metriche che possono essere "indicatori utili":

- {{Glossary("First_paint", "First Paint")}}
  - : Il tempo per l'inizio della prima operazione di paint. Si noti che questo cambiamento potrebbe non essere visibile; può essere un semplice aggiornamento del colore di sfondo o qualcosa di meno evidente.
- {{Glossary("First_contentful_paint", "First Contentful Paint")}} (FCP)
  - : Il tempo fino al primo rendering significativo (ad esempio, di testo, immagine di primo piano o sfondo, canvas o SVG, ecc.). Si noti che questo contenuto non è necessariamente utile o significativo.
- {{Glossary("First_meaningful_paint", "First Meaningful Paint")}} (FMP)
  - : Il momento in cui il contenuto utile viene renderizzato sullo schermo.
- [Largest Contentful Paint](https://wicg.github.io/largest-contentful-paint/) (LCP)
  - : Il tempo di rendering dell'elemento di contenuto più grande visibile nella viewport.
- {{Glossary("Speed_index", "Speed index")}}
  - : Misura il tempo medio per il painting dei pixel sullo schermo visibile.
- {{Glossary("Time_to_interactive", "Time to interactive")}}
  - : Tempo fino a quando l'interfaccia utente è disponibile per l'interazione dell'utente (cioè, l'ultimo {{Glossary("Long_task", "compito lungo")}} del processo di caricamento finisce).

## Migliorare le prestazioni

Ecco alcuni consigli e trucchi per migliorare le prestazioni percepite:

### Minimizzare il caricamento iniziale

Per migliorare le prestazioni percepite, minimizzare il caricamento originale della pagina. In altre parole, scaricare prima il contenuto con cui l'utente interagirà immediatamente e scaricare il resto dopo "in background". La quantità totale di contenuto scaricato potrebbe effettivamente aumentare, ma l'utente attende solo una piccola parte, quindi il download sembra più rapido.

Separare la funzionalità interattiva dal contenuto e caricare testo, stili e immagini visibili al caricamento iniziale. Ritardare, o caricare in modo differito, immagini o script che non sono utilizzati o visibili al caricamento iniziale della pagina. Inoltre, ottimizzare le risorse che si caricano. Le immagini e i video dovrebbero essere serviti nel formato più ottimale, compressi e della dimensione corretta.

### Prevenire il salto e altri rimescolamenti del contenuto

Immagini o altre risorse che causano lo spostamento verso il basso del contenuto o il salto in un'altra posizione, come il caricamento di annunci di terzi, possono far sembrare che la pagina sia ancora in fase di caricamento e sono dannose per le prestazioni percepite. Il rimescolamento del contenuto è particolarmente negativo per l'esperienza dell'utente quando non è avviato dall'interazione dell'utente. Se alcune risorse impiegano più tempo a caricarsi rispetto ad altre, con elementi che si caricano dopo che altro contenuto è già stato dipinto sullo schermo, pianificare in anticipo e lasciare spazio nel layout per esse in modo che il contenuto non salti o cambi dimensione, specialmente dopo che il sito è diventato interattivo.

### Evitare ritardi nei file dei font

La scelta del font è importante. Selezionare un font appropriato può migliorare notevolmente l'esperienza dell'utente. Dal punto di vista delle prestazioni percepite, "importazione di font subottimali" può provocare sfarfallii quando il testo è stilizzato o quando si torna ad altri font.

Rendere i font di fallback delle stesse dimensioni e peso in modo che quando i font si caricano il cambiamento della pagina sia meno evidente.

### Elementi interattivi sempre interattivi

Assicurarsi che gli elementi interattivi visibili siano sempre interattivi e reattivi. Se gli elementi di input sono visibili, l'utente dovrebbe poter interagire con essi senza alcun ritardo. Gli utenti sentono che qualcosa è lento quando ci mette più di 50ms a reagire. Sentono che una pagina si comporta male quando il contenuto si ridisegna più lentamente di 16,67ms (o 60 fotogrammi al secondo) o si ridisegna a intervalli diseguali.

Rendere funzionalità come il completamento durante la digitazione un miglioramento progressivo: utilizzare CSS per visualizzare la modalità di input, JS per aggiungere il completamento automatico non appena è disponibile.

### Rendere gli attivatori di compiti più interattivi

Eseguire una richiesta di contenuto al `keydown` piuttosto che aspettare `keyup` può ridurre il tempo di caricamento percepito del contenuto di 200ms. Aggiungere un'interessante ma non invasiva animazione di 200ms a quell'evento `keyup` può ridurre di altri 200ms il tempo di caricamento percepito. Non si risparmiano 400ms di tempo, ma l'utente non sente di aspettare il contenuto fino a quando, beh, fino a quando aspetta il contenuto.

## Conclusione

Riducendo il tempo che un utente deve aspettare per contenuti _utili_, e mantenendo il sito reattivo e coinvolgente, gli utenti sentiranno che il sito ha prestazioni migliori — anche se il tempo effettivo per caricare le risorse rimane lo stesso.

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/what_is_web_performance", "Learn_web_development/Extensions/Performance/Measuring_performance", "Learn_web_development/Extensions/Performance")}}
