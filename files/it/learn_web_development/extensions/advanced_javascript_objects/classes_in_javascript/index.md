---
title: Classi in JavaScript
slug: Learn_web_development/Extensions/Advanced_JavaScript_objects/Classes_in_JavaScript
l10n:
  sourceCommit: a1ac64fa4da965d2a152f08221b1a9aed638fd16
---

{{PreviousMenuNext("Learn_web_development/Extensions/Advanced_JavaScript_objects/Object-oriented_programming", "Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_building_practice", "Learn_web_development/Extensions/Advanced_JavaScript_objects")}}

Nell'ultimo articolo, abbiamo introdotto alcuni concetti base della programmazione orientata agli oggetti (OOP) e discusso un esempio in cui abbiamo utilizzato i principi della OOP per modellare professori e studenti in una scuola.

Abbiamo anche parlato di come sia possibile utilizzare [prototipi](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_prototypes) e [costruttori](/it/docs/Learn_web_development/Core/Scripting/Object_basics#introducing_constructors) per implementare un modello come questo, e che JavaScript fornisce anche funzionalità che mappano più strettamente i concetti di OOP classica.

In questo articolo, esamineremo queste funzionalità. È importante tenere a mente che le funzionalità descritte qui non sono un nuovo modo di combinare oggetti: in profondità, usano ancora i prototipi. Sono solo un modo per facilitare la creazione di una catena di prototipi.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con le basi di JavaScript
        (in particolare
        <a href="/it/docs/Learn_web_development/Core/Scripting/Object_basics">Basi degli oggetti</a>) e concetti di JavaScript orientato agli oggetti trattati nelle lezioni precedenti di questo modulo.
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

È possibile dichiarare una classe utilizzando la parola chiave {{jsxref("Statements/class", "class")}}. Ecco una dichiarazione di classe per il nostro `Person` dall'articolo precedente:

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

Questo dichiara una classe chiamata `Person`, con:

- una proprietà `name`.
- un costruttore che prende un parametro `name` utilizzato per inizializzare la proprietà `name` del nuovo oggetto.
- un metodo `introduceSelf()` che può riferirsi alle proprietà dell'oggetto utilizzando `this`.

La dichiarazione `name;` è opzionale: potrebbe essere omessa e la linea `this.name = name;` nel costruttore creerà la proprietà `name` prima di inizializzarla. Tuttavia, elencare esplicitamente le proprietà nella dichiarazione della classe può rendere più facile per chi legge il suo codice vedere quali proprietà fanno parte di questa classe.

Si potrebbe anche inizializzare la proprietà a un valore predefinito quando la si dichiara, con una linea come `name = '';`.

Il costruttore è definito usando la parola chiave {{jsxref("Classes/constructor", "constructor")}}. Proprio come un [costruttore al di fuori di una definizione di classe](/it/docs/Learn_web_development/Core/Scripting/Object_basics#introducing_constructors), esso:

- creerà un nuovo oggetto
- legherà `this` al nuovo oggetto, in modo da poter fare riferimento a `this` nel codice del costruttore
- eseguirà il codice nel costruttore
- restituirà il nuovo oggetto.

Dato il codice della dichiarazione della classe sopra, è possibile creare e utilizzare una nuova istanza di `Person` in questo modo:

```js
const giles = new Person("Giles");

giles.introduceSelf(); // Hi! I'm Giles
```

Si noti che chiamiamo il costruttore usando il nome della classe, `Person` in questo esempio.

### Omettere i costruttori

Se non si ha necessità di fare alcuna inizializzazione speciale, si può omettere il costruttore e un costruttore predefinito sarà generato per lei:

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

Vista la nostra classe `Person` sopra, definiamo la sottoclasse `Professor`.

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

Usiamo la parola chiave {{jsxref("Classes/extends", "extends")}} per indicare che questa classe eredita da un'altra classe.

La classe `Professor` aggiunge una nuova proprietà `teaches`, quindi la dichiariamo.

Poiché vogliamo impostare `teaches` quando viene creato un nuovo `Professor`, definiamo un costruttore che prende `name` e `teaches` come argomenti. La prima cosa che questo costruttore fa è chiamare il costruttore della superclasse usando {{jsxref("Operators/super", "super()")}}, passando il parametro `name`. Il costruttore della superclasse si occupa di impostare `name`. Dopo di ciò, il costruttore di `Professor` imposta la proprietà `teaches`.

> [!NOTE]
> Se una sottoclasse ha qualsiasi inizializzazione propria da fare, **deve** prima chiamare il costruttore della superclasse usando `super()`, passando tutti i parametri che il costruttore della superclasse si aspetta.

Abbiamo anche sovrascritto il metodo `introduceSelf()` della superclasse e aggiunto un nuovo metodo `grade()`, per valutare un elaborato (il nostro professore non è molto bravo e assegna voti casuali agli elaborati).

Con questa dichiarazione possiamo ora creare e utilizzare professori:

```js
const walsh = new Professor("Walsh", "Psychology");
walsh.introduceSelf(); // 'My name is Walsh, and I will be your Psychology professor'

walsh.grade("my paper"); // some random grade
```

## Incapsulamento

Infine, vediamo come implementare l'incapsulamento in JavaScript. Nell'articolo precedente abbiamo discusso di come vorremmo rendere la proprietà `year` di `Student` privata, in modo da poter modificare le regole sulle lezioni di tiro con l'arco senza interrompere alcun codice che utilizza la classe `Student`.

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

In questa dichiarazione di classe, `#year` è una [proprietà di dati privata](/it/docs/Web/JavaScript/Reference/Classes/Private_properties). Possiamo costruire un oggetto `Student`, e può usare internamente `#year`, ma se codice al di fuori dell'oggetto tenta di accedere a `#year` il browser genera un errore:

```js
const summers = new Student("Summers", 2);

summers.introduceSelf(); // Hi! I'm Summers, and I'm in year 2.
summers.canStudyArchery(); // true

summers.#year; // SyntaxError
```

> [!NOTE]
> Il codice eseguito nella console di Chrome può accedere alle proprietà private al di fuori della classe. Questa è una deroga esclusiva per DevTools alla restrizione sintattica di JavaScript.

Le proprietà di dati privati devono essere dichiarate nella dichiarazione della classe e i loro nomi iniziano con `#`.

### Metodi privati

È possibile avere metodi privati così come proprietà di dati privati. Proprio come le proprietà di dati privati, i loro nomi iniziano con `#` e possono essere chiamati solo dai metodi dell'oggetto stesso:

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

## Metti alla prova le tue abilità!

Ha raggiunto la fine di questo articolo, ma può ricordare le informazioni più importanti? Può trovare alcuni test ulteriori per verificare che abbia assimilato queste informazioni prima di passare oltre — vedere [Metti alla prova le tue abilità: JavaScript orientato agli oggetti](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Test_your_skills/Object-oriented_JavaScript).

## Riepilogo

In questo articolo, abbiamo esaminato i principali strumenti disponibili in JavaScript per scrivere programmi orientati agli oggetti. Non abbiamo coperto tutto qui, ma questo dovrebbe essere sufficiente per iniziare. Il nostro [articolo sulle Classi](/it/docs/Web/JavaScript/Reference/Classes) è un buon posto per imparare di più.

{{PreviousMenuNext("Learn_web_development/Extensions/Advanced_JavaScript_objects/Object-oriented_programming", "Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_building_practice", "Learn_web_development/Extensions/Advanced_JavaScript_objects")}}
