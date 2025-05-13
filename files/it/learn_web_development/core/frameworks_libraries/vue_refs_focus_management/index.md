---
title: Vue refs e metodi del ciclo di vita per la gestione del focus
slug: Learn_web_development/Core/Frameworks_libraries/Vue_refs_focus_management
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Vue_conditional_rendering","Learn_web_development/Core/Frameworks_libraries/Vue_resources", "Learn_web_development/Core/Frameworks_libraries")}}

Siamo quasi alla fine con Vue. L'ultimo aspetto da esaminare è la gestione del focus, o in altre parole, come possiamo migliorare l'accessibilità della tastiera della nostra app. Esamineremo l'utilizzo dei **ref di Vue** per gestire questo — una funzionalità avanzata che consente di avere accesso diretto ai nodi DOM sottostanti il virtual DOM, o accesso diretto da un componente alla struttura DOM interna di un componente figlio.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          Familiarità con i linguaggi di base <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>,
          conoscenza del
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminale/command line</a
          >.
        </p>
        <p>
          I componenti Vue sono scritti come una combinazione di oggetti JavaScript che
          gestiscono i dati dell'app e una sintassi di template basata su HTML che mappa
          la struttura DOM sottostante. Per l'installazione e per utilizzare alcune delle
          funzionalità più avanzate di Vue (come i Componenti a File Singolo o le funzioni di rendering), avrà bisogno di un terminale con node + npm installati.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Imparare a gestire il focus utilizzando i ref di Vue.</td>
    </tr>
  </tbody>
</table>

## Il problema della gestione del focus

Anche se abbiamo funzionalità di modifica funzionanti, non forniamo una grande esperienza per gli utenti non mouse. In particolare, quando un utente attiva il pulsante "Modifica", rimuoviamo il pulsante "Modifica" dal DOM, ma non spostiamo il focus dell'utente da nessuna parte, quindi in effetti sparisce. Questo può essere disorientante per gli utenti della tastiera e non visivi.

Per capire cosa sta succedendo attualmente:

1. Ricarichi la pagina, quindi prema <kbd>Tab</kbd>. Dovrebbe vedere un contorno di messa a fuoco sull'input per l'aggiunta di nuovi elementi da fare.

2. Prema <kbd>Tab</kbd> di nuovo. Il focus dovrebbe spostarsi sul pulsante "Aggiungi".

3. Lo prema ancora una volta, e sarà sul primo checkbox. Ancora una volta, e il focus dovrebbe essere sul primo pulsante "Modifica".
4. Attivi il pulsante "Modifica" premendo <kbd>Invio</kbd>.
   Il checkbox verrà sostituito con il nostro componente di modifica, ma il contorno di messa a fuoco sparirà.

Questo comportamento può essere sconcertante. Inoltre, ciò che accade quando preme nuovamente <kbd>Tab</kbd> varia a seconda del browser che sta usando. Allo stesso modo, se salva o annulla la modifica, il focus scomparirà nuovamente mentre torna alla vista non modificata.

Per offrire agli utenti un'esperienza migliore, aggiungeremo codice per controllare il focus in modo che venga impostato sul campo di modifica quando viene mostrato il modulo di modifica. Vorremo anche rimettere il focus sul pulsante "Modifica" quando un utente annulla o salva la sua modifica. Per impostare il focus, dobbiamo comprendere un po' meglio come Vue funziona internamente.

## Virtual DOM e ref

Vue, come alcuni altri framework, utilizza un virtual DOM (VDOM) per gestire gli elementi. Ciò significa che Vue mantiene una rappresentazione di tutti i nodi dell'app in memoria. Gli aggiornamenti vengono eseguiti prima sui nodi in memoria e poi tutte le modifiche che devono essere apportate ai nodi effettivi sulla pagina vengono sincronizzate in un batch.

Poiché la lettura e la scrittura dei nodi DOM effettivi è spesso più costosa rispetto ai nodi virtuali, ciò può comportare migliori performance. Tuttavia, significa anche che spesso non dovrebbe modificare direttamente gli elementi HTML tramite API native del browser (come [`Document.getElementById`](/it/docs/Web/API/Document/getElementById)) quando utilizza framework, perché ciò comporta che il VDOM e il DOM reale si desincronizzino.

