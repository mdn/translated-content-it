---
title: Styling Vue components with CSS
slug: Learn_web_development/Core/Frameworks_libraries/Vue_styling
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Vue_methods_events_models","Learn_web_development/Core/Frameworks_libraries/Vue_computed_properties", "Learn_web_development/Core/Frameworks_libraries")}}

È finalmente arrivato il momento di rendere la nostra app un po' più carina. In questo articolo esploreremo i diversi modi di stilizzare i componenti Vue con CSS.

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
            >terminal/command line</a
          >.
        </p>
        <p>
          I componenti Vue sono scritti come una combinazione di oggetti JavaScript che
          gestiscono i dati dell'app e di una sintassi di template basata su HTML che si mappa
          alla struttura del DOM sottostante. Per l'installazione, e per utilizzare alcune delle
          funzionalità più avanzate di Vue (come i Componenti a File Singolo o le funzioni di render),
          è necessaria una terminale con node + npm installati.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Imparare come stilizzare i componenti Vue.</td>
    </tr>
  </tbody>
</table>

## Stilizzare componenti Vue con CSS

Prima di passare ad aggiungere funzionalità più avanzate alla nostra app, dovremmo aggiungere un po' di CSS di base per migliorarne l'aspetto. Vue offre tre approcci comuni per stilizzare le app:

- File CSS esterni.
- Stili globali nei Componenti a File Singolo (`.vue` files).
- Stili a livello di componente nei Componenti a File Singolo.

Per aiutarti a familiarizzare con ciascuno di essi, utilizzeremo una combinazione di tutti e tre per dare alla nostra app un aspetto e una sensazione più gradevoli.

## Stilizzare con file CSS esterni

È possibile includere file CSS esterni e applicarli globalmente alla tua app. Vediamo come si fa.

Per iniziare, crea un file chiamato `reset.css` nella directory `src/assets`. I file in questa cartella vengono elaborati da webpack. Questo significa che possiamo usare pre-processori CSS (come SCSS) o post-processori (come PostCSS).

Sebbene questo tutorial non utilizzi tali strumenti, è utile sapere che includendo tale codice nella cartella assets, esso verrà elaborato automaticamente.

Aggiungi i seguenti contenuti al file `reset.css`:

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

Successivamente, nel tuo file `src/main.js`, importa il file `reset.css` in questo modo:

```js
import "./assets/reset.css";
```

Questo farà sì che il file venga rilevato durante il passaggio di build e automaticamente aggiunto al nostro sito.

Gli stili di reset dovrebbero ora essere applicati all'app. Le immagini qui sotto mostrano l'aspetto dell'app prima e dopo l'applicazione del reset.

Prima:

![the todo app with partial styling added; the app is now in a card, but some of the internal features still need styling](todo-app-unstyled.png)

Dopo:

![the todo app with partial styling added; the app is now in a card, but some of the internal features still need styling](todo-app-reset-styles.png)

I cambiamenti evidenti includono la rimozione dei puntini dell'elenco, i cambiamenti nel colore di sfondo e modifiche agli stili base di pulsanti e input.

## Aggiunta di stili globali a Componenti a File Singolo

Ora che abbiamo resettato il nostro CSS per essere uniforme tra i browser, dobbiamo personalizzare un po' di più gli stili. Ci sono alcuni stili che vogliamo applicare tra i componenti della nostra app. Sebbene aggiungere direttamente questi file al foglio di stile `reset.css` possa funzionare, li aggiungeremo invece ai tag `<style>` in `App.vue` per dimostrare come possono essere utilizzati.

Ci sono già alcuni stili presenti nel file. Rimuoviamoli e sostituiamoli con gli stili sottostanti. Questi stili fanno alcune cose — aggiungono alcuni stili ai pulsanti e agli input, e personalizzano l'elemento `#app` e i suoi figli.

Aggiorna l'elemento `<style>` del tuo file `App.vue` affinché assomigli a questo:

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

