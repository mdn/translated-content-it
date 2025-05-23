---
title: Come caricare i file su un server web?
slug: Learn_web_development/Howto/Tools_and_setup/Upload_files_to_a_web_server
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

Questo articolo mostra come pubblicare il tuo sito online utilizzando strumenti di trasferimento file.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Devi sapere
        <a href="/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_web_server"
          >cos'è un server web</a
        >
        e
        <a href="/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_domain_name"
          >come funzionano i nomi di dominio</a
        >. Devi anche sapere come
        <a
          href="/it/docs/Learn_web_development/Howto/Tools_and_setup/set_up_a_local_testing_server"
          >configurare un ambiente di base</a
        >
        e come
        <a href="/it/docs/Learn_web_development/Getting_started/Your_first_website"
          >scrivere una semplice pagina web</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare come inviare file a un server utilizzando i vari strumenti di trasferimento file disponibili.
      </td>
    </tr>
  </tbody>
</table>

## Sommario

Se hai creato una semplice pagina web (vedi [Nozioni di base su HTML](/it/docs/Learn_web_development/Getting_started/Your_first_website/Creating_the_content) per un esempio), probabilmente desideri metterla online, su un server web. In questo articolo discuteremo come farlo, utilizzando varie opzioni disponibili come client SFTP, RSync e GitHub.

## SFTP

