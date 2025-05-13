---
title: Creare box eleganti
slug: Learn_web_development/Howto/Solve_CSS_problems/Create_fancy_boxes
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

I box CSS sono i mattoni fondamentali di qualsiasi pagina web stilizzata con CSS. Renderli belli esteticamente è sia divertente che impegnativo. È divertente perché si tratta di trasformare un'idea di design in codice funzionante; è impegnativo a causa dei vincoli del CSS. Facciamo dei box eleganti.

Prima di immergerci nell'aspetto pratico, assicuratevi di avere familiarità con [il modello di box CSS](/it/docs/Learn_web_development/Core/Styling_basics/Box_model). È anche una buona idea, ma non un prerequisito, essere familiari con alcune [basi del layout CSS](/it/docs/Learn_web_development/Core/CSS_layout/Introduction).

Dal punto di vista tecnico, creare box eleganti riguarda soprattutto il padroneggiare le proprietà di bordo e di sfondo del CSS e come applicarle a un determinato box. Ma oltre alle tecniche si tratta anche di liberare la tua creatività. Non sarà fatto in un giorno e alcuni sviluppatori web passano tutta la loro vita a divertirsi con esso.

Stiamo per vedere molti esempi, ma lavoreremo sempre sul pezzo di HTML più semplice possibile, un semplice elemento:

```html
<div class="fancy">Hi! I want to be fancy.</div>
```

Ok, è un piccolo pezzo di HTML, cosa possiamo modificare su quell'elemento? Tutto il seguente:

- Le sue proprietà del modello di box: {{cssxref("width")}}, {{cssxref("height")}}, {{cssxref("padding")}}, {{cssxref("border")}}, ecc.
- Le sue proprietà dello sfondo: {{cssxref("background")}}, {{cssxref("background-color")}}, {{cssxref("background-image")}}, {{cssxref("background-position")}}, {{cssxref("background-size")}}, ecc.
- I suoi pseudo-elementi: {{cssxref("::before")}} e {{cssxref("::after")}}
- e alcune proprietà extra come: {{cssxref("box-shadow")}}, {{cssxref("rotate")}}, {{cssxref("outline")}}, ecc.

Quindi abbiamo un campo di gioco molto ampio. Che il divertimento abbia inizio.

## Modifica del modello di box

Il modello di box da solo ci consente di fare alcune cose di base, come aggiungere bordi semplici, creare quadrati, ecc. Inizia a diventare interessante quando si spingono le proprietà al limite avendo `padding` e/o `margin` negativi o avendo `border-radius` più grande della dimensione effettiva del box.

### Creare cerchi

```html hidden
<div class="fancy">Hi! I want to be fancy.</div>
```

Questo è qualcosa che è sia molto semplice che molto divertente. La proprietà {{cssxref("border-radius")}} è fatta per creare un angolo arrotondato applicato ai box, ma cosa succede se la dimensione del raggio è uguale o più grande della larghezza effettiva del box?

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

Sì, otteniamo un cerchio:

{{ EmbedLiveSample('Making_circles', '100%', '120') }}

## Sfondi

Quando parliamo di un box elegante, le proprietà principali per gestirlo sono le [proprietà background-\*](/it/docs/Web/CSS/CSS_backgrounds_and_borders). Quando inizi a giocare con gli sfondi, è come se il tuo box CSS si trasformasse in una tela bianca che riempirai.

Prima di passare a esempi pratici, facciamo un passo indietro poiché ci sono due cose che dovrebbe sapere sugli sfondi.

- È possibile impostare [diversi sfondi](/it/docs/Web/CSS/CSS_backgrounds_and_borders/Using_multiple_backgrounds) su un singolo box. Sono impilati uno sopra l'altro come strati.
- Gli sfondi possono essere colori solidi o immagini: i colori solidi riempiono sempre l'intera superficie ma le immagini possono essere scalate e posizionate.

