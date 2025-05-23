---
title: Struttura dell'app Ember e componentizzazione
slug: Learn_web_development/Core/Frameworks_libraries/Ember_structure_componentization
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Ember_getting_started","Learn_web_development/Core/Frameworks_libraries/Ember_interactivity_events_state", "Learn_web_development/Core/Frameworks_libraries")}}

In questo articolo pianificheremo la struttura della nostra app TodoMVC Ember, aggiungendo l'HTML e poi suddividendo quella struttura HTML in componenti.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          È consigliato avere familiarità con i linguaggi core
          <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e
          avere conoscenza del
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminal/command line</a
          >.
        </p>
        <p>
          Una comprensione più approfondita delle caratteristiche moderne di JavaScript (come classi, moduli, ecc.) sarà estremamente utile, poiché Ember ne fa ampio uso.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare come strutturare un'app Ember, e poi suddividere quella struttura in componenti.
      </td>
    </tr>
  </tbody>
</table>

## Pianificare il layout dell'app TodoMVC

Nell'ultimo articolo abbiamo impostato un nuovo progetto Ember, poi aggiunto e configurato i nostri stili CSS. A questo punto aggiungiamo un po' di HTML, pianificando la struttura e la semantica della nostra app TodoMVC.

La pagina di atterraggio HTML della nostra applicazione è definita in `app/templates/application.hbs`. Questo esiste già, e il suo contenuto attualmente appare nel seguente modo:

```hbs
\{{!-- The following component displays Ember's default welcome message. --}}
<WelcomePage />
\{{!-- Feel free to remove this! --}}

\{{outlet}}
```

`<WelcomePage />` è un componente fornito da un addon Ember che rende la pagina di benvenuto predefinita che abbiamo visto nell'articolo precedente, quando abbiamo navigato per la prima volta al nostro server su `localhost:4200`.

Tuttavia, non vogliamo questo. Invece, vogliamo che contenga la struttura dell'app TodoMVC. Per iniziare, elimina il contenuto di `application.hbs` e sostituiscilo con il seguente:

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

> **Nota:** [`aria-label`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-label) fornisce un'etichetta per la tecnologia assistiva — ad esempio, per far sì che un lettore di schermo la legga. Questo è utile in casi come quando abbiamo un [`<input>`](/it/docs/Web/HTML/Reference/Elements/input) utilizzato senza un testo HTML corrispondente che potrebbe essere trasformato in un'etichetta.

Quando salvi `application.hbs`, il server di sviluppo che hai avviato in precedenza ricompilerà automaticamente l'app e aggiornerà il browser. Il risultato renderizzato dovrebbe ora apparire così:

![app todo renderizzata nel browser con solo il campo di input per il nuovo todo visualizzato](todos-initial-render.png)

Non ci vuole molto sforzo per far sembrare il nostro HTML come un'app di lista di cose da fare completamente funzionale. Aggiorna di nuovo il file `application.hbs` in modo che il suo contenuto appaia così:

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

Il risultato renderizzato dovrebbe ora essere il seguente:

![app todo renderizzata nel browser con il campo di input per il nuovo todo e i todo esistenti visualizzati, - acquistare biglietti del cinema e andare al cinema](todos-with-todo-items.png)

Questo sembra abbastanza completo, ma ricordate che questa è solo una prototipo statico. Ora dobbiamo suddividere il nostro codice HTML in componenti dinamici; più avanti lo trasformeremo in un'app completamente interattiva.

Guardando il codice accanto all'app todo renderizzata, ci sono diversi modi in cui potremmo decidere come suddividere l'interfaccia utente, ma pianifichiamo di dividere l'HTML nei seguenti componenti:

![screenshot del codice annotato per mostrare quali parti del codice andranno in quale componente](todos-ui-component-breakdown.png)

I raggruppamenti dei componenti sono i seguenti:

