---
title: Classi in JavaScript
slug: Learn_web_development/Extensions/Advanced_JavaScript_objects/Classes_in_JavaScript
l10n:
  sourceCommit: 46c276b76c9fbf1468070686ecd3abbf64761500
---

{{PreviousMenuNext("Learn_web_development/Extensions/Advanced_JavaScript_objects/Object-oriented_programming", "Learn_web_development/Extensions/Advanced_JavaScript_objects/Test_your_skills/Object-oriented_JavaScript", "Learn_web_development/Extensions/Advanced_JavaScript_objects")}}

Nell'articolo precedente, sono stati introdotti alcuni concetti di base della programmazione orientata agli oggetti (OOP) ed è stato discusso un esempio in cui sono stati usati i principi OOP per modellare professori e studenti in una scuola.

È stato inoltre spiegato come sia possibile usare [prototipi](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_prototypes) e [costruttori](/it/docs/Learn_web_development/Core/Scripting/Object_basics#introducing_constructors) per implementare un modello di questo tipo e come JavaScript fornisca anche funzionalità che corrispondono più strettamente ai concetti classici dell'OOP.

In questo articolo verranno analizzate queste funzionalità. È importante ricordare che le funzionalità descritte qui non rappresentano un nuovo modo di combinare oggetti: internamente, usano ancora i prototipi. Sono semplicemente un modo per rendere più facile la configurazione di una catena di prototipi.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con le basi di JavaScript
        (in particolare
        <a href="/it/docs/Learn_web_development/Core/Scripting/Object_basics">Fondamenti degli oggetti</a>) e con i concetti di JavaScript orientato agli oggetti trattati nelle lezioni precedenti di questo modulo.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Creazione di classi in JavaScript.</li>
          <li>Creazione di costruttori in JavaScript.</li>
          <li>Ereditarietà e incapsulamento in JavaScript.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Classi e costruttori

È possibile dichiarare una classe usando la parola chiave {{jsxref("Statements/class", "class")}}. Ecco una dichiarazione di classe per la nostra `Person` dell'articolo precedente:

```js
class Person {
  name;

  constructor(name) {
    this.name = name;
  }

  introduceSelf() {
    console.log(`Hi! I'm ${this.name}`);
  }
}
```

Questa dichiara una classe chiamata `Person`, con:

- una proprietà `name`;
- un costruttore che accetta un parametro `name`, usato per inizializzare la proprietà `name` del nuovo oggetto;
- un metodo `introduceSelf()` che può fare riferimento alle proprietà dell'oggetto usando `this`.

La dichiarazione `name;` è facoltativa: può essere omessa e la riga `this.name = name;` nel costruttore creerà la proprietà `name` prima di inizializzarla. Tuttavia, elencare esplicitamente le proprietà nella dichiarazione della classe può rendere più semplice per chi legge il codice capire quali proprietà fanno parte di questa classe.

È anche possibile inizializzare la proprietà con un valore predefinito al momento della dichiarazione, con una riga come `name = '';`.

Il costruttore viene definito usando la parola chiave {{jsxref("Classes/constructor", "constructor")}}. Proprio come un [costruttore esterno a una definizione di classe](/it/docs/Learn_web_development/Core/Scripting/Object_basics#introducing_constructors), esso:

- crea un nuovo oggetto;
- associa `this` al nuovo oggetto, in modo da poter fare riferimento a `this` nel codice del costruttore;
- esegue il codice nel costruttore;
- restituisce il nuovo oggetto.

Data la dichiarazione di classe sopra, è possibile creare e usare una nuova istanza di `Person` in questo modo:

```js
const giles = new Person("Giles");

giles.introduceSelf(); // Hi! I'm Giles
```

Si noti che il costruttore viene chiamato usando il nome della classe, `Person` in questo esempio.

### Omettere i costruttori

Se non è necessario eseguire alcuna inizializzazione speciale, è possibile omettere il costruttore e verrà generato un costruttore predefinito:

```js
class Animal {
  sleep() {
    console.log("zzzzzzz");
  }
}

const spot = new Animal();

spot.sleep(); // 'zzzzzzz'
```

## Ereditarietà

Data la classe `Person` sopra, definiamo la sottoclasse `Professor`.

```js
class Professor extends Person {
  teaches;

  constructor(name, teaches) {
    super(name);
    this.teaches = teaches;
  }

  introduceSelf() {
    console.log(
      `My name is ${this.name}, and I will be your ${this.teaches} professor.`,
    );
  }

