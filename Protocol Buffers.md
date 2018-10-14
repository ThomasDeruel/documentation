# Protocol Buffers

Google a crée un format de sérialisation le `Protocol Buffers`, une alternative du XML.

Le Protocol Buffers est un format de sérialisation (cf data), comme JSON et XML, avec un IDL (**une Interface description language**) développé par Google.
L'implémentation d'origine publiée par Google pour **C++**, **Java** et **Python** est disponible sous une licence libre.

Les Protobuffers constituent un mécanisme flexible, efficace et automatisé pour la sérialisation des données structurées - pensé XML, mais plus petit, plus rapide, et plus simple.
Nous pouvons définir comment notre donnée sera structurée une fois, puis nous pouvons utiliser du code source généré pour faciliter l'écriture et lire nos données structurées
et à partir d'une variété de flux de données et en utilisant une variété de langues. Nous pouvons mettre à jour notre structure de données sans casser les programmes déployés qui sont compilés dans un "vieux" format.

## comment ca marche ?

Sert à definir un schéma.

Permet de créer des `.proto` et à partir de se proto on peut générer notre fichier et on peut les utiliser comme des objets tels qu'ils étaient au départ
Elle gère la classe de chaque langage
On peut le séréaliser, en texte, en binaire (Wire), pb JS,pb ULR et les parser

## Avantages

Comparé au XML, le protoBuffer a plusieurs avantages :
* Taille de message et temps de sérialisation/désérialisation optimisés
* Extensible
* léger (contrairement au XML)
* les données sont typées (contrairement à json qui lui est dynamique, donc verifié "à la volé")
* prend en charge de nombreux le langages & Interpolarité entre langages facile

## Comment intégrer en JAVA côté client

### Définir un format de protocol dans un fichier **.proto**

####  1.  Définir la syntax

 Dans un premier temps, on définit la syntax qu'on va utiliser avec la propriété `syntax`, prenant soit la valeur `proto2` ou `proto3`

```js
syntax = "proto3";
//ou syntax = "proto2";
```
---
####  2.  On déclare un package.

```js
package tutorial // /!\ important
option java_package = "com.example.tutorial";
option java_outer_classname = "AddressBookProtos";
```

Ce package nous aidera à empecher les conflits d'appellation entre les différents projets.
En Java, le nom du package est utilisé comme le package Java, **SAUF** si vous avez explicitement spécifié un package `java_package`

___On devra toujours définir un package de base pour éviter les collisions de noms dans le namespace (espace de nom) du protoBuffer ainsi que dans les langages non Java.___

#### 3.  Déclaration des options)

Après avoir déclarer notre package. Nous pouvons ajouter des **options** comme par exemple :
  * **java_package**

    *`java_package` spécifie dans quel nom de package Java nos classes devront vivre*
    > Si ceci n'est pas spécifié explicitement, il va simplement matcher le nom de package donné par la déclaration `package`, mais ces noms ne sont généralement pas des noms de package Java appropriés

  * **java_outer_classname**

    *l'option `java_out_classname` défini le nom de classe que devra contenir toutes les classes de ce fichier*
    > Si cette option n'est pas spécifiée explicitement, cela sera généré par la conversion du nom de dossier en CamelCase
    exmple : "my_proto.proto" devra par défaut utiliser "MyProto" comme nom de classe externe

#### 4. Définitions de mon message

Un message est juste un agrégat (cumul de données) contenant un ensemble de champs typés.
De nombreux types de standard sont disponibles comme champs types :
* bool
* int32
* float
* double
* string
* enum (ensemble de valeurs constantes)

Nous pouvons également ajouter d'autres structures supplémentaires à nos messages en utilisante d'autres types de messages comme type de champs.

**Nous pouvons égalements définir des types de message imbriqués dans d'autres messages !**
```js
message Person {
  required string name = 1;
  required int32 id = 2;
  optional string email = 3;

  enum PhoneType {
    MOBILE = 0;
    HOME = 1;
    WORK = 2;
  }

  message PhoneNumber {
    required string number = 1;
    optional PhoneType type = 2 [default = HOME];
  }

  repeated PhoneNumber phones = 4;
}

message AddressBook {
  repeated Person people = 1;
}
```
Les marqueurs `= 1`, `= 2` sur chaque élément identifie l'unique "*tag*" que le champs va utiliser
dans l'encodage binaire.
Les nombres de tag de 1 à 15 requièrent un octet de moins pour être codé par rapport aux nombres supérieurs.
>Ainsi pour des questions d'optimisation on laissera ces valeurs à des options peu utilisées.

Chaque élément dans un champs **répété** nécessite de ré-encoder le numero du tag.
Les champs répétés sont particulièrement utilises pour cette optimisation
#### 5. Les modificateurs

Chaque champs doit être annoté avec l'un des modificateurs suivants:

* **required**

  Une valeur du champs doit être fourni, sinon le message sera considé comme *non initialisé*.
    >Essayer de construire un message non initialisé renverra une erreur `RuntimeException`.
    Parser un message non initilisé renverra comme erreur `IOException`

    A part cela, un champs `required` se comporte exactement comme un champs `optional`.
