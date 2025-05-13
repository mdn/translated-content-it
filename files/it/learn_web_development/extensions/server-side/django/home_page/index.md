---
title: "Tutorial di Django Parte 5: Creare la nostra home page"
short-title: "5: Home page"
slug: Learn_web_development/Extensions/Server-side/Django/Home_page
l10n:
  sourceCommit: be1922d62a0d31e4e3441db0e943aed8df736481
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Admin_site", "Learn_web_development/Extensions/Server-side/Django/Generic_views", "Learn_web_development/Extensions/Server-side/Django")}}

Siamo ora pronti ad aggiungere il codice che visualizza la nostra prima pagina completa — una home page per il sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website). La home page mostrerà il numero di record che abbiamo per ciascun tipo di modello e fornirà collegamenti di navigazione laterale alle nostre altre pagine. Durante questo processo acquisiremo esperienza pratica nella scrittura di mappe URL di base e viste, nell'ottenere record dal database e nell'usare i template.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Leggere l'<a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/Introduction">Introduzione a Django</a>. Completare i precedenti argomenti del tutorial (incluso <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/Admin_site">Tutorial di Django Parte 4: Sito admin di Django</a>).
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare a creare mappe URL e viste semplici (dove nessun dato è codificato nell'URL), ottenere dati dai modelli e creare template.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

Dopo aver definito i nostri modelli e creato alcuni record iniziali della biblioteca con cui lavorare, è tempo di scrivere il codice che presenta tali informazioni agli utenti. La prima cosa che dobbiamo fare è determinare quali informazioni vogliamo visualizzare nelle nostre pagine e definire gli URL da utilizzare per restituire tali risorse. Quindi creeremo un mappatore URL, viste e template per visualizzare le pagine.

Il seguente diagramma descrive il flusso di dati principale e i componenti richiesti quando si gestiscono le richieste e risposte HTTP. Poiché abbiamo già implementato il modello, i principali componenti che creeremo sono:

- Mappatori URL per inoltrare gli URL supportati (e qualsiasi informazione codificata negli URL) alle funzioni di vista appropriate.
- Funzioni di vista per ottenere i dati richiesti dai modelli, creare pagine HTML che visualizzano i dati e restituire le pagine all'utente per visualizzarle nel browser.
- Template da utilizzare durante il rendering dei dati nelle viste.

![Diagramma del flusso di dati principale: componenti URL, Model, View & Template richiesti per gestire richieste e risposte HTTP in un'applicazione Django. Una richiesta HTTP colpisce un server Django, viene inoltrata al file 'urls.py' del componente URLs. La richiesta è inoltrata alla vista appropriata. La vista può leggere e scrivere dati nel file Models 'models.py' contenente il codice relativo ai modelli. La vista accede anche al componente template del file HTML. La vista restituisce la risposta all'utente.](basic-django.png)

Come vedrà nella sezione successiva, abbiamo 5 pagine da visualizzare, che è una quantità di informazioni troppo grande per essere documentata in un singolo articolo. Pertanto, questo articolo si concentrerà su come implementare la home page, e tratteremo le altre pagine in un successivo articolo. Questo dovrebbe fornirle una buona comprensione end-to-end di come funzionano i mappatori URL, le viste e i modelli nella pratica.

## Definizione degli URL delle risorse

Poiché questa versione di [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website) è essenzialmente di sola lettura per gli utenti finali, dobbiamo solo fornire una pagina di destinazione per il sito (una home page), e pagine che _visualizzano_ liste e viste dettagliate per libri e autori.

Gli URL di cui abbiamo bisogno per le nostre pagine sono:

- `catalog/` — La pagina principale (indice).
- `catalog/books/` — Una lista di tutti i libri.
- `catalog/authors/` — Una lista di tutti gli autori.
- `catalog/book/<id>` — La vista dettagliata per un particolare libro, con una chiave primaria di campo di `<id>` (il valore predefinito). Ad esempio, l'URL per il terzo libro aggiunto alla lista sarà `/catalog/book/3`.
- `catalog/author/<id>` — La vista dettagliata per l'autore specifico con un campo chiave primaria di `<id>`. Ad esempio, l'URL per l'11° autore aggiunto alla lista sarà `/catalog/author/11`.

I primi tre URL restituiranno la pagina dell'indice, la lista dei libri, e la lista degli autori. Questi URL non codificano informazioni aggiuntive, e le query che recuperano i dati dal database saranno sempre le stesse. Tuttavia, i risultati che le query restituiranno dipenderanno dal contenuto del database.

Al contrario, gli ultimi due URL visualizzeranno informazioni dettagliate su un libro o un autore specifico. Questi URL codificano l'identità dell'elemento da visualizzare (rappresentato da `<id>` sopra). Il mappatore URL estrarrà le informazioni codificate e le passerà alla vista, e la vista determinerà dinamicamente quali informazioni ottenere dal database. Codificando le informazioni nell'URL utilizzamo un unico set di una mappatura URL, una vista e un template per gestire tutti i libri (o autori).

> [!NOTE]
> Con Django, può costruire i suoi URL nel modo che ritiene più opportuno — può codificare informazioni nel corpo dell'URL come mostrato sopra, o includere parametri `GET` nell'URL, ad esempio `/book/?id=6`. Qualunque approccio utilizzi, gli URL dovrebbero essere mantenuti puliti, logici e leggibili, come [raccomandato dal W3C](https://www.w3.org/Provider/Style/URI).
> La documentazione di Django raccomanda di codificare informazioni nel corpo dell'URL per ottenere un miglior design degli URL.

Come menzionato nel riassunto, il resto di questo articolo descrive come costruire la pagina dell'indice.

## Creare la pagina dell'indice

La prima pagina che creeremo è la pagina dell'indice (`catalog/`). La pagina dell'indice includerà qualche HTML statico, insieme a "conteggi" generati di diversi record nel database. Per farlo funzionare creeremo una mappatura URL, una vista, e un template.

> [!NOTE]
> Vale la pena prestare un po' di attenzione in più in questa sezione. La maggior parte delle informazioni si applica anche alle altre pagine che creeremo.

### Mappatura URL

Quando abbiamo creato il [sito web scheletro](/it/docs/Learn_web_development/Extensions/Server-side/Django/skeleton_website), abbiamo aggiornato il file **locallibrary/urls.py** per assicurare che ogni volta che viene ricevuto un URL che inizia con `catalog/`, il modulo di _URLConf_ `catalog.urls` elaborerà la sottostringa rimanente.

Il seguente frammento di codice da **locallibrary/urls.py** include il modulo `catalog.urls`:

```python
urlpatterns += [
    path('catalog/', include('catalog.urls')),
]
```

> [!NOTE]
> Ogni volta che Django incontra la funzione di import [`django.urls.include()`](https://docs.djangoproject.com/en/5.0/ref/urls/#django.urls.include), divide la stringa URL al carattere di fine designato e invia la sottostringa rimanente al modulo di _URLConf_ incluso per ulteriore elaborazione.

Abbiamo anche creato un file segnaposto per il modulo _URLConf_, chiamato **/catalog/urls.py**.
Aggiungere le seguenti righe a quel file:

```python
urlpatterns = [
    path('', views.index, name='index'),
]
```

La funzione `path()` definisce quanto segue:

- Un pattern URL, che è una stringa vuota: `''`. Discuteremo i pattern URL in dettaglio quando lavoreremo su altre viste.
- Una funzione di vista che sarà chiamata se viene rilevato il pattern URL: `views.index`, che è la funzione denominata `index()` nel file **views.py**.

La funzione `path()` specifica anche un parametro `name`, che è un identificatore univoco per _questa_ particolare mappatura URL. Può usare il nome per "reversare" il mappatore, cioè, per creare dinamicamente un URL che punti alla risorsa che il mappatore è progettato per gestire.
Ad esempio, possiamo utilizzare il parametro name per collegarci alla nostra home page da qualsiasi altra pagina aggiungendo il seguente collegamento in un template:

```django
<a href="{% url 'index' %}">Home</a>.
```

> [!NOTE]
> Possiamo codificare direttamente il collegamento come in `<a href="/catalog/">Home</a>`), ma se cambiamo il pattern per la nostra home page, ad esempio, in `/catalog/index`) i template non si collegheranno più correttamente. Utilizzare una mappatura URL invertita è più robusto.

### Vista (basata su funzione)

Una vista è una funzione che elabora una richiesta HTTP, recupera i dati richiesti dal database, rende i dati in una pagina HTML usando un template HTML, e poi restituisce l'HTML generato in una risposta HTTP per visualizzare la pagina all'utente. La vista dell'indice segue questo modello — recupera informazioni sul numero di record `Book`, `BookInstance`, `BookInstance` disponibili e `Author` che abbiamo nel database, e passa tali informazioni a un template per la visualizzazione.

Aprire **catalog/views.py** e notare che il file importa già la funzione scorciatoia [render()](https://docs.djangoproject.com/en/5.0/topics/http/shortcuts/#django.shortcuts.render) per generare un file HTML usando un template e dati:

```python
from django.shortcuts import render

# Create your views here.
```

Incollare le seguenti righe in fondo al file:

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

La prima riga importa le classi di modelli che utilizzeremo per accedere ai dati in tutte le nostre viste.

La prima parte della funzione di vista recupera il numero di record utilizzando l'attributo `objects.all()` sulle classi di modelli. Ottiene anche una lista di oggetti `BookInstance` che hanno un valore di 'a' (Disponibile) nel campo status. Può trovare ulteriori informazioni su come accedere ai dati del modello nel nostro tutorial precedente [Django Tutorial Parte 3: Uso dei modelli > Ricerca di record](/it/docs/Learn_web_development/Extensions/Server-side/Django/Models#searching_for_records).

Alla fine della funzione di vista chiamiamo la funzione `render()` per creare una pagina HTML e restituire la pagina come risposta. Questa funzione scorciatoia avvolge un numero di altre funzioni per semplificare un caso d'uso molto comune. La funzione `render()` accetta i seguenti parametri:

- l'oggetto `request` originale, che è un `HttpRequest`.
- un template HTML con segnaposti per i dati.
- una variabile `context`, che è un dizionario Python, contenente i dati da inserire nei segnaposti.

Parleremo di più sui template e la variabile `context` nella prossima sezione. Procediamo a creare il nostro template così possiamo effettivamente visualizzare qualcosa all'utente!

### Template

Un template è un file di testo che definisce la struttura o il layout di un file (come una pagina HTML), utilizza segnaposti per rappresentare il contenuto effettivo.

Un'applicazione Django creata usando **startapp** (come lo scheletro di questo esempio) cercherà i template in una sottodirectory chiamata '**templates**' delle sue applicazioni. Ad esempio, nella vista dell'indice che abbiamo appena aggiunto, la funzione `render()` si aspetterà di trovare il file **_index.html_** in **/django-locallibrary-tutorial/catalog/templates/** e solleverà un errore se il file non è presente.

Può verificare questo salvando le modifiche precedenti e accedendo a `127.0.0.1:8000` nel browser - visualizzerà un messaggio di errore abbastanza intuitivo: "TemplateDoesNotExist at /catalog/", e altri dettagli.

> [!NOTE]
> In base al file delle impostazioni del progetto, Django cercherà i template in diversi luoghi, cercando nelle sue applicazioni installate per impostazione predefinita. Può trovare ulteriori informazioni su come Django trova i template e quali formati di template supporta nella [sezione Template della documentazione di Django](https://docs.djangoproject.com/en/5.0/topics/templates/).

#### Estendere i template

Il template dell'indice avrà bisogno di un markup HTML standard per l'intestazione e il corpo, insieme a sezioni di navigazione per collegare le altre pagine del sito (che non abbiamo ancora creato), e a sezioni che visualizzano testo introduttivo e dati dei libri.

Gran parte della struttura HTML e di navigazione sarà la stessa in ogni pagina del nostro sito. Invece di duplicare il codice boilerplate in ogni pagina, può usare il linguaggio di template di Django per dichiarare un template base e poi estenderlo per sostituire solo le parti che sono diverse per ogni pagina specifica.

Il seguente frammento di codice è un esempio di template base da un file **base_generic.html**.
Creeremo il template per LocalLibrary a breve.
L'esempio di seguito include HTML comune con sezioni per un titolo, una barra laterale, e contenuti principali contrassegnati con i blocchi `block` e `endblock` del template.
Può lasciare i blocchi vuoti, o includere contenuto predefinito da utilizzare quando si renderizzano pagine derivate dal template.

> [!NOTE]
> I _tag_ dei template sono funzioni che può usare in un template per iterare attraverso liste, eseguire operazioni condizionali in base al valore di una variabile, e così via. Oltre ai tag dei template, la sintassi del template permette di fare riferimento alle variabili che vengono passate al template dalla vista, e utilizzare _filtri di template_ per formattare le variabili (ad esempio, per convertire una stringa in minuscolo).

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

Quando si definisce un template per una particolare vista, si specifica prima il template base usando il tag `extends` del template — veda l'esempio di codice sottostante. Poi si dichiara quali sezioni del template si vogliono sostituire (se presenti), usando sezioni `block`/`endblock` come nel template base.

Ad esempio, il frammento di codice qui sotto mostra come usare il tag `extends` del template e sovrascrivere il blocco `content`. L'HTML generato includerà il codice e la struttura definiti nel template base, incluso il contenuto predefinito definito nel blocco `title`, ma il nuovo blocco `content` al posto di quello predefinito.

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

#### Il template base di LocalLibrary

Utilizzeremo il seguente frammento di codice come template base per il sito web _LocalLibrary_. Come può vedere, contiene del codice HTML e definisce blocchi per `title`, `sidebar` e `content`. Abbiamo un titolo predefinito e una barra laterale predefinita con collegamenti a liste di tutti i libri e autori, entrambi racchiusi in blocchi per essere facilmente modificabili in futuro.

> [!NOTE]
> Introduciamo anche due ulteriori tag di template: `url` e `load static`. Questi tag saranno spiegati nelle sezioni successive.

Creare un nuovo file **base_generic.html** in **/django-locallibrary-tutorial/catalog/templates/** e incollare il seguente codice nel file:

```django
<!doctype html>
<html lang="en">
  <head>
    {% block title %}
      <title>Local Library</title>
    {% endblock %}
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
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

Il template include CSS da [Bootstrap](https://getbootstrap.com/) per migliorare il layout e la presentazione della pagina HTML. Usare Bootstrap (o un altro framework web lato client) è un modo rapido per creare una pagina attraente che si visualizza bene su diverse dimensioni dello schermo.

Il template base fa anche riferimento a un file CSS locale (**styles.css**) che fornisce ulteriori stili. Creare un file **styles.css** in **/django-locallibrary-tutorial/catalog/static/css/** e incollare il seguente codice nel file:

```css
.sidebar-nav {
  margin-top: 20px;
  padding: 0;
  list-style: none;
}
```

#### Il template dell'indice

Creare un nuovo file HTML **index.html** in **/django-locallibrary-tutorial/catalog/templates/** e incollare il seguente codice nel file.
Questo codice estende il nostro template base nella prima riga e poi sostituisce il blocco `content` predefinito per il template.

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

Nella sezione _Contenuti dinamici_ dichiariamo segnaposti (_variabili di template_) per le informazioni dalla vista che vogliamo includere.
Le variabili sono racchiuse con doppi parentesi graffe (handlebars).

> [!NOTE]
> Può facilmente riconoscere variabili di template e tag di template (funzioni) - le variabili sono racchiuse in doppi parentesi graffe (`\{{ num_books }}`), e i tag sono racchiusi in parentesi singole con segni di percentuale (`{% extends "base_generic.html" %}`).

La cosa importante da notare qui è che le variabili sono nominate con le _chiavi_ che passiamo nel dizionario `context` nella funzione `render()` della nostra vista (vedi esempio sotto).
Le variabili saranno sostituite con i loro _valori_ associati quando il template viene renderizzato.

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

Il suo progetto è probabilmente destinato ad usare risorse statiche, inclusi JavaScript, CSS, e immagini. Poiché la posizione di questi file potrebbe non essere nota (o potrebbe cambiare), Django permette di specificare la posizione nei suoi template in relazione all'impostazione globale `STATIC_URL`. Il sito web scheletro predefinito imposta il valore di `STATIC_URL` a `"/static/"`, ma potrebbe decidere di ospitarli su una rete di distribuzione dei contenuti o altrove.

All'interno del template si chiama prima il tag `load` del template specificando "static" per aggiungere la libreria del template, come mostrato nel codice di esempio qui sotto. Può quindi usare il tag `static` del template e specificare l'URL relativo al file richiesto.

```django
<!-- Add additional CSS in static file -->
{% load static %}
<link rel="stylesheet" href="{% static 'css/styles.css' %}" />
```

Può aggiungere un'immagine nella pagina in modo simile, ad esempio:

```django
{% load static %}
<img
  src="{% static 'images/local_library_model_uml.png' %}"
  alt="UML diagram"
  style="width:555px;height:540px;" />
```

> [!NOTE]
> I campioni sopra specificano dove si trovano i file, ma Django non li serve per impostazione predefinita. Abbiamo configurato il server web di sviluppo per servire file modificando il mappatore URL globale (**/django-locallibrary-tutorial/locallibrary/urls.py**) quando abbiamo [creato lo scheletro del sito web](/it/docs/Learn_web_development/Extensions/Server-side/Django/skeleton_website), ma è ancora necessario abilitare il servizio di file in produzione. Ne parleremo successivamente.

Per ulteriori informazioni sul lavoro con file statici, consultare [Gestione dei file statici](https://docs.djangoproject.com/en/5.0/howto/static-files/) nella documentazione di Django.

#### Collegamenti agli URL

Il template base sopra ha introdotto il tag `url` del template.

```django
<li><a href="{% url 'index' %}">Home</a></li>
```

Questo tag accetta il nome di una funzione `path()` chiamata nel suo **urls.py** e i valori di eventuali argomenti che la vista associata riceverà da quella funzione, e restituisce un URL che può utilizzare per collegarsi alla risorsa.

#### Configurare dove trovare i template

Il luogo in cui Django cerca i template è specificato nell'oggetto `TEMPLATES` nel file **settings.py**.
Il **settings.py** predefinito (così come creato per questo tutorial) appare in questo modo:

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

L'impostazione di `'APP_DIRS': True`, è l'importante, poiché indica a Django di cercare i template in una sottodirectory di ciascuna applicazione nel progetto, chiamata "templates" (questo rende più facile raggruppare i template con l'applicazione associata per un facile riutilizzo).

Possiamo anche specificare luoghi specifici in cui Django deve cercare directory usando `'DIRS': []` (ma non è ancora necessario).

> [!NOTE]
> Può scoprire di più su come Django trova i template e quali formati di template supporta nella [sezione Template della documentazione di Django](https://docs.djangoproject.com/en/5.0/topics/templates/).

## Che aspetto ha?

A questo punto abbiamo creato tutte le risorse richieste per visualizzare la pagina dell'indice. Eseguire il server (`python3 manage.py runserver`) e aprire `http://127.0.0.1:8000/` nel browser. Se tutto è configurato correttamente, il suo sito dovrebbe apparire come la seguente schermata.

![Pagina dell'indice per il sito web LocalLibrary](index_page_ok.png)

> [!NOTE]
> I collegamenti **Tutti i libri** e **Tutti gli autori** non funzioneranno ancora perché i percorsi, le viste, e i template per quelle pagine non sono definiti. Abbiamo solo inserito segnaposti per quei collegamenti nel template `base_generic.html`.

## Mettiti alla prova

Ecco un paio di compiti per testare la sua familiarità con le query del modello, le viste e i template.

1. Il [template base](#il_template_base_di_locallibrary) di LocalLibrary include un blocco `title`. Sovrascrivere questo blocco nel [template dell'indice](#il_template_dell'indice) e creare un nuovo titolo per la pagina.

   > [!NOTE]
   > La sezione [Estendere i template](#estendere_i_template) spiega come creare blocchi e estendere un blocco in un altro template.

2. Modificare la [vista](#view_function-based) per generare conteggi per _generi_ e _libri_ che contengono una parola particolare (senza distinzione tra maiuscole e minuscole), e passare i risultati al `context`. Raggiunge questo obiettivo in modo simile a creare e usare `num_books` e `num_instances_available`. Poi aggiornare il [template dell'indice](#il_template_dell'indice) per includere queste variabili.

## Riepilogo

Abbiamo appena creato la home page per il nostro sito — una pagina HTML che visualizza un numero di record dal database e collegamenti ad altre pagine ancora da creare. Lungo il percorso abbiamo appreso informazioni fondamentali sui mappatori URL, viste, query del database con modelli, passaggio di informazioni a un template da una vista, e creazione ed estensione dei template.

Nel prossimo articolo costruiremo su questa conoscenza per creare le altre quattro pagine del nostro sito web.

## Vedi anche

- [Scrivere la sua prima app Django, parte 3: Viste e Template](https://docs.djangoproject.com/en/5.0/intro/tutorial03/) (Django docs)
- [Dispatcher URL](https://docs.djangoproject.com/en/5.0/topics/http/urls/) (Django docs)
- [Funzioni di vista](https://docs.djangoproject.com/en/5.0/topics/http/views/) (Django docs)
- [Template](https://docs.djangoproject.com/en/5.0/topics/templates/) (Django docs)
- [Gestione dei file statici](https://docs.djangoproject.com/en/5.0/howto/static-files/) (Django docs)
- [Funzioni scorciatoia di Django](https://docs.djangoproject.com/en/5.0/topics/http/shortcuts/#django.shortcuts.render) (Django docs)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Admin_site", "Learn_web_development/Extensions/Server-side/Django/Generic_views", "Learn_web_development/Extensions/Server-side/Django")}}
