---
title: Strutturare i contenuti con HTML
short-title: HTML
slug: Learn_web_development/Core/Structuring_content
l10n:
  sourceCommit: 25a3f6c781777a135143b0edd4b5e1f85857b802
---

{{NextMenu("Learn_web_development/Core/Structuring_content/Basic_HTML_syntax", "Learn_web_development/Core")}}

HTML è la tecnologia che definisce il contenuto e la struttura di qualsiasi sito web. Se scritto correttamente, dovrebbe anche definire la semantica (il significato) del contenuto in modo leggibile dalle macchine, aspetto fondamentale per l'accessibilità, l'ottimizzazione per i motori di ricerca e l'uso delle funzionalità integrate che i browser offrono affinché i contenuti funzionino in modo ottimale. Questo modulo tratta le basi del linguaggio, prima di esaminare aree chiave quali struttura del documento, collegamenti, elenchi, immagini, moduli e altro ancora.

## Prerequisiti

Prima di iniziare questo modulo, non è necessaria alcuna conoscenza precedente di HTML, ma è consigliabile avere almeno una familiarità di base con l'uso dei computer e con l'uso passivo del web (ovvero, semplicemente consultarlo e fruirne i contenuti). Dovrebbe essere configurato un ambiente di lavoro di base (come descritto in [Installazione del software di base](/it/docs/Learn_web_development/Getting_started/Environment_setup/Installing_software)) e si dovrebbe comprendere come creare e gestire file (come descritto in [Gestione dei file](/it/docs/Learn_web_development/Getting_started/Environment_setup/Dealing_with_files)). Entrambi fanno parte del nostro modulo completo per principianti [Introduzione al web](/it/docs/Learn_web_development/Getting_started/Your_first_website).