- L'input principale / "new-todo" (rosso nell'immagine)
- Il corpo contenitore della lista di cose da fare + il pulsante `mark-all-complete` (viola nell'immagine)

  - Il pulsante `mark-all-complete`, esplicitamente evidenziato per le ragioni date di seguito (giallo nell'immagine)
  - Ogni todo è un componente individuale (verde nell'immagine)

- Il footer (blu nell'immagine)

Una cosa strana da notare è che la casella di controllo `mark-all-complete` (segnata in giallo), mentre è nella sezione "main", è resa accanto all'input "new-todo". Questo perché il CSS predefinito posiziona assolutamente la casella di controllo + etichetta con valori negativi di top e left per spostarla accanto all'input, piuttosto che al suo interno nella sezione "main".

![app todo vista attraverso devtools](todos-devtools-view.png)

## Usare la CLI per creare i nostri componenti

Quindi per rappresentare la nostra app, vogliamo creare 4 componenti:

- Header
- Lista
- Todo individuale
- Footer

Per creare un componente, usiamo il comando `ember generate component`, seguito dal nome del componente. Creiamo prima il componente header. Per farlo:

1. Ferma il server eseguendo il comando nel terminale e premendo <kbd>Ctrl</kbd> + <kbd>C</kbd>.
2. Inserisci il seguente comando nel tuo terminale:

   ```bash
   ember generate component header
   ```

   Questo genererà alcuni nuovi file, come mostrato nell'output del terminale risultante:

   ```plain
   installing component
     create app/components/header.hbs
     skip app/components/header.js
     tip to add a class, run `ember generate component-class header`
   installing component-test
     create tests/integration/components/header-test.js
   ```

`header.hbs` è il file template dove includeremo la struttura HTML solo per quel componente. Più avanti aggiungeremo la funzionalità dinamica richiesta come i data binding, la risposta all'interazione dell'utente, ecc.

> [!NOTE]
> Il file `header.js` (mostrato come skipped) è per la connessione a una Glimmer Component Class di supporto, di cui non abbiamo bisogno per ora, poiché sono per aggiungere interattività e manipolazione dello stato. Di default, `generate component` genera componenti solo template, perché in grandi applicazioni, i componenti solo template finiscono per essere la maggioranza dei componenti.

`header-test.js` è per scrivere collaudi automatici per garantire che la nostra app continui a funzionare nel tempo man mano che aggiorniamo, aggiungiamo funzionalità, refattoriamo, ecc. Il collaudo è fuori dal campo di questo tutorial, anche se generalmente dovrebbe essere implementato mentre si sviluppa, piuttosto che dopo, altrimenti tenderebbe ad essere dimenticato. Se sei curioso riguardo al collaudo, o perché si vorrebbero avere collaudo automatici, dai un'occhiata al [tutorial ufficiale di Ember sul collaudo](https://guides.emberjs.com/release/tutorial/part-1/automated-testing/).

Prima di iniziare ad aggiungere codice componente, creiamo l'impalcatura per gli altri componenti. Inserisci nel tuo terminale le seguenti righe, una alla volta:

```bash
ember generate component todo-list
ember generate component todo
ember generate component footer
```

Ora vedrai quanto segue all'interno della tua directory `todomvc/app/components`:

![la directory componenti dell'app, mostrando i file di template di componenti che abbiamo creato](todos-components-directory.png)

Ora che abbiamo tutti i nostri file di struttura dei componenti, possiamo tagliare e incollare l'HTML per ciascun componente dal file `application.hbs` in ognuno di quei componenti, e poi riscrivere `application.hbs` per riflettere le nostre nuove astrazioni.

1. Il file `header.hbs` dovrebbe essere aggiornato per contenere quanto segue:

   ```html
   <input
     class="new-todo"
     aria-label="What needs to be done?"
     placeholder="What needs to be done?"
     autofocus />
   ```

2. `todo-list.hbs` dovrebbe essere aggiornato per contenere questo pezzo di codice:

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
   > L'unica parte non HTML in questo nuovo `todo-list.hbs` è l'invocazione del componente `<Todo />`. In Ember, un'invocazione di componente è simile alla dichiarazione di un elemento HTML, ma la prima lettera inizia con una lettera maiuscola, e i nomi sono scritti in {{Glossary("camel_case", "upper camel case")}}, come vedrai con `<TodoList />` più avanti. Il contenuto del file `todo.hbs` sottostante sostituirà `<Todo />` nella pagina renderizzata al caricamento della nostra applicazione.

3. Aggiungi il seguente codice nel file `todo.hbs`:

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

5. Infine, il contenuto di `application.hbs` dovrebbe essere aggiornato in modo che chiami i componenti appropriati, come segue:

   ```hbs
   <section class="todoapp">
     <h1>todos</h1>

     <Header />
     <TodoList />
     <Footer />
   </section>
   ```

6. Con queste modifiche fatte, esegui `npm start` nel tuo terminale di nuovo, poi vai su `http://localhost:4200` per garantire che l'app todo sembri ancora come prima del refactoring.

![app todo renderizzata nel browser con il campo di input per il nuovo todo e i todo esistenti visualizzati, entrambi dicendo acquistare biglietti del cinema](todos-components-render.png)

Nota come gli elementi todo dicano entrambi "Acquista biglietti del cinema" — questo perché lo stesso componente viene invocato due volte, e il testo del todo è codificato al suo interno. Guarderemo come mostrare elementi todo diversi nel prossimo articolo!

## Riassunto

Ottimo! Tutto appare come dovrebbe. Abbiamo refattorizzato con successo il nostro HTML in componenti! Nel prossimo articolo inizieremo a guardare come aggiungere interattività alla nostra applicazione Ember.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Ember_getting_started","Learn_web_development/Core/Frameworks_libraries/Ember_interactivity_events_state", "Learn_web_development/Core/Frameworks_libraries")}}
