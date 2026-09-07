---
title: Che cos'è un URL?
slug: Learn_web_development/Howto/Web_mechanics/What_is_a_URL
l10n:
  sourceCommit: 0b9a7f55285ab727f5e14f6d983f2812c70d62d1
---

Questo articolo tratta gli Uniform Resource Locator (URL), spiegando cosa sono e come sono strutturati.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Occorre prima conoscere
        <a href="/it/docs/Learn_web_development/Howto/Web_mechanics/How_does_the_Internet_work"
          >come funziona Internet</a
        >,
        <a href="/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_web_server"
          >che cos'è un server Web</a
        >
        e
        <a href="/it/docs/Learn_web_development/Howto/Web_mechanics/What_are_hyperlinks"
          >i concetti alla base dei link sul Web</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Si apprenderà cos'è un URL e come funziona sul Web.</td>
    </tr>
  </tbody>
</table>

## Riepilogo

Un **URL** (Uniform Resource Locator) è l'indirizzo di una risorsa univoca su Internet. È uno dei meccanismi principali utilizzati dai {{Glossary("Browser", "browser")}} per recuperare risorse pubblicate, come pagine HTML, documenti CSS, immagini e così via.

In teoria, ogni URL valido punta a una risorsa univoca. In pratica, esistono alcune eccezioni; la più comune è un URL che punta a una risorsa non più esistente o che è stata spostata. Poiché la risorsa rappresentata dall'URL e l'URL stesso sono gestiti dal server Web, spetta al proprietario del server Web gestire attentamente quella risorsa e l'URL associato.

## Concetti di base: anatomia di un URL

Ecco alcuni esempi di URL:

```plain
https://developer.mozilla.org
https://developer.mozilla.org/en-US/docs/Learn_web_development/
https://developer.mozilla.org/en-US/search?q=URL
```

Ciascuno di questi URL può essere digitato nella barra degli indirizzi del browser per indicare il caricamento della risorsa associata, che in tutti e tre i casi è una pagina Web.

Un URL è composto da parti diverse, alcune obbligatorie e altre facoltative. Le parti più importanti sono evidenziate nell'URL seguente (i dettagli sono forniti nelle sezioni successive):

![URL completo](mdn-url-all.png)

> [!NOTE]
> Un URL può essere considerato come un normale indirizzo postale: lo _scheme_ rappresenta il servizio postale che si desidera utilizzare, il _domain name_ è la città o il comune e la _port_ è simile al codice postale; il _path_ rappresenta l'edificio in cui deve essere consegnata la posta; i _parameters_ rappresentano informazioni aggiuntive, come il numero dell'appartamento nell'edificio; infine, l'_anchor_ rappresenta la persona effettiva a cui è indirizzata la posta.

