---
title: Quale software è necessario per costruire un sito web?
slug: Learn_web_development/Howto/Tools_and_setup/What_software_do_I_need
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

In questo articolo, illustriamo quali componenti software sono necessari per modificare, caricare o visualizzare un sito web.

<table class="standard-table">
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Dovresti già conoscere
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Browsing_the_web"
          >la differenza tra pagine web, siti web, server web e motori di ricerca.</a
        >
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Impara quali componenti software sono necessari se vuoi modificare, caricare o visualizzare un sito web.
      </td>
    </tr>
  </tbody>
</table>

## Riepilogo

Puoi scaricare gratuitamente la maggior parte dei programmi necessari per lo sviluppo web. Forniremo alcuni link in questo articolo.

Avrai bisogno di strumenti per:

- Creare e modificare pagine web
- Caricare file sul tuo server web
- Visualizzare il tuo sito web

Quasi tutti i sistemi operativi includono di default un editor di testo e un browser, che puoi utilizzare per visualizzare siti web. Di conseguenza, di solito è necessario solo acquisire software per trasferire file sul tuo server web.

## Apprendimento Attivo

_Non c'è ancora apprendimento attivo disponibile. [Per favore, considera di contribuire](/it/docs/MDN/Community/Getting_started)._

## Approfondisci

### Creazione e modifica di pagine web

Per creare e modificare un sito web, hai bisogno di un editor di testo. Gli editor di testo creano e modificano file di testo non formattato. Altri formati, come **{{Glossary("RTF", "RTF")}}**, consentono di aggiungere formattazione, come grassetto o sottolineato. Questi formati non sono adatti per scrivere pagine web. Dovresti riflettere su quale editor di testo utilizzare, dato che ci lavorerai intensamente mentre costruisci il sito web.

Tutti i sistemi operativi desktop includono un editor di testo di base. Questi editor sono tutti semplici, ma mancano di funzionalità speciali per il coding di pagine web. Se desideri qualcosa di un po' più sofisticato, ci sono molti strumenti di terze parti disponibili. Gli editor di terze parti spesso includono funzionalità extra, tra cui la colorazione della sintassi, il completamento automatico, sezioni pieghevoli e ricerca del codice. Ecco un breve elenco di editor:

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

### Caricamento di file sul Web

Quando il tuo sito web è pronto per la visualizzazione pubblica, dovrai caricare le tue pagine sul tuo server web. Puoi acquistare spazio su un server da vari fornitori (vedi [Quanto costa fare qualcosa sul web?](/it/docs/Learn_web_development/Howto/Tools_and_setup/How_much_does_it_cost)). Una volta deciso quale fornitore utilizzare, il fornitore ti invierà via email le informazioni di accesso, di solito sotto forma di un URL SFTP, nome utente, password e altre informazioni necessarie per connettersi al loro server. Tieni presente che (S)FTP è ormai un po' obsoleto, e stanno diventando popolari altri sistemi di caricamento, come [RSync](https://en.wikipedia.org/wiki/Rsync) e [Git/GitHub](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site).

> [!NOTE]
> FTP è intrinsecamente insicuro. Dovresti assicurarti che il tuo fornitore di hosting permetta l'uso di una connessione sicura, ad esempio, SFTP o RSync su SSH.

Caricare file su un server web è un passaggio molto importante nella creazione di un sito web, quindi lo trattiamo in dettaglio in [un articolo separato](/it/docs/Learn_web_development/Howto/Tools_and_setup/Upload_files_to_a_web_server). Per ora, ecco un breve elenco di client (S)FTP di base gratuiti:

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

### Test dei siti web

Ci sono [molti browser web disponibili](https://en.wikipedia.org/wiki/List_of_web_browsers). Quando stai sviluppando un sito web dovresti testarlo almeno con i seguenti browser principali su entrambe le piattaforme desktop e mobile, per assicurarti che il tuo sito funzioni per la maggior parte delle persone:

- [Mozilla Firefox](https://www.mozilla.org/en-US/firefox/new/)
- [Google Chrome](https://www.google.com/chrome/)
- [Apple Safari](https://www.apple.com/safari/)

Se stai puntando a un gruppo specifico (ad esempio, piattaforma tecnica o lingua locale), potresti dover testare il sito con browser aggiuntivi, come [UC Browser](https://www.ucweb.com/) o [Opera Mini](https://www.opera.com/mini).

Il testing diventa complicato perché alcuni browser funzionano solo su determinati sistemi operativi. In particolare, Apple Safari funziona su iOS, iPadOS e macOS. È meglio approfittare di servizi come [Browsershots](https://browsershots.org/) o [Browserstack](https://www.browserstack.com/). Browsershots crea screenshot del tuo sito web come apparirà in vari browser. Browserstack ti dà pieno accesso remoto a macchine virtuali, così puoi testare il tuo sito negli ambienti più comuni e su diversi sistemi operativi. In alternativa, puoi configurare le tue macchine virtuali, ma questo richiede una certa competenza.

Consulta [Strategie per effettuare i test: Mettere insieme un laboratorio di test](/it/docs/Learn_web_development/Extensions/Testing/Testing_strategies#putting_together_a_testing_lab) per maggiori informazioni.

Assicurati di eseguire alcuni test su un dispositivo reale, in particolare su dispositivi mobili reali. I dispositivi mobili costano denaro, ovviamente, quindi consigliamo di condividere i dispositivi all'interno di un team se si desidera testare su molte piattaforme senza spendere troppo. Per un accesso cloud scalabile al testing su dispositivi reali, ti consigliamo anche di dare un'occhiata ad [App Live: la piattaforma interattiva di test delle app mobili di BrowserStack](https://www.browserstack.com/app-live).

## Prossimi passi

- Alcuni di questi software sono gratuiti, ma non tutti. [Scopri quanto costa fare qualcosa sul web](/it/docs/Learn_web_development/Howto/Tools_and_setup/How_much_does_it_cost).
- Se desideri saperne di più sugli editor di testo, leggi il nostro articolo su [come scegliere e installare un editor di testo](/it/docs/Learn_web_development/Howto/Tools_and_setup/Available_text_editors).
- Se ti stai chiedendo come pubblicare il tuo sito web sul web, guarda ["Come caricare file su un server web"](/it/docs/Learn_web_development/Howto/Tools_and_setup/Upload_files_to_a_web_server).
