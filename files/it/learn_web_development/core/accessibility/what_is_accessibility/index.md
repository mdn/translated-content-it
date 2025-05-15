---
title: Cos'è l'accessibilità?
slug: Learn_web_development/Core/Accessibility/What_is_accessibility
l10n:
  sourceCommit: eaec5c4226ac64696a95314a7bce995165a4d124
---

{{NextMenu("Learn_web_development/Core/Accessibility/Tooling", "Learn_web_development/Core/Accessibility")}}

Questo articolo inizia il modulo con uno sguardo approfondito su cosa sia l'accessibilità — questa panoramica include i gruppi di persone che dobbiamo considerare e perché, quali strumenti utilizzano le diverse persone per interagire con il web e come possiamo integrare l'accessibilità nel nostro flusso di lavoro di sviluppo web.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Familiarità con <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>.</td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>Il punto dell'accessibilità — aumento dell'accesso ai servizi digitali per coloro che hanno bisogno di supporto aggiuntivo, miglioramento dell'usabilità per tutti, miglior SEO e un pubblico target più ampio.</li>
          <li>Consapevolezza dei requisiti legali in materia di accessibilità.</li>
          <li>Che l'accessibilità dovrebbe essere considerata dall'inizio di un progetto, e non aggiunta interamente alla fine.</li>
          <li>Familiarità con i criteri di conformità delle Linee Guida per l'Accessibilità dei Contenuti Web (WCAG).</li>
          <li>Consapevolezza delle API di accessibilità e del loro scopo.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Allora, cos'è l'accessibilità?

L'accessibilità è la pratica di rendere i siti web usabili dal maggior numero possibile di persone. Tradizionalmente pensiamo che riguardi le persone con disabilità, ma la pratica di rendere i siti accessibili beneficia anche altri gruppi, come coloro che usano dispositivi mobili o che hanno connessioni di rete lente.

Potrebbe anche considerare l'accessibilità come trattare tutti allo stesso modo e offrire loro pari opportunità, indipendentemente dalle loro capacità o circostanze. Proprio come è sbagliato escludere qualcuno da un edificio fisico perché usa una sedia a rotelle (gli edifici pubblici moderni sono generalmente dotati di rampe o ascensori per le sedie a rotelle), non è giusto escludere qualcuno da un sito web perché ha una disabilità visiva. Siamo tutti diversi, ma siamo tutti umani e, pertanto, abbiamo gli stessi diritti umani.

L'accessibilità è la cosa giusta da fare. Fornire siti accessibili è parte delle leggi in alcuni paesi, il che può aprire mercati significativi che altrimenti non sarebbero in grado di utilizzare i servizi o acquistare i prodotti.

Costruire siti accessibili offre benefici a tutti:

- L'HTML semantico, che migliora l'accessibilità, migliora anche la SEO, rendendo il sito più facilmente reperibile.
- Prendersi cura dell'accessibilità dimostra etica e morale corrette, migliorando l'immagine pubblica.
- Altre buone pratiche che migliorano l'accessibilità rendono anche il sito più usabile da altri gruppi, come gli utenti di telefoni cellulari o coloro che hanno connessioni a bassa velocità. In effetti, tutti possono beneficiare di tali miglioramenti.
- Abbiamo già detto che è anche una legge in alcuni luoghi?

## Quali tipi di disabilità stiamo esaminando?

Le persone con disabilità sono tanto diverse quanto quelle senza disabilità, e così le loro disabilità. La lezione chiave qui è pensare al di là del proprio computer e del come si utilizza il web, iniziando a imparare come altri lo usano — _lei non è i suoi utenti_. Le principali tipologie di disabilità da considerare sono spiegate di seguito, insieme agli strumenti speciali che utilizzano per accedere ai contenuti web (noti come **tecnologie assistive** o **AT**).

