---
title: "Tutorial Django - Parte 5: Creare la pagina iniziale"
short-title: "5: Pagina iniziale"
slug: Learn_web_development/Extensions/Server-side/Django/Home_page
l10n:
  sourceCommit: 4c58f4735f986a91bee1b77e336143630df727a2
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Admin_site", "Learn_web_development/Extensions/Server-side/Django/Generic_views", "Learn_web_development/Extensions/Server-side/Django")}}

Ora è possibile aggiungere il codice che visualizza la prima pagina completa: una pagina iniziale per il sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website). La pagina iniziale mostrerà il numero di record disponibili per ciascun tipo di modello e fornirà collegamenti di navigazione nella barra laterale alle altre pagine. Durante il percorso si acquisirà esperienza pratica nella scrittura di mappe URL e view di base, nel recupero dei record dal database e nell'uso dei template.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Leggere l'<a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/Introduction">Introduzione a Django</a>. Completare gli argomenti dei tutorial precedenti (incluso <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/Admin_site">Tutorial Django - Parte 4: sito di amministrazione Django</a>).
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare a creare semplici mappe URL e view (in cui non sono codificati dati nell'URL), recuperare dati dai modelli e creare template.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

Dopo aver definito i modelli e creato alcuni record iniziali della libreria con cui lavorare, è il momento di scrivere il codice che presenta queste informazioni agli utenti. La prima cosa da fare è determinare quali informazioni visualizzare nelle pagine e definire gli URL da utilizzare per restituire tali risorse. Verranno quindi creati un mapper URL, view e template per visualizzare le pagine.

Il diagramma seguente descrive il flusso di dati principale e i componenti necessari durante la gestione di richieste e risposte HTTP. Poiché il modello è già stato implementato, i componenti principali da creare sono:

- Mapper URL per inoltrare gli URL supportati, e qualsiasi informazione codificata negli URL, alle funzioni view appropriate.
- Funzioni view per recuperare i dati richiesti dai modelli, creare pagine HTML che visualizzano i dati e restituire le pagine all'utente affinché le visualizzi nel browser.
- Template da utilizzare durante il rendering dei dati nelle view.

![Diagramma del flusso di dati principale: componenti URL, Model, View e Template necessari durante la gestione di richieste e risposte HTTP in un'applicazione Django. Una richiesta HTTP raggiunge un server Django e viene inoltrata al file 'urls.py' del componente URLS. La richiesta viene inoltrata alla view appropriata. La view può leggere e scrivere dati dal file 'models.py' dei Models, contenente il codice relativo ai modelli. La view accede anche al componente template del file HTML. La view restituisce la risposta all'utente.](basic-django.png)

Come verrà illustrato nella sezione successiva, sono disponibili 5 pagine da visualizzare, una quantità di informazioni eccessiva da documentare in un singolo articolo. Pertanto, questo articolo si concentrerà sull'implementazione della pagina iniziale, mentre le altre pagine saranno trattate in un articolo successivo. Questo dovrebbe fornire una buona comprensione completa di come mapper URL, view e modelli funzionano nella pratica.

## Definire gli URL delle risorse

Poiché questa versione di [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website) è essenzialmente di sola lettura per gli utenti finali, è sufficiente fornire una pagina di destinazione per il sito, ovvero una pagina iniziale, e pagine che _visualizzano_ viste elenco e di dettaglio per libri e autori.

Gli URL necessari per le pagine sono:

- `catalog/` — La pagina iniziale (index).
- `catalog/books/` — Un elenco di tutti i libri.
- `catalog/authors/` — Un elenco di tutti gli autori.
- `catalog/book/<id>` — La vista di dettaglio di un particolare libro, con chiave primaria del campo `<id>` (predefinita). Ad esempio, l'URL del terzo libro aggiunto all'elenco sarà `/catalog/book/3`.
- `catalog/author/<id>` — La vista di dettaglio dell'autore specifico con chiave primaria del campo `<id>`. Ad esempio, l'URL dell'undicesimo autore aggiunto all'elenco sarà `/catalog/author/11`.

I primi tre URL restituiranno la pagina index, l'elenco dei libri e l'elenco degli autori. Questi URL non codificano informazioni aggiuntive e le query che recuperano i dati dal database saranno sempre le stesse. Tuttavia, i risultati restituiti dalle query dipenderanno dal contenuto del database.

Al contrario, gli ultimi due URL visualizzeranno informazioni dettagliate su un libro o autore specifico. Questi URL codificano l'identità dell'elemento da visualizzare, rappresentata sopra da `<id>`. Il mapper URL estrarrà le informazioni codificate e le passerà alla view, che determinerà dinamicamente quali informazioni recuperare dal database. Codificando le informazioni nell'URL, verrà utilizzato un singolo insieme di mappatura URL, view e template per gestire tutti i libri, o tutti gli autori.

> [!NOTE]
> Con Django è possibile costruire gli URL secondo necessità: è possibile codificare le informazioni nel corpo dell'URL come mostrato sopra oppure includere parametri `GET` nell'URL, ad esempio `/book/?id=6`. Qualunque approccio venga utilizzato, gli URL devono essere mantenuti puliti, logici e leggibili, come [raccomandato dal W3C](https://www.w3.org/Provider/Style/URI).
> La documentazione di Django raccomanda di codificare le informazioni nel corpo dell'URL per ottenere una progettazione migliore degli URL.

Come accennato nella panoramica, il resto dell'articolo descrive come costruire la pagina index.

## Creare la pagina index

La prima pagina da creare è la pagina index (`catalog/`). La pagina index includerà codice HTML statico insieme a "conteggi" generati di diversi record nel database. Per farlo, verranno creati una mappatura URL, una view e un template.

> [!NOTE]
> Vale la pena prestare un po' di attenzione in più a questa sezione. La maggior parte delle informazioni si applica anche alle altre pagine che verranno create.

### Mappatura URL

Quando è stato creato il [sito web scheletro](/it/docs/Learn_web_development/Extensions/Server-side/Django/skeleton_website), il file **locallibrary/urls.py** è stato aggiornato per assicurarsi che, ogni volta che viene ricevuto un URL che inizia con `catalog/`, il modulo _URLConf_ `catalog.urls` elabori la sottostringa rimanente.

Il seguente frammento di codice da **locallibrary/urls.py** include il modulo `catalog.urls`:

```python
urlpatterns += [
    path('catalog/', include('catalog.urls')),
]
```

> [!NOTE]
> Ogni volta che Django incontra la funzione di importazione [`django.urls.include()`](https://docs.djangoproject.com/en/5.0/ref/urls/#django.urls.include), divide la stringa URL al carattere finale designato e invia la sottostringa rimanente al modulo _URLConf_ incluso per un'ulteriore elaborazione.

È stato inoltre creato un file segnaposto per il modulo _URLConf_, denominato **/catalog/urls.py**.
Aggiungere le righe seguenti a quel file:

```python
urlpatterns = [
    path('', views.index, name='index'),
]
```

La funzione `path()` definisce quanto segue:

- Un pattern URL, che è una stringa vuota: `''`. I pattern URL saranno trattati in dettaglio durante il lavoro sulle altre view.
- Una funzione view che verrà chiamata se viene rilevato il pattern URL: `views.index`, ovvero la funzione denominata `index()` nel file **views.py**.

La funzione `path()` specifica anche un parametro `name`, che è un identificatore univoco per _questa_ particolare mappatura URL. È possibile utilizzare il nome per "invertire" il mapper, ovvero per creare dinamicamente un URL che punta alla risorsa che il mapper è progettato per gestire.
Ad esempio, è possibile usare il parametro name per collegarsi alla pagina iniziale da qualsiasi altra pagina aggiungendo il seguente collegamento in un template:

```django
<a href="{% url 'index' %}">Home</a>.
```

> [!NOTE]
> È possibile codificare il collegamento direttamente, come in `<a href="/catalog/">Home</a>`, ma se viene modificato il pattern della pagina iniziale, ad esempio in `/catalog/index`, i template non si collegheranno più correttamente. Usare una mappatura URL invertita è più robusto.

### View (basata su funzione)

Una view è una funzione che elabora una richiesta HTTP, recupera i dati richiesti dal database, esegue il rendering dei dati in una pagina HTML usando un template HTML e poi restituisce l'HTML generato in una risposta HTTP per visualizzare la pagina all'utente. La view index segue questo modello: recupera informazioni sul numero di record `Book`, `BookInstance`, `BookInstance` disponibili e `Author` presenti nel database, e passa tali informazioni a un template per la visualizzazione.

Aprire **catalog/views.py** e notare che il file importa già la funzione di scelta rapida [render()](https://docs.djangoproject.com/en/5.0/topics/http/shortcuts/#django.shortcuts.render), per generare un file HTML usando un template e dati:

```python
from django.shortcuts import render

# Create your views here.
```

Incollare le seguenti righe alla fine del file:

```python
from .models import Book, Author, BookInstance, Genre

def index(request):
    """View function for home page of site."""

    # Generate counts of some of the main objects
    num_books = Book.objects.all().count()
    num_instances = BookInstance.objects.all().count()

    # Available books (status = 'a')
    num_instances_available = BookInstance.objects.filter(status__exact='a').count()

    # The 'all()' is implied by default.
    num_authors = Author.objects.count()

    context = {
        'num_books': num_books,
        'num_instances': num_instances,
        'num_instances_available': num_instances_available,
        'num_authors': num_authors,
    }

    # Render the HTML template index.html with the data in the context variable
    return render(request, 'index.html', context=context)
```

La prima riga importa le classi del modello che verranno utilizzate per accedere ai dati in tutte le view.

La prima parte della funzione view recupera il numero di record usando l'attributo `objects.all()` sulle classi del modello. Recupera inoltre un elenco di oggetti `BookInstance` che hanno il valore 'a' (Available) nel campo status. Per ulteriori informazioni su come accedere ai dati del modello, consultare il precedente tutorial [Tutorial Django - Parte 3: usare i modelli > Cercare record](/it/docs/Learn_web_development/Extensions/Server-side/Django/Models#searching_for_records).

Alla fine della funzione view viene chiamata la funzione `render()` per creare una pagina HTML e restituirla come risposta. Questa funzione di scelta rapida incapsula diverse altre funzioni per semplificare un caso d'uso molto comune. La funzione `render()` accetta i seguenti parametri:

- L'oggetto `request` originale, che è un `HttpRequest`.
- Un template HTML con segnaposto per i dati.
- Una variabile `context`, che è un dizionario Python contenente i dati da inserire nei segnaposto.

Nella prossima sezione verranno approfonditi i template e la variabile `context`. Ora è il momento di creare il template, così da poter effettivamente visualizzare qualcosa per l'utente.

### Template

Un template è un file di testo che definisce la struttura o il layout di un file, ad esempio una pagina HTML, e usa segnaposto per rappresentare il contenuto effettivo.

Un'applicazione Django creata usando **startapp**, come lo scheletro di questo esempio, cercherà i template in una sottodirectory denominata '**templates**' delle applicazioni. Ad esempio, nella view index appena aggiunta, la funzione `render()` si aspetterà di trovare il file **_index.html_** in **/django-locallibrary-tutorial/catalog/templates/** e genererà un errore se il file non è presente.

È possibile verificarlo salvando le modifiche precedenti e accedendo a `127.0.0.1:8000` nel browser: verrà visualizzato un messaggio di errore piuttosto intuitivo, "TemplateDoesNotExist at /catalog/", insieme ad altri dettagli.

> [!NOTE]
> In base al file delle impostazioni del progetto, Django cercherà i template in vari percorsi, effettuando per impostazione predefinita la ricerca nelle applicazioni installate. Per ulteriori informazioni su come Django trova i template e sui formati di template supportati, consultare [la sezione Templates della documentazione Django](https://docs.djangoproject.com/en/5.0/topics/templates/).

#### Estendere i template

Il template index avrà bisogno del markup HTML standard per head e body, insieme a sezioni di navigazione per collegarsi alle altre pagine del sito, che non sono ancora state create, e a sezioni che visualizzano testo introduttivo e dati sui libri.

Gran parte della struttura HTML e di navigazione sarà identica in ogni pagina del sito. Invece di duplicare il codice boilerplate in ogni pagina, è possibile usare il linguaggio di templating Django per dichiarare un template di base e poi estenderlo per sostituire solo le parti diverse per ciascuna pagina specifica.

Il seguente frammento di codice è un esempio di template di base da un file **base_generic.html**.
Il template per LocalLibrary verrà creato a breve.
L'esempio seguente include HTML comune con sezioni per un titolo, una barra laterale e contenuti principali contrassegnati con i tag di template denominati `block` e `endblock`.
I blocchi possono essere lasciati vuoti oppure possono includere contenuti predefiniti da usare durante il rendering delle pagine derivate dal template.

> [!NOTE]
> I _tag_ di template sono funzioni che possono essere utilizzate in un template per iterare su elenchi, eseguire operazioni condizionali in base al valore di una variabile e così via. Oltre ai tag di template, la sintassi dei template consente di fare riferimento a variabili passate nel template dalla view e di usare _filtri di template_ per formattare le variabili, ad esempio per convertire una stringa in minuscolo.

```django
<!doctype html>
<html lang="en">
  <head>
    {% block title %}
      <title>Local Library</title>
    {% endblock %}
  </head>
  <body>
    {% block sidebar %}
      <!-- insert default navigation text for every page -->
    {% endblock %}
    {% block content %}
      <!-- default content text (typically empty) -->
    {% endblock %}
  </body>
</html>
```

Quando si definisce un template per una view particolare, si specifica prima il template di base usando il tag di template `extends`, come mostrato nell'esempio di codice seguente. Quindi si dichiarano le sezioni del template da sostituire, se presenti, usando sezioni `block`/`endblock` come nel template di base.

Ad esempio, il frammento di codice seguente mostra come usare il tag di template `extends` e sovrascrivere il blocco `content`. L'HTML generato includerà il codice e la struttura definiti nel template di base, compreso il contenuto predefinito definito nel blocco `title`, ma con il nuovo blocco `content` al posto di quello predefinito.

```django
{% extends "base_generic.html" %}

{% block content %}
  <h1>Local Library Home</h1>
  <p>
    Welcome to LocalLibrary, a website developed by
    <em>Mozilla Developer Network</em>!
  </p>
{% endblock %}
```

#### Il template di base LocalLibrary

Il seguente frammento di codice verrà usato come template di base per il sito web _LocalLibrary_. Come si può notare, contiene codice HTML e definisce blocchi per `title`, `sidebar` e `content`. Sono presenti un titolo predefinito e una barra laterale predefinita con collegamenti agli elenchi di tutti i libri e gli autori, entrambi racchiusi in blocchi per poterli modificare facilmente in futuro.

> [!NOTE]
> Vengono inoltre introdotti due tag di template aggiuntivi: `url` e `load static`. Questi tag saranno spiegati nelle sezioni successive.

Creare un nuovo file **base_generic.html** in **/django-locallibrary-tutorial/catalog/templates/** e incollare nel file il codice seguente:

```django
<!doctype html>
<html lang="en">
  <head>
    {% block title %}
      <title>Local Library</title>
    {% endblock %}
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width" />
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css"
      rel="stylesheet"
      integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH"
      crossorigin="anonymous">
    <!-- Add additional CSS in static file -->
    {% load static %}
    <link rel="stylesheet" href="{% static 'css/styles.css' %}" />
  </head>
  <body>
    <div class="container-fluid">
      <div class="row">
        <div class="col-sm-2">
          {% block sidebar %}
            <ul class="sidebar-nav">
              <li><a href="{% url 'index' %}">Home</a></li>
              <li><a href="">All books</a></li>
              <li><a href="">All authors</a></li>
            </ul>
          {% endblock %}
        </div>
        <div class="col-sm-10 ">{% block content %}{% endblock %}</div>
      </div>
    </div>
  </body>
</html>
```

Il template include CSS da [Bootstrap](https://getbootstrap.com/) per migliorare il layout e la presentazione della pagina HTML. Usare Bootstrap, o un altro framework web lato client, è un modo rapido per creare una pagina attraente che venga visualizzata bene su schermi di dimensioni diverse.

Il template di base fa inoltre riferimento a un file CSS locale, **styles.css**, che fornisce uno stile aggiuntivo. Creare un file **styles.css** in **/django-locallibrary-tutorial/catalog/static/css/** e incollare il codice seguente nel file:

```css
.sidebar-nav {
  margin-top: 20px;
  padding: 0;
  list-style: none;
}
```

#### Il template index

Creare un nuovo file HTML **index.html** in **/django-locallibrary-tutorial/catalog/templates/** e incollare nel file il codice seguente.
Questo codice estende il template di base nella prima riga e poi sostituisce il blocco `content` predefinito per il template.

```django
{% extends "base_generic.html" %}

{% block content %}
  <h1>Local Library Home</h1>
  <p>
    Welcome to LocalLibrary, a website developed by
    <em>Mozilla Developer Network</em>!
  </p>
  <h2>Dynamic content</h2>
  <p>The library has the following record counts:</p>
  <ul>
    <li><strong>Books:</strong> \{{ num_books }}</li>
    <li><strong>Copies:</strong> \{{ num_instances }}</li>
    <li><strong>Copies available:</strong> \{{ num_instances_available }}</li>
    <li><strong>Authors:</strong> \{{ num_authors }}</li>
  </ul>
{% endblock %}
```

Nella sezione _Dynamic content_ vengono dichiarati segnaposto, ovvero _variabili di template_, per le informazioni della view da includere.
Le variabili sono racchiuse tra doppie parentesi graffe.

> [!NOTE]
> Le variabili di template e i tag di template, ovvero funzioni, sono facilmente riconoscibili: le variabili sono racchiuse tra doppie parentesi graffe (`\{{ num_books }}`), mentre i tag sono racchiusi tra parentesi graffe singole con segni di percentuale (`{% extends "base_generic.html" %}`).

È importante notare che le variabili hanno i nomi delle _chiavi_ passate nel dizionario `context` nella funzione `render()` della view, come mostrato nell'esempio seguente.
Le variabili saranno sostituite dai rispettivi _valori_ durante il rendering del template.

```python
context = {
    'num_books': num_books,
    'num_instances': num_instances,
    'num_instances_available': num_instances_available,
    'num_authors': num_authors,
}

return render(request, 'index.html', context=context)
```

#### Fare riferimento ai file statici nei template

È probabile che il progetto utilizzi risorse statiche, tra cui JavaScript, CSS e immagini. Poiché la posizione di questi file potrebbe non essere nota, o potrebbe cambiare, Django consente di specificarne la posizione nei template relativamente all'impostazione globale `STATIC_URL`. Il sito web scheletro predefinito imposta il valore di `STATIC_URL` su `"/static/"`, ma potrebbe essere preferibile ospitare queste risorse su una content delivery network o altrove.

All'interno del template, prima viene chiamato il tag di template `load` specificando "static", per aggiungere la libreria di template, come mostrato nell'esempio di codice seguente. È quindi possibile usare il tag di template `static` e specificare l'URL relativo del file richiesto.

```django
<!-- Add additional CSS in static file -->
{% load static %}
<link rel="stylesheet" href="{% static 'css/styles.css' %}" />
```

Un'immagine può essere aggiunta alla pagina in modo simile, ad esempio:

```django
{% load static %}
<img
  src="{% static 'images/local_library_model_uml.png' %}"
  alt="UML diagram"
  style="width:555px;height:540px;" />
```

> [!NOTE]
> Gli esempi sopra specificano dove si trovano i file, ma Django non li serve per impostazione predefinita. Il server web di sviluppo è stato configurato per servire i file modificando il mapper URL globale (**/django-locallibrary-tutorial/locallibrary/urls.py**) durante la [creazione dello scheletro del sito web](/it/docs/Learn_web_development/Extensions/Server-side/Django/skeleton_website), ma è ancora necessario abilitare il serving dei file in produzione. Questo aspetto verrà affrontato più avanti.

Per ulteriori informazioni sul lavoro con file statici, consultare [Managing static files](https://docs.djangoproject.com/en/5.0/howto/static-files/) nella documentazione Django.

#### Collegarsi agli URL

Il template di base precedente ha introdotto il tag di template `url`.

```django
<li><a href="{% url 'index' %}">Home</a></li>
```

Questo tag accetta il nome di una funzione `path()` chiamata nel file **urls.py** e i valori per eventuali argomenti che la view associata riceverà da quella funzione, e restituisce un URL che può essere utilizzato per collegarsi alla risorsa.

#### Configurare dove trovare i template

La posizione in cui Django cerca i template è specificata nell'oggetto `TEMPLATES` del file **settings.py**.
Il file **settings.py** predefinito, creato per questo tutorial, appare più o meno così:

```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

L'impostazione `'APP_DIRS': True` è la più importante, poiché indica a Django di cercare i template in una sottodirectory di ogni applicazione del progetto denominata "templates". Questo semplifica il raggruppamento dei template con l'applicazione associata, favorendone il riutilizzo.

È inoltre possibile specificare posizioni particolari in cui Django deve cercare directory usando `'DIRS': []`, ma questo non è ancora necessario.

> [!NOTE]
> Per ulteriori informazioni su come Django trova i template e sui formati di template supportati, consultare [la sezione Templates della documentazione Django](https://docs.djangoproject.com/en/5.0/topics/templates/).

## Come appare?

A questo punto sono state create tutte le risorse necessarie per visualizzare la pagina index. Avviare il server (`python3 manage.py runserver`) e aprire `http://127.0.0.1:8000/` nel browser. Se tutto è configurato correttamente, il sito dovrebbe apparire come nello screenshot seguente.

![Pagina index del sito web LocalLibrary](index_page_ok.png)

> [!NOTE]
> I collegamenti **All books** e **All authors** non funzioneranno ancora, perché i percorsi, le view e i template di tali pagine non sono definiti. Sono stati inseriti soltanto segnaposto per questi collegamenti nel template `base_generic.html`.

## Mettiti alla prova

Ecco un paio di attività per verificare la familiarità con query sui modelli, view e template.

1. Il [template di base](#il_template_di_base_locallibrary) di LocalLibrary include un blocco `title`. Sovrascrivere questo blocco nel [template index](#il_template_index) e creare un nuovo titolo per la pagina.

   > [!NOTE]
   > La sezione [Estendere i template](#estendere_i_template) spiega come creare blocchi ed estendere un blocco in un altro template.

2. Modificare la [view](#view_function-based) per generare conteggi di _generi_ e _libri_ che contengono una parola particolare, senza distinzione tra maiuscole e minuscole, e passare i risultati a `context`. Questo si realizza in modo simile alla creazione e all'uso di `num_books` e `num_instances_available`. Aggiornare quindi il [template index](#il_template_index) per includere queste variabili.

## Riepilogo

È stata appena creata la pagina iniziale del sito: una pagina HTML che visualizza diversi record dal database e collegamenti ad altre pagine che devono ancora essere create. Durante il percorso sono state apprese informazioni fondamentali sui mapper URL, le view, l'esecuzione di query sul database con i modelli, il passaggio di informazioni da una view a un template e la creazione e l'estensione di template.

Nel prossimo articolo queste conoscenze saranno usate per creare le quattro pagine rimanenti del sito web.

## Vedi anche

- [Scrivere la prima app Django, parte 3: View e Template](https://docs.djangoproject.com/en/5.0/intro/tutorial03/) (documentazione Django)
- [Dispatcher URL](https://docs.djangoproject.com/en/5.0/topics/http/urls/) (documentazione Django)
- [Funzioni view](https://docs.djangoproject.com/en/5.0/topics/http/views/) (documentazione Django)
- [Template](https://docs.djangoproject.com/en/5.0/topics/templates/) (documentazione Django)
- [Gestire i file statici](https://docs.djangoproject.com/en/5.0/howto/static-files/) (documentazione Django)
- [Funzioni di scelta rapida Django](https://docs.djangoproject.com/en/5.0/topics/http/shortcuts/#django.shortcuts.render) (documentazione Django)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Admin_site", "Learn_web_development/Extensions/Server-side/Django/Generic_views", "Learn_web_development/Extensions/Server-side/Django")}}
