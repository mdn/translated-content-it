---
title: Creazione di un componente per gli elementi
slug: Learn_web_development/Core/Frameworks_libraries/Angular_item_component
l10n:
  sourceCommit: 759102220c07fb140b3e06971cd5981d8f0f134f
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Angular_styling","Learn_web_development/Core/Frameworks_libraries/Angular_filtering", "Learn_web_development/Core/Frameworks_libraries")}}

I componenti offrono un modo per organizzare la tua applicazione. Questo articolo ti guida nella creazione di un componente per gestire i singoli elementi nella lista, e nell'aggiunta della funzionalità di spunta, modifica e rimozione. Qui viene trattato il modello di eventi di Angular.

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
          >terminale/riga di comando</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare di più sui componenti, incluso come funzionano gli eventi per gestire
        gli aggiornamenti. Aggiungere funzionalità di spunta, modifica e rimozione.
      </td>
    </tr>
  </tbody>
</table>

## Creazione del nuovo componente

Nella riga di comando, crea un componente chiamato `item` con il seguente comando CLI:

```bash
ng generate component item
```

Il comando `ng generate component` crea un componente e una cartella con il nome specificato.
Qui, il nome della cartella e del componente è `item`.
Puoi trovare la directory `item` all'interno della cartella `app`:

```plain
src/app/item
├── item.component.css
├── item.component.html
├── item.component.spec.ts
└── item.component.ts
```

Proprio come con `AppComponent`, `ItemComponent` è composto dai seguenti file:

- `item.component.html` per HTML
- `item.component.ts` per la logica
- `item.component.css` per gli stili
- `item.component.spec.ts` per testare il componente

Puoi vedere un riferimento ai file HTML e CSS nei metadati del decoratore `@Component()` in `item.component.ts`.

```ts
@Component({
  selector: "app-item",
  standalone: true,
  imports: [],
  templateUrl: "./item.component.html",
  styleUrl: "./item.component.css",
})
export class ItemComponent {
  // …
}
```

## Aggiungi HTML per ItemComponent

`ItemComponent` può occuparsi del compito di fornire all'utente un modo per spuntare gli elementi, modificarli o eliminarli.

Aggiungi il markup per gestire gli elementi sostituendo il contenuto segnaposto in `item.component.html` con il seguente:

```html
<div class="item">
  <input
    [id]="item.description"
    type="checkbox"
    (change)="item.done = !item.done"
    [checked]="item.done" />
  <label [for]="item.description">\{{item.description}}</label>

  @if (!editable) {
  <div class="btn-wrapper">
    <button class="btn" (click)="editable = !editable">Edit</button>
    <button class="btn btn-warn" (click)="remove.emit()">Delete</button>
  </div>
  }

  <!-- This section shows only if user clicks Edit button -->
  @if (editable) {
  <div>
    <input
      class="sm-text-input"
      placeholder="edit item"
      [value]="item.description"
      #editedItem
      (keyup.enter)="saveItem(editedItem.value)" />

    <div class="btn-wrapper">
      <button class="btn" (click)="editable = !editable">Cancel</button>
      <button class="btn btn-save" (click)="saveItem(editedItem.value)">
        Save
      </button>
    </div>
  </div>
  }
</div>
```

Il primo input è un checkbox affinché gli utenti possano spuntare gli elementi quando un elemento è completo.
Le doppie parentesi graffe, `\{{}}`, nel `<label>` per il checkbox rappresentano l'interpolazione di Angular.
Angular usa `\{{item.description}}` per recuperare la descrizione dell'attuale `item` dall'array `items`.
La sezione successiva spiega in dettaglio come i componenti condividono i dati.

I due pulsanti successivi per modificare ed eliminare l'elemento corrente si trovano all'interno di un `<div>`.
Su questo `<div>` c'è un blocco `@if` che puoi usare per rendere parti di un template basate su una condizione.
Questo `@if` significa che se `editable` è `false`, questo `<div>` viene reso nel template. Se `editable` è `true`, Angular rimuove questo `<div>` dal DOM.

```html
@if (!editable) {
<div class="btn-wrapper">
  <button class="btn" (click)="editable = !editable">Edit</button>
  <button class="btn btn-warn" (click)="remove.emit()">Delete</button>
</div>
}
```

Quando un utente clicca sul pulsante **Edit**, `editable` diventa true, il che rimuove questo `<div>` e i suoi figli dal DOM.
Se, invece di cliccare su **Edit**, un utente clicca su **Delete**, `ItemComponent` solleva un evento che notifica `AppComponent` della rimozione.

