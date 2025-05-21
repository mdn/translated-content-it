---
title: Risolvere problemi comuni in JavaScript
short-title: Problemi comuni in JavaScript
slug: Learn_web_development/Howto/Solve_JavaScript_problems
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

I seguenti link puntano a soluzioni di problemi comuni che si possono incontrare quando si scrive JavaScript.

## Errori comuni dei principianti

### Ortografia e maiuscole corrette

Se il tuo codice non funziona e/o il browser segnala che qualcosa non è definito, controlla che tu abbia scritto tutti i nomi delle variabili, dei metodi, etc. correttamente.

Alcune funzioni integrate del browser che causano problemi sono:

| Corretto                   | Errato                     |
| -------------------------- | -------------------------- |
| `getElementsByTagName()`   | `getElementByTagName()`    |
| `getElementsByName()`      | `getElementByName()`       |
| `getElementsByClassName()` | `getElementByClassName()`  |
| `getElementById()`         | `getElementsById()`        |

### Posizione del punto e virgola

Devi assicurarti di non posizionare punti e virgola in modo errato. Per esempio:

| Corretto                     | Errato                       |
| ---------------------------- | ---------------------------- |
| `elem.style.color = 'red';`  | `elem.style.color = 'red;'`  |

### Funzioni

Ci sono diversi errori che possono accadere con le funzioni.

Uno degli errori più comuni è dichiarare la funzione ma non chiamarla da nessuna parte. Per esempio:

```js
function myFunction() {
  alert("This is my function.");
}
```

Questo codice non farà nulla a meno che tu non lo chiami con la seguente istruzione:

```js
myFunction();
```

#### Ambito delle funzioni

