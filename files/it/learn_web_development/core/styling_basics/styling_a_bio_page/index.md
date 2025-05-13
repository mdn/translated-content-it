---
title: "Sfida: Stile per una pagina biografica"
short-title: "Sfida: Pagina biografica"
slug: Learn_web_development/Core/Styling_basics/Styling_a_bio_page
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Getting_started", "Learn_web_development/Core/Styling_basics/Basic_selectors", "Learn_web_development/Core/Styling_basics")}}

In questa sfida stilizzerà una semplice pagina biografica, mettendo alla prova alcune delle competenze apprese nelle ultime lezioni, inclusi la scrittura di selettori e lo stile del testo.

> [!NOTE]
> Può cliccare su "Play" nei campioni dal vivo qui sotto per aprire il codice nel MDN Playground, oppure può copiare e incollare il codice nel proprio IDE o in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/) o [Glitch](https://glitch.com/).
> Se si trova in difficoltà, può contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Descrizione del progetto

Il seguente esempio dal vivo mostra una biografia, che è stata stilizzata utilizzando CSS. Le proprietà CSS utilizzate sono le seguenti — ciascuna collega alla sua pagina delle proprietà su MDN, che fornirà ulteriori esempi del suo utilizzo.

- {{cssxref("font-family")}}
- {{cssxref("color")}}
- {{cssxref("border-bottom")}}
- {{cssxref("font-weight")}}
- {{cssxref("font-size")}}
- {{cssxref("font-style")}}
- {{cssxref("text-decoration")}}

Nell'esempio, c'è già del CSS in atto che seleziona parti del documento utilizzando selettori di elementi, classi e pseudo-classi. Apportare le seguenti modifiche a questo CSS:

1. Rendi il titolo di livello uno rosa, utilizzando la parola chiave del colore CSS `hotpink`.
2. Dai al titolo un {{cssxref("border-bottom")}} tratteggiato di 10px che utilizza la parola chiave del colore CSS `purple`.
3. Fai il titolo di livello 2 in corsivo.
4. Dai al `ul` utilizzato per i dettagli di contatto un {{cssxref("background-color")}} di `#eeeeee`, e un {{cssxref("border")}} solido viola di 5px. Usa un po' di {{cssxref("padding")}} per spingere il contenuto lontano dal bordo.
5. Rendi i link `green` quando sono in hover.

## Suggerimenti e consigli

- Utilizzare il [W3C CSS Validator](https://jigsaw.w3.org/css-validator/) per rilevare errori involontari nel suo CSS — errori che avrebbe potuto altrimenti perdere — in modo da poterli correggere.
- Successivamente, provi a cercare alcune proprietà non menzionate in questa pagina nella [riferimento CSS MDN](/it/docs/Web/CSS/Reference) e si avventuri!
- Ricordi che non c'è una risposta sbagliata qui — a questo punto del suo apprendimento può permettersi di divertirsi un po'.

## Esempio

Dovrebbe ottenere qualcosa simile a questa immagine.

![Screenshot di come l'esempio dovrebbe apparire dopo aver completato la valutazione.](learn-css-basics-assessment.png)

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
