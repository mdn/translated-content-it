---
title: "Tutorial Django - Parte 3: Uso dei modelli"
short-title: "3: Modelli"
slug: Learn_web_development/Extensions/Server-side/Django/Models
l10n:
  sourceCommit: 26fb7eaa7b398a35c2463fa15ab6ccfa46a9e06d
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/skeleton_website", "Learn_web_development/Extensions/Server-side/Django/Admin_site", "Learn_web_development/Extensions/Server-side/Django")}}

Questo articolo mostra come definire i modelli per il sito web LocalLibrary. Spiega che cos'è un modello, come viene dichiarato e alcuni dei principali tipi di campo. Mostra inoltre brevemente alcuni dei principali modi per accedere ai dati dei modelli.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/skeleton_website">Tutorial Django - Parte 2: Creazione dello scheletro di un sito web</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        <p>
          Essere in grado di progettare e creare modelli personali, scegliendo i campi in modo appropriato.
        </p>
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

Le applicazioni web Django accedono ai dati e li gestiscono attraverso oggetti Python denominati modelli. I modelli definiscono la _struttura_ dei dati archiviati, inclusi i _tipi_ di campo e, potenzialmente, anche la loro dimensione massima, i valori predefiniti, le opzioni delle liste di selezione, il testo di aiuto per la documentazione, il testo delle etichette per i moduli e così via. La definizione del modello è indipendente dal database sottostante: nelle impostazioni del progetto è possibile scegliere uno tra diversi database. Dopo aver scelto il database da utilizzare, non è necessario interagirvi direttamente: basta scrivere la struttura del modello e altro codice, mentre Django gestisce tutto il lavoro di comunicazione con il database.

Questo tutorial mostra come definire e accedere ai modelli dell'esempio del sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website).

## Progettazione dei modelli LocalLibrary

Prima di iniziare a scrivere il codice dei modelli, è utile dedicare qualche minuto a pensare a quali dati devono essere archiviati e alle relazioni tra i diversi oggetti.

Sappiamo di dover archiviare informazioni sui libri (titolo, riassunto, autore, lingua di scrittura, categoria, ISBN) e che potrebbero essere disponibili più copie (con id univoco globale, stato di disponibilità e così via). Potrebbe essere necessario archiviare più informazioni sull'autore oltre al semplice nome, e potrebbero esistere più autori con nomi uguali o simili. Si desidera poter ordinare le informazioni in base al titolo del libro, all'autore, alla lingua di scrittura e alla categoria.

Durante la progettazione dei modelli, è opportuno disporre di modelli separati per ogni "oggetto" (un gruppo di informazioni correlate). In questo caso, gli oggetti evidenti sono libri, copie dei libri e autori.

Si potrebbero inoltre utilizzare modelli per rappresentare le opzioni di una lista di selezione (ad esempio, un elenco a discesa di scelte), anziché codificare le scelte direttamente nel sito web. Ciò è consigliato quando tutte le opzioni non sono note in anticipo o potrebbero cambiare. Candidati evidenti per i modelli, in questo caso, includono il genere del libro (ad esempio, fantascienza, poesia francese e così via) e la lingua (inglese, francese, giapponese).

Dopo aver deciso i modelli e i campi, occorre considerare le relazioni. Django consente di definire relazioni uno a uno (`OneToOneField`), uno a molti (`ForeignKey`) e molti a molti (`ManyToManyField`).

Tenendo presente ciò, il diagramma di associazione UML seguente mostra i modelli che verranno definiti in questo caso (come riquadri).

