---
title: Che cosa sono gli hyperlink?
slug: Learn_web_development/Howto/Web_mechanics/What_are_hyperlinks
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

In questo articolo, esamineremo cosa sono gli hyperlink e perché sono importanti.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Dovrebbe sapere
        <a href="/it/docs/Learn_web_development/Howto/Web_mechanics/How_does_the_Internet_work"
          >come funziona Internet</a
        >
        ed essere familiare con la <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Browsing_the_web"
        >
          differenza tra una pagina web, un sito web, un server web e un
          motore di ricerca</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Imparare a conoscere i link sul web e perché sono importanti.</td>
    </tr>
  </tbody>
</table>

## Sommario

Gli hyperlink, solitamente chiamati link, sono un concetto fondamentale alla base del Web. Per spiegare cosa sono i link, dobbiamo tornare ai fondamenti dell'architettura Web.

Nel 1989, Tim Berners-Lee, l'inventore del Web, ha parlato dei tre pilastri su cui si regge il Web:

1. {{Glossary("URL", "URL")}}, un sistema di indirizzi che tiene traccia dei documenti Web
2. {{Glossary("HTTP", "HTTP")}}, un protocollo di trasferimento per trovare documenti dato il loro URL
3. {{Glossary("HTML", "HTML")}}, un formato di documenti che consente l'incorporazione di _hyperlink_

Come si può vedere nei tre pilastri, tutto sul Web ruota attorno ai documenti e a come accedervi. Lo scopo originale del Web era fornire un modo semplice per raggiungere, leggere e navigare tra documenti di testo. Da allora, il Web si è evoluto per fornire accesso a immagini, video e dati binari, ma questi miglioramenti hanno cambiato ben poco i tre pilastri.

Prima del Web, era piuttosto difficile accedere ai documenti e passare da uno all'altro. Essendo leggibili dall'uomo, gli URL già rendevano le cose più semplici, ma è difficile digitare un lungo URL ogni volta che si desidera accedere a un documento. È qui che gli hyperlink hanno rivoluzionato tutto. I link possono correlare qualsiasi stringa di testo con un URL, in modo tale che l'utente possa raggiungere istantaneamente il documento di destinazione attivando il link.

I link si distinguono dal testo circostante per essere sottolineati e in testo blu. Tocchi o clicchi su un link per attivarlo, oppure, se utilizza una tastiera, prema Tab finché il link non è a fuoco e prema Invio o la Barra spaziatrice.

![Esempio di una semplice visualizzazione ed effetto di un link in una pagina web](link-1.png)

I link sono l'innovazione che ha reso il Web così utile e di successo. Nel resto di questo articolo, discuteremo i vari tipi di link e la loro importanza per il design moderno del Web.

## Approfondimento

Come abbiamo detto, un link è una stringa di testo associata a un URL e utilizziamo i link per consentire passaggi facili da un documento all'altro. Detto ciò, ci sono alcune sfumature che meritano considerazione:

### Tipi di link

- Link interno
  - : Un link tra due pagine web, dove entrambe le pagine appartengono allo stesso sito web, è chiamato link interno. Senza i link interni, non esiste un sito web (a meno che, naturalmente, non sia un sito ad una sola pagina).
- Link esterno
  - : Un link dalla sua pagina web alla pagina web di qualcun altro. Senza link esterni, non c'è il Web, poiché il Web è una rete di pagine web. Utilizzi i link esterni per fornire informazioni oltre al contenuto disponibile attraverso la sua pagina web.
- Link in entrata
  - : Un link dalla pagina web di qualcun altro al suo sito. È l'opposto di un link esterno. Tenga presente che non è necessario restituire il link quando qualcuno collega al suo sito.

Quando costruisce un sito web, si concentri sui link interni, poiché questi rendono il suo sito utilizzabile. Trovi un buon equilibrio tra avere troppi link e troppo pochi. Parleremo della progettazione della navigazione del sito web in un altro articolo, ma come regola, ogni volta che aggiunge una nuova pagina web, si assicuri che almeno una delle sue altre pagine si colleghi a quella nuova pagina. D'altra parte, se il suo sito ha più di circa dieci pagine, è controproducente collegarsi a tutte le pagine da ogni altra pagina.

Quando inizia, non deve preoccuparsi tanto dei link esterni e in entrata, ma diventano molto importanti se vuole che i motori di ricerca trovino il suo sito (vedi sotto per maggiori dettagli).

### Ancore

La maggior parte dei link collega due pagine web insieme. **Ancore** legano due sezioni di un documento insieme. Quando segue un link che punta a un'ancora, il suo browser salta a un'altra parte del documento corrente anziché caricare un nuovo documento. Tuttavia, si creano e utilizzano le ancore allo stesso modo di altri link.

![Esempio di una semplice visualizzazione ed effetto di un'ancora in una pagina web](link-2.png)

### Link e motori di ricerca

I link sono importanti sia per gli utenti che per i motori di ricerca. Ogni volta che i motori di ricerca eseguono la scansione di una pagina web, indicizzano il sito seguendo i link disponibili sulla pagina. I motori di ricerca non solo seguono i link per scoprire le varie pagine del sito, ma utilizzano anche il testo visibile del link per determinare quali ricerche sono appropriate per raggiungere la pagina di destinazione.

I link influenzano la prontezza con cui un motore di ricerca collegherà al suo sito. Il problema è che è difficile misurare le attività dei motori di ricerca. Le aziende naturalmente vogliono che i loro siti siano classificati in alto nei risultati di ricerca. Sappiamo che i motori di ricerca determinano il rango di un sito in base ai seguenti fattori:

- Il _testo visibile_ di un link influenza quali ricerche troveranno un dato URL.
- Più _link in entrata_ può vantare una pagina web, più in alto si classifica nei risultati di ricerca.
- I _link esterni_ influenzano il posizionamento nei motori di ricerca sia delle pagine sorgente che delle pagine di destinazione, ma non è chiaro di quanto.

[SEO](https://en.wikipedia.org/wiki/Search_engine_optimization) (ottimizzazione per i motori di ricerca) è lo studio su come far classificare in alto i siti web nei risultati di ricerca. Migliorare l'uso dei link su un sito web è una tecnica di SEO utile.

## Passi successivi

Ora vorrà configurare alcune pagine web con link.

- Per ottenere un po' più di background teorico, si informi su [URL e la loro struttura](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_URL), poiché ogni link punta a un URL.
- Desidera qualcosa di più pratico? La nostra [Guida alla creazione di link](/it/docs/Learn_web_development/Core/Structuring_content/Creating_links) spiega in dettaglio come implementare i link.
