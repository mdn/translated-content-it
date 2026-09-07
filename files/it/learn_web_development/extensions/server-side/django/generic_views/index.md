---
title: "Tutorial Django Parte 6: viste generiche di elenco e dettaglio"
short-title: "6: viste generiche di elenco e dettaglio"
slug: Learn_web_development/Extensions/Server-side/Django/Generic_views
l10n:
  sourceCommit: a4fcf79b60471db6f148fa4ba36f2cdeafbbeb70
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Home_page", "Learn_web_development/Extensions/Server-side/Django/Sessions", "Learn_web_development/Extensions/Server-side/Django")}}

Questo tutorial estende il sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website), aggiungendo pagine di elenco e dettaglio per libri e autori. Verranno illustrate le viste generiche basate su classi e verrà mostrato come possano ridurre la quantità di codice da scrivere per i casi d'uso comuni. Verrà inoltre approfondita la gestione degli URL, mostrando come eseguire il pattern matching di base.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Completare tutti gli argomenti dei tutorial precedenti, incluso <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/Home_page">Tutorial Django Parte 5: creazione della home page</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere dove e come utilizzare le viste generiche basate su classi e come estrarre pattern dagli URL e passare le informazioni alle viste.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

In questo tutorial verrà completata la prima versione del sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website), aggiungendo pagine di elenco e dettaglio per libri e autori (o, più precisamente, verrà mostrato come implementare le pagine dei libri e sarà richiesto di creare autonomamente quelle degli autori).

Il processo è simile alla creazione della pagina indice, mostrata nel tutorial precedente. Sarà comunque necessario creare mappe URL, viste e template. La differenza principale è che, per le pagine di dettaglio, si presenta l'ulteriore sfida di estrarre informazioni dai pattern nell'URL e passarle alla vista. Per queste pagine verrà illustrato un tipo di vista completamente diverso: le viste generiche di elenco e dettaglio basate su classi. Queste possono ridurre significativamente la quantità di codice delle viste necessaria, rendendole più semplici da scrivere e mantenere.

La parte finale del tutorial mostrerà come paginare i dati quando si utilizzano viste generiche di elenco basate su classi.

## Pagina dell'elenco dei libri

La pagina dell'elenco dei libri mostrerà un elenco di tutti i record dei libri disponibili nella pagina, accessibile tramite l'URL: `catalog/books/`. La pagina mostrerà un titolo e un autore per ogni record, con il titolo come collegamento ipertestuale alla pagina di dettaglio del libro associato. La pagina avrà la stessa struttura e navigazione di tutte le altre pagine del sito e potrà quindi estendere il template di base (**base_generic.html**) creato nel tutorial precedente.

### Mappatura URL

Aprire **/catalog/urls.py** e copiare la riga che imposta il percorso per `'books/'`, come mostrato di seguito.
Come per la pagina indice, questa funzione `path()` definisce un pattern da confrontare con l'URL (**'books/'**), una funzione di vista che verrà chiamata se l'URL corrisponde (`views.BookListView.as_view()`) e un nome per questa particolare mappatura.

```python
urlpatterns = [
    path('', views.index, name='index'),
    path('books/', views.BookListView.as_view(), name='books'),
]
```

Come illustrato nel tutorial precedente, l'URL deve avere già corrisposto a `/catalog`, quindi la vista verrà effettivamente chiamata per l'URL: `/catalog/books/`.

La funzione di vista ha un formato diverso rispetto a prima: questo perché la vista verrà effettivamente implementata come classe. Verrà ereditata una funzione di vista generica esistente che esegue già gran parte delle operazioni richieste, anziché scriverne una nuova da zero.

Per le viste Django basate su classi, si accede a una funzione di vista appropriata chiamando il metodo della classe `as_view()`. Questo metodo svolge tutto il lavoro necessario per creare un'istanza della classe e assicurare che vengano chiamati i corretti metodi handler per le richieste HTTP in arrivo.

### Vista (basata su classi)

Sarebbe abbastanza semplice scrivere la vista dell'elenco dei libri come funzione normale, proprio come la precedente vista indice, che eseguirebbe una query sul database per tutti i libri e poi chiamerebbe `render()` per passare l'elenco a un template specificato. Verrà invece utilizzata una vista generica di elenco basata su classi (`ListView`), ovvero una classe che eredita da una vista esistente. Poiché la vista generica implementa già la maggior parte delle funzionalità necessarie e segue le best practice di Django, sarà possibile creare una vista elenco più robusta con meno codice, meno ripetizioni e, in definitiva, meno manutenzione.

Aprire **catalog/views.py** e copiare il codice seguente in fondo al file:

```python
from django.views import generic

class BookListView(generic.ListView):
    model = Book
```

È tutto! La vista generica eseguirà una query sul database per ottenere tutti i record per il modello specificato (`Book`), quindi eseguirà il rendering di un template situato in **/django-locallibrary-tutorial/catalog/templates/catalog/book_list.html** (che verrà creato di seguito). All'interno del template è possibile accedere all'elenco dei libri tramite la variabile di template denominata `object_list` OPPURE `book_list` (ovvero, in modo generico, `<the model name>_list`).

