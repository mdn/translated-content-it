---
title: Come assicurarsi che il tuo sito web funzioni correttamente?
slug: Learn_web_development/Howto/Tools_and_setup/Checking_that_your_web_site_is_working_properly
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

In questo articolo, esaminiamo vari passaggi per la risoluzione dei problemi di un sito web e alcune azioni di base da intraprendere per risolvere questi problemi.

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
        Imparerai come diagnosticare e risolvere alcuni problemi di base che puoi incontrare con il tuo sito web.
      </td>
    </tr>
  </tbody>
</table>

Hai pubblicato il tuo sito web online? Molto bene! Ma sei sicuro che funzioni correttamente?

Un server web remoto spesso si comporta in modo molto diverso da uno locale, quindi è una buona idea testare il tuo sito web una volta che è online. Potresti essere sorpreso di quanti problemi si presentano: immagini che non appaiono, pagine che non si caricano o si caricano lentamente e così via. La maggior parte delle volte non è niente di grave, solo un semplice errore o un problema con la configurazione dell'hosting web.

Vediamo come diagnosticare e risolvere questi problemi.

## Apprendimento Attivo

_Non ci sono ancora attività di apprendimento attivo disponibili. [Per favore, considera di contribuire](/it/docs/MDN/Community/Getting_started)._

## Approfondimento

### Test nel tuo browser

Se vuoi sapere se il tuo sito web funziona correttamente, la prima cosa da fare è aprire il browser e andare alla pagina che vuoi testare.

#### Uh-oh, dov'è l'immagine?

Guardiamo il nostro sito personale, `http://demozilla.examplehostingprovider.net/`. Non mostra l'immagine che ci aspettavamo!

