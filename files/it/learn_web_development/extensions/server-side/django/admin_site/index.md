---
title: "Guida Django Parte 4: Sito amministrativo di Django"
short-title: "4: Sito amministrativo di Django"
slug: Learn_web_development/Extensions/Server-side/Django/Admin_site
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Models", "Learn_web_development/Extensions/Server-side/Django/Home_page", "Learn_web_development/Extensions/Server-side/Django")}}

Ora che abbiamo creato i modelli per il sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website), useremo il sito amministrativo di Django per aggiungere alcuni dati di libri "reali". Prima ti mostreremo come registrare i modelli con il sito amministrativo, poi ti mostreremo come effettuare il login e creare alcuni dati. Alla fine dell'articolo mostreremo alcuni dei modi in cui puoi ulteriormente migliorare la presentazione del sito amministrativo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Completa prima: <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/Models"
          >Guida Django Parte 3: Uso dei modelli</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere i benefici e le limitazioni del sito amministrativo di Django e utilizzarlo per creare alcuni record per i nostri modelli.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

L'_applicazione_ amministrativa di Django può utilizzare i tuoi modelli per costruire automaticamente un'area del sito che puoi usare per creare, visualizzare, aggiornare ed eliminare record. Questo può farti risparmiare molto tempo durante lo sviluppo, rendendo molto facile testare i tuoi modelli e capire se hai i dati _giusti_. L'applicazione amministrativa può anche essere utile per gestire i dati in produzione, a seconda del tipo di sito web. Il progetto Django la raccomanda solo per la gestione interna dei dati (ovvero solo per l'uso da parte di amministratori, o persone interne alla tua organizzazione), poiché l'approccio centrato sui modelli non è necessariamente la miglior interfaccia possibile per tutti gli utenti ed espone molti dettagli inutili sui modelli.

Tutta la configurazione richiesta per includere l'applicazione amministrativa nel tuo sito è stata fatta automaticamente quando hai [creato il progetto scheletro](/it/docs/Learn_web_development/Extensions/Server-side/Django/skeleton_website) (per informazioni sulle reali dipendenze necessarie, vedi i [documenti di Django qui](https://docs.djangoproject.com/en/5.0/ref/contrib/admin/)). Di conseguenza, tutto quello che **devi** fare per aggiungere i tuoi modelli all'applicazione amministrativa è _registrarli_. Alla fine di questo articolo forniremo una breve dimostrazione di come potresti ulteriormente configurare l'area amministrativa per visualizzare meglio i nostri dati del modello.

Dopo aver registrato i modelli ti mostreremo come creare un nuovo "superuser", effettuare il login al sito e creare alcuni libri, autori, istanze di libri e generi. Questi saranno utili per testare le visualizzazioni e i template che inizieremo a creare nel prossimo tutorial.

## Registrare i modelli

Per prima cosa, apri **admin.py** nell'applicazione catalogo (**/django-locallibrary-tutorial/catalog/admin.py**). Al momento appare così — nota che importa già `django.contrib.admin`:

```python
from django.contrib import admin

# Register your models here.
```

Registrare i modelli copiando il seguente testo alla fine del file. Questo codice importa i modelli e poi chiama `admin.site.register` per registrare ciascuno di essi.

```python
from .models import Author, Genre, Book, BookInstance, Language

admin.site.register(Book)
admin.site.register(Author)
admin.site.register(Genre)
admin.site.register(BookInstance)
admin.site.register(Language)
```

