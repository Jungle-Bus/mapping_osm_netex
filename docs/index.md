# Aide à la conversion des attributs OpenStreetMap vers NeTEx

!!! warning "Attention"

    Cette documentation est un brouillon temporaire basée sur la version à paraitre du profil NeTEx. En attendant sa finalisation, voici [la dernière version publiée](https://github.com/Jungle-Bus/mapping_osm_netex/releases/download/v1.1/JungleBus_AccesLibreMobilites_ReglesConversion_OSM-NeTEx_1.1.pdf).

OpenStreetMap (OSM) est un projet collaboratif de cartographie en ligne qui vise à constituer une base de données géographiques libre du monde. OpenStreetMap est souvent présenté comme le Wikipédia des cartes. Les données de transport, de cheminement piéton et d'accessibilité y sont représentées sur certains territoires.

Ce document propose une standardisation de la conversion des éléments et attributs d'OpenStreetMap vers NeTEx. Il s'appuie sur le profil français et plus particulièrement :

- le [Profil NeTEx accessibilité France](https://normes.transport.data.gouv.fr/normes/netex/accessibilite/)
- le [Profil France - Description des arrêts](https://normes.transport.data.gouv.fr/normes/netex/arrets/)

Le modèle attributaire d'OpenStreetMap est communautaire : il se construit au gré des contributions et des éléments cartographiques que les contributeurs souhaitent représenter sur la carte. Il s'agit donc d'un modèle vivant qui est en constante évolution. À titre d'exemple, les conventions pour représenter un validateur de titre de transports ont [évolué en 2022](https://wiki.openstreetmap.org/wiki/Proposal:Ticket_validator) afin de faire converger les différents attributs jusque-là utilisés. Le présent document est basé sur les pratiques de contributions en usage à la date de rédaction du document dans la communauté mondiale et plus particulièrement française.

!!! info "Remarque"
    Il s'agit d'un guide de conversion des attributs d'OpenStreetMap vers NeTEx, et non d'une spécification pour réaliser un convertisseur d'OpenStreetMap vers NeTEx. En effet, outre les conversions des tags, il conviendra de tenir compte des modélisations qui peuvent différer entre OpenStreetMap et NeTEx, et donc des transformations supplémentaires qui pourraient être nécessaires : par exemple OpenStreetMap dispose d'un modèle de représentation des données d'intérieur qui ne nécessite pas forcément de créer le graphe piéton associé ; un convertisseur OSM vers NeTEx pourrait choisir de générer automatiquement les cheminements associés à partir des espaces intérieurs, mais cette transformation n'est pas couverte par le présent document.

Par ailleurs, les données OpenStreetMap sont mises à disposition gratuitement par la fondation OpenStreetMap sous la licence libre ODbL. Des conditions s'appliquent lors de l'utilisation et de la conversion de ces données vers NeTEx, ainsi que sur les fichiers ainsi produits.

Ce guide de conversion n'a pas vocation à être exhaustif dans sa couverture de NeTEx ou d'OpenStreetMap, mais s'efforce de couvrir les objets les plus importants concernant l'accessibilité et les cheminements à pied.

## Liens utiles

* [Profil France de NeTEx](https://normes.transport.data.gouv.fr/normes/netex/)
* [Documentation concernant la cartographie des cheminements piétons et de l'accessibilité dans OpenStreetMap](https://wiki.openstreetmap.org/wiki/FR:Cheminements_pi%C3%A9tons)
