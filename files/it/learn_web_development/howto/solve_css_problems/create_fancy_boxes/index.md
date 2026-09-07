---
title: Creare riquadri elaborati
slug: Learn_web_development/Howto/Solve_CSS_problems/Create_fancy_boxes
l10n:
  sourceCommit: f78ca75460fbdbc7f17b6e366dad47b9760054b0
---

I riquadri CSS sono i blocchi costitutivi di qualsiasi pagina web stilizzata con CSS. Renderli gradevoli è al tempo stesso divertente e impegnativo. È divertente perché consiste nel trasformare un'idea di design in codice funzionante; è impegnativo a causa dei vincoli di CSS. Creiamo quindi dei riquadri elaborati.

Prima di passare al lato pratico, assicurarsi di conoscere [il box model CSS](/it/docs/Learn_web_development/Core/Styling_basics/Box_model). È inoltre una buona idea, ma non un prerequisito, conoscere alcune [nozioni di base sul layout CSS](/it/docs/Learn_web_development/Core/CSS_layout/Introduction).

Dal punto di vista tecnico, creare riquadri elaborati significa padroneggiare le proprietà CSS per bordi e sfondi e sapere come applicarle a un determinato riquadro. Ma oltre alle tecniche, si tratta anche di liberare la creatività. Non sarà fatto in un giorno e alcuni sviluppatori web si divertono a farlo per tutta la vita.

Vedremo molti esempi, ma lavoreremo sempre sul più semplice frammento HTML possibile, un semplice elemento:

```html
<div class="fancy">Hi! I want to be fancy.</div>
```

Bene, è un frammento HTML molto piccolo: cosa si può modificare su quell'elemento? Tutto quanto segue:

- Le sue proprietà del box model: {{cssxref("width")}}, {{cssxref("height")}}, {{cssxref("padding")}}, {{cssxref("border")}}, ecc.
- Le sue proprietà di sfondo: {{cssxref("background")}}, {{cssxref("background-color")}}, {{cssxref("background-image")}}, {{cssxref("background-position")}}, {{cssxref("background-size")}}, ecc.
- I suoi pseudo-elementi: {{cssxref("::before")}} e {{cssxref("::after")}}
- e alcune proprietà aggiuntive come: {{cssxref("box-shadow")}}, {{cssxref("rotate")}}, {{cssxref("outline")}}, ecc.

Quindi si dispone di un'area di sperimentazione molto ampia. Che il divertimento abbia inizio.

## Modificare il box model

Il solo box model consente di realizzare alcune operazioni di base, come aggiungere bordi semplici, creare quadrati e così via. Inizia a diventare interessante quando si spingono le proprietà al limite, usando `padding` e/o `margin` negativi oppure un `border-radius` più grande delle dimensioni effettive del riquadro.

### Creare cerchi

```html hidden
<div class="fancy">Hi! I want to be fancy.</div>
```

Questo è qualcosa di molto semplice e al tempo stesso molto divertente. La proprietà {{cssxref("border-radius")}} è progettata per creare angoli arrotondati nei riquadri, ma cosa accade se la dimensione del raggio è uguale o superiore alla larghezza effettiva del riquadro?

```css
.fancy {
  /* Within a circle, centered text looks prettier. */
  text-align: center;

  /* Let's avoid our text touching the border. As
     our text will still flow in a square, it looks
     nicer that way, giving the feeling that it's a "real"
     circle. */
  padding: 1em;

  /* The border will make the circle visible.
     You could also use a background, as
     backgrounds are clipped by border radius */
  border: 0.5em solid black;

  /* Let's make sure we have a square.
     If it's not a square, we'll get an
     ellipsis rather than a circle */
  width: 4em;
  height: 4em;

  /* and let's turn the square into a circle */
  border-radius: 100%;
}
```

Sì, si ottiene un cerchio:

{{ EmbedLiveSample('Making_circles', '100%', '120') }}

## Sfondi

