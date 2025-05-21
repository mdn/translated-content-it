---
title: Prestazioni percepite
slug: Learn_web_development/Extensions/Performance/Perceived_performance
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/what_is_web_performance", "Learn_web_development/Extensions/Performance/Measuring_performance", "Learn_web_development/Extensions/Performance")}}

**{{Glossary("Perceived_performance", "Prestazioni percepite")}}** è una misura soggettiva delle prestazioni, della reattività e dell'affidabilità del sito web. In altre parole, quanto velocemente un sito web sembra all'utente. È più difficile da quantificare e misurare rispetto alla velocità effettiva di funzionamento, ma forse anche più importante.

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
          >tecnologie web client-side</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Acquisire familiarità di base con la percezione degli utenti delle prestazioni web.</td>
    </tr>
  </tbody>
</table>

## Panoramica

La percezione di quanto velocemente (e senza intoppi) le pagine si caricano e rispondono all'interazione dell'utente è ancora più importante del tempo effettivo richiesto per recuperare le risorse. Anche se potresti non essere in grado di far funzionare fisicamente il tuo sito più velocemente, potresti comunque essere in grado di migliorare quanto velocemente _sembra_ per i tuoi utenti.

Una buona regola generale per migliorare le prestazioni percepite è che è generalmente meglio fornire una risposta rapida e aggiornamenti regolari di stato piuttosto che far aspettare l'utente fino al completamento di un'operazione (prima di fornire qualsiasi informazione). Ad esempio, quando si carica una pagina è meglio visualizzare il testo quando arriva piuttosto che aspettare che tutte le immagini e altre risorse vengano scaricate. Anche se il contenuto non è stato completamente scaricato, l'utente può vedere che qualcosa sta accadendo e può iniziare a interagire con il contenuto.

> [!NOTE]
> Il tempo sembra passare più velocemente per gli utenti che sono attivamente coinvolti, distratti o intrattenuti, rispetto a quelli che stanno aspettando passivamente che accada qualcosa. Quando possibile, coinvolgere attivamente e informare gli utenti che stanno aspettando il completamento di un'attività.

Allo stesso modo, è meglio visualizzare un'"animazione di caricamento" non appena un utente fa clic su un link per eseguire un'operazione di lunga durata. Anche se questo non cambia il tempo necessario per completare l'operazione, il sito sembra più reattivo e l'utente sa che si sta lavorando su qualcosa di utile.

## Metriche di prestazione

Non esiste un'unica metrica o test che possa essere eseguito su un sito per valutare come un utente "si sente". Tuttavia, ci sono una serie di metriche che possono essere "indicatori utili":

- {{Glossary("First_paint", "First Paint")}}
  - : Il tempo per l'inizio della prima operazione di rendering. Si noti che questo cambiamento potrebbe non essere visibile; può essere un semplice aggiornamento del colore di sfondo o qualcosa di ancora meno evidente.
- {{Glossary("First_contentful_paint", "First Contentful Paint")}} (FCP)
  - : Il tempo fino al primo rendering significativo (ad esempio, di testo, immagine di primo piano o sfondo, canvas o SVG, ecc.). Si noti che questo contenuto non è necessariamente utile o significativo.
- {{Glossary("First_meaningful_paint", "First Meaningful Paint")}} (FMP)
  - : Il momento in cui viene renderizzato il contenuto utile sullo schermo.
- [Largest Contentful Paint](https://wicg.github.io/largest-contentful-paint/) (LCP)
  - : Il tempo di rendering del maggiore elemento di contenuto visibile nel viewport.
- {{Glossary("Speed_index", "Speed index")}}
  - : Misura il tempo medio per la pittura dei pixel sullo schermo visibile.
- {{Glossary("Time_to_interactive", "Time to interactive")}}
  - : Tempo fino a quando l'interfaccia utente è disponibile per l'interazione dell'utente (ovvero l'ultimo {{Glossary("Long_task", "compito lungo")}} del processo di caricamento termina).

## Migliorare le prestazioni

Ecco alcuni suggerimenti e trucchi per aiutare a migliorare le prestazioni percepite:

### Minimizzare il carico iniziale

Per migliorare le prestazioni percepite, minimizzare il caricamento iniziale della pagina. In altre parole, scaricare prima il contenuto con cui l'utente interagirà immediatamente e scaricare il resto successivamente "in background". La quantità totale di contenuti scaricata potrebbe effettivamente aumentare, ma l'utente _aspetta_ solo una piccolissima parte, quindi il download sembra più veloce.

Separare la funzionalità interattiva dal contenuto e caricare testo, stili e immagini visibili al caricamento iniziale. Ritardare, o caricare pigramente, immagini o script che non sono usati o visibili al caricamento iniziale della pagina. Inoltre, dovresti ottimizzare le risorse che carichi. Immagini e video dovrebbero essere serviti nel formato più ottimale, compressi e nella dimensione corretta.

### Prevenire contenuti che saltano e altri rientri

Immagini o altre risorse che causano lo spostamento o il salto del contenuto a una posizione diversa, come il caricamento di pubblicità di terze parti, possono far sembrare che la pagina sia ancora in carica ed è negativa per le prestazioni percepite. Il rientro del contenuto è particolarmente negativo per l'esperienza utente quando non è iniziato dall'interazione utente. Se alcune risorse saranno più lente da caricare rispetto ad altre, pianificare in anticipo e lasciare spazio nel layout per evitare che il contenuto salti o ridimensioni, soprattutto dopo che il sito è diventato interattivo.

### Evitare ritardi nei file di font

La scelta del font è importante. Selezionare un font appropriato può migliorare notevolmente l'esperienza utente. Dal punto di vista delle prestazioni percepite, "importazioni di font subottimali" possono causare sfarfallio quando il testo viene stilizzato o quando si ricorre ad altri font.

Assicurati che i font di riserva abbiano la stessa dimensione e peso in modo che quando i font si caricano il cambiamento della pagina sia meno evidente.

### Elementi interattivi sono interattivi

Assicurati che gli elementi interattivi visibili siano sempre interattivi e reattivi. Se gli elementi di input sono visibili, l'utente dovrebbe essere in grado di interagirci senza ritardo. Gli utenti percepiscono qualcosa come lento quando ci vogliono più di 50 ms per reagire. Sentono che una pagina si comporta male quando il contenuto viene ridipinto più lentamente di 16,67 ms (o 60 fotogrammi al secondo) o viene ridipinto a intervalli irregolari.

Rendere cose come il suggerimento di testo un miglioramento progressivo: usa CSS per visualizzare il modale di input, JS per aggiungere l'autocomplete quando è disponibile

### I fattori di inizio attività appaiono più interattivi

Effettuare una richiesta di contenuto al `keydown` piuttosto che aspettare il `keyup` può ridurre il tempo percepito di caricamento del contenuto di 200 ms. Aggiungere un'animazione interessante ma non intrusiva di 200 ms a quell'evento `keyup` può ridurre altri 200 ms di caricamento percepito. Non stai risparmiando 400 ms di tempo, ma l'utente non sente che sta aspettando il contenuto fino a quando, beh, fino a quando non sta aspettando il contenuto.

## Conclusione

Riducendo il tempo che un utente deve aspettare per un contenuto _utile_ e mantenendo il sito reattivo e coinvolgente, gli utenti sentiranno che il sito funziona meglio, anche se il tempo effettivo per caricare le risorse rimane lo stesso.

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/what_is_web_performance", "Learn_web_development/Extensions/Performance/Measuring_performance", "Learn_web_development/Extensions/Performance")}}
