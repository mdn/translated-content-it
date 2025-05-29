---
title: Modulo Elimina Autore
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Delete_author_form
l10n:
  sourceCommit: 2c0f972d873ea2db5163dbcb12987847124751ad
---

Questo sottoarticolo mostra come definire una pagina per eliminare oggetti `Author`.

Come discusso nella sezione [disegno del modulo](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms#form_design), la nostra strategia sarà quella di consentire l'eliminazione solo di oggetti che non sono referenziati da altri oggetti (in questo caso, ciò significa che non permetteremo che un `Author` venga eliminato se è referenziato da un `Book`).
In termini di implementazione, questo significa che il modulo deve confermare che non ci siano libri associati prima che l'autore venga eliminato.
Se ci sono libri associati, dovrebbe mostrarli e dichiarare che devono essere eliminati prima che l'oggetto `Author` possa essere eliminato.

## Controller—rotte get

Apri **/controllers/authorController.js**. Trova il metodo controller esportato `author_delete_get()` e sostituiscilo con il seguente codice.

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
    author,
    author_books: allBooksByAuthor,
  });
});
```

Il controller ottiene l'id dell'istanza `Author` da eliminare dal parametro URL (`req.params.id`).
Usa `await` sulla promessa restituita da `Promise.all()` per aspettare asincronamente sul record dell'autore specificato e tutti i libri associati (in parallelo).
Quando entrambe le operazioni sono completate, renderizza la vista **author_delete.pug**, passando variabili per il `title`, `author` e `author_books`.

> [!NOTE]
> Se `findById()` non restituisce alcun risultato, l'autore non è nel database.
> In questo caso non c'è nulla da eliminare, quindi reindirizziamo immediatamente all'elenco di tutti gli autori.
>
> ```js
> if (author === null) {
>   // Nessun risultato.
>   res.redirect("/catalog/authors");
> }
> ```

## Controller—rotte post

Trova il metodo controller esportato `author_delete_post()`, e sostituiscilo con il seguente codice.

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
      author,
      author_books: allBooksByAuthor,
    });
    return;
  }
  // Author has no books. Delete object and redirect to the list of authors.
  await Author.findByIdAndDelete(req.body.authorid);
  res.redirect("/catalog/authors");
});
```

Per prima cosa, verifichiamo che sia stato fornito un id (questo viene inviato tramite i parametri del corpo del modulo, piuttosto che usando la versione nell'URL).
Poi otteniamo l'autore e i suoi libri associati nello stesso modo della rotta `GET`.
Se non ci sono libri, allora eliminiamo l'oggetto autore e reindirizziamo all'elenco di tutti gli autori.
Se ci sono ancora libri, allora ri-renderizziamo semplicemente il modulo, passando l'autore e l'elenco dei libri da eliminare.

> [!NOTE]
> Potremmo verificare se la chiamata a `findById()` restituisce qualche risultato e, in caso contrario, renderizzare immediatamente l'elenco di tutti gli autori.
> Abbiamo lasciato il codice com'è sopra per brevità (restituirà comunque l'elenco degli autori se l'id non viene trovato, ma ciò accadrà dopo `findByIdAndDelete()`).

## Vista

Crea **/views/author_delete.pug** e copia il testo sottostante.

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

La vista estende il modello di layout, sovrascrivendo il blocco denominato `content`. In cima mostra i dettagli dell'autore.
Include quindi una dichiarazione condizionale basata sul numero di **`author_books`** (le clausole `if` e `else`).

- Se _ci sono_ libri associati all'autore, la pagina elenca i libri e dichiara che questi devono essere eliminati prima che questo `Author` possa essere eliminato.
- Se _non ci sono_ libri, la pagina visualizza un prompt di conferma.
- Se viene cliccato il pulsante **Delete**, l'id dell'autore viene inviato al server in una richiesta `POST` e il record di quell'autore sarà eliminato.

## Aggiungere un controllo di eliminazione

Successivamente, aggiungeremo un controllo **Delete** alla vista _dettaglio Autore_ (la pagina dei dettagli è un buon posto da cui eliminare un record).

> [!NOTE]
> In un'implementazione completa, il controllo sarebbe reso visibile solo agli utenti autorizzati.
> Tuttavia, a questo punto non abbiamo un sistema di autorizzazione in atto!

Apri la vista **author_detail.pug** e aggiungi le seguenti righe in fondo.

```pug
hr
p
  a(href=author.url+'/delete') Delete author
```

Il controllo dovrebbe ora apparire come un link, come mostrato di seguito sulla pagina _dettaglio Autore_.

![La sezione Dettagli Autore dell'applicazione Bibliotecario locale. La colonna sinistra ha una barra di navigazione verticale. La sezione di destra contiene i dettagli dell'autore con un'intestazione che ha il nome dell'Autore seguito dalle date di vita dell'autore e elenca i libri scritti dall'autore sotto di essa. C'è un pulsante etichettato 'Elimina Autore' in fondo.](locallibary_express_author_detail_delete.png)

## Come appare?

Esegui l'applicazione e apri il browser su `http://localhost:3000/`.
Poi seleziona il link _All authors_ e quindi seleziona un autore specifico. Infine seleziona il link _Delete author_.

Se l'autore non ha libri, ti verrà presentata una pagina come questa.
Dopo aver premuto delete, il server eliminerà l'autore e reindirizzerà all'elenco degli autori.

![La sezione Elimina Autore dell'applicazione Bibliotecario locale di un autore che non ha libri. La colonna sinistra ha una barra di navigazione verticale. La sezione di destra contiene il nome dell'autore e le date di vita. C'è la domanda "Vuoi davvero eliminare questo autore" con un pulsante etichettato 'Delete'.](locallibary_express_author_delete_nobooks.png)

Se l'autore ha libri, ti verrà presentata una vista come la seguente.
Puoi quindi eliminare i libri dalle loro pagine di dettaglio (una volta che quel codice è implementato!).

![La sezione Elimina Autore dell'applicazione Bibliotecario locale di un autore che ha libri sotto il suo nome. La sezione contiene il nome e le date di vita dell'autore. C'è una dichiarazione che dice "Elimina i seguenti libri prima di tentare di eliminare questo autore", seguita dai libri dell'autore. L'elenco include i titoli di ciascun libro, come link, seguiti da una breve descrizione in testo semplice.](locallibary_express_author_delete_withbooks.png)

> [!NOTE]
> Le altre pagine per eliminare gli oggetti possono essere implementate in modo molto simile.
> Lasciamo questo come una sfida.

## Prossimi passi

- Torna a [Express Tutorial Parte 6: Lavorare con i moduli](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms).
- Procedi all'ultimo sottoarticolo della parte 6: [Modulo Aggiorna Libro](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Update_Book_form).
