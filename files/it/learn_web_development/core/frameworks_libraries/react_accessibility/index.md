---
title: Accessibilità in React
short-title: Accessibilità in React
slug: Learn_web_development/Core/Frameworks_libraries/React_accessibility
l10n:
  sourceCommit: 77ea71add6054857698eb7ac1bfec8c7afe9ad4f
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/React_interactivity_filtering_conditional_rendering","Learn_web_development/Core/Frameworks_libraries/React_resources", "Learn_web_development/Core/Frameworks_libraries")}}

Nel nostro ultimo articolo del tutorial, ci concentreremo (gioco di parole voluto) sull'accessibilità, inclusa la gestione del focus in React, che può migliorare l'usabilità e ridurre la confusione sia per gli utenti che usano solo la tastiera sia per quelli che utilizzano lettori di schermo.

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
      <td>Implementare l'accessibilità tramite tastiera in React.</td>
    </tr>
  </tbody>
</table>

## Includere gli utenti della tastiera

A questo punto, sono state implementate tutte le funzionalità che ci eravamo prefissati di implementare. Gli utenti possono aggiungere una nuova attività, selezionare e deselezionare le attività, eliminare attività o modificarne i nomi. Possono inoltre filtrare l'elenco delle attività in base a tutte le attività, quelle attive o quelle completate.

O, almeno, possono fare tutte queste cose con un mouse. Purtroppo, queste funzionalità non sono molto accessibili per gli utenti che usano solo la tastiera. Esploriamo ora questo aspetto.

## Esplorare il problema dell'usabilità tramite tastiera

Iniziare facendo clic sull'input nella parte superiore dell'app, come se si stesse per aggiungere una nuova attività. Attorno a quell'input verrà visualizzato un contorno spesso e tratteggiato. Questo contorno è l'indicatore visivo che segnala che il browser ha attualmente il focus su questo elemento. Premere il tasto <kbd>Tab</kbd> e il contorno apparirà attorno al pulsante "Add" sotto l'input. Questo mostra che il focus del browser si è spostato.

Premere <kbd>Tab</kbd> ancora alcune volte e questo indicatore di focus tratteggiato si sposterà tra ciascuno dei pulsanti filtro. Continuare finché l'indicatore di focus non si trova attorno al primo pulsante "Edit". Premere <kbd>Enter</kbd>.

Il componente `<Todo />` cambierà template, come progettato, e verrà visualizzato un modulo che consente di modificare il nome dell'attività.

Ma dove è finito l'indicatore di focus?

Quando si passa da un template all'altro nel componente `<Todo />`, gli elementi del vecchio template vengono completamente rimossi e sostituiti con gli elementi del nuovo template. Ciò significa che l'elemento su cui era presente il focus non esiste più, quindi non vi è alcun segnale visivo che indichi dove si trova il focus del browser. Questo potrebbe confondere un'ampia varietà di utenti, in particolare quelli che dipendono dalla tastiera o che usano tecnologie assistive.

Per migliorare l'esperienza degli utenti della tastiera e delle tecnologie assistive, è opportuno gestire autonomamente il focus del browser.

### A parte: una nota sull'indicatore di focus

Facendo clic con il mouse sui pulsanti filtro "All", "Active" o "Completed", _non_ verrà visualizzato un indicatore di focus, ma questo apparirà spostandosi tra essi con il tasto <kbd>Tab</kbd> della tastiera. Non è un problema: il codice non è danneggiato!

Il file CSS usa la pseudo-classe {{cssxref(":focus-visible")}} per fornire uno stile personalizzato all'indicatore di focus e il browser usa un insieme di regole interne per determinare quando mostrarlo all'utente. In generale, il browser _mostrerà_ un indicatore di focus in risposta all'input da tastiera e _potrebbe_ mostrarlo in risposta all'input del mouse. Gli elementi `<button>` _non_ mostrano un indicatore di focus in risposta all'input del mouse, mentre gli elementi `<input>` _lo_ mostrano.

Il comportamento di `:focus-visible` è più selettivo rispetto alla più vecchia pseudo-classe {{cssxref(":focus")}}, con la quale potrebbe esserci maggiore familiarità. `:focus` mostra un indicatore di focus in molte più situazioni e può essere usata al posto di, o in combinazione con, `:focus-visible`, se preferito.

## Spostare il focus tra i template

