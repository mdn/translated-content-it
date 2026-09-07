---
title: Framework web lato server
short-title: Framework lato server
slug: Learn_web_development/Extensions/Server-side/First_steps/Web_frameworks
l10n:
  sourceCommit: 56f3d7018159127dbe92842413fb45d0aa7e8193
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/First_steps/Client-Server_overview", "Learn_web_development/Extensions/Server-side/First_steps/Website_security", "Learn_web_development/Extensions/Server-side/First_steps")}}

L'articolo precedente ha mostrato come avviene la comunicazione tra client web e server, la natura delle richieste e delle risposte HTTP e cosa deve fare un'applicazione web lato server per rispondere alle richieste provenienti da un browser web. Con queste conoscenze, è il momento di esplorare come i framework web possano semplificare queste attività e di fornire un'idea di come scegliere un framework per la prima applicazione web lato server.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Comprensione di base di come il codice lato server
        gestisce e risponde alle richieste HTTP (vedere <a
          href="/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Client-Server_overview"
          >Panoramica client-server</a
        >).
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere come i framework web possano semplificare lo sviluppo e la manutenzione del
        codice lato server e incoraggiare i lettori a scegliere un framework
        per il proprio sviluppo.
      </td>
    </tr>
  </tbody>
</table>

Le sezioni seguenti illustrano alcuni aspetti usando frammenti di codice tratti da framework web reali. Non è necessario preoccuparsi se ora non risulta chiaro **tutto**; il codice verrà analizzato nei moduli specifici per ciascun framework.

## Panoramica

I framework web lato server (noti anche come "framework per applicazioni web") sono framework software che rendono più semplice scrivere, mantenere e scalare le applicazioni web. Forniscono strumenti e librerie che semplificano le attività comuni dello sviluppo web, tra cui l'instradamento degli URL verso gli handler appropriati, l'interazione con i database, il supporto per sessioni e autorizzazione degli utenti, la formattazione dell'output (ad esempio HTML, JSON, XML) e il miglioramento della sicurezza contro gli attacchi web.

La sezione successiva fornisce maggiori dettagli su come i framework web possano facilitare lo sviluppo di applicazioni web. Vengono poi illustrati alcuni criteri utilizzabili per scegliere un framework web e vengono elencate alcune opzioni.

## Cosa può fare un framework web?

I framework web forniscono strumenti e librerie per semplificare le operazioni comuni dello sviluppo web. Non è _obbligatorio_ usare un framework web lato server, ma è fortemente consigliato: semplificherà notevolmente il lavoro.

Questa sezione descrive alcune delle funzionalità spesso fornite dai framework web (non tutti i framework offriranno necessariamente tutte queste caratteristiche).

### Lavorare direttamente con richieste e risposte HTTP

Come visto nell'articolo precedente, server web e browser comunicano tramite il protocollo HTTP: i server attendono richieste HTTP dal browser e restituiscono poi informazioni nelle risposte HTTP. I framework web permettono di scrivere una sintassi semplificata che genera codice lato server per lavorare con tali richieste e risposte. Ciò rende più semplice il lavoro, perché si interagisce con codice di livello più alto e più semplice, anziché con primitive di rete di livello inferiore.

L'esempio seguente mostra come funziona nel framework web Django (Python). Ogni funzione "view" (un handler delle richieste) riceve un oggetto `HttpRequest` contenente informazioni sulla richiesta e deve restituire un oggetto `HttpResponse` con l'output formattato (in questo caso una stringa).

```python
# Django view function
from django.http import HttpResponse

def index(request):
    # Get an HttpRequest (request)
    # perform operations using information from the request.
    # Return HttpResponse
    return HttpResponse('Output string to return')
```

### Instradare le richieste verso l'handler appropriato

La maggior parte dei siti fornisce diverse risorse, accessibili tramite URL distinti. Gestirle tutte in un'unica funzione sarebbe difficile da mantenere, quindi i framework web forniscono meccanismi semplici per associare pattern URL a funzioni handler specifiche. Questo approccio offre vantaggi anche in termini di manutenzione, poiché è possibile modificare l'URL usato per fornire una funzionalità specifica senza dover cambiare il codice sottostante.

Framework diversi usano meccanismi diversi per l'associazione. Ad esempio, il framework web Flask (Python) aggiunge route alle funzioni view usando un decorator.

