---
title: Come apparirà il tuo sito web?
short-title: Come apparirà?
slug: Learn_web_development/Getting_started/Your_first_website/What_will_your_website_look_like
l10n:
  sourceCommit: 04b891269af86287313a1d6e28423560a674cd2d
---

{{NextMenu("Learn_web_development/Getting_started/Your_first_website/Creating_the_content", "Learn_web_development/Getting_started/Your_first_website")}}

_Come apparirà il tuo sito web?_ discute la pianificazione e il lavoro di progettazione che devi fare per il tuo sito web prima di scrivere codice, includendo "Quali informazioni offre il mio sito web?", "Quali font e colori voglio usare?" e "Cosa fa il mio sito?".

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con il sistema operativo del tuo computer, il software di base che utilizzerai per costruire un sito web e i sistemi di file.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Pianificare un sito web di base.</li>
          <li>Utilizzare un processo di progettazione base.</li>
          <li>Raccogliere asset.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Prima di tutto: pianificazione

Prima di fare qualsiasi cosa, hai bisogno di alcune idee. Cosa dovrebbe effettivamente fare il tuo sito web? Un sito web può fare praticamente qualsiasi cosa, ma, per il tuo primo tentativo, dovresti mantenere le cose semplici. Inizieremo creando una semplice pagina web con un'intestazione, un'immagine e alcuni paragrafi.

Per iniziare, dovrai rispondere a queste domande:

1. **Di cosa parla il tuo sito web?** Ti piacciono i cani, New York o Pac-Man?
2. **Quali informazioni stai presentando sull'argomento?** Scrivi un titolo e alcuni paragrafi e pensa a un'immagine che vorresti mostrare sulla tua pagina.
3. **Che aspetto ha il tuo sito web,** in termini semplici e generali? Qual è il colore di sfondo? Che tipo di font è appropriato: formale, cartoonesco, audace e rumoroso, sottile?

> [!NOTE]
> I progetti complessi necessitano di linee guida dettagliate che trattano tutti i dettagli di colori, font, spaziatura tra gli elementi di una pagina, stile di scrittura appropriato, e così via. Questo a volte è chiamato guida di design, sistema di design o brand book, e puoi vedere un esempio nel [Firefox Acorn Design System](https://acorn.firefox.com/latest).

## Abbozzare il tuo design

Successivamente, prendi carta e penna e abbozza il modo in cui vuoi che appaia il tuo sito. Per la tua prima semplice pagina web, non c'è molto da disegnare, ma dovresti abituarti a farlo ora. Aiuta davvero — non devi essere Van Gogh!

![Un disegno e uno schizzo approssimativi di un sito web su carta](website-drawing-scan.png)

> [!NOTE]
> Anche su siti web reali e complessi, i team di progettazione di solito iniziano con schizzi approssimativi su carta e successivamente costruiscono mockup digitali utilizzando un editor grafico o tecnologie web.
>
> I team web includono spesso sia un grafico che un designer {{Glossary("UX", "esperienza utente")}} (UX). I grafici si occupano dell'aspetto visivo del sito web. I designer UX hanno un ruolo più astratto nell'indirizzare come gli utenti sperimenteranno e interagiranno con il sito web.

A questo punto, è bene iniziare a mettere insieme il contenuto che apparirà eventualmente sulla tua pagina web. Dovresti avere ancora i tuoi paragrafi e il titolo da prima. Tienili a portata di mano.

## Scegliere un colore tema

Per scegliere un colore, vai allo [Selettore di Colori](/it/docs/Web/CSS/CSS_colors/Color_picker_tool) e trova un colore che ti piace. Quando clicchi su un colore, vedrai uno strano codice a sei caratteri come `#660066`. Questo è chiamato _codice hex_ (abbreviazione di esadecimale) e rappresenta il tuo colore. Copia il codice in un luogo sicuro per ora.

![Strumento Selettore di Colori sul sito web MDN Docs con colori RGB, HSL, e HEX](color-picker.png)

## Scegliere un'immagine

Per scegliere un'immagine, vai a [Google Immagini](https://www.google.com/imghp) e cerca qualcosa di adatto.

1. Quando trovi l'immagine che vuoi, clicca sull'immagine per ottenere una vista ingrandita.
2. Clicca con il tasto destro sull'immagine (Ctrl + clic su un Mac), scegli _Salva immagine con nome…_, e scegli un luogo sicuro dove salvare la tua immagine.

![Risultati di ricerca per un termine di ricerca su Google Immagini](updated-google-images.png)

Nota che la maggior parte delle immagini sul web, inclusi Google Immagini, sono coperte da copyright. Per ridurre la possibilità di violare il diritto d'autore, puoi utilizzare il filtro di licenza di Google. Clicca sul pulsante _Strumenti_, poi sull'opzione _Diritti di utilizzo_ che appare sotto. Dovresti scegliere l'opzione _Licenze Creative Commons_.

![Risultati di ricerca filtrati per ottenere immagini con licenze Creative Commons su Google Immagini](updated-google-images-licensing.png)

## Scegliere un font

Esiste un insieme di font chiamati [font sicuri per il web](/it/docs/Learn_web_development/Core/Text_styling/Fundamentals#web_safe_fonts) — come Arial, Times New Roman o Courier New — che sono generalmente disponibili sulla maggior parte dei sistemi informatici. Se utilizzi uno di questi font sul tuo sito web, il browser caricherà il file del font disponibile sul computer dell'utente.

Tuttavia, se desideri utilizzare altri font non generalmente disponibili sui dispositivi, devi includerli insieme ai file del tuo sito web o fare riferimento ai file di font da un servizio di font di terze parti in modo che il browser possa scaricarli come necessario. [Google Fonts](https://fonts.google.com/) è uno di questi servizi che fornisce accesso a molti font.

Usiamo Google Fonts per scegliere un font per il tuo sito web:

1. Vai a [Google Fonts](https://fonts.google.com/).
2. Scorri l'elenco dei font fino a trovarne uno che ti piace. Se hai difficoltà a trovarne uno, puoi utilizzare i filtri disponibili nella colonna laterale per restringere la ricerca.
3. Clicca sull'opzione del font, quindi sulla pagina successiva clicca sul pulsante "Ottieni font".
4. Sulla pagina successiva, clicca su "Ottieni codice di incorporamento".
5. Copia entrambi i blocchi di codice forniti e salvali in un luogo sicuro per uso futuro.

> [!NOTE]
> Come per le immagini, molti font sono protetti da licenze, il che significa che non puoi necessariamente utilizzarli liberamente su siti web commerciali. Per ora stai bene mentre lavori su esempi di apprendimento, ma tienilo a mente quando scegli font per siti web reali.

{{NextMenu("Learn_web_development/Getting_started/Your_first_website/Creating_the_content", "Learn_web_development/Getting_started/Your_first_website")}}
