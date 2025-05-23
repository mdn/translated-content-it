---
title: Come ospitare il tuo sito web su Google App Engine?
slug: Learn_web_development/Howto/Tools_and_setup/How_do_you_host_your_website_on_Google_App_Engine
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

[Google App Engine](https://cloud.google.com/appengine) è una piattaforma potente che consente di costruire ed eseguire applicazioni sull'infrastruttura di Google — sia che si desideri creare un'applicazione web multi-livello da zero o ospitare un sito web statico. Ecco una guida passo-passo per ospitare il tuo sito web su Google App Engine.

## Creazione di un progetto su Google Cloud Platform

Per utilizzare gli strumenti di Google per il tuo sito o app, è necessario creare un nuovo progetto su Google Cloud Platform. Questo richiede di avere un account Google.

1. Vai alla [dashboard di App Engine](https://console.cloud.google.com/projectselector/appengine) sulla Google Cloud Platform Console e premi il pulsante _Crea_.
2. Se non hai mai creato un progetto prima, dovrai selezionare se desideri ricevere aggiornamenti via email o meno, accettare i Termini di servizio, e poi dovresti essere in grado di continuare.
3. Inserisci un nome per il progetto, modifica il tuo ID progetto e annotalo. Per questo tutorial, verranno utilizzati i seguenti valori:

   - Nome del progetto: _GAE Sample Site_
   - ID del progetto: _gaesamplesite_

4. Clicca sul pulsante _Crea_ per creare il tuo progetto.

## Creazione di un'applicazione

Ogni progetto Cloud Platform può contenere un'applicazione App Engine. Prepariamo un'app per il nostro progetto.

1. Avremo bisogno di un'applicazione di esempio da pubblicare. Se non ne hai una da usare, scarica e decomprimi questa [app di esempio](https://gaesamplesite.appspot.com/downloads.html).
2. Dai un'occhiata alla struttura dell'applicazione di esempio — la cartella `website` contiene il contenuto del tuo sito web e `app.yaml` è il file di configurazione della tua applicazione.

   - Il contenuto del tuo sito web deve andare all'interno della cartella `website` e la sua pagina di atterraggio deve essere chiamata `index.html`, ma a parte questo può assumere qualsiasi forma desideri.
   - Il file `app.yaml` è un file di configurazione che dice ad App Engine come mappare gli URL ai tuoi file statici. Non hai bisogno di modificarlo.

## Pubblicazione della tua applicazione

Ora che abbiamo creato il nostro progetto e raccolto i file dell'app di esempio, pubblichiamo la nostra app.

1. Apri [Google Cloud Shell](https://shell.cloud.google.com/).
2. Trascina e rilascia la cartella `sample-app` nel riquadro a sinistra dell'editor di codice.
3. Esegui il seguente comando nella riga di comando per selezionare il tuo progetto:

   ```bash
   gcloud config set project gaesamplesite
   ```

4. Poi esegui il seguente comando per andare nella directory della tua app:

   ```bash
   cd sample-app
   ```

5. Ora sei pronto per distribuire la tua applicazione, cioè caricare la tua app su App Engine:

   ```bash
   gcloud app deploy
   ```

6. Inserisci un numero per scegliere la regione in cui vuoi che sia posizionata la tua applicazione.
7. Inserisci `Y` per confermare.
8. Ora naviga nel tuo browser su _your-project-id_.appspot.com per vedere il tuo sito web online. Ad esempio, per l'ID progetto _gaesamplesite_, vai su [_gaesamplesite_.appspot.com](https://gaesamplesite.appspot.com/).

## Vedi anche

Per saperne di più, consulta la [documentazione di Google App Engine](https://cloud.google.com/appengine/docs/).
