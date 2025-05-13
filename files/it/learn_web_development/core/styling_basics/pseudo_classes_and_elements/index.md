---
title: Pseudoclassi e pseudo-elementi
short-title: Pseudoclassi ed elementi
slug: Learn_web_development/Core/Styling_basics/Pseudo_classes_and_elements
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Attribute_selectors", "Learn_web_development/Core/Styling_basics/Combinators", "Learn_web_development/Core/Styling_basics")}}

Il prossimo set di selettori che esamineremo è denominato **pseudoclassi** e **pseudo-elementi**. Ce ne sono un gran numero, e spesso servono a scopi assai specifici. Una volta che sa come utilizzarli, può esaminare i diversi tipi per vedere se c'è qualcosa che funziona per il compito che sta cercando di realizzare.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Nozioni di base su HTML (studi
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi di base HTML</a
        >), <a href="/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors">Selettori CSS di base</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Pseudoclassi e pseudo-elementi.</li>
          <li>La differenza tra i due.</li>
          <li>Combinare pseudoclassi e pseudo-elementi.</li>
          <li>Contenuto generato.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Che cos'è una pseudoclasse?

Una pseudoclasse è un selettore che seleziona elementi che sono in uno stato specifico, per esempio, sono il primo elemento del loro tipo, oppure sono in corso di interazione del puntatore del mouse. Tendono ad agire come se lei avesse applicato una classe a una parte del suo documento, spesso aiutandola a ridurre il numero eccessivo di classi nel suo markup, e offrendole un codice più flessibile e manutenibile.

Le pseudoclassi sono parole chiave che iniziano con un due punti. Ad esempio, `:hover` è una pseudoclasse.

### Esempio base di pseudoclasse

Diamo un'occhiata a un esempio di base. Se volessimo rendere il primo paragrafo in un articolo più grande e in grassetto, potremmo aggiungere una classe a quel paragrafo e poi aggiungere CSS a quella classe, come mostrato nel primo esempio qui sotto:

```html live-sample___first-child
<article>
  <p class="first">
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

```css live-sample___first-child
.first {
  font-size: 120%;
  font-weight: bold;
}
```

{{EmbedLiveSample("first-child")}}

Tuttavia, questo potrebbe essere fastidioso da mantenere — e se venisse aggiunto un nuovo paragrafo all'inizio del documento? Dovremmo spostare la classe sul nuovo paragrafo. Invece di aggiungere la classe, potremmo usare il selettore pseudoclasse {{cssxref(":first-child")}} — questo si rivolgerà _sempre_ al primo elemento figlio nell'articolo, e non avremo più bisogno di modificare l'HTML (questo potrebbe comunque non essere sempre possibile, magari perché generato da un CMS).

```html live-sample___first-child2
<article>
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

```css live-sample___first-child2
article p:first-child {
  font-size: 120%;
  font-weight: bold;
}
```

{{EmbedLiveSample("first-child2")}}

Tutte le pseudoclassi si comportano in questo stesso modo. Targettizzano una parte del suo documento che è in un determinato stato, comportandosi come se avesse aggiunto una classe nel suo HTML. Dia un'occhiata ad alcuni altri esempi su MDN:

- [`:last-child`](/it/docs/Web/CSS/:last-child)
- [`:only-child`](/it/docs/Web/CSS/:only-child)
- [`:invalid`](/it/docs/Web/CSS/:invalid)

> [!NOTE]
> È valido scrivere pseudoclassi ed elementi senza alcun selettore di elementi che li precede. Nell'esempio sopra, potrebbe scrivere `:first-child` e la regola si applicherebbe a _qualsiasi_ elemento che è il primo figlio di un elemento `<article>`, non solo un paragrafo primo figlio — `:first-child` è equivalente a `*:first-child`. Tuttavia, di solito si vuole più controllo di così, quindi è necessario essere più specifici.

### Pseudoclassi di azione dell'utente

Alcune pseudoclassi si applicano solo quando l'utente interagisce con il documento in qualche modo. Queste **pseudoclassi di azione dell'utente**, talvolta chiamate **pseudoclassi dinamiche**, agiscono come se una classe fosse stata aggiunta all'elemento quando l'utente interagisce con esso. Gli esempi includono:

- [`:hover`](/it/docs/Web/CSS/:hover) — menzionata sopra; questa si applica solo se l'utente sposta il suo puntatore su un elemento, tipicamente un link.
- [`:focus`](/it/docs/Web/CSS/:focus) — si applica solo se l'utente focalizza l'elemento cliccando o utilizzando i controlli della tastiera.

```html live-sample___hover
<p><a href="">Hover over me</a></p>
```

```css live-sample___hover
a:link,
a:visited {
  color: rebeccapurple;
  font-weight: bold;
}

a:hover {
  color: hotpink;
}
```

{{EmbedLiveSample("hover")}}

## Che cos'è un pseudo-elemento?

Gli pseudo-elementi si comportano in modo simile. Tuttavia, agiscono come se le avesse aggiunto un nuovo elemento HTML nel markup, piuttosto che applicare una classe a elementi esistenti.

Gli pseudo-elementi iniziano con un doppio due punti `::`. `::before` è un esempio di pseudo-elemento.

> [!NOTE]
> Alcuni primi pseudo-elementi usavano la sintassi a un solo due punti, quindi potrebbe vederlo a volte nel codice o negli esempi. I moderni browser supportano i primi pseudo-elementi con la sintassi a uno o due punti per la compatibilità con le versioni precedenti.

