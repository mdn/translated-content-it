---
title: "Tutorial Django Parte 6: Viste generiche di lista e dettaglio"
short-title: "6: Viste generiche di lista e dettaglio"
slug: Learn_web_development/Extensions/Server-side/Django/Generic_views
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Home_page", "Learn_web_development/Extensions/Server-side/Django/Sessions", "Learn_web_development/Extensions/Server-side/Django")}}

Questo tutorial estende il nostro sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website), aggiungendo pagine di lista e dettaglio per libri e autori. Qui impareremo sulle viste generiche basate su classi, e mostreremo come possono ridurre la quantità di codice che lei deve scrivere per i casi d'uso comuni. Mostreremo anche la gestione degli URL in maggiore dettaglio, illustrando come eseguire l'abbinamento di pattern di base.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Completi tutti gli argomenti del tutorial precedente, compreso <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/Home_page">Django Tutorial Parte 5: Creazione della nostra home page</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere dove e come usare le viste generiche basate su classi, e come estrarre pattern dagli URL e passare l'informazione alle viste.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

In questo tutorial completeremo la prima versione del sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website) aggiungendo pagine di lista e dettaglio per libri e autori (o, per essere più precisi, le mostreremo come implementare le pagine dei libri, mentre lei creerà le pagine degli autori da sola!).

Il processo è simile a quello della creazione della pagina indice, che abbiamo mostrato nel tutorial precedente. Avremo ancora bisogno di creare mappe URL, viste, e template. La principale differenza è che per le pagine di dettaglio affronteremo la sfida aggiuntiva di estrarre informazioni dai pattern negli URL e passarle alla vista. Per queste pagine, dimostreremo un tipo di vista completamente diverso: viste generiche basate su classi di lista e dettaglio. Queste possono ridurre significativamente la quantità di codice delle viste necessario, rendendole più facili da scrivere e mantenere.

La parte finale del tutorial dimostrerà come paginare i dati quando si utilizzano viste generiche basate su classi di lista.

## Pagina lista libri

La pagina lista libri mostrerà un elenco di tutti i record di libri disponibili nella pagina, accessibile tramite l'URL: `catalog/books/`. La pagina mostrerà un titolo e un autore per ogni record, con il titolo che sarà un hyperlink alla pagina dettaglio del libro associato. La pagina avrà la stessa struttura e navigazione di tutte le altre pagine nel sito, e pertanto potremo estendere il template di base (**base_generic.html**) che abbiamo creato nel tutorial precedente.

### Mappatura URL

Apro **/catalog/urls.py** e copi la riga che imposta il percorso per `'books/'`, come mostrato di seguito. Proprio come per la pagina indice, questa funzione `path()` definisce un pattern da abbinare all'URL (**'books/'**), una funzione vista che sarà chiamata se l'URL corrisponde (`views.BookListView.as_view()`), e un nome per questa particolare mappatura.

```python
urlpatterns = [
    path('', views.index, name='index'),
    path('books/', views.BookListView.as_view(), name='books'),
]
```

Come discusso nel tutorial precedente, l'URL deve già aver abbinato `/catalog`, quindi la vista sarà effettivamente chiamata per l'URL: `/catalog/books/`.

La funzione vista ha un formato diverso dal precedente — questo perché questa vista sarà effettivamente implementata come una classe. Invece di scrivere da zero la nostra funzione vista, erediteremo una funzione vista generica esistente che già fa la maggior parte di ciò che vogliamo.

Per le viste basate su classi di Django, accediamo a una funzione vista appropriata chiamando il metodo della classe `as_view()`. Questo fa tutto il lavoro di creazione di un'istanza della classe, assicurando che i metodi del gestore giusti vengano chiamati per le richieste HTTP in arrivo.

### Vista (basata su classi)

Potremmo facilmente scrivere la vista della lista dei libri come una funzione regolare (proprio come la nostra vista indice precedente), che interroga il database per tutti i libri, e poi chiama `render()` per passare l'elenco a un template specificato. Tuttavia, useremo una vista generica basata su classi (`ListView`) — una classe che eredita da una vista esistente. Poiché la vista generica implementa già la maggior parte delle funzionalità di cui abbiamo bisogno e segue le migliori pratiche di Django, saremo in grado di creare una vista di lista più robusta con meno codice, meno ripetizioni e, in definitiva, meno manutenzione.

Apro **catalog/views.py**, e copi il seguente codice in fondo al file:

```python
from django.views import generic

class BookListView(generic.ListView):
    model = Book
```

È tutto! La vista generica interrogherà il database per ottenere tutti i record per il modello specificato (`Book`) quindi renderà un template situato in **/django-locallibrary-tutorial/catalog/templates/catalog/book_list.html** (che creeremo di seguito). All'interno del template lei può accedere alla lista dei libri con la variabile di template denominata `object_list` O `book_list` (ossia, genericamente `<the model name>_list`).

