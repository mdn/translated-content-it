---
title: Come caricare i file su un server web?
slug: Learn_web_development/Howto/Tools_and_setup/Upload_files_to_a_web_server
l10n:
  sourceCommit: e5cd1cab36e2fdcf5dfe28e10b0a7cb235354e62
---

Questo articolo mostra come pubblicare un sito online usando strumenti di trasferimento file.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        È necessario sapere
        <a href="/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_web_server"
          >che cos'è un server web</a
        >
        e
        <a href="/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_domain_name"
          >come funzionano i nomi di dominio</a
        >. È inoltre necessario sapere come
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
        Imparare a trasferire file a un server utilizzando i vari strumenti di
        trasferimento file disponibili.
      </td>
    </tr>
  </tbody>
</table>

## Riepilogo

Se è stata realizzata una semplice pagina web (vedere [le basi di HTML](/it/docs/Learn_web_development/Getting_started/Your_first_website/Creating_the_content) per un esempio), probabilmente si desidera pubblicarla online, su un server web. In questo articolo verrà illustrato come farlo, utilizzando varie opzioni disponibili come client SFTP, RSync e GitHub.

## SFTP

Esistono diversi client SFTP. La demo utilizza [FileZilla](https://filezilla-project.org/), poiché è gratuito e disponibile per Windows, macOS e Linux. Per installare FileZilla, visitare la [pagina dei download di FileZilla](https://filezilla-project.org/download.php?type=client), fare clic sul grande pulsante Download, quindi installare dal file di installazione nel modo consueto.

> [!NOTE]
> Naturalmente esistono molte altre opzioni. Per ulteriori informazioni, vedere [Strumenti di pubblicazione](/it/docs/Learn_web_development/Howto/Tools_and_setup/How_much_does_it_cost#publishing_tools).

Aprire l'applicazione FileZilla; dovrebbe essere visualizzato qualcosa di simile:

![Schermata dell'interfaccia utente dell'applicazione FTP FileZilla. Il campo Host è attivo.](filezilla-ui.png)

### Accesso

Per questo esempio, supponiamo che il provider di hosting (il servizio che ospiterà il server web HTTP) sia un'azienda fittizia chiamata "Example Hosting Provider", i cui URL hanno questo aspetto: `mypersonalwebsite.examplehostingprovider.net`.

È stato appena aperto un account e sono state ricevute queste informazioni:

> Congratulazioni per aver aperto un account presso Example Hosting Provider.
>
> L'account è: `demozilla`
>
> Il sito web sarà visibile all'indirizzo `demozilla.examplehostingprovider.net`
>
> Per pubblicare su questo account, connettersi tramite SFTP con le seguenti credenziali:
>
> - Server SFTP: `sftp://demozilla.examplehostingprovider.net`
> - Nome utente: `demozilla`
> - Password: `quickbrownfox`
> - Porta: `5548`
> - Per pubblicare sul Web, inserire i file nella directory `Public/htdocs`.

Osserviamo innanzitutto `http://demozilla.examplehostingprovider.net/`: come si può vedere, per ora non contiene nulla:

![Il sito web personale demozilla visualizzato in un browser: è vuoto](demozilla-empty.png)

> [!NOTE]
> A seconda del provider di hosting, nella maggior parte dei casi verrà visualizzata una pagina con un messaggio simile a "This website is hosted by \[Hosting Service]." quando si visita per la prima volta l'indirizzo web.

Per connettere il client SFTP al server remoto, seguire questi passaggi:

1. Scegliere _File > Site Manager…_ dal menu principale.
2. Nella finestra _Site Manager_, premere il pulsante _New Site_, quindi inserire **demozilla** come nome del sito nello spazio fornito.
3. Inserire il server SFTP fornito dal provider nel campo _Host:_.
4. Nel menu a discesa _Logon Type:_, scegliere _Normal_, quindi inserire il nome utente e la password forniti nei rispettivi campi.
5. Inserire la porta corretta e le altre informazioni.

La finestra dovrebbe apparire più o meno così:

![Schermata della pagina iniziale predefinita di un sito web fittizio quando la directory dei file è vuota](site-manager.png)

Ora premere _Connect_ per connettersi al server SFTP.

Nota: assicurarsi che il provider di hosting offra una connessione SFTP (Secure FTP) allo spazio di hosting. FTP è intrinsecamente non sicuro e non dovrebbe essere utilizzato.

### Qui e lì: visualizzazione locale e remota

Una volta connessi, lo schermo dovrebbe apparire più o meno così (è stato utilizzato un esempio proprietario per dare un'idea):

![Client SFTP che visualizza i contenuti del sito web dopo essersi connesso al server SFTP. I file locali sono a sinistra. I file remoti sono a destra.](connected.png)

Esaminiamo ciò che viene visualizzato:

- Nel riquadro centrale sinistro sono visualizzati i file locali. Accedere alla directory in cui è memorizzato il sito web (ad esempio, `mdn`).
- Nel riquadro centrale destro sono visualizzati i file remoti. È stato effettuato l'accesso alla root FTP remota (in questo caso, `users/demozilla`).
- Per il momento è possibile ignorare i riquadri inferiore e superiore. Rispettivamente, mostrano un registro dei messaggi che indica lo stato della connessione tra il computer e il server SFTP e un registro in tempo reale di ogni interazione tra il client SFTP e il server.

### Caricamento sul server

Le istruzioni dell'host di esempio indicavano: "Per pubblicare sul Web, inserire i file nella directory `Public/htdocs`." È necessario accedere alla directory specificata nel riquadro destro. Questa directory è effettivamente la root del sito web, dove verranno collocati il file `index.html` e le altre risorse.

Una volta individuata la directory remota corretta in cui inserire i file, per caricarli sul server è necessario trascinarli dal riquadro sinistro a quello destro.

### Sono davvero online?

Fin qui tutto bene, ma i file sono davvero online? È possibile verificarlo tornando al sito web (ad esempio, `http://demozilla.examplehostingprovider.net/`) nel browser:

![Ecco fatto: il sito web è online!](here-we-go.png)

E il sito web è online!

## Rsync

{{Glossary("Rsync", "Rsync")}} è uno strumento di sincronizzazione dei file da locale a remoto, generalmente disponibile sulla maggior parte dei sistemi basati su Unix (come macOS e Linux), ma esistono anche versioni per Windows.

È considerato uno strumento più avanzato di SFTP perché, per impostazione predefinita, viene usato dalla riga di comando. Un comando di base ha questo aspetto:

```bash
rsync [-options] SOURCE user@x.x.x.x:DESTINATION
```

- `-options` è un trattino seguito da una o più lettere, ad esempio `-v` per messaggi di errore dettagliati e `-b` per creare backup. L'elenco completo è disponibile nella [pagina man di rsync](https://linux.die.net/man/1/rsync) (cercare "Options summary").
- `SOURCE` è il percorso del file o della directory locale da cui si desidera copiare i file.
- `user@` sono le credenziali dell'utente sul server remoto verso il quale si desidera copiare i file.
- `x.x.x.x` è l'indirizzo IP del server remoto.
- `DESTINATION` è il percorso della posizione sul server remoto in cui si desidera copiare la directory o i file.

Questi dettagli devono essere ottenuti dal provider di hosting.

Per ulteriori informazioni ed esempi, vedere [How to Use Rsync to Copy/Sync Files Between Servers](https://www.atlantic.net/vps-hosting/how-to-use-rsync-copy-sync-files-servers/).

Naturalmente, è consigliabile utilizzare una connessione sicura, come con FTP. Nel caso di Rsync, si specificano i dettagli SSH per effettuare la connessione tramite SSH usando l'opzione `-e`. Ad esempio:

```bash
rsync [-options] -e "ssh [SSH DETAILS GO HERE]" SOURCE user@x.x.x.x:DESTINATION
```

Ulteriori dettagli sui requisiti sono disponibili in [How To Copy Files With Rsync Over SSH](https://www.digitalocean.com/community/tutorials/how-to-copy-files-with-rsync-over-ssh).

### Strumenti GUI per Rsync

Sono disponibili strumenti GUI per Rsync, per chi non ha dimestichezza con l'uso della riga di comando. [Acrosync](https://acrosync.com/mac.html) è uno di questi strumenti ed è disponibile per Windows e macOS.

Anche in questo caso, le credenziali di connessione devono essere ottenute dal provider di hosting, ma in questo modo è disponibile una GUI in cui inserirle.

## GitHub

GitHub consente di pubblicare siti web tramite [GitHub Pages](https://pages.github.com/) (gh-pages).

Le basi del suo utilizzo sono state trattate nell'articolo [Pubblicare il proprio sito web](/it/docs/Learn_web_development/Getting_started/Your_first_website/Publishing_your_website) della guida [Primi passi con il Web](/it/docs/Learn_web_development/Getting_started/Your_first_website), quindi non verranno ripetute qui.

Tuttavia, è utile sapere che è anche possibile ospitare un sito web su GitHub utilizzando un dominio personalizzato. Per una guida dettagliata, vedere [Using a custom domain with GitHub Pages](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site).

## Altri metodi per caricare file

Il protocollo FTP è un metodo noto per pubblicare un sito web, ma non è l'unico. Ecco alcune altre possibilità:

- **Interfacce web**. Un'interfaccia HTML che funge da front-end per un servizio remoto di caricamento file. Fornita dal servizio di hosting.
- **{{Glossary("WebDAV", "WebDAV")}}**. Un'estensione del protocollo {{Glossary("HTTP", "HTTP")}} che consente una gestione dei file più avanzata.
