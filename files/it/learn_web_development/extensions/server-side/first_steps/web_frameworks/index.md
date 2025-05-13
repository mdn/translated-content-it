---
title: Framework web lato server
short-title: Frameworks lato server
slug: Learn_web_development/Extensions/Server-side/First_steps/Web_frameworks
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/First_steps/Client-Server_overview", "Learn_web_development/Extensions/Server-side/First_steps/Website_security", "Learn_web_development/Extensions/Server-side/First_steps")}}

L'articolo precedente le ha mostrato come appare la comunicazione tra clienti web e server, la natura delle richieste e delle risposte HTTP e cosa deve fare un'applicazione web lato server per rispondere alle richieste da un browser web. Con questa conoscenza, è tempo di esplorare come i framework web possano semplificare questi compiti e fornire un'idea di come scegliere un framework per la sua prima applicazione web lato server.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Una conoscenza di base di come il codice lato server
        gestisce e risponde alle richieste HTTP (vedi <a
          href="/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Client-Server_overview"
          >Panoramica Client-Server</a
        >).
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere come i framework web possano semplificare lo sviluppo/manutenzione del
        codice lato server e avvicinare i lettori alla selezione di un framework
        per il proprio sviluppo.
      </td>
    </tr>
  </tbody>
</table>

Le sezioni seguenti illustrano alcuni punti utilizzando frammenti di codice tratti da framework web reali. Non si preoccupi se non le è tutto chiaro ora; lavoreremo tramite il codice nei nostri moduli specifici per framework.

## Panoramica

I framework web lato server (noti anche come "web application frameworks") sono framework software che rendono più facile scrivere, mantenere e scalare applicazioni web. Forniscono strumenti e librerie che semplificano i compiti comuni di sviluppo web, inclusi il routing degli URL verso i gestori appropriati, l'interazione con i database, il supporto a sessioni e autorizzazioni utente, la formattazione dell'output (ad esempio, HTML, JSON, XML) e il miglioramento della sicurezza contro gli attacchi web.

La sezione successiva fornisce maggiori dettagli su come i framework web possano facilitare lo sviluppo di applicazioni web. Spieghiamo quindi alcuni dei criteri che si possono utilizzare per scegliere un framework web, per poi elencare alcune delle sue opzioni.

## Cosa può fare un framework web per lei?

I framework web forniscono strumenti e librerie per semplificare le operazioni comuni di sviluppo web. Non è _obbligatorio_ utilizzare un framework web lato server, ma è fortemente consigliato: le renderà la vita molto più semplice.

Questa sezione discute alcune delle funzionalità spesso fornite dai framework web (non tutti i framework forniranno necessariamente tutte queste caratteristiche!).

### Lavorare direttamente con le richieste e le risposte HTTP

Come abbiamo visto nell'ultimo articolo, i server web e i browser comunicano tramite il protocollo HTTP — i server attendono richieste HTTP dal browser e poi restituiscono informazioni nelle risposte HTTP. I framework web permettono di scrivere una sintassi semplificata che genererà codice lato server per lavorare con queste richieste e risposte. Questo significa che avrà un compito più semplice, interagendo con un codice più facile e di alto livello piuttosto che con primitive di rete di basso livello.

L'esempio seguente mostra come funziona questo nel framework web Django (Python). Ogni funzione "view" (un gestore di richieste) riceve un oggetto `HttpRequest` contenente le informazioni di richiesta, e deve restituire un oggetto `HttpResponse` con l'output formattato (in questo caso una stringa).

```python
# Django view function
from django.http import HttpResponse

def index(request):
    # Get an HttpRequest (request)
    # perform operations using information from the request.
    # Return HttpResponse
    return HttpResponse('Output string to return')
```

### Instradare le richieste al gestore appropriato

La maggior parte dei siti fornisce un certo numero di risorse diverse, accessibili tramite URL distinti. Gestire tutto in una funzione sarebbe difficile da mantenere, quindi i framework web forniscono meccanismi semplici per mappare schemi URL a specifiche funzioni gestore. Questo approccio ha anche benefici in termini di manutenzione, perché può cambiare l'URL utilizzato per offrire una particolare funzione senza dover modificare il codice sottostante.

Diversi framework usano meccanismi diversi per la mappatura. Ad esempio, il framework web Flask (Python) aggiunge rotte alle funzioni di visualizzazione usando un decoratore.

