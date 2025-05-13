---
title: Combiners
slug: Learn_web_development/Core/Styling_basics/Combinators
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Pseudo_classes_and_elements", "Learn_web_development/Core/Styling_basics/Box_model", "Learn_web_development/Core/Styling_basics")}}

Gli ultimi selettori che esamineremo sono chiamati combiners. I combiners sono utilizzati per combinare altri selettori in un modo che ci consente di selezionare elementi in base alla loro posizione nel DOM rispetto ad altri elementi (ad esempio, figlio o fratello).

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Concetti base di HTML (studiare
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi HTML di base</a
        >), <a href="/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors">Selettori CSS di base</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Il concetto base dei combiners.</li>
          <li>Combiner discendenti e figli.</li>
          <li>Combiner di fratelli successivi e seguenti.</li>
          <li>Innestamento.</li>
          <li>Combinare combiners con selettori.</li>
        <ul>
      </td>
    </tr>
  </tbody>
</table>

## Combiner discendente

Il **combiner discendente** — tipicamente rappresentato da un singolo spazio (<code> </code>) — combina due selettori in modo tale che gli elementi corrispondenti al secondo selettore siano selezionati se hanno un antenato (genitore, genitore del genitore, genitore del genitore del genitore, ecc.) che corrisponde al primo selettore. I selettori che utilizzano un combiner discendente sono chiamati _selettori discendenti_.

```css
body article p {
}
```

Nell'esempio seguente, stiamo selezionando solo l'elemento `<p>` che si trova all'interno di un elemento con una classe di `.box`.

```html live-sample___descendant
<div class="box"><p>Text in .box</p></div>
<p>Text not in .box</p>
```

```css live-sample___descendant
.box p {
  color: red;
}
```

{{EmbedLiveSample("descendant")}}

## Combiner figlio

Il **combiner figlio** (`>`) è posizionato tra due selettori CSS. Seleziona solo quegli elementi corrispondenti al secondo selettore che sono figli diretti degli elementi corrispondenti al primo. Gli elementi discendenti più in basso nella gerarchia non corrispondono. Ad esempio, per selezionare solo gli elementi `<p>` che sono figli diretti degli elementi `<article>`:

```css
article > p {
  /* … */
}
```

In questo esempio successivo, abbiamo una lista ordinata ({{htmlelement("ol")}}) annidata all'interno di una lista non ordinata ({{htmlelement("ul")}}). Il combiner figlio seleziona solo quegli elementi `<li>` che sono figli diretti di un `<ul>`, e li stila con un bordo superiore.

Se rimuove il `>` che designa questo come combiner figlio, si ottiene un selettore discendente e tutti gli elementi `<li>` riceveranno un bordo rosso.

```html live-sample___child
<ul>
  <li>Unordered item</li>
  <li>
    Unordered item
    <ol>
      <li>Item 1</li>
      <li>Item 2</li>
    </ol>
  </li>
</ul>
```

```css live-sample___child
ul > li {
  border-top: 5px solid red;
}
```

{{EmbedLiveSample("child")}}

## Combiner di fratello successivo

Il **combiner di fratello successivo** (`+`) è posizionato tra due selettori CSS. Seleziona solo quegli elementi corrispondenti al secondo selettore che si trovano immediatamente dopo l'elemento corrispondente al primo selettore. Ad esempio, per selezionare tutti gli elementi `<img>` che sono immediatamente preceduti da un elemento `<p>`:

```css
p + img {
  /* … */
}
```

Un caso d'uso comune è fare qualcosa con un paragrafo che segue un'intestazione, come nell'esempio seguente. In quell'esempio, stiamo cercando qualsiasi paragrafo che condivida un elemento genitore con un `<h1>`, e segua immediatamente quel `<h1>`.

Se inserisce qualche altro elemento come un `<h2>` tra l'`<h1>` e il `<p>`, troverà che il paragrafo non è più corrispondente al selettore e quindi non ottiene il colore di sfondo e di primo piano applicato quando l'elemento è adiacente.

```html live-sample___adjacent
<article>
  <h1>A heading</h1>
  <p>
    Veggies es bonus vobis, proinde vos postulo essum magis kohlrabi welsh onion
    daikon amaranth tatsoi tomatillo melon azuki bean garlic.
  </p>

  <p>
    Gumbo beet greens corn soko endive gumbo gourd. Parsley shallot courgette
    tatsoi pea sprouts fava bean collard greens dandelion okra wakame tomato.
    Dandelion cucumber earthnut pea peanut soko zucchini.
  </p>
</article>
```

