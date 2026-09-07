---
title: Configurazione corretta dei tipi MIME del server
short-title: Configurazione dei tipi MIME del server
slug: Learn_web_development/Extensions/Server-side/Configuring_server_MIME_types
l10n:
  sourceCommit: f85d2e26b062decf7a2bb9179c3a93003f4067a9
---

I tipi MIME descrivono il tipo di contenuto multimediale, sia nelle email sia quando viene servito da server web o applicazioni web. Il loro scopo è fornire un'indicazione su come il contenuto debba essere elaborato e visualizzato.

Esempi di tipi MIME:

- `text/html` per documenti HTML.
- `text/plain` per testo semplice.
- `text/css` per Cascading Style Sheets.
- `text/javascript` per file JavaScript.
- `text/markdown` per file Markdown.
- `application/octet-stream` per file binari per i quali è prevista un'azione dell'utente.

Le configurazioni predefinite dei server variano notevolmente e impostano valori di tipo MIME _predefiniti_ diversi per i file privi di un tipo di contenuto definito.

Le versioni del server web Apache **precedenti alla 2.2.7** erano configurate per riportare un tipo MIME `text/plain` o `application/octet-stream` per i tipi di contenuto sconosciuti. Le versioni moderne di Apache riportano `none` per i file con tipi di contenuto sconosciuti.

