---
title: Font web
slug: Learn_web_development/Core/Text_styling/Web_fonts
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Text_styling/Styling_links", "Learn_web_development/Core/Text_styling/Typesetting_a_homepage", "Learn_web_development/Core/Text_styling")}}

Nel primo articolo del modulo, abbiamo esplorato le caratteristiche base del CSS disponibili per lo styling dei font e del testo. In questo articolo approfondiremo ulteriormente, esplorando in dettaglio i font web. Vedremo come utilizzare font personalizzati con la sua pagina web per consentire uno stile del testo più vario e personalizzato.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Strutturare il contenuto con HTML</a
        >,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Elementi base di CSS Styling</a>,
        <a href="/it/docs/Learn_web_development/Core/Text_styling/Fundamentals">Fondamenti di stile del testo e dei font</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
       <ul>
         <li>Capire che i font web permettono agli sviluppatori di andare oltre il set di font sicuri per il web e utilizzare font personalizzati nelle loro applicazioni web.</li>
         <li>Impostazione di base — l'at-rule <code>@font-face</code> e descrittori comuni.</li>
         <li>Utilizzare un font web con la proprietà <code>font-family</code>.</li>
         <li>Utilizzare un servizio online per trovare font web e generare codice per i font web, ad esempio, <a href="https://www.fontsquirrel.com/">Font Squirrel</a> o <a href="https://fonts.google.com/">Google Fonts</a>.</li>
       </ul>
      </td>
    </tr>
  </tbody>
</table>

## Riepilogo delle famiglie di font

Come abbiamo visto in [Fondamenti di stile del testo e dei font](/it/docs/Learn_web_development/Core/Text_styling/Fundamentals), i font applicati al suo HTML possono essere controllati utilizzando la proprietà {{cssxref("font-family")}}. Questo accetta uno o più nomi di famiglie di font. Quando viene visualizzata una pagina web, un browser percorrerà un elenco di valori di font-family finché non trova un font disponibile sul sistema su cui è in esecuzione:

```css
p {
  font-family: Helvetica, "Trebuchet MS", Verdana, sans-serif;
}
```

