---
title: "Django Tutorial Parte 8: Autenticazione e permessi utente"
short-title: "8: Autenticazione e permessi"
slug: Learn_web_development/Extensions/Server-side/Django/Authentication
l10n:
  sourceCommit: be1922d62a0d31e4e3441db0e943aed8df736481
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Sessions", "Learn_web_development/Extensions/Server-side/Django/Forms", "Learn_web_development/Extensions/Server-side/Django")}}

In questo tutorial, vi mostreremo come consentire agli utenti di accedere al vostro sito con i propri account e come controllare cosa possono fare e vedere in base al fatto che siano connessi o meno e ai loro _permessi_. Come parte di questa dimostrazione, estenderemo il sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website), aggiungendo pagine di login e logout, e pagine specifiche per utenti e staff per visualizzare i libri che sono stati presi in prestito.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Completare tutti i temi precedenti del tutorial, fino e incluso <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/Sessions">Django Tutorial Parte 7: il framework delle sessioni</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Capire come configurare e utilizzare l'autenticazione e i permessi utente.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

Django fornisce un sistema di autenticazione e autorizzazione ("permesso"), costruito sopra il framework delle sessioni discusso nel [tutorial precedente](/it/docs/Learn_web_development/Extensions/Server-side/Django/Sessions), che permette di verificare le credenziali degli utenti e definire quali azioni ogni utente è autorizzato a eseguire. Il framework include modelli integrati per `Users` e `Groups` (un modo generico di applicare permessi a più di un utente alla volta), permessi/flag che designano se un utente può svolgere un compito, moduli e viste per il login degli utenti e strumenti di vista per limitare i contenuti.

> [!NOTE]
> Secondo Django, il sistema di autenticazione mira a essere molto generico e quindi non fornisce alcune funzionalità offerte da altri sistemi di autenticazione web. Soluzioni per alcuni problemi comuni sono disponibili come pacchetti di terze parti. Ad esempio, {{Glossary("throttle", "limitazione")}} dei tentativi di accesso e autenticazione contro terze parti (ad es., OAuth).

In questo tutorial, vi mostreremo come abilitare l'autenticazione utente nel sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website), creare le vostre pagine di login e logout, aggiungere permessi ai vostri modelli e controllare l'accesso alle pagine. Useremo l'autenticazione/permessi per visualizzare elenchi di libri che sono stati presi in prestito sia per gli utenti sia per i bibliotecari.

Il sistema di autenticazione è molto flessibile e potete creare i vostri URL, form, viste e template da zero se lo desiderate, utilizzando semplicemente l'API fornita per loggare l'utente. Tuttavia, in questo articolo, utilizzeremo le viste e i form di autenticazione "stock" di Django per le nostre pagine di login e logout. Dovremo comunque creare alcuni template, ma è abbastanza semplice.

Vi mostreremo anche come creare permessi e verificare lo stato di accesso e i permessi sia nelle viste sia nei template.

## Abilitare l'autenticazione

L'autenticazione è stata abilitata automaticamente quando abbiamo [creato il sito web scheletro](/it/docs/Learn_web_development/Extensions/Server-side/Django/skeleton_website) (nel tutorial 2) quindi non è necessario fare altro a questo punto.

> [!NOTE]
> La configurazione necessaria è stata tutta fatta per noi quando abbiamo creato l'app usando il comando `django-admin startproject`. Le tabelle del database per gli utenti e i permessi del modello sono state create quando abbiamo chiamato per la prima volta `python manage.py migrate`.

La configurazione è impostata nelle sezioni `INSTALLED_APPS` e `MIDDLEWARE` del file del progetto (**django-locallibrary-tutorial/locallibrary/settings.py**), come mostrato di seguito:

```python
INSTALLED_APPS = [
    # …
    'django.contrib.auth',  # Core authentication framework and its default models.
    'django.contrib.contenttypes',  # Django content type system (allows permissions to be associated with models).
    # …

MIDDLEWARE = [
    # …
    'django.contrib.sessions.middleware.SessionMiddleware',  # Manages sessions across requests
    # …
    'django.contrib.auth.middleware.AuthenticationMiddleware',  # Associates users with requests using sessions.
    # …
```

## Creare utenti e gruppi

Avete già creato il vostro primo utente quando abbiamo esaminato il [sito admin di Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/Admin_site) nel tutorial 4 (questo era un superuser, creato con il comando `python manage.py createsuperuser`).
Il nostro superuser è già autenticato e ha tutti i permessi, quindi dovremo creare un utente di prova per rappresentare un normale utente del sito. Useremo il sito admin per creare i gruppi _locallibrary_ e i login al sito web, poiché è uno dei modi più rapidi per farlo.

