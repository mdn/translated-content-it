---
title: Iniziare la nostra app di lista delle cose da fare in Angular
slug: Learn_web_development/Core/Frameworks_libraries/Angular_todo_list_beginning
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Angular_getting_started","Learn_web_development/Core/Frameworks_libraries/Angular_styling", "Learn_web_development/Core/Frameworks_libraries")}}

A questo punto, siamo pronti a iniziare a creare la nostra applicazione di lista delle cose da fare usando Angular. L'applicazione finita mostrerà un elenco di elementi da fare e includerà funzionalità di modifica, eliminazione e aggiunta. In questo articolo conoscerai la struttura dell'applicazione e lavorerai fino a visualizzare un elenco di base di elementi da fare.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi di base <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
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
        Creare la struttura di base della nostra app, visualizzare un elenco di
        elementi da fare e comprendere i concetti fondamentali di Angular come la struttura dei componenti, la condivisione dei dati tra componenti, e la creazione di contenuti in ciclo.
      </td>
    </tr>
  </tbody>
</table>

## La struttura dell'applicazione di lista delle cose da fare

Come qualsiasi applicazione web, un'applicazione Angular ha un `index.html` come punto di ingresso. L'`index.html` è in realtà il modello HTML di livello superiore dell'app:

```html
<!doctype html>
<html lang="en">
  <head>
    <!-- ... -->
  </head>
  <body>
    <app-root></app-root>
  </body>
</html>
```

All'interno del tag `<body>`, Angular utilizza un elemento speciale — `<app-root>` — per inserire il tuo componente principale, che a sua volta include altri componenti che crei. Generalmente, non è necessario toccare l'`index.html`, e la tua attenzione si concentra principalmente nelle aree specializzate della tua applicazione chiamate componenti.

### Organizza la tua applicazione con componenti

I componenti sono un blocco costruttivo centrale delle applicazioni Angular. Questa applicazione di lista delle cose da fare ha due componenti — un componente come fondazione per la tua applicazione, e un componente per gestire gli elementi della lista.

Ogni componente è costituito da una classe TypeScript, HTML e CSS. TypeScript trascompila, o converte, in JavaScript, il che significa che alla fine la tua applicazione si traduce in puro JavaScript ma hai la comodità di usare le funzionalità estese e la sintassi semplificata di TypeScript.

### Controllo del flusso con blocchi @if e @for

