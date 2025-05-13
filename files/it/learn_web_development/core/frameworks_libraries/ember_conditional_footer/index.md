---
title: "Interattività di Ember: Funzionalità del footer, rendering condizionale"
slug: Learn_web_development/Core/Frameworks_libraries/Ember_conditional_footer
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Ember_interactivity_events_state","Learn_web_development/Core/Frameworks_libraries/Ember_routing", "Learn_web_development/Core/Frameworks_libraries")}}

Ora è il momento di affrontare la funzionalità del footer nella nostra app. Qui faremo in modo che il contatore dei todo si aggiorni per mostrare il numero corretto di todo ancora da completare e applicheremo correttamente lo stile ai todo completati (cioè dove la casella di controllo è stata selezionata). Collegheremo anche il nostro pulsante "Clear completed". Lungo il percorso, impareremo a utilizzare il rendering condizionale nei nostri template.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          Si consiglia di avere familiarità con i linguaggi di base
          <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e
          avere conoscenze di
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminal/command line</a
          >.
        </p>
        <p>
          Una comprensione più approfondita delle funzionalità moderne di JavaScript (come classi,
          moduli, ecc.) sarà estremamente utile, poiché Ember fa ampio uso
          di esse.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Continuare il nostro apprendimento sulle classi di componenti, iniziare a osservare
        il rendering condizionale e collegare alcune delle funzionalità del footer.
      </td>
    </tr>
  </tbody>
</table>

## Connettere il comportamento nel footer

Per far funzionare il footer, dobbiamo implementare le seguenti tre aree di funzionalità:

- Un contatore di todo in sospeso.
- Filtri per tutti i todo, attivi e completati.
- Un pulsante per cancellare i todo completati.

1. Poiché abbiamo bisogno di accedere al nostro servizio dal componente footer, dobbiamo generare una classe per il footer. Inserisca il seguente comando nel terminal per farlo:

   ```bash
   ember generate component-class footer
   ```

2. Successivamente, trovi il file appena creato `todomvc/app/components/footer.js` e lo aggiorni come segue:

   ```ts
   import Component from "@glimmer/component";
   import { inject as service } from "@ember/service";

   export default class FooterComponent extends Component {
     @service("todo-data") todos;
   }
   ```

