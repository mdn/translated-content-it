---
title: "Sfida: Strutturare una tabella dati dei pianeti"
short-title: "Sfida: Tabella dati dei pianeti"
slug: Learn_web_development/Core/Structuring_content/Planet_data_table
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Table_accessibility", "Learn_web_development/Core/Structuring_content/HTML_forms", "Learn_web_development/Core/Structuring_content")}}

In questa sfida, ti forniamo alcuni dati sui pianeti del nostro sistema solare. Il tuo compito è strutturarli in una tabella HTML accessibile.

## Punto di partenza

Per iniziare la valutazione, crea copie locali di [blank-template.html](https://github.com/mdn/learning-area/blob/main/html/tables/assessment-start/blank-template.html), [minimal-table.css](https://github.com/mdn/learning-area/blob/main/html/tables/assessment-start/minimal-table.css), e [planets-data.txt](https://github.com/mdn/learning-area/blob/main/html/tables/assessment-start/planets-data.txt) in una nuova directory nel tuo computer locale.

> [!NOTE]
> Puoi provare le soluzioni nel tuo editor di codice o in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
>
> Se hai difficoltà, puoi contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Breve panoramica del progetto

Stai lavorando in una scuola; attualmente i tuoi studenti stanno studiando i pianeti del nostro sistema solare, e vuoi fornire loro un insieme di dati facile da seguire per consultare fatti e cifre sui pianeti. Una tabella dati HTML sarebbe ideale — devi prendere i dati grezzi disponibili e trasformarli in una tabella, seguendo i passi seguenti.

### Passaggi da completare

I passaggi seguenti descrivono ciò che devi fare per completare l'esempio di tabella. Tutti i dati di cui avrai bisogno sono contenuti nel file `planets-data.txt`. Se hai difficoltà a visualizzare i dati, guarda l'esempio dal vivo qui sotto, o prova a disegnare un diagramma.

1. Apri la tua copia di `blank-template.html`, e inizia la tabella dandole un contenitore esterno, un'intestazione della tabella e un corpo tabella. Non hai bisogno di un piè di pagina della tabella per questo esempio.
2. Aggiungi la didascalia fornita alla tua tabella.
3. Aggiungi una riga all'intestazione della tabella contenente tutte le intestazioni delle colonne.
4. Crea tutte le righe di contenuto all'interno del corpo della tabella, ricordandoti di rendere tutte le intestazioni di riga semanticamente intestazioni.
5. Assicurati che tutti i contenuti siano posizionati nelle celle giuste — nei dati grezzi, ogni riga di dati sui pianeti è mostrata accanto al pianeta associato.
6. Aggiungi attributi per associare in modo inequivocabile le intestazioni di riga e colonna con le righe, colonne o gruppi di righe per cui fungono da intestazioni.
7. Aggiungi un [bordo](/it/docs/Web/CSS/border) nero solo intorno alla colonna che contiene tutte le intestazioni di riga dei nomi dei pianeti.

## Suggerimenti e consigli

- La prima cella della riga di intestazione deve essere vuota e coprire due colonne.
- Le intestazioni di gruppo delle righe (ad esempio, _Pianeti gioviani_) che si trovano a sinistra delle intestazioni di riga dei nomi dei pianeti (ad esempio, _Saturno_) sono un po' difficili da sistemare — devi assicurarti che ciascuna copra il numero corretto di righe e colonne.
- Un modo di associare le intestazioni alle loro righe/colonne è molto più semplice dell'altro modo.

## Esempio

La tabella finita dovrebbe apparire così:

![Una tabella complessa ha una didascalia sopra di essa. Le celle della riga superiore sono intestazioni di colonne. Ci sono tre colonne di intestazioni. Le prime due colonne hanno celle unite, mentre la terza colonna ha intestazioni individuali per ciascuna riga. Tutto il testo è centrato. Le intestazioni e ogni altra riga hanno un leggero colore di sfondo.](assessment-table.png)

Puoi anche [vedere l'esempio dal vivo qui](https://mdn.github.io/learning-area/html/tables/assessment-finished/planets-data.html) (non guardare il codice sorgente — non barare!)

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Table_accessibility", "Learn_web_development/Core/Structuring_content/HTML_forms", "Learn_web_development/Core/Structuring_content")}}
