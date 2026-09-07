---
title: Formattazione delle date con luxon
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Date_formatting_using_moment
l10n:
  sourceCommit: e1e7e2ac2cb1e40293c32c24bc0667905e9a7a04
---

Il rendering predefinito delle date dai nostri modelli è piuttosto brutto: _Mon Apr 10 2020 15:49:58 GMT+1100 (AUS Eastern Daylight Time)_. In questa sezione verrà mostrato come aggiornare la pagina _BookInstance List_ della sezione precedente per presentare il campo `due_date` in un formato più gradevole: Apr 10th, 2023.

L'approccio utilizzato consiste nel creare una proprietà virtuale nel modello `BookInstance` che restituisca la data formattata. La formattazione effettiva verrà eseguita usando [luxon](https://www.npmjs.com/package/luxon), una libreria potente, moderna e intuitiva per analizzare, convalidare, manipolare, formattare e localizzare le date.

> [!NOTE]
> È possibile utilizzare _luxon_ per formattare le stringhe direttamente nei template Pug oppure formattare la stringa in diversi altri punti. L'uso di una proprietà virtuale consente di ottenere la data formattata esattamente nello stesso modo in cui si ottiene attualmente `due_date`.

## Installare luxon

Inserire il seguente comando nella radice del progetto:

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
> Luxon può importare stringhe in molti formati ed esportarle sia in formati predefiniti sia in formati liberi. In questo caso viene usato `fromJSDate()` per importare una stringa di data JavaScript e `toLocaleString()` per restituire la data nel formato `DATE_MED` in inglese: Apr 10th, 2023.
> Per informazioni su altri formati e sull'internazionalizzazione delle stringhe di data, consultare la documentazione di Luxon sulla [formattazione](https://github.com/moment/luxon/blob/master/docs/formatting.md#formatting).

## Aggiornare la vista

Aprire **/views/bookinstance_list.pug** e sostituire `due_back` con `due_back_formatted`.

```pug
      if val.status != 'Available'
        //span  (Due: #{val.due_back} )
        span  (Due: #{val.due_back_formatted} )
```

Questo è tutto. Selezionando _All book-instances_ nella barra laterale, ora tutte le date di scadenza dovrebbero apparire molto più gradevoli!

## Passaggi successivi

- Tornare a [Tutorial su Express Parte 5: Visualizzazione dei dati della biblioteca](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data).
- Proseguire al sottoarticolo successivo della parte 5: [Sfida: pagina dell'elenco degli autori e pagina dell'elenco dei generi](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Author_list_page).
