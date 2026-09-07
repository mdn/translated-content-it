---
title: Web font
slug: Learn_web_development/Core/Text_styling/Web_fonts
l10n:
  sourceCommit: 916eb95f63de092d96ed1b1b13f3e2261739a8e2
---

{{PreviousMenuNext("Learn_web_development/Core/Text_styling/Styling_links", "Learn_web_development/Core/Text_styling/Typesetting_a_homepage", "Learn_web_development/Core/Text_styling")}}

Nel primo articolo del modulo sono state esplorate le funzionalità CSS di base disponibili per applicare stili a font e testo. In questo articolo si andrà oltre, esplorando in dettaglio i web font. Verrà illustrato come usare font personalizzati nella pagina web per consentire una maggiore varietà nella personalizzazione dello stile del testo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Strutturare i contenuti con HTML</a
        >,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Fondamenti dello stile CSS</a>,
        <a href="/it/docs/Learn_web_development/Core/Text_styling/Fundamentals">Stile fondamentale di testo e font</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
       <ul>
         <li>Comprendere che i web font consentono agli sviluppatori di andare oltre il set di font sicuri per il web e di usare font personalizzati nelle proprie applicazioni web.</li>
         <li>Configurazione di base — l'at-rule <code>@font-face</code> e i descrittori comuni.</li>
         <li>Uso di un web font con la proprietà <code>font-family</code>.</li>
         <li>Uso di servizi online per trovare web font e generare codice per web font.</li>
       </ul>
      </td>
    </tr>
  </tbody>
</table>

## Riepilogo delle famiglie di font

Come illustrato in [Stile fondamentale di testo e font](/it/docs/Learn_web_development/Core/Text_styling/Fundamentals), i font applicati all'HTML possono essere controllati tramite la proprietà {{cssxref("font-family")}}. Questa accetta uno o più nomi di famiglie di font. Quando visualizza una pagina web, un browser scorre un elenco di valori `font-family` finché non trova un font disponibile sul sistema in cui è in esecuzione:

```css
p {
  font-family: "Helvetica", "Trebuchet MS", "Verdana", sans-serif;
}
```

