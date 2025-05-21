---
title: Cos'è un URL?
slug: Learn_web_development/Howto/Web_mechanics/What_is_a_URL
l10n:
  sourceCommit: 324d86d51118c339d4ad7afa973e38fb64f6564e
---

Questo articolo discute gli Uniform Resource Locators (URL), spiegando cosa sono e come sono strutturati.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        È necessario prima sapere
        <a href="/it/docs/Learn_web_development/Howto/Web_mechanics/How_does_the_Internet_work"
          >come funziona Internet</a
        >,
        <a href="/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_web_server"
          >che cos'è un server Web</a
        >
        e
        <a href="/it/docs/Learn_web_development/Howto/Web_mechanics/What_are_hyperlinks"
          >i concetti dietro i collegamenti sul web</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Imparerai che cos'è un URL e come funziona sul Web.</td>
    </tr>
  </tbody>
</table>

## Sommario

Un **URL** (Uniform Resource Locator) è l'indirizzo di una risorsa unica su internet. È uno dei meccanismi chiave utilizzati dai {{Glossary("Browser", "browser")}} per recuperare risorse pubblicate, come pagine HTML, documenti CSS, immagini, e così via.

In teoria, ogni URL valido punta a una risorsa unica. In pratica, ci sono alcune eccezioni, la più comune è un URL che punta a una risorsa che non esiste più o che è stata spostata. Dato che la risorsa rappresentata dall'URL e l'URL stesso sono gestiti dal server Web, spetta al proprietario del server web gestire con cura quella risorsa e il relativo URL.

## Nozioni di base: l'anatomia di un URL

Ecco alcuni esempi di URL:

```plain
https://developer.mozilla.org
https://developer.mozilla.org/en-US/docs/Learn_web_development/
https://developer.mozilla.org/en-US/search?q=URL
```

Qualsiasi di questi URL può essere digitato nella barra degli indirizzi del tuo browser per indicargli di caricare la risorsa associata, che in tutti e tre i casi è una pagina Web.

Un URL è composto da diverse parti, alcune obbligatorie e altre opzionali. Le parti più importanti sono evidenziate nell'URL qui sotto (i dettagli sono forniti nelle sezioni seguenti):

![full URL](mdn-url-all.png)

> [!NOTE]
> Potresti pensare a un URL come un indirizzo postale tradizionale: lo _schema_ rappresenta il servizio postale che vuoi utilizzare, il _nome del dominio_ è la città o il paese, e la _porta_ è come il CAP; il _percorso_ rappresenta l'edificio dove dovrebbe essere consegnata la tua mail; i _parametri_ rappresentano informazioni extra come il numero dell'appartamento nell'edificio; e, infine, l'_ancora_ rappresenta la persona a cui hai indirizzato la tua mail.

