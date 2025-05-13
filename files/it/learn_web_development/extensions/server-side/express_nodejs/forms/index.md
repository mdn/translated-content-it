---
title: "Tutorial di Express Parte 6: Lavorare con i moduli"
short-title: "6: Lavorare con i moduli"
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/forms
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data", "Learn_web_development/Extensions/Server-side/Express_Nodejs/deployment", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}

In questo tutorial le mostreremo come lavorare con i moduli HTML in Express utilizzando Pug. In particolare, discuteremo come scrivere moduli per creare, aggiornare e eliminare documenti dal database del sito.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Completi tutti gli argomenti precedenti del tutorial, incluso <a href="/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data">Express Tutorial Part 5: Visualizzare i dati della libreria</a>
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

Un [Modulo HTML](/it/docs/Learn_web_development/Extensions/Forms) è un gruppo di uno o più campi/widget su una pagina web che possono essere utilizzati per raccogliere informazioni dagli utenti per l'invio a un server. I moduli sono un meccanismo flessibile per raccogliere input dagli utenti poiché ci sono input di modulo adatti per inserire molti tipi diversi di dati: caselle di testo, caselle di controllo, pulsanti di selezione, selettori di date, ecc. I moduli sono anche un modo relativamente sicuro per condividere dati con il server, poiché ci consentono di inviare dati nelle richieste `POST` con protezione contro la falsificazione di richieste cross-site.

Lavorare con i moduli può essere complicato! Gli sviluppatori devono scrivere HTML per il modulo, validare e sanificare correttamente i dati inseriti sul server (e possibilmente anche nel browser), ripubblicare il modulo con messaggi di errore per informare gli utenti di eventuali campi non validi, gestire i dati quando sono stati inviati con successo, e infine rispondere all'utente in qualche modo per indicare il successo.

In questo tutorial, le mostreremo come le operazioni sopra possono essere effettuate in _Express_. Durante il percorso, estenderemo il sito web _LocalLibrary_ per consentire agli utenti di creare, modificare ed eliminare elementi dalla libreria.

> [!NOTE]
> Non abbiamo visto come limitare determinate rotte agli utenti autenticati o autorizzati, quindi a questo punto, qualsiasi utente sarà in grado di apportare modifiche al database.

### Moduli HTML

Innanzitutto, una breve panoramica dei [Moduli HTML](/it/docs/Learn_web_development/Extensions/Forms). Consideri un semplice modulo HTML, con un singolo campo di testo per inserire il nome di un qualche "team", e la sua etichetta associata:

![Esempio di campo nome semplice in un modulo HTML](form_example_name_field.png)

Il modulo è definito in HTML come una collezione di elementi all'interno dei tag `<form>…</form>`, contenente almeno un elemento `input` di `type="submit"`.

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

Mentre qui abbiamo incluso solo un (testo) campo per inserire il nome del team, un modulo _può_ contenere qualsiasi numero di altri elementi input e le loro etichette associate. L'attributo `type` del campo definisce quale tipo di widget verrà visualizzato. Il `name` e `id` del campo vengono utilizzati per identificare il campo in JavaScript/CSS/HTML, mentre `value` definisce il valore iniziale per il campo quando viene visualizzato per la prima volta. L'etichetta di squadra corrispondente è specificata utilizzando il tag `label` (vedi "Enter name" sopra), con un campo `for` contenente il valore `id` dell'`input` associato.

L'input `submit` sarà visualizzato come un pulsante (per impostazione predefinita)—questo può essere premuto dall'utente per caricare i dati contenuti dagli altri elementi input al server (in questo caso, solo il `team_name`). Gli attributi del modulo definiscono il `method` HTTP utilizzato per inviare i dati e la destinazione dei dati sul server (`action`):

- `action`: La risorsa/URL dove i dati devono essere inviati per l'elaborazione quando il modulo viene inviato. Se non è impostato (o impostato su una stringa vuota), allora il modulo sarà inviato di nuovo all'URL della pagina corrente.
- `method`: Il metodo HTTP utilizzato per inviare i dati: `POST` o `GET`.

  - Il metodo `POST` deve essere sempre utilizzato se i dati comporteranno una modifica al database del server, perché questo può essere reso più resistente agli attacchi di falsificazione di richieste cross-site.
  - Il metodo `GET` deve essere utilizzato solo per i moduli che non cambiano i dati dell'utente (ad esempio, un modulo di ricerca). È raccomandato quando lei vuole poter aggiungere ai preferiti o condividere l'URL.

