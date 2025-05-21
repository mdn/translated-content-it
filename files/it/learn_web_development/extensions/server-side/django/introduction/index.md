---
title: Introduzione a Django
slug: Learn_web_development/Extensions/Server-side/Django/Introduction
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Extensions/Server-side/Django/development_environment", "Learn_web_development/Extensions/Server-side/Django")}}

In questo primo articolo su Django, risponderemo alla domanda "Cos'è Django?" e ti forniremo una panoramica su ciò che rende questo framework web speciale.

Descriveremo le caratteristiche principali, compresa qualche funzionalità avanzata che non avremo tempo di trattare in dettaglio in questo modulo. Mostreremo inoltre alcuni dei principali elementi costitutivi di un'applicazione Django (anche se a questo punto non avrai ancora un ambiente di sviluppo in cui testarla).

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Una comprensione generale della <a href="/it/docs/Learn_web_development/Extensions/Server-side/First_steps">programmazione server-side per siti web</a>, e in particolare le dinamiche delle <a href="/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Client-Server_overview">interazioni client-server nei siti web</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Acquisire familiarità con cosa sia Django, quali funzionalità fornisca, e quali siano i principali elementi costitutivi di un'applicazione Django.
      </td>
    </tr>
  </tbody>
</table>

## Cos'è Django?

Django è un framework web Python di alto livello che permette lo sviluppo rapido di siti web sicuri e mantenibili. Costruito da sviluppatori esperti, Django si occupa di gran parte delle complicazioni dello sviluppo web, così puoi concentrarti sullo sviluppo della tua app senza la necessità di reinventare la ruota. È gratuito e open source, ha una comunità attiva e fiorente, una documentazione eccellente e molte opzioni per supporto gratuito e a pagamento.

Django ti aiuta a scrivere software che è:

- Completo
  - : Django segue la filosofia "Batteries included" e offre quasi tutto ciò che potrebbe essere necessario agli sviluppatori già "out of the box". Poiché tutto ciò di cui hai bisogno è parte di un unico "prodotto", tutto funziona senza intoppi insieme, seguendo principi di design coerenti e con una documentazione [aggiornata](https://docs.djangoproject.com/en/stable/).
- Versatile

  - : Django può essere (ed è stato) utilizzato per costruire quasi qualsiasi tipo di sito web — dai sistemi di gestione dei contenuti e wiki, ai social network e siti di notizie. Può funzionare con qualsiasi framework client-side e può fornire contenuti in quasi ogni formato (inclusi HTML, feed RSS, JSON, e XML).

    Internamente, pur fornendo opzioni per quasi ogni funzionalità che potresti desiderare (es. diversi database popolari, motori di template, ecc.), può anche essere esteso per usare altri componenti se necessario.

- Sicuro

  - : Django aiuta gli sviluppatori a evitare molti errori comuni di sicurezza fornendo un framework che è stato progettato per "fare le cose giuste" per proteggere automaticamente il sito web. Ad esempio, Django fornisce un modo sicuro per gestire gli account utente e le password, evitando errori comuni come inserire informazioni di sessione nei cookie dove sono vulnerabili (invece i cookie contengono solo una chiave, e i dati reali sono memorizzati nel database) o memorizzare direttamente le password piuttosto che un hash della password.

    _Un hash di password è un valore di lunghezza fissa creato inviando la password attraverso una [funzione di hash crittografica](https://en.wikipedia.org/wiki/Cryptographic_hash_function). Django può verificare se una password inserita è corretta eseguendola attraverso la funzione di hash e confrontando l'output con il valore hash memorizzato. Tuttavia, a causa della natura "one-way" della funzione, anche se un valore hash memorizzato viene compromesso, è difficile per un attaccante risalire alla password originale._

    Django abilita la protezione contro molte vulnerabilità per impostazione predefinita, inclusi SQL injection, scripting cross-site, falsificazione di richiesta cross-site e [clickjacking](/it/docs/Web/Security/Attacks/Clickjacking) (vedi [Sicurezza del sito web](/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Website_security) per maggiori dettagli su tali attacchi).

- Scalabile
  - : Django utilizza un'architettura basata su componenti "[shared-nothing](https://en.wikipedia.org/wiki/Shared_nothing_architecture)" (ogni parte dell'architettura è indipendente dalle altre e può quindi essere sostituita o cambiata se necessario). Avere una chiara separazione tra le parti diverse significa che può scalare per un traffico aumentato aggiungendo hardware a qualsiasi livello: server di caching, server di database o server di applicazioni. Alcuni dei siti più trafficati hanno scalato con successo Django per soddisfare le loro esigenze (es. Instagram e Disqus, per citarne solo due).
- Manutenibile
  - : Il codice Django è scritto utilizzando principi e pattern di design che incoraggiano la creazione di codice manutenibile e riutilizzabile. In particolare, fa uso del principio Don't Repeat Yourself (DRY) in modo che non ci sia duplicazione non necessaria, riducendo la quantità di codice. Django promuove anche il raggruppamento della funzionalità correlata in "applicazioni" riutilizzabili e, a un livello inferiore, raggruppa il codice correlato in moduli (sulla falsariga del pattern {{Glossary("MVC", "Model View Controller (MVC)")}}).
- Portatile
  - : Django è scritto in Python, che viene eseguito su molte piattaforme. Ciò significa che non sei legato a nessuna piattaforma server particolare, e puoi eseguire le tue applicazioni su molti tipi di Linux, Windows e macOS. Inoltre, Django è ben supportato da molti provider di hosting web, che spesso forniscono infrastruttura specifica e documentazione per l'hosting di siti Django.

## Da dove viene?

Django è stato inizialmente sviluppato tra il 2003 e il 2005 da un team web responsabile della creazione e manutenzione di siti web di giornali. Dopo aver creato numerosi siti, il team ha iniziato a separare e riutilizzare molto codice comune e pattern di design. Questo codice comune si è evoluto in un framework generico per lo sviluppo web, che è stato reso open source come progetto "Django" nel luglio 2005.

Django ha continuato a crescere e migliorare, dalla sua prima release significativa (1.0) nel settembre 2008 fino alla versione 5.0 alla fine del 2023. Ogni release ha aggiunto nuove funzionalità e correzioni di bug, spaziando dal supporto per nuovi tipi di database, motori di template, e caching, fino all'aggiunta di funzioni e classi di visualizzazioni "generiche" (che riducono la quantità di codice che gli sviluppatori devono scrivere per una serie di compiti di programmazione).

> [!NOTE]
> Consulta le [note di rilascio](https://docs.djangoproject.com/en/stable/releases/) sul sito di Django per vedere cosa è cambiato nelle versioni recenti e quanto lavoro viene fatto per migliorare Django.

Django è ora un progetto open source collaborativo e fiorente, con molte migliaia di utenti e contributori. Sebbene abbia ancora alcune caratteristiche che riflettono la sua origine, Django si è evoluto in un framework versatile in grado di sviluppare qualsiasi tipo di sito web.

## Quanto è popolare Django?

Non esiste una misurazione facilmente disponibile e definitiva della popolarità dei framework server-side (anche se puoi stimarne la popolarità utilizzando meccanismi come il conteggio dei progetti su GitHub e delle domande su Stack Overflow per ciascuna piattaforma). Una domanda migliore è se Django è "abbastanza popolare" da evitare i problemi delle piattaforme impopolari. Continua a evolversi? Puoi ottenere aiuto se ne hai bisogno? C'è un'opportunità di lavoro retribuito se impari Django?

In base al numero di siti di alto profilo che utilizzano Django, il numero di persone che contribuiscono alla codebase e il numero di persone che forniscono supporto sia gratuito che a pagamento, allora sì, Django è un framework popolare!

Siti di alto profilo che usano Django includono: Disqus, Instagram, Knight Foundation, MacArthur Foundation, Mozilla, National Geographic, Open Knowledge Foundation, Pinterest e Open Stack (fonte: [pagina panoramica di Django](https://www.djangoproject.com/start/overview/)).

## Django è "opinionated"?

I framework web spesso si definiscono come "opinionated" o "unopinionated".

I framework "opinionated" sono quelli che hanno opinioni su "il modo giusto" di gestire un determinato compito. Supportano spesso lo sviluppo rapido _in un dominio particolare_ (risoluzione di problemi di un tipo particolare) perché il modo giusto per fare qualcosa è solitamente ben compreso e ben documentato. Tuttavia, possono essere meno flessibili nel risolvere problemi al di fuori del loro dominio principale e tendono ad offrire meno scelte per quali componenti e approcci utilizzare.

I framework "unopinionated", al contrario, impongono molte meno restrizioni sul miglior modo di unire componenti per raggiungere un obiettivo, o addirittura su quali componenti dovrebbero essere utilizzati. Rendono più facile per gli sviluppatori utilizzare gli strumenti più adatti per completare un determinato compito, sebbene al costo che devi trovare quei componenti da solo.

Django è "parzialmente opinionated", e quindi offre "il meglio di entrambi i mondi". Fornisce un insieme di componenti per gestire la maggior parte dei compiti di sviluppo web e uno (o due) modi preferiti per utilizzarli. Tuttavia, l'architettura disaccoppiata di Django significa che puoi generalmente scegliere da una serie di opzioni differenti o aggiungere supporto per nuove se necessario.

## Come appare il codice Django?

In un sito web tradizionale basato su dati, un'applicazione web attende richieste HTTP dal browser web (o da un altro client). Quando viene ricevuta una richiesta, l'applicazione determina cosa è necessario in base all'URL e, possibilmente, alle informazioni nei dati `POST` o `GET`. A seconda di cosa è richiesto, può poi leggere o scrivere informazioni da o verso un database o eseguire altri compiti necessari per soddisfare la richiesta. L'applicazione quindi restituirà una risposta al browser web, spesso creando dinamicamente una pagina HTML per il browser da visualizzare, inserendo i dati recuperati in segnaposti in un template HTML.

Le applicazioni web Django tipicamente raggruppano il codice che gestisce ciascuno di questi passaggi in file separati:

![Django - file per visualizzazioni, modello, URL, template](basic-django.png)

- **URL:** Sebbene sia possibile elaborare richieste da ogni singolo URL tramite una singola funzione, è molto più gestibile scrivere una funzione di vista separata per gestire ciascuna risorsa. Un mappatore di URL viene utilizzato per reindirizzare le richieste HTTP alla vista appropriata in base all'URL della richiesta. Il mappatore di URL può anche abbinare particolari pattern di stringhe o cifre che appaiono in un URL e passarle a una funzione di vista come dati.
- **Vista:** Una vista è una funzione di gestione delle richieste, che riceve richieste HTTP e restituisce risposte HTTP. Le viste accedono ai dati necessari per soddisfare le richieste tramite _modelli_, e delegano la formattazione della risposta ai _template_.
- **Modelli:** I modelli sono oggetti Python che definiscono la struttura dei dati di un'applicazione e forniscono meccanismi per gestire (aggiungere, modificare, eliminare) e interrogare i record nel database.
- **Template:** Un template è un file di testo che definisce la struttura o il layout di un file (come una pagina HTML), con segnaposti utilizzati per rappresentare contenuti effettivi. Una _vista_ può creare dinamicamente una pagina HTML utilizzando un template HTML, popolandolo con dati da un _modello_. Un template può essere utilizzato per definire la struttura di qualsiasi tipo di file; non deve necessariamente essere HTML!

> [!NOTE]
> Django si riferisce a questa organizzazione come architettura "Model View Template (MVT)". Ha molte somiglianze con l'architettura più familiare {{Glossary("MVC", "Model View Controller")}}.

Le sezioni seguenti ti daranno un'idea di come appaiono queste parti principali di un'app Django (entreremo più nel dettaglio nel corso più avanti, una volta che avremo impostato un ambiente di sviluppo).

### Invio della richiesta alla vista corretta (urls.py)

Un mappatore di URL è tipicamente memorizzato in un file chiamato **urls.py**.
Nell'esempio seguente, il mappatore (`urlpatterns`) definisce un elenco di corrispondenze tra _rotte_ (pattern di URL specifici) e le corrispondenti funzioni di vista.
Se viene ricevuta una richiesta HTTP che ha un URL che corrisponde a un pattern specificato, la funzione di vista associata verrà chiamata e riceverà la richiesta.

```python
urlpatterns = [
    path('admin/', admin.site.urls),
    path('book/<int:id>/', views.book_detail, name='book_detail'),
    path('catalog/', include('catalog.urls')),
    re_path(r'^([0-9]+)/$', views.best),
]
```

L'oggetto `urlpatterns` è un elenco di funzioni `path()` e/o `re_path()` (gli elenchi in Python sono definiti utilizzando parentesi quadre, dove gli elementi sono separati da virgole e possono avere una [virgola finale opzionale](https://docs.python.org/3/faq/design.html#why-does-python-allow-commas-at-the-end-of-lists-and-tuples). Per esempio: `[item1, item2, item3,]`).

Il primo argomento a entrambi i metodi è una rotta (pattern) che verrà abbinata. Il metodo `path()` utilizza parentesi angolate per definire le parti di un URL che saranno catturate e passate alla funzione di vista come argomenti denominati. La funzione `re_path()` utilizza un approccio di matching dei pattern flessibile noto come espressione regolare. Ne parleremo in un articolo successivo!

Il secondo argomento è un'altra funzione che verrà chiamata quando il pattern coincide. La notazione `views.book_detail` indica che la funzione si chiama `book_detail()` e può essere trovata in un modulo chiamato `views` (cioè dentro un file chiamato `views.py`).

### Gestione della richiesta (views.py)

Le viste sono il cuore dell'applicazione web, ricevendo richieste HTTP dai client web e restituendo risposte HTTP. Nel frattempo, coordinano le altre risorse del framework per accedere ai database, eseguire rendering di template, ecc.

L'esempio seguente mostra una funzione di vista minima `index()`, che potrebbe essere stata chiamata dal nostro mappatore di URL nella sezione precedente. Come tutte le funzioni di vista, riceve un oggetto `HttpRequest` come parametro (`request`) e restituisce un oggetto `HttpResponse`. In questo caso, non facciamo nulla con la richiesta, e la nostra risposta restituisce una stringa codificata a mano. Ti mostreremo una richiesta che fa qualcosa di più interessante in una sezione successiva.

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
> - I [moduli Python](https://docs.python.org/3/tutorial/modules.html) sono "librerie" di funzioni, memorizzate in file separati, che potremmo voler utilizzare nel nostro codice. Qui importiamo solo l'oggetto `HttpResponse` dal modulo `django.http` così che possiamo usarlo nella nostra vista: `from django.http import HttpResponse`. Esistono altri modi per importare alcuni o tutti gli oggetti da un modulo.
> - Le funzioni sono dichiarate utilizzando la parola chiave `def` come mostrato sopra, con parametri denominati elencati tra parentesi dopo il nome della funzione; l'intera riga finisce con due punti. Nota come le righe successive siano tutte **indentate**. L'indentazione è importante, poiché specifica che le righe di codice sono all'interno di quel particolare blocco (l'indentazione obbligatoria è una caratteristica fondamentale di Python, ed è uno dei motivi per cui il codice Python è così facile da leggere).

Le viste sono di solito memorizzate in un file chiamato **views.py**.

### Definire modelli di dati (models.py)

Le applicazioni web Django gestiscono e interrogano i dati tramite oggetti Python chiamati modelli. I modelli definiscono la struttura dei dati memorizzati, inclusi i tipi di campo e possibilmente anche la loro dimensione massima, i valori predefiniti, le opzioni della lista di selezione, il testo di aiuto per la documentazione, il testo dell'etichetta per i moduli, ecc. La definizione del modello è indipendente dal database sottostante — puoi scegliere uno di diversi come parte delle impostazioni del tuo progetto. Una volta che hai scelto quale database vuoi usare, non è necessario dialogare direttamente con esso — scrivi semplicemente la struttura del modello e altro codice, e Django gestisce tutto il "lavoro sporco" di comunicare con il database per te.

Il frammento di codice sottostante mostra un modello Django molto semplice per un oggetto `Team`. La classe `Team` è derivata dalla classe Django `models.Model`. Definisce il nome del team e il livello del team come campi di caratteri e specifica un numero massimo di caratteri da memorizzare per ciascun record. Il `team_level` può essere uno di diversi valori, quindi lo definiamo come un campo di scelta e forniamo una mappatura tra le scelte da visualizzare e i dati da memorizzare, insieme a un valore predefinito.

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
> Python supporta la "programmazione orientata agli oggetti", uno stile di programmazione in cui organizziamo il nostro codice in oggetti, che includono dati correlati e funzioni per operare su quei dati. Gli oggetti possono anche ereditare/estendere/derivare da altri oggetti, permettendo di condividere un comportamento comune tra oggetti correlati. In Python usiamo la parola chiave `class` per definire il "progetto" per un oggetto. Possiamo creare molteplici _istanze_ specifiche del tipo di oggetto basato sul modello nella classe.
>
> Così come, per esempio, qui abbiamo una classe `Team`, che deriva dalla classe `Model`. Ciò significa che è un modello, e conterrà tutti i metodi di un modello, ma possiamo anche darle caratteristiche specializzate sue proprie. Nel nostro modello definiamo i campi che il nostro database avrà bisogno per memorizzare i nostri dati, assegnando loro nomi specifici. Django utilizza queste definizioni, inclusi i nomi dei campi, per creare il database sottostante.

### Interrogare i dati (views.py)

Il modello Django fornisce un'API di interrogazione semplice per cercare nel database associato. Questa può abbinare più campi in una volta utilizzando criteri diversi (ad esempio, esatto, case-insensitive, maggiore di, ecc.) e può supportare dichiarazioni complesse (per esempio, puoi specificare una ricerca su team U11 che hanno un nome di team che inizia con "Fr" o termina con "al").

Il frammento di codice mostra una funzione di vista (gestore delle risorse) per visualizzare tutti i nostri team U09. La riga `list_teams = Team.objects.filter(team_level__exact="U09")` mostra come possiamo usare l'API di interrogazione del modello per filtrare tutti i record in cui il campo `team_level` ha esattamente il testo `U09` (nota come questo criterio è passato alla funzione `filter()` come argomento, con il nome del campo e il tipo di abbinamento separati da un doppio underscore: **`team_level__exact`**).

```python
## filename: views.py

from django.shortcuts import render
from .models import Team

def index(request):
    list_teams = Team.objects.filter(team_level__exact="U09")
    context = {'youngest_teams': list_teams}
    return render(request, '/best/index.html', context)
```

Questa funzione utilizza la funzione `render()` per creare l'`HttpResponse` che viene inviato al browser. Questa funzione è una scorciatoia; crea un file HTML combinando un template HTML specificato e alcuni dati da inserire nel template (forniti nella variabile chiamata `context`). Nella sezione successiva mostriamo come il template abbia i dati inseriti per creare l'HTML.

### Rendere i dati (template HTML)

I sistemi di template ti permettono di specificare la struttura di un documento di output, utilizzando segnaposti per i dati che verranno inseriti quando una pagina viene generata. I template sono spesso utilizzati per creare HTML, ma possono anche creare altri tipi di documento. Django supporta sia il suo sistema di templating nativo che un'altra libreria Python popolare chiamata Jinja2 di default (può anche essere reso compatibile con altri sistemi se necessario).

Il frammento di codice mostra come potrebbe appare il template HTML chiamato dalla funzione `render()` nella sezione precedente. Questo template è stato scritto sotto l'assunzione che avrà accesso a una lista variabile chiamata `youngest_teams` quando viene renderizzato (questa è contenuta nella variabile `context` dentro la funzione `render()` sopra). All'interno dello scheletro HTML abbiamo un'espressione che prima verifica se la variabile `youngest_teams` esiste, e poi la itera in un ciclo `for`. Ad ogni iterazione il template visualizza il valore `team_name` di ciascun team in un elemento `{{htmlelement("li")}}`.

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

## Cos'altro puoi fare?

Le sezioni precedenti mostrano le caratteristiche principali che userai in quasi ogni applicazione web: mapping degli URL, viste, modelli e template. Alcune delle altre cose fornite da Django includono:

- **Moduli**: i moduli HTML vengono utilizzati per raccogliere dati dell'utente da elaborare sul server. Django semplifica la creazione, convalida e elaborazione dei moduli.
- **Autenticazione e permessi dell'utente**: Django include un robusto sistema di autenticazione e permessi degli utenti costruito pensando alla sicurezza.
- **Caching**: Creare contenuti dinamicamente è molto più intensivo dal punto di vista computazionale (e lento) rispetto a servire contenuti statici. Django fornisce caching flessibile in modo che tu possa memorizzare tutto o parte di una pagina renderizzata affinché non venga ri-renderizzata se non necessario.
- **Sito di amministrazione**: Il sito di amministrazione di Django è incluso di default quando crei un'app usando lo scheletro base. Rende trivialmente facile fornire una pagina di amministrazione per gli amministratori del sito per creare, modificare e visualizzare qualsiasi modello di dati nel tuo sito.
- **Serializzazione dei dati**: Django rende facile serializzare e servire i tuoi dati come XML o JSON. Questo può essere utile quando si crea un web service (un sito web che serve puramente dati per essere consumati da altre applicazioni o siti, e non visualizza nulla esso stesso), o quando si crea un sito web in cui il codice client-side gestisce tutto il rendering dei dati.

## Riassunto

Congratulazioni, hai completato il primo passo nel tuo viaggio con Django! Ora dovresti comprendere i principali vantaggi di Django, un po' della sua storia e capire a grandi linee come potrebbero apparire le principali parti di un'app Django. Dovresti aver imparato anche alcune cose sul linguaggio di programmazione Python, incluso la sintassi per liste, funzioni e classi.

Hai già visto un po' di codice Django reale sopra, ma a differenza del codice client-side, hai bisogno di impostare un ambiente di sviluppo per eseguirlo. Questo è il nostro prossimo passo.

{{NextMenu("Learn_web_development/Extensions/Server-side/Django/development_environment", "Learn_web_development/Extensions/Server-side/Django")}}
