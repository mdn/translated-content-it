---
title: Che cos'è l'accessibilità?
slug: Learn_web_development/Core/Accessibility/What_is_accessibility
l10n:
  sourceCommit: 8a6ebefa23ff414c256ee69d08fc20bd5ebe540b
---

{{NextMenu("Learn_web_development/Core/Accessibility/Tooling", "Learn_web_development/Core/Accessibility")}}

Questo articolo avvia il modulo con una panoramica approfondita di cosa sia l'accessibilità: include i gruppi di persone da considerare e il motivo, gli strumenti che persone diverse usano per interagire con il web e come rendere l'accessibilità parte del flusso di lavoro di sviluppo web.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Familiarità con <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Lo scopo dell'accessibilità: maggiore accesso ai servizi digitali per chi ha esigenze aggiuntive, migliore usabilità per tutti, SEO migliore e un pubblico di destinazione più ampio.</li>
          <li>Consapevolezza dei requisiti legali relativi all'accessibilità.</li>
          <li>L'accessibilità deve essere considerata fin dall'inizio di un progetto e non aggiunta alla fine.</li>
          <li>Familiarità con i criteri di conformità delle Web Content Accessibility Guidelines (WCAG).</li>
          <li>Consapevolezza delle accessibility API e del loro scopo.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Che cos'è dunque l'accessibilità?

L'accessibilità è la pratica di rendere i siti web utilizzabili dal maggior numero possibile di persone. Tradizionalmente si pensa che riguardi le persone con disabilità, ma la pratica di rendere accessibili i siti apporta benefici anche ad altri gruppi, come chi utilizza dispositivi mobili o chi dispone di connessioni di rete lente.

Si può anche intendere l'accessibilità come il trattare tutti allo stesso modo e offrire pari opportunità, indipendentemente dalle capacità o dalle circostanze. Così come è sbagliato escludere una persona da un edificio fisico perché usa una sedia a rotelle (gli edifici pubblici moderni dispongono generalmente di rampe o ascensori), non è corretto escludere una persona da un sito web perché ha una disabilità visiva. Siamo tutti diversi, ma siamo tutti esseri umani e pertanto abbiamo gli stessi diritti umani.

L'accessibilità è la scelta giusta. Fornire siti accessibili è previsto dalla legge in alcuni paesi, e ciò può aprire mercati importanti che altrimenti non potrebbero utilizzare i servizi o acquistare i prodotti.

La realizzazione di siti accessibili apporta benefici a tutti:

- L'HTML semantico, che migliora l'accessibilità, migliora anche la SEO, rendendo il sito più facilmente individuabile.
- Occuparsi dell'accessibilità dimostra una buona etica e una buona morale, migliorando l'immagine pubblica.
- Altre buone pratiche che migliorano l'accessibilità rendono il sito più utilizzabile anche da altri gruppi, come gli utenti di telefoni cellulari o chi dispone di una connessione di rete lenta. In effetti, tutti possono beneficiare di molti di questi miglioramenti.
- Abbiamo già detto che in alcuni luoghi è previsto anche dalla legge?

## Quali tipi di disabilità vengono considerati?

Le persone con disabilità sono tanto diverse quanto le persone senza disabilità, e lo stesso vale per le loro disabilità. La lezione fondamentale è pensare oltre il proprio computer e il proprio modo di usare il web, e iniziare a conoscere il modo in cui lo usano gli altri: _non si è i propri utenti_. Di seguito vengono illustrati i principali tipi di disabilità da considerare, insieme agli eventuali strumenti speciali usati per accedere ai contenuti web, noti come **tecnologie assistive** o **AT**.

> [!NOTE]
> La scheda informativa dell'Organizzazione mondiale della sanità [Disability and health](https://www.who.int/en/news-room/fact-sheets/detail/disability-and-health) afferma che "oltre un miliardo di persone, circa il 15% della popolazione mondiale, ha una qualche forma di disabilità" e che "tra 110 milioni e 190 milioni di adulti hanno notevoli difficoltà di funzionamento".