* **optional**

  Le champs peut ou ne peut-être défini. Si la valeur un champs optionnel n'est pas définie, une valeur par
  défaut est utilisée. Pour les types simples, **nous pouvons spécifier notre propre valeur par défaut**, comme
  nous avons fait pour le `type` de numéro de téléphone dans l'exemple. Autrement, un système par défaut est utilisé:
    * pour les **types numériques** : 0
    * pour les **types string** : un champs vide
    * pour les **types boolean** : `false`
  Pour les messages incorporés, la valeur par défaut est TOUJOURS *l'instance par défaut* ou *le prototype* du
  message, qui n'a aucun de ses champs définis
    >L'appel de l'accesseur pour obtenir la valeur d'un champ facultatif (ou obligatoire)
    qui n'a pas été explicitement défini renvoie toujours la valeur par défaut de ce champ.

* **repeated**

  Le champs peut-être répété X fois (inclus 0). Il faut imaginer ces champs repeated comme **des tableaux de taille dynamique**

---

### Compiler notre protocol Buffer

#### En java

Une fois que nous avons notre `.proto` la prochaine étape à faire est de générer Les classes qu'on aura besoin pour **lire**
et **écrire** les messages de notre proto. Pour cela, nous aurons besoin de lancer le compilateur du Protocol Buffer `protoc` sur
notre `.proto`

  1.  Dans un premier temps nous devons installer le compilateur, suivre la doc [ici](https://developers.google.com/protocol-buffers/docs/downloads)

  2.  Nous devons ensuite lancer le compilateur, nous devons:
    1. spécifier le répertoire source
    2. le répertoire de destination (où le code généré doit aller, souvent le même que $SRC_DIR)
    3. et le chemon de notre `.proto`

    ```js
    // pour du java:
    protoc -I=$SRC_DIR --java_out=$DST_DIR $SRC_DIR/monproto.proto
    ```

    >**A savoir**: $SRC_DIR représente le contenu du répertoire courant.
    
#### En javascript

Le compilation du Protocol Buffer s'effectue en js en invoquant la ligne de
commande `--js_out=`.
L'option du paramètre `--js_out=` représente le répertoire de destination (comme en java).
Pour en savoir: cf le site officiel...

---
Le répertoire de sortie dépend si nous voulons utiliser le style d'import de fermeture' `Closure-style` ou le style d'import commun au js
`CommonJS-style`. Le compilateur supporte les 2.

  *  **Le Closure imports**

   Par défaut, le compilateur génère le code avec ce style d'importation. Si nous spécifions une option `library` quand on lance le compilateur,
    celui ci crée **un seul fichier** `.js` avec notre nom de la librairie spécifiée.
    Autrement dit, le compilateur génère un fichier `.js` **pour chaque message** dans notre fichier `.proto`.
    
   Les **noms des fichier de sortie** sont calculés en prenant la **valeur** de la `library` OU du nom du message (lowercase),
    avec les modifications suivantes :   
   *	l'extention `.js` est ajouté
   * le chemin du proto (spécifié par la ligne de commande `--proto_path=` ou `-I`) est remplacé par le chemin de sortie (spécifié avec le flag `--js_out`)

   ```js
protoc --proto_path=src --js_out=library=whizz/ponycopter,binary:build/gen src/foo.proto src/bar/baz.proto
```
Dans cet exemple, le compilateur lira les fichier `src/foo.proto` et `src/bar/baz.proto` et produit un seul fichier de sortie :`build/gen/whizz/ponycopter.js`.
    Le compilateur creera automatiquement le répertoire `build/gen/whizz` si cela est necessaire.

   >/!\ Cela ne creera pas le `build` ou `buid/gen`; ils doivent déjà exister.

   Le(s) fichier(s) généré(s) `goog.prodide()` tous les types définis dans notre/nos fichier(s) `.proto` et `google.require()`tous les types dans le noyau de la libraire du Protocol Buffer
   et la librairie de Google Closure

   >/!\ Bien vérifier que la configuration `goog.provide()`/`goog.require()` peuvent trouver tout le code générer : les fichiers `.js` de la librairie principale & la libairire Google Closure elle-même

**Le CommonJs imports**

Une autre manière d'importer: cf la doc officielle*

#### Packages et Closure Imports

Si nous avons utiliser l'importation de style `Closure` et un fichier `.proto` contenant une déclaration d'un package, le code généré utilise le package proto comme partie du namespace javascript pour nos types de messages.

Ex: un proto ayant un nom de package `example.high_score` génère un namespace js de `proto.example.high_score`

```js
goog.provide('proto.example.high_score.Ponycopter');
```

Autrement, si le fichier `.proto` ne contient pas de declaration d'un package, le code généré utilise juste le `proto` comme namespace pour nos types de messages, qui est la **racine** du namespace
du Protocol Buffer