```python
@app.route("/")
def hello():
    return "Hello World!"
```

Django, invece, si aspetta che gli sviluppatori definiscano un elenco di associazioni URL tra un pattern URL e una funzione view.

```python
urlpatterns = [
    url(r'^$', views.index),
    # example: /best/my_team_name/5/
    url(r'^best/(?P<team_name>\w+?)/(?P<team_number>[0-9]+)/$', views.best),
]
```

### Rendere semplice l'accesso ai dati nella richiesta

I dati possono essere codificati in una richiesta HTTP in diversi modi. Una richiesta HTTP `GET` per ottenere file o dati dal server può codificare i dati richiesti nei parametri URL o nella struttura dell'URL. Una richiesta HTTP `POST` per aggiornare una risorsa sul server includerà invece le informazioni di aggiornamento come "dati POST" nel corpo della richiesta. La richiesta HTTP può anche includere informazioni sulla sessione o sull'utente corrente in un cookie lato client.

I framework web forniscono meccanismi appropriati al linguaggio di programmazione per accedere a queste informazioni. Ad esempio, l'oggetto `HttpRequest` che Django passa a ogni funzione view contiene metodi e proprietà per accedere all'URL di destinazione, al tipo di richiesta (ad esempio un HTTP `GET`), ai parametri `GET` o `POST`, ai dati dei cookie e delle sessioni, ecc. Django può anche passare informazioni codificate nella struttura dell'URL definendo "capture pattern" nel mapper URL (vedere l'ultimo frammento di codice nella sezione precedente).

### Astrarre e semplificare l'accesso al database

I siti web usano database per memorizzare informazioni sia da condividere con gli utenti sia sugli utenti. I framework web spesso forniscono un livello database che astrae le operazioni di lettura, scrittura, query ed eliminazione del database. Questo livello di astrazione è chiamato Object-Relational Mapper (ORM).

L'uso di un ORM offre due vantaggi:

- È possibile sostituire il database sottostante senza necessariamente dover modificare il codice che lo utilizza. Questo permette agli sviluppatori di ottimizzare in base alle caratteristiche di database diversi e al loro utilizzo.
- La convalida di base dei dati può essere implementata nel framework. Ciò rende più semplice e sicuro verificare che i dati siano memorizzati nel tipo corretto di campo del database, abbiano il formato corretto (ad esempio un indirizzo email) e non siano in alcun modo dannosi (gli hacker possono usare determinati pattern di codice per compiere azioni indesiderate, come eliminare record del database).

Ad esempio, il framework web Django fornisce un ORM e chiama _model_ l'oggetto usato per definire la struttura di un record. Il model specifica i _tipi_ di campo da memorizzare, che possono fornire una convalida a livello di campo sulle informazioni memorizzabili (ad esempio, un campo email consentirebbe soltanto indirizzi email validi). Le definizioni dei campi possono anche specificarne la dimensione massima, i valori predefiniti, le opzioni dell'elenco di selezione, il testo di aiuto per la documentazione, il testo dell'etichetta per i moduli, ecc. Il model non indica alcuna informazione sul database sottostante, poiché questa è un'impostazione di configurazione che può essere modificata separatamente dal codice.

Il primo frammento di codice seguente mostra un model Django molto semplice per un oggetto `Team`. Memorizza il nome e il livello della squadra come campi di caratteri e specifica un numero massimo di caratteri da memorizzare per ogni record. `team_level` è un campo di scelta, quindi viene fornita anche un'associazione tra le scelte da visualizzare e i dati da memorizzare, insieme a un valore predefinito.

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

Il model Django fornisce una semplice API di query per cercare nel database. Può effettuare corrispondenze su più campi contemporaneamente usando criteri diversi (ad esempio corrispondenza esatta, senza distinzione tra maiuscole e minuscole, maggiore di, ecc.) e può supportare istruzioni complesse (ad esempio, è possibile specificare una ricerca sulle squadre U11 il cui nome inizia con "Fr" oppure termina con "al").

Il secondo frammento di codice mostra una funzione view (handler della risorsa) per visualizzare tutte le squadre U09. In questo caso viene specificato che si desidera filtrare tutti i record in cui il campo `team_level` contiene esattamente il testo 'U09' (si noti di seguito come questo criterio venga passato alla funzione `filter()` come argomento con nome del campo e tipo di corrispondenza separati da doppi underscore: **team_level\_\_exact**).

