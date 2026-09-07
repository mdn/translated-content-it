---
title: "Tutorial su Django, parte 8: Autenticazione utente e permessi"
short-title: "8: Autenticazione e permessi"
slug: Learn_web_development/Extensions/Server-side/Django/Authentication
l10n:
  sourceCommit: 81a384e18b61c1d1b23d7f58f1fbd8ec3af45558
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Sessions", "Learn_web_development/Extensions/Server-side/Django/Forms", "Learn_web_development/Extensions/Server-side/Django")}}

In questo tutorial verrà mostrato come consentire agli utenti di accedere al sito con i propri account e come controllare ciò che possono fare e vedere in base al fatto che abbiano effettuato o meno l'accesso e ai loro _permessi_. Nell'ambito di questa dimostrazione, verrà esteso il sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website), aggiungendo pagine di login e logout, nonché pagine specifiche per utenti e personale per visualizzare i libri presi in prestito.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Completare tutti gli argomenti precedenti del tutorial, incluso <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/Sessions">Tutorial su Django, parte 7: Framework delle sessioni</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere come configurare e utilizzare l'autenticazione utente e i permessi.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

Django fornisce un sistema di autenticazione e autorizzazione ("permessi"), basato sul framework delle sessioni illustrato nel [tutorial precedente](/it/docs/Learn_web_development/Extensions/Server-side/Django/Sessions), che consente di verificare le credenziali degli utenti e definire quali azioni ogni utente è autorizzato a eseguire. Il framework include modelli integrati per `Users` e `Groups` (un modo generico per applicare permessi a più utenti contemporaneamente), permessi/flag che indicano se un utente può eseguire un'attività, form e viste per effettuare il login degli utenti e strumenti per le viste che limitano i contenuti.

> [!NOTE]
> Secondo Django, il sistema di autenticazione mira a essere molto generico e pertanto non fornisce alcune funzionalità offerte da altri sistemi di autenticazione web. Sono disponibili pacchetti di terze parti per risolvere alcuni problemi comuni. Ad esempio, il {{Glossary("throttle", "throttling")}} dei tentativi di login e l'autenticazione presso terze parti (ad esempio OAuth).

In questo tutorial verrà mostrato come abilitare l'autenticazione utente nel sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website), creare pagine personalizzate di login e logout, aggiungere permessi ai modelli e controllare l'accesso alle pagine. Verranno utilizzati autenticazione e permessi per visualizzare elenchi di libri presi in prestito sia per gli utenti sia per i bibliotecari.

Il sistema di autenticazione è molto flessibile ed è possibile costruire URL, form, viste e template da zero, richiamando semplicemente l'API fornita per effettuare il login dell'utente. Tuttavia, in questo articolo verranno utilizzate le viste e i form di autenticazione "standard" di Django per le pagine di login e logout. Sarà comunque necessario creare alcuni template, ma è piuttosto semplice.

Verrà inoltre mostrato come creare permessi e controllare lo stato di login e i permessi sia nelle viste sia nei template.

## Abilitazione dell'autenticazione

L'autenticazione è stata abilitata automaticamente quando è stato [creato il sito web scheletro](/it/docs/Learn_web_development/Extensions/Server-side/Django/skeleton_website) (nel tutorial 2), quindi a questo punto non è necessario fare altro.

> [!NOTE]
> Tutta la configurazione necessaria è stata eseguita al momento della creazione dell'app usando il comando `django-admin startproject`. Le tabelle del database per gli utenti e i permessi dei modelli sono state create quando è stato chiamato per la prima volta `python manage.py migrate`.

La configurazione è impostata nelle sezioni `INSTALLED_APPS` e `MIDDLEWARE` del file di progetto (**django-locallibrary-tutorial/locallibrary/settings.py**), come mostrato di seguito:

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

## Creazione di utenti e gruppi

Il primo utente è già stato creato durante l'analisi del [sito di amministrazione Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/Admin_site) nel tutorial 4 (si trattava di un superutente, creato con il comando `python manage.py createsuperuser`).
Il superutente è già autenticato e dispone di tutti i permessi, quindi sarà necessario creare un utente di test che rappresenti un normale utente del sito. Verrà utilizzato il sito di amministrazione per creare i gruppi _locallibrary_ e i login del sito web, poiché è uno dei modi più rapidi per farlo.