```python
@app.route("/")
def hello():
    return "Hello World!"
```

Mentre Django si aspetta che gli sviluppatori definiscano una lista di mappature URL tra uno schema URL e una funzione di visualizzazione.

```python
urlpatterns = [
    url(r'^$', views.index),
    # example: /best/my_team_name/5/
    url(r'^best/(?P<team_name>\w+?)/(?P<team_number>[0-9]+)/$', views.best),
]
```

### Rendere facile l'accesso ai dati nella richiesta

I dati possono essere codificati in una richiesta HTTP in diversi modi. Una richiesta HTTP `GET` per ottenere file o dati dal server può codificare quali dati sono richiesti nei parametri dell'URL o nella struttura dell'URL. Una richiesta HTTP `POST` per aggiornare una risorsa sul server includerà invece le informazioni di aggiornamento come "dati POST" nel corpo della richiesta. La richiesta HTTP può anche includere informazioni sulla sessione corrente o sull'utente in un cookie lato client.

I framework web forniscono meccanismi adatti al linguaggio di programmazione per accedere a queste informazioni. Ad esempio, l'oggetto `HttpRequest` che Django passa a ogni funzione di visualizzazione contiene metodi e proprietà per accedere all'URL di destinazione, al tipo di richiesta (ad esempio, un HTTP `GET`), ai parametri `GET` o `POST`, ai dati dei cookie e della sessione, ecc. Django può anche passare informazioni codificate nella struttura dell'URL definendo "pattern di cattura" nel mapper URL (vedere l'ultimo frammento di codice nella sezione precedente).

### Astrarre e semplificare l'accesso al database

I siti web utilizzano database per memorizzare informazioni sia da condividere con gli utenti che sugli utenti stessi. I framework web spesso forniscono un livello di database che astrae le operazioni di lettura, scrittura, interrogazione e cancellazione del database. Questo livello di astrazione è chiamato Object-Relational Mapper (ORM).

Utilizzare un ORM ha due vantaggi:

- Può sostituire il database sottostante senza avere necessariamente bisogno di cambiare il codice che lo usa. Questo permette agli sviluppatori di ottimizzare le caratteristiche di diversi database in base al loro utilizzo.
- La convalida di base dei dati può essere implementata all'interno del framework. Questo rende più facile e sicuro verificare che i dati siano memorizzati nel tipo corretto di campo del database, abbiano il formato corretto (ad esempio, un indirizzo email) e non siano in alcun modo dannosi (gli hacker possono utilizzare alcuni pattern di codice per fare cose maligne, come eliminare i record del database).

Ad esempio, il framework web Django fornisce un ORM e si riferisce all'oggetto utilizzato per definire la struttura di un record come il _modello_. Il modello specifica i _tipi_ di campo da memorizzare, che possono fornire una convalida a livello di campo su quali informazioni possono essere memorizzate (ad esempio, un campo email consentirebbe solo indirizzi email validi). Le definizioni dei campi possono anche specificare la dimensione massima, i valori predefiniti, le opzioni della lista di selezione, il testo di aiuto per la documentazione, il testo dell'etichetta per i moduli ecc. Il modello non specifica alcuna informazione sul database sottostante poiché quello è un parametro di configurazione che può essere modificato separatamente dal nostro codice.

Il primo frammento di codice sotto mostra un modello Django molto semplice per un oggetto `Team`. Questo memorizza il nome del team e il livello del team come campi caratteri e specifica un numero massimo di caratteri da memorizzare per ciascun record. Il `team_level` è un campo di scelta, quindi forniamo anche una mappatura tra le scelte da visualizzare e i dati da memorizzare, insieme a un valore predefinito.

```python
#best/models.py

from django.db import models

class Team(models.Model):
    team_name = models.CharField(max_length=40)

    TEAM_LEVELS = (
        ('U09', 'Under 09s'),
        ('U10', 'Under 10s'),
        ('U11', 'Under 11s'),
        # List our other teams
    )
    team_level = models.CharField(max_length=3,choices=TEAM_LEVELS,default='U11')
```

Il modello Django fornisce un'API di interrogazione semplice per la ricerca nel database. Questa può corrispondere a un numero di campi contemporaneamente utilizzando criteri diversi (ad esempio, esatti, senza distinzione fra maiuscole e minuscole, maggiore di, ecc.), e può supportare dichiarazioni complesse (ad esempio, può specificare una ricerca sui team U11 che hanno un nome di team che inizia con "Fr" o termina con "al").

