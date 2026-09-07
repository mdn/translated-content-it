---
title: Pagina dell'elenco delle istanze di libri
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/BookInstance_list_page
l10n:
  sourceCommit: 8443cb34d9944d8eb8e2c5add598bec26ed6d21f
---

Successivamente verrà implementato l'elenco di tutte le copie di libri (`BookInstance`) nella biblioteca. Questa pagina deve includere il titolo del `Book` associato a ciascun `BookInstance` (collegato alla relativa pagina dei dettagli), insieme ad altre informazioni nel modello `BookInstance`, inclusi lo stato, l'imprint e l'id univoco di ciascuna copia. Il testo dell'id univoco deve essere collegato alla pagina dei dettagli del `BookInstance`.

## Controller

La funzione controller dell'elenco `BookInstance` deve ottenere un elenco di tutte le istanze di libri, popolare le informazioni del libro associato e quindi passare l'elenco al template per il rendering.

Aprire `/controllers/bookinstanceController.js`.
Trovare il metodo controller esportato `bookinstance_list()` e sostituirlo con il codice seguente.

```js
// Display list of all BookInstances.
exports.bookinstance_list = async (req, res, next) => {
  const allBookInstances = await BookInstance.find().populate("book").exec();

  res.render("bookinstance_list", {
    title: "Book Instance List",
    bookinstance_list: allBookInstances,
  });
};
```

Il route handler chiama la funzione `find()` sul modello `BookInstance`, quindi concatena una chiamata a `populate()` con il campo `book`: questo sostituirà l'id del libro memorizzato per ogni `BookInstance` con un documento `Book` completo.
Alla fine viene concatenato anche `exec()` per eseguire la query e restituire una promise.

Il route handler usa `await` per attendere la promise, sospendendo l'esecuzione finché non viene completata.
Se la promise viene soddisfatta, i risultati della query vengono salvati nella variabile `allBookInstances` e il route handler continua l'esecuzione.

L'ultima parte del codice chiama `render()`, specificando il template **bookinstance_list** (.pug) e passando al template i valori per `title` e `bookinstance_list`.

## Vista

Creare **/views/bookinstance_list.pug** e copiare il testo seguente.

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

Questa vista è molto simile a tutte le altre. Estende il layout, sostituendo il blocco _content_, visualizza il `title` passato dal controller e itera su tutte le copie di libri in `bookinstance_list`. Per ogni copia vengono visualizzati lo stato (codificato tramite colore) e, se il libro non è disponibile, la data di restituzione prevista. Viene introdotta una nuova funzionalità: è possibile usare la notazione con punto dopo un tag per assegnare una classe. Quindi `span.text-success` verrà compilato in `<span class="text-success">` (e può anche essere scritto in Pug come `span(class="text-success")`).

## Che aspetto ha?

Eseguire l'applicazione, aprire il browser all'indirizzo `http://localhost:3000/`, quindi selezionare il collegamento _Tutte le istanze di libri_. Se tutto è configurato correttamente, il sito dovrebbe avere un aspetto simile allo screenshot seguente.

![Pagina dell'elenco BookInstance - sito Express Local Library](locallibary_express_bookinstance_list.png)

## Passaggi successivi

- Tornare a [Tutorial Express Parte 5: Visualizzazione dei dati della biblioteca](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data).
- Procedere al prossimo sottoarticolo della parte 5: [Formattazione delle date mediante luxon](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Date_formatting_using_moment).
