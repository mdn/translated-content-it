---
title: "Tutorial di Django Parte 3: Utilizzo dei modelli"
short-title: "3: Modelli"
slug: Learn_web_development/Extensions/Server-side/Django/Models
l10n:
  sourceCommit: e488eba036b2fee56444fd579c3759ef45ff2ca8
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/skeleton_website", "Learn_web_development/Extensions/Server-side/Django/Admin_site", "Learn_web_development/Extensions/Server-side/Django")}}

Questo articolo mostra come definire i modelli per il sito web LocalLibrary. Spiega cos'è un modello, come viene dichiarato e alcuni dei principali tipi di campo. Viene inoltre mostrato brevemente alcuni dei modi principali in cui si può accedere ai dati del modello.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/skeleton_website">Tutorial di Django Parte 2: Creazione di un sito web scheletro</a>.
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

Le applicazioni web Django accedono e gestiscono i dati attraverso oggetti Python denominati modelli. I modelli definiscono la _struttura_ dei dati memorizzati, inclusi i tipi di campo e possibilmente anche la loro dimensione massima, i valori predefiniti, le opzioni della lista di selezione, il testo di aiuto per la documentazione, il testo dell'etichetta per i moduli, ecc. La definizione del modello è indipendente dal database sottostante — è possibile scegliere uno di diversi come parte delle impostazioni del progetto. Una volta scelto quale database utilizzare, non è necessario comunicare direttamente con esso — basta scrivere la struttura del modello e altro codice, e Django gestisce tutto il lavoro sporco di comunicare con il database per lei.

Questo tutorial mostra come definire e accedere ai modelli per l'esempio del sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website).

## Progettare i modelli di LocalLibrary

Prima di immergersi e iniziare a codificare i modelli, vale la pena prendersi qualche minuto per pensare a quali dati dobbiamo memorizzare e alle relazioni tra i diversi oggetti.

Sappiamo che dobbiamo memorizzare informazioni sui libri (titolo, riassunto, autore, lingua scritta, categoria, ISBN) e che potremmo avere più copie disponibili (con ID unico globale, stato di disponibilità, ecc.). Potremmo aver bisogno di memorizzare più informazioni sull'autore oltre al loro nome, e ci potrebbero essere più autori con nomi uguali o simili. Vogliamo essere in grado di ordinare le informazioni in base al titolo del libro, all'autore, alla lingua scritta e alla categoria.

Quando si progettano i modelli, ha senso avere modelli separati per ogni "oggetto" (un gruppo di informazioni correlate). In questo caso, gli oggetti ovvi sono i libri, le istanze del libro e gli autori.

Si potrebbe anche voler utilizzare modelli per rappresentare le opzioni della lista di selezione (per esempio, come una lista a discesa di scelte), piuttosto che codificare le scelte direttamente nel sito web stesso — questo è raccomandato quando tutte le opzioni non sono conosciute in anticipo o possono cambiare. I candidati ovvi per i modelli, in questo caso, includono il genere del libro (per esempio, Fantascienza, Poesia Francese, ecc.) e la lingua (Inglese, Francese, Giapponese).

Una volta deciso sui nostri modelli e campi, dobbiamo pensare alle relazioni. Django permette di definire relazioni che sono uno a uno (`OneToOneField`), uno a molti (`ForeignKey`) e molti a molti (`ManyToManyField`).

Tenendo questo a mente, il diagramma di associazione UML qui sotto mostra i modelli che definiremo in questo caso (come scatole).

![LocalLibrary Model UML with fixed Author multiplicity inside the Book class](local_library_model_uml.svg)

Abbiamo creato modelli per il libro (i dettagli generici del libro), l'istanza del libro (stato delle copie fisiche specifiche del libro disponibili nel sistema) e l'autore. Abbiamo deciso anche di avere un modello per il genere in modo che i valori possano essere creati/selezionati tramite l'interfaccia di amministrazione. Abbiamo deciso di non avere un modello per lo `status` di `BookInstance` — abbiamo codificato i valori (`LOAN_STATUS`) perché non ci aspettiamo che questi cambino. All'interno di ciascuna scatola, si può vedere il nome del modello, i nomi dei campi e i tipi, e anche i metodi e i loro tipi di ritorno.