> [!NOTE]
> Le righe sopra assumono che tu abbia accettato la sfida di creare un modello per rappresentare la lingua naturale di un libro ([vedi l'articolo del tutorial sui modelli](/it/docs/Learn_web_development/Extensions/Server-side/Django/Models))!

Questo è il modo più semplice per registrare un modello o modelli con il sito. Il sito amministrativo è altamente personalizzabile, e parleremo di altri modi di registrare i tuoi modelli più avanti.

## Creare un superuser

Per effettuare il login al sito amministrativo, abbiamo bisogno di un account utente con lo stato _Staff_ abilitato. Per visualizzare e creare record abbiamo anche bisogno che questo utente abbia i permessi per gestire tutti i nostri oggetti. Puoi creare un account "superuser" che ha accesso completo al sito e tutti i permessi necessari usando **manage.py**.

Esegui il seguente comando, nella stessa directory di **manage.py**, per creare il superuser. Ti verrà chiesto di inserire un nome utente, un indirizzo email e una password _forte_.

```bash
python3 manage.py createsuperuser
```

Una volta completato questo comando, verrà aggiunto un nuovo superuser al database. Ora riavvia il server di sviluppo in modo che possiamo testare il login:

```bash
python3 manage.py runserver
```

## Effettuare il login e usare il sito

Per accedere al sito, apri l'URL _/admin_ (ad esempio, `http://127.0.0.1:8000/admin`) e inserisci le tue credenziali di userid e password del nuovo superuser (verrai reindirizzato alla pagina di _login_ e poi di nuovo all'URL _/admin_ dopo aver inserito i tuoi dettagli).

Questa parte del sito visualizza tutti i nostri modelli, raggruppati per applicazione installata. Puoi cliccare su un nome di modello per andare a una schermata che elenca tutti i suoi record associati, e puoi cliccare ulteriormente su quei record per modificarli. Puoi anche cliccare direttamente sul link **Add** accanto a ciascun modello per iniziare a creare un record di quel tipo.

![Admin Site - Home page](admin_home.png)

Clicca sul link **Add** a destra di _Books_ per creare un nuovo libro (questo visualizzerà una finestra di dialogo molto simile a quella sottostante). Nota come i titoli di ciascun campo, il tipo di widget utilizzato e il `help_text` (se presente) corrispondono ai valori che hai specificato nel modello.

Inserisci i valori per i campi. Puoi creare nuovi autori o generi premendo il pulsante **+** accanto ai rispettivi campi (o selezionare valori esistenti dalle liste se li hai già creati). Quando hai finito puoi premere **SAVE**, **Save and add another**, o **Save and continue editing** per salvare il record.

![Admin Site - Book Add](admin_book_add.png)

> [!NOTE]
> A questo punto ti invitiamo a dedicare un po' di tempo ad aggiungere alcuni libri, autori, lingue e generi (ad esempio, Fantasy) alla tua applicazione. Assicurati che ogni autore e genere includa un paio di libri diversi (questo renderà più interessanti le tue viste elenco e dettaglio quando le implementeremo successivamente nella serie di articoli).

Quando hai finito di aggiungere libri, clicca sul link **Home** nel segnalibro superiore per tornare alla pagina principale dell'amministratore. Poi clicca sul link **Books** per visualizzare l'elenco attuale dei libri (o su uno degli altri link per vedere altri elenchi di modelli). Ora che hai aggiunto alcuni libri, l'elenco potrebbe risultare simile allo screenshot qui sotto. Viene visualizzato il titolo di ciascun libro; questo è il valore restituito nel metodo `__str__()` del modello Book che abbiamo specificato nel precedente articolo.

![Admin Site - List of book objects](admin_book_list.png)

Da questo elenco puoi eliminare i libri selezionando la casella di controllo accanto al libro che non desideri, selezionando l'azione _delete..._ dal menu a discesa _Action_ e quindi premendo il pulsante **Go**. Puoi anche aggiungere nuovi libri premendo il pulsante **ADD BOOK**.

Puoi modificare un libro selezionando il suo nome nel link. La pagina di modifica di un libro, mostrata di seguito, è quasi identica alla pagina "Add". Le principali differenze sono il titolo della pagina (_Change book_) e l'aggiunta dei pulsanti **Delete**, **HISTORY** e **VIEW ON SITE** (quest'ultimo pulsante appare perché abbiamo definito il metodo `get_absolute_url()` nel nostro modello).

> [!NOTE]
> Cliccando sul pulsante **VIEW ON SITE** si solleva un'eccezione `NoReverseMatch` perché il metodo `get_absolute_url()` tenta di `reverse()` un mapping URL denominato ('book-detail') che non è ancora stato definito.
> Definiremo un mapping URL e una vista associata in [Guida Django Parte 6: Viste elenco e dettaglio generiche](/it/docs/Learn_web_development/Extensions/Server-side/Django/Generic_views).

