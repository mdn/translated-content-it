---
title: Costruire applicazioni Angular e ulteriori risorse
slug: Learn_web_development/Core/Frameworks_libraries/Angular_building
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenu("Learn_web_development/Core/Frameworks_libraries/Angular_filtering", "Learn_web_development/Core/Frameworks_libraries")}}

Questo articolo finale su Angular copre come costruire un'app pronta per la produzione e fornisce ulteriori risorse per continuare il suo percorso di apprendimento.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con le lingue di base <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>,
        conoscenza della
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
          >terminale/linea di comando</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Imparare come costruire la sua app Angular.</td>
    </tr>
  </tbody>
</table>

## Costruire la sua applicazione finale

Una volta terminato lo sviluppo della sua applicazione, può eseguire il comando `build` dell'Angular CLI.
Quando esegue il comando `build` nella sua directory `todo`, la sua applicazione viene compilata in una directory di output chiamata `dist/`.

Nella directory `todo`, esegua il seguente comando nella linea di comando:

```bash
ng build -c production
```

La CLI compila l'applicazione e inserisce l'output in una nuova directory `dist`.
Il flag `--configuration production`/`-c production` con `ng build` elimina ciò che non è necessario per la produzione.

## Distribuire la sua applicazione

Per distribuire la sua applicazione, può copiare il contenuto della cartella `dist/my-project-name` sul suo server web.
Poiché questi file sono statici, può ospitarli su qualsiasi server web in grado di servire file, come:

- Node.js
- Java
- .NET

Può utilizzare qualsiasi backend come [Firebase](https://firebase.google.com/docs/hosting), [Google Cloud](https://cloud.google.com/solutions/web-hosting) o [App Engine](https://cloud.google.com/appengine/docs/standard/hosting-a-static-website).

### Ospitare localmente

Per divertimento, può ospitare l'app costruita sul suo computer usando il pacchetto [`http-server`](https://www.npmjs.com/package/http-server) eseguendo il seguente comando dopo aver eseguito una costruzione:

```bash
npx http-server ./dist/todo/browser/ -o
```

Questo comando serve la directory `dist/todo/browser` sulla porta `8080` in modo che possa aprire `http://127.0.0.1:8080` nel suo browser per vedere l'app in esecuzione.
Il server HTTP le consente anche di accedere all'app all'indirizzo IP del suo computer da qualsiasi altro dispositivo sulla sua rete locale, e questo indirizzo è indicato sotto l'indirizzo `127.0.0.1` nella console.

## Cosa fare dopo

A questo punto, ha costruito un'applicazione di base, ma il suo viaggio con Angular è appena iniziato.
Può saperne di più esplorando la documentazione di Angular, come:

- [Tutorials](https://angular.dev/tutorials): Un tutorial approfondito che evidenzia le funzionalità di Angular, come l'uso dei servizi, la navigazione e l'ottenimento di dati da un server.
- Le guide ai [Components](https://angular.dev/guide/components) di Angular: Una serie di articoli che coprono argomenti come il ciclo di vita, l'interazione tra componenti e l'incapsulamento delle viste.
- Le guide ai [Forms](https://angular.dev/guide/forms): Articoli che la guidano nella costruzione di form reattivi in Angular, nella validazione dell'input e nella costruzione di form dinamici.

## Riepilogo

Per ora è tutto. Speriamo che si sia divertito con Angular!

{{PreviousMenu("Learn_web_development/Core/Frameworks_libraries/Angular_filtering", "Learn_web_development/Core/Frameworks_libraries")}}
