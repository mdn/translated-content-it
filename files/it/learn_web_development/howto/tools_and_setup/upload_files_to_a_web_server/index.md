---
title: Come caricare i propri file su un server web?
slug: Learn_web_development/Howto/Tools_and_setup/Upload_files_to_a_web_server
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

Questo articolo mostra come pubblicare il proprio sito online utilizzando strumenti di trasferimento file.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Deve sapere cos'è un
        <a href="/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_web_server"
          >server web</a
        >
        e come funzionano i
        <a href="/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_domain_name"
          >nomi di dominio</a
        >. Deve anche sapere come
        <a
          href="/it/docs/Learn_web_development/Howto/Tools_and_setup/set_up_a_local_testing_server"
          >configurare un ambiente di base</a
        >
        e come
        <a href="/it/docs/Learn_web_development/Getting_started/Your_first_website"
          >scrivere una pagina web semplice</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Impari come caricare file su un server utilizzando i vari strumenti di trasferimento file disponibili.
      </td>
    </tr>
  </tbody>
</table>

## Sommario

Se ha costruito una semplice pagina web (consultare [Basi di HTML](/it/docs/Learn_web_development/Getting_started/Your_first_website/Creating_the_content) per un esempio), probabilmente vorrà metterla online, su un server web. In questo articolo discuteremo come farlo, utilizzando varie opzioni disponibili come client SFTP, RSync e GitHub.

## SFTP