```python
#best/views.py

from django.shortcuts import render
from .models import Team

def youngest(request):
    list_teams = Team.objects.filter(team_level__exact="U09")
    context = {'youngest_teams': list_teams}
    return render(request, 'best/index.html', context)
```

### Rendering dei dati

I framework web spesso forniscono sistemi di template. Questi consentono di specificare la struttura di un documento di output, usando segnaposto per i dati che verranno aggiunti durante la generazione di una pagina. I template vengono spesso usati per creare HTML, ma possono anche creare altri tipi di documenti.

I framework web forniscono spesso un meccanismo per rendere semplice la generazione di altri formati dai dati memorizzati, inclusi {{Glossary("JSON", "JSON")}} e {{Glossary("XML", "XML")}}.

Ad esempio, il sistema di template Django consente di specificare variabili usando una sintassi a "doppie parentesi graffe" (ad esempio `\{{ variable_name }}`), che verranno sostituite dai valori passati dalla funzione view durante il rendering di una pagina. Il sistema di template offre anche supporto per espressioni (con sintassi: `{% expression %}`), che consentono ai template di eseguire operazioni semplici come l'iterazione sui valori delle liste passate al template.

> [!NOTE]
> Molti altri sistemi di template usano una sintassi simile, ad esempio: Jinja2 (Python), Handlebars (JavaScript), Mustache (JavaScript), ecc.

Il frammento di codice seguente mostra come funziona. Proseguendo l'esempio della "squadra più giovane" della sezione precedente, il template HTML riceve dalla view una variabile lista chiamata `youngest_teams`. All'interno dello scheletro HTML è presente un'espressione che verifica prima se la variabile `youngest_teams` esiste e poi la itera in un ciclo `for`. A ogni iterazione, il template visualizza il valore `team_name` della squadra in un elemento della lista.

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

Esistono numerosi framework web per quasi tutti i linguaggi di programmazione che si potrebbero voler usare (nella sezione seguente vengono elencati alcuni dei framework più popolari). Con così tante possibilità, può diventare difficile capire quale framework costituisca il miglior punto di partenza per una nuova applicazione web.

Alcuni dei fattori che possono influenzare la decisione sono:

- **Impegno per l'apprendimento:** l'impegno necessario per imparare un framework web dipende dalla familiarità con il linguaggio di programmazione sottostante, dalla coerenza della sua API, dalla qualità della documentazione e dalle dimensioni e attività della sua comunità. Per chi parte senza alcuna esperienza di programmazione, si consiglia di considerare Django (in base ai criteri sopra indicati, è uno dei più facili da imparare). Se si fa parte di un team di sviluppo che ha già un'esperienza significativa con un determinato framework web o linguaggio di programmazione, è opportuno continuare a usare quello.
- **Produttività:** la produttività misura la velocità con cui è possibile creare nuove funzionalità una volta acquisita familiarità con il framework e include sia l'impegno per scrivere sia quello per mantenere il codice (poiché non è possibile scrivere nuove funzionalità quando quelle precedenti sono danneggiate). Molti dei fattori che influenzano la produttività sono simili a quelli relativi all'"impegno per l'apprendimento", ad esempio documentazione, comunità, esperienza di programmazione, ecc. Altri fattori includono:
  - _Scopo/origine del framework_: alcuni framework web sono stati creati inizialmente per risolvere determinati tipi di problemi e rimangono _migliori_ nella creazione di app web con vincoli simili. Ad esempio, Django è stato creato per supportare lo sviluppo del sito web di un giornale, quindi è adatto a blog e altri siti che prevedono la pubblicazione di contenuti. Flask, invece, è un framework molto più leggero ed è ottimo per creare app web eseguite su dispositivi embedded.
  - _Opinionated rispetto a unopinionated_: un framework opinionated è un framework in cui esistono modalità "migliori" consigliate per risolvere un determinato problema. I framework opinionated tendono a essere più produttivi quando si cerca di risolvere problemi comuni, perché indicano la giusta direzione; tuttavia, talvolta sono meno flessibili.
  - _Batteries included rispetto a get it yourself_: alcuni framework web includono, "per impostazione predefinita", strumenti e librerie che affrontano ogni problema immaginabile per i loro sviluppatori, mentre i framework più leggeri si aspettano che gli sviluppatori web scelgano le soluzioni ai problemi tra librerie separate (Django è un esempio del primo caso, mentre Flask è un esempio di framework molto leggero). I framework che includono tutto sono spesso più semplici da usare inizialmente, perché contengono già tutto il necessario e probabilmente è ben integrato e documentato. Tuttavia, se un framework più piccolo contiene tutto ciò che serve, può essere eseguito in ambienti con maggiori vincoli e presenta un insieme di elementi da apprendere più ristretto e semplice.
  - _Se il framework incoraggia o meno buone pratiche di sviluppo_: ad esempio, un framework che incoraggia un'architettura {{Glossary("MVC", "Model-View-Controller")}} per separare il codice in funzioni logiche produrrà codice più manutenibile rispetto a uno che non pone aspettative sugli sviluppatori. Analogamente, la progettazione del framework può avere un grande impatto sulla facilità con cui è possibile testare e riutilizzare il codice.