  grade(paper) {
    const grade = Math.floor(Math.random() * (5 - 1) + 1);
    console.log(grade);
  }
}
```

Viene usata la parola chiave {{jsxref("Classes/extends", "extends")}} per indicare che questa classe eredita da un'altra classe.

La classe `Professor` aggiunge una nuova proprietà `teaches`, quindi viene dichiarata.

Poiché si desidera impostare `teaches` quando viene creato un nuovo `Professor`, viene definito un costruttore che accetta `name` e `teaches` come argomenti. La prima cosa che fa questo costruttore è chiamare il costruttore della superclasse usando {{jsxref("Operators/super", "super()")}}, passando il parametro `name`. Il costruttore della superclasse si occupa di impostare `name`. Successivamente, il costruttore di `Professor` imposta la proprietà `teaches`.

> [!NOTE]
> Se una sottoclasse deve eseguire una propria inizializzazione, **deve** prima chiamare il costruttore della superclasse usando `super()`, passando tutti i parametri previsti dal costruttore della superclasse.

È stato inoltre sovrascritto il metodo `introduceSelf()` della superclasse ed è stato aggiunto un nuovo metodo `grade()`, per valutare un elaborato (il nostro professore non è molto bravo e assegna semplicemente voti casuali agli elaborati).

Con questa dichiarazione è ora possibile creare e usare professori:

```js
const walsh = new Professor("Walsh", "Psychology");
walsh.introduceSelf(); // 'My name is Walsh, and I will be your Psychology professor'

walsh.grade("my paper"); // some random grade
```

## Incapsulamento

Infine, vediamo come implementare l'incapsulamento in JavaScript. Nell'articolo precedente è stato discusso come rendere privata la proprietà `year` di `Student`, così da poter modificare le regole sulle classi di tiro con l'arco senza interrompere il codice che usa la classe `Student`.

Ecco una dichiarazione della classe `Student` che fa proprio questo:

```js
class Student extends Person {
  #year;

  constructor(name, year) {
    super(name);
    this.#year = year;
  }

  introduceSelf() {
    console.log(`Hi! I'm ${this.name}, and I'm in year ${this.#year}.`);
  }

  canStudyArchery() {
    return this.#year > 1;
  }
}
```

In questa dichiarazione di classe, `#year` è un [campo privato](/it/docs/Web/JavaScript/Reference/Classes/Private_elements). È possibile costruire un oggetto `Student`, che può usare `#year` internamente, ma se del codice esterno all'oggetto tenta di accedere a `#year`, il browser genera un errore:

```js
const summers = new Student("Summers", 2);

summers.introduceSelf(); // Hi! I'm Summers, and I'm in year 2.
summers.canStudyArchery(); // true

summers.#year; // SyntaxError
```

> [!NOTE]
> Il codice eseguito nella console di Chrome può accedere agli elementi privati dall'esterno della classe. Si tratta di un allentamento della restrizione sintattica di JavaScript disponibile solo in DevTools.

I campi privati devono essere dichiarati nella dichiarazione della classe e i loro nomi iniziano con `#`.

### Metodi privati

È possibile avere metodi privati oltre ai campi privati. Proprio come per i campi privati, i nomi dei metodi privati iniziano con `#` e possono essere chiamati solo dai metodi dell'oggetto stesso:

```js
class Example {
  somePublicMethod() {
    this.#somePrivateMethod();
  }

  #somePrivateMethod() {
    console.log("You called me?");
  }
}

const myExample = new Example();

myExample.somePublicMethod(); // 'You called me?'

myExample.#somePrivateMethod(); // SyntaxError
```

## Riepilogo

In questo articolo sono stati esaminati i principali strumenti disponibili in JavaScript per scrivere programmi orientati agli oggetti. Non è stato trattato tutto, ma questo dovrebbe essere sufficiente per iniziare. Il nostro [articolo sulle classi](/it/docs/Web/JavaScript/Reference/Classes) è un buon punto di partenza per approfondire.

Successivamente, verranno proposti alcuni test che possono essere usati per verificare quanto siano state comprese e ricordate le informazioni fornite finora su JavaScript orientato agli oggetti.

{{PreviousMenuNext("Learn_web_development/Extensions/Advanced_JavaScript_objects/Object-oriented_programming", "Learn_web_development/Extensions/Advanced_JavaScript_objects/Test_your_skills/Object-oriented_JavaScript", "Learn_web_development/Extensions/Advanced_JavaScript_objects")}}
