---
title: Creare scatole fantasiose
slug: Learn_web_development/Howto/Solve_CSS_problems/Create_fancy_boxes
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

I box CSS sono i mattoni fondamentali di qualsiasi pagina web stilizzata con CSS. Renderli piacevoli alla vista è sia divertente che impegnativo. È divertente perché si tratta di trasformare un'idea di design in codice funzionante; è impegnativo a causa dei vincoli del CSS. Facciamo alcune scatole fantasiose.

Prima di iniziare a passare alla parte pratica, assicurati di essere familiare con [il modello di box CSS](/it/docs/Learn_web_development/Core/Styling_basics/Box_model). È anche una buona idea, ma non un requisito, essere familiari con alcune [basi di layout CSS](/it/docs/Learn_web_development/Core/CSS_layout/Introduction).

Dal punto di vista tecnico, creare scatole fantasiose riguarda tutto il padroneggiare le proprietà di bordi e sfondi del CSS e come applicarle a un box dato. Ma al di là delle tecniche è anche questione di liberare la propria creatività. Non sarà completato in un giorno, e alcuni sviluppatori web passano tutta la vita a divertirsi con questo.

Vedremo molti esempi, ma lavoreremo sempre sull'elemento HTML più semplice possibile:

```html
<div class="fancy">Hi! I want to be fancy.</div>
```

Ok, questo è un piccolo frammento di HTML, cosa possiamo modificare su quell'elemento? Tutto quanto segue:

- Le proprietà del suo modello: {{cssxref("width")}}, {{cssxref("height")}}, {{cssxref("padding")}}, {{cssxref("border")}}, ecc.
- Le proprietà del suo sfondo: {{cssxref("background")}}, {{cssxref("background-color")}}, {{cssxref("background-image")}}, {{cssxref("background-position")}}, {{cssxref("background-size")}}, ecc.
- Il suo pseudo-elemento: {{cssxref("::before")}} e {{cssxref("::after")}}
- e alcune proprietà collaterali come: {{cssxref("box-shadow")}}, {{cssxref("rotate")}}, {{cssxref("outline")}}, ecc.

Quindi abbiamo un ampio campo di gioco. Che il divertimento abbia inizio.

## Modifica del modello di box

Il modello di box da solo ci permette di fare alcune cose basilari, come aggiungere bordi semplici, creare quadrati, ecc. Inizia a diventare interessante quando si spingono le proprietà al limite avendo `padding` negativi e/o `margin` e avendo un `border-radius` più grande della dimensione effettiva del box.

### Creazione di cerchi

```html hidden
<div class="fancy">Hi! I want to be fancy.</div>
```

Questo è qualcosa di molto semplice e molto divertente. La proprietà {{cssxref("border-radius")}} è fatta per creare un angolo arrotondato applicato ai box, ma cosa succede se la dimensione del raggio è uguale o maggiore della larghezza effettiva del box?

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

Quando parliamo di un box fantasioso, le proprietà principali per gestirlo sono [le proprietà background-\*](/it/docs/Web/CSS/CSS_backgrounds_and_borders). Quando inizi a giocherellare con gli sfondi è come se il tuo box CSS si trasformasse in una tela bianca che riempirai.

Prima di passare a degli esempi pratici, facciamo un passo indietro perché ci sono due cose che dovresti sapere sugli sfondi.

- È possibile impostare [diversi sfondi](/it/docs/Web/CSS/CSS_backgrounds_and_borders/Using_multiple_backgrounds) su un singolo box. Sono impilati l'uno sopra l'altro come strati.
- Gli sfondi possono essere sia colori solidi che immagini: il colore solido riempie sempre tutta la superficie, ma le immagini possono essere scalate e posizionate.

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
> I gradienti possono essere utilizzati in modi molto creativi. Se desideri vedere alcuni esempi creativi, dai un'occhiata ai [patterns CSS di Lea Verou](https://projects.verou.me/css3patterns/). Se vuoi saperne di più sui gradienti, sentiti libero di leggere il [nostro articolo dedicato](/it/docs/Web/CSS/CSS_images/Using_CSS_gradients).

## Pseudo-elementi

Quando si stile un singolo box, potresti trovarti limitato e desidereresti avere più box per creare stili ancora più sorprendenti. La maggior parte delle volte, questo porta a inquinare il DOM aggiungendo elementi HTML extra per il solo scopo del design. Anche se è necessario, è considerata una cattiva pratica. Una soluzione per evitare tali trappole è usare [gli pseudo-elementi CSS](/it/docs/Web/CSS/Pseudo-elements).

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

### Blockquote

Un esempio più pratico di utilizzo degli pseudo-elementi è costruire una formattazione piacevole per gli elementi HTML {{HTMLElement('blockquote')}}. Quindi vediamo un esempio con un frammento di HTML leggermente diverso (che ci offre un'opportunità per vedere come gestire anche la localizzazione del design):

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

Ecco il nostro stile:

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

## Tutto insieme e oltre

Quindi è possibile creare un effetto meraviglioso quando mettiamo tutto insieme. Ad un certo punto, per realizzare tale abbellimento del box è una questione di creatività, sia nel design che nell'uso tecnico delle proprietà CSS. Facendo così è possibile creare illusioni ottiche che possono dare vita ai tuoi box come in questo esempio:

```html hidden
<div class="fancy">Hi! I want to be fancy.</div>
```

Creiamo degli effetti parziali di ombra cadente. La proprietà {{cssxref("box-shadow")}} ci permette di creare una luce interna e un effetto ombra piatta, ma con qualche lavoro extra diventa possibile creare una geometria più naturale usando uno pseudo-elemento e la proprietà {{cssxref("rotate")}}, una delle tre proprietà individuali di {{cssxref("transform")}}.

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
