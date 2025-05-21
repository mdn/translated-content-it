---
title: Framework web lato server
short-title: Framework lato server
slug: Learn_web_development/Extensions/Server-side/First_steps/Web_frameworks
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/First_steps/Client-Server_overview", "Learn_web_development/Extensions/Server-side/First_steps/Website_security", "Learn_web_development/Extensions/Server-side/First_steps")}}

L'articolo precedente ti ha mostrato come appare la comunicazione tra client e server web, la natura delle richieste e delle risposte HTTP, e cosa deve fare un'applicazione web lato server per rispondere alle richieste di un browser web. Con questa conoscenza, è il momento di esplorare come i framework web possano semplificare questi compiti e darti un'idea su come scegliere un framework per la tua prima applicazione web lato server.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Comprensione di base di come il codice lato server gestisce e risponde alle richieste HTTP (vedi <a
          href="/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Client-Server_overview"
          >Panoramica Client-Server</a
        >).
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere come i framework web possano semplificare lo sviluppo/la manutenzione del codice lato server e far
        riflettere i lettori sulla scelta di un framework per il proprio sviluppo.
      </td>
    </tr>
  </tbody>
</table>

Le sezioni seguenti illustrano alcuni punti usando frammenti di codice presi da framework web reali. Non preoccuparti se tutto ciò non ha **tutto** senso adesso; lavoreremo attraverso il codice nei nostri moduli specifici del framework.

## Panoramica

I framework web lato server (noti anche come "framework di applicazioni web") sono framework software che rendono più facile scrivere, mantenere ed espandere le applicazioni web. Forniscono strumenti e librerie che semplificano le operazioni comuni nello sviluppo web, inclusi l'instradamento degli URL agli handler appropriati, l'interazione con i database, il supporto per le sessioni e l'autorizzazione degli utenti, la formattazione dell'output (ad esempio, HTML, JSON, XML) e il miglioramento della sicurezza contro gli attacchi web.

La sezione successiva fornisce un po' più di dettagli su come i framework web possano facilitare lo sviluppo delle applicazioni web. Successivamente, spiegheremo alcuni dei criteri che puoi utilizzare per scegliere un framework web e poi elencheremo alcune delle tue opzioni.

## Cosa può fare un framework web per te?

I framework web forniscono strumenti e librerie per semplificare le operazioni comuni nello sviluppo web. Non _devi_ usare un framework web lato server, ma è fortemente consigliato — renderà la tua vita molto più semplice.

Questa sezione discute alcune delle funzionalità che spesso sono fornite dai framework web (non tutti i framework necessariamente forniranno tutte queste funzionalità!).

### Lavorare direttamente con le richieste e le risposte HTTP

Come abbiamo visto nell'ultimo articolo, i server web e i browser comunicano tramite il protocollo HTTP — i server attendono le richieste HTTP dal browser e quindi restituiscono informazioni nelle risposte HTTP. I framework web ti permettono di scrivere una sintassi semplificata che genererà codice lato server per lavorare con queste richieste e risposte. Ciò significa che avrai un compito più facile, interagendo con codice più semplice e ad alto livello piuttosto che con primitive di rete a basso livello.

L'esempio seguente mostra come funziona questo nel framework web Django (Python). Ogni funzione "view" (un handler di richiesta) riceve un oggetto `HttpRequest` contenente informazioni sulla richiesta e deve restituire un oggetto `HttpResponse` con l'output formattato (in questo caso una stringa).

```python
# Django view function
from django.http import HttpResponse

def index(request):
    # Get an HttpRequest (request)
    # perform operations using information from the request.
    # Return HttpResponse
    return HttpResponse('Output string to return')
```

### Instradare le richieste all'handler appropriato

La maggior parte dei siti fornirà un numero di risorse diverse, accessibili tramite URL distinti. Gestire tutte queste risorse con una singola funzione sarebbe difficile da mantenere, quindi i framework web forniscono meccanismi semplici per mappare i modelli di URL a funzioni di handler specifiche. Questo approccio ha anche vantaggi in termini di manutenzione, perché puoi cambiare l'URL utilizzato per fornire una particolare funzionalità senza dover cambiare il codice sottostante.

