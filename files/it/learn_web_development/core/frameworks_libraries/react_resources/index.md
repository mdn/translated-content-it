---
title: Risorse su React
slug: Learn_web_development/Core/Frameworks_libraries/React_resources
l10n:
  sourceCommit: 7f138099644a02640a903b2abc39e685ca8ca7cd
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/React_accessibility","Learn_web_development/Core/Accessibility", "Learn_web_development/Core/Frameworks_libraries")}}

Il nostro articolo conclusivo fornisce un elenco di risorse su React da usare per approfondire l'apprendimento.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi fondamentali <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, nonché con il <a href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line">terminale/riga di comando</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>Familiarità con ulteriori risorse per imparare di più su React.</td>
    </tr>
  </tbody>
</table>

## Stili a livello di componente

Sebbene in questo tutorial sia stato mantenuto tutto il CSS in un singolo file `index.css`, nelle applicazioni React è comune definire fogli di stile per singolo componente. In un'applicazione basata su Vite, è possibile farlo creando un file CSS e importandolo nel modulo del componente corrispondente.

Ad esempio, si sarebbe potuto scrivere un file dedicato `Form.css` per contenere il CSS relativo al componente `<Form />`, quindi importare gli stili in `Form.jsx`, in questo modo:

```jsx
import Form from "./Form";
import "./Form.css";
```

Questo approccio semplifica l'identificazione e la gestione del CSS appartenente a un componente specifico, distinguendolo dagli stili dell'intera applicazione. Tuttavia, frammenta anche il foglio di stile nell'intero codice sorgente e questa frammentazione potrebbe non essere vantaggiosa. Per applicazioni più grandi con centinaia di viste uniche e molte parti in movimento, ha senso usare stili a livello di componente e limitare così la quantità di codice irrilevante inviata all'utente in un determinato momento.

È possibile leggere ulteriori informazioni su questo e altri approcci per applicare stili ai componenti React nell'articolo di Smashing Magazine, [Styling Components In React](https://www.smashingmagazine.com/2020/05/styling-components-react/).

## React DevTools

In questo tutorial è stato usato `console.log()` per controllare lo stato e le props dell'applicazione, e sono stati anche mostrati alcuni degli avvisi e messaggi di errore utili forniti da React sia nella CLI sia nella console JavaScript del browser. Ma è possibile fare di più.

L'utilità React DevTools consente di ispezionare direttamente nel browser gli elementi interni dell'applicazione React. Aggiunge un nuovo pannello agli strumenti per sviluppatori del browser, che permette di ispezionare lo stato e le props dei vari componenti e persino di modificare stato e props per apportare cambiamenti immediati all'applicazione.

Questa schermata mostra l'applicazione completata così come appare in React DevTools:

![Il nostro progetto mostrato in React DevTools](react-devtools.png)

A sinistra sono visibili tutti i componenti che costituiscono l'applicazione, comprese le chiavi univoche per gli elementi renderizzati dagli array. A destra sono visibili le props e gli hook utilizzati dal componente App. Si noti inoltre che i componenti `Form`, `FilterButton` e `Todo` sono rientrati verso destra: questo indica che `App` è il loro elemento padre. Questa vista è ottima per comprendere a colpo d'occhio le relazioni padre/figlio ed è preziosa per comprendere applicazioni più complesse.

React DevTools è disponibile in diverse forme:

- Un'[estensione per il browser Chrome](https://chromewebstore.google.com/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en).
- Un'[estensione per il browser Firefox](https://addons.mozilla.org/en-US/firefox/addon/react-devtools/).
- Un'[estensione per il browser Microsoft Edge](https://microsoftedge.microsoft.com/addons/detail/react-developer-tools/gpphkfbcpidddadnkolkpfckpihlkkil).
- Un'[applicazione autonoma installabile con npm o Yarn](https://www.npmjs.com/package/react-devtools).

Provare a installarne una e poi a usarla per ispezionare l'applicazione appena creata.