> [!NOTE]
> Questo percorso scomodo per la posizione del template non è un errore di stampa — le viste generiche cercano i template in `/application_name/the_model_name_list.html` (`catalog/book_list.html` in questo caso) all'interno della directory `/application_name/templates/` dell'applicazione (`/catalog/templates/).

Può aggiungere degli attributi per cambiare il comportamento predefinito sopra. Ad esempio, lei può specificare un altro file di template se ha bisogno di avere più viste che usano questo stesso modello, o potrebbe voler usare un nome di variabile di template diverso se `book_list` non è intuitivo per il suo particolare caso d'uso del template. Forse la variazione più utile è cambiare/filtrare il sottoinsieme di risultati che vengono restituiti — quindi invece di elencare tutti i libri potrebbe elencare i primi 5 libri che sono stati letti da altri utenti.

```python
class BookListView(generic.ListView):
    model = Book
    context_object_name = 'book_list'   # your own name for the list as a template variable
    queryset = Book.objects.filter(title__icontains='war')[:5] # Get 5 books containing the title war
    template_name = 'books/my_arbitrary_template_name_list.html'  # Specify your own template name/location
```

#### Sovrascritture dei metodi nelle viste basate su classi

Anche se non abbiamo bisogno di farlo qui, è possibile anche sovrascrivere alcuni dei metodi della classe.

Ad esempio, possiamo sovrascrivere il metodo `get_queryset()` per cambiare l'elenco dei record restituiti. Questo è più flessibile rispetto a impostare semplicemente l'attributo `queryset` come abbiamo fatto nel frammento di codice precedente (anche se in questo caso non ci sono reali benefici):

```python
class BookListView(generic.ListView):
    model = Book

    def get_queryset(self):
        return Book.objects.filter(title__icontains='war')[:5] # Get 5 books containing the title war
```

Potremmo anche sovrascrivere `get_context_data()` per passare variabili di contesto aggiuntive al template (ad esempio, l'elenco dei libri è passato per impostazione predefinita). Il frammento qui sotto mostra come aggiungere una variabile chiamata `some_data` al contesto (sarebbe poi disponibile come variabile di template).

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

Quando si fa questo è importante seguire lo schema usato sopra:

- Prima ottenga il contesto esistente dalla nostra superclasse.
- Poi aggiunga la sua nuova informazione di contesto.
- Poi ritorni il nuovo contesto (aggiornato).

> [!NOTE]
> Dai un'occhiata alle [Viste generiche basate su classi integrate](https://docs.djangoproject.com/en/5.0/topics/class-based-views/generic-display/) (documentazione Django) per molti più esempi di cosa si può fare.

### Creazione del template della Vista Lista

Crea il file HTML **/django-locallibrary-tutorial/catalog/templates/catalog/book_list.html** e copi il testo qui sotto. Come discusso sopra, questo è il file di template predefinito atteso dalla vista generica basata su classi di lista (per un modello chiamato `Book` in un'applicazione chiamata `catalog`).

I template per le viste generiche sono proprio come qualsiasi altro template (anche se ovviamente il contesto/informazione passata al template può differire). Come con il nostro template _index_, estendiamo il nostro template di base nella prima riga e poi sostituiamo il blocco chiamato `content`.

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

La vista passa il contesto (lista dei libri) per impostazione predefinita come alias `object_list` e `book_list`; entrambi funzioneranno.

#### Esecuzione condizionale

Usiamo i tag template [`if`](https://docs.djangoproject.com/en/5.0/ref/templates/builtins/#if), `else`, e `endif` per verificare se `book_list` è definito e non è vuoto. Se `book_list` è vuoto, allora la clausola `else` visualizza testo che spiega che non ci sono libri da elencare. Se `book_list` non è vuoto, allora iteriamo attraverso la lista dei libri.

```django
{% if book_list %}
  <!-- code here to list the books -->
{% else %}
  <p>There are no books in the library.</p>
{% endif %}
```

La condizione sopra controlla solo un caso, ma è possibile testare condizioni aggiuntive utilizzando il tag template `elif` (ad esempio, `{% elif var2 %}`). Per ulteriori informazioni sugli operatori condizionali, vedere: [if](https://docs.djangoproject.com/en/5.0/ref/templates/builtins/#if), [ifequal/ifnotequal](https://docs.djangoproject.com/en/5.0/ref/templates/builtins/#ifequal-and-ifnotequal), e [ifchanged](https://docs.djangoproject.com/en/5.0/ref/templates/builtins/#ifchanged) in [Tag template e filtri integrati](https://docs.djangoproject.com/en/5.0/ref/templates/builtins/) (documentazione Django).

#### Cicli For

Il template usa i tag template [for](https://docs.djangoproject.com/en/5.0/ref/templates/builtins/#for) e `endfor` per iterare attraverso la lista dei libri, come mostrato di seguito. Ogni iterazione popola la variabile template `book` con l'informazione per l'articolo attuale della lista.

```django
{% for book in book_list %}
  <li><!-- code here get information from each book item --></li>
{% endfor %}
```

Si potrebbe anche usare il tag template `{% empty %}` per definire cosa succede se la lista dei libri è vuota (anche se il nostro template sceglie di usare invece una condizione):

```django
<ul>
  {% for book in book_list %}
    <li><!-- code here get information from each book item --></li>
  {% empty %}
    <p>There are no books in the library.</p>
  {% endfor %}
</ul>
```

Anche se non viene utilizzato qui, all'interno del ciclo Django creerà altre variabili che si possono usare per tenere traccia dell'iterazione. Ad esempio, può testare la variabile `forloop.last` per eseguire un'elaborazione condizionale l'ultima volta che il ciclo viene eseguito.

#### Accesso alle variabili

Il codice all'interno del ciclo crea un elemento della lista per ogni libro che mostra sia il titolo (come link alla vista dettaglio non ancora creata) che l'autore.

```django
<a href="\{{ book.get_absolute_url }}">\{{ book.title }}</a> (\{{book.author}})
```

Accediamo ai _campi_ del record di libro associato usando la "notazione punto" (ad esempio, `book.title` e `book.author`), dove il testo che segue l'elemento `book` è il nome del campo (come definito nel modello).

Possiamo anche chiamare _funzioni_ nel modello all'interno del nostro template — in questo caso chiamiamo `Book.get_absolute_url()` per ottenere un URL che si potrebbe usare per visualizzare il record dettaglio associato. Questo funziona a patto che la funzione non abbia argomenti (non c'è modo di passare argomenti!).

> [!NOTE]
> Dobbiamo essere un po' attenti agli "effetti collaterali" quando chiamiamo funzioni nei template. Qui otteniamo solo un URL da visualizzare, ma una funzione può fare praticamente qualsiasi cosa — non vorremmo cancellare il nostro database (per esempio) solo renderizzando il nostro template!

#### Aggiornare il template di base

Apro il template di base (**/django-locallibrary-tutorial/catalog/templates/_base_generic.html_**) e inserisco **{% url 'books' %}** nel link URL per **All books**, come mostrato di seguito. Questo abiliterà il link su tutte le pagine (possiamo metterlo in atto ora correttamente che abbiamo creato il mapper "books" per gli URL).

```django
<li><a href="{% url 'index' %}">Home</a></li>
<li><a href="{% url 'books' %}">All books</a></li>
<li><a href="">All authors</a></li>
```

### Che aspetto ha?

Non sarà ancora in grado di costruire la lista dei libri, perché ci manca una dipendenza — il mapper URL per le pagine di dettaglio dei libri, che è necessario per creare hyperlink ai singoli libri. Mostreremo entrambe le viste di lista e dettaglio dopo la prossima sezione.

## Pagina dettaglio libro

La pagina dettaglio libro mostrerà informazioni su un libro specifico, accessibile tramite l'URL `catalog/book/<id>` (dove `<id>` è la chiave primaria per il libro). Oltre ai campi nel modello `Book` (autore, riassunto, ISBN, lingua e genere), elencheremo anche i dettagli delle copie disponibili (`BookInstances`) includendo lo stato, la data di ritorno prevista, l'impronta, e l'id. Questo permetterà ai nostri lettori non solo di conoscere il libro, ma anche di confermare se/quando è disponibile.

### Mappatura URL

Apro **/catalog/urls.py** e aggiungo il percorso nominato '**book-detail**' mostrato qui sotto. Questa funzione `path()` definisce un pattern, una vista generica basata su classi di dettaglio associata, e un nome.

```python
urlpatterns = [
    path('', views.index, name='index'),
    path('books/', views.BookListView.as_view(), name='books'),
    path('book/<int:pk>', views.BookDetailView.as_view(), name='book-detail'),
]
```

Per il percorso _book-detail_ il pattern URL usa una sintassi speciale per catturare l'id specifico del libro che vogliamo vedere. La sintassi è molto semplice: le parentesi angolari definiscono la parte dell'URL da catturare, racchiudendo il nome della variabile che la vista può usare per accedere ai dati catturati. Ad esempio, **\<qualcosa>**, catturerà il pattern contrassegnato e passerà il valore alla vista come variabile "qualcosa". Può opzionalmente precedere il nome della variabile con una [specificazione di convertitore](https://docs.djangoproject.com/en/5.0/topics/http/urls/#path-converters) che definisce il tipo di dati (int, str, slug, uuid, path).

In questo caso usiamo `'<int:pk>'` per catturare l'id del libro, che deve essere una stringa formattata in modo speciale e la passiamo alla vista come parametro chiamato `pk` (abbreviazione di chiave primaria). Questo è l'id che viene utilizzato per memorizzare il libro in modo univoco nel database, come definito nel Modello Book.

> [!NOTE]
> Come discusso precedentemente, il nostro URL abbinato è in realtà `catalog/book/<cifre>` (poiché siamo nell'applicazione **catalog**, si presuppone `/catalog/`).

> [!WARNING]
> La vista generica basata su classi di dettaglio _si aspetta_ di ricevere un parametro chiamato **pk**. Se sta scrivendo la sua funzione vista può usare qualsiasi nome di parametro preferisce, o addirittura passare l'informazione come un argomento senza nome.

#### Abbinamento avanzato di percorsi/introduzione alle espressioni regolari

> [!NOTE]
> Non avrà bisogno di questa sezione per completare il tutorial! La forniamo perché sapere di questa opzione potrebbe essere utile nel suo futuro focalizzato su Django.

L'abbinamento dei pattern fornito da `path()` è semplice e utile nei casi (molto comuni) in cui si vuole catturare _qualsiasi_ stringa o intero. Se ha bisogno di un filtro più raffinato (ad esempio, per filtrare solo stringhe che hanno un certo numero di caratteri), allora può usare il metodo [re_path()](https://docs.djangoproject.com/en/5.0/ref/urls/#django.urls.re_path).

Questo metodo viene usato proprio come `path()` tranne che permette di specificare un pattern usando una [Espressione regolare](https://docs.python.org/3/library/re.html). Ad esempio, il percorso precedente potrebbe essere stato scritto come mostrato qui sotto:

```python
re_path(r'^book/(?P<pk>\d+)$', views.BookDetailView.as_view(), name='book-detail'),
```

Le _espressioni regolari_ sono uno strumento di mappatura dei pattern incredibilmente potente. Sono, francamente, piuttosto poco intuitive e possono essere intimidatorie per i principianti. Qui sotto c'è una breve introduzione!

La prima cosa da sapere è che le espressioni regolari dovrebbero di solito essere dichiarate usando la sintassi dei letterali di stringa grezzi (cioè, sono racchiuse come mostrato: **r'\<il testo della tua espressione regolare va qui>'**).

Le principali parti della sintassi di cui avrà bisogno per dichiarare i pattern di abbinamento sono:

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
      <td>Abbina l'inizio del testo</td>
    </tr>
    <tr>
      <td>$</td>
      <td>Abbina la fine del testo</td>
    </tr>
    <tr>
      <td>\d</td>
      <td>Abbina un numero (0, 1, 2, … 9)</td>
    </tr>
    <tr>
      <td>\w</td>
      <td>
        Abbina un carattere di parola, ad esempio, qualsiasi carattere dell'alfabeto
        maiuscolo o minuscolo, numero o il carattere underscore (_)
      </td>
    </tr>
    <tr>
      <td>+</td>
      <td>
        Abbina uno o più caratteri precedenti. Ad esempio, per abbinare
        uno o più numeri si userebbe <code>\d+</code>. Per abbinare
        uno o più caratteri "a", si potrebbe usare <code>a+</code>
      </td>
    </tr>
    <tr>
      <td>*</td>
      <td>
        Abbina zero o più caratteri precedenti. Ad esempio, per abbinare
        nulla o una parola potrebbe usare <code>\w*</code>
      </td>
    </tr>
    <tr>
      <td>( )</td>
      <td>
        Cattura la parte del pattern all'interno delle parentesi. Qualsiasi valore 
        catturato sarà passato alla vista come argomenti senza nome (se più pattern 
        sono catturati, i parametri associati saranno forniti nell'ordine 
        dichiarato).
      </td>
    </tr>
    <tr>
      <td>(?P&#x3C;<em>nome</em>>...)</td>
      <td>
        Cattura il pattern (indicato da ...) come una variabile denominata
        (in questo caso "nome"). I valori catturati sono passati alla vista 
        con il nome specificato. La sua vista deve pertanto dichiarare un parametro 
        con lo stesso nome!
      </td>
    </tr>
    <tr>
      <td>[ ]</td>
      <td>
        Abbina contro un carattere nel set. Ad esempio, [abc] abbinerà su
        'a' o 'b' o 'c'. [-\w] abbinerà sul carattere '-' o su qualsiasi
        carattere di parola.
      </td>
    </tr>
  </tbody>
</table>

La maggior parte degli altri caratteri può essere presa letteralmente!

Consideriamo alcuni veri esempi di pattern:

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
          Questa è l'espressione RE usata nel nostro mapper URL. Abbina una stringa che ha
          <code>book/</code> all'inizio della linea (<strong>^book/</strong>),
          poi ha uno o più numeri (<code>\d+</code>), e poi finisce (con nessun
          carattere non cifra prima del marcatore di fine linea).
        </p>
        <p>
          Cattura anche tutti i numeri <strong>(?P&#x3C;pk>\d+)</strong> e
          li passa alla vista in un parametro chiamato 'pk'.
          <strong>I valori catturati sono sempre passati come stringa!</strong>
        </p>
        <p>
          Ad esempio, questo abbinerebbe <code>book/1234</code>, e invierebbe un
          variabile <code>pk='1234'</code> alla vista.
        </p>
      </td>
    </tr>
    <tr>
      <td><strong>r'^book/(\d+)$'</strong></td>
      <td>
        Questo abbina gli stessi URL come il caso precedente. Le informazioni catturate
        sarebbero inviate come un argomento senza nome alla vista.
      </td>
    </tr>
    <tr>
      <td><strong>r'^book/(?P&#x3C;stub>[-\w]+)$'</strong></td>
      <td>
        <p>
          Questo abbina una stringa che ha <code>book/</code> all'inizio della
          linea (<strong>^book/</strong>), poi ha uno o più caratteri che sono 
          <em>o</em> un '-' o un carattere di parola 
          (<strong>[-\w]+</strong>), e poi finisce. Cattura anche questo set di
          caratteri e li passa alla vista in un parametro chiamato 'stub'.
        </p>
        <p>
          Questo è un pattern abbastanza tipico per uno "stub". Gli stub sono
          chiavi primarie basate su parole compatibili con gli URL per i dati. Si potrebbe usare
          uno stub se si volesse che il suo URL del libro fosse più informativo. Ad esempio
          <code>/catalog/book/the-secret-garden</code> invece di
          <code>/catalog/book/33</code>.
        </p>
      </td>
    </tr>
  </tbody>
</table>

Si possono catturare più pattern in un unico match, e quindi codificare un sacco di informazioni differenti in un URL.

> [!NOTE]
> Come sfida, consideri come potrebbe codificare un URL per elencare tutti i libri rilasciati in un particolare anno, mese, giorno, e il RE che potrebbe essere utilizzato per abbinarlo.

#### Passaggio di opzioni aggiuntive nelle sue mappe URL

Una caratteristica che non abbiamo utilizzato qui, ma che potrà trovare utile, è che può passare un [dizionario contenente opzioni aggiuntive](https://docs.djangoproject.com/en/5.0/topics/http/urls/#views-extra-options) alla vista (usando il terzo argomento non nominato nella funzione `path()`). Questo approccio può essere utile se si vuole utilizzare la stessa vista per più risorse, e passare dati per configurarne il comportamento in ciascun caso.

Ad esempio, dato il percorso mostrato sotto, per una richiesta a `/my-url/halibut/` Django chiamerà `views.my_view(request, fish='halibut', my_template_name='some_path')`.

```python
path('my-url/<fish>', views.my_view, {'my_template_name': 'some_path'}, name='aurl'),
```

> [!NOTE]
> Sia i pattern catturati con nome che le opzioni di dizionario sono passate alla vista come argomenti _con nome_. Se usa lo **stesso nome** sia per un pattern catturato che per una chiave di dizionario, allora l'opzione di dizionario sarà utilizzata.

### Vista (basata su classi)

Apro **catalog/views.py**, e copi il seguente codice in fondo al file:

```python
class BookDetailView(generic.DetailView):
    model = Book
```

È tutto! Tutto ciò che deve fare ora è creare un template chiamato **/django-locallibrary-tutorial/catalog/templates/catalog/book_detail.html**, e la vista gli passerà le informazioni del database per il record `Book` specifico estratto dal mapper URL. All'interno del template lei può accedere ai dettagli del libro con la variabile di template denominata `object` O `book` (cioè, genericamente `the_model_name`).

Se necessario, è possibile cambiare il template utilizzato e il nome dell'oggetto di contesto utilizzato per riferirsi al libro nel template. Può anche sovrascrivere i metodi per, ad esempio, aggiungere informazioni aggiuntive al contesto.

#### Cosa succede se il record non esiste?

Se un record richiesto non esiste, la vista generica basata su classi di dettaglio solleverà automaticamente un'eccezione `Http404` per lei — in produzione, questo visualizzerà automaticamente una pagina appropriata di "risorsa non trovata", che lei può personalizzare se lo desidera.

Per darle un'idea di come funziona, il frammento di codice qui sotto dimostra come implementerebbe la vista basata su classi come una funzione se **non** stesse usando la vista generica basata su classi di dettaglio.

```python
def book_detail_view(request, primary_key):
    try:
        book = Book.objects.get(pk=primary_key)
    except Book.DoesNotExist:
        raise Http404('Book does not exist')

    return render(request, 'catalog/book_detail.html', context={'book': book})
```

La vista prima cerca di ottenere il record libro specifico dal modello. Se questo non riesce, la vista dovrebbe sollevare un'eccezione `Http404` per indicare che il libro è "non trovato". L'ultimo passaggio è poi, come al solito, chiamare `render()` con il nome del template e i dati del libro nel parametro `context` (come dizionario).

Un altro modo in cui potrebbe fare questo se non stesse usando una vista generica sarebbe quello di chiamare la funzione `get_object_or_404()`. Questo è un shortcut per sollevare un'eccezione `Http404` se il record non è trovato.

```python
from django.shortcuts import get_object_or_404

def book_detail_view(request, primary_key):
    book = get_object_or_404(Book, pk=primary_key)
    return render(request, 'catalog/book_detail.html', context={'book': book})
```

### Creazione del template della Vista Dettaglio

Crea il file HTML **/django-locallibrary-tutorial/catalog/templates/catalog/book_detail.html** e inserisci il contenuto qui sotto. Come discusso sopra, questo è il nome del file di template predefinito atteso dalla vista generica basata su classi di dettaglio (per un modello chiamato `Book` in un'applicazione chiamata `catalog`).

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
> Il link dell'autore nel template sopra ha un URL vuoto perché non abbiamo ancora creato una pagina di dettaglio per gli autori a cui collegarci. Una volta che la pagina dettaglio esiste possiamo ottenere il suo URL con uno dei seguenti due approcci:
>
> - Usare il tag template `url` per invertire l'URL 'author-detail' (definito nel mapper URL), passando l'istanza dell'autore per il libro:
>
>   ```django
>   <a href="{% url 'author-detail' book.author.pk %}">\{{ book.author }}</a>
>   ```
>
> - Chiamare il metodo `get_absolute_url()` del modello autore (questo esegue la stessa operazione inversa):
>
>   ```django
>   <a href="\{{ book.author.get_absolute_url }}">\{{ book.author }}</a>
>   ```
>
> Anche se entrambi i metodi fanno effettivamente la stessa cosa, `get_absolute_url()` è preferito perché aiuta a scrivere un codice più consistente e manutenibile (eventuali cambiamenti devono essere fatti solo in un unico posto: il modello autore).

Anche se un po' più grande, quasi tutto in questo template è stato descritto in precedenza:

- Estendiamo il nostro template di base e sovrascriviamo il blocco "content".
- Usiamo l'elaborazione condizionale per determinare se mostrare o meno contenuti specifici.
- Usiamo i cicli `for` per iterare attraverso gli elenchi di oggetti.
- Accediamo ai campi di contesto usando la notazione punto (poiché abbiamo usato la vista generica di dettaglio, il contesto è denominato `book`; potremmo anche usare `object`)

La prima cosa interessante che non abbiamo ancora visto è la funzione `book.bookinstance_set.all()`. Questo metodo è creato "automaticamente" da Django per restituire l'insieme dei record `BookInstance` associati a un particolare `Book`.

```django
{% for copy in book.bookinstance_set.all %}
  <!-- code to iterate across each copy/instance of a book -->
{% endfor %}
```

Questo metodo è necessario perché lei dichiara un campo `ForeignKey` (uno-a-molti) solo nel lato "molti" della relazione (il `BookInstance`). Poiché non si fa nulla per dichiarare la relazione nell'altro modello ("uno"), esso (il `Book`) non ha alcun campo per ottenere l'insieme di record associati. Per superare questo problema, Django crea una funzione di "lookup inverso" con un nome appropriato che si può usare. Il nome della funzione è costruito trasformando in minuscolo il nome del modello dove è stato dichiarato il `ForeignKey`, seguito da `_set` (cioè, la funzione creata in `Book` è `bookinstance_set()`).

> [!NOTE]
> Qui usiamo `all()` per ottenere tutti i record (il predefinito). Anche se può usare il metodo `filter()` per ottenere un sottoinsieme di record nel codice, non può farlo direttamente nei template perché non può specificare argomenti per le funzioni.
>
> Attenzione anche che se non definisce un ordine (nella sua vista basata su classi o modello), vedrà anche errori dal server di sviluppo come questo:
>
> ```plain
> [29/May/2017 18:37:53] "GET /catalog/books/?page=1 HTTP/1.1" 200 1637
> /foo/local_library/venv/lib/python3.5/site-packages/django/views/generic/list.py:99: UnorderedObjectListWarning: Pagination may yield inconsistent results with an unordered object_list: <QuerySet [<Author: Ortiz, David>, <Author: H. McRaven, William>, <Author: Leigh, Melinda>]>
>   allow_empty_first_page=allow_empty_first_page, **kwargs)
> ```
>
> Questo succede perché l'oggetto [paginator](https://docs.djangoproject.com/en/5.0/topics/pagination/#paginator-objects) si aspetta di vedere alcuni ORDINE ESECUITO sulla sua base di dati sottostante. Senza questo, non può essere sicuro che i record restituiti siano effettivamente nell'ordine giusto!
>
> Questo tutorial non ha ancora coperto la **Paginazione** (ancora!), ma poiché non può usare `sort_by()` e passare un parametro (lo stesso con `filter()` descritto sopra) dovrà scegliere tra tre opzioni:
>
> 1. Aggiungere un `ordering` all'interno di una dichiarazione `class Meta` nel suo modello.
> 2. Aggiungere un attributo `queryset` nella sua vista personalizzata basata su classi, specificando un `order_by()`.
> 3. Aggiungere un metodo `get_queryset` alla sua vista personalizzata basata su classi e specificare anche `order_by()`.
>
> Se decide di andare con una `class Meta` per il modello `Author` (probabilmente non flessibile come personalizzare la vista basata su classi, ma abbastanza facile), finirà con qualcosa simile a questo:
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
> Naturalmente, il campo non deve essere `last_name`: potrebbe essere qualsiasi altro.
>
> Infine, ma non meno importante, dovrebbe ordinarlo per un attributo/una colonna che abbia effettivamente un indice (unico o meno) nel suo database per evitare problemi di prestazioni. Naturalmente, questo non sarà necessario qui (probabilmente stiamo anticipando con così pochi libri e utenti), ma è qualcosa di utile da tenere a mente per progetti futuri.

La seconda cosa interessante (e non ovvia) nel template è dove visualizziamo il testo di stato per ogni istanza di libro ("disponibile", "in manutenzione", ecc.). I lettori acuti noteranno che il metodo `BookInstance.get_status_display()` che usiamo per ottenere il testo di stato non appare altrove nel codice.

```django
 <p class="{% if copy.status == 'a' %}text-success{% elif copy.status == 'm' %}text-danger{% else %}text-warning{% endif %}">
 \{{ copy.get_status_display }} </p>
