---
title: Iniziare con Vue
slug: Learn_web_development/Core/Frameworks_libraries/Vue_getting_started
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Core/Frameworks_libraries/Vue_first_component", "Learn_web_development/Core/Frameworks_libraries")}}

Ora introduciamo Vue, il terzo dei nostri framework. In questo articolo esamineremo un po' il contesto di Vue, impareremo come installarlo e creare un nuovo progetto, studieremo la struttura ad alto livello dell'intero progetto e di un singolo componente, vedremo come eseguire il progetto localmente e lo prepareremo per iniziare a costruire il nostro esempio.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          Familiarità con i linguaggi di base <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>,
          conoscenza del
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminale/linea di comando</a
          >.
        </p>
        <p>
          I componenti Vue sono scritti come una combinazione di oggetti JavaScript che
          gestiscono i dati dell'app e una sintassi di template basata su HTML che si mappa
          alla struttura sottostante del DOM. Per l'installazione e per usare alcune delle
          funzionalità più avanzate di Vue (come i componenti a file singolo o le funzioni di rendering), avrà bisogno di un terminale con node + npm installati.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Configurare un ambiente di sviluppo Vue locale, creare un'app di base e
        comprendere le basi del suo funzionamento.
      </td>
    </tr>
  </tbody>
</table>

