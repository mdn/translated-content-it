---
title: "Sfida: comprensione fondamentale del layout"
short-title: "Sfida: layout fondamentale"
slug: Learn_web_development/Core/CSS_layout/Fundamental_Layout_Comprehension
l10n:
  sourceCommit: 4c58f4735f986a91bee1b77e336143630df727a2
---

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Test_your_skills/Grid", "Learn_web_development/Core/CSS_layout/Responsive_Design", "Learn_web_development/Core/CSS_layout")}}

Questa sfida metterà alla prova la conoscenza delle funzionalità di layout affrontate finora nel modulo, ovvero flexbox, float, grid e posizionamento. Al termine, sarà stata sviluppata una pagina web usando tutti questi strumenti fondamentali.

## Punto di partenza

Questa sfida va risolta nell'ambiente di sviluppo locale; idealmente, l'esempio andrebbe visualizzato in una finestra del browser a schermo intero per assicurarsi che le funzionalità di layout funzionino come previsto.

1. Creare sul computer una nuova cartella denominata `layout-challenge`.
2. All'interno della cartella, creare un file `index.html` e incollarvi il seguente contenuto:

   ```html
   <!doctype html>
   <html lang="en-US">
     <head>
       <meta charset="utf-8" />
       <meta name="viewport" content="width=device-width" />
       <title>Layout Task</title>
       <link href="style.css" rel="stylesheet" type="text/css" />
     </head>

     <body>
       <div class="logo">My exciting website!</div>

       <nav>
         <ul>
           <li><a href="">Home</a></li>
           <li><a href="">Blog</a></li>
           <li><a href="">About us</a></li>
           <li><a href="">Our history</a></li>
           <li><a href="">Contacts</a></li>
         </ul>
       </nav>

       <main class="grid">
         <article>
           <h1>An Exciting Blog Post</h1>
           <img src="images/square6.jpg" alt="placeholder" class="feature" />
           <p>
             Lorem ipsum dolor sit amet, consectetur adipiscing elit. Duis non
             justo at erat egestas porttitor vel nec tortor. Mauris in molestie
             ipsum. Vivamus diam elit, ornare ornare nisi vitae, ullamcorper
             pharetra ligula. In vel lacus quis nulla sollicitudin pellentesque.
           </p>

           <p>
             Nunc vitae eleifend odio, eget tincidunt sem. Cras et varius justo.
             Nulla sollicitudin quis urna vitae efficitur. Pellentesque
             hendrerit molestie arcu sit amet lacinia. Vivamus vulputate sed
             purus at eleifend. Phasellus malesuada sem vel libero hendrerit,
             sed finibus massa porta. Vestibulum luctus scelerisque libero, sit
             amet sagittis eros sollicitudin ac. Class aptent taciti sociosqu ad
             litora torquent per conubia nostra, per inceptos himenaeos.
           </p>

           <p>
             Phasellus tincidunt eros iaculis, feugiat mi at, eleifend mauris.
             Quisque porttitor lacus eu massa condimentum, eu tincidunt nisl
             consequat. Nunc egestas lacus dolor, id scelerisque ante tincidunt
             ac. In risus massa, sodales ac enim eu, iaculis eleifend lorem.
           </p>

           <p>
             Maecenas euismod condimentum enim, non rhoncus neque tempor ut.
             Vestibulum eget nisi ornare, vehicula felis id, aliquet nibh. Donec
             in mauris in diam aliquam commodo nec ac nunc. Aliquam nisl risus,
             eleifend a iaculis id, tempor vel tortor. Nam ullamcorper dictum
             tellus id rhoncus. Sed quis nulla in mi aliquam euismod nec eu
             metus.
           </p>

           <p>
             Nam orci nulla, convallis aliquet ante ut, lobortis hendrerit
             risus. Nulla malesuada porta turpis in consequat. Duis suscipit
             nulla a mauris pellentesque vehicula. Fusce euismod, mi malesuada
             venenatis vestibulum, metus erat faucibus dui, vel rutrum turpis
             nibh ut diam.
           </p>

           <p>
             Nam ornare et mauris eget tincidunt. Nam ornare et mauris eget
             tincidunt. Donec et ipsum a orci elementum commodo et ut ex.
             Vivamus porttitor sem in purus maximus, eu imperdiet felis
             lobortis.
           </p>

           <p>
             Pellentesque ullamcorper dolor ut ullamcorper convallis. Duis a
             orci aliquet, pretium neque ut, auctor purus. Proin viverra
             tincidunt nisi id fringilla. Maecenas interdum risus in ultricies
             finibus. Vestibulum volutpat tincidunt libero, a feugiat leo
             suscipit in. Sed eget lacus rutrum, semper ligula a, vestibulum
             ipsum. Mauris in odio fringilla, accumsan eros blandit, mattis
             odio. Ut viverra mollis augue, vitae ullamcorper velit hendrerit
             eu. Curabitur mi lacus, condimentum in auctor sed, ornare sed leo.
           </p>
         </article>

         <aside>
           <h2>Photography</h2>
           <ul class="photos">
             <li><img src="images/square1.jpg" alt="placeholder" /></li>
             <li><img src="images/square2.jpg" alt="placeholder" /></li>
             <li><img src="images/square3.jpg" alt="placeholder" /></li>
             <li><img src="images/square4.jpg" alt="placeholder" /></li>
             <li><img src="images/square5.jpg" alt="placeholder" /></li>
           </ul>
         </aside>
       </main>
     </body>
   </html>
   ```

