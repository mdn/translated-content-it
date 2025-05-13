---
title: Come funziona Internet?
slug: Learn_web_development/Howto/Web_mechanics/How_does_the_Internet_work
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

Questo articolo discute cosa sia Internet e come funzioni.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Nessuno, ma Le incoraggiamo a leggere per prima cosa l'
        <a href="/it/docs/Learn_web_development/Howto/Design_and_accessibility/Thinking_before_coding"
          >Articolo sul fissare gli obiettivi del progetto</a
        >
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparerà le basi dell'infrastruttura tecnica del Web e la differenza tra Internet e il Web.
      </td>
    </tr>
  </tbody>
</table>

## Riepilogo

**Internet** è la spina dorsale del Web, l'infrastruttura tecnica che rende possibile il Web. Alla sua base, Internet è una grande rete di computer che comunicano tutti insieme.

[La storia di Internet è alquanto oscura](https://en.wikipedia.org/wiki/Internet#History). Iniziò negli anni '60 come un progetto di ricerca finanziato dall'esercito statunitense, poi si è evoluto in un'infrastruttura pubblica negli anni '80 con il supporto di molte università pubbliche e aziende private. Le varie tecnologie che supportano Internet si sono evolute nel tempo, ma il modo in cui funziona non è cambiato molto: Internet è un modo per collegare i computer tra loro e garantire che, qualunque cosa accada, trovino un modo per rimanere connessi.

## Apprendimento attivo

- [Come funziona Internet in 5 minuti](https://www.youtube.com/watch?v=7_LPdttKXPc): Un video di 5 minuti per capire le basi di Internet di Aaron Titus.
- [Come funziona Internet?](https://www.youtube.com/watch?v=x3c1ih2NJEg) Video dettagliato e ben visualizzato di 9 minuti.

## Approfondimento

### Una semplice rete

Quando due computer devono comunicare, è necessario collegarli, sia fisicamente (di solito con un [cavo Ethernet](https://en.wikipedia.org/wiki/Ethernet_crossover_cable)) sia in modalità wireless (ad esempio con sistemi [Wi-Fi](https://en.wikipedia.org/wiki/Wi-Fi) o [Bluetooth](https://en.wikipedia.org/wiki/Bluetooth)). Tutti i computer moderni possono sostenere qualsiasi di queste connessioni.

> [!NOTE]
> Per il resto di questo articolo, parleremo solo di cavi fisici, ma le reti wireless funzionano allo stesso modo.

![Due computer collegati tra loro](internet-schema-1.png)

Tale rete non è limitata a due computer. Può collegare quanti computer desidera. Ma diventa rapidamente complicato. Se cerca di collegare, ad esempio, dieci computer, ha bisogno di 45 cavi, con nove prese per computer!

![Dieci computer tutti insieme](internet-schema-2.png)

Per risolvere questo problema, ogni computer su una rete è collegato a un piccolo computer speciale chiamato _switch di rete_ (o brevemente _switch_). Questo switch ha un solo compito: come un segnalatore in una stazione ferroviaria, assicura che i messaggi inviati da un determinato computer arrivino solo al computer di destinazione previsto. Per inviare un messaggio al computer B, il computer A invia il messaggio allo switch, che a sua volta lo inoltra al computer B — il computer B non riceve messaggi destinati ad altri computer e nessuno dei messaggi per il computer B raggiunge altri computer sulla rete locale.

Una volta aggiunto uno switch al sistema, la nostra rete di 10 computer richiede solo 10 cavi: una presa per ciascun computer e uno switch con 10 prese.

![Dieci computer con uno switch](internet-schema-3.png)

### Una rete di reti

Fin qui tutto bene. Ma che dire di collegare centinaia, migliaia, miliardi di computer? Ovviamente un solo switch non può scalare così tanto, ma, se ha letto attentamente, abbiamo detto che uno switch è un computer come qualsiasi altro, quindi cosa ci impedisce di collegare due switch insieme? Niente, quindi facciamolo.

![Due switch collegati tra loro](internet-schema-4.png)

Può immaginare che possiamo collegare switch insieme all'infinito, per formare una rete come questa:

![Switch collegati a switch](internet-schema-5.png)

In realtà, questo porta a molti problemi di ingegneria. Più switch un pacchetto deve attraversare, più tempo ci vuole per raggiungere la sua destinazione. E non può avere solo un albero di switch, perché allora un singolo guasto dello switch potrebbe disconnettere una vasta porzione di dispositivi. Per risolvere questo problema, manteniamo ciascuna rete locale il più piccola possibile e colleghiamo queste reti locali utilizzando un dispositivo separato chiamato _router_. Un router è un computer che sa come inoltrare messaggi tra reti. Il router è come un ufficio postale: quando arriva un pacchetto, legge l'indirizzo del destinatario e inoltra il pacchetto al destinatario giusto direttamente, senza passare attraverso livelli di relè.

Una tale rete si avvicina molto a ciò che chiamiamo Internet. Necessitiamo solo del mezzo fisico (cavi) per collegare tutti questi router. Fortunatamente, tale infrastruttura già esisteva prima di Internet, ed è la rete telefonica. Per collegare la nostra rete all'infrastruttura telefonica, abbiamo bisogno di un dispositivo speciale chiamato _modem_. Questo _modem_ trasforma le informazioni dalla nostra rete in informazioni gestibili dall'infrastruttura telefonica e viceversa.

![Un router collegato a un modem](internet-schema-6.png)

Noti che il router commerciale nella sua casa è probabilmente una combinazione di uno switch, un router e un modem, tutto in un unico dispositivo.

Quindi siamo connessi all'infrastruttura telefonica. Il passo successivo è inviare i messaggi dalla nostra rete alla rete che desideriamo raggiungere. Per farlo, collegheremo la nostra rete a un Fornitore di Servizi Internet (ISP). Un ISP è un'azienda che gestisce alcuni _router_ speciali che sono tutti collegati tra loro e possono anche accedere ad altri router di ISP. Quindi il messaggio dalla nostra rete viene trasportato attraverso la rete di reti ISP alla rete di destinazione. Internet consiste in tutta questa infrastruttura di reti.

![Struttura completa di Internet](internet-schema-7.png)

### Trovare computer

Se vuole inviare un messaggio a un computer, deve specificare quale. Pertanto, qualsiasi computer collegato a una rete ha un indirizzo univoco che lo identifica, chiamato "indirizzo IP" (dove IP sta per _Internet Protocol_). È un indirizzo composto da una serie di quattro numeri separati da punti, ad esempio: `192.0.2.172`.

Questo va benissimo per i computer, ma noi esseri umani abbiamo difficoltà a ricordare questo tipo di indirizzo. Per facilitare le cose, possiamo sostituire un indirizzo IP con un nome leggibile dall'uomo chiamato _nome di dominio_. Ad esempio (al momento della scrittura; gli indirizzi IP possono cambiare) `google.com` è il nome di dominio utilizzato sopra l'indirizzo IP `142.250.190.78`. Quindi utilizzare il nome di dominio è il modo più semplice per noi di raggiungere un computer tramite Internet.

![Mostra come un nome di dominio può sostituire un indirizzo IP](dns-ip.png)

### Internet e il web

Come potrà notare, quando navighiamo sul Web con un browser Web, di solito utilizziamo il nome di dominio per raggiungere un sito web. Significa questo che Internet e il Web sono la stessa cosa? Non è così semplice. Come abbiamo visto, Internet è un'infrastruttura tecnica che permette a miliardi di computer di essere connessi tra loro. Tra questi computer, alcuni (chiamati _server web_) possono inviare messaggi comprensibili dai browser web. _Internet_ è un'infrastruttura, mentre il _Web_ è un servizio costruito sopra l'infrastruttura. Vale la pena notare che ci sono diversi altri servizi costruiti sopra Internet, come la posta elettronica e {{Glossary("IRC", "IRC")}}.

### Intranet ed Extranet

Le intranet sono reti _private_ che sono limitate ai membri di una particolare organizzazione.
Vengono comunemente utilizzate per fornire un portale ai membri per accedere in modo sicuro a risorse condivise, collaborare e comunicare.
Ad esempio, l'intranet di un'organizzazione potrebbe ospitare pagine web per la condivisione di informazioni del dipartimento o del team, unità condivise per la gestione di documenti e file chiave,
portali per eseguire compiti di amministrazione aziendale e strumenti di collaborazione come wiki, bacheche di discussione e sistemi di messaggistica.

Le extranet sono molto simili alle intranet, ad eccezione del fatto che aprono tutto o parte di una rete privata per consentire la condivisione e la collaborazione con altre organizzazioni.
Sono tipicamente utilizzate per condividere in modo sicuro e protetto informazioni con clienti e stakeholder che lavorano a stretto contatto con un'azienda.
Spesso le loro funzioni sono simili a quelle fornite da un'intranet: condivisione di informazioni e file, strumenti di collaborazione, bacheche di discussione, ecc.

Sia le intranet che le extranet funzionano sulla stessa infrastruttura di Internet e utilizzano gli stessi protocolli.
Pertanto, possono essere accessibili dai membri autorizzati da diverse località fisiche.

![Rappresentazione grafica di come funzionano Extranet e Intranet](internet-schema-8.png)

## Prossimi passi

- [Come funziona il Web](/it/docs/Learn_web_development/Getting_started/Web_standards/How_the_web_works)
- [Comprendere la differenza tra una pagina web, un sito web, un server web e un motore di ricerca](/it/docs/Learn_web_development/Getting_started/Environment_setup/Browsing_the_web)
- [Comprendere i nomi di dominio](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_domain_name)
