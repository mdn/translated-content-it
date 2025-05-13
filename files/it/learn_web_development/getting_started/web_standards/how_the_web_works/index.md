---
title: Come funziona il web
slug: Learn_web_development/Getting_started/Web_standards/How_the_web_works
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Getting_started/Web_standards/The_Web_standards_model", "Learn_web_development/Getting_started/Web_standards")}}

_Come funziona il web_ fornisce una descrizione ad alto livello di ciò che accade quando si utilizza un browser web per navigare su una pagina web, spiegando la magia che avviene dietro le quinte per consegnare il codice pertinente al computer affinché il browser possa assemblarlo in qualcosa di visibile.

Questa teoria non è essenziale per scrivere codice web nel breve termine, ma ben presto inizierà davvero a beneficiare della comprensione di ciò che accade in background.

> [!NOTE]
> Questo articolo non copre come i browser web effettivamente rendono il codice in pagine web. Questo è trattato in [Come i browser caricano i siti web](/it/docs/Learn_web_development/Getting_started/Web_standards/How_browsers_load_websites).

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con il sistema operativo del computer, i browser web e le tecnologie web.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Client e server e i loro ruoli nel web.</li>
          <li>DNS e come funziona a un livello alto.</li>
          <li>Lo scopo di TCP/IP, HTTP e pacchetti.</li>
          <li>Sintassi HTTP a un livello base.</li>
          <li>Codici di risposta HTTP comuni (ad esempio, 200, 301, 403, 404 e 500).</li>
          <li>Componenti base di un URL (protocollo, dominio, sottodominio, percorso).</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Client e server

I computer connessi a internet sono chiamati **client** e **server**. Un diagramma semplificato di come interagiscono potrebbe assomigliare a questo:

![Due cerchi che rappresentano il client e il server. Una freccia etichettata richiesta va dal client al server, e una freccia etichettata risposte va dal server al client](simple-client-server.png)

- I client sono i dispositivi connessi a internet dell'utente web tipico (ad esempio, il computer connesso al Wi-Fi, o il telefono connesso alla rete mobile) e il software di accesso web disponibile su quei dispositivi (di solito un browser web come Firefox o Chrome).
- I server sono computer che memorizzano pagine web, siti o app. Quando un client vuole accedere a una pagina web, una copia del codice della pagina web viene scaricata dal server sul dispositivo del client per essere resa dal browser e visualizzata all'utente.

## Le altre parti della cassetta degli attrezzi

Il client e il server che abbiamo descritto sopra non raccontano l'intera storia. Ci sono molte altre parti coinvolte, e le descriveremo qui di seguito.

Per ora, immaginiamo che internet sia una strada. Da un capo della strada c'è il client, che è come la sua casa. All'altro capo della strada c'è il server, che è come un negozio da cui desidera acquistare qualcosa.

![Una foto in bianco e nero di una persona che attraversa una strada su un passaggio pedonale](road.jpg)

Per far sì che i dati vadano avanti e indietro, abbiamo bisogno delle seguenti cose:

- **La sua connessione a internet**: Le consente di inviare e ricevere dati su internet. È fondamentalmente come la strada tra la sua casa e il negozio.
- **TCP/IP**: **Transmission Control Protocol** e **Internet Protocol** (TCP/IP) sono protocolli di comunicazione che definiscono come i dati dovrebbero viaggiare su internet. Questo è come i meccanismi di trasporto che le permettono di effettuare un ordine, andare al negozio e acquistare i suoi beni. Nel nostro esempio, questo è come un'auto o una bicicletta (o qualsiasi altro modo in cui potrebbe viaggiare lungo la strada).
- **DNS**: Il **Domain Name System** (DNS) è come un elenco di indirizzi per siti web. Quando digita un indirizzo web nel suo browser, il browser guarda il DNS per trovare l'indirizzo IP del sito web — l'indirizzo effettivo in cui si trova il server — prima che possa recuperare il sito web (vedere [DNS spiegato](#dns_spiegato) qui sotto per ulteriori informazioni). Il browser deve trovare su quale server risiede il sito web, in modo da poter inviare messaggi HTTP al posto giusto (vedere sotto). Questo è come cercare l'indirizzo del negozio prima di visitarlo.
- **HTTP**: **Hypertext Transfer Protocol** (HTTP) è un {{Glossary("Protocol", "protocollo")}} applicativo che definisce un linguaggio per far sì che client e server possano comunicare tra loro. Questo è come il linguaggio che utilizza per ordinare i suoi beni. Vedere [basics di HTTP](#basi_di_http) qui sotto.
- **File**: Un sito web è costituito da molti file diversi, che sono come i diversi beni che acquista dal negozio. Questi file si dividono in due tipi principali:

  - **Codice**: I siti web sono costruiti principalmente da HTML, CSS e JavaScript — i diversi linguaggi di programmazione in cui sono scritti i siti web, che il browser interpreta e assembla in una pagina web da visualizzare.
  - **Risorse**: Questo è un termine collettivo per tutti gli altri elementi che appaiono su un sito web — come immagini, musica, video, documenti Word e PDF — che non sono codice che il browser interpreta.

  > [!NOTE]
  > Può scoprire come il browser assembla questi file in una pagina web più avanti nel corso, in [Come i browser caricano i siti web](/it/docs/Learn_web_development/Getting_started/Web_standards/How_browsers_load_websites).

## Quindi cosa succede, esattamente?

Quando digita un indirizzo web (che è tecnicamente parte di un [URL](#componenti_di_un_url)) nella barra degli indirizzi del browser, avvengono i seguenti passaggi:

1. Il browser va al server DNS e trova il vero indirizzo del server su cui risiede il sito web (cerca l'indirizzo del negozio).
2. Il browser invia un messaggio di richiesta HTTP al server, chiedendogli di inviare una copia del sito web al client (va al negozio e ordina i suoi beni). Questo messaggio, e tutti gli altri dati inviati tra il client e il server, vengono inviati attraverso la connessione internet utilizzando TCP/IP.
3. Se il server approva la richiesta del client, il server invia al client un messaggio "200 OK", che significa "Certo, può vedere quel sito web! Ecco qua", e poi inizia a inviare i file del sito web al browser come una serie di piccoli blocchi chiamati [pacchetti di dati](#pacchetti_spiegati) (il negozio le consegna i suoi beni, e lei li porta a casa).
4. Il browser assembla i piccoli blocchi in una pagina web completa e la visualizza (ottenere i beni a casa — roba nuova e brillante, fantastico!).

## DNS spiegato

I veri indirizzi web ([URL](#componenti_di_un_url)) non sono le belle stringhe memorabili che digita nella barra degli indirizzi per trovare i suoi siti web preferiti. Sono numeri speciali che assomigliano a questo: `192.0.2.172`.

Questo è chiamato un {{Glossary("IP_Address", "indirizzo IP")}} e rappresenta una posizione unica sul web. Tuttavia, non è molto facile da ricordare, vero? Ecco perché è stato inventato il Domain Name System. Questo sistema utilizza server speciali che associano un indirizzo web digitato nel browser (come "mozilla.org") all'indirizzo reale (IP) del sito web.

I siti web possono essere raggiunti direttamente tramite i loro indirizzi IP. Può utilizzare uno strumento di ricerca DNS per trovare l'indirizzo IP di un sito web.

> [!CALLOUT]
>
> **Provalo**
>
> 1. Vai allo [strumento di ricerca DNS di NsLookup.io](https://www.nslookup.io/website-to-ip-lookup/), inserisci `developer.mozilla.org` e premi il pulsante.
> 2. Nella schermata dei risultati, copia l'indirizzo IP (l'indirizzo IPv4) negli appunti del sistema.
> 3. Apri una nuova scheda del browser, incolla l'indirizzo IP nella barra degli indirizzi e premi <kbd>Invio</kbd>/<kbd>Invio</kbd>. Dovrebbe vedere l'apertura di MDN, dimostrando che l'indirizzo IP punta a esso.

## Pacchetti spiegati

In precedenza abbiamo usato il termine "pacchetti" per descrivere il formato in cui i dati vengono trasferiti tra client e server. Cosa intendiamo qui?

Fondamentalmente, quando i dati vengono inviati attraverso il web, vengono inviati in migliaia di piccoli blocchi. Ci sono molteplici ragioni per cui i dati vengono inviati in piccoli pacchetti, ma in particolare:

- A volte vengono persi o corrotti e, quando ciò accade, è più rapido e semplice sostituire piccoli blocchi piuttosto che interi file.
- Inoltre, i pacchetti possono essere instradati lungo percorsi diversi, rendendo lo scambio più veloce e consentendo a molti utenti di scaricare lo stesso sito web contemporaneamente. Se ogni sito web fosse inviato come un unico grande blocco, solo un utente potrebbe scaricarlo alla volta, il che renderebbe il web molto inefficiente e non molto divertente da usare.

## Basi di HTTP

HTTP utilizza un linguaggio semplice di verbi per eseguire azioni come effettuare richieste (vedere [Metodi di richiesta HTTP](/it/docs/Web/HTTP/Reference/Methods)). Il metodo HTTP [`GET`](/it/docs/Web/HTTP/Reference/Methods/GET) è quello normalmente usato per effettuare richieste HTTP del tipo sopra descritto. Ad esempio, una richiesta per la home page di MDN potrebbe apparire così:

```http
GET /en-US/ HTTP/2

Host: developer.mozilla.org
```

La risposta inviata dal server potrebbe apparire qualcosa del genere:

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

La risposta completa è più complessa di questa, ma abbiamo omesso la maggior parte per brevità. Le parti principali sono le seguenti:

- `HTTP/2 200`
  - : La versione di HTTP che il server sta utilizzando per inviare la risposta, in questo caso HTTP/2, seguita da un [codice di stato](/it/docs/Web/HTTP/Reference/Status) che indica se la richiesta è stata effettuata con successo. `200` indica successo.
- `date`, `expires`, etc.
  - : [Intestazioni HTTP](/it/docs/Web/HTTP/Reference/Headers) che contengono informazioni aggiuntive sulla risposta (si noti che anche le richieste possono avere intestazioni), che forniscono informazioni extra e/o modificano il suo comportamento.
- `<!doctype html>`, etc.
  - : Il corpo della risposta, che in questo caso contiene il documento HTML della homepage di MDN.

> [!NOTE]
> Può vedere il riferimento HTTP di MDN [HTTP reference](/it/docs/Web/HTTP) per molti più dettagli su HTTP, se è curioso. [Una panoramica di HTTP](/it/docs/Web/HTTP/Guides/Overview) è un buon punto di partenza.

### Altri codici di stato

Sopra, abbiamo incontrato il codice di stato `200`, che indica che la richiesta HTTP è stata effettuata con successo. Ci sono molti codici di stato HTTP con significati e usi specifici, ma ne vedrà comunemente solo alcuni:

- `301`
  - : La risorsa richiesta è stata spostata permanentemente in una nuova posizione, fornita nella risposta. Questo è usato per reindirizzare i contenuti quando vengono spostati.
- `400`
  - : Il server non può elaborare la richiesta. Questo di solito accade quando la richiesta non è in un formato che il server comprende, o presenta errori.
- `403`
  - : Il server non darà accesso al client alla risorsa richiesta. Questo di solito accade quando il server sa chi è il client, ma non ha il permesso di accedere alla pagina richiesta.
- `404`
  - : Il server non riesce a trovare la risorsa richiesta. Questo codice di stato viene comunemente restituito se l'URL è sbagliato o se il contenuto è stato eliminato senza inserire un reindirizzamento.
- `503`
  - : La richiesta non può essere gestita a causa di un problema con il server. Questo è comune quando i server sono offline per manutenzione ed è previsto che sia temporaneo.

## Componenti di un URL

Tecnicamente, gli indirizzi web che digita nella barra degli indirizzi del browser formano parte degli **Uniform Resource Locators** (**URL**). Gli URL definiscono le posizioni delle risorse uniche su internet.

Un URL è un indirizzo web più un protocollo: per esempio, se apre una nuova scheda nel suo browser, digita `developer.mozilla.org` nella barra degli indirizzi e preme <kbd>Invio</kbd>/<kbd>Return</kbd>, verrà reindirizzato a un URL come il seguente:

```plain
https://developer.mozilla.org/en-US/
```

Le principali parti dell'URL sono:

- `https`
  - : Il **protocollo** utilizzato per inviare la richiesta. In questo caso, stiamo utilizzando {{Glossary("HTTPS", "HTTPS")}}, che è una versione sicura di HTTP che impedisce a persone malintenzionate di leggere i suoi dati mentre vengono trasportati. Sul web moderno, quasi ogni server utilizza HTTPS, quindi se non lo include esplicitamente, il browser presume che sia ciò che sta usando e lo aggiunge per lei.
- `developer.mozilla.org`
  - : Il [**nome di dominio**](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_domain_name) dell'URL, che rappresenta la posizione di alto livello del server a cui si sta collegando. In questo caso, l'indirizzo web che ha digitato è uguale al nome di dominio, ma non è sempre così — potrebbe scegliere di digitare un indirizzo web più complicato. Si noti che la parte `developer` è un **sottodominio** (area di contenuto distintiva) del dominio `mozilla.org` di Mozilla. Ci sono altri sottodomini sul sito di Mozilla che ospitano contenuti distinti — vedere ad esempio [support.mozilla.org](https://support.mozilla.org/) e [bugzilla.mozilla.org](https://bugzilla.mozilla.org/).
- `/en-US/`

  - : Il **percorso** verso la risorsa sul server a cui si sta accedendo. MDN conserva tutti i suoi contenuti in inglese americano in una cartella chiamata `en-US`, che è ciò a cui questo URL sta puntando.

    Se ha impostato il suo browser per preferire i contenuti in inglese per impostazione predefinita, allora questo è l'URL a cui verrà reindirizzato quando digita `developer.mozilla.org`. Se ha impostato il suo browser per preferire un'altra lingua che MDN supporta, come il francese, verrà reindirizzato a un URL diverso, come `https://developer.mozilla.org/fr/` invece. Questo non è disponibile per ogni sito web per impostazione predefinita; gli sviluppatori di MDN hanno configurato MDN in questo modo per consentire alle persone di accedere facilmente alla lingua che preferiscono.

> [!NOTE]
> Ci sono molti più componenti che possono apparire negli URL. Vedere [Che cos'è un URL?](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_URL) per ulteriori dettagli.

## Vedi anche

- [Come funziona Internet](/it/docs/Learn_web_development/Howto/Web_mechanics/How_does_the_Internet_work)

## Credito

Foto di strada: [Street composing](https://www.pinterest.com/pin/400538960580676851/), di [Kevin Digga](https://www.pinterest.com/kevindigga/).

{{NextMenu("Learn_web_development/Getting_started/Web_standards/The_web_standards_model", "Learn_web_development/Getting_started/Web_standards")}}