```

Questa funzione viene creata automaticamente perché `BookInstance.status` è un [campo di scelte](https://docs.djangoproject.com/en/5.0/ref/models/fields/#choices). Django crea automaticamente un metodo `get_foo_display()` per ogni campo di scelte `foo` in un modello, che può essere usato per ottenere il valore corrente del campo.

## Che aspetto ha?

A questo punto, dovremmo aver creato tutto il necessario per visualizzare sia la pagina di elenco dei libri sia quella di dettaglio dei libri. Esegua il server (`python3 manage.py runserver`) e apra il suo browser su `http://127.0.0.1:8000/`.

> [!WARNING]
> Non clicchi ancora nessun link di dettaglio degli autori — li creerà nella sfida!

Clicchi sul link **All books** per visualizzare l'elenco dei libri.

![Pagina Lista Libri](book_list_page_no_pagination.png)

Poi clicchi su un link per uno dei suoi libri. Se tutto è configurato correttamente, dovrebbe vedere qualcosa di simile allo screenshot seguente.

![Pagina Dettaglio Libro](book_detail_page_no_pagination.png)

## Paginazione

Se ha solo pochi record, la nostra pagina dell'elenco dei libri andrà bene. Tuttavia, man mano che si entra nelle decine o centinaia di record la pagina impiegherà sempre più tempo a caricarsi (e avranno un contenuto troppo abbondante per essere navigati con senso). La soluzione a questo problema è aggiungere la paginazione alle sue viste di elenco, riducendo il numero di elementi visualizzati su ogni pagina.