Il secondo frammento di codice mostra una funzione di visualizzazione (gestore di risorse) per la visualizzazione di tutti i nostri team U09. In questo caso specifichiamo che vogliamo filtrare tutti i record in cui il campo `team_level` ha esattamente il testo 'U09' (si noti sotto come questo criterio venga passato alla funzione `filter()` come argomento con il nome del campo e il tipo di corrispondenza separati da due trattini bassi: **team_level\_\_exact**).

```python
#best/views.py

from django.shortcuts import render
from .models import Team

def youngest(request):
    list_teams = Team.objects.filter(team_level__exact="U09")
    context = {'youngest_teams': list_teams}
    return render(request, 'best/index.html', context)
```

### Renderizzazione dei dati

I framework web spesso forniscono sistemi di templating. Questi permettono di specificare la struttura di un documento di output, utilizzando segnaposto per i dati che verranno aggiunti quando una pagina viene generata. I template sono spesso usati per creare HTML, ma possono anche generare altri tipi di documenti.

I framework web offrono spesso un meccanismo per facilitare la generazione di altri formati dai dati memorizzati, inclusi {{Glossary("JSON", "JSON")}} e {{Glossary("XML", "XML")}}.

Ad esempio, il sistema di template di Django permette di specificare variabili usando una sintassi "double-handlebars" (ad esempio, `\{{ variable_name }}`), che verranno sostituite da valori passati dalla funzione di visualizzazione quando una pagina è renderizzata. Il sistema template supporta anche espressioni (con sintassi: `{% expression %}`), che permettono ai template di eseguire semplici operazioni come iterazione di valori di lista passati nel template.

> [!NOTE]
> Molti altri sistemi di templating utilizzano una sintassi simile, ad esempio: Jinja2 (Python), handlebars (JavaScript), moustache (JavaScript), eccetera.

Il frammento di codice sotto mostra come funziona questo. Continuando con l'esempio "team più giovane" dalla sezione precedente, il template HTML riceve una variabile lista chiamata `youngest_teams` dalla visualizzazione. All'interno dello scheletro HTML abbiamo un'espressione che verifica prima se la variabile `youngest_teams` esiste, e poi la itera in un ciclo `for`. Ad ogni iterazione il template visualizza il valore `team_name` del team in un elenco di elementi.

