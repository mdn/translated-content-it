---
title: Che cos'è l'accessibilità?
slug: Learn_web_development/Core/Accessibility/What_is_accessibility
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Core/Accessibility/Tooling", "Learn_web_development/Core/Accessibility")}}

Questo articolo introduce il modulo con uno sguardo approfondito su cosa sia l'accessibilità. Questa panoramica include quali gruppi di persone dobbiamo considerare e perché, quali strumenti diverse persone usano per interagire con il web e come possiamo rendere l'accessibilità parte del nostro flusso di lavoro di sviluppo web.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Familiarità con <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>.</td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Lo scopo dell'accessibilità — maggiore accesso ai servizi digitali per coloro che hanno bisogni aggiuntivi, miglioramento dell'usabilità per tutti, miglior SEO e un pubblico target più ampio.</li>
          <li>Consapevolezza dei requisiti legali dell'accessibilità.</li>
          <li>L'accessibilità dovrebbe essere considerata dall'inizio di un progetto, e non aggiunta alla fine.</li>
          <li>Familiarità con i criteri di conformità delle Web Content Accessibility Guidelines (WCAG).</li>
          <li>Consapevolezza delle API di accessibilità e del loro scopo.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Allora cos'è l'accessibilità?

L'accessibilità è la pratica di rendere i vostri siti web utilizzabili dal maggior numero di persone possibile. Tradizionalmente pensiamo che questa riguardi le persone con disabilità, ma praticare l'accessibilità dei siti beneficia anche altri gruppi come quelli che usano dispositivi mobili, o coloro con connessioni di rete lente.

Si potrebbe anche pensare all'accessibilità come al trattamento di tutti allo stesso modo, dando loro pari opportunità, indipendentemente dalle loro abilità o circostanze. Proprio come è sbagliato escludere qualcuno da un edificio fisico perché è su una sedia a rotelle (gli edifici pubblici moderni in genere hanno rampe per sedie a rotelle o ascensori), non è giusto escludere qualcuno da un sito web perché ha un deficit visivo. Siamo tutti diversi, ma siamo tutti umani e pertanto abbiamo gli stessi diritti umani.

Fornire siti accessibili è la cosa giusta da fare. Eseguire siti accessibili è parte della legge in alcuni paesi, il che può aprire mercati significativi che altrimenti non sarebbero in grado di usare i vostri servizi o acquistare i vostri prodotti.

Costruire siti accessibili beneficia a tutti:

- L'HTML semantico, che migliora l'accessibilità, migliora anche la SEO, rendendo il vostro sito più rintracciabile.
- Prendersi cura dell'accessibilità dimostra buone etiche e morale, migliorando la vostra immagine pubblica.
- Altre buone pratiche che migliorano l'accessibilità rendono anche il sito più utilizzabile da altri gruppi, come gli utenti di telefoni cellulari o coloro che hanno bassa velocità di rete. Infatti, tutti possono beneficiare di molti di tali miglioramenti.
- Abbiamo menzionato che è anche legge in alcuni luoghi?

## Quali tipi di disabilità stiamo considerando?

Le persone con disabilità sono tanto diverse quanto le persone senza disabilità, e così sono le loro disabilità. La lezione chiave qui è pensare oltre il proprio computer e come si usa il web, e iniziare a imparare come altri lo usano — _lei non è i suoi utenti_. I principali tipi di disabilità da considerare sono spiegati di seguito, insieme agli strumenti speciali che usano per accedere ai contenuti web (noti come **tecnologie assistive**, o **AT**).

> [!NOTE]
> Il foglio informativo dell'Organizzazione Mondiale della Sanità [Disabilità e salute](https://www.who.int/en/news-room/fact-sheets/detail/disability-and-health) afferma che "Oltre un miliardo di persone, circa il 15% della popolazione mondiale, hanno qualche forma di disabilità", e "Tra 110 e 190 milioni di adulti hanno difficoltà significative nel funzionamento."

### Persone con disabilità visive

Le persone con disabilità visive includono persone con cecità, visione ridotta e daltonismo. Molti individui con disabilità visive utilizzano ingranditori di schermo che possono essere ingranditori fisici o capacità di zoom del software. La maggior parte dei browser e dei sistemi operativi oggigiorno hanno capacità di zoom. Alcuni utenti si affideranno ai lettori di schermo, che sono software che leggono il testo digitale ad alta voce. Alcuni esempi di lettori di schermo includono:

