---
title: Formattazione delle date utilizzando luxon
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Date_formatting_using_moment
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Il rendering predefinito delle date dai nostri modelli è molto brutto: _Mon Apr 10 2020 15:49:58 GMT+1100 (AUS Eastern Daylight Time)_. In questa sezione mostreremo come è possibile aggiornare la pagina _Elenco delle istanze dei libri_ della sezione precedente per presentare il campo `due_date` in un formato più amichevole: 10 Apr 2023.

L'approccio che utilizzeremo è creare una proprietà virtuale nel nostro modello `BookInstance` che restituisce la data formattata. Eseguirremo la formattazione effettiva utilizzando [luxon](https://www.npmjs.com/package/luxon), una libreria potente, moderna e amichevole per l'analisi, la convalida, la manipolazione, la formattazione e la localizzazione delle date.

> [!NOTE]
> È possibile utilizzare _luxon_ per formattare le stringhe direttamente nei nostri modelli Pug, oppure potremmo formattare la stringa in diversi altri punti. Utilizzare una proprietà virtuale ci consente di ottenere la data formattata esattamente allo stesso modo in cui otteniamo attualmente `due_date`.

## Installare luxon

Inserisci il seguente comando nella root del progetto:

```bash
npm install luxon
```

## Creare la proprietà virtuale

1. Apri **./models/bookinstance.js**.
2. In cima alla pagina, importa _luxon_.

   ```js
   const { DateTime } = require("luxon");
   ```

Aggiungi la proprietà virtuale `due_back_formatted` subito dopo la proprietà URL.

```js
BookInstanceSchema.virtual("due_back_formatted").get(function () {
  return DateTime.fromJSDate(this.due_back).toLocaleString(DateTime.DATE_MED);
});
```

> [!NOTE]
> Luxon può importare stringhe in molti formati ed esportare sia in formati predefiniti che in formati liberi. In questo caso, usiamo `fromJSDate()` per importare una stringa di data JavaScript e `toLocaleString()` per mostrare la data nel formato `DATE_MED` in inglese: Apr 10th, 2023.
> Per informazioni su altri formati e internazionalizzazione delle stringhe di data, consulta la documentazione di Luxon sulla [formattazione](https://github.com/moment/luxon/blob/master/docs/formatting.md#formatting).

## Aggiornare la vista

Apri **/views/bookinstance_list.pug** e sostituisci `due_back` con `due_back_formatted`.

```pug
      if val.status != 'Available'
        //span  (Due: #{val.due_back} )
        span  (Due: #{val.due_back_formatted} )
```

Ecco fatto. Se vai su _Tutte le istanze dei libri_ nella barra laterale, dovresti ora vedere che tutte le date di scadenza sono molto più attraenti!

## Prossimi passi

- Ritorna a [Express Tutorial Parte 5: Visualizzazione dei dati della biblioteca](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data).
- Procedi al subarticolo successivo della parte 5: [Sfida della pagina dell'elenco degli autori e della pagina dell'elenco dei generi](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Author_list_page).
