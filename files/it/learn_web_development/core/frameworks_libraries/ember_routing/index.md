---
title: Routing in Ember
slug: Learn_web_development/Core/Frameworks_libraries/Ember_routing
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Ember_conditional_footer","Learn_web_development/Core/Frameworks_libraries/Ember_resources", "Learn_web_development/Core/Frameworks_libraries")}}

In questo articolo impariamo il **routing**, o filtraggio basato su URL come a volte viene chiamato. Lo useremo per fornire un URL unico per ciascuna delle tre viste todo — "Tutti", "Attivi" e "Completati".

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          È consigliato avere familiarità con i linguaggi principali
          <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e
          avere conoscenza del
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminale/command line</a
          >.
        </p>
        <p>
          Una comprensione più approfondita delle caratteristiche moderne di JavaScript (come classi,
          moduli, ecc.) sarà estremamente utile, poiché Ember ne fa largo uso.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Imparare a implementare il routing in Ember.</td>
    </tr>
  </tbody>
</table>

## Filtraggio basato su URL

Ember viene fornito con un sistema di routing strettamente integrato con l'URL del browser. Tipicamente, quando si scrivono applicazioni web, si desidera che la pagina sia rappresentata dall'URL in modo che se (per qualsiasi ragione), la pagina ha bisogno di essere aggiornata, l'utente non sia sorpreso dallo stato dell'app web — possono collegarsi direttamente alle viste significative dell'app.

Al momento, abbiamo già la pagina "Tutti", dato che attualmente non stiamo facendo alcun filtraggio nella pagina su cui stavamo lavorando, ma dovremo riorganizzarla un po' per gestire una vista diversa per i todo "Attivi" e "Completati".

Un'applicazione Ember ha una route predefinita "application", che è collegata al template `app/templates/application.hbs`. Poiché quel template di application è il punto di ingresso della nostra app todo, dovremo apportare alcune modifiche per consentire il routing.

## Creare le route

Iniziamo creando tre nuove route: "Index", "Active" e "Completed". Per farlo, sarà necessario inserire i seguenti comandi nel terminale, all'interno della directory principale della tua app:

```bash
ember generate route index
ember generate route completed
ember generate route active
```

Il secondo e il terzo comando dovrebbero aver generato non solo nuovi file, ma anche aggiornato un file esistente, `app/router.js`. Contiene i seguenti contenuti:

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

Le righe evidenziate sono state aggiunte quando i comandi 2 e 3 sopra sono stati eseguiti.

`router.js` si comporta come una "sitemap" per i sviluppatori per consentire di vedere rapidamente come l'intera app è strutturata. Dice anche a Ember come interagire con la tua route, come durante il caricamento di dati arbitrari, la gestione degli errori durante il caricamento di quei dati o l'interpretazione di segmenti dinamici dell'URL. Poiché i nostri dati sono statici, non utilizzeremo nessuna di quelle funzionalità avanzate, ma ci assicureremo comunque che la route fornisca i dati minimi richiesti per visualizzare una pagina.

Creare la route "Index" non ha aggiunto una linea di definizione della route a `router.js`, perché, come con la navigazione URL e il caricamento dei moduli JavaScript, "Index" è una parola speciale che indica la route predefinita da renderizzare, caricare, ecc.

Per adattare il nostro vecchio modo di renderizzare l'app TodoList, dovremo prima sostituire l'invocazione del componente TodoList dal template dell'applicazione con una chiamata `\{{outlet}}`, che significa "qualsiasi sotto-route verrà renderizzata al posto qui".

Vai al file `todomvc/app/templates/application.hbs` e sostituisci

```hbs
<TodoList />
```

Con

```hbs
\{{outlet}}
```

