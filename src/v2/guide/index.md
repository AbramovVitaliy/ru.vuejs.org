---
title: Введение
type: guide
order: 2
---

## Что такое Vue.js?

Vue (произносится /vjuː/, примерно как **view**) — это **прогрессивный фреймворк** для создания пользовательских интерфейсов. В отличие от фреймворков-монолитов, Vue разработан с нуля для постепенной его адаптации. Ядро Vue в первую очередь решает задачаи уровня представления (view), а также легко адаптируется и интегрируется с другими библиотеками и существующими проектами. С другой стороны, Vue также отлично подходит для создания сложных одностраничных приложений (SPA, Single-Page Applications) при использовании его с [современными инструментами](single-file-components.html) и [вспомогательными библиотеками](https://github.com/vuejs/awesome-vue#libraries--plugins).

Если вы опытный frontend-разработчик и хотите узнать, чем Vue отличается от остальных библиотек или фреймворков, обратите внимание на [сравнение с другими фреймворками](comparison.html).

## Начало работы

<p class="tip">Официальное руководство предполагает, что вы уже знакомы с HTML, CSS и JavaScript на базовом уровне. Если же вы новичок во frontend-разработке, начинать сразу с изучения фреймворка может быть не лучшей идеей - возвращайтесь, разобравшись с основами! Предшествующий опыт с другими фреймворками может помочь, но не является обязательным.</p>

Проще всего попробовать Vue.js, начав с [примера Hello World на JSFiddle](https://jsfiddle.net/chrisvfritz/50wL7mdz/). Просто откройте его в другой вкладке, и следуйте инструкциям ниже. Также вы можете просто создать `.html` файл и подключить Vue:

``` html
<script src="https://unpkg.com/vue/dist/vue.js"></script>
```

Документация по [установке](installation.html) описывает другие варианты установки Vue. Обратите внимание, что мы **не рекомендуем** новичкам начинать с `vue-cli`, особенно если у них нет опыта работы с инструментами для сборки основанных на Node.js.

## Декларативный рендеринг

В ядре Vue.js находится система, которая позволяет декларативно отображать данные в DOM, используя простой синтаксис шаблонов:

``` html
<div id="app">
  {{ message }}
</div>
```
``` js
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
```
{% raw %}
<div id="app" class="demo">
  {{ message }}
</div>
<script>
var app = new Vue({
  el: '#app',
  data: {
    message: 'Привет Vue!'
  }
})
</script>
{% endraw %}

Вот мы и создали наше первое Vue-приложение! Больше похоже на простой рендеринг шаблона, но Vue выполнил немало работы "под капотом". Данные и DOM теперь **реактивно** связаны. Как это проверить? Просто откройте консоль разработчика в своём браузере, и задайте полю `app.message` другое значение. Вы должны увидеть как сообщение автоматически обновится.

В дополнение к интерполяции текста, мы можем также связывать атрибуты элементов:

``` html
<div id="app-2">
  <span v-bind:title="message">
    Наведите курсор мыши на меня в течение нескольких секунд, чтобы увидеть динамически связанное значение title!
  </span>
</div>
```
``` js
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: 'Вы загрузили данную страницу в ' + new Date()
  }
})
```
{% raw %}
<div id="app-2" class="demo">
  <span v-bind:title="message">
    Наведите курсор мыши на меня в течение нескольких секунд, чтобы увидеть динамически связанное значение title!
  </span>
</div>
<script>
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: 'Вы загрузили данную страницу в ' + new Date()
  }
})
</script>
{% endraw %}

Здесь мы встречаемся с кое-чем новым. Аттрибут `v-bind`, который вы видите, называется **директивой**. Директивы имеют префикс `v-`, указывающий на их особую природу. Как вы уже могли догадаться, они применяют к отображаемому DOM особое реактивное поведение, управляемое Vue. В данном примере директива говорит "сохраняй значение title этого элемента актуальным при изменении свойства `message` у экзепляра Vue".

Если вы снова откроете консоль разработчика в браузере и введёте `app2.message = 'Новое сообщение'`, вы вновь увидите, что связанный атрибут `title` обновился.

## Условия и циклы

Управлять включением элемента в DOM тоже довольно просто:

``` html
<div id="app-3">
  <p v-if="seen">Сейчас элемент видно</p>
</div>
```

``` js
var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})
```

{% raw %}
<div id="app-3" class="demo">
  <span v-if="seen">Сейчас элемент видно</span>
</div>
<script>
var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})
</script>
{% endraw %}

Попробуйте ввести в консоль `app3.seen = false`. Сообщение должно пропасть.

Этот пример демонстрирует возможность связывания данных не только с текстом или атрибутами, но и с DOM **структурой**. Более того, Vue также имеет мощную систему анимаций, которая автоматически применяет [эффекты при переходах](transitions.html) при добавлении/обновлении или удалении элементов.

Есть несколько других директив, каждая из которых выполняет свои специальные функции. Например, директива `v-for` может быть использована для отображения списков, используя данные из массива:

``` html
<div id="app-4">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
```
``` js
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: 'Изучить JavaScript' },
      { text: 'Изучить Vue' },
      { text: 'Сделать чтон-нибудь классное' }
    ]
  }
})
```
{% raw %}
<div id="app-4" class="demo">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
<script>
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: 'Изучить JavaScript' },
      { text: 'Изучить Vue' },
      { text: 'Сделать что-нибудь классное' }
    ]
  }
})
</script>
{% endraw %}

В консоли разработчика введите `app4.todos.push({ text: 'Рассказать всем' })`, и вы увидите, что к списку добавился новый элемент.

