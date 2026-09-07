---
title: Iniziare la nostra app ToDo in React
short-title: App ToDo in React
slug: Learn_web_development/Core/Frameworks_libraries/React_todo_list_beginning
l10n:
  sourceCommit: 9f7e7e9075e9f2b1937d2c8000f52a8ff76bff52
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/React_getting_started","Learn_web_development/Core/Frameworks_libraries/React_components", "Learn_web_development/Core/Frameworks_libraries")}}

Supponiamo di dover creare un proof of concept in React: un'app che consenta agli utenti di aggiungere, modificare ed eliminare le attività su cui vogliono lavorare, oltre a contrassegnare le attività come completate senza eliminarle. Questo articolo guida attraverso la struttura di base e lo stile di un'applicazione di questo tipo, pronta per la definizione dei singoli componenti e dell'interattività, che verranno aggiunti in seguito.

> [!NOTE]
> Se è necessario confrontare il proprio codice con la nostra versione, una versione completa del codice di esempio dell'app React è disponibile nel repository [todo-react](https://github.com/mdn/todo-react). Per una versione live in esecuzione, vedere <https://mdn.github.io/todo-react/>.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi fondamentali <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, nonché con il <a href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line">terminale/riga di comando</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        Familiarità con il nostro caso di studio della lista di attività e predisposizione
        della struttura e dello stile di base di <code>App</code>.
      </td>
    </tr>
  </tbody>
</table>

## Storie utente della nostra app

Nello sviluppo software, una storia utente è un obiettivo attuabile dal punto di vista dell'utente. Definire le storie utente prima di iniziare il lavoro aiuta a mantenerne il focus. La nostra app dovrebbe soddisfare le seguenti storie:

Come utente, è possibile:

- leggere un elenco di attività;
- aggiungere un'attività usando il mouse o la tastiera;
- contrassegnare qualsiasi attività come completata, usando il mouse o la tastiera;
- eliminare qualsiasi attività, usando il mouse o la tastiera;
- modificare qualsiasi attività, usando il mouse o la tastiera;
- visualizzare uno specifico sottoinsieme di attività: tutte le attività, solo l'attività attiva oppure solo le attività completate.

Affronteremo queste storie una alla volta.

## Pulizia preliminare del progetto

Vite ha fornito del codice che non verrà usato affatto per il progetto. I seguenti comandi del terminale lo elimineranno per fare spazio al nuovo progetto. Assicurarsi di partire dalla directory radice dell'app.

```bash
# Move into the src directory
cd src
# Delete the App.css file and the React logo provided by Vite
rm App.css assets/react.svg
# Empty the contents of App.jsx and index.css
echo -n > App.jsx && echo -n > index.css
# Move back up to the root of the project
cd ..
```

> [!NOTE]
> Se il server è stato arrestato per eseguire le attività nel terminale menzionate sopra, sarà necessario avviarlo di nuovo usando `npm run dev`.

## Codice iniziale del progetto

Come punto di partenza per questo progetto, verranno forniti due elementi: una funzione `App()` per sostituire quella appena eliminata e del CSS per definire lo stile dell'app.

### Il JSX

Copiare il seguente frammento negli appunti, quindi incollarlo in `App.jsx`:

```jsx
function App(props) {
  return (
    <div className="todoapp stack-large">
      <h1>TodoMatic</h1>
      <form>
        <h2 className="label-wrapper">
          <label htmlFor="new-todo-input" className="label__lg">
            What needs to be done?
          </label>
        </h2>
        <input
          type="text"
          id="new-todo-input"
          className="input input__lg"
          name="text"
          autoComplete="off"
        />
        <button type="submit" className="btn btn__primary btn__lg">
          Add
        </button>
      </form>
      <div className="filters btn-group stack-exception">
        <button type="button" className="btn toggle-btn" aria-pressed="true">
          <span className="visually-hidden">Show </span>
          <span>all</span>
          <span className="visually-hidden"> tasks</span>
        </button>
        <button type="button" className="btn toggle-btn" aria-pressed="false">
          <span className="visually-hidden">Show </span>
          <span>Active</span>
          <span className="visually-hidden"> tasks</span>
        </button>
        <button type="button" className="btn toggle-btn" aria-pressed="false">
          <span className="visually-hidden">Show </span>
          <span>Completed</span>
          <span className="visually-hidden"> tasks</span>
        </button>
      </div>
      <h2 id="list-heading">3 tasks remaining</h2>
      <ul
        role="list"
        className="todo-list stack-large stack-exception"
        aria-labelledby="list-heading">
        <li className="todo stack-small">
          <div className="c-cb">
            <input id="todo-0" type="checkbox" defaultChecked />
            <label className="todo-label" htmlFor="todo-0">
              Eat
            </label>
          </div>
          <div className="btn-group">
            <button type="button" className="btn">
              Edit <span className="visually-hidden">Eat</span>
            </button>
            <button type="button" className="btn btn__danger">
              Delete <span className="visually-hidden">Eat</span>
            </button>
          </div>
        </li>
        <li className="todo stack-small">
          <div className="c-cb">
            <input id="todo-1" type="checkbox" />
            <label className="todo-label" htmlFor="todo-1">
              Sleep
            </label>
          </div>
          <div className="btn-group">
            <button type="button" className="btn">
              Edit <span className="visually-hidden">Sleep</span>
            </button>
            <button type="button" className="btn btn__danger">
              Delete <span className="visually-hidden">Sleep</span>
            </button>
          </div>
        </li>
        <li className="todo stack-small">
          <div className="c-cb">
            <input id="todo-2" type="checkbox" />
            <label className="todo-label" htmlFor="todo-2">
              Repeat
            </label>
          </div>
          <div className="btn-group">
            <button type="button" className="btn">
              Edit <span className="visually-hidden">Repeat</span>
            </button>
            <button type="button" className="btn btn__danger">
              Delete <span className="visually-hidden">Repeat</span>
            </button>
          </div>
        </li>
      </ul>
    </div>
  );
}

export default App;
```

