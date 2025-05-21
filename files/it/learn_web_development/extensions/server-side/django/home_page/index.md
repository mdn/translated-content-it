---
title: "Tutorial di Django Parte 5: Creazione della nostra home page"
short-title: "5: Home page"
slug: Learn_web_development/Extensions/Server-side/Django/Home_page
l10n:
  sourceCommit: be1922d62a0d31e4e3441db0e943aed8df736481
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Admin_site", "Learn_web_development/Extensions/Server-side/Django/Generic_views", "Learn_web_development/Extensions/Server-side/Django")}}

Ora siamo pronti per aggiungere il codice che visualizza la nostra prima pagina completa — una home page per il sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website). La home page mostrerà il numero di record che abbiamo per ciascun tipo di modello e fornirà link di navigazione nella barra laterale alle nostre altre pagine. Nel frattempo acquisiremo esperienza pratica nella scrittura di mappe URL di base e viste, ottenendo record dal database e utilizzando template.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Leggi <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/Introduction">Introduzione a Django</a>. Completa gli argomenti dei precedenti tutorial (incluso <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/Admin_site">Tutorial di Django Parte 4: Sito admin di Django</a>).
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Impara a creare mappe URL e viste semplici (dove nessun dato è codificato nell'URL), ottenere dati dai modelli e creare template.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

Dopo aver definito i nostri modelli e creato alcuni record iniziali della biblioteca con cui lavorare, è tempo di scrivere il codice che presenta queste informazioni agli utenti. La prima cosa che dobbiamo fare è determinare quali informazioni vogliamo visualizzare nelle nostre pagine e definire gli URL da utilizzare per restituire quelle risorse. Poi creeremo un mapper URL, le viste e i template per visualizzare le pagine.

Il diagramma seguente descrive il flusso principale dei dati e i componenti richiesti quando si gestiscono richieste e risposte HTTP. Poiché abbiamo già implementato il modello, i componenti principali che creeremo sono:

- Mapper URL per inoltrare gli URL supportati (e le informazioni codificate negli URL) alle funzioni di vista appropriate.
- Funzioni di vista per ottenere i dati richiesti dai modelli, creare pagine HTML che visualizzano i dati e restituire le pagine all'utente per visualizzarle nel browser.
- Template da utilizzare quando si rendono i dati nelle viste.

![Diagramma principale di flusso dati: URL, modello, componente Vista & Template richiesti quando si gestiscono richieste e risposte HTTP in un'applicazione Django. Una richiesta HTTP colpisce un server Django e viene inoltrata al file 'urls.py' del componente URLS. La richiesta viene inoltrata alla vista appropriata. La vista può leggere e scrivere dati dal file Modelli 'models.py' che contiene il codice relativo ai modelli. La vista accede anche al componente template del file HTML. La vista restituisce la risposta all'utente.](basic-django.png)

Come vedrai nella prossima sezione, abbiamo 5 pagine da visualizzare, il che è troppa informazione da documentare in un solo articolo. Pertanto, questo articolo si concentrerà su come implementare la home page, e tratteremo le altre pagine in un articolo successivo. Questo dovrebbe darti una buona comprensione da un'estremità all'altra di come mapper URL, viste e modelli funzionano in pratica.

## Definizione degli URL delle risorse

Poiché questa versione di [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website) è essenzialmente in sola lettura per gli utenti finali, dobbiamo solo fornire una pagina d'arrivo per il sito (una home page), e pagine che _visualizzano_ liste e vista dettagliate per libri e autori.

Gli URL di cui avremo bisogno per le nostre pagine sono:

- `catalog/` — La home page (indice).
- `catalog/books/` — Un elenco di tutti i libri.
- `catalog/authors/` — Un elenco di tutti gli autori.
- `catalog/book/<id>` — La vista dettagliata per un particolare libro, con un campo chiave primaria di `<id>` (il valore predefinito). Ad esempio, l'URL per il terzo libro aggiunto all'elenco sarà `/catalog/book/3`.
- `catalog/author/<id>` — La vista dettagliata per lo specifico autore con un campo chiave primaria di `<id>`. Ad esempio, l'URL per l'11° autore aggiunto all'elenco sarà `/catalog/author/11`.

I primi tre URL restituiranno la pagina indice, l'elenco dei libri e l'elenco degli autori. Questi URL non codificano alcuna informazione aggiuntiva e le query che recuperano i dati dal database saranno sempre le stesse. Tuttavia, i risultati che le query restituiscono dipenderanno dai contenuti del database.

Invece gli ultimi due URL mostreranno informazioni dettagliate su un libro o autore specifico. Questi URL codificano l'identità dell'elemento da visualizzare (rappresentato da `<id>` sopra). Il mapper URL estrarrà le informazioni codificate e le passerà alla vista, e la vista determinerà dinamicamente quali informazioni ottenere dal database. Codificando le informazioni nell'URL utilizzeremo un unico insieme di mapper URL, vista e template per gestire tutti i libri (o autori).

> [!NOTE]
> Con Django, puoi costruire i tuoi URL come preferisci — puoi codificare le informazioni nel corpo dell'URL come mostrato sopra, o includere parametri `GET` nell'URL, ad esempio `/book/?id=6`. Qualunque approccio tu utilizzi, gli URL dovrebbero essere mantenuti puliti, logici e leggibili, come [raccomandato dal W3C](https://www.w3.org/Provider/Style/URI).
> La documentazione di Django raccomanda di codificare le informazioni nel corpo dell'URL per ottenere un design URL migliore.

Come menzionato nella panoramica, il resto di questo articolo descrive come costruire la pagina indice.

## Creazione della pagina indice

La prima pagina che creeremo è la pagina indice (`catalog/`). La pagina indice includerà un po' di HTML statico, insieme a "conteggi" generati di diversi record nel database. Per rendere possibile questo procederemo a creare una mappa URL, una vista e un template.

> [!NOTE]
> Vale la pena prestare particolare attenzione in questa sezione. La maggior parte delle informazioni si applica anche alle altre pagine che creeremo.

### Mappatura URL

Quando abbiamo creato il [sito scheletro](/it/docs/Learn_web_development/Extensions/Server-side/Django/skeleton_website), abbiamo aggiornato il file **locallibrary/urls.py** per garantire che ogni volta che viene ricevuto un URL che inizia con `catalog/`, il modulo _URLConf_ `catalog.urls` processerà la sottostringa rimanente.

Il seguente snippet di codice da **locallibrary/urls.py** include il modulo `catalog.urls`:

```python
urlpatterns += [
    path('catalog/', include('catalog.urls')),
]
```

> [!NOTE]
> Ogni volta che Django incontra la funzione di importazione [`django.urls.include()`](https://docs.djangoproject.com/en/5.0/ref/urls/#django.urls.include), divide la stringa dell'URL al carattere di fine designato e invia la sottostringa rimanente al modulo _URLConf_ incluso per un ulteriore processamento.

Abbiamo anche creato un file segnaposto per il modulo _URLConf_, chiamato **/catalog/urls.py**.
Aggiungi le seguenti righe a quel file:

```python
urlpatterns = [
    path('', views.index, name='index'),
]
```

La funzione `path()` definisce quanto segue:

- Un pattern URL, che è una stringa vuota: `''`. Discuteremo i pattern URL in dettaglio quando lavoreremo sulle altre viste.
- Una funzione vista che verrà chiamata se il pattern URL viene rilevato: `views.index`, che è la funzione chiamata `index()` nel file **views.py**.

La funzione `path()` specifica anche un parametro `name`, che è un identificatore univoco per _questa_ particolare mappatura URL. Puoi usare il nome per "invertire" il mapper, ossia per creare dinamicamente un URL che punti alla risorsa che il mapper è progettato per gestire.
Ad esempio, possiamo usare il parametro nome per collegarci alla nostra home page da qualsiasi altra pagina aggiungendo il seguente link in un template:

```django
<a href="{% url 'index' %}">Home</a>.
```

> [!NOTE]
> Possiamo codificare il link come in `<a href="/catalog/">Home</a>`), ma se cambiamo il pattern per la nostra home page, ad esempio, in `/catalog/index`) i template non linkeranno più correttamente. Usare una mappatura URL inversa è più robusto.

### Vista (basata su funzione)

Una vista è una funzione che elabora una richiesta HTTP, recupera i dati richiesti dal database, rielabora i dati in una pagina HTML usando un template HTML, e poi restituisce l'HTML generato in una risposta HTTP per visualizzare la pagina all'utente. La vista di indice segue questo modello — recupera informazioni sul numero di record `Book`, `BookInstance`, `BookInstance` disponibili e `Author` che abbiamo nel database e passa tale informazione a un template per la visualizzazione.

Apri **catalog/views.py** e nota che il file importa già la funzione scorciatoia [render()](https://docs.djangoproject.com/en/5.0/topics/http/shortcuts/#django.shortcuts.render) per generare un file HTML utilizzando un template e dati:

```python
from django.shortcuts import render

# Create your views here.
```

Incolla le seguenti righe alla fine del file:

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

La prima riga importa le classi di modello che useremo per accedere ai dati in tutte le nostre viste.

La prima parte della funzione vistas ottiene il numero di record utilizzando l'attributo `objects.all()` sulle classi di modello. Ottiene anche un elenco di oggetti `BookInstance` che hanno un valore 'a' (Disponibile) nel campo di stato. Puoi trovare maggiori informazioni su come accedere ai dati del modello nel nostro tutorial precedente [Django Tutorial Parte 3: Utilizzo dei modelli > Ricerca di record](/it/docs/Learn_web_development/Extensions/Server-side/Django/Models#searching_for_records).

Alla fine della funzione vista chiamiamo la funzione `render()` per creare una pagina HTML e restituire la pagina come risposta. Questa funzione scorciatoia racchiude alcune altre funzioni per semplificare un caso d'uso molto comune. La funzione `render()` accetta i seguenti parametri:

- l'oggetto `request` originale, che è un `HttpRequest`.
- un template HTML con segnaposto per i dati.
- una variabile `context`, che è un dizionario Python, contenente i dati da inserire nei segnaposto.

Parleremo di più sui template e della variabile `context` nella prossima sezione. Passiamo a creare il nostro template così possiamo effettivamente visualizzare qualcosa per l'utente!

### Template

Un template è un file di testo che definisce la struttura o il layout di un file (come una pagina HTML), utilizza segnaposto per rappresentare il contenuto effettivo.

Una applicazione Django creata usando **startapp** (come lo scheletro di questo esempio) cercherà i template in una sottodirectory chiamata '**templates**' delle vostre applicazioni. Ad esempio, nella vista indice che abbiamo appena aggiunto, la funzione `render()` si aspetterà di trovare il file **_index.html_** in **/django-locallibrary-tutorial/catalog/templates/** e solleverà un errore se il file non è presente.

Puoi verificarlo salvando le modifiche precedenti e accedendo a `127.0.0.1:8000` nel tuo browser - visualizzerà un messaggio di errore abbastanza intuitivo: "TemplateDoesNotExist at /catalog/", e altri dettagli.

> [!NOTE]
> In base al file delle impostazioni del tuo progetto, Django cercherà i template in diversi posti, cercando per default nelle tue applicazioni installate. Puoi trovare maggiori informazioni su come Django trova i template e quali formati di template supporta nella [sezione Template della documentazione di Django](https://docs.djangoproject.com/en/5.0/topics/templates/).

#### Estensione dei template

Il template di indice avrà bisogno di markup HTML standard per il head e body, insieme a sezioni di navigazione per collegarsi alle altre pagine del sito (che non abbiamo ancora creato), e a sezioni che visualizzano testo introduttivo e dati sui libri.

Gran parte della struttura HTML e della navigazione sarà la stessa in ogni pagina del nostro sito. Invece di duplicare il codice boilerplate su ogni pagina, puoi usare il linguaggio dei template di Django per dichiarare un template base, e poi estenderlo per sostituire solo le parti che sono diverse per ciascuna pagina specifica.

Lo snippet di codice seguente è un esempio di template base da un file **base_generic.html**.
Creeremo il template per LocalLibrary a breve.
L'esempio qui sotto include HTML comune con sezioni per un titolo, una barra laterale e contenuti principali contrassegnati con i tag di template denominati `block` e `endblock`.
Puoi lasciare i blocchi vuoti o includere contenuti predefiniti da utilizzare durante il rendering delle pagine derivate dal template.

> [!NOTE]
> I _tag di template_ sono funzioni che puoi utilizzare in un template per iterare su liste, eseguire operazioni condizionali basate sul valore di una variabile, e così via. Oltre ai tag di template, la sintassi del template ti permette di fare riferimento alle variabili che vengono passate nel template dalla vista, e usare i _filtri di template_ per formattare le variabili (ad esempio, per convertire una stringa in minuscolo).

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

Quando si definisce un template per una particolare vista, si specifica prima il template di base utilizzando il tag di template `extends` — vedi l'esempio di codice qui sotto. Poi dichiariamo quali sezioni del template vogliamo sostituire (se presenti), usando sezioni `block`/`endblock` come nel template di base.

Ad esempio, lo snippet di codice qui sotto mostra come utilizzare il tag di template `extends` e sovrascrivere il blocco `content`. L'HTML generato includerà il codice e la struttura definiti nel template di base, incluso il contenuto predefinito che hai definito nel blocco `title`, ma il nuovo blocco `content` al posto di quello predefinito.

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

Utilizzeremo il seguente snippet di codice come template base per il sito _LocalLibrary_. Come puoi vedere, contiene del codice HTML e definisce blocchi per `title`, `sidebar`, e `content`. Abbiamo un titolo di default e una barra laterale di default con link a tutte le liste dei libri e degli autori, entrambi racchiusi in blocchi per rendere facile il cambiamento in futuro.

> [!NOTE]
> Introdurremo anche due tag di template aggiuntivi: `url` e `load static`. Questi tag saranno spiegati nelle sezioni successive.

Crea un nuovo file **base_generic.html** in **/django-locallibrary-tutorial/catalog/templates/** e incolla il seguente codice nel file:

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

Il template include CSS da [Bootstrap](https://getbootstrap.com/) per migliorare il layout e la presentazione della pagina HTML. Usare Bootstrap (o un altro framework web lato client) è un modo rapido per creare una pagina attraente che viene visualizzata bene su diverse dimensioni di schermo.

Il template di base fa anche riferimento a un file CSS locale (**styles.css**) che fornisce uno stile aggiuntivo. Crea un file **styles.css** in **/django-locallibrary-tutorial/catalog/static/css/** e incolla il seguente codice nel file:

```css
.sidebar-nav {
  margin-top: 20px;
  padding: 0;
  list-style: none;
}
```

#### Il template di indice

Crea un nuovo file HTML **index.html** in **/django-locallibrary-tutorial/catalog/templates/** e incolla il seguente codice nel file.
Questo codice estende il nostro template di base nella prima riga e poi sostituisce il blocco `content` predefinito per il template.

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

Nella sezione _Contenuto dinamico_ dichiariamo segnaposto (_variabili di template_) per le informazioni dalla vista che vogliamo includere.
Le variabili sono racchiuse con doppie parentesi (handlebars).

> [!NOTE]
> Puoi facilmente riconoscere le variabili di template e i tag di template (funzioni) - le variabili sono racchiuse in doppie parentesi (`\{{ num_books }}`), e i tag sono racchiusi in parentesi singole con segni di percentuale (`{% extends "base_generic.html" %}`).

La cosa importante da notare qui è che le variabili sono chiamate con le _chiavi_ che passiamo nel dizionario `context` nella funzione `render()` della nostra vista (vedi esempio sotto).
Le variabili saranno sostituite con i loro _valori_ associati quando il template sarà renderizzato.

```python
context = {
    'num_books': num_books,
    'num_instances': num_instances,
    'num_instances_available': num_instances_available,
    'num_authors': num_authors,
}

return render(request, 'index.html', context=context)
```

#### Riferimento ai file statici nei template

Il tuo progetto è probabilmente destinato a usare risorse statiche, inclusi JavaScript, CSS e immagini. Poiché la posizione di questi file potrebbe non essere nota (o potrebbe cambiare), Django ti permette di specificare la posizione nei tuoi template in relazione all'impostazione globale `STATIC_URL`. Il valore di default `STATIC_URL` nel sito web scheletro è impostato su `"/static/"`, ma potresti scegliere di ospitarli su una rete di distribuzione di contenuti o altrove.

All'interno del template si chiama prima il tag di template `load` specificando "static" per aggiungere la libreria di template, come mostrato nell'esempio di codice seguente. Puoi quindi utilizzare il tag di template `static` e specificare l'URL relativo al file richiesto.

```django
<!-- Add additional CSS in static file -->
{% load static %}
<link rel="stylesheet" href="{% static 'css/styles.css' %}" />
```

Puoi aggiungere un'immagine nella pagina in modo simile, ad esempio:

```django
{% load static %}
<img
  src="{% static 'images/local_library_model_uml.png' %}"
  alt="UML diagram"
  style="width:555px;height:540px;" />
```

> [!NOTE]
> Gli esempi sopra specificano dove si trovano i file, ma Django non li serve per impostazione predefinita. Abbiamo configurato il server web di sviluppo per servire i file modificando il mapper URL globale (**/django-locallibrary-tutorial/locallibrary/urls.py**) quando abbiamo [creato lo scheletro del sito web](/it/docs/Learn_web_development/Extensions/Server-side/Django/skeleton_website), ma è ancora necessario consentire la fornitura di file in produzione. Ne parleremo più avanti.

Per ulteriori informazioni su come lavorare con i file statici, vedi [Gestione dei file statici](https://docs.djangoproject.com/en/5.0/howto/static-files/) nella documentazione di Django.

#### Collegamento agli URL

Il template di base sopra ha introdotto il tag di template `url`.

```django
<li><a href="{% url 'index' %}">Home</a></li>
```

Questo tag accetta il nome di una funzione `path()` chiamata nel tuo **urls.py** e i valori per qualsiasi argomento che la vista associata riceverà da quella funzione, e restituisce un URL che puoi usare per collegarti alla risorsa.

#### Configurazione del percorso dei template

Il percorso che Django utilizza per cercare i template è specificato nell'oggetto `TEMPLATES` nel file **settings.py**.
Il file **settings.py** di default (come creato per questo tutorial) appare in questo modo:

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

L'impostazione `'APP_DIRS': True`, è la più importante, poiché dice a Django di cercare template in una sottodirectory di ciascuna applicazione nel progetto, chiamata "templates" (questo rende più facile raggruppare i template con la loro applicazione associata per un facile riutilizzo).

Possiamo anche specificare posizioni specifiche per Django per cercare directory utilizzando `'DIRS': []` (ma non è necessario per ora).

> [!NOTE]
> Puoi scoprire di più su come Django trova i template e quali formati di template supporta nella [sezione Template della documentazione di Django](https://docs.djangoproject.com/en/5.0/topics/templates/).

## Che aspetto ha?

A questo punto abbiamo creato tutte le risorse necessarie per visualizzare la pagina indice. Avvia il server (`python3 manage.py runserver`) e apri `http://127.0.0.1:8000/` nel tuo browser. Se tutto è configurato correttamente, il tuo sito dovrebbe apparire come nell'immagine seguente.

![Pagina indice del sito web LocalLibrary](index_page_ok.png)

> [!NOTE]
> I link **Tutti i libri** e **Tutti gli autori** non funzioneranno ancora perché i percorsi, le viste e i template per quelle pagine non sono definiti. Abbiamo appena inserito dei segnaposto per quei link nel template `base_generic.html`.

## Metti alla prova te stesso

Ecco un paio di compiti per testare la tua familiarità con le query sui modelli, viste e template.

1. Il [template base di LocalLibrary](#il_template_base_di_locallibrary) include un blocco `title`. Sovrascrivi questo blocco nel [template di indice](#il_template_di_indice) e crea un nuovo titolo per la pagina.

   > [!NOTE]
   > La sezione [Estensione dei template](#estensione_dei_template) spiega come creare blocchi e estendere un blocco in un altro template.

2. Modifica la [vista](#view_function-based) per generare conteggi per _generi_ e _libri_ che contengono una particolare parola (senza distinzione tra maiuscole e minuscole), e passa i risultati al `context`. Lo si realizza in un modo simile alla creazione e utilizzo di `num_books` e `num_instances_available`. Quindi aggiorna il [template di indice](#il_template_di_indice) per includere queste variabili.

## Sommario

Abbiamo appena creato la home page del nostro sito — una pagina HTML che visualizza un numero di record dal database e link ad altre pagine ancora da creare. Nel frattempo abbiamo appreso informazioni fondamentali su mapper URL, viste, query al database con modelli, passaggio di informazioni a un template da una vista, e creazione ed estensione dei template.

Nel prossimo articolo costruiremo su questa conoscenza per creare le rimanenti quattro pagine del nostro sito web.

## Vedi anche

- [Scrivere la tua prima app Django, parte 3: Viste e template](https://docs.djangoproject.com/en/5.0/intro/tutorial03/) (documentazione Django)
- [Dispatcher URL](https://docs.djangoproject.com/en/5.0/topics/http/urls/) (documentazione Django)
- [Funzioni delle viste](https://docs.djangoproject.com/en/5.0/topics/http/views/) (documentazione Django)
- [Template](https://docs.djangoproject.com/en/5.0/topics/templates/) (documentazione Django)
- [Gestione dei file statici](https://docs.djangoproject.com/en/5.0/howto/static-files/) (documentazione Django)
- [Funzioni scorciatoia di Django](https://docs.djangoproject.com/en/5.0/topics/http/shortcuts/#django.shortcuts.render) (documentazione Django)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Admin_site", "Learn_web_development/Extensions/Server-side/Django/Generic_views", "Learn_web_development/Extensions/Server-side/Django")}}
