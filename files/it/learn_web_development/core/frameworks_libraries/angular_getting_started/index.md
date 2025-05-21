---
title: Introduzione ad Angular
slug: Learn_web_development/Core/Frameworks_libraries/Angular_getting_started
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Core/Frameworks_libraries/Angular_todo_list_beginning", "Learn_web_development/Core/Frameworks_libraries")}}

È giunto il momento di esaminare il framework Angular di Google, un'altra opzione popolare che incontrerai spesso. In questo articolo vediamo cosa ha da offrire Angular, installiamo i prerequisiti e configuriamo un'app di esempio, e analizziamo l'architettura di base di Angular.

> [!NOTE]
> Questo tutorial si riferisce alla [versione 18 di Angular](https://angular.dev/overview) ed è stato revisionato l'ultima volta in agosto 2024 (`Angular CLI: 18.2.1`).

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con le lingue di base
        <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>,
        conoscenza del
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
          >terminal/linea di comando</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Configurare un ambiente di sviluppo Angular locale, creare un'app di avvio
        e comprendere le basi del suo funzionamento.
      </td>
    </tr>
  </tbody>
</table>

## Che cos'è Angular?

Angular è un framework e una piattaforma di sviluppo, costruito su [TypeScript](https://www.typescriptlang.org/). Viene utilizzato per creare applicazioni web a pagina singola. Come piattaforma, Angular include:

- Un framework basato su componenti per costruire applicazioni web scalabili
- Una raccolta di librerie ben integrate che coprono una vasta gamma di funzionalità, inclusi routing, gestione dei moduli, comunicazione client-server e altro
- Un insieme di strumenti per sviluppatori per aiutare a sviluppare, costruire, testare e aggiornare il codice

Quando crei applicazioni con Angular, stai sfruttando una piattaforma che può scalare da progetti a singolo sviluppatore ad applicazioni a livello aziendale. Angular è progettato per rendere l'aggiornamento il più semplice possibile, così puoi sfruttare le ultime innovazioni con uno sforzo minimo. Infine, l'ecosistema di Angular consiste in un gruppo diversificato di oltre 1,7 milioni di sviluppatori, autori di librerie e creatori di contenuti.

Prima di iniziare a esplorare la piattaforma Angular, dovresti sapere dell'Angular CLI. L'Angular CLI è il modo più veloce, semplice e raccomandato per sviluppare applicazioni Angular. L'Angular CLI facilita una serie di compiti. Ecco alcuni comandi di esempio che utilizzerai frequentemente:

| Comando                                          | Descrizione                                                           |
| ------------------------------------------------ | --------------------------------------------------------------------- |
| [`ng build`](https://angular.dev/cli/build)      | Compila un'app Angular in una directory di output.                    |
| [`ng serve`](https://angular.dev/cli/serve)      | Compila e avvia la tua applicazione, ricompilando su modifiche ai file.|
| [`ng generate`](https://angular.dev/cli/generate)| Genera o modifica file basati su uno schema.                          |
| [`ng test`](https://angular.dev/cli/test)        | Esegue test unitari su un determinato progetto.                       |
| [`ng e2e`](https://angular.dev/cli/e2e)          | Compila e avvia un'applicazione Angular, quindi esegue test end-to-end.|

Troverai l'Angular CLI uno strumento prezioso per costruire le tue applicazioni.

## Cosa costruirai

Questa serie di tutorial ti guida nella costruzione di un'applicazione di lista delle cose da fare. Tramite questa applicazione imparerai a utilizzare Angular per gestire, modificare, aggiungere, eliminare e filtrare elementi.

## Prerequisiti

Per installare Angular sul tuo sistema locale, hai bisogno di quanto segue:

- **Node.js**

  Angular richiede una versione di Node.js [attiva LTS o di manutenzione LTS](https://nodejs.org/en/about/previous-releases). Per informazioni sui requisiti specifici di versione, consulta la pagina [Compatibilità delle versioni](https://angular.dev/reference/versions).

  Per ulteriori informazioni sull'installazione di Node.js, visita [nodejs.org](https://nodejs.org/en/download).
  Se non sei sicuro della versione di Node.js in esecuzione sul tuo sistema, esegui `node -v` in una finestra di terminale.

- **Gestore di pacchetti npm**

  Angular, l'Angular CLI e le applicazioni Angular dipendono da [pacchetti npm](https://docs.npmjs.com/getting-started/what-is-npm/) per molte funzionalità.
  Per scaricare e installare pacchetti npm, hai bisogno di un gestore di pacchetti npm.
  Questa guida utilizza l'interfaccia da linea di comando [npm client](https://docs.npmjs.com/cli/install/), che è installata di default con `Node.js`.
  Per verificare di avere il client npm installato, esegui `npm -v` in una finestra di terminale.

## Creazione di un'applicazione Angular

Puoi usare l'Angular CLI per eseguire comandi nel tuo terminale per generare, compilare, testare e distribuire applicazioni Angular.
Per installare l'Angular CLI a livello globale, esegui il seguente comando nel tuo terminale:

```bash
npm install -g @angular/cli
```

I comandi Angular CLI iniziano tutti con `ng`, seguiti da ciò che vuoi che la CLI faccia.
Crea una nuova directory dove vuoi costruire la tua app e passa alla directory nel terminale. Quindi usa il seguente comando [`ng new`](https://angular.dev/cli/new) per creare una nuova applicazione chiamata `todo`:

```bash
ng new todo --routing=false --style=css --ssr=false
```

Il comando `ng new` crea un'applicazione Angular di avvio minimale.
Le ulteriori flag, `--routing` e `--style`, e `--ssr` definiscono come gestire la navigazione e gli stili nell'applicazione, e configurano il rendering lato server.
Questo tutorial descrive queste funzionalità più in dettaglio in seguito.

La prima volta che esegui `ng`, ti potrebbe essere chiesto se desideri abilitare il [completamento automatico](https://angular.dev/cli/completion) del terminale e le analisi.
Il completamento automatico è comodo perché premere <kbd>TAB</kbd> mentre si digitano comandi `ng` mostrerà le opzioni possibili e completerà automaticamente gli argomenti.

Puoi anche decidere se consentire che le analisi sull'uso della CLI vengano inviate ai manutentori di Angular presso Google.
Per saperne di più sulle analisi, consulta la [documentazione di Angular `ng analytics`](https://angular.dev/cli/analytics).

Per eseguire la tua applicazione `todo`, naviga nel tuo nuovo progetto con il comando `cd` ed esegui `ng serve`:

```bash
cd todo
ng serve
```

Nel browser, naviga a `http://localhost:4200/` per vedere la tua nuova applicazione di avvio.
Se cambi uno qualsiasi dei file sorgente, l'applicazione si ricarica automaticamente.

Mentre `ng serve` è in esecuzione, apri una seconda scheda di terminale o una finestra di terminale per eseguire comandi senza fermare il server.
Se in qualsiasi momento desideri smettere di servire la tua applicazione, premi `Ctrl+c` nel terminale che sta eseguendo il comando `ng serve`.

## Familiarizzare con la tua applicazione Angular

I file sorgente dell'applicazione sui quali questo tutorial si concentra si trovano in `src/app`:

```plain
src/app
├── app.component.css
├── app.component.html
├── app.component.spec.ts
├── app.component.ts
└── app.config.ts
```

I file chiave che la CLI genera automaticamente sono i seguenti:

1. `app.component.ts`: Conosciuta anche come la classe, contiene la logica per la pagina principale dell'applicazione.
2. `app.component.html`: Contiene l'HTML per `AppComponent`. Il contenuto di questo file è anche conosciuto come il template.
   Il template determina la vista o ciò che vedi nel browser.
3. `app.component.css`: Contiene gli stili per `AppComponent`. Utilizzi questo file quando vuoi definire stili che si applicano solo a un componente specifico, anziché a tutta l'applicazione.

Un componente in Angular è composto da tre parti principali: il template, gli stili e la classe.
Ad esempio, `app.component.ts`, `app.component.html` e `app.component.css` insieme costituiscono l'`AppComponent`.
Questa struttura separa la logica, la vista e gli stili in modo che l'applicazione sia più manutenibile e scalabile.
In questo modo, stai utilizzando le migliori pratiche fin dall'inizio.

L'Angular CLI genera anche un file per il test del componente chiamato `app.component.spec.ts`, ma questo tutorial non approfondisce i test, quindi puoi ignorare quel file.
Ogni volta che generi un componente, la CLI crea questi file in una directory con il nome che specifichi e vedremo un esempio di ciò più avanti.

Per saperne di più sui test, consulta la [guida ai test di Angular](https://angular.dev/guide/testing).

## La struttura di un'applicazione Angular

Angular è costruito con TypeScript.
TypeScript è un superset di JavaScript, il che significa che qualsiasi JavaScript valido è anche TypeScript valido.
TypeScript offre la tipizzazione e una sintassi più concisa rispetto al semplice JavaScript, il che ti fornisce uno strumento per creare codice più manutenibile e minimizzare i bug.

I componenti sono i blocchi costitutivi di un'applicazione Angular.
Un componente include una classe TypeScript che ha un decoratore `@Component()`.

### Il decoratore

Utilizzi il decoratore `@Component()` per specificare i metadati (template HTML e stili) relativi a una classe.

### La classe

La classe è dove metti qualsiasi logica di cui il tuo componente ha bisogno.
Questo codice può includere funzioni, gestori di eventi, proprietà e riferimenti a servizi, per nominarne alcuni.
La classe si trova in un file con un nome del tipo `feature.component.ts`, dove `feature` è il nome del tuo componente.
Quindi, potresti avere file con nomi come `header.component.ts`, `signup.component.ts` o `feed.component.ts`.
Crei un componente con un decoratore `@Component()` che ha metadati che dicono ad Angular dove trovare l'HTML e il CSS.
Un tipico componente è il seguente:

```ts
import { Component } from "@angular/core";

@Component({
  selector: "app-item",
  standalone: true,
  imports: [],
  // the following metadata specifies the location of the other parts of the component
  templateUrl: "./item.component.html",
  styleUrl: "./item.component.css",
})
export class ItemComponent {
  // code goes here
}
```

Questo componente si chiama `ItemComponent`, e il suo selettore è `app-item`.
Utilizzi un selettore proprio come i normali tag HTML inserendolo all'interno di altri template, i.e., `<app-item></app-item>`.
Quando un selettore è in un template, il browser rende il template di quel componente ogni volta che viene incontrata un'istanza del selettore.
Questa guida ti condurrà nella creazione di due componenti e nell'utilizzo di uno all'interno dell'altro.

> [!NOTE]
> Il nome del componente sopra è `ItemComponent`, che è anche il nome della classe.
> I nomi sono gli stessi semplicemente perché un componente non è altro che una classe arricchita da un decoratore TypeScript.

Il modello di componenti di Angular offre una forte incapsulazione e una struttura applicativa intuitiva.
I componenti inoltre rendono la tua applicazione più facile da testare unitariamente e possono migliorare la leggibilità complessiva del tuo codice.

### Il template HTML

Ogni componente ha un template HTML che dichiara come quel componente viene reso.
Puoi definire questo template sia inline che per percorso file.

Per riferirsi a un file HTML esterno, utilizza la proprietà `templateUrl`:

```ts
@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
})
export class AppComponent {
  // code goes here
}
```

Per scrivere HTML inline, utilizza la proprietà `template` e scrivi il tuo HTML all'interno di backtick:

```ts
@Component({
  selector: "app-root",
  template: `<h1>To do application</h1>`,
})
export class AppComponent {
  // code goes here
}
```

Angular estende HTML con sintassi aggiuntiva che ti permette di inserire valori dinamici dal tuo componente.
Angular aggiorna automaticamente il DOM reso quando lo stato del tuo componente cambia.
Un uso di questa funzionalità è l'inserimento di testo dinamico, come mostrato nel seguente esempio.

```html
<h1>\{{ title }}</h1>
```

Le doppie parentesi graffe istruiscono Angular a interpolare il contenuto al loro interno.
Il valore per `title` proviene dalla classe del componente:

```ts
import { Component } from "@angular/core";

@Component({
  selector: "app-root",
  standalone: true,
  imports: [],
  template: "<h1>\{{ title }}</h1>",
  styleUrl: "./app.component.css",
})
export class AppComponent {
  title = "To do application";
}
```

Quando l'applicazione carica il componente e il suo template, il browser vede il seguente:

```html
<h1>To do application</h1>
```

### Stili

Un componente può ereditare stili globali dal file `styles.css` dell'applicazione e aumentare o sovrascriverli con i propri stili.
Puoi scrivere stili specifici del componente direttamente nel decoratore `@Component()` o specificare il percorso a un file CSS.

Per includere gli stili direttamente nel decoratore del componente, utilizza la proprietà `styles`:

```ts
@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styles: ["h1 { color: red; }"],
})
export class AppComponent {
  // …
}
```

Tipicamente, un componente utilizza stili in un file separato.
Puoi usare la proprietà `styleUrl` con il percorso al file CSS come stringa o `styleUrls` con un array di stringhe se ci sono più fogli di stile CSS che vuoi includere:

```ts
@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrl: "./app.component.css",
})
export class AppComponent {
  // …
}
```

Con stili specifici del componente, puoi organizzare il tuo CSS in modo che sia facilmente manutenibile e portabile.

### Componenti standalone

È consigliato [rendere i componenti standalone](https://angular.dev/guide/components/importing#standalone-components) a meno che un progetto già utilizzi [NgModules](https://angular.dev/guide/ngmodules) (moduli Angular) per organizzare il codice.
Questo tutorial utilizza [componenti standalone](https://angular.dev/guide/components/importing#standalone-components) che sono più facili da iniziare.

È comune importare [`CommonModule`](https://angular.dev/api/common/CommonModule) in modo che il tuo componente possa utilizzare direttive e pipe comuni.

```ts
import { Component } from "@angular/core";
import { CommonModule } from "@angular/common";

@Component({
  standalone: true,
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrl: "./app.component.css",
  imports: [CommonModule],
})
export class AppComponent {
  // …
}
```

## Riepilogo

Questo è tutto per la tua prima introduzione ad Angular. A questo punto dovresti essere configurato e pronto a costruire un'app Angular, e avere una comprensione di base di come funziona Angular. Nel prossimo articolo approfondiremo questa conoscenza e inizieremo a costruire la struttura della nostra applicazione di lista delle cose da fare.

{{NextMenu("Learn_web_development/Core/Frameworks_libraries/Angular_todo_list_beginning", "Learn_web_development/Core/Frameworks_libraries")}}