Per esempio, se volesse selezionare la prima riga di un paragrafo potrebbe racchiuderla in un elemento `<span>` e usare un selettore di elementi; tuttavia, ciò fallirebbe se il numero di parole che ha racchiuso fosse più lungo o più corto della larghezza dell'elemento genitore. Poiché tendiamo a non sapere quante parole si adatteranno a una riga — poiché ciò cambierà se la larghezza dello schermo o la dimensione del carattere cambia — è impossibile fare questo in modo robusto aggiungendo HTML.

Il selettore pseudo-elemento `::first-line` lo farà per lei in modo affidabile — se il numero di parole aumenta o diminuisce selezionerà comunque solo la prima riga.

```html live-sample___first-line
<article>
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

```css live-sample___first-line
article p::first-line {
  font-size: 120%;
  font-weight: bold;
}
```

{{EmbedLiveSample("first-line")}}

Si comporta come se un `<span>` fosse magicamente avvolto attorno a quella prima riga formattata, e aggiornato ogni volta che la lunghezza della riga cambiava.

Può vedere che questo seleziona la prima riga di entrambi i paragrafi.

## Combinare pseudoclassi e pseudo-elementi

Se volesse rendere in grassetto la prima riga del primo paragrafo, potrebbe concatenare insieme i selettori `:first-child` e `::first-line`. Provi a modificare il precedente esempio live in modo che utilizzi il seguente CSS. Stiamo dicendo che vogliamo selezionare la prima riga, del primo elemento `<p>`, che è all'interno di un elemento `<article>`.

```css
article p:first-child::first-line {
  font-size: 120%;
  font-weight: bold;
}
```

## Generare contenuto con ::before e ::after

Ci sono un paio di pseudo-elementi speciali, che vengono utilizzati insieme alla proprietà [`content`](/it/docs/Web/CSS/content) per inserire contenuto nel suo documento usando CSS.

Potrebbe usarli per inserire una stringa di testo, come nell'esempio live qui sotto. Provi a cambiare il valore del testo della proprietà {{cssxref("content")}} e veda come cambia nel risultato. Potrebbe anche cambiare il pseudo-elemento `::before` in `::after` e vedere il testo inserito alla fine dell'elemento invece che all'inizio.

```html live-sample___before
<p class="box">Content in the box in my HTML page.</p>
```

```css live-sample___before
.box::before {
  content: "This should show before the other content. ";
}
```

{{EmbedLiveSample("before")}}

Inserire stringhe di testo da CSS non è davvero qualcosa che facciamo molto spesso sul web, tuttavia, poiché quel testo è inaccessibile a alcuni screen reader e potrebbe essere difficile per qualcuno trovarlo e modificarlo in futuro.

Un uso più valido di questi pseudo-elementi è quello di inserire un'icona, ad esempio la piccola freccia aggiunta nell'esempio sotto, che è un indicatore visivo che non vorremmo che fosse letto da un screen reader:

```html live-sample___after-icon
<p class="box">Content in the box in my HTML page.</p>
```

```css live-sample___after-icon
.box::after {
  content: " ➥";
}
```

{{EmbedLiveSample("after-icon")}}

Questi pseudo-elementi sono anche frequentemente utilizzati per inserire una stringa vuota, che può poi essere stilizzata proprio come qualsiasi elemento sulla pagina.

In questo prossimo esempio, abbiamo aggiunto una stringa vuota usando il pseudo-elemento `::before`. L'abbiamo impostata su `display: block` in modo da poterla stilizzare con una larghezza e un'altezza. Poi usiamo CSS per stilizzarla proprio come qualsiasi elemento. Può giocare con il CSS e cambiare il suo aspetto e comportamento.

```html live-sample___before-styled
<p class="box">Content in the box in my HTML page.</p>
```

```css live-sample___before-styled
.box::before {
  content: "";
  display: block;
  width: 100px;
  height: 100px;
  background-color: rebeccapurple;
  border: 1px solid black;
}
```

{{EmbedLiveSample("before-styled", "", "160")}}

L'uso dei pseudo-elementi `::before` e `::after` insieme alla proprietà `content` è definito "Contenuto Generato" in CSS, e vedrà spesso questa tecnica utilizzata per vari compiti. Un ottimo esempio è il sito [CSS Arrow Please](https://cssarrowplease.com/), che la aiuta a generare una freccia con CSS. Guardi il CSS mentre crea la sua freccia e vedrà i pseudo-elementi {{cssxref("::before")}} e {{cssxref("::after")}} in uso. Ogni volta che vede questi selettori, guardi la proprietà {{cssxref("content")}} per vedere cosa viene aggiunto all'elemento HTML.

## Sommario

In questo articolo abbiamo introdotto le pseudoclassi e i pseudo-elementi CSS, che sono tipi speciali di selettori.

Le pseudoclassi le permettono di targettizzare un elemento quando è in un particolare stato, come se avesse aggiunto una classe per quello stato al DOM. Gli pseudo-elementi agiscono come se avesse aggiunto un intero nuovo elemento al DOM, e le permettono di stilizzarlo. Gli pseudo-elementi `::before` e `::after` le permettono di inserire contenuto nel documento usando CSS.

Nel prossimo articolo, impareremo i combinatori.

## Vedi anche

- [Riferimento alle pseudoclassi](/it/docs/Web/CSS/Pseudo-classes)
- [Riferimento ai pseudo-elementi](/it/docs/Web/CSS/Pseudo-elements)

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Attribute_selectors", "Learn_web_development/Core/Styling_basics/Combinators", "Learn_web_development/Core/Styling_basics")}}
