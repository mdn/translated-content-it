---
title: Struttura dell'app Ember e componentizzazione
slug: Learn_web_development/Core/Frameworks_libraries/Ember_structure_componentization
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Ember_getting_started","Learn_web_development/Core/Frameworks_libraries/Ember_interactivity_events_state", "Learn_web_development/Core/Frameworks_libraries")}}

In questo articolo ci occuperemo di pianificare la struttura della nostra app Ember TodoMVC, aggiungendo l'HTML e poi suddividendo tale struttura HTML in componenti.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          Si raccomanda almeno un minimo di familiarità con le lingue principali,
          <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e
          avere conoscenze di base del
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminale/linea di comando</a
          >.
        </p>
        <p>
          Una comprensione approfondita delle funzionalità moderne di JavaScript (come classi,
          moduli, ecc.) sarà estremamente utile, poiché Ember fa largo uso di queste.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare a strutturare un'app Ember e poi suddividere tale struttura
        in componenti.
      </td>
    </tr>
  </tbody>
</table>

## Pianificazione del layout dell'app TodoMVC

Nell'ultimo articolo abbiamo impostato un nuovo progetto Ember, poi aggiunto e configurato i nostri stili CSS. A questo punto, aggiungiamo un po' di HTML, pianificando la struttura e la semantica della nostra app TodoMVC.

L'HTML della pagina di atterraggio della nostra applicazione è definito in `app/templates/application.hbs`. Questo file esiste già, e il suo contenuto attualmente appare in questo modo:

```hbs
\{{!-- The following component displays Ember's default welcome message. --}}
<WelcomePage />
\{{!-- Feel free to remove this! --}}

\{{outlet}}
```

`<WelcomePage />` è un componente fornito da un addon di Ember che rende la pagina di benvenuto predefinita che abbiamo visto nell'articolo precedente, quando abbiamo navigato per la prima volta al nostro server su `localhost:4200`.

Tuttavia, non vogliamo questo. Invece, vogliamo che contenga la struttura dell'app TodoMVC. Per iniziare, cancelli i contenuti di `application.hbs` e li sostituisca con quanto segue:

```html
<section class="todoapp">
  <h1>todos</h1>
  <input
    class="new-todo"
    aria-label="What needs to be done?"
    placeholder="What needs to be done?"
    autofocus />
</section>
```

> **Note:** [`aria-label`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-label) fornisce un'etichetta per la tecnologia assistiva di cui fare uso — ad esempio, per un lettore di schermo per leggere a voce alta. Questo è utile in quei casi in cui abbiamo un [`<input>`](/it/docs/Web/HTML/Reference/Elements/input) usato senza testo HTML corrispondente che potrebbe essere trasformato in un'etichetta.

Quando salva `application.hbs`, il server di sviluppo che ha avviato in precedenza ricostruirà automaticamente l'app e aggiornerà il browser. L'output reso dovrebbe ora apparire così:

![app todo visualizzata nel browser mostrando solo il campo di input per il nuovo todo](todos-initial-render.png)

Non ci vuole molto sforzo per far sembrare il nostro HTML un'app lista di compiti completa. Aggiorni di nuovo il file `application.hbs` in modo che il suo contenuto appaia così:

```html
<section class="todoapp">
  <h1>todos</h1>
  <input
    class="new-todo"
    aria-label="What needs to be done?"
    placeholder="What needs to be done?"
    autofocus />

  <section class="main">
    <input id="mark-all-complete" class="toggle-all" type="checkbox" />
    <label for="mark-all-complete">Mark all as complete</label>

    <ul class="todo-list">
      <li>
        <div class="view">
          <input
            aria-label="Toggle the completion of this todo"
            class="toggle"
            type="checkbox" />
          <label>Buy Movie Tickets</label>
          <button
            type="button"
            class="destroy"
            title="Remove this todo"></button>
        </div>

        <input autofocus class="edit" value="Todo Text" />
      </li>

      <li>
        <div class="view">
          <input
            aria-label="Toggle the completion of this todo"
            class="toggle"
            type="checkbox" />
          <label>Go to Movie</label>
          <button
            type="button"
            class="destroy"
            title="Remove this todo"></button>
        </div>

        <input autofocus class="edit" value="Todo Text" />
      </li>
    </ul>
  </section>

  <footer class="footer">
    <span class="todo-count"> <strong>0</strong> todos left </span>

    <ul class="filters">
      <li>
        <a href="#">All</a>
        <a href="#">Active</a>
        <a href="#">Completed</a>
      </li>
    </ul>

    <button type="button" class="clear-completed">Clear Completed</button>
  </footer>
</section>
```

