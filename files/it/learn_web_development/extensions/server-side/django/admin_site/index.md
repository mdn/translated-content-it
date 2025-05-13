---
title: "Guida a Django Parte 4: Sito di amministrazione Django"
short-title: "4: Sito di amministrazione Django"
slug: Learn_web_development/Extensions/Server-side/Django/Admin_site
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Models", "Learn_web_development/Extensions/Server-side/Django/Home_page", "Learn_web_development/Extensions/Server-side/Django")}}

Ora che abbiamo creato i modelli per il sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website), useremo il sito di amministrazione Django per aggiungere alcuni dati "reali" sui libri. Prima le mostreremo come registrare i modelli con il sito di amministrazione, poi le mostreremo come effettuare il login e creare alcuni dati. Alla fine dell'articolo mostreremo alcuni dei modi in cui può migliorare ulteriormente la presentazione del sito di amministrazione.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Completare prima: <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/Models"
          >Guida a Django Parte 3: Utilizzo dei modelli</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere i vantaggi e le limitazioni del sito di amministrazione Django e usarlo per creare alcuni record per i nostri modelli.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

L'_applicazione_ di amministrazione Django può utilizzare i suoi modelli per costruire automaticamente un'area del sito che può utilizzare per creare, visualizzare, aggiornare e eliminare i record. Questo può farle risparmiare molto tempo durante lo sviluppo, rendendo molto facile testare i suoi modelli e verificare se dispone dei dati _corretti_. L'applicazione di amministrazione può anche essere utile per gestire i dati in produzione, a seconda del tipo di sito web. Il progetto Django lo raccomanda solo per la gestione interna dei dati (cioè, solo per l'uso da parte degli amministratori, o persone interne alla sua organizzazione), poiché l'approccio centrato sul modello non è necessariamente la migliore interfaccia possibile per tutti gli utenti, ed espone molti dettagli non necessari sui modelli.

Tutta la configurazione necessaria per includere l'applicazione di amministrazione nel suo sito web è stata eseguita automaticamente quando ha [creato il progetto scheletro](/it/docs/Learn_web_development/Extensions/Server-side/Django/skeleton_website) (per informazioni sulle dipendenze effettivamente richieste, vedere i [documenti Django qui](https://docs.djangoproject.com/en/5.0/ref/contrib/admin/)). Di conseguenza, tutto ciò che deve fare per aggiungere i suoi modelli all'applicazione di amministrazione è _registrarli_. Alla fine di questo articolo forniremo una breve dimostrazione di come potrebbe ulteriormente configurare l'area di amministrazione per visualizzare meglio i dati del nostro modello.

Dopo aver registrato i modelli, mostreremo come creare un nuovo "superuser", accedere al sito e creare alcuni libri, autori, istanze di libri e generi. Questi saranno utili per testare le viste e i modelli che inizieremo a creare nel prossimo tutorial.

## Registrazione dei modelli

Per prima cosa apra **admin.py** nell'applicazione catalogo (**/django-locallibrary-tutorial/catalog/admin.py**). Attualmente appare così — noti che importa già `django.contrib.admin`:

```python
from django.contrib import admin

# Register your models here.
```

Registri i modelli copiando il seguente testo nella parte inferiore del file. Questo codice importa i modelli e poi chiama `admin.site.register` per registrare ciascuno di essi.

```python
from .models import Author, Genre, Book, BookInstance, Language

admin.site.register(Book)
admin.site.register(Author)
admin.site.register(Genre)
admin.site.register(BookInstance)
admin.site.register(Language)
```

