---
title: "Tutorial Django Parte 9: lavorare con i form"
short-title: "9: Form"
slug: Learn_web_development/Extensions/Server-side/Django/Forms
l10n:
  sourceCommit: f46a2540200b2aac78b86c48804f8da60f954c25
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Authentication", "Learn_web_development/Extensions/Server-side/Django/Testing", "Learn_web_development/Extensions/Server-side/Django")}}

In questo tutorial verrà mostrato come lavorare con i form HTML in Django e, in particolare, il modo più semplice per scrivere form per creare, aggiornare ed eliminare istanze di modello. Nell'ambito di questa dimostrazione, verrà esteso il sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website) affinché i bibliotecari possano rinnovare libri, nonché creare, aggiornare ed eliminare autori utilizzando form propri, anziché usare l'applicazione di amministrazione.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Completare tutti gli argomenti dei tutorial precedenti, incluso
        <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/Authentication">Tutorial Django Parte 8: autenticazione utente e autorizzazioni</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere come scrivere form per ottenere informazioni dagli utenti e aggiornare il database.
        Comprendere come le viste generiche di modifica basate su classi possano semplificare enormemente la creazione di form per lavorare con un singolo modello.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

Un [form HTML](/it/docs/Learn_web_development/Extensions/Forms) è un gruppo di uno o più campi/widget in una pagina web, che può essere usato per raccogliere informazioni dagli utenti e inviarle a un server. I form sono un meccanismo flessibile per raccogliere l'input degli utenti perché esistono widget adatti all'inserimento di molti tipi diversi di dati, inclusi caselle di testo, checkbox, radio button, selettori di data e così via. I form sono anche un modo relativamente sicuro per condividere dati con il server, perché consentono di inviare dati nelle richieste `POST` con protezione contro la falsificazione di richieste tra siti.

Sebbene finora non siano stati creati form in questo tutorial, sono già stati incontrati nel sito di amministrazione di Django — ad esempio, lo screenshot seguente mostra un form per modificare uno dei modelli [Book](/it/docs/Learn_web_development/Extensions/Server-side/Django/Models), composto da numerosi elenchi di selezione ed editor di testo.

![Sito di amministrazione - Aggiunta libro](admin_book_add.png)

Lavorare con i form può essere complicato! Gli sviluppatori devono scrivere l'HTML del form, convalidare e sanitizzare correttamente i dati inseriti sul server, e possibilmente anche nel browser, ripubblicare il form con messaggi di errore per informare gli utenti dei campi non validi, gestire i dati quando sono stati inviati correttamente e infine rispondere all'utente in qualche modo per indicare l'esito positivo. I _Django Forms_ semplificano molto tutti questi passaggi, fornendo un framework che consente di definire programmaticamente i form e i relativi campi e quindi di utilizzare questi oggetti sia per generare il codice HTML del form sia per gestire gran parte della convalida e dell'interazione utente.

In questo tutorial verranno mostrati alcuni dei modi per creare e utilizzare i form e, in particolare, come le viste generiche di modifica possano ridurre significativamente la quantità di lavoro necessaria per creare form che manipolano i modelli. Nel frattempo, verrà estesa l'applicazione _LocalLibrary_ aggiungendo un form che permetta ai bibliotecari di rinnovare i libri della biblioteca, e verranno create pagine per creare, modificare ed eliminare libri e autori, riproducendo una versione di base del form mostrato sopra per modificare i libri.

## Form HTML

Per prima cosa, una breve panoramica dei [form HTML](/it/docs/Learn_web_development/Extensions/Forms). Si consideri un semplice form HTML, con un singolo campo di testo per inserire il nome di una "squadra" e la relativa etichetta:

![Esempio di campo nome semplice in un form HTML](form_example_name_field.png)

Il form è definito in HTML come una raccolta di elementi all'interno dei tag `<form>…</form>`, contenente almeno un elemento `input` di `type="submit"`.

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

Sebbene qui ci sia un solo campo di testo per inserire il nome della squadra, un form _può_ avere qualsiasi numero di altri elementi di input e le relative etichette. L'attributo `type` del campo definisce il tipo di widget che verrà visualizzato. `name` e `id` del campo sono usati per identificarlo in JavaScript/CSS/HTML, mentre `value` definisce il valore iniziale del campo quando viene visualizzato per la prima volta. L'etichetta della squadra corrispondente viene specificata usando il tag `label` (vedere "Enter name" sopra), con un campo `for` che contiene il valore `id` dell'`input` associato.

L'input `submit` verrà visualizzato come pulsante per impostazione predefinita.
Può essere premuto per caricare sul server i dati di tutti gli altri elementi di input nel form, in questo caso solo il campo `team_name`.
Gli attributi del form definiscono il `method` HTTP usato per inviare i dati e la destinazione dei dati sul server (`action`):

- `action`: la risorsa/URL a cui devono essere inviati i dati per l'elaborazione quando il form viene inviato. Se non è impostato, oppure è impostato su una stringa vuota, il form verrà inviato nuovamente all'URL della pagina corrente.
- `method`: il metodo HTTP usato per inviare i dati: _post_ oppure _get_.
  - Il metodo `POST` dovrebbe essere sempre utilizzato se i dati produrranno una modifica al database del server, perché può essere reso più resistente agli attacchi di falsificazione di richieste tra siti.
  - Il metodo `GET` dovrebbe essere utilizzato soltanto per i form che non modificano dati utente, ad esempio un form di ricerca. È consigliato quando si desidera poter aggiungere l'URL ai segnalibri oppure condividerlo.

