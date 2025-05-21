---
title: "Tutorial Django Parte 10: Testare un'applicazione web Django"
short-title: "10: Testare"
slug: Learn_web_development/Extensions/Server-side/Django/Testing
l10n:
  sourceCommit: be1922d62a0d31e4e3441db0e943aed8df736481
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Forms", "Learn_web_development/Extensions/Server-side/Django/Deployment", "Learn_web_development/Extensions/Server-side/Django")}}

Man mano che i siti web crescono, diventano più difficili da testare manualmente. Non solo c'è più da testare, ma, poiché le interazioni tra i componenti diventano più complesse, un piccolo cambiamento in un'area può influire su altre, quindi saranno necessarie più modifiche per assicurarsi che tutto continui a funzionare e che non vengano introdotti errori mano a mano che si apportano altre modifiche. Un modo per mitigare questi problemi è scrivere test automatizzati, che possono essere eseguiti facilmente e in modo affidabile ogni volta che si apporta una modifica. Questo tutorial mostra come automatizzare il _test unitario_ del tuo sito web utilizzando il framework di test di Django.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Completare tutti gli argomenti dei tutorial precedenti, incluso <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/Forms">Django Tutorial Parte 9: Lavorare con i moduli</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Capire come scrivere test unitari per siti web basati su Django.</td>
    </tr>
  </tbody>
</table>

## Panoramica

La [Biblioteca Locale](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website) attualmente ha pagine per mostrare elenchi di tutti i libri e autori, viste dettagliate per gli elementi `Book` e `Author`, una pagina per rinnovare gli elementi `BookInstance`, e pagine per creare, aggiornare e eliminare elementi `Author` (e anche record di `Book` se hai completato la _sfida_ nel [tutorial sui moduli](/it/docs/Learn_web_development/Extensions/Server-side/Django/Forms)). Anche con questo sito relativamente piccolo, navigare manualmente su ogni pagina e controllare _superficialmente_ che tutto funzioni come previsto può richiedere diversi minuti. Man mano che apportiamo modifiche e facciamo crescere il sito, il tempo necessario per controllare manualmente che tutto funzioni "correttamente" aumenterà solo. Se continuassimo come stiamo facendo, alla fine passeremmo la maggior parte del tempo a testare, e pochissimo tempo a migliorare il nostro codice.

I test automatizzati possono davvero aiutare con questo problema! I benefici evidenti sono che possono essere eseguiti molto più velocemente dei test manuali, possono testare a un livello di dettaglio molto più basso, e testare esattamente la stessa funzionalità ogni volta (i tester umani non sono nemmeno lontanamente così affidabili!). Poiché sono veloci, i test automatizzati possono essere eseguiti più regolarmente, e se un test fallisce, indicano esattamente dove il codice non sta funzionando come previsto.

Inoltre, i test automatizzati possono agire come il primo "utente" reale del tuo codice, costringendoti a essere rigoroso nel definire e documentare come dovrebbe comportarsi il tuo sito web. Spesso sono alla base dei tuoi esempi di codice e documentazione. Per questi motivi, alcuni processi di sviluppo software iniziano con la definizione e l'implementazione dei test, dopodiché il codice è scritto per corrispondere al comportamento richiesto (ad esempio, lo sviluppo [basato sui test](https://en.wikipedia.org/wiki/Test-driven_development) e lo sviluppo [basato sul comportamento](https://en.wikipedia.org/wiki/Behavior-driven_development)).

Questo tutorial mostra come scrivere test automatizzati per Django, aggiungendo una serie di test al sito web _LocalLibrary_.

### Tipi di test

Esistono numerosi tipi, livelli e classificazioni di test e approcci di test. I test automatizzati più importanti sono:

- Test unitari
  - : Verificano il comportamento funzionale dei singoli componenti, spesso al livello di classe e funzione.
- Test di regressione
  - : Test che riproducono bug storici. Ogni test viene inizialmente eseguito per verificare che il bug sia stato corretto, e quindi eseguito nuovamente per garantire che non sia stato reintrodotto in seguito a modifiche successive al codice.
- Test di integrazione
  - : Verificano come funzionano i gruppi di componenti quando utilizzati insieme. I test di integrazione sono a conoscenza delle interazioni richieste tra i componenti, ma non necessariamente delle operazioni interne di ogni componente. Possono coprire semplici gruppi di componenti fino all'intero sito web.

> [!NOTE]
> Altri tipi comuni di test includono test a scatola nera, test a scatola bianca, test manuali, test automatizzati, test canarino, test di fumo, test di conformità, test di accettazione, test funzionali, test di sistema, test delle prestazioni, test di carico e test di stress. Cerca informazioni su di essi per maggiori dettagli.

### Cosa fornisce Django per i test?

Testare un sito web è un compito complesso, perché è costituito da diversi livelli di logica – dalla gestione delle richieste a livello HTTP, alle query dei modelli, alla validazione e elaborazione dei moduli, fino al rendering dei template.

