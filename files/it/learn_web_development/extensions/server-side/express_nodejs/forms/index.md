---
title: "Parte 6 del Tutorial Express: Lavorare con i moduli"
short-title: "6: Lavorare con i moduli"
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/forms
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data", "Learn_web_development/Extensions/Server-side/Express_Nodejs/deployment", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}

In questo tutorial mostreremo come lavorare con moduli HTML in Express utilizzando Pug. In particolare, discuteremo come scrivere moduli per creare, aggiornare e cancellare documenti dal database del sito.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Completa tutti i precedenti argomenti del tutorial, incluso <a href="/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data">Parte 5 del Tutorial Express: Visualizzare i dati della biblioteca</a>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Capire come scrivere moduli per ottenere dati dagli utenti e aggiornare il database con questi dati.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

Un [modulo HTML](/it/docs/Learn_web_development/Extensions/Forms) è un gruppo di uno o più campi/widget su una pagina web che possono essere utilizzati per raccogliere informazioni dagli utenti per l'invio a un server. I moduli sono un meccanismo flessibile per raccogliere input degli utenti poiché ci sono input di modulo adatti per inserire molti tipi diversi di dati—caselle di testo, caselle di controllo, pulsanti radio, selettori di date, ecc. I moduli sono anche un modo relativamente sicuro per condividere dati con il server, poiché ci permettono di inviare dati in richieste `POST` con protezione contro attacchi di cross-site request forgery.

Lavorare con i moduli può essere complicato! Gli sviluppatori devono scrivere l'HTML per il modulo, convalidare e correttamente sanificare i dati inseriti sul server (e possibilmente anche nel browser), ripubblicare il modulo con messaggi di errore per informare gli utenti di eventuali campi non validi, gestire i dati quando sono stati inviati con successo e infine rispondere all'utente in qualche modo per indicare il successo.

In questo tutorial, mostreremo come le operazioni sopra possono essere eseguite in _Express_. Nel frattempo, estenderemo il sito web _LocalLibrary_ per permettere agli utenti di creare, modificare ed eliminare elementi dalla biblioteca.

> [!NOTE]
> Non abbiamo esaminato come restringere particolari rotte agli utenti autenticati o autorizzati, quindi a questo punto, qualsiasi utente sarà in grado di apportare modifiche al database.

### Moduli HTML

Prima una breve panoramica sui [moduli HTML](/it/docs/Learn_web_development/Extensions/Forms). Considera un semplice modulo HTML, con un singolo campo di testo per inserire il nome di un "team" e la sua etichetta associata:

![Esempio di campo nome semplice in un modulo HTML](form_example_name_field.png)

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

Qui abbiamo incluso solo un campo (testo) per inserire il nome del team, ma un modulo _può_ contenere qualsiasi numero di elementi di input e le loro etichette associate. L'attributo `type` del campo definisce quale tipo di widget verrà visualizzato. Il `name` e `id` del campo sono usati per identificare il campo in JavaScript/CSS/HTML, mentre `value` definisce il valore iniziale del campo quando è mostrato per la prima volta. L'etichetta del team corrispondente è specificata usando il tag `label` (vedi "Enter name" sopra), con un campo `for` che contiene il valore `id` dell'`input` associato.

L'input `submit` sarà visualizzato come un pulsante (di default)—può essere premuto dall'utente per caricare i dati contenuti negli altri elementi di input al server (in questo caso, solo il `team_name`). Gli attributi del modulo definiscono il `method` HTTP usato per inviare i dati e la destinazione dei dati sul server (`action`):

- `action`: La risorsa/URL dove i dati devono essere inviati per l'elaborazione quando il modulo viene inviato. Se non è impostato (o impostato a una stringa vuota), allora il modulo sarà inviato di nuovo all'URL della pagina corrente.
- `method`: Il metodo HTTP usato per inviare i dati: `POST` o `GET`.

  - Il metodo `POST` dovrebbe sempre essere usato se i dati porteranno a un cambiamento nel database del server, poiché questo può essere reso più resistente agli attacchi di cross-site forgery request.
  - Il metodo `GET` dovrebbe essere usato solo per moduli che non modificano i dati utente (ad esempio, un modulo di ricerca). È raccomandato quando si desidera poter salvare tra i preferiti o condividere l'URL.

### Processo di gestione dei moduli

