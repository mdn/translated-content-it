---
title: Risorse e risoluzione dei problemi di Ember
slug: Learn_web_development/Core/Frameworks_libraries/Ember_resources
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenu("Learn_web_development/Core/Frameworks_libraries/Ember_routing", "Learn_web_development/Core/Frameworks_libraries")}}

Il nostro articolo finale su Ember ti offre un elenco di risorse che puoi utilizzare per approfondire il tuo apprendimento, oltre a informazioni utili per la risoluzione dei problemi e altre informazioni.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          È raccomandato almeno essere familiari con i linguaggi base
          <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e
          avere conoscenza del
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminale/command line</a
          >.
        </p>
        <p>
          Una comprensione più profonda delle funzionalità moderne di JavaScript (come classi, moduli, ecc.) sarà estremamente vantaggiosa, poiché Ember fa ampio uso di esse.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Fornire ulteriori link a risorse e informazioni per la risoluzione dei problemi.
      </td>
    </tr>
  </tbody>
</table>

## Ulteriori risorse

- [Guide di Ember.JS](https://guides.emberjs.com/release/)

  - [Tutorial: Super Rentals](https://guides.emberjs.com/release/tutorial/part-1/)

- [Documentazione API di Ember.JS](https://api.emberjs.com/ember/release/)
- [Server Discord Ember.JS](https://discord.com/invite/emberjs) — un forum/server di chat dove puoi incontrare la comunità Ember, chiedere aiuto e aiutare altri!

## Risoluzione generale dei problemi, aspetti critici e concezioni errate

Questa non è affatto una lista esaustiva, ma è un elenco di cose che sono emerse al momento della scrittura (ultimo aggiornamento, maggio 2020).

### Come faccio a fare il debug di ciò che accade nel framework?

Per le cose _specifiche del framework_, c'è l'[add-on ember-inspector](https://guides.emberjs.com/release/ember-inspector/), che consente l'ispezione di:

- Rotte e Controller
- Componenti
- Servizi
- Promesse
- Dati (ad esempio, da un'API remota — di default da ember-data)
- Informazioni sulla deprecazione
- Prestazioni di rendering

Per il debugging generale di JavaScript, consulta le nostre [guide sul Debugging di JavaScript](https://firefox-source-docs.mozilla.org/devtools-user/debugger/index.html)
oltre che a interagire con gli [altri strumenti di debugging del browser](https://firefox-source-docs.mozilla.org/devtools-user/index.html). In qualsiasi progetto Ember
predefinito, ci saranno due principali file JavaScript, `vendor.js` e `{app-name}.js`. Entrambi questi file sono generati con sourcemaps, quindi quando apri il `vendor.js` o `{app-name}.js` per cercare il codice pertinente, quando viene posizionato un debugger, il sourcemap sarà caricato e il breakpoint sarà posizionato nel codice pre-transpiled per una più facile correlazione con il codice del tuo progetto.

Per ulteriori informazioni sui sourcemaps, perché sono necessari e cosa fa l'ember-cli con essi, consulta la guida [Uso avanzato: Compilazione delle risorse](https://cli.emberjs.com/release/advanced-use/asset-compilation/). Nota che i sourcemaps sono abilitati per default.

### `ember-data` viene preinstallato; ne ho bisogno?

Assolutamente no. Sebbene `ember-data` risolva _i problemi più comuni_ con cui si confronterà qualsiasi app che gestisce dati, è possibile realizzare un proprio client dati front-end. Un'alternativa comune a qualsiasi client dati front-end completo è [The Fetch API](/it/docs/Web/API/Fetch_API/Using_Fetch).

Utilizzando i modelli di progettazione forniti dal framework, una `Route` usando `fetch()` potrebbe avere questo aspetto:

```js
import Route from "@ember/routing/route";

export default class MyRoute extends Route {
  async model() {
    let response = await fetch("some/url/to/json/data");
    let json = await response.json();

    return {
      data: json,
    };
  }
}
```

Maggiori informazioni su [specificare il modello della `Route`](https://guides.emberjs.com/release/routing/specifying-a-routes-model/) qui.

### Perché non posso semplicemente usare JavaScript?

Questa è la domanda _più_ comune che le persone fanno quando hanno una precedente esperienza con [React](/it/docs/Learn_web_development/Core/Frameworks_libraries/React_getting_started). Sebbene sia tecnicamente possibile utilizzare JSX, o qualsiasi altra forma di creazione del DOM, non c'è ancora nulla di altrettanto robusto quanto il sistema di template di Ember. Il minimalismo intenzionale forza alcune decisioni, e consente un codice più coerente, mantenendo il template più strutturale piuttosto che essere riempito con logica su misura.

Vedi anche: [ReactiveConf 2017: Secrets of the Glimmer VM](https://www.youtube.com/watch?v=nXCSloXZ-wc)

### Qual è lo stato dell'helper `mut`?

`mut` non è stato trattato in questo tutorial ed è realmente un retaggio di un tempo di transizione in cui Ember stava passando da dati a doppio legame a un flusso di dati a singolo legame più comune e facile da comprendere. Potrebbe essere considerato come una trasformazione a tempo di compilazione che avvolge il suo argomento con una funzione setter.

Più concretamente, l'uso di `mut` consente di dichiarare impostazioni solo template:

```hbs-nolint
<Checkbox
  @value=\{{this.someData}}
  @onToggle=\{{fn (mut this.someData) (not this.someData)}}
/>
```

Mentre, senza `mut`, sarebbe necessaria una classe componente:

```js
import Component from "@glimmer/component";
import { tracked } from "@glimmer/tracking";
import { action } from "@ember/object";

export default class Example extends Component {
  @tracked someData = false;

  @action
  setData(newValue) {
    this.someData = newValue;
  }
}
```

Che verrebbe poi chiamata nel template in questo modo:

```hbs-nolint
<Checkbox @data=\{{this.someData}} @onChange=\{{this.setData}} />
```

A causa della concisione dell'uso di `mut`, potrebbe essere desiderabile optare per esso. Tuttavia, `mut` ha semantiche innaturali e ha causato molta confusione durante la sua esistenza.

Ci sono state alcune nuove idee messe insieme nella forma di componenti aggiuntivi che usano le API pubbliche, [`ember-set-helper`](https://github.com/adopted-ember-addons/ember-set-helper) e [`ember-box`](https://github.com/pzuraq/ember-box). Entrambi cercano di risolvere i problemi di `mut` introducendo concetti più evidenti e "meno magici", evitando trasformazioni a tempo di compilazione e comportamenti impliciti della Glimmer VM.

Con `ember-set-helper`:

```hbs
<Checkbox @value=\{{this.someData}} @onToggle=\{{set this "someData" (not
this.someData)}} />
```

Con `ember-box`:

```hbs-nolint
\{{#let (box this.someData) as |someData|}}
  <Checkbox
    @value=\{{unwrap someData}}
    @onToggle=\{{update someData (not this.someData)}}
  />
\{{/let}}
```

Nota che nessuna di queste soluzioni è particolarmente comune tra i membri della comunità, e nel complesso, le persone stanno ancora cercando di trovare un'API ergonomica e semplice per impostare dati in un contesto solo template, senza JavaScript di supporto.

### Qual è lo scopo dei Controller?

[I Controller](https://guides.emberjs.com/release/routing/controllers/) sono [Singleton](https://en.wikipedia.org/wiki/Singleton_pattern), che possono aiutare a gestire il contesto di rendering della rotta attiva. In superficie, funzionano in modo molto simile al JavaScript di supporto di un componente. I Controller sono (a partire da gennaio 2020), l'unico modo per gestire i Parametri di Query URL.

Idealmente, i controller dovrebbero avere responsabilità piuttosto leggere, delegando ai Componenti e ai Servizi dove possibile.

### Qual è lo scopo delle Rotte?

Una [Route](https://guides.emberjs.com/release/routing/defining-your-routes/) rappresenta parte dell'URL quando un utente naviga da un luogo all'altro nell'app.
Una Route ha solo un paio di responsabilità:

- Caricare i _dati minimamente richiesti_ per rendere la rotta (o la vista-sotto-albero).
- Controllare l'accesso alla rotta e reindirizzare se necessario.
- Gestire gli stati di caricamento ed errore dai dati minimamente richiesti.

Una Route ha solo 3 hook del ciclo di vita, tutti opzionali:

- `beforeModel` — controllare l'accesso alla rotta.
- `model` — dove vengono caricati i dati.
- `afterModel` — verificare l'accesso.

Infine, una Route ha la capacità di gestire eventi comuni risultanti dalla configurazione del `model`:

- `loading` — cosa fare quando il `model` è in fase di caricamento.
- `error` — cosa fare quando un errore viene generato da `model`.

Sia `loading` che `error` possono eseguire template di default oltre che template personalizzati definiti altrove nell'applicazione, unificando stati di caricamento/errore.

Maggiori informazioni su [tutto ciò che una Route può fare si trovano nella documentazione API](https://api.emberjs.com/ember/release/classes/route/).

{{PreviousMenu("Learn_web_development/Core/Frameworks_libraries/Ember_routing", "Learn_web_development/Core/Frameworks_libraries")}}
