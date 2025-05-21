---
title: Routing in Ember
slug: Learn_web_development/Core/Frameworks_libraries/Ember_routing
l10n:
  sourceCommit: 611edf6335e4a833a6f394d0d98b117e7b0a36bf
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Ember_conditional_footer","Learn_web_development/Core/Frameworks_libraries/Ember_resources", "Learn_web_development/Core/Frameworks_libraries")}}

In questo articolo impariamo a conoscere il **routing**, o filtraggio basato sugli URL come viene talvolta indicato. Lo useremo per fornire un URL unico per ciascuna delle tre visualizzazioni todo — "Tutti", "Attivi" e "Completati".

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          Al minimo, è raccomandato avere familiarità con i linguaggi
          <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>
          di base, e avere conoscenza del 
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminale/linea di comando</a
          >.
        </p>
        <p>
          Una comprensione più approfondita delle funzionalità moderne di JavaScript (come le classi,
          i moduli, ecc.), sarà estremamente utile, dato che Ember ne fa largo uso.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Imparare a implementare il routing in Ember.</td>
    </tr>
  </tbody>
</table>

## Filtraggio basato sugli URL

Ember include un sistema di routing che ha un'integrazione stretta con l'URL del browser. Tipicamente, quando si scrivono applicazioni web, si vuole che la pagina sia rappresentata dall'URL in modo tale che se (per qualsiasi motivo), la pagina ha bisogno di essere ricaricata, l'utente non resti sorpreso dallo stato dell'app web — possono collegarsi direttamente a viste significative dell'app.

Al momento, abbiamo già la pagina "Tutti", dato che al momento non stiamo effettuando alcun filtraggio nella pagina su cui abbiamo lavorato, ma dovremo riorganizzarla un po' per gestire una vista diversa per i todo "Attivi" e "Completati".

Un'applicazione Ember ha una route "application" predefinita, che è collegata al template `app/templates/application.hbs`. Poiché quel template dell'applicazione è il punto d'ingresso della nostra app todo, dovremo apportare alcune modifiche per permettere il routing.

## Creazione delle route

Iniziamo creando tre nuove route: "Index", "Active" e "Completed". Per fare questo bisognerà inserire i seguenti comandi nel terminale, all'interno della directory radice della tua app:

```bash
ember generate route index
ember generate route completed
ember generate route active
```

Il secondo e il terzo comando non solo dovrebbero aver generato nuovi file, ma anche aggiornato un file esistente, `app/router.js`. Contiene i seguenti contenuti:

```js
import EmberRouter from "@ember/routing/router";
import config from "./config/environment";

export default class Router extends EmberRouter {
  location = config.locationType;
  rootURL = config.rootURL;
}

Router.map(function () {
  this.route("completed");
  this.route("active");
});
```

Le righe evidenziate sono state aggiunte quando sono stati eseguiti il secondo e il terzo comando sopra menzionati.

`router.js` si comporta come una "mappa del sito" per permettere agli sviluppatori di vedere rapidamente come è strutturata l'intera app. Inoltre, dice a Ember come interagire con la tua route, come quando caricare dati arbitrari, gestire errori durante il caricamento di quei dati, o interpretare segmenti dinamici dell'URL. Dato che i nostri dati sono statici, non utilizzeremo nessuna di quelle funzionalità avanzate, ma assicureremo comunque che la route fornisca i dati minimamente richiesti per visualizzare una pagina.

Creare la route "Index" non ha aggiunto una riga di definizione della route a `router.js`, perché, come nel caso della navigazione URL e il caricamento dei moduli JavaScript, "Index" è una parola speciale che indica la route predefinita da rendere, caricare, ecc.

Per adattare il nostro vecchio modo di visualizzare l'app TodoList, dovremo prima sostituire l'invocazione del componente TodoList dal template applicazione con una chiamata `\{{outlet}}`, che significa "qualsiasi sotto-route sarà renderizzata qui".

Vai al file `todomvc/app/templates/application.hbs` e sostituisci

```hbs
<TodoList />
```

Con

```hbs
\{{outlet}}
```

