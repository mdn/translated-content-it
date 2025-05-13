---
title: Risolvere problemi comuni con JavaScript
short-title: Problemi comuni in JavaScript
slug: Learn_web_development/Howto/Solve_JavaScript_problems
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

I seguenti link puntano a soluzioni per problemi comuni che potrebbe incontrare quando si scrive JavaScript.

## Errori comuni dei principianti

### Ortografia e maiuscole corrette

Se il suo codice non funziona e/o il browser lamenta che qualcosa è indefinito, verifichi di aver scritto correttamente tutti i nomi delle variabili, delle funzioni, ecc.

Alcune funzioni integrate del browser che causano problemi comuni sono:

| Corretto                   | Errato                     |
|----------------------------|----------------------------|
| `getElementsByTagName()`   | `getElementByTagName()`    |
| `getElementsByName()`      | `getElementByName()`       |
| `getElementsByClassName()` | `getElementByClassName()`  |
| `getElementById()`         | `getElementsById()`        |

### Posizione del punto e virgola

Deve assicurarsi di non posizionare nessun punto e virgola in modo errato. Per esempio:

| Corretto                      | Errato                         |
|-------------------------------|------------------------------- |
| `elem.style.color = 'red';`   | `elem.style.color = 'red;'`    |

### Funzioni

Ci sono diverse cose che possono andare storte con le funzioni.

Uno degli errori più comuni è dichiarare la funzione, ma non chiamarla in nessun punto. Per esempio:

```js
function myFunction() {
  alert("This is my function.");
}
```

Questo codice non farà nulla a meno che non lo chiami con la seguente istruzione:

```js
myFunction();
```

#### Ambito della funzione