> [!NOTE]
> Potete anche creare utenti programmaticamente come mostrato di seguito.
> Dovreste farlo, ad esempio, se state sviluppando un'interfaccia per permettere agli utenti "ordinari" di creare i propri login (non dovreste dare accesso alla maggior parte degli utenti al sito admin).
>
> ```python
> from django.contrib.auth.models import User
>
> # Creare un utente e salvarlo nel database
> user = User.objects.create_user('myusername', 'myemail@crazymail.com', 'mypassword')
>
> # Aggiornare i campi e poi salvare di nuovo
> user.first_name = 'Tyrone'
> user.last_name = 'Citizen'
> user.save()
> ```
>
> Si noti tuttavia che è fortemente raccomandato configurare un _modello utente personalizzato_ quando si avvia un progetto, poiché sarà possibile personalizzarlo facilmente in futuro se necessario.
> Se si utilizza un modello utente personalizzato, il codice per creare lo stesso utente sarebbe il seguente:
>
> ```python
> # Ottenere il modello utente corrente dalle impostazioni
> from django.contrib.auth import get_user_model
> User = get_user_model()
>
> # Creare utente dal modello e salvare nel database
> user = User.objects.create_user('myusername', 'myemail@crazymail.com', 'mypassword')
>
> # Aggiornare i campi e poi salvare di nuovo
> user.first_name = 'Tyrone'
> user.last_name = 'Citizen'
> user.save()
> ```
>
> Per ulteriori informazioni, vedere [Utilizzare un modello utente personalizzato quando si avvia un progetto](https://docs.djangoproject.com/en/5.0/topics/auth/customizing/#using-a-custom-user-model-when-starting-a-project) (documentazione di Django).

Di seguito, creeremo prima un gruppo e poi un utente. Anche se non abbiamo ancora alcun permesso da aggiungere per i membri della nostra biblioteca, se ci sarà bisogno dopo, sarà molto più facile aggiungerli una volta al gruppo che individualmente a ogni membro.

Avviare il server di sviluppo e navigare verso il sito admin nel proprio browser web locale (`http://127.0.0.1:8000/admin/`). Accedere al sito usando le credenziali del proprio account superuser. Il livello superiore del sito Admin visualizza tutti i vostri modelli, ordinati per "applicazione Django". Dalla sezione **Authentication and Authorization**, potete cliccare i link **Users** o **Groups** per vedere i loro record esistenti.

![Sito admin - aggiungi gruppi o utenti](admin_authentication_add.png)

Prima creiamo un nuovo gruppo per i nostri membri della biblioteca.

1. Cliccare il pulsante **Add** (accanto a Group) per creare un nuovo _Group_; inserire il **Name** "Library Members" per il gruppo.
   ![Sito admin - aggiungi gruppo](admin_authentication_add_group.png)
2. Non abbiamo bisogno di permessi per il gruppo, quindi basta premere **SAVE** (sarete portati a un elenco di gruppi).

Ora creiamo un utente:

1. Tornare alla home page del sito admin.
2. Cliccare il pulsante **Add** accanto a _Users_ per aprire la finestra di dialogo _Add user_.
   ![Sito admin - aggiungi utente pt1](admin_authentication_add_user_prt1.png)
3. Inserire un **Username** appropriato e **Password**/**Password confirmation** per il proprio utente di test.
4. Premere **SAVE** per creare l'utente.

   Il sito admin creerà il nuovo utente e vi porterà immediatamente a uno schermo _Change user_ dove potete cambiare il vostro **username** e aggiungere informazioni per i campi opzionali del modello User. Questi campi includono il nome, il cognome, l'indirizzo email e lo stato e i permessi dell'utente (solo il flag **Active** dovrebbe essere impostato). Più in basso potete specificare i gruppi e i permessi dell'utente, e vedere date importanti relative all'utente (ad esempio, la loro data di adesione e ultima data di accesso).
   ![Sito admin - aggiungi utente pt2](admin_authentication_add_user_prt2.png)

5. Nella sezione _Groups_, selezionare il gruppo **Library Member** dall'elenco dei _Available groups_ e poi premere la **freccia destra** tra le caselle per spostarlo nella casella _Chosen groups_.
   ![Sito admin - aggiungi utente al gruppo](admin_authentication_user_add_group.png)
6. Non c'è bisogno di fare altro qui, quindi selezionare di nuovo **SAVE** per andare all'elenco degli utenti.

È tutto! Ora avete un account di "normale membro della biblioteca" che potrete utilizzare per i test (una volta che avremo implementato le pagine per consentire loro di effettuare il login).

> [!NOTE]
> Dovreste provare a creare un altro utente membro della biblioteca. Inoltre, creare un gruppo per i bibliotecari e aggiungere un utente anche a quello!

## Impostazione delle viste di autenticazione

Django fornisce quasi tutto ciò di cui avete bisogno per creare pagine di autenticazione per gestire il login, il logout e la gestione della password "pronto all'uso". Questo include un URL mapper, viste e form, ma non include i modelli — dobbiamo creare i nostri!

In questa sezione, vi mostriamo come integrare il sistema predefinito nel sito web _LocalLibrary_ e creare i modelli. Li metteremo negli URL principali del progetto.

> [!NOTE]
> Non è necessario utilizzare nessuno di questo codice, ma è probabile che lo vogliate perché rende le cose molto più facili.
> Probabilmente avrete bisogno di cambiare il codice di gestione del form se cambiate il vostro modello utente, ma anche in tal caso, sarete in grado di usare le funzioni di vista incluse.

> [!NOTE]
> In questo caso, potremmo ragionevolmente mettere le pagine di autenticazione, compresi gli URL e i template, all'interno della nostra applicazione catalogo.
> Tuttavia, se abbiamo più applicazioni, sarebbe meglio separare questo comportamento di login condiviso e averlo disponibile su tutto il sito, quindi è quello che abbiamo mostrato qui!

### URL del progetto

Aggiungere quanto segue alla fine del file urls.py del progetto (**django-locallibrary-tutorial/locallibrary/urls.py**):

```python
# Add Django site authentication urls (for login, logout, password management)

urlpatterns += [
    path('accounts/', include('django.contrib.auth.urls')),
]
```

Navigare all'URL `http://127.0.0.1:8000/accounts/` (notare la barretta finale!).
Django mostrerà un errore che non ha trovato una mappatura per questo URL, e elencherà tutti gli URL che ha provato.
Da ciò potete vedere gli URL che funzioneranno una volta creati i modelli.

> [!NOTE]
> Aggiungendo il percorso `accounts/`, come mostrato sopra, si aggiungono i seguenti URL, insieme a nomi (indicati tra parentesi quadre) che possono essere utilizzati per invertire la mappatura degli URL. Non è necessario implementare altro — la mappatura degli URL sopra mappa automaticamente gli URL menzionati di seguito.
>
> ```python
> accounts/ login/ [name='login']
> accounts/ logout/ [name='logout']
> accounts/ password_change/ [name='password_change']
> accounts/ password_change/done/ [name='password_change_done']
> accounts/ password_reset/ [name='password_reset']
> accounts/ password_reset/done/ [name='password_reset_done']
> accounts/ reset/<uidb64>/<token>/ [name='password_reset_confirm']
> accounts/ reset/done/ [name='password_reset_complete']
> ```

Ora provate a navigare all'URL di login (`http://127.0.0.1:8000/accounts/login/`). Questo fallirà di nuovo, ma con un errore che vi dice che ci manca il modello richiesto (**registration/login.html**) nel percorso di ricerca del modello.
Vedrete le seguenti righe elencate nella sezione gialla in alto:

```python
Exception Type:    TemplateDoesNotExist
Exception Value:    registration/login.html
```

Il passo successivo è creare una directory per i template chiamata "registration" e quindi aggiungere il file **login.html**.

### Directory dei template

Gli URL (e implicitamente, le viste) che abbiamo appena aggiunto si aspettano di trovare i loro modelli associati in una directory **/registration/** da qualche parte nel percorso di ricerca dei modelli.

Per questo sito, metteremo le nostre pagine HTML nella directory **templates/registration/**. Questa directory dovrebbe trovarsi nella vostra directory radice del progetto, cioè la stessa directory delle cartelle **catalog** e **locallibrary**. Vi preghiamo di creare queste cartelle ora.

> [!NOTE]
> La vostra struttura delle cartelle dovrebbe ora apparire come quella di seguito:
>
> ```plain
> django-locallibrary-tutorial/   # Cartella del progetto principale di Django
>   catalog/
>   locallibrary/
>   templates/
>     registration/
> ```

Per rendere visibile la directory **templates** al caricatore di template, dobbiamo aggiungerla nel percorso di ricerca dei template.
Aprire le impostazioni del progetto (**/django-locallibrary-tutorial/locallibrary/settings.py**).