> [!NOTE]
> Questo percorso apparentemente insolito per la posizione del template non è un errore di stampa: le viste generiche cercano i template in `/application_name/the_model_name_list.html` (`catalog/book_list.html` in questo caso) all'interno della directory `/application_name/templates/` dell'applicazione (`/catalog/templates/`).

È possibile aggiungere attributi per modificare il comportamento predefinito descritto sopra. Ad esempio, è possibile specificare un altro file di template se sono necessarie più viste che utilizzano lo stesso modello, oppure potrebbe essere opportuno utilizzare un nome diverso per la variabile di template se `book_list` non è intuitivo per il particolare caso d'uso del template. Probabilmente la variazione più utile consiste nel modificare/filtrare il sottoinsieme di risultati restituiti: anziché elencare tutti i libri, si potrebbero elencare i primi 5 libri letti da altri utenti.

```python
class BookListView(generic.ListView):
    model = Book
    context_object_name = 'book_list'   # your own name for the list as a template variable
    queryset = Book.objects.filter(title__icontains='war')[:5] # Get 5 books containing the title war
    template_name = 'books/my_arbitrary_template_name_list.html'  # Specify your own template name/location
```

#### Sovrascrittura dei metodi nelle viste basate su classi

Sebbene non sia necessario farlo in questo caso, è anche possibile sovrascrivere alcuni metodi della classe.

Ad esempio, è possibile sovrascrivere il metodo `get_queryset()` per modificare l'elenco dei record restituiti. Questo approccio è più flessibile rispetto alla sola impostazione dell'attributo `queryset`, come nel frammento di codice precedente, anche se in questo caso non offre alcun vantaggio concreto:

```python
class BookListView(generic.ListView):
    model = Book

    def get_queryset(self):
        return Book.objects.filter(title__icontains='war')[:5] # Get 5 books containing the title war
```

Potrebbe inoltre essere necessario sovrascrivere `get_context_data()` per passare al template ulteriori variabili di contesto, ad esempio l'elenco dei libri viene passato per impostazione predefinita. Il frammento seguente mostra come aggiungere al contesto una variabile denominata `some_data`, che sarà quindi disponibile come variabile di template.

```python
class BookListView(generic.ListView):
    model = Book

    def get_context_data(self, **kwargs):
        # Call the base implementation first to get the context
        context = super(BookListView, self).get_context_data(**kwargs)
        # Create any data and add it to the context
        context['some_data'] = 'This is just some data'
        return context
```

In questo caso, è importante seguire il pattern usato sopra:

- Prima ottenere il contesto esistente dalla superclasse.
- Poi aggiungere le nuove informazioni di contesto.
- Infine restituire il nuovo contesto aggiornato.

