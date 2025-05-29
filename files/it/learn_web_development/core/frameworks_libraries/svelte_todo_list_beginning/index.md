---
title: Iniziare la nostra app di lista di cose da fare con Svelte
slug: Learn_web_development/Core/Frameworks_libraries/Svelte_Todo_list_beginning
l10n:
  sourceCommit: edb16c0a662d7e719efe67561389a7a087c1ace9
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Svelte_getting_started","Learn_web_development/Core/Frameworks_libraries/Svelte_variables_props", "Learn_web_development/Core/Frameworks_libraries")}}

Ora che abbiamo un comprensione di base di come funzionano le cose in Svelte, possiamo iniziare a costruire la nostra app di esempio: una lista di cose da fare. In questo articolo esamineremo innanzitutto le funzionalità desiderate per la nostra app, quindi creeremo un componente `Todos.svelte` e inseriremo markup statico e stili, lasciando tutto pronto per iniziare a sviluppare le funzionalità della nostra app di lista di cose da fare, che approfondiremo nei successivi articoli.

Vogliamo che i nostri utenti possano sfogliare, aggiungere ed eliminare attività, e anche segnarle come completate. Questa sarà la funzionalità di base che svilupperemo in questa serie di tutorial e lungo il percorso esamineremo anche alcuni concetti più avanzati.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          Come minimo, si raccomanda di avere familiarità con i linguaggi core
          <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e
          avere conoscenza del
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminale/riga di comando</a
          >.
        </p>
        <p>
          Avrai bisogno di un terminale con node + npm installati per compilare e costruire
          la tua app.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare a creare un componente Svelte, renderizzarlo all'interno di un altro
        componente, passare dati tramite props e salvare il suo stato.
      </td>
    </tr>
  </tbody>
</table>

## Seguire il codice con noi

### Git

Clona il repository GitHub (se non lo hai già fatto) con:

```bash
git clone https://github.com/opensas/mdn-svelte-tutorial.git
```

Quindi, per raggiungere lo stato corrente dell'app, esegui

```bash
cd mdn-svelte-tutorial/02-starting-our-todo-app
```

Oppure scarica direttamente il contenuto della cartella:

```bash
npx degit opensas/mdn-svelte-tutorial/02-starting-our-todo-app
```

Ricorda di eseguire `npm install && npm run dev` per avviare la tua app in modalità sviluppo.

### REPL

Per seguire il codice con noi usando il REPL, inizia da

<https://svelte.dev/repl/b7b831ea3a354d3789cefbc31e2ca495?version=3.23.2>

## Funzionalità dell'app di lista di cose da fare

Questo è come apparirà la nostra app di lista di cose da fare una volta completata:

![app tipica di lista di cose da fare, con un titolo 'cosa deve essere fatto', un input per inserire altri to-do, e una lista di to-do con checkbox](01-todo-list-app.png)

Usando questa interfaccia utente, il nostro utente sarà in grado di:

- Sfogliare le sue attività
- Segnare le attività come completate/in sospeso senza eliminarle
- Rimuovere attività
- Aggiungere nuove attività
- Filtrare attività per stato: tutte le attività, attività attive o completate
- Modificare attività
- Segnare tutte le attività come attive/completate
- Rimuovere tutte le attività completate

## Costruire il nostro primo componente

Creiamo un componente `Todos.svelte`. Questo conterrà la nostra lista di cose da fare.

1. Crea una nuova cartella — `src/components`.

   > [!NOTE]
   > Puoi posizionare i tuoi componenti ovunque all'interno della cartella `src`, ma la cartella `components` è una convenzione riconosciuta da seguire, permettendoti di trovare facilmente i tuoi componenti.

2. Crea un file chiamato `src/components/Todos.svelte` con il seguente contenuto:

   ```svelte
   <h1>Svelte to-do list</h1>
   ```

3. Cambia l'elemento `title` in `public/index.html` per contenere il testo _Svelte to-do list_:

   ```svelte
   <title>Svelte to-do list</title>
   ```

