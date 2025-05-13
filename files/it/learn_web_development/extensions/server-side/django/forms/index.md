---
title: "Tutoriale Django Parte 9: Lavorare con i moduli"
short-title: "9: Form"
slug: Learn_web_development/Extensions/Server-side/Django/Forms
l10n:
  sourceCommit: be1922d62a0d31e4e3441db0e943aed8df736481
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Sessions", "Learn_web_development/Extensions/Server-side/Django/Testing", "Learn_web_development/Extensions/Server-side/Django")}}

In questo tutorial, le mostreremo come lavorare con i moduli HTML in Django e, in particolare, il modo più semplice per scrivere moduli per creare, aggiornare e cancellare istanze di modelli. Come parte di questa dimostrazione, estenderemo il sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website) affinché i bibliotecari possano rinnovare libri e creare, aggiornare e cancellare autori utilizzando i nostri moduli (anziché utilizzare l'applicazione dell'amministratore).

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Completare tutti gli argomenti precedenti del tutorial, inclusi
        <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/Authentication">Tutorial di Django Parte 8: Autenticazione e permessi degli utenti</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere come scrivere moduli per ottenere informazioni dagli utenti e aggiornare il database.
        Comprendere come le viste di modifica basate su classi generiche possano semplificare notevolmente la creazione di moduli per lavorare con un singolo modello.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

Un [Modulo HTML](/it/docs/Learn_web_development/Extensions/Forms) è un gruppo di uno o più campi/widget su una pagina web, che possono essere utilizzati per raccogliere informazioni dagli utenti da inviare a un server. I moduli sono un meccanismo flessibile per raccogliere input dall'utente poiché ci sono widget adatti per l'inserimento di molti tipi di dati diversi, inclusi caselle di testo, checkbox, pulsanti di opzione, selezionatori di data e così via. I moduli sono anche un modo relativamente sicuro per condividere dati con il server, poiché ci permettono di inviare dati in richieste `POST` con protezione contro la contraffazione delle richieste tra siti.

Anche se non abbiamo ancora creato moduli in questo tutorial, li abbiamo già incontrati nel sito di amministrazione di Django — ad esempio, lo screenshot qui sotto mostra un modulo per la modifica di uno dei nostri modelli di [Book](/it/docs/Learn_web_development/Extensions/Server-side/Django/Models), composto da un numero di liste di selezione e editor di testo.

![Admin Site - Aggiungi Libro](admin_book_add.png)

Lavorare con i moduli può essere complicato! Gli sviluppatori devono scrivere HTML per il modulo, convalidare e sanificare correttamente i dati inseriti sul server (e possibilmente anche nel browser), ripubblicare il modulo con messaggi di errore per informare gli utenti di eventuali campi non validi, gestire i dati quando sono stati inviati con successo e infine rispondere all'utente in qualche modo per indicare il successo. I _Django Forms_ tolgono molto lavoro da tutti questi passaggi, fornendo un framework che le permette di definire i moduli e i loro campi in modo programmatico, e poi utilizzare questi oggetti per generare il codice HTML del modulo e gestire gran parte della validazione e dell'interazione con l'utente.

In questo tutorial, le mostreremo alcuni dei modi in cui può creare e lavorare con i moduli, e in particolare, come le viste di modifica generiche possono ridurre significativamente la quantità di lavoro necessaria per creare moduli per manipolare i suoi modelli. Lungo il percorso, estenderemo la nostra applicazione _LocalLibrary_ aggiungendo un modulo per consentire ai bibliotecari di rinnovare i libri della biblioteca, e creeremo pagine per creare, modificare e cancellare libri e autori (riproducendo una versione base del modulo mostrato sopra per la modifica dei libri).

## Moduli HTML

Prima, una breve panoramica dei [Moduli HTML](/it/docs/Learn_web_development/Extensions/Forms). Consideri un semplice modulo HTML, con un singolo campo di testo per inserire il nome di una "squadra", e la relativa etichetta:

![Esempio di campo nome semplice in modulo HTML](form_example_name_field.png)

Il modulo è definito in HTML come una raccolta di elementi all'interno dei tag `<form>…</form>`, contenente almeno un elemento `input` di `type="submit"`.

```html
<form action="/team_name_url/" method="post">
  <label for="team_name">Enter name: </label>
  <input
    id="team_name"
    type="text"
    name="name_field"
    value="Default name for team." />
  <input type="submit" value="OK" />
</form>
```

Mentre qui abbiamo appena un campo di testo per inserire il nome della squadra, un modulo _può_ avere un qualsiasi numero di altri elementi di input e le loro etichette associate. L'attributo `type` del campo definisce quale tipo di widget verrà visualizzato. I `name` e `id` del campo sono utilizzati per identificare il campo in JavaScript/CSS/HTML, mentre `value` definisce il valore iniziale per il campo quando viene visualizzato per la prima volta. L'etichetta di squadra corrispondente è specificata usando il tag `label` (vedi "Enter name" sopra), con un campo `for` contenente il valore `id` del `input` associato.

L'input `submit` verrà visualizzato di default come un pulsante.
Questo può essere premuto per caricare i dati in tutti gli altri elementi di input nel modulo al server (in questo caso, solo il campo `team_name`).
Gli attributi del modulo definiscono il `metodo` HTTP utilizzato per inviare i dati e la destinazione dei dati sul server (`action`):

- `action`: La risorsa/URL a cui i dati devono essere inviati per l'elaborazione quando il modulo viene inviato. Se questo non è impostato (o impostato su una stringa vuota), allora il modulo verrà inviato di nuovo all'URL corrente della pagina.
- `method`: Il metodo HTTP utilizzato per inviare i dati: _post_ o _get_.

  - Il metodo `POST` dovrebbe essere sempre utilizzato se i dati comporteranno una modifica al database del server, perché può essere reso più resistente agli attacchi di contraffazione delle richieste tra siti.
  - Il metodo `GET` dovrebbe essere utilizzato solo per moduli che non cambiano i dati dell'utente (ad esempio, un modulo di ricerca). È consigliato quando si vuole essere in grado di salvare o condividere l'URL.

Il ruolo del server è prima di tutto quello di eseguire il rendering dello stato iniziale del modulo - contenente campi vuoti o pre-popolati con valori iniziali. Dopo che l'utente preme il pulsante di invio, il server riceverà i dati del modulo con valori dal browser web e deve convalidare le informazioni. Se il modulo contiene dati non validi, il server dovrebbe mostrare di nuovo il modulo, questa volta con i dati inseriti dall'utente nei campi "validi" e messaggi che descrivono il problema per i campi non validi. Una volta che il server riceve una richiesta con tutti i dati del modulo validi, può eseguire un'azione appropriata (come: salvare i dati, restituire il risultato di una ricerca, caricare un file, ecc.) e quindi notificare l'utente.

Come può immaginare, creare l'HTML, convalidare i dati restituiti, ridisplay dei dati inseriti con report di errore se necessario, e eseguire l'operazione desiderata sui dati validi può richiedere parecchio sforzo per "fare bene". Django rende tutto questo molto più facile togliendo parte del lavoro pesante e il codice ripetitivo!