> [!NOTE]
> Le righe sopra presumono che lei abbia accettato la sfida di creare un modello per rappresentare la lingua naturale di un libro ([vedi l'articolo del tutorial sui modelli](/it/docs/Learn_web_development/Extensions/Server-side/Django/Models))!

Questo è il modo più semplice per registrare un modello, o modelli, con il sito. Il sito di amministrazione è altamente personalizzabile, e parleremo più in dettaglio dei modi alternativi di registrare i suoi modelli più in basso.

## Creare un superuser

Per accedere al sito di amministrazione, abbiamo bisogno di un account utente con lo stato _Staff_ abilitato. Per visualizzare e creare record abbiamo anche bisogno che questo utente abbia i permessi per gestire tutti i nostri oggetti. Può creare un account "superuser" che ha pieno accesso al sito e tutte le autorizzazioni necessarie usando **manage.py**.

Chiami il seguente comando, nella stessa directory di **manage.py**, per creare il superuser. Le verrà richiesto di inserire un nome utente, un indirizzo email e una password _forte_.

```bash
python3 manage.py createsuperuser
```

Una volta completato questo comando, un nuovo superuser sarà stato aggiunto al database. Ora riavvii il server di sviluppo per poter testare il login:

```bash
python3 manage.py runserver
```

## Accedere e utilizzare il sito

Per accedere al sito, apra l'URL _/admin_ (per esempio, `http://127.0.0.1:8000/admin`) e inserisca il suo nuovo userid e password di superuser (sarà reindirizzato alla pagina di _login_, e poi di nuovo all'URL _/admin_ dopo aver inserito i suoi dettagli).

Questa parte del sito visualizza tutti i nostri modelli, raggruppati per applicazione installata. Può cliccare su un nome di modello per andare a una schermata che elenca tutti i suoi record associati, e può cliccare ulteriormente su quei record per modificarli. Può anche cliccare direttamente sul link **Add** accanto a ciascun modello per iniziare a creare un record di quel tipo.

![Sito di amministrazione - Pagina iniziale](admin_home.png)

Fare clic sul collegamento **Add** a destra di _Books_ per creare un nuovo libro (questo mostrerà un dialogo simile a quello sotto). Noti come i titoli di ciascun campo, il tipo di widget utilizzato e il `help_text` (se presente) corrispondono ai valori specificati nel modello.

Inserisca i valori per i campi. Può creare nuovi autori o generi premendo il pulsante **+** accanto ai rispettivi campi (o selezionare valori esistenti dagli elenchi se li ha già creati). Quando ha finito può premere **SAVE**, **Save and add another**, o **Save and continue editing** per salvare il record.

![Sito di amministrazione - Aggiunta libro](admin_book_add.png)

> [!NOTE]
> A questo punto vorremmo che dedicasse un po' di tempo ad aggiungere alcuni libri, autori, lingue e generi (ad esempio, Fantasy) alla sua applicazione. Assicurati che ciascun autore e genere includa un paio di libri diversi (questo renderà le sue viste elenco e dettaglio più interessanti quando le implementeremo più avanti nella serie di articoli).

Quando ha finito di aggiungere libri, clicchi sul link **Home** nel segnalibro superiore per tornare alla pagina principale dell'amministrazione. Poi clicchi sul link **Books** per visualizzare l'elenco attuale dei libri (o su uno degli altri link per vedere altri elenchi di modelli). Ora che ha aggiunto alcuni libri, l'elenco potrebbe apparire simile a quello nello screenshot qui sotto. Il titolo di ciascun libro viene visualizzato; questo è il valore restituito nel metodo `__str__()` del modello Book che abbiamo specificato nell'ultimo articolo.

![Sito di amministrazione - Elenco oggetti libro](admin_book_list.png)

Da questo elenco può eliminare i libri selezionando la casella di controllo accanto al libro che non desidera, selezionando l'azione _delete…_ dal menu a tendina _Action_, e poi premendo il pulsante **Go**. Può anche aggiungere nuovi libri premendo il pulsante **ADD BOOK**.

Può modificare un libro selezionando il nome nel link. La pagina di modifica di un libro, mostrata di seguito, è quasi identica alla pagina "Aggiungi". Le principali differenze sono il titolo della pagina (_Change book_) e l'aggiunta di pulsanti **Delete**, **HISTORY** e **VIEW ON SITE** (quest'ultimo pulsante appare perché abbiamo definito il metodo `get_absolute_url()` nel nostro modello).

