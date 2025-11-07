# Conventions de représentation

[node]: img/node.png
[way]: img/way.png
[relation]: img/relation.png
[area]: img/area.png

NeTEx s'appuie sur le format XML, qui repose sur une syntaxe avec des balises entourées de chevrons.

Dans l'exemple suivant représentant un *TravelatorEquipment* :

```xml
<TravelatorEquipment id="ALM:TravelatorEquipment:w45561_n2_n502:LOC" version="152">
        <PublicCode>FC2</PublicCode>
        <Width>0.2</Width>
        <DirectionOfUse>both</DirectionOfUse>
        <TactileActuators>true</TactileActuators>
</TravelatorEquipment>
```

`TravelatorEquipment/@id` représente l'attribut xml `id` de l'élément TravelatorEquipment.

`TravelatorEquipment/PublicCode` représente l'élément `PublicCode`, contenu dans l'élément TravelatorEquipment.

Le modèle de données OpenStreetMap (OSM) repose sur des éléments géométriques simples appelés ![node] *nœud*, ![way] *chemin* et ![relation] *relation*.

Les conventions suivantes sont utilisées pour décrire les éléments géométriques dans cette documentation :

- ![node] *nœud* fait référence à un objet
    ponctuel

- ![way] *chemin* correspond à une succession
    de
    ![node] nœuds. Dans OSM, cela peut
    constituer soit un objet linéaire soit une zone dans le cas d'un
    chemin fermé formant un polygone

- ![area] *zone* fait référence à un objet
    polygonal. Il peut s'agir à la fois d'un
    ![way] chemin fermé ou d'une
    ![relation] relation, c'est-à-dire
    l'association de plusieurs
    ![way] chemins constituant un polygone ou
    un multi-polygone

De plus, OpenStreetMap utilise un modèle basé sur des attributs
clef-valeur appelés tags. Le [wiki
d'OpenStreetMap](https://wiki.openstreetmap.org/wiki/Main_Page) est la
référence documentaire pour leur définition et description.

Les conventions suivantes sont utilisées pour décrire les tags dans le
présent document :

- highway=\* signifie que la clef *highway* est présente, avec
    n'importe quelle valeur

- conveying!=no signifie que la clef *conveying* est présente, et que
    la valeur associée n'est pas no. Par exemple : conveying=yes

- conveying=yes/no signifie que la clef *conveying* a soit la valeur
    yes, soit la valeur no

- conveying!=yes/no signifie que la clef *conveying* est présente, et
    que sa valeur n'est ni yes, ni no. Par exemple conveying=reversible

- cuisine~pizza signifie que la clef *cuisine* est présente, et
    que sa valeur contient *pizza*. Par exemple *cuisine=pizza;italian*

Lorsqu'il est indiqué qu'un élément NeTEx est construit à partir de la
valeur d'un tag OSM, il est sous-entendu que si le tag OSM n'est pas
présent, l'élément NeTEx associé n'est pas présent non plus.