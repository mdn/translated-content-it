---
title: "Tutorial di Django Parte 2: Creare lo scheletro di un sito web"
short-title: "2: Scheletro del sito web"
slug: Learn_web_development/Extensions/Server-side/Django/skeleton_website
l10n:
  sourceCommit: 324c613947adaa5e19ad0f409c5f4c535ee8cf6b
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website", "Learn_web_development/Extensions/Server-side/Django/Models", "Learn_web_development/Extensions/Server-side/Django")}}

Questo secondo articolo del [Tutorial di Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website) mostra come creare un progetto di sito web "scheletro" come base, che potrà poi essere popolata con impostazioni, percorsi, modelli, viste e template specifici del sito.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/development_environment">Configurare un ambiente di sviluppo Django</a>.
        Consultare il <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website">Tutorial di Django</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Essere in grado di utilizzare gli strumenti di Django per avviare nuovi progetti di siti web.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

Questo articolo mostra come creare un sito web "scheletro", che potrà poi essere popolato con impostazioni, percorsi, modelli, viste e template specifici del sito (che verranno trattati negli articoli successivi).

Per iniziare:

1. Utilizzare lo strumento `django-admin` per generare una cartella del progetto, i template di file di base e **manage.py**, che funge da script di gestione del progetto.
2. Utilizzare **manage.py** per creare una o più _applicazioni_.

   > [!NOTE]
   > Un sito web può essere costituito da una o più sezioni. Ad esempio, sito principale, blog, wiki, area download e così via. Django incoraggia lo sviluppo di questi componenti come _applicazioni_ separate, che possono poi essere riutilizzate in progetti diversi, se necessario.

3. Registrare le nuove applicazioni per includerle nel progetto.
4. Collegare il mapper **url/path** per ogni applicazione.

Per il [sito web Local Library](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website), le cartelle del sito web e del progetto sono denominate _locallibrary_ e includono un'applicazione denominata _catalog_.
La struttura della cartella di livello superiore sarà quindi la seguente:

```bash
locallibrary/         # Website folder
    manage.py         # Script to run Django tools for this project (created using django-admin)
    locallibrary/     # Website/project folder (created using django-admin)
    catalog/          # Application folder (created using manage.py)
```

Le sezioni seguenti analizzano in dettaglio i passaggi del processo e mostrano come testare le modifiche.
Alla fine di questo articolo vengono trattate anche altre configurazioni valide per l'intero sito che possono essere effettuate in questa fase.

## Creare il progetto

Per creare il progetto:

1. Aprire una shell dei comandi (o una finestra del terminale) e assicurarsi di trovarsi nel proprio [ambiente virtuale](/it/docs/Learn_web_development/Extensions/Server-side/Django/development_environment#using_a_virtual_environment).
2. Passare alla cartella in cui si desidera creare l'applicazione della libreria locale (in seguito verrà spostata in "django_local_library", che è stato [creato come repository GitHub locale](/it/docs/Learn_web_development/Extensions/Server-side/Django/development_environment#clone_the_repo_to_your_local_computer) durante la configurazione dell'ambiente di sviluppo).
3. Creare il nuovo progetto utilizzando il comando `django-admin startproject`, come mostrato, quindi passare alla cartella del progetto:

   ```bash
   django-admin startproject locallibrary
   cd locallibrary
   ```

   Lo strumento `django-admin` crea una struttura di cartelle/file come la seguente:

   ```bash
   locallibrary/
       manage.py
       locallibrary/
           __init__.py
           settings.py
           urls.py
           wsgi.py
           asgi.py
   ```

La sottocartella del progetto _locallibrary_ è il punto di ingresso per il sito web:

- **\_\_init\_\_.py** è un file vuoto che indica a Python di trattare questa directory come un pacchetto Python.
- **settings.py** contiene tutte le impostazioni del sito web, inclusa la registrazione delle applicazioni create, la posizione dei file statici, i dettagli di configurazione del database e così via.
- **urls.py** definisce le mappature tra URL e viste del sito. Sebbene potrebbe contenere _tutto_ il codice di mappatura degli URL, è più comune delegare alcune delle mappature a particolari applicazioni, come verrà mostrato in seguito.
- **wsgi.py** viene utilizzato per aiutare l'applicazione Django a comunicare con il server web. Può essere considerato boilerplate.
- **asgi.py** è uno standard che consente alle app web e ai server asincroni Python di comunicare tra loro. Asynchronous Server Gateway Interface (ASGI) è il successore asincrono di Web Server Gateway Interface (WSGI). ASGI fornisce uno standard sia per le app Python asincrone sia per quelle sincrone, mentre WSGI forniva uno standard solo per le app sincrone. ASGI è retrocompatibile con WSGI e supporta più server e framework applicativi.

Lo script **manage.py** viene utilizzato per creare applicazioni, lavorare con i database e avviare il server web di sviluppo.

## Creare l'applicazione catalog

Successivamente, eseguire il comando seguente per creare l'applicazione _catalog_ che risiederà all'interno del progetto _locallibrary_. Assicurarsi di eseguire questo comando dalla stessa cartella in cui si trova **manage.py** del progetto:

```bash
# Linux/macOS
python3 manage.py startapp catalog

# Windows
py manage.py startapp catalog
```

> [!NOTE]
> Il resto del tutorial utilizza la sintassi Linux/macOS.
> Se si lavora su Windows, ovunque sia presente un comando che inizia con `python3` si deve usare invece `py` (oppure `py -3`).