Ora aprire `index.html` e modificare il testo dell'elemento [`<title>`](/it/docs/Web/HTML/Reference/Elements/title) in `TodoMatic`. In questo modo, corrisponderà all'elemento [`<h1>`](/it/docs/Web/HTML/Reference/Elements/Heading_Elements) nella parte superiore dell'app.

```html
<title>TodoMatic</title>
```

Quando il browser si aggiorna, dovrebbe essere visualizzato qualcosa di simile a questo:

![app todo-matic senza stile, che mostra un insieme confuso di etichette, input e pulsanti](unstyled-app.png)

Non è bella e non funziona ancora, ma va bene: tra poco verrà definito il suo stile. Prima, si consideri il JSX disponibile e il modo in cui corrisponde alle storie utente:

- È presente un elemento [`<form>`](/it/docs/Web/HTML/Reference/Elements/form), con un [`<input type="text">`](/it/docs/Web/HTML/Reference/Elements/input/text) per scrivere una nuova attività e un pulsante per inviare il modulo.
- È presente un array di pulsanti che verranno usati per filtrare le attività.
- È presente un'intestazione che indica quante attività rimangono.
- Sono presenti 3 attività, organizzate in un elenco non ordinato. Ogni attività è una voce di elenco ([`<li>`](/it/docs/Web/HTML/Reference/Elements/li)) e dispone di pulsanti per modificarla ed eliminarla, nonché di una casella di controllo per contrassegnarla come completata.

Il modulo consentirà di _creare_ attività; i pulsanti consentiranno di _filtrarle_; l'intestazione e l'elenco sono il modo per _leggerle_. L'interfaccia utente per _modificare_ un'attività è per ora vistosamente assente. Va bene così: verrà scritta in seguito.

### Funzionalità di accessibilità

Si potrebbe notare del markup insolito. Ad esempio:

```jsx
<button type="button" className="btn toggle-btn" aria-pressed="true">
  <span className="visually-hidden">Show </span>
  <span>all</span>
  <span className="visually-hidden"> tasks</span>
</button>
```

In questo caso, `aria-pressed` indica alle tecnologie assistive, come gli screen reader, che il pulsante può trovarsi in uno di due stati: `pressed` oppure `unpressed`. Si possono considerare come analoghi di `on` e `off`. L'impostazione del valore `"true"` indica che il pulsante è premuto per impostazione predefinita.

La classe `visually-hidden` non ha ancora alcun effetto, poiché non è stato incluso alcun CSS. Tuttavia, una volta applicati gli stili, qualsiasi elemento con questa classe sarà nascosto agli utenti vedenti e rimarrà disponibile agli utenti delle tecnologie assistive. Questo perché tali parole non sono necessarie per gli utenti vedenti: servono a fornire maggiori informazioni su ciò che fa il pulsante agli utenti delle tecnologie assistive che non dispongono del contesto visivo aggiuntivo.

Più avanti si trova l'elemento [`<ul>`](/it/docs/Web/HTML/Reference/Elements/ul):

```jsx
<ul
  role="list"
  className="todo-list stack-large stack-exception"
  aria-labelledby="list-heading">
  …
</ul>
```

