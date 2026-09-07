---
title: "Sfida: Correggere gli stili della pagina del blog"
short-title: "Sfida: Correggere gli stili del blog"
slug: Learn_web_development/Core/Styling_basics/Fixing_blog_styles
l10n:
  sourceCommit: 50a1895c9c499b1b9207f7af945a0fe45de58cca
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Test_your_skills/Cascade", "Learn_web_development/Core/Styling_basics/Values_and_units", "Learn_web_development/Core/Styling_basics")}}

In questa sfida viene fornito un esempio di pagina blog con uno stile applicato solo in parte. Occorre correggere alcuni problemi nel CSS esistente e aggiungere alcuni stili per completarla. Durante il percorso verranno messe alla prova le conoscenze di selettori, box model e conflitti/cascade.

## Punto di partenza

Per iniziare, fare clic sul pulsante **Play** in uno dei pannelli di codice seguenti per aprire l'esempio fornito nel Playground MDN. Quindi, seguire le istruzioni nella sezione [Descrizione del progetto](#descrizione_del_progetto) per applicare correttamente lo stile alla pagina.

```html live-sample___blog-start live-sample___blog-finish
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>Sizing a blog page challenge</title>
    <link href="style.css" rel="stylesheet" />
  </head>
  <body>
    <header>
      <h1>A most excellent blog</h1>
      <nav>
        <ul>
          <li><a href="#">Home</a></li>
          <li><a href="#">Blog</a></li>
          <li><a href="#">About</a></li>
          <li><a href="#">Contact</a></li>
        </ul>
      </nav>
    </header>
    <main>
      <section id="introduction" class="highlight">
        <h2>Our newest post</h2>
        <p>
          Laoreet lorem curae lectus blandit conubia vel semper laoreet congue
          at taciti.
          <a href="#">Phasellus hac consectetur iaculis dui</a> sapien iaculis
          hac ultricies per luctus. Suscipit mattis lacus semper in porta
          phasellus sollicitudin ipsum fermentum phasellus sapien. Inceptos
          etiam placerat porttitor finibus auctor at platea hendrerit aenean
          laoreet elit lorem odio.
        </p>
      </section>
      <section>
        <h2>Exciting content</h2>
        <p>
          Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do
          eiusmod tempor incididunt ut labore et dolore magna aliqua. Proin
          tortor purus <a href="#">platea sit eu id</a> nisi litora libero.
          Neque vulputate consequat ac amet augue blandit maximus aliquet
          congue. Pharetra vestibulum posuere ornare
          <a href="#">faucibus fusce dictumst</a> orci aenean eu facilisis ut
          volutpat commodo senectus purus himenaeos fames primis convallis nisi.
        </p>
        <ul>
          <li>Lorem ipsum dolor</li>
          <li>Neque vulputate consequat</li>
          <li>Phasellus fermentum malesuada</li>
          <li>Curabitur semper venenatis</li>
          <li>Duis lectus porta mattis</li>
        </ul>
        <p>
          Phasellus fermentum malesuada phasellus netus dictum aenean placerat
          egestas amet.
          <a href="#">Ornare taciti semper dolor tristique</a> morbi. Sem leo
          tincidunt aliquet semper eu lectus scelerisque quis. Sagittis vivamus
          mollis nisi mollis enim fermentum laoreet.
        </p>
        <h2>More exciting content</h2>
        <p>
          Curabitur semper venenatis lectus viverra ex dictumst nulla maximus.
          Primis iaculis elementum conubia feugiat venenatis dolor augue ac
          blandit nullam ac <a href="#">phasellus turpis</a> feugiat mollis.
          Duis lectus porta mattis imperdiet vivamus augue litora lectus arcu.
          Justo torquent pharetra volutpat ad blandit bibendum
          <a href="#">accumsan nec elit cras</a> luctus primis ipsum gravida
          class congue.
        </p>
        <p>
          Vehicula etiam elementum finibus enim duis feugiat commodo adipiscing
          tortor <a href="#">tempor elit</a>. Et mollis consectetur habitant
          turpis tortor consectetur adipiscing vulputate dolor lectus iaculis
          convallis adipiscing. Nam hendrerit
          <a href="#">dignissim condimentum ullamcorper diam</a> morbi eget
          consectetur odio in sagittis.
        </p>
      </section>
      <section id="summary" class="highlight">
        <h2>Summary</h2>
        <p>
          Et arcu tortor lorem ac primis ac suspendisse lectus nulla. Habitant
          fermentum <a href="#">leo facilisis lobortis</a> risus lobortis
          maximus gravida. Euismod fames maecenas imperdiet senectus
          <a href="#">nec nisi amet pellentesque felis</a> vitae vestibulum
          integer nec tellus. Eros posuere lacinia et tellus quis fames mattis
          quisque mauris placerat rhoncus pretium sed consectetur
          <a href="#">convallis</a>.
        </p>
      </section>
    </main>
    <footer class="highlight">
      <p>©️ 2025 Nobody</p>
    </footer>
  </body>
</html>
```

