---
title: Configurazione corretta dei tipi MIME del server
slug: Learn_web_development/Extensions/Server-side/Configuring_server_MIME_types
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

I tipi MIME descrivono il tipo di media del contenuto, sia nelle email, sia serviti dai server web o dalle applicazioni web. Sono intesi per aiutare a fornire un'indicazione su come il contenuto dovrebbe essere elaborato e visualizzato.

Esempi di tipi MIME:

- `text/html` per documenti HTML.
- `text/plain` per testo semplice.
- `text/css` per Fogli di Stile a Cascata.
- `text/javascript` per file JavaScript.
- `text/markdown` per file Markdown.
- `application/octet-stream` per file binari dove è attesa un'azione dell'utente.

Le configurazioni predefinite del server variano notevolmente e impostano diversi valori di tipi MIME _predefiniti_ per i file senza un tipo di contenuto definito.

Versioni del Server Web Apache **prima della 2.2.7** erano configurate per riportare un tipo MIME di `text/plain` o `application/octet-stream` per tipi di contenuto sconosciuti. Le versioni moderne di Apache riportano `none` per i file con tipi di contenuto sconosciuti.

[Nginx](https://nginx.org/) riporterà `text/plain` se non viene definito un tipo di contenuto predefinito.

Man mano che vengono inventati o aggiunti nuovi tipi di contenuto ai server web, gli amministratori web possono non riuscire ad aggiungere i nuovi tipi MIME alla configurazione del loro server web. Questo è una fonte principale di problemi per gli utenti dei browser che rispettano i tipi MIME riportati dai server web e dalle applicazioni.

## Perché i tipi MIME corretti sono importanti?

Se un server web o un'applicazione riporta un tipo MIME errato per il contenuto (incluso un "tipo predefinito" per il contenuto sconosciuto), un browser web non ha modo di conoscere le intenzioni dell'autore. Questo può causare comportamenti inaspettati.

Alcuni browser web possono cercare di _indovinare_ il tipo MIME corretto. Questo consente ai server e alle applicazioni web configurati in modo errato di continuare a funzionare per quei browser (ma non per altri browser che implementano correttamente lo standard). Oltre a violare la specifica HTTP, questa è una pessima idea per un paio di altre ragioni significative:

- Perdita di controllo

  - : Se il browser ignora il tipo MIME riportato, gli amministratori web e gli autori non hanno più alcun controllo su come il loro contenuto deve essere elaborato.

    Ad esempio, un sito web orientato agli sviluppatori web potrebbe desiderare di inviare alcuni documenti HTML di esempio come `text/html` o `text/plain` per far sì che i documenti siano elaborati e visualizzati come HTML o come codice sorgente. Se il browser indovina il tipo MIME, questa opzione non è più disponibile per l'autore.

- Sicurezza

  - : Alcuni tipi di contenuto, come i programmi eseguibili, sono intrinsecamente non sicuri. Per questo motivo, questi tipi MIME sono solitamente limitati riguardo le azioni che un browser web intraprenderà quando riceve quel tipo di contenuto. Un programma eseguibile non dovrebbe essere eseguito sul computer dell'utente e dovrebbe almeno causare l'apertura di una finestra di dialogo **chiedendo all'utente** se desidera scaricare il file.

## Tipi MIME legacy per JavaScript

Quando si cerca informazioni sui tipi MIME di JavaScript, si potrebbero vedere diversi tipi MIME che fanno riferimento a JavaScript. Alcuni di questi tipi MIME includono:

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

Anche se i browser possono supportare qualsiasi, alcuni o tutti questi tipi MIME alternativi, dovrebbe essere usato **solo** `text/javascript` per indicare il tipo MIME dei file JavaScript.

> [!NOTE]
> Vedere [Tipi MIME (tipi media IANA)](/it/docs/Web/HTTP/Guides/MIME_types) per ulteriori informazioni.

## Come determinare il tipo MIME da impostare

Ci sono diversi modi per determinare il valore del tipo MIME corretto da utilizzare per servire il proprio contenuto.

- Se il suo contenuto è stato creato utilizzando software commerciale, legga la documentazione del fornitore per vedere quali tipi MIME devono essere riportati per l'applicazione.
- Controlli il [registro dei Tipi Media MIME di IANA](https://www.iana.org/assignments/media-types/media-types.xhtml), che contiene informazioni su tutti i tipi MIME registrati.
- Cerchi l'estensione del file in [FILExt](https://filext.com/) o il [Riferimento alle estensioni dei file](https://www.file-extensions.org/) per vedere quali tipi MIME sono associati a quell'estensione. Prestare molta attenzione poiché l'applicazione può avere più tipi MIME che differiscono solo per una lettera.

## Come controllare il tipo MIME del contenuto ricevuto

- In Firefox

  - Carichi il file e vada su **Strumenti > Informazioni pagina** per ottenere il tipo di contenuto per la pagina a cui ha avuto accesso.
  - Può anche andare su **Strumenti > Sviluppo web > Rete** e ricaricare la pagina. La scheda delle richieste le fornirà un elenco di tutte le risorse caricate dalla pagina. Cliccando su qualsiasi risorsa verranno elencate tutte le informazioni disponibili, incluso l'intestazione [`Content-Type`](/it/docs/Web/HTTP/Reference/Headers/Content-Type) della pagina.

- In Chrome

  - Carichi il file e vada su **Visualizza > Sviluppatore > Strumenti per sviluppatori** e scelga la scheda _Rete_. Ricarichi la pagina e selezioni la risorsa che desidera ispezionare. Sotto le intestazioni, cerchi `Content-Type` e verrà riportato il tipo di contenuto della risorsa.

- Cerchi un elemento `<meta>` nel sorgente della pagina che fornisce il tipo MIME, ad esempio `<meta http-equiv="Content-Type" content="text/html">`.

  - Secondo gli standard, l'elemento `<meta>` che specifica il tipo MIME dovrebbe essere ignorato se è disponibile un'intestazione Content-Type.

[IANA](https://www.iana.org/) mantiene un elenco di [Tipi Media MIME registrati](https://www.iana.org/assignments/media-types/media-types.xhtml). La [specifica HTTP](https://www.w3.org/Protocols/rfc2616/rfc2616.html) definisce un superstite dei tipi MIME, utilizzato per descrivere i tipi di media utilizzati sul web.

## Come configurare il proprio server per inviare i tipi MIME corretti

L'obiettivo è configurare il proprio server in modo da inviare l'intestazione {{HTTPHeader("Content-Type")}} corretta per ciascun documento.

- Se si utilizza il server web Apache, consultare la sezione **_Tipi di Media e Codifiche dei Caratteri_** di [Configurazione Apache: .htaccess](/it/docs/Learn_web_development/Extensions/Server-side/Apache_Configuration_htaccess) per esempi di diversi tipi di documenti e dei relativi tipi MIME.
- Se si utilizza Nginx, si noti che Nginx non ha uno strumento equivalente a `.htaccess`, quindi tutte le modifiche verranno effettuate nel file di configurazione principale.
- Se si utilizza uno script lato server o un framework per generare contenuti, il modo per indicare il tipo di contenuto dipenderà dallo strumento utilizzato. Consultare la documentazione del framework o della libreria.

Indipendentemente dal sistema server che si utilizza, l'effetto che si deve ottenere è impostare un'intestazione di risposta con il nome {{httpheader("Content-Type")}}, seguito da due punti e uno spazio, seguito da un tipo MIME. Gli ambienti ad alto livello spesso consentono di impostare tali intestazioni durante la generazione della pagina. Ad esempio, in un ambiente PHP, potrebbe impostare l'intestazione di risposta per le risorse PDF in questo modo:

```php
header('Content-Type: application/pdf')
```

Cercare di impostarlo invece solo con `header('application/pdf')` non funzionerà.

## Link correlati

- [IANA | MIME Media Types](https://www.iana.org/assignments/media-types/media-types.xhtml)
- [Protocollo di Trasferimento Ipertestuale — HTTP/1.1](https://www.w3.org/Protocols/rfc2616/rfc2616.html)
- [Tipi MIME (tipi media IANA)](/it/docs/Web/HTTP/Guides/MIME_types)
- [Apache vs Nginx: Considerazioni pratiche](https://www.digitalocean.com/community/tutorials/apache-vs-nginx-practical-considerations)
- [Migrare .htaccess Apache a blocco server Nginx](https://barryvanveen.nl/articles/56-migrate-apache-htaccess-to-nginx-server-block/)