Anche il `<div>` successivo ha un `@if`, ma è impostato su un valore `editable` di `true`.
In questo caso, se `editable` è `true`, Angular inserisce nel DOM il `<div>` e i suoi elementi figli `<input>` e `<button>`.

```html
<!-- This section shows only if user clicks Edit button -->
@if (editable) {
<div>
  <input
    class="sm-text-input"
    placeholder="edit item"
    [value]="item.description"
    #editedItem
    (keyup.enter)="saveItem(editedItem.value)" />

  <div class="btn-wrapper">
    <button class="btn" (click)="editable = !editable">Cancel</button>
    <button class="btn btn-save" (click)="saveItem(editedItem.value)">
      Save
    </button>
  </div>
</div>
}
```

Con `[value]="item.description"`, il valore del `<input>` è collegato alla `description` dell'attuale elemento.
Questo binding rende la `description` dell'elemento il valore dell'`<input>`.
Quindi, se la `description` è `mangiare`, la `description` è già nel `<input>`.
In questo modo, quando l'utente modifica l'elemento, il valore dell'`<input>` è già `mangiare`.

La variabile del template, `#editedItem`, sull'`<input>` significa che Angular memorizza qualunque cosa un utente digiti in questo `<input>` in una variabile chiamata `editedItem`.
L'evento `keyup` chiama il metodo `saveItem()` e passa il valore di `editedItem` se l'utente sceglie di premere invio invece di cliccare su **Save**.

Quando un utente clicca sul pulsante **Cancel**, `editable` viene impostato nuovamente su `false`, il che rimuove l'input e i pulsanti per la modifica dal DOM.
Quando `editable` è `false`, Angular riposiziona nel DOM il `<div>` con i pulsanti **Edit** e **Delete**.

Cliccando sul pulsante **Save** si chiama il metodo `saveItem()`.
Il metodo `saveItem()` prende il valore dall'elemento `#editedItem` e cambia la `description` dell'elemento in `editedItem.value` stringa.

## Prepara l'AppComponent

Nella sezione successiva, aggiungerai codice che si basa sulla comunicazione tra `AppComponent` e `ItemComponent`.
Aggiungi la seguente riga vicino alla cima del file `app.component.ts` per importare `Item`:

```ts
import { Item } from "./item";
import { ItemComponent } from "./item/item.component";
```

Poi, configura AppComponent aggiungendo il seguente codice alla classe dello stesso file:

```ts
export class AppComponent {
  // …
  remove(item: Item) {
    this.allItems.splice(this.allItems.indexOf(item), 1);
  }
  // …
}
```

