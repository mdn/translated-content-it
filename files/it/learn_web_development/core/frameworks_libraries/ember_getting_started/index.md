---
title: Iniziare con Ember
slug: Learn_web_development/Core/Frameworks_libraries/Ember_getting_started
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Core/Frameworks_libraries/Ember_structure_componentization", "Learn_web_development/Core/Frameworks_libraries")}}

Nel nostro primo articolo su Ember esamineremo come funziona Ember e per cosa è utile, installeremo la toolchain di Ember localmente, creeremo un'app di esempio e poi faremo alcune configurazioni iniziali per prepararla allo sviluppo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          È consigliato, come minimo, avere familiarità con i linguaggi core
          <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e avere conoscenza del
          <a href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line">terminale/linea di comando</a>.
        </p>
        <p>
          Una comprensione più approfondita delle moderne funzionalità di JavaScript (come le classi,
          i moduli, ecc.) sarà estremamente utile, poiché Ember fa ampio uso di esse.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Imparare come installare Ember e creare un'app starter.</td>
    </tr>
  </tbody>
</table>

## Introduzione a Ember

Ember è un framework di componenti e servizi che si concentra sull'esperienza complessiva di sviluppo delle applicazioni web, riducendo al minimo le differenze banali tra le applicazioni, pur essendo uno strato moderno e leggero sopra JavaScript nativo. Ember ha anche una grande compatibilità a ritroso e in avanti per aiutare le aziende a mantenersi aggiornate con le ultime versioni di Ember e le ultime convenzioni guidate dalla comunità.

Cosa significa essere un framework di componenti e servizi? I componenti sono pacchetti individuali di comportamento, stile e markup, molto simili a quelli forniti da altri framework frontend, come React, Vue e Angular. Il lato servizio fornisce uno stato condiviso a lunga durata, comportamenti e un'interfaccia per l'integrazione con altre librerie o sistemi. Ad esempio, il Router (che verrà menzionato più avanti in questo tutorial) è un servizio. Componenti e Servizi costituiscono la maggior parte di qualsiasi applicazione EmberJS.

## Casi d'uso

Generalmente, EmberJS funziona bene per la costruzione di app che desiderano avere una o entrambe le seguenti caratteristiche:

- Applicazioni a pagina singola, comprese app web simili a quelle native e [app web progressive](/it/docs/Web/Progressive_web_apps) (PWAs)

  - Ember funziona meglio quando è l'intero front-end della tua applicazione.

- Aumento della coesione tra gli stack tecnologici di molti team

  - Le "migliori pratiche" supportate dalla comunità consentono una velocità di sviluppo a lungo termine più veloce.
  - Ember ha convenzioni chiare che sono utili per garantire la coerenza e aiutare i membri del team a mettersi rapidamente al passo.

### Ember con add-on

EmberJS ha un'architettura a plugin, il che significa che gli add-on possono essere installati e fornire funzionalità aggiuntive senza molta, se non alcuna, configurazione.

Esempi includono:

- [PREmber](https://github.com/ef4/prember): Rendering di siti web statici per blog o contenuti di marketing.
- [empress-blog](https://empress-blog.netlify.app/welcome/): Creazione di post di blog in markdown ottimizzando per SEO con PREmber.
- [ember-service-worker](https://ember-service-worker.com/): Configurazione di una PWA in modo che l'app possa essere installata su dispositivi mobili, proprio come le app dal rispettivo app-store del dispositivo.

### App mobili native

Ember può essere utilizzato anche con app mobili native con un bridge native-mobile per JavaScript, come quello fornito da [Corber](http://corber.io/).

## Opinioni

EmberJS è uno dei framework frontend più opinati in circolazione. Ma cosa significa essere opinato? In Ember, le opinioni sono un insieme di convenzioni che aiutano ad aumentare l'efficienza degli sviluppatori al costo di dover apprendere quelle convenzioni. Poiché le convenzioni sono definite e condivise, le opinioni che sostengono quelle convenzioni aiutano a ridurre le differenze banali tra le app — un obiettivo comune a tutti i framework opinati, attraversando qualsiasi linguaggio ed ecosistema. Gli sviluppatori sono quindi più facilmente in grado di passare tra progetti e applicazioni senza dover reimparare completamente l'architettura, i modelli, le convenzioni, ecc.

Mentre lavori attraverso questa serie di tutorial, noterai le opinioni di Ember — come le convenzioni rigide di denominazione dei file dei componenti.

## Come si relaziona Ember con JavaScript puro?

Ember è costruito su tecnologie JavaScript ed è un sottile strato sopra la tradizionale [programmazione orientata agli oggetti](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object-oriented_programming), permettendo comunque agli sviluppatori di utilizzare tecniche di [programmazione funzionale](https://opensource.com/article/17/6/functional-javascript).

Ember utilizza due principali sintassi:

- JavaScript (o opzionalmente, [TypeScript](https://www.typescriptlang.org/))
- Il linguaggio di template proprio di Ember, che è vagamente basato su [Handlebars](https://handlebarsjs.com/guide/).

Il linguaggio di template è usato per fare ottimizzazioni di build e runtime che altrimenti non sarebbero possibili. Più importante, è un superset di HTML — il che significa che chiunque conosca HTML può dare contributi significativi a qualsiasi progetto Ember con minima paura di rompere il codice. I designer e altri non-sviluppatori possono contribuire ai template delle pagine senza alcuna conoscenza di JavaScript, e l'interattività può essere aggiunta in seguito.

Questo linguaggio abilita anche payload di asset più leggeri grazie alla _compilazione_ dei template in un "byte code" che può essere elaborato più velocemente di JavaScript. La **Glimmer VM** abilita un tracciamento dei cambiamenti del DOM estremamente veloce senza la necessità di gestire e confrontare una rappresentazione virtuale cache (che è un approccio comune per mitigare la lenta I/O dei cambiamenti del DOM).

Per ulteriori informazioni sugli aspetti tecnici della Glimmer VM, il repository GitHub ha della [documentazione](https://github.com/glimmerjs/glimmer-vm/tree/main/guides) — in particolare, [References](https://github.com/glimmerjs/glimmer-vm/blob/main/guides/04-references.md) e [Validators](https://github.com/glimmerjs/glimmer-vm/blob/main/guides/05-validators.md) potrebbero essere di interesse.

Tutto il resto in Ember è _solo_ JavaScript. In particolare, le classi di JavaScript. Questo è dove la maggior parte delle parti del "framework" entrano in gioco, poiché ci sono [superclassi](<https://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)#Subclasses_and_superclasses>), dove ogni tipo di _elemento_ ha uno scopo diverso e una posizione diversa attesa all'interno del tuo progetto.

Ecco una dimostrazione dell'impatto che Ember ha sul JavaScript presente nei progetti tipici:
[Gavin dimostra come meno del 20% del JS scritto è specifico di Ember](https://x.com/gavinjoyce/status/1174726713101705216).

![un insieme di file di codice con il JavaScript specifico di Ember evidenziato, mostrando che solo il 20% del codice JavaScript è specifico di Ember](20percent-js-specific-ember.png)

## Iniziare

Il resto del materiale di Ember che troverai qui consiste in un tutorial multiparte, in cui realizzeremo una versione della classica [app di esempio TodoMVC](https://todomvc.com/), insegnandoti a usare le basi del framework Ember lungo il percorso. TodoMVC è un'applicazione base di tracciamento delle cose da fare implementata in molte tecnologie diverse.

[Ecco la versione completata di Ember](https://nullvoxpopuli.github.io/ember-todomvc-tutorial/), come riferimento.

### Una nota su TodoMVC e accessibilità

Il progetto TodoMVC ha alcuni problemi in termini di aderenza alle pratiche web accessibili/di default. Ci sono un paio di problemi aperti su GitHub riguardanti la famiglia di progetti TodoMVC:

- [Aggiungi accesso da tastiera alle demo](https://github.com/tastejs/todomvc/issues/1017)
- [Riattiva Outline sugli elementi focalizzabili](https://github.com/tastejs/todomvc-app-css/issues/35)

Ember ha un forte impegno per essere accessibile per default e c'è un'intera sezione delle Ember Guides sull'accessibilità su cosa significhi per il design dei siti web / delle app.

Detto ciò, poiché questo tutorial si concentra sul lato JavaScript della creazione di una piccola applicazione web, il valore di TodoMVC deriva dal fornire CSS pre-fatti e una struttura HTML consigliata, che elimina piccole differenze tra le implementazioni, consentendo un confronto più facile. Più avanti nel tutorial, ci concentreremo sull'aggiungere codice alla nostra applicazione per correggere alcuni dei maggiori difetti di TodoMVC.

## Installare gli strumenti di Ember

Ember utilizza un'interfaccia a riga di comando (CLI) per costruire e organizzare parti della tua applicazione.

1. Avrai bisogno di node e npm installati prima di poter installare ember-cli. [Vai qui per scoprire come installare node e npm](/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line#adding_powerups), se non li hai già.
2. Ora digita quanto segue nel tuo terminale per installare ember-cli:

   ```bash
   npm install -g ember-cli
   ```

   Questo strumento fornisce il programma `ember` nel tuo terminale, che viene usato per creare, costruire, sviluppare, testare e organizzare la tua applicazione (esegui `ember --help` per un elenco completo dei comandi e delle loro opzioni).

3. Per creare una nuova applicazione, digita quanto segue nel tuo terminale. Questo crea una nuova directory all'interno della directory corrente in cui ti trovi chiamata todomvc, contenente l'organizzazione per una nuova app Ember. Assicurati di navigare in un posto appropriato nel terminale prima di eseguirlo. (Buoni suggerimenti sono le directory "desktop" o "documents", in modo che sia facile da trovare):

   ```bash
   ember new todomvc
   ```

   Oppure, su Windows:

   ```bash
   npx ember-cli new todomvc
   ```

Questo genera un ambiente di sviluppo pronto per la produzione che include le seguenti funzionalità di default:

- Server di sviluppo con ricaricamento live.
- Architettura a plugin che consente a pacchetti di terze parti di arricchire la tua applicazione.
- JavaScript più recente tramite l'integrazione di Babel e webpack.
- Ambiente di test automatizzato che esegue i tuoi test nel browser, permettendo di _testare come un utente_.
- Transpilazione e minificazione sia di CSS che di JavaScript per le build di produzione.
- Convenzioni per minimizzare le differenze tra le applicazioni (consentendo un cambio di contesto mentale più facile).

## Prepararsi a costruire il nostro progetto Ember

Avrai bisogno di un editor di codice prima di continuare a interagire con il tuo nuovo progetto. Se non hai già configurato uno, [The Ember Atlas](https://www.notion.so/Editors-Tooling-5da96f0b2baf4ce1bf3fd58e3b60c7f6) ha alcune guide su come configurare vari editor.

### Installare gli asset condivisi per i progetti TodoMVC

Installare asset condivisi, come stiamo per fare, non è normalmente un passo necessario per i nuovi progetti, ma ci permette di utilizzare CSS condiviso esistente in modo che non dobbiamo provare a indovinare quale CSS è necessario per creare gli stili TodoMVC.

1. Innanzitutto, entra nella tua directory `todomvc` nel terminale, ad esempio utilizzando `cd todomvc` su macOS/Linux.
2. Ora esegui il seguente comando per posizionare il CSS comune di todomvc all'interno della tua app:

   ```bash
   npm install --save-dev todomvc-app-css todomvc-common
   ```

3. Successivamente, trova il file [ember-cli-build.js](https://github.com/ember-cli/ember-cli/blob/master/blueprints/app/files/ember-cli-build.js) all'interno della directory todomvc (è proprio lì all'interno della radice) e aprilo nel tuo editor di codice scelto. ember-cli-build.js è responsabile della configurazione dei dettagli su come il tuo progetto è costruito — incluse l'aggregazione di tutti i tuoi file, la minificazione degli asset e la creazione di sourcemaps — con impostazioni predefinite ragionevoli, quindi tipicamente non devi preoccuparti di questo file.

   Aggiungeremo però delle linee al file ember-cli-build.js per importare i nostri file CSS condivisi, in modo che diventino parte della nostra build senza doverli esplicitamente [`@import`](/it/docs/Web/CSS/@import) nel file `app.css` (questo richiederebbe riscritture URL al momento della build e quindi sarebbe meno efficiente e più complicato da impostare).

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

   Per maggiori informazioni su cosa fa `ember-cli-build.js`, e per altri modi in cui puoi personalizzare la tua build / pipeline, le Ember Guides hanno una pagina su [Addons and Dependencies](https://guides.emberjs.com/release/addons-and-dependencies/).

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

Questo CSS sovrascrive alcuni degli stili forniti dal pacchetto npm `todomvc-app-css`, consentendo quindi al fuoco della tastiera di essere visibile. Questo va in qualche modo verso la risoluzione di uno dei maggiori svantaggi di accessibilità dell'app predefinita TodoMVC.

### Avviare il server di sviluppo

Puoi avviare l'app in modalità _sviluppo_ digitando il seguente comando nel tuo terminale, mentre sei nella directory `todomvc`:

```bash
ember server
```

Dovrebbe darti un output simile al seguente:

```plain
Build successful (190ms) – Serving on http://localhost:4200/

Slowest Nodes (totalTime >= 5%)          | Total (avg)
-----------------------------------------+-----------
BroccoliMergeTrees (17)                  | 35ms (2 ms)
Package /assets/vendor.js (1)            | 13ms
Concat: Vendor Styles/assets/vend... (1) | 12ms
```

Il server di sviluppo si avvia su `http://localhost:4200`, che puoi visitare nel tuo browser per controllare come appare il tuo lavoro finora.

Se tutto funziona correttamente, dovresti vedere una pagina come questa:

![La pagina iniziale predefinita quando crei una nuova app Ember, con una mascotte cartone animato che dice congratulazioni](ember-start-page.png)

> [!NOTE]
> Su sistemi Windows senza [Windows Subsystem for Linux (WSL)](https://learn.microsoft.com/en-us/windows/wsl/install), sperimenterai tempi di build più lenti nel complesso rispetto a macOS, Linux e Windows _con_ WSL.

## Riepilogo

Finora tutto bene. Siamo arrivati al punto in cui possiamo iniziare a costruire la nostra app di esempio TodoMVC in Ember. Nel prossimo articolo vedremo come costruire la struttura di markup della nostra app, come un gruppo di componenti logici.

{{NextMenu("Learn_web_development/Core/Frameworks_libraries/Ember_structure_componentization", "Learn_web_development/Core/Frameworks_libraries")}}
