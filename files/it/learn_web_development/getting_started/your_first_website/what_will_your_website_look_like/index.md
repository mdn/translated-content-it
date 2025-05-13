---
title: Come apparirà il suo sito web?
short-title: Come apparirà?
slug: Learn_web_development/Getting_started/Your_first_website/What_will_your_website_look_like
l10n:
  sourceCommit: 04b891269af86287313a1d6e28423560a674cd2d
---

{{NextMenu("Learn_web_development/Getting_started/Your_first_website/Creating_the_content", "Learn_web_development/Getting_started/Your_first_website")}}

_Come apparirà il suo sito web?_ discute il lavoro di pianificazione e progettazione che deve svolgere per il suo sito web prima di scrivere il codice, includendo "Quali informazioni offre il mio sito web?", "Quali font e colori desidero?", e "Cosa fa il mio sito?"

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con il suo sistema operativo, il software di base che utilizzerà per creare un sito web, e i sistemi di file.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>Pianificare un sito web di base.</li>
          <li>Utilizzare un processo di progettazione di base.</li>
          <li>Raccogliere risorse.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Prima di tutto: pianificazione

Prima di fare qualsiasi cosa, serve avere delle idee. Che cosa dovrebbe fare effettivamente il suo sito web? Un sito web può fare praticamente qualsiasi cosa, ma, per il suo primo tentativo, dovrebbe mantenere le cose semplici. Inizieremo creando una semplice pagina web con un titolo, un'immagine e alcuni paragrafi.

Per iniziare, dovrà rispondere a queste domande:

1. **Di cosa tratta il suo sito web?** Le piacciono i cani, New York, o Pac-Man?
2. **Quali informazioni sta presentando sull'argomento?** Scriva un titolo e alcuni paragrafi e pensi a un'immagine che vorrebbe mostrare sulla sua pagina.
3. **Com'è l'aspetto del suo sito web**, in termini semplici e generali? Qual è il colore di sfondo? Che tipo di carattere è appropriato: formale, fumettistico, audace e forte, sottile?

> [!NOTE]
> I progetti complessi necessitano di linee guida dettagliate che includano tutti i dettagli di colori, font, spaziatura tra gli elementi di una pagina, stile di scrittura appropriato, e così via. A volte questo è chiamato guida di design, sistema di progettazione, o libro di marca, e può vedere un esempio al [Firefox Acorn Design System](https://acorn.firefox.com/latest).

## Schizzare il suo design

Successivamente, prenda carta e penna e schizzi approssimativamente come vuole che il suo sito appaia. Per la sua prima semplice pagina web, non c'è molto da disegnare, ma dovrebbe abituarsi a farlo adesso. Aiuta davvero — non deve essere Van Gogh!

![Un disegno e uno schizzo approssimativo di un sito web su carta](website-drawing-scan.png)

> [!NOTE]
> Anche per siti web reali e complessi, i team di progettazione di solito iniziano con schizzi approssimativi su carta e successivamente realizzano mockup digitali usando un editor grafico o tecnologie web.
>
> I team web spesso includono sia un designer grafico che un designer {{Glossary("UX", "user experience")}} (UX). I designer grafici assemblano i visual del sito web. I designer UX hanno un ruolo un po' più astratto nel trattare come gli utenti sperimenteranno e interagiranno con il sito web.

A questo punto, è bene iniziare a mettere insieme il contenuto che apparirà eventualmente sulla sua pagina web. Dovrebbe avere ancora i suoi paragrafi e il titolo di prima. Tenga queste cose a portata di mano.

## Scegliere un colore del tema

Per scegliere un colore, vada a [il selettore di colori](/it/docs/Web/CSS/CSS_colors/Color_picker_tool) e trovi un colore che le piace. Quando clicca su un colore, vedrà uno strano codice a sei caratteri come `#660066`. Questo è chiamato _codice hex_ (abbreviazione di esadecimale), e rappresenta il suo colore. Copi il codice in un luogo sicuro per il momento.

![Strumento selettore di colori sul sito MDN Docs con colori RGB, HSL e HEX](color-picker.png)

## Scegliere un'immagine

Per scegliere un'immagine, vada a [Google Immagini](https://www.google.com/imghp) e cerchi qualcosa di adatto.

1. Quando trova l'immagine che vuole, clicchi sull'immagine per ottenere una vista ingrandita di essa.
2. Faccia clic con il tasto destro (Ctrl + clic su un Mac), scelga _Salva immagine come..._, e scelga un posto sicuro per salvare l'immagine.

![Risultati di ricerca per un termine di ricerca su Google Immagini](updated-google-images.png)

Si noti che la maggior parte delle immagini sul web, inclusi Google Immagini, sono protette da copyright. Per ridurre la probabilità di violare il copyright, può utilizzare il filtro di licenza di Google. Clicchi sul pulsante _Strumenti_, quindi sull'opzione _Diritti di utilizzo_ che appare sotto. Dovrebbe scegliere l'opzione _Licenze Creative Commons_.

![Risultati di ricerca filtrati per ottenere immagini di licenze Creative Commons su Google Immagini](updated-google-images-licensing.png)

## Scegliere un font

Esiste un insieme di font chiamato [i font sicuri per il web](/it/docs/Learn_web_development/Core/Text_styling/Fundamentals#web_safe_fonts) — come Arial, Times New Roman, o Courier New — che sono generalmente disponibili sulla maggior parte dei sistemi informatici. Se utilizza uno di questi font sul suo sito web, il browser caricherà il file del font disponibile sul computer dell'utente.

Tuttavia, se desidera utilizzare altri font non generalmente disponibili sui dispositivi, deve includerli insieme ai file del sito web o riferirsi ai file del font da un servizio di font di terze parti così che il browser possa scaricarli secondo necessità. [Google Fonts](https://fonts.google.com/) è uno di questi servizi che fornisce accesso a molti font.

Usiamo Google Font per scegliere un font per il suo sito web:

1. Vada a [Google Fonts](https://fonts.google.com/).
2. Scorra l'elenco dei font finché non ne trova uno che le piace. Se sta avendo difficoltà a trovarne uno, può usare i filtri disponibili nell'altra colonna per restringere la ricerca.
3. Clicchi sull'opzione del font, poi nella pagina successiva clicchi sul pulsante "Ottieni font".
4. Nella pagina successiva, clicchi su "Ottieni codice di incorporamento".
5. Copi entrambi i blocchi di codice forniti e li salvi in un luogo sicuro per un uso successivo.

> [!NOTE]
> Come per le immagini, molti font sono protetti da licenze, il che significa che non può necessariamente usarli liberamente su siti web commerciali. Andrà bene per ora mentre lavora su esempi di apprendimento, ma tenga questo a mente quando sceglie font per siti web reali.

{{NextMenu("Learn_web_development/Getting_started/Your_first_website/Creating_the_content", "Learn_web_development/Getting_started/Your_first_website")}}