Django ha un eccellente supporto integrato per la paginazione. Ancora meglio, è integrato nelle viste generiche basate su classi di elenco, quindi non deve fare molto per abilitarlo!

### Viste

Apro **catalog/views.py** e aggiungo la linea `paginate_by` mostrata di seguito.

```python
class BookListView(generic.ListView):
    model = Book
    paginate_by = 10
```

Con questa aggiunta, non appena avrà più di 10 record, la vista inizierà a paginare i dati che invia al template. Le diverse pagine sono accessibili usando i parametri GET — per accedere alla pagina 2 usere sarebbe l'URL `/catalog/books/?page=2`.

### Template

Ora che i dati sono paginati, dobbiamo aggiungere il supporto al template per scorrere attraverso il set di risultati. Poiché potremmo voler paginare tutte le viste di elenco, aggiungeremo questo al template di base.

Apro **/django-locallibrary-tutorial/catalog/templates/_base_generic.html_** e trova il "blocco di contenuto" (come mostrato di seguito).

```django
{% block content %}{% endblock %}
```

Copia il seguente blocco di paginazione immediatamente dopo `{% endblock %}`. Il codice verifica prima se la paginazione è abilitata sulla pagina corrente. Se sì, aggiunge i link _successivi_ e _precedenti_ come appropriato (e il numero di pagina corrente).

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