```css live-sample___blog-start
/* Basic type and text */

body {
  font: 1.2em / 1.5 system-ui;
  width: clamp(480px, 70%, 1000px);
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

a:hover {
  text-decoration: none;
}

/* Nav menu */

ul {
  display: flex;
  padding: 0;
  list-style-type: none;
  justify-content: space-between;
  gap: 10px;
}

li {
  flex: 1;
}

a {
  text-decoration: none;
  color: black;
  background-color: yellowgreen;
  text-align: center;
  padding: 10px;
}

a:hover {
  background-color: goldenrod;
}

/* Intro and summary */

.highlight {
  margin-top: 0;
  background-color: darkslategray;
  color: cornsilk;
}

.highlight a {
  color: purple;
}

/* Footer */

footer {
  margin-top: 20px;
  background-color: goldenrod;
  text-shadow: 1px 1px 1px black;
}
```

{{embedlivesample("blog-start", "100%", 500)}}

## Descrizione del progetto

L'esempio di blog fornito non è completo e il codice esistente presenta alcuni problemi. Seguire i passaggi seguenti per completare il progetto.

1. Si desidera che ogni elemento in questa pagina utilizzi il box model alternativo. Aggiungere una regola al foglio di stile per ottenere questo risultato.

2. C'è un problema con le regole per il menu di navigazione: gli stili sono per lo più corretti, ma influenzano anche l'altra lista non ordinata e i link del contenuto, facendoli apparire male. È possibile modificare i selettori di queste regole affinché abbiano come target solo il menu di navigazione?

3. In realtà, c'è un altro problema con il menu di navigazione: gli elementi `<a>` non occupano l'intera larghezza dei rispettivi elementi genitori `<li>`, come dovrebbero. È possibile modificare il modo in cui vengono visualizzati affinché occupino l'intera larghezza?

4. Sia per i link del menu di navigazione sia per i normali link del contenuto, viene impostato uno stile diverso al passaggio del mouse, in modo che gli utenti con mouse possano vedere su quale link stanno passando. Questo presenta un problema di accessibilità per gli utenti di tastiera, che non potranno vedere questi stili. È possibile modificare i selettori nelle regole pertinenti affinché questi stili vengano applicati anche quando un utente di tastiera raggiunge i link usando il tasto Tab?

5. Si desidera che introduzione, riassunto e footer abbiano `20px` di padding su tutti i lati. Aggiungere una singola dichiarazione da qualche parte nel foglio di stile per ottenere questo risultato.

6. Aggiungere una regola che selezioni la prima riga di ogni paragrafo che appare immediatamente dopo un'intestazione di secondo livello, rendendola in grassetto.

7. Come seguito della domanda precedente, è possibile pensare a un modo per rendere in grassetto la prima riga di ogni paragrafo che segue un'intestazione di secondo livello, ma solo quando l'elemento genitore non è l'introduzione, il riassunto o il footer? È possibile farlo in diversi modi, alcuni più concisi di altri.