Il ruolo del server è innanzitutto eseguire il rendering dello stato iniziale del form, contenente campi vuoti oppure precompilati con valori iniziali. Dopo che l'utente ha premuto il pulsante di invio, il server riceverà i dati del form con i valori dal browser web e dovrà convalidare le informazioni. Se il form contiene dati non validi, il server dovrebbe visualizzare di nuovo il form, questa volta con i dati inseriti dall'utente nei campi "validi" e messaggi che descrivano il problema per i campi non validi. Una volta che il server riceve una richiesta con tutti i dati del form validi, può eseguire un'azione appropriata, ad esempio salvare i dati, restituire il risultato di una ricerca, caricare un file e così via, e quindi notificare l'utente.

Come si può immaginare, creare l'HTML, convalidare i dati restituiti, visualizzare nuovamente i dati inseriti con report degli errori se necessario ed eseguire l'operazione desiderata sui dati validi può richiedere parecchio impegno per essere eseguito correttamente. Django rende tutto ciò molto più semplice, eliminando parte del lavoro gravoso e del codice ripetitivo.

## Processo di gestione dei form in Django

La gestione dei form in Django utilizza tutte le stesse tecniche apprese nei tutorial precedenti, per visualizzare informazioni sui modelli: la vista riceve una richiesta, esegue tutte le azioni richieste incluso leggere i dati dai modelli, quindi genera e restituisce una pagina HTML, da un template a cui viene passato un _context_ contenente i dati da visualizzare. Ciò che rende le cose più complicate è che il server deve anche essere in grado di elaborare i dati forniti dall'utente e visualizzare nuovamente la pagina se sono presenti errori.

Di seguito viene mostrato un diagramma di flusso del modo in cui Django gestisce le richieste dei form, iniziando da una richiesta per una pagina contenente un form, mostrata in verde.

![Documento aggiornato sul processo di gestione dei form](form_handling_-_standard.png)

In base al diagramma precedente, le attività principali svolte dalla gestione dei form di Django sono:

1. Visualizzare il form predefinito la prima volta che viene richiesto dall'utente.
   - Il form può contenere campi vuoti se si sta creando un nuovo record oppure può essere precompilato con valori iniziali, ad esempio se si sta modificando un record o se sono disponibili utili valori iniziali predefiniti.
   - A questo punto il form è definito _unbound_, perché non è associato ad alcun dato inserito dall'utente, anche se può avere valori iniziali.

2. Ricevere dati da una richiesta di invio e associarli al form.
   - Associare i dati al form significa che i dati inseriti dall'utente e gli eventuali errori sono disponibili quando è necessario visualizzare nuovamente il form.

3. Pulire e convalidare i dati.
   - La pulizia dei dati esegue la sanitizzazione dei campi di input, ad esempio rimuovendo caratteri non validi che potrebbero essere usati per inviare contenuto dannoso al server, e li converte in tipi Python coerenti.
   - La convalida verifica che i valori siano appropriati per il campo, ad esempio che rientrino nell'intervallo di date corretto, che non siano troppo brevi o troppo lunghi e così via.

4. Se alcuni dati non sono validi, visualizzare nuovamente il form, questa volta con i valori popolati dall'utente e i messaggi di errore per i campi problematici.
5. Se tutti i dati sono validi, eseguire le azioni richieste, ad esempio salvare i dati, inviare un'email, restituire il risultato di una ricerca, caricare un file e così via.
6. Una volta completate tutte le azioni, reindirizzare l'utente a un'altra pagina.

Django fornisce numerosi strumenti e approcci per aiutare con le attività descritte sopra. Il più fondamentale è la classe `Form`, che semplifica sia la generazione dell'HTML del form sia la pulizia/convalida dei dati. Nella sezione successiva viene descritto il funzionamento dei form usando l'esempio pratico di una pagina che consente ai bibliotecari di rinnovare i libri.

> [!NOTE]
> Comprendere come viene usato `Form` sarà utile nella discussione delle classi del framework di form più "ad alto livello" di Django.

## Form di rinnovo libro con un Form e una vista funzione

Successivamente verrà aggiunta una pagina per consentire ai bibliotecari di rinnovare i libri presi in prestito. A questo scopo verrà creato un form che consente agli utenti di inserire un valore di data. Il campo verrà inizializzato con un valore di 3 settimane dalla data corrente, il normale periodo di prestito, e verrà aggiunta una convalida per assicurarsi che il bibliotecario non possa inserire una data nel passato o una data troppo lontana nel futuro. Quando viene inserita una data valida, verrà scritta nel campo `BookInstance.due_back` del record corrente.

L'esempio utilizzerà una vista basata su funzione e una classe `Form`. Le sezioni seguenti spiegano come funzionano i form e le modifiche necessarie nel progetto _LocalLibrary_ in corso.

### Form

La classe `Form` è il cuore del sistema di gestione dei form di Django. Specifica i campi del form, il relativo layout, i widget di visualizzazione, le etichette, i valori iniziali, i valori validi e, una volta convalidati, i messaggi di errore associati ai campi non validi. La classe fornisce anche metodi per eseguire il rendering di sé stessa nei template utilizzando formati predefiniti, tabelle, elenchi e così via, oppure per ottenere il valore di qualsiasi elemento, consentendo un rendering manuale dettagliato.

#### Dichiarazione di un Form

