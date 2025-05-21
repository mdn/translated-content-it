---
title: "Tutorial Django Parte 6: Visualizzazioni generiche di lista e dettaglio"
short-title: "6: Visualizzazioni generiche di lista e dettaglio"
slug: Learn_web_development/Extensions/Server-side/Django/Generic_views
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Home_page", "Learn_web_development/Extensions/Server-side/Django/Sessions", "Learn_web_development/Extensions/Server-side/Django")}}

Questo tutorial estende il nostro sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website), aggiungendo pagine di lista e dettaglio per libri e autori. Impareremo a conoscere le viste generiche basate su classi e mostreremo come possono ridurre la quantità di codice da scrivere per casi d'uso comuni. Approfondiremo anche la gestione degli URL, mostrando come eseguire il pattern matching di base.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Completare tutti gli argomenti precedenti del tutorial, incluso <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/Home_page">Tutorial Django Parte 5: Creazione della nostra home page</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere dove e come utilizzare le visualizzazioni generiche basate su classi e come estrarre i pattern dagli URL e passare le informazioni alle visualizzazioni.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

In questo tutorial completeremo la prima versione del sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website) aggiungendo pagine di lista e dettaglio per libri e autori (o, per essere più precisi, vi mostreremo come implementare le pagine dei libri e vi faremo creare le pagine degli autori da soli!).

Il processo è simile a quello della creazione della pagina indice, che abbiamo mostrato nel tutorial precedente. Avremo ancora bisogno di creare mappature URL, viste e template. La differenza principale è che per le pagine di dettaglio, avremo la sfida aggiuntiva di estrarre informazioni dai pattern nell'URL e passarle alla vista. Per queste pagine, dimostreremo un tipo di vista completamente diverso: viste generiche di lista e dettaglio basate su classi. Queste possono ridurre significativamente la quantità di codice delle viste necessarie, rendendole più facili da scrivere e mantenere.

L'ultima parte del tutorial mostrerà come paginare i dati quando si utilizzano viste generiche di lista basate su classi.

## Pagina elenco libri

La pagina elenco libri visualizzerà un elenco di tutti i record di libri disponibili nella pagina, accessibile tramite l'URL: `catalog/books/`. La pagina mostrerà un titolo e un autore per ogni record, con il titolo che sarà un collegamento ipertestuale alla pagina di dettaglio del libro associato. La pagina avrà la stessa struttura e navigazione di tutte le altre pagine del sito e potremo quindi estendere il template base (**base_generic.html**) che abbiamo creato nel tutorial precedente.

### Mappatura URL

Aprire **/catalog/urls.py** e copiare la riga che imposta il percorso per `'books/'`, come mostrato di seguito. Come per la pagina indice, questa funzione `path()` definisce un pattern da confrontare con l'URL (**'books/'**), una funzione vista che verrà chiamata se l'URL corrisponde (`views.BookListView.as_view()`), e un nome per questa mappatura particolare.

```python
urlpatterns = [
    path('', views.index, name='index'),
    path('books/', views.BookListView.as_view(), name='books'),
]
```

Come discusso nel tutorial precedente, l'URL deve già aver corrisposto a `/catalog`, quindi la vista sarà effettivamente chiamata per l'URL: `/catalog/books/`.

La funzione vista ha un formato diverso rispetto a prima — questo perché questa vista sarà effettivamente implementata come classe. Erediteremo una funzione vista generica esistente che fa già la maggior parte di ciò che desideriamo che questa funzione vista faccia, anziché scrivere la nostra da zero.

Per le visualizzazioni basate su classi di Django, accediamo a una funzione vista appropriata chiamando il metodo di classe `as_view()`. Questo fa tutto il lavoro di creare un'istanza della classe e garantire che i metodi handler corretti siano chiamati per le richieste HTTP in arrivo.

### Vista (basata su classi)

Potremmo facilmente scrivere la vista elenco libri come una funzione regolare (proprio come la nostra vista indice precedente), che interrogherebbe il database per tutti i libri, e quindi chiamerebbe `render()` per passare l'elenco a un template specificato. Invece, tuttavia, useremo una vista generica di elenco basata su classi (`ListView`) — una classe che eredita da una vista esistente. Poiché la vista generica implementa già la maggior parte delle funzionalità di cui abbiamo bisogno e segue le migliori pratiche di Django, saremo in grado di creare una vista elenco più robusta con meno codice, meno ripetizioni e, in definitiva, meno manutenzione.

Aprire **catalog/views.py** e copiare il seguente codice nella parte inferiore del file:

```python
from django.views import generic

class BookListView(generic.ListView):
    model = Book
```

