---
title: Che cos'è l'accessibilità?
slug: Learn_web_development/Core/Accessibility/What_is_accessibility
l10n:
  sourceCommit: eaec5c4226ac64696a95314a7bce995165a4d124
---

{{NextMenu("Learn_web_development/Core/Accessibility/Tooling", "Learn_web_development/Core/Accessibility")}}

Questo articolo inizia il modulo con un buon approfondimento su cosa sia l'accessibilità: questa panoramica include quali gruppi di persone dobbiamo considerare e perché, quali strumenti diverse persone usano per interagire con il web e come possiamo rendere l'accessibilità parte integrante del nostro workflow di sviluppo web.

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
          <li>Il punto dell'accessibilità: maggiore accesso ai servizi digitali per coloro che hanno esigenze aggiuntive, miglioramento dell'usabilità per tutti, migliore SEO e un pubblico target più ampio.</li>
          <li>Consapevolezza dei requisiti legali dell'accessibilità.</li>
          <li>L'accessibilità dovrebbe essere considerata dall'inizio di un progetto, e non aggiunta alla fine.</li>
          <li>Familiarità con i criteri di conformità delle Web Content Accessibility Guidelines (WCAG).</li>
          <li>Consapevolezza delle API di accessibilità e del loro scopo.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Cos'è quindi l'accessibilità?

L'accessibilità è la pratica di rendere i tuoi siti web utilizzabili dal maggior numero possibile di persone. Tradizionalmente pensiamo a questo come un tema legato alle persone con disabilità, ma praticare l'accessibilità porta benefici anche ad altri gruppi, come chi usa dispositivi mobili o chi ha connessioni di rete lente.

Si può anche considerare l'accessibilità come un modo per trattare tutti allo stesso modo, offrendo loro pari opportunità, indipendentemente dalla loro abilità o circostanze. Così come è sbagliato escludere qualcuno da un edificio fisico perché si trova su una sedia a rotelle (gli edifici pubblici moderni hanno generalmente rampe o ascensori), è altrettanto sbagliato escludere qualcuno da un sito web perché ha un'imperfezione visiva. Siamo tutti diversi, ma siamo tutti umani e quindi abbiamo gli stessi diritti umani.

L'accessibilità è la scelta giusta. Fornire siti accessibili è previsto dalla legge in alcuni paesi, il che può aprire mercati significativi che altrimenti non potrebbero utilizzare i tuoi servizi o acquistare i tuoi prodotti.

Costruire siti accessibili porta beneficio a tutti:

- L'HTML semantico che migliora l'accessibilità migliora anche la SEO, rendendo il tuo sito più rintracciabile.
- Curare l'accessibilità dimostra buone etiche e valori morali, migliorando la tua immagine pubblica.
- Altre buone pratiche che migliorano l'accessibilità rendono anche il tuo sito più usabile da altri gruppi, come gli utenti di telefoni cellulari o coloro che hanno basse velocità di rete. In effetti, tutti possono trarre beneficio da molti di questi miglioramenti.
- Abbiamo anche menzionato che è la legge in alcuni luoghi?

## Che tipo di disabilità stiamo considerando?

Le persone con disabilità sono altrettanto diverse quanto le persone senza disabilità, e lo sono anche le loro disabilità. La lezione chiave qui è pensare oltre il proprio computer e il modo in cui si utilizza il web, iniziando a imparare come altri lo utilizzano — _tu non sei i tuoi utenti_. I principali tipi di disabilità da considerare sono descritti di seguito, insieme a eventuali strumenti speciali che usano per accedere ai contenuti web (conosciuti come **tecnologie assistive**, o **AT**).

> [!NOTE]
> La scheda informativa "[Disabilità e salute](https://www.who.int/en/news-room/fact-sheets/detail/disability-and-health)" dell'Organizzazione Mondiale della Sanità afferma che "Oltre un miliardo di persone, circa il 15% della popolazione mondiale, ha qualche forma di disabilità", e "Tra i 110 e i 190 milioni di adulti hanno difficoltà significative nel funzionamento."