Ricordi che [le funzioni hanno un proprio ambito](/it/docs/Learn_web_development/Core/Scripting/Functions#function_scope_and_conflicts) — non può accedere a un valore di variabile impostato all'interno di una funzione dall'esterno della funzione, a meno che non dichiari la variabile globalmente (cioè, non all'interno di nessuna funzione), o [restituisca il valore](/it/docs/Learn_web_development/Core/Scripting/Return_values) dalla funzione.

#### Esecuzione di codice dopo un'istruzione return

Ricordi anche che quando restituisce da una funzione, l'interprete JavaScript esce dalla funzione — nessun codice dopo l'istruzione return verrà eseguito.

Infatti, alcuni browser (come Firefox) le daranno un messaggio di errore nella console degli sviluppatori se ha del codice dopo un'istruzione return. Firefox le mostrerà "codice non raggiungibile dopo l'istruzione return".

### Notazione a oggetti versus assegnazione normale

Quando assegna normalmente qualcosa in JavaScript, utilizza un singolo segno di uguale, ad esempio:

```js
const myNumber = 0;
```

Con [gli oggetti](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects), tuttavia, deve prestare attenzione a utilizzare la sintassi corretta. L'oggetto deve essere racchiuso tra parentesi graffe, i nomi dei membri devono essere separati dai loro valori utilizzando i due punti e i membri devono essere separati da virgole. Ad esempio:

```js
const myObject = {
  name: "Chris",
  age: 38,
};
```

## Definizioni di base

- [Cos'è JavaScript?](/it/docs/Learn_web_development/Core/Scripting/What_is_JavaScript#a_high-level_definition)
- [Cos'è una variabile?](/it/docs/Learn_web_development/Core/Scripting/Variables#what_is_a_variable)
- [Cosa sono le stringhe?](/it/docs/Learn_web_development/Core/Scripting/Strings)
- [Cos'è un array?](/it/docs/Learn_web_development/Core/Scripting/Arrays#what_is_an_array)
- [Cos'è un ciclo?](/it/docs/Learn_web_development/Core/Scripting/Loops)
- [Cos'è una funzione?](/it/docs/Learn_web_development/Core/Scripting/Functions)
- [Cos'è un evento?](/it/docs/Learn_web_development/Core/Scripting/Events)
- [Cos'è un oggetto?](/it/docs/Learn_web_development/Core/Scripting/Object_basics#object_basics)
- [Cos'è JSON?](/it/docs/Learn_web_development/Core/Scripting/JSON#no_really_what_is_json)
- [Cos'è un'API web?](/it/docs/Learn_web_development/Extensions/Client-side_APIs/Introduction#what_are_apis)
- [Cos'è il DOM?](/it/docs/Learn_web_development/Core/Scripting/DOM_scripting#the_document_object_model)

## Casi d'uso di base

### Generale

- [Come aggiungere JavaScript alla sua pagina?](/it/docs/Learn_web_development/Core/Scripting/What_is_JavaScript#how_do_you_add_javascript_to_your_page)
- [Come aggiungere commenti al codice JavaScript?](/it/docs/Learn_web_development/Core/Scripting/What_is_JavaScript#comments)

### Variabili

- [Come dichiarare una variabile?](/it/docs/Learn_web_development/Core/Scripting/Variables#declaring_a_variable)
- [Come inizializzare una variabile con un valore?](/it/docs/Learn_web_development/Core/Scripting/Variables#initializing_a_variable)
- [Come aggiornare il valore di una variabile?](/it/docs/Learn_web_development/Core/Scripting/Variables#updating_a_variable) (vedi anche [Operatori di assegnazione](/it/docs/Learn_web_development/Core/Scripting/Math#assignment_operators))
- [Quali tipi di dati possono avere valori in JavaScript?](/it/docs/Learn_web_development/Core/Scripting/Variables#variable_types)
- [Cosa significa 'tipizzazione debole'?](/it/docs/Learn_web_development/Core/Scripting/Variables#dynamic_typing)

### Matematica

- [Quali tipi di numeri deve trattare nello sviluppo web?](/it/docs/Learn_web_development/Core/Scripting/Math#types_of_numbers)
- [Come fare matematica di base in JavaScript?](/it/docs/Learn_web_development/Core/Scripting/Math#arithmetic_operators)
- [Cos'è la precedenza degli operatori e come è gestita in JavaScript?](/it/docs/Learn_web_development/Core/Scripting/Math#operator_precedence)
- [Come incrementare e decrementare valori in JavaScript?](/it/docs/Learn_web_development/Core/Scripting/Math#increment_and_decrement_operators)
- [Come confrontare valori in JavaScript?](/it/docs/Learn_web_development/Core/Scripting/Math#comparison_operators) (ad esempio, per vedere quale è più grande o per vedere se un valore è uguale a un altro).

### Stringhe

- [Come creare una stringa in JavaScript?](/it/docs/Learn_web_development/Core/Scripting/Strings#declaring_strings)
- [Deve utilizzare virgolette singole o doppie?](/it/docs/Learn_web_development/Core/Scripting/Strings#single_quotes_double_quotes_and_backticks)
- [Come unire le stringhe tra loro?](/it/docs/Learn_web_development/Core/Scripting/Strings#concatenation_in_context)
- [Può unire stringhe e numeri tra loro?](/it/docs/Learn_web_development/Core/Scripting/Strings#numbers_vs._strings)
- [Come trovare la lunghezza di una stringa?](/it/docs/Learn_web_development/Core/Scripting/Useful_string_methods#finding_the_length_of_a_string)
- [Come trovare quale carattere si trova in una certa posizione in una stringa?](/it/docs/Learn_web_development/Core/Scripting/Useful_string_methods#retrieving_a_specific_string_character)
- [Come trovare ed estrarre una sottostringa specifica da una stringa?](/it/docs/Learn_web_development/Core/Scripting/Useful_string_methods#extracting_a_substring_from_a_string)
- [Come cambiare il case di una stringa?](/it/docs/Learn_web_development/Core/Scripting/Useful_string_methods#changing_case)
- [Come sostituire una sottostringa specifica con un'altra?](/it/docs/Learn_web_development/Core/Scripting/Useful_string_methods#updating_parts_of_a_string)

### Array

- [Come creare un array?](/it/docs/Learn_web_development/Core/Scripting/Arrays#creating_arrays)
- [Come accedere e modificare gli elementi in un array?](/it/docs/Learn_web_development/Core/Scripting/Arrays#accessing_and_modifying_array_items) (questo include array multidimensionali)
- [Come trovare la lunghezza di un array?](/it/docs/Learn_web_development/Core/Scripting/Arrays#finding_the_length_of_an_array)
- [Come aggiungere elementi a un array?](/it/docs/Learn_web_development/Core/Scripting/Arrays#adding_items)
- [Come rimuovere elementi da un array?](/it/docs/Learn_web_development/Core/Scripting/Arrays#removing_items)
- [Come dividere una stringa in elementi di un array, o unire elementi di un array in una stringa?](/it/docs/Learn_web_development/Core/Scripting/Arrays#converting_between_strings_and_arrays)

### Debugging JavaScript

- [Quali sono i tipi di errore di base?](/it/docs/Learn_web_development/Core/Scripting/What_went_wrong#types_of_error)
- [Cosa sono gli strumenti per sviluppatori del browser e come accedervi?](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools)
- [Come registrare un valore nella console di JavaScript?](/it/docs/Learn_web_development/Core/Scripting/Debugging_JavaScript#the_console_api)
- [Come utilizzare i punti di interruzione e altre funzionalità di debugging in JavaScript?](/it/docs/Learn_web_development/Core/Scripting/Debugging_JavaScript#using_the_javascript_debugger)

Per ulteriori informazioni sul debugging in JavaScript, veda [Debugging e gestione degli errori in JavaScript](/it/docs/Learn_web_development/Core/Scripting/Debugging_JavaScript). Inoltre, consulti [Altri errori comuni](/it/docs/Learn_web_development/Core/Scripting/What_went_wrong#other_common_errors) per una descrizione degli errori comuni.

### Prendere decisioni nel codice

- [Come eseguire diversi blocchi di codice a seconda del valore di una variabile o di altre condizioni?](/it/docs/Learn_web_development/Core/Scripting/Conditionals)
- [Come usare le istruzioni if ...else?](/it/docs/Learn_web_development/Core/Scripting/Conditionals#if...else_statements)
- [Come nidificare un blocco decisionale all'interno di un altro?](/it/docs/Learn_web_development/Core/Scripting/Conditionals#nesting_if...else)
- [Come usare gli operatori logici AND, OR e NOT in JavaScript?](/it/docs/Learn_web_development/Core/Scripting/Conditionals#logical_operators_and_or_and_not)
- [Come gestire comodamente un gran numero di scelte per una condizione?](/it/docs/Learn_web_development/Core/Scripting/Conditionals#switch_statements)
- [Come usare un operatore ternario per fare una scelta rapida tra due opzioni basate su un test vero o falso?](/it/docs/Learn_web_development/Core/Scripting/Conditionals#ternary_operator)

### Cicli/iterazione

- [Come eseguire lo stesso pezzo di codice più e più volte?](/it/docs/Learn_web_development/Core/Scripting/Loops)
- [Come uscire da un ciclo prima del termine se si incontra una determinata condizione?](/it/docs/Learn_web_development/Core/Scripting/Loops#exiting_loops_with_break)
- [Come saltare alla prossima iterazione di un ciclo se si incontra una determinata condizione?](/it/docs/Learn_web_development/Core/Scripting/Loops#skipping_iterations_with_continue)
- [Come usare i cicli while e do...while?](/it/docs/Learn_web_development/Core/Scripting/Loops#while_and_do...while)

## Casi d'uso intermedi

### Funzioni

- [Come trovare le funzioni nel browser?](/it/docs/Learn_web_development/Core/Scripting/Functions#built-in_browser_functions)
- [Qual è la differenza tra una funzione e un metodo?](/it/docs/Learn_web_development/Core/Scripting/Functions#functions_versus_methods)
- [Come creare le proprie funzioni?](/it/docs/Learn_web_development/Core/Scripting/Build_your_own_function)
- [Come eseguire (chiamare o invocare) una funzione?](/it/docs/Learn_web_development/Core/Scripting/Functions#invoking_functions)
- [Cos'è una funzione anonima?](/it/docs/Learn_web_development/Core/Scripting/Functions#anonymous_functions_and_arrow_functions)
- [Come specificare i parametri (o argomenti) quando si invoca una funzione?](/it/docs/Learn_web_development/Core/Scripting/Functions#function_parameters)
- [Cos'è l'ambito di una funzione?](/it/docs/Learn_web_development/Core/Scripting/Functions#function_scope_and_conflicts)
- [Cosa sono i valori di ritorno e come utilizzarli?](/it/docs/Learn_web_development/Core/Scripting/Return_values)

### Oggetti

- [Come creare un oggetto?](/it/docs/Learn_web_development/Core/Scripting/Object_basics#object_basics)
- [Cos'è la notazione a punto?](/it/docs/Learn_web_development/Core/Scripting/Object_basics#dot_notation)
- [Cos'è la notazione a parentesi quadre?](/it/docs/Learn_web_development/Core/Scripting/Object_basics#bracket_notation)
- [Come ottenere e impostare i metodi e le proprietà di un oggetto?](/it/docs/Learn_web_development/Core/Scripting/Object_basics#setting_object_members)
- [Cosa significa `this` nel contesto di un oggetto?](/it/docs/Learn_web_development/Core/Scripting/Object_basics#what_is_this)
- [Cos'è la programmazione orientata agli oggetti?](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object-oriented_programming)
- [Cosa sono i costruttori e le istanze, e come crearli?](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object-oriented_programming#classes_and_instances)

### JSON

- [Come strutturare i dati JSON e leggerli da JavaScript?](/it/docs/Learn_web_development/Core/Scripting/JSON#json_structure)
- [Come convertire un oggetto JSON in una stringa di testo e viceversa?](/it/docs/Learn_web_development/Core/Scripting/JSON#converting_between_objects_and_text)

### Eventi

- [Cosa sono i gestori di eventi e come usarli?](/it/docs/Learn_web_development/Core/Scripting/Events#event_handler_properties)
- [Cosa sono i gestori di eventi inline?](/it/docs/Learn_web_development/Core/Scripting/Events#inline_event_handlers_—_dont_use_these)
- [Cosa fa la funzione `addEventListener()` e come usarla?](/it/docs/Learn_web_development/Core/Scripting/Events#using_addeventlistener)
- [Cosa sono gli oggetti evento e come usarli?](/it/docs/Learn_web_development/Core/Scripting/Events#event_objects)
- [Come prevenire il comportamento predefinito di un evento?](/it/docs/Learn_web_development/Core/Scripting/Events#preventing_default_behavior)
- [Come gli eventi vengono attivati su elementi annidati? (propagazione dell'evento, anche correlata — bubbling e capturing degli eventi)](/it/docs/Learn_web_development/Core/Scripting/Event_bubbling)
- [Cos'è la delega degli eventi e come funziona?](/it/docs/Learn_web_development/Core/Scripting/Event_bubbling#event_delegation)

### JavaScript orientato agli oggetti

- [Cosa sono i prototipi degli oggetti?](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_prototypes)
- [Come aggiungere metodi al costruttore?](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_prototypes#setting_a_prototype)
- [Come creare un nuovo costruttore che eredita i propri membri da un costruttore padre?](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Classes_in_JavaScript)
- [Quando dovrebbe utilizzare l'ereditarietà in JavaScript?](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object-oriented_programming#inheritance)

### API Web

- [Come manipolare il DOM (ad esempio, aggiungendo o rimuovendo elementi) usando JavaScript?](/it/docs/Learn_web_development/Core/Scripting/DOM_scripting#active_learning_basic_dom_manipulation)