Quindi, importare il modulo `os` (aggiungere la seguente riga vicino alla parte superiore del file se non è già presente).

```python
import os # needed by code below
```

Aggiornare la linea `'DIRS'` della sezione `TEMPLATES` come mostrato:

```python
    # …
    TEMPLATES = [
      {
       # …
       'DIRS': [os.path.join(BASE_DIR, 'templates')],
       'APP_DIRS': True,
       # …
```

### Modello di login

> [!WARNING]
> I modelli di autenticazione forniti in questo articolo sono una versione molto basica/leggermente modificata dei modelli di dimostrazione del login di Django. Potrebbe essere necessario personalizzarli per il proprio uso!

Creare un nuovo file HTML chiamato /**django-locallibrary-tutorial/templates/registration/login.html** e dargli i seguenti contenuti:

```django
{% extends "base_generic.html" %}

{% block content %}

  {% if form.errors %}
    <p>Your username and password didn't match. Please try again.</p>
  {% endif %}

  {% if next %}
    {% if user.is_authenticated %}
      <p>Your account doesn't have access to this page. To proceed,
      please login with an account that has access.</p>
    {% else %}
      <p>Please login to see this page.</p>
    {% endif %}
  {% endif %}

  <form method="post" action="{% url 'login' %}">
    {% csrf_token %}
    <table>
      <tr>
        <td>\{{ form.username.label_tag }}</td>
        <td>\{{ form.username }}</td>
      </tr>
      <tr>
        <td>\{{ form.password.label_tag }}</td>
        <td>\{{ form.password }}</td>
      </tr>
    </table>
    <input type="submit" value="login">
    <input type="hidden" name="next" value="\{{ next }}">
  </form>

  {# Assumes you set up the password_reset view in your URLConf #}
  <p><a href="{% url 'password_reset' %}">Lost password?</a></p>

{% endblock %}
```

Questo modello condivide alcune somiglianze con quelli che abbiamo visto prima — estende il nostro modello base e sovrascrive il blocco `content`. Il resto del codice è un codice di gestione dei form abbastanza standard, che discuteremo in un tutorial successivo. Tutto quello che dovete sapere per ora è che questo visualizzerà un form in cui potete inserire il vostro username e la vostra password, e che se immettete valori non validi, vi sarà richiesto di inserire i valori corretti quando la pagina viene aggiornata.

Tornate alla pagina di login (`http://127.0.0.1:8000/accounts/login/`) una volta salvato il vostro modello, e dovreste vedere qualcosa di simile a questo:

![Pagina di login della biblioteca v1](library_login.png)

Se effettuate il login utilizzando credenziali valide, sarete reindirizzati a un'altra pagina (per impostazione predefinita, sarà `http://127.0.0.1:8000/accounts/profile/`). Il problema è che, per impostazione predefinita, Django si aspetta che una volta effettuato il login, vogliate essere portati a una pagina del profilo, il che potrebbe o non potrebbe essere il caso. Poiché non avete ancora definito questa pagina, riceverete un altro errore!

Aprire le impostazioni del progetto (**/django-locallibrary-tutorial/locallibrary/settings.py**) e aggiungere il testo di seguito alla fine. Ora, quando effettuate il login, dovreste essere reindirizzati alla home page del sito per impostazione predefinita.