Django fornisce un framework di test con una piccola gerarchia di classi che si basa sulla libreria standard [`unittest`](https://docs.python.org/3/library/unittest.html#module-unittest) di Python. Nonostante il nome, questo framework di test è adatto sia per i test unitari che per i test di integrazione. Il framework di Django aggiunge metodi API e strumenti per aiutare a testare il comportamento web e specifico di Django. Questi consentono di simulare richieste, inserire dati di test e ispezionare l'output dell'applicazione. Django fornisce anche un'API ([LiveServerTestCase](https://docs.djangoproject.com/en/5.0/topics/testing/tools/#liveservertestcase)) e strumenti per [utilizzare diversi framework di test](https://docs.djangoproject.com/en/5.0/topics/testing/advanced/#other-testing-frameworks), ad esempio è possibile integrare il popolare framework [Selenium](/it/docs/Learn_web_development/Extensions/Testing/Your_own_automation_environment) per simulare un utente che interagisce con un browser dal vivo.

Per scrivere un test, si deriva da una qualsiasi delle classi base di test di Django (o _unittest_) ([SimpleTestCase](https://docs.djangoproject.com/en/5.0/topics/testing/tools/#simpletestcase), [TransactionTestCase](https://docs.djangoproject.com/en/5.0/topics/testing/tools/#transactiontestcase), [TestCase](https://docs.djangoproject.com/en/5.0/topics/testing/tools/#testcase), [LiveServerTestCase](https://docs.djangoproject.com/en/5.0/topics/testing/tools/#liveservertestcase)) e quindi si scrivono metodi separati per verificare che funzionalità specifiche funzionino come previsto (i test utilizzano metodi "assert" per verificare che le espressioni risultino nei valori `True` o `False`, o che due valori siano uguali, ecc.) Quando si avvia un'esecuzione di test, il framework esegue i metodi di test scelti nelle classi derivate. I metodi di test vengono eseguiti indipendentemente, con comportamento comune di configurazione e/o smantellamento definito nella classe, come mostrato sotto.

```python
class YourTestClass(TestCase):
    def setUp(self):
        # Setup run before every test method.
        pass

    def tearDown(self):
        # Clean up run after every test method.
        pass

    def test_something_that_will_pass(self):
        self.assertFalse(False)

    def test_something_that_will_fail(self):
        self.assertTrue(False)
```

La migliore classe base per la maggior parte dei test è [django.test.TestCase](https://docs.djangoproject.com/en/5.0/topics/testing/tools/#testcase). Questa classe di test crea un database pulito prima che vengano eseguiti i suoi test e ogni funzione di test viene eseguita all'interno della propria transazione. La classe possiede anche un test [Client](https://docs.djangoproject.com/en/5.0/topics/testing/tools/#django.test.Client) che è possibile utilizzare per simulare un utente che interagisce con il codice a livello di vista. Nelle sezioni seguenti ci concentreremo sui test unitari, creati utilizzando questa classe base [TestCase](https://docs.djangoproject.com/en/5.0/topics/testing/tools/#testcase).

> [!NOTE]
> La classe [django.test.TestCase](https://docs.djangoproject.com/en/5.0/topics/testing/tools/#testcase) è molto comoda, ma può comportare che alcuni test siano più lenti di quanto necessario (non tutti i test avranno bisogno di impostare il proprio database o simulare l'interazione con la vista). Una volta che hai familiarità con cosa puoi fare con questa classe, potresti voler sostituire alcuni dei tuoi test con le classi di test più semplici disponibili.

### Cosa dovresti testare?

Dovresti testare tutti gli aspetti del tuo codice, ma non le librerie o le funzionalità fornite come parte di Python o Django.

Quindi, ad esempio, considera il modello `Author` definito qui sotto. Non è necessario testare esplicitamente che `first_name` e `last_name` siano stati memorizzati correttamente come `CharField` nel database perché ciò è qualcosa definito da Django (anche se, ovviamente, in pratica si testerà inevitabilmente questa funzionalità durante lo sviluppo). Né è necessario testare che `date_of_birth` sia stato validato come campo data, perché anche questo è qualcosa implementato in Django.

Tuttavia, dovresti controllare il testo usato per le etichette (_First name, Last name, Date of birth, Died_) e la dimensione del campo allocata per il testo (_100 caratteri_), perché questi fanno parte del tuo progetto e qualcosa che potrebbe essere rotto o cambiato in futuro.

```python
class Author(models.Model):
    first_name = models.CharField(max_length=100)
    last_name = models.CharField(max_length=100)
    date_of_birth = models.DateField(null=True, blank=True)
    date_of_death = models.DateField('Died', null=True, blank=True)

    def get_absolute_url(self):
        return reverse('author-detail', args=[str(self.id)])

    def __str__(self):
        return '%s, %s' % (self.last_name, self.first_name)
```

Allo stesso modo, dovresti verificare che i metodi personalizzati `get_absolute_url()` e `__str__()` si comportino come richiesto perché sono il tuo codice/logica aziendale. Nel caso di `get_absolute_url()`, puoi fidarti che il metodo Django `reverse()` sia stato implementato correttamente, quindi quello che stai testando è che la vista associata sia stata effettivamente definita.

> [!NOTE]
> I lettori più attenti potrebbero notare che vorremmo anche vincolare le date di nascita e morte a valori sensati e verificare che la morte avvenga dopo la nascita.
> In Django questo vincolo verrebbe aggiunto alle classi di modulo (sebbene sia possibile definire validatori per i campi del modello e validatori di modello, questi vengono utilizzati a livello di modulo solo se vengono chiamati dal metodo `clean()` del modello. Questo richiede un `ModelForm`, o il metodo `clean()` del modello deve essere specificamente chiamato.)

Tenendo presente ciò, iniziamo a esaminare come definire ed eseguire i test.

## Panoramica della struttura dei test

Prima di entrare nei dettagli di "cosa testare", diamo prima un'occhiata brevemente _dove_ e _come_ vengono definiti i test.

Django utilizza la [scoperta dei test incorporata](https://docs.python.org/3/library/unittest.html#unittest-test-discovery) del modulo unittest, che scoprirà i test nella directory di lavoro corrente in qualsiasi file denominato con il pattern **test\*.py**. A condizione di denominare correttamente i file, puoi utilizzare qualsiasi struttura desideri. Raccomandiamo di creare un modulo per il tuo codice di test e di avere file separati per modelli, viste, moduli e qualsiasi altro tipo di codice che devi testare. Ad esempio:

```plain
catalog/
  /tests/
    __init__.py
    test_models.py
    test_forms.py
    test_views.py
```

Crea una struttura di file come mostrato sopra nel tuo progetto _LocalLibrary_. Il file **\_\_init\_\_.py** dovrebbe essere un file vuoto (questo indica a Python che la directory è un pacchetto). È possibile creare i tre file di test copiando e rinominando il file di test scheletro **/catalog/tests.py**.

> [!NOTE]
> Il file di test scheletro **/catalog/tests.py** è stato creato automaticamente quando abbiamo [costruito il sito web scheletro di Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/skeleton_website). È perfettamente "legale" inserire tutti i test al suo interno, ma se testi correttamente, finirai rapidamente con un file di test molto grande e ingestibile.
>
> Elimina il file scheletro poiché non ne avremo bisogno.

Apri **/catalog/tests/test_models.py**. Il file dovrebbe importare `django.test.TestCase`, come mostrato:

```python
from django.test import TestCase

# Create your tests here.
```

Spesso aggiungerai una classe di test per ciascun modello/vista/modulo che desideri testare, con metodi individuali per testare funzionalità specifiche. In altri casi potresti voler avere una classe separata per testare un caso d'uso specifico, con funzioni di test individuali che testano aspetti di quel caso d'uso (ad esempio, una classe per testare che un campo del modello sia correttamente validato, con funzioni per testare ciascuno dei casi di fallimento possibili). Ancora una volta, la struttura è molto a tua discrezione, ma è meglio se sei coerente.

Aggiungi la classe di test qui sotto alla fine del file. La classe dimostra come costruire una classe di caso di test derivando da `TestCase`.

```python
class YourTestClass(TestCase):
    @classmethod
    def setUpTestData(cls):
        print("setUpTestData: Run once to set up non-modified data for all class methods.")
        pass

    def setUp(self):
        print("setUp: Run once for every test method to set up clean data.")
        pass

    def test_false_is_false(self):
        print("Method: test_false_is_false.")
        self.assertFalse(False)

    def test_false_is_true(self):
        print("Method: test_false_is_true.")
        self.assertTrue(False)

    def test_one_plus_one_equals_two(self):
        print("Method: test_one_plus_one_equals_two.")
        self.assertEqual(1 + 1, 2)
```

La nuova classe definisce due metodi che puoi utilizzare per la configurazione pre-test (ad esempio, per creare i modelli o altri oggetti di cui avrai bisogno per il test):

- `setUpTestData()` viene chiamato una volta all'inizio dell'esecuzione del test per la configurazione a livello di classe. Usalo per creare oggetti che non verranno modificati o modificati in nessuno dei metodi di test.
- `setUp()` viene chiamato prima di ogni funzione di test per impostare eventuali oggetti che potrebbero essere modificati dal test (ogni funzione di test otterrà una versione "fresca" di questi oggetti).

> [!NOTE]
> Le classi di test hanno anche un metodo `tearDown()` che non abbiamo usato. Questo metodo non è particolarmente utile per i test di database, poiché la classe base `TestCase` si occupa della rimozione del database per te.

Sotto questi abbiamo un certo numero di metodi di test, che utilizzano le funzioni `Assert` per testare se le condizioni sono vere, false o uguali (`AssertTrue`, `AssertFalse`, `AssertEqual`). Se la condizione non viene valutata come previsto, il test fallirà e segnalera l'errore alla tua console.

Le `AssertTrue`, `AssertFalse`, `AssertEqual` sono asserzioni standard fornite da **unittest**. Ci sono altre asserzioni standard nel framework e anche asserzioni [specifiche di Django](https://docs.djangoproject.com/en/5.0/topics/testing/tools/#assertions) per testare se una vista esegue un reindirizzamento (`assertRedirects`), testare se è stato utilizzato un particolare modello (`assertTemplateUsed`), ecc.

> [!NOTE]
> Di solito **non** dovresti includere le funzioni **print()** nei tuoi test come mostrato sopra. Lo facciamo qui solo in modo che tu possa vedere l'ordine in cui le funzioni di configurazione sono chiamate nella console (nella sezione seguente).

## Come eseguire i test

Il modo più semplice per eseguire tutti i test è utilizzare il comando:

```bash
python3 manage.py test
```

Questo scoprirà tutti i file denominati con il pattern **test\*.py** nella directory corrente ed eseguirà tutti i test definiti utilizzando le classi base appropriate (qui abbiamo un certo numero di file di test, ma solo **/catalog/tests/test_models.py** contiene attualmente dei test). Per impostazione predefinita, i test segnaleranno individualmente solo sui fallimenti dei test, seguiti da un riepilogo del test.

> [!NOTE]
> Se ottieni errori simili a: `ValueError: Missing staticfiles manifest entry...` ciò può essere dovuto al fatto che il test non esegue _collectstatic_ per impostazione predefinita, e la tua app sta usando una classe di archiviazione che lo richiede (vedi [manifest_strict](https://docs.djangoproject.com/en/5.0/ref/contrib/staticfiles/#django.contrib.staticfiles.storage.ManifestStaticFilesStorage.manifest_strict) per maggiori informazioni). Ci sono diversi modi per superare questo problema - il più semplice è eseguire _collectstatic_ prima di eseguire i test:
>
> ```bash
> python3 manage.py collectstatic
> ```

Esegui i test nella directory principale di _LocalLibrary_. Dovresti vedere un output simile a quello qui sotto.

```bash
> python3 manage.py test

Creating test database for alias 'default'...
setUpTestData: Run once to set up non-modified data for all class methods.
setUp: Run once for every test method to set up clean data.
Method: test_false_is_false.
setUp: Run once for every test method to set up clean data.
Method: test_false_is_true.
setUp: Run once for every test method to set up clean data.
Method: test_one_plus_one_equals_two.
.
======================================================================
FAIL: test_false_is_true (catalog.tests.tests_models.YourTestClass)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "D:\GitHub\django_tmp\library_w_t_2\locallibrary\catalog\tests\tests_models.py", line 22, in test_false_is_true
    self.assertTrue(False)
AssertionError: False is not true

----------------------------------------------------------------------
Ran 3 tests in 0.075s

FAILED (failures=1)
Destroying test database for alias 'default'...
```

Qui vediamo che abbiamo avuto un fallimento del test, e possiamo vedere esattamente quale funzione ha fallito e perché (questo fallimento è previsto, perché `False` non è `True`!).

> [!NOTE]
> La cosa più importante da apprendere dall'output del test sopra è che è molto più prezioso se usi nomi descrittivi/informativi per i tuoi oggetti e metodi.

L'output delle funzioni `print()` mostra come il metodo `setUpTestData()` venga chiamato una volta per la classe e `setUp()` venga chiamato prima di ogni metodo.
Ancora una volta, ricorda che normalmente non aggiungeresti questo tipo di `print()` ai tuoi test.

Le prossime sezioni mostrano come è possibile eseguire test specifici e come controllare la quantità di informazioni visualizzata dai test.

### Mostrare più informazioni sui test

Se vuoi ottenere più informazioni sull'esecuzione del test, puoi cambiare la _verbosità_. Ad esempio, per elencare anche i successi dei test oltre ai fallimenti (e un sacco di informazioni su come viene impostato il database di test), puoi impostare la verbosità su "2" come mostrato:

```bash
python3 manage.py test --verbosity 2
```

I livelli di verbosità consentiti sono 0, 1, 2 e 3, con il valore predefinito impostato su "1".

### Velocizzare le cose

Se i tuoi test sono indipendenti, su una macchina multiprocessore puoi accelerarli notevolmente eseguendoli in parallelo.
L'uso di `--parallel auto` sotto esegue un processo di test per ogni core disponibile.
Il `auto` è opzionale, ed è possibile specificare anche un numero particolare di core da utilizzare.

```bash
python3 manage.py test --parallel auto
```

Per ulteriori informazioni, inclusi cosa fare se i tuoi test non sono indipendenti, vedi [DJANGO_TEST_PROCESSES](https://docs.djangoproject.com/en/5.0/ref/django-admin/#envvar-DJANGO_TEST_PROCESSES).

### Eseguire test specifici

Se vuoi eseguire solo una parte dei tuoi test, puoi farlo specificando il percorso completo a punti per i pacchetti, il modulo, la sottoclasse `TestCase` o il metodo:

```bash
# Run the specified module
python3 manage.py test catalog.tests

# Run the specified module
python3 manage.py test catalog.tests.test_models

# Run the specified class
python3 manage.py test catalog.tests.test_models.YourTestClass

# Run the specified method
python3 manage.py test catalog.tests.test_models.YourTestClass.test_one_plus_one_equals_two
```

### Altre opzioni del runner di test

Il runner di test offre molte altre opzioni, inclusa la possibilità di mischiare i test (`--shuffle`), eseguirli in modalità debug (`--debug-mode`) e utilizzare il logger Python per catturare i risultati.
Per ulteriori informazioni, consulta la documentazione del [test runner](https://docs.djangoproject.com/en/5.0/ref/django-admin/#test) di Django.

## Test di LocalLibrary

Ora sappiamo come eseguire i nostri test e cosa dobbiamo testare, diamo un'occhiata ad alcuni esempi pratici.

> [!NOTE]
> Non scriveremo ogni possibile test, ma questo dovrebbe darti un'idea di come funzionano i test e cosa puoi fare di più.

### Modelli

Come discusso sopra, dovremmo testare tutto ciò che fa parte del nostro progetto o che è definito dal codice che abbiamo scritto, ma non librerie/codice che è già testato da Django o dal team di sviluppo di Python.

Ad esempio, considera il modello `Author` qui sotto. Dovremmo testare le etichette per tutti i campi, perché anche se non ne abbiamo esplicitamente specificati la maggior parte, abbiamo un progetto che dice quali dovrebbero essere questi valori. Se non testiamo i valori, non sapremo che le etichette dei campi hanno i loro valori previsti. Allo stesso modo, mentre ci fidiamo che Django crei un campo della lunghezza specificata, vale la pena specificare un test per questa lunghezza per assicurarsi che sia stato implementato come pianificato.

```python
class Author(models.Model):
    first_name = models.CharField(max_length=100)
    last_name = models.CharField(max_length=100)
    date_of_birth = models.DateField(null=True, blank=True)
    date_of_death = models.DateField('Died', null=True, blank=True)

    def get_absolute_url(self):
        return reverse('author-detail', args=[str(self.id)])

    def __str__(self):
        return f'{self.last_name}, {self.first_name}'
```

Apri il nostro **/catalog/tests/test_models.py**, e sostituisci qualsiasi codice esistente con il seguente codice di test per il modello `Author`.

Qui vedrai che prima importiamo `TestCase` e deriviamo la nostra classe di test (`AuthorModelTest`) da esso, usando un nome descrittivo in modo da poter identificare facilmente eventuali test non riusciti nell'output del test. Quindi chiamiamo `setUpTestData()` per creare un oggetto autore che utilizzeremo ma non modificheremo in nessuno dei test.

```python
from django.test import TestCase

from catalog.models import Author

class AuthorModelTest(TestCase):
    @classmethod
    def setUpTestData(cls):
        # Set up non-modified objects used by all test methods
        Author.objects.create(first_name='Big', last_name='Bob')

    def test_first_name_label(self):
        author = Author.objects.get(id=1)
        field_label = author._meta.get_field('first_name').verbose_name
        self.assertEqual(field_label, 'first name')

    def test_date_of_death_label(self):
        author = Author.objects.get(id=1)
        field_label = author._meta.get_field('date_of_death').verbose_name
        self.assertEqual(field_label, 'died')

    def test_first_name_max_length(self):
        author = Author.objects.get(id=1)
        max_length = author._meta.get_field('first_name').max_length
        self.assertEqual(max_length, 100)

    def test_object_name_is_last_name_comma_first_name(self):
        author = Author.objects.get(id=1)
        expected_object_name = f'{author.last_name}, {author.first_name}'
        self.assertEqual(str(author), expected_object_name)

    def test_get_absolute_url(self):
        author = Author.objects.get(id=1)
        # This will also fail if the URLConf is not defined.
        self.assertEqual(author.get_absolute_url(), '/catalog/author/1')
```

I test dei campi controllano che i valori delle etichette dei campi (`verbose_name`) e che la dimensione dei campi di testo siano come previsto. Questi metodi hanno tutti nomi descrittivi e seguono lo stesso schema:

```python
# Get an author object to test
author = Author.objects.get(id=1)

# Get the metadata for the required field and use it to query the required field data
field_label = author._meta.get_field('first_name').verbose_name

# Compare the value to the expected result
self.assertEqual(field_label, 'first name')
```

Le cose interessanti da notare sono:

- Non possiamo ottenere il `verbose_name` direttamente utilizzando `author.first_name.verbose_name`, perché `author.first_name` è una _stringa_ (non un handle all'oggetto `first_name` che possiamo usare per accedere alle sue proprietà). Invece, dobbiamo usare l'attributo `_meta` dell'autore per ottenere un'istanza del campo e usarla per interrogare le informazioni aggiuntive.
- Abbiamo scelto di usare `assertEqual(field_label,'first name')` piuttosto che `assertTrue(field_label == 'first name')`. Il motivo di questo è che se il test fallisce l'output per il primo ti dice quale era effettivamente l'etichetta, il che rende il debug del problema solo un po' più facile.

> [!NOTE]
> I test per le etichette `last_name` e `date_of_birth`, e anche il test per la lunghezza del campo `last_name` sono stati omessi. Aggiungi ora le tue versioni, seguendo le convenzioni di denominazione e gli approcci sopra mostrati.

Dobbiamo anche testare i nostri metodi personalizzati. Questi essenzialmente controllano che il nome dell'oggetto sia costruito come ci aspettavamo usando il formato "Cognome", "Nome", e che l'URL che otteniamo per un oggetto `Author` sia come ci aspetteremmo.

```python
def test_object_name_is_last_name_comma_first_name(self):
    author = Author.objects.get(id=1)
    expected_object_name = f'{author.last_name}, {author.first_name}'
    self.assertEqual(str(author), expected_object_name)

def test_get_absolute_url(self):
    author = Author.objects.get(id=1)
    # This will also fail if the URLConf is not defined.
    self.assertEqual(author.get_absolute_url(), '/catalog/author/1')
```

Esegui i test ora. Se hai creato il modello Author come abbiamo descritto nel tutorial sui modelli, è molto probabile che otterrai un errore per l'etichetta `date_of_death` come mostrato qui sotto. Il test non riesce perché è stato scritto aspettandosi che la definizione dell'etichetta seguisse la convenzione di Django di non maiuscolare la prima lettera dell'etichetta (Django lo fa per te).

```bash
======================================================================
FAIL: test_date_of_death_label (catalog.tests.test_models.AuthorModelTest)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "D:\...\locallibrary\catalog\tests\test_models.py", line 32, in test_date_of_death_label
    self.assertEqual(field_label,'died')
AssertionError: 'Died' != 'died'
- Died
? ^
+ died
? ^
```

Questo è un bug molto minore, ma evidenzia come scrivere test possa verificare più a fondo qualsiasi presupposizione che potresti avere fatto.

> [!NOTE]
> Cambia l'etichetta per il campo `date_of_death` (**/catalog/models.py**) in "died" e riesegui i test.

I pattern per testare gli altri modelli sono simili, quindi non continueremo a discuterli ulteriormente. Sentiti libero di creare i tuoi test per gli altri modelli.

### Moduli

La filosofia per testare i tuoi moduli è la stessa che per testare i tuoi modelli; devi testare qualsiasi cosa che hai codificato o che il tuo progetto specifica, ma non il comportamento del framework sottostante e di altre librerie di terze parti.

Generalmente questo significa che dovresti testare che i moduli abbiano i campi che desideri e che questi siano visualizzati con etichette e testi di aiuto appropriati. Non è necessario verificare che Django validi correttamente il tipo di campo (a meno che non hai creato un tuo campo personalizzato e la sua validazione) — i.e., non è necessario testare che un campo email accetti solo email. Tuttavia, sarebbe necessario testare qualsiasi ulteriore validazione che ti aspetti venga eseguita sui campi e qualsiasi messaggio che il tuo codice genererà per errori.

Considera il nostro modulo per il rinnovo dei libri. Questo ha solo un campo per la data di rinnovo, che avrà un'etichetta e un testo di aiuto che dovremo verificare.

```python
class RenewBookForm(forms.Form):
    """Form for a librarian to renew books."""
    renewal_date = forms.DateField(help_text="Enter a date between now and 4 weeks (default 3).")

    def clean_renewal_date(self):
        data = self.cleaned_data['renewal_date']

        # Check if a date is not in the past.
        if data < datetime.date.today():
            raise ValidationError(_('Invalid date - renewal in past'))

        # Check if date is in the allowed range (+4 weeks from today).
        if data > datetime.date.today() + datetime.timedelta(weeks=4):
            raise ValidationError(_('Invalid date - renewal more than 4 weeks ahead'))

        # Remember to always return the cleaned data.
        return data
```

Apri il nostro file **/catalog/tests/test_forms.py** e sostituisci qualsiasi codice esistente con il seguente codice di test per il modulo `RenewBookForm`. Iniziamo importando il nostro modulo e alcune librerie Python e Django per aiutare a testare la funzionalità relativa al tempo. Quindi dichiariamo la nostra classe di test del modulo nello stesso modo in cui abbiamo fatto per i modelli, usando un nome descrittivo per la nostra classe di test derivata `TestCase`.

```python
import datetime

from django.test import TestCase
from django.utils import timezone

from catalog.forms import RenewBookForm

class RenewBookFormTest(TestCase):
    def test_renew_form_date_field_label(self):
        form = RenewBookForm()
        self.assertTrue(form.fields['renewal_date'].label is None or form.fields['renewal_date'].label == 'renewal date')

    def test_renew_form_date_field_help_text(self):
        form = RenewBookForm()
        self.assertEqual(form.fields['renewal_date'].help_text, 'Enter a date between now and 4 weeks (default 3).')

    def test_renew_form_date_in_past(self):
        date = datetime.date.today() - datetime.timedelta(days=1)
        form = RenewBookForm(data={'renewal_date': date})
        self.assertFalse(form.is_valid())

    def test_renew_form_date_too_far_in_future(self):
        date = datetime.date.today() + datetime.timedelta(weeks=4) + datetime.timedelta(days=1)
        form = RenewBookForm(data={'renewal_date': date})
        self.assertFalse(form.is_valid())

    def test_renew_form_date_today(self):
        date = datetime.date.today()
        form = RenewBookForm(data={'renewal_date': date})
        self.assertTrue(form.is_valid())

    def test_renew_form_date_max(self):
        date = timezone.localtime() + datetime.timedelta(weeks=4)
        form = RenewBookForm(data={'renewal_date': date})
        self.assertTrue(form.is_valid())
```

Le prime due funzioni testano che l'etichetta del campo e il testo di aiuto siano come previsto. Dobbiamo accedere al campo utilizzando il dizionario dei campi (e.g., `form.fields['renewal_date']`). Si noti qui che dobbiamo anche testare se il valore dell'etichetta è `None`, perché anche se Django visualizzerà l'etichetta corretta restituisce `None` se il valore non è _esplicitamente_ impostato.

Il resto delle funzioni testano che il modulo sia valido per date di rinnovo appena entro il range accettabile e invalide per valori al di fuori del range. Nota come costruiamo i valori delle date di test intorno alla nostra data attuale (`datetime.date.today()`) utilizzando `datetime.timedelta()` (in questo caso specificando un numero di giorni o settimane). Quindi creiamo semplicemente il modulo, passando i nostri dati, e testiamo se è valido.

> [!NOTE]
> Qui non usiamo effettivamente il database o il client di test. Considera la possibilità di modificare questi test per utilizzare [SimpleTestCase](https://docs.djangoproject.com/en/5.0/topics/testing/tools/#django.test.SimpleTestCase).
>
> Dobbiamo anche validare che gli errori corretti vengano sollevati se il modulo è invalido, tuttavia questo è di solito fatto come parte dell'elaborazione della vista, quindi lo affronteremo nella sezione successiva.

> [!WARNING]
> Se usi la classe [ModelForm](/it/docs/Learn_web_development/Extensions/Server-side/Django/Forms#modelforms) `RenewBookModelForm(forms.ModelForm)` invece della classe `RenewBookForm(forms.Form)`, allora il nome del campo del modulo sarebbe **'due_back'** invece di **'renewal_date'**.

Questo è tutto per i moduli; ne abbiamo altri, ma vengono creati automaticamente dalle nostre viste di modifica basate su classi generiche e dovrebbero essere testati lì! Esegui i test e conferma che il nostro codice passi ancora!

### Viste

Per convalidare il comportamento delle nostre viste, utilizziamo il test [Client](https://docs.djangoproject.com/en/5.0/topics/testing/tools/#django.test.Client) di Django. Questa classe agisce come un browser web fittizio che possiamo utilizzare per simulare richieste `GET` e `POST` su un URL e osservare la risposta. Possiamo vedere quasi tutto della risposta, dai livelli di HTTP (intestazioni di risultato e codici di stato) attraverso il modello che stiamo usando per rendere l'HTML e i dati di contesto che stiamo passando a esso. Possiamo anche vedere la catena di reindirizzamenti (se presenti) e controllare l'URL e il codice di stato a ciascun passo. Questo ci permette di verificare che ogni vista stia facendo quello che ci si aspetta.

Iniziamo con una delle nostre viste più semplici, che fornisce un elenco di tutti gli Autori. Questo viene visualizzato all'URL **/catalog/authors/** (un URL denominato 'authors' nella configurazione degli URL).

```python
class AuthorListView(generic.ListView):
    model = Author
    paginate_by = 10
```

Poiché questa è una vista di elenco generico, quasi tutto viene fatto per noi da Django. Si potrebbe dire che se ti fidi di Django l'unica cosa che devi testare è che la vista sia accessibile all'URL corretto e possa essere accessibile utilizzando il suo nome. Tuttavia, se stai utilizzando un processo di sviluppo basato sui test, inizierai scrivendo test che confermano che la vista visualizza tutti gli Autori, paginandoli in gruppi di 10.

Apri il file **/catalog/tests/test_views.py** e sostituisci qualsiasi testo esistente con il seguente codice di test per la `AuthorListView`. Come prima importiamo il nostro modello e alcune classi utili. Nel metodo `setUpTestData()` impostiamo un certo numero di oggetti `Author` in modo da poter testare la nostra paginazione.

```python
from django.test import TestCase
from django.urls import reverse

from catalog.models import Author

class AuthorListViewTest(TestCase):
    @classmethod
    def setUpTestData(cls):
        # Create 13 authors for pagination tests
        number_of_authors = 13

        for author_id in range(number_of_authors):
            Author.objects.create(
                first_name=f'Dominique {author_id}',
                last_name=f'Surname {author_id}',
            )

    def test_view_url_exists_at_desired_location(self):
        response = self.client.get('/catalog/authors/')
        self.assertEqual(response.status_code, 200)

    def test_view_url_accessible_by_name(self):
        response = self.client.get(reverse('authors'))
        self.assertEqual(response.status_code, 200)

    def test_view_uses_correct_template(self):
        response = self.client.get(reverse('authors'))
        self.assertEqual(response.status_code, 200)
        self.assertTemplateUsed(response, 'catalog/author_list.html')

    def test_pagination_is_ten(self):
        response = self.client.get(reverse('authors'))
        self.assertEqual(response.status_code, 200)
        self.assertTrue('is_paginated' in response.context)
        self.assertTrue(response.context['is_paginated'] == True)
        self.assertEqual(len(response.context['author_list']), 10)

    def test_lists_all_authors(self):
        # Get second page and confirm it has (exactly) remaining 3 items
        response = self.client.get(reverse('authors')+'?page=2')
        self.assertEqual(response.status_code, 200)
        self.assertTrue('is_paginated' in response.context)
        self.assertTrue(response.context['is_paginated'] == True)
        self.assertEqual(len(response.context['author_list']), 3)
```

Tutti i test utilizzano il client (che appartiene alla nostra classe derivata `TestCase`) per simulare una richiesta `GET` e ottenere una risposta. La prima versione controlla un URL specifico (nota, solo il percorso specifico senza il dominio) mentre la seconda genera l'URL dal suo nome nella configurazione degli URL.

```python
response = self.client.get('/catalog/authors/')
response = self.client.get(reverse('authors'))
```

Una volta che abbiamo la risposta, la interroghiamo per il suo codice di stato, il modello usato, se la risposta è paginata o meno, il numero di elementi restituiti e il numero totale di elementi.

> [!NOTE]
> Se imposti la variabile `paginate_by` nel tuo file **/catalog/views.py** a un numero diverso da 10, assicurati di aggiornare le righe che verificano che il numero corretto di elementi siano visualizzati nei template paginati sopra e nelle sezioni seguenti. Ad esempio, se imposti la variabile per la pagina dell'elenco degli autori a 5, aggiorna la riga sopra a:
>
> ```python
> self.assertTrue(len(response.context['author_list']) == 5)
> ```

La variabile più interessante che dimostriamo sopra è `response.context`, che è la variabile del contesto passata al template dalla vista.
Questo è incredibilmente utile per il test, poiché ci consente di confermare che il nostro template stia ricevendo tutti i dati di cui ha bisogno. In altre parole possiamo verificare che stiamo utilizzando il modello previsto e quali dati il modello sta ricevendo, il che va molto lontano nel verificare che eventuali problemi di rendering siano dovuti esclusivamente al template.

#### Viste riservate agli utenti connessi

In alcuni casi potresti voler testare una vista riservata solo agli utenti connessi. Ad esempio, la nostra `LoanedBooksByUserListView` è molto simile alla nostra vista precedente ma è disponibile solo agli utenti connessi e visualizza solo i record di `BookInstance` che sono presi in prestito dall'utente corrente, hanno lo stato 'in prestito' e sono ordinati "i più vecchi prima".

```python
from django.contrib.auth.mixins import LoginRequiredMixin

class LoanedBooksByUserListView(LoginRequiredMixin, generic.ListView):
    """Generic class-based view listing books on loan to current user."""
    model = BookInstance
    template_name ='catalog/bookinstance_list_borrowed_user.html'
    paginate_by = 10

    def get_queryset(self):
        return BookInstance.objects.filter(borrower=self.request.user).filter(status__exact='o').order_by('due_back')
```

Aggiungi il seguente codice di test a **/catalog/tests/test_views.py**. Qui usiamo prima `SetUp()` per creare alcuni account di accesso utente e oggetti `BookInstance` (insieme ai loro libri associati e altri record) che utilizzeremo in seguito nei test. Metà dei libri sono presi in prestito da ciascun utente di test, ma inizialmente abbiamo impostato lo stato di tutti i libri su "manutenzione". Abbiamo usato `SetUp()` piuttosto che `setUpTestData()` perché modificheremo alcuni di questi oggetti in seguito.

> [!NOTE]
> Il codice `setUp()` qui sotto crea un libro con una `Language` specificata, ma _il tuo_ codice potrebbe non includere il modello `Language` poiché questo è stato creato come _sfida_. Se è così, commenta le parti del codice che creano o importano oggetti Language. Dovresti farlo anche nella sezione `RenewBookInstancesViewTest` che segue.

```python
import datetime

from django.utils import timezone

# Get user model from settings
from django.contrib.auth import get_user_model
User = get_user_model()

from catalog.models import BookInstance, Book, Genre, Language

class LoanedBookInstancesByUserListViewTest(TestCase):
    def setUp(self):
        # Create two users
        test_user1 = User.objects.create_user(username='testuser1', password='1X<ISRUkw+tuK')
        test_user2 = User.objects.create_user(username='testuser2', password='2HJ1vRV0Z&3iD')

        test_user1.save()
        test_user2.save()

        # Create a book
        test_author = Author.objects.create(first_name='John', last_name='Smith')
        test_genre = Genre.objects.create(name='Fantasy')
        test_language = Language.objects.create(name='English')
        test_book = Book.objects.create(
            title='Book Title',
            summary='My book summary',
            isbn='ABCDEFG',
            author=test_author,
            language=test_language,
        )

        # Create genre as a post-step
        genre_objects_for_book = Genre.objects.all()
        test_book.genre.set(genre_objects_for_book) # Direct assignment of many-to-many types not allowed.
        test_book.save()

        # Create 30 BookInstance objects
        number_of_book_copies = 30
        for book_copy in range(number_of_book_copies):
            return_date = timezone.localtime() + datetime.timedelta(days=book_copy%5)
            the_borrower = test_user1 if book_copy % 2 else test_user2
            status = 'm'
            BookInstance.objects.create(
                book=test_book,
                imprint='Unlikely Imprint, 2016',
                due_back=return_date,
                borrower=the_borrower,
                status=status,
            )

    def test_redirect_if_not_logged_in(self):
        response = self.client.get(reverse('my-borrowed'))
        self.assertRedirects(response, '/accounts/login/?next=/catalog/mybooks/')

    def test_logged_in_uses_correct_template(self):
        login = self.client.login(username='testuser1', password='1X<ISRUkw+tuK')
        response = self.client.get(reverse('my-borrowed'))

        # Check our user is logged in
        self.assertEqual(str(response.context['user']), 'testuser1')
        # Check that we got a response "success"
        self.assertEqual(response.status_code, 200)

        # Check we used correct template
        self.assertTemplateUsed(response, 'catalog/bookinstance_list_borrowed_user.html')
```

Per verificare che la vista reindirizzi a una pagina di accesso se l'utente non è connesso usiamo `assertRedirects`, come dimostrato in `test_redirect_if_not_logged_in()`. Per verificare che la pagina sia visualizzata per un utente connesso accediamo prima il nostro utente di test e quindi accediamo di nuovo la pagina e controlliamo che otteniamo un `status_code` di 200 (successo).

Il resto dei test verifica che la nostra vista ritorni solo libri che sono in prestito al nostro attuale locatario. Copia il codice sotto e incollalo alla fine della classe di test sopra.

```python
    def test_only_borrowed_books_in_list(self):
        login = self.client.login(username='testuser1', password='1X<ISRUkw+tuK')
        response = self.client.get(reverse('my-borrowed'))

        # Check our user is logged in
        self.assertEqual(str(response.context['user']), 'testuser1')
        # Check that we got a response "success"
        self.assertEqual(response.status_code, 200)

        # Check that initially we don't have any books in list (none on loan)
        self.assertTrue('bookinstance_list' in response.context)
        self.assertEqual(len(response.context['bookinstance_list']), 0)

        # Now change all books to be on loan
        books = BookInstance.objects.all()[:10]

        for book in books:
            book.status = 'o'
            book.save()

        # Check that now we have borrowed books in the list
        response = self.client.get(reverse('my-borrowed'))
        # Check our user is logged in
        self.assertEqual(str(response.context['user']), 'testuser1')
        # Check that we got a response "success"
        self.assertEqual(response.status_code, 200)

        self.assertTrue('bookinstance_list' in response.context)

        # Confirm all books belong to testuser1 and are on loan
        for book_item in response.context['bookinstance_list']:
            self.assertEqual(response.context['user'], book_item.borrower)
            self.assertEqual(book_item.status, 'o')

    def test_pages_ordered_by_due_date(self):
        # Change all books to be on loan
        for book in BookInstance.objects.all():
            book.status='o'
            book.save()

        login = self.client.login(username='testuser1', password='1X<ISRUkw+tuK')
        response = self.client.get(reverse('my-borrowed'))

        # Check our user is logged in
        self.assertEqual(str(response.context['user']), 'testuser1')
        # Check that we got a response "success"
        self.assertEqual(response.status_code, 200)

        # Confirm that of the items, only 10 are displayed due to pagination.
        self.assertEqual(len(response.context['bookinstance_list']), 10)

        last_date = 0
        for book in response.context['bookinstance_list']:
            if last_date == 0:
                last_date = book.due_back
            else:
                self.assertTrue(last_date <= book.due_back)
                last_date = book.due_back
```

Potresti anche aggiungere test di paginazione, se lo desideri!

#### Testare viste con moduli

Testare viste con moduli è un po' più complicato rispetto ai casi sopra, perché è necessario testare più percorsi di codice: visualizzazione iniziale, visualizzazione dopo che la convalida dei dati è fallita, e visualizzazione dopo che la convalida è riuscita. La buona notizia è che utilizziamo il client per i test quasi esattamente allo stesso modo in cui abbiamo fatto per le viste di sola visualizzazione.

Per dimostrare, scriviamo alcuni test per la vista utilizzata per rinnovare i libri (`renew_book_librarian()`):

```python
from catalog.forms import RenewBookForm

@permission_required('catalog.can_mark_returned')
def renew_book_librarian(request, pk):
    """View function for renewing a specific BookInstance by librarian."""
    book_instance = get_object_or_404(BookInstance, pk=pk)

    # If this is a POST request then process the Form data
    if request.method == 'POST':

        # Create a form instance and populate it with data from the request (binding):
        book_renewal_form = RenewBookForm(request.POST)

        # Check if the form is valid:
        if form.is_valid():
            # process the data in form.cleaned_data as required (here we just write it to the model due_back field)
            book_instance.due_back = form.cleaned_data['renewal_date']
            book_instance.save()

            # redirect to a new URL:
            return HttpResponseRedirect(reverse('all-borrowed'))

    # If this is a GET (or any other method) create the default form
    else:
        proposed_renewal_date = datetime.date.today() + datetime.timedelta(weeks=3)
        book_renewal_form = RenewBookForm(initial={'renewal_date': proposed_renewal_date})

    context = {
        'book_renewal_form': book_renewal_form,
        'book_instance': book_instance,
    }

    return render(request, 'catalog/book_renew_librarian.html', context)
```

Dobbiamo testare che la vista sia disponibile solo agli utenti che hanno il permesso `can_mark_returned`, e che gli utenti siano reindirizzati a una pagina di errore HTTP 404 se tentano di rinnovare un `BookInstance` che non esiste. Dovremmo controllare che il valore iniziale del modulo sia impostato con una data tra tre settimane, e che se la convalida riesce siamo reindirizzati alla vista "tutti i libri presi in prestito". Nel controllare i test di convalida non riuscita, controlleremo anche che il nostro modulo mandi i messaggi di errore appropriati.

Aggiungi la prima parte della classe di test (mostrata qui sotto) alla fine di **/catalog/tests/test_views.py**.
Questo crea due utenti e due istanze di libri, ma solo un utente ha il permesso richiesto per accedere alla vista.

```python
import uuid

from django.contrib.auth.models import Permission # Required to grant the permission needed to set a book as returned.

class RenewBookInstancesViewTest(TestCase):
    def setUp(self):
        # Create a user
        test_user1 = User.objects.create_user(username='testuser1', password='1X<ISRUkw+tuK')
        test_user2 = User.objects.create_user(username='testuser2', password='2HJ1vRV0Z&3iD')

        test_user1.save()
        test_user2.save()

        # Give test_user2 permission to renew books.
        permission = Permission.objects.get(name='Set book as returned')
        test_user2.user_permissions.add(permission)
        test_user2.save()

        # Create a book
        test_author = Author.objects.create(first_name='John', last_name='Smith')
        test_genre = Genre.objects.create(name='Fantasy')
        test_language = Language.objects.create(name='English')
        test_book = Book.objects.create(
            title='Book Title',
            summary='My book summary',
            isbn='ABCDEFG',
            author=test_author,
            language=test_language,
        )

        # Create genre as a post-step
        genre_objects_for_book = Genre.objects.all()
        test_book.genre.set(genre_objects_for_book) # Direct assignment of many-to-many types not allowed.
        test_book.save()

        # Create a BookInstance object for test_user1
        return_date = datetime.date.today() + datetime.timedelta(days=5)
        self.test_bookinstance1 = BookInstance.objects.create(
            book=test_book,
            imprint='Unlikely Imprint, 2016',
            due_back=return_date,
            borrower=test_user1,
            status='o',
        )

        # Create a BookInstance object for test_user2
        return_date = datetime.date.today() + datetime.timedelta(days=5)
        self.test_bookinstance2 = BookInstance.objects.create(
            book=test_book,
            imprint='Unlikely Imprint, 2016',
            due_back=return_date,
            borrower=test_user2,
            status='o',
        )
```

Aggiungi i seguenti test alla fine della classe di test. Questi verificano che solo gli utenti con i permessi corretti (_testuser2_) possano accedere alla vista. Controlliamo tutti i casi: quando l'utente non è connesso, quando un utente è connesso ma non ha i permessi corretti, quando l'utente ha i permessi ma non è il locatario (dovrebbe riuscire), e cosa succede quando provano ad accedere a un `BookInstance` che non esiste. Verifichiamo anche che venga usato il modello corretto.

```python
   def test_redirect_if_not_logged_in(self):
        response = self.client.get(reverse('renew-book-librarian', kwargs={'pk': self.test_bookinstance1.pk}))
        # Manually check redirect (Can't use assertRedirect, because the redirect URL is unpredictable)
        self.assertEqual(response.status_code, 302)
        self.assertTrue(response.url.startswith('/accounts/login/'))

    def test_forbidden_if_logged_in_but_not_correct_permission(self):
        login = self.client.login(username='testuser1', password='1X<ISRUkw+tuK')
        response = self.client.get(reverse('renew-book-librarian', kwargs={'pk': self.test_bookinstance1.pk}))
        self.assertEqual(response.status_code, 403)

    def test_logged_in_with_permission_borrowed_book(self):
        login = self.client.login(username='testuser2', password='2HJ1vRV0Z&3iD')
        response = self.client.get(reverse('renew-book-librarian', kwargs={'pk': self.test_bookinstance2.pk}))

        # Check that it lets us login - this is our book and we have the right permissions.
        self.assertEqual(response.status_code, 200)

    def test_logged_in_with_permission_another_users_borrowed_book(self):
        login = self.client.login(username='testuser2', password='2HJ1vRV0Z&3iD')
        response = self.client.get(reverse('renew-book-librarian', kwargs={'pk': self.test_bookinstance1.pk}))

        # Check that it lets us login. We're a librarian, so we can view any users book
        self.assertEqual(response.status_code, 200)

    def test_HTTP404_for_invalid_book_if_logged_in(self):
        # unlikely UID to match our bookinstance!
        test_uid = uuid.uuid4()
        login = self.client.login(username='testuser2', password='2HJ1vRV0Z&3iD')
        response = self.client.get(reverse('renew-book-librarian', kwargs={'pk':test_uid}))
        self.assertEqual(response.status_code, 404)

    def test_uses_correct_template(self):
        login = self.client.login(username='testuser2', password='2HJ1vRV0Z&3iD')
        response = self.client.get(reverse('renew-book-librarian', kwargs={'pk': self.test_bookinstance1.pk}))
        self.assertEqual(response.status_code, 200)

        # Check we used correct template
        self.assertTemplateUsed(response, 'catalog/book_renew_librarian.html')
```

Aggiungi il prossimo metodo di test, come mostrato sotto. Questo controlla che la data iniziale del modulo sia tra tre settimane. Nota come siamo in grado di accedere al valore iniziale del campo del modulo (`response.context['form'].initial['renewal_date'])`.

```python
    def test_form_renewal_date_initially_has_date_three_weeks_in_future(self):
        login = self.client.login(username='testuser2', password='2HJ1vRV0Z&3iD')
        response = self.client.get(reverse('renew-book-librarian', kwargs={'pk': self.test_bookinstance1.pk}))
        self.assertEqual(response.status_code, 200)

        date_3_weeks_in_future = datetime.date.today() + datetime.timedelta(weeks=3)
        self.assertEqual(response.context['form'].initial['renewal_date'], date_3_weeks_in_future)
```

Il prossimo test (aggiungi anche questo alla classe) verifica che la vista reindirizzi all'elenco di tutti i libri presi in prestito se il rinnovo riesce. Ciò che differisce qui è che per la prima volta mostriamo come è possibile inviare dati `POST` utilizzando il client. I dati _post_ sono il secondo argomento alla funzione post, e sono specificati come un dizionario di chiavi/valori.

```python
    def test_redirects_to_all_borrowed_book_list_on_success(self):
        login = self.client.login(username='testuser2', password='2HJ1vRV0Z&3iD')
        valid_date_in_future = datetime.date.today() + datetime.timedelta(weeks=2)
        response = self.client.post(reverse('renew-book-librarian', kwargs={'pk':self.test_bookinstance1.pk,}), {'renewal_date':valid_date_in_future})
        self.assertRedirects(response, reverse('all-borrowed'))
```

> [!WARNING]
> La vista _all-borrowed_ è stata aggiunta come _sfida_, e il tuo codice potrebbe invece reindirizzare alla pagina iniziale '/'. Se è così, modifica le ultime due righe del codice di test per essere come il codice qui sotto. Il `follow=True` nella richiesta assicura che la richiesta restituisca l'URL della destinazione finale (da qui il controllo `/catalog/` piuttosto che `/`).
>
> ```python
>  response = self.client.post(reverse('renew-book-librarian', kwargs={'pk':self.test_bookinstance1.pk,}), {'renewal_date':valid_date_in_future}, follow=True)
>  self.assertRedirects(response, '/catalog/')
> ```

Copia le ultime due funzioni nella classe, come visto sotto. Queste testano di nuovo le richieste `POST`, ma in questo caso con date di rinnovo non valide. Utilizziamo `assertFormError()` per verificare che i messaggi di errore siano come previsto.

```python
    def test_form_invalid_renewal_date_past(self):
        login = self.client.login(username='testuser2', password='2HJ1vRV0Z&3iD')
        date_in_past = datetime.date.today() - datetime.timedelta(weeks=1)
        response = self.client.post(reverse('renew-book-librarian', kwargs={'pk': self.test_bookinstance1.pk}), {'renewal_date': date_in_past})
        self.assertEqual(response.status_code, 200)
        self.assertFormError(response.context['form'], 'renewal_date', 'Invalid date - renewal in past')

    def test_form_invalid_renewal_date_future(self):
        login = self.client.login(username='testuser2', password='2HJ1vRV0Z&3iD')
        invalid_date_in_future = datetime.date.today() + datetime.timedelta(weeks=5)
        response = self.client.post(reverse('renew-book-librarian', kwargs={'pk': self.test_bookinstance1.pk}), {'renewal_date': invalid_date_in_future})
        self.assertEqual(response.status_code, 200)
        self.assertFormError(response.context['form'], 'renewal_date', 'Invalid date - renewal more than 4 weeks ahead')
```

Le stesse tecniche possono essere usate per testare l'altra vista.

### Template

Django fornisce API di test per verificare che il modello corretto venga chiamato dalle tue viste e per consentirti di verificare che le informazioni corrette vengano inviate. Tuttavia, non esiste un supporto API specifico per i test in Django che ti permetta di verificare che il tuo output HTML venga reso come previsto.

## Altri strumenti di test raccomandati

Il framework di test di Django può aiutarti a scrivere test unitari e di integrazione efficaci — abbiamo solo graffiato la superficie di ciò che il framework **unittest** sottostante può fare, per non parlare delle aggiunte di Django (ad esempio, scopri come puoi usare [unittest.mock](https://docs.python.org/3/library/unittest.mock-examples.html) per patchare librerie di terze parti in modo da poter testare più a fondo il tuo codice).

Mentre ci sono numerosi altri strumenti di test che puoi usare, ne evidenziamo solo due:

- [Coverage](https://coverage.readthedocs.io/en/latest/): Questo strumento Python segnala quanto del tuo codice viene effettivamente eseguito dai tuoi test. È particolarmente utile quando stai iniziando e stai cercando di capire esattamente cosa dovresti testare.
- [Selenium](/it/docs/Learn_web_development/Extensions/Testing/Your_own_automation_environment) è un framework per automatizzare i test in un browser reale. Ti permette di simulare un vero utente che interagisce con il sito e fornisce un ottimo framework per test di sistema del tuo sito (il passo successivo rispetto ai test di integrazione).

## Sfida te stesso

Ci sono molti altri modelli e viste che possiamo testare. Come sfida, prova a creare un caso di test per la vista `AuthorCreate`.

```python
class AuthorCreate(PermissionRequiredMixin, CreateView):
    model = Author
    fields = ['first_name', 'last_name', 'date_of_birth', 'date_of_death']
    initial = {'date_of_death': '11/11/2023'}
    permission_required = 'catalog.add_author'
```

Ricorda che devi verificare tutto ciò che specifichi o che fa parte del progetto.
Questo includerà chi ha accesso, la data iniziale, il modello usato e dove la vista reindirizza in caso di successo.

Potresti utilizzare il seguente codice per impostare il tuo test e assegnare al tuo utente il permesso appropriato

```python
class AuthorCreateViewTest(TestCase):
    """Test case for the AuthorCreate view (Created as Challenge)."""

    def setUp(self):
        # Create a user
        test_user = User.objects.create_user(
            username='test_user', password='some_password')

        content_typeAuthor = ContentType.objects.get_for_model(Author)
        permAddAuthor = Permission.objects.get(
            codename="add_author",
            content_type=content_typeAuthor,
        )

        test_user.user_permissions.add(permAddAuthor)
        test_user.save()
```

## Sommario

Scrivere codice di test non è né divertente né affascinante, ed è conseguentemente spesso lasciato per ultimo (o per niente) quando si crea un sito web. È tuttavia una parte essenziale per garantire che il tuo codice sia sicuro da rilasciare dopo aver apportato modifiche e sia conveniente da mantenere.

In questo tutorial ti abbiamo mostrato come scrivere ed eseguire test per i tuoi modelli, moduli e viste. Soprattutto, abbiamo fornito un breve riassunto di cosa dovresti testare, che è spesso la parte più difficile da capire quando stai iniziando. C'è molto altro da sapere, ma anche con quanto hai già imparato dovresti essere in grado di creare test unitari efficaci per i tuoi siti web.

Il prossimo e ultimo tutorial mostra come puoi distribuire il tuo meraviglioso sito web Django (e completamente testato!).

## Vedi anche

- [Scrivere ed eseguire test](https://docs.djangoproject.com/en/5.0/topics/testing/overview/) (documenti Django)
- [Scrivere la tua prima app Django, parte 5 > Introduzione ai test automatizzati](https://docs.djangoproject.com/en/5.0/intro/tutorial05/) (documenti Django)
- [Riferimento agli strumenti di test](https://docs.djangoproject.com/en/5.0/topics/testing/tools/) (documenti Django)
- [Argomenti avanzati di test](https://docs.djangoproject.com/en/5.0/topics/testing/advanced/) (documenti Django)
- [Una guida ai test in Django](https://toastdriven.com/blog/2011/apr/09/guide-to-testing-in-django/) (Blog Toast Driven, 2011)
- [Workshop: Sviluppo Web Test-Driven con Django](https://test-driven-django-development.readthedocs.io/en/latest/index.html) (San Diego Python, 2014)
- [Testare in Django (Parte 1) - Migliori pratiche ed esempi](https://realpython.com/testing-in-django-part-1-best-practices-and-examples/) (RealPython, 2013)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Forms", "Learn_web_development/Extensions/Server-side/Django/Deployment", "Learn_web_development/Extensions/Server-side/Django")}}
