---
title: "Configurazione di Apache: .htaccess"
slug: Learn_web_development/Extensions/Server-side/Apache_Configuration_htaccess
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

I file .htaccess di Apache consentono agli utenti di configurare le directory del server web che controllano senza modificare il file di configurazione principale.

Sebbene questo sia utile, è importante notare che l'utilizzo dei file `.htaccess` rallenta Apache, quindi, se si ha accesso al file di configurazione principale del server (che solitamente si chiama `httpd.conf`), è consigliabile aggiungere questa logica lì sotto un blocco `Directory`.

Vedere [.htaccess](https://httpd.apache.org/docs/current/howto/htaccess.html) nella documentazione del sito Apache HTTPD per ulteriori dettagli su cosa possono fare i file .htaccess.

Il resto di questo documento discuterà delle diverse opzioni di configurazione che si possono aggiungere a `.htaccess` e cosa fanno.

La maggior parte dei seguenti blocchi utilizza la direttiva [IfModule](https://httpd.apache.org/docs/2.4/mod/core.html#ifmodule) per eseguire le istruzioni all'interno del blocco solo se il modulo corrispondente è stato configurato correttamente e il server lo ha caricato. In questo modo si previene il crash del server nel caso in cui il modulo non sia stato caricato.

## Redirect

Ci sono momenti in cui è necessario comunicare agli utenti che una risorsa è stata spostata, sia temporaneamente che permanentemente. Questo è il motivo per cui si utilizzano `Redirect` e `RedirectMatch`.

```apacheconf
<IfModule mod_alias.c>
  # Redirect to a URL on a different host
  Redirect "/service" "http://foo2.example.com/service"

  # Redirect to a URL on the same host
  Redirect "/one" "/two"

  # Equivalent redirect to URL on the same host
  Redirect temp "/one" "/two"

  # Permanent redirect to a URL on the same host
  Redirect permanent "/three" "/four"

  # Redirect to an external URL
  # Using regular expressions and RedirectMatch
  RedirectMatch "^/oldfile\.html/?$" "http://example.com/newfile.php"
</IfModule>
```

I valori possibili per il primo parametro sono elencati di seguito. Se il primo parametro non è incluso, il valore predefinito è `temp`.

- permanent
  - : Restituisce uno stato di redirect permanente (301) che indica che la risorsa è stata spostata permanentemente.
- temp
  - : Restituisce uno stato di redirect temporaneo (302). **Questo è il valore predefinito**.
- seeother
  - : Restituisce uno stato "See Other" (303) che indica che la risorsa è stata sostituita.
- gone
  - : Restituisce uno stato "Gone" (410) che indica che la risorsa è stata rimossa permanentemente. Quando si utilizza questo stato, l'argomento _URL_ dovrebbe essere omesso.

## Risorse cross-origin

Il primo insieme di direttive controlla l'accesso [CORS](https://fetch.spec.whatwg.org/) (Cross-Origin Resource Sharing) alle risorse dal server. CORS è un meccanismo basato su intestazioni HTTP che permette a un server di indicare le origini esterne (dominio, protocollo o porta) che un browser deve autorizzare per il caricamento delle risorse.

Per motivi di sicurezza, i browser limitano le richieste HTTP cross-origin iniziate dagli script. Ad esempio, XMLHttpRequest e l'API Fetch seguono la stessa politica d'origine. Un'applicazione web che utilizza queste API può richiedere risorse solo dalla stessa origine da cui è stata caricata, a meno che la risposta da altre origini includa le intestazioni CORS appropriate.

### Accesso generale ai CORS

Questa direttiva aggiungerà l'intestazione CORS per tutte le risorse nella directory da qualsiasi sito web.

```apacheconf
<IfModule mod_headers.c>
  Header set Access-Control-Allow-Origin "*"
</IfModule>
```

A meno che non si sovrascriva la direttiva più avanti nella configurazione o nella configurazione di una directory al di sotto di quella in cui si imposta questa, ogni richiesta da server esterni sarà rispettata, il che è poco probabile che sia ciò che si desidera.

Un'alternativa è dichiarare esplicitamente quali domini hanno accesso ai contenuti del sito. Nell'esempio seguente, ci limitiamo a consentire l'accesso a un sottodominio del nostro sito principale (example.com). Questo è più sicuro e, probabilmente, ciò che si intendeva fare.

```apacheconf
<IfModule mod_headers.c>
  Header set Access-Control-Allow-Origin "subdomain.example.com"
</IfModule>
```

### Immagini cross-origin

Come riportato nel [Blog di Chromium](https://blog.chromium.org/2011/07/using-cross-domain-images-in-webgl-and.html) e documentato in [Permettere l'uso cross-origin di immagini e canvas](/it/docs/Web/HTML/How_to/CORS_enabled_image), può portare ad attacchi di {{Glossary("Fingerprinting", "fingerprinting")}}.

Per mitigare la possibilità di questi attacchi, è necessario utilizzare l'attributo `crossorigin` nelle immagini richieste e il frammento di codice seguente nel proprio `.htaccess` per impostare l'intestazione CORS dal server.

```apacheconf
<IfModule mod_setenvif.c>
  <IfModule mod_headers.c>
    <FilesMatch "\.(bmp|cur|gif|ico|jpe?g|a?png|svgz?|webp|heic|heif|avif)$">
      SetEnvIf Origin ":" IS_CORS
      Header set Access-Control-Allow-Origin "*" env=*IS_CORS*
    </FilesMatch>
  </IfModule>
</IfModule>
```

La [Guida alla risoluzione dei problemi di Google Fonts](https://developers.google.com/fonts/docs/troubleshooting) di Google Chrome ci dice che, mentre Google Fonts può inviare l'intestazione CORS con ogni risposta, alcuni server proxy potrebbero rimuoverla prima che il browser possa utilizzarla per rendere il font.

```apacheconf
<IfModule mod_headers.c>
  <FilesMatch "\.(eot|otf|tt[cf]|woff2?)$">
    Header set Access-Control-Allow-Origin "*"
  </FilesMatch>
</IfModule>
```

### Temporizzazione delle risorse cross-origin

La specifica [Resource Timing Level 1](https://www.w3.org/TR/resource-timing/) definisce un'interfaccia per le applicazioni web per accedere alle informazioni complete di temporizzazione per le risorse in un documento.

L'intestazione di risposta [Timing-Allow-Origin](/it/docs/Web/HTTP/Reference/Headers/Timing-Allow-Origin) specifica le origini che sono autorizzate a vedere i valori degli attributi recuperati tramite le funzionalità dell'API di temporizzazione delle risorse, che altrimenti verrebbero riportate a zero a causa delle restrizioni cross-origin.

Se una risorsa non viene servita con una `Timing-Allow-Origin` o se l'intestazione non include l'origine dopo aver effettuato la richiesta, alcuni attributi dell'oggetto `PerformanceResourceTiming` verranno impostati a zero.

```apacheconf
<IfModule mod_headers.c>
  Header set Timing-Allow-Origin: "*"
</IfModule>
```

## Pagine/messaggi di errore personalizzati

Apache permette di fornire pagine di errore personalizzate per gli utenti a seconda del tipo di errore che ricevono.

Le pagine di errore sono presentate come URL. Questi URL possono iniziare con una barra (/) per i percorsi web locali (relativi al DocumentRoot) o essere un URL completo che il client può risolvere.

Consultare la documentazione della [Direttiva ErrorDocument](https://httpd.apache.org/docs/current/mod/core.html#errordocument) nel sito di documentazione HTTPD per ulteriori informazioni.

```apacheconf
ErrorDocument 500 /errors/500.html
ErrorDocument 404 /errors/400.html
ErrorDocument 401 https://example.com/subscription_info.html
ErrorDocument 403 "Sorry, can't allow you access today."
```

## Prevenzione degli errori

Questa impostazione influisce su come funzionano i `MultiViews` per la directory alla quale si applica la configurazione.

L'effetto di `MultiViews` è il seguente: se il server riceve una richiesta per /some/dir/foo, se /some/dir ha `MultiViews` abilitato, e /some/dir/foo non esiste, allora il server legge la directory cercando file chiamati foo.\*, e imita effettivamente una mappa di tipo che nomina tutti quei file, assegnando loro gli stessi tipi di media e codifiche di contenuto che avrebbe se il client avesse richiesto uno di essi per nome. Scelga quindi la migliore corrispondenza ai requisiti del client.

L'impostazione disabilita `MultiViews` per la directory a cui si applica questa configurazione e impedisce ad Apache di restituire un errore 404 come risultato di una riscrittura quando la directory con lo stesso nome non esiste.

```apacheconf
Options -MultiViews
```

## Tipi di media e codifiche di caratteri

Apache utilizza [mod_mime](https://httpd.apache.org/docs/current/mod/mod_mime.html#addtype) per assegnare metadati di contenuto al contenuto selezionato per una risposta HTTP mappando modelli nell'URI o nei nomi dei file ai valori dei metadati.

Ad esempio, le estensioni di file dei contenuti definiscono spesso il tipo di media Internet del contenuto, la lingua, il set di caratteri e la codifica del contenuto. Queste informazioni sono inviate nei messaggi HTTP contenenti quel contenuto e utilizzate nella negoziazione del contenuto quando si selezionano alternative, in modo che le preferenze dell'utente siano rispettate nella scelta di uno dei contenuti possibili da servire.

**La modifica dei metadati per un file non modifica il valore dell'intestazione Last-Modified. Pertanto, le copie precedentemente memorizzate nella cache possono ancora essere usate da un client o proxy, con le intestazioni precedenti. Se si modificano i metadati (lingua, tipo di contenuto, set di caratteri o codifica), potrebbe essere necessario 'toccare' i file interessati (aggiornando la loro data di ultima modifica) per garantire che tutti i visitatori ricevano le intestazioni di contenuto corrette.**

### Servire risorse con i tipi di media appropriati (alias tipi MIME)

Associa tipi di media a una o più estensioni per assicurarsi che le risorse siano servite correttamente.

I server dovrebbero utilizzare `text/javascript` per le risorse JavaScript come indicato nella [specifica HTML](https://html.spec.whatwg.org/multipage/scripting.html#scriptingLanguages).

```apacheconf
<IfModule mod_mime.c>
  # Data interchange
    AddType application/atom+xml      atom
    AddType application/json          json map topojson
    AddType application/ld+json       jsonld
    AddType application/rss+xml       rss
    AddType application/geo+json      geojson
    AddType application/rdf+xml       rdf
    AddType application/xml           xml
  # JavaScript
    AddType text/javascript           js mjs
  # Manifest files
    AddType application/manifest+json     webmanifest
    AddType application/x-web-app-manifest+json         webapp
    AddType text/cache-manifest           appcache
  # Media files
    AddType audio/mp4                     f4a f4b m4a
    AddType audio/ogg                     oga ogg opus
    AddType image/bmp                     bmp
    AddType image/svg+xml                 svg svgz
    AddType image/webp                    webp
    AddType video/mp4                     f4v f4p m4v mp4
    AddType video/ogg                     ogv
    AddType video/webm                    webm
    AddType image/x-icon    cur ico
  # HEIF Images
    AddType image/heic                    heic
    AddType image/heif                    heif
  # HEIF Image Sequence
    AddType image/heics                   heics
    AddType image/heifs                   heifs
  # AVIF Images
    AddType image/avif                    avif
  # AVIF Image Sequence
    AddType image/avis                    avis
  # WebAssembly
    AddType application/wasm              wasm
  # Web fonts
    AddType font/woff                         woff
    AddType font/woff2                        woff2
    AddType application/vnd.ms-fontobject                eot
    AddType font/ttf                          ttf
    AddType font/collection                   ttc
    AddType font/otf                          otf
  # Other
    AddType application/octet-stream          safariextz
    AddType application/x-bb-appworld         bbaw
    AddType application/x-chrome-extension    crx
    AddType application/x-opera-extension     oex
    AddType application/x-xpinstall           xpi
    AddType text/calendar                     ics
    AddType text/markdown                     markdown md
    AddType text/vcard                        vcard vcf
    AddType text/vnd.rim.location.xloc        xloc
    AddType text/vtt                          vtt
    AddType text/x-component                  htc
</IfModule>
```

## Impostare l'attributo charset predefinito

Ogni contenuto sul web ha un set di caratteri. La maggior parte, se non tutto, il contenuto è in Unicode UTF-8.

Usa [AddDefaultCharset](https://httpd.apache.org/docs/current/mod/core.html#adddefaultcharset) per servire tutte le risorse etichettate come `text/html` o `text/plain` con il charset `UTF-8`.

```apacheconf
<IfModule mod_mime.c>
  AddDefaultCharset utf-8
</IfModule>
```

## Impostare il charset per tipi specifici di media

Servire i seguenti tipi di file con il parametro `charset` impostato su `UTF-8` utilizzando la direttiva [AddCharset](https://httpd.apache.org/docs/current/mod/mod_mime.html#addcharset) disponibile in `mod_mime`.

```apacheconf
<IfModule mod_mime.c>
  AddCharset utf-8 .appcache \
    .bbaw \
    .css \
    .htc \
    .ics \
    .js \
    .json \
    .manifest \
    .map \
    .markdown \
    .md \
    .mjs \
    .topojson \
    .vtt \
    .vcard \
    .vcf \
    .webmanifest \
    .xloc
</IfModule>
```

## Direttive `Mod_rewrite` e `RewriteEngine`

[mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html) offre un modo per modificare dinamicamente le richieste URL in arrivo basandosi su regole di espressione regolare. Questo permette di mappare URL arbitrari sulla struttura URL interna a piacimento.

Supporta un numero illimitato di regole e condizioni di regola allegate per ciascuna regola, fornendo un meccanismo di manipolazione degli URL veramente flessibile e potente. Le manipolazioni dell'URL possono dipendere da vari test: variabili del server, variabili dell'ambiente, intestazioni HTTP, timestamp, ricerche di database esterni e vari altri programmi o gestori esterni, possono essere utilizzati per ottenere una corrispondenza degli URL granulare.

### Abilitare `mod_rewrite`

Il modello di base per abilitare `mod_rewrite` è un prerequisito per tutte le altre attività che lo utilizzano.

I passaggi richiesti sono:

1. Attivare il motore di riscrittura (questo è necessario affinché le direttive `RewriteRule` funzionino) come documentato nella documentazione di [RewriteEngine](https://httpd.apache.org/docs/current/mod/mod_rewrite.html#RewriteEngine).
2. Abilitare l'opzione `FollowSymLinks` se non è già abilitata. Vedere la documentazione delle [Opzioni fondamentali](https://httpd.apache.org/docs/current/mod/core.html#options).
3. Se il provider web non consente l'opzione `FollowSymlinks`, è necessario commentarla o rimuoverla e quindi annunciare la riga `Options +SymLinksIfOwnerMatch`, ma fare attenzione all'[impatto sulle prestazioni](https://httpd.apache.org/docs/current/misc/perf-tuning.html#symlinks).

   - Alcuni servizi di hosting cloud richiederanno di impostare `RewriteBase`.
   - Vedere [Rackspace FAQ](https://web.archive.org/web/20151223141222/http://www.rackspace.com/knowledge_center/frequently-asked-question/why-is-modrewrite-not-working-on-my-site) e la [documentazione HTTPD](https://httpd.apache.org/docs/current/mod/mod_rewrite.html#rewritebase).
   - A seconda di come è configurato il server, potrebbe essere necessario utilizzare la direttiva [`RewriteOptions`](https://httpd.apache.org/docs/current/mod/mod_rewrite.html#rewriteoptions) per abilitare alcune opzioni per il motore di riscrittura.

```apacheconf
<IfModule mod_rewrite.c>
  RewriteEngine On
  Options +FollowSymlinks
  # Options +SymLinksIfOwnerMatch
  # RewriteBase /
  # RewriteOptions <options>
</IfModule>
```

### Forzare HTTPS

Queste regole di riscrittura reindirizzeranno dalla versione insicura `http://` alla versione sicura `https://` dell'URL come descritto nel [wiki Apache HTTPD](https://cwiki.apache.org/confluence/display/httpd/RewriteHTTPToHTTPS).

```apacheconf
<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteCond %{HTTPS} !=on
  RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R,L]
</IfModule>
```

Se si utilizza cPanel AutoSSL o il metodo Let's Encrypt webroot per creare i certificati TLS, fallirà nel convalidare il certificato se le richieste di validazione sono reindirizzate a HTTPS. Attivare la/le condizione/i necessaria/e.

```apacheconf
<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteCond %{HTTPS} !=on
  RewriteCond %{REQUEST_URI} !^/\.well-known/acme-challenge/
  RewriteCond %{REQUEST_URI} !^/\.well-known/cpanel-dcv/[\w-]+$
  RewriteCond %{REQUEST_URI} !^/\.well-known/pki-validation/[A-F0-9]{32}\.txt(?:\ Comodo\ DCV)?$
  RewriteRule ^ https://%{HTTP_HOST}%{REQUEST_URI} [R=301,L]
</IfModule>
```

### Reindirizzare da URL con `www.`

Queste direttive riscriveranno `www.example.com` in `example.com`.

Non si dovrebbe duplicare il contenuto in più origini (con e senza www). Ciò può causare problemi SEO (contenuto duplicato), pertanto si dovrebbe scegliere una delle alternative e reindirizzare l'altra. Si dovrebbe anche usare [URL canonici](https://www.semrush.com/blog/canonical-url-guide/) per indicare quale URL i motori di ricerca dovrebbero rastrellare (se supportano questa funzionalità).

Impostare la variabile `%{ENV:PROTO}`, per consentire automaticamente le riscritture per il reindirizzamento con lo schema appropriato (`http` o `https`).

La regola assume per impostazione predefinita che siano disponibili ambienti HTTP e HTTPS per il reindirizzamento.

```apacheconf
<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteCond %{HTTPS} =on
  RewriteRule ^ - [E=PROTO:https]
  RewriteCond %{HTTPS} !=on
  RewriteRule ^ - [E=PROTO:http]

  RewriteCond %{HTTP_HOST} ^www\.(.+)$ [NC]
  RewriteRule ^ %{ENV:PROTO}://%1%{REQUEST_URI} [R=301,L]
</IfModule>
```

### Inserire il `www.` all'inizio degli URL

Queste regole inseriranno `www.` all'inizio di un URL. È importante notare che non si dovrebbe mai rendere disponibile lo stesso contenuto tramite due URL diversi.

Ciò può causare problemi SEO (contenuto duplicato), pertanto si dovrebbe scegliere una delle alternative e reindirizzare l'altra. Per i motori di ricerca che le supportano, si dovrebbe usare [URL canonici](https://www.semrush.com/blog/canonical-url-guide/) per indicare quale URL i motori di ricerca dovrebbero rastrellare.

Impostare la variabile `%{ENV:PROTO}`, per consentire automaticamente le riscritture per il reindirizzamento con lo schema appropriato (`http` o `https`).

La regola assume per impostazione predefinita che siano disponibili ambienti HTTP e HTTPS per il reindirizzamento. Se il certificato TLS non può gestire uno dei domini utilizzati durante il reindirizzamento, si dovrebbe attivare la condizione.

Quanto segue potrebbe non essere una buona idea se si utilizzano sottodomini "reali" per alcune parti del sito web.

```apacheconf
<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteCond %{HTTPS} =on
  RewriteRule ^ - [E=PROTO:https]
  RewriteCond %{HTTPS} !=on
  RewriteRule ^ - [E=PROTO:http]

  RewriteCond %{HTTPS} !=on

  RewriteCond %{HTTP_HOST} !^www\. [NC]
  RewriteCond %{SERVER_ADDR} !=127.0.0.1
  RewriteCond %{SERVER_ADDR} !=::1
  RewriteRule ^ %{ENV:PROTO}://www.%{HTTP_HOST}%{REQUEST_URI} [R=301,L]
</IfModule>
```

## Opzioni dei frame

L'esempio seguente invia l'intestazione di risposta `X-Frame-Options` con DENY come valore, informando i browser di non visualizzare il contenuto della pagina web in nessun frame per proteggere il sito web contro il [clickjacking](/it/docs/Web/Security/Attacks/Clickjacking).

Questo potrebbe non essere l'impostazione migliore per tutti. Si dovrebbe leggere su [gli altri due valori possibili per l'intestazione `X-Frame-Options`](https://datatracker.ietf.org/doc/html/rfc7034#section-2.1): `SAMEORIGIN` e `ALLOW-FROM`.

Anche se si potrebbe inviare l'intestazione `X-Frame-Options` per tutte le pagine del sito web, questo ha il potenziale svantaggio che proibisce anche qualsiasi frammentazione del contenuto (es.: quando gli utenti visitano il sito web utilizzando una pagina di risultati di ricerca per immagini di Google).

Tuttavia, si dovrebbe garantire che l'intestazione `X-Frame-Options` sia inviata per tutte le pagine che consentono a un utente di effettuare un'operazione che cambia lo stato (ad es., pagine che contengono collegamenti di acquisto con un clic, pagine di pagamento, di conferma del trasferimento bancario, pagine che effettuano modifiche permanenti alla configurazione, ecc.).

```apacheconf
<IfModule mod_headers.c>
  Header always set X-Frame-Options "DENY" "expr=%{CONTENT_TYPE} =~ m#text/html#i"
</IfModule>
```

## Content Security Policy (CSP)

[CSP (Content Security Policy)](https://content-security-policy.com/) mitiga il rischio di cross-site scripting e altri attacchi di iniezione di contenuti impostando una `Content Security Policy` che consente solo fonti di contenuti fidate per il sito web.

Non esiste una politica adatta a tutti i siti web, l'esempio seguente è inteso come linee guida da modificare per il proprio sito.

Per rendere più semplice l'implementazione di CSP, si può utilizzare un [generatore di intestazioni CSP online](https://report-uri.com/home/generate/). Si dovrebbe anche utilizzare un [validatore](https://csp-evaluator.withgoogle.com/) per assicurarsi che l'intestazione faccia ciò che si desidera.

```apacheconf
<IfModule mod_headers.c>
  Content-Security-Policy "default-src 'self'; base-uri 'none'; form-action 'self'; frame-ancestors 'none'; upgrade-insecure-requests" "expr=%{CONTENT_TYPE} =~ m#text\/(html|javascript)|application\/pdf|xml#i"
</IfModule>
```

## Accesso alle directory

Questa direttiva impedirà l'accesso alle directory che non hanno un file index presente nel formato che il server è configurato per utilizzare, come `index.html` o `index.php`.

```apacheconf
<IfModule mod_autoindex.c>
    Options -Indexes
</IfModule>
```

## Bloccare l'accesso a file e directory nascosti

Nei sistemi Macintosh e Linux, i file che iniziano con un punto sono nascosti alla vista ma non all'accesso se si conosce il loro nome e posizione. Questi tipi di file solitamente contengono preferenze utente o lo stato conservato di un'utilità e possono includere posti piuttosto privati come, ad esempio, le directory `.git` o `.svn`.

La directory `.well-known/` rappresenta [lo standard (RFC 5785)](https://datatracker.ietf.org/doc/html/rfc5785) per i "luoghi noti" (es.: `/.well-known/manifest.json`, `/.well-known/keybase.txt`) e, quindi, l'accesso al suo contenuto visibile non dovrebbe essere bloccato.

```apacheconf
<IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteCond %{REQUEST_URI} "!(^|/)\.well-known/([^./]+./?)+$" [NC]
    RewriteCond %{SCRIPT_FILENAME} -d [OR]
    RewriteCond %{SCRIPT_FILENAME} -f
    RewriteRule "(^|/)\." - [F]
</IfModule>
```

## Bloccare l'accesso ai file con informazioni sensibili

Bloccare l'accesso ai file di backup e sorgenti che possono essere lasciati da alcuni editor di testo e possono rappresentare un rischio per la sicurezza quando chiunque può accedervi.

Aggiornare l'espressione regolare `<FilesMatch>` nel seguente esempio per includere tutti i file che potrebbero finire sul server di produzione e possono esporre informazioni sensibili sul sito web. Questi file possono includere file di configurazione o file che contengono metadati sul progetto, tra gli altri.

```apacheconf
<IfModule mod_authz_core.c>
  <FilesMatch "(^#.*#|\.(bak|conf|dist|fla|in[ci]|log|orig|psd|sh|sql|sw[op])|~)$">
    Require all denied
  </FilesMatch>
</IfModule>
```

## HTTP Strict Transport Security (HSTS)

Se un utente digita `example.com` nel proprio browser, anche se il server lo reindirizza alla versione sicura del sito web, rimane ancora una finestra di opportunità (la connessione HTTP iniziale) per un attaccante di eseguire un degrado o reindirizzare la richiesta.

L'intestazione seguente garantisce che un browser si connetta solo al server tramite HTTPS, indipendentemente da ciò che gli utenti digitano nella barra degli indirizzi del browser.

Essere consapevoli del fatto che HTTP Strict Transport Security non è revocabile e bisogna garantire di poter servire il sito su HTTPS per tutto il tempo specificato nella direttiva `max-age`. Se non si dispone più di una connessione TLS valida (es.: a causa di un certificato TLS scaduto), i visitatori vedranno un messaggio di errore anche cercando di connettersi su HTTP.

```apacheconf
<IfModule mod_headers.c>
  # Header always set
  Strict-Transport-Security "max-age=16070400; includeSubDomains" "expr=%{HTTPS} == 'on'"
  # (1) Enable your site for HSTS preload inclusion.
  # Header always set
  Strict-Transport-Security "max-age=31536000; includeSubDomains; preload" "expr=%{HTTPS} == 'on'"
</IfModule>
```

## Prevenire alcuni browser dal MIME-sniffing della risposta

1. Restringe tutti i fetch per impostazione predefinita all'origine del sito web corrente impostando la direttiva `default-src` su `'self'` - che funge da fallback per tutte le {{Glossary("Fetch_directive", "direttive Fetch")}}.

   - Questo è conveniente poiché non è necessario specificare tutte le direttive Fetch che si applicano al sito, ad esempio: `connect-src 'self'; font-src 'self'; script-src 'self'; style-src 'self'`, ecc.
   - Questa restrizione significa anche che bisogna definire esplicitamente da quali siti il tuo sito web è autorizzato a caricare risorse. Altrimenti, sarà limitato alla stessa origine della pagina della richiesta

2. Non consente l'elemento `<base>` sul sito web. Questo è per impedire agli attaccanti di modificare le posizioni delle risorse caricate da URL relativi

   - Se si desidera utilizzare l'elemento `<base>`, allora usare `base-uri 'self'` invece

3. Permette solo invii di moduli dall'origine corrente con: `form-action 'self'`
4. Impedisce a tutti i siti web (compreso il proprio) di incorporare le pagine web entro, ad esempio, l'elemento `<iframe>` o `<object>` impostando: `frame-ancestors 'none'`.

   - La direttiva `frame-ancestors` aiuta a evitare attacchi di [clickjacking](/it/docs/Web/Security/Attacks/Clickjacking) ed è simile all'intestazione `X-Frame-Options`
   - I browser che supportano l'intestazione CSP ignoreranno `X-Frame-Options` se `frame-ancestors` è anche specificato

5. Forza il browser a trattare tutte le risorse che sono servite su HTTP, come se fossero caricate in modo sicuro su HTTPS impostando la direttiva `upgrade-insecure-requests`

   - **`upgrade-insecure-requests` non garantisce HTTPS per la navigazione di livello superiore. Se si vuole forzare il sito web stesso a essere caricato su HTTPS bisogna includere l'intestazione `Strict-Transport-Security`**

6. Include l'intestazione `Content-Security-Policy` in tutte le risposte che sono in grado di eseguire script. Questo include i tipi di file comunemente usati: documenti HTML, XML e PDF. Anche se i file JavaScript non possono eseguire script in un "contesto di navigazione", sono inclusi per indirizzare i [worker web](/it/docs/Web/HTTP/Reference/Headers/Content-Security-Policy#csp_in_workers).

Alcuni vecchi browser tenterebbero di indovinare il tipo di contenuto di una risorsa, anche quando non è correttamente configurato nella configurazione del server. Ciò riduce l'esposizione a attacchi di download drive-by e perdite di dati cross-origin.

```apacheconf
<IfModule mod_headers.c>
    Header always set X-Content-Type-Options "nosniff"
</IfModule>
```

## Politica Referer

Includiamo l'intestazione `Referrer-Policy` nelle risposte per le risorse che sono in grado di richiedere (o navigare verso) altre risorse.

Questo include i tipi di risorse comunemente usati: HTML, CSS, XML/SVG, documenti PDF, script e worker.

Per evitare completamente la perdita di referer, specificare il valore `no-referrer`. È importante notare che l'effetto potrebbe impattare negativamente gli strumenti di analisi.

Usare servizi come quelli sotto per controllare la tua `Referrer-Policy`:

- [Osservatorio HTTP](/en-US/observatory)
- [securityheaders.com](https://securityheaders.com/)

```apacheconf
<IfModule mod_headers.c>
  Header always set Referrer-Policy "strict-origin-when-cross-origin" "expr=%{CONTENT_TYPE} =~ m#text\/(css|html|javascript)|application\/pdf|xml#i"
</IfModule>
```

## Disabilitare il metodo HTTP `TRACE`

Il metodo [TRACE](/it/docs/Web/HTTP/Reference/Methods/TRACE), pur sembrando innocuo, può essere sfruttato con successo in alcuni scenari per rubare le credenziali degli utenti legittimi. Vedere [Un attacco di Tracing Cross-Site (XST)](https://owasp.org/www-community/attacks/Cross_Site_Tracing) e [Guida al test della sicurezza web di OWASP](https://owasp.org/www-project-web-security-testing-guide/v41/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/06-Test_HTTP_Methods#test-xst-potential).

I browser moderni ora impediscono le richieste TRACE effettuate tramite JavaScript, tuttavia, sono stati scoperti altri modi per inviare richieste TRACE con i browser, come usare Java.

Se si ha accesso al file di configurazione principale del server, utilizzare invece la direttiva [`TraceEnable`](https://httpd.apache.org/docs/current/mod/core.html#traceenable).

```apacheconf
<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteCond %{REQUEST_METHOD} ^TRACE [NC]
  RewriteRule .* - [R=405,L]
</IfModule>
```

## Rimuovere l'intestazione di risposta `X-Powered-By`

Alcuni framework come PHP e ASP.NET impostano un'intestazione `X-Powered-By` che contiene informazioni su di essi (es.: loro nome, numero di versione).

Questa intestazione non fornisce alcun valore e, in alcuni casi, le informazioni che fornisce possono esporre vulnerabilità.

```apacheconf
<IfModule mod_headers.c>
  Header unset X-Powered-By
  Header always unset X-Powered-By
</IfModule>
```

Se possibile, si dovrebbe disabilitare l'intestazione `X-Powered-By` a livello di linguaggio/framework (es.: per PHP, lo si può fare impostando quanto segue in `php.ini`).

```ini
expose_php = off;
```

## Rimuovere il piè di pagina informativo generato dal server Apache

Impedire ad Apache di aggiungere una riga piè di pagina contenente informazioni sul server ai documenti generati dal server (es.: messaggi di errore, elenchi di directory, ecc.). Vedere la documentazione della direttiva [`ServerSignature`](https://httpd.apache.org/docs/current/mod/core.html#serversignature) per maggiori informazioni su cosa fornisce la firma del server e la direttiva [`ServerTokens`](https://httpd.apache.org/docs/current/mod/core.html#servertokens) per informazioni su come configurare le informazioni fornite nella firma.

```apacheconf
ServerSignature Off
```

## Correggere le intestazioni `AcceptEncoding` interrotte

Alcuni proxy e software di sicurezza manomettono o rimuovono l'intestazione HTTP `Accept-Encoding`. Vedere [Spingersi oltre la compressione Gzip](https://calendar.perfplanet.com/2010/pushing-beyond-gzipping/) per una spiegazione più dettagliata.

```apacheconf
<IfModule mod_deflate.c>
  <IfModule mod_setenvif.c>
    <IfModule mod_headers.c>
      SetEnvIfNoCase ^(Accept-EncodXng|X-cept-Encoding|X{15}|~{15}|-{15})$ ^((gzip|deflate)\s*,?\s*)+|[X~-]{4,13}$ HAVE_Accept-Encoding
      RequestHeader append Accept-Encoding "gzip,deflate" env=HAVE_Accept-Encoding
    </IfModule>
  </IfModule>
</IfModule>
```

## Comprimere i tipi di media

Comprimere tutto l'output etichettato con uno dei seguenti tipi di media utilizzando la direttiva [AddOutputFilterByType](https://httpd.apache.org/docs/current/mod/mod_filter.html#addoutputfilterbytype).

```apacheconf
<IfModule mod_deflate.c>
  <IfModule mod_filter.c>
    AddOutputFilterByType DEFLATE "application/atom+xml" \
      "application/javascript" \
      "application/json" \
      "application/ld+json" \
      "application/manifest+json" \
      "application/rdf+xml" \
      "application/rss+xml" \
      "application/schema+json" \
      "application/geo+json" \
      "application/vnd.ms-fontobject" \
      "application/wasm" \
      "application/x-font-ttf" \
      "application/x-javascript" \
      "application/x-web-app-manifest+json" \
      "application/xhtml+xml" \
      "application/xml" \
      "font/eot" \
      "font/opentype" \
      "font/otf" \
      "font/ttf" \
      "image/bmp" \
      "image/svg+xml" \
      "image/vnd.microsoft.icon" \
      "text/cache-manifest" \
      "text/calendar" \
      "text/css" \
      "text/html" \
      "text/javascript" \
      "text/plain" \
      "text/markdown" \
      "text/vcard" \
      "text/vnd.rim.location.xloc" \
      "text/vtt" \
      "text/x-component" \
      "text/x-cross-domain-policy" \
      "text/xml"
  </IfModule>
</IfModule>
```

## Mappare le estensioni ai tipi di media

Mappare le seguenti estensioni di nome file al tipo di codifica specificato utilizzando [AddEncoding](https://httpd.apache.org/docs/current/mod/mod_mime.html#addencoding) affinché Apache possa servire i tipi di file con l'intestazione di risposta `Content-Encoding` appropriata (questo NON farà sì che Apache li comprima!). Se questi tipi di file fossero serviti senza un'intestazione di risposta `Content-Encoding` appropriata, le applicazioni client (es.: i browser) non saprebbero che devono prima decomprimere la risposta, e quindi, non sarebbero in grado di comprendere il contenuto.

```apacheconf
<IfModule mod_deflate.c>
  <IfModule mod_mime.c>
    AddEncoding gzip svgz
  </IfModule>
</IfModule>
```

## Scadenza della cache

Servire le risorse con una data di scadenza nel lontano futuro utilizzando il modulo [mod_expires](https://httpd.apache.org/docs/current/mod/mod_expires.html), e le intestazioni [Cache-Control](/it/docs/Web/HTTP/Reference/Headers/Cache-Control) e [Expires](/it/docs/Web/HTTP/Reference/Headers/Expires).

```apacheconf
<IfModule mod_expires.c>
    ExpiresActive on
    ExpiresDefault                                      "access plus 1 month"

  # CSS
    ExpiresByType text/css                              "access plus 1 year"
  # Data interchange
    ExpiresByType application/atom+xml                  "access plus 1 hour"
    ExpiresByType application/rdf+xml                   "access plus 1 hour"
    ExpiresByType application/rss+xml                   "access plus 1 hour"
    ExpiresByType application/json                      "access plus 0 seconds"
    ExpiresByType application/ld+json                   "access plus 0 seconds"
    ExpiresByType application/schema+json               "access plus 0 seconds"
    ExpiresByType application/geo+json                  "access plus 0 seconds"
    ExpiresByType application/xml                       "access plus 0 seconds"
    ExpiresByType text/calendar                         "access plus 0 seconds"
    ExpiresByType text/xml                              "access plus 0 seconds"
  # Favicon (cannot be renamed!) and cursor images
    ExpiresByType image/vnd.microsoft.icon              "access plus 1 week"
    ExpiresByType image/x-icon                          "access plus 1 week"
  # HTML
    ExpiresByType text/html                             "access plus 0 seconds"
  # JavaScript
    ExpiresByType text/javascript                       "access plus 1 year"
  # Manifest files
    ExpiresByType application/manifest+json             "access plus 1 week"
    ExpiresByType application/x-web-app-manifest+json   "access plus 0 seconds"
    ExpiresByType text/cache-manifest                   "access plus 0 seconds"
  # Markdown
    ExpiresByType text/markdown                         "access plus 0 seconds"
  # Media files
    ExpiresByType audio/ogg                             "access plus 1 month"
    ExpiresByType image/bmp                             "access plus 1 month"
    ExpiresByType image/gif                             "access plus 1 month"
    ExpiresByType image/jpeg                            "access plus 1 month"
    ExpiresByType image/svg+xml                         "access plus 1 month"
    ExpiresByType image/webp                            "access plus 1 month"
    # PNG and animated PNG
    ExpiresByType image/apng                            "access plus 1 month"
    ExpiresByType image/png                             "access plus 1 month"
    # HEIF Images
    ExpiresByType image/heic                            "access plus 1 month"
    ExpiresByType image/heif                            "access plus 1 month"
    # HEIF Image Sequence
    ExpiresByType image/heics                           "access plus 1 month"
    ExpiresByType image/heifs                           "access plus 1 month"
    # AVIF Images
    ExpiresByType image/avif                            "access plus 1 month"
    # AVIF Image Sequence
    ExpiresByType image/avis                            "access plus 1 month"
    ExpiresByType video/mp4                             "access plus 1 month"
    ExpiresByType video/ogg                             "access plus 1 month"
    ExpiresByType video/webm                            "access plus 1 month"
  # WebAssembly
    ExpiresByType application/wasm                      "access plus 1 year"
  # Web fonts
    # Collection
    ExpiresByType font/collection                       "access plus 1 month"
    # Embedded OpenType (EOT)
    ExpiresByType application/vnd.ms-fontobject         "access plus 1 month"
    ExpiresByType font/eot                              "access plus 1 month"
    # OpenType
    ExpiresByType font/opentype                         "access plus 1 month"
    ExpiresByType font/otf                              "access plus 1 month"
    # TrueType
    ExpiresByType application/x-font-ttf                "access plus 1 month"
    ExpiresByType font/ttf                              "access plus 1 month"
    # Web Open Font Format (WOFF) 1.0
    ExpiresByType application/font-woff                 "access plus 1 month"
    ExpiresByType application/x-font-woff               "access plus 1 month"
    ExpiresByType font/woff                             "access plus 1 month"
    # Web Open Font Format (WOFF) 2.0
    ExpiresByType application/font-woff2                "access plus 1 month"
    ExpiresByType font/woff2                            "access plus 1 month"
  # Other
    ExpiresByType text/x-cross-domain-policy            "access plus 1 week"
</IfModule>
```
