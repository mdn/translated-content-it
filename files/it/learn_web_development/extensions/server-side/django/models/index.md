---
title: "Tutorial su Django Parte 3: Utilizzo dei modelli"
short-title: "3: Modelli"
slug: Learn_web_development/Extensions/Server-side/Django/Models
l10n:
  sourceCommit: e488eba036b2fee56444fd579c3759ef45ff2ca8
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/skeleton_website", "Learn_web_development/Extensions/Server-side/Django/Admin_site", "Learn_web_development/Extensions/Server-side/Django")}}

Questo articolo mostra come definire modelli per il sito web LocalLibrary. Spiega cos'è un modello, come viene dichiarato e alcuni dei principali tipi di campo. Mostra anche brevemente alcuni dei principali modi in cui è possibile accedere ai dati del modello.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/skeleton_website">Tutorial su Django Parte 2: Creazione di un sito web scheletro</a>.
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

Le applicazioni web Django accedono e gestiscono i dati attraverso oggetti Python definiti modelli. I modelli definiscono la _struttura_ dei dati archiviati, inclusi i _tipi_ di campo e possibilmente anche la loro dimensione massima, valori predefiniti, opzioni di elenco di selezione, testo di aiuto per la documentazione, testo dell'etichetta per i form, ecc. La definizione del modello è indipendente dal database sottostante — puoi scegliere uno dei diversi come parte delle impostazioni del tuo progetto. Una volta che hai scelto il database che vuoi utilizzare, non hai bisogno di interfacciarti direttamente con esso — basta scrivere la struttura del modello e altro codice, e Django gestisce tutto il lavoro sporco di comunicazione con il database per te.

Questo tutorial mostra come definire e accedere ai modelli per l'esempio di sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website).

## Progettare i modelli di LocalLibrary

Prima di iniziare a programmare i modelli, vale la pena prendersi qualche minuto per pensare a quali dati dobbiamo memorizzare e le relazioni tra i diversi oggetti.

Sappiamo che dobbiamo memorizzare informazioni sui libri (titolo, sommario, autore, lingua scritta, categoria, ISBN) e che potremmo avere più copie disponibili (con ID univoco a livello globale, stato di disponibilità, ecc.). Potremmo aver bisogno di memorizzare più informazioni sull'autore oltre al suo nome, e potrebbero esserci più autori con lo stesso o simili nomi. Vogliamo essere in grado di ordinare le informazioni in base al titolo del libro, all'autore, alla lingua scritta e alla categoria.

Quando si progettano i propri modelli, ha senso avere modelli separati per ogni "oggetto" (un gruppo di informazioni correlate). In questo caso, gli oggetti ovvi sono libri, istanze di libri e autori.

Potresti anche voler usare modelli per rappresentare le opzioni di elenco di selezione (ad es., come un elenco a discesa di scelte), anziché codificare le scelte nel sito web stesso — questo è consigliato quando non si conoscono tutte le opzioni in anticipo o possono cambiare. Candidati ovvi per modelli, in questo caso, includono il genere del libro (ad es., Fantascienza, Poesia francese, ecc.) e la lingua (Inglese, Francese, Giapponese).

Una volta che abbiamo deciso i nostri modelli e campi, dobbiamo pensare alle relazioni. Django ti permette di definire relazioni che sono uno a uno (`OneToOneField`), uno a molti (`ForeignKey`) e molti a molti (`ManyToManyField`).

Tenendo ciò a mente, il diagramma di associazione UML qui sotto mostra i modelli che definiremo in questo caso (come box).

![UML del Modello LocalLibrary con molteplicità dell'Autore fissata all'interno della classe Book](local_library_model_uml.svg)

Abbiamo creato modelli per il libro (i dettagli generici del libro), l'istanza del libro (stato delle copie fisiche specifiche del libro disponibili nel sistema) e l'autore. Abbiamo anche deciso di avere un modello per il genere in modo che i valori possano essere creati/selezionati tramite l'interfaccia admin. Abbiamo deciso di non avere un modello per lo `status` di `BookInstance` — abbiamo integrato i valori (`LOAN_STATUS`) perché non ci aspettiamo che cambino. All'interno di ciascun box, puoi vedere il nome del modello, i nomi e tipi dei campi, e anche i metodi e i loro tipi di ritorno.

