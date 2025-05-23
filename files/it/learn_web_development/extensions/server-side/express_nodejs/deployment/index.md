---
title: "Tutorial Express Parte 7: Distribuzione in produzione"
short-title: "7: Distribuzione"
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/deployment
l10n:
  sourceCommit: 3d53de838dbcb25b210ccd708c681771cdeb14e4
---

{{PreviousMenu("Learn_web_development/Extensions/Server-side/Express_Nodejs/forms", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}

Ora che hai creato (e testato) un fantastico sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website), vorrai installarlo su un server web pubblico in modo che possa essere accessibile dal personale della biblioteca e dai membri tramite Internet. Questo articolo fornisce una panoramica di come trovare un host per distribuire il tuo sito web e cosa devi fare per preparare il sito per la produzione.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Completare tutti i precedenti argomenti del tutorial, inclusa la <a href="/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms">Tutorial Express Parte 6: Lavorare con i moduli</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare dove e come è possibile distribuire un'app Express in produzione.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

Una volta che il tuo sito è finito (o "abbastanza finito" per iniziare i test pubblici) dovrai ospitarlo in un luogo più pubblico e accessibile rispetto al tuo computer di sviluppo personale.

Fino ad ora, hai lavorato in un [ambiente di sviluppo](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment), utilizzando Express/Node come server web per condividere il tuo sito con il browser/rete locale, e eseguendo il tuo sito web con impostazioni di sviluppo (non sicure) che espongono informazioni di debug e altre informazioni private. Prima che tu possa ospitare un sito web esternamente, dovrai prima:

- Scegliere un ambiente per l'hosting dell'app Express.
- Fare alcune modifiche alle impostazioni del progetto.
- Configurare un'infrastruttura a livello di produzione per servire il tuo sito web.

Questo tutorial fornisce alcune indicazioni sulle opzioni per scegliere un sito di hosting, una breve panoramica di ciò che devi fare per preparare la tua app Express per la produzione, e un esempio pratico di come installare il sito web LocalLibrary sul servizio di cloud hosting [Railway](https://railway.com/).

## Che cos'è un ambiente di produzione?

L'ambiente di produzione è l'ambiente fornito dal computer server dove eseguirai il tuo sito web per il consumo esterno. L'ambiente include:

- Hardware del computer su cui gira il sito web.
- Sistema operativo (ad esempio, Linux o Windows).
- Runtime del linguaggio di programmazione e librerie del framework su cui è scritto il tuo sito web.
- Infrastruttura del server web, che potrebbe includere un server web, reverse proxy, load balancer, ecc.
- Database da cui il tuo sito web dipende.

Il computer server potrebbe essere situato nei tuoi locali e collegato a Internet tramite un collegamento veloce, ma è molto più comune utilizzare un computer ospitato "nel cloud". Cosa significa in realtà è che il tuo codice viene eseguito su qualche computer remoto (o possibilmente un computer "virtuale") nel data center della tua azienda di hosting. Il server remoto di solito offre un certo livello garantito di risorse di calcolo (ad esempio, CPU, RAM, memoria di archiviazione, ecc.) e connettività a Internet per un certo prezzo.

Questo tipo di hardware di calcolo/rete accessibile da remoto è definito come _Infrastructure as a Service (IaaS)_. Molti fornitori di IaaS offrono opzioni per preinstallare un particolare sistema operativo, sul quale devi installare gli altri componenti del tuo ambiente di produzione. Altri fornitori consentono di selezionare ambienti più completi, forse includendo un setup completo di Node.

> [!NOTE]
> Gli ambienti preconfigurati possono semplificare molto la configurazione del tuo sito web perché riducono la configurazione, ma le opzioni disponibili potrebbero limitarti a un server sconosciuto (o altri componenti) e potrebbero essere basati su una versione precedente del sistema operativo. Spesso è meglio installare i componenti da soli in modo da ottenere quelli che vuoi, e quando sarà necessario aggiornare parti del sistema, avrai un'idea di dove iniziare!

Altri fornitori di hosting supportano Express come parte di un'offerta _Platform as a Service_ (_PaaS_). Utilizzando questo tipo di hosting, non devi preoccuparti della maggior parte del tuo ambiente di produzione (server, load balancers, ecc.) poiché la piattaforma host se ne occupa per te. Questo rende abbastanza facile la distribuzione perché devi concentrarti solo sulla tua applicazione web e non su nessun'altra infrastruttura server.

Alcuni sviluppatori sceglieranno la maggiore flessibilità fornita dalla IaaS rispetto alla PaaS, mentre altri apprezzeranno la ridotta necessità di manutenzione e la maggiore facilità di scalare della PaaS. Quando inizierai, configurare il tuo sito su un sistema PaaS sarà molto più facile, quindi è quello che faremo in questo tutorial.

> [!NOTE]
> Se scegli un fornitore di hosting compatibile con Node/Express, dovrebbe fornire istruzioni su come configurare un sito web Express utilizzando diverse configurazioni di web server, application server, reverse proxy, ecc. Ad esempio, ci sono molte guide dettagliate per varie configurazioni nei [documenti della comunità DigitalOcean Node](https://www.digitalocean.com/community/tutorials?q=node).

## Scegliere un fornitore di hosting

Ci sono numerosi fornitori di hosting che sono noti per supportare attivamente o funzionare bene con _Node_ (e _Express_). Questi fornitori offrono diversi tipi di ambienti (IaaS, PaaS) e diversi livelli di risorse di calcolo e rete a prezzi diversi.

> [!NOTE]
> Ci sono molte soluzioni di hosting, e i loro servizi e prezzi possono cambiare nel tempo. Mentre presentiamo alcune opzioni di seguito, vale la pena controllare sia queste che altre opzioni prima di selezionare un fornitore di hosting.

Alcune cose da considerare quando si sceglie un host:

- Quanto traffico è probabile che faccia il tuo sito e il costo delle risorse di dati e di calcolo necessarie per soddisfare tale domanda.
- Livello di supporto per scalare orizzontalmente (aggiungendo più macchine) e verticalmente (aggiornando a macchine più potenti) e i costi per farlo.
- Le sedi in cui il fornitore dispone di data center, e quindi dove l'accesso è probabile che sia più veloce.
- Lo storico della disponibilità e dei tempi di inattività dell'host.
- Strumenti forniti per la gestione del sito — sono facili da usare e sono sicuri (ad esempio, SFTP vs. FTP).
- Framework integrati per monitorare il tuo server.
- Limitazioni note. Alcuni host bloccheranno deliberatamente alcuni servizi (ad esempio, email). Altri offrono solo un certo numero di ore di "tempo attivo" in alcuni livelli di prezzo, o offrono solo una piccola quantità di spazio di archiviazione.
- Benefici aggiuntivi. Alcuni fornitori offrono nomi di dominio gratuiti e supporto per certificati TLS che dovresti altrimenti pagare.
- Se il livello "gratuito" su cui ti stai affidando scade nel tempo e se il costo della migrazione a un livello più costoso significa che sarebbe stato meglio usare un altro servizio fin dall'inizio!

La buona notizia è che quando inizi, ci sono diversi siti che forniscono ambienti di calcolo "gratuiti" destinati alla valutazione e al test.
Questi sono di solito ambienti piuttosto limitati in termini di risorse ed è necessario essere consapevoli che potrebbero scadere dopo un certo periodo introduttivo o avere altre limitazioni.
Tuttavia, sono ottimi per testare siti a basso traffico in un ambiente ospitato e possono offrire una facile migrazione al pagamento per più risorse quando il tuo sito diventa più impegnato.
Scelte popolari in questa categoria includono [Glitch](https://glitch.com/), [Python Anywhere](https://www.pythonanywhere.com/), [Amazon Web Services](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/billing-free-tier.html), [Microsoft Azure](https://azure.microsoft.com/en-us/pricing/details/app-service/linux/), ecc.

La maggior parte dei fornitori offre anche un livello "base" o "hobby" pensato per piccoli siti di produzione, e che forniscono livelli più utili di potenza di calcolo e meno limitazioni.
[Railway](https://railway.com/), [Heroku](https://www.heroku.com/), [DigitalOcean](https://www.digitalocean.com/) e [Python Anywhere](https://www.pythonanywhere.com/) sono esempi di fornitori di hosting popolari che hanno un livello di calcolo base relativamente economico (nella fascia di prezzo da 5 a 10 USD al mese).

> [!NOTE]
> Ricorda che il prezzo non è l'unico criterio di selezione.
> Se il tuo sito web ha successo, potrebbe risultare che la scalabilità sia la considerazione più importante.

## Preparare il tuo sito web per la pubblicazione

Le cose principali a cui pensare quando si pubblica il tuo sito web sono la sicurezza web e le prestazioni.
Al minimo indispensabile, vorrai modificare la configurazione del database in modo da poter utilizzare un database diverso per la produzione e proteggere le sue credenziali, rimuovere le tracce dello stack incluse nelle pagine di errore durante lo sviluppo, sistemare il tuo logging e impostare gli header appropriati per evitare molte minacce comuni alla sicurezza.

Nelle sezioni seguenti, descriviamo le modifiche più importanti che dovresti apportare alla tua app.

> [!NOTE]
> Ci sono altri suggerimenti utili nei documenti di Express — vedi [Production best practices: performance and reliability](https://expressjs.com/en/advanced/best-practice-performance.html) e [Production Best Practices: Security](https://expressjs.com/en/advanced/best-practice-security.html).

### Configurazione del database

Finora in questo tutorial, abbiamo utilizzato un unico database di sviluppo, per il quale l'indirizzo e le credenziali sono codificati in **app.js**.
Poiché il database di sviluppo non contiene informazioni che ci preoccupiamo di esporre o corrompere, non vi è alcun rischio particolare nel far trapelare questi dettagli.
Tuttavia, se stai lavorando con dati reali, in particolare informazioni personali degli utenti, allora proteggere le credenziali del tuo database è molto importante.

Per questa ragione vogliamo utilizzare un database diverso per la produzione rispetto a quello usato per lo sviluppo e tenere anche le credenziali del database di produzione separate dal codice sorgente in modo che possano essere adeguatamente protette.

Se il tuo fornitore di hosting supporta l'impostazione delle variabili d'ambiente tramite un'interfaccia web (come molti fanno), un modo per farlo è far sì che il server ottenga l'URL del database da una variabile d'ambiente.
Di seguito modifichiamo il sito web LocalLibrary in modo da ottenere l'URI del database da una variabile d'ambiente del sistema operativo, se è stato definito, e utilizzare altrimenti l'URL del database di sviluppo.

Apri **app.js** e trova la riga che imposta la variabile di connessione MongoDB.
Sarà simile a quanto segue:

```js
const mongoDB =
  "mongodb+srv://your_user_name:your_password@cluster0.cojoign.mongodb.net/local_library?retryWrites=true&w=majority";
```

Sostituisci la riga con il seguente codice che utilizza `process.env.MONGODB_URI` per ottenere la stringa di connessione da una variabile d'ambiente chiamata `MONGODB_URI` se è stata impostata (usa il tuo URL del database al posto del segnaposto di seguito).

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
> Un altro modo comune per tenere separate le credenziali del database di produzione dal codice sorgente è leggerle da un file `.env` che viene distribuito separatamente al file system (ad esempio, potrebbero essere letti utilizzando il modulo npm [dotenv](https://www.npmjs.com/package/dotenv)).

### Impostare NODE_ENV su 'production'

Possiamo rimuovere le trace dello stack nelle pagine di errore impostando la variabile d'ambiente `NODE_ENV` su _production_ (è impostata su '_development_' per impostazione predefinita). Oltre a generare messaggi di errore meno verbosi, impostare la variabile su _production_ memorizza nella cache i modelli di visualizzazione e i file CSS generati dalle estensioni CSS. I test indicano che impostare `NODE_ENV` su _production_ può migliorare le prestazioni dell'app di un fattore di tre!

Questo cambiamento può essere effettuato utilizzando `export`, un file di ambiente o il sistema di inizializzazione del sistema operativo.

> [!NOTE]
> Questo è effettivamente un cambiamento che effettui nell'impostazione dell'ambiente piuttosto che nella tua app, ma abbastanza importante da essere notato qui! Mostreremo come questo è impostato per il nostro esempio di hosting di seguito.

### Registrare i log in modo appropriato

Le chiamate di registrazione dei log possono avere un impatto su un sito ad alto traffico. In un ambiente di produzione, potrebbe essere necessario registrare l'attività del sito web (ad esempio, tracciare il traffico o registrare le chiamate API) ma bisognerebbe cercare di minimizzare la quantità di registrazione aggiunta per scopi di debug.

Un modo per minimizzare il logging di "debug" in produzione è utilizzare un modulo come [debug](https://www.npmjs.com/package/debug) che consente di controllare quale logging viene eseguito impostando una variabile d'ambiente.
Ad esempio, il frammento di codice seguente mostra come potrebbe essere impostata la registrazione dell’"autore".
La variabile di debug viene dichiarata con il nome 'author', e il prefisso "author" verrà automaticamente visualizzato per tutti i log da questo oggetto.

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

Puoi quindi abilitare un particolare set di log specificandoli come elenco separato da virgole nella variabile d'ambiente `DEBUG`.
Puoi impostare le variabili per visualizzare i log di autori e libri come mostrato (sono supportati anche i caratteri jolly).

```bash
#Windows
set DEBUG=author,book

#Linux
export DEBUG="author,book"
```

> [!NOTE]
> Le chiamate a `debug` possono sostituire i log che potresti aver precedentemente eseguito utilizzando `console.log()` o `console.error()`. Sostituisci qualsiasi chiamata `console.log()` nel tuo codice con la registrazione tramite il modulo [debug](https://www.npmjs.com/package/debug). Attiva e disattiva la registrazione nel tuo ambiente di sviluppo impostando la variabile DEBUG e osserva l'impatto che ha sulla registrazione.

Se hai bisogno di registrare l'attività del sito web puoi utilizzare una libreria di logging come _Winston_ o _Bunyan_. Per maggiori informazioni su questo argomento vedi: [Production best practices: performance and reliability](https://expressjs.com/en/advanced/best-practice-performance.html).

### Usa la compressione gzip/deflate per le risposte

I server web possono spesso comprimere la risposta HTTP inviata a un client, riducendo significativamente il tempo necessario affinché il client ottenga e carichi la pagina. Il metodo di compressione utilizzato dipenderà dai metodi di decompressione che il client dice di supportare nella richiesta (la risposta verrà inviata non compressa se non ci sono metodi di compressione supportati).

Aggiungi questo al tuo sito utilizzando il middleware [compression](https://www.npmjs.com/package/compression). Installalo alla radice del tuo progetto eseguendo il seguente comando:

```bash
npm install compression
```

Apri **./app.js** e richiedi la libreria di compressione come mostrato. Aggiungi la libreria di compressione alla catena di middleware con il metodo `use()` (questo dovrebbe apparire prima di qualsiasi percorso che vuoi comprimere — in questo caso, tutti!)

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
> Per un sito web ad alto traffico in produzione non utilizzeresti questo middleware. Invece, utilizzeresti un reverse proxy come [Nginx](https://nginx.org/).

### Usa Helmet per proteggerti da vulnerabilità note

[Helmet](https://www.npmjs.com/package/helmet) è un pacchetto middleware. Può impostare header HTTP appropriati che aiutano a proteggere la tua app da vulnerabilità web note (vedi i [documenti](https://helmetjs.github.io/) per ulteriori informazioni su quali header imposta e contro quali vulnerabilità protegge).

Installalo alla radice del tuo progetto eseguendo il seguente comando:

```bash
npm install helmet
```

Apri **./app.js** e richiedi la libreria _helmet_ come mostrato.
Quindi aggiungi il modulo alla catena di middleware con il metodo `use()`.

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

Normalmente potremmo inserire solo `app.use(helmet());` per aggiungere il _subset_ di header correlati alla sicurezza che ha senso per la maggior parte dei siti.
Tuttavia nel [modello base di LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/LocalLibrary_base_template) includiamo alcuni script di bootstrap e jQuery.
Questi violano la _Content Security Policy (CSP)_ _predefinita_ di helmet che non consente il caricamento di script cross-site.
Per consentire il caricamento di questi script, modifichiamo la configurazione di helmet in modo che imposti le direttive CSP per consentire il caricamento di script dai domini indicati.
Per il tuo server puoi aggiungere/disattivare specifici header come necessario seguendo le [istruzioni per l'uso di helmet qui](https://www.npmjs.com/package/helmet).

### Aggiungi un limite di velocità alle rotte API

[Express-rate-limit](https://www.npmjs.com/package/express-rate-limit) è un pacchetto middleware che può essere utilizzato per limitare le richieste ripetute alle API e ai percorsi.
Ci sono molte ragioni per cui potrebbero essere effettuate richieste eccessive al tuo sito, come gli attacchi denial of service, gli attacchi a forza bruta, o anche solo un client o uno script che non si comporta come ci si aspetta.
Oltre ai problemi di prestazioni che possono derivare da troppe richieste che rallentano il server, potresti anche essere addebitato per il traffico aggiuntivo.
Questo pacchetto può essere utilizzato per limitare il numero di richieste che possono essere effettuate a un determinato percorso o insieme di percorsi.

Installalo alla radice del tuo progetto eseguendo il seguente comando:

```bash
npm install express-rate-limit
```

Apri **./app.js** e richiedi la libreria _express-rate-limit_ come mostrato.
Quindi aggiungi il modulo alla catena di middleware con il metodo `use()`.

```js
const compression = require("compression");
const helmet = require("helmet");
const RateLimit = require("express-rate-limit");

const app = express();

// Set up rate limiter: maximum of twenty requests per minute
const limiter = RateLimit({
  windowMs: 1 * 60 * 1000, // 1 minute
  max: 20,
});
// Apply rate limiter to all requests
app.use(limiter);

// …
```

Il comando sopra limita tutte le richieste a 20 al minuto (puoi cambiare questo valore come necessario).

> [!NOTE]
> Servizi di terze parti come [Cloudflare](https://www.cloudflare.com/) possono anche essere utilizzati se hai bisogno di più protezione avanzata contro il denial of service o altri tipi di attacchi.

#### Imposta la versione di Node

Per applicazioni Node, comprese quelle Express, il file **package.json** contiene tutto ciò che un fornitore di hosting dovrebbe aver bisogno per determinare le dipendenze dell'applicazione e il file punto di ingresso.

L'unica informazione importante mancante dal nostro attuale **package.json** è la versione di node richiesta dalla libreria.
Puoi trovare la versione di node utilizzata per lo sviluppo inserendo il comando:

```bash
>node --version
v16.17.1
```

Apri **package.json**, e aggiungi questa informazione come **engines > node** come mostrato (utilizzando il numero di versione del tuo sistema).

```json
  "engines": {
    "node": ">=16.17.1"
  },
```

Il servizio di hosting potrebbe non supportare la versione specifica di node indicata, ma questo cambiamento dovrebbe garantire che tenti di utilizzare una versione con lo stesso numero di versione principale, o una versione più recente.

Nota che potrebbero esserci altri modi per specificare la versione di node su diversi servizi di hosting, ma l'approccio **package.json** è ampiamente supportato.

#### Ottieni dipendenze e riprova

Prima di procedere, proviamo nuovamente il sito e assicuriamoci che non sia stato influenzato da nessuno dei nostri cambiamenti.

Per prima cosa, dovremo recuperare le nostre dipendenze. Puoi fare questo eseguendo il seguente comando nel tuo terminale alla radice del progetto:

```bash
npm install
```

Ora esegui il sito (vedi [Testing the routes](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/routes#testing_the_routes) per i comandi rilevanti) e verifica che il sito si comporta ancora come ti aspetti.

### Creare un repository di applicazioni su GitHub

Molti servizi di hosting consentono di importare e/o sincronizzare i progetti da un repository locale o da piattaforme di controllo del versione del software basate su cloud.
Questo può rendere la distribuzione e lo sviluppo iterativo molto più semplice.

Per questo tutorial, configureremo un account [GitHub](https://github.com/) e un repository per la biblioteca, e utilizzeremo lo strumento **git** per caricare il nostro codice sorgente.

> [!NOTE]
> Puoi saltare questo passaggio se stai già usando GitHub per gestire il tuo codice sorgente!
>
> Nota che utilizzare strumenti di gestione del codice sorgente è una buona pratica nello sviluppo software, poiché consente di provare le modifiche e passare dai propri esperimenti al "codice funzionale noto" quando è necessario!

I passaggi sono:

1. Visita <https://github.com/> e crea un account.
2. Una volta effettuato l'accesso, fai clic sul link **+** nella barra degli strumenti superiore e seleziona **New repository**.
3. Compila tutti i campi su questo modulo. Sebbene non siano obbligatori, sono fortemente consigliati.

   - Inserisci un nuovo nome di repository (ad esempio, _express-locallibrary-tutorial_), e una descrizione (ad esempio, "Sito web Local Library scritto in Express (Node)").
   - Scegli **Node** nella lista di selezione _Add .gitignore_.
   - Scegli la tua licenza preferita nella lista di selezione _Add license_.
   - Seleziona **Initialize this repository with a README**.

   > [!WARNING]
   > L'accesso "Public" predefinito renderà _tutto_ il codice sorgente — inclusi il nome utente e la password del tuo database — visibile a chiunque su Internet! Assicurati che il codice sorgente legga le credenziali _solo_ dalle variabili d'ambiente e non abbia credenziali codificate in modo rigido.
   >
   > Altrimenti, seleziona l'opzione "Private" per consentire solo a persone selezionate di vedere il codice sorgente.

4. Premi **Create repository**.
5. Fai clic sul pulsante verde **Clone or download** sulla tua nuova pagina di repo.
6. Copia il valore dell'URL dal campo di testo all'interno della finestra di dialogo che appare.
   Se hai utilizzato il nome di repository "express-locallibrary-tutorial", l'URL dovrebbe essere qualcosa del tipo: `https://github.com/<your_git_user_id>/express-locallibrary-tutorial.git`.

Ora che il repository ("repo") è creato su GitHub vogliamo clonarlo (copiarlo) sul nostro computer locale:

1. Installa _git_ per il tuo computer locale ([guida ufficiale al download di Git](https://git-scm.com/downloads)).
2. Apri un prompt dei comandi/terminale e clona il tuo repo utilizzando l'URL che hai copiato sopra:

   ```bash
   git clone https://github.com/<your_git_user_id>/express-locallibrary-tutorial.git
   ```

   Questo creerà il repository all'interno della directory corrente.

3. Naviga nella cartella del repo.

   ```bash
   cd express-locallibrary-tutorial
   ```

Quindi copia i tuoi file sorgente dell'applicazione nella cartella del repo, rendili parte del repo usando _git_ e caricali su GitHub:

1. Copia la tua applicazione Express in questa cartella (escludendo **/node_modules**, che contiene file di dipendenza che dovresti recuperare da npm secondo necessità).
2. Apri un prompt dei comandi/terminale e utilizza il comando `add` per aggiungere tutti i file a git.

   ```bash
   git add -A
   ```

3. Usa il comando `status` per controllare che tutti i file che stai per `commit` siano corretti (vuoi includere i file sorgente, non i file binari, temporanei, ecc.).
   Dovrebbe sembrare un po' come l'elenco qui sotto.

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

4. Quando sei soddisfatto, `commit` i file al tuo repo locale.
   Questo è equivalente a firmare le modifiche e renderle una parte ufficiale del repo locale.

   ```bash
   git commit -m "First version of application moved into GitHub"
   ```

5. A questo punto, il repo remoto non è stato cambiato.
   L'ultimo passaggio è sincronizzare (`push`) il tuo repo locale su GitHub utilizzando il seguente comando:

   ```bash
   git push origin main
   ```

Quando questa operazione è completata, dovresti essere in grado di tornare alla pagina su GitHub dove hai creato il tuo repo, aggiornare la pagina e vedere che la tua intera applicazione è stata ora caricata. Puoi continuare ad aggiornare il tuo repo man mano che i file cambiano utilizzando questo ciclo add/commit/push.

Questo è un buon momento per fare un backup del tuo progetto "vanilla" — mentre alcune delle modifiche che stiamo per fare nelle sezioni seguenti potrebbero essere utili per la distribuzione su qualsiasi servizio di hosting (o per lo sviluppo) altre potrebbero non esserlo.
Puoi fare questo usando `git` dalla riga di comando:

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
> Git è incredibilmente potente!
> Per saperne di più, guarda [Learning Git](https://docs.github.com/en/get-started/start-your-journey/git-and-github-learning-resources).

## Esempio: Hosting su Glitch

Questa sezione fornisce una dimostrazione pratica di come ospitare _LocalLibrary_ su [Glitch](https://glitch.com/).

### Perché Glitch?

Scegliamo di utilizzare Glitch per diversi motivi:

- Glitch ha un [piano iniziale gratuito](https://glitch.com/pricing) che è _davvero_ gratuito, anche se con alcune limitazioni.
  Il fatto che sia accessibile a tutti gli sviluppatori è molto importante per MDN!
- Glitch si occupa dell'infrastruttura in modo che tu non debba farlo.
  Non dover preoccuparsi dei server, dei bilanciatori di carico, dei reverse proxy, e così via, rende molto più facile iniziare.
- Le abilità e i concetti che imparerai utilizzando Glitch sono trasferibili.
- Le limitazioni del servizio e del piano non ci influenzano realmente nell'utilizzo di Glitch per il tutorial.
  Ad esempio:

  - Il piano iniziale offre solo 1000 "ore di progetto" al mese, che vengono resettate mensilmente.
    Questo viene utilizzato quando stai attivamente modificando il sito o se qualcuno lo sta accedendo.
    Se nessuno sta accedendo o modificando il sito, andrà in sospensione.
  - L'ambiente del piano iniziale ha una quantità limitata di RAM del contenitore e spazio di archiviazione.
    C'è più che a sufficienza per il tutorial, in particolare perché il nostro database è ospitato altrove.
  - I domini personalizzati non sono ben supportati (al momento della scrittura).
  - Altre limitazioni possono essere trovate nella [pagina delle restrizioni tecniche di Glitch](https://help.glitch.com/s/article/Technical-Restrictions).

Anche se Glitch è appropriato per ospitare questa dimostrazione, dovresti prendere il tempo per determinare se è [adatto per il tuo sito web](#scegliere_un_fornitore_di_hosting).

### Come funziona Glitch?

Glitch fornisce un'interfaccia basata sul web in cui è possibile creare progetti da modelli di partenza, o importarli da GitHub, e quindi aggiungere e modificare i file del progetto.
Man mano che fai cambiamenti, il progetto viene costruito ed eseguito nel proprio contenitore virtualizzato isolato e indipendente.

Come funziona tutto "sotto il cofano" è un mistero — Glitch non lo dice.
Quello che è chiaro è che purché crei un'applicazione web nodejs abbastanza standard (ad esempio, usando `package.json` per le tue dipendenze), e non consumi più risorse di quelle elencate nelle [restrizioni tecniche](https://help.glitch.com/s/article/Technical-Restrictions), la tua applicazione dovrebbe "funzionare e basta".

Una volta che l'applicazione è in esecuzione, può essere configurata per la produzione utilizzando [dati privati](https://help.glitch.com/s/article/Adding-Private-Data) forniti in un file `.env`.
I valori nei dati segreti vengono letti dall'applicazione come variabili d'ambiente, che, come ricorderai da una sezione precedente, è come abbiamo configurato la nostra applicazione per ottenere il suo URL del database.
Nota che le variabili sono _segrete_: il `.env` non dovrebbe essere incluso nel tuo repository GitHub.

La vista di modifica di Glitch fornisce anche accesso al _terminale_ all'ambiente dell'app web, che puoi utilizzare per lavorare con l'app web come se stesse girando sulla tua macchina locale.

Questo è tutto ciò che ti serve per iniziare.
Successivamente, configureremo un account Glitch, caricheremo il progetto della Biblioteca da GitHub e lo collegheremo a un database.

### Ottieni un account Glitch

Per iniziare a utilizzare Glitch dovrai prima creare un account:

- Vai a [glitch.com](https://glitch.com/) e fai clic sul pulsante **Sign up** nella barra degli strumenti superiore.
- Seleziona GitHub nel pop-up per registrarti utilizzando le tue credenziali GitHub.
- Verrai quindi connesso alla dashboard di Glitch: <https://glitch.com/dashboard>.

### Risoluzione dei problemi della versione di Node.js

I fornitori di hosting supportano comunemente una versione principale di rilasci recenti di Node.js.
Se la versione "minore" esatta che hai specificato nel tuo file `package.json` non è supportata, solitamente torneranno alla versione più vicina che supportano (e spesso funzionerà).

Sfortunatamente, al momento della scrittura, la versione più alta supportata su Glitch è Node.js 16.
Se hai sviluppato con Node.js 17 o successivo, dovresti ridurre la versione utilizzata nel tuo file `package.json` come mostrato.
Dovrai anche ritestare:

```json
  "engines": {
    "node": ">=v16"
  },
```

Glitch [pianifica di aggiornare node e tenerlo meglio aggiornato in futuro](https://blog.glitch.com/post/rebuilding-glitch/) — e potrebbe essere che quando leggerai questo la limitazione della versione non esista più.
Invece di eseguire il downgrade della versione di `node`, potresti caricare il tuo progetto per vedere se si compila.
Se ci sono errori e la tua applicazione non si carica, dovresti provare a impostare la versione di `node` su `>=v16` nel tuo `package.json` nell'editor di Glitch.

> [!NOTE]
> Puoi anche controllare le versioni supportate inserendo il seguente comando nel terminale di qualsiasi progetto Glitch:
>
> ```sh
> ls -l /opt/nvm/versions/node | grep '^d' | awk '{ print $9 }'
> ```

### Distribuzione su Glitch da GitHub

Ora importeremo il progetto della Biblioteca da GitHub.
Per prima cosa, scegli l'opzione **Dashboard** dal menu in alto del sito, poi seleziona il pulsante **New project**.
Glitch visualizzerà un elenco di opzioni per il nuovo progetto; seleziona **Import from GitHub**.

![Dashboard del sito web di Glitch che mostra il pulsante per un nuovo progetto e un menu popup con l'opzione "Import from GitHub"](glitch_new_project_import_github.png)

Apparirà un pop-up.
Inserisci l'URL del tuo repository GitHub della libreria nel pop-up e premi **OK**.
Di seguito, abbiamo inserito il repo per il progetto funzionante.

![Pop-up di Glitch per l'inserimento dell'URL del repository GitHub da importare](glitch_new_project_github_repo_url.png)

Glitch importerà quindi il progetto, visualizzando le notifiche di avanzamento.
Al termine, mostrerà la vista di modifica per il nuovo progetto, come mostrato di seguito.

![Vista editor di Glitch per il progetto importato](glitch_imported_project_in_editor.png)

Puoi ottnere l'URL del sito attivo selezionando il pulsante **Share**.

![Vista editor di Glitch per il progetto importato](glitch_share_project.png)

Apri una nuova scheda del browser e copia il link per il sito attivo nella barra degli indirizzi.
Il sito della biblioteca locale dovrebbe aprirsi e visualizzare i dati dal database di sviluppo.

> [!NOTE]
> Questo processo è stato un'importazione una tantum da GitHub.
> Puoi anche utilizzare le azioni GitHub, come [glitch-project-sync](https://github.com/marketplace/actions/glitch-project-sync), per tenere sincronizzati Glitch e il tuo progetto.

### Usa un database MongoDB di produzione

Dovresti configurare un database diverso per la produzione rispetto a quello di sviluppo.
Mentre Glitch ospita solo database SQLite (e siamo configurati per utilizzare MongoDB), molti altri siti offrono database MongoDB come servizio.

Una opzione è seguire le istruzioni [Configurazione del database MongoDB](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose#setting_up_the_mongodb_database) di una sezione precedente del tutorial per configurare un nuovo database di produzione.

Per rendere il database di produzione accessibile all'applicazione della biblioteca, apri il file `.env` nella vista editor del progetto.
Inserisci la variabile `MONGODB_URI` con l'URL del tuo database di produzione.
Il sito si aggiorna man mano che inserisci i valori nell'editor.

![Editor del file .env di Glitch per i dati privati con variabili di produzione](glitch_env.png)

> [!NOTE]
> Non abbiamo creato questo file.
> È destinato ai [dati privati](https://help.glitch.com/s/article/Adding-Private-Data) ed è stato creato automaticamente durante l'importazione su Glitch.
> Non viene mai esportato o copiato.

### Altre variabili di configurazione

Ricorderai da una sezione precedente che dobbiamo [impostare NODE_ENV su 'production'](#set_node_env_to_production) per migliorare le prestazioni e generare messaggi di errore meno verbosi. Facciamo questo nello stesso file in cui impostiamo la variabile `MONGODB_URI`.

Apri `.env` e aggiungi una variabile `NODE_ENV` con valore `production` (vedi lo screenshot nella sezione precedente).

L'applicazione della biblioteca locale è ora impostata e configurata per l'uso in produzione.
Puoi aggiungere dati tramite l'interfaccia del sito web e dovrebbe funzionare come faceva durante lo sviluppo (anche se con meno informazioni di debug esposte per le pagine non valide).

> [!NOTE]
> Se vuoi solo aggiungere alcuni dati per il testing, potresti usare lo script `populatedb` (con il tuo URL del database di produzione MongoDB) come discusso nella sezione [Express Tutorial Parte 3: Uso di un Database (con Mongoose) Testing — creare alcuni elementi](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose#testing_%E2%80%94_create_some_items).

### Debugging delle app Express su Glitch

Glitch consente un debug efficace.
Alcune delle cose che puoi fare sono:

- Selezionare il pulsante dei log nella parte inferiore della vista editor per vedere le informazioni del log dal tuo server, come l'output del log console.
- Selezionare il pulsante del terminale nella parte inferiore della vista editor per aprire un terminale nell'ambiente di hosting.
  Puoi usarlo per eseguire comandi e strumenti nell'ambiente.
  Ad esempio, potresti usare `node -v` per controllare la versione di node.
- Debugging interattivo in VS Code utilizzando l'estensione GLITCH per VS Code.

## Esempio: Hosting su Railway

Questa sezione fornisce una dimostrazione pratica di come installare _LocalLibrary_ su [Railway](https://railway.com/).

### Perché Railway?

> [!WARNING]
> Railway non ha più un livello di avviamento completamente gratuito.
> Abbiamo mantenuto queste istruzioni perché Railway ha alcune ottime funzionalità e sarà un'opzione migliore per alcuni utenti.

Railway è un'opzione di hosting attraente per diversi motivi:

- Railway si occupa della maggior parte dell'infrastruttura in modo che tu non debba farlo.
  Non dover preoccuparsi dei server, dei bilanciatori di carico, dei reverse proxy, e così via, rende molto più facile iniziare.
- Railway ha un [focus sull'esperienza dello sviluppatore per lo sviluppo e la distribuzione](https://docs.railway.com/maturity/compare-to-heroku), che porta a una curva di apprendimento più veloce e più soft rispetto a molte altre alternative.
- Le abilità e i concetti che imparerai utilizzando Railway sono trasferibili.
  Anche se Railway ha alcune nuove funzionalità eccellenti, molti altri servizi di hosting popolari utilizzano molte delle stesse idee e approcci.
- [La documentazione Railway](https://docs.railway.com/) è chiara e completa.
- Il servizio sembra essere molto affidabile e se finisci per amarlo, i prezzi sono prevedibili, e scalare la tua app è molto facile.

Dovresti prendere il tempo per determinare se Railway è [adatto per il tuo proprio sito web](#scegliere_un_fornitore_di_hosting).

### Come funziona Railway?

Le applicazioni web vengono eseguite ciascuna nel proprio contenitore virtualizzato isolato e indipendente.
Per eseguire la tua applicazione, Railway deve essere in grado di configurare l'ambiente appropriato e le dipendenze, e anche capire come viene lanciata.

Railway rende questo facile, poiché può riconoscere automaticamente e installare molti diversi framework e ambienti di applicazioni web basati sul loro utilizzo delle "convenzioni comuni".
Ad esempio, Railway riconosce le applicazioni node perché hanno un file **package.json**, e può determinare il gestore di pacchetti utilizzato per la compilazione dal file "lock".
Ad esempio, se l'applicazione include il file **package-lock.json** Railway sa di usare _npm_ per installare i pacchetti, mentre se trova **yarn.lock** sa di usare _yarn_.
Avendo installato tutte le dipendenze, Railway cercherà script denominati "build" e "start" nel file di pacchetto, e utilizzerà questi per costruire ed eseguire il codice.

> [!NOTE]
> Railway usa [Nixpacks](https://nixpacks.com/docs) per riconoscere vari framework di applicazioni web scritti in diversi linguaggi di programmazione.
> Non hai bisogno di sapere altro per questo tutorial, ma puoi trovare più opzioni per distribuire applicazioni node in [Nixpacks Node](https://nixpacks.com/docs/providers/node).

Una volta che l'applicazione è in esecuzione, può configurarsi utilizzando informazioni fornite nelle [variabili d'ambiente](https://docs.railway.com/guides/variables).
Ad esempio, un'applicazione che utilizza un database deve ottenere l'indirizzo utilizzando una variabile.
Il servizio del database stesso può essere ospitato da Railway o da un altro fornitore.

Gli sviluppatori interagiscono con Railway attraverso il sito Railway e utilizzando uno speciale strumento [Command Line Interface (CLI)](https://docs.railway.com/guides/cli).
Il CLI consente di associare un repository GitHub locale a un progetto railway, caricare il repository dal ramo locale al sito attivo, ispezionare i log del processo in esecuzione, impostare e ottenere variabili di configurazione e molto altro.
Una delle funzionalità più utili è che puoi usare il CLI per eseguire il tuo progetto locale con le stesse variabili d'ambiente del progetto attivo.

Questo è tutto l'overview di cui hai bisogno per distribuire l'app su Railway.
Ora imposteremo un account Railway, installeremo il nostro sito web e un database, e proveremo il client Railway.

### Ottieni un account Railway

Per iniziare a usare Railway dovrai prima creare un account:

- Vai su [railway.com](https://railway.com/) e fai clic sul link **Login** nella barra degli strumenti in alto.
- Seleziona GitHub nel pop-up per accedere utilizzando le tue credenziali GitHub
- Potresti dover andare alla tua email e verificare il tuo account.
- Sarai quindi loggato nella dashboard di Railway.com: <https://railway.com/dashboard>.

### Distribuisci su Railway da GitHub

Successivamente configureremo Railway per distribuire la nostra biblioteca da GitHub.
Per prima cosa scegli l'opzione **Dashboard** dal menu in alto del sito, poi seleziona il pulsante **New Project**:

![Dashboard del sito Railway che mostra il pulsante nuovo progetto](railway_new_project_button.png)

Railway visualizzerà un elenco di opzioni per il nuovo progetto, compresa l'opzione per distribuire un progetto da un modello che viene prima creato nel tuo account GitHub, e un numero di database.
Seleziona **Deploy from GitHub repo**.

![Popup di Railway che mostra le opzioni di distribuzione con l'opzione Deploy from GitHub repo evidenziata](railway_new_project_button_deploy_github_repo.png)

Tutti i progetti nei repository GitHub che hai condiviso con Railway durante l'installazione vengono visualizzati.
Seleziona il tuo repository GitHub per la biblioteca locale: `<user-name>/express-locallibrary-tutorial`.

![Popup di Railway che mostra i repository GitHub che possono essere distribuiti](railway_new_project_button_deploy_github_selectrepo.png)

Conferma la tua distribuzione selezionando **Deploy Now**.

![Schermata di conferma in cui puoi selezionare la distribuzione del progetto](railway_new_project_deploy_confirm.png)

Railway caricherà e distribuirà quindi il tuo project, visualizzando i progressi nella scheda distribuzioni.
Quando la distribuzione sarà completata con successo, vedrai una schermata come quella qui sotto.

![Dashboard Railway che mostra la scheda Deployments per il progetto distribuito](railway_project_deploy.png)

Ora seleziona la scheda _Settings_, quindi scorri verso il basso fino alla sezione Domains e premi il pulsante **Generate Domain**.

![Scheda delle impostazioni del progetto Railway che mostra il pulsante per generare un dominio](railway_project_generate_domain.png)

Questo pubblicherà il sito e metterà il dominio al posto del pulsante, come mostrato di seguito.

![Scheda delle impostazioni del progetto Railway che mostra un link al sito della biblioteca locale](railway_project_domain.png)

Seleziona l'URL del dominio per aprire la tua applicazione della biblioteca.
Nota che perché non abbiamo specificato un database di produzione, la biblioteca locale si aprirà utilizzando i tuoi dati di sviluppo.

### Fornire e connettere un database MongoDB

Invece di usare i nostri dati di sviluppo, ora creiamo un database MongoDB di produzione per usarli al posto.
Creeremo il database come parte del progetto dell'applicazione Railway, anche se nulla vieta di crearlo in un proprio progetto separato, o in effetti di usare un database _MongoDB Atlas_ per i dati di produzione, proprio come hai fatto per il database di sviluppo.

Su Railway, scegli l'opzione **Dashboard** dal menu in alto del sito e poi seleziona il tuo progetto di applicazione.
A questo punto contiene solo un singolo servizio per l'applicazione (questo può essere selezionato per impostare variabili e altri dettagli del servizio).
Seleziona il pulsante **New**, che viene utilizzato per aggiungere servizi al progetto attuale.

![Progetto Railway con evidenziato il pulsante nuovo servizio](railway_project_open_no_database.png)

Seleziona **Database** quando ti viene chiesto il tipo di servizio da aggiungere:

![Popup di Railway che mostra le opzioni per un nuovo servizio, come database, repository GitHub, servizio vuoto ecc.](railway_database_add.png)

Quindi seleziona **Add MongoDB** per iniziare ad aggiungere il database

![Popup di Railway che mostra i diversi database che possono essere selezionati: Postgres, MySQL, MongoDB e così via](railway_database_select_type.png)

Railway fornirà quindi un servizio contenente un database vuoto nello stesso progetto.
Al completamento vedrai ora sia i servizi dell'applicazione che del database nella vista del progetto.

![Progetto Railway con servizi di applicazione e database](railway_project_two_services.png)

Seleziona il servizio MongoDB per visualizzare le informazioni sul database.
Apri la scheda _Variables_ e copia il "Mongo_URL" (questo è l'indirizzo del database).

![Schermata delle impostazioni del database Railway che mostra l'URL necessario per connettersi al database](railway_mongodb_connect.png)

Per rendere questo accessibile all'applicazione della biblioteca dobbiamo aggiungerlo al processo dell'applicazione utilizzando una variabile d'ambiente.
Per prima cosa apri il servizio dell'applicazione.
Poi seleziona la scheda _Variables_ e premi il pulsante **New Variable**.

Inserisci il nome della variabile `MONGODB_URI` e l'URL di connessione che hai copiato per il database (`MONGODB_URI` è il nome della variabile d'ambiente da cui [abbiamo configurato l'applicazione](#configurazione_del_database) per leggere l'indirizzo del database).
Questo apparirà come la schermata mostrata sotto.

![Schermata delle variabili del sito Railway mentre si sta aggiungendo la variabile MONGODB_URI e l'indirizzo](railway_variables_database_url.png)

Seleziona **Add** per aggiungere la variabile.

Railway riavvia la tua app quando aggiorna le variabili. Se controlli la home page ora dovrebbe mostrare valori zero per i tuoi conteggi degli oggetti, poiché le modifiche sopra significano che ora stiamo usando un nuovo database (vuoto).

### Altre variabili di configurazione

Ricorderai da una sezione precedente che dobbiamo [impostare NODE_ENV su 'production'](#set_node_env_to_production) per migliorare le prestazioni e generare messaggi di errore meno verbosi. Possiamo farlo nella stessa schermata in cui impostiamo la variabile `MONGODB_URI`.

Apri il servizio dell'applicazione.
Poi seleziona la scheda _Variables_, dove vedrai che `MONGODB_URI` è già definito, e premi il pulsante **New Variable**.

![Scheda delle variabili Railway con il pulsante New Variable evidenziato](railway_variables_new.png)

Inserisci `NODE_ENV` come nome della nuova variabile e `production` come nome dell'ambiente.
Poi premi il pulsante **Add**.

![Scheda delle variabili Railway con la nuova variabile NODE_ENV impostata su 'production'](railway_variables_new_node_env.png)

L'applicazione della biblioteca locale è ora configurata e configurata per l'uso in produzione.
Puoi aggiungere dati tramite l'interfaccia del sito web e dovrebbe funzionare allo stesso modo in cui funziona durante lo sviluppo (anche se con meno informazioni di debug esposte per le pagine non valide).

> [!NOTE]
> Se vuoi solo aggiungere alcuni dati per il testing potresti usare lo script `populatedb` (con il tuo URL del database di produzione MongoDB) come discusso nella sezione [Express Tutorial Parte 3: Uso di un Database (con Mongoose) Testing — creare alcuni elementi](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose#testing_%E2%80%94_create_some_items).

### Installa il client

Scarica e installa il client Railway per il tuo sistema operativo locale seguendo le [istruzioni qui](https://docs.railway.com/guides/cli).

Dopo che il client è stato installato sarai in grado di eseguire i comandi.
Alcune delle operazioni più importanti includono la distribuzione della directory corrente del tuo computer in un progetto Railway associato (senza dover caricare su GitHub), e l'esecuzione del tuo progetto localmente utilizzando le stesse impostazioni che hai sul server di produzione.

Puoi ottenere un elenco di tutti i comandi possibili inserendo quanto segue in un terminale.

```bash
railway help
```

### Debugging

Il client Railway fornisce il comando dei log per mostrare la coda dei log (un log più completo è disponibile sul sito per ogni progetto):

```bash
railway logs
```

## Riepilogo

Questo è la fine di questo tutorial sulla configurazione delle app Express in produzione, e anche della serie di tutorial sul lavoro con Express. Speriamo che li trovi utili. Puoi controllare una versione completamente lavorata del [codice sorgente su GitHub qui](https://github.com/mdn/express-locallibrary-tutorial).

## Vedi anche

- [Production best practices: performance and reliability](https://expressjs.com/en/advanced/best-practice-performance.html) (documenti Express)
- [Production Best Practices: Security](https://expressjs.com/en/advanced/best-practice-security.html) (documenti Express)
- Documenti Railway

  - [CLI](https://docs.railway.com/guides/cli)

- DigitalOcean

  - tutorial [Express](https://www.digitalocean.com/community/tutorials?q=express)
  - tutorial [Node.js](https://www.digitalocean.com/community/tutorials?q=node.js)

- Heroku

  - [Introduzione a Heroku con Node.js](https://devcenter.heroku.com/articles/getting-started-with-nodejs) (documenti Heroku)
  - [Distribuire applicazioni Node.js su Heroku](https://devcenter.heroku.com/articles/deploying-nodejs) (documenti Heroku)
  - [Supporto Node.js di Heroku](https://devcenter.heroku.com/articles/nodejs-support) (documenti Heroku)
  - [Ottimizzare la concorrenza delle applicazioni Node.js](https://devcenter.heroku.com/articles/node-concurrency) (documenti Heroku)
  - [Come funziona Heroku](https://devcenter.heroku.com/articles/how-heroku-works) (documenti Heroku)
  - [Dynos e il Dyno Manager](https://devcenter.heroku.com/articles/dynos) (documenti Heroku)
  - [Configurazione e variabili di configurazione](https://devcenter.heroku.com/articles/config-vars) (documenti Heroku)
  - [Limiti](https://devcenter.heroku.com/articles/limits) (documenti Heroku)

{{PreviousMenu("Learn_web_development/Extensions/Server-side/Express_Nodejs/forms", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}
