---
title: Strutturare il contenuto con HTML
short-title: HTML
slug: Learn_web_development/Core/Structuring_content
l10n:
  sourceCommit: 6a5c619dfad295ca9a9d317a4088908cfd33e686
---

{{NextMenu("Learn_web_development/Core/Structuring_content/Basic_HTML_syntax", "Learn_web_development/Core")}}

HTML è la tecnologia che definisce il contenuto e la struttura di qualsiasi sito web. Se scritto correttamente, dovrebbe anche definire la semantica (significato) del contenuto in modo leggibile dalla macchina, il che è fondamentale per l'accessibilità, l'ottimizzazione nei motori di ricerca e per sfruttare le funzionalità integrate che i browser offrono affinché il contenuto funzioni in maniera ottimale. Questo modulo copre le basi del linguaggio, prima di approfondire aree chiave come la struttura del documento, i link, le liste, le immagini, i form e altro ancora.

## Prerequisiti

Prima di iniziare questo modulo, non è necessaria alcuna conoscenza pregressa di HTML, ma dovresti avere almeno una familiarità di base con l'uso dei computer e l'uso passivo del web (cioè, semplicemente consultarlo e consumare contenuti). Dovresti avere un ambiente di lavoro di base impostato (come descritto in [Installazione del software di base](/it/docs/Learn_web_development/Getting_started/Environment_setup/Installing_software)), e capire come creare e gestire file (come dettagliato in [Gestione dei file](/it/docs/Learn_web_development/Getting_started/Environment_setup/Dealing_with_files)). Entrambi fanno parte del nostro modulo completo per principianti [Primi passi sul web](/it/docs/Learn_web_development/Getting_started/Your_first_website).

> [!NOTE]
> Se stai lavorando su un computer/tablet/altri dispositivi dove non hai la possibilità di creare i tuoi file, potresti provare (la maggior parte) degli esempi di codice in un programma di codifica online come [JSBin](https://jsbin.com/) o [Glitch](https://glitch.com/).

## Tutorial e sfide

- [Sintassi base di HTML](/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax)
  - : Copre le basi assolute di {{Glossary("HTML", "HTML")}}, per iniziare: definiamo elementi, attributi e altri termini importanti e mostriamo dove si inseriscono nel linguaggio. Mostriamo anche come è strutturata una tipica pagina HTML e come è strutturato un elemento HTML, e spieghiamo altre caratteristiche di base del linguaggio importanti. Lungo il percorso, giocheremo con qualche HTML per suscitare il tuo interesse!
- [Cosa c'è nell'head? Metadati della pagina web](/it/docs/Learn_web_development/Core/Structuring_content/Webpage_metadata)
  - : L'{{Glossary("Head", "head")}} di un documento HTML è la parte che **non è** visualizzata nel browser web quando la pagina viene caricata. Contiene informazioni di metadati come il {{htmlelement("title")}} della pagina, link a {{Glossary("CSS", "CSS")}} (se desideri stilizzare il tuo contenuto HTML con CSS), link a favicon personalizzate e metadati (dati sull'HTML, come chi lo ha scritto e le parole chiave importanti che descrivono il documento).
- [Titoli e paragrafi](/it/docs/Learn_web_development/Core/Structuring_content/Headings_and_paragraphs)
  - : Uno dei principali compiti di HTML è dare struttura al testo in modo che un browser possa visualizzare un documento HTML come l'ha inteso il suo sviluppatore. Questo articolo spiega come HTML può essere usato per fornire la struttura fondamentale della pagina definendo titoli e paragrafi.
- [Enfasi e importanza](/it/docs/Learn_web_development/Core/Structuring_content/Emphasis_and_importance)
  - : L'articolo precedente ha esaminato perché le semantiche sono importanti in HTML, concentrandosi su titoli e paragrafi. Questo articolo continua il tema delle semantiche, analizzando gli elementi HTML che applicano enfasi e importanza al testo (in parallelo con il testo in corsivo e grassetto nei media stampati).
- [Liste](/it/docs/Learn_web_development/Core/Structuring_content/Lists)
  - : Le liste sono ovunque nella vita—dalla lista della spesa alla lista delle direzioni che segui inconsciamente per arrivare a casa ogni giorno, alle liste di istruzioni che stai seguendo in questi tutorial! Non dovrebbe sorprendere che HTML abbia un insieme conveniente di elementi che permette di definire diversi tipi di lista. Sul web, abbiamo tre tipi di liste: non ordinate, ordinate e di descrizione. Questa lezione mostra come usare i diversi tipi.
