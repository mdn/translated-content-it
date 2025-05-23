---
title: Accessibilità in React
short-title: Accessibilità in React
slug: Learn_web_development/Core/Frameworks_libraries/React_accessibility
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/React_interactivity_filtering_conditional_rendering","Learn_web_development/Core/Frameworks_libraries/React_resources", "Learn_web_development/Core/Frameworks_libraries")}}

Nel nostro ultimo articolo di tutorial, ci concentreremo (gioco di parole intenzionale) sull'accessibilità, inclusa la gestione del focus in React, che può migliorare l'usabilità e ridurre la confusione per gli utenti che utilizzano esclusivamente la tastiera e per quelli che usano i lettori di schermo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi principali <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e con il <a href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line">terminale/riga di comando</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>Implementazione dell'accessibilità della tastiera in React.</td>
    </tr>
  </tbody>
</table>

## Inclusione degli utenti della tastiera

A questo punto, abbiamo implementato tutte le funzionalità che ci eravamo prefissati. Gli utenti possono aggiungere un nuovo compito, selezionare e deselezionare compiti, eliminare compiti o modificare i nomi dei compiti. Inoltre, possono filtrare la loro lista di compiti per tutti, attivi o completati.

O, almeno, possono fare tutte queste cose con un mouse. Purtroppo, queste funzionalità non sono molto accessibili agli utenti che utilizzano solo la tastiera. Esploriamo questo problema ora.

## Esplorare il problema della usabilità con la tastiera

Inizia facendo clic sull'input nella parte superiore della nostra app, come se stessi per aggiungere un nuovo compito. Vedrai un contorno spesso e tratteggiato attorno a quell'input. Questo contorno è il tuo indicatore visivo che il browser è attualmente concentrato su questo elemento. Premi il tasto <kbd>Tab</kbd>, e vedrai il contorno apparire attorno al pulsante "Add" sotto l'input. Questo ti mostra che il focus del browser si è spostato.

Premi <kbd>Tab</kbd> alcune volte ancora, e vedrai questo indicatore di focus tratteggiato spostarsi tra ciascuno dei pulsanti di filtro. Continua fino a quando l'indicatore di focus è attorno al primo pulsante "Edit". Premi <kbd>Enter</kbd>.

Il componente `<Todo />` passerà al template che abbiamo progettato, e vedrai un modulo che ti permette di modificare il nome del compito.

Ma dove è finito il nostro indicatore di focus?

Quando passiamo da un template all'altro nel nostro componente `<Todo />`, rimuoviamo completamente gli elementi dal vecchio template e li sostituiamo con gli elementi del nuovo template. Questo significa che l'elemento su cui eravamo focalizzati non esiste più, quindi non c'è alcun segnale visivo per vedere dove si trova il focus del browser. Questo potrebbe confondere una vasta gamma di utenti — in particolare utenti che si affidano alla tastiera o utenti che utilizzano tecnologia assistiva.

Per migliorare l'esperienza per gli utenti della tastiera e della tecnologia assistiva, dovremmo gestire il focus del browser noi stessi.

### A parte: una nota sul nostro indicatore di focus

Se clicchi sui pulsanti del filtro "All", "Active" o "Completed" con il mouse, _non_ vedrai un indicatore di focus visibile, ma lo vedrai se ti sposti tra loro con il tasto <kbd>Tab</kbd> sulla tastiera. Non preoccuparti — il tuo codice non è rotto!

Il nostro file CSS utilizza la pseudo-classe [`:focus-visible`](/it/docs/Web/CSS/:focus-visible) per fornire uno stile personalizzato per l'indicatore di focus, e il browser utilizza un insieme di regole interne per determinare quando mostrarlo all'utente. Generalmente, il browser _mostrerà_ un indicatore di focus in risposta all'input da tastiera, e _potrebbe_ mostrarlo in risposta all'input del mouse. Gli elementi `<button>` _non_ mostrano un indicatore di focus in risposta all'input del mouse, mentre gli elementi `<input>` _lo fanno_.

Il comportamento di `:focus-visible` è più selettivo rispetto alla vecchia pseudo-classe [`:focus`](/it/docs/Web/CSS/:focus), con cui potresti avere maggiore familiarità. `:focus` mostra un indicatore di focus in molte più situazioni, e puoi usarlo al posto di o in combinazione con `:focus-visible` se preferisci.

## Focalizzarsi tra i template

Quando un utente cambia il template `<Todo />` da visualizzazione a modifica, dovremmo concentrare il focus sull'`<input>` usato per rinominarlo; quando cambiano di nuovo da modifica a visualizzazione, dovremmo spostare il focus di nuovo sul pulsante "Edit".