Successivamente, nei nostri template `index.hbs`, `completed.hbs`, e `active.hbs` (anch'essi situati nella directory dei template) possiamo per ora semplicemente inserire l'invocazione del componente TodoList.

In ciascun caso, sostituisci

```hbs
\{{outlet}}
```

con

```hbs
<TodoList />
```

A questo punto, se provi l'applicazione di nuovo e visiti una delle tre route

`localhost:4200 localhost:4200/active localhost:4200/completed`

vedrai esattamente la stessa cosa. Ad ogni URL, il template che corrisponde al percorso specifico ("Active", "Completed" o "Index"), renderizzerà il componente `<TodoList />`. La posizione nella pagina in cui `<TodoList />` è renderizzato è determinata da `\{{ outlet }}` all'interno della route principale, che in questo caso è `application.hbs`. Quindi abbiamo le nostre route al loro posto. Ottimo!

Ma ora abbiamo bisogno di un modo per differenziare ciascuna di queste route, in modo che mostrino ciò che dovrebbero mostrare.

Prima di tutto, ritorna una volta di più al nostro file `todo-data.js`. Contiene già un getter che restituisce tutti i todo e un getter che restituisce i todo incompleti. Il getter che ci manca è uno per restituire solo i todo completati. Aggiungi il seguente sotto i getter esistenti:

```js
get completed() {
  return this.todos.filter((todo) => todo.isCompleted);
}
```

## Modelli

Ora abbiamo bisogno di aggiungere modelli ai nostri file JavaScript delle route per consentirci di restituire facilmente insiemi di dati specifici da visualizzare in quei modelli. `model` è un hook del ciclo di vita del caricamento dei dati. Per TodoMVC, le capacità di model non sono così importanti per noi; puoi trovare maggiori informazioni nella [guida sui modelli di Ember](https://guides.emberjs.com/release/routing/specifying-a-routes-model/) se vuoi approfondire. Forniamo anche accesso al servizio, come abbiamo fatto per i componenti.

### Il modello della route index

Prima di tutto, aggiorna `todomvc/app/routes/index.js` così che appaia come segue:

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

Ora possiamo aggiornare il file `todomvc/app/templates/index.hbs` in modo che quando include il componente `<TodoList />`, lo faccia esplicitamente con il modello disponibile, chiamando il suo getter `allTodos()` per assicurarsi che tutti i todo vengano mostrati.

In questo file, cambia

```hbs
<TodoList />
```

In

```hbs-nolint
<TodoList @todos=\{{ @model.allTodos }} />
```

### Il modello della route completed

Ora aggiorna `todomvc/app/routes/completed.js` così che appaia come segue:

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

Ora possiamo aggiornare il file `todomvc/app/templates/completed.hbs` in modo che quando include il componente `<TodoList />`, lo faccia esplicitamente con il modello disponibile, chiamando il suo getter `completedTodos()` per assicurarsi che vengano mostrati solo i todo completati.

In questo file, cambia

```hbs
<TodoList />
```

In

```hbs-nolint
<TodoList @todos=\{{ @model.completedTodos }} />
```

### Il modello della route active

Infine per le route, occupiamoci della nostra route active. Inizia aggiornando `todomvc/app/routes/active.js` in modo che appaia come segue:

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

Ora possiamo aggiornare il file `todomvc/app/templates/active.hbs` in modo che quando include il componente `<TodoList />`, lo faccia esplicitamente con il modello disponibile, chiamando il suo getter `activeTodos()` per assicurarsi che vengano mostrati solo i todo attivi (incompleti).

In questo file, cambia

```hbs
<TodoList />
```

In

```hbs-nolint
<TodoList @todos=\{{ @model.activeTodos }} />
```

Si noti che, in ciascuno degli hook dei modelli delle route, stiamo restituendo un oggetto con un getter invece di un oggetto statico, o più semplicemente dell'elenco statico dei todo (per esempio, `this.todos.completed`). Il motivo è che vogliamo che il template abbia un riferimento dinamico all'elenco dei todo, e se restituissimo l'elenco direttamente, i dati non verrebbero mai ri-calcolati, il che comporterebbe la visualizzazioni che sembrano fallire / non effettivamente filtrare. Avendo un getter definito nell'oggetto restituito dai dati del modello, i todo vengono ri-cercati in modo che i nostri cambiamenti all'elenco dei todo siano rappresentati nell'elenco renderizzato.

## Rendere funzionanti i link nel footer

Quindi la nostra funzionalità di routing è ora tutta in atto, ma non possiamo accedervi dalla nostra app. 

Torniamo a `todomvc/app/components/footer.hbs`, e trova la seguente parte di markup:

```hbs
<a href="#">All</a>
<a href="#">Active</a>
<a href="#">Completed</a>
```

Aggiornala in

```hbs
<LinkTo @route="index">All</LinkTo>
<LinkTo @route="active">Active</LinkTo>
<LinkTo @route="completed">Completed</LinkTo>
```

`<LinkTo>` è un componente Ember integrato che gestisce tutti i cambiamenti di stato durante la navigazione tra le route, oltre ad impostare una classe "active" su qualsiasi link che corrisponde all'URL, nel caso ci sia il desiderio di stilizzarlo diversamente dai link inattivi.

## Aggiornare la visualizzazione dei todo all'interno di TodoList

Una piccola cosa finale che dobbiamo correggere è che in precedenza, all'interno di `todomvc/app/components/todo-list.hbs`, stavamo accedendo direttamente al servizio di dati todo e iterando su tutti i todo, come mostrato qui:

```hbs
\{{#each this.todos.all as |todo| }}
```

Dal momento che ora vogliamo avere il nostro componente TodoList che mostra un elenco filtrato, vogliamo passare un argomento al componente TodoList che rappresenta "l'elenco corrente dei todo", come mostrato qui:

```hbs
\{{#each @todos as |todo| }}
```

E questo è tutto per questo tutorial! La tua app dovrebbe ora avere link completamente funzionanti nel footer che visualizzano le route "Indice"/predefinito, "Attivo" e "Completato".

![L'app lista todo, mostra il routing funzionante per tutti, attivi e completati.](todos-navigation.gif)

## Sommario

Congratulazioni! Hai terminato questo tutorial!

C'è molto altro da implementare prima che quanto coperto qui abbia la parità con l'originale [app TodoMVC](https://todomvc.com/), come modificare, eliminare e mantenere i todo anche dopo il ricaricamento della pagina.

Per vedere la nostra implementazione finale in Ember, controlla la cartella del progetto finito nel repository per [il codice di questo tutorial](https://github.com/NullVoxPopuli/ember-todomvc-tutorial/tree/master/steps/00-finished-todomvc/todomvc) o vedi la [versione live](https://nullvoxpopuli.github.io/ember-todomvc-tutorial/) qui. Studia il codice per saperne di più su Ember e dai anche un'occhiata al prossimo articolo che fornisce collegamenti a più risorse e alcuni consigli di risoluzione dei problemi.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Ember_conditional_footer","Learn_web_development/Core/Frameworks_libraries/Ember_resources", "Learn_web_development/Core/Frameworks_libraries")}}
