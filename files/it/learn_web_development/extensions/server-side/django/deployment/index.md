---
title: "Tutorial Django Parte 11: Deployment di Django in produzione"
short-title: "11: Deployment"
slug: Learn_web_development/Extensions/Server-side/Django/Deployment
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Testing", "Learn_web_development/Extensions/Server-side/Django/web_application_security", "Learn_web_development/Extensions/Server-side/Django")}}

Ora che hai creato (e testato) un fantastico sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website), vorrai installarlo su un server web pubblico in modo che possa essere accessibile dallo staff della biblioteca e dai membri su Internet. Questo articolo fornisce una panoramica di come trovare un host per il tuo sito web, e cosa devi fare per preparare il tuo sito per la produzione.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Completa tutti i temi precedenti del tutorial, inclusa la <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/Testing">Parte 10 del Tutorial Django: Test dell'applicazione web Django</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Imparare dove e come puoi distribuire un'app Django in produzione.</td>
    </tr>
  </tbody>
</table>

## Panoramica

Una volta che il tuo sito è terminato (o abbastanza completo per iniziare il test pubblico), avrai bisogno di ospitarlo da qualche parte più pubblica e accessibile rispetto al tuo computer di sviluppo personale.

Fino a questo momento hai lavorato in un ambiente di sviluppo, usando il server web di sviluppo di Django per condividere il tuo sito con il browser/rete locale, e gestendo il tuo sito web con impostazioni di sviluppo (insicure) che espongono il debug e altre informazioni private. Prima di poter ospitare un sito web esternamente dovrai prima:

- Apportare alcune modifiche alle impostazioni del tuo progetto.
- Scegliere un ambiente per ospitare l'app Django.
- Scegliere un ambiente per ospitare eventuali file statici.
- Configurare un'infrastruttura a livello di produzione per servire il tuo sito web.

