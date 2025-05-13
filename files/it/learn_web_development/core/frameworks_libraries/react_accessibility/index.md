---
title: Accessibilità in React
short-title: Accessibilità in React
slug: Learn_web_development/Core/Frameworks_libraries/React_accessibility
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/React_interactivity_filtering_conditional_rendering","Learn_web_development/Core/Frameworks_libraries/React_resources", "Learn_web_development/Core/Frameworks_libraries")}}

Nel nostro ultimo articolo del tutorial, ci concentreremo (gioco di parole voluto) sull'accessibilità, compresa la gestione del focus in React, che può migliorare l'usabilità e ridurre la confusione sia per gli utenti che utilizzano solo tastiera che per quelli che usano lettori di schermo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi di base <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e con il <a href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line">terminale/command line</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi formativi:</th>
      <td>Implementare l'accessibilità da tastiera in React.</td>
    </tr>
  </tbody>
</table>

## Includere gli utenti della tastiera

A questo punto, abbiamo implementato tutte le funzionalità che ci eravamo prefissati di implementare. Gli utenti possono aggiungere un nuovo compito, selezionare e deselezionare compiti, eliminare compiti o modificare i nomi dei compiti. Inoltre, possono filtrare la loro lista di compiti per tutti, attivi o completati.

O, almeno, possono fare tutte queste cose con un mouse. Sfortunatamente, queste caratteristiche non sono molto accessibili agli utenti che utilizzano solo la tastiera. Esploriamo questo aspetto ora.

## Esplorare il problema dell'usabilità da tastiera

Inizia facendo clic sull'input nella parte superiore della nostra app, come se dovessi aggiungere un nuovo compito. Vedrai un contorno spesso e tratteggiato attorno a quell'input. Questo contorno è il tuo indicatore visivo che il browser è attualmente focalizzato su questo elemento. Premi il tasto <kbd>Tab</kbd>, e vedrai il contorno apparire attorno al pulsante "Aggiungi" sotto l'input. Questo ti mostra che il focus del browser si è spostato.

Premi <kbd>Tab</kbd> ancora alcune volte, e vedrai questo indicatore di focus tratteggiato muoversi tra ciascuno dei pulsanti di filtro. Continua finché l'indicatore di focus non è attorno al primo pulsante "Modifica". Premi <kbd>Invio</kbd>.

Il componente `<Todo />` cambierà modelli, come progettato, e vedrai un modulo che ci permette di modificare il nome del compito.

Ma dov'è finito il nostro indicatore di focus?

Quando passiamo da un modello all'altro nel nostro componente `<Todo />`, rimuoviamo completamente gli elementi dal vecchio modello e li sostituiamo con gli elementi del nuovo modello. Ciò significa che l'elemento su cui eravamo focalizzati non esiste più, quindi non c'è alcun suggerimento visivo su dove sia il focus del browser. Questo potrebbe confondere una vasta gamma di utenti — in particolare utenti che si affidano alla tastiera o che utilizzano tecnologie assistive.

Per migliorare l'esperienza per gli utenti della tastiera e delle tecnologie assistive, dovremmo gestire il focus del browser noi stessi.

### A parte: una nota sul nostro indicatore di focus

Se fai clic sui pulsanti filtro "Tutti", "Attivi" o "Completati" con il mouse, _non_ vedrai un indicatore di focus visibile, ma lo vedrai se ti muovi tra di essi con il tasto <kbd>Tab</kbd> sulla tastiera. Non preoccuparti — il tuo codice non è rotto!

Il nostro file CSS utilizza la pseudo-classe [`:focus-visible`](/it/docs/Web/CSS/:focus-visible) per fornire uno stile personalizzato per l'indicatore di focus, e il browser utilizza un insieme di regole interne per determinare quando mostrarlo all'utente. Generalmente, il browser _mostrerà_ un indicatore di focus in risposta all'input della tastiera, e _potrebbe_ mostrarlo in risposta all'input del mouse. Gli elementi `<button>` _non_ mostrano un indicatore di focus in risposta all'input del mouse, mentre gli elementi `<input>` _lo fanno_.

Il comportamento di `:focus-visible` è più selettivo rispetto alla vecchia pseudo-classe [`:focus`](/it/docs/Web/CSS/:focus), con cui potresti essere più familiare. `:focus` mostra un indicatore di focus in molte più situazioni, e puoi usarlo invece di o in combinazione con `:focus-visible` se preferisci.

## Focalizzarsi tra i modelli