È tutto! La vista generica interrogherà il database per ottenere tutti i record per il modello specificato (`Book`) quindi renderà un template situato in **/django-locallibrary-tutorial/catalog/templates/catalog/book_list.html** (che creeremo di seguito). All'interno del template, si può accedere all'elenco dei libri con la variabile template chiamata `object_list` O `book_list` (cioè, genericamente `<il nome del modello>_list`).

> [!NOTE]
> Questo percorso "scomodo" per la posizione del template non è un errore di stampa — le viste generiche cercano i template in `/application_name/the_model_name_list.html` (`catalog/book_list.html` in questo caso) all'interno della directory `/application_name/templates/` dell'applicazione (`/catalog/templates/`).

Si possono aggiungere attributi per cambiare il comportamento predefinito sopra. Ad esempio, si può specificare un altro file di template se si ha bisogno di avere più viste che utilizzano lo stesso modello, o si potrebbe voler usare un diverso nome di variabile template se `book_list` non è intuitivo per il particolare caso d'uso del template. Forse la variazione più utile è cambiare/filtrare il sottoinsieme dei risultati che vengono restituiti — così invece di elencare tutti i libri, si potrebbero elencare i primi 5 libri che sono stati letti da altri utenti.

```python
class BookListView(generic.ListView):
    model = Book
    context_object_name = 'book_list'   # your own name for the list as a template variable
    queryset = Book.objects.filter(title__icontains='war')[:5] # Get 5 books containing the title war
    template_name = 'books/my_arbitrary_template_name_list.html'  # Specify your own template name/location
```

#### Sostituzione dei metodi nelle visualizzazioni basate su classi

Anche se non è necessario farlo qui, è possibile anche sovrascrivere alcuni dei metodi della classe.

Ad esempio, si può sovrascrivere il metodo `get_queryset()` per cambiare l'elenco dei record restituiti. Questo è più flessibile rispetto a impostare semplicemente l'attributo `queryset` come abbiamo fatto nel precedente frammento di codice (anche se in questo caso non ci sono reali vantaggi):

```python
class BookListView(generic.ListView):
    model = Book

    def get_queryset(self):
        return Book.objects.filter(title__icontains='war')[:5] # Get 5 books containing the title war
```

