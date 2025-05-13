---
title: Iniziare la nostra app di lista di cose da fare con Svelte
slug: Learn_web_development/Core/Frameworks_libraries/Svelte_Todo_list_beginning
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Svelte_getting_started","Learn_web_development/Core/Frameworks_libraries/Svelte_variables_props", "Learn_web_development/Core/Frameworks_libraries")}}

Ora che abbiamo una comprensione di base del funzionamento di Svelte, possiamo iniziare a costruire la nostra app di esempio: una lista di cose da fare. In questo articolo vedremo prima la funzionalità desiderata della nostra app, quindi creeremo un componente `Todos.svelte` e metteremo in atto il markup statico e gli stili, lasciando tutto pronto per iniziare a sviluppare le funzionalità della nostra app di lista di cose da fare, che svilupperemo in articoli successivi.

Vogliamo che i nostri utenti possano sfogliare, aggiungere e eliminare attività, e anche segnarle come completate. Questa sarà la funzionalità di base che svilupperemo in questa serie di tutorial, e lungo il percorso vedremo anche alcuni concetti più avanzati.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          È consigliato, come minimo, avere familiarità con i linguaggi di base
          <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e
          avere conoscenze del
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminale/linea di comando</a
          >.
        </p>
        <p>
          Avrà bisogno di un terminale con node + npm installati per compilare e costruire
          la sua app.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare a creare un componente Svelte, renderizzarlo all'interno di un altro
        componente, passare dati in esso usando le props e salvare il suo stato.
      </td>
    </tr>
  </tbody>
</table>

## Codificare insieme a noi

### Git