L'attributo `role` aiuta le tecnologie assistive a spiegare quale tipo di elemento rappresenta un tag. Per impostazione predefinita, un `<ul>` viene trattato come un elenco, ma gli stili che stanno per essere aggiunti ne interromperanno la funzionalità. Questo ruolo ripristinerà il significato di "elenco" per l'elemento `<ul>`. Per ulteriori informazioni sul motivo per cui è necessario, si può consultare l'articolo di [Scott O'Hara, "Fixing Lists"](https://www.scottohara.me/blog/2019/01/12/lists-and-safari.html).

L'attributo `aria-labelledby` indica alle tecnologie assistive che l'intestazione dell'elenco viene trattata come l'etichetta che descrive lo scopo dell'elenco sottostante. Creare questa associazione fornisce all'elenco un contesto più informativo, che può aiutare gli utenti delle tecnologie assistive a comprenderne meglio lo scopo.

Infine, le etichette e gli input nelle voci dell'elenco possiedono alcuni attributi specifici di JSX:

```jsx
<div className="c-cb">
  <input id="todo-0" type="checkbox" defaultChecked />
  <label className="todo-label" htmlFor="todo-0">
    Eat
  </label>
</div>
```

L'attributo `defaultChecked` nel tag `<input />` indica a React di selezionare inizialmente questa casella di controllo. Se venisse usato `checked`, come nel normale HTML, React registrerebbe alcuni avvisi nella console del browser relativi alla gestione degli eventi sulla casella di controllo, cosa da evitare. Non è necessario preoccuparsene troppo per ora: l'argomento verrà trattato più avanti, quando si arriverà all'uso degli eventi.

L'attributo `htmlFor` corrisponde all'attributo `for` usato in HTML. Non è possibile usare `for` come attributo in JSX perché `for` è una parola riservata, quindi React usa invece `htmlFor`.

### Una nota sugli attributi booleani in JSX

L'attributo `defaultChecked` nella sezione precedente è un attributo booleano, ovvero un attributo il cui valore può essere `true` oppure `false`. Come in HTML, un attributo booleano è `true` se è presente e `false` se è assente; l'assegnazione sul lato destro dell'espressione è facoltativa. È possibile impostarne esplicitamente il valore passandolo tra parentesi graffe, ad esempio `defaultChecked={true}` oppure `defaultChecked={false}`.

Poiché JSX è JavaScript, esiste un dettaglio da tenere presente con gli attributi booleani: scrivere `defaultChecked="false"` imposterà un valore _stringa_ di `"false"` anziché un valore _booleano_. Le stringhe non vuote sono {{Glossary("Truthy", "truthy")}}, quindi React considererà `defaultChecked` come `true` e selezionerà la casella di controllo per impostazione predefinita. Non è ciò che si desidera, quindi è opportuno evitarlo.

Se si desidera, è possibile esercitarsi a scrivere attributi booleani con un altro attributo già visto in precedenza, [`hidden`](/it/docs/Web/HTML/Reference/Global_attributes/hidden), che impedisce il rendering degli elementi nella pagina. Provare ad aggiungere `hidden` all'elemento `<h1>` in `App.jsx` per vedere cosa accade, quindi provare a impostarne esplicitamente il valore su `{false}`. Si noti nuovamente che scrivere `hidden="false"` produce un valore truthy, quindi l'elemento `<h1>` _verrà_ nascosto. Non dimenticare di rimuovere questo codice al termine.

> [!NOTE]
> L'attributo `aria-pressed` usato nel frammento di codice precedente ha un valore di `"true"` perché `aria-pressed` non è un vero attributo booleano come lo è `checked`.

### Implementazione degli stili

Incollare il seguente codice CSS in `src/index.css`:

```css
/* Resets */
*,
*::before,
*::after {
  box-sizing: border-box;
}
*:focus-visible {
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
  -moz-osx-font-smoothing: inherit;
  -webkit-font-smoothing: inherit;
  appearance: none;
  background: transparent;
  border: none;
  color: inherit;
  font: inherit;
  line-height: normal;
  margin: 0;
  overflow: visible;
  padding: 0;
  width: auto;
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
  background-color: whitesmoke;
  color: #4d4d4d;
  font:
    1.6rem/1.25 "Arial",
    sans-serif;
  margin: 0 auto;
  max-width: 68rem;
  width: 100%;
}
@media screen and (width >= 620px) {
  body {
    font-size: 1.9rem;
    line-height: 1.31579;
  }
}
/* End resets */
/* Global styles */
.form-group > input[type="text"] {
  display: inline-block;
  margin-top: 0.4rem;
}
.btn {
  border: 0.2rem solid #4d4d4d;
  cursor: pointer;
  padding: 0.8rem 1rem 0.7rem;
  text-transform: capitalize;
}
.btn.toggle-btn {
  border-color: lightgray;
  border-width: 1px;
}
.btn.toggle-btn[aria-pressed="true"] {
  border-color: #4d4d4d;
  text-decoration: underline;
}
.btn__danger {
  background-color: #ca3c3c;
  border-color: #bd2130;
  color: white;
}
.btn__filter {
  border-color: lightgrey;
}
.btn__primary {
  background-color: black;
  color: white;
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
  flex: 0 0 100%;
  margin: 0;
  text-align: center;
}
.visually-hidden {
  clip: rect(1px, 1px, 1px, 1px);
  height: 1px;
  overflow: hidden;
  position: absolute !important;
  white-space: nowrap;
  width: 1px;
}
[class*="stack"] > * {
  margin-bottom: 0;
  margin-top: 0;
}
.stack-small > * + * {
  margin-top: 1.25rem;
}
.stack-large > * + * {
  margin-top: 2.5rem;
}
@media screen and (width >= 550px) {
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
/* End global styles */
/* General app styles */
.todoapp {
  background: white;
  box-shadow:
    0 2px 4px 0 rgb(0 0 0 / 20%),
    0 2.5rem 5rem 0 rgb(0 0 0 / 10%);
  margin: 2rem 0 4rem 0;
  padding: 1rem;
  position: relative;
}
@media screen and (width >= 550px) {
  .todoapp {
    padding: 4rem;
  }
}
.todoapp > * {
  margin-left: auto;
  margin-right: auto;
  max-width: 50rem;
}
.todoapp > form {
  max-width: 100%;
}
.todoapp > h1 {
  display: block;
  margin: 0;
  margin-bottom: 1rem;
  max-width: 100%;
  text-align: center;
}
.label__lg {
  line-height: 1.01567;
  font-weight: 300;
  margin-bottom: 1rem;
  padding: 0.8rem;
  text-align: center;
}
.input__lg {
  border: 2px solid black;
  padding: 2rem;
}
.input__lg:focus-visible {
  border-color: #4d4d4d;
  box-shadow: inset 0 0 0 2px;
}
[class*="__lg"] {
  display: inline-block;
  font-size: 1.9rem;
  width: 100%;
}
[class*="__lg"]:not(:last-child) {
  margin-bottom: 1rem;
}
@media screen and (width >= 620px) {
  [class*="__lg"] {
    font-size: 2.4rem;
  }
}
/* End general app styles */
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
  border: 2px solid #565656;
  min-height: 4.4rem;
  padding: 0.4rem 0.8rem;
  width: 100%;
}
.todo-text:focus-visible {
  box-shadow: inset 0 0 0 2px;
}
/* End todo item styles */
/* Checkbox styles */
.c-cb {
  -webkit-font-smoothing: antialiased;
  box-sizing: border-box;
  clear: left;
  display: block;
  font-family: "Arial", sans-serif;
  font-size: 1.6rem;
  font-weight: normal;
  line-height: 1.25;
  min-height: 44px;
  padding-left: 40px;
  position: relative;
}
.c-cb > label::before,
.c-cb > input[type="checkbox"] {
  box-sizing: border-box;
  height: 44px;
  left: -2px;
  top: -2px;
  width: 44px;
}
.c-cb > input[type="checkbox"] {
  -webkit-font-smoothing: antialiased;
  cursor: pointer;
  margin: 0;
  opacity: 0;
  position: absolute;
  z-index: 1;
}
.c-cb > label {
  cursor: pointer;
  display: inline-block;
  font-family: inherit;
  font-size: inherit;
  line-height: inherit;
  margin-bottom: 0;
  padding: 8px 15px 5px;
  touch-action: manipulation;
}
.c-cb > label::before {
  background: transparent;
  border: 2px solid currentColor;
  content: "";
  position: absolute;
}
.c-cb > input[type="checkbox"]:focus-visible + label::before {
  border-width: 4px;
  outline: 3px dashed #228bec;
}
.c-cb > label::after {
  background: transparent;
  border: solid;
  border-width: 0 0 5px 5px;
  border-top-color: transparent;
  box-sizing: content-box;
  content: "";
  height: 7px;
  left: 9px;
  opacity: 0;
  position: absolute;
  top: 11px;
  transform: rotate(-45deg);
  width: 18px;
}
.c-cb > input[type="checkbox"]:checked + label::after {
  opacity: 1;
}
/* End checkbox styles */
```

Salvare e tornare al browser: l'app dovrebbe ora avere uno stile ragionevole.

## Riepilogo

Ora l'app della lista di attività assomiglia un po' di più a una vera app. Il problema è che non fa ancora nulla. Si inizierà a risolvere questo problema nel prossimo capitolo.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/React_getting_started","Learn_web_development/Core/Frameworks_libraries/React_components", "Learn_web_development/Core/Frameworks_libraries")}}
