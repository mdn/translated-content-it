---
title: Framework Web Django (Python)
slug: Learn_web_development/Extensions/Server-side/Django
l10n:
  sourceCommit: 815f1a18f44059500b337719295c6eda14b6228e
---

{{NextMenu("Learn_web_development/Extensions/Server-side/Django/Introduction", "Learn_web_development/Extensions/Server-side")}}

Django è un framework web lato server estremamente popolare e completo, scritto in Python. Questo modulo mostra perché Django è uno dei framework per server web più diffusi, come configurare un ambiente di sviluppo e come iniziare a usarlo per creare le proprie applicazioni web.

## Prerequisiti

Prima di iniziare questo modulo non è necessario avere alcuna conoscenza di Django. Idealmente, è necessario comprendere cosa sono la programmazione web lato server e i framework web leggendo gli argomenti del nostro modulo [Primi passi nella programmazione di siti web lato server](/it/docs/Learn_web_development/Extensions/Server-side/First_steps).

È consigliata una conoscenza generale dei concetti di programmazione e di {{Glossary("Python", "Python")}}, ma non è essenziale per comprendere i concetti fondamentali.

> [!NOTE]
> Python è uno dei linguaggi di programmazione più facili da leggere e comprendere per i principianti. Detto questo, per comprendere meglio questo modulo sono disponibili su Internet numerosi libri e tutorial gratuiti (i nuovi programmatori potrebbero consultare la pagina [Python for Non Programmers](https://wiki.python.org/moin/BeginnersGuide/NonProgrammers) sul wiki di python.org).

## Tutorial

- [Introduzione a Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/Introduction)
  - : In questo primo articolo su Django rispondiamo alla domanda "Che cos'è Django?" e forniamo una panoramica di ciò che rende speciale questo framework web. Illustreremo le funzionalità principali, incluse alcune funzionalità avanzate che non sarà possibile trattare in dettaglio in questo modulo. Mostreremo inoltre alcuni dei componenti principali di un'applicazione Django, per dare un'idea di ciò che può fare prima di configurarla e iniziare a sperimentare.
- [Configurare un ambiente di sviluppo Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/development_environment)
  - : Ora che è chiaro a cosa serve Django, mostreremo come configurare e testare un ambiente di sviluppo Django su Windows, Linux (Ubuntu) e macOS: qualunque sistema operativo comune venga utilizzato, questo articolo dovrebbe fornire ciò che serve per iniziare a sviluppare app Django.
- [Tutorial Django: il sito web Local Library](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website)
  - : Il primo articolo della nostra serie di tutorial pratici spiega cosa verrà appreso e fornisce una panoramica della "biblioteca locale", un sito web di esempio su cui lavoreremo e che evolveremo negli articoli successivi.
- [Tutorial Django - Parte 2: Creare un sito web scheletro](/it/docs/Learn_web_development/Extensions/Server-side/Django/skeleton_website)
  - : Questo articolo mostra come creare un progetto di sito web "scheletro", che può poi essere popolato con impostazioni, URL, modelli, viste e template specifici del sito.
- [Tutorial Django - Parte 3: Usare i modelli](/it/docs/Learn_web_development/Extensions/Server-side/Django/Models)
  - : Questo articolo mostra come definire i modelli per il sito web _LocalLibrary_: i modelli rappresentano le strutture di dati in cui si desidera archiviare i dati dell'app e consentono inoltre a Django di memorizzare i dati in un database (e modificarli in seguito). Spiega che cos'è un modello, come viene dichiarato e alcuni dei principali tipi di campo. Mostra inoltre brevemente alcuni dei principali modi per accedere ai dati del modello.
- [Tutorial Django - Parte 4: Sito di amministrazione Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/Admin_site)
  - : Ora che sono stati creati i modelli per il sito web _LocalLibrary_, verrà usato il sito Django Admin per aggiungere alcuni dati di libri "reali". Prima verrà mostrato come registrare i modelli nel sito di amministrazione, quindi come effettuare l'accesso e creare alcuni dati. Alla fine, vengono mostrati alcuni modi per migliorare ulteriormente la presentazione del sito di amministrazione.
- [Tutorial Django - Parte 5: Creare la pagina iniziale](/it/docs/Learn_web_development/Extensions/Server-side/Django/Home_page)
  - : Ora è possibile aggiungere il codice per visualizzare la prima pagina completa: una pagina iniziale per _LocalLibrary_ che mostra quanti record sono presenti per ogni tipo di modello e fornisce collegamenti di navigazione nella barra laterale alle altre pagine. Durante il percorso verrà acquisita esperienza pratica nella scrittura di mappe URL e viste di base, nel recupero di record dal database e nell'uso dei template.
- [Tutorial Django - Parte 6: Viste generiche per elenchi e dettagli](/it/docs/Learn_web_development/Extensions/Server-side/Django/Generic_views)
  - : Questo tutorial estende il sito web _LocalLibrary_, aggiungendo pagine di elenco e dettaglio per libri e autori. Verranno illustrate le viste generiche basate su classi e mostrato come possano ridurre la quantità di codice da scrivere per casi d'uso comuni. Verrà inoltre approfondita la gestione degli URL, mostrando come eseguire il pattern matching di base.
- [Tutorial Django - Parte 7: Framework delle sessioni](/it/docs/Learn_web_development/Extensions/Server-side/Django/Sessions)
  - : Questo tutorial estende il sito web _LocalLibrary_, aggiungendo alla pagina iniziale un contatore delle visite basato sulle sessioni. Si tratta di un esempio relativamente semplice, ma mostra come usare il framework delle sessioni per fornire un comportamento persistente agli utenti anonimi dei propri siti.
- [Tutorial Django - Parte 8: Autenticazione e autorizzazioni degli utenti](/it/docs/Learn_web_development/Extensions/Server-side/Django/Authentication)
  - : In questo tutorial mostreremo come consentire agli utenti di accedere al sito con i propri account e come controllare cosa possono fare e vedere in base al fatto che abbiano effettuato o meno l'accesso e alle loro _permissions_. Nell'ambito di questa dimostrazione, estenderemo il sito web _LocalLibrary_, aggiungendo pagine di login e logout e pagine specifiche per utenti e personale per visualizzare i libri presi in prestito.
- [Tutorial Django - Parte 9: Lavorare con i moduli](/it/docs/Learn_web_development/Extensions/Server-side/Django/Forms)
  - : In questo tutorial mostreremo come lavorare con i [moduli HTML](/it/docs/Learn_web_development/Extensions/Forms) in Django e, in particolare, il modo più semplice per scrivere moduli per creare, aggiornare ed eliminare istanze di modelli. Nell'ambito di questa dimostrazione, estenderemo il sito web _LocalLibrary_ affinché i bibliotecari possano rinnovare i libri e creare, aggiornare ed eliminare autori usando i nostri moduli, anziché usare l'applicazione di amministrazione.
- [Tutorial Django - Parte 10: Testare un'applicazione web Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/Testing)
  - : Con la crescita dei siti web, testarli manualmente diventa più difficile: non solo aumenta ciò che deve essere testato, ma, poiché le interazioni tra i componenti diventano più complesse, una piccola modifica in un'area può richiedere molti test aggiuntivi per verificarne l'impatto su altre aree. Un modo per mitigare questi problemi consiste nello scrivere test automatizzati, che possono essere eseguiti facilmente e in modo affidabile ogni volta che viene apportata una modifica. Questo tutorial mostra come automatizzare il _unit testing_ del sito web usando il framework di test di Django.
- [Tutorial Django - Parte 11: Distribuire Django in produzione](/it/docs/Learn_web_development/Extensions/Server-side/Django/Deployment)
  - : Ora che è stato creato (e testato) uno straordinario sito web _LocalLibrary_, sarà necessario installarlo su un server web pubblico affinché possa essere accessibile al personale e agli iscritti della biblioteca tramite Internet. Questo articolo fornisce una panoramica su come trovare un host su cui distribuire il sito web e su cosa fare per preparare il sito alla produzione.
- [Sicurezza delle applicazioni web Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/web_application_security)
  - : Proteggere i dati degli utenti è una parte essenziale della progettazione di qualsiasi sito web. In precedenza abbiamo illustrato alcune delle minacce di sicurezza più comuni nell'articolo [Sicurezza web](/it/docs/Web/Security): questo articolo fornisce una dimostrazione pratica di come le protezioni integrate di Django gestiscono tali minacce.

## Valutazioni

La valutazione seguente metterà alla prova la comprensione di come creare un sito web usando Django, come descritto nei tutorial elencati sopra.

- [Mini blog Django fai da te](/it/docs/Learn_web_development/Extensions/Server-side/Django/django_assessment_blog)
  - : In questa valutazione verranno usate alcune delle conoscenze apprese in questo modulo per creare un blog personale.

{{NextMenu("Learn_web_development/Extensions/Server-side/Django/Introduction", "Learn_web_development/Extensions/Server-side")}}
