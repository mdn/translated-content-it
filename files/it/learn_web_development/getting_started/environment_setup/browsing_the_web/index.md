---
title: Navigare nel web
slug: Learn_web_development/Getting_started/Environment_setup/Browsing_the_web
l10n:
  sourceCommit: be1922d62a0d31e4e3441db0e943aed8df736481
---

{{PreviousMenuNext("Learn_web_development/Getting_started/Environment_setup/Installing_software", "Learn_web_development/Getting_started/Environment_setup/Code_editors", "Learn_web_development/Getting_started/Environment_setup")}}

A questo punto nel modulo, dovresti aver installato più browser web moderni sul tuo computer o altri dispositivi disponibili. Questo articolo approfondisce l'uso dei browser, esaminando come funziona un browser web, la differenza tra alcune delle cose di tutti i giorni con cui interagirai e come cercare informazioni.

> [!NOTE]
> Se non hai installato alcun browser oltre a quelli predefiniti forniti con i tuoi dispositivi, installane altri. Consulta [Browser web moderni](/it/docs/Learn_web_development/Getting_started/Environment_setup/Installing_software#modern_web_browsers) per maggiori informazioni.

Come in ogni area di conoscenza, anche il web viene con una moltitudine di gergo e terminologia tecnica. Non preoccuparti: non ti sommergeremo con tutto fin dall'inizio (puoi consultare il [glossario](/it/docs/Glossary) se sei curioso). Tuttavia, ci sono alcuni termini di base che devi capire fin dall'inizio poiché sentirai queste espressioni spesso. Introduciamo alcuni termini importanti di seguito.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con il sistema operativo del tuo computer.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
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

Inizieremo descrivendo vari concetti legati al web: pagine web, siti web, server web e motori di ricerca. Questi termini sono spesso confusi dai neofiti del web o vengono utilizzati in modo errato. Assicuriamoci di sapere cosa significano ciascuno di essi! Iniziamo con alcune definizioni:

- **Pagina web**
  - : Un documento che può essere visualizzato in un {{Glossary("browser", "browser")}} web. Questi sono spesso chiamati semplicemente "pagine". Tali documenti sono scritti nel linguaggio {{Glossary("HTML", "HTML")}} (che esamineremo in dettaglio più avanti).
- **Sito web**
  - : Una raccolta di pagine web raggruppate insieme in una singola risorsa, con collegamenti che le connettono insieme. Spesso chiamato "sito".
- **Server web**
  - : Un computer che ospita un sito web su Internet.
- **Servizio web**
  - : Un software che risponde a richieste su Internet per eseguire una funzione o fornire dati. Un servizio web è tipicamente supportato da un server web e può fornire pagine web con cui gli utenti possono interagire. Molti siti web sono anche servizi web, sebbene alcuni siti, come MDN, siano composti solo da contenuti statici. Esempi di servizi web potrebbero essere qualcosa che ridimensiona le immagini, fornisce un report meteorologico, o gestisce l'accesso degli utenti.
- **Motore di ricerca**
  - : Un servizio web che ti aiuta a trovare altre pagine web, come Google, Bing, Yahoo, o DuckDuckGo. I motori di ricerca sono normalmente accessibili tramite un browser web (ad esempio, puoi eseguire ricerche nel motore di ricerca direttamente nella barra degli indirizzi di Firefox, Chrome, ecc.) o tramite una pagina web (ad esempio, [bing.com](https://www.bing.com/) o [duckduckgo.com](https://duckduckgo.com/)).

Guardiamo un'analogia: una biblioteca pubblica. Questo è generalmente ciò che faresti visitando una biblioteca:

1. Trova un indice di ricerca e cerca il titolo del libro che desideri.
2. Prendi nota del numero di catalogo del libro.
3. Vai alla sezione particolare contenente il libro, trova il numero di catalogo giusto e prendi il libro.

Confrontiamo una biblioteca pubblica con il web:

- La biblioteca è come un server web. Ha diverse sezioni, simile a un server web che ospita più siti web.
- Le diverse sezioni (scienza, matematica, storia, ecc.) nella biblioteca sono come siti web. Ogni sezione è come un sito web unico (due sezioni non contengono gli stessi libri).
- I libri in ogni sezione sono come pagine web. Un sito web potrebbe avere diverse pagine web, per esempio, la sezione Scienza (il sito web) avrà libri su calore, suono, termodinamica, statica, ecc.
- L'indice di ricerca è come il motore di ricerca. Ogni libro ha la sua posizione unica nella biblioteca (due libri non possono essere tenuti nello stesso posto) specificata dal numero di catalogo.

Prendiamo ora il tempo di esaminare ogni termine in modo un po' più dettagliato.

### Pagina web

Una **pagina web** è un semplice documento visualizzabile da un browser. Una pagina web può includere una varietà di diversi tipi di risorse come:

- _Informazioni sullo stile_ — controllo dell'aspetto e della sensazione di una pagina.
- _Script_ — che aggiungono interattività alla pagina.
- _Media_ — immagini, suoni e video.

> [!NOTE]
> I browser possono anche visualizzare altri documenti come file {{Glossary("PDF", "PDF")}} e altre risorse come immagini o video, ma il termine **pagina web** si riferisce specificamente ai documenti HTML.

Tutte le pagine web possono essere trovate ciascuna in una posizione unica (indirizzo web, chiamato anche {{Glossary("URL", "URL")}}). Per accedere a una pagina, basta digitare il suo indirizzo nella barra degli indirizzi del browser:

![Esempio di un indirizzo di pagina web nella barra degli indirizzi del browser](web-page.jpg)

> [!CALLOUT]
>
> **Provalo**
>
> Prova a caricare uno dei tuoi siti preferiti in un browser ora.

### Sito web

Un _sito web_ è una raccolta di pagine web collegate (più le loro risorse associate) che condividono un unico [nome di dominio](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_domain_name). Ogni pagina web di un dato sito fornisce link espliciti—la maggior parte delle volte sotto forma di parti di testo cliccabili—che consentono all'utente di spostarsi da una pagina del sito all'altra.

Quando carichi il tuo sito preferito in un browser, tende a visualizzare prima la pagina principale del sito web, o _homepage_ (chiamata casualmente "home"):

![Esempio di un nome di dominio di un sito web nella barra degli indirizzi del browser](web-site.jpg)

> [!CALLOUT]
>
> **Provalo**
>
> Prova a cliccare alcuni elementi del menu o link per visualizzare alcune pagine diverse sul tuo sito preferito.

> [!NOTE]
> È anche possibile avere un {{Glossary("SPA", "_app a pagina unica_")}}: un sito web che consiste di una singola pagina web che viene aggiornata dinamicamente con nuovi contenuti quando necessario.

### Server web

Un _server web_ è un computer che ospita uno o più _siti web_. "Ospitare" significa che tutte le _pagine web_ e i loro file associati sono disponibili su quel computer. Il _server web_ invierà i file delle pagine web che sta ospitando al browser di un utente quando essi tentano di caricarli.

Non confondere _siti web_ e _server web_. Ad esempio, se senti qualcuno dire, "Il mio sito non risponde", significa in realtà che il _server web_ non risponde e quindi il _sito web_ non è disponibile. Più importante, poiché un server web può ospitare più siti, il termine _server web_ non è mai usato per designare un sito web, poiché potrebbe causare una grande confusione. Nel nostro esempio precedente, se avessimo detto, "Il mio server web non risponde", significa che più siti su quel server web non sono disponibili.

### Motore di ricerca

I motori di ricerca sono una fonte comune di confusione sul web. Un motore di ricerca è un tipo speciale di sito web che aiuta gli utenti a trovare pagine web da _altri_ siti.

Ce ne sono molti là fuori: [Google](https://www.google.com/), [Bing](https://www.bing.com/), [Yandex](https://yandex.com/), [DuckDuckGo](https://duckduckgo.com/), e molti altri. Alcuni sono generici, altri sono specializzati intorno a certi argomenti.

Molti principianti sul web confondono i motori di ricerca con i browser. Facciamo chiarezza: Un _browser_ è un software che recupera e visualizza pagine web; un _motore di ricerca_ è un sito web che aiuta le persone a trovare pagine web da altri siti. La confusione nasce perché, la prima volta che qualcuno lancia un browser, spesso il browser visualizza la homepage di un motore di ricerca o una casella di ricerca che consente loro di cercare un termine usando quel motore di ricerca. La maggior parte dei browser consente anche ai propri utenti di utilizzare un motore di ricerca digitando i termini di ricerca direttamente nella barra degli indirizzi del browser.

Questo ha senso perché la prima cosa che la gente tende a voler fare con un browser è trovare una pagina web da visualizzare. Non confondere il software (il browser) con il servizio (il motore di ricerca).

Ecco un esempio di Firefox che mostra una casella di ricerca di Google come la sua pagina di avvio predefinita:

![Esempio di Firefox nightly che visualizza una pagina personalizzata di Google come predefinita](search-engine.jpg)

> [!CALLOUT]
>
> **Provalo**
>
> Esegui una ricerca in un motore di ricerca:
>
> - Visitando una homepage di un motore di ricerca e inserendo un termine di ricerca.
> - Inserendo un termine di ricerca nella barra degli indirizzi del browser.

## Come funziona il web: le basi

In molte parti del mondo, il web è diventato uno strumento essenziale per la nostra vita quotidiana, così come le posate, le biciclette e le automobili, o gli spazzolini da denti. Se questo ti sembra irrealistico, pensa a quante volte utilizzi un sito web o un'app per smartphone ogni giorno! Anche se non stai digitando un indirizzo web in un browser per accedere a contenuti o servizi, è probabile che l'app che stai usando stia probabilmente usando la tecnologia web dietro le quinte per raccogliere dati da presentarti.

Quando accedi al web, succede molto tra la tua prima interazione (ad esempio, digitare un indirizzo web (URL) in un browser e premere <kbd>Enter</kbd>/<kbd>Return</kbd>) e il risultato della tua azione che viene presentato a te (ad esempio, il sito web che appare nel tuo browser web):

1. Il browser web richiede la risorsa (ad esempio, una pagina web, alcuni dati, o un'immagine o video) che vuoi accedere dal server web su cui è memorizzata. Tali richieste (e le risposte risultanti) sono effettuate utilizzando una tecnologia chiamata {{Glossary("HTTP", "HTTP")}} (Hypertext Transfer Protocol), che utilizza un linguaggio di verbi (come **GET**) per descrivere cosa dovrebbe accadere.
2. Se la richiesta ha successo, il server web invia una risposta HTTP al browser web contenente la risorsa richiesta.
3. In alcuni casi, la risorsa richiesta attiverà ulteriori richieste HTTP, che porteranno a più risposte. Ad esempio:
   1. Quando un sito web viene caricato, inizialmente viene richiesta la file index HTML principale della home page del sito.
   2. Quando quel file viene ricevuto dal browser, inizierà a analizzarlo e probabilmente troverà istruzioni per fare ulteriori richieste. Come discusso sopra, queste potrebbero essere per file da incorporare come immagini, informazioni sullo stile, script, e così via.
4. Quando tutte le risorse sono state richieste, il browser web le analizza e le renderizza come richiesto, prima di visualizzare il risultato all'utente.

Questa descrizione di come funziona il web è fortemente semplificata, ma è tutto ciò che devi sapere per ora. Troverai una spiegazione più dettagliata di come le pagine web vengono richieste e renderizzate da un browser web nel nostro modulo [Standard web](/it/docs/Learn_web_development/Getting_started/Web_standards), un po' più avanti.

Per ora, prova ad aprire un browser web e caricare un paio dei tuoi siti preferiti, pensando ai passaggi sopra mentre lo fai.

## Cercare informazioni

Come sviluppatore web, trascorrerai molto tempo a cercare informazioni, da sintassi che non riesci a ricordare a soluzioni per problemi specifici. È quindi una buona idea imparare come cercare efficacemente sul web.

Se stai cercando informazioni generali su una specifica funzionalità della tecnologia web, dovresti digitare il nome della funzionalità nella casella di ricerca di MDN. Ad esempio, prova a digitare `box model`, `fetch()` o `video element` nella casella di ricerca e vedi cosa esce. Se non trovi le informazioni di cui hai bisogno, prova ad ampliare la tua ricerca — prova il tuo termine di ricerca in un motore di ricerca.

Se stai cercando una soluzione a un problema specifico, come `come stampare la sequenza di Fibonacci con JavaScript` o `come calcolare se un numero è un numero primo con JavaScript`, è una buona idea cercare su un sito web come [StackOverflow](https://stackoverflow.com/), che è una comunità dedicata a rispondere a problemi di programmazione. Ancora una volta, prova a utilizzare un motore di ricerca generale se un sito specifico non ti dà una risposta utile.

> [!CALLOUT]
>
> **Provalo**
>
> Prova alcune ricerche, come indicato sopra:
>
> - Inizia cercando i termini esatti che abbiamo incluso sopra.
> - Poi, passa a cercare alcuni argomenti personali di cui vuoi saperne di più. Prova a utilizzare ricerche più e meno specifiche e termini correlati diversi per vedere cosa funziona meglio.
> - Vedi il nostro [Consigli per la ricerca](#consigli_per_la_ricerca) per altre cose da provare.

### Utilizzo dell'IA

I risultati di ricerca generati dall'IA sono un modo molto popolare per ricevere informazioni. Forniscono fondamentalmente una ricerca superpotente: fanno molte ricerche in background, prima di compilare i risultati in una risposta unica e facilmente digeribile. Le scelte comuni sono [ChatGPT](https://chatgpt.com/), [Google Gemini](https://gemini.google.com/app), e [Microsoft Copilot](https://copilot.microsoft.com/), accessibili direttamente in un formato chat, o tramite aiuto in-app potenziato dall'IA o sistemi di automazione.

Quando si impara a programmare, i prompt di chat IA possono essere utili in vari modi:

- Eseguendo ricerche convenzionali, come gli esempi sopra.
- Capire i bug in un blocco di codice. Se ti stai frustrando perché il tuo codice non funziona, puoi incollare il tuo codice in un prompt di chat IA, preceduto da una domanda come `Dov'è l'errore in questo codice?`
- Generare una versione ottimizzata di un blocco di codice specifico. Questo può essere utile quando hai scritto un blocco di codice che funziona, ma vuoi scoprire come potrebbe essere fatto in modo più efficiente, o in un modo più robusto che risolve più casi d'uso.
- Fornire consigli su come fare qualcosa. Ad esempio, se non vuoi solo sapere dove si trova il bug in un blocco di codice, ma invece vuoi consigli su quale strategia utilizzare per eseguirne il debug.

> [!CALLOUT]
>
> **Provalo**
>
> Prova a utilizzare un paio di strumenti IA per fare alcune ricerche.

### Una storia ammonitrice

In realtà, l'IA può fare così tanto che potresti cominciare a chiederti perché hai bisogno di imparare a programmare.

Ma aspetta! Il seguente è importante: **Devi comunque capire cosa stai cercando di fare a un livello alto, cosa fa il codice e dove ciascun pezzo di codice deve essere usato**. Se non lo fai, non sarai molto utile quando cercherai di risolvere problemi del mondo reale. Ciò significa che devi comunque imparare a programmare. L'IA può essere uno strumento davvero utile per aiutarti a trovare le risposte più rapidamente, ma se digiti ogni domanda che ti viene posta in un prompt IA, non capirai come funziona nulla.

Inoltre:

- Gli strumenti IA presentano le loro risposte in un tono sicuro e autorevole, ma possono spesso essere fuorvianti o semplicemente sbagliati. Alcuni degli errori che fanno possono essere molto sottili. Non hanno alcuna intelligenza innata — sono fondamentalmente strumenti avanzati di corrispondenza dei modelli. Gli strumenti IA compilano le loro risposte da altre fonti lì fuori, quindi assorbono informazioni errate così come informazioni corrette. Anche due fonti corrette possono essere combinate per creare una risposta scorretta.
- Le informazioni più recenti potrebbero non essere disponibili, o le risposte potrebbero essere orientate alla documentazione più vecchia e più prevalente, quindi "come fare X in JS" potrebbe darti una guida obsoleta.

Di conseguenza, è necessario prestare attenzione a verificare le risposte che ti danno, e non fidarti ciecamente di tutto senza fare domande.

**Quando stai imparando, trascorri del tempo a cercare di risolvere il problema da solo prima di cercare una risposta, sia che stai usando l'IA o un motore di ricerca convenzionale. Ti renderà un programmatore migliore.**

### Consigli per la ricerca

- Dovresti includere il linguaggio che stai usando nel termine di ricerca, come mostrato negli esempi sopra. Se scrivi solo `how to print out the fibonacci sequence`, probabilmente finisci con diverse soluzioni in Python, C++, Java, Ruby, o altri linguaggi — non proprio utile quando stai cercando di imparare JavaScript!
- Quando trovi una risposta utile, mettici un segnalibro o fanne una copia da qualche parte in modo da poterla ritrovare più tardi. Sarai sorpreso di quante volte ti imbatti nello stesso problema.
- Se il tuo codice restituisce un messaggio di errore specifico, prova a inserire l'errore in un motore di ricerca o in un prompt IA. Altre persone probabilmente hanno già affrontato lo stesso errore in passato e hanno registrato soluzioni pubblicamente da qualche parte.
- Se possibile, rimani su siti raccomandati come MDN e [StackOverflow](https://stackoverflow.com/).
- Ci sono molte tecniche di ricerca avanzate che puoi usare nei motori di ricerca che ti daranno risultati migliori rispetto a digitare solo un termine di ricerca semplice. Digitando un termine di ricerca semplice come `ant fish cheese` restituirà risultati che contengono qualsiasi combinazione di quelle parole. Tuttavia, la maggior parte dei motori di ricerca supporta variazioni dei seguenti formati:

  - Digitando `"ant fish cheese"` (con le virgolette) restituirà solo risultati che contengono quella frase esatta.
  - `"ant cheese" -fish` restituirà risultati che contengono `ant` e/o `cheese` ma non `fish`.
  - `ant OR cheese` restituirà solo risultati con un termine o l'altro, non entrambi. Dai nostri test, questo sembrava funzionare efficacemente solo su Google.
  - `intitle:cheese` restituirà solo risultati che hanno "cheese" nel titolo principale della pagina.

  > [!NOTE]
  > Ci sono molte altre tecniche che puoi usare in vari motori di ricerca diversi. Prova a vedere quali altre puoi trovare — alcune risorse utili sono [Raffina le ricerche su Google](https://support.google.com/websearch/answer/2466433?hl=en), [Come usare la sintassi avanzata su DuckDuckGo Search](https://duckduckgo.com/duckduckgo-help-pages/results/syntax), e [Microsoft: Opzioni di ricerca avanzata](https://support.microsoft.com/en-us/topic/advanced-search-options-b92e25f1-0085-4271-bdf9-14aaea720930).

{{PreviousMenuNext("Learn_web_development/Getting_started/Environment_setup/Installing_software", "Learn_web_development/Getting_started/Environment_setup/Code_editors", "Learn_web_development/Getting_started/Environment_setup")}}