![Admin Site - Book Edit](admin_book_modify.png)

Ora torna alla pagina **Home** (usando il link _Home_ nel breadcrumb) e poi visualizza l'elenco **Author** e **Genre** — dovresti già averne creati abbastanza quando hai aggiunto i nuovi libri, ma sentiti libero di aggiungerne altri.

Quello che non avrai è un qualsiasi elemento _Book Instances_, perché questi non vengono creati dai Books (anche se puoi creare un `Book` da un `BookInstance` — questa è la natura del campo `ForeignKey`). Torna alla pagina _Home_ e premi il pulsante **Add** associato per visualizzare la schermata _Add book instance_ di seguito. Nota l'Id univoco globale grande, che può essere usato per identificare separatamente una singola copia di un libro in biblioteca.

![Admin Site - BookInstance Add](admin_bookinstance_add.png)

Crea un certo numero di questi record per ciascuno dei tuoi libri. Imposta lo stato come _Available_ per almeno alcuni record e _On loan_ per altri. Se lo stato **non** è _Available_, allora imposta anche una data _Due back_ futura.

Ecco fatto! Hai ora imparato come configurare e utilizzare il sito di amministrazione. Hai anche creato record per `Book`, `BookInstance`, `Genre`, `Language` e `Author` che saremo in grado di utilizzare una volta che avremo creato le nostre viste e template.

## Configurazione avanzata

Django fa un buon lavoro nel creare un sito amministrativo di base usando le informazioni dai modelli registrati:

- Ogni modello ha un elenco di record individuali, identificati dalla stringa creata con il metodo `__str__()` del modello, e collegati alle viste/form di dettaglio per la modifica. Per impostazione predefinita, questa vista ha un menu azioni in cima che puoi utilizzare per eseguire operazioni di eliminazione in blocco sui record.
- Le form di record di dettaglio del modello per la modifica e l'aggiunta di record contengono tutti i campi nel modello, disposti verticalmente nell'ordine di dichiarazione.

Puoi ulteriormente personalizzare l'interfaccia per renderla ancora più facile da usare. Alcune delle cose che puoi fare sono:

- Viste elenco:

  - Aggiungi campi/informazioni aggiuntivi visualizzati per ciascun record.
  - Aggiungi filtri per selezionare quali record sono elencati, basati su data o qualche altro valore di selezione (ad esempio, stato del prestito del libro).
  - Aggiungi opzioni aggiuntive al menu delle azioni nelle viste elenco e scegli dove visualizzare questo menu nel form.

