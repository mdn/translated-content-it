---
title: Iniziare la nostra app ToDo in React
short-title: App ToDo in React
slug: Learn_web_development/Core/Frameworks_libraries/React_todo_list_beginning
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/React_getting_started","Learn_web_development/Core/Frameworks_libraries/React_components", "Learn_web_development/Core/Frameworks_libraries")}}

Supponiamo di essere stati incaricati di creare un proof-of-concept in React: un'app che permette agli utenti di aggiungere, modificare ed eliminare attività su cui vogliono lavorare, e anche segnarle come completate senza eliminarle. Questo articolo la guiderà attraverso la struttura di base e lo stile di tale applicazione, pronta per la definizione dei singoli componenti e interattività, che aggiungeremo in seguito.

> [!NOTE]
> Se ha bisogno di verificare il suo codice rispetto alla nostra versione, può trovare una versione completa del codice dell'app di esempio React nel nostro [repository todo-react](https://github.com/mdn/todo-react). Per una versione live funzionante, veda <https://mdn.github.io/todo-react/>.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi base <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e con il <a href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line">terminale/linea di comando</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        Familiarità con il nostro case study della lista di cose da fare e impostare la struttura e lo stile base
        del <code>App</code>.
      </td>
    </tr>
  </tbody>
</table>

## Le storie utente della nostra app

Nello sviluppo software, una storia utente è un obiettivo attuabile dal punto di vista dell'utente. Definire le storie utente prima di iniziare il lavoro ci aiuterà a concentrarci. La nostra app dovrebbe soddisfare le seguenti storie:

Come utente, posso

- leggere un elenco di attività.
- aggiungere un'attività usando il mouse o la tastiera.
- segnare qualsiasi attività come completata, usando il mouse o la tastiera.
- eliminare qualsiasi attività, usando il mouse o la tastiera.
- modificare qualsiasi attività, usando il mouse o la tastiera.
- visualizzare un sottoinsieme specifico di attività: tutte le attività, solo quella attiva, o solo quelle completate.

Affronteremo queste storie una per una.

## Compiti preliminari al progetto

Vite ci ha fornito del codice che non useremo affatto per il nostro progetto. I seguenti comandi del terminale lo elimineranno per far spazio al nostro nuovo progetto. Assicurarsi di partire dalla directory radice dell'app!

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
> Se ha fermato il suo server per eseguire le attività del terminale sopra menzionate, dovrà riavviarlo usando `npm run dev`.

## Codice d'inizio del progetto

Come punto di partenza per questo progetto, forniremo due cose: una funzione `App()` per sostituire quella che ha appena eliminato, e del CSS per stilizzare la sua app.

### Il JSX

Copiare il seguente snippet negli appunti, quindi incollarlo in `App.jsx`:

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

Ora apra `index.html` e cambi il testo dell'elemento [`<title>`](/it/docs/Web/HTML/Reference/Elements/title) in `TodoMatic`. In questo modo, corrisponderà all'elemento [`<h1>`](/it/docs/Web/HTML/Reference/Elements/Heading_Elements) in cima alla nostra app.

```html
<title>TodoMatic</title>
```

Quando il browser si aggiorna, dovrebbe vedere qualcosa di simile a questo:

![app todo-matic, non stilizzata, che mostra un disordine di etichette, input e pulsanti](unstyled-app.png)

È brutto e non funziona ancora, ma va bene così — lo stilizzeremo tra un momento. Prima, consideri il JSX che abbiamo e come corrisponde alle nostre storie utente:

- Abbiamo un elemento [`<form>`](/it/docs/Web/HTML/Reference/Elements/form), con un [`<input type="text">`](/it/docs/Web/HTML/Reference/Elements/input/text) per scrivere una nuova attività e un pulsante per inviare il modulo.
- Abbiamo una serie di pulsanti che verranno usati per filtrare le nostre attività.
- Abbiamo un'intestazione che ci dice quante attività restano.
- Abbiamo le nostre 3 attività, disposte in un elenco non ordinato. Ogni attività è un elemento di lista ([`<li>`](/it/docs/Web/HTML/Reference/Elements/li)), e ha pulsanti per modificarla ed eliminarla, e una casella di controllo per spuntarla come completata.

Il modulo ci permetterà di _creare_ attività; i pulsanti ci permetteranno di _filtrarle_; l'intestazione e l'elenco sono il nostro modo di _leggerle_. L'interfaccia utente per _modificare_ un'attività è assente per ora. Va bene - la scriveremo più tardi.

### Caratteristiche di accessibilità

Potrebbe notare alcuni markup insoliti qui. Ad esempio:

```jsx
<button type="button" className="btn toggle-btn" aria-pressed="true">
  <span className="visually-hidden">Show </span>
  <span>all</span>
  <span className="visually-hidden"> tasks</span>
</button>
```

Qui, `aria-pressed` dice alla tecnologia assistiva (come i lettori di schermo) che il pulsante può essere in uno di due stati: `pressed` o `unpressed`. Pensateli come analoghi a `on` e `off`. Impostare un valore di `"true"` significa che il pulsante è premuto per impostazione predefinita.

La classe `visually-hidden` non ha effetto ancora, perché non abbiamo incluso alcun CSS. Una volta che avremo messo i nostri stili in atto, però, qualsiasi elemento con questa classe sarà nascosto agli utenti vedenti e ancora disponibile agli utenti di tecnologie assistive — questo perché queste parole non sono necessarie agli utenti vedenti; sono lì per fornire più informazioni su ciò che fa il pulsante per gli utenti di tecnologie assistive che non hanno il contesto visivo extra per aiutarli.

