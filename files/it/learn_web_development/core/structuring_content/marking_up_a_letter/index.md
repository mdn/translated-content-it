---
title: "Sfida: contrassegnare una lettera"
short-title: "Sfida: markup di una lettera"
slug: Learn_web_development/Core/Structuring_content/Marking_up_a_letter
l10n:
  sourceCommit: 39b59de03c08a5de12ce99a217988f6a0756407d
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Test_your_skills/Advanced_HTML_text", "Learn_web_development/Core/Structuring_content/Structuring_documents", "Learn_web_development/Core/Structuring_content")}}

Prima o poi tutti imparano a scrivere una lettera; è anche un esempio utile per mettere alla prova le competenze di formattazione del testo. In questa sfida viene fornita una lettera da contrassegnare come test delle competenze di formattazione del testo HTML e della conoscenza dei contenuti di HTML `<head>`.

## Punto di partenza

Per iniziare, fare clic sul pulsante **Play** nel pannello dell'esempio di codice seguente per aprire il testo del corpo fornito nell'MDN Playground. Seguire le istruzioni nelle sezioni successive per contrassegnare il testo in modo appropriato.

```html-nolint live-sample___start
Dr. Eleanor Gaye
Awesome Science faculty
University of Awesome
Bobtown, CA 99999,
USA
Tel: 123-456-7890
Email: no_reply@example.com

20 January 2016

Miss Eileen Dover
4321 Cliff Top Edge
Dover, CT9 XXX
UK

Re: Eileen Dover university application

Dear Eileen,

Thank you for your recent application to join us at the University of
Awesome's science faculty to study as part of your
PhD (Doctor of Philosophy) next year. I will answer your
questions one by one, in the following sections.

Starting dates

We are happy to accommodate you starting your study with us at any time,
however it would suit us better if you could start at the beginning of a
semester; the start dates for each one are as follows:

First semester: 9 September 2016
Second semester: 15 January 2017
Third semester: 2 May 2017

Please let me know if this is ok, and if so which start date you would
prefer.

Subjects of study

At the Awesome Science Faculty, we have a pretty open-minded research
facility — as long as the subjects fall somewhere in the realm of science
and technology. You seem like an intelligent, dedicated researcher, and
just the kind of person we'd like to have on our team. Saying that, of the
ideas you submitted we were most intrigued by are as follows, in order of
priority:

Turning H2O into wine, and the health benefits of Resveratrol
(C14H12O3).
Measuring the effect on performance of funk bass players at temperatures
exceeding 30°C (86°F), when the audience size exponentially increases
(effect of 3 × 103 increasing to 3 × 104).
HTML, Hypertext Markup Language, and CSS,
Cascading Style Sheets, constructs for representing musical scores.

So please can you provide more information on each of these subjects,
including how long you'd expect the research to take, required staff and
other resources, and anything else you think we'd need to know? Thanks.

Exotic dance moves

Yes, you are right! As part of my post-doctorate work, I
did study exotic tribal dances. To answer your question, my
favorite dances are as follows, with definitions:

Polynesian chicken dance
    A little known but very influential dance dating back as far as
    300 BCE, a whole village would
    dance around in a circle like chickens, to encourage their livestock to
    be "fruitful".
Icelandic brownian shuffle
    Before the Icelanders developed fire as a means of getting warm, they
    used to practice this dance, which involved huddling close together in a
    circle on the floor, and shuffling their bodies around in imperceptibly
    tiny, very rapid movements. One of my fellow students used to say that
    he thought this dance inspired modern styles such as Twerking.
Arctic robot dance
    An interesting example of historic misinformation, English explorers in
    the 1960s believed to have discovered a new dance style characterized by
    "robotic", stilted movements, being practiced by inhabitants of Northern
    Alaska and Canada. Later on however it was discovered that they were
    just moving like this because they were really cold.

Yours sincerely,

Dr Eleanor Gaye

University of Awesome motto: Be awesome to each other. --
The memoirs of Bill S Preston, Esq.
```

{{embedlivesample("start", "100%", "200px")}}

## Descrizione del progetto

Per questo progetto, il compito è contrassegnare una lettera che deve essere ospitata su una intranet universitaria. La lettera è una risposta di un ricercatore a un potenziale studente di dottorato riguardo alla sua domanda di ammissione all'università.