### Targeting dei nostri elementi

Fino a questo punto, abbiamo scritto componenti JSX e lasciato che React costruisse il DOM risultante dietro le quinte. La maggior parte del tempo, non abbiamo bisogno di puntare a elementi specifici nel DOM perché possiamo usare lo stato e le props di React per controllare ciò che viene renderizzato. Tuttavia, per gestire il focus, _abbiamo_ bisogno di essere in grado di puntare a elementi specifici del DOM.

È qui che entra in gioco il hook `useRef()`.

Per prima cosa, cambia l'istruzione `import` nella parte superiore di `Todo.jsx` in modo che includa `useRef`:

```jsx
import { useRef, useState } from "react";
```

`useRef()` crea un oggetto con una singola proprietà: `current`. I ref possono memorizzare qualsiasi valore vogliamo, e possiamo guardare quei valori in seguito. Possiamo persino memorizzare riferimenti a elementi del DOM, che è esattamente quello che faremo qui.

Successivamente, crea due nuove costanti sotto i hook `useState()` nella tua funzione `Todo()`. Ciascuna dovrebbe essere un ref: uno per il pulsante "Edit" nel template di visualizzazione e uno per il campo di modifica nel template di modifica.

```jsx
const editFieldRef = useRef(null);
const editButtonRef = useRef(null);
```

Questi ref hanno un valore predefinito di `null` per chiarire che saranno vuoti fino a quando non saranno collegati ai loro elementi del DOM. Per collegarli ai loro elementi, aggiungeremo l'attributo speciale `ref` a ciascun elemento JSX, e imposteremo i valori di quegli attributi sugli oggetti `ref` appropriati.

Aggiorna l'`<input>` nel tuo template di modifica in modo che legga così:

```jsx
<input
  id={props.id}
  className="todo-text"
  type="text"
  value={newName}
  onChange={handleChange}
  ref={editFieldRef}
/>
```

Aggiorna il pulsante "Edit" nel tuo template di visualizzazione in modo che legga così:

```jsx
<button
  type="button"
  className="btn"
  onClick={() => setEditing(true)}
  ref={editButtonRef}>
  Edit <span className="visually-hidden">{props.name}</span>
</button>
```

Facendo questo, popoleremo i nostri `editFieldRef` e `editButtonRef` con riferimenti agli elementi del DOM a cui sono collegati, ma _solo_ dopo che React ha reso il componente. Prova tu stesso: aggiungi la seguente riga da qualche parte nel corpo della tua funzione `Todo()`, sotto dove `editButtonRef` è inizializzato:

```jsx
console.log(editButtonRef.current);
```

Vedrai che il valore di `editButtonRef.current` è `null` quando il componente viene renderizzato per la prima volta, ma se clicchi su un pulsante "Edit", registrerà l'elemento `<button>` nella console. Questo perché il ref viene popolato solo dopo che il componente è stato renderizzato, e cliccando sul pulsante "Edit" si induce il componente a essere rerenderizzato. Assicurati di eliminare questo log prima di procedere.

> [!NOTE]
> I tuoi log appariranno 6 volte perché abbiamo 3 istanze di `<Todo />` nella nostra app e React renderizza i nostri componenti due volte in ambiente di sviluppo.

Ci stiamo avvicinando! Per sfruttare i nostri elementi di riferimento appena creati, dobbiamo usare un altro hook di React: `useEffect()`.

### Implementazione di `useEffect()`

