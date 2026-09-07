---
title: Funzionalità di testo avanzate
slug: Learn_web_development/Core/Structuring_content/Advanced_text_features
l10n:
  sourceCommit: 2066cc916dfdcbb782340bf0ce562b230e947cba
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Test_your_skills/HTML_text_basics", "Learn_web_development/Core/Structuring_content/Test_your_skills/Advanced_HTML_text", "Learn_web_development/Core/Structuring_content")}}

In HTML esistono molti altri elementi per definire la semantica del testo, che non sono stati trattati nell'articolo [Enfasi e importanza](/it/docs/Learn_web_development/Core/Structuring_content/Emphasis_and_importance). Gli elementi descritti in questo articolo sono meno noti, ma è comunque utile conoscerli (e questo non è affatto un elenco completo). Qui verranno illustrate le modalità per contrassegnare citazioni, codice informatico e altro testo correlato, apici e pedici, informazioni di contatto e altro ancora.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Conoscenza di base di HTML, come illustrato in
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi HTML di base</a
        >. Semantica a livello di testo, come <a href="/it/docs/Learn_web_development/Core/Structuring_content/Headings_and_paragraphs"
          >titoli e paragrafi</a
        > e <a href="/it/docs/Learn_web_development/Core/Structuring_content/Lists"
          >elenchi</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>Citazioni.</li>
          <li>Abbreviazioni e acronimi.</li>
          <li>Indirizzi.</li>
          <li>Ore e date.</li>
          <li>Apici e pedici.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Citazioni

HTML include funzionalità per contrassegnare le citazioni; l'elemento da usare dipende dal fatto che si stia contrassegnando una citazione a blocco o in linea.

### Citazioni a blocco

Se una sezione di contenuto a livello di blocco, come un paragrafo, più paragrafi, un elenco e così via, viene citata da un'altra fonte, è necessario racchiuderla in un elemento {{htmlelement("blockquote")}} per indicarlo e includere un URL che punti alla fonte della citazione in un attributo [`cite`](/it/docs/Web/HTML/Reference/Elements/blockquote#cite). Ad esempio, il seguente markup è tratto dalla pagina MDN dell'elemento `<blockquote>`:

```html
<p>
  The <strong>HTML <code>&lt;blockquote&gt;</code> Element</strong> (or
  <em>HTML Block Quotation Element</em>) indicates that the enclosed text is an
  extended quotation.
</p>
```

Per trasformarlo in una citazione a blocco, sarebbe sufficiente fare quanto segue:

```html
<p>Here is a blockquote:</p>
<blockquote
  cite="https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/blockquote">
  <p>
    The <strong>HTML <code>&lt;blockquote&gt;</code> Element</strong> (or
    <em>HTML Block Quotation Element</em>) indicates that the enclosed text is
    an extended quotation.
  </p>
</blockquote>
```

Lo stile predefinito del browser renderà questo contenuto come un paragrafo rientrato, a indicare che si tratta di una citazione; il paragrafo sopra la citazione serve a dimostrarlo.

{{EmbedLiveSample('Blockquotes', '100%', '200px')}}

### Citazioni in linea

Le citazioni in linea funzionano esattamente allo stesso modo, tranne per il fatto che usano l'elemento {{htmlelement("q")}}. Ad esempio, il frammento di markup seguente contiene una citazione dalla pagina MDN di `<q>`:

```html
<p>
  The quote element — <code>&lt;q&gt;</code> — is
  <q
    cite="https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/q">
    intended for short quotations that don't require paragraph breaks.
  </q>
</p>
```

Lo stile predefinito del browser renderà questo testo normale racchiuso tra virgolette per indicare una citazione, in questo modo:

{{EmbedLiveSample('Inline_quotations', '100%', '78px')}}

### Riferimenti bibliografici

Il contenuto dell'attributo [`cite`](/it/docs/Web/HTML/Reference/Elements/blockquote#cite) sembra utile, ma purtroppo browser, screen reader e così via non ne fanno realmente molto uso. Non esiste un modo per far visualizzare al browser il contenuto di `cite` senza scrivere una soluzione personalizzata usando JavaScript o CSS. Se si desidera rendere disponibile nella pagina la fonte della citazione, occorre renderla disponibile nel testo tramite un link o un altro metodo appropriato.

Esiste un elemento {{htmlelement("cite")}}, ma è pensato per contenere il titolo della risorsa citata, ad esempio il nome del libro. Non c'è tuttavia alcun motivo per cui non si possa collegare in qualche modo il testo all'interno di `<cite>` alla fonte della citazione:

```html-nolint
<p>
  According to the
  <a href="/en-US/docs/Web/HTML/Reference/Elements/blockquote">
    <cite>MDN blockquote page</cite></a>:
</p>

<blockquote
  cite="https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/blockquote">
  <p>
    The <strong>HTML <code>&lt;blockquote&gt;</code> Element</strong> (or
    <em>HTML Block Quotation Element</em>) indicates that the enclosed text is
    an extended quotation.
  </p>
</blockquote>

<p>
  The quote element — <code>&lt;q&gt;</code> — is
  <q cite="https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/q">
    intended for short quotations that don't require paragraph breaks.
  </q>
  — <a href="/en-US/docs/Web/HTML/Reference/Elements/q"><cite>MDN q page</cite></a>.
</p>
```

Per impostazione predefinita, i riferimenti bibliografici sono stilizzati in corsivo.

{{EmbedLiveSample('Citations', '100%', '179px')}}

### Chi l'ha detto? Esercizio sulle citazioni a blocco

È il momento di un altro esercizio. In questo esempio occorre:

1. Fare clic su **"Play"** nel blocco di codice seguente per modificare l'esempio nel MDN Playground.
2. Trasformare il paragrafo centrale in una citazione a blocco, includendo un attributo `cite`.
3. Trasformare "The Need To Eliminate Negative Self Talk" nel terzo paragrafo in una citazione in linea e includere un attributo `cite`.
4. Racchiudere il titolo di ogni fonte in tag `<cite>` e trasformarli entrambi in un link alla rispettiva fonte.

Le fonti delle citazioni necessarie sono:

- `http://www.brainyquote.com/quotes/authors/c/confucius.html` per la citazione di Confucio
- `http://example.com/affirmationsforpositivethinking` per "The Need To Eliminate Negative Self Talk".

Se si commette un errore, è possibile cancellare il lavoro usando il pulsante _Reset_ nel MDN Playground. Se risulta particolarmente difficile, è possibile visualizzare la soluzione sotto il blocco di codice.

```html live-sample___advanced-text-1
<p>Hello and welcome to my motivation page. As Confucius' quotes site says:</p>
<p>It does not matter how slowly you go as long as you do not stop.</p>
<p>
  I also love the concept of positive thinking, and The Need To Eliminate
  Negative Self Talk (as mentioned in Affirmations for Positive Thinking.)
</p>
```

