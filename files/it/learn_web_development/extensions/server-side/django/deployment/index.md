---
title: "Django Tutorial Parte 11: Distribuire Django in produzione"
short-title: "11: Distribuire"
slug: Learn_web_development/Extensions/Server-side/Django/Deployment
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Testing", "Learn_web_development/Extensions/Server-side/Django/web_application_security", "Learn_web_development/Extensions/Server-side/Django")}}

Ora che ha creato (e testato) un fantastico sito web per la [biblioteca locale](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website), vorrà installarlo su un server web pubblico in modo che il personale e i membri della biblioteca possano accedervi tramite internet. Questo articolo fornisce una panoramica su come potrebbe procedere a trovare un host per distribuire il suo sito web e cosa deve fare per preparare il suo sito alla produzione.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Completare tutti gli argomenti dei tutorial precedenti, incluso <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/Testing">Django Tutorial Parte 10: Testare un'applicazione web Django</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Imparare dove e come può distribuire un'app Django in produzione.</td>
    </tr>
  </tbody>
</table>

## Panoramica

Una volta che il suo sito è finito (o abbastanza finito per iniziare i test pubblici) dovrà ospitarlo da qualche parte più pubblica e accessibile rispetto al suo computer personale di sviluppo.

Fino ad ora ha lavorato in un ambiente di sviluppo, utilizzando il server di sviluppo Django per condividere il suo sito con il browser/rete locale e eseguendo il suo sito web con impostazioni di sviluppo (insicure) che espongono il debug e altre informazioni private. Prima di poter ospitare un sito web esternamente, dovrà prima fare:

- Alcune modifiche ai suoi settings del progetto.
- Scegliere un ambiente per l'hosting dell'app Django.
- Scegliere un ambiente per l'hosting di eventuali file statici.
- Configurare un'infrastruttura a livello di produzione per servire il suo sito web.

