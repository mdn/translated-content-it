---
title: Pseudo-classi e pseudo-elementi
short-title: Pseudo-classi ed elementi
slug: Learn_web_development/Core/Styling_basics/Pseudo_classes_and_elements
l10n:
  sourceCommit: 3fbc8b2ba17c1cf331fb67ce2e6561b15bf4f197
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Attribute_selectors", "Learn_web_development/Core/Styling_basics/Combinators", "Learn_web_development/Core/Styling_basics")}}

Il prossimo insieme di selettori che verrà esaminato è costituito dalle **pseudo-classi** e dagli **pseudo-elementi**. Ne esiste un gran numero e spesso hanno scopi molto specifici. Una volta imparato a usarli, è possibile esaminare i diversi tipi per verificare se ce n'è uno adatto all'attività da svolgere.

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
          <li>Pseudo-classi e pseudo-elementi.</li>
          <li>La differenza tra i due.</li>
          <li>Combinare pseudo-classi e pseudo-elementi.</li>
          <li>Contenuto generato.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Che cos'è una pseudo-classe?

Una pseudo-classe è un selettore che seleziona elementi che si trovano in uno stato specifico, ad esempio che sono il primo elemento del loro tipo o che si trovano sotto il puntatore del mouse. Tendono ad agire come se fosse stata applicata una classe a una parte del documento, spesso aiutando a ridurre le classi superflue nel markup e fornendo codice più flessibile e manutenibile.

Le pseudo-classi sono parole chiave che iniziano con due punti. Ad esempio, `:hover` è una pseudo-classe.

### Esempio di pseudo-classe di base

Vediamo un esempio di base. Se si volesse rendere più grande e in grassetto il primo paragrafo di un articolo, si potrebbe aggiungere una classe a quel paragrafo e quindi aggiungere CSS a tale classe:

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

Tuttavia, questo potrebbe essere fastidioso da mantenere: cosa succederebbe se un nuovo paragrafo venisse aggiunto all'inizio del documento? Sarebbe necessario spostare la classe nel nuovo paragrafo. Invece di aggiungere la classe, è possibile usare il selettore della pseudo-classe {{cssxref(":first-child")}}: questo selezionerà _sempre_ il primo elemento figlio di un elemento (in questo caso `<article>`), e non sarà più necessario modificare l'HTML (cosa che potrebbe non essere sempre possibile, ad esempio perché viene generato da un CMS).

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

Tutte le pseudo-classi si comportano in questo modo. Selezionano una parte del documento che si trova in un determinato stato, comportandosi come se fosse stata aggiunta una classe nell'HTML.

> [!NOTE]
> È valido scrivere pseudo-classi e pseudo-elementi senza che siano preceduti da un selettore di elemento. Nell'esempio precedente, si potrebbe scrivere `:first-child` e la regola si applicherebbe a _qualsiasi_ elemento che sia il primo figlio di un elemento `<article>`, non solo a un paragrafo come primo figlio: `:first-child` equivale a `*:first-child`. Tuttavia, di solito è necessario un controllo maggiore, quindi occorre essere più specifici.

### Pseudo-classi di azione dell'utente

Alcune pseudo-classi si applicano solo quando l'utente interagisce in qualche modo con il documento. Queste pseudo-classi di **azione dell'utente**, talvolta chiamate **pseudo-classi dinamiche**, agiscono come se una classe fosse stata aggiunta all'elemento quando l'utente interagisce con esso. Alcuni esempi includono:

- {{cssxref(":hover")}} — menzionata in precedenza; si applica solo quando l'utente sposta il puntatore sopra un elemento, in genere un link.
- {{cssxref(":focus")}} — si applica solo quando l'utente mette a fuoco l'elemento facendo clic o usando i controlli della tastiera.

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

### Sperimentare con le pseudo-classi

