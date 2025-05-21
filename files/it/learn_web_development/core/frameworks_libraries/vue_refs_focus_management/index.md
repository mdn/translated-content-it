---
title: Vue refs e metodi del ciclo di vita per la gestione del focus
slug: Learn_web_development/Core/Frameworks_libraries/Vue_refs_focus_management
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Vue_conditional_rendering","Learn_web_development/Core/Frameworks_libraries/Vue_resources", "Learn_web_development/Core/Frameworks_libraries")}}

Siamo quasi alla fine con Vue. L'ultimo aspetto funzionale che andremo a esaminare è la gestione del focus, o detto in altri termini, come possiamo migliorare l'accessibilità della nostra app tramite tastiera. Esamineremo l'uso dei **Vue refs** per gestire questo aspetto, una funzionalità avanzata che consente di avere accesso diretto ai nodi DOM sottostanti il virtual DOM, o accesso diretto dalla struttura interna del DOM di un componente figlio.

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
          gestiscono i dati dell'app e una sintassi basata su HTML che mappa
          la struttura del DOM sottostante. Per l'installazione e per usare alcune delle
          funzionalità più avanzate di Vue (come i Single File Components o le funzioni di render),
          è necessario un terminale con node + npm installati.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Imparare a gestire la gestione del focus usando i Vue refs.</td>
    </tr>
  </tbody>
</table>

## Il problema della gestione del focus

Anche se abbiamo una funzionalità di modifica funzionante, non stiamo offrendo una grande esperienza per gli utenti che non usano il mouse. Nello specifico, quando un utente attiva il pulsante "Modifica", rimuoviamo il pulsante "Modifica" dal DOM, ma non spostiamo il focus dell'utente da nessuna parte, quindi di fatto scompare. Questo può essere disorientante per gli utenti che usano la tastiera o non vedenti.

Per capire cosa succede attualmente:

1. Ricarica la tua pagina, quindi premi <kbd>Tab</kbd>. Dovresti vedere un contorno di focus sull'input per l'aggiunta di nuovi elementi da fare.

2. Premi di nuovo <kbd>Tab</kbd>. Il focus dovrebbe passare al pulsante "Aggiungi".

3. Premi di nuovo, e sarà sulla prima casella di controllo. Ancora una volta, e il focus dovrebbe essere sul primo pulsante "Modifica".
4. Attiva il pulsante "Modifica" premendo <kbd>Invio</kbd>.
   La casella di controllo sarà sostituita con il nostro componente di modifica, ma il contorno del focus sarà sparito.

Questo comportamento può essere destabilizzante. Inoltre, ciò che accade quando si preme di nuovo <kbd>Tab</kbd> varia a seconda del browser utilizzato. Allo stesso modo, se si salva o si annulla la modifica, il focus scomparirà di nuovo passando alla vista non modificabile.

Per offrire agli utenti un'esperienza migliore, aggiungeremo del codice per controllare il focus in modo che sia posto sul campo di modifica quando il modulo di modifica viene mostrato. Vorremo anche rimettere il focus sul pulsante "Modifica" quando un utente annulla o salva la sua modifica. Per impostare il focus, dobbiamo capire un po' di più su come Vue funziona internamente.

## Virtual DOM e refs

Vue, come alcuni altri framework, utilizza un virtual DOM (VDOM) per gestire gli elementi. Questo significa che Vue mantiene una rappresentazione di tutti i nodi nella nostra app in memoria. Qualsiasi aggiornamento è prima effettuato sui nodi in memoria, e poi tutte le modifiche necessarie ai nodi reali nella pagina sono sincronizzate in blocco.

Poiché la lettura e la scrittura di nodi DOM reali è spesso più costosa rispetto ai nodi virtuali, questo può comportare un miglioramento delle prestazioni. Tuttavia, questo significa anche che spesso non si dovrebbe modificare direttamente i tuoi elementi HTML tramite le API native del browser (come [`Document.getElementById`](/it/docs/Web/API/Document/getElementById)) quando si usano framework, perché porta il VDOM e il vero DOM a essere non sincronizzati.