Successivamente, nei nostri template `index.hbs`, `completed.hbs`, e `active.hbs` (anche essi nella directory dei template) possiamo per ora semplicemente inserire l'invocazione del componente TodoList.

In ciascun caso, sostituisci

```hbs
\{{outlet}}
```

con

```hbs
<TodoList />
```

Quindi a questo punto, se provi di nuovo l'app e visiti una qualsiasi delle tre route

`localhost:4200 localhost:4200/active localhost:4200/completed`

vedrai esattamente la stessa cosa. A ciascun URL, il template che corrisponde al percorso specifico ("Active", "Completed", o "Index"), renderizzerà il componente `<TodoList />`. La posizione nella pagina dove `<TodoList />` viene renderizzato è determinata dal `\{{ outlet }}` all'interno della route principale, che in questo caso è `application.hbs`. Quindi abbiamo le nostre route in atto. Ottimo!

Ma ora abbiamo bisogno di un modo per distinguere tra ciascuna di queste route, in modo che mostrino ciò che dovrebbero mostrare.

Innanzitutto, torniamo ancora una volta al nostro file `todo-data.js`. Contiene già un getter che restituisce tutti i todo, e un getter che restituisce i todo incompleti. Il getter che manca è uno per restituire solo i todo completati. Aggiungi il seguente sotto i getter esistenti:

```js
export default class TodoDataService extends Service {
  // …
  get completed() {
    return this.todos.filter((todo) => todo.isCompleted);
  }
  // …
}
```

## Modelli