Il diagramma mostra anche le relazioni tra i modelli, comprese le loro _molteplicità_. Le molteplicità sono i numeri sul diagramma che mostrano il numero (massimo e minimo) di ciascun modello che può essere presente nella relazione. Ad esempio, la linea di collegamento tra le scatole mostra che un Libro e un Genere sono correlati. I numeri vicini al modello Genere mostrano che un libro deve avere uno o più Generi (quanti ne desidera), mentre i numeri dall'altra parte della linea accanto al modello Libro mostrano che un Genere può avere zero o molti libri associati.

> [!NOTE]
> La sezione successiva fornisce una guida di base che spiega come i modelli sono definiti e utilizzati. Mentre la legge, consideri come costruiremo ciascuno dei modelli nel diagramma sopra.

## Introduzione ai modelli

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

Nelle sezioni seguenti esploreremo ciascuna delle caratteristiche del modello nel dettaglio:

#### Campi

Un modello può avere un numero arbitrario di campi, di qualsiasi tipo — ciascuno rappresenta una colonna di dati che vogliamo memorizzare in una delle nostre tabelle di database. Ogni record del database (riga) sarà composto da uno di ciascun valore di campo. Diamo un'occhiata all'esempio visto di seguito:

```python
my_field_name = models.CharField(max_length=20, help_text='Enter field documentation')
```

Il nostro esempio sopra ha un unico campo chiamato `my_field_name`, di tipo `models.CharField` — il che significa che questo campo conterrà stringhe di caratteri alfanumerici. I tipi di campo sono assegnati utilizzando classi specifiche, che determinano il tipo di record che viene utilizzato per memorizzare i dati nel database, insieme ai criteri di validazione da utilizzare quando i valori sono ricevuti da un modulo HTML (cioè, cosa costituisce un valore valido). I tipi di campo possono anche prendere argomenti che specificano ulteriormente come il campo viene memorizzato o può essere utilizzato. In questo caso stiamo dando al nostro campo due argomenti:

- `max_length=20` — Indica che la lunghezza massima di un valore in questo campo è di 20 caratteri.
- `help_text='Enter field documentation'` — testo di aiuto che può essere visualizzato in un modulo per aiutare gli utenti a capire come viene utilizzato il campo.

Il nome del campo viene utilizzato per riferirsi ad esso nelle query e nei modelli.
I campi hanno anche un'etichetta, che è specificata utilizzando l'argomento `verbose_name` (con un valore predefinito di `None`).
Se `verbose_name` non è impostato, l'etichetta viene creata dal nome del campo sostituendo eventuali trattini bassi con uno spazio e capitalizzando la prima lettera (ad esempio, il campo `my_field_name` avrebbe un'etichetta predefinita di _My field name_ quando utilizzato nei moduli).

L'ordine in cui i campi sono dichiarati influenzerà il loro ordine predefinito se un modello viene visualizzato in un modulo (ad esempio, nel sito di amministrazione), anche se ciò può essere sostituito.

##### Argomenti comuni dei campi

I seguenti argomenti comuni possono essere usati quando si dichiarano molti/la maggior parte dei diversi tipi di campo:

- [help_text](https://docs.djangoproject.com/en/5.0/ref/models/fields/#help-text): Fornisce un'etichetta di testo per i moduli HTML (ad esempio, nel sito di amministrazione), come descritto sopra.
- [verbose_name](https://docs.djangoproject.com/en/5.0/ref/models/fields/#verbose-name): Un nome leggibile per il campo utilizzato nelle etichette dei campi. Se non specificato, Django dedurrà il nome esteso predefinito dal nome del campo.
- [default](https://docs.djangoproject.com/en/5.0/ref/models/fields/#default): Il valore predefinito per il campo. Questo può essere un valore o un oggetto chiamabile, nel qual caso l'oggetto sarà chiamato ogni volta che viene creato un nuovo record.
- [null](https://docs.djangoproject.com/en/5.0/ref/models/fields/#null): Se `True`, Django memorizzerà i valori vuoti come `NULL` nel database per i campi in cui ciò è appropriato (un `CharField` memorizzerà invece una stringa vuota). Il valore predefinito è `False`.
- [blank](https://docs.djangoproject.com/en/5.0/ref/models/fields/#blank): Se `True`, il campo è autorizzato a essere vuoto nei suoi moduli. Il valore predefinito è `False`, il che significa che la validazione del modulo di Django la costringerà a inserire un valore. Questo è spesso usato con `null=True`, perché se si intende consentire valori vuoti, si desidera anche che il database possa rappresentarli in modo appropriato.
- [choices](https://docs.djangoproject.com/en/5.0/ref/models/fields/#choices): Un gruppo di scelte per questo campo. Se questo è fornito, il widget di modulo predefinito corrispondente sarà una casella di selezione con queste scelte invece del campo di testo standard.
- [unique](https://docs.djangoproject.com/en/5.0/ref/models/fields/#unique):
  Se `True`, garantisce che il valore del campo sia unico in tutto il database.
  Questo può essere usato per prevenire la duplicazione di campi che non possono avere gli stessi valori.
  Il valore predefinito è `False`.
- [primary_key](https://docs.djangoproject.com/en/5.0/ref/models/fields/#primary-key):
  Se `True`, imposta il campo corrente come chiave primaria per il modello (Una chiave primaria è una colonna speciale del database designata per identificare univocamente tutti i diversi record di tabella).
  Se nessun campo è specificato come chiave primaria, Django aggiungerà automaticamente un campo per questo scopo.
  Il tipo di campi chiave primaria creati automaticamente può essere specificato per ogni app in [`AppConfig.default_auto_field`](https://docs.djangoproject.com/en/5.0/ref/applications/#django.apps.AppConfig.default_auto_field) o globalmente nell'impostazione [`DEFAULT_AUTO_FIELD`](https://docs.djangoproject.com/en/5.0/ref/settings/#std:setting-DEFAULT_AUTO_FIELD).

  > [!NOTE]
  > Le app create utilizzando **manage.py** impostano il tipo di chiave primaria su un [BigAutoField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#bigautofield).
  > È possibile vedere questo nel file **catalog/apps.py** della libreria locale:
  >
  > ```python
  > class CatalogConfig(AppConfig):
  >   default_auto_field = 'django.db.models.BigAutoField'
  > ```

Esistono molte altre opzioni — è possibile visualizzare l'[elenco completo delle opzioni di campo qui](https://docs.djangoproject.com/en/5.0/ref/models/fields/#field-options).

##### Tipi comuni di campi

Il seguente elenco descrive alcuni dei tipi di campi più comunemente usati.

- [CharField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#django.db.models.CharField) viene utilizzato per definire stringhe a lunghezza fissa da corte a medie. È necessario specificare la `max_length` dei dati da memorizzare.
- [TextField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#django.db.models.TextField) viene utilizzato per stringhe arbitrarie di lunghezza grande. È possibile specificare una `max_length` per il campo, ma questo viene usato solo quando il campo viene visualizzato nei moduli (non viene applicato a livello di database).
- [IntegerField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#django.db.models.IntegerField) è un campo per memorizzare valori interi (numeri interi) e per convalidare i valori inseriti come interi nei moduli.
- [DateField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#datefield) e [DateTimeField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#datetimefield) sono utilizzati per memorizzare/rappresentare informazioni su date e date/orari (rispettivamente come oggetti Python `datetime.date` e `datetime.datetime`). Questi campi possono dichiarare inoltre i parametri (mutuamente esclusivi) `auto_now=True` (per impostare il campo sulla data corrente ogni volta che il modello viene salvato), `auto_now_add` (per impostare la data solo quando il modello è creato per la prima volta), e `default` (per impostare una data predefinita che può essere sovrascritta dall'utente).
- [EmailField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#emailfield) è utilizzato per memorizzare e convalidare indirizzi email.
- [FileField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#filefield) e [ImageField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#imagefield) vengono utilizzati per caricare rispettivamente file e immagini (l'`ImageField` aggiunge una convalida aggiuntiva che il file caricato è un'immagine). Questi hanno parametri per definire come e dove sono memorizzati i file caricati.
- [AutoField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#autofield) è un tipo speciale di `IntegerField` che viene incrementato automaticamente. Una chiave primaria di questo tipo viene aggiunta automaticamente al tuo modello se non specifica esplicitamente una.
- [ForeignKey](https://docs.djangoproject.com/en/5.0/ref/models/fields/#foreignkey) viene utilizzato per specificare una relazione uno a molti a un altro modello di database (ad esempio, un'auto ha un produttore, ma un produttore può realizzare molte auto). Il lato "uno" della relazione è il modello che contiene la "chiave" (i modelli che contengono una "chiave esterna" riferendo a quella "chiave", sono sul lato "molti" di una tale relazione).
- [ManyToManyField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#manytomanyfield) viene utilizzato per specificare una relazione molti a molti (ad esempio, un libro può avere diversi generi e ciascun genere può contenere diversi libri). Nella nostra app della libreria lo utilizzeremo molto similmente ai `ForeignKeys`, ma possono essere utilizzati in modi più complicati per descrivere le relazioni tra gruppi. Questi hanno il parametro `on_delete` per definire cosa succede quando il record associato viene eliminato (ad esempio, un valore di `models.SET_NULL` imposterebbe il valore a `NULL`).

Ci sono molti altri tipi di campi, inclusi campi per diversi tipi di numeri (interi grandi, interi piccoli, numeri con virgola mobile), booleani, URL, slug, id unici e altre informazioni "relative al tempo" (durata, orario, ecc.). È possibile visualizzare l'[elenco completo qui](https://docs.djangoproject.com/en/5.0/ref/models/fields/#field-types).

#### Metadati

È possibile dichiarare metadati a livello di modello per il suo Modello dichiarando `class Meta`, come mostrato.

```python
class Meta:
    ordering = ['-my_field_name']
```

Una delle caratteristiche più utili di questi metadati è controllare l'_ordinamento predefinito_ dei record restituiti quando si interroga il tipo di modello. Lei fa questo specificando l'ordine del match in un elenco di nomi di campi all'attributo `ordering`, come mostrato sopra. L'ordinamento dipenderà dal tipo di campo (i campi di caratteri sono ordinati alfabeticamente, mentre i campi di data sono ordinati in ordine cronologico). Come mostrato sopra, è possibile anteporre il nome del campo a un simbolo meno (-) per invertire l'ordine di ordinamento.

Quindi, ad esempio, se scegliamo di ordinare i libri in questo modo per impostazione predefinita:

```python
ordering = ['title', '-publish_date']
```

i libri sarebbero ordinati alfabeticamente per titolo, da A-Z, e poi per data di pubblicazione all'interno di ciascun titolo, dal più recente al più vecchio.

Un altro attributo comune è `verbose_name`, un nome esteso per la classe in forma singolare e plurale:

```python
verbose_name = 'BetterName'
```

I metadati della classe possono essere utilizzati per creare e applicare nuove "autorizzazioni di accesso" per il modello (le autorizzazioni predefinite sono applicate automaticamente), consentire l'ordinamento basato su un altro campo, definire [Vincoli](https://docs.djangoproject.com/en/5.0/ref/models/constraints/) sui valori possibili dei dati che possono essere memorizzati, o per dichiarare che la classe è "astratta" (una classe base che non le permette di creare record, e verrà invece derivata per creare altri modelli).

Molte delle altre opzioni di metadati controllano quale database deve essere utilizzato per il modello e come i dati sono memorizzati (queste sono davvero utili solo se è necessario mappare un modello su un database esistente).

L'elenco completo delle opzioni di metadati è disponibile qui: [Opzioni di metadati dei modelli](https://docs.djangoproject.com/en/5.0/ref/models/options/) (Django docs).

#### Metodi

Un modello può anche avere metodi.

**Minimamente, in ogni modello dovreste definire il metodo standard della classe Python `__str__()` per restituire una stringa leggibile per ogni oggetto.** Questa stringa viene utilizzata per rappresentare i record individuali nel sito di amministrazione (e ovunque si abbia bisogno di fare riferimento a un'istanza del modello). Spesso questo restituirà un campo di titolo o nome dal modello.

```python
def __str__(self):
    return self.my_field_name
```

Un altro metodo comune da includere nei modelli Django è `get_absolute_url()`, che restituisce un URL per visualizzare i record individuali del modello sul sito web (se definisce questo metodo, Django aggiungerà automaticamente un pulsante "Visualizza sul Sito" alle schermate di modifica dei record del modello nel sito di amministrazione). Un modello tipico per `get_absolute_url()` è mostrato di seguito.

```python
def get_absolute_url(self):
    """Returns the URL to access a particular instance of the model."""
    return reverse('model-detail-view', args=[str(self.id)])
```

> [!NOTE]
> Supponendo che utilizzerà URL come `/my-application/my-model-name/2` per visualizzare record individuali per il suo modello (dove "2" è l'`id` per un particolare record), dovrà creare un mapper URL per passare la risposta e l'id a una "vista dettaglio modello" (che farà il lavoro necessario per visualizzare il record). La funzione `reverse()` sopra è in grado di "invertire" il tuo mapper URL (nel caso sopra chiamato _'model-detail-view'_) per creare un URL del formato corretto.
>
> Naturalmente, per farlo funzionare, deve ancora scrivere la mappatura URL, la vista e il template!

Può anche definire qualsiasi altro metodo che desidera e chiamarli dal suo codice o modelli (a condizione che non richiedano parametri).

### Gestione dei modelli

Una volta definite le classi del suo modello, può usarle per creare, aggiornare o eliminare record, e per eseguire interrogazioni per ottenere tutti i record o particolari sotto-insiemi di record. Le mostreremo come farlo nel tutorial quando definiremo le nostre viste, ma ecco un breve riepilogo.

#### Creare e modificare i record

Per creare un record, può definire un'istanza del modello e poi chiamare `save()`.

```python
# Create a new record using the model's constructor.
record = MyModelName(my_field_name="Instance #1")

# Save the object into the database.
record.save()
```

> [!NOTE]
> Se non ha dichiarato alcun campo come `primary_key`, al nuovo record verrà assegnato uno automaticamente, con il nome del campo `id`. Potrebbe consultare questo campo dopo aver salvato il record sopra e avrebbe un valore di 1.

Può accedere ai campi in questo nuovo record utilizzando la sintassi del punto e cambiare i valori. Deve chiamare `save()` per memorizzare i valori modificati nel database.

```python
# Access model field values using Python attributes.
print(record.id) # should return 1 for the first record.
print(record.my_field_name) # should print 'Instance #1'

# Change record by modifying the fields, then calling save().
record.my_field_name = "New Instance Name"
record.save()
```

#### Ricerca dei record

Può cercare i record che corrispondono a determinati criteri utilizzando l'attributo `objects` del modello (fornito dalla classe base).

> [!NOTE]
> Spiegare come cercare i record utilizzando nomi di modello e campo "astratti" può essere un po' confuso. Nella discussione seguente ci riferiremo a un modello `Book` con campi `title` e `genre`, dove `genre` è anche un modello con un singolo campo `name`.

Possiamo ottenere tutti i record per un modello come un `QuerySet`, usando `objects.all()`. Il `QuerySet` è un oggetto iterabile, il che significa che contiene un certo numero di oggetti su cui possiamo iterare/eseguire un loop.

```python
all_books = Book.objects.all()
```

Il metodo `filter()` di Django ci permette di filtrare il `QuerySet` restituito per corrispondere un campo **testuale** o **numerico** a criteri particolari. Ad esempio, per filtrare i libri che contengono "wild" nel titolo e quindi contarli, potremmo fare quanto segue:

```python
wild_books = Book.objects.filter(title__contains='wild')
number_wild_books = wild_books.count()
```

I campi da corrispondere e il tipo di corrispondenza sono definiti nel nome del parametro `filter`, utilizzando il formato: `field_name__match_type` (notare il _doppio trattino basso_ tra `title` e `contains` sopra). Sopra stiamo filtrando `title` con una corrispondenza sensibile alle maiuscole. Ci sono molti altri tipi di corrispondenza che si possono fare: `icontains` (sensibile alle minuscole), `iexact` (corrispondenza esatta insensibile alle maiuscole), `exact` (corrispondenza esatta sensibile alle maiuscole) e `in`, `gt` (maggiore di), `startswith`, ecc. L'elenco completo è disponibile qui](https://docs.djangoproject.com/en/5.0/ref/models/querysets/#field-lookups).

In alcuni casi, sarà necessario filtrare su un campo che definisce una relazione uno a molti con un altro modello (ad esempio, un `ForeignKey`). In questo caso, può "indicizzare" a campi all'interno del modello correlato con altri doppi trattini bassi.
Ad esempio, per filtrare i libri con uno schema specifico di genere, dovrà indicizzare al `name` attraverso il campo `genre`, come mostrato di seguito:

```python
# Will match on: Fiction, Science fiction, non-fiction etc.
books_containing_genre = Book.objects.filter(genre__name__icontains='fiction')
```

> [!NOTE]
> Può usare i trattini bassi (`__`) per navigare quanti più livelli di relazioni (`ForeignKey`/`ManyToManyField`) le piacciono.
> Ad esempio, un `Book` che aveva diversi tipi, definiti utilizzando un'ulteriore relazione "cover" potrebbe avere un nome di parametro: `type__cover__name__exact='hard'.`

Ci sono molte altre cose che si possono fare con le query, incluse le ricerche inverse dai modelli correlati, il concatenamento di filtri, il ritorno di un insieme più piccolo di valori, ecc. Per ulteriori informazioni, vedere [Fare query](https://docs.djangoproject.com/en/5.0/topics/db/queries/) (Django Docs).

## Definire i Modelli LocalLibrary

In questa sezione inizieremo a definire i modelli per la libreria. Apri `models.py` (in /django-locallibrary-tutorial/catalog/). Il boilerplate nella parte superiore della pagina importa il modulo _models_, che contiene la classe base del modello `models.Model` da cui ereditano i nostri modelli.

```python
from django.db import models

# Create your models here.
```

### Modello Genre

Copia il codice del modello `Genre` mostrato sotto e incollalo nella parte inferiore del file `models.py`. Questo modello viene utilizzato per memorizzare informazioni sulla categoria del libro — ad esempio se è finzione o non finzione, storia d'amore o storia militare, ecc.
Come menzionato sopra, abbiamo creato il genere come un modello piuttosto che come testo libero o una lista di selezione in modo che i valori possano essere gestiti attraverso il database piuttosto che essere codificati.

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

Il modello ha un singolo campo `CharField` (`name`), che viene utilizzato per descrivere il genere (questo è limitato a 200 caratteri e ha un po' di `help_text`).
Abbiamo impostato questo campo per essere unico (`unique=True`) perché dovrebbe esserci solo un record per ciascun genere.

Dopo il campo, dichiariamo un metodo `__str__()`, che restituisce il nome del genere definito da un particolare record. Nessun nome verboso è stato definito, quindi l'etichetta del campo sarà `Name` quando viene utilizzato nei moduli.
Poi dichiariamo il metodo `get_absolute_url()`, che restituisce un URL che può essere utilizzato per accedere a un record di dettaglio per questo modello (per far funzionare questo, dovremo definire una mappatura URL che ha il nome `genre-detail` e definire una vista associata e un modello).

L'impostazione di `unique=True` sul campo sopra impedisce che vengano creati generi con esattamente lo stesso nome, ma non variazioni come "fantasy", "Fantasy", o anche "FaNtAsY".
L'ultima parte della definizione del modello utilizza un'opzione [`constraints`](https://docs.djangoproject.com/en/5.0/ref/models/options/#constraints) sui [metadati](#metadati) del modello per specificare che il valore in minuscole nel campo `name` deve essere unico nel database e visualizzare la stringa `violation_error_message` se non lo è.
Qui non è necessario fare nient'altro, ma è possibile definire più vincoli contro un campo o campi.
Per ulteriori informazioni, vedere il [riferimento sui vincoli](https://docs.djangoproject.com/en/5.0/ref/models/constraints/), inclusi [`UniqueConstraint()`](https://docs.djangoproject.com/en/5.0/ref/models/constraints/#uniqueconstraint) (e [`Lower()`](https://docs.djangoproject.com/en/5.0/ref/models/database-functions/#lower)).

### Modello Book

Copia il modello `Book` di seguito e incolla nuovamente alla fine del tuo file. Il modello `Book` rappresenta tutte le informazioni su un libro disponibile in senso generale, ma non su una particolare "istanza" o "copia" fisica disponibile per il prestito.

Il modello utilizza un `CharField` per rappresentare il `title` e l'`isbn` del libro.
Per `isbn`, nota come il primo parametro senza nome imposta esplicitamente l'etichetta come "ISBN" (altrimenti, sarebbe stato "Isbn"). Impostiamo anche il parametro `unique` come `true` per garantire che tutti i libri abbiano un ISBN univoco (il parametro unique rende il valore del campo univoco globalmente in una tabella).
A differenza dell'`isbn` (e del nome del genere), il `title` non è impostato per essere unico, perché possono esserci diversi libri con lo stesso nome.
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

Il genere è un `ManyToManyField`, quindi un libro può avere più generi e un genere può avere molti libri. L'autore è dichiarato come `ForeignKey`, quindi ogni libro avrà un solo autore, ma un autore può avere molti libri (nella pratica un libro potrebbe avere più autori, ma non in questa implementazione!)

In entrambi i tipi di campo la classe modello correlata è dichiarata come primo parametro senza nome utilizzando sia la classe del modello che una stringa contenente il nome del modello correlato. Deve usare il nome del modello come una stringa se la classe associata non è ancora stata definita in questo file prima di essere referenziata! Gli altri parametri di interesse nel campo `author` sono `null=True`, che permette al database di memorizzare un valore `Null` se nessun autore è selezionato, e `on_delete=models.RESTRICT`, che impedirà l'eliminazione dell'autore associato al libro se è referenziato da un qualsiasi libro.

> [!WARNING]
> Per impostazione predefinita, `on_delete=models.CASCADE`, il che significa che se l'autore venisse eliminato, questo libro verrebbe eliminato anche lui! Utilizziamo `RESTRICT` qui, ma potremmo anche usare `PROTECT` per impedire che l'autore venga eliminato mentre un libro lo utilizza o `SET_NULL` per impostare l'autore del libro su `Null` se il record viene eliminato.

Il modello definisce anche `__str__()`, utilizzando il campo `title` del libro per rappresentare un record `Book`. L'ultimo metodo, `get_absolute_url()` restituisce un URL che può essere utilizzato per accedere a un record di dettaglio per questo modello (dovremo definire una mappatura URL che ha il nome `book-detail`, e definire una vista associata e un modello).

### Modello BookInstance

Successivamente, copia il modello `BookInstance` (mostrato di seguito) sotto agli altri modelli. Il `BookInstance` rappresenta una copia specifica di un libro che qualcuno potrebbe prendere in prestito, e include informazioni sull'eventuale disponibilità della copia o su quale data è prevista la restituzione, i dettagli dell'"imprint" o della versione, e un ID unico per il libro nella libreria.

Alcuni dei campi e metodi le saranno ormai familiari. Il modello utilizza:

- `ForeignKey` per identificare il `Book` associato (ogni libro può avere molte copie, ma una copia può avere solo un `Book`). La chiave specifica `on_delete=models.RESTRICT` per garantire che il `Book` non possa essere eliminato mentre viene referenziato da un `BookInstance`.
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

- `UUIDField` è utilizzato per il campo `id` per impostarlo come `primary_key` per questo modello.
  Questo tipo di campo assegna un valore univoco globalmente per ciascuna istanza (uno per ogni libro che può trovare nella libreria).
- `DateField` è utilizzato per la data `due_back` (alla quale il libro è previsto per diventare disponibile dopo essere stato preso in prestito o in manutenzione). Questo valore può essere `blank` o `null` (necessario per quando il libro è disponibile). I metadati del modello (`Class Meta`) usano questo campo per ordinare i record quando vengono restituiti in una query.
- `status` è un `CharField` che definisce una lista di scelta/selezione. Come può vedere, definiamo una tupla contenente tuple di coppie chiave-valore e la passiamo all'argomento delle scelte. Il valore in una coppia chiave/valore è un valore di visualizzazione che un utente può selezionare, mentre le chiavi sono i valori che vengono effettivamente salvati se l'opzione è selezionata. Abbiamo anche impostato un valore predefinito di 'm' (manutenzione) poiché i libri saranno inizialmente creati non disponibili prima di essere immagazzinati sugli scaffali.

Il metodo `__str__()` rappresenta l'oggetto `BookInstance` utilizzando una combinazione del suo ID unico e il titolo del `Book` associato.

> [!NOTE]
> Un po' di Python:
>
> - A partire da Python 3.6, può utilizzare la sintassi di interpolazione delle stringhe (nota anche come f-strings): `f'{self.id} ({self.book.title})'`.
> - Nelle versioni precedenti di questo tutorial, stavamo usando una sintassi [di formato stringa](https://peps.python.org/pep-3101/), che è anche un modo valido di formattare le stringhe in Python (ad esempio, `'{0} ({1})'.format(self.id,self.book.title)`).

### Modello Author

Copia il modello `Author` (mostrato sotto) sotto il codice esistente in **models.py**.

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

Tutti i campi/metodi dovrebbero ora essere familiari. Il modello definisce un autore come avente un nome, cognome e date di nascita e morte (entrambe opzionali). Specifica che il predefinito `__str__()` restituisce il nome nell'ordine _cognome_, _nome_. Il metodo `get_absolute_url()` inverte la mappatura URL `author-detail` per ottenere l'URL per visualizzare un autore individuale.

## Eseguire nuovamente le migrazioni del database

Ora tutti i modelli sono stati creati. Ora esegua nuovamente le migrazioni del database per aggiungerli al suo database.

```bash
python3 manage.py makemigrations
python3 manage.py migrate
```

## Modello Language — sfida

Immagini che un benefattore locale doni un certo numero di nuovi libri scritti in un'altra lingua (per esempio, Farsi). La sfida è capire come questi verrebbero meglio rappresentati nel nostro sito web della biblioteca, e quindi aggiungerli ai modelli.

Alcune cose da considerare:

- Il "language" dovrebbe essere associato a un `Book`, `BookInstance`, o a qualche altro oggetto?
- Le diverse lingue dovrebbero essere rappresentate utilizzando un modello, un campo di testo libero, o una lista di selezione codificata?

Dopo aver deciso, aggiunga il campo. Può vedere cosa abbiamo deciso [per il nostro progetto su GitHub](https://github.com/mdn/django-locallibrary-tutorial/blob/main/catalog/models.py).

Non dimentichi che dopo una modifica al suo modello, dovrebbe eseguire nuovamente le migrazioni del database per aggiungere le modifiche.

```bash
python3 manage.py makemigrations
python3 manage.py migrate
```

## Sommario

In questo articolo abbiamo appreso come vengono definiti i modelli, e poi utilizzato queste informazioni per progettare e implementare modelli appropriati per il sito web _LocalLibrary_.

A questo punto ci prenderemo una breve pausa dalla creazione del sito e daremo un'occhiata al _sito di amministrazione di Django_. Questo sito ci permetterà di aggiungere alcuni dati alla libreria, che poi potremo visualizzare utilizzando le nostre (ancora da creare) viste e modelli.

## Vedere anche

- [Scrivere la sua prima app Django, parte 2](https://docs.djangoproject.com/en/5.0/intro/tutorial02/) (Django docs)
- [Fare query](https://docs.djangoproject.com/en/5.0/topics/db/queries/) (Django Docs)
- [QuerySet API Reference](https://docs.djangoproject.com/en/5.0/ref/models/querysets/) (Django Docs)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/skeleton_website", "Learn_web_development/Extensions/Server-side/Django/Admin_site", "Learn_web_development/Extensions/Server-side/Django")}}
