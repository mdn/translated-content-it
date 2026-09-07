---
title: "Tutorial Django - Parte 4: sito di amministrazione Django"
short-title: "4: sito di amministrazione Django"
slug: Learn_web_development/Extensions/Server-side/Django/Admin_site
l10n:
  sourceCommit: 815f1a18f44059500b337719295c6eda14b6228e
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Models", "Learn_web_development/Extensions/Server-side/Django/Home_page", "Learn_web_development/Extensions/Server-side/Django")}}

Ora che sono stati creati i modelli per il sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website), verrà utilizzato il sito di amministrazione Django per aggiungere alcuni dati di libri "reali". Per prima cosa verrà illustrato come registrare i modelli nel sito di amministrazione, quindi come effettuare l'accesso e creare alcuni dati. Alla fine dell'articolo verranno mostrati alcuni modi per migliorare ulteriormente la presentazione del sito di amministrazione.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Completare prima: <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/Models"
          >Tutorial Django - Parte 3: utilizzo dei modelli</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere i vantaggi e i limiti del sito di amministrazione Django e utilizzarlo per creare alcuni record per i modelli.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

L'_applicazione_ di amministrazione Django può usare i modelli per costruire automaticamente un'area del sito utilizzabile per creare, visualizzare, aggiornare ed eliminare record. Questo può far risparmiare molto tempo durante lo sviluppo, rendendo molto semplice testare i modelli e verificare se i dati sono quelli _corretti_. L'applicazione di amministrazione può essere utile anche per gestire i dati in produzione, a seconda del tipo di sito web. Il progetto Django la raccomanda solo per la gestione interna dei dati (ovvero, solo per l'uso da parte degli amministratori o delle persone interne all'organizzazione), poiché l'approccio incentrato sul modello non è necessariamente la migliore interfaccia possibile per tutti gli utenti ed espone molti dettagli non necessari sui modelli.

Tutta la configurazione necessaria per includere l'applicazione di amministrazione nel sito web è stata eseguita automaticamente durante la [creazione del progetto scheletro](/it/docs/Learn_web_development/Extensions/Server-side/Django/skeleton_website) (per informazioni sulle dipendenze effettivamente necessarie, vedere la [documentazione Django qui](https://docs.djangoproject.com/en/5.0/ref/contrib/admin/)). Di conseguenza, per aggiungere i modelli all'applicazione di amministrazione è **necessario** soltanto _registrarli_. Alla fine di questo articolo verrà fornita una breve dimostrazione di come configurare ulteriormente l'area di amministrazione per visualizzare meglio i dati dei modelli.

Dopo aver registrato i modelli verrà mostrato come creare un nuovo "superuser", effettuare l'accesso al sito e creare alcuni libri, autori, istanze di libri e generi. Questi saranno utili per testare le viste e i template che verranno creati nel prossimo tutorial.

## Registrazione dei modelli

Per prima cosa, aprire **admin.py** nell'applicazione catalog (**/django-locallibrary-tutorial/catalog/admin.py**). Attualmente ha il seguente aspetto: notare che importa già `django.contrib.admin`:

```python
from django.contrib import admin

# Register your models here.
```

Registrare i modelli copiando il testo seguente in fondo al file. Questo codice importa i modelli e quindi chiama `admin.site.register` per registrare ciascuno di essi.

```python
from .models import Author, Genre, Book, BookInstance, Language

admin.site.register(Book)
admin.site.register(Author)
admin.site.register(Genre)
admin.site.register(BookInstance)
admin.site.register(Language)
```