Ora dobbiamo aggiungere modelli ai nostri file JavaScript delle route per consentirci di restituire facilmente set di dati specifici da visualizzare in quei modelli. `model` è un hook del ciclo di vita di caricamento dei dati. Per TodoMVC, le capacità di model non sono così importanti per noi; puoi trovare ulteriori informazioni nella [guida al model di Ember](https://guides.emberjs.com/release/routing/specifying-a-routes-model/) se vuoi approfondire. Forniamo anche l'accesso al servizio, come abbiamo fatto per i componenti.

### Il modello della route index

Prima di tutto, aggiorna `todomvc/app/routes/index.js` in modo che appaia come segue:

```js
import Route from "@ember/routing/route";
import { inject as service } from "@ember/service";

export default class IndexRoute extends Route {
  @service("todo-data") todos;

  model() {
    let todos = this.todos;

    return {
      get allTodos() {
        return todos.all;
      },
    };
  }
}
```

Ora possiamo aggiornare il file `todomvc/app/templates/index.hbs` in modo che quando include il componente `<TodoList />`, lo faccia esplicitamente con il modello disponibile, chiamando il suo getter `allTodos()` per assicurarsi che tutti i todo siano mostrati.

In questo file, cambia

```hbs
<TodoList />
```

In

```hbs-nolint
<TodoList @todos=\{{ @model.allTodos }} />
```

### Il modello della route completed

Ora aggiorna `todomvc/app/routes/completed.js` in modo che appaia come segue:

```js
import Route from "@ember/routing/route";
import { inject as service } from "@ember/service";

export default class CompletedRoute extends Route {
  @service("todo-data") todos;

  model() {
    let todos = this.todos;

    return {
      get completedTodos() {
        return todos.completed;
      },
    };
  }
}
```

Ora possiamo aggiornare il file `todomvc/app/templates/completed.hbs` in modo che quando include il componente `<TodoList />`, lo faccia esplicitamente con il modello disponibile, chiamando il suo getter `completedTodos()` per assicurarsi che solo i todo completati siano mostrati.

In questo file, cambia

```hbs
<TodoList />
```

In

```hbs-nolint
<TodoList @todos=\{{ @model.completedTodos }} />
```

### Il modello della route active

Infine per le route, sistemiamo la nostra vecchia route active. Inizia aggiornando `todomvc/app/routes/active.js` in modo che appaia come segue:

```js
import Route from "@ember/routing/route";
import { inject as service } from "@ember/service";

export default class ActiveRoute extends Route {
  @service("todo-data") todos;

  model() {
    let todos = this.todos;

    return {
      get activeTodos() {
        return todos.incomplete;
      },
    };
  }
}
```

Ora possiamo aggiornare il file `todomvc/app/templates/active.hbs` in modo che quando include il componente `<TodoList />`, lo faccia esplicitamente con il modello disponibile, chiamando il suo getter `activeTodos()` per assicurarsi che solo i todo attivi (incompleti) siano mostrati.

In questo file, cambia

```hbs
<TodoList />
```

In

```hbs-nolint
<TodoList @todos=\{{ @model.activeTodos }} />
```

Nota che, in ciascuno degli hook del modello delle route, stiamo restituendo un oggetto con un getter invece di un oggetto statico, o semplicemente la lista statica dei todo (ad esempio, `this.todos.completed`). Il motivo di questo è che vogliamo che il template abbia un riferimento dinamico alla lista dei todo, e se restituissimo la lista direttamente, i dati non sarebbero mai rirecalcolati, il che risulterebbe in navigazioni che sembrano fallire / non riescono effettivamente a filtrare. Definendo un getter nell'oggetto di ritorno dai dati del modello, i todo sono riesaminati in modo che le nostre modifiche alla lista dei todo siano rappresentate nella lista renderizzata.

## Attivazione dei link nel footer

Quindi la nostra funzionalità di route è ora tutta funzionante, ma non possiamo accedervi dalla nostra app. Attiviamo i link nel footer in modo che cliccandoci sopra si vada alle route desiderate.

Torna a `todomvc/app/components/footer.hbs` e trova il seguente markup:

```hbs
<a href="#">All</a>
<a href="#">Active</a>
<a href="#">Completed</a>
```

Aggiorna a

```hbs
<LinkTo @route="index">All</LinkTo>
<LinkTo @route="active">Active</LinkTo>
<LinkTo @route="completed">Completed</LinkTo>
```

`<LinkTo>` è un componente Ember integrato che gestisce tutti i cambi di stato quando si naviga tra le route, così come l'impostazione di una classe "attiva" su qualsiasi link che corrisponda all'URL, nel caso in cui ci sia il desiderio di stilizzarlo diversamente dai link inattivi.

## Aggiornamento della visualizzazione dei todo all'interno di TodoList

Una piccola cosa finale che dobbiamo correggere è che precedentemente, all'interno di `todomvc/app/components/todo-list.hbs`, stavamo accedendo direttamente al servizio todo-data e iterando su tutti i todo, come mostrato qui:

```hbs
\{{#each this.todos.all as |todo| }}
```

Dato che ora vogliamo che il nostro componente TodoList mostri una lista filtrata, vorremo passare un argomento al componente TodoList che rappresenti "la lista corrente dei todo", come mostrato qui:

```hbs
\{{#each @todos as |todo| }}
```

E questo è tutto per questo tutorial! La tua app dovrebbe ora avere collegamenti completamente funzionanti nel footer che visualizzano le route "Index"/predefinita, "Active" e "Completed".

![L'app todo list, che mostra il routing funzionante per tutti, attivi e completati.](todos-navigation.gif)

## Riepilogo

Congratulazioni! Hai completato questo tutorial!

C'è molto altro da implementare prima che quanto abbiamo trattato qui sia in parità con l'app originale [TodoMVC app](https://todomvc.com/), come la modifica, la cancellazione e la memorizzazione persistente dei todo tra i ricaricamenti di pagina.

Per vedere la nostra implementazione Ember finita, controlla la cartella dell'app completata nel repository per [il codice di questo tutorial](https://github.com/NullVoxPopuli/ember-todomvc-tutorial/tree/master/steps/00-finished-todomvc/todomvc) o vedi la [versione distribuita live](https://nullvoxpopuli.github.io/ember-todomvc-tutorial/) qui. Studia il codice per saperne di più su Ember, e controlla anche l'articolo successivo, che fornisce collegamenti a più risorse e alcuni consigli per la risoluzione dei problemi.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Ember_conditional_footer","Learn_web_development/Core/Frameworks_libraries/Ember_resources", "Learn_web_development/Core/Frameworks_libraries")}}
