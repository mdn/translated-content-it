---
title: Panoramica client-server
slug: Learn_web_development/Extensions/Server-side/First_steps/Client-Server_overview
l10n:
  sourceCommit: 3e543cdfe8dddfb4774a64bf3decdcbab42a4111
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/First_steps/Introduction", "Learn_web_development/Extensions/Server-side/First_steps/Web_frameworks", "Learn_web_development/Extensions/Server-side/First_steps")}}

Ora che è noto lo scopo e i potenziali vantaggi della programmazione lato server, esamineremo in dettaglio cosa accade quando un server riceve una "richiesta dinamica" da un browser. Poiché il codice lato server della maggior parte dei siti web gestisce richieste e risposte in modi simili, questo aiuterà a comprendere cosa occorre fare quando si scrive gran parte del proprio codice.

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
        Comprendere le interazioni client-server in un sito web dinamico e, in
        particolare, quali operazioni devono essere eseguite dal codice lato server.
      </td>
    </tr>
  </tbody>
</table>

Non è presente alcun vero codice nella trattazione perché non è stato ancora scelto un web framework da usare per scrivere il codice. Questa trattazione è comunque ancora molto rilevante, poiché il comportamento descritto deve essere implementato dal codice lato server, indipendentemente dal linguaggio di programmazione o dal web framework selezionato.

## Web server e HTTP (un'introduzione)

I browser web comunicano con i [web server](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_web_server) usando l'**H**yper**T**ext **T**ransfer **P**rotocol ([HTTP](/it/docs/Web/HTTP)). Quando si fa clic su un link in una pagina web, si invia un form o si esegue una ricerca, il browser invia una _richiesta HTTP_ al server.

Questa richiesta include:

- Un URL che identifica il server e la risorsa di destinazione (ad esempio, un file HTML, un particolare dato sul server o uno strumento da eseguire).
- Un metodo che definisce l'azione richiesta (ad esempio, ottenere un file, salvare o aggiornare alcuni dati). I diversi metodi/verbi e le azioni associate sono elencati di seguito:
  - `GET`: ottiene una risorsa specifica (ad esempio, un file HTML contenente informazioni su un prodotto o un elenco di prodotti).
  - `POST`: crea una nuova risorsa (ad esempio, aggiunge un nuovo articolo a un wiki, aggiunge un nuovo contatto a un database).
  - `HEAD`: ottiene le informazioni sui metadati di una risorsa specifica senza ottenerne il corpo, come farebbe `GET`. Si potrebbe, ad esempio, usare una richiesta `HEAD` per scoprire l'ultima volta che una risorsa è stata aggiornata, quindi usare la richiesta `GET` (più "costosa") per scaricare la risorsa solo se è cambiata.
  - `PUT`: aggiorna una risorsa esistente (o ne crea una nuova se non esiste).
  - `DELETE`: elimina la risorsa specificata.
  - `TRACE`, `OPTIONS`, `CONNECT`, `PATCH`: questi verbi sono destinati a operazioni meno comuni/avanzate, pertanto non verranno trattati qui.

- Informazioni aggiuntive possono essere codificate nella richiesta (ad esempio, dati di form HTML). Le informazioni possono essere codificate come:
  - Parametri URL: le richieste `GET` codificano i dati nell'URL inviato al server aggiungendo coppie nome/valore alla sua fine, ad esempio `http://example.com?name=Fred&age=11`. È sempre presente un punto interrogativo (`?`) che separa il resto dell'URL dai parametri URL, un segno di uguale (`=`) che separa ciascun nome dal valore associato e una e commerciale (`&`) che separa ciascuna coppia. I parametri URL sono intrinsecamente "non sicuri", poiché possono essere modificati dagli utenti e quindi inviati nuovamente. Di conseguenza, i parametri URL/le richieste `GET` non vengono usati per richieste che aggiornano dati sul server.
  - Dati `POST`. Le richieste `POST` aggiungono nuove risorse, i cui dati sono codificati nel corpo della richiesta.
  - Cookie lato client. I cookie contengono dati di sessione relativi al client, incluse chiavi che il server può usare per determinare lo stato di accesso e le autorizzazioni/gli accessi alle risorse.

