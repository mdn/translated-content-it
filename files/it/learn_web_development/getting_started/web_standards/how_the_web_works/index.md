---
title: Come funziona il web
slug: Learn_web_development/Getting_started/Web_standards/How_the_web_works
l10n:
  sourceCommit: b5ee197a87ea18acbc4dd9544efa8c0e46253785
---

{{NextMenu("Learn_web_development/Getting_started/Web_standards/The_web_standards_model", "Learn_web_development/Getting_started/Web_standards")}}

_Come funziona il web_ fornisce una descrizione di alto livello di ciò che accade quando si usa un browser web per navigare verso una pagina web, spiegando la magia che avviene dietro le quinte per consegnare il codice pertinente al computer affinché il browser lo assembli in qualcosa che è possibile visualizzare.

Questa teoria non è essenziale per scrivere codice web nel breve termine, ma ben presto sarà davvero utile comprendere cosa accade in background.

> [!NOTE]
> Questo articolo non tratta il modo in cui i browser web eseguono effettivamente il rendering del codice in pagine web. Questo argomento è trattato in [Come i browser caricano i siti web](/it/docs/Learn_web_development/Getting_started/Web_standards/How_browsers_load_websites).

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con il sistema operativo del computer, i browser web e le tecnologie web.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Client e server e i loro ruoli nel web.</li>
          <li>DNS e il suo funzionamento, a livello generale.</li>
          <li>Lo scopo di TCP/IP, HTTP e dei pacchetti.</li>
          <li>Sintassi HTTP a livello di base.</li>
          <li>Codici di risposta HTTP comuni (ad esempio, 200, 301, 403, 404 e 500).</li>
          <li>Componenti di base di un URL (protocollo, dominio, sottodominio, percorso).</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Client e server

I computer connessi a Internet sono chiamati **client** e **server**. Un diagramma semplificato del modo in cui interagiscono potrebbe essere simile a questo:

![Due cerchi che rappresentano client e server. Una freccia con l'etichetta request va dal client al server e una freccia con l'etichetta responses va dal server al client](simple-client-server.png)

- I client sono i dispositivi connessi a Internet del tipico utente web (ad esempio, il computer connesso al Wi-Fi oppure il telefono connesso alla rete mobile) e il software per accedere al web disponibile su tali dispositivi (di solito un browser web come Firefox o Chrome).
- I server sono computer che memorizzano pagine web, siti o app. Quando un client vuole accedere a una pagina web, una copia del codice della pagina web viene scaricata dal server alla macchina client, dove viene sottoposta a rendering dal browser e visualizzata all'utente.

Il seguente contenuto incorporato di Scrimba<sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> fornisce ulteriori informazioni su client e server, inclusi un quiz e una discussione.

<mdn-scrim-inline url="https://scrimba.com/frontend-path-c0j/~0lq" scrimtitle="Client e server"></mdn-scrim-inline>

## Le altre parti della cassetta degli attrezzi

Il client e il server descritti sopra non raccontano tutta la storia. Sono coinvolte molte altre parti, che verranno descritte di seguito.

Per ora, immaginiamo che Internet sia una strada. A un'estremità della strada c'è il client, che è come una casa. All'altra estremità della strada c'è il server, che è come un negozio da cui si vuole comprare qualcosa.

![Una foto in bianco e nero di una persona che attraversa una strada sulle strisce pedonali](road.jpg)

Affinché i dati possano viaggiare in entrambe le direzioni, sono necessarie le seguenti cose:

- **La connessione Internet**: consente di inviare e ricevere dati su Internet. È sostanzialmente come la strada tra casa e il negozio.
- **TCP/IP**: **Transmission Control Protocol** e **Internet Protocol** (TCP/IP) sono protocolli di comunicazione che definiscono come i dati devono viaggiare attraverso Internet. Sono come i meccanismi di trasporto che consentono di effettuare un ordine, andare al negozio e acquistare merci. Nel nostro esempio, sono come un'auto o una bicicletta, oppure qualsiasi altro mezzo usato per percorrere la strada.
- **DNS**: il **Domain Name System** (DNS) è come una rubrica per i siti web. Quando si digita un indirizzo web nel browser, il browser consulta il DNS per trovare l'indirizzo IP del sito web — l'indirizzo effettivo in cui si trova il server — prima di poter recuperare il sito web (per maggiori informazioni, vedere [Spiegazione del DNS](#spiegazione_del_dns) qui sotto). Il browser deve scoprire su quale server risiede il sito web, in modo da poter inviare messaggi HTTP al posto giusto (vedere sotto). È come cercare l'indirizzo del negozio prima di visitarlo.
- **HTTP**: **Hypertext Transfer Protocol** (HTTP) è un {{Glossary("Protocol", "protocollo")}} applicativo che definisce un linguaggio affinché client e server possano comunicare tra loro. È come il linguaggio usato per ordinare le merci. Vedere [Nozioni di base su HTTP](#nozioni_di_base_su_http) qui sotto.
- **File**: un sito web è composto da molti file diversi, che sono come i diversi prodotti acquistati nel negozio. Questi file appartengono a due tipi principali:
  - **Codice**: i siti web sono costruiti principalmente con HTML, CSS e JavaScript, i diversi linguaggi di programmazione in cui sono scritti i siti web, che il browser interpreta e assembla in una pagina web da visualizzare all'utente.
  - **Risorse**: è un termine collettivo per tutti gli altri elementi che appaiono su un sito web, come immagini, musica, video, documenti Word e PDF, che non sono codice interpretato dal browser.

  > [!NOTE]
  > Più avanti nel corso è possibile scoprire come il browser assembla questi file in una pagina web, in [Come i browser caricano i siti web](/it/docs/Learn_web_development/Getting_started/Web_standards/How_browsers_load_websites).

## Quindi cosa accade, esattamente?

Quando si digita un indirizzo web, che tecnicamente è parte di un [URL](#componenti_di_un_url), nella barra degli indirizzi del browser, si verificano i passaggi seguenti:

1. Il browser si rivolge al server DNS e trova l'indirizzo reale del server su cui risiede il sito web.
2. Il browser invia un messaggio di richiesta HTTP al server, chiedendogli di inviare una copia del sito web al client. Questo messaggio, e tutti gli altri dati inviati tra il client e il server, viene trasmesso attraverso la connessione Internet usando TCP/IP.
3. Se il server approva la richiesta del client, invia al client un messaggio "200 OK", che significa "Certo che puoi guardare quel sito web! Eccolo", quindi inizia a inviare i file del sito web al browser come una serie di piccoli blocchi chiamati [pacchetti](#spiegazione_dei_pacchetti).
4. Il browser assembla i piccoli blocchi in una pagina web completa e la visualizza.

## Spiegazione del DNS

I veri indirizzi web ([URL](#componenti_di_un_url)) non sono le piacevoli stringhe facili da ricordare che si digitano nella barra degli indirizzi per trovare i siti web preferiti. Sono numeri speciali che hanno questo aspetto: `192.0.2.172`.

Questo è chiamato {{Glossary("IP_Address", "indirizzo IP")}} e rappresenta una posizione univoca sul web. Tuttavia, non è molto facile da ricordare, vero? Per questo motivo è stato inventato il Domain Name System. Questo sistema usa server speciali che associano un indirizzo web digitato nel browser, come `mozilla.org`, all'indirizzo reale (IP) del sito web. I siti web di grandi dimensioni sono comunemente resi disponibili su più server, affinché vengano caricati in modo efficiente per utenti diversi in tutto il mondo. Di conseguenza, l'indirizzo IP può variare in base alla posizione.

È possibile usare uno strumento di ricerca DNS per trovare gli indirizzi IP di un sito web. Ad esempio, visitare lo [strumento di ricerca DNS NsLookup.io](https://www.nslookup.io/website-to-ip-lookup/), digitare `developer.mozilla.org` e premere il pulsante.

## Spiegazione dei pacchetti

In precedenza, è stato usato il termine "pacchetti" per descrivere il formato in cui i dati vengono trasferiti tra client e server. Cosa significa?

Quando i dati vengono inviati attraverso il web, vengono trasmessi in più piccoli blocchi chiamati pacchetti. Ogni pacchetto contiene:

- Un'**intestazione**, che include dettagli quali l'indirizzo IP del server e del client, il numero del pacchetto, il numero totale di pacchetti nella trasmissione e dettagli dei protocolli usati nella trasmissione.
- Un **payload**, che contiene i dati effettivi inviati nel pacchetto.

Esistono vari motivi per cui i dati vengono inviati in piccoli pacchetti, ma i più importanti sono:

- Talvolta vengono persi o danneggiati e, quando ciò accade, è più rapido e semplice per il client richiedere i pacchetti mancanti anziché un intero file.
- I pacchetti possono essere instradati lungo percorsi diversi, rendendo la trasmissione quanto più efficiente possibile e riducendo la possibilità di rallentare la rete, specialmente quando molti utenti richiedono la stessa risorsa contemporaneamente. I pacchetti possono arrivare fuori sequenza, ma il client può usare le informazioni nelle intestazioni dei pacchetti per assicurarsi che vengano assemblati nell'ordine corretto.

## Nozioni di base su HTTP

HTTP usa un linguaggio semplice di verbi per eseguire azioni come effettuare richieste (vedere [metodi di richiesta HTTP](/it/docs/Web/HTTP/Reference/Methods)). Il metodo HTTP [`GET`](/it/docs/Web/HTTP/Reference/Methods/GET) è quello normalmente usato per effettuare richieste HTTP del tipo descritto sopra. Ad esempio, una richiesta per la pagina iniziale di MDN potrebbe apparire così:

```http
GET /en-US/ HTTP/2

Host: developer.mozilla.org
```

La risposta inviata dal server potrebbe apparire più o meno così:

```http
HTTP/2 200

date: Tue, 11 Feb 2025 11:13:30 GMT
expires: Tue, 11 Feb 2025 11:40:01 GMT
server: Google frontend
last-modified: Tue, 11 Feb 2025 00:49:32 GMT
ETag: "65f26b7f6463e2347f4e5a7a2adcee54"
content-length: 45227
content-type: text/html

<!doctype html> ... (the 45227 bytes of the requested web page HTML)
```

La risposta completa è più complessa di questa, ma per brevità la maggior parte è stata omessa. Le parti principali sono le seguenti:

- `HTTP/2 200`
  - : La versione di HTTP che il server sta usando per inviare la risposta, in questo caso HTTP/2, seguita da un [codice di stato](/it/docs/Web/HTTP/Reference/Status) che indica se la richiesta ha avuto successo. `200` indica successo.
- `date`, `expires`, ecc.
  - : [Intestazioni HTTP](/it/docs/Web/HTTP/Reference/Headers) contenenti informazioni aggiuntive sulla risposta (si noti che anche le richieste possono avere intestazioni), che forniscono informazioni extra e/o ne modificano il comportamento.
- `<!doctype html>`, ecc.
  - : Il corpo della risposta, che in questo caso contiene il documento HTML della home page di MDN.

> [!NOTE]
> Per molti più dettagli su HTTP, consultare il [riferimento HTTP](/it/docs/Web/HTTP) di MDN. [Una panoramica di HTTP](/it/docs/Web/HTTP/Guides/Overview) è un buon punto di partenza.

### Altri codici di stato

Sopra è stato incontrato il [codice di stato](/it/docs/Web/HTTP/Reference/Status) `200`, che indica che la richiesta HTTP ha avuto successo. Esistono molti codici di stato HTTP con significati e usi specifici, ma comunemente se ne vedranno solo alcuni:

- `301`
  - : La risorsa richiesta è stata spostata permanentemente in una nuova posizione, fornita nella risposta. Questo viene usato per reindirizzare i contenuti quando vengono spostati.
- `400`
  - : Il server non può elaborare la richiesta. Questo di solito accade quando la richiesta non è in un formato compreso dal server oppure contiene errori.
- `403`
  - : Il server non concede al client l'accesso alla risorsa richiesta. Questo di solito accade quando il server sa chi è il client, ma quest'ultimo non ha il permesso di accedere alla pagina richiesta.
- `404`
  - : Il server non riesce a trovare la risorsa richiesta. Questo stato viene comunemente restituito se l'URL è errato oppure se il contenuto viene eliminato senza impostare un reindirizzamento.
- `503`
  - : La richiesta non può essere gestita a causa di un problema con il server. Questo è comune quando i server sono offline per manutenzione e si prevede che sia temporaneo.

## Componenti di un URL

Tecnicamente, gli indirizzi web digitati nella barra degli indirizzi del browser fanno parte degli **Uniform Resource Locator** (**URL**). Gli URL definiscono le posizioni di risorse univoche su Internet.

Un URL è un indirizzo web più un protocollo: ad esempio, se si apre una nuova scheda nel browser, si digita `developer.mozilla.org` nella barra degli indirizzi e si preme <kbd>Enter</kbd>/<kbd>Return</kbd>, si verrà reindirizzati a un URL simile al seguente:

```plain
https://developer.mozilla.org/en-US/
```

Le parti principali dell'URL sono:

- `https`
  - : Il **protocollo** usato per inviare la richiesta. In questo caso si usa {{Glossary("HTTPS", "HTTPS")}}, che è una versione sicura di HTTP che impedisce a malintenzionati di leggere i dati durante il trasporto. Nel web moderno, praticamente ogni server usa HTTPS, quindi se non viene incluso esplicitamente, il browser presume che sia quello usato e lo aggiunge automaticamente.
- `developer.mozilla.org`
  - : Il [**nome di dominio**](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_domain_name) dell'URL, che rappresenta la posizione di livello superiore del server a cui ci si sta connettendo. In questo caso, l'indirizzo web digitato coincide con il nome di dominio, ma non è sempre così: si potrebbe scegliere di digitare un indirizzo web più complesso. Si noti che la parte `developer` è un **sottodominio** (area di contenuto distinta) del dominio `mozilla.org` di Mozilla. Sul sito di Mozilla esistono altri sottodomini che ospitano contenuti distinti; vedere ad esempio [support.mozilla.org](https://support.mozilla.org/) e [bugzilla.mozilla.org](https://bugzilla.mozilla.org/).
- `/en-US/`
  - : Il **percorso** alla risorsa sul server a cui si sta accedendo. MDN conserva tutti i suoi contenuti in inglese statunitense in una cartella chiamata `en-US`, a cui punta questo URL.

    Se il browser è configurato per preferire i contenuti in inglese per impostazione predefinita, questo è l'URL a cui si verrà reindirizzati digitando `developer.mozilla.org`. Se il browser è configurato per preferire un'altra lingua supportata da MDN, come il francese, si verrà reindirizzati a un URL diverso, come `https://developer.mozilla.org/fr/`. Questa funzionalità non è disponibile per impostazione predefinita su tutti i siti web; gli sviluppatori di MDN hanno configurato MDN in questo modo per consentire alle persone di accedere facilmente alla lingua preferita.

> [!NOTE]
> Negli URL possono apparire molti altri componenti. Per maggiori dettagli, vedere [Che cos'è un URL?](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_URL).

## Vedere anche

- [Come funziona Internet](/it/docs/Learn_web_development/Howto/Web_mechanics/How_does_the_Internet_work)

## Riconoscimenti

Foto della strada: [Street composing](https://www.pinterest.com/pin/400538960580676851/), di [Kevin Digga](https://www.pinterest.com/kevindigga/).

{{NextMenu("Learn_web_development/Getting_started/Web_standards/The_web_standards_model", "Learn_web_development/Getting_started/Web_standards")}}
