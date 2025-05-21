---
title: "Sfida: Stilizzare una pagina biografica"
short-title: "Sfida: Pagina biografica"
slug: Learn_web_development/Core/Styling_basics/Styling_a_bio_page
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Getting_started", "Learn_web_development/Core/Styling_basics/Basic_selectors", "Learn_web_development/Core/Styling_basics")}}

In questa sfida stilerai una pagina biografica semplice, mettendo alla prova alcune delle abilità che hai appreso nelle ultime lezioni, inclusa la scrittura dei selettori e la stilizzazione del testo.

> [!NOTE]
> Puoi fare clic su "Play" nei campioni live qui sotto per aprire il codice nel MDN Playground, oppure puoi copiare e incollare il codice nel tuo IDE o in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/) o [Glitch](https://glitch.com/).
> Se ti blocchi, puoi contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Descrizione del progetto

Il seguente campione live mostra una biografia, che è stata stilizzata utilizzando CSS. Le proprietà CSS che sono utilizzate sono le seguenti — ciascuna collega alla pagina della proprietà su MDN, che ti darà più esempi sul suo utilizzo.

- {{cssxref("font-family")}}
- {{cssxref("color")}}
- {{cssxref("border-bottom")}}
- {{cssxref("font-weight")}}
- {{cssxref("font-size")}}
- {{cssxref("font-style")}}
- {{cssxref("text-decoration")}}

Nell'esempio, c'è già del CSS in atto che seleziona parti del documento utilizzando selettori di elementi, classi e pseudo-classi. Apporta le seguenti modifiche a questo CSS:

1. Rendi la intestazione di livello uno rosa, utilizzando la parola chiave di colore CSS `hotpink`.
2. Dai alla intestazione un {{cssxref("border-bottom")}} a puntini di 10px che utilizza la parola chiave di colore CSS `purple`.
3. Rendi la intestazione di livello 2 in corsivo.
4. Dai alla `ul` usata per i dettagli di contatto un {{cssxref("background-color")}} di `#eeeeee`, e un {{cssxref("border")}} solido viola di 5px. Usa un po' di {{cssxref("padding")}} per allontanare il contenuto dal bordo.
5. Rendi i link `green` al passaggio del mouse.

## Suggerimenti e consigli

- Usa il [W3C CSS Validator](https://jigsaw.w3.org/css-validator/) per individuare errori involontari nel tuo CSS — errori che altrimenti potresti aver trascurato — in modo da poterli correggere.
- Successivamente prova a cercare alcune proprietà non menzionate in questa pagina nel [riferimento CSS di MDN](/it/docs/Web/CSS/Reference) e sii intraprendente!
- Ricorda che non esiste una risposta sbagliata qui — a questo punto del tuo apprendimento puoi permetterti di divertirti un po'.

## Esempio

Dovresti arrivare a qualcosa di simile a questa immagine.

![Schermata di come l'esempio dovrebbe apparire dopo aver completato l'esercitazione.](learn-css-basics-assessment.png)

Ecco i blocchi di codice HTML e CSS e il risultato della loro combinazione:

```html live-sample___biog
<h1>Jane Doe</h1>
<div class="job-title">Web Developer</div>
<p>
  Far far away, behind the word mountains, far from the countries Vokalia and
  Consonantia, there live the blind texts. Separated they live in Bookmarksgrove
  right at the coast of the Semantics, a large language ocean.
</p>

<p>
  A small river named Duden flows by their place and supplies it with the
  necessary regelialia. It is a paradisematic country, in which roasted parts of
  sentences fly into your mouth.
</p>

<h2>Contact information</h2>
<ul>
  <li>Email: <a href="mailto:jane@example.com">jane@example.com</a></li>
  <li>Web: <a href="http://example.com">http://example.com</a></li>
  <li>Tel: 123 45678</li>
</ul>
```

```css live-sample___biog
body {
  font-family: Arial, Helvetica, sans-serif;
}

h1 {
  color: #375e97;
  font-size: 2em;
  font-family: Georgia, "Times New Roman", Times, serif;
  border-bottom: 1px solid #375e97;
}

h2 {
  font-size: 1.5em;
}

.job-title {
  color: #999999;
  font-weight: bold;
}

a:link,
a:visited {
  color: #fb6542;
}

a:hover {
  text-decoration: none;
}
```

{{EmbedLiveSample("biog", "", "400px")}}

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Getting_started", "Learn_web_development/Core/Styling_basics/Basic_selectors", "Learn_web_development/Core/Styling_basics")}}
