---
title: Come funziona il web
slug: Learn_web_development/Getting_started/Web_standards/How_the_web_works
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Getting_started/Web_standards/The_Web_standards_model", "Learn_web_development/Getting_started/Web_standards")}}

_Come funziona il web_ fornisce una descrizione di alto livello di ciò che accade quando si utilizza un browser web per navigare verso una pagina web, spiegando la magia che avviene dietro le quinte per consegnare il codice rilevante al tuo computer affinché il browser lo assembli in qualcosa che puoi visualizzare.

Questa teoria non è essenziale per scrivere codice web a breve termine, ma col tempo davvero inizierai a beneficiare della comprensione di ciò che accade in background.

> [!NOTE]
> Questo articolo non copre come i browser web effettivamente renderizzano il codice in pagine web. Questo viene coperto in [Come i browser caricano i siti web](/it/docs/Learn_web_development/Getting_started/Web_standards/How_browsers_load_websites).

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con il tuo sistema operativo, i browser web e le tecnologie web.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Client e server e i loro ruoli nel web.</li>
          <li>DNS e come funziona a un livello elevato.</li>
          <li>La funzione di TCP/IP, HTTP e pacchetti.</li>
          <li>La sintassi HTTP a un livello di base.</li>
          <li>Codici di risposta HTTP comuni (ad es., 200, 301, 403, 404 e 500).</li>
          <li>Componenti base di un URL (protocollo, dominio, sottodominio, percorso).</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Client e server

I computer connessi a internet sono chiamati **client** e **server**. Un diagramma semplificato di come interagiscono potrebbe essere simile a questo:

![Due cerchi che rappresentano client e server. Una freccia etichettata "request" va dal client al server e una freccia etichettata "responses" va dal server al client](simple-client-server.png)

- I client sono i dispositivi tipici degli utenti web connessi a internet (ad esempio, il tuo computer connesso al Wi-Fi o il tuo telefono connesso alla rete mobile) e il software per accedere al web disponibile su quei dispositivi (di solito un browser web come Firefox o Chrome).
- I server sono computer che memorizzano pagine web, siti o app. Quando un client vuole accedere a una pagina web, una copia del codice della pagina web viene scaricata dal server sul computer client per essere renderizzata dal browser e mostrata all'utente.

## Le altre parti della cassetta degli attrezzi

Il client e il server che abbiamo descritto sopra non raccontano tutta la storia. Ci sono molte altre parti coinvolte, e le descriveremo di seguito.

Per ora, immaginiamo che internet sia una strada. All'estremità della strada c'è il client, che è come la tua casa. All'altra estremità della strada c'è il server, che è come un negozio da cui vuoi comprare qualcosa.

![Una foto in bianco e nero di una persona che attraversa una strada sulle strisce pedonali](road.jpg)

Per far sì che i dati vadano avanti e indietro, abbiamo bisogno delle seguenti cose:

- **La tua connessione internet**: Ti consente di inviare e ricevere dati su internet. È fondamentalmente come la strada tra la tua casa e il negozio.
- **TCP/IP**: **Transmission Control Protocol** e **Internet Protocol** (TCP/IP) sono protocolli di comunicazione che definiscono come i dati dovrebbero viaggiare su internet. Questo è come i meccanismi di trasporto che ti permettono di fare un ordine, andare al negozio e acquistare i tuoi beni. Nel nostro esempio, è come un'auto o una bicicletta (o comunque tu decida di viaggiare lungo la strada).
- **DNS**: Il **Domain Name System** (DNS) è come un rubrica per i siti web. Quando digiti un indirizzo web nel tuo browser, il browser consulta il DNS per trovare l'indirizzo IP del sito web — l'indirizzo reale in cui si trova il server — prima di poter recuperare il sito web (vedi [DNS spiegato](#dns_spiegato) sotto per maggiori informazioni). Il browser deve trovare su quale server si trova il sito web, così da poter inviare i messaggi HTTP nel posto giusto (vedi sotto). Questo è come cercare l'indirizzo del negozio prima di visitarlo.
- **HTTP**: **Hypertext Transfer Protocol** (HTTP) è un {{Glossary("Protocol", "protocollo")}} applicativo che definisce un linguaggio affinché i client e i server possano comunicare tra loro. Questo è come il linguaggio che usi per ordinare i tuoi beni. Vedi [Nozioni di base HTTP](#nozioni_di_base_http) sotto.
- **File**: Un sito web è costituito da molti file diversi, che sono come i vari beni che acquisti dal negozio. Questi file si suddividono in due tipi principali:

  - **Codice**: I siti web sono costruiti principalmente con HTML, CSS e JavaScript — i vari linguaggi di programmazione in cui sono scritti i siti web, che il browser interpreta e assembla in una pagina web da mostrare a un utente.
  - **Risorse**: Questo è un termine collettivo per tutti gli altri elementi che appaiono su un sito web — come immagini, musica, video, documenti Word e PDF — che non sono codice interpretato dal browser.

  > [!NOTE]
  > Puoi scoprire come il browser assembli questi file in una pagina web più avanti nel corso, in [Come i browser caricano i siti web](/it/docs/Learn_web_development/Getting_started/Web_standards/How_browsers_load_websites).

## Allora cosa succede, esattamente?

Quando digiti un indirizzo web (che è tecnicamente parte di un [URL](#componenti_di_un_url)) nella barra degli indirizzi del tuo browser, si verificano i seguenti passaggi:

1. Il browser consulta il server DNS per trovare il vero indirizzo del server in cui risiede il sito web (cerchi l'indirizzo del negozio).
2. Il browser invia un messaggio di richiesta HTTP al server, chiedendogli di inviare una copia del sito web al client (vai al negozio e ordini i tuoi beni). Questo messaggio, e tutti gli altri dati inviati tra il client e il server, viene trasmesso attraverso la tua connessione internet usando TCP/IP.
3. Se il server approva la richiesta del client, invia al client un messaggio "200 OK", che significa "Certo che puoi vedere quel sito web! Eccolo qui" e poi inizia a inviare i file del sito web al browser come una serie di piccoli frammenti chiamati [pacchetti di dati](#pacchetti_spiegati) (il negozio ti dà i tuoi beni e li porti a casa).
4. Il browser assembla i piccoli frammenti in una pagina web completa e te la mostra (porti i beni a casa — roba nuova e brillante, fantastico!).

## DNS spiegato

I veri indirizzi web ([URL](#componenti_di_un_url)) non sono le stringhe belle e memorabili che digiti nella barra degli indirizzi per trovare i tuoi siti web preferiti. Sono numeri speciali che somigliano a questo: `192.0.2.172`.

Questo è chiamato {{Glossary("IP_Address", "indirizzo IP")}}, e rappresenta una posizione univoca sul web. Tuttavia, non è molto facile da ricordare, vero? Ecco perché è stato inventato il Domain Name System. Questo sistema utilizza server speciali che abbinano un indirizzo web che digiti nel tuo browser (come "mozilla.org") all'indirizzo reale (IP) del sito web.

I siti web possono essere raggiunti direttamente tramite i loro indirizzi IP. Puoi usare uno strumento di ricerca DNS per trovare l'indirizzo IP di un sito web.

> [!CALLOUT]
>
> **Provalo tu stesso**
>
> 1. Vai allo strumento di ricerca DNS [NsLookup.io](https://www.nslookup.io/website-to-ip-lookup/), digita `developer.mozilla.org` e premi il pulsante.
> 2. Nella schermata dei risultati, copia l'indirizzo IP (l'indirizzo IPv4) negli appunti del tuo sistema.
> 3. Apri una nuova scheda del browser, incolla l'indirizzo IP nella barra degli indirizzi e premi <kbd>Invio</kbd>/<kbd>Return</kbd>. Dovresti vedere il caricamento di MDN, dimostrando che l'indirizzo IP punta ad esso.

## Pacchetti spiegati

In precedenza abbiamo usato il termine "pacchetti" per descrivere il formato in cui i dati vengono trasferiti tra il client e il server. Cosa intendiamo qui?

Fondamentalmente, quando i dati vengono inviati attraverso il web, vengono inviati in migliaia di piccoli frammenti. Ci sono molteplici ragioni per cui i dati vengono inviati in piccoli pacchetti, ma la più significativa è:

- A volte vengono persi o corrotti e, quando ciò accade, è più veloce e semplice sostituire piccoli frammenti invece di file interi.
- Inoltre, i pacchetti possono essere instradati lungo percorsi diversi, rendendo lo scambio più veloce e consentendo a molti utenti diversi di scaricare lo stesso sito web contemporaneamente. Se ogni sito web venisse inviato come un unico grande blocco, solo un utente alla volta potrebbe scaricarlo, rendendo il web molto inefficiente e poco piacevole da usare.

## Nozioni di base HTTP

HTTP utilizza un linguaggio semplice di verbi per eseguire azioni come fare richieste (vedi [Metodi di richiesta HTTP](/it/docs/Web/HTTP/Reference/Methods)). Il metodo HTTP [`GET`](/it/docs/Web/HTTP/Reference/Methods/GET) è quello normalmente usato per fare richieste HTTP del tipo descritto sopra. Ad esempio, una richiesta per la home page di MDN potrebbe sembrare così:

```http
GET /en-US/ HTTP/2

Host: developer.mozilla.org
```

La risposta inviata dal server potrebbe sembrare qualcosa del genere:

```http
HTTP/2 200

date: Tue, 11 Feb 2025 11:13:30 GMT
expires: Tue, 11 Feb 2025 11:40:01 GMT
server: Google frontend
last-modified: Tue, 11 Feb 2025 00:49:32 GMT
etag: "65f26b7f6463e2347f4e5a7a2adcee54"
content-length: 45227
content-type: text/html

<!doctype html> ... (the 45227 bytes of the requested web page HTML)
```

La risposta completa è più complessa di così, ma abbiamo omesso la maggior parte per brevità. Le parti principali sono le seguenti:

- `HTTP/2 200`
  - : La versione di HTTP che il server sta usando per inviare la risposta, in questo caso HTTP/2, seguito da un [codice di stato](/it/docs/Web/HTTP/Reference/Status) che indica se la richiesta è stata eseguita con successo. `200` indica successo.
- `date`, `expires`, ecc.
  - : [Intestazioni HTTP](/it/docs/Web/HTTP/Reference/Headers) contenenti informazioni aggiuntive sulla risposta (nota che anche le richieste possono avere intestazioni), che forniscono informazioni extra e/o modificano il suo comportamento.
- `<!doctype html>`, ecc.
  - : Il corpo della risposta, che in questo caso contiene il documento HTML della home page di MDN.

> [!NOTE]
> Vedi la [riferimento HTTP](/it/docs/Web/HTTP) su MDN per molti più dettagli su HTTP, se sei curioso. [Una panoramica di HTTP](/it/docs/Web/HTTP/Guides/Overview) è un buon punto di partenza.

### Altri codici di stato

Sopra, abbiamo incontrato il codice di stato `200`, che indica che la richiesta HTTP è stata eseguita con successo. Ci sono molti codici di stato HTTP con significati e usi specifici, ma ne vedrai comunemente solo alcuni:

- `301`
  - : La risorsa richiesta è stata spostata permanentemente in una nuova posizione, che è fornita nella risposta. Questo è usato per reindirizzare i contenuti quando sono stati spostati.
- `400`
  - : Il server non può processare la richiesta. Questo accade di solito quando la richiesta non è in un formato che il server comprende o contiene errori.
- `403`
  - : Il server non darà al client accesso alla risorsa richiesta. Questo di solito accade quando il server sa chi è il client, ma non ha i permessi per accedere alla pagina richiesta.
- `404`
  - : Il server non può trovare la risorsa richiesta. Questo stato viene comunemente restituito se l'URL è sbagliato o se il contenuto è stato eliminato senza impostare un reindirizzamento.
- `503`
  - : La richiesta non può essere gestita a causa di un problema con il server. Questo è comune quando i server sono offline per manutenzione ed è previsto che sia temporaneo.

## Componenti di un URL

Tecnicamente, gli indirizzi web che digiti nella barra degli indirizzi del browser fanno parte dei **Localizzatori Uniformi di Risorse** (**URL**). Gli URL definiscono le posizioni di risorse uniche su internet.

Un URL è un indirizzo web più un protocollo: per esempio, se apri una nuova scheda nel tuo browser, digiti `developer.mozilla.org` nella barra degli indirizzi e premi <kbd>Invio</kbd>/<kbd>Return</kbd>, sarai reindirizzato a un URL come il seguente:

```plain
https://developer.mozilla.org/en-US/
```

Le parti principali dell'URL sono:

- `https`
  - : Il **protocollo** utilizzato per inviare la richiesta. In questo caso, stiamo usando {{Glossary("HTTPS", "HTTPS")}}, che è una versione sicura di HTTP che impedisce alle persone malintenzionate di leggere i tuoi dati mentre vengono trasportati. Sul web moderno, praticamente ogni server usa HTTPS, quindi se non lo includi esplicitamente, il browser assume che lo stai usando e lo aggiunge per te.
- `developer.mozilla.org`
  - : Il [**nome di dominio**](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_domain_name) dell'URL, che rappresenta la posizione di alto livello del server a cui ti stai connettendo. In questo caso, l'indirizzo web che hai digitato è uguale al nome di dominio, ma non è sempre così — potresti scegliere di digitare un indirizzo web più complicato. Nota che la parte `developer` è un **sottodominio** (area di contenuto distinta) del dominio `mozilla.org` di Mozilla. Ci sono altri sottodomini sul sito di Mozilla che ospitano contenuti distinti — vedi [support.mozilla.org](https://support.mozilla.org/) e [bugzilla.mozilla.org](https://bugzilla.mozilla.org/), per esempio.
- `/en-US/`

  - : Il **percorso** verso la risorsa sul server a cui stai accedendo. MDN tiene tutti i suoi contenuti in inglese americano in una cartella chiamata `en-US`, che è ciò a cui punta questo URL.

    Se hai impostato il tuo browser per preferire i contenuti in inglese per impostazione predefinita, allora questo è l'URL a cui sarai reindirizzato quando digiti `developer.mozilla.org`. Se hai impostato il tuo browser per preferire una lingua diversa supportata da MDN, come il francese, sarai reindirizzato a un URL diverso, come `https://developer.mozilla.org/fr/` invece. Questo non è disponibile per ogni sito web di default; gli sviluppatori di MDN hanno configurato MDN in questo modo per consentire alle persone di accedere facilmente nella lingua che preferiscono.

> [!NOTE]
> Ci sono molti più componenti che possono apparire negli URL. Vedi [Che cos'è un URL?](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_URL) per ulteriori dettagli.

## Vedi anche

- [Come funziona Internet](/it/docs/Learn_web_development/Howto/Web_mechanics/How_does_the_Internet_work)

## Credito

Foto della strada: [Street composing](https://www.pinterest.com/pin/400538960580676851/), di [Kevin Digga](https://www.pinterest.com/kevindigga/).

{{NextMenu("Learn_web_development/Getting_started/Web_standards/The_web_standards_model", "Learn_web_development/Getting_started/Web_standards")}}
