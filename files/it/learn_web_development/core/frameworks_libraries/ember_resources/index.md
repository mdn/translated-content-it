---
title: Risorse e risoluzione dei problemi di Ember
slug: Learn_web_development/Core/Frameworks_libraries/Ember_resources
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenu("Learn_web_development/Core/Frameworks_libraries/Ember_routing", "Learn_web_development/Core/Frameworks_libraries")}}

Il nostro articolo finale su Ember le offre un elenco di risorse che può utilizzare per approfondire il suo apprendimento, oltre a informazioni utili per la risoluzione dei problemi e altre informazioni.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          È consigliato avere familiarità con i linguaggi di base
          <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e
          avere conoscenze del
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminale/linea di comando</a
          >.
        </p>
        <p>
          Una comprensione più approfondita delle funzionalità moderne di JavaScript (come classi,
          moduli, ecc.) sarà molto utile, poiché Ember ne fa ampio uso.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Fornire ulteriori risorse di collegamento e informazioni per la risoluzione dei problemi.
      </td>
    </tr>
  </tbody>
</table>

## Ulteriori risorse

- [Guide di Ember.JS](https://guides.emberjs.com/release/)

  - [Tutorial: Super Rentals](https://guides.emberjs.com/release/tutorial/part-1/)

- [Documentazione API di Ember.JS](https://api.emberjs.com/ember/release/)
- [Server Discord di Ember.JS](https://discord.com/invite/emberjs) — un forum/server di chat dove può incontrare la comunità Ember, chiedere aiuto e aiutare gli altri!

## Risoluzione generale dei problemi, gotchas e incomprensioni

Questa non è una lista estesa, ma è un elenco di cose emerse al momento della scrittura (ultimo aggiornamento, maggio 2020).

### Come posso eseguire il debug di ciò che accade nel framework?

Per le cose _specifiche del framework_, c'è l'[add-on ember-inspector](https://guides.emberjs.com/release/ember-inspector/), che consente di ispezionare:

- Routes & Controllers
- Componenti
- Servizi
- Promesse
- Dati (ovvero, da un'API remota — da ember-data, per impostazione predefinita)
- Informazioni di deprecazione
- Prestazioni di rendering

Per il debug generale di JavaScript, consulti la nostra [guida al Debugging di JavaScript](https://firefox-source-docs.mozilla.org/devtools-user/debugger/index.html)
così come l'interazione con gli [altri strumenti di debug del browser](https://firefox-source-docs.mozilla.org/devtools-user/index.html). In qualsiasi progetto Ember predefinito, ci saranno due file JavaScript principali, `vendor.js` e `{app-name}.js`. Entrambi
questi file sono generati con sourcemap, quindi quando apre `vendor.js` o `{app-name}.js` per cercare il codice rilevante, quando viene posto un debugger, il sourcemap verrà caricato e il punto di interruzione verrà posizionato nel codice pre-tradotto per una più facile correlazione al codice del suo progetto.

Per ulteriori informazioni sulle sourcemap, sul perché sono necessarie e su cosa fa ember-cli con loro, veda la [Guida all'uso avanzato: Compilazione delle Risorse](https://cli.emberjs.com/release/advanced-use/asset-compilation/). Noti che le sourcemap sono abilitate per impostazione predefinita.

### `ember-data` viene preinstallato; ne ho bisogno?

Assolutamente no. Sebbene `ember-data` risolva _i problemi più comuni_ che qualsiasi app che si occupa di dati può incontrare, è possibile creare il proprio client dati front-end. Un'alternativa comune a qualsiasi client dati front-end completo è [The Fetch API](/it/docs/Web/API/Fetch_API/Using_Fetch).

Utilizzando i pattern di progettazione forniti dal framework, un `Route` usando `fetch()` apparirebbe in questo modo:

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

Per ulteriori informazioni su [specificare il modello di `Route`](https://guides.emberjs.com/release/routing/specifying-a-routes-model/) veda qui.

### Perché non posso semplicemente usare JavaScript?

Questa è la domanda _più_ comune che gli utenti di Ember sentono porgere da persone che hanno esperienza precedente con [React](/it/docs/Learn_web_development/Core/Frameworks_libraries/React_getting_started). Sebbene sia tecnicamente possibile utilizzare JSX o qualsiasi altra forma di creazione del DOM, non c'è ancora nulla di altrettanto robusto come il sistema di templating di Ember. Il minimalismo intenzionale forza certe decisioni e consente un codice più coerente, mantenendo il template più strutturale piuttosto che averlo riempito di logica su misura.

Si veda anche: [ReactiveConf 2017: Segreti della Glimmer VM](https://www.youtube.com/watch?v=nXCSloXZ-wc)

### Qual è lo stato del helper `mut`?

`mut` non è stato trattato in questo tutorial ed è davvero un retaggio di un periodo di transizione quando Ember stava passando da dati a doppio legame a un flusso di dati a legame unidirezionale più comune e più facile da comprendere. Potrebbe essere considerato come una trasformazione eseguita in fase di build che avvolge il suo argomento con una funzione setter.

Più concretamente, usando `mut` è possibile dichiarare funzioni di impostazione solo nel template:

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

Che verrebbe quindi chiamata nel template in questo modo:

```hbs-nolint
<Checkbox @data=\{{this.someData}} @onChange=\{{this.setData}} />
```

A causa della concisione dell'utilizzo di `mut`, potrebbe essere desiderabile farvi ricorso. Tuttavia, `mut` ha semantiche innaturali e ha causato molta confusione nel corso della sua esistenza.

Sono state proposte alcune nuove idee sotto forma di addon che utilizzano le API pubbliche, [`ember-set-helper`](https://github.com/adopted-ember-addons/ember-set-helper) e [`ember-box`](https://github.com/pzuraq/ember-box). Entrambi questi tentano di risolvere i problemi di `mut` introducendo concetti più evidenti / "meno magici", evitando trasformazioni eseguite in fase di build e comportamenti impliciti della Glimmer VM.

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

Noti che nessuna di queste soluzioni è particolarmente comune tra i membri della comunità, e nel complesso, le persone stanno ancora cercando di trovare un'API ergonomica e semplice per impostare dati in un contesto di template-only, senza il supporto di JS.

### Qual è lo scopo dei Controllers?

[I Controllers](https://guides.emberjs.com/release/routing/controllers/) sono [Singletons](https://en.wikipedia.org/wiki/Singleton_pattern), che possono aiutare a gestire il contesto di rendering della route attiva. In superficie, funzionano in modo molto simile al supporto JavaScript di un componente. I Controllers sono (a partire da gennaio 2020), l'unico modo per gestire i parametri di query URL.

Idealmente, i controllers dovrebbero avere responsabilità piuttosto leggere, delegando a Componenti e Servizi ove possibile.

### Qual è lo scopo delle Routes?

Una [Route](https://guides.emberjs.com/release/routing/defining-your-routes/) rappresenta parte dell'URL quando un utente naviga da un luogo all'altro nell'app.
Una Route ha solo un paio di responsabilità:

- Caricare i _dati minimamente richiesti_ per visualizzare la route (o la sotto-vista).
- Controllare l'accesso alla route e reindirizzare se necessario.
- Gestire gli stati di caricamento ed errore dai dati minimamente richiesti.

Una Route ha solo 3 hook di ciclo di vita, tutti facoltativi:

- `beforeModel` — controllare l'accesso alla route.
- `model` — dove vengono caricati i dati.
- `afterModel` — verificare l'accesso.

Infine, una Route ha la capacità di gestire eventi comuni risultanti dalla configurazione del `model`:

- `loading` — cosa fare quando la funzione `model` è in caricamento.
- `error` — cosa fare quando un errore viene lanciato da `model`.

Sia `loading` che `error` possono renderizzare template di default così come template personalizzati definiti altrove nell'applicazione, unificando gli stati di caricamento/errore.

Ulteriori informazioni su [tutto ciò che una Route può fare si trovano nella documentazione API](https://api.emberjs.com/ember/release/classes/route/).

{{PreviousMenu("Learn_web_development/Core/Frameworks_libraries/Ember_routing", "Learn_web_development/Core/Frameworks_libraries")}}