Quando si parla di un riquadro elaborato, le proprietà principali da gestire sono le [proprietà background-\*](/it/docs/Web/CSS/Guides/Backgrounds_and_borders). Quando si inizia a sperimentare con gli sfondi, è come se il riquadro CSS si trasformasse in una tela bianca da riempire.

Prima di passare a esempi pratici, è utile fare un passo indietro: ci sono due aspetti da conoscere sugli sfondi.

- È possibile impostare [diversi sfondi](/it/docs/Web/CSS/Guides/Backgrounds_and_borders/Using_multiple_backgrounds) su un singolo riquadro. Vengono sovrapposti come livelli.
- Gli sfondi possono essere colori pieni o immagini: un colore pieno riempie sempre l'intera superficie, mentre le immagini possono essere ridimensionate e posizionate.

```html hidden
<div class="fancy">Hi! I want to be fancy.</div>
```

Bene, divertiamoci con gli sfondi:

```css
.fancy {
  padding: 1em;
  width: 100%;
  height: 200px;
  box-sizing: border-box;

  /* At the bottom of our background stack,
     let's have a misty grey solid color */
  background-color: #e4e4d9;

  /* We stack linear gradients on top of each
     other to create our color strip effect.
     As you will notice, color gradients are
     considered to be images and can be
     manipulated as such */
  background-image:
    linear-gradient(175deg, transparent 95%, #8da389 95%),
    linear-gradient(85deg, transparent 95%, #8da389 95%),
    linear-gradient(175deg, transparent 90%, #b4b07f 90%),
    linear-gradient(85deg, transparent 92%, #b4b07f 92%),
    linear-gradient(175deg, transparent 85%, #c5a68e 85%),
    linear-gradient(85deg, transparent 89%, #c5a68e 89%),
    linear-gradient(175deg, transparent 80%, #ba9499 80%),
    linear-gradient(85deg, transparent 86%, #ba9499 86%),
    linear-gradient(175deg, transparent 75%, #9f8fa4 75%),
    linear-gradient(85deg, transparent 83%, #9f8fa4 83%),
    linear-gradient(175deg, transparent 70%, #74a6ae 70%),
    linear-gradient(85deg, transparent 80%, #74a6ae 80%);
}
```

{{ EmbedLiveSample('Backgrounds', '100%', '200') }}