> [!NOTE]
> Fare clic sul pulsante **VIEW ON SITE** genera un'eccezione `NoReverseMatch` perché il metodo `get_absolute_url()` tenta di `reverse()` un mapping URL denominato ('book-detail') che non è ancora stato definito.
> Definiremo un mapping URL e la vista associata in [Guida a Django Parte 6: Viste elenco e dettaglio generiche](/it/docs/Learn_web_development/Extensions/Server-side/Django/Generic_views).

![Sito di amministrazione - Modifica libro](admin_book_modify.png)

Ora navighi nuovamente alla pagina **Home** (utilizzando il collegamento _Home_ nel breadcrumb) e poi visualizzi gli elenchi **Author** e **Genre** — dovrebbe già averne creati parecchi quando ha aggiunto i nuovi libri, ma sentiti libero di aggiungerne altri.

Quello che non avrà sono le _Istanze di libri_, perché queste non sono create dai Libri (sebbene lei possa creare un `Book` da un `BookInstance` — questa è la natura del campo `ForeignKey`). Torni alla pagina _Home_ e prema il pulsante **Add** associato per visualizzare la schermata _Add book instance_ qui sotto. Noti l'Id grande e univoco a livello globale, che può essere utilizzato per identificare separatamente una singola copia di un libro nella biblioteca.

![Sito di amministrazione - Aggiunta Istanza di libro](admin_bookinstance_add.png)

Crea un certo numero di questi record per ciascuno dei suoi libri. Imposti lo stato su _Available_ per almeno alcuni record e _On loan_ per altri. Se lo stato è **non** _Available_, allora imposti anche una data futura di _Due back_.

È tutto! Ha ora imparato come impostare e utilizzare il sito di amministrazione. Ha anche creato record per `Book`, `BookInstance`, `Genre`, `Language` e `Author` che potremo usare una volta create le nostre viste e modelli.

## Configurazione avanzata

Django fa un ottimo lavoro nel creare un sito di amministrazione di base utilizzando le informazioni dai modelli registrati:

- Ogni modello ha un elenco di record individuali, identificati dalla stringa creata con il metodo `__str__()` del modello, e collegati a viste dettagliate/moduli per la modifica. Di default, questa vista ha un menu di azioni in alto che può utilizzare per eseguire operazioni di eliminazione in blocco sui record.
- I moduli dei record dettagliati del modello per la modifica e l'aggiunta di record contengono tutti i campi nel modello, disposti verticalmente nell'ordine di dichiarazione.

Può personalizzare ulteriormente l'interfaccia per renderla ancora più facile da usare. Alcuni dei miglioramenti che può fare sono:

- Viste elenco:

  - Aggiungere campi/informazioni aggiuntivi visualizzati per ciascun record.
  - Aggiungere filtri per selezionare quali record sono elencati, basati su una data o su qualche altro criterio di selezione (ad esempio, stato di prestito del Book).
  - Aggiungere opzioni aggiuntive al menu delle azioni nelle viste elenco e scegliere dove questo menu sia visualizzato nel modulo.

