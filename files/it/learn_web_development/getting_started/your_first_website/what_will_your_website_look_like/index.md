---
title: Che aspetto avrà il sito web?
short-title: Che aspetto avrà?
slug: Learn_web_development/Getting_started/Your_first_website/What_will_your_website_look_like
l10n:
  sourceCommit: 85fccefc8066bd49af4ddafc12c77f35265c7e2d
---

{{NextMenu("Learn_web_development/Getting_started/Your_first_website/Creating_the_content", "Learn_web_development/Getting_started/Your_first_website")}}

_Che aspetto avrà il sito web?_ tratta il lavoro di pianificazione e progettazione da svolgere per il sito web prima di scrivere codice, incluse domande come: "Quali informazioni offre il sito web?", "Quali font e colori usare?" e "Che cosa fa il sito?"

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con il sistema operativo del computer, con il software di base da usare per creare un sito web e con i file system.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Pianificare un sito web di base.</li>
          <li>Usare un processo di progettazione di base.</li>
          <li>Raccogliere risorse.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Prima di tutto: pianificare

Prima di fare qualsiasi cosa, servono alcune idee. Che cosa dovrebbe fare effettivamente il sito web? Un sito web può fare praticamente qualsiasi cosa, ma, per il primo tentativo, è meglio mantenere le cose semplici. Si inizierà creando una semplice pagina web con un titolo, un'immagine e alcuni paragrafi.

Per iniziare, è necessario rispondere a queste domande:

1. **Di che cosa parla il sito web?** Piacciono i cani, New York o Pac-Man?
2. **Quali informazioni vengono presentate sull'argomento?** Scrivere un titolo e alcuni paragrafi, quindi pensare a un'immagine da mostrare nella pagina.
3. **Che aspetto ha il sito web**, in termini semplici e generali? Qual è il colore di sfondo? Che tipo di font è appropriato: formale, fumettistico, marcato e vistoso, sobrio?

> [!NOTE]
> I progetti complessi richiedono linee guida dettagliate che coprano ogni aspetto relativo a colori, font, spaziatura tra gli elementi di una pagina, stile di scrittura appropriato e così via. A volte queste linee guida sono chiamate guida di progettazione, design system o brand book; è possibile vederne un esempio nel [Firefox Acorn Design System](https://acorn.firefox.com/latest).

## Abbozzare il progetto

Successivamente, prendere carta e penna e abbozzare approssimativamente come dovrebbe apparire il sito. Per la prima semplice pagina web non c'è molto da abbozzare, ma è bene prendere fin da ora l'abitudine di farlo. Aiuta davvero: non è necessario essere Van Gogh!

![Un disegno e uno schizzo approssimativo di un sito web su carta](website-drawing-scan.png)

> [!NOTE]
> Anche nei siti web reali e complessi, i team di progettazione di solito iniziano con schizzi approssimativi su carta e, in seguito, creano mockup digitali usando un editor grafico o tecnologie web.
>
> I team web includono spesso sia un [graphic designer](/it/docs/Learn_web_development/Getting_started/Soft_skills/Workflows_and_processes#graphic_designer) sia un [user experience (UX) designer](/it/docs/Learn_web_development/Getting_started/Soft_skills/Workflows_and_processes#user_experience_ux_designer). I graphic designer realizzano gli elementi visivi del sito web. Gli UX designer hanno un ruolo più astratto, relativo al modo in cui gli utenti vivranno e interagiranno con il sito web.

A questo punto, è utile iniziare a raccogliere i contenuti che appariranno infine nella pagina web. I paragrafi e il titolo preparati in precedenza dovrebbero essere ancora disponibili. Tenerli a portata di mano.

## Scegliere un colore per il tema

Scegliamo un colore di sfondo per la pagina.

1. Andare al [Color Picker](/it/docs/Web/CSS/Guides/Colors/Color_format_converter) e trovare un colore di proprio gradimento.
2. Quando si sceglie un colore, verrà visualizzato uno strano codice di sei caratteri come `#660066`. Questo è chiamato _codice esadecimale_ (abbreviazione di hexadecimal) e rappresenta il colore. Copiare il codice in un posto sicuro per il momento.

![Strumento di conversione del formato dei colori sul sito web MDN Docs](color_format_converter.jpg)

## Scegliere un'immagine

Ora è il momento di trovare un'immagine da mostrare sul sito.

1. Andare a [Google Images](https://www.google.com/imghp).
2. Tenere presente che la maggior parte delle immagini sul web, incluse quelle in Google Images, è protetta da copyright. Per ridurre la probabilità di violare il copyright, è possibile usare il filtro delle licenze di Google. Fare clic sul pulsante _Tools_, quindi sull'opzione _Usage rights_ visualizzata sotto. Scegliere l'opzione _Creative Commons licenses_.

   ![Risultati di ricerca filtrati per ottenere immagini con licenze Creative Commons su Google Images](updated-google-images-licensing.png)

3. Cercare un'immagine adatta.
4. Quando si trova l'immagine desiderata, fare clic sull'immagine per visualizzarla ingrandita.
5. Fare clic con il pulsante destro sull'immagine (<kbd>Ctrl</kbd> + clic su Mac), scegliere _Save Image As…_ e selezionare un posto sicuro in cui salvare l'immagine.

   ![Risultati di ricerca per un termine di ricerca su Google Images](updated-google-images.png)

## Scegliere un font

Esiste un insieme di font chiamati [web safe fonts](/it/docs/Learn_web_development/Core/Text_styling/Fundamentals#web_safe_fonts), come Arial, Times New Roman o Courier New, generalmente disponibili sulla maggior parte dei sistemi informatici. Se viene usato uno di questi font sul sito web, il browser caricherà il file del font disponibile sul computer dell'utente.

Tuttavia, se si desidera usare altri font non generalmente disponibili sui dispositivi, è necessario includerli insieme ai file del sito web oppure fare riferimento ai file del font da un servizio di font di terze parti, in modo che il browser possa scaricarli quando necessario. [Google Fonts](https://fonts.google.com/) è uno di questi servizi, che fornisce accesso a molti font.

Usiamo Google Fonts per scegliere un font per il sito web:

1. Andare a [Google Fonts](https://fonts.google.com/).
2. Scorrere l'elenco dei font fino a trovarne uno di proprio gradimento. Se è difficile trovarne uno, è possibile usare i filtri disponibili nell'altra colonna per restringere la ricerca.
3. Fare clic sull'opzione del font, quindi, nella pagina successiva, fare clic sul pulsante "Get font".
4. Nella pagina successiva, fare clic su "Get embed code".
5. Copiare entrambi i blocchi di codice forniti e salvarli in un posto sicuro per usarli in seguito.

> [!NOTE]
> Come per le immagini, molti font sono protetti da licenze, il che significa che non possono necessariamente essere usati liberamente sui siti web commerciali. Per ora non ci saranno problemi lavorando sugli esempi di apprendimento, ma è importante tenerlo presente quando si scelgono font per siti web reali.

{{NextMenu("Learn_web_development/Getting_started/Your_first_website/Creating_the_content", "Learn_web_development/Getting_started/Your_first_website")}}