Quando un utente cambia il modello del `<Todo />` da visualizzazione a modifica, dovremmo focalizzarci sull'`<input>` usato per rinominarlo; quando torna dalla modifica alla visualizzazione, dovremmo spostare il focus di nuovo sul pulsante "Modifica".

### Destinazione dei nostri elementi

Fino a questo punto, abbiamo scritto componenti JSX e lasciato che React costruisse il DOM risultante dietro le quinte. La maggior parte delle volte, non abbiamo bisogno di destinare elementi specifici nel DOM perché possiamo usare lo stato e i props di React per controllare ciò che viene reso. Per gestire il focus, tuttavia, _abbiamo_ bisogno di poter destinare elementi DOM specifici.

Qui entra in gioco il hook `useRef()`.

Innanzitutto, cambia l'istruzione `import` nella parte superiore di `Todo.jsx` in modo che includa `useRef`:

```jsx
import { useRef, useState } from "react";
```

`useRef()` crea un oggetto con una singola proprietà: `current`. I riferimenti possono memorizzare qualsiasi valore desideriamo, e possiamo recuperare quei valori in seguito. Possiamo persino memorizzare riferimenti a elementi DOM, che è esattamente quello che faremo qui.

Successivamente, crea due nuove costanti sotto gli hook `useState()` nella tua funzione `Todo()`. Ognuna dovrebbe essere un riferimento – uno per il pulsante "Modifica" nel modello di visualizzazione e uno per il campo di modifica nel modello di modifica.

```jsx
const editFieldRef = useRef(null);
const editButtonRef = useRef(null);
```

Questi riferimenti hanno un valore predefinito di `null` per chiarire che saranno vuoti finché non verranno associati ai loro elementi DOM. Per associarli ai loro elementi, aggiungeremo l'attributo speciale `ref` a ciascun elemento JSX e imposteremo i valori di quegli attributi sugli oggetti `ref` con il nome appropriato.

Aggiorna l'`<input>` nel tuo modello di modifica in modo che appaia così:

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

Aggiorna il pulsante "Modifica" nel tuo modello di visualizzazione in modo che appaia così:

```jsx
<button
  type="button"
  className="btn"
  onClick={() => setEditing(true)}
  ref={editButtonRef}>
  Edit <span className="visually-hidden">{props.name}</span>
</button>
```

Facendo ciò, popoleremo il nostro `editFieldRef` e `editButtonRef` con riferimenti agli elementi DOM a cui sono associati, ma _solo_ dopo che React ha reso il componente. Prova tu stesso: aggiungi la seguente riga da qualche parte nel corpo della tua funzione `Todo()`, sotto dove `editButtonRef` è inizializzato:

```jsx
console.log(editButtonRef.current);
```

Vedrai che il valore di `editButtonRef.current` è `null` quando il componente viene reso per la prima volta, ma se fai clic su un pulsante "Modifica", registrerà l'elemento `<button>` sulla console. Questo perché il riferimento viene popolato solo dopo che il componente è stato reso, e facendo clic sul pulsante "Modifica" provoca il re-render del componente. Assicurati di eliminare questo log prima di procedere.

> [!NOTE]
> I tuoi log appariranno 6 volte perché abbiamo 3 istanze di `<Todo />` nella nostra app e React rende i nostri componenti due volte in sviluppo.

Ci stiamo avvicinando! Per sfruttare i nostri elementi referenziati di recente, abbiamo bisogno di usare un altro hook di React: `useEffect()`.

### Implementazione di `useEffect()`

