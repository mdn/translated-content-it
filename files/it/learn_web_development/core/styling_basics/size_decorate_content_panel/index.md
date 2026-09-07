---
title: "Sfida: dimensionare e decorare un pannello di contenuto"
short-title: "Sfida: dimensionamento e decorazione"
slug: Learn_web_development/Core/Styling_basics/Size_decorate_content_panel
l10n:
  sourceCommit: 50a1895c9c499b1b9207f7af945a0fe45de58cca
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Test_your_skills/Overflow", "Learn_web_development/Core/Styling_basics/Images_media_forms", "Learn_web_development/Core/Styling_basics")}}

In questa sfida viene fornita una struttura di pagina leggermente stilizzata che visualizza un pannello di contenuto contenente testo e immagini, con un'intestazione in alto e una barra di pulsanti in basso. Seguire le istruzioni per dimensionarlo e decorarlo, ottenendo come risultato un layout interessante. Durante il percorso, verranno verificate le conoscenze relative a valori e unità CSS, dimensionamento, overflow, sfondi e bordi.

## Punto di partenza

La sfida verrà risolta nell'ambiente di sviluppo locale; idealmente, l'esempio dovrebbe essere visualizzato in una finestra completa del browser per assicurarsi di procedere nella giusta direzione.

1. Creare sul computer una nuova cartella denominata `size-decorate-content-panel`.
2. All'interno della cartella, creare un file `index.html` e incollarvi il seguente contenuto:

   ```html-nolint live-sample___content-pane-start live-sample___content-pane-finish
   <!doctype html>
   <html lang="en">
     <head>
       <meta charset="utf-8" />
       <title>Challenge: Content pane with button bar</title>
       <link href="style.css" rel="stylesheet" />
     </head>
     <body>
       <section class="pane">
         <h1>Content pane</h1>
         <div class="content">
           <h2>Some exciting content</h2>

           <p>
             Lorem ipsum dolor sit amet, consectetur adipiscing elit,
             sed do eiusmod tempor incididunt ut labore et dolore magna
             aliqua. Proin tortor purus <a href="#">platea sit eu id</a>
             nisi litora libero. Neque vulputate consequat ac amet augue
             blandit maximus aliquet congue. Pharetra vestibulum posuere
             ornare <a href="#">faucibus fusce dictumst</a> orci aenean eu
             facilisis ut volutpat commodo senectus purus himenaeos fames
             primis convallis nisi.
           </p>
           <img
             src="https://mdn.github.io/shared-assets/images/examples/leopard.jpg"
             alt="Closeup of a large wild cat's eyes and nose" />
           <p>
             Phasellus fermentum malesuada phasellus netus dictum aenean
             placerat egestas amet. <a href="#">Ornare taciti semper dolor
             tristique</a> morbi. Sem leo tincidunt aliquet semper eu lectus
             scelerisque quis. Sagittis vivamus mollis nisi mollis enim
             fermentum laoreet.
           </p>

           <h2>More exciting content</h2>

           <p>
             Curabitur semper venenatis lectus viverra ex dictumst nulla
             maximus. Primis iaculis elementum conubia feugiat venenatis
             dolor augue ac blandit nullam ac <a href="#">phasellus turpis</a>
             feugiat mollis. Duis lectus porta mattis imperdiet vivamus augue
             litora lectus arcu. Justo torquent pharetra volutpat ad blandit
             bibendum <a href="#">accumsan nec elit cras</a> luctus primis
             ipsum gravida class congue.
           </p>
           <img
             src="https://mdn.github.io/shared-assets/images/examples/balloons-landscape.jpg"
             alt="Three colorful hot air balloons floating across a blue, nearly cloudless sky" />
           <p>
             Vehicula etiam elementum finibus enim duis feugiat commodo
             adipiscing tortor <a href="#">tempor elit</a>. Et mollis
             consectetur habitant turpis tortor consectetur adipiscing
             vulputate dolor lectus iaculis convallis adipiscing. Nam
             hendrerit <a href="#">dignissim condimentum ullamcorper diam</a>
             morbi eget consectetur odio in sagittis.
           </p>
         </div>
         <div class="controls">
           <button>One</button>
           <button>Two</button>
           <button>Three</button>
           <button>Four</button>
         </div>
       </section>
     </body>
   </html>
   ```

