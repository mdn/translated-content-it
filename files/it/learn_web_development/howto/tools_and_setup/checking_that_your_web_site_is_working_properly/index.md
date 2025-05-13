---
title: Come assicurarsi che il suo sito web funzioni correttamente?
slug: Learn_web_development/Howto/Tools_and_setup/Checking_that_your_web_site_is_working_properly
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

In questo articolo, esploreremo vari passaggi per la risoluzione dei problemi di un sito web e alcune azioni di base da intraprendere per risolvere questi problemi.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Deve sapere come
        <a
          href="/it/docs/Learn_web_development/Howto/Tools_and_setup/Upload_files_to_a_web_server"
          >caricare file su un server web</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparerà a diagnosticare e risolvere alcuni problemi di base che può incontrare con il suo sito web.
      </td>
    </tr>
  </tbody>
</table>

Ha pubblicato il suo sito web online? Molto bene! Ma è sicuro che funzioni correttamente?

Un server web distante spesso si comporta in modo molto diverso da uno locale, quindi è una buona idea testare il suo sito una volta che è online. Potrebbe essere sorpreso dal numero di problemi che possono sorgere: le immagini non si mostrano, le pagine non si caricano o si caricano lentamente, e così via. La maggior parte delle volte non è un grosso problema, solo un piccolo errore o un problema con la configurazione dell'hosting web.

Vediamo come diagnosticare e risolvere questi problemi.

## Apprendimento attivo

_Non è disponibile ancora alcun apprendimento attivo. [Per favore, consideri di contribuire](/it/docs/MDN/Community/Getting_started)._

## Approfondimenti

### Test nel suo browser

Se vuole sapere se il suo sito web funziona correttamente, la prima cosa da fare è aprire il browser e accedere alla pagina che desidera testare.

#### Uh-oh, dov'è l'immagine?

Diamo un'occhiata al nostro sito web personale, `http://demozilla.examplehostingprovider.net/`. Non mostra l'immagine che ci aspettavamo!