> [!NOTE]
> La scheda informativa della [World Health Organization](https://www.who.int/en/news-room/fact-sheets/detail/disability-and-health) afferma che "Oltre un miliardo di persone, circa il 15% della popolazione mondiale, ha qualche forma di disabilità" e "Tra i 110 e i 190 milioni di adulti hanno difficoltà significative nel funzionamento".

### Persone con disabilità visive

Le persone con disabilità visive includono ciechi, persone con visione ridotta e daltoniche. Molti con disabilità visive usano ingranditori dello schermo che possono essere sia ingranditori fisici che capacità di zoom software disponibili. La maggior parte dei browser e dei sistemi operativi di oggi ha capacità di zoom. Alcuni utenti si affidano a screen reader, che sono software che leggono ad alta voce il testo digitale. Alcuni esempi di screen reader includono:

- Prodotti commerciali a pagamento, come [JAWS](https://www.freedomscientific.com/Products/software/JAWS/) (Windows) e [Dolphin Screen Reader](https://yourdolphin.com/ScreenReader) (Windows).
- Prodotti gratuiti, come [NVDA](https://www.nvaccess.org/) (Windows), [ChromeVox](https://support.google.com/chromebook/answer/7031755) (Chrome) e [Orca](https://wiki.gnome.org/Projects/Orca) (Linux).
- Software integrato nel sistema operativo, come [VoiceOver](https://www.apple.com/accessibility/features/?vision) (macOS, iPadOS, iOS), [Narrator](https://support.microsoft.com/en-us/windows/complete-guide-to-narrator-e4397a0d-ef4f-b386-d8ae-c172f109bdb1) (Windows), [ChromeVox](https://support.google.com/chromebook/answer/7031755) (su ChromeOS) e [TalkBack](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback) (Android).

È una buona idea prendere familiarità con gli screen reader; dovrebbe anche configurare uno screen reader e sperimentarlo per capire come funziona. Vedere i nostri [tutorial sugli screen reader](/it/docs/Learn_web_development/Core/Accessibility/Tooling#screen_readers) per maggiori dettagli sul loro uso. Il video qui sotto fornisce anche un breve esempio di com'è l'esperienza utente.

{{EmbedYouTube("IK97XMibEws")}}

In termini di statistiche, l'Organizzazione Mondiale della Sanità stima che "285 milioni di persone sono stimate come visivamente disabili in tutto il mondo: 39 milioni sono ciechi e 246 milioni hanno una vista ridotta." (vedere [Disabilità visiva e cecità](https://www.who.int/en/news-room/fact-sheets/detail/blindness-and-visual-impairment)). Si tratta di una popolazione di utenti grande e significativa che potrebbe essere esclusa perché il suo sito non è codificato correttamente — quasi la stessa dimensione della popolazione degli Stati Uniti d'America.

### Persone con disabilità uditive

[Le persone sorde e con problemi di udito (DHH)](https://www.nad.org/resources/american-sign-language/community-and-culture-frequently-asked-questions/) hanno vari livelli di perdita dell'udito, che variano da lieve a profondo. Anche se alcuni utilizzano tecnologie assistive (vedere [Dispositivi assistivi per persone con disturbi dell'udito, della voce, del linguaggio o della parola](https://www.nidcd.nih.gov/health/assistive-devices-people-hearing-voice-speech-or-language-disorders)), non sono ampiamente diffusi.

Per fornire accesso, devono essere forniti alternative testuali. I video dovrebbero essere sottotitolati manualmente e trascrizioni dovrebbero essere disponibili per contenuti audio. Inoltre, a causa dei livelli elevati di [deprivazione linguistica](https://epicspecialeducationstaffing.com/language-deprivation/#:~:text=Language%20deprivation%20is%20the%20term,therefore%20not%20exposed%20to%20language.) nelle popolazioni DHH, [la semplificazione del testo dovrebbe essere considerata](https://circlcenter.org/collaborative-research-automatic-text-simplification-and-reading-assistance-to-support-self-directed-learning-by-deaf-and-hard-of-hearing-computing-workers/).

Le persone sorde e con problemi di udito rappresentano anche una significativa base di utenti — "466 milioni di persone in tutto il mondo hanno una disabilità uditiva grave", afferma il foglio informativo dell'Organizzazione Mondiale della Sanità su [Sordità e perdita dell'udito](https://www.who.int/en/news-room/fact-sheets/detail/deafness-and-hearing-loss).

### Persone con disabilità motorie

Queste persone hanno disabilità riguardanti il movimento, che potrebbero coinvolgere problemi puramente fisici (come la perdita di un arto o paralisi), o disturbi neurologici/genetici che portano a debolezza o perdita di controllo degli arti. Alcune persone potrebbero avere difficoltà a compiere i movimenti precisi richiesti per utilizzare un mouse, mentre altre potrebbero essere più gravemente colpite, magari essendo significativamente paralizzate al punto da dover utilizzare un [puntatore testa](https://www.performancehealth.com/adjustable-headpointer) per interagire con i computer.

Questo tipo di disabilità può anche derivare dall'età avanzata, piuttosto che da un trauma o condizione specifica, e può anche derivare da limitazioni hardware — alcuni utenti potrebbero non avere un mouse.

Il modo in cui questo solitamente influisce sul lavoro di sviluppo web è la necessità che i controlli siano accessibili tramite tastiera — discuteremo l'accessibilità della tastiera in articoli successivi nel modulo, ma è una buona idea provare alcuni siti web usando solo la tastiera per vedere come va. Può usare il tasto Tab per spostarsi tra i diversi controlli di un modulo web, ad esempio? Può trovare maggiori dettagli sui controlli da tastiera nella nostra sezione [Usare controlli UI semantici ove possibile](/it/docs/Learn_web_development/Core/Accessibility/HTML#use_semantic_ui_controls_where_possible).

In termini di statistiche, un numero significante di persone ha disabilità motorie. I Centri per il Controllo e la Prevenzione delle Malattie degli USA [Disabilità e funzionamento (Adulti non istituzionalizzati dai 18 anni in su)](https://www.cdc.gov/nchs/fastats/disability.htm) riportano che negli USA "La percentuale di adulti con difficoltà di funzionamento fisico è del 16,1%".

### Persone con disabilità cognitive

La disabilità cognitiva si riferisce a un'ampia gamma di disabilità, da persone con disabilità intellettive che hanno capacità limitate, a tutte le persone anziane che hanno difficoltà a pensare e ricordare. La gamma include persone con malattie mentali, come [depressione](https://www.nimh.nih.gov/health/topics/depression) e [schizofrenia](https://www.nimh.nih.gov/health/topics/schizophrenia). Include anche persone con difficoltà di apprendimento, come [dislessia](https://www.nichd.nih.gov/health/topics/learningdisabilities) e [disturbo da deficit di attenzione e iperattività](https://www.nimh.nih.gov/health/topics/attention-deficit-hyperactivity-disorder-adhd). Importante, sebbene vi sia una grande diversità nelle definizioni cliniche delle disabilità cognitive, le persone con esse sperimentano un insieme comune di problemi funzionali. Questi includono difficoltà nella comprensione dei contenuti, nel ricordare come completare i compiti, e confusione causata da layout incoerenti delle pagine web.

Una buona base di accessibilità per le persone con disabilità cognitive comprende:

- Fornire contenuto in più di un modo, come attraverso un testo-to-speech o tramite video.
- Contenuto facilmente comprensibile, come testo scritto utilizzando standard di linguaggio semplice.
- Focalizzare l'attenzione sui contenuti importanti.
- Minimizare le distrazioni, come contenuti o pubblicità non necessarie.
- Layout e navigazione delle pagine web coerenti.
- Elementi familiari, come collegamenti sottolineati in blu quando non visitati e in viola quando visitati.
- Divisione dei processi in passaggi logici ed essenziali con indicatori di progresso.
- Autenticazione del sito web il più semplice possibile senza compromettere la sicurezza.
- Rendere facili da completare i moduli, ad esempio con messaggi di errore chiari e un recupero degli errori semplice.

### Note

- Progettare con [accessibilità cognitiva](/it/docs/Web/Accessibility/Guides/Cognitive_accessibility) porterà a buone pratiche di design. Ne beneficeranno tutti.
- Molte persone con disabilità cognitive hanno anche disabilità fisiche. I siti web devono conformarsi alle [Linee Guida per l'Accessibilità dei Contenuti Web](https://www.w3.org/WAI/standards-guidelines/wcag/) del W3C, incluse [linee guida per l'accessibilità cognitiva](/it/docs/Web/Accessibility/Guides/Cognitive_accessibility#wcag_guidelines).
- Il [Cognitive and Learning Disabilities Accessibility Task Force](https://www.w3.org/WAI/GL/task-forces/coga/) del W3C produce linee guida web per l'accessibilità per persone con disabilità cognitive.
- WebAIM ha una [pagina dedicata alla cognitiva](https://webaim.org/articles/cognitive/) con risorse e informazioni rilevanti.
- I Centri per il Controllo e la Prevenzione delle Malattie degli Stati Uniti stimano che, a partire dal 2018, 1 cittadino statunitense su 4 abbia una disabilità e, tra loro, [la disabilità cognitiva è la più comune tra i giovani](https://archive.cdc.gov/www_cdc_gov/media/releases/2018/p0816-disability.html).
- Negli USA, alcune disabilità intellettive sono storicamente state chiamate "ritardo mentale." Molti ora considerano questo termine dispregiativo, quindi se ne dovrebbe evitare l'uso.
- Nel Regno Unito, alcune disabilità intellettive sono indicate come "learning disabilities" o "learning difficulties".

## Implementare l'accessibilità nel suo progetto

Un mito comune sull'accessibilità è che essa sia un "extra" costoso da implementare su un progetto. Questo mito può effettivamente _essere_ vero se:

- Sta cercando di "installare ex post" l'accessibilità su un sito web esistente che ha problemi significativi di accessibilità.
- Ha iniziato a considerare l'accessibilità e ha scoperto problemi correlati nelle fasi finali di un progetto.

Se però, considera l'accessibilità fin dall'inizio di un progetto, il costo per rendere la maggior parte del contenuto accessibile dovrebbe essere abbastanza minimo.

Quando pianifica il suo progetto, consideri il test di accessibilità come parte del suo regime di test, proprio come testa per qualsiasi altro segmento di pubblico target importante (per esempio, browser per desktop o mobile). Testa presto e spesso, idealmente eseguendo test automatici per rilevare ausili programmabili mancanti (come mancano [testo alternativo](/it/docs/Learn_web_development/Core/Accessibility/HTML#text_alternatives) o cattivo testo del collegamento — vedere [Usare etichette di testo significative](/it/docs/Learn_web_development/Core/Accessibility/HTML#use_meaningful_text_labels)) e facendo alcuni test con gruppi di utenti con disabilità per vedere come funzionano bene le funzionalità del sito più complesse per loro. Ad esempio:

- Il mio widget per la selezione della data è utilizzabile da persone che usano screen reader?
- Se il contenuto si aggiorna dinamicamente, le persone con disabilità visive lo sanno?
- I miei pulsanti UI sono accessibili sia agli utenti di tastiera che di interfaccia touch?

Può e dovrebbe tenere nota delle potenziali aree problematiche nel suo contenuto che richiederanno lavoro per renderlo accessibile, assicurarsi che siano testate accuratamente e pensare a soluzioni/alternative. I contenuti testuali (come vedrà nel prossimo articolo) sono facili, ma che dire dei suoi contenuti multimediali, e delle sue grafiche 3D avanzate? Dovrebbe esaminare il budget del progetto e pensare a quali soluzioni sono disponibili per rendere accessibili tali contenuti. Avere tutti i contenuti multimediali trascritti è un'opzione possibile che, sebbene costosa, è fattibile.

Inoltre, sia realistico. "100% accessibilità" è un ideale irraggiungibile — incontrerà sempre qualche tipo di caso limite che risulterà in un certo utente che trova un certo contenuto difficile da usare — ma dovrebbe fare tutto il possibile. Se prevede di includere una grafica a torta 3D avanzata realizzata utilizzando WebGL, potrebbe voler includere una tabella dati come alternativa accessibile della rappresentazione dei dati. Oppure, potrebbe voler solo includere la tabella e rinunciare alla grafica a torta 3D — la tabella è accessibile a tutti, più veloce da codificare, meno impegnativa per la CPU e più facile da mantenere.

D'altra parte, se sta lavorando su un sito web di galleria che mostra arte 3D interessante, sarebbe irragionevole aspettarsi che ogni pezzo di arte sia perfettamente accessibile alle persone con disabilità visive, dato che si tratta di un medium interamente visivo.

Per dimostrare che ha a cuore e ha pensato all'accessibilità, pubblichi una dichiarazione sull'accessibilità sul suo sito che dettagli quale sia la sua politica verso l'accessibilità, e quali passi ha intrapreso per rendere il sito accessibile. Se qualcuno le notifica che il suo sito ha un problema di accessibilità, cominci un dialogo con loro, sia empatico e prenda misure ragionevoli per cercare di risolvere il problema.

Per riassumere:

- Consideri l'accessibilità dall'inizio di un progetto e testi presto e spesso. Proprio come qualsiasi altro bug, un problema di accessibilità diventa più costoso da risolvere più tardi viene scoperto.
- Tenga presente che molte delle migliori pratiche di accessibilità beneficiano tutti, non solo gli utenti con disabilità. Ad esempio, il markup semantico snello non è solo buono per gli screen reader, ma è anche veloce da caricare e performante. Questo beneficia tutti, soprattutto quelli su dispositivi mobili e/o connessioni lente.
- Pubblicare una dichiarazione di accessibilità sul suo sito e interagire con le persone che hanno problemi.

## Linee guida per l'accessibilità e la legge

Esistono numerosi elenchi di controllo e insiemi di linee guida disponibili su cui basare i test di accessibilità, che potrebbero sembrare opprimenti a prima vista. Il nostro consiglio è di familiarizzare con le aree di base in cui bisogna prestare attenzione, oltre a comprendere le strutture ad alto livello delle linee guida più rilevanti per lei.

- Per cominciare, il W3C ha pubblicato un documento ampio e molto dettagliato che include criteri molto precisi, indipendenti dalla tecnologia, per la conformità all'accessibilità. Queste sono chiamate [Linee guida per l'accessibilità dei contenuti web](https://www.w3.org/WAI/standards-guidelines/wcag/) (WCAG), e non sono sicuramente una lettura breve. I criteri sono suddivisi in quattro categorie principali, che specificano come le implementazioni possono essere rese percepibili, utilizzabili, comprensibili e solide. Il posto migliore per avere una leggera introduzione e iniziare a imparare è [WCAG a colpo d'occhio](https://www.w3.org/WAI/standards-guidelines/wcag/glance/). Non c'è bisogno di imparare tutti i criteri WCAG — essere consapevoli delle principali aree di preoccupazione, e usare una varietà di tecniche e strumenti per evidenziare eventuali aree che non si conformano ai criteri WCAG (vedi sotto per ulteriori informazioni).
- Il suo paese può anche avere legislazioni specifiche che governano la necessità per i siti web che servono la loro popolazione di essere accessibili — ad esempio [EN 301 549](https://www.etsi.org/deliver/etsi_en/301500_301599/301549/02.01.02_60/en_301549v020102p.pdf) nell'UE, [Sezione 508 del Rehabilitation Act](https://www.section508.gov/training/) negli Stati Uniti, [Ordinanza federale sulla tecnologia dell'informazione accessibile](https://www.aktion-mensch.de/inklusion/barrierefreiheit/barrierefreie-website) in Germania, i [Regulations 2018 per l'accessibilità](https://www.legislation.gov.uk/uksi/2018/952/introduction/made) nel Regno Unito, [Accessibilità](https://www.agid.gov.it/it/ambiti-intervento/accessibilita-usabilita) in Italia, il [Disability Discrimination Act](https://humanrights.gov.au/our-work/disability-rights/publications/guidelines-equal-access-digital-goods-and-services) in Australia, ecc. Il W3C mantiene un elenco di [Leggi e politiche sull'accessibilità web](https://www.w3.org/WAI/policies/) per paese.

Così, mentre le WCAG sono un insieme di linee guida, il vostro paese probabilmente avrà leggi che governano l'accessibilità web, o almeno l'accessibilità dei servizi disponibili al pubblico (che potrebbe includere siti web, televisione, spazi fisici, ecc.) È una buona idea scoprire quali siano le vostre leggi. Se non si fa alcuno sforzo per controllare che il contenuto sia accessibile, si potrebbe essere legalmente responsabili se le persone si lamentano.

Questo sembra serio, ma in realtà deve solo considerare l'accessibilità come la priorità principale delle pratiche di sviluppo web, come delineato sopra. In caso di dubbio, ottenga consigli da un avvocato qualificato. Non offriremo altri consigli oltre a questo, perché non siamo avvocati.

## API di accessibilità

I browser web fanno uso di speciali **API di accessibilità** (fornite dal sistema operativo sottostante) che espongono informazioni utili per le tecnologie assistive (AT) — le tecnologie assistive tendono per lo più a utilizzare informazioni semantiche, quindi queste informazioni non includono cose come informazioni sui dettagli stilistici o JavaScript. Queste informazioni sono strutturate in un albero di informazioni chiamato **accessibility tree**.

I diversi sistemi operativi hanno diverse API di accessibilità disponibili:

- Windows: MSAA/IAccessible, UIAExpress, IAccessible2
- macOS: NSAccessibility
- Linux: AT-SPI
- Android: Framework di accessibilità
- iOS: UIAccessibility

Dove l'informazione semantica nativa fornita dagli elementi HTML nelle sue app web viene a mancare, può integrarla con le funzionalità della [specifica WAI-ARIA](https://www.w3.org/TR/wai-aria/), che aggiungono informazioni semantiche all'albero di accessibilità per migliorare l'accessibilità. Può imparare molto di più su WAI-ARIA nel nostro articolo [Basi di WAI-ARIA](/it/docs/Learn_web_development/Core/Accessibility/WAI-ARIA_basics).

## Riassunto

Questo articolo dovrebbe averle dato una panoramica utile ad alto livello sull'accessibilità, mostrato perché è importante e visto come può adattarsi al suo flusso di lavoro. Dovrebbe ora anche avere sete di imparare i dettagli di implementazione che possono rendere i siti accessibili, e quali strumenti possono aiutare. Esamineremo gli strumenti di accessibilità nel prossimo articolo.

## Vedere anche

- [WCAG](/it/docs/Web/Accessibility/Guides/Understanding_WCAG)

  - [Percepibile](/it/docs/Web/Accessibility/Guides/Understanding_WCAG/Perceivable)
  - [Utilizzabile](/it/docs/Web/Accessibility/Guides/Understanding_WCAG/Operable)
  - [Comprensibile](/it/docs/Web/Accessibility/Guides/Understanding_WCAG/Understandable)
  - [Solido](/it/docs/Web/Accessibility/Guides/Understanding_WCAG/Robust)

- [Google Chrome ha rilasciato un'estensione di sottotitolazione automatica](https://blog.google/products/chrome/live-caption-chrome/)

{{NextMenu("Learn_web_development/Core/Accessibility/Tooling", "Learn_web_development/Core/Accessibility")}}
