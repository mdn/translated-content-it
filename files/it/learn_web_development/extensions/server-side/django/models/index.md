---
title: "Tutorial Django Parte 3: Utilizzare i modelli"
short-title: "3: Modelli"
slug: Learn_web_development/Extensions/Server-side/Django/Models
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/skeleton_website", "Learn_web_development/Extensions/Server-side/Django/Admin_site", "Learn_web_development/Extensions/Server-side/Django")}}

Questo articolo mostra come definire modelli per il sito web LocalLibrary. Spiega cos'è un modello, come viene dichiarato e alcuni dei principali tipi di campo. Mostra anche brevemente alcuni dei modi principali in cui è possibile accedere ai dati del modello.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/skeleton_website">Tutorial Django Parte 2: Creare un sito scheletro</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        <p>
          Essere in grado di progettare e creare i propri modelli, scegliendo i campi in modo appropriato.
        </p>
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

Le applicazioni web Django accedono e gestiscono i dati tramite oggetti Python chiamati modelli. I modelli definiscono la _struttura_ dei dati memorizzati, includendo i tipi di campo e possibilmente anche la loro dimensione massima, valori predefiniti, opzioni di elenco di selezione, testo di aiuto per la documentazione, testo etichetta per i moduli, ecc. La definizione del modello è indipendente dal database sottostante — è possibile scegliere uno dei diversi come parte delle impostazioni del progetto. Una volta scelto quale database si desidera utilizzare, non è necessario dialogare direttamente con esso — basta scrivere la struttura del modello e altro codice, e Django si occupa di tutta la parte complessa della comunicazione con il database per lei.

Questo tutorial mostra come definire e accedere ai modelli per l'esempio del sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website).

## Progettare i modelli di LocalLibrary

Prima di iniziare a scrivere il codice dei modelli, vale la pena prendere qualche minuto per riflettere su quali dati dobbiamo memorizzare e le relazioni tra i vari oggetti.

Sappiamo che dobbiamo memorizzare informazioni sui libri (titolo, riassunto, autore, lingua scritta, categoria, ISBN) e che potremmo avere più copie disponibili (con un id univoco globale, stato di disponibilità, ecc.). Potremmo aver bisogno di memorizzare più informazioni sull'autore oltre al nome, e potrebbero esserci più autori con lo stesso o simili nomi. Vogliamo essere in grado di ordinare le informazioni in base al titolo del libro, all'autore, alla lingua scritta e alla categoria.

Quando si progettano i propri modelli, ha senso avere modelli separati per ogni "oggetto" (un gruppo di informazioni correlate). In questo caso, gli oggetti evidenti sono libri, istanze di libri e autori.

Potrebbe anche voler usare i modelli per rappresentare opzioni dell'elenco di selezione (ad esempio, come un elenco a discesa di scelte), piuttosto che codificare rigidamente le scelte nel sito web stesso — questo è consigliato quando tutte le opzioni non sono note in anticipo o potrebbero cambiare. Candidati evidenti per i modelli, in questo caso, comprendono il genere del libro (es. Fantascienza, Poesia Francese, ecc.) e la lingua (Inglese, Francese, Giapponese).

Una volta deciso sui nostri modelli e campi, dobbiamo pensare alle relazioni. Django permette di definire relazioni che sono uno a uno (`OneToOneField`), uno a molti (`ForeignKey`) e molti a molti (`ManyToManyField`).

Con questo in mente, il diagramma di associazione UML sottostante mostra i modelli che definiremo in questo caso (come scatole).

![Modello UML di LocalLibrary con molteplicità fissa dell'autore all'interno della classe Libro](local_library_model_uml.svg)

Abbiamo creato modelli per il libro (i dettagli generici del libro), l'istanza del libro (stato delle copie fisiche specifiche del libro disponibili nel sistema) e l'autore. Abbiamo anche deciso di avere un modello per il genere in modo che i valori possano essere creati/selezionati tramite l'interfaccia di amministrazione. Abbiamo deciso di non avere un modello per `BookInstance:status` — abbiamo codificato i valori (`LOAN_STATUS`) perché non ci aspettiamo che questi cambino. All'interno di ciascuna delle scatole, si può vedere il nome del modello, i nomi e i tipi dei campi, e anche i metodi e i loro tipi di ritorno.

Il diagramma mostra anche le relazioni tra i modelli, comprese le loro _molteplicità_. Le molteplicità sono i numeri sul diagramma che mostrano i numeri (massimo e minimo) di ciascun modello che può essere presente nella relazione. Ad esempio, la linea di collegamento tra le scatole mostra che il Libro e un Genere sono correlati. I numeri vicino al modello Genere mostrano che un libro deve avere uno o più Generi (quanti ne vuole), mentre i numeri sull'altro lato della linea accanto al modello Libro mostrano che un Genere può avere zero o molti libri associati.

