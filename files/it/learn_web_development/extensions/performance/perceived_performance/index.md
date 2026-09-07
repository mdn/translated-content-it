---
title: Prestazioni percepite
slug: Learn_web_development/Extensions/Performance/Perceived_performance
l10n:
  sourceCommit: f4c14731a1a157fc8d8f7357ac4d74d14a7d7fb5
---

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/What_is_web_performance", "Learn_web_development/Extensions/Performance/Measuring_performance", "Learn_web_development/Extensions/Performance")}}

Le **{{Glossary("Perceived_performance", "prestazioni percepite")}}** sono una misura soggettiva delle prestazioni, della reattività e dell'affidabilità di un sito web. In altre parole, quanto velocemente un sito web sembra funzionare per l'utente. Sono più difficili da quantificare e misurare rispetto alla velocità effettiva di funzionamento, ma forse ancora più importanti.

Questo articolo fornisce una breve introduzione ai fattori che influenzano le prestazioni percepite, insieme a diversi strumenti per valutarle e migliorarle.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Installing_software"
          >Software di base installato</a
        > e conoscenza di base delle
        <a href="/it/docs/Learn_web_development/Getting_started/Your_first_website"
          >tecnologie web lato client</a
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

La percezione della velocità (e fluidità) con cui le pagine vengono caricate e rispondono all'interazione dell'utente è ancora più importante del tempo effettivamente necessario per recuperare le risorse. Anche se potrebbe non essere possibile rendere fisicamente il sito più veloce, è probabilmente possibile migliorare la sensazione di velocità per gli utenti.

Una buona regola generale per migliorare le prestazioni percepite è che di solito è meglio fornire una risposta rapida e aggiornamenti regolari sullo stato piuttosto che far attendere l'utente finché un'operazione non viene completata interamente, prima di fornire qualsiasi informazione. Ad esempio, durante il caricamento di una pagina è meglio visualizzare il testo quando arriva anziché attendere tutte le immagini e le altre risorse. Anche se il contenuto non è stato scaricato completamente, l'utente può vedere che qualcosa sta accadendo e può iniziare a interagire con il contenuto.

> [!NOTE]
> Il tempo sembra trascorrere più rapidamente per gli utenti che sono attivamente coinvolti, distratti o intrattenuti rispetto a quelli che attendono passivamente che accada qualcosa. Quando possibile, coinvolgere attivamente e informare gli utenti in attesa del completamento di un'attività.

Analogamente, è meglio visualizzare un'"animazione di caricamento" non appena un utente fa clic su un link per eseguire un'operazione di lunga durata. Sebbene ciò non modifichi il tempo necessario per completare l'operazione, il sito sembra più reattivo e l'utente sa che sta svolgendo qualcosa di utile.

## Metriche delle prestazioni

Non esiste una singola metrica o un singolo test che possa essere eseguito su un sito per valutare come si "sente" un utente. Esistono tuttavia diverse metriche che possono essere "indicatori utili":

- {{Glossary("First_paint", "First Paint")}}
  - : Il tempo fino all'inizio della prima operazione di painting. Si noti che questa modifica potrebbe non essere visibile; può trattarsi di un semplice aggiornamento del colore di sfondo o di qualcosa di ancora meno evidente.
- {{Glossary("First_contentful_paint", "First Contentful Paint")}} (FCP)
  - : Il tempo fino al primo rendering significativo, ad esempio di testo, immagine in primo piano o di sfondo, canvas o SVG. Si noti che questo contenuto non è necessariamente utile o significativo.
- {{Glossary("First_meaningful_paint", "First Meaningful Paint")}} (FMP)
  - : Il momento in cui il contenuto utile viene renderizzato sullo schermo.