3. Ora dobbiamo tornare al nostro file `todo-data.js` e aggiungere alcune funzionalità che ci permetteranno di restituire il numero di todo incompleti (utile per mostrare quanti ne rimangono) e cancellare i todo completati dalla lista (che è ciò di cui ha bisogno la funzionalità "Clear completed").

   In `todo-data.js`, aggiunga il seguente getter sotto il getter esistente `all()` per definire quali siano effettivamente i todo incompleti:

   ```ts
   get incomplete() {
     return this.todos.filter((todo) => !todo.isCompleted);
   }
   ```

   Utilizzando il metodo [`filter()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Array/filter), chiediamo tutti gli elementi todo dove la proprietà `isCompleted` è uguale a `false`, e poiché `isCompleted` è `@tracked` nel nostro oggetto `Todo`, questo getter verrà ricalcolato quando il valore cambia su un Oggetto nell'array.

4. Successivamente, aggiunga la seguente azione sotto l'azione esistente `add(text)`:

   ```ts
   @action
   clearCompleted() {
     this.todos = this.incomplete;
   }
   ```

   Questo è molto comodo per cancellare i todo — dobbiamo solo impostare l'array `todos` uguale all'elenco dei todo incompleti.

5. Infine, dobbiamo utilizzare questa nuova funzionalità nel nostro template `footer.hbs`. Vai a questo file ora.
6. Prima di tutto, sostituisca questa riga:

   ```hbs
   <strong>0</strong> todos left
   ```

   Con questa, che popola il numero incompleto con la lunghezza dell'array `incomplete`:

   ```hbs
   <strong>\{{this.todos.incomplete.length}}</strong> todos left
   ```

7. Successivamente, sostituisca questo:

   ```hbs
   <button type="button" class="clear-completed">
   ```

   Con questo:

   ```hbs
   <button type="button" class="clear-completed" \{{on 'click' this.todos.clearCompleted}}>
   ```

Quindi ora quando il pulsante viene cliccato, viene eseguita l'azione `clearCompleted()` che abbiamo aggiunto prima.
Tuttavia, se prova a cliccare il pulsante "Clear Completed" ora, non sembra fare nulla, perché attualmente non c'è modo di "completare" un todo. Dobbiamo collegare il template `todo.hbs` al servizio, in modo che controllando la casella di controllo pertinente cambi lo stato di ciascun todo. Faremo questo passaggio successivo.

## Il problema del plurale todo/todos

Quanto sopra va bene, ma abbiamo un altro piccolo problema da affrontare. L'indicatore "todos rimasti" dice sempre "x todos left", anche quando c'è solo un todo rimasto, il che è grammaticalmente scorretto!

Per risolvere questo problema, dobbiamo aggiornare questa parte del template per includere del rendering condizionale. In Ember, puoi rendere condizionalmente parti del template usando [contenuto condizionale](https://guides.emberjs.com/v3.18.0/components/conditional-content/); un semplice esempio di blocco appare così:

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

Ciò ci darà un errore, tuttavia — in Ember, questi semplici if statement possono attualmente testare solo per un valore truthy/falsy, non un'espressione più complessa come un confronto. Per risolvere questo problema, dovremo aggiungere un getter a `todo-data.js` per restituire il risultato di `this.incomplete.length === 1`, e quindi chiamarlo nel nostro template.

Aggiungi il seguente nuovo getter a `todo-data.js`, appena sotto i getter esistenti. Si noti che qui abbiamo bisogno di `this.incomplete.length`, non di `this.todos.incomplete.length`, perché stiamo facendo questo all'interno del servizio, dove il getter `incomplete()` è disponibile direttamente (nel template, il contenuto del servizio è stato reso disponibile come `todos` tramite la riga `@service('todo-data') todos;` all'interno della classe footer, di conseguenza è `this.todos.incomplete.length` lì).

```ts
get todoCountIsOne() {
  return this.incomplete.length === 1;
}
```

Quindi torna a `footer.hbs` e aggiorna la precedente sezione del template che abbiamo modificato con il seguente:

```hbs
<strong>\{{this.todos.incomplete.length}}</strong>
\{{#if this.todos.todoCountIsOne}}todo\{{else}}todos\{{/if}} left
```

Ora salvi e testi, e vedrai il corretto uso del plurale quando hai solo un elemento todo presente!

Si noti che questo è il modulo a blocchi di `if` in Ember; potrebbe anche utilizzare la forma in linea:

```hbs
\{{if this.todos.todoCountIsOne "todo" "todos"}}
```

## Completare i todo

Come con gli altri componenti, abbiamo bisogno di una classe per accedere al servizio.

### Creare una classe todo

1. Esegui il seguente comando nel tuo terminal:

   ```bash
   ember generate component-class todo
   ```

2. Ora vai al file appena creato `todomvc/app/components/todo.js` e aggiorna il contenuto in modo che assomigli a questo, per dare al componente todo accesso al servizio:

   ```ts
   import Component from "@glimmer/component";
   import { inject as service } from "@ember/service";

   export default class TodoComponent extends Component {
     @service("todo-data") todos;
   }
   ```

3. Successivamente, torni nuovamente al nostro file di servizio `todo-data.js` e aggiungi la seguente azione appena sotto le precedenti, che ci consentirà di cambiare lo stato di completamento per ogni todo:

   ```ts
   @action
   toggleCompletion(todo) {
     todo.isCompleted = !todo.isCompleted;
   }
   ```

### Aggiornare il template per mostrare lo stato completato

Infine, modicheremo il template `todo.hbs` in modo che il valore della casella di controllo sia ora associato alla proprietà `isCompleted` del todo e in modo che, al cambiamento, il metodo `toggleCompletion()` sul servizio todo venga invocato.

1. In `todo.hbs`, prima trova la seguente riga:

   ```hbs
   <li>
   ```

   E sostituiscila con questa — noterai che qui stiamo usando del contenuto condizionale aggiuntivo per aggiungere il valore classe se appropriato:

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
   > Lo snippet sopra usa una nuova parola chiave specifica di Ember — `fn`. `fn` consente la [applicazione parziale](https://en.wikipedia.org/wiki/Partial_application), che è simile a [`bind`](/it/docs/Web/JavaScript/Reference/Global_Objects/Function/bind), ma non cambia mai il contesto di invocazione; ciò equivale a utilizzare `bind` con un primo argomento `null`.

Prova a riavviare il server di sviluppo e ad andare su `localhost:4200` di nuovo, e ora vedrai che abbiamo un contatore di "todos rimasti" completamente operativo e un pulsante di cancellazione:

![todo contrassegnati come completati e cancellati](todos-being-marked-completed-and-cleared.gif)

Se si sta chiedendo perché non stiamo semplicemente eseguendo il toggle sul componente, dal momento che la funzione è completamente autonoma e non richiede nulla dal servizio, ha perfettamente ragione a farsi questa domanda! Tuttavia, poiché _alla fine_, desidereremo memorizzare permanentemente o sincronizzare tutte le modifiche all'elenco dei todo nello [storage locale](/it/docs/Web/API/Window/localStorage) (vedi la [versione finale dell'app](https://nullvoxpopuli.github.io/ember-todomvc-tutorial/)), ha senso avere tutte le operazioni che modificano uno stato persistente nello stesso posto.

## Riepilogo

È abbastanza per ora. A questo punto, non solo possiamo contrassegnare i todo come completi, ma possiamo anche cancellarli. Ora l'unica cosa rimasta da collegare nel footer sono i tre link di filtro: "All", "Active" e "Completed". Faremo questo nel prossimo articolo, utilizzando il Routing.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Ember_interactivity_events_state","Learn_web_development/Core/Frameworks_libraries/Ember_routing", "Learn_web_development/Core/Frameworks_libraries")}}
