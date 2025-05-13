---
title: "Tutorial Django Parte 10: Testare un'applicazione web Django"
short-title: "10: Testare"
slug: Learn_web_development/Extensions/Server-side/Django/Testing
l10n:
  sourceCommit: be1922d62a0d31e4e3441db0e943aed8df736481
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Forms", "Learn_web_development/Extensions/Server-side/Django/Deployment", "Learn_web_development/Extensions/Server-side/Django")}}

Man mano che i siti web crescono, diventano più difficili da testare manualmente. Non solo ci sarà più da testare, ma, man mano che le interazioni tra i componenti diventano più complesse, una piccola modifica in un'area può influenzare altre aree, quindi saranno necessarie ulteriori modifiche per garantire che tutto continui a funzionare e che non vengano introdotti errori man mano che vengono apportate altre modifiche. Un modo per mitigare questi problemi è scrivere test automatizzati, che possono essere eseguiti facilmente e in modo affidabile ogni volta che si apporta una modifica. Questo tutorial mostra come automatizzare i _test unitari_ del suo sito web utilizzando il framework di test di Django.

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
      <td>Comprendere come scrivere test unitari per siti Web basati su Django.</td>
    </tr>
  </tbody>
</table>

## Panoramica

La [Libreria Locale](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website) attualmente ha pagine per visualizzare elenchi di tutti i libri e autori, viste dettagliate per gli elementi `Book` e `Author`, una pagina per rinnovare gli elementi `BookInstance` e pagine per creare, aggiornare ed eliminare gli elementi `Author` (e anche i record `Book`, se ha completato la _sfida_ nel [tutorial sui moduli](/it/docs/Learn_web_development/Extensions/Server-side/Django/Forms)). Anche con questo sito relativamente piccolo, navigare manualmente su ogni pagina e controllare _superficialmente_ che tutto funzioni come previsto può richiedere diversi minuti. Man mano che apportiamo modifiche e sviluppiamo il sito, il tempo richiesto per verificare manualmente che tutto funzioni "correttamente" crescerà solo. Se continuassimo come siamo, alla fine spenderemmo la maggior parte del nostro tempo a testare e pochissimo tempo a migliorare il nostro codice.

I test automatizzati possono davvero aiutare con questo problema! I benefici ovvi sono che possono essere eseguiti molto più velocemente dei test manuali, possono testare a un livello di dettaglio molto inferiore e testare esattamente la stessa funzionalità ogni volta (i tester umani non sono neanche lontanamente così affidabili!). Poiché sono veloci, i test automatizzati possono essere eseguiti più regolarmente e se un test fallisce, indicano esattamente dove il codice non sta performando come previsto.

In aggiunta, i test automatizzati possono agire come il primo "utente" reale del suo codice, costringendola ad essere rigoroso nel definire e documentare come dovrebbe comportarsi il suo sito web. Spesso sono la base per i suoi esempi di codice e la documentazione. Per questi motivi, alcuni processi di sviluppo software iniziano con la definizione e l'implementazione dei test, dopo di che il codice viene scritto per corrispondere al comportamento richiesto (ad esempio, sviluppo [guidato dai test](https://en.wikipedia.org/wiki/Test-driven_development) e [guidato dal comportamento](https://en.wikipedia.org/wiki/Behavior-driven_development)).

Questo tutorial mostra come scrivere test automatizzati per Django, aggiungendo una serie di test al sito web _LocalLibrary_.

### Tipi di test

Esistono numerosi tipi, livelli e classificazioni di test e approcci di testing. I test automatizzati più importanti sono:

- Test unitari
  - : Verificano il comportamento funzionale dei singoli componenti, spesso a livello di classe e funzione.
- Test di regressione
  - : Test che riproducono bug storici. Ogni test viene eseguito inizialmente per verificare che il bug sia stato corretto e poi rieseguito per garantire che non sia stato reintrodotto in seguito a modifiche successive al codice.
- Test di integrazione
  - : Verificano come funzionano insieme i raggruppamenti di componenti. I test di integrazione sono a conoscenza delle interazioni richieste tra i componenti, ma non necessariamente delle operazioni interne di ciascun componente. Possono coprire semplici raggruppamenti di componenti fino all'intero sito web.

> [!NOTE]
> Altri tipi comuni di test includono: black box, white box, manuale, automatizzato, canary, smoke, conformità, accettazione, funzionale, sistema, prestazioni, carico e stress. Cercateli per ulteriori informazioni.

### Cosa fornisce Django per il testing?

Testare un sito web è un compito complesso, perché è composto da diversi livelli di logica - dalla gestione delle richieste a livello HTTP, alle query del modello, alla validazione e al processamento dei moduli, e al rendering dei template.

