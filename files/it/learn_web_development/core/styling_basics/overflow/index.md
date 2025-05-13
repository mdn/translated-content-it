---
title: Contenuto traboccante
short-title: Overflow
slug: Learn_web_development/Core/Styling_basics/Overflow
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Backgrounds_and_borders", "Learn_web_development/Core/Styling_basics/Images_media_forms", "Learn_web_development/Core/Styling_basics")}}

Il trabocco è ciò che accade quando c'è troppo contenuto per adattarsi all'interno di una scatola di un elemento. In questa lezione, imparerà a gestire il trabocco utilizzando CSS.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Nozioni di base su HTML (studio della
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi di base HTML</a
        >), CSS <a href="/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units">Valori e unità</a> e <a href="/it/docs/Learn_web_development/Core/Styling_basics/Sizing">Ridimensionamento</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere cosa è il trabocco.</li>
          <li>Controllare il trabocco con la proprietà <code>overflow</code>.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Che cos'è il trabocco?

Tutto in CSS è una scatola. Lei può limitare la dimensione di queste scatole assegnando valori come {{cssxref("width")}} e {{cssxref("height")}}. **Il trabocco si verifica quando c'è troppo contenuto per adattarsi a una scatola.** CSS fornisce diversi strumenti per gestire il trabocco. Approfondendo con il layout CSS e scrivendo CSS, incontrerà più situazioni di trabocco.

## CSS cerca di evitare la "perdita di dati"

Consideriamo due esempi che dimostrano il comportamento predefinito di CSS quando c'è un trabocco.

Il primo esempio è una scatola limitata impostando un `height`. Poi aggiungiamo contenuto che supera lo spazio assegnato. Il contenuto trabocca dalla scatola e va nel paragrafo sottostante.

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

Il secondo esempio è una parola in una scatola. La scatola è stata resa troppo piccola per la parola e quindi esce dalla scatola.

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

Potrebbe chiedersi perché CSS funzioni in modo così disordinato, mostrando contenuto al di fuori del suo contenitore previsto. Perché non nascondere il contenuto traboccante? Perché non ridimensionare il contenitore per adattare tutto il contenuto?

Ovunque possibile, CSS non nasconde il contenuto. Ciò causerebbe la perdita di dati. Il problema con la perdita di dati è che potrebbe non accorgersene. I visitatori del sito potrebbero non accorgersene. Se il pulsante di invio su un modulo scompare e nessuno può completare il modulo, questo potrebbe essere un grande problema! Invece, CSS trabocca in modi visibili. È più probabile che si veda che c'è un problema. Nel peggiore dei casi, un visitatore del sito le farà sapere che il contenuto si sta sovrapponendo.

