---
title: Quale software è necessario per creare un sito web?
slug: Learn_web_development/Howto/Tools_and_setup/What_software_do_I_need
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

In questo articolo, esponiamo quali componenti software sono necessari quando si modifica, carica o visualizza un sito web.

<table class="standard-table">
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Dovrebbe già sapere
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Browsing_the_web"
          >la differenza tra pagine web, siti web, server web e motori di ricerca.</a
        >
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare quali componenti software sono necessari se si desidera modificare, caricare o visualizzare un sito web.
      </td>
    </tr>
  </tbody>
</table>

## Riepilogo

È possibile scaricare la maggior parte dei programmi necessari per lo sviluppo web gratuitamente. Forniremo alcuni link in questo articolo.

Avrà bisogno di strumenti per:

- Creare e modificare pagine web
- Caricare file sul proprio server web
- Visualizzare il proprio sito web

Quasi tutti i sistemi operativi includono per impostazione predefinita un editor di testo e un browser, che possono essere usati per visualizzare siti web. Di conseguenza, di solito è necessario acquisire software solo per trasferire file sul proprio server web.

## Apprendimento attivo

_Non è ancora disponibile apprendimento attivo. [Prego, consideri di contribuire](/it/docs/MDN/Community/Getting_started)._

## Approfondisci

### Creare e modificare pagine web

Per creare e modificare un sito web, è necessario un editor di testo. Gli editor di testo creano e modificano file di testo non formattati. Altri formati, come **{{Glossary("RTF", "RTF")}}**, consentono di aggiungere formattazioni, come grassetto o sottolineato. Questi formati non sono adatti per scrivere pagine web. Dovrebbe pensare bene a quale editor di testo utilizzare, poiché interagirà intensamente con esso mentre costruisce il sito web.

Tutti i sistemi operativi desktop vengono forniti con un editor di testo di base. Questi editor sono semplici, ma mancano di funzionalità speciali per la codifica di pagine web. Se desidera qualcosa di più sofisticato, ci sono molti strumenti di terze parti disponibili. Gli editor di terze parti spesso includono funzioni aggiuntive, come la colorazione della sintassi, il completamento automatico, sezioni comprimibili e la ricerca nel codice. Ecco un breve elenco di editor:

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
            <a href="https://visualstudio.microsoft.com/">Visual Studio Code</a>
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
            <a href="https://visualstudio.microsoft.com/">Visual Studio Code</a>
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
            (Tutti UNIX)
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
            <a href="https://visualstudio.microsoft.com/">Visual Studio Code</a>
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
          <li><a href="https://en.wikipedia.org/wiki/Text_(Chrome_app)">Text</a></li>
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

Ecco uno screenshot di un editor di testo avanzato:

![Screenshot di Notepad++.](notepadplusplus.png)

Ecco uno screenshot di un editor di testo online:

![Screenshot di ShiftEdit](shiftedit.png)

### Caricare file sul Web

Quando il suo sito web è pronto per essere visualizzato pubblicamente, dovrà caricare le sue pagine web sul suo server. Può acquistare spazio su un server da vari fornitori (vedi [Quanto costa fare qualcosa sul web?](/it/docs/Learn_web_development/Howto/Tools_and_setup/How_much_does_it_cost)). Una volta scelto il fornitore da utilizzare, il fornitore le invierà via email le informazioni di accesso, di solito sotto forma di un URL SFTP, nome utente, password e altre informazioni necessarie per connettersi al loro server. Tenga presente che (S)FTP è ormai piuttosto obsoleto e stanno iniziando a diventare popolari altri sistemi di caricamento, come [RSync](https://en.wikipedia.org/wiki/Rsync) e [Git/GitHub](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site).

> [!NOTE]
> FTP è intrinsecamente insicuro. Deve assicurarsi che il suo fornitore di hosting permetta l'uso di una connessione sicura, ad esempio SFTP o RSync su SSH.

Caricare file su un server web è un passaggio molto importante durante la creazione di un sito, quindi lo affrontiamo in dettaglio [in un articolo separato](/it/docs/Learn_web_development/Howto/Tools_and_setup/Upload_files_to_a_web_server). Per ora, ecco un breve elenco di client (S)FTP di base gratuiti:

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
            <a href="https://filezilla-project.org/">FileZilla</a> (Tutti OS)
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
          <li><a href="https://shiftedit.net/">ShiftEdit</a> (Tutti OS)</li>
        </ul>
      </td>
      <td></td>
    </tr>
  </tbody>
</table>

### Testare i siti web

Ci sono [molti browser web disponibili](https://en.wikipedia.org/wiki/List_of_web_browsers). Quando sviluppa un sito web, dovrebbe testarlo almeno con i seguenti browser principali sia su piattaforme desktop che mobile, per assicurarsi che il suo sito funzioni per la maggior parte delle persone:

- [Mozilla Firefox](https://www.mozilla.org/en-US/firefox/new/)
- [Google Chrome](https://www.google.com/chrome/)
- [Apple Safari](https://www.apple.com/safari/)

Se si sta rivolgendo a un gruppo specifico (ad esempio, piattaforma tecnica o locale), potrebbe dover testare il sito con browser aggiuntivi, come [UC Browser](https://www.ucweb.com/) o [Opera Mini](https://www.opera.com/mini).

Il testing diventa complicato perché alcuni browser funzionano solo su determinati sistemi operativi. In particolare, Apple Safari funziona su iOS, iPadOS e macOS. È meglio sfruttare servizi come [Browsershots](https://browsershots.org/) o [Browserstack](https://www.browserstack.com/). Browsershots crea screenshot del suo sito web come apparirà in vari browser. Browserstack le offre accesso remoto completo a macchine virtuali, così può testare il suo sito nei più comuni ambienti e su diversi sistemi operativi. In alternativa, può configurare le sue proprie macchine virtuali, ma ciò richiede un po' di esperienza.

Veda [Strategie per eseguire i test: Mettere insieme un laboratorio di test](/it/docs/Learn_web_development/Extensions/Testing/Testing_strategies#putting_together_a_testing_lab) per ulteriori informazioni.

In ogni modo, esegua alcuni test su un vero dispositivo, specialmente su dispositivi mobili reali. I dispositivi mobili hanno un costo, ovviamente, quindi consigliamo di condividere dispositivi all'interno di un team se vuole testare su molte piattaforme senza spendere troppo. Per un accesso scalabile tramite cloud al testing su dispositivi reali, raccomandiamo anche di dare un'occhiata a [App Live: la piattaforma di test interattivi per app mobili di BrowserStack](https://www.browserstack.com/app-live).

## Prossimi passi

- Alcuni di questi software sono gratuiti, ma non tutti. [Scopra quanto costa fare qualcosa sul web](/it/docs/Learn_web_development/Howto/Tools_and_setup/How_much_does_it_cost).
- Se desidera saperne di più sugli editor di testo, legga il nostro articolo su [come scegliere e installare un editor di testo](/it/docs/Learn_web_development/Howto/Tools_and_setup/Available_text_editors).
- Se si chiede come pubblicare il suo sito web sul web, guardi ["Come caricare file su un server web"](/it/docs/Learn_web_development/Howto/Tools_and_setup/Upload_files_to_a_web_server).
