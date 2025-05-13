---
title: Sicurezza del sito web
slug: Learn_web_development/Extensions/Server-side/First_steps/Website_security
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenu("Learn_web_development/Extensions/Server-side/First_steps/Web_frameworks", "Learn_web_development/Extensions/Server-side/First_steps")}}

La sicurezza di un sito web richiede vigilanza in tutti gli aspetti della progettazione e dell'utilizzo del sito stesso. Questo articolo introduttivo non farà di lei un esperto di sicurezza web, ma la aiuterà a capire da dove provengono le minacce e cosa può fare per proteggere la sua applicazione web contro i più comuni attacchi.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Conoscenze informatiche di base.</td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere le più comuni minacce alla sicurezza delle applicazioni web e
        cosa può fare per ridurre il rischio che il suo sito venga violato.
      </td>
    </tr>
  </tbody>
</table>

## Che cos'è la sicurezza del sito web?

Internet è un posto pericoloso! Con grande regolarità, sentiamo parlare di siti resi indisponibili a causa di attacchi di denial of service, o che visualizzano informazioni modificate (e spesso dannose) sulle loro pagine iniziali. In altri casi di alto profilo, milioni di password, indirizzi email e dettagli delle carte di credito sono stati divulgati nel dominio pubblico, esponendo gli utenti dei siti sia all'imbarazzo personale che al rischio finanziario.

Lo scopo della sicurezza del sito web è prevenire questi (o qualsiasi altro) tipi di attacchi. La definizione più formale di sicurezza del sito web è _l'atto/la pratica di proteggere i siti web da accessi, usi, modifiche, distruzioni o interruzioni non autorizzati_.

Una sicurezza efficace del sito web richiede uno sforzo progettuale su tutto il sito: nella sua applicazione web, nella configurazione del server web, nelle sue politiche per creare e rinnovare le password e nel codice lato client. Sebbene tutto ciò possa sembrare molto minaccioso, la buona notizia è che se sta utilizzando un web framework lato server, esso probabilmente abiliterà "di default" meccanismi di difesa robusti e ben pensati contro un numero di attacchi tra i più comuni. Altri attacchi possono essere mitigati attraverso la configurazione del suo server web, per esempio abilitando HTTPS. Infine, esistono strumenti scanner di vulnerabilità disponibili pubblicamente che possono aiutarla a scoprire se ha commesso errori evidenti.

Il resto di questo articolo le fornirà ulteriori dettagli su alcune minacce comuni e alcuni semplici passaggi che può adottare per proteggere il suo sito.

> [!NOTE]
> Questo è un argomento introduttivo, progettato per aiutarla a iniziare a pensare alla sicurezza del sito web, ma non è esaustivo.

## Minacce alla sicurezza del sito web

Questa sezione elenca solo alcune delle minacce più comuni ai siti web e come vengono mitigate. Durante la lettura, noti come le minacce siano più efficaci quando l'applicazione web o si fida, o non è abbastanza paranioca sui dati provenienti dal browser.

### Cross-Site Scripting (XSS)

XSS è un termine usato per descrivere una classe di attacchi che permettono a un attaccante di iniettare script lato client _attraverso_ il sito nei browser di altri utenti. Poiché il codice iniettato arriva al browser dal sito, il codice è _considerato affidabile_ e può compiere azioni come inviare il cookie di autorizzazione del sito dell'utente all'attaccante. Quando l'attaccante ha il cookie, può effettuare il login in un sito come se fosse l'utente e fare tutto ciò che l'utente può fare, come accedere ai dettagli della loro carta di credito, vedere i dettagli di contatto, o cambiare password.

> [!NOTE]
> Storicamente, le vulnerabilità XSS sono state più comuni rispetto a qualsiasi altro tipo di minaccia alla sicurezza.

Le vulnerabilità XSS si dividono in _riflesse_ e _persistenti_, a seconda di come il sito restituisce al browser gli script iniettati.

- Una vulnerabilità XSS _riflessa_ si verifica quando il contenuto utente passato al server è restituito _immediatamente_ e _non modificato_ per la visualizzazione nel browser. Qualsiasi script nel contenuto utente originale verrà eseguito quando la nuova pagina viene caricata.
  Per esempio, consideri una funzione di ricerca nel sito dove i termini di ricerca sono codificati come parametri URL e questi termini vengono visualizzati insieme ai risultati. Un attaccante può costruire un link di ricerca che contiene uno script malevolo come parametro (ad esempio, `https://developer.mozilla.org?q=beer<script%20src="http://example.com/tricky.js"></script>`) e inviarlo tramite email a un altro utente. Se l'utente di destinazione clicca su questo "link interessante", lo script verrà eseguito quando i risultati della ricerca vengono visualizzati. Come discusso in precedenza, questo fornisce all'attaccante tutte le informazioni di cui ha bisogno per entrare nel sito come utente bersaglio, potenzialmente effettuando acquisti come l'utente o condividendo le loro informazioni di contatto.