Diversi framework utilizzano meccanismi diversi per la mappatura. Ad esempio, il framework web Flask (Python) aggiunge percorsi alle funzioni di visualizzazione utilizzando un decoratore.

```python
@app.route("/")
def hello():
    return "Hello World!"
```

Mentre Django richiede agli sviluppatori di definire un elenco di mappature di URL tra un modello di URL e una funzione di visualizzazione.

```python
urlpatterns = [
    url(r'^$', views.index),
    # example: /best/my_team_name/5/
    url(r'^best/(?P<team_name>\w+?)/(?P<team_number>[0-9]+)/$', views.best),
]
```

### Facilitare l'accesso ai dati nella richiesta

I dati possono essere codificati in una richiesta HTTP in diversi modi. Una richiesta HTTP `GET` per ottenere file o dati dal server può codificare quali dati sono richiesti nei parametri URL o all'interno della struttura URL. Una richiesta HTTP `POST` per aggiornare una risorsa sul server includerà invece le informazioni di aggiornamento come "dati POST" nel corpo della richiesta. La richiesta HTTP può anche includere informazioni sulla sessione corrente o sull'utente in un cookie lato client.

I framework web forniscono meccanismi appropriati nella programmazione per accedere a queste informazioni. Ad esempio, l'oggetto `HttpRequest` che Django passa a ogni funzione di visualizzazione contiene metodi e proprietà per accedere all'URL di destinazione, al tipo di richiesta (ad esempio, un HTTP `GET`), ai parametri `GET` o `POST`, ai dati dei cookie e delle sessioni, ecc. Django può anche passare le informazioni codificate nella struttura dell'URL definendo "modelli di cattura" nel mapper degli URL (vedi l'ultimo frammento di codice nella sezione sopra).

### Astrazione e semplificazione dell'accesso al database

I siti web usano i database per memorizzare informazioni sia da condividere con gli utenti che sugli utenti stessi. I framework web spesso forniscono un livello di database che astrae le operazioni di lettura, scrittura, query e cancellazione del database. Questo livello di astrazione è indicato come un Object-Relational Mapper (ORM).

Usare un ORM ha due vantaggi:

- Puoi sostituire il database sottostante senza necessariamente dover cambiare il codice che lo utilizza. Questo permette agli sviluppatori di ottimizzare per le caratteristiche di diversi database in base al loro utilizzo.
- La validazione di base dei dati può essere implementata all'interno del framework. Ciò rende più facile e sicuro verificare che i dati siano archiviati nel corretto tipo di campo del database, abbiano il formato corretto (ad esempio, un indirizzo email) e non siano in alcun modo dannosi (gli hacker possono utilizzare certi pattern di codice per fare cose maligne come eliminare record del database).

Per esempio, il framework web Django fornisce un ORM, e fa riferimento all'oggetto usato per definire la struttura di un record come il _modello_. Il modello specifica i tipi di campo da archiviare, che possono fornire una validazione a livello di campo su quali informazioni possono essere archiviate (ad esempio, un campo email permetterebbe solo indirizzi email validi). Le definizioni dei campi possono anche specificare la loro dimensione massima, valori predefiniti, opzioni della lista di selezione, testo di aiuto per la documentazione, testo dell'etichetta per i form, ecc. Il modello non dichiara alcuna informazione sul database sottostante in quanto è una impostazione di configurazione che può essere modificata separatamente dal nostro codice.

Il primo frammento di codice qui sotto mostra un modello Django molto semplice per un oggetto `Team`. Questo memorizza il nome della squadra e il livello della squadra come campi di caratteri e specifica un numero massimo di caratteri da archiviare per ogni record. Il `team_level` è un campo di scelta, quindi forniamo anche una mappatura tra le scelte da visualizzare e i dati da archiviare, insieme a un valore predefinito.

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

Il modello Django fornisce un'API di query semplice per cercare nel database. Questo può corrispondere a un numero di campi alla volta usando diversi criteri (ad esempio, esatto, case-insensitive, maggiore di, ecc.), e può supportare dichiarazioni complesse (ad esempio, puoi specificare una ricerca su squadre U11 che hanno un nome di squadra che inizia con "Fr" o finisce con "al").

