---
title: Pagina elenco BookInstance
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/BookInstance_list_page
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Prossimamente implementeremo il nostro elenco di tutte le copie dei libri (`BookInstance`) nella biblioteca. Questa pagina deve includere il titolo del `Book` associato a ogni `BookInstance` (collegato alla sua pagina di dettaglio) insieme ad altre informazioni nel modello `BookInstance`, compresi lo stato, l'imprint e l'id unico di ciascuna copia. Il testo dell'id unico dovrebbe essere collegato alla pagina di dettaglio del `BookInstance`.

## Controller

La funzione controller dell'elenco `BookInstance` deve ottenere un elenco di tutte le istanze di libri, popolare le informazioni del libro associato e quindi passare l'elenco al template per il rendering.

Apri `/controllers/bookinstanceController.js`.
Trova il metodo controller esportato `bookinstance_list()` e sostituiscilo con il seguente codice.

```js
// Display list of all BookInstances.
exports.bookinstance_list = asyncHandler(async (req, res, next) => {
  const allBookInstances = await BookInstance.find().populate("book").exec();

  res.render("bookinstance_list", {
    title: "Book Instance List",
    bookinstance_list: allBookInstances,
  });
});
```

Il gestore della rotta chiama la funzione `find()` sul modello `BookInstance`, e poi concatena una chiamata a `populate()` con il campo `book` — ciò sostituirà l'id del libro memorizzato per ogni `BookInstance` con un documento completo di `Book`.
`exec()` viene quindi concatenato alla fine per eseguire la query e restituire una promise.

Il gestore della rotta utilizza `await` per attendere la promise, sospendendo l'esecuzione fino a quando non è risolta.
Se la promise è soddisfatta, i risultati della query vengono salvati nella variabile `allBookInstances`, e il gestore della rotta continua l'esecuzione.

L'ultima parte del codice chiama `render()`, specificando il template **bookinstance_list** (.pug) e passando i valori per il `title` e `bookinstance_list` nel template.

## Vista

Crea **/views/bookinstance_list.pug** e copia il testo qui sotto.

```pug
extends layout

block content
  h1= title

  if bookinstance_list.length
    ul
      each val in bookinstance_list
        li
          a(href=val.url) #{val.book.title} : #{val.imprint} -&nbsp;
          if val.status=='Available'
            span.text-success #{val.status}
          else if val.status=='Maintenance'
            span.text-danger #{val.status}
          else
            span.text-warning #{val.status}
          if val.status!='Available'
            span  (Due: #{val.due_back} )

  else
    p There are no book copies in this library.
```

Questa vista è molto simile a tutte le altre. Estende il layout, sostituendo il blocco _content_, visualizza il `title` passato dal controller e itera attraverso tutte le copie dei libri in `bookinstance_list`. Per ogni copia visualizziamo il suo stato (con codice colore) e, se il libro non è disponibile, la data prevista di ritorno. Viene introdotta una nuova funzionalità: possiamo usare la notazione a punti dopo un tag per assegnare una classe. Quindi `span.text-success` sarà compilato in `<span class="text-success">` (e potrebbe anche essere scritto in Pug come `span(class="text-success")`).

## Come appare?

Esegui l'applicazione, apri il tuo browser su `http://localhost:3000/`, quindi seleziona il link _All book-instances_. Se tutto è configurato correttamente, il tuo sito dovrebbe apparire simile allo screenshot seguente.

![Pagina Elenco BookInstance - Sito biblioteca locale Express](locallibary_express_bookinstance_list.png)

## Prossimi passi

- Torna a [Tutorial Express Parte 5: Visualizzare i dati della biblioteca](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data).
- Passa al prossimo sottoarticolo della parte 5: [Formattazione delle date usando luxon](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Date_formatting_using_moment).