```html hidden
<div class="fancy">Hi! I want to be fancy.</div>
```

Ok, divertiamoci con gli sfondi:

```css-nolint
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
  background-image: linear-gradient(175deg, rgb(0 0 0 / 0%) 95%, #8da389 95%),
                    linear-gradient( 85deg, rgb(0 0 0 / 0%) 95%, #8da389 95%),
                    linear-gradient(175deg, rgb(0 0 0 / 0%) 90%, #b4b07f 90%),
                    linear-gradient( 85deg, rgb(0 0 0 / 0%) 92%, #b4b07f 92%),
                    linear-gradient(175deg, rgb(0 0 0 / 0%) 85%, #c5a68e 85%),
                    linear-gradient( 85deg, rgb(0 0 0 / 0%) 89%, #c5a68e 89%),
                    linear-gradient(175deg, rgb(0 0 0 / 0%) 80%, #ba9499 80%),
                    linear-gradient( 85deg, rgb(0 0 0 / 0%) 86%, #ba9499 86%),
                    linear-gradient(175deg, rgb(0 0 0 / 0%) 75%, #9f8fa4 75%),
                    linear-gradient( 85deg, rgb(0 0 0 / 0%) 83%, #9f8fa4 83%),
                    linear-gradient(175deg, rgb(0 0 0 / 0%) 70%, #74a6ae 70%),
                    linear-gradient( 85deg, rgb(0 0 0 / 0%) 80%, #74a6ae 80%);
}
```

{{ EmbedLiveSample('Backgrounds', '100%', '200') }}

> [!NOTE]
> I gradienti possono essere usati in modi molto creativi. Se desidera vedere alcuni esempi creativi, dia un'occhiata ai [pattern CSS di Lea Verou](https://projects.verou.me/css3patterns/). Se desidera saperne di più sui gradienti, non esiti a consultare [il nostro articolo dedicato](/it/docs/Web/CSS/CSS_images/Using_CSS_gradients).

## Pseudo-elementi

Quando si stile un singolo box, potrebbe trovarsi limitato e desiderare di avere più box per creare stili ancora più straordinari. La maggior parte delle volte, questo porta a "inquinare" il DOM aggiungendo elementi HTML extra per il solo scopo dello stile. Anche se è necessario, è in qualche modo considerata una cattiva pratica. Una soluzione per evitare tali insidie è utilizzare [pseudo-elementi CSS](/it/docs/Web/CSS/Pseudo-elements).

### Una nuvola

```html hidden
<div class="fancy">Hi! I want to be fancy.</div>
```

Facciamo un esempio trasformando il nostro box in una nuvola:

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

### Citazione

Un esempio più pratico di utilizzo degli pseudo-elementi è costruire un bel formato per elementi HTML {{HTMLElement('blockquote')}}. Vediamo quindi un esempio con un frammento HTML leggermente diverso (che ci offre un'opportunità di vedere come gestire anche la localizzazione del design):

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

Ed ecco il nostro stile:

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
    6rem/100% Georgia,
    "Times New Roman",
    Times,
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

## Tutto insieme e altro

È quindi possibile creare un effetto meraviglioso quando mescoliamo tutto questo insieme. A un certo punto, realizzare tale abbellimento del box è una questione di creatività, sia nel design che nell'uso tecnico delle proprietà CSS. Facendo così è possibile creare illusioni ottiche che possono dare vita ai vostri box come in questo esempio:

```html hidden
<div class="fancy">Hi! I want to be fancy.</div>
```

Creiamo alcuni effetti di ombra parziale. La proprietà {{cssxref("box-shadow")}} ci consente di creare una luce interna e un effetto di ombra piatta, ma con un po' di lavoro extra diventa possibile creare una geometria più naturale usando uno pseudo-elemento e la proprietà {{cssxref("rotate")}}, una delle tre proprietà individuali di {{cssxref("transform")}}.

```css
.fancy {
  position: relative;
  background-color: #ffc;
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
