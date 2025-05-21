---
title: Pagina elenco libri
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Book_list_page
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Successivamente implementeremo la pagina dell'elenco dei libri. Questa pagina deve mostrare un elenco di tutti i libri nel database insieme al loro autore, con ciascun titolo di libro che funge da collegamento ipertestuale alla relativa pagina di dettaglio del libro.

## Controller

La funzione del controller dell'elenco dei libri deve ottenere un elenco di tutti gli oggetti `Book` nel database, ordinarli e quindi passarli al template per il rendering.

Apri **/controllers/bookController.js**. Trova il metodo `book_list()` esportato del controller e sostituiscilo con il seguente codice.

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

Il gestore della route chiama la funzione `find()` sul modello `Book`, selezionando di restituire solo `title` e `author` poiché non abbiamo bisogno degli altri campi (restituirà anche i campi `_id` e virtuali), e ordinando i risultati alfabeticamente per titolo usando il metodo `sort()`. Chiamiamo anche `populate()` su `Book`, specificando il campo `author`: questo sostituirà l'id dell'autore del libro memorizzato con i dettagli completi dell'autore.
`exec()` viene quindi concatenato alla fine per eseguire la query e restituire una promessa.

Il gestore della route utilizza `await` per attendere la promessa, sospendendo l'esecuzione fino a quando non è risolta. Se la promessa viene soddisfatta, i risultati della query vengono salvati nella variabile `allBooks` e il gestore continua l'esecuzione.

La parte finale del gestore della route chiama `render()`, specificando il template **book_list** (.pug) e passando i valori per `title` e `book_list` nel template.

## Visualizzazione

Crea **/views/book_list.pug** e copia il testo qui sotto.

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

La vista estende il template base **layout.pug** e sovrascrive il `block` denominato '**content**'. Visualizza il `title` che abbiamo passato dal controller (tramite il metodo `render()`) e itera attraverso la variabile `book_list` utilizzando la sintassi `each`-`in`. Viene creato un elemento di elenco per ogni libro visualizzando il titolo del libro come link alla pagina di dettaglio del libro seguito dal nome dell'autore.
Se non ci sono libri nella `book_list`, allora viene eseguita la clausola `else`, che visualizza il testo 'Non ci sono libri'.

> [!NOTE]
> Utilizziamo `book.url` per fornire il link al record dettaglio per ciascun libro (abbiamo implementato questo percorso, ma non la pagina ancora). Questa è una proprietà virtuale del modello `Book` che utilizza il campo `_id` dell'istanza del modello per produrre un percorso URL univoco.

Di particolare interesse qui è che ciascun libro è definito su due righe, utilizzando la pipe per la seconda riga. Questo approccio è necessario perché se il nome dell'autore fosse sulla riga precedente allora farebbe parte del collegamento ipertestuale.

## Che aspetto ha?

Esegui l'applicazione (vedi [Testing the routes](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/routes#testing_the_routes) per i comandi pertinenti) e apri il tuo browser su `http://localhost:3000/`. Quindi seleziona il link _All books_. Se tutto è configurato correttamente, il tuo sito dovrebbe apparire come nello screenshot seguente.

![Pagina elenco libri - Sito Express Local Library](new_book_list.png)

## Prossimi passi

- Torna a [Express Tutorial Parte 5: Visualizzare i dati della biblioteca](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data).
- Procedi al sottocapitolo successivo della parte 5: [Pagina elenco BookInstance](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/BookInstance_list_page).