```django
#best/templates/best/index.html

<!doctype html>
<html lang="en">
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

## Come selezionare un framework web

Esistono numerosi framework web per quasi ogni linguaggio di programmazione che potrebbe voler utilizzare (elenciamo alcuni dei framework più popolari nella sezione successiva). Con così tante scelte, può diventare difficile capire quale framework fornisca il miglior punto di partenza per la sua nuova applicazione web.

Alcuni dei fattori che potrebbero influenzare la sua decisione sono:

- **Sforzo di apprendimento:** Lo sforzo per imparare un framework web dipende da quanto è familiare con il linguaggio di programmazione sottostante, dalla consistenza della sua API, dalla qualità della sua documentazione e dalla dimensione e attività della sua community. Se inizia da zero esperienza di programmazione, allora potrà considerare Django (è uno dei più facili da imparare in base ai criteri sopra citati). Se fa parte di un team di sviluppo che ha già una significativa esperienza con un particolare framework web o linguaggio di programmazione, allora ha senso restare con quello.
- **Produttività:** La produttività è una misura di quanto velocemente può creare nuove funzionalità una volta che è familiarizzato con il framework, e include sia lo sforzo per scrivere sia per mantenere il codice (poiché non può scrivere nuove funzionalità mentre le vecchie sono rotte). Molti dei fattori che influenzano la produttività sono simili a quelli per lo "Sforzo di apprendimento" — ad esempio, la documentazione, la community, l'esperienza di programmazione, eccetera. Altri fattori includono:

  - _Scopo/origine del framework_: Alcuni framework web sono stati inizialmente creati per risolvere determinati tipi di problemi e rimangono _migliori_ nel creare app web con vincoli simili. Ad esempio, Django è stato creato per supportare lo sviluppo di un sito giornalistico, quindi è adatto per blog e altri siti che coinvolgono la pubblicazione di contenuti. Al contrario, Flask è un framework molto più leggero ed è ideale per creare app web che girano su dispositivi embedded.
  - _Opinionated vs. unopinionated_: Un framework opinionated ha raccomandato "migliori" modi per risolvere un particolare problema. I framework opinionated tendono a essere più produttivi quando si cerca di risolvere problemi comuni, poiché portano nella giusta direzione, tuttavia a volte sono meno flessibili.
  - _Batteries included vs. get it yourself_: Alcuni framework web includono strumenti/librerie che affrontano ogni problema che i loro sviluppatori possono immaginare "di default", mentre i framework più leggeri si aspettano che gli sviluppatori web scelgano e trovino soluzioni ai problemi a partire da librerie separate (Django è un esempio del primo, mentre Flask è un esempio di un framework molto leggero). I framework che includono tutto sono spesso più facili da avviare perché si ha già tutto ciò che serve, e probabilmente è ben integrato e ben documentato. Tuttavia, se un framework più piccolo ha tutto ciò che le serve (e servirà) allora può funzionare in ambienti più limitati e avrà un subset più piccolo e più facile da imparare.
  - _Se il framework incoraggia buone pratiche di sviluppo o meno_: Ad esempio, un framework che incoraggia un'architettura {{Glossary("MVC", "Model-View-Controller")}} per separare il codice in funzioni logiche risulterà in un codice più manutenibile rispetto a uno che non ha aspettative sugli sviluppatori. Allo stesso modo, il design del framework può avere un grande impatto su quanto sia facile testare e riutilizzare il codice.

- **Performance del framework/linguaggio di programmazione:** Di solito la "velocità" non è il fattore più importante nella selezione poiché anche runtime relativamente lenti come Python sono più che "abbastanza buoni" per siti di medie dimensioni che girano su hardware moderato. I benefici di velocità percepiti di un altro linguaggio, ad esempio C++ o JavaScript, possono essere compensati dai costi di apprendimento e manutenzione.
- **Supporto per il caching:** Man mano che il suo sito web diventa più di successo può scoprire che non è più in grado di gestire il numero di richieste che sta ricevendo mentre gli utenti vi accedono. A questo punto potrebbe considerare di aggiungere il supporto per il caching. Il caching è un'ottimizzazione in cui memorizza tutta o parte di una risposta web in modo da non dover essere ricalcolata in richieste successive. Restituire una risposta memorizzata è molto più veloce che calcolarne una in primo luogo. Il caching può essere implementato nel suo codice o nel server (vedi [reverse proxy](https://en.wikipedia.org/wiki/Reverse_proxy)). I framework web avranno supporti di livello diverso per definire quale contenuto può essere memorizzato in cache.
- **Scalabilità:** Una volta che il suo sito web è fantastico e di successo esaurirà i benefici del caching e raggiungerà persino i limiti dello _scaling verticale_ (eseguire la sua applicazione web su hardware più potente). A questo punto potrebbe aver bisogno di _scalare orizzontalmente_ (condividere il carico distribuendo il suo sito su un certo numero di web server e database) o scalare "geograficamente" poiché alcuni dei suoi clienti sono basati molto lontano dal suo server. Il framework web che sceglie può fare una grande differenza su quanto sia facile scalare il suo sito.
- **Sicurezza web:** Alcuni framework web forniscono un migliore supporto per la gestione degli attacchi web comuni. Django ad esempio sanitizza tutti gli input degli utenti nei template HTML in modo che il JavaScript inserito dagli utenti non possa essere eseguito. Altri framework offrono simili protezioni, ma non sempre sono abilitate di default.

Ci sono molti altri fattori possibili, includendo le licenze, se il framework è sotto sviluppo attivo, eccetera.

Se è un principiante assoluto alla programmazione allora probabilmente sceglierà il suo framework basandosi sulla "facilità di apprendimento". Oltre alla "facilità d'uso" del linguaggio stesso, documentazione/tutorial di alta qualità e una community attiva che aiuta i nuovi utenti sono le sue risorse più preziose. Abbiamo scelto [Django](https://www.djangoproject.com/) (Python) e [Express](https://expressjs.com/) (Node/JavaScript) per scrivere i nostri esempi più avanti nel corso, principalmente perché sono facili da imparare e hanno un buon supporto.

> [!NOTE]
> Visitiamo i siti principali di [Django](https://www.djangoproject.com/) (Python) e [Express](https://expressjs.com/) (Node/JavaScript) e verifichiamo la loro documentazione e community.
>
> 1. Acceda ai siti principali (link sopra)
>
>    - Faccia clic sui link del menu Documentazione (nominati come "Documentation, Guide, API Reference, Getting Started", ecc.).
>    - Può vedere argomenti che mostrano come impostare il routing degli URL, i template e i database/modelli?
>    - I documenti sono chiari?
>
> 2. Acceda alle mailing list per ciascun sito (accessibili dai link di Community).
>
>    - Quante domande sono state pubblicate negli ultimi giorni
>    - Quante hanno risposte?
>    - Hanno una community attiva?

## Qualche buon framework web?

Passiamo ora a discutere alcuni specifici framework web lato server.

I framework lato server sotto rappresentano _alcuni_ dei più popolari disponibili al momento della stesura. Tutti hanno tutto ciò di cui ha bisogno per essere produttivi: sono open source, sono sotto sviluppo attivo, hanno community entusiastiche che creano documentazione e aiutano gli utenti su forum di discussione, e sono usati in un gran numero di siti web di grande rilievo. Ci sono molti altri ottimi framework lato server che può scoprire con una semplice ricerca su internet.

> [!NOTE]
> Le descrizioni provengono (parzialmente) dai siti web dei framework!

### Django (Python)

[Django](https://www.djangoproject.com/) è un framework web Python di alto livello che incoraggia lo sviluppo rapido e un design pulito e pragmatico. Sviluppato da esperti, si occupa di gran parte delle complicazioni dello sviluppo web, così può concentrarsi sulla scrittura della sua app senza dover reinventare la ruota. È gratuito e open source.

Django segue la filosofia "Batteries included" e fornisce quasi tutto ciò che la maggior parte degli sviluppatori potrebbe voler fare "out of the box". Poiché tutto è incluso, tutto funziona insieme, segue principi di design coerenti ed ha una documentazione ampia e aggiornata. È anche veloce, sicuro e molto scalabile. Essendo basato su Python, il codice Django è facile da leggere e mantenere.

I siti popolari che utilizzano Django (dalla pagina principale di Django) includono: Disqus, Instagram, Knight Foundation, MacArthur Foundation, Mozilla, National Geographic, Open Knowledge Foundation, Pinterest, Open Stack.

### Flask (Python)

[Flask](https://flask.palletsprojects.com/) è un microframework per Python.

Sebbene minimalista, Flask può creare siti web seri out of the box. Contiene un server di sviluppo e un debugger, e include supporto per il templating [Jinja2](https://github.com/pallets/jinja), cookie sicuri, [unit testing](https://en.wikipedia.org/wiki/Unit_testing) e dispatching delle richieste [RESTful](https://restapitutorial.com/). Ha una buona documentazione e una community attiva.

Flask è diventato estremamente popolare, in particolare per gli sviluppatori che devono fornire servizi web su sistemi piccoli e con risorse limitate (ad esempio, eseguire un server web su un [Raspberry Pi](https://www.raspberrypi.org/), [controller per droni](https://www.techuseful.com/drone-definitions-learning-the-drone-lingo/), ecc.).

### Express (Node.js/JavaScript)

[Express](https://expressjs.com/) è un framework web veloce, unopinionated, flessibile e minimalista per [Node.js](https://nodejs.org/en/) (node è un ambiente senza browser per l'esecuzione di JavaScript). Fornisce un set robusto di funzionalità per applicazioni web e mobili e offre metodi utility HTTP utili e {{Glossary("Middleware", "middleware")}}.

Express è estremamente popolare, in parte perché facilita la migrazione dei programmatori web JavaScript lato client nello sviluppo lato server, e in parte perché è efficiente in termini di risorse (l'ambiente node sottostante utilizza il multitasking leggero all'interno di un thread anziché generare processi separati per ogni nuova richiesta web).

Poiché Express è un framework web minimalista non incorpora ogni componente che potrebbe voler utilizzare (ad esempio, accesso al database e supporto per utenti e sessioni sono forniti tramite librerie indipendenti). Ci sono molti eccellenti componenti indipendenti, ma a volte può essere difficile capire quale sia il migliore per un particolare scopo!

Molti popolari framework lato server e full stack (comprendenti sia framework lato server che lato client) sono basati su Express, inclusi [Feathers](https://feathersjs.com/), [ItemsAPI](https://itemsapi.com/), [KeystoneJS](https://keystonejs.com/), [Kraken](https://krakenjs.com/), [LoopBack](https://loopback.io/), [MEAN](https://github.com/linnovate/mean), e [Sails](https://sailsjs.com/).

Un sacco di aziende di alto profilo usano Express, tra cui: Uber, Accenture, IBM, ecc.

### Deno (JavaScript)

[Deno](https://deno.com/) è un runtime e framework [JavaScript](/it/docs/Web/JavaScript)/TypeScript semplice, moderno e sicuro costruito sopra Chrome V8 e [Rust](https://www.rust-lang.org/).

Deno è alimentato da [Tokio](https://tokio.rs/) — un runtime asincrono basato su Rust che gli permette di servire pagine web più velocemente. Ha anche il supporto interno per il [WebAssembly](/it/docs/WebAssembly), che consente la compilazione di codice binario per l'uso lato client. Deno mira a colmare alcune delle lacune in [Node.js](/it/docs/Learn_web_development/Extensions/Server-side/Node_server_without_framework) fornendo un meccanismo che mantiene naturalmente una migliore sicurezza.

Le caratteristiche di Deno includono:

- Sicurezza per impostazione predefinita. [I moduli Deno limitano i permessi](https://docs.deno.com/runtime/fundamentals/security/) all'accesso a **file**, **network** o **environment** a meno che non siano esplicitamente consentiti.
- Supporto per TypeScript **out-of-the-box**.
- Meccanismo di attesa di prima classe.
- Funzionalità di testing e formattatore di codice integrati (`deno fmt`)
- Compatibilità con i browser (JavaScript): i programmi Deno scritti interamente in JavaScript escludendo il namespace `Deno` (o effettuando il test delle funzionalità per esso), dovrebbero funzionare direttamente in qualsiasi browser moderno.
- Unione di script in un singolo file JavaScript.

Deno fornisce un modo semplice e potente per utilizzare JavaScript sia per la programmazione lato client che server.

### Ruby on Rails (Ruby)

[Rails](https://rubyonrails.org/) (solitamente chiamato "Ruby on Rails") è un framework web scritto per il linguaggio di programmazione Ruby.

Rails segue una filosofia di design molto simile a quella di Django. Come Django, fornisce meccanismi standard per instradare URL, accedere a dati da un database, generare HTML da template e formattare i dati come {{Glossary("JSON", "JSON")}} o {{Glossary("XML", "XML")}}. Incoraggia similmente l'uso di design pattern come DRY ("don't repeat yourself" — scrivere il codice solo una volta se possibile), MVC (model-view-controller) e molti altri.

Ci sono ovviamente molte differenze dovute a decisioni progettuali specifiche e alla natura dei linguaggi.

Rails è stato usato per siti web di alto profilo, tra cui: [Basecamp](https://basecamp.com/), [GitHub](https://github.com/), [Shopify](https://www.shopify.com/), [Airbnb](https://www.airbnb.com/), [Twitch](https://www.twitch.tv/), [SoundCloud](https://soundcloud.com/), [Hulu](https://www.hulu.com/welcome), [Zendesk](https://www.zendesk.com/), [Square](https://squareup.com/us/en), [Highrise](https://highrisehq.com/).

### Laravel (PHP)

[Laravel](https://laravel.com/) è un framework per applicazioni web con sintassi espressiva ed elegante. Laravel tenta di eliminare il dolore dallo sviluppo semplificando i compiti comuni utilizzati nella maggior parte dei progetti web, come:

- [Motore di instradamento semplice e veloce](https://laravel.com/docs/routing).
- [Contenitore di iniezione delle dipendenze potente](https://laravel.com/docs/container).
- Multipli back-end per l'archiviazione di [session](https://laravel.com/docs/session) e [cache](https://laravel.com/docs/cache).
- ORM [database espressivo e intuitivo](https://laravel.com/docs/eloquent).
- Migrazioni degli schemi [database agnostic](https://laravel.com/docs/migrations).
- [Elaborazione di job in background robusta](https://laravel.com/docs/queues).
- [Broadcasting di eventi in tempo reale](https://laravel.com/docs/broadcasting).

Laravel è accessibile, eppure potente, fornendo gli strumenti necessari per grandi applicazioni robuste.

### ASP.NET

[ASP.NET](https://dotnet.microsoft.com/en-us/apps/aspnet) è un framework web open source sviluppato da Microsoft per costruire moderne applicazioni e servizi web. Con ASP.NET può creare rapidamente siti web basati su HTML, CSS e JavaScript, scalarli per l'uso da parte di milioni di utenti e aggiungere facilmente capacità più complesse come Web APIs, moduli su dati, o comunicazioni in tempo reale.

Uno dei differenziatori per ASP.NET è che è costruito sul [Common Language Runtime](https://en.wikipedia.org/wiki/Common_Language_Runtime) (CLR), permettendo ai programmatori di scrivere codice ASP.NET usando qualsiasi linguaggio .NET supportato (C#, Visual Basic, ecc.). Come molti prodotti Microsoft, beneficia di strumenti eccellenti (spesso gratuiti), una community di sviluppatori attiva e una documentazione ben scritta.

ASP.NET è utilizzato da Microsoft, Xbox.com, Stack Overflow e molti altri.

### Mojolicious (Perl)

[Mojolicious](https://mojolicious.org/) è un framework web di nuova generazione per il linguaggio di programmazione Perl.

All'inizio del web, molte persone hanno imparato Perl grazie a una meravigliosa libreria Perl chiamata [CGI](https://metacpan.org/pod/CGI). Era abbastanza semplice da far iniziare senza sapere molto del linguaggio e abbastanza potente da tenere lo sviluppo in corso. Mojolicious implementa questa idea utilizzando tecnologie all'avanguardia.

Alcune delle caratteristiche fornite da Mojolicious sono:

- Un framework web in tempo reale, per far crescere facilmente prototipi a file unico in applicazioni web ben strutturate MRV.
- Rotte RESTful, plugin, comandi, template Perl-ish, negoziazione dei contenuti, gestione delle sessioni, validazione dei form, framework di test, server di file statici, rilevamento CGI/[PSGI](https://plackperl.org/) e supporto Unicode di prima classe.
- Una implementazione completa del client/server HTTP e WebSocket con supporto IPv6, TLS, SNI, IDNA, proxy HTTP/SOCKS5, socket di dominio UNIX, Comet (long polling), connessione persistente, pooling di connessione, timeout, cookie, multipart e compressione gzip.
- Parser e generatori JSON e HTML/XML con supporto ai selettori CSS.
- API pure-Perl molto pulite, portatili e orientate agli oggetti senza magia nascosta.
- Codice fresco basato su anni di esperienza, gratuito e open source.

### Spring Boot (Java)

[Spring Boot](https://spring.io/projects/spring-boot/) è uno dei numerosi progetti forniti da [Spring](https://spring.io/). È un buon punto di partenza per fare sviluppo web lato server usando [Java](https://www.java.com/).

Sebbene non sia di certo l'unico framework basato su [Java](https://www.java.com/), è facile da usare per creare applicazioni Spring standalone, di classe produzione che può "eseguire direttamente". È una visione opinionated della piattaforma Spring e delle librerie di terze parti, ma permette di iniziare con il minimo sforzo e configurazione.

Può essere usato per problemi piccoli ma la sua forza è costruire applicazioni di scala più grande che utilizzano un approccio cloud. Di solito più applicazioni girano in parallelo parlando tra loro, alcune fornendo interazione con gli utenti e altre eseguendo lavori di back-end (ad esempio, accedendo a database o altri servizi). I load balancer servono ad assicurare ridondanza e affidabilità o consentono la gestione geoposizionata delle richieste dell'utente per garantire la reattività.

## Sommario

Questo articolo ha mostrato che i framework web possono facilitare lo sviluppo e la manutenzione di codice lato server. Ha anche fornito una panoramica a livello generale di alcuni framework popolari e discusso i criteri per scegliere un framework di applicazioni web. Ora dovrebbe avere almeno un'idea di come scegliere un framework web per il suo sviluppo lato server. Se così non fosse, non si preoccupi: più avanti nel corso le forniremo tutorial dettagliati su Django e Express per fornirle esperienza diretta di lavoro con un framework web.

Per l'articolo successivo in questo modulo cambieremo leggermente direzione e considereremo la sicurezza web.

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/First_steps/Client-Server_overview", "Learn_web_development/Extensions/Server-side/First_steps/Website_security", "Learn_web_development/Extensions/Server-side/First_steps")}}