Quando un utente cambia il template di `<Todo />` dalla visualizzazione alla modifica, è opportuno impostare il focus sull'`<input>` usato per rinominarlo; quando torna dalla modifica alla visualizzazione, il focus dovrebbe ritornare al pulsante "Edit".

### Selezionare gli elementi

Fino a questo punto, sono stati scritti componenti JSX lasciando che React costruisse il DOM risultante dietro le quinte. Nella maggior parte dei casi, non è necessario selezionare elementi specifici nel DOM perché è possibile usare state e props di React per controllare ciò che viene renderizzato. Per gestire il focus, tuttavia, è necessario poter selezionare elementi DOM specifici.

Qui entra in gioco l'hook `useRef()`.

Per prima cosa, modificare l'istruzione `import` nella parte superiore di `Todo.jsx` in modo che includa `useRef`:

```jsx
import { useRef, useState } from "react";
```

`useRef()` crea un oggetto con una singola proprietà: `current`. Le ref possono memorizzare qualsiasi valore desiderato ed è possibile recuperare tali valori in seguito. Possono persino memorizzare riferimenti a elementi DOM, che è esattamente ciò che verrà fatto qui.

Successivamente, creare due nuove costanti sotto gli hook `useState()` nella funzione `Todo()`. Ciascuna dovrebbe essere una ref: una per il pulsante "Edit" nel template di visualizzazione e una per il campo di modifica nel template di modifica.

```jsx
const editFieldRef = useRef(null);
const editButtonRef = useRef(null);
```

Queste ref hanno un valore predefinito di `null` per chiarire che saranno vuote finché non verranno collegate ai rispettivi elementi DOM. Per collegarle ai loro elementi, verrà aggiunto a ogni elemento JSX lo speciale attributo `ref`, impostando i valori di tali attributi sugli oggetti `ref` con il nome appropriato.

Aggiornare l'`<input>` nel template di modifica affinché sia così:

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

Aggiornare il pulsante "Edit" nel template di visualizzazione affinché sia così:

```jsx
<button
  type="button"
  className="btn"
  onClick={() => setEditing(true)}
  ref={editButtonRef}>
  Edit <span className="visually-hidden">{props.name}</span>
</button>
```

Questa operazione popolerà `editFieldRef` e `editButtonRef` con riferimenti agli elementi DOM ai quali sono collegati, ma _solo_ dopo che React ha renderizzato il componente. Verificarlo personalmente: aggiungere la riga seguente in un punto qualsiasi del corpo della funzione `Todo()`, sotto il punto in cui viene inizializzato `editButtonRef`:

```jsx
console.log(editButtonRef.current);
```

Il valore di `editButtonRef.current` sarà `null` quando il componente viene renderizzato per la prima volta, ma facendo clic su un pulsante "Edit" verrà registrato l'elemento `<button>` nella console. Questo accade perché la ref viene popolata solo dopo il rendering del componente e facendo clic sul pulsante "Edit" il componente viene renderizzato nuovamente. Assicurarsi di eliminare questo log prima di proseguire.

> [!NOTE]
> I log appariranno 6 volte perché nell'app sono presenti 3 istanze di `<Todo />` e React renderizza i componenti due volte durante lo sviluppo.

Ci si sta avvicinando! Per sfruttare i nuovi elementi referenziati, è necessario usare un altro hook di React: `useEffect()`.

### Implementare `useEffect()`