> [!NOTE]
> È inoltre possibile creare utenti programmaticamente, come mostrato di seguito.
> Questo sarebbe necessario, ad esempio, se si stesse sviluppando un'interfaccia che consenta agli utenti "ordinari" di creare i propri login (alla maggior parte degli utenti non dovrebbe essere dato accesso al sito di amministrazione).
>
> ```python
> from django.contrib.auth.models import User
>
> # Create user and save to the database
> user = User.objects.create_user('myusername', 'myemail@crazymail.com', 'mypassword')
>
> # Update fields and then save again
> user.first_name = 'Tyrone'
> user.last_name = 'Citizen'
> user.save()
> ```
>
> Si noti tuttavia che è altamente consigliato configurare un _modello utente personalizzato_ all'avvio di un progetto, poiché sarà possibile personalizzarlo facilmente in futuro, se necessario.
> Se si utilizza un modello utente personalizzato, il codice per creare lo stesso utente avrebbe questo aspetto:
>
> ```python
> # Get current user model from settings
> from django.contrib.auth import get_user_model
> User = get_user_model()
>
> # Create user from model and save to the database
> user = User.objects.create_user('myusername', 'myemail@crazymail.com', 'mypassword')
>
> # Update fields and then save again
> user.first_name = 'Tyrone'
> user.last_name = 'Citizen'
> user.save()
> ```
>
> Per ulteriori informazioni, consultare [Using a custom user model when starting a project](https://docs.djangoproject.com/en/5.0/topics/auth/customizing/#using-a-custom-user-model-when-starting-a-project) (documentazione Django).

Di seguito verranno creati prima un gruppo e poi un utente. Anche se non sono ancora presenti permessi da aggiungere per i membri della biblioteca, qualora fosse necessario in seguito sarà molto più semplice aggiungerli una sola volta al gruppo anziché individualmente a ciascun membro.

Avviare il server di sviluppo e accedere al sito di amministrazione nel browser web locale (`http://127.0.0.1:8000/admin/`). Effettuare il login al sito usando le credenziali dell'account superutente. Il livello superiore del sito di amministrazione mostra tutti i modelli, ordinati per "applicazione Django". Dalla sezione **Authentication and Authorization**, è possibile fare clic sui link **Users** o **Groups** per vedere i record esistenti.

![Sito di amministrazione - aggiungere gruppi o utenti](admin_authentication_add.png)

Per prima cosa, creare un nuovo gruppo per i membri della biblioteca.

1. Fare clic sul pulsante **Add** (accanto a Group) per creare un nuovo _Group_; inserire il **Name** "Library Members" per il gruppo.
   ![Sito di amministrazione - aggiungere un gruppo](admin_authentication_add_group.png)
2. Non sono necessari permessi per il gruppo, quindi premere semplicemente **SAVE** (verrà visualizzato un elenco di gruppi).

Ora creare un utente:

1. Tornare alla pagina principale del sito di amministrazione.
2. Fare clic sul pulsante **Add** accanto a _Users_ per aprire la finestra di dialogo _Add user_.
   ![Sito di amministrazione - aggiungere un utente parte 1](admin_authentication_add_user_prt1.png)
3. Inserire un **Username** e una **Password**/**Password confirmation** appropriati per l'utente di test.
4. Premere **SAVE** per creare l'utente.

   Il sito di amministrazione creerà il nuovo utente e porterà immediatamente alla schermata _Change user_, in cui è possibile modificare lo **username** e aggiungere informazioni per i campi facoltativi del modello User. Questi campi includono nome, cognome, indirizzo email, stato e permessi dell'utente (deve essere impostato solo il flag **Active**). Più in basso è possibile specificare i gruppi e i permessi dell'utente e vedere date importanti relative all'utente, ad esempio la data di iscrizione e l'ultima data di login.
   ![Sito di amministrazione - aggiungere un utente parte 2](admin_authentication_add_user_prt2.png)

5. Nella sezione _Groups_, selezionare il gruppo **Library Member** dall'elenco _Available groups_, quindi premere la **freccia destra** tra le caselle per spostarlo nella casella _Chosen groups_.
   ![Sito di amministrazione - aggiungere un utente al gruppo](admin_authentication_user_add_group.png)
6. Non è necessario fare altro, quindi selezionare nuovamente **SAVE** per passare all'elenco degli utenti.

Ecco fatto. Ora è disponibile un account di "normale membro della biblioteca" che sarà possibile utilizzare per i test, una volta implementate le pagine che ne consentono il login.

> [!NOTE]
> Provare a creare un altro utente membro della biblioteca. Creare inoltre un gruppo per i bibliotecari e aggiungervi un utente.

## Configurazione delle viste di autenticazione

Django fornisce praticamente tutto il necessario per creare pagine di autenticazione che gestiscono login, logout e gestione delle password "pronte all'uso". Questo include un mapper URL, viste e form, ma non include i template: sarà necessario creare quelli personalizzati.

In questa sezione viene mostrato come integrare il sistema predefinito nel sito web _LocalLibrary_ e creare i template.

> [!NOTE]
> Django non include una vista di autenticazione integrata per la registrazione iniziale degli utenti ("signup").
> È possibile crearne una se necessario, ma in questo tutorial si presume che solo i bibliotecari siano autorizzati a registrare utenti e che lo facciano usando l'interfaccia di amministrazione Django.

> [!NOTE]
> Non è obbligatorio usare questo codice, ma è probabile che lo si voglia fare perché semplifica notevolmente le operazioni.
> Sarà quasi certamente necessario modificare il codice di gestione dei form se viene modificato il modello utente, ma anche in questo caso sarà comunque possibile utilizzare le funzioni di vista standard.

> [!NOTE]
> In questo caso, sarebbe ragionevole inserire le pagine di autenticazione, inclusi URL e template, all'interno dell'applicazione catalog.
> Tuttavia, se ci fossero più applicazioni, sarebbe meglio separare questo comportamento di login condiviso e renderlo disponibile nell'intero sito; è quindi questo l'approccio mostrato qui.

### URL del progetto

Aggiungere quanto segue alla fine del file urls.py del progetto (**django-locallibrary-tutorial/locallibrary/urls.py**):

```python
# Add Django site authentication urls (for login, logout, password management)

urlpatterns += [
    path('accounts/', include('django.contrib.auth.urls')),
]
```

Accedere all'URL `http://127.0.0.1:8000/accounts/` (notare la barra finale).
Django mostrerà un errore indicando che non è stato possibile trovare una mappatura per questo URL ed elencherà tutti gli URL provati.
Da questo elenco è possibile vedere gli URL che funzioneranno una volta creati i template.

> [!NOTE]
> L'aggiunta del percorso `accounts/` come mostrato sopra aggiunge i seguenti URL, insieme a nomi, indicati tra parentesi quadre, che possono essere utilizzati per invertire le mappature URL. Non è necessario implementare altro: la mappatura URL precedente mappa automaticamente gli URL riportati di seguito.
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

Ora provare ad accedere all'URL di login (`http://127.0.0.1:8000/accounts/login/`). Anche questa operazione non riuscirà, ma con un errore che indica l'assenza del template richiesto (**registration/login.html**) nel percorso di ricerca dei template.
Nella sezione gialla in alto saranno elencate le seguenti righe:

```python
Exception Type:    TemplateDoesNotExist
Exception Value:    registration/login.html
```

Il passaggio successivo consiste nel creare una directory per i template denominata "registration" e quindi aggiungere il file **login.html**.

### Directory dei template

Gli URL, e implicitamente le viste, appena aggiunti prevedono di trovare i template associati in una directory **/registration/** presente da qualche parte nel percorso di ricerca dei template.

Per questo sito, le pagine HTML verranno collocate nella directory **templates/registration/**. Questa directory deve trovarsi nella directory radice del progetto, ossia nella stessa directory delle cartelle **catalog** e **locallibrary**. Creare ora queste cartelle.

> [!NOTE]
> La struttura delle cartelle dovrebbe ora essere simile alla seguente:
>
> ```plain
> django-locallibrary-tutorial/   # Django top level project folder
>   catalog/
>   locallibrary/
>   templates/
>     registration/
> ```

Per rendere la directory **templates** visibile al loader dei template, è necessario aggiungerla al percorso di ricerca dei template.
Aprire le impostazioni del progetto (**/django-locallibrary-tutorial/locallibrary/settings.py**).

