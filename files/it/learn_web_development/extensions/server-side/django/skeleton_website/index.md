---
title: "Django Tutorial Parte 2: Creazione di un sito web scheletro"
short-title: "2: Sito web scheletro"
slug: Learn_web_development/Extensions/Server-side/Django/skeleton_website
l10n:
  sourceCommit: be1922d62a0d31e4e3441db0e943aed8df736481
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website", "Learn_web_development/Extensions/Server-side/Django/Models", "Learn_web_development/Extensions/Server-side/Django")}}

Questo secondo articolo del nostro [Django Tutorial](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website) mostra come è possibile creare un progetto di sito web "scheletro" come base, che puoi poi popolare con impostazioni specifiche del sito, percorsi, modelli, viste e template.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/development_environment">Impostare un ambiente di sviluppo Django</a>.
        Rivedere il <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website">Django Tutorial</a>.
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

Questo articolo mostra come è possibile creare un sito web "scheletro", che puoi poi popolare con impostazioni specifiche del sito, percorsi, modelli, viste e template (ne discutiamo in articoli successivi).

Per iniziare:

1. Usa lo strumento `django-admin` per generare una cartella di progetto, i modelli di file di base e **manage.py**, che funge da script di gestione del progetto.
2. Usa **manage.py** per creare una o più _applicazioni_.

   > [!NOTE]
   > Un sito web può essere composto da una o più sezioni. Ad esempio, sito principale, blog, wiki, area download, ecc. Django incoraggia a sviluppare questi componenti come _applicazioni_ separate, che potrebbero poi essere riutilizzate in diversi progetti, se lo si desidera.

3. Registra le nuove applicazioni per includerle nel progetto.
4. Collegare il mapper **url/path** per ciascuna applicazione.

Per il [sito web della Biblioteca Locale](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website), il sito web e le cartelle di progetto sono denominate _locallibrary_, e includono un'applicazione chiamata _catalog_.
La struttura di cartelle a livello superiore sarà quindi la seguente:

```bash
locallibrary/         # Website folder
    manage.py         # Script to run Django tools for this project (created using django-admin)
    locallibrary/     # Website/project folder (created using django-admin)
    catalog/          # Application folder (created using manage.py)
```

Le sezioni seguenti discutono i passaggi del processo in dettaglio e mostrano come è possibile testare le modifiche.
Alla fine di questo articolo, discutiamo altre configurazioni a livello di sito che potresti anche fare a questo stadio.

## Creazione del progetto

Per creare il progetto:

1. Apri una shell di comando (o una finestra del terminale) e assicurati di essere nel tuo [ambiente virtuale](/it/docs/Learn_web_development/Extensions/Server-side/Django/development_environment#using_a_virtual_environment).
2. Naviga nella cartella in cui desideri creare la tua applicazione libreria locale (più avanti la sposteremo nella cartella "django_local_library" che hai [creato come repository locale GitHub](/it/docs/Learn_web_development/Extensions/Server-side/Django/development_environment#clone_the_repo_to_your_local_computer) durante la configurazione dell'ambiente di sviluppo).
3. Crea il nuovo progetto utilizzando il comando `django-admin startproject` come mostrato, quindi naviga nella cartella del progetto:

   ```bash
   django-admin startproject locallibrary
   cd locallibrary
   ```

   Lo strumento `django-admin` crea una struttura di cartelle/file come segue:

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

La sotto-cartella del progetto _locallibrary_ è il punto di ingresso per il sito web:

- **\_\_init\_\_.py** è un file vuoto che istruisce Python a trattare questa directory come un pacchetto Python.
- **settings.py** contiene tutte le impostazioni del sito web, inclusa la registrazione di qualunque applicazione creiamo, la posizione dei nostri file statici, i dettagli di configurazione del database, ecc.
- **urls.py** definisce le mappature URL-to-view del sito. Anche se questo potrebbe contenere _tutta_ la mappatura degli URL, è più comune delegare alcune delle mappature a particolari applicazioni, come vedrai in seguito.
- **wsgi.py** viene utilizzato per aiutare la tua applicazione Django a comunicare con il server web. Puoi considerarlo come boilerplate.
- **asgi.py** è uno standard per le app web asincrone Python e i server per comunicare tra loro. L'Asynchronous Server Gateway Interface (ASGI) è il successore asincrono del Web Server Gateway Interface (WSGI). ASGI fornisce uno standard per app Python sia asincrone che sincrone, mentre WSGI forniva uno standard solo per app sincrone. ASGI è compatibile con WSGI e supporta più server e framework applicativi.

Lo script **manage.py** viene utilizzato per creare applicazioni, lavorare con i database e avviare il server web di sviluppo.

## Creazione dell'applicazione catalog

Successivamente, esegui il seguente comando per creare l'applicazione _catalog_ che vivrà all'interno del nostro progetto _locallibrary_. Assicurati di eseguire questo comando dalla stessa cartella in cui si trova il tuo **manage.py** del progetto:

```bash
# Linux/macOS
python3 manage.py startapp catalog

# Windows
py manage.py startapp catalog
```

> [!NOTE]
> Il resto del tutorial utilizza la sintassi Linux/macOS.
> Se stai lavorando su Windows, ogni volta che vedi un comando che inizia con `python3` dovresti invece usare `py` (o `py -3`).

Lo strumento crea una nuova cartella e la popola con file per le diverse parti dell'applicazione (mostrate nell'esempio seguente).
La maggior parte dei file è denominata in base al loro scopo (ad esempio, le viste dovrebbero essere archiviate in **views.py**, i modelli in **models.py**, i test in **tests.py**, la configurazione del sito di amministrazione in **admin.py**, la registrazione dell'applicazione in **apps.py**) e contengono del codice boilerplate minimo per lavorare con gli oggetti associati.

La directory aggiornata del progetto dovrebbe ora apparire così:

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

Inoltre, abbiamo ora:

- Una cartella _migrations_, utilizzata per memorizzare le "migrazioni" — file che consentono di aggiornare automaticamente il database mentre modifichi i tuoi modelli.
- **\_\_init\_\_.py** — un file vuoto creato qui affinché Django/Python riconosca la cartella come un [Pacchetto Python](https://docs.python.org/3/tutorial/modules.html#packages) e consenta di utilizzare i suoi oggetti all'interno di altre parti del progetto.

> [!NOTE]
> Hai notato cosa manca dall'elenco dei file sopra? Sebbene ci sia un posto per le tue viste e modelli, non c'è un posto per mettere le tue mappature URL, template e file statici. Ti mostreremo come crearli più avanti (questi non sono necessari in ogni sito web ma lo sono in questo esempio).

## Registrazione dell'applicazione catalog

Ora che l'applicazione è stata creata, dobbiamo registrarla con il progetto in modo che venga inclusa quando vengono eseguiti strumenti (come l'aggiunta di modelli al database, per esempio). Le applicazioni sono registrate aggiungendole alla lista `INSTALLED_APPS` nelle impostazioni del progetto.

Apri il file delle impostazioni del progetto, **django-locallibrary-tutorial/locallibrary/settings.py**, e trova la definizione per la lista `INSTALLED_APPS`. Quindi aggiungi una nuova riga alla fine dell'elenco, come mostrato di seguito:

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

La nuova riga specifica l'oggetto di configurazione dell'applicazione (`CatalogConfig`) che è stato generato per te in **/django-locallibrary-tutorial/catalog/apps.py** quando hai creato l'applicazione.

> [!NOTE]
> Noterai che ci sono già molte altre `INSTALLED_APPS` (e `MIDDLEWARE`, più in basso nel file delle impostazioni). Questi abilitano il supporto per il [sito di amministrazione di Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/Admin_site) e le funzionalità che utilizza (incluse sessioni, autenticazione, ecc.).

## Specificare il database

Questo è anche il punto in cui normalmente specificheresti il database da utilizzare per il progetto. Ha senso utilizzare lo stesso database per lo sviluppo e la produzione dove possibile, al fine di evitare piccole differenze di comportamento. Puoi trovare informazioni sulle diverse opzioni in [Databases](https://docs.djangoproject.com/en/5.0/ref/settings/#databases) (documenti Django).

Useremo il database predefinito SQLite per la maggior parte di questo esempio, perché non prevediamo di richiedere molto accesso concorrente su un database dimostrativo, e non richiede alcun lavoro aggiuntivo per essere impostato! Puoi vedere come è configurato questo database in **settings.py**:

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```

Più avanti, nel [Distribuire Django in produzione](/it/docs/Learn_web_development/Extensions/Server-side/Django/Deployment#database_configuration) ti mostreremo anche come configurare un database Postgres, che potrebbe essere più adatto per siti di maggiori dimensioni.

## Altre impostazioni del progetto

Il file **settings.py** viene utilizzato anche per configurare un numero di altre impostazioni, ma a questo punto probabilmente vuoi solo cambiare il [TIME_ZONE](https://docs.djangoproject.com/en/5.0/ref/settings/#std:setting-TIME_ZONE) — questo dovrebbe essere uguale a una stringa proveniente dall'elenco standard dei [fusi orari del database tz](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) (la colonna TZ nella tabella contiene i valori desiderati). Cambia il valore `TIME_ZONE` con una di queste stringhe appropriate per il tuo fuso orario, ad esempio:

```python
TIME_ZONE = 'Europe/London'
```

Ci sono altre due impostazioni che non cambierai ora, ma di cui dovresti essere consapevole:

- `SECRET_KEY`. Questa è una chiave segreta utilizzata come parte della strategia di sicurezza del sito web di Django. Se non stai proteggendo questo codice in fase di sviluppo, dovrai utilizzare un codice diverso (forse letto da una variabile d'ambiente o file) quando lo metti in produzione.
- `DEBUG`. Questo abilita i log di debug da visualizzare in caso di errore, anziché nelle risposte del codice di stato HTTP. Questo dovrebbe essere impostato su `False` in produzione poiché le informazioni di debug sono utili per gli attaccanti, ma per ora possiamo tenerlo impostato su `True`.

## Collegare il mapper URL

Il sito web viene creato con un file di mapper URL (**urls.py**) nella cartella del progetto. Anche se puoi usare questo file per gestire tutte le tue mappature URL, è più comune deferire le mappature all'applicazione associata.

Apri **django-locallibrary-tutorial/locallibrary/urls.py** e osserva il testo istruttivo che spiega alcuni dei modi per utilizzare il mapper URL.

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

Le mappature URL sono gestite tramite la variabile `urlpatterns`, che è un _elenco_ _Python_ di funzioni `path()`. Ogni funzione `path()` associa un modello URL a una _vista specifica_, che verrà visualizzata quando il modello viene abbinato, o con un altro elenco di codice di test dei modelli URL (in questo secondo caso, il modello diventa l'URL "base" per i modelli definiti nel modulo di destinazione). L'elenco `urlpatterns` inizialmente definisce una singola funzione che mappa tutti gli URL con il modello _admin/_ al modulo `admin.site.urls`, che contiene le proprie definizioni di mappatura degli URL dell'applicazione di amministrazione.

> [!NOTE]
> Il percorso in `path()` è una stringa che definisce un modello URL da abbinare. Questa stringa può includere una variabile denominata (tra parentesi angolari), ad esempio, `'catalog/<id>/'`. Questo modello corrisponderà a un URL come **catalog/_qualsiasi_carattere_/** e passerà _`qualsiasi_carattere`_ alla vista come stringa con il nome del parametro `id`. Discutiamo ulteriormente i metodi del percorso e i modelli di percorso in argomenti successivi.

Per aggiungere un nuovo elemento all'elenco `urlpatterns`, aggiungi le seguenti righe alla fine del file. Questo nuovo elemento include un `path()` che inoltra le richieste con il modello `catalog/` al modulo `catalog.urls` (il file con l'URL relativo **catalog/urls.py**).

```python
# Use include() to add paths from the catalog application
from django.urls import include

urlpatterns += [
    path('catalog/', include('catalog.urls')),
]
```

> [!NOTE]
> Nota che abbiamo incluso la linea di importazione (`from django.urls import include`) con il codice che la utilizza (quindi è facile vedere cosa abbiamo aggiunto), ma è comune includere tutte le linee di importazione nella parte superiore di un file Python.

Ora redirigiamo l'URL radice del nostro sito (cioè `127.0.0.1:8000`) all'URL `127.0.0.1:8000/catalog/`. Questa è l'unica app che useremo in questo progetto. Per fare ciò, useremo una funzione di vista speciale, `RedirectView`, che prende il nuovo URL relativo a cui reindirizzare (`/catalog/`) come suo primo argomento quando il modello di URL specificato nella funzione `path()` corrisponde (l'URL radice, in questo caso).

Aggiungi le seguenti righe alla fine del file:

```python
# Add URL maps to redirect the base URL to our application
from django.views.generic import RedirectView
urlpatterns += [
    path('', RedirectView.as_view(url='catalog/', permanent=True)),
]
```

Lascia il primo parametro della funzione path vuoto per implicare '/'. Se scrivi il primo parametro come '/' Django ti darà il seguente avviso quando inizi il server di sviluppo:

```python
System check identified some issues:

WARNINGS:
?: (urls.W002) Your URL pattern '/' has a route beginning with a '/'.
Remove this slash as it is unnecessary.
If this pattern is targeted in an include(), ensure the include() pattern has a trailing '/'.
```

Django non serve file statici come CSS, JavaScript e immagini per impostazione predefinita, ma può essere utile per il server web di sviluppo farlo mentre stai creando il tuo sito. Come aggiunta finale a questo mapper URL, puoi abilitare il servizio di file statici durante lo sviluppo aggiungendo le seguenti righe.

Aggiungi ora il seguente blocco finale alla fine del file:

```python
# Use static() to add URL mapping to serve static files during development (only)
from django.conf import settings
from django.conf.urls.static import static

urlpatterns += static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
```

> [!NOTE]
> Ci sono diversi modi per estendere l'elenco `urlpatterns` (in precedenza, abbiamo semplicemente aggiunto un nuovo elemento dell'elenco usando l'operatore `+=` per separare chiaramente il vecchio e il nuovo codice). Avremmo potuto invece includere questa nuova mappa del modello nella definizione dell'elenco originale:
>
> ```python
> urlpatterns = [
>     path('admin/', admin.site.urls),
>     path('catalog/', include('catalog.urls')),
>     path('', RedirectView.as_view(url='catalog/')),
> ] + static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
> ```

Come passo finale, crea un file all'interno della tua cartella _catalog_ chiamato **urls.py**, e aggiungi il seguente testo per definire l'`urlpatterns` importato (vuoto). Questo è dove aggiungeremo i nostri modelli mentre costruiamo l'applicazione.

```python
from django.urls import path
from . import views

urlpatterns = [

]
```

## Testare il framework del sito web

A questo punto abbiamo un progetto scheletro completo. Il sito web in realtà non _fa_ nulla per ora, ma vale la pena eseguirlo per assicurarsi che nessuna delle nostre modifiche abbia rotto qualcosa.

Prima di farlo, dovremmo eseguire una _migrazione del database_. Questo aggiorna il nostro database (per includere eventuali modelli nelle nostre applicazioni installate) e rimuove alcuni avvisi di compilazione.

### Esecuzione delle migrazioni del database

Django utilizza un Object-Relational-Mapper (ORM) per mappare le definizioni dei modelli nel codice Django alla struttura dati utilizzata dal database sottostante. Man mano che cambiamo le nostre definizioni di modelli, Django tiene traccia delle modifiche e può creare script di migrazione del database (in **/django-locallibrary-tutorial/catalog/migrations/**) per migrare automaticamente la struttura dati sottostante nel database per farla corrispondere al modello.

Quando abbiamo creato il sito web, Django ha aggiunto automaticamente un numero di modelli per l'uso nella sezione di amministrazione del sito (che vedremo più avanti). Esegui i seguenti comandi per definire le tabelle per quei modelli nel database (assicurati di essere nella directory che contiene **manage.py**):

```bash
python3 manage.py makemigrations
python3 manage.py migrate
```

> [!WARNING]
> Dovrai eseguire questi comandi ogni volta che i tuoi modelli cambiano in modo che influiscano sulla struttura dei dati che deve essere archiviata (inclusa l'aggiunta e la rimozione di interi modelli e campi individuali).

Il comando `makemigrations` _crea_ (ma non applica) le migrazioni per tutte le applicazioni installate nel tuo progetto. Puoi specificare anche il nome dell'applicazione per eseguire una migrazione solo per una singola app. Questo ti dà la possibilità di controllare il codice per queste migrazioni prima che vengano applicate. Se sei esperto di Django, puoi scegliere di modificarle leggermente!

Il comando `migrate` è ciò che applica le migrazioni al tuo database. Django tiene traccia di quali sono state aggiunte al database corrente.

> [!NOTE]
> Dovresti ri-eseguire le migrazioni e ri-testare il sito ogni volta che apporti modifiche significative. Non ci vuole molto tempo!
>
> Consulta [Migrations](https://docs.djangoproject.com/en/5.0/topics/migrations/) (documenti Django) per ulteriori informazioni sui comandi di migrazione meno utilizzati.

### Esecuzione del sito web

Durante lo sviluppo, puoi servire il sito web prima utilizzando il _server web di sviluppo_ e poi visualizzandolo sul tuo browser web locale.

> [!NOTE]
> Il server web di sviluppo non è abbastanza robusto o performante per l'uso in produzione, ma è un modo molto semplice per mettere su il tuo sito web Django ed eseguirlo durante lo sviluppo per effettuare un rapido test pratico. Per impostazione predefinita, servirà il sito al tuo computer locale (`http://127.0.0.1:8000/)`, ma puoi anche specificare altri computer sulla tua rete a cui fornire il servizio. Per ulteriori informazioni consulta [django-admin and manage.py: runserver](https://docs.djangoproject.com/en/5.0/ref/django-admin/#runserver) (documenti Django).

Esegui il _server web di sviluppo_ chiamando il comando `runserver` (nella stessa directory di **manage.py**):

```bash
python3 manage.py runserver
```

Una volta che il server è in esecuzione, puoi visualizzare il sito navigando su `http://127.0.0.1:8000/` nel tuo browser web locale. Dovresti vedere una pagina di errore del sito che appare così:

![Pagina di debug di Django (Django 4.2)](django_404_debug_page.png)

Non preoccuparti! Questa pagina di errore è prevista perché non abbiamo pagine/URL definiti nel modulo `catalog.urls` (al quale siamo reindirizzati quando otteniamo un URL alla radice del sito).

A questo punto, sappiamo che Django sta funzionando!

> [!NOTE]
> La pagina di esempio dimostra una grande funzione di Django — logging di debug automatizzato. Ogni volta che una pagina non può essere trovata, Django visualizza una schermata di errore con informazioni utili o qualsiasi errore sollevato dal codice. In questo caso, possiamo vedere che l'URL che abbiamo fornito non corrisponde a nessuno dei nostri modelli di URL (come elencato). Il log è disattivato in produzione (che è quando rendiamo il sito attivo sul web), nel qual caso verrà servita una pagina meno informativa ma più user-friendly.

## Non dimenticare il backup su GitHub

Abbiamo appena fatto un lavoro significativo, quindi ora è un buon momento per fare il backup del progetto usando GitHub.

Per prima cosa sposta il _contenuto_ della cartella di livello superiore **locallibrary** nella cartella **django_local_library** che hai [creato come repository locale GitHub](/it/docs/Learn_web_development/Extensions/Server-side/Django/development_environment#clone_the_repo_to_your_local_computer) durante la configurazione dell'ambiente di sviluppo.
Ciò include **manage.py**, la sotto-cartella **locallibrary**, la sotto-cartella **catalog** e qualsiasi altra cosa all'interno della cartella di livello superiore.

Poi aggiungi e committi le modifiche nella cartella **django_local_library** e spingile su GitHub.
Dalla radice di tale cartella, puoi usare un insieme di comandi simile a quelli nella sezione [Modify and sync changes](/it/docs/Learn_web_development/Extensions/Server-side/Django/development_environment#modify_and_sync_changes) dell'argomento _Development environment_:

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

Quindi crea e unisci un PR dal tuo repo GitHub.
Dopo aver unito, puoi tornare al ramo `main` e ottenere le tue modifiche da GitHub:

```bash
git checkout main
git pull origin main
```

> [!NOTE]
> Se non elimini il ramo `skeleton_website`, puoi sempre tornare su di esso in un secondo momento.

Non necessariamente lo menzioneremo ancora in futuro, ma potresti trovare utile aggiornare GitHub con le tue modifiche alla fine di ciascuna sezione in questo tutorial.

## Sfida te stesso

La directory **catalog/** contiene file per le viste, i modelli e altre parti dell'applicazione. Apri questi file e controlla il boilerplate.

Come hai visto in precedenza, una mappatura URL per il sito di amministrazione è già stata aggiunta nel **urls.py** del progetto. Naviga nell'area di amministrazione nel tuo browser e vedi cosa succede (puoi dedurre l'URL corretto dalla mappatura).

## Riassunto

Ora hai creato un progetto di sito web scheletro completo, che puoi continuare a popolare con URL, modelli, viste e template.

Ora che lo scheletro per il [sito web della Biblioteca Locale](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website) è completo e funzionante, è tempo di iniziare a scrivere il codice che farà fare a questo sito web ciò che dovrebbe fare.

## Vedi anche

- [Scrivere la tua prima app Django - parte 1](https://docs.djangoproject.com/en/5.0/intro/tutorial01/) (documenti Django)
- [Applications](https://docs.djangoproject.com/en/5.0/ref/applications/#configuring-applications) (documenti Django).
  Contiene informazioni sulla configurazione delle applicazioni.

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website", "Learn_web_development/Extensions/Server-side/Django/Models", "Learn_web_development/Extensions/Server-side/Django")}}