> [!NOTE]
> Le righe precedenti presuppongono che sia stata accettata la sfida di creare un modello per rappresentare il linguaggio naturale di un libro ([vedere l'articolo del tutorial sui modelli](/it/docs/Learn_web_development/Extensions/Server-side/Django/Models))!

Questo è il modo più semplice per registrare un modello, o dei modelli, nel sito. Il sito di amministrazione è altamente personalizzabile e le altre modalità di registrazione dei modelli saranno illustrate più avanti.

## Creazione di un superuser

Per effettuare l'accesso al sito di amministrazione, è necessario un account utente con lo stato _Staff_ abilitato. Per visualizzare e creare record, è inoltre necessario che questo utente disponga delle autorizzazioni per gestire tutti gli oggetti. È possibile creare un account "superuser" che dispone di accesso completo al sito e di tutte le autorizzazioni necessarie usando **manage.py**.

Eseguire il comando seguente, nella stessa directory di **manage.py**, per creare il superuser. Verrà richiesto di inserire un nome utente, un indirizzo email e una password _robusta_.

```bash
python3 manage.py createsuperuser
```

Al termine di questo comando, un nuovo superuser sarà stato aggiunto al database. Riavviare ora il server di sviluppo per testare l'accesso:

```bash
python3 manage.py runserver
```

## Accesso e utilizzo del sito

Per effettuare l'accesso al sito, aprire l'URL _/admin_ (ad esempio, `http://127.0.0.1:8000/admin`) e inserire le nuove credenziali userid e password del superuser (si verrà reindirizzati alla pagina di _login_, quindi nuovamente all'URL _/admin_ dopo aver inserito i propri dati).

Questa parte del sito mostra tutti i modelli, raggruppati per applicazione installata. È possibile fare clic sul nome di un modello per accedere a una schermata che elenca tutti i record associati e fare ulteriormente clic su tali record per modificarli. È anche possibile fare direttamente clic sul collegamento **Add** accanto a ciascun modello per iniziare a creare un record di quel tipo.

![Sito di amministrazione - Pagina principale](admin_home.png)

Fare clic sul collegamento **Add** a destra di _Books_ per creare un nuovo libro (verrà visualizzata una finestra di dialogo simile a quella seguente). Notare come i titoli di ciascun campo, il tipo di widget usato e il `help_text` (se presente) corrispondano ai valori specificati nel modello.

Inserire i valori per i campi. È possibile creare nuovi autori o generi premendo il pulsante **+** accanto ai rispettivi campi (oppure selezionare valori esistenti dagli elenchi, se sono già stati creati). Al termine, è possibile premere **SAVE**, **Save and add another** o **Save and continue editing** per salvare il record.

![Sito di amministrazione - Aggiunta libro](admin_book_add.png)

> [!NOTE]
> A questo punto, dedicare del tempo all'aggiunta di alcuni libri, autori, lingue e generi (ad esempio, Fantasy) all'applicazione. Assicurarsi che ciascun autore e genere includa un paio di libri diversi: questo renderà più interessanti le viste elenco e dettaglio quando verranno implementate più avanti nella serie di articoli.

Dopo aver finito di aggiungere libri, fare clic sul collegamento **Home** nel segnalibro superiore per tornare alla pagina principale di amministrazione. Quindi fare clic sul collegamento **Books** per visualizzare l'elenco corrente dei libri, oppure su uno degli altri collegamenti per vedere gli altri elenchi di modelli. Ora che sono stati aggiunti alcuni libri, l'elenco potrebbe essere simile allo screenshot seguente. Viene visualizzato il titolo di ogni libro; questo è il valore restituito nel metodo `__str__()` del modello Book specificato nell'articolo precedente.

![Sito di amministrazione - Elenco di oggetti libro](admin_book_list.png)

Da questo elenco è possibile eliminare libri selezionando la casella di controllo accanto al libro indesiderato, selezionando l'azione _delete…_ dall'elenco a discesa _Action_ e quindi premendo il pulsante **Go**. È inoltre possibile aggiungere nuovi libri premendo il pulsante **ADD BOOK**.

È possibile modificare un libro selezionandone il nome nel collegamento. La pagina di modifica di un libro, mostrata di seguito, è quasi identica alla pagina "Add". Le principali differenze sono il titolo della pagina (_Change book_) e l'aggiunta dei pulsanti **Delete**, **HISTORY** e **VIEW ON SITE** (quest'ultimo pulsante viene visualizzato perché è stato definito il metodo `get_absolute_url()` nel modello).

> [!NOTE]
> Facendo clic sul pulsante **VIEW ON SITE** viene generata un'eccezione `NoReverseMatch`, poiché il metodo `get_absolute_url()` tenta di eseguire `reverse()` di una mappatura URL con nome ('book-detail') che non è stata ancora definita.
> Una mappatura URL e la vista associata verranno definite in [Tutorial Django - Parte 6: viste generiche elenco e dettaglio](/it/docs/Learn_web_development/Extensions/Server-side/Django/Generic_views).

![Sito di amministrazione - Modifica libro](admin_book_modify.png)

Ora tornare alla pagina **Home** usando il collegamento _Home_ nel percorso di navigazione e quindi visualizzare gli elenchi **Author** e **Genre**. Dovrebbero già essercene parecchi, creati durante l'aggiunta dei nuovi libri, ma è possibile aggiungerne altri.

Non saranno invece presenti _Book Instances_, poiché queste non vengono create a partire dai Books (anche se è possibile creare un `Book` da un `BookInstance`: questa è la natura del campo `ForeignKey`). Tornare alla pagina _Home_ e premere il pulsante **Add** associato per visualizzare la schermata _Add book instance_ seguente. Notare il grande Id univoco globale, che può essere usato per identificare separatamente una singola copia di un libro nella biblioteca.

![Sito di amministrazione - Aggiunta BookInstance](admin_bookinstance_add.png)

Creare alcuni di questi record per ciascuno dei libri. Impostare lo stato su _Available_ per almeno alcuni record e su _On loan_ per altri. Se lo stato **non** è _Available_, impostare anche una data futura per _Due back_.

È tutto! Ora è stato appreso come configurare e usare il sito di amministrazione. Sono stati inoltre creati record per `Book`, `BookInstance`, `Genre`, `Language` e `Author`, che potranno essere usati una volta create le viste e i template personalizzati.

## Configurazione avanzata

Django svolge un ottimo lavoro nella creazione di un sito di amministrazione di base usando le informazioni dei modelli registrati:

- Ogni modello dispone di un elenco di singoli record, identificati dalla stringa creata con il metodo `__str__()` del modello e collegati a viste/moduli di dettaglio per la modifica. Per impostazione predefinita, questa vista contiene nella parte superiore un menu delle azioni utilizzabile per eseguire operazioni di eliminazione collettiva sui record.
- I moduli dei record di dettaglio del modello per modificare e aggiungere record contengono tutti i campi del modello, disposti verticalmente nell'ordine della loro dichiarazione.

È possibile personalizzare ulteriormente l'interfaccia per renderla ancora più semplice da usare. Alcune delle operazioni possibili sono:

- Viste elenco:
  - Aggiungere ulteriori campi/informazioni visualizzati per ogni record.
  - Aggiungere filtri per selezionare quali record vengono elencati, in base alla data o a un altro valore di selezione, ad esempio lo stato del prestito di un libro.
  - Aggiungere ulteriori opzioni al menu delle azioni nelle viste elenco e scegliere dove visualizzare questo menu nel modulo.

- Viste dettaglio:
  - Scegliere quali campi visualizzare, o escludere, insieme al loro ordine, raggruppamento, possibilità di modifica, widget usato, orientamento e così via.
  - Aggiungere campi correlati a un record per consentire la modifica inline, ad esempio aggiungere la possibilità di aggiungere e modificare record di libri durante la creazione del record del relativo autore.

In questa sezione verranno esaminate alcune modifiche che miglioreranno l'interfaccia per _LocalLibrary_, incluso l'inserimento di ulteriori informazioni negli elenchi dei modelli `Book` e `Author` e il miglioramento del layout delle rispettive viste di modifica. La presentazione dei modelli `Language` e `Genre` non verrà modificata perché hanno un solo campo ciascuno, quindi non vi è alcun reale vantaggio nel farlo.

È possibile trovare un riferimento completo di tutte le opzioni di personalizzazione del sito di amministrazione in [Il sito di amministrazione Django](https://docs.djangoproject.com/en/5.0/ref/contrib/admin/) (Documentazione Django).

### Registrare una classe ModelAdmin

Per modificare il modo in cui un modello viene visualizzato nell'interfaccia di amministrazione, definire una classe [ModelAdmin](https://docs.djangoproject.com/en/5.0/ref/contrib/admin/#modeladmin-objects), che descrive il layout, e registrarla con il modello.

Iniziare con il modello `Author`. Aprire **admin.py** nell'applicazione catalog (**/django-locallibrary-tutorial/catalog/admin.py**). Commentare la registrazione originale, anteponendo un #, per il modello `Author`:

```python
# admin.site.register(Author)
```

Ora aggiungere un nuovo `AuthorAdmin` e la registrazione, come mostrato di seguito.

```python
# Define the admin class
class AuthorAdmin(admin.ModelAdmin):
    pass

# Register the admin class with the associated model
admin.site.register(Author, AuthorAdmin)
```

Ora verranno aggiunte classi `ModelAdmin` per `Book` e `BookInstance`. Occorre nuovamente commentare le registrazioni originali:

```python
# admin.site.register(Book)
# admin.site.register(BookInstance)
```

Ora creare e registrare i nuovi modelli. Ai fini di questa dimostrazione, verrà invece usato il decorator `@register` per registrare i modelli; questo esegue esattamente la stessa operazione della sintassi `admin.site.register()`:

```python
# Register the Admin classes for Book using the decorator
@admin.register(Book)
class BookAdmin(admin.ModelAdmin):
    pass

# Register the Admin classes for BookInstance using the decorator
@admin.register(BookInstance)
class BookInstanceAdmin(admin.ModelAdmin):
    pass
```

Attualmente tutte le classi di amministrazione sono vuote, come indicato da `pass`, quindi il comportamento dell'amministrazione non verrà modificato. È ora possibile estenderle per definire il comportamento di amministrazione specifico per i modelli.

### Configurare le viste elenco

_LocalLibrary_ al momento elenca tutti gli autori usando il nome dell'oggetto generato dal metodo `__str__()` del modello. Questo va bene quando sono presenti solo pochi autori, ma con molti autori potrebbero esserci duplicati. Per distinguerli, o semplicemente perché si desidera mostrare informazioni più interessanti su ciascun autore, è possibile usare [list_display](https://docs.djangoproject.com/en/5.0/ref/contrib/admin/#django.contrib.admin.ModelAdmin.list_display) per aggiungere ulteriori campi alla vista.

Sostituire la classe `AuthorAdmin` con il codice seguente. I nomi dei campi da visualizzare nell'elenco sono dichiarati in una _tuple_ nell'ordine richiesto, come mostrato; sono gli stessi nomi specificati nel modello originale.

```python
class AuthorAdmin(admin.ModelAdmin):
    list_display = ('last_name', 'first_name', 'date_of_birth', 'date_of_death')
```

Ora passare all'elenco degli autori nel sito web. I campi precedenti dovrebbero essere ora visualizzati come segue:

![Sito di amministrazione - Elenco autori migliorato](admin_improved_author_list.png)

Per il modello `Book` verranno inoltre visualizzati `author` e `genre`. `author` è una relazione di campo `ForeignKey` (uno-a-molti), quindi sarà rappresentato dal valore `__str__()` del record associato. Sostituire la classe `BookAdmin` con la versione seguente.

```python
class BookAdmin(admin.ModelAdmin):
    list_display = ('title', 'author', 'display_genre')
```

Purtroppo non è possibile specificare direttamente il campo `genre` in `list_display`, poiché è un `ManyToManyField` (Django lo impedisce perché comporterebbe un elevato "costo" di accesso al database). Verrà invece definita una funzione `display_genre` per ottenere le informazioni come stringa; è la funzione chiamata in precedenza e verrà definita qui sotto.

> [!NOTE]
> Ottenere `genre` potrebbe non essere una buona idea in questo caso, a causa del "costo" dell'operazione sul database. Viene mostrato questo approccio perché chiamare funzioni nei modelli può essere molto utile per altri motivi, ad esempio per aggiungere un collegamento _Delete_ accanto a ogni elemento dell'elenco.

Aggiungere il codice seguente al modello `Book` (**models.py**). Questo crea una stringa dai primi tre valori del campo `genre`, se esistono, e crea una `short_description` che può essere usata nel sito di amministrazione per questo metodo.

```python
def display_genre(self):
    """Create a string for the Genre. This is required to display genre in Admin."""
    return ', '.join(genre.name for genre in self.genre.all()[:3])

display_genre.short_description = 'Genre'
```

Dopo aver salvato il modello e l'amministrazione aggiornata, aprire il sito web e andare alla pagina dell'elenco _Books_; dovrebbe essere visualizzato un elenco di libri simile a quello seguente:

![Sito di amministrazione - Elenco libri migliorato](admin_improved_book_list.png)

Il modello `Genre`, e il modello `Language` se ne è stato definito uno, hanno entrambi un solo campo, quindi non ha senso creare un modello aggiuntivo per visualizzare ulteriori campi.

> [!NOTE]
> Vale la pena aggiornare l'elenco del modello `BookInstance` affinché mostri almeno lo stato e la data di restituzione prevista. Questa attività è stata aggiunta come sfida alla fine dell'articolo.

### Aggiungere filtri agli elenchi

Quando un elenco contiene molti elementi, può essere utile poter filtrare gli elementi visualizzati.
Questo viene fatto elencando i campi nell'attributo `list_filter`.
Sostituire la classe `BookInstanceAdmin` corrente con il frammento di codice seguente.

```python
class BookInstanceAdmin(admin.ModelAdmin):
    list_filter = ('status', 'due_back')
```

La vista elenco includerà ora una casella di filtro a destra. Notare come sia possibile scegliere date e stato per filtrare i valori:

![Sito di amministrazione - Filtri elenco BookInstance](admin_improved_bookinstance_list_filters.png)

### Organizzare il layout della vista dettaglio

Per impostazione predefinita, le viste dettaglio dispongono tutti i campi verticalmente, nell'ordine della loro dichiarazione nel modello. È possibile modificare l'ordine di dichiarazione, i campi visualizzati o esclusi, l'uso di sezioni per organizzare le informazioni, la visualizzazione orizzontale o verticale dei campi e persino i widget di modifica usati nei moduli di amministrazione.

> [!NOTE]
> I modelli di _LocalLibrary_ sono relativamente semplici, quindi non vi è una grande necessità di modificare il layout; verranno comunque apportate alcune modifiche, solo per mostrare come fare.

#### Controllare quali campi vengono visualizzati e il loro layout

Aggiornare la classe `AuthorAdmin` aggiungendo la riga `fields`, come mostrato di seguito:

```python
class AuthorAdmin(admin.ModelAdmin):
    list_display = ('last_name', 'first_name', 'date_of_birth', 'date_of_death')

    fields = ['first_name', 'last_name', ('date_of_birth', 'date_of_death')]
```

L'attributo `fields` elenca soltanto i campi da visualizzare nel modulo, nell'ordine indicato. Per impostazione predefinita i campi vengono visualizzati verticalmente, ma saranno visualizzati orizzontalmente se vengono ulteriormente raggruppati in una tuple, come mostrato per i campi "date" precedenti.

Nel sito web, passare alla vista dettaglio dell'autore: dovrebbe ora apparire come mostrato di seguito:

![Sito di amministrazione - Dettaglio autore migliorato](admin_improved_author_detail.png)

> [!NOTE]
> È possibile usare anche l'attributo `exclude` per dichiarare un elenco di attributi da escludere dal modulo; verranno visualizzati tutti gli altri attributi del modello.

#### Suddividere la vista dettaglio in sezioni

È possibile aggiungere "sezioni" per raggruppare informazioni correlate del modello nel modulo di dettaglio, usando l'attributo [fieldsets](https://docs.djangoproject.com/en/5.0/ref/contrib/admin/#django.contrib.admin.ModelAdmin.fieldsets).

Nel modello `BookInstance` sono presenti informazioni su quale sia il libro, ovvero `name`, `imprint` e `id`, e su quando sarà disponibile, ovvero `status` e `due_back`. È possibile aggiungerle alla classe `BookInstanceAdmin` come mostrato di seguito, usando la proprietà `fieldsets`.

```python
@admin.register(BookInstance)
class BookInstanceAdmin(admin.ModelAdmin):
    list_filter = ('status', 'due_back')

    fieldsets = (
        (None, {
            'fields': ('book', 'imprint', 'id')
        }),
        ('Availability', {
            'fields': ('status', 'due_back')
        }),
    )
```

Ogni sezione ha il proprio titolo, oppure `None` se non si desidera un titolo, e una tuple associata di campi in un dizionario. Il formato è complesso da descrivere, ma abbastanza facile da comprendere osservando il frammento di codice immediatamente precedente.

Ora passare a una vista di un'istanza di libro nel sito web; il modulo dovrebbe apparire come mostrato di seguito:

![Sito di amministrazione - Dettaglio BookInstance migliorato con sezioni](admin_improved_bookinstance_detail_sections.png)

### Modifica inline dei record associati

A volte può essere opportuno poter aggiungere record associati contemporaneamente. Ad esempio, può avere senso avere sia le informazioni sul libro sia le informazioni sulle copie specifiche disponibili nella stessa pagina di dettaglio.

Questo è possibile dichiarando [inlines](https://docs.djangoproject.com/en/5.0/ref/contrib/admin/#django.contrib.admin.ModelAdmin.inlines), di tipo [TabularInline](https://docs.djangoproject.com/en/5.0/ref/contrib/admin/#django.contrib.admin.TabularInline) per un layout orizzontale oppure [StackedInline](https://docs.djangoproject.com/en/5.0/ref/contrib/admin/#django.contrib.admin.StackedInline) per un layout verticale, proprio come il layout predefinito del modello. È possibile aggiungere inline le informazioni `BookInstance` al dettaglio di `Book` specificando `inlines` in `BookAdmin`:

```python
class BooksInstanceInline(admin.TabularInline):
    model = BookInstance

@admin.register(Book)
class BookAdmin(admin.ModelAdmin):
    list_display = ('title', 'author', 'display_genre')

    inlines = [BooksInstanceInline]
```

Ora passare a una vista di un `Book` nel sito web: nella parte inferiore dovrebbero ora essere visibili le istanze del libro relative a questo libro, immediatamente sotto i campi del genere del libro.

![Sito di amministrazione - Libro con elementi inline](admin_improved_book_detail_inlines.png)

In questo caso è stata soltanto dichiarata la classe inline tabulare, che aggiunge tutti i campi del modello _inline_. È possibile specificare molte altre informazioni per il layout, inclusi i campi da visualizzare, il loro ordine, se sono di sola lettura o meno e così via. Per ulteriori informazioni, vedere [TabularInline](https://docs.djangoproject.com/en/5.0/ref/contrib/admin/#django.contrib.admin.TabularInline).

> [!NOTE]
> Questa funzionalità presenta alcuni limiti problematici. Nello screenshot precedente sono presenti tre istanze di libro esistenti, seguite da tre segnaposto per nuove istanze di libro, che sembrano molto simili. Sarebbe preferibile non avere istanze di libro inutilizzate per impostazione predefinita e aggiungerle soltanto con il collegamento **Add another Book instance**, oppure poter semplicemente elencare i `BookInstance` come collegamenti non modificabili da qui. La prima opzione può essere ottenuta impostando l'attributo `extra` su `0` nel modello `BooksInstanceInline`; provare autonomamente.

## Mettiti alla prova

In questa sezione sono stati appresi molti concetti, quindi è il momento di provare alcune attività.

1. Per la vista elenco `BookInstance`, aggiungere codice per visualizzare il libro, lo stato, la data di restituzione prevista e l'id, anziché il testo `__str__()` predefinito.
2. Aggiungere un elenco inline di elementi `Book` alla vista dettaglio `Author` usando lo stesso approccio usato per `Book`/`BookInstance`.

## Riepilogo

È tutto! Ora è stato appreso come configurare il sito di amministrazione sia nella sua forma più semplice sia in quella migliorata, come creare un superuser e come navigare nel sito di amministrazione per visualizzare, eliminare e aggiornare record. Nel frattempo sono stati creati molti Books, BookInstances, Genres e Authors che potranno essere elencati e visualizzati una volta create le viste e i template personalizzati.

## Ulteriori letture

- [Scrivere la prima app Django, parte 2: introduzione all'amministrazione Django](https://docs.djangoproject.com/en/5.0/intro/tutorial02/#introducing-the-django-admin) (Documentazione Django)
- [Il sito di amministrazione Django](https://docs.djangoproject.com/en/5.0/ref/contrib/admin/) (Documentazione Django)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Models", "Learn_web_development/Extensions/Server-side/Django/Home_page", "Learn_web_development/Extensions/Server-side/Django")}}