Questo tutorial fornisce alcune indicazioni sulle opzioni per scegliere un sito di hosting, una breve panoramica su ciò che deve fare per preparare la sua app Django per la produzione e un esempio funzionante di come installare il sito Web della biblioteca locale sul servizio di cloud hosting [Railway](https://railway.com/).

## Cos'è un ambiente di produzione?

L'ambiente di produzione è l'ambiente fornito dal computer server dove il suo sito web sarà eseguito per il consumo esterno. L'ambiente include:

- Hardware del computer su cui il sito web viene eseguito.
- Sistema operativo (ad esempio, Linux, Windows).
- Runtime del linguaggio di programmazione e librerie del framework in cima al quale il suo sito web è scritto.
- Server web utilizzato per servire pagine e altri contenuti (ad esempio, Nginx, Apache).
- Server di applicazione che passa richieste "dinamiche" tra il suo sito web Django e il server web.
- Database da cui il suo sito web dipende.

> [!NOTE]
> A seconda di come è configurato il suo ambiente di produzione, potrebbe avere anche un reverse proxy, un bilanciatore di carico, e così via.

Il computer server potrebbe trovarsi nei suoi locali e connettersi a Internet tramite un collegamento veloce, ma è molto più comune utilizzare un computer ospitato "nel cloud". Ciò significa che il suo codice viene eseguito su qualche computer remoto (o possibilmente un computer "virtuale") nel data center della sua azienda di hosting. Il server remoto offrirà di solito un livello garantito di risorse di calcolo (CPU, RAM, memoria di archiviazione, ecc.) e connettività Internet per un certo prezzo.

Questo tipo di hardware di calcolo/rete accessibile da remoto è chiamato _Infrastructure as a Service (IaaS)_. Molti fornitori di IaaS offrono opzioni per preinstallare un particolare sistema operativo, sul quale deve installare gli altri componenti del suo ambiente di produzione. Altri fornitori le consentono di selezionare ambienti più completi, forse inclusi un setup completo di Django e server web.

> [!NOTE]
> Gli ambienti preconfigurati possono rendere la configurazione del suo sito web molto semplice poiché riducono la configurazione, ma le opzioni disponibili potrebbero limitarla a un server sconosciuto (o altri componenti) e potrebbero basarsi su una versione più vecchia del sistema operativo. Spesso è meglio installare lei stesso i componenti, in modo da avere quelli che desidera, e quando è necessario aggiornare parti del sistema, ha un'idea di dove cominciare!

Altri fornitori di hosting supportano Django come parte di un'offerta _Platform as a Service_ (PaaS). In questo tipo di hosting non deve preoccuparsi della maggior parte del suo ambiente di produzione (server web, server applicazioni, bilanciatori di carico) poiché la piattaforma host si occupa di questi per lei — insieme alla maggior parte di ciò che deve fare per scalare la sua applicazione.
Ciò rende la distribuzione molto semplice, perché deve solo concentrarsi sulla sua applicazione web e non su tutta l'altra infrastruttura del server.

Alcuni sviluppatori sceglieranno la flessibilità aumentata offerta dall'IaaS rispetto al PaaS, mentre altri apprezzeranno la riduzione della manutenzione e la scalabilità più facile del PaaS. Quando inizia, configurare il suo sito su un sistema PaaS è molto più semplice, ed è quindi ciò che faremo in questo tutorial.

> [!NOTE]
> Se sceglie un fornitore di hosting compatibile con Python/Django, dovrebbero fornirle le istruzioni su come configurare un sito web Django usando diverse configurazioni di server web, server applicazioni, reverse proxy. (questo non sarà rilevante se si sceglie un PaaS). Ad esempio, ci sono molte guide passo-passo per varie configurazioni nei [documenti della community Django di DigitalOcean](https://www.digitalocean.com/community/tutorials?q=django).

## Scegliere un fornitore di hosting

Ci sono molti fornitori di hosting che sono noti per supportare attivamente o lavorare bene con Django, tra cui: [Heroku](https://www.heroku.com/), [DigitalOcean](https://www.digitalocean.com/), [Railway](https://railway.com/), [Python Anywhere](https://www.pythonanywhere.com/), [Amazon Web Services](https://aws.amazon.com/), [Azure](https://azure.microsoft.com/en-us/), [Google Cloud](https://cloud.google.com/), [Hetzner](https://www.hetzner.com/), e [Vultr Cloud Compute](https://blogs.vultr.com/new-free-tier-plan) — per citarne solo alcuni.
Questi fornitori forniscono diversi tipi di ambienti (IaaS, PaaS), e diversi livelli di risorse di calcolo e rete a diversi prezzi.

Alcune delle cose da considerare quando sceglie un host:

- Quanto traffico si aspetta che il suo sito riceverà e il costo dei dati e delle risorse di calcolo necessarie per soddisfare quella domanda.
- Livello di supporto per la scalabilità orizzontale (aggiungendo più macchine) e verticale (aggiornando a macchine più potenti) e i costi per farlo.
- Dove il fornitore ha i data center e quindi dove l'accesso sarà probabilmente più veloce.
- Storico delle prestazioni di uptime e downtime dell'host.
- Strumenti forniti per la gestione del sito — sono facili da usare e sicuri (ad esempio, SFTP vs. FTP).
- Framework integrati per il monitoraggio del suo server.
- Limitazioni note. Alcuni host bloccheranno deliberatamente alcuni servizi (ad esempio, email). Altri offrono solo un certo numero di ore di "tempo di vita" in alcuni livelli di prezzo, o offrono solo una piccola quantità di spazio di archiviazione.
- Benefici aggiuntivi. Alcuni fornitori offriranno nomi di dominio gratuiti e supporto per certificati TLS che altrimenti dovrebbe pagare.
- Se il livello "gratuito" su cui si sta basando scade nel tempo, e se il costo della migrazione a un livello più costoso significa che sarebbe stato meglio utilizzare un altro servizio in primo luogo!

La buona notizia quando si inizia è che ci sono parecchi siti che forniscono ambienti di calcolo "gratuiti" che sono destinati alla valutazione e al testing.
Questi sono di solito ambienti abbastanza limitati in risorse, ma deve essere consapevole che potrebbero scadere dopo un certo periodo introduttivo o avere altri vincoli. Tuttavia, sono ottimi per testare siti a basso traffico in un ambiente ospitato e possono offrire una facile migrazione per pagare per più risorse quando il suo sito diventa più frequentato.
Le scelte popolari in questa categoria includono [Vultr Cloud Compute](https://blogs.vultr.com/new-free-tier-plan), [Python Anywhere](https://www.pythonanywhere.com/), [Amazon Web Services](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/billing-free-tier.html), [Microsoft Azure](https://azure.microsoft.com/en-us/pricing/details/app-service/linux/), e così via.

La maggior parte dei fornitori offre anche un livello "base" pensato per piccoli siti in produzione, che offre livelli più utili di potenza di calcolo e meno limitazioni.
[Railway](https://railway.com/), [Heroku](https://www.heroku.com/), e [DigitalOcean](https://www.digitalocean.com/) sono esempi di fornitori di hosting popolari che hanno un livello di calcolo di base relativamente economico (nella fascia di $5 a $10 USD al mese).

> [!NOTE]
> Ricorda che il prezzo non è l'unico criterio di selezione. Se il suo sito web ha successo, può emergere che la scalabilità sia la considerazione più importante.

## Preparare il suo sito per la pubblicazione

La [struttura base del sito Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/skeleton_website) creata utilizzando gli strumenti _django-admin_ e _manage.py_ è configurata per facilitare lo sviluppo. Molte delle impostazioni del progetto Django (specificate in **settings.py**) dovrebbero essere diverse per la produzione, sia per motivi di sicurezza che di prestazioni.

> [!NOTE]
> È comune avere un file **settings.py** separato per la produzione, e/o importare condizionalmente impostazioni sensibili da un file separato o una variabile d'ambiente. Questo file dovrebbe poi essere protetto, anche se il resto del codice sorgente è disponibile su un repository pubblico.

Le impostazioni critiche che deve controllare sono:

- `DEBUG`. Questo dovrebbe essere impostato su `False` in produzione (`DEBUG = False`). Ciò impedisce che le tracce di debug sensibili/confidenziali e le informazioni delle variabili siano visualizzate.
- `SECRET_KEY`. Questo è un grande valore casuale usato per la protezione CSRF, ecc. È importante che la chiave usata in produzione non sia sotto controllo del codice sorgente o accessibile al di fuori del server di produzione.

I documenti di Django suggeriscono che le informazioni segrete potrebbero essere caricate al meglio da una variabile d'ambiente o lette da un file accessibile solo dal server.
Modifichiamo l'applicazione _LocalLibrary_ in modo da leggere le nostre variabili `SECRET_KEY` e `DEBUG` dalle variabili d'ambiente, se sono definite, passando ai valori definiti in un file **.env** nella radice, e infine utilizzando i valori predefiniti nel file di configurazione.
Questo è molto flessibile in quanto consente qualsiasi configurazione supportata dal server di hosting.

Per leggere i valori dell'ambiente da un file utilizzeremo [python-dotenv](https://pypi.org/project/python-dotenv/).
Questa è una libreria per leggere coppie chiave-valore da un file e usarle come variabili d'ambiente, ma solo se la corrispondente variabile d'ambiente non è definita.

Installa la libreria nel suo ambiente virtuale come mostrato (e aggiorna anche il file `requirements.txt`):

```bash
pip3 install python-dotenv
```

Poi apra **/locallibrary/settings.py** e inserisca il codice seguente dopo che `BASE_DIR` è definito, ma prima dell'avvertimento sulla sicurezza: `# SECURITY WARNING: keep the secret key used in production secret!`

```python
# Support env variables from .env file if defined
import os
from dotenv import load_dotenv
env_path = load_dotenv(os.path.join(BASE_DIR, '.env'))
load_dotenv(env_path)
```

Questo carica il file `.env` dalla radice dell'applicazione web.
Le variabili definite come `KEY=VALUE` nel file sono importate quando la chiave è utilizzata in `os.environ.get('<KEY>'', '<DEFAULT VALUE>')`, se definita.

> [!NOTE]
> Tutti i valori che aggiunge a **.env** potrebbero essere _segreti_!
> Non deve salvarli su GitHub, e dovrebbe aggiungere `.env` al suo file `.gitignore` affinché non venga aggiunto per errore.

Quindi disabiliti la configurazione originale `SECRET_KEY` e aggiunga le nuove righe come mostrato di seguito.
Durante lo sviluppo non sarà specificata alcuna variabile ambientale per la chiave, quindi verrà utilizzato il valore predefinito (non dovrebbe importare quale chiave usa qui, o se la chiave "fugga", perché non la utilizzerà in produzione).

```python
# SECURITY WARNING: keep the secret key used in production secret!
# SECRET_KEY = 'django-insecure-&psk#na5l=p3q8_a+-$4w1f^lt3lx1c@d*p4x$ymm_rn7pwb87'
import os
SECRET_KEY = os.environ.get('DJANGO_SECRET_KEY', 'django-insecure-&psk#na5l=p3q8_a+-$4w1f^lt3lx1c@d*p4x$ymm_rn7pwb87')
```

Poi commenti l'impostazione esistente `DEBUG` e aggiunga la nuova riga mostrata di seguito.

```python
# SECURITY WARNING: don't run with debug turned on in production!
# DEBUG = True
DEBUG = os.environ.get('DJANGO_DEBUG', '') != 'False'
```

Il valore di `DEBUG` sarà `True` per impostazione predefinita, ma sarà solo `False` se il valore della variabile d'ambiente `DJANGO_DEBUG` viene impostato su `False` o `DJANGO_DEBUG=False` è impostato nel file **.env**.
Si noti che le variabili d'ambiente sono stringhe e non tipi Python. Dobbiamo quindi confrontare le stringhe. L'unico modo per impostare la variabile `DEBUG` su `False` è effettivamente impostarla sulla stringa `False`.

Può impostare la variabile d'ambiente su "False" su Linux emettendo il seguente comando:

```bash
export DJANGO_DEBUG=False
```

Un elenco completo delle impostazioni che potrebbe voler cambiare è fornito nel [Checklist di distribuzione](https://docs.djangoproject.com/en/5.0/howto/deployment/checklist/) (documenti Django). Può anche elencare un certo numero di queste utilizzando il comando del terminale qui sotto:

```python
python3 manage.py check --deploy
```

### Gunicorn

[Gunicorn](https://gunicorn.org/) è un server HTTP puro-Python comunemente usato per servire applicazioni WSGI Django.

Sebbene non ci serva _Gunicorn_ per servire la nostra applicazione LocalLibrary durante lo sviluppo, lo installeremo localmente affinché diventi parte dei nostri [requirements](#requisiti) quando l'applicazione viene distribuita.

Prima si assicuri di essere nell'ambiente virtuale Python creato quando ha [configurato l'ambiente di sviluppo](/it/docs/Learn_web_development/Extensions/Server-side/Django/development_environment) (utilizzi il comando `workon [name-of-virtual-environment]`).
Poi installi _Gunicorn_ localmente dalla riga di comando usando _pip_:

```bash
pip3 install gunicorn
```

### Configurazione del database

SQLite, il database predefinito di Django che ha utilizzato per lo sviluppo, è una scelta ragionevole per siti web di piccole e medie dimensioni.
Purtroppo non può essere usato su alcuni servizi di hosting popolari, come Heroku, perché non forniscono archiviazione dati persistente nell'ambiente applicativo (una richiesta di SQLite).
Sebbene ciò potrebbe non influenzarci per la distribuzione dell'esempio, le mostreremo un altro approccio che funzionerà su Railway, Heroku e alcuni altri servizi.

L'approccio consiste nell'utilizzare un database che viene eseguito nel suo processo da qualche parte su Internet e viene accesso dall'applicazione libraria Django usando un indirizzo passato come variabile d'ambiente.
In questo caso utilizzeremo un database Postgres anch'esso ospitato su Railway, ma potrebbe usare qualsiasi servizio di hosting di database che desidera.

Le informazioni di connessione al database verranno fornite a Django utilizzando una variabile d'ambiente chiamata `DATABASE_URL`.
Piuttosto che codificare queste informazioni in Django, utilizzeremo il pacchetto [dj-database-url](https://pypi.org/project/dj-database-url/) per analizzare la variabile d'ambiente `DATABASE_URL` e convertirla automaticamente nel formato di configurazione desiderato da Django.
Oltre a installare il pacchetto _dj-database-url_ dovremo anche installare [psycopg2](https://www.psycopg.org/), poiché Django ha bisogno di questo per interagire con i database Postgres.

#### dj-database-url

_dj-database-url_ è utilizzato per estrarre la configurazione del database Django da una variabile d'ambiente.

Installalo localmente affinché diventi parte dei nostri [requirements](#requisiti) per impostare sul server di distribuzione:

```bash
pip3 install dj-database-url
```

#### settings.py

Apri **/locallibrary/settings.py** e copia la seguente configurazione nella parte inferiore del file:

```python
# Update database configuration from $DATABASE_URL environment variable (if defined)
import dj_database_url

if 'DATABASE_URL' in os.environ:
    DATABASES['default'] = dj_database_url.config(
        conn_max_age=500,
        conn_health_checks=True,
    )
```

Django utilizzerà ora la configurazione del database in `DATABASE_URL` se la variabile d'ambiente è impostata; altrimenti utilizzerà il database SQLite predefinito.
Il valore `conn_max_age=500` rende la connessione persistente, il che è molto più efficiente rispetto a ricreare la connessione su ogni ciclo di richiesta (questo è facoltativo e può essere rimosso se necessario).

#### psycopg2

<!-- Django 4.2 ora supporta Psycopg (3): https://docs.djangoproject.com/en/5.0/releases/4.2/#psycopg-3-support
  Ma non funziona su Railway!
  Prova di nuovo ad aggiornare nella prossima versione.
-->

Django ha bisogno del pacchetto _psycopg2_ per lavorare con i database Postgres.
Installalo localmente affinché diventi parte dei nostri [requirements](#requisiti) per Railway sul server remoto:

```bash
pip3 install psycopg2-binary
```

Si noti che Django userà il database SQLite durante lo sviluppo per impostazione predefinita, a meno che `DATABASE_URL` non sia impostato.
Può passare completamente a Postgres e utilizzare lo stesso database ospitato per sviluppo e produzione impostando la stessa variabile d'ambiente nel suo ambiente di sviluppo (Railway semplifica l'utilizzo dello stesso ambiente per produzione e sviluppo).
In alternativa può anche installare e utilizzare un [database Postgres autogestito](https://www.psycopg.org/docs/install.html) sul suo computer locale.

### Servire file statici in produzione

Durante lo sviluppo utilizziamo Django e il server web di sviluppo Django per servire sia i nostri HTML dinamici sia i nostri file statici (CSS, JavaScript, ecc.).
Ciò è inefficiente per i file statici, perché le richieste devono passare attraverso Django anche se Django non fa nulla con loro.
Se ciò non importa durante lo sviluppo, avrebbe un impatto significativo sulle prestazioni se utilizzassimo lo stesso approccio in produzione.

Nell'ambiente di produzione separiamo tipicamente i file statici dall'applicazione web Django, rendendo più semplice servirli direttamente dal server web o da una rete di distribuzione dei contenuti (CDN).

Le variabili importanti delle impostazioni sono:

- `STATIC_URL`: Questo è il percorso URL di base da cui verranno serviti i file statici, ad esempio su un CDN.
- `STATIC_ROOT`: Questo è il percorso assoluto verso una directory in cui lo strumento _collectstatic_ di Django raccoglierà i file statici referenziati nei nostri template. Una volta raccolti, possono essere quindi caricati in gruppo ovunque i file debbano essere ospitati.
- `STATICFILES_DIRS`: Questo elenco contiene directory aggiuntive che lo strumento _collectstatic_ di Django deve cercare per i file statici.

I template Django fanno riferimento alle posizioni dei file statici relative ad un tag `static` (può vederlo nel template di base definito in [Django Tutorial Parte 5: Creare la nostra home page](/it/docs/Learn_web_development/Extensions/Server-side/Django/Home_page#the_locallibrary_base_template)), che a sua volta scansiona l'impostazione `STATIC_URL`.
I file statici possono quindi essere caricati su qualsiasi host e può aggiornare l'applicazione per trovarli utilizzando questa impostazione.

Lo strumento _collectstatic_ è utilizzato per raccogliere i file statici nella cartella definita dall'impostazione `STATIC_ROOT` del progetto.
Viene chiamato con il seguente comando:

```bash
python3 manage.py collectstatic
```

Per questo tutorial, _collectstatic_ può essere eseguito prima che l'applicazione venga caricata, copiando tutti i file statici nell'applicazione nella posizione specificata in `STATIC_ROOT`.
Successivamente, `Whitenoise` trova i file dalla posizione definita da `STATIC_ROOT` (per impostazione predefinita) e li serve all'URL base definito da `STATIC_URL`.

#### settings.py

Apri **/locallibrary/settings.py** e copia la seguente configurazione nella parte inferiore del file.
Il `BASE_DIR` dovrebbe già essere stato definito nel suo file (lo `STATIC_URL` potrebbe già essere stato definito all'interno del file quando è stato creato.
Nonostante non causi danni, dovreste comunque eliminare il precedente riferimento duplicato).

```python
# Static files (CSS, JavaScript, Images)
# https://docs.djangoproject.com/en/5.0/howto/static-files/

# The absolute path to the directory where collectstatic will collect static files for deployment.
STATIC_ROOT = BASE_DIR / 'staticfiles'

# The URL to use when referring to static files (where they will be served from)
STATIC_URL = '/static/'
```

Infatti, faremo il servizio dei file utilizzando una libreria chiamata [WhiteNoise](https://pypi.org/project/whitenoise/), che installiamo e configuriamo nella sezione successiva.

### Whitenoise

Ci sono molti modi per servire file statici in produzione (abbiamo visto le impostazioni di Django rilevanti nelle sezioni precedenti).
Il progetto [WhiteNoise](https://pypi.org/project/whitenoise/) fornisce uno dei metodi più semplici per servire direttamente asset statici da Gunicorn in produzione.

Consultate la documentazione di [WhiteNoise](https://pypi.org/project/whitenoise/) per una spiegazione su come funziona e perché l'implementazione è un metodo relativamente efficiente per servire questi file.

I passaggi per impostare _WhiteNoise_ per il progetto sono [forniti qui](https://whitenoise.readthedocs.io/en/stable/django.html) (e riprodotti di seguito):

#### Installa whitenoise

Installa whitenoise localmente utilizzando il seguente comando:

```bash
pip3 install whitenoise
```

#### settings.py

Per installare _WhiteNoise_ nella sua applicazione Django, apri **/locallibrary/settings.py**, trova l'impostazione `MIDDLEWARE` e aggiungi `WhiteNoiseMiddleware` vicino alla parte superiore dell'elenco, subito sotto `SecurityMiddleware`:

```python
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'whitenoise.middleware.WhiteNoiseMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]
```

Facoltativamente, può ridurre la dimensione dei file statici quando vengono serviti (ciò è più efficiente).
Basta aggiungere quanto segue alla parte inferiore di **/locallibrary/settings.py**:

```python
# Static file serving.
# https://whitenoise.readthedocs.io/en/stable/django.html#add-compression-and-caching-support
STORAGES = {
    # ...
    "staticfiles": {
        "BACKEND": "whitenoise.storage.CompressedManifestStaticFilesStorage",
    },
}
```

Non deve fare nient'altro per configurare _WhiteNoise_ perché utilizza le impostazioni del suo progetto per `STATIC_ROOT` e `STATIC_URL` per impostazione predefinita.

### Requisiti

I requisiti Python della sua applicazione web dovrebbero essere archiviati in un file **requirements.txt** nella radice del suo repository.
Molti servizi di hosting installeranno automaticamente le dipendenze in questo file (in altri dovrà farlo lei stesso).
Può creare questo file utilizzando _pip_ dalla riga di comando (eseguendo il seguente comando nella radice del repo):

```bash
pip3 freeze > requirements.txt
```

Dopo aver installato tutte le diverse dipendenze sopra, il suo file **requirements.txt** dovrebbe avere _almeno_ questi elementi elencati (sebbene i numeri di versione potrebbero essere diversi).
Elimini pur favore qualsiasi altra dipendenza non elencata di seguito, a meno che non l'abbia aggiunta esplicitamente per questa applicazione.

```plain
Django==5.0.2
dj-database-url==2.1.0
gunicorn==21.2.0
psycopg2-binary==2.9.9
wheel==0.38.1
whitenoise==6.6.0
python-dotenv==1.0.1
```

### Aggiornare il suo repository di applicazioni su GitHub

Molti servizi di hosting consentono di importare e/o sincronizzare progetti da un repository locale o da piattaforme di controllo versione basate su cloud.
Ciò può semplificare notevolmente la distribuzione e lo sviluppo iterativo.

Dovrebbe già utilizzare GitHub per archiviare il codice sorgente della biblioteca locale (questo è stato impostato nel [gestione del codice sorgente con Git e GitHub](/it/docs/Learn_web_development/Extensions/Server-side/Django/development_environment#source_code_management_with_git_and_github) come parte della configurazione del suo ambiente di sviluppo).

Questo è un buon punto per fare il backup del suo progetto "vanilla" — mentre alcune delle modifiche che stiamo per fare nelle sezioni successive potrebbero essere utili per la distribuzione su qualsiasi servizio di hosting (o per lo sviluppo) altre potrebbero non esserlo.
Assumendo che abbia già effettuato il backup di tutti i cambiamenti fatti finora al ramo `main` su GitHub, può creare un nuovo ramo per fare il backup delle sue modifiche come mostrato:

```bash
# Fetch the latest main branch
git checkout main
git pull origin main

# Create branch vanilla_deployment from the current branch (main)
git checkout -b vanilla_deployment

# Push the new branch to GitHub
git push origin vanilla_deployment

# Switch back to main
git checkout main

# Make any further changes in a new branch
git checkout -b my_changes_for_deployment # Create a new branch
```

## Esempio: Hosting su PythonAnywhere

Questa sezione fornisce una dimostrazione pratica su come ospitare _LocalLibrary_ su [PythonAnywhere](https://www.pythonanywhere.com/).

### Perché PythonAnywhere?

Abbiamo scelto di utilizzare PythonAnywhere per diversi motivi:

- PythonAnywhere offre un [piano per principianti gratuito](https://www.pythonanywhere.com/pricing/) che è _davvero_ gratuito, sebbene con alcune limitazioni.
  Il fatto che sia accessibile a tutti gli sviluppatori è davvero importante per MDN!

  > [!NOTE]
  > Questo tutorial è stato ospitato su Heroku, Railway e ora PythonAnywhere, migrando quando i piani gratuiti precedenti sono stati dismessi.
  > Abbiamo scelto PythonAnywhere perché pensiamo che questo piano possa rimanere gratuito.
  > Abbiamo mantenuto anche l'esempio di Railway, che non è gratuito, per confronto e perché ci consente di dimostrare più facilmente funzionalità come l'integrazione con un database Postgres che esegue su un altro servizio.

- PythonAnywhere si occupa dell'infrastruttura così che non deve preoccuparsene lei.
  Non doversi preoccupare di server, bilanciatori di carico, proxy inversi e altro, rende molto più facile iniziare.
- Le capacità e i concetti che imparerà utilizzando PythonAnywhere sono trasferibili.
- I limiti del servizio e del piano non ci influenzano particolarmente nell'utilizzo di PythonAnywhere per il tutorial.
  Ad esempio:

  - Il piano per i principianti consente una web app su `<your-username>.pythonanywhere.com`, accesso Internet in uscita limitato dalle sue app, CPU/banda limitata, nessun supporto IPython/ Jupyter notebook, nessun database Postgres gratuito.
    Ma c'è abbastanza spazio per il nostro sito di base per essere eseguito!
  - Non sono supportati domini personalizzati (al momento della scrittura).
  - L'ambiente si spegne quando non è in uso, quindi può essere lento al riavvio.
    Può farlo girare per sempre, ma dovrà visitare il sito ogni tre mesi e rinnovare l'applicazione web.
  - C'è supporto gratuito per un database MySQL separato, ma non per Postgres.
    In questa dimostrazione utilizzeremo solo il database predefinito SQLite di Django.

PythonAnywhere è adatto per ospitare questa dimostrazione e può essere scalato a progetti più grandi se necessario.
Dovrebbe prendersi il tempo necessario per determinare se è [adatto per il suo sito web personale](#scegliere_un_fornitore_di_hosting).

### Come funziona PythonAnywhere?

PythonAnywhere fornisce un'interfaccia completamente basata sul web per caricare, modificare e lavorare con la sua applicazione.

Attraverso l'interfaccia può lanciare una console bash in un ambiente Ubuntu Linux in cui può creare la sua applicazione.
In questa dimostrazione useremo la console per clonare il nostro repository GitHub della biblioteca locale e creare un ambiente Python in cui possiamo eseguire l'applicazione web.

Il piano gratuito non offre supporto separato per Postgres.
Mentre potremmo usare qualche altro servizio di hosting per il nostro database, utilizzeremo solo il database predefinito SQLite creato da Django nell'ambiente Ubuntu ospitato (c'è più che abbastanza spazio per dimostrare la funzionalità della biblioteca).

Una volta che l'applicazione è in esecuzione, può essere configurata per la produzione impostando variabili d'ambiente attraverso la console bash.

È tutto ciò che serve per iniziare.

### Ottenere un account PythonAnywhere

Per iniziare a usare PythonAnywhere dovrà prima creare un account:

- Andare sulla pagina di [Piani e prezzi di PythonAnywhere](https://www.pythonanywhere.com/pricing/), e selezionare il pulsante **Crea un account per principianti**.
- Creare un account con il suo nome utente, email e password, accettare i termini e le condizioni, e poi selezionare **Registrati**.
- Verrà quindi loggato e reindirizzato alla dashboard di PythonAnywhere: `https://www.pythonanywhere.com/user/<your_user_name>/`.

### Installare la libreria da GitHub

Successivamente apriremo un prompt Bash, imposteremo un ambiente virtuale e prenderemo il codice sorgente della biblioteca locale da GitHub.
Configureremo anche il database predefinito e raccoglieremo file statici in modo che possano essere serviti da PythonAnywhere.

1. Innanzitutto aprire lo schermo di gestione della Console selezionando **Console** nella barra superiore dell'applicazione.
2. Poi selezionare il link **Bash** per creare e lanciare una nuova console:

   ![Immagine della schermata di gestione della console di PythonAnywhere](python_anywhere_start_bash_console.png)

   Si noti che qualsiasi console che si crea è salvata per riutilizzo futuro, insieme a tutta la sua cronologia.
   La freccia verde sopra mostra che questo account ha una console che possiamo aprire invece.

3. Nella console, inserire il seguente comando per creare un ambiente virtuale Python 3.10 chiamato "env_local_library" per installare le dipendenze della libreria locale.

   ```bash
   mkvirtualenv --python=python3.10 env_local_library
   ```

   Questo è esattamente lo stesso processo trattato in [Impostare un ambiente di sviluppo Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/development_environment).
   Potremmo aver chiamato l'ambiente in qualunque modo, e possiamo disattivarlo e riattivarlo utilizzando i comandi seguenti:

   ```bash
   deactivate
   workon env_local_library
   ```

4. Successivamente, ottenere le sorgenti della libreria da GitHub.
   PythonAnywhere si aspetta che lei installi le applicazioni in una cartella chiamata con il nome dell'URL del suo sito.

   > [!NOTE]
   > Poiché stiamo utilizzando l'account gratuito, può solo nominare il suo account `<your_pythonanywhere_username>.pythonanywhere.com` (ad esempio, se il suo nome utente è "Odtsetseg", dovrà mettere il sorgente della biblioteca locale in una cartella chiamata `odtsetseg.pythonanywhere.com`).

   Inserire il comando seguente per clonare le sorgenti della sua libreria in una cartella denominata correttamente (dovrà sostituire i valori del nome utente con il suo nome):

   ```bash
   git clone https://github.com/<github_username>/django-locallibrary-tutorial.git <your_pythonanywhere_username>.pythonanywhere.com

   # Navigate into the new folder
   cd <your_pythonanywhere_username>.pythonanywhere.com
   ```

5. Installare le dipendenze della libreria utilizzando il file `requirements.txt`:

   ```bash
   pip3 install -r requirements.txt
   ```

6. Creare e configurare un database SQLite sul computer hosting (proprio come abbiamo fatto durante lo sviluppo).

   ```bash
   python manage.py migrate
   ```

   > [!NOTE]
   > Per l'esempio Railway, configureremo un database Postgres e lo collegheremo impostando la variabile d'ambiente `DATABASE_URL`.
   > È importante che `migrate` sia chiamato _dopo_ aver configurato quale database utilizzare.

7. Raccogliere tutti i file statici in una posizione in cui possono essere [serviti in produzione](#servire_file_statici_in_produzione):

   ```bash
   python manage.py collectstatic --no-input
   ```

8. Creare un superuser per accedere al sito (come descritto nella sezione [Django admin site](/it/docs/Learn_web_development/Extensions/Server-side/Django/Admin_site#creating_a_superuser)):

   ```bash
   python manage.py createsuperuser
   ```

   Prendere nota dei dettagli, poiché ne avrà bisogno per testare il suo sito.

### Configurare l'app web

Dopo aver ottenuto le sorgenti della biblioteca locale e installato le dipendenze in un ambiente virtuale, dobbiamo dire a PythonAnywhere come trovarle e usarle come app web.

1. Navigare alla sezione _Web_ del sito e selezionare il link **Aggiungi una nuova app web**:

   ![Sezione "Web" di PythonAnywhere mostrando il pulsante per aggiungere una nuova app](python_anywhere_web_add_new_app.png)

   Verrà quindi aperto il _wizard Crea una nuova app web_ per guidarla nella configurazione delle proprietà principali dell'app web.

2. Selezionare **Avanti** per saltare la configurazione del nome di dominio dell'app web.
   L'account gratuito creerà il dominio in base al suo nome utente: `<user_name>.pythonanywhere.com`.

   ![Prompt di PythonAnywhere per impostare il nome di dominio della nuova app web](python_anywhere_web_add_new_app_prompt.png)

3. Nella schermata _Seleziona un framework web Python_ selezionare **Configurazione manuale**.

   ![Prompt di PythonAnywhere per selezionare il framework web utilizzato per l'applicazione](python_anywhere_web_add_select_framework_manual.png)

   La configurazione manuale le consente un controllo completo su come l'ambiente è configurato.
   Questo non importa tanto ora, ma importerebbe se stessimo ospitando più siti, potenzialmente con versioni diverse di Python e/o Django.

4. Nella schermata _Seleziona una versione di Python_ selezionare **3.10**

   ![Prompt di selezione della versione Python per l'applicazione Web di PythonAnywhere](python_anywhere_web_add_select_python_version.png)

   Più in generale, dovrebbe selezionare l'ultima versione di Python consentita dalla versione di Django che sta utilizzando.

5. Nella schermata _Configurazione manuale_ selezionare **Avanti** (la schermata spiega solo alcune delle opzioni per la configurazione)

   ![Prompt di PythonAnywhere che spiega le opzioni di configurazione successive](python_anywhere_web_add_manual_config.png)

   L'app web è creata, e visualizzata nella sezione Web come mostrato.
   La schermata ha un pulsante **Ricarica** che può usare per ricaricare l'applicazione web dopo aver apportato ulteriori modifiche.
   Come riportato nello schermo, noterà che dovrà cliccare sul pulsante **Esegui fino a 3 mesi da oggi** per mantenere il sito attivo per altri tre mesi (e continuativamente).

   ![Configurazione dell'app web su PythonAnywhere](python_anywhere_web_configuration.png)

6. Scorrere verso il basso nella scheda _Web_ fino alla sezione "Codice" e selezionare il link al file di configurazione WSGI.
   Questo avrà un nome del tipo `/var/www/<user_name>_pythonanywhere_com_wsgi.py`.

   ![File WSGI di PythonAnywhere nella scheda Web, sezione codice](python_anywhere_web_code_wsgi_select.png)

   Sostituire il contenuto del file con il seguente testo (prima aggiornando "hamishwillee" con il proprio nome utente), quindi selezionare il pulsante **Salva**.

   ```python
   import os
   import sys

   path = '/home/hamishwillee/hamishwillee.pythonanywhere.com'
   if path not in sys.path:
       sys.path.append(path)

   os.environ['DJANGO_SETTINGS_MODULE'] = 'locallibrary.settings'

   from django.core.wsgi import get_wsgi_application
   application = get_wsgi_application()
   ```

   Si noti che il ruolo del file WSGI è quello di aiutare il server Gunicorn a trovare l'applicazione della biblioteca locale.
   PythonAnywhere si aspetta che questo file sia in questa posizione, motivo per cui il file WSGI già nel progetto non può essere usato.

7. Scorrere verso il basso nella scheda _Web_ fino alla sezione "Virtualenv".
   Selezionare il collegamento **Inserire il percorso a un ambiente virtuale, se desiderato** e inserire il percorso dell'ambiente virtuale creato nella sezione precedente.
   Se lo ha chiamato "env_local_library" come suggerito, il percorso sarà: `/home/<user_name>/.virtualenvs/env_local_library`

   ![Sezione Ambiente virtuale della scheda Web di PythonAnywhere](python_anywhere_web_virtualenv.png)

8. Scorrere verso il basso nella scheda _Web_ fino alla sezione "File statici".

   ![Sezione File statici della scheda Web di PythonAnywhere](python_anywhere_web_static_files.png)

   Selezionare il collegamento **Inserire URL** e inserire `\static_files\`.
   Questo è lo `STATIC_URL` nelle [impostazioni dell'applicazione](#settings.py_2), e riflette la posizione in cui i file sono stati copiati quando abbiamo eseguito `collectstatic` nella sezione precedente.

9. Vicino alla parte superiore della scheda _Web_ selezionare il pulsante **Ricarica** per riavviare il sito.
   Poi selezionare il link all'URL del sito per avviare il sito live:

![Schermato Web di PythonAnywhere con il link per lanciare il sito evidenziato](python_anywhere_web_open_site.png)

### Impostare ALLOWED_HOSTS e CSRF_TRUSTED_ORIGINS

All'apertura del sito, a questo punto vedrà una schermata di errore debug come mostrato di seguito.
Questo è un errore di sicurezza Django segnalato perché il nostro codice sorgente non è in esecuzione su un "host consentito".

![Pagina di errore dettagliata con un traceback completo di un'intestazione HTTP_HOST non valida](python_anywhere_error_disallowed_host.png)

> [!NOTE]
> Questo tipo di informazione di debug è molto utile quando si sta configurando, ma è un rischio per la sicurezza in un sito distribuito.
> Nella sezione successiva mostreremo come disabilitare questo livello di logging sul sito live utilizzando le [variabili d'ambiente](#utilizzo_delle_variabili_d'ambiente_su_pythonanywhere).

Aprire **/locallibrary/settings.py** nel progetto GitHub e cambiare l'impostazione [ALLOWED_HOSTS](https://docs.djangoproject.com/en/5.0/ref/settings/#allowed-hosts) per includere l'URL del sito PythonAnywhere:

```python
## For example, for a site URL at 'hamishwillee.pythonanywhere.com'
## (replace the string below with your own site URL):
ALLOWED_HOSTS = ['hamishwillee.pythonanywhere.com', '127.0.0.1']

# During development, you can instead set just the base URL
# (you might decide to change the site a few times).
# ALLOWED_HOSTS = ['.pythonanywhere.com','127.0.0.1']
```

Poiché le applicazioni usano la protezione CSRF, dovrà anche impostare la chiave [CSRF_TRUSTED_ORIGINS](https://docs.djangoproject.com/en/5.0/ref/settings/#csrf-trusted-origins).
Aprire **/locallibrary/settings.py** e aggiungere una riga come quella di seguito:

```python
## For example, for a site URL is at 'web-production-3640.up.railway.app'
## (replace the string below with your own site URL):
CSRF_TRUSTED_ORIGINS = ['https://hamishwillee.pythonanywhere.com']

# During development/for this tutorial you can instead set just the base URL
# CSRF_TRUSTED_ORIGINS = ['https://*.pythonanywhere.com']
```

Salvare queste impostazioni e confermarle nel suo repo GitHub.

Dovrà quindi aggiornare la versione del suo progetto su PythonAnywhere.
Supponendo che stia usando il suo prompt Bash nella cartella `<user_name>.pythonanywhere.com`, e abbia spinto i cambiamenti al ramo principale, allora potrebbe importarli nel prompt Bash usando il comando:

```bash
git pull origin main
```

Utilizzare il pulsante **Ricarica** sulla scheda `Web` per riavviare l'applicazione.
Se aggiorna il suo sito ospitato, dovrebbe ora aprirsi e mostrare la home page del sito.

Dovrebbe essere in grado di accedere con l'account superuser che ha creato sopra, e creare autori, generi, libri e così via, proprio come ha fatto sul suo computer locale.

### Utilizzo delle variabili d'ambiente su PythonAnywhere

Nella sezione su [Preparare il sito per la pubblicazione](#preparare_il_suo_sito_per_la_pubblicazione) abbiamo modificato l'applicazione in modo che possa essere configurata utilizzando variabili d'ambiente o variabili in un file **.env** in produzione.

Specificamente, abbiamo configurato la biblioteca in modo che possa impostare:

- `DJANGO_DEBUG=False` per ridurre il tracciamento del debug mostrato all'utente quando c'è un errore.
- `DJANGO_SECRET_KEY` per avere un valore segreto in produzione.
- `DATABASE_URL` se la sua applicazione utilizza un database ospitato (cosa che non facciamo in questo esempio).

Il modo in cui le variabili d'ambiente sono impostate dipende dal servizio di hosting.
Per PythonAnywhere deve leggerle da un file ambiente.
Siamo già impostati per quello, quindi tutto ciò che deve fare è creare il file.

I passaggi sono:

1. Aprire un prompt Bash di PythonAnywhere.
2. Navigare nella sua directory applicativa (sostituendo `<user-name>` con il suo account):

   ```bash
   cd ~/<user-name>.pythonanywhere.com
   ```

3. Impostare le variabili d'ambiente scrivendole come coppie chiave-valore nel file `.env`.
   Ad esempio, per impostare `DJANGO_DEBUG` su `False` nel console Bash, inserire il seguente comando:

   ```bash
   echo "DJANGO_DEBUG=False" >> .env
   ```

4. Riavviare l'applicazione.

Può testare se l'operazione ha funzionato tentando di aprire un record che non esiste (ad esempio, creare un genere, quindi incrementare il numero nella barra degli indirizzi per aprire un record che non è ancora stato creato).
Se la variabile d'ambiente è stata caricata, si riceverà un messaggio "Non trovato" piuttosto che una traccia dettagliata del debug.

## Esempio: Hosting su Railway

Questa sezione fornisce una dimostrazione pratica di come installare _LocalLibrary_ su [Railway](https://railway.com/).

### Perché Railway?

> [!WARNING]
> Railway non ha più un livello per principianti completamente gratuito.
> Abbiamo mantenuto queste istruzioni perché Railway ha alcune funzionalità eccellenti e sarà un'opzione migliore per alcuni utenti.

Railway è una scelta di hosting interessante per diversi motivi:

- Railway si occupa della maggior parte dell'infrastruttura così che non deve preoccuparsene lei.
  Non doversi preoccupare di server, bilanciatori di carico, proxy inversi e altro, rende molto più facile iniziare.
- Railway ha un [focus sull'esperienza dello sviluppatore per lo sviluppo e la distribuzione](https://docs.railway.com/maturity/compare-to-heroku), il che porta a una curva di apprendimento più rapida e morbida rispetto a molte altre alternative.
- Le capacità e i concetti che imparerà utilizzando Railway sono trasferibili.
  Anche se Railway offre alcune nuove caratteristiche eccellenti, altri servizi di hosting popolari utilizzano molte delle stesse idee e approcci.
- La documentazione [Railway](https://docs.railway.com/) è chiara e completa.
- Il servizio sembra essere molto affidabile, e se alla fine lo adorerà, i prezzi sono prevedibili e la scalabilità dell'app è molto facile.

Dovrebbe prendersi il tempo necessario per determinare se Railway è [adatto per il suo sito web personale](#scegliere_un_fornitore_di_hosting).

### Come funziona Railway?

Le applicazioni web sono eseguite ciascuna nel proprio contenitore virtualizzato isolato e indipendente.
Per eseguire la sua applicazione, Railway deve essere in grado di impostare l'ambiente e le dipendenze appropriate e anche comprendere come viene lanciata.
Per le app Django forniamo queste informazioni in diversi file di testo:

- **runtime.txt**: indica la versione del linguaggio di programmazione da utilizzare.
- **requirements.txt**: elenca le dipendenze Python necessarie per il suo sito, incluso Django.
- **Procfile**: un elenco di processi da eseguire per avviare l'applicazione web.
  Per Django questo sarà solitamente il server applicativo web Gunicorn (con uno script `.wsgi`).
- **wsgi.py**: configurazione [WSGI](https://wsgi.readthedocs.io/en/latest/what.html) per chiamare la nostra applicazione Django nell'ambiente Railway.

Una volta che l'applicazione è in esecuzione può configurarsi utilizzando le informazioni fornite in [variabili d'ambiente](https://docs.railway.com/guides/variables).
Ad esempio, un'applicazione che utilizza un database può ottenere l'indirizzo usando la variabile `DATABASE_URL`.
Il servizio di database stesso può essere ospitato da Railway o da qualche altro fornitore.

Gli sviluppatori interagiscono con Railway tramite il sito Railway e utilizzando uno speciale strumento [Command Line Interface (CLI)](https://docs.railway.com/guides/cli).
Il CLI le consente di associare un repository GitHub locale a un progetto railway, caricare il repository dal ramo locale al sito live, ispezionare i log del processo in esecuzione, impostare e ottenere variabili di configurazione e molto altro ancora.
Una delle caratteristiche più utili è che può usare il CLI per eseguire il suo progetto locale con le stesse variabili d'ambiente del progetto live.

Per far funzionare la nostra applicazione su Railway, dovremo inserire la nostra applicazione web Django in un repository git, aggiungere i file di cui sopra, integrare con un add-on di database e fare le modifiche necessarie per gestire correttamente i file statici.
Una volta che avremo fatto tutto ciò, possiamo creare un account Railway, prendere il client Railway e installare il nostro sito web.

È tutto ciò che serve per iniziare.

### Aggiornare l'app per Railway

Questa sezione spiega le modifiche che dovrà apportare alla nostra applicazione _LocalLibrary_ per farla funzionare su Railway.
Dobbiamo solo creare un file `Procfile` e `runtime.txt`, perché quasi tutto il resto è già presente.

Noti che queste modifiche non le impediranno di usare i test locali e i flussi di lavoro che abbiamo già imparato.

#### Procfile

Un _Procfile_ è il "punto di ingresso" dell'applicazione web.
Elenca i comandi che saranno eseguiti da Railway per avviare il suo sito.

Crei il file `Procfile` (senza estensione) nella radice del suo repo GitHub e copi/incolli nel testo seguente:

```plain
web: python manage.py migrate && python manage.py collectstatic --no-input && gunicorn locallibrary.wsgi
```

Il prefisso `web:` dice a Railway che si tratta di un processo web e può essere inviato traffico HTTP.
Poi chiamiamo il comando di migrazione di Django `python manage.py migrate` per impostare le tabelle del database.
Successivamente, chiamiamo il comando di Django `python manage.py collectstatic` per raccogliere file statici nella cartella definita dall'impostazione del progetto `STATIC_ROOT` (vedi sezione [servire file statici in produzione](#servire_file_statici_in_produzione) di seguito).
Infine, avviamo il processo _gunicorn_, un popolare server applicativo web, passandogli informazioni di configurazione nel modulo `locallibrary.wsgi` (creato con lo schema dell'app: **/locallibrary/wsgi.py**).

Noterà che abbiamo già predisposto il progetto per includere _gunicorn_ e supportare il servizio di file statici!

Può anche usare il Procfile per avviare processi worker o eseguire altri compiti non interattivi prima che la release venga distribuita.

#### Runtime

Il file **runtime.txt**, se definito, dice a Railway quale versione di Python usare.
Crei il file nella radice del repo e aggiunga il seguente testo:

```plain
python-3.10.2
```

> [!NOTE]
> I fornitori di hosting non supportano necessariamente ogni versione minore del runtime Python.
> Di solito usano la versione supportata più vicina al valore specificato.

#### Ri-test e salvare le modifiche su GitHub

Prima di procedere, provi di nuovo il sito localmente e accetti che non sia stato danneggiato da nessuna delle modifiche sopra.
Esegua il server web di sviluppo come al solito e poi controlli che il sito funzioni ancora come si aspetta nel suo browser.

```bash
python3 manage.py runserver
```

Successivamente, facciamo un `push` delle modifiche su GitHub.
Nel terminale (dopo essersi posizionati nel nostro repository locale), inserire i seguenti comandi:

```python
git checkout -b railway_changes
git add -A
git commit -m "Added files and changes required for deployment"
git push origin railway_changes
```

Poi crei e unisca il PR su GitHub.

Dovremmo ora essere pronti per iniziare a distribuire LocalLibrary su Railway.

### Ottenere un account Railway

Per iniziare a usare Railway deve prima creare un account:

- Andare su [railway.com](https://railway.com/) e cliccare sul link **Login** nella barra superiore degli strumenti.
- Selezionare GitHub nel pop-up per accedere utilizzando le credenziali del suo GitHub
- Potrebbe quindi dover andare alla sua email e verificare il suo account.
- Sarà quindi loggato nel dashboard Railway.com: <https://railway.com/dashboard>.

### Distribuire su Railway da GitHub

Successivamente configureremo Railway per distribuire la nostra biblioteca da GitHub.
Scelga prima l'opzione **Dashboard** dal menu superiore del sito, poi selezioni il **Nuovo progetto**:

![Dashboard del sito Railway con il pulsante nuovo progetto](railway_new_project_button.png)

Railway mostrerà un elenco di opzioni per il nuovo progetto, inclusa l'opzione di distribuire un progetto da un modello creato per primo nel suo account GitHub, e un certo numero di database.
Selezioni **Distribuire da repo GitHub**.

![Schermata del sito Railway - distribuisci](railway_new_project_button_deploy_github_repo.png)

Tutti i progetti nei repo GitHub che ha condiviso con Railway durante la configurazione vengono visualizzati.
Selezionare il suo repository GitHub per la biblioteca locale: `<user-name>/django-locallibrary-tutorial`.

![Schermata del sito Railway che mostra una finestra di dialogo per scegliere un repository GitHub esistente o sceglierne uno nuovo](railway_new_project_button_deploy_github_selectrepo.png)

Confermi la sua distribuzione selezionando **Distribuisci ora**.

![Schermata di conferma - selezionare distribuisci](railway_new_project_deploy_confirm.png)

Railway quindi caricherà e distribuirà il suo progetto, mostrando i progressi nella scheda delle distribuzioni.
Quando la distribuzione sarà completata con successo, vedrà una schermata come quella qui sotto.

![Schermata del sito Railway - distribuzione](railway_project_deploy.png)

Può cliccare l'URL del sito (evidenziato sopra) per aprire il sito in un browser (non funzionerà ancora, perché la configurazione non è completa).

### Impostare ALLOWED_HOSTS e CSRF_TRUSTED_ORIGINS

All'apertura del sito, a questo punto vedrà una schermata di errore debug come mostrato sotto.
Questo è un errore di sicurezza Django sollevato perché il nostro codice sorgente non è in esecuzione su un "host consentito".

![Pagina di errore dettagliata con un traceback completo di un'intestazione HTTP_HOST non valida](site_error_disallowed_host.png)

> [!NOTE]
> Questo tipo di informazione di debug è molto utile quando si sta configurando, ma è un rischio per la sicurezza in un sito distribuito.
> Mostreremo come disabilitarlo una volta che il sito è up e funzionante.

Aprire **/locallibrary/settings.py** nel progetto GitHub e cambiare l'impostazione [ALLOWED_HOSTS](https://docs.djangoproject.com/en/5.0/ref/settings/#allowed-hosts) per includere l'URL del suo sito Railway:

```python
## For example, for a site URL at 'web-production-3640.up.railway.app'
## (replace the string below with your own site URL):
ALLOWED_HOSTS = ['web-production-3640.up.railway.app', '127.0.0.1']

# During development, you can instead set just the base URL
# (you might decide to change the site a few times).
# ALLOWED_HOSTS = ['.railway.com','127.0.0.1']
```

Poiché le applicazioni usano la protezione CSRF, dovrà anche impostare la chiave [CSRF_TRUSTED_ORIGINS](https://docs.djangoproject.com/en/5.0/ref/settings/#csrf-trusted-origins).
Aprire **/locallibrary/settings.py** e aggiungere una riga come quella di sotto:

```python
## For example, for a site URL is at 'web-production-3640.up.railway.app'
## (replace the string below with your own site URL):
CSRF_TRUSTED_ORIGINS = ['https://web-production-3640.up.railway.app']

# During development/for this tutorial you can instead set just the base URL
# CSRF_TRUSTED_ORIGINS = ['https://*.railway.app']
```

Poi salvi le sue impostazioni e le confermi nel suo repo GitHub (Railway aggiornerà e ridistribuirà automaticamente la sua applicazione).

### Fornire e collegare un database SQL Postgres

Successivamente dobbiamo creare un database Postgres e collegarlo all'applicazione Django che abbiamo appena distribuito.
(Se apre il sito ora otterrà un nuovo errore perché il database non può essere accesso).
Creeremo il database come parte del progetto applicativo, anche se può creare il database nel suo progetto separato.

Su Railway, scegliere l'opzione **Dashboard** dal menu superiore del sito e quindi selezionare il suo progetto applicativo.
In questa fase contiene solo un singolo servizio per la sua applicazione (questo può essere selezionato per impostare variabili e altri dettagli del servizio).
Il pulsante **Impostazioni** può essere selezionato per cambiare le impostazioni a livello di progetto.
Selezionare il pulsante **Nuovo**, che viene usato per aggiungere servizi al progetto.

![Progetto Railway con pulsante nuovo servizio evidenziato](railway_project_open_no_database.png)

Quando richiesto sul tipo di servizio da aggiungere, selezionare **Database**:

![Progetto Railway - seleziona database come nuovo servizio](railway_project_add_database.png)

Quindi selezionare **Add PostgreSQL** per iniziare ad aggiungere il database

![Progetto Railway - seleziona Postgres come nuovo servizio](railway_project_add_database_select_type.png)

Railway fornirà quindi un servizio contenente un database vuoto nello stesso progetto.
Al completamento vedrà ora entrambi i servizi applicativi e di database nella vista del progetto.

![Progetto Railway con servizio applicazione e database Postgres](railway_project_two_services.png)

Selezionare il servizio web e poi la scheda _Variabili_.
Selezionare **Nuova Variabile** e poi nella casella _Nome variabile_, selezionare **Aggiungi riferimento**.
Scorra verso il basso e selezioni `DATABASE_URL` (questo è il nome della variabile che abbiamo configurato la biblioteca locale per leggere come variabile d'ambiente).

![Schermata del sito Railway che seleziona una DATABASE_URL](railway_postgresql_connect.png)

Quindi selezionare **Aggiungi** per aggiungere il riferimento alla variabile e infine **Distribuisci** (questo apparirà in un popup).
Noti che potrebbe anche aver aperto il database Postgres, quindi la sua scheda variabili, e aver copiato la variabile.

Se apre ora il progetto dovrà visualizzare esattamente come è stato visualizzato localmente.
Noti tuttavia che non c'è modo di popolare la biblioteca con dati ancora, perché non abbiamo ancora creato un account superuser.
Lo faremo usando il [CLI](https://docs.railway.com/guides/cli) sul nostro computer locale.

### Installare il client

Scaricare e installare il client Railway per il suo sistema operativo locale seguendo le [istruzioni qui](https://docs.railway.com/guides/cli).

Dopo che il client è installato sarà in grado di eseguire i comandi.
Alcune delle operazioni più importanti includono la distribuzione della directory corrente del suo computer in un progetto Railway associato (senza dover caricare su GitHub), e l'esecuzione del suo progetto Django localmente utilizzando le stesse impostazioni che ha sul server di produzione.
Mostriamo questi passaggi nelle sezioni successive.

Può ottenere un elenco di tutti i comandi possibili inserendo il seguente comando in un terminale.

```bash
railway help
```

> [!NOTE]
> Nella sezione seguente utilizziamo `railway login` e `railway link` per collegare il progetto attuale a una directory.
> Se viene disconnesso dal sistema, dovrà richiamare entrambi i comandi per ricollegare il progetto.

### Configurare un superuser

Per creare un superuser, dobbiamo chiamare il comando Django `createsuperuser` contro il database di produzione (questo è la stessa operazione che abbiamo eseguito in locale nel [Django Tutorial Parte 4: Django admin site > Creating a superuser](/it/docs/Learn_web_development/Extensions/Server-side/Django/Admin_site#creating_a_superuser)).
Railway non fornisce accesso diretto al terminale al server, e non possiamo aggiungere questo comando al [Procfile](#procfile) perché è interattivo.

Quello che possiamo fare è chiamare questo comando localmente sul nostro progetto Django quando è collegato al database _di produzione_.
Il client Railway lo rende facile fornendo un meccanismo per eseguire comandi localmente utilizzando le stesse variabili d'ambiente del server di produzione, inclusa la stringa di connessione al database.

Prima aprire un terminale o un prompt dei comandi in una clonazione git del suo progetto locallibrary.
Poi effettui il login al suo account browser usando il comando `login` o `login --browserless` (segua le eventuali istruzioni risultanti dal client o dal sito web per completare il login):

```bash
railway login
```

Una volta loggato, colleghi la sua directory locallibrary corrente al progetto Railway associato utilizzando il comando seguente.
Noti che dovrà selezionare/inserire un particolare progetto quando richiesto:

```bash
railway link
```

Ora che la directory locale e il progetto sono _collegati_ può eseguire il progetto Django locale con le impostazioni dall'ambiente di produzione.
Innanzitutto si assicuri che il suo normale [ambiente di sviluppo Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/development_environment) sia pronto.
Poi chiami il comando seguente, inserendo il nome, l'email e la password se richiesto:

```bash
railway run python manage.py createsuperuser
```

A questo punto dovrebbe essere in grado di aprire l'area amministrativa del suo sito-web (`https://[your-url].railway.app/admin/`) e popolare il database, proprio come mostrato in [Django Tutorial Parte 4: Django admin site](/it/docs/Learn_web_development/Extensions/Server-side/Django/Admin_site)).

### Impostare le variabili di configurazione

L'ultimo passo è rendere il sito sicuro.
Specificamente, dobbiamo disabilitare la registrazione di debug e impostare una chiave CSRF segreta.
Il lavoro per leggere i valori necessari dalle variabili d'ambiente è stato fatto in [preparare il suo sito per la pubblicazione](#preparare_il_suo_sito_per_la_pubblicazione) (vedi `DJANGO_DEBUG` e `DJANGO_SECRET_KEY`).

Aprire la schermata delle informazioni per il progetto e selezionare la scheda _Variabili_.
Questa dovrebbe avere già il `DATABASE_URL` come mostrato sotto.

![Railway - schermata per aggiungere una nuova variabile](railway_variable_new.png)

Ci sono molti modi per generare una chiave segreta crittograficamente.
Un modo semplice è eseguire il seguente comando Python sul suo computer di sviluppo:

```bash
python -c "import secrets; print(secrets.token_urlsafe())"
```

Selezionare il pulsante **Nuova Variabile** e inserire la chiave `DJANGO_SECRET_KEY` con il suo valore segreto (poi selezioni **Aggiungi**).
Poi inserire la chiave `DJANGO_DEBUG` con il valore `False`.
Il set finale di variabili dovrebbe apparire così:

![Schermata Railway che mostra tutte le variabili del progetto](railway_variables_all.png)

### Debugging

Il client Railway fornisce il comando logs per mostrare la coda dei log (un log più pieno è disponibile sul sito per ogni progetto):

```bash
railway logs
```

Se ha bisogno di più informazioni di quanto possa fornire, dovrà iniziare a esaminare il [logging di Django](https://docs.djangoproject.com/en/5.0/topics/logging/).

## Riepilogo

Questo è la conclusione di questo tutorial sull'impostazione delle app Django in produzione, e anche della serie di tutorial sul lavoro con Django. Speriamo che li abbia trovati utili. Può controllare una versione completamente elaborata del [codice sorgente su GitHub qui](https://github.com/mdn/django-locallibrary-tutorial).

Il prossimo passo è leggere i nostri ultimi articoli e quindi completare l'attività di valutazione.

## Vedi anche

- [Distribuire Django](https://docs.djangoproject.com/en/5.0/howto/deployment/) (documenti Django)

  - [Checklist di distribuzione](https://docs.djangoproject.com/en/5.0/howto/deployment/checklist/) (documenti Django)
  - [Distribuire file statici](https://docs.djangoproject.com/en/5.0/howto/static-files/deployment/) (documenti Django)
  - [Come distribuire con WSGI](https://docs.djangoproject.com/en/5.0/howto/deployment/wsgi/) (documenti Django)
  - [Come usare Django con Apache e mod_wsgi](https://docs.djangoproject.com/en/5.0/howto/deployment/wsgi/modwsgi/) (documenti Django)
  - [Come usare Django con Gunicorn](https://docs.djangoproject.com/en/5.0/howto/deployment/wsgi/gunicorn/) (documenti Django)

- Documenti Railway

  - [CLI](https://docs.railway.com/guides/cli)

- DigitalOcean

  - [Come servire applicazioni Django con uWSGI e Nginx su Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-serve-django-applications-with-uwsgi-and-nginx-on-ubuntu-16-04)
  - [Altri documenti della community Django di DigitalOcean](https://www.digitalocean.com/community/tutorials?q=django)

- Documenti Heroku (concetti di configurazione simili)

  - [Configurare app Django per Heroku](https://devcenter.heroku.com/articles/django-app-configuration) (documenti Heroku)
  - [Iniziare su Heroku con Django](https://devcenter.heroku.com/articles/getting-started-with-python#introduction) (documenti Heroku)
  - [Django e asset statici](https://devcenter.heroku.com/articles/django-assets) (documenti Heroku)
  - [Concorrente e connessioni di database nel Django](https://devcenter.heroku.com/articles/python-concurrency-and-database-connections) (documenti Heroku)
  - [Come funziona Heroku](https://devcenter.heroku.com/articles/how-heroku-works) (documenti Heroku)
  - [Dynos e il gestore Dyno](https://devcenter.heroku.com/articles/dynos) (documenti Heroku)
  - [Configurazione e variabili di configurazione](https://devcenter.heroku.com/articles/config-vars) (documenti Heroku)
  - [Limiti](https://devcenter.heroku.com/articles/limits) (documenti Heroku)
  - [Distribuire applicazioni Python con Gunicorn](https://devcenter.heroku.com/articles/python-gunicorn) (documenti Heroku)
  - [Lavorare con Django](https://devcenter.heroku.com/categories/working-with-django) (documenti Heroku)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Testing", "Learn_web_development/Extensions/Server-side/Django/web_application_security", "Learn_web_development/Extensions/Server-side/Django")}}
