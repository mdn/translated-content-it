---
title: Django Web Framework (Python)
slug: Learn_web_development/Extensions/Server-side/Django
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Extensions/Server-side/Django/Introduction", "Learn_web_development/Extensions/Server-side")}}

Django è un framework web server-side estremamente popolare e completo, scritto in Python. Questo modulo mostra perché Django è uno dei framework web server-side più popolari, come impostare un ambiente di sviluppo e come iniziare a usarlo per creare le proprie applicazioni web.

## Prerequisiti

Prima di iniziare questo modulo, non è necessario avere conoscenze di Django. Idealmente, sarebbe utile comprendere cosa siano la programmazione web lato server e i framework web leggendo gli argomenti nel nostro modulo [Primi passi nella programmazione di siti web lato server](/it/docs/Learn_web_development/Extensions/Server-side/First_steps).

È consigliata una conoscenza generale dei concetti di programmazione e di {{Glossary("Python", "Python")}}, ma non è essenziale per comprendere i concetti principali.

> [!NOTE]
> Python è uno dei linguaggi di programmazione più facili da leggere e capire per i principianti. Detto ciò, se desidera comprendere meglio questo modulo, ci sono numerosi libri e tutorial gratuiti disponibili su internet per aiutarla (i nuovi programmatori potrebbero voler controllare la pagina [Python for Non Programmers](https://wiki.python.org/moin/BeginnersGuide/NonProgrammers) sul wiki di python.org).

## Tutorial

- [Introduzione a Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/Introduction)
  - : In questo primo articolo su Django rispondiamo alla domanda "Cos'è Django?" e forniamo una panoramica di ciò che rende speciale questo framework web. Illustrieremo le caratteristiche principali, comprese alcune funzionalità avanzate che non avremo tempo di trattare in dettaglio in questo modulo. Mostreremo anche alcuni dei principali elementi costitutivi di un'applicazione Django, per darle un'idea di ciò che può fare prima di installarlo e iniziare a sperimentare.
- [Impostazione di un ambiente di sviluppo Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/development_environment)
  - : Ora che sa a cosa serve Django, le mostreremo come impostare e testare un ambiente di sviluppo Django su Windows, Linux (Ubuntu) e macOS — qualunque sistema operativo comunemente utilizzato, questo articolo dovrebbe fornirle ciò di cui ha bisogno per iniziare a sviluppare app Django.
- [Tutorial Django: Il sito web della Biblioteca Locale](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website)
  - : Il primo articolo della nostra serie di tutorial pratici spiega cosa imparerà e fornisce una panoramica della "biblioteca locale" — un sito web di esempio che analizzeremo e svilupperemo in articoli successivi.
- [Tutorial Django Parte 2: Creare un sito web scheletro](/it/docs/Learn_web_development/Extensions/Server-side/Django/skeleton_website)
  - : Questo articolo mostra come creare un progetto di sito web "scheletro", che può poi essere popolato con impostazioni specifiche del sito, URL, modelli, viste e template.
- [Tutorial Django Parte 3: Utilizzare i modelli](/it/docs/Learn_web_development/Extensions/Server-side/Django/Models)
  - : Questo articolo mostra come definire modelli per il sito web _LocalLibrary_ — i modelli rappresentano le strutture dati in cui vogliamo memorizzare i dati della nostra app, e permettono anche a Django di memorizzare i dati in un database per noi (e modificarli in seguito). Spiega cosa sia un modello, come si dichiara e alcuni dei principali tipi di campo. Mostra anche brevemente alcuni dei modi principali per accedere ai dati dei modelli.
- [Tutorial Django Parte 4: Il sito amministrativo di Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/Admin_site)
  - : Ora che abbiamo creato i modelli per il sito web _LocalLibrary_, utilizzeremo il sito Admin di Django per aggiungere alcuni dati "reali" sui libri. Per prima cosa, mostreremo come registrare i modelli nel sito amministrativo, poi mostreremo come accedere e creare alcuni dati. Alla fine, mostreremo alcuni modi per migliorare ulteriormente la presentazione del sito amministrativo.
- [Tutorial Django Parte 5: Creare la nostra pagina iniziale](/it/docs/Learn_web_development/Extensions/Server-side/Django/Home_page)
  - : Siamo ora pronti ad aggiungere il codice per visualizzare la nostra prima pagina completa — una pagina iniziale per il _LocalLibrary_ che mostra quanti record abbiamo di ciascun tipo di modello e fornisce link di navigazione laterali alle nostre altre pagine. Lungo il percorso, acquisiremo esperienza pratica nella scrittura di mappe URL e viste di base, nell'ottenere record dal database e nell'utilizzare i template.
- [Tutorial Django Parte 6: Visite generiche di elenco e dettaglio](/it/docs/Learn_web_development/Extensions/Server-side/Django/Generic_views)
  - : Questo tutorial estende il nostro sito _LocalLibrary_, aggiungendo pagine di elenco e dettaglio per libri e autori. Qui impareremo a conoscere le viste generiche basate su classi e mostreremo come possono ridurre la quantità di codice da scrivere per casi d'uso comuni. Inoltre, approfondiremo la gestione degli URL, mostrando come effettuare il pattern matching di base.
- [Tutorial Django Parte 7: Framework delle sessioni](/it/docs/Learn_web_development/Extensions/Server-side/Django/Sessions)
  - : Questo tutorial estende il nostro sito _LocalLibrary_, aggiungendo un contatore di visite basato su sessione alla pagina iniziale. Questo è un esempio relativamente semplice, ma dimostra come il framework delle sessioni possa essere utilizzato per fornire un comportamento persistente per utenti anonimi sui propri siti.
- [Tutorial Django Parte 8: Autenticazione e permessi utente](/it/docs/Learn_web_development/Extensions/Server-side/Django/Authentication)
  - : In questo tutorial mostreremo come consentire agli utenti di accedere al suo sito con i propri account e come controllare ciò che possono fare e vedere in base al fatto che siano o meno connessi e ai loro _permessi_. Come parte di questa dimostrazione, estenderemo il sito _LocalLibrary_, aggiungendo pagine di accesso e disconnessione, e pagine specifiche per utenti e personale per visualizzare i libri presi in prestito.
- [Tutorial Django Parte 9: Lavorare con i moduli](/it/docs/Learn_web_development/Extensions/Server-side/Django/Forms)
  - : In questo tutorial le mostreremo come lavorare con i [Moduli HTML](/it/docs/Learn_web_development/Extensions/Forms) in Django, e in particolare il modo più semplice per scrivere moduli per creare, aggiornare ed eliminare istanze di modelli. Come parte di questa dimostrazione, estenderemo il sito _LocalLibrary_ in modo che i bibliotecari possano rinnovare i libri e creare, aggiornare ed eliminare autori utilizzando i nostri moduli (anziché utilizzare l'applicazione di amministrazione).
- [Tutorial Django Parte 10: Testare un'applicazione web Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/Testing)
  - : Man mano che i siti web crescono, diventa più difficile testarli manualmente — non solo c'è di più da testare, ma, man mano che le interazioni tra i componenti si fanno più complesse, una piccola modifica in un'area può richiedere molti test aggiuntivi per verificarne l'impatto su altre aree. Un modo per mitigare questi problemi è scrivere test automatizzati, che possono essere eseguiti facilmente e in modo affidabile ogni volta che si fa una modifica. Questo tutorial mostra come automatizzare il _unit testing_ del suo sito web utilizzando il framework di test di Django.
- [Tutorial Django Parte 11: Distribuire Django in produzione](/it/docs/Learn_web_development/Extensions/Server-side/Django/Deployment)
  - : Ora che ha creato (e testato) un fantastico sito web _LocalLibrary_, vorrà installarlo su un server web pubblico in modo che possa essere accessibile dallo staff della biblioteca e dai membri su internet. Questo articolo fornisce una panoramica di come potrebbe procedere per trovare un host per distribuire il suo sito web e cosa bisogna fare per preparare il suo sito per la produzione.
- [Sicurezza delle applicazioni web Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/web_application_security)
  - : Proteggere i dati degli utenti è una parte essenziale di qualsiasi design di sito web. Abbiamo spiegato in precedenza alcune delle minacce alla sicurezza più comuni nell'articolo [Sicurezza web](/it/docs/Web/Security) — questo articolo fornisce una dimostrazione pratica di come le protezioni integrate di Django gestiscano tali minacce.

## Valutazioni

La seguente valutazione testerà la sua comprensione di come creare un sito web utilizzando Django, come descritto nei tutorial elencati sopra.

- [Fai da te: Mini blog Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/django_assessment_blog)
  - : In questa valutazione utilizzerà alcune delle conoscenze acquisite da questo modulo per creare il suo blog.

{{NextMenu("Learn_web_development/Extensions/Server-side/Django/Introduction", "Learn_web_development/Extensions/Server-side")}}
