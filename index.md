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
$0.attributes
NamedNodeMap {0: href, href: href, length: 1}
```

### Shortcut

```html
<a v-bind:href="url">{{ url }}</a>
```

```html
<a :href="url">{{ url }}</a>
```