- **Prestazioni del framework/linguaggio di programmazione:** in genere la "velocità" non è il fattore più importante nella scelta, perché anche runtime relativamente lenti come Python sono più che "sufficienti" per siti di medie dimensioni eseguiti su hardware moderato. I vantaggi percepiti in termini di velocità di un altro linguaggio, ad esempio C++ o JavaScript, potrebbero essere compensati dai costi di apprendimento e manutenzione.
- **Supporto per il caching:** quando il sito web avrà più successo, potrebbe non riuscire più a gestire il numero di richieste ricevute dagli utenti. A quel punto può essere opportuno aggiungere il supporto per il caching. Il caching è un'ottimizzazione che memorizza tutta o parte di una risposta web, in modo che non debba essere ricalcolata nelle richieste successive. Restituire una risposta memorizzata nella cache è molto più veloce che calcolarla inizialmente. Il caching può essere implementato nel codice o nel server (vedere [reverse proxy](https://en.wikipedia.org/wiki/Reverse_proxy)). I framework web offrono diversi livelli di supporto per definire quali contenuti possono essere memorizzati nella cache.
- **Scalabilità:** quando il sito web avrà un successo straordinario, i vantaggi del caching saranno esauriti e si raggiungeranno persino i limiti dello _scaling verticale_ (eseguire l'applicazione web su hardware più potente). A quel punto potrebbe essere necessario effettuare lo _scaling orizzontale_ (condividere il carico distribuendo il sito su più server web e database) oppure scalare "geograficamente", perché alcuni clienti sono molto lontani dal server. Il framework web scelto può fare una grande differenza nella facilità con cui il sito può essere scalato.
- **Sicurezza web:** alcuni framework web offrono un supporto migliore per gestire gli attacchi web comuni. Django, ad esempio, sanitizza tutti gli input degli utenti nei template HTML, impedendo l'esecuzione di JavaScript inserito dagli utenti. Altri framework offrono protezioni simili, ma non sempre sono abilitate per impostazione predefinita.

Esistono molti altri fattori possibili, tra cui la licenza, l'eventuale sviluppo attivo del framework, ecc.

Per chi è un principiante assoluto nella programmazione, il framework sarà probabilmente scelto in base alla "facilità di apprendimento". Oltre alla "facilità d'uso" del linguaggio stesso, documentazione e tutorial di alta qualità, nonché una comunità attiva che aiuti i nuovi utenti, sono le risorse più preziose. Sono stati scelti [Django](https://www.djangoproject.com/) (Python) e [Express](https://expressjs.com/) (Node/JavaScript) per scrivere gli esempi più avanti nel corso, soprattutto perché sono facili da imparare e hanno un buon supporto.

> [!NOTE]
> Visitiamo i siti web principali di [Django](https://www.djangoproject.com/) (Python) e [Express](https://expressjs.com/) (Node/JavaScript) e controlliamo la loro documentazione e comunità.
>
> 1. Passare ai siti principali (collegati sopra).
>    - Fare clic sui collegamenti del menu Documentation (con nomi come "Documentation, Guide, API Reference, Getting Started", ecc.).
>    - Sono presenti argomenti che mostrano come configurare l'instradamento URL, i template e i database/model?
>    - I documenti sono chiari?
> 2. Passare alle mailing list di ciascun sito (accessibili dai collegamenti Community).
>    - Quante domande sono state pubblicate negli ultimi giorni?
>    - Quante hanno ricevuto risposte?
>    - È presente una comunità attiva?

## Alcuni buoni framework web?

Passiamo ora a discutere alcuni framework web lato server specifici.

I framework lato server riportati di seguito rappresentano _alcuni_ dei più popolari disponibili al momento della scrittura. Tutti hanno tutto il necessario per essere produttivi: sono open source, sono in fase di sviluppo attivo, hanno comunità entusiaste che creano documentazione e aiutano gli utenti nei forum di discussione e sono usati da numerosi siti web di alto profilo. Esistono molti altri ottimi framework lato server che è possibile scoprire con una semplice ricerca su internet.

> [!NOTE]
> Le descrizioni provengono (in parte) dai siti web dei framework!

### Django (Python)

[Django](https://www.djangoproject.com/) è un framework web Python di alto livello che incoraggia lo sviluppo rapido e una progettazione pulita e pragmatica. Creato da sviluppatori esperti, si occupa di gran parte delle difficoltà dello sviluppo web, consentendo di concentrarsi sulla scrittura dell'app senza dover reinventare la ruota. È gratuito e open source.

Django segue la filosofia "Batteries included" e fornisce quasi tutto ciò che la maggior parte degli sviluppatori potrebbe voler fare "out of the box". Poiché include tutto, ogni componente funziona insieme agli altri, segue principi di progettazione coerenti e dispone di documentazione ampia e aggiornata. È inoltre veloce, sicuro e molto scalabile. Essendo basato su Python, il codice Django è facile da leggere e mantenere.

Tra i siti popolari che usano Django (dalla home page di Django) figurano: Disqus, Instagram, Knight Foundation, MacArthur Foundation, Mozilla, National Geographic, Open Knowledge Foundation, Pinterest, Open Stack.

### Flask (Python)

[Flask](https://flask.palletsprojects.com/) è un microframework per Python.

Pur essendo minimalista, Flask può creare siti web importanti fin da subito. Contiene un server di sviluppo e un debugger e include il supporto per template [Jinja2](https://github.com/pallets/jinja), cookie sicuri, [unit testing](https://en.wikipedia.org/wiki/Unit_testing) e dispatching di richieste [RESTful](https://restapitutorial.com/). Dispone di una buona documentazione e di una comunità attiva.

Flask è diventato estremamente popolare, in particolare tra gli sviluppatori che devono fornire servizi web su sistemi piccoli e con risorse limitate, ad esempio eseguendo un server web su un [Raspberry Pi](https://www.raspberrypi.org/), [controller per droni](https://www.techuseful.com/drone-definitions-learning-the-drone-lingo/), ecc.

### Express (Node.js/JavaScript)

[Express](https://expressjs.com/) è un framework web veloce, unopinionated, flessibile e minimalista per [Node.js](https://nodejs.org/en/) (node è un ambiente senza browser per eseguire JavaScript). Fornisce un robusto insieme di funzionalità per applicazioni web e mobili e offre metodi di utilità HTTP e {{Glossary("Middleware", "middleware")}} utili.

Express è estremamente popolare, in parte perché facilita la migrazione degli sviluppatori web JavaScript lato client verso lo sviluppo lato server e in parte perché è efficiente nell'uso delle risorse (l'ambiente node sottostante usa multitasking leggero all'interno di un thread anziché generare processi separati per ogni nuova richiesta web).

Poiché Express è un framework web minimalista, non incorpora ogni componente che potrebbe essere utile usare (ad esempio, l'accesso al database e il supporto per utenti e sessioni sono forniti tramite librerie indipendenti). Esistono molti ottimi componenti indipendenti, ma talvolta può essere difficile capire quale sia il migliore per uno scopo specifico.

Molti framework popolari lato server e full stack (che comprendono sia framework lato server sia lato client) sono basati su Express, tra cui [Feathers](https://feathersjs.com/), [ItemsAPI](https://itemsapi.com/), [KeystoneJS](https://keystonejs.com/), [Kraken](https://krakenjs.com/), [LoopBack](https://loopback.io/), [MEAN](https://github.com/linnovate/mean) e [Sails](https://sailsjs.com/).

Molte aziende di alto profilo usano Express, tra cui Uber, Accenture, IBM, ecc.

### Deno (JavaScript)

[Deno](https://deno.com/) è un runtime e framework [JavaScript](/it/docs/Web/JavaScript)/TypeScript semplice, moderno e sicuro, costruito su Chrome V8 e [Rust](https://rust-lang.org/).

Deno è alimentato da [Tokio](https://tokio.rs/), un runtime asincrono basato su Rust che consente di servire pagine web più velocemente. Dispone inoltre di supporto interno per [WebAssembly](/it/docs/WebAssembly), che abilita la compilazione di codice binario da usare lato client. Deno mira a colmare alcune delle lacune di [Node.js](/it/docs/Learn_web_development/Extensions/Server-side/Node_server_without_framework) fornendo un meccanismo che mantiene naturalmente una maggiore sicurezza.

Le caratteristiche di Deno includono:

- Sicurezza per impostazione predefinita. I [moduli Deno limitano le autorizzazioni](https://docs.deno.com/runtime/fundamentals/security/) all'accesso a **file**, **rete** o **ambiente**, salvo autorizzazione esplicita.
- Supporto TypeScript **out-of-the-box**.
- Meccanismo await di prima classe.
- Strumento integrato per i test e formattatore di codice (`deno fmt`).
- Compatibilità del browser (JavaScript): i programmi Deno scritti completamente in JavaScript, escludendo lo spazio dei nomi `Deno` (o che ne eseguono un feature test), dovrebbero funzionare direttamente in qualsiasi browser moderno.
- Bundling degli script in un singolo file JavaScript.

Deno fornisce un modo semplice ma potente per usare JavaScript sia per la programmazione lato client sia lato server.

### Ruby on Rails (Ruby)

[Rails](https://rubyonrails.org/) (solitamente chiamato "Ruby on Rails") è un framework web scritto per il linguaggio di programmazione Ruby.

Rails segue una filosofia di progettazione molto simile a quella di Django. Come Django, fornisce meccanismi standard per l'instradamento degli URL, l'accesso ai dati da un database, la generazione di HTML dai template e la formattazione dei dati come {{Glossary("JSON", "JSON")}} o {{Glossary("XML", "XML")}}. Incoraggia analogamente l'uso di pattern di progettazione come DRY ("don't repeat yourself" — scrivere il codice una sola volta, se possibile), MVC (model-view-controller) e molti altri.

Naturalmente esistono molte differenze dovute a decisioni progettuali specifiche e alla natura dei linguaggi.

Rails è stato usato per siti di alto profilo, tra cui: [Basecamp](https://basecamp.com/), [GitHub](https://github.com/), [Shopify](https://www.shopify.com/), [Airbnb](https://www.airbnb.com/), [Twitch](https://www.twitch.tv/), [SoundCloud](https://soundcloud.com/), [Hulu](https://www.hulu.com/welcome), [Zendesk](https://www.zendesk.com/), [Square](https://squareup.com/us/en).

### Laravel (PHP)

[Laravel](https://laravel.com/) è un framework per applicazioni web con una sintassi espressiva ed elegante. Laravel cerca di eliminare le difficoltà dallo sviluppo semplificando attività comuni utilizzate nella maggior parte dei progetti web, quali:

- [Motore di routing semplice e veloce](https://laravel.com/docs/routing).
- [Potente container per dependency injection](https://laravel.com/docs/container).
- Più back-end per l'archiviazione di [sessioni](https://laravel.com/docs/session) e [cache](https://laravel.com/docs/cache).
- [ORM per database](https://laravel.com/docs/eloquent) espressivo e intuitivo.
- [Migrazioni dello schema](https://laravel.com/docs/migrations) indipendenti dal database.
- [Elaborazione robusta di job in background](https://laravel.com/docs/queues).
- [Trasmissione di eventi in tempo reale](https://laravel.com/docs/broadcasting).

Laravel è accessibile ma potente e fornisce gli strumenti necessari per applicazioni grandi e robuste.

### ASP.NET

[ASP.NET](https://dotnet.microsoft.com/en-us/apps/aspnet) è un framework web open source sviluppato da Microsoft per creare moderne applicazioni e servizi web. Con ASP.NET è possibile creare rapidamente siti web basati su HTML, CSS e JavaScript, scalarli per l'uso da parte di milioni di utenti e aggiungere facilmente funzionalità più complesse come Web API, moduli sui dati o comunicazioni in tempo reale.

Uno degli elementi distintivi di ASP.NET è che è costruito sul [Common Language Runtime](https://en.wikipedia.org/wiki/Common_Language_Runtime) (CLR), consentendo ai programmatori di scrivere codice ASP.NET usando qualsiasi linguaggio .NET supportato (C#, Visual Basic, ecc.). Come molti prodotti Microsoft, trae vantaggio da strumenti eccellenti (spesso gratuiti), una comunità di sviluppatori attiva e documentazione ben scritta.

ASP.NET è usato da Microsoft, Xbox.com, Stack Overflow e molti altri.

### Mojolicious (Perl)

[Mojolicious](https://mojolicious.org/) è un framework web di nuova generazione per il linguaggio di programmazione Perl.

Agli albori del web, molte persone impararono Perl grazie a una straordinaria libreria Perl chiamata [CGI](https://metacpan.org/pod/CGI). Era abbastanza semplice da permettere di iniziare senza conoscere molto del linguaggio e abbastanza potente da consentire di proseguire. Mojolicious implementa questa idea usando tecnologie all'avanguardia.

Alcune delle funzionalità offerte da Mojolicious sono:

- Un framework web in tempo reale, per far crescere facilmente prototipi in un singolo file fino a ottenere applicazioni web MVC ben strutturate.
- Route RESTful, plugin, comandi, template in stile Perl, negoziazione dei contenuti, gestione delle sessioni, convalida dei moduli, framework di test, server di file statici, rilevamento CGI/[PSGI](https://plackperl.org/) e supporto Unicode di prima classe.
- Un'implementazione completa di client/server HTTP e WebSocket con supporto per IPv6, TLS, SNI, IDNA, proxy HTTP/SOCKS5, socket di dominio UNIX, Comet (long polling), keep-alive, connection pooling, timeout, cookie, multipart e compressione gzip.
- Parser e generatori JSON e HTML/XML con supporto per selettori CSS.
- API pure-Perl molto pulita, portabile e orientata agli oggetti, senza magia nascosta.
- Codice recente basato su anni di esperienza, gratuito e open source.

### Spring Boot (Java)

[Spring Boot](https://spring.io/projects/spring-boot/) è uno dei numerosi progetti forniti da [Spring](https://spring.io/). È un buon punto di partenza per lo sviluppo web lato server usando [Java](https://www.java.com/).

Sebbene non sia certamente l'unico framework basato su [Java](https://www.java.com/), è semplice da usare per creare applicazioni standalone di livello produttivo basate su Spring che è possibile "semplicemente eseguire". Offre una visione opinionated della piattaforma Spring e delle librerie di terze parti, ma consente di iniziare con il minimo sforzo e configurazione.

Può essere usato per piccoli problemi, ma il suo punto di forza è la creazione di applicazioni su larga scala che adottano un approccio cloud. Solitamente, più applicazioni vengono eseguite in parallelo e comunicano tra loro: alcune forniscono interazione con l'utente e altre svolgono lavoro di back-end, ad esempio accedendo a database o altri servizi. I bilanciatori di carico aiutano a garantire ridondanza e affidabilità oppure consentono la gestione geolocalizzata delle richieste degli utenti per assicurare reattività.

## Riepilogo

Questo articolo ha mostrato che i framework web possono rendere più semplice lo sviluppo e la manutenzione del codice lato server. Ha inoltre fornito una panoramica di alto livello di alcuni framework popolari e ha discusso i criteri per scegliere un framework per applicazioni web. A questo punto dovrebbe essere disponibile almeno un'idea di come scegliere un framework web per il proprio sviluppo lato server. In caso contrario, non c'è da preoccuparsi: più avanti nel corso verranno forniti tutorial dettagliati su Django ed Express per fare esperienza concreta di lavoro con un framework web.

Nel prossimo articolo di questo modulo la direzione cambierà leggermente per considerare la sicurezza web.

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/First_steps/Client-Server_overview", "Learn_web_development/Extensions/Server-side/First_steps/Website_security", "Learn_web_development/Extensions/Server-side/First_steps")}}
