---
title: Panoramica client-server
slug: Learn_web_development/Extensions/Server-side/First_steps/Client-Server_overview
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/First_steps/Introduction", "Learn_web_development/Extensions/Server-side/First_steps/Web_frameworks", "Learn_web_development/Extensions/Server-side/First_steps")}}

Ora che conosci lo scopo e i potenziali benefici della programmazione lato server, esamineremo in dettaglio cosa succede quando un server riceve una "richiesta dinamica" da un browser. Poiché la maggior parte del codice lato server dei siti web gestisce richieste e risposte in modi simili, questo ti aiuterà a comprendere ciò che devi fare quando scrivi gran parte del tuo codice.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Una comprensione di base di cosa sia un web server.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere le interazioni client-server in un sito web dinamico e in
        particolare quali operazioni devono essere svolte dal codice lato server.
      </td>
    </tr>
  </tbody>
</table>

Non ci sono veri codici nella discussione perché non abbiamo ancora scelto un framework web da usare per scrivere il nostro codice! Tuttavia, questa discussione è ancora molto rilevante, poiché il comportamento descritto deve essere implementato dal tuo codice lato server, indipendentemente dal linguaggio di programmazione o framework web selezionato.

## Web server e HTTP (un'introduzione)

I browser web comunicano con i [web server](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_web_server) usando il **P**rotocollo di **T**rasferimento di **I**pertesto ([HTTP](/it/docs/Web/HTTP)). Quando clicchi su un link in una pagina web, invii un modulo, o esegui una ricerca, il browser invia una _richiesta HTTP_ al server.

Questa richiesta include:

- Un URL che identifica il server di destinazione e la risorsa (per esempio, un file HTML, un particolare dato sul server, o uno strumento da eseguire).
- Un metodo che definisce l'azione richiesta (ad esempio, ottenere un file o salvare o aggiornare alcuni dati). I diversi metodi/verbi e le loro azioni associate sono elencati di seguito:

  - `GET`: Ottenere una specifica risorsa (ad es. un file HTML che contiene informazioni su un prodotto o un elenco di prodotti).
  - `POST`: Creare una nuova risorsa (ad es. aggiungere un nuovo articolo a un wiki, aggiungere un nuovo contatto a un database).
  - `HEAD`: Ottenere le informazioni sui metadati di una specifica risorsa senza ottenere il corpo come farebbe `GET`. Potresti, per esempio, usare una richiesta `HEAD` per scoprire l'ultima volta che una risorsa è stata aggiornata, e quindi usare solo la richiesta `GET` (più "costosa") per scaricare la risorsa se è stata modificata.
  - `PUT`: Aggiornare una risorsa esistente (o crearne una nuova se non esiste).
  - `DELETE`: Eliminare la risorsa specificata.
  - `TRACE`, `OPTIONS`, `CONNECT`, `PATCH`: Questi verbi sono per compiti meno comuni/avanzati, quindi non li copriremo qui.

- Ulteriori informazioni possono essere codificate con la richiesta (per esempio, dati di un modulo HTML). L'informazione può essere codificata come:

  - Parametri URL: le richieste `GET` codificano i dati nell'URL inviato al server aggiungendo coppie nome/valore alla fine di esso — per esempio `http://example.com?name=Fred&age=11`. C'è sempre un punto interrogativo (`?`) che separa il resto dell'URL dai parametri URL, un segno uguale (`=`) che separa ogni nome dal suo valore associato, e un'e commerciale (`&`) che separa ogni coppia. I parametri URL sono intrinsecamente "insicuri" poiché possono essere modificati dagli utenti e poi risubmitted. Di conseguenza, i parametri URL/le richieste `GET` non sono utilizzati per richieste che aggiornano dati sul server.
  - Dati `POST`. Le richieste `POST` aggiungono nuove risorse, i cui dati sono codificati all'interno del corpo della richiesta.
  - Cookie lato client. I cookie contengono dati di sessione sul client, inclusi chiavi che il server può usare per determinare il loro stato di login e permessi/accessi alle risorse.