[`useEffect()`](https://react.dev/reference/react/useEffect) è chiamato così perché esegue qualsiasi effetto collaterale che vogliamo aggiungere al processo di rendering ma che non può essere eseguito all'interno del corpo principale della funzione. `useEffect()` si esegue subito dopo che un componente è stato reso, il che significa che gli elementi DOM che abbiamo referenziato nella sezione precedente saranno disponibili per essere utilizzati.

Cambia l'istruzione di importazione di `Todo.jsx` di nuovo per aggiungere `useEffect`:

```jsx
import { useEffect, useRef, useState } from "react";
```

`useEffect()` prende una funzione come argomento; questa funzione viene eseguita _dopo_ che il componente è stato reso. Per dimostrarlo, metti la seguente chiamata `useEffect()` giusto sopra l'istruzione `return` nel corpo di `Todo()`, e passagli una funzione che registra le parole "effetto collaterale" sulla tua console:

```jsx
useEffect(() => {
  console.log("side effect");
});
```

Per illustrare la differenza tra il processo di rendering principale e il codice eseguito all'interno di `useEffect()`, aggiungi un altro log – metti questo al di sotto della precedente aggiunta:

```jsx
console.log("main render");
```

Ora, apri l'app nel tuo browser. Dovresti vedere entrambi i messaggi nella tua console, con ognuno che si ripete più volte. Nota come "rendering principale" è stato registrato per primo, e "effetto collaterale" è stato registrato per secondo, anche se il log "effetto collaterale" appare per primo nel codice.

```plain
main render                                     Todo.jsx
side effect                                     Todo.jsx
```

Ancora una volta, i log sono ordinati in questo modo perché il codice all'interno di `useEffect()` si esegue _dopo_ che il componente è stato reso. Ciò richiede un po' di tempo per abituarsi, tienilo solo a mente mentre procedi. Per ora, elimina `console.log("rendering principale")` e passeremo alla gestione del nostro focus.

### Focalizzarsi sul nostro campo di modifica

Ora che sappiamo che il nostro hook `useEffect()` funziona, possiamo gestire il focus con esso. Come promemoria, vogliamo focalizzarci sul campo di modifica quando passiamo al modello di modifica.

Aggiorna il tuo hook `useEffect()` esistente in modo che appaia così:

```jsx
useEffect(() => {
  if (isEditing) {
    editFieldRef.current.focus();
  }
}, [isEditing]);
```

Queste modifiche fanno in modo che, se `isEditing` è vero, React legge il valore corrente di `editFieldRef` e vi sposta il focus del browser. Passiamo anche un array a `useEffect()` come secondo argomento. Questo array è un elenco di valori da cui deve dipendere `useEffect()`. Con questi valori inclusi, `useEffect()` verrà eseguito solo quando uno di questi valori cambia. Vogliamo cambiare il focus solo quando il valore di `isEditing` cambia.

Prova ora: usa il tasto <kbd>Tab</kbd> per navigare a uno dei pulsanti "Modifica", poi premi <kbd>Invio</kbd>. Dovresti vedere il componente `<Todo />` passare al suo modello di modifica, e l'indicatore di focus del browser dovrebbe apparire attorno all'elemento `<input>`!

### Spostare il focus sul pulsante di modifica

A prima vista, ottenere che React sposti il focus di nuovo sul nostro pulsante "Modifica" quando la modifica è salvata o annullata sembra ingannevolmente semplice. Sicuramente potremmo aggiungere una condizione al nostro `useEffect` per focalizzarci sul pulsante di modifica se `isEditing` è `false`? Proviamolo ora — aggiorna la chiamata `useEffect()` in questo modo:

```jsx
useEffect(() => {
  if (isEditing) {
    editFieldRef.current.focus();
  } else {
    editButtonRef.current.focus();
  }
}, [isEditing]);
```

Questo funziona in parte. Se usi la tastiera per attivare il pulsante "Modifica" (ricorda: <kbd>Tab</kbd> per arrivarci e premi <kbd>Invio</kbd>), vedrai il tuo focus muoversi tra l'`<input>` di modifica e il pulsante "Modifica" mentre inizi e termini una modifica. Tuttavia, potresti aver notato un nuovo problema — il pulsante "Modifica" nel componente `<Todo />` finale è focalizzato immediatamente al caricamento della pagina prima ancora di interagire con l'app!

Il nostro hook `useEffect()` si comporta esattamente come lo abbiamo progettato: si esegue non appena il componente è reso, vede che `isEditing` è `false`, e focalizza il pulsante "Modifica". Ci sono tre istanze di `<Todo />`, e il focus viene dato al pulsante "Modifica" di quello che viene reso per ultimo.

Dobbiamo ristrutturare il nostro approccio in modo che il focus cambi solo quando `isEditing` cambia da un valore a un altro.

## Gestione del focus più robusta

Per soddisfare i nostri criteri raffinati, dobbiamo sapere non solo il valore di `isEditing`, ma anche _quando quel valore è cambiato_. Per farlo, dobbiamo essere in grado di leggere il valore precedente della costante `isEditing`. Utilizzando pseudocodice, la nostra logica dovrebbe essere qualcosa di simile:

```jsx
if (wasNotEditingBefore && isEditingNow) {
  focusOnEditField();
} else if (wasEditingBefore && isNotEditingNow) {
  focusOnEditButton();
}
```

Il team di React ha discusso delle [modalità per ottenere lo stato precedente di un componente](https://legacy.reactjs.org/docs/hooks-faq.html#how-to-get-the-previous-props-or-state), e ha fornito un esempio di hook che possiamo usare per il lavoro.

### Entra `usePrevious()`

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

`usePrevious()` è un _hook personalizzato_ che traccia un valore attraverso i rendering. Esso:

1. Usa il hook `useRef()` per creare un `ref` vuoto.
2. Restituisce il valore `current` del `ref` al componente che lo ha chiamato.
3. Chiama `useEffect()` e aggiorna il valore memorizzato in `ref.current` dopo ogni rendering del componente chiamante.

Il comportamento di `useEffect()` è fondamentale per questa funzionalità. Poiché `ref.current` viene aggiornato all'interno di una chiamata `useEffect()`, è sempre un passo indietro rispetto al valore che si trova nel ciclo di rendering principale del componente – da qui il nome `usePrevious()`.

### Utilizzare `usePrevious()`

Ora possiamo definire una costante `wasEditing` per tracciare il valore precedente di `isEditing`; questo viene ottenuto chiamando `usePrevious` con `isEditing` come argomento. Aggiungi il seguente all'interno di `Todo()`, sotto le righe `useRef`:

```jsx
const wasEditing = usePrevious(isEditing);
```

Puoi vedere come si comporta `usePrevious()` aggiungendo un log della console sotto questa linea:

```jsx
console.log(wasEditing);
```

In questo log, il valore `current` di `wasEditing` sarà sempre il valore precedente di `isEditing`. Fai clic sul pulsante "Modifica" e sul pulsante "Annulla" alcune volte per osservarlo cambiare, poi elimina questo log quando sei pronto per procedere.

Con questa costante `wasEditing`, possiamo aggiornare il nostro hook `useEffect()` per implementare il pseudocodice di cui abbiamo discusso prima:

```jsx
useEffect(() => {
  if (!wasEditing && isEditing) {
    editFieldRef.current.focus();
  } else if (wasEditing && !isEditing) {
    editButtonRef.current.focus();
  }
}, [wasEditing, isEditing]);
```

Nota che la logica di `useEffect()` ora dipende da `wasEditing`, quindi lo forniamo nell'array delle dipendenze.

Prova a usare la tua tastiera per attivare i pulsanti "Modifica" e "Annulla" nel componente `<Todo />`; vedrai l'indicatore di focus del browser muoversi in modo appropriato, senza il problema di cui abbiamo discusso all'inizio di questa sezione.

## Focalizzarsi quando l'utente elimina un compito

C'è ancora una lacuna nell'esperienza da tastiera: quando un utente elimina un compito dall'elenco, il focus scompare. Seguiremo un modello simile alle nostre modifiche precedenti: creeremo un nuovo riferimento e utilizzeremo il nostro hook `usePrevious()`, in modo da poter focalizzarci sull'intestazione dell'elenco ogni volta che un utente elimina un compito.

### Perché l'intestazione dell'elenco?

A volte, il luogo in cui vogliamo inviare il nostro focus è ovvio: quando abbiamo cambiato i nostri modelli `<Todo />`, avevamo un punto d'origine su cui "tornare" — il pulsante "Modifica". In questo caso, tuttavia, dato che stiamo completamente rimuovendo gli elementi dal DOM, non abbiamo un luogo in cui tornare. La prossima cosa migliore è una posizione intuitiva nelle vicinanze. L'intestazione dell'elenco è la nostra scelta migliore perché è vicina all'elemento dell'elenco che l'utente eliminerà, e concentrandoci su di essa indicheremo all'utente quanti compiti sono rimasti.

### Creazione del nostro riferimento

Importa i hook `useRef()` e `useEffect()` in `App.jsx` — avrai bisogno di entrambi qui sotto:

```jsx
import { useState, useRef, useEffect } from "react";
```

Successivamente, dichiara un nuovo riferimento all'interno della funzione `App()`, appena sopra l'istruzione `return`:

```jsx
const listHeadingRef = useRef(null);
```

### Preparare l'intestazione

Gli elementi di intestazione come il nostro `<h2>` di solito non sono focalizzabili. Questo non è un problema — possiamo rendere qualsiasi elemento programmabilmente focalizzabile aggiungendo l'attributo [`tabindex="-1"`](/it/docs/Web/HTML/Reference/Global_attributes/tabindex) ad esso. Questo significa _solo focalizzabile con JavaScript_. Non puoi premere <kbd>Tab</kbd> per focalizzarti su un elemento con un tabindex di `-1` nello stesso modo in cui potresti fare con un elemento [`<button>`](/it/docs/Web/HTML/Reference/Elements/button) o [`<a>`](/it/docs/Web/HTML/Reference/Elements/a) (questo può essere fatto utilizzando `tabindex="0"`, ma non è appropriato in questo caso).

Aggiungiamo l'attributo `tabindex` — scritto come `tabIndex` in JSX — all'intestazione sopra il nostro elenco di compiti, insieme al nostro `listHeadingRef`:

```jsx
<h2 id="list-heading" tabIndex="-1" ref={listHeadingRef}>
  {headingText}
</h2>
```

> [!NOTE]
> L'attributo `tabindex` è eccellente per i casi limite di accessibilità, ma dovreste curare **con grande attenzione** di non abusarne. Applicate un `tabindex` a un elemento solo quando siete sicuri che renderlo focalizzabile porterà qualche beneficio all'utente. Nella maggior parte dei casi, dovreste utilizzare elementi che possono naturalmente prendere il focus, come pulsanti, ancoraggi e input. Un uso irresponsabile di `tabindex` potrebbe avere un impatto profondamente negativo sugli utenti della tastiera e dei lettori di schermo!

### Ottenere lo stato precedente

Vogliamo concentrarci sull'elemento associato al nostro riferimento (tramite l'attributo `ref`) solo quando il nostro utente elimina un compito dalla sua lista. Questo richiederà il hook `usePrevious()` che abbiamo utilizzato in precedenza. Aggiungilo nella parte superiore del tuo file `App.jsx`, appena sotto le importazioni:

```jsx
function usePrevious(value) {
  const ref = useRef();
  useEffect(() => {
    ref.current = value;
  });
  return ref.current;
}
```

Ora aggiungi il seguente codice, sopra l'istruzione `return` all'interno della funzione `App()`:

```jsx
const prevTaskLength = usePrevious(tasks.length);
```

Qui stiamo invocando `usePrevious()` per tracciare la lunghezza precedente della matrice dei compiti.

> [!NOTE]
> Poiché ora stiamo utilizzando `usePrevious()` in due file, potrebbe essere più efficiente spostare la funzione `usePrevious()` in un proprio file, esportarla da quel file e importarla dove necessario. Prova a farlo come esercizio una volta raggiunta la fine.

### Utilizzare `useEffect()` per controllare il focus dell'intestazione

Ora che abbiamo memorizzato quanti compiti avevamo in precedenza, possiamo impostare un hook `useEffect()` per eseguire quando il nostro numero di compiti cambia, che focalizzerà l'intestazione se il numero di compiti che abbiamo ora è inferiore a quello che era in precedenza — cioè, abbiamo eliminato un compito!

Aggiungi il seguente codice nel corpo della tua funzione `App()`, proprio sotto le tue aggiunte precedenti:

```jsx
useEffect(() => {
  if (tasks.length < prevTaskLength) {
    listHeadingRef.current.focus();
  }
}, [tasks.length, prevTaskLength]);
```

Cerchiamo solo di focalizzarci sulla nostra intestazione dell'elenco se abbiamo meno compiti ora rispetto a prima. Le dipendenze passate in questo hook assicurano che tenterà di eseguire nuovamente solo quando uno di quei valori (il numero di compiti attuali o il numero di compiti precedenti) cambiano.

Ora, quando usi la tua tastiera per eliminare un compito nel tuo browser, vedrai il nostro contorno di focus tratteggiato apparire attorno all'intestazione sopra l'elenco.

## Finito!

Hai appena finito di costruire un'app React dal principio! Congratulazioni! Le competenze che hai appreso qui saranno una grande base su cui costruire mentre continui a lavorare con React.

Nella maggior parte dei casi, puoi essere un contributore efficace a un progetto React anche se tutto ciò che fai è pensare attentamente ai componenti e al loro stato e props. Ricorda sempre di scrivere il miglior HTML possibile.

`useRef()` e `useEffect()` sono funzionalità piuttosto avanzate, e dovresti essere orgoglioso di te stesso per averle utilizzate! Cerca opportunità per esercitarle di più, perché farlo ti permetterà di creare esperienze inclusive per gli utenti. Ricorda: la nostra app non sarebbe stata accessibile agli utenti della tastiera senza di esse!

> [!NOTE]
> Se hai bisogno di confrontare il tuo codice con la nostra versione, puoi trovare una versione finita del codice dell'app React di esempio nel nostro [repository todo-react](https://github.com/mdn/todo-react). Per una versione in esecuzione dal vivo, vedi <https://mdn.github.io/todo-react/>.

Nell'ultimo articolo ti presenteremo un elenco di risorse React che puoi utilizzare per proseguire il tuo apprendimento.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/React_interactivity_filtering_conditional_rendering","Learn_web_development/Core/Frameworks_libraries/React_resources", "Learn_web_development/Core/Frameworks_libraries")}}