> [!NOTE]
> La sezione successiva offre una guida di base che spiega come i modelli sono definiti e utilizzati. Mentre la legge, consideri come costruiremo ciascuno dei modelli nel diagramma sopra.

## Introduzione ai modelli

Questa sezione fornisce una breve panoramica su come si definisce un modello e alcuni dei campi e argomenti di campo più importanti.

### Definizione del modello

I modelli sono solitamente definiti in un file **models.py** di un'applicazione. Sono implementati come sottoclassi di `django.db.models.Model` e possono includere campi, metodi e metadati. Il frammento di codice sottostante mostra un modello "tipico", chiamato `MyModelName`:

```python
from django.db import models
from django.urls import reverse

class MyModelName(models.Model):
    """A typical class defining a model, derived from the Model class."""

    # Fields
    my_field_name = models.CharField(max_length=20, help_text='Enter field documentation')
    # …

    # Metadata
    class Meta:
        ordering = ['-my_field_name']

    # Methods
    def get_absolute_url(self):
        """Returns the URL to access a particular instance of MyModelName."""
        return reverse('model-detail-view', args=[str(self.id)])

    def __str__(self):
        """String for representing the MyModelName object (in Admin site etc.)."""
        return self.my_field_name
```

Nelle sezioni sottostanti esploreremo ognuna delle caratteristiche all'interno del modello in dettaglio:

#### Campi

Un modello può avere un numero arbitrario di campi, di qualsiasi tipo — ognuno rappresenta una colonna di dati che desideriamo memorizzare in una delle nostre tabelle del database. Ogni record del database (riga) consisterà in uno di ciascun valore di campo. Diamo un'occhiata all'esempio visto sotto:

```python
my_field_name = models.CharField(max_length=20, help_text='Enter field documentation')
```

Il nostro esempio sopra ha un singolo campo chiamato `my_field_name`, di tipo `models.CharField` — il che significa che questo campo conterrà stringhe di caratteri alfanumerici. I tipi di campo sono assegnati usando classi specifiche, che determinano il tipo di record usato per memorizzare i dati nel database, insieme ai criteri di validazione da utilizzare quando i valori vengono ricevuti da un form HTML (cioè, cosa costituisce un valore valido). I tipi di campo possono anche avere argomenti che specificano ulteriormente come il campo è memorizzato o può essere usato. In questo caso diamo al nostro campo due argomenti:

- `max_length=20` — indica che la lunghezza massima di un valore in questo campo è di 20 caratteri.
- `help_text='Enter field documentation'` — testo utile che può essere visualizzato in un modulo per aiutare gli utenti a capire come viene utilizzato il campo.

Il nome del campo è usato per riferirsi ad esso nelle query e nei modelli.
I campi hanno anche un'etichetta, che è specificata usando l'argomento `verbose_name` (con un valore predefinito di `None`).
Se `verbose_name` non è impostato, l'etichetta è creata dal nome del campo sostituendo eventuali underscore con uno spazio e capitalizzando la prima lettera (ad esempio, il campo `my_field_name` avrebbe un'etichetta predefinita di _My field name_ quando usato nei moduli).

L'ordine in cui i campi sono dichiarati influenzerà il loro ordine predefinito se un modello è reso in un modulo (ad esempio, nel sito di amministrazione), anche se questo può essere sovrascritto.

##### Argomenti comuni dei campi

I seguenti argomenti comuni possono essere utilizzati quando si dichiarano molti/la maggior parte dei diversi tipi di campo:

- [help_text](https://docs.djangoproject.com/en/5.0/ref/models/fields/#help-text): Fornisce un'etichetta di testo per moduli HTML (es. nel sito di amministrazione), come descritto sopra.
- [verbose_name](https://docs.djangoproject.com/en/5.0/ref/models/fields/#verbose-name): Un nome leggibile dall'uomo per il campo usato nelle etichette di campo. Se non specificato, Django dedurrà il nome verboso predefinito dal nome del campo.
- [default](https://docs.djangoproject.com/en/5.0/ref/models/fields/#default): Il valore predefinito per il campo. Questo può essere un valore o un oggetto callable, nel qual caso l'oggetto verrà chiamato ogni volta che viene creato un nuovo record.
- [null](https://docs.djangoproject.com/en/5.0/ref/models/fields/#null): Se `True`, Django memorizzerà i valori vuoti come `NULL` nel database per i campi dove questo è appropriato (un `CharField` memorizzerà invece una stringa vuota). Il valore predefinito è `False`.
- [blank](https://docs.djangoproject.com/en/5.0/ref/models/fields/#blank): Se `True`, il campo può essere lasciato vuoto nei moduli. Il valore predefinito è `False`, il che significa che la validazione del modulo di Django la obbligherà a inserire un valore. Questo è spesso usato con `null=True`, perché se intende permettere valori vuoti, vuole anche che il database sia in grado di rappresentarli correttamente.
- [choices](https://docs.djangoproject.com/en/5.0/ref/models/fields/#choices): Un gruppo di scelte per questo campo. Se questo è fornito, il widget del modulo predefinito corrispondente sarà una casella di selezione con queste scelte anziché il campo di testo standard.
- [unique](https://docs.djangoproject.com/en/5.0/ref/models/fields/#unique):
  Se `True`, assicura che il valore del campo sia unico in tutto il database.
  Questo può essere usato per prevenire la duplicazione dei campi che non possono avere gli stessi valori.
  Il valore predefinito è `False`.
- [primary_key](https://docs.djangoproject.com/en/5.0/ref/models/fields/#primary-key):
  Se `True`, imposta il campo attuale come chiave primaria per il modello (una chiave primaria è una colonna speciale del database designata per identificare univocamente tutti i diversi record della tabella).
  Se nessun campo è specificato come chiave primaria, Django aggiungerà automaticamente un campo a questo scopo.
  Il tipo di campi chiave primaria auto-creati può essere specificato per ogni app in [`AppConfig.default_auto_field`](https://docs.djangoproject.com/en/5.0/ref/applications/#django.apps.AppConfig.default_auto_field) o globalmente nell'impostazione [`DEFAULT_AUTO_FIELD`](https://docs.djangoproject.com/en/5.0/ref/settings/#std:setting-DEFAULT_AUTO_FIELD).

  > [!NOTE]
  > Le app create utilizzando **manage.py** impostano il tipo della chiave primaria su un [BigAutoField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#bigautofield).
  > Può vedere questo nel file **catalog/apps.py** della libreria locale:
  >
  > ```python
  > class CatalogConfig(AppConfig):
  >   default_auto_field = 'django.db.models.BigAutoField'
  > ```

Ci sono molte altre opzioni — può vedere la [lista completa delle opzioni dei campi qui](https://docs.djangoproject.com/en/5.0/ref/models/fields/#field-options).

##### Tipi comuni di campi

La seguente lista descrive alcuni dei tipi di campo più comunemente usati.

- [CharField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#django.db.models.CharField) è usato per definire stringhe di lunghezza fissa di dimensioni medie o corte. Deve specificare la `max_length` dei dati da memorizzare.
- [TextField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#django.db.models.TextField) è utilizzato per grandi stringhe di lunghezza arbitraria. Può specificare una `max_length` per il campo, ma questo è usato solo quando il campo viene visualizzato nei moduli (non è imposto a livello di database).
- [IntegerField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#django.db.models.IntegerField) è un campo per memorizzare valori numerici interi (numeri interi) e per convalidare i valori inseriti come interi nei moduli.
- [DateField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#datefield) e [DateTimeField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#datetimefield) sono usati per memorizzare/rappresentare date e informazioni su date/ora (come oggetti Python `datetime.date` e `datetime.datetime`, rispettivamente). Questi campi possono inoltre dichiarare i parametri (mutuamente esclusivi) `auto_now=True` (per impostare il campo alla data corrente ogni volta che il modello è salvato), `auto_now_add` (per impostare la data solo quando il modello viene creato per la prima volta) e `default` (per impostare una data predefinita che l'utente può sovrascrivere).
- [EmailField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#emailfield) è usato per memorizzare e convalidare indirizzi email.
- [FileField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#filefield) e [ImageField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#imagefield) sono usati per caricare file e immagini rispettivamente (il `ImageField` aggiunge una validazione addizionale in modo che il file caricato sia un'immagine). Questi hanno parametri per definire come e dove i file caricati sono memorizzati.
- [AutoField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#autofield) è un tipo speciale di `IntegerField` che incrementa automaticamente. Una chiave primaria di questo tipo è automaticamente aggiunta al suo modello se non ne specifica esplicitamente una.
- [ForeignKey](https://docs.djangoproject.com/en/5.0/ref/models/fields/#foreignkey) è usato per specificare una relazione uno a molti con un altro modello di database (es. un'auto ha un produttore, ma un produttore può fabbricare molte auto). Il lato "uno" della relazione è il modello che contiene la "chiave" (i modelli che contengono una "chiave esterna" che si riferisce a quella "chiave" sono sul lato "molti" di tale relazione).
- [ManyToManyField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#manytomanyfield) è usato per specificare una relazione molti a molti (es. un libro può avere diversi generi, e ciascun genere può contenere diversi libri). Nella nostra app libreria useremo questi molto similmente ai `ForeignKeys`, ma possono essere usati in modi più complicati per descrivere le relazioni tra gruppi. Questi hanno il parametro `on_delete` per definire cosa succede quando il record associato viene eliminato (es. un valore di `models.SET_NULL` imposterebbe il valore su `NULL`).

Ci sono molti altri tipi di campi, inclusi campi per diversi tipi di numeri (grandi interi, piccoli interi, float), booleani, URL, slug, id unici, e altre informazioni "relative al tempo" (durata, orario, ecc.). Può vedere la [lista completa qui](https://docs.djangoproject.com/en/5.0/ref/models/fields/#field-types).

#### Metadati

Può dichiarare metadati a livello di modello per il suo Modello dichiarando `class Meta`, come mostrato.

```python
class Meta:
    ordering = ['-my_field_name']
```

Una delle funzionalità più utili di questi metadati è controllare l'_ordinamento predefinito_ dei record restituiti quando si interroga il tipo di modello. Può fare ciò specificando l'ordine di confronto in un elenco di nomi di campo all'attributo `ordering`, come mostrato sopra. L'ordinamento dipenderà dal tipo di campo (i campi carattere sono ordinati alfabeticamente, mentre i campi data sono ordinati in ordine cronologico). Come mostrato sopra, può prefissare il nome del campo con un simbolo meno (-) per invertire l'ordine di ordinamento.

Quindi, come esempio, se scegliessimo di ordinare i libri in questo modo per impostazione predefinita:

```python
ordering = ['title', '-publish_date']
```

i libri sarebbero ordinati alfabeticamente per titolo, da A-Z, e poi per data di pubblicazione all'interno di ogni titolo, dal più recente al meno recente.

Un altro attributo comune è `verbose_name`, un nome verboso per la classe in forma singolare e plurale:

```python
verbose_name = 'BetterName'
```

I metadati della classe possono essere usati per creare e applicare nuovi "permessi di accesso" per il modello (i permessi predefiniti vengono applicati automaticamente), permettere l'ordinamento basato su un altro campo, definire [vincoli](https://docs.djangoproject.com/en/5.0/ref/models/constraints/) sui possibili valori di dati che possono essere memorizzati, o dichiarare che la classe è "astratta" (una classe base per cui non può creare record, e sarà invece derivata per creare altri modelli).

Molte delle altre opzioni dei metadati controllano quale database deve essere usato per il modello e come i dati sono memorizzati (questi sono realmente utili solo se è necessario mappare un modello a un database esistente).

La lista completa delle opzioni dei metadati è disponibile qui: [Opzioni dei metadati del modello](https://docs.djangoproject.com/en/5.0/ref/models/options/) (documentazione Django).

#### Metodi

Un modello può anche avere metodi.

**Minimamente, in ogni modello dovrebbe definire il metodo standard della classe Python `__str__()` per restituire una stringa leggibile per ciascun oggetto.** Questa stringa è usata per rappresentare singoli record nel sito di amministrazione (e ovunque altro sia necessario fare riferimento a un'istanza del modello). Spesso questo restituirà un campo titolo o nome dal modello.

```python
def __str__(self):
    return self.my_field_name
```

Un altro metodo comune da includere nei modelli Django è `get_absolute_url()`, che restituisce un URL per visualizzare record di modelli individuali sul sito web (se definisce questo metodo, Django aggiungerà automaticamente un pulsante "Visualizza sul sito" agli schermi di modifica dei record del modello nel sito di amministrazione). Un pattern tipico per `get_absolute_url()` è mostrato di seguito.

```python
def get_absolute_url(self):
    """Returns the URL to access a particular instance of the model."""
    return reverse('model-detail-view', args=[str(self.id)])
```

> [!NOTE]
> Supponendo che userà URL come `/my-application/my-model-name/2` per visualizzare record individuali per il suo modello (dove "2" è l'`id` per un particolare record), sarà necessario creare un mappeggiatore URL per far passare la risposta e l'id a una "vista dettagliata del modello" (che farà il lavoro richiesto per visualizzare il record). La funzione `reverse()` sopra è in grado di "invertire" il suo mappeggiatore URL (nel caso sopra chiamato _'model-detail-view'_) per creare un URL di formato corretto.
>
> Ovviamente, per far funzionare questo, deve comunque scrivere la mappatura URL, la vista e il modello!

Può anche definire qualsiasi altro metodo desideri, e chiamarli dal suo codice o dai modelli (a condizione che non prendano parametri).

### Gestione dei modelli

Una volta definiti le sue classi modello, può utilizzarle per creare, aggiornare o eliminare record e per eseguire query per ottenere tutti i record o sottoinsiemi particolari di record. Le mostreremo come fare ciò nel tutorial quando definiremo le nostre viste, ma qui c'è un breve riassunto.

#### Creare e modificare record

Per creare un record, può definire un'istanza del modello e poi chiamare `save()`.

```python
# Create a new record using the model's constructor.
record = MyModelName(my_field_name="Instance #1")

# Save the object into the database.
record.save()
```

> [!NOTE]
> Se non ha dichiarato alcun campo come `primary_key`, al nuovo record verrà assegnato automaticamente uno, con il nome del campo `id`. Potrebbe interrogare questo campo dopo aver salvato il record sopra, e avrebbe un valore di 1.

Può accedere ai campi in questo nuovo record usando la sintassi del punto, e cambiare i valori. Deve chiamare `save()` per memorizzare i valori modificati nel database.

```python
# Access model field values using Python attributes.
print(record.id) # should return 1 for the first record.
print(record.my_field_name) # should print 'Instance #1'

# Change record by modifying the fields, then calling save().
record.my_field_name = "New Instance Name"
record.save()
```

#### Ricercare record

Può cercare record che corrispondono a determinati criteri usando l'attributo `objects` del modello (fornito dalla classe base).

> [!NOTE]
> Spiegare come cercare i record usando nomi di modelli e campi "astratti" può essere un po' confuso. Nella discussione sottostante, ci riferiremo a un modello `Book` con i campi `title` e `genre`, dove il genere è anche un modello con un singolo campo `name`.

Possiamo ottenere tutti i record per un modello come un `QuerySet`, usando `objects.all()`. Il `QuerySet` è un oggetto iterabile, il che significa che contiene un numero di oggetti attraverso cui possiamo iterare/ciclare.

```python
all_books = Book.objects.all()
```

Il metodo `filter()` di Django ci permette di filtrare il `QuerySet` restituito per far corrispondere un campo **testo** o **numerico** contro particolari criteri. Ad esempio, per filtrare i libri che contengono "wild" nel titolo e poi contarli, potremmo fare quanto segue:

```python
wild_books = Book.objects.filter(title__contains='wild')
number_wild_books = wild_books.count()
```

I campi da confrontare e il tipo di confronto sono definiti nel nome del parametro del filtro, usando il formato: `field_name__match_type` (noti il doppio underscore tra `title` e `contains` sopra). Sopra stiamo filtrando il `title` con una corrispondenza sensibile al case. Ci sono molti altri tipi di confronti che può fare: `icontains` (case insensitive), `iexact` (corrispondenza esatta case insensitive), `exact` (corrispondenza esatta sensibile al case) e `in`, `gt` (maggiore di), `startswith`, ecc. La [lista completa è qui](https://docs.djangoproject.com/en/5.0/ref/models/querysets/#field-lookups).

In alcuni casi, dovrà filtrare su un campo che definisce una relazione uno-a-molti con un altro modello (es. una `ForeignKey`). In questo caso, può "indicizzare" i campi all'interno del modello correlato con ulteriori doppie sottolineature.
Quindi, per esempio, per filtrare i libri con un modello di genere specifico, sarà necessario indicizzare attraverso il campo `genre`, come mostrato di seguito:

```python
# Will match on: Fiction, Science fiction, non-fiction etc.
books_containing_genre = Book.objects.filter(genre__name__icontains='fiction')
```

> [!NOTE]
> Può usare gli underscore (`__`) per navigare attraverso quanti livelli di relazioni (`ForeignKey`/`ManyToManyField`) desidera.
> Ad esempio, un `Book` che aveva diversi tipi, definiti usando una ulteriore relazione "cover", potrebbe avere un nome di parametro: `type__cover__name__exact='hard'.`

Può fare molto di più con le query, inclusi ricerche inverse da modelli correlati, concatenare filtri, restituire un set ridotto di valori, ecc. Per ulteriori informazioni, guardi [Fare query](https://docs.djangoproject.com/en/5.0/topics/db/queries/) (Django Docs).

## Definire i Modelli di LocalLibrary

In questa sezione inizieremo a definire i modelli per la libreria. Apra `models.py` (in /django-locallibrary-tutorial/catalog/). Il boilerplate in cima alla pagina importa il modulo _models_, che contiene la classe base del modello `models.Model` da cui i nostri modelli ereditano.

```python
from django.db import models

# Create your models here.
```

### Modello Genre

Copi il codice del modello `Genre` mostrato di seguito e lo incolli nella parte inferiore del suo file `models.py`. Questo modello è utilizzato per memorizzare informazioni sulla categoria del libro — ad esempio se è finzione o non finzione, romanzo o storia militare, ecc.
Come menzionato sopra, abbiamo creato il genere come un modello piuttosto che come testo libero o un elenco di selezione in modo che i valori possibili possano essere gestiti tramite il database piuttosto che essere codificati rigidamente.

```python
from django.urls import reverse # Used in get_absolute_url() to get URL for specified ID

from django.db.models import UniqueConstraint # Constrains fields to unique values
from django.db.models.functions import Lower # Returns lower cased value of field

class Genre(models.Model):
    """Model representing a book genre."""
    name = models.CharField(
        max_length=200,
        unique=True,
        help_text="Enter a book genre (e.g. Science Fiction, French Poetry etc.)"
    )

    def __str__(self):
        """String for representing the Model object."""
        return self.name

    def get_absolute_url(self):
        """Returns the url to access a particular genre instance."""
        return reverse('genre-detail', args=[str(self.id)])

    class Meta:
        constraints = [
            UniqueConstraint(
                Lower('name'),
                name='genre_name_case_insensitive_unique',
                violation_error_message = "Genre already exists (case insensitive match)"
            ),
        ]
```

Il modello ha un solo campo `CharField` (`name`), usato per descrivere il genere (questo è limitato a 200 caratteri e ha un po' di `help_text`).
Abbiamo impostato questo campo come univoco (`unique=True`) perché dovrebbe esserci solo un record per ciascun genere.

Dopo il campo, dichiariamo un metodo `__str__()`, che restituisce il nome del genere definito da un particolare record. Non è stato definito alcun nome verboso, quindi l'etichetta del campo sarà `Name` quando usata nei moduli.
Poi dichiariamo il metodo `get_absolute_url()`, che restituisce un URL che può essere usato per accedere a un record di dettaglio per questo modello (per far funzionare questo, dovremo definire una mappatura URL che abbia il nome `genre-detail` e definire una vista e un template associati).

Impostare `unique=True` sul campo sopra previene che i generi siano creati con _esattamente_ lo stesso nome, ma non le varianti come "fantasy", "Fantasy", o anche "FaNtAsY".
L'ultima parte della definizione del modello utilizza un'opzione [`constraints`](https://docs.djangoproject.com/en/5.0/ref/models/options/#constraints) sui [metadati](#metadati) del modello per specificare che il minuscolo del valore nel campo `name` deve essere univoco nel database e visualizzare il `violation_error_message` stringa se non lo è.
Qui non abbiamo bisogno di fare altro, ma può definire più vincoli contro un campo o campi.
Per ulteriori informazioni, guardi il [Riferimento sui vincoli](https://docs.djangoproject.com/en/5.0/ref/models/constraints/), inclusi [`UniqueConstraint()`](https://docs.djangoproject.com/en/5.0/ref/models/constraints/#uniqueconstraint) (e [`Lower()`](https://docs.djangoproject.com/en/5.0/ref/models/database-functions/#lower)).

### Modello Book

Copi il modello `Book` qui sotto e lo incolli nuovamente in fondo al suo file. Il modello `Book` rappresenta tutte le informazioni su un libro disponibile in un senso generale, ma non un particolare "istanza" o "copia" fisica disponibile per il prestito.

Il modello utilizza un `CharField` per rappresentare il `title` e l'`isbn` del libro.
Per `isbn`, noti come il primo parametro senza nome imposta in modo esplicito l'etichetta come "ISBN" (altrimenti sarebbe predefinita "Isbn"). Impostiamo anche il parametro `unique` come `true` per garantire che tutti i libri abbiano un ISBN univoco (il parametro unique rende il valore del campo univoco a livello globale in una tabella).
A differenza dell'`isbn` (e del nome del genere), il `title` non è impostato per essere univoco, poiché è possibile che libri diversi abbiano lo stesso nome.
Il modello utilizza `TextField` per il `summary`, poiché questo testo può necessitare di essere piuttosto lungo.

```python
class Book(models.Model):
    """Model representing a book (but not a specific copy of a book)."""
    title = models.CharField(max_length=200)
    author = models.ForeignKey('Author', on_delete=models.RESTRICT, null=True)
    # Foreign Key used because book can only have one author, but authors can have multiple books.
    # Author as a string rather than object because it hasn't been declared yet in file.

    summary = models.TextField(
        max_length=1000, help_text="Enter a brief description of the book")
    isbn = models.CharField('ISBN', max_length=13,
                            unique=True,
                            help_text='13 Character <a href="https://www.isbn-international.org/content/what-isbn'
                                      '">ISBN number</a>')

    # ManyToManyField used because genre can contain many books. Books can cover many genres.
    # Genre class has already been defined so we can specify the object above.
    genre = models.ManyToManyField(
        Genre, help_text="Select a genre for this book")

    def __str__(self):
        """String for representing the Model object."""
        return self.title

    def get_absolute_url(self):
        """Returns the URL to access a detail record for this book."""
        return reverse('book-detail', args=[str(self.id)])
```

Il genere è un `ManyToManyField`, quindi un libro può avere più generi e un genere può avere molti libri. L'autore è dichiarato come `ForeignKey`, quindi ciascun libro avrà solo un autore, ma un autore può avere molti libri (in pratica un libro potrebbe avere più autori, ma non in questa implementazione!)

In entrambi i tipi di campo, la classe del modello correlato viene dichiarata come primo parametro senza nome utilizzando la classe del modello o una stringa contenente il nome del modello correlato. Deve usare il nome del modello come stringa se la classe associata non è ancora stata definita in questo file prima che venga referenziata! Gli altri parametri di interesse nel campo `author` sono `null=True`, che consente al database di memorizzare un valore `Null` se non viene selezionato alcun autore, e `on_delete=models.RESTRICT`, che impedirà la cancellazione dell'autore associato al libro se è referenziato da qualsiasi libro.

> [!WARNING]
> Per impostazione predefinita `on_delete=models.CASCADE`, il che significa che se l'autore venisse eliminato, questo libro sarebbe eliminato anche! Usciamo `RESTRICT` qui, ma potremmo anche usare `PROTECT` per prevenire che l'autore sia eliminato mentre un libro lo utilizza o `SET_NULL` per impostare l'autore del libro su `Null` se il record viene eliminato.

Il modello definisce anche `__str__()`, usando il campo `title` del libro per rappresentare un record `Book`. L'ultimo metodo, `get_absolute_url()`, restituisce un URL che può essere usato per accedere a un record di dettaglio per questo modello (dovremo definire una mappatura URL che abbia il nome `book-detail`, e definire una vista e un modello associati).

### Modello BookInstance

Successivamente, copi il modello `BookInstance` (mostrato di seguito) sotto gli altri modelli. Il `BookInstance` rappresenta una copia specifica di un libro che qualcuno potrebbe prendere in prestito e include informazioni su se la copia è disponibile o in quale data è prevista di ritorno, dettagli sull' "imprint" o versione, e un id univoco per il libro nella biblioteca.

Alcuni dei campi e dei metodi saranno ora familiari. Il modello utilizza:

- `ForeignKey` per identificare il `Book` associato (ogni libro può avere molte copie, ma una copia può avere solo un `Book`). La chiave specifica `on_delete=models.RESTRICT` per assicurarsi che il `Book` non possa essere eliminato mentre viene referenziato da un `BookInstance`.
- `CharField` per rappresentare l'imprint (rilascio specifico) del libro.

```python
import uuid # Required for unique book instances

class BookInstance(models.Model):

    """Model representing a specific copy of a book (i.e. that can be borrowed from the library)."""
    id = models.UUIDField(primary_key=True, default=uuid.uuid4,
                          help_text="Unique ID for this particular book across whole library")
    book = models.ForeignKey('Book', on_delete=models.RESTRICT, null=True)
    imprint = models.CharField(max_length=200)
    due_back = models.DateField(null=True, blank=True)

    LOAN_STATUS = (
        ('m', 'Maintenance'),
        ('o', 'On loan'),
        ('a', 'Available'),
        ('r', 'Reserved'),
    )

    status = models.CharField(
        max_length=1,
        choices=LOAN_STATUS,
        blank=True,
        default='m',
        help_text='Book availability',
    )

    class Meta:
        ordering = ['due_back']

    def __str__(self):
        """String for representing the Model object."""
        return f'{self.id} ({self.book.title})'
```

Dichiariamo inoltre alcuni nuovi tipi di campo:

- `UUIDField` è usato per il campo `id` per impostarlo come il `primary_key` per questo modello.
  Questo tipo di campo assegna un valore univoco globale per ciascuna istanza (una per ogni libro che può trovare nella biblioteca).
- `DateField` è usato per la data `due_back` (alla quale si prevede che il libro diventi disponibile dopo essere stato preso in prestito o in manutenzione). Questo valore può essere `blank` o `null` (necessario per quando il libro è disponibile). I metadati del modello (`Class Meta`) usano questo campo per ordinare i record quando vengono restituiti in una query.
- `status` è un `CharField` che definisce una lista di scelte/selezioni. Come può vedere, definiamo una tupla contenente tuple di coppie chiave-valore e la passiamo all'argomento delle scelte. Il valore in una coppia chiave/valore è un valore di visualizzazione che un utente può selezionare, mentre le chiavi sono i valori che vengono effettivamente salvati se viene selezionata l'opzione. Abbiamo anche impostato un valore predefinito di 'm' (manutenzione) poiché i libri saranno inizialmente creati non disponibili prima che vengano immagazzinati sugli scaffali.

Il metodo `__str__()` rappresenta l'oggetto `BookInstance` utilizzando una combinazione del suo id univoco e il titolo del `Book` associato.

> [!NOTE]
> Un po' di Python:
>
> - A partire da Python 3.6, può usare la sintassi dell'interpolazione di stringa (nota anche come f-strings): `f'{self.id} ({self.book.title})'`.
> - Nelle versioni precedenti di questo tutorial, stavamo usando una sintassi di [stringa formattata](https://peps.python.org/pep-3101/), che è anche un modo valido per formattare le stringhe in Python (es. `'{0} ({1})'.format(self.id,self.book.title)`).

### Modello Author

Copia il modello `Author` (mostrato di seguito) sotto il codice esistente in **models.py**.

```python
class Author(models.Model):
    """Model representing an author."""
    first_name = models.CharField(max_length=100)
    last_name = models.CharField(max_length=100)
    date_of_birth = models.DateField(null=True, blank=True)
    date_of_death = models.DateField('Died', null=True, blank=True)

    class Meta:
        ordering = ['last_name', 'first_name']

    def get_absolute_url(self):
        """Returns the URL to access a particular author instance."""
        return reverse('author-detail', args=[str(self.id)])

    def __str__(self):
        """String for representing the Model object."""
        return f'{self.last_name}, {self.first_name}'
```

Tutti i campi/metodi dovrebbero ora essere familiari. Il modello definisce un autore come avente un nome, cognome, e date di nascita e morte (entrambe opzionali). Specifica che per impostazione predefinita `__str__()` restituisce il nome in ordine _cognome_, _nome_. Il metodo `get_absolute_url()` inverte la mappatura URL `author-detail` per ottenere l'URL per visualizzare un autore individuale.

## Riavvi la migrazione del database

Ora tutti i modelli sono stati creati. Ora riavvii la migrazione del database per aggiungerli al suo database.

```bash
python3 manage.py makemigrations
python3 manage.py migrate
```

## Modello Language — sfida

Immagini che un benefattore locale doni un certo numero di nuovi libri scritti in un'altra lingua (diciamo, il Farsi). La sfida è trovare il modo migliore di rappresentarli nel nostro sito web della biblioteca, e quindi aggiungerli ai modelli.

Alcune cose da considerare:

- La "lingua" dovrebbe essere associata a un `Book`, `BookInstance` o qualche altro oggetto?
- Le diverse lingue dovrebbero essere rappresentate usando un modello, un campo di testo libero, o una lista di selezione codificata?

Dopo aver deciso, aggiunga il campo. Può vedere cosa abbiamo deciso su GitHub [qui](https://github.com/mdn/django-locallibrary-tutorial/blob/main/catalog/models.py).

Non dimentichi che dopo una modifica al suo modello, dovrebbe di nuovo riavviare le migrazioni del database per aggiungere le modifiche.

```bash
python3 manage.py makemigrations
python3 manage.py migrate
```

## Riassunto

In questo articolo abbiamo imparato come i modelli sono definiti e quindi usato queste informazioni per progettare e implementare modelli appropriati per il sito web _LocalLibrary_.

A questo punto ci distoglieremo brevemente dalla creazione del sito e controlleremo il _sito di amministrazione Django_. Questo sito ci permetterà di aggiungere alcuni dati alla libreria, che possiamo quindi visualizzare usando le nostre (ancora da creare) viste e modelli.

## Vedi anche

- [Scrivere la sua prima app Django, parte 2](https://docs.djangoproject.com/en/5.0/intro/tutorial02/) (documentazione Django)
- [Fare query](https://docs.djangoproject.com/en/5.0/topics/db/queries/) (documentazione Django)
- [Riferimento API QuerySet](https://docs.djangoproject.com/en/5.0/ref/models/querysets/) (documentazione Django)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/skeleton_website", "Learn_web_development/Extensions/Server-side/Django/Admin_site", "Learn_web_development/Extensions/Server-side/Django")}}