Se limita una scatola con una `width` o una `height`, CSS si fida che sappia cosa sta facendo. CSS presume che gestisca il potenziale di trabocco. In generale, limitare la dimensione del blocco è problematico quando la scatola contiene testo. Potrebbe esserci più testo di quanto si sia previsto durante la progettazione del sito, o il testo potrebbe essere più grande (per esempio, se l'utente ha aumentato la dimensione del carattere).

## La proprietà overflow

La proprietà {{cssxref("overflow")}} le aiuta a gestire il trabocco del contenuto di un elemento. Utilizzando questa proprietà, può comunicare a un browser come dovrebbe gestire il contenuto traboccante. Il valore predefinito del tipo di valore [`<overflow>`](/it/docs/Web/CSS/overflow_value) è `visible`. Con questa impostazione predefinita, si può vedere il contenuto quando trabocca.

### Nascondere il contenuto traboccante

Per nascondere il contenuto quando trabocca, può impostare `overflow: hidden`. Questo fa esattamente ciò che dice: nasconde il trabocco. Attenzione che questo può rendere invisibile qualche contenuto. Dovrebbe farlo solo se nascondere il contenuto non causerà problemi.

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

Invece, forse vorrebbe aggiungere delle barre di scorrimento quando il contenuto trabocca? Utilizzando `overflow: scroll`, i browser con barre di scorrimento visibili le mostreranno sempre, anche se non c'è abbastanza contenuto da fare traboccare. Questo offre il vantaggio di mantenere costante il layout, invece di far apparire o scomparire le barre di scorrimento, a seconda della quantità di contenuto nel contenitore.

Rimuova un po' di contenuto dalla scatola sottostante. Noti come le barre di scorrimento restano, anche se non c'è necessità di scorrimento:

> [!NOTE]
> La visibilità della barra di scorrimento dipende dal sistema operativo.
> Potrebbe dover cambiare le impostazioni del browser per mostrare sempre le barre di scorrimento affinché le barre di scorrimento si mostrino sempre nei seguenti esempi.

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

Nell'esempio sopra, abbiamo bisogno di scorrere solo sull'asse `y`, tuttavia otteniamo le barre di scorrimento in entrambi gli assi. Per scorrere solo sull'asse `y`, potrebbe usare la proprietà {{cssxref("overflow-y")}}, impostando `overflow-y: scroll`.

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

Può anche abilitare lo scorrimento lungo l'asse x utilizzando {{cssxref("overflow-x")}}, anche se questo non è un modo consigliato per ospitare parole lunghe! Se ha una parola lunga in una piccola scatola, consideri di usare la proprietà {{cssxref("word-break")}} o {{cssxref("overflow-wrap")}}. Inoltre, alcuni dei metodi discussi in [Dimensionare gli oggetti in CSS](/it/docs/Learn_web_development/Core/Styling_basics/Sizing) potrebbe aiutarla a creare scatole che si adattino meglio a quantità variabili di contenuto.

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

Come con `scroll`, ottiene una barra di scorrimento nella dimensione di scorrimento, che ci sia o meno abbastanza contenuto per causare una barra di scorrimento.

> [!NOTE]
> Può specificare lo scorrimento per gli assi x e y utilizzando la proprietà `overflow`, passando due valori. Se vengono specificate due parole chiave, la prima si applica a `overflow-x` e la seconda si applica a `overflow-y`. Altrimenti, sia `overflow-x` che `overflow-y` vengono impostati sullo stesso valore. Per esempio, `overflow: scroll hidden` imposterà `overflow-x` su `scroll` e `overflow-y` su `hidden`.

### Mostrare le barre di scorrimento solo quando necessario

Se desidera che le barre di scorrimento appaiano solo quando c'è più contenuto di quanto possa essere contenuto nella scatola, usi `overflow: auto`. Questo permette al browser di determinare se dovrebbe mostrare le barre di scorrimento.

Nell'esempio qui sotto, rimuova il contenuto finché non si adatta alla scatola. Dovrebbe vedere le barre di scorrimento sparire:

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

I moderni metodi di layout (che incontrerà più avanti nel modulo [CSS layout](/it/docs/Learn_web_development/Core/CSS_layout)) gestiscono il trabocco. Funzionano in gran parte senza supposizioni o dipendenze sulla quantità di contenuto che ci sarà su una pagina web.

Questo non è sempre stato la norma. In passato, alcuni siti sono stati costruiti con contenitori a altezza fissa per allineare il fondo delle scatole. Queste scatole potrebbero altrimenti non avere avuto alcuna relazione tra loro. Era fragile. Se incontra una scatola dove il contenuto si sovrappone ad altro contenuto sulla pagina in applicazioni legacy, riconoscerà ora che ciò accade con il trabocco. Idealmente, ristrutturerà il layout per non dipendere da contenitori a altezza fissa.

Quando sviluppa un sito, tenga sempre presente il trabocco. Testi i design con grandi e piccole quantità di contenuto. Aumenti e diminuisca le dimensioni dei caratteri di almeno due incrementi. Verifichi che il CSS sia robusto. Cambiare i valori di trabocco per nascondere il contenuto o per aggiungere barre di scorrimento è riservato ad alcuni casi d'uso selezionati (per esempio, quando si intende avere una scatola di scorrimento).

## Metta alla prova le sue competenze!

È arrivato alla fine di questo articolo, ma ricorda le informazioni più importanti? Può trovare alcuni ulteriori test per verificare di aver trattenuto queste informazioni prima di continuare — veda [Metta alla prova le sue competenze: Overflow](/it/docs/Learn_web_development/Core/Styling_basics/Test_your_skills/Overflow).

## Riepilogo

Questa lezione ha introdotto il concetto di trabocco. Dovrebbe capire che il CSS predefinito evita di rendere invisibile il contenuto traboccante. Ha scoperto che può gestire il potenziale di trabocco e anche che dovrebbe verificare il lavoro per assicurarsi che non causi accidentalmente trabocchi problematici.

Nel prossimo articolo, esamineremo come gestire lo stile delle caratteristiche speciali della pagina, come immagini ed elementi del modulo.

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Backgrounds_and_borders", "Learn_web_development/Core/Styling_basics/Images_media_forms", "Learn_web_development/Core/Styling_basics")}}
