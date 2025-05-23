---
title: Introduzione a Vue
slug: Learn_web_development/Core/Frameworks_libraries/Vue_getting_started
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Core/Frameworks_libraries/Vue_first_component", "Learn_web_development/Core/Frameworks_libraries")}}

Ora introduciamo Vue, il terzo dei nostri framework. In questo articolo daremo uno sguardo al background di Vue, impareremo come installarlo e creare un nuovo progetto, studieremo la struttura ad alto livello del progetto intero e di un componente individuale, vedremo come eseguire il progetto localmente e lo prepareremo per iniziare a costruire il nostro esempio.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          Familiarità con i linguaggi core <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>,
          conoscenza del
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminal/command line</a
          >.
        </p>
        <p>
          I componenti Vue sono scritti come una combinazione di oggetti JavaScript che
          gestiscono i dati dell'app e una sintassi di template basata su HTML che mappa
          la struttura DOM sottostante. Per l'installazione, e per usare alcune delle
          funzionalità più avanzate di Vue (come i componenti a file singolo o le funzioni di rendering),
          avrai bisogno di un terminale con node + npm installati.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Configurare un ambiente di sviluppo locale Vue, creare un'app iniziale, e
        comprendere le basi di come funziona.
      </td>
    </tr>
  </tbody>
</table>

