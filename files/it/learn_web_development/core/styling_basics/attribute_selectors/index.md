---
title: Selettori di attributo
slug: Learn_web_development/Core/Styling_basics/Attribute_selectors
l10n:
  sourceCommit: 4436741f7ac013be6aa98e71132fcce457657d48
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Basic_selectors", "Learn_web_development/Core/Styling_basics/Pseudo_classes_and_elements", "Learn_web_development/Core/Styling_basics")}}

Come sai dal tuo studio dell'HTML, gli elementi possono avere attributi che forniscono ulteriori dettagli sugli elementi contrassegnati. In CSS puoi utilizzare i selettori di attributo per mirare agli elementi con determinati attributi. Questa lezione ti mostrerà come utilizzare questi selettori molto utili.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Nozioni fondamentali di HTML (studio della
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >sintassi di base dell'HTML</a
        >), <a href="/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors">selettori CSS di base</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Il concetto base dei selettori di attributo.</li>
          <li>Selettori di presenza e valore degli attributi.</li>
          <li>Selettori di corrispondenza di sottostringhe degli attributi.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Selettori di presenza e valore

Questi selettori consentono la selezione di un elemento basata solo sulla presenza di un attributo (per esempio `href`), o su varie corrispondenze diverse rispetto al valore dell'attributo.

| Selettore        | Esempio                         | Descrizione                                                                                                                                        |
| ---------------- | ------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| `[attr]`         | `a[title]`                      | Corrisponde agli elementi con un attributo _attr_ (il cui nome è il valore tra parentesi quadre).                                                  |
| `[attr=value]`   | `a[href="https://example.com"]` | Corrisponde agli elementi con un attributo _attr_ il cui valore è esattamente _value_ — la stringa all'interno delle virgolette.                   |
| `[attr~=value]`  | `p[class~="special"]`           | Corrisponde agli elementi con un attributo _attr_ il cui valore è esattamente _value_ o contiene _value_ nel suo elenco di valori separati da spazio. |
| `[attr\|=value]` | `div[lang\|="zh"]`              | Corrisponde agli elementi con un attributo _attr_ il cui valore è esattamente _value_ o inizia con _value_ immediatamente seguito da un trattino.  |

Nell'esempio sottostante puoi vedere come vengono utilizzati questi selettori.

- Usando `li[class]` possiamo corrispondere a qualsiasi elemento di lista con un attributo classe. Questo corrisponde a tutti gli elementi della lista tranne il primo.
- `li[class="a"]` corrisponde a un selettore con una classe di `a`, ma non a un selettore con una classe di `a` con un'altra classe separata da spazi come parte del valore. Seleziona il secondo elemento della lista.
- `li[class~="a"]` corrisponderà a una classe di `a`, ma anche a un valore che contiene la classe di `a` come parte di un elenco separato da spazi bianchi. Seleziona il secondo e terzo elemento della lista.

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

## Selettori di corrispondenza di sottostringhe

Questi selettori consentono una corrispondenza più avanzata di sottostringhe all'interno del valore del tuo attributo. Per esempio, se hai delle classi `box-warning` e `box-error` e vuoi corrispondere a tutto ciò che inizia con la stringa "box-", puoi usare `[class^="box-"]` per selezionarle entrambe (o `[class|="box"]` come descritto nella sezione sopra).

| Selettore        | Esempio             | Descrizione                                                                                                                                      |
| ---------------  | ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| `[attr^=value]`  | `li[class^="box-"]` | Corrisponde agli elementi con un attributo _attr_ il cui valore inizia con _value_.                                                              |
| `[attr$=value]`  | `li[class$="-box"]` | Corrisponde agli elementi con un attributo _attr_ il cui valore termina con _value_.                                                             |
| `[attr*=value]`  | `li[class*="box"]`  | Corrisponde agli elementi con un attributo _attr_ il cui valore contiene _value_ in qualsiasi parte della stringa.                               |

(A parte: Può essere utile notare che `^` e `$` sono stati a lungo utilizzati come _ancore_ nelle cosiddette _espressioni regolari_ per significare _inizia con_ e _finisce con_ rispettivamente.)

Il prossimo esempio mostra l'uso di questi selettori:

- `li[class^="a"]` corrisponde a qualsiasi valore di attributo che inizia con `a`, quindi corrisponde ai primi due elementi della lista.
- `li[class$="a"]` corrisponde a qualsiasi valore di attributo che termina con `a`, quindi corrisponde al primo e al terzo elemento della lista.
- `li[class*="a"]` corrisponde a qualsiasi valore di attributo in cui `a` appare ovunque nella stringa, quindi corrisponde a tutti i nostri elementi della lista.

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

Ora che abbiamo finito con i selettori di attributo, puoi continuare con il prossimo articolo e leggere i selettori di pseudo-classe e pseudo-elemento.

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Basic_selectors", "Learn_web_development/Core/Styling_basics/Pseudo_classes_and_elements", "Learn_web_development/Core/Styling_basics")}}
