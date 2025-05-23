---
title: "Express Tutorial Parte 7: Distribuzione in produzione"
short-title: "7: Distribuire"
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/deployment
l10n:
  sourceCommit: e488eba036b2fee56444fd579c3759ef45ff2ca8
---

{{PreviousMenu("Learn_web_development/Extensions/Server-side/Express_Nodejs/forms", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}

Ora che avete creato (e testato) un fantastico sito web della [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website), vorrete installarlo su un server web pubblico in modo che possa essere accessibile dal personale della biblioteca e dai membri tramite Internet. Questo articolo fornisce una panoramica su come trovare un host per distribuire il vostro sito web e ciò che è necessario fare per preparare il vostro sito per il lancio in produzione.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Completare tutti gli argomenti precedenti del tutorial, inclusa <a href="/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms">Express Tutorial Parte 6: Lavorare con i moduli</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare dove e come si può distribuire un'app Express in produzione.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

Una volta completato il vostro sito (o abbastanza completato per iniziare il collaudo pubblico), dovrete ospitarlo in un luogo più pubblico e accessibile rispetto al vostro computer di sviluppo personale.

Finora, avete lavorato in un [ambiente di sviluppo](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment), utilizzando Express/Node come server web per condividere il vostro sito con il browser/rete locale, e gestendo il vostro sito web con impostazioni di sviluppo (insicure) che espongono il debug e altre informazioni private. Prima di poter ospitare un sito web esternamente, dovrete innanzitutto:

- Scegliere un ambiente per ospitare l'app Express.
- Effettuare alcune modifiche alle impostazioni del vostro progetto.
- Configurare un'infrastruttura a livello di produzione per servire il vostro sito web.

Questo tutorial fornisce alcune indicazioni sulle opzioni disponibili per scegliere un sito di hosting, una breve panoramica di ciò che è necessario fare per preparare la vostra app Express per la produzione, e un esempio pratico di come installare il sito web LocalLibrary sul servizio di hosting cloud [Railway](https://railway.com/).

## Cos'è un ambiente di produzione?

L'ambiente di produzione è l'ambiente fornito dal computer server dove farete girare il sito web per il consumo esterno. L'ambiente include:

- Hardware del computer su cui gira il sito web.
- Sistema operativo (ad esempio, Linux o Windows).
- Linguaggio di programmazione e librerie di framework sopra cui è scritto il sito web.
- Infrastruttura del server web, eventualmente inclusa un server web, proxy inverso, bilanciatore del carico, ecc.
- Database dai quali il vostro sito web dipende.

Il computer server potrebbe essere situato nei vostri locali e connesso a Internet tramite un collegamento veloce, ma è molto più comune usare un computer ospitato "nel cloud". Questo significa in realtà che il vostro codice viene eseguito su un qualche computer remoto (o forse un computer "virtuale") nel o nei data center della vostra compagnia di hosting. Il server remoto offrirà generalmente un certo livello garantito di risorse informatiche (ad esempio, CPU, RAM, memoria di archiviazione, ecc.) e connettività Internet a un determinato prezzo.

Questo tipo di hardware di rete/informatica accessibile in remoto è definito _Infrastructure as a Service (IaaS)_. Molti fornitori IaaS offrono opzioni per preinstallare un sistema operativo particolare, sul quale dovete installare gli altri componenti del vostro ambiente di produzione. Altri fornitori permettono di selezionare ambienti più completi, che potrebbero includere un'installazione completa di Node.

> [!NOTE]
> Gli ambienti preconfigurati possono rendere molto facile impostare il vostro sito web perché riducono la configurazione, ma le opzioni disponibili potrebbero limitarvi a un server sconosciuto (o altri componenti) e potrebbero essere basate su una versione più vecchia del sistema operativo. Spesso è meglio installare i componenti da soli in modo da ottenere quelli desiderati, e quando avete bisogno di aggiornare parti del sistema, sapete da dove iniziare!

Altri provider di hosting supportano Express come parte di un'offerta _Platform as a Service_ (_PaaS_). Utilizzando questo tipo di hosting non dovete preoccuparvi della maggior parte del vostro ambiente di produzione (server, bilanciatori del carico, ecc.) poiché la piattaforma ospitante si occupa di questi aspetti per voi. Questo rende la distribuzione piuttosto semplice perché dovete concentrarvi solo sulla vostra applicazione web e non su alcuna altra infrastruttura del server.

Alcuni sviluppatori sceglieranno la maggiore flessibilità fornita da IaaS rispetto a PaaS, mentre altri apprezzeranno la minore manutenzione necessaria e la più facile scalabilità di PaaS. Quando iniziate, impostare il vostro sito web su un sistema PaaS è molto più semplice, ed è quindi ciò che faremo in questo tutorial.

> [!NOTE]
> Se sceglierete un provider di hosting amico di Node/Express, questi ultimi dovrebbero fornire istruzioni su come impostare un sito web Express usando diverse configurazioni di server web, server applicativo, proxy inverso, ecc. Ad esempio, ci sono molte guide passo per passo per varie configurazioni nei [documenti della comunità di DigitalOcean Node](https://www.digitalocean.com/community/tutorials?q=node).

## Scegliere un fornitore di hosting

Esistono numerosi fornitori di servizi di hosting noti per supportare attivamente o funzionare bene con _Node_ (e _Express_). Questi fornitori offrono diversi tipi di ambienti (IaaS, PaaS), e diversi livelli di risorse informatiche e di rete a prezzi differenti.

> [!NOTE]
> Ci sono molte soluzioni di hosting, e i loro servizi e prezzi possono cambiare nel tempo. Mentre introduciamo alcune opzioni di seguito, vale la pena verificare sia queste che altre opzioni prima di selezionare un provider di hosting.

Alcune delle cose da considerare quando si sceglie un host:

- Quanto sarà trafficato il vostro sito e il costo dei dati e delle risorse informatiche necessarie per soddisfare quella domanda.
- Livello di supporto per lo scaling orizzontale (aggiunta di più macchine) e verticale (aggiornamento a macchine più potenti) e i costi che ne derivano.
- Le posizioni in cui il fornitore dispone di data center, e quindi dove l'accesso è probabilmente più veloce.
- Performance storica di uptime e downtime dell'host.
- Strumenti forniti per gestire il sito — sono facili da usare e sicuri (ad esempio, SFTP rispetto a FTP).
- Framework integrati per monitorare il vostro server.
- Limiti noti. Alcuni host bloccheranno deliberatamente alcuni servizi (ad esempio, e-mail). Altri offrono solo un certo numero di ore di "tempo reale" in alcune categorie di prezzo, o offrono solo una piccola quantità di archiviazione.
- Benefici aggiuntivi. Alcuni fornitori offriranno nomi di dominio gratuiti e supporto per certificati TLS che altrimenti dovreste pagare.
- Se il "livello gratuito" su cui fate affidamento scade nel tempo, e se il costo di migrare a un livello più costoso significa che sarebbe stato meglio utilizzare un altro servizio fin dall'inizio!

La buona notizia è che quando iniziate ci sono parecchi siti che offrono ambienti informatici "gratuiti", pensati per la valutazione e il collaudo.
Questi si presentano generalmente come ambienti limitati e con risorse relativamente contenute, ma è bene essere consapevoli che potrebbero scadere dopo un certo periodo introduttivo o avere altre limitazioni.
Sono comunque eccellenti per testare siti a basso traffico in un ambiente ospitato e possono offrire una facile migrazione al pagamento per maggiori risorse quando il sito diventa più frequentato.
Scelte popolari in questa categoria includono [Glitch](https://glitch.com/), [Python Anywhere](https://www.pythonanywhere.com/), [Amazon Web Services](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/billing-free-tier.html), [Microsoft Azure](https://azure.microsoft.com/en-us/pricing/details/app-service/linux/), ecc.

La maggior parte dei fornitori offre anche un livello "base" o "hobby" pensato per siti di piccole dimensioni, che forniscono livelli più utili di potenza informatica e meno restrizioni.
[Railway](https://railway.com/), [Heroku](https://www.heroku.com/), [DigitalOcean](https://www.digitalocean.com/) e [Python Anywhere](https://www.pythonanywhere.com/) sono esempi di popolari fornitori di hosting che hanno un livello di computazione base relativamente economico (nella fascia di prezzo compresa tra 5 e 10 dollari al mese).

> [!NOTE]
> Ricordate che il prezzo non è l'unico criterio di selezione.
> Se il vostro sito web avrà successo, potrebbe risultare che la scalabilità sarà la considerazione più importante.

## Preparare il sito web per la pubblicazione

Le principali cose a cui pensare quando si pubblica il vostro sito web sono la sicurezza web e le prestazioni.
Al minimo indispensabile, vorrete modificare la configurazione del database in modo da poter utilizzare un database diverso per la produzione e proteggere le sue credenziali, rimuovere le tracce di stack che sono incluse nelle pagine di errore durante lo sviluppo, pulire il vostro logging e impostare gli header appropriati per evitare molte minacce comuni alla sicurezza.

Nelle seguenti sottosezioni, delineiamo i cambiamenti più importanti che dovreste fare alla vostra applicazione.

> [!NOTE]
> Ci sono altri consigli utili nei documenti di Express — vedere [Best practices di produzione: performance e affidabilità](https://expressjs.com/en/advanced/best-practice-performance.html) e [Migliori pratiche di produzione: Sicurezza](https://expressjs.com/en/advanced/best-practice-security.html).

### Configurazione del database

Finora, in questo tutorial, abbiamo usato un unico database di sviluppo, per il quale l'indirizzo e le credenziali sono hard-coded in **app.js**.
Poiché il database di sviluppo non contiene alcuna informazione che ci preoccupi venga esposta o corrotta, non c'è particolare rischio nel rivelare questi dettagli.
Tuttavia, se state lavorando con dati reali, in particolare informazioni personali degli utenti, proteggere le credenziali del database è molto importante.

Per questo motivo, vogliamo utilizzare un database diverso per la produzione rispetto a quello che utilizziamo per lo sviluppo, e tenere anche le credenziali del database di produzione separate dal codice sorgente in modo che possano essere adeguatamente protette.

Se il vostro fornitore di hosting supporta l'impostazione delle variabili d'ambiente tramite un'interfaccia web (come fanno molte), un modo per farlo è far ottenere al server l'URL del database da una variabile d'ambiente.
Di seguito modifichiamo il sito web LocalLibrary per ottenere l'URI del database da una variabile d'ambiente del sistema operativo, se è stata definita, e altrimenti utilizziamo l'URL del database di sviluppo.

Aprite **app.js** e trovate la linea che imposta la variabile di connessione MongoDB.
Sarà simile a questa:

```js
const mongoDB =
  "mongodb+srv://your_user_name:your_password@cluster0.cojoign.mongodb.net/local_library?retryWrites=true&w=majority";
```

Sostituite la linea con il seguente codice che utilizza `process.env.MONGODB_URI` per ottenere la stringa di connessione da una variabile d'ambiente chiamata `MONGODB_URI`, se è stata impostata (utilizzate il vostro URL del database invece del segnaposto sottostante).

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
> Un altro metodo comune per mantenere separate le credenziali del database di produzione dal codice sorgente è leggerle da un file `.env` che viene distribuito separatamente nel file system (ad esempio, potrebbero essere lette usando il modulo npm [dotenv](https://www.npmjs.com/package/dotenv)).

### Impostare NODE_ENV su 'production'

Possiamo rimuovere le tracce di stack nelle pagine di errore impostando la variabile d'ambiente `NODE_ENV` su _production_ (è impostata su '_development_' per impostazione predefinita). Oltre a generare messaggi di errore meno dettagliati, impostare la variabile su _production_ memorizza nella cache i modelli di visualizzazione e i file CSS generati dalle estensioni CSS. I test indicano che impostare `NODE_ENV` su _production_ può migliorare le prestazioni dell'app di un fattore tre!

Questa modifica può essere effettuata tramite `export`, con un file d'ambiente o il sistema di inizializzazione del sistema operativo.

> [!NOTE]
> Questa è in realtà una modifica che fate nella configurazione dell'ambiente piuttosto che nella vostra app, ma è abbastanza importante da meritare una nota qui! Mostreremo come impostarla nel nostro esempio di hosting qui sotto.

### Regolare il logging

Le chiamate di logging possono avere un impatto su un sito web molto trafficato. In un ambiente di produzione, potreste aver bisogno di registrare l'attività del sito web (ad esempio, monitorare il traffico o registrare chiamate API) ma dovreste cercare di ridurre al minimo la quantità di logging aggiunta per scopi di debug.

Un modo per minimizzare il logging "debug" in produzione è utilizzare un modulo come [debug](https://www.npmjs.com/package/debug) che permette di controllare quale logging viene eseguito impostando una variabile d'ambiente.
Ad esempio, il frammento di codice seguente mostra come potreste configurare il logging "author".
La variabile debug viene dichiarata con il nome 'author', e il prefisso "author" verrà visualizzato automaticamente per tutti i log provenienti da questo oggetto.

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

È quindi possibile abilitare un particolare set di log specificandoli come un elenco separato da virgole nella variabile d'ambiente `DEBUG`.
Potete impostare le variabili per visualizzare i log di author e book come mostrato (i caratteri jolly sono anche supportati).

```bash
#Windows
set DEBUG=author,book

#Linux
export DEBUG="author,book"
```

> [!NOTE]
> Le chiamate a `debug` possono sostituire il logging che potreste aver fatto utilizzando precedentemente `console.log()` o `console.error()`. Sostituite qualsiasi chiamata a `console.log()` nel vostro codice con logging attraverso il modulo [debug](https://www.npmjs.com/package/debug). Attivate o disattivate il logging nel vostro ambiente di sviluppo impostando la variabile DEBUG e osservate l'impatto che ciò ha sul logging.

Se avete bisogno di registrare l'attività del sito web potete utilizzare una libreria di logging come _Winston_ o _Bunyan_. Per ulteriori informazioni su questo argomento, vedere: [Migliori pratiche di produzione: performance e affidabilità](https://expressjs.com/en/advanced/best-practice-performance.html).

### Utilizzare la compressione gzip/deflate per le risposte

I server web possono spesso comprimere la risposta HTTP inviata al client, riducendo significativamente il tempo richiesto al client per ottenere e caricare la pagina. Il metodo di compressione utilizzato dipenderà dai metodi di decompressione che il client dichiara di supportare nella richiesta (la risposta sarà inviata senza compressione se non sono supportati metodi di compressione).

Aggiungete questo al sito usando il middleware [compression](https://www.npmjs.com/package/compression). Installatelo alla radice del vostro progetto eseguendo il seguente comando:

```bash
npm install compression
```

Aprite **./app.js** e richiedete la libreria di compressione come mostrato. Aggiungete la libreria di compressione alla catena del middleware con il metodo `use()` (questo dovrebbe apparire prima di qualsiasi rotta che desiderate comprimere — in questo caso, tutte!)

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
> Per un sito web ad alto traffico in produzione, non usereste questo middleware. Invece, usereste un proxy inverso come [Nginx](https://nginx.org/).

### Usare Helmet per proteggere da vulnerabilità note

[Helmet](https://www.npmjs.com/package/helmet) è un pacchetto middleware. Può impostare header HTTP appropriati che aiutano a proteggere la vostra app da vulnerabilità web note (vedere i [documenti](https://helmetjs.github.io/) per ulteriori informazioni su quali header imposta e sulle vulnerabilità che protegge).

Installatelo alla radice del vostro progetto eseguendo il seguente comando:

```bash
npm install helmet
```

Aprite **./app.js** e richiedete la libreria _helmet_ come mostrato.
Quindi aggiungete il modulo alla catena del middleware con il metodo `use()`.

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

Potremmo normalmente aver appena inserito `app.use(helmet());` per aggiungere il _sottoinsieme_ di header relativi alla sicurezza che ha senso per la maggior parte dei siti.
Tuttavia, nel [modello base LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/LocalLibrary_base_template), includiamo alcuni script bootstrap e jQuery.
Questi violano il _default_ [Content Security Policy (CSP)](/it/docs/Web/HTTP/Guides/CSP) di helmet, che non permette il caricamento di script cross-site.
Per consentire il caricamento di questi script, modifichiamo la configurazione di helmet in modo che imposti direttive CSP per consentire il caricamento degli script dai domini indicati.
Per il vostro server potreste aggiungere/disabilitare specifici header seguendo le [istruzioni per usare helmet qui](https://www.npmjs.com/package/helmet).

### Aggiungere il rate limiting alle rotte API

[Express-rate-limit](https://www.npmjs.com/package/express-rate-limit) è un pacchetto middleware che può essere utilizzato per limitare le richieste ripetute alle API e agli endpoint.
Ci sono molte ragioni per cui potrebbero essere fatte richieste eccessive al vostro sito, come attacchi denial of service, attacchi di forza bruta, o anche solo un client o uno script che non si comportano come previsto.
Oltre ai problemi di performance che possono derivare da troppe richieste che causano rallentamenti del vostro server, potreste anche essere addebitati per il traffico aggiuntivo.
Questo pacchetto può essere utilizzato per limitare il numero di richieste che possono essere fatte a una particolare rotta o a un insieme di rotte.

Installatelo alla radice del vostro progetto eseguendo il seguente comando:

```bash
npm install express-rate-limit
```

Aprite **./app.js** e richiedete la libreria _express-rate-limit_ come mostrato.
Quindi aggiungete il modulo alla catena del middleware con il metodo `use()`.

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

Il comando sopra limita tutte le richieste a 20 al minuto (è possibile modificarlo secondo necessità).

> [!NOTE]
> Servizi di terze parti come [Cloudflare](https://www.cloudflare.com/) possono anche essere usati se avete bisogno di protezione più avanzata contro attacchi denial of service o altri tipi di attacchi.

#### Impostare la versione di Node

Per le applicazioni Node, incluse Express, il file **package.json** contiene tutto ciò che un fornitore di hosting dovrebbe fare per determinare le dipendenze dell'applicazione e il file di punto di entrata.

L'unica informazione importante mancante dal nostro attuale **package.json** è la versione di Node richiesta dalla libreria.
Potrete trovare la versione di Node che è stata usata per lo sviluppo inserendo il comando:

```bash
>node --version
v16.17.1
```

Aprite **package.json**, e aggiungete questa informazione come **engines > node** come mostrato (usate il numero di versione per il vostro sistema).

```json
  "engines": {
    "node": ">=16.17.1"
  },
```

Il servizio di hosting potrebbe non supportare la versione specifica di Node indicata, ma questa modifica dovrebbe assicurare che tenti di usare una versione con lo stesso numero di versione principale o una versione più recente.

Notate che possono esserci altri modi per specificare la versione di Node su diversi servizi di hosting, ma l'approccio con **package.json** è ampiamente supportato.

#### Recuperare le dipendenze e rieseguire i test

Prima di procedere, proviamo di nuovo il sito e assicuriamoci che non sia stato influenzato da nessuna delle nostre modifiche.

Per prima cosa, dovremo ottenere le nostre dipendenze. È possibile farlo eseguendo il seguente comando nel terminale alla radice del progetto:

```bash
npm install
```

Ora eseguite nuovamente il sito (vedere [Testing the routes](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/routes#testing_the_routes) per i comandi pertinenti) e controllate che il sito si comporti ancora come previsto.

### Creazione di un repository applicativo su GitHub

Molti servizi di hosting consentono di importare e/o sincronizzare progetti da un repository locale o da piattaforme di controllo di versione del codice sorgente basate sul cloud.
Questo può rendere la distribuzione e lo sviluppo iterativo molto più semplice.

Per questo tutorial, creeremo un account [GitHub](https://github.com/) e un repository per la biblioteca, e utilizzeremo lo strumento **git** per caricare il nostro codice sorgente.

> [!NOTE]
> È possibile saltare questo passaggio se si utilizza già GitHub per gestire il codice sorgente!
>
> Notate che usare strumenti di gestione del codice sorgente è una buona pratica di sviluppo software, in quanto vi permette di provare modifiche, e passare tra i vostri esperimenti e "codice noto buono" quando ne avete bisogno!

I passaggi sono:

1. Visitate <https://github.com/> e create un account.
2. Una volta effettuato l'accesso, cliccate sul link **+** nella barra degli strumenti in alto e selezionate **New repository**.
3. Compilate tutti i campi in questo modulo. Anche se non sono obbligatori, sono fortemente consigliati.

   - Inserite un nuovo nome per il repository (ad esempio, _express-locallibrary-tutorial_) e una descrizione (ad esempio, "Local Library website scritto in Express (Node)").
   - Scegliete **Node** nell'elenco di selezione _Add .gitignore_.
   - Scegliete la vostra licenza preferita nell'elenco di selezione _Add license_.
   - Selezionate **Initialize this repository with a README**.

   > [!WARNING]
   > L'accesso "Pubblico" predefinito renderà _tutto_ il codice sorgente — inclusi nome utente e password del database — visibili a chiunque su internet! Assicuratevi che il codice sorgente legga le credenziali _solo_ da variabili d'ambiente e non abbia credenziali hard-coded.
   >
   > In caso contrario, selezionate l'opzione "Privato" per consentire solo a persone selezionate di vedere il codice sorgente.

4. Premete **Create repository**.
5. Cliccate sul pulsante verde **Clone or download** sulla pagina del nuovo repo.
6. Copiate il valore dell'URL dalla casella di testo all'interno della finestra di dialogo che appare.
   Se avete usato il nome del repository "express-locallibrary-tutorial", l'URL dovrebbe essere simile a: `https://github.com/<your_git_user_id>/express-locallibrary-tutorial.git`.

Ora che il repository ("repo") è stato creato su GitHub, vogliamo clonarlo (copiarlo) sul nostro computer locale:

1. Installate _git_ per il vostro computer locale ([guida ufficiale al download di Git](https://git-scm.com/downloads)).
2. Aprite un prompt dei comandi/terminale e clonate il vostro repo utilizzando l'URL che avete copiato sopra:

   ```bash
   git clone https://github.com/<your_git_user_id>/express-locallibrary-tutorial.git
   ```

   Questo creerà il repository all'interno della directory corrente.

3. Navigate nella cartella del repo.

   ```bash
   cd express-locallibrary-tutorial
   ```

Quindi copiate i vostri file sorgente dell'applicazione nella cartella del repo, rendeteli parte del repo utilizzando _git_, e caricateli su GitHub:

1. Copiate la vostra applicazione Express in questa cartella (escludendo **/node_modules**, che contiene i file di dipendenza che dovreste recuperare da npm secondo necessità).
2. Aprite un prompt dei comandi/terminale e utilizzate il comando `add` per aggiungere tutti i file a git.

   ```bash
   git add -A
   ```

3. Usate il comando `status` per verificare che tutti i file che state per `commit` siano corretti (desiderate includere i file sorgente, non i file binari, temporanei ecc.).
   Dovrebbe sembrare un po' come l'elenco sottostante.

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

4. Quando siete soddisfatti, `commit` gli file nel vostro repo locale.
   Questo equivale a firmare le modifiche e renderle una parte ufficiale del repo locale.

   ```bash
   git commit -m "First version of application moved into GitHub"
   ```

5. A questo punto, il repo remoto non è stato modificato.
   L'ultimo passo è sincronizzare (`push`) il vostro repo locale verso il repo remoto su GitHub utilizzando il seguente comando:

   ```bash
   git push origin main
   ```

Quando questa operazione è completata, dovreste essere in grado di tornare alla pagina su GitHub dove avete creato il vostro repo, aggiornare la pagina, e vedere che la vostra intera applicazione è stata ora caricata. Potete continuare a aggiornare il vostro repo mentre i file cambiano usando questo ciclo add/commit/push.

Questo è un buon momento per fare un backup del vostro progetto "vanilla" — mentre alcune delle modifiche che faremo nelle sezioni seguenti potrebbero essere utili per la distribuzione su qualsiasi servizio di hosting (o per lo sviluppo), altre potrebbero non esserlo.
Potete farlo utilizzando `git` dalla riga di comando:

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
> Per saperne di più, vedere [Learning Git](https://docs.github.com/en/get-started/start-your-journey/git-and-github-learning-resources).

## Esempio: Hosting su Glitch

Questa sezione offre una dimostrazione pratica di come ospitare _LocalLibrary_ su [Glitch](https://glitch.com/).

### Perché Glitch?

Abbiamo scelto di utilizzare Glitch per diversi motivi:

- Glitch ha un [piano iniziale gratuito](https://glitch.com/pricing) che è _davvero_ gratuito, anche se con alcune limitazioni.
  Il fatto che sia accessibile a tutti gli sviluppatori è davvero importante per MDN!
- Glitch si prende cura dell'infrastruttura in modo che voi non dobbiate preoccuparvi.
  Non dover pensare ai server, bilanciatori del carico, proxy inversi e così via, rende molto più facile iniziare.
- Le competenze e i concetti che imparerete utilizzando Glitch sono trasferibili.
- Le limitazioni del servizio e del piano non influiscono davvero sull'uso di Glitch per il tutorial.
  Ad esempio:

  - Il piano iniziale offre solo 1000 "ore di progetto" al mese, che vengono reimpostate mensilmente.
    Questo viene utilizzato quando si sta modificando attivamente il sito o se qualcuno lo sta accedendo.
    Se nessuno sta accedendo o modificando il sito, questo andrà in stand-by.
  - L'ambiente del piano iniziale ha una quantità limitata di RAM del container e spazio di archiviazione.
    C'è più che sufficiente per il tutorial, in particolare perché il nostro database è ospitato altrove.
  - I domini personalizzati non sono ben supportati (al momento della scrittura).
  - Altre limitazioni possono essere trovate nella [pagina di restrizioni tecniche di Glitch](https://help.glitch.com/s/article/Technical-Restrictions).

Mentre Glitch è appropriato per ospitare questa dimostrazione, dovreste prendervi il tempo necessario per determinare se sia [adatto per il vostro sito web](#scegliere_un_fornitore_di_hosting).

### Come funziona Glitch?

Glitch fornisce un'interfaccia web in cui puoi creare progetti da modelli iniziali, oppure importarli da GitHub, e poi aggiungere e modificare i file del progetto.
Man mano che si fanno modifiche, il progetto viene costruito ed eseguito in un proprio container virtualizzato e isolato.

Come tutto ciò funzioni "sotto il cofano" è un mistero — Glitch non lo dice.
Ciò che è chiaro è che finché crei un'applicazione web Node.js abbastanza standard (ad esempio, utilizzando `package.json` per le tue dipendenze) e non consumi più risorse di quanto elencato nelle [restrizioni tecniche](https://help.glitch.com/s/article/Technical-Restrictions), la tua applicazione dovrebbe "funzionare semplicemente".

Una volta che l'applicazione è in esecuzione, può essere configurata per la produzione utilizzando [dati privati](https://help.glitch.com/s/article/Adding-Private-Data) forniti in un file `.env`.
I valori nei dati segreti vengono letti dall'applicazione come variabili d'ambiente, che, come ricorderete da una sezione precedente, è il modo in cui abbiamo configurato la nostra applicazione per ottenere l'URL del database.
Notate che le variabili sono _segrete_: il `.env` non dovrebbe essere incluso nel vostro repository GitHub.

La visualizzazione di editing di Glitch fornisce anche un accesso _terminale_ all'ambiente dell'app web, che potete usare per lavorare con l'app web come se fosse in esecuzione sul vostro computer locale.

Questo è tutto ciò che è necessario sapere per iniziare.
Successivamente, ci occuperemo di configurare un account Glitch, caricare il progetto della Library da GitHub e connetterlo a un database.

### Ottenere un account Glitch

Per iniziare a utilizzare Glitch, sarà necessario prima creare un account:

- Vai a [glitch.com](https://glitch.com/) e clicca sul pulsante **Sign up** nella barra degli strumenti in alto.
- Seleziona GitHub nel popup per registrarti utilizzando le tue credenziali GitHub.
- Sarai quindi registrato e connesso nella dashboard di Glitch: <https://glitch.com/dashboard>.

### Risoluzione problemi versione Node.js

I fornitori di hosting supportano comunemente alcune versioni principali delle versioni recenti di Node.js.
Se la "versione minore" esatta specificata nel tuo file `package.json` non è supportata, di solito si ritireranno sulla versione più vicina che supportano (e spesso questo funzionerà).

Sfortunatamente, al momento della scrittura, la versione più alta supportata su Glitch è Node.js 16.
Se avete sviluppato con Node.js 17 o versione successiva, dovreste ridurre la versione usata nel vostro file `package.json` come mostrato.
Dovrete anche testare nuovamente:

```json
  "engines": {
    "node": ">=v16"
  },
```

Glitch [pianifica di aggiornare Node e mantenerlo meglio aggiornato in futuro](https://blog.glitch.com/post/rebuilding-glitch/) — e potrebbe essere che quando leggerete questo la limitazione sulla versione non esista più.
Invece di eseguire il downgrade della versione di `node`, potete caricare il vostro progetto per vedere se si compila.
Se ci sono errori e l'applicazione non si carica, dovreste provare a impostare la versione di `node` su `>=v16` direttamente nel vostro `package.json` nell'editor Glitch.

> [!NOTE]
> Potete anche controllare le versioni supportate inserendo il seguente comando nel terminale di qualsiasi progetto Glitch:
>
> ```sh
> ls -l /opt/nvm/versions/node | grep '^d' | awk '{ print $9 }'
> ```

### Distribuire su Glitch da GitHub

Ora importeremo il progetto della Library da GitHub.
Prima scegliete l'opzione **Dashboard** dal menu in alto del sito, quindi selezionate il pulsante **New project**.
Glitch mostrerà un elenco di opzioni per il nuovo progetto; selezionate **Import from GitHub**.

![Dashboard del sito Glitch che mostra un pulsante nuovo progetto e un menu a comparsa con l'opzione "Import from GitHub"](glitch_new_project_import_github.png)

Apparirà un popup.
Immettete l'URL del vostro repository della Library GitHub nel popup e premete **OK**.
Di seguito abbiamo inserito il repo per il progetto lavorato.

![Popup Glitch per l'inserimento dell'URL del repository GitHub da importare](glitch_new_project_github_repo_url.png)

Glitch importerà quindi il progetto, mostrando notifiche di progresso.
Al termine, mostrerà la vista di editing per il nuovo progetto, come mostrato di seguito.

![Vista editor Glitch per progetto importato](glitch_imported_project_in_editor.png)

Potete ottenere l'URL del sito live selezionando il pulsante **Share**.

![Vista editor Glitch per progetto importato](glitch_share_project.png)

Aprite una nuova scheda del browser e copiate il collegamento per il sito live nella barra degli indirizzi.
Il sito della biblioteca locale dovrebbe aprirsi e visualizzare i dati dal database di sviluppo.

> [!NOTE]
> Questo processo è stato un'importazione una tantum da GitHub.
> Potete anche utilizzare azioni GitHub come [glitch-project-sync](https://github.com/marketplace/actions/glitch-project-sync) per mantenere Glitch e > il vostro progetto sincronizzati.

### Usare un database MongoDB di produzione

Dovreste impostare un database diverso per la produzione rispetto allo sviluppo.
Mentre Glitch ospita solo database SQLite (e noi siamo impostati per usare MongoDB), molti altri siti forniscono database MongoDB come servizio.

Una opzione è quella di seguire le istruzioni di [Impostazione del database MongoDB](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose#setting_up_the_mongodb_database) da una sezione precedente del tutorial per impostare un nuovo database di produzione.

Per rendere accessibile il database di produzione all'applicazione della biblioteca, aprite il file `.env` nella vista editor per il progetto.
Inserite la variabile dell'URL del database `MONGODB_URI` e l'URL del vostro database di produzione.
Il sito si aggiorna mentre immettete i valori nell'editor.

![Editor del file .env di Glitch per dati privati con variabili di produzione](glitch_env.png)

> [!NOTE]
> Non abbiamo creato questo file.
> È destinato a [dati privati](https://help.glitch.com/s/article/Adding-Private-Data) ed è stato creato automaticamente all'importazione su Glitch.
> Non viene mai esportato o copiato.

### Altre variabili di configurazione

Ricorderete da una sezione precedente che dobbiamo [impostare NODE_ENV su 'production'](#set_node_env_to_production) per migliorare le prestazioni e generare messaggi di errore meno dettagliati. Facciamo questo nello stesso file in cui impostiamo la variabile `MONGODB_URI`.

Aprite `.env` e aggiungete una variabile `NODE_ENV` con valore `production` (vedere lo screenshot nella sezione precedente).

L'applicazione della biblioteca locale è ora impostata e configurata per l'uso in produzione.
Potete aggiungere dati tramite l'interfaccia del sito web, e dovrebbe funzionare come faceva durante lo sviluppo (anche se con meno informazioni di debug esposte per pagine non valide).

> [!NOTE]
> Se desiderate solo aggiungere alcuni dati per il test, potreste utilizzare lo script `populatedb` (con il vostro URL del database di produzione MongoDB) come discusso nella sezione [Express Tutorial Parte 3: Usare un Database (con Mongoose) Collaudo — creare alcuni oggetti](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose#testing_%E2%80%94_create_some_items).

### Debugging di app Express su Glitch

Glitch consente un debug efficace.
Alcune delle cose che potete fare sono:

- Selezionare il pulsante dei log in fondo alla vista dell'editor per vedere le informazioni di log dal server, come l'output del log della console.
- Selezionare il pulsante del terminale in fondo alla vista dell'editor per aprire un terminale nell'ambiente di hosting.
  Potete usare questo per eseguire comandi e strumenti nell'ambiente.
  Ad esempio, potete usare `node -v` per controllare la versione di Node.
- Debugging interattivo in VS Code utilizzando l'estensione GLITCH per VS Code.

## Esempio: Hosting su Railway

Questa sezione offre una dimostrazione pratica di come installare _LocalLibrary_ su [Railway](https://railway.com/).

### Perché Railway?

> [!WARNING]
> Railway non ha più un livello di partenza completamente gratuito.
> Abbiamo mantenuto queste istruzioni perché Railway ha alcune ottime funzionalità, e sarà una scelta migliore per alcuni utenti.

Railway è un'opzione di hosting interessante per diversi motivi:

- Railway si occupa della maggior parte dell'infrastruttura in modo che voi non dobbiate preoccuparvi.
  Non dover pensare a server, bilanciatori del carico, proxy inversi e così via, rende molto più facile iniziare.
- Railway ha un [focus sull'esperienza di sviluppo per lo sviluppo e il deployment](https://docs.railway.com/maturity/compare-to-heroku), che porta a una curva di apprendimento più rapida e morbida rispetto a molte altre alternative.
- Le competenze e i concetti che imparerete utilizzando Railway sono trasferibili.
  Sebbene Railway abbia alcune eccellenti nuove funzionalità, altri servizi di hosting popolari utilizzano molte delle stesse idee e approcci.
- La [documentazione di Railway](https://docs.railway.com/) è chiara e completa.
- Il servizio sembra essere molto affidabile, e se finisci per amarlo, la tariffazione è prevedibile, e scalare la tua app è molto semplice.

Dovreste prendervi il tempo necessario per determinare se Railway sia [adatto per il vostro sito web](#scegliere_un_fornitore_di_hosting).

### Come funziona Railway?

Le applicazioni web vengono eseguite ciascuna nel proprio container virtuale indipendente e isolato.
Per eseguire la vostra applicazione, Railway deve essere in grado di impostare l'ambiente e le dipendenze appropriate e anche capire come viene avviata.

Railway rende questo facile, poiché è in grado di riconoscere e installare automaticamente molti diversi framework e ambienti di applicazioni web basati sul loro uso di "convenzioni comuni".
Ad esempio, Railway riconosce le applicazioni Node perché hanno un file **package.json**, e può determinare il gestore dei pacchetti usato per la creazione dal file "lock".
Ad esempio, se l'applicazione include il file **package-lock.json**, Railway sa di dover usare _npm_ per installare i pacchetti, mentre se trova **yarn.lock** sa di dover usare _yarn_.
Avendo installato tutte le dipendenze, Railway cercherà script chiamati "build" e "start" nel file del pacchetto e userà questi per costruire ed eseguire il codice.

> [!NOTE]
> Railway utilizza [Nixpacks](https://nixpacks.com/docs) per riconoscere vari framework di applicazioni web scritti in diversi linguaggi di programmazione.
> Non avete bisogno di sapere altro per questo tutorial, ma potete trovare maggiori informazioni sulle opzioni per distribuire applicazioni Node in [Nixpacks Node](https://nixpacks.com/docs/providers/node).

Una volta che l'applicazione è in esecuzione, può configurarsi utilizzando le informazioni fornite nelle [variabili d'ambiente](https://docs.railway.com/guides/variables).
Ad esempio, un'applicazione che utilizza un database deve ottenere l'indirizzo utilizzando una variabile.
Il servizio del database stesso può essere ospitato da Railway o da un altro fornitore.

Gli sviluppatori interagiscono con Railway attraverso il sito Railway, e utilizzando uno speciale strumento [Interface a Riga di Comando (CLI)](https://docs.railway.com/guides/cli).
Il CLI vi permette di associare un repository GitHub locale con un progetto Railway, caricare il repository dal branch locale al sito live, ispezionare i log del processo in esecuzione, impostare e ottenere variabili di configurazione e molto altro.
Una delle funzionalità più utili è che potete usare il CLI per eseguire il vostro progetto locale con le stesse variabili d'ambiente del progetto live.

Questa è tutta la panoramica di cui avete bisogno per distribuire l'app su Railway.
Successivamente configureremo un account Railway, installeremo il nostro sito web e un database, e proveremo il client Railway.

### Ottenere un account Railway

Per iniziare a utilizzare Railway, sarà necessario prima creare un account:

- Vai a [railway.com](https://railway.com/) e clicca sul link **Login** nella barra degli strumenti in alto.
- Seleziona GitHub nel popup per accedere utilizzando le tue credenziali GitHub.
- Potreste dover andare alla vostra email e verificare il vostro account.
- Sarai quindi connesso nella dashboard di Railway.com: <https://railway.com/dashboard>.

### Distribuire su Railway da GitHub

Ora configuriamo Railway per distribuire la nostra library da GitHub.
Prima scegliete l'opzione **Dashboard** dal menu in alto del sito, quindi selezionate il pulsante **New Project**:

![Dashboard del sito Railway che mostra il pulsante nuovo progetto](railway_new_project_button.png)

Railway mostrerà un elenco di opzioni per il nuovo progetto, inclusa l'opzione di distribuire un progetto da un modello che viene creato prima nel vostro account GitHub, e numerosi database.
Selezionate **Deploy from GitHub repo**.

![Popup Railway che mostra le opzioni di distribuzione con l'opzione "Deploy from GitHub repo" evidenziata](railway_new_project_button_deploy_github_repo.png)

Tutti i progetti nei repository GitHub che avete condiviso con Railway durante la configurazione vengono visualizzati.
Selezionate il vostro repository GitHub per la local library: `<user-name>/express-locallibrary-tutorial`.

![Popup Railway che mostra i repository GitHub che possono essere distribuiti](railway_new_project_button_deploy_github_selectrepo.png)

Confermate la vostra distribuzione selezionando **Deploy Now**.

![Schermata di conferma in cui puoi selezionare la distribuzione del progetto](railway_new_project_deploy_confirm.png)

Railway caricherà e distribuirà quindi il vostro progetto, mostrando i progressi nella scheda Deployment.
Quando la distribuzione viene completata con successo, vedrete una schermata come quella qui sotto.

![Dashboard Railway che mostra la scheda Deployment per il progetto distribuito](railway_project_deploy.png)

Ora selezionate la scheda _Settings_, quindi scorrete verso il basso fino alla sezione Domains, e premete il pulsante **Generate Domain**.

![Scheda delle impostazioni del progetto Railway che mostra il pulsante per generare un dominio](railway_project_generate_domain.png)

Questo pubblicherà il sito e metterà il dominio al posto del pulsante, come mostrato di seguito.

![Scheda delle impostazioni del progetto Railway che mostra un link al sito della biblioteca locale](railway_project_domain.png)

Selezionate l'URL del dominio per aprire la vostra applicazione library.
Notate che, poiché non abbiamo specificato un database di produzione, la biblioteca locale si aprirà utilizzando i vostri dati di sviluppo.

### Creazione e connessione di un database MongoDB

Invece di usare i nostri dati di sviluppo, creiamo un database MongoDB di produzione da utilizzare.
Creeremo il database come parte del progetto dell'applicazione Railway, anche se niente vieta di crearlo nel proprio progetto separato o di usare un database _MongoDB Atlas_ per i dati di produzione, proprio come avete fatto per il database di sviluppo.

Su Railway, scegliete l'opzione **Dashboard** dal menu in alto del sito e quindi selezionate il vostro progetto dell'applicazione.
A questo punto contiene solo un servizio per la vostra applicazione (questo può essere selezionato per impostare variabili e altri dettagli del servizio).
Selezionate il pulsante **New**, che viene usato per aggiungere servizi al progetto corrente.

![Progetto Railway con il pulsante nuovo servizio evidenziato](railway_project_open_no_database.png)

Selezionate **Database** quando vi viene chiesto del tipo di servizio da aggiungere:

![Popup Railway che mostra le opzioni per un nuovo servizio, come database, repository GitHub, servizio vuoto ecc.](railway_database_add.png)

Quindi selezionate **Add MongoDB** per iniziare ad aggiungere il database.

![Popup Railway che mostra i diversi database che possono essere selezionati: Postgres, MySQL, MongoDB e così via](railway_database_select_type.png)

Railway provvederà quindi a un servizio contenente un database vuoto nello stesso progetto.
Al termine, vedrete ora sia i servizi dell'applicazione che del database nella visualizzazione del progetto.

![Progetto Railway con servizi dell'applicazione e del database](railway_project_two_services.png)

Selezionate il servizio MongoDB per visualizzare le informazioni sul database.
Aprite la scheda _Variables_ e copiate il "Mongo_URL" (questo è l'indirizzo del database).

![Schermata delle impostazioni del database Railway che mostra l'URL necessario per connettersi al database](railway_mongodb_connect.png)

Per renderlo accessibile all'applicazione library, dobbiamo aggiungerlo al processo dell'applicazione utilizzando una variabile d'ambiente.
Prima di tutto aprite il servizio dell'applicazione.
Quindi selezionate la scheda _Variables_ e premete il pulsante **New Variable**.

Immettete il nome della variabile `MONGODB_URI` e l'URL di connessione che avete copiato per il database (`MONGODB_URI` è il nome della variabile d'ambiente da cui [abbiamo configurato l'applicazione](#configurazione_del_database) per leggere l'indirizzo del database).
Questo apparirà simile alla schermata mostrata qui sotto.

![Schermata delle variabili del sito Railway mentre si aggiunge la variabile MONGODB_URI e l'indirizzo](railway_variables_database_url.png)

Selezionate **Add** per aggiungere la variabile.

Railway riavvia la tua app quando aggiorna le variabili. Se controllate la home page ora dovrebbe mostrare zero valori per i conteggi degli oggetti, poiché le modifiche sopra significano che stiamo ora utilizzando un nuovo database (vuoto).

### Altre variabili di configurazione

Ricorderete da una sezione precedente che dobbiamo [impostare NODE_ENV su 'production'](#set_node_env_to_production) per migliorare le nostre prestazioni e generare messaggi di errore meno dettagliati. Possiamo farlo nella stessa schermata in cui abbiamo impostato la variabile `MONGODB_URI`.

Aprite il servizio dell'applicazione.
Quindi selezionate la scheda _Variables_, dove vedrete che `MONGODB_URI` è già definita, e premete il pulsante **New Variable**.

![Scheda delle variabili Railway con il pulsante Nuova variabile evidenziato](railway_variables_new.png)

Immettete `NODE_ENV` come nome della nuova variabile e `production` come nome dell'ambiente.
Quindi premete il pulsante **Add**.

![Scheda delle variabili Railway con nuova variabile NODE_ENV impostata su 'production'](railway_variables_new_node_env.png)

L'applicazione library locale è ora impostata e configurata per l'uso in produzione.
Potete aggiungere dati tramite l'interfaccia del sito web e dovrebbe funzionare allo stesso modo di quando era in fase di sviluppo (anche se con meno informazioni di debug esposte per pagine non valide).

> [!NOTE]
> Se desiderate solo aggiungere alcuni dati per il test, potreste utilizzare lo script `populatedb` (con il vostro URL del database di produzione MongoDB) come discusso nella sezione [Express Tutorial Parte 3: Usare un Database (con Mongoose) Testing — creare alcuni oggetti](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose#testing_%E2%80%94_create_some_items).

### Installare il client

Scaricate e installate il client Railway per il sistema operativo locale seguendo le [istruzioni qui](https://docs.railway.com/guides/cli).

Dopo che il client è stato installato, sarete in grado di eseguire comandi.
Alcune delle operazioni più importanti includono distribuire la directory corrente del vostro computer a un progetto Railway associato (senza dover caricare su GitHub), ed eseguire il vostro progetto localmente utilizzando le stesse impostazioni che avete sul server di produzione.

Potete ottenere un elenco di tutti i comandi possibili inserendo il seguente comando in un terminale.

```bash
railway help
```

### Debugging

Il client Railway fornisce il comando logs per mostrare l'ultimo dei log (un log più completo è disponibile sul sito per ogni progetto):

```bash
railway logs
```

## Riepilogo

Questo è la fine di questo tutorial su come impostare app Express in produzione, e anche della serie di tutorial sul lavoro con Express. Speriamo che li abbiate trovati utili. Potete consultare una versione completamente lavorata del [codice sorgente su GitHub qui](https://github.com/mdn/express-locallibrary-tutorial).

## Vedi anche

- [Migliori pratiche di produzione: performance e affidabilità](https://expressjs.com/en/advanced/best-practice-performance.html) (Express docs)
- [Migliori pratiche di produzione: Sicurezza](https://expressjs.com/en/advanced/best-practice-security.html) (Express docs)
- Documenti di Railway

  - [CLI](https://docs.railway.com/guides/cli)

- DigitalOcean

  - Tutorial [Express](https://www.digitalocean.com/community/tutorials?q=express)
  - Tutorial [Node.js](https://www.digitalocean.com/community/tutorials?q=node.js)

- Heroku

  - [Introduzione a Heroku con Node.js](https://devcenter.heroku.com/articles/getting-started-with-nodejs) (Heroku docs)
  - [Distribuire Applicazioni Node.js su Heroku](https://devcenter.heroku.com/articles/deploying-nodejs) (Heroku docs)
  - [Supporto Node.js di Heroku](https://devcenter.heroku.com/articles/nodejs-support) (Heroku docs)
  - [Ottimizzazione della Concorrenza delle Applicazioni Node.js](https://devcenter.heroku.com/articles/node-concurrency) (Heroku docs)
  - [Come funziona Heroku](https://devcenter.heroku.com/articles/how-heroku-works) (Heroku docs)
  - [Dynos e il Gestore dei Dyno](https://devcenter.heroku.com/articles/dynos) (Heroku docs)
  - [Configurazione e Config Vars](https://devcenter.heroku.com/articles/config-vars) (Heroku docs)
  - [Limiti](https://devcenter.heroku.com/articles/limits) (Heroku docs)

{{PreviousMenu("Learn_web_development/Extensions/Server-side/Express_Nodejs/forms", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}
