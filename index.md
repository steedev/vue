## Virtual DOM

Sam js wykonuje swoje zadania bardzo szybko, jednak w momencie odwołania do `document` wysyła on zapytanie do `C++` i to już nie jest optymalnym rozwiązaniem, dlatego używamy `Virtual DOM`.

Kod html jest odwzorowywany w js, a następnie tworzony jest snapshot, w momencie kiedy dane ulegną zmianie sytuacja zostanie powtórzona. Nowy snapshot zostanie porównany ze starym, zmiany będą kolejkowane aby jak najoptymalniej wyrenderować grupę elementów z danego rodzica. Jak najrzadziej sięgamy do obiektowego modelu dokumentu.

## Vue

```html
<div class="container grid-lg">
  <div id="app" v-cloak>
    <h1>{{ heading }}</h1>
    <p>{{ toUpperCase(text) }}</p>
    <p>Name: {{ name }}</p>
  </div>
</div>
>
```

```js
new Vue({
  el: "#app",
  data: {
    heading: "Hello world",
    text: "Welcome in Vue.js",
    name: "",
  },
  methods: {
    toUpperCase(value) {
      return value.toUpperCase();
    },
    getName() {
      this.name = prompt("Type your name:");
    },
  },
});
```

Wszystko z `data & methods` jest `proxowane` na główną instancję, dlatego możemy się odwołać do nich po `this`.

### Computed properties

Metody są wywoływane za każdym `rerenderingiem` w przeciwieństwie do `computed` potrafi sobie przeanalizować czy wartości zostały zmienione. Nie powinniśmy zwracać z wyliczonych właściwości jakiś rzeczy, które nie korzystają gdzieś z `this`. Przydatne przy tworzeniu obiektu daty.

```js
computed: {
  fullName() {
    return `${this.firstName} ${this.lastName}`;
  },
},
```

### Interpolacja

```html
<h1>{{ fullName }}</h1>
```

### Dyrektywa

```html
<h1 v-text="fullName"></h1>
```

```html
<h1 v-html="embolden(fullName)"></h1>
```

Jeśli dane mają się wczytać raz i nie mają być reaktywne to możemy użyć `v-once`

```html
<h1 v-html="embolden(fullName)" v-once></h1>
```

Atrybuty są iterowane po wartościach pseudo tablicy

```html
$0.attributes NamedNodeMap {0: href, href: href, length: 1}
```

### Shortcut

```html
<a v-bind:href="url">{{ url }}</a>
```

```html
<a :href="url">{{ url }}</a>
```

### Wykrywanie zmian

Vue bazuje na `getterach & setterach`, w momencie wywołanie `settera` zostaje uruchomione sprawdzenie virtualnego domu i podmiana wartości.

### Single source of truth

```html
$vm0.items (3) ['sok', 'melko', 'chleb', __ob__: Observer]
$vm0.items.push("hello")
```

Podczas wykonania metody `push` nie wywoła się setter. Vue wie, że items to tablica, defaultowa metoda zostaje zastąpiona nową metodą, która wywoła defaultową metodę oraz analizę przerenderowania widoku.

#### Opakowane metody:

- push
- pop
- shift
- unshift
- splice
- sort
- reverse

#### `Ciekawostka`

Vue potrafi porównać dwie tablice i jeżeli np.: bazuje na nich renderowanie elementów listy, to zostawi na stronie te elementy, które należą do zbioru wspólnego. Wpływa to na wydajność.

### Montowanie aplikacji

```js
const vm = new Vue({
  template: "<h1>{{ fullName }}</h1>",
  data: {
    firstName: "Jan",
    lastName: "Kowalski",
  },
  computed: {
    fullName() {
      return `${this.firstName} ${this.lastName}`;
    },
  },
});

vm.$mount();

setTimeout(() => {
  document.querySelector("#app").appendChild(vm.$el);
}, 1000);
```

Możemy też użyć skrypu z dowolną nazwą po `/`, ponieważ jeśli przeglądarka go nie zrozumie to nie wyrzuci o tym błędu

```js
template: "#app-template";
```

W `root` może być tylko jeden element!

### Lifecycle Diagram

![img.png](src/imgs/diagram.png)