Invece, se hai bisogno di accedere ai nodi DOM sottostanti (come quando imposti il focus), puoi usare [Vue refs](https://vuejs.org/guide/essentials/template-refs.html). Per i componenti Vue personalizzati, puoi anche usare i refs per accedere direttamente alla struttura interna di un componente figlio, tuttavia questo dovrebbe essere fatto con cautela poiché può rendere il codice più difficile da ragionare e comprendere.

Per usare un ref in un componente, aggiungi un attributo `ref` all'elemento che vuoi accedere, con un identificatore di stringa come valore dell'attributo. È importante notare che un ref deve essere univoco all'interno di un componente. Nessun due elementi renderizzati allo stesso tempo dovrebbero avere lo stesso ref.

### Aggiungere un ref alla nostra app

Quindi, associamo un ref al nostro pulsante "Modifica" in `ToDoItem.vue`. Aggiorna così:

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

Per accedere al valore associato al nostro ref, usiamo la proprietà `$refs` fornita sull'istanza del nostro componente. Per vedere il valore del ref quando clicchiamo sul nostro pulsante "Modifica", aggiungiamo un `console.log()` al nostro metodo `toggleToItemEditForm()`, come segue:

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

Se attivi il pulsante "Modifica" a questo punto, dovresti vedere un elemento HTML `<button>` riferito nella tua console.

## Metodo $nextTick() di Vue

Vogliamo impostare il focus sul pulsante "Modifica" quando un utente salva o annulla la sua modifica. Per farlo, dobbiamo gestire il focus nei metodi `itemEdited()` e `editCancelled()` del componente `ToDoItem`.

Per comodità, crea un nuovo metodo chiamato `focusOnEditButton()` che non prende argomenti. All'interno, assegna il tuo `ref` a una variabile, e poi chiama il metodo `focus()` sul ref.

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

Successivamente, aggiungi una chiamata a `this.focusOnEditButton()` alla fine dei metodi `itemEdited()` e `editCancelled()`:

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

Prova a modificare e quindi a salvare/annullare un elemento da fare tramite tastiera. Noterai che il focus non viene impostato, quindi abbiamo ancora un problema da risolvere. Se apri la tua console, vedrai un errore generato come _"impossibile accedere alla proprietà "focus", editButtonRef è indefinito"_. Questo sembra strano. Il tuo ref del pulsante era definito quando hai attivato il pulsante "Modifica", ma ora non lo è. Cosa sta succedendo?

Bene, ricorda che quando cambiamo `isEditing` a `true`, non rendiamo più la sezione del componente con il pulsante "Modifica". Questo significa che non c'è alcun elemento a cui associare il ref, quindi diventa `undefined`.

Potresti ora pensare "ehi, non impostiamo `isEditing=false` prima di provare ad accedere al `ref`, quindi il `v-if` non dovrebbe ora mostrare il pulsante?" Questo è dove entra in gioco il virtual DOM. Poiché Vue sta cercando di ottimizzare e bloccare le modifiche, non aggiornerà immediatamente il DOM quando impostiamo `isEditing` a `false`. Quindi quando chiamiamo `focusOnEditButton()`, il pulsante "Modifica" non è stato ancora renderizzato.

Invece, dobbiamo aspettare fino a dopo che Vue ha completato il prossimo ciclo di aggiornamento del DOM. Per farlo, i componenti Vue hanno un metodo speciale chiamato `$nextTick()`. Questo metodo accetta una funzione di callback, che viene poi eseguita dopo gli aggiornamenti del DOM.

Poiché il metodo `focusOnEditButton()` deve essere invocato dopo che il DOM è stato aggiornato, possiamo incorporare il corpo della funzione esistente all'interno di una chiamata a `$nextTick()`.

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

Ora quando attivi il pulsante "Modifica" e poi annulli o salvi le tue modifiche tramite la tastiera, il focus dovrebbe essere restituito al pulsante "Modifica". Successo!

## Metodi di ciclo di vita di Vue

Successivamente, dobbiamo spostare il focus sull'elemento `<input>` del modulo di modifica quando il pulsante "Modifica" viene cliccato. Tuttavia, poiché il nostro modulo di modifica è in un componente diverso rispetto al nostro pulsante "Modifica", non possiamo semplicemente impostare il focus all'interno del gestore eventi click del pulsante "Modifica". Invece, possiamo sfruttare il fatto che rimuoviamo e rimontiamo il componente `ToDoItemEditForm` ogni volta che il pulsante "Modifica" viene cliccato per gestire questo.

Quindi come funziona? Bene, i componenti Vue attraversano una serie di eventi, noti come **ciclo di vita**. Questo ciclo di vita si estende da prima che gli elementi siano _creati_ e aggiunti al VDOM (_montati_), fino a quando sono rimossi dal VDOM (_distrutti_).

Vue ti permette di eseguire metodi in varie fasi di questo ciclo di vita usando **metodi del ciclo di vita**. Questo può essere utile per cose come il recupero dei dati, dove potrebbe essere necessario ottenere i dati prima che il tuo componente venga renderizzato, o dopo che una proprietà cambia. L'elenco dei metodi del ciclo di vita è il seguente, nell'ordine in cui vengono eseguiti.

1. `beforeCreate()` — Esegue prima che l'istanza del tuo componente sia creata. Dati ed eventi non sono ancora disponibili.
2. `created()` — Esegue dopo che il tuo componente è inizializzato ma prima che il componente sia aggiunto al VDOM. Spesso è qui che avviene il recupero dei dati.
3. `beforeMount()` — Esegue dopo che il tuo template è stato compilato, ma prima che il tuo componente sia renderizzato al DOM vero e proprio.
4. `mounted()` — Esegue dopo che il tuo componente è montato nel DOM. Qui puoi accedere ai `refs`.
5. `beforeUpdate()` — Esegue ogni volta che i dati nel tuo componente cambiano, ma prima che i cambiamenti siano resi nel DOM.
6. `updated()` — Esegue ogni volta che i dati nel tuo componente sono cambiati e dopo che i cambiamenti sono stati resi nel DOM.
7. `beforeDestroy()` — Esegue prima che un componente sia rimosso dal DOM.
8. `destroyed()` — Esegue dopo che un componente è stato rimosso dal DOM.
9. `activated()` — Usato solo nei componenti avvolti in un tag speciale `keep-alive`. Esegue dopo che il componente è attivato.
10. `deactivated()` — Usato solo nei componenti avvolti in un tag speciale `keep-alive`. Esegue dopo che il componente è disattivato.

> [!NOTE]
> La documentazione di Vue fornisce un [bel diagramma per visualizzare quando questi hook si verificano](https://vuejs.org/guide/essentials/lifecycle.html#lifecycle-diagram). Questo articolo dal [Blog della Community di DigitalOcean approfondisce più nel dettaglio i metodi del ciclo di vita](https://www.digitalocean.com/community/tutorials/vuejs-component-lifecycle).

Ora che abbiamo esaminato i metodi del ciclo di vita, usiamone uno per attivare il focus quando il nostro componente `ToDoItemEditForm` è montato.

In `ToDoItemEditForm.vue`, associa `ref="labelInput"` all'elemento `<input>`, così:

```vue
<input
  :id="id"
  ref="labelInput"
  type="text"
  autocomplete="off"
  v-model.lazy.trim="newName" />
```

Successivamente, aggiungi una proprietà `mounted()` appena all'interno del tuo oggetto componente — **nota che questo non dovrebbe essere collocato all'interno della proprietà `methods`, bensì allo stesso livello gerarchico di `props`, `data()`, e `methods`.** I metodi del ciclo di vita sono metodi speciali che stanno da soli, non accanto ai metodi definiti dall'utente. Questo non deve prendere input. Nota che non puoi usare una funzione freccia qui poiché abbiamo bisogno di accedere a `this` per accedere al nostro ref `labelInput`.

```js
export default {
  // …
  mounted() {},
  // …
};
```

All'interno del tuo metodo `mounted()`, assegna il tuo ref `labelInput` a una variabile, e poi chiama la funzione `focus()` del ref. Non devi usare `$nextTick()` qui perché il componente è già stato aggiunto al DOM quando `mounted()` è chiamato.

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

Ora quando attivi il pulsante "Modifica" con la tastiera, il focus dovrebbe essere immediatamente spostato all'elemento `<input>` di modifica.

## Gestione del focus durante l'eliminazione di elementi da fare

C'è un altro punto in cui dobbiamo considerare la gestione del focus: quando un utente elimina un elemento da fare. Quando si clicca sul pulsante "Edit", ha senso spostare il focus sulla casella di testo del nome da modificare, e tornare sul pulsante "Edit" quando si annulla o si salva dalla schermata di modifica.

Tuttavia, a differenza del modulo di modifica, non abbiamo una posizione chiara dove spostare il focus quando un elemento è eliminato. Abbiamo anche bisogno di un modo per fornire agli utenti di tecnologie assistive informazioni che confermino che un elemento è stato eliminato.

Stiamo già tracciando il numero di elementi nel nostro intestazione della lista — l'`<h2>` in `App.vue` — ed è associata alla nostra lista di elementi da fare. Questo la rende un posto ragionevole dove spostare il focus quando eliminiamo un nodo.

Per prima cosa, dobbiamo aggiungere un ref alla nostra intestazione della lista. Dobbiamo anche aggiungere un `tabindex="-1"` — questo rende l'elemento programmabilmente focalizzabile (cioè può essere focalizzato tramite JavaScript), quando di default non lo è.

All'interno di `App.vue`, aggiorna il tuo `<h2>` come segue:

```vue
<h2 id="list-summary" ref="listSummary" tabindex="-1">\{{listSummary}}</h2>
```

> **Nota:** [`tabindex`](/it/docs/Web/HTML/Reference/Global_attributes/tabindex) è uno strumento molto potente per gestire certi problemi di accessibilità. Tuttavia, dovrebbe essere usato con cautela. Usare troppo `tabindex="-1"` può causare problemi per tutti i tipi di utenti, quindi usalo solo dove è strettamente necessario. Non dovresti mai usare `tabindex` >= `0`, poiché può causare problemi agli utenti in quanto può far sì che il flusso del DOM e l'ordine di tabulazione non corrispondano, e/o aggiungere elementi non interattivi all'ordine di tabulazione. Questo può essere confuso per gli utenti, specialmente quelli che utilizzano screen reader e altre tecnologie assistive.

Ora che abbiamo un `ref` e abbiamo informato i browser che possiamo focalizzare programmabilmente l'`<h2>`, dobbiamo impostarvi il focus. Alla fine di `deleteToDo()`, usa il ref `listSummary` per impostare il focus sull'`<h2>`. Poiché l'`<h2>` è sempre renderizzato nell'app, non devi preoccuparti di usare `$nextTick()` o i metodi del ciclo di vita per gestire la focalizzazione.

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

Ora, quando elimini un elemento dalla tua lista, il focus dovrebbe essere spostato verso l'alto all'intestazione della lista. Questo dovrebbe fornire un'esperienza di focus ragionevole per tutti i nostri utenti.

## Sommario

Quindi questo è quanto per la gestione del focus e per la nostra app! Congratulazioni per aver completato tutti i nostri tutorial su Vue. Nell'articolo successivo chiuderemo il cerchio con alcune risorse aggiuntive per approfondire lo studio di Vue.

> [!NOTE]
> Se hai bisogno di controllare il tuo codice rispetto alla nostra versione, puoi trovare una versione completata del codice di esempio Vue app nel nostro repository todo-vue. Per una versione live in esecuzione, vedi <https://mdn.github.io/todo-vue/>.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Vue_conditional_rendering","Learn_web_development/Core/Frameworks_libraries/Vue_resources", "Learn_web_development/Core/Frameworks_libraries")}}
