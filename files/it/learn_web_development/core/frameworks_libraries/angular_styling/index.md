---
title: Stilizzare la nostra app Angular
slug: Learn_web_development/Core/Frameworks_libraries/Angular_styling
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Angular_todo_list_beginning","Learn_web_development/Core/Frameworks_libraries/Angular_item_component", "Learn_web_development/Core/Frameworks_libraries")}}

Ora che abbiamo impostato la struttura di base della nostra applicazione e iniziato a visualizzare qualcosa di utile, passiamo ad un articolo che esamina come Angular gestisce la stilizzazione delle applicazioni.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi di base <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>,
        conoscenza del
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
          >terminale/linea di comando</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Imparare come stilizzare un'app Angular.</td>
    </tr>
  </tbody>
</table>

## Aggiungere un po' di stile ad Angular

L'Angular CLI genera due tipi di file di stile:

- Stili del componente: Angular CLI attribuisce a ciascun componente il proprio file di stili. Gli stili in questo file si applicano solo al suo componente.
- `styles.css`: Nella directory `src`, gli stili in questo file si applicano a tutta l'applicazione a meno che non specifichi stili a livello di componente.

A seconda che si utilizzi un preprocessore CSS, l'estensione dei file CSS può variare.
Angular supporta CSS semplice, SCSS, Sass e Less.

In `src/styles.css`, incolla i seguenti stili:

```css
body {
  font-family: Helvetica, Arial, sans-serif;
}

.btn-wrapper {
  /* flexbox */
  display: flex;
  flex-wrap: nowrap;
  justify-content: space-between;
}

.btn {
  color: #000;
  background-color: #fff;
  border: 2px solid #cecece;
  padding: 0.35rem 1rem 0.25rem 1rem;
  font-size: 1rem;
}

.btn:hover {
  background-color: #ecf2fd;
}

.btn:active {
  background-color: #d1e0fe;
}

.btn:focus {
  outline: none;
  border: black solid 2px;
}

.btn-primary {
  color: #fff;
  background-color: #000;
  width: 100%;
  padding: 0.75rem;
  font-size: 1.3rem;
  border: black solid 2px;
  margin: 1rem 0;
}

.btn-primary:hover {
  background-color: #444242;
}

.btn-primary:focus {
  color: #000;
  outline: none;
  border: #000 solid 2px;
  background-color: #d7ecff;
}

.btn-primary:active {
  background-color: #212020;
}
```

Il CSS in `src/styles.css` si applica all'intera applicazione, tuttavia, questi stili non influenzano tutto sulla pagina.
Il passo successivo è aggiungere stili che si applicano specificamente al `AppComponent`.

In `app.component.css`, aggiungi i seguenti stili:

```css
.main {
  max-width: 500px;
  width: 85%;
  margin: 2rem auto;
  padding: 1rem;
  text-align: center;
  box-shadow:
    0 2px 4px 0 rgb(0 0 0 / 20%),
    0 2.5rem 5rem 0 rgb(0 0 0 / 10%);
}

@media screen and (min-width: 600px) {
  .main {
    width: 70%;
  }
}

label {
  font-size: 1.5rem;
  font-weight: bold;
  display: block;
  padding-bottom: 1rem;
}

.lg-text-input {
  width: 100%;
  padding: 1rem;
  border: 2px solid #000;
  display: block;
  box-sizing: border-box;
  font-size: 1rem;
}

.btn-wrapper {
  margin-bottom: 2rem;
}

.btn-menu {
  flex-basis: 32%;
}

.active {
  color: green;
}

ul {
  padding-inline-start: 0;
}

ul li {
  list-style: none;
}
```

L'ultimo passo è rivisitare il suo browser e osservare come lo stile è stato aggiornato. Le cose ora hanno un po' più senso.

## Riepilogo

Ora che il nostro breve tour sulla stilizzazione in Angular è terminato, torniamo a creare la funzionalità della nostra app. Nel prossimo articolo, creeremo un componente adeguato per gli elementi to-do, in modo che lei possa controllare, modificare e cancellare gli elementi to-do.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Angular_todo_list_beginning","Learn_web_development/Core/Frameworks_libraries/Angular_item_component", "Learn_web_development/Core/Frameworks_libraries")}}
