---
title: Creare un componente di articolo
slug: Learn_web_development/Core/Frameworks_libraries/Angular_item_component
l10n:
  sourceCommit: 759102220c07fb140b3e06971cd5981d8f0f134f
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Angular_styling","Learn_web_development/Core/Frameworks_libraries/Angular_filtering", "Learn_web_development/Core/Frameworks_libraries")}}

I componenti offrono un modo per organizzare la sua applicazione. Questo articolo la guida nella creazione di un componente per gestire gli elementi individuali nella lista, e nell'aggiungere funzionalità di controllo, modifica e cancellazione. Il modello di evento Angular è trattato qui.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i listati di base di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>,
        e conoscenze del
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
          >terminale/linea di comando</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare di più sui componenti, incluso come funzionano gli eventi per gestire
        gli aggiornamenti. Aggiungere funzionalità di controllo, modifica e cancellazione.
      </td>
    </tr>
  </tbody>
</table>

## Creare il nuovo componente

Alla linea di comando, creare un componente chiamato `item` con il seguente comando CLI:

```bash
ng generate component item
```

Il comando `ng generate component` crea un componente e una cartella con il nome specificato.
Qui, il nome della cartella e del componente è `item`.
Può trovare la directory `item` all'interno della cartella `app`:

```plain
src/app/item
├── item.component.css
├── item.component.html
├── item.component.spec.ts
└── item.component.ts
```

Proprio come con il `AppComponent`, il `ItemComponent` è composto dai seguenti file:

- `item.component.html` per l'HTML
- `item.component.ts` per la logica
- `item.component.css` per gli stili
- `item.component.spec.ts` per testare il componente

Può vedere un riferimento ai file HTML e CSS nei metadati del decorator `@Component()` in `item.component.ts`.

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

## Aggiungere HTML per il ItemComponent

Il `ItemComponent` può svolgere il compito di fornire all'utente un modo per spuntare come completati, modificarli o cancellarli.

Aggiungere markup per gestire gli articoli sostituendo il contenuto segnaposto in `item.component.html` con il seguente:

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

Il primo input è una casella di controllo così che gli utenti possano spuntare gli articoli quando un articolo è completo.
Le doppie parentesi graffe, `\{{}}`, nel `<label>` della casella di controllo indicano l'interpolazione di Angular.
Angular utilizza `\{{item.description}}` per recuperare la descrizione dell'attuale `item` dall'array `items`.
La sezione successiva spiega in dettaglio come i componenti condividono i dati.

I due pulsanti successivi per la modifica e la cancellazione dell'articolo corrente sono all'interno di un `<div>`.
Su questo `<div>` è presente un blocco `@if` che può usare per rendere parti di un template basate su una condizione.
Questo `@if` significa che se `editable` è `false`, questo `<div>` è renderizzato nel template. Se `editable` è `true`, Angular rimuove questo `<div>` dal DOM.

```html
@if (!editable) {
<div class="btn-wrapper">
  <button class="btn" (click)="editable = !editable">Edit</button>
  <button class="btn btn-warn" (click)="remove.emit()">Delete</button>
</div>
}
```

Quando un utente clicca sul pulsante **Modifica**, `editable` diventa vero, il che rimuove questo `<div>` e i suoi figli dal DOM.
Se, invece di cliccare su **Modifica**, un utente clicca su **Cancella**, il `ItemComponent` genera un evento che notifica all'`AppComponent` la cancellazione.

Un `@if` è presente anche sul prossimo `<div>`, ma è impostato su un valore `editable` di `true`.
In questo caso, se `editable` è `true`, Angular mette il `<div>` e i suoi elementi figli `<input>` e `<button>` nel DOM.

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

Con `[value]="item.description"`, il valore del `<input>` è legato alla `description` dell'elemento corrente.
Questo collegamento fa sì che la `description` dell'articolo sia il valore del `<input>`.
Quindi se la `description` è `mangiare`, la `description` è già nel `<input>`.
In questo modo, quando l'utente modifica l'articolo, il valore del `<input>` è già `mangiare`.

La variabile di template, `#editedItem`, sul `<input>` significa che Angular memorizza qualsiasi cosa un utente scriva in questo `<input>` in una variabile chiamata `editedItem`.
L'evento `keyup` chiama il metodo `saveItem()` e passa il valore `editedItem` se l'utente sceglie di premere invio invece di cliccare **Salva**.

