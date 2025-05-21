---
title: Invio dei dati del modulo
slug: Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data
l10n:
  sourceCommit: be1922d62a0d31e4e3441db0e943aed8df736481
---

{{PreviousMenu("Learn_web_development/Extensions/Forms/Form_validation", "Learn_web_development/Extensions/Forms")}}

Dopo che i dati del modulo sono stati convalidati lato client, è possibile inviare il modulo. E, dato che abbiamo trattato la convalida nell'articolo precedente, siamo pronti per l'invio! Questo articolo esamina cosa accade quando un utente invia un modulo: dove vanno i dati e come gestirli quando arrivano a destinazione? Esaminiamo anche alcune problematiche di sicurezza legate all'invio dei dati del modulo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Una
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >comprensione di HTML</a
        > e una conoscenza di base di
        <a href="/it/docs/Web/HTTP">HTTP</a> e
        <a href="/it/docs/Learn_web_development/Extensions/Server-side/First_steps"
          >programmazione lato server</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere cosa accade quando vengono inviati i dati di un modulo,
        includendo un'idea di base su come i dati vengono elaborati sul server.
      </td>
    </tr>
  </tbody>
</table>

Per prima cosa, discuteremo cosa accade ai dati quando viene inviato un modulo.

## Architettura client/server

Nel suo concetto più semplice, il web utilizza un'architettura client/server che può essere riassunta come segue: un client (solitamente un browser web) invia una richiesta a un server (la maggior parte delle volte un server web come [Apache](https://httpd.apache.org/), [Nginx](https://nginx.org/), [IIS](https://www.iis.net/), [Tomcat](https://tomcat.apache.org/), ecc.), utilizzando il [protocollo HTTP](/it/docs/Web/HTTP). Il server risponde alla richiesta utilizzando lo stesso protocollo.

