---
title: "Tutorial su Express, parte 6: Lavorare con i moduli"
short-title: "6: Lavorare con i moduli"
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/forms
l10n:
  sourceCommit: 8443cb34d9944d8eb8e2c5add598bec26ed6d21f
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data", "Learn_web_development/Extensions/Server-side/Express_Nodejs/deployment", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}

In questo tutorial verrà mostrato come lavorare con i moduli HTML in Express usando Pug. In particolare, verrà illustrato come scrivere moduli per creare, aggiornare ed eliminare documenti dal database del sito.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Completare tutti gli argomenti dei tutorial precedenti, incluso <a href="/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data">Tutorial su Express, parte 5: Visualizzare i dati della libreria</a>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere come scrivere moduli per ottenere dati dagli utenti e aggiornare il database con tali dati.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

Un [modulo HTML](/it/docs/Learn_web_development/Extensions/Forms) è un gruppo di uno o più campi/widget in una pagina web che può essere usato per raccogliere informazioni dagli utenti e inviarle a un server. I moduli sono un meccanismo flessibile per raccogliere l'input dell'utente perché sono disponibili controlli adatti all'inserimento di molti tipi diversi di dati: caselle di testo, caselle di controllo, pulsanti di opzione, selettori di data e così via. I moduli rappresentano inoltre un modo relativamente sicuro per condividere dati con il server, poiché consentono di inviare dati in richieste `POST` con protezione contro la falsificazione di richieste tra siti.

Lavorare con i moduli può essere complicato. Gli sviluppatori devono scrivere l'HTML del modulo, convalidare e sottoporre a sanitizzazione i dati inseriti sul server (e possibilmente anche nel browser), ripubblicare il modulo con messaggi di errore per informare gli utenti di eventuali campi non validi, gestire i dati dopo il loro invio corretto e infine rispondere all'utente in qualche modo per indicare l'esito positivo.

In questo tutorial verrà mostrato come le operazioni precedenti possono essere eseguite in _Express_. Durante il percorso, il sito web _LocalLibrary_ verrà esteso per consentire agli utenti di creare, modificare ed eliminare elementi della libreria.

> [!NOTE]
> Non è stato ancora illustrato come limitare particolari route agli utenti autenticati o autorizzati, quindi a questo punto qualsiasi utente potrà apportare modifiche al database.

### Moduli HTML

Iniziamo con una breve panoramica dei [moduli HTML](/it/docs/Learn_web_development/Extensions/Forms). Si consideri un semplice modulo HTML, con un singolo campo di testo per inserire il nome di una "squadra" e la relativa etichetta:

![Esempio di semplice campo del nome in un modulo HTML](form_example_name_field.png)

Il modulo è definito in HTML come una raccolta di elementi all'interno dei tag `<form>…</form>`, contenente almeno un elemento `input` di `type="submit"`.

```html
<form action="/team_name_url/" method="post">
  <label for="team_name">Enter name: </label>
  <input
    id="team_name"
    type="text"
    name="name_field"
    value="Default name for team." />
  <input type="submit" value="OK" />
</form>
```

Sebbene qui sia stato incluso un solo campo (di testo) per inserire il nome della squadra, un modulo _può_ contenere qualsiasi numero di altri elementi di input e le relative etichette. L'attributo `type` del campo definisce il tipo di widget che verrà visualizzato. Il `name` e l'`id` del campo vengono usati per identificare il campo in JavaScript/CSS/HTML, mentre `value` definisce il valore iniziale del campo quando viene visualizzato per la prima volta. L'etichetta della squadra corrispondente viene specificata usando il tag `label` (vedere "Enter name" sopra), con un campo `for` contenente il valore `id` dell'`input` associato.

L'input `submit` verrà visualizzato come pulsante (per impostazione predefinita): l'utente può premerlo per caricare sul server i dati contenuti dagli altri elementi di input (in questo caso, solo `team_name`). Gli attributi del modulo definiscono il `method` HTTP usato per inviare i dati e la destinazione dei dati sul server (`action`):

- `action`: la risorsa/URL a cui inviare i dati per l'elaborazione quando il modulo viene inviato. Se non viene impostato (o viene impostato su una stringa vuota), il modulo verrà inviato nuovamente all'URL della pagina corrente.
- `method`: il metodo HTTP usato per inviare i dati: `POST` o `GET`.
  - Il metodo `POST` deve essere sempre usato se i dati produrranno una modifica al database del server, perché può essere reso più resistente agli attacchi di falsificazione di richieste tra siti.
  - Il metodo `GET` deve essere usato solo per moduli che non modificano i dati dell'utente (ad esempio, un modulo di ricerca). È consigliato quando si desidera poter aggiungere ai segnalibri o condividere l'URL.

### Processo di gestione dei moduli

