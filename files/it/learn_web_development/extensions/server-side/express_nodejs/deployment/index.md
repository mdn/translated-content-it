---
title: "Parte 7 del Tutorial su Express: Deploy in produzione"
short-title: "7: Deploy"
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/deployment
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenu("Learn_web_development/Extensions/Server-side/Express_Nodejs/forms", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}

Ora che ha creato (e testato) un sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website) straordinario, vorrà installarlo su un server web pubblico in modo che possa essere accessibile dal personale della biblioteca e dai membri via Internet. Questo articolo fornisce una panoramica su come potrebbe procedere per trovare un host per il suo sito web e cosa è necessario fare per preparare il suo sito per la produzione.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Completi tutti i precedenti argomenti del tutorial, inclusi <a href="/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms">Parte 6 del Tutorial su Express: Lavorare con i moduli</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obbiettivo:</th>
      <td>
        Imparare dove e come può distribuire un'app Express per la produzione.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

Una volta che il suo sito è finito (o abbastanza finito da iniziare i test pubblici), dovrà ospitarlo in un luogo più pubblico e accessibile rispetto al suo computer di sviluppo personale.

Fino a ora, ha lavorato in un [ambiente di sviluppo](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment), utilizzando Express/Node come server web per condividere il suo sito al browser/rete locale e eseguendo il suo sito con impostazioni di sviluppo (insicure) che espongono informazioni di debug e altre informazioni private. Prima di poter ospitare un sito web esternamente dovrà prima:

- Scegliere un ambiente per l'hosting dell'app Express.
- Apportare alcune modifiche alle impostazioni del suo progetto.
- Configurare un'infrastruttura a livello di produzione per servire il suo sito web.

