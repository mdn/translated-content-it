---
title: Sicurezza delle applicazioni web Django
short-title: Sicurezza di Django
slug: Learn_web_development/Extensions/Server-side/Django/web_application_security
l10n:
  sourceCommit: 6030ef1aadf967b80e2c79c3d3463cccc8ea0c95
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Deployment", "Learn_web_development/Extensions/Server-side/Django/django_assessment_blog", "Learn_web_development/Extensions/Server-side/Django")}}

Proteggere i dati degli utenti è una parte essenziale della progettazione di qualsiasi sito web. In precedenza sono state illustrate alcune delle minacce di sicurezza più comuni nell'articolo [Sicurezza sul web](/it/docs/Web/Security): questo articolo fornisce una dimostrazione pratica di come le protezioni integrate di Django gestiscano tali minacce.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Leggere l'argomento "<a href="/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Website_security">Sicurezza dei siti web</a>" della programmazione lato server.
        Completare gli argomenti del tutorial su Django almeno fino a <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/Forms">Tutorial Django - Parte 9: utilizzo dei moduli</a>, incluso.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere le principali operazioni da eseguire, o da non eseguire, per proteggere un'applicazione web Django.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

L'argomento [Sicurezza dei siti web](/it/docs/Web/Security) fornisce una panoramica di cosa significhi la sicurezza dei siti web per la progettazione lato server e di alcune delle minacce più comuni dalle quali proteggersi. Uno dei messaggi principali di tale articolo è che quasi tutti gli attacchi hanno successo quando l'applicazione web considera attendibili i dati provenienti dal browser.

> [!WARNING]
> La lezione più importante da apprendere sulla sicurezza dei siti web è **non fidarsi mai dei dati provenienti dal browser**. Ciò include i dati delle richieste `GET` nei parametri URL, i dati `POST`, gli header HTTP e i cookie, i file caricati dagli utenti e così via. Controllare e sanificare sempre tutti i dati in ingresso. Presumere sempre lo scenario peggiore.

La buona notizia per gli utenti di Django è che il framework gestisce molte delle minacce più comuni. L'articolo [Security in Django](https://docs.djangoproject.com/en/5.0/topics/security/) (documentazione di Django) illustra le funzionalità di sicurezza di Django e come proteggere un sito web basato su Django.

## Minacce/protezioni comuni

Anziché duplicare qui la documentazione di Django, questo articolo dimostrerà solo alcune delle funzionalità di sicurezza nel contesto del tutorial Django [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website).

### Cross-site scripting (XSS)

XSS è un termine usato per descrivere una classe di attacchi che consente a un aggressore di iniettare script lato client _attraverso_ il sito web nei browser di altri utenti. Questo avviene solitamente memorizzando script dannosi nel database, dove possono essere recuperati e visualizzati da altri utenti, oppure inducendo gli utenti a fare clic su un collegamento che farà eseguire il JavaScript dell'aggressore dal browser dell'utente.

Il sistema di template di Django protegge dalla maggior parte degli attacchi XSS tramite l'[escape di caratteri specifici](https://docs.djangoproject.com/en/5.0/ref/templates/language/#automatic-html-escaping) che sono "pericolosi" in HTML. È possibile dimostrarlo tentando di iniettare del JavaScript nel sito web LocalLibrary tramite il modulo di creazione degli autori configurato in [Tutorial Django - Parte 9: utilizzo dei moduli](/it/docs/Learn_web_development/Extensions/Server-side/Django/Forms).

1. Avviare il sito web usando il server di sviluppo (`python3 manage.py runserver`).
2. Aprire il sito nel browser locale ed effettuare l'accesso con l'account superuser.
3. Passare alla pagina di creazione dell'autore, che dovrebbe trovarsi all'URL: `http://127.0.0.1:8000/catalog/author/create/`.
4. Inserire nomi e dettagli della data per un nuovo utente, quindi aggiungere il testo seguente al campo del cognome:
   `<script>alert('Test alert');</script>`.
   ![Test XSS del modulo autore](author_create_form_alert_xss.png)

   > [!NOTE]
   > Si tratta di uno script innocuo che, se eseguito, visualizzerà una finestra di avviso nel browser. Se l'avviso viene visualizzato quando il record viene inviato, il sito è vulnerabile alle minacce XSS.

