---
title: Strutturare il contenuto con HTML
short-title: HTML
slug: Learn_web_development/Core/Structuring_content
l10n:
  sourceCommit: 6a5c619dfad295ca9a9d317a4088908cfd33e686
---

{{NextMenu("Learn_web_development/Core/Structuring_content/Basic_HTML_syntax", "Learn_web_development/Core")}}

HTML è la tecnologia che definisce il contenuto e la struttura di qualsiasi sito web. Se scritto correttamente, dovrebbe anche definire la semantica (significato) del contenuto in un modo leggibile dalle macchine, il che è fondamentale per l'accessibilità, l'ottimizzazione per i motori di ricerca e per sfruttare le funzionalità integrate che i browser offrono affinché il contenuto funzioni in modo ottimale. Questo modulo copre le basi del linguaggio, prima di analizzare aree chiave come la struttura del documento, i link, le liste, le immagini, i moduli e altro ancora.

## Prerequisiti

Prima di iniziare questo modulo, non è necessaria alcuna conoscenza precedente di HTML, ma dovrebbe almeno avere una familiarità di base con l'utilizzo dei computer e l'utilizzo passivo del web (ossia, semplicemente osservare e consumare contenuti). Dovrebbe avere un ambiente di lavoro di base impostato (come dettagliato in [Installare software di base](/it/docs/Learn_web_development/Getting_started/Environment_setup/Installing_software)), e comprendere come creare e gestire file (come dettagliato in [Gestire i file](/it/docs/Learn_web_development/Getting_started/Environment_setup/Dealing_with_files)). Entrambi sono parti del nostro modulo completo per principianti [Introduzione al web](/it/docs/Learn_web_development/Getting_started/Your_first_website).

