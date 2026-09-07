---
title: "Metti alla prova le tue competenze: JavaScript orientato agli oggetti"
short-title: "Test: JavaScript orientato agli oggetti"
slug: Learn_web_development/Extensions/Advanced_JavaScript_objects/Test_your_skills/Object-oriented_JavaScript
l10n:
  sourceCommit: 46c276b76c9fbf1468070686ecd3abbf64761500
---

{{PreviousMenuNext("Learn_web_development/Extensions/Advanced_JavaScript_objects/Classes_in_JavaScript", "Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_building_practice", "Learn_web_development/Extensions/Advanced_JavaScript_objects")}}

L'obiettivo di questo test delle competenze è aiutare a valutare se l'articolo [Classi in JavaScript](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Classes_in_JavaScript) è stato compreso.

> [!NOTE]
> Per ottenere aiuto, leggere la guida all'uso [Metti alla prova le tue competenze](/it/docs/Learn_web_development#test_your_skills). È inoltre possibile contattarci tramite uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## OOJS 1

In questa attività viene fornito l'inizio della definizione di una classe `Shape`. Ha tre proprietà: `name`, `sides` e `sideLength`. Questa classe modella solo forme per cui tutti i lati hanno la stessa lunghezza, come un quadrato o un triangolo equilatero.

Per completare l'attività:

1. Aggiungere un costruttore a questa classe. Il costruttore accetta argomenti per le proprietà `name`, `sides` e `sideLength` e le inizializza.
2. Aggiungere alla classe un nuovo metodo `calcPerimeter()`, che calcola il perimetro (la lunghezza del bordo esterno della forma) e registra il risultato nella console.
3. Creare una nuova istanza della classe `Shape` chiamata `square`. Assegnarle `square` come `name`, `4` come `sides` e `5` come `sideLength`.
4. Chiamare il metodo `calcPerimeter()` sull'istanza, per verificare che registri nella console del browser il risultato del calcolo come previsto.
5. Creare una nuova istanza di `Shape` chiamata `triangle`, con `triangle` come `name`, `3` come `sides` e `3` come `sideLength`.
6. Chiamare `triangle.calcPerimeter()` per verificare che funzioni correttamente.

```js live-sample___oojs-1
class Shape {
  name;
  sides;
  sideLength;
}
```

{{ EmbedLiveSample("oojs-1", "100%", 60) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il JavaScript completato potrebbe avere un aspetto simile al seguente:

```js
class Shape {
  name;
  sides;
  sideLength;

  constructor(name, sides, sideLength) {
    this.name = name;
    this.sides = sides;
    this.sideLength = sideLength;
  }

  calcPerimeter() {
    console.log(
      `The ${this.name}'s perimeter length is ${this.sides * this.sideLength}.`,
    );
  }
}

const square = new Shape("square", 4, 5);
square.calcPerimeter();

const triangle = new Shape("triangle", 3, 3);
triangle.calcPerimeter();
```

</details>

## OOJS 2

Ora è il momento di aggiungere un po' di ereditarietà.

Per completare l'attività:

1. Creare una classe `Square` che eredita da `Shape`.
2. Aggiungere a `Square` un metodo `calcArea()` che calcoli la sua area.
3. Configurare il costruttore di `Square` in modo che la proprietà `name` delle istanze di oggetti `Square` venga impostata automaticamente su `square` e la proprietà `sides` venga impostata automaticamente su `4`. Quando si invoca il costruttore, dovrebbe quindi essere necessario fornire soltanto la proprietà `sideLength`.
4. Creare un'istanza della classe `Square` chiamata `square` con valori delle proprietà appropriati e chiamare i relativi metodi `calcPerimeter()` e `calcArea()` per mostrare che funziona correttamente.

```js live-sample___oojs-2
class Shape {
  name;
  sides;
  sideLength;

  constructor(name, sides, sideLength) {
    this.name = name;
    this.sides = sides;
    this.sideLength = sideLength;
  }

  calcPerimeter() {
    console.log(
      `The ${this.name}'s perimeter length is ${this.sides * this.sideLength}.`,
    );
  }
}

// Don't edit the code above here!

// Add your code here
```

{{ EmbedLiveSample("oojs-2", "100%", 60) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il JavaScript completato potrebbe avere un aspetto simile al seguente:

```js
// ...
// Don't edit the code above here!

class Square extends Shape {
  constructor(sideLength) {
    super("square", 4, sideLength);
  }

  calcArea() {
    console.log(
      `The ${this.name}'s area is ${this.sideLength * this.sideLength} squared.`,
    );
  }
}

const square = new Square(4);

square.calcPerimeter();
square.calcArea();
```

</details>

{{PreviousMenuNext("Learn_web_development/Extensions/Advanced_JavaScript_objects/Classes_in_JavaScript", "Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_building_practice", "Learn_web_development/Extensions/Advanced_JavaScript_objects")}}