- [Strutturare documenti](/it/docs/Learn_web_development/Core/Structuring_content/Structuring_documents)
  - : Oltre a definire parti individuali della tua pagina (come "un paragrafo" o "un'immagine"), HTML vanta anche un certo numero di elementi di livello blocco usati per definire aree del tuo sito web (come "l'intestazione", "il menu di navigazione", "la colonna del contenuto principale"). Questo articolo esamina come pianificare una struttura di base del sito web e scrivere l'HTML per rappresentare questa struttura.
- [Funzionalità avanzate di testo](/it/docs/Learn_web_development/Core/Structuring_content/Advanced_text_features)
  - : Ci sono molti altri elementi in HTML per definire semantiche del testo che non abbiamo affrontato nell'articolo [Enfasi e importanza](/it/docs/Learn_web_development/Core/Structuring_content/Emphasis_and_importance). Gli elementi descritti in questo articolo sono meno conosciuti, ma comunque utili da conoscere (e questo non è nemmeno un elenco completo). Qui imparerai a marcare citazioni, codice informatico e altri testi correlati, pedice e apice, informazioni di contatto e altro.
- [Creare link](/it/docs/Learn_web_development/Core/Structuring_content/Creating_links)
  - : I link (noti anche come hyperlink) sono davvero importanti — sono ciò che rende il Web _una rete_. Questo articolo mostra la sintassi necessaria per creare un link e discute le migliori pratiche dei link.
- [Marcare una lettera](/it/docs/Learn_web_development/Core/Structuring_content/Marking_up_a_letter) <sup>Challenge</sup>
  - : Impariamo tutti a scrivere una lettera prima o poi; è anche un utile esempio per testare le nostre abilità di formattazione del testo. In questa sfida, avrai una lettera da marcare come test per le tue capacità di formattazione del testo HTML, nonché di hyperlink e uso corretto dell'elemento `<head>` HTML.
- [Strutturare una pagina di contenuto](/it/docs/Learn_web_development/Core/Structuring_content/Structuring_a_page_of_content) <sup>Challenge</sup>
  - : Strutturare una pagina di contenuto pronta per essere organizzata usando CSS è una competenza molto importante da padroneggiare, quindi in questa sfida sarai testato sulla tua capacità di pensare a come potrebbe apparire una pagina e scegliere semanticamente strutturali appropriate su cui costruire un layout.
- [Immagini HTML](/it/docs/Learn_web_development/Core/Structuring_content/HTML_images)
  - : All'inizio, il web era solo testo, ed era davvero piuttosto noioso. Fortunatamente, non ci volle molto perché la capacità di incorporare immagini (e altri tipi di contenuto più interessanti) all'interno delle pagine web fosse aggiunta. In questo articolo vedremo come usare l'elemento {{htmlelement("img")}} in dettaglio, inclusi i fondamenti, annotarlo con didascalie usando {{htmlelement("figure")}}, e dettagliare come si relaziona con le immagini di sfondo {{Glossary("CSS", "CSS")}}.
- [Video e audio HTML](/it/docs/Learn_web_development/Core/Structuring_content/HTML_video_and_audio)
  - : Ora che siamo a nostro agio con l'aggiunta di semplici immagini a una pagina web, il passo successivo è iniziare ad aggiungere lettori video e audio ai tuoi documenti HTML! In questo articolo vedremo come fare proprio questo con gli elementi {{htmlelement("video")}} e {{htmlelement("audio")}}; poi finiremo esaminando come aggiungere sottotitoli ai tuoi video.
- [Pagina introduttiva di Mozilla](/it/docs/Learn_web_development/Core/Structuring_content/Mozilla_splash_page) <sup>Challenge</sup>
  - : In questa sfida, testeremo la tua conoscenza di alcune delle tecniche discusse negli ultimi articoli, chiedendoti di aggiungere immagini e video a una pagina introduttiva su Mozilla!
- [Basi delle tabelle HTML](/it/docs/Learn_web_development/Core/Structuring_content/HTML_table_basics)
  - : Questo articolo ti avvia alle tabelle HTML, coprendo i fondamenti come righe, celle, intestazioni, come fare in modo che le celle abbraccino più colonne e righe, e come raggruppare tutte le celle in una colonna per scopi di styling.