> [!NOTE]
> Questo tutorial mira alla [versione 3.4.21 di Vue](https://github.com/vuejs/core/blob/main/CHANGELOG.md#3421-2024-02-28) utilizzando [`create-vue` 3.10.2](https://github.com/vuejs/create-vue/releases/tag/v3.10.3) (con Node.js versione `v20.11.0`) ed è stato aggiornato l'ultima volta a maggio 2024.

## Un Vue più chiaro

Vue è un moderno framework JavaScript che fornisce utili funzionalità per il miglioramento progressivo — a differenza di molti altri framework, è possibile utilizzare Vue per migliorare HTML esistente. Questo ti permette di usare Vue come sostituto immediato per una libreria come [jQuery](https://jquery.com/).

Detto ciò, puoi anche usare Vue per scrivere intere Single Page Applications (SPA). Ciò ti consente di creare markup gestiti interamente da Vue, il che può migliorare l'esperienza dello sviluppatore e le prestazioni quando si lavora con applicazioni complesse. Consente anche di usufruire di librerie per il routing lato client e la gestione dello stato quando necessario. Inoltre, Vue adotta un approccio "intermedio" per strumenti come il routing lato client e la gestione dello stato. Sebbene il team core di Vue mantenga librerie suggerite per queste funzioni, non sono direttamente incluse in Vue. Questo permette di selezionare una libreria di routing/gestione stato diversa se meglio si adatta alla tua applicazione.

Oltre a permetterti di integrare progressivamente Vue nelle tue applicazioni, Vue offre anche un approccio progressivo alla scrittura del markup. Come la maggior parte dei framework, Vue ti permette di creare blocchi riutilizzabili di markup tramite componenti. La maggior parte delle volte, i componenti Vue sono scritti usando una sintassi di template HTML speciale. Quando hai bisogno di più controllo rispetto a quanto consente la sintassi HTML, puoi scrivere funzioni JavaScript in puro o JSX per definire i tuoi componenti.

Mentre lavori attraverso questo tutorial, potresti voler tenere aperta la [guida di Vue](https://vuejs.org/guide/introduction.html) e la [documentazione API](https://vuejs.org/api/) in altre schede, in modo da poter riferirti a esse se desideri ulteriori informazioni su un qualsiasi sottoargomento.

## Installazione

Per usare Vue in un sito esistente, puoi inserire uno dei seguenti elementi [`<script>`](/it/docs/Web/HTML/Reference/Elements/script) in una pagina. Questo ti consente di iniziare a usare Vue su siti esistenti, motivo per cui Vue si vanta di essere un framework progressivo. Questa è una grande opzione quando stai migrando un progetto esistente che utilizza una libreria come jQuery a Vue. Con questo metodo, puoi utilizzare molte delle caratteristiche core di Vue, come gli attributi, i componenti personalizzati e la gestione dei dati.

- Script di sviluppo (non ottimizzato, ma include avvisi di console utili per lo sviluppo.)

  ```html
  <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
  ```

- Script di produzione (Versione ottimizzata, minimi avvisi di console. Si raccomanda di specificare un numero di versione quando si include Vue sul proprio sito in modo che eventuali aggiornamenti del framework non interrompano il sito live a tua insaputa.)

  ```html
  <script src="https://unpkg.com/vue@3/dist/vue.global.prod.js"></script>
  ```

Tuttavia, questo approccio ha alcune limitazioni. Per costruire applicazioni più complesse, sarà necessario utilizzare il [pacchetto npm di Vue](https://www.npmjs.com/package/vue). Questo ti permetterà di usare le funzionalità avanzate di Vue e di utilizzare strumenti come Vite o webpack. Per facilitare la costruzione di app con Vue, esiste uno strumento di scaffolding CLI [create-vue](https://github.com/vuejs/create-vue) per semplificare il processo di sviluppo. Per utilizzare `create-vue` avrai bisogno di:

1. Node.js 20 installato.
2. [npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm/) o [yarn](https://yarnpkg.com/).

> [!NOTE]
> Se non hai installato quanto sopra, scopri [di più sull'installazione di npm e Node.js](/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line#adding_powerups) qui.

Per installare Vue e inizializzare un nuovo progetto, esegui il seguente comando nel tuo terminale:

```bash
npm create vue@latest
```

Oppure, se preferisci usare yarn:

```bash
yarn create vue@latest
```

Questo comando ti darà un elenco di configurazioni di progetto che puoi utilizzare. Ci sono alcuni predefiniti, ma puoi scegliere le impostazioni specifiche per il tuo progetto. Queste opzioni ti consentono di configurare cose come TypeScript, linting, vue-router, collaudo, e altro.
Procederemo attraverso le opzioni nei passaggi di inizializzazione qui sotto.

## Inizializzazione di un nuovo progetto

Per esplorare varie funzionalità di Vue, costruiremo un'app esempio di lista delle cose da fare. Inizieremo utilizzando `create-vue` per costruire un nuovo scaffold per la nostra app.
Nel terminale, `cd` nella directory in cui desideri creare la tua app di esempio, quindi esegui `npm create vue@latest` (o `yarn create vue@latest` se preferisci Yarn).

Lo strumento interattivo ti permette di scegliere alcune opzioni e puoi procedere premendo <kbd>Invio</kbd>.
Per questo progetto, useremo la seguente configurazione:

```plain
✔ Project name: … todo-vue
✔ Add TypeScript? … No
✔ Add JSX Support? … No
✔ Add Vue Router for Single Page Application development? … No
✔ Add Pinia for state management? … No
✔ Add Vitest for Unit Testing? … No
✔ Add an End-to-End Testing Solution? › No
✔ Add ESLint for code quality? … Yes
? Add Prettier for code formatting? › Yes
```

Dopo aver scelto queste opzioni, la struttura del tuo progetto è ora configurata e le dipendenze sono definite in un file `package.json`.
I passaggi successivi consistono nell'installare le dipendenze e avviare il server, e lo strumento stampa comodamente i comandi necessari per farlo:

```plain
Scaffolding project in /path/to/todo-vue...

Done. Now run:

  cd todo-vue
  npm install
  npm run format
  npm run dev
```

## Struttura del progetto

Se tutto è andato a buon fine, la CLI dovrebbe aver creato una serie di file e directory per il tuo progetto. I più significativi sono i seguenti:

- `package.json`: Questo file contiene l'elenco delle dipendenze per il tuo progetto, oltre ad alcuni metadati e configurazione di `eslint`.
- `yarn.lock`: Se hai scelto `yarn` come gestore dei pacchetti, questo file sarà generato con un elenco di tutte le dipendenze e sotto-dipendenze necessarie al tuo progetto.
- `jsconfig.json`: Questo è un file di configurazione per [Visual Studio Code](https://code.visualstudio.com/docs/languages/jsconfig) e fornisce contesto a VS Code sulla tua struttura di progetto e assiste nell'autocompletamento.
- `vite.config.js`: Questo è il file di configurazione per il server di sviluppo [Vite](https://vite.dev/) che costruisce e serve il tuo progetto sulla tua macchina locale.
  Il server Vite osserva i file sorgente per i cambiamenti e può ricaricare a caldo il progetto mentre apporti modifiche.
- `public`: Questa directory contiene risorse statiche che vengono pubblicate durante la build.
  - `favicon.ico`: Questo è il favicon per la tua app. Attualmente, è il logo di Vue.
- `index.html`: La tua app Vue viene eseguita da questa pagina HTML.
- `src`: Questa directory contiene il nucleo della tua app Vue.

  - `main.js`: questo è il punto di ingresso della tua applicazione. Attualmente, questo file inizializza la tua applicazione Vue e indica quale elemento HTML nel file `index.html` la tua app dovrebbe essere attaccata. Questo file è spesso dove registri componenti globali o librerie Vue aggiuntive.
  - `App.vue`: questo è il componente di livello superiore nella tua app Vue. Vedi sotto per ulteriori spiegazioni sui componenti Vue.
  - `components`: questa directory è dove conservi i tuoi componenti. Attualmente, ha solo un componente esempio.
  - `assets`: questa directory è per conservare risorse statiche come CSS e immagini. Poiché questi file sono nella directory sorgente, possono essere processati da webpack. Questo significa che puoi usare pre-processori come [Sass/SCSS](https://sass-lang.com/) o [Stylus](https://stylus-lang.com/).

> [!NOTE]
> A seconda delle opzioni selezionate durante la creazione di un nuovo progetto, potrebbero essere presenti altre directory (ad esempio, se scegli un router, avrai anche una directory `views`).

## File .vue (componenti a file singolo)

Come in molti framework front-end, i componenti sono una parte centrale della costruzione di app in Vue. Questi componenti ti permettono di suddividere una grande applicazione in blocchi discreti che possono essere creati e gestiti separatamente, e trasferire dati tra di loro come richiesto. Questi piccoli blocchi possono aiutarti a ragionare e testare il tuo codice.

Mentre alcuni framework incoraggiano a separare il codice di template, logica e stile in file separati, Vue adotta l'approccio opposto. Utilizzando i [Single File Components (SFC)](https://vuejs.org/guide/scaling-up/sfc.html), Vue ti permette di raggruppare i tuoi template, lo script corrispondente, e CSS tutti insieme in un singolo file con estensione `.vue`. Questi file sono processati da uno strumento di build JS (come Vite o webpack), il che significa che puoi sfruttare gli strumenti di build-time nel tuo progetto. Ciò ti consente di utilizzare strumenti come Babel, TypeScript, SCSS e altro ancora per creare componenti più sofisticati.

Diamo un'occhiata dentro la cartella `src` nel progetto che abbiamo creato con la CLI e ispezioniamo il tuo primo file `.vue`: `App.vue`.

### App.vue

Apri il tuo file `App.vue` — vedrai che ha tre parti: `<template>`, `<script>`, e `<style>`, che contengono il template, lo scripting, e le informazioni di stile del componente. Tutti i componenti a file singolo condividono questa stessa struttura di base.

`<template>` contiene tutta la struttura di markup e la logica di visualizzazione del tuo componente. Il tuo template può contenere qualsiasi HTML valido, così come qualche sintassi specifica di Vue che copriremo più avanti.

> [!NOTE]
> Impostando l'attributo `lang` sul tag `<template>`, puoi utilizzare la sintassi di template Pug invece che l'HTML standard — `<template lang="pug">`. Ci atterremo all'HTML standard in questo tutorial, ma vale la pena sapere che questo è possibile.

`<script>` contiene tutta la logica non di visualizzazione del tuo componente. La tua `<script>` tag è dove registri localmente i componenti, definisci gli input del componente (props), gestisci lo stato locale, definisci metodi, e altro. Il tuo passo di build elaborerà questo oggetto e lo trasformerà (con il tuo template) in un componente Vue con una funzione `render()`.

Nel caso di `App.vue`, due componenti `TheWelcome` e `HelloWorld` sono registrati tramite importazioni. Quando registri un componente in questo modo, lo stai registrando localmente. I componenti registrati localmente possono essere utilizzati solo all'interno dei componenti che li registrano, quindi devi importarli e registrarli in ogni file di componente che li utilizza. Questo è utile per lo {{Glossary("Tree_shaking", "Tree shaking")}} (non caricare codice non utilizzato) e il bundle splitting (caricare il codice solo quando necessario) perché non ogni pagina nella tua app necessita necessariamente di ogni componente.

```vue
<script setup>
import HelloWorld from "./components/HelloWorld.vue";
import TheWelcome from "./components/TheWelcome.vue";
</script>
```

> [!NOTE]
> Se vuoi utilizzare la sintassi [TypeScript](https://www.typescriptlang.org/), devi impostare l'attributo `lang` sul tag `<script>` per indicare al compilatore che stai usando TypeScript — `<script lang="ts">`.

`<style>` è dove scrivi il tuo CSS per il componente. Se aggiungi un attributo `scoped` — `<style scoped>` — Vue limiterà lo stile al contenuto del tuo SFC. Questo funziona in modo simile alle soluzioni CSS-in-JS, ma ti permette di scrivere semplicemente CSS.

> [!NOTE]
> Se selezioni un pre-processore CSS quando crei il progetto tramite la CLI, puoi aggiungere un attributo `lang` al tag `<style>` in modo che il contenuto possa essere processato al momento del build. Ad esempio, `<style lang="scss">` ti permetterà di utilizzare la sintassi SCSS nelle tue informazioni di stile.

## Esecuzione dell'app localmente

Lo strumento `create-vue` viene fornito con Vite come server di sviluppo integrato. Questo ti consente di eseguire la tua app localmente in modo da poterla testare facilmente senza dover configurare un server da zero. La CLI aggiunge comandi al file `package.json` del progetto come script npm per eseguirli facilmente.

Nel tuo terminale, prova a eseguire `npm run dev` (o `yarn dev` se preferisci yarn). Il tuo terminale dovrebbe restituire un output simile al seguente:

```plain
  VITE v5.0.11  ready in 312 ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
  ➜  press h + enter to show help
```

Se navighi all'indirizzo "localhost" in una nuova scheda del browser dovresti vedere la tua app (questo indirizzo dovrebbe essere `http://localhost:5173/` come indicato sopra, ma può variare in base alla tua configurazione). In questo momento, l'app dovrebbe contenere un messaggio di benvenuto, un link alla documentazione di Vue, link ai plugin che hai aggiunto quando hai inizializzato l'app con la tua CLI, e alcuni altri link utili alla comunità ed ecosistema di Vue.

## Apportare alcune modifiche

Facciamo la nostra prima modifica all'app — elimineremo il logo Vue. Apri il file `App.vue`, ed elimina l'elemento [`<img>`](/it/docs/Web/HTML/Reference/Elements/img) dalla sezione del template:

```vue
<img
  alt="Vue logo"
  class="logo"
  src="./assets/logo.svg"
  width="125"
  height="125" />
```

Se il tuo server è ancora in esecuzione, dovresti vedere il logo rimosso dal sito reso quasi istantaneamente. Rimuoviamo anche il componente `HelloWorld` dal nostro template.

Prima di tutto elimina questa riga:

```vue
<HelloWorld msg="You did it!" />
```

Se salvi il tuo file `App.vue` ora, il tuo editor potrebbe mostrare un errore perché abbiamo registrato il componente `HelloWorld` ma non lo stiamo utilizzando. Dobbiamo anche rimuovere le righe all'interno dell'elemento `<script>` che importano e registrano il componente:

Elimina queste righe ora:

```js
import HelloWorld from "./components/HelloWorld.vue";
```

Se rimuovi tutto all'interno del tag `<template>`, vedrai un errore che dice `Il template richiede un elemento figlio` nel tuo editor.
Puoi risolvere aggiungendo del contenuto all'interno del tag `<template>` e possiamo iniziare con un nuovo elemento `<h1>` all'interno di un `<div>`.
Dato che andremo a creare un'app di lista delle cose da fare di seguito, impostiamo l'intestazione su "To-Do List" in questo modo:

```vue
<template>
  <div id="app">
    <h1>To-Do List</h1>
  </div>
</template>
```

`App.vue` mostrerà ora la nostra intestazione, come ci si aspetta.

## Riepilogo

Lasciamo qui per ora. Abbiamo appreso alcune delle idee dietro Vue, creato dello scaffolding per la nostra app di esempio, l'abbiamo ispezionata, e apportato alcune modifiche preliminari.

Con un'introduzione di base fuori del percorso, andremo ora oltre e costruiremo la nostra app di esempio, un'applicazione di lista delle cose da fare di base che ci permetterà di memorizzare un elenco di elementi, contrassegnarli quando completati, e filtrare l'elenco per tutte le attività, complete e incomplete.

Nel prossimo articolo costruiremo il nostro primo componente personalizzato, e guarderemo alcuni concetti importanti come il passaggio dei props e il salvataggio dello stato dei dati.

{{NextMenu("Learn_web_development/Core/Frameworks_libraries/Vue_first_component", "Learn_web_development/Core/Frameworks_libraries")}}
