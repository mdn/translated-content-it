---
title: Elenchi
slug: Learn_web_development/Core/Structuring_content/Lists
l10n:
  sourceCommit: 27f34d8b137f9bb2b467f9f9a1c4e1d04e12ed89
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Emphasis_and_importance", "Learn_web_development/Core/Structuring_content/Test_your_skills/HTML_text_basics", "Learn_web_development/Core/Structuring_content")}}

Ora concentriamoci sugli elenchi. Gli elenchi sono ovunque nella vita: dalla lista della spesa all'elenco di indicazioni che si seguono inconsciamente per arrivare a casa ogni giorno, fino agli elenchi di istruzioni che si stanno seguendo in questi tutorial! Non sorprende che HTML disponga di un comodo insieme di elementi che consente di definire diversi tipi di elenco. Sul Web esistono tre tipi di elenchi: non ordinati, ordinati e di descrizione. Questa lezione mostra come utilizzare i diversi tipi.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Conoscenza di base di HTML, come illustrato in
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi HTML di base</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>La struttura HTML per i tre tipi di elenchi: non ordinati, ordinati e di descrizione.</li>
          <li>L'uso corretto di ciascun tipo di elenco.</li>
          <li>I casi d'uso più ampi degli elenchi, come i menu di navigazione.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Elenchi non ordinati

Gli elenchi non ordinati vengono utilizzati per contrassegnare elenchi di elementi per i quali l'ordine non è importante. Prendiamo come esempio una lista della spesa:

```plain
milk
eggs
bread
hummus
```

In questo esempio, gli elementi possono trovarsi in qualsiasi ordine. Per creare questo elenco in HTML, prima si racchiude l'intero elenco in un elemento {{htmlelement("ul")}} (unordered list).
Quindi, si racchiude ciascun elemento in un elemento {{htmlelement("li")}} (list item):

```html
<ul>
  <li>milk</li>
  <li>eggs</li>
  <li>bread</li>
  <li>hummus</li>
</ul>
```

### Contrassegnare un elenco non ordinato

Per fare pratica, prova a contrassegnare autonomamente l'elenco precedente:

1. Fai clic su **"Play"** nell'output del codice renderizzato qui sotto per modificare l'esempio nel MDN Playground.
2. Trasforma i singoli elementi di testo in un elenco non ordinato.

Se viene commesso un errore, è possibile cancellare il lavoro usando il pulsante _Reset_ nel MDN Playground. In caso di difficoltà, consulta di nuovo lo snippet di codice precedente.

```html hidden live-sample___lists_1
milk eggs bread hummus
```

{{ EmbedLiveSample('lists_1', "100%", 60) }}

## Ordinati

Gli elenchi ordinati sono elenchi in cui l'ordine degli elementi _è_ importante. Prendiamo come esempio una serie di indicazioni:

```plain
Drive to the end of the road
Turn right
Go straight across the first two roundabouts
Turn left at the third roundabout
The school is on your right, 300 meters up the road
```

La struttura del markup è la stessa degli elenchi non ordinati, tranne per il fatto che gli elementi dell'elenco devono essere racchiusi in un elemento {{htmlelement("ol")}}, anziché in `<ul>`:

```html
<ol>
  <li>Drive to the end of the road</li>
  <li>Turn right</li>
  <li>Go straight across the first two roundabouts</li>
  <li>Turn left at the third roundabout</li>
  <li>The school is on your right, 300 meters up the road</li>
</ol>
```

### Contrassegnare un elenco ordinato

È di nuovo il momento di fare pratica! Come nell'attività precedente, prova a contrassegnare autonomamente il precedente elenco ordinato.

1. Fai clic su **"Play"** nell'output del codice renderizzato qui sotto per modificare l'esempio nel MDN Playground.
2. Trasforma i singoli elementi di testo in un elenco ordinato.

Se viene commesso un errore, è possibile cancellare il lavoro usando il pulsante _Reset_ nel MDN Playground. In caso di difficoltà, consulta di nuovo lo snippet di codice precedente.

```html hidden live-sample___lists_2
Drive to the end of the road Turn right Go straight across the first two
roundabouts Turn left at the third roundabout The school is on your right, 300
meters up the road
```

{{ EmbedLiveSample('lists_2', "100%", 60) }}

## Contrassegnare la nostra pagina della ricetta

