---
title: Sfida pagina elenco autori e pagina elenco generi
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Author_list_page
l10n:
  sourceCommit: 8443cb34d9944d8eb8e2c5add598bec26ed6d21f
---

La pagina dell'elenco degli autori deve visualizzare un elenco di tutti gli autori nel database, con ogni nome di autore collegato alla relativa pagina di dettaglio dell'autore. La data di nascita e la data di morte devono essere elencate dopo il nome sulla stessa riga.

## Controller

La funzione controller dell'elenco degli autori deve ottenere un elenco di tutte le istanze di `Author`, quindi passarle al template per il rendering.

Aprire **/controllers/authorController.js**. Trovare il metodo controller esportato `author_list()` vicino all'inizio del file e sostituirlo con il codice seguente.

```js
// Display list of all Authors.
exports.author_list = async (req, res, next) => {
  const allAuthors = await Author.find().sort({ family_name: 1 }).exec();
  res.render("author_list", {
    title: "Author List",
    author_list: allAuthors,
  });
};
```

La funzione controller della route segue lo stesso schema delle altre pagine di elenco.
Definisce una query sul modello `Author`, utilizzando la funzione `find()` per ottenere tutti gli autori e il metodo `sort()` per ordinarli in ordine alfabetico in base a `family_name`.
Alla fine viene concatenato `exec()` per eseguire la query e restituire una promise che la funzione può gestire con `await`.

Una volta soddisfatta la promise, il gestore della route esegue il rendering del template **author_list**(.pug), passando il `title` della pagina e l'elenco degli autori (`allAuthors`) mediante chiavi del template.

## Vista

Creare **/views/author_list.pug** e sostituirne il contenuto con il testo seguente.

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

Eseguire l'applicazione e aprire il browser all'indirizzo `http://localhost:3000/`. Quindi selezionare il collegamento _All authors_. Se tutto è configurato correttamente, la pagina dovrebbe avere un aspetto simile allo screenshot seguente.

![Pagina elenco autori - sito Express Local Library](locallibary_express_author_list.png)

> [!NOTE]
> L'aspetto delle date della _lifespan_ dell'autore è sgradevole. È possibile migliorarlo usando lo [stesso approccio](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Date_formatting_using_moment) utilizzato per l'elenco `BookInstance` (aggiungendo la proprietà virtuale per la durata della vita al modello `Author`).
>
> Tuttavia, poiché l'autore potrebbe non essere morto oppure potrebbero mancare i dati di nascita/morte, in questo caso è necessario ignorare le date mancanti o i riferimenti a proprietà inesistenti. Un modo per gestire questo caso consiste nel restituire una data formattata oppure una stringa vuota, a seconda che la proprietà sia definita. Per esempio:
>
> `return this.date_of_birth ? DateTime.fromJSDate(this.date_of_birth).toLocaleString(DateTime.DATE_MED) : '';`

## Pagina elenco generi: sfida!

In questa sezione è necessario implementare una pagina di elenco dei generi. La pagina deve visualizzare un elenco di tutti i generi nel database, con ogni genere collegato alla relativa pagina di dettaglio. Di seguito è mostrato uno screenshot del risultato previsto.

![Elenco generi - sito Express Local Library](locallibary_express_genre_list.png)

La funzione controller dell'elenco dei generi deve ottenere un elenco di tutte le istanze di `Genre`, quindi passarle al template per il rendering.

1. Sarà necessario modificare `genre_list()` in **/controllers/genreController.js**.
2. L'implementazione è quasi identica al controller `author_list()`.
   - Ordinare i risultati per nome, in ordine crescente.

3. Il template di cui eseguire il rendering deve chiamarsi **genre_list.pug**.
4. Al template di cui eseguire il rendering devono essere passate le variabili `title` ('Genre List') e `genre_list` (l'elenco dei generi restituito da `Genre.find()`).
5. La vista deve corrispondere allo screenshot/ai requisiti sopra riportati (dovrebbe avere una struttura/formato molto simile alla vista dell'elenco degli autori, a eccezione del fatto che i generi non hanno date).

## Passaggi successivi

Tornare a [Tutorial Express - Parte 5: visualizzazione dei dati della libreria](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data).

Proseguire al sottoarticolo successivo della parte 5: [Pagina di dettaglio del genere](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Genre_detail_page).
