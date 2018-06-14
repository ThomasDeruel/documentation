# Documentation wordpress

prérequis :
 installer [php](http://php.net/downloads.php) / [MySql](https://www.mysql.com/fr/downloads/) (phpadmin est conseillé mais ca marche sans )et l'inserer dans le path.

**OU**

 Installer wamp ou mamp

## Installation Wordpress & ajout d'une database

  1. Installer la dernière version de [Wordpress](https://fr.wordpress.org/telechargement/)

  2. Une fois l'installation terminer, décompresser le dossier zip et inserrer dans un nouveu dossier

  3. Avant d'ouvrir notre Wordpress en `local`, vous devez **créer une nouvelle base de donnée**.
    1. Ouvrir une ligne de commande (cmd, cmder etc.) et entrer la commande `mysql -u root -p` et entrez votre mdp
    2. Une fois dans mysql, créer votre bdd en entrant la commande `CREATE DATABASE <NomDeVotreBdd>;`
    (Vous pouvez voir votre nouvelle bdd entrant la commande `SHOW DATABASES;`)
    3. Quittez mysql en entrant la commande `exit;`

  4. Dans votre ligne de commande, dirigez-vous dans votre répertoire ou se trouve votre dossier contant le Wordpress
  et entrant la commande `php -S localhost:8000`. Une fois cela fait, ouvrez un navigateur et saisisser dans l'url l'adressage suivante `localhost:8000/votreDossierWp`, et voila le résultat

  ![intro Wordpress](https://image.noelshack.com/fichiers/2018/09/6/1520091348-sans-titre.jpg)

  5. Une fois avoir cliquer sur `C'est parti` vous devez **remplir les champs** avec votre bdd que vous avez créer précédemment ainsi que le *nom* et *mdp* (de base `root`), laissez le localhost tel quel et **changer le préfixe des tables** absolument. Une fois cela fait, valider et **lancez l'installation**.

  ![détail de connexion](https://image.noelshack.com/fichiers/2018/09/6/1520091918-sans-titre.jpg)
  ![lancer l'Installation](https://image.noelshack.com/fichiers/2018/09/6/1520092090-sans-titre.jpg)

  6. Remplissez les champs demander et installez Wordpress

  ![saisir champs](https://image.noelshack.com/fichiers/2018/09/6/1520092237-sans-titre.jpg)

  7. Et voila fini, enfin presque ...

  ![succes](https://image.noelshack.com/fichiers/2018/09/6/1520092363-sans-titre.jpg)

  8. Vous n'avez plus qu'à remplir les champs user et mdp que vous avez mis :)

  ![connexion](https://image.noelshack.com/fichiers/2018/09/6/1520092461-sans-titre.jpg)

## Intégration personnalisé et fonctions sur Wordpress

  Pour intégrer son propre site, nous avons besoin de créer un thème. Un Thème, c’est un dossier qui contient :
  - un Style.css : une feuille de style
  -	Un Screenshot.png une image (falcutativeà du thème qui apparait dans l’admin)
  -	Des fichiers php

### Créer son thème

Créer votre nouveau dossier thème au répertoire suivant `votre_dossier_wp>wp-content>themes`. Vous verrez 3 themes de base.

![mon theme](http://image.noelshack.com/fichiers/2018/09/6/1520098009-sans-titre.jpg)

### Intégrer son thème (la base)

#### index.php
  La page principale de votre page:

```php
<?php
/**
 * Permet d'ajouter le header.php
 * c'est comme un include
 */
get_header(); ?>

Le code c'est "le code" ? Ça va, ils se sont pas trop cassé le bonnet, pour l'trouver celui-là !

<?php
/**
 * Permet d'ajouter le footer.php
 * c'est comme un include
 */
get_footer(); ?>
```

#### header.php

```php
<!DOCTYPE html>
<html <?php language_attributes(); ?> class="no-js"> //language du site
<head>
    <meta charset="<?php bloginfo( 'charset' ); ?>">
    <meta name="viewport" content="width=device-width">
    <link rel="profile" href="http://gmpg.org/xfn/11">
    <?php
    /**
     * IMPORTANT SINON la mort
     * -> ajoute les styles et scripts par défaut de WP
     * -> ajoute les styles et scripts des plugins
     * -> ajoute vos styles et scripts
     */
    wp_head();
    ?>
</head>

<body <?php body_class(); ?>> //recupère les classes inclues dans le Wp -->
```
#### footer.php
```php
<?php
/**
 * IMPORTANT SINON la mort
 * -> ajoute les scripts par défaut de WP
 * -> mais ajoute aussi les scripts par défaut des plugins
 * -> mais ajoute aussi vos scripts
 */
wp_footer();  ?>
</body>
</html>
```
#### single.php (articles)

```php
//correpsond aux articles
<?php
/**
 * Permet d'ajouter le header.php
 * c'est comme un include
 */
    get_header();
/**
 * IMPORTANT
 * Permet d'initialiser les fonctions de WP pour un article
 * Sans the_post(), on ne pourrait pas utiliser les fonctions juste en dessous
 */
    the_post();
?>
<h2><?php the_title() // titre ^^ en même ça peut pas être autre chose :D ?></h2>

<?php echo get_the_date() // la date ?> - <?php the_author() // l'auteur ?>
<div class="cat">
    <?php
    /**
     * On récupère toutes les catégories liées à l'article qu'on consulte
     * par défaut WP a des catégories
     * pour WP les catégories sont nommées taxonomy
     * category est le nom de la taxonomy
     * Quand on crééra de nouvelle taxonomy, c'est juste 'category' qu'il faudra modifier et op tout le reste fonctionnera
     */
    $categories = get_the_terms( get_the_ID(), 'category' );
    $cats_post = array();
    foreach( $categories as $category ) {

         $cats_post[] = '<a href="'.get_term_link($category, 'category').'">'.$category->name.'</a>';
    }
    /**
     * Le implode permet de rassembler en string le tableau $cats_post
     * On prend chaque élément du tableau et on colle le tout ensemble avec le premier paramètre de la fonction
     */
        $cat_post = implode(' --/-- ', $cats_post);

    echo $cat_post;
    ?>
</div>


<div class="contenu-editable">
    <?php the_content() ?>
</div>

<?php
/**
 * Permet d'ajouter le footer.php
 * c'est comme un include
 */
get_footer(); ?>
```
(mot de pass en md5 crypé qu'au moment d'entrer le nv mdp)

#### le front-page.php (Page principale)

pour configurer& afficher la page, créer une page `accueil` ou autre dans wordpress. Aller dans `Réglages>Lecture` et régler `La page d'accueil affiche` et cocher `Une page statique` et séléctionner la `Page d'accueil:Accueil`

![image](https://image.noelshack.com/fichiers/2018/10/3/1520420698-lol.jpg)

```php
<?php
/**
 * Permet d'ajouter le header.php
 * c'est comme un include
 */
    get_header();


/**
 * IMPORTANT
 * Permet d'initialiser les fonctions de WP pour un article
 * Sans the_post(), on ne pourrait pas utiliser les fonctions juste en dessous
 */
    the_post();


    ?>

<h1><?php the_title() ?></h1>
<?php

$args = array(
    'post_type'      => 'post',
    'posts_per_page' => 2,
    'order'          => 'DESC',
);


/**
 * Requête permettant de récupérer les 2 derniers articles
 */
$query_articles = new WP_Query( $args );

if ( $query_articles->have_posts() ) :
    while ( $query_articles->have_posts() ) : $query_articles->the_post(); ?>

        <h2><?php the_title() ?></h2>

        <a href="<?php the_permalink() ?>">Lire la suite</a>

        <?php

    endwhile;

else : ?>

<p>Pas d'article</p>
<?php
endif;
/**
 * Important après chaque requête custom !
 * Cette fonction pemret de réinitialiser les éléménts courant de la page (par exemple the_title())
 * et de se baser à nouveau sur la requête par défaut
 */
wp_reset_postdata();
?>

<h1><?php the_title() ?></h1>


<?php
$args = array(
'post_type'  => 'page',
'pagename'   => 'qui-sommes-nous'
);

/**
 * Requête permettant de récupérer une page bien précise en fonction de son slug
 */
$query_page = new WP_Query( $args );
if ( $query_page->have_posts() ) :
$query_page->the_post(); ?>


<h2><?php the_title() ?></h2>
<a href="<?php the_permalink() ?>">Lire la suite</a>

<?php
endif;

wp_reset_postdata();
?>


<h1><?php the_title() ?></h1>


<?php
/**
 * Permet d'ajouter le footer.php
 * c'est comme un include
 */
get_footer(); ?>
```

#### functions.php(integrer style + script + fonctions){
```php
<?php
/**
  * GESTION DES MINIATURES DANS LES themes
  **/
add_theme_support('post-thumbnails');//autorise d'ajouter des images à la une dans notre theme

add_image_size('466x466',466,466,true); //permet d'ajuster l'image add_image_size('dimensionDelimage',width,height,crop(true/false))
// pour l'appeler : the_post_thumbnail('466x466',array('class' => 'media'))
/**
* INITIALISATION
* STYLES, Jquery, menu
*/
add_action('wp_enqueue_scripts', 'td_wp_enqueue_scripts');

/*add_action  permet de fixer sur un fonction wordpress
permet d'ajouter/supprimer nos element à nous dans wordpress (feuille de style et js)
dès qu'on appelle la fonction wordpress 'wp_enqueue_scripts',qui une fois appelée, wordpress appelera notre fonction
get_stylesheet_directory_uri() renvoie l'adresse du theme
--------------
wp_register_style($id, $le_chemin,array(), ver:false, media:'all')
/* configurer la feuile de style,
** array() correspond à un dépendance(l'élément va se charger avant) est-ce qu'un element doit tre chargé avant le mien
** ver:false, amettre en false sinon la version sera afficher sur l'ulr (en get)

wp_enqueue_style($id) permet d'ajouter dans wordpress
/* SCRIPT -----------------------
 wp_register_script($id, $path_file, array(), version(true ou false), footer(true ou false));
L'un ne va pas sans l'autre
*/
function td_wp_enqueue_scripts() {
    $path =get_stylesheet_directory_uri();

    $css =array(
        'reset' => $path . '/css/reset.css',
        'main' => $path . '/css/main.css'
    );
    //script css
    foreach ($css as $id => $path_file) {
        wp_register_style($id, $path_file, array(), false, 'all');
        wp_enqueue_style($id);
    }
  /**
  * AJOUT des JS
  * meme delire que css mais avec les fonctions script
  */
  $js=array(
    //'jquery' => 'https://code.jquery.com/jquery-3.3.1.min.js'
       'jquery' => $path . '/js/jquery.js',
       'main' => $path . '/js/main.js',
   );
   //supp la version de base de wp
   wp_deregister_script('jquery');

   foreach ($js as $id => $path_file) {
       wp_register_script($id, $path_file, array(), false, true);
       wp_enqueue_script($id);
   }
}
//cdm
//1er parametre, string a traduitre, la 2eme un nom domaine
//__ permet de concatener dans une variable
//_e est un echo
//_x comme le premier
function mon_menu(){
  register_nav_menu('header-menu',__('Header Menu','pikachu'));
  register_nav_menu('header-footer',__('Header Footer','footer'));
  register_nav_menu('header-mob',__('Header Mob','mob'));

  // register_nav_menu(theme_location,__('identifiant ','son nom'));
}
add_action('init','mon_menu');//on lnace la fonction lorsque la fonction wd est initialisé
// permet d'ajouter une section menu dans le cms (apparances/menus) avec nos 3

/**
 * Fonction qu'on a créé pour répondre aux besoins du thème
 * Ce n'est pas une fonction WordPress
 */
function getPrevNext_toto_hetic_AAAAAAAAAAAAAAAAA9A9A99AA99A9A9A9A9A9AA9(){
    $pagelist = get_pages('sort_column=menu_order&sort_order=asc'); // On récupère les pages par ordre croissant
    $pages = array();

    /**
     * On récupère les ids de nos pages
     */
    foreach ($pagelist as $page) {
        $pages[] += $page->ID;
    }

    /**
     * On récupère l'id de la page courante
     */
    $current = array_search(get_the_ID(), $pages);

    /**
     * On récupère si on peut les ids des pages suivantes et précédantes
     */
    $prevID = (isset($pages[$current-1])) ? $pages[$current-1] : '';
    $nextID = (isset($pages[$current+1])) ? $pages[$current+1] : '';


    /**
     * On affiche les boutons suivant et/ou précédant si les variables plus haut ne sont pas vide
     */
    if (!empty($prevID) || !empty($nextID)) {

        $html = '<div class="work_nav">';
            $html .= '<ul class="btn clearfix">';
                if (!empty($prevID)) {
                    $html .= '<li><a class="previous" data-title="Previous" href="'.get_permalink($prevID).'" title="'.get_the_title($prevID).'"></a></li>';
                }
                $html .= '<li><a href="'.get_home_url().'" class="grid" data-title="Home"></a></li>';
                if (!empty($nextID)) {
                    $html .= '<li><a class="next" data-title="Next" href="'.get_permalink($nextID).'" title="'.get_the_title($nextID).'"></a></li>';
                }
            $html .= '</ul>';
        $html .= '</div>';

            echo $html;
    }
}
```
--------------------------------------

## Récuperation de parametres custom + retourner ces parametres

### Saisir les parametres dans un tableau : $args [documentation](https://codex.wordpress.org/Template_Tags/get_posts#Return_Value)

```php
$args = array(
    'post_type'      => 'post',
    'posts_per_page' => 2,
    'order'          => 'DESC'
);
```
### Faire une requête

```php
$query_articles = new WP_Query( $args );
```

### S'assurer que les parametres existent et afficher vos articles

```php
<?php
if ( $query_articles->have_posts() ) :
    while ( $query_articles->have_posts() ) : $query_articles->the_post(); ?>

        <h2><?php the_title() ?></h2>

        <a href="<?php the_permalink() ?>">Lire la suite</a>

        <?php
    endwhile;
    ?>
```
### Important après chaque requête custom !

Cette fonction pemret de réinitialiser les éléménts courant de la page (par exemple the_title()) et de se baser à nouveau sur la requête par défaut
```php
wp_reset_postdata();
?>
```

## Récupèrer toutes les catégories liées à l'article qu'on consulte

```php
    <?php
    /**
     * On récupère toutes les catégories liées à l'article qu'on consulte
     * par défaut WP a des catégories
     * pour WP les catégories sont nommées taxonomy
     * category est le nom de la taxonomy
     * Quand on crééra de nouvelle taxonomy, c'est juste 'category' qu'il faudra modifier et op tout le reste fonctionnera
     */
    $categories = get_the_terms( get_the_ID(), 'category' );
    $cats_post = array();
    foreach( $categories as $category ) {

         $cats_post[] = '<a href="'.get_term_link($category, 'category').'">'.$category->name.'</a>';
    }
    /**
     * Le implode permet de rassembler en string le tableau $cats_post
     * On prend chaque élément du tableau et on colle le tout ensemble avec le premier paramètre de la fonction
     */
        $cat_post = implode(' --/-- ', $cats_post);

    echo $cat_post;
    ?>
```

## Intégrer son propre style & fonction js avec functions.php

```php
<?php

add_action('wp_enqueue_scripts', 'td_wp_enqueue_scripts');

/*add_action  permet de fixer sur un fonction wordpress
permet d'ajouter/supprimer nos element à nous dans wordpress (feuille de style et js)
dès qu'on appelle la fonction wordpress 'wp_enqueue_scripts',qui une fois appelée, wordpress appelera notre fonction
get_stylesheet_directory_uri() renvoie l'adresse du theme
--------------
wp_register_style($id, $le_chemin,array(), ver:false, media:'all')
/* configurer la feuile de style,
** array() correspond à un dépendance(l'élément va se charger avant) est-ce qu'un element doit tre chargé avant le mien
** ver:false, amettre en false sinon la version sera afficher sur l'ulr (en get)

wp_enqueue_style($id) permet d'ajouter dans wordpress
/* SCRIPT -----------------------
 wp_register_script($id, $path_file, array(), version(true ou false), footer(true ou false));
L'un ne va pas sans l'autre
*/
function td_wp_enqueue_scripts() {
    $path =get_stylesheet_directory_uri();

    $css =array(
        'reset' => $path . '/css/reset.css',
        'main' => $path . '/css/main.css'
    );
    //script css
    foreach ($css as $id => $path_file) {
        wp_register_style($id, $path_file, array(), false, 'all');
        wp_enqueue_style($id);
    }
  /**
  * AJOUT des JS
  * meme delire que css mais avec les fonctions script
  */
  $js=array(
    //'jquery' => 'https://code.jquery.com/jquery-3.3.1.min.js'
       'jquery' => $path . '/js/jquery.js',
       'main' => $path . '/js/main.js',
   );
   //supp la version de base de wp
   wp_deregister_script('jquery');

   foreach ($js as $id => $path_file) {
       wp_register_script($id, $path_file, array(), false, true);
       wp_enqueue_script($id);
   }
}
```

## MEMENTO (fonctions)

### <span style="border-left:5px solid #ff00ff;padding-left:5px">Général

<span style="color:#ff00ff">**get_header()**</span> *recupere le header.php*

<span style="color:#ff00ff">**get_footer()**</span> *recupere le footer.php*

<span style="color:#ff00ff">**the_post()**</span> Permet d'initialiser les fonctions de WP pour un article !!! important

### <span style="border-left:5px solid #3333ff;padding-left:5px">Header.php

<span style="color:#3333ff">language_attributes()</span> *permet de gérer la langue*

<span style="color:#3333ff">body_class()</span> *recupère les classes inclues dans le Wp*

<span style="color:#3333ff">wp_head()</span> *affiche toute la balise head inclue dans wp*

### <span style="border-left:5px solid #00ff00;padding-left:5px"> index.php & front-page.php




### <span style="border-left:5px solid #ff3333;padding-left:5px">footer.php

<span style="color:#ff3333">**wp_footer()**</span> *affiche le footer*

### <span style="border-left:5px solid #008080;padding-left:5px">single.php

<span style="color:#008080">**the_title()**</span> *affiche le titre de l article*

<span style="color:#008080">**echo get_the_date()**</span> *affiche la date de l'article*

### <span style="border-left:5px solid #ffff00;padding-left:5px">functions.php

<span style="color:#ffff00">**add_theme_support('post-thumbnails');**</span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*gère les miniatures dans les themes, ici on autorise d'ajouter des images à la une dans notre theme*

<span style="color:#ffff00">**add_image_size('nomDeLaDimension',466,466,true);**</span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*permet d'ajuster l'image : add_image_size('dimensionDelimage',width,height,crop(true/false)*
<!
# Comment intégrer à partir d'un template différent

## Etape 1 changer la redirection des styles dans la `fonction.php` en fonctions de l'index.html indiqué en haut

On a en tout 4 éléments à ajouter : 2 element style et 2 element js;

![index.html](https://image.noelshack.com/fichiers/2018/11/3/1521027729-sans-titre.jpg)

Dans notre `fonctions.php` parti css:

```php
$css =array(
    'reset' => $path . '/css/reset.css',
    'main' => $path . '/css/main.css'
);
```

Dans notre `fonctions.php` parti js:
```php
$js=array(
  //'jquery' => 'https://code.jquery.com/jquery-3.3.1.min.js'
     'jquery' => $path . '/js/jquery.js',
     'main' => $path . '/js/main.js',
 );
```
## Etape 2 changer le nom de theme dans le `style.css

![style.css](https://image.noelshack.com/fichiers/2018/11/3/1521028138-sans-titre.jpg)

## Etape 3 copier l'index.html dans le front-page.php

Selon le theme qu'on a importé, on devra ajouter des éléments dans le `header.php` ou `footer`.

## Etape 4 Faire une boucle dans le `front-page.php` en php si des éléments se répètent et ajouter les fonctions post.

Element minimum :
```php
<?php
$args = array(
'post_type'      => 'post',
'posts_per_page' => 9,
'order'          => 'DESC'
);
$query_articles = new WP_Query( $args );
while ( $query_articles->have_posts() ) : $query_articles->the_post(); ?>

<!-- MA DIV -->

<?php
endwhile;
wp_reset_query();
?>
```
## Etape 5 ajouter des images à la une

dans votre `functions.php` ajoutez :
```php
/**
  * GESTION DES MINIATURES DANS LES themes
  **/
add_theme_support('post-thumbnails');//autorise d'ajouter des images à la une dans notre theme

add_image_size('466x466',466,466,true); //permet d'ajuster l'image add_image_size('dimensionDelimage',width,height,crop(true/false))
/**
* INITIALISATION
* STYLES, Jquery, menu
*/
```
pour l'appeler dans votre `front-page.php`:

```php
<?php the_post_thumbnail('466x466',array('class' => 'media')) ?>
```
ou encore :

```php
<img src="<?php echo get_the_post_thumbnail_url(get_the_ID(), '588x272') ?>">
<!--il faut ensuite regener l'image, cf :Regenerate Thumbnails-->
```

# Plugin ACF

```php
<?php the_field('mon_champs') ?> //afficher son acf
```
## Rendre les elements plus visibles :

Crée des type de champs "onglet" (pratique quand on met plusieurs champs text, cela permet de mieux structurer notre travail pour le client)

champs de class acf :

1. créer son identifiant
2. type de champs :selection
3. dans dans choix: mettre à gauche la class et à droite l'identifiant
4. dans le php dans class mettre son field ici ^^

autre methode, faire un tableau :
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
