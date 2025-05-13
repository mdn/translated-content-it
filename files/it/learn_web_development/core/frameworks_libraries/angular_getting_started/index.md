---
title: Introduzione ad Angular
slug: Learn_web_development/Core/Frameworks_libraries/Angular_getting_started
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Core/Frameworks_libraries/Angular_todo_list_beginning", "Learn_web_development/Core/Frameworks_libraries")}}

È giunto il momento di esaminare il framework Angular di Google, un'altra opzione popolare che incontrerà spesso. In questo articolo esaminiamo cosa offre Angular, installiamo i prerequisiti e configuriamo un'app di esempio, e diamo uno sguardo all'architettura di base di Angular.

> [!NOTE]
> Questo tutorial si rivolge alla [versione 18 di Angular](https://angular.dev/overview) ed è stato rivisto l'ultima volta ad agosto 2024 (`Angular CLI: 18.2.1`).

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi di base <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>,
        conoscenza del
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
          >terminale/linea di comando</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Configurare un ambiente di sviluppo Angular locale, creare un'app di partenza
        e comprendere le basi del suo funzionamento.
      </td>
    </tr>
  </tbody>
</table>

## Che cos'è Angular?

Angular è un framework e una piattaforma di sviluppo, costruito su [TypeScript](https://www.typescriptlang.org/). Viene utilizzato per creare applicazioni web a pagina singola. Come piattaforma, Angular include:

- Un framework basato su componenti per creare applicazioni web scalabili
- Una raccolta di librerie ben integrate che coprono una vasta gamma di funzionalità, tra cui routing, gestione dei form, comunicazione client-server e altro
- Una suite di strumenti per sviluppatori per aiutare a sviluppare, costruire, testare e aggiornare il codice

Quando si costruiscono applicazioni con Angular, si sfrutta una piattaforma che può scalare da progetti a singolo sviluppatore ad applicazioni a livello aziendale. Angular è progettato per rendere gli aggiornamenti il più semplici possibile, in modo da poter sfruttare gli sviluppi più recenti con il minimo sforzo. Soprattutto, l'ecosistema Angular è costituito da un gruppo diversificato di oltre 1,7 milioni di sviluppatori, autori di librerie e creatori di contenuti.

Prima di iniziare a esplorare la piattaforma Angular, dovrebbe sapere del CLI di Angular. Il CLI di Angular è il modo più veloce, facile e consigliato per sviluppare applicazioni Angular. Il CLI di Angular facilita una serie di compiti. Ecco alcuni comandi di esempio che utilizzerà frequentemente:

| Comando                                         | Descrizione                                                           |
| ------------------------------------------------- | -------------------------------------------------------------------- |
| [`ng build`](https://angular.dev/cli/build)       | Compila un'app Angular in una directory di output.                    |
| [`ng serve`](https://angular.dev/cli/serve)       | Costruisce e serve la sua applicazione, ricostruendo al cambiare dei file. |
| [`ng generate`](https://angular.dev/cli/generate) | Genera o modifica file basati su uno schema.                         |
| [`ng test`](https://angular.dev/cli/test)         | Esegue test unitari su un progetto specificato.                      |
| [`ng e2e`](https://angular.dev/cli/e2e)           | Costruisce e serve un'applicazione Angular, quindi esegue test end-to-end. |

Troverà il CLI di Angular un strumento prezioso per costruire le sue applicazioni.

## Cosa costruirà

Questa serie di tutorial la guida attraverso la costruzione di un'applicazione per una lista di attività. Tramite questa applicazione imparerà come usare Angular per gestire, modificare, aggiungere, eliminare e filtrare gli elementi.

## Prerequisiti

Per installare Angular sul proprio sistema locale, sono necessari i seguenti elementi:

- **Node.js**

  Angular richiede una versione [LTS attiva o di manutenzione LTS](https://nodejs.org/en/about/previous-releases) di Node.js. Per informazioni sui requisiti di versione specifici, consulti la pagina [Compatibilità delle versioni](https://angular.dev/reference/versions).

  Per ulteriori informazioni sull'installazione di Node.js, veda [nodejs.org](https://nodejs.org/en/download).
  Se non è sicuro di quale versione di Node.js è in esecuzione sul suo sistema, esegua `node -v` in una finestra del terminale.

- **npm package manager**

  Angular, il CLI di Angular e le applicazioni Angular dipendono da [pacchetti npm](https://docs.npmjs.com/getting-started/what-is-npm/) per molte funzionalità e funzioni.
  Per scaricare e installare i pacchetti npm, è necessario un gestore di pacchetti npm.
  Questa guida utilizza l'interfaccia della linea di comando del [client npm](https://docs.npmjs.com/cli/install/), che viene installata con `Node.js` di default.
  Per verificare di avere il client npm installato, esegua `npm -v` in una finestra del terminale.

## Creazione di un'applicazione Angular

Può utilizzare il CLI di Angular per eseguire comandi nel suo terminale per generare, costruire, testare e distribuire applicazioni Angular.
Per installare il CLI di Angular globalmente, esegua il seguente comando nel suo terminale:

```bash
npm install -g @angular/cli
```

Tutti i comandi del CLI di Angular iniziano con `ng`, seguito da ciò che desidera che il CLI faccia.
Crei una nuova directory dove desidera costruire la sua app e passi alla directory nel terminale. Poi utilizzi il seguente comando [`ng new`](https://angular.dev/cli/new) per creare una nuova applicazione chiamata `todo`:

```bash
ng new todo --routing=false --style=css --ssr=false
```

Il comando `ng new` crea un'applicazione Angular di partenza minimale.
Le ulteriori opzioni, `--routing`, `--style` e `--ssr`, definiscono come gestire la navigazione e gli stili nell'applicazione, e configurano il rendering lato server.
Questo tutorial descrive queste funzionalità più dettagliatamente più avanti.

La prima volta che esegue `ng`, le potrebbe essere chiesto se vuole abilitare il [completamento automatico](https://angular.dev/cli/completion) nel terminale e le analisi.
Il completamento automatico è comodo perché premendo <kbd>TAB</kbd> mentre scrive comandi `ng` mostrerà opzioni possibili e completerà automaticamente gli argomenti.

Può anche decidere se vuole consentire che le analisi sull'uso del CLI vengano inviate ai manutentori di Angular presso Google.
Per saperne di più sulle analisi, consulti la [documentazione del CLI `ng analytics` di Angular](https://angular.dev/cli/analytics).

Per eseguire la sua applicazione `todo`, navighi nel suo nuovo progetto con il comando `cd` e esegua `ng serve`:

```bash
cd todo
ng serve
```

Nel browser, navighi a `http://localhost:4200/` per vedere la sua nuova applicazione di partenza.
Se cambia uno qualsiasi dei file di origine, l'applicazione viene ricaricata automaticamente.

Mentre `ng serve` è in esecuzione, apra una seconda scheda nel terminale o una finestra del terminale per eseguire comandi senza fermare il server.
Se in qualsiasi momento desidera interrompere il servizio della sua applicazione, prema `Ctrl+c` nel terminale che esegue il comando `ng serve`.

## Familiarizzare con l'applicazione Angular

I file sorgente dell'applicazione su cui si concentra questo tutorial si trovano in `src/app`:

```plain
src/app
├── app.component.css
├── app.component.html
├── app.component.spec.ts
├── app.component.ts
└── app.config.ts
```

I file chiave che il CLI genera automaticamente sono i seguenti:

1. `app.component.ts`: Conosciuta anche come la classe, contiene la logica per la pagina principale dell'applicazione.
2. `app.component.html`: Contiene l'HTML per `AppComponent`. I contenuti di questo file sono anche conosciuti come il modello.
   Il modello determina la vista o ciò che vede nel browser.
3. `app.component.css`: Contiene gli stili per `AppComponent`. Si utilizza questo file quando desidera definire stili che si applicano solo a uno specifico componente, a differenza del suo intero applicativo.

Un componente in Angular è composto da tre parti principali: il modello, gli stili e la classe.
Ad esempio, `app.component.ts`, `app.component.html` e `app.component.css` costituiscono insieme il `AppComponent`.
Questa struttura separa la logica, la vista e gli stili in modo che l'applicazione sia più mantenibile e scalabile.
In questo modo, si utilizzano le migliori pratiche fin dall'inizio.

Il CLI di Angular genera anche un file per il test del componente chiamato `app.component.spec.ts`, ma questo tutorial non entra nei test, quindi può ignorare quel file.
Ogni volta che genera un componente, il CLI crea questi file in una directory con il nome che specifica e ne vedremo un esempio più avanti.

Per saperne di più sui test, veda la [guida ai test di Angular](https://angular.dev/guide/testing).

## La struttura di un'applicazione Angular

Angular è costruito con TypeScript.
TypeScript è un superset di JavaScript, il che significa che qualsiasi JavaScript valido è anche TypeScript valido.
TypeScript offre la tipizzazione e una sintassi più concisa rispetto al JavaScript puro, il che le fornisce uno strumento per creare un codice più mantenibile e minimizzare i bug.

I componenti sono i blocchi fondamentali di un'applicazione Angular.
Un componente include una classe TypeScript che ha un decoratore `@Component()`.

### Il decoratore

Si utilizza il decoratore `@Component()` per specificare i metadati (modello HTML e stili) su una classe.

### La classe

La classe è dove inserisce qualsiasi logica di cui il componente ha bisogno.
Questo codice può includere funzioni, listener di eventi, proprietà e riferimenti ai servizi per citarne alcuni.
La classe si trova in un file con un nome come `feature.component.ts`, dove `feature` è il nome del suo componente.
Quindi, potrebbe avere file con nomi come `header.component.ts`, `signup.component.ts` o `feed.component.ts`.
Crea un componente con un decoratore `@Component()` che ha metadati che dicono ad Angular dove trovare l'HTML e il CSS.
Un componente tipico è il seguente:

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
Utilizza un selettore proprio come i tag HTML regolari inserendolo all'interno di altri modelli, cioè `<app-item></app-item>`.
Quando un selettore si trova in un modello, il browser rende il modello di quel componente ogni volta che si incontra un'istanza del selettore.
Questo tutorial guida nella creazione di due componenti e nell'utilizzo di uno all'interno dell'altro.

> [!NOTE]
> Il nome del componente sopra è `ItemComponent` che è anche il nome della classe.
> I nomi sono gli stessi semplicemente perché un componente non è altro che una classe integrata da un decoratore TypeScript.

Il modello di componente di Angular offre un forte incapsulamento e una struttura applicativa intuitiva.
I componenti rendono anche l'applicazione più facile da testare unitamente e possono migliorare la leggibilità complessiva del suo codice.

### Il modello HTML

Ogni componente ha un modello HTML che dichiara come quel componente viene reso.
Può definire questo modello sia inline che per percorso di file.

Per riferirsi a un file HTML esterno, utilizzi la proprietà `templateUrl`:

```ts
@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
})
export class AppComponent {
  // code goes here
}
```

Per scrivere HTML inline, utilizzi la proprietà `template` e scriva l'HTML all'interno di backtick:

```ts
@Component({
  selector: "app-root",
  template: `<h1>To do application</h1>`,
})
export class AppComponent {
  // code goes here
}
```

Angular estende HTML con ulteriore sintassi che le consente di inserire valori dinamici dal suo componente.
Angular aggiorna automaticamente il DOM reso quando lo stato del suo componente cambia.
Un uso di questa funzionalità è l'inserimento di testo dinamico, come mostrato nel seguente esempio.

```html
<h1>\{{ title }}</h1>
```

Le doppie parentesi graffe istruiscono Angular a interpolare i contenuti al loro interno.
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

Quando l'applicazione carica il componente e il suo modello, il browser vede quanto segue:

```html
<h1>To do application</h1>
```

### Stili

Un componente può ereditare stili globali dal file `styles.css` dell'applicazione e aumentare o sovrascriverli con i suoi stili.
Può scrivere stili specifici del componente direttamente nel decoratore `@Component()` o specificare il percorso a un file CSS.

Per includere gli stili direttamente nel decoratore del componente, utilizzi la proprietà `styles`:

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

Tipicamente, un componente usa stili in un file separato.
Può utilizzare la proprietà `styleUrl` con il percorso al file CSS come stringa o `styleUrls` con un array di stringhe se ci sono più fogli di stile CSS che desidera includere:

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

Con stili specifici per il componente, può organizzare il suo CSS in modo che sia facilmente mantenibile e portatile.

### Componenti autonomi

È consigliato [rendere i componenti autonomi](https://angular.dev/guide/components/importing#standalone-components) a meno che un progetto già utilizzi [NgModules](https://angular.dev/guide/ngmodules) (moduli Angular) per organizzare il codice.
Questo tutorial utilizza [componenti autonomi](https://angular.dev/guide/components/importing#standalone-components) che sono più facili da iniziare.

È comune importare [`CommonModule`](https://angular.dev/api/common/CommonModule) in modo che il suo componente possa utilizzare [direttive](https://angular.dev/guide/directives) e [pipe](https://angular.dev/guide/pipes) comuni.

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

## Sommario

Questo è tutto per la sua prima introduzione ad Angular. A questo punto, dovrebbe essere pronto per costruire un'app Angular e avere una comprensione di base di come funziona Angular. Nel prossimo articolo approfondiremo questa conoscenza e inizieremo a costruire la struttura della nostra applicazione di lista di attività.

{{NextMenu("Learn_web_development/Core/Frameworks_libraries/Angular_todo_list_beginning", "Learn_web_development/Core/Frameworks_libraries")}}