- [Accessibilità delle tabelle HTML](/it/docs/Learn_web_development/Core/Structuring_content/Table_accessibility)
  - : In questo articolo esaminiamo ulteriori caratteristiche di accessibilità delle tabelle HTML come didascalie/sommari, raggruppamento delle righe nelle sezioni di intestazione, corpo e piè di pagina della tabella, e scoping di colonne e righe.
- [Strutturare una tabella di dati planetari](/it/docs/Learn_web_development/Core/Structuring_content/Planet_data_table) <sup>Challenge</sup>
  - : In questa sfida, ti forniamo alcuni dati sui pianeti del nostro sistema solare. Il tuo compito è strutturarli in una tabella HTML accessibile.
- [Form e pulsanti in HTML](/it/docs/Learn_web_development/Core/Structuring_content/HTML_forms)
  - : I form e i pulsanti HTML sono strumenti potenti per interagire con gli utenti — il più delle volte, vengono utilizzati per raccogliere dati dagli utenti o permettere loro di controllare un'interfaccia utente. In questo articolo forniamo un'introduzione ai fondamenti dei form e dei pulsanti.
- [Debuggare HTML](/it/docs/Learn_web_development/Core/Structuring_content/Debugging_HTML)
  - : Scrivere HTML va bene, ma cosa succede se qualcosa va storto e non riesci a capire dove sia l'errore nel codice? Questo articolo ti introdurrà a alcuni strumenti che possono aiutarti a trovare e correggere errori in HTML.
- [Testa le tue capacità: HTML](/it/docs/Learn_web_development/Core/Structuring_content/Test_your_skills)
  - : Questa pagina elenca i test HTML che puoi provare per verificare se hai compreso il contenuto di questo modulo.

## Tutorial aggiuntivi

Questi tutorial non fanno parte del percorso di apprendimento, ma sono comunque interessanti — dovresti considerarli come obiettivi di approfondimento, da studiare facoltativamente quando hai terminato gli articoli principali del Core.

- [Includere grafica vettoriale in HTML](/it/docs/Learn_web_development/Core/Structuring_content/Including_vector_graphics_in_HTML)
  - : La grafica vettoriale è molto utile in molte circostanze — hanno dimensioni di file ridotte e sono altamente scalabili, quindi non si pixelano quando ingranditi o ampliati a grandi dimensioni. In questo articolo ti mostreremo come includerne uno nella tua pagina web.
- [Da oggetto a iframe — tecnologie di incorporamento generali](/it/docs/Learn_web_development/Core/Structuring_content/General_embedding_technologies)
  - : Gli sviluppatori comunemente pensano di incorporare media come immagini, video e audio nelle pagine web. In questo articolo facciamo un passo laterale, esaminando alcuni elementi che ti permettono di incorporare una vasta gamma di tipi di contenuto nelle tue pagine web: gli elementi {{htmlelement("iframe")}}, {{htmlelement("embed")}} e {{htmlelement("object")}}. `<iframe>` serve per incorporare altre pagine web, e gli altri due permettono di incorporare risorse esterne come file PDF.

## Vedi anche

- [Impara HTML e CSS](https://scrimba.com/learn-html-and-css-c0p?via=mdn), Scrimba <sup>[_MDN learning partner_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>
  - : Il corso _Impara HTML e CSS_ di [Scrimba](https://scrimba.com?via=mdn) ti insegna HTML e CSS costruendo e distribuendo cinque progetti interessanti, con lezioni e sfide interattive e divertenti insegnate da insegnanti esperti.
- [Impara HTML](https://www.codecademy.com/learn/learn-html), Codecademy
  - : Un'altra risorsa utile per apprendere le basi di HTML.
- [Le basi di HTML semantico](https://scrimba.com/the-frontend-developer-career-path-c0j/~0xid?via=mdn), Scrimba <sup>[_MDN learning partner_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>
  - : Questa lezione interattiva fornisce una descrizione utile di HTML, con particolare enfasi sul perché l'aspetto _semantico_ di esso sia importante.

{{NextMenu("Learn_web_development/Core/Structuring_content/Basic_HTML_syntax", "Learn_web_development/Core")}}
