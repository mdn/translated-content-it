---
title: Introduzione a Django
slug: Learn_web_development/Extensions/Server-side/Django/Introduction
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Extensions/Server-side/Django/development_environment", "Learn_web_development/Extensions/Server-side/Django")}}

In questo primo articolo su Django, rispondiamo alla domanda "Cos'è Django?" e forniamo una panoramica su ciò che rende speciale questo framework web.

Descriveremo le caratteristiche principali, inclusa parte della funzionalità avanzata che non avremo tempo di trattare in dettaglio in questo modulo. Inoltre, vi mostreremo alcuni dei principali blocchi costitutivi di un'applicazione Django (anche se a questo punto non avrete ancora un ambiente di sviluppo in cui testarla).

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Una comprensione generale della <a href="/it/docs/Learn_web_development/Extensions/Server-side/First_steps">programmazione di siti web lato server</a>, e in particolare delle meccaniche delle <a href="/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Client-Server_overview">interazioni client-server nei siti web</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Acquisire familiarità con cos'è Django, quali funzionalità fornisce e i principali elementi costitutivi di un'applicazione Django.
      </td>
    </tr>
  </tbody>
</table>

## Cos'è Django?

Django è un framework web in Python ad alto livello che consente uno sviluppo rapido di siti web sicuri e facili da mantenere. Sviluppato da programmatori esperti, Django si occupa di gran parte delle difficoltà dello sviluppo web, in modo che ci si possa concentrare sulla scrittura dell'applicazione senza dover reinventare la ruota. È gratuito e open source, ha una comunità fiorente e attiva, una documentazione eccellente e molte opzioni per il supporto gratuito e a pagamento.

Django aiuta a scrivere software che è:

- Completo
  - : Django segue la filosofia "Batteries included" e fornisce quasi tutto ciò che gli sviluppatori potrebbero desiderare di fare "out of the box". Poiché tutto ciò di cui si ha bisogno fa parte di un unico "prodotto", tutto funziona perfettamente insieme, segue principi di design coerenti e dispone di una documentazione ampia e [aggiornata](https://docs.djangoproject.com/en/stable/).
- Versatile

  - : Django può essere (ed è stato) utilizzato per costruire quasi qualsiasi tipo di sito web — dai sistemi di gestione dei contenuti e wiki, fino ai social network e siti di news. Può funzionare con qualsiasi framework lato client e può offrire contenuti in quasi qualsiasi formato (inclusi HTML, feed RSS, JSON e XML).

    Internamente, mentre offre scelte per quasi tutte le funzionalità che si potrebbero volere (ad esempio, diversi database popolari, motori di template, ecc.), può anche essere esteso per utilizzare altri componenti se necessario.

- Sicuro

  - : Django aiuta gli sviluppatori a evitare molti errori di sicurezza comuni fornendo un framework progettato per "fare le cose giuste" per proteggere automaticamente il sito web. Ad esempio, Django offre un modo sicuro per gestire gli account utente e le password, evitando errori comuni come mettere le informazioni di sessione nei cookie dove sono vulnerabili (invece i cookie contengono solo una chiave e i dati reali sono memorizzati nel database) o memorizzare direttamente le password anziché un hash della password.

    _Un hash della password è un valore di lunghezza fissa creato inviando la password tramite una [funzione di hash crittografica](https://en.wikipedia.org/wiki/Cryptographic_hash_function). Django può verificare se una password inserita è corretta inviandola tramite la funzione di hash e confrontando l'output con il valore di hash memorizzato. Tuttavia, data la natura "one-way" della funzione, anche se un valore hash memorizzato viene compromesso, è difficile per un attaccante risalire alla password originale._

    Django consente la protezione contro molte vulnerabilità per impostazione predefinita, inclusi SQL injection, cross-site scripting, cross-site request forgery e [clickjacking](/it/docs/Web/Security/Attacks/Clickjacking) (vedere [Sicurezza dei siti web](/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Website_security) per maggiori dettagli su tali attacchi).

- Scalabile
  - : Django utilizza un'architettura basata su componenti "[shared-nothing](https://en.wikipedia.org/wiki/Shared_nothing_architecture)" (ogni parte dell'architettura è indipendente dalle altre, e può quindi essere sostituita o modificata se necessario). Avere una chiara separazione tra le diverse parti significa che può scalare per aumentare il traffico aggiungendo hardware a qualsiasi livello: server di caching, server di database o server di applicazioni. Alcuni dei siti più trafficati hanno dimensionato con successo Django per soddisfare le loro esigenze (ad esempio, Instagram e Disqus, per nominarne solo due).
- Manutenibile
  - : Il codice Django è scritto utilizzando principi e modelli di progettazione che incoraggiano la creazione di codice manutenibile e riutilizzabile. In particolare, fa uso del principio Don't Repeat Yourself (DRY) in modo che non ci sia duplicazione inutile, riducendo la quantità di codice. Django promuove anche il raggruppamento delle funzionalità correlate in "applicazioni" riutilizzabili e, a un livello inferiore, raggruppa il codice correlato in moduli (lungo le linee del pattern {{Glossary("MVC", "Model View Controller (MVC)")}}).
- Portabile
  - : Django è scritto in Python, che funziona su molte piattaforme. Ciò significa che non siete vincolati a nessuna piattaforma server in particolare e potete eseguire le vostre applicazioni su molte varietà di Linux, Windows e macOS. Inoltre, Django è ben supportato da molti fornitori di hosting web, che spesso forniscono infrastrutture specifiche e documentazione per l'hosting di siti Django.

## Da dove proviene?

Django è stato inizialmente sviluppato tra il 2003 e il 2005 da un team web che era responsabile della creazione e della manutenzione di siti web di giornali. Dopo aver creato una serie di siti, il team ha iniziato a estrarre e riutilizzare gran parte del codice comune e dei modelli di design. Questo codice comune si è evoluto in un framework di sviluppo web generico, che è stato rilasciato come open source con il nome "Django" nel luglio 2005.

Django ha continuato a crescere e migliorare, dalla sua prima release milestone (1.0) nel settembre 2008 fino alla versione 5.0 alla fine del 2023. Ogni versione ha aggiunto nuove funzionalità e correzioni di bug, che vanno dal supporto per nuovi tipi di database, motori di template e caching, fino all'aggiunta di funzioni e classi di vista "generiche" (che riducono la quantità di codice che gli sviluppatori devono scrivere per una serie di compiti di programmazione).

> [!NOTE]
> Consulti le [note di rilascio](https://docs.djangoproject.com/en/stable/releases/) sul sito web di Django per vedere cosa è cambiato nelle versioni recenti e quanto lavoro viene fatto per migliorare Django.

Django è ora un progetto open source vibrante e collaborativo, con molte migliaia di utenti e contributori. Anche se ha ancora alcune caratteristiche che riflettono la sua origine, Django si è evoluto in un framework versatile capace di sviluppare qualsiasi tipo di sito web.

## Quanto è popolare Django?

Non esiste una misurazione facilmente disponibile e definitiva della popolarità dei framework server-side (anche se si può stimare la popolarità utilizzando meccanismi come contare il numero di progetti su GitHub e le domande su Stack Overflow per ciascuna piattaforma). Una domanda migliore è se Django sia "abbastanza popolare" da evitare i problemi delle piattaforme poco popolari. Continua ad evolversi? Potete ottenere aiuto se necessario? C'è un'opportunità di lavoro retribuito se impara Django?

Basandosi sul numero di siti di alto profilo che utilizzano Django, sul numero di persone che contribuiscono al codice e sul numero di persone che forniscono supporto sia gratuito che a pagamento, sì, Django è un framework popolare!

I siti di alto profilo che utilizzano Django includono: Disqus, Instagram, Knight Foundation, MacArthur Foundation, Mozilla, National Geographic, Open Knowledge Foundation, Pinterest e Open Stack (fonte: [pagina di panoramica di Django](https://www.djangoproject.com/start/overview/)).

## Django è "opinionated"?

I framework web spesso si descrivono come "opinionated" o "unopinionated".

I framework "opinionated" sono quelli con opinioni sul "modo giusto" di gestire un determinato compito. Spesso supportano uno sviluppo rapido _in un particolare dominio_ (risolvendo problemi di un particolare tipo) perché il modo giusto di fare qualsiasi cosa è solitamente ben compreso e ben documentato. Tuttavia, possono essere meno flessibili nel risolvere problemi al di fuori del loro dominio principale e tendono a offrire meno scelte per quali componenti e approcci possono essere utilizzati.

I framework "unopinionated", al contrario, hanno molte meno restrizioni sul modo migliore per combinare i componenti per raggiungere un obiettivo o addirittura su quali componenti dovrebbero essere utilizzati. Rendono più facile per gli sviluppatori utilizzare gli strumenti più adatti per completare un determinato compito, sebbene al costo di dover trovare questi componenti da soli.

Django è "parzialmente opinionated" e pertanto offre il "meglio di entrambi i mondi". Fornisce una serie di componenti per gestire la maggior parte dei compiti di sviluppo web e uno (o due) modi preferiti per utilizzarli. Tuttavia, l'architettura decoupled di Django significa che si può generalmente scegliere tra un numero di opzioni diverse o aggiungere supporto per altre completamente nuove se lo si desidera.

## Che aspetto ha il codice Django?

In un sito web tradizionale basato sui dati, un'applicazione web attende richieste HTTP dal browser web (o da un altro client). Quando viene ricevuta una richiesta, l'applicazione determina ciò che è necessario in base all'URL e possibilmente alle informazioni nei dati `POST` o `GET`. A seconda di ciò che è richiesto, potrebbe quindi leggere o scrivere informazioni da un database o eseguire altre attività necessarie per soddisfare la richiesta. L'applicazione quindi restituisce una risposta al browser web, spesso creando dinamicamente una pagina HTML per il browser da visualizzare inserendo i dati recuperati nei segnaposto in un template HTML.

Le applicazioni web Django generalmente raggruppano il codice che gestisce ciascuno di questi passaggi in file separati:

![Django - file per viste, modelli, URL, template](basic-django.png)

- **URLs:** Sebbene sia possibile elaborare richieste da ogni singolo URL tramite una singola funzione, è molto più manutenibile scrivere una funzione di vista separata per gestire ciascuna risorsa. Un mapper di URL viene utilizzato per reindirizzare le richieste HTTP alla vista appropriata in base all'URL della richiesta. Il mapper URL può anche corrispondere a particolari schemi di stringhe o cifre che appaiono in un URL e passarli a una funzione di vista come dati.
- **View:** Una vista è una funzione gestore di richieste, che riceve richieste HTTP e restituisce risposte HTTP. Le viste accedono ai dati necessari per soddisfare le richieste tramite _models_ e delegano la formattazione della risposta ai _temlates_.
- **Models:** I modelli sono oggetti Python che definiscono la struttura dei dati di un'applicazione e forniscono meccanismi per gestire (aggiungere, modificare, eliminare) e interrogare i record nel database.
- **Templates:** Un template è un file di testo che definisce la struttura o il layout di un file (come una pagina HTML), con segnaposto utilizzati per rappresentare il contenuto effettivo. Una _view_ può creare dinamicamente una pagina HTML utilizzando un template HTML, popolandolo con dati da un _model_. Un template può essere utilizzato per definire la struttura di qualsiasi tipo di file; non deve essere per forza un HTML!

> [!NOTE]
> Django si riferisce a questa organizzazione come architettura "Model View Template (MVT)". Ha molte somiglianze con l'architettura {{Glossary("MVC", "Model View Controller")}} più familiare.

Le sezioni seguenti vi daranno un'idea di come appaiono queste principali parti di un'app Django (approfondiremo più nel dettaglio più avanti nel corso, una volta che avremo impostato un ambiente di sviluppo).

### Invio della richiesta alla vista corretta (urls.py)

Un mapper di URL è solitamente memorizzato in un file chiamato **urls.py**.
Nell'esempio seguente, il mapper (`urlpatterns`) definisce un elenco di mappature tra _routes_ (schemi URL specifici) e funzioni di vista corrispondenti.
Se una richiesta HTTP viene ricevuta con un URL che corrisponde a uno schema specificato, la funzione di vista associata verrà chiamata e la richiesta passata.

```python
urlpatterns = [
    path('admin/', admin.site.urls),
    path('book/<int:id>/', views.book_detail, name='book_detail'),
    path('catalog/', include('catalog.urls')),
    re_path(r'^([0-9]+)/$', views.best),
]
```

L'oggetto `urlpatterns` è un elenco di funzioni `path()` e/o `re_path()` (gli elenchi Python sono definiti utilizzando parentesi quadre, dove gli elementi sono separati da virgole e possono avere una [virgola finale opzionale](https://docs.python.org/3/faq/design.html#why-does-python-allow-commas-at-the-end-of-lists-and-tuples). Ad esempio: `[item1, item2, item3,]`).

Il primo argomento per entrambi i metodi è una rotta (schema) che verrà corrisposta. Il metodo `path()` utilizza parentesi angolari per definire parti di un URL che verranno acquisite e passate alla funzione di vista come argomenti denominati. La funzione `re_path()` utilizza un approccio flessibile di corrispondenza di schemi noto come espressione regolare. Ne parleremo in un articolo successivo!

Il secondo argomento è un'altra funzione che verrà chiamata quando lo schema viene corrisposto. La notazione `views.book_detail` indica che la funzione si chiama `book_detail()` e può essere trovata in un modulo chiamato `views` (cioè all'interno di un file chiamato `views.py`).

### Gestione della richiesta (views.py)

Le viste sono il cuore dell'applicazione web, ricevendo richieste HTTP dai web client e restituendo risposte HTTP. Nel mezzo, orchestrano le altre risorse del framework per accedere ai database, rendere i template, ecc.

L'esempio seguente mostra una funzione di vista minimamente `index()`, che potrebbe essere stata chiamata dal nostro mapper URL nella sezione precedente. Come tutte le funzioni di vista, riceve un oggetto `HttpRequest` come parametro (`request`) e restituisce un oggetto `HttpResponse`. In questo caso non facciamo nulla con la richiesta, e la nostra risposta restituisce una stringa codificata in modo statico. Vi mostreremo una richiesta che fa qualcosa di più interessante in una sezione successiva.

```python
# filename: views.py (Django view functions)

from django.http import HttpResponse

def index(request):
    # Get an HttpRequest - the request parameter
    # perform operations using information from the request.
    # Return HttpResponse
    return HttpResponse('Hello from Django!')
```

> [!NOTE]
> Un po' di Python:
>
> - I [moduli Python](https://docs.python.org/3/tutorial/modules.html) sono "librerie" di funzioni, memorizzate in file separati, che potremmo voler utilizzare nel nostro codice. Qui importiamo solo l'oggetto `HttpResponse` dal modulo `django.http` in modo da poterlo usare nella nostra vista: `from django.http import HttpResponse`. Ci sono altri modi per importare alcuni o tutti gli oggetti da un modulo.
> - Le funzioni vengono dichiarate usando la parola chiave `def` come mostrato sopra, con i parametri denominati elencati tra parentesi dopo il nome della funzione; l'intera riga termina con due punti. Notate come le righe successive sono tutte **indented**. L'indentazione è importante, dato che specifica che le righe di codice sono all'interno di quel particolare blocco (l'indentazione obbligatoria è una caratteristica chiave di Python, ed è una delle ragioni per cui il codice Python è così facile da leggere).

Le viste sono solitamente memorizzate in un file chiamato **views.py**.

### Definizione dei modelli di dati (models.py)

Le applicazioni web Django gestiscono e interrogano i dati tramite oggetti Python chiamati modelli. I modelli definiscono la struttura dei dati memorizzati, inclusi i tipi di campi e possibilmente anche la loro dimensione massima, i valori predefiniti, le opzioni delle liste di selezione, il testo di aiuto per la documentazione, il testo dell'etichetta per i moduli, ecc. La definizione del modello è indipendente dal database sottostante — è possibile scegliere uno di diversi database come parte delle impostazioni del progetto. Una volta scelto il database da utilizzare, non è necessario parlare direttamente con esso — basta scrivere la struttura del modello e altro codice, e Django gestisce tutto il "lavoro sporco" di comunicazione con il database.

Il frammento di codice seguente mostra un modello Django molto semplice per un oggetto `Team`. La classe `Team` deriva dalla classe Django `models.Model`. Definisce il nome della squadra e il livello della squadra come campi carattere e specifica un numero massimo di caratteri da memorizzare per ciascun record. Il `team_level` può essere uno di diversi valori, quindi lo definiamo come campo di scelta e forniamo una mappatura tra le scelte da visualizzare e i dati da memorizzare, insieme a un valore predefinito.

```python
# filename: models.py

from django.db import models

class Team(models.Model):
    team_name = models.CharField(max_length=40)

    TEAM_LEVELS = (
        ('U09', 'Under 09s'),
        ('U10', 'Under 10s'),
        ('U11', 'Under 11s'),
        # …
        # list other team levels
    )
    team_level = models.CharField(max_length=3, choices=TEAM_LEVELS, default='U11')
```

> [!NOTE]
> Un po' di Python:
>
> Python supporta la "programmazione orientata agli oggetti", uno stile di programmazione in cui organizziamo il nostro codice in oggetti, che includono dati correlati e funzioni per operare su tali dati. Gli oggetti possono anche ereditare/estendere/derivare da altri oggetti, consentendo la condivisione di comportamenti comuni tra oggetti correlati. In Python utilizziamo la parola chiave `class` per definire il "progetto" di un oggetto. Possiamo creare più _istanze_ specifiche del tipo di oggetto basate sul modello nella classe.
>
> Ad esempio, qui abbiamo una classe `Team`, che deriva dalla classe `Model`. Ciò significa che è un modello e conterrà tutti i metodi di un modello, ma possiamo anche dargli funzionalità speciali proprie. Nel nostro modello definiamo i campi di cui il nostro database avrà bisogno per memorizzare i nostri dati, assegnando loro nomi specifici. Django utilizza queste definizioni, inclusi i nomi dei campi, per creare il database sottostante.

### Interrogare i dati (views.py)

Il modello Django fornisce un'interfaccia API di query semplice per cercare nel database associato. Questa può confrontare un numero di campi in una volta utilizzando criteri diversi (ad es., esatto, case-insensitive, maggiore di, ecc.) e può supportare dichiarazioni complesse (ad esempio, è possibile specificare una ricerca su squadre U11 che hanno un nome di squadra che inizia con "Fr" o termina con "al").

Il frammento di codice mostra una funzione di vista (gestione risorse) per visualizzare tutte le nostre squadre U09. La riga `list_teams = Team.objects.filter(team_level__exact="U09")` mostra come possiamo usare l'API di query del modello per filtrare tutti i record dove il campo `team_level` ha esattamente il testo `U09` (notate come questo criterio è passato alla funzione `filter()` come argomento, con il nome del campo e il tipo di corrispondenza separati da un doppio underscore: **`team_level__exact`**).

```python
## filename: views.py

from django.shortcuts import render
from .models import Team

def index(request):
    list_teams = Team.objects.filter(team_level__exact="U09")
    context = {'youngest_teams': list_teams}
    return render(request, '/best/index.html', context)
```

Questa funzione utilizza la funzione `render()` per creare l'`HttpResponse` che viene inviato al browser. Questa funzione è una scorciatoia; crea un file HTML combinando un template HTML specificato e alcuni dati da inserire nel template (forniti nella variabile denominata `context`). Nella sezione successiva mostriamo come il template ha i dati inseriti per creare l'HTML.

### Rendering dei dati (template HTML)

I sistemi di template consentono di specificare la struttura di un documento di output, usando segnaposto per i dati che verranno riempiti quando una pagina viene generata. I template sono spesso utilizzati per creare HTML, ma possono anche creare altri tipi di documenti. Django supporta sia il suo sistema di template nativo che un'altra libreria Python popolare chiamata Jinja2 standard (può anche essere fatto per supportare altri sistemi se necessario).

Il frammento di codice mostra come potrebbe apparire il template HTML chiamato dalla funzione `render()` nella sezione precedente. Questo template è stato scritto presumendo che avrà accesso a una variabile di elenco chiamata `youngest_teams` quando viene renderizzato (questa è contenuta nella variabile `context` all'interno della funzione `render()` sopra). All'interno dello scheletro HTML abbiamo un'espressione che prima verifica se esiste la variabile `youngest_teams`, e poi la itera in un ciclo `for`. Ad ogni iterazione il template visualizza il valore `team_name` di ciascuna squadra in un elemento `{{htmlelement("li")}}`.

```django
## filename: best/templates/best/index.html

<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Home page</title>
</head>
<body>
  {% if youngest_teams %}
    <ul>
      {% for team in youngest_teams %}
        <li>\{{ team.team_name }}</li>
      {% endfor %}
    </ul>
  {% else %}
    <p>No teams are available.</p>
  {% endif %}
</body>
</html>
```

## Cos'altro si può fare?

Le sezioni precedenti mostrano le funzionalità principali che si utilizzeranno in quasi ogni applicazione web: mappatura degli URL, viste, modelli e template. Solo alcune delle altre cose fornite da Django includono:

- **Moduli**: i moduli HTML sono utilizzati per raccogliere dati utente per il trattamento sul server. Django semplifica la creazione, la validazione e l'elaborazione dei moduli.
- **Autenticazione e permessi utente**: Django include un robusto sistema di autenticazione e permessi utente che è stato costruito pensando alla sicurezza.
- **Caching**: creare contenuti dinamicamente è molto più intensivo dal punto di vista computazionale (e lento) che servire contenuti statici. Django fornisce un caching flessibile in modo che si possa memorizzare tutto o parte di una pagina renderizzata in modo che non venga ri-renderizzata se non necessario.
- **Sito di amministrazione**: il sito di amministrazione di Django è incluso per default quando si crea un'app utilizzando lo scheletro base. Rende immensamente facile fornire una pagina di amministrazione per gli amministratori del sito per creare, modificare e visualizzare qualsiasi modello di dati nel sito.
- **Serializzazione dei dati**: Django rende facile serializzare e servire i propri dati come XML o JSON. Questo può essere utile quando si crea un servizio web (un sito web che serve solamente dati da consumare da altre applicazioni o siti, e non visualizza niente di suo), o quando si crea un sito web in cui il codice lato client gestisce tutto il rendering dei dati.

## Sommario

Congratulazioni, ha completato il primo passo nel suo viaggio con Django! Ora dovrebbe comprendere i principali vantaggi di Django, un po' della sua storia e grossomodo cosa potrebbe sembrare ciascuna delle parti principali di un'app Django. Dovrebbe anche aver appreso alcune cose sul linguaggio di programmazione Python, inclusa la sintassi per elenchi, funzioni e classi.

Ha già visto del vero codice Django sopra, ma a differenza del codice lato client, è necessario impostare un ambiente di sviluppo per eseguirlo. Questo è il nostro prossimo passo.

{{NextMenu("Learn_web_development/Extensions/Server-side/Django/development_environment", "Learn_web_development/Extensions/Server-side/Django")}}
