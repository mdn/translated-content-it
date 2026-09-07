---
title: "Sfida: impaginare la homepage di una scuola della comunità"
short-title: "Sfida: homepage di una scuola della comunità"
slug: Learn_web_development/Core/Text_styling/Typesetting_a_homepage
l10n:
  sourceCommit: 9f7e7e9075e9f2b1937d2c8000f52a8ff76bff52
---

{{PreviousMenuNext("Learn_web_development/Core/Text_styling/Web_fonts", "Learn_web_development/Core/CSS_layout", "Learn_web_development/Core/Text_styling")}}

In questa sfida, verrà testata la comprensione delle tecniche di stile del testo trattate in questo modulo, chiedendo di impaginare la homepage di una scuola della comunità. Lungo il percorso ci si potrebbe anche divertire.

## Punto di partenza

Questa sfida verrà svolta nell'ambiente di sviluppo locale; idealmente, è consigliabile visualizzare l'esempio in una finestra del browser a schermo intero per assicurarsi di procedere nella direzione corretta.

1. Creare una nuova cartella sul computer chiamata `typesetting-challenge`.
2. All'interno della cartella, creare un file `index.html` e incollarvi il seguente contenuto:

   ```html
   <!doctype html>
   <html lang="en-US">
     <head>
       <meta charset="utf-8" />
       <meta name="viewport" content="width=device-width" />
       <title>St Huxley's Community College</title>
       <link href="style.css" type="text/css" rel="stylesheet" />
     </head>
     <body>
       <header>
         <h1>St Huxley's Community College</h1>
       </header>

       <main>
         <section>
           <h2>Brave new world</h2>

           <p>
             It's a brave new world out there. Our children are being put in
             increasingly more competitive situations as they move through the
             different stages of their life with
             <a href="https://en.wikipedia.org/wiki/Examination">examinations</a
             >, <a href="https://en.wikipedia.org/wiki/Jobs">jobs</a>,
             <a href="https://en.wikipedia.org/wiki/Career">careers</a>, and
             other life choices. Having the wrong mindset or making the wrong
             choices can lead to problems at all stages.
           </p>

           <p>
             As concerned parents, guardians, or carers, you will no doubt want
             to give your children the best possible start in life — and you've
             come to the right place.
           </p>

           <h2>The best start in life</h2>

           <p>
             At St. Huxley's, we pride ourselves in not only giving our students
             a top-quality education, but also giving them the
             <a href="https://en.wikipedia.org/wiki/Society">societal</a> and
             <a href="https://en.wikipedia.org/wiki/Emotion">emotional</a>
             intelligence they need to win big in the future. We not only excel
             at subjects such as genetics, data mining, and chemistry, but we
             also include compulsory lessons in:
           </p>

           <ul>
             <li>Emotional resilience</li>
             <li>Critical thinking</li>
             <li>Judgement</li>
             <li>Assertion</li>
             <li>Focus and resolve</li>
           </ul>

           <p>
             If you are interested, then don't hesitate to get in touch; we'd
             love to hear from you:
           </p>

           <ol>
             <li>
               <a href="#">Call</a> or <a href="#">Email</a> us for more
               information.
             </li>
             <li>
               <a href="#">Ask for a brochure</a>, which includes a signup form.
             </li>
             <li><a href="#">Book a visit</a>!</li>
           </ol>
         </section>

         <aside>
           <h2>Top courses</h2>

           <ul>
             <li><a href="#">Genetic engineering</a></li>
             <li><a href="#">Organic Chemistry</a></li>
             <li><a href="#">Pharmaceuticals</a></li>
             <li><a href="#">Behavioral science</a></li>
             <li><a href="#">Biochemistry</a></li>
             <li><a href="#">Data mining</a></li>
             <li><a href="#">Computer security</a></li>
             <li><a href="#">Bioinformatics</a></li>
             <li><a href="#">Cybernetics</a></li>
           </ul>

           <p><a href="#">See all...</a></p>
         </aside>

         <nav>
           <ul>
             <li><a href="#">Home</a></li>
             <li><a href="#">Finding us</a></li>
             <li><a href="#">Courses</a></li>
             <li><a href="#">Staff</a></li>
             <li><a href="#">Media</a></li>
             <li><a href="#">Prospectus</a></li>
           </ul>
         </nav>
       </main>

       <footer>
         <p>&copy; 2025 St Huxley's Community College</p>
       </footer>
     </body>
   </html>
   ```