3. All'interno della cartella, creare un file `style.css` e incollarvi il seguente contenuto:

   ```css live-sample___content-pane-start
   /* Type and text */

   * {
     box-sizing: border-box;
   }

   html {
     height: 100%;
   }

   body {
     height: inherit;
     font: 1.2em / 1.5 system-ui;
     margin: 0 auto;
   }

   h1 {
     font-size: 2em;
   }

   h2 {
     font-size: 1.5em;
   }

   a {
     color: red;
   }

   a:hover,
   a:focus {
     text-decoration: none;
   }

   /* Styling the pane */

   .pane {
     height: 100%;
   }

   h1,
   .controls {
     margin: 0;
     display: flex;
     justify-content: center;
     align-items: center;
   }

   .content {
   }

   .controls {
     justify-content: space-around;
     gap: 20px;
     padding: 20px;
   }

   button {
     flex: 1;
   }
   ```

4. Salvare i file e caricare `index.html` in un browser, pronti per effettuare i test.

## Descrizione del progetto

Seguire i passaggi riportati di seguito per completare il progetto, dimensionando opportunamente il pannello di contenuto e aggiungendo le decorazioni richieste.

### Intestazioni

1. Usare il contenuto generato per far apparire un'emoji di libro (📖) all'inizio dell'intestazione di primo livello. Aggiungere `20px` di spazio tra l'emoji e il testo dell'intestazione.
2. Attualmente, le intestazioni sono dimensionate in `em`. Modificare il dimensionamento in modo che sia responsive, cambiando in base alla larghezza del viewport ma rimanendo anche zoomabile. Per ottenere questo risultato, rendere la dimensione di ogni livello di intestazione uguale a una percentuale appropriata della larghezza del viewport più un valore `em` più piccolo.

### Dimensionamento del contenitore

1. Impostare la larghezza dell'elemento wrapper `<section>` con classe `pane` su `60%`, ma assegnargli una larghezza massima di `1000px` e una larghezza minima di `480px`. Provare a individuare una funzione CSS che consenta di impostare questi valori mediante una singola dichiarazione.
2. Centrare orizzontalmente nella pagina il `<section>` `pane` usando margini `auto`.
3. Impostare sia `<h1>` sia `<div>` con classe `controls` su un'altezza di `100px`. Impostare `<div>` con classe `content` in modo che sia alto il `100%` dell'altezza di `<body>`, meno l'altezza di `<h1>` e di `<div class="controls">`. Questo dovrebbe produrre un'interfaccia che si estende sempre fino all'altezza del viewport, con un contenitore di contenuto flessibile e un'intestazione e una barra di pulsanti ad altezza fissa.
4. I pulsanti appaiono un po' sottili e difficili da leggere. Assegnare loro un'altezza pari al `100%` del relativo contenitore e una dimensione del carattere di `1.2em`.
5. Assegnare al `<section>` `pane` e al `<div>` `content` un padding superiore/inferiore di `0` su entrambi i lati e un padding sinistro/destro di `20px` su entrambi i lati.

### Posizionamento delle immagini

1. Le immagini attualmente fuoriescono dal contenitore di contenuto. Impostare su di esse una larghezza massima del `90%` per impedire che ciò accada.
2. Centrare orizzontalmente le immagini usando margini `auto`.

### Decorazione

1. Applicare al `<section>` `pane` un gradiente lineare che cambi gradualmente da `#9fb4c7` in alto a `#7f7caf` in basso.
2. Assegnare alle immagini un bordo `1px solid` e al `<div>` `content` un bordo `2px solid`. Impostare per i bordi il colore `#28587b`.
3. Assegnare al `<div>` `content` un colore di sfondo `#eeeeff` e un'immagine di sfondo `https://mdn.github.io/shared-assets/images/examples/big-star.png`. L'immagine di sfondo non deve ripetersi, deve avere dimensioni di `40px` per `40px` e deve essere posizionata a `5px` dal bordo superiore del contenitore e a `15px` dal bordo destro.
4. Assegnare ai pulsanti un colore del testo `white` e un colore di sfondo `rgb(40 88 123 / 0.8)`. Al passaggio del mouse o quando ricevono il focus, i pulsanti devono cambiare per avere una versione completamente opaca dello stesso colore di sfondo.
5. Impostare un raggio del bordo di `10px` sul `<div>` `content` e sui pulsanti.

### Overflow