5. Premere **Submit** per salvare il record.
6. Quando l'autore viene salvato, sarà visualizzato come mostrato di seguito. Grazie alle protezioni XSS, `alert()` non dovrebbe essere eseguito. Lo script viene invece visualizzato come testo semplice.
   ![Test XSS della vista dettagli dell'autore](author_detail_alert_xss.png)

Se si visualizza il codice sorgente HTML della pagina, è possibile osservare che i caratteri pericolosi dei tag script sono stati trasformati nei rispettivi equivalenti di escape innocui, ad esempio `>` ora è `&gt;`.

```html
<h1>
  Author: Boon&lt;script&gt;alert(&#39;Test alert&#39;);&lt;/script&gt;, David
  (Boonie)
</h1>
```

L'uso dei template Django protegge dalla maggior parte degli attacchi XSS. Tuttavia, è possibile disattivare questa protezione e la protezione non viene applicata automaticamente a tutti i tag che normalmente non verrebbero popolati dall'input utente. Per esempio, `help_text` in un campo del modulo solitamente non è fornito dall'utente, pertanto Django non applica l'escape a tali valori.

Gli attacchi XSS possono inoltre provenire da altre fonti di dati non attendibili, come cookie, servizi Web o file caricati, ogni volta che i dati non vengono sanificati in modo sufficiente prima di essere inclusi in una pagina. Se vengono visualizzati dati provenienti da tali fonti, potrebbe essere necessario aggiungere codice di sanificazione personalizzato.

### Protezione cross-site request forgery (CSRF)

Gli attacchi CSRF consentono a un utente malintenzionato di eseguire azioni usando le credenziali di un altro utente senza che quest'ultimo ne sia a conoscenza o abbia dato il consenso. Per esempio, si consideri il caso di un hacker che desidera creare ulteriori autori per LocalLibrary.

> [!NOTE]
> Evidentemente questo hacker non lo fa per denaro. Un hacker più ambizioso potrebbe usare lo stesso approccio su altri siti per svolgere attività molto più dannose, come trasferire denaro sui propri conti e così via.

Per farlo, potrebbe creare un file HTML come quello seguente, contenente un modulo di creazione dell'autore, simile a quello usato nella sezione precedente, che viene inviato non appena il file viene caricato.
Il file verrebbe quindi inviato a tutti i bibliotecari, suggerendo loro di aprirlo: contiene informazioni innocue, davvero. Se il file viene aperto da un bibliotecario che ha effettuato l'accesso, il modulo verrà inviato con le sue credenziali e verrà creato un nuovo autore.

```html
<html lang="en">
  <body onload="document.EvilForm.submit()">
    <form
      action="http://127.0.0.1:8000/catalog/author/create/"
      method="post"
      name="EvilForm">
      <label for="id_first_name">First name:</label>
      <input
        id="id_first_name"
        maxlength="100"
        name="first_name"
        type="text"
        value="Mad"
        required />
      <label for="id_last_name">Last name:</label>
      <input
        id="id_last_name"
        maxlength="100"
        name="last_name"
        type="text"
        value="Man"
        required />
      <label for="id_date_of_birth">Date of birth:</label>
      <input id="id_date_of_birth" name="date_of_birth" type="text" />
      <label for="id_date_of_death">Died:</label>
      <input
        id="id_date_of_death"
        name="date_of_death"
        type="text"
        value="12/10/2016" />
      <input type="submit" value="Submit" />
    </form>
  </body>
</html>
```

Avviare il server web di sviluppo ed effettuare l'accesso con l'account superuser. Copiare il testo precedente in un file e aprirlo nel browser. Dovrebbe essere visualizzato un errore CSRF, poiché Django dispone di una protezione contro questo tipo di attacco.

La protezione viene abilitata includendo il tag template `{% csrf_token %}` nella definizione del modulo. Questo token viene quindi renderizzato nell'HTML come mostrato di seguito, con un valore specifico per l'utente nel browser corrente.

```html
<input
  type="hidden"
  name="csrfmiddlewaretoken"
  value="0QRWHnYVg776y2l66mcvZqp8alrv4lb8S8lZ4ZJUWGZFA5VHrVfL2mpH29YZ39PW" />
```

Django genera una chiave specifica per utente/browser e rifiuterà i moduli che non contengono il campo oppure che contengono un valore di campo non corretto per l'utente/browser.

Per usare questo tipo di attacco, l'hacker dovrebbe ora individuare e includere la chiave CSRF per lo specifico utente di destinazione. Non può nemmeno usare l'approccio indiscriminato di inviare un file dannoso a tutti i bibliotecari sperando che uno di essi lo apra, poiché la chiave CSRF è specifica del browser.

La protezione CSRF di Django è attivata per impostazione predefinita. Usare sempre il tag template `{% csrf_token %}` nei moduli e usare `POST` per le richieste che potrebbero modificare o aggiungere dati al database.

### Altre protezioni

Django fornisce anche altre forme di protezione, molte delle quali sarebbero difficili o poco utili da dimostrare:

- Protezione dall'iniezione SQL
  - : Le vulnerabilità di iniezione SQL consentono a utenti malintenzionati di eseguire codice SQL arbitrario su un database, permettendo di accedere ai dati, modificarli o eliminarli indipendentemente dalle autorizzazioni dell'utente. Nella quasi totalità dei casi si accederà al database usando i queryset/model di Django, pertanto il SQL risultante verrà sottoposto correttamente a escape dal driver del database sottostante. Se è necessario scrivere query raw o SQL personalizzato, occorre considerare esplicitamente come prevenire l'iniezione SQL.
- Protezione dal clickjacking
  - : In questo attacco, un utente malintenzionato intercetta i clic destinati a un sito visibile di livello superiore e li reindirizza a una pagina nascosta sottostante. Questa tecnica potrebbe essere usata, per esempio, per visualizzare un sito bancario legittimo ma acquisire le credenziali di accesso in un [`<iframe>`](/it/docs/Web/HTML/Reference/Elements/iframe) invisibile controllato dall'aggressore. Django include la protezione dal [clickjacking](/it/docs/Web/Security/Attacks/Clickjacking) sotto forma del middleware [`X-Frame-Options`](https://docs.djangoproject.com/en/4.0/ref/middleware/#django.middleware.clickjacking.XFrameOptionsMiddleware), che, in un browser supportato, può impedire che un sito venga renderizzato all'interno di un frame.
- Applicazione di TLS/HTTPS
  - : TLS/HTTPS può essere abilitato sul server web per crittografare tutto il traffico tra il sito e il browser, incluse le credenziali di autenticazione che altrimenti verrebbero inviate in testo semplice. L'abilitazione di HTTPS è fortemente consigliata. Se HTTPS è abilitato, Django fornisce diverse altre protezioni utilizzabili:
    - [`SECURE_PROXY_SSL_HEADER`](https://docs.djangoproject.com/en/5.0/ref/settings/#std:setting-SECURE_PROXY_SSL_HEADER) può essere usato per verificare se il contenuto è sicuro, anche se proviene da un proxy non HTTP.
    - [`SECURE_SSL_REDIRECT`](https://docs.djangoproject.com/en/5.0/ref/settings/#std:setting-SECURE_SSL_REDIRECT) viene usato per reindirizzare tutte le richieste HTTP a HTTPS.
    - Usare [HTTP Strict Transport Security](https://docs.djangoproject.com/en/5.0/ref/middleware/#http-strict-transport-security) (HSTS). Si tratta di un header HTTP che informa un browser che tutte le connessioni future a un determinato sito devono usare sempre HTTPS. In combinazione con il reindirizzamento delle richieste HTTP a HTTPS, questa impostazione garantisce che HTTPS venga sempre usato dopo una connessione riuscita. HSTS può essere configurato con [`SECURE_HSTS_SECONDS`](https://docs.djangoproject.com/en/5.0/ref/settings/#std:setting-SECURE_HSTS_SECONDS) e [`SECURE_HSTS_INCLUDE_SUBDOMAINS`](https://docs.djangoproject.com/en/5.0/ref/settings/#std:setting-SECURE_HSTS_INCLUDE_SUBDOMAINS), oppure sul server Web.
    - Usare cookie "secure" impostando [`SESSION_COOKIE_SECURE`](https://docs.djangoproject.com/en/5.0/ref/settings/#std:setting-SESSION_COOKIE_SECURE) e [`CSRF_COOKIE_SECURE`](https://docs.djangoproject.com/en/5.0/ref/settings/#std:setting-CSRF_COOKIE_SECURE) su `True`. Ciò garantisce che i cookie vengano inviati solo tramite HTTPS.
- Convalida dell'header host
  - : Usare [`ALLOWED_HOSTS`](https://docs.djangoproject.com/en/5.0/ref/settings/#std:setting-ALLOWED_HOSTS) per accettare richieste solo da host attendibili.

Esistono molte altre protezioni e avvertenze riguardo all'uso dei meccanismi sopra descritti. Sebbene questo articolo abbia fornito una panoramica di ciò che offre Django, è comunque necessario leggere la documentazione sulla sicurezza di Django.

## Riepilogo

Django dispone di protezioni efficaci contro diverse minacce comuni, inclusi gli attacchi XSS e CSRF. In questo articolo è stato dimostrato come tali minacce specifiche siano gestite da Django nel sito web _LocalLibrary_. È stata inoltre fornita una breve panoramica di alcune altre protezioni.

Questa è stata un'introduzione molto breve alla sicurezza web. Si raccomanda vivamente di leggere [Security in Django](https://docs.djangoproject.com/en/5.0/topics/security/) per acquisire una comprensione più approfondita.

Il passaggio successivo e finale di questo modulo su Django consiste nel completare il [compito di valutazione](/it/docs/Learn_web_development/Extensions/Server-side/Django/django_assessment_blog).

## Vedi anche

- [Sicurezza sul web](/it/docs/Web/Security)
- [Guide pratiche all'implementazione della sicurezza](/it/docs/Web/Security/Practical_implementation_guides)
- [Security in Django](https://docs.djangoproject.com/en/5.0/topics/security/) (documentazione di Django)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Deployment", "Learn_web_development/Extensions/Server-side/Django/django_assessment_blog", "Learn_web_development/Extensions/Server-side/Django")}}