Tornare al [primo esempio di pseudo-classe](#esempio_di_pseudo-classe_di_base) e modificare il CSS usando il playground MDN:

1. Aggiungere una regola che colori di `blue` il testo del paragrafo quando il puntatore vi passa sopra.
2. Aggiungere una regola che selezioni solo l'ultimo paragrafo all'interno dell'articolo e gli assegni un `background-color` `orange`.

Informazioni su tutte le altre pseudo-classi disponibili sono disponibili nella pagina di riferimento MDN sulle [pseudo-classi](/it/docs/Web/CSS/Reference/Selectors/Pseudo-classes).

## Che cos'è uno pseudo-elemento?

Gli pseudo-elementi si comportano in modo simile. Tuttavia, agiscono come se fosse stato aggiunto al markup un intero nuovo elemento HTML, anziché applicare una classe agli elementi esistenti.

Gli pseudo-elementi iniziano con due punti doppi `::`. `::before` è un esempio di pseudo-elemento.

> [!NOTE]
> Alcuni pseudo-elementi iniziali utilizzavano la sintassi con due punti singoli, quindi talvolta potrebbe essere presente nel codice o negli esempi. I browser moderni supportano gli pseudo-elementi iniziali con sintassi a due punti singoli o doppi per compatibilità con le versioni precedenti.

Ad esempio, se si volesse selezionare la prima riga di un paragrafo, sarebbe possibile racchiuderla in un elemento `<span>` e usare un selettore di elemento; tuttavia, ciò non funzionerebbe se le parole racchiuse fossero più lunghe o più corte della larghezza dell'elemento genitore. Poiché in genere non si sa quante parole entreranno in una riga — dato che questo cambierà se cambia la larghezza dello schermo o `font-size` — è impossibile farlo in modo affidabile aggiungendo HTML.

Il selettore dello pseudo-elemento `::first-line` eseguirà questa operazione in modo affidabile: se il numero di parole aumenta o diminuisce, selezionerà comunque solo la prima riga.

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

Si comporta come se un `<span>` venisse magicamente racchiuso attorno a quella prima riga formattata e aggiornato ogni volta che la lunghezza della riga cambia.

Si può notare che viene selezionata la prima riga di entrambi i paragrafi.

### Sperimentare con gli pseudo-elementi

Modificare il CSS dell'esempio precedente usando il playground MDN:

1. Aggiungere una regola che assegni un `background-color` `red` alla porzione di testo selezionata con il cursore del mouse (sarà necessario lo pseudo-elemento {{cssxref("::selection")}}). Selezionare del testo per provarla.
2. Aggiungere una regola che assegni alla prima lettera di ogni `<p>` all'interno di `<article>`:

- Un `background-color` `yellow`.
- Un `border` `1px solid black`.
- Un `font-size` di `2rem`.

Informazioni su tutti gli altri pseudo-elementi disponibili sono disponibili nella pagina di riferimento MDN sugli [pseudo-elementi](/it/docs/Web/CSS/Reference/Selectors/Pseudo-elements).

## Combinare pseudo-classi e pseudo-elementi

Se si volesse rendere in grassetto la prima riga del primo paragrafo, si potrebbero concatenare i selettori `:first-child` e `::first-line`.

Provare a modificare l'esempio precedente affinché usi il seguente CSS. Si desidera selezionare la prima riga del primo elemento `<p>` che si trova all'interno di un elemento `<article>`.

```css
article p:first-child::first-line {
  font-size: 120%;
  font-weight: bold;
}
```

## Generare contenuto con ::before e ::after

Esistono un paio di pseudo-elementi speciali, usati insieme alla proprietà {{cssxref("content")}} per inserire contenuto nel documento usando CSS. Questa tecnica è chiamata **contenuto generato**.

Può essere usata per inserire una stringa di testo, come nell'esempio seguente. Al contenuto generato è stato inoltre assegnato un colore di sfondo `yellow`, in modo da poterlo distinguere facilmente dal contenuto del paragrafo.

```html live-sample___before
<p class="box">Content in the box in my HTML page.</p>
```

```css live-sample___before
.box::before {
  content: "This should show before the other content. ";
  background-color: yellow;
}
```

{{EmbedLiveSample("before")}}

### Sperimentare con il contenuto generato

Provare a modificare l'esempio precedente come segue:

- Modificare il valore di testo della proprietà {{cssxref("content")}} e osservarne la modifica nell'output.
- Modificare lo pseudo-elemento `::before` in `::after` e osservare il testo inserito alla fine dell'elemento anziché all'inizio.

### Icone di contenuto generato

L'esempio precedente è CSS valido. Tuttavia, inserire stringhe di testo dal CSS non è qualcosa che viene fatto molto spesso, poiché quel testo è inaccessibile ad alcuni screen reader e potrebbe essere difficile da trovare e modificare in futuro. Un uso più valido di questi pseudo-elementi consiste nell'inserire un'icona, ad esempio la piccola freccia aggiunta nell'esempio seguente, che è un indicatore visivo che non dovrebbe essere letto da uno screen reader:

```html live-sample___after-icon
<p class="box">Content in the box in my HTML page.</p>
```

```css live-sample___after-icon
.box::after {
  content: " ➥";
}
```

{{EmbedLiveSample("after-icon")}}

### Forme generate

Il contenuto generato viene spesso usato anche per inserire una stringa vuota, che può quindi essere stilizzata proprio come qualsiasi elemento della pagina.

Nell'esempio successivo, è stata aggiunta una stringa vuota usando lo pseudo-elemento `::before`. È stato impostato `display: block` in modo da poterlo stilizzare con una larghezza e un'altezza, creando una forma quadrata. Viene quindi usato CSS per stilizzarlo come qualsiasi elemento.

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

Provare a sperimentare con il CSS precedente per modificare l'aspetto e il comportamento della forma generata.

Il contenuto generato viene usato regolarmente per varie altre attività. Un ottimo esempio è il sito [CSS Arrow Please](https://cssarrowplease.com/), che aiuta a generare una freccia con CSS. Osservando il CSS mentre si crea la freccia, si vedranno in uso gli pseudo-elementi {{cssxref("::before")}} e {{cssxref("::after")}}. Quando si incontrano questi selettori, osservare la proprietà {{cssxref("content")}} per vedere cosa viene aggiunto all'elemento HTML.

## Riepilogo

In questo articolo sono state introdotte le pseudo-classi e gli pseudo-elementi CSS, che sono tipi speciali di selettori.

Le pseudo-classi consentono di selezionare un elemento quando si trova in uno stato particolare, come se fosse stata aggiunta una classe per quello stato al DOM. Gli pseudo-elementi agiscono come se fosse stato aggiunto un intero nuovo elemento al DOM e consentono di stilizzarlo. Gli pseudo-elementi `::before` e `::after` consentono di inserire contenuto nel documento usando CSS.

Nel prossimo articolo verranno esaminati i combinatori.

## Vedi anche

- [Riferimento delle pseudo-classi](/it/docs/Web/CSS/Reference/Selectors/Pseudo-classes)
- [Riferimento degli pseudo-elementi](/it/docs/Web/CSS/Reference/Selectors/Pseudo-elements)

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Attribute_selectors", "Learn_web_development/Core/Styling_basics/Combinators", "Learn_web_development/Core/Styling_basics")}}