Questo sistema funziona bene, ma tradizionalmente le scelte di font degli sviluppatori web erano limitate. Esistono soltanto pochi font la cui disponibilità può essere garantita su tutti i sistemi comuni: i cosiddetti [font sicuri per il web](/it/docs/Learn_web_development/Core/Text_styling/Fundamentals#web_safe_fonts). È possibile usare lo stack di font per specificare font preferiti, seguiti da alternative sicure per il web e infine dal font di sistema predefinito. Tuttavia, ciò aumenta il carico di lavoro a causa dei test necessari per assicurarsi che i progetti funzionino con ogni font.

## Web font

Esiste un'alternativa che funziona bene. CSS consente di specificare file di font disponibili sul web da scaricare insieme al sito web quando viene visitato. Questo significa che qualsiasi browser che supporti questa funzionalità CSS può visualizzare i font scelti specificamente. Fantastico! La sintassi richiesta è simile alla seguente:

Prima di tutto, all'inizio del CSS è presente un ruleset {{cssxref("@font-face")}}, che specifica i file di font da scaricare:

```css
@font-face {
  font-family: "myFont";
  src: url("myFont.woff2");
}
```

Successivamente, viene usato il nome della famiglia di font specificato all'interno di {{cssxref("@font-face")}} per applicare il font personalizzato a qualunque elemento, come di consueto:

```css
html {
  font-family: "myFont", "Bitstream Vera Serif", serif;
}
```

La sintassi diventa un po' più complessa di così. Di seguito verranno forniti maggiori dettagli.

Ecco alcuni aspetti importanti da tenere a mente sui web font:

1. In genere i font non sono gratuiti da usare senza restrizioni. Occorre pagarli e/o rispettare altre condizioni di licenza, come attribuire il merito al creatore del font nel codice, oppure sul sito. Non si devono sottrarre font e usarli senza fornire un'adeguata attribuzione.
2. Tutti i browser principali supportano WOFF/WOFF2 (Web Open Font Format versioni 1 e 2). Anche browser più datati, come IE9 (rilasciato nel 2011), supportano il formato WOFF.
3. WOFF2 supporta l'intera specifica TrueType e OpenType, inclusi font variabili, font cromatici e raccolte di font.
4. L'ordine in cui vengono elencati i file di font è importante. Se al browser viene fornito un elenco di più file di font da scaricare, sceglierà il primo file di font che può usare. Per questo motivo, il primo formato elencato dovrebbe essere quello preferito, ovvero WOFF2, seguito dai formati più vecchi. I browser che non comprendono un formato passeranno quindi al formato successivo nell'elenco.
5. Se è necessario lavorare con browser legacy, occorre fornire per il download web font EOT (Embedded Open Type), TTF (TrueType Font) e SVG. Questo articolo spiega come usare il generatore di web font Transfonter per creare i file richiesti.

È possibile usare il [Firefox Font Editor](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/how_to/edit_fonts/index.html) per analizzare e manipolare i font usati nella pagina, siano essi web font o meno.

## Aggiungere web font personalizzati

Tenendo presente quanto detto, viene ora creato un esempio di web font di base partendo dai principi fondamentali. Usare i file [web-font-start.html](https://github.com/mdn/learning-area/blob/main/css/web-fonts/web-font-start.html) e [web-font-start.css](https://github.com/mdn/learning-area/blob/main/css/web-fonts/web-font-start.css) come punto di partenza per aggiungere il proprio codice (vedere l'[esempio dal vivo](https://mdn.github.io/learning-area/css/web-fonts/web-font-start.html)). Creare ora una copia di questi file in una nuova directory sul computer. Nel file `web-font-start.css` è presente del CSS minimale per gestire il layout di base e la tipografia dell'esempio.

### Trovare font

Per questo esempio verranno usati due web font: uno per le intestazioni e uno per il testo del corpo. Per iniziare, occorre trovare i file di font che contengono i font. I font sono creati da fonderie tipografiche e vengono archiviati in diversi formati di file. In genere esistono tre tipi di siti dai quali è possibile ottenere font:

- Un distributore di font gratuiti: è un sito che rende disponibili font gratuiti da scaricare, anche se potrebbero comunque esserci condizioni di licenza, come attribuire il merito al creatore del font. Alcuni esempi includono [DaFont](https://www.dafont.com/) e [Everything Fonts](https://everythingfonts.com/).
- Un distributore di font a pagamento: è un sito che rende disponibili font a pagamento, come [myfonts.com](https://www.myfonts.com/). È anche possibile acquistare font direttamente dalle fonderie tipografiche, ad esempio [Linotype](https://www.linotype.com/), [Monotype](https://www.monotype.com/) o [Exljbris](https://www.exljbris.com/).
- Un servizio di font online: è un sito che archivia e fornisce i font, semplificando l'intero processo. Per maggiori dettagli, vedere la sezione [Uso di un servizio di font online](#uso_di_un_servizio_di_font_online).

Troviamo alcuni font. Andare su [DaFont](https://www.dafont.com/) e scegliere due font: un font gradevole e interessante per le intestazioni, magari un bel font display o slab serif, e un font un po' meno appariscente e più leggibile per i paragrafi. Dopo aver trovato un font, premere il pulsante di download e salvare il file nella stessa directory dei file HTML e CSS salvati in precedenza. Non importa se sono font TrueType (TTF) o OpenType (OTF).

Decomprimere i due pacchetti di font. I web font sono in genere distribuiti in file ZIP contenenti i file di font e le informazioni sulla licenza. Nel pacchetto potrebbero essere presenti più file di font: alcuni font vengono distribuiti come famiglia con diverse varianti disponibili, ad esempio thin, medium, bold, italic, thin italic e così via. Per questo esempio, è sufficiente considerare un solo file di font per ciascuna scelta.

### Generare il codice richiesto

Ora occorre generare il codice richiesto, inclusi i formati di font. Per ogni font, seguire questi passaggi:

1. Assicurarsi di aver soddisfatto tutti i requisiti di licenza se il font verrà usato in un progetto commerciale e/o web.
2. Andare al [generatore di web font](https://transfonter.org/) Transfonter.
3. Caricare i due file di font usando il pulsante _Upload your fonts_.
4. Fare clic su _Convert_.
5. Fare clic su _Download_.

Dopo avere scaricato il file ZIP, decomprimerlo e spostarlo nella stessa directory dei file HTML e CSS.

### Implementare il codice nella demo

All'interno della directory decompressa saranno presenti alcuni elementi utili:

- Due versioni di ciascun font: i file `.woff` e `.woff2`.
- Un file HTML dimostrativo per ogni font: caricarli nel browser per vedere l'aspetto del font in diversi contesti di utilizzo.
- Un file `stylesheet.css`, che contiene il codice @font-face generato necessario.

Per implementare questi font nella demo, seguire questi passaggi:

1. Rinominare la directory decompressa con un nome semplice e immediato, come `fonts`.
2. Aprire il file `stylesheet.css` e copiare i due ruleset `@font-face` nel file `web-font-start.css`: devono essere inseriti all'inizio assoluto, prima di qualsiasi altro CSS, poiché i font devono essere importati prima di poterli usare nel sito.
3. Ciascuna funzione `url()` punta a un file di font da importare nel CSS. Occorre assicurarsi che i percorsi dei file siano corretti, quindi aggiungere `fonts/` all'inizio di ciascun percorso, adattando secondo necessità.
4. A questo punto è possibile usare questi font negli stack di font, proprio come qualsiasi font sicuro per il web o font di sistema predefinito. Per esempio:

   ```css
   @font-face {
     font-family: "zantrokeregular";
     src:
       url("fonts/zantroke-webfont.woff2") format("woff2"),
       url("fonts/zantroke-webfont.woff") format("woff");
     font-weight: normal;
     font-style: normal;
     font-display: swap;
   }
   ```

   ```css
   font-family: "zantrokeregular", serif;
   ```

Il risultato dovrebbe essere una pagina dimostrativa con alcuni bei font. Poiché font diversi vengono creati con dimensioni diverse, potrebbe essere necessario regolare dimensione, spaziatura e così via, per migliorare l'aspetto generale.

![Il progetto finale di un esercizio sui web font. La pagina contiene due intestazioni e tre paragrafi. La pagina contiene font diversi e testo di dimensioni diverse.](web-font-example.png)

> [!NOTE]
> In caso di problemi nel far funzionare questo esempio, è possibile confrontare la propria versione con i file completati: vedere [web-font-finished.html](https://github.com/mdn/learning-area/blob/main/css/web-fonts/web-font-finished.html) e [web-font-finished.css](https://github.com/mdn/learning-area/blob/main/css/web-fonts/web-font-finished.css). È anche possibile scaricare il [codice da GitHub](https://github.com/mdn/learning-area/tree/main/css/web-fonts) o [eseguire l'esempio completato dal vivo](https://mdn.github.io/learning-area/css/web-fonts/web-font-finished.html).

## Uso di un servizio di font online

I servizi di font online in genere archiviano e forniscono i font, quindi non è necessario preoccuparsi di scrivere il codice `@font-face`. Invece, di solito è sufficiente inserire una o due semplici righe di codice nel sito per far funzionare tutto. Alcuni esempi includono [Adobe Fonts](https://fonts.adobe.com/) e [Cloud.typography](https://www.typography.com/webfonts). La maggior parte di questi servizi è basata su abbonamento, con la notevole eccezione di [Google Fonts](https://fonts.google.com/), un utile servizio gratuito, specialmente per test rapidi e per scrivere demo.

La maggior parte di questi servizi è facile da usare. Diamo una rapida occhiata a Google Fonts per comprendere il concetto. Anche in questo caso, usare come punto di partenza copie di `web-font-start.html` e `web-font-start.css`.

1. Andare su [Google Fonts](https://fonts.google.com/).
2. Trovare un paio di font di proprio gradimento usando i filtri e la barra di ricerca.
3. Fare clic su un font per aprire la relativa pagina dei dettagli.
4. Dopo aver trovato un font di proprio gradimento, fare clic sul pulsante **Get font** nella relativa pagina dei dettagli per aggiungerlo alla pagina dei font selezionati. Per aggiungere un altro font, fare clic sul pulsante Indietro del browser e cercare di nuovo.
5. Al termine della selezione dei font, fare clic sul pulsante **Get embed code** nella pagina dei font selezionati e copiare gli elementi `<link>` forniti.
6. Incollare gli elementi `<link>` nell'elemento `<head>` del documento HTML, sopra eventuali collegamenti a stylesheet già presenti.
7. Copiare le regole CSS `font-family` fornite e usarle nel CSS per applicare i font, in modo simile alla procedura precedente.

> [!NOTE]
> È possibile trovare una versione completata in [google-font.html](https://github.com/mdn/learning-area/blob/main/css/web-fonts/google-font.html) e [google-font.css](https://github.com/mdn/learning-area/blob/main/css/web-fonts/google-font.css), per confrontare il proprio lavoro con il nostro ([vederla dal vivo](https://mdn.github.io/learning-area/css/web-fonts/google-font.html)).

## @font-face più nel dettaglio

Esploriamo la sintassi `@font-face` generata da Transfonter. I ruleset saranno simili a questo:

```css
@font-face {
  font-family: "zantrokeregular";
  src:
    url("zantroke-webfont.woff2") format("woff2"),
    url("zantroke-webfont.woff") format("woff");
  font-weight: normal;
  font-style: normal;
  font-display: swap;
}
```

Esaminiamola per capire cosa fa:

- `font-family`: questa riga specifica il nome con cui fare riferimento al font. Può essere qualsiasi nome, purché venga usato in modo coerente in tutto il CSS.
- `src`: queste righe specificano i percorsi ai file di font da importare nel CSS, ovvero la parte `url`, e il formato di ciascun file di font, ovvero la parte `format`. Quest'ultima parte è facoltativa in ogni caso, ma è utile dichiararla perché consente ai browser di determinare più rapidamente quale font possono usare. È possibile elencare più dichiarazioni, separate da virgole. Poiché il browser le cercherà secondo le regole della cascata, è consigliabile indicare all'inizio i formati preferiti, come WOFF2.
- {{cssxref("@font-face/font-weight", "font-weight")}}/{{cssxref("@font-face/font-style", "font-style")}}: queste righe specificano quale peso ha il font e se è in corsivo oppure no. Se vengono importati più pesi dello stesso font, è possibile specificarne peso/stile e quindi usare valori diversi di `font-weight`/`font-style` per scegliere tra essi, anziché dover assegnare nomi diversi a tutti i membri della famiglia di font. [@font-face tip: define font-weight and font-style to keep your CSS simple](https://www.456bereastreet.com/archive/201012/font-face_tip_define_font-weight_and_font-style_to_keep_your_css_simple/) di Roger Johansson mostra più nel dettaglio come procedere.
- {{cssxref("@font-face/font-display", "font-display")}}: questa riga specifica come viene visualizzato il font durante il caricamento.

> [!NOTE]
> È anche possibile specificare particolari valori {{cssxref("@font-face/font-variation-settings", "font-variation-settings")}} e {{cssxref("@font-face/font-stretch", "font-stretch")}} per i web font. Nei browser più recenti è inoltre possibile specificare un valore {{cssxref("@font-face/unicode-range", "unicode-range")}}, ovvero un intervallo specifico di caratteri che si desidera usare dal web font. Nei browser che lo supportano, il font verrà scaricato solo se la pagina contiene quei caratteri specificati, evitando download non necessari. [Creating Custom Font Stacks with Unicode-Range](https://24ways.org/2011/creating-custom-font-stacks-with-unicode-range/) di Drew McLellan fornisce alcune idee utili su come sfruttare questa funzionalità.

## Riepilogo

Dopo aver completato gli articoli sui fondamenti dello stile del testo, è il momento di verificare la comprensione con la sfida del modulo: [Comporre tipograficamente la homepage di una scuola comunitaria](/it/docs/Learn_web_development/Core/Text_styling/Typesetting_a_homepage).

Dopo aver completato la sfida, è possibile passare allo studio del [layout CSS](/it/docs/Learn_web_development/Core/CSS_layout).

## Vedere anche

- [Guida ai font variabili](/it/docs/Web/CSS/Guides/Fonts/Variable_fonts)
- [Conoscenze sui font](https://fonts.google.com/knowledge), Google Fonts

{{PreviousMenuNext("Learn_web_development/Core/Text_styling/Styling_links", "Learn_web_development/Core/Text_styling/Typesetting_a_homepage", "Learn_web_development/Core/Text_styling")}}