Quando un utente clicca sul pulsante **Annulla**, `editable` cambia in `false`, il che rimuove l'input e i pulsanti per la modifica dal DOM.
Quando `editable` è `false`, Angular rimette nel DOM `<div>` con i pulsanti **Modifica** e **Cancella**.

Cliccando il pulsante **Salva** chiama il metodo `saveItem()`.
Il metodo `saveItem()` prende il valore dall'elemento `#editedItem` e modifica la `description` dell'articolo sulla stringa `editedItem.value`.

## Preparare l'AppComponent

Nella sezione successiva, aggiungerà codice che si basa sulla comunicazione tra l'`AppComponent` e il `ItemComponent`.
Aggiunga la seguente riga vicino alla cima del file `app.component.ts` per importare `Item`:

```ts
import { Item } from "./item";
import { ItemComponent } from "./item/item.component";
```

Poi, configuri l'AppComponent aggiungendo quanto segue alla stessa classe di file:

```ts
export class AppComponent {
  // …
  remove(item: Item) {
    this.allItems.splice(this.allItems.indexOf(item), 1);
  }
  // …
}
```

Il metodo `remove()` utilizza il metodo JavaScript `Array.splice()` per rimuovere un articolo all'`indexOf` dell'articolo rilevante.
In parole semplici, questo significa che il metodo `splice()` rimuove l'articolo dall'array.
Per ulteriori informazioni sul metodo `splice()`, consultare la [documentazione di `Array.prototype.splice()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Array/splice).

## Aggiungere logica a ItemComponent

Per utilizzare l'interfaccia utente del `ItemComponent`, deve aggiungere logica al componente come funzioni e metodi per il passaggio di dati in entrata e in uscita.
In `item.component.ts`, modifichi gli import JavaScript come segue:

```ts
import { Component, Input, Output, EventEmitter } from "@angular/core";
import { CommonModule } from "@angular/common";
import { Item } from "../item";
```

L'aggiunta di `Input`, `Output` e `EventEmitter` permette al `ItemComponent` di condividere dati con `AppComponent`.
Importando `Item`, il `ItemComponent` può comprendere cosa sia un `item`.
Può aggiornare il `@Component` per utilizzare il [`CommonModule`](https://angular.dev/api/common/CommonModule) in `app/item/item.component.ts` così da poter utilizzare i blocchi `@if`:

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

Scendendo in `item.component.ts`, sostituisca la classe `ItemComponent` generata con la seguente:

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

La proprietà `editable` aiuta a cambiare sezione del template dove un utente può modificare un articolo.
`editable` è la stessa proprietà nell'HTML come nella dichiarazione `@if`, `@if(editable)`.
Quando si usa una proprietà nel template, si deve anche dichiararla nella classe.

`@Input()`, `@Output()`, e `EventEmitter` facilitano la comunicazione tra i suoi due componenti.
Un `@Input()` serve come porta per far entrare i dati nel componente, e un `@Output()` funge da porta per far uscire i dati dal componente.
Un `@Output()` deve essere di tipo `EventEmitter`, in modo che un componente possa generare un evento quando ci sono dati pronti da condividere con un altro componente.

> [!NOTE]
> Il `!` nella dichiarazione della proprietà della classe è chiamato [definite assignment assertion](https://www.typescriptlang.org/docs/handbook/2/classes.html#--strictpropertyinitialization). Questo operatore comunica a TypeScript che il campo `item` è sempre inizializzato e non `undefined`, anche quando TypeScript non può dedurlo dalla definizione del costruttore. Se questo operatore non è incluso nel suo codice e si hanno impostazioni di compilazione TypeScript rigorose, l'applicazione non riuscirà a compilare.

Utilizzi `@Input()` per specificare che il valore di una proprietà può provenire dall'esterno del componente.
Utilizzi `@Output()` in combinazione con `EventEmitter` per specificare che il valore di una proprietà può lasciare il componente così che un altro componente possa ricevere quei dati.

Il metodo `saveItem()` prende come argomento una `description` di tipo `string`.
La `description` è il testo che l'utente inserisce nel `<input>` HTML quando modifica un articolo nella lista.
Questa `description` è la stessa stringa del `<input>` con la variabile di template `#editedItem`.

Se l'utente non inserisce un valore ma clicca su **Salva**, `saveItem()` non restituisce nulla e non aggiorna la `description`.
Se non c'era questa condizione `if`, l'utente potrebbe cliccare su **Salva** senza nulla nel `<input>` HTML, e la `description` diventerebbe una stringa vuota.

Se un utente inserisce del testo e clicca salva, `saveItem()` imposta `editable` a false, il che fa sì che il `@if` nel template rimuova la funzione di modifica e renda nuovamente disponibili i pulsanti **Modifica** e **Cancella**.

Anche se l'applicazione dovrebbe compilare a questo punto, ha bisogno di usare il `ItemComponent` in `AppComponent` così può vedere le nuove funzionalità nel browser.

## Utilizzare ItemComponent in AppComponent

Includere un componente all'interno di un altro nel contesto di una relazione genitore-figlio le dà la flessibilità di usare componenti ovunque ne abbia bisogno.

L'`AppComponent` funge da guscio per l'applicazione dove può includere altri componenti.

Per utilizzare il `ItemComponent` in `AppComponent`, inserisca il selettore `ItemComponent` nel template `AppComponent`.
Angular specifica il selettore di un componente nei metadati del decorator `@Component()`.
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

Per utilizzare il selettore `ItemComponent` all'interno dell'`AppComponent`, aggiunga l'elemento `<app-item>`, che corrisponde al selettore definito per la classe del componente, in `app.component.html`.
Sostituisca l'attuale lista non ordinata `<ul>` in `app.component.html` con la seguente versione aggiornata:

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

Modifichi gli `imports` in `app.component.ts` per includere `ItemComponent` così come `CommonModule`:

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

La sintassi a doppie parentesi graffe, `\{{}}`, nel `<h2>` interpola la lunghezza dell'array `items` e visualizza il numero.

Lo `<span>` nel `<h2>` utilizza un `@if` e `@else` per determinare se il `<h2>` dovrebbe dire "articolo" o "articoli".
Se c'è solo un singolo articolo nella lista, lo `<span>` mostra "articolo".
Altrimenti, se la lunghezza dell'array `items` è diversa da `1`, lo `<span>` mostra "articoli".

L'`@for` - blocco di controllo di flusso di Angular, usato per iterare su tutti gli articoli nell'array `items`.
L'`@for` di Angular, come `@if`, è un altro blocco che aiuta a cambiare la struttura del DOM scrivendo meno codice.
Per ogni `item`, Angular ripete il `<li>` e tutto ciò che è al suo interno, il che include `<app-item>`.
Questo significa che per ogni articolo nell'array, Angular crea un'altra istanza di `<app-item>`.
Per qualsiasi numero di articoli nell'array, Angular creerebbe quel numero di elementi `<li>`.

Può avvolgere altri elementi come `<div>`, `<span>`, o `<p>` all'interno di un blocco `@for`.

L'`AppComponent` ha un metodo `remove()` per rimuovere l'articolo, che è vincolato alla proprietà `remove` nel `ItemComponent`.
La proprietà `item` nelle parentesi quadre, `[]`, lega il valore di `i` tra l'`AppComponent` e il `ItemComponent`.

Ora dovrebbe essere in grado di modificare e eliminare articoli dalla lista.
Quando aggiunge o elimina articoli, il conteggio degli articoli dovrebbe cambiare.
Per rendere la lista più user-friendly, aggiunga alcuni stili al `ItemComponent`.

## Aggiungere stili a ItemComponent

Può utilizzare il foglio di stile di un componente per aggiungere stili specifici a quel componente.
Il seguente CSS aggiunge stili di base, flexbox per i pulsanti e caselle di controllo personalizzate.

Incolli i seguenti stili in `item.component.css`.

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

Ora dovrebbe avere un'applicazione Angular di lista di cose da fare stilizzata che può aggiungere, modificare e rimuovere articoli.
Il passo successivo è aggiungere il filtraggio in modo da poter visualizzare articoli che soddisfano criteri specifici.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Angular_styling","Learn_web_development/Core/Frameworks_libraries/Angular_filtering", "Learn_web_development/Core/Frameworks_libraries")}}
