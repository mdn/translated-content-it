---
title: "Interattività di Ember: Funzionalità del footer, rendering condizionale"
slug: Learn_web_development/Core/Frameworks_libraries/Ember_conditional_footer
l10n:
  sourceCommit: 611edf6335e4a833a6f394d0d98b117e7b0a36bf
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Ember_interactivity_events_state","Learn_web_development/Core/Frameworks_libraries/Ember_routing", "Learn_web_development/Core/Frameworks_libraries")}}

Ora è il momento di affrontare la funzionalità del footer nella nostra app. Qui faremo in modo che il contatore delle cose da fare si aggiorni per mostrare il numero corretto di attività ancora da completare, e applicheremo correttamente lo stile alle attività completate (cioè, dove la casella è stata selezionata). Collegheremo anche il nostro pulsante "Cancella completati". Durante il percorso, apprenderemo l'uso del rendering condizionale nei nostri template.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          Al minimo, è consigliato avere familiarità con i linguaggi di base
          <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e
          possedere conoscenze del
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminale/linea di comando</a
          >.
        </p>
        <p>
          Una comprensione più profonda delle funzionalità moderne di JavaScript (come classi,
          moduli, ecc.) sarà estremamente utile, poiché Ember ne fa un ampio uso.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Continuare il nostro apprendimento sulle classi dei componenti, iniziare a vedere il
        rendering condizionale e collegare alcune delle nostre funzionalità del footer.
      </td>
    </tr>
  </tbody>
</table>

## Collegare il comportamento nel footer

Per far funzionare il footer, dobbiamo implementare le seguenti tre aree di funzionalità:

- Un contatore delle cose da fare in sospeso.
- Filtri per tutte, attive e completate.
- Un pulsante per cancellare le attività completate.

1. Poiché abbiamo bisogno di accedere al nostro servizio dal componente del footer, dobbiamo generare una classe per il footer. Inserisci il seguente comando nel terminale per farlo:

   ```bash
   ember generate component-class footer
   ```

2. Successivamente, trova il nuovo file `todomvc/app/components/footer.js` appena creato e aggiornalo come segue:

   ```ts
   import Component from "@glimmer/component";
   import { inject as service } from "@ember/service";

   export default class FooterComponent extends Component {
     @service("todo-data") todos;
   }
   ```

