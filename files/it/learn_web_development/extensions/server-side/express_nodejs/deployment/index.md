---
title: "Tutorial di Express Parte 7: Distribuire in produzione"
short-title: "7: Distribuire"
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/deployment
l10n:
  sourceCommit: e488eba036b2fee56444fd579c3759ef45ff2ca8
---

{{PreviousMenu("Learn_web_development/Extensions/Server-side/Express_Nodejs/forms", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}

Ora che ha creato (e testato) un fantastico sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website), vorrà installarlo su un server web pubblico in modo che possa essere accessibile dal personale della biblioteca e dai membri attraverso Internet. Questo articolo fornisce una panoramica su come potrebbe procedere per trovare un host per distribuire il suo sito web e cosa deve fare per preparare il suo sito per la produzione.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Completi tutti i precedenti argomenti del tutorial, compresa la <a href="/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms">Tutorial di Express Parte 6: Lavorare con i form</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare dove e come può distribuire un'app Express in produzione.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

Una volta terminato il suo sito (o terminato "abbastanza" per iniziare il test pubblico) dovrà ospitarlo in un luogo più pubblico e accessibile del suo computer personale di sviluppo.

Fino ad ora, ha lavorato in un [ambiente di sviluppo](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment), utilizzando Express/Node come server web per condividere il suo sito al browser/rete locale, e ha eseguito il suo sito web con impostazioni di sviluppo (insicure) che espongono il debug e altre informazioni private. Prima di poter ospitare un sito web esternamente dovrà:

- Scegliere un ambiente per ospitare l'app Express.
- Apportare alcune modifiche alle impostazioni del progetto.
- Configurare un'infrastruttura a livello di produzione per servire il suo sito web.

Questo tutorial fornisce alcune indicazioni sulle opzioni per scegliere un sito di hosting, una breve panoramica di cosa deve fare per preparare l'app Express per la produzione e un esempio pratico di come installare il sito web LocalLibrary sul servizio di hosting cloud [Railway](https://railway.com/).

## Cos'è un ambiente di produzione?

L'ambiente di produzione è l'ambiente fornito dal computer server dove eseguirà il suo sito web per il consumo esterno. L'ambiente include:

- Hardware del computer su cui viene eseguito il sito web.
- Sistema operativo (ad es., Linux o Windows).
- Runtime del linguaggio di programmazione e librerie di framework su cui è scritto il suo sito web.
- Infrastruttura del server web, possibilmente includendo un server web, un proxy inverso, un bilanciatore di carico, ecc.
- Database da cui dipende il suo sito web.

Il computer server potrebbe essere situato presso i suoi locali e connesso a Internet tramite un collegamento veloce, ma è molto più comune utilizzare un computer ospitato "nel cloud". Questo significa che il suo codice viene eseguito su un computer remoto (o possibilmente su un computer "virtuale") nel/i data center del suo fornitore di hosting. Il server remoto offrirà di solito un livello garantito di risorse di calcolo (ad es., CPU, RAM, memoria di archiviazione, ecc.) e connettività Internet per un determinato prezzo.

Questo tipo di hardware di calcolo/rete accessibile in remoto è chiamato _Infrastructure as a Service (IaaS)_. Molti fornitori di IaaS offrono opzioni per preinstallare un particolare sistema operativo, sul quale dovrà installare gli altri componenti del suo ambiente di produzione. Altri fornitori le consentono di selezionare ambienti più completi, magari includendo un setup completo di Node.

> [!NOTE]
> Gli ambienti predefiniti possono rendere molto facile configurare il suo sito web perché riducono la configurazione, ma le opzioni disponibili possono limitarla a un server (o altri componenti) che non conosce e possono essere basati su una versione più vecchia del sistema operativo. Spesso è meglio installare i componenti da soli in modo da ottenere quelli che desidera, e quando dovrà aggiornare parti del sistema, avrà un'idea su dove iniziare!

Altri fornitori di hosting supportano Express come parte di un'offerta _Platform as a Service_ (_PaaS_). Quando si utilizza questo tipo di hosting non deve preoccuparsi della maggior parte della sua produzione (server, bilanciatori di carico, ecc.) poiché la piattaforma ospite si occupa di tutto ciò per lei. Questo rende la distribuzione abbastanza facile perché può concentrarsi solo sulla sua applicazione web e non su altre infrastrutture server.

Alcuni sviluppatori sceglieranno la maggiore flessibilità fornita da IaaS rispetto al PaaS, mentre altri apprezzeranno la ridotta manutenzione e la scalabilità più facile del PaaS. Quando si inizia, configurare il suo sito su un sistema PaaS è molto più facile, quindi è quello che faremo in questo tutorial.

> [!NOTE]
> Se sceglie un fornitore di hosting amichevole verso Node/Express, dovrebbe fornire istruzioni su come configurare un sito Express utilizzando diverse configurazioni di server web, application server, proxy inverso, ecc. Ad esempio, ci sono molte guide step-by-step per varie configurazioni nei [documenti della comunità Node di DigitalOcean](https://www.digitalocean.com/community/tutorials?q=node).

## Scegliere un fornitore di hosting

Ci sono numerosi fornitori di hosting noti per supportare attivamente o lavorare bene con _Node_ (e _Express_). Questi fornitori forniscono diversi tipi di ambienti (IaaS, PaaS), e diversi livelli di risorse di calcolo e di rete a prezzi diversi.

> [!NOTE]
> Ci sono molte soluzioni di hosting, e i loro servizi e prezzi possono cambiare nel tempo. Mentre introduciamo alcune opzioni qui sotto, vale la pena controllare sia queste che altre opzioni prima di selezionare un fornitore di hosting.

Alcune delle cose da considerare quando si sceglie un host:

- Quanto sarà occupato il suo sito e il costo dei dati e delle risorse di calcolo necessarie per soddisfare tale domanda.
- Il livello di supporto per la scalabilità orizzontale (aggiunta di più macchine) e verticale (aggiornamento a macchine più potenti) e i costi per farlo.
- Le località dove il fornitore ha data center, e quindi dove l'accesso è probabilmente più veloce.
- La performance storica di uptime e downtime dell'host.
- Strumenti forniti per gestire il sito — sono facili da usare e sicuri (ad es., SFTP vs. FTP).
- Framework integrati per monitorare il suo server.
- Limitazioni conosciute. Alcuni host bloccheranno deliberatamente alcuni servizi (ad es., email). Altri offrono solo un certo numero di ore di "tempo attivo" in alcuni livelli di prezzo, o offrono solo una piccola quantità di spazio di archiviazione.
- Benefici aggiuntivi. Alcuni fornitori offriranno nomi di dominio gratuiti e supporto per i certificati TLS che altrimenti dovrebbero essere pagati.
- Se il livello "gratuito" su cui si fa affidamento scade nel tempo, e se il costo di passare a un livello più costoso significa che sarebbe stato meglio utilizzare un altro servizio fin dall'inizio!

La buona notizia quando si inizia è che ci sono diversi siti che forniscono ambienti di calcolo "gratuiti" destinati alla valutazione e al testing. Questi sono solitamente ambienti abbastanza limitati/ristretti, e deve essere consapevole che possono scadere dopo un periodo introduttivo o avere altre restrizioni. Sono comunque ottimi per testare siti a bassa affluenza in un ambiente ospitato e possono facilitare la migrazione al pagamento per ulteriori risorse quando il suo sito diventa più frequentato. Scelte popolari in questa categoria includono [Glitch](https://glitch.com/), [Python Anywhere](https://www.pythonanywhere.com/), [Amazon Web Services](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/billing-free-tier.html), [Microsoft Azure](https://azure.microsoft.com/en-us/pricing/details/app-service/linux/), ecc.

La maggior parte dei fornitori offre anche un livello "base" o "hobby" destinato a piccoli siti di produzione, che forniscono livelli più utili di potenza di calcolo e meno limitazioni. [Railway](https://railway.com/), [Heroku](https://www.heroku.com/), [DigitalOcean](https://www.digitalocean.com/) e [Python Anywhere](https://www.pythonanywhere.com/) sono esempi di fornitori di hosting popolari che hanno un livello di calcolo base relativamente economico (nell'ordine di $5 a $10 USD al mese).

> [!NOTE]
> Ricordi che il prezzo non è l'unico criterio di selezione. Se il suo sito web ha successo, potrebbe risultare che la scalabilità sia la considerazione più importante.

## Preparare il suo sito web per la pubblicazione

Le cose principali a cui pensare quando si pubblica il suo sito web sono la sicurezza web e le prestazioni. Al minimo indispensabile, dovrà modificare la configurazione del database in modo che possa utilizzare un database diverso per la produzione e proteggere le sue credenziali, rimuovere le tracce dello stack incluse nelle pagine di errore durante lo sviluppo, riordinare la registrazione e impostare le intestazioni appropriate per evitare molte minacce di sicurezza comuni.

Nelle seguenti sottosezioni, delineiamo i cambiamenti più importanti che dovrebbe fare alla sua app.

> [!NOTE]
> Ci sono altri suggerimenti utili nei documenti di Express — veda [Migliori pratiche di produzione: prestazioni e affidabilità](https://expressjs.com/en/advanced/best-practice-performance.html) e [Migliori pratiche di produzione: sicurezza](https://expressjs.com/en/advanced/best-practice-security.html).

### Configurazione del database

Finora in questo tutorial, abbiamo utilizzato un unico database di sviluppo, per il quale l'indirizzo e le credenziali sono hard-coded in **app.js**. Poiché il database di sviluppo non contiene alcuna informazione a cui teniamo a essere esposta o corrotta, non c'è un rischio particolare nel divulgare questi dettagli. Tuttavia, se sta lavorando con dati reali, in particolare informazioni personali degli utenti, allora proteggere le credenziali del database è molto importante.

Per questo motivo vogliamo utilizzare un database diverso per la produzione rispetto a quello che utilizziamo per lo sviluppo, e anche mantenere le credenziali del database di produzione separate dal codice sorgente in modo che possano essere adeguatamente protette.

Se il suo fornitore di hosting supporta la configurazione di variabili d'ambiente tramite un'interfaccia web (come molti fanno), un modo per farlo è far ottenere al server l'URL del database da una variabile d'ambiente. Di seguito modifichiamo il sito web LocalLibrary per ottenere l'URI del database da una variabile d'ambiente dell'OS, se definita, altrimenti utilizzare l'URL del database di sviluppo.

Apri **app.js** e trova la linea che imposta la variabile di connessione a MongoDB. Sembrerà qualcosa di simile a questo:

```js
const mongoDB =
  "mongodb+srv://your_user_name:your_password@cluster0.cojoign.mongodb.net/local_library?retryWrites=true&w=majority";
```

Sostituisci la linea con il seguente codice che utilizza `process.env.MONGODB_URI` per ottenere la stringa di connessione da una variabile d'ambiente chiamata `MONGODB_URI` se è stata impostata (usa il tuo stesso URL del database al posto del placeholder qui sotto).

```js
// Set up mongoose connection
const mongoose = require("mongoose");
mongoose.set("strictQuery", false);

const dev_db_url =
  "mongodb+srv://your_user_name:your_password@cluster0.cojoign.mongodb.net/local_library?retryWrites=true&w=majority";
const mongoDB = process.env.MONGODB_URI || dev_db_url;

main().catch((err) => console.log(err));
async function main() {
  await mongoose.connect(mongoDB);
}
```

> [!NOTE]
> Un altro modo comune per mantenere le credenziali del database di produzione separate dal codice sorgente è leggerle da un file `.env` che viene distribuito separatamente nel file system (ad esempio, possono essere lette usando il modulo npm [dotenv](https://www.npmjs.com/package/dotenv)).

### Impostare NODE_ENV su 'production'

Possiamo rimuovere le tracce dello stack nelle pagine di errore impostando la variabile d'ambiente `NODE_ENV` su _production_ (è impostata su '_development_' per default). Oltre a generare messaggi di errore meno dettagliati, l'impostazione della variabile su _production_ memorizza nella cache i modelli di visualizzazione e i file CSS generati da estensioni CSS. I test indicano che impostare `NODE_ENV` su _production_ può migliorare le prestazioni dell'app di un fattore di tre!

Questa modifica può essere effettuata tramite `export`, un file di ambiente, o il sistema di inizializzazione dell'OS.

> [!NOTE]
> Questo è in realtà un cambiamento che si effettua nella configurazione dell'ambiente piuttosto che nell'app, ma abbastanza importante da notare qui! Mostreremo come impostarlo per il nostro esempio di hosting qui sotto.

### Registrare in modo appropriato

Le chiamate di registrazione possono avere un impatto su un sito web ad alta frequenza. In un ambiente di produzione, potrebbe dover registrare l'attività del sito web (ad esempio, tracciare il traffico o registrare le chiamate API) ma dovrebbe cercare di minimizzare la quantità di registrazioni aggiunte per scopi di debug.

Un modo per minimizzare la registrazione "debug" in produzione è utilizzare un modulo come [debug](https://www.npmjs.com/package/debug) che le permette di controllare quale registrazione viene eseguita impostando una variabile d'ambiente. Ad esempio, il frammento di codice qui sotto mostra come si potrebbe impostare la registrazione "author". La variabile di debug viene dichiarata con il nome 'author', e il prefisso "author" verrà automaticamente visualizzato per tutte le registrazioni di questo oggetto.

```js
const debug = require("debug")("author");

// Display Author update form on GET.
exports.author_update_get = asyncHandler(async (req, res, next) => {
  const author = await Author.findById(req.params.id).exec();
  if (author === null) {
    // No results.
    debug(`id not found on update: ${req.params.id}`);
    const err = new Error("Author not found");
    err.status = 404;
    return next(err);
  }

  res.render("author_form", { title: "Update Author", author: author });
});
```

Puoi quindi abilitare un particolare set di registrazioni specificandole come una lista separata da virgola nella variabile d'ambiente `DEBUG`. Può impostare le variabili per visualizzare le registrazioni di autore e libro come mostrato (sono supportati anche caratteri jolly).

```bash
#Windows
set DEBUG=author,book

#Linux
export DEBUG="author,book"
```

> [!NOTE]
> Le chiamate a `debug` possono sostituire la registrazione che potrebbe aver effettuato tramite `console.log()` o `console.error()`. Sostituisca qualsiasi chiamata a `console.log()` nel suo codice con la registrazione tramite il modulo [debug](https://www.npmjs.com/package/debug). Attiva e disattiva la registrazione nel tuo ambiente di sviluppo impostando la variabile DEBUG e osserva l'impatto che ciò ha sulla registrazione.

Se ha bisogno di registrare l'attività del sito web, può usare una libreria di registrazione come _Winston_ o _Bunyan_. Per ulteriori informazioni su questo argomento vedere: [Migliori pratiche di produzione: prestazioni e affidabilità](https://expressjs.com/en/advanced/best-practice-performance.html).

### Utilizzare la compressione gzip/deflate per le risposte

I server web possono spesso comprimere la risposta HTTP inviata indietro a un client, riducendo significativamente il tempo richiesto al client per ottenere e caricare la pagina. Il metodo di compressione utilizzato dipenderà dai metodi di decompressione che il client dice di supportare nella richiesta (la risposta verrà inviata non compressa se non sono supportati metodi di compressione).

Aggiunga questo al suo sito utilizzando il middleware [compression](https://www.npmjs.com/package/compression). Installalo nella radice del suo progetto eseguendo il seguente comando:

```bash
npm install compression
```

Apri **./app.js** e richiedi la libreria di compressione come mostrato. Aggiungi la libreria di compressione alla catena di middleware con il metodo `use()` (questo dovrebbe apparire prima di qualsiasi percorso che desideri comprimere — in questo caso, tutti!)

```js
const catalogRouter = require("./routes/catalog"); // Import routes for "catalog" area of site
const compression = require("compression");

// Create the Express application object
const app = express();

// …

app.use(compression()); // Compress all routes

app.use(express.static(path.join(__dirname, "public")));

app.use("/", indexRouter);
app.use("/users", usersRouter);
app.use("/catalog", catalogRouter); // Add catalog routes to middleware chain.

// …
```

> [!NOTE]
> Per un sito web ad alto traffico in produzione non userebbe questo middleware. In alternativa, utilizzi un proxy inverso come [Nginx](https://nginx.org/).

### Utilizzare Helmet per proteggere contro vulnerabilità conosciute

[Helmet](https://www.npmjs.com/package/helmet) è un pacchetto middleware. Può impostare intestazioni HTTP appropriate che aiutano a proteggere la sua app dalle vulnerabilità web conosciute (vedere i [documenti](https://helmetjs.github.io/) per maggiori informazioni su quali intestazioni imposta e contro quali vulnerabilità protegge).

Installalo nella radice del suo progetto eseguendo il seguente comando:

```bash
npm install helmet
```

Apri **./app.js** e richiedi la libreria _helmet_ come mostrato. Quindi aggiungi il modulo alla catena di middleware con il metodo `use()`.

```js
const compression = require("compression");
const helmet = require("helmet");

// Create the Express application object
const app = express();

// Add helmet to the middleware chain.
// Set CSP headers to allow our Bootstrap and jQuery to be served
app.use(
  helmet.contentSecurityPolicy({
    directives: {
      "script-src": ["'self'", "code.jquery.com", "cdn.jsdelivr.net"],
    },
  }),
);

// …
```

Normalmente potremmo aver appena inserito `app.use(helmet());` per aggiungere il _sottoinsieme_ di intestazioni relative alla sicurezza che hanno senso per la maggior parte dei siti. Tuttavia nel modello base di [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/LocalLibrary_base_template) includiamo alcuni script di bootstrap e jQuery. Questi violano la \_Configurazione predefinita della policy di sicurezza dei contenuti di helmet (CSP)](/it/docs/Web/HTTP/Guides/CSP), che non consente il caricamento di script cross-site. Per permettere il caricamento di questi script modifichiamo la configurazione di helmet in modo che imposti direttive CSP per consentire il caricamento degli script dai domini indicati. Per il proprio server può aggiungere/disabilitare intestazioni specifiche come necessario seguendo le [istruzioni per l'uso di helmet qui](https://www.npmjs.com/package/helmet).

### Aggiungere il rate limiting ai percorsi API

[Express-rate-limit](https://www.npmjs.com/package/express-rate-limit) è un pacchetto middleware che può essere utilizzato per limitare le richieste ripetute alle API e agli endpoint. Ci sono molte ragioni per cui possono essere fatte richieste eccessive al suo sito, come attacchi di negazione del servizio, attacchi di forza bruta o anche solo un client o uno script che non si comportano come previsto. Oltre ai problemi di prestazioni che possono derivare da troppe richieste che rallentano il suo server, potrebbe anche essere addebitato per il traffico aggiuntivo. Questo pacchetto può essere utilizzato per limitare il numero di richieste che possono essere effettuate su un particolare percorso o serie di percorsi.

Installalo nella radice del tuo progetto eseguendo il seguente comando:

```bash
npm install express-rate-limit
```

Apri **./app.js** e richiedi la libreria _express-rate-limit_ come mostrato. Quindi aggiungi il modulo alla catena di middleware con il metodo `use()`.

```js
const compression = require("compression");
const helmet = require("helmet");

const app = express();

// Set up rate limiter: maximum of twenty requests per minute
const RateLimit = require("express-rate-limit");
const limiter = RateLimit({
  windowMs: 1 * 60 * 1000, // 1 minute
  max: 20,
});
// Apply rate limiter to all requests
app.use(limiter);

// …
```

Il comando di cui sopra limita tutte le richieste a 20 al minuto (può cambiarlo come necessario).

> [!NOTE]
> Servizi di terze parti come [Cloudflare](https://www.cloudflare.com/) possono anche essere utilizzati se necessita di una protezione più avanzata contro attacchi di negazione del servizio o altri tipi di attacchi.

#### Impostare la versione di Node

Per le applicazioni Node, compreso Express, il file **package.json** contiene tutto ciò che un fornitore di hosting dovrebbe avere bisogno per determinare le dipendenze dell'applicazione e il file di punto d'ingresso.

L'unica informazione importante mancante dal nostro attuale **package.json** è la versione di Node richiesta dalla libreria. Puoi trovare la versione di Node utilizzata per lo sviluppo inserendo il comando:

```bash
>node --version
v16.17.1
```

Apri **package.json**, e aggiungi questa informazione come **engines > node** come mostrato (usando il numero di versione del suo sistema).

```json
  "engines": {
    "node": ">=16.17.1"
  },
```

Il servizio di hosting potrebbe non supportare la specifica versione indicata di Node, ma questa modifica dovrebbe garantire che tenti di usare una versione con lo stesso numero di versione principali, o una versione più recente.

Nota che ci possono essere altri modi per specificare la versione di Node su diversi servizi di hosting, ma l'approccio **package.json** è ampiamente supportato.

#### Ottenere dipendenze e ritestare

Prima di procedere, testiamo nuovamente il sito e assicuriamoci che non sia stato influenzato da nessuna delle nostre modifiche.

Per prima cosa, dovremo ottenere le nostre dipendenze. Può farlo eseguendo il seguente comando nel suo terminale alla radice del progetto:

```bash
npm install
```

Ora esegui il sito (vedi [Testare i percorsi](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/routes#testing_the_routes) per i comandi rilevanti) e controlla che il sito si comporti ancora come ti aspetti.

### Creare un repository applicativo su GitHub

Molti servizi di hosting le consentono di importare e/o sincronizzare progetti da un repository locale o da piattaforme di gestione delle versioni del codice sorgente basate su cloud. Questo può rendere la distribuzione e lo sviluppo iterativo molto più facili.

Per questo tutorial configureremo un account [GitHub](https://github.com/) e un repository per la libreria, e utilizzeremo lo strumento **git** per caricare il nostro codice sorgente.

> [!NOTE]
> Può saltare questo passaggio se sta già usando GitHub per gestire il suo codice sorgente!
>
> Nota che l'uso di strumenti di gestione del codice sorgente è una buona pratica di sviluppo software, in quanto le permette di provare cambiamenti e passare tra i suoi esperimenti e "codice noto buono" quando necessario!

I passaggi sono:

1. Visiti <https://github.com/> e crei un account.
2. Una volta effettuato l'accesso, clicchi sul link **+** nella barra degli strumenti superiore e selezioni **Nuovo repository**.
3. Compili tutti i campi su questo modulo. Sebbene questi non siano obbligatori, sono fortemente raccomandati.

   - Inserisca un nuovo nome per il repository (ad es., _express-locallibrary-tutorial_), e una descrizione (ad es. "Local Library website written in Express (Node)").
   - Scegli **Node** nella lista di selezione _Aggiungi .gitignore_.
   - Scegli la tua licenza preferita nella lista di selezione _Aggiungi licenza_.
   - Segni **Inizializza questo repository con un README**.

   > [!WARNING]
   > L'accesso "Pubblico" predefinito renderà _tutto_ il codice sorgente — comprese le credenziali del nome utente e della password del database — visibile a chiunque su Internet! Assicuri che il codice sorgente legga le credenziali _solo_ dalle variabili d'ambiente e non abbia credenziali hard-coded.
   >
   > Altrimenti, selezioni l'opzione "Privato" per consentire solo a persone selezionate di vedere il codice sorgente.

4. Premi **Crea repository**.
5. Clicchi sul pulsante verde **Clona o scarica** nella pagina del suo nuovo repository.
6. Copi il valore URL dalla casella di testo nel dialogo che appare. Se ha utilizzato il nome del repository "express-locallibrary-tutorial", l'URL dovrebbe essere qualcosa di simile a `https://github.com/<your_git_user_id>/express-locallibrary-tutorial.git`.

Ora che il repository ("repo") è creato su GitHub, vogliamo clonarlo (copiarlo) sul nostro computer locale:

1. Installi _git_ sul suo computer locale ([guida ufficiale al download di Git](https://git-scm.com/downloads)).
2. Apra un prompt dei comandi/terminale e cloni il suo repository utilizzando l'URL copiato sopra:

   ```bash
   git clone https://github.com/<your_git_user_id>/express-locallibrary-tutorial.git
   ```

   Questo creerà il repository all'interno della directory corrente.

3. Naviga nella cartella del repository.

   ```bash
   cd express-locallibrary-tutorial
   ```

Poi copi i file sorgente dell'applicazione nella cartella del repository, faccia diventare i file parte del repository usando _git_, e caricali su GitHub:

1. Copi la sua applicazione Express in questa cartella (escluso **/node_modules**, che contiene file di dipendenza che dovrebbe ottenere da npm come necessario).
2. Apri un prompt dei comandi/terminale e usi il comando `add` per aggiungere tutti i file a git.

   ```bash
   git add -A
   ```

3. Usi il comando `status` per controllare che tutti i file che sta per `commit` siano corretti (vuole includere file sorgente, non binari, file temporanei ecc.). Dovrebbe apparire un po' come l'elenco riportato di seguito.

   ```bash
   git status
   ```

   ```plain
   On branch main
   Your branch is up-to-date with 'origin/main'.
   Changes to be committed:
     (use "git reset HEAD <file>..." to unstage)

           new file:   ...
   ```

4. Quando è soddisfatto, `commit` i file nel suo repository locale. Questo è equivalente all'approvazione delle modifiche e alla dichiarazione di farle parte ufficialmente del repository locale.

   ```bash
   git commit -m "First version of application moved into GitHub"
   ```

5. A questo punto, il repository remoto non è stato modificato. L'ultimo passaggio è sincronizzare (`push`) il suo repository locale al repository remoto su GitHub utilizzando il seguente comando:

   ```bash
   git push origin main
   ```

Quando l'operazione è completata, dovrebbe essere in grado di tornare alla pagina su GitHub dove ha creato il suo repository, aggiornare la pagina, e vedere che ora tutta la sua applicazione è stata caricata. Può continuare ad aggiornare il suo repository man mano che i file cambiano utilizzando questo ciclo di add/commit/push.

Questo è un buon momento per fare un backup del suo progetto "vanilla" — mentre alcune delle modifiche che faremo nelle sezioni seguenti potrebbero essere utili per la distribuzione su qualsiasi servizio di hosting (o per lo sviluppo), altre potrebbero non esserlo. Può fare questo utilizzando `git` sulla riga di comando:

```bash
# Create branch vanilla_deployment from the current branch (main)
git checkout -b vanilla_deployment

# Push the new branch to GitHub
git push origin vanilla_deployment

# Switch back to main
git checkout main

# Make any further changes in a new branch
git pull upstream main # Merge the latest changes from GitHub
git checkout -b my_changes # Create a new branch
```

> [!NOTE]
> Git è incredibilmente potente! Per saperne di più, veda [Imparare Git](https://docs.github.com/en/get-started/start-your-journey/git-and-github-learning-resources).

## Esempio: Hosting su Glitch

Questa sezione fornisce una dimostrazione pratica su come ospitare _LocalLibrary_ su [Glitch](https://glitch.com/).

### Perché Glitch?

Scegliamo di utilizzare Glitch per diverse ragioni:

- Glitch ha un [piano iniziale gratuito](https://glitch.com/pricing) che è _davvero_ gratuito, sebbene con alcune limitazioni. Il fatto che sia accessibile a tutti gli sviluppatori è davvero importante per MDN!
- Glitch si occupa dell'infrastruttura in modo che non debba farlo lei. Non dover preoccuparsi di server, bilanciatori di carico, proxy inversi, ecc., rende molto più facile iniziare.
- Le competenze e i concetti che imparerà utilizzando Glitch sono trasferibili.
- Le limitazioni di servizio e di piano non impattano davvero sull'utilizzo di Glitch per il tutorial. Ad esempio:

  - Il piano iniziale offre solo 1000 "ore di progetto" al mese, che viene resettato mensilmente. Questo tempo viene utilizzato quando si sta attivamente modificando il sito o se qualcuno lo sta accedendo. Se nessuno sta accedendo o modificando il sito, esso andrà a dormire.
  - L'ambiente del piano iniziale ha una quantità limitata di RAM e spazio di archiviazione del contenitore. Ce n'è più a sufficienza per il tutorial, in particolare perché il nostro database è ospitato altrove.
  - I domini personalizzati non sono ben supportati (al momento della scrittura).
  - Altre limitazioni possono essere trovate nella [pagina delle restrizioni tecniche di Glitch](https://help.glitch.com/s/article/Technical-Restrictions).

Mentre Glitch è appropriato per ospitare questa dimostrazione, dovrebbe prendersi il tempo per determinare se è [adatto per il suo sito web](#scegliere_un_fornitore_di_hosting).

### Come funziona Glitch?

Glitch fornisce un'interfaccia basata sul web in cui può creare progetti da modelli di partenza, o importarli da GitHub, e quindi aggiungere e modificare i file di progetto. Mentre apporta modifiche, il progetto viene costruito ed eseguito nel proprio contenitore virtualizzato isolato e indipendente.

Come tutto questo funzioni "sotto il cofano" è un mistero — Glitch non lo dice. Quello che è chiaro è che finché crea un'applicazione web nodejs abbastanza standard (ad esempio, utilizzando `package.json` per le sue dipendenze), e non consuma più risorse di quelle elencate nelle [restrizioni tecniche](https://help.glitch.com/s/article/Technical-Restrictions), la sua applicazione dovrebbe "funzionare".

Una volta che l'applicazione è in esecuzione, può essere configurata per la produzione utilizzando [dati privati](https://help.glitch.com/s/article/Adding-Private-Data) forniti in un file `.env`. I valori nei dati segreti vengono letti dall'applicazione come variabili d'ambiente, che, come ricorderà da una sezione precedente, è come abbiamo configurato la nostra applicazione per ottenere il suo URL del database. Nota che le variabili sono _segrete_: il file `.env` non deve essere incluso nel suo repository GitHub.

La vista di modifica di Glitch fornisce anche l'accesso _al terminale_ all'ambiente web app, che potrà usare per lavorare con l'app web come se fosse in esecuzione sul suo PC locale.

Questo è tutto ciò che bisogna sapere per iniziare. Successivamente, configuriamo un account Glitch, carichiamo il progetto della biblioteca da GitHub e lo connettiano a un database.

### Ottenere un account Glitch

Per iniziare a usare Glitch, dovrà prima creare un account:

- Vada su [glitch.com](https://glitch.com/) e clicchi il pulsante **Sign up** nella barra degli strumenti superiore.
- Scelga GitHub nel popup per iscriversi utilizzando le credenziali di GitHub.
- Sarà quindi connesso alla dashboard di Glitch: <https://glitch.com/dashboard>.

### Risolvere i problemi di versione di Node.js

I fornitori di hosting supportano comunemente alcune principali versioni delle versioni recenti di Node.js. Se la "minor" esatta che ha specificato nel suo file `package.json` non è supportata, di solito passeranno alla versione più vicina che supportano (e spesso funzionerà).

Sfortunatamente, al momento della scrittura, la versione più alta supportata su Glitch è Node.js 16. Se ha sviluppato con Node.js 17 o successivi, dovrebbe ridurre la versione usata nel suo file `package.json` come mostrato. Dovrà anche ritestare:

```json
  "engines": {
    "node": ">=v16"
  },
```

Glitch [pianifica di aggiornare Node e farlo mantenere meglio aggiornato in futuro](https://blog.glitch.com/post/rebuilding-glitch/) — e potrebbe essere che al momento della lettura di questo il limite di versione non esista più. Invece di fare il downgrade della versione di `node`, potrebbe caricare il suo progetto per vedere se si costruisce. Se ci sono errori e la sua applicazione non si carica, dovrebbe provare a impostare la versione di `node` a `>=v16` nel suo `package.json` nell'editor Glitch.

> [!NOTE]
> Può anche controllare le versioni supportate inserendo il seguente comando nel terminale di qualsiasi progetto Glitch:
>
> ```sh
> ls -l /opt/nvm/versions/node | grep '^d' | awk '{ print $9 }'
> ```

### Distribuire su Glitch da GitHub

Successivamente importeremo il progetto della libreria da GitHub. Prima scelga l'opzione **Dashboard** dal menu superiore del sito, quindi selezioni il pulsante **New project**. Glitch mostrerà un elenco di opzioni per il nuovo progetto; selezioni **Import from GitHub**.

![Dashboard del sito web di Glitch che mostra un nuovo pulsante di progetto e un menu popup con opzione "Import from GitHub"](glitch_new_project_import_github.png)

Apparirà un popup. Inserisca l'URL del proprio repository GitHub nel popup e preme **OK**. Di seguito, abbiamo inserito il repository per il progetto funzionante.

![Popup di Glitch per l'inserimento dell'URL del repository GitHub da importare](glitch_new_project_github_repo_url.png)

Glitch importerà quindi il progetto, visualizzando notifiche di progresso. Al termine, visualizzerà la vista di modifica per il nuovo progetto, come mostrato di seguito.

![Vista dell'editor di Glitch per il progetto importato](glitch_imported_project_in_editor.png)

Può ottenere l'URL del sito live selezionando il pulsante **Share**.

![Vista dell'editor di Glitch per il progetto importato](glitch_share_project.png)

Apra una nuova scheda del browser e copii il link per il sito live nella barra degli indirizzi. Il sito della biblioteca locale dovrebbe aprirsi e visualizzare i dati dal database di sviluppo.

> [!NOTE]
> Questo processo è stato un import una tantum da GitHub. Può anche utilizzare azioni di GitHub come [glitch-project-sync](https://github.com/marketplace/actions/glitch-project-sync) per mantenere sincronizzati Glitch e il suo progetto.

### Utilizzare un database MongoDB di produzione

Dovrebbe configurare un database diverso per la produzione rispetto allo sviluppo. Mentre Glitch ospita solo database SQLite (e siamo configurati per utilizzare MongoDB), molti altri siti forniscono database MongoDB come servizio.

Una opzione è seguire le istruzioni sulla [Configurazione del database MongoDB](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose#setting_up_the_mongodb_database) dalla parte precedente del tutorial per configurare un nuovo database di produzione.

Per rendere il database di produzione accessibile all'applicazione della biblioteca, apri il file `.env` nella vista dell'editor del progetto. Inserisca la variabile dell'URL del database `MONGODB_URI` e l'URL del suo database di produzione. Il sito si aggiorna mentre si inseriscono i valori nell'editor.

![Editor file .env di Glitch per dati privati con variabili di produzione](glitch_env.png)

> [!NOTE]
> Non abbiamo creato questo file. È destinato ai [dati privati](https://help.glitch.com/s/article/Adding-Private-Data) ed è stato creato automaticamente all'importazione su Glitch. Non viene mai esportato o copiato.

### Altre variabili di configurazione

Si ricorderà da una sezione precedente che dobbiamo [impostare NODE_ENV su 'production'](#set_node_env_to_production) per migliorare le nostre prestazioni e generare messaggi di errore meno dettagliati. Facciamo questo nello stesso file in cui impostiamo la variabile `MONGODB_URI`.

Aprire `.env` e aggiungere una variabile `NODE_ENV` con il valore `production` (vedere lo screenshot nella sezione precedente).

L'applicazione della biblioteca locale è ora configurata e configurata per l'uso in produzione. Può aggiungere dati tramite l'interfaccia del sito web e dovrebbe funzionare come durante lo sviluppo (sebbene con meno informazioni di debug esposte per le pagine non valide).

> [!NOTE]
> Se vuole solo aggiungere alcuni dati per il test, potrebbe utilizzare lo script `populatedb` (con il suo URL del database di produzione MongoDB) come discusso nella sezione [Tutorial di Express Parte 3: Usare un Database (con Mongoose) Test — creare alcuni elementi](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose#testing_%E2%80%94_create_some_items).

### Debug delle app Express su Glitch

Glitch consente un debug efficace. Alcune delle cose che può fare sono:

- Selezioni il pulsante log nella parte inferiore della vista dell'editor per vedere le informazioni di log dal suo server, come l'output del log della console.
- Selezioni il pulsante terminal nella parte inferiore della vista dell'editor per aprire un terminale nell'ambiente di hosting. Può usare questo per eseguire comandi e strumenti nell'ambiente. Ad esempio, potrebbe usare `node -v` per controllare la versione di node.
- Debug interattivo in VS Code usando l'estensione GLITCH per VS Code.

## Esempio: Hosting su Railway

Questa sezione fornisce una dimostrazione pratica di come installare _LocalLibrary_ su [Railway](https://railway.com/).

### Perché Railway?

> [!WARNING]
> Railway non ha più un livello iniziale completamente gratuito. Abbiamo mantenuto queste istruzioni perché Railway ha alcune funzionalità interessanti e sarà una migliore opzione per alcuni utenti.

Railway è un'opzione di hosting attraente per diverse ragioni:

- Railway si occupa della maggior parte dell'infrastruttura in modo che non debba farlo lei. Non dover preoccuparsi di server, bilanciatori di carico, proxy inversi, ecc., rende molto più facile iniziare.
- Railway ha un [focus sull'esperienza dello sviluppatore per lo sviluppo e la distribuzione](https://docs.railway.com/maturity/compare-to-heroku), che porta a una curva di apprendimento più veloce e più soft rispetto a molte altre alternative.
- Le competenze e i concetti che imparerà utilizzando Railway sono trasferibili. Anche se Railway ha alcune eccellenti nuove funzionalità, altri servizi di hosting popolari utilizzano molte delle stesse idee e approcci.
- La [documentazione di Railway](https://docs.railway.com/) è chiara e completa.
- Il servizio sembra essere molto affidabile, e se le finisce per piacere, i prezzi sono prevedibili e scalare la sua app è molto facile.

Dovrebbe prendersi il tempo per determinare se Railway è [adatto per il suo sito web](#scegliere_un_fornitore_di_hosting).

### Come funziona Railway?

Le applicazioni web vengono eseguite ciascuna nel proprio contenitore virtualizzato isolato e indipendente. Per eseguire la sua applicazione, Railway deve essere in grado di impostare l'ambiente e le dipendenze appropriate, e anche capire come avviarla.

Railway rende tutto questo facile, poiché può automaticamente riconoscere e installare molti diversi framework ed ambienti per applicazioni web basati sul loro utilizzo di "convenzioni comuni". Ad esempio, Railway riconosce le applicazioni node perché hanno un file **package.json**, e può determinare il gestore di pacchetti utilizzato per la costruzione dal file "lock". Ad esempio, se l'applicazione include il file **package-lock.json** Railway sa di usare _npm_ per installare i pacchetti, mentre se trova **yarn.lock** sa di usare _yarn_. Dopo aver installato tutte le dipendenze, Railway cercherà script chiamati "build" e "start" nel file dei pacchetti e utilizzerà questi per costruire ed eseguire il codice.

> [!NOTE]
> Railway usa [Nixpacks](https://nixpacks.com/docs) per riconoscere vari framework di applicazioni web scritte in diversi linguaggi di programmazione. Non ha bisogno di sapere nient'altro per questo tutorial, ma può trovare ulteriori informazioni sulle opzioni per distribuire applicazioni node in [Nixpacks Node](https://nixpacks.com/docs/providers/node).

Una volta che l'applicazione è in esecuzione, può configurarsi usando le informazioni fornite nelle [variabili d'ambiente](https://docs.railway.com/guides/variables). Ad esempio, un'applicazione che utilizza un database deve ottenere l'indirizzo utilizzando una variabile. Il servizio del database stesso può essere ospitato da Railway o da qualche altro fornitore.

Gli sviluppatori interagiscono con Railway tramite il sito Railway, e utilizzando un particolare [interfaccia a riga di comando (CLI)](https://docs.railway.com/guides/cli). La CLI le permette di associare un repository GitHub locale a un progetto ferroviario, caricare il repository dal ramo locale al sito live, ispezionare i log del processo in esecuzione, impostare e ottenere variabili di configurazione e molto altro. Una delle caratteristiche più utili è che può usare la CLI per eseguire il suo progetto locale con le stesse variabili d'ambiente del progetto live.

Questo è tutto ciò di cui ha bisogno di sapere per distribuire l'app su Railway. Successivamente configureremo un account Railway, installeremo il nostro sito web e un database, e proveremo il client Railway.

### Ottenere un account Railway

Per iniziare a usare Railway dovrà prima creare un account:

- Vada su [railway.com](https://railway.com/) e clicchi il link **Login** nella barra degli strumenti superiore.
- Selezioni GitHub nel popup per accedere utilizzando le proprie credenziali di GitHub
- Potrebbe quindi dover accedere alla sua email e verificare il suo account.
- Sarà quindi collegato alla dashboard di Railway.com: <https://railway.com/dashboard>.

### Distribuire su Railway da GitHub

Successivamente configureremo Railway per distribuire la nostra libreria da GitHub. Prima scelga l'opzione **Dashboard** dal menu superiore del sito, quindi selezioni il pulsante **New Project**:

![Dashboard del sito web di Railway che mostra un nuovo pulsante di progetto](railway_new_project_button.png)

Railway mostrerà un elenco di opzioni per il nuovo progetto, inclusa l'opzione per distribuire un progetto da un modello che viene prima creato nel suo account GitHub, e diversi database. Selezioni **Deploy from GitHub repo**.

![Popup di Railway che mostra le opzioni di distribuzione con l'opzione Deploy from GitHub repo evidenziata](railway_new_project_button_deploy_github_repo.png)

Tutti i progetti nei repository GitHub che ha condiviso con Railway durante il setup vengono visualizzati. Selezioni il suo repository GitHub per la libreria locale: `<user-name>/express-locallibrary-tutorial`.

![Popup di Railway che mostra i repository GitHub che possono essere distribuiti](railway_new_project_button_deploy_github_selectrepo.png)

Confermi la distribuzione selezionando **Deploy Now**.

![Schermata di conferma quando può selezionare la distribuzione del progetto](railway_new_project_deploy_confirm.png)

Railway caricherà quindi e distribuirà il suo progetto, visualizzando il progresso nella tab delle distribuzioni. Quando la distribuzione sarà completata con successo, vedrà una schermata come quella qui sotto.

![Dashboard di Railway che mostra la scheda Deployments per il progetto distribuito](railway_project_deploy.png)

Ora selezioni la scheda _Impostazioni_ (Settings), poi scorri in basso fino alla sezione Domini, e premi il pulsante **Generate Domain**.

![Scheda delle impostazioni del progetto di Railway che mostra il pulsante per generare un dominio](railway_project_generate_domain.png)

Questo pubblicherà il sito e metterà il dominio al posto del pulsante, come mostrato qui sotto.

![Scheda delle impostazioni del progetto di Railway che mostra un link al sito della biblioteca locale](railway_project_domain.png)

Selezioni l'URL del dominio per aprire la sua applicazione della biblioteca. Nota che poiché non abbiamo specificato un database di produzione, la biblioteca locale si aprirà utilizzando i dati di sviluppo.

### Concedere e connettere un database MongoDB

Invece di utilizzare i nostri dati di sviluppo, ora creiamo un database MongoDB di produzione da utilizzare al suo posto. Creeremo il database come parte del progetto dell'applicazione Railway, anche se nulla ci impedisce di crearlo nel proprio progetto separato, o persino di utilizzare un database _MongoDB Atlas_ per i dati di produzione, proprio come abbiamo fatto per il database di sviluppo.

Su Railway, sceglia l'opzione **Dashboard** dal menu superiore del sito e poi selezioni il suo progetto di applicazione. A questo punto contiene solo un unico servizio per la sua applicazione (questo può essere selezionato per impostare variabili e altri dettagli del servizio). Selezioni il pulsante **New**, che viene utilizzato per aggiungere servizi al progetto corrente.

![Progetto Railway con nuovo pulsante del servizio evidenziato](railway_project_open_no_database.png)

Selezioni **Database** quando viene chiesto il tipo di servizio da aggiungere:

![Popup di Railway che mostra le opzioni per un nuovo servizio, come database, repository GitHub, servizio vuoto ecc.](railway_database_add.png)

Quindi selezioni **Add MongoDB** per iniziare ad aggiungere il database

![Popup di Railway che mostra diversi database che possono essere selezionati: Postgres, MySQL, MongoDB e così via](railway_database_select_type.png)

Railway fornirà quindi un servizio che contiene un database vuoto nello stesso progetto. Al completamento ora vedrà entrambi i servizi dell'applicazione e del database nella vista del progetto.

![Progetto Railway con servizi dell'applicazione e del database](railway_project_two_services.png)

Selezioni il servizio MongoDB per visualizzare informazioni sul database. Apra la scheda _Variables_ (Variabili) e copi l'"Mongo_URL" (questo è l'indirizzo del database).

![Schermata delle impostazioni del database di Railway che mostra l'URL necessario per connettersi al database](railway_mongodb_connect.png)

Per renderlo accessibile all'applicazione della biblioteca lo aggiunga al processo applicativo utilizzando una variabile d'ambiente. Prima apra il servizio dell'applicazione. Poi selezioni la scheda _Variables_ (Variabili) e preme il pulsante **New Variable** (Nuova Variabile).

Inserisca il nome della variabile `MONGODB_URI` e l'URL di connessione che ha copiato per il database (`MONGODB_URI` è il nome della variabile d'ambiente da cui [abbiamo configurato l'applicazione](#configurazione_del_database) per leggere l'indirizzo del database). Sarà simile alla schermata mostrata di seguito.

![Schermata delle variabili del sito web di Railway mentre si aggiunge la variabile MONGODB_URI e l'indirizzo](railway_variables_database_url.png)

Selezioni **Add** (Aggiungi) per aggiungere la variabile.

Railway riavvia sua app quando aggiorna le variabili. Se controlla la home page ora dovrebbe mostrare zero valori per i suoi articoli contati, poiché le modifiche sopra indicano che ora stiamo utilizzando un nuovo database (vuoto).

### Altre variabili di configurazione

Si ricorderà da una sezione precedente che dobbiamo [impostare NODE_ENV su 'production'](#set_node_env_to_production) per migliorare le nostre prestazioni e generare messaggi di errore meno dettagliati. Possiamo fare questo nella stessa schermata in cui impostiamo la variabile `MONGODB_URI`.

Apra il servizio dell'applicazione. Poi selezioni la scheda _Variables_ (Variabili), dove vedrà che `MONGODB_URI` è già definito, e preme il pulsante **New Variable** (Nuova Variabile).

![Scheda delle variabili di Railway con il pulsante Nuova Variabile evidenziato](railway_variables_new.png)

Inserisca `NODE_ENV` come nome della nuova variabile e `production` come nome dell'ambiente. Poi preme il pulsante **Add**.

![Scheda delle variabili di Railway con la nuova variabile NODE_ENV impostata su 'production'](railway_variables_new_node_env.png)

L'applicazione della biblioteca locale è ora configurata per l'uso in produzione. Può aggiungere dati tramite l'interfaccia del sito web e dovrebbe funzionare nello stesso modo in cui funzionava durante lo sviluppo (sebbene con meno informazioni di debug esposte per pagine non valide).

> [!NOTE]
> Se desidera solo aggiungere alcuni dati per il test potrebbe utilizzare lo script `populatedb` (con il suo URL del database di produzione MongoDB) come discusso nella sezione [Tutorial di Express Parte 3: Usare un Database (con Mongoose) Test — creare alcuni elementi](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose#testing_%E2%80%94_create_some_items).

### Installare il client

Scarichi e installi il client Railway per il suo sistema operativo locale seguendo le [istruzioni qui](https://docs.railway.com/guides/cli).

Dopo che il client è installato sarà in grado di eseguire comandi. Alcune delle operazioni più importanti includono distribuire la directory corrente del suo computer a un progetto ferroviario associato (senza dover caricare su GitHub), ed eseguire il suo progetto localmente utilizzando le stesse impostazioni di quelle sul server di produzione.

Può ottenere un elenco di tutti i possibili comandi inserendo il seguente comando in un terminale.

```bash
railway help
```

### Debug

Il client Railway fornisce il comando di log per mostrare la coda dei log (un log più ampio è disponibile sul sito per ciascun progetto):

```bash
railway logs
```

## Sommario

Questo è la fine di questo tutorial su come configurare le app Express in produzione, ed è anche la fine della serie di tutorial su come lavorare con Express. Speriamo che li abbia trovati utili. Può controllare una versione completamente lavorata del [codice sorgente su GitHub qui](https://github.com/mdn/express-locallibrary-tutorial).

## Vedere anche

- [Migliori pratiche di produzione: prestazioni e affidabilità](https://expressjs.com/en/advanced/best-practice-performance.html) (documenti di Express)
- [Migliori pratiche di produzione: sicurezza](https://expressjs.com/en/advanced/best-practice-security.html) (documenti di Express)
- Documenti di Railway

  - [CLI](https://docs.railway.com/guides/cli)

- DigitalOcean

  - [Express](https://www.digitalocean.com/community/tutorials?q=express) tutorial
  - [Node.js](https://www.digitalocean.com/community/tutorials?q=node.js) tutorial

- Heroku

  - [Getting Started on Heroku with Node.js](https://devcenter.heroku.com/articles/getting-started-with-nodejs) (documenti di Heroku)
  - [Deploying Node.js Applications on Heroku](https://devcenter.heroku.com/articles/deploying-nodejs) (documenti di Heroku)
  - [Heroku Node.js Support](https://devcenter.heroku.com/articles/nodejs-support) (documenti di Heroku)
  - [Optimizing Node.js Application Concurrency](https://devcenter.heroku.com/articles/node-concurrency) (documenti di Heroku)
  - [How Heroku works](https://devcenter.heroku.com/articles/how-heroku-works) (documenti di Heroku)
  - [Dynos and the Dyno Manager](https://devcenter.heroku.com/articles/dynos) (documenti di Heroku)
  - [Configuration and Config Vars](https://devcenter.heroku.com/articles/config-vars) (documenti di Heroku)
  - [Limits](https://devcenter.heroku.com/articles/limits) (documenti di Heroku)

{{PreviousMenu("Learn_web_development/Extensions/Server-side/Express_Nodejs/forms", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}