### Semantica a blocchi/strutturale

- Aggiungere una struttura HTML appropriata, inclusi doctype ed elementi {{htmlelement("html")}}, {{htmlelement("head")}} e {{htmlelement("body")}}.
- In generale, la lettera dovrebbe essere contrassegnata come un'organizzazione di titoli e paragrafi, tranne gli indirizzi menzionati nel punto successivo. È presente un titolo di primo livello (la riga "Re:") e tre titoli di secondo livello.
- Inserire i due indirizzi all'interno di elementi {{htmlelement("address")}}. Ogni riga dell'indirizzo deve trovarsi su una nuova riga, ma non in un nuovo paragrafo.
- Usare un tipo di elenco appropriato per contrassegnare le date di inizio del semestre, le materie di studio e le danze esotiche.

### Semantica inline

- I nomi del mittente e del destinatario (e _Tel_ ed _Email_) devono essere contrassegnati con forte importanza.
- Le quattro date nel documento devono essere racchiuse in elementi appropriati contenenti date leggibili dalla macchina.
- Al primo indirizzo e alla prima data della lettera deve essere impostato un valore dell'attributo `class` pari a `sender-column`. Il CSS che verrà aggiunto in seguito farà sì che siano allineati a destra, come dovrebbe avvenire nel layout di una lettera classica.
- Contrassegnare i cinque acronimi/abbreviazioni seguenti nel testo principale della lettera — "PhD," "HTML," "CSS," "BCE" ed "Esq." — per fornire l'espansione di ciascuno.
- I sei pedici/apici devono essere contrassegnati in modo appropriato — nelle formule chimiche e nei numeri 103 e 104 (dovrebbero essere 10 elevato alla terza e alla quarta potenza, rispettivamente).
- Contrassegnare almeno altre due parole appropriate nel testo con forte importanza/enfasi.
- Contrassegnare la citazione del motto dell'università e la relativa fonte con elementi appropriati.

### L'intestazione del documento

- Il set di caratteri del documento deve essere impostato su `utf-8` usando il tag `<meta>` appropriato.
- L'autore della lettera deve essere specificato in un tag `<meta>` appropriato.
- Impostare la lingua del documento su `en-US`.
- Includere il testo seguente all'interno di un elemento titolo del documento: "Awesome science application correspondence".
- Il CSS seguente deve essere incluso all'interno di un elemento appropriato nell'intestazione:

  ```css
  body {
    font: 1.2em / 1.5 system-ui;
  }

  .sender-column {
    text-align: right;
  }

  h1 {
    font-size: 1.5em;
  }

  h2 {
    font-size: 1.3em;
  }
  ```

## Suggerimenti

