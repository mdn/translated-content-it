---
title: Combinatori
slug: Learn_web_development/Core/Styling_basics/Combinators
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Pseudo_classes_and_elements", "Learn_web_development/Core/Styling_basics/Box_model", "Learn_web_development/Core/Styling_basics")}}

Gli ultimi selettori che esamineremo sono chiamati combinatori. I combinatori vengono utilizzati per combinare altri selettori in modo che ci permettano di selezionare elementi basati sulla loro posizione nel DOM rispetto ad altri elementi (ad esempio, figlio o fratello).

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Nozioni di base su HTML (studiare la
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi di base di HTML</a
        >), <a href="/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors">Selettori CSS di base</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Il concetto di base dei combinatori.</li>
          <li>Combinatori discendenti e di figlio.</li>
          <li>Combinatori di fratello successivo e ulteriore.</li>
          <li>Nidificazione.</li>
          <li>Combinazione di combinatori con selettori.</li>
        <ul>
      </td>
    </tr>
  </tbody>
</table>

## Combinatore discendente

Il **combinatore discendente** — tipicamente rappresentato da uno spazio singolo (<code> </code>) — combina due selettori in modo che gli elementi corrispondenti al secondo selettore vengano selezionati se hanno un antenato (genitore, genitore del genitore, ecc.) che corrisponde al primo selettore. I selettori che utilizzano un combinatore discendente sono chiamati _selettori discendenti_.

```css
body article p {
}
```

Nel seguente esempio, stiamo selezionando solo l'elemento `<p>` che si trova all'interno di un elemento con la classe `.box`.

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

## Combinatore di figlio

Il **combinatore di figlio** (`>`) si posiziona tra due selettori CSS. Seleziona solo quegli elementi corrispondenti al secondo selettore che sono figli diretti degli elementi corrispondenti al primo. Gli elementi discendenti più in basso nella gerarchia non vengono selezionati. Ad esempio, per selezionare solo gli elementi `<p>` che sono figli diretti delle `<article>`:

```css
article > p {
  /* … */
}
```

In questo esempio successivo, abbiamo un elenco ordinato ({{htmlelement("ol")}}) nidificato all'interno di un elenco non ordinato ({{htmlelement("ul")}}). Il combinatore di figlio seleziona solo quegli elementi `<li>` che sono figli diretti di un `<ul>`, e li stila con un bordo superiore.

Se rimuovi il `>` che designa questo come un combinatore di figlio, ottieni un selettore discendente e tutti gli elementi `<li>` riceveranno un bordo rosso.

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

## Combinatore di fratello successivo

Il **combinatore di fratello successivo** (`+`) si posiziona tra due selettori CSS. Seleziona solo quegli elementi corrispondenti al secondo selettore che vengono subito dopo l'elemento corrispondente al primo selettore. Ad esempio, per selezionare tutti gli `<img>` che sono immediatamente preceduti da un elemento `<p>`:

```css
p + img {
  /* … */
}
```

Un caso d'uso comune è fare qualcosa con un paragrafo che segue un titolo, come nell'esempio sottostante. In quell'esempio, stiamo cercando qualsiasi paragrafo che condivida un elemento genitore con un `<h1>`, e segua immediatamente quell'`<h1>`.

Se inserisci qualche altro elemento come un `<h2>` tra l'`<h1>` e il `<p>`, scoprirai che il paragrafo non è più selezionato dal selettore e quindi non ottiene il colore di sfondo e primo piano quando l'elemento è adiacente.

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

## Combinatore di fratello ulteriore

Se desideri selezionare fratelli di un elemento anche se non sono direttamente adiacenti, puoi utilizzare il **combinatore di fratello ulteriore** (`~`). Per selezionare tutte le `<img>` che vengono _ovunque_ dopo le `<p>`, faremmo così:

```css
p ~ img {
  /* … */
}
```

Nell'esempio di seguito selezioniamo tutti gli `<p>` che vengono dopo l'`<h1>`, e anche se c'è un `<div>` nel documento, il `<p>` che viene dopo di esso è selezionato.

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

## Creare selettori complessi con la nidificazione

Il [modulo di nidificazione CSS](/it/docs/Web/CSS/CSS_nesting/Using_CSS_nesting#combinators) consente di scrivere regole nidificate che utilizzano combinatori per creare [selettori complessi](/it/docs/Web/CSS/CSS_selectors/Selector_structure#complex_selector).

```css
p {
  ~ img {
  }
}
/* This is parsed by the browser as */
p ~ img {
}
```

Il [selettore di nidificazione `&`](/it/docs/Web/CSS/Nesting_selector) può anche essere utilizzato per creare selettori complessi:

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
> Nell'esempio sopra, il selettore di nidificazione `&` non è necessario, ma aggiungerlo aiuta a mostrare esplicitamente che si sta utilizzando la nidificazione CSS.

## Combinare combinatori con selettori

È possibile combinare qualsiasi dei selettori che abbiamo scoperto nelle lezioni precedenti con i combinatori per selezionare parte del tuo documento. Ad esempio, per selezionare gli elementi di elenco con una `classe` di `a` che sono figli diretti di un `<ul>`, prova il seguente:

```css
ul > li[class="a"] {
}
```

Fai attenzione, tuttavia, quando crei elenchi di selettori che selezionano parti molto specifiche del tuo documento. Sarà difficile riutilizzare le regole CSS poiché hai reso il selettore molto specifico per la posizione di quell'elemento nel markup.

È spesso meglio creare una classe semplice e applicarla all'elemento in questione. Detto ciò, la tua conoscenza dei combinatori sarà molto utile se hai bisogno di stilizzare qualcosa nel tuo documento e non puoi accedere all'HTML, forse perché è generato da un {{Glossary("CMS", "CMS")}}.

## Metti alla prova le tue abilità!

Hai raggiunto la fine del nostro set di lezioni sui selettori, ma riesci a ricordare le informazioni più importanti? Puoi trovare ulteriori test per verificare di aver trattenuto queste informazioni prima di andare avanti — vedi [Metti alla prova le tue abilità: Selettori](/it/docs/Learn_web_development/Core/Styling_basics/Test_your_skills/Selectors).

## Sommario

Questo è tutto per i selettori, per ora. Successivamente, passeremo a un'altra parte importante di CSS — il modello a scatola.

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Pseudo_classes_and_elements", "Learn_web_development/Core/Styling_basics/Box_model", "Learn_web_development/Core/Styling_basics")}}
