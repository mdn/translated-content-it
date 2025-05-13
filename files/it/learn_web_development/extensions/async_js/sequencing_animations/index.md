---
title: "Sfida: Sequenza di animazioni"
short-title: "Sfida: Sequenza di animazioni"
slug: Learn_web_development/Extensions/Async_JS/Sequencing_animations
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenu("Learn_web_development/Extensions/Async_JS/Introducing_workers", "Learn_web_development/Extensions/Async_JS")}}

In questa sfida aggiornerà una pagina per eseguire una serie di animazioni in sequenza. Per fare ciò utilizzerà alcune delle tecniche che abbiamo appreso nell'articolo [Come usare le Promesse](/it/docs/Learn_web_development/Extensions/Async_JS/Promises).

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Una buona comprensione dei fondamenti di JavaScript, 
        come utilizzare le API basate su promesse.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Verificare la comprensione di come usare le API basate su promesse.</td>
    </tr>
  </tbody>
</table>

## Punto di partenza

Faccia una copia locale dei file su <https://github.com/mdn/learning-area/tree/main/javascript/asynchronous/sequencing-animations/start>. Contiene quattro file:

- alice.svg
- index.html
- main.js
- style.css

L'unico file che dovrà modificare è "main.js".

Se apre il file "index.html" in un browser vedrà tre immagini disposte in diagonale:

![Screenshot della pagina della sfida sequencing-animations](./sequencing-animations.png)

Le immagini sono tratte dalla nostra guida a [Utilizzare la Web Animations API](/it/docs/Web/API/Web_Animations_API/Using_the_Web_Animations_API).

> [!NOTE]
> Se si trova in difficoltà, può contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Descrizione del progetto

Desideriamo aggiornare questa pagina per applicare un'animazione a tutte e tre le immagini, una dopo l'altra. Così quando la prima termina animiamo la seconda, e quando la seconda termina animiamo la terza.

L'animazione è già definita in "main.js": ruota semplicemente l'immagine e la riduce finché non scompare.

Per avere un'idea più chiara di come vuole che la pagina funzioni, [dia un'occhiata all'esempio finale](https://mdn.github.io/learning-area/javascript/asynchronous/sequencing-animations/finished/). Nota che le animazioni vengono eseguite solo una volta: per vederle nuovamente in esecuzione, ricarichi la pagina.

## Passaggi da completare

### Animare la prima immagine

Stiamo utilizzando la [Web Animations API](/it/docs/Web/API/Web_Animations_API) per animare le immagini, in particolare il metodo [`element.animate()`](/it/docs/Web/API/Element/animate).

Aggiorni "main.js" per aggiungere una chiamata a `alice1.animate()`, come segue:

```js
const aliceTumbling = [
  { transform: "rotate(0) scale(1)" },
  { transform: "rotate(360deg) scale(0)" },
];

const aliceTiming = {
  duration: 2000,
  iterations: 1,
  fill: "forwards",
};

const alice1 = document.querySelector("#alice1");
const alice2 = document.querySelector("#alice2");
const alice3 = document.querySelector("#alice3");

alice1.animate(aliceTumbling, aliceTiming);
```

Ricarichi la pagina, e dovrebbe vedere la prima immagine ruotare e ridursi.

### Animare tutte le immagini

Successivamente, desideriamo animare `alice2` quando `alice1` ha terminato, e `alice3` quando `alice2` ha terminato.

Il metodo `animate()` restituisce un oggetto [`Animation`](/it/docs/Web/API/Animation). Questo oggetto ha una proprietà `finished`, che è una `Promise` che viene soddisfatta quando l'animazione ha terminato di riprodursi. Quindi possiamo utilizzare questa promessa per sapere quando iniziare la prossima animazione.

Le chiediamo di provare alcuni modi diversi per implementare ciò, per rafforzare diversi modi di utilizzare le promesse.

1. Primo, implementi qualcosa che funzioni, ma che abbia la versione della promesse del problema del "callback hell" che abbiamo visto nella nostra [discussione sull'uso dei callback](/it/docs/Learn_web_development/Extensions/Async_JS/Introducing#callbacks).

2. Successivamente, lo implementi come una [catena di promesse](/it/docs/Learn_web_development/Extensions/Async_JS/Promises#chaining_promises). Nota che esistono alcuni modi diversi per scrivere questo, a causa delle diverse forme che si possono usare per una [funzione freccia](/it/docs/Learn_web_development/Core/Scripting/Functions#arrow_functions). Provi alcune forme diverse. Qual è la più concisa? Quale trova la più leggibile?

3. Infine, lo implementi usando [`async` e `await`](/it/docs/Learn_web_development/Extensions/Async_JS/Promises#async_and_await).

Ricordi che `element.animate()` non restituisce una `Promise`: restituisce un oggetto `Animation` con una proprietà `finished` che è una `Promise`.

{{PreviousMenu("Learn_web_development/Extensions/Async_JS/Introducing_workers", "Learn_web_development/Extensions/Async_JS")}}