Ora una vera sfida! A questo punto dell'articolo, sono disponibili tutte le informazioni necessarie per contrassegnare una sezione di contenuto leggermente più complessa. Occorre contrassegnare le istruzioni per la nostra ricetta di hummus preferita.

È possibile scegliere di:

- Salvare una copia locale del file iniziale [text-start.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/html-text-formatting/text-start.html) e svolgere il lavoro nel proprio editor di codice.
- Fare clic su **"Play"** nell'output del codice renderizzato qui sotto per modificare l'esempio nel MDN Playground.

Le istruzioni da seguire sono:

1. Contrassegna il titolo principale della pagina usando un elemento `<h1>` e i tre sottotitoli usando elementi `<h2>`.
2. Ci sono cinque righe di testo che è opportuno contrassegnare con elementi `<p>`. Fallo ora.
3. Contrassegna l'elenco degli ingredienti come elenco non ordinato.
4. Contrassegna l'elenco delle istruzioni come elenco ordinato.

Se viene commesso un errore, è possibile cancellare il lavoro usando il pulsante _Reset_ nel MDN Playground. In caso di notevoli difficoltà, è possibile visualizzare la soluzione sotto l'output del codice.

```html-nolint live-sample___lists_3
Quick hummus recipe

This recipe makes quick, tasty hummus, with no messing. It has been adapted from a number of different recipes that I have read over the years.

Hummus is a delicious thick paste used heavily in Greek and Middle Eastern dishes. It is very tasty with salad, grilled meats and pitta breads.

Ingredients

1 can (400g) of chick peas (garbanzo beans)
175g of tahini
6 sundried tomatoes
Half a red pepper
A pinch of cayenne pepper
1 clove of garlic
A dash of olive oil

Instructions

Remove the skin from the garlic, and chop coarsely
Remove all the seeds and stalk from the pepper, and chop coarsely
Add all the ingredients into a food processor
Process all the ingredients into a paste
If you want a coarse "chunky" hummus, process it for a short time
If you want a smooth hummus, process it for a longer time

For a different flavor, you could try blending in a small measure of lemon and coriander, chili pepper, lime and chipotle, harissa and mint, or spinach and feta cheese. Experiment and see what works for you.

Storage

Refrigerate the finished hummus in a sealed container. You should be able to use it for about a week after you've made it. If it starts to become fizzy, you should definitely discard it.

Hummus is suitable for freezing; you should thaw it and use it within a couple of months.
```

{{ EmbedLiveSample('lists_3', "100%", 260) }}

<details>
<summary>Fai clic qui per mostrare la soluzione</summary>

Un esempio dell'HTML corretto per questo esempio è disponibile in [text-complete.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/html-text-formatting/text-complete.html) nel nostro repository GitHub.

</details>

## Annidare gli elenchi

È perfettamente corretto annidare un elenco all'interno di un altro. Potrebbe essere necessario avere alcuni punti elenco secondari sotto un punto elenco di primo livello. Prendiamo il secondo elenco dell'esempio della ricetta:

```html
<ol>
  <li>Remove the skin from the garlic, and chop coarsely.</li>
  <li>Remove all the seeds and stalk from the pepper, and chop coarsely.</li>
  <li>Add all the ingredients into a food processor.</li>
  <li>Process all the ingredients into a paste.</li>
  <li>If you want a coarse "chunky" hummus, process it for a short time.</li>
  <li>If you want a smooth hummus, process it for a longer time.</li>
</ol>
```

Poiché gli ultimi due punti elenco sono strettamente correlati a quello che li precede (si leggono come sottoistruzioni o scelte che si adattano a quel punto elenco), potrebbe essere opportuno annidarli in un proprio elenco non ordinato e inserire tale elenco nel quarto punto elenco corrente. Il risultato sarebbe il seguente:

```html
<ol>
  <li>Remove the skin from the garlic, and chop coarsely.</li>
  <li>Remove all the seeds and stalk from the pepper, and chop coarsely.</li>
  <li>Add all the ingredients into a food processor.</li>
  <li>
    Process all the ingredients into a paste.
    <ul>
      <li>
        If you want a coarse "chunky" hummus, process it for a short time.
      </li>
      <li>If you want a smooth hummus, process it for a longer time.</li>
    </ul>
  </li>
</ol>
```