Se controlli l'app, vedrai che la nostra lista di cose da fare è ora in una card, e abbiamo una formattazione migliore degli oggetti da fare. Ora possiamo passare attraverso e iniziare a modificare i nostri componenti per utilizzare alcuni di questi stili.

![the todo app with partial styling added; the app is now in a card, but some of the internal features still need styling](todo-app-partial-styles.png)

### Aggiunta di classi CSS in Vue

Dovremmo applicare le classi CSS del pulsante al `<button>` nel nostro componente `ToDoForm`. Poiché i template Vue sono HTML valido, questo viene fatto allo stesso modo di come faresti in HTML normale — aggiungendo un attributo `class=""` all'elemento.

Aggiungi `class="btn btn__primary btn__lg"` all'elemento `<button>` del tuo form:

```html
<button type="submit" class="btn btn__primary btn__lg">Add</button>
```

Ora che siamo qui, c'è un altro cambiamento semantico e di stile che possiamo fare. Poiché il nostro form denota una sezione specifica della nostra pagina, potrebbe beneficiare di un elemento `<h2>`. L'etichetta, tuttavia, già denota lo scopo del form. Per evitare di ripeterci, avvolgiamo la nostra etichetta in un `<h2>`. Ci sono anche alcuni altri stili CSS globali che possiamo aggiungere. Aggiungeremo anche la classe `input__lg` al nostro elemento `<input>`.

Aggiorna il template del tuo `ToDoForm` in modo che appaia così:

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

Aggiungiamo anche la classe `stack-large` al tag `<ul>` nel nostro file `App.vue`. Questo aiuterà a migliorare un po' lo spazio dei nostri oggetti da fare.

Aggiornalo come segue:

```html
<ul aria-labelledby="list-summary" class="stack-large">
  …
</ul>
```

## Aggiunta di stili a livello di componente

L'ultimo componente che vogliamo stilizzare è il nostro componente `ToDoItem`. Per mantenere le definizioni di stile vicine al componente, possiamo aggiungere un elemento `<style>` al suo interno. Tuttavia, se questi stili alterano elementi al di fuori di questo componente, potrebbe essere difficile rintracciare gli stili responsabili e risolvere il problema. Qui è utile l'attributo `scoped` — questo assegna un selettore di attributo HTML `data` unico a tutti i tuoi stili, impedendo loro di collidere a livello globale.

Per usare il modificatore `scoped`, crea un elemento `<style>` all'interno di `ToDoItem.vue`, nella parte inferiore del file, e attribuiscigli un attributo `scoped`:

```html
<style scoped>
  /* … */
</style>
```

Successivamente, copia il seguente CSS nel nuovo elemento `<style>` creato:

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

Ora dobbiamo aggiungere alcune classi CSS al nostro template per connettere gli stili.

Alla radice `<div>`, aggiungi una classe `custom-checkbox`. All'`<input>`, aggiungi una classe `checkbox`. Infine, all'`<label>` aggiungi una classe `checkbox-label`. Il template aggiornato è di seguito:

```html
<template>
  <div class="custom-checkbox">
    <input type="checkbox" :id="id" :checked="isDone" class="checkbox" />
    <label :for="id" class="checkbox-label">\{{label}}</label>
  </div>
</template>
```

L'app dovrebbe ora avere checkbox personalizzati. La tua app dovrebbe apparire simile allo screenshot qui sotto.

![the todo app with complete styling. The input form is now styled properly, and the todo items now have spacing and custom checkboxes](todo-app-complete-styles.png)

## Riassunto

Il nostro lavoro sullo stile della nostra app di esempio è finito. Nel prossimo articolo torneremo ad aggiungere qualche funzionalità in più alla nostra app, in particolare utilizzando una proprietà computata per aggiungere un conteggio degli elementi completati della lista di cose da fare.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Vue_methods_events_models","Learn_web_development/Core/Frameworks_libraries/Vue_computed_properties", "Learn_web_development/Core/Frameworks_libraries")}}
