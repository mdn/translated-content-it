---
title: Risolvere problemi comuni di JavaScript
short-title: Problemi comuni di JavaScript
slug: Learn_web_development/Howto/Solve_JavaScript_problems
l10n:
  sourceCommit: 3143a6094e7b87cf1a96b61f9551fb4d95049777
---

I seguenti link rimandano a soluzioni per problemi comuni che potrebbero verificarsi durante la scrittura di JavaScript.

## Errori comuni dei principianti

### Ortografia e uso delle maiuscole/minuscole corretti

Se il codice non funziona e/o il browser segnala che qualcosa è undefined, verificare di aver scritto correttamente tutti i nomi delle variabili, i nomi delle funzioni e così via.

Alcune funzioni integrate del browser che spesso causano problemi sono:

| Corretto                   | Errato                    |
| -------------------------- | ------------------------- |
| `getElementsByTagName()`   | `getElementByTagName()`   |
| `getElementsByName()`      | `getElementByName()`      |
| `getElementsByClassName()` | `getElementByClassName()` |
| `getElementById()`         | `getElementsById()`       |

### Posizione del punto e virgola

È necessario assicurarsi di non inserire punti e virgola in modo errato. Ad esempio:

| Corretto                    | Errato                      |
| --------------------------- | --------------------------- |
| `elem.style.color = 'red';` | `elem.style.color = 'red;'` |

### Funzioni

Esistono diversi problemi che possono verificarsi con le funzioni.

Uno degli errori più comuni consiste nel dichiarare la funzione senza chiamarla da nessuna parte. Ad esempio:

```js
function myFunction() {
  alert("This is my function.");
}
```

Questo codice non farà nulla a meno che non venga chiamato con la seguente istruzione:

```js
myFunction();
```

#### Scope delle funzioni