La gestione dei moduli utilizza tutte le tecniche che abbiamo appreso per visualizzare informazioni sui nostri modelli: la rotta invia la nostra richiesta a una funzione del controller che esegue tutte le azioni di database richieste, inclusa la lettura dei dati dai modelli, quindi genera e restituisce una pagina HTML. Ciò che rende le cose più complicate è che il server deve anche essere in grado di elaborare i dati forniti dall'utente e ridistribuire il modulo con informazioni sugli errori se ci sono problemi.

Un diagramma di flusso del processo di elaborazione delle richieste del modulo è mostrato sotto, iniziando con una richiesta per una pagina contenente un modulo (mostrata in verde):

![Diagramma di flusso dell'elaborazione delle richieste del modulo del server web. Il browser richiede la pagina contenente il modulo inviando una richiesta HTTP GET. Il server crea un modulo vuoto di default e lo restituisce all'utente. L'utente popola o aggiorna il modulo, inviandolo tramite HTTP POST con dati del modulo. Il server convalida i dati del modulo ricevuti. Se i dati forniti dall'utente sono non validi, il server ricrea il modulo con i dati inseriti dall'utente e i messaggi di errore e lo invia di nuovo all'utente per avvisarlo di aggiornare e ripostare tramite HTTP Post, e convalida di nuovo. Se i dati sono validi, il server esegue azioni sui dati validi e reindirizza l'utente all'URL di successo.](web_server_form_handling.png)

Come mostrato nel diagramma sopra, le principali cose che il codice di gestione dei moduli deve fare sono:

1. Visualizzare il modulo predefinito la prima volta che viene richiesto dall'utente.

   - Il modulo può contenere campi vuoti (ad esempio, se stai creando un nuovo record), oppure può essere precompilato con valori iniziali (ad esempio, se stai modificando un record o hai valori iniziali utili di default).