### Persone con disabilità visive

Le persone con disabilità visive includono persone non vedenti, ipovedenti e daltoniche. Molte persone con disabilità visive usano ingranditori dello schermo, che possono essere lenti fisiche oppure funzionalità software di zoom. Oggi la maggior parte dei browser e dei sistemi operativi dispone di funzionalità di zoom. Alcuni utenti si affidano agli screen reader, software che leggono ad alta voce il testo digitale. Alcuni esempi di screen reader includono:

- Prodotti commerciali a pagamento, come [JAWS](https://vispero.com/jaws-screen-reader-software/) (Windows) e [Dolphin Screen Reader](https://yourdolphin.com/ScreenReader) (Windows).
- Prodotti gratuiti, come [NVDA](https://www.nvaccess.org/) (Windows), [ChromeVox](https://support.google.com/chromebook/answer/7031755) (Chrome) e [Orca](https://help.gnome.org/orca/introduction.html) (Linux, installato per impostazione predefinita in diverse distribuzioni).
- Software integrato nel sistema operativo, come [VoiceOver](https://www.apple.com/accessibility/features/?vision) (macOS, iPadOS, iOS), [Narrator](https://support.microsoft.com/en-US/accessibility/windows/narrator/complete-guide-to-narrator) (Windows), [ChromeVox](https://support.google.com/chromebook/answer/7031755) (su ChromeOS) e [TalkBack](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback) (Android).

È una buona idea familiarizzare con gli screen reader; è inoltre opportuno configurarne uno e sperimentarlo, per comprendere come funziona. Per maggiori dettagli sul loro utilizzo, consultare i nostri [tutorial sugli screen reader](/it/docs/Learn_web_development/Core/Accessibility/Tooling#screen_readers). Anche il video seguente fornisce un breve esempio dell'esperienza d'uso.

{{EmbedYouTube("IK97XMibEws")}}

In termini statistici, l'Organizzazione mondiale della sanità stima che "nel mondo circa 285 milioni di persone hanno disabilità visive: 39 milioni sono non vedenti e 246 milioni hanno una ridotta capacità visiva" (vedere [Visual impairment and blindness](https://www.who.int/en/news-room/fact-sheets/detail/blindness-and-visual-impairment)). Si tratta di una popolazione di utenti ampia e significativa da perdere semplicemente perché il sito non è programmato correttamente: ha quasi le stesse dimensioni della popolazione degli Stati Uniti d'America.

### Persone con disabilità uditive

Le persone [sorde e con problemi di udito (DHH)](https://www.nad.org/resources/american-sign-language/community-and-culture-frequently-asked-questions/) presentano diversi livelli di perdita dell'udito, da lieve a profonda. Sebbene alcune usino le AT (vedere [Assistive Devices for People with Hearing, Voice, Speech, or Language Disorders](https://www.nidcd.nih.gov/health/assistive-devices-people-hearing-voice-speech-or-language-disorders)), queste non sono diffuse.

Per fornire accesso, è necessario offrire alternative testuali. I video devono essere sottotitolati manualmente e per i contenuti audio devono essere fornite trascrizioni. Inoltre, a causa degli alti livelli di [deprivazione linguistica](https://epicspecialeducationstaffing.com/language-deprivation/#:~:text=Language%20deprivation%20is%20the%20term,therefore%20not%20exposed%20to%20language.) nelle popolazioni DHH, è opportuno [considerare la semplificazione del testo](https://circlcenter.org/collaborative-research-automatic-text-simplification-and-reading-assistance-to-support-self-directed-learning-by-deaf-and-hard-of-hearing-computing-workers/).

Anche le persone sorde e con problemi di udito rappresentano una base di utenti significativa: secondo la scheda informativa dell'Organizzazione mondiale della sanità [Deafness and hearing loss](https://www.who.int/en/news-room/fact-sheets/detail/deafness-and-hearing-loss), "466 milioni di persone nel mondo hanno una perdita dell'udito invalidante".

### Persone con disabilità motorie

Queste persone hanno disabilità che riguardano il movimento, che possono comportare problemi puramente fisici, come la perdita di un arto o la paralisi, oppure disturbi neurologici o genetici che causano debolezza o perdita di controllo degli arti. Alcune persone potrebbero avere difficoltà a compiere i precisi movimenti della mano necessari per usare un mouse, mentre altre potrebbero essere colpite più gravemente, magari con una paralisi significativa tale da rendere necessario l'uso di un [head pointer](https://www.performancehealth.com/adjustable-headpointer) per interagire con i computer.

Questo tipo di disabilità può anche essere il risultato della vecchiaia, anziché di uno specifico trauma o condizione, e può derivare anche da limitazioni hardware: alcuni utenti potrebbero non avere un mouse.

Il modo in cui ciò influisce generalmente sullo sviluppo web è il requisito che i controlli siano accessibili tramite tastiera. L'accessibilità tramite tastiera verrà discussa negli articoli successivi del modulo, ma è una buona idea provare alcuni siti web usando solo la tastiera per verificare come funziona. È possibile usare il tasto Tab per spostarsi tra i diversi controlli di un modulo web, ad esempio? Maggiori dettagli sui controlli da tastiera sono disponibili nella sezione [Usare controlli UI semantici quando possibile](/it/docs/Learn_web_development/Core/Accessibility/HTML#use_semantic_ui_controls_where_possible).

In termini statistici, un numero significativo di persone ha disabilità motorie. I Centers for Disease Control and Prevention degli Stati Uniti riportano in [Disability and Functioning (Non-institutionalized Adults 18 Years and Over)](https://www.cdc.gov/nchs/fastats/disability.htm) che negli USA la "percentuale di adulti con qualsiasi difficoltà di funzionamento fisico è del 16,1%".

### Persone con disabilità cognitive

La disabilità cognitiva si riferisce a un'ampia gamma di disabilità, dalle persone con disabilità intellettive che hanno capacità più limitate, fino a tutti noi quando invecchiamo e abbiamo difficoltà a pensare e ricordare. La gamma include persone con malattie mentali, come la [depressione](https://www.nimh.nih.gov/health/topics/depression) e la [schizofrenia](https://www.nimh.nih.gov/health/topics/schizophrenia). Include inoltre persone con disturbi dell'apprendimento, come la [dislessia](https://www.nichd.nih.gov/health/topics/learningdisabilities) e il [disturbo da deficit di attenzione e iperattività](https://www.nimh.nih.gov/health/topics/attention-deficit-hyperactivity-disorder-adhd). È importante sottolineare che, sebbene esista molta diversità nelle definizioni cliniche delle disabilità cognitive, le persone che ne sono affette sperimentano un insieme comune di problemi funzionali. Questi includono difficoltà nella comprensione dei contenuti, nel ricordare come completare le attività e confusione causata da layout incoerenti delle pagine web.

Una buona base di accessibilità per le persone con disabilità cognitive include:

- Fornire contenuti in più di un modo, ad esempio tramite sintesi vocale o video.
- Contenuti facilmente comprensibili, come testi scritti secondo standard di linguaggio semplice.
- Focalizzare l'attenzione sui contenuti importanti.
- Ridurre al minimo le distrazioni, come contenuti non necessari o pubblicità.
- Layout e navigazione delle pagine web coerenti.
- Elementi familiari, come link sottolineati blu quando non visitati e viola quando visitati.
- Suddividere i processi in passaggi logici ed essenziali con indicatori di avanzamento.
- Rendere l'autenticazione sul sito web il più semplice possibile senza compromettere la sicurezza.
- Rendere i moduli facili da completare, ad esempio con messaggi di errore chiari e un semplice recupero dagli errori.

### Note

- Progettare considerando l'[accessibilità cognitiva](/it/docs/Web/Accessibility/Guides/Cognitive_accessibility) porta a buone pratiche di progettazione, a beneficio di tutti.
- Molte persone con disabilità cognitive hanno anche disabilità fisiche. I siti web devono conformarsi alle [Web Content Accessibility Guidelines](https://www.w3.org/WAI/standards-guidelines/wcag/) del W3C, incluse le [linee guida sull'accessibilità cognitiva](/it/docs/Web/Accessibility/Guides/Cognitive_accessibility#wcag_guidelines).
- La [Cognitive and Learning Disabilities Accessibility Task Force](https://www.w3.org/WAI/GL/task-forces/coga/) del W3C produce linee guida per l'accessibilità web destinate alle persone con disabilità cognitive.
- WebAIM dispone di una [pagina sulle capacità cognitive](https://webaim.org/articles/cognitive/) con informazioni e risorse pertinenti.
- I Centers for Disease Control degli Stati Uniti stimano che, nel 2018, 1 cittadino statunitense su 4 avesse una disabilità e che, tra questi, [la disabilità cognitiva fosse la più comune tra i giovani](https://archive.cdc.gov/www_cdc_gov/media/releases/2018/p0816-disability.html).
- Negli Stati Uniti alcune disabilità intellettive sono state storicamente indicate come "mental retardation". Molti oggi considerano questo termine dispregiativo, pertanto se ne dovrebbe evitare l'uso.
- Nel Regno Unito alcune disabilità intellettive sono indicate come "learning disabilities" o "learning difficulties".

## Integrare l'accessibilità nel progetto

Un mito comune sull'accessibilità è che implementarla in un progetto sia un costoso "extra aggiuntivo". Questo mito può effettivamente essere vero se:

- Si cerca di "adattare" l'accessibilità a un sito web esistente con problemi di accessibilità significativi.
- Si inizia a considerare l'accessibilità solo nelle fasi finali di un progetto, scoprendo problemi correlati.

Tuttavia, se l'accessibilità viene considerata fin dall'inizio di un progetto, il costo per rendere accessibile la maggior parte dei contenuti dovrebbe essere piuttosto contenuto.

Quando si pianifica il progetto, includere i test di accessibilità nel regime di test, proprio come i test per qualsiasi altro importante segmento del pubblico di destinazione, ad esempio i browser desktop o mobili di destinazione. Eseguire test tempestivamente e spesso, idealmente usando test automatizzati per rilevare funzionalità mancanti rilevabili programmaticamente, come il [testo alternativo](/it/docs/Learn_web_development/Core/Accessibility/HTML#text_alternatives) mancante per le immagini o testo dei link inadeguato — vedere [Usare etichette di testo significative](/it/docs/Learn_web_development/Core/Accessibility/HTML#use_meaningful_text_labels) — e svolgendo test con gruppi di utenti con disabilità per verificare quanto bene funzionino per loro le funzionalità più complesse del sito. Ad esempio:

- Il widget per la selezione delle date è utilizzabile da persone che usano screen reader?
- Se il contenuto si aggiorna dinamicamente, le persone con disabilità visive ne vengono a conoscenza?
- I pulsanti UI sono accessibili sia agli utenti della tastiera sia a quelli delle interfacce touch?

È possibile, e opportuno, annotare nel contenuto le potenziali aree problematiche che richiederanno lavoro per diventare accessibili, assicurandosi che vengano testate approfonditamente e pensando a soluzioni o alternative. Il contenuto testuale, come verrà illustrato nel prossimo articolo, è semplice, ma che dire dei contenuti multimediali e della grafica 3D accattivante? Occorre esaminare il budget del progetto e valutare le soluzioni disponibili per rendere accessibili tali contenuti. Avere tutti i contenuti multimediali trascritti è un'opzione che, pur essendo costosa, è possibile.

È inoltre necessario essere realistici. Un'"accessibilità al 100%" è un ideale irraggiungibile: si incontrerà sempre qualche caso limite che rende difficile per un determinato utente utilizzare un determinato contenuto. Tuttavia, è opportuno fare il più possibile. Se si prevede di includere un accattivante grafico a torta 3D realizzato con WebGL, si potrebbe includere una tabella di dati come rappresentazione alternativa accessibile dei dati. In alternativa, si potrebbe semplicemente includere la tabella e rimuovere il grafico a torta 3D: la tabella è accessibile a tutti, più veloce da programmare, meno intensiva per la CPU e più facile da mantenere.

D'altra parte, se si lavora a un sito web di galleria che mostra interessanti opere d'arte 3D, sarebbe irragionevole aspettarsi che ogni opera d'arte sia perfettamente accessibile alle persone con disabilità visive, dato che si tratta di un mezzo interamente visivo.

Per dimostrare attenzione e riflessione sull'accessibilità, pubblicare sul sito una dichiarazione di accessibilità che descriva la politica relativa all'accessibilità e le azioni intraprese per rendere il sito accessibile. Se qualcuno segnala un problema di accessibilità nel sito, avviare un dialogo, mostrare empatia e adottare misure ragionevoli per cercare di risolvere il problema.

In sintesi:

- Considerare l'accessibilità fin dall'inizio di un progetto ed eseguire test tempestivamente e spesso. Come per qualsiasi altro bug, un problema di accessibilità diventa più costoso da risolvere quanto più tardi viene scoperto.
- Tenere presente che molte best practice di accessibilità apportano benefici a tutti, non solo agli utenti con disabilità. Ad esempio, un markup semantico snello non è solo utile per gli screen reader, ma è anche veloce da caricare e performante. Questo apporta benefici a tutti, soprattutto a chi utilizza dispositivi mobili e/o connessioni lente.
- Pubblicare una dichiarazione di accessibilità sul sito e interagire con le persone che riscontrano problemi.

## Linee guida sull'accessibilità e legge

Sono disponibili numerose checklist e insiemi di linee guida su cui basare i test di accessibilità, il che può sembrare inizialmente scoraggiante. Il consiglio è di familiarizzare con le aree fondamentali in cui occorre prestare attenzione, nonché di comprendere le strutture di alto livello delle linee guida più rilevanti.

- Per iniziare, il W3C ha pubblicato un documento ampio e molto dettagliato che include criteri molto precisi e indipendenti dalla tecnologia per la conformità all'accessibilità. Questi criteri sono chiamati [Web Content Accessibility Guidelines](https://www.w3.org/WAI/standards-guidelines/wcag/) (WCAG), e non sono affatto una lettura breve. I criteri sono suddivisi in quattro categorie principali, che specificano come le implementazioni possano essere rese percepibili, utilizzabili, comprensibili e robuste. Il posto migliore per avere una breve introduzione e iniziare a imparare è [WCAG at a Glance](https://www.w3.org/WAI/standards-guidelines/wcag/glance/). Non è necessario imparare tutti i criteri WCAG: è sufficiente conoscere le principali aree di interesse e utilizzare una varietà di tecniche e strumenti per evidenziare le aree che non soddisfano i criteri WCAG; vedere sotto per ulteriori informazioni.
- Il paese potrebbe anche disporre di legislazione specifica che regola la necessità che i siti web rivolti alla propria popolazione siano accessibili, ad esempio [EN 301 549](https://www.etsi.org/deliver/etsi_en/301500_301599/301549/02.01.02_60/en_301549v020102p.pdf) nell'UE, [Section 508 of the Rehabilitation Act](https://www.section508.gov/training/) negli Stati Uniti, [Federal Ordinance on Barrier-Free Information Technology](https://www.aktion-mensch.de/inklusion/barrierefreiheit/barrierefreie-website) in Germania, le [Accessibility Regulations 2018](https://www.legislation.gov.uk/uksi/2018/952/introduction/made) nel Regno Unito, [Accessibilità](https://www.agid.gov.it/it/ambiti-intervento/accessibilita-usabilita) in Italia, il [Disability Discrimination Act](https://humanrights.gov.au/resource-hub/by-resource-type/guidelines-and-standards/guides-and-standards-disability-rights/guidelines-equal-access-digital-goods-and-services) in Australia e così via. Il W3C mantiene un elenco delle [leggi e politiche sull'accessibilità web](https://www.w3.org/WAI/policies/) per paese.

Pertanto, mentre le WCAG sono un insieme di linee guida, il paese avrà probabilmente leggi che regolano l'accessibilità web, o almeno l'accessibilità dei servizi disponibili al pubblico, che potrebbero includere siti web, televisione, spazi fisici e così via. È una buona idea informarsi sulle leggi applicabili. Se non viene fatto alcuno sforzo per verificare che i contenuti siano accessibili, potrebbe sorgere una responsabilità legale in caso di reclamo da parte delle persone.

Questo può sembrare serio, ma in realtà è sufficiente considerare l'accessibilità come una priorità principale nelle pratiche di sviluppo web, come descritto sopra. In caso di dubbi, richiedere consulenza a un avvocato qualificato. Non verranno forniti ulteriori consigli oltre a questo, poiché non siamo avvocati.

## Accessibility API

I browser web usano speciali **accessibility API** — fornite dal sistema operativo sottostante — che espongono informazioni utili per le tecnologie assistive (AT). Le AT tendono principalmente a usare informazioni semantiche, quindi tali informazioni non includono elementi come le informazioni di stile o JavaScript. Queste informazioni sono strutturate in un albero informativo chiamato **accessibility tree**.

Sistemi operativi diversi dispongono di accessibility API diverse:

- Windows: MSAA/IAccessible, UIAExpress, IAccessible2
- macOS: NSAccessibility
- Linux: AT-SPI
- Android: Accessibility framework
- iOS: UIAccessibility

Quando le informazioni semantiche native fornite dagli elementi HTML nelle app web non sono sufficienti, è possibile integrarle con funzionalità della [specifica WAI-ARIA](https://w3c.github.io/aria/), che aggiungono informazioni semantiche all'accessibility tree per migliorare l'accessibilità. È possibile approfondire WAI-ARIA nell'articolo [Fondamenti di WAI-ARIA](/it/docs/Learn_web_development/Core/Accessibility/WAI-ARIA_basics).

## Riepilogo

Questo articolo dovrebbe aver fornito una panoramica generale utile dell'accessibilità, mostrato perché è importante e illustrato come integrarla nel flusso di lavoro. A questo punto dovrebbe esserci anche il desiderio di imparare i dettagli di implementazione che possono rendere accessibili i siti e quali strumenti possono aiutare. Nel prossimo articolo verranno esaminati gli strumenti per l'accessibilità.

## Vedere anche

- [WCAG](/it/docs/Web/Accessibility/Guides/Understanding_WCAG)
  - [Percepibile](/it/docs/Web/Accessibility/Guides/Understanding_WCAG/Perceivable)
  - [Utilizzabile](/it/docs/Web/Accessibility/Guides/Understanding_WCAG/Operable)
  - [Comprensibile](/it/docs/Web/Accessibility/Guides/Understanding_WCAG/Understandable)
  - [Robusto](/it/docs/Web/Accessibility/Guides/Understanding_WCAG/Robust)

- [Google Chrome ha rilasciato un'estensione per i sottotitoli automatici](https://blog.google/products-and-platforms/products/chrome/live-caption-chrome/)

{{NextMenu("Learn_web_development/Core/Accessibility/Tooling", "Learn_web_development/Core/Accessibility")}}
