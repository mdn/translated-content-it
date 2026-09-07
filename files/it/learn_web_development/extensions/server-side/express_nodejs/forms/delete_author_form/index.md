---
title: Modulo per eliminare un autore
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Delete_author_form
l10n:
  sourceCommit: b5ee197a87ea18acbc4dd9544efa8c0e46253785
---

Questo sottoarticolo mostra come definire una pagina per eliminare oggetti `Author`.

Come discusso nella sezione [progettazione dei moduli](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms#form_design), la strategia consiste nel consentire l'eliminazione solo degli oggetti che non sono referenziati da altri oggetti (in questo caso, non sarà possibile eliminare un `Author` se è referenziato da un `Book`).
In termini di implementazione, ciò significa che il modulo deve confermare che non esistano libri associati prima di eliminare l'autore.
Se esistono libri associati, il modulo dovrebbe visualizzarli e indicare che devono essere eliminati prima che sia possibile eliminare l'oggetto `Author`.

## Controller — route get

Aprire **/controllers/authorController.js**. Individuare il metodo controller esportato `author_delete_get()` e sostituirlo con il codice seguente.

```js
// Display Author delete form on GET.
exports.author_delete_get = async (req, res, next) => {
  // Get details of author and all their books (in parallel)
  const [author, allBooksByAuthor] = await Promise.all([
    Author.findById(req.params.id).exec(),
    Book.find({ author: req.params.id }, "title summary").exec(),
  ]);

  if (author === null) {
    // No results.
    res.redirect("/catalog/authors");
    return;
  }

  res.render("author_delete", {
    title: "Delete Author",
    author,
    author_books: allBooksByAuthor,
  });
};
```

Il controller ottiene l'id dell'istanza `Author` da eliminare dal parametro URL (`req.params.id`).
Usa `await` sulla promessa restituita da `Promise.all()` per attendere in modo asincrono il record dell'autore specificato e tutti i libri associati (in parallelo).
Quando entrambe le operazioni sono state completate, esegue il rendering della vista **author_delete.pug**, passando le variabili per `title`, `author` e `author_books`.

> [!NOTE]
> Se `findById()` non restituisce risultati, l'autore non è nel database.
> In questo caso non c'è nulla da eliminare, quindi viene eseguito immediatamente il reindirizzamento all'elenco di tutti gli autori.
>
> ```js
> if (author === null) {
>   // No results.
>   res.redirect("/catalog/authors");
>   return;
> }
> ```

## Controller — route post

Individuare il metodo controller esportato `author_delete_post()` e sostituirlo con il codice seguente.

```js
// Handle Author delete on POST.
exports.author_delete_post = async (req, res, next) => {
  // Get details of author and all their books (in parallel)
  const [author, allBooksByAuthor] = await Promise.all([
    Author.findById(req.params.id).exec(),
    Book.find({ author: req.params.id }, "title summary").exec(),
  ]);

  if (allBooksByAuthor.length > 0) {
    // Author has books. Render in same way as for GET route.
    res.render("author_delete", {
      title: "Delete Author",
      author,
      author_books: allBooksByAuthor,
    });
    return;
  }
  // Author has no books. Delete object and redirect to the list of authors.
  await Author.findByIdAndDelete(req.body.authorid);
  res.redirect("/catalog/authors");
};
```

Prima viene convalidato che sia stato fornito un id (questo viene inviato tramite i parametri del corpo del modulo, invece di usare la versione nell'URL).
Quindi vengono ottenuti l'autore e i libri associati nello stesso modo della route `GET`.
Se non ci sono libri, viene eliminato l'oggetto autore e viene eseguito il reindirizzamento all'elenco di tutti gli autori.
Se ci sono ancora libri, il modulo viene semplicemente sottoposto nuovamente a rendering, passando l'autore e l'elenco dei libri da eliminare.

> [!NOTE]
> Si potrebbe verificare se la chiamata a `findById()` restituisce un risultato e, in caso contrario, eseguire immediatamente il rendering dell'elenco di tutti gli autori.
> Per brevità il codice è stato lasciato come sopra (restituirà comunque l'elenco degli autori se l'id non viene trovato, ma questo avverrà dopo `findByIdAndDelete()`).

## Vista

Creare **/views/author_delete.pug** e copiare il testo seguente.

```pug
extends layout

block content

  h1 #{title}: #{author.name}
  p= author.lifespan

  if author_books.length

    p #[strong Delete the following books before attempting to delete this author.]
    div(style='margin-left:20px;margin-top:20px')
      h4 Books
      dl
        each book in author_books
          dt
            a(href=book.url) #{book.title}
          dd #{book.summary}

  else
    p Do you really want to delete this Author?

    form(method='POST')
      div.form-group
        input#authorid.form-control(type='hidden', name='authorid', value=author._id )

      button.btn.btn-primary(type='submit') Delete
```

La vista estende il template di layout, sovrascrivendo il blocco denominato `content`. Nella parte superiore visualizza i dettagli dell'autore.
Include quindi un'istruzione condizionale basata sul numero di **`author_books`** (le clausole `if` ed `else`).

- Se _ci sono_ libri associati all'autore, la pagina elenca i libri e indica che devono essere eliminati prima di poter eliminare questo `Author`.
- Se _non ci sono_ libri, la pagina visualizza una richiesta di conferma.
- Se viene fatto clic sul pulsante **Delete**, l'id dell'autore viene inviato al server in una richiesta `POST` e il record di quell'autore verrà eliminato.

## Aggiungere un controllo di eliminazione

Successivamente verrà aggiunto un controllo **Delete** alla vista dei _dettagli dell'autore_ (la pagina dei dettagli è un buon punto da cui eliminare un record).

> [!NOTE]
> In un'implementazione completa il controllo sarebbe visibile solo agli utenti autorizzati.
> Tuttavia, a questo punto non è ancora presente un sistema di autorizzazione.

Aprire la vista **author_detail.pug** e aggiungere le righe seguenti in fondo.

```pug
hr
p
  a(href=author.url+'/delete') Delete author
```

Il controllo dovrebbe ora apparire come collegamento, come mostrato di seguito nella pagina dei _dettagli dell'autore_.

![La sezione dei dettagli dell'autore dell'applicazione Local library. La colonna sinistra contiene una barra di navigazione verticale. La sezione destra contiene i dettagli dell'autore, con un'intestazione che riporta il nome dell'autore seguito dalle date di nascita e morte, e sotto l'elenco dei libri scritti dall'autore. In fondo è presente un pulsante denominato "Delete Author".](locallibary_express_author_detail_delete.png)

## Come appare?

Eseguire l'applicazione e aprire il browser all'indirizzo `http://localhost:3000/`.
Quindi selezionare il collegamento _All authors_ e scegliere un autore specifico. Infine, selezionare il collegamento _Delete author_.

Se l'autore non ha libri, verrà visualizzata una pagina simile a questa.
Dopo aver premuto elimina, il server eliminerà l'autore e reindirizzerà all'elenco degli autori.

![La sezione Delete Author dell'applicazione Local library per un autore che non ha libri. La colonna sinistra contiene una barra di navigazione verticale. La sezione destra contiene il nome dell'autore e le date di nascita e morte. È presente la domanda "Do you really want to delete this author" con un pulsante denominato "Delete".](locallibary_express_author_delete_nobooks.png)

Se l'autore ha dei libri, verrà invece visualizzata una vista simile alla seguente.
Sarà quindi possibile eliminare i libri dalle rispettive pagine dei dettagli (una volta implementato quel codice!).

![La sezione Delete Author dell'applicazione Local library per un autore che ha libri a proprio nome. La sezione contiene il nome dell'autore e le sue date di nascita e morte. È presente un messaggio che recita "Delete the following books before attempting to delete this author", seguito dai libri dell'autore. L'elenco include i titoli di ciascun libro, come collegamenti, seguiti da una breve descrizione in testo normale.](locallibary_express_author_delete_withbooks.png)

> [!NOTE]
> Le altre pagine per eliminare oggetti possono essere implementate in modo molto simile.
> Questo viene lasciato come esercizio.

## Passaggi successivi

- Tornare a [Tutorial su Express — Parte 6: utilizzo dei moduli](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms).
- Proseguire con il sottoarticolo finale della parte 6: [Modulo per aggiornare un libro](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Update_Book_form).