> [!NOTE]
> I gradienti possono essere utilizzati in modi molto creativi. Per vedere alcuni esempi creativi, consultare i [pattern CSS di Lea Verou](https://projects.verou.me/css3patterns/). Per approfondire i gradienti, consultare [l'articolo dedicato](/it/docs/Web/CSS/Guides/Images/Using_gradients).

## Pseudo-elementi

Quando si applicano stili a un singolo riquadro, ci si può sentire limitati e desiderare più riquadri per creare stili ancora più sorprendenti. Nella maggior parte dei casi, questo porta a inquinare il DOM aggiungendo elementi HTML aggiuntivi con l'unico scopo di applicare stili. Anche se talvolta è necessario, ciò è generalmente considerato una cattiva pratica. Una soluzione per evitare questi problemi consiste nell'usare gli [pseudo-elementi CSS](/it/docs/Web/CSS/Reference/Selectors/Pseudo-elements).

### Una nuvola

```html hidden
<div class="fancy">Hi! I want to be fancy.</div>
```

Vediamo un esempio trasformando il riquadro in una nuvola:

```css
.fancy {
  text-align: center;

  /* Same trick as previously used to make circles */
  box-sizing: border-box;
  width: 150px;
  height: 150px;
  padding: 80px 1em 0 1em;

  /* We make room for the "ears" of our cloud */
  margin: 0 100px;

  position: relative;

  background-color: #a4c9cf;

  /* Well, actually we are not making a full circle
     as we want the bottom of our cloud to be flat.
     Feel free to tweak this example to make a cloud
     that isn't flat at the bottom ;) */
  border-radius: 100% 100% 0 0;
}

/* Those are common style that apply to both our ::before
   and ::after pseudo elements. */
.fancy::before,
.fancy::after {
  /* This is required to be allowed to display the
     pseudo-elements, event if the value is an empty
     string */
  content: "";

  /* We position our pseudo-elements on the left and
     right sides of the box, but always at the bottom */
  position: absolute;
  bottom: 0;

  /* This makes sure our pseudo-elements will be below
     the box content whatever happens. */
  z-index: -1;

  background-color: #a4c9cf;
  border-radius: 100%;
}

.fancy::before {
  /* This is the size of the clouds left ear */
  width: 125px;
  height: 125px;

  /* We slightly move it to the left */
  left: -80px;

  /* To make sure that the bottom of the cloud
     remains flat, we must make the bottom right
     corner of the left ear square. */
  border-bottom-right-radius: 0;
}

.fancy::after {
  /* This is the size of the clouds left ear */
  width: 100px;
  height: 100px;

  /* We slightly move it to the right */
  right: -60px;

  /* To make sure that the bottom of the cloud
     remains flat, we must make the bottom left
     corner of the right ear square. */
  border-bottom-left-radius: 0;
}
```

{{ EmbedLiveSample('A_cloud', '100%', '160') }}

### Blockquote

Un esempio più pratico dell'uso degli pseudo-elementi consiste nel creare una gradevole formattazione per gli elementi HTML {{HTMLElement('blockquote')}}. Vediamo quindi un esempio con un frammento HTML leggermente diverso, che offre anche l'opportunità di vedere come gestire la localizzazione del design:

```html
<blockquote>
  People who think they know everything are a great annoyance to those of us who
  do. <i>Isaac Asimov</i>
</blockquote>
<blockquote lang="fr">
  L'intelligence, c'est comme les parachutes, quand on n'en a pas, on s'écrase.
  <i>Pierre Desproges</i>
</blockquote>
```

Ecco quindi lo stile:

```css
blockquote {
  min-height: 5em;
  padding: 1em 4em;
  font: 1em/150% sans-serif;
  position: relative;
  background-color: lightgoldenrodyellow;
}

blockquote::before,
blockquote::after {
  position: absolute;
  height: 3rem;
  font:
    6rem/100% "Georgia",
    serif;
}

blockquote::before {
  content: "“";
  top: 0.3rem;
  left: 0.9rem;
}

blockquote::after {
  content: "”";
  bottom: 0.3rem;
  right: 0.8rem;
}

blockquote:lang(fr)::before {
  content: "«";
  top: -1.5rem;
  left: 0.5rem;
}

blockquote:lang(fr)::after {
  content: "»";
  bottom: 2.6rem;
  right: 0.5rem;
}

blockquote i {
  display: block;
  font-size: 0.8em;
  margin-top: 1rem;
  text-align: right;
}
```

{{ EmbedLiveSample('Blockquote', '100%', '300') }}

## Tutto insieme e altro ancora

È quindi possibile creare un effetto straordinario mescolando tutti questi elementi. A un certo punto, ottenere tale decorazione dei riquadri diventa una questione di creatività, sia nel design sia nell'uso tecnico delle proprietà CSS. In questo modo è possibile creare illusioni ottiche che possono dare vita ai riquadri, come in questo esempio:

```html hidden
<div class="fancy">Hi! I want to be fancy.</div>
```

Creiamo alcuni effetti di ombra esterna parziali. La proprietà {{cssxref("box-shadow")}} consente di creare luce interna e un effetto di ombra esterna piatta, ma con un po' di lavoro aggiuntivo diventa possibile creare una geometria più naturale usando uno pseudo-elemento e la proprietà {{cssxref("rotate")}}, una delle tre proprietà individuali {{cssxref("transform")}}.

```css
.fancy {
  position: relative;
  background-color: #ffffcc;
  padding: 2rem;
  text-align: center;
  max-width: 200px;
}

.fancy::before {
  content: "";

  position: absolute;
  z-index: -1;
  bottom: 15px;
  right: 5px;
  width: 50%;
  top: 80%;
  max-width: 200px;

  box-shadow: 0px 13px 10px black;
  rotate: 4deg;
}
```

{{ EmbedLiveSample('All_together_and_more', '100%', '120') }}