Lo strumento crea una nuova cartella e la popola con file per le diverse parti dell'applicazione, come mostrato nell'esempio seguente.
La maggior parte dei file prende il nome dal proprio scopo (ad esempio, le viste devono essere memorizzate in **views.py**, i modelli in **models.py**, i test in **tests.py**, la configurazione del sito di amministrazione in **admin.py**, la registrazione dell'applicazione in **apps.py**) e contiene codice boilerplate minimo per lavorare con gli oggetti associati.

La directory del progetto aggiornata dovrebbe ora apparire così:

```bash
locallibrary/
    manage.py
    locallibrary/
    catalog/
        admin.py
        apps.py
        models.py
        tests.py
        views.py
        __init__.py
        migrations/
```

Inoltre, ora sono presenti:

- Una cartella _migrations_, utilizzata per memorizzare le "migrazioni", ovvero file che consentono di aggiornare automaticamente il database man mano che si modificano i modelli.
- **\_\_init\_\_.py** — un file vuoto creato qui affinché Django/Python riconosca la cartella come un [Python Package](https://docs.python.org/3/tutorial/modules.html#packages) e consenta di utilizzare i relativi oggetti in altre parti del progetto.

> [!NOTE]
> È stato notato cosa manca dall'elenco dei file precedente? Sebbene esista uno spazio per viste e modelli, non c'è alcun posto in cui inserire le mappature URL, i template e i file statici. Verrà mostrato in seguito come crearli (non sono necessari in tutti i siti web, ma sono necessari in questo esempio).

## Registrare l'applicazione catalog

Ora che l'applicazione è stata creata, occorre registrarla nel progetto affinché venga inclusa quando vengono eseguiti strumenti di qualsiasi tipo, come ad esempio l'aggiunta di modelli al database. Le applicazioni vengono registrate aggiungendole all'elenco `INSTALLED_APPS` nelle impostazioni del progetto.

Aprire il file delle impostazioni del progetto, **django-locallibrary-tutorial/locallibrary/settings.py**, e individuare la definizione dell'elenco `INSTALLED_APPS`. Aggiungere quindi una nuova riga alla fine dell'elenco, come mostrato di seguito:

```bash
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # Add our new application
    'catalog.apps.CatalogConfig', # This object was created for us in /catalog/apps.py
]
```

La nuova riga specifica l'oggetto di configurazione dell'applicazione (`CatalogConfig`) generato in **/django-locallibrary-tutorial/catalog/apps.py** durante la creazione dell'applicazione.

> [!NOTE]
> Si noterà che sono già presenti molte altre `INSTALLED_APPS` (e `MIDDLEWARE`, più avanti nel file delle impostazioni). Queste abilitano il supporto per il [sito di amministrazione Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/Admin_site) e per le funzionalità che utilizza, comprese sessioni, autenticazione e così via.

## Specificare il database

Questo è anche il punto in cui normalmente si specifica il database da utilizzare per il progetto. Quando possibile, ha senso utilizzare lo stesso database per lo sviluppo e la produzione, in modo da evitare piccole differenze di comportamento. È possibile scoprire le diverse opzioni in [Databases](https://docs.djangoproject.com/en/5.0/ref/settings/#databases) (documentazione di Django).

Per la maggior parte di questo esempio verrà utilizzato il database SQLite predefinito, poiché non è previsto molto accesso simultaneo a un database dimostrativo e non richiede alcun lavoro aggiuntivo per la configurazione. È possibile vedere come viene configurato questo database in **settings.py**:

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```

Più avanti, nella sezione [Distribuire Django in produzione](/it/docs/Learn_web_development/Extensions/Server-side/Django/Deployment#database_configuration), verrà anche mostrato come configurare un database Postgres, che potrebbe essere più adatto per siti più grandi.

## Altre impostazioni del progetto

Il file **settings.py** viene utilizzato anche per configurare varie altre impostazioni, ma a questo punto probabilmente è necessario modificare soltanto [TIME_ZONE](https://docs.djangoproject.com/en/5.0/ref/settings/#std:setting-TIME_ZONE), che deve essere impostato su una stringa dell'elenco standard [List of tz database time zones](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) (la colonna TZ nella tabella contiene i valori desiderati). Modificare il valore di `TIME_ZONE` usando una di queste stringhe adatte al proprio fuso orario, ad esempio:

```python
TIME_ZONE = 'Europe/London'
```

Ci sono altre due impostazioni che non verranno modificate ora, ma di cui è opportuno essere a conoscenza:

- `SECRET_KEY`. Si tratta di una chiave segreta utilizzata come parte della strategia di sicurezza dei siti web di Django. Se questo codice non viene protetto durante lo sviluppo, sarà necessario utilizzare un codice diverso, magari letto da una variabile di ambiente o da un file, quando verrà distribuito in produzione.
- `DEBUG`. Consente di visualizzare i log di debug in caso di errore, anziché le risposte con codice di stato HTTP. In produzione deve essere impostato su `False`, poiché le informazioni di debug sono utili agli aggressori; per ora può rimanere impostato su `True`.

## Collegare il mapper URL

Il sito web viene creato con un file mapper URL (**urls.py**) nella cartella del progetto. Sebbene sia possibile utilizzare questo file per gestire tutte le mappature URL, è più comune delegare le mappature all'applicazione associata.

Aprire **django-locallibrary-tutorial/locallibrary/urls.py** e osservare il testo esplicativo che descrive alcuni dei modi per utilizzare il mapper URL.

```python
"""
URL configuration for locallibrary project.

The `urlpatterns` list routes URLs to views. For more information please see:
    https://docs.djangoproject.com/en/5.0/topics/http/urls/
Examples:
Function views
    1. Add an import:  from my_app import views
    2. Add a URL to urlpatterns:  path('', views.home, name='home')
Class-based views
    1. Add an import:  from other_app.views import Home
    2. Add a URL to urlpatterns:  path('', Home.as_view(), name='home')
Including another URLConf
    1. Import the include() function: from django.urls import include, path
    2. Add a URL to urlpatterns:  path('blog/', include('blog.urls'))
"""
from django.contrib import admin
from django.urls import path

urlpatterns = [
    path('admin/', admin.site.urls),
]
```

Le mappature URL vengono gestite tramite la variabile `urlpatterns`, che è un _elenco_ Python di funzioni `path()`. Ogni funzione `path()` associa un pattern URL a una _vista specifica_, che verrà visualizzata quando il pattern corrisponde, oppure a un altro elenco di codice per il test dei pattern URL (in questo secondo caso, il pattern diventa l'"URL di base" per i pattern definiti nel modulo di destinazione). L'elenco `urlpatterns` definisce inizialmente una singola funzione che mappa tutti gli URL con il pattern _admin/_ al modulo `admin.site.urls`, che contiene le definizioni delle mappature URL dell'applicazione Administration.

> [!NOTE]
> La route in `path()` è una stringa che definisce un pattern URL da confrontare. Questa stringa può includere una variabile denominata, tra parentesi angolari, ad esempio `'catalog/<id>/'`. Questo pattern corrisponderà a un URL come **catalog/_any_chars_/** e passerà _`any_chars`_ alla vista come stringa con il nome di parametro `id`. I metodi per i percorsi e i pattern delle route verranno discussi più approfonditamente negli argomenti successivi.

Per aggiungere un nuovo elemento all'elenco `urlpatterns`, aggiungere le righe seguenti in fondo al file. Questo nuovo elemento include un `path()` che inoltra le richieste con il pattern `catalog/` al modulo `catalog.urls` (il file con URL relativo **catalog/urls.py**).

```python
# Use include() to add paths from the catalog application
from django.urls import include

urlpatterns += [
    path('catalog/', include('catalog.urls')),
]
```

> [!NOTE]
> Si noti che la riga di importazione (`from django.urls import include`) è stata inclusa insieme al codice che la utilizza, in modo che sia facile vedere cosa è stato aggiunto, ma è comune includere tutte le righe di importazione all'inizio di un file Python.

Ora reindirizziamo l'URL radice del sito, ovvero `127.0.0.1:8000`, all'URL `127.0.0.1:8000/catalog/`. Questa è l'unica app che verrà utilizzata in questo progetto. Per farlo, verrà usata una funzione di vista speciale, `RedirectView`, che accetta come primo argomento il nuovo URL relativo verso cui reindirizzare (`/catalog/`) quando corrisponde il pattern URL specificato nella funzione `path()` — in questo caso, l'URL radice.

Aggiungere le righe seguenti in fondo al file:

```python
# Add URL maps to redirect the base URL to our application
from django.views.generic import RedirectView
urlpatterns += [
    path('', RedirectView.as_view(url='catalog/', permanent=True)),
]
```

Lasciare vuoto il primo parametro della funzione path per indicare '/'. Se si scrive il primo parametro come '/' Django mostrerà il seguente avviso all'avvio del server di sviluppo:

```python
System check identified some issues:

WARNINGS:
?: (urls.W002) Your URL pattern '/' has a route beginning with a '/'.
Remove this slash as it is unnecessary.
If this pattern is targeted in an include(), ensure the include() pattern has a trailing '/'.
```

Per impostazione predefinita, Django non serve file statici come CSS, JavaScript e immagini, ma può essere utile che il server web di sviluppo lo faccia durante la creazione del sito. Come ultima aggiunta a questo mapper URL, è possibile abilitare il serving dei file statici durante lo sviluppo aggiungendo le righe seguenti.

Aggiungere ora questo blocco finale in fondo al file:

```python
# Use static() to add URL mapping to serve static files during development (only)
from django.conf import settings
from django.conf.urls.static import static

urlpatterns += static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
```

> [!NOTE]
> Esistono diversi modi per estendere l'elenco `urlpatterns` (in precedenza è stato semplicemente aggiunto un nuovo elemento dell'elenco utilizzando l'operatore `+=` per separare chiaramente il codice vecchio da quello nuovo). Si sarebbe potuto invece includere questo nuovo mapping di pattern nella definizione originale dell'elenco:
>
> ```python
> urlpatterns = [
>     path('admin/', admin.site.urls),
>     path('catalog/', include('catalog.urls')),
>     path('', RedirectView.as_view(url='catalog/')),
> ] + static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
> ```

Come passaggio finale, creare un file all'interno della cartella _catalog_ denominato **urls.py** e aggiungere il testo seguente per definire l'elemento `urlpatterns` importato, vuoto. Qui verranno aggiunti i pattern durante lo sviluppo dell'applicazione.

```python
from django.urls import path
from . import views

urlpatterns = [

]
```

## Testare il framework del sito web

A questo punto è disponibile un progetto scheletro completo. Il sito web non _fa_ ancora nulla, ma vale la pena eseguirlo per assicurarsi che nessuna delle modifiche abbia causato problemi.

Prima di farlo, occorre eseguire una _migrazione del database_. Questo aggiorna il database, includendo eventuali modelli nelle applicazioni installate, e rimuove alcuni avvisi di build.

### Eseguire le migrazioni del database

Django utilizza un Object-Relational-Mapper (ORM) per mappare le definizioni dei modelli nel codice Django alla struttura dati utilizzata dal database sottostante. Quando vengono modificate le definizioni dei modelli, Django tiene traccia delle modifiche e può creare script di migrazione del database, in **/django-locallibrary-tutorial/catalog/migrations/**, per migrare automaticamente la struttura dati sottostante nel database affinché corrisponda al modello.

Durante la creazione del sito web, Django ha aggiunto automaticamente diversi modelli da utilizzare nella sezione di amministrazione del sito, che verrà esaminata in seguito. Eseguire i comandi seguenti per definire le tabelle per questi modelli nel database, assicurandosi di trovarsi nella directory che contiene **manage.py**:

```bash
python3 manage.py makemigrations
python3 manage.py migrate
```

> [!WARNING]
> Sarà necessario eseguire questi comandi ogni volta che i modelli cambiano in un modo che influirà sulla struttura dei dati da memorizzare, inclusa sia l'aggiunta sia la rimozione di interi modelli e di singoli campi.

Il comando `makemigrations` _crea_, ma non applica, le migrazioni per tutte le applicazioni installate nel progetto. È possibile specificare anche il nome dell'applicazione per eseguire una migrazione solo per una singola app. Questo consente di controllare il codice di queste migrazioni prima che vengano applicate. Gli esperti di Django potrebbero scegliere di modificarle leggermente.

Il comando `migrate` applica le migrazioni al database. Django tiene traccia di quali migrazioni sono state aggiunte al database corrente.

> [!NOTE]
> È opportuno rieseguire le migrazioni e ritestare il sito ogni volta che vengono apportate modifiche significative. Non richiede molto tempo.
>
> Consultare [Migrations](https://docs.djangoproject.com/en/5.0/topics/migrations/) (documentazione di Django) per ulteriori informazioni sui comandi di migrazione meno utilizzati.

### Eseguire il sito web

Durante lo sviluppo, è possibile servire il sito web utilizzando prima il _server web di sviluppo_, quindi visualizzandolo nel browser web locale.

> [!NOTE]
> Il server web di sviluppo non è sufficientemente robusto o performante per l'uso in produzione, ma rappresenta un modo molto semplice per avviare e rendere operativo un sito web Django durante lo sviluppo, così da eseguire comodamente un rapido test. Per impostazione predefinita servirà il sito al computer locale (`http://127.0.0.1:8000/)`, ma è possibile specificare anche altri computer della rete a cui servirlo. Per maggiori informazioni, consultare [django-admin and manage.py: runserver](https://docs.djangoproject.com/en/5.0/ref/django-admin/#runserver) (documentazione di Django).

Eseguire il _server web di sviluppo_ chiamando il comando `runserver`, nella stessa directory di **manage.py**:

```bash
python3 manage.py runserver
```

Una volta avviato il server, è possibile visualizzare il sito passando a `http://127.0.0.1:8000/` nel browser web locale. Dovrebbe essere visualizzata una pagina di errore del sito simile a questa:

![Pagina di debug di Django (Django 4.2)](django_404_debug_page.png)

Nessun problema. Questa pagina di errore è prevista perché non sono ancora state definite pagine/URL nel modulo `catalog.urls`, al quale si viene reindirizzati quando si riceve un URL per la radice del sito.

A questo punto, è noto che Django funziona.

> [!NOTE]
> La pagina di esempio dimostra un'ottima funzionalità di Django: il logging di debug automatizzato. Ogni volta che non è possibile trovare una pagina, Django visualizza una schermata di errore con informazioni utili su qualsiasi errore generato dal codice. In questo caso, è possibile vedere che l'URL fornito non corrisponde a nessuno dei pattern URL elencati. Il logging viene disattivato in produzione, ovvero quando il sito viene pubblicato sul Web; in tal caso viene servita una pagina meno informativa, ma più adatta agli utenti.

## Non dimenticare di effettuare il backup su GitHub

È stato appena svolto del lavoro significativo, quindi questo è un buon momento per effettuare il backup del progetto utilizzando GitHub.

Spostare innanzitutto il _contenuto_ della cartella **locallibrary** di livello superiore nella cartella **django_local_library**, creata come [repository GitHub locale](/it/docs/Learn_web_development/Extensions/Server-side/Django/development_environment#clone_the_repo_to_your_local_computer) durante la configurazione dell'ambiente di sviluppo.
Ciò includerà **manage.py**, la sottocartella **locallibrary**, la sottocartella **catalog** e qualsiasi altro elemento presente nella cartella di livello superiore.

Quindi aggiungere ed eseguire il commit delle modifiche nella cartella **django_local_library**, quindi effettuare il push su GitHub.
Dalla radice di quella cartella, è possibile utilizzare una serie di comandi simile a quelli nella sezione [Modificare e sincronizzare le modifiche](/it/docs/Learn_web_development/Extensions/Server-side/Django/development_environment#modify_and_sync_changes) dell'argomento _Ambiente di sviluppo_:

```bash
# Get the current source from GitHub on the main branch
git checkout main
git pull origin main

# Create a branch and add/commit your newly created app skeleton
git checkout -b skeleton_website # Create and activate a new branch "skeleton_website"
git add -A # Add all changed files to the staging area
git commit -m "Create Skeleton framework for LocalLibrary" # Commit the changed files

# Push the branch to GitHub
git push origin skeleton_website
```

Quindi creare e unire una PR dal repository GitHub.
Dopo l'unione, è possibile tornare al branch `main` e recuperare le modifiche da GitHub:

```bash
git checkout main
git pull origin main
```

> [!NOTE]
> Se non viene eliminato il branch `skeleton_website`, sarà sempre possibile tornarvi in un secondo momento.

Questo potrebbe non essere necessariamente menzionato di nuovo in futuro, ma può essere utile aggiornare GitHub con le modifiche alla fine di ogni sezione di questo tutorial.

## Mettiti alla prova

La directory **catalog/** contiene file per le viste, i modelli e altre parti dell'applicazione. Aprire questi file e analizzare il boilerplate.

Come visto in precedenza, una mappatura URL per il sito Admin è già stata aggiunta nel file **urls.py** del progetto. Passare all'area di amministrazione nel browser e osservare cosa accade; è possibile dedurre l'URL corretto dalla mappatura.

## Riepilogo

È stato creato un progetto di sito web scheletro completo, che potrà poi essere popolato con URL, modelli, viste e template.

Ora che lo scheletro del [sito web Local Library](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website) è completo e in esecuzione, è il momento di iniziare a scrivere il codice che permette a questo sito web di fare ciò che dovrebbe fare.

## Vedere anche

- [Scrivere la prima app Django - parte 1](https://docs.djangoproject.com/en/5.0/intro/tutorial01/) (documentazione di Django)
- [Applications](https://docs.djangoproject.com/en/5.0/ref/applications/#configuring-applications) (documentazione di Django).
  Contiene informazioni sulla configurazione delle applicazioni.

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website", "Learn_web_development/Extensions/Server-side/Django/Models", "Learn_web_development/Extensions/Server-side/Django")}}
