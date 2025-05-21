---
title: Django Web Framework (Python)
slug: Learn_web_development/Extensions/Server-side/Django
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Extensions/Server-side/Django/Introduction", "Learn_web_development/Extensions/Server-side")}}

Django è un framework web server-side estremamente popolare e completo, scritto in Python. Questo modulo mostra perché Django è uno dei framework per server web più popolari, come configurare un ambiente di sviluppo e come iniziare a usarlo per creare le proprie applicazioni web.

## Prerequisiti

Prima di iniziare questo modulo non è necessario avere alcuna conoscenza di Django. Idealmente, sarebbe utile comprendere cosa siano la programmazione web server-side e i framework web leggendo gli argomenti nel nostro modulo [Primi passi con la programmazione dei siti web lato server](/it/docs/Learn_web_development/Extensions/Server-side/First_steps).

È consigliata una conoscenza generale dei concetti di programmazione e di {{Glossary("Python", "Python")}}, ma non è essenziale per comprendere i concetti principali.

> [!NOTE]
> Python è uno dei linguaggi di programmazione più facili da leggere e comprendere per i principianti. Detto ciò, se si desidera comprendere meglio questo modulo, vi sono numerosi libri e tutorial gratuiti disponibili su internet per aiutare (i nuovi programmatori potrebbero voler consultare la pagina [Python per Non Programmatori](https://wiki.python.org/moin/BeginnersGuide/NonProgrammers) sul wiki di python.org).

## Tutorial

- [Introduzione a Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/Introduction)
  - : In questo primo articolo su Django rispondiamo alla domanda "Che cos'è Django?" e forniamo una panoramica di ciò che rende speciale questo framework web. Delineeremo le caratteristiche principali, includendo alcune funzionalità avanzate che non avremo il tempo di trattare in dettaglio in questo modulo. Mostreremo anche alcuni dei blocchi di costruzione principali di un'applicazione Django, per darti un'idea di cosa può fare prima di configurarlo e iniziare a sperimentare.
- [Impostare un ambiente di sviluppo Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/development_environment)
  - : Ora che sai a cosa serve Django, ti mostreremo come configurare e testare un ambiente di sviluppo Django su Windows, Linux (Ubuntu) e macOS — qualunque sia il sistema operativo comune che stai utilizzando, questo articolo dovrebbe fornire ciò di cui hai bisogno per iniziare a sviluppare app Django.
- [Tutorial Django: Sito web della Biblioteca Locale](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website)
  - : Il primo articolo della nostra serie di tutorial pratici spiega cosa imparerai e fornisce una panoramica della "biblioteca locale" — un sito web di esempio che svilupperemo e evolveremo nei successivi articoli.
- [Django Tutorial Parte 2: Creare un sito skeleton](/it/docs/Learn_web_development/Extensions/Server-side/Django/skeleton_website)
  - : Questo articolo mostra come creare un progetto di sito "skeleton", che puoi poi popolare con impostazioni specifiche del sito, URL, modelli, viste e template.
- [Django Tutorial Parte 3: Utilizzare i modelli](/it/docs/Learn_web_development/Extensions/Server-side/Django/Models)
  - : Questo articolo mostra come definire i modelli per il sito _LocalLibrary_ — i modelli rappresentano le strutture dati in cui vogliamo memorizzare i dati dell'app, permettendo inoltre a Django di conservarli in un database per noi (e modificarli in seguito). Spiega cos'è un modello, come viene dichiarato e alcuni dei tipi di campi principali. Mostra anche brevemente alcuni dei modi principali per accedere ai dati del modello.
- [Django Tutorial Parte 4: Sito admin di Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/Admin_site)
  - : Ora che abbiamo creato i modelli per il sito _LocalLibrary_, useremo il sito Admin di Django per aggiungere alcuni dati di libri "reali". Prima ti mostreremo come registrare i modelli con il sito admin, poi ti mostreremo come effettuare il login e creare alcuni dati. Alla fine, mostreremo alcuni modi in cui puoi migliorare ulteriormente la presentazione del sito admin.
- [Django Tutorial Parte 5: Creare la nostra home page](/it/docs/Learn_web_development/Extensions/Server-side/Django/Home_page)
  - : Siamo ora pronti per aggiungere il codice per visualizzare la nostra prima pagina completa — una home page per il _LocalLibrary_ che mostra quanti record abbiamo di ciascun tipo di modello e fornisce link di navigazione nella barra laterale alle nostre altre pagine. Lungo la strada acquisiremo esperienza pratica nella scrittura di mappe di URL di base e viste, ottenendo record dal database e utilizzando i template.
- [Django Tutorial Parte 6: Viste di elenco e dettaglio generiche](/it/docs/Learn_web_development/Extensions/Server-side/Django/Generic_views)
  - : Questo tutorial estende il nostro sito _LocalLibrary_, aggiungendo pagine di elenco e dettaglio per libri e autori. Qui impareremo le viste generiche basate su classi e mostreremo come possono ridurre la quantità di codice necessario per casi d'uso comuni. Affronteremo anche in dettaglio la gestione degli URL, mostrando come eseguire il pattern matching di base.
- [Django Tutorial Parte 7: Il framework delle sessioni](/it/docs/Learn_web_development/Extensions/Server-side/Django/Sessions)
  - : Questo tutorial estende il nostro sito _LocalLibrary_, aggiungendo un contatore di visite basato su sessione alla home page. Questo è un esempio relativamente semplice, ma mostra come puoi utilizzare il framework delle sessioni per fornire un comportamento persistente per utenti anonimi sui tuoi siti.
- [Django Tutorial Parte 8: Autenticazione e permessi utente](/it/docs/Learn_web_development/Extensions/Server-side/Django/Authentication)
  - : In questo tutorial ti mostreremo come consentire agli utenti di effettuare il login al tuo sito con i propri account e come controllare quello che possono fare e vedere in base al fatto che siano o meno autenticati e ai loro _permessi_. Come parte di questa dimostrazione, estenderemo il sito _LocalLibrary_, aggiungendo pagine di login e logout, e pagine specifiche per utenti e staff per la visualizzazione dei libri presi in prestito.
- [Django Tutorial Parte 9: Lavorare con i moduli](/it/docs/Learn_web_development/Extensions/Server-side/Django/Forms)
  - : In questo tutorial ti mostreremo come lavorare con i [Formulari HTML](/it/docs/Learn_web_development/Extensions/Forms) in Django, e in particolare il modo più semplice per scrivere moduli per creare, aggiornare ed eliminare istanze di modelli. Come parte di questa dimostrazione, estenderemo il sito _LocalLibrary_ in modo che i bibliotecari possano rinnovare i libri e creare, aggiornare ed eliminare autori utilizzando i nostri moduli (anziché utilizzare l'applicazione di amministrazione).
- [Django Tutorial Parte 10: Testare un'applicazione web Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/Testing)
  - : Con la crescita dei siti web, diventa più difficile testarli manualmente — non solo c'è di più da testare, ma poiché le interazioni tra i componenti diventano più complesse, una piccola modifica in un'area può richiedere molti test aggiuntivi per verificare il suo impatto su altre aree. Un modo per mitigare questi problemi è scrivere test automatizzati, che possono essere eseguiti facilmente e in modo affidabile ogni volta che si apporta una modifica. Questo tutorial mostra come automatizzare il _test unitario_ del tuo sito web utilizzando il framework di test di Django.
- [Django Tutorial Parte 11: Deployment di Django in produzione](/it/docs/Learn_web_development/Extensions/Server-side/Django/Deployment)
  - : Ora che hai creato (e testato) un fantastico sito _LocalLibrary_, vorrai installarlo su un server web pubblico in modo che possa essere accessibile dai membri del personale e dai membri della biblioteca su internet. Questo articolo fornisce una panoramica di come potresti trovare un host per distribuire il tuo sito web e cosa devi fare per preparare il tuo sito alla produzione.
- [Sicurezza delle applicazioni web Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/web_application_security)
  - : Proteggere i dati degli utenti è una parte essenziale del design di qualsiasi sito web. Abbiamo spiegato in precedenza alcune delle minacce alla sicurezza più comuni nell'articolo [Sicurezza web](/it/docs/Web/Security) — questo articolo fornisce una dimostrazione pratica di come le protezioni integrate di Django gestiscano tali minacce.

## Valutazioni

La seguente valutazione metterà alla prova la tua comprensione di come creare un sito web usando Django, come descritto nei tutorial sopra elencati.

- [DIY Django mini blog](/it/docs/Learn_web_development/Extensions/Server-side/Django/django_assessment_blog)
  - : In questa valutazione utilizzerai alcune delle conoscenze apprese in questo modulo per creare il tuo blog.

{{NextMenu("Learn_web_development/Extensions/Server-side/Django/Introduction", "Learn_web_development/Extensions/Server-side")}}
