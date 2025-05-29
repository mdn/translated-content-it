---
title: "Tutorial di Django Parte 3: Uso dei modelli"
short-title: "3: Modelli"
slug: Learn_web_development/Extensions/Server-side/Django/Models
l10n:
  sourceCommit: cb25e0acbd9f0af27c4a99965cb962230d49a35d
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/skeleton_website", "Learn_web_development/Extensions/Server-side/Django/Admin_site", "Learn_web_development/Extensions/Server-side/Django")}}

Questo articolo mostra come definire modelli per il sito web LocalLibrary. Spiega che cos'è un modello, come viene dichiarato, e alcuni dei principali tipi di campo. Mostra anche brevemente alcuni dei principali modi in cui è possibile accedere ai dati dei modelli.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/skeleton_website">Django Tutorial Parte 2: Creazione di un sito web scheletro</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        <p>
          Essere in grado di progettare e creare i propri modelli, scegliendo correttamente i campi.
        </p>
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

Le applicazioni web di Django accedono e gestiscono i dati tramite oggetti Python chiamati modelli. I modelli definiscono la _struttura_ dei dati memorizzati, inclusi i tipi di campo e possibilmente anche la loro dimensione massima, i valori predefiniti, le opzioni della lista di selezione, il testo di aiuto per la documentazione, il testo dell'etichetta per i moduli, ecc. La definizione del modello è indipendente dal database sottostante: è possibile sceglierne uno tra diversi come parte delle impostazioni del progetto. Una volta scelto il database che si desidera utilizzare, non è necessario interagirvi direttamente: basta scrivere la struttura del modello e il resto del codice, e Django si occupa di tutto il lavoro sporco di comunicare con il database.

Questo tutorial mostra come definire e accedere ai modelli per l'esempio di sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website).

## Progettazione dei modelli LocalLibrary

Prima di iniziare a scrivere il codice dei modelli, vale la pena prendersi qualche minuto per riflettere sui dati che è necessario memorizzare e sulle relazioni tra i diversi oggetti.

Sappiamo che dobbiamo memorizzare informazioni sui libri (titolo, riassunto, autore, lingua scritta, categoria, ISBN) e che potremmo avere più copie disponibili (con ID globale unico, stato di disponibilità, ecc.). Potremmo dover memorizzare più informazioni sull'autore rispetto al semplice nome, e potrebbero esserci più autori con nomi uguali o simili. Vogliamo poter ordinare le informazioni in base al titolo del libro, all'autore, alla lingua scritta e alla categoria.

Quando si progettano i modelli, ha senso avere modelli separati per ogni "oggetto" (un gruppo di informazioni correlate). In questo caso, gli oggetti ovvi sono libri, istanze di libri e autori.

Si potrebbe anche voler utilizzare i modelli per rappresentare le opzioni della lista di selezione (ad esempio, come un elenco a discesa di scelte), piuttosto che codificare le scelte nel sito web stesso - questo è raccomandato quando non si conoscono tutte le opzioni in anticipo o potrebbero cambiare. Candidati evidenti per i modelli, in questo caso, includono il genere del libro (ad esempio, fantascienza, poesia francese, ecc.) e la lingua (inglese, francese, giapponese).

Una volta decisi i modelli e i campi, è necessario pensare alle relazioni. Django consente di definire relazioni che sono uno a uno (`OneToOneField`), uno a molti (`ForeignKey`) e molti a molti (`ManyToManyField`).

Tenendo presente ciò, il diagramma di associazione UML qui sotto mostra i modelli che definiremo in questo caso (come caselle).

