---
title: "Tutorial Django Parte 7: Framework delle sessioni"
short-title: "7: Framework delle sessioni"
slug: Learn_web_development/Extensions/Server-side/Django/Sessions
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Generic_views", "Learn_web_development/Extensions/Server-side/Django/Authentication", "Learn_web_development/Extensions/Server-side/Django")}}

Questo tutorial estende il nostro sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website), aggiungendo un contatore delle visite basato sulle sessioni alla pagina principale.
Questo è un esempio relativamente semplice, ma mostra come si può utilizzare il framework delle sessioni per fornire un comportamento persistente per gli utenti anonimi nei propri siti.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Completare tutti gli argomenti del tutorial precedenti, inclusa la <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/Generic_views">Lezione Django Parte 6: Visualizzazioni generiche di lista e dettaglio</a>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Capire come vengono utilizzate le sessioni.</td>
    </tr>
  </tbody>
</table>

## Panoramica

Il sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website) che abbiamo creato nei tutorial precedenti consente agli utenti di sfogliare libri e autori nel catalogo. Sebbene il contenuto sia generato dinamicamente dal database, ogni utente avrà essenzialmente accesso alle stesse pagine e tipologie di informazioni quando utilizza il sito.

In una biblioteca "reale" potrebbe essere desiderabile fornire agli utenti un'esperienza personalizzata, basata sul loro precedente utilizzo del sito, preferenze, ecc.
Ad esempio, si potrebbero nascondere i messaggi di avviso che l'utente ha già riconosciuto alla prossima visita al sito, oppure memorizzare e rispettare le loro preferenze (ad esempio, il numero di risultati di ricerca che vogliono vedere su ogni pagina).

Il framework delle sessioni consente di implementare questo tipo di comportamento, permettendo di memorizzare e recuperare dati arbitrari su base per-visitatore del sito.

## Cosa sono le sessioni?

Tutte le comunicazioni tra browser web e server avvengono tramite {{Glossary("HTTP", "HTTP")}}, che è _stateless_. Il fatto che il protocollo sia stateless significa che i messaggi tra il client e il server sono completamente indipendenti l'uno dall'altro — non vi è alcuna nozione di "sequenza" o comportamento basato su messaggi precedenti. Di conseguenza, se si desidera un sito che tenga traccia delle relazioni in corso con un client, è necessario implementarlo autonomamente.

Le sessioni sono il meccanismo utilizzato da Django (e dalla maggior parte di Internet) per tenere traccia dello "stato" tra il sito e un determinato browser. Le sessioni consentono di memorizzare dati arbitrari per browser, e avere questi dati disponibili per il sito ogni volta che il browser si connette. Gli elementi di dati individuali associati alla sessione vengono poi referenziati da un "key", che viene usato per memorizzare e recuperare i dati.

Django utilizza un cookie contenente un _session id_ speciale per identificare ciascun browser e la sua sessione associata con il sito. I _dati_ della sessione effettiva sono memorizzati nel database del sito per impostazione predefinita (questo è più sicuro rispetto a memorizzare i dati in un cookie, dove sono più vulnerabili agli utenti malintenzionati). È possibile configurare Django affinché memorizzi i dati della sessione in altri luoghi (cache, file, cookie "sicuri"), ma la posizione predefinita è una buona opzione relativamente sicura.

## Abilitare le sessioni

Le sessioni sono state abilitate automaticamente quando abbiamo [creato il sito web base](/it/docs/Learn_web_development/Extensions/Server-side/Django/skeleton_website) (nel tutorial 2).

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

Si può accedere all'attributo `session` all'interno di una vista dal parametro `request` (un `HttpRequest` passato come primo argomento alla vista).
Questo attributo session rappresenta la connessione specifica all'utente corrente (o, per essere più precisi, la connessione al _browser_ corrente, identificata dall'id della sessione nel cookie del browser per questo sito).

L'attributo `session` è un oggetto simile a un dizionario che può essere letto e scritto tutte le volte che si desidera nella propria vista, modificandolo a piacere. Si possono eseguire tutte le normali operazioni sui dizionari, tra cui cancellare tutti i dati, testare se una key è presente, iterare attraverso i dati, ecc. La maggior parte delle volte, però, si utilizzerà semplicemente l'API "dictionary" standard per ottenere e impostare valori.

I frammenti di codice seguenti mostrano come si possono ottenere, impostare e eliminare alcuni dati con la key `my_car`, associati alla sessione corrente (browser).