[`useEffect()`](https://react.dev/reference/react/useEffect) è così chiamato perché esegue eventuali effetti collaterali che vorremmo aggiungere al processo di rendering ma che non possono essere eseguiti all'interno del corpo della funzione principale. `useEffect()` viene eseguito subito dopo che un componente è stato renderizzato, il che significa che gli elementi del DOM a cui ci siamo riferiti nella sezione precedente saranno disponibili per l'uso.

Cambia di nuovo l'istruzione import di `Todo.jsx` per aggiungere `useEffect`:

```jsx
import { useEffect, useRef, useState } from "react";
```

`useEffect()` prende una funzione come argomento; questa funzione viene eseguita _dopo_ il rendering del componente. Per dimostrarlo, metti la seguente chiamata `useEffect()` appena sopra l'istruzione `return` nel corpo di `Todo()`, e passa una funzione che registra la parola "effetto collaterale" nella tua console:

```jsx
useEffect(() => {
  console.log("side effect");
});
```

Per illustrare la differenza tra il processo di rendering principale e il codice eseguito all'interno di `useEffect()`, aggiungi un altro log – poni questo sotto la precedente aggiunta:

```jsx
console.log("main render");
```

Ora, apri l'app nel tuo browser. Dovresti vedere entrambi i messaggi nella tua console, con ciascuno che si ripete più volte. Nota come "main render" viene registrato per primo, e "side effect" è registrato per secondo, nonostante il log "side effect" appaia per primo nel codice.

```plain
main render                                     Todo.jsx
side effect                                     Todo.jsx
```

Ancora una volta, i log sono ordinati in questo modo perché il codice all'interno di `useEffect()` viene eseguito _dopo_ il rendering del componente. Questo richiede un po' di pratica, tienilo a mente mentre procedi. Per ora, elimina `console.log("main render")` e passiamo all'implementazione della nostra gestione del focus.

### Focalizzarci sul nostro campo di modifica

Ora che sappiamo che il nostro hook `useEffect()` funziona, possiamo gestire il focus con esso. Come promemoria, vogliamo focalizzarci sul campo di modifica quando passiamo al template di modifica.

Aggiorna il tuo hook `useEffect()` esistente in modo che legga così:

```jsx
useEffect(() => {
  if (isEditing) {
    editFieldRef.current.focus();
  }
}, [isEditing]);
```

Queste modifiche fanno sì che, se `isEditing` è vero, React legga il valore corrente del `editFieldRef` e sposti il focus del browser su di esso. Passiamo anche un array a `useEffect()` come secondo argomento. Questo array è un elenco di valori da cui `useEffect()` dovrebbe dipendere. Con questi valori inclusi, `useEffect()` verrà eseguito solo quando uno di quei valori cambia. Vogliamo cambiare il focus solo quando il valore di `isEditing` cambia.

Provalo ora: utilizza il tasto <kbd>Tab</kbd> per navigare fino a uno dei pulsanti "Edit", quindi premi <kbd>Enter</kbd>. Dovresti vedere il componente `<Todo />` passare al suo template di modifica, e l'indicatore di focus del browser dovrebbe comparire attorno all'elemento `<input>`!

### Spostare il focus indietro sul pulsante di modifica

A prima vista, far sì che React sposti il focus di nuovo sul nostro pulsante "Edit" quando la modifica è salvata o annullata sembra ingannevolmente facile. Sicuramente potremmo aggiungere una condizione al nostro `useEffect` per focalizzarci sul pulsante di modifica se `isEditing` è `false`? Proviamolo ora — aggiorna la tua chiamata `useEffect()` in questo modo:

```jsx
useEffect(() => {
  if (isEditing) {
    editFieldRef.current.focus();
  } else {
    editButtonRef.current.focus();
  }
}, [isEditing]);
```

Questo tipo di funziona. Se usi la tua tastiera per attivare il pulsante "Edit" (ricorda: <kbd>Tab</kbd> premuto e premi <kbd>Enter</kbd>), vedrai che il tuo focus si sposta tra la `<input>` di modifica e il pulsante "Edit" mentre inizi e termini una modifica. Tuttavia, potresti aver notato un nuovo problema: il pulsante "Edit" nel componente finale `<Todo />` è focalizzato immediatamente all'avvio della pagina, prima ancora di interagire con l'app!

Il nostro hook `useEffect()` si comporta esattamente come lo abbiamo progettato: viene eseguito non appena il componente viene renderizzato, vede che `isEditing` è `false`, e focalizza il pulsante "Edit". Ci sono tre istanze di `<Todo />`, e il focus viene dato al pulsante "Edit" di quello che viene renderizzato per ultimo.

Dobbiamo rifare il nostro approccio in modo che il focus cambi solo quando `isEditing` cambia da un valore all'altro.

## Gestione del focus più robusta

Per soddisfare i nostri criteri raffinati, dobbiamo sapere non solo il valore di `isEditing`, ma anche _quando quel valore è cambiato_. Per farlo, dobbiamo essere in grado di leggere il valore precedente della costante `isEditing`. Usando pseudocodice, la nostra logica dovrebbe essere simile a questa:

```jsx
if (wasNotEditingBefore && isEditingNow) {
  focusOnEditField();
} else if (wasEditingBefore && isNotEditingNow) {
  focusOnEditButton();
}
```

Il team di React ha discusso su [modi per ottenere lo stato precedente di un componente](https://legacy.reactjs.org/docs/hooks-faq.html#how-to-get-the-previous-props-or-state), e ha fornito un esempio di hook che possiamo usare per questo compito.

### Introdurre `usePrevious()`

Incolla il seguente codice vicino alla parte superiore di `Todo.jsx`, sopra la tua funzione `Todo()`.

```jsx
function usePrevious(value) {
  const ref = useRef();
  useEffect(() => {
    ref.current = value;
  });
  return ref.current;
}
```

`usePrevious()` è un _custom hook_ che traccia un valore tra i rendering. Esso:

1. Usa il hook `useRef()` per creare un `ref` vuoto.
2. Restituisce il valore `current` del `ref` al componente che l'ha chiamato.
3. Chiama `useEffect()` e aggiorna il valore memorizzato in `ref.current` dopo ciascun rendering del componente chiamante.

Il comportamento di `useEffect()` è fondamentale per questa funzionalità. Poiché `ref.current` viene aggiornato all'interno di una chiamata `useEffect()`, è sempre uno step indietro rispetto a qualunque valore si trovi nel ciclo di rendering principale del componente — da qui il nome `usePrevious()`.

### Usare `usePrevious()`

Ora possiamo definire una costante `wasEditing` per tracciare il valore precedente di `isEditing`; questo viene ottenuto chiamando `usePrevious` con `isEditing` come argomento. Aggiungi il seguente all'interno di `Todo()`, sotto le righe `useRef`:

```jsx
const wasEditing = usePrevious(isEditing);
```

Puoi vedere come si comporta `usePrevious()` aggiungendo un log di console sotto questa linea:

```jsx
console.log(wasEditing);
```

In questo log, il valore `current` di `wasEditing` sarà sempre il valore precedente di `isEditing`. Clicca sui pulsanti "Edit" e "Cancel" alcune volte per vederlo cambiare, quindi elimina questo log quando sei pronto per procedere.

Con questa costante `wasEditing`, possiamo aggiornare il nostro hook `useEffect()` per implementare lo pseudocodice di cui abbiamo discusso prima:

```jsx
useEffect(() => {
  if (!wasEditing && isEditing) {
    editFieldRef.current.focus();
  } else if (wasEditing && !isEditing) {
    editButtonRef.current.focus();
  }
}, [wasEditing, isEditing]);
```

Nota che la logica di `useEffect()` ora dipende da `wasEditing`, quindi la forniamo nell'array delle dipendenze.

Prova a usare la tua tastiera per attivare i pulsanti "Edit" e "Cancel" nel componente `<Todo />`; vedrai l'indicatore di focus del browser muoversi correttamente, senza il problema discusso all'inizio di questa sezione.

## Focalizzarsi quando l'utente elimina un compito

C'è un ultimo divario nell'esperienza della tastiera: quando un utente elimina un compito dall'elenco, il focus scompare. Seguiremo un modello simile ai nostri cambiamenti precedenti: creeremo un nuovo ref e utilizzeremo il nostro hook `usePrevious()`, in modo che possiamo concentrarci sull'intestazione dell'elenco ogni volta che un utente elimina un compito.

### Perché l'intestazione dell'elenco?

A volte, il luogo in cui vogliamo inviare il nostro focus è ovvio: quando abbiamo alternato i nostri template `<Todo />`, avevamo un punto di origine a cui "tornare" — il pulsante "Edit". In questo caso, tuttavia, dato che stiamo rimuovendo completamente elementi dal DOM, non abbiamo un posto dove tornare. La prossima cosa migliore è un luogo intuitivo da qualche parte vicino. L'intestazione dell'elenco è la nostra scelta migliore perché è vicina all'elemento della lista che l'utente eliminerà, e focalizzarsi su di essa dirà all'utente quanti compiti rimangono.

### Creazione del nostro ref

Importa i hook `useRef()` e `useEffect()` in `App.jsx` — avrai bisogno di entrambi di seguito:

```jsx
import { useState, useRef, useEffect } from "react";
```

Successivamente, dichiara un nuovo ref all'interno della funzione `App()`, appena sopra l'istruzione `return`:

```jsx
const listHeadingRef = useRef(null);
```

### Preparare l'intestazione

Gli elementi di intestazione come il nostro `<h2>` non sono solitamente focalizzabili. Questo non è un problema — possiamo rendere qualsiasi elemento focalizzabile in modo programmatico aggiungendo l'attributo [`tabindex="-1"`](/it/docs/Web/HTML/Reference/Global_attributes/tabindex) ad esso. Questo significa _soltanto focalizzabile con JavaScript_. Non puoi premere <kbd>Tab</kbd> per concentrarti su un elemento con un tabindex di `-1` nello stesso modo in cui potresti fare con un elemento [`<button>`](/it/docs/Web/HTML/Reference/Elements/button) o [`<a>`](/it/docs/Web/HTML/Reference/Elements/a) (questo può essere fatto con `tabindex="0"`, ma non è appropriato in questo caso).

Aggiungiamo l'attributo `tabindex` — scritto come `tabIndex` in JSX — all'intestazione sopra il nostro elenco di compiti, insieme al nostro `listHeadingRef`:

```jsx
<h2 id="list-heading" tabIndex="-1" ref={listHeadingRef}>
  {headingText}
</h2>
```

> [!NOTE]
> L'attributo `tabindex` è eccellente per i casi limite di accessibilità, ma dovresti fare **molta attenzione** a non abusarne. Applicalo solo a un elemento quando sei sicuro che renderlo focalizzabile porterà un beneficio al tuo utente in qualche modo. Nella maggior parte dei casi, dovresti utilizzare elementi che possano prendere naturalmente il focus, come pulsanti, ancore e input. Un uso irresponsabile del `tabindex` potrebbe avere un impatto notevolmente negativo sugli utenti della tastiera e dei lettori di schermo!

### Ottenere lo stato precedente

Vogliamo concentrarci sull'elemento associato al nostro ref (tramite l'attributo `ref`) solo quando il nostro utente elimina un compito dalla loro lista. Questo richiederà l'hook `usePrevious()` che abbiamo usato in precedenza. Aggiungilo in cima al tuo file `App.jsx`, subito sotto gli import:

```jsx
function usePrevious(value) {
  const ref = useRef();
  useEffect(() => {
    ref.current = value;
  });
  return ref.current;
}
```

Ora aggiungi il seguente, sopra l'istruzione `return` all'interno della funzione `App()`:

```jsx
const prevTaskLength = usePrevious(tasks.length);
```

Qui stiamo invocando `usePrevious()` per tracciare la lunghezza precedente dell'array dei compiti.

> [!NOTE]
> Dato che ora stiamo utilizzando `usePrevious()` in due file, potrebbe essere più efficiente spostare la funzione `usePrevious()` in un file a parte, esportarla da quel file, e importarla dove serve. Prova a fare questo come esercizio una volta arrivato alla fine.

### Usare `useEffect()` per controllare il focus dell'intestazione

Ora che abbiamo memorizzato quanti compiti avevamo in precedenza, possiamo impostare un hook `useEffect()` da eseguire quando cambia il numero di compiti, che si focalizzerà sull'intestazione se il numero di compiti che abbiamo ora è inferiore rispetto a prima — ovvero, abbiamo eliminato un compito!

Aggiungi il seguente nel corpo della funzione `App()`, appena sotto le tue aggiunte precedenti:

```jsx
useEffect(() => {
  if (tasks.length < prevTaskLength) {
    listHeadingRef.current.focus();
  }
}, [tasks.length, prevTaskLength]);
```

Proviamo a concentrarci sulla nostra intestazione dell'elenco solo se abbiamo meno compiti rispetto a prima. Le dipendenze passate in questo hook garantiscono che verrà eseguito solo quando uno di quei valori (il numero di compiti attuali o il numero di compiti precedenti) cambia.

Ora, quando usi la tua tastiera per eliminare un compito nel tuo browser, vedrai il nostro contorno di focus tratteggiato apparire attorno all'intestazione sopra l'elenco.

## Finito!

Hai appena finito di costruire un'app React da zero! Congratulazioni! Le competenze che hai acquisito qui saranno un'ottima base su cui costruire mentre continui a lavorare con React.

La maggior parte del tempo, puoi essere un contributore efficace a un progetto React anche se tutto ciò che fai è pensare con attenzione ai componenti e al loro stato e props. Ricorda di scrivere sempre il miglior HTML possibile.

`useRef()` e `useEffect()` sono funzionalità piuttosto avanzate, e dovresti essere orgoglioso di averle usate! Cerca opportunità per praticarle di più, perché farlo ti permetterà di creare esperienze inclusive per gli utenti. Ricorda: la nostra app sarebbe stata inaccessibile agli utenti della tastiera senza di loro!

> [!NOTE]
> Se hai bisogno di confrontare il tuo codice con la nostra versione, puoi trovare una versione finita del codice di esempio dell'app React nel nostro [repository todo-react](https://github.com/mdn/todo-react). Per una versione live in esecuzione, vedi <https://mdn.github.io/todo-react/>.

Nell'ultimo articolo ti presenteremo un elenco di risorse React che puoi usare per approfondire il tuo apprendimento.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/React_interactivity_filtering_conditional_rendering","Learn_web_development/Core/Frameworks_libraries/React_resources", "Learn_web_development/Core/Frameworks_libraries")}}
