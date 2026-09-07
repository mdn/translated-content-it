---
title: Quale software serve per creare un sito web?
slug: Learn_web_development/Howto/Tools_and_setup/What_software_do_I_need
l10n:
  sourceCommit: c49748a0ce4fdf77427e29cb6edbca8953a514e7
---

In questo articolo vengono illustrati i componenti software necessari per modificare, caricare o visualizzare un sito web.

<table class="standard-table">
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        È necessario conoscere già
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Browsing_the_web"
          >la differenza tra pagine web, siti web, server web e motori di
          ricerca.</a
        >
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare quali componenti software sono necessari per modificare, caricare o
        visualizzare un sito web.
      </td>
    </tr>
  </tbody>
</table>

## Riepilogo

La maggior parte dei programmi necessari per lo sviluppo web può essere scaricata gratuitamente. In questo articolo verranno forniti alcuni link.

Sono necessari strumenti per:

- Creare e modificare pagine web
- Caricare file sul server web
- Visualizzare il sito web

Quasi tutti i sistemi operativi includono per impostazione predefinita un editor di testo e un browser, che possono essere usati per visualizzare i siti web. Di conseguenza, di solito è necessario procurarsi soltanto un software per trasferire i file al server web.

## Approfondimento

### Creazione e modifica di pagine web

Per creare e modificare un sito web, è necessario un editor di testo. Gli editor di testo creano e modificano file di testo non formattato. Altri formati, come **{{Glossary("RTF", "RTF")}}**, consentono di aggiungere formattazione, come il grassetto o la sottolineatura. Questi formati non sono adatti alla scrittura di pagine web. È opportuno riflettere sulla scelta dell'editor di testo, poiché verrà usato intensivamente durante la creazione del sito web.

Tutti i sistemi operativi desktop includono un editor di testo di base. Questi editor sono semplici da usare, ma non dispongono di funzionalità speciali per la scrittura del codice delle pagine web. Se si desidera qualcosa di più avanzato, sono disponibili molti strumenti di terze parti. Gli editor di terze parti spesso includono funzionalità aggiuntive, tra cui colorazione della sintassi, completamento automatico, sezioni comprimibili e ricerca nel codice. Ecco un breve elenco di editor:

<table class="standard-table">
  <thead>
    <tr>
      <th scope="col">Sistema operativo</th>
      <th scope="col">Editor integrato</th>
      <th scope="col">Editor di terze parti</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Windows</td>
      <td>
        <ul>
          <li>
            <a
              href="https://en.wikipedia.org/wiki/Notepad_%28software%29"
              rel="external"
              >Notepad</a
            >
          </li>
        </ul>
      </td>
      <td>
        <ul>
          <li><a href="https://notepad-plus-plus.org/">Notepad++</a></li>
          <li>
            <a href="https://code.visualstudio.com/">Visual Studio Code</a>
          </li>
          <li><a href="https://www.jetbrains.com/webstorm/">Web Storm</a></li>
          <li><a href="https://brackets.io/">Brackets</a></li>
          <li><a href="https://shiftedit.net/">ShiftEdit</a></li>
          <li><a href="https://www.sublimetext.com/">Sublime Text</a></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>Mac OS</td>
      <td>
        <ul>
          <li>
            <a href="https://en.wikipedia.org/wiki/TextEdit" rel="external"
              >TextEdit</a
            >
          </li>
        </ul>
      </td>
      <td>
        <ul>
          <li>
            <a href="https://www.barebones.com/products/textwrangler/"
              >TextWrangler</a
            >
          </li>
          <li>
            <a href="https://code.visualstudio.com/">Visual Studio Code</a>
          </li>
          <li><a href="https://brackets.io/">Brackets</a></li>
          <li><a href="https://shiftedit.net/">ShiftEdit</a></li>
          <li><a href="https://www.sublimetext.com/">Sublime Text</a></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>Linux</td>
      <td>
        <ul>
          <li>
            <a href="https://en.wikipedia.org/wiki/Vi_(text_editor)" rel="external">Vi</a>
            (tutti i sistemi UNIX)
          </li>
          <li>
            <a href="https://en.wikipedia.org/wiki/Gedit" rel="external"
              >GEdit</a
            >
            (GNOME)
          </li>
          <li>
            <a
              href="https://en.wikipedia.org/wiki/Kate_%28text_editor%29"
              rel="external"
              >Kate</a
            >
            (KDE)
          </li>
          <li>
            <a href="https://en.wikipedia.org/wiki/Leafpad" rel="external"
              >LeafPad</a
            >
            (Xfce)
          </li>
        </ul>
      </td>
      <td>
        <ul>
          <li><a href="https://www.gnu.org/software/emacs/">Emacs</a></li>
          <li><a href="https://www.vim.org/" rel="external">VIM</a></li>
          <li>
            <a href="https://code.visualstudio.com/">Visual Studio Code</a>
          </li>
          <li><a href="https://brackets.io/">Brackets</a></li>
          <li><a href="https://shiftedit.net/">ShiftEdit</a></li>
          <li><a href="https://www.sublimetext.com/">Sublime Text</a></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>ChromeOS</td>
      <td>
        <ul>
          <li><a href="https://github.com/GoogleChromeLabs/text-app">Text</a></li>
        </ul>
      </td>
      <td>
        <ul>
          <li><a href="https://shiftedit.net/">ShiftEdit</a></li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

Ecco una schermata di un editor di testo avanzato:

![Schermata di Notepad++.](notepadplusplus.png)

Ecco una schermata di un editor di testo online:

![Schermata di ShiftEdit](shiftedit.png)

### Caricamento di file sul Web

Quando il sito web è pronto per essere visualizzato pubblicamente, sarà necessario caricare le pagine web sul server web. È possibile acquistare spazio su un server da vari fornitori (vedere [Quanto costa fare qualcosa sul web?](/it/docs/Learn_web_development/Howto/Tools_and_setup/How_much_does_it_cost)). Dopo aver scelto il fornitore da usare, il fornitore invierà via email le informazioni di accesso, di solito sotto forma di URL SFTP, nome utente, password e altre informazioni necessarie per connettersi al proprio server. Tenere presente che (S)FTP è ormai in qualche modo obsoleto e altri sistemi di caricamento stanno iniziando a diventare popolari, come [RSync](https://en.wikipedia.org/wiki/Rsync) e [Git/GitHub](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site).

> [!NOTE]
> FTP è intrinsecamente non sicuro. Assicurarsi che il fornitore di hosting consenta l'uso di una connessione sicura, ad esempio SFTP o RSync tramite SSH.

Caricare file su un server web è un passaggio molto importante nella creazione di un sito web, perciò viene trattato in dettaglio in [un articolo separato](/it/docs/Learn_web_development/Howto/Tools_and_setup/Upload_files_to_a_web_server). Per ora, ecco un breve elenco di client (S)FTP gratuiti e di base:

<table class="standard-table">
  <thead>
    <tr>
      <th scope="col">Sistema operativo</th>
      <th colspan="2" scope="col">Software FTP</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Windows</td>
      <td>
        <ul>
          <li><a href="https://winscp.net">WinSCP</a></li>
          <li><a href="https://mobaxterm.mobatek.net/">Moba Xterm</a></li>
        </ul>
      </td>
      <td rowspan="3">
        <ul>
          <li>
            <a href="https://filezilla-project.org/">FileZilla</a> (tutti i sistemi operativi)
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>Linux</td>
      <td>
        <ul>
          <li>
            <a
              href="https://apps.gnome.org/en/Nautilus/"
              rel="external"
              >Nautilus/Files</a
            >
            (GNOME)
          </li>
          <li>
            <a href="https://dolphin.com/" rel="external">Dolphin</a> (KDE)
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>Mac OS</td>
      <td>
        <ul>
          <li><a href="https://cyberduck.de/">Cyberduck</a></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>ChromeOS</td>
      <td>
        <ul>
          <li><a href="https://shiftedit.net/">ShiftEdit</a> (tutti i sistemi operativi)</li>
        </ul>
      </td>
      <td></td>
    </tr>
  </tbody>
</table>

### Test dei siti web

Sono disponibili [molti browser web](https://en.wikipedia.org/wiki/List_of_web_browsers). Durante lo sviluppo di un sito web, è opportuno testarlo almeno con i seguenti browser principali, sia su piattaforme desktop sia mobili, per assicurarsi che il sito funzioni per la maggior parte delle persone:

- [Mozilla Firefox](https://www.firefox.com/en-US/)
- [Google Chrome](https://www.google.com/chrome/)
- [Apple Safari](https://www.apple.com/safari/)

Se il sito è destinato a un gruppo specifico, ad esempio una piattaforma tecnica o una lingua, potrebbe essere necessario testarlo con browser aggiuntivi, come [UC Browser](https://www.ucweb.com/) o [Opera Mini](https://www.opera.com/mini).

I test diventano complessi perché alcuni browser funzionano solo su determinati sistemi operativi. In particolare, Apple Safari funziona su iOS, iPadOS e macOS. È preferibile sfruttare servizi come [Browsershots](https://www.browsershots.at/) o [Browserstack](https://www.browserstack.com/). Browsershots crea schermate del sito web così come apparirà in vari browser. Browserstack offre accesso remoto completo alle macchine virtuali, consentendo di testare il sito negli ambienti più comuni e su diversi sistemi operativi. In alternativa, è possibile configurare le proprie macchine virtuali, ma ciò richiede una certa esperienza.

Per ulteriori informazioni, consultare [Strategie per l'esecuzione dei test: creare un laboratorio di test](/it/docs/Learn_web_development/Extensions/Testing/Testing_strategies#putting_together_a_testing_lab).

È assolutamente consigliabile eseguire alcuni test su un dispositivo reale, in particolare su dispositivi mobili reali. I dispositivi mobili hanno naturalmente un costo, quindi è consigliabile condividere i dispositivi all'interno di un team se si desidera testare molte piattaforme senza spendere troppo. Per un accesso cloud scalabile ai test su dispositivi reali, si consiglia anche di dare un'occhiata a [App Live: la piattaforma di test interattivi per app mobili di BrowserStack](https://www.browserstack.com/app-live).

## Passaggi successivi

- Alcuni di questi software sono gratuiti, ma non tutti. [Scopri quanto costa fare qualcosa sul web](/it/docs/Learn_web_development/Howto/Tools_and_setup/How_much_does_it_cost).
- Per saperne di più sugli editor di testo, leggere l'articolo su [come scegliere e installare un editor di testo](/it/docs/Learn_web_development/Howto/Tools_and_setup/Available_text_editors).
- Per informazioni su come pubblicare un sito web sul web, consultare ["Come caricare file su un server web"](/it/docs/Learn_web_development/Howto/Tools_and_setup/Upload_files_to_a_web_server).