- Viste dettaglio

  - Scegli quali campi visualizzare (o escludere), insieme al loro ordine, raggruppamento, se sono modificabili, il widget utilizzato, l'orientamento ecc.
  - Aggiungi campi correlati a un record per consentire la modifica inline (ad esempio, aggiungi la possibilità di aggiungere e modificare i record dei libri mentre stai creando il loro record dell'autore).

In questa sezione esamineremo alcune modifiche che miglioreranno l'interfaccia per la nostra _LocalLibrary_, includendo l'aggiunta di più informazioni agli elenchi di modelli `Book` e `Author` e il miglioramento del layout delle loro viste di modifica. Non cambieremo la presentazione del modello `Language` e `Genre` perché hanno solo un campo ciascuno, quindi non c'è un reale vantaggio nel farlo!

Puoi trovare un riferimento completo di tutte le opzioni di personalizzazione del sito amministrativo ne [Il sito amministrativo di Django](https://docs.djangoproject.com/en/5.0/ref/contrib/admin/) (Documenti di Django).

### Registrare una classe ModelAdmin

Per cambiare come un modello viene visualizzato nell'interfaccia amministrativa, devi definire una classe [ModelAdmin](https://docs.djangoproject.com/en/5.0/ref/contrib/admin/#modeladmin-objects) (che descrive il layout) e registrarla con il modello.

Iniziamo con il modello `Author`. Apri **admin.py** nell'applicazione catalogo (**/django-locallibrary-tutorial/catalog/admin.py**). Commenta la tua registrazione originale (prefissala con un #) per il modello `Author`:

```python
# admin.site.register(Author)
```

Ora aggiungi un nuovo `AuthorAdmin` e la registrazione come mostrato di seguito.

```python
# Define the admin class
class AuthorAdmin(admin.ModelAdmin):
    pass

# Register the admin class with the associated model
admin.site.register(Author, AuthorAdmin)
```

Ora aggiungeremo classi `ModelAdmin` per `Book` e `BookInstance`. Dobbiamo di nuovo commentare le registrazioni originali:

```python
# admin.site.register(Book)
# admin.site.register(BookInstance)
```

Ora per creare e registrare i nuovi modelli; per scopi dimostrativi, useremo invece il decorator `@register` per registrare i modelli (questo fa esattamente la stessa cosa della sintassi `admin.site.register()`):

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

Attualmente tutte le nostre classi amministrative sono vuote (vedi `pass`) quindi il comportamento amministrativo rimarrà invariato! Possiamo ora estendere queste classi per definire comportamenti amministrativi specifici del nostro modello.

### Configurare le viste elenco

Attualmente la _LocalLibrary_ elenca tutti gli autori usando il nome dell'oggetto generato dal metodo `__str__()` del modello. Questo va bene quando ci sono pochi autori, ma una volta che ce ne sono molti potresti finire con duplicati. Per differenziarli, o semplicemente perché vuoi mostrare informazioni più interessanti su ogni autore, puoi usare [list_display](https://docs.djangoproject.com/en/5.0/ref/contrib/admin/#django.contrib.admin.ModelAdmin.list_display) per aggiungere campi aggiuntivi alla vista.

Sostituisci la tua classe `AuthorAdmin` con il codice qui sotto. I nomi dei campi da visualizzare nell'elenco sono dichiarati in una _tupla_ nell'ordine richiesto, come mostrato (questi sono gli stessi nomi specificati nel tuo modello originale).

```python
class AuthorAdmin(admin.ModelAdmin):
    list_display = ('last_name', 'first_name', 'date_of_birth', 'date_of_death')
```

Ora naviga verso l'elenco degli autori nel tuo sito web. I campi sopra dovrebbero essere ora visualizzati, in questo modo:

![Admin Site - Improved Author List](admin_improved_author_list.png)

Per il nostro modello `Book` visualizzeremo inoltre `author` e `genre`. L'`author` è un campo `ForeignKey` (relazione uno-a-molti), e quindi sarà rappresentato dal valore `__str__()` per il record associato. Sostituisci la classe `BookAdmin` con la versione sotto.

```python
class BookAdmin(admin.ModelAdmin):
    list_display = ('title', 'author', 'display_genre')
```

Sfortunatamente non possiamo specificare direttamente il campo `genre` in `list_display` perché è un `ManyToManyField` (Django lo impedisce perché ci sarebbe un grande "costo" di accesso al database nel farlo). Invece definiremo una funzione `display_genre` per ottenere l'informazione come stringa (questa è la funzione che abbiamo chiamato sopra; la definiremo qui sotto).

> [!NOTE]
> Ottenere il `genre` potrebbe non essere una buona idea qui, a causa del "costo" dell'operazione sul database. Ti stiamo mostrando come farlo perché chiamare funzioni nei tuoi modelli può essere molto utile per altri motivi — ad esempio per aggiungere un link _Delete_ accanto a ogni elemento nell'elenco.

Aggiungi il seguente codice nel tuo modello `Book` (**models.py**). Questo crea una stringa dai primi tre valori del campo `genre` (se esistono) e crea una `short_description` che può essere usata nel sito amministrativo per questo metodo.

```python
def display_genre(self):
    """Create a string for the Genre. This is required to display genre in Admin."""
    return ', '.join(genre.name for genre in self.genre.all()[:3])

display_genre.short_description = 'Genre'
```

Dopo aver salvato il modello e aggiornato l'admin, apri il tuo sito web e vai alla pagina dell'elenco _Books_; dovresti vedere un elenco di libri come quello qui sotto:

![Admin Site - Improved Book List](admin_improved_book_list.png)

Il modello `Genre` (e il modello `Language`, se ne hai definito uno) entrambi hanno un singolo campo, quindi non ha senso creare un modello aggiuntivo per visualizzare campi aggiuntivi.

> [!NOTE]
> Vale la pena aggiornare l'elenco del modello `BookInstance` per mostrare almeno lo stato e la data di ritorno prevista. L'abbiamo aggiunto come una sfida alla fine di questo articolo!

### Aggiungere filtri elenco

Una volta che hai molti elementi in un elenco, può essere utile poter filtrare quali elementi sono visualizzati.
Questo viene fatto elencando i campi nell'attributo `list_filter`.
Sostituisci la tua classe `BookInstanceAdmin` attuale con il frammento di codice sotto.

```python
class BookInstanceAdmin(admin.ModelAdmin):
    list_filter = ('status', 'due_back')
```

La vista elenco ora includerà una casella di filtro a destra. Nota come puoi scegliere date e stati per filtrare i valori:

![Admin Site - BookInstance List Filters](admin_improved_bookinstance_list_filters.png)

### Organizzare il layout delle viste dettaglio

Per impostazione predefinita, le viste di dettaglio dispongono tutti i campi verticalmente, nel loro ordine di dichiarazione nel modello. Puoi cambiare l'ordine di dichiarazione, quali campi sono visualizzati (o esclusi), se si utilizzano sezioni per organizzare le informazioni, se i campi sono visualizzati orizzontalmente o verticalmente e persino quali widget di modifica sono usati nelle forme amministrative.

> [!NOTE]
> I modelli _LocalLibrary_ sono relativamente semplici quindi non c'è un grande bisogno per noi di cambiare il layout; faremo comunque alcune modifiche, solo per mostrarti come.

#### Controllare quali campi vengono visualizzati e disposti

Aggiorna la tua classe `AuthorAdmin` per aggiungere la riga `fields`, come mostrato qui sotto:

```python
class AuthorAdmin(admin.ModelAdmin):
    list_display = ('last_name', 'first_name', 'date_of_birth', 'date_of_death')

    fields = ['first_name', 'last_name', ('date_of_birth', 'date_of_death')]
```

L'attributo `fields` elenca solo quei campi che devono essere visualizzati sul modulo, in ordine. I campi sono visualizzati verticalmente per impostazione predefinita, ma verranno visualizzati orizzontalmente se li raggruppi ulteriormente in una tupla (come mostrato nei campi "date" sopra).

Nel tuo sito web, vai alla vista dettaglio dell'autore — dovrebbe ora apparire come mostrato qui sotto:

![Admin Site - Improved Author Detail](admin_improved_author_detail.png)

> [!NOTE]
> Puoi anche usare l'attributo `exclude` per dichiarare un elenco di attributi da escludere dal modulo (tutti gli altri attributi nel modello verranno visualizzati).

#### Inserire sezioni nella vista dettaglio

Puoi aggiungere "sezioni" per raggruppare informazioni correlate al modello all'interno del modulo di dettaglio, usando l'attributo [fieldsets](https://docs.djangoproject.com/en/5.0/ref/contrib/admin/#django.contrib.admin.ModelAdmin.fieldsets).

Nel modello `BookInstance` abbiamo informazioni relative a cosa sia il libro (cioè, `name`, `imprint` e `id`) e quando sarà disponibile (`status`, `due_back`). Possiamo aggiungere queste informazioni alla nostra classe `BookInstanceAdmin` come mostrato di seguito, usando la proprietà `fieldsets`.

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

Ogni sezione ha il suo titolo (o `None` se non vuoi un titolo) e una tupla associata di campi in un dizionario — il formato è complicato da descrivere, ma abbastanza facile da capire se guardi al frammento di codice immediatamente sopra.

Ora naviga verso la vista di un'istanza di libro nel tuo sito web; il form dovrebbe apparire come mostrato qui sotto:

![Admin Site - Improved BookInstance Detail with sections](admin_improved_bookinstance_detail_sections.png)

### Modifica inline di record associati

A volte può avere senso essere in grado di aggiungere record associati allo stesso tempo. Ad esempio, può avere senso avere sia le informazioni sui libri sia le informazioni sulle copie specifiche che hai nella stessa pagina di dettaglio.

Puoi farlo dichiarando [inlines](https://docs.djangoproject.com/en/5.0/ref/contrib/admin/#django.contrib.admin.ModelAdmin.inlines), di tipo [TabularInline](https://docs.djangoproject.com/en/5.0/ref/contrib/admin/#django.contrib.admin.TabularInline) (layout orizzontale) o [StackedInline](https://docs.djangoproject.com/en/5.0/ref/contrib/admin/#django.contrib.admin.StackedInline) (layout verticale, proprio come il layout del modello predefinito). Puoi aggiungere le informazioni `BookInstance` inline al nostro dettaglio `Book` specificando `inlines` nel tuo `BookAdmin`:

```python
class BooksInstanceInline(admin.TabularInline):
    model = BookInstance

@admin.register(Book)
class BookAdmin(admin.ModelAdmin):
    list_display = ('title', 'author', 'display_genre')

    inlines = [BooksInstanceInline]
```

Ora naviga verso una vista per un `Book` nel tuo sito web — in fondo dovresti ora vedere le istanze del libro relative a questo libro (immediatamente sotto i campi del genere del libro):

![Admin Site - Book with Inlines](admin_improved_book_detail_inlines.png)

In questo caso tutto ciò che abbiamo fatto è dichiarare la nostra classe tabulare inline, che aggiunge semplicemente tutti i campi dal modello _inlined_. Puoi specificare ogni sorta di informazione aggiuntiva per il layout, inclusi i campi da visualizzare, il loro ordine, se sono di sola lettura o meno, ecc. (vedi [TabularInline](https://docs.djangoproject.com/en/5.0/ref/contrib/admin/#django.contrib.admin.TabularInline) per ulteriori informazioni).

> [!NOTE]
> Ci sono alcuni limiti dolorosi in questa funzionalità! Nello screenshot sopra abbiamo tre istanze di libro esistenti, seguite da tre segnaposti per nuove istanze di libro (che sembrano molto simili!). Sarebbe meglio non avere istanze di libro in più per impostazione predefinita e aggiungerle solo con il link **Add another Book instance**, o essere in grado di elencare semplicemente le `BookInstance` come link non leggibili da qui. La prima opzione può essere fatta impostando l'attributo `extra` a `0` nel modello `BooksInstanceInline`, prova tu stesso.

## Sfida te stesso

Abbiamo imparato molto in questa sezione, quindi ora è il momento per te di provare alcune cose.

1. Per la vista elenco di `BookInstance`, aggiungi il codice per visualizzare il libro, lo stato, la data di ritorno prevista e l'id (anziché il testo `__str__()` predefinito).
2. Aggiungi un elenco inline di elementi `Book` alla vista dettaglio `Author` utilizzando lo stesso approccio che abbiamo usato per `Book`/`BookInstance`.

## Sommario

Ecco fatto! Hai ora imparato come configurare il sito amministrativo nella sua forma più semplice e migliorata, come creare un superuser e come navigare nel sito amministrativo e visualizzare, eliminare e aggiornare i record. Lungo la strada hai creato un sacco di Books, BookInstances, Genres e Authors che saremo in grado di elencare e visualizzare una volta che creiamo la nostra vista e templates.

## Ulteriori letture

- [Scrivere la tua prima app Django, parte 2: Introduzione al sito amministrativo di Django](https://docs.djangoproject.com/en/5.0/intro/tutorial02/#introducing-the-django-admin) (documenti di Django)
- [Il sito amministrativo di Django](https://docs.djangoproject.com/en/5.0/ref/contrib/admin/) (Documenti di Django)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Models", "Learn_web_development/Extensions/Server-side/Django/Home_page", "Learn_web_development/Extensions/Server-side/Django")}}
