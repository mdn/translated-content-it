---
title: Che cos'è un nome di dominio?
slug: Learn_web_development/Howto/Web_mechanics/What_is_a_domain_name
l10n:
  sourceCommit: 3e543cdfe8dddfb4774a64bf3decdcbab42a4111
---

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Innanzitutto è necessario sapere
        <a href="/it/docs/Learn_web_development/Howto/Web_mechanics/How_does_the_Internet_work"
          >come funziona Internet</a
        >
        e comprendere
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

## Riepilogo

I nomi di dominio sono una parte fondamentale dell'infrastruttura di Internet. Forniscono un indirizzo leggibile dall'uomo per qualsiasi server web disponibile su Internet.

Qualsiasi computer connesso a Internet può essere raggiunto tramite un {{Glossary("IP_Address", "indirizzo IP")}} pubblico, ovvero un indirizzo IPv4 (ad esempio, `192.0.2.172`) o un indirizzo IPv6 (ad esempio, `2001:db8:8b73:0000:0000:8a2e:0370:1337`).

I computer possono gestire facilmente tali indirizzi, ma per le persone è difficile scoprire chi gestisce il server o quale servizio offre il sito web. Gli indirizzi IP sono difficili da ricordare e potrebbero cambiare nel tempo.

Per risolvere tutti questi problemi si usano indirizzi leggibili dall'uomo chiamati nomi di dominio.

## Approfondimento

### Struttura dei nomi di dominio

Un nome di dominio ha una struttura semplice composta da diverse parti (può essere costituito da una sola parte, due, tre…), separate da punti e **lette da destra a sinistra**:

![Anatomia del nome di dominio di MDN](structure.png)

Ciascuna di queste parti fornisce informazioni specifiche sull'intero nome di dominio.

- {{Glossary("TLD", "TLD")}} (Top-Level Domain).
  - : I TLD indicano agli utenti lo scopo generale del servizio dietro il nome di dominio. I TLD più generici (`.com`, `.org`, `.net`) non richiedono ai servizi web di soddisfare criteri particolari, ma alcuni TLD applicano politiche più rigorose, così che il loro scopo sia più chiaro. Ad esempio:
    - I TLD locali come `.us`, `.fr` o `.se` possono richiedere che il servizio sia fornito in una determinata lingua o ospitato in un certo paese: dovrebbero indicare una risorsa in una lingua o in un paese specifico.
    - I TLD che contengono `.gov` possono essere utilizzati soltanto da dipartimenti governativi.
    - Il TLD `.edu` è riservato esclusivamente a istituzioni educative e accademiche.

    I TLD possono contenere caratteri speciali oltre a caratteri latini. La lunghezza massima di un TLD è di 63 caratteri, anche se la maggior parte è composta da circa 2–3 caratteri.

    L'elenco completo dei TLD è [gestito da ICANN](https://www.icann.org/en/contracted-parties/registry-operators/resources/list-of-top-level-domains).