```python
# Redirect to home URL after login (Default redirects to /accounts/profile/)
LOGIN_REDIRECT_URL = '/'
```

### Modello di logout

Se navigate all'URL di logout (`http://127.0.0.1:8000/accounts/logout/`), riceverete un errore perché Django 5 non consente il logout utilizzando `GET`, solo `POST`.
Aggiungeremo un form che potete utilizzare per effettuare il logout tra un minuto, ma prima creeremo la pagina a cui gli utenti vengono portati dopo il logout.

Creare e aprire **/django-locallibrary-tutorial/templates/registration/logged_out.html**. Copiare il testo di seguito:

```django
{% extends "base_generic.html" %}

{% block content %}
  <p>Logged out!</p>
  <a href="{% url 'login'%}">Click here to login again.</a>
{% endblock %}
```

Questo modello è molto semplice. Visualizza solo un messaggio che informa che siete stati disconnessi e fornisce un link che potete premere per tornare alla schermata di login. La schermata si renderizza in questo modo (dopo il logout):

![Pagina di logout della biblioteca v1](library_logout.png)

### Modelli di reset della password

Il sistema di reset della password predefinito utilizza email per inviare all'utente un link di reset. È necessario creare moduli per ottenere l'indirizzo email dell'utente, inviare l'email, consentire loro di inserire una nuova password e per segnalare quando l'intero processo è completo.

I seguenti modelli possono essere utilizzati come punto di partenza.

#### Form di reset della password