L'output reso dovrebbe ora essere il seguente:

![app todo visualizzata nel browser con il campo di input per il nuovo todo e i todo esistenti che mostrano, - comprare biglietti del cinema e andare al cinema](todos-with-todo-items.png)

Questo sembra piuttosto completo, ma ricordi che questo è solo un prototipo statico. Ora dobbiamo dividere il nostro codice HTML in componenti dinamici; successivamente trasformeremo il tutto in un'app completamente interattiva.

Guardando il codice accanto all'app todo visualizzata, ci sono vari modi in cui potremmo decidere di dividere l'interfaccia utente, ma pianifichiamo di dividere l'HTML nei seguenti componenti:

![screenshot del codice annotato per mostrare quali parti del codice andranno in quale componente](todos-ui-component-breakdown.png)

I raggruppamenti dei componenti sono i seguenti:

- L'input principale / "new-todo" (rosso nell'immagine)
- Il corpo contenente della lista di todo + il pulsante `mark-all-complete` (viola nell'immagine)

  - Il pulsante `mark-all-complete`, esplicitamente evidenziato per motivi riportati di seguito (giallo nell'immagine)
  - Ogni todo è un componente individuale (verde nell'immagine)

- Il footer (blu nell'immagine)

Qualcosa di strano da notare è che la casella di controllo `mark-all-complete` (segnata in giallo), pur essendo nella sezione "main", è visualizzata accanto all'input "new-todo". Questo perché il CSS predefinito posiziona assolutamente la casella di controllo + l'etichetta con valori top e left negativi per spostarla accanto all'input, piuttosto che all'interno della sezione "main".

![app todo vista attraverso i DevTools](todos-devtools-view.png)

## Utilizzare la CLI per creare i componenti per noi

Quindi, per rappresentare la nostra app, vogliamo creare 4 componenti:

- Header
- Lista
- Todo individuale
- Footer

Per creare un componente, utilizziamo il comando `ember generate component`, seguito dal nome del componente. Creiamo prima il componente header. Ecco come fare:

1. Fermi il server in esecuzione andando al terminale e premendo <kbd>Ctrl</kbd> + <kbd>C</kbd>.
2. Inserisca il seguente comando nel terminale:

   ```bash
   ember generate component header
   ```

   Questo genererà alcuni nuovi file, come mostrato nell'output risultante nel terminale:

   ```plain
   installing component
     create app/components/header.hbs
     skip app/components/header.js
     tip to add a class, run `ember generate component-class header`
   installing component-test
     create tests/integration/components/header-test.js
   ```

`header.hbs` è il file template dove includeremo la struttura HTML solo per quel componente. Successivamente aggiungeremo la funzionalità dinamica richiesta, come il binding dei dati, la risposta all'interazione dell'utente, ecc.

> [!NOTE]
> Il file `header.js` (mostrato come ignorato) è per la connessione a una classe di componenti Glimmer di supporto, di cui non abbiamo bisogno per ora, in quanto sono per l'aggiunta di interattività e manipolazione dello stato. Per impostazione predefinita, `generate component` genera componenti solo template, perché nelle grandi applicazioni, i componenti solo template finiscono per essere la maggioranza dei componenti.

`header-test.js` è per scrivere test automatizzati per garantire che la nostra app continui a funzionare nel tempo man mano che aggiorniamo, aggiungiamo funzionalità, rifattorizziamo, ecc. Il testing è al di fuori dello scopo di questo tutorial, anche se generalmente il testing dovrebbe essere implementato mentre si sviluppa, piuttosto che dopo, altrimenti tende a essere dimenticato. Se è curiosa riguardo ai test, o perché potrebbe voler avere test automatizzati, controlli il [tutorial ufficiale di Ember sui test](https://guides.emberjs.com/release/tutorial/part-1/automated-testing/).

Prima di iniziare ad aggiungere qualsiasi codice dei componenti, creiamo il supporto per gli altri componenti. Inserisca le seguenti righe nel terminale, una alla volta:

```bash
ember generate component todo-list
ember generate component todo
ember generate component footer
```

Ora vedrà il seguente contenuto all'interno della directory `todomvc/app/components`:

![la directory dei componenti dell'app, che mostra i file template dei componenti che abbiamo creato](todos-components-directory.png)

Ora che abbiamo tutti i nostri file di struttura dei componenti, possiamo tagliare e incollare l'HTML per ciascun componente fuori dal file `application.hbs` e in ciascuno di quei componenti, e poi riscrivere il `application.hbs` per riflettere le nostre nuove astrazioni.

1. Il file `header.hbs` dovrebbe essere aggiornato per contenere quanto segue:

   ```html
   <input
     class="new-todo"
     aria-label="What needs to be done?"
     placeholder="What needs to be done?"
     autofocus />
   ```

2. `todo-list.hbs` dovrebbe essere aggiornato per contenere questo blocco di codice:

   ```html
   <section class="main">
     <input id="mark-all-complete" class="toggle-all" type="checkbox" />
     <label for="mark-all-complete">Mark all as complete</label>

     <ul class="todo-list">
       <Todo />
       <Todo />
     </ul>
   </section>
   ```

   > [!NOTE]
   > L'unica parte non HTML in questo nuovo `todo-list.hbs` è l'invocazione del componente `<Todo />`. In Ember, un'invocazione di componente è simile a dichiarare un elemento HTML, ma la prima lettera inizia con una maiuscola, e i nomi sono scritti in {{Glossary("camel_case", "camel case maiuscolo")}}, come vedrà con `<TodoList />` più avanti. Il contenuto del file `todo.hbs` sotto sostituirà `<Todo />` nella pagina renderizzata mentre la nostra applicazione si carica.

3. Aggiungere quanto segue nel file `todo.hbs`:

   ```html
   <li>
     <div class="view">
       <input
         aria-label="Toggle the completion of this todo"
         class="toggle"
         type="checkbox" />
       <label>Buy Movie Tickets</label>
       <button type="button" class="destroy" title="Remove this todo"></button>
     </div>

     <input autofocus class="edit" value="Todo Text" />
   </li>
   ```

4. `footer.hbs` dovrebbe essere aggiornato per contenere quanto segue:

   ```html
   <footer class="footer">
     <span class="todo-count"> <strong>0</strong> todos left </span>

     <ul class="filters">
       <li>
         <a href="#">All</a>
         <a href="#">Active</a>
         <a href="#">Completed</a>
       </li>
     </ul>

     <button type="button" class="clear-completed">Clear Completed</button>
   </footer>
   ```

5. Infine, il contenuto di `application.hbs` dovrebbe essere aggiornato in modo che richiami i componenti appropriati, così:

   ```hbs
   <section class="todoapp">
     <h1>todos</h1>

     <Header />
     <TodoList />
     <Footer />
   </section>
   ```

6. Con queste modifiche effettuate, esegua nuovamente `npm start` nel terminale, poi si rechi su `http://localhost:4200` per assicurarsi che l'app todo appaia ancora come prima del refactoring.

![app todo visualizzata nel browser con il campo di input per il nuovo todo e i todo esistenti che mostrano, entrambi dicono comprare biglietti del cinema](todos-components-render.png)

Noti come gli elementi todo dicano entrambi "Buy Movie Tickets" — questo perché lo stesso componente viene richiamato due volte, e il testo del todo è codificato al suo interno. Vedremo come mostrare elementi todo differenti nel prossimo articolo!

## Riepilogo

Ottimo! Tutto appare come dovrebbe. Abbiamo rifatto con successo il nostro HTML in componenti! Nel prossimo articolo inizieremo a introdurre l'interattività nella nostra applicazione Ember.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Ember_getting_started","Learn_web_development/Core/Frameworks_libraries/Ember_interactivity_events_state", "Learn_web_development/Core/Frameworks_libraries")}}
