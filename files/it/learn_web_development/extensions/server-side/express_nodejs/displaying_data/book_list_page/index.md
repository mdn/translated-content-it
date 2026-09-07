---
title: Pagina dell'elenco dei libri
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Book_list_page
l10n:
  sourceCommit: 8443cb34d9944d8eb8e2c5add598bec26ed6d21f
---

Ora verrà implementata la pagina dell'elenco dei libri. Questa pagina deve visualizzare un elenco di tutti i libri nel database insieme ai rispettivi autori, con ogni titolo del libro come collegamento ipertestuale alla pagina dei dettagli del libro associata.

## Controller

La funzione controller dell'elenco dei libri deve ottenere un elenco di tutti gli oggetti `Book` nel database, ordinarli e quindi passarli al template per il rendering.

Aprire **/controllers/bookController.js**. Individuare il metodo controller esportato `book_list()` e sostituirlo con il codice seguente.

```js
// Display list of all books.
exports.book_list = async (req, res, next) => {
  const allBooks = await Book.find({}, "title author")
    .sort({ title: 1 })
    .populate("author")
    .exec();

  res.render("book_list", { title: "Book List", book_list: allBooks });
};
```

Il gestore della route chiama la funzione `find()` sul modello `Book`, selezionando di restituire solo `title` e `author`, poiché gli altri campi non sono necessari (restituirà anche i campi `_id` e virtuali), e ordinando i risultati alfabeticamente per titolo mediante il metodo `sort()`.
Viene inoltre chiamato `populate()` su `Book`, specificando il campo `author`: questo sostituirà l'id dell'autore del libro memorizzato con i dettagli completi dell'autore.
Alla fine viene concatenato `exec()` per eseguire la query e restituire una promise.

Il gestore della route usa `await` per attendere la promise, sospendendo l'esecuzione finché non viene completata.
Se la promise viene soddisfatta, i risultati della query vengono salvati nella variabile `allBooks` e il gestore continua l'esecuzione.

La parte finale del gestore della route chiama `render()`, specificando il template **book_list** (.pug) e passando al template i valori di `title` e `book_list`.

## Vista

Creare **/views/book_list.pug** e copiare il testo seguente.

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

La vista estende il template di base **layout.pug** e sovrascrive il `block` denominato '**content**'. Visualizza il `title` passato dal controller (tramite il metodo `render()`) e itera sulla variabile `book_list` usando la sintassi `each`-`in`. Viene creato un elemento dell'elenco per ogni libro, visualizzando il titolo del libro come collegamento alla pagina dei dettagli del libro, seguito dal nome dell'autore.
Se non ci sono libri in `book_list`, viene eseguita la clausola `else` e viene visualizzato il testo "Non ci sono libri".

> [!NOTE]
> Viene usato `book.url` per fornire il collegamento al record dei dettagli di ciascun libro (questa route è stata implementata, ma non ancora la pagina). Si tratta di una proprietà virtuale del modello `Book` che usa il campo `_id` dell'istanza del modello per produrre un percorso URL univoco.

Un aspetto interessante è che ogni libro è definito su due righe, usando la barra verticale per la seconda riga. Questo approccio è necessario perché, se il nome dell'autore fosse sulla riga precedente, farebbe parte del collegamento ipertestuale.

## Che aspetto ha?

Eseguire l'applicazione (consultare [Testare le route](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/routes#testing_the_routes) per i comandi pertinenti) e aprire il browser su `http://localhost:3000/`. Quindi selezionare il collegamento _Tutti i libri_. Se tutto è configurato correttamente, il sito dovrebbe avere un aspetto simile allo screenshot seguente.

![Pagina dell'elenco dei libri - sito Express Local Library](new_book_list.png)

## Passaggi successivi

- Tornare a [Tutorial Express Parte 5: Visualizzazione dei dati della biblioteca](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data).
- Proseguire al sottoarticolo successivo della parte 5: [Pagina dell'elenco delle istanze di libro](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/BookInstance_list_page).
