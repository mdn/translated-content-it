---
title: Web fonts
slug: Learn_web_development/Core/Text_styling/Web_fonts
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Text_styling/Styling_links", "Learn_web_development/Core/Text_styling/Typesetting_a_homepage", "Learn_web_development/Core/Text_styling")}}

Nel primo articolo del modulo, abbiamo esplorato le caratteristiche di base di CSS disponibili per stilizzare i font e il testo. In questo articolo andremo oltre, esplorando i web font in dettaglio. Vedremo come utilizzare font personalizzati con la tua pagina web per consentire una stilizzazione del testo più varia e personalizzata.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Strutturare il contenuto con HTML</a
        >,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Basi dello stile CSS</a>,
        <a href="/it/docs/Learn_web_development/Core/Text_styling/Fundamentals">Fondamenti di stilizzazione del testo e dei font</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
       <ul>
         <li>Comprendere che i web font permettono agli sviluppatori di andare oltre il set di font sicuri per il web e utilizzare font personalizzati nelle loro applicazioni web.</li>
         <li>Impostazione di base — la regola <code>@font-face</code>, e descrittori comuni.</li>
         <li>Usare un web font con la proprietà <code>font-family</code>.</li>
         <li>Utilizzare un servizio online per trovare i web font e generare il codice dei web font, ad esempio, <a href="https://www.fontsquirrel.com/">Font Squirrel</a> o <a href="https://fonts.google.com/">Google Fonts</a>.</li>
       </ul>
      </td>
    </tr>
  </tbody>
</table>

## Riepilogo delle famiglie di font

Come abbiamo visto in [Fondamenti di stilizzazione del testo e dei font](/it/docs/Learn_web_development/Core/Text_styling/Fundamentals), i font applicati al tuo HTML possono essere controllati usando la proprietà {{cssxref("font-family")}}. Questa accetta uno o più nomi di famiglie di font. Quando visualizza una pagina web, un browser scorrerà una lista di valori di font-family finché non trova un font disponibile sul sistema su cui sta operando:

```css
p {
  font-family: Helvetica, "Trebuchet MS", Verdana, sans-serif;
}
```