- Etichetta (o componente)
  - : Le etichette sono le parti che seguono il TLD. Un'etichetta è una sequenza di caratteri senza distinzione tra maiuscole e minuscole, lunga da uno a sessantatré caratteri, contenente soltanto le lettere da `A` a `Z`, le cifre da `0` a `9` e il carattere '-' (che non può essere il primo né l'ultimo carattere dell'etichetta). `a`, `97` e `hello-strange-person-16-how-are-you` sono tutti esempi di etichette valide.

    L'etichetta situata immediatamente prima del TLD è chiamata anche _Secondary Level Domain_ (SLD).

    Un nome di dominio può avere molte etichette (o componenti). Non è obbligatorio né necessario avere 3 etichette per formare un nome di dominio. Ad esempio, [informatics.ed.ac.uk](https://informatics.ed.ac.uk/) è un nome di dominio valido. Per qualsiasi dominio controllato (ad esempio, [mozilla.org](https://www.mozilla.org/en-US/)), è possibile creare "sottodomini" con contenuti diversi in ciascuno, come [developer.mozilla.org](/), [support.mozilla.org](https://support.mozilla.org/) o [bugzilla.mozilla.org](https://bugzilla.mozilla.org/).

### Acquistare un nome di dominio

#### Chi possiede un nome di dominio?

Non è possibile "acquistare un nome di dominio". Questo permette ai nomi di dominio inutilizzati di tornare eventualmente disponibili per essere usati da qualcun altro. Se ogni nome di dominio venisse acquistato, il web si riempirebbe rapidamente di nomi di dominio inutilizzati, bloccati e non utilizzabili da nessuno.

Si paga invece per il diritto di usare un nome di dominio per uno o più anni. È possibile rinnovare il proprio diritto e il rinnovo ha priorità sulle richieste di altre persone. Tuttavia, il nome di dominio non viene mai posseduto.

Le aziende chiamate registrar utilizzano i registri dei nomi di dominio per tenere traccia delle informazioni tecniche e amministrative che collegano il titolare al suo nome di dominio.

> [!NOTE]
> Per alcuni nomi di dominio, potrebbe non essere un registrar a occuparsi del monitoraggio. Ad esempio, ogni nome di dominio sotto `.fire` è gestito da Amazon.

#### Trovare un nome di dominio disponibile

Per scoprire se un determinato nome di dominio è disponibile:

- Visitare il sito web di un registrar di nomi di dominio. La maggior parte di essi fornisce un servizio "whois" che indica se un nome di dominio è disponibile.
- In alternativa, se si utilizza un sistema con una shell integrata, digitare al suo interno un comando `whois`, come mostrato qui per `mozilla.org`:

  ```bash
  whois mozilla.org
  ```

  Verrà prodotto il seguente output:

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

Come si può vedere, non è possibile registrare `mozilla.org` perché la Mozilla Foundation lo ha già registrato.

D'altra parte, vediamo se fosse possibile registrare `afunkydomainname.org`:

```bash
whois afunkydomainname.org
```

Verrà prodotto il seguente output (al momento della scrittura):

```plain
NOT FOUND
```

Come si può vedere, il dominio non esiste nel database `whois`, quindi sarebbe possibile richiederne la registrazione. Bene a sapersi!

#### Ottenere un nome di dominio

Il processo è piuttosto semplice:

1. Visitare il sito web di un registrar.
2. Solitamente è presente un evidente invito all'azione "Ottieni un nome di dominio". Fare clic su di esso.
3. Compilare il modulo con tutti i dettagli richiesti. Assicurarsi, in particolare, di non aver scritto erroneamente il nome di dominio desiderato. Una volta pagato, è troppo tardi!
4. Il registrar comunicherà quando il nome di dominio sarà registrato correttamente. Entro poche ore, tutti i server DNS avranno ricevuto le informazioni DNS.

> [!NOTE]
> Durante questo processo il registrar richiede l'indirizzo reale. Assicurarsi di compilarlo correttamente, poiché in alcuni paesi i registrar potrebbero essere obbligati a chiudere il dominio se non possono fornire un indirizzo valido.

#### Aggiornamento del DNS

I database DNS sono memorizzati su ogni server DNS nel mondo e tutti questi server fanno riferimento ad alcuni server speciali chiamati "authoritative name servers" o "top-level DNS servers": sono come i server principali che gestiscono il sistema.

Ogni volta che il registrar crea o aggiorna informazioni per un determinato dominio, tali informazioni devono essere aggiornate in ogni database DNS. Ogni server DNS che conosce un determinato dominio memorizza le informazioni per un certo periodo di tempo prima che vengano automaticamente invalidate e poi aggiornate (il server DNS interroga un server autorevole e recupera da esso le informazioni aggiornate). Pertanto, occorre del tempo affinché i server DNS che conoscono questo nome di dominio ottengano informazioni aggiornate.

### Come funziona una richiesta DNS?

Come già visto, quando si desidera visualizzare una pagina web nel browser è più semplice digitare un nome di dominio anziché un indirizzo IP. Vediamo il processo:

1. Digitare `mozilla.org` nella barra degli indirizzi del browser.
2. Il browser chiede al computer se riconosce già l'indirizzo IP identificato da questo nome di dominio (utilizzando una cache DNS locale). Se lo riconosce, il nome viene tradotto nell'indirizzo IP e il browser negozia i contenuti con il server web. Fine.
3. Se il computer non sa quale IP si trova dietro il nome `mozilla.org`, procede chiedendolo a un server DNS, il cui compito è precisamente indicare al computer quale indirizzo IP corrisponde a ciascun nome di dominio registrato.
4. Ora che il computer conosce l'indirizzo IP richiesto, il browser può negoziare i contenuti con il server web.

![Spiegazione dei passaggi necessari per ottenere il risultato di una richiesta DNS](2014-10-dns-request2.png)

## Passaggi successivi

Bene, si è parlato molto di processi e architettura. È il momento di proseguire.

- Per mettere in pratica quanto appreso, è un buon momento per iniziare ad approfondire il design ed esplorare [l'anatomia di una pagina web](/it/docs/Learn_web_development/Howto/Design_and_accessibility/Common_web_layouts).
- Vale anche la pena notare che alcuni aspetti della creazione di un sito web hanno un costo. Consultare [quanto costa creare un sito web](/it/docs/Learn_web_development/Howto/Tools_and_setup/How_much_does_it_cost).
- Oppure leggere ulteriori informazioni sui [nomi di dominio](https://en.wikipedia.org/wiki/Domain_name) su Wikipedia.
- Il tutorial [Come funziona il DNS](https://howdns.works/) offre una spiegazione divertente e colorata.
