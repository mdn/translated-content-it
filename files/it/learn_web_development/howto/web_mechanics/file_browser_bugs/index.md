---
title: Quando e come segnalare bug nei browser
slug: Learn_web_development/Howto/Web_mechanics/File_browser_bugs
l10n:
  sourceCommit: 423161782178b119c64cd0b41bff8df20dc84a56
---

I browser sono software e, come qualsiasi software, possono avere bug. A volte il sito web in fase di sviluppo potrebbe non comportarsi come previsto o come indicato dalla documentazione, ad esempio MDN o le specifiche. Ciò potrebbe indicare un bug nel codice, un bug nella documentazione (speriamo di no!) oppure un bug nel browser usato per testare il sito web. In questo articolo verrà illustrato come capire quale sia il caso e come segnalare un bug se il problema risulta essere nel browser.

## Di chi è il bug?

Prima di segnalare un bug del browser, è necessario confermare che sia effettivamente un bug nel browser. Il problema può provenire da uno di quattro punti: il codice, la documentazione, il browser o la specifica. È importante escludere le altre possibilità prima di segnalare un bug al browser. In generale, le specifiche sono la fonte più autorevole; sia i browser sia la documentazione seguono le specifiche, ma possono comunque contenere errori. Per quanto riguarda il codice... beh, è sempre consigliabile ricontrollare eventuali errori di battitura e logici prima di presumere che si tratti di un bug del browser.

### Creare un caso di test

Il primo passo per identificare l'origine del problema consiste nel creare un caso di test minimo che riproduca il bug. Dovrebbe essere piccolo e autonomo, preferibilmente un singolo file HTML con CSS e JavaScript incorporati, senza dipendenze esterne o codice non correlato. Questo è utile per due ragioni:

- Riduce al minimo la possibilità che il problema sia causato dal proprio codice o da una dipendenza esterna.
- In ogni caso è necessario fornirne uno per discuterne con chiunque, ad esempio quando si segnala un bug.

Ad esempio, il seguente sarebbe un buon caso di test per un bug relativo alla pseudo-classe {{cssxref(":autofill")}}. Si noti come sia stato ridotto al minimo indispensabile, rinunciando quindi alle buone pratiche come l'inclusione del doctype, dei tag `<head>` e `<body>` o delle etichette per gli input. Va bene così, perché il codice pertinente è comunque presente.

```html
<style>
  :autofill {
    border: 3px solid darkorange;
  }
</style>
<input id="name" name="name" type="text" autocomplete="name" />
<input id="email" name="email" type="email" autocomplete="email" />
```

### Testare il codice