Ricorda che [le funzioni hanno il loro ambito](/it/docs/Learn_web_development/Core/Scripting/Functions#function_scope_and_conflicts) — non puoi accedere a una variabile impostata all'interno di una funzione da fuori la funzione stessa, a meno che tu non abbia dichiarato la variabile a livello globale (cioè non all'interno di nessuna funzione) o [restituisca il valore](/it/docs/Learn_web_development/Core/Scripting/Return_values) dalla funzione.

#### Esecuzione del codice dopo una dichiarazione di ritorno

Ricorda inoltre che quando esci da una funzione con una dichiarazione di ritorno, l'interprete JavaScript esce dalla funzione — nessun codice dopo la dichiarazione di ritorno verrà eseguito.

Infatti, alcuni browser (come Firefox) ti daranno un messaggio di errore nella console degli sviluppatori se hai codice dopo una dichiarazione di ritorno. Firefox restituisce "unreachable code after return statement".

### Notazione oggetti vs assegnazione normale

Quando assegni qualcosa normalmente in JavaScript, usi un singolo segno di uguale, ad esempio:

```js
const myNumber = 0;
```

Con gli [Oggetti](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects), tuttavia, devi fare attenzione a utilizzare la sintassi corretta. L'oggetto deve essere circondato da parentesi graffe, i nomi dei membri devono essere separati dai loro valori usando i due punti, e i membri devono essere separati da virgole. Ad esempio:

```js
const myObject = {
  name: "Chris",
  age: 38,
};
```

## Definizioni di base

- [Che cos'è JavaScript?](/it/docs/Learn_web_development/Core/Scripting/What_is_JavaScript#a_high-level_definition)
- [Che cos'è una variabile?](/it/docs/Learn_web_development/Core/Scripting/Variables#what_is_a_variable)
- [Che cosa sono le stringhe?](/it/docs/Learn_web_development/Core/Scripting/Strings)
- [Che cos'è un array?](/it/docs/Learn_web_development/Core/Scripting/Arrays#what_is_an_array)
- [Che cos'è un ciclo?](/it/docs/Learn_web_development/Core/Scripting/Loops)
- [Che cos'è una funzione?](/it/docs/Learn_web_development/Core/Scripting/Functions)
- [Che cos'è un evento?](/it/docs/Learn_web_development/Core/Scripting/Events)
- [Che cos'è un oggetto?](/it/docs/Learn_web_development/Core/Scripting/Object_basics#object_basics)
- [Che cos'è JSON?](/it/docs/Learn_web_development/Core/Scripting/JSON#no_really_what_is_json)
- [Che cos'è una API web?](/it/docs/Learn_web_development/Extensions/Client-side_APIs/Introduction#what_are_apis)
- [Che cos'è il DOM?](/it/docs/Learn_web_development/Core/Scripting/DOM_scripting#the_document_object_model)

## Uso di base

### Generale

- [Come si aggiunge JavaScript alla tua pagina?](/it/docs/Learn_web_development/Core/Scripting/What_is_JavaScript#how_do_you_add_javascript_to_your_page)
- [Come si aggiungono commenti al codice JavaScript?](/it/docs/Learn_web_development/Core/Scripting/What_is_JavaScript#comments)

### Variabili

- [Come si dichiara una variabile?](/it/docs/Learn_web_development/Core/Scripting/Variables#declaring_a_variable)
- [Come si inizializza una variabile con un valore?](/it/docs/Learn_web_development/Core/Scripting/Variables#initializing_a_variable)
- [Come si aggiorna il valore di una variabile?](/it/docs/Learn_web_development/Core/Scripting/Variables#updating_a_variable) (vedi anche [Operatore di assegnazione](/it/docs/Learn_web_development/Core/Scripting/Math#assignment_operators))
- [Che tipi di dati possono avere i valori in JavaScript?](/it/docs/Learn_web_development/Core/Scripting/Variables#variable_types)
- [Cosa significa 'typed loosamente'?](/it/docs/Learn_web_development/Core/Scripting/Variables#dynamic_typing)

### Matematica

- [Quali tipi di numeri devi trattare nello sviluppo web?](/it/docs/Learn_web_development/Core/Scripting/Math#types_of_numbers)
- [Come fare calcoli matematici di base in JavaScript?](/it/docs/Learn_web_development/Core/Scripting/Math#arithmetic_operators)
- [Cos'è la precedenza degli operatori e come viene gestita in JavaScript?](/it/docs/Learn_web_development/Core/Scripting/Math#operator_precedence)
- [Come incrementare e decrementare valori in JavaScript?](/it/docs/Learn_web_development/Core/Scripting/Math#increment_and_decrement_operators)
- [Come confrontare valori in JavaScript?](/it/docs/Learn_web_development/Core/Scripting/Math#comparison_operators) (ad esempio, per vedere quale è maggiore, o se un valore è uguale a un altro).

### Stringhe

- [Come si crea una stringa in JavaScript?](/it/docs/Learn_web_development/Core/Scripting/Strings#declaring_strings)
- [È necessario usare virgolette singole o doppie?](/it/docs/Learn_web_development/Core/Scripting/Strings#single_quotes_double_quotes_and_backticks)
- [Come si uniscono le stringhe insieme?](/it/docs/Learn_web_development/Core/Scripting/Strings#concatenation_in_context)
- [È possibile unire stringhe e numeri insieme?](/it/docs/Learn_web_development/Core/Scripting/Strings#numbers_vs._strings)
- [Come si trova la lunghezza di una stringa?](/it/docs/Learn_web_development/Core/Scripting/Useful_string_methods#finding_the_length_of_a_string)
- [Come si trova quale carattere si trova a una certa posizione in una stringa?](/it/docs/Learn_web_development/Core/Scripting/Useful_string_methods#retrieving_a_specific_string_character)
- [Come si trova ed estrae una sottostringa specifica da una stringa?](/it/docs/Learn_web_development/Core/Scripting/Useful_string_methods#extracting_a_substring_from_a_string)
- [Come si cambia il maiuscolo/minuscolo di una stringa?](/it/docs/Learn_web_development/Core/Scripting/Useful_string_methods#changing_case)
- [Come si sostituisce una sottostringa specifica con un'altra?](/it/docs/Learn_web_development/Core/Scripting/Useful_string_methods#updating_parts_of_a_string)

### Array

- [Come si crea un array?](/it/docs/Learn_web_development/Core/Scripting/Arrays#creating_arrays)
- [Come si accede e si modifica gli elementi in un array?](/it/docs/Learn_web_development/Core/Scripting/Arrays#accessing_and_modifying_array_items) (ciò include array multidimensionali)
- [Come si trova la lunghezza di un array?](/it/docs/Learn_web_development/Core/Scripting/Arrays#finding_the_length_of_an_array)
- [Come si aggiungono elementi a un array?](/it/docs/Learn_web_development/Core/Scripting/Arrays#adding_items)
- [Come si rimuovono elementi da un array?](/it/docs/Learn_web_development/Core/Scripting/Arrays#removing_items)
- [Come si suddivide una stringa in elementi di un array o si uniscono elementi di un array in una stringa?](/it/docs/Learn_web_development/Core/Scripting/Arrays#converting_between_strings_and_arrays)

### Debugging JavaScript

- [Quali sono i tipi di base di errore?](/it/docs/Learn_web_development/Core/Scripting/What_went_wrong#types_of_error)
- [Cosa sono gli strumenti per sviluppatori nei browser e come vi si accede?](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools)
- [Come si registra un valore nella console JavaScript?](/it/docs/Learn_web_development/Core/Scripting/Debugging_JavaScript#the_console_api)
- [Come si usano i breakpoints e altre caratteristiche di debugging JavaScript?](/it/docs/Learn_web_development/Core/Scripting/Debugging_JavaScript#using_the_javascript_debugger)

Per ulteriori informazioni sul debugging in JavaScript, consulta [Debugging JavaScript e gestione degli errori](/it/docs/Learn_web_development/Core/Scripting/Debugging_JavaScript). Vedi anche [Altri errori comuni](/it/docs/Learn_web_development/Core/Scripting/What_went_wrong#other_common_errors) per una descrizione degli errori comuni.

### Prendere decisioni nel codice

- [Come eseguire blocchi di codice diversi, a seconda del valore di una variabile o di un'altra condizione?](/it/docs/Learn_web_development/Core/Scripting/Conditionals)
- [Come si usano le istruzioni if ...else?](/it/docs/Learn_web_development/Core/Scripting/Conditionals#if...else_statements)
- [Come si annida un blocco di decisione all'interno di un altro?](/it/docs/Learn_web_development/Core/Scripting/Conditionals#nesting_if...else)
- [Come si usano gli operatori AND, OR e NOT in JavaScript?](/it/docs/Learn_web_development/Core/Scripting/Conditionals#logical_operators_and_or_and_not)
- [Come gestire comodamente un gran numero di scelte per una condizione?](/it/docs/Learn_web_development/Core/Scripting/Conditionals#switch_statements)
- [Come si usa un operatore ternario per fare una scelta rapida tra due opzioni basate su un test vero o falso?](/it/docs/Learn_web_development/Core/Scripting/Conditionals#ternary_operator)

### Cicli/iterazione

- [Come si esegue la stessa porzione di codice più e più volte?](/it/docs/Learn_web_development/Core/Scripting/Loops)
- [Come si esce da un ciclo prima della fine se una certa condizione è soddisfatta?](/it/docs/Learn_web_development/Core/Scripting/Loops#exiting_loops_with_break)
- [Come si salta all'iterazione successiva di un ciclo se una certa condizione è soddisfatta?](/it/docs/Learn_web_development/Core/Scripting/Loops#skipping_iterations_with_continue)
- [Come si usano i cicli while e do...while?](/it/docs/Learn_web_development/Core/Scripting/Loops#while_and_do...while)

## Uso intermedio

### Funzioni

- [Come si trovano le funzioni nel browser?](/it/docs/Learn_web_development/Core/Scripting/Functions#built-in_browser_functions)
- [Qual è la differenza tra una funzione e un metodo?](/it/docs/Learn_web_development/Core/Scripting/Functions#functions_versus_methods)
- [Come si creano le tue funzioni?](/it/docs/Learn_web_development/Core/Scripting/Build_your_own_function)
- [Come si esegue (chiama, o invoca) una funzione?](/it/docs/Learn_web_development/Core/Scripting/Functions#invoking_functions)
- [Cos'è una funzione anonima?](/it/docs/Learn_web_development/Core/Scripting/Functions#anonymous_functions_and_arrow_functions)
- [Come si specificano i parametri (o argomenti) quando si invoca una funzione?](/it/docs/Learn_web_development/Core/Scripting/Functions#function_parameters)
- [Cos'è l'ambito delle funzioni?](/it/docs/Learn_web_development/Core/Scripting/Functions#function_scope_and_conflicts)
- [Cosa sono i valori di ritorno e come si usano?](/it/docs/Learn_web_development/Core/Scripting/Return_values)

### Oggetti

- [Come si crea un oggetto?](/it/docs/Learn_web_development/Core/Scripting/Object_basics#object_basics)
- [Cos'è la notazione a punti?](/it/docs/Learn_web_development/Core/Scripting/Object_basics#dot_notation)
- [Cos'è la notazione a parentesi quadre?](/it/docs/Learn_web_development/Core/Scripting/Object_basics#bracket_notation)
- [Come si ottengono e si impostano i metodi e le proprietà di un oggetto?](/it/docs/Learn_web_development/Core/Scripting/Object_basics#setting_object_members)
- [Cos'è `this`, nel contesto di un oggetto?](/it/docs/Learn_web_development/Core/Scripting/Object_basics#what_is_this)
- [Cos'è la programmazione orientata agli oggetti?](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object-oriented_programming)
- [Cosa sono i costruttori e le istanze, e come si creano?](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object-oriented_programming#classes_and_instances)

### JSON

- [Come strutturare i dati JSON e leggerli da JavaScript?](/it/docs/Learn_web_development/Core/Scripting/JSON#json_structure)
- [Come si converte un oggetto JSON in una stringa di testo e viceversa?](/it/docs/Learn_web_development/Core/Scripting/JSON#converting_between_objects_and_text)

### Eventi

- [Cosa sono i gestori di eventi e come si utilizzano?](/it/docs/Learn_web_development/Core/Scripting/Events#event_handler_properties)
- [Cosa sono i gestori di eventi inline?](/it/docs/Learn_web_development/Core/Scripting/Events#inline_event_handlers_—_dont_use_these)
- [Cosa fa la funzione `addEventListener()`, e come si utilizza?](/it/docs/Learn_web_development/Core/Scripting/Events#using_addeventlistener)
- [Cosa sono gli oggetti evento, e come si utilizzano?](/it/docs/Learn_web_development/Core/Scripting/Events#event_objects)
- [Come si impedisce il comportamento predefinito degli eventi?](/it/docs/Learn_web_development/Core/Scripting/Events#preventing_default_behavior)
- [Come si attivano eventi su elementi annidati? (propagazione degli eventi, anche correlata — bubbling e capturing degli eventi)](/it/docs/Learn_web_development/Core/Scripting/Event_bubbling)
- [Cos'è la delega degli eventi e come funziona?](/it/docs/Learn_web_development/Core/Scripting/Event_bubbling#event_delegation)

### JavaScript orientato agli oggetti

- [Cosa sono i prototipi degli oggetti?](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_prototypes)
- [Come si aggiungono metodi al costruttore?](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_prototypes#setting_a_prototype)
- [Come si crea un nuovo costruttore che eredita i suoi membri da un costruttore principale?](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Classes_in_JavaScript)
- [Quando si dovrebbe usare l'ereditarietà in JavaScript?](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object-oriented_programming#inheritance)

### API Web

- [Come si manipola il DOM (ad esempio, aggiungendo o rimuovendo elementi) usando JavaScript?](/it/docs/Learn_web_development/Core/Scripting/DOM_scripting#active_learning_basic_dom_manipulation)
