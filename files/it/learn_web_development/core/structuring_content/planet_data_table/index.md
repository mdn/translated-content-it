---
title: "Sfida: Strutturare una tabella dati sui pianeti"
short-title: "Sfida: Tabella dati sui pianeti"
slug: Learn_web_development/Core/Structuring_content/Planet_data_table
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Table_accessibility", "Learn_web_development/Core/Structuring_content/HTML_forms", "Learn_web_development/Core/Structuring_content")}}

In questa sfida, le forniremo alcuni dati sui pianeti del nostro sistema solare. Il suo compito è strutturarli in una tabella HTML accessibile.

## Punto di partenza

Per iniziare la valutazione, faccia copie locali di [blank-template.html](https://github.com/mdn/learning-area/blob/main/html/tables/assessment-start/blank-template.html), [minimal-table.css](https://github.com/mdn/learning-area/blob/main/html/tables/assessment-start/minimal-table.css) e [planets-data.txt](https://github.com/mdn/learning-area/blob/main/html/tables/assessment-start/planets-data.txt) in una nuova directory nel suo computer locale.

> [!NOTE]
> Può provare le soluzioni nel suo editor di codice o in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/) o [Glitch](https://glitch.com/).
>
> Se si blocca, può contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Descrizione del progetto

Lavora in una scuola; attualmente i suoi studenti stanno studiando i pianeti del nostro sistema solare, e lei vuole fornire loro un insieme di dati facile da seguire per cercare fatti e cifre sui pianeti. Una tabella dati HTML sarebbe ideale — deve prendere i dati grezzi a disposizione e trasformarli in una tabella, seguendo i passaggi di seguito.

### Passaggi da completare

I seguenti passaggi descrivono ciò che deve fare per completare l'esempio di tabella. Tutti i dati di cui ha bisogno sono contenuti nel file `planets-data.txt`. Se ha difficoltà a visualizzare i dati, guardi l'esempio dal vivo qui sotto, o provi a disegnare un diagramma.

1. Apra la sua copia di `blank-template.html` e inizi la tabella dandole un contenitore esterno, un'intestazione e un corpo. Non ha bisogno di un piè di pagina per questo esempio.
2. Aggiunga la didascalia fornita alla sua tabella.
3. Aggiunga una riga all'intestazione della tabella contenente tutte le intestazioni delle colonne.
4. Crei tutte le righe di contenuto all'interno del corpo della tabella, ricordando di rendere tutte le intestazioni delle righe semanticamente come intestazioni.
5. Si assicuri che tutto il contenuto sia inserito nelle celle corrette — nei dati grezzi, ogni riga di dati dei pianeti è mostrata accanto al relativo pianeta.
6. Aggiunga attributi per associare in modo inequivocabile le intestazioni di righe e colonne alle righe, colonne o gruppi di righe di cui fungono da intestazioni.
7. Aggiunga un [bordo](/it/docs/Web/CSS/border) nero solo attorno alla colonna che contiene tutte le intestazioni delle righe con i nomi dei pianeti.

## Suggerimenti e consigli

- La prima cella della riga di intestazione deve essere vuota e deve estendersi due colonne.
- Le intestazioni dei gruppi di righe (ad esempio, _Pianeti gioviani_) che si trovano a sinistra delle intestazioni delle righe con i nomi dei pianeti (ad esempio, _Saturno_) sono un po' complicate da sistemare — deve assicurarsi che ognuna si estenda sul corretto numero di righe e colonne.
- Un modo per associare le intestazioni alle loro righe/colonne è molto più facile dell'altro.

## Esempio

La tabella finita dovrebbe apparire così:

![Tabella complessa con una didascalia sopra. Le celle della prima riga sono intestazioni di colonna. Ci sono tre colonne di intestazioni. Le prime due colonne hanno celle unite, mentre la terza colonna ha intestazioni individuali per ogni riga. Tutto il testo è centrato. Le intestazioni e ogni altra riga hanno un leggero colore di sfondo.](assessment-table.png)

Può anche [vedere l'esempio dal vivo qui](https://mdn.github.io/learning-area/html/tables/assessment-finished/planets-data.html) (non guardi il codice sorgente — niente imbrogli!)

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Table_accessibility", "Learn_web_development/Core/Structuring_content/HTML_forms", "Learn_web_development/Core/Structuring_content")}}