È possibile salvare il codice HTML localmente e [servirlo tramite un server di test](/it/docs/Learn_web_development/Howto/Tools_and_setup/set_up_a_local_testing_server), oppure utilizzare un servizio online come [JSFiddle](https://jsfiddle.net/) o [CodePen](https://codepen.io/) per creare una demo dal vivo.

Il modo più semplice per verificare se il problema è un bug del browser consiste nell'aprire il caso di test in [più browser](/it/docs/Learn_web_development/Extensions/Testing/Introduction). Se si osserva un comportamento divergente tra i browser, è più probabile che si tratti di un bug del browser.

> [!NOTE]
> È possibile eseguire altri passaggi per isolare il problema, ad esempio effettuare il test in una finestra privata, disabilitare le estensioni o svuotare la cache. È opportuno provare anche queste soluzioni prima di segnalare il bug.

### Verificare lo stato dell'implementazione

Iniziare fidandosi della documentazione e analizzando i browser il cui comportamento non corrisponde a essa. Non tutti i comportamenti inattesi sono bug. A volte i browser possono implementare una funzionalità o un comportamento non ancora integrato nella specifica e che, di conseguenza, ha meno probabilità di essere documentato. Un'altra possibilità è che una funzionalità sia descritta nella specifica ma non sia ancora implementata in alcun browser, il che significa anche che potrebbe non essere documentata.

A questo punto, è opportuno controllare altre fonti per determinare lo stato dell'implementazione. Ecco alcuni luoghi in cui cercare:

- **Tabelle di Compatibilità del browser di MDN**: nella sezione "Compatibilità del browser" delle pagine di riferimento (ad esempio, consultare [questa sezione](/it/docs/Web/CSS/Reference/Values/basic-shape/shape#browser_compatibility) nella pagina della funzione CSS `shape()`), sono disponibili informazioni sui browser che supportano una funzionalità e in quale misura. Questo potrebbe indicare che una funzionalità non è implementata nel browser di destinazione o che è implementata solo parzialmente, ovvero presenta bug o limitazioni note.
- **Repository delle specifiche**: gli enti di standardizzazione come [WHATWG](https://github.com/whatwg) (per DOM, HTML, fetch e altro), [CSSWG](https://github.com/w3c/csswg-drafts) (per CSS) e [TC39](https://github.com/tc39) (per JavaScript) lavorano tutti pubblicamente su GitHub. È possibile verificare se una specifica è stata modificata di recente o se esiste una issue aperta relativa alla funzionalità in fase di test.
- **Forum della comunità**: la [comunità MDN](/it/docs/MDN/Community/Communication_channels) è un ottimo punto di partenza, così come altri forum sullo sviluppo web. Questi sono luoghi adatti per porre domande sul fatto che i browser non abbiano ancora implementato qualcosa o sull'esistenza di un bug noto.
- **Issue tracker del browser in fase di test**: se si scopre che è già stata segnalata una issue relativa al problema, ciò conferma che il bug è reale e non è necessario fare altro. Gli issue tracker saranno trattati nella sezione successiva.

Naturalmente, anche se tutti i browser si comportano allo stesso modo, potrebbe comunque esserci un bug in tutti loro, oppure potrebbe essere un solo browser a implementare il comportamento previsto. La documentazione potrebbe essere obsoleta o errata. Per esserne certi, è opportuno considerare la specifica come fonte di verità, tranne nel raro caso in cui i browser implementino funzionalità prima della specifica. In ogni pagina di riferimento MDN, nella sezione "Specifications" sono disponibili collegamenti alle specifiche pertinenti (vedere questo [esempio](/it/docs/Web/CSS/Reference/Values/basic-shape/shape#specifications)). Leggere la specifica per verificare quale dovrebbe essere il comportamento. A volte le specifiche possono essere difficili da comprendere, poiché sono destinate agli ingegneri dei browser, ma vale la pena fare del proprio meglio.

Se tutti i browser e la specifica sono coerenti, ma MDN è errato, è possibile prendere in considerazione l'idea di [contribuire](/it/docs/MDN/Community/Getting_started)!

## Bug tracker dei browser

Ogni browser ha il proprio bug tracker, nel quale è possibile cercare bug esistenti e segnalarne di nuovi. L'interfaccia e il processo potrebbero sembrare inizialmente poco familiari, ma di solito sono presenti istruzioni. La tabella seguente elenca i bug tracker dei principali browser:

| Browser         | Bug tracker                                           |
| --------------- | ----------------------------------------------------- |
| Apple Safari    | [WebKit Bugzilla](https://webkit.org/reporting-bugs/) |
| Google Chrome   | [Chromium Issues](https://issues.chromium.org/issues) |
| Mozilla Firefox | [Mozilla Bugzilla](https://bugzilla.mozilla.org/)     |
| Opera           | [Opera Bug Wizard](https://bugs.opera.com/wizard/)    |

Cercare segnalazioni di bug esistenti prima di crearne una nuova. Se si trova una segnalazione esistente che corrisponde al problema riscontrato, è possibile aggiungere un commento con i risultati ottenuti, ad esempio se è stata trovata una soluzione alternativa o se si dispone di ulteriori informazioni sul bug. Tuttavia, non aggiungere commenti come "Ho trovato anch'io questo bug", perché non aggiungono alcun valore. Se non è possibile trovare un bug esistente, è possibile segnalarne uno nuovo: qualcuno comunicherà se è un duplicato.

Quando si segnala un nuovo bug, assicurarsi di includere il caso di test minimo e tutte le altre informazioni richieste dal modulo di segnalazione, ad esempio la versione del browser, i risultati attesi rispetto a quelli effettivi e screenshot. Alcuni bug tracker potrebbero anche richiedere di selezionare un componente o una categoria per il bug, come rendering o networking. Gli sviluppatori del browser usano queste etichette per organizzare il lavoro. Se non è chiaro cosa scegliere, fare la scelta che sembra più appropriata: qualcuno la riassegnerà se necessario.

## Segnalare bug per software non browser

Se il bug è relativo a software non browser che può integrarsi con il browser, è necessario segnalarlo al fornitore del software pertinente. La tabella seguente elenca alcune tecnologie assistive e dove segnalare i relativi bug:

| Software                                                                    | Dove segnalare                                                                               |
| --------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| [Freedom Scientific JAWS](https://vispero.com/jaws-screen-reader-software/) | [Modulo di assistenza tecnica JAWS](https://support.freedomscientific.com/Forms/TechSupport) |
| [Non Visual Desktop Access (NVDA)](https://www.nvaccess.org/)               | [Segnalare bug di NVDA](https://github.com/nvaccess/nvda)                                    |
