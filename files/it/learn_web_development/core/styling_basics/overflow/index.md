---
title: Contenuto traboccante
short-title: Overflow
slug: Learn_web_development/Core/Styling_basics/Overflow
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Backgrounds_and_borders", "Learn_web_development/Core/Styling_basics/Images_media_forms", "Learn_web_development/Core/Styling_basics")}}

Il trabocco è ciò che accade quando c'è troppo contenuto per adattarsi all'interno di una scatola elemento. In questa lezione, imparerai come gestire il trabocco usando CSS.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Basi di HTML (studiare
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi HTML di base</a
        >), CSS <a href="/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units">Valori e unità</a> e <a href="/it/docs/Learn_web_development/Core/Styling_basics/Sizing">Dimensionamento</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere cos'è il trabocco.</li>
          <li>Controllare il trabocco con la proprietà <code>overflow</code>.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Cos'è il trabocco?

Tutto in CSS è una scatola. È possibile vincolare la dimensione di queste scatole assegnando valori come {{cssxref("width")}} e {{cssxref("height")}}. **Il trabocco avviene quando c'è troppo contenuto per entrare in una scatola.** CSS offre vari strumenti per gestire il trabocco. Man mano che si approfondisce il layout e la scrittura CSS, ci si imbatterà in situazioni di trabocco più frequentemente.

## Il CSS cerca di evitare la "perdita di dati"

Consideriamo due esempi che dimostrano il comportamento predefinito di CSS quando c'è un trabocco.

Il primo esempio è una scatola che è stata limitata impostando un `height`. Quindi aggiungiamo contenuto che supera lo spazio assegnato. Il contenuto trabocca dalla scatola e si riversa nel paragrafo sottostante.

```html live-sample___block-overflow
<div class="box">
  This box has a height and a width. This means that if there is too much
  content to be displayed within the assigned height, there will be an overflow
  situation. If overflow is set to hidden then any overflow will not be visible.
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

Il secondo esempio è una parola in una scatola. La scatola è stata resa troppo piccola per la parola e quindi questa si interrompe fuori dalla scatola.

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

Potresti chiederti perché il CSS funziona in modo così disordinato, mostrando il contenuto al di fuori del suo contenitore previsto. Perché non nascondere il contenuto che trabocca? Perché non ridimensionare il contenitore per adattarlo a tutto il contenuto?

Ovunque possibile, CSS non nasconde il contenuto. Questo causerebbe una perdita di dati. Il problema con la perdita di dati è che potresti non accorgertene. I visitatori del sito potrebbero non accorgersene. Se il pulsante invia di un modulo scompare e nessuno può completare il modulo, questo potrebbe essere un grande problema! Invece, CSS trabocca in modi visibili. È più probabile che tu veda che c'è un problema. Nei casi peggiori, un visitatore del sito ti farà sapere che il contenuto si sta sovrapponendo.

Se si restringe una scatola con una `width` o una `height`, CSS si fida che tu sappia cosa stai facendo. CSS presume che tu stia gestendo il potenziale trabocco. In generale, limitare la dimensione del blocco è problematico quando la scatola contiene testo. Potrebbe esserci più testo di quanto ci si aspettasse durante la progettazione del sito, o il testo potrebbe essere più grande (per esempio se l'utente ha aumentato la dimensione del carattere).

## La proprietà overflow

La proprietà {{cssxref("overflow")}} ti aiuta a gestire il trabocco del contenuto di un elemento. Usando questa proprietà, puoi comunicare a un browser come dovrebbe gestire il contenuto traboccante. Il valore predefinito del tipo di valore [`<overflow>`](/it/docs/Web/CSS/overflow_value) è `visible`. Con questo impostazione predefinita, è possibile vedere il contenuto quando trabocca.

### Nascondere il contenuto traboccante

Per nascondere il contenuto quando trabocca, puoi impostare `overflow: hidden`. Questo fa esattamente ciò che dice: nasconde il trabocco. Fai attenzione perché questo può rendere invisibile parte del contenuto. Dovresti farlo solo se nascondere il contenuto non causerà problemi.

```html live-sample___hidden
<div class="box">
  This box has a height and a width. This means that if there is too much
  content to be displayed within the assigned height, there will be an overflow
  situation. If overflow is set to hidden then any overflow will not be visible.
</div>

<p>This content is outside of the box.</p>
```

```css live-sample___hidden
.box {
  border: 1px solid #333333;
  width: 250px;
  height: 100px;
  overflow: hidden;
}
```

{{EmbedLiveSample("hidden", "", "200px")}}

### Scorrere il contenuto traboccante

In alternativa, forse vorresti aggiungere barre di scorrimento quando il contenuto trabocca? Usando `overflow: scroll`, i browser con barre di scorrimento visibili le mostreranno sempre—anche se non c'è abbastanza contenuto da traboccare. Questo offre il vantaggio di mantenere il layout coerente, invece che le barre di scorrimento appaiano o scompaiano, a seconda della quantità di contenuto nel contenitore.

Rimuovi un po' di contenuto dalla scatola qui sotto. Nota come le barre di scorrimento rimangono, anche se non c'è bisogno di scorrere:

> [!NOTE]
> La visibilità della barra di scorrimento dipende dal sistema operativo.
> Potresti dover modificare le impostazioni del tuo browser per mostrare sempre le barre di scorrimento affinché compaiano sempre negli esempi seguenti.

```html live-sample___scroll
<div class="box">
  This box has a height and a width. This means that if there is too much
  content to be displayed within the assigned height, there will be an overflow
  situation. If overflow is set to hidden then any overflow will not be visible.
