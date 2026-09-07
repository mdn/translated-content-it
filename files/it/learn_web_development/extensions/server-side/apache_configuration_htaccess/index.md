---
title: "Configurazione Apache: .htaccess"
short-title: Apache .htaccess
slug: Learn_web_development/Extensions/Server-side/Apache_Configuration_htaccess
l10n:
  sourceCommit: c9f3d85f24d7839c9fe36a68d8042d088d906147
---

I file Apache .htaccess consentono agli utenti di configurare le directory del server web che controllano senza modificare il file di configurazione principale.

Sebbene sia utile, è importante notare che l'uso dei file `.htaccess` rallenta Apache; quindi, se si ha accesso al file di configurazione principale del server (solitamente denominato `httpd.conf`), questa logica dovrebbe essere aggiunta lì, all'interno di un blocco `Directory`.

Per maggiori dettagli su ciò che i file .htaccess possono fare, consultare [.htaccess](https://httpd.apache.org/docs/current/howto/htaccess.html) nel sito della documentazione di Apache HTTPD.

Il resto di questo documento descrive diverse opzioni di configurazione che si possono aggiungere a `.htaccess` e la relativa funzione.

La maggior parte dei blocchi seguenti usa la direttiva [IfModule](https://httpd.apache.org/docs/2.4/mod/core.html#ifmodule) per eseguire le istruzioni all'interno del blocco solo se il modulo corrispondente è stato configurato correttamente e il server lo ha caricato. In questo modo si evita che il server vada in errore se il modulo non è stato caricato.

## Reindirizzamenti

Talvolta è necessario comunicare agli utenti che una risorsa è stata spostata, temporaneamente o permanentemente. A questo scopo si usano `Redirect` e `RedirectMatch`.

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

I possibili valori del primo parametro sono elencati di seguito. Se il primo parametro non è incluso, il valore predefinito è `temp`.

- permanent
  - : Restituisce uno stato di reindirizzamento permanente (301), a indicare che la risorsa è stata spostata permanentemente.
- temp
  - : Restituisce uno stato di reindirizzamento temporaneo (302). **Questo è il valore predefinito**.
- seeother
  - : Restituisce uno stato "See Other" (303), a indicare che la risorsa è stata sostituita.
- gone
  - : Restituisce uno stato "Gone" (410), a indicare che la risorsa è stata rimossa permanentemente. Quando si usa questo stato, l'argomento _URL_ deve essere omesso.

## Risorse cross-origin

Il primo insieme di direttive controlla l'accesso [CORS](https://fetch.spec.whatwg.org/) (Cross-Origin Resource Sharing) alle risorse del server. CORS è un meccanismo basato su header HTTP che consente a un server di indicare le origini esterne (dominio, protocollo o porta) dalle quali un browser dovrebbe consentire il caricamento delle risorse.

Per ragioni di sicurezza, i browser limitano le richieste HTTP cross-origin avviate dagli script. Ad esempio, XMLHttpRequest e la Fetch API seguono la same-origin policy. Un'applicazione web che usa queste API può richiedere risorse solo dalla stessa origine da cui è stata caricata l'applicazione, a meno che la risposta proveniente da altre origini non includa gli header CORS appropriati.

### Accesso CORS generale

Questa direttiva aggiunge l'header CORS per tutte le risorse nella directory da qualsiasi sito web.

```apacheconf
<IfModule mod_headers.c>
  Header set Access-Control-Allow-Origin "*"
</IfModule>
```

A meno che la direttiva non venga sovrascritta successivamente nella configurazione o nella configurazione di una directory sottostante rispetto a quella in cui è stata impostata, verrà accettata ogni richiesta proveniente da server esterni, il che probabilmente non è ciò che si desidera.

Un'alternativa consiste nell'indicare esplicitamente quali domini possono accedere al contenuto del sito. Nell'esempio seguente, l'accesso è limitato a un sottodominio del sito principale (example.com). Questa soluzione è più sicura e, probabilmente, è quella prevista.

```apacheconf
<IfModule mod_headers.c>
  Header set Access-Control-Allow-Origin "subdomain.example.com"
</IfModule>
```

### Immagini cross-origin

Come riportato nel [blog di Chromium](https://blog.chromium.org/2011/07/using-cross-domain-images-in-webgl-and.html) e documentato in [Consentire l'uso cross-origin di immagini e canvas](/it/docs/Web/HTML/How_to/CORS_enabled_image), ciò può portare ad attacchi di {{Glossary("Fingerprinting", "fingerprinting")}}.

Per mitigare la possibilità di questi attacchi, è necessario usare l'attributo `crossorigin` nelle immagini richieste e lo snippet di codice seguente nel proprio `.htaccess` per impostare l'header CORS dal server.

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

La [Guida alla risoluzione dei problemi di Google Fonts](https://fonts.google.com/faq#troubleshooting) di Google Chrome informa che, sebbene Google Fonts possa inviare l'header CORS con ogni risposta, alcuni server proxy potrebbero rimuoverlo prima che il browser possa usarlo per renderizzare il font.

```apacheconf
<IfModule mod_headers.c>
  <FilesMatch "\.(eot|otf|tt[cf]|woff2?)$">
    Header set Access-Control-Allow-Origin "*"
  </FilesMatch>
</IfModule>
```

### Timing delle risorse cross-origin

La specifica [Resource Timing](https://w3c.github.io/resource-timing/) definisce un'interfaccia che consente alle applicazioni web di accedere alle informazioni complete sui tempi delle risorse in un documento.

L'header di risposta [`Timing-Allow-Origin`](/it/docs/Web/HTTP/Reference/Headers/Timing-Allow-Origin) specifica le origini autorizzate a visualizzare i valori degli attributi ottenuti tramite funzionalità della Resource Timing API, che altrimenti verrebbero riportati come zero a causa delle restrizioni cross-origin.

Se una risorsa non viene fornita con `Timing-Allow-Origin` o se l'header non include l'origine dopo l'esecuzione della richiesta, alcuni attributi dell'oggetto `PerformanceResourceTiming` verranno impostati su zero.

```apacheconf
<IfModule mod_headers.c>
  Header set Timing-Allow-Origin: "*"
</IfModule>
```

## Pagine/messaggi di errore personalizzati

Apache consente di fornire agli utenti pagine di errore personalizzate in base al tipo di errore ricevuto.

Le pagine di errore vengono presentate come URL. Questi URL possono iniziare con una barra (`/`) per i percorsi web locali, relativi a DocumentRoot, oppure essere un URL completo risolvibile dal client.

Per ulteriori informazioni, consultare la documentazione della [direttiva ErrorDocument](https://httpd.apache.org/docs/current/mod/core.html#errordocument) sul sito della documentazione HTTPD.

```apacheconf
ErrorDocument 500 /errors/500.html
ErrorDocument 404 /errors/400.html
ErrorDocument 401 https://example.com/subscription_info.html
ErrorDocument 403 "Sorry, can't allow you access today."
```

## Prevenzione degli errori

Questa impostazione influenza il funzionamento di MultiViews per la directory a cui si applica la configurazione.

L'effetto di `MultiViews` è il seguente: se il server riceve una richiesta per /some/dir/foo, se /some/dir ha `MultiViews` abilitato e /some/dir/foo non esiste, il server legge la directory cercando file denominati foo.\*, quindi crea di fatto una mappa dei tipi che nomina tutti questi file, assegnando loro gli stessi tipi di media e content-encoding che avrebbe assegnato se il client avesse richiesto uno di essi per nome. Quindi seleziona la corrispondenza migliore con i requisiti del client.

L'impostazione disabilita `MultiViews` per la directory a cui si applica questa configurazione e impedisce ad Apache di restituire un errore 404 come risultato di una riscrittura quando la directory con lo stesso nome non esiste.

```apacheconf
Options -MultiViews
```

## Tipi di media e codifiche dei caratteri

Apache usa [mod_mime](https://httpd.apache.org/docs/current/mod/mod_mime.html#addtype) per assegnare metadati del contenuto al contenuto selezionato per una risposta HTTP, associando i pattern nell'URI o nei nomi di file ai valori dei metadati.

Ad esempio, le estensioni dei nomi di file dei file di contenuto definiscono spesso il tipo di media Internet, la lingua, il set di caratteri e la codifica del contenuto. Queste informazioni vengono inviate nei messaggi HTTP che contengono tale contenuto e usate nella negoziazione del contenuto durante la selezione delle alternative, in modo che le preferenze dell'utente siano rispettate quando si sceglie uno tra diversi contenuti possibili da fornire.

**La modifica dei metadati di un file non cambia il valore dell'header Last-Modified. Pertanto, copie precedentemente memorizzate nella cache potrebbero essere ancora usate da un client o proxy con gli header precedenti. Se si modificano i metadati (lingua, tipo di contenuto, set di caratteri o codifica), potrebbe essere necessario eseguire il comando "touch" sui file interessati, aggiornandone la data dell'ultima modifica, per garantire che tutti i visitatori ricevano gli header del contenuto corretti.**

### Servire le risorse con i tipi di media appropriati (noti anche come tipi MIME)

Associa tipi di media a una o più estensioni per assicurare che le risorse vengano servite correttamente.

I server dovrebbero usare `text/javascript` per le risorse JavaScript, come indicato nella [specifica HTML](https://html.spec.whatwg.org/multipage/scripting.html#scriptingLanguages).

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

Ogni contenuto sul web dispone di un set di caratteri. La maggior parte, se non tutti, i contenuti sono Unicode UTF-8.

Usare [AddDefaultCharset](https://httpd.apache.org/docs/current/mod/core.html#adddefaultcharset) per servire tutte le risorse etichettate come `text/html` o `text/plain` con il charset `UTF-8`.

```apacheconf
<IfModule mod_mime.c>
  AddDefaultCharset utf-8
</IfModule>
```

## Impostare il charset per tipi di media specifici

Servire i seguenti tipi di file con il parametro `charset` impostato su `UTF-8` usando la direttiva [AddCharset](https://httpd.apache.org/docs/current/mod/mod_mime.html#addcharset) disponibile in `mod_mime`.

```apacheconf
<IfModule mod_mime.c>
  AddCharset utf-8 \
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

## Le direttive `Mod_rewrite` e `RewriteEngine`

[mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html) fornisce un modo per modificare dinamicamente le richieste URL in entrata, in base a regole di espressioni regolari. Ciò consente di mappare URL arbitrari sulla struttura URL interna in qualsiasi modo desiderato.

Supporta un numero illimitato di regole e un numero illimitato di condizioni di regola associate a ogni regola, fornendo un meccanismo di manipolazione degli URL realmente flessibile e potente. Le manipolazioni degli URL possono dipendere da vari test: variabili del server, variabili d'ambiente, header HTTP, timestamp, ricerche in database esterni e vari altri programmi o handler esterni possono essere usati per ottenere una corrispondenza degli URL granulare.

### Abilitare `mod_rewrite`

Il pattern di base per abilitare `mod_rewrite` è un prerequisito per tutte le altre attività che lo usano.

I passaggi richiesti sono:

1. Attivare il motore di riscrittura, necessario affinché le direttive `RewriteRule` funzionino, come documentato nella documentazione di [RewriteEngine](https://httpd.apache.org/docs/current/mod/mod_rewrite.html#RewriteEngine).
2. Abilitare l'opzione `FollowSymLinks` se non è già abilitata. Consultare la documentazione delle [Core Options](https://httpd.apache.org/docs/current/mod/core.html#options).
3. Se l'host web non consente l'opzione `FollowSymlinks`, è necessario commentarla o rimuoverla e quindi decommentare la riga `Options +SymLinksIfOwnerMatch`, tenendo però presente l'[impatto sulle prestazioni](https://httpd.apache.org/docs/current/misc/perf-tuning.html#symlinks).
   - Alcuni servizi di hosting cloud richiedono l'impostazione di `RewriteBase`.
   - Consultare le [FAQ di Rackspace](https://web.archive.org/web/20151223141222/http://www.rackspace.com/knowledge_center/frequently-asked-question/why-is-modrewrite-not-working-on-my-site) e la [documentazione HTTPD](https://httpd.apache.org/docs/current/mod/mod_rewrite.html#rewritebase).
   - A seconda della configurazione del server, potrebbe essere necessario usare anche la direttiva [`RewriteOptions`](https://httpd.apache.org/docs/current/mod/mod_rewrite.html#rewriteoptions) per abilitare alcune opzioni del motore di riscrittura.

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

Queste regole Rewrite reindirizzano dalla versione non sicura `http://` alla versione sicura `https://` dell'URL, come descritto nel [wiki di Apache HTTPD](https://cwiki.apache.org/confluence/spaces/HTTPD/pages/115522478/RewriteHTTPToHTTPS).

```apacheconf
<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteCond %{HTTPS} !=on
  RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R,L]
</IfModule>
```

Se si usa cPanel AutoSSL o il metodo webroot di Let's Encrypt per creare i certificati TLS, la convalida del certificato non riuscirà se le richieste di convalida vengono reindirizzate a HTTPS. Attivare le condizioni necessarie.

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

### Reindirizzare dagli URL `www.`

Queste direttive riscrivono `www.example.com` in `example.com`.

Il contenuto non dovrebbe essere duplicato in più origini, con e senza www. Ciò può causare problemi SEO, ovvero contenuto duplicato; pertanto, è necessario scegliere una delle alternative e reindirizzare l'altra. È inoltre necessario usare gli [URL canonici](https://www.semrush.com/blog/canonical-url-guide/) per indicare quale URL i motori di ricerca dovrebbero sottoporre a scansione, se supportano la funzionalità.

Impostare la variabile `%{ENV:PROTO}` per consentire alle riscritture di reindirizzare automaticamente con lo schema appropriato (`http` o `https`).

Per impostazione predefinita, la regola presuppone che siano disponibili sia ambienti HTTP sia HTTPS per il reindirizzamento.

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

### Inserire `www.` all'inizio degli URL

Queste regole inseriscono `www.` all'inizio di un URL. È importante notare che lo stesso contenuto non dovrebbe mai essere disponibile con due URL diversi.

Ciò può causare problemi SEO, ovvero contenuto duplicato; pertanto, è necessario scegliere una delle alternative e reindirizzare l'altra. Per i motori di ricerca che li supportano, è necessario usare gli [URL canonici](https://www.semrush.com/blog/canonical-url-guide/) per indicare quale URL dovrebbero sottoporre a scansione.

Impostare la variabile `%{ENV:PROTO}` per consentire alle riscritture di reindirizzare automaticamente con lo schema appropriato (`http` o `https`).

Per impostazione predefinita, la regola presuppone che siano disponibili sia ambienti HTTP sia HTTPS per il reindirizzamento. Se il certificato TLS non può gestire uno dei domini usati durante il reindirizzamento, è necessario attivare la condizione.

Quanto segue potrebbe non essere una buona idea se si usano sottodomini "reali" per determinate parti del sito web.

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

L'esempio seguente invia l'header di risposta `X-Frame-Options` con DENY come valore, informando i browser di non visualizzare il contenuto della pagina web in alcun frame, per proteggere il sito web dal [clickjacking](/it/docs/Web/Security/Attacks/Clickjacking).

Questa potrebbe non essere l'impostazione migliore per tutti. Leggere informazioni sugli [altri due possibili valori per l'header `X-Frame-Options`](https://datatracker.ietf.org/doc/html/rfc7034#section-2.1): `SAMEORIGIN` e `ALLOW-FROM`.

Sebbene sia possibile inviare l'header `X-Frame-Options` per tutte le pagine del sito web, ciò presenta il potenziale svantaggio di vietare qualsiasi framing del contenuto, ad esempio quando gli utenti visitano il sito web tramite una pagina dei risultati di Ricerca immagini Google.

Tuttavia, è necessario assicurarsi di inviare l'header `X-Frame-Options` per tutte le pagine che consentono a un utente di eseguire un'operazione che modifica lo stato, ad esempio pagine contenenti link di acquisto con un clic, pagine di checkout o di conferma del trasferimento bancario, pagine che apportano modifiche permanenti alla configurazione e così via.

```apacheconf
<IfModule mod_headers.c>
  Header always set X-Frame-Options "DENY" "expr=%{CONTENT_TYPE} =~ m#text/html#i"
</IfModule>
```

## Content Security Policy (CSP)

[CSP (Content Security Policy)](https://content-security-policy.com/) riduce il rischio di cross-site scripting e di altri attacchi di injection del contenuto impostando una `Content Security Policy` che consente fonti attendibili di contenuto per il sito web.

Non esiste una policy adatta a tutti i siti web; l'esempio seguente è inteso come linea guida da modificare per il proprio sito.

Per semplificare l'implementazione di CSP, è possibile usare un [generatore di header CSP](https://report-uri.com/home/generate/) online. È inoltre opportuno usare un [validatore](https://csp-evaluator.withgoogle.com/) per assicurarsi che l'header svolga la funzione desiderata.

```apacheconf
<IfModule mod_headers.c>
  Content-Security-Policy "default-src 'self'; base-uri 'none'; form-action 'self'; frame-ancestors 'none'; upgrade-insecure-requests" "expr=%{CONTENT_TYPE} =~ m#text\/(html|javascript)|application\/pdf|xml#i"
</IfModule>
```

Questa CSP:

1. Limita tutte le operazioni di fetch per impostazione predefinita all'origine del sito web corrente, impostando la direttiva `default-src` su `'self'`, che agisce come fallback per tutte le {{Glossary("Fetch_directive", "direttive Fetch")}}.
   - Questo è pratico poiché non è necessario specificare tutte le direttive Fetch applicabili al sito, ad esempio: `connect-src 'self'; font-src 'self'; script-src 'self'; style-src 'self'` e così via.
   - Questa restrizione significa anche che è necessario definire esplicitamente da quali siti il sito web può caricare risorse. Altrimenti, sarà limitato alla stessa origine della pagina che effettua la richiesta.

2. Non consente l'elemento `<base>` sul sito web. Ciò evita che gli aggressori modifichino le posizioni delle risorse caricate da URL relativi.
   - Se si desidera usare l'elemento `<base>`, usare invece `base-uri 'self'`.

3. Consente l'invio dei moduli solo dall'origine corrente con: `form-action 'self'`.
4. Impedisce a tutti i siti web, incluso il proprio, di incorporare le pagine web all'interno, ad esempio, dell'elemento `<iframe>` o `<object>`, impostando: `frame-ancestors 'none'`.
   - La direttiva `frame-ancestors` aiuta a evitare gli attacchi di [clickjacking](/it/docs/Web/Security/Attacks/Clickjacking) ed è simile all'header `X-Frame-Options`.
   - I browser che supportano l'header CSP ignorano `X-Frame-Options` se viene specificato anche `frame-ancestors`.

5. Forza il browser a trattare tutte le risorse servite tramite HTTP come se fossero caricate in modo sicuro tramite HTTPS, impostando la direttiva `upgrade-insecure-requests`.
   - **`upgrade-insecure-requests` non garantisce HTTPS per la navigazione di livello superiore. Per forzare il caricamento del sito web stesso tramite HTTPS, è necessario includere l'header `Strict-Transport-Security`.**

6. Include l'header `Content-Security-Policy` in tutte le risposte in grado di eseguire script. Ciò include i tipi di file comunemente usati: documenti HTML, XML e PDF. Sebbene i file JavaScript non possano eseguire script in un "browsing context", sono inclusi per includere i [web worker](/it/docs/Web/HTTP/Reference/Headers/Content-Security-Policy#csp_in_workers).

## Accesso alle directory

Questa direttiva impedisce l'accesso alle directory che non dispongono di un file indice nel formato configurato sul server, ad esempio `index.html` o `index.php`.

```apacheconf
<IfModule mod_autoindex.c>
    Options -Indexes
</IfModule>
```

## Bloccare l'accesso a file e directory nascosti

Nei sistemi Macintosh e Linux, i file che iniziano con un punto sono nascosti alla vista, ma non all'accesso se se ne conoscono nome e posizione. Questi tipi di file di solito contengono preferenze utente o lo stato salvato di un'utilità e possono includere posizioni piuttosto private, come ad esempio le directory `.git` o `.svn`.

La directory `.well-known/` rappresenta il prefisso di percorso [standard (RFC 5785)](https://datatracker.ietf.org/doc/html/rfc5785) per le "well-known locations", ad esempio `/.well-known/manifest.json` e `/.well-known/keybase.txt`; pertanto, l'accesso al suo contenuto visibile non deve essere bloccato.

```apacheconf
<IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteCond %{REQUEST_URI} "!(^|/)\.well-known/([^./]+./?)+$" [NC]
    RewriteCond %{SCRIPT_FILENAME} -d [OR]
    RewriteCond %{SCRIPT_FILENAME} -f
    RewriteRule "(^|/)\." - [F]
</IfModule>
```

## Bloccare l'accesso a file contenenti informazioni sensibili

Bloccare l'accesso ai file di backup e sorgente che potrebbero essere lasciati da alcuni editor di testo e che possono rappresentare un rischio per la sicurezza quando chiunque può accedervi.

Aggiornare l'espressione regolare `<FilesMatch>` nell'esempio seguente per includere eventuali file che potrebbero finire sul server di produzione e che possono esporre informazioni sensibili sul sito web. Questi file possono includere file di configurazione o file contenenti metadati del progetto, tra gli altri.

```apacheconf
<IfModule mod_authz_core.c>
  <FilesMatch "(^#.*#|\.(bak|conf|dist|fla|in[ci]|log|orig|psd|sh|sql|sw[op])|~)$">
    Require all denied
  </FilesMatch>
</IfModule>
```

## HTTP Strict Transport Security (HSTS)

Se un utente digita `example.com` nel browser, anche se il server lo reindirizza alla versione sicura del sito web, resta comunque una finestra di opportunità, ovvero la connessione HTTP iniziale, per un aggressore che voglia effettuare il downgrade o reindirizzare la richiesta.

L'header seguente garantisce che un browser si connetta al server solo tramite HTTPS, indipendentemente da ciò che gli utenti digitano nella barra degli indirizzi del browser.

Tenere presente che Strict Transport Security non è revocabile e che è necessario assicurarsi di poter servire il sito tramite HTTPS per tutto il tempo specificato nella direttiva `max-age`. Se non si dispone più di una connessione TLS valida, ad esempio a causa di un certificato TLS scaduto, i visitatori visualizzeranno un messaggio di errore anche quando tentano di connettersi tramite HTTP.

```apacheconf
<IfModule mod_headers.c>
  # Header always set
  Strict-Transport-Security "max-age=16070400; includeSubDomains" "expr=%{HTTPS} == 'on'"
  # (1) Enable your site for HSTS preload inclusion.
  # Header always set
  Strict-Transport-Security "max-age=31536000; includeSubDomains; preload" "expr=%{HTTPS} == 'on'"
</IfModule>
```

## Impedire ad alcuni browser di effettuare MIME sniffing della risposta

Alcuni browser meno recenti tentavano di indovinare il tipo di contenuto di una risorsa, anche quando non è configurato correttamente nella configurazione del server. Questo riduce l'esposizione ad attacchi drive-by download e alla perdita di dati cross-origin.

```apacheconf
<IfModule mod_headers.c>
    Header always set X-Content-Type-Options "nosniff"
</IfModule>
```

## Policy del referrer

L'header `Referrer-Policy` viene incluso nelle risposte per le risorse in grado di richiedere o raggiungere tramite navigazione altre risorse.

Ciò include i tipi di risorse comunemente usati: documenti HTML, CSS, XML/SVG e PDF, script e worker.

Per prevenire completamente la perdita del referrer, specificare invece il valore `no-referrer`. Notare che l'effetto potrebbe influire negativamente sugli strumenti di analisi.

Usare servizi come quelli seguenti per controllare la propria `Referrer-Policy`:

- [HTTP Observatory](/en-US/observatory)
- [securityheaders.com](https://securityheaders.com/)

```apacheconf
<IfModule mod_headers.c>
  Header always set Referrer-Policy "strict-origin-when-cross-origin" "expr=%{CONTENT_TYPE} =~ m#text\/(css|html|javascript)|application\/pdf|xml#i"
</IfModule>
```

## Disabilitare il metodo HTTP `TRACE`

Il metodo [TRACE](/it/docs/Web/HTTP/Reference/Methods/TRACE), sebbene apparentemente innocuo, può essere sfruttato con successo in alcuni scenari per rubare le credenziali di utenti legittimi. Consultare [Un attacco Cross-Site Tracing (XST)](https://owasp.org/www-community/attacks/Cross_Site_Tracing) e la [OWASP Web Security Testing Guide](https://owasp.org/www-project-web-security-testing-guide/v41/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/06-Test_HTTP_Methods#test-xst-potential).

I browser moderni ora impediscono le richieste TRACE effettuate tramite JavaScript; tuttavia, sono stati scoperti altri modi per inviare richieste TRACE con i browser, ad esempio usando Java.

Se si ha accesso al file di configurazione principale del server, usare invece la direttiva [`TraceEnable`](https://httpd.apache.org/docs/current/mod/core.html#traceenable).

```apacheconf
<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteCond %{REQUEST_METHOD} ^TRACE [NC]
  RewriteRule .* - [R=405,L]
</IfModule>
```

## Rimuovere l'header di risposta `X-Powered-By`

Alcuni framework, come PHP e ASP.NET, impostano un header `X-Powered-By` che contiene informazioni su di essi, ad esempio il nome e il numero di versione.

Questo header non fornisce alcun valore e, in alcuni casi, le informazioni che fornisce possono esporre vulnerabilità.

```apacheconf
<IfModule mod_headers.c>
  Header unset X-Powered-By
  Header always unset X-Powered-By
</IfModule>
```

Se possibile, è opportuno disabilitare l'header `X-Powered-By` a livello del linguaggio/framework, ad esempio per PHP è possibile farlo impostando quanto segue in `php.ini`.

```ini
expose_php = off;
```

## Rimuovere il piè di pagina con le informazioni del server generato da Apache

Impedire ad Apache di aggiungere una riga di piè di pagina finale contenente informazioni sul server ai documenti generati dal server, ad esempio messaggi di errore, elenchi di directory e così via. Per maggiori informazioni sui dati forniti dalla firma del server, consultare la documentazione della [direttiva `ServerSignature`](https://httpd.apache.org/docs/current/mod/core.html#serversignature); per informazioni sulla configurazione dei dati forniti nella firma, consultare la [direttiva `ServerTokens`](https://httpd.apache.org/docs/current/mod/core.html#servertokens).

```apacheconf
ServerSignature Off
```

## Correggere gli header `AcceptEncoding` non validi

Alcuni proxy e software di sicurezza alterano o rimuovono l'header HTTP `Accept-Encoding`. Consultare [Pushing Beyond Gzipping](https://calendar.perfplanet.com/2010/pushing-beyond-gzipping/) per una spiegazione più dettagliata.

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

Comprimere tutto l'output etichettato con uno dei seguenti tipi di media usando la [direttiva AddOutputFilterByType](https://httpd.apache.org/docs/current/mod/mod_filter.html#addoutputfilterbytype).

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

Mappare le seguenti estensioni dei nomi di file al tipo di codifica specificato usando [AddEncoding](https://httpd.apache.org/docs/current/mod/mod_mime.html#addencoding), affinché Apache possa servire i tipi di file con l'header di risposta `Content-Encoding` appropriato; questo **NON** farà sì che Apache li comprima. Se questi tipi di file venissero serviti senza un header di risposta `Content-Encoding` appropriato, le applicazioni client, ad esempio i browser, non saprebbero di dover prima decomprimere la risposta e quindi non sarebbero in grado di comprendere il contenuto.

```apacheconf
<IfModule mod_deflate.c>
  <IfModule mod_mime.c>
    AddEncoding gzip svgz
  </IfModule>
</IfModule>
```

## Scadenza della cache

Servire le risorse con una data di scadenza molto futura usando il modulo [mod_expires](https://httpd.apache.org/docs/current/mod/mod_expires.html), nonché gli header [Cache-Control](/it/docs/Web/HTTP/Reference/Headers/Cache-Control) e [Expires](/it/docs/Web/HTTP/Reference/Headers/Expires).

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
