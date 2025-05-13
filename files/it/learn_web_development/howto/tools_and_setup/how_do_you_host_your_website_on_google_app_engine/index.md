---
title: Come fare l'hosting del suo sito web su Google App Engine
slug: Learn_web_development/Howto/Tools_and_setup/How_do_you_host_your_website_on_Google_App_Engine
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

[Google App Engine](https://cloud.google.com/appengine) è una potente piattaforma che le permette di costruire ed eseguire applicazioni sull'infrastruttura di Google — sia che abbia bisogno di costruire un'applicazione web multi-livello da zero o di ospitare un sito web statico. Ecco una guida passo-passo per effettuare l'hosting del suo sito web su Google App Engine.

## Creazione di un progetto su Google Cloud Platform

Per utilizzare gli strumenti di Google per il suo sito o app, è necessario creare un nuovo progetto su Google Cloud Platform. Questo richiede un account Google.

1. Vada al [dashboard di App Engine](https://console.cloud.google.com/projectselector/appengine) sul Google Cloud Platform Console e prema il pulsante _Crea_.
2. Se non ha mai creato un progetto prima, dovrà selezionare se desidera ricevere aggiornamenti via email o meno, accettare i Termini di servizio e poi dovrebbe essere in grado di continuare.
3. Inserisca un nome per il progetto, modifichi l'ID del progetto e lo annoti. Per questo tutorial, sono utilizzati i seguenti valori:

   - Nome del Progetto: _GAE Sample Site_
   - ID del Progetto: _gaesamplesite_

4. Clicchi sul pulsante _Crea_ per creare il suo progetto.

## Creazione di un'applicazione

Ogni progetto Cloud Platform può contenere una sola applicazione App Engine. Prepariamo un'app per il nostro progetto.

1. Avremo bisogno di un'applicazione di esempio da pubblicare. Se non ne ha una da utilizzare, scarichi e decomprima questa [app di esempio](https://gaesamplesite.appspot.com/downloads.html).
2. Dia un’occhiata alla struttura dell'applicazione di esempio — la cartella `website` contiene il contenuto del suo sito web e `app.yaml` è il file di configurazione della sua applicazione.

   - Il contenuto del suo sito web deve andare all'interno della cartella `website`, e la sua pagina di atterraggio deve chiamarsi `index.html`, ma a parte questo può avere qualsiasi forma le piaccia.
   - Il file `app.yaml` è un file di configurazione che informa App Engine su come mappare gli URL ai suoi file statici. Non è necessario modificarlo.

## Pubblicazione della sua applicazione

Ora che abbiamo creato il nostro progetto e raccolto i file dell'app di esempio, pubblichiamo la nostra app.

1. Apra [Google Cloud Shell](https://shell.cloud.google.com/).
2. Trascini e rilasci la cartella `sample-app` nel pannello a sinistra dell'editor di codice.
3. Esegua il seguente comando nella riga di comando per selezionare il suo progetto:

   ```bash
   gcloud config set project gaesamplesite
   ```

4. Poi esegua il seguente comando per accedere alla directory della sua app:

   ```bash
   cd sample-app
   ```

5. Ora è pronto per distribuire la sua applicazione, cioè caricare la sua app su App Engine:

   ```bash
   gcloud app deploy
   ```

6. Inserisca un numero per scegliere la regione in cui vuole che la sua applicazione sia situata.
7. Inserisca `Y` per confermare.
8. Ora navighi il suo browser su _your-project-id_.appspot.com per vedere il suo sito web online. Ad esempio, per l'ID del progetto _gaesamplesite_, vada su [_gaesamplesite_.appspot.com](https://gaesamplesite.appspot.com/).

## Veda anche

Per saperne di più, veda la [Documentazione di Google App Engine](https://cloud.google.com/appengine/docs/).