## Processo di gestione dei moduli in Django

La gestione dei moduli in Django utilizza tutte le tecniche che abbiamo imparato nei tutorial precedenti (per visualizzare informazioni sui nostri modelli): la vista riceve una richiesta, esegue qualsiasi azione richiesta inclusa la lettura dei dati dai modelli, quindi genera e restituisce una pagina HTML (da un template, in cui passiamo un _contesto_ contenente i dati da visualizzare). Ciò che rende le cose più complicate è che il server deve anche essere in grado di elaborare i dati forniti dall'utente e ridisplay della pagina se ci sono errori.

Un diagramma di flusso del processo di gestione dei moduli di Django è mostrato di seguito, partendo da una richiesta di una pagina contenente un modulo (mostrata in verde).

![Processo aggiornato di gestione dei moduli.](form_handling_-_standard.png)

Basandoci sul diagramma sopra, le cose principali che fa la gestione dei moduli di Django sono:

1. Visualizzare il modulo di default la prima volta che viene richiesto dall'utente.

   - Il modulo può contenere campi vuoti se sta creando un nuovo record, oppure può essere pre-popolato con valori iniziali (ad esempio, se sta cambiando un record, o ha valori iniziali preimpostati utili).
   - Il modulo è definito come _non collegato_ a questo punto, perché non è associato a nessun dato inserito dall'utente (anche se può avere valori iniziali).

2. Ricevere dati da una richiesta di invio e associarli al modulo.

   - Associare i dati al modulo significa che i dati inseriti dall'utente e eventuali errori sono disponibili quando dobbiamo ridisplay il modulo.

3. Pulire e convalidare i dati.

   - Pulire i dati esegue la sanificazione dei campi di input, come la rimozione di caratteri non validi che potrebbero essere utilizzati per inviare contenuti malevoli al server, e li converte in tipi Python consistenti.
   - La convalida controlla che i valori siano appropriati per il campo (ad esempio, che siano nel giusto intervallo di date, non troppo corti o troppo lunghi, ecc.)

