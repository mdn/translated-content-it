---
title: "Sfida: strutturare una pagina di contenuti"
short-title: "Sfida: sito di birdwatching"
slug: Learn_web_development/Core/Structuring_content/Structuring_a_page_of_content
l10n:
  sourceCommit: e91043ed51d5ae2232912c1ae24bc94ffe254674
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Test_your_skills/Links", "Learn_web_development/Core/Structuring_content/HTML_images", "Learn_web_development/Core/Structuring_content")}}

Strutturare una pagina di contenuti pronta per ricevere un layout tramite CSS è una competenza molto importante da padroneggiare; in questa sfida verrà testata la capacità di pensare a come potrebbe apparire una pagina e di scegliere una semantica strutturale appropriata su cui costruire un layout.

## Punto di partenza

Per risolvere questa sfida, occorre creare un semplice progetto di sito web, all'interno di una cartella sul disco rigido del computer oppure usando un editor online come [CodePen](https://codepen.io/) o [JSFiddle](https://jsfiddle.net/). Gran parte del codice necessario è già fornita.

1. Creare una nuova cartella in una posizione appropriata del computer denominata `structuring-html-challenge` (oppure aprire un editor online ed eseguire i passaggi necessari per creare un nuovo progetto).
2. Salvare il seguente listato HTML in un file nella cartella denominato `index.html` (oppure incollarlo nel pannello HTML dell'editor online).

   ```html
   <!doctype html>
   <html lang="en">
     <head>
       <meta charset="utf-8" />
       <title>Birdwatching</title>
       <link
         href="https://fonts.googleapis.com/css?family=Roboto+Condensed:300%7CCinzel+Decorative:700"
         rel="stylesheet" />
     </head>

     <body>
       <h1>Birdwatching</h1>

       Home Get started Photos Gear Forum

       <h2>Welcome</h2>

       <p>
         Welcome to our fake birdwatching site. If this were a real site, it
         would be the ideal place to come to learn more about birdwatching,
         whether you are a beginner looking to learn how to get into birding, or
         an expert wanting to share ideas, tips, and photos with other
         like-minded people.
       </p>

       <p>
         So don't waste time! Get what you need, then turn off that computer and
         get out into the great outdoors!
       </p>

       <h2>Favorite photos</h2>

       <!-- Link images here. -->

       <p>
         This fake website example is CC0 — any part of this code may be reused
         in any way you wish. Original example written by Chris Mills, 2016.
       </p>

       <p>
         <a href="http://game-icons.net/lorc/originals/dove.html">Dove icon</a>
         by Lorc.
       </p>
     </body>
   </html>
   ```

3. Salvare il seguente listato CSS in un file nella cartella denominato `style.css` (oppure incollarlo nel pannello CSS dell'editor online).

   ```css
   /* || General setup */

   body {
     margin: 0;
   }

   html {
     font-size: 10px;
     background-color: darkgrey;
   }

   body {
     width: 70%;
     min-width: 800px;
     margin: 0 auto;
   }

   /* || typography */

   h1,
   h2 {
     font-family: "Cinzel Decorative", cursive;
     color: #2a2a2a;
   }

   p,
   li {
     font-family: "Roboto Condensed", sans-serif;
     color: #2a2a2a;
   }

   h1 {
     font-size: 4rem;
     text-align: center;
     text-shadow: 2px 2px 10px black;
   }

   h2 {
     font-size: 3rem;
     text-align: center;
   }

   p,
   li {
     font-size: 1.6rem;
     line-height: 1.5;
   }

   /* || header layout */

   header {
     margin-bottom: 10px;
   }

   main,
   header,
   nav,
   article,
   aside,
   footer,
   section {
     background-color: #00ff0080;
     padding: 1%;
   }

   h1 {
     text-transform: uppercase;
     display: flex;
     align-items: center;
     justify-content: center;
     gap: 20px;
   }

   header img {
     height: 60px;
   }

   nav ul {
     padding: 0;
     list-style-type: none;
     display: flex;
   }

   nav li {
     text-align: center;
     flex: 1;
   }

   nav a {
     font-size: 2rem;
     text-transform: uppercase;
     text-decoration: none;
     color: black;
   }

   nav a:hover,
   nav a:focus {
     color: red;
   }

   /* || main layout */

   main {
     display: flex;
     gap: 10px;
   }

   article {
     flex: 4;
   }

   aside {
     flex: 1;
   }

   aside a {
     display: block;
     float: left;
     width: 50%;
   }

   aside img {
     max-width: 100%;
   }

   footer {
     margin-top: 10px;
   }
   ```

In seguito, sarà necessario includere nella pagina i seguenti URL.

- `dove.png`: [Il logo del sito](https://mdn.github.io/shared-assets/images/examples/learn/birds/dove.png)
- `favorite-bird-1.jpg`: [Versione a dimensione intera della prima immagine nella barra laterale](https://mdn.github.io/shared-assets/images/examples/learn/birds/favorite-bird-1.jpg)
- `favorite-bird-1_th.jpg`: [Miniatura della prima immagine nella barra laterale](https://mdn.github.io/shared-assets/images/examples/learn/birds/favorite-bird-1_th.jpg)
- `favorite-bird-2.jpg`: [Versione a dimensione intera della seconda immagine nella barra laterale](https://mdn.github.io/shared-assets/images/examples/learn/birds/favorite-bird-2.jpg)
- `favorite-bird-2_th.jpg`: [Miniatura della seconda immagine nella barra laterale](https://mdn.github.io/shared-assets/images/examples/learn/birds/favorite-bird-2_th.jpg)
- `favorite-bird-3.jpg`: [Versione a dimensione intera della terza immagine nella barra laterale](https://mdn.github.io/shared-assets/images/examples/learn/birds/favorite-bird-3.jpg)
- `favorite-bird-3_th.jpg`: [Miniatura della terza immagine nella barra laterale](https://mdn.github.io/shared-assets/images/examples/learn/birds/favorite-bird-3_th.jpg)
- `favorite-bird-4.jpg`: [Versione a dimensione intera della quarta immagine nella barra laterale](https://mdn.github.io/shared-assets/images/examples/learn/birds/favorite-bird-4.jpg)
- `favorite-bird-4_th.jpg`: [Miniatura della quarta immagine nella barra laterale](https://mdn.github.io/shared-assets/images/examples/learn/birds/favorite-bird-4_th.jpg)

## Descrizione del progetto

Per questo progetto, il compito consiste nel prendere i contenuti della homepage di un sito web di birdwatching e aggiungervi elementi strutturali, in modo che possa esservi applicato un layout di pagina. È inoltre necessario apportare alcune aggiunte ai contenuti.

### Aggiunte ai contenuti

1. All'interno dell'elemento `<h1>`, aggiungere un elemento `<img>` che includa nella pagina il logo della colomba. Assegnargli un testo alternativo vuoto ("").
2. Gli elementi di testo "Home", "Get started", "Photos", "Gear" e "Forum" devono essere trasformati in un menu di navigazione.
   1. Contrassegnarli come elenco non ordinato.
   2. All'interno di ogni elemento dell'elenco, racchiudere il testo in un elemento `<a>` che punti a un URL `#` (che crea un link fittizio).
3. Rimuovere il commento `<!-- Link images here. -->`. Sostituirlo con un insieme di quattro immagini in miniatura degli "uccelli preferiti". Ciascuna deve includere un testo alternativo appropriato che descriva l'immagine ed essere racchiusa in un elemento `<a>` che colleghi alla corrispondente versione a dimensione intera.

### Requisiti strutturali

La struttura del sito deve essere composta dai seguenti elementi:

1. Un'intestazione che racchiuda il titolo della pagina di livello superiore e l'elenco del menu di navigazione.
2. Un ulteriore contenitore attorno all'elenco del menu di navigazione.
3. Un'area dei contenuti principali contenente due colonne: un articolo principale che contenga il testo di benvenuto e una barra laterale (aside) che contenga le miniature delle immagini.
4. Un piè di pagina contenente le informazioni sul copyright e i crediti.

In altre parole, è necessario aggiungere un contenitore appropriato per:

- L'intestazione
- Il menu di navigazione
- Il contenuto principale
- L'articolo di benvenuto
- L'aside delle immagini
- Il piè di pagina

### Applicare lo stile alla pagina

Se necessario, applicare il CSS fornito alla pagina aggiungendo un altro elemento {{htmlelement("link")}} subito sotto quello esistente fornito nell'HTML iniziale (alcuni editor di codice online applicheranno il CSS automaticamente).

## Suggerimenti

- Usare il [validatore HTML W3C](https://validator.w3.org/) per individuare errori involontari nell'HTML e correggerli.
- Non è necessario conoscere il CSS per questa sfida; è sufficiente applicare all'HTML il CSS fornito.
- Se risulta difficile immaginare quali elementi inserire dove, disegnare un semplice diagramma a blocchi del layout della pagina e annotare gli elementi che dovrebbero racchiudere ciascun blocco. Questo è molto utile.

## Esempio

La seguente schermata mostra un esempio dell'aspetto che potrebbe avere la homepage dopo il markup. Se risulta difficile capire come ottenere questo risultato, consultare la soluzione sotto l'esempio dal vivo.

![L'esempio completato della sfida; una semplice pagina web sul birdwatching, con un'intestazione "Birdwatching", foto di uccelli e un messaggio di benvenuto](example-page.png)

<details>
<summary>Fare clic qui per visualizzare la soluzione</summary>

L'HTML finale dovrebbe essere simile al seguente:

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>Birdwatching</title>
    <link
      href="https://fonts.googleapis.com/css?family=Roboto+Condensed:300%7CCinzel+Decorative:700"
      rel="stylesheet" />
    <link href="style.css" rel="stylesheet" />
  </head>

  <body>
    <header>
      <h1>
        Birdwatching
        <img
          src="https://mdn.github.io/shared-assets/images/examples/learn/birds/dove.png"
          alt="" />
      </h1>

      <nav>
        <ul>
          <li><a href="#">Home</a></li>
          <li><a href="#">Get started</a></li>
          <li><a href="#">Photos</a></li>
          <li><a href="#">Gear</a></li>
          <li><a href="#">Forum</a></li>
        </ul>
      </nav>
    </header>

    <main>
      <article>
        <h2>Welcome</h2>

        <p>
          Welcome to our fake birdwatching site. If this were a real site, it
          would be the ideal place to come to learn more about birdwatching,
          whether you are a beginner looking to learn how to get into birding,
          or an expert wanting to share ideas, tips, and photos with other
          like-minded people.
        </p>

        <p>
          So don't waste time! Get what you need, then turn off that computer
          and get out into the great outdoors!
        </p>
      </article>

      <aside>
        <h2>Favorite photos</h2>

        <a
          href="https://mdn.github.io/shared-assets/images/examples/learn/birds/favorite-bird-1.jpg">
          <img
            src="https://mdn.github.io/shared-assets/images/examples/learn/birds/favorite-bird-1_th.jpg"
            alt="Small black bird, black claws, long black slender beak" />
        </a>
        <a
          href="https://mdn.github.io/shared-assets/images/examples/learn/birds/favorite-bird-2.jpg">
          <img
            src="https://mdn.github.io/shared-assets/images/examples/learn/birds/favorite-bird-2_th.jpg"
            alt="Top half of a pretty bird with bright blue plumage on neck, light colored beak, blue headdress" />
        </a>
        <a
          href="https://mdn.github.io/shared-assets/images/examples/learn/birds/favorite-bird-3.jpg">
          <img
            src="https://mdn.github.io/shared-assets/images/examples/learn/birds/favorite-bird-3_th.jpg"
            alt="Top half of a large bird with white plumage, very long curved narrow light colored break" />
        </a>
        <a
          href="https://mdn.github.io/shared-assets/images/examples/learn/birds/favorite-bird-4.jpg">
          <img
            src="https://mdn.github.io/shared-assets/images/examples/learn/birds/favorite-bird-4_th.jpg"
            alt="Large bird, mostly white plumage with black plumage on back and rear, long straight white beak" />
        </a>
      </aside>
    </main>

    <footer>
      <p>
        This fake website example is CC0 — any part of this code may be reused
        in any way you wish. Original example written by Chris Mills, 2016.
      </p>

      <p>
        <a href="http://game-icons.net/lorc/originals/dove.html">Dove icon</a>
        by Lorc.
      </p>
    </footer>
  </body>
</html>
```

</details>

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Test_your_skills/Links", "Learn_web_development/Core/Structuring_content/HTML_images", "Learn_web_development/Core/Structuring_content")}}
