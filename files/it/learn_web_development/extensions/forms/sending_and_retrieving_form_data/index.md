---
title: Invio dei dati del modulo
slug: Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data
l10n:
  sourceCommit: be1922d62a0d31e4e3441db0e943aed8df736481
---

{{PreviousMenu("Learn_web_development/Extensions/Forms/Form_validation", "Learn_web_development/Extensions/Forms")}}

Una volta che i dati del modulo sono stati validati sul lato client, è possibile inviare il modulo. E, poiché abbiamo trattato la validazione nell'articolo precedente, siamo pronti per inviarlo! Questo articolo esamina cosa succede quando un utente invia un modulo: dove vanno i dati e come li gestiamo una volta arrivati? Esaminiamo anche alcune delle preoccupazioni sulla sicurezza associate all'invio dei dati del modulo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Una
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >comprensione di HTML</a
        >, e una conoscenza di base di
        <a href="/it/docs/Web/HTTP">HTTP</a> e
        <a href="/it/docs/Learn_web_development/Extensions/Server-side/First_steps"
          >programmazione lato server</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere cosa succede quando vengono inviati i dati di un modulo, inclusa
        un'idea di base su come i dati vengono elaborati sul server.
      </td>
    </tr>
  </tbody>
</table>

Per prima cosa, discuteremo cosa succede ai dati quando un modulo viene inviato.

## Architettura client/server

Nel suo aspetto più semplice, il web utilizza un'architettura client/server che può essere riassunta come segue: un client (di solito un browser web) invia una richiesta a un server (la maggior parte delle volte un server web come [Apache](https://httpd.apache.org/), [Nginx](https://nginx.org/), [IIS](https://www.iis.net/), [Tomcat](https://tomcat.apache.org/), ecc.), usando il [protocollo HTTP](/it/docs/Web/HTTP). Il server risponde alla richiesta usando lo stesso protocollo.