> [!NOTE]
> Se si lavora su un computer, tablet o altro dispositivo sul quale non è possibile creare file, è possibile provare a eseguire il codice in un editor online come [CodePen](https://codepen.io/) o [JSFiddle](https://jsfiddle.net/).

## Tutorial e sfide

- [Sintassi HTML di base](/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax)
  - : Tratta le basi assolute di {{Glossary("HTML", "HTML")}}, per iniziare: definisce elementi, attributi e altri termini importanti, e mostra dove si collocano nel linguaggio. Mostra inoltre come è strutturata una tipica pagina HTML e come è strutturato un elemento HTML, spiegando anche altre importanti funzionalità di base del linguaggio. Lungo il percorso, sarà possibile sperimentare con un po' di HTML per stimolare l'interesse!
- [Cosa contiene l'head? Metadati della pagina web](/it/docs/Learn_web_development/Core/Structuring_content/Webpage_metadata)
  - : L'{{Glossary("Head", "head")}} di un documento HTML è la parte che **non** viene visualizzata nel browser web quando la pagina viene caricata. Contiene informazioni sui metadati, come l'elemento {{htmlelement("title")}} della pagina, collegamenti a {{Glossary("CSS", "CSS")}} (se si desidera applicare stili ai contenuti HTML con CSS), collegamenti a favicon personalizzate e metadati (dati sull'HTML, ad esempio chi lo ha scritto e parole chiave importanti che descrivono il documento).
- [Intestazioni e paragrafi](/it/docs/Learn_web_development/Core/Structuring_content/Headings_and_paragraphs)
  - : Uno dei compiti principali di HTML è fornire una struttura al testo affinché un browser possa visualizzare un documento HTML nel modo previsto dal suo sviluppatore. Questo articolo spiega come HTML può essere usato per fornire una struttura fondamentale alla pagina definendo intestazioni e paragrafi.
- [Enfasi e importanza](/it/docs/Learn_web_development/Core/Structuring_content/Emphasis_and_importance)
  - : L'articolo precedente ha esaminato perché la semantica è importante in HTML, concentrandosi su intestazioni e paragrafi. Questo articolo prosegue il tema della semantica, esaminando gli elementi HTML che applicano enfasi e importanza al testo (in parallelo al corsivo e al grassetto nei supporti di stampa).
- [Elenchi](/it/docs/Learn_web_development/Core/Structuring_content/Lists)
  - : Gli elenchi sono ovunque nella vita: dalla lista della spesa all'elenco di indicazioni che si seguono inconsciamente per arrivare a casa ogni giorno, fino alle liste di istruzioni seguite in questi tutorial. Non sorprende che HTML disponga di un comodo insieme di elementi che consente di definire diversi tipi di elenco. Sul web esistono tre tipi di elenchi: non ordinati, ordinati e descrittivi. Questa lezione mostra come usare i diversi tipi.
- [Funzionalità avanzate del testo](/it/docs/Learn_web_development/Core/Structuring_content/Advanced_text_features)
  - : In HTML esistono molti altri elementi per definire la semantica del testo, che non sono stati trattati nell'articolo [Enfasi e importanza](/it/docs/Learn_web_development/Core/Structuring_content/Emphasis_and_importance). Gli elementi descritti in questo articolo sono meno noti, ma comunque utili da conoscere (e questo non è affatto un elenco completo). Qui si apprenderà come effettuare il markup di citazioni, codice informatico e altro testo correlato, apici, pedici, informazioni di contatto e altro ancora.

- [Effettuare il markup di una lettera](/it/docs/Learn_web_development/Core/Structuring_content/Marking_up_a_letter) <sup>Sfida</sup>
  - : Prima o poi tutti imparano a scrivere una lettera; è anche un esempio utile per mettere alla prova le capacità di formattazione del testo. In questa sfida, viene fornita una lettera su cui effettuare il markup per testare le capacità di formattazione del testo HTML, nonché i collegamenti ipertestuali e l'uso corretto dell'elemento HTML `<head>`.

- [Strutturare documenti](/it/docs/Learn_web_development/Core/Structuring_content/Structuring_documents)
  - : Oltre a definire singole parti della pagina (come "un paragrafo" o "un'immagine"), HTML offre anche diversi elementi a livello di blocco usati per definire aree del sito web (come "l'intestazione", "il menu di navigazione", "la colonna del contenuto principale"). Questo articolo esamina come pianificare una struttura di base per un sito web e scrivere l'HTML per rappresentarla.

- [Creare collegamenti](/it/docs/Learn_web_development/Core/Structuring_content/Creating_links)
  - : I collegamenti (noti anche come collegamenti ipertestuali) sono davvero importanti: sono ciò che rende il Web _una rete_. Questo articolo mostra la sintassi necessaria per creare un collegamento e tratta le buone pratiche relative ai collegamenti.

- [Strutturare una pagina di contenuti](/it/docs/Learn_web_development/Core/Structuring_content/Structuring_a_page_of_content) <sup>Sfida</sup>
  - : Strutturare una pagina di contenuti pronta per definirne il layout con CSS è una capacità molto importante da padroneggiare; in questa sfida verrà quindi verificata la capacità di ragionare sull'aspetto finale di una pagina e di scegliere la semantica strutturale appropriata su cui costruire un layout.
- [Immagini HTML](/it/docs/Learn_web_development/Core/Structuring_content/HTML_images)
  - : All'inizio, il web era composto solo da testo ed era piuttosto noioso. Fortunatamente, non passò molto tempo prima che venisse aggiunta la possibilità di incorporare immagini (e altri tipi di contenuto più interessanti) nelle pagine web. In questo articolo verrà esaminato in dettaglio come usare l'elemento {{htmlelement("img")}}, comprese le basi, l'aggiunta di didascalie mediante {{htmlelement("figure")}} e la relazione con le immagini di sfondo {{Glossary("CSS", "CSS")}}.
- [Video e audio HTML](/it/docs/Learn_web_development/Core/Structuring_content/HTML_video_and_audio)
  - : Ora che è chiaro come aggiungere immagini semplici a una pagina web, il passaggio successivo è iniziare ad aggiungere lettori video e audio ai documenti HTML. In questo articolo verrà esaminato proprio questo, usando gli elementi {{htmlelement("video")}} e {{htmlelement("audio")}}; infine verrà illustrato come aggiungere didascalie/sottotitoli ai video.
- [Pagina iniziale di insetti e altri animaletti](/it/docs/Learn_web_development/Core/Structuring_content/Splash_page) <sup>Sfida</sup>
  - : In questa sfida verranno testate le conoscenze di alcune delle tecniche trattate nelle ultime lezioni, aggiungendo immagini e video a una pagina iniziale dedicata agli insetti e ad altri animaletti.
- [Basi delle tabelle HTML](/it/docs/Learn_web_development/Core/Structuring_content/HTML_table_basics)
  - : Questo articolo introduce alle tabelle HTML, trattando le basi fondamentali come righe, celle, intestazioni, celle che si estendono su più colonne e righe e come raggruppare tutte le celle di una colonna a fini di stile.
- [Accessibilità delle tabelle HTML](/it/docs/Learn_web_development/Core/Structuring_content/Table_accessibility)
  - : In questo articolo vengono esaminate ulteriori funzionalità di accessibilità delle tabelle HTML, come didascalie/riepiloghi, il raggruppamento delle righe nelle sezioni di intestazione, corpo e piè di pagina della tabella, e la definizione dell'ambito di colonne e righe.
- [Strutturare una tabella di dati sui pianeti](/it/docs/Learn_web_development/Core/Structuring_content/Planet_data_table) <sup>Sfida</sup>
  - : In questa sfida vengono forniti alcuni dati sui pianeti del sistema solare. Il compito è strutturarli in una tabella HTML accessibile.
- [Moduli e pulsanti in HTML](/it/docs/Learn_web_development/Core/Structuring_content/HTML_forms)
  - : I moduli e i pulsanti HTML sono strumenti potenti per interagire con gli utenti; vengono usati più comunemente per raccogliere dati dagli utenti o per consentire loro di controllare un'interfaccia utente. In questo articolo viene fornita un'introduzione alle basi dei moduli e dei pulsanti.
- [Debug di HTML](/it/docs/Learn_web_development/Core/Structuring_content/Debugging_HTML)
  - : Scrivere HTML va bene, ma cosa succede se qualcosa non funziona e non si riesce a capire dove si trova l'errore nel codice? Questo articolo introdurrà alcuni strumenti che possono aiutare a individuare e correggere errori nell'HTML.

## Metti alla prova le tue capacità

Gli articoli "Metti alla prova le tue capacità" sono collocati tra gli articoli dei tutorial per verificare se le informazioni più importanti sono state assimilate prima di proseguire. Per esplorarli tutti insieme, sono elencati in [Metti alla prova le tue capacità: HTML](/it/docs/Learn_web_development/Core/Structuring_content/Test_your_skills).

## Tutorial aggiuntivi

Questi tutorial non fanno parte del percorso di apprendimento, ma sono comunque interessanti: possono essere considerati obiettivi più avanzati, da studiare facoltativamente una volta completati gli articoli principali del Core.

- [Includere grafica vettoriale in HTML](/it/docs/Learn_web_development/Core/Structuring_content/Including_vector_graphics_in_HTML)
  - : La grafica vettoriale è molto utile in molte circostanze: ha file di piccole dimensioni ed è altamente scalabile, quindi non si pixella quando viene ingrandita o portata a grandi dimensioni. In questo articolo verrà mostrato come includerla in una pagina web.
- [Da object a iframe — tecnologie generali di incorporamento](/it/docs/Learn_web_development/Core/Structuring_content/General_embedding_technologies)
  - : Gli sviluppatori pensano comunemente all'incorporamento di contenuti multimediali come immagini, video e audio nelle pagine web. In questo articolo si fa un passo laterale, esaminando alcuni elementi che consentono di incorporare un'ampia varietà di tipi di contenuto nelle pagine web: gli elementi {{htmlelement("iframe")}}, {{htmlelement("embed")}} e {{htmlelement("object")}}. Gli `<iframe>` servono per incorporare altre pagine web, mentre gli altri due consentono di incorporare risorse esterne come file PDF.

## Vedi anche

- [Impara HTML e CSS](https://scrimba.com/learn-html-and-css-c0p?via=mdn), Scrimba <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>
  - : Il corso _Learn HTML and CSS_ di [Scrimba](https://scrimba.com?via=mdn) insegna HTML e CSS attraverso la creazione e la pubblicazione di cinque fantastici progetti, con lezioni interattive e divertenti sfide condotte da insegnanti esperti.
- [Impara HTML](https://www.codecademy.com/learn/learn-html), Codecademy
  - : Un'altra risorsa utile per imparare le basi di HTML.
- [Le basi dell'HTML semantico](https://scrimba.com/the-frontend-developer-career-path-c0j/~0xid?via=mdn), Scrimba <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>
  - : Questa lezione interattiva fornisce una descrizione utile di HTML, con particolare enfasi sul motivo per cui il suo aspetto _semantico_ è importante.

{{NextMenu("Learn_web_development/Core/Structuring_content/Basic_HTML_syntax", "Learn_web_development/Core")}}
