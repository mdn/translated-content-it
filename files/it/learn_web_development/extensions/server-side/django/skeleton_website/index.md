---
title: "Tutorial di Django Parte 2: Creare un sito web scheletro"
short-title: "2: Sito web scheletro"
slug: Learn_web_development/Extensions/Server-side/Django/skeleton_website
l10n:
  sourceCommit: be1922d62a0d31e4e3441db0e943aed8df736481
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website", "Learn_web_development/Extensions/Server-side/Django/Models", "Learn_web_development/Extensions/Server-side/Django")}}

Questo secondo articolo del nostro [Tutorial Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website) mostra come creare un progetto di sito web "scheletro" come base, che può essere poi popolato con impostazioni specifiche del sito, percorsi, modelli, viste e template.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/development_environment">Configurare un ambiente di sviluppo Django</a>.
        Rivedere il <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website">Tutorial Django</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Essere in grado di utilizzare gli strumenti di Django per avviare i propri nuovi progetti di siti web.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

Questo articolo mostra come creare un sito web "scheletro", che può poi essere popolato con impostazioni specifiche del sito, percorsi, modelli, viste e template (discuteremo di questi negli articoli successivi).

Per iniziare:

1. Utilizzare lo strumento `django-admin` per generare una cartella di progetto, i template di file di base e **manage.py**, che funge da script di gestione del progetto.
2. Utilizzare **manage.py** per creare una o più _applicazioni_.

   > [!NOTE]
   > Un sito web può consistere di una o più sezioni. Ad esempio, sito principale, blog, wiki, area download, ecc. Django incoraggia a sviluppare questi componenti come _applicazioni_ separate, che potrebbero poi essere riutilizzate in progetti diversi, se lo si desidera.

3. Registrare le nuove applicazioni per includerle nel progetto.
4. Collegare il mapper **url/path** per ciascuna applicazione.

Per il [sito web della Biblioteca Locale](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website), le cartelle del sito web e del progetto sono chiamate _locallibrary_ e includono un'applicazione denominata _catalog_.
La struttura della cartella di livello superiore sarà quindi la seguente:

```bash
locallibrary/         # Website folder
    manage.py         # Script to run Django tools for this project (created using django-admin)
    locallibrary/     # Website/project folder (created using django-admin)
    catalog/          # Application folder (created using manage.py)
```

Le sezioni seguenti discutono i passaggi del processo in dettaglio e mostrano come è possibile testare le modifiche.
Alla fine di questo articolo, discutiamo di altre configurazioni a livello di sito che potresti anche fare in questa fase.

## Creare il progetto

Per creare il progetto:

1. Aprire una shell dei comandi (o una finestra di terminale) e assicurarsi di essere nel proprio [ambiente virtuale](/it/docs/Learn_web_development/Extensions/Server-side/Django/development_environment#using_a_virtual_environment).
2. Navigare nella cartella in cui si desidera creare l'applicazione della biblioteca locale (successivamente verrà spostata nella cartella "django_local_library" che è stata [creata come repository locale GitHub](/it/docs/Learn_web_development/Extensions/Server-side/Django/development_environment#clone_the_repo_to_your_local_computer) quando si è impostato l'ambiente di sviluppo).
3. Creare il nuovo progetto usando il comando `django-admin startproject` come mostrato, quindi navigare nella cartella del progetto:

   ```bash
   django-admin startproject locallibrary
   cd locallibrary
   ```

   Lo strumento `django-admin` crea una struttura di cartella/file come segue:

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

La sottocartella di progetto _locallibrary_ è il punto di ingresso per il sito web:

- **\_\_init\_\_.py** è un file vuoto che istruisce Python a trattare questa directory come un pacchetto Python.
- **settings.py** contiene tutte le impostazioni del sito web, inclusa la registrazione di qualsiasi applicazione creata, la posizione dei file statici, i dettagli di configurazione del database, ecc.
- **urls.py** definisce le mappature URL-to-view del sito. Sebbene possa contenere _tutto_ il codice di mappatura degli URL, è più comune delegare alcune delle mappature a particolari applicazioni, come vedrai in seguito.
- **wsgi.py** viene utilizzato per aiutare l'applicazione Django a comunicare con il server web. Può essere trattato come un boilerplate.
- **asgi.py** è uno standard per le app web Python asincrone e i server per comunicare tra loro. L'Asynchronous Server Gateway Interface (ASGI) è il successore asincrono del Web Server Gateway Interface (WSGI). ASGI fornisce uno standard sia per le app Python asincrone che sincrone, mentre WSGI forniva uno standard solo per le app sincrone. ASGI è retrocompatibile con WSGI e supporta server e framework di applicazioni multipli.

