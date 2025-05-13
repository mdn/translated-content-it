---
title: Panoramica client-server
slug: Learn_web_development/Extensions/Server-side/First_steps/Client-Server_overview
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/First_steps/Introduction", "Learn_web_development/Extensions/Server-side/First_steps/Web_frameworks", "Learn_web_development/Extensions/Server-side/First_steps")}}

Ora che conosce lo scopo e i potenziali vantaggi della programmazione lato server, esamineremo in dettaglio cosa succede quando un server riceve una "richiesta dinamica" da un browser. Poiché la maggior parte del codice lato server dei siti web gestisce le richieste e le risposte in modi simili, ciò la aiuterà a capire cosa deve fare quando scrive la maggior parte del suo codice.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Una comprensione di base di cosa sia un server web.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere le interazioni client-server in un sito web dinamico e in particolare quali operazioni devono essere eseguite dal codice lato server.
      </td>
    </tr>
  </tbody>
</table>

Non vi è alcun codice reale nella discussione perché non abbiamo ancora scelto un framework web da utilizzare per scrivere il nostro codice! Tuttavia, questa discussione è ancora molto rilevante, perché il comportamento descritto deve essere implementato dal codice lato server, indipendentemente dal linguaggio di programmazione o dal framework web che si sceglie.

## Server web e HTTP (una introduzione)

I browser web comunicano con i [server web](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_web_server) utilizzando il **P**rotocollo di **T**rasferimento di **I**per**T**esto ([HTTP](/it/docs/Web/HTTP)). Quando clicca su un link in una pagina web, invia un modulo o esegue una ricerca, il browser invia una _richiesta HTTP_ al server.

Questa richiesta include:

- Un URL che identifica il server di destinazione e la risorsa (ad esempio, un file HTML, un particolare punto dati sul server o uno strumento da eseguire).
- Un metodo che definisce l'azione richiesta (ad esempio, ottenere un file o salvare o aggiornare alcuni dati). I diversi metodi/verbi e le loro azioni associate sono elencati di seguito:

  - `GET`: Ottiene una risorsa specifica (ad esempio, un file HTML contenente informazioni su un prodotto o un elenco di prodotti).
  - `POST`: Crea una nuova risorsa (ad esempio, aggiunge un nuovo articolo a un wiki, aggiunge un nuovo contatto a un database).
  - `HEAD`: Ottiene le informazioni sui metadati di una risorsa specifica senza ottenere il corpo come farebbe `GET`. Per esempio, potrebbe utilizzare una richiesta `HEAD` per scoprire l'ultima volta che una risorsa è stata aggiornata, e poi usare solo la richiesta `GET` (più "costosa") per scaricare la risorsa se è cambiata.
  - `PUT`: Aggiorna una risorsa esistente (o ne crea una nuova, se non esiste).
  - `DELETE`: Elimina la risorsa specificata.
  - `TRACE`, `OPTIONS`, `CONNECT`, `PATCH`: Questi verbi sono per compiti meno comuni/avanzati, quindi non li tratteremo qui.

- Informazioni aggiuntive possono essere codificate con la richiesta (ad esempio, dati del modulo HTML). Le informazioni possono essere codificate come:

  - Parametri URL: le richieste `GET` codificano i dati nell'URL inviato al server aggiungendo coppie nome/valore alla fine di esso — ad esempio, `http://example.com?name=Fred&age=11`. C'è sempre un punto interrogativo (`?`) che separa il resto dell'URL dai parametri URL, un segno uguale (`=`) che separa ciascun nome dal suo valore associato, e un'ampersand (`&`) che separa ciascuna coppia. I parametri URL sono intrinsecamente "insicuri" poiché possono essere modificati dagli utenti e poi reinviati. Di conseguenza i parametri URL/le richieste `GET` non sono utilizzati per richieste che aggiornano dati sul server.
  - Dati `POST`. Le richieste `POST` aggiungono nuove risorse, i dati per le quali sono codificati nel corpo della richiesta.
  - Cookie lato client. I cookie contengono dati di sessione sul client, inclusi le chiavi che il server può utilizzare per determinare il loro stato di accesso e le autorizzazioni/accessi alle risorse.

