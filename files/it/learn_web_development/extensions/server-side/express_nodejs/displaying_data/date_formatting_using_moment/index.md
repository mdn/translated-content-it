---
title: Formattazione delle date utilizzando luxon
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Date_formatting_using_moment
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

La visualizzazione predefinita delle date dai nostri modelli è molto poco attraente: _Mon Apr 10 2020 15:49:58 GMT+1100 (AUS Eastern Daylight Time)_. In questa sezione mostreremo come lei può aggiornare la pagina _BookInstance List_ della sezione precedente per presentare il campo `due_date` in un formato più gradevole: Apr 10th, 2023.

L'approccio che utilizzeremo consiste nel creare una proprietà virtuale nel nostro modello `BookInstance` che restituisca la data formattata. Faremo la formattazione effettiva utilizzando [luxon](https://www.npmjs.com/package/luxon), una libreria potente, moderna e intuitiva per l'analisi, la convalida, la manipolazione, la formattazione e la localizzazione delle date.

> [!NOTE]
> È possibile utilizzare _luxon_ per formattare le stringhe direttamente nei nostri modelli Pug, o potremmo formattare la stringa in vari altri luoghi. Utilizzare una proprietà virtuale ci consente di ottenere la data formattata nello stesso modo in cui otteniamo attualmente `due_date`.

## Installare luxon

Inserisca il seguente comando nella root del progetto:

```bash
npm install luxon
```

## Creare la proprietà virtuale

1. Aprire **./models/bookinstance.js**.
2. All'inizio della pagina, importare _luxon_.

   ```js
   const { DateTime } = require("luxon");
   ```

Aggiungere la proprietà virtuale `due_back_formatted` subito dopo la proprietà URL.

```js
BookInstanceSchema.virtual("due_back_formatted").get(function () {
  return DateTime.fromJSDate(this.due_back).toLocaleString(DateTime.DATE_MED);
});
```

> [!NOTE]
> Luxon può importare stringhe in molti formati ed esportarle in formati sia predefiniti che liberi. In questo caso, utilizziamo `fromJSDate()` per importare una stringa di data JavaScript e `toLocaleString()` per esportare la data nel formato `DATE_MED` in inglese: Apr 10th, 2023.
> Per informazioni su altri formati e sull'internazionalizzazione delle stringhe di data, consultare la documentazione Luxon sulla [formattazione](https://github.com/moment/luxon/blob/master/docs/formatting.md#formatting).

## Aggiornare la vista

Aprire **/views/bookinstance_list.pug** e sostituire `due_back` con `due_back_formatted`.

```pug
      if val.status != 'Available'
        //span  (Due: #{val.due_back} )
        span  (Due: #{val.due_back_formatted} )
```

Questo è tutto. Se accede a _Tutte le istanze del libro_ nella barra laterale, dovrebbe ora vedere tutte le date di scadenza in un formato molto più attraente!

## Prossimi passi

- Torni a [Express Tutorial Part 5: Visualizzazione dei dati della biblioteca](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data).
- Proceda al prossimo sotto-articolo della parte 5: [Pagina elenco autori e sfida pagina elenco dei generi](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Author_list_page).