Lo script **manage.py** viene utilizzato per creare applicazioni, lavorare con i database e avviare il server web di sviluppo.

## Creare l'applicazione catalog

Successivamente, eseguire il seguente comando per creare l'applicazione _catalog_ che vivrà all'interno del nostro progetto _locallibrary_. Assicurarsi di eseguire questo comando dalla stessa cartella del **manage.py** del progetto:

```bash
# Linux/macOS
python3 manage.py startapp catalog

# Windows
py manage.py startapp catalog
```

> [!NOTE]
> Il resto del tutorial utilizza la sintassi Linux/macOS.
> Se si lavora su Windows, ovunque si veda un comando che inizia con `python3` è necessario utilizzare `py` (o `py -3`).

Lo strumento crea una nuova cartella e la popola con file per le diverse parti dell'applicazione (mostrato nel seguente esempio).
La maggior parte dei file sono denominati in base al loro scopo (ad esempio, le viste devono essere archiviate in **views.py**, i modelli in **models.py**, i test in **tests.py**, la configurazione del sito di amministrazione in **admin.py**, la registrazione dell'applicazione in **apps.py**) e contengono del codice boilerplate minimo per operare con gli oggetti associati.

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

Inoltre, ora abbiamo:

- Una cartella _migrations_, usata per memorizzare le "migrazioni" — file che consentono di aggiornare automaticamente il database mentre si modificano i modelli.
- **\_\_init\_\_.py** — un file vuoto creato qui in modo che Django/Python riconosca la cartella come un [Pacchetto Python](https://docs.python.org/3/tutorial/modules.html#packages) e consenta di utilizzare i suoi oggetti all'interno di altre parti del progetto.

> [!NOTE]
> Ha notato cosa manca nella lista dei file sopra? Sebbene ci sia un posto per le viste e i modelli, non c'è alcun posto dove posizionare i mapping degli URL, i template e i file statici. Ti mostreremo come crearli più avanti (questi non sono necessari in ogni sito web, ma sono necessari in questo esempio).

## Registrare l'applicazione catalog

Ora che l'applicazione è stata creata, bisogna registrarla con il progetto in modo che venga inclusa quando vengono eseguiti gli strumenti (come l'aggiunta di modelli al database per esempio). Le applicazioni sono registrate aggiungendole alla lista `INSTALLED_APPS` nelle impostazioni del progetto.

Aprire il file delle impostazioni del progetto, **django-locallibrary-tutorial/locallibrary/settings.py**, e trovare la definizione per la lista `INSTALLED_APPS`. Poi aggiungere una nuova riga alla fine della lista, come mostrato qui sotto:

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

La nuova riga specifica l'oggetto di configurazione dell'applicazione (`CatalogConfig`) che è stato generato nel file **/django-locallibrary-tutorial/catalog/apps.py** quando è stata creata l'applicazione.

> [!NOTE]
> Noterà che ci sono già molte altre `INSTALLED_APPS` (e `MIDDLEWARE`, più in basso nel file delle impostazioni). Questi abilitano il supporto per il [sito di amministrazione di Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/Admin_site) e la funzionalità che utilizza (incluse sessioni, autenticazione, ecc.).

## Specificare il database

Questo è anche il punto in cui normalmente si specificherebbe il database da utilizzare per il progetto. Ha senso usare lo stesso database per sviluppo e produzione dove possibile, per evitare piccole differenze nel comportamento. Può trovare informazioni sulle diverse opzioni in [Databases](https://docs.djangoproject.com/en/5.0/ref/settings/#databases) (documentazione Django).

Useremo il database SQLite predefinito per la maggior parte di questo esempio, poiché non ci aspettiamo di richiedere molto accesso concorrente a un database di dimostrazione, e non richiede alcun lavoro aggiuntivo per essere impostato! Può vedere come questo database è configurato in **settings.py**:

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```

Successivamente, nel [Distribuire Django in produzione](/it/docs/Learn_web_development/Extensions/Server-side/Django/Deployment#database_configuration) ti mostreremo anche come configurare un database Postgres, che potrebbe essere più adatto per siti più grandi.

## Altre impostazioni del progetto

Il file **settings.py** è anche usato per configurare una serie di altre impostazioni, ma a questo punto probabilmente vorrà solo cambiare il [TIME_ZONE](https://docs.djangoproject.com/en/5.0/ref/settings/#std:setting-TIME_ZONE) — questo dovrebbe essere reso uguale a una stringa dall'[Elenco standard dei fusi orari del database tz](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) (la colonna TZ nella tabella contiene i valori desiderati). Cambiare il valore `TIME_ZONE` con una di queste stringhe appropriata per il suo fuso orario, ad esempio:

```python
TIME_ZONE = 'Europe/London'
```

Ci sono altre due impostazioni che non cambierà ora, ma di cui dovrebbe essere consapevole:

- `SECRET_KEY`. Questa è una chiave segreta che viene utilizzata come parte della strategia di sicurezza del sito web di Django. Se non sta proteggendo questo codice in sviluppo dovrà usare un codice diverso (forse letto da una variabile d'ambiente o file) quando lo mette in produzione.
- `DEBUG`. Questo abilita i log di debug a essere mostrati in caso di errore, piuttosto che risposte di stato HTTP. Questo dovrebbe essere impostato su `False` in produzione poiché le informazioni di debug sono utili per gli attaccanti, ma per ora possiamo lasciarlo impostato su `True`.

## Collegare il mapper URL

Il sito web viene creato con un file mapper URL (**urls.py**) nella cartella del progetto. Sebbene possa usare questo file per gestire tutte le mappature URL, è più abituale differire le mappature all'applicazione associata.

Aprire **django-locallibrary-tutorial/locallibrary/urls.py** e notare il testo istruttivo che spiega alcuni dei modi di usare il mapper URL.

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

Le mappature degli URL sono gestite attraverso la variabile `urlpatterns`, che è una _lista_ Python di funzioni `path()`. Ogni funzione `path()` associa un pattern URL a una _vista specifica_, che verrà mostrata quando il pattern viene corrisposto, o con un'altra lista di codice di test del pattern URL (in questo secondo caso, il pattern diventa l'URL di "base" per i pattern definiti nel modulo di destinazione). La lista `urlpatterns` definisce inizialmente una singola funzione che mappa tutti gli URL con il pattern _admin/_ al modulo `admin.site.urls`, che contiene le definizioni di mapping URL dell'applicazione di amministrazione.

> [!NOTE]
> La rotta in `path()` è una stringa che definisce un pattern URL da corrispondere. Questa stringa potrebbe includere una variabile nominata (tra parentesi angolari), ad esempio, `'catalog/<id>/'`. Questo pattern corrisponderà a un URL come **catalog/_qualsiasi_carattere_/** e passerà _`qualsiasi_carattere`_ alla vista come stringa con il nome del parametro `id`. Discutiamo ulteriormente i metodi path e i pattern di rotta negli argomenti successivi.

Per aggiungere un nuovo elemento alla lista `urlpatterns`, aggiungere le seguenti righe alla fine del file. Questo nuovo elemento include un `path()` che inoltra le richieste con il pattern `catalog/` al modulo `catalog.urls` (il file con l'URL relativo **catalog/urls.py**).

```python
# Use include() to add paths from the catalog application
from django.urls import include

urlpatterns += [
    path('catalog/', include('catalog.urls')),
]
```

> [!NOTE]
> Notare che abbiamo incluso la linea di importazione (`from django.urls import include`) con il codice che lo utilizza (così è facile vedere cosa abbiamo aggiunto), ma è comune includere tutte le linee di importazione all'inizio di un file Python.

Ora reindirizziamo l'URL root del nostro sito (cioè, `127.0.0.1:8000`) all'URL `127.0.0.1:8000/catalog/`. Questa è l'unica app che utilizzeremo in questo progetto. Per fare questo, utilizzeremo una funzione di vista speciale, `RedirectView`, che prende il nuovo URL relativo a cui reindirizzare (`/catalog/`) come suo primo argomento quando il pattern URL specificato nella funzione `path()` è corrisposto (l'URL radice, in questo caso).

Aggiungere le seguenti righe alla fine del file:

```python
# Add URL maps to redirect the base URL to our application
from django.views.generic import RedirectView
urlpatterns += [
    path('', RedirectView.as_view(url='catalog/', permanent=True)),
]
```

Lasciare il primo parametro della funzione path vuoto per implicare '/'. Se si scrive il primo parametro come '/' Django darà il seguente avviso quando si avvia il server di sviluppo:

```python
System check identified some issues:

WARNINGS:
?: (urls.W002) Your URL pattern '/' has a route beginning with a '/'.
Remove this slash as it is unnecessary.
If this pattern is targeted in an include(), ensure the include() pattern has a trailing '/'.
```

Django non serve file statici come CSS, JavaScript e immagini di default, ma può essere utile per il server web di sviluppo farlo mentre sta creando il sito. Come aggiunta finale a questo mapper URL, può abilitare il servizio di file statici durante lo sviluppo aggiungendo le seguenti righe.

Aggiungere ora il seguente blocco finale alla fine del file:

```python
# Use static() to add URL mapping to serve static files during development (only)
from django.conf import settings
from django.conf.urls.static import static

urlpatterns += static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
```

> [!NOTE]
> Ci sono diversi modi per estendere la lista `urlpatterns` (precedentemente, abbiamo semplicemente aggiunto un nuovo elemento alla lista usando l'operatore `+=` per separare chiaramente il vecchio e il nuovo codice). Potremmo invece includere questo nuovo pattern-map nella definizione originale della lista:
>
> ```python
> urlpatterns = [
>     path('admin/', admin.site.urls),
>     path('catalog/', include('catalog.urls')),
>     path('', RedirectView.as_view(url='catalog/')),
> ] + static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
> ```

Come passo finale, creare un file all'interno della sua cartella _catalog_ chiamato **urls.py**, e aggiungere il seguente testo per definire l'importato `urlpatterns` (vuoto). Questo è dove aggiungeremo i nostri pattern man mano che costruiamo l'applicazione.

```python
from django.urls import path
from . import views

urlpatterns = [

]
```

## Testare il framework del sito web

A questo punto abbiamo un progetto scheletro completo. Il sito web in realtà non _fa_ nulla ancora, ma vale la pena di eseguirlo per assicurarci che nessuna delle nostre modifiche abbia rotto qualcosa.

Prima di farlo, dovremmo prima eseguire una _migrazione del database_. Questo aggiorna il nostro database (per includere eventuali modelli nelle nostre applicazioni installate) e rimuove alcuni avvisi di build.

### Eseguire le migrazioni del database

Django utilizza un Object-Relational-Mapper (ORM) per mappare le definizioni dei modelli nel codice Django alla struttura dei dati utilizzata dal database sottostante. Man mano che cambiamo le nostre definizioni di modelli, Django traccia le modifiche e può creare script di migrazione del database (in **/django-locallibrary-tutorial/catalog/migrations/**) per migrare automaticamente la struttura dei dati sottostante nel database per adattarsi al modello.

Quando abbiamo creato il sito web, Django ha aggiunto automaticamente un certo numero di modelli per l'utilizzo da parte della sezione di amministrazione del sito (che esamineremo più avanti). Eseguire i seguenti comandi per definire tabelle per quei modelli nel database (assicurarsi di essere nella directory che contiene **manage.py**):

```bash
python3 manage.py makemigrations
python3 manage.py migrate
```

> [!WARNING]
> Dovrà eseguire questi comandi ogni volta che i modelli cambiano in modo da influire sulla struttura dei dati che deve essere memorizzata (inclusa sia l'aggiunta che la rimozione di modelli interi e campi individuali).

Il comando `makemigrations` _crea_ (ma non applica) le migrazioni per tutte le applicazioni installate nel suo progetto. Può anche specificare il nome dell'applicazione per eseguire una migrazione solo per un'app singola. Ciò le dà la possibilità di esaminare il codice di queste migrazioni prima che vengano applicate. Se è un esperto di Django, potrebbe scegliere di modificarle leggermente!

Il comando `migrate` è ciò che applica le migrazioni al suo database. Django tiene traccia di quelle che sono state aggiunte al database attuale.

> [!NOTE]
> Dovrebbe ri-eseguire le migrazioni e ri-testare il sito ogni volta che si effettuano modifiche significative. Non ci vuole molto tempo!
>
> Veda [Migrazioni](https://docs.djangoproject.com/en/5.0/topics/migrations/) (documentazione Django) per ulteriori informazioni sui comandi di migrazione meno comunemente usati.

### Eseguire il sito web

Durante lo sviluppo, può servire il sito web prima utilizzando il _server web di sviluppo_, e poi visualizzarlo sul suo browser web locale.

> [!NOTE]
> Il server web di sviluppo non è robusto o performante abbastanza per l'uso in produzione, ma è un modo molto semplice per mettere in funzione il suo sito web Django durante lo sviluppo per un test rapido e conveniente. Di default servirà il sito al suo computer locale (`http://127.0.0.1:8000/)`, ma può anche specificare altri computer sulla sua rete a cui servire. Per ulteriori informazioni, veda [django-admin and manage.py: runserver](https://docs.djangoproject.com/en/5.0/ref/django-admin/#runserver) (documentazione Django).

Esegua il _server web di sviluppo_ chiamando il comando `runserver` (nella stessa directory di **manage.py**):

```bash
python3 manage.py runserver
```

Una volta che il server è in esecuzione, può visualizzare il sito navigando su `http://127.0.0.1:8000/` nel suo browser web locale. Dovrebbe vedere una pagina di errore del sito che appare così:

![Pagina di debug di Django (Django 4.2)](django_404_debug_page.png)

Non si preoccupi! Questa pagina di errore è prevista perché non abbiamo alcuna pagina/url definita nel modulo `catalog.urls` (a cui siamo reindirizzati quando riceviamo un URL alla radice del sito).

A questo punto, sappiamo che Django sta funzionando!

> [!NOTE]
> La pagina di esempio dimostra una grande caratteristica di Django — la registrazione automatica del debug. Ogni volta che una pagina non può essere trovata, Django visualizza una schermata di errore con informazioni utili o qualsiasi errore sollevato dal codice. In questo caso, possiamo vedere che l'URL che abbiamo fornito non corrisponde a nessuno dei nostri pattern URL (come elencati). La registrazione è disattivata in produzione (cioè quando mettiamo il sito in diretta su Web), nel qual caso verrà servita una pagina meno informativa ma più user-friendly.

## Non dimenticare di eseguire il backup su GitHub

Abbiamo appena fatto un lavoro significativo, quindi ora è un buon momento per eseguire il backup del progetto utilizzando GitHub.

Per prima cosa, sposta il _contenuto_ della cartella di livello superiore **locallibrary** nella cartella **django_local_library** che hai [creato come repository locale GitHub](/it/docs/Learn_web_development/Extensions/Server-side/Django/development_environment#clone_the_repo_to_your_local_computer) quando hai impostato l'ambiente di sviluppo.
Questo includerà **manage.py**, la sottocartella **locallibrary**, la sottocartella **catalog**, e tutto il resto all'interno della cartella di livello superiore.

Poi aggiungi e registra i cambiamenti nella cartella **django_local_library** e spingili su GitHub.
Dalla radice di quella cartella, puoi usare un insieme di comandi simile a quelli nella sezione [Modify and sync changes](/it/docs/Learn_web_development/Extensions/Server-side/Django/development_environment#modify_and_sync_changes) del tema _Development environment_:

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

Poi crea e unisci un PR dal tuo repo GitHub.
Dopo aver unito puoi passare di nuovo al branch `main` e tirare i tuoi cambiamenti da GitHub:

```bash
git checkout main
git pull origin main
```

> [!NOTE]
> Se non si elimina il branch `skeleton_website` è sempre possibile tornarci in un secondo momento.

Non lo menzioneremo necessariamente di nuovo in futuro, ma potrebbe trovare utile aggiornare GitHub con i suoi cambiamenti al termine di ciascuna sezione di questo tutorial.

## Mettersi alla prova

La directory **catalog/** contiene file per le viste, i modelli, e altre parti dell'applicazione. Apri questi file e ispeziona il boilerplate.

Come hai visto in precedenza, un mapping URL per il sito Admin è già stato aggiunto nel **urls.py** del progetto. Naviga nell'area di amministrazione del tuo browser e vedi cosa succede (puoi dedurre l'URL corretto dal mapping).

## Sommario

Ora ha creato un progetto di sito web scheletro completo, che può procedere a popolare con URL, modelli, viste e template.

Ora che lo scheletro per il [sito web della Biblioteca Locale](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website) è completo e in esecuzione, è tempo di iniziare a scrivere il codice che farà sì che questo sito web faccia ciò che deve fare.

## Vedi anche

- [Scrivere la sua prima app Django - parte 1](https://docs.djangoproject.com/en/5.0/intro/tutorial01/) (documentazione Django)
- [Applicazioni](https://docs.djangoproject.com/en/5.0/ref/applications/#configuring-applications) (documentazione Django).
  Contiene informazioni sulla configurazione delle applicazioni.

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website", "Learn_web_development/Extensions/Server-side/Django/Models", "Learn_web_development/Extensions/Server-side/Django")}}