- Viste dettagliate:

  - Scegliere quali campi visualizzare (o escludere), insieme al loro ordine, raggruppamento, se sono modificabili, il widget utilizzato, l'orientamento, ecc.
  - Aggiungere campi correlati a un record per consentire la modifica in linea (ad esempio, aggiungere la possibilità di aggiungere e modificare record di libri mentre si sta creando il record dell'autore).

In questa sezione esamineremo alcune modifiche che miglioreranno l'interfaccia per la nostra _LocalLibrary_, tra cui l'aggiunta di più informazioni agli elenchi dei modelli `Book` e `Author`, e migliorare il layout delle loro viste di modifica. Non cambieremo la presentazione dei modelli `Language` e `Genre` perché hanno solo un campo ciascuno, quindi non c'è un vero vantaggio nel farlo!

Può trovare un riferimento completo a tutte le opzioni di personalizzazione del sito di amministrazione in [Il sito di amministrazione Django](https://docs.djangoproject.com/en/5.0/ref/contrib/admin/) (Documenti Django).

### Registrare una classe ModelAdmin

Per cambiare il modo in cui un modello è visualizzato nell'interfaccia di amministrazione, si definisce una classe [ModelAdmin](https://docs.djangoproject.com/en/5.0/ref/contrib/admin/#modeladmin-objects) (che descrive il layout) e la si registra con il modello.

Iniziamo con il modello `Author`. Apri **admin.py** nell'applicazione catalogo (**/django-locallibrary-tutorial/catalog/admin.py**). Commenti la registrazione originale (lo prefissi con un #) per il modello `Author`:

```python
# admin.site.register(Author)
```

Ora aggiunga un nuovo `AuthorAdmin` e registrazione come mostrato sotto.

```python
# Define the admin class
class AuthorAdmin(admin.ModelAdmin):
    pass

# Register the admin class with the associated model
admin.site.register(Author, AuthorAdmin)
```

Ora aggiungeremo le classi `ModelAdmin` per `Book`, e `BookInstance`. Dobbiamo nuovamente commentare le registrazioni originali:

```python
# admin.site.register(Book)
# admin.site.register(BookInstance)
```

Ora per creare e registrare i nuovi modelli; per scopi di dimostrazione, useremo invece il decoratore `@register` per registrare i modelli (questo fa esattamente la stessa cosa della sintassi `admin.site.register()`):

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

Attualmente tutte le nostre classi di amministrazione sono vuote (vedi `pass`) quindi il comportamento dell'amministrazione rimarrà invariato! Possiamo ora estenderle per definire il nostro comportamento di amministrazione specifico del modello.

### Configurare le viste elenco

_La LocalLibrary_ attualmente elenca tutti gli autori utilizzando il nome dell'oggetto generato dal metodo `__str__()` del modello. Questo va bene quando si hanno solo pochi autori, ma una volta che si hanno molti autori, potrebbero finire col duplicarsi. Per differenziarli, o semplicemente perché si vuole mostrare informazioni più interessanti su ciascun autore, si può usare [list_display](https://docs.djangoproject.com/en/5.0/ref/contrib/admin/#django.contrib.admin.ModelAdmin.list_display) per aggiungere campi aggiuntivi alla vista.

Sostituite la vostra classe `AuthorAdmin` con il codice sotto. I nomi dei campi da visualizzare nell'elenco sono dichiarati in una _tupla_ nell'ordine richiesto, come mostrato (questi sono gli stessi nomi specificati nel suo modello originale).

```python
class AuthorAdmin(admin.ModelAdmin):
    list_display = ('last_name', 'first_name', 'date_of_birth', 'date_of_death')
```

Ora navighi all'elenco degli autori nel suo sito web. I campi di cui sopra dovrebbero ora essere visualizzati, in questo modo:

![Sito di amministrazione - Elenco autori migliorato](admin_improved_author_list.png)

Per il nostro modello `Book` visualizzeremo ulteriormente l'`author` e il `genre`. L`author` è un campo `ForeignKey` (relazione uno-a-molti), e quindi sarà rappresentato dal valore di `__str__()` per il record associato. Sostituite la classe `BookAdmin` con la versione qui sotto.

```python
class BookAdmin(admin.ModelAdmin):
    list_display = ('title', 'author', 'display_genre')
```

Purtroppo non possiamo specificare direttamente il campo `genre` in `list_display` perché è un `ManyToManyField` (Django lo impedisce perché ci sarebbe un grande "costo" di accesso al database nel farlo). Invece definiremo una funzione `display_genre` per ottenere le informazioni come stringa (questa è la funzione che abbiamo chiamato sopra; la definiremo qui sotto).

> [!NOTE]
> Ottenere il `genre` potrebbe non essere una buona idea qui, a causa del "costo" dell'operazione del database. Le mostriamo come fare perché chiamare funzioni nei suoi modelli può essere molto utile per altri motivi — per esempio per aggiungere un link _Delete_ accanto a ogni elemento nell'elenco.

Aggiungi il seguente codice nel suo modello `Book` (**models.py**). Questo crea una stringa dai primi tre valori del campo `genre` (se esistono) e crea una `short_description` che può essere usata nel sito di amministrazione per questo metodo.

```python
def display_genre(self):
    """Create a string for the Genre. This is required to display genre in Admin."""
    return ', '.join(genre.name for genre in self.genre.all()[:3])

display_genre.short_description = 'Genre'
```

Dopo aver salvato il modello e aggiornato l'amministrazione, apra il suo sito web e vada alla pagina di elenchi _Books_; dovrebbe vedere un elenco di libri come quello sotto:

![Sito di amministrazione - Elenco libri migliorato](admin_improved_book_list.png)

Il modello `Genre` (e il modello `Language`, se ne ha definito uno) hanno entrambi un solo campo, quindi non c'è nessun motivo per creare un ulteriore modello per visualizzare campi aggiuntivi.

> [!NOTE]
> Vale la pena aggiornare l'elenco del modello `BookInstance` per mostrare almeno lo stato e la data di ritorno prevista. Abbiamo aggiunto questo come sfida alla fine di questo articolo!

### Aggiungere filtri elenco

Una volta che si ha un gran numero di elementi in un elenco, può essere utile poter filtrare quali elementi sono visualizzati.
Questo viene fatto elencando i campi nell'attributo `list_filter`.
Sostituisca la sua attuale classe `BookInstanceAdmin` con il frammento di codice qui sotto.

```python
class BookInstanceAdmin(admin.ModelAdmin):
    list_filter = ('status', 'due_back')
```

La vista elenco ora includerà una casella di filtro a destra. Noti come può scegliere date e stati per filtrare i valori:

![Sito di amministrazione - Filtri lista BookInstance](admin_improved_bookinstance_list_filters.png)

### Organizzare il layout della vista dettagliata

Di default, le viste dettagliate dispongono tutti i campi verticalmente, nell'ordine di dichiarazione nel modello. Può cambiare l'ordine di dichiarazione, quali campi sono visualizzati (o esclusi), se vengono utilizzate sezioni per organizzare le informazioni, se i campi sono visualizzati orizzontalmente o verticalmente, e persino quali widget di modifica sono usati nei moduli di amministrazione.

> [!NOTE]
> I modelli _LocalLibrary_ sono relativamente semplici, quindi non c'è una grande necessità per noi di cambiare il layout; faremo comunque alcune modifiche solo per mostrarti come.

#### Controllare quali campi vengono visualizzati e disposti

Aggiorni la sua classe `AuthorAdmin` per aggiungere la linea `fields`, come mostrato di seguito:

```python
class AuthorAdmin(admin.ModelAdmin):
    list_display = ('last_name', 'first_name', 'date_of_birth', 'date_of_death')

    fields = ['first_name', 'last_name', ('date_of_birth', 'date_of_death')]
```

L'attributo `fields` elenca solo quei campi che devono essere visualizzati nel modulo, nell'ordine. I campi sono visualizzati verticalmente di default, ma verranno visualizzati orizzontalmente se li raggruppa ulteriormente in una tupla (come mostrato nei campi "date" sopra).

Nel suo sito web vada alla vista dettagliata dell'autore — dovrebbe ora apparire come mostrato di seguito:

![Sito di amministrazione - Dettaglio autore migliorato](admin_improved_author_detail.png)

> [!NOTE]
> Può anche usare l'attributo `exclude` per dichiarare un elenco di attributi da escludere dal modulo (tutti gli altri attributi nel modello verranno visualizzati).

#### Sezionare la vista dettagliata

Può aggiungere "sezioni" per raggruppare le informazioni correlate ai modelli all'interno del modulo dettaglio, usando l'attributo [fieldsets](https://docs.djangoproject.com/en/5.0/ref/contrib/admin/#django.contrib.admin.ModelAdmin.fieldsets).

Nel modello `BookInstance` abbiamo informazioni relative a cosa sia il libro (ad es. `name`, `imprint`, e `id`) e quando sarà disponibile (`status`, `due_back`). Possiamo aggiungerle alla nostra classe `BookInstanceAdmin` come mostrato di seguito, usando la proprietà `fieldsets`.

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

Ogni sezione ha il suo titolo (o `None`, se non si vuole un titolo) e una tupla associata di campi in un dizionario — il formato è complicato da descrivere, ma abbastanza facile da capire se guarda il frammento di codice subito sopra.

Ora navighi a una vista di istanze del libro nel suo sito web; il modulo dovrebbe apparire come illustrato di seguito:

![Sito di amministrazione - Dettaglio BookInstance migliorato con sezioni](admin_improved_bookinstance_detail_sections.png)

### Modifica in linea dei record associati

A volte può avere senso poter aggiungere record associati nello stesso momento. Ad esempio, può avere senso avere sia le informazioni sul libro sia informazioni sulle copie specifiche che ha nella stessa pagina di dettaglio.

Può realizzare questo dichiarando [inlines](https://docs.djangoproject.com/en/5.0/ref/contrib/admin/#django.contrib.admin.ModelAdmin.inlines), di tipo [TabularInline](https://docs.djangoproject.com/en/5.0/ref/contrib/admin/#django.contrib.admin.TabularInline) (layout orizzontale) o [StackedInline](https://docs.djangoproject.com/en/5.0/ref/contrib/admin/#django.contrib.admin.StackedInline) (layout verticale, proprio come il layout predefinito del modello). Può aggiungere le informazioni `BookInstance` in linea al nostro dettaglio `Book` specificando `inlines` nel suo `BookAdmin`:

```python
class BooksInstanceInline(admin.TabularInline):
    model = BookInstance

@admin.register(Book)
class BookAdmin(admin.ModelAdmin):
    list_display = ('title', 'author', 'display_genre')

    inlines = [BooksInstanceInline]
```

Ora navighi a una vista per un `Book` nel suo sito web — in basso dovrebbe ora vedere le istanze di libro relative a questo libro (subito sotto i campi del genere del libro):

![Sito di amministrazione - Libro con inlines](admin_improved_book_detail_inlines.png)

In questo caso tutto quello che abbiamo fatto è dichiarare la nostra classe tabular inline, che aggiunge solo tutti i campi del modello _inlined_. Può specificare tutti i tipi di informazioni aggiuntive per il layout, inclusi i campi da visualizzare, il loro ordine, se sono di sola lettura o meno, ecc. (per ulteriori dettagli, vedere [TabularInline](https://docs.djangoproject.com/en/5.0/ref/contrib/admin/#django.contrib.admin.TabularInline)).

> [!NOTE]
> Ci sono alcuni limiti dolorosi in questa funzionalità! Nello screenshot sopra abbiamo tre istanze di libri esistenti, seguite da tre segnaposti per nuove istanze di libri (che sembrano molto simili!). Sarebbe meglio non avere istanze di libri di riserva di default ma aggiungerle solo con il link **Add another Book instance**, o per essere in grado di elencare le `BookInstance` come link non leggibili da qui. La prima opzione può essere fatta impostando l'attributo `extra` su `0` nel modello `BooksInstanceInline`, provi da sola.

## Metta alla prova se stessa

Abbiamo imparato molto in questa sezione, quindi ora è il momento per lei di provare alcune cose.

1. Per la vista elenco `BookInstance`, aggiungere il codice per visualizzare il libro, lo stato, la data di scadenza e l'id (piuttosto che il testo `__str__()` predefinito).
2. Aggiungere un elenco in linea di elementi `Book` alla vista dettagliata `Author` utilizzando lo stesso approccio che abbiamo usato per `Book`/`BookInstance`.

## Riassunto

È tutto! Ha ora imparato come impostare il sito di amministrazione sia nella sua forma più semplice, sia in una forma migliorata, come creare un superuser, e come navigare nel sito di amministrazione e visualizzare, eliminare, e aggiornare i record. Lungo la strada ha creato una serie di Books, BookInstances, Genres, e Authors che saremo in grado di elencare e visualizzare una volta create le nostre viste e modelli personali.

## Letture ulteriori

- [Scrivere la sua prima app Django, parte 2: Introduzione al sito di amministrazione Django](https://docs.djangoproject.com/en/5.0/intro/tutorial02/#introducing-the-django-admin) (documenti Django)
- [Il sito di amministrazione Django](https://docs.djangoproject.com/en/5.0/ref/contrib/admin/) (Documenti Django)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Models", "Learn_web_development/Extensions/Server-side/Django/Home_page", "Learn_web_development/Extensions/Server-side/Django")}}
