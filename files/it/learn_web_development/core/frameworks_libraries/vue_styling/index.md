---
title: Styling Vue components with CSS
slug: Learn_web_development/Core/Frameworks_libraries/Vue_styling
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Vue_methods_events_models","Learn_web_development/Core/Frameworks_libraries/Vue_computed_properties", "Learn_web_development/Core/Frameworks_libraries")}}

È finalmente giunto il momento di migliorare l'aspetto della nostra app. In questo articolo esploreremo i diversi modi di stilizzare i componenti Vue con CSS.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          Familiarità con i linguaggi principali <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>,
          conoscenza del
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminale/linea di comando</a
          >.
        </p>
        <p>
          I componenti Vue sono scritti come una combinazione di oggetti JavaScript che
          gestiscono i dati dell'app e una sintassi di template basata su HTML che mappa
          la struttura DOM sottostante. Per l'installazione e per usare alcune delle
          funzionalità più avanzate di Vue (come i componenti a singolo file o le funzioni di render),
          avrà bisogno di un terminale con node + npm installati.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Imparare a stilizzare i componenti Vue.</td>
    </tr>
  </tbody>
</table>

## Stilizzare i componenti Vue con CSS

Prima di aggiungere funzionalità più avanzate alla nostra app, dovremmo aggiungere del CSS di base per migliorare l'aspetto. Vue ha tre approcci comuni per stilizzare le app:

- File CSS esterni.
- Stili globali nei componenti a singolo file (`.vue` files).
- Stili a livello di componente nei componenti a singolo file.

Per aiutarla a familiarizzare con ognuno, useremo una combinazione di tutti e tre per dare alla nostra app un aspetto più piacevole.

## Stilizzare con file CSS esterni

Può includere file CSS esterni e applicarli globalmente alla sua app. Vediamo come si fa.

Per iniziare, crei un file chiamato `reset.css` nella directory `src/assets`. I file in questa cartella vengono elaborati da webpack. Questo significa che possiamo usare pre-processor CSS (come SCSS) o post-processor (come PostCSS).

Sebbene questo tutorial non userà tali strumenti, è bene sapere che quando si include tale codice nella cartella degli asset, sarà elaborato automaticamente.

Aggiunga i seguenti contenuti al file `reset.css`:

```css
/*reset.css*/
/* RESETS */
*,
*::before,
*::after {
  box-sizing: border-box;
}
*:focus {
  outline: 3px dashed #228bec;
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
  /* 1 */
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
    1.6rem/1.25 "Helvetica Neue",
    Helvetica,
    Arial,
    sans-serif;
  background-color: #f5f5f5;
  color: #4d4d4d;
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
}
@media screen and (min-width: 620px) {
  body {
    font-size: 1.9rem;
    line-height: 1.31579;
  }
}
/*END RESETS*/
```

Successivamente, nel suo file `src/main.js`, importi il file `reset.css` in questo modo:

```js
import "./assets/reset.css";
```

Questo farà sì che il file venga rilevato durante la fase di build e automaticamente aggiunto al nostro sito.

Gli stili di reset dovrebbero ora essere applicati all'app. Le immagini qui sotto mostrano l'aspetto dell'app prima e dopo l'applicazione del reset.

Prima:

![the todo app with partial styling added; the app is now in a card, but some of the internal features still need styling](todo-app-unstyled.png)

Dopo:

![the todo app with partial styling added; the app is now in a card, but some of the internal features still need styling](todo-app-reset-styles.png)

Cambiamenti evidenti includono la rimozione dei punti elenco, i cambiamenti del colore di sfondo e le modifiche agli stili base dei pulsanti e degli input.

## Aggiungere stili globali ai componenti a singolo file

Ora che abbiamo resettato il nostro CSS per essere uniforme sui vari browser, dobbiamo personalizzare ulteriormente gli stili. Ci sono alcuni stili che vogliamo applicare a tutti i componenti della nostra app. Sebbene l'aggiunta di questi file direttamente al foglio di stile `reset.css` funzionerebbe, li aggiungeremo invece ai tag `<style>` in `App.vue` per dimostrare come questo può essere utilizzato.

Ci sono già alcuni stili presenti nel file. Rimuoviamoli e sostituiamoli con gli stili qui sotto. Questi stili fanno diverse cose — aggiungono alcune stilizzazioni ai pulsanti e agli input, e personalizzano l'elemento `#app` e i suoi figli.