4. Apri `src/App.svelte` e sostituisci il suo contenuto con quanto segue:

   ```svelte
   <script>
     import Todos from "./components/Todos.svelte";
   </script>

   <Todos />
   ```

5. In modalità sviluppo, Svelte emetterà un avviso nella console del browser specificando una prop che non esiste nel componente; in questo caso abbiamo una prop `name` specificata quando instanziamo il componente `App` dentro `src/main.js`, che non viene usata all'interno di `App`. Attualmente la console dovrebbe darti un messaggio del tipo "\<App> è stato creato con un prop sconosciuto 'name'". Per eliminare questo, rimuovi la prop `name` da `src/main.js`; dovrebbe ora apparire così:

   ```js
   import App from "./App.svelte";

   const app = new App({
     target: document.body,
   });

   export default app;
   ```

Ora, se controlli l'URL del tuo server di test vedrai il nostro componente `Todos.svelte` che viene renderizzato:

![renderizzazione componente di base con un titolo che dice 'Svelte to-do list'](02-todos-component-rendered.png)

## Aggiungere il markup statico

Per il momento inizieremo con una rappresentazione del markup statico della nostra app, così potrai vedere come apparirà. Copia e incolla quanto segue nel nostro file `Todos.svelte`, sostituendo il contenuto esistente:

```svelte
<!-- Todos.svelte -->
<div class="todoapp stack-large">
  <!-- NewTodo -->
  <form>
    <h2 class="label-wrapper">
      <label for="todo-0" class="label__lg"> What needs to be done? </label>
    </h2>
    <input type="text" id="todo-0" autocomplete="off" class="input input__lg" />
    <button type="submit" disabled="" class="btn btn__primary btn__lg">
      Add
    </button>
  </form>

  <!-- Filter -->
  <div class="filters btn-group stack-exception">
    <button class="btn toggle-btn" aria-pressed="true">
      <span class="visually-hidden">Show</span>
      <span>All</span>
      <span class="visually-hidden">tasks</span>
    </button>
    <button class="btn toggle-btn" aria-pressed="false">
      <span class="visually-hidden">Show</span>
      <span>Active</span>
      <span class="visually-hidden">tasks</span>
    </button>
    <button class="btn toggle-btn" aria-pressed="false">
      <span class="visually-hidden">Show</span>
      <span>Completed</span>
      <span class="visually-hidden">tasks</span>
    </button>
  </div>

  <!-- TodosStatus -->
  <h2 id="list-heading">2 out of 3 items completed</h2>

  <!-- Todos -->
  <ul role="list" class="todo-list stack-large" aria-labelledby="list-heading">
    <!-- todo-1 (editing mode) -->
    <li class="todo">
      <div class="stack-small">
        <form class="stack-small">
          <div class="form-group">
            <label for="todo-1" class="todo-label">
              New name for 'Create a Svelte starter app'
            </label>
            <input
              type="text"
              id="todo-1"
              autocomplete="off"
              class="todo-text" />
          </div>
          <div class="btn-group">
            <button class="btn todo-cancel" type="button">
              Cancel
              <span class="visually-hidden">renaming Create a Svelte starter app</span>
            </button>
            <button class="btn btn__primary todo-edit" type="submit">
              Save
              <span class="visually-hidden">new name for Create a Svelte starter app</span>
            </button>
          </div>
        </form>
      </div>
    </li>

    <!-- todo-2 -->
    <li class="todo">
      <div class="stack-small">
        <div class="c-cb">
          <input type="checkbox" id="todo-2" checked />
          <label for="todo-2" class="todo-label">
            Create your first component
          </label>
        </div>
        <div class="btn-group">
          <button type="button" class="btn">
            Edit
            <span class="visually-hidden">Create your first component</span>
          </button>
          <button type="button" class="btn btn__danger">
            Delete
            <span class="visually-hidden">Create your first component</span>
          </button>
        </div>
      </div>
    </li>

    <!-- todo-3 -->
    <li class="todo">
      <div class="stack-small">
        <div class="c-cb">
          <input type="checkbox" id="todo-3" />
          <label for="todo-3" class="todo-label">
            Complete the rest of the tutorial
          </label>
        </div>
        <div class="btn-group">
          <button type="button" class="btn">
            Edit
            <span class="visually-hidden">Complete the rest of the tutorial</span>
          </button>
          <button type="button" class="btn btn__danger">
            Delete
            <span class="visually-hidden">Complete the rest of the tutorial</span>
          </button>
        </div>
      </div>
    </li>
  </ul>

  <hr />

  <!-- MoreActions -->
  <div class="btn-group">
    <button type="button" class="btn btn__primary">Check all</button>
    <button type="button" class="btn btn__primary">Remove completed</button>
  </div>
</div>
```