I web server attendono messaggi di richiesta dal client, li elaborano quando arrivano e rispondono al browser web con un messaggio di risposta HTTP. La risposta contiene un [codice di stato della risposta HTTP](/it/docs/Web/HTTP/Reference/Status) che indica se la richiesta è riuscita o meno (ad esempio, {{HTTPStatus("200", "200 OK")}} in caso di successo, {{HTTPStatus("404", "404 Not Found")}} se la risorsa non può essere trovata, {{HTTPStatus("403", "403 Forbidden")}} se l'utente non è autorizzato a visualizzare la risorsa, ecc.). Il corpo della risposta a una richiesta `GET` riuscita contiene la risorsa richiesta.

Quando viene restituita una pagina HTML, questa viene renderizzata dal browser web. Nell'ambito dell'elaborazione, il browser può rilevare link ad altre risorse (ad esempio, una pagina HTML fa generalmente riferimento a file JavaScript e CSS) e invierà richieste HTTP separate per scaricare questi file.

Sia i siti web statici sia quelli dinamici (trattati nelle sezioni seguenti) usano esattamente lo stesso protocollo/modello di comunicazione.

### Esempio di richiesta/risposta GET

È possibile effettuare una semplice richiesta `GET` facendo clic su un link o eseguendo una ricerca in un sito (come nella pagina iniziale di un motore di ricerca). Ad esempio, la richiesta HTTP inviata quando si esegue una ricerca su MDN per il termine "client-server overview" assomiglierà molto al testo mostrato di seguito (non sarà identica perché alcune parti del messaggio dipendono dal browser/configurazione).

> [!NOTE]
> Il formato dei messaggi HTTP è definito in uno "standard web" ([RFC9110](https://httpwg.org/specs/rfc9110.html#messages)). Non è necessario conoscere questo livello di dettaglio, ma ora è almeno chiara l'origine di tutto ciò.

#### La richiesta

Ogni riga della richiesta contiene informazioni su di essa. La prima parte è chiamata **header** e contiene informazioni utili sulla richiesta, nello stesso modo in cui un [HTML head](/it/docs/Learn_web_development/Core/Structuring_content/Webpage_metadata) contiene informazioni utili su un documento HTML (ma non il contenuto effettivo, che si trova nel corpo):

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

La prima e la seconda riga contengono la maggior parte delle informazioni trattate sopra:

- Il tipo di richiesta (`GET`).
- L'URL della risorsa di destinazione (`/en-US/search`).
- I parametri URL (`q=client%2Bserver%2Boverview&topic=apps&topic=html&topic=css&topic=js&topic=api&topic=webdev`).
- Il sito web host/di destinazione (developer.mozilla.org).
- La fine della prima riga include anche una stringa breve che identifica la versione specifica del protocollo (`HTTP/1.1`).

L'ultima riga contiene informazioni sui cookie lato client: in questo caso è possibile vedere che il cookie include un id per la gestione delle sessioni (`Cookie: sessionid=6ynxs23n521lu21b1t136rhbv7ezngie; …`).

Le righe rimanenti contengono informazioni sul browser usato e sul tipo di risposte che può gestire.
Ad esempio, qui è possibile vedere che:

- Il browser (`User-Agent`) è Mozilla Firefox (`Mozilla/5.0`).
- Può accettare informazioni compresse con gzip (`Accept-Encoding: gzip`).
- Può accettare le lingue specificate (`Accept-Language: en-US,en;q=0.8,es;q=0.6`).
- La riga `Referer` indica l'indirizzo della pagina web che conteneva il link a questa risorsa (ovvero l'origine della richiesta, `https://developer.mozilla.org/en-US/`).

Le richieste HTTP possono avere anche un corpo, ma in questo caso è vuoto.

#### La risposta

La prima parte della risposta a questa richiesta è mostrata di seguito. L'header contiene informazioni come le seguenti:

- La prima riga include il codice di risposta `200 OK`, che indica che la richiesta è riuscita.
- È possibile vedere che la risposta è formattata come `text/html` (`Content-Type`).
- È inoltre possibile vedere che usa il set di caratteri UTF-8 (`Content-Type: text/html; charset=utf-8`).
- L'header indica anche la sua dimensione (`Content-Length: 41823`).

Alla fine del messaggio si trova il contenuto del **corpo**, che contiene l'HTML effettivo restituito dalla richiesta.

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

Il resto dell'header della risposta include informazioni sulla risposta (ad esempio, quando è stata generata), sul server e su come il server si aspetta che il browser gestisca la pagina (ad esempio, la riga `X-Frame-Options: DENY` indica al browser di non consentire che questa pagina sia incorporata in un {{htmlelement("iframe")}} in un altro sito).

### Esempio di richiesta/risposta POST

Una richiesta HTTP `POST` viene effettuata quando si invia un form contenente informazioni da salvare sul server.

#### La richiesta

Il testo seguente mostra la richiesta HTTP effettuata quando un utente invia nuovi dettagli del profilo su questo sito. Il formato della richiesta è quasi uguale a quello dell'esempio di richiesta `GET` mostrato in precedenza, sebbene la prima riga identifichi questa richiesta come una `POST`.

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

La differenza principale è che l'URL non contiene parametri. Come si può vedere, le informazioni del form sono codificate nel corpo della richiesta (ad esempio, il nuovo nome completo dell'utente viene impostato usando: `&user-fullname=Hamish+Willee`).

#### La risposta

La risposta alla richiesta è mostrata di seguito. Il codice di stato `302 Found` indica al browser che il post è riuscito e che deve effettuare una seconda richiesta HTTP per caricare la pagina specificata nel campo `Location`. Per il resto, le informazioni sono simili a quelle della risposta a una richiesta `GET`.

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
> Le risposte e le richieste HTTP mostrate in questi esempi sono state acquisite usando l'applicazione [Fiddler](https://www.telerik.com/download/fiddler), ma è possibile ottenere informazioni simili usando web sniffer (ad esempio, [WebSniffer](https://websniffer.com/)) o analizzatori di pacchetti come [Wireshark](https://www.wireshark.org/). È possibile provarlo personalmente: usare uno qualsiasi degli strumenti collegati, quindi navigare in un sito e modificare le informazioni del profilo per visualizzare le diverse richieste e risposte. La maggior parte dei browser moderni dispone inoltre di strumenti che monitorano le richieste di rete (ad esempio, lo strumento [Network Monitor](https://firefox-source-docs.mozilla.org/devtools-user/network_monitor/index.html) in Firefox).

## Siti statici

Un _sito statico_ è un sito che restituisce lo stesso contenuto codificato in modo fisso dal server ogni volta che viene richiesta una determinata risorsa. Ad esempio, se è presente una pagina relativa a un prodotto in `/static/my-product1.html`, la stessa pagina verrà restituita a ogni utente. Se si aggiunge un altro prodotto simile al sito, sarà necessario aggiungere un'altra pagina (ad esempio, `my-product2.html`) e così via. Questo può iniziare a diventare davvero inefficiente: cosa accade quando si raggiungono migliaia di pagine di prodotto? Molto codice verrebbe ripetuto in ogni pagina (il template di pagina di base, la struttura, ecc.) e, se si volesse modificare qualcosa nella struttura della pagina, ad esempio aggiungere una nuova sezione "prodotti correlati", sarebbe necessario modificare ogni pagina singolarmente.

> [!NOTE]
> I siti statici sono eccellenti quando si dispone di un numero ridotto di pagine e si desidera inviare lo stesso contenuto a ogni utente. Tuttavia, possono comportare un costo di manutenzione significativo man mano che il numero di pagine aumenta.

Ricapitoliamo come funziona osservando nuovamente il diagramma dell'architettura di un sito statico esaminato nell'articolo precedente.

![Un diagramma semplificato di un web server statico.](basic_static_app_server.png)

Quando un utente vuole accedere a una pagina, il browser invia una richiesta HTTP `GET` specificando l'URL della relativa pagina HTML. Il server recupera il documento richiesto dal proprio file system e restituisce una risposta HTTP contenente il documento e un [codice di stato della risposta HTTP](/it/docs/Web/HTTP/Reference/Status) pari a `200 OK` (a indicare il successo). Il server potrebbe restituire un codice di stato diverso, ad esempio `404 Not Found` se il file non è presente sul server, oppure `301 Moved Permanently` se il file esiste ma è stato reindirizzato in una posizione diversa.

Il server di un sito statico dovrà elaborare esclusivamente richieste GET, perché il server non memorizza dati modificabili. Inoltre, non modifica le proprie risposte in base ai dati della richiesta HTTP (ad esempio, parametri URL o cookie).

Comprendere come funzionano i siti statici è comunque utile quando si apprende la programmazione lato server, perché i siti dinamici gestiscono le richieste di file statici (CSS, JavaScript, immagini statiche, ecc.) esattamente allo stesso modo.

## Siti dinamici

Un _sito dinamico_ è un sito in grado di generare e restituire contenuti in base all'URL e ai dati della richiesta specifica, anziché restituire sempre lo stesso file codificato in modo fisso per un determinato URL. Usando l'esempio di un sito di prodotti, il server memorizzerebbe i "dati" dei prodotti in un database anziché in singoli file HTML. Quando riceve una richiesta HTTP `GET` per un prodotto, il server determina l'ID del prodotto, recupera i dati dal database e quindi costruisce la pagina HTML per la risposta inserendo i dati in un template HTML. Ciò presenta notevoli vantaggi rispetto a un sito statico:

L'uso di un database consente di memorizzare le informazioni sui prodotti in modo efficiente e in una modalità facilmente estensibile, modificabile e ricercabile.

L'uso di template HTML rende molto semplice modificare la struttura HTML, poiché ciò deve essere fatto in un unico posto, in un singolo template, e non in potenzialmente migliaia di pagine statiche.

### Anatomia di una richiesta dinamica

Questa sezione fornisce una panoramica passo per passo del ciclo di richiesta e risposta HTTP "dinamico", basandosi su quanto esaminato nell'articolo precedente con molti più dettagli. Per mantenere l'esempio realistico, verrà usato il contesto di un sito web per l'allenatore di una squadra sportiva, in cui l'allenatore può selezionare il nome e la dimensione della propria squadra in un form HTML e ottenere una "migliore formazione" suggerita per la partita successiva.

Il diagramma seguente mostra gli elementi principali del sito web dell'"allenatore della squadra", insieme a etichette numerate per la sequenza di operazioni quando l'allenatore accede al proprio elenco della "migliore squadra". Le parti del sito che lo rendono dinamico sono la _Web Application_ (così verrà indicato il codice lato server che elabora le richieste HTTP e restituisce risposte HTTP), il _Database_, che contiene informazioni su giocatori, squadre, allenatori e sulle loro relazioni, e gli _HTML Templates_.

![Questo è un diagramma di un semplice web server con numeri di passaggio per ogni fase dell'interazione client-server.](web_application_with_html_and_steps.png)

Dopo che l'allenatore invia il form con il nome della squadra e il numero di giocatori, la sequenza di operazioni è:

1. Il browser web crea una richiesta HTTP `GET` al server usando l'URL di base della risorsa (`/best`) e codificando il numero della squadra e dei giocatori come parametri URL (ad esempio, `/best?team=my_team_name&show=11`) o come parte del pattern dell'URL (ad esempio, `/best/my_team_name/11/`). Viene usata una richiesta `GET` perché la richiesta si limita a recuperare dati, senza modificarli.
2. Il _Web Server_ rileva che la richiesta è "dinamica" e la inoltra alla _Web Application_ per l'elaborazione (il web server determina come gestire URL diversi in base alle regole di corrispondenza dei pattern definite nella sua configurazione).
3. La _Web Application_ identifica che l'_intento_ della richiesta è ottenere l'"elenco della migliore squadra" in base all'URL (`/best/`) e ricava dall'URL il nome della squadra richiesto e il numero di giocatori. La _Web Application_ ottiene quindi dal database le informazioni richieste (usando parametri "interni" aggiuntivi per definire quali giocatori sono i "migliori" e, possibilmente, ottenendo anche l'identità dell'allenatore che ha effettuato l'accesso da un cookie lato client).
4. La _Web Application_ crea dinamicamente una pagina HTML inserendo i dati (dal _Database_) nei segnaposto all'interno di un template HTML.
5. La _Web Application_ restituisce l'HTML generato al browser web, tramite il _Web Server_, insieme a un codice di stato HTTP 200 ("success"). Se qualcosa impedisce la restituzione dell'HTML, la _Web Application_ restituirà un altro codice, ad esempio "404" per indicare che la squadra non esiste.
6. Il browser web inizierà quindi a elaborare l'HTML restituito, inviando richieste separate per ottenere eventuali altri file CSS o JavaScript a cui fa riferimento (vedere il passaggio 7).
7. Il Web Server carica i file statici dal file system e li restituisce direttamente al browser (anche in questo caso, la corretta gestione dei file si basa su regole di configurazione e sulla corrispondenza dei pattern URL).

Un'operazione per aggiornare un record nel database verrebbe gestita in modo simile, eccetto che, come per qualsiasi aggiornamento di database, la richiesta HTTP dal browser dovrebbe essere codificata come richiesta `POST`.

### Esecuzione di altre operazioni

Il compito di una _Web Application_ è ricevere richieste HTTP e restituire risposte HTTP. Sebbene l'interazione con un database per ottenere o aggiornare informazioni sia un'attività molto comune, il codice può svolgere contemporaneamente altre operazioni o non interagire affatto con un database.

Un buon esempio di attività aggiuntiva che una _Web Application_ potrebbe eseguire è l'invio di un'email agli utenti per confermare la loro registrazione al sito. Il sito potrebbe inoltre eseguire il logging o altre operazioni.

### Restituire qualcosa di diverso da HTML

Il codice lato server di un sito web non deve necessariamente restituire snippet/file HTML nella risposta. Può invece creare e restituire dinamicamente altri tipi di file (testo, PDF, CSV, ecc.) o persino dati (JSON, XML, ecc.).

Questo è particolarmente rilevante per i siti web che funzionano recuperando contenuti dal server tramite JavaScript e aggiornando dinamicamente la pagina, anziché caricare sempre una nuova pagina quando devono essere visualizzati nuovi contenuti. Consultare [Effettuare richieste di rete con JavaScript](/it/docs/Learn_web_development/Core/Scripting/Network_requests) per ulteriori informazioni sulle motivazioni di questo approccio e su come appare questo modello dal punto di vista del client.

## I web framework semplificano la programmazione web lato server

I web framework lato server rendono molto più semplice scrivere codice per gestire le operazioni descritte in precedenza.

Una delle operazioni più importanti che svolgono è fornire meccanismi semplici per mappare gli URL di risorse/pagine diverse a funzioni handler specifiche. Ciò semplifica la separazione del codice associato a ogni tipo di risorsa. Presenta inoltre vantaggi in termini di manutenzione, perché è possibile modificare in un unico punto l'URL usato per fornire una particolare funzionalità, senza dover modificare la funzione handler.

Ad esempio, si consideri il seguente codice Django (Python), che mappa due pattern URL a due funzioni view. Il primo pattern garantisce che una richiesta HTTP con URL della risorsa `/best` venga passata a una funzione denominata `index()` nel modulo `views`. Una richiesta che presenta il pattern `/best/junior` verrà invece passata alla funzione view `junior()`.

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
> I primi parametri nelle funzioni `url()` potrebbero sembrare un po' insoliti (ad esempio, `r'^junior/$'`) perché usano una tecnica di corrispondenza dei pattern chiamata "espressioni regolari" (RegEx o RE). A questo punto non è necessario sapere come funzionano le espressioni regolari, se non che consentono di far corrispondere pattern nell'URL, anziché i valori codificati in modo fisso riportati sopra, e di usarli come parametri nelle funzioni view. Ad esempio, una RegEx molto semplice potrebbe indicare: "trova una singola lettera maiuscola, seguita da un numero compreso tra 4 e 7 di lettere minuscole".

Il web framework semplifica inoltre il recupero di informazioni dal database da parte di una funzione view. La struttura dei dati è definita nei modelli, che sono classi Python che definiscono i campi da memorizzare nel database sottostante. Se è presente un modello denominato _Team_ con un campo "_team_type_", è possibile usare una semplice sintassi di query per recuperare tutte le squadre che hanno un particolare tipo.

L'esempio seguente ottiene un elenco di tutte le squadre che hanno `team_type` esattamente, con distinzione tra maiuscole e minuscole, uguale a "junior". Notare il formato: nome del campo (`team_type`) seguito da doppio carattere di sottolineatura, quindi il tipo di corrispondenza da usare (in questo caso `exact`). Esistono molti altri tipi di corrispondenza ed è possibile concatenarli. È inoltre possibile controllare l'ordine e il numero di risultati restituiti.

```python
#best/views.py

from django.shortcuts import render

from .models import Team

def junior(request):
    list_teams = Team.objects.filter(team_type__exact="junior")
    context = {'list': list_teams}
    return render(request, 'best/index.html', context)
```

Dopo che la funzione `junior()` ottiene l'elenco delle squadre junior, chiama la funzione `render()`, passando la `HttpRequest` originale, un template HTML e un oggetto "context" che definisce le informazioni da includere nel template. La funzione `render()` è una funzione di utilità che genera HTML usando un context e un template HTML, e lo restituisce in un oggetto `HttpResponse`.

Ovviamente, i web framework possono aiutare in molte altre attività. Nel prossimo articolo verranno trattati molti altri vantaggi e alcune scelte popolari di web framework.

## Riepilogo

A questo punto dovrebbe essere disponibile una buona panoramica delle operazioni che il codice lato server deve eseguire e si dovrebbero conoscere alcuni dei modi in cui un web framework lato server può semplificarle.

In un modulo successivo verrà illustrato come scegliere il miglior Web Framework per il primo sito.

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/First_steps/Introduction", "Learn_web_development/Extensions/Server-side/First_steps/Web_frameworks", "Learn_web_development/Extensions/Server-side/First_steps")}}
