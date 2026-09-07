---
title: Invio dei dati dei moduli
slug: Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data
l10n:
  sourceCommit: 2066cc916dfdcbb782340bf0ce562b230e947cba
---

{{PreviousMenu("Learn_web_development/Extensions/Forms/Form_validation", "Learn_web_development/Extensions/Forms")}}

Dopo aver convalidato i dati del modulo lato client, è possibile inviare il modulo. Poiché la convalida è stata trattata nell'articolo precedente, è tutto pronto per l'invio. Questo articolo esamina cosa accade quando un utente invia un modulo: dove vanno i dati e come vengono gestiti quando arrivano a destinazione? Verranno inoltre analizzati alcuni problemi di sicurezza associati all'invio dei dati dei moduli.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Una
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >comprensione di HTML</a
        > e conoscenze di base di
        <a href="/it/docs/Web/HTTP">HTTP</a> e della
        <a href="/it/docs/Learn_web_development/Extensions/Server-side/First_steps"
          >programmazione lato server</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere cosa accade quando vengono inviati i dati di un modulo,
        incluso farsi un'idea di base di come i dati vengono elaborati sul server.
      </td>
    </tr>
  </tbody>
</table>

Per prima cosa, verrà illustrato cosa accade ai dati quando viene inviato un modulo.

## Architettura client/server

Nella sua forma più semplice, il Web utilizza un'architettura client/server che può essere riassunta come segue: un client (solitamente un browser web) invia una richiesta a un server (nella maggior parte dei casi un server web come [Apache](https://httpd.apache.org/), [Nginx](https://nginx.org/), [IIS](https://www.iis.net/), [Tomcat](https://tomcat.apache.org/), ecc.), utilizzando il [protocollo HTTP](/it/docs/Web/HTTP). Il server risponde alla richiesta utilizzando lo stesso protocollo.

