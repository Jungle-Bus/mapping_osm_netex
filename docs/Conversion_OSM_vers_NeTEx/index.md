# Conversion OpenStreetMap vers NeTEx

Cette partie de la documentation illustre le cas d'usage suivant : on dispose d'un export de données issu d'OpenStreetMap et on souhaite le convertir vers le format NeTEx.

Un export NeTEx conforme au profil français se présente sous la forme d'une archive zip contenant plusieurs fichiers xml :

- le fichier [accessibility.xml](accessibility.md) regroupe les équipements et les informations de cheminement.
- le fichier [parking.xml](parking.md) regroupe les informations sur les parkings. On y trouvera donc les informations sur les places de stationnement réservées aux personnes à mobilité réduite
- le fichier [stop.xml](stop.md) regroupe les informations sur les arrêts, les stations, et les entrées sorties de gares
- le fichier [resource.xml](resource.md) permet notamment d'indiquer la source de la donnée (ici OpenStreetMap)
- d'autres fichiers xml non concernés par notre cas d'usage peuvent être présents, pour exporter les lignes de transport, les informations relatives aux commerces et services publics, les horaires des transports, les tarifs, etc

Au sein de chaque fichier xml, les différents objets sont représentés en suivant le modèle de données européen [Transmodel](https://transmodel-cen.eu/), qui définit les concepts ainsi que les attributs utilisables pour les échanges de données. Les définitions des concepts sont reprises en français en préambule de chaque partie du profil NeTEx.

!!! info "Remarque"

    Il existe plusieurs manières de représenter une même réalité avec NeTEx. Ce documentation propose une approche, conforme à la norme NeTEx, au profil français et s'efforçant de respecter les usages recommandés dans le profil européen (EPIP). D'autres approches et d'autres modélisations restent possibles et valables.

    De même, il existe une grande quantité d'attributs NeTEx ; cette documentation couvrent exclusivement ceux relatifs à aux cheminements piétons et à l'accessibilité à pied.

La suite de cette documentation présente chaque fichier de l'export NeTEx et les correspondances entre les attributs OpenStreetMap et ceux de NeTEx pour chacun des objets.