![Uno schema base dell'architettura Web client/server](client-server.png)

Un modulo HTML su una pagina web non è altro che un modo comodo e user-friendly per configurare una richiesta HTTP per inviare dati a un server. Ciò permette all'utente di fornire informazioni che verranno consegnate nella richiesta HTTP.

> [!NOTE]
> Per avere un'idea migliore di come funzionano le architetture client-server, legga il nostro modulo [Primi passi nella programmazione lato server](/it/docs/Learn_web_development/Extensions/Server-side/First_steps).

## Sul lato client: definire come inviare i dati

L'elemento {{HTMLElement("form")}} definisce come verranno inviati i dati. Tutti i suoi attributi sono progettati per permetterle di configurare la richiesta da inviare quando un utente preme un {{Glossary("submit_button", "submit button")}}. I due attributi più importanti sono [`action`](/it/docs/Web/HTML/Reference/Elements/form#action) e [`method`](/it/docs/Web/HTML/Reference/Elements/form#method).

### Attributo action

L'attributo [`action`](/it/docs/Web/HTML/Reference/Elements/form#action) definisce dove vengono inviati i dati. Il suo valore deve essere un [URL](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_URL) valido, relativo o assoluto. Se questo attributo non è fornito, i dati verranno inviati all'URL della pagina contenente il modulo — la pagina corrente.

In questo esempio, i dati vengono inviati a un URL assoluto — `https://example.com`:

```html
<form action="https://example.com">…</form>
```

Qui usiamo un URL relativo — i dati vengono inviati a un URL diverso nello stesso origin:

```html
<form action="/somewhere_else">…</form>
```

Quando specificato senza attributi, come di seguito, i dati del {{HTMLElement("form")}} vengono inviati alla stessa pagina su cui è presente il modulo:

```html
<form>…</form>
```

> [!NOTE]
> È possibile specificare un URL che utilizza il protocollo HTTPS (HTTP sicuro). Quando lo fa, i dati vengono crittografati insieme al resto della richiesta, anche se il modulo stesso è ospitato su una pagina non sicura accessibile tramite HTTP. D'altra parte, se il modulo è ospitato su una pagina sicura ma specifica un URL HTTP non sicuro con l'attributo [`action`](/it/docs/Web/HTML/Reference/Elements/form#action), tutti i browser visualizzano un avviso di sicurezza all'utente ogni volta che si tenta di inviare dati, poiché i dati non saranno crittografati.

I nomi e i valori dei controlli del modulo che non sono file vengono inviati al server come coppie `name=value` unite con delle e commerciali. Il valore di `action` dovrebbe essere un file sul server che possa gestire i dati in arrivo, inclusa la validazione lato server. Il server quindi risponde, generalmente gestendo i dati e caricando l'URL definito dall'attributo `action`, causando un nuovo caricamento della pagina (o un aggiornamento della pagina esistente, se `action` punta alla stessa pagina).

Come i dati vengono inviati dipende dall'attributo `method`.

### Attributo method

L'attributo [`method`](/it/docs/Web/HTML/Reference/Elements/form#method) definisce come i dati vengono inviati. Il [protocollo HTTP](/it/docs/Web/HTTP) fornisce diversi modi per effettuare una richiesta; i dati dei moduli HTML possono essere trasmessi tramite diversi metodi, i più comuni sono il metodo `GET` e il metodo `POST`.

Per capire la differenza tra questi due metodi, facciamo un passo indietro e esaminiamo [come funziona HTTP](/it/docs/Web/HTTP/Guides/Overview). Ogni volta che desidera raggiungere una risorsa sul Web, il browser invia una richiesta a un URL. Una richiesta HTTP è composta da due parti: un [header](/it/docs/Web/HTTP/Reference/Headers) che contiene un insieme di metadati globali sulle capacità del browser, e un corpo che può contenere le informazioni necessarie al server per elaborare la specifica richiesta.

#### Il metodo GET

Il [`metodo GET`](/it/docs/Web/HTTP/Reference/Methods/GET) è il metodo utilizzato dal browser per chiedere al server di rinviare una determinata risorsa: "Ciao server, voglio ottenere questa risorsa." In questo caso, il browser invia un corpo vuoto. Poiché il corpo è vuoto, se un modulo viene inviato usando questo metodo i dati inviati al server vengono aggiunti all'URL.

Consideri il seguente modulo:

```html
<form action="http://www.foo.com" method="GET">
  <div>
    <label for="say">What greeting do you want to say?</label>
    <input name="say" id="say" value="Hi" />
  </div>
  <div>
    <label for="to">Who do you want to say it to?</label>
    <input name="to" id="to" value="Mom" />
  </div>
  <div>
    <button>Send my greetings</button>
  </div>
</form>
```

Poiché è stato utilizzato il metodo `GET`, vedrà l'URL `www.foo.com/?say=Hi&to=Mom` apparire nella barra degli indirizzi del browser quando invia il modulo.

![L'URL cambiato con i parametri di query dopo l'invio del modulo con il metodo GET con una pagina di errore del browser "server non trovato"](url-parameters.png)

I dati vengono aggiunti all'URL come una serie di coppie nome/valore. Dopo che l'indirizzo web URL è terminato, includiamo un punto interrogativo (`?`) seguito dalle coppie nome/valore, ciascuna separata da un e commerciale (`&`). In questo caso, stiamo passando due dati al server:

- `say`, che ha un valore di `Hi`
- `to`, che ha un valore di `Mom`

La richiesta HTTP appare così:

```http
GET /?say=Hi&to=Mom HTTP/2.0
Host: foo.com
```

> [!NOTE]
> Può trovare questo esempio su GitHub — veda [get-method.html](https://github.com/mdn/learning-area/blob/main/html/forms/sending-form-data/get-method.html) ([veda anche in diretta](https://mdn.github.io/learning-area/html/forms/sending-form-data/get-method.html)).

> [!NOTE]
> I dati non verranno aggiunti se lo schema URL `action` non può gestire le query, ad es. `file:`.

#### Il metodo POST

Il [`metodo POST`](/it/docs/Web/HTTP/Reference/Methods/POST) è un po' diverso. È il metodo che il browser utilizza per parlare al server quando richiede una risposta che tiene conto dei dati forniti nel corpo della richiesta HTTP: "Ciao server, dai un'occhiata a questi dati e rimandami un risultato appropriato." Se un modulo viene inviato utilizzando questo metodo, i dati vengono aggiunti al corpo della richiesta HTTP.

Vediamo un esempio: questo è lo stesso modulo che abbiamo esaminato nella sezione `GET` sopra, ma con l'attributo [`method`](/it/docs/Web/HTML/Reference/Elements/form#method) impostato su `POST`.

```html
<form action="http://www.foo.com" method="POST">
  <div>
    <label for="say">What greeting do you want to say?</label>
    <input name="say" id="say" value="Hi" />
  </div>
  <div>
    <label for="to">Who do you want to say it to?</label>
    <input name="to" id="to" value="Mom" />
  </div>
  <div>
    <button>Send my greetings</button>
  </div>
</form>
```

Quando il modulo viene inviato utilizzando il metodo `POST`, non ottiene alcun dato aggiunto all'URL, e la richiesta HTTP appare così, con i dati inclusi nel corpo della richiesta:

```http
POST / HTTP/2.0
Host: foo.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 13

say=Hi&to=Mom
```

L'intestazione `Content-Length` indica la dimensione del corpo, e l'intestazione `Content-Type` indica il tipo di risorsa inviata al server. Discuteremo di queste intestazioni più avanti.

> [!NOTE]
> Può trovare questo esempio su GitHub — veda [post-method.html](https://github.com/mdn/learning-area/blob/main/html/forms/sending-form-data/post-method.html) ([veda anche in diretta](https://mdn.github.io/learning-area/html/forms/sending-form-data/post-method.html)).

> [!NOTE]
> Il metodo `GET` sarà utilizzato invece se lo schema URL `action` non può gestire un corpo di richiesta, ad es. `data:`.

### Visualizzazione delle richieste HTTP

Le richieste HTTP non sono mai visualizzate all'utente (se desidera vederle, deve utilizzare strumenti come il [Firefox Network Monitor](https://firefox-source-docs.mozilla.org/devtools-user/network_monitor/index.html) o gli [Strumenti per sviluppatori di Chrome](https://developer.chrome.com/docs/devtools/)). Ad esempio, i dati del modulo verranno mostrati come segue nella scheda Rete di Chrome. Dopo aver inviato il modulo:

1. Aprire gli strumenti per sviluppatori.
2. Selezionare "Network"
3. Selezionare "All"
4. Selezionare "foo.com" nella scheda "Name"
5. Selezionare "Request" (Firefox) o "Payload" (Chrome/Edge)

Potrà quindi ottenere i dati del modulo, come mostrato nell'immagine sottostante.

![Richieste HTTP e dati di risposta nella scheda di monitoraggio della rete negli strumenti per sviluppatori del browser](network-monitor.png)

L'unica cosa visualizzata per l'utente è l'URL chiamato. Come accennato sopra, con una richiesta `GET` l'utente vedrà i dati nella barra degli URL, ma con una richiesta `POST` no. Questo può essere molto importante per due motivi:

1. Se ha bisogno di inviare una password (o qualsiasi altro dato sensibile), non utilizzi mai il metodo `GET` altrimenti rischia di visualizzarlo nella barra degli URL, il che sarebbe molto insicuro.
2. Se ha bisogno di inviare una grande quantità di dati, il metodo `POST` è preferibile perché alcuni browser limitano le dimensioni degli URL. Inoltre, molti server limitano la lunghezza degli URL che accettano.

## Sul lato server: recuperare i dati

Qualunque sia il metodo HTTP scelto, il server riceve una stringa che verrà analizzata per ottenere i dati come un elenco di coppie chiave/valore. Il modo in cui accede a questo elenco dipende dalla piattaforma di sviluppo utilizzata e da eventuali framework specifici che potrebbe utilizzare con essa.

### Esempio: PHP grezzo

[PHP](https://www.php.net/) offre alcuni oggetti globali per accedere ai dati. Supponendo che abbia utilizzato il metodo `POST`, il seguente esempio prende solo i dati e li visualizza per l'utente. Ovviamente, cosa farà con i dati dipende da lei. Potrebbe visualizzarli, memorizzarli in un database, inviarli per email o elaborarli in qualche altro modo.

```php
<?php
  // The global $_POST variable allows you to access the data sent with the POST method by name
  // To access the data sent with the GET method, you can use $_GET
  $say = htmlspecialchars($_POST["say"]);
  $to  = htmlspecialchars($_POST["to"]);

  echo  $say, " ", $to;
?>
```

Questo esempio visualizza una pagina con i dati che abbiamo inviato. Può vederlo in azione nel nostro file [php-example.html](https://github.com/mdn/learning-area/blob/main/html/forms/sending-form-data/php-example.html) — che contiene lo stesso modulo di esempio che abbiamo visto prima, con un `method` di `POST` e un `action` di `php-example.php`. Quando viene inviato, invia i dati del modulo a [php-example.php](https://github.com/mdn/learning-area/blob/main/html/forms/sending-form-data/php-example.php), che contiene il codice PHP visto nel blocco sopra. Quando questo codice viene eseguito, l'output nel browser è `Hi Mom`.

![Pagina web altrimenti vuota con "hi mom", i dati ricevuti in risposta dopo l'invio dei dati del modulo a un file php con il metodo POST](php-result.png)

> [!NOTE]
> Questo esempio non funzionerà quando lo caricherà in un browser localmente — i browser non possono interpretare il codice PHP, quindi quando il modulo viene inviato il browser offrirà solo di scaricare il file PHP per lei. Per farlo funzionare, è necessario eseguire l'esempio tramite un qualche tipo di server PHP. Buone opzioni per il testing PHP locale sono [MAMP](https://www.mamp.info/en/downloads/) (Mac e Windows) e [XAMPP](https://www.apachefriends.org/download.html) (Mac, Windows, Linux).
>
> Noti anche che se sta usando MAMP ma non ha MAMP Pro installato (o se la dimostrazione temporale di MAMP Pro è scaduta), potrebbe avere difficoltà a farlo funzionare. Per farlo funzionare di nuovo, abbiamo scoperto che può caricare l'app MAMP, quindi scegliere le opzioni di menu _MAMP_ > _Preferences_ > _PHP_, e impostare "Versione standard:" su "7.2.x" (x varierà a seconda di quale versione ha installato).

### Esempio: Python

Questo esempio mostra come userebbe Python per fare la stessa cosa — visualizzare i dati inviati su una pagina web. Questo utilizza il [framework Flask](https://flask.palletsprojects.com/) per il rendering dei template, la gestione dell'invio dei dati del modulo, ecc. (veda [python-example.py](https://github.com/mdn/learning-area/blob/main/html/forms/sending-form-data/python-example.py)).

```python
from flask import Flask, render_template, request

app = Flask(__name__)

@app.route('/', methods=['GET', 'POST'])
def form():
    return render_template('form.html')

@app.route('/hello', methods=['GET', 'POST'])
def hello():
    return render_template('greeting.html', say=request.form['say'], to=request.form['to'])

if __name__ == "__main__":
    app.run()
```

I due template riferiti nel codice sopra sono i seguenti (questi devono essere in una sottodirectory chiamata `templates` nella stessa directory del file `python-example.py`, se prova a eseguire l'esempio da solo):

- [form.html](https://github.com/mdn/learning-area/blob/main/html/forms/sending-form-data/templates/form.html): Lo stesso modulo che abbiamo visto sopra nella sezione [Il metodo POST](#il_metodo_post) ma con `action` impostato su `\{{ url_for('hello') }}`. Questo è un template [Jinja](https://jinja.palletsprojects.com/), che è fondamentalmente HTML ma può contenere chiamate al codice Python che sta eseguendo il server web contenuto tra parentesi graffe. `url_for('hello')` significa essenzialmente "reindirizza a `/hello` quando il modulo viene inviato".
- [greeting.html](https://github.com/mdn/learning-area/blob/main/html/forms/sending-form-data/templates/greeting.html): Questo template contiene solo una linea che rappresenta i due dati passati ad esso quando viene rappresentato. Questo viene fatto tramite la funzione `hello()` vista sopra, che viene eseguita quando l'URL `/hello` viene navigato.

> [!NOTE]
> Di nuovo, questo codice non funzionerà se tenta di caricarlo direttamente in un browser. Python funziona un po' diversamente da PHP — per eseguire questo codice localmente deve [installare Python/PIP](/it/docs/Learn_web_development/Extensions/Server-side/Django/development_environment#installing_python_3), quindi installare Flask usando `pip3 install flask`. A questo punto, dovrebbe essere in grado di eseguire l'esempio usando `python3 python-example.py`, quindi 

navigare su `localhost:5042` nel suo browser.

### Altri linguaggi e framework

Ci sono molte altre tecnologie lato server che può utilizzare per la gestione dei moduli, inclusi Perl, Java, .Net, Ruby, ecc. Scelga quella che preferisce. Detto ciò, vale la pena notare che è molto raro utilizzare direttamente queste tecnologie perché questo può essere complicato. È più comune utilizzare uno dei tanti framework di alta qualità che facilitano la gestione dei moduli, come:

- Python
  - [Django](/it/docs/Learn_web_development/Extensions/Server-side/Django)
  - [Flask](https://flask.palletsprojects.com/)
  - [web2py](https://github.com/web2py/web2py) (il più semplice per iniziare)
  - [py4web](https://py4web.com/) (scritto dagli stessi sviluppatori di web2py, ha un'impostazione più simile a Django)
- Node.js
  - [Express](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs)
  - [Next.js](https://nextjs.org/) (per app React)
  - [Nuxt](https://nuxt.com/) (per app Vue)
  - [Remix](https://remix.run/)
- PHP
  - [Laravel](https://laravel.com/)
  - [Laminas](https://getlaminas.org/) (precedentemente Zend Framework)
  - [Symfony](https://symfony.com/)
- Ruby
  - [Ruby On Rails](https://rubyonrails.org/)
- Java
  - [Spring Boot](https://spring.io/guides/gs/handling-form-submission/)

Vale la pena notare che anche usando questi framework, lavorare con i moduli non è necessariamente _facile_. Ma è molto più semplice che cercare di scrivere tutte le funzionalità da soli da zero, e le farà risparmiare un sacco di tempo.

> [!NOTE]
> Va oltre lo scopo di questo articolo insegnarle qualsiasi linguaggio o framework lato server. I link sopra le daranno un po' di aiuto, se desidera impararli.

## Un caso speciale: invio di file

L'invio di file con moduli HTML è un caso speciale. I file sono dati binari — o considerati tali — mentre tutti gli altri dati sono dati di testo. Poiché HTTP è un protocollo di testo, ci sono requisiti speciali per la gestione dei dati binari.

### L'attributo enctype

Questo attributo le permette di specificare il valore dell'intestazione HTTP `Content-Type` inclusa nella richiesta generata quando il modulo viene inviato. Questa intestazione è molto importante perché dice al server che tipo di dati viene inviato. Per impostazione predefinita, il suo valore è `application/x-www-form-urlencoded`. In termini umani, questo significa: "Questi sono dati del modulo che sono stati codificati nei parametri URL."

Se desidera inviare file, deve seguire tre passaggi extra:

- Impostare l'attributo [`method`](/it/docs/Web/HTML/Reference/Elements/form#method) su `POST` perché il contenuto del file non può essere inserito nei parametri URL.
- Impostare il valore di [`enctype`](/it/docs/Web/HTML/Reference/Elements/form#enctype) su `multipart/form-data` perché i dati verranno suddivisi in più parti, una per ogni file più una per i dati di testo inclusi nel corpo del modulo (se anche il testo è inserito nel modulo).
- Includere uno o più controlli [`<input type="file">`](/it/docs/Web/HTML/Reference/Elements/input/file) per permettere agli utenti di selezionare i file che verranno caricati.

Per esempio:

```html
<form method="post" action="https://www.foo.com" enctype="multipart/form-data">
  <div>
    <label for="file">Choose a file</label>
    <input type="file" id="file" name="myFile" />
  </div>
  <div>
    <button>Send the file</button>
  </div>
</form>
```

> [!NOTE]
> I server possono essere configurati con un limite di dimensione per i file e le richieste HTTP per prevenire abusi.

## Questioni di sicurezza

Ogni volta che invia dati a un server, deve considerare la sicurezza. I moduli HTML sono di gran lunga i vettori di attacco server più comuni (luoghi dove possono verificarsi attacchi). I problemi non derivano mai dai moduli HTML stessi — derivano da come il server gestisce i dati.

L'articolo [Sicurezza del sito web](/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Website_security) del nostro argomento di apprendimento [lato server](/it/docs/Learn_web_development/Extensions/Server-side) discute diversi attacchi comuni e potenziali difese contro di essi in dettaglio. Dovrebbe andare a controllare quell'articolo, per avere un'idea di cosa sia possibile.

### Essere paranoico: Mai fidarsi dei propri utenti

Quindi, come si combattono queste minacce? Questo è un argomento che va ben oltre questa guida, ma ci sono alcune regole da tenere a mente. La regola più importante è: mai fidarsi dei propri utenti, incluso se stesso; anche un utente fidato potrebbe essere stato compromesso.

Tutti i dati che arrivano al suo server devono essere verificati e sanificati. Sempre. Nessuna eccezione.

- **Evitare caratteri potenzialmente pericolosi**. I caratteri specifici di cui dovrebbe essere cauto variano a seconda del contesto in cui i dati vengono utilizzati e della piattaforma server che utilizza, ma tutti i linguaggi lato server hanno funzioni per questo. Cose da tenere d'occhio sono sequenze di caratteri che sembrano codice eseguibile (come comandi [JavaScript](/it/docs/Learn_web_development/Core/Scripting) o [SQL](https://en.wikipedia.org/wiki/SQL)).
- **Limitare la quantità di dati in entrata per consentire solo ciò che è necessario**.
- **Isolare i file caricati**. Salvarli su un server separato e consentire l'accesso al file solo tramite un sottodominio diverso o, ancora meglio, tramite un dominio completamente diverso.

Dovrebbe essere in grado di evitare molti/la maggior parte dei problemi se segue queste tre regole, ma è sempre una buona idea far effettuare una revisione della sicurezza da una terza parte competente. Non presuma di aver visto tutti i possibili problemi.

## Sommario

Come accennato sopra, inviare i dati di un modulo è facile, ma proteggere un'applicazione può essere complicato. Ricordi solo che un sviluppatore front-end non è colui che dovrebbe definire il modello di sicurezza dei dati. È possibile eseguire la [validazione del modulo lato client](/it/docs/Learn_web_development/Extensions/Forms/Form_validation), ma il server non può fidarsi di questa validazione perché non ha modo di sapere realmente cosa è successo sul lato client.

Se ha seguito questi tutorial in ordine, ora sa come marcare e stilizzare un modulo, fare la validazione lato client e avere un'idea di base su come inviare un modulo.

## Vedi anche

Se desidera imparare di più sulla sicurezza di un'applicazione web, può approfondire queste risorse:

- [Primi passi nella programmazione di siti web lato server](/it/docs/Learn_web_development/Extensions/Server-side/First_steps)
- [The Open Web Application Security Project (OWASP)](https://owasp.org/)
- [Web Security by Mozilla](https://infosec.mozilla.org/guidelines/web_security)

{{PreviousMenu("Learn_web_development/Extensions/Forms/Form_validation", "Learn_web_development/Extensions/Forms")}}
