---
title: Cos'è un Nome di Dominio?
slug: Learn_web_development/Howto/Web_mechanics/What_is_a_domain_name
l10n:
  sourceCommit: e488eba036b2fee56444fd579c3759ef45ff2ca8
---

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Prima è necessario sapere
        <a href="/it/docs/Learn_web_development/Howto/Web_mechanics/How_does_the_Internet_work"
          >come funziona Internet</a
        >
        e capire
        <a href="/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_URL"
          >cosa sono gli URL</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare cosa sono i nomi di dominio, come funzionano e perché sono importanti.
      </td>
    </tr>
  </tbody>
</table>

## Sommario

I nomi di dominio sono una parte fondamentale dell'infrastruttura di Internet. Forniscono un indirizzo leggibile per qualsiasi server web disponibile su Internet.

Qualsiasi computer connesso a Internet può essere raggiunto tramite un {{Glossary("IP_Address", "Indirizzo IP")}} pubblico, sia un indirizzo IPv4 (ad esempio, `192.0.2.172`) sia un indirizzo IPv6 (ad esempio, `2001:db8:8b73:0000:0000:8a2e:0370:1337`).

I computer possono gestire facilmente tali indirizzi, ma le persone hanno difficoltà a scoprire chi gestisce il server o quale servizio offre il sito web. Gli indirizzi IP sono difficili da ricordare e potrebbero cambiare nel tempo.

Per risolvere tutti questi problemi, utilizziamo indirizzi leggibili dall'uomo chiamati nomi di dominio.

## Approfondimento

### Struttura dei nomi di dominio

Un nome di dominio ha una struttura semplice composta da diverse parti (può essere solo una parte, due, tre…), separate da punti e **lette da destra a sinistra**:

![Anatomia del nome di dominio di MDN](structure.png)

Ciascuna di queste parti fornisce informazioni specifiche sul nome di dominio completo.

