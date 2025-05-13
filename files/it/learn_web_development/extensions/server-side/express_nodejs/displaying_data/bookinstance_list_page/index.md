---
title: Pagina elenco copie libro
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/BookInstance_list_page
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Ora implementeremo il nostro elenco di tutte le copie di libri (`BookInstance`) nella biblioteca. Questa pagina deve includere il titolo del `Book` associato a ciascun `BookInstance` (collegato alla sua pagina di dettaglio) insieme ad altre informazioni nel modello `BookInstance`, incluso lo stato, l'impronta, e l'id univoco di ciascuna copia. Il testo dell'id univoco dovrebbe essere collegato alla pagina di dettaglio di `BookInstance`.

## Controller

La funzione del controller per l'elenco di `BookInstance` deve ottenere un elenco di tutte le istanze di libro, popolare le informazioni del libro associato e quindi passare l'elenco al template per il rendering.

Aprire `/controllers/bookinstanceController.js`.
Trovare il metodo del controller esportato `bookinstance_list()` e sostituirlo con il seguente codice.

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

Il gestore della route chiama la funzione `find()` sul modello `BookInstance`, e poi collega in cascata una chiamata a `populate()` con il campo `book`—questo sostituirà l'id del libro memorizzato per ciascun `BookInstance` con un documento completo di `Book`.
`exec()` viene poi concatenato alla fine per eseguire la query e restituire una promessa.

Il gestore della route utilizza `await` per attendere la promessa, sospendendo l'esecuzione fino a quando è risolta.
Se la promessa è adempiuta, i risultati della query vengono salvati nella variabile `allBookInstances`, e il gestore della route continua l'esecuzione.

L'ultima parte del codice chiama `render()`, specificando il template **bookinstance_list** (.pug) e passando i valori per `title` e `bookinstance_list` nel template.

## Vista

Creare **/views/bookinstance_list.pug** e copiare il testo qui sotto.

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

Questa vista è molto simile a tutte le altre. Estende il layout, sostituendo il blocco _content_, visualizza il `title` passato dal controller e itera attraverso tutte le copie di libro in `bookinstance_list`. Per ogni copia visualizziamo il suo stato (codificato a colori) e, se il libro non è disponibile, la sua data di ritorno prevista. Viene introdotta una nuova funzionalità—possiamo usare la notazione a punti dopo un tag per assegnare una classe. Quindi `span.text-success` sarà compilato in `<span class="text-success">` (e potrebbe anche essere scritto in Pug come `span(class="text-success")`).

## Com'è il risultato?

Eseguire l'applicazione, aprire il browser su `http://localhost:3000/`, quindi selezionare il link _All book-instances_. Se tutto è configurato correttamente, il sito dovrebbe apparire come nello screenshot seguente.

![Pagina elenco copie libro - Sito Express Local Library](locallibary_express_bookinstance_list.png)

## Passi successivi

- Ritorna a [Express Tutorial Parte 5: Visualizzare i dati della biblioteca](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data).
- Procedi al prossimo sotto-articolo della parte 5: [Formattazione delle date usando luxon](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Date_formatting_using_moment).
