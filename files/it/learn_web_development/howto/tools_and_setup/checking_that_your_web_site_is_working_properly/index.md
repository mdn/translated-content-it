---
title: Come assicurarsi che il proprio sito web funzioni correttamente?
slug: Learn_web_development/Howto/Tools_and_setup/Checking_that_your_web_site_is_working_properly
l10n:
  sourceCommit: f33de00c56ac53878eb2cb7cb5849df1f9ab8db7
---

In questo articolo vengono illustrati vari passaggi per la risoluzione dei problemi di un sito web e alcune azioni di base da intraprendere per risolverli.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        È necessario sapere come
        <a
          href="/it/docs/Learn_web_development/Howto/Tools_and_setup/Upload_files_to_a_web_server"
          >caricare file su un server web</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Si imparerà a diagnosticare e risolvere alcuni problemi di base che possono
        verificarsi con il proprio sito web.
      </td>
    </tr>
  </tbody>
</table>

Il sito web è stato pubblicato online? Ottimo! Ma funziona davvero correttamente?

Un server web remoto si comporta spesso in modo molto diverso da uno locale, quindi è consigliabile testare il sito web una volta online. Potrebbe sorprendere quanti problemi possono emergere: le immagini non vengono visualizzate, le pagine non si caricano o si caricano lentamente e così via. Nella maggior parte dei casi non si tratta di nulla di grave, ma solo di un semplice errore o di un problema nella configurazione dell'hosting web.

Vediamo come diagnosticare e risolvere questi problemi.

## Approfondimento

### Test nel browser

Per sapere se il sito web funziona correttamente, la prima cosa da fare è aprire il browser e visitare la pagina da testare.

#### Oh no, dov'è l'immagine?

Osserviamo il sito web personale, `http://demozilla.examplehostingprovider.net/`. L'immagine prevista non viene visualizzata.