- {{Glossary("TLD", "TLD")}} (Top-Level Domain).

  - : I TLD indicano agli utenti lo scopo generale del servizio dietro il nome di dominio. I TLD più generici (`.com`, `.org`, `.net`) non richiedono ai servizi web di soddisfare alcun criterio particolare, ma alcuni TLD applicano politiche più rigorose per chiarire quale sia il loro scopo. Ad esempio:

    - I TLD locali come `.us`, `.fr` o `.se` possono richiedere che il servizio venga fornito in una determinata lingua o ospitato in un determinato paese — dovrebbero indicare una risorsa in una lingua o paese specifico.
    - I TLD contenenti `.gov` sono consentiti solo per l'uso da parte di dipartimenti governativi.
    - Il TLD `.edu` è destinato solo alle istituzioni educative e accademiche.

    I TLD possono contenere caratteri speciali oltre a quelli latini. La lunghezza massima di un TLD è di 63 caratteri, anche se la maggior parte ha una lunghezza di 2–3 caratteri.

    La lista completa dei TLD è [gestita da ICANN](https://www.icann.org/en/contracted-parties/registry-operators/resources/list-of-top-level-domains).

- Etichetta (o componente)

  - : Le etichette seguono il TLD. Un'etichetta è una sequenza di caratteri insensibile alla maiuscola/lowercase ovunque da uno a sessantatré caratteri di lunghezza, contenente solo le lettere da `A` a `Z`, cifre da `0` a `9`, e il carattere `-` (che non può essere il primo o l'ultimo carattere dell'etichetta). `a`, `97` e `hello-strange-person-16-how-are-you` sono tutti esempi di etichette valide.

    L'etichetta situata appena prima del TLD è anche chiamata _Second Level Domain_ (SLD).

    Un nome di dominio può avere molte etichette (o componenti). Non è obbligatorio né necessario avere 3 etichette per formare un nome di dominio. Ad esempio, [informatics.ed.ac.uk](https://informatics.ed.ac.uk/) è un nome di dominio valido. Per qualsiasi dominio controllato (ad es., [mozilla.org](https://www.mozilla.org/en-US/)), si possono creare "sottodomini" con contenuti diversi localizzati in ciascuno, come [developer.mozilla.org](/), [support.mozilla.org](https://support.mozilla.org/), o [bugzilla.mozilla.org](https://bugzilla.mozilla.org/).

### Acquistare un nome di dominio

#### Chi possiede un nome di dominio?

Non si può "comprare un nome di dominio". Questo per fare in modo che i nomi di dominio non utilizzati diventino disponibili per essere usati nuovamente da qualcun altro. Se ogni nome di dominio fosse acquistato, il web si riempirebbe rapidamente di nomi di dominio non utilizzati che sarebbero bloccati e non potrebbero essere usati da nessuno.

Invece, si paga per il diritto di utilizzare un nome di dominio per uno o più anni. Si può rinnovare il diritto, e il rinnovo avrà la priorità sulle applicazioni di altre persone. Ma non si possiede mai il nome di dominio.

Le società chiamate registrar utilizzano registri di nomi di dominio per tenere traccia delle informazioni tecniche e amministrative che collegano l'utente al proprio nome di dominio.

> [!NOTE]
> Per alcuni nomi di dominio, potrebbe non essere un registrar ad essere incaricato di tenere traccia. Ad esempio, ogni nome di dominio sotto `.fire` è gestito da Amazon.

#### Trovare un nome di dominio disponibile

Per scoprire se un determinato nome di dominio è disponibile,

- Vai al sito web di un registrar di nomi di dominio. La maggior parte di loro offre un servizio "whois" che ti dice se un nome di dominio è disponibile.
- In alternativa, se si utilizza un sistema con una shell integrata, digitare un comando `whois` al suo interno, come mostrato qui per `mozilla.org`:

  ```bash
  whois mozilla.org
  ```

  Questo produrrà il seguente output:

  ```plain
  Domain Name:MOZILLA.ORG
  Domain ID: D1409563-LROR
  Creation Date: 1998-01-24T05:00:00Z
  Updated Date: 2013-12-08T01:16:57Z
  Registry Expiry Date: 2015-01-23T05:00:00Z
  Sponsoring Registrar:MarkMonitor Inc. (R37-LROR)
  Sponsoring Registrar IANA ID: 292
  WHOIS Server:
  Referral URL:
  Domain Status: clientDeleteProhibited
  Domain Status: clientTransferProhibited
  Domain Status: clientUpdateProhibited
  Registrant ID:mmr-33684
  Registrant Name:DNS Admin
  Registrant Organization:Mozilla Foundation
  Registrant Street: 650 Castro St Ste 300
  Registrant City:Mountain View
  Registrant State/Province:CA
  Registrant Postal Code:94041
  Registrant Country:US
  Registrant Phone:+1.6509030800
  ```

Come puoi vedere, non posso registrare `mozilla.org` perché è già stato registrato dalla Mozilla Foundation.

D'altra parte, vediamo se potrei registrare `afunkydomainname.org`:

```bash
whois afunkydomainname.org
```

Questo produrrà il seguente output (al momento della scrittura):

```plain
NOT FOUND
```

Come puoi vedere, il dominio non esiste nel database `whois`, quindi potremmo chiedere di registrarlo. Buono a sapersi!

#### Ottenere un nome di dominio

Il processo è piuttosto diretto:

1. Vai al sito web di un registrar.
2. Di solito c'è un'azione chiamata "Get a domain name" che spicca. Fai clic su di essa.
3. Compila il modulo con tutti i dettagli richiesti. Assicurati, soprattutto, di non aver scritto erroneamente il nome di dominio desiderato. Una volta pagato, è troppo tardi!
4. Il registrar ti farà sapere quando il nome di dominio è registrato correttamente. Entro poche ore, tutti i server DNS avranno ricevuto le tue informazioni DNS.

> [!NOTE]
> In questo processo il registrar ti chiede il tuo indirizzo reale. Assicurati di compilarlo correttamente, poiché in alcuni paesi i registrar potrebbero essere costretti a chiudere il dominio se non possono fornire un indirizzo valido.

#### Aggiornamento del DNS

I database DNS sono memorizzati su ogni server DNS a livello mondiale, e tutti questi server fanno riferimento a pochi server speciali chiamati "authoritative name servers" o "server DNS di primo livello" — sono come i server principali che gestiscono il sistema.

Ogni volta che il tuo registrar crea o aggiorna qualsiasi informazione per un determinato dominio, le informazioni devono essere aggiornate in ogni database DNS. Ogni server DNS che conosce un dato dominio memorizza le informazioni per un certo tempo prima che vengano automaticamente invalidate e poi aggiornate (il server DNS interroga un server autorevole e recupera le informazioni aggiornate da esso). Pertanto, ci vuole un po' di tempo perché i server DNS che conoscono questo nome di dominio ricevano le informazioni aggiornate.

### Come funziona una richiesta DNS?

Come abbiamo già visto, quando si desidera visualizzare una pagina web nel browser è più semplice digitare un nome di dominio piuttosto che un indirizzo IP. Diamo un'occhiata al processo:

1. Digita `mozilla.org` nella barra degli indirizzi del tuo browser.
2. Il tuo browser chiede al tuo computer se riconosce già l'indirizzo IP identificato da questo nome di dominio (usando una cache DNS locale). Se lo fa, il nome è tradotto in un indirizzo IP e il browser negozia i contenuti con il server web. Fine della storia.
3. Se il tuo computer non sa quale IP si nasconde dietro il nome `mozilla.org`, procede a chiedere a un server DNS, il cui compito è precisamente di dire al tuo computer quale indirizzo IP corrisponde a ciascun nome di dominio registrato.
4. Ora che il computer conosce l'indirizzo IP richiesto, il tuo browser può negoziare i contenuti con il server web.

![Spiegazione dei passaggi necessari per ottenere il risultato di una richiesta DNS](2014-10-dns-request2.png)

## Passaggi successivi

Bene, abbiamo parlato molto di processi e architettura. È tempo di andare avanti.

- Se vuoi mettere in pratica, è un buon momento per iniziare a scavare nel design e esplorare [l'anatomia di una pagina web](/it/docs/Learn_web_development/Howto/Design_and_accessibility/Common_web_layouts).
- Vale anche la pena notare che alcuni aspetti della costruzione di un sito web costano denaro. Ti invitiamo a consultare [quanto costa costruire un sito web](/it/docs/Learn_web_development/Howto/Tools_and_setup/How_much_does_it_cost).
- Oppure leggi di più sui [Nomi di Dominio](https://en.wikipedia.org/wiki/Domain_name) su Wikipedia.
- Il tutorial [How DNS works](https://howdns.works/) offre una spiegazione divertente e colorata.