> [!NOTE]
> Questo tutorial è rivolto a [Vue versione 3.4.21](https://github.com/vuejs/core/blob/main/CHANGELOG.md#3421-2024-02-28) utilizzando [`create-vue` 3.10.2](https://github.com/vuejs/create-vue/releases/tag/v3.10.3) (con Node.js versione `v20.11.0`) ed è stato rivisto per l'ultima volta a maggio 2024.

## Un Vue più chiaro

Vue è un moderno framework JavaScript che fornisce utility utili per l'arricchimento progressivo — a differenza di molti altri framework, può utilizzare Vue per migliorare l'HTML esistente. Ciò le consente di utilizzare Vue come sostituto di una libreria esistente come [jQuery](https://jquery.com/).

Detto questo, può anche utilizzare Vue per scrivere intere Single Page Applications (SPA). Ciò le consente di creare markup gestito interamente da Vue, migliorando l'esperienza degli sviluppatori e le prestazioni quando si gestiscono applicazioni complesse. Offre anche l'opportunità di utilizzare librerie per il routing lato client e la gestione dello stato quando necessita. Inoltre, Vue adotta un approccio di mezzo al tooling come il routing lato client e la gestione dello stato. Anche se il team core di Vue mantiene librerie suggerite per queste funzioni, non sono direttamente integrate in Vue. Ciò consente di selezionare una libreria di routing/gestione dello stato diversa se si adatta meglio alla sua applicazione.

Oltre a consentirle di integrare progressivamente Vue nelle sue applicazioni, Vue offre un approccio progressivo alla scrittura del markup. Come la maggior parte dei framework, Vue le consente di creare blocchi di markup riutilizzabili tramite componenti. Nella maggior parte dei casi, i componenti Vue sono scritti utilizzando una speciale sintassi di template HTML. Quando necessita di un controllo maggiore rispetto a quanto consente la sintassi HTML, può scrivere funzioni JSX o JavaScript puro per definire i componenti.

Mentre lavora a questo tutorial, potrebbe voler tenere aperta la [guida di Vue](https://vuejs.org/guide/introduction.html) e la [documentazione API](https://vuejs.org/api/) in altre schede, in modo da poterle consultare se vuole maggiori informazioni su un qualsiasi sotto-argomento.

## Installazione

Per utilizzare Vue in un sito esistente, può aggiungere uno dei seguenti elementi [`<script>`](/it/docs/Web/HTML/Reference/Elements/script) a una pagina. Ciò le consente di iniziare a utilizzare Vue su siti esistenti, ed è per questo che Vue si vanta di essere un framework progressivo. Questa è un'ottima opzione quando si migra un progetto esistente utilizzando una libreria come jQuery a Vue. Con questo metodo, può utilizzare molte delle funzionalità di base di Vue, come attributi, componenti personalizzati e gestione dei dati.

- Script di sviluppo (non ottimizzato, ma include avvisi sulla console che è ottimo per lo sviluppo).

  ```html
  <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
  ```

- Script di produzione (versione ottimizzata, avvisi sulla console minimi. Si consiglia di specificare un numero di versione quando si include Vue sul sito in modo che eventuali aggiornamenti del framework non interrompano il sito dal vivo senza che se ne renda conto).

  ```html
  <script src="https://unpkg.com/vue@3/dist/vue.global.prod.js"></script>
  ```

Tuttavia, questo approccio ha delle limitazioni. Per costruire applicazioni più complesse, sarà necessario utilizzare il [pacchetto npm di Vue](https://www.npmjs.com/package/vue). Ciò le permetterà di utilizzare funzionalità avanzate di Vue e utilizzare strumenti come Vite o webpack. Per facilitare la costruzione di applicazioni con Vue, è disponibile uno strumento di scaffolding CLI [create-vue](https://github.com/vuejs/create-vue) per ottimizzare il processo di sviluppo. Per utilizzare `create-vue`, avrà bisogno di:

1. Node.js 20 installato.
2. [npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm/) o [yarn](https://yarnpkg.com/).

> [!NOTE]
> Se non ha installato quanto sopra, scopra [maggiori dettagli su come installare npm e Node.js](/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line#adding_powerups) qui.

Per installare Vue e inizializzare un nuovo progetto, esegua il seguente comando nel suo terminale:

```bash
npm create vue@latest
```

Oppure, se preferisce utilizzare yarn:

```bash
yarn create vue@latest
```

Questo comando le darà un elenco di configurazioni di progetto che può utilizzare. Ci sono alcuni valori predefiniti, ma può scegliere le proprie impostazioni specifiche per il progetto. Queste opzioni le consentono di configurare cose come TypeScript, linting, vue-router, testing e altro.
Passeremo attraverso le opzioni nei passaggi di inizializzazione di seguito.

## Inizializzazione di un nuovo progetto

Per esplorare le varie funzionalità di Vue, costruiremo un'app di esempio per una lista di cose da fare. Inizieremo utilizzando `create-vue` per creare un nuovo scaffold per la nostra app.
Nel terminale, faccia `cd` dove vorrebbe creare la sua app di esempio, quindi esegua `npm create vue@latest` (o `yarn create vue@latest` se preferisce Yarn).

Lo strumento interattivo le permette di scegliere alcune opzioni e può procedere premendo <kbd>Enter</kbd>.
Per questo progetto, utilizzeremo la seguente configurazione:

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

Dopo aver scelto queste opzioni, la struttura del progetto è ora configurata e le dipendenze sono definite in un file `package.json`.
I passaggi successivi sono installare le dipendenze e avviare il server, e lo strumento le stampa comodamente i comandi necessari per farlo:

```plain
Scaffolding project in /path/to/todo-vue...

Done. Now run:

  cd todo-vue
  npm install
  npm run format
  npm run dev
```

## Struttura del progetto

Se tutto è andato a buon fine, la CLI dovrebbe aver creato una serie di file e directory per il suo progetto. I più significativi sono i seguenti:

- `package.json`: Questo file contiene l'elenco delle dipendenze per il suo progetto, oltre ad alcuni metadati e configurazione `eslint`.
- `yarn.lock`: Se ha scelto `yarn` come gestore dei pacchetti, questo file verrà generato con un elenco di tutte le dipendenze e sotto-dipendenze di cui il suo progetto ha bisogno.
- `jsconfig.json`: Questo è un file di configurazione per [Visual Studio Code](https://code.visualstudio.com/docs/languages/jsconfig) e fornisce al VS Code il contesto sulla struttura del suo progetto e assiste l'autocompletamento.
- `vite.config.js`: Questo è il file di configurazione per il server di sviluppo [Vite](https://vite.dev/) che costruisce e serve il suo progetto sulla sua macchina locale.
  Il server Vite osserva i file sorgente per i cambiamenti e può ricaricare il progetto mentre apporta modifiche.
- `public`: Questa directory contiene asset statici che sono pubblicati durante il build.
  - `favicon.ico`: Questa è la favicon per la sua app. Attualmente è il logo di Vue.
- `index.html`: La sua app Vue è eseguita da questa pagina HTML.
- `src`: Questa directory contiene il nucleo della sua app Vue.

  - `main.js`: questo è il punto di ingresso per la sua applicazione. Attualmente, questo file inizializza l'applicazione Vue e indica quale elemento HTML nel file `index.html` la sua app deve essere collegata. Questo file è spesso dove registra componenti globali o librerie Vue aggiuntive.
  - `App.vue`: questo è il componente di livello superiore nella sua app Vue. Veda sotto per una spiegazione più dettagliata dei componenti Vue.
  - `components`: questa directory è dove conserva i suoi componenti. Attualmente, ha solo un componente di esempio.
  - `assets`: questa directory è per memorizzare asset statici come CSS e immagini. Poiché questi file sono nella directory sorgente, possono essere elaborati da webpack. Ciò significa che può utilizzare pre-processori come [Sass/SCSS](https://sass-lang.com/) o [Stylus](https://stylus-lang.com/).

> [!NOTE]
> A seconda delle opzioni selezionate durante la creazione di un nuovo progetto, potrebbero essere presenti altre directory (ad esempio, se sceglie un router, avrà anche una directory `views`).

## File .vue (componenti a file singolo)

Come in molti framework front-end, i componenti sono una parte centrale della costruzione di app in Vue. Questi componenti le consentono di suddividere una grande applicazione in blocchi discreti che possono essere creati e gestiti separatamente, e trasferire dati tra di loro secondo necessità. Questi piccoli blocchi possono aiutarla a ragionare e testare il suo codice.

Mentre alcuni framework incoraggiano a separare il codice di template, logica e styling in file separati, Vue adotta l'approccio opposto. Utilizzando i [Componenti a File Singolo (SFC)](https://vuejs.org/guide/scaling-up/sfc.html), Vue le consente di raggruppare i suoi template, script corrispondente e CSS tutto insieme in un unico file che termina con `.vue`. Questi file sono elaborati da un tool di build JS (come Vite o webpack), il che significa che può sfruttare il tooling al momento del build nel suo progetto. Ciò le consente di utilizzare strumenti come Babel, TypeScript, SCSS e altro per creare componenti più sofisticati.

Diamo uno sguardo all'interno della cartella `src` nel progetto che abbiamo creato con la CLI e ispezioniamo il suo primo file `.vue`: `App.vue`.

### App.vue

Apro il suo file `App.vue` — vedrà che ha tre parti: `<template>`, `<script>`, e `<style>`, che contengono il template, lo script e le informazioni di styling del componente. Tutti i Componenti a File Singolo condividono questa struttura di base.

`<template>` contiene tutta la struttura di markup e la logica di visualizzazione del suo componente. Il suo template può contenere qualsiasi HTML valido, oltre ad alcune sintassi specifiche di Vue che trattiamo più avanti.

> [!NOTE]
> Impostando l'attributo `lang` sul tag `<template>`, può utilizzare la sintassi del template Pug anziché l'HTML standard — `<template lang="pug">`. Rimaniamo sull'HTML standard durante questo tutorial, ma è utile sapere che ciò è possibile.

`<script>` contiene tutta la logica non relativa alla visualizzazione del suo componente. Più importantemente, il suo tag `<script>` dove registra localmente i componenti, definisce gli input del componente (props), gestisce lo stato locale, definisce metodi, e altro. Il suo passaggio di build elaborerà questo oggetto e lo trasformerà (con il suo template) in un componente Vue con una funzione `render()`.

Nel caso di `App.vue`, due componenti `TheWelcome` e `HelloWorld` sono registrati tramite importazioni. Quando registra un componente in questo modo, lo sta registrando localmente. I componenti registrati localmente possono essere usati solo all'interno dei componenti che li registrano, quindi ha bisogno di importarli e registrarli in ogni file di componente che li utilizza. Questo è utile per il {{Glossary("Tree_shaking", "Tree shaking")}} (non caricare codice non utilizzato) e la divisione del bundle (caricare il codice solo quando necessario) perché non tutte le pagine della sua app necessitano necessariamente di ogni componente.

```vue
<script setup>
import HelloWorld from "./components/HelloWorld.vue";
import TheWelcome from "./components/TheWelcome.vue";
</script>
```

> [!NOTE]
> Se vuole utilizzare la sintassi [TypeScript](https://www.typescriptlang.org/), deve impostare l'attributo `lang` sul tag `<script>` per indicare al compilatore che sta utilizzando TypeScript — `<script lang="ts">`.

`<style>` è dove scrive il suo CSS per il componente. Se aggiunge un attributo `scoped` — `<style scoped>` — Vue scoperà gli stili ai contenuti del suo SFC. Questo funziona in modo simile alle soluzioni CSS-in-JS, ma permette di scrivere semplicemente il CSS normale.

> [!NOTE]
> Se seleziona un pre-processore CSS quando crea il progetto tramite la CLI, può aggiungere un attributo `lang` al tag `<style>` così che i contenuti possano essere elaborati al momento del build. Ad esempio, `<style lang="scss">` le permetterà di utilizzare la sintassi SCSS nelle sue informazioni di stile.

## Esecuzione dell'app localmente

Lo strumento `create-vue` include Vite come server di sviluppo integrato. Ciò le consente di eseguire l'app localmente in modo da poterla testare facilmente senza dover configurare un server da zero. La CLI aggiunge comandi al file `package.json` del progetto come script npm, così può eseguirli facilmente.

Nel suo terminale, provi a eseguire `npm run dev` (o `yarn dev` se preferisce yarn). Il suo terminale dovrebbe visualizzare qualcosa del genere:

```plain
  VITE v5.0.11  ready in 312 ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
  ➜  press h + enter to show help
```

Se naviga all'indirizzo "localhost" in una nuova scheda del browser dovrebbe vedere la sua app (questo indirizzo dovrebbe essere `http://localhost:5173/` come indicato sopra, ma potrebbe variare in base alla sua configurazione). Al momento, l'app dovrebbe contenere un messaggio di benvenuto, un link alla documentazione di Vue, link ai plugin che ha aggiunto quando ha inizializzato l'app con il CLI, e alcuni altri link utili alla comunità e all'ecosistema di Vue.

## Fare un paio di modifiche

Facciamo la nostra prima modifica all'app — rimuoviamo il logo di Vue. Apra il file `App.vue` e rimuova l'elemento [`<img>`](/it/docs/Web/HTML/Reference/Elements/img) dalla sezione template:

```vue
<img
  alt="Vue logo"
  class="logo"
  src="./assets/logo.svg"
  width="125"
  height="125" />
```

Se il server è ancora in esecuzione, dovrebbe vedere il logo rimosso dal sito quasi istantaneamente. Rimuoviamo anche il componente `HelloWorld` dal nostro template.

Per prima cosa, elimini questa riga:

```vue
<HelloWorld msg="You did it!" />
```

Se salva il suo file `App.vue` ora, il suo editor potrebbe mostrare un errore perché abbiamo registrato il componente `HelloWorld` ma non lo stiamo usando. Dobbiamo anche rimuovere le righe all'interno dell'elemento `<script>` che importano e registrano il componente:

Elimini queste righe ora:

```js
import HelloWorld from "./components/HelloWorld.vue";
```

Se rimuove tutto ciò che c'è dentro al tag `<template>`, vedrà un errore che dice `The template requires child element` nel suo editor.
Può risolvere questo problema aggiungendo del contenuto all'interno del tag `<template>` e possiamo iniziare con un nuovo elemento `<h1>` all'interno di un `<div>`.
Poiché andremo a creare un'app di lista di cose da fare qui sotto, impostiamo il nostro titolo su "To-Do List" così:

```vue
<template>
  <div id="app">
    <h1>To-Do List</h1>
  </div>
</template>
```

`App.vue` mostrerà ora il nostro titolo, come ci si aspetterebbe.

## Riepilogo

Lasciamoci qui per ora. Abbiamo appreso alcune delle idee dietro Vue, creato uno scaffolding per la nostra app di esempio, l'abbiamo ispezionata e abbiamo fatto alcune modifiche preliminari.

Con una introduzione di base fatta, ora ci spingeremo oltre e costruiremo ancora di più la nostra app di esempio, un'applicazione di Lista di cose da fare di base che ci permetterà di memorizzare un elenco di elementi, segnare quelli fatti e filtrare l'elenco per tutte le attività, complete e incomplete.

Nel prossimo articolo costruiremo il nostro primo componente personalizzato e vedremo alcuni concetti importanti come passare `props` a esso e salvare il suo stato dei dati.

{{NextMenu("Learn_web_development/Core/Frameworks_libraries/Vue_first_component", "Learn_web_development/Core/Frameworks_libraries")}}
