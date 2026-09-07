---
title: Combinatori
slug: Learn_web_development/Core/Styling_basics/Combinators
l10n:
  sourceCommit: 57bc2729e3963907c0b54158ae1a31318a2ebbd1
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Pseudo_classes_and_elements", "Learn_web_development/Core/Styling_basics/Test_your_skills/Selectors", "Learn_web_development/Core/Styling_basics")}}

Gli ultimi selettori che esamineremo sono chiamati combinatori. I combinatori vengono utilizzati per combinare altri selettori in modo da consentire la selezione di elementi in base alla loro posizione nel DOM rispetto ad altri elementi (ad esempio, figlio o fratello).

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Nozioni di base di HTML (studiare
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi HTML di base</a
        >), <a href="/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors">Selettori CSS di base</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Il concetto di base dei combinatori.</li>
          <li>Combinatori discendente e figlio.</li>
          <li>Combinatori fratello successivo e fratello successivo generale.</li>
          <li>Annidamento.</li>
          <li>Combinazione di combinatori con i selettori.</li>
        <ul>
      </td>
    </tr>
  </tbody>
</table>

## Combinatore discendente

Il **combinatore discendente** — rappresentato da un singolo carattere spazio (<code> </code>) — combina due selettori in modo tale che gli elementi corrispondenti al secondo selettore vengano selezionati se hanno un elemento antenato (un genitore, il genitore di un genitore, il genitore del genitore di un genitore e così via) corrispondente al primo selettore. I selettori che utilizzano un combinatore discendente sono chiamati _selettori discendenti_.

```css
body article p {
}
```

Nell'esempio seguente, viene associato solo l'elemento `<p>` che si trova all'interno di un elemento con classe `.box`.

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

> [!NOTE]
> [Approfondimento: selettori composti](https://scrimba.com/frontend-path-c0j/~0br?via=mdn) <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> di Scrimba è una lezione interattiva che offre un approccio pratico ai combinatori discendenti.

## Combinatore figlio

Il **combinatore figlio** (`>`) viene inserito tra due selettori CSS. Corrisponde solo agli elementi corrispondenti al secondo selettore che sono figli diretti degli elementi corrispondenti al primo. Gli elementi discendenti più in basso nella gerarchia non corrispondono. Ad esempio, per selezionare solo gli elementi `<p>` che sono figli diretti di elementi `<article>`:

```css
article > p {
  /* … */
}
```

In questo esempio successivo, è presente un elenco ordinato ({{htmlelement("ol")}}) annidato all'interno di un elenco non ordinato ({{htmlelement("ul")}}). Il combinatore figlio seleziona solo gli elementi `<li>` che sono figli diretti di un `<ul>` e applica loro uno stile con un bordo superiore.

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

Nell'esempio precedente, provare a rimuovere il `>` che identifica il selettore come selettore figlio. Si otterrà un selettore discendente e tutti gli elementi `<li>` riceveranno un bordo rosso.

## Combinatore fratello successivo

Il **combinatore fratello successivo** (`+`) viene inserito tra due selettori CSS. Corrisponde solo agli elementi corrispondenti al secondo selettore che si trovano immediatamente dopo l'elemento corrispondente al primo selettore. Ad esempio, per selezionare tutti gli elementi `<img>` immediatamente preceduti da un elemento `<p>`:

```css
p + img {
  /* … */
}
```

Un caso d'uso comune consiste nell'applicare qualcosa a un paragrafo che segue un'intestazione, come nell'esempio seguente. Qui viene selezionato qualsiasi paragrafo che condivide un elemento genitore con un `<h1>` e che segue immediatamente tale `<h1>`.

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
  background-color: #333333;
  color: white;
  padding: 0.5em;
}
```

{{EmbedLiveSample("adjacent", "", "220px")}}

Nell'esempio precedente:

1. Provare a inserire un altro elemento, ad esempio un `<h2>`, tra l'`<h1>` e il `<p>`. Il paragrafo non corrisponderà più al selettore e quindi non riceverà i colori di sfondo e primo piano applicati quando l'elemento è adiacente.
2. Ora modificare il selettore `h1 + p` in modo che lo stile speciale venga applicato nuovamente al primo paragrafo.

## Combinatore fratello successivo generale

Se si desidera selezionare i fratelli di un elemento anche se non sono direttamente adiacenti, è possibile utilizzare il **combinatore fratello successivo generale** (`~`). Per selezionare tutti gli elementi `<img>` che si trovano _in qualsiasi punto_ dopo elementi `<p>`, si procederebbe così:

```css
p ~ img {
  /* … */
}
```

Nell'esempio seguente vengono selezionati tutti gli elementi `<p>` che si trovano dopo l'`<h1>` e, anche se nel documento è presente anche un `<div>`, viene selezionato il `<p>` che lo segue.

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
  background-color: #333333;
  color: white;
  padding: 0.5em;
}
```

{{EmbedLiveSample("general", "", "220px")}}

## Combinazione di combinatori con i selettori

È possibile combinare qualsiasi selettore scoperto nelle lezioni precedenti con i combinatori, per selezionare una parte del documento. Ad esempio, per selezionare elementi di elenco con una `class` pari a `a` che sono figli diretti di un `<ul>`, provare quanto segue:

```css
ul > li[class="a"] {
}
```

Prestare attenzione, tuttavia, quando si creano lunghi elenchi di selettori che selezionano parti molto specifiche del documento. Sarà difficile riutilizzare le regole CSS, poiché il selettore è stato reso molto specifico per la posizione di quell'elemento nel markup.

Spesso è preferibile creare una classe semplice e applicarla all'elemento in questione. Detto questo, la conoscenza dei combinatori sarà molto utile quando occorre applicare uno stile a qualcosa nel documento e non è possibile accedere all'HTML, magari perché viene generato da un {{Glossary("CMS", "CMS")}}.

## Riepilogo

Per ora è tutto sui selettori. Successivamente, verranno proposti alcuni test che consentono di verificare quanto bene siano state comprese e memorizzate le informazioni fornite sui selettori CSS.

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Pseudo_classes_and_elements", "Learn_web_development/Core/Styling_basics/Test_your_skills/Selectors", "Learn_web_development/Core/Styling_basics")}}
