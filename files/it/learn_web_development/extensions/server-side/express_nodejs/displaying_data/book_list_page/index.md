---
title: Pagina dell'elenco dei libri
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Book_list_page
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Successivamente implementeremo la pagina dell'elenco dei libri. Questa pagina deve visualizzare un elenco di tutti i libri nel database insieme al loro autore, con ogni titolo di libro come collegamento ipertestuale alla pagina dettagliata del libro associato.

## Controller

La funzione del controller dell'elenco dei libri deve ottenere un elenco di tutti gli oggetti `Book` nel database, ordinarli, e passarli al template per il rendering.

Aprire **/controllers/bookController.js**. Trovare il metodo `book_list()` esportato e sostituirlo con il seguente codice.

```js
// Display list of all books.
exports.book_list = asyncHandler(async (req, res, next) => {
  const allBooks = await Book.find({}, "title author")
    .sort({ title: 1 })
    .populate("author")
    .exec();

  res.render("book_list", { title: "Book List", book_list: allBooks });
});
```

Il gestore della route chiama la funzione `find()` sul modello `Book`, scegliendo di restituire solo `title` e `author` poiché non abbiamo bisogno degli altri campi (restituirà anche i campi `_id` e virtuali), e ordina i risultati alfabeticamente per titolo utilizzando il metodo `sort()`.
Chiamiamo anche `populate()` su `Book`, specificando il campo `author`—ciò sostituirà l'ID dell'autore memorizzato del libro con i dettagli completi dell'autore.
`exec()` è poi concatenato a catena alla fine per eseguire la query e restituire una promessa.

Il gestore della route utilizza `await` per attendere la promessa, sospendendo l'esecuzione fino a quando non è risolta.
Se la promessa è soddisfatta, i risultati della query vengono salvati nella variabile `allBooks` e il gestore continua l'esecuzione.

La parte finale del gestore della route chiama `render()`, specificando il template **book_list** (.pug) e passando i valori per il `title` e `book_list` al template.

## Vista

Creare **/views/book_list.pug** e copiare il testo qui sotto.

```pug
extends layout

block content
  h1= title
  if book_list.length
    ul
      each book in book_list
        li
          a(href=book.url) !{book.title}
          |  (#{book.author.name})

  else
    p There are no books.
```

La vista estende il template base **layout.pug** e sovrascrive il `block` chiamato '**content**'. Visualizza il `title` passato dal controller (tramite il metodo `render()`) e itera attraverso la variabile `book_list` utilizzando la sintassi `each`-`in`. Viene creato un elemento di elenco per ogni libro che visualizza il titolo del libro come un collegamento alla pagina di dettaglio del libro seguito dal nome dell'autore.
Se non ci sono libri in `book_list` allora viene eseguita la clausola `else`, e viene visualizzato il testo 'Non ci sono libri'.

> [!NOTE]
> Usiamo `book.url` per fornire il collegamento al record di dettaglio per ciascun libro (abbiamo implementato questa route, ma non la pagina ancora). Questa è una proprietà virtuale del modello `Book` che utilizza il campo `_id` dell'istanza del modello per produrre un percorso URL univoco.

Di interesse qui è che ogni libro è definito come due righe, utilizzando il pipe per la seconda riga. Questo approccio è necessario perché se il nome dell'autore fosse sulla riga precedente allora farebbe parte del collegamento ipertestuale.

## Come appare?

Eseguire l'applicazione (vedere [Testing the routes](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/routes#testing_the_routes) per i comandi pertinenti) e aprire il browser su `http://localhost:3000/`. Poi selezionare il link _All books_. Se tutto è configurato correttamente, il sito dovrebbe apparire come nello screenshot seguente.

![Pagina dell'elenco dei libri - sito Express Local Library](new_book_list.png)

## Passi successivi

- Tornare a [Express Tutorial Parte 5: Visualizzazione dei dati della biblioteca](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data).
- Proseguire con il prossimo sottoarticolo della parte 5: [Pagina dell'elenco dei BookInstance](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/BookInstance_list_page).