- Prodotti commerciali a pagamento, come [JAWS](https://www.freedomscientific.com/Products/software/JAWS/) (Windows) e [Dolphin Screen Reader](https://yourdolphin.com/ScreenReader) (Windows).
- Prodotti gratuiti, come [NVDA](https://www.nvaccess.org/) (Windows), [ChromeVox](https://support.google.com/chromebook/answer/7031755) (Chrome), e [Orca](https://wiki.gnome.org/Projects/Orca) (Linux).
- Software integrato nel sistema operativo, come [VoiceOver](https://www.apple.com/accessibility/vision/) (macOS, iPadOS, iOS), [Narrator](https://support.microsoft.com/en-us/windows/complete-guide-to-narrator-e4397a0d-ef4f-b386-d8ae-c172f109bdb1) (Windows), [ChromeVox](https://support.google.com/chromebook/answer/7031755) (su ChromeOS), e [TalkBack](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback) (Android).

È una buona idea familiarizzare con i lettori di schermo; dovrebbe anche impostare un lettore di schermo e fare esperimenti con esso, per avere un'idea di come funziona. Veda i nostri [tutorial sui lettori di schermo](/it/docs/Learn_web_development/Core/Accessibility/Tooling#screen_readers) per maggiori dettagli sull'uso di questi strumenti. Il video sotto fornisce anche un esempio breve di quale sia l'esperienza.

{{EmbedYouTube("IK97XMibEws")}}

In termini di statistiche, l'Organizzazione Mondiale della Sanità stima che "si stima che 285 milioni di persone siano ipovedenti in tutto il mondo: 39 milioni sono ciechi e 246 milioni hanno una visione ridotta." (vedi [Disabilità visiva e cecità](https://www.who.int/en/news-room/fact-sheets/detail/blindness-and-visual-impairment)). Questa è una popolazione di utenti ampia e significativa da mancare solo perché il vostro sito non è codificato correttamente — quasi della stessa dimensione della popolazione degli Stati Uniti d'America.

### Persone con disabilità uditive

Le persone [sorde e con problemi di udito (DHH)](https://www.nad.org/resources/american-sign-language/community-and-culture-frequently-asked-questions/) hanno vari livelli di perdita dell'udito che vanno da lieve a profonda. Sebbene alcuni utilizzino AT (vedi [Dispositivi assistivi per persone con disturbi dell'udito, della voce, del linguaggio](https://www.nidcd.nih.gov/health/assistive-devices-people-hearing-voice-speech-or-language-disorders)), non sono diffusi.

Per fornire accesso, devono essere forniti alternative testuali. I video dovrebbero essere sottotitolati manualmente e devono essere forniti trascrizioni per contenuti audio. Inoltre, a causa degli alti livelli di [privazione linguistica](https://epicspecialeducationstaffing.com/language-deprivation/#:~:text=Language%20deprivation%20is%20the%20term,therefore%20not%20exposed%20to%20language.) nelle popolazioni DHH, [la semplificazione del testo dovrebbe essere presa in considerazione](https://circlcenter.org/collaborative-research-automatic-text-simplification-and-reading-assistance-to-support-self-directed-learning-by-deaf-and-hard-of-hearing-computing-workers/).

Le persone sorde e con problemi di udito rappresentano anche una significativa base di utenti — "466 milioni di persone nel mondo hanno una perdita uditiva disabilitante", afferma il foglio informativo dell'Organizzazione Mondiale della Sanità [Sordità e perdita dell'udito](https://www.who.int/en/news-room/fact-sheets/detail/deafness-and-hearing-loss).

### Persone con disabilità motorie

Queste persone hanno disabilità riguardanti il movimento, che potrebbero coinvolgere puramente problemi fisici (come perdita di un arto o paralisi), o disturbi neurologici/genetici che portano a debolezza o perdita di controllo degli arti. Alcune persone potrebbero avere difficoltà nello svolgere i movimenti precisi richiesti per usare un mouse, mentre altre potrebbero essere più gravemente colpite, forse significativamente paralizzate al punto da dover usare un [puntatore con la testa](https://www.performancehealth.com/adjustable-headpointer) per interagire con i computer.

Questo tipo di disabilità può essere anche il risultato dell'età avanzata, piuttosto che un trauma specifico o una condizione, e potrebbe anche derivare da limitazioni hardware — alcuni utenti potrebbero non avere un mouse.

Il modo in cui questo generalmente influisce sul lavoro di sviluppo web è il requisito che i controlli siano accessibili tramite tastiera — discuteremo dell'accessibilità della tastiera in articoli successivi del modulo, ma è una buona idea provare alcuni siti web utilizzando solo la tastiera per vedere come se la cava. Può utilizzare il tasto Tab per spostarsi tra i diversi controlli di un modulo web, ad esempio? Può trovare maggiori dettagli sui controlli della tastiera nella nostra sezione [Utilizzare i controlli UI semantici dove possibile](/it/docs/Learn_web_development/Core/Accessibility/HTML#use_semantic_ui_controls_where_possible).

In termini di statistiche, un numero significativo di persone ha disabilità motorie. I Centri per il Controllo e la Prevenzione delle Malattie degli Stati Uniti [Disabilità e Funzionamento (Adulti non istituzionalizzati di 18 anni e oltre)](https://www.cdc.gov/nchs/fastats/disability.htm) riportano che negli USA "Percentuale di adulti con qualsiasi difficoltà di funzionamento fisico: 16,1%".

### Persone con disabilità cognitive

La disabilità cognitiva si riferisce a un ampio range di disabilità, dalle persone con disabilità intellettiva che hanno le capacità più limitate, a tutte le persone mentre invecchiano e hanno difficoltà a pensare e ricordare. Il range include persone con malattie mentali, come [depressione](https://www.nimh.nih.gov/health/topics/depression) e [schizofrenia](https://www.nimh.nih.gov/health/topics/schizophrenia). Include anche persone con disabi