Esistono numerosi client SFTP. Il nostro esempio copre [FileZilla](https://filezilla-project.org/), poiché è gratuito e disponibile per Windows, macOS e Linux. Per installare FileZilla, vai alla [pagina di download di FileZilla](https://filezilla-project.org/download.php?type=client), clicca sul grande pulsante di download e poi installa dal file di installazione nel modo consueto.

> [!NOTE]
> Ovviamente ci sono molte altre opzioni. Vedi [Strumenti di pubblicazione](/it/docs/Learn_web_development/Howto/Tools_and_setup/How_much_does_it_cost#publishing_tools) per ulteriori informazioni.

Apri l'applicazione FileZilla; dovresti vedere qualcosa di simile:

![Screenshot dell'interfaccia utente dell'applicazione FileZilla FTP. Il campo Host ha il focus.](filezilla-ui.png)

### Accesso

Per questo esempio, supporremo che il nostro fornitore di hosting (il servizio che ospiterà il nostro server web HTTP) sia una società fittizia "Example Hosting Provider" i cui URL sono simili a questo: `mypersonalwebsite.examplehostingprovider.net`.

Abbiamo appena aperto un account e ricevuto queste informazioni da loro:

> Congratulazioni per l'apertura di un account presso Example Hosting Provider.
>
> Il tuo account è: `demozilla`
>
> Il tuo sito web sarà visibile su `demozilla.examplehostingprovider.net`
>
> Per pubblicare su questo account, connettiti tramite SFTP con le seguenti credenziali:
>
> - Server SFTP: `sftp://demozilla.examplehostingprovider.net`
> - Nome utente: `demozilla`
> - Password: `quickbrownfox`
> - Porta: `5548`
> - Per pubblicare sul web, inserisci i tuoi file nella directory `Public/htdocs`.

Guardiamo prima `http://demozilla.examplehostingprovider.net/` — come puoi vedere, finora non c'è nulla:

![Il nostro sito web personale demozilla, visto in un browser: è vuoto](demozilla-empty.png)

> [!NOTE]
> A seconda del tuo fornitore di hosting, la maggior parte delle volte vedrai una pagina che dice qualcosa come "Questo sito è ospitato da \[Servicio di Hosting]." quando vai per la prima volta al tuo indirizzo web.

Per connettere il tuo client SFTP al server remoto, segui questi passaggi:

1. Scegli _File > Gestione Siti…_ dal menu principale.
2. Nella finestra _Gestione Siti_, premi il pulsante _Nuovo Sito_, quindi inserisci il nome del sito come **demozilla** nello spazio fornito.
3. Inserisci il server SFTP fornito dal tuo host nel campo _Host:_.
4. Nel menu a tendina _Tipo di accesso:_, scegli _Normale_, quindi inserisci il nome utente e la password forniti nei campi pertinenti.
5. Inserisci la porta corretta e altre informazioni.

La tua finestra dovrebbe assomigliare a questa:

![Screenshot della pagina di destinazione predefinita di un sito web fittizio quando la directory dei file è vuota](site-manager.png)

Ora premi _Connetti_ per connetterti al server SFTP.

Nota: Assicurati che il tuo fornitore di hosting offra una connessione SFTP (FTP Sicuro) al tuo spazio di hosting. L'FTP è intrinsecamente insicuro e non dovresti usarlo.

### Qui e lì: vista locale e remota

Una volta connesso, il tuo schermo dovrebbe apparire in questo modo (ci siamo connessi a un nostro esempio per darti un'idea):

![Client SFTP che visualizza i contenuti del sito web una volta connesso al server SFTP. I file locali sono a sinistra. I file remoti sono a destra.](connected.png)

Esaminiamo cosa stai vedendo:

- Nel pannello centrale sinistro, vedi i tuoi file locali. Naviga nella directory in cui memorizzi il tuo sito web (ad esempio, `mdn`).
- Nel pannello centrale destro, vedi i file remoti. Siamo connessi alla nostra radice FTP remota (in questo caso, `users/demozilla`).
- Puoi ignorare i pannelli inferiore e superiore per ora. Sono rispettivamente un registro dei messaggi che mostrano lo stato della connessione tra il tuo computer e il server SFTP, e un registro in tempo reale di ogni interazione tra il tuo client SFTP e il server.

### Caricamento sul server

Le istruzioni del nostro host di esempio ci hanno detto "Per pubblicare sul web, inserisci i tuoi file nella directory `Public/htdocs`." Devi navigare nella directory specificata nel tuo pannello destro. Questa directory è effettivamente la radice del tuo sito web — dove andranno il tuo file `index.html` e altre risorse.

Una volta che hai trovato la directory remota corretta in cui inserire i tuoi file, per caricare i tuoi file sul server devi trascinarli dal pannello sinistro a quello destro.

### Sono davvero online?

Finora, tutto bene, ma i file sono davvero online? Puoi ricontrollare tornando al tuo sito web (ad esempio, `http://demozilla.examplehostingprovider.net/`) nel tuo browser:

![Ecco fatto: il nostro sito web è online!](here-we-go.png)

E il nostro sito web è online!

## Rsync

{{Glossary("Rsync", "Rsync")}} è uno strumento di sincronizzazione file locale-remoto, generalmente disponibile sulla maggior parte dei sistemi Unix-based (come macOS e Linux), ma esistono anche versioni per Windows.

È considerato uno strumento più avanzato di SFTP, poiché di default è utilizzato da riga di comando. Un comando di base sembra questo:

```bash
rsync [-options] SOURCE user@x.x.x.x:DESTINATION
```

- `-options` è un trattino seguito da una o più lettere, per esempio `-v` per messaggi di errore dettagliati e `-b` per fare backup. Puoi vedere l'elenco completo alla [pagina man di rsync](https://linux.die.net/man/1/rsync) (cerca "Riepilogo delle opzioni").
- `SOURCE` è il percorso al file locale o alla directory da cui vuoi copiare i file.
- `user@` sono le credenziali dell'utente sul server remoto a cui vuoi copiare i file.
- `x.x.x.x` è l'indirizzo IP del server remoto.
- `DESTINATION` è il percorso della posizione in cui vuoi copiare la tua directory o i tuoi file sul server remoto.

Dovrai ottenere questi dettagli dal tuo fornitore di hosting.

Per ulteriori informazioni ed esempi aggiuntivi, vedi [Come usare Rsync per copiare/sincronizzare file tra server](https://www.atlantic.net/vps-hosting/how-to-use-rsync-copy-sync-files-servers/).

Ovviamente, è una buona idea usare una connessione sicura, come con FTP. Nel caso di Rsync, specifici i dettagli SSH per effettuare la connessione su SSH, utilizzando l'opzione `-e`. Ad esempio:

```bash
rsync [-options] -e "ssh [SSH DETAILS GO HERE]" SOURCE user@x.x.x.x:DESTINATION
```

Puoi trovare maggiori dettagli su ciò che è necessario su [Come copiare file con Rsync su SSH](https://www.digitalocean.com/community/tutorials/how-to-copy-files-with-rsync-over-ssh).

### Strumenti GUI per Rsync

Esistono strumenti GUI per Rsync (per chi non si sente a proprio agio con la riga di comando). [Acrosync](https://acrosync.com/mac.html) è uno di questi strumenti, ed è disponibile per Windows e macOS.

Ancora una volta, dovresti ottenere le credenziali di connessione dal tuo fornitore di hosting, ma in questo modo avresti una GUI per inserirle.

## GitHub

GitHub ti permette di pubblicare siti web tramite [GitHub pages](https://pages.github.com/) (gh-pages).

Abbiamo coperto le basi di questo nell'articolo [Pubblicare il tuo sito web](/it/docs/Learn_web_development/Getting_started/Your_first_website/Publishing_your_website) dalla nostra [Guida introduttiva al Web](/it/docs/Learn_web_development/Getting_started/Your_first_website), quindi non lo ripeteremo tutto qui.

Tuttavia, è utile sapere che puoi anche ospitare un sito web su GitHub, ma usare un dominio personalizzato con esso. Vedi [Usare un dominio personalizzato con GitHub Pages](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site) per una guida dettagliata.

## Altri metodi per caricare file

Il protocollo FTP è un metodo ben noto per pubblicare un sito web, ma non è l'unico. Ecco alcune altre possibilità:

- **Interfacce web**. Un'interfaccia HTML che funge da front-end per un servizio di caricamento file remoto. Fornito dal tuo servizio di hosting.
- **{{Glossary("WebDAV", "WebDAV")}}**. Un'estensione del protocollo {{Glossary("HTTP", "HTTP")}} per consentire una gestione dei file più avanzata.