I server web attendono i messaggi di richiesta client, li elaborano quando arrivano, e rispondono al browser web con un messaggio di risposta HTTP. La risposta contiene un [codice di stato della risposta HTTP](/it/docs/Web/HTTP/Reference/Status) che indica se la richiesta è stata eseguita con successo (ad esempio, {{HTTPStatus("200", "200 OK")}} per successo, {{HTTPStatus("404", "404 Not Found")}} se la risorsa non può essere trovata, {{HTTPStatus("403", "403 Forbidden")}} se l'utente non è autorizzato a vedere la risorsa, ecc.). Il corpo della risposta a una richiesta `GET` riuscita contiene la risorsa richiesta.

Quando una pagina HTML viene restituita, essa viene resa dal browser web. Come parte del processo, il browser potrebbe scoprire collegamenti ad altre risorse (ad esempio, una pagina HTML in genere fa riferimento a file JavaScript e CSS), e invierà richieste HTTP separate per scaricare questi file.

Sia i siti web statici che quelli dinamici (discusso nelle sezioni seguenti) utilizzano esattamente lo stesso protocollo di comunicazione/schemi.

### Esempio di richiesta/risposta GET

Può fare una semplice richiesta `GET` cliccando su un link o effettuando una ricerca su un sito (come una pagina iniziale di un motore di ricerca). Ad esempio, la richiesta HTTP che viene inviata quando esegue una ricerca su MDN per il termine "panoramica client-server" sarà molto simile al testo mostrato di seguito (non sarà identico perché parti del messaggio dipendono dal suo browser/configurazione).

> [!NOTE]
> Il formato dei messaggi HTTP è definito in un "standard web" ([RFC9110](https://httpwg.org/specs/rfc9110.html#messages)). Non necessita di conoscere questo livello di dettaglio, ma almeno ora sa da dove proviene tutto questo!

#### La richiesta

Ogni riga della richiesta contiene informazioni al riguardo. La prima parte si chiama **header**, e contiene informazioni utili sulla richiesta, allo stesso modo in cui una [testa HTML](/it/docs/Learn_web_development/Core/Structuring_content/Webpage_metadata) contiene informazioni utili su un documento HTML (ma non il contenuto effettivo, che si trova nel corpo):

```http
GET /en-US/search?q=client+server+overview&topic=apps&topic=html&topic=css&topic=js&topic=api&topic=webdev HTTP/1.1
Host: developer.mozilla.org
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/52.0.2743.116 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Referer: https://developer.mozilla.org/en-US/
Accept-Encoding: gzip, deflate, sdch, br
Accept-Language: en-US,en;q=0.8,es;q=0.6
Cookie: sessionid=6ynxs23n521lu21b1t136rhbv7ezngie; csrftoken=zIPUJsAZv6pcgCBJSCj1zU6pQZbfMUAT; dwf_section_edit=False; dwf_sg_task_completion=False; _gat=1; _ga=GA1.2.1688886003.1471911953; ffo=true
```

La prima e la seconda riga contengono la maggior parte delle informazioni di cui abbiamo parlato sopra:

- Il tipo di richiesta (`GET`).
- L'URL della risorsa di destinazione (`/en-US/search`).
- I parametri URL (`q=client%2Bserver%2Boverview&topic=apps&topic=html&topic=css&topic=js&topic=api&topic=webdev`).
- Il sito di destinazione/host (developer.mozilla.org).
- La fine della prima riga include anche una stringa breve che identifica la versione specifica del protocollo (`HTTP/1.1`).

L'ultima riga contiene informazioni sui cookie lato client — può vedere in questo caso che il cookie include un ID per gestire le sessioni (`Cookie: sessionid=6ynxs23n521lu21b1t136rhbv7ezngie; …`).

Le righe rimanenti contengono informazioni sul browser utilizzato e sul tipo di risposte che può gestire.
Per esempio, può vedere qui che:

- Il mio browser (`User-Agent`) è Mozilla Firefox (`Mozilla/5.0`).
- Può accettare informazioni compresse gzip (`Accept-Encoding: gzip`).
- Può accettare le lingue specificate (`Accept-Language: en-US,en;q=0.8,es;q=0.6`).
- La linea `Referer` indica l'indirizzo della pagina web che conteneva il link a questa risorsa (cioè, l'origine della richiesta, `https://developer.mozilla.org/en-US/`).

Le richieste HTTP possono avere anche un corpo, ma in questo caso è vuoto.

#### La risposta

La prima parte della risposta per questa richiesta è mostrata di seguito. L'header contiene informazioni come le seguenti:

- La prima riga include il codice di risposta `200 OK`, che ci dice che la richiesta è riuscita.
- possiamo vedere che la risposta è formattata `text/html` (`Content-Type`).
- possiamo anche vedere che utilizza il set di caratteri UTF-8 (`Content-Type: text/html; charset=utf-8`).
- La testa ci dice anche quanto è grande (`Content-Length: 41823`).

Alla fine del messaggio vediamo il contenuto del **corpo** — che contiene l'HTML effettivamente restituito dalla richiesta.

```http
HTTP/1.1 200 OK
Server: Apache
X-Backend-Server: developer1.webapp.scl3.mozilla.com
Vary: Accept, Cookie, Accept-Encoding
Content-Type: text/html; charset=utf-8
Date: Wed, 07 Sep 2016 00:11:31 GMT
Keep-Alive: timeout=5, max=999
Connection: Keep-Alive
X-Frame-Options: DENY
Allow: GET
X-Cache-Info: caching
Content-Length: 41823

<!doctype html>
<html lang="en-US" dir="ltr" class="redesign no-js" data-ffo-opensanslight=false data-ffo-opensans=false >
<head prefix="og: http://ogp.me/ns#">
  <meta charset="utf-8">
  <meta http-equiv="X-UA-Compatible" content="IE=Edge">
  <script>(function(d) { d.className = d.className.replace(/\bno-js/, ''); })(document.documentElement);</script>
  …
```

Il resto dell'header della risposta include informazioni sulla risposta (ad esempio, quando è stata generata), sul server e su come si aspetta che il browser gestisca la pagina (ad esempio, la riga `X-Frame-Options: DENY` dice al browser di non consentire che questa pagina venga incorporata in un {{htmlelement("iframe")}} in un altro sito).

### Esempio di richiesta/risposta POST

Un `POST` HTTP viene eseguito quando si invia un modulo contenente informazioni da salvare sul server.

#### La richiesta

Il testo di seguito mostra la richiesta HTTP effettuata quando un utente invia nuovi dettagli del profilo su questo sito. Il formato della richiesta è quasi lo stesso dell'esempio di richiesta `GET` mostrato in precedenza, sebbene la prima riga identifichi questa richiesta come un `POST`.

```http
POST /en-US/profiles/hamishwillee/edit HTTP/1.1
Host: developer.mozilla.org
Connection: keep-alive
Content-Length: 432
Pragma: no-cache
Cache-Control: no-cache
Origin: https://developer.mozilla.org
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/52.0.2743.116 Safari/537.36
Content-Type: application/x-www-form-urlencoded
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Referer: https://developer.mozilla.org/en-US/profiles/hamishwillee/edit
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.8,es;q=0.6
Cookie: sessionid=6ynxs23n521lu21b1t136rhbv7ezngie; _gat=1; csrftoken=zIPUJsAZv6pcgCBJSCj1zU6pQZbfMUAT; dwf_section_edit=False; dwf_sg_task_completion=False; _ga=GA1.2.1688886003.1471911953; ffo=true

csrfmiddlewaretoken=zIPUJsAZv6pcgCBJSCj1zU6pQZbfMUAT&user-username=hamishwillee&user-fullname=Hamish+Willee&user-title=&user-organization=&user-location=Australia&user-locale=en-US&user-timezone=Australia%2FMelbourne&user-irc_nickname=&user-interests=&user-expertise=&user-twitter_url=&user-stackoverflow_url=&user-linkedin_url=&user-mozillians_url=&user-facebook_url=
```

La principale differenza è che l'URL non ha parametri. Come può vedere, le informazioni dal modulo sono codificate nel corpo della richiesta (ad esempio, il nuovo nome completo dell'utente è impostato utilizzando: `&user-fullname=Hamish+Willee`).

#### La risposta

La risposta alla richiesta è mostrata di seguito. Il codice di stato `302 Found` dice al browser che il post è riuscito e che deve emettere una seconda richiesta HTTP per caricare la pagina specificata nel campo `Location`. Le informazioni sono altrimenti simili a quelle della risposta a una richiesta `GET`.

```http
HTTP/1.1 302 FOUND
Server: Apache
X-Backend-Server: developer3.webapp.scl3.mozilla.com
Vary: Cookie
Vary: Accept-Encoding
Content-Type: text/html; charset=utf-8
Date: Wed, 07 Sep 2016 00:38:13 GMT
Location: https://developer.mozilla.org/en-US/profiles/hamishwillee
Keep-Alive: timeout=5, max=1000
Connection: Keep-Alive
X-Frame-Options: DENY
X-Cache-Info: not cacheable; request wasn't a GET or HEAD
Content-Length: 0
```

> [!NOTE]
> Le risposte e richieste HTTP mostrate in questi esempi sono state catturate utilizzando l'applicazione [Fiddler](https://www.telerik.com/download/fiddler), ma può ottenere informazioni simili utilizzando analizzatori web (ad esempio, [WebSniffer](https://websniffer.com/)) o analizzatori di pacchetti come [Wireshark](https://www.wireshark.org/). Può provare lei stesso. Utilizzi uno degli strumenti collegati e poi navighi attraverso un sito ed editi le informazioni del profilo per vedere le diverse richieste e risposte. La maggior parte dei browser moderni dispone anche di strumenti che monitorano le richieste di rete (per esempio, lo strumento [Network Monitor](https://firefox-source-docs.mozilla.org/devtools-user/network_monitor/index.html) in Firefox).

## Siti statici

Un _sito statico_ è uno che restituisce lo stesso contenuto predefinito dal server ogni volta che viene richiesta una particolare risorsa. Quindi, ad esempio, se ha una pagina su un prodotto a `/static/my-product1.html`, questa stessa pagina verrà restituita a ogni utente. Se aggiunge un altro prodotto simile al suo sito, dovrà aggiungere un'altra pagina (ad esempio, `my-product2.html`) e così via. Questo può cominciare a diventare davvero inefficiente — cosa succede quando arriva a migliaia di pagine di prodotto? Ripeterebbe molto codice in ciascuna pagina (il modello di pagina di base, la struttura, ecc.), e se volesse cambiare qualcosa nella struttura della pagina — ad esempio aggiungere una nuova sezione "prodotti correlati" — dovrebbe modificare ogni pagina singolarmente.

> [!NOTE]
> I siti statici sono eccellenti quando ha un numero limitato di pagine e vuole inviare lo stesso contenuto a ogni utente. Tuttavia, possono avere un costo significativo per la manutenzione man mano che aumenta il numero di pagine.

Ricapitoliamo come funziona ciò, guardando di nuovo il diagramma dell'architettura del sito statico che abbiamo visto nell'ultimo articolo.

![Un diagramma semplificato di un server web statico.](basic_static_app_server.png)

Quando un utente desidera navigare su una pagina, il browser invia una richiesta HTTP `GET` specificando l'URL della sua pagina HTML. Il server recupera il documento richiesto dal suo file system e restituisce una risposta HTTP contenente il documento e un [codice di stato della risposta HTTP](/it/docs/Web/HTTP/Reference/Status) di `200 OK` (che indica successo). Il server potrebbe restituire un codice di stato diverso, ad esempio `404 Not Found` se il file non è presente sul server, o `301 Moved Permanently` se il file esiste ma è stato reindirizzato a una posizione diversa.

Il server per un sito statico dovrà elaborare solo richieste GET, poiché il server non memorizza alcun dato modificabile. Non modifica nemmeno le sue risposte in base ai dati della richiesta HTTP (ad esempio, parametri URL o cookie).

Comprendere come funzionano i siti statici è tuttavia utile quando si impara la programmazione lato server, perché i siti dinamici gestiscono le richieste per file statici (CSS, JavaScript, immagini statiche, ecc.) esattamente allo stesso modo.

## Siti dinamici

Un _sito dinamico_ è uno che può generare e restituire contenuti in base all'URL specifico della richiesta e ai dati (anziché restituire sempre lo stesso file codificato in modo fisso per un determinato URL). Utilizzando l'esempio di un sito di prodotti, il server memorizzerebbe i "dati" dei prodotti in un database, anziché nei singoli file HTML. Quando riceve una richiesta HTTP `GET` per un prodotto, il server determina l'ID del prodotto, recupera i dati dal database, e poi costruisce la pagina HTML per la risposta inserendo i dati in un modello HTML. Questo ha vantaggi importanti rispetto a un sito statico:

L'utilizzo di un database consente di memorizzare le informazioni sui prodotti in modo efficiente in un modo facilmente estensibile, modificabile e ricercabile.

L'utilizzo di modelli HTML rende molto facile modificare la struttura HTML, poiché ciò deve essere fatto solo in un unico posto, in un singolo modello, e non su migliaia di pagine statiche.

### Anatomia di una richiesta dinamica

Questa sezione fornisce una panoramica dettagliata del ciclo di richiesta e risposta HTTP "dinamico", basato su ciò che abbiamo visto nell'ultimo articolo con molto più dettaglio. Per "mantenere le cose reali" utilizzeremo il contesto di un sito web per allenatori di squadre sportive in cui un allenatore può selezionare il nome della sua squadra e il numero di giocatori in un modulo HTML e ottenere un suggerimento di "migliore formazione" per il loro prossimo gioco.

Il diagramma sottostante mostra i principali elementi del sito web "allenatore di squadra", insieme a etichette numerate per la sequenza delle operazioni quando l'allenatore accede alla sua lista di "migliore formazione". Le parti del sito che lo rendono dinamico sono l'_Applicazione Web_ (così chiameremo il codice lato server che elabora le richieste HTTP e restituisce le risposte HTTP), il _Database_, che contiene informazioni sui giocatori, squadre, allenatori e le loro relazioni, e i _Modelli HTML_.

![Questo è un diagramma di un server web semplice con numeri per ogni passaggio dell'interazione client-server.](web_application_with_html_and_steps.png)

Dopo che l'allenatore invia il modulo con il nome della squadra e il numero di giocatori, la sequenza delle operazioni è:

1. Il browser web crea una richiesta HTTP `GET` al server utilizzando l'URL di base per la risorsa (`/best`) e codificando il team e il numero di giocatori sia come parametri URL (ad esempio, `/best?team=my_team_name&show=11`) sia come parte del modello URL (ad esempio, `/best/my_team_name/11/`). Viene utilizzata una richiesta `GET` perché la richiesta sta solo recuperando dati (non modificando dati).
2. Il _Server Web_ rileva che la richiesta è "dinamica" e la inoltra all'_Applicazione Web_ per l'elaborazione (il server web determina come gestire diversi URL in base a regole di pattern matching definite nella sua configurazione).
3. L'_Applicazione Web_ identifica che l'_intenzione_ della richiesta è di ottenere la "lista della migliore squadra" in base all'URL (`/best/`) e scopre il nome della squadra richiesto e il numero di giocatori dall'URL. L'_Applicazione Web_ quindi ottiene le informazioni richieste dal database (utilizzando parametri aggiuntivi "interni" per definire quali giocatori sono "migliori", e possibilmente anche ottenendo l'identità dell'allenatore connesso da un cookie lato client).
4. L'_Applicazione Web_ crea dinamicamente una pagina HTML inserendo i dati (dal _Database_) in segnaposti all'interno di un modello HTML.
5. L'_Applicazione Web_ restituisce l'HTML generato al browser web (tramite il _Server Web_), insieme a un codice di stato HTTP di 200 ("successo"). Se qualcosa impedisce che l'HTML venga restituito, l'_Applicazione Web_ restituirà un altro codice — ad esempio "404" per indicare che la squadra non esiste.
6. Il Browser Web inizierà quindi a elaborare l'HTML restituito, inviando richieste separate per ottenere qualsiasi altro file CSS o JavaScript a cui fa riferimento (vedi passaggio 7).
7. Il Server Web carica i file statici dal file system e li restituisce al browser direttamente (ancora una volta, il corretto trattamento dei file si basa su regole di configurazione e pattern matching degli URL).

Un'operazione per aggiornare un record nel database verrebbe gestita in modo simile, tranne che come qualsiasi aggiornamento del database, la richiesta HTTP dal browser dovrebbe essere codificata come una richiesta `POST`.

### Fare altri lavori

Il compito di un'_Applicazione Web_ è ricevere richieste HTTP e restituire risposte HTTP. Sebbene interagire con un database per ottenere o aggiornare informazioni siano attività molto comuni, il codice potrebbe fare anche altre cose allo stesso tempo, o non interagire affatto con un database.

Un buon esempio di un'attività aggiuntiva che un'_Applicazione Web_ potrebbe eseguire sarebbe inviare un'email agli utenti per confermare la loro registrazione al sito. Il sito potrebbe anche eseguire registri o altre operazioni.

### Restituire qualcosa di diverso dall'HTML

Il codice del sito web lato server non deve necessariamente restituire snippet/file HTML nella risposta. Può invece creare dinamicamente e restituire altri tipi di file (testo, PDF, CSV, ecc.) o persino dati (JSON, XML, ecc.).

Ciò è particolarmente rilevante per i siti web che funzionano recuperando contenuti dal server utilizzando JavaScript e aggiornando la pagina dinamicamente, anziché caricare sempre una nuova pagina quando devono essere mostrati nuovi contenuti. Vedere [Fare richieste di rete con JavaScript](/it/docs/Learn_web_development/Core/Scripting/Network_requests) per ulteriori informazioni sulla motivazione di questo approccio, e su come appare questo modello dal punto di vista del client.

## I framework web semplificano la programmazione web lato server

I framework web lato server rendono molto più facile scrivere codice per gestire le operazioni descritte sopra.

Una delle operazioni più importanti che svolgono è fornire meccanismi semplici per mappare gli URL per diverse risorse/pagine a funzioni specifiche. Ciò rende più semplice mantenere separato il codice associato a ogni tipo di risorsa. Ha anche vantaggi in termini di manutenzione, perché può cambiare l'URL usato per fornire una particolare funzione in un unico posto, senza dover cambiare la funzione di gestione.

Ad esempio, consideri il seguente codice Django (Python) che mappa due modelli URL a due funzioni di visualizzazione. Il primo modello garantisce che una richiesta HTTP con un URL di risorsa di `/best` venga passata a una funzione chiamata `index()` nel modulo `views`. Una richiesta che ha il modello `/best/junior`, verrà invece passata alla funzione di visualizzazione `junior()`.

```python
# file: best/urls.py
#

from django.conf.urls import url

from . import views

urlpatterns = [
    # example: /best/
    url(r'^$', views.index),
    # example: /best/junior/
    url(r'^junior/$', views.junior),
]
```

> [!NOTE]
> I primi parametri nelle funzioni `url()` potrebbero sembrare un po' strani (ad esempio, `r'^junior/$'`) perché utilizzano una tecnica di pattern matching chiamata "espressioni regolari" (RegEx, o RE). Non dev'essere a conoscenza di come funzionano le espressioni regolari in questo momento, se non che ci permettono di abbinare schemi nell'URL (piuttosto che i valori codificati in precedenza) e usarli come parametri nelle nostre funzioni di visualizzazione. Come esempio, un RegEx molto semplice potrebbe dire "abbina una singola lettera maiuscola, seguita da tra le 4 e le 7 lettere minuscole."

Il framework web rende anche facile per una funzione di visualizzazione recuperare informazioni dal database. La struttura dei nostri dati è definita in modelli, che sono classi Python che definiscono i campi da memorizzare nel database sottostante. Se abbiamo un modello denominato _Team_ con un campo di "_team_type_" allora possiamo utilizzare una semplice sintassi di query per ottenere tutte le squadre che hanno un tipo particolare.

L'esempio seguente ottiene un elenco di tutte le squadre che hanno esattamente (sensibile al maiuscolo/minuscolo) il `team_type` di "junior" — nota il formato: nome del campo (`team_type`) seguito da doppio underscore, e poi il tipo di corrispondenza da utilizzare (in questo caso `exact`). Ci sono molti altri tipi di corrispondenze e possiamo concatenarli a catena. Inoltre, possiamo controllare l'ordine e il numero di risultati restituiti.

```python
#best/views.py

from django.shortcuts import render

from .models import Team

def junior(request):
    list_teams = Team.objects.filter(team_type__exact="junior")
    context = {'list': list_teams}
    return render(request, 'best/index.html', context)
```

Dopo che la funzione `junior()` ha ottenuto l'elenco delle squadre junior, chiama la funzione `render()`, passando il `HttpRequest` originale, un modello HTML, e un oggetto "context" che definisce le informazioni da includere nel modello. La funzione `render()` è una funzione di comodo che genera HTML utilizzando un contesto e un modello HTML, e lo restituisce in un oggetto `HttpResponse`.

Ovviamente, i framework web possono aiutarla con molte altre attività. Discutiamo di molti altri vantaggi e alcune scelte popolari di framework web nel prossimo articolo.

## Riepilogo

A questo punto dovrebbe avere una buona panoramica delle operazioni che il codice lato server deve eseguire e conoscere alcuni dei modi in cui un framework web lato server può rendere questo più facile.

In un modulo seguente l'aiuteremo a scegliere il miglior Web Framework per il suo primo sito.

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/First_steps/Introduction", "Learn_web_development/Extensions/Server-side/First_steps/Web_frameworks", "Learn_web_development/Extensions/Server-side/First_steps")}}