Controlla di nuovo la resa, e vedrai qualcosa di simile a questo:

![Un'app to-do list, ma non stilizzata, con un titolo di `what needs to be done`, input, checkbox, ecc.](03-unstyled-todo-app.png)

Il markup HTML sopra non è molto ben stilizzato ed è anche funzionalmente inutile. Tuttavia, diamo un'occhiata al markup e vediamo come si rapporta alle nostre funzionalità desiderate:

- Un'etichetta e una casella di testo per inserire nuove attività
- Tre pulsanti per filtrare per stato dell'attività
- Un'etichetta che mostra il numero totale di attività e le attività completate
- Una lista non ordinata, che contiene un elemento di lista per ciascuna attività
- Quando l'attività è in fase di modifica, l'elemento di lista ha un input e due pulsanti per annullare o salvare le modifiche
- Se l'attività non è in fase di modifica, c'è un checkbox per impostare lo stato completato, e due pulsanti per modificare o eliminare l'attività
- Infine ci sono due pulsanti per selezionare/deselezionare tutte le attività e per rimuovere le attività completate

Nei successivi articoli faremo funzionare tutte queste funzionalità, e anche di più.

### Caratteristiche di accessibilità della lista di cose da fare

Potresti notare alcuni attributi insoliti qui. Per esempio:

```svelte
<button class="btn toggle-btn" aria-pressed="true">
  <span class="visually-hidden">Show</span>
  <span>All</span>
  <span class="visually-hidden">tasks</span>
</button>
```

Qui, `aria-pressed` comunica alla tecnologia assistiva (come i lettori di schermo) che il pulsante può trovarsi in uno di due stati: `premuto` o `non premuto`. Considerali come analoghi a acceso e spento. Impostare un valore di `true` significa che il pulsante è premuto di default.

La classe `visually-hidden` non ha ancora effetto, perché non abbiamo incluso alcun CSS. Una volta che avremo messo i nostri stili, tuttavia, qualsiasi elemento con questa classe sarà nascosto agli utenti vedenti e ancora disponibile per gli utenti dei lettori di schermo — questo perché queste parole non sono necessarie per gli utenti vedenti; sono lì per fornire maggiori informazioni su ciò che fa il pulsante per gli utenti dei lettori di schermo che non hanno il contesto visivo extra per aiutarli.

Più in basso, puoi trovare il seguente elemento `<ul>`:

```svelte
<ul
  role="list"
  class="todo-list stack-large"
  aria-labelledby="list-heading">
```

L'attributo `role` aiuta la tecnologia assistiva a spiegare quale tipo di valore semantico ha un elemento — o quale è il suo scopo. Un `<ul>` è trattato di default come una lista, ma gli stili che stiamo per aggiungere romperanno quella funzionalità. Questo ruolo ripristinerà il significato di "lista" all'elemento `<ul>`. Se vuoi saperne di più sul perché questo è necessario, puoi controllare l'articolo di Scott O'Hara ["Fixing Lists"](https://www.scottohara.me/blog/2019/01/12/lists-and-safari.html) (2019).

L'attributo `aria-labelledby` dice alle tecnologie assistive che stiamo trattando il nostro `<h2>` con un `id` di `list-heading` come l'etichetta che descrive lo scopo della lista sotto di esso. Fare questa associazione dà alla lista un contesto più informativo, che potrebbe aiutare gli utenti dei lettori di schermo a capire meglio il suo scopo.

Questo sembra il momento giusto per parlare di come Svelte affronta l'accessibilità; facciamolo ora.

## Supporto all'accessibilità di Svelte

Svelte ha un'enfasi speciale sull'accessibilità. L'intenzione è quella di incoraggiare gli sviluppatori a scrivere codice più accessibile "di default". Essendo un compilatore, Svelte può analizzare staticamente i nostri modelli HTML per fornire avvisi di accessibilità quando i componenti sono in fase di compilazione.

L'accessibilità (abbreviata in a11y) non è sempre facile da gestire correttamente, ma Svelte ti aiuterà avvisandoti se scrivi markup inaccessibile.

Per esempio, se aggiungiamo un elemento `<img>` al nostro componente `todos.svelte` senza la sua corrispondente prop `alt`:

```svelte
<h1>Svelte To-Do list</h1>

<img height="32" width="88" src="https://www.w3.org/WAI/wcag2A" />
```

Il compilatore emetterà il seguente avviso:

```bash
(!) Plugin svelte: A11y: <img> element should have an alt attribute
src/components/Todos.svelte
1: <h1>Svelte To-Do list</h1>
2:
3: <img height="32" width="88" src="https://www.w3.org/WAI/wcag2A">
   ^
created public/build/bundle.js in 220ms

[2020-07-15 04:07:43] waiting for changes...
```

Inoltre, il nostro editor può visualizzare questo avviso anche prima di chiamare il compilatore:

![Una finestra dell'editor di codice che mostra un tag immagine, con un messaggio di errore popup che dice che l'elemento dovrebbe avere un attributo alt](04-svelte-accessibility-support.png)

È possibile dire a Svelte di ignorare questo avviso per il prossimo blocco di markup con un [commento](https://svelte.dev/docs/basic-markup#comments) che inizia con `svelte-ignore`, come questo:

```svelte
<!-- svelte-ignore a11y-missing-attribute -->
<img height="32" width="88" src="https://www.w3.org/WAI/wcag2A" />
```

> [!NOTE]
> Con VS Code puoi aggiungere automaticamente questo commento di ignoranza cliccando sul link _Quick fix…_ o premendo <kbd>Ctrl</kbd> + <kbd>.</kbd>.

Se desideri disabilitare globalmente questo avviso, puoi aggiungere questo gestore `onwarn` al tuo file `rollup.config.js` all'interno della configurazione per il plugin `Svelte`, come questo:

```js
export default {
  // …
  plugins: [
    svelte({
      dev: !production,
      css(css) {
        css.write("public/build/bundle.css");
      },
      // Warnings are normally passed straight to Rollup. You can
      // optionally handle them here, for example to squelch
      // warnings with a particular code
      onwarn(warning, handler) {
        // e.g. I don't care about screen readers -> please DON'T DO THIS!!!
        if (warning.code === "a11y-missing-attribute") {
          return;
        }

        // let Rollup handle all other warnings normally
        handler(warning);
      },
    }),

    // …
  ],
  // …
};
```

Per design, questi avvisi sono implementati direttamente nel compilatore e non come un plugin che puoi scegliere di aggiungere al tuo progetto. L'idea è di controllare le problematiche di a11y nel tuo markup di default e permetterti di escludere avvisi specifici.

> [!NOTE]
> Dovresti disabilitare questi avvisi solo se hai buone ragioni per farlo, per esempio mentre costruisci un prototipo veloce. È importante essere un buon cittadino del web e rendere le tue pagine accessibili alla base utente più ampia possibile.

Le regole di accessibilità controllate da Svelte sono prese da [eslint-plugin-jsx-a11y](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y#supported-rules), un plugin per ESLint che fornisce controlli statici per molte regole di accessibilità sugli elementi JSX. Svelte mira ad implementarle tutte nel suo compilatore, e la maggior parte di esse è già stata portata in Svelte. Su GitHub puoi vedere [quali controlli di accessibilità sono ancora mancanti](https://github.com/sveltejs/svelte/issues/820). Puoi controllare il significato di ciascuna regola cliccando sul suo link.

## Stili per il nostro markup

Rendiamo la lista di cose da fare un po' più bella. Sostituisci il contenuto del file `public/global.css` con il seguente:

```css
/* RESETS */
*,
*::before,
*::after {
  box-sizing: border-box;
}
*:focus {
  outline: 3px dashed #228bec;
  outline-offset: 0;
}
html {
  font: 62.5% / 1.15 sans-serif;
}
h1,
h2 {
  margin-bottom: 0;
}
ul {
  list-style: none;
  padding: 0;
}
button {
  border: none;
  margin: 0;
  padding: 0;
  width: auto;
  overflow: visible;
  background: transparent;
  color: inherit;
  font: inherit;
  line-height: normal;
  -webkit-font-smoothing: inherit;
  -moz-osx-font-smoothing: inherit;
  appearance: none;
}
button::-moz-focus-inner {
  border: 0;
}
button,
input,
optgroup,
select,
textarea {
  font-family: inherit;
  font-size: 100%;
  line-height: 1.15;
  margin: 0;
}
button,
input {
  overflow: visible;
}
input[type="text"] {
  border-radius: 0;
}
body {
  width: 100%;
  max-width: 68rem;
  margin: 0 auto;
  font:
    1.6rem/1.25 Arial,
    sans-serif;
  background-color: #f5f5f5;
  color: #4d4d4d;
}
@media screen and (min-width: 620px) {
  body {
    font-size: 1.9rem;
    line-height: 1.31579;
  }
}
/*END RESETS*/

/* GLOBAL STYLES */
.form-group > input[type="text"] {
  display: inline-block;
  margin-top: 0.4rem;
}
.btn {
  padding: 0.8rem 1rem 0.7rem;
  border: 0.2rem solid #4d4d4d;
  cursor: pointer;
  text-transform: capitalize;
}
.btn.toggle-btn {
  border-width: 1px;
  border-color: #d3d3d3;
}
.btn.toggle-btn[aria-pressed="true"] {
  text-decoration: underline;
  border-color: #4d4d4d;
}
.btn__danger {
  color: #fff;
  background-color: #ca3c3c;
  border-color: #bd2130;
}
.btn__filter {
  border-color: lightgrey;
}
.btn__primary {
  color: #fff;
  background-color: #000;
}
.btn__primary:disabled {
  color: darkgrey;
  background-color: #565656;
}
.btn-group {
  display: flex;
  justify-content: space-between;
}
.btn-group > * {
  flex: 1 1 49%;
}
.btn-group > * + * {
  margin-left: 0.8rem;
}
.label-wrapper {
  margin: 0;
  flex: 0 0 100%;
  text-align: center;
}
.visually-hidden {
  position: absolute !important;
  height: 1px;
  width: 1px;
  overflow: hidden;
  clip: rect(1px, 1px, 1px, 1px);
  white-space: nowrap;
}
[class*="stack"] > * {
  margin-top: 0;
  margin-bottom: 0;
}
.stack-small > * + * {
  margin-top: 1.25rem;
}
.stack-large > * + * {
  margin-top: 2.5rem;
}
@media screen and (min-width: 550px) {
  .stack-small > * + * {
    margin-top: 1.4rem;
  }
  .stack-large > * + * {
    margin-top: 2.8rem;
  }
}
.stack-exception {
  margin-top: 1.2rem;
}
/* END GLOBAL STYLES */

.todoapp {
  background: #fff;
  margin: 2rem 0 4rem 0;
  padding: 1rem;
  position: relative;
  box-shadow:
    0 2px 4px 0 rgb(0 0 0 / 20%),
    0 2.5rem 5rem 0 rgb(0 0 0 / 10%);
}
@media screen and (min-width: 550px) {
  .todoapp {
    padding: 4rem;
  }
}
.todoapp > * {
  max-width: 50rem;
  margin-left: auto;
  margin-right: auto;
}
.todoapp > form {
  max-width: 100%;
}
.todoapp > h1 {
  display: block;
  max-width: 100%;
  text-align: center;
  margin: 0;
  margin-bottom: 1rem;
}
.label__lg {
  line-height: 1.01567;
  font-weight: 300;
  padding: 0.8rem;
  margin-bottom: 1rem;
  text-align: center;
}
.input__lg {
  padding: 2rem;
  border: 2px solid #000;
}
.input__lg:focus {
  border-color: #4d4d4d;
  box-shadow: inset 0 0 0 2px;
}
[class*="__lg"] {
  display: inline-block;
  width: 100%;
  font-size: 1.9rem;
}
[class*="__lg"]:not(:last-child) {
  margin-bottom: 1rem;
}
@media screen and (min-width: 620px) {
  [class*="__lg"] {
    font-size: 2.4rem;
  }
}
.filters {
  width: 100%;
  margin: unset;
}
/* Todo item styles */
.todo {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
}
.todo > * {
  flex: 0 0 100%;
}
.todo-text {
  width: 100%;
  min-height: 4.4rem;
  padding: 0.4rem 0.8rem;
  border: 2px solid #565656;
}
.todo-text:focus {
  box-shadow: inset 0 0 0 2px;
}
/* CHECKBOX STYLES */
.c-cb {
  box-sizing: border-box;
  font-family: Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  font-weight: 400;
  font-size: 1.6rem;
  line-height: 1.25;
  display: block;
  position: relative;
  min-height: 44px;
  padding-left: 40px;
  clear: left;
}
.c-cb > label::before,
.c-cb > input[type="checkbox"] {
  box-sizing: border-box;
  top: -2px;
  left: -2px;
  width: 44px;
  height: 44px;
}
.c-cb > input[type="checkbox"] {
  -webkit-font-smoothing: antialiased;
  cursor: pointer;
  position: absolute;
  z-index: 1;
  margin: 0;
  opacity: 0;
}
.c-cb > label {
  font-size: inherit;
  font-family: inherit;
  line-height: inherit;
  display: inline-block;
  margin-bottom: 0;
  padding: 8px 15px 5px;
  cursor: pointer;
  touch-action: manipulation;
}
.c-cb > label::before {
  content: "";
  position: absolute;
  border: 2px solid currentcolor;
  background: transparent;
}
.c-cb > input[type="checkbox"]:focus + label::before {
  border-width: 4px;
  outline: 3px dashed #228bec;
}
.c-cb > label::after {
  box-sizing: content-box;
  content: "";
  position: absolute;
  top: 11px;
  left: 9px;
  width: 18px;
  height: 7px;
  transform: rotate(-45deg);
  border: solid;
  border-width: 0 0 5px 5px;
  border-top-color: transparent;
  opacity: 0;
  background: transparent;
}
.c-cb > input[type="checkbox"]:checked + label::after {
  opacity: 1;
}
```

Con il nostro markup stilizzato, tutto ora appare meglio:

![La nostra app di lista di cose da fare, stilizzata, con un titolo 'cosa deve essere fatto', un input per inserire più to-do, e una lista di to-do con checkbox](05-styled-todo-app.png)

## Il codice finora

### Git

Per vedere lo stato del codice come dovrebbe essere alla fine di questo articolo, accedi alla tua copia del nostro repository in questo modo:

```bash
cd mdn-svelte-tutorial/03-adding-dynamic-behavior
```

Oppure scarica direttamente il contenuto della cartella:

```bash
npx degit opensas/mdn-svelte-tutorial/03-adding-dynamic-behavior
```

Ricorda di eseguire `npm install && npm run dev` per avviare la tua app in modalità sviluppo.

### REPL

Per vedere lo stato corrente del codice in un REPL, visita:

<https://svelte.dev/repl/c862d964d48d473ca63ab91709a0a5a0?version=3.23.2>

## Sommario

Con il nostro markup e lo stile in atto, la nostra app di lista di cose da fare sta iniziando a prendere forma, e abbiamo tutto pronto per iniziare a concentrarci sulle funzionalità che dobbiamo implementare.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Svelte_getting_started","Learn_web_development/Core/Frameworks_libraries/Svelte_variables_props", "Learn_web_development/Core/Frameworks_libraries")}}
