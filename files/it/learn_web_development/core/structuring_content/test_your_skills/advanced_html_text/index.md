---
title: "Metti alla prova le tue competenze: Testo HTML avanzato"
short-title: "Test: Testo HTML avanzato"
slug: Learn_web_development/Core/Structuring_content/Test_your_skills/Advanced_HTML_text
l10n:
  sourceCommit: 1cf3cb0fb22bf89c780fefe74c3db7f1b9e8ca09
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Advanced_text_features", "Learn_web_development/Core/Structuring_content/Marking_up_a_letter", "Learn_web_development/Core/Structuring_content")}}

Lo scopo di questo test delle competenze è aiutare a valutare se si comprende come utilizzare [elementi HTML meno noti per contrassegnare caratteristiche semantiche avanzate](/it/docs/Learn_web_development/Core/Structuring_content/Advanced_text_features).

> [!NOTE]
> Per ottenere aiuto, leggere la nostra guida all'uso di [Metti alla prova le tue competenze](/it/docs/Learn_web_development#test_your_skills). È inoltre possibile contattarci utilizzando uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Testo avanzato 1

In questa attività, si desidera aggiungere della semantica all'HTML fornito.

Per completare questa attività:

1. Trasformare il secondo paragrafo in una citazione a livello di blocco e indicare semanticamente che la citazione è tratta da [Accessibilità](/it/docs/Learn_web_development/Core/Accessibility).
2. Contrassegnare semanticamente "HTML" e "CSS" come acronimi, fornendo le espansioni come tooltip.
3. Utilizzare pedice e apice per fornire la semantica corretta alle formule chimiche e alle date, e visualizzarle correttamente.
4. Associare semanticamente date leggibili dalle macchine alle date nel testo.

Il punto di partenza dell'attività è simile a questo:

{{ EmbedLiveSample('advanced-text', "100%", 260) }}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___advanced-text
<h1>Advanced text semantics</h1>

<p>Let's start with a quote:</p>

<p>
  HTML, Hypertext Markup Language is by default accessible, if used correctly.
</p>

<p>CSS can also be used to make web pages more, or less, accessible.</p>

<p>Chemical Formulae: H2O (Water), C2H6O (Ethanol).</p>

<p>
  Dates: December 25th 2019 (Christmas Day), November 2nd 2019 (Día de los
  Muertos).
</p>
```

```css hidden live-sample___advanced-text live-sample___advanced-text-solution
body {
  background-color: white;
  color: #333333;
  font:
    1em / 1.4 "Helvetica Neue",
    "Helvetica",
    "Arial",
    sans-serif;
  padding: 1em;
  margin: 0;
}

h1 {
  font-size: 2rem;
  margin: 0;
  color: purple;
}

p {
  margin: 0.5em 0;
}

abbr,
time {
  color: green;
}
```

Il contenuto aggiornato dovrebbe apparire così:

{{EmbedLiveSample('advanced-text-solution', "", 260)}}

<details>
<summary>Fare clic qui per visualizzare la soluzione</summary>

L'HTML finale dovrebbe apparire così:

```html live-sample___advanced-text-solution
<h1>Advanced text semantics</h1>

<p>Let's start with a quote:</p>

<blockquote cite="https://developer.mozilla.org/en-US/docs/Learn/Accessibility">
  <p>
    <abbr title="HyperText Markup Language">HTML</abbr>, Hypertext Markup
    Language is by default accessible, if used correctly.
  </p>
</blockquote>

<p>
  <abbr title="Cascading Style Sheets">CSS</abbr>, Cascading Style Sheets, can
  also be used to make web pages more, or less, accessible.
</p>

<p>
  Chemical Formulae: H<sub>2</sub>O (Water), C<sub>2</sub>H<sub>6</sub>O
  (Ethanol).
</p>

<p>
  Dates:
  <time datetime="2019-12-25">December 25<sup>th</sup> 2019</time>
  (Christmas Day),
  <time datetime="2019-11-02">November 2<sup>nd</sup> 2019</time> (Día de los
  Muertos).
</p>
```

</details>

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Advanced_text_features", "Learn_web_development/Core/Structuring_content/Marking_up_a_letter", "Learn_web_development/Core/Structuring_content")}}