Ricordare che [le funzioni hanno il proprio scope](/it/docs/Learn_web_development/Core/Scripting/Functions#function_scope_and_conflicts): non è possibile accedere dall'esterno della funzione al valore di una variabile impostata al suo interno, a meno che la variabile non sia stata dichiarata globalmente, ovvero non all'interno di alcuna funzione, oppure che [il valore venga restituito](/it/docs/Learn_web_development/Core/Scripting/Return_values) dalla funzione.

#### Eseguire codice dopo un'istruzione return

Ricordare inoltre che, quando si esce da una funzione, l'interprete JavaScript esce dalla funzione: nessun codice dopo l'istruzione return verrà eseguito.

In effetti, alcuni browser, come Firefox, visualizzeranno un messaggio di errore nella console per sviluppatori se è presente del codice dopo un'istruzione return. Firefox visualizza "unreachable code after return statement".

### Notazione degli oggetti rispetto all'assegnazione normale

Quando si assegna qualcosa normalmente in JavaScript, si utilizza un singolo segno di uguale, ad esempio:

```js
const myNumber = 0;
```

Con gli [oggetti](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects), tuttavia, è necessario fare attenzione a usare la sintassi corretta. L'oggetto deve essere racchiuso tra parentesi graffe, i nomi dei membri devono essere separati dai relativi valori mediante due punti e i membri devono essere separati da virgole. Ad esempio:

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
- [Che cos'è una web API?](/it/docs/Learn_web_development/Extensions/Client-side_APIs/Introduction#what_are_apis)
- [Che cos'è il DOM?](/it/docs/Learn_web_development/Core/Scripting/DOM_scripting#the_document_object_model)

## Casi d'uso di base

### Generali

- [Come si aggiunge JavaScript alla propria pagina?](/it/docs/Learn_web_development/Core/Scripting/What_is_JavaScript#how_do_you_add_javascript_to_your_page)
- [Come si aggiungono commenti al codice JavaScript?](/it/docs/Learn_web_development/Core/Scripting/What_is_JavaScript#comments)

### Variabili

- [Come si dichiara una variabile?](/it/docs/Learn_web_development/Core/Scripting/Variables#declaring_a_variable)
- [Come si inizializza una variabile con un valore?](/it/docs/Learn_web_development/Core/Scripting/Variables#initializing_a_variable)
- [Come si aggiorna il valore di una variabile?](/it/docs/Learn_web_development/Core/Scripting/Variables#updating_a_variable) (vedere anche gli [operatori di assegnazione](/it/docs/Learn_web_development/Core/Scripting/Math#assignment_operators))
- [Quali tipi di dati possono avere i valori in JavaScript?](/it/docs/Learn_web_development/Core/Scripting/Variables#variable_types)
- [Che cosa significa "loosely typed"?](/it/docs/Learn_web_development/Core/Scripting/Variables#dynamic_typing)

### Matematica

- [Quali tipi di numeri devono essere gestiti nello sviluppo web?](/it/docs/Learn_web_development/Core/Scripting/Math#types_of_numbers)
- [Come si eseguono operazioni matematiche di base in JavaScript?](/it/docs/Learn_web_development/Core/Scripting/Math#arithmetic_operators)
- [Che cos'è la precedenza degli operatori e come viene gestita in JavaScript?](/it/docs/Learn_web_development/Core/Scripting/Math#operator_precedence)
- [Come si incrementano e decrementano i valori in JavaScript?](/it/docs/Learn_web_development/Core/Scripting/Math#increment_and_decrement_operators)
- [Come si confrontano i valori in JavaScript?](/it/docs/Learn_web_development/Core/Scripting/Math#comparison_operators) (ad esempio, per vedere quale è maggiore o se un valore è uguale a un altro).

### Stringhe

- [Come si crea una stringa in JavaScript?](/it/docs/Learn_web_development/Core/Scripting/Strings#declaring_strings)
- [È necessario usare apici singoli o doppi?](/it/docs/Learn_web_development/Core/Scripting/Strings#single_quotes_double_quotes_and_backticks)
- [Come si uniscono le stringhe?](/it/docs/Learn_web_development/Core/Scripting/Strings#concatenation_in_context)
- [È possibile unire stringhe e numeri?](/it/docs/Learn_web_development/Core/Scripting/Strings#numbers_vs._strings)
- [Come si trova la lunghezza di una stringa?](/it/docs/Learn_web_development/Core/Scripting/Useful_string_methods#finding_the_length_of_a_string)
- [Come si trova il carattere in una determinata posizione di una stringa?](/it/docs/Learn_web_development/Core/Scripting/Useful_string_methods#retrieving_a_specific_string_character)
- [Come si trova ed estrae una sottostringa specifica da una stringa?](/it/docs/Learn_web_development/Core/Scripting/Useful_string_methods#extracting_a_substring_from_a_string)
- [Come si modifica il maiuscolo/minuscolo di una stringa?](/it/docs/Learn_web_development/Core/Scripting/Useful_string_methods#changing_case)
- [Come si sostituisce una sottostringa specifica con un'altra?](/it/docs/Learn_web_development/Core/Scripting/Useful_string_methods#updating_parts_of_a_string)

### Array

- [Come si crea un array?](/it/docs/Learn_web_development/Core/Scripting/Arrays#creating_arrays)
- [Come si accede agli elementi di un array e come li si modifica?](/it/docs/Learn_web_development/Core/Scripting/Arrays#accessing_and_modifying_array_items) (questo include gli array multidimensionali)
- [Come si trova la lunghezza di un array?](/it/docs/Learn_web_development/Core/Scripting/Arrays#finding_the_length_of_an_array)
- [Come si aggiungono elementi a un array?](/it/docs/Learn_web_development/Core/Scripting/Arrays#adding_items)
- [Come si rimuovono elementi da un array?](/it/docs/Learn_web_development/Core/Scripting/Arrays#removing_items)
- [Come si divide una stringa in elementi di un array, oppure si uniscono gli elementi di un array in una stringa?](/it/docs/Learn_web_development/Core/Scripting/Arrays#converting_between_strings_and_arrays)

### Debugging di JavaScript

- [Quali sono i tipi di errore di base?](/it/docs/Learn_web_development/Core/Scripting/What_went_wrong#types_of_error)
- [Che cosa sono gli strumenti per sviluppatori del browser e come vi si accede?](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools)
- [Come si registra un valore nella console JavaScript?](/it/docs/Learn_web_development/Core/Scripting/Debugging_JavaScript#the_console_api)
- [Come si usano i breakpoint e le altre funzionalità di debugging di JavaScript?](/it/docs/Learn_web_development/Core/Scripting/Debugging_JavaScript#using_the_javascript_debugger)

Per ulteriori informazioni sul debugging di JavaScript, vedere [Debugging e gestione degli errori di JavaScript](/it/docs/Learn_web_development/Core/Scripting/Debugging_JavaScript). Vedere inoltre [Altri errori comuni](/it/docs/Learn_web_development/Core/Scripting/What_went_wrong#other_common_errors) per una descrizione degli errori comuni.

### Prendere decisioni nel codice

- [Come si eseguono blocchi di codice diversi, a seconda del valore di una variabile o di un'altra condizione?](/it/docs/Learn_web_development/Core/Scripting/Conditionals)
- [Come si usano le istruzioni if ...else?](/it/docs/Learn_web_development/Core/Scripting/Conditionals#if...else_statements)
- [Come si annida un blocco decisionale all'interno di un altro?](/it/docs/Learn_web_development/Core/Scripting/Conditionals#nesting_if...else)
- [Come si usano gli operatori AND, OR e NOT in JavaScript?](/it/docs/Learn_web_development/Core/Scripting/Conditionals#logical_operators_and_or_and_not)
- [Come si gestisce comodamente un gran numero di scelte per una condizione?](/it/docs/Learn_web_development/Core/Scripting/Conditionals#switch_statements)
- [Come si usa un operatore ternario per scegliere rapidamente tra due opzioni in base a un test vero o falso?](/it/docs/Learn_web_development/Core/Scripting/Conditionals#ternary_operator)

### Cicli/iterazione

- [Come si esegue ripetutamente lo stesso frammento di codice?](/it/docs/Learn_web_development/Core/Scripting/Loops)
- [Come si esce da un ciclo prima della fine se viene soddisfatta una determinata condizione?](/it/docs/Learn_web_development/Core/Scripting/Loops#exiting_loops_with_break)
- [Come si passa all'iterazione successiva di un ciclo se viene soddisfatta una determinata condizione?](/it/docs/Learn_web_development/Core/Scripting/Loops#skipping_iterations_with_continue)
- [Come si usano i cicli while e do...while?](/it/docs/Learn_web_development/Core/Scripting/Loops#while_and_do...while)

## Casi d'uso intermedi

### Funzioni

- [Come si trovano le funzioni nel browser?](/it/docs/Learn_web_development/Core/Scripting/Functions#built-in_browser_functions)
- [Qual è la differenza tra una funzione e un metodo?](/it/docs/Learn_web_development/Core/Scripting/Functions#functions_versus_methods)
- [Come si creano le proprie funzioni?](/it/docs/Learn_web_development/Core/Scripting/Build_your_own_function)
- [Come si esegue, chiama o invoca una funzione?](/it/docs/Learn_web_development/Core/Scripting/Functions#invoking_functions)
- [Che cos'è una funzione anonima?](/it/docs/Learn_web_development/Core/Scripting/Functions#anonymous_functions_and_arrow_functions)
- [Come si specificano parametri, o argomenti, quando si invoca una funzione?](/it/docs/Learn_web_development/Core/Scripting/Functions#function_arguments_and_parameters)
- [Che cos'è lo scope di una funzione?](/it/docs/Learn_web_development/Core/Scripting/Functions#function_scope_and_conflicts)
- [Che cosa sono i valori restituiti e come si usano?](/it/docs/Learn_web_development/Core/Scripting/Return_values)

### Oggetti

- [Come si crea un oggetto?](/it/docs/Learn_web_development/Core/Scripting/Object_basics#object_basics)
- [Che cos'è la dot notation?](/it/docs/Learn_web_development/Core/Scripting/Object_basics#dot_notation)
- [Che cos'è la bracket notation?](/it/docs/Learn_web_development/Core/Scripting/Object_basics#bracket_notation)
- [Come si ottengono e impostano i metodi e le proprietà di un oggetto?](/it/docs/Learn_web_development/Core/Scripting/Object_basics#setting_object_members)
- [Che cos'è `this` nel contesto di un oggetto?](/it/docs/Learn_web_development/Core/Scripting/Object_basics#what_is_this)
- [Che cos'è la programmazione orientata agli oggetti?](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object-oriented_programming)
- [Che cosa sono i costruttori e le istanze e come si creano?](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object-oriented_programming#classes_and_instances)

### JSON

- [Come si strutturano i dati JSON e come li si legge da JavaScript?](/it/docs/Learn_web_development/Core/Scripting/JSON#json_structure)
- [Come si converte un oggetto JSON in una stringa di testo e viceversa?](/it/docs/Learn_web_development/Core/Scripting/JSON#converting_between_objects_and_text)

### Eventi

- [Che cosa sono gli event handler e come si usano?](/it/docs/Learn_web_development/Core/Scripting/Events#event_handler_properties)
- [Che cosa sono gli event handler inline?](/it/docs/Learn_web_development/Core/Scripting/Events#inline_event_handlers_—_dont_use_these)
- [Che cosa fa la funzione `addEventListener()` e come si usa?](/it/docs/Learn_web_development/Core/Scripting/Events#using_addeventlistener)
- [Che cosa sono gli oggetti evento e come si usano?](/it/docs/Learn_web_development/Core/Scripting/Events#event_objects)
- [Come si impedisce il comportamento predefinito di un evento?](/it/docs/Learn_web_development/Core/Scripting/Events#preventing_default_behavior)
- [Come vengono attivati gli eventi sugli elementi annidati? (propagazione degli eventi, inclusi anche bubbling e capturing degli eventi)](/it/docs/Learn_web_development/Core/Scripting/Event_bubbling)
- [Che cos'è la delega degli eventi e come funziona?](/it/docs/Learn_web_development/Core/Scripting/Event_bubbling#event_delegation)

### JavaScript orientato agli oggetti

- [Che cosa sono i prototipi degli oggetti?](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_prototypes)
- [Come si aggiungono metodi al costruttore?](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_prototypes#setting_a_prototype)
- [Come si crea un nuovo costruttore che eredita i suoi membri da un costruttore padre?](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Classes_in_JavaScript)
- [Quando è opportuno usare l'ereditarietà in JavaScript?](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object-oriented_programming#inheritance)

### Web API

- [Come si manipola il DOM, ad esempio aggiungendo o rimuovendo elementi, usando JavaScript?](/it/docs/Learn_web_development/Core/Scripting/DOM_scripting#doing_some_basic_dom_manipulation)
