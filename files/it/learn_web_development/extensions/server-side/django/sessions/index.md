---
title: "Tutorial di Django Parte 7: Framework delle sessioni"
short-title: "7: Framework delle sessioni"
slug: Learn_web_development/Extensions/Server-side/Django/Sessions
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Generic_views", "Learn_web_development/Extensions/Server-side/Django/Authentication", "Learn_web_development/Extensions/Server-side/Django")}}

Questo tutorial estende il nostro sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website), aggiungendo un contatore di visite basato su sessioni alla pagina principale. Questo è un esempio relativamente semplice, ma mostra come puoi utilizzare il framework delle sessioni per fornire un comportamento persistente per gli utenti anonimi nei tuoi siti.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Completa tutti i precedenti argomenti del tutorial, incluso <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/Generic_views">Tutorial di Django Parte 6: Viste elenchi e dettagli generiche</a>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Comprendere come vengono utilizzate le sessioni.</td>
    </tr>
  </tbody>
</table>

## Panoramica

Il sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website) creato nei tutorial precedenti consente agli utenti di navigare tra i libri e gli autori nel catalogo. Sebbene il contenuto sia generato dinamicamente dal database, ogni utente avrà essenzialmente accesso alle stesse pagine e tipi di informazioni quando utilizza il sito.

In una biblioteca "reale" potresti desiderare fornire agli utenti individuali un'esperienza personalizzata, basata sull'uso precedente del sito, preferenze, ecc. Ad esempio, potresti nascondere i messaggi di avviso che l'utente ha già riconosciuto la volta successiva che visita il sito, oppure memorizzare e rispettare le loro preferenze (come la quantità di risultati di ricerca che vogliono visualizzare in ogni pagina).

Il framework delle sessioni ti permette di implementare questo tipo di comportamento, consentendo di memorizzare e recuperare dati arbitrari per ogni visitatore del sito.

## Cosa sono le sessioni?

Tutta la comunicazione tra browser e server web avviene tramite {{Glossary("HTTP", "HTTP")}}, che è _stateless_. Il fatto che il protocollo sia stateless significa che i messaggi tra il client e il server sono completamente indipendenti l'uno dall'altro — non esiste un concetto di "sequenza" o comportamento basato su messaggi precedenti. Di conseguenza, se vuoi avere un sito che tenga traccia delle relazioni in corso con un client, devi implementare tu stesso tale funzionalità.

Le sessioni sono il meccanismo utilizzato da Django (e dalla maggior parte di Internet) per tenere traccia dello "stato" tra il sito e un particolare browser. Le sessioni ti permettono di memorizzare dati arbitrari per ogni browser, e avere questi dati disponibili per il sito ogni volta che il browser si connette. Gli elementi di dati individuali associati alla sessione sono poi referenziati da una "chiave", usata sia per memorizzare che per recuperare i dati.

Django utilizza un cookie contenente un _session id_ speciale per identificare ogni browser e la sua sessione associata con il sito. I dati della sessione effettivi sono memorizzati nel database del sito per impostazione predefinita (questo è più sicuro che memorizzare i dati in un cookie, dove sono più vulnerabili agli utenti malintenzionati). Puoi configurare Django per memorizzare i dati della sessione in altri luoghi (cache, file, "cookie sicuri"), ma la posizione predefinita è una buona opzione relativamente sicura.

## Abilitare le sessioni

Le sessioni sono state abilitate automaticamente quando abbiamo [creato lo scheletro del sito web](/it/docs/Learn_web_development/Extensions/Server-side/Django/skeleton_website) (nel tutorial 2).

La configurazione è impostata nelle sezioni `INSTALLED_APPS` e `MIDDLEWARE` del file di progetto (**django-locallibrary-tutorial/locallibrary/settings.py**), come mostrato di seguito:

```python
INSTALLED_APPS = [
    # …
    'django.contrib.sessions',
    # …

MIDDLEWARE = [
    # …
    'django.contrib.sessions.middleware.SessionMiddleware',
    # …
```

## Utilizzare le sessioni

Puoi accedere all'attributo `session` all'interno di una vista dal parametro `request` (un `HttpRequest` passato come primo argomento alla vista). Questo attributo session rappresenta la connessione specifica all'utente corrente (o per essere più precisi, la connessione al _browser_ corrente, come identificato dal session id nel cookie del browser per questo sito).

L'attributo `session` è un oggetto simile a un dizionario che puoi leggere e scrivere tutte le volte che vuoi nella tua vista, modificandolo secondo i desideri. Puoi eseguire tutte le normali operazioni su un dizionario, inclusa la cancellazione di tutti i dati, verifica se una chiave è presente, scorrere i dati, ecc. La maggior parte delle volte, tuttavia, utilizzerai semplicemente l'API "dictionary" standard per ottenere e impostare i valori.

I frammenti di codice mostrano come puoi ottenere, impostare e cancellare alcuni dati con la chiave `my_car`, associati alla sessione corrente (browser).

> [!NOTE]
> Una delle grandi cose di Django è che non devi pensare ai meccanismi che legano la sessione alla richiesta corrente nella tua vista. Se dovessimo usare i frammenti di codice qui sotto nella nostra vista, sapremmo che le informazioni su `my_car` sono associate solo al browser che ha inviato la richiesta corrente.