- Usare il [validatore HTML del W3C](https://validator.w3.org/) per convalidare l'HTML. Assegnare punti bonus se la convalida ha esito positivo.
- Non è necessario conoscere CSS per svolgere questo esercizio. Basta inserire il CSS fornito all'interno di un elemento HTML.

## Esempio

Il seguente esempio live mostra come dovrebbe apparire la lettera dopo il markup. Se risulta difficile capire come ottenere parte di questo risultato, consultare la soluzione sotto l'esempio live.

{{embedlivesample("finish", "100%", "500px")}}

<details>
<summary>Fare clic qui per visualizzare la soluzione</summary>

L'HTML completato dovrebbe apparire così:

```html live-sample___finish
<!doctype html>
<html lang="en-US">
  <head>
    <meta charset="utf-8" />
    <meta name="author" content="Dr. Eleanor Gaye" />
    <title>Awesome science application correspondence</title>
    <style>
      body {
        font: 1.2em / 1.5 system-ui;
      }

      .sender-column {
        text-align: right;
      }

      h1 {
        font-size: 1.5em;
      }

      h2 {
        font-size: 1.3em;
      }
    </style>
  </head>
  <body>
    <address class="sender-column">
      <strong>Dr. Eleanor Gaye</strong><br />
      Awesome Science faculty<br />
      University of Awesome<br />
      Bobtown, CA 99999,<br />
      USA<br />
      <strong>Tel</strong>: 123-456-7890<br />
      <strong>Email</strong>: no_reply@example.com
    </address>

    <p class="sender-column">
      <time datetime="2016-01-20">20 January 2016</time>
    </p>

    <address>
      <strong>Miss Eileen Dover</strong><br />
      4321 Cliff Top Edge<br />
      Dover, CT9 XXX<br />
      UK
    </address>

    <h1>Re: Eileen Dover university application</h1>

    <p>Dear Eileen,</p>

    <p>
      Thank you for your recent application to join us at the University of
      Awesome's science faculty to study as part of your
      <abbr>PhD</abbr> (Doctor of Philosophy) next year. I will answer your
      questions one by one, in the following sections.
    </p>

    <h2>Starting dates</h2>

    <p>
      We are happy to accommodate you starting your study with us at any time,
      however it would suit us better if you could start at the beginning of a
      semester; the start dates for each one are as follows:
    </p>

    <ul>
      <li>
        First semester: <time datetime="2016-09-09">9 September 2016</time>
      </li>
      <li>
        Second semester: <time datetime="2017-01-15">15 January 2017</time>
      </li>
      <li>Third semester: <time datetime="2017-05-02">2 May 2017</time></li>
    </ul>

    <p>
      Please let me know if this is ok, and if so which start date you would
      prefer.
    </p>

    <h2>Subjects of study</h2>

    <p>
      At the Awesome Science Faculty, we have a pretty open-minded research
      facility — as long as the subjects fall somewhere in the realm of science
      and technology. You seem like an intelligent, dedicated researcher, and
      just the kind of person we'd like to have on our team. Saying that, of the
      ideas you submitted we were most intrigued by are as follows, in order of
      priority:
    </p>

    <ol>
      <li>
        Turning H<sub>2</sub>O into wine, and the health benefits of Resveratrol
        (C<sub>14</sub>H<sub>12</sub>O<sub>3</sub>).
      </li>
      <li>
        Measuring the effect on performance of funk bass players at temperatures
        exceeding 30°C (86°F), when the audience size exponentially increases
        (effect of 3 × 10<sup>3</sup> increasing to 3 × 10<sup>4</sup>).
      </li>
      <li>
        <abbr>HTML</abbr>, Hypertext Markup Language, and <abbr>CSS</abbr>,
        Cascading Style Sheets, constructs for representing musical scores.
      </li>
    </ol>

    <p>
      So please can you provide more information on each of these subjects,
      including how long you'd expect the research to take, required staff and
      other resources, and anything else you think we'd need to know? Thanks.
    </p>

    <h2>Exotic dance moves</h2>

    <p>
      Yes, you are right! As part of my post-doctorate work, I
      <em>did</em> study exotic tribal dances. To answer your question, my
      favorite dances are as follows, with definitions:
    </p>

    <dl>
      <dt>Polynesian chicken dance</dt>
      <dd>
        A little known but <em>very</em> influential dance dating back as far as
        300 <abbr title="Before Common Era">BCE</abbr>, a whole village would
        dance around in a circle like chickens, to encourage their livestock to
        be "fruitful".
      </dd>
      <dt>Icelandic brownian shuffle</dt>
      <dd>
        Before the Icelanders developed fire as a means of getting warm, they
        used to practice this dance, which involved huddling close together in a
        circle on the floor, and shuffling their bodies around in imperceptibly
        tiny, very rapid movements. One of my fellow students used to say that
        he thought this dance inspired modern styles such as Twerking.
      </dd>
      <dt>Arctic robot dance</dt>
      <dd>
        An interesting example of historic misinformation, English explorers in
        the 1960s believed to have discovered a new dance style characterized by
        "robotic", stilted movements, being practiced by inhabitants of Northern
        Alaska and Canada. Later on however it was discovered that they were
        just moving like this because they were really cold.
      </dd>
    </dl>

    <p>Yours sincerely,</p>

    <p>Dr Eleanor Gaye</p>

    <p>
      University of Awesome motto: <q>Be awesome to each other.</q> --
      <cite
        >The memoirs of Bill S Preston, <abbr title="Esquire">Esq.</abbr></cite
      >
    </p>
  </body>
</html>
```

</details>

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Test_your_skills/Advanced_HTML_text", "Learn_web_development/Core/Structuring_content/Structuring_documents", "Learn_web_development/Core/Structuring_content")}}
