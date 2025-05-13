---
title: Il modello degli standard web
slug: Learn_web_development/Getting_started/Web_standards/The_web_standards_model
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Getting_started/Web_standards/How_the_web_works", "Learn_web_development/Getting_started/Web_standards/How_browsers_load_websites", "Learn_web_development/Getting_started/Web_standards")}}

Questo articolo fornisce alcune informazioni di base utili sul web e sugli standard web: come sono nati, cosa sono le tecnologie degli standard web e come funzionano insieme.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Conoscenza di base del sistema operativo del proprio computer, dei browser web e delle tecnologie web.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>Standard web e i principi chiave su cui si basano.</li>
          <li>Come operano gli organismi di standardizzazione — ad esempio il <a href="https://www.w3.org/">W3C</a>, <a href="https://whatwg.org/">WHATWG</a>, <a href="https://tc39.es/">TC39</a> e il <a href="https://www.khronos.org/">Gruppo Khronos</a>; il processo di creazione degli standard.</li>
          <li>Le principali tecnologie degli standard web e come lavorano insieme.</li>
          <li>File lato server (dinamici) rispetto a file lato client (statici).</li>
          <li>Le migliori pratiche web.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Breve storia del web

Alla fine degli anni ‘60, l’esercito statunitense sviluppò una rete di comunicazione chiamata {{Glossary("Arpanet", "ARPANET")}}. Questa può essere considerata un precursore di **internet**, poiché operava sul [commutazione di pacchetto](https://en.wikipedia.org/wiki/Packet_switching), e presentava la prima implementazione del suite di protocolli [TCP/IP](https://en.wikipedia.org/wiki/Internet_protocol_suite). Queste due tecnologie costituiscono la base dell'infrastruttura su cui è costruita internet.

Nel 1980, [Tim Berners-Lee](https://en.wikipedia.org/wiki/Tim_Berners-Lee) (spesso indicato come TimBL) scrisse un programma di notebook chiamato ENQUIRE, che presentava il concetto di collegamenti tra diversi nodi. Suona familiare?

Avanti veloce al 1989, e TimBL ha scritto [Information Management: A Proposal](https://www.w3.org/History/1989/proposal.html) e HyperText al CERN; queste due pubblicazioni insieme hanno fornito le basi per il funzionamento del web. Hanno ricevuto un notevole interesse, sufficiente per convincere i capi di TimBL a consentirgli di procedere e creare un sistema di ipertesto globale.

Entro il 1990-91, TimBL aveva creato tutto il necessario per eseguire la prima versione del **World Wide Web** (generalmente indicato come **web**) — [HTTP](/it/docs/Web/HTTP), [HTML](/it/docs/Web/HTML), il primo browser web, che si chiamava [WorldWideWeb](https://en.wikipedia.org/wiki/WorldWideWeb), un server web e alcune pagine web da visualizzare.

> [!NOTE]
> Le persone a volte usano "il web" e "internet" in modo intercambiabile, ma sono cose diverse. Internet è l'infrastruttura che permette il trasporto di informazioni in tutto il mondo tra diversi server e client, mentre il web è un sistema costruito sopra internet. Il web definisce i tipi di informazioni (contenuti e codice) che vengono trasportati tramite internet e i protocolli di comunicazione per gestire quel trasporto.

Nel 1994, TimBL ha fondato il [World Wide Web Consortium](https://en.wikipedia.org/wiki/World_Wide_Web_Consortium) (W3C), un'organizzazione che riunisce rappresentanti di molte aziende diverse per collaborare alla creazione di tecnologie web. Il W3C ha lavorato alla standardizzazione e al miglioramento delle tecnologie web esistenti come HTML e HTTP, e alla creazione di nuove tecnologie come [CSS](/it/docs/Web/CSS) e [JavaScript](/it/docs/Web/JavaScript). CSS e JavaScript in particolare erano essenziali per dare stilizzazione e interattività al web, facendolo sembrare più simile al web che conosciamo oggi.

Nei successivi anni, il web è esploso, con il rilascio di diversi browser, la creazione di migliaia di server web e la creazione di milioni di pagine web. Altri organismi di standardizzazione sono anche apparsi per aiutare a standardizzare diversi aspetti delle tecnologie web.

> [!NOTE]
> Se è interessato a leggere un resoconto più dettagliato della storia del web, provi a cercare "storia del web" nel suo motore di ricerca preferito e veda cosa trova.

## Standard web

Gli **standard web** sono le tecnologie che utilizziamo per costruire siti web. Questi standard esistono come lunghi documenti tecnici chiamati specifiche, che dettagliano esattamente come la tecnologia dovrebbe funzionare. Questi documenti non sono molto utili per imparare come utilizzare le tecnologie che descrivono (ecco perché abbiamo siti come MDN Web Docs). Invece, sono destinati a essere utilizzati dagli ingegneri del software per implementare queste tecnologie (di solito nei browser web).

### Organismi di standardizzazione e processi

Gli standard web sono creati da organismi di standardizzazione — istituzioni che invitano gruppi di persone di diverse aziende tecnologiche a riunirsi e concordare su come le tecnologie dovrebbero funzionare nel miglior modo per soddisfare tutti i loro casi d'uso.

Il W3C è il più noto organismo di standardizzazione web, ma ce ne sono altri. Ad esempio:

- [WHATWG](https://whatwg.org/) mantiene [HTML Living Standard](https://html.spec.whatwg.org/multipage/), che descrive esattamente come HTML (tutti gli elementi HTML, e le loro API associate, e altre tecnologie circostanti) dovrebbe essere implementato.
- [TC39](https://tc39.es/) e [ECMA](https://ecma-international.org/) specificano e pubblicano lo standard per ECMAScript, su cui si basa il moderno JavaScript.
- [Khronos](https://www.khronos.org/) pubblica tecnologie per la grafica 3D, come WebGL.

I processi completi con cui vengono creati gli standard possono diventare complessi e profondi. Tuttavia, a meno che non si voglia creare le proprie funzionalità tecnologiche web, non è necessario comprenderne la maggior parte. Se si desidera contribuire alla discussione sulle nuove tecnologie e fornire feedback, di solito è solo una questione di unirsi alla lista di distribuzione o ad un altro meccanismo di discussione pertinente. Le discussioni sugli standard si svolgono in pubblico, da cui il termine ["Standard aperti"](#open_standards).

Per ora, le forniamo una comprensione generale di alto livello di come funzionano i processi standard:

1. Qualcuno nota la necessità di una nuova funzionalità standard web che renderà più facile la vita degli sviluppatori. Ad esempio, potrebbe esserci un modello comune comunemente utilizzato nelle interfacce utente web, ma è difficile da implementare. Una funzionalità CSS dedicata lo renderebbe molto più semplice. Quel qualcuno potrebbe essere chiunque — un singolo sviluppatore, o un ingegnere che lavora per una grande azienda tecnologica.
2. La persona discute questa funzionalità con altri sviluppatori, ingegneri del browser, ecc., e inizia a creare interesse nell'implementazione della funzionalità. Di solito scrivono un documento esplicativo che spiega la necessità della funzionalità e come funzionerà, e una dimostrazione del codice che mostra come apparirebbe la funzionalità in azione.
3. Se c'è abbastanza interesse nella funzionalità, viene discussa formalmente all'interno del gruppo di lavoro dell'organismo di standardizzazione pertinente. Ad esempio, le funzionalità CSS sono di solito discusse dal [Gruppo di lavoro CSS](https://www.w3.org/groups/wg/css/) (WG) (vedere anche la [pagina Wikipedia del Gruppo di lavoro CSS](https://en.wikipedia.org/wiki/CSS_Working_Group) per una descrizione e una storia più dettagliata). Prima che una nuova tecnologia web sia accettata, deve essere valutata rigorosamente per assicurarsi che sia buona per il web — ad esempio, non introduce problemi di sicurezza, è [accessibile e compatibile](#accessibile_e_interoperabile) con altre tecnologie web, e non si basa su brevetti.
4. Per provare la funzionalità, accadono diverse cose. Questi punti possono accadere intorno al momento del Punto 3., o anche prima (i fornitori di browser a volte implementano funzionalità non standard/proprietarie e poi tentano di standardizzarle successivamente):

   1. Uno o più fornitori di browser implementeranno una versione sperimentale della nuova funzionalità, spesso disabilitata per impostazione predefinita, ma che può essere abilitata da persone che vogliono testarla e fornire feedback.
   2. Un membro del gruppo di lavoro la aggiungerà anche a una specifica tecnologica in modo che i fornitori di browser possano implementarla in modo coerente.
   3. Cercheranno anche feedback da altri fornitori di browser per vedere quali problemi hanno con la proposta e quanto è probabile che la implementino. Questi sono chiamati Posizioni sugli standard. Vedere ad esempio [Posizioni sugli standard Mozilla](https://mozilla.github.io/standards-positions/).
   4. Le persone coinvolte scriveranno anche una suite di test estesa per dimostrare che la funzionalità funziona come descritto.

5. Infine, se tutto va bene, la funzionalità sarà implementata su tutti i browser e potrà iniziare a essere utilizzata nella creazione di siti web.

> [!NOTE]
> È perfettamente possibile che le persone che suggeriscono la funzionalità, implementandola in un browser, creando la specifica, scrivendo test e raccogliendo feedback su di essa, siano la stessa persona/persone.

Potete trovare ulteriori informazioni su processi di specifici organismi di standardizzazione. Vedere ad esempio:

- [Documento del Processo W3C](https://www.w3.org/policies/process/)
- [WHATWG — Modalità di Lavoro](https://whatwg.org/working-mode)
- [Il Processo TC39](https://tc39.es/process-document/)

## Principi chiave degli standard web

I principi chiave del web, che rendono il web un'industria unica ed emozionante in cui coinvolgersi, sono i seguenti:

- Aperto al contributo e all'uso e quindi non soggetto a brevetti o controllato da una singola entità privata.
- Accessibile e interoperabile.
- Non interrompono il web.

Osserviamo ciascuno di questi in un po' più di dettaglio.

### Standard "Aperti"

Uno degli aspetti chiave degli standard web, su cui TimBL e il W3C hanno concordato fin dall'inizio, è che il web (e le tecnologie web) dovrebbero essere **aperti**. Ciò significa che sono liberi sia di contribuire che di utilizzare, e non gravati da brevetti/licenze. Questo è importante: se una tecnologia web si basa su tecnologie brevettate/licenziate per funzionare, il proprietario del brevetto può quindi addebitare ai fornitori di browser implementatori potenzialmente grandi somme di denaro, e quei costi verrebbero poi trasferiti agli utenti del browser.

Inoltre, poiché le tecnologie web sono create apertamente, in collaborazione tra molte aziende diverse, significa che nessuna azienda ha il controllo su di esse, il che è una cosa davvero buona. Non si vorrebbe che un'unica azienda decidesse improvvisamente di mettere tutto il web dietro un paywall, o rilasciasse una nuova versione di HTML che tutti devono acquistare per continuare a fare siti web, o peggio ancora, decidesse che non sono più interessati e la spegnesse.

Gli standard aperti consentono al web di rimanere una risorsa pubblica liberamente disponibile, dove chiunque può scrivere il codice per costruire un sito web gratuitamente e chiunque può contribuire al processo di creazione degli standard.

### Accessibile e interoperabile

Il web e i browser web sono fondamentalmente progettati affinché i contenuti web siano **accessibili** a persone con disabilità. Era originariamente concepito come un grande livellatore, consentendo alle persone di accedere alle informazioni indipendentemente dalle circostanze. Ciò significa che, ad esempio:

- Le persone che non sono in grado di utilizzare un mouse o un dispositivo di puntamento possono utilizzare la tastiera per navigare nel web.
- Le persone ipovedenti possono ingrandire i contenuti o utilizzare un programma chiamato **screen reader** per leggere il contenuto a loro e descrivere i controlli in un modo che abbia senso.

> [!NOTE]
> Imparerà di più sull'[Accessibilità](/it/docs/Learn_web_development/Core/Accessibility) più avanti nel percorso di apprendimento.

Inoltre, le tecnologie web sono progettate per essere **interoperabili**. Poiché le tecnologie web sono implementate secondo standard pubblicati, i browser dovrebbero fornire la stessa output reso per un determinato input (ad esempio, codice HTML, CSS o JS) — in altre parole, un sito web dovrebbe funzionare in modo coerente su più browser.

### Non interrompere il web

Un'altra frase che sentirà sugli standard web aperti è "non interrompere il web". L'idea alla base di questo è che qualsiasi nuova tecnologia web dovrebbe essere compatibile all'indietro con ciò che l'ha preceduto, in modo che i siti web esistenti continuino a funzionare allo stesso modo di prima.

I fornitori di browser web dovrebbero essere in grado di implementare nuove tecnologie web senza causare una differenza nel rendering o nella funzionalità che indurrebbe i loro utenti a pensare che un sito web sia rotto e provare un altro browser come risultato.

## Panoramica delle moderne tecnologie web

Ci sono un certo numero di tecnologie da imparare se desidera diventare un sviluppatore web front-end. In questa sezione le descriveremo brevemente.

### HTML, CSS e JavaScript

[HTML](/it/docs/Web/HTML), [CSS](/it/docs/Web/CSS) e [JavaScript](/it/docs/Web/JavaScript) sono le tre principali tecnologie che utilizzerà per costruire un sito web.

- HTML è per la struttura e la semantica (significato).
- CSS è per lo stile e il layout.
- JavaScript e le API sono per controllare il comportamento dinamico.

#### HTML

HyperText Markup Language, o **HTML**, è un linguaggio di markup costituito da diversi elementi che può avvolgere (segnare) contenuti per dare loro significato (semantica) e struttura. HTML semplice appare così:

```html
<h1>This is a top-level heading</h1>

<p>This is a paragraph of text.</p>

<img src="cat.jpg" alt="A picture of my cat" />
```

Se adottiamo un'analogia di costruzione di case, HTML sarebbe come le fondamenta e i muri della casa, che le danno struttura e la tengono insieme.

#### CSS

Cascading Style Sheets (**CSS**) è un linguaggio basato su regole utilizzato per applicare stili al suo HTML — ad esempio, impostare i colori del testo e dello sfondo, aggiungere bordi, animare cose o disporre una pagina in un determinato modo. Come esempio semplice, il seguente codice renderebbe rossi tutti i paragrafi HTML:

```css
p {
  color: red;
}
```

Nell'analogia della casa, il CSS è come la vernice, la carta da parati, i tappeti e i dipinti che utilizzerebbe per rendere la casa bella.

#### JavaScript (e API)

**JavaScript** è il linguaggio di programmazione che usiamo per aggiungere interattività ai siti web, dal cambio di stile dinamico al recupero di aggiornamenti dal server, fino a grafica 3D complessa. Il seguente semplice JavaScript memorizza un riferimento a un paragrafo in memoria e cambia il testo al suo interno:

```js
let pElem = document.querySelector("p");
pElem.textContent = "We changed the text!";
```

Sentirà anche il termine **API** insieme a JavaScript. API sta per **Application Programming Interface**. In termini generali, un'API è un pezzo di codice che le consente di controllare altri pezzi di codice più complessi o altre funzionalità sul suo computer (come dispositivi hardware come la sua webcam o il suo microfono) in un modo gestibile.

Ad esempio, scrivere la sua interfaccia per comunicare con la sua webcam e catturare un flusso video da essa sarebbe piuttosto difficile, ma il metodo API JavaScript [`getUserMedia()`](/it/docs/Web/API/MediaDevices/getUserMedia#examples) le permette di farlo facilmente. Fa tutto il lavoro difficile per lei, dietro le quinte, in modo che non debba reinventare la ruota ogni volta.

Il semplice frammento di codice sopra utilizza anche un'API. [`querySelector()`](/it/docs/Web/API/Document/querySelector) e [`textContent`](/it/docs/Web/API/Node/textContent) fanno entrambe parte della famiglia di API del [Document Object Model (DOM)](/it/docs/Learn_web_development/Core/Scripting/DOM_scripting), che le permettono di usare JavaScript per manipolare documenti web.

Nell'analogia della casa, JavaScript è come la cucina, la TV, il microonde o l'asciugacapelli: le cose che danno alla sua casa una funzionalità utile.

### Altre tecnologie web

Esistono altre tecnologie utilizzate sul web, ad esempio:

- [HTTP](/it/docs/Web/HTTP) per la comunicazione tra client e server, come menzionato in precedenza.
- [SVG](/it/docs/Web/SVG) per creare e manipolare grafica vettoriale.
- [MathML](/it/docs/Web/MathML) per descrivere formule matematiche.

Tuttavia, HTML, CSS e JavaScript sono di gran lunga le tecnologie più importanti da apprendere, quindi ci concentreremo principalmente su quelle nel nostro percorso di apprendimento.

## Strumenti

Una volta appresi gli standard, le tecnologie fondamentali utilizzate per costruire pagine web (come HTML, CSS e JavaScript), presto incontrerà vari strumenti che possono essere utilizzati per facilitare o rendere più efficiente il suo lavoro. Gli esempi includono:

- [Strumenti per sviluppatori](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools) all'interno dei browser moderni che possono essere utilizzati per eseguire il debug del suo codice.
- [Strumenti di testing](/it/docs/Learn_web_development/Extensions/Testing) che possono essere utilizzati per eseguire test per mostrare se il suo codice si comporta come previsto.
- [Framework e librerie](/it/docs/Learn_web_development/Core/Frameworks_libraries) costruiti su JavaScript che le consentono di costruire determinati tipi di siti web molto più rapidamente ed efficacemente.
- Cosiddetti **Linters** e **formattatori**, che prendono un insieme di regole per lo stile di codifica, guardano il suo codice e aggiornano il suo codice per seguire quelle regole. Prettier, che lei [ha incontrato in precedenza nel corso](/it/docs/Learn_web_development/Getting_started/Environment_setup/Code_editors#enhancing_your_code_editor_with_extensions), è un esempio di formattatore.

## Linguaggi e framework lato server

HTML, CSS e JavaScript sono linguaggi lato front-end (o lato client), il che significa che vengono eseguiti dal browser per produrre un front-end di un sito web che i suoi utenti possono utilizzare.

Esiste un'altra classe di linguaggi chiamati linguaggi back-end (o lato server), il che significa che vengono eseguiti sul server prima che il risultato venga poi inviato al browser per essere visualizzato. Un uso tipico di un linguaggio lato server è ottenere dei dati da un database, generare qualche HTML per contenere i dati, quindi inviare l'HTML al browser per visualizzarlo all'utente.

Esempi di framework e linguaggi lato server includono ASP.NET (C#), Django (Python), Laravel (PHP) e Next.js (JavaScript).

Queste tecnologie non sono considerate "standard web" — sono sviluppate da organizzazioni al di fuori dei processi di standardizzazione web di organizzazioni come il W3C e WHATWG — sebbene alcune di esse abbiano processi altrettanto aperti.

### Statico rispetto a dinamico

Un altro modo in cui i linguaggi lato client e lato server vengono spesso descritti è **statico** e **dinamico**:

- Un file HTML semplice è memorizzato sul server. Quando viene richiesto, viene consegnato al client, invariato, e reso dal browser. Poiché non cambia, è riferito come "statico".
- Quando il codice lato server (ad esempio, uno script Python o una pagina ASP.NET) genera dell'HTML contenente dati e restituisce quell'HTML al client, il contenuto dell'HTML cambia a seconda di ciò che fa il codice lato server. È quindi riferito come "dinamico".

C'è spesso un po' di sovrapposizione tra i concetti di codice statico e dinamico. I linguaggi lato server di solito definiscono strutture HTML all'interno di un file modello, che tendono a essere principalmente HTML statico con alcune sezioni dinamiche speciali incluse che cambiano a seconda di quali dati devono essere inseriti.

## Migliori pratiche web

Abbiamo parlato brevemente delle tecnologie che utilizzerà per costruire siti web. Ora discutiamo delle migliori pratiche che gli sviluppatori web generalmente impiegano per assicurarsi che i loro siti web siano usabili dal maggior numero di persone possibile.

Quando si fa sviluppo web, la principale causa di incertezza proviene dal fatto che non si sa quale combinazione di tecnologia ogni utente utilizzerà per visualizzare il suo sito web:

- L'utente 1 potrebbe guardarlo su un iPhone, con uno schermo piccolo e stretto.
- L'utente 2 potrebbe guardarlo su un laptop Windows con un monitor widescreen collegato a esso.
- L'utente 3 potrebbe essere ipovedente e utilizzare un lettore di schermo per leggere e interagire con la pagina web.
- L'utente 4 potrebbe utilizzare una macchina desktop molto vecchia che non può eseguire browser moderni.

Poiché non si sa esattamente cosa utilizzeranno i propri utenti, è necessario progettare in modo difensivo — rendere il proprio sito web il più flessibile possibile, in modo che tutti gli utenti sopra menzionati possano usufruirne, anche se potrebbero non avere tutti la stessa esperienza.

Incontrerà i seguenti concetti a un certo punto nei suoi studi, che rappresentano le migliori pratiche a cui i suoi siti web dovrebbero idealmente aderire. Non si preoccupi troppo di questi per ora. Durante la maggior parte del corso cerchiamo di insegnare queste implicitamente, il che significa che quando le insegniamo HTML, CSS e JavaScript, i nostri esempi seguiranno le migliori pratiche laddove possibile. Più avanti nel suo percorso di apprendimento probabilmente esplorerà insegnamenti espliciti in questi ambiti.

- **Enhancement progressivo**
  - : Creare un'esperienza minima che fornisca la funzionalità essenziale a tutti gli utenti, e stratificare su un'esperienza migliore e altri miglioramenti nei browser che possono supportarli. L'enhancement progressivo è spesso visto come non importante, perché i browser tendono a supportare le nuove funzionalità in modo più coerente al giorno d'oggi, e le persone tendono ad avere connessioni internet più veloci con limiti più alti sul consumo di dati. Tuttavia, consideri esempi come tagliare sulla decorazione per rendere un'esperienza mobile più fluida e risparmiare sui dati o fornire un'esperienza più leggera e a basso traffico per gli utenti che pagano al megabyte o hanno connessioni misurate.
- **Compatibilità tra browser**
  - : Cercare di assicurarsi che la sua pagina web funzioni su quanti più dispositivi possibile. Ciò include l'utilizzo di tecnologie che tutti i browser supportano, offrendo esperienze migliori ai browser che possono gestirle (enhancement progressivo), e/o scrivendo codice in modo che cada su un'esperienza più semplice ma ancora utilizzabile nei browser più vecchi (definito **degradazione elegante**). Richiede anche test per vedere se qualcosa fallisce in certi browser, e poi più lavoro per correggere quei fallimenti.
- **Separazione dei livelli**
  - : Mettere il suo contenuto (HTML), lo stile (CSS) e il comportamento (JavaScript) in file di codice diversi, piuttosto che raggrupparli tutti insieme nello stesso posto. Questo è una buona idea per molti motivi, inclusa la gestione e la comprensione del codice e il lavoro di squadra/separazione dei ruoli. In realtà, la separazione non è sempre chiara. È un ideale da raggiungere ove possibile, piuttosto che un assoluto.
- **Design web responsive**
  - : Rendere la sua funzionalità e layout flessibili in modo che possano adattarsi automaticamente a diversi browser. Un esempio ovvio è un sito web che è disposto in un modo in un browser widescreen sul desktop, ma si visualizza come un layout più compatto, a colonna singola, su browser di telefoni cellulari. Provi a regolare la larghezza della finestra del suo browser ora, e veda cosa succede al layout del sito.
- **Prestazioni**
  - : Far caricarette i siti web il più rapidamente possibile, ma anche renderli intuitivi e facili da usare in modo che gli utenti non si frustrino e vadano da qualche altra parte.
- **Internazionalizzazione**
  - : Rendere i siti web usabili da persone di diverse culture, che parlano lingue diverse dalla sua. Ci sono considerazioni tecniche qui (come alterare il suo layout in modo che funzioni bene anche per lingue da destra a sinistra o dall'alto verso il basso), e umane (come usare un linguaggio semplice, non gergale in modo che culture diverse siano più propense a comprendere il suo testo).
- **Privacy** e **Sicurezza**
  - : Questi due concetti sono correlati ma diversi. La privacy si riferisce a consentire alle persone di svolgere i loro affari in modo privato e non spiarli o raccogliere più dei loro dati di quanto sia assolutamente necessario. La sicurezza si riferisce alla costruzione del suo sito web in un modo sicuro in modo che gli utenti malintenzionati non possano rubare le informazioni contenute su di esso da lei o dai suoi utenti.

{{PreviousMenuNext("Learn_web_development/Getting_started/Web_standards/How_the_web_works", "Learn_web_development/Getting_started/Web_standards/How_browsers_load_websites", "Learn_web_development/Getting_started/Web_standards")}}
