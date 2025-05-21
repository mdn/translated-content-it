---
title: Risorse di React
slug: Learn_web_development/Core/Frameworks_libraries/React_resources
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/React_accessibility","Learn_web_development/Core/Accessibility", "Learn_web_development/Core/Frameworks_libraries")}}

Il nostro ultimo articolo ti fornisce un elenco di risorse su React che puoi utilizzare per proseguire il tuo apprendimento.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi core <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e il <a href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line">terminale/command line</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>Familiarità con ulteriori risorse per approfondire React.</td>
    </tr>
  </tbody>
</table>

## Stili a livello di componente

Mentre abbiamo mantenuto tutti i CSS per il nostro tutorial in un unico file `index.css`, è comune per le applicazioni React definire fogli di stile per componente. In un'applicazione alimentata da Vite, questo può essere fatto creando un file CSS e importandolo nel modulo del componente corrispondente.

Ad esempio: avremmo potuto creare un file dedicato `Form.css` per ospitare i CSS relativi al componente `<Form />`, quindi importare gli stili in `Form.jsx`, in questo modo:

```jsx
import Form from "./Form";
import "./Form.css";
```

Questo approccio rende facile identificare e gestire i CSS che appartengono a un componente specifico e distinguerli dagli stili dell'intera app. Tuttavia, frammenta anche il tuo foglio di stile all'interno del codice, e questa frammentazione potrebbe non valere la pena. Per applicazioni più grandi con centinaia di viste uniche e molte parti in movimento, ha senso utilizzare stili a livello di componente e quindi limitare la quantità di codice irrilevante inviato all'utente in qualsiasi momento.

Puoi leggere di più su questo e altri approcci per stilizzare i componenti React nell'articolo di Smashing Magazine, [Styling Components In React](https://www.smashingmagazine.com/2020/05/styling-components-react/).

## React DevTools

Abbiamo usato `console.log()` per controllare lo stato e i props della nostra applicazione in questo tutorial, e avrai anche visto alcuni degli avvisi e messaggi di errore utili che React ti fornisce sia nel CLI che nella console JavaScript del tuo browser. Ma c'è di più che possiamo fare qui.

L'utility React DevTools ti permette di ispezionare l'interno della tua applicazione React direttamente nel browser. Aggiunge un nuovo pannello agli strumenti per sviluppatori del browser che ti consente di ispezionare lo stato e i props di vari componenti e persino di modificare stato e props per apportare modifiche immediate alla tua applicazione.

Questo screenshot mostra la nostra applicazione finita come appare in React DevTools:

![Il nostro progetto mostrato in React devtools](react-devtools.png)

A sinistra, vediamo tutti i componenti che compongono la nostra applicazione, inclusi chiavi uniche per gli elementi che sono resi da array. A destra, vediamo i props e gli hook che il nostro componente App utilizza. Nota anche che i componenti `Form`, `FilterButton` e `Todo` sono rientrati a destra – questo indica che `App` è il loro genitore. Questa vista è ottima per comprendere rapidamente le relazioni tra genitori e figli ed è inestimabile per comprendere app più complesse.

React DevTools è disponibile in diverse forme:

- Un [estensione per il browser Chrome](https://chromewebstore.google.com/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en).
- Un [estensione per il browser Firefox](https://addons.mozilla.org/en-US/firefox/addon/react-devtools/).
- Un [estensione per il browser Microsoft Edge](https://microsoftedge.microsoft.com/addons/detail/react-developer-tools/gpphkfbcpidddadnkolkpfckpihlkkil).
- Un [applicazione standalone che puoi installare con npm o Yarn](https://www.npmjs.com/package/react-devtools).

Prova a installarne una e poi usala per ispezionare l'app che hai appena costruito!

Puoi [leggere di più su React DevTools nella documentazione di React](https://react.dev/learn/react-developer-tools).

## L'hook `useReducer()`

In questo tutorial, abbiamo utilizzato l'hook `useState()` per gestire lo stato attraverso una piccola raccolta di funzioni gestore di eventi. Questo andava bene per scopi didattici, ma ha lasciato la nostra logica di gestione dello stato legata ai gestori di eventi del componente – soprattutto quelli del componente `<Todo />`.

L'hook `useReducer()` offre agli sviluppatori un modo per consolidare diverse ma correlate logiche di gestione dello stato in una singola funzione. È un po' più complesso di `useState()`, ma è un buon strumento da avere a disposizione. Puoi [leggere di più su `useReducer()` nella documentazione di React](https://react.dev/learn/extracting-state-logic-into-a-reducer).

## L'API Context

L'applicazione che abbiamo costruito in questo tutorial ha utilizzato i props dei componenti per passare dati dal suo componente `App` ai componenti figli che ne avevano bisogno. La maggior parte delle volte, i props sono un metodo appropriato per condividere dati; tuttavia, per applicazioni complesse e profondamente annidate, non sempre sono la scelta migliore.

React fornisce l'[API Context](https://react.dev/learn/passing-data-deeply-with-context) come un modo per fornire dati ai componenti che ne hanno bisogno _senza_ passare props lungo l'albero dei componenti. C'è anche un [hook useContext](https://react.dev/reference/react/useContext) che facilita questo.

Se ti piacerebbe provare questa API tu stesso, Smashing Magazine ha scritto un [articolo introduttivo sull'API React context](https://www.smashingmagazine.com/2020/01/introduction-react-context-api/).

## Componenti di classe

Anche se questo tutorial non li menziona, è possibile costruire componenti React usando [classi JavaScript](/it/docs/Web/JavaScript/Reference/Classes) – questi sono chiamati componenti di classe. Fino all'arrivo degli hook, le classi erano l'unico modo per portare lo stato nei componenti o gestire effetti collaterali di rendering. Sono ancora l'unico modo per gestire certi casi limite, e sono comuni nei progetti React legacy. La documentazione ufficiale di React mantiene un riferimento per la classe base [`Component`](https://react.dev/reference/react/Component), ma raccomanda di usare gli hook per gestire [stato](https://react.dev/learn/state-a-components-memory) e [effetti collaterali](https://react.dev/learn/synchronizing-with-effects).

## Test

Librerie come [React Testing Library](https://testing-library.com/docs/react-testing-library/intro/) permettono di scrivere test unitari per i componenti React. Ci sono molti modi per _eseguire_ questi test. Il framework di test [Vitest](https://vitest.dev/) è costruito sopra Vite ed è un ottimo compagno per le tue applicazioni React alimentate da Vite. [Jest](https://jestjs.io/) è un altro popolare framework di test che può essere utilizzato con React.

## Routing

Sebbene il routing sia tradizionalmente gestito da un server e non da un'applicazione sul computer dell'utente, è possibile configurare un'applicazione web per leggere e aggiornare la posizione del browser, e renderizzare certe interfacce utente. Questo è chiamato _client-side routing_. È possibile creare molteplici percorsi unici per la tua applicazione (come `/home`, `/dashboard` o `/login`).

[React Router](https://reactrouter.com/) è la libreria di routing lato client più popolare e robusta per React. Consente agli sviluppatori di definire i percorsi della loro applicazione e di associare componenti a tali percorsi. Inoltre, fornisce una serie di hook e componenti utili per gestire la posizione e la cronologia del browser.

> [!NOTE]
> Il client-side routing può far sembrare la tua applicazione veloce, ma pone numerosi problemi di accessibilità, specialmente per le persone che si affidano a tecnologie assistive. Puoi leggere di più su questo nell'articolo di Marcy Sutton, ["The Implications of Client-Side Routing"](https://testingaccessibility.com/implications-of-client-side-routing).

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/React_accessibility","Learn_web_development/Core/Accessibility", "Learn_web_development/Core/Frameworks_libraries")}}