I web server attendono messaggi di richiesta client, li elaborano quando arrivano e rispondono al browser web con un messaggio di risposta HTTP. La risposta contiene un [codice di stato della risposta HTTP](/it/docs/Web/HTTP/Reference/Status) che indica se la richiesta è andata a buon fine (ad esempio, {{HTTPStatus("200", "200 OK")}} per successo, {{HTTPStatus("404", "404 Not Found")}} se la risorsa non può essere trovata, {{HTTPStatus("403", "403 Forbidden")}} se l'utente non è autorizzato a vedere la risorsa, ecc.). Il corpo della risposta a una richiesta `GET` di successo contiene la risorsa richiesta.

Quando viene restituita una pagina HTML, viene renderizzata dal browser web. Come parte del processo, il browser può scoprire collegamenti ad altre risorse (ad esempio, una pagina HTML di solito fa riferimento a file JavaScript e CSS) e invierà richieste HTTP separate per scaricare questi file.

Sia i siti statici che quelli dinamici (discussi nelle sezioni seguenti) utilizzano esattamente lo stesso protocollo di comunicazione/schemi.

### Esempio di richiesta/risposta GET

Puoi fare una semplice richiesta `GET` cliccando su un link o cercando su un sito (come la homepage di un motore di ricerca). Ad esempio, la richiesta HTTP che viene inviata quando esegui una ricerca su MDN per il termine "client-server overview" sarà molto simile al testo mostrato sotto (non sarà identico perché parti del messaggio dipendono dal tuo browser/configurazione).

> [!NOTE]
> Il formato dei messaggi HTTP è definito in uno "standard web" ([RFC9110](https://httpwg.org/specs/rfc9110.html#messages)). Non è necessario conoscere questo livello di dettaglio, ma almeno ora sai da dove proviene tutto questo!

#### La richiesta

Ogni riga della richiesta contiene informazioni su di essa. La prima parte è chiamata **header** e contiene informazioni utili sulla richiesta, nello stesso modo in cui un [head HTML](/it/docs/Learn_web_development/Core/Structuring_content/Webpage_metadata) contiene informazioni utili su un documento HTML (ma non il contenuto effettivo, che si trova nel corpo):

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

Le prime due righe contengono la maggior parte delle informazioni di cui abbiamo parlato sopra:

- Il tipo di richiesta (`GET`).
- L'URL della risorsa di destinazione (`/en-US/search`).
- I parametri URL (`q=client%2Bserver%2Boverview&topic=apps&topic=html&topic=css&topic=js&topic=api&topic=webdev`).
- Il sito web di destinazione/host (developer.mozilla.org).
- La fine della prima riga include anche una breve stringa che identifica la versione specifica del protocollo (`HTTP/1.1`).

L'ultima riga contiene informazioni sui cookie lato client — puoi vedere in questo caso che il cookie include un id per la gestione delle sessioni (`Cookie: sessionid=6ynxs23n521lu21b1t136rhbv7ezngie; …`).

Le righe rimanenti contengono informazioni sul browser utilizzato e sul tipo di risposte che può gestire.
Ad esempio, puoi vedere che:

- Il mio browser (`User-Agent`) è Mozilla Firefox (`Mozilla/5.0`).
- Può accettare informazioni compresse gzip (`Accept-Encoding: gzip`).
- Può accettare le lingue specificate (`Accept-Language: en-US,en;q=0.8,es;q=0.6`).
- La linea `Referer` indica l'indirizzo della pagina web che conteneva il link a questa risorsa (cioè, l'origine della richiesta, `https://developer.mozilla.org/en-US/`).

Le richieste HTTP possono anche avere un corpo, ma in questo caso è vuoto.

#### La risposta

La prima parte della risposta per questa richiesta è mostrata di seguito. L'header contiene informazioni come le seguenti:

- La prima riga include il codice di risposta `200 OK`, che ci informa che la richiesta è stata eseguita con successo.
- Possiamo vedere che la risposta è formattata come `text/html` (`Content-Type`).
- Possiamo anche vedere che utilizza il set di caratteri UTF-8 (`Content-Type: text/html; charset=utf-8`).
- L'head ci dice anche quanto è grande (`Content-Length: 41823`).

Alla fine del messaggio vediamo il contenuto del **body** — che contiene l'HTML effettivamente restituito dalla richiesta.

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

Il resto dell'header della risposta include informazioni sulla risposta (ad es., quando è stata generata), sul server e su come ci si aspetta che il browser gestisca la pagina (ad es., la linea `X-Frame-Options: DENY` dice al browser di non consentire che questa pagina sia incorporata in un {{htmlelement("iframe")}} in un altro sito).

### Esempio di richiesta/risposta POST

Un HTTP `POST` viene fatto quando invii un modulo contenente informazioni da salvare sul server.

#### La richiesta

Il testo di seguito mostra la richiesta HTTP fatta quando un utente invia nuovi dettagli del profilo su questo sito. Il formato della richiesta è quasi lo stesso dell'esempio di richiesta `GET` mostrato in precedenza, sebbene la prima riga identifichi questa richiesta come un `POST`.

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

La principale differenza è che l'URL non ha parametri. Come puoi vedere, le informazioni del modulo sono codificate nel corpo della richiesta (ad esempio, il nuovo nome completo dell'utente è impostato usando: `&user-fullname=Hamish+Willee`).

#### La risposta

La risposta alla richiesta è mostrata di seguito. Il codice di stato di `302 Found` dice al browser che il post è riuscito, e che deve emettere una seconda richiesta HTTP per caricare la pagina specificata nel campo `Location`. Le informazioni sono altrimenti simili a quelle per la risposta a una richiesta `GET`.

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
> Le risposte e le richieste HTTP mostrate in questi esempi sono state catturate utilizzando l'applicazione [Fiddler](https://www.telerik.com/download/fiddler), ma puoi ottenere informazioni simili usando sniffer web (ad es., [WebSniffer](https://websniffer.com/)) o analizzatori di pacchetti come [Wireshark](https://www.wireshark.org/). Puoi provare tu stesso. Usa uno degli strumenti collegati e poi naviga attraverso un sito ed edita le informazioni del profilo per vedere le diverse richieste e risposte. La maggior parte dei browser moderni ha anche strumenti che monitorano le richieste di rete (ad esempio, lo strumento [Network Monitor](https://firefox-source-docs.mozilla.org/devtools-user/network_monitor/index.html) in Firefox).

## Siti statici

Un _sito statico_ è uno che restituisce sempre lo stesso contenuto codificato dal server ogni volta che viene richiesta una particolare risorsa. Per esempio, se hai una pagina su un prodotto su `/static/my-product1.html`, questa stessa pagina sarà restituita a ogni utente. Se aggiungi un altro prodotto simile al tuo sito dovrai aggiungere un'altra pagina (ad esempio, `my-product2.html`) e così via. Questo può diventare davvero inefficiente — cosa succede quando si arriva a migliaia di pagine di prodotto? Ripeteresti molto codice su ogni pagina (il modello di base della pagina, la struttura, ecc.), e se volessi cambiare qualcosa della struttura della pagina — come aggiungere una nuova sezione "prodotti correlati" ad esempio — dovresti modificare ogni pagina individualmente.

> [!NOTE]
> I siti statici sono eccellenti quando hai un numero ridotto di pagine e vuoi inviare lo stesso contenuto a ogni utente. Tuttavia, possono avere un costo significativo per la manutenzione man mano che aumenta il numero di pagine.

Ripetiamo come funziona, guardando di nuovo il diagramma dell'architettura del sito statico che abbiamo visto nell'ultimo articolo.

![Un diagramma semplificato di un server web statico.](basic_static_app_server.png)

Quando un utente vuole navigare verso una pagina, il browser invia una richiesta HTTP `GET` specificando l'URL della sua pagina HTML. Il server recupera il documento richiesto dal suo file system e restituisce una risposta HTTP contenente il documento e un [codice di stato della risposta HTTP](/it/docs/Web/HTTP/Reference/Status) di `200 OK` (indicando successo). Il server potrebbe restituire un codice di stato diverso, ad esempio `404 Not Found` se il file non è presente sul server o `301 Moved Permanently` se il file esiste ma è stato reindirizzato a una posizione diversa.

Il server di un sito statico avrà bisogno solo di elaborare richieste GET, perché il server non memorizza nessun dato modificabile. Inoltre, non modifica le sue risposte in base ai dati della richiesta HTTP (ad es., parametri URL o cookie).

Comprendere come funzionano i siti statici è comunque utile quando si impara la programmazione lato server, perché i siti dinamici gestiscono le richieste per i file statici (CSS, JavaScript, immagini statiche, ecc.) esattamente nello stesso modo.

## Siti dinamici

Un _sito dinamico_ è uno che può generare e restituire contenuti in base all'URL e ai dati specifici della richiesta (anziché restituire sempre lo stesso file codificato per un determinato URL). Utilizzando l'esempio di un sito di prodotti, il server memorizzerebbe i "dati" del prodotto in un database anziché singoli file HTML. Quando riceve una richiesta HTTP `GET` per un prodotto, il server determina l'ID del prodotto, preleva i dati dal database e quindi costruisce la pagina HTML per la risposta inserendo i dati in un modello HTML. Questo ha vantaggi significativi rispetto a un sito statico:

L'uso di un database consente di memorizzare le informazioni sui prodotti in modo efficiente, facilmente estensibile, modificabile e ricercabile.

L'uso di modelli HTML rende molto facile modificare la struttura HTML, poiché questo deve essere fatto solo in un unico posto, in un singolo modello, e non su migliaia di pagine statiche potenzialmente.

### Anatomia di una richiesta dinamica

Questa sezione fornisce una panoramica passo-passo del ciclo di richiesta e risposta HTTP "dinamico", basandosi su ciò che abbiamo visto nell'ultimo articolo con molti più dettagli. Per mantenere una connessione con la realtà, utilizzeremo il contesto di un sito web per manager di squadre sportive dove un allenatore può selezionare il nome della sua squadra e la dimensione della squadra in un form HTML e ottenere un suggerimento sul "miglior schieramento" per la prossima partita.

Il diagramma qui sotto mostra i principali elementi del sito web del "team coach", insieme a etichette numerate per la sequenza delle operazioni quando l'allenatore accede alla sua lista del "miglior team". Le parti del sito che lo rendono dinamico sono la _Web Application_ (così chiameremo il codice lato server che elabora le richieste HTTP e restituisce le risposte HTTP), il _Database_, che contiene informazioni su giocatori, squadre, allenatori e le loro relazioni, e i _Template HTML_.

![Questo è un diagramma di un semplice server web con numeri di step per ciascun passaggio dell'interazione client-server.](web_application_with_html_and_steps.png)

Dopo che l'allenatore invia il form con il nome della squadra e il numero di giocatori, la sequenza delle operazioni è:

1. Il browser web crea una richiesta HTTP `GET` al server usando l'URL di base per la risorsa (`/best`) e codificando il nome della squadra e il numero di giocatori come parametri URL (es. `/best?team=my_team_name&show=11`) o come parte del pattern dell'URL (es. `/best/my_team_name/11/`). Viene utilizzata una richiesta `GET` perché la richiesta sta solo recuperando dati (non modificando dati).
2. Il _Web Server_ rileva che la richiesta è "dinamica" e la inoltra alla _Web Application_ per l'elaborazione (il web server determina come gestire i diversi URL in base a regole di pattern matching definite nella sua configurazione).
3. La _Web Application_ identifica che l'_intenzione_ della richiesta è quella di ottenere la "lista del miglior team" basata sull'URL (`/best/`) e scopre il nome della squadra necessario e il numero di giocatori dall'URL. La _Web Application_ quindi ottiene le informazioni necessarie dal database (usando parametri "interni" aggiuntivi per definire quali giocatori siano i "migliori" e possibilmente anche ottenendo l'identità dell'allenatore collegato da un cookie lato client).
4. La _Web Application_ crea dinamicamente una pagina HTML inserendo i dati (dal _Database_) negli spazi riservati all'interno di un modello HTML.
5. La _Web Application_ restituisce l'HTML generato al browser web (tramite il _Web Server_), insieme a un codice di stato HTTP di 200 ("successo"). Se qualcosa impedisce che l'HTML venga restituito, la _Web Application_ restituirà un altro codice — ad esempio "404" per indicare che il team non esiste.
6. Il Browser Web inizierà quindi a elaborare l'HTML restituito, inviando richieste separate per ottenere eventuali altri file CSS o JavaScript che fa riferimento (vedi step 7).
7. Il Web Server carica i file statici dal file system e li restituisce direttamente al browser (ancora una volta, la gestione corretta dei file si basa su regole di configurazione e pattern matching degli URL).

Un'operazione per aggiornare un record nel database sarebbe gestita in modo simile, ad eccezione che come qualsiasi aggiornamento del database, la richiesta HTTP dal browser dovrebbe essere codificata come una richiesta `POST`.

### Svolgere altri lavori

Il compito di una _Web Application_ è ricevere richieste HTTP e restituire risposte HTTP. Sebbene interagire con un database per ottenere o aggiornare informazioni siano attività molto comuni, il codice può fare altre cose contemporaneamente, o non interagire affatto con un database.

Un buon esempio di un compito aggiuntivo che può eseguire una _Web Application_ potrebbe essere inviare un'email agli utenti per confermare la loro registrazione al sito. Il sito potrebbe anche eseguire operazioni di registrazione o altre operazioni.

### Restituire qualcosa di diverso dall'HTML

Il codice del sito web lato server non deve restituire frammenti/file HTML nella risposta. Può invece creare dinamicamente e restituire altri tipi di file (testo, PDF, CSV, ecc.) o anche dati (JSON, XML, ecc.).

Questo è particolarmente rilevante per i siti web che funzionano recuperando contenuti dal server usando JavaScript e aggiornando la pagina dinamicamente, anziché caricare sempre una nuova pagina quando si vuole mostrare un nuovo contenuto. Vedi [Esecuzione di richieste di rete con JavaScript](/it/docs/Learn_web_development/Core/Scripting/Network_requests) per saperne di più sulla motivazione di questo approccio e su come appare questo modello dal punto di vista del client.

## I framework web semplificano la programmazione web lato server

I framework web lato server rendono molto più facile scrivere codice per gestire le operazioni descritte sopra.

Una delle operazioni più importanti che svolgono è fornire meccanismi semplici per mappare gli URL per diverse risorse/pagine a specifiche funzioni handler. Questo rende più facile mantenere separato il codice associato a ciascun tipo di risorsa. Ha anche vantaggi in termini di manutenzione, perché puoi cambiare l'URL utilizzato per offrire una funzionalità particolare in un solo posto, senza dover cambiare la funzione handler.

Per esempio, considera il seguente codice Django (Python) che mappa due pattern URL a due funzioni view. Il primo pattern garantisce che una richiesta HTTP con un URL di risorsa di `/best` sarà passata a una funzione chiamata `index()` nel modulo `views`. Una richiesta che ha il pattern `/best/junior`, sarà invece passata alla funzione view `junior()`.

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
> I primi parametri nelle funzioni `url()` possono sembrare un po' strani (ad es., `r'^junior/$'`) perché usano una tecnica di pattern matching chiamata "espressioni regolari" (RegEx, o RE). Non è necessario sapere come funzionano le espressioni regolari in questo punto, oltre al fatto che ci permettono di abbinare pattern nell'URL (anziché i valori codificati sopra) e usarli come parametri nelle nostre funzioni view. Come esempio, una RegEx molto semplice potrebbe dire "abbina una singola lettera maiuscola, seguita da fra 4 e 7 lettere minuscole."

Il framework web rende anche facile per una funzione view recuperare informazioni dal database. La struttura dei nostri dati è definita nei modelli, che sono classi Python che definiscono i campi da memorizzare nel database sottostante. Se abbiamo un modello chiamato _Team_ con un campo di "_team_type_" possiamo usare una semplice sintassi di query per ottenere tutte le squadre che hanno un particolare tipo.

L'esempio seguente ottiene un elenco di tutte le squadre che hanno esattamente (case sensitive) `team_type` di "junior" — nota il formato: nome del campo (`team_type`) seguito da doppio underscore e poi il tipo di corrispondenza da usare (in questo caso `exact`). Ci sono molti altri tipi di corrispondenze e possiamo concatenarli. Possiamo anche controllare l'ordine e il numero di risultati restituiti.

```python
#best/views.py

from django.shortcuts import render

from .models import Team

def junior(request):
    list_teams = Team.objects.filter(team_type__exact="junior")
    context = {'list': list_teams}
    return render(request, 'best/index.html', context)
```

Dopo che la funzione `junior()` ottiene l'elenco delle squadre junior, chiama la funzione `render()`, passando la richiesta originale `HttpRequest`, un template HTML, e un oggetto "context" che definisce le informazioni da includere nel template. La funzione `render()` è una funzione di comodità che genera HTML usando un context e un template HTML, e lo restituisce in un oggetto `HttpResponse`.

Ovviamente i framework web possono aiutarti con molte altre attività. Discuteremo molti altri benefici e alcune scelte popolari di framework web nel prossimo articolo.

## Riepilogo

A questo punto dovresti avere una buona panoramica delle operazioni che il codice lato server deve eseguire, e conoscere alcuni dei modi in cui un framework web lato server può renderlo più facile.

In un modulo successivo ti aiuteremo a scegliere il miglior Web Framework per il tuo primo sito.

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/First_steps/Introduction", "Learn_web_development/Extensions/Server-side/First_steps/Web_frameworks", "Learn_web_development/Extensions/Server-side/First_steps")}}