La gestione dei moduli usa tutte le stesse tecniche apprese per visualizzare informazioni sui modelli: la route invia la richiesta a una funzione controller che esegue le azioni richieste sul database, inclusa la lettura dei dati dai modelli, quindi genera e restituisce una pagina HTML. Ciò che rende le cose più complicate è che il server deve anche essere in grado di elaborare i dati forniti dall'utente e di visualizzare nuovamente il modulo con informazioni sugli errori in caso di problemi.

Di seguito viene mostrato un diagramma di flusso del processo per l'elaborazione delle richieste dei moduli, a partire da una richiesta per una pagina contenente un modulo (mostrata in verde):

![Diagramma di flusso dell'elaborazione delle richieste di moduli da parte del server web. Il browser richiede la pagina contenente il modulo inviando una richiesta HTTP GET. Il server crea un modulo predefinito vuoto e lo restituisce all'utente. L'utente compila o aggiorna il modulo, inviandolo tramite HTTP POST con i dati del modulo. Il server convalida i dati del modulo ricevuti. Se i dati forniti dall'utente non sono validi, il server ricrea il modulo con i dati inseriti dall'utente e i messaggi di errore e lo invia nuovamente all'utente affinché lo aggiorni e lo invii di nuovo tramite HTTP POST, quindi esegue nuovamente la convalida. Se i dati sono validi, il server esegue le azioni sui dati validi e reindirizza l'utente all'URL di successo.](web_server_form_handling.png)

Come mostrato nel diagramma precedente, le principali operazioni che il codice di gestione dei moduli deve eseguire sono:

1. Visualizzare il modulo predefinito la prima volta che viene richiesto dall'utente.
   - Il modulo può contenere campi vuoti, ad esempio quando viene creato un nuovo record, oppure può essere precompilato con valori iniziali, ad esempio quando viene modificato un record o sono disponibili valori iniziali predefiniti utili.

2. Ricevere i dati inviati dall'utente, solitamente in una richiesta HTTP `POST`.
3. Convalidare e sottoporre a sanitizzazione i dati.
4. Se alcuni dati non sono validi, visualizzare nuovamente il modulo, questa volta con gli eventuali valori compilati dall'utente e messaggi di errore per i campi problematici.
5. Se tutti i dati sono validi, eseguire le azioni richieste, ad esempio salvare i dati nel database, inviare un'email di notifica, restituire il risultato di una ricerca, caricare un file e così via.
6. Una volta completate tutte le azioni, reindirizzare l'utente a un'altra pagina.

Spesso il codice di gestione dei moduli viene implementato usando una route `GET` per la visualizzazione iniziale del modulo e una route `POST` allo stesso percorso per gestire la convalida e l'elaborazione dei dati del modulo. Questo è l'approccio che verrà usato in questo tutorial.

Express stesso non fornisce supporto specifico per le operazioni di gestione dei moduli, ma può usare middleware per elaborare i parametri `POST` e `GET` del modulo e per convalidarne/sottoporli a sanitizzazione.

### Convalida e sanitizzazione

Prima che i dati di un modulo vengano memorizzati, devono essere convalidati e sottoposti a sanitizzazione:

- La convalida verifica che i valori inseriti siano appropriati per ciascun campo, ovvero che rientrino nell'intervallo e nel formato corretti e così via, e che siano stati forniti valori per tutti i campi obbligatori.
- La sanitizzazione rimuove/sostituisce nei dati i caratteri che potrebbero potenzialmente essere usati per inviare contenuto dannoso al server.