### Processo di gestione dei moduli

La gestione dei moduli utilizza tutte le stesse tecniche che abbiamo imparato per visualizzare le informazioni sui nostri modelli: la rotta invia la nostra richiesta a una funzione controller che esegue tutte le azioni del database richieste, inclusa la lettura dei dati dai modelli, quindi genera e restituisce una pagina HTML. Ciò che rende le cose più complicate è che il server deve essere in grado di elaborare anche i dati forniti dall'utente e ridisplay il modulo con le informazioni sugli errori se ci sono problemi.

Un diagramma di flusso per l'elaborazione delle richieste dei moduli è mostrato di seguito, partendo da una richiesta per una pagina contenente un modulo (mostrata in verde):

![Diagramma di flusso di elaborazione delle richieste del modulo del server web. Le richieste del browser per la pagina contenente il modulo inviando una richiesta HTTP GET. Il server crea un modulo predefinito vuoto e lo restituisce all'utente. L'utente compila o aggiorna il modulo, inviandolo tramite HTTP POST con i dati del modulo. Il server valida i dati del modulo ricevuti. Se i dati forniti dall'utente sono non validi, il server ricrea il modulo con i dati inseriti dall'utente e messaggi di errore e lo invia di nuovo all'utente per l'utente di aggiornare e reinviare tramite HTTP Post, e lo valida nuovamente. Se i dati sono validi, il server esegue azioni sui dati validi e reindirizza l'utente all'URL di successo.](web_server_form_handling.png)

Come mostrato nel diagramma sopra, le cose principali che il codice di gestione del modulo deve fare sono:

1. Visualizzare il modulo predefinito la prima volta che viene richiesto dall'utente.

   - Il modulo può contenere campi vuoti (ad esempio, se sta creando un nuovo record), o può essere pre-popolato con valori iniziali (ad esempio, se sta cambiando un record, o ha valori iniziali utili di default).

