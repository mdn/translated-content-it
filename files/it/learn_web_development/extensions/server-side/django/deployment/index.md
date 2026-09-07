---
title: "Tutorial Django - Parte 11: Distribuire Django in produzione"
short-title: "11: Distribuzione"
slug: Learn_web_development/Extensions/Server-side/Django/Deployment
l10n:
  sourceCommit: 324c613947adaa5e19ad0f409c5f4c535ee8cf6b
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Testing", "Learn_web_development/Extensions/Server-side/Django/web_application_security", "Learn_web_development/Extensions/Server-side/Django")}}

È già stato creato e testato un sito web di esempio usando Django; ora è il momento di installarlo su un server web affinché sia accessibile a chiunque tramite Internet pubblico.
Questa pagina descrive come ospitare un progetto Django e cosa è necessario preparare per distribuire il sito in produzione.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Completare tutti gli argomenti precedenti del tutorial, incluso <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/Testing">Tutorial Django - Parte 10: Testare un'applicazione web Django</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Imparare dove e come distribuire un'app Django in produzione.</td>
    </tr>
  </tbody>
</table>

## Panoramica

Una volta completato il sito (o completato "abbastanza" da iniziare i test pubblici), sarà necessario ospitarlo in un luogo più pubblico e accessibile rispetto al computer personale di sviluppo.

Finora si è lavorato in un ambiente di sviluppo, usando il server web di sviluppo Django per condividere il sito con il browser/rete locale, ed eseguendo il sito web con impostazioni di sviluppo (non sicure) che espongono informazioni di debug e altre informazioni private. Prima di poter ospitare esternamente un sito web, sarà prima necessario:

- Apportare alcune modifiche alle impostazioni del progetto.
- Scegliere un ambiente in cui ospitare l'app Django.
- Scegliere un ambiente in cui ospitare eventuali file statici.
- Configurare un'infrastruttura di livello produzione per servire il sito web.

Questo tutorial fornisce alcune indicazioni sulle opzioni disponibili per scegliere un sito di hosting, una breve panoramica di ciò che è necessario fare per preparare l'app Django alla produzione e un esempio funzionante di come installare il sito web LocalLibrary sul servizio di cloud hosting [Railway](https://railway.com/).

## Che cos'è un ambiente di produzione?

L'ambiente di produzione è l'ambiente fornito dal computer server sul quale verrà eseguito il sito web per l'uso esterno. L'ambiente include:

- Hardware del computer sul quale viene eseguito il sito web.
- Sistema operativo (ad esempio Linux, Windows).
- Runtime del linguaggio di programmazione e librerie del framework sui quali è scritto il sito web.
- Server web utilizzato per servire pagine e altri contenuti (ad esempio Nginx, Apache).
- Server applicativo che inoltra le richieste "dinamiche" tra il sito web Django e il server web.
- Database dai quali dipende il sito web.

> [!NOTE]
> A seconda di come è configurato l'ambiente di produzione, potrebbero essere presenti anche un reverse proxy, un load balancer e così via.

Il computer server potrebbe trovarsi presso la propria sede ed essere connesso a Internet tramite un collegamento veloce, ma è molto più comune utilizzare un computer ospitato "nel cloud". In pratica, ciò significa che il codice viene eseguito su un computer remoto (o potenzialmente su un computer "virtuale") nei data center della società di hosting. Il server remoto offre generalmente un certo livello garantito di risorse computazionali (CPU, RAM, memoria di archiviazione e così via) e connettività Internet a un determinato prezzo.

Questo tipo di hardware computazionale/di rete accessibile da remoto viene definito _Infrastructure as a Service (IaaS)_. Molti fornitori IaaS offrono opzioni per preinstallare un particolare sistema operativo, sul quale è necessario installare gli altri componenti dell'ambiente di produzione. Altri fornitori consentono di selezionare ambienti più completi, che potrebbero includere una configurazione completa di Django e server web.

> [!NOTE]
> Gli ambienti preconfigurati possono rendere molto semplice la configurazione del sito web poiché riducono il lavoro di configurazione, ma le opzioni disponibili possono limitare a un server non familiare (o ad altri componenti) e possono basarsi su una versione meno recente del sistema operativo. Spesso è preferibile installare personalmente i componenti, in modo da ottenere quelli desiderati e, quando è necessario aggiornare parti del sistema, avere un'idea di dove iniziare.

Altri provider di hosting supportano Django nell'ambito di un'offerta _Platform as a Service_ (PaaS). In questo tipo di hosting non è necessario preoccuparsi della maggior parte dell'ambiente di produzione (server web, server applicativo, load balancer), perché la piattaforma host se ne occupa — insieme alla maggior parte di ciò che è necessario fare per scalare l'applicazione.
Ciò rende la distribuzione piuttosto semplice, perché è sufficiente concentrarsi sull'applicazione web e non su tutta l'altra infrastruttura del server.

Alcuni sviluppatori sceglieranno la maggiore flessibilità offerta da IaaS rispetto a PaaS, mentre altri apprezzeranno il minore carico di manutenzione e la scalabilità più semplice di PaaS. All'inizio, configurare il sito web su un sistema PaaS è molto più semplice, quindi è ciò che verrà fatto in questo tutorial.