Più in basso, può trovare il nostro elemento [`<ul>`](/it/docs/Web/HTML/Reference/Elements/ul):

```html
<ul
  role="list"
  className="todo-list stack-large stack-exception"
  aria-labelledby="list-heading">
  …
</ul>
```

L'attributo `role` aiuta la tecnologia assistiva a spiegare che tipo di elemento rappresenta un tag. Un `<ul>` è trattato come una lista di default, ma gli stili che stiamo per aggiungere romperanno questa funzionalità. Questo ruolo ripristinerà il significato di "lista" all'elemento `<ul>`. Se vuole saperne di più su perché questo è necessario, può controllare l'articolo di [Scott O'Hara, "Fixing Lists"](https://www.scottohara.me/blog/2019/01/12/lists-and-safari.html).

L'attributo `aria-labelledby` dice alle tecnologie assistive che stiamo trattando la nostra intestazione della lista come l'etichetta che descrive lo scopo della lista sottostante. Fare questa associazione dà alla lista un contesto più informativo, che potrebbe aiutare gli utenti di tecnologie assistive a comprendere meglio lo scopo della lista.

Infine, le etichette e gli input nei nostri elementi lista hanno alcuni attributi unici per il JSX:

```jsx
<input id="todo-0" type="checkbox" defaultChecked />
<label className="todo-label" htmlFor="todo-0">
  Eat
</label>
```

L'attributo `defaultChecked` nel tag `<input />` dice a React di selezionare inizialmente questa casella di controllo. Se dovessimo usare `checked`, come faremo in HTML normale, React registrerebbe alcuni avvertimenti nella nostra console del browser relativi alla gestione degli eventi sulla casella di controllo, che vogliamo evitare. Non si preoccupi troppo di questo per ora — lo vedremo più avanti, quando useremo gli eventi.

L'attributo `htmlFor` corrisponde all'attributo `for` usato in HTML. Non possiamo usare `for` come attributo nel JSX perché `for` è una parola riservata, quindi React usa `htmlFor` invece.

### Una nota sugli attributi booleani nel JSX

L'attributo `defaultChecked` nella sezione precedente è un attributo booleano – un attributo il cui valore è `true` o `false`. Come in HTML, un attributo booleano è `true` se è presente e `false` se è assente; l'assegnazione sul lato destro dell'espressione è facoltativa. Può impostare esplicitamente il suo valore racchiudendolo tra parentesi graffe – ad esempio, `defaultChecked={true}` o `defaultChecked={false}`.

Poiché JSX è JavaScript, c'è un tranello da conoscere con gli attributi booleani: scrivere `defaultChecked="false"` imposterà un valore _stringa_ di `"false"` piuttosto che un valore _booleano_. Le stringhe non vuote sono {{Glossary("Truthy", "truthy")}}, quindi React considererà `defaultChecked` come `true` e spunterà la casella per impostazione predefinita. Questo non è ciò che vogliamo, quindi dovremmo evitarlo.

Se desidera, può praticare la scrittura di attributi booleani con un altro attributo che potrebbe aver già visto, [`hidden`](/it/docs/Web/HTML/Reference/Global_attributes/hidden), che impedisce agli elementi di essere renderizzati sulla pagina. Provi ad aggiungere `hidden` all'elemento `<h1>` in `App.jsx` per vedere cosa succede, quindi provi a impostare esplicitamente il suo valore su `{false}`. Noti, ancora una volta, che scrivere `hidden="false"` risulta in un valore truthy quindi il `<h1>` _si_ nasconderà. Non dimentichi di rimuovere questo codice quando ha finito.

> [!NOTE]
> L'attributo `aria-pressed` usato nel nostro snippet di codice precedente ha un valore di `"true"` perché `aria-pressed` non è un vero attributo booleano come `checked`.

### Implementazione dei nostri stili

Incolli il seguente codice CSS in `src/index.css`:

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
  background-color: #f5f5f5;
  color: #4d4d4d;
  font:
    1.6rem/1.25 Arial,
    sans-serif;
  margin: 0 auto;
  max-width: 68rem;
  width: 100%;
}
@media screen and (min-width: 620px) {
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
  border-color: #d3d3d3;
  border-width: 1px;
}
.btn.toggle-btn[aria-pressed="true"] {
  border-color: #4d4d4d;
  text-decoration: underline;
}
.btn__danger {
  background-color: #ca3c3c;
  border-color: #bd2130;
  color: #fff;
}
.btn__filter {
  border-color: lightgrey;
}
.btn__primary {
  background-color: #000;
  color: #fff;
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
  clip: rect(1px 1px 1px 1px);
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
/* End global styles */
/* General app styles */
.todoapp {
  background: #fff;
  box-shadow:
    0 2px 4px 0 rgb(0 0 0 / 20%),
    0 2.5rem 5rem 0 rgb(0 0 0 / 10%);
  margin: 2rem 0 4rem 0;
  padding: 1rem;
  position: relative;
}
@media screen and (min-width: 550px) {
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
  border: 2px solid #000;
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
@media screen and (min-width: 620px) {
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
  font-family: Arial, sans-serif;
  font-size: 1.6rem;
  font-weight: 400;
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
  border: 2px solid currentcolor;
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

Salvi e torni a controllare il suo browser: la sua app dovrebbe ora avere uno styling ragionevole.

## Riepilogo

Ora la nostra app di lista di cose da fare sembra effettivamente un po' più una vera app! Il problema è: non fa effettivamente nulla. Inizieremo a risolvere questo nel prossimo capitolo!

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/React_getting_started","Learn_web_development/Core/Frameworks_libraries/React_components", "Learn_web_development/Core/Frameworks_libraries")}}
