---
title: "Tutorial Django - Parte 10: testare un'applicazione web Django"
short-title: "10: Test"
slug: Learn_web_development/Extensions/Server-side/Django/Testing
l10n:
  sourceCommit: f4c14731a1a157fc8d8f7357ac4d74d14a7d7fb5
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Forms", "Learn_web_development/Extensions/Server-side/Django/Deployment", "Learn_web_development/Extensions/Server-side/Django")}}

Man mano che i siti web crescono, diventa più difficile testarli manualmente. Non solo ci sono più elementi da testare, ma, poiché le interazioni tra componenti diventano più complesse, una piccola modifica in un'area può influire su altre aree. Saranno quindi necessarie ulteriori modifiche per assicurarsi che tutto continui a funzionare e che non vengano introdotti errori man mano che si effettuano altre modifiche. Un modo per attenuare questi problemi consiste nello scrivere test automatizzati, che possono essere eseguiti facilmente e in modo affidabile ogni volta che viene apportata una modifica. Questo tutorial mostra come automatizzare gli _unit test_ del sito web usando il framework di test di Django.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Completare tutti gli argomenti dei tutorial precedenti, incluso <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/Forms">Tutorial Django - Parte 9: lavorare con i moduli</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Comprendere come scrivere unit test per siti web basati su Django.</td>
    </tr>
  </tbody>
</table>

## Panoramica

La [biblioteca locale](/it/docs/Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website) dispone attualmente di pagine per visualizzare elenchi di tutti i libri e gli autori, viste dettagliate per gli elementi `Book` e `Author`, una pagina per rinnovare elementi `BookInstance` e pagine per creare, aggiornare ed eliminare elementi `Author` (e anche record `Book`, se è stata completata la _sfida_ nel [tutorial sui moduli](/it/docs/Learn_web_development/Extensions/Server-side/Django/Forms)). Anche con questo sito relativamente piccolo, navigare manualmente verso ogni pagina e verificare _superficialmente_ che tutto funzioni come previsto può richiedere diversi minuti. Man mano che si apportano modifiche e il sito cresce, il tempo richiesto per verificare manualmente che tutto funzioni "correttamente" non potrà che aumentare. Continuando in questo modo, si finirebbe per dedicare la maggior parte del tempo ai test e pochissimo tempo al miglioramento del codice.

I test automatizzati possono davvero aiutare a risolvere questo problema. I vantaggi più evidenti sono che possono essere eseguiti molto più velocemente dei test manuali, possono verificare un livello di dettaglio molto maggiore e testano esattamente la stessa funzionalità ogni volta (i tester umani non sono neanche lontanamente altrettanto affidabili). Poiché sono rapidi, i test automatizzati possono essere eseguiti più regolarmente e, se un test fallisce, indicano esattamente dove il codice non si comporta come previsto.

Inoltre, i test automatizzati possono fungere da primo vero "utente" del codice, costringendo a essere rigorosi nel definire e documentare il comportamento atteso del sito web. Spesso costituiscono la base per esempi di codice e documentazione. Per questi motivi, alcuni processi di sviluppo software iniziano con la definizione e l'implementazione dei test, dopo le quali il codice viene scritto per soddisfare il comportamento richiesto (ad esempio, lo sviluppo [guidato dai test](https://en.wikipedia.org/wiki/Test-driven_development) e [guidato dal comportamento](https://en.wikipedia.org/wiki/Behavior-driven_development)).

Questo tutorial mostra come scrivere test automatizzati per Django, aggiungendo vari test al sito web _LocalLibrary_.

### Tipi di test

Esistono numerosi tipi, livelli e classificazioni di test e approcci al testing. I test automatizzati più importanti sono:

- Unit test
  - : Verificano il comportamento funzionale dei singoli componenti, spesso a livello di classi e funzioni.
- Test di regressione
  - : Test che riproducono bug storici. Ogni test viene inizialmente eseguito per verificare che il bug sia stato corretto, quindi rieseguito per assicurarsi che non sia stato reintrodotto a seguito di successive modifiche al codice.
- Test di integrazione
  - : Verificano il funzionamento di gruppi di componenti quando vengono usati insieme. I test di integrazione conoscono le interazioni richieste tra i componenti, ma non necessariamente le operazioni interne di ogni componente. Possono coprire semplici gruppi di componenti fino all'intero sito web.

> [!NOTE]
> Altri tipi comuni di test includono test black box, white box, manuali, automatizzati, canary, smoke, di conformità, di accettazione, funzionali, di sistema, di prestazioni, di carico e di stress. Consultare ulteriori fonti per maggiori informazioni.

### Cosa fornisce Django per i test?

Testare un sito web è un'attività complessa, perché è composto da diversi livelli di logica: dalla gestione delle richieste a livello HTTP, alle query dei modelli, alla convalida e all'elaborazione dei moduli, fino al rendering dei template.

