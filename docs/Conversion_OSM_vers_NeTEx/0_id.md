# Gestion des identifiants

[node]: ../img/node.png
[area]: ../img/area.png

Pour faciliter l'interopérabilité, le débug et les éventuelles mises à jour de données, il est conseillé de garder la trace des identifiants des objets OpenStreetMap utilisés pour construire les éléments NeTEx.

Cela peut se faire directement avec le mécanisme d'identifiant et de version de NeTEx si les données OpenStreetMap sont exportées sans modification, par exemple :

- `<Quay version="4" id="[Fournisseur]:Quay:node6289317157:">` pour le ![node] nœud OSM [https://www.openstreetmap.org/node/6289317157](https://www.openstreetmap.org/node/6289317157) dans sa version 4

- `<Quay version="8" id="[Fournisseur]:Quay:way139759427">` pour le ![area] chemin OSM [https://www.openstreetmap.org/way/139759427](https://www.openstreetmap.org/way/139759427) dans sa version 8

À défaut, le mécanisme d'extension de NeTEx (attribut KeyList de l'élément abstrait DataManagedObject) peut être utilisé pour conserver l'identifiant, le type d'objet et éventuellement la version utilisée à l'import. Par exemple :

```xml
<keyList>
    <KeyValue typeOfKey="OpenStreetMap_ref">
        <Key>id</Key>
        <Value>1104609983</Value>
    </KeyValue>
    <KeyValue typeOfKey="OpenStreetMap_ref">
        <Key>type</Key>
        <Value>way</Value>
    </KeyValue>
    <KeyValue typeOfKey="OpenStreetMap_ref">
        <Key>version</Key>
        <Value>1</Value>
    </KeyValue>
</keyList>
```