Per questo tutorial verrà usato il popolare modulo [express-validator](https://www.npmjs.com/package/express-validator) per eseguire sia la convalida sia la sanitizzazione dei dati dei moduli.

#### Installazione

Installare il modulo eseguendo il comando seguente nella radice del progetto.

```bash
npm install express-validator
```

#### Usare express-validator

> [!NOTE]
> La [guida a express-validator](https://express-validator.github.io/docs/#basic-guide) su GitHub fornisce una buona panoramica dell'API. Si consiglia di leggerla per farsi un'idea di tutte le sue funzionalità, incluso l'uso della [convalida dello schema](https://express-validator.github.io/docs/guides/schema-validation/) e la [creazione di validatori personalizzati](https://express-validator.github.io/docs/guides/customizing/#custom-validators-and-sanitizers). Di seguito viene trattato solo un sottoinsieme utile per _LocalLibrary_.

Per usare il validatore nei controller, vengono specificate le particolari funzioni da importare dal modulo [express-validator](https://www.npmjs.com/package/express-validator), come mostrato di seguito:

```js
const { body, validationResult } = require("express-validator");
```

Sono disponibili molte funzioni che consentono di controllare e sottoporre a sanitizzazione i dati provenienti da parametri della richiesta, body, header, cookie e così via, oppure tutti contemporaneamente. Per questo tutorial verranno usati principalmente `body` e `validationResult` (come "required" sopra).

Le funzioni sono definite come segue:

- [`body(fields, message)`](https://express-validator.github.io/docs/api/check/#body): specifica un insieme di campi nel body della richiesta, un parametro `POST`, da convalidare e/o sottoporre a sanitizzazione, insieme a un messaggio di errore opzionale che può essere visualizzato se i test non vengono superati. I criteri di convalida e sanitizzazione sono concatenati al metodo `body()`.

  Ad esempio, la riga seguente definisce innanzitutto che viene controllato il campo "name" e che un errore di convalida imposterà il messaggio di errore "Empty name". Viene quindi chiamato il metodo di sanitizzazione `trim()` per rimuovere gli spazi bianchi dall'inizio e dalla fine della stringa, quindi `isLength()` per verificare che la stringa risultante non sia vuota. Infine, viene chiamato `escape()` per rimuovere dalla variabile i caratteri HTML che potrebbero essere usati in attacchi JavaScript di cross-site scripting.

  ```js
  [
    // …
    body("name", "Empty name").trim().isLength({ min: 1 }).escape(),
    // …
  ];
  ```

  Questo test verifica che il campo dell'età sia una data valida e usa `optional()` per specificare che i valori null e le stringhe vuote non causeranno un errore di convalida.

  ```js
  [
    // …
    body("age", "Invalid age")
      .optional({ values: "falsy" })
      .isISO8601()
      .toDate(),
    // …
  ];
  ```

  È inoltre possibile concatenare diversi validatori e aggiungere messaggi visualizzati se i validatori precedenti restituiscono false.

  ```js
  [
    // …
    body("name")
      .trim()
      .isLength({ min: 1 })
      .withMessage("Name empty.")
      .isAlpha()
      .withMessage("Name must be alphabet letters."),
    // …
  ];
  ```

- [`validationResult(req)`](https://express-validator.github.io/docs/api/validation-result/#validationresult): esegue la convalida, rendendo gli errori disponibili sotto forma di oggetto risultato `validation`. Viene invocato in un callback separato, come mostrato di seguito:

  ```js
  async (req, res, next) => {
    // Extract the validation errors from a request.
    const errors = validationResult(req);

    if (!errors.isEmpty()) {
      // There are errors. Render form again with sanitized values/errors messages.
      // Error messages can be returned in an array using `errors.array()`.
    } else {
      // Data from form is valid.
    }
  };
  ```

  Viene usato il metodo `isEmpty()` del risultato della convalida per verificare se sono presenti errori e il suo metodo `array()` per ottenere l'insieme dei messaggi di errore. Per ulteriori informazioni, vedere la [sezione sulla gestione della convalida](https://express-validator.github.io/docs/guides/getting-started/#handling-validation-errors).

Le catene di convalida e sanitizzazione sono middleware che devono essere passati al gestore della route Express, operazione eseguita indirettamente tramite il controller. Quando il middleware viene eseguito, ogni validatore/sanitizzatore viene eseguito nell'ordine specificato.

Alcuni esempi pratici verranno illustrati quando si implementeranno i moduli di _LocalLibrary_ qui sotto.

### Progettazione dei moduli

Molti dei modelli della libreria sono correlati/dipendenti: ad esempio, un `Book` _richiede_ un `Author` e _può_ inoltre avere uno o più `Genres`. Ciò solleva la questione di come gestire il caso in cui un utente desideri:

- Creare un oggetto quando i relativi oggetti correlati non esistono ancora, ad esempio un libro il cui oggetto autore non è stato definito.
- Eliminare un oggetto che è ancora usato da un altro oggetto, ad esempio eliminare un `Genre` che è ancora usato da un `Book`.

Per questo progetto, l'implementazione verrà semplificata stabilendo che un modulo può soltanto:

- Creare un oggetto usando oggetti che esistono già, pertanto gli utenti dovranno creare tutte le istanze `Author` e `Genre` richieste prima di tentare di creare oggetti `Book`.
- Eliminare un oggetto se non è referenziato da altri oggetti, pertanto, ad esempio, non sarà possibile eliminare un `Book` finché non saranno stati eliminati tutti gli oggetti `BookInstance` associati.

> [!NOTE]
> Un'implementazione più flessibile potrebbe consentire di creare gli oggetti dipendenti durante la creazione di un nuovo oggetto e di eliminare qualsiasi oggetto in qualsiasi momento, ad esempio eliminando gli oggetti dipendenti o rimuovendo dal database i riferimenti all'oggetto eliminato.

### Route

Per implementare il codice di gestione dei moduli, saranno necessarie due route con lo stesso pattern URL. La prima route (`GET`) viene usata per visualizzare un nuovo modulo vuoto per creare l'oggetto. La seconda route (`POST`) viene usata per convalidare i dati inseriti dall'utente, quindi salvare le informazioni e reindirizzare alla pagina dei dettagli, se i dati sono validi, oppure visualizzare nuovamente il modulo con gli errori, se i dati non sono validi.

Le route per tutte le pagine di creazione dei modelli sono già state create in **/routes/catalog.js** (in un [tutorial precedente](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/routes)). Ad esempio, di seguito sono mostrate le route del genere:

```js
// GET request for creating a Genre. NOTE This must come before route that displays Genre (uses id).
router.get("/genre/create", genre_controller.genre_create_get);

// POST request for creating Genre.
router.post("/genre/create", genre_controller.genre_create_post);
```

## Sottoarticoli sui moduli Express

I seguenti sottoarticoli illustreranno il processo di aggiunta dei moduli richiesti all'applicazione di esempio. È necessario leggere e seguire ciascuno di essi in sequenza prima di passare al successivo.

1. [Modulo per la creazione di un genere](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_genre_form) — Definizione di una pagina per creare oggetti `Genre`.
2. [Modulo per la creazione di un autore](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_author_form) — Definizione di una pagina per creare oggetti `Author`.
3. [Modulo per la creazione di un libro](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_book_form) — Definizione di una pagina/modulo per creare oggetti `Book`.
4. [Modulo per la creazione di una BookInstance](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_BookInstance_form) — Definizione di una pagina/modulo per creare oggetti `BookInstance`.
5. [Modulo per l'eliminazione di un autore](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Delete_author_form) — Definizione di una pagina per eliminare oggetti `Author`.
6. [Modulo per l'aggiornamento di un libro](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Update_Book_form) — Definizione di una pagina per aggiornare oggetti `Book`.

## Mettiti alla prova

Implementare le pagine di eliminazione per i modelli `Book`, `BookInstance` e `Genre`, collegandole dalle pagine dei dettagli associate nello stesso modo della pagina di _eliminazione dell'autore_. Le pagine devono seguire lo stesso approccio di progettazione:

- Se esistono riferimenti all'oggetto da parte di altri oggetti, tali oggetti devono essere visualizzati insieme a una nota che indichi che questo record non può essere eliminato finché gli oggetti elencati non sono stati eliminati.
- Se non esistono altri riferimenti all'oggetto, la vista deve richiederne l'eliminazione. Se l'utente preme il pulsante **Delete**, il record deve essere eliminato.

Alcuni suggerimenti:

- L'eliminazione di un `Genre` è uguale all'eliminazione di un `Author`, poiché entrambi gli oggetti sono dipendenze di `Book`; in entrambi i casi, l'oggetto può essere eliminato solo quando vengono eliminati i libri associati.
- Anche l'eliminazione di un `Book` è simile, poiché è necessario prima verificare che non vi siano `BookInstances` associati.
- L'eliminazione di una `BookInstance` è la più semplice, poiché non esistono oggetti dipendenti. In questo caso è sufficiente trovare il record associato ed eliminarlo.

Implementare le pagine di aggiornamento per i modelli `BookInstance`, `Author` e `Genre`, collegandole dalle pagine dei dettagli associate nello stesso modo della pagina di _aggiornamento del libro_.

Alcuni suggerimenti:

- La _pagina di aggiornamento del libro_ appena implementata è la più difficile. Gli stessi pattern possono essere usati per le pagine di aggiornamento degli altri oggetti.
- I campi della data di morte e della data di nascita di `Author` e il campo due_date di `BookInstance` sono nel formato errato per l'inserimento nel campo di input della data del modulo, che richiede dati nel formato "YYYY-MM-DD". Il modo più semplice per risolvere il problema consiste nel definire una nuova proprietà virtuale per le date che le formatti in modo appropriato e quindi usare questo campo nei template di vista associati.
- In caso di difficoltà, sono disponibili esempi delle pagine di aggiornamento [nell'esempio qui](https://github.com/mdn/express-locallibrary-tutorial).

## Riepilogo

_Express_, node e i pacchetti di terze parti su npm forniscono tutto ciò che serve per aggiungere moduli al sito web. In questo articolo è stato illustrato come creare moduli usando _Pug_, convalidare e sottoporre a sanitizzazione l'input usando _express-validator_ e aggiungere, eliminare e modificare record nel database.

Ora dovrebbe essere chiaro come aggiungere moduli di base e codice di gestione dei moduli ai propri siti web node.

## Vedi anche

- [express-validator](https://www.npmjs.com/package/express-validator) (documentazione npm).

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data", "Learn_web_development/Extensions/Server-side/Express_Nodejs/deployment", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}
