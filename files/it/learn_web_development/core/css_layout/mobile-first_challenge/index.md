---
title: "Sfida: un layout mobile-first"
short-title: "Sfida: mobile-first"
slug: Learn_web_development/Core/CSS_layout/Mobile-first_challenge
l10n:
  sourceCommit: 4c58f4735f986a91bee1b77e336143630df727a2
---

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Test_your_skills/Responsive_design", "Learn_web_development/Core/Scripting", "Learn_web_development/Core/CSS_layout")}}
Questa sfida conclude il modulo [Layout CSS](/it/docs/Learn_web_development/Core/CSS_layout), chiedendo di aggiornare un layout mobile esistente affinché funzioni bene anche nei browser desktop. Durante il percorso, verranno inoltre verificate competenze relative a funzionalità di layout responsive quali media query, griglia CSS, flexbox e immagini responsive.

Dopo aver completato questa sfida, è possibile passare allo studio dell'implementazione di comportamenti dinamici con [JavaScript](/it/docs/Learn_web_development/Core/Scripting).

## Punto di partenza

Questa sfida va risolta nell'ambiente di sviluppo locale; idealmente, è consigliabile visualizzare l'esempio in una finestra del browser a dimensione intera, per assicurarsi che le funzionalità di layout funzionino come previsto.

1. Creare una nuova cartella sul computer chiamata `mobile-first-challenge`.
2. All'interno della cartella, creare un file `index.html` e incollarvi il seguente contenuto:

   ```html
   <!doctype html>
   <html lang="en-US">
     <head>
       <meta charset="utf-8" />
       <title>RWD Task</title>
       <link href="style.css" rel="stylesheet" type="text/css" />
       <script defer src="script.js"></script>
     </head>

     <body>
       <header>
         <div class="logo">My exciting website!</div>
         <button aria-label="Open menu">☰</button>
       </header>

       <main class="grid">
         <nav>
           <ul>
             <li><a href="#">Home</a></li>
             <li><a href="#">Blog</a></li>
             <li><a href="#">About us</a></li>
             <li><a href="#">Our history</a></li>
             <li><a href="#">Contacts</a></li>
           </ul>
         </nav>
         <article>
           <h1>An Exciting Blog Post</h1>
           <img src="images/square6.jpg" alt="placeholder" class="feature" />
           <p>
             Veggies es bonus vobis, proinde vos postulo essum magis kohlrabi
             welsh onion daikon amaranth tatsoi tomatillo melon azuki bean
             garlic.
           </p>

           <p>
             Turnip greens yarrow ricebean rutabaga endive cauliflower sea
             lettuce kohlrabi amaranth water spinach avocado daikon napa
             asparagus winter purslane kale. Celery potato scallion desert
             raisin horseradish spinach carrot soko. Lotus root water spinach
             fennel kombu maize bamboo shoot green bean swiss chard seakale
             pumpkin onion chickpea gram corn pea. Brussels sprout coriander
             water chestnut gourd swiss chard wakame kohlrabi beetroot carrot
             watercress. Corn amaranth salsify bunya nuts nori azuki bean
             chickweed potato bell pepper artichoke.
           </p>

           <p>
             Gumbo beet greens corn soko endive gumbo gourd. Parsley shallot
             courgette tatsoi pea sprouts fava bean collard greens dandelion
             okra wakame tomato. Dandelion cucumber earthnut pea peanut soko
             zucchini.
           </p>

           <p>
             Nori grape silver beet broccoli kombu beet greens fava bean potato
             quandong celery. Bunya nuts black-eyed pea prairie turnip leek
             lentil turnip greens parsnip. Sea lettuce lettuce water chestnut
             eggplant winter purslane fennel azuki bean earthnut pea sierra
             leone bologi leek soko chicory celtuce parsley jícama salsify.
           </p>

           <p>
             Celery quandong swiss chard chicory earthnut pea potato. Salsify
             taro garlic gram celery wattle seed collard greens nori. Grape
             wattle seed kombu beetroot horseradish carrot squash brussels
             sprout chard.
           </p>

           <p>
             Veggies es bonus vobis, proinde vos postulo essum magis kohlrabi
             welsh onion daikon amaranth tatsoi tomatillo melon azuki bean
             garlic.
           </p>

           <p>
             Turnip greens yarrow ricebean rutabaga endive cauliflower sea
             lettuce kohlrabi amaranth water spinach avocado daikon napa
             asparagus winter purslane kale. Celery potato scallion desert
             raisin horseradish spinach carrot soko. Lotus root water spinach
             fennel kombu maize bamboo shoot green bean swiss chard seakale
             pumpkin onion chickpea gram corn pea. Brussels sprout coriander
             water chestnut gourd swiss chard wakame kohlrabi beetroot carrot
             watercress. Corn amaranth salsify bunya nuts nori azuki bean
             chickweed potato bell pepper artichoke.
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
   /* General styles */

   * {
     box-sizing: border-box;
   }

   body {
     background-color: white;
     color: #333333;
     margin: 0;
     font: 1.2em / 1.6 sans-serif;
     padding: 0 20px 20px 20px;
   }

   img {
     display: block;
     border: 1px solid black;
   }

   /* Mobile layout */

   header {
     padding: 50px 0;
     display: flex;
     gap: 20px;
     justify-content: space-between;
     align-items: center;
   }

   .logo {
     font-size: 200%;
   }

   button {
     font-size: 250%;
     border: 0;
     background: none;
     cursor: pointer;
   }

   button:hover,
   button:focus {
     text-shadow: 0 0 2px black;
   }

   nav {
     position: fixed;
     inset: 10%;
     background-color: white;
     display: none;
   }

   nav ul {
     margin: 0;
     padding: 0;
     list-style: none;
     text-align: center;
     height: 100%;
     display: flex;
     gap: 10px;
     flex-direction: column;
   }

   nav li {
     flex: 1;
   }

   nav a {
     display: flex;
     justify-content: center;
     align-items: center;
     font-size: 150%;
     width: 100%;
     height: 100%;
     background-color: black;
     color: white;
     text-decoration: none;
   }

   nav a:hover,
   nav a:focus {
     font-weight: bold;
   }

   .photos {
     list-style: none;
     margin: 0;
     padding: 0;
     display: grid;
     gap: 5px;
     grid-template-columns: 1fr 1fr;
   }

   .feature {
     width: 200px;
     float: left;
     margin: 8px 30px 20px 0;
   }
   ```

