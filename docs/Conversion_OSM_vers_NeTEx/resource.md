# Fichier resource.xml

Cette partie de la documentation explique comment générer le fichier `resource.xml` de l'export NeTEx.

## DataSource

L'élément NeTEx DataSource permet de référencer une source de données.

Voici les valeurs recommandées pour identifier OpenStreetMap et remplir les conditions d'attributions imposées par sa licence :

```xml
<DataSource id="DataSource:OpenStreetMap" version="any">
    <Name>OpenStreetMap</Name>
    <Description>OpenStreetMap est un ensemble de données ouvertes, disponible sous la licence
libre ODbL accordée par la Fondation OpenStreetMap.</Description>
    <DataLicenceCode ref="ODbL"/>
    <DataLicenceUrl>https://wiki.osmfoundation.org/wiki/Licence</DataLicenceUrl>
</DataSource>
```

En complément, il est possible d'utiliser l'attribut *dataSourceRef*
de l'élément abstrait *DataManagedObject* sur tous les objets créés à
partir d'OpenStreetMap et d'y référencer ce DataSource.