![Oops, l'immagine 'unicorn' è mancante](image-missing.png)

Aprire lo strumento di rete di Firefox (**Strumenti ➤ Sviluppatore web ➤ Rete**) e ricaricare la pagina:

![L'immagine ha un errore 404](error404.png)

Ecco il problema, quel "404" in fondo. "404" significa "risorsa non trovata", ed è per questo che non vedevamo l'immagine.

#### Stati HTTP

I server rispondono con un messaggio di stato ogni volta che ricevono una richiesta. Ecco gli stati più comuni:

- **200: OK**
  - : La risorsa richiesta è stata consegnata.
- **301: Spostato permanentemente**
  - : La risorsa è stata spostata in una nuova posizione. Non vedrà molto questo nel suo browser, ma è bene sapere del "301" poiché i motori di ricerca utilizzano molto queste informazioni per aggiornare i loro indici.
- **304: Non modificato**
  - : Il file non è cambiato dall'ultima volta che è stato richiesto, quindi il suo browser può visualizzarlo dalla cache, risultando in tempi di risposta più rapidi e un uso più efficiente della larghezza di banda.
- **403: Proibito**
  - : Non le è permesso visualizzare la risorsa. Di solito è dovuto a un errore di configurazione (ad esempio, il suo fornitore di hosting ha dimenticato di darle i diritti di accesso a una directory).
- **404: Non trovato**
  - : Autodescrittivo. Discuteremo come risolverlo di seguito.
- **500: Errore interno del server**
  - : Qualcosa è andato storto sul server. Ad esempio, forse il linguaggio lato server ({{Glossary("PHP", "PHP")}}, .Net, ecc.) ha smesso di funzionare, o il server web stesso ha un problema di configurazione. Di solito è meglio rivolgersi al team di supporto del suo fornitore di hosting.
- **503: Servizio non disponibile**
  - : Di solito risultante da un sovraccarico del sistema a breve termine. Il server ha qualche tipo di problema. Provi di nuovo tra un po'.

Come principianti che controllano il nostro (semplice) sito web, ci occuperemo più spesso di 200, 304, 403 e 404.

#### Correggere il 404

Quindi, cosa è andato storto?

![L'elenco delle immagini nel nostro progetto](demozilla-images-list.png)

A prima vista, l'immagine che abbiamo richiesto sembra essere nel posto giusto, ma lo strumento di rete ha riportato un "404". Si scopre che abbiamo commesso un errore di battitura nel nostro codice HTML: `unicorn_pics.png` anziché `unicorn_pic.png`. Quindi corregga l'errore di battitura nel suo editor di codice cambiando l'attributo `src` dell'immagine:

![Eliminazione della 's'](code-correct.png)

Salvi, [carichi al server](/it/docs/Learn_web_development/Howto/Tools_and_setup/Upload_files_to_a_web_server), e ricarichi la pagina nel suo browser:

![L'immagine si carica correttamente nel browser](image-corrected.png)

Ecco fatto! Diamo un'occhiata di nuovo agli stati {{Glossary("HTTP", "HTTP")}}:

- **200** per `/` e per `unicorn_pic.png` significa che siamo riusciti a ricaricare la pagina e l'immagine.
- **304** per `basic.css` significa che questo file non è cambiato dall'ultima richiesta, quindi il browser può usare il file nella sua cache piuttosto che ricevere una nuova copia.

Così abbiamo risolto l'errore e imparato alcuni stati HTTP lungo la strada!

### Errori frequenti

Gli errori più frequenti che troviamo sono questi:

#### Errori di battitura nell'indirizzo

Volevamo digitare `http://demozilla.examplehostingprovider.net/` ma abbiamo digitato troppo velocemente e abbiamo dimenticato una "l":

![Indirizzo irraggiungibile](cannot-find-server.png)

L'indirizzo non può essere trovato. In effetti.

#### Errori 404

Molte volte l'errore risulta solo da un errore di battitura, ma a volte potrebbe aver dimenticato di caricare una risorsa o ha perso la connessione di rete mentre stava caricando le sue risorse. Per primo controlli l'ortografia e l'accuratezza del percorso del file, e se c'è ancora un problema, carichi di nuovo i suoi file. È probabile che ciò risolva il problema.

#### Errori di JavaScript

Qualcuno (forse lei) ha aggiunto uno script alla pagina e ha commesso un errore. Questo non impedirà il caricamento della pagina, ma sentirà che qualcosa è andato storto.

Apra la console (**Strumenti ➤ Sviluppatore web ➤ Console web**) e ricarichi la pagina:

![Un errore JavaScript è mostrato nella Console](js-error.png)

In questo esempio, apprendiamo (piuttosto chiaramente) quale sia l'errore e possiamo andare a correggerlo (tratteremo JavaScript in [un'altra serie](/it/docs/Learn_web_development/Core/Scripting) di articoli).

### Altre cose da controllare

Abbiamo elencato alcuni modi semplici per controllare che il suo sito web funzioni correttamente, nonché gli errori più comuni che può incontrare e come risolverli. Può anche testare se la sua pagina soddisfa questi criteri:

#### Come sono le prestazioni?

La pagina si carica abbastanza velocemente? Risorse come [WebPageTest.org](https://www.webpagetest.org/) o componenti aggiuntivi del browser come [YSlow](https://github.com/marcelduran/yslow) possono dirle qualche cosa interessante:

![Diagnostica Yslow](yslow-diagnostics.png)

I voti vanno dalla A alla F. La nostra pagina è piuttosto piccola e soddisfa la maggior parte dei criteri. Ma possiamo già notare che sarebbe stato meglio usare un {{Glossary("CDN", "CDN")}}. Questo non importa molto quando serviamo solo un'immagine, ma sarebbe critico per un sito web ad alta larghezza di banda che serve molte migliaia di immagini.

#### Il server è abbastanza reattivo?

`ping` è uno strumento utile della shell che testa il nome di dominio che si fornisce e le dice se il server risponde o meno:

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

Basta ricordare una scorciatoia da tastiera utile: **Ctrl+C**. Ctrl+C invia un segnale di "interruzione" al runtime e gli dice di fermarsi. Se non ferma il runtime, `ping` pingerà il server indefinitamente.

### Un semplice elenco di controllo

- Controllare i 404
- Assicurarsi che tutte le pagine web si comportino come si aspetta
- Controllare il suo sito web in diversi browser per assicurarsi che venga visualizzato in modo coerente

## Prossimi passi

Congratulazioni, il suo sito web è attivo e funzionante per chiunque voglia visitarlo. È un grande successo. Ora può iniziare ad approfondire vari argomenti.

- Poiché le persone possono arrivare al suo sito web da tutto il mondo, dovrebbe considerare di renderlo [accessibile a tutti](/it/docs/Learn_web_development/Howto/Design_and_accessibility/What_is_accessibility).
- Il design del suo sito web è un po' troppo grezzo? È ora di [saperne di più su CSS](/it/docs/Learn_web_development/Core/Styling_basics).
