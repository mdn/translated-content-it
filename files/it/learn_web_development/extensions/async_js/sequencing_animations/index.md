---
title: "Sfida: Sequenziamento delle animazioni"
short-title: "Sfida: Sequenza di animazioni"
slug: Learn_web_development/Extensions/Async_JS/Sequencing_animations
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenu("Learn_web_development/Extensions/Async_JS/Introducing_workers", "Learn_web_development/Extensions/Async_JS")}}

In questa sfida aggiornerai una pagina per riprodurre una serie di animazioni in sequenza. Per farlo utilizzerai alcune delle tecniche che abbiamo appreso nell'articolo [Come usare le Promises](/it/docs/Learn_web_development/Extensions/Async_JS/Promises).

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Una comprensione ragionevole dei fondamenti di JavaScript,
        come utilizzare le API basate su Promise.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Verificare la comprensione di come utilizzare le API basate su Promise.</td>
    </tr>
  </tbody>
</table>

## Punto di partenza

Fai una copia locale dei file su <https://github.com/mdn/learning-area/tree/main/javascript/asynchronous/sequencing-animations/start>. Contiene quattro file:

- alice.svg
- index.html
- main.js
- style.css

L'unico file che dovrai modificare è "main.js".

Se apri "index.html" in un browser vedrai tre immagini disposte diagonalmente:

![Schermata della pagina della sfida del sequenziamento delle animazioni](./sequencing-animations.png)

Le immagini sono tratte dalla nostra guida su [Utilizzo delle Web Animations API](/it/docs/Web/API/Web_Animations_API/Using_the_Web_Animations_API).

> [!NOTE]
> Se ti blocchi, puoi contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Breve del progetto

Vogliamo aggiornare questa pagina in modo da applicare un'animazione a tutte e tre le immagini, una dopo l'altra. Quindi, quando la prima ha finito, animiamo la seconda, e quando la seconda ha finito, animiamo la terza.

L'animazione è già definita in "main.js": ruota l'immagine e la riduce fino a quando scompare.

Per darti un'idea più precisa di come vorremmo che la pagina funzionasse, [dai un'occhiata all'esempio finito](https://mdn.github.io/learning-area/javascript/asynchronous/sequencing-animations/finished/). Nota che le animazioni vengono eseguite una sola volta: per rivederle, ricarica la pagina.

## Passaggi da completare

### Animare la prima immagine

Stiamo utilizzando le [Web Animations API](/it/docs/Web/API/Web_Animations_API) per animare le immagini, specificamente il metodo [`element.animate()`](/it/docs/Web/API/Element/animate).

Aggiorna "main.js" per aggiungere una chiamata a `alice1.animate()`, come questa:

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

Ricarica la pagina, e dovresti vedere la prima immagine ruotare e ridursi.

### Animare tutte le immagini

Successivamente, vogliamo animare `alice2` quando `alice1` ha finito, e `alice3` quando `alice2` ha finito.

Il metodo `animate()` restituisce un oggetto [`Animation`](/it/docs/Web/API/Animation). Questo oggetto ha una proprietà `finished`, che è una `Promise` che viene risolta quando l'animazione ha finito di riprodursi. Quindi possiamo utilizzare questa promise per sapere quando avviare la prossima animazione.

Vorremmo che provassi alcuni modi diversi per implementare questo, per rafforzare i diversi modi di utilizzare le promises.

1. Prima, implementa qualcosa che funzioni, ma ha il problema della "callback hell" delle promises che abbiamo visto nella nostra [discussione sull'uso delle callback](/it/docs/Learn_web_development/Extensions/Async_JS/Introducing#callbacks).

2. Successivamente, implementalo come una [chain di promises](/it/docs/Learn_web_development/Extensions/Async_JS/Promises#chaining_promises). Nota che ci sono diversi modi in cui puoi scrivere questo, a causa delle diverse forme che puoi usare per una [funzione freccia](/it/docs/Learn_web_development/Core/Scripting/Functions#arrow_functions). Prova alcune forme diverse. Qual è la più concisa? Quale trovi più leggibile?

3. Infine, implementalo usando [`async` e `await`](/it/docs/Learn_web_development/Extensions/Async_JS/Promises#async_and_await).

Ricorda che `element.animate()` _non_ restituisce una `Promise`: restituisce un oggetto `Animation` con una proprietà `finished` che è una `Promise`.

{{PreviousMenu("Learn_web_development/Extensions/Async_JS/Introducing_workers", "Learn_web_development/Extensions/Async_JS")}}