Django fornisce un framework di test con una piccola gerarchia di classi che si basano sulla libreria standard di Python [`unittest`](https://docs.python.org/3/library/unittest.html#module-unittest). Nonostante il nome, questo framework di test è adatto sia per test unitari che di integrazione. Il framework Django aggiunge metodi API e strumenti per aiutare a testare il comportamento web e specifico di Django. Questi le permettono di simulare richieste, inserire dati di test e ispezionare l'output della sua applicazione. Django fornisce anche un'API ([LiveServerTestCase](https://docs.djangoproject.com/en/5.0/topics/testing/tools/#liveservertestcase)) e strumenti per [utilizzare diversi framework di testing](https://docs.djangoproject.com/en/5.0/topics/testing/advanced/#other-testing-frameworks), ad esempio può integrare il popolare framework [Selenium](/it/docs/Learn_web_development/Extensions/Testing/Your_own_automation_environment) per simulare un utente che interagisce con un browser live.

Per scrivere un test l'utente deve derivare da qualsiasi delle classi base di test di Django (o _unittest_) ([SimpleTestCase](https://docs.djangoproject.com/en/5.0/topics/testing/tools/#simpletestcase), [TransactionTestCase](https://docs.djangoproject.com/en/5.0/topics/testing/tools/#transactiontestcase), [TestCase](https://docs.djangoproject.com/en/5.0/topics/testing/tools/#testcase), [LiveServerTestCase](https://docs.djangoproject.com/en/5.0/topics/testing/tools/#liveservertestcase)) e poi scrivere metodi separati per verificare che funzionalità specifiche funzionino come previsto (i test usano metodi "assert" per testare se le espressioni risultano in valori `True` o `False`, o se due valori sono uguali, ecc.) Quando avvia un test, il framework esegue i metodi di test scelti nelle sue classi derivate. I metodi di test vengono eseguiti in modo indipendente, con comportamento comune di setup e/o teardown definito nella classe, come mostrato di seguito.

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

La migliore classe base per la maggior parte dei test è [django.test.TestCase](https://docs.djangoproject.com/en/5.0/topics/testing/tools/#testcase). Questa classe di test crea un database pulito prima che i suoi test vengano eseguiti ed esegue ogni funzione di test nella propria transazione. La classe possiede anche un test [Client](https://docs.djangoproject.com/en/5.0/topics/testing/tools/#django.test.Client) che può usare per simulare un utente che interagisce con il codice a livello di vista. Nelle sezioni seguenti ci concentreremo sui test unitari, creati usando questa classe base [TestCase](https://docs.djangoproject.com/en/5.0/topics/testing/tools/#testcase).

> [!NOTE]
> La classe [django.test.TestCase](https://docs.djangoproject.com/en/5.0/topics/testing/tools/#testcase) è molto comoda, ma può causare che alcuni test siano più lenti di quanto necessario (non tutti i test avranno bisogno di configurare il proprio database o simulare l'interazione con la vista). Una volta che ha familiarizzato con ciò che può fare con questa classe, potrebbe voler sostituire alcuni dei suoi test con le classi di test più semplici disponibili.

### Cosa dovrebbe testare?

Dovrebbe testare tutti gli aspetti del proprio codice, ma non nessuna libreria o funzionalità fornita come parte di Python o Django.

Ad esempio, consideri il modello `Author` definito di seguito. Non è necessario testare esplicitamente che `first_name` e `last_name` siano stati memorizzati correttamente come `CharField` nel database perché ciò è qualcosa definito da Django (anche se ovviamente in pratica testerà inevitabilmente questa funzionalità durante lo sviluppo). Né deve testare che `date_of_birth` sia stato convalidato come campo di data, perché anche questo è qualcosa implementato in Django.

Tuttavia, dovrebbe controllare il testo utilizzato per le etichette (_Nome, Cognome, Data di nascita, Morto_), e la dimensione del campo assegnato per il testo (_100 caratteri_), perché questi fanno parte del suo design e qualcosa che potrebbe essere rotto/cambiato in futuro.

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

Allo stesso modo, dovrebbe controllare che i metodi personalizzati `get_absolute_url()` e `__str__()` funzionino come richiesto perché sono il suo codice/logica di business. Nel caso di `get_absolute_url()` può fidarsi che il metodo `reverse()` di Django sia stato implementato correttamente, quindi ciò che sta testando è che la vista associata sia effettivamente stata definita.

> [!NOTE]
> I lettori attenti possono notare che vorremmo anche limitare la data di nascita e di morte a valori sensati e controllare che la morte avvenga dopo la nascita.
> In Django questo vincolo sarebbe aggiunto alle sue classi di moduli (anche se può definire validatori per i campi del modello e i validatori del modello saranno utilizzati solo a livello di modulo se sono chiamati dal metodo `clean()` del modello. Questo richiede un `ModelForm`, o il metodo `clean()` del modello deve essere specificamente chiamato).

Con questo in mente, iniziamo a vedere come definire ed eseguire i test.

## Panoramica della struttura del test

Prima di entrare nel dettaglio di "cosa testare", vediamo brevemente _dove_ e _come_ sono definiti i test.

Django utilizza la [scoperta dei test](https://docs.python.org/3/library/unittest.html#unittest-test-discovery) integrata del modulo unittest, che scoprirà i test nella directory di lavoro corrente in qualsiasi file denominato con il modello **test\*.py**. A condizione di nominare i file in modo appropriato, può utilizzare qualsiasi struttura desideri. Si consiglia di creare un modulo per il suo codice di test e di avere file separati per modelli, viste, moduli e qualsiasi altro tipo di codice che deve testare. Per esempio:

```plain
catalog/
  /tests/
    __init__.py
    test_models.py
    test_forms.py
    test_views.py
```

Crei una struttura di file come mostrato sopra nel suo progetto _LocalLibrary_. Il **\_\_init\_\_.py** dovrebbe essere un file vuoto (questo comunica a Python che la directory è un pacchetto). Può creare i tre file di test copiando e rinominando il file di test scheletro **/catalog/tests.py**.

> [!NOTE]
> Il file di test scheletro **/catalog/tests.py** è stato creato automaticamente quando abbiamo [costruito il sito web scheletro di Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/skeleton_website). È perfettamente "legale" mettere tutti i suoi test all'interno, ma se testa correttamente, finirà rapidamente con un file di test molto grande e ingestibile.
>
> Elimini il file scheletro poiché non ne avremo bisogno.

Apro il file **/catalog/tests/test_models.py**. Il file dovrebbe importare `django.test.TestCase`, come mostrato:

```python
from django.test import TestCase

# Create your tests here.
```

Spesso aggiungerà una classe di test per ogni modello/vista/modulo che desidera testare, con metodi individuali per testare funzionalità specifiche. In altri casi potrebbe voler avere una classe separata per testare un caso d'uso specifico, con funzioni di test individuali che testano aspetti di quel caso d'uso (ad esempio, una classe per testare che un campo del modello sia correttamente convalidato, con funzioni per testare ciascuno dei possibili casi di errore). Ancora una volta, la struttura è molto a sua discrezione, ma è meglio se è coerente.

Aggiunga la classe di test sotto alla fine del file. La classe dimostra come costruire una classe di casi di test derivando da `TestCase`.

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

La nuova classe definisce due metodi che può utilizzare per la configurazione pre-test (ad esempio, per creare eventuali modelli o altri oggetti di cui avrà bisogno per il test):

- `setUpTestData()` viene chiamato una volta all'inizio dell'esecuzione del test per la configurazione a livello di classe. Utilizzerebbe questo per creare oggetti che non saranno modificati o cambiati in nessuno dei metodi di test.
- `setUp()` viene chiamato prima di ogni funzione di test per impostare eventuali oggetti che possono essere modificati dal test (ogni funzione di test riceverà una versione "fresca" di questi oggetti).

> [!NOTE]
> Le classi di test hanno anche un metodo `tearDown()` che non abbiamo utilizzato. Questo metodo non è particolarmente utile per i test del database, poiché la classe base `TestCase` si occupa del teardown del database per lei.

Di seguito abbiamo una serie di metodi di test, che utilizzano le funzioni `Assert` per testare se le condizioni sono vere, false o uguali (`AssertTrue`, `AssertFalse`, `AssertEqual`). Se la condizione non si verifica come previsto, il test fallirà e segnalerà l'errore nella sua console.

Le funzioni `AssertTrue`, `AssertFalse`, `AssertEqual` sono affermazioni standard fornite da **unittest**. Ci sono altre affermazioni standard nel framework, e anche [affermazioni specifiche per Django](https://docs.djangoproject.com/en/5.0/topics/testing/tools/#assertions) per testare se una vista reindirizza (`assertRedirects`), per testare se un particolare template è stato utilizzato (`assertTemplateUsed`), ecc.

> [!NOTE]
> Normalmente non dovrebbe includere le funzioni **print()** nei suoi test come mostrato sopra. Lo facciamo qui solo perché possa vedere l'ordine in cui vengono chiamate le funzioni di configurazione nella console (nella sezione seguente).

## Come eseguire i test

Il modo più semplice per eseguire tutti i test è usare il comando:

```bash
python3 manage.py test
```

Questo scoprirà tutti i file denominati con il modello **test\*.py** nella directory corrente e eseguirà tutti i test definiti utilizzando classi base appropriate (qui abbiamo diversi file di test, ma solo **/catalog/tests/test_models.py** contiene attualmente dei test.) Per impostazione predefinita, i test segnaleranno individualmente solo sui fallimenti dei test, seguiti da un riepilogo del test.

> [!NOTE]
> Se riceve errori simili a: `ValueError: Missing staticfiles manifest entry...` potrebbe essere perché il test non esegue _collectstatic_ per impostazione predefinita e la sua app sta usando una classe di memorizzazione che lo richiede (vedi [manifest_strict](https://docs.djangoproject.com/en/5.0/ref/contrib/staticfiles/#django.contrib.staticfiles.storage.ManifestStaticFilesStorage.manifest_strict) per ulteriori informazioni). Ci sono diversi modi per superare questo problema - il più semplice è eseguire _collectstatic_ prima di eseguire i test:
>
> ```bash
> python3 manage.py collectstatic
> ```

Esegua i test nella directory principale di _LocalLibrary_. Dovrebbe vedere un output simile a quello seguente.

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

Qui vediamo che abbiamo avuto un fallimento di test e possiamo vedere esattamente quale funzione ha fallito e perché (questo fallimento è previsto, perché `False` non è `True`!).

> [!NOTE]
> La cosa più importante da imparare dall'output del test sopra è che è molto più prezioso se usa nomi descrittivi/informativi per i suoi oggetti e metodi.

L'output delle funzioni `print()` mostra come il metodo `setUpTestData()` viene chiamato una volta per la classe e `setUp()` viene chiamato prima di ogni metodo.
Ancora una volta, ricordi che normalmente non aggiungerebbe questo tipo di `print()` ai suoi test.

Le sezioni successive mostrano come può eseguire test specifici e come controllare la quantità di informazioni che i test visualizzano.

### Visualizzazione di ulteriori informazioni sui test

Se desidera ottenere più informazioni sull'esecuzione del test, può cambiare la _verbosità_. Ad esempio, per elencare i successi dei test oltre ai fallimenti (e una serie di informazioni su come è impostato il database di test) può impostare la verbosità a "2" come mostrato:

```bash
python3 manage.py test --verbosity 2
```

I livelli di verbosità consentiti sono 0, 1, 2 e 3, con il valore predefinito "1".

### Accelerare le cose

Se i suoi test sono indipendenti, su una macchina multiprocessore può accelerarli significativamente eseguendoli in parallelo.
L'uso di `--parallel auto` di seguito esegue un processo di test per ogni core disponibile.
L'`auto` è opzionale e può anche specificare un numero particolare di core da utilizzare.

```bash
python3 manage.py test --parallel auto
```

Per ulteriori informazioni, incluso cosa fare se i suoi test non sono indipendenti, vedi [DJANGO_TEST_PROCESSES](https://docs.djangoproject.com/en/5.0/ref/django-admin/#envvar-DJANGO_TEST_PROCESSES).

### Eseguire test specifici

Se desidera eseguire un sottoinsieme dei suoi test, può farlo specificando il percorso completo del pacchetto/i, modulo, sottoclasse `TestCase` o metodo:

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

### Altre opzioni del test runner

Il test runner fornisce molte altre opzioni, tra cui la possibilità di mescolare i test (`--shuffle`), eseguirli in modalità debug (`--debug-mode`) e utilizzare il logger Python per catturare i risultati.
Per ulteriori informazioni, consulti la documentazione del [test runner](https://docs.djangoproject.com/en/5.0/ref/django-admin/#test) di Django.

## Test LocalLibrary

Ora che sappiamo come eseguire i nostri test e quali cose dobbiamo testare, diamo un'occhiata ad alcuni esempi pratici.

> [!NOTE]
> Non scriveremo ogni possibile test, ma questo dovrebbe darle un'idea di come funzionano i test e cosa può fare di più.

### Modelli

Come discusso sopra, dovremmo testare tutto ciò che fa parte del nostro design o che è definito dal codice che abbiamo scritto, ma non le librerie/codice che sono già testate da Django o dal team di sviluppo Python.

Ad esempio, consideri il modello `Author` di seguito. Qui dovremmo testare le etichette per tutti i campi, perché anche se non ne abbiamo esplicitamente specificati la maggior parte, abbiamo un design che dice quali sono questi valori. Se non testiamo i valori, allora non sappiamo che le etichette dei campi hanno i valori previsti. Allo stesso modo, mentre ci fidiamo che Django creerà un campo della lunghezza specificata, è utile specificare un test per questa lunghezza per garantire che sia stata implementata come previsto.

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

Apro il nostro **/catalog/tests/test_models.py**, e sostituisco qualsiasi codice esistente con il seguente codice di test per il modello `Author`.

Qui vedrà che importiamo prima `TestCase` e deriviamo la nostra classe di test (`AuthorModelTest`) da essa, usando un nome descrittivo in modo da poter identificare facilmente eventuali test falliti nell'output del test. Quindi chiamiamo `setUpTestData()` per creare un oggetto autore che utilizzeremo ma non modificheremo in nessuno dei test.

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

I test sui campi controllano che i valori delle etichette dei campi (`verbose_name`) e che la dimensione dei campi di caratteri siano come previsto. Questi metodi hanno tutti nomi descrittivi e seguono lo stesso modello:

```python
# Get an author object to test
author = Author.objects.get(id=1)

# Get the metadata for the required field and use it to query the required field data
field_label = author._meta.get_field('first_name').verbose_name

# Compare the value to the expected result
self.assertEqual(field_label, 'first name')
```

Le cose interessanti da notare sono:

- Non possiamo ottenere il `verbose_name` direttamente usando `author.first_name.verbose_name`, perché `author.first_name` è una _stringa_ (non un puntatore all'oggetto `first_name` che possiamo usare per accedere alle sue proprietà). Invece, abbiamo bisogno di usare l'attributo `_meta` dell'autore per ottenere un'istanza del campo e utilizzarlo per ottenere ulteriori informazioni.
- Abbiamo scelto di usare `assertEqual(field_label,'first name')` piuttosto che `assertTrue(field_label == 'first name')`. La ragione di ciò è che se il test fallisce, l'output del primo ci dice qual era effettivamente l'etichetta, il che rende il debug del problema un po' più facile.

> [!NOTE]
> I test per le etichette `last_name` e `date_of_birth`, e anche il test per la lunghezza del campo `last_name` sono stati omessi. Aggiunga le sue versioni ora, seguendo le convenzioni di denominazione e gli approcci mostrati sopra.

Dobbiamo anche testare i nostri metodi personalizzati. Questi essenzialmente verificano che il nome dell'oggetto sia stato costruito come ci aspettavamo utilizzando il formato "Cognome", "Nome", e che l'URL che otteniamo per un elemento `Author` sia come ci aspetteremmo.

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

Esegua i test ora. Se ha creato il modello Author come descritto nel tutorial sui modelli, è molto probabile che otterrà un errore per l'etichetta `date_of_death` come mostrato di seguito. Il test fallisce perché è stato scritto aspettandosi che la definizione dell'etichetta seguisse la convenzione di Django di non capitalizzare la prima lettera dell'etichetta (Django lo fa per lei).

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

Questo è un bug molto minore, ma evidenzia come scrivere test possa più accuratamente controllare eventuali supposizioni che potrebbe aver fatto.

> [!NOTE]
> Cambi l'etichetta per il campo `date_of_death` (**/catalog/models.py**) in "died" e riesegu

i i test.

I modelli per testare gli altri modelli sono simili quindi non discuteremo ulteriormente. Sentiti libero di creare i suoi test per i nostri altri modelli.

### Moduli

La filosofia di testare i suoi moduli è la stessa di testare i suoi modelli; deve testare qualsiasi cosa abbia codificato o il suo design specifica, ma non il comportamento del framework sottostante e di altre librerie di terze parti.

Generalmente questo significa che dovrebbe testare che i moduli abbiano i campi che desidera e che vengano visualizzati con etichette e testi di aiuto appropriati. Non ha bisogno di verificare che Django convalidi correttamente il tipo di campo (a meno che non abbia creato il proprio campo personalizzato e la convalida) — cioè, non ha bisogno di testare che un campo email accetti solo email. Tuttavia, dovrebbe testare qualsiasi convalida aggiuntiva che si aspetta venga eseguita sui campi e qualsiasi messaggio che il suo codice genererà per gli errori.

Consideri il nostro modulo per il rinnovo dei libri. Questo ha solo un campo per la data di rinnovo, che avrà un'etichetta e un testo di aiuto che dovremo verificare.

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

Apro il nostro file **/catalog/tests/test_forms.py** e sostituisco qualsiasi codice esistente con il seguente codice di test per il modulo `RenewBookForm`. Iniziamo importando il nostro modulo e alcune librerie di Python e Django per aiutare a testare le funzionalità legate al tempo. Quindi dichiariamo la nostra classe di test del modulo allo stesso modo in cui abbiamo fatto per i modelli, utilizzando un nome descrittivo per la nostra classe di test derivata `TestCase`.

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

Le prime due funzioni testano che l'etichetta del campo e il testo di aiuto siano come previsto. Dobbiamo accedere al campo utilizzando il dizionario dei campi (ad esempio, `form.fields['renewal_date']`). Qui notiamo che dobbiamo anche testare se il valore dell'etichetta è `None`, perché anche se Django renderà l'etichetta corretta, ritorna `None` se il valore non è stato impostato _esplicitamente_.

Le restanti funzioni testano che il modulo sia valido per le date di rinnovo appena entro l'intervallo accettabile e non valido per i valori al di fuori dell'intervallo. Noti come costruiamo i valori di data di test attorno alla nostra data corrente (`datetime.date.today()`) usando `datetime.timedelta()` (in questo caso specificando un numero di giorni o settimane). Creiamo quindi semplicemente il modulo, passando i nostri dati, e testiamo se è valido.

> [!NOTE]
> Qui non utilizziamo effettivamente il database o il client di test. Consideri di modificare questi test per utilizzare [SimpleTestCase](https://docs.djangoproject.com/en/5.0/topics/testing/tools/#django.test.SimpleTestCase).
>
> Dobbiamo anche convalidare che vengano sollevati gli errori corretti se il modulo non è valido, tuttavia questo viene solitamente fatto come parte dell'elaborazione della vista, quindi ce ne occuperemo nella sezione successiva.

> [!WARNING]
> Se utilizza la classe [ModelForm](/it/docs/Learn_web_development/Extensions/Server-side/Django/Forms#modelforms) `RenewBookModelForm(forms.ModelForm)` invece della classe `RenewBookForm(forms.Form)`, allora il nome del campo del modulo sarebbe **'due_back'** invece di **'renewal_date'**.

Questo è tutto per i moduli; ne abbiamo alcuni altri, ma sono creati automaticamente dalle nostre viste di modifica basate su classi generiche, e dovrebbero essere testati lì! Esegua i test e confermi che il nostro codice è ancora valido!

### Viste

Per convalidare il comportamento della nostra vista, utilizziamo il test [Client](https://docs.djangoproject.com/en/5.0/topics/testing/tools/#django.test.Client) di Django. Questa classe si comporta come un browser web fittizio che possiamo utilizzare per simulare richieste `GET` e `POST` su un URL e osservare la risposta. Possiamo vedere quasi tutto sulla risposta, dal basso livello HTTP (intestazioni dei risultati e codici di stato) fino al modello che stiamo usando per renderizzare l'HTML e i dati di contesto che stiamo passando ad esso. Possiamo anche vedere la catena di reindirizzamenti (se presente) e controllare l'URL e il codice di stato a ogni passaggio. Questo ci consente di verificare che ogni vista stia facendo ciò che è previsto.

Iniziamo con una delle nostre viste più semplici, che fornisce un elenco di tutti gli autori. Questo viene visualizzato all'URL **/catalog/authors/** (un URL denominato 'authors' nella configurazione dell'URL).

```python
class AuthorListView(generic.ListView):
    model = Author
    paginate_by = 10
```

Poiché questa è una vista di elenco generica, quasi tutto viene fatto per noi da Django. Probabilmente, se si fida di Django, l'unica cosa che deve testare è che la vista sia accessibile all'URL corretto e possa essere accessibile usando il suo nome. Tuttavia, se sta utilizzando un processo di sviluppo guidato dai test, inizierà scrivendo test che confermano che la vista visualizzi tutti gli autori, paginandoli in gruppi di 10.

Apro il file **/catalog/tests/test_views.py** e sostituisco qualsiasi testo esistente con il seguente codice di test per `AuthorListView`. Come prima, importiamo il nostro modello e alcune classi utili. Nel metodo `setUpTestData()` impostiamo un certo numero di oggetti `Author` in modo da poter testare la nostra paginazione.

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

Tutti i test utilizzano il client (appartenente alla nostra classe derivata `TestCase`) per simulare una richiesta `GET` e ottenere una risposta. La prima versione controlla un URL specifico (nota, solo il percorso specifico senza il dominio) mentre la seconda genera l'URL dal suo nome nella configurazione dell'URL.

```python
response = self.client.get('/catalog/authors/')
response = self.client.get(reverse('authors'))
```

Una volta che abbiamo la risposta, la interroghiamo per il suo codice di stato, il modello utilizzato, se la risposta è paginata o meno, il numero di elementi restituiti e il numero totale di elementi.

> [!NOTE]
> Se imposta la variabile `paginate_by` nel suo file **/catalog/views.py** su un numero diverso da 10, si assicuri di aggiornare le righe che testano che il numero corretto di elementi siano visualizzati nei modelli paginati sopra e nelle sezioni seguenti. Ad esempio, se imposta la variabile per la pagina dell'elenco degli autori su 5, aggiorni la riga sopra a:
>
> ```python
> self.assertTrue(len(response.context['author_list']) == 5)
> ```

La variabile più interessante che abbiamo dimostrato sopra è `response.context`, che è la variabile di contesto passata al modello dalla vista.
Questo è incredibilmente utile per i test, perché ci consente di confermare che il nostro modello stia ottenendo tutti i dati di cui ha bisogno. In altre parole possiamo verificare che stiamo usando il modello previsto e quali dati stia ottenendo il modello, il che va lontano nel verificare che eventuali problemi di rendering siano dovuti solo al modello.

#### Viste che sono riservate agli utenti connessi

In alcuni casi potrebbe voler testare una vista riservata solo agli utenti connessi. Ad esempio, la nostra `LoanedBooksByUserListView` è molto simile alla vista precedente ma è disponibile solo per gli utenti connessi e visualizza solo i record `BookInstance` presi in prestito dall'utente corrente, con stato "in prestito" e ordinati "dal più vecchio al più recente".

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

Aggiunge il seguente codice di test a **/catalog/tests/test_views.py**. Qui prima utilizziamo `SetUp()` per creare alcuni account di accesso utente e oggetti `BookInstance` (insieme ai libri associati e altri record) che useremo successivamente nei test. Metà dei libri sono presi in prestito da ogni utente di prova, ma inizialmente abbiamo impostato lo stato di tutti i libri su "manutenzione". Abbiamo usato `SetUp()` piuttosto che `setUpTestData()` perché modificheremo alcuni di questi oggetti più avanti.

> [!NOTE]
> Il codice `setUp()` qui sotto crea un libro con una `Language` specificata, ma _il suo_ codice potrebbe non includere il modello `Language` in quanto questo è stato creato come una _sfida_. Se è così, commenti le parti di codice che creano o importano oggetti Language. Dovrebbe farlo anche nella sezione `RenewBookInstancesViewTest` che segue.

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

Per verificare che la vista reindirizzi a una pagina di accesso se l'utente non è connesso, utilizziamo `assertRedirects`, come dimostrato in `test_redirect_if_not_logged_in()`. Per verificare che la pagina sia visualizzata per un utente connesso, accediamo al nostro utente di prova e poi accediamo di nuovo alla pagina e controlliamo che otteniamo un `status_code` di 200 (successo).

Il resto dei test verifica che la nostra vista ritorni solo i libri che sono in prestito al nostro attuale mutuatario. Copi il codice qui sotto e incollalo alla fine della classe di test sopra.

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

Può anche aggiungere test di paginazione, se lo desidera!

#### Testing delle viste con moduli

Il test delle viste con moduli è un po' più complicato rispetto ai casi precedenti, perché è necessario testare più percorsi di codice: visualizzazione iniziale, visualizzazione dopo che la convalida dei dati è fallita e visualizzazione dopo che la convalida è riuscita. La buona notizia è che usiamo il client per il test in quasi lo stesso modo in cui abbiamo fatto per le viste solo di visualizzazione.

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

Dovremo testare che la vista sia disponibile solo per gli utenti che hanno il permesso `can_mark_returned`, e che gli utenti vengano reindirizzati a una pagina di errore HTTP 404 se tentano di rinnovare un `BookInstance` che non esiste. Dovremmo controllare che il valore iniziale del modulo sia prefissato con una data a tre settimane nel futuro e che se la convalida riesce venga reindirizzato alla vista "tutti i libri presi in prestito". Come parte del controllo dei test di convalida fallita, controlleremo anche che il nostro modulo stia inviando i messaggi di errore appropriati.

Aggiunga la prima parte della classe di test (mostrata di seguito) alla fine di **/catalog/tests/test_views.py**.
Questo crea due utenti e due istanze di libro, ma solo uno degli utenti ha il permesso richiesto per accedere alla vista.

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

Aggiunge i seguenti test alla fine della classe di test. Questi verificano che solo gli utenti con le autorizzazioni appropriate (_testuser2_) possano accedere alla vista. Controlliamo tutti i casi: quando l'utente non è connesso, quando un utente è connesso ma non ha le autorizzazioni corrette, quando l'utente ha le autorizzazioni ma non è il mutuatario (dovrebbe riuscire), e cosa succede quando cercano di accedere a un `BookInstance` che non esiste. Controlliamo anche che venga utilizzato il modello corretto.

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

Aggiunga il prossimo metodo di test, come mostrato di seguito. Questo controlla che la data iniziale per il modulo sia tre settimane in futuro. Nota come siamo in grado di accedere al valore del valore iniziale del campo del modulo (`response.context['form'].initial['renewal_date']`).

```python
    def test_form_renewal_date_initially_has_date_three_weeks_in_future(self):
        login = self.client.login(username='testuser2', password='2HJ1vRV0Z&3iD')
        response = self.client.get(reverse('renew-book-librarian', kwargs={'pk': self.test_bookinstance1.pk}))
        self.assertEqual(response.status_code, 200)

        date_3_weeks_in_future = datetime.date.today() + datetime.timedelta(weeks=3)
        self.assertEqual(response.context['form'].initial['renewal_date'], date_3_weeks_in_future)
```

Il prossimo test (aggiunga questo alla classe anche) verifica che la vista reindirizzi a un elenco di tutti i libri presi in prestito se il rinnovo riesce. Ciò che differisce qui è che per la prima volta mostriamo come può eseguire una `POST` di dati utilizzando il client. I _data_ di post sono il secondo argomento della funzione post e sono specificati come un dizionario di chiavi/valori.

```python
    def test_redirects_to_all_borrowed_book_list_on_success(self):
        login = self.client.login(username='testuser2', password='2HJ1vRV0Z&3iD')
        valid_date_in_future = datetime.date.today() + datetime.timedelta(weeks=2)
        response = self.client.post(reverse('renew-book-librarian', kwargs={'pk':self.test_bookinstance1.pk,}), {'renewal_date':valid_date_in_future})
        self.assertRedirects(response, reverse('all-borrowed'))
```

> [!WARNING]
> La vista _all-borrowed_ è stata aggiunta come una _sfida_ e il suo codice potrebbe invece reindirizzare alla home page '/'. Se è così, modifichi le ultime due righe del codice di test per essere simili al codice seguente. Il `follow=True` nella richiesta garantisce che la richiesta ritorni l'URL di destinazione finale (quindi controllando `/catalog/` piuttosto che `/`).
>
> ```python
>  response = self.client.post(reverse('renew-book-librarian', kwargs={'pk':self.test_bookinstance1.pk,}), {'renewal_date':valid_date_in_future}, follow=True)
>  self.assertRedirects(response, '/catalog/')
> ```

Copi le ultime due funzioni nella classe, come visto sotto. Questi controllano ancora le richieste `POST`, ma in questo caso con date di rinnovo non valide. Usiamo `assertFormError()` per verificare che i messaggi di errore siano come previsti.

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

Le stesse tecniche possono essere utilizzate per testare l'altra vista.

### Template

Django fornisce API di test per verificare che il modello corretto venga chiamato dalle sue viste e per permetterle di verificare che le informazioni corrette siano inviate. Tuttavia, non c'è supporto API specifico per il testing in Django che il suo output HTML venga renderizzato come previsto.

## Altri strumenti di test raccomandati

Il framework di test di Django può aiutarla a scrivere test unitari e di integrazione efficaci - abbiamo solo scalfito la superficie di ciò che il framework **unittest** sottostante può fare, per non parlare delle aggiunte di Django (ad esempio, scopra come può utilizzare [unittest.mock](https://docs.python.org/3/library/unittest.mock-examples.html) per modificare le librerie di terze parti in modo da poter testare più a fondo il suo codice).

Mentre ci sono numerosi altri strumenti di test che può utilizzare, evidenzieremo solo due:

- [Coverage](https://coverage.readthedocs.io/en/latest/): Questo strumento Python riporta quanto del suo codice è effettivamente eseguito dai suoi test. È particolarmente utile quando sta iniziando e sta cercando di capire esattamente cosa dovrebbe testare.
- [Selenium](/it/docs/Learn_web_development/Extensions/Testing/Your_own_automation_environment) è un framework per automatizzare i test in un browser reale. Le consente di simulare un utente reale che interagisce con il sito e fornisce un ottimo framework per testare il suo sito a livello di sistema (il passo successivo rispetto al testing di integrazione).

## Mettiti alla prova

Ci sono molti altri modelli e viste che possiamo testare. Come una sfida, provi a creare un caso di test per la vista `AuthorCreate`.

```python
class AuthorCreate(PermissionRequiredMixin, CreateView):
    model = Author
    fields = ['first_name', 'last_name', 'date_of_birth', 'date_of_death']
    initial = {'date_of_death': '11/11/2023'}
    permission_required = 'catalog.add_author'
```

Ricordi che deve controllare qualsiasi cosa specifichi o che faccia parte del design.
Questo includerà chi ha accesso, la data iniziale, il modello utilizzato e dove il reindirizzamento della vista avviene al successo.

Potrebbe usare il seguente codice per configurare il suo test e assegnare all'utente il permesso appropriato

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

## Riepilogo

Scrivere codice di test non è né divertente né affascinante ed è quindi spesso lasciato per ultimo (o non fatto affatto) quando si crea un sito web. Tuttavia, è una parte essenziale per assicurarsi che il suo codice sia sicuro da rilasciare dopo aver apportato modifiche ed efficace nei costi di mantenimento.

In questo tutorial abbiamo mostrato come scrivere ed eseguire test per i suoi modelli, moduli e viste. Soprattutto, abbiamo fornito un breve riassunto di cosa dovrebbe testare, che è spesso la cosa più difficile da capire quando si inizia. C'è molto altro da sapere, ma anche con ciò che ha già imparato dovrebbe essere in grado di creare test unitari efficaci per i suoi siti web.

Il prossimo e ultimo tutorial mostra come può distribuire il suo meraviglioso (e completamente testato!) sito web Django.

## Vedi anche

- [Scrivere ed eseguire test](https://docs.djangoproject.com/en/5.0/topics/testing/overview/) (Django docs)
- [Scrivere la sua prima app Django, parte 5 > Introduzione ai test automatizzati](https://docs.djangoproject.com/en/5.0/intro/tutorial05/) (Django docs)
- [Riferimento sugli strumenti di test](https://docs.djangoproject.com/en/5.0/topics/testing/tools/) (Django docs)
- [Argomenti avanzati sui test](https://docs.djangoproject.com/en/5.0/topics/testing/advanced/) (Django docs)
- [Una Guida ai Test in Django](https://toastdriven.com/blog/2011/apr/09/guide-to-testing-in-django/) (Toast Driven Blog, 2011)
- [Workshop: Test-Driven Web Development con Django](https://test-driven-django-development.readthedocs.io/en/latest/index.html) (San Diego Python, 2014)
- [Testing in Django (Parte 1) - Best Practices e Esempi](https://realpython.com/testing-in-django-part-1-best-practices-and-examples/) (RealPython, 2013)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Forms", "Learn_web_development/Extensions/Server-side/Django/Deployment", "Learn_web_development/Extensions/Server-side/Django")}}
