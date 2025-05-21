---
title: Pseudo-classi e pseudo-elementi
short-title: Pseudo-classi ed elementi
slug: Learn_web_development/Core/Styling_basics/Pseudo_classes_and_elements
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Attribute_selectors", "Learn_web_development/Core/Styling_basics/Combinators", "Learn_web_development/Core/Styling_basics")}}

Il prossimo set di selettori che esamineremo è riferito come **pseudo-classi** e **pseudo-elementi**. Ce ne sono un gran numero, e spesso servono scopi molto specifici. Una volta che sai come usarli, puoi esaminare i diversi tipi per vedere se c'è qualcosa che funziona per il compito che stai cercando di raggiungere.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Fondamenti di HTML (studiare la
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi di base dell'HTML</a
        >), <a href="/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors">Selettori di base CSS</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati attesi:</th>
      <td>
        <ul>
          <li>Pseudo-classi e pseudo-elementi.</li>
          <li>La differenza tra i due.</li>
          <li>Combinare pseudo-classi e pseudo-elementi.</li>
          <li>Contenuto generato.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Cos'è una pseudo-classe?

Una pseudo-classe è un selettore che seleziona elementi che si trovano in uno stato specifico, ad esempio, sono il primo elemento del loro tipo, o sono sotto l'hover del puntatore del mouse. Tendono ad agire come se avessi applicato una classe a una parte del tuo documento, spesso aiutandoti a ridurre le classi in eccesso nel tuo markup, e a fornirti un codice più flessibile e manutenibile.

Le pseudo-classi sono parole chiave che iniziano con un due punti. Ad esempio, `:hover` è una pseudo-classe.

### Esempio di pseudo-classe di base

Esaminiamo un esempio di base. Se volessimo rendere il primo paragrafo di un articolo più grande e in grassetto, potremmo aggiungere una classe a quel paragrafo e poi aggiungere CSS a quella classe, come mostrato nel primo esempio qui sotto:

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

Tuttavia, questo potrebbe essere fastidioso da mantenere — cosa succede se viene aggiunto un nuovo paragrafo in cima al documento? Dovremmo spostare la classe sul nuovo paragrafo. Invece di aggiungere la classe, potremmo usare il selettore per pseudo-classe {{cssxref(":first-child")}} — questo selezionerà _sempre_ il primo elemento figlio nell'articolo, e non avremo più bisogno di modificare l'HTML (cosa che potrebbe non essere possibile comunque, magari perché generato da un CMS).

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

Tutte le pseudo-classi si comportano in questo stesso modo. Selezionano una parte del tuo documento che si trova in un certo stato, comportandosi come se avessi aggiunto una classe nel tuo HTML. Guarda altri esempi su MDN:

- [`:last-child`](/it/docs/Web/CSS/:last-child)
- [`:only-child`](/it/docs/Web/CSS/:only-child)
- [`:invalid`](/it/docs/Web/CSS/:invalid)

> [!NOTE]
> È valido scrivere pseudo-classi ed elementi senza alcun selettore di elementi preceduto. Nell'esempio sopra, potresti scrivere `:first-child` e la regola si applicherebbe a _qualsiasi_ elemento che è il primo figlio di un elemento `<article>`, non solo a un paragrafo primo figlio — `:first-child` è equivalente a `*:first-child`. Tuttavia, di solito vuoi più controllo di così, quindi devi essere più specifico.

### Pseudo-classi di azione dell'utente

Alcune pseudo-classi si applicano solo quando l'utente interagisce con il documento in qualche modo. Queste pseudo-classi **azione dell'utente**, a volte chiamate **pseudo-classi dinamiche**, agiscono come se una classe fosse stata aggiunta all'elemento quando l'utente interagisce con esso. Esempi includono:

- [`:hover`](/it/docs/Web/CSS/:hover) — menzionato sopra; si applica solo se l'utente sposta il suo puntatore su un elemento, tipicamente un collegamento.
- [`:focus`](/it/docs/Web/CSS/:focus) — si applica solo se l'utente mette a fuoco l'elemento cliccando o usando i controlli della tastiera.

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

## Cos'è un pseudo-elemento?

Gli pseudo-elementi si comportano in maniera simile. Tuttavia, agiscono come se tu avessi aggiunto un intero nuovo elemento HTML nel markup, piuttosto che applicare una classe agli elementi esistenti.

Gli pseudo-elementi iniziano con un doppio due punti `::`. `::before` è un esempio di pseudo-elemento.

> [!NOTE]
> Alcuni primi pseudo-elementi usavano la sintassi con il singolo due punti, quindi a volte potresti vederla nel codice o negli esempi. I moderni browser supportano i primi pseudo-elementi con sintassi a singolo o doppio due punti per compatibilità con le versioni precedenti.