Il secondo frammento di codice mostra una funzione di visualizzazione (gestore di risorse) per visualizzare tutte le nostre squadre U09. In questo caso specifichiamo che vogliamo filtrare per tutti i record dove il campo `team_level` ha esattamente il testo 'U09' (nota qui sotto come questo criterio sia passato alla funzione `filter()` come argomento con il nome del campo e il tipo di corrispondenza separati da doppio underscore: **team_level\_\_exact**).

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

I framework web includono spesso sistemi di templating. Questi ti permettono di specificare la struttura di un documento di output, usando segnaposto per i dati che verranno aggiunti quando una pagina viene generata. I template sono spesso usati per creare HTML, ma possono anche creare altri tipi di documenti.

I framework web forniscono spesso un meccanismo per rendere facile generare altri formati dai dati archiviati, compresi {{Glossary("JSON", "JSON")}} e {{Glossary("XML", "XML")}}.

Per esempio, il sistema di template di Django ti permette di specificare variabili utilizzando una sintassi a "doppio manico" (ad esempio, `\{{ variable_name }}`), che verrà sostituita da valori passati dalla funzione di visualizzazione quando una pagina viene renderizzata. Il sistema di template offre anche supporto per espressioni (con sintassi: `{% expression %}`), che permettono ai template di eseguire operazioni semplici come iterare valori di lista passati nel template.

> [!NOTE]
> Molti altri sistemi di templating usano una sintassi simile, ad esempio: Jinja2 (Python), handlebars (JavaScript), moustache (JavaScript), ecc.

Il frammento di codice seguente mostra come funziona. Continuando l'esempio della "squadra più giovane" dalla sezione precedente, il template HTML riceve una variabile di lista chiamata `youngest_teams` dalla vista. All'interno dello scheletro HTML abbiamo un'espressione che prima verifica se la variabile `youngest_teams` esiste e poi la itera in un ciclo `for`. Ad ogni iterazione il template visualizza il valore `team_name` della squadra in un elemento di lista.

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

Esistono numerosi framework web per quasi ogni linguaggio di programmazione che potresti voler utilizzare (elenchiamo alcuni dei framework più popolari nella sezione seguente). Con così tante scelte, può diventare difficile capire quale framework fornisca il miglior punto di partenza per la tua nuova applicazione web.

Alcuni dei fattori che possono influenzare la tua decisione sono:

- **Sforzo di apprendimento:** Lo sforzo per apprendere un framework web dipende da quanto sei familiare con il linguaggio di programmazione sottostante, dalla coerenza della sua API, dalla qualità della sua documentazione, e dalla dimensione e attività della sua comunità. Se parti senza alcuna esperienza di programmazione considera Django (è uno dei più facili da apprendere in base ai criteri sopra menzionati). Se fai parte di un team di sviluppo che ha già una significativa esperienza con un particolare framework web o linguaggio di programmazione, allora ha senso restare con quello.
- **Produttività:** La produttività è una misura di quanto velocemente puoi creare nuove funzionalità una volta che sei familiare con il framework e comprende sia lo sforzo per scrivere che per mantenere il codice (dato che non puoi scrivere nuove funzionalità mentre le vecchie sono rotte). Molti dei fattori che influenzano la produttività sono simili a quelli per "Sforzo di apprendimento" — ad esempio, documentazione, comunità, esperienza di programmazione, ecc. — altri fattori includono:

  - _Scopo/origine del framework_: Alcuni framework web sono stati inizialmente creati per risolvere certi tipi di problemi, e rimangono _migliori_ nel creare app web con simili vincoli. Ad esempio, Django è stato creato per supportare lo sviluppo di un sito web di un giornale, quindi è buono per i blog e altri siti che coinvolgono la pubblicazione di cose. Invece, Flask è un framework molto più leggero ed è ottimo per creare app web che funzionano su dispositivi integrati.
  - _Opinate vs. non opinate_: Un framework opinato è uno in cui ci sono modi "migliori" raccomandati per risolvere un problema particolare. I framework opinati tendono a essere più produttivi quando cerchi di risolvere problemi comuni, perché ti indirizzano nella direzione giusta, tuttavia a volte sono meno flessibili.
  - _Batterie incluse vs. arrangiarsi_: Alcuni framework web includono strumenti/librerie che rispondono a ogni problema che i loro sviluppatori possono pensare "di default", mentre framework più leggeri si aspettano che gli sviluppatori web scelgano e trovino soluzioni a problemi da librerie separate (Django è un esempio del primo, mentre Flask è un esempio di un framework molto leggero). I framework che includono tutto sono spesso più facili da iniziare perché hanno già tutto ciò di cui hai bisogno, e le probabilità sono che sia ben integrato e ben documentato. Tuttavia, se un framework più piccolo ha tutto ciò di cui hai (avrai mai) bisogno, può funzionare in ambienti più limitati e avrà un sottoinsieme più piccolo e facile di cose da apprendere.
  - _Se il framework incoraggia o meno buone pratiche di sviluppo_: Ad esempio, un framework che incoraggia un'architettura {{Glossary("MVC", "Model-View-Controller")}} per separare il codice in funzioni logiche risulterà in codice più mantenibile rispetto a uno che non ha aspettative sugli sviluppatori. Allo stesso modo, il design del framework può avere un grande impatto su quanto sia facile testare e riutilizzare il codice.

