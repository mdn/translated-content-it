---
title: "Sfida: Impaginazione per una homepage di una scuola comunitaria"
short-title: "Sfida: Homepage di una scuola comunitaria"
slug: Learn_web_development/Core/Text_styling/Typesetting_a_homepage
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Text_styling/Web_fonts", "Learn_web_development/Core/CSS_layout", "Learn_web_development/Core/Text_styling")}}

In questa sfida, metteremo alla prova la sua comprensione di tutte le tecniche di stile del testo che abbiamo trattato in questo modulo facendole stilizzare il testo per la homepage di una scuola comunitaria. Potrebbe anche divertirsi un po' lungo il percorso.

## Punto di partenza

Per iniziare questa sfida, dovrebbe:

- Scaricare i file di [HTML](https://github.com/mdn/learning-area/blob/main/css/styling-text/typesetting-a-homepage-start/index.html) e [CSS](https://github.com/mdn/learning-area/blob/main/css/styling-text/typesetting-a-homepage-start/style.css) per l'esercizio, e l' [icona di link esterno](https://github.com/mdn/learning-area/blob/main/css/styling-text/typesetting-a-homepage-start/external-link-52.png) fornita.
- Effettuarne una copia sul proprio computer locale.

In alternativa, potrebbe utilizzare un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
Potrebbe incollare l'HTML e completare il CSS in uno di questi editor online, e utilizzare [questa icona di link esterno](https://mdn.github.io/learning-area/css/styling-text/typesetting-a-homepage-start/external-link-52.png) come immagine di sfondo.

> [!NOTE]
> Se incontra difficoltà, può contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Brief del progetto

Le è stato fornito del semplice HTML per la homepage di un immaginario college comunitario, oltre a un po' di CSS che stila la pagina in un layout a tre colonne e fornisce altri stili rudimentali. Dovrà scrivere le sue aggiunte CSS sotto il commento in fondo al file CSS per assicurarsi che sia facile individuare le parti che ha realizzato. Non si preoccupi se alcuni selettori sono ripetitivi; questa volta la lasciamo passare.

Font:

- Prima di tutto, scarichi un paio di font gratuiti da usare. Poiché si tratta di un college, i font dovrebbero essere scelti per dare alla pagina un aspetto abbastanza serio, formale e affidabile: un font serif per l'intero sito per il testo del corpo principale, accoppiato con un sans-serif o slab serif per i titoli potrebbe essere ideale.
- Utilizzi un servizio adatto per generare codice `@font-face` a prova di errore per questi due font.
- Applichi il font per il corpo dell'intera pagina, e il font per i titoli ai suoi titoli.

Stilizzazione generale del testo:

- Imposti un `font-size` di `10px` per tutto il sito.
- Definisca un font-size appropriato per i titoli e altri tipi di elementi utilizzando un'unità relativa adeguata.
- Imposti una `line-height` adatta per il corpo del testo.
- Centri il suo titolo di primo livello nella pagina.
- Aggiunga un po' di `letter-spacing` ai titoli per evitare che siano troppo schiacciati e permettere alle lettere di "respirare".
- Aggiunga un po' di `letter-spacing` e `word-spacing` al testo del corpo, come appropriato.
- Imposti una leggera text-indentation, diciamo di 20px, per il primo paragrafo dopo ciascun titolo nel `<section>`.

Link:

- Imposti per gli stati di link, visitato, focus e hover dei colori che si accordano con il colore delle barre orizzontali in cima e in fondo alla pagina.
- Faccia in modo che i link siano sottolineati di default, ma quando vi si passa sopra o vi si concentra, la sottolineatura scompaia.
- Rimuova il contorno di focus predefinito da TUTTI i link nella pagina.
- Renderizzi lo stato attivo in modo che si distingua nettamente, ma che si adatti comunque al design complessivo della pagina.
- Faccia in modo che i link _esterni_ abbiano l'icona di link esterno inserita accanto ad essi.

Liste:

- Si assicuri che la spaziatura delle sue liste e degli elementi delle liste funzioni bene con lo stile complessivo della pagina. Ogni elemento di lista dovrebbe avere la stessa `line-height` di una linea di paragrafo, e ciascuna lista dovrebbe avere la stessa spaziatura in cima e in fondo come quella tra i paragrafi.
- Scelga un bel proiettile per gli elementi della lista che si adatti al design della pagina. È a sua discrezione scegliere un'immagine di proiettile personalizzata o qualcos'altro.

Menu di navigazione:

- Stilizzi il suo menu di navigazione in modo che si armonizzi con la pagina.

## Suggerimenti e consigli

- Non è necessario modificare l'HTML in alcun modo per questo esercizio.
- Non è necessariamente obbligato a fare in modo che il menu di navigazione somigli a dei bottoni, ma dovrebbe essere un po' più alto in modo che non sembri ridicolo sul lato della pagina; ricordi anche che deve rendere questo menu di navigazione verticale.

## Esempio

Il seguente screenshot mostra un esempio di come potrebbe apparire il design finale:

![Uno screenshot del design finale della sfida. Il titolo superiore recita 'St Huxley's Community College'. C'è una linea rossa che separa l'intestazione del banner dal contenuto. Il contenuto principale ha tre colonne, due contenenti testo, e una barra di navigazione verticale nella terza colonna.](example2.png)

{{PreviousMenuNext("Learn_web_development/Core/Text_styling/Web_fonts", "Learn_web_development/Core/CSS_layout", "Learn_web_development/Core/Text_styling")}}