Si potrebbe anche sovrascrivere `get_context_data()` per passare ulteriori variabili di contesto al template (ad esempio, l'elenco dei libri viene passato per impostazione predefinita). Il frammento sottostante mostra come aggiungere una variabile chiamata `some_data` al contesto (sarebbe poi disponibile come variabile del template).

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

Quando si fa questo, è importante seguire il pattern utilizzato sopra:

- Prima ottenere il contesto esistente dalla nostra superclasse.
- Poi aggiungere le nuove informazioni di contesto.
- Poi restituire il nuovo contesto (aggiornato).

> [!NOTE]
> Controlla [Viste generiche incorporate basate su classi](https://docs.djangoproject.com/en/5.0/topics/class-based-views/generic-display/) (documentazione Django) per molti altri esempi di cosa puoi fare.

### Creare il template della vista elenco

Creare il file HTML **/django-locallibrary-tutorial/catalog/templates/catalog/book_list.html** e copiare il testo sottostante. Come discusso sopra, questo è il file di template predefinito previsto dalla vista generica basata su classi (per un modello chiamato `Book` in un'applicazione chiamata `catalog`).

I template per le viste generiche sono proprio come qualsiasi altro template (sebbene naturalmente il contesto/le informazioni passate al template possono essere diverse). Come nel nostro template _index_, estendiamo il nostro template base nella prima riga e quindi sostituiamo il blocco chiamato `content`.

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

La vista passa il contesto (elenco dei libri) per impostazione predefinita come alias `object_list` e `book_list`; entrambi funzioneranno.

#### Esecuzione condizionale

Utilizziamo i tag template [`if`](https://docs.djangoproject.com/en/5.0/ref/templates/builtins/#if), `else`, e `endif` per verificare se il `book_list` è stato definito e non è vuoto. Se `book_list` è vuoto, la clausola `else` visualizza il testo che spiega che non ci sono libri da elencare. Se `book_list` non è vuoto, iteriamo attraverso l'elenco dei libri.

```django
{% if book_list %}
  <!-- code here to list the books -->
{% else %}
  <p>There are no books in the library.</p>
{% endif %}
```

La condizione sopra controlla solo un caso, ma è possibile testare ulteriori condizioni utilizzando il tag template `elif` (ad esempio, `{% elif var2 %}`). Per ulteriori informazioni sugli operatori condizionali vedere: [if](https://docs.djangoproject.com/en/5.0/ref/templates/builtins/#if), [ifequal/ifnotequal](https://docs.djangoproject.com/en/5.0/ref/templates/builtins/#ifequal-and-ifnotequal), e [ifchanged](https://docs.djangoproject.com/en/5.0/ref/templates/builtins/#ifchanged) nei [Tag e filtri integrati del template](https://docs.djangoproject.com/en/5.0/ref/templates/builtins/) (Documentazione Django).

#### Cicli for

Il template utilizza i tag template [for](https://docs.djangoproject.com/en/5.0/ref/templates/builtins/#for) e `endfor` per iterare attraverso l'elenco dei libri, come mostrato di seguito. Ogni iterazione popola la variabile template `book` con le informazioni per l'elemento corrente dell'elenco.

```django
{% for book in book_list %}
  <li><!-- code here get information from each book item --></li>
{% endfor %}
```

Si potrebbe anche utilizzare il tag template `{% empty %}` per definire cosa accade se l'elenco dei libri è vuoto (sebbene il nostro template scelga di utilizzare invece un'istruzione condizionale):

```django
<ul>
  {% for book in book_list %}
    <li><!-- code here get information from each book item --></li>
  {% empty %}
    <p>There are no books in the library.</p>
  {% endfor %}
</ul>
```

Anche se non utilizzato qui, all'interno del ciclo Django creerà anche altre variabili che si possono usare per tenere traccia dell'iterazione. Ad esempio, si può testare la variabile `forloop.last` per eseguire il processing condizionale l'ultima volta che il ciclo viene eseguito.

#### Accesso alle variabili

Il codice all'interno del ciclo crea un elemento di elenco per ciascun libro che mostra sia il titolo (come link alla vista di dettaglio ancora da creare) sia l'autore.

```django
<a href="\{{ book.get_absolute_url }}">\{{ book.title }}</a> (\{{book.author}})
```

Accediamo ai _campi_ del record del libro associato usando la "notazione a punto" (ad esempio, `book.title` e `book.author`), dove il testo che segue l'elemento `book` è il nome del campo (come definito nel modello).

Possiamo anche chiamare _funzioni_ nel modello dall'interno del nostro template — in questo caso chiamiamo `Book.get_absolute_url()` per ottenere un URL che si potrebbe usare per visualizzare il record di dettaglio associato. Questo funziona a condizione che la funzione non abbia argomenti (non c'è modo di passare argomenti!)

> [!NOTE]
> Dobbiamo fare un po' di attenzione agli "effetti collaterali" quando chiamiamo funzioni nei template. Qui otteniamo solo un URL da visualizzare, ma una funzione può fare praticamente qualsiasi cosa — non vorremmo cancellare il nostro database (per esempio) solo renderizzando il nostro template!

#### Aggiornare il template base

Aprire il template base (**/django-locallibrary-tutorial/catalog/templates/_base_generic.html_**) e inserire **{% url 'books' %}** nel collegamento URL per **All books**, come mostrato di seguito. Questo abiliterà il link in tutte le pagine (possiamo metterlo ora che abbiamo creato il "books" URL mapper).

```django
<li><a href="{% url 'index' %}">Home</a></li>
<li><a href="{% url 'books' %}">All books</a></li>
<li><a href="">All authors</a></li>
```

### Come si presenta?

Non riuscirete ancora a costruire l'elenco dei libri, perché ci manca una dipendenza — la mappa URL per le pagine di dettaglio dei libri, necessaria per creare link ipertestuali a singoli libri. Mostreremo sia le visualizzazioni di elenco che di dettaglio dopo la prossima sezione.

## Pagina dettaglio libro

La pagina dettaglio libro visualizzerà informazioni su un libro specifico, accessibile tramite l'URL `catalog/book/<id>` (dove `<id>` è la chiave primaria per il libro). Oltre ai campi nel modello `Book` (autore, riassunto, ISBN, lingua e genere), elencheremo anche i dettagli delle copie disponibili (`BookInstances`) inclusi lo stato, la data di ritorno prevista, l'impronta e l'id. Questo permetterà ai nostri lettori di non solo apprendere informazioni sul libro, ma anche di confermare se/quando è disponibile.

### Mappatura URL

Aprire **/catalog/urls.py** e aggiungere il percorso denominato '**book-detail**' mostrato di seguito. Questa funzione `path()` definisce un pattern, una vista dettagliata generica basata su classi associata e un nome.

```python
urlpatterns = [
    path('', views.index, name='index'),
    path('books/', views.BookListView.as_view(), name='books'),
    path('book/<int:pk>', views.BookDetailView.as_view(), name='book-detail'),
]
```

Per il percorso _book-detail_, il pattern URL utilizza una sintassi speciale per catturare l'id specifico del libro che vogliamo vedere. La sintassi è molto semplice: le parentesi angolari definiscono la parte dell'URL da catturare, includendo il nome della variabile che la vista può utilizzare per accedere ai dati catturati. 

Ad esempio, **\<something>**, catturerà il pattern contrassegnato e passerà il valore alla vista come variabile "something". Potete opzionalmente precedere il nome della variabile con una [specifica del convertitore](https://docs.djangoproject.com/en/5.0/topics/http/urls/#path-converters) che definisce il tipo di dati (int, str, slug, uuid, path).

In questo caso usiamo `'<int:pk>'` per catturare l'id del libro, che deve essere una stringa formattata in modo speciale e passarla alla vista come un parametro denominato `pk` (abbreviazione di primary key). Questo è l'id che viene utilizzato per memorizzare il libro univocamente nel database, come definito nel Modello Book.

> [!NOTE]
> Come discusso in precedenza, il nostro URL corrisponde effettivamente a `catalog/book/<digits>` (perché siamo nell'applicazione **catalog**, `/catalog/` è assunto).

> [!WARNING]
> La vista dettagliata generica basata su classi si _aspetta_ di ricevere un parametro denominato **pk**. Se stai scrivendo la tua funzione vista, puoi usare qualsiasi nome di parametro tu voglia, o persino passare l'informazione in un argomento senza nome.

#### Abbinamento di percorsi avanzati/introduzione alle espressioni regolari

> [!NOTE]
> Non avete bisogno di questa sezione per completare il tutorial! La forniamo perché sapere questa opzione potrebbe essere utile nel vostro futuro incentrato su Django.

Il pattern matching fornito da `path()` è semplice e utile per i casi (molto comuni) in cui si desidera semplicemente catturare _qualsiasi_ stringa o numero intero. Se hai bisogno di un filtraggio più raffinato (ad esempio, per filtrare solo le stringhe che hanno un certo numero di caratteri), allora puoi utilizzare il metodo [re_path()](https://docs.djangoproject.com/en/5.0/ref/urls/#django.urls.re_path).

Questo metodo viene utilizzato proprio come `path()`, eccetto che consente di specificare un pattern utilizzando una [espressione regolare](https://docs.python.org/3/library/re.html). Ad esempio, il percorso precedente potrebbe essere stato scritto come mostrato di seguito:

```python
re_path(r'^book/(?P<pk>\d+)$', views.BookDetailView.as_view(), name='book-detail'),
```

_Le espressioni regolari_ sono uno strumento di mappatura dei pattern incredibilmente potente. Sono, francamente, piuttosto poco intuitive e possono intimidire i principianti. Di seguito una brevissima introduzione!

La prima cosa da sapere è che le espressioni regolari dovrebbero solitamente essere dichiarate usando la sintassi raw string literal (cioè, sono racchiuse come mostrato: **r'\<il tuo testo di espressione regolare va qui>'**).

Le parti principali della sintassi che dovrai conoscere per dichiarare i pattern di abbinamento sono:

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
      <td>Corrisponde a un numero (0, 1, 2, … 9)</td>
    </tr>
    <tr>
      <td>\w</td>
      <td>
        Corrisponde a un carattere di parola, ad esempio, qualsiasi carattere alfabetico maiuscolo o minuscolo, cifra o il carattere di underscore (_)
      </td>
    </tr>
    <tr>
      <td>+</td>
      <td>
        Corrisponde a uno o più caratteri precedenti. Ad esempio, per coincidere con uno o più numeri si userebbe <code>\d+</code>. Per coincidere con uno o più caratteri "a" si potrebbe usare <code>a+</code>
      </td>
    </tr>
    <tr>
      <td>*</td>
      <td>
        Corrisponde a zero o più caratteri precedenti. Ad esempio, per coincidere con nulla o una parola si potrebbe usare <code>\w*</code>
      </td>
    </tr>
    <tr>
      <td>( )</td>
      <td>
        Cattura la parte del pattern all'interno delle parentesi. Qualsiasi valore catturato verrà passato alla vista come argomenti senza nome (se più pattern sono catturati, gli argomenti associati verranno forniti nell'ordine dichiarato).
      </td>
    </tr>
    <tr>
      <td>(?P&#x3C;<em>nome</em>>...)</td>
      <td>
        Cattura il pattern (indicato da...) come variabile denominata (in questo caso "nome"). I valori catturati vengono passati alla vista con il nome specificato. Pertanto, la tua vista deve dichiarare un parametro con lo stesso nome!
      </td>
    </tr>
    <tr>
      <td>[ ]</td>
      <td>
        Corrisponde a un carattere nel set. Ad esempio, [abc] coinciderà su 'a' o 'b' o 'c'. [-\w] coinciderà sul carattere '-' o qualsiasi carattere di parola.
      </td>
    </tr>
  </tbody>
</table>

La maggior parte degli altri caratteri può essere presa letteralmente!

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
          Questo è il RE utilizzato nella nostra mappa URL. Corresponde a una stringa che ha
          <code>book/</code> all'inizio della riga (<strong>^book/</strong>),
          poi ha uno o più numeri (<code>\d+</code>), e poi termina (con nessun carattere non numero prima del marcatore di fine riga).
        </p>
        <p>
          Cattura anche tutti i numeri <strong>(?P&#x3C;pk>\d+)</strong> e li passa alla vista in un parametro denominato 'pk'.
          <strong>I valori catturati vengono sempre passati come stringa!</strong>
        </p>
        <p>
          Ad esempio, questo corrisponderebbe a <code>book/1234</code>, e invierebbe una variabile <code>pk='1234'</code> alla vista.
        </p>
      </td>
    </tr>
    <tr>
      <td><strong>r'^book/(\d+)$'</strong></td>
      <td>
        Corrisponde agli stessi URL come il caso precedente. Le informazioni catturate sarebbero inviate come argomento senza nome alla vista.
      </td>
    </tr>
    <tr>
      <td><strong>r'^book/(?P&#x3C;stub>[-\w]+)$'</strong></td>
      <td>
        <p>
          Corrisponde a una stringa che ha <code>book/</code> all'inizio della riga (<strong>^book/</strong>), poi ha uno o più caratteri che sono <em>o</em> un '-' o un carattere di parola
          (<strong>[-\w]+</strong>), e poi termina. Cattura anche questo set di caratteri e li passa alla vista in un parametro denominato 'stub'.
        </p>
        <p>
          Questo è un pattern abbastanza tipico per uno "stub". Gli stub sono chiavi primarie basate su parole amichevoli per gli URL. Potresti usare uno stub se vuoi che il tuo URL del libro sia più informativo. Ad esempio
          <code>/catalog/book/the-secret-garden</code> piuttosto che
          <code>/catalog/book/33</code>.
        </p>
      </td>
    </tr>
  </tbody>
</table>

Puoi catturare più pattern in un solo match, e quindi codificare molte informazioni diverse in un URL.

> [!NOTE]
> Come sfida, considera come potresti codificare un URL per elencare tutti i libri rilasciati in un particolare anno, mese, giorno, e il RE che potrebbe essere utilizzato per abbinarlo.

#### Passare opzioni aggiuntive nelle tue mappe URL

Una caratteristica che non abbiamo usato qui, ma che potreste trovare preziosa, è che si può passare un [dizionario contenente opzioni aggiuntive](https://docs.djangoproject.com/en/5.0/topics/http/urls/#views-extra-options) alla vista (usando il terzo argomento non nominato della funzione `path()`). Questo approccio può essere utile se vuoi usare la stessa vista per più risorse, e passare dati per configurare il suo comportamento in ogni caso.

Ad esempio, dato il percorso mostrato di seguito, per una richiesta a `/my-url/halibut/` Django chiamerà `views.my_view(request, fish='halibut', my_template_name='some_path')`.

```python
path('my-url/<fish>', views.my_view, {'my_template_name': 'some_path'}, name='aurl'),
```

> [!NOTE]
> Sia i pattern catturati nominati sia le opzioni del dizionario sono passati alla vista come argomenti _nominati_. Se usi lo **stesso nome** per entrambi un pattern di cattura e una chiave del dizionario, allora verrà utilizzata l'opzione del dizionario.

### Vista (basata su classi)

Apri **catalog/views.py**, e copia il seguente codice nella parte inferiore del file:

```python
class BookDetailView(generic.DetailView):
    model = Book
```

È tutto! Tutto ciò che devi fare ora è creare un template chiamato **/django-locallibrary-tutorial/catalog/templates/catalog/book_detail.html**, e la vista gli passerà le informazioni del database per il record di `Book` specifico estratto dal mapper URL. All'interno del template si possono accedere ai dettagli del libro con la variabile template denominata `object` O `book` (cioè, genericamente `the_model_name`).

Se necessario, puoi modificare il template utilizzato e il nome dell'oggetto di contesto utilizzato per fare riferimento al libro nel template. È possibile anche sovrascrivere i metodi per, ad esempio, aggiungere ulteriori informazioni al contesto.

#### Cosa succede se il record non esiste?

Se un record richiesto non esiste allora la vista dettagliata generica basata su classi solleverà un'eccezione `Http404` automaticamente — in produzione, ciò mostrerà automaticamente una pagina di "risorsa non trovata" adeguatamente personalizzata, se desiderato.

Solo per darti un'idea di come funziona, il frammento di codice di seguito dimostra come implementeresti la vista basata su classi come funzione se **non** stessi usando la vista dettagliata basata su classi generica.

```python
def book_detail_view(request, primary_key):
    try:
        book = Book.objects.get(pk=primary_key)
    except Book.DoesNotExist:
        raise Http404('Book does not exist')

    return render(request, 'catalog/book_detail.html', context={'book': book})
```

La vista prima tenta di ottenere il record del libro specifico dal modello. Se questo fallisce, la vista dovrebbe sollevare un'eccezione `Http404` per indicare che il libro è "non trovato". L'ultimo passo, quindi, è chiamare `render()` con il nome del template e i dati del libro nel parametro `context` (come un dizionario).

Un altro modo per farlo se non si stesse usando una vista generica sarebbe di chiamare la funzione `get_object_or_404()`. Questo è un collegamento per sollevare un'eccezione `Http404` se il record non viene trovato.

```python
from django.shortcuts import get_object_or_404

def book_detail_view(request, primary_key):
    book = get_object_or_404(Book, pk=primary_key)
    return render(request, 'catalog/book_detail.html', context={'book': book})
```

### Creare il template della visualizzazione dettaglio

Crea il file HTML **/django-locallibrary-tutorial/catalog/templates/catalog/book_detail.html** e inserisci il contenuto riportato di seguito. Come discusso sopra, questo è il nome di file template predefinito previsto dalla vista generica basata su classi _dettaglio_ (per un modello chiamato `Book` in un'applicazione chiamata `catalog`).

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
> Il link dell'autore nel template sopra ha un URL vuoto perché non abbiamo ancora creato una pagina di dettaglio dell'autore a cui collegare.
> Una volta che esisterà la pagina di dettaglio, potremmo ottenere il suo URL con uno qualsiasi di questi due approcci:
>
> - Utilizzare il tag template `url` per invertire l'URL 'author-detail' (definito nel mapper URL), passando l'istanza dell'autore per il libro:
>
>   ```django
>   <a href="{% url 'author-detail' book.author.pk %}">\{{ book.author }}</a>
>   ```
>
> - Chiama il metodo `get_absolute_url()` del modello autore (questo opera la stessa inversione):
>
>   ```django
>   <a href="\{{ book.author.get_absolute_url }}">\{{ book.author }}</a>
>   ```
>
> Mentre entrambi i metodi fanno praticamente la stessa cosa, `get_absolute_url()` è preferibile perché aiuta a scrivere codice più coerente e manutenibile (qualsiasi modifica deve essere fatta solo in un punto: il modello autore).

Anche se un po' più grande, quasi tutto in questo template è stato descritto in precedenza:

- Estendiamo il nostro template base e sostituiamo il blocco "content".
- Usiamo l'elaborazione condizionale per determinare se visualizzare o meno contenuti specifici.
- Utilizziamo i cicli `for` per scorrere le liste degli oggetti.
- Accediamo ai campi del contesto usando la notazione a punto (poiché abbiamo usato la vista generica di dettaglio, il contesto è denominato `book`; potremmo anche usare `object`)

La prima cosa interessante che non abbiamo visto prima è la funzione `book.bookinstance_set.all()`. Questo metodo è "automagicamente" costruito da Django per restituire il set di record `BookInstance` associato con un particolare `Book`.

```django
{% for copy in book.bookinstance_set.all %}
  <!-- code to iterate across each copy/instance of a book -->
{% endfor %}
```

Questo metodo è necessario perché dichiarate un campo `ForeignKey` (uno-a-molti) solo nel lato "molti" della relazione (il `BookInstance`). Poiché non dichiarate nulla per definire la relazione nell'altro modello ("uno"), esso (il `Book`) non ha alcun campo per ottenere il set di record associati. Per superare questo problema, Django costruisce una funzione di ricerca inversa con un nome appropriato che si può utilizzare. Il nome della funzione viene costruito abbassando il nome del modello in cui è stata dichiarata la `ForeignKey`, seguito da `_set` (cioè, la funzione creata in `Book` è `bookinstance_set()`).

> [!NOTE]
> Qui usiamo `all()` per ottenere tutti i record (il predefinito). Mentre si può usare il metodo `filter()` per ottenere un sottoinsieme di record nel codice, non si può farlo direttamente nei template poiché non si possono specificare argomenti alle funzioni.
>
> Attenzione anche che se non definite un ordine (sulla vostra vista basata su classi o modello), vedrete anche errori dal server di sviluppo come questo:
>
> ```plain
> [29/May/2017 18:37:53] "GET /catalog/books/?page=1 HTTP/1.1" 200 1637
> /foo/local_library/venv/lib/python3.5/site-packages/django/views/generic/list.py:99: UnorderedObjectListWarning: Pagination may yield inconsistent results with an unordered object_list: <QuerySet [<Author: Ortiz, David>, <Author: H. McRaven, William>, <Author: Leigh, Melinda>]>
>   allow_empty_first_page=allow_empty_first_page, **kwargs)
> ```
>
> Ciò accade perché l'oggetto [paginator](https://docs.djangoproject.com/en/5.0/topics/pagination/#paginator-objects) si aspetta di vedere qualche ORDINE ESECUTIVO eseguito sul tuo database sottostante. Senza di esso, non può essere sicuro che i record restituiti siano effettivamente nell'ordine giusto!
>
> Questo tutorial non ha coperto **Paginazione** (ancora!), ma poiché non si può usare `sort_by()` e passare un parametro (lo stesso con `filter()` descritto sopra) dovrai scegliere tra tre opzioni:
>
> 1. Aggiungere un `ordine` all'interno di una dichiarazione `class Meta` nel tuo modello.
> 2. Aggiungere un attributo `queryset` nella tua vista personalizzata basata su classi, specificando un `order_by()`.
> 3. Aggiungere un metodo `get_queryset` alla tua vista personalizzata basata su classi e specificare anche `order_by()`.
>
> Se decidete di usare un `class Meta` per il modello `Author` (probabilmente non è flessibile quanto personalizzare la vista basata su classi, ma abbastanza facile), finirete con qualcosa di simile a questo:
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
> Ovviamente, il campo non deve essere `last_name`: potrebbe essere qualsiasi altro.
>
> Infine, ma non meno importante, dovresti ordinare per un attributo/colonna che in realtà ha un indice (unico o meno) sul tuo database per evitare problemi di prestazioni. Ovviamente, questo non sarà necessario qui (probabilmente stiamo andando troppo avanti con noi stessi con così pochi libri e utenti), ma è qualcosa da tenere a mente per progetti futuri.

La seconda cosa interessante (e non ovvia) nel template è dove visualizziamo il testo dello stato per ciascuna istanza del libro ("disponibile", "manutenzione", ecc.). I lettori attenti noteranno che il metodo `BookInstance.get_status_display()` che usiamo per ottenere il testo dello stato non appare altrove nel codice.

```django
 <p class="{% if copy.status == 'a' %}text-success{% elif copy.status == 'm' %}text-danger{% else %}text-warning{% endif %}">
 \{{ copy.get_status_display }} </p>
```

Questa funzione viene automaticamente creata perché `BookInstance.status` è un [campo di scelte](https://docs.djangoproject.com/en/5.0/ref/models/fields/#choices). Django crea automaticamente un metodo `get_foo_display()` per ogni campo di scelte `foo` in un modello, che può essere utilizzato per ottenere il valore corrente del campo.

## Come si presenta?

A questo punto, dovremmo aver creato tutto il necessario per visualizzare sia le pagine di elenco dei libri che le pagine di dettaglio dei libri. Avvia il server (`python3 manage.py runserver`) e apri il tuo browser a `http://127.0.0.1:8000/`.

> [!WARNING]
> Non fare ancora clic sui collegamenti di dettagli autore o autore — li creerai nella sfida!

Clicca sul collegamento **All books** per visualizzare l'elenco dei libri.

![Pagina Elenco Libri](book_list_page_no_pagination.png)

Poi clicca su un link ad uno dei tuoi libri. Se tutto è configurato correttamente, dovresti vedere qualcosa di simile allo screenshot seguente.

![Pagina Dettaglio Libro](book_detail_page_no_pagination.png)

## Paginazione

Se hai solo pochi record, la nostra pagina di elenco libri sembrerà a posto. Tuttavia, man mano che si entra nelle decine o centinaia di record, la pagina impiegherà progressivamente più tempo per caricarsi (e avrà contenuti troppo lunghi per essere sfogliati in modo sensato). La soluzione a questo problema è aggiungere la paginazione alle tue viste elenco, riducendo il numero di elementi visualizzati su ogni pagina.

Django ha un supporto eccellente integrato per la paginazione. Ancora meglio, questo è incorporato nelle viste generiche basate su classi, quindi non bisogna fare molto per abilitarlo!

### Viste

Aprire **catalog/views.py** e aggiungere la riga `paginate_by` mostrata di seguito.

```python
class BookListView(generic.ListView):
    model = Book
    paginate_by = 10
```

Con questa aggiunta, non appena si avranno più di 10 record, la vista inizierà a paginare i dati che invia al template. Le diverse pagine sono accessibili utilizzando parametri GET — per accedere alla pagina 2 si utilizzerebbe l'URL `/catalog/books/?page=2`.

### Template

Ora che i dati sono paginati, dobbiamo aggiungere il supporto al template per scorrere il set di risultati. Poiché potremmo voler paginare tutte le viste elenco, lo aggiungeremo al template base.

Aprire **/django-locallibrary-tutorial/catalog/templates/_base_generic.html_** e trovare il "content block" (come mostrato di seguito).

```django
{% block content %}{% endblock %}
```

Copiare nel seguente blocco di paginazione immediatamente dopo `{% endblock %}`. Il codice prima controlla se la paginazione è abilitata sulla pagina corrente. Se così è, aggiunge collegamenti _successivo_ e _precedente_ come appropriato (e il numero di pagina corrente).

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

Il `page_obj` è un oggetto [Paginator](https://docs.djangoproject.com/en/5.0/topics/pagination/#paginator-objects) che esisterà se la paginazione è utilizzata sulla pagina corrente. Ti permette di ottenere tutte le informazioni sulla pagina corrente, le pagine precedenti, quante pagine ci sono, ecc.

Usiamo `\{{ request.path }}` per ottenere l'URL della pagina corrente per creare i link di paginazione. Questo è utile perché è indipendente dall'oggetto che stiamo paginando.

Ecco qua!

### Come si presenta?

Lo screenshot qui sotto mostra come appare la paginazione — se non hai inserito più di 10 titoli nel tuo database, puoi testarlo più facilmente abbassando il numero specificato nella riga `paginate_by` nel tuo file **catalog/views.py**. Per ottenere il risultato sottostante, lo abbiamo cambiato in `paginate_by = 2`.

I link di paginazione sono visualizzati sul fondo, con link successivi/precedenti visualizzati a seconda di quale pagina ti trovi.

![Pagina Elenco Libri - paginata](book_list_paginated.png)

## Sfida te stesso

La sfida in questo articolo è creare le viste di dettaglio e elenco degli autori richieste per completare il progetto. Queste dovrebbero essere disponibili ai seguenti URL:

- `catalog/authors/` — L'elenco di tutti gli autori.
- `catalog/author/<id>` — La vista di dettaglio per lo specifico autore con un campo chiave primaria denominato `<id>`

Il codice richiesto per i mapper URL e le viste dovrebbe essere praticamente identico alle viste di elenco e dettaglio `Book` che abbiamo creato sopra. I template saranno diversi ma condivideranno un comportamento simile.

> [!NOTE]
>
> - Una volta che avrai creato il mapper URL per la pagina elenco autori, dovrai anche aggiornare il link **All authors** nel template base.
>   Segui lo [stesso processo](#aggiornare_il_template_base) che abbiamo fatto quando abbiamo aggiornato il link **All books**.
> - Una volta che avrai creato il mapper URL per la pagina di dettaglio autore, dovresti anche aggiornare il [template della vista di dettaglio libro](#creare_il_template_della_visualizzazione_dettaglio) (**/django-locallibrary-tutorial/catalog/templates/catalog/book_detail.html**) in modo che il link dell'autore punti alla tua nuova pagina di dettaglio autore (anziché essere un URL vuoto).
>   Il modo consigliato per farlo è chiamare `get_absolute_url()` sul modello autore come mostrato di seguito.
>
>   ```django
>   <p>
>     <strong>Autore:</strong>
>     <a href="\{{ book.author.get_absolute_url }}">\{{ book.author }}</a>
>   </p>
>   ```

Quando hai finito, le tue pagine dovrebbero assomigliare a qualcosa come gli screenshot qui sotto.

![Pagina Elenco Autori](author_list_page_no_pagination.png)

![Pagina Dettaglio Autore](author_detail_page_no_pagination.png)

## Riepilogo

Congratulazioni, la nostra funzionalità di biblioteca di base è ora completa!

In questo articolo, abbiamo imparato a usare le viste generiche di lista e dettaglio basate su classi e le abbiamo utilizzate per creare pagine per visualizzare i nostri libri e autori. Lungo il percorso, abbiamo imparato a fare pattern matching con le espressioni regolari e come si possono passare dati dagli URL alle viste. Abbiamo anche imparato alcuni trucchi in più per l'utilizzo dei template. Infine, abbiamo mostrato come paginare le viste elenco in modo che i nostri elenchi siano gestibili anche quando abbiamo molti record.

Nei nostri prossimi articoli, estenderemo questa biblioteca per supportare gli account utente e quindi dimostrare l'autenticazione utente, le autorizzazioni, le sessioni e i moduli.

## Vedi anche

- [Viste generiche incorporate basate su classi](https://docs.djangoproject.com/en/5.0/topics/class-based-views/generic-display/) (documentazione Django)
- [Viste di visualizzazione generiche](https://docs.djangoproject.com/en/5.0/ref/class-based-views/generic-display/) (documentazione Django)
- [Introduzione alle viste basate su classi](https://docs.djangoproject.com/en/5.0/topics/class-based-views/intro/) (documentazione Django)
- [Tag e filtri template incorporati](https://docs.djangoproject.com/en/5.0/ref/templates/builtins/) (documentazione Django)
- [Paginazione](https://docs.djangoproject.com/en/5.0/topics/pagination/) (documentazione Django)
- [Effettuare query > Oggetti correlati](https://docs.djangoproject.com/en/5.0/topics/db/queries/#related-objects) (documentazione Django)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Home_page", "Learn_web_development/Extensions/Server-side/Django/Sessions", "Learn_web_development/Extensions/Server-side/Django")}}