Quindi importare il modulo `os` (aggiungere la riga seguente vicino all'inizio del file se non è già presente).

```python
import os # needed by code below
```

Aggiornare la riga `'DIRS'` della sezione `TEMPLATES` come mostrato:

```python
    # …
    TEMPLATES = [
      {
       # …
       'DIRS': [os.path.join(BASE_DIR, 'templates')],
       'APP_DIRS': True,
       # …
```

### Template di login

> [!WARNING]
> I template di autenticazione forniti in questo articolo sono una versione molto essenziale/leggermente modificata dei template di login dimostrativi di Django. Potrebbe essere necessario personalizzarli per l'uso specifico.

Creare un nuovo file HTML denominato /**django-locallibrary-tutorial/templates/registration/login.html** e assegnargli il seguente contenuto:

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

Questo template presenta alcune somiglianze con quelli visti in precedenza: estende il template di base e sovrascrive il blocco `content`. Il resto del codice è codice standard di gestione dei form, che verrà discusso in un tutorial successivo. Per ora è sufficiente sapere che verrà visualizzato un form in cui inserire username e password e che, se vengono immessi valori non validi, verrà richiesto di inserire valori corretti al successivo aggiornamento della pagina.

Tornare alla pagina di login (`http://127.0.0.1:8000/accounts/login/`) dopo aver salvato il template; dovrebbe essere visualizzato qualcosa di simile:

![Pagina di login della biblioteca v1](library_login.png)

Se viene effettuato il login usando credenziali valide, si verrà reindirizzati a un'altra pagina, per impostazione predefinita `http://127.0.0.1:8000/accounts/profile/`. Il problema è che, per impostazione predefinita, Django presume che dopo il login si desideri accedere a una pagina del profilo, cosa che potrebbe essere o meno appropriata. Poiché questa pagina non è ancora stata definita, verrà visualizzato un altro errore.

Aprire le impostazioni del progetto (**/django-locallibrary-tutorial/locallibrary/settings.py**) e aggiungere il testo seguente in fondo. Ora, dopo il login, il reindirizzamento predefinito dovrebbe portare alla homepage del sito.