3. All'interno della cartella, creare un file `style.css` e incollarvi il seguente contenuto:

   ```css
   /* General setup */

   * {
     box-sizing: border-box;
   }

   body {
     margin: 0 auto;
     padding: 0 20px;
     min-width: 980px;
     max-width: 1400px;
   }

   /* Layout */

   main {
     display: grid;
     grid-template-columns: 5fr 2fr 2fr;
     gap: 40px;
     padding: 20px 0;
   }

   /* header and footer */

   header {
     border-bottom: 5px solid #aa6666;
   }

   footer {
     border-top: 5px solid #aa6666;
   }

   footer p {
     text-align: center;
   }
   ```

4. Scaricare l'icona [`external-link-52.png`](https://mdn.github.io/shared-assets/images/examples/external-link-52.png) e salvarla nella cartella, allo stesso livello dei file di codice.

5. Salvare i file e caricare `index.html` in un browser, pronti per il test.

## Specifiche del progetto

Sono stati forniti dell'HTML per la homepage di un immaginario college della comunità, oltre a del CSS che dispone il contenuto in tre colonne e fornisce altri stili rudimentali. Occorre aggiungere delle regole in fondo al file CSS per risolvere le sfide descritte nelle sezioni seguenti.

### Applicare i font alla pagina

1. Scegliere i font per i titoli e per il corpo del testo da applicare alla pagina:
   - Poiché si tratta di un college, i font dovrebbero dare al sito un aspetto piuttosto serio e affidabile. Un font serif per l'intero sito, destinato al corpo del testo generale, abbinato a un font pesante/slab per i titoli potrebbe funzionare.
   - Si può scegliere se utilizzare un servizio di font online come Google Fonts per accedere ai font oppure scaricare localmente i file dei font nel progetto. Qualunque sia la scelta, rendere i font disponibili alla pagina. Se si scelgono file di font locali, utilizzare un servizio adeguato per generare codice `@font-face` affidabile per essi.
2. Applicare il font del corpo del testo all'intera pagina e il font dei titoli ai titoli.

### Stile generale del testo

1. Assegnare ai titoli e agli altri tipi di elementi dimensioni del font appropriate, definite usando un'unità relativa adatta.
2. Assegnare al corpo del testo un `line-height` adatto.
3. Centrare sulla pagina il titolo di livello più alto.
4. Rimuovere il margine inferiore dai titoli di secondo livello.
5. Assegnare ai titoli e al corpo del testo un po' di `letter-spacing`, affinché non risultino troppo compressi e le lettere abbiano un po' più di spazio.
6. Assegnare al primo paragrafo dopo ogni titolo nel `<section>` una piccola indentazione del testo, ad esempio `2rem`.

### Stile dei link

1. Assegnare agli stati link, visitato, focus e hover colori che si abbinino al colore delle barre orizzontali nella parte superiore e inferiore della pagina.
2. Fare in modo che i link siano sottolineati per impostazione predefinita, ma che la sottolineatura scompaia al passaggio del mouse o quando ricevono il focus.
3. Rimuovere il contorno di focus predefinito da TUTTI i link della pagina.
4. Fare in modo che nei link _esterni_ venga inserita l'icona del link esterno alla loro destra, con una dimensione adeguata.

### Stile delle liste

1. Assicurarsi che la spaziatura delle liste e degli elementi delle liste funzioni bene con lo stile dell'intera pagina. Ogni lista dovrebbe avere lo stesso `line-height` e gli stessi margini superiore e inferiore dei paragrafi.
2. Assegnare agli elementi delle liste stili di punto elenco appropriati per il design della pagina. Si può scegliere se utilizzare un'immagine personalizzata come punto elenco oppure qualcos'altro.

### Stile del menu di navigazione

Applicare uno stile al menu di navigazione in modo che si armonizzi con la pagina. Questa parte è lasciata in gran parte alla scelta dello sviluppatore, ma ecco alcuni suggerimenti:

1. Fare in modo che i link appaiano come pulsanti, larghi quanto la colonna in cui si trovano e sufficientemente alti affinché gli elementi di navigazione occupino una quantità adeguata di spazio.
2. Applicare al testo dei link di navigazione lo stesso font applicato ai titoli.
3. Assicurarsi che l'area attivabile di ciascun link sia espansa in modo da riempire interamente il relativo elemento della lista padre.
4. Centrare il testo all'interno di ciascun link.
5. Trasformare il testo in maiuscolo, usando CSS e non modificando l'HTML.

## Suggerimenti

- Non è necessario modificare l'HTML per questo esercizio, a meno che non sia richiesto per applicare i font alla pagina.

## Esempio

La schermata seguente mostra l'aspetto iniziale della pagina:

![Una schermata dello stato iniziale della pagina. Il titolo principale riporta "St Huxley's Community College" e il footer contiene un avviso di copyright. Linee rosse separano header e footer dal contenuto. Il contenuto principale è composto da tre colonne: una contiene il corpo del testo e le altre due contengono liste di link. Il testo è visualizzato con gli stili predefiniti del browser](example-start.png)

La schermata seguente, invece, mostra un esempio del possibile aspetto del design completato:

![Una schermata del design della sfida completato. Il titolo principale riporta "St Huxley's Community College". Una linea rossa separa l'header dal contenuto. Il contenuto principale è composto da tre colonne: una contiene il corpo del testo, una contiene una lista di link e nella terza è presente una barra di navigazione verticale. Il testo è visualizzato con alcuni stili appropriati](example-finished.png)

<details>
<summary>Fare clic qui per visualizzare una possibile soluzione</summary>

Il nostro CSS completato appare così:

```css
/* Solution: Apply fonts to the page */

@import "https://fonts.googleapis.com/css2?family=Bevan:ital@0;1&family=IBM+Plex+Serif:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;1,100;1,200;1,300;1,400;1,500;1,600;1,700&display=swap";

html {
  font-family: "IBM Plex Serif", serif;
}

h1,
h2 {
  font-family: "Bevan", serif;
}

/* General setup */

* {
  box-sizing: border-box;
}

body {
  margin: 0 auto;
  padding: 0 20px;
  min-width: 980px;
  max-width: 1400px;
}

/* Layout */

main {
  display: grid;
  grid-template-columns: 5fr 2fr 2fr;
  gap: 40px;
  padding: 20px 0;
}

/* Header and footer */

header {
  border-bottom: 5px solid #aa6666;
}

footer {
  border-top: 5px solid #aa6666;
}

footer p {
  text-align: center;
}

/* Solution: General text styling */

h1 {
  font-size: 3rem;
  text-align: center;
  letter-spacing: 3px;
}

h2 {
  font-size: 2rem;
  margin-bottom: 0;
  letter-spacing: 1px;
}

section h2 + p {
  text-indent: 2rem;
}

p,
li {
  line-height: 1.6;
  letter-spacing: 0.5px;
}

/* Solution: Link styling */

a {
  outline: none;
}

a[href*="http"] {
  padding-right: 16px;
  background: url("external-link-52.png") no-repeat right center;
  background-size: 14px 14px;
}

a:link,
a:visited {
  color: #aa6666;
}

a:focus,
a:hover {
  text-decoration: none;
  color: #773333;
}

/* Solution: List styling */

ul,
ol {
  margin: 1rem 0;
}

ul {
  list-style-type: square;
}

ol {
  list-style-type: lower-roman;
}

/* Solution: Navigation menu styling */

nav ul {
  padding-left: 0;
}

nav li {
  list-style-type: none;
  margin-bottom: 1rem;
}

nav li a {
  font-family: "Bevan", serif;
  text-decoration: none;
  display: inline-block;
  width: 100%;
  line-height: 3.5;
  text-transform: uppercase;
  text-align: center;
  letter-spacing: 1px;
  font-size: 1.3rem;
  font-weight: bold;
  border: 1px solid #aa6666;
}

nav li a:focus,
nav li a:hover {
  color: white;
  background: #aa6666;
}

nav li a:active {
  color: white;
  background: black;
}
```

</details>

{{PreviousMenuNext("Learn_web_development/Core/Text_styling/Web_fonts", "Learn_web_development/Core/CSS_layout", "Learn_web_development/Core/Text_styling")}}