![Modello UML LocalLibrary con molteplicità autore fissa all'interno della classe Book](local_library_model_uml.svg)

Abbiamo creato modelli per il libro (i dettagli generici del libro), l'istanza del libro (lo stato delle copie fisiche specifiche del libro disponibili nel sistema) e l'autore. Abbiamo anche deciso di avere un modello per il genere in modo che i valori possano essere creati/selezionati attraverso l'interfaccia di amministrazione. Abbiamo deciso di non avere un modello per `BookInstance:status` — abbiamo codificato i valori (`LOAN_STATUS`) perché non ci aspettiamo che cambino. All'interno di ciascuna delle caselle, è possibile vedere il nome del modello, i nomi e i tipi dei campi, nonché i metodi e i loro tipi di ritorno.

Il diagramma mostra anche le relazioni tra i modelli, comprese le loro _molteplici_ molteplicità. Le molteplicità sono i numeri sul diagramma che mostrano i numeri (massimi e minimi) di ciascun modello che possono essere presenti nella relazione. Ad esempio, la linea di collegamento tra le caselle mostra che Book e un Genere sono correlati. I numeri vicini al modello Genere mostrano che un libro deve avere uno o più generi (quanti ne vuoi), mentre i numeri dall'altra parte della linea accanto al modello Book mostrano che un Genere può avere zero o molti libri associati.

> [!NOTE]
> La sezione successiva fornisce una panoramica di base che spiega come i modelli sono definiti e utilizzati. Mentre la leggi, considera come costruiremo ciascuno dei modelli nel diagramma sopra.

## Introduzione al modello

Questa sezione fornisce una breve panoramica su come viene definito un modello e alcuni dei campi e degli argomenti di campo più importanti.

### Definizione del modello

I modelli sono solitamente definiti nel file **models.py** di un'applicazione. Sono implementati come sottoclassi di `django.db.models.Model` e possono includere campi, metodi e metadati. Il frammento di codice qui sotto mostra un modello "tipico", chiamato `MyModelName`:

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

Nelle sezioni seguenti esploreremo ciascuna delle caratteristiche all'interno del modello in dettaglio:

#### Campi

Un modello può avere un numero arbitrario di campi, di qualsiasi tipo — ognuno rappresenta una colonna di dati che vogliamo memorizzare in una delle nostre tabelle del database. Ogni record del database (riga) consisterà in uno di ciascun valore di campo. Diamo un'occhiata all'esempio visto sotto:

```python
my_field_name = models.CharField(max_length=20, help_text='Enter field documentation')
```

Il nostro esempio sopra ha un singolo campo chiamato `my_field_name`, di tipo `models.CharField` — il che significa che questo campo conterrà stringhe di caratteri alfanumerici. I tipi di campo sono assegnati utilizzando classi specifiche, che determinano il tipo di record che viene utilizzato per memorizzare i dati nel database, insieme ai criteri di validazione da utilizzare quando i valori vengono ricevuti da un modulo HTML (cioè cosa costituisce un valore valido). I tipi di campo possono anche accettare argomenti che specificano ulteriormente come il campo viene memorizzato o può essere utilizzato. In questo caso stiamo dando al nostro campo due argomenti:

- `max_length=20` — Indica che la lunghezza massima di un valore in questo campo è di 20 caratteri.
- `help_text='Enter field documentation'` — testo di aiuto che può essere visualizzato in un modulo per aiutare gli utenti a capire come viene utilizzato il campo.

Il nome del campo viene utilizzato per riferirsi ad esso nelle query e nei modelli.
I campi hanno anche un'etichetta, che viene specificata utilizzando l'argomento `verbose_name` (con un valore predefinito di `None`).
Se `verbose_name` non è impostato, l'etichetta viene creata dal nome del campo sostituendo eventuali trattini bassi con uno spazio e capitalizzando la prima lettera (ad esempio, il campo `my_field_name` avrebbe un'etichetta predefinita di _Nome del campo mio_ quando utilizzato nei moduli).

L'ordine in cui i campi sono dichiarati influenzerà il loro ordine predefinito se un modello viene reso in un modulo (ad esempio, nel sito dell'amministratore), anche se ciò può essere sovrascritto.

##### Argomenti di campo comuni

I seguenti argomenti comuni possono essere utilizzati quando si dichiarano molti/la maggior parte dei diversi tipi di campo:

- [help_text](https://docs.djangoproject.com/en/5.0/ref/models/fields/#help-text): Fornisce un'etichetta di testo per i moduli HTML (ad esempio, nel sito dell'amministratore), come descritto sopra.
- [verbose_name](https://docs.djangoproject.com/en/5.0/ref/models/fields/#verbose-name): Un nome leggibile per il campo utilizzato nelle etichette dei campi. Se non specificato, Django dedurrà il nome predefinito verbose dal nome del campo.
- [default](https://docs.djangoproject.com/en/5.0/ref/models/fields/#default): Il valore predefinito per il campo. Questo può essere un valore o un oggetto callable, nel qual caso l'oggetto verrà chiamato ogni volta che viene creato un nuovo record.
- [null](https://docs.djangoproject.com/en/5.0/ref/models/fields/#null): Se `True`, Django memorizzerà i valori vuoti come `NULL` nel database per i campi in cui ciò è appropriato (un `CharField` memorizzerà invece una stringa vuota). Il valore predefinito è `False`.
- [blank](https://docs.djangoproject.com/en/5.0/ref/models/fields/#blank): Se `True`, il campo può essere lasciato vuoto nei tuoi moduli. Il valore predefinito è `False`, il che significa che la validazione del modulo di Django ti costringerà a inserire un valore. Questo viene spesso utilizzato con `null=True`, perché se si consente ai valori di essere vuoti, si vuole anche che il database sia in grado di rappresentarli in modo appropriato.
- [choices](https://docs.djangoproject.com/en/5.0/ref/models/fields/#choices): Un gruppo di scelte per questo campo. Se questo è fornito, il widget del modulo di default corrispondente sarà una casella di selezione con queste scelte anziché il normale campo di testo.
- [unique](https://docs.djangoproject.com/en/5.0/ref/models/fields/#unique):
  Se `True`, assicura che il valore del campo sia unico nel database.
  Questo può essere utilizzato per prevenire la duplicazione di campi che non possono avere gli stessi valori.
  Il valore predefinito è `False`.
- [primary_key](https://docs.djangoproject.com/en/5.0/ref/models/fields/#primary-key):
  Se `True`, imposta il campo corrente come chiave primaria per il modello (una chiave primaria è una colonna speciale del database designata per identificare in modo univoco tutti i diversi record della tabella).
  Se nessun campo è specificato come chiave primaria, Django aggiungerà automaticamente un campo a questo scopo.
  Il tipo di campi chiave primaria auto-creati può essere specificato per ciascuna app in [`AppConfig.default_auto_field`](https://docs.djangoproject.com/en/5.0/ref/applications/#django.apps.AppConfig.default_auto_field) o globalmente nell'impostazione [`DEFAULT_AUTO_FIELD`](https://docs.djangoproject.com/en/5.0/ref/settings/#std:setting-DEFAULT_AUTO_FIELD).

  > [!NOTE]
  > Le app create utilizzando **manage.py** impostano il tipo della chiave primaria su un [BigAutoField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#bigautofield).
  > Puoi vedere questo nel file **catalogo/apps.py** della libreria locale:
  >
  > ```python
  > class CatalogConfig(AppConfig):
  >   default_auto_field = 'django.db.models.BigAutoField'
  > ```

Ci sono molte altre opzioni - puoi visualizzare [l'elenco completo delle opzioni di campo qui](https://docs.djangoproject.com/en/5.0/ref/models/fields/#field-options).

##### Tipi di campi comuni

Il seguente elenco descrive alcuni dei tipi di campi più comunemente usati.

- [CharField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#django.db.models.CharField) viene utilizzato per definire stringhe a lunghezza fissa da piccole a medie dimensioni. Devi specificare `max_length` dei dati da memorizzare.
- [TextField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#django.db.models.TextField) viene utilizzato per stringhe di lunghezza arbitraria di grandi dimensioni. Puoi specificare una `max_length` per il campo, ma questo viene utilizzato solo quando il campo è visualizzato nei moduli (non è applicato a livello di database).
- [IntegerField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#django.db.models.IntegerField) è un campo per memorizzare valori interi (numeri interi) e per validare i valori inseriti come interi nei moduli.
- [DateField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#datefield) e [DateTimeField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#datetimefield) vengono utilizzati per memorizzare/rappresentare informazioni relative a date e tempo (come oggetti Python `datetime.date` e `datetime.datetime`, rispettivamente). Questi campi possono dichiarare anche i parametri (mutuamente esclusivi) `auto_now=True` (per impostare il campo alla data attuale ogni volta che il modello è salvato), `auto_now_add` (per impostare solo la data quando il modello è creato inizialmente), e `default` (per impostare una data predefinita che può essere sovrascritta dall'utente).
- [EmailField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#emailfield) viene utilizzato per memorizzare e validare indirizzi email.
- [FileField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#filefield) e [ImageField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#imagefield) sono utilizzati rispettivamente per caricare file e immagini (il `ImageField` aggiunge una validazione aggiuntiva che il file caricato è un'immagine). Questi hanno parametri per definire come e dove sono memorizzati i file caricati.
- [AutoField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#autofield) è un tipo speciale di `IntegerField` che si incrementa automaticamente. Una chiave primaria di questo tipo viene aggiunta automaticamente al tuo modello se non specifichi esplicitamente una.
- [ForeignKey](https://docs.djangoproject.com/en/5.0/ref/models/fields/#foreignkey) è utilizzato per specificare una relazione uno-a-molti con un altro modello di database (ad esempio, un'auto ha un solo produttore, ma un produttore può fare molte auto). Il lato "uno" della relazione è il modello che contiene la "chiave" (i modelli che contengono una "chiave esterna" che si riferiscono a quella "chiave", sono sul lato "molti" di una tale relazione).
- [ManyToManyField](https://docs.djangoproject.com/en/5.0/ref/models/fields/#manytomanyfield) è utilizzato per specificare una relazione molti-a-molti (ad esempio, un libro può avere diversi generi, e ogni genere può contenere diversi libri). Nella nostra app della biblioteca useremo questi in modo molto simile a `ForeignKeys`, ma possono essere utilizzati in modi più complicati per descrivere le relazioni tra gruppi. Questi hanno il parametro `on_delete` per definire cosa succede quando il record associato viene eliminato (ad esempio, un valore di `models.SET_NULL` imposterebbe il valore a `NULL`).

Esistono molti altri tipi di campi, inclusi campi per diversi tipi di numeri (grandi interi, piccoli interi, numeri decimali), booleani, URL, slug, ID univoci e altre informazioni "relative al tempo" (durata, orario, ecc.). Puoi visualizzare [l'elenco completo qui](https://docs.djangoproject.com/en/5.0/ref/models/fields/#field-types).

#### Metadata

Puoi dichiarare i metadati a livello di modello per il tuo Modello dichiarando `class Meta`, come mostrato.

```python
class Meta:
    ordering = ['-my_field_name']
```

Una delle caratteristiche più utili di questi metadati è controllare l'_ordinamento predefinito_ dei record restituiti quando si esegue una query sul tipo di modello. Fai questo specificando l'ordine di corrispondenza in un elenco di nomi di campo nell'attributo `ordering`, come mostrato sopra. L'ordine dipenderà dal tipo di campo (i campi di carattere sono ordinati alfabeticamente, mentre i campi data sono ordinati in ordine cronologico). Come mostrato sopra, puoi anteporre al nome del campo un simbolo meno (-) per invertire l'ordine di ordinamento.

Quindi, come esempio, se scegliessimo di ordinare i libri in questo modo per impostazione predefinita:

```python
ordering = ['title', '-publish_date']
```

i libri sarebbero ordinati alfabeticamente per titolo, da A a Z, e quindi per data di pubblicazione all'interno di ciascun titolo, dai più recenti ai più vecchi.

Un altro attributo comune è `verbose_name`, un nome descrittivo per la classe in forma singolare e plurale:

```python
verbose_name = 'BetterName'
```

I metadati della classe possono essere utilizzati per creare e applicare nuove "autorizzazioni di accesso" per il modello (le autorizzazioni predefinite vengono applicate automaticamente), consentire l'ordinamento basato su un altro campo, definire [vincoli](https://docs.djangoproject.com/en/5.0/ref/models/constraints/) sui valori possibili dei dati che possono essere memorizzati, o dichiarare che la classe è "astratta" (una classe di base per cui non puoi creare record, e sarà invece derivata per creare altri modelli).

Molte delle altre opzioni dei metadati controllano quale database deve essere utilizzato per il modello e come vengono memorizzati i dati (queste sono realmente utili solo se hai bisogno di mappare un modello su un database esistente).

L'elenco completo delle opzioni dei metadati è disponibile qui: [Opzioni dei metadati del Modello](https://docs.djangoproject.com/en/5.0/ref/models/options/) (Documentazione di Django).

#### Metodi

Un modello può anche avere metodi.

**Minimamente, in ogni modello dovresti definire il metodo standard di classe Python `__str__()` per restituire una stringa leggibile per ogni oggetto.** Questa stringa viene utilizzata per rappresentare i record individuali nel sito di amministrazione (e ovunque tu abbia bisogno di riferirti a un'istanza del modello). Spesso questo restituirà un campo di titolo o nome del modello.

```python
def __str__(self):
    return self.my_field_name
```

Un altro metodo comune da includere nei modelli Django è `get_absolute_url()`, che restituisce un URL per la visualizzazione dei record di modello individuali sul sito web (se definisci questo metodo, Django aggiungerà automaticamente un pulsante "Visualizza sul Sito" alle schermate di modifica dei record del modello nel sito dell'amministratore). Un modello tipico per `get_absolute_url()` è mostrato di seguito.

```python
def get_absolute_url(self):
    """Returns the URL to access a particular instance of the model."""
    return reverse('model-detail-view', args=[str(self.id)])
```

> [!NOTE]
> Supponendo che utilizzerai URL come `/my-application/my-model-name/2` per visualizzare i record individuali per il tuo modello (dove "2" è l'`id` per un determinato record), dovrai creare un mapper di URL per passare la risposta e l'id a una "visione dettagliata del modello" (che svolgerà il lavoro necessario per visualizzare il record). La funzione `reverse()` sopra è in grado di "invertire" il tuo mapper di URL (nel caso sopra chiamato _'model-detail-view'_) per creare un URL del formato corretto.
>
> Naturalmente, per far funzionare questo, devi comunque scrivere il mapping URL, la visione e il modello!

Puoi anche definire tutti gli altri metodi che desideri, e chiamarli dal tuo codice o dai modelli (a condizione che non prendano alcun parametro).

### Gestione del modello

Una volta definite le classi dei tuoi modelli, puoi utilizzarle per creare, aggiornare o eliminare record, e per eseguire query per ottenere tutti i record o particolari sottoinsiemi di record. Lo mostreremo nel tutorial quando definiamo le nostre viste, ma ecco un breve riassunto.

#### Creazione e modifica dei record

Per creare un record puoi definire un'istanza del modello e poi chiamare `save()`.

```python
# Create a new record using the model's constructor.
record = MyModelName(my_field_name="Instance #1")

# Save the object into the database.
record.save()
```

> [!NOTE]
> Se non hai dichiarato alcun campo come `primary_key`, al nuovo record verrà dato uno automaticamente, con il nome del campo `id`. Potresti interrogare questo campo dopo aver salvato il record sopra, e avrebbe un valore di 1.

Puoi accedere ai campi in questo nuovo record utilizzando la sintassi a punti e modificare i valori. Devi chiamare `save()` per memorizzare i valori modificati nel database.

```python
# Access model field values using Python attributes.
print(record.id) # should return 1 for the first record.
print(record.my_field_name) # should print 'Instance #1'

# Change record by modifying the fields, then calling save().
record.my_field_name = "New Instance Name"
record.save()
```

#### Ricerca dei record

Puoi cercare i record che corrispondono a determinati criteri utilizzando l'attributo `objects` del modello (fornito dalla classe base).

> [!NOTE]
> Spiegare come cercare i record utilizzando nomi di modelli e campi "astratti" può essere un po' confuso. Nella discussione di seguito, ci riferiremo a un modello `Book` con campi `title` e `genre`, dove il genere è anche un modello con un solo campo `name`.

Possiamo ottenere tutti i record per un modello come `QuerySet`, usando `objects.all()`. Il `QuerySet` è un oggetto iterabile, il che significa che contiene un numero di oggetti che possiamo iterare/scorrere.

```python
all_books = Book.objects.all()
```

Il metodo `filter()` di Django ci permette di filtrare il `QuerySet` restituito per corrispondere un certo campo **testo** o **numerico** a particolari criteri. Ad esempio, per filtrare i libri che contengono "wild" nel titolo e poi contarli, potremmo fare quanto segue:

```python
wild_books = Book.objects.filter(title__contains='wild')
number_wild_books = wild_books.count()
```

I campi da confrontare e il tipo di confronto sono definiti nel nome del parametro del filtro, usando il formato: `field_name__match_type` (nota il _doppio trattino basso_ tra `title` e `contains` sopra). Sopra stiamo filtrando `title` con una corrispondenza case-sensitive. Ci sono molti altri tipi di corrispondenze che puoi fare: `icontains` (case-insensitive), `iexact` (corrispondenza esatta case-insensitive), `exact` (corrispondenza esatta case-sensitive) e `in`, `gt` (maggiore di), `startswith`, ecc. La [lista completa è qui](https://docs.djangoproject.com/en/5.0/ref/models/querysets/#field-lookups).

In alcuni casi, sarà necessario filtrare su un campo che definisce una relazione uno a molti con un altro modello (ad esempio, un `ForeignKey`). In questo caso, puoi "indicizzare" ai campi all'interno del modello correlato con ulteriori doppi trattini bassi. Quindi, ad esempio, per filtrare i libri con un determinato schema di genere, dovrai indicizzare al `name` attraverso il campo `genre`, come mostrato di seguito:

```python
# Will match on: Fiction, Science fiction, non-fiction etc.
books_containing_genre = Book.objects.filter(genre__name__icontains='fiction')
```

> [!NOTE]
> Puoi utilizzare gli underscore (`__`) per navigare quanti livelli di relazioni (`ForeignKey`/`ManyToManyField`) desideri.
> Ad esempio, un `Book` che aveva diversi tipi, definiti utilizzando un'ulteriore relazione "cover" potrebbe avere un nome di parametro: `type__cover__name__exact='hard'`.

Ci sono molte altre cose che puoi fare con le query, inclusi ricerche all'indietro dai modelli correlati, concatenazione dei filtri, restituzione di un set di valori più piccolo, ecc. Per ulteriori informazioni, vedi [Esecuzione delle query](https://docs.djangoproject.com/en/5.0/topics/db/queries/) (Documentazione di Django).

## Definizione dei modelli LocalLibrary

In questa sezione inizieremo a definire i modelli per la biblioteca. Apri `models.py` (in /django-locallibrary-tutorial/catalog/). Il codice boilerplate nella parte superiore della pagina importa il modulo _models_, che contiene la classe base del modello `models.Model` da cui erediteranno i nostri modelli.

```python
from django.db import models

# Create your models here.
```

### Modello Genre

Copia il codice del modello `Genre` mostrato qui sotto e incollalo nella parte inferiore del tuo file `models.py`. Questo modello viene utilizzato per memorizzare informazioni sulla categoria del libro — ad esempio se è narrativa o saggistica, romanzo o storia militare, ecc. Come accennato sopra, abbiamo creato il genere come un modello piuttosto che come testo libero o una lista di selezione in modo che i valori possibili possano essere gestiti attraverso il database piuttosto che essere codificati.

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

Il modello ha un solo campo `CharField` (`name`), che viene utilizzato per descrivere il genere (questo è limitato a 200 caratteri e ha un po' di `help_text`). Abbiamo impostato questo campo come unico (`unique=True`) perché dovrebbe esserci solo un record per ogni genere.

Dopo il campo, dichiariamo un metodo `__str__()`, che restituisce il nome del genere definito da un determinato record. Non è stato definito alcun nome verbose, quindi l'etichetta del campo sarà `Name` quando viene utilizzata nei moduli. Poi dichiariamo il metodo `get_absolute_url()`, che restituisce un URL che può essere utilizzato per accedere a un record di dettaglio per questo modello (per farlo funzionare, dovremo definire un mapping di URL che abbia il nome `genre-detail`, e definire una vista e un modello associati).

Impostare `unique=True` sul campo sopra impedisce la creazione di generi con _esattamente_ lo stesso nome, ma non variazioni come "fantasy", "Fantasy", o anche "FaNtAsY". L'ultima parte della definizione del modello utilizza un'opzione [`constraints`](https://docs.djangoproject.com/en/5.0/ref/models/options/#constraints) nei [metadati](#metadata) del modello per specificare che il valore in minuscolo nel campo `name` deve essere unico nel database, e visualizza la stringa `violation_error_message` se non lo è. Qui non c'è bisogno di fare nient'altro, ma puoi definire più vincoli contro un campo o campi. Per ulteriori informazioni, vedi il [Riferimento dei vincoli](https://docs.djangoproject.com/en/5.0/ref/models/constraints/), inclusi [`UniqueConstraint()`](https://docs.djangoproject.com/en/5.0/ref/models/constraints/#uniqueconstraint) (e [`Lower()`](https://docs.djangoproject.com/en/5.0/ref/models/database-functions/#lower)).

### Modello Book

Copia il modello `Book` sotto e incollalo di nuovo nella parte inferiore del tuo file. Il modello `Book` rappresenta tutte le informazioni su un libro disponibile in senso generale, ma non un particolare "istanza" fisica o "copia" disponibile per il prestito.

Il modello utilizza un `CharField` per rappresentare il `title` e l'`isbn` del libro. Per `isbn`, nota come il primo parametro senza nome imposta esplicitamente l'etichetta come "ISBN" (altrimenti sarebbe predefinito a "Isbn"). Abbiamo anche impostato il parametro `unique` come `true` per garantire che tutti i libri abbiano un ISBN univoco (il parametro unico rende il valore del campo globalmente unico in una tabella). A differenza dell'`isbn` (e del nome del genere), il `title` non è impostato per essere unico, perché è possibile che diversi libri abbiano lo stesso nome. Il modello utilizza `TextField` per il `summary`, poiché questo testo potrebbe dover essere piuttosto lungo.

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

In entrambi i tipi di campo, la classe del modello correlato è dichiarata come il primo parametro senza nome utilizzando o la classe del modello o una stringa contenente il nome del modello correlato. Devi utilizzare il nome del modello come stringa se la classe associata non è ancora stata definita in questo file prima che venga referenziata! Gli altri parametri di interesse nel campo `author` sono `null=True`, che consente al database di memorizzare un valore `Null` se non viene selezionato alcun autore, e `on_delete=models.RESTRICT`, che impedirà che l'autore associato al libro venga eliminato se è referenziato da qualsiasi libro.

> [!WARNING]
> Per impostazione predefinita `on_delete=models.CASCADE`, il che significa che se l'autore venisse cancellato, anche questo libro verrebbe cancellato! Usiamo `RESTRICT` qui, ma potremmo anche utilizzare `PROTECT` per impedire che l'autore venga eliminato mentre un qualsiasi libro lo utilizza o `SET_NULL` per impostare l'autore del libro su `Null` se il record viene eliminato.

Il modello definisce anche `__str__()`, utilizzando il campo `title` del libro per rappresentare un record `Book`. Il metodo finale, `get_absolute_url()` restituisce un URL che può essere utilizzato per accedere a un record di dettaglio per questo modello (dovremo definire un mapping di URL che abbia il nome `book-detail`, e definire una vista e un modello associati).

### Modello BookInstance

Successivamente, copia il modello `BookInstance` (mostrato di seguito) sotto gli altri modelli. Il `BookInstance` rappresenta una copia specifica di un libro che qualcuno potrebbe prendere in prestito e include informazioni su se la copia è disponibile o in quale data è prevista la restituzione, dettagli sull'"impronta" o versione e un ID unico per il libro nella biblioteca.

Alcuni dei campi e dei metodi saranno ora familiari. Il modello utilizza:

- `ForeignKey` per identificare il `Book` associato (ogni libro può avere molte copie, ma una copia può avere solo un `Book`). La chiave specifica `on_delete=models.RESTRICT` per garantire che il `Book` non possa essere cancellato mentre è referenziato da un `BookInstance`.
- `CharField` per rappresentare l'impronta (rilascio specifico) del libro.

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

Dichiariamo anche alcuni nuovi tipi di campo:

- `UUIDField` viene utilizzato per il campo `id` per impostarlo come chiave primaria per questo modello. Questo tipo di campo alloca un valore univoco globale per ciascuna istanza (uno per ogni libro che puoi trovare nella biblioteca).
- `DateField` viene utilizzato per la data di `due_back` (nella quale si prevede che il libro diventi disponibile dopo essere stato preso in prestito o in manutenzione). Questo valore può essere `blank` o `null` (necessario per quando il libro è disponibile). I metadati del modello (`Class Meta`) usano questo campo per ordinare i record quando vengono restituiti in una query.
- `status` è un `CharField` che definisce una lista di scelta/selezione. Come puoi vedere, definiamo una tuple che contiene tuple di coppie chiave-valore e la passiamo all'argomento choices. Il valore in una coppia chiave/valore è un valore di visualizzazione che un utente può selezionare, mentre i tasti sono i valori che vengono effettivamente salvati se viene selezionata l'opzione. Abbiamo anche impostato un valore predefinito di 'm' (manutenzione) poiché i libri saranno creati inizialmente non disponibili prima di essere messi sugli scaffali.

Il metodo `__str__()` rappresenta l'oggetto `BookInstance` utilizzando una combinazione del suo ID unico e del titolo del `Book` associato.

> [!NOTE]
> Un po' di Python:
>
> - A partire da Python 3.6, puoi usare la sintassi di interpolazione delle stringhe (nota anche come f-strings): `f'{self.id} ({self.book.title})'`.
> - Nelle versioni precedenti di questo tutorial, stavamo usando una sintassi [stringa formattata](https://peps.python.org/pep-3101/), che è anche un modo valido di formattare le stringhe in Python (ad es., `'{0} ({1})'.format(self.id,self.book.title)`).

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

Tutti i campi/metodi dovrebbero ora essere familiari. Il modello definisce un autore come avente un nome, cognome e date di nascita e morte (entrambi opzionali). Specifica che per impostazione predefinita `__str__()` restituisce il nome in ordine _cognome_, _nome_. Il metodo `get_absolute_url()` inverte il mapping URL `author-detail` per ottenere l'URL per visualizzare un autore individuale.

## Esegui nuovamente le migrazioni del database

Tutti i tuoi modelli sono stati ora creati. Ora esegui nuovamente le migrazioni del database per aggiungerli al tuo database.

```bash
python3 manage.py makemigrations
python3 manage.py migrate
```

## Modello Language — sfida

Immagina che un benefattore locale doni un numero di nuovi libri scritti in un'altra lingua (diciamo, farsi). La sfida è capire come questi sarebbero meglio rappresentati nel nostro sito web della biblioteca e poi aggiungerli ai modelli.

Alcune cose da considerare:

- La "lingua" dovrebbe essere associata a un `Book`, `BookInstance`, o qualche altro oggetto?
- Le diverse lingue dovrebbero essere rappresentate utilizzando un modello, un campo testo libero, o un elenco a discesa selezione hard-coded?

Dopo aver deciso, aggiungi il campo. Puoi vedere cosa abbiamo deciso [per il nostro progetto su GitHub](https://github.com/mdn/django-locallibrary-tutorial/blob/main/catalog/models.py).

Non dimenticare che dopo una modifica al tuo modello, dovresti di nuovo eseguire le migrazioni del database per aggiungere le modifiche.

```bash
python3 manage.py makemigrations
python3 manage.py migrate
```

## Riassunto

In questo articolo abbiamo imparato come sono definiti i modelli, e poi usato queste informazioni per progettare e implementare modelli appropriati per il sito web _LocalLibrary_.

A questo punto ci allontaneremo brevemente dalla creazione del sito e daremo un’occhiata al sito di _amministrazione di Django_. Questo sito ci permetterà di aggiungere alcuni dati alla biblioteca, che potremo poi mostrare usando le nostre (ancora da creare) viste e modelli.

## Vedi anche

- [Scrivere la tua prima app Django, parte 2](https://docs.djangoproject.com/en/5.0/intro/tutorial02/) (Documentazione di Django)
- [Esecuzione delle query](https://docs.djangoproject.com/en/5.0/topics/db/queries/) (Documentazione di Django)
- [QuerySet API Reference](https://docs.djangoproject.com/en/5.0/ref/models/querysets/) (Documentazione di Django)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/skeleton_website", "Learn_web_development/Extensions/Server-side/Django/Admin_site", "Learn_web_development/Extensions/Server-side/Django")}}