> [!NOTE]
> Se viene scelto un provider di hosting adatto a Python/Django, dovrebbe fornire istruzioni su come configurare un sito web Django usando diverse configurazioni di server web, server applicativo, reverse proxy e così via. Questo non sarà rilevante se viene scelto un PaaS. Ad esempio, sono disponibili molte guide dettagliate per varie configurazioni nella [documentazione della community Django di DigitalOcean](https://www.digitalocean.com/community/tutorials?q=django).

## Scegliere un provider di hosting

Esistono molti provider di hosting noti per supportare attivamente Django o per funzionare bene con Django, tra cui: [Heroku](https://www.heroku.com/), [DigitalOcean](https://www.digitalocean.com/), [Railway](https://railway.com/), [Python Anywhere](https://www.pythonanywhere.com/), [Amazon Web Services](https://aws.amazon.com/), [Azure](https://azure.microsoft.com/en-us), [Google Cloud](https://cloud.google.com/), [Hetzner](https://www.hetzner.com/) e [Vultr Cloud Compute](https://blogs.vultr.com/new-free-tier-plan), solo per citarne alcuni.
Questi fornitori offrono diversi tipi di ambienti (IaaS, PaaS) e diversi livelli di risorse computazionali e di rete a prezzi differenti.

Alcuni aspetti da considerare nella scelta di un host:

- Quanto è probabile che il sito sia frequentato e il costo delle risorse di dati e calcolo necessarie per soddisfare tale domanda.
- Livello di supporto per la scalabilità orizzontale (aggiunta di più macchine) e verticale (aggiornamento a macchine più potenti), nonché i relativi costi.
- Dove il fornitore dispone di data center e, di conseguenza, dove è probabile che l'accesso sia più rapido.
- Storico del provider relativo a uptime e downtime.
- Strumenti forniti per la gestione del sito: sono facili da usare e sicuri (ad esempio SFTP rispetto a FTP)?
- Framework integrati per monitorare il server.
- Limitazioni note. Alcuni host bloccano deliberatamente determinati servizi (ad esempio la posta elettronica). Altri offrono solo un determinato numero di ore di "tempo attivo" in alcune fasce di prezzo, oppure solo una piccola quantità di spazio di archiviazione.
- Vantaggi aggiuntivi. Alcuni provider offrono nomi di dominio gratuiti e supporto per certificati TLS per i quali altrimenti sarebbe necessario pagare.
- Se il livello "gratuito" su cui si fa affidamento scade nel tempo e se il costo della migrazione a un livello più costoso implica che sarebbe stato meglio usare fin dall'inizio un altro servizio.

La buona notizia, quando si inizia, è che esistono diversi siti che forniscono ambienti computazionali "gratuiti" destinati alla valutazione e ai test.
In genere si tratta di ambienti con risorse piuttosto limitate e occorre essere consapevoli che possono scadere dopo un periodo introduttivo o avere altri vincoli.
Sono comunque ottimi per testare siti a basso traffico in un ambiente ospitato e possono offrire una facile migrazione al pagamento di più risorse quando il sito riceve più traffico.
Scelte popolari in questa categoria includono [Vultr Cloud Compute](https://blogs.vultr.com/new-free-tier-plan), [Python Anywhere](https://www.pythonanywhere.com/), [Amazon Web Services](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/free-tier.html), [Microsoft Azure](https://azure.microsoft.com/en-us/pricing/details/app-service/linux/) e così via.

La maggior parte dei provider offre anche un livello "base" destinato a piccoli siti di produzione, che fornisce livelli più utili di potenza di calcolo e meno limitazioni.
[Railway](https://railway.com/), [Heroku](https://www.heroku.com/) e [DigitalOcean](https://www.digitalocean.com/) sono esempi di provider di hosting popolari che dispongono di un livello base di calcolo relativamente economico, nella fascia tra $5 e $10 USD al mese.

> [!NOTE]
> Ricordare che il prezzo non è l'unico criterio di selezione. Se il sito web avrà successo, potrebbe risultare che la scalabilità è la considerazione più importante.

## Preparare il sito web per la pubblicazione

Il [sito web scheletro Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/skeleton_website) creato con gli strumenti _django-admin_ e _manage.py_ è configurato per semplificare lo sviluppo. Molte delle impostazioni del progetto Django (specificate in **settings.py**) dovrebbero essere diverse in produzione, per ragioni di sicurezza o prestazioni.

> [!NOTE]
> È comune avere un file **settings.py** separato per la produzione e/o importare condizionatamente impostazioni sensibili da un file separato o da una variabile d'ambiente. Questo file deve quindi essere protetto, anche se il resto del codice sorgente è disponibile in un repository pubblico.

Le impostazioni critiche da verificare sono:

- `DEBUG`. In produzione deve essere impostata su `False` (`DEBUG = False`). Questo impedisce la visualizzazione della traccia di debug sensibile/riservata e delle informazioni sulle variabili.
- `SECRET_KEY`. Si tratta di un valore casuale di grandi dimensioni utilizzato per la protezione CSRF e così via. È importante che la chiave usata in produzione non sia nel controllo del codice sorgente né accessibile al di fuori del server di produzione.

La documentazione di Django suggerisce che le informazioni segrete siano preferibilmente caricate da una variabile d'ambiente o lette da un file disponibile solo sul server.
Modifichiamo l'applicazione _LocalLibrary_ in modo da leggere le variabili `SECRET_KEY` e `DEBUG` dalle variabili d'ambiente, se definite, ricorrendo ai valori definiti in un file **.env** nella root e, infine, ai valori predefiniti nel file di configurazione.
Questo approccio è molto flessibile perché consente qualsiasi configurazione supportata dal server di hosting.

Per leggere valori di ambiente da un file verrà utilizzato [python-dotenv](https://pypi.org/project/python-dotenv/).
Questa è una libreria per leggere coppie chiave-valore da un file e usarle come variabili d'ambiente, ma solo se la corrispondente variabile d'ambiente non è definita.

Installare la libreria nell'ambiente virtuale come mostrato, aggiornando anche il file `requirements.txt`:

```bash
pip3 install python-dotenv
```

Quindi aprire **/locallibrary/settings.py** e inserire il codice seguente dopo la definizione di `BASE_DIR`, ma prima dell'avviso di sicurezza: `# SECURITY WARNING: keep the secret key used in production secret!`

```python
# Support env variables from .env file if defined
import os
from dotenv import load_dotenv

env_path = os.path.join(BASE_DIR, ".env")
if os.path.exists(env_path):
    load_dotenv(env_path)
```

Questo carica il file `.env` dalla root dell'applicazione web.
Le variabili definite come `KEY=VALUE` nel file vengono importate quando la chiave viene usata in `os.environ.get('<KEY>'', '<DEFAULT VALUE>')`, se definita.

> [!NOTE]
> Tutti i valori aggiunti a **.env** sono probabilmente dei _segreti_.
> Non devono essere salvati su GitHub e `.env` dovrebbe essere aggiunto al file `.gitignore`, per evitare che venga aggiunto accidentalmente.

Quindi disabilitare la configurazione originale di `SECRET_KEY` e aggiungere le nuove righe come mostrato di seguito.
Durante lo sviluppo non verrà specificata alcuna variabile d'ambiente per la chiave, pertanto verrà utilizzato il valore predefinito; non dovrebbe avere importanza quale chiave venga utilizzata qui o se la chiave venga "divulgata", perché non verrà usata in produzione.

```python
# SECURITY WARNING: keep the secret key used in production secret!
# SECRET_KEY = 'django-insecure-&psk#na5l=p3q8_a+-$4w1f^lt3lx1c@d*p4x$ymm_rn7pwb87'
import os
SECRET_KEY = os.environ.get('DJANGO_SECRET_KEY', 'django-insecure-&psk#na5l=p3q8_a+-$4w1f^lt3lx1c@d*p4x$ymm_rn7pwb87')
```

Quindi commentare l'impostazione `DEBUG` esistente e aggiungere la nuova riga mostrata di seguito.

```python
# SECURITY WARNING: don't run with debug turned on in production!
# DEBUG = True
DEBUG = os.environ.get('DJANGO_DEBUG', '') != 'False'
```

Il valore di `DEBUG` sarà `True` per impostazione predefinita, ma sarà `False` solo se il valore della variabile d'ambiente `DJANGO_DEBUG` è impostato su `False` oppure se nel file **.env** è impostato `DJANGO_DEBUG=False`.
Si noti che le variabili d'ambiente sono stringhe e non tipi Python. È quindi necessario confrontare stringhe. L'unico modo per impostare la variabile `DEBUG` su `False` consiste nell'impostarla effettivamente sulla stringa `False`.

È possibile impostare la variabile d'ambiente su "False" in Linux eseguendo il seguente comando:

```bash
export DJANGO_DEBUG=False
```

Un elenco completo delle impostazioni che potrebbe essere necessario modificare è fornito nella [lista di controllo per la distribuzione](https://docs.djangoproject.com/en/5.0/howto/deployment/checklist/) (documentazione Django). È inoltre possibile elencarne alcune usando il comando da terminale seguente:

```bash
python3 manage.py check --deploy
```

### Gunicorn

[Gunicorn](https://gunicorn.org/) è un server HTTP puro Python comunemente utilizzato per servire applicazioni Django WSGI.

Sebbene _Gunicorn_ non sia necessario per servire l'applicazione LocalLibrary durante lo sviluppo, verrà installato localmente affinché diventi parte dei [requisiti](#requisiti) quando l'applicazione verrà distribuita.

Innanzitutto assicurarsi di essere nell'ambiente virtuale Python creato durante la [configurazione dell'ambiente di sviluppo](/it/docs/Learn_web_development/Extensions/Server-side/Django/development_environment) (usare il comando `workon [name-of-virtual-environment]`).
Quindi installare _Gunicorn_ localmente nella riga di comando usando _pip_:

```bash
pip3 install gunicorn
```

### Configurazione del database

SQLite, il database Django predefinito utilizzato per lo sviluppo, è una scelta ragionevole per siti web di dimensioni piccole o medie.
Purtroppo non può essere utilizzato su alcuni popolari servizi di hosting, come Heroku, perché non forniscono archiviazione dati persistente nell'ambiente dell'applicazione, un requisito di SQLite.
Sebbene questo non ci riguardi nelle distribuzioni di esempio, verrà mostrato un altro approccio che funziona su Railway, Heroku e altri servizi.

L'approccio consiste nell'usare un database eseguito nel proprio processo da qualche parte su Internet, a cui accede l'applicazione Django mediante un indirizzo passato come variabile d'ambiente.
In questo caso verrà usato un database Postgres ospitato anch'esso su Railway, ma può essere utilizzato qualsiasi servizio di hosting di database.

Le informazioni di connessione al database saranno fornite a Django mediante una variabile d'ambiente denominata `DATABASE_URL`.
Invece di codificare rigidamente queste informazioni in Django, verrà usato il pacchetto [dj-database-url](https://pypi.org/project/dj-database-url/) per analizzare la variabile d'ambiente `DATABASE_URL` e convertirla automaticamente nel formato di configurazione desiderato da Django.
Oltre a installare il pacchetto _dj-database-url_, sarà necessario installare anche [psycopg2](https://www.psycopg.org/), poiché Django ne ha bisogno per interagire con i database Postgres.

#### dj-database-url

_dj-database-url_ viene usato per estrarre la configurazione del database Django da una variabile d'ambiente.

Installarlo localmente affinché diventi parte dei [requisiti](#requisiti) da configurare sul server di distribuzione:

```bash
pip3 install dj-database-url
```

#### settings.py

Aprire **/locallibrary/settings.py** e copiare la configurazione seguente in fondo al file:

```python
# Update database configuration from $DATABASE_URL environment variable (if defined)
import dj_database_url

if 'DATABASE_URL' in os.environ:
    DATABASES['default'] = dj_database_url.config(
        conn_max_age=500,
        conn_health_checks=True,
    )
```

Django utilizzerà ora la configurazione del database in `DATABASE_URL` se la variabile d'ambiente è impostata; altrimenti usa il database SQLite predefinito.
Il valore `conn_max_age=500` rende persistente la connessione, il che è molto più efficiente rispetto a ricrearla a ogni ciclo di richiesta. Questo parametro è facoltativo e può essere rimosso se necessario.

#### psycopg2

<!-- Django 4.2 now supports Psycopg (3) : https://docs.djangoproject.com/en/5.0/releases/4.2/#psycopg-3-support
  But didn't work on Railway!
  Try again to update in next release.
-->

Django richiede _psycopg2_ per funzionare con i database Postgres.
Installarlo localmente affinché diventi parte dei [requisiti](#requisiti) che Railway configurerà sul server remoto:

```bash
pip3 install psycopg2-binary
```

Si noti che Django utilizzerà il database SQLite durante lo sviluppo per impostazione predefinita, a meno che non sia impostato `DATABASE_URL`.
È possibile passare completamente a Postgres e usare lo stesso database ospitato per sviluppo e produzione impostando la stessa variabile d'ambiente nell'ambiente di sviluppo; Railway semplifica l'uso dello stesso ambiente per produzione e sviluppo.
In alternativa, è possibile installare e usare un [database Postgres ospitato localmente](https://www.psycopg.org/docs/install.html) sul computer locale.

### Servire file statici in produzione

Durante lo sviluppo si usano Django e il server web di sviluppo Django per servire sia l'HTML dinamico sia i file statici, quali CSS, JavaScript e così via.
Questo è inefficiente per i file statici, perché le richieste devono passare attraverso Django anche se Django non esegue alcuna operazione su di essi.
Sebbene questo non sia rilevante durante lo sviluppo, avrebbe un impatto significativo sulle prestazioni se venisse usato lo stesso approccio in produzione.

Nell'ambiente di produzione, in genere si separano i file statici dall'applicazione web Django, rendendo più semplice servirli direttamente dal server web o da una content delivery network (CDN).

Le importanti variabili di impostazione sono:

- `STATIC_URL`: posizione URL di base dalla quale verranno serviti i file statici, ad esempio su una CDN.
- `STATIC_ROOT`: percorso assoluto di una directory nella quale lo strumento _collectstatic_ di Django raccoglierà tutti i file statici referenziati nei template. Una volta raccolti, possono essere caricati come gruppo ovunque debbano essere ospitati.
- `STATICFILES_DIRS`: elenca le directory aggiuntive nelle quali lo strumento _collectstatic_ di Django deve cercare file statici.

I template Django fanno riferimento alle posizioni dei file statici relative a un tag `static` — visibile nel template base definito in [Tutorial Django - Parte 5: Creare la home page](/it/docs/Learn_web_development/Extensions/Server-side/Django/Home_page#the_locallibrary_base_template) — che a sua volta viene mappato all'impostazione `STATIC_URL`.
I file statici possono quindi essere caricati su qualsiasi host ed è possibile aggiornare l'applicazione affinché li trovi usando questa impostazione.

Lo strumento _collectstatic_ viene usato per raccogliere i file statici nella cartella definita dall'impostazione di progetto `STATIC_ROOT`.
Viene chiamato con il seguente comando:

```bash
python3 manage.py collectstatic
```

Per questo tutorial, _collectstatic_ può essere eseguito prima di caricare l'applicazione, copiando tutti i file statici dell'applicazione nella posizione specificata in `STATIC_ROOT`.
`Whitenoise` trova quindi i file dalla posizione definita da `STATIC_ROOT`, per impostazione predefinita, e li serve all'URL di base definito da `STATIC_URL`.

#### settings.py

Aprire **/locallibrary/settings.py** e copiare la configurazione seguente in fondo al file.
`BASE_DIR` dovrebbe essere già definito nel file; `STATIC_URL` potrebbe essere già stato definito nel file quando è stato creato.
Sebbene non causi alcun danno, è possibile eliminare il precedente riferimento duplicato.

```python
# Static files (CSS, JavaScript, Images)
# https://docs.djangoproject.com/en/5.0/howto/static-files/

# The absolute path to the directory where collectstatic will collect static files for deployment.
STATIC_ROOT = BASE_DIR / 'staticfiles'

# The URL to use when referring to static files (where they will be served from)
STATIC_URL = '/static/'
```

Il servizio dei file verrà effettivamente eseguito usando una libreria chiamata [WhiteNoise](https://pypi.org/project/whitenoise/), che verrà installata e configurata nella sezione successiva.

### Whitenoise

Esistono molti modi per servire file statici in produzione; le impostazioni Django pertinenti sono state esaminate nelle sezioni precedenti.
Il progetto [WhiteNoise](https://pypi.org/project/whitenoise/) fornisce uno dei metodi più semplici per servire asset statici direttamente da Gunicorn in produzione.

Consultare la documentazione di [WhiteNoise](https://pypi.org/project/whitenoise/) per una spiegazione del suo funzionamento e del motivo per cui l'implementazione è un metodo relativamente efficiente per servire questi file.

I passaggi per configurare _WhiteNoise_ da usare con il progetto sono [disponibili qui](https://whitenoise.readthedocs.io/en/stable/django.html) e vengono riprodotti di seguito.

#### Installare whitenoise

Installare whitenoise localmente usando il seguente comando:

```bash
pip3 install whitenoise
```

#### settings.py

Per installare _WhiteNoise_ nell'applicazione Django, aprire **/locallibrary/settings.py**, trovare l'impostazione `MIDDLEWARE` e aggiungere `WhiteNoiseMiddleware` vicino all'inizio dell'elenco, subito sotto `SecurityMiddleware`:

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

Facoltativamente, è possibile ridurre le dimensioni dei file statici quando vengono serviti, il che è più efficiente.
Aggiungere semplicemente quanto segue in fondo a **/locallibrary/settings.py**:

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

Non è necessario fare altro per configurare _WhiteNoise_, perché usa per impostazione predefinita le impostazioni del progetto `STATIC_ROOT` e `STATIC_URL`.

### Requisiti

I requisiti Python dell'applicazione web devono essere archiviati in un file **requirements.txt** nella root del repository.
Molti servizi di hosting installeranno automaticamente le dipendenze in questo file; negli altri sarà necessario farlo manualmente.
È possibile creare questo file usando _pip_ nella riga di comando; eseguire quanto segue nella root del repository:

```bash
pip3 freeze > requirements.txt
```

Dopo aver installato tutte le diverse dipendenze sopra indicate, il file **requirements.txt** dovrebbe contenere _almeno_ questi elementi, anche se i numeri di versione possono essere diversi.
Eliminare qualsiasi altra dipendenza non elencata di seguito, a meno che non sia stata aggiunta esplicitamente per questa applicazione.

```plain
Django==5.0.2
dj-database-url==2.1.0
gunicorn==21.2.0
psycopg2-binary==2.9.9
wheel==0.38.1
whitenoise==6.6.0
python-dotenv==1.0.1
```

### Aggiornare il repository dell'applicazione su GitHub

Molti servizi di hosting consentono di importare e/o sincronizzare progetti da un repository locale o da piattaforme cloud di controllo versione del codice sorgente.
Questo può rendere molto più semplici la distribuzione e lo sviluppo iterativo.

GitHub dovrebbe già essere utilizzato per archiviare il codice sorgente della libreria locale; è stato configurato in [Gestione del codice sorgente con Git e GitHub](/it/docs/Learn_web_development/Extensions/Server-side/Django/development_environment#source_code_management_with_git_and_github) come parte della configurazione dell'ambiente di sviluppo.

Questo è un buon momento per creare un backup del progetto "vanilla": mentre alcune delle modifiche che verranno effettuate nelle sezioni seguenti potrebbero essere utili per la distribuzione su qualsiasi servizio di hosting, o per lo sviluppo, altre potrebbero non esserlo.
Supponendo di avere già salvato su GitHub, nel ramo `main`, tutte le modifiche effettuate finora, è possibile creare un nuovo ramo per effettuare il backup delle modifiche come mostrato:

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

## Esempio: hosting su PythonAnywhere

Questa sezione fornisce una dimostrazione pratica di come ospitare _LocalLibrary_ su [PythonAnywhere](https://www.pythonanywhere.com/).

### Perché PythonAnywhere?

PythonAnywhere viene scelto per diverse ragioni:

- PythonAnywhere dispone di un [piano gratuito per principianti](https://www.pythonanywhere.com/pricing/) che è _davvero_ gratuito, seppur con alcune limitazioni.
  Il fatto che sia accessibile economicamente a tutti gli sviluppatori è molto importante per MDN.

  > [!NOTE]
  > Questo tutorial è stato ospitato su Heroku, Railway e ora PythonAnywhere, migrando quando i precedenti piani gratuiti sono stati interrotti.
  > È stato scelto PythonAnywhere perché si ritiene che questo piano rimarrà probabilmente gratuito.
  > È stato mantenuto anche l'esempio Railway, che non è gratuito, per confronto e perché consente di dimostrare più facilmente funzionalità quali l'integrazione con database Postgres eseguiti su un servizio diverso.

- PythonAnywhere si occupa dell'infrastruttura, quindi non è necessario farlo manualmente.
  Non doversi preoccupare di server, load balancer, reverse proxy e così via rende molto più semplice iniziare.
- Le competenze e i concetti appresi usando PythonAnywhere sono trasferibili.
- Le limitazioni del servizio e del piano non hanno un impatto particolare sull'uso di PythonAnywhere per il tutorial.
  Ad esempio:
  - Il piano per principianti consente un'app web in `<your-username>.pythonanywhere.com`, accesso Internet in uscita limitato dalle app, CPU/larghezza di banda ridotte, nessun supporto per notebook IPython/Jupyter e nessun database Postgres gratuito.
    Tuttavia, c'è spazio sufficiente per eseguire il sito di base.
  - I domini personalizzati non sono supportati al momento della scrittura.
  - L'ambiente si arresta quando non viene utilizzato, quindi potrebbe essere lento a riavviarsi.
    È possibile eseguirlo per sempre, ma sarà necessario visitare il sito ogni tre mesi e rinnovare l'applicazione web.
  - È disponibile supporto gratuito per un database MySQL separato, ma non per Postgres.
    In questa dimostrazione verrà usato soltanto il database SQLite Django predefinito.

PythonAnywhere è appropriato per ospitare questa dimostrazione e può essere scalato a progetti più grandi, se necessario.
Occorre dedicare del tempo a determinare se è [adatto al proprio sito web](#scegliere_un_provider_di_hosting).

### Come funziona PythonAnywhere?

PythonAnywhere offre un'interfaccia interamente basata sul web per caricare, modificare e lavorare in altro modo con l'applicazione.

Attraverso l'interfaccia è possibile avviare una console bash in un ambiente Ubuntu Linux, nel quale creare l'applicazione.
In questa dimostrazione la console verrà usata per clonare il repository GitHub della libreria locale e creare un ambiente Python nel quale eseguire l'applicazione web.

Il piano gratuito non fornisce supporto Postgres separato.
Sebbene sia possibile usare un altro servizio di hosting per il database, verrà semplicemente usato il database SQLite predefinito creato da Django nell'ambiente Ubuntu ospitato; c'è spazio più che sufficiente per dimostrare la funzionalità della libreria.

Una volta in esecuzione, l'applicazione può essere configurata per la produzione impostando variabili d'ambiente tramite la console bash.

Questa è tutta la panoramica necessaria per iniziare.

### Ottenere un account PythonAnywhere

Per iniziare a usare PythonAnywhere è necessario prima creare un account:

- Andare alla pagina [Piani e prezzi](https://www.pythonanywhere.com/pricing/) di PythonAnywhere e selezionare il pulsante **Create a Beginner account**.
- Creare un account con nome utente, email e password, accettare termini e condizioni, quindi selezionare **Register**.
- Verrà quindi effettuato l'accesso e si verrà reindirizzati alla dashboard PythonAnywhere: `https://www.pythonanywhere.com/user/<your_user_name>/`.

### Installare la libreria da GitHub

Successivamente verrà aperto un prompt Bash, verrà configurato un ambiente virtuale e verrà recuperato il codice sorgente della libreria locale da GitHub.
Verranno inoltre configurati il database predefinito e raccolti i file statici affinché possano essere serviti da PythonAnywhere.

1. Aprire innanzitutto la schermata di gestione Console selezionando **Consoles** nella barra superiore dell'applicazione.
2. Quindi selezionare il collegamento **Bash** per creare e avviare una nuova console:

   ![Immagine della schermata di gestione Console di PythonAnywhere](python_anywhere_start_bash_console.png)

   Si noti che ogni console creata viene salvata per un riutilizzo successivo, insieme a tutta la sua cronologia.
   La freccia verde sopra mostra che questo account dispone di una console che sarebbe possibile aprire invece.

3. Nella console, immettere il comando seguente per creare un ambiente virtuale Python 3.10 denominato "env_local_library" per installare le dipendenze della libreria locale.

   ```bash
   mkvirtualenv --python=python3.10 env_local_library
   ```

   Si tratta esattamente dello stesso processo trattato in [Configurare un ambiente di sviluppo Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/development_environment).
   L'ambiente avrebbe potuto avere qualsiasi nome e può essere disattivato e riattivato usando i comandi seguenti:

   ```bash
   deactivate
   workon env_local_library
   ```

4. Recuperare quindi i sorgenti della libreria da GitHub.
   PythonAnywhere richiede l'installazione delle applicazioni in una cartella denominata come l'URL del sito.

   > [!NOTE]
   > Poiché viene usato l'account gratuito, è possibile denominare l'account soltanto come `<your_pythonanywhere_username>.pythonanywhere.com`; ad esempio, se il nome utente è "Odtsetseg", il sorgente della libreria locale dovrà essere inserito in una cartella denominata `odtsetseg.pythonanywhere.com`.

   Immettere il comando seguente per clonare i sorgenti della libreria in una cartella con il nome appropriato; sarà necessario sostituire i valori del nome utente con il proprio nome:

   ```bash
   git clone https://github.com/<github_username>/django-locallibrary-tutorial.git <your_pythonanywhere_username>.pythonanywhere.com

   # Navigate into the new folder
   cd <your_pythonanywhere_username>.pythonanywhere.com
   ```

5. Installare le dipendenze della libreria usando il file `requirements.txt`:

   ```bash
   pip3 install -r requirements.txt
   ```

6. Creare e configurare un database SQLite sul computer di hosting, esattamente come è stato fatto durante lo sviluppo.

   ```bash
   python manage.py migrate
   ```

   > [!NOTE]
   > Per l'esempio Railway verrà [configurato un database Postgres](#effettuare_il_provisioning_e_connettere_un_database_sql_postgres), al quale ci si connetterà impostando la variabile d'ambiente `DATABASE_URL`.
   > È importante che `migrate` venga chiamato _dopo_ aver configurato il database da usare.

7. Raccogliere tutti i file statici in una posizione dalla quale possano essere [serviti in produzione](#servire_file_statici_in_produzione):

   ```bash
   python manage.py collectstatic --no-input
   ```

8. Creare un superuser per accedere al sito, come trattato nella sezione [sito di amministrazione Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/Admin_site#creating_a_superuser):

   ```bash
   python manage.py createsuperuser
   ```

   Annotare i dettagli, poiché saranno necessari per testare il sito.

### Configurare l'app web

Dopo aver ottenuto i sorgenti della libreria locale e installato le dipendenze in un ambiente virtuale, è necessario indicare a PythonAnywhere come trovarli e utilizzarli come app web.

1. Passare alla sezione _Web_ del sito e selezionare il collegamento **Add a new web app**:

   ![Sezione "Web" di PythonAnywhere che mostra il pulsante per aggiungere una nuova app](python_anywhere_web_add_new_app.png)

   Si aprirà quindi la procedura guidata _Create new web app_, che guiderà nella configurazione delle proprietà principali dell'app web.

2. Selezionare **Next** per saltare la configurazione del nome di dominio dell'app web.
   L'account gratuito creerà il dominio in base al nome utente: `<user_name>.pythonanywhere.com`.

   ![Prompt PythonAnywhere per impostare il nome di dominio della nuova app web](python_anywhere_web_add_new_app_prompt.png)

3. Nella schermata _Select a Python Web framework_, selezionare **Manual configuration**.

   ![Prompt PythonAnywhere per selezionare il framework web usato dall'applicazione](python_anywhere_web_add_select_framework_manual.png)

   La configurazione manuale consente il controllo completo del modo in cui l'ambiente viene configurato.
   Questo non è molto importante ora, ma lo sarebbe se venissero ospitati più siti, potenzialmente con versioni diverse di Python e/o Django.

4. Nella schermata _Select a Python version_, selezionare **3.10**.

   ![Prompt PythonAnywhere per selezionare la versione Python per l'applicazione Web](python_anywhere_web_add_select_python_version.png)

   Più in generale, dovrebbe essere selezionata la versione più recente di Python consentita dalla versione di Django utilizzata.

5. Nella schermata _Manual configuration_, selezionare **Next**; la schermata spiega soltanto alcune delle opzioni di configurazione.

   ![Prompt PythonAnywhere che spiega le opzioni di configurazione successive](python_anywhere_web_add_manual_config.png)

   L'app web viene creata e visualizzata nella sezione Web come mostrato.
   La schermata dispone di un pulsante **Reload** che può essere usato per ricaricare l'applicazione web dopo ulteriori modifiche.
   Come indicato nella schermata, sarà necessario fare clic sul pulsante **Run until 3 months from today** per mantenere vivo il sito per altri tre mesi, e successivamente.

   ![App Web PythonAnywhere configurata](python_anywhere_web_configuration.png)

6. Scorrere fino alla sezione "Code" della scheda _Web_ e selezionare il collegamento al file di configurazione WSGI.
   Avrà un nome nella forma `/var/www/<user_name>_pythonanywhere_com_wsgi.py`.

   ![File WSGI PythonAnywhere nella scheda Web, sezione code](python_anywhere_web_code_wsgi_select.png)

   Sostituire il contenuto del file con il testo seguente, aggiornando prima "hamishwillee" con il proprio nome utente, quindi selezionare il pulsante **Save**.

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

   Si noti che il ruolo del file WSGI è aiutare il server Gunicorn a trovare l'applicazione della libreria locale.
   PythonAnywhere si aspetta che questo file sia in questa posizione, motivo per cui non può essere utilizzato il file WSGI già presente nel progetto.

7. Scorrere fino alla sezione "Virtualenv" della scheda _Web_.
   Selezionare il collegamento **Enter the path to a virtual env, if desired** e immettere il percorso dell'ambiente virtuale creato nella sezione precedente.
   Se è stato denominato "env_local_library" come suggerito, il percorso sarà: `/home/<user_name>/.virtualenvs/env_local_library`

   ![Sezione ambiente virtuale nella scheda Web di PythonAnywhere](python_anywhere_web_virtualenv.png)

8. Scorrere fino alla sezione "Static files" della scheda _Web_.

   ![Sezione file statici nella scheda Web di PythonAnywhere](python_anywhere_web_static_files.png)

   Selezionare il collegamento **Enter URL** e immettere `\static_files\`.
   Questo è `STATIC_URL` nelle [impostazioni dell'applicazione](#settings.py_2) e riflette la posizione in cui i file sono stati copiati quando è stato eseguito `collectstatic` nella sezione precedente.

9. Vicino alla parte superiore della scheda _Web_, selezionare il pulsante **Reload** per riavviare il sito.
   Selezionare quindi il collegamento dell'URL del sito per aprire il sito attivo:

![Schermata Web PythonAnywhere con il collegamento per avviare il sito evidenziato](python_anywhere_web_open_site.png)

### Impostare ALLOWED_HOSTS e CSRF_TRUSTED_ORIGINS

Quando il sito viene aperto, a questo punto verrà visualizzata una schermata di errore di debug come quella mostrata di seguito.
Questo è un errore di sicurezza Django generato perché il codice sorgente non viene eseguito su un "host consentito".

![Pagina di errore dettagliata con traceback completo di un'intestazione HTTP_HOST non valida](python_anywhere_error_disallowed_host.png)

> [!NOTE]
> Questo tipo di informazioni di debug è molto utile durante la configurazione, ma rappresenta un rischio per la sicurezza in un sito distribuito.
> Nella sezione successiva verrà mostrato come disabilitare questo livello di registrazione sul sito attivo usando le [variabili d'ambiente](#usare_le_variabili_d'ambiente_su_pythonanywhere).

Aprire **/locallibrary/settings.py** nel progetto GitHub e modificare l'impostazione [ALLOWED_HOSTS](https://docs.djangoproject.com/en/5.0/ref/settings/#allowed-hosts) in modo da includere l'URL del sito PythonAnywhere:

```python
## For example, for a site URL at 'hamishwillee.pythonanywhere.com'
## (replace the string below with your own site URL):
ALLOWED_HOSTS = ['hamishwillee.pythonanywhere.com', '127.0.0.1']

# During development, you can instead set just the base URL
# (you might decide to change the site a few times).
# ALLOWED_HOSTS = ['.pythonanywhere.com','127.0.0.1']
```

Poiché le applicazioni usano la protezione CSRF, sarà inoltre necessario impostare la chiave [CSRF_TRUSTED_ORIGINS](https://docs.djangoproject.com/en/5.0/ref/settings/#csrf-trusted-origins).
Aprire **/locallibrary/settings.py** e aggiungere una riga simile a quella seguente:

```python
## For example, for a site URL is at 'web-production-3640.up.railway.app'
## (replace the string below with your own site URL):
CSRF_TRUSTED_ORIGINS = ['https://hamishwillee.pythonanywhere.com']

# During development/for this tutorial you can instead set just the base URL
# CSRF_TRUSTED_ORIGINS = ['https://*.pythonanywhere.com']
```

Salvare queste impostazioni ed eseguire il commit nel repository GitHub.

Sarà quindi necessario aggiornare la versione del progetto su PythonAnywhere.
Supponendo di usare il prompt Bash nella cartella `<user_name>.pythonanywhere.com` e di avere inviato le modifiche al ramo main, sarà possibile importarle nel prompt Bash usando il comando:

```bash
git pull origin main
```

Usare il pulsante **Restart** nella scheda `Web` per riavviare l'applicazione.
Aggiornando il sito ospitato, ora dovrebbe aprirsi e mostrare la home page del sito.

Dovrebbe essere possibile accedere con l'account superuser creato sopra e creare autori, generi, libri e così via, proprio come è stato fatto sul computer locale.

### Usare le variabili d'ambiente su PythonAnywhere

Nella sezione [Preparare il sito web per la pubblicazione](#preparare_il_sito_web_per_la_pubblicazione), l'applicazione è stata modificata affinché possa essere configurata usando variabili d'ambiente o variabili in un file **.env** in produzione.

Nello specifico, la libreria è stata configurata in modo da poter impostare:

- `DJANGO_DEBUG=False` per ridurre il tracing di debug mostrato all'utente quando si verifica un errore.
- `DJANGO_SECRET_KEY` su un valore segreto in produzione.
- `DATABASE_URL` se l'applicazione usa un database ospitato; in questo esempio non lo usa.

Il modo in cui vengono impostate le variabili d'ambiente dipende dal servizio di hosting.
Per PythonAnywhere è necessario leggerle da un file di ambiente.
Tutto è già predisposto a tale scopo, quindi occorre solo creare il file.

I passaggi sono:

1. Aprire un prompt Bash di PythonAnywhere.
2. Passare alla directory dell'applicazione, sostituendo `<user-name>` con il proprio account:

   ```bash
   cd ~/<user-name>.pythonanywhere.com
   ```

3. Impostare le variabili d'ambiente scrivendole come coppie chiave-valore nel file `.env`.
   Ad esempio, per impostare `DJANGO_DEBUG` su `False` nella console Bash, immettere il comando seguente:

   ```bash
   echo "DJANGO_DEBUG=False" >> .env
   ```

4. Riavviare l'applicazione.

È possibile verificare che l'operazione abbia funzionato tentando di aprire un record che non esiste, ad esempio creando un genere e incrementando poi il numero nella barra dell'URL per aprire un record che non è stato ancora creato.
Se la variabile d'ambiente è stata caricata, verrà visualizzato un messaggio "Not found" invece di una traccia di debug dettagliata.

## Esempio: hosting su Railway

Questa sezione fornisce una dimostrazione pratica di come installare _LocalLibrary_ su [Railway](https://railway.com/).

### Perché Railway?

> [!WARNING]
> Railway non dispone più di un livello iniziale completamente gratuito.
> Queste istruzioni sono state mantenute perché Railway offre alcune ottime funzionalità e sarà un'opzione migliore per alcuni utenti.

Railway è un'opzione di hosting interessante per diverse ragioni:

- Railway si occupa della maggior parte dell'infrastruttura, quindi non è necessario farlo manualmente.
  Non doversi preoccupare di server, load balancer, reverse proxy e così via rende molto più semplice iniziare.
- Railway è [orientato all'esperienza dello sviluppatore per sviluppo e distribuzione](https://docs.railway.com/platform/compare-to-heroku), il che comporta una curva di apprendimento più rapida e graduale rispetto a molte altre alternative.
- Le competenze e i concetti appresi usando Railway sono trasferibili.
  Sebbene Railway disponga di alcune eccellenti funzionalità nuove, altri popolari servizi di hosting utilizzano molte delle stesse idee e degli stessi approcci.
- La [documentazione Railway](https://docs.railway.com/) è chiara e completa.
- Il servizio sembra essere molto affidabile e, se dovesse risultare particolarmente apprezzato, i prezzi sono prevedibili e scalare l'app è molto semplice.

Occorre dedicare del tempo a determinare se Railway è [adatto al proprio sito web](#scegliere_un_provider_di_hosting).

### Come funziona Railway?

Le applicazioni web vengono eseguite ciascuna nel proprio container virtualizzato isolato e indipendente.
Per eseguire l'applicazione, Railway deve poter configurare l'ambiente e le dipendenze appropriati e comprendere anche come viene avviata.
Per le app Django, queste informazioni vengono fornite in diversi file di testo:

- **runtime.txt**: indica il linguaggio di programmazione e la versione da usare.
- **requirements.txt**: elenca le dipendenze Python necessarie per il sito, incluso Django.
- **Procfile**: elenco dei processi da eseguire per avviare l'applicazione web.
  Per Django, questo sarà in genere il server dell'applicazione web Gunicorn, con uno script `.wsgi`.
- **wsgi.py**: configurazione [WSGI](https://wsgi.readthedocs.io/en/latest/what.html) per chiamare l'applicazione Django nell'ambiente Railway.

Una volta in esecuzione, l'applicazione può configurarsi usando le informazioni fornite nelle [variabili d'ambiente](https://docs.railway.com/variables).
Ad esempio, un'applicazione che usa un database può ottenere l'indirizzo usando la variabile `DATABASE_URL`.
Il servizio database stesso può essere ospitato da Railway o da un altro provider.

Gli sviluppatori interagiscono con Railway attraverso il sito Railway e usando uno speciale strumento [Command Line Interface (CLI)](https://docs.railway.com/cli).
La CLI consente di associare un repository GitHub locale a un progetto Railway, caricare il repository dal ramo locale al sito attivo, ispezionare i log del processo in esecuzione, impostare e ottenere variabili di configurazione e molto altro.
Una delle funzionalità più utili è che è possibile usare la CLI per eseguire il progetto locale con le stesse variabili d'ambiente del progetto attivo.

Per far funzionare l'applicazione su Railway, sarà necessario inserire l'applicazione web Django in un repository git, aggiungere i file precedenti, integrare un add-on di database e apportare modifiche per gestire correttamente i file statici.
Una volta fatto tutto ciò, sarà possibile configurare un account Railway, ottenere il client Railway e installare il sito web.

Questa è tutta la panoramica necessaria per iniziare.

### Aggiornare l'app per Railway

Questa sezione spiega le modifiche necessarie all'applicazione _LocalLibrary_ per farla funzionare su Railway.
È realmente necessario soltanto creare un file `Procfile` e `runtime.txt`, perché quasi tutto il resto è già presente.

Si noti che queste modifiche non impediranno l'uso dei test locali e dei flussi di lavoro già appresi.

#### Procfile

Un _Procfile_ è il "punto di ingresso" dell'applicazione web.
Elenca i comandi che Railway eseguirà per avviare il sito.

Creare il file `Procfile`, senza estensione, nella root del repository GitHub e copiare/incollare il testo seguente:

```plain
web: python manage.py migrate && python manage.py collectstatic --no-input && gunicorn locallibrary.wsgi
```

Il prefisso `web:` indica a Railway che si tratta di un processo web e che può ricevere traffico HTTP.
Viene quindi chiamato il comando di migrazione Django `python manage.py migrate` per configurare le tabelle del database.
Successivamente viene chiamato il comando Django `python manage.py collectstatic` per raccogliere i file statici nella cartella definita dall'impostazione di progetto `STATIC_ROOT`; vedere la sezione [servire file statici in produzione](#servire_file_statici_in_produzione) di seguito.
Infine viene avviato il processo _gunicorn_, un popolare server per applicazioni web, passando informazioni di configurazione nel modulo `locallibrary.wsgi`, creato con lo scheletro dell'applicazione: **/locallibrary/wsgi.py**.

Si noterà che il progetto è già stato configurato per includere _gunicorn_ e supportare il servizio dei file statici.

È inoltre possibile usare il Procfile per avviare processi worker o eseguire altre attività non interattive prima della distribuzione della release.

#### Runtime

Il file **runtime.txt**, se definito, indica a Railway quale versione di Python usare.
Creare il file nella root del repository e aggiungere il testo seguente:

```plain
python-3.10.2
```

> [!NOTE]
> I provider di hosting non supportano necessariamente tutte le versioni minori del runtime Python.
> In genere utilizzeranno la versione supportata più vicina al valore specificato.

#### Ripetere i test e salvare le modifiche su GitHub

Prima di procedere, testare nuovamente il sito localmente e assicurarsi che non sia stato compromesso da nessuna delle modifiche precedenti.
Eseguire il server web di sviluppo come al solito, quindi verificare nel browser che il sito continui a funzionare come previsto.

```bash
python3 manage.py runserver
```

Successivamente, inviare le modifiche a GitHub con `push`.
Nel terminale, dopo aver raggiunto il repository locale, immettere i comandi seguenti:

```bash
git checkout -b railway_changes
git add -A
git commit -m "Added files and changes required for deployment"
git push origin railway_changes
```

Quindi creare e unire la PR su GitHub.

Ora dovrebbe essere tutto pronto per iniziare a distribuire LocalLibrary su Railway.

### Ottenere un account Railway

Per iniziare a usare Railway è necessario prima creare un account:

- Andare su [railway.com](https://railway.com/) e fare clic sul collegamento **Login** nella barra degli strumenti superiore.
- Selezionare GitHub nella finestra pop-up per accedere usando le credenziali GitHub.
- Potrebbe quindi essere necessario andare alla propria email e verificare l'account.
- Verrà quindi effettuato l'accesso alla dashboard Railway.com: <https://railway.com/dashboard>.

### Distribuire su Railway da GitHub

Successivamente verrà configurato Railway per distribuire la libreria da GitHub.
Prima scegliere l'opzione **Dashboard** dal menu superiore del sito, quindi selezionare il pulsante **New Project**:

![Dashboard del sito Railway con il pulsante nuovo progetto](railway_new_project_button.png)

Railway mostrerà un elenco di opzioni per il nuovo progetto, inclusa l'opzione per distribuire un progetto da un template che viene prima creato nell'account GitHub, e diversi database.
Selezionare **Deploy from GitHub repo**.

![Schermata del sito Railway - distribuzione](railway_new_project_button_deploy_github_repo.png)

Vengono visualizzati tutti i progetti nei repository GitHub condivisi con Railway durante la configurazione.
Selezionare il repository GitHub della libreria locale: `<user-name>/django-locallibrary-tutorial`.

![Schermata del sito Railway che mostra una finestra di dialogo per scegliere un repository GitHub esistente o sceglierne uno nuovo](railway_new_project_button_deploy_github_selectrepo.png)

Confermare la distribuzione selezionando **Deploy Now**.

![Schermata di conferma - selezionare distribuzione](railway_new_project_deploy_confirm.png)

Railway caricherà quindi e distribuirà il progetto, mostrando l'avanzamento nella scheda delle distribuzioni.
Quando la distribuzione sarà completata correttamente, verrà visualizzata una schermata simile alla seguente.

![Schermata del sito Railway - distribuzione](railway_project_deploy.png)

È possibile fare clic sull'URL del sito, evidenziato sopra, per aprirlo nel browser; non funzionerà ancora perché la configurazione non è completa.

### Impostare ALLOWED_HOSTS e CSRF_TRUSTED_ORIGINS

Quando il sito viene aperto, a questo punto verrà visualizzata una schermata di errore di debug come quella mostrata di seguito.
Questo è un errore di sicurezza Django generato perché il codice sorgente non viene eseguito su un "host consentito".

![Pagina di errore dettagliata con traceback completo di un'intestazione HTTP_HOST non valida](site_error_disallowed_host.png)

> [!NOTE]
> Questo tipo di informazioni di debug è molto utile durante la configurazione, ma rappresenta un rischio per la sicurezza in un sito distribuito.
> Verrà mostrato come disabilitarlo una volta che il sito sarà attivo e funzionante.

Aprire **/locallibrary/settings.py** nel progetto GitHub e modificare l'impostazione [ALLOWED_HOSTS](https://docs.djangoproject.com/en/5.0/ref/settings/#allowed-hosts) in modo da includere l'URL del sito Railway:

```python
## For example, for a site URL at 'web-production-3640.up.railway.app'
## (replace the string below with your own site URL):
ALLOWED_HOSTS = ['web-production-3640.up.railway.app', '127.0.0.1']

# During development, you can instead set just the base URL
# (you might decide to change the site a few times).
# ALLOWED_HOSTS = ['.railway.com','127.0.0.1']
```

Poiché le applicazioni usano la protezione CSRF, sarà inoltre necessario impostare la chiave [CSRF_TRUSTED_ORIGINS](https://docs.djangoproject.com/en/5.0/ref/settings/#csrf-trusted-origins).
Aprire **/locallibrary/settings.py** e aggiungere una riga simile a quella seguente:

```python
## For example, for a site URL is at 'web-production-3640.up.railway.app'
## (replace the string below with your own site URL):
CSRF_TRUSTED_ORIGINS = ['https://web-production-3640.up.railway.app']

# During development/for this tutorial you can instead set just the base URL
# CSRF_TRUSTED_ORIGINS = ['https://*.railway.app']
```

Quindi salvare le impostazioni ed eseguire il commit nel repository GitHub; Railway aggiornerà e ridistribuirà automaticamente l'applicazione.

### Effettuare il provisioning e connettere un database SQL Postgres

Successivamente è necessario creare un database Postgres e connetterlo all'applicazione Django appena distribuita.
Se si apre ora il sito, verrà visualizzato un nuovo errore perché non è possibile accedere al database.
Il database verrà creato come parte del progetto dell'applicazione, anche se è possibile crearlo in un progetto separato.

Su Railway, scegliere l'opzione **Dashboard** dal menu superiore del sito, quindi selezionare il progetto dell'applicazione.
In questa fase contiene soltanto un singolo servizio per l'applicazione; può essere selezionato per impostare variabili e altri dettagli del servizio.
Il pulsante **Settings** può essere selezionato per modificare le impostazioni a livello di progetto.
Selezionare il pulsante **New**, usato per aggiungere servizi al progetto.

![Progetto Railway con il pulsante nuovo servizio evidenziato](railway_project_open_no_database.png)

Quando viene richiesto il tipo di servizio da aggiungere, selezionare **Database**:

![Progetto Railway - selezionare database come nuovo servizio](railway_project_add_database.png)

Quindi selezionare **Add PostgreSQL** per iniziare ad aggiungere il database.

![Progetto Railway - selezionare Postgres come nuovo servizio](railway_project_add_database_select_type.png)

Railway effettuerà quindi il provisioning di un servizio contenente un database vuoto nello stesso progetto.
Al completamento, nella vista del progetto saranno ora visibili sia i servizi dell'applicazione sia quelli del database.

![Progetto Railway con servizio dell'applicazione e servizio database Postgres](railway_project_two_services.png)

Selezionare il servizio web e quindi la scheda _Variables_.
Selezionare **New Variable**, quindi nella casella _Variable name_ selezionare **Add reference**.
Scorrere verso il basso e selezionare `DATABASE_URL`; questo è il nome della variabile configurata affinché locallibrary la legga come variabile d'ambiente.

![Schermata del sito Railway che seleziona un DATABASE_URL](railway_postgresql_connect.png)

Quindi selezionare **Add** per aggiungere il riferimento alla variabile e infine **Deploy**, che apparirà in una finestra pop-up.
Si noti che sarebbe stato possibile anche aprire il database Postgres, quindi la scheda delle variabili, e copiare la variabile.

Se si apre ora il progetto, dovrebbe essere visualizzato esattamente come in locale.
Si noti tuttavia che non esiste ancora un modo per popolare la libreria con dati, poiché non è stato ancora creato un account superuser.
Questo verrà fatto usando lo strumento [CLI](https://docs.railway.com/cli) sul computer locale.

### Installare il client

Scaricare e installare il client Railway per il sistema operativo locale seguendo le [istruzioni disponibili qui](https://docs.railway.com/cli).

Dopo aver installato il client sarà possibile eseguire comandi.
Alcune delle operazioni più importanti includono la distribuzione della directory corrente del computer in un progetto Railway associato, senza dover caricare su GitHub, e l'esecuzione locale del progetto Django usando le stesse impostazioni presenti sul server di produzione.
Queste operazioni vengono mostrate nelle sezioni seguenti.

È possibile ottenere un elenco di tutti i possibili comandi immettendo quanto segue in un terminale.

```bash
railway help
```

> [!NOTE]
> Nella sezione seguente vengono usati `railway login` e `railway link` per collegare il progetto corrente a una directory.
> Se il sistema effettua il logout, sarà necessario chiamare di nuovo entrambi i comandi per ricollegare il progetto.

### Configurare un superuser

Per creare un superuser, è necessario chiamare il comando Django `createsuperuser` sul database di produzione; è la stessa operazione eseguita localmente in [Tutorial Django - Parte 4: Sito di amministrazione Django > Creare un superuser](/it/docs/Learn_web_development/Extensions/Server-side/Django/Admin_site#creating_a_superuser).
Railway non fornisce accesso diretto al terminale del server e non è possibile aggiungere questo comando al [Procfile](#procfile) perché è interattivo.

Ciò che è possibile fare è chiamare questo comando localmente nel progetto Django quando è connesso al database di _produzione_.
Il client Railway semplifica questa operazione offrendo un meccanismo per eseguire comandi localmente usando le stesse variabili d'ambiente del server di produzione, inclusa la stringa di connessione al database.

Aprire innanzitutto un terminale o un prompt dei comandi in un clone git del progetto locallibrary.
Quindi accedere all'account nel browser usando il comando `login` o `login --browserless`; seguire eventuali prompt e istruzioni del client o del sito web per completare l'accesso:

```bash
railway login
```

Dopo aver effettuato l'accesso, collegare la directory locallibrary corrente al progetto Railway associato usando il comando seguente.
Si noti che sarà necessario selezionare/immettere un progetto specifico quando richiesto:

```bash
railway link
```

Ora che la directory locale e il progetto sono _collegati_, è possibile eseguire il progetto Django locale con le impostazioni dell'ambiente di produzione.
Assicurarsi innanzitutto che il normale [ambiente di sviluppo Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/development_environment) sia pronto.
Quindi chiamare il comando seguente, immettendo nome, email e password secondo necessità:

```bash
railway run python manage.py createsuperuser
```

A questo punto dovrebbe essere possibile aprire l'area di amministrazione del sito (`https://[your-url].railway.app/admin/`) e popolare il database, proprio come mostrato in [Tutorial Django - Parte 4: Sito di amministrazione Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/Admin_site)).

### Impostare le variabili di configurazione

Il passaggio finale consiste nel rendere sicuro il sito.
Nello specifico, è necessario disabilitare la registrazione di debug e impostare una chiave CSRF segreta.
Il lavoro per leggere i valori necessari dalle variabili d'ambiente è stato eseguito in [preparare il sito web per la pubblicazione](#preparare_il_sito_web_per_la_pubblicazione); vedere `DJANGO_DEBUG` e `DJANGO_SECRET_KEY`.

Aprire la schermata delle informazioni del progetto e selezionare la scheda _Variables_.
Dovrebbe già contenere `DATABASE_URL`, come mostrato di seguito.

![Railway - schermata di aggiunta di una nuova variabile](railway_variable_new.png)

Esistono molti modi per generare una chiave segreta crittograficamente sicura.
Un modo semplice consiste nell'eseguire il seguente comando Python sul computer di sviluppo:

```bash
python -c "import secrets; print(secrets.token_urlsafe())"
```

Selezionare il pulsante **New Variable** e immettere la chiave `DJANGO_SECRET_KEY` con il valore segreto, quindi selezionare **Add**.
Quindi immettere la chiave `DJANGO_DEBUG` con il valore `False`.
L'insieme finale di variabili dovrebbe apparire così:

![Schermata Railway che mostra tutte le variabili del progetto](railway_variables_all.png)

### Debug

Il client Railway fornisce il comando logs per mostrare la parte finale dei log; un log più completo è disponibile sul sito per ciascun progetto:

```bash
railway logs
```

Se è necessaria una quantità maggiore di informazioni rispetto a quella che questo può fornire, sarà necessario iniziare a esaminare il [logging Django](https://docs.djangoproject.com/en/5.0/topics/logging/).

## Riepilogo

Questo conclude il tutorial sulla configurazione delle app Django in produzione e anche la serie di tutorial sul lavoro con Django. Si spera che siano risultati utili. È possibile consultare una versione completamente sviluppata del [codice sorgente su GitHub qui](https://github.com/mdn/django-locallibrary-tutorial).

Il passaggio successivo consiste nel leggere gli ultimi articoli e poi completare l'attività di valutazione.

## Vedi anche

- [Distribuire Django](https://docs.djangoproject.com/en/5.0/howto/deployment/) (documentazione Django)
  - [Lista di controllo per la distribuzione](https://docs.djangoproject.com/en/5.0/howto/deployment/checklist/) (documentazione Django)
  - [Distribuire file statici](https://docs.djangoproject.com/en/5.0/howto/static-files/deployment/) (documentazione Django)
  - [Come distribuire con WSGI](https://docs.djangoproject.com/en/5.0/howto/deployment/wsgi/) (documentazione Django)
  - [Come usare Django con Apache e mod_wsgi](https://docs.djangoproject.com/en/5.0/howto/deployment/wsgi/modwsgi/) (documentazione Django)
  - [Come usare Django con Gunicorn](https://docs.djangoproject.com/en/5.0/howto/deployment/wsgi/gunicorn/) (documentazione Django)

- Documentazione Railway
  - [CLI](https://docs.railway.com/cli)

- DigitalOcean
  - [Come servire applicazioni Django con uWSGI e Nginx su Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-serve-django-applications-with-uwsgi-and-nginx-on-ubuntu-16-04)
  - [Altra documentazione della community Django di DigitalOcean](https://www.digitalocean.com/community/tutorials?q=django)

- Documentazione Heroku (concetti di configurazione simili)
  - [Configurare app Django per Heroku](https://devcenter.heroku.com/articles/django-app-configuration) (documentazione Heroku)
  - [Iniziare a usare Heroku con Django](https://devcenter.heroku.com/articles/getting-started-with-python#introduction) (documentazione Heroku)
  - [Django e asset statici](https://devcenter.heroku.com/articles/django-assets) (documentazione Heroku)
  - [Concorrenza e connessioni al database in Django](https://devcenter.heroku.com/articles/python-concurrency-and-database-connections) (documentazione Heroku)
  - [Come funziona Heroku](https://devcenter.heroku.com/articles/how-heroku-works) (documentazione Heroku)
  - [Dyno e Dyno Manager](https://devcenter.heroku.com/articles/dynos) (documentazione Heroku)
  - [Configurazione e Config Vars](https://devcenter.heroku.com/articles/config-vars) (documentazione Heroku)
  - [Limiti](https://devcenter.heroku.com/articles/limits) (documentazione Heroku)
  - [Distribuire applicazioni Python con Gunicorn](https://devcenter.heroku.com/articles/python-gunicorn) (documentazione Heroku)
  - [Lavorare con Django](https://devcenter.heroku.com/categories/working-with-django) (documentazione Heroku)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Testing", "Learn_web_development/Extensions/Server-side/Django/web_application_security", "Learn_web_development/Extensions/Server-side/Django")}}