```css live-sample___adjacent
body {
  font-family: sans-serif;
}

h1 + p {
  font-weight: bold;
  background-color: #333;
  color: #fff;
  padding: 0.5em;
}
```

{{EmbedLiveSample("adjacent", "", "220px")}}

## Combiner di fratelli seguenti

Se desidera selezionare fratelli di un elemento anche se non sono direttamente adiacenti, può utilizzare il **combiner di fratelli seguenti** (`~`). Per selezionare tutti gli elementi `<img>` che si trovano _ovunque_ dopo gli elementi `<p>`, facciamo questo:

```css
p ~ img {
  /* … */
}
```

Nell'esempio seguente, stiamo selezionando tutti gli elementi `<p>` che si trovano dopo l'`<h1>`, e anche se c'è un `<div>` nel documento, il `<p>` che si trova dopo di esso viene selezionato.

```html live-sample___general
<article>
  <h1>A heading</h1>
  <p>I am a paragraph.</p>
  <div>I am a div</div>
  <p>I am another paragraph.</p>
</article>
```

```css live-sample___general
body {
  font-family: sans-serif;
}

h1 ~ p {
  font-weight: bold;
  background-color: #333;
  color: #fff;
  padding: 0.5em;
}
```

{{EmbedLiveSample("general", "", "220px")}}

## Creare selettori complessi con l'innestamento

Il [modulo di innestamento CSS](/it/docs/Web/CSS/CSS_nesting/Using_CSS_nesting#combinators) le consente di scrivere regole annidate che utilizzano combiners per creare [selettori complessi](/it/docs/Web/CSS/CSS_selectors/Selector_structure#complex_selector).

```css
p {
  ~ img {
  }
}
/* This is parsed by the browser as */
p ~ img {
}
```

Il [`&` nesting selector](/it/docs/Web/CSS/Nesting_selector) può anche essere utilizzato per creare selettori complessi:

```css
p {
  & img {
  }
}
/* This is parsed by the browser as */
p img {
}
```

Ecco un esempio che dimostra selettori complessi:

```html live-sample___nesting
<article>
  <h1>A heading</h1>
  <p>I am a paragraph.</p>
  <div>I am a div</div>
  <p>I am another paragraph.</p>
</article>
```

```css live-sample___nesting
body {
  font-family: sans-serif;
}

h1 {
  & ~ p {
    /* this is parsed by the browser as h1 ~ p */
    font-weight: bold;
    background-color: #333;
    color: #fff;
    padding: 0.5em;
  }
}
```

{{EmbedLiveSample("nesting", "", "220px")}}

> [!NOTE]
> Nell'esempio sopra, il selettore di innestamento `&` non è richiesto, ma aggiungerlo aiuta a mostrare esplicitamente che si sta utilizzando l'innestamento CSS.

## Combinare combiners con selettori

È possibile combinare qualsiasi dei selettori che abbiamo scoperto nelle lezioni precedenti con i combiners per selezionare parte del suo documento. Ad esempio, per selezionare gli elementi di lista con una `class` di `a` che sono figli diretti di un `<ul>`, provi il seguente:

```css
ul > li[class="a"] {
}
```

Faccia attenzione, tuttavia, quando crea lunghe liste di selettori che selezionano parti molto specifiche del suo documento. Sarà difficile riutilizzare le regole CSS poiché ha reso il selettore molto specifico per la posizione di quell'elemento nel markup.

È spesso meglio creare una semplice classe e applicarla all'elemento in questione. Detto ciò, la sua conoscenza dei combiners sarà molto utile se ha bisogno di stilizzare qualcosa nel suo documento e non può accedere all'HTML, forse perché è generato da un {{Glossary("CMS", "CMS")}}.

## Metta alla prova le sue abilità!

Ha raggiunto la fine del nostro set di lezioni sui selettori, ma ricorda le informazioni più importanti? Può trovare ulteriori test per verificare di aver mantenuto queste informazioni prima di passare oltre — veda [Metta alla prova le sue abilità: Selettori](/it/docs/Learn_web_development/Core/Styling_basics/Test_your_skills/Selectors).

## Riepilogo

Questo è tutto per i selettori, per ora. Successivamente, ci sposteremo su un'altra parte importante del CSS — il modello di riquadro.

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Pseudo_classes_and_elements", "Learn_web_development/Core/Styling_basics/Box_model", "Learn_web_development/Core/Styling_basics")}}