La sintassi di dichiarazione per un `Form` è molto simile a quella per dichiarare un `Model` e condivide gli stessi tipi di campo, nonché alcuni parametri simili. Questo ha senso perché in entrambi i casi è necessario assicurarsi che ogni campo gestisca i tipi corretti di dati, sia limitato a dati validi e abbia una descrizione per la visualizzazione/documentazione.

I dati dei form sono archiviati nel file forms.py di un'applicazione, all'interno della directory dell'applicazione. Creare e aprire il file **django-locallibrary-tutorial/catalog/forms.py**. Per creare un `Form`, importare la libreria `forms`, derivare dalla classe `Form` e dichiarare i campi del form. Di seguito è mostrata una classe di form molto basilare per il form di rinnovo del libro della biblioteca: aggiungerla al nuovo file.

```python
from django import forms

class RenewBookForm(forms.Form):
    renewal_date = forms.DateField(help_text="Enter a date between now and 4 weeks (default 3).")
```

#### Campi del form

In questo caso, è presente un singolo [`DateField`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#datefield) per inserire la data di rinnovo, che verrà renderizzato in HTML con un valore vuoto, l'etichetta predefinita "_Renewal date:_" e un utile testo di utilizzo: "_Enter a date between now and 4 weeks (default 3 weeks)._". Poiché non sono specificati altri argomenti facoltativi, il campo accetterà date utilizzando gli [input_formats](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#django.forms.DateField.input_formats): YYYY-MM-DD (2024-11-06), MM/DD/YYYY (02/26/2024), MM/DD/YY (10/25/24), e verrà renderizzato utilizzando il [widget](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#widget) predefinito: [DateInput](https://docs.djangoproject.com/en/5.0/ref/forms/widgets/#django.forms.DateInput).

Esistono molti altri tipi di campi form, che saranno in gran parte riconoscibili per la loro somiglianza con le classi di campo del modello equivalenti:

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

Gli argomenti comuni alla maggior parte dei campi sono elencati di seguito, e dispongono di valori predefiniti ragionevoli:

- [`required`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#required): se `True`, il campo non può essere lasciato vuoto né ricevere un valore `None`. I campi sono obbligatori per impostazione predefinita, quindi impostare `required=False` per consentire valori vuoti nel form.
- [`label`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#label): l'etichetta da usare quando viene eseguito il rendering del campo in HTML. Se non viene specificata una [label](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#label), Django ne creerà una dal nome del campo rendendo maiuscola la prima lettera e sostituendo gli underscore con spazi, ad esempio _Renewal date_.
- [`label_suffix`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#label-suffix): per impostazione predefinita, viene visualizzato un due punti dopo l'etichetta, ad esempio Renewal date&ZeroWidthSpace;**:**. Questo argomento consente di specificare un suffisso diverso contenente altri caratteri.
- [`initial`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#initial): il valore iniziale del campo quando il form viene visualizzato.
- [`widget`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#widget): il widget di visualizzazione da usare.
- [`help_text`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#help-text), come nell'esempio precedente: testo aggiuntivo che può essere visualizzato nei form per spiegare come usare il campo.
- [`error_messages`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#error-messages): un elenco di messaggi di errore per il campo. Se necessario, è possibile sostituirli con messaggi propri.
- [`validators`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#validators): un elenco di funzioni che verranno chiamate sul campo durante la convalida.
- [`localize`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#localize): abilita la localizzazione dell'input di dati del form; per ulteriori informazioni, vedere il link.
- [`disabled`](https://docs.djangoproject.com/en/5.0/ref/forms/fields/#disabled): il campo viene visualizzato, ma il relativo valore non può essere modificato se è `True`. Il valore predefinito è `False`.

#### Convalida

Django fornisce numerosi punti in cui è possibile convalidare i dati. Il modo più semplice per convalidare un singolo campo è sovrascrivere il metodo `clean_<field_name>()` per il campo da verificare. Ad esempio, è possibile convalidare che i valori `renewal_date` inseriti siano compresi tra oggi e 4 settimane implementando `clean_renewal_date()` come mostrato di seguito.

Aggiornare il file forms.py affinché abbia questo aspetto:

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

Ci sono due aspetti importanti da notare. Il primo è che i dati vengono ottenuti usando `self.cleaned_data['renewal_date']` e che questi dati vengono restituiti indipendentemente dal fatto che siano modificati o meno alla fine della funzione.
Questo passaggio restituisce dati "puliti" e sanitizzati da input potenzialmente non sicuro utilizzando i validator predefiniti, e convertiti nel tipo standard corretto per i dati, in questo caso un oggetto Python `datetime.datetime`.

Il secondo punto è che, se un valore non rientra nell'intervallo, viene sollevato un `ValidationError`, specificando il testo dell'errore da visualizzare nel form se viene inserito un valore non valido.
L'esempio precedente racchiude inoltre questo testo in una delle [funzioni di traduzione](https://docs.djangoproject.com/en/5.0/topics/i18n/translation/) di Django, `gettext_lazy()`, importata come `_()`, una buona pratica se si desidera tradurre il sito in seguito.

> [!NOTE]
> Esistono numerosi altri metodi ed esempi per convalidare i form in [Convalida di form e campi](https://docs.djangoproject.com/en/5.0/ref/forms/validation/) (documentazione Django). Ad esempio, nei casi in cui sono presenti più campi che dipendono l'uno dall'altro, è possibile sovrascrivere la funzione [Form.clean()](https://docs.djangoproject.com/en/5.0/ref/forms/api/#django.forms.Form.clean) e sollevare nuovamente un `ValidationError`.

Questo è tutto ciò che serve per il form di questo esempio.

### Configurazione URL

Prima di creare la vista, aggiungere una configurazione URL per la pagina _renew-books_. Copiare la seguente configurazione in fondo a **django-locallibrary-tutorial/catalog/urls.py**:

```python
urlpatterns += [
    path('book/<uuid:pk>/renew/', views.renew_book_librarian, name='renew-book-librarian'),
]
```

La configurazione URL reindirizzerà gli URL con il formato **/catalog/book/_\<bookinstance_id>_/renew/** alla funzione denominata `renew_book_librarian()` in **views.py** e invierà l'id di `BookInstance` come parametro denominato `pk`. Il pattern corrisponde soltanto se `pk` è un `uuid` formattato correttamente.

> [!NOTE]
> I dati URL acquisiti possono avere qualsiasi nome, perché esiste il controllo completo sulla funzione della vista, e non viene usata una classe di vista dettagli generica che prevede parametri con un determinato nome. Tuttavia, `pk`, abbreviazione di "primary key", è una convenzione ragionevole da utilizzare.

### Vista

Come discusso nel [processo di gestione dei form in Django](#processo_di_gestione_dei_form_in_django) sopra, la vista deve eseguire il rendering del form predefinito quando viene chiamata per la prima volta e quindi visualizzarlo nuovamente con messaggi di errore se i dati non sono validi, oppure elaborare i dati e reindirizzare a una nuova pagina se i dati sono validi. Per eseguire queste diverse azioni, la vista deve essere in grado di sapere se viene chiamata per la prima volta per il rendering del form predefinito o in un momento successivo per convalidare i dati.

Per i form che utilizzano una richiesta `POST` per inviare informazioni al server, il pattern più comune consiste nel verificare il tipo di richiesta `POST`, `if request.method == 'POST':`, per identificare le richieste di convalida del form e `GET`, utilizzando una condizione `else`, per identificare la richiesta iniziale di creazione del form. Se i dati vengono inviati usando una richiesta `GET`, un approccio tipico per identificare se si tratta della prima o di una successiva invocazione della vista consiste nel leggere i dati del form, ad esempio per leggere un valore nascosto nel form.

Il processo di rinnovo del libro scriverà nel database, quindi, per convenzione, viene usato l'approccio con richiesta `POST`.
Il frammento di codice seguente mostra il pattern, molto standard, per questo tipo di vista funzione.

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

Per prima cosa, vengono importati il form, `RenewBookForm`, e numerosi altri oggetti/metodi utili utilizzati nel corpo della funzione della vista:

- [`get_object_or_404()`](https://docs.djangoproject.com/en/5.0/topics/http/shortcuts/#get-object-or-404): restituisce un oggetto specificato da un modello in base al valore della sua chiave primaria e solleva un'eccezione `Http404`, non trovato, se il record non esiste.
- [`HttpResponseRedirect`](https://docs.djangoproject.com/en/5.0/ref/request-response/#django.http.HttpResponseRedirect): crea un reindirizzamento a un URL specificato, codice di stato HTTP 302.
- [`reverse()`](https://docs.djangoproject.com/en/5.0/ref/urlresolvers/#django.urls.reverse): genera un URL da un nome di configurazione URL e da un insieme di argomenti. È l'equivalente Python del tag `url` usato nei template.
- [`datetime`](https://docs.python.org/3/library/datetime.html): una libreria Python per manipolare date e orari.

Nella vista, viene dapprima utilizzato l'argomento `pk` in `get_object_or_404()` per ottenere il `BookInstance` corrente; se non esiste, la vista termina immediatamente e la pagina visualizza un errore "not found".
Se questa _non_ è una richiesta `POST`, gestita dalla clausola `else`, viene creato il form predefinito passando un valore `initial` per il campo `renewal_date`, a 3 settimane dalla data corrente.

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

Dopo aver creato il form, viene chiamato `render()` per creare la pagina HTML, specificando il template e un context che contiene il form. In questo caso, il context contiene anche il `BookInstance`, che verrà usato nel template per fornire informazioni sul libro che viene rinnovato.

Tuttavia, se questa è una richiesta `POST`, viene creato l'oggetto `form` e popolato con i dati della richiesta. Questo processo è denominato "binding" e permette di convalidare il form.

Viene quindi verificato se il form è valido, eseguendo tutto il codice di convalida su tutti i campi, incluso sia il codice generico per verificare che il campo data sia effettivamente una data valida, sia la funzione `clean_renewal_date()` del form specifico per verificare che la data sia nell'intervallo corretto.

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

Se il form non è valido, viene chiamato nuovamente `render()`, ma questa volta il valore del form passato nel context includerà i messaggi di errore.

Se il form è valido, è possibile iniziare a utilizzare i dati, accedendovi tramite l'attributo `form.cleaned_data`, ad esempio `data = form.cleaned_data['renewal_date']`. Qui, i dati vengono semplicemente salvati nel valore `due_back` dell'oggetto `BookInstance` associato.

> [!WARNING]
> Sebbene sia possibile accedere ai dati del form direttamente anche tramite la richiesta, ad esempio `request.POST['renewal_date']` oppure `request.GET['renewal_date']` quando si usa una richiesta GET, questo NON è consigliato. I dati puliti sono sanitizzati, convalidati e convertiti in tipi compatibili con Python.

Il passaggio finale nella parte di gestione del form della vista consiste nel reindirizzare a un'altra pagina, solitamente una pagina di "successo". In questo caso vengono usati `HttpResponseRedirect` e `reverse()` per reindirizzare alla vista denominata `'all-borrowed'`, creata come "sfida" in [Tutorial Django Parte 8: autenticazione utente e autorizzazioni](/it/docs/Learn_web_development/Extensions/Server-side/Django/Authentication#challenge_yourself). Se quella pagina non è stata creata, considerare il reindirizzamento alla pagina iniziale all'URL `/`.

Questo è tutto ciò che serve per la gestione del form, ma è ancora necessario limitare l'accesso alla vista ai soli bibliotecari autenticati che dispongono dell'autorizzazione per rinnovare i libri. Viene usato `@login_required` per richiedere l'autenticazione dell'utente e il decoratore di funzione `@permission_required` con l'autorizzazione esistente `can_mark_returned` per consentire l'accesso, poiché i decoratori vengono elaborati in ordine. Probabilmente sarebbe stata necessaria una nuova impostazione di autorizzazione in `BookInstance`, `can_renew`, ma verrà riutilizzata quella esistente per mantenere semplice l'esempio.

La vista finale è pertanto mostrata di seguito. Copiare questo codice in fondo a **django-locallibrary-tutorial/catalog/views.py**.

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

Creare il template a cui si fa riferimento nella vista, **/catalog/templates/catalog/book_renew_librarian.html**, e copiarvi il codice seguente:

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

La maggior parte di questo codice sarà completamente familiare dai tutorial precedenti.

Viene esteso il template di base e quindi ridefinito il blocco di contenuto. È possibile fare riferimento a `\{{ book_instance }}`, e alle sue variabili, poiché è stato passato nell'oggetto context nella funzione `render()`, e viene usato per elencare il titolo del libro, il mutuatario e la data di restituzione originale.

Il codice del form è relativamente semplice. Innanzitutto vengono dichiarati i tag `form`, specificando dove il form deve essere inviato, `action`, e il `method` per l'invio dei dati, in questo caso un `POST`. Come ricordato nella panoramica dei [form HTML](#form_html) nella parte superiore della pagina, un `action` vuoto, come quello mostrato, indica che i dati del form saranno inviati nuovamente all'URL corrente della pagina, che è ciò che serve. All'interno dei tag viene definito l'input `submit`, che l'utente può premere per inviare i dati. Il tag `{% csrf_token %}` aggiunto subito all'interno dei tag del form fa parte della protezione di Django contro la falsificazione tra siti.

> [!NOTE]
> Aggiungere `{% csrf_token %}` a ogni template Django creato che usa `POST` per inviare dati. Questo ridurrà la probabilità che i form vengano dirottati da utenti dannosi.

Rimane soltanto la variabile di template `\{{ form }}`, passata al template nel dizionario context.
Forse non sorprendentemente, quando viene utilizzata come mostrato fornisce il rendering predefinito di tutti i campi del form, comprese le etichette, i widget e il testo di aiuto; il rendering è mostrato di seguito:

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
> Potrebbe non essere evidente poiché è presente un solo campo, ma per impostazione predefinita ogni campo viene definito nella propria riga di tabella. Lo stesso rendering viene fornito facendo riferimento alla variabile di template `\{{ form.as_table }}`.

Inserendo una data non valida, verrebbe inoltre visualizzato un elenco degli errori renderizzati nella pagina, vedere `error-list` sotto.

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

#### Altri modi di utilizzare la variabile di template form

Usando `\{{ form.as_table }}` come mostrato sopra, ogni campo viene renderizzato come una riga di tabella. È anche possibile renderizzare ogni campo come elemento di elenco, usando `\{{ form.as_ul }}`, oppure come paragrafo, usando `\{{ form.as_p }}`.

È inoltre possibile avere il controllo completo sul rendering di ogni parte del form, indicizzando le sue proprietà con la notazione a punti. Ad esempio, è possibile accedere a numerosi elementi separati per il campo `renewal_date`:

- `\{{ form.renewal_date }}`: l'intero campo.
- `\{{ form.renewal_date.errors }}`: l'elenco degli errori.
- `\{{ form.renewal_date.id_for_label }}`: l'id dell'etichetta.
- `\{{ form.renewal_date.help_text }}`: il testo di aiuto del campo.

Per ulteriori esempi su come renderizzare manualmente i form nei template ed eseguire dinamicamente un ciclo sui campi del template, vedere [Lavorare con i form > Rendering manuale dei campi](https://docs.djangoproject.com/en/5.0/topics/forms/#rendering-fields-manually) (documentazione Django).

### Test della pagina

Se è stata accettata la "sfida" in [Tutorial Django Parte 8: autenticazione utente e autorizzazioni](/it/docs/Learn_web_development/Extensions/Server-side/Django/Authentication#challenge_yourself), sarà disponibile una vista che mostra tutti i libri in prestito nella biblioteca, visibile solo al personale della biblioteca.
La vista potrebbe assomigliare a questa:

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

È possibile aggiungere un link alla pagina di rinnovo del libro accanto a ciascun elemento aggiungendo il seguente codice di template al testo dell'elemento dell'elenco sopra.
Questo codice di template può essere eseguito soltanto all'interno del ciclo `{% for %}`, perché è qui che viene definito il valore `bookinst`.

```django
{% if perms.catalog.can_mark_returned %}- <a href="{% url 'renew-book-librarian' bookinst.id %}">Renew</a>{% endif %}
```

> [!NOTE]
> Ricordare che il login di test dovrà disporre dell'autorizzazione `catalog.can_mark_returned` per vedere il nuovo link "Renew" aggiunto sopra e per accedere alla pagina collegata; può essere utilizzato l'account superuser.

In alternativa, è possibile costruire manualmente un URL di test in questo modo: `http://127.0.0.1:8000/catalog/book/<bookinstance_id>/renew/`. Un `bookinstance_id` valido può essere ottenuto navigando alla pagina dei dettagli di un libro nella biblioteca e copiando il campo `id`.

### Come appare?

Se tutto ha avuto successo, il form predefinito apparirà così:

![Form predefinito che visualizza i dettagli del libro, la data di restituzione, la data di rinnovo e un pulsante di invio, nel caso in cui il link funzioni correttamente](forms_example_renew_default.png)

Il form con un valore non valido inserito apparirà così:

![Stesso form di sopra con un messaggio di errore: data non valida - rinnovo nel passato](forms_example_renew_invalid.png)

L'elenco di tutti i libri con i link per il rinnovo apparirà così:

![Visualizza l'elenco di tutti i libri rinnovati insieme ai relativi dettagli. Le scadenze superate sono in rosso.](forms_example_renew_allbooks.png)

## ModelForms

Creare una classe `Form` utilizzando l'approccio descritto sopra è molto flessibile, poiché consente di creare qualsiasi tipo di pagina di form e associarla a qualsiasi modello o modelli.

Tuttavia, se serve soltanto un form per mappare i campi di un _singolo_ modello, il modello definirà già la maggior parte delle informazioni necessarie nel form: campi, etichette, testo di aiuto e così via. Invece di ricreare le definizioni del modello nel form, è più semplice utilizzare la classe di supporto [ModelForm](https://docs.djangoproject.com/en/5.0/topics/forms/modelforms/) per creare il form dal modello. Questo `ModelForm` può quindi essere utilizzato nelle viste esattamente nello stesso modo di un normale `Form`.

Di seguito viene mostrato un `ModelForm` di base contenente lo stesso campo del `RenewBookForm` originale. Tutto ciò che serve per creare il form è aggiungere `class Meta` con il `model` associato, `BookInstance`, e un elenco dei `fields` del modello da includere nel form.

```python
from django.forms import ModelForm

from catalog.models import BookInstance

class RenewBookModelForm(ModelForm):
    class Meta:
        model = BookInstance
        fields = ['due_back']
```

> [!NOTE]
> È possibile includere tutti i campi nel form anche usando `fields = '__all__'`, oppure usare `exclude`, invece di `fields`, per specificare i campi da _non_ includere dal modello.
>
> Nessuno dei due approcci è consigliato, perché i nuovi campi aggiunti al modello vengono quindi inclusi automaticamente nel form, senza che lo sviluppatore consideri necessariamente le possibili implicazioni di sicurezza.

> [!NOTE]
> Questo potrebbe non sembrare molto più semplice del solo utilizzo di un `Form`, e non lo è in questo caso, perché è presente un solo campo. Tuttavia, se sono presenti molti campi, può ridurre considerevolmente la quantità di codice richiesta.

Il resto delle informazioni proviene dalle definizioni dei campi del modello, ad esempio etichette, widget, testo di aiuto e messaggi di errore. Se non sono del tutto corrette, possono essere sovrascritte in `class Meta`, specificando un dizionario contenente il campo da modificare e il suo nuovo valore. Ad esempio, in questo form potrebbe essere desiderata un'etichetta per il campo "_Renewal date_", anziché quella predefinita basata sul nome del campo, _Due Back_, e potrebbe essere necessario che il testo di aiuto sia specifico per questo caso d'uso.
Il `Meta` seguente mostra come sovrascrivere questi campi; analogamente, è possibile impostare `widgets` e `error_messages` se i valori predefiniti non sono sufficienti.

```python
class Meta:
    model = BookInstance
    fields = ['due_back']
    labels = {'due_back': _('New renewal date')}
    help_texts = {'due_back': _('Enter a date between now and 4 weeks (default 3).')}
```

Per aggiungere la convalida è possibile utilizzare lo stesso approccio di un normale `Form`: definire una funzione denominata `clean_<field_name>()` e sollevare eccezioni `ValidationError` per i valori non validi.
L'unica differenza rispetto al form originale è che il campo del modello si chiama `due_back` e non `renewal_date`.
Questa modifica è necessaria poiché il campo corrispondente in `BookInstance` si chiama `due_back`.

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

La classe `RenewBookModelForm` sopra è ora funzionalmente equivalente al `RenewBookForm` originale. Può essere importata e usata ovunque venga usato attualmente `RenewBookForm`, a condizione di aggiornare anche il nome della variabile di form corrispondente da `renewal_date` a `due_back`, come nella seconda dichiarazione del form: `RenewBookModelForm(initial={'due_back': proposed_renewal_date}`.

## Viste generiche di modifica

L'algoritmo di gestione dei form usato nell'esempio della vista funzione sopra rappresenta un pattern estremamente comune nelle viste di modifica dei form. Django astrae gran parte di questo "codice boilerplate" creando [viste generiche di modifica](https://docs.djangoproject.com/en/5.0/ref/class-based-views/generic-editing/) per creare, modificare ed eliminare viste basate su modelli. Queste non solo gestiscono il comportamento della "vista", ma creano automaticamente anche la classe del form, un `ModelForm`, a partire dal modello.

> [!NOTE]
> Oltre alle viste di modifica descritte qui, esiste anche una classe [FormView](https://docs.djangoproject.com/en/5.0/ref/class-based-views/generic-editing/#formview), che si colloca a metà tra la vista funzione e le altre viste generiche in termini di "flessibilità" rispetto allo "sforzo di scrittura del codice". Usando `FormView`, è ancora necessario creare il proprio `Form`, ma non è necessario implementare tutti i pattern standard di gestione dei form. È invece sufficiente fornire un'implementazione della funzione che verrà chiamata quando l'invio è noto come valido.

In questa sezione verranno usate le viste generiche di modifica per creare pagine che aggiungono funzionalità per creare, modificare ed eliminare record `Author` dalla biblioteca, fornendo di fatto una reimplementazione di base di parti del sito di amministrazione. Questo potrebbe essere utile quando è necessario offrire funzionalità di amministrazione in un modo più flessibile di quanto possa essere fornito dal sito di amministrazione.

### Viste

Aprire il file delle viste, **django-locallibrary-tutorial/catalog/views.py**, e aggiungere il seguente blocco di codice in fondo:

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

Come si può vedere, per creare, aggiornare o eliminare le viste è necessario derivare rispettivamente da `CreateView`, `UpdateView` e `DeleteView`, quindi definire il modello associato.
La chiamata a queste viste viene inoltre limitata soltanto agli utenti autenticati con le autorizzazioni `add_author`, `change_author` e `delete_author`, rispettivamente.

Per i casi di "creazione" e "aggiornamento" è inoltre necessario specificare i campi da visualizzare nel form, usando la stessa sintassi di `ModelForm`. In questo caso viene mostrato come elencarli singolarmente e la sintassi per elencare "tutti" i campi. È inoltre possibile specificare valori iniziali per ciascuno dei campi utilizzando un dizionario di coppie _field_name_/_value_. Qui viene impostata arbitrariamente la data di morte a scopo dimostrativo: potrebbe essere opportuno rimuoverla. Per impostazione predefinita, queste viste reindirizzeranno in caso di successo a una pagina che visualizza l'elemento del modello appena creato o modificato, che in questo caso sarà la vista dettagli dell'autore creata in un tutorial precedente. È possibile specificare una posizione di reindirizzamento alternativa dichiarando esplicitamente il parametro `success_url`.

La classe `AuthorDelete` non deve visualizzare alcuno dei campi, quindi non è necessario specificarli.
Viene inoltre impostato un `success_url`, come mostrato sopra, perché non esiste un URL predefinito ovvio a cui Django possa navigare dopo aver eliminato correttamente l'`Author`. Sopra viene usata la funzione [`reverse_lazy()`](https://docs.djangoproject.com/en/5.0/ref/urlresolvers/#reverse-lazy) per reindirizzare all'elenco degli autori dopo che un autore è stato eliminato. `reverse_lazy()` è una versione di `reverse()` eseguita in modo lazy, usata qui perché viene fornito un URL a un attributo di una vista basata su classi.

Se l'eliminazione degli autori dovesse sempre riuscire, questo sarebbe tutto.
Purtroppo l'eliminazione di un `Author` causerà un'eccezione se l'autore ha un libro associato, perché il [`modello Book`](/it/docs/Learn_web_development/Extensions/Server-side/Django/Models#book_model) specifica `on_delete=models.RESTRICT` per il campo `ForeignKey` dell'autore.
Per gestire questo caso, la vista sovrascrive il metodo [`form_valid()`](https://docs.djangoproject.com/en/5.0/ref/class-based-views/mixins-editing/#django.views.generic.edit.FormMixin.form_valid) in modo che, se l'eliminazione dell'`Author` riesce, reindirizzi a `success_url`; altrimenti, reindirizzi nuovamente allo stesso form.
Il template verrà aggiornato sotto per chiarire che non è possibile eliminare un'istanza `Author` utilizzata in un `Book`.

### Configurazioni URL

Aprire il file di configurazione URL, **django-locallibrary-tutorial/catalog/urls.py**, e aggiungere la seguente configurazione in fondo al file:

```python
urlpatterns += [
    path('author/create/', views.AuthorCreate.as_view(), name='author-create'),
    path('author/<int:pk>/update/', views.AuthorUpdate.as_view(), name='author-update'),
    path('author/<int:pk>/delete/', views.AuthorDelete.as_view(), name='author-delete'),
]
```

Non c'è nulla di particolarmente nuovo qui. Si può vedere che le viste sono classi e quindi devono essere chiamate tramite `.as_view()`, e i pattern URL in ciascun caso dovrebbero essere riconoscibili. È necessario usare `pk` come nome per il valore della chiave primaria acquisita, poiché questo è il nome del parametro previsto dalle classi di vista.

### Template

Le viste "create" e "update" usano lo stesso template per impostazione predefinita, che verrà denominato in base al modello: `model_name_form.html`. È possibile cambiare il suffisso in qualcosa di diverso da **\_form** utilizzando il campo `template_name_suffix` nella vista, ad esempio `template_name_suffix = '_other_suffix'`.

Creare il file template `django-locallibrary-tutorial/catalog/templates/catalog/author_form.html` e copiare il testo seguente.

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

È simile ai form precedenti e renderizza i campi usando una tabella. Si noti inoltre come venga nuovamente dichiarato `{% csrf_token %}` per garantire che i form siano resistenti agli attacchi CSRF.

La vista "delete" prevede di trovare un template denominato nel formato `[model_name]_confirm_delete.html`, e anche in questo caso è possibile modificare il suffisso usando `template_name_suffix` nella vista.
Creare il file template `django-locallibrary-tutorial/catalog/templates/catalog/author_confirm_delete.html` e copiare il testo seguente.

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
Innanzitutto verifica se l'autore è utilizzato in qualche libro e, in tal caso, visualizza l'elenco dei libri che devono essere eliminati prima che il record dell'autore possa essere eliminato.
In caso contrario, visualizza un form che chiede all'utente di confermare di voler eliminare il record dell'autore.

Il passaggio finale consiste nel collegare le pagine nella barra laterale.
Innanzitutto verrà aggiunto un link per creare l'autore nel _template di base_, affinché sia visibile in tutte le pagine per gli utenti autenticati considerati "staff" e che dispongono dell'autorizzazione per creare autori, `catalog.add_author`.
Aprire **/django-locallibrary-tutorial/catalog/templates/base_generic.html** e aggiungere le righe che consentono agli utenti con l'autorizzazione di creare l'autore, nello stesso blocco del link che mostra i libri "All Borrowed".
Ricordare di fare riferimento all'URL usando il suo nome `'author-create'`, come mostrato di seguito.

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

I link per aggiornare ed eliminare gli autori verranno aggiunti alla pagina dei dettagli dell'autore.
Aprire **catalog/templates/catalog/author_detail.html** e aggiungere il seguente codice:

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

Questo blocco sovrascrive il blocco `sidebar` nel template di base e quindi include il contenuto originale usando `\{{ block.super }}`.
Aggiunge quindi link per aggiornare o eliminare l'autore, ma solo quando l'utente dispone delle autorizzazioni corrette e il record dell'autore non è associato ad alcun libro.

Le pagine sono ora pronte per il test.

### Test della pagina

Per prima cosa, accedere al sito con un account che dispone delle autorizzazioni per aggiungere, modificare ed eliminare autori.

Navigare a una pagina qualsiasi e selezionare "Create author" nella barra laterale, con URL `http://127.0.0.1:8000/catalog/author/create/`.
La pagina dovrebbe apparire come nello screenshot seguente.

![Esempio di form: creazione autore](forms_example_create_author.png)

Inserire i valori per i campi e quindi premere **Submit** per salvare il record dell'autore.
Si dovrebbe ora essere indirizzati a una vista dettagli per il nuovo autore, con un URL simile a `http://127.0.0.1:8000/catalog/author/10`.

![Esempio di form: dettagli autore che mostrano i link Update e Delete](forms_example_detail_author_update.png)

È possibile testare la modifica del record selezionando il link "Update author", con un URL simile a `http://127.0.0.1:8000/catalog/author/10/update/`. Non viene mostrato uno screenshot perché appare esattamente come la pagina "create".

Infine, è possibile eliminare la pagina selezionando "Delete author" dalla barra laterale nella pagina dei dettagli.
Django dovrebbe visualizzare la pagina di eliminazione mostrata sotto se il record dell'autore non è usato in alcun libro.
Premere "**Yes, delete.**" per rimuovere il record ed essere indirizzati all'elenco di tutti gli autori.

![Form con opzione per eliminare l'autore](forms_example_delete_author.png)

## Mettiti alla prova

Creare alcuni form per creare, modificare ed eliminare record `Book`. È possibile usare esattamente la stessa struttura usata per gli `Authors`. Per l'eliminazione, ricordare che non è possibile eliminare un `Book` finché non vengono eliminati tutti i record `BookInstance` associati, e occorre usare le autorizzazioni corrette.
Se il template **book_form.html** è semplicemente una copia rinominata del template **author_form.html**, la nuova pagina "create book" apparirà come nello screenshot seguente:

![Screenshot che mostra vari campi nel form, come titolo, autore, riassunto, ISBN, genere e lingua](forms_example_create_book.png)

## Riepilogo

La creazione e la gestione dei form può essere un processo complicato. Django lo rende molto più semplice fornendo meccanismi programmatici per dichiarare, renderizzare e convalidare i form. Inoltre, Django fornisce viste generiche di modifica dei form in grado di svolgere _quasi tutto_ il lavoro necessario per definire pagine che possono creare, modificare ed eliminare record associati a una singola istanza di modello.

Con i form si può fare molto di più, consultare l'elenco [Vedere anche](#vedere_anche) qui sotto, ma a questo punto dovrebbe essere chiaro come aggiungere form di base e codice di gestione dei form ai propri siti web.

## Vedere anche

- [Lavorare con i form](https://docs.djangoproject.com/en/5.0/topics/forms/) (documentazione Django)
- [Scrivere la prima app Django, parte 4 > Scrivere un form semplice](https://docs.djangoproject.com/en/5.0/intro/tutorial04/#write-a-simple-form) (documentazione Django)
- [L'API Forms](https://docs.djangoproject.com/en/5.0/ref/forms/api/) (documentazione Django)
- [Campi form](https://docs.djangoproject.com/en/5.0/ref/forms/fields/) (documentazione Django)
- [Convalida di form e campi](https://docs.djangoproject.com/en/5.0/ref/forms/validation/) (documentazione Django)
- [Gestione dei form con viste basate su classi](https://docs.djangoproject.com/en/5.0/topics/class-based-views/generic-editing/) (documentazione Django)
- [Creazione di form dai modelli](https://docs.djangoproject.com/en/5.0/topics/forms/modelforms/) (documentazione Django)
- [Viste generiche di modifica](https://docs.djangoproject.com/en/5.0/ref/class-based-views/generic-editing/) (documentazione Django)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Authentication", "Learn_web_development/Extensions/Server-side/Django/Testing", "Learn_web_development/Extensions/Server-side/Django")}}