- **Performance del framework/language di programmazione:** Solitamente la "velocità" non è il fattore più importante nella selezione perché anche runtime relativamente lenti come Python sono più che "sufficientemente buoni" per siti di medie dimensioni che funzionano su hardware moderato. I benefici percepiti in termini di velocità di un altro linguaggio, ad esempio C++ o JavaScript, possono benissimo essere compensati dai costi di apprendimento e manutenzione.
- **Supporto per la cache:** Man mano che il tuo sito diventa più popolare, potresti scoprire che non riesce più a gestire il numero di richieste che sta ricevendo man mano che gli utenti vi accedono. A questo punto, potresti considerare l'aggiunta del supporto per la memorizzazione nella cache. La memorizzazione nella cache è un'ottimizzazione in cui memorizzi tutta o una parte di una risposta web in modo tale da non dover essere ricalcolata per le richieste successive. Restituire una risposta memorizzata nella cache è molto più veloce che calcolarne una inizialmente. La memorizzazione nella cache può essere implementata nel tuo codice o nel server (vedi [reverse proxy](https://en.wikipedia.org/wiki/Reverse_proxy)). I framework web hanno diversi livelli di supporto per definire quale contenuto può essere memorizzato nella cache.
- **Scalabilità:** Una volta che il tuo sito web è enormemente successo, esaurirai i benefici della memorizzazione nella cache e raggiungerai persino i limiti del _vertical scaling_ (eseguire la tua applicazione web su hardware più potente). A questo punto potresti dover _scalare orizzontalmente_ (condividere il carico distribuendo il tuo sito su diversi web server e database) o scalare "geograficamente" perché alcuni dei tuoi clienti sono basati molto lontano dal tuo server. Il framework web che scegli può fare una grande differenza su quanto sia facile scalare il tuo sito.
- **Sicurezza web:** Alcuni framework web forniscono un miglior supporto per gestire attacchi web comuni. Ad esempio, Django sana automaticamente tutta l'input utente dai template HTML in modo che lo JavaScript inserito dagli utenti non possa essere eseguito. Altri framework forniscono protezioni simili, ma non sempre sono abilitati di default.

Ci sono molti altri possibili fattori, inclusi licenza, se il framework è in attivo sviluppo, ecc.

Se sei un principiante assoluto nella programmazione, probabilmente sceglierai il tuo framework in base alla "facilità di apprendimento". In aggiunta alla "facilità d'uso" del linguaggio stesso, documentazione di qualità e una comunità attiva che aiuta i nuovi utenti sono le tue risorse più preziose. Abbiamo scelto [Django](https://www.djangoproject.com/) (Python) e [Express](https://expressjs.com/) (Node/JavaScript) per scrivere i nostri esempi più avanti nel corso, principalmente perché sono facili da imparare e hanno un buon supporto.

> [!NOTE]
> Andiamo ai siti principali di [Django](https://www.djangoproject.com/) (Python) e [Express](https://expressjs.com/) (Node/JavaScript) e controlliamo la loro documentazione e comunità.
>
> 1. Naviga verso i siti principali (linkati sopra)
>
> - Clicca sui link del menu dedicati alla Documentazione (nome come "Documentazione, Guida, API Reference, Getting Started", ecc.).
> - Puoi vedere argomenti che mostrano come configurare l'instradamento degli URL, i template e i database/modelli?
> - I documenti sono chiari?
>
> 2. Naviga verso le mailing list per ogni sito (accessibili dai link Comunità).
>
> - Quante domande sono state postate negli ultimi giorni?
> - Quante hanno ricevuto risposte?
> - Hanno una comunità attiva?

## Alcuni buoni framework web?

Passiamo ora a discutere di alcuni specifici framework web lato server.

I framework lato server seguenti rappresentano _alcuni_ dei più popolari disponibili al momento della scrittura. Tutti hanno tutto ciò di cui hai bisogno per essere produttivo — sono open source, sono sotto attivo sviluppo, hanno comunità entusiaste che creano documentazione e aiutano gli utenti nei forum, e sono usati in un gran numero di siti web di alto profilo. Ci sono molti altri ottimi framework lato server che puoi scoprire con una semplice ricerca su internet.

> [!NOTE]
> Le descrizioni provengono (parzialmente) dai siti web dei framework!

### Django (Python)

[Django](https://www.djangoproject.com/) è un framework web Python ad alto livello che incoraggia lo sviluppo rapido e un design pulito e pragmatico. Costruito da sviluppatori esperti, si occupa di gran parte delle difficoltà dello sviluppo web, così puoi concentrarti sulla scrittura della tua app senza dover reinventare la ruota. È gratuito e open source.

Django segue la filosofia delle "Batterie incluse" e fornisce quasi tutto ciò che la maggior parte degli sviluppatori potrebbe voler fare "fuori dal comportamento predefinito". Poiché tutto è incluso, funziona tutto insieme, segue principi di design coerenti e ha una documentazione estesa e aggiornata. È anche veloce, sicuro e molto scalabile. Basato su Python, il codice Django è facile da leggere e mantenere.

I siti popolari che usano Django (dalla pagina principale di Django) includono: Disqus, Instagram, Knight Foundation, MacArthur Foundation, Mozilla, National Geographic, Open Knowledge Foundation, Pinterest, Open Stack.

### Flask (Python)

[Flask](https://flask.palletsprojects.com/) è un microframework per Python.

Sebbene minimalista, Flask può creare siti web seri sin da subito. Contiene un server di sviluppo e un debugger, e include supporto per il templating [Jinja2](https://github.com/pallets/jinja), cookie sicuri, [unit testing](https://en.wikipedia.org/wiki/Unit_testing) e smistamento delle richieste [RESTful](https://restapitutorial.com/). Ha una buona documentazione e una comunità attiva.

Flask è diventato estremamente popolare, in particolare per gli sviluppatori che devono fornire servizi web su sistemi piccoli e con risorse limitate (ad esempio, eseguire un server web su un [Raspberry Pi](https://www.raspberrypi.org/), [controller di droni](https://www.techuseful.com/drone-definitions-learning-the-drone-lingo/), ecc.)

### Express (Node.js/JavaScript)

[Express](https://expressjs.com/) è un framework web veloce, flessibile e minimalista per [Node.js](https://nodejs.org/en/) (node è un ambiente senza browser per eseguire JavaScript). Fornisce un set robusto di funzionalità per applicazioni web e mobili e offre utili metodi per le utilità HTTP e {{Glossary("Middleware", "middleware")}}.

Express è estremamente popolare, in parte perché facilita la migrazione di programmatori di JavaScript lato client nello sviluppo lato server, e in parte perché è efficiente nelle risorse (l'ambiente sottostante di node utilizza multitasking leggero all'interno di un thread piuttosto che generare processi separati per ogni nuova richiesta web).

Poiché Express è un framework web minimalista non incorpora ogni componente che potresti voler usare (ad esempio, l'accesso al database e il supporto per utenti e sessioni sono forniti attraverso librerie indipendenti). Ci sono molti eccellenti componenti indipendenti, ma a volte può essere difficile capire quale sia il migliore per un particolare scopo!

Molti popolari framework lato server e full stack (composti sia da framework server che client-side) si basano su Express, tra cui [Feathers](https://feathersjs.com/), [ItemsAPI](https://itemsapi.com/), [KeystoneJS](https://keystonejs.com/), [Kraken](https://krakenjs.com/), [LoopBack](https://loopback.io/), [MEAN](https://github.com/linnovate/mean), e [Sails](https://sailsjs.com/).

Molte aziende di alto profilo usano Express, inclusi: Uber, Accenture, IBM, ecc.

### Deno (JavaScript)

[Deno](https://deno.com/) è un runtime e framework semplice, moderno e sicuro per [JavaScript](/it/docs/Web/JavaScript)/TypeScript costruito sopra a Chrome V8 e [Rust](https://www.rust-lang.org/).

Deno è alimentato da [Tokio](https://tokio.rs/) — un runtime asincrono basato su Rust che gli permette di servire pagine web più velocemente. Ha anche supporto interno per [WebAssembly](/it/docs/WebAssembly), che abilita la compilazione di codice binario per l'uso lato client. Deno mira a colmare alcune delle lacune in [Node.js](/it/docs/Learn_web_development/Extensions/Server-side/Node_server_without_framework) fornendo un meccanismo che mantiene naturalmente una sicurezza migliore.

Le funzionalità di Deno includono:

- Sicurezza di default. [I moduli Deno limitano le autorizzazioni](https://docs.deno.com/runtime/fundamentals/security/) per l'accesso a **file**, **rete** o **ambiente**, a meno che non sia esplicitamente consentito.
- Supporto TypeScript **out-of-the-box**.
- Meccanismo await di primo livello.
- Funzionalità di test integrata e formattatore di codice (`deno fmt`)
- Compatibilità con il Browser per il JavaScript: i programmi Deno scritti completamente in JavaScript escludendo lo spazio dei nomi `Deno` (o controllandone la presenza), dovrebbero funzionare direttamente in qualsiasi browser moderno.
- Bundling di script in un unico file JavaScript.

Deno fornisce un modo facile ma potente per utilizzare JavaScript sia per la programmazione lato client che lato server.

### Ruby on Rails (Ruby)

[Rails](https://rubyonrails.org/) (solitamente chiamato "Ruby on Rails") è un framework web scritto per il linguaggio di programmazione Ruby.

Rails segue una filosofia di design molto simile a Django. Come Django, fornisce meccanismi standard per l'instradamento degli URL, l'accesso ai dati da un database, la generazione di HTML dai template e la formattazione dei dati come {{Glossary("JSON", "JSON")}} o {{Glossary("XML", "XML")}}. Incoraggia simile l'uso di pattern di design come DRY ("don't repeat yourself" — scrivere il codice una volta sola se possibile), MVC (model-view-controller) e una serie di altri.

Naturalmente ci sono molte differenze dovute a decisioni di design specifiche e alla natura dei linguaggi.

Rails è stato utilizzato per siti di alto profilo, tra cui: [Basecamp](https://basecamp.com/), [GitHub](https://github.com/), [Shopify](https://www.shopify.com/), [Airbnb](https://www.airbnb.com/), [Twitch](https://www.twitch.tv/), [SoundCloud](https://soundcloud.com/), [Hulu](https://www.hulu.com/welcome), [Zendesk](https://www.zendesk.com/), [Square](https://squareup.com/us/en), [Highrise](https://highrisehq.com/).

### Laravel (PHP)

[Laravel](https://laravel.com/) è un framework per applicazioni web con una sintassi espressiva ed elegante. Laravel cerca di togliere il dolore dal processo di sviluppo facilitando compiti comuni utilizzati nella maggioranza dei progetti web, come:

- [Motore di routing semplice e veloce](https://laravel.com/docs/routing).
- [Potente contenitore di iniezione delle dipendenze](https://laravel.com/docs/container).
- Multiple back-end per lo [storage delle sessioni](https://laravel.com/docs/session) e [cache](https://laravel.com/docs/cache).
- [ORM per database espressivo e intuitivo](https://laravel.com/docs/eloquent).
- [Migrazioni dello schema del database agnostiche](https://laravel.com/docs/migrations).
- [Elaborazione dei lavori in background robusta](https://laravel.com/docs/queues).
- [Trasmissione di eventi in tempo reale](https://laravel.com/docs/broadcasting).

Laravel è accessibile, ma potente, offrendo gli strumenti necessari per applicazioni grandi e robuste.

### ASP.NET

[ASP.NET](https://dotnet.microsoft.com/en-us/apps/aspnet) è un framework web open source sviluppato da Microsoft per costruire applicazioni e servizi web moderni. Con ASP.NET puoi creare rapidamente siti web basati su HTML, CSS e JavaScript, scalarli per l'uso da parte di milioni di utenti e aggiungere facilmente funzionalità più complesse come API Web, form sui dati, o comunicazioni in tempo reale.

Uno dei differenziatori per ASP.NET è che è costruito sul [Common Language Runtime](https://en.wikipedia.org/wiki/Common_Language_Runtime) (CLR), permettendo ai programmatori di scrivere codice ASP.NET utilizzando qualsiasi linguaggio .NET supportato (C#, Visual Basic, ecc.). Come molti prodotti Microsoft, beneficia di strumenti eccellenti (spesso gratuiti), di una comunità di sviluppatori attiva e di una documentazione ben scritta.

ASP.NET è utilizzato da Microsoft, Xbox.com, Stack Overflow e molti altri.

### Mojolicious (Perl)

[Mojolicious](https://mojolicious.org/) è un framework web di nuova generazione per il linguaggio di programmazione Perl.

Nei primi giorni del web, molte persone hanno imparato Perl grazie a una meravigliosa libreria Perl chiamata [CGI](https://metacpan.org/pod/CGI). Era sufficientemente semplice per iniziare senza sapere molto sul linguaggio e abbastanza potente per continuare. Mojolicious implementa questa idea utilizzando tecnologie all'avanguardia.

Alcune delle funzionalità fornite da Mojolicious sono:

- Un framework web in tempo reale, per far crescere facilmente prototipi singoli-file in applicazioni web MVC ben strutturate.
- Routing RESTful, plugin, comandi, template in stile Perl, negoziazione del contenuto, gestione delle sessioni, validazione dei form, framework di testing, server di file statici, rilevamento CGI/[PSGI](https://plackperl.org/) e supporto Unicode di primo livello.
- Una implementazione client/server HTTP e WebSocket full-stack con supporto IPv6, TLS, SNI, IDNA, proxy HTTP/SOCKS5, socket di dominio UNIX, Comet (long polling), keep-alive, pooling delle connessioni, timeout, cookie, multipart e compressione gzip.
- Parser e generatori di JSON e HTML/XML con supporto per selettori CSS.
- API pulita, portatile e orientata agli oggetti di puro-Perl senza magia nascosta.
- Codice fresco basato su anni di esperienza, gratuito e open-source.

### Spring Boot (Java)

[Spring Boot](https://spring.io/projects/spring-boot/) è uno dei numerosi progetti offerti da [Spring](https://spring.io/). È un buon punto di partenza per fare sviluppo web lato server utilizzando [Java](https://www.java.com/).

Anche se non è sicuramente l'unico framework basato su [Java](https://www.java.com/) è facile da usare per creare applicazioni standalone, pronte per la produzione, basate su Spring che puoi semplicemente "eseguire". Offre una visione opinata della piattaforma Spring e delle librerie di terze parti ma permette di iniziare con il minimo sforzo e configurazione.

Può essere utilizzato per piccoli problemi ma la sua forza è costruire applicazioni di grandi dimensioni che utilizzano un approccio cloud. Solitamente più applicazioni vengono eseguite in parallelo parlando tra di loro, con alcune forniscono l'interazione utente e altre che svolgono lavori di backend (ad esempio, l'accesso ai database o ad altri servizi). I bilanciatori del carico aiutano a garantire ridondanza e affidabilità o permettono la gestione geolocalizzata delle richieste utenti per garantire la reattività.

## Sommario

Questo articolo ha dimostrato che i framework web possono rendere più facile sviluppare e mantenere codice lato server. Ha inoltre fornito una panoramica ad alto livello di alcuni framework popolari e discusso criteri per scegliere un framework di applicazione web. Dovresti ora avere almeno un'idea di come scegliere un framework web per il tuo sviluppo lato server. Se no, non preoccuparti — più avanti nel corso ti insegneremo dettagliati tutorial su Django e Express per darti qualche esperienza di lavoro effettivo con un framework web.

Per l'articolo successivo in questo modulo cambieremo leggermente direzione e considereremo la sicurezza web.

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/First_steps/Client-Server_overview", "Learn_web_development/Extensions/Server-side/First_steps/Website_security", "Learn_web_development/Extensions/Server-side/First_steps")}}