Questo tutorial copre due importanti [blocchi di controllo del flusso](https://angular.dev/guide/templates/control-flow) che dicono al framework quando e come devono essere resi i tuoi modelli. Il primo blocco che questo tutorial copre è il blocco [`@for`](https://angular.dev/api/core/@for) che cicla attraverso una collezione e rende ripetutamente il contenuto di un blocco.

Il secondo blocco che impari in questo tutorial è [`@if`](https://angular.dev/api/core/@if). Puoi usare `@if` per visualizzare contenuti in base a una condizione. Ad esempio, se un utente clicca su un pulsante "modifica", puoi mostrare elementi usati per modificare un elemento. Se un utente clicca su "annulla", puoi rimuovere gli elementi usati per la modifica.

### Condividere dati tra componenti

In questa applicazione di lista delle cose da fare, configuri i tuoi componenti per condividere i dati. Per aggiungere nuovi elementi alla lista, il componente principale deve inviare il nuovo elemento al secondo componente. Questo secondo componente gestisce gli elementi e si occupa della modifica, marcatura come fatto e eliminazione dei singoli elementi.

Ottieni la condivisione dei dati tra componenti Angular con speciali decoratori chiamati `@Input()` e `@Output()`. Usi questi decoratori per specificare che alcune proprietà consentono ai dati di entrare o uscire da un componente. Per usare `@Output()`, generi un evento in un componente in modo che l'altro componente sappia che ci sono dati disponibili.

## Definire Item

Nella directory `app`, crea un nuovo file chiamato `item.ts` con il seguente contenuto:

```ts
export interface Item {
  description: string;
  done: boolean;
}
```

Non utilizzerai questo file fino a [dopo](/it/docs/Learn_web_development/Core/Frameworks_libraries/Angular_item_component#add_logic_to_itemcomponent), ma è un buon momento per conoscere e registrare la tua conoscenza di cosa sia un `item`. L'interfaccia `Item` crea un modello di oggetto `item` in modo che la tua applicazione capisca cosa sia un `item`. Per questa lista delle cose da fare, un `item` è un oggetto che ha una descrizione e può essere segnato come fatto.

## Aggiungere logica ad AppComponent

Ora che sai cos'è un `item`, puoi dare alla tua applicazione alcuni elementi aggiungendoli all'app. In `app.component.ts`, sostituisci il contenuto con il seguente:

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
  componentTitle = "My To Do List";

  filter: "all" | "active" | "done" = "all";

  allItems = [
    { description: "eat", done: true },
    { description: "sleep", done: false },
    { description: "play", done: false },
    { description: "laugh", done: false },
  ];

  get items() {
    if (this.filter === "all") {
      return this.allItems;
    }
    return this.allItems.filter((item) =>
      this.filter === "done" ? item.done : !item.done,
    );
  }
}
```

Le prime due righe sono importazioni in JavaScript. In questo caso stanno importando librerie Angular. Il decoratore `@Component()` specifica i metadati riguardanti `AppComponent`. Ecco alcune informazioni sui metadati che stiamo usando:

- [`standalone`](https://angular.dev/api/core/Component#standalone): Descrive se il componente richiede o meno un [NgModule](https://angular.dev/guide/ngmodules#the-basic-ngmodule). La tua app gestirà direttamente le dipendenze del modello (componenti, direttive, ecc.) usando le importazioni quando è standalone.
- [`selector`](https://angular.dev/api/core/Directive#selector): Ti dice il selettore CSS che usi in un modello per posizionare questo componente. Qui è `'app-root'`. Nell'`index.html`, all'interno del tag `body`, l'Angular CLI ha aggiunto `<app-root></app-root>` quando ha generato la tua applicazione. Tutti i selettori dei componenti li usi allo stesso modo aggiungendoli ad altri modelli HTML dei componenti.
- [`templateUrl`](https://angular.dev/api/core/Component#templateUrl): Specifica il file HTML da associare a questo componente. Qui è `'./app.component.html'`.
- [`styleUrl`](https://angular.dev/api/core/Component#styleUrl): Fornisce la posizione e il nome del file per i tuoi stili che si applicano specificamente a questo componente. Qui è `'./app.component.css'`.
- [`imports`](https://angular.dev/api/core/Component#imports): Ti permette di specificare le dipendenze del componente che possono essere utilizzate all'interno del suo modello.

La proprietà `filter` è di tipo `union`, il che significa che `filter` potrebbe avere il valore `all`, `active` o `done`. Con il tipo `union`, se commetti un errore di battitura nel valore che assegni alla proprietà `filter`, TypeScript te lo fa sapere in modo che tu possa individuare il bug presto. Questa guida ti mostra come aggiungere il filtraggio in un passaggio successivo, ma puoi anche usare un filtro per mostrare l'elenco predefinito di tutti gli elementi.

L'array `allItems` contiene gli elementi della lista delle cose da fare e se sono fatti. Il primo elemento, `eat`, ha un valore `done` pari a true.

Il getter, `get items()`, recupera gli elementi dall'array `allItems` se il `filter` è uguale a `all`. Altrimenti, `get items()` restituisce gli elementi completati o pendenti a seconda di come l'utente filtra la visualizzazione. Il getter stabilisce anche il nome dell'array come `items`, che userai nella sezione seguente.

## Aggiungi HTML al modello AppComponent

Per vedere l'elenco degli elementi nel browser, sostituisci i contenuti di `app.component.html` con il seguente HTML:

```html
<div class="main">
  <h1>\{{ componentTitle }}</h1>
  <h2>What would you like to do today?</h2>

  <ul>
    @for(item of items; track item.description){
    <li>\{{item.description}}</li>
    }
  </ul>
</div>
```

Il `<li>` è all'interno di un blocco `@for` che itera sugli elementi nell'array `items`. Per ogni elemento, viene creato un nuovo `<li>`. Le doppie parentesi graffe che contengono `item.description` istruiscono Angular a popolare ciascun `<li>` con il testo delle descrizioni di ciascun elemento.

La parola chiave `track` nel blocco `@for` di Angular aiuta Angular a identificare quali elementi in un array sono cambiati, sono stati aggiunti o rimossi. Questo rende più facile e veloce per Angular aggiornare il DOM quando l'array viene modificato.

Nel browser, dovresti vedere l'elenco degli elementi come segue:

```plain
My To Do List
What would you like to do today?

* eat
* sleep
* play
* laugh
```

## Aggiungi elementi alla lista

Una lista delle cose da fare ha bisogno di un modo per aggiungere elementi, quindi iniziamo. In `app.component.ts`, aggiungi il seguente metodo alla classe dopo l'array `allItems`:

```ts
export class AppComponent {
  // …
  addItem(description: string) {
    if (!description) return;

    this.allItems.unshift({
      description,
      done: false,
    });
  }
  // …
}
```

Il metodo `addItem()` prende un elemento fornito dall'utente e lo aggiunge all'array quando l'utente clicca sul pulsante **Add**. Il metodo `addItem()` utilizza il metodo array `unshift()` per aggiungere un nuovo elemento all'inizio dell'array e in cima alla lista. Potresti alternativamente usare `push()`, che aggiungerebbe il nuovo elemento alla fine dell'array e in fondo alla lista.

Per usare il metodo `addItem()`, modifica l'HTML nel modello AppComponent. In `app.component.html`, sostituisci l'`<h2>` con il seguente:

```html
<label for="addItemInput">What would you like to do today?</label>

<input
  #newItem
  placeholder="add an item"
  (keyup.enter)="addItem(newItem.value); newItem.value = ''"
  class="lg-text-input"
  id="addItemInput" />

<button class="btn-primary" (click)="addItem(newItem.value)">Add</button>
```

Nell'HTML sopra, `#newItem` è una variabile di modello. La variabile di modello in questo caso utilizza l'elemento `<input>` come suo valore. Le variabili di modello possono essere riferite ovunque nel modello del componente.

Quando l'utente digita un nuovo elemento nel campo `<input>` e preme **Enter**, il metodo `addItem()` aggiunge il valore all'array `allItems`. Premendo il tasto **Enter** si resetta anche il valore di `<input>` a una stringa vuota. La variabile di modello `#newItem` viene utilizzata per accedere al valore dell'elemento `<input>` nel modello. Invece di premere il tasto **Enter**, l'utente può anche cliccare sul pulsante **Add**, che chiama lo stesso metodo `addItem()`.

## Riepilogo

A questo punto dovresti avere il tuo elenco di base delle cose da fare visualizzato nel tuo browser. È un ottimo progresso! Ovviamente, abbiamo ancora molto da fare. Nel prossimo articolo vedremo come aggiungere un po' di stile alla nostra applicazione.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Angular_getting_started","Learn_web_development/Core/Frameworks_libraries/Angular_styling", "Learn_web_development/Core/Frameworks_libraries")}}