3. All'interno della cartella, creare un file `style.css` e incollarvi il seguente contenuto:

   ```css
   * {
     box-sizing: border-box;
   }

   body {
     background-color: white;
     color: #333333;
     margin: 0;
     font: 1.2em / 1.6 sans-serif;
   }

   img {
     max-width: 100%;
     display: block;
     border: 1px solid black;
   }

   .logo {
     font-size: 200%;
     padding: 50px 20px;
     margin: 0 auto;
     max-width: 980px;
   }

   .grid {
     margin: 0 auto;
     max-width: 980px;
   }

   nav {
     background-color: black;
     padding: 0.5em;
   }

   nav ul {
     margin: 0;
     padding: 0;
     list-style: none;
   }

   nav a {
     color: white;
     text-decoration: none;
     padding: 0.5em 1em;
   }

   .photos {
     list-style: none;
     margin: 0;
     padding: 0;
   }

   .feature {
     width: 200px;
   }
   ```

4. All'interno della cartella, creare una sottocartella denominata `images` e salvarvi i seguenti file di immagini:
   - [`square1.jpg`](https://mdn.github.io/shared-assets/images/examples/learn/balloons/square1.jpg)
   - [`square2.jpg`](https://mdn.github.io/shared-assets/images/examples/learn/balloons/square2.jpg)
   - [`square3.jpg`](https://mdn.github.io/shared-assets/images/examples/learn/balloons/square3.jpg)
   - [`square4.jpg`](https://mdn.github.io/shared-assets/images/examples/learn/balloons/square4.jpg)
   - [`square5.jpg`](https://mdn.github.io/shared-assets/images/examples/learn/balloons/square5.jpg)
   - [`square6.jpg`](https://mdn.github.io/shared-assets/images/examples/learn/balloons/square6.jpg)
5. Salvare i file e caricare `index.html` in un browser, pronti per eseguire i test. Il punto di partenza della pagina dispone di uno stile di base ma non di un layout e dovrebbe avere un aspetto simile a questo:

   ![Punto di partenza dell'esercizio di layout. Gli elementi non sono disposti ordinatamente. È presente un titolo del sito web, sopra una barra di navigazione nera con 5 collegamenti allineati a sinistra, seguita dal titolo del post del blog e dal contenuto del post. Tra il titolo del blog e il contenuto del blog è presente una foto allineata a sinistra.](layout-task-start.png)

## Specifiche del progetto

Sono stati forniti HTML grezzo, CSS di base e immagini: ora occorre creare un layout per il design.

Le attività da completare sono:

1. Visualizzare gli elementi di navigazione in una riga, con una quantità uguale di spazio tra gli elementi e una quantità minore di spazio alle due estremità della riga.
2. Applicare uno stile alla barra di navigazione affinché scorra normalmente con il contenuto, ma rimanga poi fissata nella parte superiore della viewport quando la raggiunge.
3. Fare in modo che il testo si disponga attorno all'immagine "feature" all'interno dell'articolo, a destra e sotto di essa, con una quantità di spazio adeguata tra l'immagine e il testo.
4. Visualizzare gli elementi {{htmlelement("article")}} e {{htmlelement("aside")}} come un layout a due colonne, con il primo largo tre volte il secondo. Le colonne devono avere una dimensione flessibile, così che se la finestra del browser si restringe, anche le colonne si restringano. Includere uno spazio di 20 pixel tra le due colonne.
5. Le fotografie devono essere visualizzate come una griglia a due colonne con colonne di uguale dimensione e uno spazio di 5 pixel tra le immagini.

## Suggerimenti e consigli

- Non è necessario modificare l'HTML per completare questa sfida.
- Esistono diversi modi per completare alcune delle attività nelle specifiche del progetto e spesso non esiste un unico modo corretto o sbagliato per svolgerle. Provare approcci diversi e verificare quale funziona meglio. Prendere appunti durante gli esperimenti.

## Esempio

Lo screenshot seguente mostra un esempio dell'aspetto che dovrebbe avere il layout finale del design:

![Sito web dell'esercizio di layout completato. Gli elementi sono disposti ordinatamente. È presente un titolo del sito web, sopra una barra di navigazione nera contenente 5 collegamenti equidistanti. Sotto la barra di navigazione sono presenti due sezioni. A sinistra si trova un post del blog: un titolo del post seguito dal relativo contenuto. Il contenuto del blog si dispone attorno a una foto allineata a sinistra. Sul lato destro è presente un titolo "photography" sopra un gruppo di immagini disposte in una griglia larga due immagini.](layout-task-complete.png)

<details>
<summary>Fare clic qui per mostrare una possibile soluzione</summary>

Il CSS completato è il seguente:

```css
* {
  box-sizing: border-box;
}

body {
  background-color: white;
  color: #333333;
  margin: 0;
  font: 1.2em / 1.6 sans-serif;
}

img {
  max-width: 100%;
  display: block;
  border: 1px solid black;
}

.logo {
  font-size: 200%;
  padding: 50px 20px;
  margin: 0 auto;
  max-width: 980px;
}

.grid {
  margin: 0 auto;
  max-width: 980px;
  /* Solution: Display <article> and <aside> as two flexible
  columns, with <article> three times the width of <aside>,
  and a 20px gap */
  display: grid;
  grid-template-columns: 3fr 1fr;
  gap: 20px;
}

nav {
  background-color: black;
  padding: 0.5em;
  /* Solution: Make navigation bar scroll with content normally but
  then stick to top of viewport */
  top: 0;
  position: sticky;
}

nav ul {
  margin: 0;
  padding: 0;
  list-style: none;
  /* Solution: Display the navigation items in a row with equal space
  in between and less space at the ends  */
  display: flex;
  justify-content: space-around;
}

nav a {
  color: white;
  text-decoration: none;
  padding: 0.5em 1em;
}

.photos {
  list-style: none;
  margin: 0;
  padding: 0;
  /* Solution: Display photos in two-column grid with equal columns
  and a 5px gap */
  display: grid;
  gap: 5px;
  grid-template-columns: 1fr 1fr;
}

.feature {
  width: 200px;
  /* Solution: Wrap text around the "feature" image to the right and bottom,
  with suitable space between image and text */
  float: left;
  margin: 8px 30px 20px 0;
}
```

</details>

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Test_your_skills/Grid", "Learn_web_development/Core/CSS_layout/Responsive_Design", "Learn_web_development/Core/CSS_layout")}}