4. All'interno della cartella, creare un file `script.js` e incollarvi il seguente contenuto:

   ```js
   const btn = document.querySelector("button");
   const nav = document.querySelector("nav");

   function showNav() {
     nav.style.display = "block";
   }

   function hideNav() {
     nav.style.display = "none";
   }

   function hideNavEsc(e) {
     if (e.key === "Escape") {
       nav.style.display = "none";
     }
   }

   function handleEventListeners() {
     if (matchMedia("(width > 800px)").matches) {
       btn.removeEventListener("click", showNav);
       nav.removeEventListener("click", hideNav);
       document.body.removeEventListener("keydown", hideNavEsc);
       if (nav.style.display === "none") {
         nav.style.display = "block";
       }
     } else {
       btn.addEventListener("click", showNav);
       nav.addEventListener("click", hideNav);
       document.body.addEventListener("keydown", hideNavEsc);
       if (nav.style.display === "block") {
         nav.style.display = "none";
       }
     }
   }

   handleEventListeners();

   window.addEventListener("resize", handleEventListeners);
   ```

5. All'interno della cartella, creare una sottocartella chiamata `images` e salvare al suo interno i seguenti file di immagine:
   - [`square1.jpg`](https://mdn.github.io/shared-assets/images/examples/learn/balloons/square1.jpg)
   - [`square2.jpg`](https://mdn.github.io/shared-assets/images/examples/learn/balloons/square2.jpg)
   - [`square3.jpg`](https://mdn.github.io/shared-assets/images/examples/learn/balloons/square3.jpg)
   - [`square4.jpg`](https://mdn.github.io/shared-assets/images/examples/learn/balloons/square4.jpg)
   - [`square5.jpg`](https://mdn.github.io/shared-assets/images/examples/learn/balloons/square5.jpg)
   - [`square6.jpg`](https://mdn.github.io/shared-assets/images/examples/learn/balloons/square6.jpg)
6. Salvare i file e caricare `index.html` in un browser, pronti per eseguire i test. Il punto di partenza della pagina dovrebbe assomigliare a questo quando viene visualizzato in un viewport stretto:

   ![Punto di partenza dell'attività mobile-first. Un layout a colonna singola con un logo in alto e un'icona del menu hamburger, seguiti da un'intestazione di primo livello e da contenuto testuale con un'immagine flottante.](rwd-task-start.png)

## Descrizione del progetto

Il contenuto fornito per questo esempio è lo stesso della sfida precedente, [Comprensione dei layout fondamentali](/it/docs/Learn_web_development/Core/CSS_layout/Fundamental_Layout_Comprehension), con alcune piccole differenze strutturali. Ha inoltre un layout per lo più completo fin dall'inizio, anche se, come si può notare osservandolo, appare terribile in un viewport widescreen.

Questo accade perché viene fornito inizialmente un layout mobile. Si noti come al menu di navigazione si acceda premendo l'icona del "menu hamburger" e come possa essere chiuso facendo clic su una voce di menu o premendo il tasto <kbd>Esc</kbd>. Questa funzionalità è gestita mediante JavaScript e funziona soltanto quando il viewport è largo meno di `800px`, in modo da non interferire con i layout per schermi più ampi che verranno implementati.

Nello specifico, occorre implementare due layout: il primo viene attivato quando la larghezza supera `800px`, mentre il secondo viene attivato sopra `1300px`. Occorrerà inoltre correggere un paio di problemi nel codice esistente e implementare alcune funzionalità aggiuntive.

### Correzione di alcuni problemi di visualizzazione

Prima di tutto, è necessario risolvere un paio di problemi lasciati intenzionalmente nel modello iniziale.

1. Al momento, i layout non vengono visualizzati correttamente nei browser mobili. Aggiungere un tag al `<head>` del documento `<html>` per correggere il problema.
2. Con la finestra del browser impostata su una larghezza ridotta, osservare la parte inferiore della pagina: la galleria fotografica non viene visualizzata correttamente perché le fotografie fuoriescono dai rispettivi contenitori. Aggiungere una dichiarazione al file CSS per risolvere il problema.

### Creazione del layout intermedio

Il layout intermedio deve essere applicato alla pagina oltre una larghezza del viewport di `800px`. Per completare il layout, seguire questi passaggi:

1. Nascondere il `<button>` del menu e mostrare il `<nav>`. Il menu a comparsa deve essere usato solo nel layout mobile.
2. Modificare il posizionamento del `<nav>` in modo che, anziché sovrapporsi alla maggior parte del contenuto, si trovi nella parte superiore del sito, appena sotto il logo "My exciting website!". Dovrebbe inoltre rimanere fissato alla parte superiore del viewport una volta che il contenuto viene fatto scorrere fino a quel punto.
3. Le voci dell'elenco di navigazione sono attualmente visualizzate in colonna. Per questo layout, devono invece essere visualizzate come una riga lungo l'intera larghezza dello schermo.
4. Regolare gli elementi `<a>` all'interno delle voci dell'elenco per assegnare loro `10px` di padding superiore e inferiore e una dimensione del carattere minore, ad esempio `100%`.
5. Gli elementi `<nav>`, `<article>` e `<aside>` sono tutti figli dell'elemento `<main>`. Disporli in una griglia, utilizzando aree del modello di griglia denominate, con la seguente struttura:

   ```plain
   ┌----------------------------------------┐
   |                  <nav>                 |
   ├------------------------------┬---------┤
   |           <article>          | <aside> |
   |                              |         |
   ```

   L'elemento `<article>` deve avere una larghezza tripla rispetto all'elemento `<aside>`; entrambi gli elementi devono trovarsi sulla stessa riga. L'elemento `<nav>` deve trovarsi su una riga separata sopra gli altri due e occupare tutta la larghezza disponibile. Includere inoltre uno spazio di `20px` tra i diversi elementi della griglia.

### Creazione del layout widescreen

Il layout widescreen deve essere applicato alla pagina oltre una larghezza del viewport di `1300px`. Per completare il layout, seguire questi passaggi:

1. Modificare il layout a griglia implementato per il layout intermedio con uno diverso, utilizzando nuovamente aree del modello di griglia denominate. Questa volta, la struttura deve essere la seguente:

   ```plain
   ┌--------┬------------------------------┬---------┐
   | <nav>  |           <article>          | <aside> |
   |        |                              |         |
   ```

   Questa volta, tutti e tre gli elementi si trovano sulla stessa riga. Gli elementi `<nav>` e `<aside>` devono occupare la stessa larghezza; l'elemento `<article>` deve essere largo tre volte gli altri due.

2. Le voci dell'elenco di navigazione vengono visualizzate in riga come risultato del layout intermedio; affinché il layout widescreen funzioni, è necessario regolare lo stile dell'elenco affinché le voci vengano nuovamente visualizzate in colonna, come nel layout mobile.
3. Le voci dell'elenco hanno attualmente un valore `flex` pari a `1`, il che significa che si estendono per riempire l'intera altezza della colonna. Regolare il valore di questa proprietà in modo che le voci di navigazione siano alte solo quanto il loro contenuto e il `padding` impostato.

### Implementazione della tipografia responsive

Regolare lo stile degli elementi `<h1>` e `<h2>` affinché:

1. Il loro `margin` superiore e inferiore venga rimosso, così da adattarsi più strettamente al contenuto soprastante e sottostante.
2. La loro dimensione cambi in modo responsive quando il viewport viene allargato o ristretto, pur rimanendo zoomabile. Scegliere unità appropriate affinché le intestazioni occupino bene lo spazio disponibile senza andare a capo su più righe.

### Adattamento del layout per la stampa

Aggiungere un blocco di stile che rimuova gli elementi `<button>` e `<nav>` dal layout durante la stampa della pagina.

## Suggerimenti e consigli

1. Non è necessario modificare il JavaScript per completare questa sfida.
2. Esistono diversi modi per completare alcune delle attività nella descrizione del progetto e spesso non esiste un unico approccio giusto o sbagliato. Provare alcuni approcci diversi e osservare quale funziona meglio. Prendere appunti durante gli esperimenti.
3. Talvolta un valore di proprietà impostato per un layout precedente causa problemi nei layout successivi. Parte dell'abilità nella progettazione responsive consiste nel sapere quando annullare o sovrascrivere valori di proprietà impostati in precedenza.

## Esempio

La seguente schermata mostra l'aspetto che dovrebbe avere il layout intermedio completato:

![Layout intermedio completato del sito dell'attività rwd. Un logo in alto, seguito da un menu di navigazione orizzontale, seguito da due colonne: contenuto testuale a sinistra e una galleria fotografica a destra.](rwd-task-middle.png)

La seguente schermata mostra l'aspetto che dovrebbe avere il layout widescreen completato:

![Layout widescreen completato del sito dell'attività rwd. Un logo in alto, seguito da tre colonne: menu di navigazione verticale a sinistra, contenuto testuale al centro e una galleria fotografica a destra.](rwd-task-widescreen.png)

<details>
<summary>Fare clic qui per mostrare una possibile soluzione</summary>

Per visualizzare correttamente i layout nei browser mobili, occorre aggiungere un tag `<meta>` viewport all'interno del `<head>` del documento HTML:

```html
<meta name="viewport" content="width=device-width" />
```

Il CSS completato dovrebbe essere simile al seguente:

```css
/* General styles */

* {
  box-sizing: border-box;
}

body {
  background-color: white;
  color: #333333;
  margin: 0;
  font: 1.2em / 1.6 sans-serif;
  padding: 0 20px 20px 20px;
}

img {
  display: block;
  border: 1px solid black;
  /* Solution: Stop the photographs from breaking out of
  their containers */
  max-width: 100%;
}

/* Mobile layout */

header {
  padding: 50px 0;
  display: flex;
  gap: 20px;
  justify-content: space-between;
  align-items: center;
}

.logo {
  font-size: 200%;
}

button {
  font-size: 250%;
  border: 0;
  background: none;
  cursor: pointer;
}

button:hover,
button:focus {
  text-shadow: 0 0 2px black;
}

nav {
  position: fixed;
  inset: 10%;
  background-color: white;
  display: none;
}

nav ul {
  margin: 0;
  padding: 0;
  list-style: none;
  text-align: center;
  height: 100%;
  display: flex;
  gap: 10px;
  flex-direction: column;
}

nav li {
  flex: 1;
}

nav a {
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 150%;
  width: 100%;
  height: 100%;
  background-color: black;
  color: white;
  text-decoration: none;
}

nav a:hover,
nav a:focus {
  font-weight: bold;
}

.photos {
  list-style: none;
  margin: 0;
  padding: 0;
  display: grid;
  gap: 5px;
  grid-template-columns: 1fr 1fr;
}

.feature {
  width: 200px;
  float: left;
  margin: 8px 30px 20px 0;
}

/* Solution: Creating the middle layout (breakpoint: 800px) */

@media (width > 800px) {
  /* Sort out navigation styling for middle breakpoint */
  button {
    display: none;
  }

  nav {
    display: block;
    inset: unset;
    position: sticky;
    top: 0;
  }

  nav ul {
    flex-direction: row;
  }

  nav a {
    font-size: 100%;
    padding: 10px 0;
  }

  /* Create grid layout for middle breakpoint */

  nav {
    grid-area: nav;
  }

  article {
    grid-area: main;
  }

  aside {
    grid-area: photos;
  }

  .grid {
    display: grid;
    grid-template-columns: 3fr 1fr;
    grid-template-areas:
      "nav nav"
      "main photos";
    gap: 20px;
  }
}

/* Solution: Creating the widescreen layout (breakpoint: 1300px) */

@media (width > 1300px) {
  .grid {
    grid-template-columns: 1fr 3fr 1fr;
    grid-template-areas: "nav main photos";
  }

  nav ul {
    flex-direction: column;
  }

  nav li {
    flex: unset;
  }
}

/* 4. Solution: Implementing responsive typography */

h1 {
  font-size: calc(1.3rem + 3vw);
  margin: 0;
}

h2 {
  font-size: calc(1rem + 2vw);
  margin: 0;
}

/* 5. Solution: Adjusting the layout for print */

@media print {
  nav,
  button {
    display: none;
  }
}
```

</details>

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Test_your_skills/Responsive_design", "Learn_web_development/Core/Scripting", "Learn_web_development/Core/CSS_layout")}}
