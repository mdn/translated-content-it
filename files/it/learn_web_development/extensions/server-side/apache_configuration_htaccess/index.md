---
title: "Configurazione Apache: .htaccess"
slug: Learn_web_development/Extensions/Server-side/Apache_Configuration_htaccess
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

I file .htaccess di Apache consentono agli utenti di configurare le directory del server web che controllano senza modificare il file di configurazione principale.

Sebbene questo sia utile, è importante notare che l'uso dei file `.htaccess` rallenta Apache, quindi, se si ha accesso al file di configurazione principale del server (che di solito si chiama `httpd.conf`), conviene aggiungere questa logica lì sotto un blocco `Directory`.

Consulta [.htaccess](https://httpd.apache.org/docs/current/howto/htaccess.html) nel sito della documentazione di Apache HTTPD per ulteriori dettagli su cosa possono fare i file .htaccess.

Il resto di questo documento discute le diverse opzioni di configurazione che puoi aggiungere a `.htaccess` e cosa fanno.

La maggior parte dei blocchi seguenti utilizza la direttiva [IfModule](https://httpd.apache.org/docs/2.4/mod/core.html#ifmodule) per eseguire le istruzioni all'interno del blocco solo se il modulo corrispondente è stato configurato correttamente e il server lo ha caricato. In questo modo si evita che il server si arresti in caso di mancato caricamento del modulo.

## Reindirizzamenti

Ci sono momenti in cui dobbiamo dire agli utenti che una risorsa è stata spostata, temporaneamente o permanentemente. Questo è ciò per cui usiamo `Redirect` e `RedirectMatch`.

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

I possibili valori per il primo parametro sono elencati di seguito. Se il primo parametro non è incluso, il valore predefinito è `temp`.

- permanent
  - : Restituisce uno stato di reindirizzamento permanente (301) che indica che la risorsa è stata spostata in modo permanente.
- temp
  - : Restituisce uno stato di reindirizzamento temporaneo (302). **Questo è il predefinito**.
- seeother
  - : Restituisce uno stato "See Other" (303) che indica che la risorsa è stata sostituita.
- gone
  - : Restituisce uno stato "Gone" (410) che indica che la risorsa è stata rimossa permanentemente. Quando viene utilizzato questo stato, l'argomento _URL_ deve essere omesso.

## Risorse cross-origin

Il primo set di direttive controlla l'accesso [CORS](https://fetch.spec.whatwg.org/) (Cross-Origin Resource Sharing) a risorse del server. CORS è un meccanismo basato su intestazioni HTTP che consente a un server di indicare le origini esterne (dominio, protocollo o porta) che un browser dovrebbe consentire per il caricamento delle risorse.

Per motivi di sicurezza, i browser bloccano le richieste HTTP cross-origin iniziate dagli script. Ad esempio, XMLHttpRequest e la Fetch API seguono la stessa politica di origine. Un'applicazione web che utilizza queste API può richiedere risorse solo dall'origine da cui è stata caricata, a meno che la risposta da altre origini non includa le intestazioni CORS appropriate.

### Accesso CORS generale

Questa direttiva aggiungerà l'intestazione CORS a tutte le risorse nella directory da qualsiasi sito web.

```apacheconf
<IfModule mod_headers.c>
  Header set Access-Control-Allow-Origin "*"
</IfModule>
```

A meno che tu non sovrascriva la direttiva più avanti nella configurazione o nella configurazione di una directory inferiore a dove l'hai impostata, ogni richiesta da server esterni sarà accettata, il che è improbabile sia quello che vuoi.

Un'alternativa è dichiarare esplicitamente quali domini hanno accesso al contenuto del tuo sito. Nell'esempio seguente, limitiamo l'accesso a un sottodominio del nostro sito principale (example.com). Questo è più sicuro e, probabilmente, quello che intendi fare.

```apacheconf
<IfModule mod_headers.c>
  Header set Access-Control-Allow-Origin "subdomain.example.com"
</IfModule>
```

### Immagini cross-origin

Come riportato nel [Blog di Chromium](https://blog.chromium.org/2011/07/using-cross-domain-images-in-webgl-and.html) e documentato in [Consentire l'uso cross-origin di immagini e canvas](/it/docs/Web/HTML/How_to/CORS_enabled_image) può portare ad attacchi di {{Glossary("Fingerprinting", "fingerprinting")}}.

Per mitigare la possibilità di questi attacchi, dovresti usare l'attributo `crossorigin` nelle immagini che richiedi e il frammento di codice seguente nel tuo `.htaccess` per impostare l'intestazione CORS dal server.

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

La [guida alla risoluzione dei problemi di Google Fonts](https://developers.google.com/fonts/docs/troubleshooting) di Google Chrome ci informa che, mentre i Google Fonts possono inviare l'intestazione CORS con ogni risposta, alcuni server proxy potrebbero rimuoverla prima che il browser possa usarla per visualizzare il font.

```apacheconf
<IfModule mod_headers.c>
  <FilesMatch "\.(eot|otf|tt[cf]|woff2?)$">
    Header set Access-Control-Allow-Origin "*"
  </FilesMatch>
</IfModule>
```

### Tempistica delle risorse cross-origin

La specifica [Resource Timing Level 1](https://www.w3.org/TR/resource-timing/) definisce un'interfaccia per le applicazioni web per accedere alle informazioni temporali complete per le risorse in un documento.

L'intestazione di risposta [Timing-Allow-Origin](/it/docs/Web/HTTP/Reference/Headers/Timing-Allow-Origin) specifica le origini autorizzate a vedere i valori degli attributi recuperati tramite le funzionalità dell'API Resource Timing, che altrimenti verrebbero riportate come zero a causa delle restrizioni cross-origin.

Se una risorsa non viene servita con un `Timing-Allow-Origin` o se l'intestazione non include l'origine dopo aver effettuato la richiesta, alcuni attributi dell'oggetto `PerformanceResourceTiming` saranno impostati su zero.

```apacheconf
<IfModule mod_headers.c>
  Header set Timing-Allow-Origin: "*"
</IfModule>
```

## Pagine/messaggi di errore personalizzati

Apache ti consente di fornire pagine di errore personalizzate per gli utenti a seconda del tipo di errore che ricevono.

Le pagine di errore vengono presentate come URL. Questi URL possono iniziare con una slash (/) per percorsi web locali (relativi a DocumentRoot) o essere un URL completo che il client può risolvere.

Consulta la documentazione su [ErrorDocument Directive](https://httpd.apache.org/docs/current/mod/core.html#errordocument) nel sito della documentazione HTTPD per maggiori informazioni.

```apacheconf
ErrorDocument 500 /errors/500.html
ErrorDocument 404 /errors/400.html
ErrorDocument 401 https://example.com/subscription_info.html
ErrorDocument 403 "Sorry, can't allow you access today."
```

## Prevenzione degli errori

Questa impostazione influisce su come funzionano i MultiViews per la directory a cui si applica la configurazione.

L'effetto di `MultiViews` è il seguente: se il server riceve una richiesta per /some/dir/foo, se /some/dir ha `MultiViews` attivato e /some/dir/foo non esiste, allora il server legge la directory cercando file chiamati foo.\*, e di fatto crea una mappa di tipo che nomina tutti quei file, assegnando loro gli stessi tipi di media e codifiche di contenuto che avrebbe se il cliente avesse richiesto uno di essi per nome. Poi sceglie la migliore corrispondenza per le esigenze del cliente.

L'impostazione disabilita `MultiViews` per la directory a cui si applica la configurazione e impedisce ad Apache di restituire un errore 404 come risultato della riscrittura quando la directory con lo stesso nome non esiste.

```apacheconf
Options -MultiViews
```

## Tipi di media e codifiche dei caratteri

Apache utilizza [mod_mime](https://httpd.apache.org/docs/current/mod/mod_mime.html#addtype) per assegnare i metadati del contenuto al contenuto selezionato per una risposta HTTP mappando le corrispondenze nell'URI o nei nomi dei file ai valori dei metadati.

Ad esempio, le estensioni dei nomi di file dei file di contenuto spesso definiscono il tipo di media Internet del contenuto, la lingua, il set di caratteri e la codifica del contenuto. Queste informazioni vengono inviate nei messaggi HTTP contenenti quel contenuto e utilizzate nella negoziazione del contenuto durante la selezione delle alternative, in modo che le preferenze dell'utente siano rispettate quando si sceglie uno dei diversi contenuti possibili da servire.

**La modifica dei metadati di un file non modifica il valore dell'intestazione Last-Modified. Pertanto, le copie memorizzate precedentemente nella cache possono ancora essere utilizzate da un client o proxy, con le intestazioni precedenti. Se si modifica i metadati (lingua, tipo di contenuto, set di caratteri o codifica), potrebbe essere necessario 'toccare' i file interessati (aggiornando la loro data di ultima modifica) per garantire che tutti i visitatori ricevano le intestazioni di contenuto corrette.**

### Servizio risorse con i tipi di media appropriati (anche noti come tipi MIME)

Associa i tipi di media a una o più estensioni per assicurarsi che le risorse vengano servite correttamente.

I server dovrebbero utilizzare `text/javascript` per le risorse JavaScript come indicato nella [specifica HTML](https://html.spec.whatwg.org/multipage/scripting.html#scriptingLanguages)

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

## Imposta l'attributo charset predefinito

Ogni contenuto sul web ha un set di caratteri. La maggior parte, se non tutto, il contenuto è in Unicode UTF-8.

Utilizza [AddDefaultCharset](https://httpd.apache.org/docs/current/mod/core.html#adddefaultcharset) per servire tutte le risorse etichettate come `text/html` o `text/plain` con il set di caratteri `UTF-8`.

```apacheconf
<IfModule mod_mime.c>
  AddDefaultCharset utf-8
</IfModule>
```

## Imposta il charset per tipi di media specifici

Servi i seguenti tipi di file con il parametro `charset` impostato su `UTF-8` utilizzando la direttiva [AddCharset](https://httpd.apache.org/docs/current/mod/mod_mime.html#addcharset) disponibile in `mod_mime`.

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

[mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html) fornisce un modo per modificare dinamicamente le richieste URL in ingresso basandosi su regole di espressione regolare. Ciò consente di mappare URL arbitrari sulla struttura interna degli URL in qualsiasi modo si desideri.

Supporta un numero illimitato di regole e un numero illimitato di condizioni di regola allegate per ciascuna regola, fornendo un meccanismo di manipolazione degli URL davvero flessibile e potente. Le manipolazioni degli URL possono dipendere da vari test: variabili del server, variabili di ambiente, intestazioni HTTP, timestamp, ricerche in database esterni e vari altri programmi o gestori esterni possono essere utilizzati per ottenere una corrispondenza degli URL dettagliata.

### Abilita `mod_rewrite`

Il modello base per abilitare `mod_rewrite` è un prerequisito per tutte le altre attività che lo utilizzano.

I passaggi richiesti sono:

1. Accendi il motore di riscrittura (ciò è necessario affinché le direttive `RewriteRule` funzionino) come documentato nella documentazione [RewriteEngine](https://httpd.apache.org/docs/current/mod/mod_rewrite.html#RewriteEngine)
2. Abilita l'opzione `FollowSymLinks` se non è già abilitata. Consulta la documentazione [Core Options](https://httpd.apache.org/docs/current/mod/core.html#options)
3. Se il tuo host web non consente l'opzione `FollowSymlinks`, devi commentarla o rimuoverla, e quindi decommentare la riga `Options +SymLinksIfOwnerMatch`, ma fai attenzione all'[impatto sulle prestazioni](https://httpd.apache.org/docs/current/misc/perf-tuning.html#symlinks)

   - Alcuni servizi di hosting cloud ti richiederanno di impostare `RewriteBase`
   - Leggi le [FAQ di Rackspace](https://web.archive.org/web/20151223141222/http://www.rackspace.com/knowledge_center/frequently-asked-question/why-is-modrewrite-not-working-on-my-site) e la[documentazione HTTPD](https://httpd.apache.org/docs/current/mod/mod_rewrite.html#rewritebase)
   - A seconda di come è configurato il server, potrebbe essere necessario utilizzare la direttiva [`RewriteOptions`](https://httpd.apache.org/docs/current/mod/mod_rewrite.html#rewriteoptions) per abilitare alcune opzioni per il motore di riscrittura

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

Queste regole di riscrittura reindirizzeranno dalla versione insicura `http://` alla versione sicura `https://` dell'URL come descritto nel [wiki di Apache HTTPD](https://cwiki.apache.org/confluence/display/httpd/RewriteHTTPToHTTPS).

```apacheconf
<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteCond %{HTTPS} !=on
  RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R,L]
</IfModule>
```

Se stai utilizzando cPanel AutoSSL o il metodo webroot di Let's Encrypt per creare i tuoi certificati TLS, non sarà possibile convalidare il certificato se le richieste di convalida vengono reindirizzate a HTTPS. Attiva la condizione/i che ti serve(ono).

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

### Reindirizzamento dagli URL con `www.`

Queste direttive riscriveranno `www.example.com` in `example.com`.

Non dovresti duplicare il contenuto in origini multiple (con e senza www). Questo può causare problemi SEO (contenuto duplicato), quindi dovresti scegliere una delle alternative e reindirizzare l'altra. Dovresti anche usare [URL Canonici](https://www.semrush.com/blog/canonical-url-guide/) per indicare quale URL i motori di ricerca dovrebbero esaminare (se supportano la funzione).

Imposta la variabile `%{ENV:PROTO}`, per consentire alle riscritture di reindirizzare automaticamente con lo schema appropriato (`http` o `https`).

La regola presume di default che entrambi gli ambienti HTTP e HTTPS siano disponibili per il reindirizzamento.

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

### Inserimento del prefisso `www.` agli URL

Queste regole inseriscono `www.` all'inizio di un URL. È importante notare che non dovresti mai rendere disponibile lo stesso contenuto sotto due diversi URL.

Questo può causare problemi SEO (contenuto duplicato), quindi dovresti scegliere una delle alternative e reindirizzare l'altra. Per i motori di ricerca che le supportano, dovresti usare [URL Canonici](https://www.semrush.com/blog/canonical-url-guide/) per indicare quale URL i motori di ricerca dovrebbero esaminare.

Imposta la variabile `%{ENV:PROTO}`, per consentire alle riscritture di reindirizzare automaticamente con lo schema appropriato (`http` o `https`).

La regola presume di default che entrambi gli ambienti HTTP e HTTPS siano disponibili per il reindirizzamento. Se il tuo certificato TLS non può gestire uno dei domini utilizzati durante il reindirizzamento, dovresti attivare la condizione.

Il seguente potrebbe non essere una buona idea se utilizzi sottodomini "reali" per alcune parti del tuo sito web.

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

## Opzioni frame

L'esempio seguente invia l'intestazione di risposta `X-Frame-Options` con il valore DENY, informando i browser di non visualizzare il contenuto della pagina web in nessun frame per proteggere il sito web da attacchi di [clickjacking](/it/docs/Web/Security/Attacks/Clickjacking).

Questo potrebbe non essere l'impostazione migliore per tutti. Dovresti leggere riguardo [agli altri due possibili valori per l'intestazione `X-Frame-Options`](https://datatracker.ietf.org/doc/html/rfc7034#section-2.1): `SAMEORIGIN` e `ALLOW-FROM`.

Sebbene potessi inviare l'intestazione `X-Frame-Options` per tutte le pagine del tuo sito web, ciò ha il potenziale svantaggio di proibire qualsiasi incorniciatura del tuo contenuto (esempio: quando gli utenti visitano il tuo sito web utilizzando una pagina di risultati di ricerca di immagini di Google).

Tuttavia, dovresti assicurarti di inviare l'intestazione `X-Frame-Options` per tutte le pagine che consentono a un utente di effettuare un'operazione che cambia lo stato (esempio: pagine che contengono link di acquisto con un clic, pagine di pagamento o conferma di trasferimento bancario, pagine che effettuano modifiche di configurazione permanenti, ecc.).

```apacheconf
<IfModule mod_headers.c>
  Header always set X-Frame-Options "DENY" "expr=%{CONTENT_TYPE} =~ m#text/html#i"
</IfModule>
```

## Content Security Policy (CSP)

[CSP (Content Security Policy)](https://content-security-policy.com/) mitiga il rischio di scripting cross-site e altri attacchi di iniezione di contenuti impostando una `Content Security Policy` che consente fonti fidate di contenuto per il tuo sito web.

Non esiste una policy che si adatti a tutti i siti web, l'esempio seguente è inteso come linea guida per te da modificare per il tuo sito.

Per semplificare l'implementazione di CSP, puoi utilizzare un [generatore di intestazioni CSP](https://report-uri.com/home/generate/) online. Dovresti anche utilizzare un [validatore](https://csp-evaluator.withgoogle.com/) per assicurarti che la tua intestazione faccia quello che vuoi.

```apacheconf
<IfModule mod_headers.c>
  Content-Security-Policy "default-src 'self'; base-uri 'none'; form-action 'self'; frame-ancestors 'none'; upgrade-insecure-requests" "expr=%{CONTENT_TYPE} =~ m#text\/(html|javascript)|application\/pdf|xml#i"
</IfModule>
```

## Accesso alla directory

Questa direttiva impedirà l'accesso alle directory che non hanno un file indice presente, in qualsiasi formato il server sia configurato per utilizzare, come `index.html` o `index.php`.

```apacheconf
<IfModule mod_autoindex.c>
    Options -Indexes
</IfModule>
```

## Blocca l'accesso a file e directory nascosti

Nei sistemi Macintosh e Linux, i file che iniziano con un punto sono nascosti dalla vista ma non dall'accesso se conosci il loro nome e posizione. Questi tipi di file solitamente contengono preferenze utente o lo stato conservato di un'utilità, e possono includere luoghi piuttosto privati come, ad esempio, le directory `.git` o `.svn`.

La directory `.well-known/` rappresenta [lo standard (RFC 5785)](https://datatracker.ietf.org/doc/html/rfc5785) per i percorsi "ben noti" (esempio: `/.well-known/manifest.json`, `/.well-known/keybase.txt`), e quindi, l'accesso al suo contenuto visibile non dovrebbe essere bloccato.

```apacheconf
<IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteCond %{REQUEST_URI} "!(^|/)\.well-known/([^./]+./?)+$" [NC]
    RewriteCond %{SCRIPT_FILENAME} -d [OR]
    RewriteCond %{SCRIPT_FILENAME} -f
    RewriteRule "(^|/)\." - [F]
</IfModule>
```

## Blocca l'accesso a file con informazioni sensibili

Blocca l'accesso ai file di backup e di origine che possono essere lasciati da alcuni editor di testo e possono rappresentare un rischio per la sicurezza quando chiunque ha accesso ad essi.

Aggiorna l'espressione regolare `<FilesMatch>` nel seguente esempio per includere qualsiasi file che potrebbe finire sul tuo server di produzione e può esporre informazioni sensibili sul tuo sito web. Questi file possono includere file di configurazione o file che contengono metadati sul progetto, tra gli altri.

```apacheconf
<IfModule mod_authz_core.c>
  <FilesMatch "(^#.*#|\.(bak|conf|dist|fla|in[ci]|log|orig|psd|sh|sql|sw[op])|~)$">
    Require all denied
  </FilesMatch>
</IfModule>
```

## HTTP Strict Transport Security (HSTS)

Se un utente digita `example.com` nel browser, anche se il server lo reindirizza alla versione sicura del sito web, ciò lascia comunque una finestra di opportunità (la connessione HTTP iniziale) per un attaccante di degradare o reindirizzare la richiesta.

L'intestazione seguente assicura che un browser si connetta al tuo server solo tramite HTTPS, indipendentemente da cosa gli utenti digitano nella barra degli indirizzi del browser.

Tieni presente che Strict Transport Security non è revocabile, e devi assicurarti di essere in grado di servire il sito tramite HTTPS per tutto il tempo specificato nella direttiva `max-age`. Se non hai più una connessione TLS valida (esempio: a causa di un certificato TLS scaduto), i tuoi visitatori vedranno un messaggio di errore anche tentando di connettersi tramite HTTP.

```apacheconf
<IfModule mod_headers.c>
  # Header always set
  Strict-Transport-Security "max-age=16070400; includeSubDomains" "expr=%{HTTPS} == 'on'"
  # (1) Enable your site for HSTS preload inclusion.
  # Header always set
  Strict-Transport-Security "max-age=31536000; includeSubDomains; preload" "expr=%{HTTPS} == 'on'"
</IfModule>
```

## Prevenzione dell'interpretazione MIME in alcuni browser

1. Limita tutte le richieste di fetch per impostazione predefinita all'origine del sito web corrente impostando la direttiva `default-src` su `'self'` - che funge da fallback per tutte le {{Glossary("Fetch_directive", "direttive Fetch")}}.

   - Questo è conveniente poiché non devi specificare tutte le direttive Fetch che si applicano al tuo sito, ad esempio: `connect-src 'self'; font-src 'self'; script-src 'self'; style-src 'self'`, ecc.
   - Questa restrizione significa anche che devi definire esplicitamente da quale sito il tuo sito web è autorizzato a caricare risorse. Altrimenti, sarà limitato alla stessa origine della pagina che fa la richiesta

2. Non consente l'uso dell'elemento `<base>` sul sito. Questo per impedire agli attaccanti di modificare le posizioni delle risorse caricate da URL relativi

   - Se vuoi utilizzare l'elemento `<base>`, allora usa `base-uri 'self'` invece

3. Consente solo che i moduli siano inviati dall'origine corrente con: `form-action 'self'`
4. Impedisce a tutti i siti web (compreso il tuo) di incorporare le tue pagine web all'interno di e.g., l'elemento `<iframe>` o `<object>` impostando: `frame-ancestors 'none'`.

   - La direttiva `frame-ancestors` aiuta a evitare attacchi di [clickjacking](/it/docs/Web/Security/Attacks/Clickjacking) ed è simile all'intestazione `X-Frame-Options`
   - I browser che supportano l'intestazione CSP ignoreranno `X-Frame-Options` se è specificato anche `frame-ancestors`

5. Costringe il browser a trattare tutte le risorse servite su HTTP come se fossero caricate in sicurezza su HTTPS impostando la direttiva `upgrade-insecure-requests`

   - **`upgrade-insecure-requests` non garantisce HTTPS per la navigazione principale. Se vuoi costringere il sito web stesso a essere caricato su HTTPS devi includere l'intestazione `Strict-Transport-Security`**

6. Include l'intestazione `Content-Security-Policy` in tutte le risposte che possono eseguire script. Questo include i tipi di file comunemente usati: documenti HTML, XML e PDF. Anche se i file JavaScript non possono eseguire script in un "contesto di navigazione", sono inclusi per mirare ai [web worker](/it/docs/Web/HTTP/Reference/Headers/Content-Security-Policy#csp_in_workers)

Alcuni browser più vecchi proverebbero a indovinare il tipo di contenuto di una risorsa, anche quando non è configurato correttamente sul server. Ciò riduce l'esposizione agli attacchi di download drive-by e alle perdite di dati cross-origin.

```apacheconf
<IfModule mod_headers.c>
    Header always set X-Content-Type-Options "nosniff"
</IfModule>
```

## Politica dei referrer

Includiamo l'intestazione `Referrer-Policy` nelle risposte per le risorse in grado di richiedere (o navigare verso) altre risorse.

Questo include i tipi di risorse comunemente usati: documenti HTML, CSS, XML/SVG, PDF, script e worker.

Per impedire completamente la fuga di referrer, specifica il valore `no-referrer`. Nota che l'effetto potrebbe influire negativamente sugli strumenti di analisi.

Usa servizi come quelli sotto per controllare la tua `Referrer-Policy`:

- [HTTP Observatory](/en-US/observatory)
- [securityheaders.com](https://securityheaders.com/)

```apacheconf
<IfModule mod_headers.c>
  Header always set Referrer-Policy "strict-origin-when-cross-origin" "expr=%{CONTENT_TYPE} =~ m#text\/(css|html|javascript)|application\/pdf|xml#i"
</IfModule>
```

## Disabilita il metodo HTTP `TRACE`

Il metodo [TRACE](/it/docs/Web/HTTP/Reference/Methods/TRACE), sebbene apparentemente innocuo, può essere utilizzato con successo in alcuni scenari per rubare le credenziali di utenti legittimi. Consulta [Un attacco di Cross-Site Tracing (XST)](https://owasp.org/www-community/attacks/Cross_Site_Tracing) e la [Guida al test della sicurezza web di OWASP](https://owasp.org/www-project-web-security-testing-guide/v41/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/06-Test_HTTP_Methods#test-xst-potential).

I browser moderni ora impediscono le richieste TRACE effettuate tramite JavaScript, tuttavia, sono stati scoperti altri modi di inviare richieste TRACE con i browser, come usando Java.

Se hai accesso al file di configurazione principale del server, usa la direttiva [`TraceEnable`](https://httpd.apache.org/docs/current/mod/core.html#traceenable) invece.

```apacheconf
<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteCond %{REQUEST_METHOD} ^TRACE [NC]
  RewriteRule .* - [R=405,L]
</IfModule>
```

## Rimuovi l'intestazione di risposta `X-Powered-By`

Alcuni framework come PHP e ASP.NET impostano un'intestazione `X-Powered-By` che contiene informazioni su di essi (esempio, il loro nome, il numero di versione).

Questa intestazione non fornisce alcun valore e, in alcuni casi, le informazioni che fornisce possono esporre vulnerabilità.

```apacheconf
<IfModule mod_headers.c>
  Header unset X-Powered-By
  Header always unset X-Powered-By
</IfModule>
```

Se puoi, dovresti disabilitare l'intestazione `X-Powered-By` dalla lingua/livello del framework (esempio: per PHP, puoi farlo impostando quanto segue in `php.ini`).

```ini
expose_php = off;
```

## Rimuovere il piè di pagina delle informazioni sul server generato da Apache

Impedisci ad Apache di aggiungere una riga di piè di pagina contenente informazioni sul server ai documenti generati dal server (esempio, messaggi di errore, liste di directory, ecc.). Consulta la documentazione della direttiva [`ServerSignature`](https://httpd.apache.org/docs/current/mod/core.html#serversignature) per ulteriori informazioni su cosa fornisce la firma del server e la direttiva [`ServerTokens`](https://httpd.apache.org/docs/current/mod/core.html#servertokens) per informazioni sulla configurazione delle informazioni fornite nella firma.

```apacheconf
ServerSignature Off
```

## Correggi le intestazioni `AcceptEncoding` rotte

Alcuni proxy e software di sicurezza storpiano o rimuovono l'intestazione HTTP `Accept-Encoding`. Consulta [Pushing Beyond Gzipping](https://calendar.perfplanet.com/2010/pushing-beyond-gzipping/) per una spiegazione più dettagliata.

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

## Comprimi tipi di media

Comprimi tutte le uscite etichettate con uno dei seguenti tipi di media utilizzando la [Direttiva AddOutputFilterByType](https://httpd.apache.org/docs/current/mod/mod_filter.html#addoutputfilterbytype).

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

## Mappa estensioni ai tipi di media

Mappa le seguenti estensioni di nome file al tipo di codifica specificato utilizzando [AddEncoding](https://httpd.apache.org/docs/current/mod/mod_mime.html#addencoding) in modo che Apache possa servire i tipi di file con l'intestazione di risposta `Content-Encoding` appropriata (questo NON farà comprimere Apache!). Se questi tipi di file fossero serviti senza un'intestazione di risposta `Content-Encoding` appropriata, le applicazioni client (esempio, browser) non saprebbero che devono prima decomprimere la risposta e, quindi, non sarebbero in grado di comprendere il contenuto.

```apacheconf
<IfModule mod_deflate.c>
  <IfModule mod_mime.c>
    AddEncoding gzip svgz
  </IfModule>
</IfModule>
```

## Scadenza della cache

Servi le risorse con una data di scadenza lontana nel futuro utilizzando il modulo [mod_expires](https://httpd.apache.org/docs/current/mod/mod_expires.html), e le intestazioni [Cache-Control](/it/docs/Web/HTTP/Reference/Headers/Cache-Control) e [Expires](/it/docs/Web/HTTP/Reference/Headers/Expires).

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
