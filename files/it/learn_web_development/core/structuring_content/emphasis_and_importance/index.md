---
title: Enfasi e importanza
slug: Learn_web_development/Core/Structuring_content/Emphasis_and_importance
l10n:
  sourceCommit: cc7ed25d67ec3df5df8cfa255e1066cb5845e293
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Headings_and_paragraphs", "Learn_web_development/Core/Structuring_content/Lists", "Learn_web_development/Core/Structuring_content")}}

L'articolo precedente ha esaminato perché la semantica è importante in HTML, concentrandosi su titoli e paragrafi. Questo articolo prosegue il tema della semantica, esaminando gli elementi HTML che applicano enfasi e importanza al testo (in parallelo con il corsivo e il grassetto nei supporti di stampa).

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Conoscenza di base di HTML, come trattato in
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi HTML di base</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Il significato di enfasi e importanza e gli elementi di base che li applicano in HTML, come <code>&lt;em&gt;</code> e <code>&lt;strong&gt;</code>.</li>
          <li>Identificare il markup di presentazione che non dovrebbe più essere usato affatto (ad esempio, <code>&lt;big&gt;</code> e <code>&lt;font&gt;</code>); è deprecato.</li>
          <li>Identificare il markup di presentazione a cui è stato assegnato un nuovo significato semantico (ad esempio, <code>&lt;i&gt;</code> e <code>&lt;b&gt;</code>).</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Cosa sono enfasi e importanza?

Nel linguaggio umano, spesso si pone l'enfasi su determinate parole per modificare il significato di una frase e, spesso, si desidera contrassegnare certe parole come importanti o in qualche modo diverse. HTML fornisce vari elementi semantici per consentire di marcare il contenuto testuale con tali effetti e, in questa sezione, verranno esaminati alcuni dei più comuni.

### Enfasi

Quando si desidera aggiungere enfasi nel linguaggio parlato, si _accentuano_ certe parole, modificando sottilmente il significato di ciò che viene detto. Analogamente, nel linguaggio scritto si tende ad accentuare le parole mettendole in corsivo. Per esempio, le due frasi seguenti hanno significati diversi.

> Sono contento che non fossi in ritardo.
>
> Sono _contento_ che non fossi in _ritardo_.

La prima frase sembra esprimere un sollievo genuino perché la persona non era in ritardo. Al contrario, la seconda, con entrambe le parole "contento" e "ritardo" in corsivo, suona sarcastica o passivo-aggressiva, esprimendo fastidio perché la persona è arrivata con un po' di ritardo.

In HTML si usa l'elemento {{htmlelement("em")}} (enfasi) per marcare questi casi. Oltre a rendere il documento più interessante da leggere, questi elementi sono riconosciuti dagli screen reader, che possono essere configurati per pronunciarli con un tono di voce diverso. Per impostazione predefinita, i browser applicano il corsivo, ma questo tag non dovrebbe essere usato esclusivamente per ottenere lo stile corsivo. A questo scopo, si userebbe un elemento {{htmlelement("span")}} e del CSS, oppure forse un elemento {{htmlelement("i")}} (vedere di seguito).

```html
<p>I am <em>glad</em> you weren't <em>late</em>.</p>
```

### Forte importanza

Per enfatizzare parole importanti, si tende ad accentuarle nel linguaggio parlato e a scriverle in **grassetto** nel linguaggio scritto. Per esempio:

> Questo liquido è **altamente tossico**.
>
> Conto su di te. **Non** fare tardi!

In HTML si usa l'elemento {{htmlelement("strong")}} (forte importanza) per marcare questi casi. Oltre a rendere il documento più utile, anche questi elementi sono riconosciuti dagli screen reader, che possono essere configurati per pronunciarli con un tono di voce diverso. Per impostazione predefinita, i browser applicano il testo in grassetto, ma questo tag non dovrebbe essere usato esclusivamente per ottenere lo stile in grassetto. A questo scopo, si userebbe un elemento {{htmlelement("span")}} e del CSS, oppure forse un elemento {{htmlelement("b")}} (vedere di seguito).

```html
<p>This liquid is <strong>highly toxic</strong>.</p>

<p>I am counting on you. <strong>Do not</strong> be late!</p>
```

Se lo si desidera, è possibile annidare `strong` ed enfasi l'uno dentro l'altro:

```html-nolint
<p>This liquid is <strong>highly toxic</strong> — if you drink it, <strong>you may <em>die</em></strong>.</p>
```

{{EmbedLiveSample('Strong importance')}}

## Esercitiamoci con enfasi e importanza

In questa sezione, è necessario esercitarsi con enfasi e importanza:

1. Fare clic su **"Play"** nel blocco di codice seguente per modificare l'esempio nel MDN Playground.
2. Nel titolo principale, dare enfasi alla parola "Emphasis" e forte importanza alla parola "importance".
3. Nel primo paragrafo, dare forte importanza al nome della macchina da caffè ed enfatizzare gli aggettivi usati per descrivere il caffè.
4. Nel secondo paragrafo, dare forte importanza alla descrizione della temperatura ("cold") e all'azione da intraprendere ("wrap up warm to avoid falling ill"). Dare a "falling ill" un markup aggiuntivo affinché sia sia enfatizzato sia importante.

