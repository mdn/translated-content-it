---
title: "Sfida: Marcare una lettera"
short-title: "Sfida: Marcare la lettera"
slug: Learn_web_development/Core/Structuring_content/Marking_up_a_letter
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Creating_links", "Learn_web_development/Core/Structuring_content/Structuring_a_page_of_content", "Learn_web_development/Core/Structuring_content")}}

Tutti noi impariamo a scrivere una lettera prima o poi; è anche un esempio utile per testare le nostre competenze di formattazione del testo. In questa sfida, avrai una lettera da marcare come test per le tue competenze di formattazione del testo HTML, nonché per i collegamenti ipertestuali e l'uso corretto dell'elemento HTML `<head>`.

## Punto di partenza

Per iniziare, recupera il [testo grezzo che devi marcare](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/marking-up-a-letter-start/letter-text.txt), e il [CSS per stilizzare l'HTML](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/marking-up-a-letter-start/css.txt). Crea un nuovo file `.html` utilizzando il tuo editor di testo o usa un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).

> [!NOTE]
> Se ti blocchi, puoi contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Breve descrizione del progetto

Per questo progetto, il tuo compito è marcare una lettera che deve essere ospitata su un intranet universitario. La lettera è una risposta da parte di un ricercatore ad un prospettivo studente di dottorato riguardante la loro candidatura all'università.

### Semantica del blocco/strutturale

- Usa una struttura del documento appropriata includendo il doctype, e gli elementi {{htmlelement("html")}}, {{htmlelement("head")}} e {{htmlelement("body")}}.
- In generale, la lettera dovrebbe essere marcata come un'organizzazione di intestazioni e paragrafi, con la seguente eccezione. C'è un'intestazione di livello superiore (la linea "Re:") e tre intestazioni di secondo livello.
- Usa un tipo di lista appropriato per marcare le date di inizio semestre, le materie di studio e le danze esotiche.
- Metti gli indirizzi dentro elementi {{htmlelement("address")}}. Ogni riga dell'indirizzo dovrebbe essere su una nuova riga, ma non in un nuovo paragrafo.

### Semantica inline

- I nomi del mittente e del destinatario (e _Tel_ e _Email_) devono essere marcati con una forte importanza.
- Le quattro date nel documento dovrebbero avere elementi appropriati contenenti date leggibili dalle macchine.
- Il primo indirizzo e la prima data nella lettera dovrebbero avere un attributo di classe con valore _sender-column_. Il CSS che aggiungerai in seguito le allineerà a destra, come dovrebbe essere nel caso di un layout di lettera classico.
- Marca i seguenti cinque acronimi/abbreviazioni nel testo principale della lettera — "PhD," "HTML," "CSS," "BC," e "Esq." — per fornire espansioni per ciascuno.
- I sei apici/sottoscritti dovrebbero essere marcati appropriatamente — nelle formule chimiche, e nei numeri 103 e 104 (dovrebbero essere 10 alla potenza di 3 e 4, rispettivamente).
- Prova a marcare almeno due parole appropriate nel testo con forte importanza/enfasi.
- Ci sono due luoghi dove la lettera dovrebbe avere un collegamento ipertestuale. Aggiungi collegamenti appropriati con titoli. Per il luogo a cui i collegamenti puntano, puoi utilizzare `http://example.com` come URL.
- Marca il motto dell'università e la citazione con elementi appropriati.

### La testa del documento

- Il set di caratteri del documento dovrebbe essere impostato come utf-8 utilizzando il meta tag appropriato.
- L'autore della lettera dovrebbe essere specificato in un meta tag appropriato.
- Il CSS fornito dovrebbe essere incluso all'interno di un tag appropriato.

## Suggerimenti e consigli

- Usa il [validator HTML W3C](https://validator.w3.org/) per convalidare il tuo HTML. Premiati con punti bonus se è valido.
- Non hai bisogno di conoscere alcun CSS per questo compito. Devi solo inserire il CSS fornito all'interno di un elemento HTML.

## Esempio

Il seguente screenshot mostra un esempio di come potrebbe apparire la lettera dopo essere stata marcata.

![Esempio](letter-update.png)

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Creating_links", "Learn_web_development/Core/Structuring_content/Structuring_a_page_of_content", "Learn_web_development/Core/Structuring_content")}}
