---
title: Prestazioni web
slug: Learn_web_development/Extensions/Performance
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Extensions/Performance/why_web_performance", "Learn_web_development/Extensions")}}

La costruzione di siti web richiede HTML, CSS e JavaScript. Per creare siti web e applicazioni che le persone vogliono usare, che attraggono e mantengono gli utenti, è necessario creare una buona esperienza utente. Parte di una buona esperienza utente è garantire che i contenuti vengano caricati rapidamente e siano reattivi alle interazioni dell'utente. Questo concetto è noto come **prestazioni web**, e in questo modulo ci concentreremo sui fondamenti per creare siti web performanti.

Il resto del nostro materiale di apprendimento per principianti cerca di attenersi il più possibile alle migliori pratiche web come le prestazioni e l'[accessibilità](/it/docs/Learn_web_development/Core/Accessibility), tuttavia, è bene concentrarsi specificamente su questi argomenti e assicurarsi di esserne familiari.

## Prerequisiti

Mentre la conoscenza di HTML, CSS e JavaScript è necessaria per implementare molte raccomandazioni di miglioramento delle prestazioni web, sapere come costruire applicazioni non è un prerequisito necessario per comprendere e misurare le prestazioni web. Tuttavia, raccomandiamo che prima di affrontare questo modulo, si abbia almeno un'idea di base dello sviluppo web lavorando attraverso il modulo [Il tuo primo sito web](/it/docs/Learn_web_development/Getting_started/Your_first_website).

Sarebbe anche utile approfondire questi argomenti con moduli come:

- [Strutturare i contenuti con HTML](/it/docs/Learn_web_development/Core/Structuring_content)
- [Nozioni di base sullo stile CSS](/it/docs/Learn_web_development/Core/Styling_basics)
- [Scripting dinamico con JavaScript](/it/docs/Learn_web_development/Core/Scripting)

## Guida

- [Il "perché" delle prestazioni web](/it/docs/Learn_web_development/Extensions/Performance/why_web_performance)
  - : Questo articolo descrive perché le prestazioni web sono importanti per l'accessibilità, l'esperienza utente e gli obiettivi aziendali.
- [Cosa sono le prestazioni web?](/it/docs/Learn_web_development/Extensions/Performance/What_is_web_performance)
  - : Si sa che le prestazioni web sono importanti, ma cosa costituisce le prestazioni web? Questo articolo introduce i componenti delle prestazioni, dal caricamento e rendering della pagina web, incluso come i tuoi contenuti arrivano nel browser dei tuoi utenti per essere visualizzati, a quali gruppi di persone dobbiamo considerare quando pensiamo alle prestazioni.
- [Come percepiscono le prestazioni gli utenti?](/it/docs/Learn_web_development/Extensions/Performance/Perceived_performance)
  - : Più importante della velocità del tuo sito in millisecondi, è la velocità con cui gli utenti percepiscono il tuo sito. Queste percezioni sono influenzate dal tempo effettivo di caricamento della pagina, dall'attesa, dalla reattività all'interazione dell'utente e dalla fluidità dello scorrimento e di altre animazioni. In questo articolo, discutiamo delle varie metriche di caricamento, animazione e reattività, insieme alle migliori pratiche per migliorare la percezione dell'utente, se non i tempi effettivi.
- [Misurare le prestazioni](/it/docs/Learn_web_development/Extensions/Performance/Measuring_performance)
  - : Ora che comprendi alcune metriche delle prestazioni, approfondiamo gli strumenti, le metriche e le API per le prestazioni e come possiamo integrare le prestazioni nel flusso di lavoro dello sviluppo web.
- [Multimedia: immagini](/it/docs/Learn_web_development/Extensions/Performance/Multimedia)
  - : Il frutto più facilmente raggiungibile delle prestazioni web è spesso l'ottimizzazione dei media. È possibile servire diversi file multimediali in base alla capacità, dimensione e densità dei pixel di ogni user agent. In questo articolo discutiamo l'impatto che le immagini hanno sulle prestazioni e i metodi per ridurre il numero di byte inviati per immagine.
