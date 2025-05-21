---
title: "Sfida: Composizione tipografica della homepage di una scuola comunitaria"
short-title: "Sfida: Homepage di una scuola comunitaria"
slug: Learn_web_development/Core/Text_styling/Typesetting_a_homepage
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Text_styling/Web_fonts", "Learn_web_development/Core/CSS_layout", "Learn_web_development/Core/Text_styling")}}

In questa sfida, metteremo alla prova la tua comprensione di tutte le tecniche di stilizzazione del testo che abbiamo coperto in questo modulo, facendoti stilizzare il testo per la homepage di una scuola comunitaria. Potresti anche divertirti lungo la strada.

## Punto di partenza

Per iniziare questa sfida, dovresti:

- Scaricare i file [HTML](https://github.com/mdn/learning-area/blob/main/css/styling-text/typesetting-a-homepage-start/index.html) e [CSS](https://github.com/mdn/learning-area/blob/main/css/styling-text/typesetting-a-homepage-start/style.css) per l'esercizio, e l'icona del [collegamento esterno](https://github.com/mdn/learning-area/blob/main/css/styling-text/typesetting-a-homepage-start/external-link-52.png) fornita.
- Fare una copia di essi sul tuo computer locale.

In alternativa, potresti utilizzare un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
Potresti incollare l'HTML e inserire il CSS in uno di questi editor online, e usare [questa icona di collegamento esterno](https://mdn.github.io/learning-area/css/styling-text/typesetting-a-homepage-start/external-link-52.png) come immagine di sfondo.

> [!NOTE]
> Se ti blocchi, puoi contattarci tramite uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Progetto

Ti è stato fornito del semplice HTML per la homepage di un immaginario college comunitario, oltre a del CSS che stila la pagina in un layout a tre colonne e fornisce qualche altra stilizzazione elementare. Devi scrivere le tue aggiunte CSS sotto il commento in fondo al file CSS per assicurarti che sia facile identificare le parti da te fatte. Non preoccuparti se alcuni selettori sono ripetitivi; tralasceremo questa volta.

Font:

- Innanzitutto, scarica un paio di font gratuiti. Poiché si tratta di un college, i font dovrebbero essere scelti per dare alla pagina un aspetto abbastanza serio, formale, e affidabile: un font serif per tutto il sito per il testo generale, accoppiato con un sans-serif o un slab serif per i titoli potrebbe essere una buona scelta.
- Usa un servizio appropriato per generare codice `@font-face` a prova di futuro per questi due font.
- Applica il tuo font per il corpo a tutta la pagina e il font per i titoli ai tuoi titoli.

Stilizzazione generale del testo:

- Dai alla pagina una `font-size` generale di `10px`.
- Dai ai tuoi titoli e ad altri tipi di elementi dimensioni di font appropriate definite usando un'unità relativa adatta.
- Dai al tuo testo del corpo un `line-height` adeguato.
- Centra il tuo titolo di livello più alto sulla pagina.
- Dai ai tuoi titoli un po' di `letter-spacing` per evitare che siano troppo schiacciati e permettere alle lettere di respirare un po'.
- Dai al tuo testo del corpo del `letter-spacing` e del `word-spacing`, come appropriato.
- Dai al primo paragrafo dopo ogni titolo nella sezione un po' di rientro, ad esempio 20px.

Link:

- Dai agli stati del link, visitato, messa a fuoco, e al passaggio del mouse dei colori che si accordano con il colore delle barre orizzontali nella parte superiore e inferiore della pagina.
- Fai sì che i link siano sottolineati per impostazione predefinita, ma quando ci passi sopra col mouse o li metti a fuoco, la sottolineatura scompare.
- Rimuovi il contorno di messa a fuoco predefinito da TUTTI i link sulla pagina.
- Dai allo stato attivo una stilizzazione visibilmente diversa affinché si distingua bene, ma che si adatti comunque al design generale della pagina.
- Assicurati che i link _esterni_ abbiano l'icona del collegamento esterno inserita accanto a loro.

Liste:

- Assicurati che lo spaziamento delle tue liste e dei tuoi elementi di lista funzioni bene con lo stile generale della pagina. Ogni elemento di lista dovrebbe avere lo stesso `line-height` di una linea di un paragrafo, e ogni lista dovrebbe avere lo stesso spazio in cima e in fondo come hai tra i paragrafi.
- Dai ai tuoi elementi di lista un bel proiettile che sia appropriato per il design della pagina. Sta a te decidere se scegliere un'immagine di proiettile personalizzata o qualcos'altro.

Menu di navigazione:

- Stila il tuo menu di navigazione in modo che sia in armonia con la pagina.

## Consigli e suggerimenti

- Non è necessario modificare l'HTML in alcun modo per questo esercizio.
- Non è necessario che il menu di navigazione sembri dei pulsanti, ma deve essere un po' più alto in modo che non sembri ridicolo sul lato della pagina; ricorda anche che devi farne un menu di navigazione verticale.

## Esempio

Lo screenshot seguente mostra un esempio di come potrebbe apparire il design finito:

![Uno screenshot del design di sfida finito. Il titolo principale recita 'St Huxley's Community College'. C'è una linea rossa che separa il banner dell'intestazione dal contenuto. Il contenuto principale ha tre colonne, due contenenti testo, e un menu di navigazione verticale nella terza colonna.](example2.png)

{{PreviousMenuNext("Learn_web_development/Core/Text_styling/Web_fonts", "Learn_web_development/Core/CSS_layout", "Learn_web_development/Core/Text_styling")}}