Il `page_obj` è un oggetto [Paginator](https://docs.djangoproject.com/en/5.0/topics/pagination/#paginator-objects) che esisterà se la paginazione viene usata sulla pagina corrente. Permette di ottenere tutte le informazioni sulla pagina corrente, le pagine precedenti, quante pagine ci sono, ecc.

Usiamo `\{{ request.path }}` per ottenere l'URL della pagina corrente per creare i link di paginazione. Questo è utile perché è indipendente dall'oggetto che stiamo paginando.

È tutto!

### Che aspetto ha?

Lo screenshot qui sotto mostra che aspetto ha la paginazione — se non ha inserito più di 10 titoli nel suo database, allora può testarlo più facilmente abbassando il numero specificato nella linea `paginate_by` nel suo file **catalog/views.py**. Per ottenere il risultato seguente, lo abbiamo cambiato in `paginate_by = 2`.

I link di paginazione sono visualizzati in fondo, con i link successivi/precedenti visualizzati a seconda della pagina su cui si trova.

![Pagina Lista Libri - paginata](book_list_paginated.png)

## Metta alla prova le sue abilità

La sfida in questo articolo è creare le viste di elenco e dettaglio degli autori richieste per completare il progetto. Queste dovrebbero essere rese disponibili ai seguenti URL:

- `catalog/authors/` — L'elenco di tutti gli autori.
- `catalog/author/<id>` — La vista di dettaglio per l'autore specifico con un campo di chiave primaria denominato `<id>`

Il codice richiesto per i mapper URL e le viste dovrebbe essere praticamente identico alle viste di lista e dettaglio dei `Book` che abbiamo creato sopra. I template saranno diversi ma condivideranno comportamenti simili.

> [!NOTE]
>
> - Una volta creato il mapper URL per la pagina di elenco degli autori, dovrà anche aggiornare il link **All authors** nel template base.
>   Segua lo [stesso processo](#aggiornare_il_template_di_base) che abbiamo seguito quando abbiamo aggiornato il link **All books**.
> - Una volta creato il mapper URL per la pagina di dettaglio dell'autore, dovrebbe anche aggiornare il [template della vista di dettaglio del libro](#creazione_del_template_della_vista_dettaglio) (**/django-locallibrary-tutorial/catalog/templates/catalog/book_detail.html**) in modo che il link dell'autore punti alla sua nuova pagina di dettaglio dell'autore (anziché essere un URL vuoto).
>   Il modo consigliato per farlo è chiamare `get_absolute_url()` sul modello autore come mostrato di seguito.
>
>   ```django
>   <p>
>     <strong>Author:</strong>
>     <a href="\{{ book.author.get_absolute_url }}">\{{ book.author }}</a>
>   </p>
>   ```

Quando avrà finito, le sue pagine dovrebbero sembrare qualcosa come gli screenshot di seguito.

![Pagina Lista Autori](author_list_page_no_pagination.png)

![Pagina Dettaglio Autore](author_detail_page_no_pagination.png)

## Sommario

Congratulazioni, la nostra funzionalità di base della biblioteca è ora completa!

In questo articolo, abbiamo imparato a utilizzare le viste generiche basate su classi di lista e dettaglio e le abbiamo usate per creare pagine per visualizzare i nostri libri e autori. Nel corso del tempo abbiamo appreso sulla corrispondenza dei pattern con le espressioni regolari e come si può passare dati dagli URL alle viste. Abbiamo anche scoperto alcuni trucchi in più per usare i template. Infine, abbiamo mostrato come paginare le viste di lista in modo che i nostri elenchi siano gestibili anche quando abbiamo molti record.

Nei nostri prossimi articoli, estenderemo questa libreria per supportare gli account utente, e quindi dimostreremo l'autenticazione utente, i permessi, le sessioni e i form.

## Vedere anche

- [Viste generiche basate su classi integrate](https://docs.djangoproject.com/en/5.0/topics/class-based-views/generic-display/) (documentazione Django)
- [Viste di visualizzazione generiche](https://docs.djangoproject.com/en/5.0/ref/class-based-views/generic-display/) (documentazione Django)
- [Introduzione alle viste basate su classi](https://docs.djangoproject.com/en/5.0/topics/class-based-views/intro/) (documentazione Django)
- [Tag template e filtri integrati](https://docs.djangoproject.com/en/5.0/ref/templates/builtins/) (documentazione Django)
- [Paginazione](https://docs.djangoproject.com/en/5.0/topics/pagination/) (documentazione Django)
- [Creazione di query > Oggetti correlati](https://docs.djangoproject.com/en/5.0/topics/db/queries/#related-objects) (documentazione Django)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Home_page", "Learn_web_development/Extensions/Server-side/Django/Sessions", "Learn_web_development/Extensions/Server-side/Django")}}
