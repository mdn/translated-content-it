---
title: Introduzione a Django
slug: Learn_web_development/Extensions/Server-side/Django/Introduction
l10n:
  sourceCommit: a4fcf79b60471db6f148fa4ba36f2cdeafbbeb70
---

{{NextMenu("Learn_web_development/Extensions/Server-side/Django/development_environment", "Learn_web_development/Extensions/Server-side/Django")}}

In questo primo articolo su Django, rispondiamo alla domanda "Che cos'è Django?" e forniamo una panoramica di ciò che rende speciale questo framework web.

Illustreremo le funzionalità principali, incluse alcune funzionalità avanzate che non sarà possibile trattare in dettaglio in questo modulo. Mostreremo inoltre alcuni dei principali elementi costitutivi di un'applicazione Django (anche se a questo punto non è ancora disponibile un ambiente di sviluppo in cui testarli).

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Una comprensione generale della <a href="/it/docs/Learn_web_development/Extensions/Server-side/First_steps">programmazione di siti web lato server</a>, e in particolare dei meccanismi delle <a href="/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Client-Server_overview">interazioni client-server nei siti web</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Acquisire familiarità con Django, le funzionalità che fornisce e i principali elementi costitutivi di un'applicazione Django.
      </td>
    </tr>
  </tbody>
</table>

## Che cos'è Django?

Django è un framework web Python di alto livello che consente lo sviluppo rapido di siti web sicuri e manutenibili. Creato da sviluppatori esperti, Django si occupa di gran parte delle complessità dello sviluppo web, consentendo di concentrarsi sulla scrittura dell'app senza dover reinventare la ruota. È gratuito e open source, dispone di una comunità attiva e fiorente, di ottima documentazione e di molte opzioni di supporto gratuite e a pagamento.

Django aiuta a scrivere software che sia:

- Completo
  - : Django segue la filosofia "Batteries included" e fornisce quasi tutto ciò che gli sviluppatori potrebbero voler fare "out of the box". Poiché tutto ciò che serve fa parte dello stesso "prodotto", ogni componente funziona perfettamente insieme agli altri, segue principi di progettazione coerenti e dispone di una documentazione estesa e [aggiornata](https://docs.djangoproject.com/en/stable/).
- Versatile
  - : Django può essere (ed è stato) utilizzato per creare quasi ogni tipo di sito web, dai sistemi di gestione dei contenuti e wiki fino ai social network e ai siti di notizie. Può funzionare con qualsiasi framework lato client e può fornire contenuti in quasi ogni formato, inclusi HTML, feed RSS, JSON e XML.

    Internamente, pur offrendo opzioni per quasi ogni funzionalità desiderata, ad esempio diversi database ed engine di template diffusi, può anche essere esteso per utilizzare altri componenti se necessario.

- Sicuro
  - : Django aiuta gli sviluppatori a evitare molti errori di sicurezza comuni fornendo un framework progettato per "fare le cose nel modo giusto" e proteggere automaticamente il sito web. Ad esempio, Django fornisce un modo sicuro per gestire account utente e password, evitando errori comuni come inserire informazioni di sessione nei cookie, dove sono vulnerabili (i cookie contengono invece soltanto una chiave, mentre i dati effettivi sono archiviati nel database), oppure memorizzare direttamente le password anziché il relativo hash.

    _Un hash della password è un valore di lunghezza fissa creato passando la password attraverso una [funzione hash crittografica](https://en.wikipedia.org/wiki/Cryptographic_hash_function). Django può verificare se una password immessa è corretta passandola attraverso la funzione hash e confrontando l'output con il valore hash memorizzato. Tuttavia, a causa della natura "a senso unico" della funzione, anche se un valore hash memorizzato viene compromesso, per un attaccante è difficile risalire alla password originale._

    Django abilita per impostazione predefinita la protezione contro molte vulnerabilità, incluse SQL injection, cross-site scripting, cross-site request forgery e [clickjacking](/it/docs/Web/Security/Attacks/Clickjacking) (vedere [Sicurezza dei siti web](/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Website_security) per maggiori dettagli su tali attacchi).

- Scalabile
  - : Django utilizza un'architettura "[shared-nothing](https://en.wikipedia.org/wiki/Shared_nothing_architecture)" basata su componenti (ogni parte dell'architettura è indipendente dalle altre e può quindi essere sostituita o modificata se necessario). La chiara separazione tra le diverse parti consente di gestire l'aumento del traffico aggiungendo hardware a qualsiasi livello: server di caching, server di database o server delle applicazioni. Alcuni dei siti più trafficati hanno scalato con successo Django per soddisfare le proprie esigenze, ad esempio Instagram e Disqus.
- Manutenibile
  - : Il codice Django è scritto utilizzando principi e pattern di progettazione che favoriscono la creazione di codice manutenibile e riutilizzabile. In particolare, utilizza il principio Don't Repeat Yourself (DRY), evitando duplicazioni non necessarie e riducendo la quantità di codice. Django promuove inoltre il raggruppamento di funzionalità correlate in "applicazioni" riutilizzabili e, a un livello inferiore, raggruppa il codice correlato in moduli, seguendo il pattern {{Glossary("MVC", "Model View Controller (MVC)")}}.
- Portabile
  - : Django è scritto in Python, che viene eseguito su molte piattaforme. Ciò significa che non è vincolato a una piattaforma server particolare ed è possibile eseguire le applicazioni su varie distribuzioni Linux, Windows e macOS. Inoltre, Django è ben supportato da molti provider di hosting web, che spesso forniscono infrastrutture e documentazione specifiche per ospitare siti Django.

## Da dove proviene?

Django è stato sviluppato inizialmente tra il 2003 e il 2005 da un team web responsabile della creazione e della manutenzione di siti web di giornali. Dopo aver creato diversi siti, il team iniziò a estrarre e riutilizzare molto codice comune e numerosi pattern di progettazione. Questo codice comune si è evoluto in un framework generico per lo sviluppo web, pubblicato come progetto open source con il nome "Django" nel luglio 2005.

Django ha continuato a crescere e migliorare, dalla prima versione importante (1.0) nel settembre 2008 fino alla versione 5.0 alla fine del 2023. Ogni rilascio ha aggiunto nuove funzionalità e correzioni di bug, dal supporto per nuovi tipi di database, engine di template e caching fino all'aggiunta di funzioni e classi di view "generiche", che riducono la quantità di codice che gli sviluppatori devono scrivere per varie attività di programmazione.

> [!NOTE]
> Consultare le [note di rilascio](https://docs.djangoproject.com/en/stable/releases/) sul sito web di Django per vedere cosa è cambiato nelle versioni recenti e quanto lavoro viene dedicato al miglioramento di Django.

Django è oggi un progetto open source collaborativo e fiorente, con molte migliaia di utenti e collaboratori. Sebbene conservi ancora alcune caratteristiche che riflettono la sua origine, Django si è evoluto in un framework versatile capace di sviluppare qualsiasi tipo di sito web.

## Quanto è popolare Django?

Non esiste una misura definitiva e facilmente disponibile della popolarità dei framework lato server, anche se è possibile stimarla tramite meccanismi come il conteggio del numero di progetti GitHub e di domande su Stack Overflow per ciascuna piattaforma. Una domanda migliore è se Django sia "abbastanza popolare" da evitare i problemi delle piattaforme poco diffuse. Continua a evolversi? È possibile ottenere aiuto quando serve? Imparando Django, esistono opportunità di lavoro retribuito?

In base al numero di siti importanti che utilizzano Django, al numero di persone che contribuiscono alla codebase e al numero di persone che forniscono supporto gratuito e a pagamento, la risposta è sì: Django è un framework popolare.

Tra i siti importanti che utilizzano Django figurano: Disqus, Instagram, Knight Foundation, MacArthur Foundation, Mozilla, National Geographic, Open Knowledge Foundation, Pinterest e Open Stack (fonte: [pagina panoramica di Django](https://www.djangoproject.com/start/overview/)).

## Django è opinionated?

I framework web spesso si definiscono "opinionated" oppure "unopinionated".

I framework opinionated hanno opinioni sul modo "giusto" di gestire una determinata attività. Spesso supportano lo sviluppo rapido _in un dominio particolare_ — risolvendo problemi di un tipo specifico — perché il modo giusto di fare qualcosa è in genere ben compreso e ben documentato. Tuttavia, possono essere meno flessibili nella risoluzione di problemi al di fuori del loro dominio principale e tendono a offrire meno possibilità di scelta riguardo ai componenti e agli approcci utilizzabili.

I framework unopinionated, al contrario, hanno molte meno restrizioni sul modo migliore per collegare insieme i componenti per raggiungere un obiettivo, o persino sui componenti da utilizzare. Rendono più semplice per gli sviluppatori usare gli strumenti più adatti a completare una determinata attività, a costo però di dover trovare autonomamente tali componenti.

Django è "in parte opinionated" e offre quindi il "meglio di entrambi i mondi". Fornisce un insieme di componenti per gestire la maggior parte delle attività di sviluppo web e uno, o due, modi preferiti per utilizzarli. Tuttavia, l'architettura disaccoppiata di Django permette generalmente di scegliere tra diverse opzioni oppure di aggiungere supporto per opzioni completamente nuove, se desiderato.

## Che aspetto ha il codice Django?

In un sito web tradizionale basato sui dati, un'applicazione web attende richieste HTTP provenienti dal browser web, o da un altro client. Quando riceve una richiesta, l'applicazione determina ciò che serve in base all'URL e, possibilmente, alle informazioni nei dati `POST` o `GET`. A seconda di ciò che è richiesto, può quindi leggere o scrivere informazioni in un database oppure eseguire altre attività necessarie per soddisfare la richiesta. L'applicazione restituisce poi una risposta al browser web, creando spesso dinamicamente una pagina HTML da visualizzare nel browser mediante l'inserimento dei dati recuperati nei segnaposto di un template HTML.

Le applicazioni web Django in genere raggruppano il codice che gestisce ciascuno di questi passaggi in file separati:

![Django - file per view, model, URL e template](basic-django.png)

- **URL:** Sebbene sia possibile elaborare le richieste provenienti da ogni singolo URL tramite una sola funzione, è molto più manutenibile scrivere una funzione di view separata per gestire ciascuna risorsa. Un URL mapper viene utilizzato per reindirizzare le richieste HTTP alla view appropriata in base all'URL della richiesta. L'URL mapper può anche individuare particolari pattern di stringhe o cifre presenti in un URL e passarli come dati a una funzione di view.
- **View:** Una view è una funzione di gestione delle richieste, che riceve richieste HTTP e restituisce risposte HTTP. Le view accedono ai dati necessari per soddisfare le richieste tramite i _model_ e delegano la formattazione della risposta ai _template_.
- **Model:** I model sono oggetti Python che definiscono la struttura dei dati di un'applicazione e forniscono meccanismi per gestire — aggiungere, modificare, eliminare — e interrogare i record nel database.
- **Template:** Un template è un file di testo che definisce la struttura o il layout di un file, ad esempio una pagina HTML, con segnaposto utilizzati per rappresentare il contenuto effettivo. Una _view_ può creare dinamicamente una pagina HTML usando un template HTML e popolandola con dati provenienti da un _model_. Un template può essere utilizzato per definire la struttura di qualsiasi tipo di file; non deve necessariamente essere HTML.

> [!NOTE]
> Django definisce questa organizzazione come architettura "Model View Template (MVT)". Presenta molte somiglianze con la più nota architettura {{Glossary("MVC", "Model View Controller")}}.

Le sezioni seguenti forniscono un'idea dell'aspetto di queste parti principali di un'app Django. I dettagli verranno approfonditi più avanti nel corso, dopo aver configurato un ambiente di sviluppo.

### Inviare la richiesta alla view corretta (urls.py)

Un URL mapper viene solitamente memorizzato in un file denominato **urls.py**.
Nell'esempio seguente, il mapper (`urlpatterns`) definisce un elenco di associazioni tra _route_ (specifici _pattern_ URL) e le corrispondenti funzioni di view.
Se viene ricevuta una HTTP Request con un URL corrispondente a un pattern specificato, viene chiamata la funzione di view associata e le viene passata la richiesta.

```python
urlpatterns = [
    path('admin/', admin.site.urls),
    path('book/<int:id>/', views.book_detail, name='book_detail'),
    path('catalog/', include('catalog.urls')),
    re_path(r'^([0-9]+)/$', views.best),
]
```

L'oggetto `urlpatterns` è un elenco di funzioni `path()` e/o `re_path()` (gli elenchi Python vengono definiti usando parentesi quadre, in cui gli elementi sono separati da virgole e possono avere una [virgola finale facoltativa](https://docs.python.org/3/faq/design.html#why-does-python-allow-commas-at-the-end-of-lists-and-tuples). Ad esempio: `[item1, item2, item3,]`).

Il primo argomento di entrambi i metodi è una route, ovvero un pattern, che verrà confrontata. Il metodo `path()` utilizza parentesi angolari per definire parti di un URL che verranno acquisite e passate alla funzione di view come argomenti con nome. La funzione `re_path()` utilizza un approccio flessibile di corrispondenza dei pattern noto come espressione regolare. Questi aspetti verranno trattati in un articolo successivo.

Il secondo argomento è un'altra funzione che verrà chiamata quando il pattern corrisponde. La notazione `views.book_detail` indica che la funzione si chiama `book_detail()` e può essere trovata in un modulo denominato `views`, cioè all'interno di un file chiamato `views.py`.

### Gestire la richiesta (views.py)

Le view sono il cuore dell'applicazione web: ricevono richieste HTTP dai client web e restituiscono risposte HTTP. Nel frattempo, coordinano le altre risorse del framework per accedere ai database, renderizzare i template e così via.

L'esempio seguente mostra una funzione di view minima, `index()`, che potrebbe essere stata chiamata dall'URL mapper della sezione precedente. Come tutte le funzioni di view, riceve un oggetto `HttpRequest` come parametro (`request`) e restituisce un oggetto `HttpResponse`. In questo caso non viene eseguita alcuna operazione con la richiesta e la risposta restituisce una stringa hard-coded. Una richiesta più interessante verrà mostrata in una sezione successiva.

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
> - I [moduli Python](https://docs.python.org/3/tutorial/modules.html) sono "librerie" di funzioni, memorizzate in file separati, che potrebbero essere utilizzate nel codice. Qui viene importato solo l'oggetto `HttpResponse` dal modulo `django.http`, per poterlo utilizzare nella view: `from django.http import HttpResponse`. Esistono altri modi per importare alcuni o tutti gli oggetti da un modulo.
> - Le funzioni vengono dichiarate usando la parola chiave `def`, come mostrato sopra, con parametri denominati elencati tra parentesi dopo il nome della funzione; l'intera riga termina con due punti. Si noti che le righe successive sono tutte **indentate**. L'indentazione è importante poiché specifica che le righe di codice appartengono a quel particolare blocco. L'indentazione obbligatoria è una caratteristica fondamentale di Python e uno dei motivi per cui il codice Python è così facile da leggere.

Le view vengono generalmente memorizzate in un file denominato **views.py**.

### Definire i modelli di dati (models.py)

Le applicazioni web Django gestiscono e interrogano i dati tramite oggetti Python denominati model. I model definiscono la struttura dei dati memorizzati, inclusi i _tipi_ di campo e, potenzialmente, anche la loro dimensione massima, i valori predefiniti, le opzioni degli elenchi di selezione, il testo di aiuto per la documentazione, il testo delle etichette per i moduli e così via. La definizione del model è indipendente dal database sottostante: è possibile scegliere uno tra diversi database nelle impostazioni del progetto. Dopo aver scelto il database da usare, non è necessario interagire direttamente con esso: basta scrivere la struttura del model e altro codice, e Django gestisce tutto il "lavoro sporco" della comunicazione con il database.

Il frammento di codice seguente mostra un model Django molto semplice per un oggetto `Team`. La classe `Team` deriva dalla classe Django `models.Model`. Definisce il nome e il livello della squadra come campi di caratteri e specifica un numero massimo di caratteri da memorizzare per ogni record. `team_level` può assumere uno tra diversi valori, quindi viene definito come campo di scelta e viene fornita una mappatura tra le scelte da visualizzare e i dati da memorizzare, insieme a un valore predefinito.

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
> Python supporta la "programmazione orientata agli oggetti", uno stile di programmazione in cui il codice viene organizzato in oggetti, che includono dati correlati e funzioni per operare su tali dati. Gli oggetti possono anche ereditare/estendere/derivare da altri oggetti, consentendo di condividere comportamenti comuni tra oggetti correlati. In Python si usa la parola chiave `class` per definire il "progetto" di un oggetto. È possibile creare più _istanze_ specifiche del tipo di oggetto basandosi sul modello nella classe.
>
> Ad esempio, qui è presente una classe `Team`, che deriva dalla classe `Model`. Ciò significa che è un model e conterrà tutti i metodi di un model, ma è anche possibile assegnarle caratteristiche specializzate proprie. Nel model vengono definiti i campi di cui il database avrà bisogno per memorizzare i dati, assegnando loro nomi specifici. Django utilizza queste definizioni, inclusi i nomi dei campi, per creare il database sottostante.

### Interrogare i dati (views.py)

Il model Django fornisce una semplice API di query per cercare nel database associato. Può effettuare corrispondenze su più campi contemporaneamente usando criteri diversi, ad esempio esatto, senza distinzione tra maiuscole e minuscole, maggiore di e così via, e può supportare istruzioni complesse. Ad esempio, è possibile specificare una ricerca sulle squadre U11 con un nome della squadra che inizia con "Fr" o termina con "al".

Il frammento di codice mostra una funzione di view, ovvero un gestore di risorse, per visualizzare tutte le squadre U09. La riga `list_teams = Team.objects.filter(team_level__exact="U09")` mostra come utilizzare l'API di query del model per filtrare tutti i record in cui il campo `team_level` contiene esattamente il testo `U09`. Si noti come questo criterio venga passato alla funzione `filter()` come argomento, con il nome del campo e il tipo di corrispondenza separati da un doppio carattere di sottolineatura: **`team_level__exact`**.

```python
## filename: views.py

from django.shortcuts import render
from .models import Team

def index(request):
    list_teams = Team.objects.filter(team_level__exact="U09")
    context = {'youngest_teams': list_teams}
    return render(request, '/best/index.html', context)
```

Questa funzione utilizza la funzione `render()` per creare l'oggetto `HttpResponse` inviato al browser. Questa funzione è una _scorciatoia_: crea un file HTML combinando un template HTML specificato e alcuni dati da inserire nel template, forniti nella variabile denominata `context`. Nella sezione successiva viene mostrato come i dati vengano inseriti nel template per creare l'HTML.

### Renderizzare i dati (template HTML)

I sistemi di template consentono di specificare la struttura di un documento di output, utilizzando segnaposto per i dati che verranno riempiti quando viene generata una pagina. I template vengono spesso usati per creare HTML, ma possono anche creare altri tipi di documento. Django supporta immediatamente sia il proprio sistema di templating nativo sia un'altra popolare libreria Python chiamata Jinja2. Se necessario, può essere configurato anche per supportare altri sistemi.

Il frammento di codice mostra il possibile aspetto del template HTML chiamato dalla funzione `render()` nella sezione precedente. Questo template è stato scritto presumendo che, al momento del rendering, abbia accesso a una variabile elenco denominata `youngest_teams`, contenuta nella variabile `context` all'interno della funzione `render()` precedente. All'interno della struttura HTML è presente un'espressione che verifica dapprima l'esistenza della variabile `youngest_teams`, quindi la itera in un ciclo `for`. A ogni iterazione, il template visualizza il valore `team_name` di ciascuna squadra in un elemento {{htmlelement("li")}}.

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

## Cos'altro è possibile fare?

Le sezioni precedenti mostrano le funzionalità principali utilizzate in quasi ogni applicazione web: mappatura degli URL, view, model e template. Tra le altre funzionalità fornite da Django figurano:

- **Moduli**: i moduli HTML vengono utilizzati per raccogliere dati degli utenti da elaborare sul server. Django semplifica la creazione, la convalida e l'elaborazione dei moduli.
- **Autenticazione e autorizzazioni degli utenti**: Django include un solido sistema di autenticazione degli utenti e autorizzazioni, creato tenendo conto della sicurezza.
- **Caching**: la creazione dinamica dei contenuti richiede molte più risorse computazionali, ed è più lenta, rispetto alla distribuzione di contenuto statico. Django fornisce un caching flessibile, in modo da poter memorizzare tutta o parte di una pagina renderizzata ed evitare di renderizzarla nuovamente se non quando necessario.
- **Sito di amministrazione**: il sito di amministrazione Django è incluso per impostazione predefinita quando si crea un'app usando lo scheletro di base. Rende estremamente semplice fornire una pagina di amministrazione affinché gli amministratori del sito possano creare, modificare e visualizzare qualsiasi modello di dati nel sito.
- **Serializzazione dei dati**: Django semplifica la serializzazione e la distribuzione dei dati come XML o JSON. Questo può essere utile quando si crea un servizio web, ovvero un sito web che serve esclusivamente dati destinati a essere utilizzati da altre applicazioni o siti e non visualizza nulla direttamente, oppure quando si crea un sito web in cui il codice lato client gestisce tutto il rendering dei dati.

## Riepilogo

Complimenti, è stato completato il primo passo nel percorso con Django. Ora dovrebbero essere chiari i principali vantaggi di Django, alcuni aspetti della sua storia e, approssimativamente, l'aspetto di ciascuna delle parti principali di un'app Django. Sono stati inoltre appresi alcuni elementi del linguaggio di programmazione Python, inclusa la sintassi per elenchi, funzioni e classi.

In precedenza è già stato visto del vero codice Django, ma, diversamente dal codice lato client, per eseguirlo è necessario configurare un ambiente di sviluppo. Questo è il prossimo passo.

{{NextMenu("Learn_web_development/Extensions/Server-side/Django/development_environment", "Learn_web_development/Extensions/Server-side/Django")}}
