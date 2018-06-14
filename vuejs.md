# vue.js

Vue est un framework qui permet de construire des interfaces utilisateur. À la différence des autres frameworks monolithiques, Vue a été conçu et pensé pour pouvoir être adopté de manière incrémentale. Le cœur de la bibliothèque est concentré uniquement sur la partie vue, et il est vraiment simple de l’intégrer avec d’autres bibliothèques ou projets existants. D’un autre côté, Vue est tout à fait capable de faire tourner des applications web monopages quand il est couplé avec des outils modernes et des bibliothèques complémentaires.

## Rendu déclaratif
Rien de plus simple, entrer votre variable entouré d'une double moustage `{{ variable }}`

Dans le html :

```html
<div id="app">
  {{ message }} <!-- 'message' qui doit se situer dans le data -->
</div>
```

Dans le js, on appelle notre variable comme suit ci-dessous:

```js
var app = new Vue({
  el: '#app',//notre #id
  data: {
    message: 'Hello Vue !' //notre message qui contient 'Hello Vue !'
  }
})
```
## Ajout d'évènement d'un attribut

Entrer dans une balise l’attribut  `v-bind:attribute`,
 et remplacer attribute par l'attribut concernée

```html
<div id="app-2">
  <span v-bind:title="message">
    Passez votre souris sur moi pendant quelques secondes
    pour voir mon titre lié dynamiquement !
  </span>
</div>
```

```js
var app2 = new Vue({
  el: '#app-2', //notre #id
  data: { //#notre data
    message: 'Vous avez affiché cette page le ' + new Date().toLocaleString()
  }
})
```

L'attribut `v-bind` est appelée une **directive**.Les directives sont toujours
préfixées par `v-` pour indiquer qu'on utilise des attributs spécifiques à vue.