![Schema di base dell'architettura client/server del Web](client-server.png)

Un modulo HTML in una pagina web non è altro che un modo pratico e intuitivo per configurare una richiesta HTTP che invii dati a un server. Ciò consente all'utente di fornire informazioni da trasmettere nella richiesta HTTP.

> [!NOTE]
> Per avere un'idea più chiara di come funzionano le architetture client-server, leggere il modulo [Primi passi nella programmazione di siti web lato server](/it/docs/Learn_web_development/Extensions/Server-side/First_steps).

## Lato client: definire come inviare i dati

L'elemento {{HTMLElement("form")}} definisce come verranno inviati i dati. Tutti i suoi attributi sono progettati per consentire di configurare la richiesta da inviare quando un utente preme un {{Glossary("submit_button", "pulsante di invio")}}. I due attributi più importanti sono [`action`](/it/docs/Web/HTML/Reference/Elements/form#action) e [`method`](/it/docs/Web/HTML/Reference/Elements/form#method).

### L'attributo action

L'attributo [`action`](/it/docs/Web/HTML/Reference/Elements/form#action) definisce dove vengono inviati i dati. Il relativo valore deve essere un [URL](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_URL) relativo o assoluto valido. Se questo attributo non viene fornito, i dati verranno inviati all'URL della pagina contenente il modulo, ovvero la pagina corrente.

In questo esempio, i dati vengono inviati a un URL assoluto: `https://www.example.com`:

```html
<form action="https://www.example.com">…</form>
```

Qui viene utilizzato un URL relativo: i dati vengono inviati a un URL diverso nella stessa origine:

```html
<form action="/somewhere_else">…</form>
```

Quando non vengono specificati attributi, come di seguito, i dati di {{HTMLElement("form")}} vengono inviati alla stessa pagina in cui è presente il modulo:

```html
<form>…</form>
```

> [!NOTE]
> È possibile specificare un URL che utilizza il protocollo HTTPS (HTTP sicuro). In questo caso, i dati vengono crittografati insieme al resto della richiesta, anche se il modulo stesso è ospitato su una pagina non sicura accessibile tramite HTTP. D'altra parte, se il modulo è ospitato su una pagina sicura ma viene specificato un URL HTTP non sicuro con l'attributo [`action`](/it/docs/Web/HTML/Reference/Elements/form#action), tutti i browser visualizzano un avviso di sicurezza all'utente ogni volta che tenta di inviare dati, perché tali dati non verranno crittografati.

I nomi e i valori dei controlli del modulo che non sono file vengono inviati al server come coppie `name=value` unite da e commerciali. Il valore di `action` dovrebbe essere un file sul server in grado di gestire i dati in ingresso, compresa la convalida lato server. Il server risponde quindi, generalmente gestendo i dati e caricando l'URL definito dall'attributo `action`, causando il caricamento di una nuova pagina (o l'aggiornamento della pagina esistente, se `action` punta alla stessa pagina).

Il modo in cui i dati vengono inviati dipende dall'attributo `method`.

### L'attributo method

L'attributo [`method`](/it/docs/Web/HTML/Reference/Elements/form#method) definisce come vengono inviati i dati. Il [protocollo HTTP](/it/docs/Web/HTTP) offre diversi modi per eseguire una richiesta; i dati dei moduli HTML possono essere trasmessi tramite vari metodi, i più comuni dei quali sono il metodo `GET` e il metodo `POST`.

Per comprendere la differenza fra questi due metodi, occorre fare un passo indietro ed esaminare [come funziona HTTP](/it/docs/Web/HTTP/Guides/Overview). Ogni volta che si vuole raggiungere una risorsa sul Web, il browser invia una richiesta a un URL. Una richiesta HTTP è composta da due parti: un'[intestazione](/it/docs/Web/HTTP/Reference/Headers) che contiene un insieme di metadati globali sulle capacità del browser, e un corpo che può contenere le informazioni necessarie al server per elaborare la richiesta specifica.

#### Il metodo GET

Il [`metodo GET`](/it/docs/Web/HTTP/Reference/Methods/GET) è il metodo usato dal browser per chiedere al server di restituire una determinata risorsa: "Ehi server, voglio ottenere questa risorsa". In questo caso, il browser invia un corpo vuoto. Poiché il corpo è vuoto, se un modulo viene inviato usando questo metodo, i dati inviati al server vengono aggiunti all'URL.

Si consideri il seguente modulo:

```html
<form action="https://www.example.com" method="GET">
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

Poiché è stato utilizzato il metodo `GET`, dopo l'invio del modulo nella barra degli indirizzi del browser verrà visualizzato l'URL `https://www.example.com/?say=Hi&to=Mom`.

![URL modificato con parametri di query dopo l'invio del modulo con il metodo GET](url-parameters.png)

I dati vengono aggiunti all'URL come una serie di coppie nome/valore. Dopo l'indirizzo web URL viene inserito un punto interrogativo (`?`) seguito dalle coppie nome/valore, ciascuna separata da una e commerciale (`&`). In questo caso, vengono passate al server due informazioni:

- `say`, che ha il valore `Hi`
- `to`, che ha il valore `Mom`

La richiesta HTTP appare così:

```http
GET /?say=Hi&to=Mom HTTP/2.0
Host: example.com
```

> [!NOTE]
> Questo esempio è disponibile su GitHub: vedere [get-method.html](https://github.com/mdn/learning-area/blob/main/html/forms/sending-form-data/get-method.html) ([visualizzarlo anche dal vivo](https://mdn.github.io/learning-area/html/forms/sending-form-data/get-method.html)).

> [!NOTE]
> I dati non verranno aggiunti se lo schema URL di `action` non può gestire query, ad esempio `file:`.

#### Il metodo POST

Il [`metodo POST`](/it/docs/Web/HTTP/Reference/Methods/POST) è leggermente diverso. È il metodo che il browser utilizza per comunicare con il server quando richiede una risposta che tenga conto dei dati forniti nel corpo della richiesta HTTP: "Ehi server, esamina questi dati e inviami un risultato appropriato". Se un modulo viene inviato utilizzando questo metodo, i dati vengono aggiunti al corpo della richiesta HTTP.

Si osservi un esempio: è lo stesso modulo visto nella sezione precedente su `GET`, ma con l'attributo [`method`](/it/docs/Web/HTML/Reference/Elements/form#method) impostato su `POST`.

```html
<form action="https://www.example.com" method="POST">
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

Quando il modulo viene inviato utilizzando il metodo `POST`, non vengono aggiunti dati all'URL e la richiesta HTTP appare come segue, con i dati inclusi invece nel corpo della richiesta:

```http
POST / HTTP/2.0
Host: example.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 13

say=Hi&to=Mom
```

L'intestazione `Content-Length` indica la dimensione del corpo, mentre l'intestazione `Content-Type` indica il tipo di risorsa inviata al server. Queste intestazioni verranno analizzate in seguito.

> [!NOTE]
> Questo esempio è disponibile su GitHub: vedere [post-method.html](https://github.com/mdn/learning-area/blob/main/html/forms/sending-form-data/post-method.html) ([visualizzarlo anche dal vivo](https://mdn.github.io/learning-area/html/forms/sending-form-data/post-method.html)).

> [!NOTE]
> Verrà utilizzato il metodo `GET` se lo schema URL di `action` non può gestire un corpo della richiesta, ad esempio `data:`.

### Visualizzare le richieste HTTP

Le richieste HTTP non vengono mai visualizzate all'utente (per vederle, occorre utilizzare strumenti come [Firefox Network Monitor](https://firefox-source-docs.mozilla.org/devtools-user/network_monitor/index.html) o gli [strumenti per sviluppatori di Chrome](https://developer.chrome.com/docs/devtools/)). Ad esempio, i dati del modulo verranno visualizzati come segue nella scheda Network di Chrome. Dopo aver inviato il modulo:

1. Aprire gli strumenti per sviluppatori.
2. Selezionare "Network".
3. Selezionare "All".
4. Selezionare "example.com" nella scheda "Name".
5. Selezionare "Request" (Firefox) o "Payload" (Chrome/Edge).

Sarà quindi possibile ottenere i dati del modulo, come mostrato nell'immagine seguente.

![Richieste HTTP e dati di risposta nella scheda di monitoraggio della rete negli strumenti per sviluppatori del browser](network-monitor.png)

L'unica cosa visualizzata all'utente è l'URL chiamato. Come già detto, con una richiesta `GET` l'utente vedrà i dati nella barra dell'URL, mentre con una richiesta `POST` non li vedrà. Ciò può essere molto importante per due motivi:

1. Se occorre inviare una password, o qualsiasi altro dato sensibile, non usare mai il metodo `GET`, altrimenti si rischia di visualizzarla nella barra dell'URL, cosa che sarebbe molto insicura.
2. Se occorre inviare una grande quantità di dati, è preferibile il metodo `POST`, poiché alcuni browser limitano le dimensioni degli URL. Inoltre, molti server limitano la lunghezza degli URL che accettano.

## Lato server: recuperare i dati

Indipendentemente dal metodo HTTP scelto, il server riceve una stringa che verrà analizzata per ottenere i dati come elenco di coppie chiave/valore. Il modo di accedere a questo elenco dipende dalla piattaforma di sviluppo utilizzata e da eventuali framework specifici impiegati con essa.

### Esempio: PHP puro

[PHP](https://www.php.net/) offre alcuni oggetti globali per accedere ai dati. Supponendo di aver utilizzato il metodo `POST`, l'esempio seguente si limita a recuperare i dati e visualizzarli all'utente. Naturalmente, cosa fare con i dati dipende dall'applicazione: potrebbero essere visualizzati, memorizzati in un database, inviati via email o elaborati in altro modo.

```php
<?php
  // The global $_POST variable allows you to access the data sent with the POST method by name
  // To access the data sent with the GET method, you can use $_GET
  $say = htmlspecialchars($_POST["say"]);
  $to  = htmlspecialchars($_POST["to"]);

  echo  $say, " ", $to;
?>
```

Questo esempio visualizza una pagina con i dati inviati. È possibile vederlo in azione nel file di esempio [php-example.html](https://github.com/mdn/learning-area/blob/main/html/forms/sending-form-data/php-example.html), che contiene lo stesso modulo di esempio visto in precedenza, con un `method` pari a `POST` e un'`action` pari a `php-example.php`. Quando viene inviato, trasmette i dati del modulo a [php-example.php](https://github.com/mdn/learning-area/blob/main/html/forms/sending-form-data/php-example.php), che contiene il codice PHP mostrato nel blocco precedente. Quando questo codice viene eseguito, l'output nel browser è `Hi Mom`.

![Pagina web altrimenti vuota con "hi mom", i dati ricevuti in risposta dopo l'invio dei dati del modulo a un file PHP con il metodo POST](php-result.png)

> [!NOTE]
> Questo esempio non funzionerà se viene caricato localmente in un browser: i browser non possono interpretare il codice PHP, pertanto, quando il modulo viene inviato, il browser proporrà semplicemente di scaricare il file PHP. Per farlo funzionare, occorre eseguire l'esempio tramite un qualche tipo di server PHP. Buone opzioni per testare PHP localmente sono [MAMP](https://www.mamp.info/en/downloads/) (Mac e Windows) e [XAMPP](https://www.apachefriends.org/download.html) (Mac, Windows, Linux).
>
> Si noti inoltre che, se si utilizza MAMP ma non è installato MAMP Pro, oppure se la prova demo di MAMP Pro è scaduta, potrebbe essere difficile farlo funzionare. Per farlo funzionare nuovamente, è possibile aprire l'app MAMP, quindi scegliere le opzioni di menu _MAMP_ > _Preferences_ > _PHP_ e impostare "Standard Version:" su "7.2.x" (la x varierà a seconda della versione installata).

### Esempio: Python

Questo esempio mostra come utilizzare Python per fare la stessa cosa: visualizzare i dati inviati in una pagina web. Utilizza il [framework Flask](https://flask.palletsprojects.com/) per il rendering dei template, la gestione dell'invio dei dati del modulo e così via (vedere [python-example.py](https://github.com/mdn/learning-area/blob/main/html/forms/sending-form-data/python-example.py)).

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

I due template a cui fa riferimento il codice precedente sono i seguenti (se si prova a eseguire l'esempio, devono trovarsi in una sottodirectory denominata `templates`, nella stessa directory del file `python-example.py`):

- [form.html](https://github.com/mdn/learning-area/blob/main/html/forms/sending-form-data/templates/form.html): lo stesso modulo visto sopra nella sezione [Il metodo POST](#il_metodo_post), ma con `action` impostato su `\{{ url_for('hello') }}`. Si tratta di un template [Jinja](https://jinja.palletsprojects.com/), che è fondamentalmente HTML ma può contenere chiamate al codice Python in esecuzione sul server web racchiuse tra parentesi graffe. `url_for('hello')` indica essenzialmente "reindirizza a `/hello` quando il modulo viene inviato".
- [greeting.html](https://github.com/mdn/learning-area/blob/main/html/forms/sending-form-data/templates/greeting.html): questo template contiene semplicemente una riga che renderizza i due dati passati quando viene renderizzato. Ciò avviene tramite la funzione `hello()` vista sopra, che viene eseguita quando si naviga all'URL `/hello`.

> [!NOTE]
> Anche in questo caso, il codice non funzionerà se si prova semplicemente a caricarlo direttamente in un browser. Python funziona in modo leggermente diverso da PHP: per eseguire questo codice localmente occorre [installare Python/PIP](/it/docs/Learn_web_development/Extensions/Server-side/Django/development_environment#installing_python_3), quindi installare Flask utilizzando `pip3 install flask`. A questo punto, dovrebbe essere possibile eseguire l'esempio usando `python3 python-example.py`, quindi navigare a `localhost:5042` nel browser.

### Altri linguaggi e framework

Esistono molte altre tecnologie lato server utilizzabili per la gestione dei moduli, tra cui Perl, Java, .Net, Ruby e altre. Basta scegliere quella preferita. Detto ciò, vale la pena notare che è molto raro usare queste tecnologie direttamente, poiché può essere difficile. È più comune utilizzare uno dei numerosi framework di alta qualità che semplificano la gestione dei moduli, come:

- Python
  - [Django](/it/docs/Learn_web_development/Extensions/Server-side/Django)
  - [Flask](https://flask.palletsprojects.com/)
  - [web2py](https://github.com/web2py/web2py) (il più semplice con cui iniziare)
  - [py4web](https://py4web.com/) (scritto dagli stessi sviluppatori di web2py, con una configurazione più simile a Django)
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

Vale la pena notare che, anche utilizzando questi framework, lavorare con i moduli non è necessariamente _facile_. Tuttavia, è molto più semplice rispetto a tentare di scrivere tutte le funzionalità autonomamente da zero e consentirà di risparmiare molto tempo.

> [!NOTE]
> L'insegnamento di linguaggi o framework lato server non rientra nell'ambito di questo articolo. I collegamenti precedenti forniranno aiuto a chi desideri impararli.

## Un caso speciale: inviare file

L'invio di file con moduli HTML è un caso speciale. I file sono dati binari, o vengono considerati tali, mentre tutti gli altri dati sono dati testuali. Poiché HTTP è un protocollo testuale, esistono requisiti speciali per la gestione dei dati binari.

### L'attributo enctype

Questo attributo consente di specificare il valore dell'intestazione HTTP `Content-Type` inclusa nella richiesta generata quando il modulo viene inviato. Questa intestazione è molto importante perché indica al server quale tipo di dati viene inviato. Per impostazione predefinita, il suo valore è `application/x-www-form-urlencoded`. In termini comuni, ciò significa: "Questi sono dati di modulo codificati in parametri URL".

Per inviare file, occorre eseguire tre passaggi aggiuntivi:

- Impostare l'attributo [`method`](/it/docs/Web/HTML/Reference/Elements/form#method) su `POST`, poiché il contenuto dei file non può essere inserito nei parametri URL.
- Impostare il valore di [`enctype`](/it/docs/Web/HTML/Reference/Elements/form#enctype) su `multipart/form-data`, poiché i dati verranno suddivisi in più parti: una per ogni file più una per i dati testuali inclusi nel corpo del modulo, se viene inserito anche testo nel modulo.
- Includere uno o più controlli [`<input type="file">`](/it/docs/Web/HTML/Reference/Elements/input/file) per consentire agli utenti di selezionare i file da caricare.

Ad esempio:

```html
<form
  method="post"
  action="https://example.com/upload"
  enctype="multipart/form-data">
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
> I server possono essere configurati con un limite di dimensione per i file e le richieste HTTP, al fine di prevenire abusi.

## Problemi di sicurezza

Ogni volta che si inviano dati a un server, è necessario considerare la sicurezza. I moduli HTML sono di gran lunga i vettori di attacco ai server più comuni, ovvero i punti in cui possono verificarsi attacchi. I problemi non derivano mai dai moduli HTML stessi: derivano da come il server gestisce i dati.

L'articolo [Sicurezza dei siti web](/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Website_security) del nostro argomento di apprendimento sul [lato server](/it/docs/Learn_web_development/Extensions/Server-side) illustra in dettaglio diversi attacchi comuni e le possibili difese contro di essi. È consigliabile consultare tale articolo per farsi un'idea delle possibilità.

### Essere paranoici: non fidarsi mai degli utenti

Come si possono quindi contrastare queste minacce? Si tratta di un argomento che va ben oltre questa guida, ma ci sono alcune regole da tenere a mente. La regola più importante è: non fidarsi mai, in nessun caso, degli utenti, incluso se stessi; anche un utente affidabile potrebbe essere stato compromesso.

Tutti i dati che arrivano al server devono essere controllati e sanitizzati. Sempre. Senza eccezioni.

- **Effettuare l'escape dei caratteri potenzialmente pericolosi**. I caratteri specifici a cui prestare attenzione variano in base al contesto in cui vengono utilizzati i dati e alla piattaforma server adottata, ma tutti i linguaggi lato server dispongono di funzioni per questo scopo. Occorre fare attenzione alle sequenze di caratteri che sembrano codice eseguibile, come comandi [JavaScript](/it/docs/Learn_web_development/Core/Scripting) o [SQL](https://en.wikipedia.org/wiki/SQL).
- **Limitare la quantità di dati in ingresso per consentire solo ciò che è necessario**.
- **Isolare in una sandbox i file caricati**. Memorizzarli su un server diverso e consentire l'accesso al file solo tramite un sottodominio differente o, meglio ancora, tramite un dominio completamente diverso.

Seguendo queste tre regole dovrebbe essere possibile evitare molti o la maggior parte dei problemi, ma è sempre una buona idea richiedere una revisione della sicurezza da parte di terzi competenti. Non presumere di aver identificato tutti i possibili problemi.

## Riepilogo

Come anticipato in precedenza, inviare i dati dei moduli è semplice, ma proteggere un'applicazione può essere difficile. È importante ricordare che non è uno sviluppatore front-end a dover definire il modello di sicurezza dei dati. È possibile eseguire la [convalida dei moduli lato client](/it/docs/Learn_web_development/Extensions/Forms/Form_validation), ma il server non può fidarsi di questa convalida perché non ha modo di sapere davvero cosa è accaduto lato client.

Seguendo questi tutorial in ordine, a questo punto si sa come creare il markup e applicare lo stile a un modulo, eseguire la convalida lato client e si ha un'idea dell'invio di un modulo.

## Vedere anche

Per approfondire la protezione di un'applicazione web, è possibile consultare queste risorse:

- [Primi passi nella programmazione di siti web lato server](/it/docs/Learn_web_development/Extensions/Server-side/First_steps)
- [The Open Web Application Security Project (OWASP)](https://owasp.org/)
- [Sicurezza web di Mozilla](https://infosec.mozilla.org/guidelines/web_security)

{{PreviousMenu("Learn_web_development/Extensions/Forms/Form_validation", "Learn_web_development/Extensions/Forms")}}