Django fornisce un framework di test con una piccola gerarchia di classi che si basa sulla libreria standard Python [`unittest`](https://docs.python.org/3/library/unittest.html#module-unittest). Nonostante il nome, questo framework di test è adatto sia agli unit test sia ai test di integrazione. Il framework Django aggiunge metodi API e strumenti per facilitare il test del comportamento specifico del web e di Django. Questi consentono di simulare richieste, inserire dati di test e ispezionare l'output dell'applicazione. Django fornisce inoltre un'API ([LiveServerTestCase](https://docs.djangoproject.com/en/5.0/topics/testing/tools/#liveservertestcase)) e strumenti per [usare framework di test diversi](https://docs.djangoproject.com/en/5.0/topics/testing/advanced/#other-testing-frameworks); ad esempio, è possibile integrare il popolare framework [Selenium](/it/docs/Learn_web_development/Extensions/Testing/Your_own_automation_environment) per simulare un utente che interagisce con un browser attivo.

Per scrivere un test, si deriva da una delle classi base di test di Django (o di _unittest_) ([SimpleTestCase](https://docs.djangoproject.com/en/5.0/topics/testing/tools/#simpletestcase), [TransactionTestCase](https://docs.djangoproject.com/en/5.0/topics/testing/tools/#transactiontestcase), [TestCase](https://docs.djangoproject.com/en/5.0/topics/testing/tools/#testcase), [LiveServerTestCase](https://docs.djangoproject.com/en/5.0/topics/testing/tools/#liveservertestcase)) e si scrivono quindi metodi separati per verificare che funzionalità specifiche funzionino come previsto (i test usano metodi "assert" per verificare che le espressioni producano valori `True` o `False`, oppure che due valori siano uguali e così via). Quando viene avviata l'esecuzione dei test, il framework esegue i metodi di test scelti nelle classi derivate. I metodi di test vengono eseguiti indipendentemente, con il comportamento comune di configurazione e/o pulizia definito nella classe, come mostrato di seguito.

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

La migliore classe base per la maggior parte dei test è [django.test.TestCase](https://docs.djangoproject.com/en/5.0/topics/testing/tools/#testcase). Questa classe di test crea un database pulito prima dell'esecuzione dei test ed esegue ogni funzione di test nella propria transazione. La classe possiede anche un [Client](https://docs.djangoproject.com/en/5.0/topics/testing/tools/#django.test.Client) di test che può essere usato per simulare un utente che interagisce con il codice a livello di vista. Nelle sezioni seguenti ci concentreremo sugli unit test, creati usando questa classe base [TestCase](https://docs.djangoproject.com/en/5.0/topics/testing/tools/#testcase).

> [!NOTE]
> La classe [django.test.TestCase](https://docs.djangoproject.com/en/5.0/topics/testing/tools/#testcase) è molto comoda, ma può rendere alcuni test più lenti del necessario, poiché non ogni test dovrà configurare il proprio database o simulare l'interazione con la vista. Una volta acquisita familiarità con ciò che è possibile fare con questa classe, potrebbe essere opportuno sostituire alcuni test con le classi di test più semplici disponibili.

### Cosa dovrebbe essere testato?

Dovrebbero essere testati tutti gli aspetti del proprio codice, ma non le librerie o le funzionalità fornite da Python o Django.

Si consideri, ad esempio, il modello `Author` definito di seguito. Non è necessario testare esplicitamente che `first_name` e `last_name` siano stati memorizzati correttamente come `CharField` nel database, poiché questo è definito da Django, anche se nella pratica questa funzionalità verrà inevitabilmente testata durante lo sviluppo. Non è nemmeno necessario testare che `date_of_birth` sia stato convalidato come campo data, poiché anche questa è una funzionalità implementata in Django.

Tuttavia, è necessario controllare il testo usato per le etichette (_First name, Last name, Date of birth, Died_) e la dimensione del campo assegnato al testo (_100 caratteri_), poiché fanno parte del progetto e potrebbero essere modificati o danneggiati in futuro.

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

Analogamente, è necessario verificare che i metodi personalizzati `get_absolute_url()` e `__str__()` si comportino come richiesto, poiché costituiscono il proprio codice o la propria logica di business. Nel caso di `get_absolute_url()`, è possibile confidare che il metodo Django `reverse()` sia implementato correttamente; ciò che viene testato è quindi che la vista associata sia stata effettivamente definita.

> [!NOTE]
> I lettori più attenti potrebbero osservare che sarebbe opportuno anche limitare le date di nascita e di morte a valori sensati e verificare che la morte avvenga dopo la nascita.
> In Django questo vincolo verrebbe aggiunto alle classi di moduli. Sebbene sia possibile definire validator per i campi del modello e validator del modello, questi vengono usati a livello di modulo solo se chiamati dal metodo `clean()` del modello. Ciò richiede un `ModelForm`, oppure il metodo `clean()` del modello deve essere chiamato in modo specifico.

Tenendo presente questo, iniziamo a esaminare come definire ed eseguire i test.

## Panoramica della struttura dei test

Prima di entrare nei dettagli di "cosa testare", vediamo brevemente _dove_ e _come_ vengono definiti i test.

Django usa la [rilevazione dei test integrata](https://docs.python.org/3/library/unittest.html#unittest-test-discovery) del modulo unittest, che rileva i test nella directory di lavoro corrente in qualsiasi file il cui nome corrisponda al pattern **test\*.py**. A condizione di assegnare nomi appropriati ai file, è possibile usare qualsiasi struttura. Si consiglia di creare un modulo per il codice di test e di avere file separati per modelli, viste, moduli e qualsiasi altro tipo di codice da testare. Ad esempio:

```plain
catalog/
  /tests/
    __init__.py
    test_models.py
    test_forms.py
    test_views.py
```

Creare una struttura di file come quella mostrata sopra nel progetto _LocalLibrary_. Il file **\_\_init\_\_.py** deve essere vuoto, poiché indica a Python che la directory è un pacchetto. È possibile creare i tre file di test copiando e rinominando il file di test scheletro **/catalog/tests.py**.

> [!NOTE]
> Il file di test scheletro **/catalog/tests.py** è stato creato automaticamente quando è stato [creato il sito web scheletro Django](/it/docs/Learn_web_development/Extensions/Server-side/Django/skeleton_website). È perfettamente lecito inserire tutti i test al suo interno, ma testando correttamente ci si ritroverà rapidamente con un file di test molto grande e difficile da gestire.
>
> Eliminare il file scheletro, poiché non sarà necessario.

Aprire **/catalog/tests/test_models.py**. Il file dovrebbe importare `django.test.TestCase`, come mostrato:

```python
from django.test import TestCase

# Create your tests here.
```

Spesso viene aggiunta una classe di test per ogni modello, vista o modulo da testare, con metodi individuali per testare funzionalità specifiche. In altri casi può essere utile avere una classe separata per testare uno specifico caso d'uso, con singole funzioni di test che verificano aspetti di quel caso d'uso, ad esempio una classe per verificare che un campo di un modello sia convalidato correttamente, con funzioni per testare ciascun possibile caso di errore. Anche in questo caso, la struttura dipende dalla scelta adottata, ma è preferibile essere coerenti.

Aggiungere la classe di test seguente alla fine del file. La classe mostra come costruire una classe di test derivando da `TestCase`.

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

La nuova classe definisce due metodi che possono essere usati per la configurazione precedente ai test, ad esempio per creare modelli o altri oggetti necessari per il test:

- `setUpTestData()` viene chiamato una volta all'inizio dell'esecuzione dei test per la configurazione a livello di classe. Va usato per creare oggetti che non verranno modificati o cambiati in nessuno dei metodi di test.
- `setUp()` viene chiamato prima di ogni funzione di test per configurare gli oggetti che potrebbero essere modificati dal test. Ogni funzione di test riceverà una versione "nuova" di questi oggetti.

> [!NOTE]
> Le classi di test dispongono anche di un metodo `tearDown()`, che qui non è stato usato. Questo metodo non è particolarmente utile per i test del database, poiché la classe base `TestCase` si occupa della pulizia del database.

Sotto questi sono presenti vari metodi di test che usano funzioni `assert` per verificare se le condizioni sono vere, false o uguali (`assertTrue`, `assertFalse`, `assertEqual`). Se la condizione non viene valutata come previsto, il test fallirà e segnalerà l'errore nella console.

`assertTrue`, `assertFalse` e `assertEqual` sono asserzioni standard fornite da **unittest**. Nel framework sono disponibili altre asserzioni standard, nonché [asserzioni specifiche di Django](https://docs.djangoproject.com/en/5.0/topics/testing/tools/#assertions) per verificare se una vista reindirizza (`assertRedirects`), se è stato usato un particolare template (`assertTemplateUsed`) e così via.

> [!NOTE]
> Normalmente **non** dovrebbero essere incluse funzioni **print()** nei test come mostrato sopra. Qui vengono usate solo per mostrare nella console l'ordine in cui vengono chiamate le funzioni di configurazione, nella sezione seguente.

## Come eseguire i test

Il modo più semplice per eseguire tutti i test consiste nell'usare il comando:

```bash
python3 manage.py test
```

Questo rileverà tutti i file il cui nome corrisponde al pattern **test\*.py** nella directory corrente ed eseguirà tutti i test definiti usando classi base appropriate. Qui sono presenti vari file di test, ma al momento solo **/catalog/tests/test_models.py** contiene dei test. Per impostazione predefinita, i test riportano singolarmente solo gli errori, seguiti da un riepilogo dei test.

> [!NOTE]
> Se si ottengono errori simili a `ValueError: Missing staticfiles manifest entry...`, ciò potrebbe dipendere dal fatto che il testing non esegue _collectstatic_ per impostazione predefinita e l'app usa una classe di storage che lo richiede. Per maggiori informazioni, vedere [manifest_strict](https://docs.djangoproject.com/en/5.0/ref/contrib/staticfiles/#django.contrib.staticfiles.storage.ManifestStaticFilesStorage.manifest_strict). Esistono diversi modi per risolvere il problema; il più semplice consiste nell'eseguire _collectstatic_ prima di eseguire i test:
>
> ```bash
> python3 manage.py collectstatic
> ```

Eseguire i test nella directory radice di _LocalLibrary_. Dovrebbe essere visualizzato un output simile a quello seguente.

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

Qui si vede che è presente un errore di test ed è possibile vedere esattamente quale funzione è fallita e perché. Questo errore è previsto, poiché `False` non è `True`.

> [!NOTE]
> La cosa più importante da apprendere dall'output dei test mostrato sopra è che è molto più utile usare nomi descrittivi e informativi per oggetti e metodi.

L'output delle funzioni `print()` mostra come il metodo `setUpTestData()` venga chiamato una volta per la classe e `setUp()` venga chiamato prima di ogni metodo. Anche in questo caso, normalmente questo tipo di `print()` non verrebbe aggiunto ai test.

Le sezioni successive mostrano come eseguire test specifici e come controllare la quantità di informazioni visualizzate dai test.

### Mostrare più informazioni sui test

Per ottenere maggiori informazioni sull'esecuzione dei test è possibile modificare la _verbosità_. Ad esempio, per elencare i test riusciti oltre a quelli falliti, e molte informazioni su come viene configurato il database di test, è possibile impostare la verbosità su "2", come mostrato:

```bash
python3 manage.py test --verbosity 2
```

I livelli di verbosità consentiti sono 0, 1, 2 e 3; il valore predefinito è "1".

### Velocizzare l'esecuzione

Se i test sono indipendenti, in una macchina multiprocessore è possibile velocizzarli significativamente eseguendoli in parallelo. L'uso di `--parallel auto` riportato di seguito esegue un processo di test per ogni core disponibile. `auto` è facoltativo ed è possibile anche specificare un numero particolare di core da usare.

```bash
python3 manage.py test --parallel auto
```

Per maggiori informazioni, incluso cosa fare se i test non sono indipendenti, vedere [DJANGO_TEST_PROCESSES](https://docs.djangoproject.com/en/5.0/ref/django-admin/#envvar-DJANGO_TEST_PROCESSES).

### Eseguire test specifici

Per eseguire un sottoinsieme dei test, è possibile specificare il percorso completo separato da punti al pacchetto, modulo, sottoclasse `TestCase` o metodo:

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

Il test runner offre molte altre opzioni, inclusa la possibilità di mescolare i test (`--shuffle`), eseguirli in modalità debug (`--debug-mode`) e usare il logger Python per acquisire i risultati. Per maggiori informazioni, vedere la documentazione del [test runner](https://docs.djangoproject.com/en/5.0/ref/django-admin/#test) Django.

## Test di LocalLibrary

Ora che è noto come eseguire i test e quali tipi di elementi devono essere testati, vediamo alcuni esempi pratici.

> [!NOTE]
> Non verranno scritti tutti i test possibili, ma questo dovrebbe fornire un'idea di come funzionano i test e di cos'altro è possibile fare.

### Modelli

Come discusso sopra, è necessario testare tutto ciò che fa parte del progetto o che è definito dal codice scritto autonomamente, ma non le librerie o il codice già testato da Django o dal team di sviluppo Python.

Si consideri, ad esempio, il modello `Author` seguente. In questo caso occorre testare le etichette di tutti i campi perché, anche se la maggior parte di esse non è stata specificata esplicitamente, esiste un progetto che definisce quali dovrebbero essere questi valori. Se non si testano tali valori, non si sa se le etichette dei campi hanno i valori previsti. Analogamente, pur confidando che Django crei un campo della lunghezza specificata, è utile specificare un test per questa lunghezza per assicurarsi che sia stata implementata come pianificato.

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

Aprire **/catalog/tests/test_models.py** e sostituire qualsiasi codice esistente con il seguente codice di test per il modello `Author`.

Qui è possibile vedere che viene prima importato `TestCase` e che la classe di test (`AuthorModelTest`) deriva da essa, usando un nome descrittivo per identificare facilmente eventuali test falliti nell'output. Viene quindi chiamato `setUpTestData()` per creare un oggetto autore che sarà usato, ma non modificato, in nessuno dei test.

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

I test dei campi verificano che i valori delle etichette dei campi (`verbose_name`) e la dimensione dei campi carattere siano quelli previsti. Questi metodi hanno tutti nomi descrittivi e seguono lo stesso schema:

```python
# Get an author object to test
author = Author.objects.get(id=1)

# Get the metadata for the required field and use it to query the required field data
field_label = author._meta.get_field('first_name').verbose_name

# Compare the value to the expected result
self.assertEqual(field_label, 'first name')
```

Gli aspetti interessanti da osservare sono:

- Non è possibile ottenere direttamente `verbose_name` usando `author.first_name.verbose_name`, poiché `author.first_name` è una _stringa_, non un handle all'oggetto `first_name` che può essere usato per accedere alle sue proprietà. È invece necessario usare l'attributo `_meta` dell'autore per ottenere un'istanza del campo e usarla per richiedere le informazioni aggiuntive.
- È stato scelto di usare `assertEqual(field_label,'first name')` anziché `assertTrue(field_label == 'first name')`. Il motivo è che, se il test fallisce, l'output della prima variante indica quale fosse effettivamente l'etichetta, rendendo il debug del problema leggermente più semplice.

> [!NOTE]
> I test per le etichette `last_name` e `date_of_birth`, oltre al test per la lunghezza del campo `last_name`, sono stati omessi. Aggiungere ora le proprie versioni seguendo le convenzioni di denominazione e gli approcci mostrati sopra.

È inoltre necessario testare i metodi personalizzati. Questi verificano essenzialmente che il nome dell'oggetto sia stato costruito come previsto usando il formato "Last Name", "First Name" e che l'URL ottenuto per un elemento `Author` sia quello atteso.

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

Eseguire ora i test. Se il modello Author è stato creato come descritto nel tutorial sui modelli, è molto probabile che venga restituito un errore per l'etichetta `date_of_death`, come mostrato di seguito. Il test fallisce perché è stato scritto aspettandosi che la definizione dell'etichetta segua la convenzione Django di non usare la maiuscola nella prima lettera dell'etichetta; Django lo fa automaticamente.

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

Si tratta di un bug molto minore, ma evidenzia come la scrittura dei test possa verificare più approfonditamente le eventuali supposizioni effettuate.

> [!NOTE]
> Modificare l'etichetta per il campo `date_of_death` (**/catalog/models.py**) in "died" e rieseguire i test.

Gli schemi per testare gli altri modelli sono simili, quindi non verranno discussi ulteriormente. È possibile creare autonomamente test per gli altri modelli.

### Moduli

La filosofia per testare i moduli è la stessa adottata per i modelli: è necessario testare tutto ciò che è stato programmato o che è specificato dal progetto, ma non il comportamento del framework sottostante e delle altre librerie di terze parti.

In generale, ciò significa che è necessario verificare che i moduli abbiano i campi desiderati e che questi siano visualizzati con etichette e testo di aiuto appropriati. Non è necessario verificare che Django convalidi correttamente il tipo di campo, a meno che non sia stato creato un campo e una convalida personalizzati. Ad esempio, non è necessario testare che un campo email accetti solo email. Tuttavia, è necessario testare qualsiasi convalida aggiuntiva che debba essere eseguita sui campi e qualsiasi messaggio che il codice genererà per gli errori.

Si consideri il modulo per il rinnovo dei libri. Ha un solo campo per la data di rinnovo, con un'etichetta e un testo di aiuto che dovranno essere verificati.

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

Aprire il file **/catalog/tests/test_forms.py** e sostituire qualsiasi codice esistente con il seguente codice di test per il modulo `RenewBookForm`. Si inizia importando il modulo e alcune librerie Python e Django che aiutano a testare funzionalità legate al tempo. Viene quindi dichiarata la classe di test del modulo nello stesso modo adottato per i modelli, usando un nome descrittivo per la classe di test derivata da `TestCase`.

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

Le prime due funzioni verificano che `label` e `help_text` del campo siano quelli previsti. È necessario accedere al campo attraverso il dizionario dei campi, ad esempio `form.fields['renewal_date']`. Si noti che occorre verificare anche se il valore dell'etichetta è `None`, perché, anche se Django visualizza l'etichetta corretta, restituisce `None` se il valore non è impostato _esplicitamente_.

Le restanti funzioni verificano che il modulo sia valido per date di rinnovo appena entro l'intervallo accettabile e non valido per valori fuori dall'intervallo. Si noti come vengano costruiti valori di data di test attorno alla data corrente (`datetime.date.today()`) usando `datetime.timedelta()`, in questo caso specificando un numero di giorni o settimane. Viene quindi creato il modulo passando i dati e verificandone la validità.

> [!NOTE]
> Qui non vengono effettivamente usati il database né il client di test. Considerare di modificare questi test per usare [SimpleTestCase](https://docs.djangoproject.com/en/5.0/topics/testing/tools/#django.test.SimpleTestCase).
>
> È inoltre necessario convalidare che vengano sollevati gli errori corretti se il modulo non è valido; tuttavia, ciò viene normalmente eseguito come parte dell'elaborazione della vista, quindi verrà affrontato nella sezione successiva.

> [!WARNING]
> Se viene usata la classe [ModelForm](/it/docs/Learn_web_development/Extensions/Server-side/Django/Forms#modelforms) `RenewBookModelForm(forms.ModelForm)` anziché la classe `RenewBookForm(forms.Form)`, il nome del campo del modulo sarà **'due_back'** anziché **'renewal_date'**.

Questo è tutto per i moduli. Ce ne sono altri, ma vengono creati automaticamente dalle viste generiche di modifica basate su classi e dovrebbero essere testati lì. Eseguire i test e confermare che il codice continui a superarli.

### Viste

Per convalidare il comportamento delle viste si usa il [Client](https://docs.djangoproject.com/en/5.0/topics/testing/tools/#django.test.Client) di test Django. Questa classe agisce come un browser web fittizio che può essere usato per simulare richieste `GET` e `POST` a un URL e osservare la risposta. È possibile osservare quasi tutto ciò che riguarda la risposta, dall'HTTP di basso livello, inclusi header dei risultati e codici di stato, al template usato per il rendering dell'HTML e ai dati di contesto passati a esso. È inoltre possibile osservare la catena di reindirizzamenti, se presente, e controllare URL e codice di stato a ogni passaggio. Ciò permette di verificare che ogni vista faccia ciò che è previsto.

Iniziamo con una delle viste più semplici, che fornisce un elenco di tutti gli autori. Questa viene visualizzata all'URL **/catalog/authors/**, un URL denominato 'authors' nella configurazione degli URL.

```python
class AuthorListView(generic.ListView):
    model = Author
    paginate_by = 10
```

Poiché si tratta di una vista elenco generica, quasi tutto viene svolto automaticamente da Django. Se si ha fiducia in Django, si potrebbe sostenere che l'unica cosa da testare sia che la vista sia accessibile all'URL corretto e tramite il suo nome. Tuttavia, se viene usato un processo di sviluppo guidato dai test, si inizierà scrivendo test che confermano che la vista visualizza tutti gli autori, dividendoli in pagine da 10 elementi.

Aprire il file **/catalog/tests/test_views.py** e sostituire qualsiasi testo esistente con il seguente codice di test per `AuthorListView`. Come in precedenza, vengono importati il modello e alcune classi utili. Nel metodo `setUpTestData()` vengono configurati vari oggetti `Author` in modo da poter testare la paginazione.

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

Tutti i test usano il client, appartenente alla classe derivata da `TestCase`, per simulare una richiesta `GET` e ottenere una risposta. La prima versione verifica un URL specifico, ossia solo il percorso senza il dominio, mentre la seconda genera l'URL a partire dal suo nome nella configurazione degli URL.

```python
response = self.client.get('/catalog/authors/')
response = self.client.get(reverse('authors'))
```

Una volta ottenuta la risposta, vengono interrogati il relativo codice di stato, il template usato, se la risposta è paginata o meno, il numero di elementi restituiti e il numero totale di elementi.

> [!NOTE]
> Se la variabile `paginate_by` nel file **/catalog/views.py** è stata impostata su un numero diverso da 10, assicurarsi di aggiornare le righe che verificano che il numero corretto di elementi sia visualizzato nei template paginati sopra e nelle sezioni successive. Ad esempio, se la variabile per la pagina dell'elenco degli autori è stata impostata a 5, aggiornare la riga sopra come segue:
>
> ```python
> self.assertTrue(len(response.context['author_list']) == 5)
> ```

La variabile più interessante mostrata sopra è `response.context`, ovvero la variabile di contesto passata al template dalla vista. È estremamente utile per i test, perché consente di confermare che il template riceva tutti i dati necessari. In altre parole, è possibile controllare che venga usato il template previsto e quali dati riceva il template: ciò contribuisce notevolmente a verificare che eventuali problemi di rendering siano dovuti esclusivamente al template.

#### Viste riservate agli utenti connessi

In alcuni casi sarà necessario testare una vista riservata solo agli utenti connessi. Ad esempio, `LoanedBooksByUserListView` è molto simile alla vista precedente, ma è disponibile solo per gli utenti connessi e visualizza solo record `BookInstance` presi in prestito dall'utente corrente, che hanno lo stato 'on loan' e sono ordinati dal più vecchio al più recente.

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

Aggiungere il seguente codice di test a **/catalog/tests/test_views.py**. Qui viene prima usato `SetUp()` per creare alcuni account di accesso utente e oggetti `BookInstance`, insieme ai relativi libri e altri record, che verranno usati successivamente nei test. Metà dei libri viene presa in prestito da ciascun utente di test, ma lo stato iniziale di tutti i libri è impostato su "maintenance". Viene usato `SetUp()` anziché `setUpTestData()` perché alcuni di questi oggetti verranno modificati in seguito.

> [!NOTE]
> Il codice `setUp()` seguente crea un libro con un `Language` specificato, ma il codice potrebbe non includere il modello `Language`, poiché è stato creato come _sfida_. In questo caso, commentare le parti di codice che creano o importano oggetti Language. Lo stesso deve essere fatto nella sezione `RenewBookInstancesViewTest` che segue.

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
        test_author = Author.objects.create(first_name='Dominique', last_name='Rousseau')
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

Per verificare che la vista reindirizzi a una pagina di accesso se l'utente non è connesso, viene usato `assertRedirects`, come mostrato in `test_redirect_if_not_logged_in()`. Per verificare che la pagina venga visualizzata per un utente connesso, viene prima effettuato l'accesso dell'utente di test, quindi si accede nuovamente alla pagina e si controlla di ottenere uno `status_code` pari a 200, ovvero successo.

Il resto dei test verifica che la vista restituisca solo i libri in prestito al mutuatario corrente. Copiare il codice seguente e incollarlo alla fine della classe di test sopra.

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

Se lo si desidera, è possibile aggiungere anche test di paginazione.

#### Testare viste con moduli

Testare viste con moduli è un po' più complicato rispetto ai casi precedenti, perché occorre testare più percorsi di codice: visualizzazione iniziale, visualizzazione dopo un fallimento della convalida dei dati e visualizzazione dopo il successo della convalida. La buona notizia è che il client viene usato per i test quasi esattamente come per le viste di sola visualizzazione.

Per dimostrarlo, scriviamo alcuni test per la vista usata per rinnovare i libri (`renew_book_librarian()`):

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

Occorre verificare che la vista sia disponibile solo agli utenti che dispongono dell'autorizzazione `can_mark_returned` e che gli utenti vengano reindirizzati a una pagina di errore HTTP 404 se tentano di rinnovare un `BookInstance` inesistente. Occorre controllare che il valore iniziale del modulo sia inizializzato con una data a tre settimane nel futuro e che, se la convalida riesce, venga effettuato il reindirizzamento alla vista "tutti i libri in prestito". Nel controllo dei test di fallimento della convalida, verrà inoltre verificato che il modulo invii i messaggi di errore appropriati.

Aggiungere la prima parte della classe di test, mostrata di seguito, alla fine di **/catalog/tests/test_views.py**. Questa crea due utenti e due istanze di libro, ma assegna a un solo utente l'autorizzazione necessaria per accedere alla vista.

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
        test_author = Author.objects.create(first_name='Dominique', last_name='Rousseau')
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

Aggiungere i test seguenti alla fine della classe di test. Questi verificano che solo gli utenti con le autorizzazioni corrette, _testuser2_, possano accedere alla vista. Vengono controllati tutti i casi: quando l'utente non è connesso, quando un utente è connesso ma non ha le autorizzazioni corrette, quando l'utente ha le autorizzazioni ma non è il mutuatario, caso che dovrebbe riuscire, e cosa accade quando tenta di accedere a un `BookInstance` inesistente. Viene inoltre controllato che venga usato il template corretto.

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

Aggiungere il metodo di test successivo, come mostrato di seguito. Questo verifica che la data iniziale per il modulo sia tra tre settimane. Si noti come sia possibile accedere al valore iniziale del campo del modulo (`response.context['form'].initial['renewal_date']`).

```python
    def test_form_renewal_date_initially_has_date_three_weeks_in_future(self):
        login = self.client.login(username='testuser2', password='2HJ1vRV0Z&3iD')
        response = self.client.get(reverse('renew-book-librarian', kwargs={'pk': self.test_bookinstance1.pk}))
        self.assertEqual(response.status_code, 200)

        date_3_weeks_in_future = datetime.date.today() + datetime.timedelta(weeks=3)
        self.assertEqual(response.context['form'].initial['renewal_date'], date_3_weeks_in_future)
```

Il test successivo, da aggiungere anch'esso alla classe, verifica che la vista reindirizzi a un elenco di tutti i libri in prestito se il rinnovo riesce. La differenza qui è che, per la prima volta, viene mostrato come inviare dati `POST` usando il client. I _dati_ post sono il secondo argomento della funzione post e vengono specificati come dizionario di chiavi e valori.

```python
    def test_redirects_to_all_borrowed_book_list_on_success(self):
        login = self.client.login(username='testuser2', password='2HJ1vRV0Z&3iD')
        valid_date_in_future = datetime.date.today() + datetime.timedelta(weeks=2)
        response = self.client.post(reverse('renew-book-librarian', kwargs={'pk':self.test_bookinstance1.pk,}), {'renewal_date':valid_date_in_future})
        self.assertRedirects(response, reverse('all-borrowed'))
```

> [!WARNING]
> La vista _all-borrowed_ è stata aggiunta come _sfida_ e il codice potrebbe invece reindirizzare alla pagina iniziale '/'. In questo caso, modificare le ultime due righe del codice di test come nel codice seguente. `follow=True` nella richiesta assicura che la richiesta restituisca l'URL di destinazione finale, controllando quindi `/catalog/` anziché `/`.
>
> ```python
>  response = self.client.post(reverse('renew-book-librarian', kwargs={'pk':self.test_bookinstance1.pk,}), {'renewal_date':valid_date_in_future}, follow=True)
>  self.assertRedirects(response, '/catalog/')
> ```

Copiare le ultime due funzioni nella classe, come mostrato di seguito. Anche queste testano richieste `POST`, ma in questo caso con date di rinnovo non valide. Viene usato `assertFormError()` per verificare che i messaggi di errore siano quelli previsti.

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

Gli stessi tipi di tecniche possono essere usati per testare le altre viste.

### Template

Django fornisce API di test per verificare che le viste chiamino il template corretto e per consentire di verificare che vengano inviate le informazioni corrette. Non esiste tuttavia un supporto API specifico in Django per verificare che l'output HTML sia renderizzato come previsto.

## Altri strumenti di test consigliati

Il framework di test di Django può aiutare a scrivere efficaci unit test e test di integrazione: è stata solo scalfita la superficie di ciò che il framework **unittest** sottostante può fare, senza contare le aggiunte di Django. Ad esempio, è possibile vedere come usare [unittest.mock](https://docs.python.org/3/library/unittest.mock-examples.html) per applicare patch a librerie di terze parti e testare più approfonditamente il proprio codice.

Sebbene esistano numerosi altri strumenti di test utilizzabili, qui ne verranno evidenziati solo due:

- [Coverage](https://coverage.readthedocs.io/en/latest/): questo strumento Python riporta la quantità di codice effettivamente eseguita dai test. È particolarmente utile all'inizio, quando si cerca di capire esattamente cosa testare.
- [Selenium](/it/docs/Learn_web_development/Extensions/Testing/Your_own_automation_environment) è un framework per automatizzare i test in un browser reale. Permette di simulare un utente reale che interagisce con il sito e fornisce un ottimo framework per eseguire test di sistema del sito, ovvero il livello successivo rispetto ai test di integrazione.

## Mettiti alla prova

Ci sono molti altri modelli e viste che possono essere testati. Come sfida, provare a creare un caso di test per la vista `AuthorCreate`.

```python
class AuthorCreate(PermissionRequiredMixin, CreateView):
    model = Author
    fields = ['first_name', 'last_name', 'date_of_birth', 'date_of_death']
    initial = {'date_of_death': '11/11/2023'}
    permission_required = 'catalog.add_author'
```

Ricordare che è necessario controllare tutto ciò che viene specificato o che fa parte del progetto. Ciò includerà chi ha accesso, la data iniziale, il template usato e dove la vista reindirizza in caso di successo.

Il codice seguente può essere usato per configurare il test e assegnare all'utente l'autorizzazione appropriata.

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

Scrivere codice di test non è né divertente né affascinante e, di conseguenza, viene spesso lasciato per ultimo, o non viene fatto affatto, durante la creazione di un sito web. È tuttavia una parte essenziale per assicurarsi che il codice sia sicuro da rilasciare dopo aver apportato modifiche e conveniente da mantenere.

In questo tutorial è stato mostrato come scrivere ed eseguire test per modelli, moduli e viste. Ancora più importante, è stato fornito un breve riepilogo di ciò che dovrebbe essere testato, che spesso è l'aspetto più difficile da capire quando si inizia. C'è molto altro da imparare, ma già con quanto appreso si dovrebbero poter creare unit test efficaci per i propri siti web.

Il tutorial successivo e conclusivo mostra come distribuire il meraviglioso sito web Django, completamente testato.

## Vedi anche

- [Scrivere ed eseguire test](https://docs.djangoproject.com/en/5.0/topics/testing/overview/) (documentazione Django)
- [Scrivere la prima app Django, parte 5 > Introduzione al testing automatizzato](https://docs.djangoproject.com/en/5.0/intro/tutorial05/) (documentazione Django)
- [Riferimento agli strumenti di test](https://docs.djangoproject.com/en/5.0/topics/testing/tools/) (documentazione Django)
- [Argomenti avanzati sul testing](https://docs.djangoproject.com/en/5.0/topics/testing/advanced/) (documentazione Django)
- [Una Guida al Testing in Django](https://toastdriven.com/blog/2011/apr/09/guide-to-testing-in-django/) (Toast Driven Blog, 2011)
- [Workshop: sviluppo web guidato dai test con Django](https://test-driven-django-development.readthedocs.io/en/latest/index.html) (San Diego Python, 2014)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/Forms", "Learn_web_development/Extensions/Server-side/Django/Deployment", "Learn_web_development/Extensions/Server-side/Django")}}
