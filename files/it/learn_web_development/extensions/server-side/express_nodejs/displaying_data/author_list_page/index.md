---
title: Pagina dell'elenco autori e Pagina dell'elenco generi
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Author_list_page
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

La pagina dell'elenco autori deve visualizzare un elenco di tutti gli autori nel database, con il nome di ciascun autore collegato alla sua pagina di dettaglio associata. La data di nascita e la data di morte devono essere elencate dopo il nome sulla stessa riga.

## Controller

La funzione del controller dell'elenco autori deve ottenere un elenco di tutte le istanze di `Author`, e successivamente passarle al template per il rendering.

Aprire **/controllers/authorController.js**. Trovare il metodo `author_list()` esportato vicino all'inizio del file e sostituirlo con il seguente codice.

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

La funzione del controller di routing segue lo stesso schema delle altre pagine di elenco.
Definisce una query sul modello `Author`, utilizzando la funzione `find()` per ottenere tutti gli autori, e il metodo `sort()` per ordinarli alfabeticamente per `family_name`.
`exec()` è concatenato alla fine per eseguire la query e restituire una promessa che la funzione può `await`.

Una volta che la promessa è stata soddisfatta, il gestore della rotta esegue il rendering del template **author_list**(.pug), passando il `title` della pagina e l'elenco degli autori (`allAuthors`) utilizzando le chiavi del template.

## Vista

Creare **/views/author_list.pug** e sostituire il suo contenuto con il testo seguente.

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

Eseguire l'applicazione e aprire il browser su `http://localhost:3000/`. Poi selezionare il link _Tutti gli autori_. Se tutto è stato configurato correttamente, la pagina dovrebbe apparire come lo screenshot seguente.

![Pagina elenco autori - Sito Express Local Library](locallibary_express_author_list.png)

> [!NOTE]
> L'aspetto delle date di durata della vita dell'autore è brutto! Può migliorarlo utilizzando lo [stesso approccio](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Date_formatting_using_moment) che abbiamo usato per l'elenco di `BookInstance` (aggiungendo la proprietà virtuale per la durata della vita al modello `Author`).
>
> Tuttavia, poiché è possibile che l'autore non sia morto o che abbia dati di nascita/morte mancanti, in questo caso dobbiamo ignorare le date mancanti o i riferimenti a proprietà inesistenti. Un modo per affrontare ciò è restituire una data formattata o una stringa vuota, a seconda che la proprietà sia definita. Ad esempio:
>
> `return this.date_of_birth ? DateTime.fromJSDate(this.date_of_birth).toLocaleString(DateTime.DATE_MED) : '';`

## Pagina elenco generi—sfida!

In questa sezione deve implementare la propria pagina dell'elenco generi. La pagina deve visualizzare un elenco di tutti i generi nel database, con ciascun genere collegato alla sua pagina di dettaglio associata. Un'immagine dei risultati attesi è mostrata sotto.

![Elenco Generi - Sito Express Local Library](locallibary_express_genre_list.png)

La funzione del controller dell'elenco generi deve ottenere un elenco di tutte le istanze di `Genre`, e successivamente passarle al template per il rendering.

1. Dovrà modificare `genre_list()` in **/controllers/genreController.js**.
2. L'implementazione è quasi esattamente la stessa del controller `author_list()`.

   - Ordina i risultati per nome, in ordine ascendente.

3. Il template da eseguire dovrebbe essere chiamato **genre_list.pug**.
4. Al template da eseguire devono essere passate le variabili `title` ('Elenco Generi') e `genre_list` (l'elenco dei generi restituiti dal suo callback `Genre.find()`).
5. La vista dovrebbe corrispondere allo screenshot/requisiti sopra (dovrebbe avere una struttura/formato molto simile alla vista dell'elenco autori, eccetto per il fatto che i generi non hanno date).

## Passaggi successivi

Tornare a [Tutorial Express Parte 5: Visualizzazione dati della biblioteca](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data).

Procedere al prossimo sottoarticolo della parte 5: [Pagina di dettaglio del genere](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Genre_detail_page).
