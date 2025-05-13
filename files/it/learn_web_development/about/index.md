---
title: Informazioni su Learn web development
slug: Learn_web_development/About
l10n:
  sourceCommit: be1922d62a0d31e4e3441db0e943aed8df736481
---

MDN Learn web development mira a insegnare le competenze e le conoscenze fondamentali che un sviluppatore web front-end dovrebbe avere per essere impiegabile e mantenere longevità nell'industria odierna del web. Incorpora i valori che riteniamo il web dovrebbe avere — accessibilità, sostenibilità, usabilità, prestazioni e comunità. Ci piacerebbe che educatori, sviluppatori e studenti utilizzassero questa risorsa e promuovessero questi valori nel loro lavoro, nel loro insegnamento e nei prodotti che costruiscono.

Questo contenuto è stato creato dalla comunità MDN con revisione e feedback da esperti all'interno di Mozilla e dell'intera comunità web. Grazie per il vostro prezioso contributo; sapete chi siete!

## Contesto e motivazione

Abbiamo originariamente [lanciato la sezione MDN Learn Web Development nel 2016](https://hacks.mozilla.org/2016/06/learning-to-code-for-the-web-the-mdn-learning-area-welcomes-you/) con l'obiettivo di rendere MDN più accessibile ai non esperti e aiutare i nuovi sviluppatori web a passare da "principiante a competente".

Il contenuto ha avuto un discreto successo, ma, andando avanti alcuni anni, abbiamo notato che la struttura era sotto la media. I principianti desiderano davvero un percorso solido che possano seguire per ottenere le conoscenze di cui hanno bisogno, piuttosto che dover capire da soli cosa imparare e quando.

Inoltre, Mozilla parla tutti i giorni con professionisti del settore, e riceviamo regolarmente feedback sulle lacune di conoscenza dei nuovi assunti. I responsabili delle assunzioni osservano spesso:

- Troppa enfasi sull'uso di framework per costruire app web rapidamente, accoppiata a una mancanza di comprensione delle tecnologie sottostanti dietro questi framework. Ciò porta a una mancanza di abilità nella risoluzione dei problemi e a una minore impiegabilità a lungo termine con il cambiamento degli strumenti.
- Una mancanza delle migliori pratiche fondamentali come semantica, accessibilità e design responsivo. Ciò si traduce in una mancanza di attenzione all'utente, portando a limitazioni nell'usabilità.
- Lacune nella conoscenza di come funzionano fondamentalmente i browser, come presentano le informazioni e l'interattività che si ottiene gratuitamente. Questo provoca soluzioni eccessivamente complesse e spesso inaccessibili.
- Limitate capacità di risoluzione dei problemi, lavoro di squadra, ricerca e altre importanti competenze trasversali.

Di conseguenza, abbiamo creato un curriculum per aiutare le persone a imparare un insieme di competenze migliori, renderle più impiegabili e permettere loro di costruire un web di domani migliore, più accessibile e più responsabile. Vogliamo che abbiano la migliore possibilità di successo. Abbiamo [lanciato il Curriculum MDN all'inizio del 2024](/en-US/blog/mdn-curriculum-launch/).

Tuttavia, abbiamo presto ricevuto feedback che gli utenti trovavano confuso avere due risorse di apprendimento su MDN, con il curriculum/percorso di apprendimento da una parte e il contenuto didattico dall'altra. Di conseguenza, abbiamo [integrato il Curriculum nell'area di apprendimento a dicembre 2024](/it/docs/Learn_web_development/Changelog#december_2024).

## Pubblico di riferimento

### Studenti

Questo curriculum è utile per diversi gruppi di studenti:

- Studenti che vogliono trovare lavoro nell'industria, il che potrebbe comportare l'ottenimento di una qualifica o certificazione correlata. Il curriculum fungerà da guida per ciò che dovrebbero studiare.
- Sviluppatori web esistenti che vogliono "migliorare" le loro competenze, assicurandosi che il loro insieme di abilità sia attuale e identificando le lacune nelle loro conoscenze che dovrebbero approfondire.
- Sviluppatori non front-end che hanno esperienza di sviluppo in altre aree (per esempio sviluppatori web back-end o sviluppatori specifici per piattaforma), che desiderano entrare nello sviluppo web front-end e vogliono una guida sui temi da apprendere.

### Educatori

Gli educatori possono utilizzare questo contenuto come guida nella creazione di programmi, unità e specifiche di valutazione per un corso universitario, un corso di college, un corso di scuola di programmazione o simili legati al web. Conformandosi agli obiettivi di apprendimento nei nostri articoli, sarà possibile assicurarsi che i corsi insegnino tecniche attuali e migliori pratiche, evitando cattive pratiche e informazioni obsolete.

Per saperne di più, consultare la nostra pagina [Risorse per Educatori](/it/docs/Learn_web_development/Educators).

> [!NOTE]
> Il Curriculum completo di MDN Learn Web Development è disponibile in un comodo PDF da condividere con i suoi studenti e colleghi. [Scarica il Curriculum](https://github.com/mdn/curriculum/releases/latest/download/MDN-Curriculum.pdf).

## Ambito

Il termine _sviluppatore front-end_ può essere ambiguo; può significare cose diverse per persone diverse, e si può aspettare che coloro che lavorano sul front-end svolgano una vasta gamma di compiti diversi.

### Cosa viene coperto

Questo set di articoli non tenta di insegnare ogni argomento che uno sviluppatore web potrebbe verosimilmente essere tenuto a conoscere in profondità. Il curriculum copre i seguenti aspetti:

- Competenze tecniche di base come HTML semantico, CSS e i fondamenti di JavaScript.
- Migliori pratiche come accessibilità, design responsivo e teoria del design dell'interfaccia utente.
- Strumenti chiave come framework e controllo di versione.
- Competenze trasversali per promuovere la mentalità e l'attitudine necessarie per ottenere un lavoro.
- Conoscenze ambientali come computer e sistemi di file, navigazione web, basi della riga di comando ed editor di codice.
- Diverse "estensioni" che riteniamo costituiscano competenze aggiuntive utili da apprendere man mano che gli sviluppatori iniziano ad ampliare le loro conoscenze e sviluppano specializzazioni. Questo include:
  - Trasformazioni CSS e animazione
  - Categorie comuni di Web API (ad esempio, media, grafica e archiviazione lato client)
  - Fondamenti di sviluppo web lato server
  - Prestazioni
  - Sicurezza e privacy
  - Test

### Livello di dettaglio

Gli argomenti presentati sono coperti a diversi livelli di dettaglio.

- Alcuni sono trattati in profondità, ad esempio, i fondamenti di HTML e CSS. È importante avere una chiara comprensione di questi prima che uno studente prosegua troppo nel suo percorso di apprendimento.
- Alcuni sono coperti più superficialmente, ad esempio, il controllo di versione o i test. È importante capire cosa siano questi argomenti e iniziare con alcune basi, ma queste tipologie di competenze possono essere ampliate man mano che si prosegue nella carriera.

### Cosa non viene coperto

Ci sono anche diverse aree che esplicitamente non copriamo in questo curriculum, ovvero:

- Copertura esaustiva di linguaggi/piattaforme back-end. Forniamo una breve introduzione in [Node.js (Express)](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs) e [Python (Django)](/it/docs/Learn_web_development/Extensions/Server-side/Django) in quanto è utile a ogni sviluppatore web capire come funzionano HTTP e le tecnologie lato server. Tuttavia, non forniamo una copertura esaustiva su più piattaforme; sarebbe al di fuori dell'ambito di MDN.
- Copertura dettagliata dei database relazionali tradizionali (ad esempio, [MySQL](https://dev.mysql.com/doc/) o [Postgres](https://www.postgresql.org/)) e di altri archivi di dati lato server (ad esempio, database cloud come [MongoDB](https://www.mongodb.com/) o [Google Cloud Datastore](https://cloud.google.com/products/datastore)). Forniamo una breve introduzione a tali tecnologie nei nostri moduli [Programmazione lato server](/it/docs/Learn_web_development/Extensions/Server-side).
- Approfondimento dei temi DevOps come le piattaforme cloud per il provisioning e l'automazione (ad esempio, [Amazon AWS](https://aws.amazon.com/), [Google Cloud Platform](https://console.cloud.google.com/) e [Microsoft Azure](https://azure.microsoft.com/)) e gli strumenti di containerizzazione (ad esempio, [Kubernetes](https://kubernetes.io/) e [Docker](https://www.docker.com/)). Accenniamo leggermente ad alcuni strumenti che sono considerati nello spazio DevOps — come GitHub e strumenti di test automatizzati — ma questi hanno un distinto crossover nello spazio dello sviluppatore front-end.
- Design grafico oltre le conoscenze di base delineate in [Design per sviluppatori](/it/docs/Learn_web_development/Core/Design_for_developers).
- Competenze legate a ruoli come gestione del prodotto e del programma (ad esempio, organizzazione, ricerca e pianificazione).

## Attribuzione

Questa risorsa è gratuita per chiunque voglia utilizzarla. Se la trova utile, le chiediamo di considerare di fare quanto segue:

- Collegarsi a essa. Ad esempio, un educatore potrebbe includere quanto segue nelle informazioni pubbliche del suo programma:

  ```html
  <p>
    This course is based on
    <a href="https://developer.mozilla.org/en-US/curriculum/"
      >MDN Learn Web Development</a
    >.
  </p>
  ```

- Ne parli con gli altri! Vorremmo che quanti più studenti e educatori possibile iniziassero a utilizzare questo materiale e convergere attorno a esso come standard per la conoscenza di base dello sviluppatore web.

> [!NOTE]
> Gli educatori dovrebbero utilizzare questo materiale come guida, ma il suo uso non implica un'approvazione da parte di Mozilla.

## Processo di aggiornamento

L'industria dello sviluppo web cambia costantemente e rapidamente. Per mantenere le nostre raccomandazioni attuali, rivedremo il nostro materiale regolarmente, aggiorneremo il nostro [changelog](/it/docs/Learn_web_development/Changelog), e faremo un annuncio ogni anno, contattando i creatori di corsi riconosciuti come conformi per informarli che il corso è cambiato e incoraggiarli a rivedere/aggiornare i loro corsi secondo quanto necessario.

Intendiamo farlo nel Q2 di ogni anno, per dare agli educatori il tempo durante Q2/Q3 per implementare le modifiche prima dell'inizio dell'anno accademico successivo.

## Domande frequenti

### Domande sulla collaborazione con Scrimba

#### Come fa MDN a sapere che i corsi di Scrimba sono di alta qualità e seguono le migliori pratiche?

Scrimba aveva già una grande reputazione prima che iniziassimo a parlare con loro di una partnership. Tuttavia, non ci siamo affidati solo alla parola della comunità. Abbiamo eseguito una revisione approfondita del [Frontend Developer Career Path](https://scrimba.com/the-frontend-developer-career-path-c0j:details?via=mdn) (FDCP) di Scrimba e abbiamo fornito loro feedback su possibili miglioramenti, concentrandoci sull'aumento della copertura delle migliori pratiche e della conformità ai nostri [Moduli principali](/it/docs/Learn_web_development/Core). Scrimba ha implementato tutti i nostri feedback, e l'FDCP è ancora migliore di prima. Ora che è conforme al nostro Curriculum Core, siamo sicuri che sia allineato agli standard MDN.

#### MDN condivide i dati degli utenti con Scrimba?

Diamo priorità alla privacy degli utenti e alla trasparenza. L'unica informazione che MDN condivide con Scrimba è la navigazione dell'utente verso Scrimba da MDN, e questo avviene tramite le proprie azioni seguendo un link marcato come esterno.

Nei casi in cui incorporiamo contenuti di Scrimba su MDN, Scrimba non vedrà i dati dell'utente fino a quando l'utente non sceglierà di interagire con il contenuto di Scrimba.

#### Il contenuto di Scrimba non è gratuito. Non è in conflitto con la filosofia di MDN di fornire contenuto gratuito?

Gran parte del contenuto di Scrimba richiede un abbonamento a pagamento, ma offrono anche diversi corsi completi che sono accessibili gratuitamente dopo la registrazione.

Vale anche la pena notare che i corsi di Scrimba non sono necessari per utilizzare MDN Learn Web Development — sono un miglioramento per chi desidera pagare per un corso strutturato che copra il nostro curriculum core. È ancora possibile apprendere tutti i nostri risultati di apprendimento gratuitamente lavorando attraverso i nostri articoli.

#### Viene rilasciato un certificato al completamento del Frontend Developer Career Path di Scrimba?

Sì, una volta completati tutti gli argomenti nel Frontend Developer Career Path, è possibile accedere a un certificato di completamento per condividerlo con potenziali datori di lavoro o includerlo nel suo portfolio. Veda [Where can I find my completion certificate?](https://forum.scrimba.com/t/where-can-i-find-my-completion-certificate/43?via=mdn) per maggiori informazioni.