8. Più avanti, viene usato `.highlight a` per selezionare gli elementi `<a>` all'interno dell'introduzione e del riassunto, colorandoli di `purple` nella regola associata. Tuttavia, questo non va bene: il contrasto del colore è pessimo. Supponendo che non sia consentito modificare o rimuovere quella regola, è possibile aggiungere un'altra regola prima di essa nell'ordine sorgente che colori gli elementi `<a>` di `yellow`? Essendo prima di essa nell'ordine sorgente, dovrà avere una specificità maggiore.

9. Si può osservare che si sta cercando di selezionare il `<footer>` alla fine del foglio di stile e di assegnargli un'ombra al testo, un margine per allontanarlo dal riassunto e un colore di sfondo diverso per farlo risaltare. Tuttavia, non riceve gli stili desiderati relativi a margine e colore di sfondo perché la regola `.highlight` ha una specificità maggiore, quindi le sue dichiarazioni prevalgono. È possibile modificare il selettore per assicurarsi che tali stili vengano applicati?

## Suggerimenti e consigli

- Usare il [W3C CSS Validator](https://jigsaw.w3.org/css-validator/) per individuare errori involontari nel CSS, che altrimenti potrebbero passare inosservati, così da poterli correggere.
- Non è necessario modificare l'HTML in alcun modo.

## Esempio

Il progetto completato dovrebbe apparire così:

{{embedlivesample("blog-finish", "100%", 500)}}

<details>
<summary>Fare clic qui per visualizzare la soluzione</summary>

Il CSS completato è il seguente:

```css live-sample___blog-finish
/* Basic type and text */

/* Solution: Set alternative box model on all elements */
* {
  box-sizing: border-box;
}

body {
  font: 1.2em / 1.5 system-ui;
  width: clamp(480px, 70%, 1000px);
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

/* Solution: Update :hover styles to also apply on :focus
so that keyboard users can see the updated styles when
they tab to links */
a:hover,
a:focus {
  text-decoration: none;
}

/* Solution: bold ::first-line of each paragraph that appears
right after a second-level heading, but only when the parent
element is not the introduction, summary, or footer
(use :not(.highlight) to specify this second bit) */
section:not(.highlight) h2 + p::first-line {
  font-weight: bold;
}

/*

Alternative to the above solution: bold all instances first,
then remove it from those inside an element with the highlight
class afterwards

section h2 + p::first-line {
  font-weight: bold;
}

.highlight h2 + p::first-line {
  font-weight: normal;
}

*/

/* Nav menu */

/* Solution: Adjust nav rule selectors to only
target the <nav> menu */

nav ul {
  display: flex;
  padding: 0;
  list-style-type: none;
  justify-content: space-between;
  gap: 10px;
}

nav li {
  flex: 1;
}

nav a {
  text-decoration: none;
  color: black;
  background-color: yellowgreen;
  /* Solution: Set nav <a> elements to display: block so they span
  the full width of their <li> element parents */
  display: block;
  text-align: center;
  padding: 10px;
}

/* Solution: Update :hover styles to also apply on :focus
so that keyboard users can see the updated styles when
they tab to links */
nav a:hover,
nav a:focus {
  background-color: goldenrod;
}

/* Intro and summary */

.highlight {
  margin-top: 0;
  background-color: darkslategray;
  color: cornsilk;
  /* Solution: Set 20px of padding on all sides of the
  introduction, summary, and footer. They all have the
  highlight class set on them */
  padding: 20px;
}

/* Solution: Add higher specificity rule above ".highlight a"
rule to override color setting (ID selectors have a higher
specificity than class selectors) */
#introduction a,
#summary a {
  color: yellow;
}

.highlight a {
  color: purple;
}

/* Footer */

/* Solution: Increase footer rule specificity by adding ".highlight"
so that its margin-top and background-color styles are applied */
footer.highlight {
  margin-top: 20px;
  background-color: goldenrod;
  text-shadow: 1px 1px 1px black;
}
```

</details>

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Test_your_skills/Cascade", "Learn_web_development/Core/Styling_basics/Values_and_units", "Learn_web_development/Core/Styling_basics")}}