Questo tutorial fornisce una guida sulle tue opzioni per scegliere un sito di hosting, una breve panoramica su cosa devi fare per preparare la tua app Django per la produzione, e un esempio pratico su come installare il sito LocalLibrary sul servizio di cloud hosting [Railway](https://railway.com/).

## Cos'è un ambiente di produzione?

L'ambiente di produzione è quello fornito dal computer server dove eseguirai il tuo sito web per il consumo esterno. L'ambiente include:

- Hardware del computer su cui gira il sito web.
- Sistema operativo (es. Linux, Windows).
- Runtime del linguaggio di programmazione e librerie di framework su cui è scritto il tuo sito web.
- Server web usato per servire pagine e altri contenuti (es. Nginx, Apache).
- Application server che passa richieste "dinamiche" tra il tuo sito web Django e il server web.
- Database su cui dipende il tuo sito web.

> [!NOTE]
> A seconda di come è configurato il tuo ambiente di produzione potresti avere anche un reverse proxy, un bilanciatore di carico, ecc.

Il computer server potrebbe essere collocato nei tuoi locali e connesso a Internet tramite un collegamento veloce, ma è molto più comune usare un computer ospitato "nel cloud". Questo significa che il tuo codice viene eseguito su un computer remoto (o forse un "computer virtuale") nei data center della tua compagnia di hosting. Il server remoto offrirà solitamente un certo livello garantito di risorse di calcolo (CPU, RAM, memoria di archiviazione, ecc.) e connettività Internet per un certo prezzo.

Questo tipo di hardware di computing/rete accessibile da remoto è chiamato _Infrastructure as a Service (IaaS)_. Molti fornitori di IaaS offrono opzioni per preinstallare un particolare sistema operativo, su cui devi installare gli altri componenti del tuo ambiente di produzione. Altri fornitori ti permettono di selezionare ambienti più completi, forse includendo un setup completo di Django e server web.

> [!NOTE]
> Gli ambienti pre-costruiti possono rendere molto facile configurare il tuo sito web perché riducono la configurazione, ma le opzioni disponibili potrebbero limitarti a un server sconosciuto (o ad altri componenti) e potrebbero essere basate su una versione più vecchia del sistema operativo. Spesso è meglio installare i componenti da soli, in modo da ottenere quelli che desideri, e quando hai bisogno di aggiornare parti del sistema, sai da dove iniziare!

Altri fornitori di hosting supportano Django come parte di un'offerta di _Platform as a Service_ (PaaS). In questo tipo di hosting non devi preoccuparti della maggior parte del tuo ambiente di produzione (server web, application server, bilanciatori di carico) poiché la piattaforma host si occupa di quelli per te — insieme a quasi tutto ciò che devi fare per scalare la tua applicazione. Questo rende il deployment piuttosto facile, perché devi solo concentrarti sulla tua applicazione web e non su tutta l'altra infrastruttura del server.

Alcuni sviluppatori sceglieranno la maggiore flessibilità offerta da IaaS rispetto a PaaS, mentre altri apprezzeranno la ridotta manutenzione e il facile scaling di PaaS. Quando stai iniziando, configurare il tuo sito web su un sistema PaaS è molto più semplice, quindi è quello che faremo in questo tutorial.

> [!NOTE]
> Se scegli un fornitore di hosting compatibile con Python/Django dovrebbero fornire istruzioni su come configurare un sito web Django usando diverse configurazioni di web server, application server, reverse proxy, ecc. (questo non sarà rilevante se scegli un PaaS). Ad esempio, ci sono molte guide passo-passo per varie configurazioni nei [documenti della comunità Django di DigitalOcean](https://www.digitalocean.com/community/tutorials?q=django).

## Scegliere un fornitore di hosting

Ci sono molti fornitori di hosting noti per supportare attivamente o funzionare bene con Django, tra cui: [Heroku](https://www.heroku.com/), [DigitalOcean](https://www.digitalocean.com/), [Railway](https://railway.com/), [Python Anywhere](https://www.pythonanywhere.com/), [Amazon Web Services](https://aws.amazon.com/), [Azure](https://azure.microsoft.com/en-us/), [Google Cloud](https://cloud.google.com/), [Hetzner](https://www.hetzner.com/), e [Vultr Cloud Compute](https://blogs.vultr.com/new-free-tier-plan) — per nominarne alcuni. Questi fornitori offrono diversi tipi di ambienti (IaaS, PaaS), e diversi livelli di risorse di calcolo e di rete a prezzi differenti.

Alcune delle cose da considerare quando si sceglie un host:

- Quanto traffico è probabile che riceva il tuo sito e il costo delle risorse di dati e calcolo richieste per soddisfare quella domanda.
- Livello di supporto per lo scaling orizzontale (aggiungendo più macchine) e verticale (aggiornando a macchine più potenti) e i costi per farlo.
- Dove il fornitore ha i suoi data center, e quindi dove l'accesso è probabilmente più veloce.
- Le prestazioni storiche di uptime e downtime dell'host.
- Gli strumenti forniti per gestire il sito — sono facili da usare e sono sicuri (es. SFTP vs. FTP).
- Framework integrati per monitorare il tuo server.
- Limitazioni conosciute. Alcuni host bloccheranno deliberatamente determinati servizi (es. email). Altri offrono solo un certo numero di ore di "tempo live" in alcuni livelli di prezzo, o offrono solo una piccola quantità di spazio di archiviazione.
- Vantaggi aggiuntivi. Alcuni fornitori offrono nomi di dominio gratuiti e supporto per certificati TLS per i quali altrimenti dovresti pagare.
- Il livello "gratuito" su cui fai affidamento scade nel tempo, e se il costo di migrazione a un livello più costoso significa che sarebbe stato meglio utilizzare un altro servizio fin dall'inizio!

La buona notizia quando stai iniziando è che ci sono diversi siti che forniscono ambienti di calcolo "gratuiti" destinati alla valutazione e al test. Questi sono solitamente ambienti abbastanza limitati in termini di risorse, e devi essere consapevole che potrebbero scadere dopo un periodo introduttivo o avere altre restrizioni. Tuttavia sono ottimi per testare siti a basso traffico in un ambiente ospitato e possono fornire una facile migrazione a pagare per più risorse quando il tuo sito diventa più trafficato. Scelte popolari in questa categoria includono [Vultr Cloud Compute](https://blogs.vultr.com/new-free-tier-plan), [Python Anywhere](https://www.pythonanywhere.com/), [Amazon Web Services](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/billing-free-tier.html), [Microsoft Azure](https://azure.microsoft.com/en-us/pricing/details/app-service/linux/), e così via.

La maggior parte dei fornitori offre anche un livello "base" destinato a siti di produzione piccoli, e che forniscono livelli più utili di potenza di calcolo e meno limitazioni. [Railway](https://railway.com/), [Heroku](https://www.heroku.com/), e [DigitalOcean](https://www.digitalocean.com/) sono esempi di fornitori di hosting popolari che hanno un livello di calcolo di base relativamente economico (nell'ordine di $5 a $10 USD al mese).

> [!NOTE]
> Ricorda che il prezzo non è l'unico criterio di selezione. Se il tuo sito web ha successo, potrebbe risultare che la scalabilità è la considerazione più importante.

## Preparare il tuo sito web per la pubblicazione

Il [sito web scheletrico di Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/skeleton_website) creato usando gli strumenti _django-admin_ e _manage.py_ è configurato per facilitare lo sviluppo. Molte delle impostazioni del progetto Django (specificate in **settings.py**) dovrebbero essere diverse per la produzione, sia per motivi di sicurezza che di prestazioni.

> [!NOTE]
> È comune avere un file **settings.py** separato per la produzione e/o importare condizionalmente impostazioni sensibili da un file separato o da una variabile d'ambiente. Questo file dovrebbe quindi essere protetto, anche se il resto del codice sorgente è disponibile in un repository pubblico.

Le impostazioni critiche che devi controllare sono:

- `DEBUG`. Questo dovrebbe essere impostato su `False` in produzione (`DEBUG = False`). Questo impedisce che le tracce di debug sensibili/confidenziali e le informazioni sulle variabili vengano visualizzate.
- `SECRET_KEY`. Questo è un valore casuale grande usato per la protezione CSRF, ecc. È importante che la chiave usata in produzione non sia nel controllo del codice sorgente o accessibile al di fuori del server di produzione.

I documenti di Django suggeriscono che le informazioni segrete potrebbero essere caricate al meglio da una variabile d'ambiente o lette da un file disponibile solo sul server.
Modificheremo l'applicazione _LocalLibrary_ in modo da leggere le nostre variabili `SECRET_KEY` e `DEBUG` da variabili d'ambiente se sono definite, passando poi a valori definiti in un file **.env** nella radice, e infine usando i valori predefiniti nel file di configurazione.
Questo è molto flessibile poiché consente qualsiasi configurazione supportata dal server di hosting.

Per leggere i valori delle variabili d'ambiente da un file useremo [python-dotenv](https://pypi.org/project/python-dotenv/).
Questa è una libreria per leggere coppie chiave-valore da un file e usarle come variabili d'ambiente, ma solo se la corrispondente variabile d'ambiente non è definita.

Installa la libreria nel tuo ambiente virtuale come mostrato (e aggiorna anche il tuo file `requirements.txt`):

```bash
pip3 install python-dotenv
```

Quindi apri **/locallibrary/settings.py** e inserisci il seguente codice dopo `BASE_DIR` è definito, ma prima dell'avviso di sicurezza: `# SECURITY WARNING: keep the secret key used in production secret!`

```python
# Support env variables from .env file if defined
import os
from dotenv import load_dotenv
env_path = load_dotenv(os.path.join(BASE_DIR, '.env'))
load_dotenv(env_path)
```

Questo carica il file `.env` dalla radice dell'applicazione web.
Le variabili definite come `KEY=VALUE` nel file vengono importate quando la chiave viene utilizzata in `os.environ.get('<KEY>', '<DEFAULT VALUE>')`, se definite.

> [!NOTE]
> Qualsiasi valore aggiunto a **.env** probabilmente è un _segreto_!
> Non devi salvarle su GitHub, e dovresti aggiungere `.env` al tuo file `.gitignore` in modo che non venga aggiunto per errore.

Successivamente disabilita la configurazione originale `SECRET_KEY` e aggiungi le nuove righe come mostrato di seguito.
Durante lo sviluppo non verrà specificata alcuna variabile d'ambiente per la chiave, quindi verrà utilizzato il valore predefinito (non dovrebbe importare quale chiave usi qui, o se la chiave "viene rivelata", poiché non la userai in produzione).

```python
# SECURITY WARNING: keep the secret key used in production secret!
# SECRET_KEY = 'django-insecure-&psk#na5l=p3q8_a+-$4w1f^lt3lx1c@d*p4x$ymm_rn7pwb87'
import os
SECRET_KEY = os.environ.get('DJANGO_SECRET_KEY', 'django-insecure-&psk#na5l=p3q8_a+-$4w1f^lt3lx1c@d*p4x$ymm_rn7pwb87')
```

Quindi commenta l'impostazione esistente `DEBUG` e aggiungi la nuova riga mostrata di seguito.

```python
# SECURITY WARNING: don't run with debug turned on in production!
# DEBUG = True
DEBUG = os.environ.get('DJANGO_DEBUG', '') != 'False'
```

Il valore di `DEBUG` sarà `True` per impostazione predefinita, ma sarà `False` solo se il valore della variabile d'ambiente `DJANGO_DEBUG` è impostato su `False` o `DJANGO_DEBUG=False` è impostato nel file **.env**. Si noti che le variabili d'ambiente sono stringhe e non tipi Python. Dobbiamo quindi confrontare stringhe. L'unico modo per impostare la variabile `DEBUG` su `False` è effettivamente impostarla sulla stringa `False`.

Puoi impostare la variabile d'ambiente su "False" su Linux emettendo il seguente comando:

```bash
export DJANGO_DEBUG=False
```

Un elenco completo delle impostazioni che potresti voler modificare è fornito nella [lista di controllo per il deployment](https://docs.djangoproject.com/en/5.0/howto/deployment/checklist/) (documenti di Django). Puoi anche elencarne un certo numero usando il comando del terminale di seguito:

```python
python3 manage.py check --deploy
```

### Gunicorn

[Gunicorn](https://gunicorn.org/) è un server HTTP puro-Python che viene comunemente usato per servire applicazioni WSGI Django.

Anche se non abbiamo bisogno di _Gunicorn_ per servire la nostra applicazione LocalLibrary durante lo sviluppo, lo installeremo localmente in modo che diventi parte dei nostri [requisiti](#requisiti) quando l'applicazione verrà distribuita.

Prima assicurati di essere nell'ambiente virtuale Python creato quando hai [configurato l'ambiente di sviluppo](/it/docs/Learn_web_development/Extensions/Server-side/Django/development_environment) (usa il comando `workon [nome-dell-ambiente-virtuale]`).
Quindi installa _Gunicorn_ localmente sulla riga di comando usando _pip_:

```bash
pip3 install gunicorn
```

### Configurazione del database

SQLite, il database Django predefinito che hai utilizzato per lo sviluppo, è una scelta ragionevole per siti piccoli e medi.
Sfortunatamente non può essere usato su alcuni popolari servizi di hosting, come Heroku, perché non forniscono storage dati persistente nell'ambiente dell'applicazione (un requisito di SQLite).
Anche se ciò potrebbe non influenzarci per il/i deployment di esempio, ti mostreremo un altro approccio che funzionerà su Railway, Heroku e altri servizi.

L'approccio consiste nell'usare un database che viene eseguito nel suo processo da qualche parte su Internet e viene accesso dall'applicazione biblioteca Django usando un indirizzo passato come variabile d'ambiente.
In questo caso useremo un database Postgres che è ospitato anche su Railway, ma potresti usare qualsiasi servizio di hosting database che preferisci.

Le informazioni sulla connessione al database verranno fornite a Django usando una variabile d'ambiente chiamata `DATABASE_URL`.
Piuttosto che codificare a mano queste informazioni in Django, useremo il pacchetto [dj-database-url](https://pypi.org/project/dj-database-url/) per analizzare la variabile d'ambiente `DATABASE_URL` e convertirla automaticamente nel formato di configurazione desiderato da Django.
Oltre a installare il pacchetto _dj-database-url_ avremo anche bisogno di installare [psycopg2](https://www.psycopg.org/), poiché Django ne ha bisogno per interagire con i database Postgres.

#### dj-database-url

_dj-database-url_ viene utilizzato per estrarre la configurazione del database Django da una variabile d'ambiente.

Installalo localmente in modo che diventi parte dei nostri [requisiti](#requisiti) da configurare sul server di distribuzione:

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

Django ora utilizzerà la configurazione del database in `DATABASE_URL` se la variabile d'ambiente è impostata; altrimenti usa il database SQLite predefinito.
Il valore `conn_max_age=500` rende la connessione persistente, il che è molto più efficiente che ricreare la connessione ad ogni ciclo di richiesta (questo è opzionale e può essere rimosso se necessario).

#### psycopg2

<!-- Django 4.2 now supports Psycopg (3) : https://docs.djangoproject.com/en/5.0/releases/4.2/#psycopg-3-support
  Ma non ha funzionato su Railway!
  Provare a fare aggiornamento nella prossima versione.
-->

Django ha bisogno di _psycopg2_ per lavorare con i database Postgres.
Installalo localmente in modo che diventi parte dei nostri [requisiti](#requisiti) per Railway da configurare sul server remoto:

```bash
pip3 install psycopg2-binary
```

Nota che Django utilizzerà il database SQLite durante lo sviluppo per impostazione predefinita, a meno che `DATABASE_URL` non sia impostato.
Puoi passare completamente a Postgres e usare lo stesso database ospitato per sviluppo e produzione impostando la stessa variabile d'ambiente nel tuo ambiente di sviluppo (Railway rende facile usare lo stesso ambiente per produzione e sviluppo).
In alternativa puoi anche installare e utilizzare un [database Postgres autogestito](https://www.psycopg.org/docs/install.html) sul tuo computer locale.

### Servire i file statici in produzione

Durante lo sviluppo usiamo Django e il server web di sviluppo di Django per servire sia il nostro HTML dinamico che i nostri file statici (CSS, JavaScript, ecc.).
Questo è inefficiente per i file statici, perché le richieste devono passare attraverso Django anche se Django non fa niente con loro.
Anche se questo non importa durante lo sviluppo, avrebbe un impatto significativo sulle prestazioni se dovessimo usare lo stesso approccio in produzione.

Nell'ambiente di produzione solitamente separiamo i file statici dall'applicazione web Django, rendendoli più facili da servire direttamente dal server web o da una rete di distribuzione dei contenuti (CDN).

Le variabili di impostazione importanti sono:

- `STATIC_URL`: Questo è il percorso URL di base da cui verranno serviti i file statici, ad esempio su un CDN.
- `STATIC_ROOT`: Questo è il percorso assoluto di una directory dove verranno raccolti i file statici del tool _collectstatic_ di Django referenziati nei nostri template. Una volta raccolti, possono poi essere caricati in gruppo ovunque i file debbano essere ospitati.
- `STATICFILES_DIRS`: Questo elenca directory aggiuntive che il tool _collectstatic_ di Django dovrebbe cercare per i file statici.

I template Django si riferiscono alle posizioni dei file statici in relazione a un tag `static` (puoi vederlo nel template di base definito in [Django Tutorial Parte 5: Creare la nostra homepage](/it/docs/Learn_web_development/Extensions/Server-side/Django/Home_page#the_locallibrary_base_template)), che a sua volta mappa l'impostazione `STATIC_URL`.
I file statici possono quindi essere caricati su qualsiasi host e puoi aggiornare la tua applicazione per trovarli usando questa impostazione.

Il tool _collectstatic_ viene utilizzato per raccogliere file statici nella cartella definita dall'impostazione del progetto `STATIC_ROOT`.
Si chiama con il seguente comando:

```bash
python3 manage.py collectstatic
```

Per questo tutorial, _collectstatic_ può essere eseguito prima che l'applicazione venga caricata, copiando tutti i file statici nell'applicazione nel percorso specificato in `STATIC_ROOT`.
`Whitenoise` trova quindi i file dal percorso definito da `STATIC_ROOT` (per impostazione predefinita) e li serve all'URL di base definito in `STATIC_URL`.

#### settings.py

Apri **/locallibrary/settings.py** e copia la seguente configurazione nella parte inferiore del file.
Il `BASE_DIR` dovrebbe essere già stato definito nel tuo file (il `STATIC_URL` potrebbe essere già stato definito all'interno del file quando è stato creato.
Anche se non causerà alcun danno, puoi anche eliminare il precedente riferimento duplicato).

```python
# Static files (CSS, JavaScript, Images)
# https://docs.djangoproject.com/en/5.0/howto/static-files/

# The absolute path to the directory where collectstatic will collect static files for deployment.
STATIC_ROOT = BASE_DIR / 'staticfiles'

# The URL to use when referring to static files (where they will be served from)
STATIC_URL = '/static/'
```

Serviremo effettivamente i file usando una libreria chiamata [WhiteNoise](https://pypi.org/project/whitenoise/), che installeremo e configureremo nella sezione successiva.

### Whitenoise

Ci sono molti modi per servire file statici in produzione (abbiamo visto le relative impostazioni di Django nelle sezioni precedenti).
Il progetto [WhiteNoise](https://pypi.org/project/whitenoise/) fornisce uno dei metodi più semplici per servire risorse statiche direttamente da Gunicorn in produzione.

Consulta la documentazione [WhiteNoise](https://pypi.org/project/whitenoise/) per una spiegazione di come funziona e perché l'implementazione è un metodo relativamente efficiente per servire questi file.

I passi per configurare _WhiteNoise_ da usare con il progetto sono [elencati qui](https://whitenoise.readthedocs.io/en/stable/django.html) (e riprodotti di seguito):

#### Installare whitenoise

Installa whitenoise localmente usando il seguente comando:

```bash
pip3 install whitenoise
```

#### settings.py

Per installare _WhiteNoise_ nella tua applicazione Django, apri **/locallibrary/settings.py**, trova l'impostazione `MIDDLEWARE` e aggiungi `WhiteNoiseMiddleware` vicino alla cima della lista, subito sotto `SecurityMiddleware`:

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

Opzionalmente, puoi ridurre la dimensione dei file statici quando vengono serviti (questo è più efficiente).
Basta aggiungere quanto segue alla fine di **/locallibrary/settings.py**:

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

Non devi fare nient'altro per configurare _WhiteNoise_ perché usa le impostazioni del tuo progetto per `STATIC_ROOT` e `STATIC_URL` per impostazione predefinita.

### Requisiti

I requisiti Python della tua applicazione web dovrebbero essere memorizzati in un file **requirements.txt** nella radice del tuo repository.
Molti servizi di hosting installeranno automaticamente le dipendenze in questo file (in altri devi farlo tu stesso).
Puoi creare questo file usando _pip_ sulla riga di comando (esegui il seguente comando nella radice del repo):

```bash
pip3 freeze > requirements.txt
```

Dopo aver installato tutte le diverse dipendenze sopra, il tuo file **requirements.txt** dovrebbe avere _almeno_ questi articoli elencati (anche se i numeri di versione possono essere diversi).
Per favore elimina eventuali altre dipendenze non elencate qui, a meno che tu non le abbia aggiunte esplicitamente per questa applicazione.

```plain
Django==5.0.2
dj-database-url==2.1.0
gunicorn==21.2.0
psycopg2-binary==2.9.9
wheel==0.38.1
whitenoise==6.6.0
python-dotenv==1.0.1
```

### Aggiorna il tuo repository dell'applicazione su GitHub

Molti servizi di hosting ti permettono di importare e/o sincronizzare progetti da un repository locale o da piattaforme di controllo versione cloud.
Questo può rendere molto più semplice il deploy e lo sviluppo iterativo.

Dovresti già usare GitHub per memorizzare il codice sorgente della biblioteca locale (questo è stato impostato in [Gestione del codice sorgente con Git e GitHub](/it/docs/Learn_web_development/Extensions/Server-side/Django/development_environment#source_code_management_with_git_and_github) come parte della configurazione del tuo ambiente di sviluppo.

Questo è un buon punto per fare un backup del tuo progetto "vanilla" — mentre alcuni dei cambiamenti che faremo nelle sezioni successive potrebbero essere utili per il deploy su qualsiasi servizio di hosting (o per lo sviluppo) altri potrebbero non esserlo.
Assumendo che tu abbia già fatto il backup di tutte le modifiche apportate finora sul branch `main` su GitHub, puoi creare un nuovo branch per il backup delle modifiche come mostrato:

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

Questa sezione fornisce una dimostrazione pratica di come ospitare _LocalLibrary_ su [PythonAnywhere](https://www.pythonanywhere.com/).

### Perché PythonAnywhere?

Abbiamo scelto di utilizzare PythonAnywhere per diversi motivi:

- PythonAnywhere offre un [piano iniziale gratuito](https://www.pythonanywhere.com/pricing/) che è davvero gratuito, anche se con alcune limitazioni.
  Il fatto che sia accessibile a tutti gli sviluppatori è davvero importante per MDN!

  > [!NOTE]
  > Questo tutorial è stato ospitato su Heroku, Railway, e ora PythonAnywhere, migrando quando i piani gratuiti precedenti sono stati interrotti.
  > Abbiamo scelto PythonAnywhere perché pensiamo che questo piano sia probabile che rimanga gratuito.
  > Abbiamo mantenuto anche l'esempio Railway, che non è gratuito, per un confronto, e perché ci permette di dimostrare più facilmente funzionalità come l'integrazione con un database Postgres in esecuzione su un servizio diverso.

- PythonAnywhere si occupa dell'infrastruttura quindi non devi farlo tu.
  Non dover preoccuparsi di server, bilanciatori di carico, reverse proxy, ecc., rende molto più facile iniziare.
- Le competenze e i concetti che imparerai utilizzando PythonAnywhere sono trasferibili.
- I limiti del servizio e del piano non influenzano particolarmente l'uso di PythonAnywhere per il tutorial.
  Ad esempio:

  - Il piano iniziale consente una sola web app su `<your-username>.pythonanywhere.com`, accesso Internet in uscita limitato dalle tue app, CPU/banda ridotta, nessun supporto IPython/Jupyter notebook, nessun database Postgres gratuito.
    Ma c'è abbastanza spazio per il nostro sito di base per funzionare!
  - I domini personalizzati non sono supportati (al momento della scrittura).
  - L'ambiente si spegne quando non è in uso, quindi potrebbe essere lento a ripartire.
    Puoi farlo funzionare per sempre, ma dovrai visitare il sito ogni tre mesi e rinnovare l'applicazione web.
  - C'è supporto gratuito per un database MySQL separato, ma non Postgres.
    In questa dimostrazione useremo solo il database SQLite predefinito di Django.

PythonAnywhere è appropriato per ospitare questa dimostrazione, e può essere scalato per progetti più grandi se necessario.
Dovresti prenderti il tempo per determinare se è [adatto al tuo sito web](#scegliere_un_fornitore_di_hosting).

### Come funziona PythonAnywhere?

PythonAnywhere fornisce un'interfaccia completamente basata sul web per caricare, modificare e lavorare in altro modo con la tua applicazione.

Attraverso l'interfaccia puoi avviare una console bash su un ambiente Ubuntu Linux in cui puoi creare la tua applicazione.
In questa dimostrazione useremo la console per clonare il nostro repository locale della biblioteca su GitHub e creare un ambiente Python in cui possiamo eseguire l'applicazione web.

Il piano gratuito non offre supporto Postgres separato.
Anche se potremmo usare qualche altro servizio di hosting per il nostro database, useremo solo il database SQLite predefinito creato da Django nell'ambiente Ubuntu ospitato (c'è più che abbastanza spazio per dimostrare la funzionalità della biblioteca).

Una volta che l'applicazione è in esecuzione, può essere configurata per la produzione impostando variabili d'ambiente tramite la console bash.

Questa è tutta la panoramica di cui hai bisogno per iniziare.

### Ottieni un account PythonAnywhere

Per iniziare a usare PythonAnywhere avrai prima bisogno di creare un account:

- Vai alla pagina [Piani e prezzi](https://www.pythonanywhere.com/pricing/) di PythonAnywhere, e seleziona il pulsante **Crea un account per principianti**.
- Crea un account con il tuo username, email, e password, riconosci i termini e condizioni, e seleziona **Registra**.
- Sarai quindi connesso e reindirizzato alla dashboard di PythonAnywhere: `https://www.pythonanywhere.com/user/<your_user_name>/`.

### Installa la biblioteca da GitHub

Successivamente apriremo un prompt Bash, imposteremo un ambiente virtuale, e prenderemo il codice sorgente della biblioteca locale da GitHub.
Configureremo anche il database predefinito e raccoglieremo file statici in modo che possano essere serviti da PythonAnywhere.

1. Apri prima di tutto la schermata di gestione console selezionando **Consoles** nella barra superiore delle applicazioni.
2. Quindi seleziona il link **Bash** per creare e avviare una nuova console:

   ![Immagine della schermata di gestione Console di PythonAnywhere](python_anywhere_start_bash_console.png)

   Nota che qualsiasi console che crei viene salvata per il tuo successivo riutilizzo, insieme a tutta la sua cronologia.
   La freccia verde sopra mostra che questo account ha una console che avremmo potuto aprire invece.

3. Nella console, inserisci il seguente comando per creare un ambiente virtuale Python 3.10 chiamato "env_local_library" per installare le dipendenze della biblioteca locale.

   ```bash
   mkvirtualenv --python=python3.10 env_local_library
   ```

   Questo è esattamente lo stesso processo coperto in [Configura un ambiente di sviluppo Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/development_environment).
   Avremmo potuto nominare l'ambiente in qualsiasi modo, e possiamo disattivarlo e riattivarlo usando i comandi sottostanti:

   ```bash
   deactivate
   workon env_local_library
   ```

4. Quindi ottenere i sorgenti della biblioteca da GitHub.
   PythonAnywhere si aspetta che tu installi le applicazioni in una cartella denominata come l'URL del tuo sito.

   > [!NOTE]
   > Poiché stiamo usando l'account gratuito puoi solo nominare il tuo account `<your_pythonanywhere_username>.pythonanywhere.com` (ad esempio, se il tuo username è "Odtsetseg" dovrai mettere il codice sorgente della biblioteca locale in una cartella chiamata `odtsetseg.pythonanywhere.com`).

   Inserisci il seguente comando per clonare i sorgenti della tua biblioteca in una cartella dal nome appropriato (dovrai sostituire i valori degli username con il tuo nome):

   ```bash
   git clone https://github.com/<github_username>/django-locallibrary-tutorial.git <your_pythonanywhere_username>.pythonanywhere.com

   # Navigate into the new folder
   cd <your_pythonanywhere_username>.pythonanywhere.com
   ```

5. Installa le dipendenze della biblioteca usando il file `requirements.txt`:

   ```bash
   pip3 install -r requirements.txt
   ```

6. Crea e configura un database SQLite sul computer di hosting (come abbiamo fatto durante lo sviluppo).

   ```bash
   python manage.py migrate
   ```

   > [!NOTE]
   > Per l'esempio Railway configureremo un [database Postgres](#provision_e_connessione_a_un_database_postgres_sql), e ci connetteremo a esso impostando la variabile d'ambiente `DATABASE_URL`.
   > È importante che `migrate` venga chiamato _dopo_ aver configurato quale database utilizzare.

7. Raccogli tutti i file statici in una posizione dove possano essere [serviti in produzione](#servire_i_file_statici_in_produzione):

   ```bash
   python manage.py collectstatic --no-input
   ```

8. Crea un superutente per accedere al sito (come trattato nella sezione [Sito admin Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/Admin_site#creating_a_superuser)):

   ```bash
   python manage.py createsuperuser
   ```

   Annota i dettagli, poiché ne avrai bisogno per testare il tuo sito.

### Imposta l'app web

Dopo aver ottenuto i sorgenti della biblioteca locale e installato le dipendenze in un ambiente virtuale, dobbiamo dire a PythonAnywhere come trovarle e usarle come app web.

1. Naviga nella sezione _Web_ del sito e seleziona il link **Aggiungi una nuova web app**:

   ![Sezione "Web" di PythonAnywhere che mostra il pulsante per aggiungere una nuova app](python_anywhere_web_add_new_app.png)

   L'assistente _Crea nuova web app_ si aprirà quindi per guidarti nella configurazione delle proprietà principali dell'app web.

2. Seleziona **Next** per passare oltre la configurazione del nome di dominio dell'app web.
   L'account gratuito creerà il dominio in base al tuo nome utente: `<user_name>.pythonanywhere.com`.

   ![Prompt PythonAnywhere per impostare il nome di dominio della nuova app web](python_anywhere_web_add_new_app_prompt.png)

3. Nella schermata _Seleziona un framework Web Python_ seleziona **Manuale**.

   ![Prompt PythonAnywhere per selezionare il framework web utilizzato per l'applicazione](python_anywhere_web_add_select_framework_manual.png)

   La configurazione manuale ci consente il controllo completo su come è configurato l'ambiente.
   Ciò non importa molto ora, ma lo farebbe se ospitassimo più siti, potenzialmente con diverse versioni di Python e/o Django.

4. Nella schermata _Seleziona una versione di Python_ seleziona **3.10**

   ![Prompt PythonAnywhere per selezionare la versione di Python per l'applicazione Web](python_anywhere_web_add_select_python_version.png)

   In generale dovresti selezionare l'ultima versione di Python consentita dalla versione di Django che stai utilizzando.

5. Nella schermata _Configurazione manuale_ seleziona **Next** (la schermata spiega solo alcune delle opzioni per la configurazione)

   ![Prompt PythonAnywhere che spiega le prossime opzioni di configurazione](python_anywhere_web_add_manual_config.png)

   L'app web è creata e visualizzata nella sezione Web come mostrato.
   Lo schermo ha un pulsante **Reload** che puoi usare per ricaricare l'app web dopo aver apportato eventuali ulteriori modifiche.
   Come indicato sullo schermo, dovrai cliccare il pulsante **Run until 3 months from today** per tenere il sito attivo per altri tre mesi (e continuo).

   ![App web configurata di PythonAnywhere](python_anywhere_web_configuration.png)

6. Scorri verso il basso fino alla sezione "Codice" della scheda _Web_ e seleziona il link al file di configurazione WSGI.
   Questo avrà un nome del tipo `/var/www/<user_name>_pythonanywhere_com_wsgi.py`.

   ![File WSGI di PythonAnywhere nella scheda Web, sezione del codice](python_anywhere_web_code_wsgi_select.png)

   Sostituisci il contenuto nel file con il seguente testo (aggiornando prima "hamishwillee" con il tuo username), e quindi seleziona il pulsante **Save**.

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

   Nota che il ruolo del file WSGI è di aiutare il server Gunicorn a trovare l'applicazione biblioteca locale.
   PythonAnywhere si aspetta che questo file sia in questa posizione, motivo per cui il file WSGI già presente nel progetto non può essere utilizzato.

7. Scorri verso il basso fino alla sezione "Virtualenv" della scheda _Web_.
   Seleziona il link **Enter the path to a virtual env, if desired** e inserisci il percorso dell'ambiente virtuale creato nella sezione precedente.
   Se l'hai chiamato "env_local_library" come suggerito, il percorso sarà: `/home/<user_name>/.virtualenvs/env_local_library`

   ![Sezione Virtual env di PythonAnywhere nella scheda Web](python_anywhere_web_virtualenv.png)

8. Scorri verso il basso fino alla sezione "File statici" della scheda _Web_.

   ![Sezione File statici di PythonAnywhere nella scheda Web](python_anywhere_web_static_files.png)

   Seleziona il link **Enter URL** e inserisci `\static_files\`.
   Questo è l'`STATIC_URL` nelle [impostazioni dell'applicazione](#settings.py_2), e riflette il percorso in cui i file sono stati copiati quando abbiamo eseguito `collectstatic` nella sezione precedente.

9. Vicino alla parte superiore della scheda _Web_ seleziona il pulsante **Reload** per riavviare il sito.
   Quindi seleziona il link URL del sito per lanciare il sito live:

![Schermata Web di PythonAnywhere con evidenziato il link per avviare il sito](python_anywhere_web_open_site.png)

### Imposta ALLOWED_HOSTS e CSRF_TRUSTED_ORIGINS

Quando il sito viene aperto, a questo punto vedrai una schermata di errore di debug come mostrato di seguito.
Questo è un errore di sicurezza di Django sollevato perché il nostro codice sorgente non sta eseguendo su un "host consentito".

![Una pagina di errore dettagliata con un traceback completo di un'intestazione HTTP_HOST non valida](python_anywhere_error_disallowed_host.png)

> [!NOTE]
> Questo tipo di informazioni di debug è molto utile quando ti stai preparando, ma è un rischio per la sicurezza in un sito distribuito.
> Nella prossima sezione ti mostreremo come disabilitare questo livello di logging sul sito live usando le [variabili d'ambiente](#usare_variabili_d'ambiente_su_pythonanywhere).

Apri **/locallibrary/settings.py** nel tuo progetto GitHub e modifica l'impostazione [ALLOWED_HOSTS](https://docs.djangoproject.com/en/5.0/ref/settings/#allowed-hosts) per includere l'URL del tuo sito PythonAnywhere:

```python
## For example, for a site URL at 'hamishwillee.pythonanywhere.com'
## (replace the string below with your own site URL):
ALLOWED_HOSTS = ['hamishwillee.pythonanywhere.com', '127.0.0.1']

# During development, you can instead set just the base URL
# (you might decide to change the site a few times).
# ALLOWED_HOSTS = ['.pythonanywhere.com','127.0.0.1']
```

Poiché l'applicazione usa la protezione CSRF, dovrai anche impostare la chiave [CSRF_TRUSTED_ORIGINS](https://docs.djangoproject.com/en/5.0/ref/settings/#csrf-trusted-origins).
Apri **/locallibrary/settings.py** e aggiungi una riga come quella sotto:

```python
## For example, for a site URL is at 'web-production-3640.up.railway.app'
## (replace the string below with your own site URL):
CSRF_TRUSTED_ORIGINS = ['https://hamishwillee.pythonanywhere.com']

# During development/for this tutorial you can instead set just the base URL
# CSRF_TRUSTED_ORIGINS = ['https://*.pythonanywhere.com']
```

Salva queste impostazioni e aggiungile al tuo repository GitHub.

Dovrai quindi aggiornare la versione del tuo progetto su PythonAnywhere.
Supponendo che stai usando il tuo terminale Bash nella cartella `<user_name>.pythonanywhere.com`, e hai spinto le modifiche al branch principale, allora potresti importarle nel prompt Bash usando il comando:

```bash
git pull origin main
```

Usa il pulsante **Restart** nella scheda `Web` per riavviare l'applicazione.
Se aggiorni il tuo sito ospitato, dovrebbe ora aprirsi e mostrare la home page del sito.

Dovresti essere in grado di accedere con l'account superuser che hai creato sopra, e creare autori, generi, libri, ecc., proprio come hai fatto sul tuo computer locale.

### Usare variabili d'ambiente su PythonAnywhere

Nella sezione [Preparare il tuo sito web per la pubblicazione](#preparare_il_tuo_sito_web_per_la_pubblicazione) abbiamo modificato l'applicazione in modo che possa essere configurata usando variabili d'ambiente o variabili in un file **.env** in produzione.

Specifically we set up the library so that you can set:

- `DJANGO_DEBUG=False` per ridurre il tracciamento del debug mostrato all'utente quando c'è un errore.
- `DJANGO_SECRET_KEY` per un qualche valore segreto in produzione.
- `DATABASE_URL` se la tua applicazione usa un database ospitato (non lo facciamo in questo esempio).

Il modo in cui le variabili d'ambiente sono impostate dipende dal servizio di hosting.
Per PythonAnywhere devi leggerle da un file di ambiente.
Siamo già impostati per quello, quindi tutto quello che dobbiamo fare è creare il file.

I passaggi sono:

1. Apri un prompt Bash di PythonAnywhere.
2. Naviga nella directory della tua applicazione (sostituendo `<user-name>` con il tuo account):

   ```bash
   cd ~/<user-name>.pythonanywhere.com
   ```

3. Imposta le variabili d'ambiente scrivendole come coppie chiave-valore nel file `.env`.
   Ad esempio, per impostare `DJANGO_DEBUG` su `False` nella console Bash, inserisci il seguente comando:

   ```bash
   echo "DJANGO_DEBUG=False" >> .env
   ```

4. Riavvia l'applicazione.

Puoi testare che l'operazione ha funzionato tentando di aprire un record che non esiste (ad esempio, crea un genere, quindi incrementa il numero nella barra dell'URL per aprire un record che non è stato ancora creato).
Se la variabile d'ambiente è stata caricata otterrai un messaggio "Non trovato" anziché un tracciato di debug dettagliato.

## Esempio: Hosting su Railway

Questa sezione fornisce una dimostrazione pratica di come installare _LocalLibrary_ su [Railway](https://railway.com/).

### Perché Railway?

> [!WARNING]
> Railway non ha più un livello iniziale completamente gratuito.
> Abbiamo comunque mantenuto queste istruzioni perché Railway ha alcune ottime funzionalità, e sarà una scelta migliore per alcuni utenti.

Railway è un'opzione di hosting interessante per diversi motivi:

- Railway si occupa della maggior parte dell'infrastruttura quindi non devi farlo tu.
  Non dover preoccuparsi di server, bilanciatori di carico, reverse proxy, ecc., rende molto più facile iniziare.
- Railway ha un [focus sull'esperienza del sviluppatore per lo sviluppo e il deployment](https://docs.railway.com/maturity/compare-to-heroku), che conduce a una curva di apprendimento più veloce e morbida rispetto a molte altre alternative.
- Le competenze e i concetti che imparerai utilizzando Railway sono trasferibili.
  Anche se Railway ha alcune eccellenti funzionalità nuove, molti altri servizi di hosting popolari usano molte delle stesse idee e approcci.
- [Documentazione Railway](https://docs.railway.com/) è chiara e completa.
- Il servizio sembra essere molto affidabile, e se finisci per amarlo, il prezzo è prevedibile, e scalare la tua app è molto facile.

Dovresti prenderti il tempo per determinare se Railway è [adatto per il tuo sito web](#scegliere_un_fornitore_di_hosting).

### Come funziona Railway?

Le applicazioni web sono eseguite ciascuna nel proprio container virtualizzato isolato e indipendente.
Per eseguire la tua applicazione, Railway deve essere in grado di configurare l'ambiente e le dipendenze appropriate, ed essere anche in grado di comprendere come viene avviata.
Per le app Django forniamo queste informazioni in diversi file di testo:

- **runtime.txt**: indica il linguaggio di programmazione e la versione da usare.
- **requirements.txt**: elenca le dipendenze Python necessarie per il tuo sito, incluso Django.
- **Procfile**: Una lista di processi da eseguire per avviare l'applicazione web.
  Per Django questo sarà solitamente il server di applicazioni web Gunicorn (con uno script `.wsgi`).
- **wsgi.py**: configurazione [WSGI](https://wsgi.readthedocs.io/en/latest/what.html) per chiamare la nostra applicazione Django nell'ambiente Railway.

Una volta che l'applicazione è in esecuzione, può configurarsi utilizzando le informazioni fornite nelle [variabili d'ambiente](https://docs.railway.com/guides/variables).
Ad esempio, un'applicazione che usa un database può ottenere l'indirizzo usando la variabile `DATABASE_URL`.
Il servizio del database stesso potrebbe essere ospitato da Railway o qualche altro fornitore.

Gli sviluppatori interagiscono con Railway tramite il sito Railway, e utilizzando un [Command Line Interface (CLI)](https://docs.railway.com/guides/cli) speciale.
La CLI ti permette di associare un repository GitHub locale a un progetto railway, caricare il repository dal branch locale al sito live, ispezionare i log del processo in esecuzione, impostare e ottenere variabili di configurazione e molto altro.
Una delle funzionalità più utili è che puoi usare la CLI per eseguire il tuo progetto locale con le stesse variabili d'ambiente del progetto live.

Per far funzionare la nostra applicazione su Railway, dovremo mettere la nostra applicazione web Django in un repository git, aggiungere i file di cui sopra, integrare un database add-on e apportare modifiche per gestire correttamente i file statici.
Una volta che avremo fatto tutto questo, potremo configurare un account Railway, ottenere il client Railway e installare il nostro sito web.

Questa è tutta la panoramica di cui hai bisogno per iniziare.

### Aggiorna l'app per Railway

Questa sezione spiega le modifiche che dovrai apportare alla nostra applicazione _LocalLibrary_ per farla funzionare su Railway.
Dobbiamo davvero solo creare un file `Procfile` e `runtime.txt`, poiché quasi tutto il resto è già presente.

Nota che queste modifiche non impediranno di utilizzare i test locali e i flussi di lavoro che abbiamo già imparato.

#### Procfile

Un _Procfile_ è il "punto di ingresso" dell'applicazione web.
Elenca i comandi che verranno eseguiti da Railway per avviare il tuo sito.

Crea il file `Procfile` (senza estensione) nella radice del tuo repo GitHub e copia/incolla il seguente testo:

```plain
web: python manage.py migrate && python manage.py collectstatic --no-input && gunicorn locallibrary.wsgi
```

Il prefisso `web:` dice a Railway che questo è un processo web e può ricevere traffico HTTP.
Chiamiamo poi il comando di migrazione di Django `python manage.py migrate` per configurare le tabelle del database.
Successivamente, chiamiamo il comando Django `python manage.py collectstatic` per raccogliere file statici nella cartella definita dall'impostazione del progetto `STATIC_ROOT` (vedi la sezione [servire file statici in produzione](#servire_i_file_statici_in_produzione) qui sotto).
Infine, avviamo il processo _gunicorn_, un popolare server di applicazioni web, passando le informazioni di configurazione nel modulo `locallibrary.wsgi` (creato con il nostro progetto scheletratico: **/locallibrary/wsgi.py**).

Nota che abbiamo già configurato il progetto per includere _gunicorn_ e supportare il servizio di file statici!

Puoi anche usare il Procfile per avviare processi di lavoro o per eseguire altre operazioni non interattive prima che il rilascio venga distribuito.

#### Runtime

Il file **runtime.txt**, se definito, dice a Railway quale versione di Python usare.
Crea il file nella radice del repo e aggiungi il seguente testo:

```plain
python-3.10.2
```

> [!NOTE]
> I fornitori di hosting non supportano necessariamente ogni versione minore del runtime Python.
> Userebbero generalmente la versione più vicina supportata al valore che specifichi.

#### Riprova e salva le modifiche su GitHub

Prima di procedere, testa nuovamente il sito localmente e assicurati che non sia stato rotto da qualcuno dei cambiamenti sopra.
Esegui normalmente il server web di sviluppo e verifica che il sito funzioni ancora come ti aspetti nel tuo browser.

```bash
python3 manage.py runserver
```

Successivamente, facciamo `push` delle modifiche su GitHub.
Nel terminale (dopo esserti spostato nel nostro repository locale), inserisci i seguenti comandi:

```python
git checkout -b railway_changes
git add -A
git commit -m "Added files and changes required for deployment"
git push origin railway_changes
```

Quindi crea e unisci la PR su GitHub.

Dovremmo ora essere pronti per iniziare a distribuire LocalLibrary su Railway.

### Ottieni un account Railway

Per iniziare a usare Railway dovrai prima creare un account:

- Vai su [railway.com](https://railway.com/) e clicca sul link **Login** nella barra degli strumenti superiore del sito.
- Seleziona GitHub nel popup per accedere utilizzando le tue credenziali GitHub.
- Potrebbe essere necessario andare nella tua email e verificare il tuo account.
- Sarai quindi loggato nella dashboard Railway.com: <https://railway.com/dashboard>.

### Distribuire su Railway da GitHub

Successivamente configureremo Railway per distribuire la nostra libreria da GitHub.
Per prima cosa scegli l'opzione **Dashboard** dal menu superiore del sito, quindi seleziona il pulsante **Nuovo progetto**:

![Dashboard sito Railway con nuovo pulsante progetto](railway_new_project_button.png)

Railway visualizzerà un elenco di opzioni per il nuovo progetto, tra cui l'opzione di distribuire un progetto da un modello che viene prima creato nel tuo account GitHub e un numero di database.
Seleziona **Distribuisci da repo GitHub**.

![Schermata sito Railway - distribuisci](railway_new_project_button_deploy_github_repo.png)

Tutti i progetti nei repository GitHub che hai condiviso con Railway durante la configurazione sono visualizzati.
Seleziona il tuo repository GitHub per la biblioteca locale: `<user-name>/django-locallibrary-tutorial`.

![Schermata del sito Railway che mostra una finestra di dialogo per scegliere un repository GitHub esistente o sceglierne uno nuovo](railway_new_project_button_deploy_github_selectrepo.png)

Conferma la tua distribuzione selezionando **Distribuisci ora**.

![Schermata di conferma - seleziona distribuisci](railway_new_project_deploy_confirm.png)

Railway quindi caricherà e distribuirà il tuo progetto, visualizzando i progressi nella scheda distribuzioni.
Quando la distribuzione termina con successo, vedrai una schermata come quella qui sotto.

![Schermata del sito Railway - distribuzione](railway_project_deploy.png)

Puoi cliccare sull'URL del sito (evidenziato sopra) per aprire il sito in un browser (non funzionerà ancora, poiché la configurazione non è completa).

### Imposta ALLOWED_HOSTS e CSRF_TRUSTED_ORIGINS

Quando il sito viene aperto, a questo punto vedrai una schermata di errore di debug come mostrato di seguito.
Questo è un errore di sicurezza di Django sollevato perché il nostro codice sorgente non sta eseguendo su un "host consentito".

![Una pagina di errore dettagliata con un traceback completo di un'intestazione HTTP_HOST non valida](site_error_disallowed_host.png)

> [!NOTE]
> Questo tipo di informazioni di debug è molto utile quando ti stai preparando, ma è un rischio per la sicurezza in un sito distribuito.
> Ti mostreremo come disabilitarlo una volta che il sito è in funzione.

Apri **/locallibrary/settings.py** nel tuo progetto GitHub e modifica l'impostazione [ALLOWED_HOSTS](https://docs.djangoproject.com/en/5.0/ref/settings/#allowed-hosts) per includere l'URL del tuo sito Railway:

```python
## For example, for a site URL at 'web-production-3640.up.railway.app'
## (replace the string below with your own site URL):
ALLOWED_HOSTS = ['web-production-3640.up.railway.app', '127.0.0.1']

# During development, you can instead set just the base URL
# (you might decide to change the site a few times).
# ALLOWED_HOSTS = ['.railway.com','127.0.0.1']
```

Poiché l'applicazione usa la protezione CSRF, dovrai anche impostare la chiave [CSRF_TRUSTED_ORIGINS](https://docs.djangoproject.com/en/5.0/ref/settings/#csrf-trusted-origins).
Apri **/locallibrary/settings.py** e aggiungi una riga come quella sotto:

```python
## For example, for a site URL is at 'web-production-3640.up.railway.app'
## (replace the string below with your own site URL):
CSRF_TRUSTED_ORIGINS = ['https://web-production-3640.up.railway.app']

# During development/for this tutorial you can instead set just the base URL
# CSRF_TRUSTED_ORIGINS = ['https://*.railway.app']
```

Quindi salva le tue impostazioni e aggiungile al tuo repository GitHub (Railway aggiornerà automaticamente e ridistribuirà la tua applicazione).

### Provision e connessione a un database Postgres SQL

Successivamente dobbiamo creare un database Postgres e collegarlo all'applicazione Django che abbiamo appena distribuito.
(Se apri il sito ora otterrai un nuovo errore perché il database non può essere accesso).
Creeremo il database come parte del progetto dell'applicazione, anche se puoi creare il database in un progetto separato.

Su Railway, scegli l'opzione **Dashboard** dalla barra superiore del sito e quindi seleziona il tuo progetto applicazione.
In questa fase contiene solo un singolo servizio per la tua applicazione (questo può essere selezionato per impostare variabili e altri dettagli del servizio).
Il pulsante **Impostazioni** può essere selezionato per cambiare le impostazioni di progetto.
Seleziona il pulsante **Nuovo**, usato per aggiungere servizi al progetto.

![Progetto Railway con pulsante nuovo servizio evidenziato](railway_project_open_no_database.png)

Seleziona **Database** quando viene chiesto che tipo di servizio aggiungere:

![Progetto Railway - seleziona database come nuovo servizio](railway_project_add_database.png)

Quindi seleziona **Aggiungi PostgreSQL** per iniziare ad aggiungere il database

![Progetto Railway - seleziona Postgres come nuovo servizio](railway_project_add_database_select_type.png)

Railway quindi fornirà un servizio contenente un database vuoto nello stesso progetto.
Al completamento vedrai ora entrambi i servizi di applicazione e database nella vista del progetto.

![Progetto Railway con servizio applicazione e database Postgres](railway_project_two_services.png)

Seleziona il servizio web e quindi la scheda _Variables_.
Seleziona **Nuova Variabile** e quindi nella casella _Nome della variabile_, seleziona **Aggiungi riferimento**.
Scorri verso il basso e seleziona `DATABASE_URL` (questo è il nome della variabile che abbiamo impostato perché la libreria leggesse come variabile d'ambiente).

![Schermata del sito Railway che seleziona un DATABASE_URL](railway_postgresql_connect.png)

Quindi seleziona **Aggiungi** per aggiungere il riferimento variabile e infine **Distribuisci** (questo apparirà in un popup).
Nota che potresti anche aver aperto il database Postgres, quindi la sua scheda delle variabili, e copiato la variabile.

Se apri ora il progetto dovrebbe essere visualizzato proprio come ha fatto localmente.
Nota tuttavia che non c'è modo di popolarel'indirizzo email del securizzata la biblioteca di pocchi dati ancora, perché non abbiamo ancora creato un account superuser.
Lo faremo utilizzando il [CLI](https://docs.railway.com/guides/cli) tool sul nostro computer locale.

### Installa il client

Scarica e installa il client Railway per il tuo sistema operativo locale seguendo le [istruzioni qui](https://docs.railway.com/guides/cli).

Dopo che il client è installato sarai in grado di eseguire comandi.
Alcune delle operazioni più importanti includono distribuire la directory corrente del tuo computer a un progetto Railway associato (senza dover caricare su GitHub), ed eseguire il tuo progetto Django localmente utilizzando le stesse impostazioni del server di produzione.
Mostriamo queste funzioni nelle sezioni successive.

Puoi ottenere un elenco di tutti i comandi possibili inserendo il seguente comando in una terminale.

```bash
railway help
```

> [!NOTE]
> Nella sezione successiva usiamo `railway login` e `railway link` per collegare il progetto corrente a una directory.
> Se sei scollegato dal sistema, dovrai chiamare entrambi i comandi di nuovo per ricollegare il progetto.

### Configura un superuser

Per creare un superuser, dobbiamo chiamare il comando Django `createsuperuser` contro il database di produzione (questa è la stessa operazione che abbiamo eseguito localmente in [Django Tutorial Parte 4: Sito admin Django > Creazione di un superuser](/it/docs/Learn_web_development/Extensions/Server-side/Django/Admin_site#creating_a_superuser)).
Railway non fornisce accesso diretto al terminale sul server, e non possiamo aggiungere questo comando al [Procfile](#procfile) perché è interattivo.

Quello che possiamo fare è chiamare questo comando localmente sul nostro progetto Django quando è connesso al database _di produzione_.
Il client Railway rende questo facile fornendo un meccanismo per eseguire comandi localmente usando le stesse variabili d'ambiente del server di produzione, incluso il stringa di connessione al database.

Per prima cosa apri un terminale o prompt dei comandi in una clone git del tuo progetto della biblioteca locale.
Quindi accedi al tuo account browser utilizzando il comando `login` o `login --browserless` (segui eventuali prompt risultanti e istruzioni dal client o sito per completare l'accesso):

```bash
railway login
```

Una volta effettuato l'accesso, collega la tua directory corrente della libreria al progetto Railway associato usando il seguente comando.
Nota che dovrai selezionare/inserire un progetto particolare quando richiesto:

```bash
railway link
```

Ora che la directory locale e il progetto sono _collegati_ puoi eseguire il progetto Django localmente con le impostazioni dell'ambiente di produzione.
Prima assicurati che il tuo normale [ambiente di sviluppo Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/development_environment) sia pronto.
Quindi chiama il seguente comando, inserendo nome, email, e password come richiesto:

```bash
railway run python manage.py createsuperuser
```

Dovresti ora essere in grado di aprire l'area admin del tuo sito web (`https://[your-url].railway.app/admin/`) e popolarlo con dati, proprio come mostrato in [Django Tutorial Parte 4: Sito admin Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/Admin_site)).

### Impostare variabili di configurazione

L'ultimo passaggio è rendere sicuro il sito.
Specificamente, dobbiamo disabilitare il logging di debug e impostare una chiave segreta CSRF.
Il lavoro per leggere i valori necessari dalle variabili d'ambiente è stato fatto nel [preparare il tuo sito web alla pubblicazione](#preparare_il_tuo_sito_web_per_la_pubblicazione) (vedi `DJANGO_DEBUG` e `DJANGO_SECRET_KEY`).

Apri la schermata di informazioni per il progetto e seleziona la scheda _Variables_.
Questo dovrebbe già avere il `DATABASE_URL` come mostrato di seguito.

![Railway - aggiungi una nuova schermata variabile](railway_variable_new.png)

Ci sono molti modi per generare una chiave segreta crittografica.
Un modo semplice è eseguire il seguente comando Python sul tuo computer di sviluppo:

```bash
python -c "import secrets; print(secrets.token_urlsafe())"
```

Seleziona il pulsante **Nuova Variabile** e inserisci la chiave `DJANGO_SECRET_KEY` con il tuo valore segreto (quindi seleziona **Aggiungi**).
Quindi inserisci la chiave `DJANGO_DEBUG` con il valore `False`.
La serie finale di variabili dovrebbe apparire così:

![Schermata Railway che mostra tutte le variabili di progetto](railway_variables_all.png)

### Debugging

Il client Railway fornisce il comando dei log per mostrare la coda dei log (un log più completo è disponibile sul sito per ogni progetto):

```bash
railway logs
```

Se hai bisogno di più informazioni di quanto questo possa fornire, dovrai iniziare a esaminare il [logging di Django](https://docs.djangoproject.com/en/5.0/topics/logging/).

## Riepilogo

Questo è la fine di questo tutorial sulla configurazione delle app Django in produzione, e anche della serie di tutorial sulla lavorazione con Django. Speriamo che tu l'abbia trovato utile. Puoi controllare una versione completamente funzionante del [codice sorgente su GitHub qui](https://github.com/mdn/django-locallibrary-tutorial).

Il prossimo passo è leggere i nostri ultimi articoli e poi completare il compito di valutazione.

## Vedi anche

- [Deployment di Django](https://docs.djangoproject.com/en/5.0/howto/deployment/) (documenti Django)

  - [Lista di controllo per il deployment](https://docs.djangoproject.com/en/5.0/howto/deployment/checklist/) (documenti Django)
  - [Distribuzione di file statici](https://docs.djangoproject.com/en/5.0/howto/static-files/deployment/) (documenti Django)
  - [Come distribuire con WSGI](https://docs.djangoproject.com/en/5.0/howto/deployment/wsgi/) (documenti Django)
  - [Come usare Django con Apache e mod_wsgi](https://docs.djangoproject.com/en/5.0/howto/deployment/wsgi/modwsgi/) (documenti Django)
  - [Come usare Django con Gunicorn](https://docs.djangoproject.com/en/5.0/howto/deployment/wsgi/gunicorn/) (documenti Django)

- Documenti Railway

  - [CLI](https://docs.railway.com/guides/cli)

- DigitalOcean

  - [Come servire applicazioni Django con uWSGI e Nginx su Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-serve-django-applications-with-uwsgi-and-nginx-on-ubuntu-16-04)
  - [Altri documenti della comunità Django di DigitalOcean](https://www.digitalocean.com/community/tutorials?q=django)

- Documenti Heroku (concetti di configurazione simili)

  - [Configurazione di app Django su Heroku](https://devcenter.heroku.com/articles/django-app-configuration) (documenti Heroku)
  - [Inizia su Heroku con Django](https://devcenter.heroku.com/articles/getting-started-with-python#introduction) (documenti Heroku)
  - [Django e risorse statiche](https://devcenter.heroku.com/articles/django-assets) (documenti Heroku)
  - [Concorrenza e connessioni al database in Django](https://devcenter.heroku.com/articles/python-concurrency-and-database-connections) (documenti Heroku)
  - [Come funziona Heroku](https://devcenter.heroku.com/articles/how-heroku-works) (documenti Heroku)
  - [Dynos e il Dyno Manager](https://devcenter.heroku.com/articles/dynos) (documenti Heroku)
  - [Configurazione e variabili di configurazione](https://devcenter.heroku.com/articles/config-vars) (documenti Heroku)
  - [Limiti](https://devcenter.heroku.com/articles/limits) (documenti Heroku)
  - [Distribuzione di applicazioni Python con Gunicorn](https://devcenter.heroku.com/articles/python-gunicorn) (documenti Heroku)
  - [Lavorare con Django](https://devcenter.heroku.com/categories/working-with-django) (documenti Heroku)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Testing", "Learn_web_development/Extensions/Server-side/Django/web_application_security", "Learn_web_development/Extensions/Server-side/Django")}}
