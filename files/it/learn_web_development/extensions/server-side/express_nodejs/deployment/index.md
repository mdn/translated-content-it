---
title: "Guida al tutorial su Express Parte 7: Distribuzione in produzione"
short-title: "7: Distribuzione"
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/deployment
l10n:
  sourceCommit: 2c0f972d873ea2db5163dbcb12987847124751ad
---

{{PreviousMenu("Learn_web_development/Extensions/Server-side/Express_Nodejs/forms", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}

Ora che hai creato (e testato) un fantastico sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website), probabilmente vorrai installarlo su un server web pubblico in modo che possa essere accessibile dallo staff e dai membri della biblioteca tramite Internet. Questo articolo fornisce una panoramica su come potresti procedere per trovare un host per distribuire il tuo sito web e cosa devi fare per preparare il tuo sito per la produzione.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Completa tutti i precedenti argomenti del tutorial, inclusa la <a href="/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms">Guida al tutorial su Express Parte 6: Lavorare con i form</a>.
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

Una volta che il tuo sito è finito (o abbastanza "finito" da iniziare il test pubblico), dovrai ospitarlo in un luogo più pubblico e accessibile del tuo computer di sviluppo personale.

Fino ad ora, hai lavorato in un [ambiente di sviluppo](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment), utilizzando Express/Node come server web per condividere il tuo sito sul browser/rete locale e eseguendo il tuo sito web con impostazioni di sviluppo (insicure) che espongono il debugging e altre informazioni private. Prima di poter ospitare un sito web esternamente, dovrai innanzitutto:

- Scegliere un ambiente per ospitare l'app Express.
- Apportare alcune modifiche alle impostazioni del tuo progetto.
- Configurare un'infrastruttura a livello di produzione per servire il tuo sito web.

Questo tutorial fornisce alcune indicazioni sulle opzioni per scegliere un sito di hosting, una breve panoramica su ciò che devi fare per preparare il tuo sito Express per la produzione e un esempio pratico di come installare il sito LocalLibrary sul servizio di cloud hosting [Railway](https://railway.com/).

## Cos'è un ambiente di produzione?

L'ambiente di produzione è l'ambiente fornito dal computer server dove eseguirai il tuo sito web per il consumo esterno. L'ambiente include:

- L'hardware del computer su cui gira il sito web.
- Sistema operativo (ad esempio, Linux o Windows).
- Runtime del linguaggio di programmazione e librerie del framework su cui è scritto il tuo sito web.
- Infrastruttura del server web, possibilmente incluso un server web, proxy inverso, bilanciatore di carico, ecc.
- Database da cui dipende il tuo sito web.

Il computer server potrebbe essere situato nei tuoi locali e connesso a Internet attraverso un link veloce, ma è molto più comune utilizzare un computer ospitato "nel cloud". Ciò significa che il tuo codice viene eseguito su un computer remoto (o possibilmente un computer "virtuale") nei data center della tua azienda di hosting. Il server remoto offrirà solitamente un livello garantito di risorse di calcolo (ad esempio, CPU, RAM, memoria di archiviazione, ecc.) e connettività Internet per un determinato prezzo.

Questo tipo di hardware per il computing/rete accessibile a distanza viene denominato _Infrastructure as a Service (IaaS)_. Molti fornitori IaaS offrono opzioni per preinstallare un particolare sistema operativo, sul quale devi installare gli altri componenti del tuo ambiente di produzione. Altri fornitori ti permettono di selezionare ambienti più completi, forse includendo un intero set Node.

> [!NOTE]
> Gli ambienti preconfigurati possono facilitare molto la configurazione del tuo sito web perché riducono la configurazione, ma le opzioni disponibili potrebbero limitarti a un server (o altri componenti) non familiare e potrebbero basarsi su una versione più vecchia del sistema operativo. Spesso è meglio installare i componenti da soli in modo da ottenere quelli che vuoi, e quando hai bisogno di aggiornare parti del sistema, hai un'idea da dove iniziare!

Altri fornitori di hosting supportano Express come parte di un'offerta _Platform as a Service_ (_PaaS_). Quando si usa questo tipo di hosting non è necessario preoccuparsi della maggior parte del tuo ambiente di produzione (server, bilanciatori di carico, ecc.) poiché la piattaforma host si occupa di questi per te. Questo rende la distribuzione abbastanza semplice perché puoi concentrarti solo sulla tua applicazione web e non su qualsiasi altra infrastruttura del server.

Alcuni sviluppatori sceglieranno la maggiore flessibilità fornita da IaaS rispetto a PaaS, mentre altri apprezzeranno la minore manutenzione e la scalabilità più facile di PaaS. Quando inizi, configurare il tuo sito web su un sistema PaaS è molto più facile, quindi è quello che faremo in questo tutorial.

> [!NOTE]
> Se scegli un fornitore di hosting compatibile con Node/Express, dovrebbe fornire istruzioni su come impostare un sito web Express utilizzando diverse configurazioni di server web, server applicativo, proxy inverso, ecc. Ad esempio, ci sono molte guide passo passo per varie configurazioni nei [documenti della community di DigitalOcean Node](https://www.digitalocean.com/community/tutorials?q=node).

## Scegliere un fornitore di hosting

Ci sono numerosi fornitori di hosting che sono noti per supportare attivamente o funzionare bene con _Node_ (e _Express_). Questi fornitori offrono diversi tipi di ambienti (IaaS, PaaS), e diversi livelli di risorse di computing e rete a diversi prezzi.

> [!NOTE]
> Ci sono molte soluzioni di hosting, e i loro servizi e prezzi possono cambiare nel tempo. Mentre introduciamo alcune opzioni di seguito, vale la pena controllare sia queste che altre opzioni prima di selezionare un fornitore di hosting.

Alcune delle cose da considerare quando si sceglie un host:

- Quanto è probabile che sia visitato il tuo sito e il costo dei dati e delle risorse di computing richieste per soddisfare quella domanda.
- Livello di supporto per la scalabilità orizzontale (aggiungendo più macchine) e verticale (aggiornamento a macchine più potenti) e i costi per farlo.
- Le località dove il fornitore ha data center, e quindi dove l'accesso è probabilmente più veloce.
- Le prestazioni storiche di uptime e downtime dell'host.
- Strumenti forniti per gestire il sito - sono facili da usare e sono sicuri (ad esempio, SFTP vs. FTP).
- Framework integrati per monitorare il tuo server.
- Limitazioni conosciute. Alcuni host bloccheranno deliberatamente certi servizi (ad esempio, email). Altri offrono solo un certo numero di ore di "live time" in alcune fasce di prezzo o offrono solo una piccola quantità di spazio di archiviazione.
- Benefici aggiuntivi. Alcuni fornitori offriranno nomi di dominio gratuiti e supporto per certificati TLS che altrimenti dovresti pagare.
- Se il tier "gratuito" su cui stai facendo affidamento scade nel tempo e se il costo di passare a un tier più costoso significa che sarebbe stato meglio usare un altro servizio dall'inizio!

La buona notizia quando si inizia è che ci sono parecchi siti che forniscono ambienti di computing "gratuiti" che sono destinati alla valutazione e al testing.
Di solito questi sono ambienti con risorse limitate e devi essere consapevole che potrebbero scadere dopo un certo periodo introduttivo o avere altre limitazioni.
Tuttavia, sono ottimi per testare siti a basso traffico in un ambiente ospitato e possono fornire una facile migrazione per pagare più risorse quando il tuo sito diventa più frequentato.
Le scelte popolari in questa categoria includono [Glitch](https://glitch.com/), [Python Anywhere](https://www.pythonanywhere.com/), [Amazon Web Services](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/billing-free-tier.html), [Microsoft Azure](https://azure.microsoft.com/en-us/pricing/details/app-service/linux/), ecc.

La maggior parte degli host offre anche un tier "base" o "hobby" destinato a piccoli siti di produzione, che fornisce livelli più utili di potenza di calcolo e meno limitazioni.
[Railway](https://railway.com/), [Heroku](https://www.heroku.com/), [DigitalOcean](https://www.digitalocean.com/) e [Python Anywhere](https://www.pythonanywhere.com/) sono esempi di fornitori di hosting popolari che hanno un tier di calcolo base relativamente economico (nella fascia di 5-10 USD al mese).

> [!NOTE]
> Ricorda che il prezzo non è l'unico criterio di selezione.
> Se il tuo sito web ha successo, potrebbe risultare che la scalabilità è la considerazione più importante.

## Preparare il sito web per la pubblicazione

Le principali cose a cui pensare quando si pubblica il tuo sito web sono la sicurezza e le prestazioni del web.
Al minimo indispensabile, vuoi modificare la configurazione del database in modo da poter utilizzare un database diverso per la produzione e proteggere le sue credenziali, rimuovere le tracce dello stack che sono incluse nelle pagine di errore durante lo sviluppo, sistemare i tuoi log e impostare le intestazioni appropriate per evitare molte minacce comuni alla sicurezza.

Nelle sezioni seguenti, delineiamo i cambiamenti più importanti che dovresti apportare alla tua app.

> [!NOTE]
> Ci sono altri suggerimenti utili nei documenti di Express — vedi [Migliori pratiche di produzione: prestazioni e affidabilità](https://expressjs.com/en/advanced/best-practice-performance.html) e [Migliori pratiche di produzione: Sicurezza](https://expressjs.com/en/advanced/best-practice-security.html).

### Configurazione del database

Fino a questo punto del tutorial, abbiamo usato un unico database di sviluppo, per il quale l'indirizzo e le credenziali sono codificati in **app.js**.
Poiché il database di sviluppo non contiene informazioni che temiamo possano essere esposte o corrotte, non c'è alcun rischio particolare nel far trapelare questi dettagli.
Tuttavia, se lavorate con dati reali, in particolare informazioni personali degli utenti, proteggere le credenziali del database è molto importante.

Per questo motivo vogliamo usare un database diverso per la produzione rispetto a quello usato per lo sviluppo, e inoltre mantenere le credenziali del database di produzione separate dal codice sorgente in modo che possano essere adeguatamente protette.

Se il tuo fornitore di hosting supporta l'impostazione di variabili d'ambiente tramite un'interfaccia web (come fanno molti), un modo per farlo è far ottenere al server l'URL del database da una variabile d'ambiente.
Di seguito, modifichiamo il sito LocalLibrary per ottenere l'URI del database da una variabile d'ambiente del sistema operativo, se è stata definita, e altrimenti utilizziamo l'URL del database di sviluppo.

Apri **app.js** e trova la riga che imposta la variabile di connessione MongoDB.
Sarà simile a questa:

```js
const mongoDB =
  "mongodb+srv://your_user_name:your_password@cluster0.cojoign.mongodb.net/local_library?retryWrites=true&w=majority";
```

Sostituisci la riga con il seguente codice che usa `process.env.MONGODB_URI` per ottenere la stringa di connessione da una variabile d'ambiente denominata `MONGODB_URI` se è stata impostata (usa il tuo URL del database al posto del segnaposto qui sotto).

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
> Un altro modo comune per tenere separate le credenziali del database di produzione dal codice sorgente è leggerle da un file `.env` che viene distribuito separatamente nel file system (ad esempio, potrebbero essere lette usando il modulo npm [dotenv](https://www.npmjs.com/package/dotenv)).

### Impostare NODE_ENV su 'production'

Possiamo rimuovere le tracce dello stack nelle pagine di errore impostando la variabile d'ambiente `NODE_ENV` su _production_ (è impostata su '_development_' di default). Oltre a generare messaggi di errore meno verbosi, impostare la variabile su _production_ memorizza nella cache i template delle viste e i file CSS generati dalle estensioni CSS. I test indicano che impostare `NODE_ENV` su _production_ può migliorare le prestazioni dell'app di un fattore di tre!

Questa modifica può essere fatta utilizzando `export`, un file di ambiente o il sistema di inizializzazione del sistema operativo.

> [!NOTE]
> Questo è in realtà un cambiamento che fai nella tua configurazione dell'ambiente piuttosto che nella tua app, ma importante abbastanza da essere notato qui! Mostreremo come questo è impostato per il nostro esempio di hosting qui sotto.

### Registrare in modo appropriato

Le chiamate di logging possono avere un impatto su un sito web ad alto traffico. In un ambiente di produzione, potrebbe essere necessario registrare l'attività del sito web (ad es., monitorare il traffico o registrare le chiamate alle API) ma dovresti cercare di minimizzare il numero di log aggiunti per scopi di debug.

Un modo per minimizzare il "debug" logging in produzione è utilizzare un modulo come [debug](https://www.npmjs.com/package/debug) che permette di controllare quali log vengono eseguiti impostando una variabile d'ambiente.
Ad esempio, il frammento di codice qui sotto mostra come potresti configurare il logging "author".
La variabile di debug è dichiarata con il nome 'author', e il prefisso "author" verrà visualizzato automaticamente in tutti i log da questo oggetto.

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

  res.render("author_form", { title: "Update Author", author });
});
```

Puoi quindi abilitare un particolare set di log specificandoli come una lista separata da virgole nella variabile d'ambiente `DEBUG`.
Puoi impostare le variabili per visualizzare i log dell'autore e del libro come mostrato (si supportano anche caratteri jolly).

```bash
#Windows
set DEBUG=author,book

#Linux
export DEBUG="author,book"
```

> [!NOTE]
> Le chiamate a `debug` possono sostituire il logging che potresti aver fatto in precedenza usando `console.log()` o `console.error()`. Sostituisci qualsiasi chiamata a `console.log()` nel tuo codice con il logging tramite il modulo [debug](https://www.npmjs.com/package/debug). Attiva e disattiva il logging nel tuo ambiente di sviluppo impostando la variabile DEBUG e osserva l'impatto che ciò ha sul logging.

Se hai bisogno di registrare l'attività del sito web puoi utilizzare una libreria di logging come _Winston_ o _Bunyan_. Per ulteriori informazioni su questo argomento vedi: [Migliori pratiche di produzione: prestazioni e affidabilità](https://expressjs.com/en/advanced/best-practice-performance.html).

### Utilizzare la compressione gzip/deflate per le risposte

I server web possono spesso comprimere la risposta HTTP inviata a un client, riducendo significativamente il tempo richiesto per il client per ottenere e caricare la pagina. Il metodo di compressione utilizzato dipenderà dai metodi di decompressione che il client dichiara di supportare nella richiesta (la risposta verrà inviata non compressa se non sono supportati metodi di compressione).

Aggiungi questo al tuo sito utilizzando il middleware [compression](https://www.npmjs.com/package/compression). Installa questo alla radice del tuo progetto eseguendo il seguente comando:

```bash
npm install compression
```

Apri **./app.js** e richiedi la libreria di compressione come mostrato. Aggiungi la libreria di compressione alla catena dei middleware con il metodo `use()` (questo dovrebbe apparire prima di tutte le rotte che vuoi comprimere - in questo caso, tutte!)

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
> Per un sito web ad alto traffico in produzione non utilizzeresti questo middleware. Al contrario, utilizzeresti un proxy inverso come [Nginx](https://nginx.org/).

### Utilizzare Helmet per proteggersi da vulnerabilità note

[Helmet](https://www.npmjs.com/package/helmet) è un pacchetto middleware. Può impostare intestazioni HTTP appropriate che aiutano a proteggere la tua app dalle vulnerabilità web note (vedi i [documenti](https://helmetjs.github.io/) per ulteriori informazioni sulle intestazioni che imposta e sulle vulnerabilità da cui protegge).

Installa questo alla radice del tuo progetto eseguendo il seguente comando:

```bash
npm install helmet
```

Apri **./app.js** e richiedi la libreria _helmet_ come mostrato.
Quindi aggiungi il modulo alla catena dei middleware con il metodo `use()`.

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

Normalmente potremmo aver appena inserito `app.use(helmet());` per aggiungere il _subset_ delle intestazioni relative alla sicurezza che hanno senso per la maggior parte dei siti.
Tuttavia nel [template di base di LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/LocalLibrary_base_template) includiamo alcuni script bootstrap e jQuery.
Questi violano il _default_ di helmet [Content Security Policy (CSP)](/it/docs/Web/HTTP/Guides/CSP), che non consente il caricamento di script cross-site.
Per consentire il caricamento di questi script modifichiamo la configurazione di helmet in modo che imposti direttive CSP per consentire il caricamento degli script dai domini indicati.
Per il tuo server puoi aggiungere/disabilitare intestazioni specifiche secondo necessità seguendo le [istruzioni per usare helmet qui](https://www.npmjs.com/package/helmet).

### Aggiungi limitazione di velocità alle rotte delle API

[Express-rate-limit](https://www.npmjs.com/package/express-rate-limit) è un pacchetto middleware che può essere utilizzato per limitare richieste ripetute alle API e agli endpoint.
Ci sono molti motivi per cui potrebbero essere fatte richieste eccessive al tuo sito, come attacchi di negazione del servizio, attacchi brute force, o anche solo un client o uno script che non si comporta come previsto.
Oltre ai problemi di prestazioni che possono derivare da troppe richieste che rallentano il tuo server, potresti anche essere addebitato per il traffico aggiuntivo.
Questo pacchetto può essere utilizzato per limitare il numero di richieste che possono essere fatte a una particolare rotta o set di rotte.

Installa questo alla radice del tuo progetto eseguendo il seguente comando:

```bash
npm install express-rate-limit
```

Apri **./app.js** e richiedi la libreria _express-rate-limit_ come mostrato.
Quindi aggiungi il modulo alla catena dei middleware con il metodo `use()`.

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

Il comando sopra limita tutte le richieste a 20 al minuto (puoi cambiarlo secondo necessità).

> [!NOTE]
> Servizi di terze parti come [Cloudflare](https://www.cloudflare.com/) possono anche essere utilizzati se hai bisogno di protezione più avanzata contro attacchi di negazione del servizio o altri tipi di attacchi.

#### Imposta la versione di node

Per le applicazioni Node, compreso Express, il file **package.json** contiene tutto ciò di cui un fornitore di hosting dovrebbe aver bisogno per capire le dipendenze dell'applicazione e il file di entry point.

L'unica informazione importante mancante dal nostro attuale **package.json** è la versione di node richiesta dalla libreria.
Puoi trovare la versione di node utilizzata per lo sviluppo inserendo il comando:

```bash
>node --version
v16.17.1
```

Apri **package.json**, e aggiungi queste informazioni come **engines > node** come mostrato (utilizzando il numero di versione per il tuo sistema).

```json
  "engines": {
    "node": ">=16.17.1"
  },
```

Il servizio di hosting potrebbe non supportare la versione specifica di node indicata, ma questo cambiamento dovrebbe garantire che tenti di usare una versione con lo stesso numero di versione principale, o una versione più recente.

Nota che ci potrebbero essere altri modi per specificare la versione di node su diversi servizi di hosting, ma l'approccio **package.json** è ampiamente supportato.

#### Ottieni dipendenze e riesegui i test

Prima di procedere, testiamo di nuovo il sito e assicuriamoci che non sia stato influenzato da una delle nostre modifiche.

Prima dovremo ottenere le nostre dipendenze. Puoi farlo eseguendo il seguente comando nel tuo terminale alla radice del progetto:

```bash
npm install
```

Ora avvia il sito (vedi [Testare le rotte](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/routes#testing_the_routes) per i comandi pertinenti) e verifica che il sito si comporti ancora come previsto.

### Creare un repository applicazione su GitHub

Molti servizi di hosting ti permettono di importare e/o sincronizzare progetti da un repository locale o da piattaforme di controllo versione del codice sorgente basate su cloud.
Questo può rendere la distribuzione e lo sviluppo iterativo molto più semplici.

Per questo tutorial configureremo un account [GitHub](https://github.com/) e un repository per la libreria, e useremo lo strumento **git** per caricare il nostro codice sorgente.

> [!NOTE]
> Puoi saltare questo passaggio se stai già usando GitHub per gestire il tuo codice sorgente!
>
> Nota che l'uso di strumenti di gestione del codice sorgente è una buona pratica di sviluppo software, poiché ti consente di provare modifiche e passare tra esperimenti e "codice buono conosciuto" quando necessario!

I passaggi sono:

1. Visita <https://github.com/> e crea un account.
2. Una volta effettuato l'accesso, clicca sul link **+** nella barra degli strumenti superiore e seleziona **New repository**.
3. Compila tutti i campi di questo modulo. Anche se non sono obbligatori, sono fortemente raccomandati.

   - Immetti un nuovo nome di repository (ad es., _express-locallibrary-tutorial_), e una descrizione (ad es., "Sito web della Biblioteca Locale scritto in Express (Node)").
   - Scegli **Node** nella lista di selezione _Add .gitignore_.
   - Scegli la tua licenza preferita nella lista di selezione _Add license_.
   - Seleziona **Initialize this repository with a README**.

   > [!WARNING]
   > L'accesso "Public" predefinito renderà _tutto_ il codice sorgente — comprese le credenziali del tuo database — visibile a chiunque su Internet! Assicurati che il codice sorgente legga le credenziali _solo_ dalle variabili di ambiente e non abbia credenziali codificate.
   >
   > Altrimenti, seleziona l'opzione "Private" per consentire solo a persone selezionate di vedere il codice sorgente.

4. Premi **Create repository**.
5. Clicca sul pulsante verde **Clone or download** sulla nuova pagina del tuo repository.
6. Copia il valore URL dal campo di testo all'interno della finestra di dialogo che appare.
   Se hai usato il nome del repository "express-locallibrary-tutorial", l'URL dovrebbe essere qualcosa come: `https://github.com/<your_git_user_id>/express-locallibrary-tutorial.git`.

Ora che il repository ("repo") è creato su GitHub, vogliamo clonarlo (copiarlo) sul nostro computer locale:

1. Installa _git_ per il tuo computer locale ([guida ufficiale per il download di Git](https://git-scm.com/downloads)).
2. Apri un prompt dei comandi / terminale e clona il tuo repo utilizzando l'URL che hai copiato sopra:

   ```bash
   git clone https://github.com/<your_git_user_id>/express-locallibrary-tutorial.git
   ```

   Questo creerà il repository all'interno della directory corrente.

3. Naviga nella cartella del repo.

   ```bash
   cd express-locallibrary-tutorial
   ```

Quindi copia i tuoi file sorgente dell'applicazione nella cartella del repo, rendili parte del repo usando _git_ e caricali su GitHub:

1. Copia la tua applicazione Express in questa cartella (escludendo **/node_modules**, che contiene i file di dipendenze che dovresti ottenere da npm quando necessario).
2. Apri un prompt dei comandi / terminale e usa il comando `add` per aggiungere tutti i file a git.

   ```bash
   git add -A
   ```

3. Usa il comando `status` per verificare che tutti i file che stai per `commit` siano corretti (vuoi includere file di origine, non binari, file temporanei ecc.).
   Dovrebbe assomigliare un po' all'elenco qui sotto.

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
   Questo equivale a firmare sui cambiamenti e renderli una parte ufficiale del repo locale.

   ```bash
   git commit -m "First version of application moved into GitHub"
   ```

5. A questo punto, il repo remoto non è stato modificato.
   L'ultimo passo è sincronizzare (`push`) il tuo repo locale fino al repo remoto di GitHub usando il seguente comando:

   ```bash
   git push origin main
   ```

Quando quest'operazione è completata, dovresti essere in grado di tornare alla pagina su GitHub dove hai creato il tuo repo, aggiornare la pagina e vedere che tutta la tua applicazione è stata ora caricata. Puoi continuare ad aggiornare il tuo repo man mano che i file cambiano usando questo ciclo add/commit/push.

Questo è un buon punto per fare un backup del tuo progetto "vanilla" — mentre alcuni dei cambiamenti che stiamo per fare nelle sezioni successive potrebbero essere utili per la distribuzione su qualsiasi servizio di hosting (o per lo sviluppo), altri potrebbero non esserlo.
Puoi farlo usando `git` sulla riga di comando:

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
> Per saperne di più, vedi [Imparare Git](https://docs.github.com/en/get-started/start-your-journey/git-and-github-learning-resources).

## Esempio: Ospitare su Glitch

Questa sezione fornisce una dimostrazione pratica di come ospitare la _LocalLibrary_ su [Glitch](https://glitch.com/).

### Perché Glitch?

Scegliamo di usare Glitch per diversi motivi:

- Glitch ha un [piano gratuito di partenza](https://glitch.com/pricing) che è _davvero_ gratuito, sebbene con alcune limitazioni.
  Il fatto che sia accessibile a tutti gli sviluppatori è davvero importante per MDN!
- Glitch si occupa dell'infrastruttura, quindi non devi farlo tu.
  Non dovendo preoccuparti di server, bilanciatori di carico, proxy inversi, e così via, diventa molto più facile iniziare.
- Le competenze e i concetti che imparerai utilizzando Glitch sono trasferibili.
- Il servizio e le limitazioni del piano non incidono molto sull'uso di Glitch per il tutorial.
  Ad esempio:

  - Il piano starter offre solo 1000 "ore di progetto" al mese, che vengono ripristinate mensilmente.
    Questo viene utilizzato quando stai modificando attivamente il sito o quando qualcuno vi accede.
    Se nessuno sta accedendo o modificando il sito, andrà in sospensione.
  - L'ambiente del piano starter ha una quantità limitata di RAM del container e spazio di archiviazione.
    Ce n'è più che sufficiente per il tutorial, in particolare perché il nostro database è ospitato altrove.
  - I domini personalizzati non sono ben supportati (al momento della scrittura).
  - Altre limitazioni possono essere trovate nella [pagina delle restrizioni tecniche di Glitch](https://help.glitch.com/s/article/Technical-Restrictions).

Mentre Glitch è appropriato per ospitare questa dimostrazione, dovresti prenderti il tempo di determinare se è [adatto per il tuo sito web](#scegliere_un_fornitore_di_hosting).

### Come funziona Glitch?

Glitch fornisce un'interfaccia basata su web in cui puoi creare progetti da modelli di partenza, o importarli da GitHub, e quindi aggiungere e modificare i file del progetto.
Man mano che apporti modifiche, il progetto viene costruito ed eseguito nel proprio container virtualizzato, isolato e indipendente.

Come tutto questo funziona "sotto il cofano" è un mistero — Glitch non lo dice.
Ciò che è chiaro è che finché crei un'app Web fairly standard nodejs (ad esempio, utilizzando `package.json` per le dipendenze), e non consumi più risorse di quelle elencate nelle [restrizioni tecniche](https://help.glitch.com/s/article/Technical-Restrictions), la tua applicazione dovrebbe "funzionare e basta".

Una volta che l'applicazione è in esecuzione, può essere configurata per la produzione utilizzando i [dati privati](https://help.glitch.com/s/article/Adding-Private-Data) forniti in un file `.env`.
I valori nei dati segreti vengono letti dall'applicazione come variabili d'ambiente, che, come ricorderai da una sezione precedente, è il modo in cui abbiamo configurato la nostra applicazione per ottenere l'URL del database.
Nota che le variabili sono _segrete_: il `.env` non dovrebbe essere incluso nel tuo repository GitHub.

La vista di modifica di Glitch fornisce anche l'accesso al _terminale_ all'ambiente dell'app web, che puoi usare per lavorare con l'app web come se fosse in esecuzione sul tuo computer locale.

Questa è tutta la panoramica di cui hai bisogno per iniziare.
In seguito, configureremo un account Glitch, caricheremo il progetto Library da GitHub, e lo connetteremo a un database.

### Ottieni un account su Glitch

Per iniziare a usare Glitch, devi prima creare un account:

- Vai su [glitch.com](https://glitch.com/) e clicca sul pulsante **Sign up** nella barra degli strumenti in alto.
- Seleziona GitHub nel popup per registrarti utilizzando le tue credenziali GitHub.
- Sarai quindi loggato alla dashboard di Glitch: <https://glitch.com/dashboard>.

### Risoluzione dei problemi della versione di Node.js

I fornitori di hosting in generale supportano alcune versioni principali delle ultime release di Node.js.
Se la "minor" version esatta che hai specificato nel tuo file `package.json` non è supportata, di solito ricadono sulla versione più vicina che supportano (e spesso questo funzionerà).

Sfortunatamente, al momento della scrittura, la versione massima supportata su Glitch è Node.js 16.
Se hai sviluppato con Node.js 17 o successivi, dovresti ridurre la versione utilizzata nel tuo file `package.json` come mostrato.
Dovrai anche rieseguire i test:

```json
  "engines": {
    "node": ">=v16"
  },
```

Glitch [prevede di aggiornare e mantenere meglio aggiornata la versione di node in futuro](https://blog.glitch.com/post/rebuilding-glitch/) — e può essere che quando leggi questo, il limite di versionamento non esista più.
Invece di ridurre la versione di `node`, potresti caricare il tuo progetto per vedere se viene costruito.
Se ci sono errori e la tua applicazione non viene caricata, dovresti provare a impostare la versione di `node` su `>=v16` nel tuo `package.json` nell'editor di Glitch.

> [!NOTE]
> Puoi anche controllare le versioni supportate immettendo il seguente comando nel terminale di qualsiasi progetto Glitch:
>
> ```sh
> ls -l /opt/nvm/versions/node | grep '^d' | awk '{ print $9 }'
> ```

### Distribuire su Glitch da GitHub

Successivamente importeremo il progetto Library da GitHub.
Per prima cosa scegli l'opzione **Dashboard** dal menu in alto del sito, quindi seleziona il pulsante **New project**.
Glitch visualizzerà un elenco di opzioni per il nuovo progetto; seleziona **Import from GitHub**.

![Dashboard del sito web di Glitch che mostra un pulsante nuovo progetto e un menu a comparsa con l'opzione "Import from GitHub"](glitch_new_project_import_github.png)

Apparirà un popup.
Immetti l'URL del tuo repository GitHub nel popup e premi **OK**.
Di seguito, abbiamo inserito il repo per il progetto lavorato.

![Popup di Glitch per inserire l'URL del repository GitHub da importare](glitch_new_project_github_repo_url.png)

Glitch importerà quindi il progetto, visualizzando le notifiche di progresso.
Al termine, mostrerà la vista di modifica per il nuovo progetto, come mostrato di seguito.

![Vista dell'editor di Glitch per il progetto importato](glitch_imported_project_in_editor.png)

Puoi ottenere l'URL del sito live selezionando il pulsante **Share**.

![Vista dell'editor di Glitch per il progetto importato](glitch_share_project.png)

Apri una nuova scheda del browser e copia il link per il sito live nella barra degli indirizzi.
Il sito della biblioteca locale dovrebbe aprirsi e visualizzare i dati dal database di sviluppo.

> [!NOTE]
> Questo processo è stata un'importazione una tantum da GitHub.
> Puoi anche usare azioni GitHub come [glitch-project-sync](https://github.com/marketplace/actions/glitch-project-sync) per mantenere Glitch e il tuo progetto sincronizzati.

### Usare un database MongoDB di produzione

Dovresti configurare un database diverso per la produzione rispetto a quello per lo sviluppo.
Mentre Glitch ospita solo database SQLite (e siamo configurati per usare MongoDB), molti altri siti forniscono database MongoDB come servizio.

Un'opzione è seguire le istruzioni di [Configurazione del database MongoDB](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose#setting_up_the_mongodb_database) precedenti nel tutorial per configurare un nuovo database di produzione.

Per rendere il database di produzione accessibile all'applicazione libreria, apri il file `.env` nella vista dell'editor per il progetto.
Immetti la variabile URL del database `MONGODB_URI` e l'URL del tuo database di produzione.
Il sito si aggiorna man mano che inserisci valori nell'editor.

![Editor del file .env di Glitch per dati privati con variabili di produzione](glitch_env.png)

> [!NOTE]
> Non abbiamo creato questo file.
> È destinato ai [dati privati](https://help.glitch.com/s/article/Adding-Private-Data) ed è stato creato automaticamente all'importazione in Glitch.
> Non viene mai esportato o copiato.

### Altre variabili di configurazione

Ricorderai dalla sezione precedente che dobbiamo [impostare NODE_ENV su 'production'](#set_node_env_to_production) per migliorare le nostre prestazioni e generare messaggi di errore meno verbosi. Facciamo questo nello stesso file in cui impostiamo la variabile `MONGODB_URI`.

Apri `.env` e aggiungi una variabile `NODE_ENV` con il valore `production` (vedi la schermata nella sezione precedente).

L'applicazione biblioteca locale è ora impostata e configurata per l'uso in produzione.
Puoi aggiungere dati tramite l'interfaccia del sito web e dovrebbe funzionare come durante lo sviluppo (anche se con meno informazioni di debug esposte per le pagine non valide).

> [!NOTE]
> Se desideri solo aggiungere dei dati per il test, potresti utilizzare lo script `populatedb` (con il tuo URL del database di produzione MongoDB) come discusso nella sezione [Tutorial su Express Parte 3: Utilizzare un Database (con Mongoose) Test — creare alcuni elementi](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose#testing_%E2%80%94_create_some_items).

### Debug delle app Express su Glitch

Glitch consente un debug efficace.
Alcune delle cose che puoi fare sono:

- Seleziona il pulsante dei log nella parte inferiore della vista dell'editor per vedere le informazioni di log dal tuo server, come l'output del log della console.
- Seleziona il pulsante terminale nella parte inferiore della vista dell'editor per aprire un terminale nell'ambiente di hosting.
  Puoi usare questo per eseguire comandi e strumenti nell'ambiente.
  Ad esempio, potresti usare `node -v` per controllare la versione di node.
- Debug interattivo in VS Code utilizzando l'estensione GLITCH per VS Code.

## Esempio: Ospitare su Railway

Questa sezione fornisce una dimostrazione pratica di come installare _LocalLibrary_ su [Railway](https://railway.com/).

### Perché Railway?

> [!WARNING]
> Railway non ha più un tier starter completamente gratuito.
> Abbiamo mantenuto queste istruzioni perché Railway ha alcune ottime funzionalità e sarà un'opzione migliore per alcuni utenti.

Railway è un'opzione di hosting attraente per diversi motivi:

- Railway si prende cura della maggior parte dell'infrastruttura, quindi non devi farlo tu.
  Non dovendo preoccuparti di server, bilanciatori di carico, proxy inversi, e così via, diventa molto più facile iniziare.
- Railway ha un [focus sull'esperienza dello sviluppatore per lo sviluppo e la distribuzione](https://docs.railway.com/maturity/compare-to-heroku), che porta a una curva di apprendimento più veloce e più morbida rispetto a molte altre alternative.
- Le competenze e i concetti che imparerai utilizzando Railway sono trasferibili.
  Anche se Railway ha alcune eccellenti nuove funzionalità, altri servizi di hosting popolari utilizzano molte delle stesse idee e approcci.
- [Documentazione di Railway](https://docs.railway.com/) è chiara e completa.
- Il servizio sembra essere molto affidabile, e se finisci per amarlo, i prezzi sono prevedibili e scalare la tua app è molto facile.

Dovresti prenderti il tempo di determinare se Railway è [adatto per il tuo sito web](#scegliere_un_fornitore_di_hosting).

### Come funziona Railway?

Le applicazioni web vengono eseguite ciascuna nel proprio container virtualizzato, isolato e indipendente.
Per eseguire la tua applicazione, Railway deve essere in grado di impostare l'ambiente e le dipendenze appropriate e anche capire come viene lanciata.

Railway rende tutto questo facile, poiché può automaticamente riconoscere e installare molti diversi framework ed ambienti di applicazioni web basati sul loro uso di "convenzioni comuni".
Ad esempio, Railway riconosce le applicazioni node poiché hanno un file **package.json**, e può determinare il gestore dei pacchetti usato per la costruzione dal file "lock".
Ad esempio, se l'applicazione include il file **package-lock.json**, Railway sa di usare _npm_ per installare i pacchetti, mentre se trova **yarn.lock** sa di usare _yarn_.
Una volta installate tutte le dipendenze, Railway cercherà script denominati "build" e "start" nel file del pacchetto, e userà questi per costruire ed eseguire il codice.

> [!NOTE]
> Railway usa [Nixpacks](https://nixpacks.com/docs) per riconoscere vari framework di applicazioni web scritti in diversi linguaggi di programmazione.
> Non hai bisogno di sapere altro per questo tutorial, ma puoi trovare ulteriori informazioni sulle opzioni per distribuire applicazioni node in [Nixpacks Node](https://nixpacks.com/docs/providers/node).

Una volta che l'applicazione è in esecuzione, può configurarsi utilizzando le informazioni fornite nelle [variabili d'ambiente](https://docs.railway.com/guides/variables).
Ad esempio, un'applicazione che utilizza un database deve ottenere l'indirizzo usando una variabile.
Il servizio di database stesso può essere ospitato da Railway o da qualche altro fornitore.

Gli sviluppatori interagiscono con Railway attraverso il sito di Railway e utilizzando un [interfaccia a riga di comando (CLI)](https://docs.railway.com/guides/cli) speciale.
La CLI ti consente di associare un repository GitHub locale a un progetto Railway, caricare il repository dal branch locale al sito live, ispezionare i log del processo in esecuzione, impostare e ottenere variabili di configurazione e molto altro.
Una delle funzionalità più utili è che puoi usare la CLI per eseguire il tuo progetto locale con le stesse variabili d'ambiente del progetto live.

Questa è tutta la panoramica di cui hai bisogno per distribuire l'app su Railway.
Nel seguito configureremo un account Railway, installeremo il nostro sito web e un database e proveremo il client Railway.

### Ottieni un account su Railway

Per iniziare a usare Railway, devi prima creare un account:

- Vai su [railway.com](https://railway.com/) e clicca sul link **Login** nella barra degli strumenti in alto.
- Seleziona GitHub nel popup per accedere utilizzando le tue credenziali GitHub
- Potrebbe essere necessario andare sulla tua email e verificare il tuo account.
- Sarai quindi loggato alla dashboard di Railway.com: <https://railway.com/dashboard>.

### Distribuire su Railway da GitHub

Successivamente configureremo Railway per distribuire la nostra libreria da GitHub.
Per prima cosa scegli l'opzione **Dashboard** dal menu in alto del sito, quindi seleziona il pulsante **New Project**:

![Dashboard del sito web di Railway che mostra un pulsante nuovo progetto](railway_new_project_button.png)

Railway visualizzerà un elenco di opzioni per il nuovo progetto, inclusa l'opzione per distribuire un progetto da un modello che viene prima creato nel tuo account GitHub e un numero di database.
Seleziona **Deploy from GitHub repo**.

![Popup di Railway che mostra le opzioni di distribuzione con l'opzione "Deploy from GitHub repo" evidenziata](railway_new_project_button_deploy_github_repo.png)

Vengono visualizzati tutti i progetti nei repository GitHub che hai condiviso con Railway durante la configurazione.
Seleziona il tuo repository GitHub per la biblioteca locale: `<user-name>/express-locallibrary-tutorial`.

![Popup di Railway che mostra i repository GitHub che possono essere distribuiti](railway_new_project_button_deploy_github_selectrepo.png)

Conferma la tua distribuzione selezionando **Deploy Now**.

![Schermata di conferma in cui puoi selezionare la distribuzione del progetto](railway_new_project_deploy_confirm.png)

Railway caricherà quindi e distribuirà il tuo progetto, visualizzando il progresso nella scheda distribuzioni.
Quando la distribuzione viene completata correttamente, vedrai una schermata come quella qui sotto.

![Dashboard di Railway che mostra la scheda Distribuzioni per il progetto distribuito](railway_project_deploy.png)

Ora seleziona la scheda _Settings_, quindi scorri verso il basso fino alla sezione Domini e premi il pulsante **Generate Domain**.

![Scheda delle impostazioni del progetto Railway che mostra il pulsante per generare un dominio](railway_project_generate_domain.png)

Ciò pubblicherà il sito e posizionerà il dominio al posto del pulsante, come mostrato di seguito.

![Scheda delle impostazioni del progetto Railway che mostra un link al sito della biblioteca locale](railway_project_domain.png)

Seleziona l'URL del dominio per aprire la tua applicazione libreria.
Nota che, poiché non abbiamo specificato un database di produzione, la biblioteca locale si aprirà utilizzando i tuoi dati di sviluppo.

### Fornire e connettere un database MongoDB

Invece di utilizzare i nostri dati di sviluppo, ora creiamo un database MongoDB di produzione da utilizzare invece.
Creeremo il database come parte del progetto dell'applicazione Railway, anche se niente ti impedisce di crearlo nel suo progetto separato o, in realtà, di utilizzare un database _MongoDB Atlas_ per i dati di produzione, proprio come hai fatto per il database di sviluppo.

Su Railway, scegli l'opzione **Dashboard** dal menu in alto del sito, quindi seleziona il tuo progetto applicativo.
In questa fase, contiene solo un servizio per la tua applicazione (questo può essere selezionato per impostare variabili e altri dettagli del servizio).
Seleziona il pulsante **New**, che viene utilizzato per aggiungere servizi al progetto corrente.

![Progetto Railway con il pulsante nuovo servizio evidenziato](railway_project_open_no_database.png)

Seleziona **Database** quando ti viene chiesto quale tipo di servizio aggiungere:

![Popup di Railway che mostra le opzioni per un nuovo servizio, come database, repository GitHub, servizio vuoto ecc.](railway_database_add.png)

Quindi seleziona **Add MongoDB** per iniziare ad aggiungere il database

![Popup di Railway che mostra i diversi database che possono essere selezionati: Postgres, MySQL, MongoDB e così via](railway_database_select_type.png)

Railway fornirà quindi un servizio contenente un database vuoto nello stesso progetto.
Al completamento vedrai entrambi i servizi applicativi e di database nella vista del progetto.

![Progetto Railway con servizi di applicazione e database](railway_project_two_services.png)

Seleziona il servizio MongoDB per visualizzare le informazioni sul database.
Apri la scheda _Variables_ e copia il "Mongo_URL" (questo è l'indirizzo del database).

![Schermata delle impostazioni del database Railway che mostra l'URL necessario per connettersi al database](railway_mongodb_connect.png)

Per rendere questo accessibile all'applicazione libreria, dobbiamo aggiungerlo al processo di applicazione utilizzando una variabile d'ambiente.
Prima apri il servizio di applicazione.
Quindi seleziona la scheda _Variables_ e premi il pulsante **New Variable**.

Inserisci il nome della variabile `MONGODB_URI` e l'URL di connessione che hai copiato per il database (`MONGODB_URI` è il nome della variabile d'ambiente da cui [abbiamo configurato l'applicazione](#configurazione_del_database) per leggere l'indirizzo del database).
Questo sarà simile alla schermata mostrata di seguito.

![Schermata delle variabili del sito web Railway mentre si aggiunge la variabile MONGODB_URI e l'indirizzo](railway_variables_database_url.png)

Seleziona **Add** per aggiungere la variabile.

Railway riavvia la tua app quando aggiorna le variabili. Se controlli la homepage ora, dovrebbe mostrare valori zero per il conteggio degli oggetti, poiché le modifiche di cui sopra significano che stiamo ora utilizzando un nuovo database (vuoto).

### Altre variabili di configurazione

Ricorderai dalla sezione precedente che abbiamo bisogno di [impostare NODE_ENV su 'production'](#set_node_env_to_production) per migliorare le nostre prestazioni e generare messaggi di errore meno verbosi.
Possiamo farlo nella stessa schermata in cui impostiamo la variabile `MONGODB_URI`.

Apri il servizio di applicazione.
Quindi seleziona la scheda _Variables_, dove vedrai che `MONGODB_URI` è già definito, e premi il pulsante **New Variable**.

![Scheda delle variabili di Railway con il pulsante New Variable evidenziato](railway_variables_new.png)

Inserisci `NODE_ENV` come nome della nuova variabile e `production` come nome dell'ambiente.
Quindi premi il pulsante **Add**.

![Scheda delle variabili di Railway con nuova variabile NODE_ENV impostata su 'production'](railway_variables_new_node_env.png)

L'applicazione della biblioteca locale è ora configurata e pronta per l'uso in produzione.
Puoi aggiungere dati tramite l'interfaccia del sito e dovrebbe funzionare nello stesso modo in cui ha funzionato durante lo sviluppo (sebbene con meno informazioni di debug esposte per le pagine non valide).

> [!NOTE]
> Se vuoi solo aggiungere alcuni dati per il testing, potresti usare lo script `populatedb` (con il tuo URL del database di produzione MongoDB) come discusso nella sezione [Tutorial su Express Parte 3: Utilizzare un Database (con Mongoose) Testing — creare alcuni elementi](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose#testing_%E2%80%94_create_some_items).

### Installare il client

Scarica e installa il client Railway per il tuo sistema operativo locale seguendo le [istruzioni qui](https://docs.railway.com/guides/cli).

Dopo che il client è stato installato, sarai in grado di eseguire i comandi.
Alcune delle operazioni più importanti includono distribuire la directory corrente del tuo computer a un progetto Railway associato (senza dover caricare su GitHub) e eseguire il tuo progetto localmente utilizzando le stesse impostazioni che hai sul server di produzione.

Puoi ottenere un elenco di tutti i comandi possibili immettendo il seguente nel terminale.

```bash
railway help
```

### Debugging

Il client Railway fornisce il comando logs per mostrare la coda dei log (un log più completo è disponibile sul sito per ogni progetto):

```bash
railway logs
```

## Sommario

Questo è la fine di questo tutorial su come impostare le app Express in produzione, e anche la serie di tutorial su come lavorare con Express. Speriamo che li abbia trovati utili. Puoi controllare una versione completamente lavorata del [codice sorgente su GitHub qui](https://github.com/mdn/express-locallibrary-tutorial).

## Vedi anche

- [Migliori pratiche di produzione: prestazioni e affidabilità](https://expressjs.com/en/advanced/best-practice-performance.html) (documenti Express)
- [Migliori pratiche di produzione: Sicurezza](https://expressjs.com/en/advanced/best-practice-security.html) (documenti Express)
- Documenti Railway

  - [CLI](https://docs.railway.com/guides/cli)

- DigitalOcean

  - Tutorial [Express](https://www.digitalocean.com/community/tutorials?q=express)
  - Tutorial [Node.js](https://www.digitalocean.com/community/tutorials?q=node.js)

- Heroku

  - [Introduzione a Heroku con Node.js](https://devcenter.heroku.com/articles/getting-started-with-nodejs) (documenti Heroku)
  - [Distribuire Applicazioni Node.js su Heroku](https://devcenter.heroku.com/articles/deploying-nodejs) (documenti Heroku)
  - [Supporto di Heroku per Node.js](https://devcenter.heroku.com/articles/nodejs-support) (documenti Heroku)
  - [Ottimizzare la Concorrenza delle Applicazioni Node.js](https://devcenter.heroku.com/articles/node-concurrency) (documenti Heroku)
  - [Come funziona Heroku](https://devcenter.heroku.com/articles/how-heroku-works) (documenti Heroku)
  - [Dynos e il Gestore di Dyno](https://devcenter.heroku.com/articles/dynos) (documenti Heroku)
  - [Configurazione e Variabili di Config](https://devcenter.heroku.com/articles/config-vars) (documenti Heroku)
  - [Limiti](https://devcenter.heroku.com/articles/limits) (documenti Heroku)

{{PreviousMenu("Learn_web_development/Extensions/Server-side/Express_Nodejs/forms", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}