Esistono diversi client SFTP. La nostra demo copre [FileZilla](https://filezilla-project.org/), poiché è gratuito e disponibile per Windows, macOS e Linux. Per installare FileZilla, vada alla [pagina di download di FileZilla](https://filezilla-project.org/download.php?type=client), clicchi sul grande pulsante Scarica, quindi installi dal file di installazione nel solito modo.

> [!NOTE]
> Ovviamente ci sono molte altre opzioni. Consultare [Strumenti di pubblicazione](/it/docs/Learn_web_development/Howto/Tools_and_setup/How_much_does_it_cost#publishing_tools) per ulteriori informazioni.

Apra l'applicazione FileZilla; dovrebbe vedere qualcosa del genere:

![Screenshot dell'interfaccia utente dell'applicazione FileZilla FTP. L'input dell'host è in evidenza.](filezilla-ui.png)

### Accesso

Per questo esempio, supporremo che il nostro fornitore di hosting (il servizio che ospiterà il nostro server web HTTP) sia una società fittizia "Example Hosting Provider" i cui URL sono simili a questo: `mypersonalwebsite.examplehostingprovider.net`.

Abbiamo appena aperto un account e ricevuto queste informazioni da loro:

> Congratulazioni per aver aperto un account presso Example Hosting Provider.
>
> Il suo account è: `demozilla`
>
> Il suo sito web sarà visibile su `demozilla.examplehostingprovider.net`
>
> Per pubblicare su questo account, si connetta tramite SFTP con le seguenti credenziali:
>
> - Server SFTP: `sftp://demozilla.examplehostingprovider.net`
> - Nome utente: `demozilla`
> - Password: `quickbrownfox`
> - Porta: `5548`
> - Per pubblicare sul web, inserisca i suoi file nella directory `Public/htdocs`.

Diamo prima un'occhiata a `http://demozilla.examplehostingprovider.net/` — come può vedere, finora non c'è nulla:

![Il nostro sito web personale demozilla, visto in un browser: è vuoto](demozilla-empty.png)

> [!NOTE]
> A seconda del suo fornitore di hosting, la maggior parte delle volte vedrà una pagina che dice qualcosa tipo "Questo sito web è ospitato da \[Servizio di Hosting]." quando va per la prima volta al suo indirizzo web.

Per connettere il suo client SFTP al server remoto, segua questi passaggi:

1. Scegli _File > Gestione Siti…_ dal menu principale.
2. Nella finestra _Gestione Siti_, premi il pulsante _Nuovo Sito_, quindi compila il nome del sito come **demozilla** nello spazio fornito.
3. Compili il server SFTP fornito dall'host nel campo _Host:_.
4. Nel menu a discesa _Tipo di accesso:_, scelga _Normale_, quindi inserisca il nome utente e la password forniti nei campi pertinenti.
5. Compili la porta corretta e altre informazioni.

La sua finestra dovrebbe apparire simile a questa:

![Screenshot della pagina predefinita di un sito web fittizio quando la directory dei file è vuota](site-manager.png)

Ora premi _Connect_ per connettersi al server SFTP.

Nota: Si assicuri che il suo fornitore di hosting offra una connessione SFTP (FTP sicuro) al suo spazio di hosting. FTP è intrinsecamente insicuro e non dovrebbe essere usato.

### Qui e là: visualizzazione locale e remota

Una volta connessi, il suo schermo dovrebbe apparire più o meno così (ci siamo connessi a un nostro esempio per darle un'idea):

![Client SFTP che mostra il contenuto del sito web una volta che è stato connesso al server SFTP. I file locali sono a sinistra. I file remoti sono a destra.](connected.png)

Esaminiamo cosa vede:

- Nel pannello centrale a sinistra, vede i suoi file locali. Navighi nella directory in cui memorizza il suo sito web (ad es., `mdn`).
- Nel pannello centrale a destra, vede i file remoti. Siamo connessi alla nostra radice FTP remota (in questo caso, `users/demozilla`)
- Può ignorare i pannelli inferiore e superiore per ora. Rispetivamente, mostrano un log di messaggi indicando lo stato della connessione tra il suo computer e il server SFTP, e un log live di ogni interazione tra il suo client SFTP e il server.

### Caricare sul server

Le istruzioni dell'host nel nostro esempio ci hanno detto "Per pubblicare sul web, inserisca i suoi file nella directory `Public/htdocs`". Deve navigare nella directory specificata nel suo pannello di destra. Questa directory è effettivamente la radice del suo sito web — dove il suo file `index.html` e altri asset andranno.

Una volta trovata la directory remota corretta dove mettere i file, per caricarli sul server deve trascinarli dal pannello di sinistra a quello di destra.

### Sono veramente online?

Fino a qui, tutto bene, ma i file sono veramente online? Può ricontrollare tornando al suo sito web (ad es., `http://demozilla.examplehostingprovider.net/`) nel suo browser:

![Ecco qui: il nostro sito è attivo!](here-we-go.png)

E il nostro sito è attivo!

## Rsync

{{Glossary("Rsync", "Rsync")}} è uno strumento per sincronizzare file da locale a remoto, generalmente disponibile sulla maggior parte dei sistemi basati su Unix (come macOS e Linux), ma esistono versioni anche per Windows.

È considerato uno strumento più avanzato rispetto all'SFTP, poiché di default viene usato sulla linea di comando. Un comando di base appare così:

```bash
rsync [-options] SOURCE user@x.x.x.x:DESTINATION
```

- `-options` è un trattino seguito da una o più lettere, per esempio `-v` per messaggi di errore verbosi, e `-b` per fare backup. Può vedere l'elenco completo nella [pagina di manuale di rsync](https://linux.die.net/man/1/rsync) (cerchi "Options summary").
- `SOURCE` è il percorso del file o della directory locale da cui vuole copiare i file.
- `user@` sono le credenziali dell'utente sul server remoto a cui vuole copiare i file.
- `x.x.x.x` è l'indirizzo IP del server remoto.
- `DESTINATION` è il percorso della posizione in cui desidera copiare la sua directory o i suoi file sul server remoto.

Dovrebbe ottenere tali dettagli dal suo fornitore di hosting.

Per ulteriori informazioni ed esempi aggiuntivi, consulti [Come usare Rsync per copiare/sincronizzare file tra server](https://www.atlantic.net/vps-hosting/how-to-use-rsync-copy-sync-files-servers/).

Ovviamente, è una buona idea usare una connessione sicura, come con FTP. Nel caso di Rsync, specifica i dettagli SSH per effettuare la connessione tramite SSH, usando l'opzione `-e`. Per esempio:

```bash
rsync [-options] -e "ssh [SSH DETAILS GO HERE]" SOURCE user@x.x.x.x:DESTINATION
```

Può trovare ulteriori dettagli su cosa è necessario su [Come copiare i file con Rsync tramite SSH](https://www.digitalocean.com/community/tutorials/how-to-copy-files-with-rsync-over-ssh).

### Strumenti GUI Rsync

Sono disponibili strumenti GUI per Rsync (per coloro che non si sentono a proprio agio con la riga di comando). [Acrosync](https://acrosync.com/mac.html) è uno di questi strumenti, ed è disponibile per Windows e macOS.

Ancora una volta, dovrebbe ottenere le credenziali di connessione dal suo fornitore di hosting, ma in questo modo avrebbe una GUI per inserirli.

## GitHub

GitHub permette di pubblicare siti web tramite [GitHub pages](https://pages.github.com/) (gh-pages).

Abbiamo coperto le basi dell'utilizzo di questo nell'articolo [Pubblicare il proprio sito web](/it/docs/Learn_web_development/Getting_started/Your_first_website/Publishing_your_website) dalla nostra guida [Iniziare con il Web](/it/docs/Learn_web_development/Getting_started/Your_first_website), quindi non ripeteremo tutto qui.

Tuttavia, è utile sapere che può anche ospitare un sito web su GitHub, ma usarlo con un dominio personalizzato. Veda [Usare un dominio personalizzato con GitHub Pages](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site) per una guida dettagliata.

## Altri metodi per caricare file

Il protocollo FTP è un metodo ben noto per pubblicare un sito web, ma non è l'unico. Ecco alcune altre possibilità:

- **Interfacce web**. Un'interfaccia HTML che funge da front-end per un servizio di upload file remoto. Fornito dal suo servizio di hosting.
- **{{Glossary("WebDAV", "WebDAV")}}**. Un'estensione del protocollo {{Glossary("HTTP", "HTTP")}} per consentire una gestione file più avanzata.
