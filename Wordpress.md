## WordPress

dans ```fonctions.php```

```php
add_theme_support('post-thumbnails') // permet d'ajouter des images dans
post thumbnails permet d'ajouter des images à la une
```
savoir quel element on doit lier dans notre theme
check le html et voir ce que l'utilisateur a linké
 commen t linké son css et js ?

```add_action('wp_enqueue_scripts')``` des que wordpress fait appelle a cette fonction, elle s'arrete
voir si une autre fonction s'est greffé dessus et on effectue la fonction

permet d'appeler entre autre les styles et js

```
add_action('wp_enqueue_scripts','my_script')
'my_script' contient dans notre cas nos styles et javascript
```

wp_deregister_script('jquery'); -> permet d'appeler notre jquery car le jquery de base de wordpress peut faire deconner notre
code si celui ci est mise a jour

get_stylesheet_directory_uri() permet de mettre en palce l'adressage d'une image(href/src)
exemple :

```html
<img src='<?php echo get_stylesheet_directory_uri()?>/img/lol.png' />
```

ajout d'un lien vers un url front :

```
<?php echo get_home_url() ?>
```

ajout de nav :
```php
function add_menu(){
  register_nav_menu('header-menu','Head menu');
}
add_action('init', 'add_menu')
//cette fonction permet d'ajouter un onglet menu dans wordpress :)
```

afficher le nav :
```php
wp_nav_menu(array(
'theme_location' => 'header-menu',
'container' => ''
));
```

## Gérer la taille de l'image

gérer la taile d'image :

```php
add_image_size('588x272',580,272,true) // ('nomdelataille',hauteur,largeur,crop(true ou false))

the_post_thumbnail('588x272');
//il faut ensuite regener l'image, cf :Regenerate Thumbnails
```

ou

```php
<img src="<?php echo get_the_post_thumbnail_url(get_the_ID(), '588x272') ?>">
<!--il faut ensuite regener l'image, cf :Regenerate Thumbnails-->
```

afficher category_name

```php
category_name => 'blabla' // affiche category
```
--------------------------------------------
# ACF

Ajout un champ text:
titre:
description:
zone de text:
nouvelles lignes: ajouter des br

```php
<?php the_field('mon_champs') ?> //afficher son acf
```
rendre les elements plus visibles :

Crée des type de champs "onglet" (pratique quand on met plusieurs champs text, cela permet de mieux structurer notre travail pour le client)

champs de class acf :

1. créer son identifiant
2. type de champs :selection
3. dans dans choix: mettre à gauche la class et à droite l'identifiant
4. dans le php dans class mettre son field ici ^^

autre methode mieux faire un tableau :
```php
`$data = array(
  'first' => array(
    'item1' => "blablabla",
    'item2' =>'blablabla'
  ),
  'second' => array(
    'item1' => "blablabla",
    'item2' =>'blablabla'
  ),
  'third' => array(
    'item1' => "blablabla",
    'item2' =>'blablabla'
  )
  )`;
  foreach($data as $class => $item):
    ...
```
# Import database & changement serveur local

## import database

```
mysql -u user -p base_de_données* < ficher.sql
```
*La base de donnée doit être vierge

## modifier serveur local

```
UPDATE ton_WP_options* SET option_value = replace (option_value,'http://localhost/different','http://localhost:8000')
WHERE option_name = 'home' or option_name = 'siteurl';
```
*WP_options correspond à ta table d'option de la base de donnée

## expoter sa database

```
mysqldump -u user -ppass base_de_données > fichier_dump.sql
```
