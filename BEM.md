# Documentation BEM
*  Block
*  Element `__`
*  Modifiers(s) `--`
```html
<header class="header">
  <nav class="headerNav">
    <ul class="headerNav__List--xmas">
      <li class="headerNav__ListItem">
        <a href="/about" class="headerNav_ListIitem__Link"></a>
      </li>
      <li class="headerNav__ListItem">
        <a href="/home" class="headerNav_ListIitem__Link"></a>
      </li>
      <li class="headerNav__ListItem">
        <a href="/account" class="headerNav_ListIitem__Link headerNav_ListIitem__Link--active">A propos</a>
      </li>
    </ul>
  </nav>
</header>
```

```sass
  $bptablet: 768 px;
  $bpdesktop: 1024px;

  @mixin bp(val){
    @media (min-width; #{val}){
      @content;
    }
  }
  .headerNav{
    display: flex;
    justify-content: space - between;
    &--xmas{
      background:red;
    }
    &__item{
      text-decoration:none;
      &--isDisabled{
        color:grey;
      }
      &--isActive{
        color:green;
      }
    }
  }
```
## Why SCSS is better than CSS with BEM
```css
.nav{

}
.nav__list{

}
.nav__listItem{

}
.nav__ListItem--isActive{

}
.nav__ListItem--isDisabled{

}
```

# Pseudo attribut ::after & ::before

Les pseudos attributes `before`et `after`sont essentiellment utilisés pour ajouter des éléments
) votre DOM pour décorer sans que cela ait un impact sur le référencemen,t ou votre HTML.

### **ATTENTION**
* Il est obligatoire d'avoir un `content: ''` afin que que vos after / before s'affichent.
* Vous pouvez leur donner donner une taille, une position (absolute par rapport à son parent)
* essentiellement utilisé pour la décoration

```
<section class="cover">
<h2 class="cover__title">Présentation</h2>
</section>
```

```sass
.cover{
  &__title{
    font-size: 24px;
    color:#bbb;
    text-align:center;
    position:relative;
    &::before,
    &::after{
     content:'';
     position:absolute;
     top:0;
     width:50px;
     height:50px;
     background: green;
    }
     &::before{
      left:0;
    }
     &::after{
      right:0;
    }
  }
}
```
## Les unités

### REM
L'unité de mesure REM est basée sur la taille de l'élément racine HTML.
Par défaut, la taille du `<html>` est de 16px, ce qui veut dire que `1 rem = 16px`
Pour éviter de devoir faire des calculs afin de respecter les tailles relatives à votre
maquette, vous pouvez ré-écrire cette font-size afin que `1rem = 10px`.

```css
html{
  font-size:  62.5%;
}
.mainTitle{
  font-size:  2.4rem;
}
.cover{
  width:  32rem;
  }
```

### EM
Le `EM` di 'REM' se base à lui sur la taille de son parent direct. (pas son grand-parent).
On peut donc dire que c'est nul.

## VW

## Les grilds avec Flexboxgrid

* [documentation](http://flexboxgrid.com/)

Après avoir téléchargé la documentation, on récupère le fichier `css/flexboxgrid.min.css` et on le transfert dans
son dossier css. On l'appelle dans son `index.html` comme si-dessous :

```HTML
<link rel="stylesheet" href="css/flexboxgrid.min.css" type="text/css"> <!-- grid au dessus de ton style-->
<link rel="stylesheet" href="css/style.css" type="text/css">
```

# Questions entretient

https://github.com/h5bp/Front-end-Developer-Interview-Questions

# cssnext
[lien](http://cssnext.io/)
ce qui va remplacer le scss.

On peut déclarer nos variables, mettre des themes comme mixin, faire calcul, des custom selector, on peut tout faire, actuellement ce n'est pas supporter partout