> [!NOTE]
> Se lavora su un computer/tablet/altro dispositivo dove non ha la possibilità di creare i propri file, potrebbe provare (la maggior parte) degli esempi di codice in un programma di codifica online come [JSBin](https://jsbin.com/) o [Glitch](https://glitch.com/).

## Tutorial e sfide

- [Sintassi di base di HTML](/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax)
  - : Copre le basi assolute di {{Glossary("HTML", "HTML")}}, per iniziare — definiamo elementi, attributi e altri termini importanti, e mostriamo dove si inseriscono nel linguaggio. Mostriamo anche come è strutturata una tipica pagina HTML e come è strutturato un elemento HTML, e spieghiamo altre importanti funzionalità di base del linguaggio. Lungo il cammino, giocheremo con un po' di HTML per suscitare il suo interesse!
- [Cosa c'è nel head? Metadati della pagina web](/it/docs/Learn_web_development/Core/Structuring_content/Webpage_metadata)
  - : La {{Glossary("Head", "head")}} di un documento HTML è la parte che **non** viene visualizzata nel browser quando la pagina viene caricata. Contiene informazioni sui metadati come il {{htmlelement("title")}} della pagina, i link a {{Glossary("CSS", "CSS")}} (se si desidera stilizzare il contenuto HTML con CSS), link a favicon personalizzate e metadati (dati sull'HTML, come chi l'ha scritto, e parole chiave importanti che descrivono il documento).
- [Intestazioni e paragrafi](/it/docs/Learn_web_development/Core/Structuring_content/Headings_and_paragraphs)
  - : Uno dei compiti principali dell'HTML è dare struttura al testo affinché un browser possa visualizzare un documento HTML come inteso dal suo sviluppatore. Questo articolo spiega come l'HTML possa essere utilizzato per fornire la struttura fondamentale della pagina definendo intestazioni e paragrafi.
- [Enfasi e importanza](/it/docs/Learn_web_development/Core/Structuring_content/Emphasis_and_importance)
  - : L'articolo precedente ha esaminato il motivo per cui la semantica è importante in HTML, concentrandosi su intestazioni e paragrafi. Questo articolo continua sul tema della semantica, esaminando gli elementi HTML che applicano enfasi e importanza al testo (parallelamente al testo in corsivo e grassetto nei media stampati).
- [Liste](/it/docs/Learn_web_development/Core/Structuring_content/Lists)
  - : Le liste sono ovunque nella vita — dalla lista della spesa alla lista delle indicazioni che si seguono inconsciamente per arrivare a casa ogni giorno, alle liste di istruzioni che sta seguendo in questi tutorial! Non sorprende che l'HTML abbia un insieme comodo di elementi che ci permettono di definire diversi tipi di lista. Sul web, abbiamo tre tipi di liste: non ordinate, ordinate e liste descrittive. Questa lezione mostra come utilizzare i diversi tipi.
- [Strutturare documenti](/it/docs/Learn_web_development/Core/Structuring_content/Structuring_documents)
  - : Oltre a definire parti individuali della sua pagina (come "un paragrafo" o "un'immagine"), HTML dispone anche di un certo numero di elementi di livello blocco utilizzati per definire aree del suo sito web (come "l'intestazione", "il menu di navigazione", "la colonna del contenuto principale"). Questo articolo esamina come pianificare una struttura di base del sito web e scrivere il codice HTML per rappresentare questa struttura.
- [Funzionalità di testo avanzate](/it/docs/Learn_web_development/Core/Structuring_content/Advanced_text_features)
  - : Ci sono molti altri elementi in HTML per definire la semantica del testo, che non abbiamo affrontato nell'articolo [Enfasi e importanza](/it/docs/Learn_web_development/Core/Structuring_content/Emphasis_and_importance). Gli elementi descritti in questo articolo sono meno conosciuti, ma ancora utili da conoscere (e questa non è ancora una lista completa in ogni caso). Qui imparerà a marcare citazioni, codice del computer e testo correlato, testo in apice e pedice, informazioni di contatto e altro ancora.
- [Creare link](/it/docs/Learn_web_development/Core/Structuring_content/Creating_links)
  - : I link (noti anche come hyperlink) sono davvero importanti — sono ciò che rende il Web _una rete_. Questo articolo mostra la sintassi richiesta per creare un link e discute le migliori pratiche per i link.
- [Marcare una lettera](/it/docs/Learn_web_development/Core/Structuring_content/Marking_up_a_letter) <sup>Sfida</sup>
  - : Prima o poi tutti impariamo a scrivere una lettera; è anche un esempio utile per testare le nostre abilità di formattazione del testo. In questa sfida, avrà una lettera da marcare come test per le sue abilità di formattazione del testo in HTML, nonché per i hyperlink e l'uso corretto dell'elemento `<head>` in HTML.
- [Strutturare una pagina di contenuto](/it/docs/Learn_web_development/Core/Structuring_content/Structuring_a_page_of_content) <sup>Sfida</sup>
  - : Strutturare una pagina di contenuto pronta per essere disposta utilizzando CSS è una competenza molto importante da padroneggiare, quindi in questa sfida verrà testata sulla sua capacità di pensare a come potrebbe finire per apparire una pagina e scegliere la semantica strutturale appropriata per costruire un layout sopra di essa.
- [Immagini HTML](/it/docs/Learn_web_development/Core/Structuring_content/HTML_images)
  - : Inizialmente, il web era solo testo ed era davvero piuttosto noioso. Fortunatamente, non passò molto tempo prima che la possibilità di incorporare immagini (e altri tipi di contenuto più interessanti) all'interno delle pagine web fosse aggiunta. In questo articolo esamineremo come utilizzare l'elemento {{htmlelement("img")}} in dettaglio, comprese le basi, annotarlo con didascalie utilizzando {{htmlelement("figure")}} e dettagliare come si relaziona con le immagini di sfondo del {{Glossary("CSS", "CSS")}}.
- [Video e audio in HTML](/it/docs/Learn_web_development/Core/Structuring_content/HTML_video_and_audio)
  - : Ora che siamo a nostro agio con l'aggiunta di immagini semplici a una pagina web, il passo successivo è iniziare ad aggiungere lettori video e audio ai suoi documenti HTML! In questo articolo esamineremo proprio questo con gli elementi {{htmlelement("video")}} e {{htmlelement("audio")}}; finiremo poi esaminando come aggiungere sottotitoli ai suoi video.
- [Pagina di benvenuto Mozilla](/it/docs/Learn_web_development/Core/Structuring_content/Mozilla_splash_page) <sup>Sfida</sup>
  - : In questa sfida, testeremo le sue conoscenze di alcune delle tecniche discusse nelle ultime lezioni, facendole aggiungere alcune immagini e video a una pagina di benvenuto funky tutta su Mozilla!
- [Elementi di base delle tabelle HTML](/it/docs/Learn_web_development/Core/Structuring_content/HTML_table_basics)
  - : Questo articolo la introduce alle tabelle HTML, coprendo le basi come righe, celle, intestazioni, rendendo le celle in grado di estendersi su più colonne e righe e come raggruppare tutte le celle di una colonna per scopi di stilizzazione.
- [Accessibilità delle tabelle HTML](/it/docs/Learn_web_development/Core/Structuring_content/Table_accessibility)
  - : In questo articolo esaminiamo ulteriori funzionalità di accessibilità delle tabelle HTML come didascalie/riassunti, raggruppando le righe in sezioni di testa, corpo e piè di pagina e stabilendo l'ambito di colonne e righe.
- [Strutturare una tabella di dati sui pianeti](/it/docs/Learn_web_development/Core/Structuring_content/Planet_data_table) <sup>Sfida</sup>
  - : In questa sfida, le forniamo alcuni dati sui pianeti nel nostro sistema solare. Il suo compito è strutturarli in una tabella HTML accessibile.
- [Forms e bottoni in HTML](/it/docs/Learn_web_development/Core/Structuring_content/HTML_forms)
  - : I forms e i bottoni HTML sono strumenti potenti per interagire con gli utenti — più comunemente vengono utilizzati per raccogliere dati dagli utenti o per permettere loro di controllare un'interfaccia utente. In questo articolo le forniamo un'introduzione alle basi di forms e bottoni.
- [Debugging HTML](/it/docs/Learn_web_development/Core/Structuring_content/Debugging_HTML)
  - : Scrivere HTML va bene, ma cosa succede se qualcosa va storto e non riesce a scoprire dove si trova l'errore nel codice? Questo articolo le presenterà alcuni strumenti che possono aiutarla a trovare e correggere errori in HTML.
- [Mettere alla prova le sue abilità: HTML](/it/docs/Learn_web_development/Core/Structuring_content/Test_your_skills)
  - : Questa pagina elenca i test HTML che può provare per verificare se ha compreso il contenuto di questo modulo.

## Tutorial aggiuntivi

Questi tutorial non fanno parte del percorso di apprendimento, ma sono comunque interessanti — dovrebbe considerarli come obiettivi estesi, da studiare facoltativamente quando avrà terminato gli articoli del percorso principale.

- [Inclusione di grafica vettoriale in HTML](/it/docs/Learn_web_development/Core/Structuring_content/Including_vector_graphics_in_HTML)
  - : La grafica vettoriale è molto utile in molte circostanze — ha dimensioni di file ridotte ed è altamente scalabile, quindi non si pixelizza quando viene ingrandita o ingrandita a una dimensione maggiore. In questo articolo le mostreremo come includerne una nella sua pagina web.
- [Da object a iframe — tecnologie di incorporamento generali](/it/docs/Learn_web_development/Core/Structuring_content/General_embedding_technologies)
  - : Gli sviluppatori comunemente pensano di incorporare contenuti multimediali come immagini, video e audio nelle pagine web. In questo articolo faremo un passo laterale, esaminando alcuni elementi che le consentono di incorporare una vasta gamma di tipi di contenuto nelle sue pagine web: gli elementi {{htmlelement("iframe")}}, {{htmlelement("embed")}} e {{htmlelement("object")}}. Gli `<iframe>` sono per incorporare altre pagine web, e gli altri due permettono di incorporare risorse esterne come file PDF.

## Vedi anche

- [Imparare HTML e CSS](https://scrimba.com/learn-html-and-css-c0p?via=mdn), Scrimba <sup>[_Partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>
  - : Il corso _Imparare HTML e CSS_ di [Scrimba](https://scrimba.com?via=mdn) le insegna HTML e CSS attraverso la costruzione e il deployment di cinque progetti fantastici, con lezioni interattive divertenti e sfide insegnate da insegnanti esperti.
- [Imparare HTML](https://www.codecademy.com/learn/learn-html), Codecademy
  - : Un'altra risorsa utile per apprendere le basi di HTML.
- [I fondamenti dell'HTML semantico](https://scrimba.com/the-frontend-developer-career-path-c0j/~0xid?via=mdn), Scrimba <sup>[_Partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>
  - : Questa lezione interattiva fornisce una descrizione utile dell'HTML, con particolare enfasi sul perché l'aspetto _semantico_ è importante.

{{NextMenu("Learn_web_development/Core/Structuring_content/Basic_HTML_syntax", "Learn_web_development/Core")}}
