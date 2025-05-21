---
title: Pagina elenco degli autori e pagina elenco dei generi
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Author_list_page
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

La pagina elenco degli autori deve visualizzare un elenco di tutti gli autori nel database, con ogni nome di autore collegato alla relativa pagina di dettaglio dell'autore. La data di nascita e la data di morte devono essere elencate dopo il nome sulla stessa riga.

## Controller

La funzione del controller dell'elenco degli autori deve ottenere un elenco di tutte le istanze `Author`, e poi passarle al template per il rendering.

Aprire **/controllers/authorController.js**. Trovare il metodo `author_list()` esportato vicino alla parte superiore del file e sostituirlo con il codice seguente.

```js
// Display list of all Authors.
exports.author_list = asyncHandler(async (req, res, next) => {
  const allAuthors = await Author.find().sort({ family_name: 1 }).exec();
  res.render("author_list", {
    title: "Author List",
    author_list: allAuthors,
  });
});
```

La funzione del controller della route segue lo stesso schema delle altre pagine elenco. Definisce una query sul modello `Author`, utilizzando la funzione `find()` per ottenere tutti gli autori, e il metodo `sort()` per ordinarli alfabeticamente per `family_name`. `exec()` è concatenato alla fine per eseguire la query e restituire una promessa che la funzione può `await`.

Una volta che la promessa è soddisfatta, il gestore della route rende il template **author_list**(.pug), passando il `title` della pagina e l'elenco degli autori (`allAuthors`) utilizzando le chiavi del template.

## Vista

Creare **/views/author_list.pug** e sostituire il suo contenuto con il testo sottostante.

```pug
extends layout

block content
  h1= title

  if author_list.length
    ul
      each author in author_list
        li
          a(href=author.url) #{author.name}
          |  (#{author.date_of_birth} - #{author.date_of_death})
  else
    p There are no authors.
```

Eseguire l'applicazione e aprire il browser su `http://localhost:3000/`. Poi selezionare il link _All authors_. Se tutto è impostato correttamente, la pagina dovrebbe apparire come nello screenshot seguente.

![Pagina elenco degli autori - Sito Express Local Library](locallibary_express_author_list.png)

> [!NOTE]
> L'aspetto delle date del _lifespan_ dell'autore è brutto! È possibile migliorarlo usando lo [stesso approccio](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Date_formatting_using_moment) che abbiamo usato per l'elenco `BookInstance` (aggiungendo la proprietà virtuale per il lifespan al modello `Author`).
>
> Tuttavia, poiché l'autore potrebbe non essere deceduto o potrebbe avere dati di nascita/morte mancanti, in questo caso dobbiamo ignorare date mancanti o riferimenti a proprietà inesistenti. Un modo per affrontare questo è restituire una data formattata o una stringa vuota, a seconda che la proprietà sia definita. Ad esempio:
>
> `return this.date_of_birth ? DateTime.fromJSDate(this.date_of_birth).toLocaleString(DateTime.DATE_MED) : '';`

## Pagina elenco dei generi—sfida!

In questa sezione dovresti implementare la tua pagina elenco dei generi. La pagina dovrebbe visualizzare un elenco di tutti i generi nel database, con ogni genere collegato alla relativa pagina di dettaglio. Uno screenshot del risultato atteso è mostrato di seguito.

![Elenco dei generi - Sito Express Local Library](locallibary_express_genre_list.png)

La funzione del controller dell'elenco dei generi deve ottenere un elenco di tutte le istanze `Genre`, e poi passarle al template per il rendering.

1. Dovrai modificare `genre_list()` in **/controllers/genreController.js**.
2. L'implementazione è quasi identica a quella del controller `author_list()`.

   - Ordina i risultati per nome, in ordine crescente.

3. Il template da rendere dovrebbe essere denominato **genre_list.pug**.
4. Al template da rendere dovrebbero essere passate le variabili `title` ('Elenco dei Generi') e `genre_list` (l'elenco dei generi restituiti dal tuo callback `Genre.find()`).
5. La vista dovrebbe corrispondere allo screenshot/requisiti sopra (dovrebbe avere una struttura/formato molto simile alla vista dell'elenco autori, tranne per il fatto che i generi non hanno date).

## Prossimi passi

Torna a [Tutorial Express Parte 5: Visualizzazione dei dati della biblioteca](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data).

Procedi al prossimo sottoarticolo della parte 5: [Pagina dettagli del genere](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Genre_detail_page).