{{ EmbedLiveSample('advanced-text-1', "100%", 200) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

L'HTML completato dovrebbe essere simile a questo:

```html
<p>
  Hello and welcome to my motivation page. As
  <a href="http://www.brainyquote.com/quotes/authors/c/confucius.html"
    ><cite>Confucius' quotes site</cite></a
  >
  says:
</p>

<blockquote cite="http://www.brainyquote.com/quotes/authors/c/confucius.html">
  <p>It does not matter how slowly you go as long as you do not stop.</p>
</blockquote>

<p>
  I also love the concept of positive thinking, and
  <q cite="http://example.com/affirmationsforpositivethinking"
    >The Need To Eliminate Negative Self Talk</q
  >
  (as mentioned in
  <a href="http://example.com/affirmationsforpositivethinking"
    ><cite>Affirmations for Positive Thinking</cite></a
  >.)
</p>
```

</details>

## Abbreviazioni

Un altro elemento abbastanza comune che si può incontrare esplorando il Web è {{htmlelement("abbr")}} — viene usato per racchiudere un'abbreviazione o un acronimo. Quando si include uno dei due, fornire alla prima occorrenza l'espansione completa del termine in testo semplice, insieme a `<abbr>` per contrassegnare l'abbreviazione. Questo fornisce un'indicazione agli user agent su come annunciare o visualizzare il contenuto, informando al contempo tutti gli utenti sul significato dell'abbreviazione.

Se fornire l'espansione oltre all'abbreviazione ha poco senso e l'abbreviazione o l'acronimo è un termine molto contratto, fornire l'espansione completa del termine come valore dell'attributo [`title`](/it/docs/Web/HTML/Reference/Global_attributes/title):

### Esempio di abbreviazione

Vediamo un esempio.

```html
<p>
  We use <abbr>HTML</abbr>, Hypertext Markup Language, to structure our web
  documents.
</p>

<p>
  I think <abbr title="Reverend">Rev.</abbr> Green did it in the kitchen with
  the chainsaw.
</p>
```

Vengono resi nel modo seguente:

{{EmbedLiveSample('Abbreviation_example', '100%', '90')}}

> [!NOTE]
> Le versioni precedenti di HTML includevano anche il supporto per l'elemento {{htmlelement("acronym")}}, ma è stato rimosso dalla specifica HTML in favore dell'uso di `<abbr>` per rappresentare sia abbreviazioni sia acronimi. Non usare `<acronym>`.

### Contrassegnare un'abbreviazione

Per questo esercizio di apprendimento, occorre contrassegnare un'abbreviazione.

1. Fare clic su **"Play"** nel blocco di codice seguente per modificare l'esempio nel MDN Playground.
2. Contrassegnare le abbreviazioni incluse usando HTML appropriato. È anche possibile sostituirle con una propria abbreviazione e provare a contrassegnare quella.

Se si commette un errore, è possibile cancellare il lavoro usando il pulsante _Reset_ nel MDN Playground. Se risulta particolarmente difficile, è possibile visualizzare la soluzione sotto il blocco di codice.

```html-nolint live-sample___advanced-text-2
<p>NASA sure does some exciting work.</p>

<p>The new user interface design LGTM!</p>
```

{{ EmbedLiveSample('advanced-text-2', "100%", 90) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

L'HTML completato dovrebbe essere simile al seguente frammento di codice:

```html
<p>
  <abbr>NASA</abbr> (the National Aeronautics and Space Administration) sure
  does some exciting work.
</p>

<p>The new user interface design <abbr title="Looks good to me">LGTM</abbr>!</p>
```

- Si potrebbe sostenere che NASA dovrebbe essere espanso nel testo alla prima menzione, poiché è un'informazione utile che tutti dovrebbero avere disponibile nel testo.
- Acronimi come "LGTM", invece, sono scritti esclusivamente per risparmiare spazio e tempo, quindi non avrebbe senso scriverne anche l'espansione; per questo l'espansione viene inserita nell'attributo `title`. In un'applicazione reale, probabilmente non si farebbe manualmente: si userebbe invece una sorta di script per aggiungerla automaticamente ai termini noti.

</details>

## Contrassegnare i dettagli di contatto

HTML dispone di un elemento per contrassegnare i dettagli di contatto: {{htmlelement("address")}}. Questo racchiude i dettagli di contatto, ad esempio:

```html
<address>Chris Mills, Manchester, The Grim North, UK</address>
```

Può anche includere markup più complesso e altre forme di informazioni di contatto, ad esempio:

```html
<address>
  <p>
    Chris Mills<br />
    Manchester<br />
    The Grim North<br />
    UK
  </p>

  <ul>
    <li>Tel: 01234 567 890</li>
    <li>Email: me@grim-north.co.uk</li>
  </ul>
</address>
```

Si noti che anche qualcosa di simile sarebbe corretto, se la pagina collegata contenesse le informazioni di contatto:

```html
<address>
  Page written by <a href="../authors/chris-mills/">Chris Mills</a>.
</address>
```

> [!NOTE]
> L'elemento {{htmlelement("address")}} deve essere usato solo per fornire informazioni di contatto per il documento contenuto dall'elemento {{htmlelement("article")}} o {{htmlelement("body")}} più vicino. Sarebbe corretto usarlo nel piè di pagina di un sito per includere le informazioni di contatto dell'intero sito oppure all'interno di un articolo per i dettagli di contatto dell'autore, ma non per contrassegnare un elenco di indirizzi non correlati al contenuto della pagina.

## Apice e pedice

Occasionalmente sarà necessario usare apici e pedici quando si contrassegnano elementi come date, formule chimiche ed equazioni matematiche, affinché abbiano il significato corretto. Gli elementi {{htmlelement("sup")}} e {{htmlelement("sub")}} svolgono questo compito. Ad esempio:

```html
<p>My birthday is on the 25<sup>th</sup> of May 2001.</p>
<p>
  Caffeine's chemical formula is
  C<sub>8</sub>H<sub>10</sub>N<sub>4</sub>O<sub>2</sub>.
</p>
<p>If x<sup>2</sup> is 9, x must equal 3 or -3.</p>
```

L'output di questo codice è il seguente:

{{ EmbedLiveSample('Superscript_and_subscript', '100%', 160) }}

## Rappresentare il codice informatico

Sono disponibili diversi elementi per contrassegnare il codice informatico usando HTML:

- {{htmlelement("code")}}: per contrassegnare frammenti generici di codice informatico.
- {{htmlelement("pre")}}: per conservare gli spazi vuoti, generalmente nei blocchi di codice. Se si usa l'indentazione o spazi vuoti aggiuntivi nel testo, i browser li ignorano e non saranno visibili nella pagina renderizzata. Tuttavia, se il testo viene racchiuso nei tag `<pre></pre>`, gli spazi vuoti verranno resi in modo identico a come appaiono nell'editor di testo.
- {{htmlelement("var")}}: per contrassegnare specificamente i nomi delle variabili.
- {{htmlelement("kbd")}}: per contrassegnare l'input da tastiera, e altri tipi di input, inserito nel computer.
- {{htmlelement("samp")}}: per contrassegnare l'output di un programma informatico.

Vediamo esempi di questi elementi e di come vengono usati per rappresentare il codice informatico:

```html
<pre><code>const para = document.querySelector('p');

para.onclick = function() {
  alert('Owww, stop poking me!');
}</code></pre>

<p>
  You shouldn't use presentational elements like <code>&lt;font&gt;</code> and
  <code>&lt;center&gt;</code>.
</p>

<p>
  In the above JavaScript example, <var>para</var> represents a paragraph
  element.
</p>

<p>Select all the text with <kbd>Ctrl</kbd>/<kbd>Cmd</kbd> + <kbd>A</kbd>.</p>

<pre>$ <kbd>ping mozilla.org</kbd>
<samp>PING mozilla.org (63.245.215.20): 56 data bytes
64 bytes from 63.245.215.20: icmp_seq=0 ttl=40 time=158.233 ms</samp></pre>
```

Il codice precedente viene reso nel modo seguente:

{{ EmbedLiveSample('Representing_computer_code','100%',350) }}

## Contrassegnare ore e date

HTML fornisce anche l'elemento {{htmlelement("time")}} per contrassegnare ore e date in un formato leggibile dalle macchine. Ad esempio:

```html
<time datetime="2016-01-20">20 January 2016</time>
```

Perché è utile? Gli esseri umani scrivono le date in molti modi diversi. La data precedente potrebbe essere scritta come:

<!-- markdownlint-disable MD033 -->

- 20 January 2016
- 20th January 2016
- Jan 20 2016
- 20/01/16
- 01/20/16
- The 20th of next month
- <span lang="fr">20e Janvier 2016</span>
- <span lang="ja">2016 年 1 月 20 日</span>
- E così via.

<!-- markdownlint-enable MD033 -->

Tuttavia, queste diverse forme non possono essere riconosciute facilmente dai computer: cosa accadrebbe se fosse necessario recuperare automaticamente le date di tutti gli eventi in una pagina e inserirle in un calendario? L'elemento {{htmlelement("time")}} consente di associare a questo scopo un'ora o una data non ambigua e leggibile dalle macchine.

L'esempio di base precedente fornisce semplicemente una data leggibile dalle macchine, ma sono possibili molte altre opzioni, ad esempio:

```html
<!-- Standard simple date -->
<time datetime="2016-01-20">20 January 2016</time>
<!-- Just year and month -->
<time datetime="2016-01">January 2016</time>
<!-- Just month and day -->
<time datetime="01-20">20 January</time>
<!-- Just time, hours and minutes -->
<time datetime="19:30">19:30</time>
<!-- You can do seconds and milliseconds too! -->
<time datetime="19:30:01.856">19:30:01.856</time>
<!-- Date and time -->
<time datetime="2016-01-20T19:30">7.30pm, 20 January 2016</time>
<!-- Date and time with timezone offset -->
<time datetime="2016-01-20T19:30+01:00">
  7.30pm, 20 January 2016 is 8.30pm in France
</time>
<!-- Calling out a specific week number -->
<time datetime="2016-W04">The fourth week of 2016</time>
```

## Riepilogo

Questo conclude lo studio della semantica del testo HTML meno comune. Quanto visto durante questo corso non è un elenco esaustivo degli elementi di testo HTML: l'obiettivo era trattare gli elementi essenziali e alcuni di quelli più comuni che si incontrano sul Web.

Successivamente verranno proposti alcuni test utilizzabili per verificare quanto bene sono state comprese e memorizzate le informazioni fornite sulle funzionalità di testo HTML meno comuni.

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Test_your_skills/HTML_text_basics", "Learn_web_development/Core/Structuring_content/Test_your_skills/Advanced_HTML_text", "Learn_web_development/Core/Structuring_content")}}