- [Multimedia: video](/it/docs/Learn_web_development/Extensions/Performance/video)
  - : Il frutto più facilmente raggiungibile delle prestazioni web è spesso l'ottimizzazione dei media. In questo articolo discutiamo l'impatto dei contenuti video sulle prestazioni e copriamo consigli come rimuovere le tracce audio dai video di sfondo per migliorare le prestazioni.
- [Ottimizzazione delle prestazioni di JavaScript](/it/docs/Learn_web_development/Extensions/Performance/JavaScript)
  - : JavaScript, se usato correttamente, può consentire esperienze web interattive e immersive — o può danneggiare significativamente il tempo di download, il tempo di rendering, le prestazioni in-app, la durata della batteria e l'esperienza utente. Questo articolo elenca alcune delle best practice di JavaScript che dovrebbero essere considerate per garantire che anche i contenuti complessi siano il più possibile performanti.
- [Ottimizzazione delle prestazioni di HTML](/it/docs/Learn_web_development/Extensions/Performance/HTML)
  - : Alcuni attributi e l'ordine delle sorgenti del tuo markup possono influire sulle prestazioni del tuo sito web. Minimizzando il numero di nodi DOM, assicurandosi che l'ordine e gli attributi migliori vengano utilizzati per includere contenuti come stili, script, media e script di terze parti, è possibile migliorare notevolmente l'esperienza utente. Questo articolo esamina in dettaglio come l'HTML possa essere utilizzato per garantire la massima performance.
- [Ottimizzazione delle prestazioni CSS](/it/docs/Learn_web_development/Extensions/Performance/CSS)
  - : Il CSS può essere un focus di ottimizzazione meno importante per migliorare le prestazioni, ma ci sono alcune caratteristiche CSS che influiscono sulle prestazioni più di altre. In questo articolo esaminiamo alcune proprietà CSS che influiscono sulle prestazioni e suggeriamo modi di gestire gli stili per garantire che le prestazioni non siano negativamente influenzate.
- [Il caso aziendale per le prestazioni web](/it/docs/Learn_web_development/Extensions/Performance/business_case_for_performance)
  - : Ci sono molte cose che uno sviluppatore può fare per migliorare le prestazioni, ma quanto veloce è abbastanza veloce? Come si può convincere chi prende le decisioni dell'importanza di questi sforzi? Una volta ottimizzato, come si può garantire che il sovraccarico non ritorni? In questo articolo esaminiamo il convincimento del management, lo sviluppo di una cultura delle prestazioni e un budget delle prestazioni, e introduciamo modi per garantire che le regressioni non si insinuino nel tuo codice.

## Vedi anche

- [Risorse sulle prestazioni web](/it/docs/Learn_web_development/Extensions/Performance/Web_Performance_Basics)
  - : Oltre ai componenti front-end di HTML, CSS, JavaScript e file multimediali, ci sono funzionalità che possono rendere le applicazioni più lente e funzionalità che possono rendere le applicazioni soggettivamente e oggettivamente più veloci. Ci sono molte API, strumenti per sviluppatori, best practice e cattive pratiche relative alle prestazioni web. Qui forniremo un'introduzione a molte di queste funzionalità a livello base e forniremo link per approfondimenti per migliorare le prestazioni per ciascun argomento.
- [Immagini responsive](/it/docs/Web/HTML/Guides/Responsive_images)
  - : In questo articolo, impareremo il concetto di immagini responsive — immagini che funzionano bene su dispositivi con dimensioni dello schermo, risoluzioni e altre caratteristiche ampiamente diverse — e vedremo quali strumenti HTML fornisce per aiutarne l'implementazione. Questo aiuta a migliorare le prestazioni su diversi dispositivi. Le immagini responsive sono solo una parte del [design responsive](/it/docs/Learn_web_development/Core/CSS_layout/Responsive_Design), un futuro argomento CSS da apprendere.
- [Sezione principale delle prestazioni web su MDN](/it/docs/Web/Performance)
  - : La nostra sezione principale sulle prestazioni web — qui troverai molti più dettagli sulle prestazioni web, inclusi panoramiche delle API delle prestazioni, strumenti di test e analisi, e problematiche comuni delle prestazioni.

{{NextMenu("Learn_web_development/Extensions/Performance/why_web_performance", "Learn_web_development/Extensions")}}
