---
title: Contenuto in overflow
short-title: Overflow
slug: Learn_web_development/Core/Styling_basics/Overflow
l10n:
  sourceCommit: 936233e89fd5714c957c5931b26dfb56c64f9a91
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Test_your_skills/Backgrounds_and_borders", "Learn_web_development/Core/Styling_basics/Test_your_skills/Overflow", "Learn_web_development/Core/Styling_basics")}}

L'overflow si verifica quando c'è troppo contenuto per entrare all'interno del riquadro di un elemento. In questa lezione, verrà illustrato come gestire l'overflow usando CSS.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Nozioni di base di HTML (studiare
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi HTML di base</a
        >), <a href="/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units">Valori e unità</a> di CSS e <a href="/it/docs/Learn_web_development/Core/Styling_basics/Sizing">Dimensionamento</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere che cos'è l'overflow.</li>
          <li>Controllare l'overflow con la proprietà <code>overflow</code>. </li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Che cos'è l'overflow?

In CSS tutto è un riquadro. È possibile vincolare la dimensione di questi riquadri impostando valori per proprietà come {{cssxref("width")}} e {{cssxref("height")}}. **L'overflow si verifica quando c'è troppo contenuto per entrare in un riquadro.** CSS fornisce vari strumenti per gestire l'overflow. Procedendo con il layout CSS e la scrittura di CSS, si incontreranno ulteriori situazioni di overflow.

## CSS cerca di evitare la "perdita di dati"

Consideriamo due esempi che dimostrano il comportamento predefinito dell'overflow in CSS.

Il primo esempio presenta un riquadro che è stato limitato impostando una `height`. Il contenuto del riquadro supera lo spazio disponibile; pertanto, fuoriesce dal riquadro e si sovrappone al paragrafo sottostante.

```html live-sample___block-overflow
<div class="box">
  This box has a height and a width. This means that if there is too much
  content to be displayed within the assigned height, there will be an overflow
  situation. If overflow is set to hidden, then any overflow will not be
  visible.
</div>

<p>This content is outside of the box.</p>
```

```css live-sample___block-overflow
.box {
  border: 1px solid #333333;
  width: 250px;
  height: 100px;
}
```

{{EmbedLiveSample("block-overflow", "", "200px")}}

Il secondo esempio presenta una parola in un riquadro. La dimensione del riquadro è impostata troppo piccola per la parola, quindi la parola fuoriesce dal riquadro.

```html live-sample___inline-overflow
<div class="word">Overflow</div>
```

```css live-sample___inline-overflow
.word {
  border: 1px solid #333333;
  width: 100px;
  font-size: 250%;
}
```

{{EmbedLiveSample("inline-overflow")}}

Ci si potrebbe chiedere perché CSS funzioni in modo così disordinato, visualizzando il contenuto al di fuori del contenitore previsto. Perché non nascondere il contenuto in overflow? Perché non ridimensionare il contenitore in modo da adattarlo a tutto il contenuto?

Ove possibile, CSS non nasconde il contenuto. Ciò causerebbe una perdita di dati. Il problema della perdita di dati è che lo sviluppatore, o i visitatori del sito web, potrebbero non accorgersene. Se il pulsante di invio di un modulo scompare e nessuno può completare il modulo, questo potrebbe essere un grosso problema. CSS, invece, mostra l'overflow in modi visibili. È più probabile che un problema venga notato. Nel peggiore dei casi, un visitatore del sito segnalerà che il contenuto si sovrappone.

