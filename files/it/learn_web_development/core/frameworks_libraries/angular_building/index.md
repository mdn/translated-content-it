---
title: Creare applicazioni Angular e ulteriori risorse
slug: Learn_web_development/Core/Frameworks_libraries/Angular_building
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenu("Learn_web_development/Core/Frameworks_libraries/Angular_filtering", "Learn_web_development/Core/Frameworks_libraries")}}

Questo ultimo articolo su Angular copre come costruire un'app pronta per la produzione e fornisce ulteriori risorse per continuare il tuo percorso di apprendimento.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi di base <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, conoscenza del
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
          >terminale/linea di comando</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Imparare come costruire la tua app Angular.</td>
    </tr>
  </tbody>
</table>

## Creare la tua applicazione finita

Ora che hai finito di sviluppare la tua applicazione, puoi eseguire il comando `build` della CLI di Angular.
Quando esegui il comando `build` nella tua directory `todo`, la tua applicazione viene compilata in una directory di output chiamata `dist/`.

Nella directory `todo`, esegui il seguente comando in linea di comando:

```bash
ng build -c production
```

La CLI compila l'applicazione e mette l'output in una nuova directory `dist`.
Il flag `--configuration production`/`-c production` con `ng build` elimina ciò di cui non hai bisogno per la produzione.

## Distribuire la tua applicazione

Per distribuire la tua applicazione, puoi copiare i contenuti della cartella `dist/my-project-name` sul tuo server web.
Poiché questi file sono statici, puoi ospitarli su qualsiasi server web in grado di servire file, come:

- Node.js
- Java
- .NET

Puoi usare qualsiasi backend come [Firebase](https://firebase.google.com/docs/hosting), [Google Cloud](https://cloud.google.com/solutions/web-hosting) o [App Engine](https://cloud.google.com/appengine/docs/standard/hosting-a-static-website).

### Ospitare localmente

Per divertimento, puoi ospitare l'app costruita sul tuo computer usando il pacchetto [`http-server`](https://www.npmjs.com/package/http-server) eseguendo il seguente comando dopo aver eseguito una build:

```bash
npx http-server ./dist/todo/browser/ -o
```

Questo comando serve la directory `dist/todo/browser` sulla porta `8080` quindi puoi aprire `http://127.0.0.1:8080` nel tuo browser per vedere l'app in esecuzione.
Il server HTTP ti permette anche di accedere all'app all'indirizzo IP del tuo computer da qualsiasi altro dispositivo sulla tua rete locale, e questo indirizzo è elencato sotto l'indirizzo `127.0.0.1` nella console.

## Cosa fare dopo

A questo punto, hai costruito un'applicazione base, ma il tuo percorso con Angular è appena iniziato.
Puoi imparare di più esplorando la documentazione di Angular, come:

- [Tutorials](https://angular.dev/tutorials): Un tutorial approfondito che evidenzia le funzionalità di Angular, come l'uso dei servizi, la navigazione e l'acquisizione di dati da un server.
- Le guide [Components](https://angular.dev/guide/components) di Angular: Una serie di articoli che coprono argomenti come cicli di vita, interazione tra componenti e incapsulamento delle viste.
- Le guide [Forms](https://angular.dev/guide/forms): Articoli che ti guidano attraverso la costruzione di form reattivi in Angular, la validazione degli input e la costruzione di form dinamici.

## Sommario

Ecco fatto per ora. Speriamo che ti sia divertito con Angular!

{{PreviousMenu("Learn_web_development/Core/Frameworks_libraries/Angular_filtering", "Learn_web_development/Core/Frameworks_libraries")}}