- Una vulnerabilità XSS _persistente_ si verifica quando lo script malevolo è _memorizzato_ sul sito e poi ridistribuito non modificato per essere eseguito inavvertitamente da altri utenti.
  Per esempio, una bacheca di discussione che accetta commenti che contengono HTML non modificato potrebbe memorizzare uno script malevolo da parte di un attaccante. Quando i commenti vengono visualizzati, lo script viene eseguito e può inviare all'attaccante le informazioni necessarie per accedere all'account dell'utente. Questo tipo di attacco è estremamente popolare e potente, perché l'attaccante potrebbe non avere nemmeno un contatto diretto con le vittime.

Sebbene i dati provenienti dalle richieste `POST` o `GET` siano la fonte più comune di vulnerabilità XSS, qualsiasi dato dal browser è potenzialmente vulnerabile, come i dati dei cookie visualizzati dal browser, o i file utente caricati e visualizzati.

La migliore difesa contro le vulnerabilità XSS è rimuovere o disabilitare qualsiasi markup che possa potenzialmente contenere istruzioni per eseguire il codice. Per l'HTML questo include elementi come `<script>`, `<object>`, `<embed>`, e `<link>`.

Il processo di modifica dei dati dell'utente in modo che non possano essere utilizzati per eseguire script o altrimenti influenzare l'esecuzione del codice del server è noto come sanitizzazione degli input. Molti framework web automatizzano la sanitizzazione degli input dell'utente provenienti da forme HTML di default.

### SQL injection

Le vulnerabilità SQL injection permettono agli utenti malintenzionati di eseguire codice SQL arbitrario su un database, consentendo l'accesso, la modifica o l'eliminazione dei dati indipendentemente dalle autorizzazioni dell'utente. Un attacco injectivo riuscito potrebbe simulare identità, creare nuove identità con diritti di amministrazione, accedere a tutti i dati sul server o distruggere/modificare i dati rendendoli inutilizzabili.

I tipi di SQL injection includono SQL injection basata su errori, SQL injection basata su errori booleani, e SQL injection basata su tempo.

Questa vulnerabilità è presente se l'input dell'utente passato a un'istruzione SQL sottostante può cambiarne il significato. Per esempio, il seguente codice è destinato a elencare tutti gli utenti con un particolare nome (`userName`) fornito da un modulo HTML:

```python
statement = "SELECT * FROM users WHERE name = '" + userName + "';"
```

Se l'utente specifica un nome reale, l'istruzione funzionerà come previsto. Tuttavia, un utente malintenzionato potrebbe cambiare completamente il comportamento di questa istruzione SQL nella nuova istruzione dell'esempio seguente, specificando `a';DROP TABLE users; SELECT * FROM userinfo WHERE 't' = 't` per il `userName`.

```sql
SELECT * FROM users WHERE name = 'a';DROP TABLE users; SELECT * FROM userinfo WHERE 't' = 't';
```

L'istruzione modificata crea un'istruzione SQL valida che elimina la tabella `users` e seleziona tutti i dati dalla tabella `userinfo` (che rivela le informazioni di ogni utente). Questo funziona perché la prima parte del testo iniettato (`a';`) completa l'istruzione originale.

Per evitare tali attacchi, la migliore pratica è utilizzare query parametrizzate (istruzioni preparate). Questo approccio garantisce che l'input dell'utente sia trattato come una stringa di dati piuttosto che come SQL eseguibile, in modo che l'utente non possa abusare dei caratteri speciali della sintassi SQL per generare istruzioni SQL non intenzionali. Il seguente è un esempio:

```sql
SELECT * FROM users WHERE name = ? AND password = ?;
```

Durante l'esecuzione della query sopra, per esempio, in Python, passiamo il `name` e il `password` come parametri, come mostrato di seguito.

```python
cursor.execute("SELECT * FROM users WHERE name = ? AND password = ?", (name, password))
```

Le librerie spesso forniscono API ben astratte che gestiscono la protezione da SQL injection per lo sviluppatore, come i modelli di Django. Può evitare SQL injection utilizzando API incapsulate piuttosto che scrivere direttamente SQL grezzo.

### Cross-Site Request Forgery (CSRF)