2. Ricevere i dati inviati dall'utente, di solito in una richiesta HTTP `POST`.
3. Validare e sanificare i dati.
4. Se qualche dato è non valido, ridisplay il modulo—questa volta con i valori popolati dall'utente e i messaggi di errore per i campi problematici.
5. Se tutti i dati sono validi, eseguire le azioni richieste (ad esempio, salvare i dati nel database, inviare un'email di notifica, restituire il risultato di una ricerca, caricare un file, ecc.)
6. Una volta completate tutte le azioni, reindirizzare l'utente a un'altra pagina.

Spesso il codice di gestione del modulo è implementato utilizzando una rotta `GET` per la visualizzazione iniziale del modulo e una rotta `POST` per lo stesso percorso per gestire la validazione e l'elaborazione dei dati del modulo. Questo è l'approccio che verrà utilizzato in questo tutorial.

Express stesso non fornisce alcun supporto specifico per le operazioni di gestione dei moduli, ma può utilizzare middleware per elaborare i parametri `POST` e `GET` dal modulo, e per validare/sanificare i loro valori.

### Validazione e sanificazione

Prima che i dati da un modulo siano memorizzati, devono essere validati e sanificati:

- La validazione verifica che i valori inseriti siano appropriati per ciascun campo (siano nel intervallo corretto, formato, ecc.) e che siano stati forniti valori per tutti i campi richiesti.
- La sanificazione rimuove/sostituisce i caratteri nei dati che potrebbero potenzialmente essere utilizzati per inviare contenuti dannosi al server.

Per questo tutorial, utilizzeremo il modulo popolare [express-validator](https://www.npmjs.com/package/express-validator) per eseguire sia la validazione che la sanificazione dei nostri dati del modulo.

#### Installazione

Installi il modulo eseguendo il seguente comando nella radice del progetto.

```bash
npm install express-validator
```

#### Utilizzo di express-validator

> [!NOTE]
> La guida di [express-validator](https://express-validator.github.io/docs/#basic-guide) su GitHub fornisce una buona panoramica dell'API. Le consigliamo di leggerla per avere un'idea di tutte le sue capacità (incluso l'utilizzo della [validazione dello schema](https://express-validator.github.io/docs/guides/schema-validation/) e [creare validatori personalizzati](https://express-validator.github.io/docs/guides/customizing/#custom-validators-and-sanitizers)). Di seguito copriamo solo un sottoinsieme utile per la _LocalLibrary_.

Per usare il validatore nei nostri controller, specifichiamo le particolari funzioni che vogliamo importare dal modulo [express-validator](https://www.npmjs.com/package/express-validator), come mostrato di seguito:

```js
const { body, validationResult } = require("express-validator");
```

Ci sono molte funzioni disponibili, permettendole di controllare e sanificare dati dai parametri della richiesta, corpo, intestazioni, cookie, ecc., o tutti loro in una volta. Per questo tutorial, utilizzeremo principalmente `body` e `validationResult` (come "richiesto" sopra).

Le funzioni sono definite come segue:

- [`body(fields, message)`](https://express-validator.github.io/docs/api/check/#body): Specifica un insieme di campi nel corpo della richiesta (un parametro `POST`) da validare e/o sanificare insieme a un messaggio di errore opzionale che può essere visualizzato se non supera i test. I criteri di validazione e sanificazione sono concatenati al metodo `body()`.

  Per esempio, la riga sotto definisce prima che stiamo controllando il campo "name" e che un errore di validazione imposterà un messaggio di errore "Empty name". Poi chiamiamo il metodo di sanificazione `trim()` per rimuovere lo spazio vuoto dall'inizio e dalla fine della stringa, e poi `isLength()` per controllare che la stringa risultante non sia vuota. Infine, chiamiamo `escape()` per rimuovere caratteri HTML dalla variabile che potrebbero essere usati negli attacchi di scripting cross-site JavaScript.

  ```js
  [
    // …
    body("name", "Empty name").trim().isLength({ min: 1 }).escape(),
    // …
  ];
  ```

  Questo test verifica che il campo age sia una data valida e utilizza `optional()` per specificare che null e stringhe vuote non falliranno la validazione.

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

  Può anche concatenare diversi validatori, e aggiungere messaggi che vengono visualizzati se i validatori precedenti sono falsi.

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

- [`validationResult(req)`](https://express-validator.github.io/docs/api/validation-result/#validationresult): Esegue la validazione, rendendo disponibili gli errori sotto forma di un oggetto risultato di `validation`. Questo viene invocato in un callback separato, come mostrato di seguito:

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

  Usiamo il metodo `isEmpty()` del risultato della validazione per controllare se ci sono stati errori, e il suo metodo `array()` per ottenere l'insieme di messaggi di errore. Veda la [sezione Gestione della validazione](https://express-validator.github.io/docs/guides/getting-started/#handling-validation-errors) per maggiori informazioni.

Le catene di validazione e sanificazione sono middleware che dovrebbero essere passate all'handler di rotta di Express (lo facciamo indirettamente, tramite il controller). Quando il middleware viene eseguito, ciascun validatore/sanitizzatore viene eseguito nell'ordine specificato.

Copriamo alcuni esempi reali quando implementiamo i moduli _LocalLibrary_ di seguito.

### Progettazione dei moduli

Molti dei modelli nella libreria sono correlati/dipendenti—ad esempio, un `Book` _richiede_ un `Author`, e _può_ anche avere uno o più `Genres`. Questo solleva la questione di come dovremmo gestire il caso in cui un utente desideri:

- Creare un oggetto quando i suoi oggetti correlati non esistono ancora (per esempio, un libro dove l'oggetto autore non è stato ancora definito).
- Eliminare un oggetto che è ancora utilizzato da un altro oggetto (quindi per esempio, eliminare un `Genre` che è ancora utilizzato da un `Book`).

Per questo progetto semplificheremo l'implementazione affermando che un modulo può solo:

- Creare un oggetto usando oggetti che già esistono (quindi gli utenti dovranno creare qualsiasi istanza di `Author` e `Genre` richiesta prima di tentare di creare qualsiasi oggetto `Book`).
- Eliminare un oggetto se non è referenziato da altri oggetti (quindi per esempio, non sarà in grado di eliminare un `Book` fino a quando tutti gli oggetti `BookInstance` associati non saranno eliminati).

> [!NOTE]
> Un'implementazione più flessibile potrebbe consentire di creare gli oggetti dipendenti quando si crea un nuovo oggetto, ed eliminare qualsiasi oggetto in qualsiasi momento (ad esempio, eliminando oggetti dipendenti, o rimuovendo riferimenti all'oggetto eliminato dal database).

### Rotte

Per implementare il nostro codice di gestione dei moduli, avremo bisogno di due rotte che abbiano lo stesso modello di URL. La prima rotta (`GET`) viene utilizzata per visualizzare un nuovo modulo vuoto per creare l'oggetto. La seconda rotta (`POST`) viene utilizzata per validare i dati inseriti dall'utente, quindi per salvare le informazioni e reindirizzare alla pagina di dettaglio (se i dati sono validi) o per ridisplay il modulo con errori (se i dati sono non validi).

Abbiamo già creato le rotte per tutte le nostre pagine di creazione dei modelli in **/routes/catalog.js** (in un [tutorial precedente](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/routes)). Ad esempio, le rotte del genere sono mostrate di seguito:

```js
// GET request for creating a Genre. NOTE This must come before route that displays Genre (uses id).
router.get("/genre/create", genre_controller.genre_create_get);

// POST request for creating Genre.
router.post("/genre/create", genre_controller.genre_create_post);
```

## Sottoarticoli sui moduli di Express

I seguenti sottoarticoli ci guideranno nel processo di aggiunta dei moduli richiesti alla nostra applicazione di esempio. Dovrà leggere e lavorare su ciascuno di essi a turno, prima di passare al successivo.

1. [Crea il modulo di genere](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_genre_form) — Definire una pagina per creare oggetti `Genre`.
2. [Crea il modulo di autore](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_author_form) — Definire una pagina per creare oggetti `Author`.
3. [Crea il modulo di libro](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_book_form) — Definire una pagina/modulo per creare oggetti `Book`.
4. [Crea il modulo di BookInstance](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_BookInstance_form) — Definire una pagina/modulo per creare oggetti `BookInstance`.
5. [Elimina il modulo di autore](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Delete_author_form) — Definire una pagina per eliminare oggetti `Author`.
6. [Aggiorna il modulo di libro](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Update_Book_form) — Definire pagina per aggiornare oggetti `Book`.

## Sfida personale

Implementi le pagine di eliminazione per i modelli `Book`, `BookInstance`, e `Genre`, collegandole dalle pagine di dettaglio associate nello stesso modo della nostra pagina di eliminazione _Author_. Le pagine dovrebbero seguire lo stesso approccio progettuale:

- Se ci sono riferimenti all'oggetto da parte di altri oggetti, allora questi altri oggetti dovrebbero essere visualizzati insieme a una nota che questo record non può essere eliminato fino a quando gli oggetti elencati non siano stati eliminati.
- Se non ci sono altri riferimenti all'oggetto, allora la vista dovrebbe chiedere di eliminarlo. Se l'utente preme il pulsante **Elimina**, il record dovrebbe quindi essere eliminato.

Alcuni suggerimenti:

- Eliminare un `Genre` è proprio come eliminare un `Author`, poiché entrambi gli oggetti sono dipendenze di `Book` (quindi in entrambi i casi può eliminare l'oggetto solo quando i libri associati sono eliminati).
- Eliminare un `Book` è anche simile poiché deve prima verificare che non ci siano `BookInstances` associati.
- Eliminare un `BookInstance` è il più semplice di tutti perché non ci sono oggetti dipendenti. In questo caso, può semplicemente trovare il record associato ed eliminarlo.

Implementi le pagine di aggiornamento per i modelli `BookInstance`, `Author`, e `Genre`, collegandole dalle pagine di dettaglio associate nello stesso modo della nostra pagina di aggiornamento _Book_.

Alcuni suggerimenti:

- La pagina di aggiornamento _Book_ che abbiamo appena implementato è la più difficile! Gli stessi schemi possono essere utilizzati per le pagine di aggiornamento per gli altri oggetti.
- I campi di data di morte e data di nascita di `Author` e il campo di `due_date` di `BookInstance` sono nel formato sbagliato per essere inseriti nel campo di input data del modulo (richiede dati nel formato "YYYY-MM-DD"). Il modo più semplice per aggirare questo è definire una nuova proprietà virtuale per le date che formatta le date appropriatamente e poi utilizzare questo campo nei modelli di vista associati.
- Se resta bloccato, ci sono esempi delle pagine di aggiornamento [nell'esempio qui](https://github.com/mdn/express-locallibrary-tutorial).

## Riepilogo

_Express_, node, e pacchetti di terze parti su npm forniscono tutto ciò di cui ha bisogno per aggiungere moduli al suo sito web. In questo articolo, ha imparato come creare moduli utilizzando _Pug_, validare e sanificare l'input utilizzando _express-validator_, e aggiungere, eliminare e modificare record nel database.

Dovrebbe ora capire come aggiungere moduli di base e codice di gestione dei moduli ai suoi siti web node!

## Vedere anche

- [express-validator](https://www.npmjs.com/package/express-validator) (documentazione npm).

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data", "Learn_web_development/Extensions/Server-side/Express_Nodejs/deployment", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}