> [!NOTE]
> Ci sono [alcune parti extra e alcune regole extra](https://en.wikipedia.org/wiki/Uniform_Resource_Locator) riguardanti gli URL, ma non sono rilevanti per utenti regolari o sviluppatori Web. Non preoccuparti di questo, non hai bisogno di conoscerle per costruire e usare URL pienamente funzionali.

## Schema

![Schema](mdn-url-protocol@x2_update.png)

La prima parte dell'URL è lo _schema_, che indica il protocollo che il browser deve utilizzare per richiedere la risorsa (un protocollo è un metodo stabilito per scambiare o trasferire dati su una rete informatica). Di solito per i siti web il protocollo è HTTPS o HTTP (la sua versione non sicura). Per indirizzare le pagine web è necessario uno di questi due, ma i browser sanno come gestire anche altri schemi come `mailto:` (per aprire un client di posta), quindi non sorprenderti se vedi altri protocolli.

## Autorità

![Autorità](mdn-url-authority.png)

Segue poi l'_autorità_, che è separata dallo schema dal carattere `://`. Se presente, l'autorità include sia il _dominio_ (es. `www.example.com`) sia la _porta_ (`80`), separati da un due punti:

- Il dominio indica quale server Web viene richiesto. Di solito si tratta di un [nome di dominio](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_domain_name), ma può essere utilizzato anche un {{Glossary("IP_address", "indirizzo IP")}} (anche se è raro perché è molto meno conveniente).
- La porta indica il "gate" tecnico usato per accedere alle risorse sul server web. Di solito viene omessa se il server web utilizza le porte standard del protocollo HTTP (80 per HTTP e 443 per HTTPS) per garantire l'accesso alle sue risorse. Altrimenti è obbligatoria.

> [!NOTE]
> Il separatore tra schema e autorità è `://`. I due punti separano lo schema dalla parte successiva dell'URL, mentre `//` indica che la parte successiva dell'URL è l'autorità.
>
> Un esempio di URL che non utilizza un'autorità è il client di posta (`mailto:foobar`). Contiene uno schema ma non utilizza un componente di autorità. Pertanto, i due punti non sono seguiti da due barre e funzionano solo come delimitatore tra lo schema e l'indirizzo email.

## Percorso alla risorsa

![Percorso al file](mdn-url-path@x2.png)

`/path/to/myfile.html` è il percorso alla risorsa sul server Web. Nei primi giorni del Web, un percorso come questo rappresentava una posizione fisica del file sul server Web. Al giorno d'oggi, è per lo più un'astrazione gestita dai server Web senza alcuna realtà fisica.

## Parametri

![Parametri](mdn-url-parameters@x2.png)

`?key1=value1&key2=value2` sono parametri extra forniti al server Web. Questi parametri sono una lista di coppie chiave/valore separate con il simbolo `&`. Il server Web può utilizzare questi parametri per svolgere operazioni aggiuntive prima di restituire la risorsa. Ogni server Web ha le proprie regole riguardo ai parametri, e l'unico modo affidabile per sapere se un determinato server Web gestisce i parametri è chiedere al proprietario del server Web.

## Ancora

![Ancora](mdn-url-anchor@x2.png)

`#SomewhereInTheDocument` è un'ancora ad un'altra parte della risorsa stessa. Un'ancora rappresenta una sorta di "segnalibro" all'interno della risorsa, fornendo al browser le indicazioni per mostrare il contenuto situato in quel punto "segnalato". Su un documento HTML, ad esempio, il browser scorrerà fino al punto in cui l'ancora è definita; su un documento video o audio, il browser cercherà di andare al tempo rappresentativo dell'ancora. Vale la pena notare che la parte dopo il **#**, nota anche come **fragment identifier**, non viene mai inviata al server con la richiesta.

## Come usare gli URL

Qualsiasi URL può essere digitato direttamente nella barra degli indirizzi del browser per accedere alla risorsa dietro di esso. Ma questo è solo la punta dell'iceberg!

Il linguaggio {{Glossary("HTML", "HTML")}} (vedi [Strutturare i contenuti con HTML](/it/docs/Learn_web_development/Core/Structuring_content)) fa ampio uso degli URL:

- per creare collegamenti ad altri documenti con l'elemento {{HTMLElement("a")}};
- per collegare un documento con le sue risorse correlate attraverso vari elementi come {{HTMLElement("link")}} o {{HTMLElement("script")}};
- per visualizzare media come immagini (con l'elemento {{HTMLElement("img")}}), video (con l'elemento {{HTMLElement("video")}}), suoni e musica (con l'elemento {{HTMLElement("audio")}}), ecc.;
- per visualizzare altri documenti HTML con l'elemento {{HTMLElement("iframe")}}.

> [!NOTE]
> Quando si specificano URL per caricare risorse come parte di una pagina (come quando si utilizza `<script>`, `<audio>`, `<img>`, `<video>`, e simili), si dovrebbe generalmente utilizzare solo URL HTTP e HTTPS, con poche eccezioni (una notevole è `data:`; vedi [Data URLs](/it/docs/Web/URI/Reference/Schemes/data)). Usare FTP, ad esempio, non è sicuro e non è più supportato dai browser moderni.

Altre tecnologie, come {{Glossary("CSS", "CSS")}} o {{Glossary("JavaScript", "JavaScript")}}, utilizzano ampiamente gli URL, e questi sono davvero il cuore del Web.

## URL assoluti vs. URL relativi

Ciò che abbiamo visto sopra è chiamato URL _assoluto_, ma c'è anche qualcosa chiamato URL _relativo_. Lo [standard URL](https://url.spec.whatwg.org/#absolute-url-string) definisce entrambi — anche se utilizza i termini [_stringa URL assoluta_](https://url.spec.whatwg.org/#absolute-url-string) e [_stringa URL relativa_](https://url.spec.whatwg.org/#relative-url-string), per distinguerli dagli [oggetti URL](https://url.spec.whatwg.org/#url) (che sono rappresentazioni in memoria di URL).

Esaminiamo cosa significa la distinzione tra _assoluto_ e _relativo_ nel contesto degli URL.

Le parti richieste di un URL dipendono in larga misura dal contesto in cui l'URL viene utilizzato. Nella barra degli indirizzi del tuo browser, un URL non ha alcun contesto, quindi devi fornire un URL completo (o _assoluto_), come quelli che abbiamo visto sopra. Non è necessario includere il protocollo (il browser utilizza HTTP per impostazione predefinita) o la porta (che è necessaria solo quando il server Web di destinazione utilizza una porta insolita), ma tutte le altre parti dell'URL sono necessarie.

Quando un URL viene utilizzato all'interno di un documento, come in una pagina HTML, le cose sono un po' diverse. Poiché il browser ha già l'URL del documento stesso, può utilizzare queste informazioni per riempire le parti mancanti di qualsiasi URL disponibile all'interno di quel documento. Possiamo differenziare tra un URL _assoluto_ e un URL _relativo_ guardando solo la parte del _percorso_ dell'URL. Se la parte del percorso dell'URL inizia con il carattere `/`, il browser recupererà quella risorsa dalla radice del server, senza fare riferimento al contesto fornito dal documento corrente.

Osserviamo alcuni esempi per chiarire questo concetto. Supponiamo che gli URL siano definiti all'interno del documento situato all'URL seguente: `https://developer.mozilla.org/it/docs/Learn_web_development`.

`https://developer.mozilla.org/it/docs/Learn_web_development` stesso è un URL assoluto. Ha tutte le parti necessarie per localizzare la risorsa a cui punta.

Tutti i seguenti URL sono URL relativi:

- URL relativo allo schema: `//developer.mozilla.org/it/docs/Learn_web_development` — manca solo il protocollo. Il browser utilizzerà lo stesso protocollo usato per caricare il documento che ospita quell'URL.
- URL relativo al dominio: `/it/docs/Learn_web_development` — mancano il protocollo e il nome del dominio. Il browser utilizzerà lo stesso protocollo e lo stesso nome del dominio usati per caricare il documento che ospita quell'URL.
- Sotto-risorse: `Howto/Web_mechanics/What_is_a_URL` — mancano il protocollo e il nome del dominio, e il percorso non inizia con `/`. Il browser tenterà di trovare il documento in una sottodirectory di quella contenente la risorsa attuale. In questo caso, vogliamo davvero raggiungere questo URL: `https://developer.mozilla.org/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_URL`.
- Tornare indietro nella struttura delle directory: `../CSS/display` — mancano il protocollo e il nome del dominio, e il percorso inizia con `..`. Questo è ereditato dal mondo dei file system UNIX — per indicare al browser che vogliamo tornare indietro di un livello. Qui vogliamo raggiungere questo URL: `https://developer.mozilla.org/it/docs/Learn_web_development/../Web/CSS/display`, che può essere semplificato in: `https://developer.mozilla.org/it/docs/Web/CSS/display`.
- Solo ancora: `#semantic_urls` - tutte le parti mancano tranne l'ancora. Il browser utilizzerà l'URL del documento corrente e sostituirà o aggiungerà la parte dell'ancora ad esso. Questo è utile quando si desidera collegarsi a una parte specifica del documento corrente.

## Nomi utente e password negli URL

Meno comuni rispetto alle parti dell'URL discusse sopra, potresti vedere un nome utente e una password inclusi negli URL.

Ad esempio:

```plain
https://username:password@www.example.com:80/
```

Quando sono inclusi, il nome utente e la password vengono inseriti tra i caratteri `://` e l'autorità, con un due punti tra i due e una chiocciola (`@`) alla fine.

Un nome utente e una password possono essere inclusi nell'URL quando si accede a siti web che utilizzano il meccanismo di sicurezza [autenticazione HTTP](/it/docs/Web/HTTP/Guides/Authentication), per accedere immediatamente a un sito web e bypassare la finestra di dialogo nome utente/password che altrimenti apparirebbe per inserire le tue credenziali.

Sebbene tu possa ancora vedere questo meccanismo utilizzato in giro, è deprecato a causa di preoccupazioni sulla sicurezza, e i siti web moderni tendono a utilizzare altri meccanismi per l'autenticazione. Vedi [Accesso utilizzando le credenziali nell'URL](/it/docs/Web/HTTP/Guides/Authentication#access_using_credentials_in_the_url) per ulteriori dettagli.

## URL semantici

Nonostante il loro sapore molto tecnico, gli URL rappresentano un punto di ingresso leggibile dall'uomo per un sito web. Possono essere memorizzati, e chiunque può digitarli nella barra degli indirizzi di un browser. Le persone sono al centro del Web, e quindi è considerata una buona pratica costruire quelli che sono chiamati [_URL semantici_](https://en.wikipedia.org/wiki/Semantic_URL). Gli URL semantici utilizzano parole con significato intrinseco che possono essere comprese da chiunque, indipendentemente dalla loro conoscenza tecnica.

La semantica linguistica è ovviamente irrilevante per i computer. Probabilmente hai spesso visto URL che sembrano assemblaggi di caratteri casuali. Ma ci sono molti vantaggi nel creare URL leggibili dall'uomo:

- È più facile per te manipolarli.
- Chiarisce le cose per gli utenti in termini di dove si trovano, cosa stanno facendo, cosa stanno leggendo o con cosa stanno interagendo sul Web.
- Alcuni motori di ricerca possono usare quella semantica per migliorare la classificazione delle pagine associate.

## Vedi anche

[Data URLs](/it/docs/Web/URI/Reference/Schemes/data): URL prefissati con lo schema `data:`, permettono ai creatori di contenuti di incorporare piccoli file direttamente nei documenti.
