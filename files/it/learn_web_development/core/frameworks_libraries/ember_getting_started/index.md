---
title: Introduzione a Ember
slug: Learn_web_development/Core/Frameworks_libraries/Ember_getting_started
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Core/Frameworks_libraries/Ember_structure_componentization", "Learn_web_development/Core/Frameworks_libraries")}}

Nel nostro primo articolo su Ember esamineremo come funziona Ember e a cosa è utile, installeremo la toolchain Ember localmente, creeremo un'app di esempio, e faremo alcune configurazioni iniziali per prepararla allo sviluppo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          È consigliato avere familiarità con le basi dei linguaggi
          <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e avere conoscenze dell'
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminale/riga di comando</a
          >.
        </p>
        <p>
          Una comprensione più profonda delle funzionalità moderne di JavaScript (come classi, moduli, ecc.) sarà estremamente utile, poiché Ember ne fa largo uso.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Imparare come installare Ember e creare un'app iniziale.</td>
    </tr>
  </tbody>
</table>

## Introduzione a Ember

Ember è un framework di componenti-servizi che si concentra sull'esperienza complessiva di sviluppo delle applicazioni web, minimizzando le differenze banali tra le applicazioni — il tutto essendo uno strato moderno e leggero sopra JavaScript nativo. Ember ha anche una grande compatibilità retroattiva e futura per aiutare le aziende a rimanere aggiornate con le ultime versioni di Ember e le più recenti convenzioni guidate dalla comunità.

Cosa significa essere un framework di componenti-servizi? I componenti sono pacchetti individuali di comportamento, stile e markup — proprio come forniscono altri framework frontend, quali React, Vue e Angular. Il lato servizio fornisce stato condiviso a lungo termine, comportamento e un'interfaccia per integrare altre librerie o sistemi. Ad esempio, il Router (che verrà menzionato più avanti in questo tutorial) è un servizio. I componenti e i servizi costituiscono la maggior parte di qualsiasi applicazione EmberJS.

## Casi d'uso

Generalmente, EmberJS funziona bene per costruire app che desiderano una o entrambe le seguenti caratteristiche:

- Applicazioni a pagina singola, inclusi web app simili a quelle native e [progressive web apps](/it/docs/Web/Progressive_web_apps) (PWAs)

  - Ember funziona meglio quando è l'intero front-end della tua applicazione.

- Aumento della coesione tra molti stack tecnologici del team

  - Le "migliori pratiche" sostenute dalla comunità consentono una velocità di sviluppo a lungo termine più rapida.
  - Ember ha convenzioni chiare che sono utili per imporre coerenza e aiutare i membri del team a prendere rapidamente confidenza.

### Ember con add-on

EmberJS ha un'architettura a plugin, il che significa che gli add-on possono essere installati e fornire funzionalità aggiuntive senza molta, se non alcuna, configurazione.

Esempi includono:

- [PREmber](https://github.com/ef4/prember): Rendering di siti web statici per blog o contenuti di marketing.
- [empress-blog](https://empress-blog.netlify.app/welcome/): Scrittura di post di blog in markdown ottimizzando per la SEO con PREmber.
- [ember-service-worker](https://ember-service-worker.com/): Configurazione di una PWA in modo che l'app possa essere installata su dispositivi mobili, proprio come le app del rispettivo app-store del dispositivo.

### App mobili native

Ember può anche essere utilizzato con app mobili native con un bridge mobile-nativo per JavaScript, come quello fornito da [Corber](http://corber.io/).

## Opinioni

EmberJS è uno dei framework front-end più "opinionati" che ci siano. Ma cosa significa essere "opinionati"? In Ember, le opinioni sono un insieme di convenzioni che aiutano ad aumentare l'efficienza degli sviluppatori al costo di dover apprendere tali convenzioni. Man mano che le convenzioni vengono definite e condivise, le opinioni che supportano tali convenzioni aiutano a ridurre le differenze banali tra le app — un obiettivo comune tra tutti i framework "opinionati", in qualsiasi linguaggio ed ecosistema. Gli sviluppatori possono quindi passare più facilmente tra progetti e applicazioni senza dover riapprendere completamente l'architettura, i modelli, le convenzioni, ecc.

Mentre segue questa serie di tutorial, noterà le opinioni di Ember — come le rigide convenzioni di denominazione dei file dei componenti.

## Come si relaziona Ember al JavaScript puro?

Ember è costruito su tecnologie JavaScript ed è uno strato sottile sopra la tradizionale [programmazione orientata agli oggetti](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object-oriented_programming), consentendo al contempo agli sviluppatori di utilizzare [tecniche di programmazione funzionale](https://opensource.com/article/17/6/functional-javascript).

Ember utilizza due sintassi principali:

- JavaScript (o, facoltativamente, [TypeScript](https://www.typescriptlang.org/))
- Il linguaggio di template di Ember, che è vagamente basato su [Handlebars](https://handlebarsjs.com/guide/).

Il linguaggio di template è utilizzato per ottimizzare la build e il runtime che altrimenti non sarebbero possibili. Soprattutto, è un superset di HTML, il che significa che chiunque conosca HTML può dare contributi significativi a qualsiasi progetto Ember senza temere di rompere il codice. Designer e altri non sviluppatori possono contribuire ai modelli di pagina senza alcuna conoscenza di JavaScript, e l'interattività può essere aggiunta successivamente.

Questo linguaggio consente anche carichi di asset più leggeri grazie alla _compilazione_ dei template in un "codice byte" che può essere analizzato più velocemente di JavaScript. La **Glimmer VM** consente un tracciamento dei cambiamenti del DOM estremamente veloce senza il bisogno di gestire e differenziare una rappresentazione virtuale memorizzata nella cache (che è un approccio comune per mitigare la lentezza dell'I/O dei cambiamenti del DOM).

Per maggiori informazioni sugli aspetti tecnici della Glimmer VM, il repository GitHub ha della [documentazione](https://github.com/glimmerjs/glimmer-vm/tree/main/guides) — in particolare, [References](https://github.com/glimmerjs/glimmer-vm/blob/main/guides/04-references.md) e [Validators](https://github.com/glimmerjs/glimmer-vm/blob/main/guides/05-validators.md) possono essere di interesse.

Tutto il resto in Ember è _solo_ JavaScript. In particolare, classi JavaScript. Questo è il punto in cui entra in gioco la maggior parte delle parti del "framework", poiché ci sono [superclassi](<https://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)#Subclasses_and_superclasses>), in cui ciascun tipo di _cosa_ ha un diverso scopo e una diversa posizione prevista all'interno del progetto.

Ecco una dimostrazione dell'impatto di Ember sul JavaScript che si trova nei progetti tipici:
[Gavin dimostra come meno del 20% del JS scritto sia specifico di Ember](https://x.com/gavinjoyce/status/1174726713101705216).

![un insieme di file di codice con il JavaScript specifico di Ember evidenziato, mostrando che solo il 20% del codice Ember è specifico di Ember](20percent-js-specific-ember.png)

## Iniziare

Il resto del materiale su Ember che trova qui consiste in un tutorial in più parti, in cui realizzeremo una versione della classica [app di esempio TodoMVC](https://todomvc.com/), insegnando come utilizzare gli elementi essenziali del framework Ember lungo il percorso. TodoMVC è un'applicazione di tracciamento delle attività basilare implementata in molte tecnologie diverse.

[Qui è la versione completa di Ember](https://nullvoxpopuli.github.io/ember-todomvc-tutorial/), per riferimento.

### Una nota su TodoMVC e l'accessibilità

Il progetto TodoMVC presenta alcuni problemi in termini di adesione alle pratiche web accessibili/di default. Ci sono un paio di problemi GitHub aperti su questa famiglia di progetti TodoMVC:

- [Aggiungere accesso tramite tastiera alle demo](https://github.com/tastejs/todomvc/issues/1017)
- [Riab+ilitare Outline sugli elementi focalizzabili](https://github.com/tastejs/todomvc-app-css/issues/35)

Ember ha un forte impegno a essere accessibile di default e vi è un'[intera sezione delle Guide Ember sull'accessibilità](https://guides.emberjs.com/release/accessibility/) su cosa significa per la progettazione di siti web/app.

Detto ciò, poiché questo tutorial si focalizza sul lato JavaScript della creazione di una piccola applicazione web, il valore di TodoMVC deriva dal fornire CSS preconfezionato e una struttura HTML consigliata, che elimina le piccole differenze tra implementazioni, consentendo un confronto più facile. Più avanti nel tutorial, ci concentreremo sull'aggiungere codice alla nostra applicazione per correggere alcuni dei maggiori difetti di TodoMVC.

## Installazione degli strumenti Ember

Ember utilizza uno strumento command-line interface (CLI) per costruire e creare lo scheletro delle parti della tua applicazione.

1. Avrai bisogno di avere node e npm installati prima di poter installare ember-cli. [Vai qui per scoprire come installare node e npm](/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line#adding_powerups), se non li hai già installati.
2. Ora digita il seguente comando nel tuo terminale per installare ember-cli:

   ```bash
   npm install -g ember-cli
   ```

   Questo strumento fornisce il programma `ember` nel tuo terminale, che viene utilizzato per creare, costruire, sviluppare, testare e creare lo scheletro della tua applicazione (esegui `ember --help` per un elenco completo dei comandi e delle loro opzioni).

3. Per creare una nuova applicazione, digita il seguente comando nel tuo terminale. Questo crea una nuova directory all'interno della directory corrente in cui ti trovi chiamata todomvc, contenente lo scheletro per una nuova app Ember. Assicurati di navigare in un percorso appropriato nel terminale prima di eseguirlo. (Buoni suggerimenti sono le directory del tuo "desktop" o "documenti", in modo che sia facile da trovare):

   ```bash
   ember new todomvc
   ```

   Oppure, su Windows:

   ```bash
   npx ember-cli new todomvc
   ```

Questo genera un ambiente di sviluppo applicativo pronto per la produzione che include le seguenti funzionalità per impostazione predefinita:

- Server di sviluppo con live reload.
- Architettura a plugin che consente ai pacchetti di terze parti di potenziare notevolmente la tua applicazione.
- Le ultime novità di JavaScript attraverso l'integrazione di Babel e webpack.
- Ambiente di test automatizzato che esegue i tuoi test nel browser, consentendoti di _testare come un utente_.
- Transpilazione e minificazione di entrambi CSS e JavaScript per build di produzione.
- Convenzioni per minimizzare le differenze tra applicazioni (consentendo un più facile passaggio del contesto mentale).

## Preparazione per costruire il nostro progetto Ember

Avrai bisogno di un editor di codice prima di continuare a interagire con il tuo nuovo progetto. Se non ne hai già configurato uno, [The Ember Atlas](https://www.notion.so/Editors-Tooling-5da96f0b2baf4ce1bf3fd58e3b60c7f6) ha alcune guide su come configurare vari editor.

### Installazione degli asset condivisi per progetti TodoMVC

L'installazione degli asset condivisi, come stiamo per fare, non è generalmente un passaggio richiesto per nuovi progetti, ma ci consente di utilizzare un CSS condiviso esistente in modo da non dover cercare di indovinare quale CSS è necessario creare gli stili TodoMVC.

1. Innanzitutto, entra nella tua directory `todomvc` nel terminale, ad esempio utilizzando `cd todomvc` su macOS/Linux.
2. Ora esegui il seguente comando per posizionare il CSS comune di todomvc all'interno della tua app:

   ```bash
   npm install --save-dev todomvc-app-css todomvc-common
   ```

3. Successivamente, trova il file [ember-cli-build.js](https://github.com/ember-cli/ember-cli/blob/master/blueprints/app/files/ember-cli-build.js) all'interno della directory todomvc (è proprio lì dentro la radice) e aprilo nel tuo editor di codice scelto. ember-cli-build.js è responsabile della configurazione dei dettagli su come viene creata la tua applicazione — incluso il bundle di tutti i tuoi file insieme, la minificazione degli asset e la creazione di sourcemap — con impostazioni predefinite ragionevoli, perciò tipicamente non devi preoccuparti di questo file.

   Tuttavia, aggiungeremo righe al file ember-cli-build.js per importare i nostri file CSS condivisi, in modo che diventino parte della nostra build senza doverli esplicitamente [`@import`](/it/docs/Web/CSS/@import) nel file `app.css` (questo richiederebbe riscritture dell'URL al momento della build e quindi sarebbe meno efficiente e più complicato da configurare).

4. In `ember-cli-build.js`, trova il seguente codice:

   ```js
   let app = new EmberApp(defaults, {
     // Add options here
   });
   ```

5. Aggiungi le seguenti righe sotto di esso prima di salvare il file:

   ```js
   app.import("node_modules/todomvc-common/base.css");
   app.import("node_modules/todomvc-app-css/index.css");
   ```

   Per ulteriori informazioni su cosa fa `ember-cli-build.js`, e per altri modi in cui puoi personalizzare la tua build / pipeline, le Ember Guides hanno una pagina su [Addons e Dipendenze](https://guides.emberjs.com/release/addons-and-dependencies/).

6. Infine, trova `app.css`, situato in `app/styles/app.css`, e incolla quanto segue:

   ```css
   :focus,
   .view label:focus,
   .todo-list li .toggle:focus + label,
   .toggle-all:focus + label {
     /* !important needed because todomvc styles deliberately disable the outline */
     outline: #d86f95 solid !important;
   }
   ```

Questo CSS sovrascrive alcuni degli stili forniti dal pacchetto npm `todomvc-app-css`, consentendo così di rendere visibile il focus della tastiera. Questo va in qualche modo verso la risoluzione di uno dei principali svantaggi di accessibilità dell'app TodoMVC di default.

### Avviare il server di sviluppo

Può avviare l'app in modalità _development_ digitando il seguente comando nel tuo terminale, mentre sei nella directory `todomvc`:

```bash
ember server
```

Dovresti ottenere un output simile al seguente:

```plain
Build successful (190ms) – Serving on http://localhost:4200/

Slowest Nodes (totalTime >= 5%)          | Total (avg)
-----------------------------------------+-----------
BroccoliMergeTrees (17)                  | 35ms (2 ms)
Package /assets/vendor.js (1)            | 13ms
Concat: Vendor Styles/assets/vend... (1) | 12ms
```

Il server di sviluppo si avvia a `http://localhost:4200`, che può visitare nel suo browser per vedere come appare il suo lavoro finora.

Se tutto funziona correttamente, dovrebbe vedere una pagina come questa:

![La pagina iniziale predefinita quando si crea una nuova app Ember, con una mascotte a cartone animato, che dice congratulazioni](ember-start-page.png)

> [!NOTE]
> Nei sistemi Windows senza il [Windows Subsystem for Linux (WSL)](https://learn.microsoft.com/en-us/windows/wsl/install), si sperimenteranno tempi di build generalmente più lenti rispetto a macOS, Linux e Windows _con_ WSL.

## Sommario

Finora, tutto bene. Siamo arrivati ​​al punto in cui possiamo iniziare a costruire la nostra app di esempio TodoMVC in Ember. Nell'articolo successivo esamineremo come costruire la struttura del markup della nostra app, come un gruppo di componenti logici.

{{NextMenu("Learn_web_development/Core/Frameworks_libraries/Ember_structure_componentization", "Learn_web_development/Core/Frameworks_libraries")}}