![Ops, l'immagine del "unicorn" non è presente](image-missing.png)

Aprire lo strumento Rete di Firefox (**Strumenti ➤ Sviluppo web ➤ Rete**) e ricaricare la pagina:

![L'immagine ha un errore 404](error404.png)

Ecco il problema: quel "404" in fondo. "404" significa "risorsa non trovata", ed è per questo che l'immagine non viene visualizzata.

#### Stati HTTP

I server rispondono con un messaggio di stato ogni volta che ricevono una richiesta. Ecco gli stati più comuni:

- **200: OK**
  - : La risorsa richiesta è stata consegnata.
- **301: Moved permanently**
  - : La risorsa è stata spostata in una nuova posizione. Questo stato non appare spesso nel browser, ma è utile conoscere il "301", poiché i motori di ricerca usano molto questa informazione per aggiornare i propri indici.
- **304: Not modified**
  - : Il file non è cambiato dall'ultima richiesta, quindi il browser può visualizzare la versione presente nella cache, ottenendo tempi di risposta più rapidi e un uso più efficiente della larghezza di banda.
- **403: Forbidden**
  - : Non è consentito visualizzare la risorsa. Di solito dipende da un errore di configurazione, ad esempio il provider di hosting non ha assegnato i diritti di accesso a una directory.
- **404: Not found**
  - : Il significato è evidente. Di seguito verrà illustrato come risolverlo.
- **500: Internal server error**
  - : Qualcosa è andato storto sul server. Ad esempio, il linguaggio lato server ({{Glossary("PHP", "PHP")}}, .Net e così via) potrebbe aver smesso di funzionare, oppure il server web stesso potrebbe avere un problema di configurazione. Di solito è preferibile rivolgersi al team di supporto del provider di hosting.
- **503: Service unavailable**
  - : In genere è causato da un sovraccarico temporaneo del sistema. Il server ha qualche tipo di problema. Riprovare dopo qualche istante.

Come principianti che controllano un sito web (semplice), gli stati più frequenti saranno 200, 304, 403 e 404.

#### Correggere il 404

Che cosa è andato storto?

![L'elenco di immagini nel progetto](demozilla-images-list.png)

A prima vista, l'immagine richiesta sembra trovarsi nel posto giusto, ma lo strumento Rete ha segnalato un "404". Si scopre che è stato commesso un errore di battitura nel codice HTML: `unicorn_pics.png` anziché `unicorn_pic.png`. Correggere quindi l'errore nell'editor di codice modificando l'attributo `src` dell'immagine:

![Eliminazione della "s"](code-correct.png)

Salvare, [inviare al server](/it/docs/Learn_web_development/Howto/Tools_and_setup/Upload_files_to_a_web_server) e ricaricare la pagina nel browser:

![L'immagine viene caricata correttamente nel browser](image-corrected.png)

Ecco fatto! Osserviamo nuovamente gli stati {{Glossary("HTTP", "HTTP")}}:

- **200** per `/` e per `unicorn_pic.png` significa che il ricaricamento della pagina e dell'immagine è riuscito.
- **304** per `basic.css` significa che questo file non è cambiato dall'ultima richiesta, quindi il browser può usare il file nella cache invece di riceverne una nuova copia.

L'errore è stato risolto e, nel frattempo, sono stati appresi alcuni stati HTTP.

### Errori frequenti

Gli errori più frequenti sono i seguenti:

#### Errori di battitura nell'indirizzo

Si voleva digitare `http://demozilla.examplehostingprovider.net/`, ma si è digitato troppo velocemente e si è dimenticata una "l":

![Indirizzo non raggiungibile](cannot-find-server.png)

L'indirizzo non può essere trovato. Effettivamente.

#### Errori 404

Molte volte l'errore deriva semplicemente da un errore di battitura, ma a volte potrebbe essere stata dimenticata l'operazione di caricamento di una risorsa oppure potrebbe essersi persa la connessione di rete durante il caricamento delle risorse. Controllare prima l'ortografia e la correttezza del percorso del file; se il problema persiste, caricare nuovamente i file. Questo probabilmente risolverà il problema.

#### Errori JavaScript

Qualcuno, forse lo sviluppatore stesso, ha aggiunto uno script alla pagina e ha commesso un errore. Questo non impedirà il caricamento della pagina, ma sarà evidente che qualcosa non ha funzionato.

Aprire la console (**Strumenti ➤ Sviluppo web ➤ Console web**) e ricaricare la pagina:

![Nella Console viene mostrato un errore JavaScript](js-error.png)

In questo esempio, viene indicato chiaramente qual è l'errore ed è possibile correggerlo (JavaScript verrà trattato in [un'altra serie](/it/docs/Learn_web_development/Core/Scripting) di articoli).

### Altri aspetti da controllare

Sono stati elencati alcuni semplici modi per verificare che il sito web funzioni correttamente, oltre agli errori più comuni che potrebbero verificarsi e a come risolverli. È anche possibile verificare se la pagina soddisfa questi criteri:

#### Come sono le prestazioni?

La pagina si carica abbastanza velocemente? Risorse come [WebPageTest.org](https://www.webpagetest.org/) o componenti aggiuntivi del browser come [YSlow](https://github.com/marcelduran/yslow) possono fornire alcune informazioni interessanti:

![Diagnostica di Yslow](yslow-diagnostics.png)

Le valutazioni vanno da A a F. La pagina è piccola e soddisfa la maggior parte dei criteri. Tuttavia, si può già notare che sarebbe stato meglio utilizzare una {{Glossary("CDN", "CDN")}}. Questo non è molto importante quando viene distribuita una sola immagine, ma sarebbe fondamentale per un sito web a elevata larghezza di banda che distribuisce molte migliaia di immagini.

#### Il server risponde abbastanza rapidamente?

`ping` è un utile strumento di shell che testa il nome di dominio fornito e indica se il server risponde oppure no:

```plain
$ ping mozilla.org
PING mozilla.org (63.245.215.20): 56 data bytes
64 bytes from 63.245.215.20: icmp_seq=0 ttl=44 time=148.741 ms
64 bytes from 63.245.215.20: icmp_seq=1 ttl=44 time=148.541 ms
64 bytes from 63.245.215.20: icmp_seq=2 ttl=44 time=148.734 ms
64 bytes from 63.245.215.20: icmp_seq=3 ttl=44 time=147.857 ms
^C
--- mozilla.org ping statistics ---
4 packets transmitted, 4 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 147.857/148.468/148.741/0.362 ms
```

È sufficiente ricordare una comoda scorciatoia da tastiera: **Ctrl+C**. Ctrl+C invia un segnale di "interruzione" al runtime e gli indica di arrestarsi. Se il runtime non viene arrestato, `ping` eseguirà il ping del server indefinitamente.

### Una semplice checklist

- Verificare la presenza di errori 404.
- Assicurarsi che tutte le pagine web si comportino come previsto.
- Controllare il sito web in diversi browser per assicurarsi che venga visualizzato in modo coerente.

## Passaggi successivi

Congratulazioni, il sito web è attivo e funzionante, pronto per essere visitato da chiunque. È un grande traguardo. Ora è possibile approfondire diversi argomenti.

- Poiché le persone possono raggiungere il sito web da tutto il mondo, è opportuno valutare di renderlo [accessibile a tutti](/it/docs/Learn_web_development/Howto/Design_and_accessibility/What_is_accessibility).
- Il design del sito web è un po' troppo grezzo? È il momento di [imparare di più su CSS](/it/docs/Learn_web_development/Core/Styling_basics).
