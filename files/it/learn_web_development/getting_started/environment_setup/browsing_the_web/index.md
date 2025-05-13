---
title: Navigare sul web
slug: Learn_web_development/Getting_started/Environment_setup/Browsing_the_web
l10n:
  sourceCommit: be1922d62a0d31e4e3441db0e943aed8df736481
---

{{PreviousMenuNext("Learn_web_development/Getting_started/Environment_setup/Installing_software", "Learn_web_development/Getting_started/Environment_setup/Code_editors", "Learn_web_development/Getting_started/Environment_setup")}}

A questo punto nel modulo, dovreste avere installato diversi browser web moderni sul vostro computer o altri dispositivi disponibili. Questo articolo approfondisce l'utilizzo dei browser, analizzando come funziona un browser web, la differenza tra alcune delle cose quotidiane con cui interagirete e come cercare informazioni.

> [!NOTE]
> Se non ha installato nessun browser oltre a quelli predefiniti forniti con i suoi dispositivi, installi degli altri. Veda [Browser web moderni](/it/docs/Learn_web_development/Getting_started/Environment_setup/Installing_software#modern_web_browsers) per maggiori informazioni.

Come in qualsiasi area di conoscenza, il web presenta molti termini tecnici e gergo. Non si preoccupi: non la sommergeremo con tutto subito (può consultare il [glossario](/it/docs/Glossary) se è curiosa). Tuttavia, ci sono alcuni termini di base che deve comprendere fin dall'inizio, dato che sentirà usare queste espressioni frequentemente. Introdurremo alcuni termini importanti di seguito.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con il sistema operativo del suo computer.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>La differenza tra un browser web, un sito web e un motore di ricerca.</li>
          <li>Come funziona un browser web a un livello base.</li>
          <li>Cercare informazioni.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## La differenza tra pagina web, sito web, server web e motore di ricerca

Inizieremo descrivendo vari concetti legati al web: pagine web, siti web, server web e motori di ricerca. Questi termini sono spesso confusi dai neofiti del web o usati in modo errato. Assicuriamoci che sappiate cosa significhino! Cominciamo con alcune definizioni:

- **Pagina web**
  - : Un documento che può essere visualizzato in un {{Glossary("browser", "browser")}} web. Questi documenti sono spesso chiamati semplicemente "pagine". Tali documenti sono scritti nel linguaggio {{Glossary("HTML", "HTML")}} (che esamineremo in maggior dettaglio più avanti).
- **Sito web**
  - : Una raccolta di pagine web raggruppate insieme in una singola risorsa, con collegamenti che le uniscono. Spesso chiamato "sito".
- **Server web**
  - : Un computer che ospita un sito web su Internet.
- **Servizio web**
  - : Un software che risponde a richieste su Internet per eseguire una funzione o fornire dati. Un servizio web è tipicamente supportato da un server web e può fornire pagine web con cui gli utenti interagiscono. Molti siti web sono anche servizi web, anche se alcuni siti (come MDN) sono costituiti solo da contenuti statici. Esempi di servizi web potrebbero essere qualcosa che ridimensiona immagini, fornisce un rapporto meteorologico, o gestisce il login degli utenti.
- **Motore di ricerca**
  - : Un servizio web che l'aiuta a trovare altre pagine web, come Google, Bing, Yahoo, o DuckDuckGo. I motori di ricerca sono normalmente accessibili attraverso un browser web (ad esempio, si possono effettuare ricerche nei motori di ricerca direttamente nella barra degli indirizzi di Firefox, Chrome, ecc.) o attraverso una pagina web (ad esempio, [bing.com](https://www.bing.com/) o [duckduckgo.com](https://duckduckgo.com/)).

Guardiamo un'analogia: una biblioteca pubblica. Questo è ciò che generalmente farebbe quando visita una biblioteca:

1. Trova un indice di ricerca e cerca il titolo del libro che desidera.
2. Prende nota del numero di catalogo del libro.
3. Va alla sezione specifica che contiene il libro, trova il giusto numero di catalogo, e prende il libro.

Confrontiamo una biblioteca pubblica con il web:

- La biblioteca è come un server web. Ha diverse sezioni, simile a un server web che ospita diversi siti web.
- Le diverse sezioni (scienza, matematica, storia, ecc.) nella biblioteca sono come siti web. Ogni sezione è come un sito web unico (due sezioni non contengono gli stessi libri).
- I libri in ogni sezione sono come pagine web. Un sito web può avere diverse pagine web, ad esempio, la sezione Scienza (il sito web) avrà libri su calore, suono, termodinamica, statica, ecc.
- L'indice di ricerca è come il motore di ricerca. Ogni libro ha una propria posizione unica nella biblioteca (due libri non possono essere mantenuti nello stesso posto) che è specificata dal numero di catalogo.

Prendiamoci ora il tempo di esaminare ogni termine in un po' più di dettaglio.

### Pagina web

Una **pagina web** è un semplice documento visualizzabile da un browser. Una pagina web può incorporare una varietà di tipi diversi di risorse come:

- _Informazioni di stile_ — che controllano l'aspetto e la struttura di una pagina.
- _Script_ — che aggiungono interattività alla pagina.
- _Media_ — immagini, suoni e video.

> [!NOTE]
> I browser possono visualizzare anche altri documenti come file {{Glossary("PDF", "PDF")}} e altre risorse come immagini o video, ma il termine **pagina web** si riferisce specificamente a documenti HTML.

Tutte le pagine web possono essere trovate in un luogo unico (indirizzo web, anche chiamato {{Glossary("URL", "URL")}}). Per accedere a una pagina, basta digitare il suo indirizzo nella barra degli indirizzi del browser:

![Esempio di un indirizzo di una pagina web nella barra degli indirizzi del browser](web-page.jpg)

> [!CALLOUT]
>
> **Mettilo in pratica**
>
> Prova a caricare uno dei suoi siti web preferiti in un browser adesso.

### Sito web

Un _sito web_ è una raccolta di pagine web collegate (più le risorse associate) che condividono un unico [nome di dominio](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_domain_name). Ogni pagina web di un determinato sito offre collegamenti espliciti — per la maggior parte sotto forma di porzioni di testo cliccabili — che consentono all'utente di passare da una pagina del sito all'altra.

Quando carica il suo sito web preferito in un browser, tende a visualizzare per prima la pagina principale del sito, o _home page_ (comunemente chiamata "home"):

![Esempio di un nome di dominio di un sito web nella barra degli indirizzi del browser](web-site.jpg)

> [!CALLOUT]
>
> **Mettilo in pratica**
>
> Prova a fare clic su alcuni elementi di menu o collegamenti per visualizzare alcune pagine diverse sul suo sito web preferito.

> [!NOTE]
> È anche possibile avere un {{Glossary("SPA", "__app a pagina singola__")}}: un sito web che consiste di una singola pagina web che viene aggiornata dinamicamente con nuovo contenuto quando necessario.

### Server web

Un _server web_ è un computer che ospita uno o più _siti web_. "Ospitare" significa che tutte le _pagine web_ e i loro file associati sono disponibili su quel computer. Il _server web_ invierà i file delle pagine web che ospita al browser di un utente quando essi tenteranno di caricarlo.

Non confonda _siti web_ e _server web_. Ad esempio, se sente qualcuno dire, "Il mio sito web non risponde", in realtà significa che il _server web_ non risponde e quindi il _sito web_ non è disponibile. Più importante, poiché un server web può ospitare più siti web, il termine _server web_ non è mai usato per designare un sito web, poiché potrebbe causare grande confusione. Nel nostro esempio precedente, se dicessimo, "Il mio server web non risponde", significa che più siti web su quel server web non sono disponibili.

### Motore di ricerca

I motori di ricerca sono una fonte comune di confusione sul web. Un motore di ricerca è un particolare tipo di sito web che aiuta gli utenti a trovare pagine web da _altri_ siti web.

Ce ne sono molti: [Google](https://www.google.com/), [Bing](https://www.bing.com/), [Yandex](https://yandex.com/), [DuckDuckGo](https://duckduckgo.com/), e molti altri. Alcuni sono generici, alcuni sono specializzati su certi argomenti.

Molti principianti del web confondono i motori di ricerca con i browser. Facciamo chiarezza: Un _browser_ è un pezzo di software che recupera e visualizza pagine web; un _motore di ricerca_ è un sito web che aiuta le persone a trovare pagine web da altri siti. La confusione sorge perché, la prima volta che qualcuno avvia un browser, spesso il browser mostra la homepage di un motore di ricerca o una casella di ricerca che permette di cercare un termine usando quel motore di ricerca. La maggior parte dei browser permette anche ai loro utenti di usare un motore di ricerca digitando termini di ricerca direttamente nella barra degli indirizzi del browser.

Tutto ciò ha senso perché la prima cosa che le persone tendono a voler fare con un browser è trovare una pagina web da visualizzare. Non confonda il software (il browser) con il servizio (il motore di ricerca).

Ecco un'istanza di Firefox che mostra una casella di ricerca Google come sua pagina di avvio predefinita:

![Esempio di Firefox nightly che visualizza una pagina Google personalizzata come predefinita](search-engine.jpg)

> [!CALLOUT]
>
> **Mettilo in pratica**
>
> Effettui una ricerca in un motore di ricerca:
>
> - Andando alla homepage di un motore di ricerca e inserendo un termine di ricerca.
> - Inserendo un termine di ricerca nella barra degli indirizzi del browser.

## Come funziona il web: le basi

In molte parti del mondo, il web è diventato uno strumento essenziale nella nostra vita quotidiana come posate, biciclette e auto, o spazzolini da denti. Se ciò le sembra irrealistico, pensi solo a quanto spesso utilizza un sito web o un'app per cellulare ogni giorno! Anche se non sta inserendo un indirizzo web in un browser per accedere a contenuti o servizi, la possibilità che l'app che sta utilizzando stia probabilmente usando tecnologia web dietro le quinte per ottenere dati da presentarvi è alta.

Quando accede al web, succedono molte cose tra la sua prima interazione (ad esempio, digitare un indirizzo web (URL) in un browser e premere <kbd>Invio</kbd>/<kbd>Return</kbd>) e il risultato della sua azione che le viene presentato (ad esempio, il sito web che appare nel suo browser web):

1. Il browser web richiede la risorsa (ad esempio, una pagina web, alcuni dati, o un'immagine o video) che desidera accedere al server web su cui è memorizzata. Tali richieste (e le risposte risultanti) vengono effettuate utilizzando una tecnologia chiamata {{Glossary("HTTP", "HTTP")}} (Hypertext Transfer Protocol), che utilizza un linguaggio di verbi (come **GET**) per descrivere cosa dovrebbe accadere.
2. Se la richiesta ha successo, il server web invia una risposta HTTP al browser web contenente la risorsa richiesta.
3. In alcuni casi, la risorsa richiesta genererà ulteriori richieste HTTP, che porteranno a ulteriori risposte. Ad esempio:
   1. Quando un sito web è caricato, inizialmente viene richiesta la principale file HTML di indice della home page del sito.
   2. Quando quel file viene ricevuto dal browser, inizierà a interpretarlo e probabilmente troverà istruzioni per effettuare ulteriori richieste. Come discusso sopra, queste potrebbero riguardare file da incorporare come immagini, informazioni di stile, script e così via.
4. Quando tutte le risorse sono state richieste, il browser le analizza e le rende come richiesto, prima di visualizzare il risultato all'utente.

Questa descrizione su come funziona il web è molto semplificata, ma è tutto ciò che realmente deve sapere a questo punto. Troverà un racconto più dettagliato su come le pagine web vengono richieste e rese da un browser web nel nostro modulo [Standard web](/it/docs/Learn_web_development/Getting_started/Web_standards), leggermente più avanti.

Per ora, provi ad aprire un browser e a caricare un paio dei suoi siti preferiti, pensando ai passi sopra descritti mentre lo fa.

## Cercare informazioni

Come sviluppatore web, passerà molto tempo a cercare informazioni, dalla sintassi che non riesce a ricordare alle soluzioni a problemi specifici. È quindi una buona idea imparare a cercare efficacemente sul web.

Se sta cercando informazioni generali su una caratteristica specifica della tecnologia web, dovrebbe digitare il nome della caratteristica nella casella di ricerca di MDN. Ad esempio, provi a digitare `box model`, `fetch()` o `video element` nella casella di ricerca e vedrà cosa viene fuori. Se non trova le informazioni di cui ha bisogno, provi ad ampliare la sua ricerca — provi il suo termine di ricerca in un motore di ricerca.

Se sta cercando una soluzione a un problema specifico, come `come stampare la sequenza di fibonacci con JavaScript` o `come calcolare se un numero è un numero primo con JavaScript`, è una buona idea cercare su un sito web come [StackOverflow](https://stackoverflow.com/), che è una community dedicata a risolvere problemi di programmazione. Ancora, provi a usare un motore di ricerca generale se un sito specifico non le offre una risposta utile.

> [!CALLOUT]
>
> **Mettilo in pratica**
>
> Provi alcune ricerche, come indicato sopra:
>
> - Inizi cercando i termini esatti che abbiamo incluso sopra.
> - Successivamente, passi a cercare alcuni argomenti di suo interesse su cui desidera sapere di più. Provi a usare ricerca più o meno specifiche e termini correlati diversi per vedere cosa funziona meglio.
> - Veda i nostri [Consigli di ricerca](#consigli_di_ricerca) per altre cose da provare.

### Uso dell'IA

I risultati di ricerca generati dall'IA sono un modo molto popolare per ricevere informazioni. Offrono fondamentalmente una ricerca superpotenziata: fanno molte ricerche in sottofondo, prima di compilare i risultati in una singola risposta facilmente digeribile. Le scelte comuni sono [ChatGPT](https://chatgpt.com/), [Google Gemini](https://gemini.google.com/app), e [Microsoft Copilot](https://copilot.microsoft.com/), accessibili sia direttamente in un formato chat, sia tramite aiuto in-app potenziato dall'IA o sistemi di automazione.

Quando si impara a programmare, i prompt della chat IA possono essere utili in vari modi:

- Fare ricerche convenzionali, come gli esempi sopra.
- Capire i bug in un blocco di codice. Se si sta frustrando perché il suo codice non funziona, può incollare il suo codice in un prompt di chat IA, preceduto da una domanda come `Dov'è l'errore in questo codice?`
- Generare una versione ottimizzata di un blocco di codice specifico. Questo può essere utile quando ha scritto un blocco di codice che funziona, ma desidera scoprire come potrebbe essere fatto in modo più efficiente, o in modo più robusto che risolva più casi d'uso.
- Fornire consigli su come fare qualcosa. Ad esempio, se non desidera soltanto sapere dove si trova il bug in un blocco di codice, ma piuttosto desidera un consiglio su quale strategia adottare per eseguirne il debug.

> [!CALLOUT]
>
> **Mettilo in pratica**
>
> Provi a utilizzare un paio di strumenti IA per fare alcune ricerche.

### Una storia di avvertimento

In verità, l'IA può fare tanto che potrebbe cominciare a chiedersi perché bisogna imparare a programmare.

Ma aspetti! Il seguente è importante: **Deve ancora comprendere cosa sta cercando di fare a un livello alto, cosa sta facendo il codice, e dove ogni pezzo di codice deve essere usato**. In caso contrario, non sarà molto utile quando cercherà di risolvere problemi del mondo reale. Questo significa che deve ancora imparare a programmare. L'IA può essere uno strumento molto utile per aiutarla a trovare risposte più rapidamente, ma se digitasse ogni domanda che le viene fatta in un prompt IA, non comprenderebbe come funziona nulla.

Inoltre:

- Gli strumenti IA presentano le loro risposte in una voce sicura, autorevole, ma spesso possono essere fuorvianti o semplicemente sbagliati. Alcuni degli errori che fanno possono essere molto sottili. Non hanno alcuna intelligenza innata propria — sono fondamentalmente strumenti di riconoscimento di schemi avanzati. Gli strumenti IA compilano le loro risposte da altre fonti là fuori, quindi assorbiranno informazioni errate così come corrette. Anche due fonti corrette possono essere combinate per creare una risposta che è sbagliata.
- Le informazioni più recenti potrebbero non essere disponibili, o le risposte potrebbero essere orientate verso documentazioni più datate e prevalenti, quindi "come fare X in JS" potrebbe darle linee guida obsolete.

Di conseguenza, deve fare attenzione a controllare le risposte che le forniscono e non fidarsi di tutto senza questionare.

**Quando sta imparando, passi del tempo cercando di risolvere il problema da sola prima di cercare una risposta, sia che stia utilizzando l'IA o un motore di ricerca convenzionale. La renderà una sviluppatrice migliore.**

### Consigli di ricerca

- Deve includere il linguaggio che sta usando nel termine di ricerca, come mostrato negli esempi sopra. Se digitasse semplicemente `come stampare la sequenza di fibonacci`, probabilmente finirebbe con diverse soluzioni in Python, C++, Java, Ruby o altri linguaggi — non proprio utile quando sta cercando di imparare JavaScript!
- Quando trova una risposta utile, ne favorisca la pagina o ne faccia una copia da qualche parte in modo da poterla trovare di nuovo successivamente. Saresti sorpresa di quante volte si imbatterà negli stessi problemi.
- Se il suo codice restituisce un messaggio di errore specifico, provi a immettere l'errore in un motore di ricerca o in un prompt IA. Altre persone probabilmente hanno già affrontato lo stesso errore in passato e registrato soluzioni pubblicamente da qualche parte.
- Se possibile, rimanga con siti raccomandati come MDN e [StackOverflow](https://stackoverflow.com/).
- Esistono molte tecniche di ricerca avanzate che può utilizzare nei motori di ricerca che le daranno risultati migliori che digitare solo un termine di ricerca generico. Digitare un termine di ricerca generico come `ant fish cheese` restituirà risultati che contengono qualsiasi combinazione di quelle parole. Tuttavia, la maggior parte dei motori di ricerca supporta variazioni dei seguenti formati:

  - Digitare `"ant fish cheese"` (con le virgolette) restituirà solo risultati che contengono quella frase esatta.
  - `"ant cheese" -fish` restituirà risultati che contengono `ant` e/o `cheese` ma non `fish`.
  - `ant OR cheese` restituirà solo risultati con uno o l'altro termine, non entrambi. Dalle nostre prove, questo sembra funzionare efficacemente solo su Google.
  - `intitle:cheese` restituirà solo risultati che hanno "cheese" nel titolo principale della pagina.

  > [!NOTE]
  > Esistono molte altre tecniche che può utilizzare in vari motori di ricerca. Provi a vedere quali altre può trovare — alcune risorse utili sono [Raffinare le ricerche su Google](https://support.google.com/websearch/answer/2466433?hl=en), [Come usare la sintassi avanzata sulle ricerche DuckDuckGo](https://duckduckgo.com/duckduckgo-help-pages/results/syntax), e [Microsoft: Opzioni avanzate di ricerca](https://support.microsoft.com/en-us/topic/advanced-search-options-b92e25f1-0085-4271-bdf9-14aaea720930).

{{PreviousMenuNext("Learn_web_development/Getting_started/Environment_setup/Installing_software", "Learn_web_development/Getting_started/Environment_setup/Code_editors", "Learn_web_development/Getting_started/Environment_setup")}}