![UML del modello LocalLibrary con molteplicità fissa di Author all'interno della classe Book](local_library_model_uml.svg)

Sono stati creati modelli per il libro (i dettagli generici del libro), la copia del libro (lo stato di specifiche copie fisiche del libro disponibili nel sistema) e l'autore. Si è anche deciso di avere un modello per il genere, in modo che i valori possano essere creati/selezionati tramite l'interfaccia di amministrazione. Si è deciso di non avere un modello per `BookInstance:status`: i valori (`LOAN_STATUS`) sono stati codificati direttamente perché non si prevede che cambino. All'interno di ciascun riquadro sono visibili il nome del modello, i nomi e i tipi dei campi, nonché i metodi e i relativi tipi di ritorno.

Il diagramma mostra inoltre le relazioni tra i modelli, incluse le rispettive _molteplicità_. Le molteplicità sono i numeri nel diagramma che indicano il numero (massimo e minimo) di ogni modello che può essere presente nella relazione. Ad esempio, la linea di collegamento tra i riquadri mostra che Book e Genre sono correlati. I numeri vicini al modello Genre indicano che un libro deve avere uno o più generi (tutti quelli desiderati), mentre i numeri all'altra estremità della linea, accanto al modello Book, indicano che un genere può avere zero o molti libri associati.

> [!NOTE]
> La sezione successiva fornisce un'introduzione di base che spiega come i modelli vengono definiti e utilizzati. Durante la lettura, considera come verrà costruito ciascuno dei modelli nel diagramma precedente.

## Introduzione ai modelli

Questa sezione fornisce una breve panoramica su come viene definito un modello e su alcuni dei campi e argomenti di campo più importanti.

### Definizione del modello

I modelli vengono solitamente definiti nel file **models.py** di un'applicazione. Sono implementati come sottoclassi di `django.db.models.Model` e possono includere campi, metodi e metadati. Il frammento di codice seguente mostra un modello "tipico", denominato `MyModelName`:

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

Nelle sezioni seguenti verranno esaminate in dettaglio tutte le funzionalità all'interno del modello:

#### Campi

Un modello può avere un numero arbitrario di campi, di qualsiasi tipo: ciascuno rappresenta una colonna di dati che si desidera archiviare in una delle tabelle del database. Ogni record del database (riga) sarà costituito da un valore per ciascun campo. Vediamo l'esempio seguente:

```python
my_field_name = models.CharField(max_length=20, help_text='Enter field documentation')
```

L'esempio precedente ha un singolo campo denominato `my_field_name`, di tipo `models.CharField`, il che significa che questo campo conterrà stringhe di caratteri alfanumerici. I tipi di campo vengono assegnati mediante classi specifiche, che determinano il tipo di record utilizzato per archiviare i dati nel database, insieme ai criteri di validazione da usare quando si ricevono valori da un modulo HTML (ovvero, cosa costituisce un valore valido). I tipi di campo possono anche accettare argomenti che specificano ulteriormente come il campo viene archiviato o può essere utilizzato. In questo caso vengono forniti due argomenti al campo:

- `max_length=20`: indica che la lunghezza massima di un valore in questo campo è di 20 caratteri.
- `help_text='Enter field documentation'`: testo utile che può essere visualizzato in un modulo per aiutare gli utenti a comprendere come viene utilizzato il campo.

Il nome del campo viene usato per farvi riferimento nelle query e nei template.
I campi hanno anche un'etichetta, specificata tramite l'argomento `verbose_name` (con valore predefinito `None`).
Se `verbose_name` non è impostato, l'etichetta viene creata a partire dal nome del campo sostituendo gli eventuali trattini bassi con uno spazio e rendendo maiuscola la prima lettera (ad esempio, il campo `my_field_name` avrebbe l'etichetta predefinita _My field name_ quando utilizzato nei moduli).

L'ordine in cui vengono dichiarati i campi influirà sul loro ordine predefinito quando un modello viene reso in un modulo, ad esempio nel sito di amministrazione, anche se questo può essere sovrascritto.

##### Argomenti di campo comuni

I seguenti argomenti comuni possono essere utilizzati quando si dichiarano molti o la maggior parte dei diversi tipi di campo:

- [help_text](https://docs.djangoproject.com/en/5.0/ref/models/fields/#help-text): fornisce un'etichetta di testo per i moduli HTML, ad esempio nel sito di amministrazione, come descritto in precedenza.
- [verbose_name](https://docs.djangoproject.com/en/5.0/ref/models/fields/#verbose-name): un nome leggibile per il campo, utilizzato nelle etichette dei campi. Se non specificato, Django deduce il nome descrittivo predefinito dal nome del campo.
- [default](https://docs.djangoproject.com/en/5.0/ref/models/fields/#default): il valore predefinito del campo. Può essere un valore o un oggetto chiamabile, nel qual caso l'oggetto verrà chiamato ogni volta che viene creato un nuovo record.
- [null](https://docs.djangoproject.com/en/5.0/ref/models/fields/#null): se `True`, Django archivierà i valori vuoti come `NULL` nel database per i campi in cui ciò è appropriato (un `CharField` archivierà invece una stringa vuota). Il valore predefinito è `False`.
- [blank](https://docs.djangoproject.com/en/5.0/ref/models/fields/#blank): se `True`, il campo può essere vuoto nei moduli. Il valore predefinito è `False`, il che significa che la validazione dei moduli di Django obbligherà a inserire un valore. Spesso viene utilizzato insieme a `null=True`, poiché se si intendono consentire valori vuoti, si vuole anche che il database sia in grado di rappresentarli in modo appropriato.
- [choices](https://docs.djangoproject.com/en/5.0/ref/models/fields/#choices): un gruppo di scelte per questo campo. Se fornito, il widget del modulo predefinito corrispondente sarà una casella di selezione con queste scelte anziché il campo di testo standard.
- [unique](https://docs.djangoproject.com/en/5.0/ref/models/fields/#unique):
  Se `True`, garantisce che il valore del campo sia univoco nel database.
  Può essere utilizzato per impedire la duplicazione di campi che non possono avere gli stessi valori.
  Il valore predefinito è `False`.
- [primary_key](https://docs.djangoproject.com/en/5.0/ref/models/fields/#primary-key):
  Se `True`, imposta il campo corrente come chiave primaria per il modello (una chiave primaria è una speciale colonna del database designata per identificare in modo univoco tutti i diversi record della tabella).
  Se nessun campo viene specificato come chiave primaria, Django aggiunge automaticamente un campo a questo scopo.
  Il tipo dei campi chiave primaria creati automaticamente può essere specificato per ogni app in [`AppConfig.default_auto_field`](https://docs.djangoproject.com/en/5.0/ref/applications/#django.apps.AppConfig.default_auto_field) o globalmente nell'impostazione [`DEFAULT_AUTO_FIELD`](https://docs.djangoproject.com/en/5.0/ref/settings/#std:setting-DEFAULT_AUTO_FIELD).

  > [!NOTE]
  > Le app create mediante **manage.py** impostano il tipo della chiave primaria su [BigAutoField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#bigautofield).
  > Questo è visibile nel file **catalog/apps.py** della libreria locale:
  >
  > ```python
  > class CatalogConfig(AppConfig):
  >   default_auto_field = 'django.db.models.BigAutoField'
  > ```

Esistono molte altre opzioni: è possibile consultare [l'elenco completo delle opzioni di campo qui](https://docs.djangoproject.com/en/5.0/ref/models/fields/#field-options).

##### Tipi di campo comuni

L'elenco seguente descrive alcuni dei tipi di campo più comunemente utilizzati.

- [CharField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#django.db.models.CharField) viene utilizzato per definire stringhe a lunghezza fissa di dimensione breve o media. È necessario specificare il `max_length` dei dati da archiviare.
- [TextField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#django.db.models.TextField) viene utilizzato per stringhe grandi di lunghezza arbitraria. È possibile specificare un `max_length` per il campo, ma questo viene usato solo quando il campo è visualizzato nei moduli, non viene applicato a livello del database.
- [IntegerField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#django.db.models.IntegerField) è un campo per archiviare valori interi e per validare come interi i valori inseriti nei moduli.
- [DateField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#datefield) e [DateTimeField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#datetimefield) vengono utilizzati per archiviare/rappresentare date e informazioni di data/ora, rispettivamente come oggetti Python `datetime.date` e `datetime.datetime`. Questi campi possono inoltre dichiarare i parametri mutuamente esclusivi `auto_now=True` (per impostare il campo alla data corrente ogni volta che il modello viene salvato), `auto_now_add` (per impostare la data solo quando il modello viene creato per la prima volta) e `default` (per impostare una data predefinita che può essere sovrascritta dall'utente).
- [EmailField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#emailfield) viene utilizzato per archiviare e validare indirizzi email.
- [FileField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#filefield) e [ImageField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#imagefield) vengono utilizzati rispettivamente per caricare file e immagini (`ImageField` aggiunge una validazione ulteriore per verificare che il file caricato sia un'immagine). Dispongono di parametri per definire come e dove vengono archiviati i file caricati.
- [AutoField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#autofield) è un tipo speciale di `IntegerField` che si incrementa automaticamente. Una chiave primaria di questo tipo viene aggiunta automaticamente al modello se non ne viene specificata esplicitamente una.
- [ForeignKey](https://docs.djangoproject.com/en/5.0/ref/models/fields/#foreignkey) viene utilizzato per specificare una relazione uno a molti con un altro modello di database, ad esempio un'automobile ha un solo produttore, ma un produttore può produrre molte automobili. Il lato "uno" della relazione è il modello che contiene la "chiave"; i modelli che contengono una "chiave esterna" che fa riferimento a tale "chiave" si trovano sul lato "molti" della relazione.
- [ManyToManyField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#manytomanyfield) viene utilizzato per specificare una relazione molti a molti, ad esempio un libro può avere diversi generi e ogni genere può contenere diversi libri. Nell'app della libreria verranno usati in modo molto simile a `ForeignKeys`, ma possono essere utilizzati in modi più complessi per descrivere le relazioni tra gruppi. Dispongono del parametro `on_delete` per definire cosa accade quando il record associato viene eliminato, ad esempio un valore di `models.SET_NULL` imposterebbe il valore a `NULL`.

Esistono molti altri tipi di campo, inclusi campi per diversi tipi di numeri (interi grandi, interi piccoli, numeri in virgola mobile), booleani, URL, slug, id univoci e altre informazioni "relative al tempo" (durata, orario e così via). È possibile visualizzare [l'elenco completo qui](https://docs.djangoproject.com/en/5.0/ref/models/fields/#field-types).

#### Metadati

È possibile dichiarare metadati a livello di modello per il proprio Model dichiarando `class Meta`, come mostrato.

```python
class Meta:
    ordering = ['-my_field_name']
```

Una delle funzionalità più utili di questi metadati consiste nel controllare l'_ordinamento predefinito_ dei record restituiti quando viene eseguita una query sul tipo di modello. Questo viene fatto specificando l'ordine di corrispondenza in un elenco di nomi di campo nell'attributo `ordering`, come mostrato in precedenza. L'ordinamento dipenderà dal tipo di campo: i campi di caratteri sono ordinati alfabeticamente, mentre i campi data sono ordinati cronologicamente. Come mostrato sopra, è possibile anteporre al nome del campo un simbolo meno (-) per invertire l'ordine di ordinamento.

Ad esempio, se si scegliesse di ordinare i libri in questo modo per impostazione predefinita:

```python
ordering = ['title', '-publish_date']
```

i libri verrebbero ordinati alfabeticamente per titolo, da A a Z, e poi per data di pubblicazione all'interno di ciascun titolo, dal più recente al meno recente.

Un altro attributo comune è `verbose_name`, un nome descrittivo per la classe nelle forme singolare e plurale:

```python
verbose_name = 'BetterName'
```

I metadati della classe possono essere utilizzati per creare e applicare nuovi "permessi di accesso" per il modello (i permessi predefiniti vengono applicati automaticamente), consentire l'ordinamento basato su un altro campo, definire [vincoli](https://docs.djangoproject.com/en/5.0/ref/models/constraints/) sui possibili valori dei dati che possono essere archiviati oppure dichiarare che la classe è "astratta" (una classe base per la quale non è possibile creare record e dalla quale verranno invece derivati altri modelli).

Molte delle altre opzioni dei metadati controllano quale database deve essere utilizzato per il modello e come vengono archiviati i dati. Queste sono realmente utili solo se è necessario mappare un modello a un database esistente.

L'elenco completo delle opzioni dei metadati è disponibile qui: [Opzioni dei metadati del modello](https://docs.djangoproject.com/en/5.0/ref/models/options/) (documentazione Django).

#### Metodi

Un modello può inoltre avere metodi.

**Come minimo, in ogni modello dovrebbe essere definito il metodo standard della classe Python `__str__()` per restituire una stringa leggibile per ogni oggetto.** Questa stringa viene utilizzata per rappresentare i singoli record nel sito di amministrazione, e ovunque sia necessario fare riferimento a un'istanza del modello. Spesso restituisce un campo titolo o nome del modello.

```python
def __str__(self):
    return self.my_field_name
```

Un altro metodo comune da includere nei modelli Django è `get_absolute_url()`, che restituisce un URL per visualizzare i singoli record del modello nel sito web. Se viene definito questo metodo, Django aggiungerà automaticamente un pulsante "View on Site" alle schermate di modifica dei record del modello nel sito di amministrazione. Di seguito è mostrato uno schema tipico per `get_absolute_url()`.

```python
def get_absolute_url(self):
    """Returns the URL to access a particular instance of the model."""
    return reverse('model-detail-view', args=[str(self.id)])
```

> [!NOTE]
> Supponendo di utilizzare URL come `/my-application/my-model-name/2` per visualizzare i singoli record del modello, dove "2" è l'`id` di un particolare record, sarà necessario creare un mapper URL per passare la risposta e l'id a una "vista di dettaglio del modello", che svolgerà il lavoro necessario per visualizzare il record. La funzione `reverse()` precedente è in grado di "invertire" il mapper URL, nel caso precedente denominato _'model-detail-view'_, per creare un URL nel formato corretto.
>
> Naturalmente, per far funzionare tutto ciò è comunque necessario scrivere la mappatura URL, la vista e il template.

È inoltre possibile definire qualsiasi altro metodo desiderato e richiamarlo dal proprio codice o dai template, purché non accetti parametri.

### Gestione dei modelli

Dopo aver definito le classi dei modelli, è possibile utilizzarle per creare, aggiornare o eliminare record ed eseguire query per ottenere tutti i record o particolari sottoinsiemi di record. Verrà mostrato come farlo nel tutorial durante la definizione delle viste, ma ecco un breve riepilogo.

#### Creazione e modifica dei record

Per creare un record è possibile definire un'istanza del modello e quindi chiamare `save()`.

```python
# Create a new record using the model's constructor.
record = MyModelName(my_field_name="Instance #1")

# Save the object into the database.
record.save()
```

> [!NOTE]
> Se non è stato dichiarato alcun campo come `primary_key`, al nuovo record ne verrà assegnato automaticamente uno, con nome del campo `id`. Questo campo potrebbe essere interrogato dopo il salvataggio del record precedente e avrebbe valore 1.

È possibile accedere ai campi in questo nuovo record usando la sintassi con il punto e modificarne i valori. È necessario chiamare `save()` per archiviare i valori modificati nel database.

```python
# Access model field values using Python attributes.
print(record.id) # should return 1 for the first record.
print(record.my_field_name) # should print 'Instance #1'

# Change record by modifying the fields, then calling save().
record.my_field_name = "New Instance Name"
record.save()
```

#### Ricerca dei record

È possibile cercare record che corrispondono a determinati criteri usando l'attributo `objects` del modello, fornito dalla classe base.

> [!NOTE]
> Spiegare come cercare record usando nomi di modelli e campi "astratti" può essere un po' confuso. Nella discussione seguente, si farà riferimento a un modello `Book` con campi `title` e `genre`, dove anche genre è un modello con un singolo campo `name`.

È possibile ottenere tutti i record per un modello come `QuerySet`, usando `objects.all()`. Il `QuerySet` è un oggetto iterabile, il che significa che contiene diversi oggetti sui quali è possibile iterare.

```python
all_books = Book.objects.all()
```

Il metodo `filter()` di Django consente di filtrare il `QuerySet` restituito per confrontare un campo **testuale** o **numerico** specificato con criteri particolari. Ad esempio, per filtrare i libri che contengono "wild" nel titolo e quindi contarli, si potrebbe procedere come segue:

```python
wild_books = Book.objects.filter(title__contains='wild')
number_wild_books = wild_books.count()
```

I campi da confrontare e il tipo di confronto sono definiti nel nome del parametro di filtro, utilizzando il formato `field_name__match_type`. Si noti il _doppio trattino basso_ tra `title` e `contains` sopra. Nell'esempio precedente viene filtrato `title` con un confronto che distingue tra maiuscole e minuscole. Esistono molti altri tipi di confronto: `icontains` (senza distinzione tra maiuscole e minuscole), `iexact` (corrispondenza esatta senza distinzione tra maiuscole e minuscole), `exact` (corrispondenza esatta con distinzione tra maiuscole e minuscole), `in`, `gt` (maggiore di), `startswith` e così via. [L'elenco completo è disponibile qui](https://docs.djangoproject.com/en/5.0/ref/models/querysets/#field-lookups).

In alcuni casi, sarà necessario filtrare su un campo che definisce una relazione uno a molti con un altro modello, ad esempio un `ForeignKey`. In questo caso, è possibile "indicizzare" i campi all'interno del modello correlato tramite ulteriori doppi trattini bassi.
Quindi, ad esempio, per filtrare i libri con uno schema di genere specifico, sarà necessario indicizzare `name` attraverso il campo `genre`, come mostrato di seguito:

```python
# Will match on: Fiction, Science fiction, non-fiction etc.
books_containing_genre = Book.objects.filter(genre__name__icontains='fiction')
```

> [!NOTE]
> È possibile utilizzare i trattini bassi (`__`) per attraversare tutti i livelli di relazioni (`ForeignKey`/`ManyToManyField`) desiderati.
> Ad esempio, un `Book` con tipi diversi, definiti mediante un'ulteriore relazione "cover", potrebbe avere un nome di parametro: `type__cover__name__exact='hard'`.

Con le query è possibile fare molto altro, incluse ricerche inverse dai modelli correlati, concatenazione di filtri, restituzione di un insieme più piccolo di valori e così via. Per ulteriori informazioni, vedere [Esecuzione di query](https://docs.djangoproject.com/en/5.0/topics/db/queries/) (documentazione Django).

## Definizione dei modelli LocalLibrary

In questa sezione verrà avviata la definizione dei modelli per la libreria. Aprire `models.py` (in /django-locallibrary-tutorial/catalog/). Il codice boilerplate nella parte superiore della pagina importa il modulo _models_, che contiene la classe base dei modelli `models.Model` da cui i modelli erediteranno.

```python
from django.db import models

# Create your models here.
```

### Modello Genre

Copiare il codice del modello `Genre` mostrato di seguito e incollarlo alla fine del file `models.py`. Questo modello viene utilizzato per archiviare informazioni sulla categoria del libro, ad esempio se si tratta di narrativa o saggistica, romanzo o storia militare e così via.
Come indicato in precedenza, il genere è stato creato come modello anziché come testo libero o lista di selezione affinché i valori possibili possano essere gestiti tramite il database anziché essere codificati direttamente.

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

Il modello ha un singolo campo `CharField` (`name`), utilizzato per descrivere il genere, limitato a 200 caratteri e con un `help_text`.
Questo campo è stato impostato come univoco (`unique=True`) poiché dovrebbe esistere un solo record per ogni genere.

Dopo il campo, viene dichiarato un metodo `__str__()`, che restituisce il nome del genere definito da un particolare record. Non è stato definito alcun nome descrittivo, pertanto l'etichetta del campo sarà `Name` quando viene usato nei moduli.
Viene quindi dichiarato il metodo `get_absolute_url()`, che restituisce un URL utilizzabile per accedere a un record di dettaglio per questo modello. Perché funzioni, sarà necessario definire una mappatura URL denominata `genre-detail`, nonché una vista e un template associati.

L'impostazione `unique=True` sul campo precedente impedisce la creazione di generi con _esattamente_ lo stesso nome, ma non di varianti come "fantasy", "Fantasy" o persino "FaNtAsY".
L'ultima parte della definizione del modello utilizza un'opzione [`constraints`](https://docs.djangoproject.com/en/5.0/ref/models/options/#constraints) nei [metadati](#metadati) del modello per specificare che la versione minuscola del valore nel campo `name` deve essere univoca nel database e per visualizzare la stringa `violation_error_message` in caso contrario.
In questo caso non è necessario fare altro, ma è possibile definire più vincoli rispetto a un campo o a più campi.
Per ulteriori informazioni, vedere il [riferimento sui vincoli](https://docs.djangoproject.com/en/5.0/ref/models/constraints/), inclusi [`UniqueConstraint()`](https://docs.djangoproject.com/en/5.0/ref/models/constraints/#uniqueconstraint) e [`Lower()`](https://docs.djangoproject.com/en/5.0/ref/models/database-functions/#lower).

### Modello Book

Copiare il modello `Book` seguente e incollarlo nuovamente alla fine del file. Il modello `Book` rappresenta tutte le informazioni su un libro disponibile in senso generale, ma non una particolare "istanza" fisica o "copia" disponibile per il prestito.

Il modello utilizza un `CharField` per rappresentare `title` e `isbn` del libro.
Per `isbn`, si noti come il primo parametro senza nome imposti esplicitamente l'etichetta su "ISBN", altrimenti sarebbe impostata per default su "Isbn". Viene inoltre impostato il parametro `unique` su `True` per garantire che tutti i libri abbiano un ISBN univoco; il parametro unique rende il valore del campo globalmente univoco in una tabella.
Diversamente da `isbn`, e dal nome del genere, `title` non è impostato come univoco, poiché è possibile che libri diversi abbiano lo stesso nome.
Il modello utilizza `TextField` per `summary`, poiché questo testo potrebbe dover essere piuttosto lungo.

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

Il genere è un `ManyToManyField`, in modo che un libro possa avere più generi e un genere possa avere molti libri. L'autore è dichiarato come `ForeignKey`, quindi ogni libro avrà un solo autore, ma un autore potrà avere molti libri. In pratica un libro potrebbe avere più autori, ma non in questa implementazione.

In entrambi i tipi di campo, la classe del modello correlato viene dichiarata come primo parametro senza nome, utilizzando la classe del modello oppure una stringa contenente il nome del modello correlato. È necessario utilizzare il nome del modello come stringa se la classe associata non è ancora stata definita in questo file prima di essere referenziata. Gli altri parametri di interesse nel campo `author` sono `null=True`, che consente al database di archiviare un valore `Null` se non viene selezionato alcun autore, e `on_delete=models.RESTRICT`, che impedirà l'eliminazione dell'autore associato al libro se è referenziato da qualsiasi libro.

> [!WARNING]
> Per impostazione predefinita, `on_delete=models.CASCADE`, il che significa che se l'autore venisse eliminato, verrebbe eliminato anche questo libro. Qui viene usato `RESTRICT`, ma si potrebbe anche usare `PROTECT` per impedire l'eliminazione dell'autore finché un qualsiasi libro lo utilizza, oppure `SET_NULL` per impostare l'autore del libro su `Null` se il record viene eliminato.

Il modello definisce anche `__str__()`, utilizzando il campo `title` del libro per rappresentare un record `Book`. Il metodo finale, `get_absolute_url()`, restituisce un URL utilizzabile per accedere a un record di dettaglio per questo modello. Sarà necessario definire una mappatura URL denominata `book-detail`, nonché una vista e un template associati.

### Modello BookInstance

Successivamente, copiare il modello `BookInstance`, mostrato di seguito, sotto gli altri modelli. `BookInstance` rappresenta una copia specifica di un libro che qualcuno potrebbe prendere in prestito e include informazioni sul fatto che la copia sia disponibile o sulla data prevista per la restituzione, sui dettagli di "imprint" o versione e su un id univoco per il libro nella libreria.

Alcuni dei campi e metodi saranno ormai familiari. Il modello utilizza:

- `ForeignKey` per identificare il `Book` associato: ogni libro può avere molte copie, ma una copia può avere un solo `Book`. La chiave specifica `on_delete=models.RESTRICT` per garantire che il `Book` non possa essere eliminato mentre è referenziato da un `BookInstance`.
- `CharField` per rappresentare l'imprint, cioè l'edizione specifica, del libro.

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

Vengono inoltre dichiarati alcuni nuovi tipi di campo:

- `UUIDField` viene utilizzato per il campo `id`, per impostarlo come `primary_key` per questo modello.
  Questo tipo di campo assegna un valore globalmente univoco a ogni istanza, uno per ogni libro che può essere trovato nella libreria.
- `DateField` viene utilizzato per la data `due_back`, ovvero la data in cui il libro dovrebbe tornare disponibile dopo essere stato preso in prestito o sottoposto a manutenzione. Questo valore può essere `blank` o `null`, necessario quando il libro è disponibile. I metadati del modello (`Class Meta`) utilizzano questo campo per ordinare i record quando vengono restituiti in una query.
- `status` è un `CharField` che definisce una lista di scelta/selezione. Come visibile, viene definita una tupla contenente tuple di coppie chiave-valore e passata all'argomento choices. Il valore in una coppia chiave/valore è un valore visualizzato che un utente può selezionare, mentre le chiavi sono i valori effettivamente salvati se viene selezionata l'opzione. È stato inoltre impostato il valore predefinito 'm' (manutenzione), poiché i libri verranno inizialmente creati come non disponibili prima di essere collocati sugli scaffali.

Il metodo `__str__()` rappresenta l'oggetto `BookInstance` utilizzando una combinazione del suo id univoco e del titolo del `Book` associato.

> [!NOTE]
> Un po' di Python:
>
> - A partire da Python 3.6, è possibile utilizzare la sintassi di interpolazione delle stringhe, nota anche come f-string: `f'{self.id} ({self.book.title})'`.
> - Nelle versioni precedenti di questo tutorial veniva utilizzata la sintassi delle [stringhe formattate](https://peps.python.org/pep-3101/), che è anch'essa un modo valido per formattare stringhe in Python, ad esempio `'{0} ({1})'.format(self.id,self.book.title)`.

### Modello Author

Copiare il modello `Author`, mostrato di seguito, sotto il codice esistente in **models.py**.

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

Tutti i campi/metodi dovrebbero ormai essere familiari. Il modello definisce un autore come dotato di nome, cognome, date di nascita e morte, entrambe facoltative. Specifica che, per impostazione predefinita, `__str__()` restituisce il nome nell'ordine _cognome_, _nome_. Il metodo `get_absolute_url()` inverte la mappatura URL `author-detail` per ottenere l'URL per visualizzare un singolo autore.

## Eseguire nuovamente le migrazioni del database

Tutti i modelli sono stati creati. Ora eseguire nuovamente le migrazioni del database per aggiungerli al database.

```bash
python3 manage.py makemigrations
python3 manage.py migrate
```

## Modello Language — esercizio

Immaginare che un benefattore locale doni diversi nuovi libri scritti in un'altra lingua, ad esempio il farsi. L'esercizio consiste nel capire come rappresentarli al meglio nel sito web della libreria e aggiungerli quindi ai modelli.

Alcuni aspetti da considerare:

- La "lingua" dovrebbe essere associata a un `Book`, a un `BookInstance` o a un altro oggetto?
- Le diverse lingue dovrebbero essere rappresentate utilizzando un modello, un campo di testo libero o una lista di selezione codificata direttamente?

Dopo aver deciso, aggiungere il campo. È possibile vedere cosa è stato deciso [per il progetto su GitHub](https://github.com/mdn/django-locallibrary-tutorial/blob/main/catalog/models.py).

Non dimenticare che dopo una modifica al modello è necessario eseguire nuovamente le migrazioni del database per aggiungere le modifiche.

```bash
python3 manage.py makemigrations
python3 manage.py migrate
```

## Riepilogo

In questo articolo è stato appreso come vengono definiti i modelli e queste informazioni sono state poi utilizzate per progettare e implementare modelli appropriati per il sito web _LocalLibrary_.

A questo punto si farà una breve deviazione dalla creazione del sito per esaminare il _sito di amministrazione Django_. Questo sito consentirà di aggiungere alcuni dati alla libreria, che potranno poi essere visualizzati usando le viste e i template, che devono ancora essere creati.

## Vedere anche

- [Scrivere la prima app Django, parte 2](https://docs.djangoproject.com/en/5.0/intro/tutorial02/) (documentazione Django)
- [Esecuzione di query](https://docs.djangoproject.com/en/5.0/topics/db/queries/) (documentazione Django)
- [Riferimento API QuerySet](https://docs.djangoproject.com/en/5.0/ref/models/querysets/) (documentazione Django)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/skeleton_website", "Learn_web_development/Extensions/Server-side/Django/Admin_site", "Learn_web_development/Extensions/Server-side/Django")}}