In caso di errore, è possibile cancellare il lavoro usando il pulsante _Reset_ nel MDN Playground. Se si rimane davvero bloccati, è possibile visualizzare la soluzione sotto il blocco di codice.

```css hidden live-sample___emphasis_importance
h1 {
  font-weight: normal;
}
```

```html live-sample___emphasis_importance
<h1>Emphasis and importance</h1>

<p>
  My new coffee machine is called The Percolator 2000. It produces the most
  sublime and wonderful brew.
</p>

<p>
  In the dead of winter, it will be cold. You should wrap up warm to avoid
  falling ill.
</p>
```

{{ EmbedLiveSample('emphasis_importance', "100%", 160) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

L'HTML completato dovrebbe avere questo aspetto:

```html
<h1><em>Emphasis</em> and <strong>importance</strong></h1>

<p>
  My new coffee machine is called <strong>The Percolator 2000</strong>. It
  produces the most <em>sublime</em> and <em>wonderful</em> brew.
</p>

<p>
  In the dead of winter, it will be <strong>cold</strong>. You should
  <strong>wrap up warm to avoid <em>falling ill</em></strong
  >.
</p>
```

</details>

## Corsivo, grassetto, sottolineatura…

Gli elementi discussi finora hanno una semantica associata ben definita. La situazione con {{htmlelement("b")}}, {{htmlelement("i")}} e {{htmlelement("u")}} è un po' più complicata. Sono nati per consentire di scrivere testo in grassetto, corsivo o sottolineato in un'epoca in cui CSS era ancora scarsamente supportato o non era supportato affatto. Elementi di questo tipo, che influenzano solo la presentazione e non la semantica, sono noti come **elementi di presentazione** e non dovrebbero più essere usati perché, come visto in precedenza, la semantica è molto importante per l'accessibilità, la SEO e così via.

HTML5 ha ridefinito `<b>`, `<i>` e `<u>` assegnando loro nuovi ruoli semantici, in qualche modo confusi.

Ecco la regola migliore da ricordare: è appropriato usare `<b>`, `<i>` o `<u>` solo per trasmettere un significato tradizionalmente comunicato con grassetto, corsivo o sottolineatura quando non esiste un elemento più adatto; e di solito ne esiste uno. Considerare se `<strong>`, `<em>`, `<mark>` o `<span>` possano essere più appropriati.

Mantenere sempre una prospettiva orientata all'accessibilità. Il concetto di corsivo non è molto utile per le persone che usano screen reader o per quelle che usano un sistema di scrittura diverso dall'alfabeto latino.

- {{HTMLElement('i')}} viene usato per trasmettere un significato tradizionalmente comunicato dal corsivo: parole straniere, designazione tassonomica, termini tecnici, un pensiero…
- {{HTMLElement('b')}} viene usato per trasmettere un significato tradizionalmente comunicato dal grassetto: parole chiave, nomi di prodotti, frase introduttiva…
- {{HTMLElement('u')}} viene usato per trasmettere un significato tradizionalmente comunicato dalla sottolineatura: nome proprio, errore ortografico…

> [!NOTE]
> Le persone associano fortemente la sottolineatura ai collegamenti ipertestuali. Perciò, sul web, è preferibile sottolineare solo i collegamenti. Usare l'elemento `<u>` quando è semanticamente appropriato, ma considerare l'uso di CSS per modificare la sottolineatura predefinita in qualcosa di più adatto al web. L'esempio seguente illustra come farlo.

<!-- cSpell:ignore spel -->

```html
<!-- scientific names -->
<p>
  The Ruby-throated Hummingbird (<i>Archilochus colubris</i>) is the most common
  hummingbird in Eastern North America.
</p>

<!-- foreign words -->
<p>
  The menu was a sea of exotic words like <i lang="uk-latn">vatrushka</i>,
  <i lang="id">nasi goreng</i> and <i lang="fr">soupe à l'oignon</i>.
</p>

<!-- a known misspelling -->
<p>Someday I'll learn how to <u class="spelling-error">spel</u> better.</p>

<!-- term being defined when used in a definition -->
<dl>
  <dt>Semantic HTML</dt>
  <dd>
    Use the elements based on their <b>semantic</b> meaning, not their
    appearance.
  </dd>
</dl>
```

{{EmbedLiveSample('Italic, bold, underline…','100%','270')}}

## Riepilogo

Per il momento è concluso l'approfondimento su enfasi e importanza. Si passa ora a esaminare come rappresentare gli elenchi in HTML.

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Headings_and_paragraphs", "Learn_web_development/Core/Structuring_content/Lists", "Learn_web_development/Core/Structuring_content")}}
