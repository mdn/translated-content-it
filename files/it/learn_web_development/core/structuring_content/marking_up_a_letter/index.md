---
title: "Sfida: Contrassegnare una lettera"
short-title: "Sfida: Marcatura della lettera"
slug: Learn_web_development/Core/Structuring_content/Marking_up_a_letter
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Creating_links", "Learn_web_development/Core/Structuring_content/Structuring_a_page_of_content", "Learn_web_development/Core/Structuring_content")}}

Tutti impariamo a scrivere una lettera prima o poi; è anche un esempio utile per mettere alla prova le nostre abilità di formattazione del testo. In questa sfida, avrà una lettera da marcare come test per le sue abilità di formattazione del testo HTML, così come hyperlink e uso corretto dell'elemento `<head>` di HTML.

## Punto di partenza

Per iniziare, ottenga il [testo grezzo che deve contrassegnare](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/marking-up-a-letter-start/letter-text.txt), e il [CSS per stilizzare l'HTML](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/marking-up-a-letter-start/css.txt).
Crei un nuovo file `.html` utilizzando il suo editor di testo o usi un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).

> [!NOTE]
> Se è bloccato, può contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Breve progetto

Per questo progetto, il suo compito è di marcare una lettera che deve essere ospitata su un intranet universitario. La lettera è una risposta da un ricercatore a un potenziale studente di dottorato riguardo la loro domanda all'università.

### Semantica del blocco/strutturale

- Usi una struttura del documento appropriata, incluso il doctype, e gli elementi {{htmlelement("html")}}, {{htmlelement("head")}} e {{htmlelement("body")}}.
- In generale, la lettera dovrebbe essere marcata come un'organizzazione di intestazioni e paragrafi, con la seguente eccezione. C'è un'intestazione di livello superiore (la linea "Re:") e tre intestazioni di secondo livello.
- Usi un tipo di lista appropriato per marcare le date di inizio semestre, le materie di studio e le danze esotiche.
- Metta i due indirizzi all'interno di elementi {{htmlelement("address")}}. Ogni riga dell'indirizzo dovrebbe trovarsi su una nuova riga, ma non in un nuovo paragrafo.

### Semantica inline

- I nomi del mittente e del destinatario (e _Tel_ ed _Email_) devono essere marcati con forte importanza.
- Le quattro date nel documento dovrebbero avere elementi appropriati contenenti date leggibili dalla macchina.
- Il primo indirizzo e la prima data nella lettera dovrebbero avere un attributo di classe con il valore _sender-column_. Il CSS che aggiungerà più tardi farà sì che questi siano allineati a destra, come dovrebbe essere nel caso di un layout classico della lettera.
- Marcare i seguenti cinque acronimi/abbreviazioni nel testo principale della lettera — "PhD," "HTML," "CSS," "BC," e "Esq." — per fornire le espansioni di ciascuno.
- I sei apici/sottotitoli dovrebbero essere marcati appropriatamente — nelle formule chimiche, e nei numeri 103 e 104 (dovrebbero essere 10 alla potenza rispettivamente di 3 e 4).
- Provare a marcare almeno due parole appropriate nel testo con forte importanza/enfasi.
- Ci sono due luoghi dove la lettera dovrebbe avere un hyperlink. Aggiunga link appropriati con titoli. Per la posizione a cui i link puntano, può utilizzare `http://example.com` come URL.
- Marcare il motto dell'università e la citazione con elementi appropriati.

### L'intestazione del documento

- Il set di caratteri del documento dovrebbe essere impostato come utf-8 usando il meta tag appropriato.
- L'autore della lettera dovrebbe essere specificato in un meta tag appropriato.
- Il CSS fornito dovrebbe essere incluso all'interno di un tag appropriato.

## Consigli e suggerimenti

- Utilizzi il [validator HTML del W3C](https://validator.w3.org/) per validare il suo HTML. Si assegni punti bonus se convalida.
- Non ha bisogno di conoscere CSS per fare questo compito. Deve solo mettere il CSS fornito all'interno di un elemento HTML.

## Esempio

Il seguente screenshot mostra un esempio di come potrebbe apparire la lettera dopo essere stata marcata.

![Esempio](letter-update.png)

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Creating_links", "Learn_web_development/Core/Structuring_content/Structuring_a_page_of_content", "Learn_web_development/Core/Structuring_content")}}