Questo tutorial fornisce alcune indicazioni sulle opzioni per scegliere un sito di hosting, una breve panoramica su ciò che deve fare per preparare il suo sito Express per la produzione, e un esempio pratico di come installare il sito web LocalLibrary sul servizio di cloud hosting [Railway](https://railway.com/).

## Cos'è un ambiente di produzione?

L'ambiente di produzione è l'ambiente fornito dal server computer dove eseguirà il suo sito web per il consumo esterno. L'ambiente include:

- Hardware del computer su cui viene eseguito il sito web.
- Sistema operativo (ad esempio, Linux o Windows).
- Runtime del linguaggio di programmazione e librerie framework su cui è scritto il suo sito web.
- Infrastruttura del server web, che potrebbe includere un server web, un reverse proxy, un load balancer, ecc.
- Database da cui dipende il suo sito web.

Il computer server potrebbe essere situato nei suoi locali e collegato a Internet tramite un collegamento veloce, ma è molto più comune usare un computer ospitato "nel cloud". Ciò significa che il suo codice viene eseguito su un qualche computer remoto (o possibilmente un computer "virtuale") nel data center della sua società di hosting. Il server remoto di solito offre un certo livello garantito di risorse informatiche (ad esempio, CPU, RAM, memoria di archiviazione, ecc.) e connettività Internet per un certo prezzo.

Questo tipo di hardware di calcolo/rete accessibile da remoto viene indicato come _Infrastruttura come Servizio (IaaS)_. Molti fornitori IaaS offrono opzioni per pre-installare un particolare sistema operativo, su cui deve installare gli altri componenti del suo ambiente di produzione. Altri fornitori le permettono di selezionare ambienti più ricchi di funzionalità, forse incluso un setup completo di Node.

> [!NOTE]
> Gli ambienti pre-costruiti possono rendere molto semplice l'installazione del suo sito web perché riducono la configurazione, ma le opzioni disponibili potrebbero limitarla a un server sconosciuto (o ad altri componenti) e potrebbero basarsi su una versione più vecchia del sistema operativo. Spesso è meglio installare i componenti da soli in modo da ottenere quelli che desidera e quando ha bisogno di aggiornare parti del sistema, ha un'idea di dove iniziare!

Altri fornitori di hosting supportano Express come parte di un'offerta di _Piattaforma come Servizio_ (_PaaS_). Quando utilizza questo tipo di hosting non ha bisogno di preoccuparsi della maggior parte del suo ambiente di produzione (server, load balancers, ecc.) poiché la piattaforma host si occupa di queste cose per lei. Ciò rende il deployment piuttosto facile perché deve concentrarsi solo sulla sua applicazione web e non su qualsiasi altra infrastruttura server.

Alcuni sviluppatori sceglieranno la maggiore flessibilità offerta da IaaS rispetto a PaaS, mentre altri apprezzeranno la minore manutenzione e la scalabilità più facile di PaaS. Quando inizia, configurare il suo sito su un sistema PaaS è molto più facile, quindi è ciò che faremo in questo tutorial.

> [!NOTE]
> Se sceglie un fornitore di hosting compatibile con Node/Express, dovrebbe fornire istruzioni su come configurare un sito web Express utilizzando diverse configurazioni di server web, server applicazioni, reverse proxy, ecc. Ad esempio, ci sono molte guide passo-passo per varie configurazioni nei [documenti comunitari di DigitalOcean Node](https://www.digitalocean.com/community/tutorials?q=node).

## Scegliere un fornitore di hosting

Ci sono numerosi fornitori di hosting noti per supportare attivamente o funzionare bene con _Node_ (ed _Express_). Questi fornitori offrono diversi tipi di ambienti (IaaS, PaaS) e diversi livelli di risorse informatiche e di rete a prezzi diversi.

> [!NOTE]
> Ci sono molte soluzioni di hosting, e i loro servizi e prezzi possono cambiare nel tempo. Mentre introduciamo alcune opzioni di seguito, vale la pena controllare sia queste che altre opzioni prima di selezionare un fornitore di hosting.

Alcune delle cose da considerare quando sceglie un host:

- Quanto sarà probabilmente occupato il suo sito e il costo dei dati e delle risorse informatiche necessarie per soddisfare tale domanda.
- Livello di supporto per la scalabilità orizzontale (aggiungere più macchine) e verticale (aggiornamenti a macchine più potenti) e i costi per farlo.
- Le posizioni in cui il fornitore ha data center, e quindi dove l'accesso è probabilmente più veloce.
- Storico di uptime e downtime del fornitore.
- Strumenti forniti per gestire il sito — sono facili da usare e sicuri (ad esempio, SFTP vs. FTP).
- Framework integrati per monitorare il suo server.
- Limitazioni note. Alcuni host bloccheranno deliberatamente determinati servizi (ad esempio, e-mail). Altri offrono solo un certo numero di ore di "live time" in alcuni livelli di prezzo, o offrono solo una piccola quantità di spazio di archiviazione.
- Benefici aggiuntivi. Alcuni fornitori offriranno nomi di dominio gratuiti e supporto per i certificati TLS che altrimenti dovrebbe pagare.
- Se il livello "gratuito" su cui si affida scade nel tempo e se il costo della migrazione a un livello più costoso significa che sarebbe stato meglio utilizzare qualche altro servizio fin dall'inizio!

La buona notizia, quando inizia, è che ci sono diversi siti che forniscono ambienti di calcolo "gratuiti" destinati alla valutazione e al test.
Questi sono di solito ambienti con limitate/constrained risorse, e deve essere consapevole che potrebbero scadere dopo un periodo introduttivo o avere altre limitazioni.
Tuttavia, sono ottimi per il test di siti a basso traffico in un ambiente ospitato e possono offrire una facile migrazione al pagamento per più risorse quando il suo sito diventa più trafficato.
Scelte popolari in questa categoria includono [Glitch](https://glitch.com/), [Python Anywhere](https://www.pythonanywhere.com/), [Amazon Web Services](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/billing-free-tier.html), [Microsoft Azure](https://azure.microsoft.com/en-us/pricing/details/app-service/linux/), ecc.

La maggior parte dei fornitori offre anche un livello "base" o "hobby" destinato a piccoli siti di produzione, e che forniscono livelli più utili di potenza computazionale e meno limitazioni.
[Railway](https://railway.com/), [Heroku](https://www.heroku.com/), [DigitalOcean](https://www.digitalocean.com/) e [Python Anywhere](https://www.pythonanywhere.com/) sono esempi di fornitori di hosting popolari che hanno un livello di computazione di base relativamente economico (nell'intervallo di 5-10 USD al mese).

> [!NOTE]
> Si ricordi che il prezzo non è l'unico criterio di selezione.
> Se il suo sito web ha successo, potrebbe risultare che la scalabilità sia la considerazione più importante.

## Preparare il suo sito web per la pubblicazione

Le principali cose a cui pensare quando pubblica il suo sito web sono la sicurezza web e le prestazioni.
Al minimo, vorrà modificare la configurazione del database in modo da poter utilizzare un database differente per la produzione e assicurare le sue credenziali, rimuovere gli stack traces che sono inclusi nelle pagine di errore durante lo sviluppo, sistemare il suo logging e impostare le intestazioni appropriate per evitare molte minacce di sicurezza comuni.

Nelle sottosezioni seguenti, delineiamo i cambiamenti più importanti che dovrebbe apportare alla sua app.

> [!NOTE]
> Ci sono altri suggerimenti utili nei documenti Express — veda [Migliori pratiche di produzione: prestazioni e affidabilità](https://expressjs.com/en/advanced/best-practice-performance.html) e [Migliori pratiche di produzione: Sicurezza](https://expressjs.com/en/advanced/best-practice-security.html).

### Configurazione del database

Finora in questo tutorial abbiamo utilizzato un singolo database di sviluppo, per il quale l'indirizzo e le credenziali sono hard-coded in **app.js**.
Poiché il database di sviluppo non contiene informazioni che ci preoccupiamo di esporre o corrompere, non c'è nessun rischio particolare nel divulgare questi dettagli.
Tuttavia, se sta lavorando con dati reali, in particolare informazioni personali degli utenti, proteggere le credenziali del database è molto importante.

Per questo motivo vogliamo utilizzare un database diverso per la produzione rispetto a quello di sviluppo e tenere anche le credenziali del database di produzione separate dal codice sorgente per poterle proteggere adeguatamente.

Se il suo fornitore di hosting supporta l'impostazione delle variabili d'ambiente tramite un'interfaccia web (come molti fanno), un modo per farlo è far ottenere al server l'URL del database da una variabile d'ambiente.
Di seguito modificheremo il sito web LocalLibrary per ottenere l'URI del database da una variabile d'ambiente del sistema operativo, se è stata definita, e in caso contrario utilizzeremo l'URL del database di sviluppo.

Apra **app.js** e trova la riga che imposta la variabile di connessione a MongoDB.
Assomiglierà a qualcosa di simile a:

```js
const mongoDB =
  "mongodb+srv://your_user_name:your_password@cluster0.cojoign.mongodb.net/local_library?retryWrites=true&w=majority";
```

Sostituisca la riga con il seguente codice che utilizza `process.env.MONGODB_URI` per ottenere la stringa di connessione da una variabile d'ambiente denominata `MONGODB_URI` se è stata impostata (usi il suo URL del database invece del segnaposto di seguito).

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
> Un altro modo comune per mantenere le credenziali del database di produzione separate dal codice sorgente è leggerle da un file `.env` che viene distribuito separatamente nel file system (ad esempio, potrebbero essere lette usando il modulo npm [dotenv](https://www.npmjs.com/package/dotenv)).

### Impostare NODE_ENV su 'production'

Possiamo rimuovere gli stack traces nelle pagine di errore impostando la variabile d'ambiente `NODE_ENV` su _production_ (è impostata su '_development_' per impostazione predefinita). Oltre a generare messaggi di errore meno verbosi, impostare la variabile su _production_ memorizza nella cache i modelli di vista e i file CSS generati dalle estensioni CSS. I test indicano che impostare `NODE_ENV` su _production_ può migliorare le prestazioni dell'app di un fattore tre!

Questa modifica può essere effettuata utilizzando `export`, un file di ambiente, o il sistema di inizializzazione del sistema operativo.

> [!NOTE]
> Questa è effettivamente una modifica che fa nella sua configurazione dell'ambiente piuttosto che nella sua app, ma abbastanza importante da essere annotata qui! Mostreremo come è impostata per il nostro esempio di hosting qui sotto.

### Registrare correttamente

Le chiamate di registrazione possono avere un impatto su un sito web ad alto traffico. In un ambiente di produzione, potrebbe dover registrare l'attività del sito web (ad esempio, tracciamento del traffico o registrazione delle chiamate API), ma dovrebbe cercare di minimizzare la quantità di registrazioni aggiunte per scopi di debug.

Un modo per minimizzare la registrazione "debug" in produzione è utilizzare un modulo come [debug](https://www.npmjs.com/package/debug) che permette di controllare quali registrazioni vengono eseguite impostando una variabile d'ambiente.
Ad esempio, il frammento di codice seguente mostra come potrebbe impostare la registrazione "autore".
La variabile di debug viene dichiarata con il nome 'author', e il prefisso "author" verrà visualizzato automaticamente per tutti i registri di questo oggetto.

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

Può quindi abilitare un particolare set di registri specificandoli come un elenco separato da virgole nella variabile d'ambiente `DEBUG`.
Può impostare le variabili per visualizzare i registri degli autori e dei libri come mostrato (sono supportati anche i caratteri jolly).

```bash
#Windows
set DEBUG=author,book

#Linux
export DEBUG="author,book"
```

> [!NOTE]
> Le chiamate a `debug` possono sostituire le registrazioni che potrebbe aver fatto in precedenza utilizzando `console.log()` o `console.error()`. Sostituisca qualsiasi chiamata a `console.log()` nel suo codice con la registrazione tramite il modulo [debug](https://www.npmjs.com/package/debug). Accenda e spenga la registrazione nel suo ambiente di sviluppo impostando la variabile DEBUG e osservi l'impatto che questo ha sulla registrazione.

Se ha bisogno di registrare l'attività del sito web può utilizzare una libreria di registrazione come _Winston_ o _Bunyan_. Per ulteriori informazioni su questo argomento veda: [Migliori pratiche di produzione: prestazioni e affidabilità](https://expressjs.com/en/advanced/best-practice-performance.html).

### Usare la compressione gzip/deflate per le risposte

Spesso i server web possono comprimere la risposta HTTP inviata a un client, riducendo significativamente il tempo richiesto al client per ottenere e caricare la pagina. Il metodo di compressione utilizzato dipenderà dai metodi di decompressione che il client dice di supportare nella richiesta (la risposta verrà inviata non compressa se non vengono supportati metodi di compressione).

Aggiunga questo al suo sito utilizzando il middleware [compression](https://www.npmjs.com/package/compression). Installi questo nella radice del suo progetto eseguendo il seguente comando:

```bash
npm install compression
```

Apra **./app.js** e richieda la libreria di compressione come mostrato. Aggiunga la libreria di compressione alla catena middleware con il metodo `use()` (questo dovrebbe apparire prima di qualsiasi rotta che desidera comprimere — in questo caso, tutte!)

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
> Per un sito web ad alto traffico in produzione non utilizzerebbe questo middleware. Invece, utilizzerebbe un reverse proxy come [Nginx](https://nginx.org/).

### Usare Helmet per proteggersi da vulnerabilità note

[Helmet](https://www.npmjs.com/package/helmet) è un pacchetto middleware che può impostare intestazioni HTTP appropriate che aiutano a proteggere la sua app dalle vulnerabilità web ben note (veda i [documenti](https://helmetjs.github.io/) per maggiori informazioni su quali intestazioni imposta e le vulnerabilità da cui protegge).

Installi questo nella radice del suo progetto eseguendo il seguente comando:

```bash
npm install helmet
```

Apra **./app.js** e richieda la libreria _helmet_ come mostrato.
Poi aggiunga il modulo alla catena middleware con il metodo `use()`.

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

Normalmente potremmo aver appena inserito `app.use(helmet());` per aggiungere il _sottoinsieme_ delle intestazioni correlate alla sicurezza che hanno senso per la maggior parte dei siti.
Tuttavia, nel [modello base di LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/LocalLibrary_base_template) includiamo alcuni script di bootstrap e jQuery.
Questi violano la _Content Security Policy (CSP)_ _default_ di helmet, che non permette il caricamento di script cross-site.
Per permettere il caricamento di questi script modifichiamo la configurazione di helmet in modo che imposti le direttive CSP per consentire il caricamento degli script dai domini indicati.
Per il suo server, può aggiungere/disabilitare intestazioni specifiche come necessario seguendo le [istruzioni per l'uso di helmet disponibili qui](https://www.npmjs.com/package/helmet).

### Aggiungere il rate limiting alle rotte API

[Express-rate-limit](https://www.npmjs.com/package/express-rate-limit) è un pacchetto middleware che può essere utilizzato per limitare le richieste ripetute a API e punti finali.
Ci sono molte ragioni per cui possono essere fatte richieste eccessive al suo sito, come attacchi denial of service, attacchi brute force, o anche solo un client o uno script che non si comporta come previsto.
Oltre ai problemi di prestazioni che possono derivare da troppe richieste che rallentano il suo server, potrebbe anche essere addebitato per il traffico aggiuntivo.
Questo pacchetto può essere utilizzato per limitare il numero di richieste che possono essere fatte a una particolare rotta o insieme di rotte.

Installi questo nella radice del suo progetto eseguendo il seguente comando:

```bash
npm install express-rate-limit
```

Apra **./app.js** e richieda la libreria _express-rate-limit_ come mostrato.
Poi aggiunga il modulo alla catena middleware con il metodo `use()`.

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

Il comando sopra limita tutte le richieste a 20 al minuto (può cambiare questo come necessario).

> [!NOTE]
> Servizi di terze parti come [Cloudflare](https://www.cloudflare.com/) possono anche essere utilizzati se ha bisogno di una protezione più avanzata contro il denial of service o altri tipi di attacchi.

#### Impostare la versione di node

Per le applicazioni node, includendo Express, il file **package.json** contiene tutto ciò che un fornitore di hosting dovrebbe aver bisogno per determinare le dipendenze dell'applicazione e il file del punto di ingresso.

L'unica informazione importante mancante dal nostro attuale **package.json** è la versione di node richiesta dalla libreria.
Può trovare la versione di node utilizzata per lo sviluppo inserendo il comando:

```bash
>node --version
v16.17.1
```

Apra **package.json**, e aggiunga queste informazioni come **engines > node** come mostrato (utilizzando il numero di versione per il suo sistema).

```json
  "engines": {
    "node": ">=16.17.1"
  },
```

Il servizio di hosting potrebbe non supportare la versione specifica di node indicata, ma questa modifica dovrebbe garantire che tenti di usare una versione con lo stesso numero di versione principale, o una versione più recente.

Si noti che ci possono essere altri modi per specificare la versione di node su diversi servizi di hosting, ma l'approccio con **package.json** è ampiamente supportato.

#### Ottenere le dipendenze e ritestare

Prima di procedere, testiamo di nuovo il sito e assicuriamoci che non sia stato influenzato da nessuna delle nostre modifiche.

Per prima cosa, dovremo ottenere le nostre dipendenze. Può farlo eseguendo il seguente comando nel suo terminale alla radice del progetto:

```bash
npm install
```

Ora esegua il sito (veda [Testare le rotte](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/routes#testing_the_routes) per i comandi rilevanti) e controlli che il sito si comporti ancora come si aspetta.

### Creare un repository dell'applicazione su GitHub

Molti servizi di hosting le permettono di importare e/o sincronizzare i progetti da un repository locale o da piattaforme di controllo di versione del codice sorgente basate sul cloud.
Questo può facilitare molto il deployment e lo sviluppo iterativo.

Per questo tutorial imposteremo un account [GitHub](https://github.com/) e un repository per la libreria, e utilizzeremo lo strumento **git** per caricare il nostro codice sorgente.

> [!NOTE]
> Può saltare questo passaggio se sta già usando GitHub per gestire il suo codice sorgente!
>
> Si noti che utilizzare strumenti di gestione del codice sorgente è una buona pratica di sviluppo software, in quanto le permette di provare le modifiche e passare tra i suoi esperimenti e il "codice buono noto" quando ne ha bisogno!

I passaggi sono:

1. Visiti <https://github.com/> e crei un account.
2. Una volta effettuato il login, faccia clic sul link **+** nella barra degli strumenti superiore e selezioni **New repository**.
3. Compili tutti i campi su questo modulo. Anche se non sono obbligatori, sono fortemente consigliati.

   - Inserisca un nuovo nome di repository (ad esempio, _express-locallibrary-tutorial_), e una descrizione (ad esempio, "Sito web Local Library scritto in Express (Node)").
   - Scelga **Node** nell'elenco di selezione _Add .gitignore_.
   - Scelga la sua licenza preferita nell'elenco di selezione _Add license_.
   - Selezioni **Initialize this repository with a README**.

   > [!WARNING]
   > L'accesso "Pubblico" predefinito renderà _tutto_ il codice sorgente — incluso il nome utente e la password del database — visibile a chiunque su Internet! Assicuri che il codice sorgente legga le credenziali _solo_ dalle variabili d'ambiente e non abbia alcuna credenziale hard-coded.
   >
   > Altrimenti, selezioni l'opzione "Privato" per permettere solo a persone selezionate di vedere il codice sorgente.

4. Premi **Create repository**.
5. Fai clic sul pulsante verde **Clone or download** sulla pagina del tuo nuovo repo.
6. Copia il valore dell'URL dal campo di testo all'interno della finestra di dialogo che appare.
   Se ha usato il nome del repository "express-locallibrary-tutorial", l'URL dovrebbe essere qualcosa del tipo: `https://github.com/<tuo_id_git>/express-locallibrary-tutorial.git`.

Ora che il repository ("repo") è creato su GitHub, vogliamo clonarlo (copiarlo) sul nostro computer locale:

1. Installa _git_ per il suo computer locale (può trovare versioni per diverse piattaforme [qui](https://git-scm.com/downloads)).
2. Apra un prompt dei comandi/terminale e cloni il suo repo utilizzando l'URL che ha copiato sopra:

   ```bash
   git clone https://github.com/<your_git_user_id>/express-locallibrary-tutorial.git
   ```

   Questo creerà il repository all'interno della directory corrente.

3. Naviga nella cartella del repo.

   ```bash
   cd express-locallibrary-tutorial
   ```

Poi copi i suoi file sorgente dell'applicazione nella cartella del repo, li renda parte del repo utilizzando _git_, e li carichi su GitHub:

1. Copi la sua applicazione Express in questa cartella (escludendo **/node_modules**, che contiene i file di dipendenza che dovrebbe ottenere da npm secondo necessità).
2. Apra un prompt dei comandi/terminale e usi il comando `add` per aggiungere tutti i file a git.

   ```bash
   git add -A
   ```

3. Utilizzi il comando `status` per controllare che tutti i file che sta per `commit` siano corretti (vuole includere i file sorgente, non i binari, i file temporanei, ecc.).
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

4. Quando è soddisfatto, `commit` i file al suo repository locale.
   Questo è equivalente a firmare le modifiche e renderle una parte ufficiale del repo locale.

   ```bash
   git commit -m "First version of application moved into GitHub"
   ```

5. A questo punto, il repository remoto non è stato modificato.
   L'ultimo passaggio è sincronizzare (`push`) il suo repo locale sul repository GitHub remoto utilizzando il seguente comando:

   ```bash
   git push origin main
   ```

Quando questa operazione si completa, dovrebbe essere in grado di tornare alla pagina su GitHub dove ha creato il suo repo, aggiornare la pagina, e vedere che tutta la sua applicazione è stata ora caricata. Può continuare ad aggiornare il suo repo man mano che i file cambiano utilizzando questo ciclo di add/commit/push.

Questo è un buon punto per fare un backup del suo progetto "vanilla" — mentre alcune delle modifiche che stiamo per fare nelle sezioni seguenti potrebbero essere utili per il breakdown su qualsiasi servizio di hosting (o per lo sviluppo) altre potrebbero non esserlo.
Può farlo usando `git` dalla riga di comando:

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
> Per saperne di più, veda [Imparare Git](https://docs.github.com/en/get-started/start-your-journey/git-and-github-learning-resources).

## Esempio: Hosting su Glitch

Questa sezione fornisce una dimostrazione pratica di come ospitare _LocalLibrary_ su [Glitch](https://glitch.com/).

### Perché Glitch?

Abbiamo scelto di utilizzare Glitch per diverse ragioni:

- Glitch ha un [piano starter gratuito](https://glitch.com/pricing) che è _davvero_ gratuito, sebbene con alcune limitazioni.
  Il fatto che sia accessibile a tutti gli sviluppatori è davvero importante per MDN!
- Glitch si occupa dell'infrastruttura, così non deve farlo lei.
  Non doversi preoccupare di server, load balancers, reverse proxies, e così via, rende molto più facile iniziare.
- Le competenze e i concetti che imparerà utilizzando Glitch sono trasferibili.
- Le limitazioni del servizio e del piano non incidono davvero sull'utilizzo di Glitch per il tutorial.
  Ad esempio:

  - Il piano starter offre solo 1000 "ore di progetto" al mese, che viene reimpostato mensilmente.
    Questo è utilizzato quando sta modificando attivamente il sito o se qualcuno lo sta accedendo.
    Se nessuno sta accedendo o modificando il sito, si addormenterà.
  - L'ambiente del piano starter ha una quantità limitata di memoria RAM del contenitore e spazio di archiviazione.
    C'è più che abbastanza per il tutorial, in particolare perché il nostro database è ospitato altrove.
  - I domini personalizzati non sono ben supportati (al momento della scrittura).
  - Altre limitazioni possono essere trovate nella [pagina delle restrizioni tecniche di Glitch](https://help.glitch.com/s/article/Technical-Restrictions).

Anche se Glitch è appropriato per l'hosting di questa dimostrazione, dovrebbe prendersi il tempo di determinare se è [adatto al suo sito web](#scegliere_un_fornitore_di_hosting).

### Come funziona Glitch?

Glitch fornisce un'interfaccia basata sul web in cui può creare progetti da modelli starter, o importarli da GitHub, e quindi aggiungere e modificare i file del progetto.
Man mano che apporta modifiche, il progetto viene costruito ed eseguito nel proprio contenitore virtualizzato isolato e indipendente.

Come tutto ciò funzioni "sotto il cofano" è un mistero — Glitch non lo dice.
Quello che è chiaro è che fintanto che crea un'applicazione web nodejs abbastanza standard (ad esempio, utilizzando `package.json` per le sue dipendenze), e non consuma più risorse di quelle elencate nelle [restrizioni tecniche](https://help.glitch.com/s/article/Technical-Restrictions), la sua applicazione dovrebbe "funzionare semplicemente".

Una volta che l'applicazione è in esecuzione, può essere configurata per la produzione utilizzando [dati privati](https://help.glitch.com/s/article/Adding-Private-Data) forniti in un file `.env`.
I valori nei dati segreti vengono letti dall'applicazione come variabili d'ambiente, che, come ricorderà da una sezione precedente, è come abbiamo configurato la nostra applicazione per ottenere il suo URL del database.
Si noti che le variabili sono _segrete_: il `.env` non dovrebbe essere incluso nel suo repository GitHub.

La vista di modifica di Glitch fornisce anche l'accesso _terminale_ all'ambiente dell'app web, che può utilizzare per lavorare con l'app web come se fosse in esecuzione sulla sua macchina locale.

Questo è tutto ciò di cui ha bisogno per iniziare.
Successivamente, imposteremo un account Glitch, caricheremo il progetto Library da GitHub e lo collegheremo a un database.

### Ottenere un account Glitch

Per iniziare a usare Glitch dovrà prima creare un account:

- Vada su [glitch.com](https://glitch.com/) e faccia clic sul pulsante **Sign up** nella barra degli strumenti superiore.
- Selezioni GitHub nel popup per iscriversi utilizzando le sue credenziali GitHub.
- Sarà quindi effettuato l'accesso al dashboard di Glitch: <https://glitch.com/dashboard>.

### Risoluzione dei problemi sulla versione di Node.js

I fornitori di hosting supportano comunemente una certa versione principale delle recenti release di Node.js.
Se la versione "minor" esatta specificata nel suo file `package.json` non è supportata, di solito essi passeranno alla versione più vicina supportata (e spesso ciò funzionerà).

Purtroppo, al momento della scrittura, la versione più alta supportata su Glitch è Node.js 16.
Se ha sviluppato con Node.js 17 o successivo, dovrebbe ridurre la versione utilizzata nel suo file `package.json` come mostrato.
Avrà anche bisogno di ritestare:

```json
  "engines": {
    "node": ">=v16"
  },
```

Glitch [prevede di aggiornare node e tenerlo meglio aggiornato nel futuro](https://blog.glitch.com/post/rebuilding-glitch/) — e potrebbe essere che quando leggerà questo, il limite di versione non esista più.
Invece di downgradare la versione di `node`, potrebbe caricare il suo progetto per vedere se viene costruito.
Se ci sono errori e la sua applicazione non si carica, dovrebbe provare a impostare la versione di `node` su `>=v16` nel suo `package.json` nell'editor di Glitch.

> [!NOTE]
> Può anche controllare le versioni supportate inserendo il seguente comando nel terminale di qualsiasi progetto Glitch:
>
> ```sh
> ls -l /opt/nvm/versions/node | grep '^d' | awk '{ print $9 }'
> ```

### Distribuire su Glitch da GitHub

Successivamente importeremo il progetto Library da GitHub.
Per prima cosa scelga l'opzione **Dashboard** dal menu superiore del sito, quindi selezioni il pulsante **New project**.
Glitch mostrerà un elenco di opzioni per il nuovo progetto; selezioni **Import from GitHub**.

![Dashboard del sito Web Glitch che mostra un nuovo pulsante di progetto e un menu popup con l'opzione "Import from GitHub"](glitch_new_project_import_github.png)

Apparirà un popup.
Inserisca l'URL del repository della libreria GitHub nel popup e premi **OK**.
Di seguito, abbiamo inserito il repository per il progetto lavorato.

![Popup di Glitch per inserire l'URL del repository GitHub da importare](glitch_new_project_github_repo_url.png)

Glitch importerà quindi il progetto, visualizzando le notifiche dei progressi.
Al termine, visualizzerà la vista di modifica per il nuovo progetto, come mostrato di seguito.

![Vista dell'editor di Glitch per il progetto importato](glitch_imported_project_in_editor.png)

Può ottenere l'URL del sito live selezionando il pulsante **Share**.

![Vista dell'editor di Glitch per il progetto importato](glitch_share_project.png)

Apra una nuova scheda del browser e copi il link per il sito live nella barra degli indirizzi.
Il sito locale della biblioteca dovrebbe aprirsi e visualizzare i dati del database di sviluppo.

> [!NOTE]
> Questo processo è stato un'importazione una tantum da GitHub.
> Può anche utilizzare azioni GitHub come [glitch-project-sync](https://github.com/marketplace/actions/glitch-project-sync) per mantenere Glitch e il suo progetto sincronizzati.

### Usare un database MongoDB di produzione

Dovrebbe impostare un database diverso per la produzione rispetto allo sviluppo.
Anche se Glitch ospita solo database SQLite (e siamo configurati per usare MongoDB), molti altri siti forniscono database MongoDB come servizio.

Una opzione è seguire le istruzioni [Impostazione del database MongoDB](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose#setting_up_the_mongodb_database) da prima nel tutorial per impostare un nuovo database di produzione.

Per rendere accessibile il database di produzione all'applicazione della libreria, apra il file `.env` nella vista dell'editor per il progetto.
Inserisca la variabile URL del database `MONGODB_URI` e l'URL del suo database di produzione.
Il sito si aggiorna mentre inserisce i valori nell'editor.

![Editor di file .env di Glitch per dati privati con variabili di produzione](glitch_env.png)

> [!NOTE]
> Non abbiamo creato questo file.
> È destinato ai [dati privati](https://help.glitch.com/s/article/Adding-Private-Data) ed è stato creato automaticamente all'importazione su Glitch.
> Non viene mai esportato o copiato.

### Altre variabili di configurazione

Ricorderà da una sezione precedente che dobbiamo [impostare NODE_ENV su 'production'](#set_node_env_to_production) per migliorare le prestazioni e generare messaggi di errore meno verbosi. Facciamo questo nello stesso file in cui impostiamo la variabile `MONGODB_URI`.

Apra `.env` e aggiunga una variabile `NODE_ENV` con valore `production` (vedi lo screenshot nella sezione precedente).

L'applicazione della biblioteca locale è ora configurata e configurata per l'uso in produzione.
Può aggiungere dati tramite l'interfaccia del sito web, e dovrebbe funzionare come ha fatto durante lo sviluppo (sebbene con meno informazioni di debug esposte per pagine non valide).

> [!NOTE]
> Se vuole solo aggiungere alcuni dati per il test, potrebbe usare lo script `populatedb` (con il suo URL del database di produzione MongoDB) come discusso nella sezione [Parte 3 del Tutorial su Express: Usare un Database (con Mongoose) Test — creare alcuni oggetti](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose#testing_%E2%80%94_create_some_items).

### Debugging di applicazioni Express su Glitch

Glitch permette un debugging efficace.
Alcune delle cose che può fare sono:

- Selezioni il pulsante dei log nella parte inferiore della vista dell'editor per vedere le informazioni di log dal suo server, come l'output dei log console.
- Selezionare il pulsante del terminale nella parte inferiore della vista dell'editor per aprire un terminale nell'ambiente di hosting.
  Può utilizzare questo per eseguire comandi e strumenti nell'ambiente.
  Ad esempio, potrebbe utilizzare `node -v` per controllare la versione di node.
- Debugging interattivo in VS Code usando l'estensione GLITCH per VS Code.

## Esempio: Hosting su Railway

Questa sezione fornisce una dimostrazione pratica di come installare _LocalLibrary_ su [Railway](https://railway.com/).

### Perché Railway?

> [!WARNING]
> Railway non ha più un livello starter completamente gratuito.
> Abbiamo conservato queste istruzioni perché Railway ha alcune ottime funzionalità e sarà una migliore opzione per alcuni utenti.

Railway è un'opzione di hosting interessante per diverse ragioni:

- Railway si occupa della maggior parte dell'infrastruttura, così non deve farlo lei.
  Non doversi preoccupare di server, load balancers, reverse proxies, e così via, rende molto più facile iniziare.
- Railway ha un [focus sull'esperienza dello sviluppatore nello sviluppo e nel deployment](https://docs.railway.com/maturity/compare-to-heroku), che si traduce in una curva di apprendimento più veloce e morbida rispetto a molte altre alternative.
- Le competenze e i concetti che imparerà utilizzando Railway sono trasferibili.
  Mentre Railway ha alcune eccellenti nuove funzionalità, altri servizi di hosting popolari utilizzano molte delle stesse idee e approcci.
- La [documentazione di Railway](https://docs.railway.com/) è chiara e completa.
- Il servizio sembra essere molto affidabile e se finisce per amarlo, il prezzo è prevedibile e scalare l'app è molto facile.

Dovrebbe prendersi il tempo di determinare se Railway è [adatto per il suo sito web](#scegliere_un_fornitore_di_hosting).

### Come funziona Railway?

Le applicazioni web vengono eseguite ciascuna nel proprio contenitore virtualizzato isolato e indipendente.
Per eseguire la sua applicazione, Railway deve essere in grado di configurare l'ambiente e le dipendenze appropriate e anche comprendere come viene avviata.

Railway rende questo semplice, poiché può riconoscere e installare automaticamente molti diversi framework e ambienti di applicazioni web basandosi sull'uso di "convenzioni comuni".
Ad esempio, Railway riconosce le applicazioni node perché hanno un file **package.json**, e può determinare il gestore pacchetti usato per la costruzione dal file di "lock".
Ad esempio, se l'applicazione include il file **package-lock.json**, Railway sa di usare _npm_ per installare i pacchetti, mentre se trova **yarn.lock**, sa di usare _yarn_.
Dopo aver installato tutte le dipendenze, Railway cercherà script denominati "build" e "start" nel file di pacchetto, e li utilizzerà per costruire ed eseguire il codice.

> [!NOTE]
> Railway usa [Nixpacks](https://nixpacks.com/docs) per riconoscere vari framework di applicazioni web scritti in diversi linguaggi di programmazione.
> Non ha bisogno di sapere altro per questo tutorial, ma può scoprire di più sulle opzioni per distribuire applicazioni node in [Nixpacks Node](https://nixpacks.com/docs/providers/node).

Una volta che l'applicazione è in esecuzione, può configurarsi utilizzando informazioni fornite in [variabili d'ambiente](https://docs.railway.com/guides/variables).
Ad esempio, un'applicazione che usa un database deve ottenere l'indirizzo usando una variabile.
Il servizio di database stesso può essere ospitato da Railway o da un altro fornitore.

Gli sviluppatori interagiscono con Railway tramite il sito Railway e utilizzando uno strumento speciale [Command Line Interface (CLI)](https://docs.railway.com/guides/cli).
La CLI le permette di associare un repository GitHub locale a un progetto Railway, caricare il repository dal branch locale sul sito live, ispezionare i log del processo in esecuzione, impostare e ottenere variabili di configurazione e molto altro.
Una delle caratteristiche più utili è che può utilizzare la CLI per eseguire il suo progetto locale con le stesse variabili d'ambiente del progetto live.

Questo è tutto ciò di cui ha bisogno per disporre l'app su Railway.
Successivamente, imposteremo un account Railway, installeremo il nostro sito web e un database, e proveremo il client di Railway.

### Ottenere un account Railway

Per iniziare a usare Railway dovrà prima creare un account:

- Vada su [railway.com](https://railway.com/) e faccia clic sul collegamento **Login** nella barra degli strumenti superiore.
- Selezioni GitHub nel popup per accedere utilizzando le sue credenziali GitHub
- Potrebbe quindi dover controllare la sua email e verificare il suo account.
- Sarà quindi effettuato l'accesso al dashboard di Railway.com: <https://railway.com/dashboard>.

### Distribuire su Railway da GitHub

Successivamente imposteremo Railway per distribuire la nostra libreria da GitHub.
Per prima cosa scelga l'opzione **Dashboard** dal menu superiore del sito, quindi selezioni il pulsante **New Project**:

![Dashboard del sito Web Railway che mostra il pulsante nuovo progetto](railway_new_project_button.png)

Railway mostrerà un elenco di opzioni per il nuovo progetto, che includono l'opzione per distribuire un progetto da un modello creato nel suo account GitHub e un certo numero di database.
Selezioni **Deploy from GitHub repo**.

![Popup Railway che mostra le opzioni di deployment con l'opzione Deploy from GitHub repo evidenziata](railway_new_project_button_deploy_github_repo.png)

Verranno visualizzati tutti i progetti nei repository GitHub che ha condiviso con Railway durante la configurazione.
Selezioni il suo repository GitHub per la biblioteca locale: `<nome-utente>/express-locallibrary-tutorial`.

![Popup Railway che mostra i repository GitHub che possono essere distribuiti](railway_new_project_button_deploy_github_selectrepo.png)

Confermi la sua distribuzione selezionando **Deploy Now**.

![Schermo di conferma quando può selezionare la distribuzione del progetto](railway_new_project_deploy_confirm.png)

Railway caricherà e distribuirà quindi il suo progetto, mostrando i progressi sulla pagina delle distribuzioni.
Quando la distribuzione verrà completata con successo, vedrà una schermata come quella qui sotto.

![Dashboard di Railway che mostra la scheda Deployments per il progetto distribuito](railway_project_deploy.png)

Ora selezioni la scheda _Settings_, quindi scorra verso il basso fino alla sezione Domains e premi il pulsante **Generate Domain**.

![Scheda delle impostazioni del progetto Railway che mostra il pulsante per generare un dominio](railway_project_generate_domain.png)

Questo pubblicherà il sito e metterà il dominio al posto del pulsante, come mostrato di seguito.

![Scheda delle impostazioni del progetto Railway che mostra un link al sito della biblioteca locale](railway_project_domain.png)

Selezioni l'URL del dominio per aprire la sua applicazione della libreria.
Si noti che poiché non abbiamo specificato un database di produzione, la biblioteca locale si aprirà utilizzando i dati di sviluppo.

### Provision e connessione di un database MongoDB

Invece di usare i nostri dati di sviluppo, procediamo ora a creare un database di produzione MongoDB da usare.
Creeremo il database come parte del progetto dell'applicazione Railway, anche se nulla impedisce di crearlo nel suo progetto separato o, anzi, di usare un database _MongoDB Atlas_ per i dati di produzione, proprio come ha fatto per il database di sviluppo.

Su Railway, scelga l'opzione **Dashboard** dalla barra dei menu superiore e quindi selezioni il suo progetto di applicazione.
In questo stato contiene solo un singolo servizio per la sua applicazione (questo può essere selezionato per impostare le variabili e altri dettagli del servizio).
Selezioni il pulsante **New**, utilizzato per aggiungere servizi al progetto corrente.

![Progetto di Railway con il pulsante nuovo servizio evidenziato](railway_project_open_no_database.png)

Selezioni **Database** quando richiesto sul tipo di servizio da aggiungere:

![Popup Railway che mostra le opzioni per un nuovo servizio, come database, repository GitHub, servizio vuoto ecc](railway_database_add.png)

Quindi selezioni **Add MongoDB** per iniziare ad aggiungere il database

![Popup Railway che mostra i diversi database che possono essere selezionati: Postgres, MySQL, MongoDB e così via](railway_database_select_type.png)

Railway prorogherà quindi un servizio contenente un database vuoto nello stesso progetto.
Al completamento, vedrà ora entrambi i servizi di applicazione e database nella vista del progetto.

![Progetto di Railway con servizi applicativi e di database](railway_project_two_services.png)

Selezioni il servizio MongoDB per visualizzare le informazioni sul database.
Apra la scheda _Variables_ e copi l'"Mongo_URL" (questo è l'indirizzo del database).

![Schermata delle impostazioni del database Railway che mostra l'URL necessario per connettersi al database](railway_mongodb_connect.png)

Per rendere questo accessibile all'applicazione della libreria, dobbiamo aggiungerlo al processo dell'applicazione utilizzando una variabile d'ambiente.
Apra prima il servizio dell'applicazione.
Selezioni quindi la scheda _Variables_ e premi il pulsante **New Variable**.

Inserisca il nome della variabile `MONGODB_URI` e l'URL di connessione che ha copiato per il database (`MONGODB_URI` è il nome della variabile d'ambiente da cui [abbiamo configurato l'applicazione](#configurazione_del_database) per leggere l'indirizzo del database).
Questo apparirà in modo simile alla schermata mostrata di seguito.

![Schermata delle variabili Railway durante l'aggiunta della variabile MONGODB_URI e dell'indirizzo](railway_variables_database_url.png)

Selezioni **Add** per aggiungere la variabile.

Railway riavvia la sua app quando aggiorna le variabili. Se controlla la home page ora, dovrebbe mostrare zero valori per i conteggi degli oggetti, poiché le modifiche sopra indicano che stiamo ora utilizzando un nuovo database (vuoto).

### Altre variabili di configurazione

Ricorderà da una sezione precedente che dobbiamo [impostare NODE_ENV su 'production'](#set_node_env_to_production) per migliorare le prestazioni e generare messaggi di errore meno verbosi. Possiamo farlo nella stessa schermata in cui impostiamo la variabile `MONGODB_URI`.

Apra il servizio dell'applicazione.
Selezioni quindi la scheda _Variables_, dove vedrà che `MONGODB_URI` è già definita, e premi il pulsante **New Variable**.

![Scheda delle variabili di Railway con il pulsante New Variable evidenziato](railway_variables_new.png)

Inserisca `NODE_ENV` come nome della nuova variabile e `production` come nome dell'ambiente.
Poi premi il pulsante **Add**.

![Scheda delle variabili di Railway con la nuova variabile NODE_ENV impostata su 'production'](railway_variables_new_node_env.png)

L'applicazione della biblioteca locale è ora configurata e configurata per l'uso in produzione.
Può aggiungere dati tramite l'interfaccia del sito web e dovrebbe funzionare nello stesso modo in cui ha fatto durante lo sviluppo (sebbene con meno informazioni di debug esposte per le pagine non valide).

> [!NOTE]
> Se vuole solo aggiungere alcuni dati per il test, potrebbe usare lo script `populatedb` (con il suo URL del database di produzione MongoDB) come discusso nella sezione [Parte 3 del Tutorial su Express: Usare un Database (con Mongoose) Test — creare alcuni oggetti](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose#testing_%E2%80%94_create_some_items).

### Installare il client

Scarichi e installi il client Railway per il suo sistema operativo locale seguendo le [istruzioni qui](https://docs.railway.com/guides/cli).

Dopo l'installazione del client sarà in grado di eseguire comandi.
Alcune delle operazioni più importanti includono la distribuzione della directory corrente del suo computer a un progetto Railway associato (senza dover caricare su GitHub) e l'esecuzione del suo progetto localmente utilizzando le stesse impostazioni che ha sul server di produzione.

Può ottenere un elenco di tutti i comandi possibili inserendo quanto segue in un terminale.

```bash
railway help
```

### Debugging

Il client Railway fornisce il comando logs per visualizzare la coda dei registri (un log più completo è disponibile sul sito per ciascun progetto):

```bash
railway logs
```

## Sommario

Questo è la fine di questo tutorial sulla configurazione delle app Express in produzione, e anche della serie di tutorial sul lavoro con Express. Speriamo che li abbia trovati utili. Può controllare una versione completamente svolta del [codice sorgente su GitHub qui](https://github.com/mdn/express-locallibrary-tutorial).

## Vedi anche

- [Migliori pratiche di produzione: prestazioni e affidabilità](https://expressjs.com/en/advanced/best-practice-performance.html) (documenti Express)
- [Migliori pratiche di produzione: Sicurezza](https://expressjs.com/en/advanced/best-practice-security.html) (documenti Express)
- Documenti Railway

  - [CLI](https://docs.railway.com/guides/cli)

- DigitalOcean

  - Tutorial su [Express](https://www.digitalocean.com/community/tutorials?q=express)
  - Tutorial su [Node.js](https://www.digitalocean.com/community/tutorials?q=node.js)

- Heroku

  - [Iniziare su Heroku con Node.js](https://devcenter.heroku.com/articles/getting-started-with-nodejs) (documenti Heroku)
  - [Distribuire applicazioni Node.js su Heroku](https://devcenter.heroku.com/articles/deploying-nodejs) (documenti Heroku)
  - [Supporto Node.js di Heroku](https://devcenter.heroku.com/articles/nodejs-support) (documenti Heroku)
  - [Ottimizzare la concorrenza delle applicazioni Node.js](https://devcenter.heroku.com/articles/node-concurrency) (documenti Heroku)
  - [Come funziona Heroku](https://devcenter.heroku.com/articles/how-heroku-works) (documenti Heroku)
  - [Dynos e il Dyno Manager](https://devcenter.heroku.com/articles/dynos) (documenti Heroku)
  - [Configurazione e variabili di configurazione](https://devcenter.heroku.com/articles/config-vars) (documenti Heroku)
  - [Limiti](https://devcenter.heroku.com/articles/limits) (documenti Heroku)

{{PreviousMenu("Learn_web_development/Extensions/Server-side/Express_Nodejs/forms", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}
