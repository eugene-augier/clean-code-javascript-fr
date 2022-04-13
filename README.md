# clean-code-javascript
Original repository: https://github.com/ryanmcdermott/clean-code-javascript

## Sommaire

1. [Introduction](#introduction)
2. [Variables](#variables)
3. [Fonctions](#fonctions)
4. [Objets et structures de données](#Objets-et-structures-de-données)
5. [Classes](#classes)
6. [SOLID](#solid)
7. [Tests](#Tests)
8. [Concurrence](#Concurrence)
9. [Gestion des erreurs](#Gestion-des-erreurs)
10. [Formatage](#Formatage)
11. [Commentaires](#Commentaires)

## Introduction

Principes d'ingénierie logicielle, du livre [_Clean Code_](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)
de Robert C. Martin's adapté au JavaScript.
Il ne s'agit pas d'un guide de style. C'est un guide pour produire des logiciels
[lisibles, réutilisables, et refactorisables](https://github.com/ryanmcdermott/3rs-of-software-architecture) en JavaScript.

Il n'est pas nécessaire de suivre à la lettre tous les principes énoncés dans le présent document, et rares sont ceux qui feront l'objet d'un consensus universellement accepté. Il s'agit de lignes directrices et rien de plus, mais elles ont été codifiées par les auteurs de _Clean Code_ au fil de nombreuses années d'expérience collective.

Notre métier d'ingénieur logiciel a un peu plus de 50 ans, et nous apprenons encore beaucoup. Lorsque l'architecture logicielle sera aussi vieille que l'architecture elle-même, nous aurons peut-être alors des règles plus difficiles à suivre. Pour l'instant, laissez ces directives servir de pierre de touche pour évaluer la qualité du code JavaScript que vous et votre équipe produisez.

Une dernière chose : connaître ces règles ne fera pas immédiatement de vous un meilleur développeur de logiciels, et travailler avec elles pendant de nombreuses années ne signifie pas que vous ne ferez pas d'erreurs. Chaque morceau de code commence par une première ébauche, comme de l'argile humide que l'on façonne pour lui donner sa forme finale. Enfin, nous ciselons les imperfections lorsque nous le révisons avec nos pairs. Ne vous blâmez pas pour les premières ébauches qui doivent être améliorer. Battez-vous plutôt contre le code !

## **Variables**

### Utiliser des noms de variables explicites et prononçables

**Mauvais :**

```javascript
const yyyymmdstr = moment().format("YYYY/MM/DD");
```

**Bien :**

```javascript
const currentDate = moment().format("YYYY/MM/DD");
```

**[⬆ retour en haut](#sommaire)**

### Utiliser le même vocabulaire pour le même type de variable

**Mauvais :**

```javascript
getUserInfo();
getClientData();
getCustomerRecord();
```

**Bien :**

```javascript
getUser();
```

**[⬆ retour en haut](#sommaire)**

### Utiliser des noms recherchables

Nous lirons plus de code que nous n'en écrirons jamais. Il est important que le code que nous écrivons soit lisible et consultable. En _ne nommant pas_ les variables qui finissent par être significatives pour la compréhension de notre programme, nous nuisons à nos lecteurs.
Faites en sorte que vos noms soient consultables. Des outils comme [buddy.js](https://github.com/danielstjules/buddy.js) et [ESLint](https://github.com/eslint/eslint/blob/660e0918933e6e7fede26bc675a0763a6b357c94/docs/rules/no-magic-numbers.md) peuvent vous aider à identifier les constantes sans nom.

**Mauvais :**

```javascript
// À quoi sert 86400000 ?
setTimeout(blastOff, 86400000);
```

**Bien :**

```javascript
// Déclarez-les en tant que constantes nommées en majuscules.
const MILLISECONDS_PER_DAY = 60 * 60 * 24 * 1000; //86400000;
setTimeout(blastOff, MILLISECONDS_PER_DAY);
```

**[⬆ retour en haut](#sommaire)**

### Utiliser des variables explicatives

**Mauvais :**

```Javascript
const address = "One Infinite Loop, Cupertino 95014" ;
const cityZipCodeRegex = /^[^,\\]+[,\\\\\s]+(.+ ?)\s*(\d{5})?$/ ;
saveCityZipCode(
    adresse.match(cityZipCodeRegex)[1],
    adresse.match(cityZipCodeRegex)[2]
) ;
```

**Bien :**

```Javascript
const address = "One Infinite Loop, Cupertino 95014" ;
const cityZipCodeRegex = /^[^,\\]+[,\\\\\s]+(.+ ?)\s*(\d{5})?$/ ;
const [_, city, zipCode] = address.match(cityZipCodeRegex) || [] ;
saveCityZipCode(city, zipCode) ;
```

**[⬆ retour en haut](#sommaire)**

### Éviter la cartographie mentale

L'explicite est préférable à l'implicite.

**Mauvais :**

```Javascript
const locations = ["Austin", "New York", "San Francisco"] ;
locations.forEach(l => {
    doStuff() ;
    doSomeOtherStuff() ;
    // ...
    // ...
    // ...
    // Attendez, à quoi sert `l` déjà ?
    dispatch(l) ;
}) ;
```

**Bien :**

```Javascript
const locations = ["Austin", "New York", "San Francisco"] ;
locations.forEach(location => {
    doStuff() ;
    doSomeOtherStuff() ;
    // ...
    // ...
    // ...
    dispatch(location) ;
}) ;
```

**[⬆ retour en haut](#sommaire)**

### Ne pas ajouter de contexte inutile

Si le nom de votre classe/objet vous indique quelque chose, ne le répétez pas dans votre nom de variable.

**Mauvais :**

```Javascript
const Car = {
    carMake: "Honda",
    carModel: "Accord",
    carColor: "Blue"
} ;
function paintCar(car, color) {
    car.carColor = color ;
}
```

**Bien :**

```Javascript
const Car = {
    make: "Honda",
    model: "Accord",
    color: "Blue"
} ;
function paintCar(car, color) {
    car.color = color ;
}
```

**[⬆ retour en haut](#sommaire)**

### Utiliser des arguments par défaut à la place des court-circuits ou des conditions

Les arguments par défaut sont souvent plus propres que le court-circuitage.
Soyez conscient que si vous les utilisez, votre fonction ne fournira des valeurs par défaut que pour les arguments `non définis`.
Les autres valeurs "fausses" telles que `''`, `""`, `false`, `null`, `0`, et `NaN`, ne seront pas remplacées par une valeur par défaut.

**Mauvais :**

```Javascript
function createMicrobrewery(name) {
const breweryName = name || "Hipster Brew Co." ;
// ...
}
```

**Bien :**

```Javascript
function createMicrobrewery(name = "Hipster Brew Co.") {
  // ...
}
```

**[⬆ retour en haut](#sommaire)**

## **Fonctions**

### Arguments de fonction (2 ou moins idéalement)

Limiter le nombre de paramètres de fonction est extrêmement important car cela facilite le test de votre fonction.
En avoir plus de trois conduit à une explosion combinatoire où vous devez tester des tonnes de cas différents avec chaque argument séparé.

Un ou deux arguments est le cas idéal, et trois devraient être évités si possible.
Tout ce qui est supérieur à cela doit être consolidé. En général, si vous avez plus de deux arguments, c'est que votre fonction essaie d'en faire trop.
Dans les cas où ce n'est pas le cas, la plupart du temps, un objet de niveau supérieur suffira comme argument.

Étant donné que JavaScript vous permet de créer des objets à la volée, sans avoir besoin d'un grand nombre de classes passe-partout, vous pouvez utiliser un objet si vous avez besoin de beaucoup d'arguments.

Pour rendre évidentes les propriétés attendues par la fonction, vous pouvez utiliser la syntaxe de déstructuration ES2015/ES6.
Cela présente quelques avantages :
1. Lorsqu'on regarde la signature de la fonction, on voit immédiatement quelles propriétés sont utilisées.
2. Elle peut être utilisée pour simuler des paramètres nommés.
3. La déstructuration clone également les valeurs primitives spécifiées de l'objet argument passé dans la fonction. Cela peut aider à prévenir les effets secondaires.
   Remarque : les objets et les tableaux qui sont déstructurés à partir de l'objet argument ne sont PAS clonés.
4. Les linters peuvent vous avertir des propriétés inutilisées, ce qui serait impossible sans la déstructuration.

**Mauvais :**

```javascript
function createMenu(title, body, buttonText, cancellable) {
    // ...
}
createMenu("Foo", "Bar", "Baz", true);
```

**Bien :**

```javascript
function createMenu({ title, body, buttonText, cancellable }) {
    // ...
}
createMenu({
    title: "Foo",
    body: "Bar",
    buttonText: "Baz",
    cancellable: true
});
```

**[⬆ retour en haut](#sommaire)**

### Les fonctions doivent faire une seule chose

C'est de loin la règle la plus importante en ingénierie logicielle.
Lorsque les fonctions font plus d'une chose, elles sont plus difficiles à composer, à tester et à raisonner.
Lorsque vous pouvez isoler une fonction pour une seule action, elle peut être facilement remaniée et votre code sera beaucoup plus propre.
Si vous ne retenez rien d'autre de ce guide, vous aurez une longueur d'avance sur de nombreux développeurs.

**Mauvais :**

```javascript
function emailClients(clients) {
    clients.forEach(client => {
        const clientRecord = database.lookup(client);
        if (clientRecord.isActive()) {
            email(client);
        }
    });
}
```

**Bien :**

```javascript
function emailActiveClients(clients) {
    clients.filter(isActiveClient).forEach(email);
}
function isActiveClient(client) {
    const clientRecord = database.lookup(client);
    return clientRecord.isActive();
}
```

**[⬆ retour en haut](#sommaire)**

### Les noms des fonctions devraient dire ce qu'elles font

**Mauvais :**

```Javascript
function addToDate(date, mois) {
    // ...
}
const date = new Date() ;
// Il est difficile de dire, à partir du nom de la fonction, ce qui est ajouté
addToDate(date, 1) ;
```

**Bien :**

```Javascript
function addMonthToDate(month, date) {
    // ...
}
const date = new Date() ;
addMonthToDate(1, date) ;
```

**[⬆ retour en haut](#sommaire)**

### Les fonctions ne doivent avoir qu'un seul niveau d'abstraction.

Lorsque vous avez plus d'un niveau d'abstraction, votre fonction fait généralement trop de choses.
Le découpage des fonctions permet de les réutiliser et facilite les tests.

**Mauvais :**

```Javascript
function parseBetterJSAlternative(code) {
    const REGEXES = [
        // ...
    ] ;
    const statements = code.split(" ") ;
    const tokens = [] ;
    REGEXES.forEach(REGEX => {
        statements.forEach(statement => {
            // ...
        }) ;
    }) ;
    const ast = [] ;
    tokens.forEach(token => {
        // lex...
    }) ;
    ast.forEach(node => {
        // analyse syntaxique...
    }) ;
}
```

**Bien :**

```javascript
function parseBetterJSAlternative(code) {
    const tokens = tokenize(code);
    const syntaxTree = parse(tokens);
    syntaxTree.forEach(node => {
        // parse...
    });
}
function tokenize(code) {
    const REGEXES = [
        // ...
    ];
    const statements = code.split(" ");
    const tokens = [];
    REGEXES.forEach(REGEX => {
        statements.forEach(statement => {
            tokens.push(/* ... */);
        });
    });
    return tokens;
}
function parse(tokens) {
    const syntaxTree = [];
    tokens.forEach(token => {
        syntaxTree.push(/* ... */);
    });
    return syntaxTree;
}
```

**[⬆ retour en haut](#sommaire)**

### Supprimer le code dupliqué

Faites de votre mieux pour éviter les doublons.
Le code dupliqué est mauvais car cela signifie qu'il y a plus d'un endroit où modifier quelque chose si vous devez changer une logique.

Imaginez que vous dirigez un restaurant et que vous gardez la trace de votre inventaire : toutes vos tomates, oignons, ail, épices, etc.
Si vous disposez de plusieurs listes, elles doivent toutes être mises à jour lorsque vous servez un plat contenant des tomates.
Si vous n'avez qu'une seule liste, il n'y a qu'un seul endroit à mettre à jour !

Souvent, vous avez du code dupliqué parce que vous avez deux ou plusieurs choses légèrement différentes, qui ont beaucoup en commun, mais leurs différences vous obligent à avoir deux ou plusieurs fonctions distinctes qui font à peu près la même chose.
Supprimer le code dupliqué signifie créer une abstraction qui peut gérer cet ensemble de différences avec une seule fonction/module/classe.

Il est essentiel d'obtenir une abstraction correcte, c'est pourquoi vous devez suivre les principes SOLID énoncés dans la section _Classes_.
De mauvaises abstractions peuvent être pires que du code dupliqué, alors faites attention !
Ceci étant dit, si vous pouvez faire une bonne abstraction, faites-le !

__Ne vous répétez pas__, sinon vous vous retrouverez à mettre à jour plusieurs endroits chaque fois que vous voudrez changer une chose.

**Mauvais :**

```javascript
function showDeveloperList(developers) {
    developers.forEach(developer => {
        const expectedSalary = developer.calculateExpectedSalary();
        const experience = developer.getExperience();
        const githubLink = developer.getGithubLink();
        const data = {
            expectedSalary,
            experience,
            githubLink
        };
        render(data);
    });
}
function showManagerList(managers) {
    managers.forEach(manager => {
        const expectedSalary = manager.calculateExpectedSalary();
        const experience = manager.getExperience();
        const portfolio = manager.getMBAProjects();
        const data = {
            expectedSalary,
            experience,
            portfolio
        };
        render(data);
    });
}
```

**Bien :**

```javascript
function showEmployeeList(employees) {
    employees.forEach(employee => {
        const expectedSalary = employee.calculateExpectedSalary();
        const experience = employee.getExperience();
        const data = {
            expectedSalary,
            experience
        };
        switch (employee.type) {
            case "manager":
                data.portfolio = employee.getMBAProjects();
                break;
            case "developer":
                data.githubLink = employee.getGithubLink();
                break;
        }
        render(data);
    });
}
```

**[⬆ retour en haut](#sommaire)**

### Définir des objets par défaut avec Object.assign

**Mauvais :**

```javascript
const menuConfig = {
    title: null,
    body: "Bar",
    buttonText: null,
    cancellable: true
};
function createMenu(config) {
    config.title = config.title || "Foo";
    config.body = config.body || "Bar";
    config.buttonText = config.buttonText || "Baz";
    config.cancellable =
        config.cancellable !== undefined ? config.cancellable : true;
}
createMenu(menuConfig);
```

**Bien :**

```javascript
const menuConfig = {
    title: "Order",
    // User n'inclut pas la clé 'body'
    buttonText: "Send",
    cancellable: true
};
function createMenu(config) {
    let finalConfig = Object.assign(
        {
            title: "Foo",
            body: "Bar",
            buttonText: "Baz",
            cancellable: true
        },
        config
    );
    return finalConfig
    // config est maintenant égal à : {title: "Order", body: "Bar", buttonText: "Send", cancellable: true}
    // ...
}
createMenu(menuConfig);
```

**[⬆ retour en haut](#sommaire)**

### Ne pas utiliser de drapeaux comme paramètres de fonction

Les drapeaux indiquent à votre utilisateur que cette fonction fait plus d'une chose.
Les fonctions doivent faire une seule chose.
Séparez vos fonctions si elles suivent des chemins de code différents basés sur un booléen.

**Mauvais :**

```javascript
function createFile(name, temp) {
    if (temp) {
        fs.create(`./temp/${name}`);
    } else {
        fs.create(name);
    }
}
```

**Bien :**

```javascript
function createFile(name) {
    fs.create(name);
}
function createTempFile(name) {
    createFile(`./temp/${name}`);
}
```

**[⬆ retour en haut](#sommaire)**

### Évitez les effets de bord (partie 1)

Une fonction produit un effet secondaire si elle fait autre chose que prendre une valeur en entrée et renvoyer une ou plusieurs autres valeurs.
Un effet secondaire peut être l'écriture dans un fichier, la modification d'une variable globale ou le transfert accidentel de tout votre argent à un étranger.

Il est parfois nécessaire d'avoir des effets secondaires dans un programme.
Comme dans l'exemple précédent, vous pouvez avoir besoin d'écrire dans un fichier. Ce que vous voulez faire, c'est centraliser l'endroit où vous le faites.
N'ayez pas plusieurs fonctions et classes qui écrivent dans un fichier particulier.
N'ayez qu'un seul service qui s'en charge. Un et un seul.

Le point principal est d'éviter les pièges communs comme le partage d'état entre objets sans aucune structure, l'utilisation de types de données mutables qui peuvent être écrits par n'importe qui,
et ne pas centraliser l'endroit où se produisent les effets secondaires.
Si vous y parvenez, vous serez plus heureux que la grande majorité des autres programmeurs.

**Mauvais :**

```javascript
// Variable globale référencée par la fonction suivante.
// Si on avait une autre fonction qui utilisait ce nom, maintenant ce serait un tableau et ça pourrait le casser.
let name = "Ryan McDermott";
function splitIntoFirstAndLastName() {
    name = name.split(" ");
}
splitIntoFirstAndLastName();
console.log(name); // ['Ryan', 'McDermott'];
```

**Bien :**

```javascript
function splitIntoFirstAndLastName(name) {
    return name.split(" ");
}
const name = "Ryan McDermott";
const newName = splitIntoFirstAndLastName(name);
console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```

**[⬆ retour en haut](#sommaire)**

### Éviter les effets de bord (partie 2)

En JavaScript, certaines valeurs sont immuables (immutable) et d'autres sont modifiables (mutable).
Les objets et les tableaux sont deux types de valeurs mutables. Il est donc important de les manipuler avec précaution lorsqu'ils sont transmis comme paramètres à une fonction.
Une fonction JavaScript peut modifier les propriétés d'un objet ou altérer le contenu d'un tableau, ce qui pourrait facilement provoquer des bogues ailleurs.

Supposons qu'une fonction accepte un paramètre de tableau représentant un panier d'achat.
Si la fonction modifie ce tableau - en ajoutant un article, par exemple -, toute autre fonction utilisant le même tableau `cart` sera affectée par cet ajout. Cela peut être une bonne chose, mais cela peut aussi être mauvais.

Imaginons une mauvaise situation :

L'utilisateur clique sur le bouton "Acheter", ce qui appelle une fonction `purchase` qui génère une requête réseau et envoie le tableau `cart` au serveur. 
En raison d'une mauvaise connexion réseau, la fonction `purchase` doit sans cesse réessayer la requête.
Maintenant, que se passe-t-il si entre-temps l'utilisateur clique accidentellement sur le bouton "Ajouter au panier" d'un article qu'il ne veut pas vraiment avant que la requête réseau ne commence ?
Si cela se produit et que la requête réseau commence, la fonction d'achat enverra l'article ajouté accidentellement parce que le tableau `cart` a été modifié.

Une bonne solution serait que la fonction `addItemToCart` clone toujours le `cart`, le modifie et retourne le clone.
Ainsi, les fonctions qui utilisent encore l'ancien panier ne seraient pas affectées par les modifications.

Deux mises en garde à propos de cette approche :

1. Il peut y avoir des cas où vous souhaitez réellement modifier l'objet d'entrée, mais si vous adoptez cette pratique de programmation, vous constaterez que ces cas sont plutôt rares. La plupart des choses peuvent être remaniées pour n'avoir aucun effet secondaire !

2. Le clonage de gros objets peut être très coûteux en termes de performances. Heureusement, ce n'est pas un gros problème dans la pratique car il existe d'excellentes [bibliothèques](https://facebook.github.io/immutable-js/) qui permettent à ce type d'approche de programmation d'être rapide et moins gourmande en mémoire que si vous cloniez manuellement des objets et des tableaux.

**Mauvais :**

```javascript
const addItemToCart = (cart, item) => {
    cart.push({ item, date: Date.now() });
};
```

**Bien :**

```javascript
const addItemToCart = (cart, item) => {
    return [...cart, { item, date: Date.now() }];
};
```

**[⬆ retour en haut](#sommaire)**

### Ne pas écrire dans les fonctions globales

Polluer les fonctions globales est une mauvaise pratique en JavaScript, car vous pourriez entrer en conflit avec une autre bibliothèque et l'utilisateur de votre API n'en saurait rien jusqu'à ce qu'il obtienne une exception en production.
Prenons un exemple : que se passerait-il si vous vouliez étendre la méthode native Array de JavaScript pour avoir une méthode `diff` qui pourrait montrer la différence entre deux tableaux ?
Vous pourriez écrire votre nouvelle fonction dans `Array.prototype`, mais elle pourrait entrer en conflit avec une autre bibliothèque qui essaye de faire la même chose.

Et si cette autre bibliothèque utilisait simplement `diff` pour trouver la différence entre le premier et le dernier élément d'un tableau ? C'est pourquoi il serait bien mieux d'utiliser les classes ES2015/ES6 et d'étendre simplement le global `Array`.

**Mauvais :**

```javascript
Array.prototype.diff = function diff(comparisonArray) {
    const hash = new Set(comparisonArray);
    return this.filter(elem => !hash.has(elem));
};
```

**Bien :**

```javascript
class SuperArray extends Array {
    diff(comparisonArray) {
        const hash = new Set(comparisonArray);
        return this.filter(elem => !hash.has(elem));
    }
}
```

**[⬆ retour en haut](#sommaire)**

### Favoriser la programmation fonctionnelle plutôt que la programmation impérative

JavaScript n'est pas un langage fonctionnel comme Haskell, mais il a une saveur fonctionnelle. Les langages fonctionnels peuvent être plus propres et plus faciles à tester.
Favorisez ce style de programmation lorsque vous le pouvez.

**Mauvais :**

```javascript
const programmerOutput = [
    {
        name: "Uncle Bobby",
        linesOfCode: 500
    },
    {
        name: "Suzie Q",
        linesOfCode: 1500
    },
    {
        name: "Jimmy Gosling",
        linesOfCode: 150
    },
    {
        name: "Gracie Hopper",
        linesOfCode: 1000
    }
];
let totalOutput = 0;
for (let i = 0; i < programmerOutput.length; i++) {
    totalOutput += programmerOutput[i].linesOfCode;
}
```

**Bien :**

```javascript
const programmerOutput = [
    {
        name: "Uncle Bobby",
        linesOfCode: 500
    },
    {
        name: "Suzie Q",
        linesOfCode: 1500
    },
    {
        name: "Jimmy Gosling",
        linesOfCode: 150
    },
    {
        name: "Gracie Hopper",
        linesOfCode: 1000
    }
];
const totalOutput = programmerOutput.reduce(
    (totalLines, output) => totalLines + output.linesOfCode,
    0
);
```

**[⬆ retour en haut](#sommaire)**

### Encapsuler les conditions

**Mauvais :**

```javascript
if (fsm.state === "fetching" && isEmpty(listNode)) {
    // ...
}
```

**Bien :**

```javascript
function shouldShowSpinner(fsm, listNode) {
    return fsm.state === "fetching" && isEmpty(listNode);
}
if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
    // ...
}
```

**[⬆ retour en haut](#sommaire)**

### Évitez les conditions négatives

**Mauvais :**

```javascript
function isDOMNodeNotPresent(node) {
    // ...
}
if (!isDOMNodeNotPresent(node)) {
    // ...
}
```

**Bien :**

```javascript
function isDOMNodePresent(node) {
    // ...
}
if (isDOMNodePresent(node)) {
    // ...
}
```

**[⬆ retour en haut](#sommaire)**

### Éviter les conditions

Cela semble être une tâche impossible. En entendant cela pour la première fois, la plupart des gens disent : "comment suis-je censé faire quelque chose sans une instruction "if" ?".
La réponse est que vous pouvez utiliser le polymorphisme pour réaliser la même tâche dans de nombreux cas.
La deuxième question est généralement "c'est génial mais pourquoi voudrais-je faire ça ?".
La réponse est un concept de __clean code__ que nous avons appris précédemment : une fonction ne doit faire qu'une seule chose.
Lorsque vous avez des classes et des fonctions qui ont des instructions "if", vous dites à votre utilisateur que votre fonction fait plus d'une chose.
Rappelez-vous, ne faites qu'une seule chose.

**Mauvais :**

```javascript
class Airplane {
    // ...
    getCruisingAltitude() {
        switch (this.type) {
            case "777":
                return this.getMaxAltitude() - this.getPassengerCount();
            case "Air Force One":
                return this.getMaxAltitude();
            case "Cessna":
                return this.getMaxAltitude() - this.getFuelExpenditure();
        }
    }
}
```

**Bien :**

```javascript
class Airplane {
    // ...
}
class Boeing777 extends Airplane {
    // ...
    getCruisingAltitude() {
        return this.getMaxAltitude() - this.getPassengerCount();
    }
}
class AirForceOne extends Airplane {
    // ...
    getCruisingAltitude() {
        return this.getMaxAltitude();
    }
}
class Cessna extends Airplane {
    // ...
    getCruisingAltitude() {
        return this.getMaxAltitude() - this.getFuelExpenditure();
    }
}
```

**[⬆ retour en haut](#sommaire)**

### Évitez la vérification de type (partie 1)

JavaScript n'est pas typé, ce qui signifie que vos fonctions peuvent prendre n'importe quel type d'argument.
Parfois, vous êtes mordu par cette liberté et il devient tentant de faire de la vérification de type dans vos fonctions.
Il existe de nombreuses façons d'éviter de devoir le faire.
La première chose à considérer est la cohérence des API.

**Mauvais :**

```javascript
function travelToTexas(vehicle) {
    if (vehicle instanceof Bicycle) {
        vehicle.pedal(this.currentLocation, new Location("texas"));
    } else if (vehicle instanceof Car) {
        vehicle.drive(this.currentLocation, new Location("texas"));
    }
}
```

**Bien :**

```javascript
function travelToTexas(vehicle) {
    vehicle.move(this.currentLocation, new Location("texas"));
}
```

**[⬆ retour en haut](#sommaire)**

### Évitez la vérification de type (partie 2)

Si vous travaillez avec des valeurs primitives de base comme les chaînes de caractères et les entiers, et que vous ne pouvez pas utiliser le polymorphisme mais que vous ressentez toujours le besoin de vérifier le type,
vous devriez envisager d'utiliser TypeScript.
Il s'agit d'une excellente alternative au JavaScript normal, car il vous offre un typage statique en plus de la syntaxe JavaScript standard. 
Le problème de la vérification manuelle du type de JavaScript normal est que le faire correctement nécessite tellement de verbiage supplémentaire que la fausse "sécurité de type" que vous obtenez
ne compense pas la perte de lisibilité. Gardez votre JavaScript propre, écrivez de bons tests et faites de bonnes revues de code. Sinon, faites tout cela mais avec TypeScript (qui, comme je l'ai dit, est une excellente alternative !).

**Mauvais :**

```javascript
function combine(val1, val2) {
    if (
        (typeof val1 === "number" && typeof val2 === "number") ||
        (typeof val1 === "string" && typeof val2 === "string")
    ) {
        return val1 + val2;
    }
    throw new Error("Must be of type String or Number");
}
```

**Bien :**

```javascript
function combine(val1, val2) {
    return val1 + val2;
}
```

**[⬆ retour en haut](#sommaire)**

### Ne pas suroptimiser

Les navigateurs modernes effectuent beaucoup d'optimisation sous le capot au moment de l'exécution.
Souvent, si vous optimisez, vous perdez votre temps.
[Il existe de bonnes ressources](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers) pour voir où l'optimisation fait défaut.
Ciblez-les entre-temps, jusqu'à ce qu'ils soient corrigés, si possible.

**Mauvais :**
```javascript
// Sur les anciens navigateurs, chaque itération avec `list.length` non mis en cache serait coûteuse
// à cause du recalcul de `list.length`. 
// Dans les navigateurs modernes, ceci est optimisé.
for (let i = 0, len = list.length; i < len; i++) {
    // ...
}
```

**Bien :**

```javascript
for (let i = 0; i < list.length; i++) {
    // ...
}
```

**[⬆ retour en haut](#sommaire)**

### Supprimer le code mort

Le code mort est tout aussi mauvais que le code dupliqué.
Il n'y a aucune raison de le garder dans votre base de code. S'il n'est pas appelé, débarrassez-vous-en !
Il sera toujours en sécurité dans votre historique de versions si vous en avez encore besoin.

**Mauvais :**

```javascript
function oldRequestModule(url) {
    // ...
}
function newRequestModule(url) {
    // ...
}
const req = newRequestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```

**Bien :**

```javascript
function newRequestModule(url) {
    // ...
}
const req = newRequestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```

**[⬆ retour en haut](#sommaire)**

## **Objets et structures de données**

### Utiliser les getters et setters

L'utilisation de getters et setters pour accéder aux données des objets peut s'avérer plus efficace que la simple recherche d'une propriété sur un objet.
Vous vous demandez peut-être pourquoi ?

Eh bien, voici une liste non organisée de raisons :

- Lorsque vous voulez faire plus que d'obtenir une propriété d'un objet, vous n'avez pas besoin de rechercher et de modifier chaque accesseur dans votre codebase.
- Rend l'ajout de la validation simple quand on fait un `set`.
- Encapsule la représentation interne.
- Il est facile d'ajouter la journalisation et la gestion des erreurs lors de l'obtention et de la définition.
- Vous pouvez charger paresseusement (lazy loading) les propriétés de votre objet, par exemple en le récupérant depuis un serveur.

**Mauvais :**

```javascript
function makeBankAccount() {
    // ...
    return {
        balance: 0
        // ...
    };
}
const account = makeBankAccount();
account.balance = 100;
```

**Bien :**

```javascript
function makeBankAccount() {
    // celle-ci est privée
    let balance = 0 ;
    // un "getter", rendu public via l'objet retourné ci-dessous
    function getBalance() {
        return balance;
    }
    // un "setter", rendu public via l'objet retourné ci-dessous
    function setBalance(amount) {
        // ... valider avant de mettre à jour le solde
        balance = amount;
    }
    return {
        // ...
        getBalance,
        setBalance
    } ;
}
const account = makeBankAccount();
account.setBalance(100);
```

**[⬆ retour en haut](#sommaire)**

### Faites en sorte que les objets aient des membres privés

Cela peut être réalisé par le biais de __closures__ (pour ES5 et inférieur).

**Mauvais :**

```javascript
const Employee = function(name) {
    this.name = name;
};
Employee.prototype.getName = function getName() {
    return this.name;
};
const employee = new Employee("John Doe");
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // Employee name: undefined
```

**Bien :**

```javascript
function makeEmployee(name) {
    return {
        getName() {
            return name;
        }
    };
}
const employee = makeEmployee("John Doe");
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
```

**[⬆ retour en haut](#sommaire)**

## **Classes**

### Préférer les classes ES2015/ES6 aux fonctions simples ES5

Il est très difficile d'obtenir un héritage de classe, une construction et des définitions de méthodes lisibles pour les classes ES5 classiques.
Si vous avez besoin de l'héritage (et sachez que vous pourriez ne pas en avoir besoin), alors préférez les classes ES2015/ES6.
Cependant, préférez les petites fonctions aux classes jusqu'à ce que vous ayez besoin d'objets plus grands et plus complexes.

**Mauvais :**

```javascript
const Animal = function(age) {
    if (!(this instanceof Animal)) {
        throw new Error("Instantiate Animal with `new`");
    }
    this.age = age;
};
Animal.prototype.move = function move() {};
const Mammal = function(age, furColor) {
    if (!(this instanceof Mammal)) {
        throw new Error("Instantiate Mammal with `new`");
    }
    Animal.call(this, age);
    this.furColor = furColor;
};
Mammal.prototype = Object.create(Animal.prototype);
Mammal.prototype.constructor = Mammal;
Mammal.prototype.liveBirth = function liveBirth() {};
const Human = function(age, furColor, languageSpoken) {
    if (!(this instanceof Human)) {
        throw new Error("Instantiate Human with `new`");
    }
    Mammal.call(this, age, furColor);
    this.languageSpoken = languageSpoken;
};
Human.prototype = Object.create(Mammal.prototype);
Human.prototype.constructor = Human;
Human.prototype.speak = function speak() {};
```

**Bien :**

```javascript
class Animal {
    constructor(age) {
        this.age = age;
    }
    move() {
        /* ... */
    }
}
class Mammal extends Animal {
    constructor(age, furColor) {
        super(age);
        this.furColor = furColor;
    }
    liveBirth() {
        /* ... */
    }
}
class Human extends Mammal {
    constructor(age, furColor, languageSpoken) {
        super(age, furColor);
        this.languageSpoken = languageSpoken;
    }
    speak() {
        /* ... */
    }
}
```

**[⬆ retour en haut](#sommaire)**

### Utiliser le chaînage de méthodes

Ce modèle est très utile en JavaScript et vous le voyez dans de nombreuses bibliothèques telles que jQuery et Lodash.
Il permet à votre code d'être explicite et moins verbeux.
Pour cette raison, je vous conseille d'utiliser le chaînage de méthodes et regardez comment votre code sera propre.
Dans vos fonctions de classe, retournez simplement `this` à la fin de chaque fonction, et vous pouvez enchaîner d'autres méthodes de classe sur celle-ci.

**Mauvais :**

```javascript
class Car {
    constructor(make, model, color) {
        this.make = make;
        this.model = model;
        this.color = color;
    }
    setMake(make) {
        this.make = make;
    }
    setModel(model) {
        this.model = model;
    }
    setColor(color) {
        this.color = color;
    }
    save() {
        console.log(this.make, this.model, this.color);
    }
}
const car = new Car("Ford", "F-150", "red");
car.setColor("pink");
car.save();
```

**Bien :**

```javascript
class Car {
    constructor(make, model, color) {
        this.make = make;
        this.model = model;
        this.color = color;
    }
    setMake(make) {
        this.make = make;
        // NOTE: Renvoyer 'this' pour le chaînage
        return this;
    }
    setModel(model) {
        this.model = model;
        // NOTE: Renvoyer 'this' pour le chaînage
        return this;
    }
    setColor(color) {
        this.color = color;
        // NOTE: Renvoyer 'this' pour le chaînage
        return this;
    }
    save() {
        console.log(this.make, this.model, this.color);
        // NOTE: Renvoyer 'this' pour le chaînage
        return this;
    }
}
const car = new Car("Ford", "F-150", "red").setColor("pink").save();
```

**[⬆ retour en haut](#sommaire)**

### Préférez la composition à l'héritage

Comme l'indique le célèbre Gang of Four dans [_Design Patterns_](https://en.wikipedia.org/wiki/Design_Patterns), vous devez préférer la composition à l'héritage lorsque vous le pouvez.
Il y a beaucoup de bonnes raisons d'utiliser l'héritage et beaucoup de bonnes raisons d'utiliser la composition.
Le point principal de cette maxime est que si votre esprit se dirige instinctivement vers l'héritage, demandez-vous si la composition pourrait mieux modéliser votre problème. Dans certains cas, c'est possible.

Vous vous demandez peut-être alors : "Quand dois-je utiliser l'héritage ?" Cela dépend de votre problème à portée de main, mais voici une liste décente des cas où l'héritage a plus de sens que la composition :

1. Votre héritage représente une relation "est-un" et non une relation "a-un" (Human->Animal vs. User->UserDetails).
2. Vous pouvez réutiliser le code des classes de base (les humains peuvent se déplacer comme tous les animaux).
3. Vous voulez faire des changements globaux aux classes dérivées en changeant une classe de base. (Modifier la dépense calorique de tous les animaux lorsqu'ils se déplacent).

**Mauvais :**

```javascript
class Employee {
    constructor(name, email) {
        this.name = name;
        this.email = email;
    }
    // ...
}
// Mauvais car les employés "ont" des données fiscales. EmployeeTaxData n'est pas un type de Employee
class EmployeeTaxData extends Employee {
    constructor(ssn, salary) {
        super();
        this.ssn = ssn;
        this.salary = salary;
    }
    // ...
}
```

**Bien :**

```javascript
class EmployeeTaxData {
    constructor(ssn, salary) {
        this.ssn = ssn;
        this.salary = salary;
    }
    // ...
}
class Employee {
    constructor(name, email) {
        this.name = name;
        this.email = email;
    }
    setTaxData(ssn, salary) {
        this.taxData = new EmployeeTaxData(ssn, salary);
    }
    // ...
}
```

**[⬆ retour en haut](#sommaire)**

## **SOLID**

### Principe de responsabilité unique (SRP)

Comme indiqué dans Clean Code, "Il ne devrait jamais y avoir plus d'une raison pour qu'une classe soit modifiée".
Il est tentant de bourrer une classe d'un grand nombre de fonctionnalités, comme lorsque vous ne pouvez prendre qu'une seule valise sur votre vol en avion.
Le problème est que votre classe ne sera pas conceptuellement cohérente et qu'elle aura de nombreuses raisons de changer.
Il est important de minimiser le nombre de fois où vous devez changer une classe.
C'est important parce que si trop de fonctionnalités se trouvent dans une classe et que vous en modifiez une partie, il peut être difficile de comprendre comment cela affectera les autres modules dépendants de votre codebase.

**Mauvais :**

```javascript
class UserSettings {
    constructor(user) {
        this.user = user;
    }
    changeSettings(settings) {
        if (this.verifyCredentials()) {
            // ...
        }
    }
    verifyCredentials() {
        // ...
    }
}
```

**Bien :**

```javascript
class UserAuth {
    constructor(user) {
        this.user = user;
    }
    verifyCredentials() {
        // ...
    }
}
class UserSettings {
    constructor(user) {
        this.user = user;
        this.auth = new UserAuth(user);
    }
    changeSettings(settings) {
        if (this.auth.verifyCredentials()) {
            // ...
        }
    }
}
```

**[⬆ retour en haut](#sommaire)**

### Principe ouvert/fermé (OCP)

Comme le dit Bertrand Meyer, "les entités logicielles (classes, modules, fonctions, etc.) doivent être ouvertes à l'extension, mais fermées à la modification." 
Mais qu'est-ce que cela signifie ? 
Ce principe stipule essentiellement que vous devez permettre aux utilisateurs d'ajouter de nouvelles fonctionnalités sans modifier le code existant.

**Mauvais :**

```javascript
class AjaxAdapter extends Adapter {
    constructor() {
        super();
        this.name = "ajaxAdapter";
    }
}
class NodeAdapter extends Adapter {
    constructor() {
        super();
        this.name = "nodeAdapter";
    }
}
class HttpRequester {
    constructor(adapter) {
        this.adapter = adapter;
    }
    fetch(url) {
        if (this.adapter.name === "ajaxAdapter") {
            return makeAjaxCall(url).then(response => {
                // transform response and return
            });
        } else if (this.adapter.name === "nodeAdapter") {
            return makeHttpCall(url).then(response => {
                // transform response and return
            });
        }
    }
}
function makeAjaxCall(url) {
    // request and return promise
}
function makeHttpCall(url) {
    // request and return promise
}
```

**Bien :**

```javascript
class AjaxAdapter extends Adapter {
    constructor() {
        super();
        this.name = "ajaxAdapter";
    }
    request(url) {
        // request and return promise
    }
}
class NodeAdapter extends Adapter {
    constructor() {
        super();
        this.name = "nodeAdapter";
    }
    request(url) {
        // request and return promise
    }
}
class HttpRequester {
    constructor(adapter) {
        this.adapter = adapter;
    }
    fetch(url) {
        return this.adapter.request(url).then(response => {
            // transform response and return
        });
    }
}
```

**[⬆ retour en haut](#sommaire)**

### Principe de substitution de Liskov (LSP)

Il s'agit d'un terme effrayant pour un concept très simple. 
Il est formellement défini comme suit : "Si S est un sous-type de T, alors les objets de type T peuvent être remplacés par des objets de type S sans altérer aucune des propriétés désirées de ce programme (correction, tâche exécutée, etc.)". 
C'est une définition encore plus effrayante.

La meilleure explication pour cela est que si vous avez une classe parent et une classe enfant, alors la classe de base et la classe enfant peuvent être utilisées de manière interchangeable sans obtenir de résultats incorrects. Cela peut encore prêter à confusion, alors examinons l'exemple classique du carré et du rectangle.

Mathématiquement, un carré est un rectangle, mais si vous le modélisez en utilisant la relation "est-un" via l'héritage, vous aurez rapidement des problèmes.

**Mauvais :**

```javascript
class Rectangle {
    constructor() {
        this.width = 0;
        this.height = 0;
    }
    setColor(color) {
        // ...
    }
    render(area) {
        // ...
    }
    setWidth(width) {
        this.width = width;
    }
    setHeight(height) {
        this.height = height;
    }
    getArea() {
        return this.width * this.height;
    }
}
class Square extends Rectangle {
    setWidth(width) {
        this.width = width;
        this.height = width;
    }
    setHeight(height) {
        this.width = height;
        this.height = height;
    }
}
function renderLargeRectangles(rectangles) {
    rectangles.forEach(rectangle => {
        rectangle.setWidth(4);
        rectangle.setHeight(5);
        const area = rectangle.getArea(); // Mauvais: Retourne 25 pour Square. Devrait retourner 20.
        rectangle.render(area);
    });
}
const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);
```

**Bien :**

```javascript
class Shape {
    setColor(color) {
        // ...
    }
    render(area) {
        // ...
    }
}
class Rectangle extends Shape {
    constructor(width, height) {
        super();
        this.width = width;
        this.height = height;
    }
    getArea() {
        return this.width * this.height;
    }
}
class Square extends Shape {
    constructor(length) {
        super();
        this.length = length;
    }
    getArea() {
        return this.length * this.length;
    }
}
function renderLargeShapes(shapes) {
    shapes.forEach(shape => {
        const area = shape.getArea();
        shape.render(area);
    });
}
const shapes = [new Rectangle(4, 5), new Rectangle(4, 5), new Square(5)];
renderLargeShapes(shapes);
```

**[⬆ retour en haut](#sommaire)**

### Principe de ségrégation des interfaces (ISP)

JavaScript n'ayant pas d'interfaces, ce principe ne s'applique pas aussi strictement que les autres.
Cependant, il est important et pertinent même avec l'absence de système de types en JavaScript.

L'ISP stipule que "les clients ne doivent pas être forcés de dépendre d'interfaces qu'ils n'utilisent pas".
Les interfaces sont des contrats implicites en JavaScript en raison de l'absence de typage.

Un bon exemple à regarder qui démontre ce principe en JavaScript est pour les classes qui nécessitent de grands objets de paramétrage.
Il est avantageux de ne pas demander aux clients de configurer un grand nombre d'options, car la plupart du temps, ils n'auront pas besoin de tous les paramètres.
Les rendre optionnels permet d'éviter d'avoir une "grosse interface".

**Mauvais :**

```javascript
class DOMTraverser {
    constructor(settings) {
        this.settings = settings;
        this.setup();
    }
    setup() {
        this.rootNode = this.settings.rootNode;
        this.settings.animationModule.setup();
    }
    traverse() {
        // ...
    }
}
const $ = new DOMTraverser({
    rootNode: document.getElementsByTagName("body"),
    animationModule() {} // La plupart du temps, nous n'aurons pas besoin d'animer lors de la traversée.
    // ...
});
```

**Bien :**

```javascript
class DOMTraverser {
    constructor(settings) {
        this.settings = settings;
        this.options = settings.options;
        this.setup();
    }
    setup() {
        this.rootNode = this.settings.rootNode;
        this.setupOptions();
    }
    setupOptions() {
        if (this.options.animationModule) {
            // ...
        }
    }
    traverse() {
        // ...
    }
}
const $ = new DOMTraverser({
    rootNode: document.getElementsByTagName("body"),
    options: {
        animationModule() {}
    }
});
```

**[⬆ retour en haut](#sommaire)**

### Principe d'inversion des dépendances (DIP)

Ce principe énonce deux choses essentielles :

1. Les modules de haut niveau ne doivent pas dépendre des modules de bas niveau. Les deux doivent dépendre d'abstractions.
2. Les abstractions ne doivent pas dépendre des détails. Les détails doivent dépendre des abstractions.

Cela peut être difficile à comprendre au début, mais si vous avez travaillé avec AngularJS, vous avez vu une mise en œuvre de ce principe sous la forme de l'injection de dépendances (DI).
Bien qu'il ne s'agisse pas de concepts identiques, l'injection de dépendance empêche les modules de haut niveau de connaître les détails de ses modules de bas niveau et de les mettre en place.
Il peut y parvenir grâce à l'injection de dépendance. L'un des grands avantages de cette méthode est qu'elle réduit le couplage entre les modules.
Le couplage est un très mauvais modèle de développement car il rend votre code difficile à remanier.

Comme indiqué précédemment, JavaScript n'a pas d'interfaces et les abstractions dont on dépend sont des contrats implicites.
C'est-à-dire les méthodes et les propriétés qu'un objet/classe expose à un autre objet/classe.
Dans l'exemple ci-dessous, le contrat implicite est que tout module de requête pour un `InventoryTracker` aura une méthode `requestItems`.

**Mauvais :**

```javascript
class InventoryRequester {
    constructor() {
        this.REQ_METHODS = ["HTTP"];
    }
    requestItem(item) {
        // ...
    }
}
class InventoryTracker {
    constructor(items) {
        this.items = items;
        // MAUVAIS : Nous avons créé une dépendance sur une implémentation de requête spécifique.
        // Nous devrions simplement faire dépendre les requestItems d'une méthode de requête : `request`.
        this.requester = new InventoryRequester();
    }
    requestItems() {
        this.items.forEach(item => {
            this.requester.requestItem(item);
        });
    }
}
const inventoryTracker = new InventoryTracker(["apples", "bananas"]);
inventoryTracker.requestItems();
```

**Bien :**

```javascript
class InventoryTracker {
    constructor(items, requester) {
        this.items = items;
        this.requester = requester;
    }
    requestItems() {
        this.items.forEach(item => {
            this.requester.requestItem(item);
        });
    }
}
class InventoryRequesterV1 {
    constructor() {
        this.REQ_METHODS = ["HTTP"];
    }
    requestItem(item) {
        // ...
    }
}
class InventoryRequesterV2 {
    constructor() {
        this.REQ_METHODS = ["WS"];
    }
    requestItem(item) {
        // ...
    }
}
// En construisant nos dépendances en externe et en les injectant, nous pouvons facilement
// remplacer notre module de requête par un nouveau module fantaisiste qui utilise les WebSockets.
const inventoryTracker = new InventoryTracker(
    ["apples", "bananas"],
    new InventoryRequesterV2()
);
inventoryTracker.requestItems();
```

**[⬆ retour en haut](#sommaire)**

## **Tests**

Les tests sont plus importants que la livraison.
Si vous n'avez pas de tests ou si vous en avez une quantité insuffisante, à chaque fois que vous livrerez du code, vous ne serez jamais sûr de n'avoir rien cassé.
C'est à votre équipe de décider ce qui constitue une quantité adéquate, mais une couverture à 100% (toutes les déclarations et branches) est le moyen d'obtenir une confiance très élevée et la tranquillité des développeurs.
Cela signifie qu'en plus de disposer d'un excellent cadre de test, vous devez également utiliser un [bon outil de couverture](https://gotwarlost.github.io/istanbul/).

Il n'y a aucune excuse pour ne pas écrire de tests. Il existe [une multitude de bons cadres de test JS](https://jstherightway.org/#testing-tools), trouvez-en un que votre équipe préfère.
Lorsque vous aurez trouvé celui qui convient à votre équipe, efforcez-vous de toujours écrire des tests pour chaque nouvelle fonctionnalité/module que vous introduisez.
Si votre méthode préférée est le développement piloté par les tests (TDD), c'est parfait, mais l'essentiel est de vous assurer que vous atteignez vos objectifs de couverture avant de lancer une fonctionnalité ou de remanier une fonctionnalité existante.

### Un seul concept par test

**Mauvais :**

```javascript
import assert from "assert";
describe("MomentJS", () => {
    it("handles date boundaries", () => {
        let date;
        date = new MomentJS("1/1/2015");
        date.addDays(30);
        assert.equal("1/31/2015", date);
        date = new MomentJS("2/1/2016");
        date.addDays(28);
        assert.equal("02/29/2016", date);
        date = new MomentJS("2/1/2015");
        date.addDays(28);
        assert.equal("03/01/2015", date);
    });
});
```

**Bien :**

```javascript
import assert from "assert";
describe("MomentJS", () => {
    it("handles 30-day months", () => {
        const date = new MomentJS("1/1/2015");
        date.addDays(30);
        assert.equal("1/31/2015", date);
    });
    it("handles leap year", () => {
        const date = new MomentJS("2/1/2016");
        date.addDays(28);
        assert.equal("02/29/2016", date);
    });
    it("handles non-leap year", () => {
        const date = new MomentJS("2/1/2015");
        date.addDays(28);
        assert.equal("03/01/2015", date);
    });
});
```

**[⬆ retour en haut](#sommaire)**

## **Concurrence**

### Utiliser des promesses, pas des callbacks

Les callbacks ne sont pas propres, et ils provoquent des quantités excessives d'imbrication. Avec ES2015/ES6,
les promesses sont un type global intégré. Utilisez-les !

**Mauvais :**

```javascript
import { get } from "request";
import { writeFile } from "fs";
get(
    "https://en.wikipedia.org/wiki/Robert_Cecil_Martin",
    (requestErr, response, body) => {
        if (requestErr) {
            console.error(requestErr);
        } else {
            writeFile("article.html", body, writeErr => {
                if (writeErr) {
                    console.error(writeErr);
                } else {
                    console.log("File written");
                }
            });
        }
    }
);
```

**Bien :**

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-extra";
get("https://en.wikipedia.org/wiki/Robert_Cecil_Martin")
    .then(body => {
        return writeFile("article.html", body);
    })
    .then(() => {
        console.log("File written");
    })
    .catch(err => {
        console.error(err);
    });
```

**[⬆ retour en haut](#sommaire)**

### Async/Await sont encore plus propres que les promesses

Les promesses sont une alternative très propre aux callbacks, mais ES2017/ES8 apporte async et await qui offrent une solution encore plus propre.
Tout ce dont vous avez besoin est une fonction qui est préfixée par un mot-clé `async`, et ensuite vous pouvez écrire votre logique de manière impérative sans
une chaîne de fonctions `then`. Utilisez ceci si vous pouvez profiter des fonctionnalités de ES2017/ES8 dès aujourd'hui !

**Mauvais :**

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-extra";
get("https://en.wikipedia.org/wiki/Robert_Cecil_Martin")
    .then(body => {
        return writeFile("article.html", body);
    })
    .then(() => {
        console.log("File written");
    })
    .catch(err => {
        console.error(err);
    });
```

**Bien :**

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-extra";
async function getCleanCodeArticle() {
    try {
        const body = await get(
            "https://en.wikipedia.org/wiki/Robert_Cecil_Martin"
        );
        await writeFile("article.html", body);
        console.log("File written");
    } catch (err) {
        console.error(err);
    }
}
getCleanCodeArticle()
```

**[⬆ retour en haut](#sommaire)**

## **Gestion des erreurs**

Les erreurs capturées sont une bonne chose !
Elles signifient que le runtime a réussi à identifier le moment où quelque chose dans votre programme a mal tourné et il vous le fait savoir en arrêtant l'exécution de la fonction sur la pile actuelle, en tuant le processus (dans Node), et en vous notifiant dans la console avec une trace de pile (stack trace).

### N'ignorez pas les erreurs détectées

Ne rien faire face à une erreur détectée ne vous donne pas la possibilité de la corriger ou de réagir à cette erreur. Enregistrer l'erreur dans la console (`console.log`) n'est pas beaucoup mieux car souvent elle peut se perdre dans un océan d'affichages par la console.
Si vous enveloppez un bout de code dans un `try/catch`, cela signifie que vous pensez qu'une erreur peut se produire à cet endroit et que vous devez donc avoir un plan, ou créer un chemin de code, pour le cas où elle se produirait.

**Mauvais :**

```javascript
try {
    functionThatMightThrow();
} catch (error) {
    console.log(error);
}
```

**Bien :**

```javascript
try {
    functionThatMightThrow();
} catch (error) {
    // Une option (plus percutant que console.log) :
    console.error(error) ;
    // Une autre option :
    notifyUserOfError(error) ;
    // Une autre option :
    reportErrorToService(error) ;
    // OU faites les trois !
}
```

### N'ignorez pas les promesses rejetées

Pour la même raison, vous ne devez pas ignorer les erreurs attrapées de `try/catch`.

**Mauvais :**

```javascript
getdata()
    .then(data => {
        functionThatMightThrow(data);
    })
    .catch(error => {
        console.log(error);
    });
```

**Bien :**

```javascript
getdata()
    .then(data => {
        functionThatMightThrow(data);
    })
    .catch(error => {
        // Une option (plus percutant que console.log) :
        console.error(error) ;
        // Une autre option :
        notifyUserOfError(error) ;
        // Une autre option :
        reportErrorToService(error) ;
        // OU faites les trois !
    });
```

**[⬆ retour en haut](#sommaire)**

## **Formatage**

Le formatage est subjectif. Comme beaucoup de règles dans le présent document, il n'y a pas de règle absolue que vous devez suivre.
L'essentiel est de NE PAS SE BATTRE sur le formatage.
Il existe [des tonnes d'outils](https://standardjs.com/rules.html) pour automatiser ce processus.
Utilisez-en un ! C'est une perte de temps et d'argent pour les ingénieurs que de se disputer sur le formatage.

Pour les choses qui ne relèvent pas du formatage automatique (indentation, tabulations ou espaces, guillemets doubles ou simples, etc.), voici quelques conseils.

### Utiliser des majuscules cohérentes

JavaScript n'étant pas typé, les majuscules en disent long sur vos variables, fonctions, etc. Ces règles sont subjectives, donc votre équipe peut choisir ce qu'elle veut.
Le fait est que, peu importe ce que vous choisissez, soyez cohérent.

**Mauvais :**

```javascript
const DAYS_IN_WEEK = 7;
const daysInMonth = 30;
const songs = ["Back In Black", "Stairway to Heaven", "Hey Jude"];
const Artists = ["ACDC", "Led Zeppelin", "The Beatles"];
function eraseDatabase() {}
function restore_database() {}
class animal {}
class Alpaca {}
```

**Bien :**

```javascript
const DAYS_IN_WEEK = 7;
const DAYS_IN_MONTH = 30;
const SONGS = ["Back In Black", "Stairway to Heaven", "Hey Jude"];
const ARTISTS = ["ACDC", "Led Zeppelin", "The Beatles"];
function eraseDatabase() {}
function restoreDatabase() {}
class Animal {}
class Alpaca {}
```

**[⬆ retour en haut](#sommaire)**

### Les fonctions appelantes et appelées doivent être proches

Si une fonction en appelle une autre, gardez ces fonctions verticalement proches dans le fichier source.
Idéalement, gardez l'appelant juste au-dessus de l'appelé. Nous avons tendance à lire le code de haut en bas, comme un journal.
Pour cette raison, faites en sorte que votre code se lise de cette façon.

**Mauvais :**

```javascript
class PerformanceReview {
    constructor(employee) {
        this.employee = employee;
    }
    lookupPeers() {
        return db.lookup(this.employee, "peers");
    }
    lookupManager() {
        return db.lookup(this.employee, "manager");
    }
    getPeerReviews() {
        const peers = this.lookupPeers();
        // ...
    }
    perfReview() {
        this.getPeerReviews();
        this.getManagerReview();
        this.getSelfReview();
    }
    getManagerReview() {
        const manager = this.lookupManager();
    }
    getSelfReview() {
        // ...
    }
}
const review = new PerformanceReview(employee);
review.perfReview();
```

**Bien :**

```javascript
class PerformanceReview {
    constructor(employee) {
        this.employee = employee;
    }
    perfReview() {
        this.getPeerReviews();
        this.getManagerReview();
        this.getSelfReview();
    }
    getPeerReviews() {
        const peers = this.lookupPeers();
        // ...
    }
    lookupPeers() {
        return db.lookup(this.employee, "peers");
    }
    getManagerReview() {
        const manager = this.lookupManager();
    }
    lookupManager() {
        return db.lookup(this.employee, "manager");
    }
    getSelfReview() {
        // ...
    }
}
const review = new PerformanceReview(employee);
review.perfReview();
```

**[⬆ retour en haut](#sommaire)**

## **Commentaires**

### Ne commentez que les éléments dont la logique métier est complexe.

Les commentaires sont une excuse, pas une exigence. Un bon code se documente __principalement__ de lui-même.

**Mauvais :**

```javascript
function hashIt(data) {
    // Le hachage
    let hash = 0 ;
    // Longueur de la chaîne
    const length = data.length ;
    // Boucle sur chaque caractère des données
    for (let i = 0 ; i < length ; i++) {
        // Obtention du code du caractère.
        const char = data.charCodeAt(i) ;
        // Créer le hachage
        hash = (hash << 5) - hash + char ;
        // Convertit en un entier de 32 bits
        hash &= hash ;
    }
}
```

**Bien :**

```javascript
function hashIt(data) {
    let hash = 0;
    const length = data.length;
    for (let i = 0; i < length; i++) {
        const char = data.charCodeAt(i);
        hash = (hash << 5) - hash + char;
        // Convertit en un nombre entier de 32 bits
        hash &= hash;
    }
}
```

**[⬆ retour en haut](#sommaire)**

### Ne laissez pas de code commenté dans votre base de données.

Le contrôle de version existe pour une raison. Laissez l'ancien code dans votre historique.

**Mauvais :**

```javascript
doStuff();
// doOtherStuff();
// doSomeMoreStuff();
// doSoMuchStuff();
```

**Bien :**

```javascript
doStuff();
```

**[⬆ retour en haut](#sommaire)**

### Ne pas avoir de commentaires de journal

Rappelez-vous, utilisez le contrôle de version ! Il n'y a pas besoin de code mort, de code commenté, et surtout de commentaires de journal.
Utilisez `git log` pour obtenir l'historique !

**Mauvais :**

```javascript
/**
 * 2016-12-20 : Suppression des monades, je ne les comprenais pas (RM).
 * 2016-10-01 : Amélioration de l'utilisation des monades spéciales (JP)
 * 2016-02-03 : Suppression du contrôle de type (LI)
 * 2015-03-14 : Ajouté la combinaison avec le contrôle de type (JR)
 */
function combine(a, b) {
    return a + b;
}
```

**Bien :**

```javascript
function combine(a, b) {
    return a + b;
}
```

**[⬆ retour en haut](#sommaire)**

### Évitez les marqueurs de position

Ils ne font généralement que brouiller. Laissez les fonctions et les noms de variables, ainsi que l'indentation et le formatage appropriés donner la structure visuelle de votre code.

**Mauvais :**

```javascript
////////////////////////////////////////////////////////////////////////////////
// Instanciation du modèle d'application
////////////////////////////////////////////////////////////////////////////////
$scope.model = {
    menu: "foo",
    nav: "bar"
};
////////////////////////////////////////////////////////////////////////////////
// Configuration de l'action
////////////////////////////////////////////////////////////////////////////////
const actions = function() {
    // ...
};
```

**Bien :**

```javascript
$scope.model = {
    menu: "foo",
    nav: "bar"
};
const actions = function() {
    // ...
};
```

**[⬆ retour en haut](#sommaire)**