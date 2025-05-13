---
title: Iniziare con la nostra app Angular della lista di cose da fare
slug: Learn_web_development/Core/Frameworks_libraries/Angular_todo_list_beginning
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Angular_getting_started","Learn_web_development/Core/Frameworks_libraries/Angular_styling", "Learn_web_development/Core/Frameworks_libraries")}}

A questo punto, siamo pronti per iniziare a creare la nostra applicazione della lista di cose da fare utilizzando Angular. L'applicazione finale mostrerà un elenco di elementi da fare e includerà funzionalità di modifica, eliminazione e aggiunta. In questo articolo conoscerà la struttura della sua applicazione e lavorerà fino a visualizzare un elenco di base di elementi da fare.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con le lingue principali
        <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
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
        Creare la struttura di base della nostra app, farla visualizzare un elenco di elementi da fare e comprendere concetti fondamentali di Angular come la struttura dei componenti, la condivisione dei dati tra componenti, e la creazione di contenuti tramite ciclo.
      </td>
    </tr>
  </tbody>
</table>

## La struttura dell'applicazione della lista di cose da fare

Come qualsiasi applicazione web, un'applicazione Angular ha un `index.html` come punto di ingresso. Il `index.html` è in realtà il modello HTML di livello superiore dell'app:

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

All'interno del tag `<body>`, Angular utilizza un elemento speciale — `<app-root>` — per inserire il suo componente principale, che a sua volta include altri componenti che lei crea.
Generalmente, non ha bisogno di modificare il `index.html`, e la maggior parte del suo lavoro si concentra in aree specializzate della sua applicazione chiamate componenti.

### Organizzare la sua applicazione con i componenti

I componenti sono un elemento fondamentale delle applicazioni Angular.
Questa applicazione della lista di cose da fare ha due componenti — un componente come fondamento della sua applicazione e un componente per gestire gli elementi da fare.

Ogni componente è composto da una classe TypeScript, HTML e CSS.
TypeScript viene traspilato, o convertito, in JavaScript, il che significa che la sua applicazione finisce per essere in JavaScript normale ma ha la comodità di utilizzare le funzionalità estese e la sintassi semplificata di TypeScript.

### Flusso di controllo con blocchi @if e @for