```python
# Get a session value by its key (e.g. 'my_car'), raising a KeyError if the key is not present
my_car = request.session['my_car']

# Get a session value, setting a default if it is not present ('mini')
my_car = request.session.get('my_car', 'mini')

# Set a session value
request.session['my_car'] = 'mini'

# Delete a session value
del request.session['my_car']
```

L'API offre anche una serie di altri metodi utilizzati principalmente per gestire il cookie di sessione associato. Ad esempio, ci sono metodi per verificare che i cookie siano supportati nel browser client, per impostare e controllare le date di scadenza dei cookie, e per cancellare sessioni scadute dall'archivio dati. Puoi trovare l'API completa in [Come usare le sessioni](https://docs.djangoproject.com/en/5.0/topics/http/sessions/) (documentazione di Django).

## Salvare i dati della sessione

Per impostazione predefinita, Django salva solo nel database della sessione e invia il cookie della sessione al client quando la sessione è stata _modificata_ (assegnata) o _cancellata_. Se stai aggiornando alcuni dati utilizzando la sua chiave di sessione come mostrato nella sezione precedente, non devi preoccuparti di questo! Ad esempio:

```python
# This is detected as an update to the session, so session data is saved.
request.session['my_car'] = 'mini'
```

Se stai aggiornando alcune informazioni _all'interno_ dei dati della sessione, Django non riconoscerà che hai apportato una modifica alla sessione e salverà i dati (ad esempio, se cambiassi i dati `wheels` all'interno dei tuoi dati `my_car`, come mostrato di seguito). In questo caso dovrai esplicitamente segnare la sessione come modificata.

```python
# Session object not directly modified, only data within the session. Session changes not saved!
request.session['my_car']['wheels'] = 'alloy'

# Set session as modified to force data updates/cookie to be saved.
request.session.modified = True
```

> [!NOTE]
> Puoi cambiare il comportamento in modo che il sito aggiorni il database/invii il cookie ad ogni richiesta aggiungendo `SESSION_SAVE_EVERY_REQUEST = True` nelle impostazioni del tuo progetto (**django-locallibrary-tutorial/locallibrary/settings.py**).

## Esempio semplice — ottenere il conteggio delle visite

Come un semplice esempio del mondo reale aggiorneremo la nostra biblioteca per comunicare all'utente corrente quante volte ha visitato la pagina principale della _LocalLibrary_.

Apri **/django-locallibrary-tutorial/catalog/views.py**, e aggiungi le righe che contengono `num_visits` in `index()` (come mostrato di seguito).

```python
def index(request):
    # …

    num_authors = Author.objects.count()  # The 'all()' is implied by default.

    # Number of visits to this view, as counted in the session variable.
    num_visits = request.session.get('num_visits', 0)
    num_visits += 1
    request.session['num_visits'] = num_visits

    context = {
        'num_books': num_books,
        'num_instances': num_instances,
        'num_instances_available': num_instances_available,
        'num_authors': num_authors,
        'num_visits': num_visits,
    }

    # Render the HTML template index.html with the data in the context variable.
    return render(request, 'index.html', context=context)
```

Qui otteniamo prima il valore della chiave di sessione `'num_visits'`, impostando il valore a 0 se non è stato precedentemente impostato. Ogni volta che viene ricevuta una richiesta, incrementiamo poi il valore e lo memorizziamo nuovamente nella sessione (per la prossima volta che l'utente visita la pagina). La variabile `num_visits` viene poi passata al template nella nostra variabile contesto.

> [!NOTE]
> Qui potremmo anche testare se i cookie sono addirittura supportati nel browser (vedi [Come usare le sessioni](https://docs.djangoproject.com/en/5.0/topics/http/sessions/) per esempi) o progettare la nostra interfaccia utente in modo che non importi se i cookie sono supportati o meno.

Aggiungi la riga mostrata in fondo al blocco seguente al tuo template HTML principale (**/django-locallibrary-tutorial/catalog/templates/index.html**) in fondo alla sezione "Contenuto dinamico" per visualizzare la variabile contesto `num_visits`.

```django
<h2>Dynamic content</h2>

<p>The library has the following record counts:</p>
<ul>
  <li><strong>Books:</strong> \{{ num_books }}</li>
  <li><strong>Copies:</strong> \{{ num_instances }}</li>
  <li><strong>Copies available:</strong> \{{ num_instances_available }}</li>
  <li><strong>Authors:</strong> \{{ num_authors }}</li>
</ul>

<p>
  You have visited this page \{{ num_visits }} time\{{ num_visits|pluralize }}.
</p>
```

Nota che usiamo il tag template integrato di Django [pluralize](https://docs.djangoproject.com/en/5.0/ref/templates/builtins/#pluralize) per aggiungere una "s" quando la pagina è stata visitata più volte.

Salva le tue modifiche e riavvia il server di test. Ogni volta che aggiorni la pagina, il numero dovrebbe aggiornarsi.

## Sommario

Adesso sai quanto sia facile usare le sessioni per migliorare l'interazione con gli utenti _anonimi_.

Nei nostri prossimi articoli spiegheremo il framework di autenticazione e autorizzazione (permessi) e ti mostreremo come supportare gli account utente.

## Vedere anche

- [Come usare le sessioni](https://docs.djangoproject.com/en/5.0/topics/http/sessions/) (documentazione di Django)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Generic_views", "Learn_web_development/Extensions/Server-side/Django/Authentication", "Learn_web_development/Extensions/Server-side/Django")}}