> [!NOTE]
> Esistono [alcune parti aggiuntive e alcune regole aggiuntive](https://en.wikipedia.org/wiki/Uniform_Resource_Locator) relative agli URL, ma non sono rilevanti per gli utenti comuni o gli sviluppatori Web. Non è necessario preoccuparsene: non occorre conoscerle per creare e utilizzare URL pienamente funzionali.

## Schema

![Schema](mdn-url-protocol@x2_update.png)

La prima parte dell'URL è lo _scheme_, che indica il protocollo che il browser deve utilizzare per richiedere la risorsa (un protocollo è un metodo stabilito per scambiare o trasferire dati attraverso una rete di computer). Per i siti Web, il protocollo è solitamente HTTPS o HTTP (la sua versione non sicura). L'accesso alle pagine Web richiede uno di questi due protocolli, ma i browser sanno gestire anche altri schemi, come `mailto:` (per aprire un client di posta), quindi non sorprende vedere altri protocolli.

## Autorità

![Autorità](mdn-url-authority.png)

Segue l'_authority_, separata dallo schema dalla sequenza di caratteri `://`. Se presente, l'autorità include sia il _domain_ (ad esempio, `www.example.com`) sia la _port_ (`80`), separati da due punti:

- Il dominio indica quale server Web viene richiesto. Solitamente si tratta di un [nome di dominio](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_domain_name), ma può essere utilizzato anche un {{Glossary("IP_address", "indirizzo IP")}} (anche se è raro, perché molto meno pratico).
- La porta indica il "varco" tecnico utilizzato per accedere alle risorse sul server Web. Solitamente viene omessa se il server Web utilizza le porte standard del protocollo HTTP (80 per HTTP e 443 per HTTPS) per concedere l'accesso alle proprie risorse. In caso contrario, è obbligatoria.

> [!NOTE]
> Il separatore tra lo schema e l'autorità è `://`. I due punti separano lo schema dalla parte successiva dell'URL, mentre `//` indica che la parte successiva dell'URL è l'autorità.
>
> Un esempio di URL che non utilizza un'autorità è il client di posta (`mailto:foobar`). Contiene uno schema ma non utilizza un componente di autorità. Pertanto, i due punti non sono seguiti da due barre e fungono solo da delimitatore tra lo schema e l'indirizzo email.

## Percorso alla risorsa

![Percorso al file](mdn-url-path@x2.png)

`/path/to/myfile.html` è il percorso alla risorsa sul server Web. Agli albori del Web, un percorso come questo rappresentava la posizione fisica di un file sul server Web. Oggi è perlopiù un'astrazione gestita dai server Web senza alcuna realtà fisica.

## Parametri

![Parametri](mdn-url-parameters@x2.png)

`?key1=value1&key2=value2` sono parametri aggiuntivi forniti al server Web. Questi parametri sono un elenco di coppie chiave/valore separate dal simbolo `&`. Il server Web può utilizzare questi parametri per svolgere operazioni aggiuntive prima di restituire la risorsa. Ogni server Web dispone di regole proprie relative ai parametri e l'unico modo affidabile per sapere se uno specifico server Web gestisce i parametri è chiederlo al proprietario del server Web.

## Ancora

![Ancora](mdn-url-anchor@x2.png)

`#SomewhereInTheDocument` è un'ancora verso un'altra parte della risorsa stessa. Un'ancora rappresenta una sorta di "segnalibro" all'interno della risorsa, fornendo al browser le istruzioni per mostrare il contenuto situato in quel punto "segnalato". In un documento HTML, ad esempio, il browser scorrerà fino al punto in cui è definita l'ancora; in un documento video o audio, il browser proverà a raggiungere il momento rappresentato dall'ancora. È importante notare che la parte dopo **#**, nota anche come **identificatore di frammento**, non viene mai inviata al server con la richiesta.

## Come utilizzare gli URL

Qualsiasi URL può essere digitato direttamente nella barra degli indirizzi del browser per raggiungere la risorsa a cui punta. Ma questa è solo la punta dell'iceberg.

Il linguaggio {{Glossary("HTML", "HTML")}} (vedere [Strutturare contenuti con HTML](/it/docs/Learn_web_development/Core/Structuring_content)) utilizza ampiamente gli URL:

- per creare link ad altri documenti con l'elemento {{HTMLElement("a")}};
- per collegare un documento alle relative risorse attraverso vari elementi come {{HTMLElement("link")}} o {{HTMLElement("script")}};
- per visualizzare contenuti multimediali come immagini (con l'elemento {{HTMLElement("img")}}), video (con l'elemento {{HTMLElement("video")}}), suoni e musica (con l'elemento {{HTMLElement("audio")}}) e così via;
- per visualizzare altri documenti HTML con l'elemento {{HTMLElement("iframe")}}.

> [!NOTE]
> Quando si specificano URL per caricare risorse come parte di una pagina, ad esempio quando si utilizzano `<script>`, `<audio>`, `<img>`, `<video>` e simili, in genere si dovrebbero utilizzare soltanto URL HTTP e HTTPS, con poche eccezioni (una rilevante è `data:`; vedere [URL data](/it/docs/Web/URI/Reference/Schemes/data)). L'utilizzo di FTP, ad esempio, non è sicuro e non è più supportato dai browser moderni.

Anche altre tecnologie, come {{Glossary("CSS", "CSS")}} o {{Glossary("JavaScript", "JavaScript")}}, utilizzano ampiamente gli URL, che costituiscono davvero il cuore del Web.

## URL assoluti e URL relativi

Quello visto sopra è chiamato _URL assoluto_, ma esiste anche il concetto di _URL relativo_. Lo [standard URL](https://url.spec.whatwg.org/#absolute-url-string) definisce entrambi, sebbene utilizzi i termini [_stringa URL assoluta_](https://url.spec.whatwg.org/#absolute-url-string) e [_stringa URL relativa_](https://url.spec.whatwg.org/#relative-url-string), per distinguerli dagli [oggetti URL](https://url.spec.whatwg.org/#url) (che sono rappresentazioni in memoria degli URL).

Esaminiamo cosa significhi la distinzione tra _assoluto_ e _relativo_ nel contesto degli URL.

Le parti obbligatorie di un URL dipendono in larga misura dal contesto in cui l'URL viene utilizzato. Nella barra degli indirizzi del browser, un URL non ha alcun contesto, quindi occorre fornire un URL completo (o _assoluto_), come quelli visti sopra. Non è necessario includere il protocollo (il browser utilizza HTTP per impostazione predefinita) né la porta (necessaria solo quando il server Web di destinazione utilizza una porta insolita), ma tutte le altre parti dell'URL sono necessarie.

Quando un URL viene utilizzato all'interno di un documento, come in una pagina HTML, la situazione è leggermente diversa. Poiché il browser conosce già l'URL del documento, può utilizzare queste informazioni per completare le parti mancanti di qualsiasi URL disponibile all'interno di quel documento. È possibile distinguere tra un _URL assoluto_ e un _URL relativo_ osservando solo la parte _path_ dell'URL. Se la parte del percorso dell'URL inizia con il carattere `/`, il browser recupererà quella risorsa dalla radice principale del server, senza fare riferimento al contesto fornito dal documento corrente.

Vediamo alcuni esempi per chiarire meglio. Si supponga che gli URL siano definiti all'interno del documento situato al seguente URL: `https://developer.mozilla.org/it/docs/Learn_web_development`.

`https://developer.mozilla.org/it/docs/Learn_web_development` è a sua volta un URL assoluto. Dispone di tutte le parti necessarie per individuare la risorsa a cui punta.

Tutti gli URL seguenti sono URL relativi:

- URL relativo allo schema: `//developer.mozilla.org/it/docs/Learn_web_development` — manca solo il protocollo. Il browser utilizzerà lo stesso protocollo utilizzato per caricare il documento che ospita quell'URL.
- URL relativo al dominio: `/it/docs/Learn_web_development` — mancano sia il protocollo sia il nome di dominio. Il browser utilizzerà lo stesso protocollo e lo stesso nome di dominio utilizzati per caricare il documento che ospita quell'URL.
- Sottorisorse: `Howto/Web_mechanics/What_is_a_URL` — mancano il protocollo e il nome di dominio e il percorso non inizia con `/`. Il browser tenterà di trovare il documento in una sottodirectory di quella che contiene la risorsa corrente. In questo caso, si desidera effettivamente raggiungere questo URL: `https://developer.mozilla.org/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_URL`.
- Risalita nell'albero delle directory: `../Web/CSS/Reference` — mancano il protocollo e il nome di dominio e il percorso inizia con `..`. Questo comportamento è ereditato dal mondo dei file system UNIX: indica al browser di risalire di un livello. Qui si desidera raggiungere questo URL: `https://developer.mozilla.org/it/docs/Learn_web_development/../Web/CSS/Reference`, che può essere semplificato in: `https://developer.mozilla.org/it/docs/Web/CSS/Reference`.
- Solo ancora: `#semantic_urls` - tutte le parti sono assenti tranne l'ancora. Il browser utilizzerà l'URL del documento corrente e sostituirà o aggiungerà a esso la parte relativa all'ancora. Questo è utile quando si desidera creare un link a una parte specifica del documento corrente.

## Nomi utente e password negli URL

Meno comuni delle parti dell'URL discusse sopra, negli URL possono comparire un nome utente e una password.

Ad esempio:

```plain
https://username:password@www.example.com:80/
```

Quando presenti, il nome utente e la password sono inseriti tra i caratteri `://` e l'autorità, con due punti tra i due e una chiocciola (`@`) alla fine.

Un nome utente e una password possono essere inclusi nell'URL quando si accede a siti Web che utilizzano il meccanismo di sicurezza dell'[autenticazione HTTP](/it/docs/Web/HTTP/Guides/Authentication), per accedere immediatamente a un sito Web ed evitare la finestra di dialogo nome utente/password che altrimenti verrebbe visualizzata per inserire le credenziali.

Sebbene questo meccanismo possa ancora essere utilizzato, è deprecato per motivi di sicurezza e i siti Web moderni tendono a usare altri meccanismi per l'autenticazione. Per ulteriori dettagli, vedere [Accesso mediante credenziali nell'URL](/it/docs/Web/HTTP/Guides/Authentication#access_using_credentials_in_the_url).

## URL semantici

Nonostante il loro carattere molto tecnico, gli URL rappresentano un punto di accesso a un sito Web leggibile dalle persone. Possono essere memorizzati e chiunque può inserirli nella barra degli indirizzi di un browser. Le persone sono al centro del Web e pertanto è considerata una buona pratica creare i cosiddetti [_URL semantici_](https://en.wikipedia.org/wiki/Semantic_URL). Gli URL semantici utilizzano parole dal significato intrinseco che possono essere comprese da chiunque, indipendentemente dalle conoscenze tecniche.

La semantica linguistica è naturalmente irrilevante per i computer. Probabilmente sono stati visti spesso URL che sembrano combinazioni di caratteri casuali. Tuttavia, la creazione di URL leggibili dalle persone offre molti vantaggi:

- Sono più facili da manipolare.
- Rendono più chiaro agli utenti dove si trovano, cosa stanno facendo, cosa stanno leggendo o con cosa stanno interagendo sul Web.
- Alcuni motori di ricerca possono utilizzare questa semantica per migliorare la classificazione delle pagine associate.

## Vedere anche

[URL data](/it/docs/Web/URI/Reference/Schemes/data): URL con prefisso dello schema `data:`, che consentono ai creatori di contenuti di incorporare piccoli file in linea nei documenti.