Aggiorni il tag `<style>` del suo file `App.vue` in modo che appaia così:

```html
<style>
  /* Global styles */
  .btn {
    padding: 0.8rem 1rem 0.7rem;
    border: 0.2rem solid #4d4d4d;
    cursor: pointer;
    text-transform: capitalize;
  }
  .btn__danger {
    color: #fff;
    background-color: #ca3c3c;
    border-color: #bd2130;
  }
  .btn__filter {
    border-color: lightgrey;
  }
  .btn__danger:focus {
    outline-color: #c82333;
  }
  .btn__primary {
    color: #fff;
    background-color: #000;
  }
  .btn-group {
    display: flex;
    justify-content: space-between;
  }
  .btn-group > * {
    flex: 1 1 auto;
  }
  .btn-group > * + * {
    margin-left: 0.8rem;
  }
  .label-wrapper {
    margin: 0;
    flex: 0 0 100%;
    text-align: center;
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
  .visually-hidden {
    position: absolute;
    height: 1px;
    width: 1px;
    overflow: hidden;
    clip: rect(1px 1px 1px 1px);
    clip: rect(1px, 1px, 1px, 1px);
    clip-path: rect(1px, 1px, 1px, 1px);
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
  /* End global styles */
  #app {
    background: #fff;
    margin: 2rem 0 4rem 0;
    padding: 1rem;
    padding-top: 0;
    position: relative;
    box-shadow:
      0 2px 4px 0 rgb(0 0 0 / 20%),
      0 2.5rem 5rem 0 rgb(0 0 0 / 10%);
  }
  @media screen and (min-width: 550px) {
    #app {
      padding: 4rem;
    }
  }
  #app > * {
    max-width: 50rem;
    margin-left: auto;
    margin-right: auto;
  }
  #app > form {
    max-width: 100%;
  }
  #app h1 {
    display: block;
    min-width: 100%;
    width: 100%;
    text-align: center;
    margin: 0;
    margin-bottom: 1rem;
  }
</style>
```

Se controlla l'app, vedrà che la nostra lista di attività è ora in una card e abbiamo una migliore formattazione degli elementi del nostro elenco di cose da fare. Adesso possiamo procedere ed iniziare a modificare i nostri componenti per utilizzare alcuni di questi stili.

![the todo app with partial styling added; the app is now in a card, but some of the internal features still need styling](todo-app-partial-styles.png)

### Aggiungere classi CSS in Vue

Dovremmo applicare le classi CSS del pulsante al pulsante `<button>` nel nostro componente `ToDoForm`. Poiché i template di Vue sono HTML valido, questo viene fatto nello stesso modo in cui potrebbe farlo in HTML normale — aggiungendo un attributo `class=""` all'elemento.

Aggiunga `class="btn btn__primary btn__lg"` al suo elemento `<button>` del form:

```html
<button type="submit" class="btn btn__primary btn__lg">Add</button>
```

Mentre siamo qui, c'è un altro cambiamento semantico e di stile che possiamo fare. Poiché il nostro modulo denota una sezione specifica della nostra pagina, potrebbe trarre beneficio da un elemento `<h2>`. L'etichetta, tuttavia, denota già lo scopo del modulo. Per evitare di ripeterci, incapsuliamo la nostra etichetta in un `<h2>`. Ci sono anche alcuni altri stili CSS globali che possiamo aggiungere. Aggiungeremo anche la classe `input__lg` al nostro elemento `<input>`.

Aggiorni il suo template `ToDoForm` affinché appaia così:

```html
<template>
  <form @submit.prevent="onSubmit">
    <h2 class="label-wrapper">
      <label for="new-todo-input" class="label__lg">
        What needs to be done?
      </label>
    </h2>
    <input
      type="text"
      id="new-todo-input"
      name="new-todo"
      autocomplete="off"
      v-model.lazy.trim="label"
      class="input__lg" />
    <button type="submit" class="btn btn__primary btn__lg">Add</button>
  </form>
</template>
```

Aggiungiamo anche la classe `stack-large` al tag `<ul>` nel nostro file `App.vue`. Questo contribuirà a migliorare leggermente la spaziatura degli elementi della nostra lista di cose da fare.

Aggiorni come segue:

```html
<ul aria-labelledby="list-summary" class="stack-large">
  …
</ul>
```

## Aggiungere stili a livello di componente