```python
# Redirect to home URL after login (Default redirects to /accounts/profile/)
LOGIN_REDIRECT_URL = '/'
```

### Template di logout

Accedendo all'URL di logout (`http://127.0.0.1:8000/accounts/logout/`) verrà visualizzato un errore perché Django 5 non consente il logout tramite `GET`, ma solo tramite `POST`.
Tra poco verrà aggiunto un form utilizzabile per il logout, ma prima verrà creata la pagina a cui gli utenti vengono indirizzati dopo il logout.

Creare e aprire **/django-locallibrary-tutorial/templates/registration/logged_out.html**. Copiare il testo seguente:

```django
{% extends "base_generic.html" %}

{% block content %}
  <p>Logged out!</p>
  <a href="{% url 'login'%}">Click here to login again.</a>
{% endblock %}
```

Questo template è molto semplice. Visualizza solo un messaggio che informa dell'avvenuto logout e fornisce un link selezionabile per tornare alla schermata di login. La schermata viene renderizzata in questo modo, dopo il logout:

![Pagina di logout della biblioteca v1](library_logout.png)

### Template per il ripristino della password

Il sistema predefinito di ripristino della password utilizza le email per inviare all'utente un link di ripristino. È necessario creare form per ottenere l'indirizzo email dell'utente, inviare l'email, consentire l'inserimento di una nuova password e indicare quando l'intero processo è completato.

I seguenti template possono essere utilizzati come punto di partenza.

#### Form per il ripristino della password

Questo è il form utilizzato per ottenere l'indirizzo email dell'utente, necessario per inviare l'email di ripristino della password. Creare **/django-locallibrary-tutorial/templates/registration/password_reset_form.html** e assegnargli il seguente contenuto:

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

#### Ripristino della password completato

Questo form viene visualizzato dopo la raccolta dell'indirizzo email. Creare **/django-locallibrary-tutorial/templates/registration/password_reset_done.html** e assegnargli il seguente contenuto:

```django
{% extends "base_generic.html" %}

{% block content %}
  <p>We've emailed you instructions for setting your password. If they haven't arrived in a few minutes, check your spam folder.</p>
{% endblock %}
```

#### Email per il ripristino della password

Questo template fornisce il testo dell'email HTML contenente il link di ripristino da inviare agli utenti. Creare **/django-locallibrary-tutorial/templates/registration/password_reset_email.html** e assegnargli il seguente contenuto:

```django
Someone asked for password reset for email \{{ email }}. Follow the link below:
\{{ protocol }}://\{{ domain }}{% url 'password_reset_confirm' uidb64=uid token=token %}
```

#### Conferma del ripristino della password

Questa pagina consente di inserire la nuova password dopo aver fatto clic sul link nell'email di ripristino della password. Creare **/django-locallibrary-tutorial/templates/registration/password_reset_confirm.html** e assegnargli il seguente contenuto:

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

#### Ripristino della password completo

Questo è l'ultimo template per il ripristino della password, visualizzato per notificare il completamento riuscito del ripristino. Creare **/django-locallibrary-tutorial/templates/registration/password_reset_complete.html** e assegnargli il seguente contenuto:

```django
{% extends "base_generic.html" %}

{% block content %}
  <h1>The password has been changed!</h1>
  <p><a href="{% url 'login' %}">log in again?</a></p>
{% endblock %}
```

### Test delle nuove pagine di autenticazione

Ora che la configurazione degli URL è stata aggiunta e sono stati creati tutti questi template, le pagine di autenticazione, eccetto il logout, dovrebbero funzionare.

È possibile testare le nuove pagine di autenticazione tentando prima di effettuare il login all'account superutente tramite l'URL `http://127.0.0.1:8000/accounts/login/`.
Sarà possibile testare la funzionalità di ripristino della password tramite il link nella pagina di login. **Tenere presente che Django invierà email di ripristino solo agli indirizzi, ossia agli utenti, già memorizzati nel suo database.**

Si noti che non sarà ancora possibile testare il logout dell'account, poiché le richieste di logout devono essere inviate come richieste `POST` anziché `GET`.