4. Se alcuni dati sono non validi, ridisplay del modulo, questa volta con eventuali valori popolati dall'utente e messaggi di errore per i campi problematici.
5. Se tutti i dati sono validi, eseguire le azioni richieste (come salvare i dati, inviare un'email, restituire il risultato di una ricerca, caricare un file, e così via).
6. Una volta che tutte le azioni sono completate, reindirizzare l'utente a un'altra pagina.

Django fornisce una serie di strumenti e approcci per aiutarla con i compiti sopra dettagliati. Il più fondamentale è la classe `Form`, che semplifica sia la generazione del codice HTML del modulo sia la pulizia/validazione dei dati. Nella sezione successiva, descriviamo come funzionano i moduli usando l'esempio pratico di una pagina per consentire ai bibliotecari di rinnovare i libri.

> [!NOTE]
> Comprendere come viene utilizzato `Form` l'aiuterà quando discuteremo delle classi del framework di moduli più "ad alto livello" di Django.

## Modulo di rinnovo libro usando un Form e vista funzione

Successivamente, aggiungeremo una pagina per consentire ai bibliotecari di rinnovare i libri presi in prestito. Per farlo, creeremo un modulo che permetta agli utenti di inserire un valore di data. Popolermo il campo con un valore iniziale di 3 settimane dalla data corrente (il periodo di prestito normale), e aggiungeremo qualche valida convalida per garantire che il bibliotecario non possa inserire una data nel passato o una data troppo lontana nel futuro. Quando una data valida è stata inserita, la scriveremo nel campo `BookInstance.due_back` del record corrente.

L'esempio utilizzerà una vista basata su funzione e una classe `Form`. Le seguenti sezioni spiegano come funzionano i moduli e le modifiche che deve effettuare al nostro progetto _LocalLibrary_ in corso.

### Modulo

La classe `Form` è il cuore del sistema di gestione dei moduli di Django. Specifica i campi nel modulo, la loro disposizione, i widget di visualizzazione, le etichette, i valori iniziali, i valori validi e (una volta validati) i messaggi di errore associati ai campi non validi. La classe fornisce anche metodi per renderizzarsi nei template utilizzando formati predefiniti (tabelle, liste, ecc.) o per ottenere il valore di qualsiasi elemento (consentendo un rendering manuale dettagliato).

#### Dichiarare un Form

La sintassi di dichiarazione di un `Form` è molto simile a quella per dichiarare un `Model`, e condivide gli stessi tipi di campo (e alcuni parametri simili). Questo ha senso perché in entrambi i casi dobbiamo assicurarci che ogni campo gestisca i giusti tipi di dati, sia vincolato a dati validi e abbia una descrizione per la visualizzazione/documentazione.

I dati del modulo sono memorizzati nel file forms.py di un'applicazione, all'interno della directory dell'applicazione. Crei e apra il file **django-locallibrary-tutorial/catalog/forms.py**. Per creare un `Form`, importiamo la libreria `forms`, deriviamo dalla classe `Form` e dichiariamo i campi del modulo. Una classe di modulo molto basilare per il nostro modulo di rinnovo dei libri della biblioteca è mostrata di seguito — aggiunga questo al suo nuovo file:

```python
from django import forms

class RenewBookForm(forms.Form):
    renewal_date = forms.DateField(help_text="Enter a date between now and 4 weeks (default 3).")
```

#### Campi del modulo

In questo caso, abbiamo un singolo [`DateField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#datefield) per inserire la data di rinnovo che verrà renderizzato in HTML con un valore vuoto, l'etichetta di default "_Renewal date:_", e alcuni testi di aiuto: "_Enter a date between now and 4 weeks (default 3 weeks)._". Poiché nessuno degli altri argomenti opzionali è specificato, il campo accetterà date usando gli [input_formats](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#django.forms.DateField.input_formats): YYYY-MM-DD (2024-11-06), MM/DD/YYYY (02/26/2024), MM/DD/YY (10/25/24), e verrà renderizzato utilizzando il [widget](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#widget) di default: [DateInput](https://docs.djangoproject.com/en/5.0/ref/forms/widgets/#django.forms.DateInput).

Ci sono molti altri tipi di campi del modulo, che riconoscerà in gran parte per la loro somiglianza alle classi di campi di modelli equivalenti:

- [`BooleanField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#booleanfield)
- [`CharField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#charfield)
- [`ChoiceField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#choicefield)
- [`TypedChoiceField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#typedchoicefield)
- [`DateField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#datefield)
- [`DateTimeField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#datetimefield)
- [`DecimalField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#decimalfield)
- [`DurationField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#durationfield)
- [`EmailField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#emailfield)
- [`FileField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#filefield)
- [`FilePathField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#filepathfield)
- [`FloatField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#floatfield)
- [`ImageField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#imagefield)
- [`IntegerField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#integerfield)
- [`GenericIPAddressField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#genericipaddressfield)
- [`MultipleChoiceField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#multiplechoicefield)
- [`TypedMultipleChoiceField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#typedmultiplechoicefield)
- [`NullBooleanField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#nullbooleanfield)
- [`RegexField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#regexfield)
- [`SlugField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#slugfield)
- [`TimeField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#timefield)
- [`URLField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#urlfield)
- [`UUIDField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#uuidfield)
- [`ComboField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#combofield)
- [`MultiValueField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#multivaluefield)
- [`SplitDateTimeField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#splitdatetimefield)
- [`ModelMultipleChoiceField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#modelmultiplechoicefield)
- [`ModelChoiceField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#modelchoicefield)

Gli argomenti che sono comuni alla maggior parte dei campi sono elencati di seguito (questi hanno valori predefiniti sensati):

- [`required`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#required): Se `True`, il campo non può essere lasciato vuoto o avere un valore `None`. I campi sono richiesti per default, quindi imposterebbe `required=False` per consentire valori vuoti nel modulo.
- [`label`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#label): L'etichetta da usare quando si renderizza il campo in HTML. Se una [label](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#label) non è specificata, Django ne creerà una dal nome del campo capitalizzando la prima lettera e sostituendo gli underscore con spazi (e.g., "Data di rinnovo").
- [`label_suffix`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#label-suffix): Per default, viene visualizzato un due punti dopo l'etichetta (e.g., Data di rinnovo&ZeroWidthSpace;**:**). Questo argomento le permette di specificare un suffisso diverso contenente altri caratteri.
- [`initial`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#initial): Il valore iniziale per il campo quando il modulo viene visualizzato.
- [`widget`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#widget): Il widget di visualizzazione da usare.
- [`help_text`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#help-text) (come visto nell'esempio sopra): Testo aggiuntivo che può essere visualizzato nei moduli per spiegare come usare il campo.
- [`error_messages`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#error-messages): Una lista di messaggi di errore per il campo. Può sovrascrivere questi con i suoi messaggi se necessario.
- [`validators`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#validators): Una lista di funzioni che verranno chiamate sul campo al momento della validazione.
- [`localize`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#localize): Abilita la localizzazione degli input dei dati del modulo (per maggiori informazioni vedere il link).
- [`disabled`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#disabled): Il campo viene visualizzato ma il suo valore non può essere modificato se questo è `True`. Il valore predefinito è `False`.

#### Validazione

Django fornisce numerosi luoghi in cui può convalidare i suoi dati. Il modo più semplice per convalidare un singolo campo è sovrascrivere il metodo `clean_<field_name>()` per il campo che vuole controllare. Così, ad esempio, possiamo convalidare che i valori inseriti `renewal_date` siano compresi tra ora e 4 settimane implementando `clean_renewal_date()` come mostrato di seguito.

Aggiorni il suo file forms.py in modo che assomigli a questo:

```python
import datetime

from django import forms

from django.core.exceptions import ValidationError
from django.utils.translation import gettext_lazy as _

class RenewBookForm(forms.Form):
    renewal_date = forms.DateField(help_text="Enter a date between now and 4 weeks (default 3).")

    def clean_renewal_date(self):
        data = self.cleaned_data['renewal_date']

        # Check if a date is not in the past.
        if data < datetime.date.today():
            raise ValidationError(_('Invalid date - renewal in past'))

        # Check if a date is in the allowed range (+4 weeks from today).
        if data > datetime.date.today() + datetime.timedelta(weeks=4):
            raise ValidationError(_('Invalid date - renewal more than 4 weeks ahead'))

        # Remember to always return the cleaned data.
        return data
```

Ci sono due cose importanti da notare. La prima è che otteniamo i nostri dati usando `self.cleaned_data['renewal_date']` e che ritorniamo questi dati sia che li abbiamo modificati sia che no alla fine della funzione.
Questo passaggio ci fornisce i dati "puliti" e sanificati dall'input potenzialmente non sicuro usando i validatori di default, e convertiti nel tipo standard corretto per i dati (in questo caso un oggetto `datetime.datetime` di Python).

Il secondo punto è che se un valore cade al di fuori del nostro intervallo solleviamo un `ValidationError`, specificando il testo di errore che vogliamo visualizzare nel modulo se un valore non valido è inserito.
L'esempio sopra avvolge inoltre questo testo in una delle funzioni di [traduzione di Django](https://docs.djangoproject.com/en/5.0/topics/i18n/translation/), `gettext_lazy()` (importato come `_()`), che è una buona pratica se si vuole tradurre il suo sito in seguito.

> [!NOTE]
> Ci sono molti altri metodi ed esempi per la validazione di moduli nella [Validazione di moduli e campi](https://docs.djangoproject.com/en/5.0/ref/forms/validation/) (documentazione di Django). Ad esempio, nei casi in cui ha più campi che dipendono l'uno dall'altro, può sovrascrivere la funzione [Form.clean()](https://docs.djangoproject.com/en/5.0/ref/forms/api/#django.forms.Form.clean) e di nuovo sollevare un `ValidationError`.

Questo è tutto ciò di cui abbiamo bisogno per il modulo in questo esempio!

### Configurazione dell'URL

Prima di creare la nostra vista, aggiungiamo una configurazione URL per la pagina di _rinnovo dei libri_. Copi la seguente configurazione in fondo a **django-locallibrary-tutorial/catalog/urls.py**:

```python
urlpatterns += [
    path('book/<uuid:pk>/renew/', views.renew_book_librarian, name='renew-book-librarian'),
]
```

La configurazione dell'URL reindirizzerà gli URL nel formato **/catalog/book/_\<bookinstance_id>_/renew/** alla funzione chiamata `renew_book_librarian()` in **views.py**, e invierà l'id `BookInstance` come parametro chiamato `pk`. Il pattern corrisponde solo se `pk` è un `uuid` formattato correttamente.

> [!NOTE]
> Possiamo chiamare i dati catturati dell'URL in qualsiasi modo scelga, poiché abbiamo il controllo totale sulla funzione vista (non stiamo usando una classe di vista dettaglio generica che si aspetta parametri con un certo nome). Tuttavia, `pk` abbreviazione di "primary key" è una convenzione ragionevole da usare!

### Vista

Come discusso nel [Processo di gestione dei moduli di Django](#processo_di_gestione_dei_moduli_in_django) sopra, la vista deve rendere il modulo di default quando viene chiamato per la prima volta e poi o renderla di nuovo con i messaggi di errore se i dati sono non validi, o elaborare i dati e reindirizzare a una nuova pagina se i dati sono validi. Per eseguire queste diverse azioni, la vista deve essere in grado di sapere se viene chiamata per la prima volta per rendere il modulo di default, o un tempo successivo per convalidare i dati.

Per i moduli che usano una richiesta `POST` per inviare informazioni al server, il pattern più comune è per la vista testare contro il tipo di richiesta `POST` (`if request.method == 'POST':`) per identificare le richieste di validazione del modulo e `GET` (usando una condizione `else`) per identificare la richiesta iniziale di creazione del modulo. Se vuole inviare i suoi dati usando una richiesta `GET`, allora un approccio tipico per identificare se questa è la prima o successiva invocazione della vista è leggere i dati del modulo (e.g., per leggere un valore nascosto nel modulo).

Il processo di rinnovo del libro scriverà al nostro database, quindi, per convenzione, usiamo l'approccio di richiesta `POST`.
Il frammento di codice sotto mostra il pattern (molto standard) per questo tipo di vista funzione.

```python
import datetime

from django.shortcuts import render, get_object_or_404
from django.http import HttpResponseRedirect
from django.urls import reverse

from catalog.forms import RenewBookForm

def renew_book_librarian(request, pk):
    book_instance = get_object_or_404(BookInstance, pk=pk)

    # If this is a POST request then process the Form data
    if request.method == 'POST':

        # Create a form instance and populate it with data from the request (binding):
        form = RenewBookForm(request.POST)

        # Check if the form is valid:
        if form.is_valid():
            # process the data in form.cleaned_data as required (here we just write it to the model due_back field)
            book_instance.due_back = form.cleaned_data['renewal_date']
            book_instance.save()

            # redirect to a new URL:
            return HttpResponseRedirect(reverse('all-borrowed'))

    # If this is a GET (or any other method) create the default form.
    else:
        proposed_renewal_date = datetime.date.today() + datetime.timedelta(weeks=3)
        form = RenewBookForm(initial={'renewal_date': proposed_renewal_date})

    context = {
        'form': form,
        'book_instance': book_instance,
    }

    return render(request, 'catalog/book_renew_librarian.html', context)
```

Per prima cosa, importiamo il nostro modulo (`RenewBookForm`) e una serie di altri oggetti/metodi utili utilizzati nel corpo della funzione di vista:

- [`get_object_or_404()`](https://docs.djangoproject.com/en/5.0/topics/http/shortcuts/#get-object-or-404): Restituisce un oggetto specificato da un modello basato sul suo valore di chiave primaria, e solleva un'eccezione `Http404` (non trovato) se il record non esiste.
- [`HttpResponseRedirect`](https://docs.djangoproject.com/en/5.0/ref/request-response/#django.http.HttpResponseRedirect): Questo crea un redirect a un URL specificato (codice stato HTTP 302).
- [`reverse()`](https://docs.djangoproject.com/en/5.0/ref/urlresolvers/#django.urls.reverse): Questo genera un URL da un nome di configurazione URL e un insieme di argomenti. È l'equivalente Python del tag `url` che abbiamo usato nei nostri template.
- [`datetime`](https://docs.python.org/3/library/datetime.html): Una libreria Python per la manipolazione di date e orari.

Nella vista, per prima cosa usiamo l'argomento `pk` in `get_object_or_404()` per ottenere l'attuale `BookInstance` (se questo non esiste, la vista uscirà immediatamente e la pagina mostrerà un errore "non trovato").
Se questo non è una richiesta `POST` (gestita dalla clausola `else`) allora creiamo il modulo di default passando un valore `initial` per il campo `renewal_date`, 3 settimane dalla data corrente.

```python
book_instance = get_object_or_404(BookInstance, pk=pk)

# If this is a GET (or any other method) create the default form
else:
    proposed_renewal_date = datetime.date.today() + datetime.timedelta(weeks=3)
    form = RenewBookForm(initial={'renewal_date': proposed_renewal_date})

context = {
    'form': form,
    'book_instance': book_instance,
}

return render(request, 'catalog/book_renew_librarian.html', context)
```

Dopo aver creato il modulo, chiamiamo `render()` per creare la pagina HTML, specificando il template e un contesto che contiene il nostro modulo. In questo caso, il contesto contiene anche il nostro `BookInstance`, che useremo nel template per fornire informazioni sul libro che stiamo rinnovando.

Tuttavia, se questa è una richiesta `POST`, allora creiamo il nostro oggetto `form` e lo popoliamo con i dati dalla richiesta. Questo processo si chiama "binding" e ci permette di convalidare il modulo.

Poi controlliamo se il modulo è valido, il che esegue tutto il codice di validazione su tutti i campi — inclusa sia il codice generico per controllare che il nostro campo data sia effettivamente una data valida sia la nostra funzione `clean_renewal_date()` specifica del modulo per controllare che la data sia nell'intervallo giusto.

```python
book_instance = get_object_or_404(BookInstance, pk=pk)

# If this is a POST request then process the Form data
if request.method == 'POST':

    # Create a form instance and populate it with data from the request (binding):
    form = RenewBookForm(request.POST)

    # Check if the form is valid:
    if form.is_valid():
        # process the data in form.cleaned_data as required (here we just write it to the model due_back field)
        book_instance.due_back = form.cleaned_data['renewal_date']
        book_instance.save()

        # redirect to a new URL:
        return HttpResponseRedirect(reverse('all-borrowed'))

context = {
    'form': form,
    'book_instance': book_instance,
}

return render(request, 'catalog/book_renew_librarian.html', context)
```

Se il modulo non è valido chiamiamo di nuovo `render()`, ma questa volta il valore del modulo passato nel contesto includerà i messaggi di errore.

Se il modulo è valido, quindi possiamo iniziare a utilizzare i dati, accedendovi attraverso l'attributo `form.cleaned_data` (ad esempio, `data = form.cleaned_data['renewal_date']`). Qui, semplicemente salviamo i dati nel valore `due_back` dell'oggetto `BookInstance` associato.

> [!WARNING]
> Anche se è possibile accedere ai dati del modulo direttamente attraverso la richiesta (ad esempio, `request.POST['renewal_date']` o `request.GET['renewal_date']` se si utilizza una richiesta GET), questo NON è raccomandato. I dati puliti sono sanificati, convalidati e convertiti in tipi python-friendly.

L'ultimo passaggio nella parte di gestione del modulo della vista è reindirizzare a un'altra pagina, solitamente una pagina di "successo". In questo caso, usiamo `HttpResponseRedirect` e `reverse()` per reindirizzare alla vista denominata `'all-borrowed'` (questa è stata creata come la "sfida" in [Tutoriale Django Parte 8: Autenticazione e permessi degli utenti](/it/docs/Learn_web_development/Extensions/Server-side/Django/Authentication#challenge_yourself)). Se non ha creato quella pagina, consideri di reindirizzare alla home page all'URL `/`).

Questo è tutto ciò che serve per la gestione del modulo, ma dobbiamo ancora limitare l'accesso alla vista solo agli utenti loggati che hanno il permesso di rinnovare i libri. Usiamo `@login_required` per richiedere che l'utente sia loggato e il decoratore di funzione `@permission_required` con il nostro permesso esistente `can_mark_returned` per consentire l'accesso (i decoratori vengono processati in ordine). Si noti che probabilmente avremmo dovuto creare un'impostazione di permesso nuova in `BookInstance` (`can_renew`), ma riutilizzeremo quella esistente per mantenere l'esempio semplice.

La vista finale è quindi come mostrato di seguito. Copi per favore questo nel fondo di **django-locallibrary-tutorial/catalog/views.py**.

```python
import datetime

from django.contrib.auth.decorators import login_required, permission_required
from django.shortcuts import get_object_or_404
from django.http import HttpResponseRedirect
from django.urls import reverse

from catalog.forms import RenewBookForm

@login_required
@permission_required('catalog.can_mark_returned', raise_exception=True)
def renew_book_librarian(request, pk):
    """View function for renewing a specific BookInstance by librarian."""
    book_instance = get_object_or_404(BookInstance, pk=pk)

    # If this is a POST request then process the Form data
    if request.method == 'POST':

        # Create a form instance and populate it with data from the request (binding):
        form = RenewBookForm(request.POST)

        # Check if the form is valid:
        if form.is_valid():
            # process the data in form.cleaned_data as required (here we just write it to the model due_back field)
            book_instance.due_back = form.cleaned_data['renewal_date']
            book_instance.save()

            # redirect to a new URL:
            return HttpResponseRedirect(reverse('all-borrowed'))

    # If this is a GET (or any other method) create the default form.
    else:
        proposed_renewal_date = datetime.date.today() + datetime.timedelta(weeks=3)
        form = RenewBookForm(initial={'renewal_date': proposed_renewal_date})

    context = {
        'form': form,
        'book_instance': book_instance,
    }

    return render(request, 'catalog/book_renew_librarian.html', context)
```

### Il template

Crei il template referenziato nella vista (**/catalog/templates/catalog/book_renew_librarian.html**) e copi il codice qui sotto dentro:

```django
{% extends "base_generic.html" %}

{% block content %}
  <h1>Renew: \{{ book_instance.book.title }}</h1>
  <p>Borrower: \{{ book_instance.borrower }}</p>
  <p {% if book_instance.is_overdue %} class="text-danger"{% endif %} >Due date: \{{ book_instance.due_back }}</p>

  <form action="" method="post">
    {% csrf_token %}
    <table>
    \{{ form.as_table }}
    </table>
    <input type="submit" value="Submit">
  </form>
{% endblock %}
```

La maggior parte di esso le sarà completamente familiare dai tutorial precedenti.

Estendiamo il template base e quindi ridefiniamo il blocco di contenuto. Siamo in grado di fare riferimento a `\{{ book_instance }}` (e le sue variabili) perché è stato passato nel contesto oggetto nella funzione `render()`, e li usiamo per elencare il titolo del libro, il prestatario e la data di scadenza originale.

Il codice del modulo è relativamente semplice. Prima di tutto, dichiariamo i tag `form`, specificando dove il modulo deve essere inviato (`action`) e il `metodo` per inviare i dati (in questo caso un `POST`) — se ricorda la panoramica dei [Moduli HTML](#moduli_html) all'inizio della pagina, un `action` vuoto come mostrato, significa che i dati del modulo verranno postati di nuovo all'URL corrente della pagina (che è ciò che vogliamo). All'interno dei tag, definiamo l'input `submit`, che un utente può premere per inviare i dati. Il `{% csrf_token %}` aggiunto appena dentro i tag del modulo fa parte della protezione di Django contro la contraffazione delle richieste tra siti.

> [!NOTE]
> Aggiunga il `{% csrf_token %}` a ogni template Django che crea che usa `POST` per inviare dati. Questo ridurrà la possibilità che i moduli vengano dirottati da utenti malintenzionati.

Tutto ciò che resta è la variabile template `\{{ form }}`, che abbiamo passato al template nel dizionario contesto.
Forse non sorprende, quando usato come mostrato questo fornisce il rendering di default di tutti i campi del modulo, incluse le loro etichette, widget e testi di aiuto — il rendering è come mostrato di seguito:

```html
<tr>
  <th><label for="id_renewal_date">Renewal date:</label></th>
  <td>
    <input
      id="id_renewal_date"
      name="renewal_date"
      type="text"
      value="2023-11-08"
      required />
    <br />
    <span class="helptext">
      Enter date between now and 4 weeks (default 3 weeks).
    </span>
  </td>
</tr>
```

> [!NOTE]
> Forse non è ovvio perché abbiamo solo un campo, ma, di default, ogni campo è definito nella propria riga della tabella. Questo stesso rendering è fornito se si fa riferimento alla variabile del template `\{{ form.as_table }}`.

Se dovesse inserire una data non valida, otterrebbe anche un elenco degli errori renderizzati sulla pagina (vedi `error-list` sotto).

```html
<tr>
  <th><label for="id_renewal_date">Renewal date:</label></th>
  <td>
    <ul class="error-list">
      <li>Invalid date - renewal in past</li>
    </ul>
    <input
      id="id_renewal_date"
      name="renewal_date"
      type="text"
      value="2023-11-08"
      required />
    <br />
    <span class="helptext">
      Enter date between now and 4 weeks (default 3 weeks).
    </span>
  </td>
</tr>
```

#### Altri modi di utilizzare la variabile del template del modulo

Utilizzando `\{{ form.as_table }}` come mostrato sopra, ogni campo è renderizzato come una riga di tabella. Può anche renderizzare ogni campo come un elemento di lista (usando `\{{ form.as_ul }}`) o come un paragrafo (usando `\{{ form.as_p }}`).

È anche possibile avere il controllo totale sul rendering di ciascuna parte del modulo, indicizzando le sue proprietà usando la notazione punto. Quindi, per esempio, possiamo accedere a un certo numero di elementi separati per il nostro campo `renewal_date`:

- `\{{ form.renewal_date }}:` Il campo intero.
- `\{{ form.renewal_date.errors }}`: La lista degli errori.
- `\{{ form.renewal_date.id_for_label }}`: L'id dell'etichetta.
- `\{{ form.renewal_date.help_text }}`: Il testo di aiuto del campo.

Per ulteriori esempi di come renderizzare manualmente i moduli nei template e eseguire un loop dinamico sui campi del template, consulti [Working with forms > Rendering fields manually](https://docs.djangoproject.com/en/5.0/topics/forms/#rendering-fields-manually) (documentazione di Django).

### Testare la pagina

Se ha accettato la "sfida" in [Tutoriale Django Parte 8: Autenticazione e permessi degli utenti](/it/docs/Learn_web_development/Extensions/Server-side/Django/Authentication#challenge_yourself) avrà una vista che mostra tutti i libri in prestito nella biblioteca, che è visibile solo al personale della biblioteca.
La vista potrebbe apparire simile a questa:

```django
{% extends "base_generic.html" %}

{% block content %}
    <h1>All Borrowed Books</h1>

    {% if bookinstance_list %}
    <ul>

      {% for bookinst in bookinstance_list %}
      <li class="{% if bookinst.is_overdue %}text-danger{% endif %}">
        <a href="{% url 'book-detail' bookinst.book.pk %}">\{{ bookinst.book.title }}</a> (\{{ bookinst.due_back }}) {% if user.is_staff %}- \{{ bookinst.borrower }}{% endif %}
      </li>
      {% endfor %}
    </ul>

    {% else %}
      <p>There are no books borrowed.</p>
    {% endif %}
{% endblock %}
```

Possiamo aggiungere un collegamento alla pagina di rinnovo del libro accanto a ciascun elemento aggiungendo il codice del template seguente al testo dell'elemento dell'elenco sopra.
Si noti che questo codice del template può essere eseguito solo all'interno del loop `{% for %}`, perché è lì che viene definito il valore `bookinst`.

```django
{% if perms.catalog.can_mark_returned %}- <a href="{% url 'renew-book-librarian' bookinst.id %}">Renew</a>{% endif %}
```

> [!NOTE]
> Ricordi che il suo login di test dovrà avere il permesso `catalog.can_mark_returned` per vedere il nuovo collegamento "Rinnova" aggiunto sopra e per accedere alla pagina collegata (forse usi il suo account superuser).

Può anche costruire manualmente un URL di test come questo — `http://127.0.0.1:8000/catalog/book/<bookinstance_id>/renew/` (un `bookinstance_id` valido può essere ottenuto navigando a una pagina di dettaglio di un libro nella sua biblioteca, e copiando il campo `id`).

### Come appare?

Se ha successo, il modulo di default apparirà così:

![Modulo di default che mostra i dettagli del libro, data di scadenza, data di rinnovo e un pulsante invia appare nel caso in cui il link funzioni correttamente](forms_example_renew_default.png)

Il modulo con un valore non valido inserito apparirà così:

![Stesso modulo di cui sopra con un messaggio di errore: data non valida - rinnovo passato](forms_example_renew_invalid.png)

L'elenco di tutti i libri con collegamenti di rinnovo apparirà così:

![Mostra l'elenco di tutti i libri rinnovati insieme ai loro dettagli. Passato dovuto è in rosso.](forms_example_renew_allbooks.png)

## ModelForms

Creare una classe `Form` usando l'approccio descritto sopra è molto flessibile, permettendole di creare qualsiasi tipo di pagina di modulo le piaccia e associarla a un qualsiasi modello o modelli.

Tuttavia, se ha solo bisogno di un modulo per mappare i campi di un _singolo_ modello allora il suo modello definirà già la maggior parte delle informazioni che le servono nel suo modulo: campi, etichette, testo di aiuto e così via. Piuttosto che ricreare le definizioni del modello nel suo modulo, è più facile usare la classe helper [ModelForm](https://docs.djangoproject.com/en/5.0/topics/forms/modelforms/) per creare il modulo dal suo modello. Questo `ModelForm` può quindi essere usato all'interno delle sue viste nello stesso modo di un normale `Form`.

Un `ModelForm` di base contenente lo stesso campo del nostro originale `RenewBookForm` è mostrato di seguito. Tutto ciò che deve fare per creare il modulo è aggiungere `class Meta` con il `model` associato (`BookInstance`) e un elenco dei `fields` del modello da includere nel modulo.

```python
from django.forms import ModelForm

from catalog.models import BookInstance

class RenewBookModelForm(ModelForm):
    class Meta:
        model = BookInstance
        fields = ['due_back']
```

> [!NOTE]
> Può anche includere tutti i campi nel modulo usando `fields = '__all__'`, o può usare `exclude` (invece di `fields`) per specificare i campi _non_ da includere dal modello).
>
> Né l'uno né l'altro approccio sono raccomandati perché i nuovi campi aggiunti al modello vengono poi automaticamente inclusi nel modulo (senza che lo sviluppatore consideri necessariamente le possibili implicazioni di sicurezza).

> [!NOTE]
> Questo potrebbe non sembrare molto più semplice rispetto all'utilizzo di un `Form` (e non lo è in questo caso, poiché abbiamo solo un campo). Tuttavia, se ha molti campi, può ridurre notevolmente la quantità di codice richiesta!

Il resto delle informazioni viene dalle definizioni del campo del modello (e.g., etichette, widget, testo di aiuto, messaggi di errore). Se questi non sono proprio corretti, allora possiamo sovrascriverli nel nostro `class Meta`, specificando un dizionario contenente il campo da cambiare e il suo nuovo valore. Ad esempio, in questo modulo, potremmo volere un'etichetta per il nostro campo "_Data di rinnovo_" (piuttosto che il default basato sul nome del campo: _Due Back_), e vogliamo anche che il nostro testo di aiuto sia specifico per questo caso d'uso.
Il `Meta` qui sotto le mostra come sovrascrivere questi campi, e può allo stesso modo impostare i `widgets` e i `error_messages` se i default non sono sufficienti.

```python
class Meta:
    model = BookInstance
    fields = ['due_back']
    labels = {'due_back': _('New renewal date')}
    help_texts = {'due_back': _('Enter a date between now and 4 weeks (default 3).')}
```

Per aggiungere la validazione può usare lo stesso approccio che per un normale `Form` — definisce una funzione chiamata `clean_<field_name>()` e solleva eccezioni `ValidationError` per i valori non validi.
L'unica differenza rispetto al nostro modulo originale è che il campo del modello è chiamato `due_back` e non `renewal_date`.
Questo cambiamento è necessario poiché il campo corrispondente in `BookInstance` si chiama `due_back`.

```python
from django.forms import ModelForm

from catalog.models import BookInstance

class RenewBookModelForm(ModelForm):
    def clean_due_back(self):
       data = self.cleaned_data['due_back']

       # Check if a date is not in the past.
       if data < datetime.date.today():
           raise ValidationError(_('Invalid date - renewal in past'))

       # Check if a date is in the allowed range (+4 weeks from today).
       if data > datetime.date.today() + datetime.timedelta(weeks=4):
           raise ValidationError(_('Invalid date - renewal more than 4 weeks ahead'))

       # Remember to always return the cleaned data.
       return data

    class Meta:
        model = BookInstance
        fields = ['due_back']
        labels = {'due_back': _('Renewal date')}
        help_texts = {'due_back': _('Enter a date between now and 4 weeks (default 3).')}
```

La classe `RenewBookModelForm` sopra ora è funzionalmente equivalente al nostro modulo originale `RenewBookForm`. Potrebbe importarla e usarla ovunque lei attualmente usi `RenewBookForm` purché aggiorni anche il nome della variabile del modulo corrispondente da `renewal_date` a `due_back` come nella seconda dichiarazione del modulo: `RenewBookModelForm(initial={'due_back': proposed_renewal_date}`.

## Viste di modifica generiche

L'algoritmo di gestione del modulo che abbiamo utilizzato nel nostro esempio di vista funzione sopra rappresenta un modello estremamente comune nelle viste di modifica del modulo. Django astrae gran parte di questo "boilerplate" per lei, creando [viste di modifica generiche](https://docs.djangoproject.com/en/5.0/ref/class-based-views/generic-editing/) per creare, modificare e cancellare le viste basate sui modelli. Non solo queste gestiscono il comportamento "vista", ma creano automaticamente la classe modulo (un `ModelForm`) per lei dal modello.

> [!NOTE]
> In aggiunta alle viste di modifica descritte qui, c'è anche una classe [FormView](https://docs.djangoproject.com/en/5.0/ref/class-based-views/generic-editing/#formview), che si posiziona da qualche parte tra la nostra vista funzione e le altre viste generiche in termini di "flessibilità" vs. "sforzo di codifica". Utilizzando `FormView`, deve ancora creare il suo `Form`, ma non deve implementare tutti i pattern standard di gestione del modulo. Invece, deve solo fornire un'implementazione della funzione che verrà chiamata una volta che l'invio è noto per essere valido.

In questa sezione, useremo le viste di modifica generiche per creare pagine che aggiungono funzionalità per creare, modificare e cancellare record di `Author` dalla nostra biblioteca — fornendo effettivamente una reimplementazione di base di parti del sito dell'amministratore (questo potrebbe essere utile se ha bisogno di offrire la funzionalità amministrativa in un modo più flessibile di quello che può essere fornito dal sito dell'amministratore).

### Viste

Apro il file delle viste (**django-locallibrary-tutorial/catalog/views.py**) e aggiunga il seguente blocco di codice al fondo:

```python
from django.views.generic.edit import CreateView, UpdateView, DeleteView
from django.urls import reverse_lazy
from .models import Author

class AuthorCreate(PermissionRequiredMixin, CreateView):
    model = Author
    fields = ['first_name', 'last_name', 'date_of_birth', 'date_of_death']
    initial = {'date_of_death': '11/11/2023'}
    permission_required = 'catalog.add_author'

class AuthorUpdate(PermissionRequiredMixin, UpdateView):
    model = Author
    # Not recommended (potential security issue if more fields added)
    fields = '__all__'
    permission_required = 'catalog.change_author'

class AuthorDelete(PermissionRequiredMixin, DeleteView):
    model = Author
    success_url = reverse_lazy('authors')
    permission_required = 'catalog.delete_author'

    def form_valid(self, form):
        try:
            self.object.delete()
            return HttpResponseRedirect(self.success_url)
        except Exception as e:
            return HttpResponseRedirect(
                reverse("author-delete", kwargs={"pk": self.object.pk})
            )
```

Come può vedere, per creare, aggiornare o cancellare le viste deve derivare da `CreateView`, `UpdateView` e `DeleteView` (rispettivamente) e quindi definire il modello associato.
Restringiamo inoltre la chiamata a queste viste solo agli utenti loggati con i permessi `add_author`, `change_author`, e `delete_author`, rispettivamente.

Per i casi "creare" e "aggiornare" deve anche specificare i campi da visualizzare nel modulo (usando la stessa sintassi come per `ModelForm`). In questo caso, mostriamo come elencarli individualmente e la sintassi per elencare "tutti" i campi. Può anche specificare valori iniziali per ciascuno dei campi usando un dizionario di coppie _field_name_/_value_ (qui impostiamo arbitrariamente la data di morte a scopo dimostrativo — potrebbe volerlo rimuovere). Di default, queste viste reindirizzeranno al successo a una pagina che visualizza il nuovo/oggetto modello creato/modificato, che nel nostro caso sarà la vista dettaglio dell'autore che abbiamo creato in un tutorial precedente. Può specificare una posizione alternativa di reindirizzamento dichiarando esplicitamente il parametro `success_url`.

La classe `AuthorDelete` non ha bisogno di visualizzare nessuno dei campi, quindi questi non hanno bisogno di essere specificati.
Impostiamo inoltre un `success_url` (come mostrato sopra), perché non c'è un URL di default ovvio per Django a cui navigare dopo aver eliminato correttamente l'`Author`. Sopra usiamo la funzione [`reverse_lazy()`](https://docs.djangoproject.com/en/5.0/ref/urlresolvers/#reverse-lazy) per reindirizzare alla nostra lista degli autori dopo che un autore è stato cancellato — `reverse_lazy()` è una versione pigramente eseguita di `reverse()`, utilizzata qui perché stiamo fornendo un URL a un attributo della vista basata su classe.

Se l'eliminazione degli autori dovesse sempre avere successo questo sarebbe tutto.
Purtroppo eliminare un `Author` causerà un'eccezione se l'autore ha un libro associato, perché il nostro [`Book` modello](/it/docs/Learn_web_development/Extensions/Server-side/Django/Models#book_model) specifica `on_delete=models.RESTRICT` per il campo `ForeignKey` dell'autore.
Per gestire questo caso, la vista sovrascrive il metodo [`form_valid()`](https://docs.djangoproject.com/en/5.0/ref/class-based-views/mixins-editing/#django.views.generic.edit.FormMixin.form_valid) in modo che se l'eliminazione dell'`Author` riesce, reindirizzi al `success_url`, ma se no, semplicemente reindirizzi di nuovo allo stesso modulo.
Aggiorneremo il template sotto per chiarire che non può cancellare un'istanza `Author` che viene utilizzata in qualsiasi `Book`.

### Configurazioni degli URL

Apri il file di configurazione degli URL (**django-locallibrary-tutorial/catalog/urls.py**) e aggiungi la seguente configurazione al fondo del file:

```python
urlpatterns += [
    path('author/create/', views.AuthorCreate.as_view(), name='author-create'),
    path('author/<int:pk>/update/', views.AuthorUpdate.as_view(), name='author-update'),
    path('author/<int:pk>/delete/', views.AuthorDelete.as_view(), name='author-delete'),
]
```

Non c'è nulla di particolarmente nuovo qui! Può vedere che le viste sono classi, e quindi devono essere chiamate via `.as_view()`, e dovrebbe essere in grado di riconoscere i modelli di URL in ogni caso. Dobbiamo usare `pk` come nome per il nostro valore di chiave primaria catturata, poiché questo è il nome del parametro atteso dalle classi delle viste.

### Template

Le viste "create" e "update" usano lo stesso template per default, che verrà chiamato come il suo modello: `model_name_form.html` (può cambiare il suffisso in qualcosa di diverso da **\_form** usando il campo `template_name_suffix` nella sua vista, per esempio `template_name_suffix = '_other_suffix'`)

Crea il file del template `django-locallibrary-tutorial/catalog/templates/catalog/author_form.html` e copi il testo sotto.

```django
{% extends "base_generic.html" %}

{% block content %}
<form action="" method="post">
  {% csrf_token %}
  <table>
    \{{ form.as_table }}
  </table>
  <input type="submit" value="Submit" />
</form>
{% endblock %}
```

Questo è simile ai nostri moduli precedenti e renderizza i campi utilizzando una tabella. Nota anche come di nuovo dichiariamo il `{% csrf_token %}` per assicurarci che i nostri moduli siano resistenti agli attacchi CSRF.

La vista "delete" si aspetta di trovare un template nominato con il formato `[model_name]_confirm_delete.html` (di nuovo, è possibile cambiare il suffisso utilizzando `template_name_suffix` nella sua vista).
Crei il file del template `django-locallibrary-tutorial/catalog/templates/catalog/author_confirm_delete.html` e copi il testo sotto.

```django
{% extends "base_generic.html" %}

{% block content %}

<h1>Delete Author: \{{ author }}</h1>

{% if author.book_set.all %}

<p>You can't delete this author until all their books have been deleted:</p>
<ul>
  {% for book in author.book_set.all %}
    <li><a href="{% url 'book-detail' book.pk %}">\{{book}}</a> (\{{book.bookinstance_set.all.count}})</li>
  {% endfor %}
</ul>

{% else %}
<p>Are you sure you want to delete the author?</p>

<form action="" method="POST">
  {% csrf_token %}
  <input type="submit" action="" value="Yes, delete.">
</form>
{% endif %}

{% endblock %}
```

Il template dovrebbe essere familiare.
Controlla prima se l'autore è utilizzato in qualsiasi libro, e se è così, visualizza l'elenco dei libri che devono essere cancellati prima che il record dell'autore possa essere eliminato.
Se no, visualizza un modulo che chiede all'utente di confermare che vuole cancellare il record dell'autore.

L'ultimo passaggio è collegare le pagine nel menu laterale.
Prima aggiungeremo un collegamento per creare l'autore nel _template di base_, in modo che sia visibile in tutte le pagine per gli utenti loggati che sono considerati "staff" e che hanno il permesso di creare autori (`catalog.add_author`).
Apro **/django-locallibrary-tutorial/catalog/templates/base_generic.html** e aggiunga le righe che consentono agli utenti con il permesso di creare l'autore (nello stesso blocco del collegamento che mostra "Tutti i libri presi a prestito").
Ricordi di fare riferimento all'URL utilizzando il suo nome `'author-create'` come mostrato di seguito.

```django
{% if user.is_staff %}
<hr>
<ul class="sidebar-nav">
<li>Staff</li>
   <li><a href="{% url 'all-borrowed' %}">All borrowed</a></li>
{% if perms.catalog.add_author %}
   <li><a href="{% url 'author-create' %}">Create author</a></li>
{% endif %}
</ul>
{% endif %}
```

Aggiungeremo i collegamenti per aggiornare e cancellare autori alla pagina di dettaglio dell'autore.
Apri **catalog/templates/catalog/author_detail.html** e aggiungi il seguente codice:

```django
{% block sidebar %}
  \{{ block.super }}

  {% if perms.catalog.change_author or perms.catalog.delete_author %}
  <hr>
  <ul class="sidebar-nav">
    {% if perms.catalog.change_author %}
      <li><a href="{% url 'author-update' author.id %}">Update author</a></li>
    {% endif %}
    {% if not author.book_set.all and perms.catalog.delete_author %}
      <li><a href="{% url 'author-delete' author.id %}">Delete author</a></li>
    {% endif %}
    </ul>
  {% endif %}

{% endblock %}
```

Questo blocco sovrascrive il blocco `sidebar` nel template base e quindi incorpora il contenuto originale usando `\{{ block.super }}`.
Quindi aggiunge i collegamenti per aggiornare o cancellare l'autore, ma quando l'utente ha i corretti permessi e il record dell'autore non è associato a nessun libro.

Le pagine ora sono pronte per il test!

### Testare la pagina

Per prima cosa, effettui l'accesso al sito con un account che ha i permessi di aggiunta, modifica e cancellazione degli autori.

Navigare a qualsiasi pagina e selezionare "Creare autore" nel menu laterale (con URL `http://127.0.0.1:8000/catalog/author/create/`).
La pagina dovrebbe apparire come lo screenshot sotto.

![Esempio del modulo: Creare Autore](forms_example_create_author.png)

Immettere valori per i campi e quindi premere **Invia** per salvare il record dell'autore.
Dovrebbe ora essere portato a una vista di dettaglio per il suo nuovo autore, con un URL simile a `http://127.0.0.1:8000/catalog/author/10`.

![Esempio del modulo: Dettaglio Autore mostrando i collegamenti Aggiorna e Cancella](forms_example_detail_author_update.png)

Può testare la modifica del record selezionando il collegamento "Aggiorna autore" (con URL simile a `http://127.0.0.1:8000/catalog/author/10/update/`) — non mostriamo uno screenshot perché assomiglia proprio alla pagina "crea"!

Infine, possiamo cancellare la pagina selezionando "Cancellare autore" dal menu laterale nella pagina del dettaglio.
Django dovrebbe visualizzare la pagina di cancellazione mostrata sotto se il record dell'autore non è usato in nessun libro.
Premi "**Sì, cancellalo.**" per rimuovere il record e vieni portato alla lista di tutti gli autori.

![Modulo con opzione per cancellare autore](forms_example_delete_author.png)

## Sfidi se stesso

Crei alcuni moduli per creare, modificare e cancellare record di `Book`. Può usare esattamente la stessa struttura che per gli `Authors` (per la cancellazione, ricordi che non può cancellare un `Book` fino a quando tutti i suoi record di `BookInstance` associati non siano eliminati) e deve usare i permessi corretti.
Se il suo template **book_form.html** è solo una copia-rinominata della **author_form.html**, allora la nuova pagina "crea libro" apparirà come lo screenshot sotto:

![Screenshot visualizzando vari campi nel modulo come titolo, autore, riassunto, ISBN, genere e lingua](forms_example_create_book.png)

## Sommario

Creare e gestire moduli può essere un processo complicato! Django lo rende molto più semplice fornendo meccanismi programmabili per dichiarare, renderizzare e convalidare moduli. Inoltre, Django fornisce viste di modifica generiche che possono fare _quasi tutto_ il lavoro per definire pagine che possono creare, modificare e cancellare record associati a un'istanza di modello singola.

C'è molto di più che può essere fatto con i moduli (controlli il nostro [Vedere anche](#vedere_anche) elenco qui sotto), ma ora dovrebbe capire come aggiungere moduli di base e il codice di gestione dei moduli ai suoi propri siti web.

## Vedere anche

- [Working with forms](https://docs.djangoproject.com/en/5.0/topics/forms/) (documentazione di Django)
- [Writing your first Django app, part 4 > Writing a simple form](https://docs.djangoproject.com/en/5.0/intro/tutorial04/#write-a-simple-form) (documentazione di Django)
- [The Forms API](https://docs.djangoproject.com/en/5.0/ref/forms/api/) (documentazione di Django)
- [Form fields](https://docs.djangoproject.com/en/5.0/ref/forms/fields/) (documentazione di Django)
- [Form and field validation](https://docs.djangoproject.com/en/5.0/ref/forms/validation/) (documentazione di Django)
- [Form handling with class-based views](https://docs.djangoproject.com/en/5.0/topics/class-based-views/generic-editing/) (documentazione di Django)
- [Creating forms from models](https://docs.djangoproject.com/en/5.0/topics/forms/modelforms/) (documentazione di Django)
- [Generic editing views](https://docs.djangoproject.com/en/5.0/ref/class-based-views/generic-editing/) (documentazione di Django)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/authentication_and_sessions", "Learn_web_development/Extensions/Server-side/Django/Testing", "Learn_web_development/Extensions/Server-side/Django")}}