2. Ricevere i dati inviati dall'utente, solitamente in una richiesta HTTP `POST`.
3. Convalidare e sanificare i dati.
4. Se qualche dato è non valido, ridistribuire il modulo—questa volta con i valori popolati dall'utente e i messaggi di errore per i campi problematici.
5. Se tutti i dati sono validi, eseguire le azioni richieste (es. salvare i dati nel database, inviare un'email di notifica, restituire il risultato di una ricerca, caricare un file, ecc.)
6. Una volta completate tutte le azioni, reindirizzare l'utente a un'altra pagina.

Spesso il codice di gestione del modulo è implementato usando una rotta `GET` per la visualizzazione iniziale del modulo e una rotta `POST` per lo stesso percorso per gestire la validazione e l'elaborazione dei dati del modulo. Questo è l'approccio che verrà utilizzato in questo tutorial.

Express stesso non fornisce alcun supporto specifico per le operazioni di gestione dei moduli, ma può utilizzare middleware per processare parametri `POST` e `GET` dal modulo e per convalidare/sanificare i loro valori.

### Validazione e sanificazione

Prima che i dati di un modulo siano memorizzati, devono essere convalidati e sanificati:

- La validazione verifica che i valori inseriti siano appropriati per ciascun campo (siano nel giusto intervallo, formato, ecc.) e che siano stati forniti valori per tutti i campi obbligatori.
- La sanificazione rimuove/sostituisce caratteri nei dati che potrebbero potenzialmente essere usati per inviare contenuti dannosi al server.

Per questo tutorial, utilizzeremo il popolare modulo [express-validator](https://www.npmjs.com/package/express-validator) per eseguire sia la validazione che la sanificazione dei dati del nostro modulo.

#### Installazione

Installa il modulo eseguendo il seguente comando nella radice del progetto.

```bash
npm install express-validator
```

#### Utilizzo di express-validator

> [!NOTE]
> La guida [express-validator](https://express-validator.github.io/docs/#basic-guide) su GitHub fornisce una buona panoramica delle API. Si consiglia di leggerla per avere un'idea di tutte le sue capacità (incluso l'utilizzo della [validazione degli schemi](https://express-validator.github.io/docs/guides/schema-validation/) e la [creazione di validatori personalizzati](https://express-validator.github.io/docs/guides/customizing/#custom-validators-and-sanitizers)). Di seguito copriamo solo un sottoinsieme utile per il _LocalLibrary_.

Per utilizzare il validatore nei nostri controller, specifichiamo le particolari funzioni che vogliamo importare dal modulo [express-validator](https://www.npmjs.com/package/express-validator), come mostrato di seguito:

```js
const { body, validationResult } = require("express-validator");
```

Sono disponibili molte funzioni, che ti consentono di verificare e sanificare dati dai parametri della richiesta, dal corpo, dagli header, dai cookie, ecc., o tutti insieme. Per questo tutorial, useremo principalmente `body` e `validationResult` (come "required" sopra).

Le funzioni sono definite come segue:

- [`body(fields, message)`](https://express-validator.github.io/docs/api/check/#body): Specifica un insieme di campi nel corpo della richiesta (un parametro `POST`) da validare e/o sanificare con un messaggio di errore opzionale che può essere visualizzato se non supera i test. I criteri di validazione e sanificazione sono concatenati al metodo `body()`.

  Per esempio, la riga seguente definisce che stiamo verificando il campo "name" e che un errore di validazione imposterà un messaggio di errore "Empty name". Poi chiamiamo il metodo di sanificazione `trim()` per rimuovere spazi bianchi dall'inizio e alla fine della stringa, e poi `isLength()` per verificare che la stringa risultante non sia vuota. Infine, chiamiamo `escape()` per rimuovere caratteri HTML dalla variabile che potrebbe essere usata negli attacchi di scripting cross-site JavaScript.

  ```js
  [
    // …
    body("name", "Empty name").trim().isLength({ min: 1 }).escape(),
    // …
  ];
  ```

  Questo test verifica che il campo age sia una data valida e utilizza `optional()` per specificare che valori nulli e stringhe vuote non falliranno la validazione.

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

  Puoi anche concatenare diversi validatori e aggiungere messaggi che vengono visualizzati se i validatori precedenti sono falsi.

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

- [`validationResult(req)`](https://express-validator.github.io/docs/api/validation-result/#validationresult): Esegue la validazione, rendendo disponibili gli errori sotto forma di un oggetto `validation` result. Questo è invocato in un callback separato, come mostrato di seguito:

  ```js
  asyncHandler(async (req, res, next) => {
    // Extract the validation errors from a request.
    const errors = validationResult(req);

    if (!errors.isEmpty()) {
      // There are errors. Render form again with sanitized values/errors messages.
      // Error messages can be returned in an array using `errors.array()`.
    } else {
      // Data from form is valid.
    }
  });
  ```

  Usiamo il metodo `isEmpty()` del risultato di validazione per verificare se ci sono stati errori, e il suo metodo `array()` per ottenere l'insieme dei messaggi di errore. Vedi la sezione [Gestione degli errori di validazione](https://express-validator.github.io/docs/guides/getting-started/#handling-validation-errors) per maggiori informazioni.

Le catene di validazione e sanificazione sono middleware che dovrebbero essere passati all'handler del percorso Express (lo facciamo indirettamente, tramite il controller). Quando il middleware viene eseguito, ogni validatore/sanitizzatore viene eseguito nell'ordine specificato.

Copriamo alcuni esempi reali quando implementiamo i moduli _LocalLibrary_ di seguito.

### Progettazione del modulo

Molti dei modelli nella biblioteca sono correlati/dipendenti—ad esempio, un `Book` _richiede_ un `Author`, e _può_ avere anche uno o più `Genres`. Questo solleva la questione di come dovremmo gestire il caso in cui un utente desideri:

- Creare un oggetto quando i suoi oggetti correlati non esistono ancora (ad esempio, un libro dove l'oggetto autore non è stato definito).
- Eliminare un oggetto che è ancora in uso da un altro oggetto (quindi, ad esempio, eliminare un `Genre` che è ancora in uso da un `Book`).

Per questo progetto semplificheremo l'implementazione affermando che un modulo può solo:

- Creare un oggetto usando oggetti che esistono già (quindi gli utenti dovranno creare eventuali istanze richieste di `Author` e `Genre` prima di tentare di creare qualsiasi oggetto `Book`).
- Cancellare un oggetto se non è referenziato da altri oggetti (quindi, ad esempio, non sarà possibile eliminare un `Book` fino a quando tutti gli oggetti `BookInstance` associati non saranno stati eliminati).

> [!NOTE]
> Un'implementazione più flessibile potrebbe consentire di creare gli oggetti dipendenti quando si crea un nuovo oggetto, e cancellare qualsiasi oggetto in qualsiasi momento (ad esempio, cancellando oggetti dipendenti, o rimuovendo riferimenti all'oggetto cancellato dal database).

### Rotte

Per implementare il nostro codice di gestione dei moduli, avremo bisogno di due rotte che abbiano lo stesso schema URL. La prima (`GET`) rotta è usata per visualizzare un nuovo modulo vuoto per la creazione dell'oggetto. La seconda rotta (`POST`) è usata per validare i dati inseriti dall'utente e quindi salvare le informazioni e reindirizzare alla pagina di dettaglio (se i dati sono validi) o ridistribuire il modulo con errori (se i dati sono non validi).

Abbiamo già creato le rotte per tutte le pagine di creazione dei nostri modelli in **/routes/catalog.js** (in un [tutorial precedente](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/routes)). Ad esempio, le rotte di genere sono mostrate di seguito:

```js
// GET request for creating a Genre. NOTE This must come before route that displays Genre (uses id).
router.get("/genre/create", genre_controller.genre_create_get);

// POST request for creating Genre.
router.post("/genre/create", genre_controller.genre_create_post);
```

## Sottoarticoli sui moduli Express

I seguenti sottoarticoli ci guideranno attraverso il processo di aggiunta dei moduli richiesti alla nostra applicazione di esempio. Devi leggere e lavorare attraverso ciascuno di essi a turno, prima di passare al successivo.

1. [Creare un modulo di genere](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_genre_form) — Definizione di una pagina per creare oggetti `Genre`.
2. [Creare un modulo autore](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_author_form) — Definizione di una pagina per creare oggetti `Author`.
3. [Creare un modulo libro](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_book_form) — Definizione di una pagina/form per creare oggetti `Book`.
4. [Creare un modulo BookInstance](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_BookInstance_form) — Definizione di una pagina/form per creare oggetti `BookInstance`.
5. [Cancellare un modulo autore](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Delete_author_form) — Definizione di una pagina per cancellare oggetti `Author`.
6. [Aggiornare un modulo libro](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Update_Book_form) — Definizione di una pagina per aggiornare oggetti `Book`.

## Sfida te stesso

Implementa le pagine di eliminazione per i modelli `Book`, `BookInstance`, e `Genre`, collegandole dalle relative pagine di dettaglio nello stesso modo della nostra pagina di eliminazione del _Author_. Le pagine dovrebbero seguire lo stesso approccio di progettazione:

- Se ci sono riferimenti all'oggetto da parte di altri oggetti, allora questi altri oggetti dovrebbero essere visualizzati insieme a una nota che indica che questo record non può essere cancellato fino a quando gli oggetti elencati non sono stati cancellati.
- Se non ci sono altri riferimenti all'oggetto allora la vista dovrebbe chiedere di cancellarlo. Se l'utente preme il pulsante **Delete**, il record dovrebbe essere cancellato.

Alcuni suggerimenti:

- Eliminare un `Genre` è esattamente come eliminare un `Author`, poiché entrambi gli oggetti sono dipendenze di `Book` (quindi in entrambi i casi puoi cancellare l'oggetto solo quando i libri associati sono cancellati).
- Eliminare un `Book` è anche simile poiché è necessario prima verificare che non ci siano `BookInstances` associati.
- Eliminare un `BookInstance` è il più facile di tutti perché non ci sono oggetti dipendenti. In questo caso, puoi semplicemente trovare il record associato e cancellarlo.

Implementa le pagine di aggiornamento per i modelli `BookInstance`, `Author`, e `Genre`, collegandole dalle relative pagine di dettaglio nello stesso modo della nostra pagina di aggiornamento del _Book_.

Alcuni suggerimenti:

- La pagina di aggiornamento _Book_ che abbiamo appena implementato è la più difficile! Gli stessi schemi possono essere utilizzati per le pagine di aggiornamento per gli altri oggetti.
- I campi di data di morte e data di nascita dell'`Author` e il campo di `due_date` di `BookInstance` sono nel formato errato per l'inserimento nel campo di input data del modulo (richiede dati nel formato "AAAA-MM-GG"). Il modo più semplice per superare questo problema è definire una nuova proprietà virtuale per le date che formatta le date in modo appropriato, e quindi utilizzare questo campo nei modelli di vista associati.
- Se rimani bloccato, ci sono esempi delle pagine di aggiornamento [nell'esempio qui](https://github.com/mdn/express-locallibrary-tutorial).

## Riepilogo

_Express_, node e pacchetti di terze parti su npm forniscono tutto il necessario per aggiungere moduli al tuo sito web. In questo articolo, hai imparato come creare moduli utilizzando _Pug_, validare e sanificare l'input utilizzando _express-validator_, e aggiungere, eliminare e modificare record nel database.

Ora dovresti capire come aggiungere moduli di base e codice di gestione dei moduli ai tuoi siti web node!

## Vedi anche

- [express-validator](https://www.npmjs.com/package/express-validator) (documentazione npm).

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data", "Learn_web_development/Extensions/Server-side/Express_Nodejs/deployment", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}