![Uno schema di base dell'architettura client/server del Web](client-server.png)

Un modulo HTML su una pagina web è nient'altro che un modo comodo e user-friendly per configurare una richiesta HTTP per inviare dati a un server. Questo consente all'utente di fornire informazioni da consegnare nella richiesta HTTP.

> [!NOTE]
> Per avere un'idea migliore di come funzionano le architetture client-server, leggi il nostro modulo [Primi passi con la programmazione di siti web lato server](/it/docs/Learn_web_development/Extensions/Server-side/First_steps).

## Lato client: definire come inviare i dati

L'elemento {{HTMLElement("form")}} definisce come i dati verranno inviati. Tutti i suoi attributi sono progettati per consentire la configurazione della richiesta da inviare quando un utente preme un {{Glossary("submit_button", "pulsante di invio")}}. Gli attributi più importanti sono [`action`](/it/docs/Web/HTML/Reference/Elements/form#action) e [`method`](/it/docs/Web/HTML/Reference/Elements/form#method).

### L'attributo action

L'attributo [`action`](/it/docs/Web/HTML/Reference/Elements/form#action) definisce dove vengono inviati i dati. Il suo valore deve essere un [URL](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_URL) valido, relativo o assoluto. Se questo attributo non è fornito, i dati saranno inviati all'URL della pagina contenente il modulo, ovvero la pagina corrente.

In questo esempio, i dati sono inviati a un URL assoluto — `https://example.com`:

```html
<form action="https://example.com">…</form>
```

Qui, utilizziamo un URL relativo — i dati sono inviati a un URL diverso sulla stessa origine:

```html
<form action="/somewhere_else">…</form>
```

Quando specificato senza attributi, come qui sotto, i dati del {{HTMLElement("form")}} sono inviati alla stessa pagina in cui si trova il modulo:

```html
<form>…</form>
```

> [!NOTE]
> È possibile specificare un URL che utilizzi il protocollo HTTPS (HTTP sicuro). Quando lo fai, i dati vengono criptati insieme al resto della richiesta, anche se il modulo stesso è ospitato su una pagina non sicura accessibile tramite HTTP. D'altra parte, se il modulo è ospitato su una pagina sicura ma si specifica un URL HTTP non sicuro con l'attributo [`action`](/it/docs/Web/HTML/Reference/Elements/form#action), tutti i browser visualizzeranno un avviso di sicurezza all'utente ogni volta che tenta di inviare dati, poiché non saranno criptati.

I nomi e i valori dei controlli del modulo non-file sono inviati al server come coppie `name=value` unite da `&`. Il valore `action` dovrebbe essere un file sul server in grado di gestire i dati in ingresso, incluso assicurare la convalida lato server. Il server poi risponde, generalmente gestendo i dati e caricando l'URL definito dall'attributo `action`, causando un nuovo caricamento della pagina (o un refresh della pagina esistente, se `action` punta alla stessa pagina).

Come i dati sono inviati dipende dall'attributo `method`.

### L'attributo method

L'attributo [`method`](/it/docs/Web/HTML/Reference/Elements/form#method) definisce come i dati sono inviati. Il [protocollo HTTP](/it/docs/Web/HTTP) fornisce diversi modi per eseguire una richiesta; i dati del modulo HTML possono essere trasmessi tramite diversi metodi, i più comuni sono il metodo `GET` e il metodo `POST`.

Per capire la differenza tra questi due metodi, facciamo un passo indietro ed esaminiamo [come funziona l'HTTP](/it/docs/Web/HTTP/Guides/Overview). Ogni volta che desideri raggiungere una risorsa sul Web, il browser invia una richiesta a un URL. Una richiesta HTTP è composta da due parti: un [header](/it/docs/Web/HTTP/Reference/Headers) che contiene un set di metadati globali sulle capacità del browser e un corpo che può contenere informazioni necessarie per il server per elaborare la richiesta specifica.

#### Il metodo GET

Il [`metodo GET`](/it/docs/Web/HTTP/Reference/Methods/GET) è il metodo utilizzato dal browser per chiedere al server di inviare una risorsa specifica: "Hey server, voglio ottenere questa risorsa." In questo caso, il browser invia un corpo vuoto. Poiché il corpo è vuoto, se un modulo è inviato utilizzando questo metodo, i dati inviati al server sono aggiunti all'URL.

Considera il seguente modulo:

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

Poiché è stato utilizzato il metodo `GET`, vedrai l'URL `www.foo.com/?say=Hi&to=Mom` apparire nella barra degli indirizzi del browser quando invii il modulo.

![L'URL cambiato con parametri di query dopo l'invio del modulo con il metodo GET e una pagina di errore del browser "server non trovato"](url-parameters.png)

I dati sono aggiunti all'URL come una serie di coppie nome/valore. Dopo che l'indirizzo web dell'URL è terminato, includiamo un punto interrogativo (`?`) seguito dalle coppie nome/valore, ognuna separata da un `&`. In questo caso, stiamo passando due pezzi di dati al server:

- `say`, che ha un valore di `Hi`
- `to`, che ha un valore di `Mom`

La richiesta HTTP appare come segue:

```http
GET /?say=Hi&to=Mom HTTP/2.0
Host: foo.com
```

> [!NOTE]
> Puoi trovare questo esempio su GitHub — vedi [get-method.html](https://github.com/mdn/learning-area/blob/main/html/forms/sending-form-data/get-method.html) ([vedi anche dal vivo](https://mdn.github.io/learning-area/html/forms/sending-form-data/get-method.html)).

> [!NOTE]
> I dati non saranno aggiunti se lo schema URL `action` non può gestire le query, ad esempio `file:`.

#### Il metodo POST

Il [`metodo POST`](/it/docs/Web/HTTP/Reference/Methods/POST) è un po' diverso. È il metodo che il browser utilizza per parlare con il server quando richiede una risposta che consideri i dati forniti nel corpo della richiesta HTTP: "Hey server, dai un'occhiata a questi dati e inviami un risultato appropriato." Se un modulo è inviato utilizzando questo metodo, i dati sono aggiunti al corpo della richiesta HTTP.

Vediamo un esempio: questo è lo stesso modulo che abbiamo visto nella sezione `GET` sopra, ma con l'attributo [`method`](/it/docs/Web/HTML/Reference/Elements/form#method) impostato su `POST`.

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

Quando il modulo è inviato utilizzando il metodo `POST`, non vedi dati aggiunti all'URL, e la richiesta HTTP appare come segue, con i dati inclusi nel corpo della richiesta:

```http
POST / HTTP/2.0
Host: foo.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 13

say=Hi&to=Mom
```

L'intestazione `Content-Length` indica la dimensione del corpo, e l'intestazione `Content-Type` indica il tipo di risorsa inviata al server. Discuteremo queste intestazioni più avanti.

> [!NOTE]
> Puoi trovare questo esempio su GitHub — vedi [post-method.html](https://github.com/mdn/learning-area/blob/main/html/forms/sending-form-data/post-method.html) ([vedi anche dal vivo](https://mdn.github.io/learning-area/html/forms/sending-form-data/post-method.html)).

> [!NOTE]
> Il metodo `GET` sarà utilizzato invece se lo schema URL `action` non può gestire un corpo di richiesta, ad esempio `data:`.

### Visualizzazione delle richieste HTTP

Le richieste HTTP non sono mai visualizzate all'utente (se vuoi vederle, devi usare strumenti come il [Monitor di Rete di Firefox](https://firefox-source-docs.mozilla.org/devtools-user/network_monitor/index.html) o i [Chrome Developer Tools](https://developer.chrome.com/docs/devtools/)). Ad esempio, i tuoi dati del modulo saranno visualizzati come segue nella scheda Rete di Chrome. Dopo aver inviato il modulo:

1. Apri gli strumenti per gli sviluppatori.
2. Seleziona "Network"
3. Seleziona "All"
4. Seleziona "foo.com" nella scheda "Name"
5. Seleziona "Request" (Firefox) o "Payload" (Chrome/Edge)

Puoi quindi ottenere i dati del modulo, come mostrato nell'immagine qui sotto.

![Richieste HTTP e dati di risposta nella scheda del monitoraggio di rete negli strumenti per sviluppatori del browser](network-monitor.png)

L'unica cosa visualizzata all'utente è l'URL chiamato. Come abbiamo menzionato sopra, con una richiesta `GET` l'utente vedrà i dati nella barra degli URL, ma con una richiesta `POST` no. Questo può essere molto importante per due motivi:

1. Se hai bisogno di inviare una password (o qualsiasi altro dato sensibile), non usare mai il metodo `GET` altrimenti rischi di visualizzarlo nella barra degli URL, il che sarebbe molto insicuro.
2. Se hai bisogno di inviare una grande quantità di dati, il metodo `POST` è preferibile perché alcuni browser limitano le dimensioni degli URL. Inoltre, molti server limitano la lunghezza degli URL che accettano.

## Lato server: recupero dei dati

Qualunque sia il metodo HTTP che scegli, il server riceve una stringa che sarà analizzata per ottenere i dati come una lista di coppie chiave/valore. Il modo in cui accedi a questa lista dipende dalla piattaforma di sviluppo che utilizzi e da eventuali specifici framework che potresti utilizzare con essa.

### Esempio: PHP grezzo

[PHP](https://www.php.net/) offre alcuni oggetti globali per accedere ai dati. Supponendo tu abbia usato il metodo `POST`, il seguente esempio prende semplicemente i dati e li visualizza all'utente. Naturalmente, cosa fai con i dati è a tua discrezione. Potresti visualizzarli, memorizzarli in un database, inviarli tramite email o elaborarli in qualche altro modo.

```php
<?php
  // The global $_POST variable allows you to access the data sent with the POST method by name
  // To access the data sent with the GET method, you can use $_GET
  $say = htmlspecialchars($_POST["say"]);
  $to  = htmlspecialchars($_POST["to"]);

  echo  $say, " ", $to;
?>
```

Questo esempio visualizza una pagina con i dati che abbiamo inviato. Puoi vedere questo in azione nel nostro esempio nel file [php-example.html](https://github.com/mdn/learning-area/blob/main/html/forms/sending-form-data/php-example.html) — che contiene lo stesso modulo di esempio che abbiamo visto prima, con un metodo di `POST` e un'`action` di `php-example.php`. Quando viene inviato, invia i dati del modulo a [php-example.php](https://github.com/mdn/learning-area/blob/main/html/forms/sending-form-data/php-example.php), che contiene il codice PHP visto nel blocco sopra. Quando questo codice viene eseguito, l'output nel browser è `Hi Mom`.

![Pagina web altrimenti vuota con "hi mom", i dati ricevuti in risposta dopo l'invio dei dati del modulo a un file PHP con il metodo POST](php-result.png)

> [!NOTE]
> Questo esempio non funzionerà quando lo carichi in un browser localmente — i browser non possono interpretare il codice PHP, quindi quando il modulo viene inviato, il browser proporrà semplicemente di scaricare il file PHP per te. Per farlo funzionare, è necessario eseguire l'esempio tramite un server PHP di qualche tipo. Buone opzioni per il test PHP locale sono [MAMP](https://www.mamp.info/en/downloads/) (Mac e Windows) e [XAMPP](https://www.apachefriends.org/download.html) (Mac, Windows, Linux).
>
> Nota inoltre che se stai usando MAMP ma non hai installato MAMP Pro (o se la demo di prova di MAMP Pro è scaduta), potresti avere problemi a farlo funzionare. Abbiamo scoperto che puoi caricare l'app MAMP, quindi scegliere le opzioni del menu _MAMP_ > _Preferences_ > _PHP_, e impostare "Standard Version:" su "7.2.x" (x varierà a seconda della versione installata).

### Esempio: Python

Questo esempio mostra come utilizzeresti Python per fare lo stesso: visualizzare i dati inviati su una pagina web. Questo utilizza il [framework Flask](https://flask.palletsprojects.com/) per rendere i template, gestire l'invio dei dati del modulo, ecc. (vedi [python-example.py](https://github.com/mdn/learning-area/blob/main/html/forms/sending-form-data/python-example.py)).

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

I due template menzionati nel codice sopra sono i seguenti (devono essere in una sottodirectory chiamata `templates` nella stessa directory del file `python-example.py`, se provi a eseguire l'esempio):

- [form.html](https://github.com/mdn/learning-area/blob/main/html/forms/sending-form-data/templates/form.html): Lo stesso modulo che abbiamo visto sopra nella sezione [Il metodo POST](#il_metodo_post) ma con `action` impostato su `\{{ url_for('hello') }}`. Questo è un template [Jinja](https://jinja.palletsprojects.com/), che è fondamentalmente HTML ma può contenere chiamate al codice Python che sta eseguendo il server web contenuto tra parentesi graffe. `url_for('hello')` sta praticamente dicendo "reindirizza a `/hello` quando il modulo viene inviato".
- [greeting.html](https://github.com/mdn/learning-area/blob/main/html/forms/sending-form-data/templates/greeting.html): Questo template contiene solo una linea che rende visibili i due pezzi di dati passati quando viene eseguito il rendering. Questo è fatto tramite la funzione `hello()` vista sopra, che viene eseguita quando si naviga all'URL `/hello`.

> [!NOTE]
> Anche in questo caso, questo codice non funzionerà se provi semplicemente a caricarlo in un browser direttamente. Python funziona in modo un po' diverso da PHP: per eseguire questo codice localmente, dovrai [installare Python/PIP](/it/docs/Learn_web_development/Extensions/Server-side/Django/development_environment#installing_python_3), quindi installare Flask utilizzando `pip3 install flask`. A questo punto, dovresti essere in grado di eseguire l'esempio usando `python3 python-example.py`, quindi navigare su `localhost:5042` nel tuo browser.

### Altri linguaggi e framework

Ci sono molte altre tecnologie lato server che puoi usare per gestire i moduli, inclusi Perl, Java, .Net, Ruby, ecc. Scegli quello che preferisci. Detto ciò, è importante notare che è molto raro utilizzare direttamente queste tecnologie, perché questo può essere complicato. È più comune utilizzare uno dei tanti framework di alta qualità che facilitano la gestione dei moduli, come:

- Python
  - [Django](/it/docs/Learn_web_development/Extensions/Server-side/Django)
  - [Flask](https://flask.palletsprojects.com/)
  - [web2py](https://github.com/web2py/web2py) (più facile da iniziare)
  - [py4web](https://py4web.com/) (scritto dagli stessi sviluppatori di web2py, ha una configurazione più simile a Django)
- Node.js
  - [Express](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs)
  - [Next.js](https://nextjs.org/) (per app React)
  - [Nuxt](https://nuxt.com/) (per app Vue)
  - [Remix](https://remix.run/)
- PHP
  - [Laravel](https://laravel.com/)
  - [Laminas](https://getlaminas.org/) (prima Zend Framework)
  - [Symfony](https://symfony.com/)
- Ruby
  - [Ruby On Rails](https://rubyonrails.org/)
- Java
  - [Spring Boot](https://spring.io/guides/gs/handling-form-submission/)

È importante notare che anche utilizzando questi framework, lavorare con i moduli non è necessariamente _facile_. Ma è molto più semplice che cercare di scrivere tutte le funzionalità da soli da zero, e questo ti farà risparmiare molto tempo.

> [!NOTE]
> È al di fuori dell'ambito di questo articolo insegnarti qualsiasi linguaggio o framework lato server. I link sopra ti daranno un po' di aiuto, se desideri impararli.

## Un caso speciale: invio di file

Inviare file con moduli HTML è un caso speciale. I file sono dati binari — o vengono considerati tali — mentre tutti gli altri dati sono dati di testo. Poiché HTTP è un protocollo di testo, ci sono requisiti speciali per la gestione dei dati binari.

### L'attributo enctype

Questo attributo ti permette di specificare il valore dell'intestazione HTTP `Content-Type` inclusa nella richiesta generata quando il modulo viene inviato. Questa intestazione è molto importante perché dice al server di che tipo di dati si tratta. Di default, il suo valore è `application/x-www-form-urlencoded`. In termini umani, questo significa: "Questi sono dati del modulo codificati come parametri URL."

Se vuoi inviare file, devi fare tre passaggi extra:

- Imposta l'attributo [`method`](/it/docs/Web/HTML/Reference/Elements/form#method) su `POST` perché il contenuto dei file non può essere inserito nei parametri URL.
- Imposta il valore di [`enctype`](/it/docs/Web/HTML/Reference/Elements/form#enctype) su `multipart/form-data` perché i dati saranno divisi in più parti, una per ogni file più una per i dati di testo inclusi nel corpo del modulo (se il testo è anche inserito nel modulo).
- Includi uno o più controlli [`<input type="file">`](/it/docs/Web/HTML/Reference/Elements/input/file) per consentire agli utenti di selezionare i file che saranno caricati.

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
> I server possono essere configurati con un limite di dimensione per file e richieste HTTP per prevenire abusi.

## Problemi di sicurezza

Ogni volta che invii dati a un server, devi considerare la sicurezza. I moduli HTML sono di gran lunga i vettori di attacco più comuni per il server (luoghi dove possono avvenire attacchi). I problemi non provengono mai dai moduli HTML stessi — provengono da come il server gestisce i dati.

L'articolo sulla [sicurezza del sito web](/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Website_security) del nostro argomento di apprendimento [lato server](/it/docs/Learn_web_development/Extensions/Server-side) discute in dettaglio diversi attacchi comuni e potenziali difese contro di essi. Dovresti controllare quell'articolo, per avere un'idea di cosa è possibile.

### Sii paranoico: mai fidarsi degli utenti

Quindi, come si affrontano queste minacce? Questo è un argomento molto oltre questa guida, ma ci sono alcune regole da tenere a mente. La regola più importante è: mai fidarsi degli utenti, inclusi se stessi; anche un utente fidato potrebbe essere stato compromesso.

Tutti i dati che arrivano al tuo server devono essere verificati e sanitizzati. Sempre. Nessuna eccezione.

- **Esegui l'escape dei caratteri potenzialmente pericolosi**. I caratteri specifici di cui dovresti essere cauto variano a seconda del contesto in cui i dati sono utilizzati e della piattaforma server che impieghi, ma tutti i linguaggi lato server hanno funzioni per questo. Le cose da tenere d'occhio sono sequenze di caratteri che sembrano codice eseguibile (come i comandi [JavaScript](/it/docs/Learn_web_development/Core/Scripting) o [SQL](https://it.wikipedia.org/wiki/SQL)).
- **Limita la quantità di dati in ingresso per consentire solo ciò che è necessario**.
- **Isola i file caricati**. Salvali su un server diverso e consenti l'accesso al file solo tramite un sottodominio diverso o meglio ancora attraverso un dominio completamente diverso.

Dovresti essere in grado di evitare molti problemi se segui queste tre regole, ma è sempre una buona idea ottenere una revisione della sicurezza effettuata da una terza parte competente. Non assumere di aver visto tutti i possibili problemi.

## Riepilogo

Come abbiamo accennato sopra, inviare dati da un modulo è facile, ma proteggere un'applicazione può essere complicato. Ricorda solo che uno sviluppatore front-end non è colui che dovrebbe definire il modello di sicurezza dei dati. È possibile effettuare una [convalida del modulo lato client](/it/docs/Learn_web_development/Extensions/Forms/Form_validation), ma il server non può fidarsi di questa convalida perché non ha modo di sapere veramente cosa è accaduto lato client.

Se hai seguito questi tutorial in ordine, ora sai come strutturare e stilare un modulo, eseguire la convalida lato client e avere un'idea su come inviare un modulo.

## Vedi anche

Se vuoi saperne di più su come proteggere un'applicazione web, puoi immergerti in queste risorse:

- [Primi passi con la programmazione di siti web lato server](/it/docs/Learn_web_development/Extensions/Server-side/First_steps)
- [Project Open Web Application Security (OWASP)](https://owasp.org/)
- [Sicurezza Web di Mozilla](https://infosec.mozilla.org/guidelines/web_security)

{{PreviousMenu("Learn_web_development/Extensions/Forms/Form_validation", "Learn_web_development/Extensions/Forms")}}