### Persone con disabilità visive

Le persone con disabilità visive includono persone cieche, con visione limitata e daltoniche. Molte persone con disabilità visive utilizzano ingranditori dello schermo, che possono essere ingranditori fisici o capacità di zoom del software. La maggior parte dei browser e dei sistemi operativi oggi dispone di capacità di zoom. Alcuni utenti si affidano a screen reader, software che leggono ad alta voce il testo digitale. Alcuni esempi di screen reader includono:

- Prodotti commerciali a pagamento, come [JAWS](https://www.freedomscientific.com/Products/software/JAWS/) (Windows) e [Dolphin Screen Reader](https://yourdolphin.com/ScreenReader) (Windows).
- Prodotti gratuiti, come [NVDA](https://www.nvaccess.org/) (Windows), [ChromeVox](https://support.google.com/chromebook/answer/7031755) (Chrome) e [Orca](https://wiki.gnome.org/Projects/Orca) (Linux).
- Software incorporato nel sistema operativo, come [VoiceOver](https://www.apple.com/accessibility/features/?vision) (macOS, iPadOS, iOS), [Narrator](https://support.microsoft.com/en-us/windows/complete-guide-to-narrator-e4397a0d-ef4f-b386-d8ae-c172f109bdb1) (Windows), [ChromeVox](https://support.google.com/chromebook/answer/7031755) (su ChromeOS) e [TalkBack](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback) (Android).

È una buona idea familiarizzare con gli screen reader; dovresti anche impostare uno screen reader e sperimentarlo per capire come funziona. Consulta i nostri [tutorial sugli screen reader](/it/docs/Learn_web_development/Core/Accessibility/Tooling#screen_readers) per maggiori dettagli sull'utilizzo. Il video qui sotto fornisce anche un breve esempio di come è l'esperienza.

{{EmbedYouTube("IK97XMibEws")}}

In termini di statistiche, l'Organizzazione Mondiale della Sanità stima che "285 milioni di persone siano stimate come ipovedenti in tutto il mondo: 39 milioni sono ciechi e 246 milioni hanno una visione ridotta". (vedi [Minorazioni visive e cecità](https://www.who.int/en/news-room/fact-sheets/detail/blindness-and-visual-impairment)). Si tratta di una popolazione di utenti molto vasta e significativa da perdere semplicemente perché il tuo sito non è correttamente codificato — quasi la stessa dimensione della popolazione degli Stati Uniti d'America.

### Persone con disabilità uditive

Le persone [sorde e con problemi di udito (DHH)](https://www.nad.org/resources/american-sign-language/community-and-culture-frequently-asked-questions/) hanno vari livelli di perdita uditiva che vanno da lieve a profonda. Sebbene alcuni usino AT (vedi [Assistive Devices for People with Hearing, Voice, Speech, or Language Disorders](https://www.nidcd.nih.gov/health/assistive-devices-people-hearing-voice-speech-or-language-disorders)), non sono diffusi.

Per garantire l'accesso, devono essere fornite alternative testuali. I video dovrebbero essere sottotitolati manualmente e dovrebbero essere forniti trascrizioni per il contenuto audio. Inoltre, a causa di alti livelli di [privazione linguistica](https://epicspecialeducationstaffing.com/language-deprivation/#:~:text=Language%20deprivation%20is%20the%20term,therefore%20not%20exposed%20to%20language.) nelle popolazioni DHH, [la semplificazione del testo dovrebbe essere considerata](https://circlcenter.org/collaborative-research-automatic-text-simplification-and-reading-assistance-to-support-self-directed-learning-by-deaf-and-hard-of-hearing-computing-workers/).

Le persone sorde e con problemi di udito rappresentano anche una significativa base di utenti — "466 milioni di persone nel mondo hanno una perdita uditiva disabilitante", afferma la scheda informativa dell'Organizzazione Mondiale della Sanità "[Sordità e perdita uditiva](https://www.who.int/en/news-room/fact-sheets/detail/deafness-and-hearing-loss)".

### Persone con disabilità motorie

Queste persone hanno disabilità relative al movimento, che potrebbero comportare problemi puramente fisici (come la perdita di un arto o la paralisi) o disturbi neurologici/genetici che portano a debolezza o perdita del controllo degli arti. Alcune persone potrebbero avere difficoltà a compiere esattamente i movimenti delle mani necessari per usare un mouse, mentre altre potrebbero essere più gravemente colpite, forse essendo significativamente paralizzate al punto di dover usare un [puntatore a testa](https://www.performancehealth.com/adjustable-headpointer) per interagire con i computer.

Questo tipo di disabilità può anche essere il risultato dell'invecchiamento, piuttosto che di un trauma o condizione specifica, e potrebbe anche derivare da limitazioni hardware — alcuni utenti potrebbero non avere un mouse.

Il modo in cui questo di solito influisce sul lavoro di sviluppo web è il requisito che i controlli siano accessibili tramite la tastiera — discuteremo dell'accessibilità della tastiera in articoli successivi nel modulo, ma è una buona idea provare ad usare alcuni siti web utilizzando solo la tastiera per vedere come ci si trova. Puoi usare il tasto Tab per muoverti tra i diversi controlli di un modulo web, ad esempio? Puoi trovare maggiori dettagli sui controlli della tastiera nella nostra sezione [Usa controlli UI semantici dove possibile](/it/docs/Learn_web_development/Core/Accessibility/HTML#use_semantic_ui_controls_where_possible).

In termini di statistiche, un numero significativo di persone ha disabilità motorie. I Centri per il controllo e la prevenzione delle malattie degli Stati Uniti [Disabilità e Funzionamento (Adulti non istituzionalizzati dai 18 anni in su)](https://www.cdc.gov/nchs/fastats/disability.htm) riportano che negli USA "la percentuale di adulti con difficoltà fisica funzionale è del 16,1%".

### Persone con disabilità cognitive

L'impairment cognitivo si riferisce a un'ampia gamma di disabilità, dalle persone con disabilità intellettive che hanno le capacità più limitate, fino a tutti noi che invecchiamo e abbiamo difficoltà a pensare e ricordare. Il range include persone con malattie mentali, come la [depressione](https://www.nimh.nih.gov/health/topics/depression) e la [schizofrenia](https://www.nimh.nih.gov/health/topics/schizophrenia). Include anche persone con disabilità di apprendimento, come la [dislessia](https://www.nichd.nih.gov/health/topics/learningdisabilities) e il [disturbo da deficit di attenzione e iperattività](https://www.nimh.nih.gov/health/topics/attention-deficit-hyperactivity-disorder-adhd). Importante è che, sebbene ci sia molta diversità nelle definizioni cliniche delle disabilità cognitive, le persone con tale disabilità affrontano una comune serie di problemi funzionali. Questi includono difficoltà nella comprensione dei contenuti, difficoltà a ricordare come completare i compiti e confusione causata da layout incoerenti della pagina web.

Una buona base di accessibilità per le persone con disabilità cognitive include:

- Fornire contenuti in più di un modo, ad esempio tramite testo-audio o video.
- Contenuti facilmente comprensibili, come testi scritti utilizzando standard di linguaggio semplice.
- Concentrare l'attenzione su contenuti importanti.
- Minimizzare le distrazioni, come contenuti o annunci non necessari.
- Layout e navigazione della pagina web coerenti.
- Elementi famigliari, come collegamenti sottolineati blu quando non visitati e viola quando visitati.
- Dividere i processi in passi logici ed essenziali con indicatori di progresso.
- Rendere l'autenticazione del sito web il più semplice possibile senza compromettere la sicurezza.
- Rendere i moduli facili da completare, ad esempio con messaggi di errore chiari e un recupero degli errori semplice.

### Note

- Progettare con [accessibilità cognitiva](/it/docs/Web/Accessibility/Guides/Cognitive_accessibility) porterà a buone pratiche di design. Beneficeranno tutti.
- Molte persone con disabilità cognitive hanno anche disabilità fisiche. I siti web devono conformarsi alle [Web Content Accessibility Guidelines](https://www.w3.org/WAI/standards-guidelines/wcag/) del W3C, comprese le [linee guida per l'accessibilità cognitiva](/it/docs/Web/Accessibility/Guides/Cognitive_accessibility#wcag_guidelines).
- Il [Cognitive and Learning Disabilities Accessibility Task Force](https://www.w3.org/WAI/GL/task-forces/coga/) del W3C produce linee guida sull'accessibilità web per le persone con disabilità cognitive.
- WebAIM ha una pagina [Cognitive](https://webaim.org/articles/cognitive/) con informazioni e risorse pertinenti.
- I Centri per il controllo e la prevenzione delle malattie degli Stati Uniti stimano che, a partire dal 2018, 1 su 4 cittadini statunitensi abbia una disabilità e, tra questi, [l'impairment cognitivo è il più comune per i giovani](https://archive.cdc.gov/www_cdc_gov/media/releases/2018/p0816-disability.html).
- Negli Stati Uniti, alcune disabilità intellettive sono storicamente state indicate come "ritardo mentale". Molti ora considerano questo termine dispregiativo, quindi il suo uso dovrebbe essere evitato.
- Nel Regno Unito, alcune disabilità intellettive sono indicate come "disabilità di apprendimento" o "difficoltà di apprendimento".

## Implementare l'accessibilità nel tuo progetto

Un mito comune sull'accessibilità è che sia un "extra costoso" da implementare in un progetto. Questo mito in realtà _può_ essere vero se:

- Stai cercando di "adattare" l'accessibilità a un sito web esistente che ha problemi significativi di accessibilità.
- Hai iniziato a considerare l'accessibilità e scoperto problemi correlati nelle fasi tardive di un progetto.

Se invece consideri l'accessibilità dall'inizio di un progetto, il costo di rendere la maggior parte dei contenuti accessibili dovrebbe essere relativamente minimo.

Quando pianifichi il tuo progetto, inserisci i test di accessibilità nel tuo regime di test, proprio come fai per qualsiasi altra importante segmento del pubblico target (ad esempio i browser desktop o mobili target). Testa in anticipo e spesso, eseguendo idealmente collaudo automatici per rilevare funzionalità mancanti rilevabili programmaticamente (come il testo alternativo mancante delle immagini o un testo di collegamento errato — vedi [Usa etichette di testo significative](/it/docs/Learn_web_development/Core/Accessibility/HTML#use_meaningful_text_labels)) e fai alcuni test con gruppi di utenti disabili per vedere quanto bene funzionano le funzionalità del sito più complesse per loro. Ad esempio:

- Il mio widget di selezione della data è utilizzabile da persone che usano screen reader?
- Se i contenuti si aggiornano dinamicamente, le persone ipovedenti lo sanno?
- I miei pulsanti UI sono accessibili sia agli utenti di tastiera che di interfaccia touch?

Puoi e dovresti tenere una nota delle potenziali aree problematiche nei tuoi contenuti che avranno bisogno di lavoro per essere resi accessibili, assicurarti che siano testati a fondo, e pensare a soluzioni/alternative. Il contenuto testuale (come vedrai nel prossimo articolo) è semplice, ma cosa succede con i tuoi contenuti multimediali e i tuoi grafici 3D elaborati? Dovresti guardare al tuo budget del progetto e pensare a quali soluzioni hai a disposizione per rendere tali contenuti accessibili. Trascrivere tutto il tuo contenuto multimediale è un'opzione che, pur essendo costosa, è possibile.

Inoltre, sii realistico. "Accessibilità al 100%" è un ideale irraggiungibile — troverai sempre qualche tipo di caso limite che comporta che un certo utente trovi difficile usare certi contenuti — ma dovresti fare il massimo possibile. Se stai progettando di includere un grafico a torta 3D realizzato con WebGL, potresti voler includere una tabella dati come rappresentazione alternativa accessibile dei dati. Oppure potresti decidere di includere solo la tabella ed eliminare il grafico a torta 3D — la tabella è accessibile a tutti, più veloce da codificare, meno dispendiosa in termini di CPU e più facile da mantenere.

D'altra parte, se stai lavorando su un sito di gallerie che mostra opere d'arte 3D interessanti, sarebbe irragionevole aspettarsi che ogni pezzo d'arte sia perfettamente accessibile alle persone ipovedenti, dato che è un mezzo completamente visivo.

Per mostrare che ti interessa l'accessibilità, pubblica una dichiarazione di accessibilità sul tuo sito che descriva quale sia la tua politica sull'accessibilità e quali passi hai intrapreso per rendere il sito accessibile. Se qualcuno ti segnala che il tuo sito presenta un problema di accessibilità, avvia un dialogo con loro, sii empatico e compi passi ragionevoli per cercare di risolvere il problema.

Per riassumere:

- Considera l'accessibilità dall'inizio di un progetto e testa in anticipo e spesso. Proprio come qualsiasi altro bug, un problema di accessibilità diventa più costoso da risolvere quanto più tardi viene scoperto.
- Tieni presente che molte delle migliori pratiche di accessibilità beneficiano tutti, non solo gli utenti con disabilità. Ad esempio, il markup semantico ha un alto valore per i lettori di schermo, è veloce da caricare e performante, beneficiando tutti, in particolare coloro che utilizzano dispositivi mobili e/o hanno connessioni lente.
- Pubblica una dichiarazione di accessibilità sul tuo sito e interagisci con le persone che segnalano problemi.

## Linee guida per l'accessibilità e la legge

Esistono numerose checklist e serie di linee guida disponibili su cui basare i test di accessibilità, che potrebbero sembrare schiaccianti a prima vista. Il nostro consiglio è di familiarizzare con le aree di base in cui devi fare attenzione e di comprendere le strutture a livello elevato delle linee guida che sono più rilevanti per te.

- Per cominciare, il W3C ha pubblicato un documento ampio e dettagliato che include criteri precisi e agnostici rispetto alla tecnologia per la conformità all'accessibilità. Si tratta delle [Web Content Accessibility Guidelines](https://www.w3.org/WAI/standards-guidelines/wcag/) (WCAG), e non sono certo una lettura breve. I criteri sono suddivisi in quattro categorie principali, che specificano come le implementazioni possano essere rese percepibili, operabili, comprensibili e robuste. Il posto migliore per avere una leggera introduzione e iniziare a imparare è [WCAG in breve](https://www.w3.org/WAI/standards-guidelines/wcag/glance/). Non è necessario imparare tutti i criteri delle WCAG — sii consapevole delle aree principali di interesse e usa una varietà di tecniche e strumenti per evidenziare eventuali aree che non soddisfano i criteri delle WCAG (vedi sotto per maggiori dettagli).
- Il tuo paese potrebbe avere una legislazione specifica che governa la necessità che i siti web che servono la loro popolazione siano accessibili — per esempio [EN 301 549](https://www.etsi.org/deliver/etsi_en/301500_301599/301549/02.01.02_60/en_301549v020102p.pdf) nell'UE, [Section 508 of the Rehabilitation Act](https://www.section508.gov/training/) negli Stati Uniti, [Federal Ordinance on Barrier-Free Information Technology](https://www.aktion-mensch.de/inklusion/barrierefreiheit/barrierefreie-website) in Germania, le [Regolazioni sull'accessibilità 2018](https://www.legislation.gov.uk/uksi/2018/952/introduction/made) nel Regno Unito, [Accessibilità](https://www.agid.gov.it/it/ambiti-intervento/accessibilita-usabilita) in Italia, il [Disability Discrimination Act](https://humanrights.gov.au/our-work/disability-rights/publications/guidelines-equal-access-digital-goods-and-services) in Australia, ecc. Il W3C mantiene un elenco di [Leggi e politiche sull'accessibilità web](https://www.w3.org/WAI/policies/) per paese.

Quindi, mentre le WCAG sono un set di linee guida, il tuo paese probabilmente avrà leggi che governano l'accessibilità web, o almeno l'accessibilità dei servizi disponibili al pubblico (cosa che potrebbe includere siti web, televisione, spazi fisici, ecc.). È una buona idea scoprire quali sono le leggi in vigore. Se non fai alcun sforzo per verificare che il tuo contenuto sia accessibile, potresti essere legalmente responsabile se le persone si lamentano.

Questo suona serio, ma in realtà è sufficiente considerare l'accessibilità come la principale priorità delle tue pratiche di sviluppo web, come delineato sopra. In caso di dubbi, chiedi consiglio a un avvocato qualificato. Non intendiamo offrire ulteriori consigli di questo tipo, perché non siamo avvocati.

## API di accessibilità

I browser web utilizzano speciali **API di accessibilità** (fornite dal sistema operativo sottostante) che espongono informazioni utili per le tecnologie assistive (AT) — le AT tendono a utilizzare principalmente informazioni semantiche, quindi queste informazioni non includono cose come le informazioni di stile o JavaScript. Queste informazioni sono strutturate in un albero di informazioni chiamato **albero di accessibilità**.

Diversi sistemi operativi hanno diverse API di accessibilità disponibili:

- Windows: MSAA/IAccessible, UIAExpress, IAccessible2
- macOS: NSAccessibility
- Linux: AT-SPI
- Android: Framework di accessibilità
- iOS: UIAccessibility

Dove le informazioni semantiche native fornite dagli elementi HTML nelle tue applicazioni web falliscono, puoi integrarle con caratteristiche provenienti dalla [specifica WAI-ARIA](https://www.w3.org/TR/wai-aria/), che aggiungono informazioni semantiche all'albero di accessibilità per migliorare l'accessibilità. Puoi imparare molto di più su WAI-ARIA nel nostro articolo [Le basi di WAI-ARIA](/it/docs/Learn_web_development/Core/Accessibility/WAI-ARIA_basics).

## Sommario

Questo articolo dovrebbe averti fornito una panoramica utile a livello elevato dell'accessibilità, mostrandoti perché è importante e guardando come puoi integrarla nel tuo workflow. Dovresti ora anche avere la curiosità di sapere di più sui dettagli di implementazione che possono rendere i siti accessibili e quali strumenti possono aiutarti. Esamineremo gli strumenti di accessibilità nell'articolo successivo.

## Vedi anche

- [WCAG](/it/docs/Web/Accessibility/Guides/Understanding_WCAG)

  - [Percepibile](/it/docs/Web/Accessibility/Guides/Understanding_WCAG/Perceivable)
  - [Operabile](/it/docs/Web/Accessibility/Guides/Understanding_WCAG/Operable)
  - [Comprensibile](/it/docs/Web/Accessibility/Guides/Understanding_WCAG/Understandable)
  - [Robusto](/it/docs/Web/Accessibility/Guides/Understanding_WCAG/Robust)

- [Google ha rilasciato un'estensione per la creazione di sottotitoli automatici](https://blog.google/products/chrome/live-caption-chrome/)

{{NextMenu("Learn_web_development/Core/Accessibility/Tooling", "Learn_web_development/Core/Accessibility")}}