> [!NOTE]
> Una delle grandi cose di Django è che non è necessario pensare ai meccanismi che legano la sessione alla richiesta attuale nella propria vista. Se fosse necessario utilizzare i frammenti sotto nella nostra vista, sapremmo che le informazioni su `my_car` sono associate solo al browser che ha inviato la richiesta corrente.

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

L'API offre anche una serie di altri metodi che vengono per lo più utilizzati per gestire il cookie della sessione associato. Ad esempio, ci sono metodi per testare che i cookie siano supportati nel browser del client, per impostare e verificare le date di scadenza dei cookie e per cancellare le sessioni scadute dal data store. Si possono trovare ulteriori informazioni sull'API completa in [Come usare le sessioni](https://docs.djangoproject.com/en/5.0/topics/http/sessions/) (documentazione Django).

## Salvare i dati delle sessioni

Per impostazione predefinita, Django salva solo nel database delle sessioni e invia il cookie della sessione al client quando la sessione è stata _modificata_ (assegnata) o _eliminata_. Se si stanno aggiornando alcuni dati utilizzando la loro key di sessione come mostrato nella sezione precedente, allora non è necessario preoccuparsi di questo! Ad esempio:

```python
# This is detected as an update to the session, so session data is saved.
request.session['my_car'] = 'mini'
```

Se si stanno aggiornando alcune informazioni _all'interno_ dei dati della sessione, Django non riconoscerà che è stata effettuata una modifica alla sessione e salverà i dati (ad esempio, se si cambiano i dati `wheels` all'interno dei dati `my_car`, come mostrato di seguito). In questo caso sarà necessario contrassegnare esplicitamente la sessione come modificata.

```python
# Session object not directly modified, only data within the session. Session changes not saved!
request.session['my_car']['wheels'] = 'alloy'

# Set session as modified to force data updates/cookie to be saved.
request.session.modified = True
```

> [!NOTE]
> Si può cambiare il comportamento in modo che il sito aggiorni il database/invii il cookie a ogni richiesta aggiungendo `SESSION_SAVE_EVERY_REQUEST = True` nelle impostazioni del progetto (**django-locallibrary-tutorial/locallibrary/settings.py**).

## Esempio semplice — ottenere i conteggi delle visite

Come semplice esempio del mondo reale aggiorneremo la nostra libreria per comunicare all'utente corrente quante volte ha visitato la pagina principale di _LocalLibrary_.

Aprire **/django-locallibrary-tutorial/catalog/views.py**, e aggiungere le righe che contengono `num_visits` in `index()` (come mostrato di seguito).

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

Qui, innanzitutto, otteniamo il valore della key di sessione `'num_visits'`, impostando il valore a 0 se non è stato precedentemente impostato. Ogni volta che viene ricevuta una richiesta, incrementiamo quindi il valore e lo memorizziamo nuovamente nella sessione (per la prossima volta che l'utente visita la pagina). La variabile `num_visits` viene quindi passata al template nella nostra variabile di contesto.

> [!NOTE]
> Potremmo anche verificare se i cookie sono supportati nel browser qui (vedere [Come usare le sessioni](https://docs.djangoproject.com/en/5.0/topics/http/sessions/) per esempi) o progettare la nostra interfaccia utente in modo che non importi se i cookie sono supportati o meno.

Aggiungere la riga mostrata in fondo al seguente blocco nel proprio template HTML principale (**/django-locallibrary-tutorial/catalog/templates/index.html**) in fondo alla sezione "Contenuto dinamico" per visualizzare la variabile di contesto `num_visits`.

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

Si noti che utilizziamo il tag template integrato di Django [pluralize](https://docs.djangoproject.com/en/5.0/ref/templates/builtins/#pluralize) per aggiungere una "s" quando la pagina è stata visitata più volt**e**.

Salvare le modifiche e riavviare il server di test. Ogni volta che si aggiorna la pagina, il numero dovrebbe aggiornarsi.

## Riepilogo

Ora sa quanto sia facile usare le sessioni per migliorare l'interazione con utenti _anonimi_.

Nei prossimi articoli spiegheremo il framework di autenticazione e autorizzazione (permessi) e le mostreremo come supportare gli account utente.

## Vedi anche

- [Come usare le sessioni](https://docs.djangoproject.com/en/5.0/topics/http/sessions/) (documentazione Django)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Generic_views", "Learn_web_development/Extensions/Server-side/Django/Authentication", "Learn_web_development/Extensions/Server-side/Django")}}