Se si limita un riquadro con una `width` o una `height`, CSS presume che si sappia cosa si sta facendo. CSS presume che venga gestita la possibilità di overflow. In generale, limitare la dimensione del blocco è problematico quando il riquadro contiene testo. Potrebbe esserci più testo del previsto durante la progettazione del sito, oppure il testo potrebbe essere più grande (ad esempio, se l'utente ha aumentato la dimensione del carattere).

## La proprietà overflow

La proprietà {{cssxref("overflow")}} consente di specificare come il browser deve gestire il contenuto in overflow. Il suo valore predefinito è `visible`, il che significa che il contenuto è visibile quando fuoriesce.

I due valori seguenti di `overflow` forniscono il comportamento necessario per risolvere la maggior parte dei problemi di overflow:

- `overflow: clip` taglia il contenuto in overflow, in modo che non sia mai visibile.
- `overflow: auto` visualizza le barre di scorrimento solo quando necessario, consentendo all'utente di scorrere i riquadri per leggere il contenuto in overflow.

Le due sezioni successive illustrano come usare questi valori. In seguito, verranno esaminati altri valori di `overflow` e verrà spiegato come controllare separatamente l'overflow degli assi x e y.

## Nascondere il contenuto in overflow

Per tagliare il contenuto quando fuoriesce, impostare `overflow: clip`. Tutto ciò che non entra viene tagliato al bordo del riquadro e non può essere raggiunto. Ciò significa che parte del contenuto diventa invisibile, quindi questa impostazione va usata solo quando nascondere il contenuto non causa problemi.

```html live-sample___clip
<div class="box">
  This box has a height and a width. This means that if there is too much
  content to be displayed within the assigned height, there will be an overflow
  situation. If overflow is set to clip, then any overflow will not be visible.
</div>

<p>This content is outside of the box.</p>
```

```css live-sample___clip
.box {
  border: 1px solid #333333;
  width: 250px;
  height: 100px;
  overflow: clip;
}
```

{{EmbedLiveSample("clip", "", "200px")}}

Provare a modificare questo esempio impostando `overflow` su `visible`, quindi di nuovo su `clip`, per osservare l'effetto.

> [!NOTE]
> Per impostazione predefinita, `clip` taglia il contenuto al bordo del riquadro. La proprietà {{cssxref("overflow-clip-margin")}} sposta verso l'esterno tale bordo di ritaglio, consentendo a una quantità specificata del contenuto in overflow di rimanere visibile prima che il resto venga tagliato.

## Scorrere il contenuto in overflow

In alternativa, potrebbe essere opportuno consentire agli utenti di scorrere il contenuto per leggerlo interamente. Impostando `overflow: auto`, il riquadro diventa scorrevole e i browser con barre di scorrimento visibili mostrano una barra di scorrimento solo quando il contenuto è effettivamente troppo grande per entrare.

Nell'esempio seguente, rimuovere del contenuto dal `<div>` finché non è più in overflow. La barra di scorrimento dovrebbe scomparire:

```html live-sample___auto
<div class="box">
  This box has a height and a width. This means that if there is too much
  content to be displayed within the assigned height, there will be an overflow
  situation. If overflow is set to auto, then scrollbars appear only when
  needed.
</div>

<p>This content is outside of the box.</p>
```

```css live-sample___auto
.box {
  border: 1px solid #333333;
  width: 250px;
  height: 100px;
  overflow: auto;
}
```

{{EmbedLiveSample("auto", "", "200px")}}

> [!NOTE]
> La visibilità delle barre di scorrimento dipende dal sistema operativo.
> Potrebbe essere necessario modificare le impostazioni del browser affinché le barre di scorrimento vengano sempre mostrate nei seguenti esempi.

## Controllare l'overflow su ciascun asse

Specificare una singola parola chiave come valore della proprietà `overflow` imposta il comportamento dell'overflow per gli assi x _e_ y di un contenitore. Nell'esempio precedente, se si imposta `overflow` su `scroll` (che fa [apparire sempre](#visualizzare_sempre_le_barre_di_scorrimento) le barre di scorrimento, indipendentemente dal fatto che il contenuto sia in overflow), saranno visibili barre di scorrimento su entrambi gli assi. Per controllare gli assi separatamente, usare le proprietà {{cssxref("overflow-x")}} e {{cssxref("overflow-y")}}. Provare a impostare `overflow-y: auto` nell'esempio.

L'esempio successivo dimostra l'abilitazione dello scorrimento lungo l'asse x con `overflow-x`, sebbene questa soluzione non sia consigliata per gestire parole lunghe. Se è presente una parola lunga in un riquadro piccolo, prendere in considerazione l'uso delle proprietà {{cssxref("word-break")}} o {{cssxref("overflow-wrap")}} per spezzare la parola su più righe. Inoltre, alcuni dei metodi trattati in [Dimensionamento degli elementi in CSS](/it/docs/Learn_web_development/Core/Styling_basics/Sizing) possono aiutare a creare riquadri che si adattano meglio a quantità di contenuto variabili.

```html live-sample___scroll-x
<div class="word">Overflow</div>
```

```css live-sample___scroll-x
.word {
  border: 5px solid #333333;
  width: 100px;
  font-size: 250%;
  overflow-x: auto;
}
```

{{EmbedLiveSample("scroll-x")}}

> [!NOTE]
> È inoltre possibile specificare separatamente l'overflow degli assi x e y passando due valori parola chiave alla proprietà `overflow`: il primo si applica a `overflow-x` e il secondo a `overflow-y`. Ad esempio, `overflow: clip auto` imposterebbe `overflow-x` su `clip` e `overflow-y` su `auto`.

`clip` è l'unico valore che può essere combinato con `visible` sull'altro asse. Se si imposta un asse su un valore di scorrimento (`auto`, `scroll` o `hidden`) e l'altro su `visible`, il valore `visible` viene invece calcolato come `auto`, poiché un riquadro non può scorrere su un asse lasciando al contempo fuoriuscire il contenuto dall'altro. Quindi `overflow: clip visible` taglia orizzontalmente e consente al contenuto di fuoriuscire verticalmente, mentre `overflow: hidden visible` si comporta come `overflow: hidden auto`.

## Visualizzare sempre le barre di scorrimento

Impostando `overflow: scroll`, il riquadro diventa scorrevole come con `overflow: auto`, con la differenza che i browser con barre di scorrimento visibili le mostrano sempre, anche quando il contenuto non è in overflow.

Il motivo principale per usare `scroll` è la coerenza del layout: la barra di scorrimento è sempre presente; pertanto, il contenuto non si sposta quando la quantità di contenuto passa da in overflow a non in overflow. Tuttavia, in un caso simile, combinare `overflow: auto` con un valore `stable` per {{cssxref("scrollbar-gutter")}} è solitamente più appropriato, poiché riserva lo spazio senza forzare il disegno di una barra di scorrimento.

## Il valore hidden

Spesso si incontrerà `overflow: hidden` nel codice esistente. Come `clip`, taglia il contenuto in overflow e non visualizza barre di scorrimento. Diversamente da `clip`, trasforma comunque il riquadro in un contenitore scorrevole e il contenuto può essere fatto scorrere con altri mezzi, ad esempio usando [JavaScript](/it/docs/Learn_web_development/Core/Scripting) o premendo Tab fino a un elemento attivabile più avanti nel contenuto, come un [elemento link](/it/docs/Learn_web_development/Core/Structuring_content/Creating_links).

Nella maggior parte dei casi è opportuno usare il valore `clip`; `hidden` è necessario solo se occorre il comportamento scorrevole descritto sopra.

## Overflow indesiderato nel web design

I moderni metodi di layout (che verranno affrontati più avanti nel modulo [layout CSS](/it/docs/Learn_web_development/Core/CSS_layout)) gestiscono l'overflow. Funzionano in gran parte senza fare supposizioni o avere dipendenze sulla quantità di contenuto presente in una pagina web.

Non è sempre stato così. In passato, alcuni siti venivano creati con contenitori ad altezza fissa per allineare i bordi inferiori dei riquadri. Questi riquadri potrebbero altrimenti non aver avuto alcuna relazione reciproca. Questa soluzione era fragile. Se si incontra un riquadro il cui contenuto si sovrappone ad altro contenuto, ora sarà possibile riconoscere che la causa potrebbe essere l'overflow. Idealmente, il layout dovrebbe essere ristrutturato per evitare contenitori ad altezza fissa.

Durante lo sviluppo di un sito, tenere sempre presente l'overflow. Testare i progetti con quantità grandi e piccole di contenuto. Aumentare e diminuire le dimensioni dei caratteri di almeno due incrementi. Assicurarsi che il CSS sia robusto. La modifica dei valori di overflow per nascondere il contenuto o aggiungere barre di scorrimento è riservata a pochi casi d'uso selezionati (ad esempio, quando si desidera che un riquadro scorrevole visualizzi sempre le barre di scorrimento).

## Riepilogo

Questa lezione ha introdotto il concetto di overflow. Per impostazione predefinita, CSS evita di rendere invisibile il contenuto in overflow. È possibile gestire il potenziale overflow e occorre testare il proprio lavoro per assicurarsi che non causi accidentalmente overflow problematici.

Nel prossimo articolo verranno proposti alcuni test utilizzabili per verificare quanto bene siano state comprese e memorizzate le informazioni fornite sull'overflow.

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Test_your_skills/Backgrounds_and_borders", "Learn_web_development/Core/Styling_basics/Test_your_skills/Overflow", "Learn_web_development/Core/Styling_basics")}}
