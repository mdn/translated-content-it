---
title: Selettori di attributo
slug: Learn_web_development/Core/Styling_basics/Attribute_selectors
l10n:
  sourceCommit: 4436741f7ac013be6aa98e71132fcce457657d48
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Basic_selectors", "Learn_web_development/Core/Styling_basics/Pseudo_classes_and_elements", "Learn_web_development/Core/Styling_basics")}}

Come sa dal suo studio dell'HTML, gli elementi possono avere attributi che forniscono ulteriori dettagli sull'elemento marcato. In CSS è possibile utilizzare i selettori di attributo per targhetizzare elementi con determinati attributi. Questa lezione le mostrerà come utilizzare questi selettori molto utili.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Basi di HTML (studiare
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi di base HTML</a
        >), <a href="/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors">Selettori CSS di base</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Il concetto base dei selettori di attributo.</li>
          <li>Selettori di attributo di presenza e valore.</li>
          <li>Selettori di attributo con corrispondenza di sottostringhe.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Selettori di presenza e valore

Questi selettori consentono di selezionare un elemento basato solo sulla presenza di un attributo (per esempio `href`), o su varie corrispondenze diverse contro il valore dell'attributo.

| Selettore       | Esempio                         | Descrizione                                                                                                                           |
| --------------- | ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| `[attr]`        | `a[title]`                      | Seleziona elementi con un attributo _attr_ (il cui nome è il valore tra parentesi quadre).                                            |
| `[attr=value]`  | `a[href="https://example.com"]` | Seleziona elementi con un attributo _attr_ il cui valore è esattamente _value_ — la stringa all'interno delle virgolette.             |
| `[attr~=value]` | `p[class~="special"]`           | Seleziona elementi con un attributo _attr_ il cui valore è esattamente _value_ o contiene _value_ nella sua lista separata da spazi.  |
| `[attr\|=value]`| `div[lang\|="zh"]`              | Seleziona elementi con un attributo _attr_ il cui valore è esattamente _value_ o inizia con _value_ immediatamente seguito da un trattino. |

Nell'esempio sotto può vedere questi selettori in uso.

- Usando `li[class]` possiamo selezionare qualsiasi elemento della lista con un attributo class. Questo seleziona tutti gli elementi della lista tranne il primo.
- `li[class="a"]` seleziona un selettore con una classe di `a`, ma non un selettore con una classe di `a` con un'altra classe separata da uno spazio come parte del valore. Seleziona il secondo elemento della lista.
- `li[class~="a"]` selezionerà una classe di `a` ma anche un valore che contiene la classe di `a` come parte di una lista separata da spazi bianchi. Seleziona il secondo e il terzo elemento della lista.

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

## Selettori con corrispondenza di sottostringhe

Questi selettori permettono di effettuare corrispondenze più avanzate di sottostringhe all'interno del valore del suo attributo. Ad esempio, se avesse classi di `box-warning` e `box-error` e volesse selezionare tutto ciò che inizia con la stringa "box-", potrebbe usare `[class^="box-"]` per selezionarli entrambi (o `[class|="box"]` come descritto nella sezione sopra).

| Selettore       | Esempio             | Descrizione                                                                                                             |
| --------------- | ------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| `[attr^=value]` | `li[class^="box-"]` | Seleziona elementi con un attributo _attr_, il cui valore inizia con _value_.                                           |
| `[attr$=value]` | `li[class$="-box"]` | Seleziona elementi con un attributo _attr_ il cui valore termina con _value_.                                           |
| `[attr*=value]` | `li[class*="box"]`  | Seleziona elementi con un attributo _attr_ il cui valore contiene _value_ in qualsiasi punto della stringa.             |

(A parte: Può essere utile notare che `^` e `$` sono stati a lungo utilizzati come _ancore_ nelle cosiddette _espressioni regolari_ per significare rispettivamente _inizia con_ e _termina con_).

Il prossimo esempio mostra l'uso di questi selettori:

- `li[class^="a"]` seleziona qualsiasi valore attributo che inizi con `a`, quindi seleziona i primi due elementi della lista.
- `li[class$="a"]` seleziona qualsiasi valore attributo che termini con `a`, quindi seleziona il primo e il terzo elemento della lista.
- `li[class*="a"]` seleziona qualsiasi valore attributo in cui `a` appare in qualsiasi punto della stringa, quindi seleziona tutti i nostri elementi della lista.

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

## Riepilogo

Ora che abbiamo terminato con i selettori di attributo, può continuare con l'articolo successivo e leggere sui selettori di pseudo-classe e pseudo-elemento.

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Basic_selectors", "Learn_web_development/Core/Styling_basics/Pseudo_classes_and_elements", "Learn_web_development/Core/Styling_basics")}}