## Работа с пользовательским вводом

Чтобы пользователи могли взаимодействовать с вашим приложением, используйте директиву `v-on` для наблюдения за событиями, указав метод-обработчик:

``` html
<div id="app-5">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">Обратить порядок букв в сообщении</button>
</div>
```
``` js
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Привет Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
```
{% raw %}
<div id="app-5" class="demo">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">Обратить порядок букв в сообщении</button>
</div>
<script>
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Привет Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
</script>
{% endraw %}

Обратите внимание, что в методе мы обновляем состояние нашего приложения не трогая DOM — всю работу с DOM производит Vue, а код, который вы пишете, представляет только логику приложения.

Vue также содержит директиву `v-model`, позволяющую легко организовывать двустороннее связывание между формами ввода и состоянием приложения:

``` html
<div id="app-6">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
```
``` js
var app6 = new Vue({
  el: '#app-6',
  data: {
    message: 'Привет Vue!'
  }
})
```
{% raw %}
<div id="app-6" class="demo">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
<script>
var app6 = new Vue({
  el: '#app-6',
  data: {
    message: 'Привет Vue!'
  }
})
</script>
{% endraw %}

## Написание приложения с использованием компонентов

Другой важной концепцией Vue является система компонентов. Эта абстракция позволяет создавать масштабные приложения из маленьких, автономных и часто переиспользуемых компонентов. Если подумать, почти любой интерфейс может быть представлен как дерево компонентов:

![Дерево Компонентов](/images/components.png)

Компонент во Vue — это, по сути, экзепляр Vue с предустановленными опциями. Зарегистрировать новый компонент во Vue очень просто:

``` js
// Определяем новый компонент под названием todo-item
Vue.component('todo-item', {
  template: '<li>Это todo item</li>'
})
```

Теперь вы можете можно использовать его в шаблоне другого компонента:

``` html
<ul>
  <!-- Создаём экземпляр компонента todo-item, созданного выше -->
  <todo-item></todo-item>
</ul>
```

Пока что у нас получилось так, что во всех todo будет содержаться один и тот же текст — это не очень интересно. Хотелось бы иметь возможность передавать данные от родителя в дочерние компоненты. Давайте изменим компонент так, чтобы он мог принимать [входной параметр](components.html#Props):

``` js
Vue.component('todo-item', {
  // Компонент todo-item теперь принимает
  // "prop", то есть пользовательский параметр.
  // Этот параметр мы назвали todo.
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})
```

Теперь можно передать текст задачи в каждый компонент с помощью `v-bind`:

``` html
<div id="app-7">
  <ol>
    <!-- Теперь мы можем передать каждому todo описание задачи, -->
    <!-- то есть отображать данные динамически                  -->
    <todo-item v-for="item in groceryList" v-bind:todo="item"></todo-item>
  </ol>
</div>
```
``` js
Vue.component('todo-item', {
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})

var app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { text: 'Овощи' },
      { text: 'Сыр' },
      { text: 'Что там ещё люди едят?' }
    ]
  }
})
```
{% raw %}
<div id="app-7" class="demo">
  <ol>
    <todo-item v-for="item in groceryList" v-bind:todo="item"></todo-item>
  </ol>
</div>
<script>
Vue.component('todo-item', {
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})
var app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { text: 'Овощи' },
      { text: 'Сыр' },
      { text: 'Что там ещё люди едят?' }
    ]
  }
})
</script>
{% endraw %}

Конечно, этот пример слегка надуман, но посмотрите — нам удалось разделить наше приложение на два меньших объекта, и дочерний оказался в разумной мере отвязан от родительского с помощью интерфейса входящих параметров. Теперь мы можем и далее улучшать наш компонент `<todo-item>`, усложняя шаблон и логику, но не влияя на родительское приложение.

В крупных приложениях разделение на компоненты становится обязательным условием для сохранения управляемости разработки. Разговор о компонентах ещё далеко не закончен — он будет продолжен [далее в этом руководстве](components.html), но уже сейчас можно взглянуть на (вымышленный) пример того, как мог бы выглядеть шаблон приложения, использующего компоненты:

``` html
<div id="app">
  <app-nav></app-nav>
  <app-view>
    <app-sidebar></app-sidebar>
    <app-content></app-content>
  </app-view>
</div>
```

### Связь с Web Components

Вы могли заметить, что компоненты Vue довольно похожи на **Веб-компоненты**, являющиеся частью [спецификации Web Components](http://www.w3.org/wiki/WebComponents/). Дело в том, что синтаксис компонентов Vue намеренно следует этой спецификации. Например, компоненты Vue реализуют [API слотов](https://github.com/w3c/webcomponents/blob/gh-pages/proposals/Slots-Proposal.md) и специальный атрибут `is`. Вместе с тем, есть и несколько ключевых различий:

1. Спецификация Web Components всё ещё находится в статусе черновика, и не реализована ни в одном из браузеров. Компоненты Vue, напротив, не требуют никаких полифиллов и устойчиво работают во всех поддерживаемых браузерах (IE9 и выше). При необходимости компоненты Vue могут быть обёрнуты в нативные пользовательские элементы.

2. Компоненты Vue предоставляют важные возможности, недоступные в простых пользовательких элементах. Самые важные из них: кросс-компонентная передача данных, коммуникация с использованием пользовательских событий и интеграция с инструментами создания сборок.

## Готовы к большему?

Мы всего лишь кратко представили самые основные возможности ядра Vue.js — остальная часть этого руководства посвящена более детальному рассмотрению этих и других возможностей, поэтому убедитесь, что прочитали его целиком!