![Ops, l'immagine 'unicorn' manca](image-missing.png)

Apri lo strumento di rete di Firefox (**Strumenti ➤ Sviluppatore Web ➤ Rete**) e ricarica la pagina:

![L'immagine ha un errore 404](error404.png)

Ecco il problema, quel "404" in fondo. "404" significa "risorsa non trovata", ed è per questo che non abbiamo visto l'immagine.

#### Stato HTTP

I server rispondono con un messaggio di stato ogni volta che ricevono una richiesta. Ecco gli stati più comuni:

- **200: OK**
  - : La risorsa richiesta è stata consegnata.
- **301: Spostato permanentemente**
  - : La risorsa è stata spostata in una nuova posizione. Non lo vedrai molto nel tuo browser, ma è bene sapere del "301" poiché i motori di ricerca usano molto queste informazioni per aggiornare i loro indici.
- **304: Non modificato**
  - : Il file non è cambiato dall'ultima volta che lo hai richiesto, quindi il tuo browser può visualizzare la versione dalla sua cache, risultando in tempi di risposta più veloci e un uso più efficiente della larghezza di banda.
- **403: Vietato**
  - : Non ti è consentito visualizzare la risorsa. Di solito è dovuto a un errore di configurazione (ad esempio, il tuo provider di hosting ha dimenticato di darti i diritti di accesso a una directory).
- **404: Non trovato**
  - : Autoesplicativo. Discuteremo come risolvere questo più avanti.
- **500: Errore interno del server**
  - : Qualcosa è andato storto sul server. Ad esempio, forse il linguaggio server-side ({{Glossary("PHP", "PHP")}}, .Net, ecc.) ha smesso di funzionare, o il web server stesso ha un problema di configurazione. Di solito è meglio ricorrere al team di supporto del tuo provider di hosting.
- **503: Servizio non disponibile**
  - : Di solito risultante da un sovraccarico di sistema a breve termine. Il server ha qualche tipo di problema. Riprova tra un po'.

Come principianti che controllano il nostro sito web (semplice), ci occuperemo più spesso di 200, 304, 403 e 404.

#### Correzione del 404

Quindi cosa è andato storto?

![Elenco delle immagini nel nostro progetto](demozilla-images-list.png)

A prima vista, l'immagine che abbiamo richiesto sembra essere al posto giusto ma lo strumento di rete ha segnalato un "404". Si scopre che abbiamo commesso un errore di battitura nel nostro codice HTML: `unicorn_pics.png` invece di `unicorn_pic.png`. Quindi correggi l'errore di battitura nel tuo editor di codice cambiando l'attributo `src` dell'immagine:

![Cancellazione della 's'](code-correct.png)

Salva, [carica sul server](/it/docs/Learn_web_development/Howto/Tools_and_setup/Upload_files_to_a_web_server), e ricarica la pagina nel tuo browser:

![L'immagine si carica correttamente nel browser](image-corrected.png)

Ecco fatto! Guardiamo di nuovo gli stati {{Glossary("HTTP", "HTTP")}}:

- **200** per `/` e per `unicorn_pic.png` significa che siamo riusciti a ricaricare la pagina e l'immagine.
- **304** per `basic.css` significa che questo file non è cambiato dall'ultima richiesta, quindi il browser può usare il file nella sua cache anziché riceverne una copia nuova.

Quindi abbiamo risolto l'errore e imparato un paio di stati HTTP lungo la strada!

### Errori frequenti

Gli errori più frequenti che troviamo sono questi:

#### Errori di battitura nell'indirizzo

Volevamo digitare `http://demozilla.examplehostingprovider.net/` ma abbiamo digitato troppo in fretta e dimenticato una "l":

![Indirizzo irraggiungibile](cannot-find-server.png)

L'indirizzo non può essere trovato. In effetti.

#### Errori 404

Molte volte l'errore deriva solo da un errore di battitura, ma a volte forse hai dimenticato di caricare una risorsa o hai perso la connessione di rete mentre stavi caricando le tue risorse. Prima verifica la correttezza e l'accuratezza del percorso del file e, se c'è ancora un problema, carica nuovamente i tuoi file. Probabilmente risolverà il problema.

#### Errori JavaScript

Qualcuno (forse tu) ha aggiunto uno script alla pagina e ha commesso un errore. Questo non impedirà il caricamento della pagina, ma noterai che qualcosa è andato storto.

Apri la console (**Strumenti ➤ Sviluppatore Web ➤ Console Web**) e ricarica la pagina:

![Un errore JavaScript viene mostrato nella Console](js-error.png)

In questo esempio, apprendiamo (abbastanza chiaramente) qual è l'errore e possiamo andare a correggerlo (tratteremo JavaScript in [un'altra serie](/it/docs/Learn_web_development/Core/Scripting) di articoli).

### Altri elementi da controllare

Abbiamo elencato alcuni semplici modi per verificare che il tuo sito web funzioni correttamente, nonché gli errori più comuni che potresti incontrare e come risolverli. Puoi anche testare se la tua pagina soddisfa questi criteri:

#### Come va la performance?

La pagina si carica abbastanza velocemente? Risorse come [WebPageTest.org](https://www.webpagetest.org/) o componenti aggiuntivi per browser come [YSlow](https://github.com/marcelduran/yslow) possono dirti alcune cose interessanti:

![Diagnostica di Yslow](yslow-diagnostics.png)

Le valutazioni vanno da A a F. La nostra pagina è piccola e soddisfa la maggior parte dei criteri. Ma possiamo già notare che sarebbe stato meglio utilizzare un {{Glossary("CDN", "CDN")}}. Non importa molto quando stiamo servendo solo un'immagine, ma sarebbe fondamentale per un sito web ad alta larghezza di banda che serve molte migliaia di immagini.

#### Il server è sufficientemente reattivo?

`ping` è uno strumento della shell utile che testa il nome di dominio fornito e ti dice se il server risponde o meno:

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

Basta ricordare una comoda scorciatoia da tastiera: **Ctrl+C**. Ctrl+C invia un segnale di "interruzione" al runtime e gli dice di fermarsi. Se non arresti il runtime, `ping` contatterà il server indefinitamente.

### Un semplice elenco di controllo

- Controlla eventuali 404
- Assicurati che tutte le pagine web si comportino come previsto
- Verifica il tuo sito web in diversi browser per assicurarti che si renda in modo coerente

## Prossimi passi

Congratulazioni, il tuo sito web è attivo e pronto per essere visitato da chiunque. È un grande traguardo. Ora, puoi iniziare ad approfondire vari argomenti.

- Poiché le persone possono arrivare al tuo sito web da tutto il mondo, dovresti considerare di renderlo [accessibile a tutti](/it/docs/Learn_web_development/Howto/Design_and_accessibility/What_is_accessibility).
- Il design del tuo sito web è un po' troppo grezzo? È ora di [saperne di più su CSS](/it/docs/Learn_web_development/Core/Styling_basics).