3. Ora dobbiamo tornare al nostro file `todo-data.js` e aggiungere alcune funzionalità che ci permetteranno di restituire il numero di attività incomplete (utile per mostrare quante ne rimangono), e cancellare le attività completate dall'elenco (che è ciò che serve alla funzionalità "Cancella completati").

   In `todo-data.js`, aggiungi il seguente getter sotto il getter `all()` esistente per definire quali sono effettivamente le cose da fare incomplete:

   ```ts
   export default class TodoDataService extends Service {
     // …
     get incomplete() {
       return this.todos.filter((todo) => !todo.isCompleted);
     }
     // …
   }
   ```

   Utilizzando il metodo [`filter()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Array/filter), stiamo chiedendo tutti gli elementi da fare dove la proprietà `isCompleted` è uguale a `false`, e poiché `isCompleted` è `@tracked` nel nostro oggetto `Todo`, questo getter verrà ricalcolato quando il valore cambia su un Oggetto nell'array.

4. Successivamente, aggiungi la seguente azione sotto l'azione `add(text)` esistente:

   ```ts
   export default class TodoDataService extends Service {
     // …
     @action
     clearCompleted() {
       this.todos = this.incomplete;
     }
     // …
   }
   ```

   Questo è piuttosto utile per cancellare le cose da fare — basta impostare l'array `todos` uguale all'elenco delle cose da fare incomplete.

5. Infine, dobbiamo utilizzare questa nuova funzionalità nel nostro template `footer.hbs`. Vai a questo file ora.
6. Prima di tutto, sostituisci questa riga:

   ```hbs
   <strong>0</strong> todos left
   ```

   Con questa, che riempie il numero di incompleti con la lunghezza dell'array `incomplete`:

   ```hbs
   <strong>\{{this.todos.incomplete.length}}</strong> todos left
   ```

7. Successivamente, sostituisci questo:

   ```hbs
   <button type="button" class="clear-completed">
   ```

   Con questo:

   ```hbs
   <button type="button" class="clear-completed" \{{on 'click' this.todos.clearCompleted}}>
   ```

Quindi ora, quando il pulsante viene cliccato, l'azione `clearCompleted()` che abbiamo aggiunto prima viene eseguita.
Tuttavia, se provi a cliccare il pulsante "Cancella Completati" ora, non sembrerà fare nulla, perché attualmente non c'è modo di "completare" una cosa da fare. Dobbiamo collegare il template `todo.hbs` al servizio, in modo che selezionando la casella di controllo pertinente cambi lo stato di ciascun todo. Lo faremo dopo.

## Il problema del singolare/plurale delle cose da fare

Quanto sopra va bene, ma abbiamo un altro piccolo problema da affrontare. L'indicatore "todos left" indica sempre "x todos left", anche quando c'è solo una cosa da fare rimasta, il che è una cattiva grammatica!

Per risolvere questo problema, dobbiamo aggiornare questa parte del template per includere del rendering condizionale. In Ember, puoi rendere condizionalmente parti del template utilizzando [contenuto condizionale](https://guides.emberjs.com/v3.18.0/components/conditional-content/); un semplice esempio di blocco appare come segue:

```hbs
\{{#if this.thingIsTrue}} Content for the block form of "if"
\{{/if}}
```

Quindi proviamo a sostituire questa parte di `footer.hbs`:

```hbs
<strong>\{{this.todos.incomplete.length}}</strong> todos left
```

con il seguente:

```hbs
<strong>\{{this.todos.incomplete.length}}</strong>
\{{#if this.todos.incomplete.length === 1}} todo
\{{else}} todos
\{{/if}} left
```

Questo tuttavia ci darà un errore — in Ember, queste semplici istruzioni if attualmente possono solo testare un valore il cui risultato è vero o falso, non un'espressione più complessa come una comparazione. Per risolvere questo problema, dovremo aggiungere un getter a `todo-data.js` per restituire il risultato di `this.incomplete.length === 1`, e poi chiamarlo nel nostro template.

Aggiungi il seguente nuovo getter a `todo-data.js`, appena sotto i getter esistenti. Nota che qui abbiamo bisogno di `this.incomplete.length`, non `this.todos.incomplete.length`, perché lo stiamo facendo all'interno del servizio, dove il getter `incomplete()` è disponibile direttamente (nel template, i contenuti del servizio sono stati resi disponibili come `todos` tramite la riga `@service("todo-data") todos;` all'interno della classe del footer, da qui `this.todos.incomplete.length`).

```ts
export default class TodoDataService extends Service {
  // …
  get todoCountIsOne() {
    return this.incomplete.length === 1;
  }
  // …
}
```

Quindi torna a `footer.hbs` e aggiorna la sezione precedente del template che abbiamo modificato al seguente:

```hbs
<strong>\{{this.todos.incomplete.length}}</strong>
\{{#if this.todos.todoCountIsOne}}todo\{{else}}todos\{{/if}} left
```

Ora salva e testa, e vedrai che viene utilizzata la corretta pluralizzazione quando hai solo un elemento presente!

Nota che questo è il modulo a blocchi di `if` in Ember; potresti anche usare la forma inline:

```hbs
\{{if this.todos.todoCountIsOne "todo" "todos"}}
```

## Completare le cose da fare

Come con gli altri componenti, abbiamo bisogno di una classe per accedere al servizio.

### Creare una classe todo

1. Esegui il seguente comando nel tuo terminale:

   ```bash
   ember generate component-class todo
   ```

2. Ora vai al file `todomvc/app/components/todo.js` creato di recente e aggiorna i contenuti in modo che appaiano come segue, per dare al componente todo accesso al servizio:

   ```ts
   import Component from "@glimmer/component";
   import { inject as service } from "@ember/service";

   export default class TodoComponent extends Component {
     @service("todo-data") todos;
   }
   ```

3. Successivamente, torna di nuovo al nostro file del servizio `todo-data.js` e aggiungi la seguente azione proprio sotto quelle precedenti, che ci permetterà di attivare lo stato di completamento per ogni cosa da fare:

   ```ts
   export default class TodoDataService extends Service {
     // …
     @action
     toggleCompletion(todo) {
       todo.isCompleted = !todo.isCompleted;
     }
     // …
   }
   ```

### Aggiornare il template per mostrare lo stato completato

Infine, modificheremo il template `todo.hbs` in modo che il valore della casella di controllo sia ora associato alla proprietà `isCompleted` sul todo, e in modo che al cambiamento venga invocato il metodo `toggleCompletion()` sul servizio todo.

1. In `todo.hbs`, trova prima la seguente riga:

   ```hbs
   <li>
   ```

   E sostituiscila con questa — noterai che qui stiamo utilizzando del contenuto condizionale per aggiungere il valore della classe se appropriato:

   ```hbs-nolint
   <li class=\{{ if @todo.isCompleted 'completed' }}>
   ```

2. Successivamente, trova la seguente riga:

   ```hbs-nolint
   <input
     aria-label="Toggle the completion of this todo"
     class="toggle"
     type="checkbox"
   >
   ```

   E sostituiscila con questa:

   ```hbs
   <input
     class="toggle"
     type="checkbox"
     aria-label="Toggle the completion of this todo"
     checked=\{{ @todo.isCompleted }}
     \{{ on 'change' (fn this.todos.toggleCompletion @todo) }}
   >
   ```

   > [!NOTE]
   > Lo snippet sopra utilizza una nuova parola chiave specifica di Ember — `fn`. `fn` permette di effettuare [applicazione parziale](https://en.wikipedia.org/wiki/Partial_application), che è simile a [`bind`](/it/docs/Web/JavaScript/Reference/Global_Objects/Function/bind), ma non cambia mai il contesto di invocazione; questo è equivalente a usare `bind` con un primo argomento `null`.

Prova a riavviare il server di sviluppo e a recarti nuovamente su `localhost:4200`, e vedrai che ora abbiamo un contatore "todos left" e un pulsante Cancella completamente operativi:

![todos being marked as complete, and cleared](todos-being-marked-completed-and-cleared.gif)

Se ti stai chiedendo perché non stiamo semplicemente effettuando il toggle sul componente, dato che la funzione è interamente autosufficiente e non ha bisogno di nulla dal servizio, allora hai assolutamente ragione a chiedertelo! Tuttavia, poiché _eventualmente_, vorremo persistere o sincronizzare tutte le modifiche all'elenco delle cose da fare al [local storage](/it/docs/Web/API/Window/localStorage) (vedi la [versione finale dell'app](https://nullvoxpopuli.github.io/ember-todomvc-tutorial/)), ha senso avere tutte le operazioni che cambiano lo stato persistente nello stesso luogo.

## Riepilogo

Abbastanza per ora. A questo punto, non solo possiamo contrassegnare le cose da fare come complete, ma possiamo anche cancellarle. Ora l'unica cosa che rimane da collegare nel footer sono i tre collegamenti di filtro: "Tutte", "Attive" e "Completate". Lo faremo nel prossimo articolo, utilizzando il Routing.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Ember_interactivity_events_state","Learn_web_development/Core/Frameworks_libraries/Ember_routing", "Learn_web_development/Core/Frameworks_libraries")}}