Il diagramma mostra anche le relazioni tra i modelli, incluse le loro _molteplicità_. Le molteplicità sono i numeri sul diagramma che mostrano i numeri (massimo e minimo) di ciascun modello che può essere presente nella relazione. Ad esempio, la linea di collegamento tra i box mostra che un Libro e un Genere sono correlati. I numeri vicino al modello Genere mostrano che un libro deve avere uno o più generi (quanti vuoi), mentre i numeri dall'altra parte della linea accanto al modello Libro mostrano che un Genere può avere zero o molti libri associati.

> [!NOTE]
> La prossima sezione fornisce una guida di base che spiega come vengono definiti e utilizzati i modelli. Mentre la leggi, considera come costruiremo ciascuno dei modelli nel diagramma sopra.

## Guida ai modelli

Questa sezione fornisce una breve panoramica di come viene definito un modello e alcuni dei campi e argomenti di campo più importanti.

### Definizione del modello

I modelli sono solitamente definiti nel file **models.py** di un'applicazione. Sono implementati come sottoclassi di `django.db.models.Model`, e possono includere campi, metodi e metadati. Il frammento di codice qui sotto mostra un modello "tipico", chiamato `MyModelName`:

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

Nelle sezioni sottostanti esploreremo ciascuna delle caratteristiche all'interno del modello nel dettaglio:

#### Campi

Un modello può avere un numero arbitrario di campi, di qualsiasi tipo — ciascuno rappresenta una colonna di dati che vogliamo memorizzare in una delle nostre tabelle del database. Ogni record del database (riga) consisterà in uno di ciascun valore di campo. Guardiamo l'esempio visto qui sotto:

```python
my_field_name = models.CharField(max_length=20, help_text='Enter field documentation')
```

Esempio sopra ha un singolo campo chiamato `my_field_name`, di tipo `models.CharField` — il che significa che questo campo conterrà stringhe di caratteri alfanumerici. I tipi di campo sono assegnati utilizzando classi specifiche, che determinano il tipo di record utilizzato per memorizzare i dati nel database, insieme a criteri di validazione da utilizzare quando i valori vengono ricevuti da un modulo HTML (ovvero, cosa costituisce un valore valido). I tipi di campo possono anche prendere argomenti che specificano ulteriormente come il campo è memorizzato o può essere utilizzato. In questo caso stiamo dando al nostro campo due argomenti:

- `max_length=20` — Indica che la lunghezza massima di un valore in questo campo è di 20 caratteri.
- `help_text='Enter field documentation'` — testo di aiuto che può essere visualizzato in un modulo per aiutare gli utenti a capire come utilizzare il campo.

Il nome del campo viene utilizzato per riferirsi ad esso nelle query e nei template.
I campi hanno anche un'etichetta, che viene specificata utilizzando l'argomento `verbose_name` (con un valore predefinito di `None`).
Se `verbose_name` non è impostato, l'etichetta viene creata dal nome del campo sostituendo eventuali underscore con uno spazio e capitalizzando la prima lettera (ad esempio, il campo `my_field_name` avrebbe un'etichetta predefinita di _My field name_ quando utilizzato nei moduli).

L'ordine con cui i campi vengono dichiarati influenzerà il loro ordine predefinito se un modello viene visualizzato in un modulo (ad esempio, nel sito Admin), sebbene questo possa essere sovrascritto.

##### Argomenti comuni per i campi

I seguenti argomenti comuni possono essere utilizzati quando si dichiarano molti/la maggior parte dei diversi tipi di campo:

- [help_text](https://docs.djangoproject.com/en/5.0/ref/models/fields/#help-text): Fornisce un'etichetta di testo per form HTML (ad es., nel sito admin), come descritto sopra.
- [verbose_name](https://docs.djangoproject.com/en/5.0/ref/models/fields/#verbose-name): Un nome leggibile per il campo utilizzato nelle etichette dei campi. Se non specificato, Django dedurrà il nome verbose predefinito dal nome del campo.
- [default](https://docs.djangoproject.com/en/5.0/ref/models/fields/#default): Il valore predefinito per il campo. Questo può essere un valore o un oggetto invocabile, nel qual caso l'oggetto verrà chiamato ogni volta che viene creato un nuovo record.
- [null](https://docs.djangoproject.com/en/5.0/ref/models/fields/#null): Se `True`, Django memorizzerà i valori vuoti come `NULL` nel database per i campi dove è appropriato (un `CharField` memorizzerà invece una stringa vuota). Il valore predefinito è `False`.
- [blank](https://docs.djangoproject.com/en/5.0/ref/models/fields/#blank): Se `True`, il campo è consentito essere vuoto nei tuoi moduli. Il valore predefinito è `False`, il che significa che la convalida del modulo di Django ti costringerà a inserire un valore. Questo viene spesso utilizzato con `null=True`, perché se si permette di avere valori vuoti, si desidera anche che il database possa rappresentarli in modo appropriato.
- [choices](https://docs.djangoproject.com/en/5.0/ref/models/fields/#choices): Un gruppo di scelte per questo campo. Se viene fornito, il widget di modulo corrispondente predefinito sarà una casella di selezione con queste scelte invece del campo di testo standard.
- [unique](https://docs.djangoproject.com/en/5.0/ref/models/fields/#unique):
  Se `True`, garantisce che il valore del campo sia unico in tutto il database.
  Questo può essere usato per prevenire la duplicazione di campi che non possono avere gli stessi valori.
  Il valore predefinito è `False`.
- [primary_key](https://docs.djangoproject.com/en/5.0/ref/models/fields/#primary-key):
  Se `True`, imposta il campo corrente come chiave primaria per il modello (Una chiave primaria è una colonna speciale del database designata per identificare in modo univoco tutti i diversi record della tabella).
  Se nessun campo è specificato come chiave primaria, Django aggiungerà automaticamente un campo per questo scopo.
  Il tipo di campi di chiave primaria creati automaticamente può essere specificato per ogni app in [`AppConfig.default_auto_field`](https://docs.djangoproject.com/en/5.0/ref/applications/#django.apps.AppConfig.default_auto_field) o globalmente nelle impostazioni [`DEFAULT_AUTO_FIELD`](https://docs.djangoproject.com/en/5.0/ref/settings/#std:setting-DEFAULT_AUTO_FIELD).

  > [!NOTE]
  > Le app create utilizzando **manage.py** impostano il tipo della chiave primaria su un [BigAutoField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#bigautofield).
  > Puoi vedere questo nel file **catalog/apps.py** della libreria locale:
  >
  > ```python
  > class CatalogConfig(AppConfig):
  >   default_auto_field = 'django.db.models.BigAutoField'
  > ```

Ci sono molte altre opzioni — puoi visualizzare [l'elenco completo delle opzioni dei campi qui](https://docs.djangoproject.com/en/5.0/ref/models/fields/#field-options).

##### Tipi di campo comuni

L'elenco seguente descrive alcuni dei tipi di campi più comunemente usati.

- [CharField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#django.db.models.CharField) è utilizzato per definire stringhe di lunghezza fissa da brevi a medie. Devi specificare la `max_length` dei dati da memorizzare.
- [TextField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#django.db.models.TextField) è usato per stringhe arbitrarie di grandi dimensioni. Puoi specificare una `max_length` per il campo, ma questa viene utilizzata solo quando il campo viene visualizzato nei form (non è applicata a livello di database).
- [IntegerField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#django.db.models.IntegerField) è un campo per memorizzare valori interi (numeri interi), e per convalidare i valori inseriti come interi nei moduli.
- [DateField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#datefield) e [DateTimeField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#datetimefield) sono utilizzati per memorizzare/rappresentare date e informazioni di data/ora (come oggetti Python `datetime.date` e `datetime.datetime`, rispettivamente). Questi campi possono dichiarare inoltre i parametri (mutuamente esclusivi) `auto_now=True` (per impostare il campo sulla data corrente ogni volta che il modello viene salvato), `auto_now_add` (per impostare la data solo quando il modello viene creato per la prima volta), e `default` (per impostare una data predefinita che può essere sovrascritta dall'utente).
- [EmailField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#emailfield) è usato per memorizzare e convalidare indirizzi email.
- [FileField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#filefield) e [ImageField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#imagefield) sono utilizzati per caricare file e immagini rispettivamente (il `ImageField` aggiunge una ulteriore convalida che il file caricato sia un'immagine). Questi hanno parametri per definire come e dove vengono memorizzati i file caricati.
- [AutoField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#autofield) è un tipo speciale di `IntegerField` che si incrementa automaticamente. Una chiave primaria di questo tipo viene aggiunta automaticamente al tuo modello se non ne specifici esplicitamente una.
- [ForeignKey](https://docs.djangoproject.com/en/5.0/ref/models/fields/#foreignkey) è usata per specificare una relazione uno-a-molti con un altro modello di database (ad esempio, un'auto ha un produttore, ma un produttore può fare molte auto). Il lato "uno" della relazione è il modello che contiene la "chiave" (i modelli che contengono una "chiave esterna" che si riferiscono a quella "chiave", sono sul lato "molti" di una tale relazione).
- [ManyToManyField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#manytomanyfield) è usata per specificare una relazione molti-a-molti (ad esempio, un libro può avere diversi generi e ciascun genere può contenere diversi libri). Nella nostra app della biblioteca li useremo molto similmente ai `ForeignKeys`, ma possono essere usati in modi più complicati per descrivere le relazioni tra gruppi. Questi hanno il parametro `on_delete` per definire cosa succede quando il record associato viene eliminato (ad esempio, un valore di `models.SET_NULL` imposta il valore a `NULL`).

Ci sono molti altri tipi di campi, inclusi campi per diversi tipi di numeri (grandi interi, piccoli interi, valori float), booleani, URL, slug, ID unici e altre informazioni "relative al tempo" (durata, tempo, ecc.). Puoi visualizzare l'[elenco completo qui](https://docs.djangoproject.com/en/5.0/ref/models/fields/#field-types).

#### Metadati

Puoi dichiarare metadati a livello di modello per il tuo Modello dichiarando `class Meta`, come mostrato.

```python
class Meta:
    ordering = ['-my_field_name']
```

Una delle caratteristiche più utili di questi metadati è di controllare l'ordinamento _predefinito_ dei record restituiti quando esegui una query sul tipo di modello. Lo fai specificando l'ordine di corrispondenza in un elenco di nomi di campo nell'attributo `ordering`, come mostrato sopra. L'ordinamento dipenderà dal tipo di campo (i campi carattere sono ordinati alfabeticamente, mentre i campi data sono ordinati in ordine cronologico). Come mostrato sopra, puoi anteporre il nome del campo con un simbolo meno (-) per invertire l'ordine di ordinamento.

Quindi come esempio, se scegliessimo di ordinare i libri in questo modo di default:

```python
ordering = ['title', '-publish_date']
```

i libri sarebbero ordinati alfabeticamente per titolo, da A-Z, e poi per data di pubblicazione all'interno di ciascun titolo, dal più recente al più vecchio.

Un altro attributo comune è `verbose_name`, un nome verbose per la classe in forma singolare e plurale:

```python
verbose_name = 'BetterName'
```

I metadati della classe possono essere utilizzati per creare e applicare nuove "autorizzazioni di accesso" per il modello (autorizzazioni predefinite vengono applicate automaticamente), consentire l'ordinamento basato su un altro campo, definire [vincoli](https://docs.djangoproject.com/en/5.0/ref/models/constraints/) sui valori possibili dei dati archiviabili, o dichiarare che la classe è "astratta" (una classe base per la quale non puoi creare record, che sarà invece derivata per creare altri modelli).

Molte delle altre opzioni dei metadati controllano quale database deve essere utilizzato per il modello e come vengono archiviati i dati (queste sono davvero utili solo se hai bisogno di mappare un modello su un database esistente).

L'elenco completo delle opzioni di metadati è disponibile qui: [Opzioni dei metadati del modello](https://docs.djangoproject.com/en/5.0/ref/models/options/) (documenti di Django).

#### Metodi

Un modello può anche avere metodi.

**Minimamente, in ogni modello dovresti definire il metodo standard Python `__str__()` per restituire una stringa leggibile per ogni oggetto.** Questa stringa viene utilizzata per rappresentare i record individuali nel sito di amministrazione (e ovunque tu debba fare riferimento a un'istanza di modello). Spesso questo restituirà un campo titolo o nome dal modello.

```python
def __str__(self):
    return self.my_field_name
```

Un altro metodo comune da includere nei modelli Django è `get_absolute_url()`, che restituisce un URL per visualizzare i record di modello individuali sul sito web (se definisci questo metodo, Django aggiungerà automaticamente un pulsante "View on Site" ai moduli di modifica dei record del modello nel sito Admin). Un modello tipico per `get_absolute_url()` viene mostrato di seguito.

```python
def get_absolute_url(self):
    """Returns the URL to access a particular instance of the model."""
    return reverse('model-detail-view', args=[str(self.id)])
```

> [!NOTE]
> Supponendo di utilizzare URL come `/my-application/my-model-name/2` per visualizzare i record individuali per il tuo modello (dove "2" è l'`id` per un particolare record), dovrai creare un mapper URL per passare la risposta e l'id a una "vista di dettaglio del modello" (che farà il lavoro necessario per visualizzare il record). La funzione `reverse()` sopra è in grado di "invertire" il tuo mapper URL (nel caso sopra chiamato _'model-detail-view'_) al fine di creare un URL nel formato corretto.
>
> Ovviamente per far funzionare questo devi comunque scrivere la mapping URL, la vista e il template!

Puoi anche definire qualsiasi altro metodo che desideri e chiamarli dal tuo codice o template (a condizione che non prendano parametri).

### Gestione del modello

Una volta definiti i tuoi modelli di classe, puoi usarli per creare, aggiornare o eliminare record e per eseguire query per ottenere tutti i record o particolari sottoinsiemi di record. Ti mostreremo come fare questo nel tutorial quando definiremo le nostre viste, ma qui c'è un breve riassunto.

#### Creazione e modifica dei record

Per creare un record puoi definire un'istanza del modello e poi chiamare `save()`.

```python
# Create a new record using the model's constructor.
record = MyModelName(my_field_name="Instance #1")

# Save the object into the database.
record.save()
```

> [!NOTE]
> Se non hai dichiarato alcun campo come `primary_key`, il nuovo record ne riceverà una automaticamente, con il nome del campo `id`. Potresti interrogare questo campo dopo aver salvato il record sopra, e avrebbe un valore di 1.

Puoi accedere ai campi in questo nuovo record usando la sintassi del punto e modificare i valori. Devi chiamare `save()` per memorizzare i valori modificati nel database.

```python
# Access model field values using Python attributes.
print(record.id) # should return 1 for the first record.
print(record.my_field_name) # should print 'Instance #1'

# Change record by modifying the fields, then calling save().
record.my_field_name = "New Instance Name"
record.save()
```

#### Ricerca di record

Puoi cercare record che corrispondono a certi criteri usando l'attributo `objects` del modello (fornito dalla classe base).

> [!NOTE]
> Spiegare come cercare record usando i nomi di modello e campo "astratti" può essere un po' confuso. Nella discussione seguente, ci riferiremo a un modello `Book` con campi `title` e `genre`, dove il genere è anche un modello con un singolo campo `name`.

Possiamo ottenere tutti i record per un modello come un `QuerySet`, usando `objects.all()`. Il `QuerySet` è un oggetto iterabile, il che significa che contiene un numero di oggetti che possiamo iterare/ciclo.

```python
all_books = Book.objects.all()
```

Il metodo `filter()` di Django ci consente di filtrare il `QuerySet` restituito per abbinare un campo** testuale** o **numerico** a particolari criteri. Ad esempio, per filtrare i libri che contengono "wild" nel titolo e poi contarli, potremmo fare quanto segue:

```python
wild_books = Book.objects.filter(title__contains='wild')
number_wild_books = wild_books.count()
```

I campi da abbinare e il tipo di abbinamento sono definiti nel nome del parametro filter, usando il formato: `field_name__match_type` (nota il doppio underscore tra `title` e `contains` sopra). Sopra stiamo filtrando `title` con una corrispondenza sensibile alle maiuscole. Ci sono molti altri tipi di corrispondenze che puoi fare: `icontains` (ignorando i casi), `iexact` (corrispondenza esatta ignorando i casi), `exact` (corrispondenza esatta sensibile al caso) e `in`, `gt` (maggiore di), `startswith`, ecc. L'[elenco completo è qui](https://docs.djangoproject.com/en/5.0/ref/models/querysets/#field-lookups).

In alcuni casi, dovrai filtrare un campo che definisce una relazione uno-a-molti con un altro modello (ad es., un `ForeignKey`). In questo caso, puoi "accedere" ai campi all'interno del modello correlato con ulteriori underscore doppi.
Ad esempio, per filtrare i libri con un pattern di genere specifico, dovrai accedere al `name` attraverso il campo `genre`, come mostrato sotto:

```python
# Will match on: Fiction, Science fiction, non-fiction etc.
books_containing_genre = Book.objects.filter(genre__name__icontains='fiction')
```

> [!NOTE]
> Puoi usare gli underscore (`__`) per navigare tanti livelli di relazioni (`ForeignKey`/`ManyToManyField`) quanto vuoi.
> Ad esempio, un `Book` che aveva tipi diversi, definiti usando una ulteriore relazione "cover", potrebbe avere un nome di parametro: `type__cover__name__exact='hard'.`

C'è molto altro che puoi fare con le query, incluse le ricerche all'indietro dai modelli correlati, il chaining dei filtri, la restituzione di un set di valori più piccolo, ecc. Per ulteriori informazioni, vedi [Effettuare query](https://docs.djangoproject.com/en/5.0/topics/db/queries/) (Documenti Django).

## Definire i modelli di LocalLibrary

In questa sezione inizieremo a definire i modelli per la biblioteca. Apri `models.py` (in /django-locallibrary-tutorial/catalog/). Il boilerplate nella parte superiore della pagina importa il modulo _models_, che contiene la classe `models.Model` di base del modello da cui i nostri modelli erediteranno.

```python
from django.db import models

# Create your models here.
```

### Modello Genre

Copia il codice del modello `Genre` mostrato sotto e incollalo alla fine del tuo file `models.py`. Questo modello è usato per memorizzare informazioni sulla categoria del libro — ad esempio, se è narrativa o non narrativa, romance o storia militare, ecc.
Come menzionato sopra, abbiamo creato il genere come modello piuttosto che come testo libero o elenco di selezione in modo che i valori possibili possano essere gestiti attraverso il database anziché essere hardcoded.

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

Il modello ha un singolo campo `CharField` (`name`), che viene utilizzato per descrivere il genere (questo è limitato a 200 caratteri e ha del `help_text`).
Abbiamo impostato questo campo per essere unico (`unique=True`) perché dovrebbe esserci solo un record per ogni genere.

Dopo il campo, dichiariamo un metodo `__str__()`, che restituisce il nome del genere definito da un particolare record. Nessun nome verbose è stato definito, quindi l’etichetta del campo sarà `Name` quando viene utilizzato nei moduli.
Poi dichiariamo il metodo `get_absolute_url()`, che restituisce un URL che può essere utilizzato per accedere a un record di dettaglio per questo modello (per far funzionare questo, dovremo definire una mappatura URL che abbia il nome `genre-detail`, e definire una vista e un template associati).

Impostare `unique=True` nel campo sopra impedisce la creazione di generi con nomi _esattamente_ uguali, ma non variazioni come "fantasy", "Fantasy", o anche "FaNtAsY".
L'ultima parte della definizione del modello utilizza un'opzione di [`constraints`](https://docs.djangoproject.com/en/5.0/ref/models/options/#constraints) sui [metadati](#metadati) del modello per specificare che il valore in minuscolo nel campo `name` deve essere unico nel database e visualizzare la stringa `violation_error_message` se non lo è.
Qui non abbiamo bisogno di fare nient'altro, ma puoi definire più vincoli contro un campo o campi.
Per ulteriori informazioni, consulta il [riferimento ai vincoli](https://docs.djangoproject.com/en/5.0/ref/models/constraints/), inclusi [`UniqueConstraint()`](https://docs.djangoproject.com/en/5.0/ref/models/constraints/#uniqueconstraint) (e [`Lower()`](https://docs.djangoproject.com/en/5.0/ref/models/database-functions/#lower)).

### Modello Book

Copia il modello `Book` sotto e incollalo ancora una volta alla fine del file. Il modello `Book` rappresenta tutte le informazioni su un libro disponibile in senso generale, ma non una particolare "istanza" fisica o "copia" disponibile per il prestito.

Il modello utilizza un `CharField` per rappresentare il `title` e l’`isbn` del libro.
Per `isbn`, nota come il primo parametro non nominato imposta esplicitamente l'etichetta come "ISBN" (altrimenti, sarebbe predefinita "Isbn"). Impostiamo anche il parametro `unique` come `true` per garantire che tutti i libri abbiano un ISBN univoco (il parametro unique rende il valore del campo univoco a livello globale in una tabella).
A differenza dell’`isbn` (e del nome del genere), il `title` non è impostato per essere unico, perché è possibile che libri diversi abbiano lo stesso nome.
Il modello utilizza `TextField` per il `summary`, perché questo testo può essere piuttosto lungo.

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

Il genere è un `ManyToManyField`, in modo che un libro possa avere più generi e un genere possa avere molti libri. L'autore è dichiarato come `ForeignKey`, quindi ogni libro avrà solo un autore, ma un autore può avere molti libri (in pratica un libro potrebbe avere più autori, ma non in questa implementazione!)

In entrambi i tipi di campo, la classe del modello correlato viene dichiarata come il primo parametro non nominato utilizzando la classe del modello o una stringa contenente il nome del modello correlato. Devi utilizzare il nome del modello come stringa se la classe associata non è ancora stata definita in questo file prima di fare riferimento ad essa! Gli altri parametri di interesse nel campo `author` sono `null=True`, che consente al database di memorizzare un valore `Null` se nessun autore è selezionato, e `on_delete=models.RESTRICT`, che impedirà la cancellazione dell'autore associato del libro se è referenziato da un qualsiasi libro.

> [!WARNING]
> Per impostazione predefinita `on_delete=models.CASCADE`, che significa che se l'autore venisse cancellato, anche questo libro sarebbe cancellato! Usiamo `RESTRICT` qui, ma potremmo anche usare `PROTECT` per impedire che l'autore venga eliminato mentre viene utilizzato da un qualsiasi libro o `SET_NULL` per impostare l'autore del libro su `Null` se il record viene cancellato.

Il modello definisce anche `__str__()`, utilizzando il campo `title` del libro per rappresentare un record `Book`. L'ultimo metodo, `get_absolute_url()`, restituisce un URL che può essere utilizzato per accedere a un record di dettaglio per questo modello (dovremo definire una mappatura URL che abbia il nome `book-detail`, e definire una vista e un template associati).

### Modello BookInstance

Successivamente, copia il modello `BookInstance` (mostrato sotto) sotto gli altri modelli. Il `BookInstance` rappresenta una copia specifica di un libro che qualcuno potrebbe prendere in prestito e include informazioni su se la copia è disponibile o in quale data ci si aspetta che ritorni, dettagli di "imprint" o versione, e un ID univoco per il libro nella biblioteca.

Alcuni dei campi e metodi ora saranno familiari. Il modello utilizza:

- `ForeignKey` per identificare il `Book` associato (ogni libro può avere molte copie, ma una copia può avere solo un `Book`). La chiave specifica `on_delete=models.RESTRICT` per assicurarsi che il `Book` non possa essere cancellato mentre è referenziato da un `BookInstance`.
- `CharField` per rappresentare l’"imprint" (rilascio specifico) del libro.

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

- `UUIDField` è usato per il campo `id` per impostarlo come la `primary_key` per questo modello.
  Questo tipo di campo assegna un valore univoco a livello globale per ogni istanza (uno per ogni libro che puoi trovare nella biblioteca).
- `DateField` è utilizzato per la data `due_back` (al quale si prevede che il libro diventi disponibile dopo essere stato preso in prestito o in manutenzione). Questo valore può essere `blank` o `null` (necessario per quando il libro è disponibile). I metadati del modello (`Class Meta`) utilizzano questo campo per ordinare i record quando vengono restituiti in una query.
- `status` è un `CharField` che definisce un elenco di scelta/selezione. Come puoi vedere, definiamo una tupla che contiene tuple di coppie chiave-valore e la passiamo all'argomento choices. Il valore in una coppia chiave/valore è un valore di visualizzazione che un utente può selezionare, mentre le chiavi sono i valori che vengono effettivamente salvati se l'opzione viene selezionata. Abbiamo impostato anche un valore predefinito di 'm' (manutenzione) perché i libri saranno inizialmente creati non disponibili prima di essere immessi sugli scaffali.

Il metodo `__str__()` rappresenta l'oggetto `BookInstance` utilizzando una combinazione del suo ID univoco e il titolo del `Book` associato.

> [!NOTE]
> Un po’ di Python:
>
> - A partire da Python 3.6, puoi usare la sintassi di interpolazione delle stringhe (nota anche come f-strings): `f'{self.id} ({self.book.title})'`.
> - Nelle versioni precedenti di questo tutorial, stavamo usando una sintassi di [stringa formattata](https://peps.python.org/pep-3101/), che è anche un modo valido di formattare le stringhe in Python (es., `'{0} ({1})'.format(self.id,self.book.title)`).

### Modello Author

Copia il modello `Author` (mostrato sotto) sotto al codice esistente nel file **models.py**.

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

Tutti i campi/metodi dovrebbero ora essere familiari. Il modello definisce un autore come avente un nome di battesimo, cognome e date di nascita e morte (entrambe opzionali). Specifica che per impostazione predefinita il metodo `__str__()` restituisce il nome in ordine _cognome_, _nome_. Il metodo `get_absolute_url()` inverte la mappatura URL `author-detail` per ottenere l'URL per visualizzare un autore individuale.

## Rieseguire le migrazioni del database

Tutti i modelli ora sono stati creati. Ora riesegui le migrazioni del database per aggiungerli al tuo database.

```bash
python3 manage.py makemigrations
python3 manage.py migrate
```

## Modello Language — sfida

Immagina che un benefattore locale doni un certo numero di nuovi libri scritti in un'altra lingua (diciamo, Farsi). La sfida è capire come questi sarebbero meglio rappresentati nel nostro sito web della biblioteca e poi aggiungerli ai modelli.

Alcune cose da considerare:

- La "lingua" dovrebbe essere associata a un `Book`, `BookInstance` o a qualche altro oggetto?
- Le diverse lingue dovrebbero essere rappresentate usando un modello, un campo di testo libero o un elenco di selezione hardcoded?

Dopo aver deciso, aggiungi il campo. Puoi vedere cosa abbiamo deciso [per il nostro progetto su GitHub](https://github.com/mdn/django-locallibrary-tutorial/blob/main/catalog/models.py).

Non dimenticare che dopo una modifica al tuo modello, dovresti nuovamente rieseguire le migrazioni del database per aggiungere le modifiche.

```bash
python3 manage.py makemigrations
python3 manage.py migrate
```

## Riepilogo

In questo articolo abbiamo imparato come sono definiti i modelli e poi abbiamo utilizzato queste informazioni per progettare e implementare modelli adeguati per il sito web _LocalLibrary_.

A questo punto ci allontaneremo brevemente dalla creazione del sito per dare un'occhiata al _sito di amministrazione di Django_. Questo sito ci permetterà di aggiungere alcuni dati alla biblioteca, che possiamo poi visualizzare usando le nostre viste e template (ancora da creare).

## Vedi anche

- [Scrivere la tua prima app Django, parte 2](https://docs.djangoproject.com/en/5.0/intro/tutorial02/) (documenti Django)
- [Effettuare query](https://docs.djangoproject.com/en/5.0/topics/db/queries/) (Documenti Django)
- [Riferimento API QuerySet](https://docs.djangoproject.com/en/5.0/ref/models/querysets/) (Documenti Django)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/skeleton_website", "Learn_web_development/Extensions/Server-side/Django/Admin_site", "Learn_web_development/Extensions/Server-side/Django")}}
