---
title: Selettori di attributo
slug: Learn_web_development/Core/Styling_basics/Attribute_selectors
l10n:
  sourceCommit: c9f602a26092661130a031b7148d696a3ac9802e
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Basic_selectors", "Learn_web_development/Core/Styling_basics/Pseudo_classes_and_elements", "Learn_web_development/Core/Styling_basics")}}

Come noto dallo studio di HTML, gli elementi possono avere attributi che forniscono ulteriori dettagli sull'elemento sottoposto a markup. In CSS è possibile usare i selettori di attributo per selezionare elementi con determinati attributi. Questa lezione mostra come usare questi selettori molto utili.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Fondamenti di HTML (studiare
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi HTML di base</a
        >), <a href="/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors">Selettori CSS di base</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Il concetto di base dei selettori di attributo.</li>
          <li>Selettori di attributo per presenza e valore.</li>
          <li>Selettori di attributo per corrispondenza di sottostringhe.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Selettori per presenza e valore

Questi selettori consentono di selezionare un elemento in base alla sola presenza di un attributo (ad esempio `href`) oppure in base a diverse corrispondenze con il valore dell'attributo.

| Selettore        | Esempio                         | Descrizione                                                                                                                                                    |
| ---------------- | ------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `[attr]`         | `a[title]`                      | Corrisponde agli elementi con un attributo _attr_ (il cui nome è il valore tra parentesi quadre).                                                              |
| `[attr=value]`   | `a[href="https://example.com"]` | Corrisponde agli elementi con un attributo _attr_ il cui valore è esattamente _value_ — la stringa tra virgolette.                                             |
| `[attr~=value]`  | `p[class~="special"]`           | Corrisponde agli elementi con un attributo _attr_ il cui valore è esattamente _value_ oppure contiene _value_ nel relativo elenco di valori separati da spazi. |
| `[attr\|=value]` | `div[lang\|="zh"]`              | Corrisponde agli elementi con un attributo _attr_ il cui valore è esattamente _value_ oppure inizia con _value_ seguito immediatamente da un trattino.         |

Nell'esempio seguente è possibile vedere questi selettori in uso.

- Usando `li[class]` è possibile trovare qualsiasi elemento di elenco con un attributo class. Questo corrisponde a tutti gli elementi di elenco tranne il primo.
- `li[class="a"]` corrisponde a un selettore con una classe `a`, ma non a un selettore con una classe `a` e un'altra classe separata da spazi come parte del valore. Seleziona il secondo elemento di elenco.
- `li[class~="a"]` corrisponde a una classe `a`, ma anche a un valore che contiene la classe `a` come parte di un elenco separato da spazi. Seleziona il secondo e il terzo elemento di elenco.

```html live-sample___attribute
<h1>Attribute presence and value selectors</h1>
<ul>
  <li>Item 1</li>
  <li class="a">Item 2</li>
  <li class="a b">Item 3</li>
  <li class="ab">Item 4</li>
</ul>
```

```css live-sample___attribute
body {
  font-family: sans-serif;
}
li[class] {
  font-size: 120%;
}

li[class="a"] {
  background-color: yellow;
}

li[class~="a"] {
  color: red;
}
```

{{EmbedLiveSample("attribute", "", "200px")}}

Provare a modificare il CSS precedente per aggiungere una regola che selezioni soltanto gli elementi di elenco con un valore dell'attributo `class` pari a `ab` e assegni loro un testo di `color` `white` e un `background-color` `purple`.

## Selettori per corrispondenza di sottostringhe

Questi selettori consentono una corrispondenza più avanzata delle sottostringhe all'interno del valore dell'attributo. Ad esempio, se fossero presenti classi `box-warning` e `box-error` e si volesse far corrispondere tutto ciò che inizia con la stringa "box-", si potrebbe usare `[class^="box-"]` per selezionarle entrambe (oppure `[class|="box"]`, come descritto nella sezione precedente).

| Selettore       | Esempio             | Descrizione                                                                                                        |
| --------------- | ------------------- | ------------------------------------------------------------------------------------------------------------------ |
| `[attr^=value]` | `li[class^="box-"]` | Corrisponde agli elementi con un attributo _attr_ il cui valore inizia con _value_.                                |
| `[attr$=value]` | `li[class$="-box"]` | Corrisponde agli elementi con un attributo _attr_ il cui valore termina con _value_.                               |
| `[attr*=value]` | `li[class*="box"]`  | Corrisponde agli elementi con un attributo _attr_ il cui valore contiene _value_ in qualsiasi punto della stringa. |

L'esempio successivo mostra l'uso di questi selettori:

- `li[class^="a"]` corrisponde a qualsiasi valore di attributo che inizia con `a`, quindi corrisponde ai primi due elementi di elenco.
- `li[class$="a"]` corrisponde a qualsiasi valore di attributo che termina con `a`, quindi corrisponde al primo e al terzo elemento di elenco.
- `li[class*="a"]` corrisponde a qualsiasi valore di attributo in cui `a` appare in qualsiasi punto della stringa, quindi corrisponde a tutti gli elementi di elenco.

```html live-sample___attribute-substring
<h1>Attribute substring matching selectors</h1>
<ul>
  <li class="a">Item 1</li>
  <li class="ab">Item 2</li>
  <li class="bca">Item 3</li>
  <li class="bcabc">Item 4</li>
</ul>
```

```css live-sample___attribute-substring
body {
  font-family: sans-serif;
}
li[class^="a"] {
  font-size: 120%;
}

li[class$="a"] {
  background-color: yellow;
}

li[class*="a"] {
  color: red;
}
```

{{EmbedLiveSample("attribute-substring", "", "200px")}}

Provare a modificare il CSS precedente per aggiungere una regola che selezioni soltanto gli elementi di elenco con un valore dell'attributo `class` che termina con `b` o `c` e assegni loro un `border` `black`, `solid` e largo `2px`. Per risolvere questo esercizio potrebbe essere necessario usare un [elenco di selettori](/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors#selector_lists).

## Riepilogo

Ora che i selettori di attributo sono stati trattati, è possibile continuare con il prossimo articolo e leggere dei selettori di pseudo-classi e pseudo-elementi.

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Basic_selectors", "Learn_web_development/Core/Styling_basics/Pseudo_classes_and_elements", "Learn_web_development/Core/Styling_basics")}}
