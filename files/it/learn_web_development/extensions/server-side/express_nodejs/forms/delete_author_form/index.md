---
title: Modulo di Eliminazione Autore
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Delete_author_form
l10n:
  sourceCommit: f2dc3d5367203c860cf1a71ce0e972f018523849
---

Questo sottoarticolo mostra come definire una pagina per eliminare oggetti `Author`.

Come discusso nella sezione [progettazione del modulo](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms#form_design), la nostra strategia sarà di consentire l'eliminazione solo di oggetti che non sono referenziati da altri oggetti (in questo caso ciò significa che non permetteremo che un `Author` venga eliminato se è referenziato da un `Book`).
In termini di implementazione ciò significa che il modulo deve confermare che non ci siano libri associati prima che l'autore venga eliminato.
Se ci sono libri associati, dovrebbe mostrarli e indicare che devono essere eliminati prima che l'oggetto `Author` possa essere eliminato.

## Controller—route get

Aprire **/controllers/authorController.js**. Trovare il metodo del controller esportato `author_delete_get()` e sostituirlo con il seguente codice.

```js
// Display Author delete form on GET.
exports.author_delete_get = asyncHandler(async (req, res, next) => {
  // Get details of author and all their books (in parallel)
  const [author, allBooksByAuthor] = await Promise.all([
    Author.findById(req.params.id).exec(),
    Book.find({ author: req.params.id }, "title summary").exec(),
  ]);

  if (author === null) {
    // No results.
    res.redirect("/catalog/authors");
  }

  res.render("author_delete", {
    title: "Delete Author",
    author: author,
    author_books: allBooksByAuthor,
  });
});
```

Il controller ottiene l'id dell'istanza `Author` da eliminare dal parametro URL (`req.params.id`).
Utilizza `await` sulla promessa restituita da `Promise.all()` per attendere asincronamente sul record dell'autore specificato e tutti i libri associati (in parallelo).
Quando entrambe le operazioni sono completate, viene renderizzata la vista **author_delete.pug**, passando variabili per il `title`, `author`, e `author_books`.

> [!NOTE]
> Se `findById()` non restituisce risultati, l'autore non è nel database.
> In questo caso non c'è nulla da eliminare, quindi reindirizziamo immediatamente alla lista di tutti gli autori.
>
> ```js
> if (author === null) {
>   // Nessun risultato.
>   res.redirect("/catalog/authors");
> }
> ```

## Controller—route post

Trovare il metodo del controller esportato `author_delete_post()` e sostituirlo con il seguente codice.

```js
// Handle Author delete on POST.
exports.author_delete_post = asyncHandler(async (req, res, next) => {
  // Get details of author and all their books (in parallel)
  const [author, allBooksByAuthor] = await Promise.all([
    Author.findById(req.params.id).exec(),
    Book.find({ author: req.params.id }, "title summary").exec(),
  ]);

  if (allBooksByAuthor.length > 0) {
    // Author has books. Render in same way as for GET route.
    res.render("author_delete", {
      title: "Delete Author",
      author: author,
      author_books: allBooksByAuthor,
    });
    return;
  }
  // Author has no books. Delete object and redirect to the list of authors.
  await Author.findByIdAndDelete(req.body.authorid);
  res.redirect("/catalog/authors");
});
```

Prima validiamo che sia stato fornito un id (questo viene inviato tramite i parametri del corpo del modulo, anziché usare la versione nell'URL).
Poi otteniamo l'autore e i suoi libri associati nello stesso modo del route `GET`.
Se non ci sono libri, allora eliminiamo l'oggetto autore e reindirizziamo alla lista di tutti gli autori.
Se ci sono ancora libri, allora semplicemente ricarichiamo il modulo, passando l'autore e la lista dei libri da eliminare.

> [!NOTE]
> Potremmo verificare se la chiamata a `findById()` restituisce un risultato e, in caso contrario, visualizzare immediatamente la lista di tutti gli autori.
> Abbiamo lasciato il codice com'è sopra per brevità (restituirà comunque la lista degli autori se l'id non viene trovato, ma ciò accadrà dopo `findByIdAndDelete()`).

## Vista

Creare **/views/author_delete.pug** e copiare il testo qui sotto.

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

La vista estende il template di layout, sovrascrivendo il blocco denominato `content`. In cima visualizza i dettagli dell'autore.
Poi include un'istruzione condizionale basata sul numero di **`author_books`** (le clausole `if` e `else`).

- Se _ci sono_ libri associati all'autore, la pagina elenca i libri e afferma che questi devono essere eliminati prima che l'autore possa essere eliminato.
- Se _non ci sono_ libri, la pagina visualizza un prompt di conferma.
- Se il pulsante **Delete** viene cliccato, allora l'id dell'autore viene inviato al server in una richiesta `POST` e il record di quell'autore verrà eliminato.

## Aggiungere un controllo di eliminazione

Successivamente aggiungeremo un controllo **Delete** alla vista _dettaglio Autore_ (la pagina di dettaglio è un buon posto da cui eliminare un record).

> [!NOTE]
> In una implementazione completa, il controllo sarebbe visibile solo agli utenti autorizzati.
> Tuttavia, a questo punto, non abbiamo ancora un sistema di autorizzazione!

Aprire la vista **author_detail.pug** e aggiungere le seguenti righe in fondo.

```pug
hr
p
  a(href=author.url+'/delete') Delete author
```

Il controllo ora dovrebbe apparire come un link, come mostrato di seguito nella pagina _dettaglio Autore_.

![La sezione dei dettagli Autore dell'applicazione Biblioteca Locale. La colonna sinistra ha una barra di navigazione verticale. La sezione destra contiene i dettagli dell'autore con un titolo che ha il nome dell'autore seguito dalle date di vita dell'autore e sotto elenca i libri scritti dall'autore. C'è un pulsante etichettato 'Delete Author' in basso.](locallibary_express_author_detail_delete.png)

## Come si presenta?

Eseguire l'applicazione e aprire il browser a `http://localhost:3000/`.
Quindi selezionare il link _Tutti gli autori_ e poi selezionare un autore particolare. Infine selezionare il link _Elimina autore_.

Se l'autore non ha libri, vi verrà presentata una pagina come questa.
Dopo aver premuto elimina, il server eliminerà l'autore e reindirizzerà alla lista degli autori.

![La sezione Eliminare Autore dell'applicazione Biblioteca Locale di un autore che non ha alcun libro. La colonna sinistra ha una barra di navigazione verticale. La sezione destra contiene il nome e le date di vita dell'autore. C'è la domanda "Vuoi davvero eliminare questo autore?" con un pulsante etichettato 'Delete'.](locallibary_express_author_delete_nobooks.png)

Se l'autore ha libri, allora vi verrà presentata una vista come la seguente.
Si possono quindi eliminare i libri dalle loro pagine di dettaglio (una volta che quel codice è implementato!).

![La sezione Eliminare Autore dell'applicazione Biblioteca Locale di un autore che ha libri a suo nome. La sezione contiene il nome e le date di vita dell'autore. C'è un'affermazione che dice "Elimina i seguenti libri prima di tentare di eliminare questo autore" seguita dai libri dell'autore. La lista include i titoli di ciascun libro, come link, seguiti da una breve descrizione in testo semplice.](locallibary_express_author_delete_withbooks.png)

> [!NOTE]
> Le altre pagine per eliminare oggetti possono essere implementate nello stesso modo.
> Abbiamo lasciato questo come una sfida.

## Prossimi passi

- Torna a [Express Tutorial Parte 6: Lavorare con i form](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms).
- Procedi all'ultimo sottoarticolo della parte 6: [Modulo di aggiornamento libro](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Update_Book_form).