Questo tutorial copre due importanti [blocchi di controllo del flusso](https://angular.dev/guide/templates/control-flow) di Angular che indicano al framework quando e come i suoi modelli dovrebbero essere resi.
Il primo blocco coperto da questo tutorial è il blocco [`@for`](https://angular.dev/api/core/@for) che itera su una raccolta e rende ripetutamente il contenuto di un blocco.

Il secondo blocco che imparerà in questo tutorial è [`@if`](https://angular.dev/api/core/@if).
Può utilizzare `@if` per visualizzare contenuti in base a una condizione.
Ad esempio, se un utente clicca un bottone "modifica", può mostrare gli elementi utilizzati per modificare un elemento.
Se un utente clicca su "annulla", può rimuovere gli elementi utilizzati per la modifica.

### Condivisione di dati tra componenti

In questa applicazione della lista di cose da fare, configura i suoi componenti per condividere dati.
Per aggiungere nuovi elementi alla lista di cosa da fare, il componente principale deve inviare il nuovo elemento al secondo componente.
Questo secondo componente gestisce gli elementi e si occupa di modificare, contrassegnare come fatto e eliminare singoli elementi.

Condivide i dati tra i componenti Angular con decoratori speciali chiamati `@Input()` e `@Output()`.
Utilizza questi decoratori per specificare che certe proprietà consentono il passaggio di dati in entrata o in uscita da un componente.
Per utilizzare `@Output()`, deve generare un evento in un componente in modo che l'altro componente sappia che ci sono dati disponibili.

## Definire Item

Nella directory `app`, crei un nuovo file denominato `item.ts` con il seguente contenuto:

```ts
export interface Item {
  description: string;
  done: boolean;
}
```

Non utilizzerà questo file fino a [più avanti](/it/docs/Learn_web_development/Core/Frameworks_libraries/Angular_item_component#add_logic_to_itemcomponent), ma è un buon momento per conoscere e registrare la sua conoscenza di cosa sia un `item`. L'interfaccia `Item` crea un modello oggetto `item` in modo che la sua applicazione capisca cosa sia un `item`. Per questa lista di cose da fare, un `item` è un oggetto che ha una descrizione e può essere segnato come fatto.

## Aggiungere logica al AppComponent

Ora che sa cos'è un `item`, può fornire alla sua applicazione alcuni elementi aggiungendoli all'app.
In `app.component.ts`, sostituisca il contenuto con il seguente:

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

Le prime due righe sono importazioni JavaScript. In questo caso stanno importando le librerie Angular.
Il decoratore `@Component()` specifica i metadati riguardanti il `AppComponent`.
Ecco ulteriori informazioni sui metadati che stiamo utilizzando:

- [`standalone`](https://angular.dev/api/core/Component#standalone): Descrive se il componente richiede o meno un [NgModule](https://angular.dev/guide/ngmodules#the-basic-ngmodule).
  La sua app gestirà direttamente le dipendenze del modello (componenti, direttive, ecc.) utilizzando le importazioni quando è autonoma.
- [`selector`](https://angular.dev/api/core/Directive#selector): Indica il selettore CSS che utilizza in un modello per posizionare questo componente. Qui è `'app-root'`.
  Nel `index.html`, all'interno del tag `body`, l'Angular CLI ha aggiunto `<app-root></app-root>` quando ha generato la sua applicazione.
  Utilizzi tutti i selettori di componenti nello stesso modo, aggiungendoli ad altri modelli HTML di componenti.
- [`templateUrl`](https://angular.dev/api/core/Component#templateUrl): Specifica il file HTML da associare a questo componente.
  Qui è, `'./app.component.html'`,
- [`styleUrl`](https://angular.dev/api/core/Component#styleUrl): Fornisce la posizione e il nome del file per i suoi stili che si applicano specificamente a questo componente. Qui è `'./app.component.css'`.
- [`imports`](https://angular.dev/api/core/Component#imports): Le permette di specificare le dipendenze del componente che possono essere utilizzate all'interno del suo modello.

La proprietà `filter` è di tipo `union`, il che significa che `filter` potrebbe avere il valore `all`, `active`, o `done`.
Con il tipo `union`, se commette un errore di battitura nel valore che assegna alla proprietà `filter`, TypeScript la avvisa in modo che possa individuare il bug precocemente.
Questa guida le mostra come aggiungere il filtraggio in un passaggio successivo, ma può anche utilizzare un filtro per mostrare l'elenco predefinito di tutti gli elementi.

L'array `allItems` contiene gli elementi da fare e se sono completati.
Il primo elemento, `eat`, ha un valore `done` pari a true.

Il getter, `get items()`, recupera gli elementi dall'array `allItems` se il `filter` è uguale a `all`. Altrimenti, `get items()` restituisce gli elementi completati o in sospeso a seconda di come l'utente filtra la visualizzazione.
Il getter stabilisce anche il nome dell'array come `items`, che utilizzerà nella sezione successiva.

## Aggiungere HTML al modello AppComponent

Per vedere l'elenco degli elementi nel browser, sostituisca il contenuto di `app.component.html` con il seguente HTML:

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

Il `<li>` si trova all'interno di un blocco `@for` che itera sugli elementi nell'array `items`.
Per ogni elemento, viene creato un nuovo `<li>`.
Le doppie parentesi graffe che contengono `item.description` istruiscono Angular a popolare ogni `<li>` con il testo della descrizione di ciascun elemento.

La parola chiave `track` nel blocco `@for` di Angular aiuta Angular a identificare quali elementi in un array sono cambiati, sono stati aggiunti o eliminati.
Questo rende più facile e veloce per Angular aggiornare il DOM quando l'array viene modificato.

Nel browser, dovrebbe vedere l'elenco degli elementi come segue:

```plain
My To Do List
What would you like to do today?

* eat
* sleep
* play
* laugh
```

## Aggiungere elementi all'elenco

Una lista di cose da fare ha bisogno di un modo per aggiungere elementi, quindi iniziamo.
In `app.component.ts`, aggiunga il seguente metodo alla classe dopo l'array `allItems`:

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

Il metodo `addItem()` prende un elemento che l'utente fornisce e lo aggiunge all'array quando l'utente clicca sul pulsante **Aggiungi**.
Il metodo `addItem()` utilizza il metodo dell'array `unshift()` per aggiungere un nuovo elemento all'inizio dell'array e all'inizio della lista.
Può alternativamente utilizzare `push()`, che aggiungerebbe il nuovo elemento alla fine dell'array e in fondo alla lista.

Per utilizzare il metodo `addItem()`, modifichi l'HTML del modello `AppComponent`.
In `app.component.html`, sostituisca l'`<h2>` con il seguente:

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

Nel codice HTML sopra, `#newItem` è una variabile template. La variabile template in questo caso utilizza l'elemento `<input>` come suo valore. Le variabili template possono essere riferite ovunque nel modello del componente.

Quando l'utente digita un nuovo elemento nel campo `<input>` e preme **Invio**, il metodo `addItem()` aggiunge il valore all'array `allItems`.
Premere il tasto **Invio** reimposta anche il valore dell'`<input>` a una stringa vuota. La variabile template `#newItem` è utilizzata per accedere al valore dell'elemento `<input>` nel modello.
Invece di premere il tasto **Invio**, l'utente può anche cliccare sul pulsante **Aggiungi**, che chiama lo stesso metodo `addItem()`.

## Sommario

Adesso dovrebbe avere la sua lista di cose da fare di base visualizzata nel browser. Questo è un grande progresso! Ovviamente, c'è ancora molto da fare. Nel prossimo articolo vedremo come aggiungere un po' di stile alla nostra applicazione.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Angular_getting_started","Learn_web_development/Core/Frameworks_libraries/Angular_styling", "Learn_web_development/Core/Frameworks_libraries")}}
