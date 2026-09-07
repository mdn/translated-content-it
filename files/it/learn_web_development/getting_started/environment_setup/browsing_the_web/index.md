---
title: Navigare sul web
slug: Learn_web_development/Getting_started/Environment_setup/Browsing_the_web
l10n:
  sourceCommit: e81cf36acffe197d01b1ad282c3582ebd7b0b54d
---

{{PreviousMenuNext("Learn_web_development/Getting_started/Environment_setup/Installing_software", "Learn_web_development/Getting_started/Environment_setup/Code_editors", "Learn_web_development/Getting_started/Environment_setup")}}

A questo punto del modulo, dovrebbero essere installati sul computer o su altri dispositivi disponibili più browser web moderni. Questo articolo approfondisce l'uso dei browser, esaminando come funziona un browser web, la differenza tra alcuni degli elementi quotidiani con cui si interagisce e come cercare informazioni.

> [!NOTE]
> Se non sono installati browser oltre a quelli predefiniti forniti con i dispositivi, installarne altri. Consultare [Browser web moderni](/it/docs/Learn_web_development/Getting_started/Environment_setup/Installing_software#modern_web_browsers) per ulteriori informazioni.

Come ogni ambito del sapere, il web include molto gergo e terminologia tecnica. Non c'è da preoccuparsi: non verrà presentato tutto subito (se si è curiosi, è possibile consultare il [glossario](/it/docs/Glossary)). Tuttavia, ci sono alcuni termini di base che è necessario comprendere fin dall'inizio, poiché queste espressioni verranno usate continuamente. Di seguito vengono introdotti alcuni termini importanti.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con il sistema operativo del computer.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>La differenza tra un browser web, un sito web e un motore di ricerca.</li>
          <li>Come funziona un browser web a livello di base.</li>
          <li>La ricerca di informazioni.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## La differenza tra pagina web, sito web, server web e motore di ricerca

Inizieremo descrivendo vari concetti relativi al web: pagine web, siti web, server web e motori di ricerca. Questi termini vengono spesso confusi da chi è alle prime armi con il web o usati in modo scorretto. Vediamo quindi cosa significano! Iniziamo con alcune definizioni:

- **Pagina web**
  - : Un documento che può essere visualizzato in un {{Glossary("browser", "browser")}} web. Spesso sono chiamate semplicemente "pagine". Tali documenti sono scritti nel linguaggio {{Glossary("HTML", "HTML")}} (che verrà approfondito più avanti).
- **Sito web**
  - : Una raccolta di pagine web raggruppate in una singola risorsa, collegate tra loro tramite link. Spesso chiamato "sito".
- **Server web**
  - : Un computer che ospita un sito web su Internet.
- **Servizio web**
  - : Un software che risponde a richieste su Internet per eseguire una funzione o fornire dati. Un servizio web è in genere supportato da un server web e può fornire pagine web con cui gli utenti possono interagire. Molti siti web sono anche servizi web, sebbene alcuni siti web (come MDN) consistano solo di contenuto statico. Esempi di servizi web sono un servizio che ridimensiona immagini, fornisce previsioni meteorologiche o gestisce l'accesso degli utenti.
- **Motore di ricerca**
  - : Un servizio web che aiuta a trovare altre pagine web, come Google, Bing, Yahoo o DuckDuckGo. Normalmente si accede ai motori di ricerca tramite un browser web (ad esempio, è possibile effettuare ricerche direttamente nella barra degli indirizzi di Firefox, Chrome e così via) oppure tramite una pagina web (ad esempio, [bing.com](https://www.bing.com/) o [duckduckgo.com](https://duckduckgo.com/)).

Consideriamo un'analogia: una biblioteca pubblica. Ecco cosa si farebbe generalmente visitando una biblioteca:

1. Trovare un indice di ricerca e cercare il titolo del libro desiderato.
2. Annotare il numero di catalogo del libro.
3. Andare nella sezione che contiene il libro, trovare il numero di catalogo corretto e prendere il libro.

Confrontiamo una biblioteca pubblica con il web:

- La biblioteca è come un server web. Ha diverse sezioni, analogamente a un server web che ospita più siti web.
- Le diverse sezioni (scienze, matematica, storia e così via) della biblioteca sono come siti web. Ogni sezione è come un sito web univoco (due sezioni non contengono gli stessi libri).
- I libri in ciascuna sezione sono come pagine web. Un sito web può avere diverse pagine web; per esempio, la sezione Scienze (il sito web) avrà libri su calore, suono, termodinamica, biologia umana e così via.
- L'indice di ricerca è come il motore di ricerca. Ogni libro ha una posizione univoca nella biblioteca (due libri non possono essere conservati nello stesso posto), specificata dal numero di catalogo.

Ora esaminiamo ciascun termine un po' più nel dettaglio.

### Pagina web

Una **pagina web** è un semplice documento visualizzabile da un browser. Una pagina web può incorporare vari tipi di risorse, quali:

- _Informazioni di stile_ — controllano l'aspetto della pagina.
- _Script_ — aggiungono interattività alla pagina.
- _Media_ — immagini, suoni e video.

> [!NOTE]
> I browser possono visualizzare anche altri documenti, come file {{Glossary("PDF", "PDF")}}, e altre risorse come immagini o video, ma il termine **pagina web** si riferisce specificamente ai documenti HTML.

Ogni pagina web si trova in una posizione univoca (indirizzo web, chiamato anche {{Glossary("URL", "URL")}}). Per accedere a una pagina, basta digitare il suo indirizzo nella barra degli indirizzi del browser:

![Esempio di indirizzo di una pagina web nella barra degli indirizzi del browser](web-page.jpg)

Provare ora a caricare uno dei siti web preferiti in un browser, tenendo presente quanto detto sopra. L'indirizzo web è stato digitato manualmente oppure è stato trovato tramite un motore di ricerca?

### Sito web

Un _sito web_ è una raccolta di pagine web collegate (oltre alle risorse associate) che condividono un [nome di dominio](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_domain_name) univoco. Ogni pagina web di un determinato sito web fornisce link espliciti, nella maggior parte dei casi sotto forma di porzioni di testo cliccabili, che consentono all'utente di spostarsi da una pagina del sito web a un'altra.

Quando si carica un sito web preferito in un browser, questo tende a visualizzare per prima cosa la pagina web principale del sito, ovvero la _homepage_ (spesso chiamata semplicemente "home"):

![Esempio di nome di dominio di un sito web nella barra degli indirizzi del browser](web-site.jpg)

Provare a fare clic su alcune voci di menu o link per visualizzare pagine diverse del sito web preferito. Notare come l'indirizzo web visualizzato cambia quando ci si sposta tra le pagine.

> [!NOTE]
> È anche possibile avere una {{Glossary("SPA", "_single-page app_")}}: un sito web costituito da una singola pagina web aggiornata dinamicamente con nuovi contenuti quando necessario. In questo caso, l'indirizzo web potrebbe non cambiare durante la visualizzazione di pagine diverse.

### Server web

Un _server web_ è un computer che ospita uno o più _siti web_. "Ospitare" significa che tutte le _pagine web_ e i file associati sono disponibili su quel computer. Il _server web_ invierà al browser di un utente i file delle pagine web che ospita quando l'utente tenta di caricarle.

Non confondere i _siti web_ con i _server web_. Per esempio, se qualcuno dice "Il mio sito web non risponde", probabilmente significa che il _server web_ non risponde e quindi il _sito web_ non è disponibile.

Ancora più importante, poiché un server web può ospitare più siti web, il termine _server web_ non viene più usato per indicare un sito web, in quanto potrebbe causare confusione. Se qualcuno dice "Il mio server web non risponde", può darsi che più siti web o applicazioni ospitati sul server web non siano disponibili.

### Motore di ricerca

È comune confondere i motori di ricerca con i siti web. Un motore di ricerca è un tipo speciale di servizio web che aiuta gli utenti a trovare le pagine web di loro interesse, oltre a tipi specifici di contenuti come immagini, video o articoli di notizie.

Tutti i motori di ricerca tendono ad avere siti web propri che possono essere usati per accedere al servizio web sottostante. Ne esistono molti: [Google](https://www.google.com/), [Bing](https://www.bing.com/), [Yandex](https://yandex.com/), [DuckDuckGo](https://duckduckgo.com/) e molti altri. Alcuni sono generici, altri sono specializzati su determinati argomenti.

Molti principianti del web confondono motori di ricerca e browser. Chiariamo:

- Un _browser_ è un software che recupera e visualizza pagine web.
- Un _motore di ricerca_ è un servizio web (e, di solito, un sito web) che aiuta le persone a trovare pagine web contenute in altri siti web.

La confusione nasce perché, la prima volta che qualcuno avvia un browser, il browser mostra spesso la homepage del sito web di un motore di ricerca oppure una casella di ricerca che consente di cercare un termine usando quel motore di ricerca. La maggior parte dei browser consente inoltre agli utenti di usare un motore di ricerca digitando termini di ricerca direttamente nella barra degli indirizzi del browser.

Tutto questo ha senso perché la prima cosa che generalmente si desidera fare con un browser è trovare una pagina web da visualizzare. Non confondere il software (il browser) con il servizio (il motore di ricerca).

Ecco un esempio di Firefox che mostra una casella di ricerca Google come pagina di avvio predefinita:

![Esempio di Firefox nightly che visualizza una pagina Google personalizzata come predefinita](search-engine.jpg)

Provare a utilizzare un motore di ricerca per trovare informazioni su un argomento di interesse:

1. Andare alla homepage di un motore di ricerca e inserire un termine di ricerca.
2. Inserire un termine di ricerca nella barra degli indirizzi del browser.

## Come funziona il web: le basi

In molte parti del mondo, il web è diventato uno strumento essenziale per la vita quotidiana tanto quanto le posate, le biciclette, le automobili o gli spazzolini da denti. Se sembra poco realistico, basta pensare a quanto spesso viene usato un sito web o un'app per telefono cellulare ogni giorno. Anche se non viene digitato un indirizzo web in un browser web per accedere a contenuti o servizi, è probabile che l'app in uso utilizzi tecnologia web dietro le quinte per recuperare dati da presentare.

Quando si accede al web, accadono molte cose tra la prima interazione (per esempio, digitare un indirizzo web (URL) in un browser e premere <kbd>Invio</kbd>/<kbd>Return</kbd>) e la presentazione del risultato dell'azione (per esempio, la comparsa del sito web nel browser):

1. Il browser web richiede la risorsa a cui si desidera accedere (per esempio, una pagina web, alcuni dati, un'immagine o un video) al server web su cui è archiviata. Tali richieste (e le relative risposte) vengono effettuate usando una tecnologia chiamata {{Glossary("HTTP", "HTTP")}} (Hypertext Transfer Protocol), che usa un linguaggio di verbi (come **GET**) per descrivere cosa deve accadere.
2. Se la richiesta ha successo, il server web invia una risposta HTTP al browser web contenente la risorsa richiesta.
3. In alcuni casi, la risorsa richiesta attiverà ulteriori richieste HTTP, che produrranno altre risposte. Per esempio:
   1. Quando viene caricato un sito web, inizialmente viene richiesto il file HTML principale dell'indice della home page del sito.
   2. Quando il browser riceve quel file, inizierà ad analizzarlo e probabilmente troverà istruzioni per effettuare altre richieste. Come discusso sopra, queste potrebbero riguardare file da incorporare, come immagini, informazioni di stile, script e così via.
4. Quando tutte le risorse sono state richieste, il browser web le analizza ed esegue il rendering secondo necessità, prima di mostrare il risultato all'utente.

Questa descrizione di come funziona il web è molto semplificata, ma per il momento è tutto ciò che è necessario sapere. Una descrizione più dettagliata di come le pagine web vengono richieste ed eseguite dal rendering di un browser web si trova nel modulo [Standard web](/it/docs/Learn_web_development/Getting_started/Web_standards), poco più avanti.

## Cercare informazioni

Come sviluppatore web, verrà dedicato molto tempo alla ricerca di informazioni, dalla sintassi che non si ricorda alle soluzioni per problemi specifici. È quindi una buona idea imparare a cercare efficacemente sul web.

Se si conosce un sito web specializzato nell'argomento che si sta imparando, spesso è una buona idea iniziare da lì.

Per esempio, se si cercano informazioni generali su una funzionalità specifica della tecnologia web, digitare il nome della funzionalità nella casella di ricerca di MDN. Per esempio, provare a digitare `box model`, `fetch()` o `video element` nella casella di ricerca e vedere cosa viene restituito. Se non si trovano le informazioni necessarie, ampliare la ricerca: provare il termine di ricerca in un motore di ricerca.

Se si cerca una soluzione a un problema specifico, come `how to print out the fibonacci sequence with JavaScript` o `how to calculate whether a number is a prime number with JavaScript`, è una buona idea cercare su un sito web come [Stack Overflow](https://stackoverflow.com/), una community dedicata a rispondere a problemi di programmazione. Anche in questo caso, provare a usare un motore di ricerca generico se un sito specifico non fornisce una risposta utile.

Prima di proseguire, provare a cercare alcuni argomenti che si desidera approfondire. Provare a usare ricerche più o meno specifiche e termini correlati diversi per vedere cosa funziona meglio. Consultare i nostri [suggerimenti per la ricerca](#suggerimenti_per_la_ricerca) per altre cose da provare.

### Usare l'IA

I risultati di ricerca generati dall'IA sono un modo molto diffuso per ricevere informazioni. In sostanza, forniscono una ricerca potenziata: effettuano molte ricerche in background prima di compilare i risultati in un'unica risposta facilmente assimilabile. Scelte comuni sono [ChatGPT](https://chatgpt.com/), [Google Gemini](https://gemini.google.com/app) e [Microsoft Copilot](https://copilot.microsoft.com/), a cui si accede direttamente in formato chat oppure tramite sistemi di aiuto o automazione nelle app basati sull'IA.

Durante l'apprendimento della programmazione, i prompt di chat dell'IA possono essere utili in vari modi:

- Effettuare ricerche convenzionali, come gli esempi riportati sopra.
- Individuare bug in un blocco di codice. Se il codice non funziona e si prova frustrazione, è possibile incollare il codice in un prompt di chat dell'IA, preceduto da una domanda come `Where is the mistake in this code?`
- Generare una versione ottimizzata di un blocco di codice specifico. Può essere utile quando è stato scritto un blocco di codice funzionante, ma si desidera scoprire come potrebbe essere realizzato in modo più efficiente o più robusto, così da risolvere più casi d'uso.
- Fornire consigli su come fare qualcosa. Per esempio, se non si vuole solo sapere dove si trova il bug in un blocco di codice, ma si desiderano invece consigli sulla strategia da usare per eseguire il debug.

Provare a usare un paio di strumenti di IA per effettuare alcune ricerche.

### Un avvertimento

L'IA può fare così tanto da far sorgere il dubbio sul perché sia necessario imparare a programmare.

Ma attenzione! Quanto segue è importante: **È comunque necessario comprendere a livello generale ciò che si sta cercando di fare, cosa fa il codice e dove deve essere usato ogni pezzo di codice**. Altrimenti, non si sarà molto utili nel tentativo di risolvere problemi del mondo reale. Ciò significa che è comunque necessario imparare a programmare. L'IA può essere uno strumento davvero utile per trovare risposte più rapidamente, ma se ogni domanda ricevuta viene semplicemente digitata in un prompt dell'IA, non si capirà come funziona nulla.

Inoltre:

- Gli strumenti di IA presentano le risposte con un tono sicuro e autorevole, ma spesso possono essere fuorvianti o semplicemente errati. Alcuni degli errori possono essere molto sottili. Non hanno alcuna intelligenza innata: sono fondamentalmente strumenti avanzati di riconoscimento dei modelli. Gli strumenti di IA compilano le loro risposte a partire da altre fonti disponibili, quindi raccolgono sia informazioni errate sia informazioni corrette. Anche due fonti corrette possono essere combinate per creare una risposta errata.
- Le informazioni più recenti potrebbero non essere disponibili, oppure le risposte potrebbero essere orientate verso documentazione più vecchia e diffusa; pertanto, cercare "how to do X in JS" potrebbe fornire indicazioni obsolete.

Di conseguenza, è necessario verificare attentamente le risposte fornite e non fidarsi di tutto senza porsi domande.

**Durante l'apprendimento, dedicare tempo a cercare di risolvere il problema autonomamente prima di cercare una risposta, sia usando l'IA sia un motore di ricerca convenzionale. Questo renderà lo sviluppatore migliore.**

### Suggerimenti per la ricerca

- Includere il linguaggio usato nel termine di ricerca, come mostrato negli esempi precedenti. Se si digitasse soltanto `how to print out the fibonacci sequence`, si finirebbe probabilmente con diverse soluzioni in Python, C++, Java, Ruby o altri linguaggi, non molto utili quando si sta cercando di imparare JavaScript.
- Quando si trova una risposta utile, aggiungerla ai segnalibri o salvarne una copia da qualche parte, così da poterla ritrovare in seguito. Sorprenderà quante volte si incontrerà lo stesso problema.
- Se il codice restituisce un messaggio di errore specifico, provare a inserire l'errore in un motore di ricerca o in un prompt dell'IA. Altre persone avranno probabilmente già affrontato lo stesso errore in passato e registrato pubblicamente le soluzioni da qualche parte.
- Se possibile, attenersi a siti consigliati come MDN e [Stack Overflow](https://stackoverflow.com/).
- Esistono molte tecniche di ricerca avanzate che è possibile usare nei motori di ricerca e che forniranno risultati migliori rispetto alla semplice digitazione di un termine di ricerca. Digitare un semplice termine di ricerca come `ant fish cheese` restituirà risultati che contengono qualsiasi combinazione di tali parole. Tuttavia, la maggior parte dei motori di ricerca supporta variazioni dei seguenti modelli di sintassi:
  - Digitare `"ant fish cheese"` (con le virgolette) restituirà solo risultati che contengono quella frase esatta.
  - `ant cheese -fish` restituirà risultati che contengono `ant` e/o `cheese`, ma non `fish`.
  - `ant OR cheese` restituirà solo risultati con un termine o l'altro, non entrambi. Dai nostri test, questo sembrava funzionare efficacemente solo in Google.
  - `intitle:cheese` restituirà solo risultati che hanno "cheese" nel titolo principale della pagina.

  > [!NOTE]
  > Esistono molte altre tecniche utilizzabili nei vari motori di ricerca. Provare a scoprire quali altre sono disponibili: alcune risorse utili sono [Refine Google Searches](https://support.google.com/websearch/answer/2466433?hl=en), [How to use advanced syntax on DuckDuckGo Search](https://duckduckgo.com/duckduckgo-help-pages/results/syntax) e [Microsoft: Advanced search options](https://support.microsoft.com/en-US/bing/advanced-search-options).

{{PreviousMenuNext("Learn_web_development/Getting_started/Environment_setup/Installing_software", "Learn_web_development/Getting_started/Environment_setup/Code_editors", "Learn_web_development/Getting_started/Environment_setup")}}