Questo è il form usato per ottenere l'indirizzo email dell'utente (per inviare l'email di reset della password). Creare **/django-locallibrary-tutorial/templates/registration/password_reset_form.html**, e dargli i seguenti contenuti:

```django
{% extends "base_generic.html" %}

{% block content %}
  <form action="" method="post">
  {% csrf_token %}
  {% if form.email.errors %}
    \{{ form.email.errors }}
  {% endif %}
      <p>\{{ form.email }}</p>
    <input type="submit" class="btn btn-default btn-lg" value="Reset password">
  </form>
{% endblock %}
```

#### Reset della password completato

Questo form viene visualizzato dopo che l'indirizzo email è stato raccolto. Creare **/django-locallibrary-tutorial/templates/registration/password_reset_done.html**, e dargli i seguenti contenuti:

```django
{% extends "base_generic.html" %}

{% block content %}
  <p>We've emailed you instructions for setting your password. If they haven't arrived in a few minutes, check your spam folder.</p>
{% endblock %}
```

#### Email di reset della password

Questo template fornisce il testo dell'email HTML contenente il link di reset che invieremo agli utenti. Creare **/django-locallibrary-tutorial/templates/registration/password_reset_email.html**, e dargli i seguenti contenuti:

```django
Someone asked for password reset for email \{{ email }}. Follow the link below:
\{{ protocol }}://\{{ domain }}{% url 'password_reset_confirm' uidb64=uid token=token %}
```

#### Conferma di reset della password

Questa pagina è dove si inserisce la nuova password dopo aver cliccato il link nell'email di reset della password. Creare **/django-locallibrary-tutorial/templates/registration/password_reset_confirm.html**, e dargli i seguenti contenuti:

```django
{% extends "base_generic.html" %}

{% block content %}
    {% if validlink %}
        <p>Please enter (and confirm) your new password.</p>
        <form action="" method="post">
        {% csrf_token %}
            <table>
                <tr>
                    <td>\{{ form.new_password1.errors }}
                        <label for="id_new_password1">New password:</label></td>
                    <td>\{{ form.new_password1 }}</td>
                </tr>
                <tr>
                    <td>\{{ form.new_password2.errors }}
                        <label for="id_new_password2">Confirm password:</label></td>
                    <td>\{{ form.new_password2 }}</td>
                </tr>
                <tr>
                    <td></td>
                    <td><input type="submit" value="Change my password"></td>
                </tr>
            </table>
        </form>
    {% else %}
        <h1>Password reset failed</h1>
        <p>The password reset link was invalid, possibly because it has already been used. Please request a new password reset.</p>
    {% endif %}
{% endblock %}
```

#### Completamento di reset della password

Questo è l'ultimo template di reset della password, che viene visualizzato per notificare quando il reset della password è riuscito. Creare **/django-locallibrary-tutorial/templates/registration/password_reset_complete.html**, e dargli i seguenti contenuti:

```django
{% extends "base_generic.html" %}

{% block content %}
  <h1>The password has been changed!</h1>
  <p><a href="{% url 'login' %}">log in again?</a></p>
{% endblock %}
```

### Test delle nuove pagine di autenticazione

Ora che avete aggiunto la configurazione degli URL e creato tutti questi template, le pagine di autenticazione (ad eccezione del logout) dovrebbero ora funzionare!

Potete testare le nuove pagine di autenticazione tentando per prima cosa di accedere al vostro account superuser utilizzando l'URL `http://127.0.0.1:8000/accounts/login/`.
Sarete in grado di testare la funzionalità di reset della password dal link nella pagina di login. **Tenete presente che Django invierà email di reset solo agli indirizzi (utenti) già memorizzati nel suo database!**

Nota che non potrete testare il logout dell'account ancora, perché le richieste di logout devono essere inviate come `POST` piuttosto che come richiesta `GET`.

> [!NOTE]
> Il sistema di reset della password richiede che il vostro sito web supporti l'email, cosa che è al di là dello scopo di questo articolo, quindi questa parte **non funzionerà ancora**. Per consentire il test, mettete la seguente riga alla fine del vostro file settings.py. Questo registra tutte le email inviate sulla console (così potete copiare il link di reset della password dalla console).
>
> ```python
> EMAIL_BACKEND = 'django.core.mail.backends.console.EmailBackend'
> ```
>
> Per ulteriori informazioni, vedere [Invio di email](https://docs.djangoproject.com/en/5.0/topics/email/) (documentazione di Django).

## Testare con utenti autenticati

Questa sezione esamina cosa possiamo fare per controllare selettivamente il contenuto che l'utente vede in base al fatto che siano o meno connessi.

### Test nei template

Potete ottenere informazioni sull'utente attualmente connesso nei template con la variabile di template `\{{ user }}` (questa è aggiunta al contesto dei template per impostazione predefinita quando impostate il progetto come abbiamo fatto nel nostro scheletro).

Tipicamente, testerete prima con la variabile di template `\{{ user.is_authenticated }}` per determinare se l'utente è idoneo a vedere contenuto specifico. Per dimostrare questo, ora aggiorneremo la nostra barra laterale per visualizzare un link "Login" se l'utente è disconnesso, e un link "Logout" se è connesso.

Aprire il modello base (**/django-locallibrary-tutorial/catalog/templates/base_generic.html**) e copiare il testo seguente nel blocco `sidebar`, immediatamente prima del tag di template `endblock`.

```django
  <ul class="sidebar-nav">
    …
   {% if user.is_authenticated %}
     <li>User: \{{ user.get_username }}</li>
     <li>
       <form id="logout-form" method="post" action="{% url 'logout' %}">
         {% csrf_token %}
         <button type="submit" class="btn btn-link">Logout</button>
       </form>
     </li>
   {% else %}
     <li><a href="{% url 'login' %}?next=\{{ request.path }}">Login</a></li>
   {% endif %}
    …
  </ul>
```

Come potete vedere, usiamo i tag di template `if` / `else` / `endif` per visualizzare condizionatamente il testo in base al fatto che `\{{ user.is_authenticated }}` sia vero. Se l'utente è autenticato, allora sappiamo di avere un utente valido, quindi chiamiamo `\{{ user.get_username }}` per visualizzare il loro nome.

Creiamo l'URL del link di login usando il tag di template `url` e il nome della configurazione dell'URL `login`. Si noti inoltre come abbiamo aggiunto `?next=\{{ request.path }}` alla fine dell'URL. Ciò che fa è aggiungere un parametro URL `next` contenente l'indirizzo (URL) della pagina _corrente_ alla fine dell'URL collegato. Dopo che l'utente ha effettuato con successo il login, la vista userà questo valore `next` per reindirizzare l'utente indietro alla pagina dove il link di login è stato cliccato.

Il codice del template di logout è diverso, perché da Django 5 per il logout si deve `POST` all'URL `admin:logout`, utilizzando un modulo con un pulsante.
Per impostazione predefinita, questo si renderà come un pulsante, ma potete stilizzare il pulsante per visualizzarlo come un link.
Per questo esempio, stiamo usando _Bootstrap_, quindi facciamo sembrare il pulsante come un link applicando `class="btn btn-link"`.
È inoltre necessario aggiungere i seguenti stili a **/django-locallibrary-tutorial/catalog/static/css/styles.css** per posizionare correttamente il link di logout accanto a tutti gli altri link della barra laterale:

```css
#logout-form {
  display: inline;
}
#logout-form button {
  padding: 0;
  margin: 0;
}
```

Provate a fare clic sui link Login/Logout nella barra laterale.
Dovreste essere portati alle pagine di logout/login che avete definito nella [Directory dei template](#directory_dei_template) sopra.

### Test nelle viste

Se stai usando viste basate su funzioni, il modo più semplice per limitare l'accesso alle tue funzioni è applicare il decorator `login_required` alla tua funzione di vista, come mostrato di seguito. Se l'utente è connesso, il codice della vista verrà eseguito normalmente. Se l'utente non è connesso, ciò reindirizzerà all'URL di login definito nelle impostazioni del progetto (`settings.LOGIN_URL`), passando il percorso assoluto corrente come parametro URL `next`. Se l'utente riesce a effettuare il login, verrà restituito a questa pagina, ma questa volta autenticato.

```python
from django.contrib.auth.decorators import login_required

@login_required
def my_view(request):
    # …
```

> [!NOTE]
> Potete fare la stessa cosa manualmente testando su `request.user.is_authenticated`, ma il decorator è molto più comodo!

Analogamente, il modo più semplice per limitare l'accesso agli utenti connessi nelle vostre viste basate su classi è derivare da `LoginRequiredMixin`. È necessario dichiarare questo mixin per primo nell'elenco della superclasse, prima della classe della vista principale.

```python
from django.contrib.auth.mixins import LoginRequiredMixin

class MyView(LoginRequiredMixin, View):
    # …
```

Questo ha esattamente lo stesso comportamento di reindirizzamento del decorator `login_required`. Potete anche specificare una posizione alternativa per reindirizzare l'utente se non è autenticato (`login_url`), e un nome di parametro URL invece di `next` per inserire il percorso assoluto corrente (`redirect_field_name`).

```python
class MyView(LoginRequiredMixin, View):
    login_url = '/login/'
    redirect_field_name = 'redirect_to'
```

Per ulteriori dettagli, consultare i [documenti Django qui](https://docs.djangoproject.com/en/5.0/topics/auth/default/#limiting-access-to-logged-in-users).

## Esempio — elencare i libri dell'utente corrente

Ora che sappiamo come limitare una pagina a un determinato utente, creiamo una vista dei libri che l'utente corrente ha preso in prestito.

Purtroppo, non abbiamo ancora un modo per far sì che gli utenti prendano in prestito i libri! Quindi, prima di poter creare l'elenco dei libri, estenderemo il modello `BookInstance` per supportare il concetto di prestito e useremo l'applicazione Admin di Django per concedere un numero di libri al nostro utente di test.

### Modelli

Per prima cosa, dovremo rendere possibile che gli utenti abbiano un `BookInstance` in prestito (abbiamo già uno `status` e una data `due_back`, ma non abbiamo ancora alcuna associazione tra questo modello e un utente particolare. Creeremo una usando un campo `ForeignKey` (uno-a-molti). Necessitiamo anche di un meccanismo semplice per verificare se un libro preso a prestito è scaduto.

Aprire **catalog/models.py**, e importare le `settings` da `django.conf` (aggiungere questo appena sotto la riga di importazione precedente nella parte superiore del file, così le impostazioni sono disponibili per il codice seguente che ne fa uso):

```python
from django.conf import settings
```

Successivamente, aggiungere il campo `borrower` al modello `BookInstance`, impostando il modello utente per la chiave come valore dell'impostazione `AUTH_USER_MODEL`.
Poiché non abbiamo sovrascritto l'impostazione con un [modello utente personalizzato](https://docs.djangoproject.com/en/5.0/topics/auth/customizing/), questo corrisponde al modello predefinito `User` da `django.contrib.auth.models`.

```python
borrower = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.SET_NULL, null=True, blank=True)
```

> [!NOTE]
> Importare il modello in questo modo riduce il lavoro richiesto se in seguito scoprite di aver bisogno di un modello utente personalizzato.
> Questo tutorial utilizza il modello predefinito, quindi potreste invece importare direttamente il modello `User` con le seguenti righe:
>
> ```python
> from django.contrib.auth.models import User
> ```
>
> ```python
> borrower = models.ForeignKey(User, on_delete=models.SET_NULL, null=True, blank=True)
> ```

Dato che siamo qui, aggiungiamo una proprietà che possiamo chiamare dai nostri template per sapere se una particolare istanza di libro è scaduta.
Anche se potremmo calcolarlo nel template stesso, usare una [proprietà](https://docs.python.org/3/library/functions.html#property) come mostrato di seguito sarà molto più efficiente.

Aggiungere questo da qualche parte nella parte superiore del file:

```python
from datetime import date
```

Ora aggiungi la definizione di proprietà alla classe `BookInstance`:

> [!NOTE]
> Il seguente codice utilizza la funzione `bool()` di Python, che valuta un oggetto o l'oggetto risultante di un'espressione e restituisce `True` a meno che il risultato non sia "falso", nel qual caso restituisce `False`.
> In Python un oggetto è _falso_ (valuta come `False`) se è: vuoto (come `[]`, `()`, `{}`), `0`, `None` o se è `False`.

```python
@property
def is_overdue(self):
    """Determines if the book is overdue based on due date and current date."""
    return bool(self.due_back and date.today() > self.due_back)
```

> [!NOTE]
> Verifichiamo prima se `due_back` è vuoto prima di fare un confronto. Un campo `due_back` vuoto causerebbe l'errore di Django invece di mostrare la pagina: i valori vuoti non sono comparabili. Questo non è qualcosa che vorremmo che i nostri utenti sperimentassero!

Ora che abbiamo aggiornato i nostri modelli, dovremo fare nuove migrazioni sul progetto e poi applicarle:

```bash
python3 manage.py makemigrations
python3 manage.py migrate
```

### Admin

Ora aprire **catalog/admin.py**, e aggiungere il campo `borrower` alla classe `BookInstanceAdmin` sia in `list_display` sia nei `fieldsets` come mostrato di seguito.
Questo renderà il campo visibile nella sezione Admin, permettendoci di assegnare un `User` a un `BookInstance` quando necessario.

```python
@admin.register(BookInstance)
class BookInstanceAdmin(admin.ModelAdmin):
    list_display = ('book', 'status', 'borrower', 'due_back', 'id')
    list_filter = ('status', 'due_back')

    fieldsets = (
        (None, {
            'fields': ('book', 'imprint', 'id')
        }),
        ('Availability', {
            'fields': ('status', 'due_back', 'borrower')
        }),
    )
```

### Prestare alcuni libri

Ora che è possibile prendere in prestito libri a utenti specifici, andate e prestate una serie di record `BookInstance`. Impostate il loro campo `borrowed` al vostro utente di test, impostate lo `status` su "On loan", e impostate le date di scadenza sia nel futuro che nel passato.

> [!NOTE]
> Non spiegheremo il processo, poiché sapete già come usare il sito Admin!

### Vista dei libri in prestito

Ora aggiungeremo una vista per ottenere l'elenco di tutti i libri che sono stati prestati all'utente corrente. Useremo la stessa vista di lista basata su classi generiche a cui siamo abituati, ma questa volta importeremo e deriveremo da `LoginRequiredMixin`, in modo che solo un utente connesso possa chiamare questa vista. Sceglieremo anche di dichiarare un `template_name`, piuttosto che usare il predefinito, perché potremmo finire per avere alcuni elenchi differenti di record BookInstance, con diverse viste e template.

Aggiungere quanto segue a **catalog/views.py**:

```python
from django.contrib.auth.mixins import LoginRequiredMixin

class LoanedBooksByUserListView(LoginRequiredMixin,generic.ListView):
    """Generic class-based view listing books on loan to current user."""
    model = BookInstance
    template_name = 'catalog/bookinstance_list_borrowed_user.html'
    paginate_by = 10

    def get_queryset(self):
        return (
            BookInstance.objects.filter(borrower=self.request.user)
            .filter(status__exact='o')
            .order_by('due_back')
        )
```

Per limitare la nostra query solo agli oggetti `BookInstance` dell'utente corrente, reimplementiamo `get_queryset()` come mostrato sopra. Si noti che "o" è il codice memorizzato per "in prestito" e ordiniamo per data `due_back` in modo che gli articoli più vecchi siano visualizzati per primi.

### Configurazione URL per i libri in prestito

Ora aprire **/catalog/urls.py** e aggiungere un `path()` che punta alla vista sopra (potete semplicemente copiare il testo qui sotto alla fine del file).

```python
urlpatterns += [
    path('mybooks/', views.LoanedBooksByUserListView.as_view(), name='my-borrowed'),
]
```

### Template per i libri in prestito

Ora, tutto ciò che dobbiamo fare per questa pagina è aggiungere un template. Creare prima il file template **/catalog/templates/catalog/bookinstance_list_borrowed_user.html** e dargli i seguenti contenuti:

```django
{% extends "base_generic.html" %}

{% block content %}
    <h1>Borrowed books</h1>

    {% if bookinstance_list %}
    <ul>

      {% for bookinst in bookinstance_list %}
      <li class="{% if bookinst.is_overdue %}text-danger{% endif %}">
        <a href="{% url 'book-detail' bookinst.book.pk %}">\{{ bookinst.book.title }}</a> (\{{ bookinst.due_back }})
      </li>
      {% endfor %}
    </ul>

    {% else %}
      <p>There are no books borrowed.</p>
    {% endif %}
{% endblock %}
```

Questo template è molto simile a quelli che abbiamo creato in precedenza per gli oggetti `Book` e `Author`.
L'unica cosa "nuova" qui è che verifichiamo il metodo che abbiamo aggiunto nel modello `(bookinst.is_overdue`) e lo usiamo per cambiare il colore degli articoli scaduti.

Quando il server di sviluppo è in esecuzione, dovreste ora essere in grado di visualizzare l'elenco per un utente connesso nel browser a `http://127.0.0.1:8000/catalog/mybooks/`. Provate questo con il vostro utente loggato e disconnesso (nel secondo caso, dovreste essere reindirizzati alla pagina di login).

### Aggiungere l'elenco alla barra laterale

L'ultimo passaggio è aggiungere un link per questa nuova pagina nella barra laterale. Lo metteremo nella stessa sezione in cui mostriamo altre informazioni per l'utente connesso.

Aprire il modello base (**/django-locallibrary-tutorial/catalog/templates/base_generic.html**) e aggiungere la riga "My Borrowed" alla barra laterale nella posizione mostrata di seguito.

```django
 <ul class="sidebar-nav">
   {% if user.is_authenticated %}
   <li>User: \{{ user.get_username }}</li>

   <li><a href="{% url 'my-borrowed' %}">My Borrowed</a></li>

   <li>
     <form id="logout-form" method="post" action="{% url 'admin:logout' %}">
       {% csrf_token %}
       <button type="submit" class="btn btn-link">Logout</button>
     </form>
   </li>
   {% else %}
   <li><a href="{% url 'login' %}?next=\{{ request.path }}">Login</a></li>
   {% endif %}
 </ul>
```

### Come appare?

Quando un utente è connesso, vedrà il link _My Borrowed_ nella barra laterale, e l'elenco dei libri viene visualizzato come di seguito (il primo libro non ha una data di scadenza, il che è un bug che speriamo di correggere in un tutorial successivo!).

![Biblioteca - libri presi in prestito dall'utente](library_borrowed_by_user.png)

## Permessi

I permessi sono associati ai modelli e definiscono le operazioni che possono essere eseguite su un'istanza del modello da un utente che ha il permesso. Per impostazione predefinita, Django automaticamente dà permessi di _aggiungere_, _cambiare_ e _cancellare_ a tutti i modelli, che permettono agli utenti con i permessi di eseguire le azioni associate tramite il sito admin. È possibile definire i propri permessi per i modelli e concederli a utenti specifici. È anche possibile cambiare i permessi associati a differenti istanze dello stesso modello.

I test sui permessi nelle viste e nei template sono quindi molto simili ai test sullo stato di autenticazione (e infatti, testare su un permesso verifica anche l'autenticazione).

### Modelli

La definizione dei permessi viene fatta nella sezione `class Meta` del modello, usando il campo `permissions`.
Potete specificare quanti permessi desiderate in una tupla, ciascun permesso stesso definito in una tupla nidificata contenente il nome del permesso e il valore di visualizzazione del permesso.
Ad esempio, potremmo definire un permesso per consentire a un utente di segnare che un libro è stato restituito come mostrato:

```python
class BookInstance(models.Model):
    # …
    class Meta:
        # …
        permissions = (("can_mark_returned", "Set book as returned"),)
```

Potremmo quindi assegnare il permesso a un gruppo "Librarian" nel sito Admin.

Aprire **catalog/models.py**, e aggiungere il permesso come mostrato sopra. Dovrete rieseguire le migrazioni (chiamare `python3 manage.py makemigrations` e `python3 manage.py migrate`) per aggiornare il database in modo appropriato.

### Template

I permessi dell'attuale utente sono memorizzati in una variabile di template chiamata `\{{ perms }}`. Potete verificare se l'utente attuale ha un determinato permesso utilizzando il nome specifico della variabile all'interno dell'app Django associata — ad esempio, `\{{ perms.catalog.can_mark_returned }}` sarà `True` se l'utente ha questo permesso, e `False` altrimenti. Tipicamente, verifichiamo il permesso utilizzando il tag di template `{% if %}` come mostrato:

```django
{% if perms.catalog.can_mark_returned %}
    <!-- We can mark a BookInstance as returned. -->
    <!-- Perhaps add code to link to a "book return" view here. -->
{% endif %}
```

### Viste

I permessi possono essere testati in funzioni di vista usando il decoratore `permission_required` o in una vista basata su classe usando `PermissionRequiredMixin`. I pattern sono li stessi dell'autenticazione di login, anche se ovviamente, potreste ragionevolmente dover aggiungere più permessi.

Decoratore per viste funzione:

```python
from django.contrib.auth.decorators import permission_required

@permission_required('catalog.can_mark_returned')
@permission_required('catalog.can_edit')
def my_view(request):
    # …
```

Un mixin con necessità di permesso per viste basate su classi.

```python
from django.contrib.auth.mixins import PermissionRequiredMixin

class MyView(PermissionRequiredMixin, View):
    permission_required = 'catalog.can_mark_returned'
    # Or multiple permissions
    permission_required = ('catalog.can_mark_returned', 'catalog.change_book')
    # Note that 'catalog.change_book' is permission
    # Is created automatically for the book model, along with add_book, and delete_book
```

> [!NOTE]
> C'è una piccola differenza predefinita nel comportamento sopra. Per **impostazione predefinita** per un utente connesso con una violazione del permesso:
>
> - `@permission_required` reindirizza alla schermata di login (HTTP Status 302).
> - `PermissionRequiredMixin` restituisce 403 (HTTP Status Forbidden).
>
> Normalmente vorrete il comportamento `PermissionRequiredMixin`: restituire 403 se un utente è loggato ma non ha il permesso corretto. Per fare ciò per una vista funzione, usate `@login_required` e `@permission_required` con `raise_exception=True` come mostrato:
>
> ```python
> from django.contrib.auth.decorators import login_required, permission_required
>
> @login_required
> @permission_required('catalog.can_mark_returned', raise_exception=True)
> def my_view(request):
>     # …
> ```

### Esempio

Non aggiorneremo qui il _LocalLibrary_; forse nel prossimo tutorial!

## Sfida

In precedenza in questo articolo, abbiamo mostrato come creare una pagina per l'utente corrente, elencando i libri che hanno preso in prestito.
La sfida ora è creare una pagina simile che sia visibile solo ai bibliotecari, che visualizza _tutti_ i libri che sono stati presi in prestito e che include il nome di ciascun prestatore.

Dovreste essere in grado di seguire lo stesso schema per l'altra vista. La differenza principale è che dovrete restringere la vista solo ai bibliotecari. Potreste farlo in base al fatto che l'utente sia un membro dello staff (decoratore della funzione: `staff_member_required`, variabile di template: `user.is_staff`), ma raccomandiamo invece di usare il permesso `can_mark_returned` e `PermissionRequiredMixin`, come descritto nella sezione precedente.

> [!WARNING]
> Ricordate di non usare il vostro superuser per i test basati sui permessi (i controlli sui permessi restituiscono sempre vero per i superuser, anche se un permesso non è ancora stato definito!). Invece, create un utente bibliotecario e aggiungete le capacità richieste.

Quando avrete terminato, la vostra pagina dovrebbe apparire come nello screenshot qui sotto.

![Tutti i libri in prestito, limitato ai bibliotecari](library_borrowed_all.png)

## Sommario

Ottimo lavoro — avete ora creato un sito web in cui i membri della biblioteca possono accedere e visualizzare il loro contenuto personale, e in cui i bibliotecari (con il permesso corretto) possono visualizzare tutti i libri prestati e i loro prestatori. Al momento stiamo ancora solo visualizzando il contenuto, ma gli stessi principi e tecniche sono utilizzati quando si desidera iniziare a modificare e aggiungere dati.

Nel nostro prossimo articolo, vedremo come potete usare i moduli Django per raccogliere l'input dell'utente e poi iniziare a modificare alcuni dei nostri dati archiviati.

## Vedere anche

- [Autenticazione utente in Django](https://docs.djangoproject.com/en/5.0/topics/auth/) (documentazione di Django)
- [Utilizzare il sistema di autenticazione (predefinito) di Django](https://docs.djangoproject.com/en/5.0/topics/auth/default/) (documentazione di Django)
- [Introduzione alle viste basate su classi > Decorare le viste basate su classi](https://docs.djangoproject.com/en/5.0/topics/class-based-views/intro/#decorating-class-based-views) (documentazione di Django)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Sessions", "Learn_web_development/Extensions/Server-side/Django/Forms", "Learn_web_development/Extensions/Server-side/Django")}}