> [!NOTE]
> Consultare [Built-in class-based generic views](https://docs.djangoproject.com/en/5.0/topics/class-based-views/generic-display/) (documentazione di Django) per molti altri esempi delle operazioni possibili.

### Creazione del template della vista elenco

Creare il file HTML **/django-locallibrary-tutorial/catalog/templates/catalog/book_list.html** e copiarvi il testo seguente. Come illustrato sopra, questo è il file di template predefinito previsto dalla vista generica di elenco basata su classi, per un modello denominato `Book` in un'applicazione denominata `catalog`.

I template per le viste generiche sono come qualsiasi altro template, anche se naturalmente il contesto/le informazioni passati al template possono differire.
Come per il template dell'_indice_, si estende il template di base nella prima riga e poi si sostituisce il blocco denominato `content`.

```django
{% extends "base_generic.html" %}

{% block content %}
  <h1>Book List</h1>
  {% if book_list %}
    <ul>
      {% for book in book_list %}
      <li>
        <a href="\{{ book.get_absolute_url }}">\{{ book.title }}</a>
        (\{{book.author}})
      </li>
      {% endfor %}
    </ul>
  {% else %}
    <p>There are no books in the library.</p>
  {% endif %}
{% endblock %}
```

La vista passa per impostazione predefinita il contesto, ovvero l'elenco di libri, mediante gli alias `object_list` e `book_list`; entrambi funzionano.

#### Esecuzione condizionale

Vengono utilizzati i tag di template [`if`](https://docs.djangoproject.com/en/5.0/ref/templates/builtins/#if), `else` e `endif` per verificare se `book_list` è stato definito e non è vuoto.
Se `book_list` è vuoto, la clausola `else` mostra un testo che spiega che non ci sono libri da elencare.
Se `book_list` non è vuoto, viene iterato l'elenco dei libri.

```django
{% if book_list %}
  <!-- code here to list the books -->
{% else %}
  <p>There are no books in the library.</p>
{% endif %}
```

La condizione precedente verifica un solo caso, ma è possibile testare ulteriori condizioni utilizzando il tag di template `elif`, ad esempio `{% elif var2 %}`.
Per ulteriori informazioni sugli operatori condizionali, vedere: [if](https://docs.djangoproject.com/en/5.0/ref/templates/builtins/#if), [ifequal/ifnotequal](https://docs.djangoproject.com/en/5.0/ref/templates/builtins/#ifequal-and-ifnotequal) e [ifchanged](https://docs.djangoproject.com/en/5.0/ref/templates/builtins/#ifchanged) in [Built-in template tags and filters](https://docs.djangoproject.com/en/5.0/ref/templates/builtins/) (documentazione di Django).

#### Cicli For

Il template utilizza i tag di template [for](https://docs.djangoproject.com/en/5.0/ref/templates/builtins/#for) e `endfor` per eseguire un ciclo nell'elenco dei libri, come mostrato di seguito.
Ogni iterazione popola la variabile di template `book` con le informazioni relative all'elemento corrente dell'elenco.

```django
{% for book in book_list %}
  <li><!-- code here get information from each book item --></li>
{% endfor %}
```

È anche possibile utilizzare il tag di template `{% empty %}` per definire ciò che avviene se l'elenco dei libri è vuoto, sebbene il template utilizzi invece una condizione:

```django
<ul>
  {% for book in book_list %}
    <li><!-- code here get information from each book item --></li>
  {% empty %}
    <p>There are no books in the library.</p>
  {% endfor %}
</ul>
```

Anche se qui non vengono utilizzate, all'interno del ciclo Django crea altre variabili che possono essere usate per tenere traccia dell'iterazione.
Ad esempio, è possibile testare la variabile `forloop.last` per eseguire elaborazioni condizionali durante l'ultima esecuzione del ciclo.

#### Accesso alle variabili

Il codice all'interno del ciclo crea un elemento di elenco per ciascun libro, mostrando sia il titolo, come link alla vista di dettaglio ancora da creare, sia l'autore.

```django
<a href="\{{ book.get_absolute_url }}">\{{ book.title }}</a> (\{{book.author}})
```

Si accede ai _campi_ del record del libro associato tramite la "dot notation", ad esempio `book.title` e `book.author`, dove il testo successivo all'elemento `book` è il nome del campo, come definito nel modello.

È inoltre possibile chiamare _funzioni_ nel modello dall'interno del template. In questo caso viene chiamata `Book.get_absolute_url()` per ottenere un URL che può essere utilizzato per mostrare il record di dettaglio associato. Questo funziona a condizione che la funzione non abbia argomenti, poiché non è possibile passare argomenti.

> [!NOTE]
> È necessario prestare un po' di attenzione agli "effetti collaterali" quando si chiamano funzioni nei template. Qui viene semplicemente ottenuto un URL da visualizzare, ma una funzione può fare praticamente qualsiasi cosa: non sarebbe desiderabile eliminare il database, per esempio, soltanto eseguendo il rendering del template.

#### Aggiornamento del template di base

Aprire il template di base (**/django-locallibrary-tutorial/catalog/templates/_base_generic.html_**) e inserire **{% url 'books' %}** nel link URL per **All books**, come mostrato di seguito. In questo modo verrà abilitato il link in tutte le pagine, ora che è stata creata la mappatura URL "books".

```django
<li><a href="{% url 'index' %}">Home</a></li>
<li><a href="{% url 'books' %}">All books</a></li>
<li><a href="">All authors</a></li>
```

### Come appare?

Non sarà ancora possibile creare l'elenco dei libri, perché manca ancora una dipendenza: la mappa URL per le pagine di dettaglio dei libri, necessaria per creare collegamenti ipertestuali ai singoli libri. Entrambe le viste elenco e dettaglio verranno mostrate dopo la sezione successiva.

## Pagina di dettaglio del libro

La pagina di dettaglio del libro mostrerà le informazioni su un libro specifico, accessibile utilizzando l'URL `catalog/book/<id>` (dove `<id>` è la chiave primaria del libro). Oltre ai campi del modello `Book` — autore, riepilogo, ISBN, lingua e genere — verranno elencati anche i dettagli delle copie disponibili (`BookInstances`), compresi stato, data di restituzione prevista, edizione e id. Questo permetterà ai lettori non solo di conoscere il libro, ma anche di verificare se e quando è disponibile.

### Mappatura URL

Aprire **/catalog/urls.py** e aggiungere il percorso denominato '**book-detail**' mostrato di seguito.
Questa funzione `path()` definisce un pattern, una vista di dettaglio generica associata basata su classi e un nome.

```python
urlpatterns = [
    path('', views.index, name='index'),
    path('books/', views.BookListView.as_view(), name='books'),
    path('book/<int:pk>', views.BookDetailView.as_view(), name='book-detail'),
]
```

Per il percorso _book-detail_, il pattern URL utilizza una sintassi speciale per catturare l'id specifico del libro che si desidera visualizzare.
La sintassi è molto semplice: le parentesi angolari definiscono la parte dell'URL da catturare e racchiudono il nome della variabile che la vista può utilizzare per accedere ai dati catturati.
Ad esempio, **\<something>** cattura il pattern contrassegnato e passa il valore alla vista come variabile "something". Facoltativamente, è possibile anteporre al nome della variabile una [specifica di convertitore](https://docs.djangoproject.com/en/5.0/topics/http/urls/#path-converters) che definisce il tipo di dati: int, str, slug, uuid, path.

In questo caso viene utilizzato `'<int:pk>'` per catturare l'id del libro, che deve essere una stringa appositamente formattata, e passarlo alla vista come parametro denominato `pk`, abbreviazione di primary key. Questo è l'id utilizzato per memorizzare in modo univoco il libro nel database, come definito nel modello Book.

> [!NOTE]
> Come illustrato in precedenza, l'URL corrispondente è in realtà `catalog/book/<digits>`, poiché ci si trova nell'applicazione **catalog** e `/catalog/` è implicito.

> [!WARNING]
> La vista generica di dettaglio basata su classi _si aspetta_ di ricevere un parametro denominato **pk**. Se viene scritta una funzione di vista personalizzata, è possibile usare qualsiasi nome di parametro oppure passare l'informazione in un argomento senza nome.

#### Introduzione al confronto avanzato dei percorsi/espressioni regolari

> [!NOTE]
> Questa sezione non è necessaria per completare il tutorial. Viene fornita perché conoscere questa opzione sarà probabilmente utile in futuro con Django.

Il pattern matching fornito da `path()` è semplice e utile nei casi molto comuni in cui è necessario catturare _qualsiasi_ stringa o intero. Se è necessario un filtraggio più preciso, ad esempio per filtrare solo stringhe con un determinato numero di caratteri, è possibile utilizzare il metodo [re_path()](https://docs.djangoproject.com/en/5.0/ref/urls/#django.urls.re_path).

Questo metodo viene utilizzato proprio come `path()`, tranne per il fatto che consente di specificare un pattern utilizzando un'[espressione regolare](https://docs.python.org/3/library/re.html). Ad esempio, il percorso precedente avrebbe potuto essere scritto come mostrato di seguito:

```python
re_path(r'^book/(?P<pk>\d+)$', views.BookDetailView.as_view(), name='book-detail'),
```

Le _espressioni regolari_ sono uno strumento incredibilmente potente per la mappatura dei pattern. Francamente, sono piuttosto poco intuitive e possono intimidire i principianti. Di seguito viene proposta una brevissima introduzione.

La prima cosa da sapere è che le espressioni regolari dovrebbero generalmente essere dichiarate utilizzando la sintassi raw string literal, ovvero racchiuse come mostrato qui: **r'\<your regular expression text goes here>'**.

Le principali parti della sintassi da conoscere per dichiarare le corrispondenze dei pattern sono:

<table class="standard-table no-markdown">
  <thead>
    <tr>
      <th scope="col">Simbolo</th>
      <th scope="col">Significato</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>^</td>
      <td>Corrisponde all'inizio del testo</td>
    </tr>
    <tr>
      <td>$</td>
      <td>Corrisponde alla fine del testo</td>
    </tr>
    <tr>
      <td>\d</td>
      <td>Corrisponde a una cifra (0, 1, 2, … 9)</td>
    </tr>
    <tr>
      <td>\w</td>
      <td>
        Corrisponde a un carattere di parola, ad esempio qualsiasi carattere maiuscolo o minuscolo dell'alfabeto, una cifra o il carattere underscore (_)
      </td>
    </tr>
    <tr>
      <td>+</td>
      <td>
        Corrisponde a uno o più caratteri precedenti. Ad esempio, per trovare una o più cifre si usa <code>\d+</code>. Per trovare uno o più caratteri "a", si può usare <code>a+</code>
      </td>
    </tr>
    <tr>
      <td>*</td>
      <td>
        Corrisponde a zero o più caratteri precedenti. Ad esempio, per trovare nulla oppure una parola si può usare <code>\w*</code>
      </td>
    </tr>
    <tr>
      <td>( )</td>
      <td>
        Cattura la parte del pattern compresa tra parentesi. Tutti i valori catturati saranno passati alla vista come parametri senza nome; se vengono catturati più pattern, i parametri associati saranno forniti nell'ordine in cui le catture sono state dichiarate.
      </td>
    </tr>
    <tr>
      <td>(?P&#x3C;<em>name</em>>...)</td>
      <td>
        Cattura il pattern, indicato da ..., come variabile con nome, in questo caso "name". I valori catturati vengono passati alla vista con il nome specificato. La vista deve quindi dichiarare un parametro con lo stesso nome.
      </td>
    </tr>
    <tr>
      <td>[ ]</td>
      <td>
        Corrisponde a un carattere nell'insieme. Ad esempio, [abc] corrisponderà a 'a', 'b' o 'c'. [-\w] corrisponderà al carattere '-' oppure a qualsiasi carattere di parola.
      </td>
    </tr>
  </tbody>
</table>

La maggior parte degli altri caratteri può essere interpretata letteralmente.

Consideriamo alcuni esempi reali di pattern:

<table class="standard-table">
  <thead>
    <tr>
      <th scope="col">Pattern</th>
      <th scope="col">Descrizione</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>r'^book/(?P&#x3C;pk>\d+)$'</strong></td>
      <td>
        <p>
          Questa è l'espressione regolare utilizzata nella mappa URL. Corrisponde a una stringa che ha
          <code>book/</code> all'inizio della riga (<strong>^book/</strong>),
          seguita da una o più cifre (<code>\d+</code>), e che poi termina, senza caratteri non numerici prima dell'indicatore di fine riga.
        </p>
        <p>
          Cattura inoltre tutte le cifre <strong>(?P&#x3C;pk>\d+)</strong> e
          le passa alla vista in un parametro denominato 'pk'.
          <strong>I valori catturati vengono sempre passati come stringa.</strong>
        </p>
        <p>
          Ad esempio, questo corrisponderebbe a <code>book/1234</code> e invierebbe alla vista una
          variabile <code>pk='1234'</code>.
        </p>
      </td>
    </tr>
    <tr>
      <td><strong>r'^book/(\d+)$'</strong></td>
      <td>
        Questo corrisponde agli stessi URL del caso precedente. Le informazioni catturate sarebbero inviate alla vista come argomento senza nome.
      </td>
    </tr>
    <tr>
      <td><strong>r'^book/(?P&#x3C;stub>[-\w]+)$'</strong></td>
      <td>
        <p>
          Questo corrisponde a una stringa che ha <code>book/</code> all'inizio della riga
          (<strong>^book/</strong>), seguita da uno o più caratteri che sono
          <em>o</em> un '-' o un carattere di parola
          (<strong>[-\w]+</strong>), e che poi termina. Cattura inoltre questo insieme di caratteri e lo passa alla vista in un parametro denominato 'stub'.
        </p>
        <p>
          Questo è un pattern abbastanza tipico per uno "stub". Gli stub sono chiavi primarie basate su parole e compatibili con gli URL. Uno stub può essere utilizzato per rendere più informativo l'URL di un libro. Ad esempio,
          <code>/catalog/book/the-secret-garden</code> anziché
          <code>/catalog/book/33</code>.
        </p>
      </td>
    </tr>
  </tbody>
</table>

È possibile catturare più pattern in una singola corrispondenza e quindi codificare molte informazioni diverse in un URL.

> [!NOTE]
> Come esercizio, si può considerare come codificare un URL per elencare tutti i libri pubblicati in un particolare anno, mese e giorno, e quale espressione regolare potrebbe essere utilizzata per trovarlo.

#### Passaggio di opzioni aggiuntive nelle mappe URL

Una funzionalità non utilizzata qui, ma che può risultare utile, è la possibilità di passare alla vista un [dizionario contenente opzioni aggiuntive](https://docs.djangoproject.com/en/5.0/topics/http/urls/#views-extra-options), utilizzando il terzo argomento senza nome della funzione `path()`. Questo approccio può essere utile se si desidera utilizzare la stessa vista per più risorse e passare dati per configurarne il comportamento in ciascun caso.

Ad esempio, dato il percorso mostrato di seguito, per una richiesta a `/my-url/halibut/` Django chiamerà `views.my_view(request, fish='halibut', my_template_name='some_path')`.

```python
path('my-url/<fish>', views.my_view, {'my_template_name': 'some_path'}, name='aurl'),
```

> [!NOTE]
> Sia i pattern catturati con nome sia le opzioni del dizionario vengono passati alla vista come argomenti _con nome_. Se viene utilizzato lo **stesso nome** sia per un pattern di cattura sia per una chiave del dizionario, verrà utilizzata l'opzione del dizionario.

### Vista (basata su classi)

Aprire **catalog/views.py** e copiare il codice seguente in fondo al file:

```python
class BookDetailView(generic.DetailView):
    model = Book
```

È tutto! Ora basta creare un template chiamato **/django-locallibrary-tutorial/catalog/templates/catalog/book_detail.html** e la vista gli passerà le informazioni del database per lo specifico record `Book` estratto dalla mappa URL. All'interno del template è possibile accedere ai dettagli del libro tramite la variabile di template denominata `object` OPPURE `book` (ovvero, in modo generico, `the_model_name`).

Se necessario, è possibile modificare il template utilizzato e il nome dell'oggetto di contesto usato per fare riferimento al libro nel template. È anche possibile sovrascrivere metodi per, ad esempio, aggiungere ulteriori informazioni al contesto.

#### Cosa succede se il record non esiste?

Se un record richiesto non esiste, la vista generica di dettaglio basata su classi solleverà automaticamente un'eccezione `Http404`. In produzione, verrà automaticamente mostrata una pagina appropriata "risorsa non trovata", che può essere personalizzata se desiderato.

Per fornire un'idea del funzionamento, il frammento di codice seguente mostra come implementare la vista basata su classi come funzione se **non** venisse utilizzata la vista generica di dettaglio basata su classi.

```python
def book_detail_view(request, primary_key):
    try:
        book = Book.objects.get(pk=primary_key)
    except Book.DoesNotExist:
        raise Http404('Book does not exist')

    return render(request, 'catalog/book_detail.html', context={'book': book})
```

La vista tenta innanzitutto di ottenere dal modello il record del libro specifico. Se questa operazione fallisce, la vista deve sollevare un'eccezione `Http404` per indicare che il libro "non è stato trovato". Il passaggio finale è poi, come di consueto, chiamare `render()` con il nome del template e i dati del libro nel parametro `context`, sotto forma di dizionario.

Un altro modo per farlo, se non venisse utilizzata una vista generica, sarebbe chiamare la funzione `get_object_or_404()`.
Questa è una scorciatoia per sollevare un'eccezione `Http404` se il record non viene trovato.

```python
from django.shortcuts import get_object_or_404

def book_detail_view(request, primary_key):
    book = get_object_or_404(Book, pk=primary_key)
    return render(request, 'catalog/book_detail.html', context={'book': book})
```

### Creazione del template della vista di dettaglio

Creare il file HTML **/django-locallibrary-tutorial/catalog/templates/catalog/book_detail.html** e assegnargli il contenuto seguente. Come illustrato sopra, questo è il nome di file del template predefinito previsto dalla vista generica di _dettaglio_ basata su classi, per un modello denominato `Book` in un'applicazione denominata `catalog`.

```django
{% extends "base_generic.html" %}

{% block content %}
  <h1>Title: \{{ book.title }}</h1>

  <p><strong>Author:</strong> <a href="">\{{ book.author }}</a></p>
  <!-- author detail link not yet defined -->
  <p><strong>Summary:</strong> \{{ book.summary }}</p>
  <p><strong>ISBN:</strong> \{{ book.isbn }}</p>
  <p><strong>Language:</strong> \{{ book.language }}</p>
  <p><strong>Genre:</strong> \{{ book.genre.all|join:", " }}</p>

  <div style="margin-left:20px;margin-top:20px">
    <h4>Copies</h4>

    {% for copy in book.bookinstance_set.all %}
      <hr />
      <p
        class="{% if copy.status == 'a' %}text-success{% elif copy.status == 'm' %}text-danger{% else %}text-warning{% endif %}">
        \{{ copy.get_status_display }}
      </p>
      {% if copy.status != 'a' %}
        <p><strong>Due to be returned:</strong> \{{ copy.due_back }}</p>
      {% endif %}
      <p><strong>Imprint:</strong> \{{ copy.imprint }}</p>
      <p class="text-muted"><strong>Id:</strong> \{{ copy.id }}</p>
    {% endfor %}
  </div>
{% endblock %}
```

> [!NOTE]
> Il link dell'autore nel template precedente ha un URL vuoto perché non è stata ancora creata una pagina di dettaglio dell'autore a cui collegarsi.
> Una volta che la pagina di dettaglio esiste, il relativo URL può essere ottenuto con uno di questi due approcci:
>
> - Utilizzare il tag di template `url` per invertire l'URL 'author-detail', definito nella mappa URL, passando l'istanza dell'autore del libro:
>
>   ```django
>   <a href="{% url 'author-detail' book.author.pk %}">\{{ book.author }}</a>
>   ```
>
> - Chiamare il metodo `get_absolute_url()` del modello dell'autore, che esegue la stessa operazione di inversione:
>
>   ```django
>   <a href="\{{ book.author.get_absolute_url }}">\{{ book.author }}</a>
>   ```
>
> Sebbene entrambi i metodi facciano di fatto la stessa cosa, è preferibile `get_absolute_url()` perché consente di scrivere codice più coerente e manutenibile: ogni modifica deve essere eseguita in un solo punto, ovvero nel modello dell'autore.

Benché leggermente più grande, quasi tutto in questo template è stato descritto in precedenza:

- Si estende il template di base e si sovrascrive il blocco "content".
- Si utilizza l'elaborazione condizionale per stabilire se mostrare o meno contenuti specifici.
- Si utilizzano cicli `for` per scorrere elenchi di oggetti.
- Si accede ai campi di contesto usando la dot notation; poiché è stata usata la vista generica di dettaglio, il contesto è denominato `book`, ma è possibile usare anche `object`.

La prima cosa interessante non vista prima è la funzione `book.bookinstance_set.all()`. Questo metodo viene costruito "automagicamente" da Django per restituire l'insieme dei record `BookInstance` associati a un particolare `Book`.

```django
{% for copy in book.bookinstance_set.all %}
  <!-- code to iterate across each copy/instance of a book -->
{% endfor %}
```

Questo metodo è necessario perché un campo `ForeignKey`, uno-a-molti, viene dichiarato solo sul lato "molti" della relazione, ovvero `BookInstance`. Poiché non viene eseguita alcuna operazione per dichiarare la relazione nell'altro modello, quello "uno", il modello `Book` non dispone di alcun campo per ottenere l'insieme dei record associati. Per risolvere il problema, Django costruisce un'apposita funzione di "reverse lookup" utilizzabile a questo scopo. Il nome della funzione viene costruito convertendo in minuscolo il nome del modello in cui è stato dichiarato `ForeignKey`, seguito da `_set`; pertanto la funzione creata in `Book` è `bookinstance_set()`.

> [!NOTE]
> Qui viene utilizzato `all()` per ottenere tutti i record, ovvero il comportamento predefinito. Sebbene nel codice si possa utilizzare il metodo `filter()` per ottenere un sottoinsieme di record, non è possibile farlo direttamente nei template perché non è possibile specificare argomenti alle funzioni.
>
> Occorre inoltre tenere presente che, se non si definisce un ordinamento nella vista basata su classi o nel modello, si vedranno anche errori dal server di sviluppo come questo:
>
> ```plain
> [29/May/2017 18:37:53] "GET /catalog/books/?page=1 HTTP/1.1" 200 1637
> /foo/local_library/venv/lib/python3.5/site-packages/django/views/generic/list.py:99: UnorderedObjectListWarning: Pagination may yield inconsistent results with an unordered object_list: <QuerySet [<Author: Ortiz, David>, <Author: H. McRaven, William>, <Author: Leigh, Melinda>]>
>   allow_empty_first_page=allow_empty_first_page, **kwargs)
> ```
>
> Questo avviene perché l'[oggetto paginator](https://docs.djangoproject.com/en/5.0/topics/pagination/#paginator-objects) si aspetta che venga eseguito un `ORDER BY` sul database sottostante. Senza di esso, non può essere sicuro che i record restituiti siano effettivamente nell'ordine corretto.
>
> Questo tutorial non ha ancora trattato la **Paginazione**, ma poiché non è possibile utilizzare `sort_by()` passando un parametro, come anche `filter()` descritto sopra, sarà necessario scegliere tra tre opzioni:
>
> 1. Aggiungere un `ordering` all'interno di una dichiarazione `class Meta` nel modello.
> 2. Aggiungere un attributo `queryset` nella vista basata su classi personalizzata, specificando un `order_by()`.
> 3. Aggiungere un metodo `get_queryset` alla vista basata su classi personalizzata e specificare anche `order_by()`.
>
> Se si decide di utilizzare una `class Meta` per il modello `Author`, probabilmente meno flessibile della personalizzazione della vista basata su classi ma abbastanza semplice, si otterrà qualcosa di simile:
>
> ```python
> class Author(models.Model):
>     first_name = models.CharField(max_length=100)
>     last_name = models.CharField(max_length=100)
>     date_of_birth = models.DateField(null=True, blank=True)
>     date_of_death = models.DateField('Died', null=True, blank=True)
>
>     def get_absolute_url(self):
>         return reverse('author-detail', args=[str(self.id)])
>
>     def __str__(self):
>         return f'{self.last_name}, {self.first_name}'
>
>     class Meta:
>         ordering = ['last_name']
> ```
>
> Naturalmente, il campo non deve necessariamente essere `last_name`: può essere qualsiasi altro campo.
>
> Infine, è opportuno ordinare in base a un attributo/colonna che abbia effettivamente un indice, univoco o meno, nel database per evitare problemi di prestazioni. Naturalmente, qui non sarà necessario, poiché ci sono probabilmente troppo pochi libri e utenti perché sia rilevante, ma è un aspetto da tenere presente per progetti futuri.

La seconda cosa interessante, e non ovvia, nel template è il punto in cui viene mostrato il testo dello stato per ciascuna istanza del libro, ad esempio "available", "maintenance" e così via.
I lettori più attenti noteranno che il metodo `BookInstance.get_status_display()` utilizzato per ottenere il testo dello stato non appare altrove nel codice.

```django
 <p class="{% if copy.status == 'a' %}text-success{% elif copy.status == 'm' %}text-danger{% else %}text-warning{% endif %}">
 \{{ copy.get_status_display }} </p>
```

Questa funzione viene creata automaticamente perché `BookInstance.status` è un [campo choices](https://docs.djangoproject.com/en/5.0/ref/models/fields/#choices).
Django crea automaticamente un metodo `get_foo_display()` per ogni campo choices `foo` in un modello, che può essere utilizzato per ottenere il valore corrente del campo.

## Come appare?

A questo punto dovrebbe essere stato creato tutto il necessario per visualizzare sia le pagine dell'elenco dei libri sia quelle dei dettagli del libro. Avviare il server (`python3 manage.py runserver`) e aprire il browser all'indirizzo `http://127.0.0.1:8000/`.

> [!WARNING]
> Non fare ancora clic su alcun link di autore o dettaglio dell'autore: verranno creati nella sfida.

Fare clic sul link **All books** per visualizzare l'elenco dei libri.

![Pagina elenco dei libri](book_list_page_no_pagination.png)

Fare quindi clic su un link a uno dei libri. Se tutto è configurato correttamente, dovrebbe apparire qualcosa di simile allo screenshot seguente.

![Pagina dettaglio del libro](book_detail_page_no_pagination.png)

## Paginazione

Se ci sono solo pochi record, la pagina dell'elenco dei libri avrà un aspetto adeguato. Tuttavia, con decine o centinaia di record, il caricamento della pagina richiederà progressivamente più tempo e il contenuto sarà eccessivo per una consultazione efficace. La soluzione consiste nell'aggiungere la paginazione alle viste elenco, riducendo il numero di elementi mostrati su ciascuna pagina.

Django offre un eccellente supporto integrato per la paginazione. Ancora meglio, questo supporto è integrato nelle viste generiche di elenco basate su classi, quindi è necessario fare molto poco per abilitarlo.

### Viste

Aprire **catalog/views.py** e aggiungere la riga `paginate_by` mostrata di seguito.

```python
class BookListView(generic.ListView):
    model = Book
    paginate_by = 10
```

Con questa aggiunta, non appena saranno presenti più di 10 record, la vista inizierà a paginare i dati inviati al template.
Alle diverse pagine si accede utilizzando parametri GET: per accedere alla pagina 2, si utilizza l'URL `/catalog/books/?page=2`.

### Template

Ora che i dati sono paginati, è necessario aggiungere al template il supporto per scorrere il set di risultati. Poiché potrebbe essere necessario paginare tutte le viste elenco, questo supporto verrà aggiunto al template di base.

Aprire **/django-locallibrary-tutorial/catalog/templates/_base_generic.html_** e trovare il "content block", come mostrato di seguito.

```django
{% block content %}{% endblock %}
```

Copiare il blocco di paginazione seguente immediatamente dopo `{% endblock %}`. Il codice verifica innanzitutto se la paginazione è abilitata nella pagina corrente. In tal caso, aggiunge i link _successivo_ e _precedente_ secondo necessità, oltre al numero della pagina corrente.

```django
{% block pagination %}
    {% if is_paginated %}
        <div class="pagination">
            <span class="page-links">
                {% if page_obj.has_previous %}
                    <a href="\{{ request.path }}?page=\{{ page_obj.previous_page_number }}">previous</a>
                {% endif %}
                <span class="page-current">
                    Page \{{ page_obj.number }} of \{{ page_obj.paginator.num_pages }}.
                </span>
                {% if page_obj.has_next %}
                    <a href="\{{ request.path }}?page=\{{ page_obj.next_page_number }}">next</a>
                {% endif %}
            </span>
        </div>
    {% endif %}
  {% endblock %}
```

`page_obj` è un oggetto [Paginator](https://docs.djangoproject.com/en/5.0/topics/pagination/#paginator-objects) che esiste se nella pagina corrente viene utilizzata la paginazione. Consente di ottenere tutte le informazioni sulla pagina corrente, sulle pagine precedenti, sul numero di pagine esistenti e così via.

Viene utilizzato `\{{ request.path }}` per ottenere l'URL della pagina corrente e creare i link di paginazione. Questo è utile perché è indipendente dall'oggetto che viene paginato.

È tutto!

### Come appare?

Lo screenshot seguente mostra l'aspetto della paginazione. Se non sono stati inseriti più di 10 titoli nel database, è possibile testarla più facilmente diminuendo il numero specificato nella riga `paginate_by` del file **catalog/views.py**. Per ottenere il risultato seguente, il valore è stato cambiato in `paginate_by = 2`.

I link di paginazione vengono visualizzati in fondo, con i link successivo/precedente mostrati a seconda della pagina in cui ci si trova.

![Pagina elenco dei libri - paginata](book_list_paginated.png)

## Mettiti alla prova

La sfida di questo articolo consiste nel creare le viste di dettaglio e di elenco degli autori necessarie per completare il progetto. Dovrebbero essere disponibili ai seguenti URL:

- `catalog/authors/` — L'elenco di tutti gli autori.
- `catalog/author/<id>` — La vista di dettaglio per l'autore specifico con un campo chiave primaria denominato `<id>`.

Il codice necessario per le mappe URL e le viste dovrebbe essere praticamente identico alle viste di elenco e dettaglio `Book` create sopra. I template saranno diversi, ma condivideranno comportamenti simili.

> [!NOTE]
>
> - Dopo aver creato la mappa URL per la pagina dell'elenco degli autori, sarà necessario aggiornare anche il link **All authors** nel template di base.
>   Seguire lo [stesso processo](#aggiornamento_del_template_di_base) utilizzato per aggiornare il link **All books**.
> - Dopo aver creato la mappa URL per la pagina di dettaglio dell'autore, aggiornare anche il [template della vista di dettaglio del libro](#creazione_del_template_della_vista_di_dettaglio) (**/django-locallibrary-tutorial/catalog/templates/catalog/book_detail.html**) affinché il link dell'autore punti alla nuova pagina di dettaglio dell'autore, anziché avere un URL vuoto.
>   Il modo consigliato per farlo è chiamare `get_absolute_url()` sul modello dell'autore, come mostrato di seguito.
>
>   ```django
>   <p>
>     <strong>Author:</strong>
>     <a href="\{{ book.author.get_absolute_url }}">\{{ book.author }}</a>
>   </p>
>   ```

Al termine, le pagine dovrebbero apparire più o meno come negli screenshot seguenti.

![Pagina elenco degli autori](author_list_page_no_pagination.png)

![Pagina dettaglio dell'autore](author_detail_page_no_pagination.png)

## Riepilogo

Congratulazioni, le funzionalità di base della libreria sono ora complete.

In questo articolo è stato illustrato come utilizzare le viste generiche di elenco e dettaglio basate su classi e come usarle per creare pagine che visualizzano libri e autori. Durante il percorso sono stati esaminati il pattern matching con espressioni regolari e il passaggio di dati dagli URL alle viste. Sono stati inoltre illustrati alcuni ulteriori accorgimenti per l'uso dei template. Infine, è stato mostrato come paginare le viste elenco affinché gli elenchi rimangano gestibili anche in presenza di molti record.

Nei prossimi articoli, questa libreria verrà estesa per supportare gli account utente e verranno quindi illustrati autenticazione utente, autorizzazioni, sessioni e form.

## Vedere anche

- [Built-in class-based generic views](https://docs.djangoproject.com/en/5.0/topics/class-based-views/generic-display/) (documentazione di Django)
- [Generic display views](https://docs.djangoproject.com/en/5.0/ref/class-based-views/generic-display/) (documentazione di Django)
- [Introduction to class-based views](https://docs.djangoproject.com/en/5.0/topics/class-based-views/intro/) (documentazione di Django)
- [Built-in template tags and filters](https://docs.djangoproject.com/en/5.0/ref/templates/builtins/) (documentazione di Django)
- [Pagination](https://docs.djangoproject.com/en/5.0/topics/pagination/) (documentazione di Django)
- [Making queries > Related objects](https://docs.djangoproject.com/en/5.0/topics/db/queries/#related-objects) (documentazione di Django)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Home_page", "Learn_web_development/Extensions/Server-side/Django/Sessions", "Learn_web_development/Extensions/Server-side/Django")}}
