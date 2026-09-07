---
title: "Tutorial su Express - Parte 7: Distribuzione in produzione"
short-title: "7: Distribuzione"
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/deployment
l10n:
  sourceCommit: e2c34c75df6238fbeff790100cea1ab7e552e49e
---

{{PreviousMenu("Learn_web_development/Extensions/Server-side/Express_Nodejs/forms", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}

Ora che è stato creato e testato un sito web di esempio con Express, è il momento di distribuirlo su un server web affinché sia accessibile pubblicamente tramite Internet.
Questa pagina spiega come ospitare un progetto Express e descrive ciò che serve per prepararlo alla produzione.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Completare tutti gli argomenti precedenti del tutorial, incluso <a href="/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms">Tutorial su Express - Parte 6: Lavorare con i moduli</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare dove e come distribuire un'app Express in produzione.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

Una volta completato il sito (o completato "a sufficienza" per iniziare i test pubblici), sarà necessario ospitarlo in un luogo più pubblico e accessibile rispetto al proprio computer di sviluppo.

Finora si è lavorato in un [ambiente di sviluppo](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment), usando Express/Node come server web per condividere il sito con il browser/rete locale ed eseguendo il sito con impostazioni di sviluppo (non sicure) che espongono informazioni di debug e altre informazioni private. Prima di poter ospitare un sito web esternamente, sarà necessario:

- Scegliere un ambiente in cui ospitare l'app Express.
- Apportare alcune modifiche alle impostazioni del progetto.
- Configurare un'infrastruttura di livello produttivo per servire il sito web.

Questo tutorial fornisce indicazioni sulle opzioni disponibili per scegliere un sito di hosting, una breve panoramica di ciò che occorre fare per preparare l'app Express alla produzione e un esempio funzionante per installare il sito web LocalLibrary sul servizio di cloud hosting [Railway](https://railway.com/).

## Che cos'è un ambiente di produzione?

L'ambiente di produzione è l'ambiente fornito dal computer server su cui verrà eseguito il sito web per la fruizione esterna. L'ambiente include:

- Hardware del computer su cui viene eseguito il sito web.
- Sistema operativo, ad esempio Linux o Windows.
- Runtime del linguaggio di programmazione e librerie del framework su cui è scritto il sito web.
- Infrastruttura del server web, che può includere un server web, un reverse proxy, un load balancer e così via.
- Database da cui dipende il sito web.

Il computer server potrebbe trovarsi presso la propria sede ed essere connesso a Internet tramite una connessione veloce, ma è molto più comune usare un computer ospitato "nel cloud". In pratica, ciò significa che il codice viene eseguito su un computer remoto, o eventualmente su un computer "virtuale", nei data center della società di hosting. Il server remoto offrirà di solito un certo livello garantito di risorse di calcolo, ad esempio CPU, RAM, memoria di archiviazione e così via, e di connettività Internet a un determinato prezzo.

Questo tipo di hardware di calcolo/rete accessibile da remoto è chiamato _Infrastructure as a Service (IaaS)_. Molti fornitori IaaS offrono opzioni per preinstallare un particolare sistema operativo, sul quale è necessario installare gli altri componenti dell'ambiente di produzione. Altri fornitori consentono di selezionare ambienti più completi, magari includendo una configurazione Node completa.

> [!NOTE]
> Gli ambienti preconfigurati possono semplificare la configurazione del sito web perché riducono la configurazione necessaria, ma le opzioni disponibili possono limitare a un server sconosciuto, o ad altri componenti, e possono basarsi su una versione meno recente del sistema operativo. Spesso è preferibile installare autonomamente i componenti, così da ottenere quelli desiderati e sapere da dove iniziare quando è necessario aggiornare parti del sistema.

Altri provider di hosting supportano Express come parte di un'offerta _Platform as a Service_ (_PaaS_). Usando questo tipo di hosting non è necessario preoccuparsi della maggior parte dell'ambiente di produzione, come server, load balancer e così via, perché la piattaforma di hosting se ne occupa. Questo rende la distribuzione piuttosto semplice, poiché è necessario concentrarsi solo sull'applicazione web e non su altre infrastrutture server.

Alcuni sviluppatori sceglieranno la maggiore flessibilità offerta da IaaS rispetto a PaaS, mentre altri apprezzeranno la riduzione della manutenzione e dello sforzo di scalabilità di PaaS. All'inizio, configurare il sito web su un sistema PaaS è molto più semplice; è quindi ciò che verrà fatto in questo tutorial.

> [!NOTE]
> Se viene scelto un provider di hosting adatto a Node/Express, dovrebbe fornire istruzioni su come configurare un sito web Express usando diverse configurazioni di server web, application server, reverse proxy e così via. Ad esempio, sono disponibili molte guide passo passo per varie configurazioni nella [documentazione della community Node di DigitalOcean](https://www.digitalocean.com/community/tutorials?q=node).

## Scegliere un provider di hosting

Esistono numerosi provider di hosting noti per supportare attivamente _Node_ e _Express_, oppure per funzionare bene con essi. Questi fornitori offrono diversi tipi di ambienti, IaaS e PaaS, e diversi livelli di risorse di calcolo e rete a prezzi differenti.

> [!NOTE]
> Esistono molte soluzioni di hosting e i relativi servizi e prezzi possono cambiare nel tempo. Sebbene vengano presentate alcune opzioni di seguito, vale la pena verificare sia queste sia altre alternative prima di selezionare un provider di hosting.

Alcuni aspetti da considerare nella scelta di un host:

- Quanto traffico è probabile che abbia il sito e il costo delle risorse di dati e calcolo richieste per soddisfare tale domanda.
- Livello di supporto per la scalabilità orizzontale, aggiungendo più macchine, e verticale, aggiornando a macchine più potenti, nonché i relativi costi.
- Le località in cui il fornitore dispone di data center e, di conseguenza, dove è probabile che l'accesso sia più rapido.
- Prestazioni storiche dell'host in termini di uptime e downtime.
- Strumenti forniti per gestire il sito: sono facili da usare e sicuri, ad esempio SFTP rispetto a FTP?
- Framework integrati per monitorare il server.
- Limitazioni note. Alcuni host bloccano deliberatamente determinati servizi, ad esempio l'email. Altri offrono solo un certo numero di ore di "tempo attivo" in alcune fasce di prezzo oppure soltanto una piccola quantità di spazio di archiviazione.
- Vantaggi aggiuntivi. Alcuni provider offrono nomi di dominio gratuiti e supporto per certificati TLS che altrimenti richiederebbero un pagamento.
- Se il piano "gratuito" su cui si fa affidamento scade nel tempo e se il costo della migrazione a un piano più costoso significa che sarebbe stato meglio usare fin dall'inizio un altro servizio.

La buona notizia per chi inizia è che esistono diversi siti che offrono ambienti di calcolo "gratuiti" pensati per la valutazione e i test.
Si tratta generalmente di ambienti con risorse piuttosto limitate ed è necessario tenere presente che potrebbero scadere dopo un periodo introduttivo o avere altri vincoli.
Sono tuttavia ottimi per testare siti con poco traffico in un ambiente ospitato e possono offrire una facile migrazione verso risorse a pagamento quando il sito diventa più frequentato.
Scelte popolari in questa categoria includono [Amazon Web Services](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/free-tier.html) e [Microsoft Azure](https://azure.microsoft.com/en-us/pricing/details/app-service/linux/).

La maggior parte dei provider offre anche un piano "basic" o "hobby" destinato a piccoli siti di produzione, con livelli più utili di potenza di calcolo e meno limitazioni.
[Railway](https://railway.com/), [Heroku](https://www.heroku.com/) e [DigitalOcean](https://www.digitalocean.com/) sono esempi di provider di hosting popolari che dispongono di un livello base di calcolo relativamente economico, nella fascia tra $5 e $10 USD al mese.

> [!NOTE]
> Ricordare che il prezzo non è l'unico criterio di selezione.
> Se il sito web ha successo, la scalabilità potrebbe rivelarsi la considerazione più importante.

## Preparare il sito web alla pubblicazione

Gli aspetti principali a cui pensare quando si pubblica un sito web sono sicurezza web e prestazioni.
Come minimo, sarà opportuno modificare la configurazione del database in modo da poter usare un database diverso per la produzione e proteggere le relative credenziali, rimuovere le stack trace incluse nelle pagine di errore durante lo sviluppo, riordinare il logging e impostare gli header appropriati per evitare molte minacce di sicurezza comuni.

Nelle sottosezioni seguenti vengono descritte le modifiche più importanti da apportare all'app.

> [!NOTE]
> Sono disponibili altri suggerimenti utili nella documentazione di Express: vedere [Best practice per la produzione: prestazioni e affidabilità](https://expressjs.com/en/advanced/best-practice-performance/) e [Best practice per la produzione: sicurezza](https://expressjs.com/en/advanced/best-practice-security/).

### Configurazione del database

Finora in questo tutorial è stato usato un unico database di sviluppo, per il quale indirizzo e credenziali erano stati [codificati direttamente in **bin/www**](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose#connect_to_mongodb).
Poiché il database di sviluppo non contiene informazioni la cui esposizione o corruzione rappresenti un problema, non esiste un rischio particolare nel divulgare questi dettagli.
Tuttavia, se si lavora con dati reali, in particolare informazioni personali degli utenti, è molto importante proteggere le credenziali del database.

Per questo motivo si desidera usare un database diverso per la produzione rispetto allo sviluppo e mantenere inoltre le credenziali del database di produzione separate dal codice sorgente, affinché possano essere protette correttamente.

Se il provider di hosting supporta l'impostazione di variabili d'ambiente tramite un'interfaccia web, come molti fanno, un modo per farlo consiste nel fare in modo che il server ottenga l'URL del database da una variabile d'ambiente.
Di seguito viene modificato il sito web LocalLibrary affinché ottenga l'URI del database da una variabile d'ambiente del sistema operativo, se definita, e usi altrimenti l'URL del database di sviluppo.

Aprire **bin.www** e individuare la riga che imposta la variabile di connessione MongoDB.
Dovrebbe assomigliare a questa:

```js
const mongoDB =
  "mongodb+srv://your_user_name:your_password@cluster0.cojoign.mongodb.net/local_library?retryWrites=true&w=majority";
```

Sostituire la riga con il codice seguente, che usa `process.env.MONGODB_URI` per ottenere la stringa di connessione da una variabile d'ambiente denominata `MONGODB_URI`, se è stata impostata. Usare il proprio URL del database al posto del segnaposto riportato di seguito.

```js
const dev_db_url =
  "mongodb+srv://your_user_name:your_password@cluster0.cojoign.mongodb.net/local_library?retryWrites=true&w=majority";
const mongoDB = process.env.MONGODB_URI || dev_db_url;
```

> [!NOTE]
> Un altro modo comune per mantenere le credenziali del database di produzione separate dal codice sorgente consiste nel leggerle da un file `.env` distribuito separatamente nel file system, ad esempio usando il modulo [dotenv](https://www.npmjs.com/package/dotenv) di npm.

### Impostare NODE_ENV su 'production'

È possibile rimuovere le stack trace dalle pagine di errore impostando la variabile d'ambiente `NODE_ENV` su _production_, che per impostazione predefinita è impostata su '_development_'. Oltre a generare messaggi di errore meno dettagliati, l'impostazione della variabile su _production_ memorizza nella cache i template delle viste e i file CSS generati dalle estensioni CSS. I test indicano che impostare `NODE_ENV` su _production_ può migliorare le prestazioni dell'app di un fattore tre.

Questa modifica può essere effettuata usando `export`, un file di ambiente oppure il sistema di inizializzazione del sistema operativo.

> [!NOTE]
> Si tratta in realtà di una modifica da apportare alla configurazione dell'ambiente anziché all'app, ma è abbastanza importante da essere segnalata qui. Di seguito verrà mostrato come viene impostata per il nostro esempio di hosting.

### Eseguire il logging in modo appropriato

Le chiamate di logging possono avere un impatto su un sito web ad alto traffico. In un ambiente di produzione potrebbe essere necessario registrare l'attività del sito web, ad esempio per monitorare il traffico o registrare le chiamate API, ma occorre cercare di ridurre al minimo la quantità di logging aggiunta per scopi di debug.

Un modo per ridurre il logging di "debug" in produzione consiste nell'usare un modulo come [debug](https://www.npmjs.com/package/debug), che permette di controllare quale logging venga eseguito impostando una variabile d'ambiente.
Ad esempio, il frammento di codice seguente mostra come configurare il logging "author".
La variabile debug viene dichiarata con il nome 'author' e il prefisso "author" verrà visualizzato automaticamente per tutti i log provenienti da questo oggetto.

```js
const debug = require("debug")("author");

// Display Author update form on GET.
exports.author_update_get = async (req, res, next) => {
  const author = await Author.findById(req.params.id).exec();
  if (author === null) {
    // No results.
    debug(`id not found on update: ${req.params.id}`);
    const err = new Error("Author not found");
    err.status = 404;
    return next(err);
  }

  res.render("author_form", { title: "Update Author", author });
};
```

È quindi possibile abilitare un particolare insieme di log specificandoli come elenco separato da virgole nella variabile d'ambiente `DEBUG`.
È possibile impostare le variabili per visualizzare i log di author e book come mostrato; sono supportati anche i caratteri jolly.

```bash
#Windows
set DEBUG=author,book

#Linux
export DEBUG="author,book"
```

> [!NOTE]
> Le chiamate a `debug` possono sostituire il logging precedentemente effettuato usando `console.log()` o `console.error()`. Sostituire nel codice tutte le chiamate a `console.log()` con il logging tramite il modulo [debug](https://www.npmjs.com/package/debug). Attivare e disattivare il logging nell'ambiente di sviluppo impostando la variabile DEBUG e osservare l'impatto sul logging.

Se occorre registrare l'attività del sito web, è possibile usare una libreria di logging come _Winston_ o _Bunyan_. Per maggiori informazioni su questo argomento, vedere: [Best practice per la produzione: prestazioni e affidabilità](https://expressjs.com/en/advanced/best-practice-performance/).

### Usare la compressione gzip/deflate per le risposte

I server web possono spesso comprimere la risposta HTTP inviata a un client, riducendo significativamente il tempo richiesto al client per ricevere e caricare la pagina. Il metodo di compressione usato dipenderà dai metodi di decompressione che il client dichiara di supportare nella richiesta; la risposta verrà inviata non compressa se non è supportato alcun metodo di compressione.

Aggiungere questa funzionalità al sito usando il middleware [compression](https://www.npmjs.com/package/compression). Installarlo nella directory radice del progetto eseguendo il comando seguente:

```bash
npm install compression
```

Aprire **./app.js** e richiedere la libreria compression come mostrato. Aggiungere la libreria compression alla catena di middleware con il metodo `use()`; questa dovrebbe comparire prima di tutte le route che si desidera comprimere, in questo caso tutte.

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
> Per un sito web di produzione ad alto traffico non verrebbe usato questo middleware. Si userebbe invece un reverse proxy come [Nginx](https://nginx.org/).

### Usare Helmet per proteggersi dalle vulnerabilità note

[Helmet](https://www.npmjs.com/package/helmet) è un pacchetto middleware. Può impostare header HTTP appropriati che aiutano a proteggere l'app da vulnerabilità web note; per maggiori informazioni sugli header impostati e sulle vulnerabilità da cui protegge, vedere la [documentazione](https://helmet.js.org/).

Installarlo nella directory radice del progetto eseguendo il comando seguente:

```bash
npm install helmet
```

Aprire **./app.js** e richiedere la libreria _helmet_ come mostrato.
Aggiungere quindi il modulo alla catena di middleware con il metodo `use()`.

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
      "script-src": ["'self'", "cdn.jsdelivr.net"],
    },
  }),
);

// …
```

Normalmente si potrebbe semplicemente inserire `app.use(helmet());` per aggiungere il _sottoinsieme_ degli header relativi alla sicurezza appropriati per la maggior parte dei siti.
Tuttavia, nel [template di base di LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/LocalLibrary_base_template) sono inclusi alcuni script bootstrap.
Questi violano la [Content Security Policy (CSP)](/it/docs/Web/HTTP/Guides/CSP) _predefinita_ di helmet, che non consente il caricamento di script cross-site.
Per consentire il caricamento di questi script, viene modificata la configurazione di helmet affinché imposti direttive CSP che consentano il caricamento di script dai domini indicati.
Per il proprio server è possibile aggiungere o disabilitare header specifici secondo necessità seguendo le [istruzioni per usare helmet](https://www.npmjs.com/package/helmet).

### Aggiungere la limitazione della frequenza alle route API

[Express-rate-limit](https://www.npmjs.com/package/express-rate-limit) è un pacchetto middleware che può essere usato per limitare le richieste ripetute alle API e agli endpoint.
Esistono molti motivi per cui potrebbero essere effettuate richieste eccessive al sito, come attacchi denial of service, attacchi brute force o anche soltanto un client o script che non si comporta come previsto.
Oltre ai problemi di prestazioni che possono sorgere quando troppe richieste rallentano il server, potrebbe essere addebitato anche il traffico aggiuntivo.
Questo pacchetto può essere usato per limitare il numero di richieste effettuabili verso una particolare route o insieme di route.

Installarlo nella directory radice del progetto eseguendo il comando seguente:

```bash
npm install express-rate-limit
```

Aprire **./app.js** e richiedere la libreria _express-rate-limit_ come mostrato.
Aggiungere quindi il modulo alla catena di middleware con il metodo `use()`.

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

Il comando precedente limita tutte le richieste a 20 al minuto; è possibile modificarlo secondo necessità.

> [!NOTE]
> Servizi di terze parti come [Cloudflare](https://www.cloudflare.com/) possono essere usati anche se serve una protezione più avanzata contro attacchi denial of service o altri tipi di attacchi.

#### Impostare la versione di node

Per le applicazioni node, inclusa Express, il file **package.json** contiene tutto ciò di cui un provider di hosting dovrebbe avere bisogno per determinare le dipendenze dell'applicazione e il file del punto di ingresso.

L'unica informazione importante mancante dal file **package.json** corrente è la versione di node richiesta dalla libreria.
È possibile trovare la versione di node usata per lo sviluppo immettendo il comando:

```bash
>node --version
v16.17.1
```

Aprire **package.json** e aggiungere queste informazioni come **engines > node**, come mostrato, usando il numero di versione del proprio sistema.

```json
{
  "engines": {
    "node": ">=22.0.0"
  }
}
```

Il servizio di hosting potrebbe non supportare la specifica versione indicata di node, ma questa modifica dovrebbe assicurare che tenti di usare una versione con lo stesso numero di versione principale, o una versione più recente.

Si noti che potrebbero esistere altri modi per specificare la versione di node su diversi servizi di hosting, ma l'approccio tramite **package.json** è ampiamente supportato.

#### Ottenere le dipendenze e testare di nuovo

Prima di procedere, testare nuovamente il sito e assicurarsi che non sia stato influenzato da alcuna modifica.

Innanzitutto, sarà necessario recuperare le dipendenze. È possibile farlo eseguendo il comando seguente nel terminale dalla directory radice del progetto:

```bash
npm install
```

Ora eseguire il sito, vedere [Testare le route](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/routes#testing_the_routes) per i comandi pertinenti, e verificare che il sito si comporti ancora come previsto.

### Creare un repository dell'applicazione in GitHub

Molti servizi di hosting permettono di importare e/o sincronizzare progetti da un repository locale o da piattaforme cloud di controllo della versione del codice sorgente.
Questo può rendere molto più semplici la distribuzione e lo sviluppo iterativo.

Per questo tutorial verranno configurati un account e un repository [GitHub](https://github.com/) per la libreria e verrà usato lo strumento **git** per caricare il codice sorgente.

> [!NOTE]
> È possibile saltare questo passaggio se si usa già GitHub per gestire il codice sorgente.
>
> Si noti che usare strumenti di gestione del codice sorgente è una buona pratica di sviluppo software, poiché consente di provare modifiche e di passare tra esperimenti e "codice noto e funzionante" quando necessario.

I passaggi sono:

1. Visitare <https://github.com/> e creare un account.
2. Dopo aver effettuato l'accesso, fare clic sul collegamento **+** nella barra degli strumenti superiore e selezionare **New repository**.
3. Compilare tutti i campi di questo modulo. Sebbene non siano obbligatori, sono fortemente consigliati.
   - Inserire un nuovo nome per il repository, ad esempio _express-locallibrary-tutorial_, e una descrizione, come "Local Library website written in Express".
   - Scegliere **Node** nell'elenco di selezione _Add .gitignore_.
   - Scegliere la licenza preferita nell'elenco di selezione _Add license_.
   - Selezionare **Initialize this repository with a README**.

   > [!WARNING]
   > L'accesso predefinito "Public" renderà visibile a chiunque su Internet _tutto_ il codice sorgente, inclusi nome utente e password del database. Assicurarsi che il codice sorgente legga le credenziali _solo_ dalle variabili d'ambiente e non contenga credenziali codificate direttamente.
   >
   > Altrimenti, selezionare l'opzione "Private" affinché soltanto persone selezionate possano vedere il codice sorgente.

4. Premere **Create repository**.
5. Fare clic sul pulsante verde **Clone or download** nella pagina del nuovo repository.
6. Copiare il valore dell'URL dal campo di testo nella finestra di dialogo visualizzata.
   Se è stato usato il nome del repository "express-locallibrary-tutorial", l'URL dovrebbe essere simile a: `https://github.com/<your_git_user_id>/express-locallibrary-tutorial.git`.

Ora che il repository, o "repo", è stato creato su GitHub, sarà necessario clonarlo, cioè copiarlo, sul computer locale:

1. Installare _git_ sul computer locale ([guida ufficiale al download di Git](https://git-scm.com/downloads/)).
2. Aprire un prompt dei comandi/terminale e clonare il repository usando l'URL copiato in precedenza:

   ```bash
   git clone https://github.com/<your_git_user_id>/express-locallibrary-tutorial.git
   ```

   Questo creerà il repository nella directory corrente.

3. Passare alla cartella del repository.

   ```bash
   cd express-locallibrary-tutorial
   ```

Quindi copiare i file sorgente dell'applicazione nella cartella del repository, renderli parte del repository usando _git_ e caricarli su GitHub:

1. Copiare l'applicazione Express in questa cartella, escludendo **/node_modules**, che contiene file di dipendenze da recuperare da npm secondo necessità.
2. Aprire un prompt dei comandi/terminale e usare il comando `add` per aggiungere tutti i file a git.

   ```bash
   git add -A
   ```

3. Usare il comando `status` per verificare che tutti i file da `commit` siano corretti: devono essere inclusi i file sorgente, non file binari, file temporanei e così via.
   Il risultato dovrebbe assomigliare all'elenco seguente.

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

4. Quando si è soddisfatti, eseguire il `commit` dei file nel repository locale.
   Questo equivale ad approvare le modifiche e renderle una parte ufficiale del repository locale.

   ```bash
   git commit -m "First version of application moved into GitHub"
   ```

5. A questo punto, il repository remoto non è stato modificato.
   L'ultimo passaggio consiste nel sincronizzare, con `push`, il repository locale con il repository GitHub remoto usando il comando seguente:

   ```bash
   git push origin main
   ```

Al termine dell'operazione, dovrebbe essere possibile tornare alla pagina GitHub in cui è stato creato il repository, aggiornare la pagina e vedere che l'intera applicazione è stata caricata. È possibile continuare ad aggiornare il repository quando cambiano i file usando questo ciclo add/commit/push.

Questo è un buon momento per effettuare un backup del progetto "vanilla": alcune delle modifiche che verranno apportate nelle sezioni successive potrebbero essere utili per la distribuzione su qualsiasi servizio di hosting, o per lo sviluppo, mentre altre potrebbero non esserlo.
È possibile farlo usando `git` nella riga di comando:

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
> Git è estremamente potente.
> Per saperne di più, vedere [Imparare Git](https://docs.github.com/en/get-started/start-your-journey/git-and-github-learning-resources).

## Esempio: hosting su Railway

Questa sezione fornisce una dimostrazione pratica di come installare _LocalLibrary_ su [Railway](https://railway.com/).

> [!NOTE]
> MDN ha migrato questo progetto da diversi servizi di hosting che non offrono più piani gratuiti.
> Per l'attuale opzione di hosting è stato deciso di usare Railway, che offre un piano hobby economico.
> La maggior parte dei servizi ha metodi di distribuzione simili, quindi le istruzioni seguenti dovrebbero aiutare a pubblicare il progetto sulla piattaforma scelta.

### Perché Railway?

Railway è un'opzione di hosting interessante per diversi motivi:

- Railway gestisce gran parte dell'infrastruttura, quindi non è necessario farlo autonomamente.
  Non doversi preoccupare di server, load balancer, reverse proxy e così via rende molto più facile iniziare.
- Railway si concentra sulla [developer experience per sviluppo e distribuzione](https://docs.railway.com/platform/compare-to-heroku), offrendo una curva di apprendimento più rapida e graduale rispetto a molte alternative.
- Le competenze e i concetti appresi usando Railway sono trasferibili.
  Sebbene Railway disponga di alcune ottime nuove funzionalità, altri servizi di hosting popolari usano molte delle stesse idee e degli stessi approcci.
- La [documentazione di Railway](https://docs.railway.com/) è chiara e completa.
- Offre un [Hobby Tier](https://railway.com/pricing) relativamente economico.
- Il servizio sembra essere molto affidabile e, se risulta particolarmente utile, i prezzi sono prevedibili e scalare l'app è molto facile.

Occorre dedicare del tempo a determinare se Railway è [adatto al proprio sito web](#scegliere_un_provider_di_hosting).

### Come funziona Railway?

Ogni applicazione web viene eseguita nel proprio container virtualizzato isolato e indipendente.
Per eseguire l'applicazione, Railway deve poter configurare l'ambiente e le dipendenze appropriati e capire inoltre come viene avviata.

Railway semplifica questa operazione, perché può riconoscere e installare automaticamente molti framework e ambienti per applicazioni web diversi in base all'uso di "convenzioni comuni".
Ad esempio, Railway riconosce le applicazioni node perché dispongono di un file **package.json** e può determinare il gestore di pacchetti usato per la compilazione dal file di "lock".
Per esempio, se l'applicazione include il file **package-lock.json**, Railway sa di dover usare _npm_ per installare i pacchetti, mentre se trova **yarn.lock** sa di dover usare _yarn_.
Dopo aver installato tutte le dipendenze, Railway cerca script denominati "build" e "start" nel file del pacchetto e li usa per compilare ed eseguire il codice.

> [!NOTE]
> Railway usa [Nixpacks](https://nixpacks.com/docs) per riconoscere vari framework di applicazioni web scritti in diversi linguaggi di programmazione.
> Non è necessario sapere altro per questo tutorial, ma è possibile approfondire le opzioni per distribuire applicazioni node in [Nixpacks Node](https://nixpacks.com/docs/providers/node).

Una volta in esecuzione, l'applicazione può configurarsi usando informazioni fornite nelle [variabili d'ambiente](https://docs.railway.com/variables).
Ad esempio, un'applicazione che usa un database deve ottenerne l'indirizzo tramite una variabile.
Il servizio di database stesso può essere ospitato da Railway o da un altro provider.

Gli sviluppatori interagiscono con Railway tramite il sito Railway e usando uno speciale strumento [Command Line Interface (CLI)](https://docs.railway.com/cli).
La CLI consente di associare un repository GitHub locale a un progetto Railway, caricare il repository dal branch locale al sito attivo, ispezionare i log del processo in esecuzione, impostare e ottenere variabili di configurazione e molto altro.
Una delle funzionalità più utili è la possibilità di usare la CLI per eseguire il progetto locale con le stesse variabili d'ambiente del progetto attivo.

Questa è tutta la panoramica necessaria per distribuire l'app su Railway.
Successivamente verranno configurati un account Railway, il sito web e un database, quindi verrà provato il client Railway.

### Ottenere un account Railway

Per iniziare a usare Railway occorre innanzitutto creare un account:

- Andare su [railway.com](https://railway.com/) e fare clic sul collegamento **Login** nella barra degli strumenti superiore.
- Selezionare GitHub nella finestra popup per accedere usando le credenziali GitHub.
- Potrebbe quindi essere necessario verificare l'account tramite email.
- Verrà quindi effettuato l'accesso alla dashboard di Railway.com: <https://railway.com/dashboard>.

### Distribuire su Railway da GitHub

Ora verrà configurato Railway per distribuire la libreria da GitHub.
Per prima cosa scegliere l'opzione **Dashboard** nel menu superiore del sito, quindi selezionare il pulsante **New Project**:

![Dashboard del sito Railway che mostra il pulsante per un nuovo progetto](railway_new_project_button.png)

Railway visualizzerà un elenco di opzioni per il nuovo progetto, inclusa l'opzione per distribuire un progetto da un template che viene prima creato nell'account GitHub, e diversi database.
Selezionare **Deploy from GitHub repo**.

![Finestra popup di Railway che mostra le opzioni di distribuzione con l'opzione Deploy from GitHub repo evidenziata](railway_new_project_button_deploy_github_repo.png)

Vengono visualizzati tutti i progetti nei repository GitHub condivisi con Railway durante la configurazione.
Selezionare il repository GitHub per la libreria locale: `<user-name>/express-locallibrary-tutorial`.

![Finestra popup di Railway che mostra i repository GitHub distribuibili](railway_new_project_button_deploy_github_selectrepo.png)

Confermare la distribuzione selezionando **Deploy Now**.

![Schermata di conferma in cui è possibile selezionare la distribuzione del progetto](railway_new_project_deploy_confirm.png)

Railway caricherà e distribuirà quindi il progetto, mostrando l'avanzamento nella scheda delle distribuzioni.
Quando la distribuzione viene completata correttamente, sarà visualizzata una schermata come quella seguente.

![Dashboard di Railway che mostra la scheda Deployments per il progetto distribuito](railway_project_deploy.png)

Ora selezionare la scheda _Settings_, quindi scorrere fino alla sezione Domains e premere il pulsante **Generate Domain**.

![Scheda delle impostazioni del progetto Railway che mostra il pulsante per generare un dominio](railway_project_generate_domain.png)

Questo pubblicherà il sito e sostituirà il pulsante con il dominio, come mostrato di seguito.

![Scheda delle impostazioni del progetto Railway che mostra un collegamento al sito LocalLibrary](railway_project_domain.png)

Selezionare l'URL del dominio per aprire l'applicazione della libreria.
Si noti che, poiché non è stato specificato un database di produzione, la libreria locale verrà aperta usando i dati di sviluppo.

### Provisioning e connessione di un database MongoDB

Anziché usare i dati di sviluppo, creare ora un database MongoDB di produzione da usare.
Il database verrà creato come parte del progetto dell'applicazione Railway, anche se nulla impedisce di crearlo in un progetto separato oppure di usare un database _MongoDB Atlas_ per i dati di produzione, proprio come per il database di sviluppo.

In Railway, scegliere l'opzione **Dashboard** dal menu superiore del sito, quindi selezionare il progetto dell'applicazione.
In questa fase contiene un singolo servizio per l'applicazione, che può essere selezionato per impostare variabili e altri dettagli del servizio.
Selezionare il pulsante **New**, usato per aggiungere servizi al progetto corrente.

![Progetto Railway con il pulsante per un nuovo servizio evidenziato](railway_project_open_no_database.png)

Selezionare **Database** quando viene richiesto il tipo di servizio da aggiungere:

![Finestra popup di Railway che mostra le opzioni per un nuovo servizio, come database, repository GitHub, servizio vuoto e così via](railway_database_add.png)

Quindi selezionare **Add MongoDB** per iniziare ad aggiungere il database.

![Finestra popup di Railway che mostra diversi database selezionabili: Postgres, MySQL, MongoDB e così via](railway_database_select_type.png)

Railway effettuerà quindi il provisioning di un servizio contenente un database vuoto nello stesso progetto.
Al completamento, nella vista del progetto saranno ora presenti sia i servizi dell'applicazione sia quelli del database.

![Progetto Railway con i servizi dell'applicazione e del database](railway_project_two_services.png)

Selezionare il servizio MongoDB per visualizzare le informazioni sul database.
Aprire la scheda _Variables_ e copiare il valore "Mongo_URL", ovvero l'indirizzo del database.

![Schermata delle impostazioni del database Railway che mostra l'URL necessario per connettersi al database](railway_mongodb_connect.png)

Per rendere questo valore accessibile all'applicazione della libreria, occorre aggiungerlo al processo dell'applicazione usando una variabile d'ambiente.
Per prima cosa aprire il servizio dell'applicazione.
Quindi selezionare la scheda _Variables_ e premere il pulsante **New Variable**.

Immettere il nome della variabile `MONGODB_URI` e l'URL di connessione copiato per il database; `MONGODB_URI` è il nome della variabile d'ambiente dalla quale [è stata configurata l'applicazione](#configurazione_del_database) per leggere l'indirizzo del database.
Il risultato sarà simile alla schermata mostrata di seguito.

![Schermata delle variabili del sito Railway durante l'aggiunta della variabile MONGODB_URI e del relativo indirizzo](railway_variables_database_url.png)

Selezionare **Add** per aggiungere la variabile.

Railway riavvia l'app quando aggiorna le variabili. Se si controlla ora la pagina iniziale, dovrebbe mostrare valori zero per i conteggi degli oggetti, poiché le modifiche precedenti implicano che ora venga usato un database nuovo e vuoto.

### Altre variabili di configurazione

Come ricordato in una sezione precedente, occorre [impostare NODE_ENV su 'production'](#set_node_env_to_production) per migliorare le prestazioni e generare messaggi di errore meno dettagliati. È possibile farlo nella stessa schermata usata per impostare la variabile `MONGODB_URI`.

Aprire il servizio dell'applicazione.
Quindi selezionare la scheda _Variables_, dove sarà già definita `MONGODB_URI`, e premere il pulsante **New Variable**.

![Scheda Variables di Railway con il pulsante New Variable evidenziato](railway_variables_new.png)

Immettere `NODE_ENV` come nome della nuova variabile e `production` come nome dell'ambiente.
Quindi premere il pulsante **Add**.

![Scheda Variables di Railway con la nuova variabile NODE_ENV impostata su 'production'](railway_variables_new_node_env.png)

L'applicazione della libreria locale è ora configurata per l'uso in produzione.
È possibile aggiungere dati tramite l'interfaccia del sito web e dovrebbe funzionare nello stesso modo in cui funzionava durante lo sviluppo, sebbene con meno informazioni di debug esposte per le pagine non valide.

> [!NOTE]
> Se si desidera soltanto aggiungere alcuni dati per il test, si potrebbe usare lo script `populatedb`, con l'URL del database MongoDB di produzione, come illustrato nella sezione [Tutorial su Express - Parte 3: Usare un database (con Mongoose), test — creare alcuni elementi](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose#testing_%E2%80%94_create_some_items).

### Installare il client

Scaricare e installare il client Railway per il sistema operativo locale seguendo le [istruzioni disponibili qui](https://docs.railway.com/cli).

Dopo l'installazione del client sarà possibile eseguire comandi.
Tra le operazioni più importanti vi sono la distribuzione della directory corrente del computer in un progetto Railway associato, senza dover caricare su GitHub, e l'esecuzione del progetto localmente usando le stesse impostazioni del server di produzione.

È possibile ottenere un elenco di tutti i comandi disponibili immettendo quanto segue in un terminale.

```bash
railway help
```

### Debug

Il client Railway fornisce il comando logs per mostrare la parte finale dei log; per ciascun progetto è disponibile un log più completo sul sito:

```bash
railway logs
```

## Riepilogo

Questo conclude il tutorial sulla configurazione delle app Express in produzione e anche la serie di tutorial sul lavoro con Express. Si spera che siano stati utili. È possibile consultare una versione completamente sviluppata del [codice sorgente su GitHub](https://github.com/mdn/express-locallibrary-tutorial).

## Vedere anche

- [Best practice per la produzione: prestazioni e affidabilità](https://expressjs.com/en/advanced/best-practice-performance/) (documentazione di Express)
- [Best practice per la produzione: sicurezza](https://expressjs.com/en/advanced/best-practice-security/) (documentazione di Express)
- Documentazione Railway
  - [CLI](https://docs.railway.com/cli)

- DigitalOcean
  - Tutorial su [Express](https://www.digitalocean.com/community/tutorials?q=express)
  - Tutorial su [Node.js](https://www.digitalocean.com/community/tutorials?q=node.js)

- Heroku
  - [Introduzione a Heroku con Node.js](https://devcenter.heroku.com/articles/getting-started-with-nodejs) (documentazione di Heroku)
  - [Distribuire applicazioni Node.js su Heroku](https://devcenter.heroku.com/articles/deploying-nodejs) (documentazione di Heroku)
  - [Supporto Node.js di Heroku](https://devcenter.heroku.com/articles/nodejs-support) (documentazione di Heroku)
  - [Ottimizzare la concorrenza delle applicazioni Node.js](https://devcenter.heroku.com/articles/node-concurrency) (documentazione di Heroku)
  - [Come funziona Heroku](https://devcenter.heroku.com/articles/how-heroku-works) (documentazione di Heroku)
  - [Dyno e Dyno Manager](https://devcenter.heroku.com/articles/dynos) (documentazione di Heroku)
  - [Configurazione e Config Vars](https://devcenter.heroku.com/articles/config-vars) (documentazione di Heroku)
  - [Limiti](https://devcenter.heroku.com/articles/limits) (documentazione di Heroku)

{{PreviousMenu("Learn_web_development/Extensions/Server-side/Express_Nodejs/forms", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}