Prova a tornare all'attività precedente e ad aggiornare il secondo elenco in questo modo.

## Elenchi di descrizione

Lo scopo degli elenchi di descrizione è contrassegnare un insieme di elementi e le descrizioni associate, come termini e definizioni oppure domande e risposte. Osserviamo un esempio di insieme di termini e definizioni:

```plain
soliloquy
In drama, where a character speaks to themselves, representing their inner thoughts or feelings and in the process relaying them to the audience (but not to other characters.)
monologue
In drama, where a character speaks their thoughts out loud to share them with the audience and any other characters present.
aside
In drama, where a character shares a comment only with the audience for humorous or dramatic effect. This is usually a feeling, thought or piece of additional background information
```

Gli elenchi di descrizione utilizzano un contenitore diverso rispetto agli altri tipi di elenco — {{htmlelement("dl")}}; inoltre, ciascun termine viene racchiuso in un elemento {{htmlelement("dt")}} (description term) e ciascuna descrizione viene racchiusa in un elemento {{htmlelement("dd")}} (description definition).

### Esempio di elenco di descrizione

Completiamo il markup del nostro esempio:

```html
<dl>
  <dt>soliloquy</dt>
  <dd>
    In drama, where a character speaks to themselves, representing their inner
    thoughts or feelings and in the process relaying them to the audience (but
    not to other characters.)
  </dd>
  <dt>monologue</dt>
  <dd>
    In drama, where a character speaks their thoughts out loud to share them
    with the audience and any other characters present.
  </dd>
  <dt>aside</dt>
  <dd>
    In drama, where a character shares a comment only with the audience for
    humorous or dramatic effect. This is usually a feeling, thought, or piece of
    additional background information.
  </dd>
</dl>
```

Gli stili predefiniti del browser visualizzano gli elenchi di descrizione con le descrizioni leggermente rientrate rispetto ai termini.

{{EmbedLiveSample('Description_list_example', '100%', '285px')}}

### Più descrizioni per un termine

Si noti che è consentito avere un singolo termine con più descrizioni, per esempio:

```html
<dl>
  <dt>aside</dt>
  <dd>
    In drama, where a character shares a comment only with the audience for
    humorous or dramatic effect. This is usually a feeling, thought, or piece of
    additional background information.
  </dd>
  <dd>
    In writing, a section of content that is related to the current topic, but
    doesn't fit directly into the main flow of content so is presented nearby
    (often in a box off to the side.)
  </dd>
</dl>
```

{{EmbedLiveSample('Multiple_descriptions_for_one_term', '100%', '193px')}}

### Contrassegnare un insieme di definizioni

È il momento di provare a contrassegnare un elenco di descrizione:

1. Fai clic su **"Play"** nel blocco di codice qui sotto per modificare l'esempio nel MDN Playground.
2. Utilizza elementi appropriati per contrassegnare i tre termini e le quattro descrizioni nel contenuto. Tieni presente che il terzo termine ha due descrizioni.

Se viene commesso un errore, è possibile cancellare il lavoro usando il pulsante _Reset_ nel MDN Playground. In caso di notevoli difficoltà, è possibile visualizzare la soluzione sotto il blocco di codice.

```html-nolint live-sample___lists_4
Love
The glue that binds the world together.
Eggs
The glue that binds the cake together.
Coffee
The drink that gets the world running in the morning.
A light brown color.
```

{{ EmbedLiveSample('lists_4', "100%", 60) }}

<details>
<summary>Fai clic qui per mostrare la soluzione</summary>

L'HTML completato dovrebbe avere questo aspetto:

```html
<dl>
  <dt>Love</dt>
  <dd>The glue that binds the world together.</dd>
  <dt>Eggs</dt>
  <dd>The glue that binds the cake together.</dd>
  <dt>Coffee</dt>
  <dd>The drink that gets the world running in the morning.</dd>
  <dd>A light brown color.</dd>
</dl>
```

</details>

## Riepilogo

Questo è tutto per gli elenchi. Successivamente verranno proposti alcuni test da utilizzare per verificare quanto bene siano state comprese e ricordate le informazioni fornite sulle basi del testo HTML.

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Emphasis_and_importance", "Learn_web_development/Core/Structuring_content/Test_your_skills/HTML_text_basics", "Learn_web_development/Core/Structuring_content")}}