Il metodo `remove()` utilizza il metodo JavaScript `Array.splice()` per rimuovere un elemento all'`indexOf` dell'elemento rilevante.
In termini semplici, questo significa che il metodo `splice()` rimuove l'elemento dall'array.
Per ulteriori informazioni sul metodo `splice()`, consulta la [documentazione di `Array.prototype.splice()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Array/splice).

## Aggiungi logica a ItemComponent

Per usare l'interfaccia utente di `ItemComponent`, devi aggiungere logica al componente come funzioni e modi per il flusso dei dati in entrata e in uscita.
In `item.component.ts`, modifica le importazioni JavaScript come segue:

```ts
import { Component, Input, Output, EventEmitter } from "@angular/core";
import { CommonModule } from "@angular/common";
import { Item } from "../item";
```

L'aggiunta di `Input`, `Output` e `EventEmitter` consente a `ItemComponent` di condividere dati con `AppComponent`.
Importando `Item`, il `ItemComponent` può capire cosa sia un `item`.
Puoi aggiornare il `@Component` per usare [`CommonModule`](https://angular.dev/api/common/CommonModule) in `app/item/item.component.ts` così da poter usare i blocchi `@if`:

```ts
@Component({
  selector: "app-item",
  standalone: true,
  imports: [CommonModule],
  templateUrl: "./item.component.html",
  styleUrl: "./item.component.css",
})
export class ItemComponent {
  // …
}
```

Più in basso in `item.component.ts`, sostituisci la classe generata `ItemComponent` con il seguente codice:

```ts
export class ItemComponent {
  editable = false;

  @Input() item!: Item;
  @Output() remove = new EventEmitter<Item>();

  saveItem(description: string) {
    if (!description) return;

    this.editable = false;
    this.item.description = description;
  }
}
```

La proprietà `editable` aiuta a gestire una sezione del template dove un utente può modificare un elemento.
`editable` è la stessa proprietà nell'HTML come nell'istruzione `@if`, `@if(editable)`.
Quando usi una proprietà nel template, devi anche dichiararla nella classe.

`@Input()`, `@Output()`, e `EventEmitter` facilitano la comunicazione tra i due componenti.
Un `@Input()` serve come un varco per i dati che entrano nel componente, e un `@Output()` agisce come un varco per i dati in uscita dal componente.
Un `@Output()` deve essere di tipo `EventEmitter`, affinché un componente possa sollevare un evento quando ci sono dati pronti per essere condivisi con un altro componente.

> [!NOTE]
> Il `!` nella dichiarazione della proprietà della classe è chiamato [definite assignment assertion](https://www.typescriptlang.org/docs/handbook/2/classes.html#--strictpropertyinitialization). Questo operatore indica a TypeScript che il campo `item` è sempre inizializzato e non `undefined`, anche quando TypeScript non può determinarlo dall'implementazione del costruttore. Se questo operatore non è incluso nel tuo codice e hai impostazioni di compilazione TypeScript rigorose, l'app non riuscirà a compilare.

Usa `@Input()` per specificare che il valore di una proprietà può provenire dall'esterno del componente.
Usa `@Output()` in combinazione con `EventEmitter` per specificare che il valore di una proprietà può uscire dal componente affinché un altro componente possa ricevere tale dato.

Il metodo `saveItem()` prende come argomento una `description` di tipo `stringa`.
La `description` è il testo che l'utente inserisce nell'HTML `<input>` durante la modifica di un elemento nell'elenco.
Questa `description` è la stessa stringa proveniente dall'`<input>` con la variabile di template `#editedItem`.

Se l'utente non inserisce un valore ma clicca su **Save**, `saveItem()` non restituisce nulla e non aggiorna la `description`.
Se non avessi questa istruzione `if`, l'utente potrebbe cliccare su **Save** senza nulla nell'HTML `<input>`, e la `description` diventerebbe una stringa vuota.

Se un utente inserisce testo e clicca su save, `saveItem()` imposta `editable` su false, il che causa l'`@if` nel template di rimuovere la funzione di modifica e mostrare nuovamente i pulsanti **Edit** e **Delete**.

Anche se l'applicazione dovrebbe compilare a questo punto, devi usare `ItemComponent` in `AppComponent` così da poter vedere le nuove funzionalità nel browser.

## Usa ItemComponent in AppComponent

Includere un componente all'interno di un altro nel contesto di una relazione genitore-figlio ti dà la flessibilità di usare i componenti ovunque ne hai bisogno.

`AppComponent` serve come shell per l'applicazione dove puoi includere altri componenti.

Per usare `ItemComponent` in `AppComponent`, inserisci il selettore di `ItemComponent` nel template di `AppComponent`.
Angular specifica il selettore di un componente nei metadati del decoratore `@Component()`.
In questo esempio, abbiamo definito il selettore come `app-item`:

```ts
@Component({
  selector: "app-item",
  // …
})
export class ItemComponent {
  // …
}
```

Per usare il selettore `ItemComponent` all'interno di `AppComponent`, aggiungi l'elemento `<app-item>`, che corrisponde al selettore che hai definito per la classe del componente in `app.component.html`.
Sostituisci l'attuale lista non ordinata `<ul>` in `app.component.html` con la seguente versione aggiornata:

```html
<h2>
  \{{items.length}}
  <span> @if (items.length === 1) { item } @else { items } </span>
</h2>

<ul>
  @for (item of items; track item.description) {
  <li>
    <app-item (remove)="remove(item)" [item]="item"></app-item>
  </li>
  }
</ul>
```

Cambia le `imports` in `app.component.ts` per includere `ItemComponent` così come `CommonModule`:

```ts
@Component({
  standalone: true,
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrl: "./app.component.css",
  imports: [CommonModule, ItemComponent],
})
export class AppComponent {
  // …
}
```

La sintassi delle doppie parentesi graffe, `\{{}}`, nel `<h2>` interpolano la lunghezza dell'array `items` e visualizzano il numero.

Il `<span>` nel `<h2>` usa un `@if` e `@else` per determinare se il `<h2>` dovrebbe dire "item" o "items".
Se c'è solo un singolo elemento nell'elenco, il `<span>` mostra "item".
Diversamente, se la lunghezza dell'array `items` è qualsiasi cosa diversa da `1`, il `<span>` mostra "items".

L'`@for` - blocco di controllo del flusso di Angular, usato per iterare su tutti gli elementi nell'array `items`.
L'`@for` di Angular, come `@if`, è un altro blocco che ti aiuta a cambiare la struttura del DOM scrivendo meno codice.
Per ogni `item`, Angular ripete il `<li>` e tutto ciò che è all'interno, il che include `<app-item>`.
Questo significa che per ogni elemento nell'array, Angular crea un'altra istanza di `<app-item>`.
Per qualsiasi numero di elementi nell'array, Angular creerebbe tanti elementi `<li>`.

Puoi avvolgere altri elementi come `<div>`, `<span>`, o `<p>` all'interno di un blocco `@for`.

`AppComponent` ha un metodo `remove()` per rimuovere l'elemento, che è collegato alla proprietà `remove` in `ItemComponent`.
La proprietà `item` tra le parentesi quadre, `[]`, lega il valore di `i` tra `AppComponent` e `ItemComponent`.

Ora dovresti essere in grado di modificare ed eliminare elementi dall'elenco.
Quando aggiungi o elimini elementi, il conteggio degli elementi dovrebbe cambiare.
Per rendere la lista più user-friendly, aggiungi alcuni stili a `ItemComponent`.

## Aggiungi stili a ItemComponent

Puoi usare il foglio di stile del componente per aggiungere stili specifici a quel componente.
Il seguente CSS aggiunge stili di base, flexbox per i pulsanti e checkbox personalizzati.

Incolla i seguenti stili in `item.component.css`.

```css
.item {
  padding: 0.5rem 0 0.75rem 0;
  text-align: left;
  font-size: 1.2rem;
}

.btn-wrapper {
  margin-top: 1rem;
  margin-bottom: 0.5rem;
}

.btn {
  /* menu buttons flexbox styles */
  flex-basis: 49%;
}

.btn-save {
  background-color: #000;
  color: #fff;
  border-color: #000;
}

.btn-save:hover {
  background-color: #444242;
}

.btn-save:focus {
  background-color: #fff;
  color: #000;
}

.checkbox-wrapper {
  margin: 0.5rem 0;
}

.btn-warn {
  background-color: #b90000;
  color: #fff;
  border-color: #9a0000;
}

.btn-warn:hover {
  background-color: #9a0000;
}

.btn-warn:active {
  background-color: #e30000;
  border-color: #000;
}

.sm-text-input {
  width: 100%;
  padding: 0.5rem;
  border: 2px solid #555;
  display: block;
  box-sizing: border-box;
  font-size: 1rem;
  margin: 1rem 0;
}

/* Custom checkboxes
Adapted from https://css-tricks.com/the-checkbox-hack/#custom-designed-radio-buttons-and-checkboxes */

/* Base for label styling */
[type="checkbox"]:not(:checked),
[type="checkbox"]:checked {
  position: absolute;
  left: -9999px;
}
[type="checkbox"]:not(:checked) + label,
[type="checkbox"]:checked + label {
  position: relative;
  padding-left: 1.95em;
  cursor: pointer;
}

/* checkbox aspect */
[type="checkbox"]:not(:checked) + label:before,
[type="checkbox"]:checked + label:before {
  content: "";
  position: absolute;
  left: 0;
  top: 0;
  width: 1.25em;
  height: 1.25em;
  border: 2px solid #ccc;
  background: #fff;
}

/* checked mark aspect */
[type="checkbox"]:not(:checked) + label:after,
[type="checkbox"]:checked + label:after {
  content: "\2713\0020";
  position: absolute;
  top: 0.15em;
  left: 0.22em;
  font-size: 1.3em;
  line-height: 0.8;
  color: #0d8dee;
  transition: all 0.2s;
  font-family: "Lucida Sans Unicode", "Arial Unicode MS", Arial;
}
/* checked mark aspect changes */
[type="checkbox"]:not(:checked) + label:after {
  opacity: 0;
  transform: scale(0);
}
[type="checkbox"]:checked + label:after {
  opacity: 1;
  transform: scale(1);
}

/* accessibility */
[type="checkbox"]:checked:focus + label:before,
[type="checkbox"]:not(:checked):focus + label:before {
  border: 2px dotted blue;
}
```

## Sommario

Dovresti ora avere un'applicazione Angular per lista di cose da fare stilizzata che può aggiungere, modificare e rimuovere elementi.
Il prossimo passo è aggiungere il filtraggio così da poter visualizzare elementi che soddisfano criteri specifici.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Angular_styling","Learn_web_development/Core/Frameworks_libraries/Angular_filtering", "Learn_web_development/Core/Frameworks_libraries")}}
