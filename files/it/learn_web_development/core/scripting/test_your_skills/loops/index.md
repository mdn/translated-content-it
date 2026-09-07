---
title: "Metti alla prova le tue competenze: cicli"
short-title: "Test: cicli"
slug: Learn_web_development/Core/Scripting/Test_your_skills/Loops
l10n:
  sourceCommit: b36d59a0df933597c7d3b55e363f7a59e30d3ba3
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Loops","Learn_web_development/Core/Scripting/Functions", "Learn_web_development/Core/Scripting")}}

L'obiettivo di questo test di competenze è aiutare a valutare se è stato compreso il nostro articolo sul [codice iterativo](/it/docs/Learn_web_development/Core/Scripting/Loops).

> [!NOTE]
> Per ricevere aiuto, leggere la guida all'uso [Metti alla prova le tue competenze](/it/docs/Learn_web_development#test_your_skills). È inoltre possibile contattarci tramite uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Manipolazione del DOM: utile da conoscere

Alcune delle domande seguenti richiedono di scrivere codice di manipolazione del {{Glossary("DOM", "DOM")}} per completarle, ad esempio creando nuovi elementi HTML, impostando il loro contenuto testuale su valori di stringa specifici e annidandoli all'interno di elementi esistenti nella pagina, il tutto tramite JavaScript.

Questo argomento non è stato ancora trattato esplicitamente nel corso, ma sono già stati mostrati alcuni esempi che ne fanno uso; è quindi necessario fare qualche ricerca sulle API DOM necessarie per rispondere correttamente alle domande. Un buon punto di partenza è il tutorial [Introduzione allo scripting DOM](/it/docs/Learn_web_development/Core/Scripting/DOM_scripting).

## Cicli 1

Nel primo esercizio sui cicli, occorre scrivere un ciclo di base che esegua l'iterazione su tutti gli elementi dell'`myArray` fornito e li visualizzi sullo schermo all'interno di elementi di elenco (elementi [`<li>`](/it/docs/Web/HTML/Reference/Elements/li)). Questi devono essere aggiunti all'elemento `list` fornito.

<!-- Codice condiviso tra gli esempi -->

```html hidden live-sample___loops-1 live-sample___loops-2 live-sample___loops-3 live-sample___loops-1-finish live-sample___loops-2-finish live-sample___loops-3-finish
<section></section>
```

```css hidden live-sample___loops-1 live-sample___loops-2 live-sample___loops-3 live-sample___loops-1-finish live-sample___loops-2-finish live-sample___loops-3-finish
* {
  box-sizing: border-box;
}

p {
  color: purple;
  margin: 0.5em 0;
}
```

<!-- Codice specifico dell'esempio -->

Il punto di partenza dell'esercizio è simile a questo (non viene ancora mostrato nulla):

{{ EmbedLiveSample("loops-1", "100%", 60) }}

Ecco il codice sottostante per questo punto di partenza:

```js live-sample___loops-1
const myArray = ["tomatoes", "chick peas", "onions", "rice", "black beans"];
const list = document.createElement("ul");
const section = document.querySelector("section");
section.appendChild(list);

// Don't edit the code above here!

// Add your code here
```

L'output aggiornato dovrebbe essere simile a questo:

{{ EmbedLiveSample("loops-1-finish", "100%", 150) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il JavaScript completato dovrebbe essere simile a questo:

```js
// ...
// Don't edit the code above here!

for (let item of myArray) {
  const listItem = document.createElement("li");
  listItem.textContent = item;
  list.appendChild(listItem);
}
```

```js hidden live-sample___loops-1-finish
const myArray = ["tomatoes", "chick peas", "onions", "rice", "black beans"];
const list = document.createElement("ul");
const section = document.querySelector("section");
section.appendChild(list);

for (let item of myArray) {
  const listItem = document.createElement("li");
  listItem.textContent = item;
  list.appendChild(listItem);
}
```

</details>

## Cicli 2

In questo esercizio successivo, occorre scrivere un semplice programma che, dato un nome, cerchi un array di {{Glossary("Object", "oggetti")}} contenenti nomi e numeri di telefono e, se trova il nome, visualizzi il nome e il numero di telefono in un paragrafo.

Per iniziare vengono fornite tre variabili:

- `name`: contiene un nome da cercare.
- `para`: contiene un riferimento a un paragrafo, che verrà utilizzato per riportare i risultati.
- `phonebook`: contiene le voci della rubrica telefonica in cui effettuare la ricerca.

> [!NOTE]
> Se gli oggetti non sono ancora stati studiati, non c'è da preoccuparsi. Per ora, è sufficiente sapere come accedere a una coppia membro-valore. È possibile approfondire gli oggetti nel tutorial [Fondamenti degli oggetti JavaScript](/it/docs/Learn_web_development/Core/Scripting/Object_basics).

Per completare l'esercizio:

1. Scrivere un ciclo che esegua l'iterazione sull'array (`phonebook`) e cerchi il `name` fornito. Si deve usare un tipo di ciclo non utilizzato nell'esercizio precedente.
2. Se viene trovato il `name`, scriverlo insieme al `number` associato nel `textContent` del paragrafo fornito (`para`), nel formato "&lt;name>'s number is &lt;number>.". Successivamente, uscire dal ciclo prima che abbia completato tutte le iterazioni.
3. Se nessuno degli oggetti contiene il `name`, scrivere "Name not found in the phonebook" nel `textContent` del paragrafo fornito.

Il punto di partenza dell'esercizio è simile a questo (non viene ancora mostrato nulla):

{{ EmbedLiveSample("loops-2", "100%", 60) }}

Ecco il codice sottostante per questo punto di partenza:

```js live-sample___loops-2
const name = "Mustafa";
const para = document.createElement("p");

const phonebook = [
  { name: "Chris", number: "1549" },
  { name: "Li Kang", number: "9634" },
  { name: "Anne", number: "9065" },
  { name: "Francesca", number: "3001" },
  { name: "Mustafa", number: "6888" },
  { name: "Tina", number: "4312" },
  { name: "Bert", number: "7780" },
  { name: "Jada", number: "2282" },
];

const section = document.querySelector("section");
section.appendChild(para);

// Don't edit the code above here!

// Add your code here
```

L'output aggiornato dovrebbe essere simile a questo:

{{ EmbedLiveSample("loops-2-finish", "100%", 60) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il JavaScript completato dovrebbe essere simile a questo:

```js
// ...
// Don't edit the code above here!

for (let i = 0; i < phonebook.length; i++) {
  if (phonebook[i].name === name) {
    para.textContent = `${phonebook[i].name}'s number is ${phonebook[i].number}.`;
    break;
  }

  if (i === phonebook.length - 1) {
    para.textContent = "Name not found in the phonebook";
  }
}
```

```js hidden live-sample___loops-2-finish
const name = "Mustafa";
const para = document.createElement("p");

const phonebook = [
  { name: "Chris", number: "1549" },
  { name: "Li Kang", number: "9634" },
  { name: "Anne", number: "9065" },
  { name: "Francesca", number: "3001" },
  { name: "Mustafa", number: "6888" },
  { name: "Tina", number: "4312" },
  { name: "Bert", number: "7780" },
  { name: "Jada", number: "2282" },
];

const section = document.querySelector("section");
section.appendChild(para);

for (let i = 0; i < phonebook.length; i++) {
  if (phonebook[i].name === name) {
    para.textContent = `${phonebook[i].name}'s number is ${phonebook[i].number}.`;
    break;
  }

  if (i === phonebook.length - 1) {
    para.textContent = "Name not found in the phonebook";
  }
}
```

</details>

## Cicli 3

In questo esercizio finale, verrà verificato ogni numero da `500` fino a `2` per stabilire quali siano numeri primi, utilizzando la funzione di test fornita e visualizzando i numeri primi.

Vengono forniti i seguenti elementi:

- `i`: inizia con un valore di `500`; deve essere usato come iteratore.
- `para`: contiene un riferimento a un paragrafo, che verrà utilizzato per riportare i risultati.
- `isPrime()`: una funzione che, quando riceve un numero, restituisce `true` se il numero è primo e `false` in caso contrario.

Per completare l'esercizio:

1. Scrivere un ciclo che esegua l'iterazione su ogni numero da `500` fino a `2` (`1` non è considerato un numero primo) ed esegua la funzione `isPrime()` fornita su ciascuno di essi.
2. Per ogni numero che non è primo, passare all'iterazione successiva del ciclo. Per ogni numero che _è_ primo, aggiungerlo al `textContent` del paragrafo insieme a un qualche tipo di separatore.

Si deve usare un tipo di ciclo non utilizzato nei due esercizi precedenti.

Il punto di partenza dell'esercizio è simile a questo (non viene ancora mostrato nulla):

{{ EmbedLiveSample("loops-3", "100%", 60) }}

Ecco il codice sottostante per questo punto di partenza:

```js live-sample___loops-3
let i = 500;
const para = document.createElement("p");
const section = document.querySelector("section");
function isPrime(num) {
  for (let i = 2; i < num; i++) {
    if (num % i === 0) {
      return false;
    }
  }
  return true;
}
// Don't edit the code above here!

// Add your code here

// Don't edit the code below here!

section.appendChild(para);
```

L'output aggiornato dovrebbe essere simile a questo:

{{ EmbedLiveSample("loops-3-finish", "100%", 120) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il JavaScript completato dovrebbe essere simile a questo:

```js
// ...
// Don't edit the code above here!

do {
  if (isPrime(i)) {
    para.textContent += `${i}, `;
  }
  i--;
} while (i > 1);

// Don't edit the code below here!
// ...
```

```js hidden live-sample___loops-3-finish
let i = 500;
const para = document.createElement("p");
const section = document.querySelector("section");
function isPrime(num) {
  for (let i = 2; i < num; i++) {
    if (num % i === 0) {
      return false;
    }
  }
  return true;
}

do {
  if (isPrime(i)) {
    para.textContent += `${i}, `;
  }
  i--;
} while (i > 1);

section.appendChild(para);
```

</details>

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Loops","Learn_web_development/Core/Scripting/Functions", "Learn_web_development/Core/Scripting")}}