L'ultimo componente che vogliamo stilizzare è il nostro componente `ToDoItem`. Per mantenere le definizioni di stile vicine al componente, possiamo aggiungere un elemento `<style>` al suo interno. Tuttavia, se questi stili alterano elementi al di fuori di questo componente, potrebbe risultare difficile individuare gli stili responsabili e risolvere il problema. Qui è dove l'attributo `scoped` può essere utile — questo attacca un selettore di attributi `data` HTML unico a tutti i suoi stili, impedendo loro di collidere a livello globale.

Per utilizzare il modificatore `scoped`, crei un elemento `<style>` all'interno di `ToDoItem.vue`, in fondo al file, e gli assegni un attributo `scoped`:

```html
<style scoped>
  /* … */
</style>
```

Successivamente, copi il seguente CSS nell'elemento `<style>` appena creato:

```css
.custom-checkbox > .checkbox-label {
  font-family: Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-weight: 400;
  font-size: 16px;
  font-size: 1rem;
  line-height: 1.25;
  color: #0b0c0c;
  display: block;
  margin-bottom: 5px;
}
.custom-checkbox > .checkbox {
  font-family: Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-weight: 400;
  font-size: 16px;
  font-size: 1rem;
  line-height: 1.25;
  box-sizing: border-box;
  width: 100%;
  height: 40px;
  height: 2.5rem;
  margin-top: 0;
  padding: 5px;
  border: 2px solid #0b0c0c;
  border-radius: 0;
  appearance: none;
}
.custom-checkbox > input:focus {
  outline: 3px dashed #fd0;
  outline-offset: 0;
  box-shadow: inset 0 0 0 2px;
}
.custom-checkbox {
  font-family: Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  font-weight: 400;
  font-size: 1.6rem;
  line-height: 1.25;
  display: block;
  position: relative;
  min-height: 40px;
  margin-bottom: 10px;
  padding-left: 40px;
  clear: left;
}
.custom-checkbox > input[type="checkbox"] {
  -webkit-font-smoothing: antialiased;
  cursor: pointer;
  position: absolute;
  z-index: 1;
  top: -2px;
  left: -2px;
  width: 44px;
  height: 44px;
  margin: 0;
  opacity: 0;
}
.custom-checkbox > .checkbox-label {
  font-size: inherit;
  font-family: inherit;
  line-height: inherit;
  display: inline-block;
  margin-bottom: 0;
  padding: 8px 15px 5px;
  cursor: pointer;
  touch-action: manipulation;
}
.custom-checkbox > label::before {
  content: "";
  box-sizing: border-box;
  position: absolute;
  top: 0;
  left: 0;
  width: 40px;
  height: 40px;
  border: 2px solid currentcolor;
  background: transparent;
}
.custom-checkbox > input[type="checkbox"]:focus + label::before {
  border-width: 4px;
  outline: 3px dashed #228bec;
}
.custom-checkbox > label::after {
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
.custom-checkbox > input[type="checkbox"]:checked + label::after {
  opacity: 1;
}
@media only screen and (min-width: 40rem) {
  label,
  input,
  .custom-checkbox {
    font-size: 19px;
    font-size: 1.9rem;
    line-height: 1.31579;
  }
}
```

Ora dobbiamo aggiungere alcune classi CSS al nostro template per collegare gli stili.

All'elemento `<div>` radice, aggiunga la classe `custom-checkbox`. All'elemento `<input>`, aggiunga la classe `checkbox`. Infine, all'elemento `<label>` aggiunga la classe `checkbox-label`. Il template aggiornato è qui sotto:

```html
<template>
  <div class="custom-checkbox">
    <input type="checkbox" :id="id" :checked="isDone" class="checkbox" />
    <label :for="id" class="checkbox-label">\{{label}}</label>
  </div>
</template>
```

L'app dovrebbe ora avere checkbox personalizzati. La sua app dovrebbe apparire come nello screenshot qui sotto.

![the todo app with complete styling. The input form is now styled properly, and the todo items now have spacing and custom checkboxes](todo-app-complete-styles.png)

## Sommario

Abbiamo completato il lavoro di stilizzazione della nostra app di esempio. Nell'articolo successivo torneremo ad aggiungere ulteriore funzionalità alla nostra app, in particolare utilizzando una proprietà computata per aggiungere un conteggio degli elementi completati alla nostra lista di cose da fare.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Vue_methods_events_models","Learn_web_development/Core/Frameworks_libraries/Vue_computed_properties", "Learn_web_development/Core/Frameworks_libraries")}}