> [!NOTE]
> Il sistema di ripristino della password richiede che il sito web supporti l'email, un argomento che va oltre lo scopo di questo articolo; pertanto questa parte **non funzionerà ancora**. Per consentire i test, inserire la riga seguente alla fine del file settings.py. In questo modo tutte le email inviate vengono registrate nella console, consentendo di copiare il link per il ripristino della password dalla console.
>
> ```python
> EMAIL_BACKEND = 'django.core.mail.backends.console.EmailBackend'
> ```
>
> Per ulteriori informazioni, consultare [Sending email](https://docs.djangoproject.com/en/5.0/topics/email/) (documentazione Django).

## Test rispetto agli utenti autenticati

Questa sezione esamina cosa è possibile fare per controllare selettivamente i contenuti che l'utente vede a seconda che abbia effettuato il login oppure no.

### Test nei template

Nei template è possibile ottenere informazioni sull'utente attualmente autenticato con la variabile di template `\{{ user }}`; questa viene aggiunta al contesto del template per impostazione predefinita quando si configura il progetto come fatto nello scheletro.

In genere, verrà prima verificata la variabile di template `\{{ user.is_authenticated }}` per stabilire se l'utente è autorizzato a visualizzare contenuti specifici. Per dimostrarlo, verrà ora aggiornato il riquadro laterale affinché visualizzi un link "Login" se l'utente ha effettuato il logout e un link "Logout" se ha effettuato il login.

Aprire il template di base (**/django-locallibrary-tutorial/catalog/templates/base_generic.html**) e copiare il testo seguente nel blocco `sidebar`, immediatamente prima del tag di template `endblock`.

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

Come si può vedere, vengono utilizzati i tag di template `if` / `else` / `endif` per visualizzare condizionalmente il testo in base al fatto che `\{{ user.is_authenticated }}` sia vero. Se l'utente è autenticato, si sa di avere un utente valido, quindi viene chiamato `\{{ user.get_username }}` per visualizzarne il nome.

L'URL del link di login viene creato usando il tag di template `url` e il nome della configurazione URL `login`. Si noti inoltre che è stato aggiunto `?next=\{{ request.path }}` alla fine dell'URL. Ciò aggiunge un parametro URL `next`, contenente l'indirizzo URL della pagina _corrente_, alla fine dell'URL collegato. Dopo che l'utente ha effettuato correttamente il login, la vista utilizzerà questo valore `next` per reindirizzare l'utente alla pagina in cui ha fatto inizialmente clic sul link di login.

Il codice del template di logout è diverso perché, a partire da Django 5, per effettuare il logout è necessario inviare una richiesta `POST` all'URL `admin:logout`, utilizzando un form con un pulsante.
Per impostazione predefinita questo verrebbe renderizzato come un pulsante, ma è possibile applicare stili al pulsante affinché venga visualizzato come un link.
In questo esempio viene utilizzato _Bootstrap_, quindi il pulsante viene reso simile a un link applicando `class="btn btn-link"`.
È inoltre necessario aggiungere gli stili seguenti a **/django-locallibrary-tutorial/catalog/static/css/styles.css** per posizionare correttamente il link di logout accanto a tutti gli altri link del riquadro laterale:

```css
#logout-form {
  display: inline;
}
#logout-form button {
  padding: 0;
  margin: 0;
}
```

Provare facendo clic sui link Login/Logout nel riquadro laterale.
Si dovrebbe essere indirizzati alle pagine di logout/login definite nella sezione [Directory dei template](#directory_dei_template) precedente.

### Test nelle viste

Se vengono utilizzate viste basate su funzioni, il modo più semplice per limitare l'accesso alle funzioni consiste nell'applicare il decoratore `login_required` alla funzione di vista, come mostrato di seguito. Se l'utente ha effettuato il login, il codice della vista verrà eseguito normalmente. Se l'utente non ha effettuato il login, verrà reindirizzato all'URL di login definito nelle impostazioni del progetto (`settings.LOGIN_URL`), passando il percorso assoluto corrente come parametro URL `next`. Se l'utente riesce a effettuare il login, verrà reindirizzato a questa pagina, ma questa volta autenticato.

```python
from django.contrib.auth.decorators import login_required

@login_required
def my_view(request):
    # …
```

> [!NOTE]
> Lo stesso tipo di controllo può essere eseguito manualmente verificando `request.user.is_authenticated`, ma il decoratore è molto più pratico.

Analogamente, il modo più semplice per limitare l'accesso agli utenti autenticati nelle viste basate su classi consiste nel derivare da `LoginRequiredMixin`. È necessario dichiarare questo mixin per primo nell'elenco delle superclassi, prima della classe di vista principale.

```python
from django.contrib.auth.mixins import LoginRequiredMixin

class MyView(LoginRequiredMixin, View):
    # …
```

Questo comportamento di reindirizzamento è esattamente lo stesso del decoratore `login_required`. È inoltre possibile specificare una posizione alternativa a cui reindirizzare l'utente se non è autenticato (`login_url`) e un nome di parametro URL, al posto di `next`, in cui inserire il percorso assoluto corrente (`redirect_field_name`).

```python
class MyView(LoginRequiredMixin, View):
    login_url = '/login/'
    redirect_field_name = 'redirect_to'
```

Per ulteriori dettagli, consultare la [documentazione Django qui](https://docs.djangoproject.com/en/5.0/topics/auth/default/#limiting-access-to-logged-in-users).

## Esempio: elenco dei libri dell'utente corrente

Ora che si sa come limitare una pagina a un determinato utente, creare una vista dei libri presi in prestito dall'utente corrente.

Sfortunatamente, non esiste ancora alcun modo per consentire agli utenti di prendere in prestito libri. Prima di poter creare l'elenco dei libri, sarà quindi necessario estendere il modello `BookInstance` per supportare il concetto di prestito e usare l'applicazione Django Admin per prestare alcuni libri all'utente di test.

### Modelli

Per prima cosa, sarà necessario rendere possibile per gli utenti avere un `BookInstance` in prestito. Esistono già uno `status` e una data `due_back`, ma non esiste ancora alcuna associazione tra questo modello e un utente specifico. Verrà creata usando un campo `ForeignKey` (uno-a-molti). È inoltre necessario un meccanismo semplice per verificare se un libro in prestito è scaduto.

Aprire **catalog/models.py** e importare `settings` da `django.conf` (aggiungere questa istruzione appena sotto la riga di importazione precedente all'inizio del file, in modo che le impostazioni siano disponibili per il codice successivo che le utilizza):

```python
from django.conf import settings
```

Successivamente, aggiungere il campo `borrower` al modello `BookInstance`, impostando il modello utente per la chiave come valore dell'impostazione `AUTH_USER_MODEL`.
Poiché l'impostazione non è stata sovrascritta con un [modello utente personalizzato](https://docs.djangoproject.com/en/5.0/topics/auth/customizing/), questo viene mappato al modello `User` predefinito di `django.contrib.auth.models`.

```python
borrower = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.SET_NULL, null=True, blank=True)
```

> [!NOTE]
> L'importazione del modello in questo modo riduce il lavoro necessario se in seguito si scopre di aver bisogno di un modello utente personalizzato.
> Questo tutorial utilizza il modello predefinito, quindi sarebbe possibile importare direttamente il modello `User` con le righe seguenti:
>
> ```python
> from django.contrib.auth.models import User
> ```
>
> ```python
> borrower = models.ForeignKey(User, on_delete=models.SET_NULL, null=True, blank=True)
> ```

Già che ci si trova qui, aggiungere una proprietà che possa essere chiamata dai template per indicare se una particolare istanza di libro è scaduta.
Sebbene sia possibile calcolarla direttamente nel template, l'uso di una [property](https://docs.python.org/3/library/functions.html#property), come mostrato di seguito, sarà molto più efficiente.

Aggiungere questo codice da qualche parte vicino all'inizio del file:

```python
from datetime import date
```

Ora aggiungere la seguente definizione di proprietà alla classe `BookInstance`:

> [!NOTE]
> Il codice seguente utilizza la funzione `bool()` di Python, che valuta un oggetto o l'oggetto risultante da un'espressione e restituisce `True` a meno che il risultato non sia "falsy", nel qual caso restituisce `False`.
> In Python un oggetto è _falsy_, ovvero viene valutato come `False`, se è vuoto, come `[]`, `()` o `{}`, se è `0`, `None` o `False`.

```python
@property
def is_overdue(self):
    """Determines if the book is overdue based on due date and current date."""
    return bool(self.due_back and date.today() > self.due_back)
```

> [!NOTE]
> Per prima cosa viene verificato se `due_back` è vuoto prima di eseguire un confronto. Un campo `due_back` vuoto farebbe generare a Django un errore anziché mostrare la pagina: i valori vuoti non sono confrontabili. Non è un'esperienza che gli utenti dovrebbero affrontare.

Ora che i modelli sono stati aggiornati, sarà necessario creare nuove migrazioni sul progetto e quindi applicarle:

```bash
python3 manage.py makemigrations
python3 manage.py migrate
```

### Admin

Ora aprire **catalog/admin.py** e aggiungere il campo `borrower` alla classe `BookInstanceAdmin` sia in `list_display` sia in `fieldsets`, come mostrato di seguito.
Questo renderà il campo visibile nella sezione Admin, consentendo di assegnare un `User` a un `BookInstance` quando necessario.

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

Ora che è possibile prestare libri a un utente specifico, andare a prestare alcuni record `BookInstance`. Impostare il loro campo `borrowed` sull'utente di test, impostare lo `status` su "On loan" e definire date di scadenza sia future sia passate.

> [!NOTE]
> Il processo non verrà descritto nel dettaglio, poiché si sa già come usare il sito di amministrazione.

### Vista dei prestiti

Ora verrà aggiunta una vista per ottenere l'elenco di tutti i libri prestati all'utente corrente. Verrà utilizzata la stessa vista elenco generica basata su classi già nota, ma questa volta verranno anche importati e usati `LoginRequiredMixin`, in modo che solo un utente autenticato possa chiamare questa vista. Verrà inoltre scelto di dichiarare un `template_name`, anziché usare quello predefinito, poiché potrebbero esserci diversi elenchi di record BookInstance, con viste e template differenti.

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

Per limitare la query ai soli oggetti `BookInstance` dell'utente corrente, viene reimplementato `get_queryset()` come mostrato sopra. Si noti che "o" è il codice memorizzato per "on loan" e che l'ordinamento viene effettuato in base alla data `due_back`, in modo che gli elementi più vecchi vengano visualizzati per primi.

### Configurazione URL per i libri in prestito

Ora aprire **/catalog/urls.py** e aggiungere un `path()` che punti alla vista precedente; è possibile semplicemente copiare il testo seguente alla fine del file.

```python
urlpatterns += [
    path('mybooks/', views.LoanedBooksByUserListView.as_view(), name='my-borrowed'),
]
```

### Template per i libri in prestito

Ora, per questa pagina, è sufficiente aggiungere un template. Per prima cosa, creare il file template **/catalog/templates/catalog/bookinstance_list_borrowed_user.html** e assegnargli il seguente contenuto:

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

Questo template è molto simile a quelli creati in precedenza per gli oggetti `Book` e `Author`.
L'unico elemento "nuovo" è che viene verificato il metodo aggiunto nel modello, `(bookinst.is_overdue)`, e utilizzato per cambiare il colore degli elementi scaduti.

Quando il server di sviluppo è in esecuzione, dovrebbe ora essere possibile visualizzare nel browser l'elenco per un utente autenticato all'indirizzo `http://127.0.0.1:8000/catalog/mybooks/`. Provare con l'utente autenticato e non autenticato; nel secondo caso, si dovrebbe essere reindirizzati alla pagina di login.

### Aggiungere l'elenco al riquadro laterale

L'ultimo passaggio consiste nell'aggiungere un link per questa nuova pagina nel riquadro laterale. Verrà inserito nella stessa sezione in cui vengono visualizzate altre informazioni per l'utente autenticato.

Aprire il template di base (**/django-locallibrary-tutorial/catalog/templates/base_generic.html**) e aggiungere la riga "My Borrowed" al riquadro laterale nella posizione mostrata di seguito.

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

Quando un utente qualsiasi effettua il login, vedrà il link _My Borrowed_ nel riquadro laterale e l'elenco dei libri visualizzato come di seguito. Il primo libro non ha una data di scadenza, il che è un bug che si spera di correggere in un tutorial successivo.

![Biblioteca - libri presi in prestito dall'utente](library_borrowed_by_user.png)

## Permessi

I permessi sono associati ai modelli e definiscono le operazioni che possono essere eseguite su un'istanza di modello da un utente che dispone del permesso. Per impostazione predefinita, Django assegna automaticamente i permessi _add_, _change_ e _delete_ a tutti i modelli; questi consentono agli utenti con i relativi permessi di eseguire le azioni associate tramite il sito di amministrazione. È possibile definire permessi personalizzati per i modelli e assegnarli a utenti specifici. È inoltre possibile modificare i permessi associati a diverse istanze dello stesso modello.

Il controllo dei permessi nelle viste e nei template è quindi molto simile al controllo dello stato di autenticazione e, in effetti, il controllo di un permesso verifica anche l'autenticazione.

### Modelli

La definizione dei permessi viene effettuata nella sezione `class Meta` del modello, usando il campo `permissions`.
È possibile specificare tutti i permessi necessari in una tupla; ciascun permesso è definito a sua volta in una tupla annidata contenente il nome del permesso e il valore di visualizzazione del permesso.
Ad esempio, potrebbe essere definito un permesso che consenta a un utente di contrassegnare un libro come restituito, come mostrato:

```python
class BookInstance(models.Model):
    # …
    class Meta:
        # …
        permissions = (("can_mark_returned", "Set book as returned"),)
```

Il permesso potrebbe quindi essere assegnato a un gruppo "Librarian" nel sito di amministrazione.

Aprire **catalog/models.py** e aggiungere il permesso come mostrato sopra. Sarà necessario eseguire nuovamente le migrazioni, chiamando `python3 manage.py makemigrations` e `python3 manage.py migrate`, per aggiornare adeguatamente il database.

### Template

I permessi dell'utente corrente vengono memorizzati in una variabile di template denominata `\{{ perms }}`. È possibile verificare se l'utente corrente dispone di un particolare permesso usando il nome specifico della variabile all'interno dell'"app" Django associata. Ad esempio, `\{{ perms.catalog.can_mark_returned }}` sarà `True` se l'utente dispone di questo permesso e `False` altrimenti. In genere il permesso viene verificato usando il tag di template `{% if %}`, come mostrato:

```django
{% if perms.catalog.can_mark_returned %}
    <!-- We can mark a BookInstance as returned. -->
    <!-- Perhaps add code to link to a "book return" view here. -->
{% endif %}
```

### Viste

I permessi possono essere verificati in una vista funzione usando il decoratore `permission_required` oppure in una vista basata su classi usando `PermissionRequiredMixin`. Gli schemi sono gli stessi dell'autenticazione tramite login, anche se naturalmente potrebbe essere ragionevole aggiungere più permessi.

Decoratore per viste funzione:

```python
from django.contrib.auth.decorators import permission_required

@permission_required('catalog.can_mark_returned')
@permission_required('catalog.can_edit')
def my_view(request):
    # …
```

Un mixin che richiede un permesso per le viste basate su classi:

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
> Esiste una piccola differenza predefinita nel comportamento illustrato sopra. Per impostazione **predefinita**, per un utente autenticato che viola un permesso:
>
> - `@permission_required` reindirizza alla schermata di login (stato HTTP 302).
> - `PermissionRequiredMixin` restituisce 403 (stato HTTP Forbidden).
>
> Normalmente sarà desiderabile il comportamento di `PermissionRequiredMixin`: restituire 403 se un utente ha effettuato il login ma non dispone del permesso corretto. Per ottenere questo comportamento in una vista funzione, usare `@login_required` e `@permission_required` con `raise_exception=True`, come mostrato:
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

Qui _LocalLibrary_ non verrà aggiornato; forse nel prossimo tutorial.

## Mettiti alla prova

In precedenza in questo articolo è stato mostrato come creare una pagina per l'utente corrente che elenca i libri presi in prestito.
La sfida ora consiste nel creare una pagina simile, visibile solo ai bibliotecari, che visualizzi _tutti_ i libri presi in prestito e includa il nome di ciascun mutuatario.

Dovrebbe essere possibile seguire lo stesso schema dell'altra vista. La differenza principale è che sarà necessario limitare la vista solo ai bibliotecari. Questo potrebbe essere fatto in base al fatto che l'utente sia un membro del personale, con il decoratore funzione `staff_member_required` e la variabile di template `user.is_staff`, ma si consiglia invece di utilizzare il permesso `can_mark_returned` e `PermissionRequiredMixin`, come descritto nella sezione precedente.

> [!WARNING]
> Ricordare di non utilizzare il superutente per i test basati sui permessi: i controlli dei permessi restituiscono sempre true per i superutenti, anche se un permesso non è stato ancora definito. Creare invece un utente bibliotecario e aggiungergli la capacità richiesta.

Al termine, la pagina dovrebbe avere un aspetto simile allo screenshot seguente.

![Tutti i libri presi in prestito, limitato ai bibliotecari](library_borrowed_all.png)

## Riepilogo

Ottimo lavoro: è stato ora creato un sito web in cui i membri della biblioteca possono effettuare il login e visualizzare i propri contenuti, mentre i bibliotecari, con il permesso corretto, possono visualizzare tutti i libri prestati e i relativi mutuatari. Al momento vengono ancora solo visualizzati contenuti, ma gli stessi principi e tecniche vengono utilizzati quando si desidera iniziare a modificare e aggiungere dati.

Nel prossimo articolo verrà analizzato come usare i form Django per raccogliere l'input degli utenti e iniziare quindi a modificare alcuni dati memorizzati.

## Vedi anche

- [User authentication in Django](https://docs.djangoproject.com/en/5.0/topics/auth/) (documentazione Django)
- [Using the (default) Django authentication system](https://docs.djangoproject.com/en/5.0/topics/auth/default/) (documentazione Django)
- [Introduction to class-based views > Decorating class-based views](https://docs.djangoproject.com/en/5.0/topics/class-based-views/intro/#decorating-class-based-views) (documentazione Django)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Sessions", "Learn_web_development/Extensions/Server-side/Django/Forms", "Learn_web_development/Extensions/Server-side/Django")}}
