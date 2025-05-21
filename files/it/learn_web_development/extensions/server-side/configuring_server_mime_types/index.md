---
title: Configurazione corretta dei tipi MIME del server
slug: Learn_web_development/Extensions/Server-side/Configuring_server_MIME_types
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

I tipi MIME descrivono il tipo di media del contenuto, sia nelle email, sia serviti dai server web o dalle applicazioni web. Sono intesi per fornire un'indicazione su come il contenuto dovrebbe essere elaborato e visualizzato.

Esempi di tipi MIME:

- `text/html` per documenti HTML.
- `text/plain` per testo normale.
- `text/css` per Cascading Style Sheets.
- `text/javascript` per file JavaScript.
- `text/markdown` per file Markdown.
- `application/octet-stream` per file binari in cui si prevede un'azione dell'utente.

Le configurazioni predefinite dei server variano enormemente e impostano diversi valori di tipo MIME _predefiniti_ per i file senza tipo di contenuto definito.

Le versioni del server Apache precedenti alla **2.2.7** erano configurate per riportare un tipo MIME di `text/plain` o `application/octet-stream` per tipi di contenuto sconosciuti. Le versioni moderne di Apache riportano `none` per i file con tipi di contenuto sconosciuti.

[Nginx](https://nginx.org/) riporterà `text/plain` se non si definisce un tipo di contenuto predefinito.

Man mano che nuovi tipi di contenuto vengono inventati o aggiunti ai server web, gli amministratori web potrebbero non aggiungere i nuovi tipi MIME alla configurazione del loro server web. Questo è una fonte importante di problemi per gli utenti di browser che rispettano i tipi MIME riportati dai server e dalle applicazioni web.

## Perché i tipi MIME corretti sono importanti?

Se un server web o un'applicazione riporta un tipo MIME errato per un contenuto (incluso un "tipo predefinito" per contenuti sconosciuti), un browser web non ha modo di conoscere le intenzioni dell'autore. Questo può causare un comportamento inatteso.

Alcuni browser web possono provare a _indovinare_ il tipo MIME corretto. Ciò consente ai server e alle applicazioni web configurate erroneamente di continuare a funzionare per quei browser (ma non per altri browser che implementano correttamente lo standard). A parte la violazione della specifica HTTP, questa è una cattiva idea per un paio di altri motivi significativi:

- Perdita di controllo

  - : Se il browser ignora il tipo MIME riportato, gli amministratori web e gli autori non hanno più controllo su come il loro contenuto deve essere elaborato.

    Ad esempio, un sito web orientato agli sviluppatori web potrebbe voler inviare alcuni documenti HTML di esempio come `text/html` o `text/plain` per avere i documenti elaborati e visualizzati come HTML o come codice sorgente. Se il browser indovina il tipo MIME, questa opzione non è più disponibile per l'autore.

- Sicurezza

  - : Alcuni tipi di contenuto, come i programmi eseguibili, sono intrinsecamente non sicuri. Per questo motivo, questi tipi MIME sono solitamente limitati riguardo alle azioni che un browser web eseguirà quando riceve quel tipo di contenuto. Un programma eseguibile non dovrebbe essere eseguito sul computer dell'utente e dovrebbe almeno causare la visualizzazione di un dialogo per **chiedere all'utente** se desidera scaricare il file.

## Tipi MIME legacy per JavaScript

Cercando informazioni sui tipi MIME di JavaScript, potreste vedere diversi tipi MIME che fanno riferimento a JavaScript. Alcuni di questi tipi MIME includono:

- `application/javascript`
- `application/ecmascript`
- `application/x-ecmascript`
- `application/x-javascript`
- `text/ecmascript`
- `text/javascript1.0`
- `text/javascript1.1`
- `text/javascript1.2`
- `text/javascript1.3`
- `text/javascript1.4`
- `text/javascript1.5`
- `text/x-ecmascript`
- `text/x-javascript`

Sebbene i browser possano supportare tutti, alcuni o nessuno di questi tipi MIME alternativi, dovreste usare **solo** `text/javascript` per indicare il tipo MIME dei file JavaScript.

> [!NOTE]
> Vedi [Tipi MIME (tipi media IANA)](/it/docs/Web/HTTP/Guides/MIME_types) per ulteriori informazioni.

## Come determinare il tipo MIME da impostare

Esistono diversi modi per determinare il valore del tipo MIME corretto da utilizzare per servire il vostro contenuto.

- Se il vostro contenuto è stato creato utilizzando software commerciale, leggete la documentazione del venditore per vedere quali tipi MIME dovrebbero essere riportati per l'applicazione.
- Consultate il registro dei [Tipi Media MIME di IANA](https://www.iana.org/assignments/media-types/media-types.xhtml), che contiene informazioni su tutti i tipi MIME registrati.
- Cercate l'estensione del file in [FILExt](https://filext.com/) o nel [Riferimento alle estensioni dei file](https://www.file-extensions.org/) per vedere quali tipi MIME sono associati a quell'estensione. Prestate attenzione poiché l'applicazione potrebbe avere più tipi MIME che differiscono solo per una lettera.

## Come controllare il tipo MIME del contenuto ricevuto

- In Firefox

  - Caricate il file e andate su **Strumenti > Informazioni pagina** per ottenere il tipo di contenuto per la pagina che avete consultato.
  - Potete anche andare su **Strumenti > Sviluppo Web > Rete** e ricaricare la pagina. La scheda delle richieste vi fornisce un elenco di tutte le risorse che la pagina ha caricato. Cliccando su qualsiasi risorsa verranno elencate tutte le informazioni disponibili, incluso l'header [`Content-Type`](/it/docs/Web/HTTP/Reference/Headers/Content-Type) della pagina.

- In Chrome

  - Caricate il file e andate su **Visualizza > Sviluppatore > Strumenti per sviluppatori** e scegliete la scheda _Rete_. Ricaricate la pagina e selezionate la risorsa che volete ispezionare. Sotto "header" cercate `Content-Type` e vi riporterà il tipo di contenuto della risorsa.

- Cercate un elemento `<meta>` nel sorgente della pagina che fornisce il tipo MIME, ad esempio `<meta http-equiv="Content-Type" content="text/html">`.

  - Secondo gli standard, l'elemento `<meta>` che specifica il tipo MIME dovrebbe essere ignorato se c'è un header Content-Type disponibile.

[IANA](https://www.iana.org/) mantiene un elenco di [Tipi Media MIME registrati](https://www.iana.org/assignments/media-types/media-types.xhtml). La [specifica HTTP](https://www.w3.org/Protocols/rfc2616/rfc2616.html) definisce un superset di tipi MIME, utilizzato per descrivere i tipi media utilizzati sul web.

## Come configurare il server per inviare i tipi MIME corretti

L'obiettivo è configurare il server per inviare l'header {{HTTPHeader("Content-Type")}} corretto per ogni documento.

- Se usate il server web Apache, controllate la sezione **_Tipi Media e Codifiche dei Caratteri_** di [Configurazione Apache: .htaccess](/it/docs/Learn_web_development/Extensions/Server-side/Apache_Configuration_htaccess) per esempi di diversi tipi di documenti e i loro corrispondenti tipi MIME.
- Se usate Nginx, notate che Nginx non ha uno strumento equivalente a `.htaccess`, quindi tutte le modifiche andranno nel file di configurazione principale.
- Se usate uno script lato server o un framework per generare contenuti, il modo per indicare il tipo di contenuto dipenderà dallo strumento che state utilizzando. Consultate la documentazione del framework o della libreria.

Indipendentemente dal sistema server che usate, l'effetto che dovete ottenere è impostare un header di risposta con il nome {{httpheader("Content-Type")}}, seguito da due punti e spazio, seguito da un tipo MIME. Gli ambienti ad alto livello spesso permettono di impostare tali header durante la generazione della pagina. Ad esempio, in un ambiente PHP, potreste impostare l'header di risposta per le risorse PDF in questo modo:

```php
header('Content-Type: application/pdf')
```

Tentare invece di impostarlo solo con `header('application/pdf')` non funzionerà.

## Link correlati

- [IANA | MIME Media Types](https://www.iana.org/assignments/media-types/media-types.xhtml)
- [Hypertext Transfer Protocol — HTTP/1.1](https://www.w3.org/Protocols/rfc2616/rfc2616.html)
- [Tipi MIME (tipi media IANA)](/it/docs/Web/HTTP/Guides/MIME_types)
- [Apache vs Nginx: Considerazioni pratiche](https://www.digitalocean.com/community/tutorials/apache-vs-nginx-practical-considerations)
- [Migrare Apache .htaccess a Nginx server block](https://barryvanveen.nl/articles/56-migrate-apache-htaccess-to-nginx-server-block/)