Questo sistema funziona bene, ma tradizionalmente le scelte di font degli sviluppatori web erano limitate. Ci sono solo una manciata di font che si può garantire siano disponibili su tutti i sistemi comuni — i cosiddetti [Font sicuri per il web](/it/docs/Learn_web_development/Core/Text_styling/Fundamentals#web_safe_fonts). Si può usare lo stack di font per specificare i font preferiti, seguiti da alternative sicure per il web e poi dal font di sistema predefinito. Tuttavia, questo aumenta il suo carico di lavoro a causa del testing richiesto per assicurarsi che i suoi design funzionino con ciascun font.

## Font web

C'è un'alternativa che funziona bene. Il CSS permette di specificare file di font disponibili sul web, che vengono scaricati insieme al suo sito mentre viene accessibile. Ciò significa che qualsiasi browser che supporta questa funzionalità CSS può visualizzare i font che ha scelto specificamente. Fantastico! La sintassi richiesta sembra qualcosa del genere:

Prima di tutto, ha un set di regole {{cssxref("@font-face")}} all'inizio del CSS, che specifica il/i file di font da scaricare:

```css
@font-face {
  font-family: "myFont";
  src: url("myFont.woff2");
}
```

Sotto questo si utilizza il nome della famiglia di font specificato all'interno di {{cssxref("@font-face")}} per applicare il proprio font personalizzato a qualsiasi cosa si desideri, come di consueto:

```css
html {
  font-family: "myFont", "Bitstream Vera Serif", serif;
}
```

La sintassi diventa un po' più complessa di così. Entreremo nei dettagli più avanti.

Ecco alcune cose importanti da tenere a mente sui font web:

1. I font generalmente non sono gratuiti senza restrizioni. Bisogna pagarli e/o seguire altre condizioni di licenza, come l'accreditamento del creatore del font nel suo codice (o sul suo sito). Non si dovrebbero rubare font e usarli senza dare il giusto credito.
2. Tutti i principali browser supportano WOFF/WOFF2 (versioni 1 e 2 del Web Open Font Format). Anche i browser più vecchi come IE9 (rilasciato nel 2011) supportano il formato WOFF.
3. WOFF2 supporta l'intera specifica TrueType e OpenType, inclusi font variabili, font cromatici e collezioni di font.
4. L'ordine in cui si elencano i file di font è importante. Se si fornisce al browser un elenco di più file di font da scaricare, il browser sceglierà il primo file di font che è in grado di utilizzare. Ecco perché il formato elencato per primo dovrebbe essere il formato preferito — cioè, WOFF2 — con i formati più vecchi elencati dopo questo. I browser che non capiscono un formato passeranno poi al formato successivo nell'elenco.
5. Se ha bisogno di lavorare con browser legacy, dovrebbe fornire font web EOT (Embedded Open Type), TTF (TrueType Font) e SVG per il download. Questo articolo spiega come utilizzare il Fontsquirrel Webfont Generator per generare i file richiesti.

Può usare il [Firefox Font Editor](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/how_to/edit_fonts/index.html) per investigare e manipolare i font in uso sulla sua pagina, che siano font web o meno.

## Apprendimento attivo: Un esempio di font web

Tenendo conto di ciò, costruiamo un esempio di font web di base partendo dai principi fondamentali. È difficile dimostrare questo utilizzando un esempio live incorporato. Quindi, vorremmo che seguisse i passaggi dettagliati nelle sezioni seguenti per avere un'idea del processo.

Dovrebbe usare i file [web-font-start.html](https://github.com/mdn/learning-area/blob/main/css/styling-text/web-fonts/web-font-start.html) e [web-font-start.css](https://github.com/mdn/learning-area/blob/main/css/styling-text/web-fonts/web-font-start.css) come punto di partenza per aggiungere il suo codice (vedere l'[esempio live](https://mdn.github.io/learning-area/css/styling-text/web-fonts/web-font-start.html)). Faccia una copia di questi file in una nuova directory sul suo computer ora. Nel file `web-font-start.css`, troverà del CSS minimo per gestire il layout e la formattazione di base dell'esempio.

### Trovare font

Per questo esempio, useremo due font web: uno per i titoli e uno per il testo del corpo. Per cominciare, dobbiamo trovare i file di font che contengono i font. I font sono creati da fonderie di font e sono memorizzati in diversi formati di file. Ci sono generalmente tre tipi di siti dove si possono ottenere font:

- Un distributore di font gratuiti: Questo è un sito che rende disponibili font gratuiti per il download (potrebbero comunque esserci alcune condizioni di licenza, come l'accreditamento del creatore del font). Esempi includono [Font Squirrel](https://www.fontsquirrel.com/), [DaFont](https://www.dafont.com/), e [Everything Fonts](https://everythingfonts.com/).
- Un distributore di font a pagamento: Questo è un sito che rende disponibili font a pagamento, come [fonts.com](https://www.fonts.com/) o [myfonts.com](https://www.myfonts.com/). Può anche acquistare font direttamente dalle fonderie di font, ad esempio [Linotype](https://www.linotype.com/), [Monotype](https://www.monotype.com/), o [Exljbris](https://www.exljbris.com/).
- Un servizio di font online: Questo è un sito che memorizza e fornisce i font per lei, rendendo l'intero processo più semplice. Vedere la sezione [Utilizzo di un servizio di font online](#utilizzare_un_servizio_di_font_online) per maggiori dettagli.

Troviamo alcuni font! Vada su [Font Squirrel](https://www.fontsquirrel.com/) e scelga due font: un font interessante per i titoli (forse un bel font display o slab serif), e un font leggermente meno appariscente e più leggibile per i paragrafi. Quando ha trovato un font, premi il pulsante di download e salvi il file nella stessa directory dei file HTML e CSS che ha salvato in precedenza. Non importa se sono TTF (True Type Fonts) o OTF (Open Type Fonts).

Decomprimere i due pacchetti di font (I font web sono normalmente distribuiti in file ZIP contenenti il/i file del font e le informazioni sulla licenza). Potrebbe trovare più file di font nel pacchetto — alcuni font sono distribuiti come una famiglia con diverse varianti disponibili, ad esempio sottile, medio, grassetto, corsivo, corsivo sottile, ecc. Per questo esempio, vogliamo solo che si preoccupi di un singolo file di font per ciascuna scelta.

> [!NOTE]
> In Font Squirrel, nella sezione "Find fonts" nella colonna di destra, può cliccare sui diversi tag e classificazioni per filtrare le scelte visualizzate.

### Generare il codice richiesto

Ora dovrà generare il codice richiesto (e i formati dei font). Per ciascun font, segua questi passaggi:

1. Assicurarsi di aver soddisfatto ogni requisito di licenza se desidera utilizzare questo in un progetto commerciale e/o web.
2. Andare al [Webfont Generator](https://www.fontsquirrel.com/tools/webfont-generator) di Fontsquirrel.
3. Caricare i suoi due file di font usando il pulsante _Upload Fonts_.
4. Selezionare il checkbox etichettato "Yes, the fonts I'm uploading are legally eligible for web embedding."
5. Fare clic su _Download your kit_.

Dopo che il generatore ha finito di elaborare, dovrebbe ottenere un file ZIP da scaricare. Salvare nella stessa directory del suo HTML e CSS.

### Implementazione del codice nel suo esempio

A questo punto, decomprima il kit webfont che ha appena generato. All'interno della directory decompressa vedrà alcuni elementi utili:

- Due versioni di ciascun font: i file `.woff`, `.woff2`.
- Un file HTML demo per ciascun font — caricandoli nel suo browser potrà vedere come il font apparirà in diversi contesti di utilizzo.
- Un file `stylesheet.css`, che contiene il codice @font-face generato di cui avrà bisogno.

Per implementare questi font nel suo esempio, segua questi passaggi:

1. Rinomini la directory decompressa in qualcosa di facile e semplice, come `fonts`.
2. Aprire il file `stylesheet.css` e copia i due set di regole `@font-face` nel suo file `web-font-start.css` — deve inserirli all'inizio, prima di qualsiasi suo CSS, poiché i font devono essere importati prima che possa usarli sul suo sito.
3. Ognuna delle funzioni `url()` punta a un file di font che vogliamo importare nel nostro CSS. Dobbiamo assicurarci che i percorsi ai file siano corretti, quindi aggiungere `fonts/` all'inizio di ogni percorso (regolare secondo necessità).
4. Ora può utilizzare questi font nei suoi stack di font, proprio come qualsiasi font sicuro per il web o di sistema predefinito. Per esempio:

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

Dovrebbe ottenere una pagina demo con alcuni bei font implementati. Poiché i diversi font sono creati in dimensioni diverse, potrebbe dover regolare la dimensione, la spaziatura, ecc., per sistemare l'aspetto e il feeling.

![Il design finale dell'esercizio di apprendimento attivo sui font web. La pagina ha due titoli e tre paragrafi. La pagina contiene diversi font e testo di diverse dimensioni.](web-font-example.png)

> [!NOTE]
> Se ha problemi a far funzionare questo, sentiti libero di confrontare la sua versione con i nostri file finiti — vedere [web-font-finished.html](https://github.com/mdn/learning-area/blob/main/css/styling-text/web-fonts/web-font-finished.html) e [web-font-finished.css](https://github.com/mdn/learning-area/blob/main/css/styling-text/web-fonts/web-font-finished.css). Può anche scaricare il [codice da GitHub](https://github.com/mdn/learning-area/tree/main/css/styling-text/web-fonts) o [eseguire l'esempio finito dal vivo](https://mdn.github.io/learning-area/css/styling-text/web-fonts/web-font-finished.html).

## Utilizzare un servizio di font online

I servizi di font online generalmente memorizzano e forniscono font per lei, quindi non deve preoccuparsi di scrivere il codice `@font-face`. Invece, in genere deve solo inserire una semplice riga o due di codice nel suo sito per far funzionare il tutto. Esempi includono [Adobe Fonts](https://fonts.adobe.com/) e [Cloud.typography](https://www.typography.com/webfonts). La maggior parte di questi servizi sono basati su abbonamento, con l'eccezione notevole di [Google Fonts](https://fonts.google.com/), un servizio gratuito utile, specialmente per un lavoro di test rapido e per la scrittura di demo.

La maggior parte di questi servizi sono facili da usare, quindi non li copriremo in dettaglio. Diamo uno sguardo veloce a Google Fonts per farle capire di cosa si tratta. Ancora una volta, utilizzi copie di `web-font-start.html` e `web-font-start.css` come punto di partenza.

1. Vada su [Google Fonts](https://fonts.google.com/).
2. Cerchi i suoi font preferiti o utilizzi i filtri nella parte superiore della pagina per visualizzare i tipi di font che desidera scegliere e selezioni un paio di font che le piacciono.
3. Per selezionare una famiglia di font, faccia clic sull'anteprima del font e premi il pulsante ⊕ accanto al font.
4. Una volta scelte le famiglie di font, premi il pulsante _View your selected families_ nell'angolo in alto a destra della pagina.
5. Nella schermata risultante, prima di tutto deve copiare la riga di codice HTML mostrata e incollarla nell'intestazione del suo file HTML. Mettila sopra l'elemento {{htmlelement("link")}} esistente, in modo che il font sia importato prima di provare a usarlo nel suo CSS.
6. Deve quindi copiare le dichiarazioni CSS elencate nel suo CSS come appropriato, per applicare i font personalizzati al suo HTML.

> [!NOTE]
> Può trovare una versione completata in [google-font.html](https://github.com/mdn/learning-area/blob/main/css/styling-text/web-fonts/google-font.html) e [google-font.css](https://github.com/mdn/learning-area/blob/main/css/styling-text/web-fonts/google-font.css), se ha bisogno di controllare il suo lavoro rispetto al nostro ([vedere dal vivo](https://mdn.github.io/learning-area/css/styling-text/web-fonts/google-font.html)).

## @font-face in maggiore dettaglio

Esploriamo quella sintassi `@font-face` generata per lei da Fontsquirrel. I set di regole appariranno simili a questo:

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

Esaminiamolo per vedere cosa fa:

- `font-family`: Questa riga specifica il nome con cui desidera riferirsi al font. Può essere qualsiasi cosa le piaccia purché lo usi in modo coerente nel suo CSS.
- `src`: Queste righe specificano i percorsi ai file di font da importare nel suo CSS (la parte `url`), e il formato di ciascun file di font (la parte `format`). La parte lattera in ciascun caso è opzionale, ma è utile dichiararla perché consente ai browser di determinare più rapidamente quale font possono usare. Possono essere elencate dichiarazioni multiple, separate da virgole. Poiché il browser le cercherà secondo le regole della cascata, è meglio indicare i suoi formati preferiti, come WOFF2, all'inizio.
- {{cssxref("font-weight")}}/{{cssxref("font-style")}}: Queste righe specificano il peso del font e se è in corsivo o meno. Se sta importando più pesi dello stesso font, può specificare quale sia il loro peso/stile e poi usare valori diversi di {{cssxref("font-weight")}}/{{cssxref("font-style")}} per scegliere tra di essi, piuttosto che dover nominare tutti i diversi membri della famiglia di font con nomi diversi. [@font-face tip: define font-weight and font-style to keep your CSS simple](https://www.456bereastreet.com/archive/201012/font-face_tip_define_font-weight_and_font-style_to_keep_your_css_simple/) di Roger Johansson mostra cosa fare in maggior dettaglio.

> [!NOTE]
> Può anche specificare particolari valori di {{cssxref("font-variant")}} e {{cssxref("font-stretch")}} per i suoi font web. Nei browser più recenti, può anche specificare un valore di {{cssxref("@font-face/unicode-range", "unicode-range")}}, che è un intervallo specifico di caratteri che potrebbe voler utilizzare dal font web. Nei browser che lo supportano, il font sarà scaricato solo se la pagina contiene quei caratteri specificati, risparmiando download non necessari. [Creating Custom Font Stacks with Unicode-Range](https://24ways.org/2011/creating-custom-font-stacks-with-unicode-range/) di Drew McLellan fornisce alcune idee utili su come sfruttare questo.

## Riepilogo

Ora che ha lavorato attraverso i nostri articoli sui fondamenti dello stile del testo, è il momento di valutare la sua comprensione con la nostra sfida per il modulo: Composizione tipografica di una homepage di una scuola comunitaria.

## Vedi anche

- [Guida sui font variabili](/it/docs/Web/CSS/CSS_fonts/Variable_fonts_guide)
- [Conoscenza sui font](https://fonts.google.com/knowledge), Google Fonts

{{PreviousMenuNext("Learn_web_development/Core/Text_styling/Styling_links", "Learn_web_development/Core/Text_styling/Typesetting_a_homepage", "Learn_web_development/Core/Text_styling")}}
