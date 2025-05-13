---
title: Risorse React
slug: Learn_web_development/Core/Frameworks_libraries/React_resources
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/React_accessibility","Learn_web_development/Core/Accessibility", "Learn_web_development/Core/Frameworks_libraries")}}

Il nostro articolo finale le fornisce un elenco di risorse React che può utilizzare per approfondire il suo apprendimento.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi di base <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e il <a href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line">terminale/command line</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>Familiarità con ulteriori risorse per approfondire la conoscenza di React.</td>
    </tr>
  </tbody>
</table>

## Stili a livello di componente

Anche se abbiamo mantenuto tutti i CSS per il nostro tutorial in un singolo file `index.css`, è comune per le applicazioni React definire fogli di stile per componente. In un'applicazione alimentata da Vite, questo può essere fatto creando un file CSS e importandolo nel suo modulo componente corrispondente.

Ad esempio: avremmo potuto scrivere un file dedicato `Form.css` per ospitare i CSS relativi al componente `<Form />`, quindi importare gli stili in `Form.jsx`, in questo modo:

```jsx
import Form from "./Form";
import "./Form.css";
```

Questo approccio facilita l'identificazione e la gestione dei CSS che appartengono a un componente specifico, distinguendoli dagli stili dell'applicazione in generale. Tuttavia, frammenta anche il suo foglio di stile in tutto il suo codice sorgente e questa frammentazione potrebbe non valerne la pena. Per applicazioni più grandi con centinaia di viste uniche e molte parti in movimento, ha senso utilizzare stili a livello di componente e così limitare la quantità di codice irrilevante che viene inviato all'utente in qualsiasi momento.

Può leggere di più su questo e su altri approcci per stilizzare i componenti React nell'articolo di Smashing Magazine, [Styling Components In React](https://www.smashingmagazine.com/2020/05/styling-components-react/).

## React DevTools

Abbiamo usato `console.log()` per controllare lo stato e le props della nostra applicazione in questo tutorial, e avrà anche visto alcuni degli utili avvisi e messaggi di errore che React le offre, sia nel CLI che nella console JavaScript del suo browser. Ma c'è di più che possiamo fare qui.

L'utilità React DevTools le consente di ispezionare l'interno della sua applicazione React direttamente nel browser. Aggiunge un nuovo pannello agli strumenti di sviluppo del suo browser che le permette di ispezionare lo stato e le props di vari componenti, e persino di modificare stato e props per apportare cambiamenti immediati alla sua applicazione.

Questo screenshot mostra la nostra applicazione finita come appare in React DevTools:

![Il nostro progetto mostrato in React devtools](react-devtools.png)

A sinistra, vediamo tutti i componenti che compongono la nostra applicazione, inclusi chiavi uniche per gli elementi che vengono resi da array. A destra, vediamo le props e i hook che il nostro componente App utilizza. Noti, inoltre, che i componenti `Form`, `FilterButton` e `Todo` sono indentati a destra – questo indica che `App` è il loro genitore. Questa visuale è ottima per comprendere rapidamente le relazioni tra genitore e figlio ed è indispensabile per capire app più complesse.

React DevTools è disponibile in diverse forme:

- Un [estensione per il browser Chrome](https://chromewebstore.google.com/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en).
- Un [estensione per il browser Firefox](https://addons.mozilla.org/en-US/firefox/addon/react-devtools/).
- Un [estensione per il browser Microsoft Edge](https://microsoftedge.microsoft.com/addons/detail/react-developer-tools/gpphkfbcpidddadnkolkpfckpihlkkil).
- Un [applicazione stand-alone che può installare con npm o Yarn](https://www.npmjs.com/package/react-devtools).

Provi a installarne una di queste, e poi la utilizzi per ispezionare l'app che ha appena costruito!

Può [leggere di più su React DevTools nella documentazione React](https://react.dev/learn/react-developer-tools).

## Il hook `useReducer()`

In questo tutorial, abbiamo utilizzato il hook `useState()` per gestire lo stato in una piccola raccolta di funzioni gestore di eventi. Questo andava bene per scopi di apprendimento, ma ha lasciato la nostra logica di gestione dello stato legata ai gestori di eventi del nostro componente – in particolare quelli del componente `<Todo />`.

Il hook `useReducer()` offre agli sviluppatori un modo per consolidare logiche di gestione dello stato diverse ma correlate in una singola funzione. È un po' più complesso di `useState()`, ma è un buon strumento da avere nel suo arsenale. Può [leggere di più su `useReducer()` nella documentazione React](https://react.dev/learn/extracting-state-logic-into-a-reducer).

## L'API Context

L'applicazione che abbiamo costruito in questo tutorial ha utilizzato le props dei componenti per passare dati dal suo componente `App` ai componenti figli che ne avevano bisogno. La maggior parte delle volte, le props sono un metodo appropriato per la condivisione dei dati; per applicazioni complesse e profondamente nidificate, tuttavia, non sono sempre la soluzione migliore.

React fornisce l'[API Context](https://react.dev/learn/passing-data-deeply-with-context) come un modo per fornire dati ai componenti che ne hanno bisogno senza passare props lungo l'albero dei componenti. C'è anche un [hook useContext](https://react.dev/reference/react/useContext) che facilita questo.

Se desidera provare questa API da sola, Smashing Magazine ha scritto un [articolo introduttivo sul contesto di React](https://www.smashingmagazine.com/2020/01/introduction-react-context-api/).

## Componenti di classe

Sebbene questo tutorial non li menzioni, è possibile costruire componenti React usando [classi JavaScript](/it/docs/Web/JavaScript/Reference/Classes) – questi sono chiamati componenti di classe. Fino all'arrivo dei hook, le classi erano l'unico modo per portare lo stato nei componenti o gestire effetti collaterali di rendering. Rimangono l'unico modo per gestire alcuni casi limite, e sono comuni nei progetti React legacy. La documentazione ufficiale di React mantiene un riferimento per la classe base [`Component`](https://react.dev/reference/react/Component), ma consiglia di utilizzare i hook per gestire [stato](https://react.dev/learn/state-a-components-memory) e [effetti collaterali](https://react.dev/learn/synchronizing-with-effects).

## Testing

Librerie come [React Testing Library](https://testing-library.com/docs/react-testing-library/intro/) rendono possibile scrivere test unitari per i componenti React. Ci sono molti modi per _eseguire_ questi test. Il framework di testing [Vitest](https://vitest.dev/) è costruito su Vite, ed è un ottimo compagno per le sue applicazioni React alimentate da Vite. [Jest](https://jestjs.io/) è un altro popolare framework di testing che può essere utilizzato con React.

## Routing

Mentre il routing è tradizionalmente gestito da un server e non da un'applicazione sul computer dell'utente, è possibile configurare un'applicazione web per leggere e aggiornare la posizione del browser, e rendere certe interfacce utente. Questo si chiama _routing lato client_. È possibile creare molteplici percorsi unici per la sua applicazione (come `/home`, `/dashboard`, o `/login`).

[React Router](https://reactrouter.com/) è la libreria di routing lato client più popolare e più robusta per React. Permette agli sviluppatori di definire i percorsi della loro applicazione, e associare componenti a quei percorsi. Fornisce anche una serie di utili hook e componenti per gestire la posizione e la cronologia del browser.

> [!NOTE]
> Il routing lato client può far sentire la sua applicazione veloce, ma pone una serie di problemi di accessibilità, specialmente per le persone che fanno affidamento sulla tecnologia assistiva. Può leggere di più su questo nell'articolo di Marcy Sutton, ["The Implications of Client-Side Routing"](https://testingaccessibility.com/implications-of-client-side-routing).

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/React_accessibility","Learn_web_development/Core/Accessibility", "Learn_web_development/Core/Frameworks_libraries")}}