[`useEffect()`](https://react.dev/reference/react/useEffect) ha questo nome perché esegue qualsiasi effetto collaterale che si desidera aggiungere al processo di rendering ma che non può essere eseguito all'interno del corpo della funzione principale. `useEffect()` viene eseguito subito dopo il rendering di un componente, quindi gli elementi DOM referenziati nella sezione precedente saranno disponibili per l'uso.

Modificare nuovamente l'istruzione import di `Todo.jsx` per aggiungere `useEffect`:

```jsx
import { useEffect, useRef, useState } from "react";
```

`useEffect()` accetta una funzione come argomento; questa funzione viene eseguita _dopo_ il rendering del componente. Per dimostrarlo, inserire la seguente chiamata a `useEffect()` subito sopra l'istruzione `return` nel corpo di `Todo()` e passarle una funzione che registra le parole "side effect" nella console:

```jsx
useEffect(() => {
  console.log("side effect");
});
```

Per illustrare la differenza tra il processo di rendering principale e il codice eseguito all'interno di `useEffect()`, aggiungere un altro log, sotto l'aggiunta precedente:

```jsx
console.log("main render");
```

Ora, aprire l'app nel browser. Nella console dovrebbero apparire entrambi i messaggi, ciascuno ripetuto più volte. Notare che "main render" viene registrato per primo e "side effect" per secondo, anche se il log di "side effect" appare per primo nel codice.

```plain
main render                                     Todo.jsx
side effect                                     Todo.jsx
```

Anche in questo caso, i log sono ordinati in questo modo perché il codice all'interno di `useEffect()` viene eseguito _dopo_ il rendering del componente. Richiede un po' di abitudine, ma è bene tenerlo a mente mentre si prosegue. Per ora, eliminare `console.log("main render")` e passare all'implementazione della gestione del focus.

### Impostare il focus sul campo di modifica

Ora che è noto il funzionamento dell'hook `useEffect()`, è possibile gestire il focus con esso. Come promemoria, è necessario impostare il focus sul campo di modifica quando si passa al template di modifica.

Aggiornare l'hook `useEffect()` esistente affinché sia così:

```jsx
useEffect(() => {
  if (isEditing) {
    editFieldRef.current.focus();
  }
}, [isEditing]);
```

Queste modifiche fanno sì che, se `isEditing` è true, React legga il valore corrente di `editFieldRef` e vi sposti il focus del browser. Viene inoltre passato un array a `useEffect()` come secondo argomento. Questo array è un elenco di valori da cui `useEffect()` dovrebbe dipendere. Con questi valori inclusi, `useEffect()` verrà eseguito solo quando uno di essi cambia. È necessario cambiare il focus solo quando cambia il valore di `isEditing`.

Provarlo ora: usare il tasto <kbd>Tab</kbd> per raggiungere uno dei pulsanti "Edit", quindi premere <kbd>Enter</kbd>. Il componente `<Todo />` dovrebbe passare al template di modifica e l'indicatore di focus del browser dovrebbe apparire attorno all'elemento `<input>`!

### Riportare il focus al pulsante di modifica

A prima vista, fare in modo che React riporti il focus al pulsante "Edit" quando la modifica viene salvata o annullata sembra ingannevolmente facile. Non basterebbe aggiungere una condizione a `useEffect` per impostare il focus sul pulsante di modifica se `isEditing` è `false`? Proviamolo ora: aggiornare la chiamata a `useEffect()` come segue:

```jsx
useEffect(() => {
  if (isEditing) {
    editFieldRef.current.focus();
  } else {
    editButtonRef.current.focus();
  }
}, [isEditing]);
```

Funziona in parte. Usando la tastiera per attivare il pulsante "Edit" (ricordare: raggiungerlo con <kbd>Tab</kbd> e premere <kbd>Enter</kbd>), il focus si sposterà tra l'`<input>` di modifica e il pulsante "Edit" quando si inizia e si termina una modifica. Tuttavia, potrebbe essere comparso un nuovo problema: il pulsante "Edit" nel componente `<Todo />` finale riceve il focus immediatamente al caricamento della pagina, prima ancora di interagire con l'app!

L'hook `useEffect()` si sta comportando esattamente come progettato: viene eseguito non appena il componente viene renderizzato, rileva che `isEditing` è `false` e imposta il focus sul pulsante "Edit". Esistono tre istanze di `<Todo />` e il focus viene assegnato al pulsante "Edit" dell'ultima istanza renderizzata.

È necessario ristrutturare l'approccio affinché il focus cambi solo quando `isEditing` passa da un valore a un altro.

## Una gestione del focus più robusta

Per soddisfare i criteri più precisi, è necessario conoscere non solo il valore di `isEditing`, ma anche _quando tale valore è cambiato_. Per farlo, occorre poter leggere il valore precedente della costante `isEditing`. Usando pseudocodice, la logica dovrebbe essere simile a questa:

```jsx
if (wasNotEditingBefore && isEditingNow) {
  focusOnEditField();
} else if (wasEditingBefore && isNotEditingNow) {
  focusOnEditButton();
}
```

Il team di React ha discusso dei [modi per ottenere lo state precedente di un componente](https://legacy.reactjs.org/docs/hooks-faq.html#how-to-get-the-previous-props-or-state) e ha fornito un hook di esempio che può essere usato per questo scopo.

### Ecco `usePrevious()`

Incollare il codice seguente vicino alla parte superiore di `Todo.jsx`, sopra la funzione `Todo()`.

```jsx
function usePrevious(value) {
  const ref = useRef();
  useEffect(() => {
    ref.current = value;
  });
  return ref.current;
}
```

`usePrevious()` è un _custom hook_ che tiene traccia di un valore attraverso i rendering. Esso:

1. Usa l'hook `useRef()` per creare una `ref` vuota.
2. Restituisce il valore `current` della `ref` al componente che lo ha chiamato.
3. Chiama `useEffect()` e aggiorna il valore memorizzato in `ref.current` dopo ogni rendering del componente chiamante.

Il comportamento di `useEffect()` è fondamentale per questa funzionalità. Poiché `ref.current` viene aggiornato all'interno di una chiamata a `useEffect()`, è sempre un passo indietro rispetto al valore presente nel ciclo di rendering principale del componente: da qui il nome `usePrevious()`.

### Usare `usePrevious()`

Ora è possibile definire una costante `wasEditing` per tenere traccia del valore precedente di `isEditing`; ciò si ottiene chiamando `usePrevious` con `isEditing` come argomento. Aggiungere quanto segue all'interno di `Todo()`, sotto le righe di `useRef`:

```jsx
const wasEditing = usePrevious(isEditing);
```

È possibile osservare il comportamento di `usePrevious()` aggiungendo un log nella console sotto questa riga:

```jsx
console.log(wasEditing);
```

In questo log, il valore `current` di `wasEditing` sarà sempre il valore precedente di `isEditing`. Fare clic sui pulsanti "Edit" e "Cancel" alcune volte per osservare il cambiamento, quindi eliminare questo log quando si è pronti a proseguire.

Con questa costante `wasEditing`, è possibile aggiornare l'hook `useEffect()` per implementare lo pseudocodice discusso in precedenza:

```jsx
useEffect(() => {
  if (!wasEditing && isEditing) {
    editFieldRef.current.focus();
  } else if (wasEditing && !isEditing) {
    editButtonRef.current.focus();
  }
}, [wasEditing, isEditing]);
```

Notare che la logica di `useEffect()` ora dipende da `wasEditing`, pertanto viene fornita nell'array delle dipendenze.

Provare a usare la tastiera per attivare i pulsanti "Edit" e "Cancel" nel componente `<Todo />`; l'indicatore di focus del browser si sposterà in modo appropriato, senza il problema discusso all'inizio di questa sezione.

## Impostare il focus quando l'utente elimina un'attività

Rimane un ultimo problema nell'esperienza tramite tastiera: quando un utente elimina un'attività dall'elenco, il focus scompare. Verrà seguito uno schema simile alle modifiche precedenti: verrà creata una nuova ref e verrà utilizzato l'hook `usePrevious()` per impostare il focus sull'intestazione dell'elenco ogni volta che un utente elimina un'attività.

### Perché l'intestazione dell'elenco?

A volte, il punto in cui inviare il focus è ovvio: quando sono stati alternati i template di `<Todo />`, c'era un punto di origine a cui "tornare", ovvero il pulsante "Edit". In questo caso, invece, poiché gli elementi vengono completamente rimossi dal DOM, non esiste un punto a cui tornare. La scelta migliore successiva è una posizione intuitiva nelle vicinanze. L'intestazione dell'elenco è la scelta migliore perché è vicina all'elemento dell'elenco che l'utente eliminerà e il focus su di essa comunicherà all'utente quante attività restano.

### Creare la ref

Importare gli hook `useRef()` e `useEffect()` in `App.jsx`: saranno necessari entrambi più avanti:

```jsx
import { useState, useRef, useEffect } from "react";
```

Successivamente, dichiarare una nuova ref all'interno della funzione `App()`, appena sopra l'istruzione `return`:

```jsx
const listHeadingRef = useRef(null);
```

### Preparare l'intestazione

Gli elementi di intestazione come `<h2>` solitamente non sono selezionabili tramite focus. Questo non è un problema: è possibile rendere qualsiasi elemento selezionabile programmaticamente aggiungendovi l'attributo [`tabindex="-1"`](/it/docs/Web/HTML/Reference/Global_attributes/tabindex). Ciò significa _selezionabile solo con JavaScript_. Non è possibile premere <kbd>Tab</kbd> per impostare il focus su un elemento con tabindex `-1` come si potrebbe fare con un elemento [`<button>`](/it/docs/Web/HTML/Reference/Elements/button) o [`<a>`](/it/docs/Web/HTML/Reference/Elements/a) (questo può essere fatto usando `tabindex="0"`, ma non è appropriato in questo caso).

Aggiungere l'attributo `tabindex`, scritto come `tabIndex` in JSX, all'intestazione sopra l'elenco delle attività, insieme a `listHeadingRef`:

```jsx
<h2 id="list-heading" tabIndex="-1" ref={listHeadingRef}>
  {headingText}
</h2>
```

> [!NOTE]
> L'attributo `tabindex` è eccellente per i casi limite di accessibilità, ma occorre prestare **molta attenzione** a non abusarne. Applicare un `tabindex` a un elemento solo quando si è certi che renderlo selezionabile tramite focus porterà un beneficio all'utente. Nella maggior parte dei casi, è opportuno utilizzare elementi che possono ricevere naturalmente il focus, come pulsanti, ancore e input. Un uso irresponsabile di `tabindex` potrebbe avere un impatto profondamente negativo sugli utenti della tastiera e dei lettori di schermo!

### Ottenere lo state precedente

È necessario impostare il focus sull'elemento associato alla ref (tramite l'attributo `ref`) solo quando l'utente elimina un'attività dal proprio elenco. Ciò richiederà l'hook `usePrevious()` usato in precedenza. Aggiungerlo nella parte superiore del file `App.jsx`, subito sotto gli import:

```jsx
function usePrevious(value) {
  const ref = useRef();
  useEffect(() => {
    ref.current = value;
  });
  return ref.current;
}
```

Ora aggiungere quanto segue sopra l'istruzione `return` all'interno della funzione `App()`:

```jsx
const prevTaskLength = usePrevious(tasks.length);
```

Qui viene invocato `usePrevious()` per tenere traccia della lunghezza precedente dell'array delle attività.

> [!NOTE]
> Poiché ora viene utilizzato `usePrevious()` in due file, potrebbe essere più efficiente spostare la funzione `usePrevious()` nel proprio file, esportarla da quel file e importarla dove necessario. Provare a farlo come esercizio dopo aver raggiunto la fine.

### Usare `useEffect()` per controllare il focus dell'intestazione

Ora che è stato memorizzato il numero di attività presenti in precedenza, è possibile configurare un hook `useEffect()` da eseguire quando cambia il numero di attività. L'hook imposterà il focus sull'intestazione se il numero attuale di attività è inferiore a quello precedente, cioè se è stata eliminata un'attività.

Aggiungere quanto segue nel corpo della funzione `App()`, subito sotto le aggiunte precedenti:

```jsx
useEffect(() => {
  if (tasks.length < prevTaskLength) {
    listHeadingRef.current.focus();
  }
}, [tasks.length, prevTaskLength]);
```

Si tenta di impostare il focus sull'intestazione dell'elenco solo se ora ci sono meno attività rispetto a prima. Le dipendenze passate a questo hook garantiscono che tenti di essere rieseguito solo quando cambia uno di questi valori, ovvero il numero delle attività attuali o quello delle attività precedenti.

Ora, usando la tastiera per eliminare un'attività nel browser, il contorno di focus tratteggiato apparirà attorno all'intestazione sopra l'elenco.

## Finito!

È stata appena completata la creazione di un'app React da zero! Congratulazioni! Le competenze apprese qui costituiranno un'ottima base su cui costruire mentre si continua a lavorare con React.

Nella maggior parte dei casi, è possibile contribuire efficacemente a un progetto React anche limitandosi a riflettere attentamente sui componenti, sul loro state e sulle loro props. Ricordare di scrivere sempre il miglior HTML possibile.

`useRef()` e `useEffect()` sono funzionalità piuttosto avanzate, ed è motivo di soddisfazione averle usate! Cercare ulteriori occasioni per esercitarsi con esse, perché questo consentirà di creare esperienze inclusive per gli utenti. Ricordare: senza di esse, l'app non sarebbe stata accessibile agli utenti della tastiera!

> [!NOTE]
> Per confrontare il codice con la nostra versione, è disponibile una versione completata del codice dell'app React di esempio nel [repository todo-react](https://github.com/mdn/todo-react). Per una versione live in esecuzione, vedere <https://mdn.github.io/todo-react/>.

Nell'ultimo articolo verrà presentato un elenco di risorse su React che possono essere usate per approfondire l'apprendimento.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/React_interactivity_filtering_conditional_rendering","Learn_web_development/Core/Frameworks_libraries/React_resources", "Learn_web_development/Core/Frameworks_libraries")}}