A questo punto, dovrebbe essere ancora evidente un problema nell'interfaccia: il contenuto incluso nel `<div>` `content` fuoriesce dal relativo contenitore e l'intera pagina scorre per consentire di accedervi completamente. Si vuole invece che sia il `<div>` `content` a scorrere. Come è possibile ottenerlo?

## Suggerimenti e consigli

- Usare il [validatore CSS del W3C](https://jigsaw.w3.org/css-validator/) per individuare errori involontari nel CSS, che altrimenti potrebbero passare inosservati, così da poterli correggere.
- Non è necessario modificare l'HTML in alcun modo.

## Esempio

Lo stato iniziale del progetto verrà visualizzato in questo modo:

{{EmbedLiveSample("content-pane-start", "100%", 500)}}

Il progetto completato dovrebbe apparire così (è stato renderizzato con una larghezza del `90%`, anziché del `60%`, per una migliore visualizzazione nel pannello di output stretto):

{{EmbedLiveSample("content-pane-finish", "100%", 500)}}

<details>
<summary>Fare clic qui per visualizzare una possibile soluzione</summary>

Il CSS completato è il seguente:

```css live-sample___content-pane-finish
/* Type and text */

* {
  box-sizing: border-box;
}

html {
  height: 100%;
}

body {
  height: inherit;
  font: 1.2em / 1.5 system-ui;
  margin: 0 auto;
}

h1 {
  /* Solution: Responsive heading sizing, equal to vw value plus em value */
  font-size: calc(2vw + 1em);
}

/* Solution: Add book emoji as generated content, with 20px spacing between
it and the heading content */
h1::before {
  content: "📖";
  margin-right: 20px;
}

h2 {
  /* Solution: Responsive heading sizing, equal to vw value plus em value */
  font-size: calc(1.5vw + 0.75em);
}

a {
  color: red;
}

a:hover,
a:focus {
  text-decoration: none;
}

.pane {
  height: 100%;
  /* Solution: Set container width percentage and min
  and max width with one declaration, using the clamp()
  function  */
  width: clamp(480px, 60%, 1000px);
  /* Solution: Center container using auto margins */
  margin: 0 auto;
  /* Solution: Set container top/bottom padding of 0 on both sides
  and left/right padding of 20px on both sides */
  padding: 0 20px;
  /* Solution: Apply linear gradient from top to bottom */
  background: linear-gradient(to bottom, #9fb4c7, #7f7caf);
}

h1,
.controls {
  margin: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  /* Solution: Set the h1 and controls div to each be 100px high */
  height: 100px;
}

.content {
  /* Solution: Set background color and image on the content div,
  and size the image */
  background: url("https://mdn.github.io/shared-assets/images/examples/big-star.png")
    no-repeat top 5px right 15px / 40px #eeeeff;
  /* Solution: Set content top/bottom padding of 0 on both sides and
  left/right padding of 20px on both sides */
  padding: 0 20px;
  /* Solution: Set the content div to be 100% high minus the h1 and
  controls div combined height (200px) */
  height: calc(100% - 200px);
  /* Solution: Set border on the content div */
  border: 2px solid #28587b;
  /* Solution: Stop the content from overflowing its container;
  make it scroll instead */
  overflow: auto;
}

img {
  /* Solution: Set 90% maximum width on the images */
  max-width: 90%;
  /* Solution: Center using auto margins */
  margin: 0 auto;
  display: block;
  /* Solution: Set border on the images */
  border: 1px solid #28587b;
}

.controls {
  justify-content: space-around;
  gap: 20px;
  padding: 20px;
}

button {
  flex: 1;
  /* Solution: Set button height to 100% and font size to 1.2em */
  height: 100%;
  font-size: 1.2em;
  /* Solution: Set white text color on the buttons */
  color: white;
  /* Solution: Set background color on the buttons */
  background-color: rgb(40 88 123 / 0.8);
}

/* Solution: Set fully-opaque background color on the
buttons on hover and focus */
button:hover,
button:focus {
  background-color: rgb(40 88 123 / 1);
}

/* Solution: Set border radius on content div and buttons */
.content,
button {
  border-radius: 10px;
}
```

```css hidden live-sample___content-pane-finish
.pane {
  width: clamp(480px, 90%, 1000px);
}
```

</details>

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Test_your_skills/Overflow", "Learn_web_development/Core/Styling_basics/Images_media_forms", "Learn_web_development/Core/Styling_basics")}}