Invece, se ha bisogno di accedere ai nodi DOM sottostanti (come quando imposta il focus), può usare i [ref di Vue](https://vuejs.org/guide/essentials/template-refs.html). Per i componenti Vue personalizzati, può anche usare i ref per accedere direttamente alla struttura interna di un componente figlio, tuttavia ciò dovrebbe essere fatto con cautela poiché può rendere il codice più difficile da capire e gestire.

Per utilizzare un ref in un componente, aggiunga un attributo `ref` all'elemento a cui vuole accedere, con un identificatore di stringa per il valore dell'attributo. È importante notare che un ref deve essere univoco all'interno di un componente. Nessun due elementi resi contemporaneamente dovrebbero avere lo stesso ref.

### Aggiungere un ref alla nostra app

Quindi, alleghiamo un ref al nostro pulsante "Modifica" in `ToDoItem.vue`. Lo aggiorni così:

```vue
<button
  type="button"
  class="btn"
  ref="editButton"
  @click="toggleToItemEditForm">
  Edit
  <span class="visually-hidden">\{{label}}</span>
</button>
```

Per accedere al valore associato al nostro ref, usiamo la proprietà `$refs` fornita sulla nostra istanza del componente. Per vedere il valore del ref quando clicchiamo il nostro pulsante "Modifica", aggiunga un `console.log()` al nostro metodo `toggleToItemEditForm()`, come mostrato:

```js
export default {
  // …
  methods: {
    // …
    toggleToItemEditForm() {
      console.log(this.$refs.editButton);
      this.isEditing = true;
    },
    // …
  },
  // …
};
```

Se attiva il pulsante "Modifica" a questo punto, dovrebbe vedere un elemento HTML `<button>` referenziato nella console.

## Metodo $nextTick() di Vue

Vogliamo impostare il focus sul pulsante "Modifica" quando un utente salva o annulla la modifica. Per fare ciò, dobbiamo gestire il focus nei metodi `itemEdited()` e `editCancelled()` del componente `ToDoItem`.

Per comodità, crei un nuovo metodo che non prende argomenti chiamato `focusOnEditButton()`. All'interno, assegni il suo `ref` a una variabile, e quindi chiami il metodo `focus()` sul ref.

```js
export default {
  // …
  methods: {
    // …
    focusOnEditButton() {
      const editButtonRef = this.$refs.editButton;
      editButtonRef.focus();
    },
    // …
  },
  // …
};
```

Successivamente, aggiunga una chiamata a `this.focusOnEditButton()` alla fine dei metodi `itemEdited()` e `editCancelled()`:

```js
export default {
  // …
  methods: {
    // …
    itemEdited(newItemName) {
      this.$emit("item-edited", newItemName);
      this.isEditing = false;
      this.focusOnEditButton();
    },
    editCancelled() {
      this.isEditing = false;
      this.focusOnEditButton();
    },
    // …
  },
  // …
};
```

Provi a modificare e quindi a salvare/annullare un elemento da fare tramite la tastiera. Noterà che il focus non viene impostato, quindi abbiamo ancora un problema da risolvere. Se apre la console, vedrà un errore generato del tipo _"impossibile accedere alla proprietà "focus", editButtonRef non è definito"_. Questo sembra strano. Il suo ref del pulsante era definito quando ha attivato il pulsante "Modifica", ma ora non lo è. Cosa sta succedendo?

Bene, ricordi che quando cambiamo `isEditing` su `true`, non rendiamo più la sezione del componente che presenta il pulsante "Modifica". Ciò significa che non c'è elemento a cui assegnare il ref, quindi diventa `undefined`.

Potrebbe pensare ora "ehi, non impostiamo `isEditing=false` prima di tentare di accedere al `ref`, quindi ora il `v-if` non dovrebbe mostrare il pulsante?" Qui entra in gioco il virtual DOM. Poiché Vue sta cercando di ottimizzare e elaborare le modifiche in batch, non aggiornerà immediatamente il DOM quando impostiamo `isEditing` su `false`. Quindi, quando chiamiamo `focusOnEditButton()`, il pulsante "Modifica" non è ancora stato renderizzato.

Invece, dobbiamo aspettare fino a quando Vue non abbia completato il prossimo ciclo di aggiornamento del DOM. Per fare ciò, i componenti Vue hanno un metodo speciale chiamato `$nextTick()`. Questo metodo accetta una funzione di callback, che viene quindi eseguita dopo che il DOM è stato aggiornato.

Poiché il metodo `focusOnEditButton()` deve essere invocato dopo che il DOM è stato aggiornato, possiamo racchiudere il corpo della funzione esistente all'interno di una chiamata a `$nextTick()`.

```js
export default {
  // …
  methods: {
    // …
    focusOnEditButton() {
      this.$nextTick(() => {
        const editButtonRef = this.$refs.editButton;
        editButtonRef.focus();
      });
    },
    // …
  },
  // …
};
```

Ora, quando attiva il pulsante "Modifica" e quindi cancella o salva le modifiche tramite la tastiera, il focus dovrebbe essere restituito al pulsante "Modifica". Successo!

## Metodi del ciclo di vita di Vue

Successivamente, dobbiamo spostare il focus sull'elemento `<input>` del modulo di modifica quando viene cliccato il pulsante "Modifica". Tuttavia, poiché il nostro modulo di modifica è in un componente diverso rispetto al nostro pulsante "Modifica", non possiamo semplicemente impostare il focus all'interno del gestore dell'evento click del pulsante "Modifica". Invece, possiamo usare il fatto che rimuoviamo e rimontiamo il nostro componente `ToDoItemEditForm` ogni volta che viene cliccato il pulsante "Modifica" per gestire questo.

Quindi, come funziona questo? Bene, i componenti Vue attraversano una serie di eventi, noti come **ciclo di vita**. Questo ciclo di vita va da quando gli elementi sono _creati_ e aggiunti al VDOM (_montati_), fino a quando vengono rimossi dal VDOM (_distrutti_).

Vue consente di eseguire metodi in vari stadi di questo ciclo di vita usando i **metodi del ciclo di vita**. Questo può essere utile per cose come il recupero di dati, dove potrebbe aver bisogno di ottenere i suoi dati prima che il componente venga renderizzato, o dopo che una proprietà cambia. Di seguito è riportato l'elenco dei metodi del ciclo di vita, nell'ordine in cui vengono chiamati.

1. `beforeCreate()` — Viene chiamato prima che l'istanza del suo componente venga creata. I dati e gli eventi non sono ancora disponibili.
2. `created()` — Viene chiamato dopo che il suo componente è stato inizializzato ma prima che il componente venga aggiunto al VDOM. Solitamente è dove viene eseguito il recupero dei dati.
3. `beforeMount()` — Viene chiamato dopo che il suo template è stato compilato, ma prima che il suo componente venga renderizzato nel DOM effettivo.
4. `mounted()` — Viene chiamato dopo che il suo componente è stato montato nel DOM. Può accedere ai `ref` qui.
5. `beforeUpdate()` — Viene chiamato ogni volta che i dati nel suo componente cambiano, ma prima che le modifiche vengano renderizzate nel DOM.
6. `updated()` — Viene chiamato ogni volta che i dati nel suo componente sono cambiati e dopo che le modifiche sono state renderizzate nel DOM.
7. `beforeDestroy()` — Viene chiamato prima che un componente venga rimosso dal DOM.
8. `destroyed()` — Viene chiamato dopo che un componente è stato rimosso dal DOM.
9. `activated()` — Viene usato solo nei componenti avvolti in un tag `keep-alive` speciale. Viene chiamato dopo che il componente è stato attivato.
10. `deactivated()` — Viene usato solo nei componenti avvolti in un tag `keep-alive` speciale. Viene chiamato dopo che il componente è stato disattivato.

> [!NOTE]
> I Documenti di Vue forniscono un [bel diagramma per visualizzare quando questi hook accadono](https://vuejs.org/guide/essentials/lifecycle.html#lifecycle-diagram). Questo articolo dal [Blog della Community di DigitalOcean approfondisce i metodi del ciclo di vita](https://www.digitalocean.com/community/tutorials/vuejs-component-lifecycle).

Ora che abbiamo esaminato i metodi del ciclo di vita, usiamone uno per attivare il focus quando il nostro componente `ToDoItemEditForm` viene montato.

In `ToDoItemEditForm.vue`, alleghi `ref="labelInput"` all'elemento `<input>`, come mostrato:

```vue
<input
  :id="id"
  ref="labelInput"
  type="text"
  autocomplete="off"
  v-model.lazy.trim="newName" />
```

Successivamente, aggiunga una proprietà `mounted()` proprio all'interno del suo oggetto componente — **noti che questo non dovrebbe essere messo all'interno della proprietà `methods`, ma piuttosto allo stesso livello gerarchico di `props`, `data()`, e `methods`.** I metodi del ciclo di vita sono metodi speciali che stanno da soli, non accanto ai metodi definiti dall'utente. Questo non dovrebbe prendere input. Noti che non può usare una funzione freccia qui poiché abbiamo bisogno di accedere a `this` per accedere al nostro ref `labelInput`.

```js
export default {
  // …
  mounted() {},
  // …
};
```

All'interno del suo metodo `mounted()`, assegni il suo ref `labelInput` a una variabile, e poi chiami la funzione `focus()` del ref. Non è necessario usare `$nextTick()` qui perché il componente è stato già aggiunto al DOM quando viene chiamato `mounted()`.

```js
export default {
  // …
  mounted() {
    const labelInputRef = this.$refs.labelInput;
    labelInputRef.focus();
  },
  // …
};
```

Ora quando attiva il pulsante "Modifica" con la tastiera, il focus dovrebbe spostarsi immediatamente sull'`<input>` di modifica.

## Gestire il focus quando si eliminano elementi da fare

C'è un altro luogo in cui dobbiamo considerare la gestione del focus: quando un utente elimina un elemento da fare. Quando si clicca il pulsante "Modifica", ha senso spostare il focus sulla casella di testo del nome da modificare, e riportarlo sul pulsante "Modifica" quando si annulla o si salva dalla schermata di modifica.

Tuttavia, a differenza del modulo di modifica, non abbiamo una posizione chiara verso cui spostare il focus quando un elemento viene eliminato. Abbiamo anche bisogno di un modo per fornire agli utenti della tecnologia assistiva informazioni che confermano che un elemento è stato eliminato.

Stiamo già monitorando il numero di elementi nel nostro titolo dell'elenco — il `<h2>` in `App.vue` — ed è associato al nostro elenco di elementi da fare. Questo lo rende un luogo ragionevole verso cui spostare il focus quando eliminiamo un nodo.

Prima, dobbiamo aggiungere un ref al nostro titolo dell'elenco. Dobbiamo anche aggiungere un `tabindex="-1"` ad esso — questo rende l'elemento programmabilmente focusabile (cioè, può essere focalizzato tramite JavaScript), quando per impostazione predefinita non lo è.

All'interno di `App.vue`, aggiorni il suo `<h2>` come segue:

```vue
<h2 id="list-summary" ref="listSummary" tabindex="-1">\{{listSummary}}</h2>
```

> **Nota:** [`tabindex`](/it/docs/Web/HTML/Reference/Global_attributes/tabindex) è uno strumento davvero potente per gestire alcuni problemi di accessibilità. Tuttavia, dovrebbe essere usato con cautela. L'uso eccessivo di `tabindex="-1"` può causare problemi per tutti i tipi di utenti, quindi lo usi esattamente dove le è necessario. Inoltre, non dovrebbe quasi mai usare `tabindex` > = `0`, poiché può causare problemi agli utenti in quanto può rendere il flusso DOM e l'ordine di tabulazione incoerenti, e/o aggiungere elementi non interattivi all'ordine di tabulazione. Questo può essere confondente per gli utenti, in particolare quelli che usano lettori di schermo e altre tecnologie assistive.

Ora che abbiamo un `ref` e abbiamo informato i browser che possiamo focalizzare l'`<h2>` programmabilmente, dobbiamo impostare il focus su di esso. Alla fine di `deleteToDo()`, utilizzi il ref `listSummary` per impostare il focus sull'`<h2>`. Poiché l'`<h2>` è sempre visualizzato nell'app, non è necessario preoccuparsi di usare `$nextTick()` o metodi del ciclo di vita per gestire il suo focus.

```js
export default {
  // …
  methods: {
    // …
    deleteToDo(toDoId) {
      const itemIndex = this.ToDoItems.findIndex((item) => item.id === toDoId);
      this.ToDoItems.splice(itemIndex, 1);
      this.$refs.listSummary.focus();
    },
    // …
  },
  // …
};
```

Ora, quando elimina un elemento dal suo elenco, il focus dovrebbe essere spostato verso l'alto al titolo dell'elenco. Questo dovrebbe fornire un'esperienza di focus ragionevole per tutti i nostri utenti.

## Riassunto

Questo è tutto per la gestione del focus, e per la nostra app! Complimenti per aver lavorato attraverso tutti i nostri tutorial di Vue. Nel prossimo articolo concluderemo con alcune risorse aggiuntive per approfondire ulteriormente la sua conoscenza di Vue.

> [!NOTE]
> Se ha bisogno di controllare il suo codice rispetto alla nostra versione, può trovare una versione finale del codice dell'app di esempio Vue nel nostro repository todo-vue. Per una versione live, visiti <https://mdn.github.io/todo-vue/>.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Vue_conditional_rendering","Learn_web_development/Core/Frameworks_libraries/Vue_resources", "Learn_web_development/Core/Frameworks_libraries")}}