</div>

<p>This content is outside of the box.</p>
```

```css live-sample___scroll
.box {
  border: 1px solid #333333;
  width: 250px;
  height: 100px;
  overflow: scroll;
}
```

{{EmbedLiveSample("scroll", "", "200px")}}

Nell'esempio sopra, abbiamo bisogno di scorrere solo sull'asse `y`, tuttavia otteniamo barre di scorrimento su entrambi gli assi. Per scorrere solo sull'asse `y`, potresti usare la proprietà {{cssxref("overflow-y")}}, impostando `overflow-y: scroll`.

```html live-sample___scroll-y
<div class="box">
  This box has a height and a width. This means that if there is too much
  content to be displayed within the assigned height, there will be an overflow
  situation. If overflow is set to hidden then any overflow will not be visible.
</div>

<p>This content is outside of the box.</p>
```

```css live-sample___scroll-y
.box {
  border: 1px solid #333333;
  width: 250px;
  height: 100px;
  overflow-y: scroll;
}
```

{{EmbedLiveSample("scroll-y", "", "200px")}}

Puoi anche abilitare lo scorrimento lungo l'asse x utilizzando {{cssxref("overflow-x")}}, anche se questo non è un modo raccomandato per gestire parole lunghe! Se hai una parola lunga in una scatola piccola, considera l'uso delle proprietà {{cssxref("word-break")}} o {{cssxref("overflow-wrap")}}. Inoltre, alcuni dei metodi discussi in [Dimensionamento degli elementi in CSS](/it/docs/Learn_web_development/Core/Styling_basics/Sizing) possono aiutarti a creare scatole che si adattano meglio a quantità variabili di contenuto.

```html live-sample___scroll-x
<div class="word">Overflow</div>
```

```css live-sample___scroll-x
.word {
  border: 5px solid #333333;
  width: 100px;
  font-size: 250%;
  overflow-x: scroll;
}
```

{{EmbedLiveSample("scroll-x")}}

Come con `scroll`, ottieni una barra di scorrimento nella dimensione di scorrimento indipendentemente dal fatto che ci sia abbastanza contenuto per causare una barra di scorrimento.

> [!NOTE]
> È possibile specificare lo scorrimento su x e y utilizzando la proprietà `overflow`, passando due valori. Se si specificano due parole chiave, la prima si applica a `overflow-x` e la seconda si applica a `overflow-y`. Altrimenti, `overflow-x` e `overflow-y` vengono impostati sullo stesso valore. Ad esempio, `overflow: scroll hidden` imposterà `overflow-x` su `scroll` e `overflow-y` su `hidden`.

### Mostrare le barre di scorrimento solo quando necessario

Se desideri che le barre di scorrimento appaiano solo quando c'è più contenuto di quello che può stare nella scatola, usa `overflow: auto`. Questo permette al browser di determinare se mostrare le barre di scorrimento.

Nell'esempio seguente, rimuovi il contenuto finché non si adatta nella scatola. Dovresti vedere le barre di scorrimento scomparire:

```html live-sample___auto
<div class="box">
  This box has a height and a width. This means that if there is too much
  content to be displayed within the assigned height, there will be an overflow
  situation. If overflow is set to hidden then any overflow will not be visible.
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

## Trabocco indesiderato nel design web

I metodi di layout moderni (che incontrerai più avanti nel modulo [Layout CSS](/it/docs/Learn_web_development/Core/CSS_layout)) gestiscono il trabocco. Funzionano in gran parte senza ipotesi o dipendenze su quanto contenuto ci sarà su una pagina web.

Questo non è sempre stato lo standard. In passato, alcuni siti sono stati costruiti con contenitori a altezza fissa per allineare il fondo delle scatole. Queste scatole potrebbero altrimenti non avere una relazione tra loro. Questo era fragile. Se incontri una scatola dove il contenuto si sovrappone ad altro contenuto sulla pagina nelle applicazioni legacy, ora riconoscerai che ciò accade con il trabocco. Idealmente, rifattorerai il layout per evitare di dipendere da contenitori a altezza fissa.

Quando sviluppi un sito, tieni sempre presente il trabocco. Testa i design con grandi e piccole quantità di contenuto. Aumenta e diminuisci le dimensioni del carattere di almeno due incrementi. Assicurati che il tuo CSS sia robusto. Cambiare i valori di trabocco per nascondere il contenuto o aggiungere barre di scorrimento è riservato a un numero limitato di casi d'uso selezionati (ad esempio, dove intendi avere una scatola di scorrimento).

## Metti alla prova le tue abilità!

Sei arrivato alla fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare alcuni ulteriori test per verificare che tu abbia conservato queste informazioni prima di procedere — vedi [Metti alla prova le tue abilità: Overflow](/it/docs/Learn_web_development/Core/Styling_basics/Test_your_skills/Overflow).

## Riassunto

Questa lezione ha introdotto il concetto di trabocco. Dovresti capire che il CSS predefinito evita di rendere invisibile il contenuto traboccante. Hai scoperto che puoi gestire il potenziale trabocco e anche che dovresti testare il tuo lavoro per assicurarti che non causi accidentalmente un trabocco problematico.

Nel prossimo articolo, esamineremo come gestire lo stile di funzionalità speciali della pagina come immagini ed elementi del modulo.

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Backgrounds_and_borders", "Learn_web_development/Core/Styling_basics/Images_media_forms", "Learn_web_development/Core/Styling_basics")}}