Gli attacchi CSRF consentono a un utente malintenzionato di eseguire azioni utilizzando le credenziali di un altro utente senza la conoscenza o il consenso di quest'ultimo.

Questo tipo di attacco è meglio spiegato con un esempio. Josh è un utente malintenzionato che sa che un determinato sito consente agli utenti connessi di inviare denaro a un determinato account utilizzando una richiesta HTTP `POST` che include il nome dell'account e una somma di denaro. Josh crea un modulo che include i suoi dettagli bancari e una somma di denaro come campi nascosti, e lo invia tramite email ad altri utenti del sito (con il pulsante _Submit_ mascherato da link a un sito "arricchisciti subito").

Se un utente clicca sul pulsante submit, verrà inviata al server una richiesta HTTP `POST` contenente i dettagli della transazione e tutti i cookie lato client che il browser associa al sito (aggiungere i cookie associati ai siti alle richieste è un comportamento normale del browser). Il server controllerà i cookie e li userà per determinare se l'utente è connesso e ha il permesso di effettuare la transazione.

Il risultato è che qualsiasi utente che clicca sul pulsante _Submit_ mentre è connesso al sito di trading effettuerà la transazione. Josh si arricchisce.

> [!NOTE]
> Il trucco qui è che Josh non ha bisogno di accedere ai cookie dell'utente (o alle credenziali di accesso). Il browser dell'utente memorizza queste informazioni e le include automaticamente in tutte le richieste al server associato.

Un modo per prevenire questo tipo di attacco è che il server richieda che le richieste `POST` includano un segreto specifico dell'utente e generato dal sito. Il segreto verrebbe fornito dal server quando invia il modulo web utilizzato per effettuare i trasferimenti. Questo approccio impedisce a Josh di creare il proprio modulo, perché dovrebbe conoscere il segreto che il server sta fornendo per l'utente. Anche se scoprisse il segreto e creasse un modulo per un determinato utente, non sarebbe più in grado di utilizzare quel modulo per attaccare ogni utente.

I framework web spesso includono tali meccanismi di prevenzione CSRF.

### Altre minacce

Altri attacchi/vulnerabilità comuni includono:

- [Clickjacking](/it/docs/Web/Security/Attacks/Clickjacking). In questo attacco, un utente malintenzionato intercetta i clic destinati a un sito di primo livello visibile e li reindirizza a una pagina nascosta sottostante. Questa tecnica potrebbe essere utilizzata, per esempio, per visualizzare un sito bancario legittimo, ma catturare le credenziali di accesso in un {{htmlelement("iframe")}} invisibile controllato dall'attaccante. Il clickjacking potrebbe essere utilizzato anche per far cliccare all'utente su un pulsante su un sito visibile, ma facendo così inavvertitamente cliccare un pulsante completamente diverso. Come difesa, il suo sito può impedire di essere incorporato in un iframe in un altro sito impostando gli appropriati header HTTP.
- {{Glossary("Distributed_Denial_of_Service", "Denial of Service")}} (DoS). Il DoS è solitamente realizzato inondando un sito bersaglio con richieste false in modo che l'accesso a un sito sia interrotto per gli utenti legittimi. Le richieste possono essere numerose o possono consumare individualmente grandi quantità di risorse (es., lente letture o caricamenti di file grandi). Le difese DoS di solito lavorano identificando e bloccando il traffico "malevolo" mentre consentono il passaggio dei messaggi legittimi. Queste difese sono tipicamente situate prima o nel server web (non fanno parte dell'applicazione web stessa).
- [Directory Traversal](https://en.wikipedia.org/wiki/Directory_traversal_attack) (File e divulgazione). In questo attacco, un utente malintenzionato tenta di accedere a parti del file system del server web a cui non dovrebbe poter accedere. Questa vulnerabilità si verifica quando l'utente è in grado di passare nomi di file che includono caratteri di navigazione del file system (per esempio, `../../`). La soluzione è sanitizzare gli input prima di utilizzarli.
- [File Inclusion](https://en.wikipedia.org/wiki/File_inclusion_vulnerability). In questo attacco, l'utente è in grado di specificare un file "non intenzionale" per la visualizzazione o l'esecuzione nei dati passati al server. Quando viene caricato, questo file potrebbe essere eseguito sul server web o lato client (portando a un attacco XSS). La soluzione è sanitizzare gli input prima di utilizzarli.
- [Command Injection](https://owasp.org/www-community/attacks/Command_Injection). Gli attacchi di command injection consentono a un utente malintenzionato di eseguire comandi di sistema arbitrari sul sistema operativo host. La soluzione è sanitizzare gli input dell'utente prima che possano essere utilizzati in chiamate di sistema.

Per una lista completa delle minacce alla sicurezza del sito web, vedere [Category: Web security exploits](https://en.wikipedia.org/wiki/Category:Web_security_exploits) (Wikipedia) e [Category: Attack](https://owasp.org/www-community/attacks/) (Open Web Application Security Project).

## Alcuni messaggi chiave

Quasi tutti gli exploit di sicurezza delle sezioni precedenti sono efficaci quando l'applicazione web si fida dei dati del browser. Qualunque altra cosa faccia per migliorare la sicurezza del suo sito web, dovrebbe sanitizzare tutti i dati provenienti dall'utente prima di visualizzarli nel browser, utilizzarli nelle query SQL o passarli a una chiamata al sistema operativo o al file system.

> [!WARNING]
> La lezione più importante che può imparare sulla sicurezza del sito web è **non fidarsi mai dei dati dal browser**. Questo include, ma non si limita a dati nei parametri URL delle richieste `GET`, richieste `POST`, header HTTP e cookie, e file caricati dall'utente. Controlli e sanitizzi sempre tutti i dati in arrivo. Supponga sempre il peggio.

Alcuni altri passi concreti che può intraprendere sono:

- Utilizzare una gestione delle password più efficace. Incoraggi password forti. Consideri l'autenticazione a due fattori per il suo sito, in modo che oltre a una password l'utente debba inserire un altro codice di autenticazione (solitamente uno che viene consegnato tramite qualche hardware fisico che solo l'utente possiederà, come un codice in un SMS inviato al loro telefono).
- Configuri il suo server web per utilizzare {{Glossary("HTTPS", "HTTPS")}} e [HTTP Strict Transport Security](/it/docs/Web/HTTP/Reference/Headers/Strict-Transport-Security) (HSTS). HTTPS cripta i dati inviati tra il suo client e il server. Questo garantisce che le credenziali di accesso, i cookie, i dati delle richieste `POST` e le informazioni degli header non siano facilmente accessibili agli attaccanti.
- Tenga traccia delle minacce più popolari (la [lista corrente OWASP è qui](https://owasp.org/www-project-top-ten/)) e affronti prima le vulnerabilità più comuni.
- Usi [strumenti di scansione delle vulnerabilità](https://owasp.org/www-community/Vulnerability_Scanning_Tools) per eseguire test di sicurezza automatizzati sul suo sito. Più avanti, il suo sito di grande successo potrebbe anche trovare bug offrendo una ricompensa per la segnalazione di bug [come fa Mozilla qui](https://www.mozilla.org/en-US/security/bug-bounty/faq-webapp/).
- Conservi e visualizzi solo i dati di cui ha bisogno. Per esempio, se i suoi utenti devono memorizzare informazioni sensibili come i dettagli delle carte di credito, visualizzi solo quanto basta del numero della carta perché possa essere identificato dall'utente, e non abbastanza perché possa essere copiato da un attaccante e utilizzato su un altro sito. Il modello più comune al momento è visualizzare solo le ultime 4 cifre di un numero di carta di credito.
- Mantenga il software aggiornato.
  La maggior parte dei server dispone di aggiornamenti di sicurezza regolari che risolvono o mitigano le vulnerabilità note.
  Se possibile, programmi aggiornamenti regolari automatizzati e idealmente pianifichi aggiornamenti durante i periodi in cui il suo sito web ha il minor traffico.
  È meglio eseguire il backup dei dati prima di aggiornare e testare le nuove versioni del software per assicurarsi che non ci siano problemi di compatibilità sul suo server.

I framework web possono aiutare a mitigare molte delle vulnerabilità più comuni.

## Riassunto

Questo articolo ha spiegato il concetto di sicurezza web e alcune delle minacce più comuni contro le quali il suo sito web dovrebbe cercare di proteggersi. La cosa più importante, dovrebbe comprendere che un'applicazione web non può fidarsi di alcun dato dal browser web. Tutti i dati degli utenti dovrebbero essere sanitizzati prima di essere visualizzati o utilizzati in query SQL e chiamate al file system.

Con questo articolo, ha concluso [questo modulo](/it/docs/Learn_web_development/Extensions/Server-side/First_steps), che copre i suoi primi passi nella programmazione lato server del sito web. Speriamo che abbia apprezzato l'apprendimento di questi concetti fondamentali e che ora sia pronto per selezionare un Web Framework e iniziare a programmare.

{{PreviousMenu("Learn_web_development/Extensions/Server-side/First_steps/Web_frameworks", "Learn_web_development/Extensions/Server-side/First_steps")}}