È possibile [leggere ulteriori informazioni su React DevTools nella documentazione di React](https://react.dev/learn/react-developer-tools).

## L'hook `useReducer()`

In questo tutorial è stato usato l'hook `useState()` per gestire lo stato in una piccola raccolta di funzioni di gestione degli eventi. Questo andava bene a fini di apprendimento, ma ha lasciato la logica di gestione dello stato legata ai gestori degli eventi del componente, in particolare a quelli del componente `<Todo />`.

L'hook `useReducer()` offre agli sviluppatori un modo per consolidare in una singola funzione logiche di gestione dello stato diverse ma correlate. È un po' più complesso di `useState()`, ma è un buon strumento da avere a disposizione. È possibile [leggere ulteriori informazioni su `useReducer()` nella documentazione di React](https://react.dev/learn/extracting-state-logic-into-a-reducer).

## L'API Context

L'applicazione creata in questo tutorial utilizzava le props dei componenti per passare dati dal proprio componente `App` ai componenti figli che ne avevano bisogno. Nella maggior parte dei casi, le props sono un metodo appropriato per condividere dati; tuttavia, per applicazioni complesse e profondamente annidate, non sono sempre la soluzione migliore.

React fornisce l'[API Context](https://react.dev/learn/passing-data-deeply-with-context) come modo per fornire dati ai componenti che ne hanno bisogno _senza_ passare le props lungo l'albero dei componenti. Esiste anche [un hook useContext](https://react.dev/reference/react/useContext) che semplifica questa operazione.

Per provare questa API, Smashing Magazine ha scritto un [articolo introduttivo sul context di React](https://www.smashingmagazine.com/2020/01/introduction-react-context-api/).

## Componenti di classe

Sebbene questo tutorial non li menzioni, è possibile creare componenti React usando le [classi JavaScript](/it/docs/Web/JavaScript/Reference/Classes): questi sono chiamati componenti di classe. Fino all'arrivo degli hook, le classi erano l'unico modo per introdurre lo stato nei componenti o gestire gli effetti collaterali del rendering. Sono ancora l'unico modo per gestire alcuni casi limite e sono comuni nei progetti React legacy. La documentazione ufficiale di React mantiene un riferimento per la classe base [`Component`](https://react.dev/reference/react/Component), ma raccomanda l'uso degli hook per gestire lo [stato](https://react.dev/learn/state-a-components-memory) e gli [effetti collaterali](https://react.dev/learn/synchronizing-with-effects).

## Test

Librerie come [React Testing Library](https://testing-library.com/docs/react-testing-library/intro/) rendono possibile scrivere test unitari per i componenti React. Esistono molti modi per _eseguire_ questi test. Il framework di test [Vitest](https://vitest.dev/) è basato su Vite ed è un ottimo complemento alle applicazioni React basate su Vite. [Jest](https://jestjs.io/) è un altro popolare framework di test utilizzabile con React.

## Routing

Sebbene il routing sia tradizionalmente gestito da un server e non da un'applicazione sul computer dell'utente, è possibile configurare un'applicazione web affinché legga e aggiorni la posizione del browser e renderizzi determinate interfacce utente. Questo è chiamato _routing lato client_. È possibile creare molte route univoche per l'applicazione, ad esempio `/home`, `/dashboard` o `/login`.

[React Router](https://reactrouter.com/) è la libreria di routing lato client per React più popolare e più robusta. Consente agli sviluppatori di definire le route dell'applicazione e di associare componenti a tali route. Fornisce inoltre numerosi hook e componenti utili per gestire la posizione e la cronologia del browser.

> [!NOTE]
> Il routing lato client può far sembrare veloce l'applicazione, ma pone diversi problemi di accessibilità, specialmente per le persone che si affidano alle tecnologie assistive. È possibile leggere ulteriori informazioni nell'articolo di Marcy Sutton, ["The Implications of Client-Side Routing"](https://testingaccessibility.com/implications-of-client-side-routing).

## Riepilogo

Questo è tutto per i framework JavaScript. Ci auguriamo che questo modulo abbia fornito una buona idea del motivo per cui esistono i framework e di come usarli.

Nel prossimo modulo, l'attenzione sarà rivolta all'[accessibilità web](/it/docs/Learn_web_development/Core/Accessibility).

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/React_accessibility","Learn_web_development/Core/Accessibility", "Learn_web_development/Core/Frameworks_libraries")}}