[Nginx](https://nginx.org/) riporterà `text/plain` se non viene definito un tipo di contenuto predefinito.

Quando nuovi tipi di contenuto vengono inventati o aggiunti ai server web, gli amministratori web potrebbero non aggiungere i nuovi tipi MIME alla configurazione del proprio server web. Questa è una delle principali fonti di problemi per gli utenti di browser che rispettano i tipi MIME riportati dai server web e dalle applicazioni.

## Perché i tipi MIME corretti sono importanti?

Se un server web o un'applicazione riporta un tipo MIME non corretto per il contenuto, incluso un "tipo predefinito" per contenuti sconosciuti, un browser web non ha modo di conoscere le intenzioni dell'autore. Ciò può causare comportamenti imprevisti.

Alcuni browser web potrebbero tentare di _indovinare_ il tipo MIME corretto. Questo consente a server web e applicazioni configurati in modo errato di continuare a funzionare in quei browser, ma non in altri browser che implementano correttamente lo standard. Oltre a violare la specifica HTTP, questa è una cattiva idea per un paio di altre ragioni importanti:

- Perdita di controllo
  - : Se il browser ignora il tipo MIME riportato, gli amministratori web e gli autori non hanno più il controllo su come il loro contenuto debba essere elaborato.

    Ad esempio, un sito web rivolto agli sviluppatori web potrebbe voler inviare alcuni documenti HTML di esempio come `text/html` o `text/plain`, affinché i documenti vengano rispettivamente elaborati e visualizzati come HTML oppure come codice sorgente. Se il browser indovina il tipo MIME, questa opzione non è più disponibile per l'autore.

- Sicurezza
  - : Alcuni tipi di contenuto, come i programmi eseguibili, sono intrinsecamente non sicuri. Per questo motivo, questi tipi MIME sono generalmente limitati rispetto alle azioni che un browser web eseguirà quando riceve contenuti di quel tipo. Un programma eseguibile non dovrebbe essere eseguito sul computer dell'utente e dovrebbe almeno far apparire una finestra di dialogo che **chieda all'utente** se desidera scaricare il file.

## Tipi MIME JavaScript legacy

Cercando informazioni sui tipi MIME JavaScript, potrebbero comparire diversi tipi MIME che fanno riferimento a JavaScript. Alcuni di questi tipi MIME includono:

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

Sebbene i browser possano supportare uno, alcuni o tutti questi tipi MIME alternativi, si dovrebbe usare **solo** `text/javascript` per indicare il tipo MIME dei file JavaScript.

> [!NOTE]
> Vedere [Tipi MIME (tipi di media IANA)](/it/docs/Web/HTTP/Guides/MIME_types) per ulteriori informazioni.

## Come determinare il tipo MIME da impostare

Esistono diversi modi per determinare il valore corretto del tipo MIME da usare per servire il contenuto.

- Se il contenuto è stato creato utilizzando software commerciale, leggere la documentazione del fornitore per verificare quali tipi MIME dovrebbero essere riportati dall'applicazione.
- Consultare il [registro dei tipi di media MIME](https://www.iana.org/assignments/media-types/media-types.xhtml) di IANA, che contiene informazioni su tutti i tipi MIME registrati.
- Cercare l'estensione del file in [FILExt](https://filext.com/) o nel [riferimento delle estensioni dei file](https://www.file-extensions.org/) per vedere quali tipi MIME sono associati a tale estensione. Prestare particolare attenzione, poiché l'applicazione potrebbe avere più tipi MIME che differiscono soltanto per una lettera.

## Come verificare il tipo MIME del contenuto ricevuto

- In Firefox
  - Caricare il file e passare a **Strumenti > Informazioni sulla pagina** per ottenere il tipo di contenuto della pagina a cui si è avuto accesso.
  - È inoltre possibile passare a **Strumenti > Sviluppo web > Rete** e ricaricare la pagina. La scheda delle richieste fornisce un elenco di tutte le risorse caricate dalla pagina. Facendo clic su una risorsa vengono elencate tutte le informazioni disponibili, incluso l'header [`Content-Type`](/it/docs/Web/HTTP/Reference/Headers/Content-Type) della pagina.

- In Chrome
  - Caricare il file e passare a **Visualizza > Sviluppatore > Strumenti per sviluppatori**, quindi scegliere la scheda _Network_. Ricaricare la pagina e selezionare la risorsa che si desidera ispezionare. Nella sezione degli header, cercare `Content-Type`: verrà riportato il tipo di contenuto della risorsa.

- Cercare nel sorgente della pagina un elemento `<meta>` che indichi il tipo MIME, ad esempio `<meta http-equiv="Content-Type" content="text/html">`.
  - Secondo gli standard, l'elemento `<meta>` che specifica il tipo MIME dovrebbe essere ignorato se è disponibile un header Content-Type.

[IANA](https://www.iana.org/) mantiene un elenco dei [tipi di media MIME](https://www.iana.org/assignments/media-types/media-types.xhtml) registrati. La [specifica HTTP](https://www.w3.org/Protocols/rfc2616/rfc2616.html) definisce un superset di tipi MIME, usato per descrivere i tipi di media utilizzati sul web.

## Come configurare il server per inviare i tipi MIME corretti

L'obiettivo è configurare il server affinché invii l'header {{HTTPHeader("Content-Type")}} corretto per ciascun documento.

- Se viene utilizzato il server web Apache, consultare la sezione **_Tipi di media e codifiche dei caratteri_** di [Configurazione Apache: .htaccess](/it/docs/Learn_web_development/Extensions/Server-side/Apache_Configuration_htaccess) per esempi di diversi tipi di documenti e dei relativi tipi MIME.
- Se viene utilizzato Nginx, tenere presente che Nginx non dispone di uno strumento equivalente a `.htaccess`, pertanto tutte le modifiche andranno nel file di configurazione principale.
- Se viene utilizzato uno script o un framework lato server per generare contenuto, il modo di indicare il tipo di contenuto dipenderà dallo strumento usato. Consultare la documentazione del framework o della libreria.

Indipendentemente dal sistema server utilizzato, l'effetto da ottenere è impostare un header di risposta denominato {{httpheader("Content-Type")}}, seguito da due punti e uno spazio, quindi da un tipo MIME. Gli ambienti di alto livello consentono spesso di impostare tali header durante la generazione della pagina. Ad esempio, in un ambiente PHP, l'header di risposta per le risorse PDF potrebbe essere impostato in questo modo:

```php
header('Content-Type: application/pdf')
```

Tentare invece di impostarlo soltanto con `header('application/pdf')` non funzionerà.

## Link correlati

- [IANA | Tipi di media MIME](https://www.iana.org/assignments/media-types/media-types.xhtml)
- [Hypertext Transfer Protocol — HTTP/1.1](https://www.w3.org/Protocols/rfc2616/rfc2616.html)
- [Tipi MIME (tipi di media IANA)](/it/docs/Web/HTTP/Guides/MIME_types)
- [Apache vs Nginx: considerazioni pratiche](https://www.digitalocean.com/community/tutorials/apache-vs-nginx-practical-considerations)
- [Migrare Apache .htaccess a un server block Nginx](https://barryvanveen.nl/articles/56-migrate-apache-htaccess-to-nginx-server-block/)