Questo sistema funziona bene, ma tradizionalmente le scelte di font dei web developer erano limitate. Ci sono solo una manciata di font che puoi garantire disponibili su tutti i sistemi comuni — i cosiddetti [font sicuri per il web](/it/docs/Learn_web_development/Core/Text_styling/Fundamentals#web_safe_fonts). Puoi utilizzare lo stack di font per specificare font preferiti, seguiti da alternative sicure per il web, seguiti dal font di sistema predefinito. Tuttavia, questo aumenta il tuo carico di lavoro a causa dei test richiesti per assicurarsi che i tuoi design funzionino con ciascun font.

## Web font

C'è un'alternativa che funziona bene. CSS ti permette di specificare file di font, disponibili sul web, da scaricare insieme al tuo sito quando viene acceduto. Ciò significa che qualsiasi browser che supporta questa caratteristica CSS può visualizzare i font che hai scelto specificamente. Fantastico! La sintassi richiesta assomiglia a questa:

Prima di tutto, hai un set di regole {{cssxref("@font-face")}} all'inizio del CSS, che specifica il/i file di font da scaricare:

```css
@font-face {
  font-family: "myFont";
  src: url("myFont.woff2");
}
```

Sotto di questo si utilizza il nome della famiglia di font specificata all'interno di {{cssxref("@font-face")}} per applicare il tuo font personalizzato a qualsiasi cosa tu voglia, come al solito:

```css
html {
  font-family: "myFont", "Bitstream Vera Serif", serif;
}
```

La sintassi diventa un po' più complessa di così. Approfondiremo di seguito alcuni dettagli.

Ecco alcune cose importanti da tenere a mente sui web font:

1. Generalmente i font non sono liberi da usare senza restrizioni. Devi pagarli e/o seguire altre condizioni di licenza, come accreditare il creatore del font nel tuo codice (o sul tuo sito). Non dovresti rubare font e usarli senza dare il dovuto credito.
2. Tutti i principali browser supportano WOFF/WOFF2 (Web Open Font Format versioni 1 e 2). Anche browser più vecchi come IE9 (rilasciato nel 2011) supportano il formato WOFF.
3. WOFF2 supporta l'intera specifica TrueType e OpenType, inclusi font variabili, font cromatici e collezioni di font.
4. L'ordine in cui elenchi i file di font è importante. Se fornisci al browser un elenco di file di font multipli da scaricare, il browser sceglierà il primo file di font che è in grado di utilizzare. Ecco perché il formato che elenchi per primo dovrebbe essere quello preferito — cioè WOFF2 — con i formati più vecchi elencati successivamente. I browser che non comprendono un formato passeranno quindi al formato successivo nell'elenco.
5. Se hai bisogno di lavorare con browser legacy, dovresti fornire EOT (Embedded Open Type), TTF (TrueType Font) e SVG web font per il download. Questo articolo spiega come usare il Fontsquirrel Webfont Generator per generare i file richiesti.

Puoi usare il [Firefox Font Editor](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/how_to/edit_fonts/index.html) per indagare e manipolare i font in uso sulla tua pagina, che si tratti di web font o meno.

## Apprendimento attivo: un esempio di web font

Con questo in mente, costruiamo un esempio di base di web font dai principi fondamentali. È difficile dimostrare questo utilizzando un esempio live integrato. Quindi invece ti invitiamo a seguire i passaggi dettagliati nelle sezioni seguenti per avere un'idea del processo.

Dovresti usare i file [web-font-start.html](https://github.com/mdn/learning-area/blob/main/css/styling-text/web-fonts/web-font-start.html) e [web-font-start.css](https://github.com/mdn/learning-area/blob/main/css/styling-text/web-fonts/web-font-start.css) come punto di partenza per aggiungere il tuo codice (vedi l'[esempio live](https://mdn.github.io/learning-area/css/styling-text/web-fonts/web-font-start.html)). Fai una copia di questi file in una nuova directory sul tuo computer ora. Nel file `web-font-start.css`, troverai alcuni CSS minimi per gestire il layout di base e l'impostazione del testo dell'esempio.

### Trovare i font

Per questo esempio, utilizzeremo due web font: uno per le intestazioni e uno per il testo del corpo. Per cominciare, dobbiamo trovare i file di font che contengono i font. I font sono creati da fonderie di caratteri e sono memorizzati in diversi formati di file. Generalmente ci sono tre tipi di siti dove puoi ottenere i font:

- Un distributore di font gratuiti: Questo è un sito che rende disponibili font gratuiti per il download (potrebbero esserci ancora alcune condizioni di licenza, come accreditare il creatore del font). Esempi includono [Font Squirrel](https://www.fontsquirrel.com/), [DaFont](https://www.dafont.com/), e [Everything Fonts](https://everythingfonts.com/).
- Un distributore di font a pagamento: Questo è un sito che rende disponibili i font a pagamento, come [fonts.com](https://www.fonts.com/) o [myfonts.com](https://www.myfonts.com/). Puoi anche acquistare i font direttamente dalle fonderie di caratteri, ad esempio [Linotype](https://www.linotype.com/), [Monotype](https://www.monotype.com/), o [Exljbris](https://www.exljbris.com/).
- Un servizio di font online: Questo è un sito che archivia e fornisce i font per te, rendendo l'intero processo più semplice. Vedi la sezione [Usare un servizio di font online](#usare_un_servizio_di_font_online) per maggiori dettagli.

Troviamo alcuni font! Vai su [Font Squirrel](https://www.fontsquirrel.com/) e scegli due font: un bel font interessante per le intestazioni (magari un bel display o slab serif) e un font leggermente meno appariscente e più leggibile per i paragrafi. Quando hai trovato un font, premi il pulsante di download e salva il file nella stessa directory dei file HTML e CSS che hai salvato in precedenza. Non importa se sono TTF (True Type Fonts) o OTF (Open Type Fonts).

Decomprimi i due pacchetti di font (i web font sono solitamente distribuiti in file ZIP contenenti il/i file di font e le informazioni sulla licenza). Potresti trovare più file di font nel pacchetto — alcuni font sono distribuiti come una famiglia con varianti diverse disponibili, ad esempio sottile, medio, grassetto, corsivo, sottile corsivo, ecc. Per questo esempio, vogliamo solo che tu ti occupi di un singolo file di font per ciascuna scelta.

> [!NOTE]
> In Font Squirrel, sotto la sezione "Find fonts" nella colonna di destra, puoi cliccare sui diversi tag e classificazioni per filtrare le scelte visualizzate.

### Generare il codice richiesto

Ora dovrai generare il codice richiesto (e i formati di font). Per ciascun font, segui questi passaggi:

1. Assicurati di aver soddisfatto qualsiasi requisito di licenza se intendi utilizzare questo in un progetto commerciale e/o Web.
2. Vai al Fontsquirrel [Webfont Generator](https://www.fontsquirrel.com/tools/webfont-generator).
3. Carica i tuoi due file di font utilizzando il pulsante _Upload Fonts_.
4. Seleziona la casella di controllo con l'etichetta "Yes, the fonts I'm uploading are legally eligible for web embedding."
5. Clicca su _Download your kit_.

Dopo che il generatore ha terminato l'elaborazione, dovrebbe restituirti un file ZIP da scaricare. Salvalo nella stessa directory dei tuoi file HTML e CSS.

### Implementare il codice nel tuo demo

A questo punto, decomprimi il kit di web font che hai appena generato. All'interno della directory decompressa vedrai alcuni elementi utili:

- Due versioni di ciascun font: i file `.woff`, `.woff2`.
- Un file HTML dimostrazione per ciascun font — carica questi nel tuo browser per vedere come apparirà il font in diversi contesti d'uso.
- Un file `stylesheet.css`, che contiene il codice @font-face generato che ti servirà.

Per implementare questi font nel tuo demo, segui questi passaggi:

1. Rinomina la directory decompressa con un nome facile e semplice, come `fonts`.
2. Apri il file `stylesheet.css` e copia i due set di regole `@font-face` nel tuo file `web-font-start.css` — devi metterli all'inizio, prima di qualsiasi tuo CSS, poiché i font devono essere importati prima di poterli utilizzare sul tuo sito.
3. Ciascuna delle funzioni `url()` punta a un file di font che vogliamo importare nel nostro CSS. Dobbiamo assicurarci che i percorsi ai file siano corretti, quindi aggiungi `fonts/` all'inizio di ciascun percorso (aggiusta come necessario).
4. Ora puoi usare questi font nel tuo stack di font, proprio come qualsiasi font sicuro per il web o di sistema predefinito. Per esempio:

   ```css
   @font-face {
     font-family: "zantrokeregular";
     src:
       url("fonts/zantroke-webfont.woff2") format("woff2"),
       url("fonts/zantroke-webfont.woff") format("woff");
     font-weight: normal;
     font-style: normal;
   }
   ```

   ```css
   font-family: "zantrokeregular", serif;
   ```

Dovresti finire con una pagina demo con dei bei font implementati. Poiché i font diversi sono creati a dimensioni differenti, potrebbe essere necessario regolare la dimensione, la spaziatura, ecc., per sistemare l'aspetto e la sensazione.

![Il design finale di un esercizio di apprendimento attivo sui web font. La pagina ha due intestazioni e tre paragrafi. La pagina contiene font diversi e testo a dimensioni diverse.](web-font-example.png)

> [!NOTE]
> Se hai problemi a far funzionare il tutto, sentiti libero di confrontare la tua versione con i nostri file finali — vedi [web-font-finished.html](https://github.com/mdn/learning-area/blob/main/css/styling-text/web-fonts/web-font-finished.html) e [web-font-finished.css](https://github.com/mdn/learning-area/blob/main/css/styling-text/web-fonts/web-font-finished.css). Puoi anche scaricare il [codice da GitHub](https://github.com/mdn/learning-area/tree/main/css/styling-text/web-fonts) o [visualizzare l'esempio finale live](https://mdn.github.io/learning-area/css/styling-text/web-fonts/web-font-finished.html).

## Usare un servizio di font online

I servizi di font online solitamente archiviano e forniscono i font per te, così non devi preoccuparti di scrivere il codice `@font-face`. Invece, generalmente devi solo inserire una o due righe di codice nel tuo sito per fare in modo che tutto funzioni. Esempi includono [Adobe Fonts](https://fonts.adobe.com/) e [Cloud.typography](https://www.typography.com/webfonts). La maggior parte di questi servizi sono basati su abbonamento, con l'eccezione notevole di [Google Fonts](https://fonts.google.com/), un servizio gratuito utile, soprattutto per lavori di test rapidi e per scrivere demo.

La maggior parte di questi servizi sono facili da usare, quindi non li copriremo in dettaglio. Diamo uno sguardo rapido a Google Fonts per farti un'idea. Ancora una volta, usa copie di `web-font-start.html` e `web-font-start.css` come punto di partenza.

1. Vai su [Google Fonts](https://fonts.google.com/).
2. Cerca i tuoi font preferiti o usa i filtri in cima alla pagina per visualizzare i tipi di font che desideri scegliere e seleziona un paio di font che ti piacciono.
3. Per selezionare una famiglia di font, clicca sull'anteprima del font e premi il pulsante ⊕ accanto al font.
4. Quando hai scelto le famiglie di font, premi il pulsante _View your selected families_ nell'angolo in alto a destra della pagina.
5. Nella schermata risultante, devi prima copiare la riga di codice HTML mostrata e incollarla nell'head del tuo file HTML. Mettila sopra l'elemento {{htmlelement("link")}} esistente, in modo che il font venga importato prima di tentare di usarlo nel tuo CSS.
6. Quindi, devi copiare le dichiarazioni CSS elencate nel tuo CSS come appropriato, per applicare i font personalizzati al tuo HTML.

> [!NOTE]
> Puoi trovare una versione completata su [google-font.html](https://github.com/mdn/learning-area/blob/main/css/styling-text/web-fonts/google-font.html) e [google-font.css](https://github.com/mdn/learning-area/blob/main/css/styling-text/web-fonts/google-font.css), se hai bisogno di controllare il tuo lavoro con il nostro ([vedi live](https://mdn.github.io/learning-area/css/styling-text/web-fonts/google-font.html)).

## @font-face in dettaglio

Esploriamo quella sintassi `@font-face` generata per te da Fontsquirrel. I set di regole appariranno in questo modo:

```css
@font-face {
  font-family: "zantrokeregular";
  src:
    url("zantroke-webfont.woff2") format("woff2"),
    url("zantroke-webfont.woff") format("woff");
  font-weight: normal;
  font-style: normal;
}
```

Vediamo cosa fa:

- `font-family`: Questa riga specifica il nome con cui vuoi riferirti al font. Può essere qualsiasi cosa tu desideri purché lo usi in modo coerente nel tuo CSS.
- `src`: Queste righe specificano i percorsi ai file di font da importare nel tuo CSS (la parte `url`) e il formato di ciascun file di font (la parte `format`). La parte successiva in ciascun caso è opzionale, ma è utile dichiarare perché consente ai browser di determinare più rapidamente quale font possono utilizzare. È possibile elencare dichiarazioni multiple, separate da virgole. Poiché il browser le cercherà secondo le regole del cascade, è meglio dichiarare i tuoi formati preferiti, come WOFF2, all'inizio.
- {{cssxref("font-weight")}}/{{cssxref("font-style")}}: Queste righe specificano che peso ha il font e se è in corsivo o meno. Se stai importando pesi multipli dello stesso font, puoi specificare quale sia il loro peso/stile e quindi usare valori diversi di {{cssxref("font-weight")}}/{{cssxref("font-style")}} per scegliere tra di essi, piuttosto che dover chiamare tutti i diversi membri della famiglia di font con nomi diversi. [@font-face tip: define font-weight and font-style to keep your CSS simple](https://www.456bereastreet.com/archive/201012/font-face_tip_define_font-weight_and_font-style_to_keep_your_css_simple/) di Roger Johansson mostra cosa fare in maggior dettaglio.

> [!NOTE]
> Puoi anche specificare valori particolari di {{cssxref("font-variant")}} e {{cssxref("font-stretch")}} per i tuoi web font. Nei browser più recenti, puoi anche specificare un valore di {{cssxref("@font-face/unicode-range", "unicode-range")}}, che è un intervallo specifico di caratteri che potresti voler utilizzare dal web font. Nei browser che supportano, il font verrà scaricato solo se la pagina contiene quei caratteri specificati, risparmiando download non necessari. [Creating Custom Font Stacks with Unicode-Range](https://24ways.org/2011/creating-custom-font-stacks-with-unicode-range/) di Drew McLellan fornisce alcune idee utili su come avvalersi di questo.

## Riepilogo

Ora che hai lavorato attraverso i nostri articoli sui fondamenti della stilizzazione del testo, è il momento di testare la tua comprensione con la nostra sfida per il modulo: Impostazione dei tipi per una homepage di una scuola comunitaria.

## Vedi anche

- [Guida ai font variabili](/it/docs/Web/CSS/CSS_fonts/Variable_fonts_guide)
- [Conoscenza dei font](https://fonts.google.com/knowledge), Google Fonts

{{PreviousMenuNext("Learn_web_development/Core/Text_styling/Styling_links", "Learn_web_development/Core/Text_styling/Typesetting_a_homepage", "Learn_web_development/Core/Text_styling")}}