Ad esempio, se volessi selezionare la prima riga di un paragrafo potresti avvolgerla in un elemento `<span>` e usare un selettore di elementi; tuttavia, ciò fallirebbe se il numero di parole che avevi avvolto fosse più lungo o più corto della larghezza dell'elemento genitore. Dato che tendiamo a non sapere quante parole si adatteranno a una riga — poiché ciò cambierà se la larghezza dello schermo o la dimensione del carattere cambiano — è impossibile fare ciò in modo robusto aggiungendo HTML.

Il selettore pseudo-elemento `::first-line` lo farà per te in modo affidabile — se il numero di parole aumenta o diminuisce selezionerà comunque solo la prima riga.

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

Si comporta come se uno `<span>` fosse magicamente avvolto attorno a quella prima riga formattata, e aggiornato ogni volta che la lunghezza della riga cambia.

Puoi vedere che questo seleziona la prima riga di entrambi i paragrafi.

## Combinare pseudo-classi e pseudo-elementi

Se volessi rendere la prima riga del primo paragrafo in grassetto potresti concatenare i selettori `:first-child` e `::first-line` insieme. Prova a modificare l'esempio live precedente in modo che utilizzi il seguente CSS. Stiamo dicendo che vogliamo selezionare la prima riga del primo elemento `<p>`, che si trova all'interno di un elemento `<article>`.

```css
article p:first-child::first-line {
  font-size: 120%;
  font-weight: bold;
}
```

## Generare contenuto con ::before e ::after

Ci sono un paio di pseudo-elementi speciali, che vengono utilizzati insieme alla proprietà [`content`](/it/docs/Web/CSS/content) per inserire contenuto nel tuo documento usando CSS.

Potresti usarli per inserire una stringa di testo, come nell'esempio live qui sotto. Prova a cambiare il valore di testo della proprietà {{cssxref("content")}} e vedilo cambiare nell'output. Potresti anche cambiare lo pseudo-elemento `::before` in `::after` e vedere il testo inserito alla fine dell'elemento invece che all'inizio.

```html live-sample___before
<p class="box">Content in the box in my HTML page.</p>
```

```css live-sample___before
.box::before {
  content: "This should show before the other content. ";
}
```

{{EmbedLiveSample("before")}}

Inserire stringhe di testo da CSS non è davvero qualcosa che facciamo molto spesso sul web, tuttavia, poiché quel testo è inaccessibile ad alcuni screen reader e potrebbe essere difficile per qualcuno trovarlo e modificarlo in futuro.

Un utilizzo più valido di questi pseudo-elementi è inserire un'icona, ad esempio la piccola freccia aggiunta nell'esempio sotto, che è un indicatore visivo che non vorremmo venga letto da uno screen reader:

```html live-sample___after-icon
<p class="box">Content in the box in my HTML page.</p>
```

```css live-sample___after-icon
.box::after {
  content: " ➥";
}
```

{{EmbedLiveSample("after-icon")}}

Questi pseudo-elementi vengono anche usati frequentemente per inserire una stringa vuota, che può poi essere stilizzata come qualsiasi elemento sulla pagina.

In questo prossimo esempio, abbiamo aggiunto una stringa vuota usando lo pseudo-elemento `::before`. Lo abbiamo impostato su `display: block` in modo che possiamo stilizzarlo con una larghezza e un'altezza. Poi usiamo CSS per stilizzarlo proprio come qualsiasi elemento. Puoi giocare con il CSS e cambiare il suo aspetto e comportamento.

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

L'uso degli pseudo-elementi `::before` e `::after` insieme alla proprietà `content` è definito "Contenuto Generato" in CSS, e vedrai spesso questa tecnica usata per vari compiti. Un ottimo esempio è il sito [CSS Arrow Please](https://cssarrowplease.com/), che ti aiuta a generare una freccia con CSS. Guarda il CSS mentre crei la tua freccia e vedrai gli pseudo-elementi {{cssxref("::before")}} e {{cssxref("::after")}} in uso. Ogni volta che vedi questi selettori, guarda la proprietà {{cssxref("content")}} per vedere cosa viene aggiunto all'elemento HTML.

## Sommario

In questo articolo abbiamo introdotto le pseudo-classi e i pseudo-elementi CSS, che sono tipi speciali di selettori.

Le pseudo-classi ti consentono di targetizzare un elemento quando si trova in un particolare stato, come se avessi aggiunto una classe per quello stato nel DOM. I pseudo-elementi si comportano come se avessi aggiunto un intero nuovo elemento al DOM, e ti consentono di stilizzarlo. Gli pseudo-elementi `::before` e `::after` ti consentono di inserire contenuti nel documento usando CSS.

Nel prossimo articolo, impareremo sui combinatori.

## Vedi anche

- [Riferimento sulle pseudo-classi](/it/docs/Web/CSS/Pseudo-classes)
- [Riferimento sugli pseudo-elementi](/it/docs/Web/CSS/Pseudo-elements)

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Attribute_selectors", "Learn_web_development/Core/Styling_basics/Combinators", "Learn_web_development/Core/Styling_basics")}}