Cloni il repo di GitHub (se non l'ha già fatto) con:

```bash
git clone https://github.com/opensas/mdn-svelte-tutorial.git
```

Poi, per ottenere lo stato attuale dell'app, esegua

```bash
cd mdn-svelte-tutorial/02-starting-our-todo-app
```

Oppure scarichi direttamente il contenuto della cartella:

```bash
npx degit opensas/mdn-svelte-tutorial/02-starting-our-todo-app
```

Ricordi di eseguire `npm install && npm run dev` per avviare la sua app in modalità di sviluppo.

### REPL

Per codificare insieme a noi usando la REPL, inizi da

<https://svelte.dev/repl/b7b831ea3a354d3789cefbc31e2ca495?version=3.23.2>

## Funzionalità dell'app di lista di cose da fare

Questo è come apparirà la nostra app di lista di cose da fare una volta pronta:

![app di lista di cose da fare tipica, con un titolo 'cosa c'è da fare', un input per inserire altre cose da fare e una lista di cose da fare con delle caselle di controllo](01-todo-list-app.png)

Usando questa interfaccia utente l'utente sarà in grado di:

- Sfogliare le loro attività
- Contrassegnare le attività come completate/in attesa senza eliminarle
- Rimuovere attività
- Aggiungere nuove attività
- Filtrare le attività per stato: tutte le attività, attività attive o completate
- Modificare le attività
- Contrassegnare tutte le attività come attive/completate
- Rimuovere tutte le attività completate

## Costruire il nostro primo componente

Creiamo un componente `Todos.svelte`. Questo conterrà la nostra lista di cose da fare.

1. Crei una nuova cartella — `src/components`.

   > [!NOTE]
   > Può mettere i suoi componenti ovunque all'interno della cartella `src`, ma la cartella `components` è una convenzione riconosciuta da seguire, permettendo di trovare facilmente i suoi componenti.

2. Crei un file denominato `src/components/Todos.svelte` con il seguente contenuto:

   ```svelte
   <h1>Svelte to-do list</h1>
   ```

3. Cambi l'elemento `title` in `public/index.html` per contenere il testo _Svelte to-do list_:

   ```svelte
   <title>Svelte to-do list</title>
   ```

4. Apra `src/App.svelte` e sostituisca il suo contenuto con il seguente:

   ```svelte
   <script>
     import Todos from "./components/Todos.svelte";
   </script>

   <Todos />
   ```

5. In modalità di sviluppo, Svelte emetterà un avviso nella console del browser quando si specifica un prop che non esiste nel componente; in questo caso abbiamo un prop `name` specificato quando instanziamo il componente `App` all'interno di `src/main.js`, che non viene utilizzato all'interno di `App`. La console dovrebbe attualmente fornirle un messaggio simile a "<App> è stato creato con prop sconosciuto 'name'". Per eliminare questo, rimuova il prop `name` da `src/main.js`; dovrebbe ora apparire così:

   ```js
   import App from "./App.svelte";

   const app = new App({
     target: document.body,
   });

   export default app;
   ```

Ora se controlla l'URL del suo server di testing vedrà il nostro componente `Todos.svelte` essere renderizzato:

![basico rendering del componente con un titolo che dice 'Svelte to-do list'](02-todos-component-rendered.png)

## Aggiungere markup statico

Per il momento inizieremo con una rappresentazione del markup statico della nostra app, in modo da vedere com'è. Copi e incolli quanto segue nel nostro file del componente `Todos.svelte`, sostituendo il contenuto esistente:

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

Controlli nuovamente il rendering, e vedrà qualcosa di simile a questo:

![Un'app di lista di cose da fare, ma non stilizzata, con un titolo 'cosa c'è da fare', input, caselle di controllo, ecc.](03-unstyled-todo-app.png)

Il markup HTML sopra non è molto ben stilizzato ed è anche funzionalmente inutile. Tuttavia, diamo un'occhiata al markup e vediamo come si relaziona alle nostre funzionalità desiderate:

- Un'etichetta e una casella di testo per inserire nuove attività
- Tre pulsanti per filtrare per stato delle attività
- Un'etichetta che mostra il numero totale di attività e le attività completate
- Una lista non ordinata, che contiene un elemento della lista per ogni attività
- Quando l'attività viene modificata, l'elemento della lista ha un input e due pulsanti per annullare o salvare le modifiche
- Se l'attività non viene modificata, c'è una casella di controllo per impostare lo stato completato, e due pulsanti per modificare o eliminare l'attività
- Infine ci sono due pulsanti per spuntare/contrassegnare tutte le attività e per rimuovere le attività completate

Nei successivi articoli faremo funzionare tutte queste funzionalità, e anche di più.

### Caratteristiche di accessibilità della lista di cose da fare

Può notare alcuni attributi insoliti qui. Ad esempio:

```svelte
<button class="btn toggle-btn" aria-pressed="true">
  <span class="visually-hidden">Show</span>
  <span>All</span>
  <span class="visually-hidden">tasks</span>
</button>
```

Qui, `aria-pressed` informa la tecnologia assistiva (come i lettori di schermo) che il pulsante può trovarsi in uno di due stati: `pressed` o `unpressed`. Li consideri come analoghi per acceso e spento. Impostare un valore di `true` significa che il pulsante è premuto per impostazione predefinita.

La classe `visually-hidden` non ha ancora effetto, perché non abbiamo incluso alcun CSS. Una volta che avremo messo in atto i nostri stili, tuttavia, qualsiasi elemento con questa classe sarà nascosto agli utenti vedenti e ancora disponibile per i lettori di schermo — questo perché queste parole non sono necessarie per gli utenti vedenti; sono lì per fornire più informazioni su ciò che fa il pulsante per gli utenti dei lettori di schermo che non hanno il contesto visivo extra per aiutarli.

Più in basso, può trovare il seguente elemento `<ul>`:

```svelte
<ul
  role="list"
  class="todo-list stack-large"
  aria-labelledby="list-heading">
```

L'attributo `role` aiuta la tecnologia assistiva a spiegare quale tipo di valore semantico ha un elemento — o quale sia il suo scopo. Un `<ul>` è trattato di default come una lista, ma gli stili che stiamo per aggiungere romperanno quella funzionalità. Questo ruolo restaurerà il significato di "lista" all'elemento `<ul>`. Se vuole saperne di più sul perché ciò è necessario, può consultare l'articolo di Scott O'Hara ["Fixing Lists"](https://www.scottohara.me/blog/2019/01/12/lists-and-safari.html) (2019).

L'attributo `aria-labelledby` informa le tecnologie assistive che stiamo trattando il nostro `<h2>` con un `id` di `list-heading` come etichetta che descrive lo scopo della lista sotto di esso. Fare questa associazione fornisce al elenco un contesto più informativo, che potrebbe aiutare gli utenti dei lettori di schermo a comprendere meglio il suo scopo.

Questo sembra un buon momento per parlare di come Svelte gestisce l'accessibilità; facciamolo ora.

## Supporto all'accessibilità di Svelte

Svelte ha una particolare enfasi sull'accessibilità. L'intenzione è di incoraggiare gli sviluppatori a scrivere codice più accessibile "di default". Essendo un compilatore, Svelte può analizzare staticamente i nostri modelli HTML per fornire avvisi di accessibilità quando i componenti vengono compilati.

L'accessibilità (abbreviata in a11y) non è sempre facile da fare correttamente, ma Svelte aiuterà avvisandola se scrive markup inaccessibile.

Ad esempio, se aggiungessimo un elemento `<img>` al nostro componente `todos.svelte` senza il suo corrispondente prop `alt`:

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

![Una finestra dell'editor di codice che mostra un'immagine tag, con un messaggio di errore a comparsa che dice che l'elemento dovrebbe avere un attributo alt](04-svelte-accessibility-support.png)

Può dire a Svelte di ignorare questo avviso per il prossimo blocco di markup con un [commento](https://svelte.dev/docs/basic-markup#comments) che inizia con `svelte-ignore`, così:

```svelte
<!-- svelte-ignore a11y-missing-attribute -->
<img height="32" width="88" src="https://www.w3.org/WAI/wcag2A" />
```

> [!NOTE]
> Con VS Code può aggiungere automaticamente questo commento di ignoramento facendo clic sul collegamento _Quick fix…_ o premendo <kbd>Ctrl</kbd> + <kbd>.</kbd>.

Se vuole disabilitare globalmente questo avviso, può aggiungere questo gestore `onwarn` al suo file `rollup.config.js` all'interno della configurazione per il plugin `Svelte`, così:

```js
plugins: [
  svelte({
    dev: !production,
    css: (css) => {
      css.write("public/build/bundle.css");
    },
    // Warnings are normally passed straight to Rollup. You can
    // optionally handle them here, for example to squelch
    // warnings with a particular code
    onwarn: (warning, handler) => {
      // e.g. I don't care about screen readers -> please DON'T DO THIS!!!
      if (warning.code === "a11y-missing-attribute") {
        return;
      }

      // let Rollup handle all other warnings normally
      handler(warning);
    },
  }),

  // …
];
```

Per design, questi avvisi sono implementati nel compilatore stesso, e non come un plug-in che può scegliere di aggiungere al suo progetto. L'idea è di controllare per i problemi di a11y nel suo markup per impostazione predefinita e lasciarle optare per l'uscita da avvisi specifici.

> [!NOTE]
> Dovrebbe disabilitare questi avvisi solo se ha buone ragioni per farlo, ad esempio durante la costruzione di un prototipo rapido. È importante essere un buon cittadino del web e rendere le sue pagine accessibili alla più ampia base di utenti possibile.

Le regole di accessibilità verificate da Svelte sono prese da [eslint-plugin-jsx-a11y](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y#supported-rules), un plug-in per ESLint che fornisce controlli statici per molte regole di accessibilità sugli elementi JSX. Svelte mira a implementare tutte queste regole nel suo compilatore, e la maggior parte di esse è già stata portata su Svelte. Su GitHub può vedere [quali controlli di accessibilità mancano ancora](https://github.com/sveltejs/svelte/issues/820). Può verificare il significato di ciascuna regola facendo clic sul relativo link.

## Stilizzare il nostro markup

Miglioriamo un po' l'aspetto della lista di cose da fare. Sostituisca il contenuto del file `public/global.css` con il seguente:

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
  clip: rect(1px 1px 1px 1px);
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
  margin: unset auto;
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

Con il nostro markup stilizzato, ora tutto appare meglio:

![La nostra app di lista di cose da fare, stilizzata, con un titolo 'cosa c'è da fare', un input per inserire altre cose da fare e una lista di cose da fare con delle caselle di controllo](05-styled-todo-app.png)

## Il codice finora

### Git

Per vedere lo stato del codice come dovrebbe essere alla fine di questo articolo, acceda alla sua copia del nostro repo così:

```bash
cd mdn-svelte-tutorial/03-adding-dynamic-behavior
```

Oppure scarichi direttamente il contenuto della cartella:

```bash
npx degit opensas/mdn-svelte-tutorial/03-adding-dynamic-behavior
```

Ricordi di eseguire `npm install && npm run dev` per avviare la sua app in modalità di sviluppo.

### REPL

Per vedere lo stato attuale del codice in una REPL, visiti:

<https://svelte.dev/repl/c862d964d48d473ca63ab91709a0a5a0?version=3.23.2>

## Sommario

Con il nostro markup e gli stili in atto, la nostra app di lista di cose da fare sta iniziando a prendere forma, e abbiamo tutto pronto affinché possiamo concentrarci sulle funzionalità che dobbiamo implementare.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Svelte_getting_started","Learn_web_development/Core/Frameworks_libraries/Svelte_variables_props", "Learn_web_development/Core/Frameworks_libraries")}}