- [Largest Contentful Paint](https://wicg.github.io/largest-contentful-paint/) (LCP)
  - : Il tempo di rendering del più grande elemento di contenuto visibile nella viewport.
- {{Glossary("Speed_index", "Speed index")}}
  - : Misura il tempo medio necessario affinché i pixel sullo schermo visibile vengano disegnati.
- {{Glossary("Time_to_interactive", "Time to interactive")}}
  - : Il tempo fino a quando l'interfaccia utente è disponibile per l'interazione dell'utente, ovvero fino al termine dell'ultima {{Glossary("Long_task", "long task")}} del processo di caricamento.

## Migliorare le prestazioni

Ecco alcuni suggerimenti e trucchi per contribuire a migliorare le prestazioni percepite:

### Ridurre al minimo il caricamento iniziale

Per migliorare le prestazioni percepite, ridurre al minimo il caricamento iniziale della pagina. In altre parole, scaricare prima il contenuto con cui l'utente interagirà immediatamente e scaricare il resto successivamente "in background". La quantità totale di contenuto scaricato potrebbe effettivamente aumentare, ma l'utente _attende_ soltanto una quantità molto ridotta di contenuto, quindi il download sembra più veloce.

Separare la funzionalità interattiva dal contenuto e caricare testo, stili e immagini visibili al caricamento iniziale. Ritardare o caricare in modo lazy immagini, iframe, media o script che non vengono utilizzati o non sono visibili nel caricamento iniziale della pagina. Inoltre, è opportuno ottimizzare le risorse effettivamente caricate. Immagini e video dovrebbero essere forniti nel formato più ottimale, compressi e nelle dimensioni corrette.

### Evitare contenuti che saltano e altri reflow

Le immagini o altre risorse che causano lo spostamento verso il basso dei contenuti o il loro salto in una posizione diversa, come il caricamento di pubblicità di terze parti, possono far sembrare che la pagina sia ancora in caricamento e peggiorano le prestazioni percepite. Il reflow del contenuto è particolarmente negativo per l'esperienza utente quando non è avviato dall'interazione dell'utente. Se alcune risorse verranno caricate più lentamente di altre, con elementi caricati dopo che altro contenuto è già stato disegnato sullo schermo, pianificare in anticipo e lasciare spazio nel layout affinché il contenuto non salti o non cambi dimensione, specialmente dopo che il sito è diventato interattivo.

### Evitare ritardi dei file dei font

La scelta del font è importante. Selezionare un font appropriato può migliorare notevolmente l'esperienza utente. Dal punto di vista delle prestazioni percepite, un'importazione non ottimale dei font può causare sfarfallio durante l'applicazione dello stile al testo o nel passaggio a font alternativi.

Impostare i font di fallback con la stessa dimensione e lo stesso peso, in modo che il cambiamento della pagina sia meno evidente quando vengono caricati i font.

### Gli elementi interattivi sono interattivi

Assicurarsi che gli elementi interattivi visibili siano sempre interattivi e reattivi. Se gli elementi di input sono visibili, l'utente dovrebbe potervi interagire senza ritardi. Gli utenti percepiscono un rallentamento quando la reazione richiede più di 50 ms. Percepiscono che una pagina si comporta male quando il contenuto viene ridisegnato più lentamente di 16,67 ms, ovvero 60 frame al secondo, oppure viene ridisegnato a intervalli irregolari.

Rendere funzionalità come la digitazione con suggerimenti un progressive enhancement: usare CSS per visualizzare l'input modale, JS per aggiungere l'autocompletamento quando disponibile.

### Rendere più interattivi gli elementi che avviano attività

Effettuare una richiesta di contenuto su `keydown` anziché attendere `keyup` può ridurre di 200 ms il tempo di caricamento percepito del contenuto. L'aggiunta di un'animazione interessante ma discreta di 200 ms a quell'evento `keyup` può ridurre di altri 200 ms il caricamento percepito. Non vengono risparmiati 400 ms di tempo, ma l'utente non ha la sensazione di attendere il contenuto finché, beh, non sta effettivamente attendendo il contenuto.

## Conclusione

Riducendo il tempo che un utente deve attendere per contenuti _utili_ e mantenendo il sito reattivo e coinvolgente, gli utenti percepiranno che il sito offre prestazioni migliori, anche se il tempo effettivo necessario per caricare le risorse rimane invariato.

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/What_is_web_performance", "Learn_web_development/Extensions/Performance/Measuring_performance", "Learn_web_development/Extensions/Performance")}}
