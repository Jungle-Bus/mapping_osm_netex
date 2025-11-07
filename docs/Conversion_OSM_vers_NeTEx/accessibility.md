# Fichier accessibility.xml

[node]: ../img/node.png
[way]: ../img/way.png
[area]: ../img/area.png

Cette partie de la documentation explique comment générer le fichier `accessibility.xml` de l'export NeTEx, qui contient les équipements et les informations de cheminement.

## SitePathLink

### Sélection

Les ![way] chemins OSM avec le tag [highway](https://wiki.openstreetmap.org/wiki/FR:Cheminements_piétons) sont susceptibles d'être convertis en SitePathLink. Les tags utilisés pour la sélection déterminent également l'élément NeTEx SitePathLink/AccessFeatureType.

Les ![way] chemins avec [conveying!=no](https://wiki.openstreetmap.org/wiki/FR:Key:conveying) et [highway=steps](https://wiki.openstreetmap.org/wiki/FR:Tag:highway%3Dsteps) sont convertis en SitePathLink avec un AccessFeatureType valant escalator.

Les ![way] chemins [conveying!=no](https://wiki.openstreetmap.org/wiki/FR:Key:conveying) et [highway=footway](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Ahighway%3Dfootway) sont convertis en SitePathLink avec un AccessFeatureType valant travelator.

Les ![way] chemins avec [highway=footway](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Ahighway%3Dfootway) et [incline](https://wiki.openstreetmap.org/wiki/FR:Key:incline) sont convertis en SitePathLink avec un AccessFeatureType valant ramp.

Les ![way] chemins suivants sont convertis en SitePathLink avec un AccessFeatureType valant hall :

- highway=footway et [indoor=yes](https://wiki.openstreetmap.org/wiki/FR:Key:indoor)
- [highway=corridor](https://wiki.openstreetmap.org/wiki/Tag%3Ahighway%3Dcorridor)

Les ![way] chemins avec area=yes et [highway=pedestrian](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Ahighway%3Dpedestrian) (places piétonnes) sont convertis en SitePathLink avec un AccessFeatureType valant openSpace.

Les ![way] chemins suivants sont convertis en SitePathLink avec un AccessFeatureType valant street :

- [highway=living_street](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Ahighway%3Dliving_street)
- [highway=pedestrian](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Ahighway%3Dpedestrian)

Les ![way] chemins suivants sont convertis en SitePathLink avec un AccessFeatureType valant pavement :

- highway=footway et [footway=sidewalk](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Afootway%3Dsidewalk)
- [highway=cycleway](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Ahighway%3Dcycleway), foot=yes/designated et [segregated=yes](https://wiki.openstreetmap.org/wiki/FR%3AKey%3Asegregated)

Les ![way] chemins suivants sont convertis en SitePathLink avec un AccessFeatureType valant crossing :

- highway=footway et [footway=crossing](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Afootway%3Dcrossing)
- [highway=path](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Ahighway%3Dpath) et path=crossing
- highway=footway/path et crossing=\*

Les ![way] chemins suivants sont convertis en SitePathLink avec un AccessFeatureType valant footpath :

- highway=path
- highway=footway
- highway=cycleway, foot=yes/designated et [segregated=no](https://wiki.openstreetmap.org/wiki/FR%3AKey%3Asegregated)

Les ![way] chemins avec [highway=steps](https://wiki.openstreetmap.org/wiki/FR:Tag:highway%3Dsteps) sont convertis en SitePathLink avec un AccessFeatureType valant stairs.

Les ![way] chemins et les ![node] nœuds [highway=elevator](https://wiki.openstreetmap.org/wiki/FR:Tag:highway%3Delevator) sont utilisés pour construire les SitePathLink avec un AccessFeatureType valant lift. Un SitePathLink est créé pour chaque couple d'étages consécutifs. Le tag level indiquant les étages desservis séparés par des ";" est utilisé à cet effet.

**Remarques additionnelles** :

Les places piétonnes (openSpace) sont des ![area] polygones dans OpenStreetMap alors que l'objet NeTEx SitePathLink est une ligne. Il peut être utile de construire des cheminements traversants la place piétonne à la place afin d'obtenir un meilleur graphe. De même, il est possible dans OpenStreetMap de modéliser les ascenseurs avec un ![area] polygone représentant la cage d'ascenseur. Dans ce cas, il sera pertinent de reconstruire des objets SitePathLink pour chaque couple d'étages au lieu d'importer le contour.

Il existe dans OpenStreetMap plusieurs manières (appelés schémas) pour cartographier les trottoirs :

- l'utilisation de highway=footway, dont l'utilisation est déjà
    décrite plus haut
- l'ajout d'attributs liés aux trottoirs directement sur la voirie,
    sur le
    ![way] chemin OSM avec highway=\* servant à
    la circulation des véhicules

Ces deux schémas peuvent être utilisés séparément ou en association à l'échelle d'un territoire. La seconde méthode est néanmoins majoritaire dans OpenStreetMap aujourd'hui (en 2025).

Pour importer les cheminements lorsque le second schéma est utilisé, on créera un SitePathLink avec un AccessFeatureType valant pavement pour les cas suivants :

- highway=\* et foot!=no
- highway=\* et sidewalk:right!=no/separate
- highway=\* et sidewalk:left!=no/separate
- highway=\* et sidewalk!=no/none/separate

Cependant, dans ce cas la géométrie est alors celle de la route et non celles de trottoirs. Des traitements géographiques assez complexes sont donc à envisager pour générer un réel graphe piéton dans ces cas, en tenant compte des intersections avec le reste des cheminements. De plus, les informations liés aux trottoirs sont alors à chercher à  la place dans des attributs préfixés (par exemple [sidewalk:both:surface](https://wiki.openstreetmap.org/wiki/Key:sidewalk:both:surface) au lieu de surface).

La manière de cartographier les passages piétons dans OpenStreetMap dépend du schéma utilisé pour cartographier les trottoirs de part et d'autre de celui-ci. On trouvera toujours le passage piéton sous forme de ![node] nœud avec highway=crossing sur la voirie pour la circulation des véhicules. Lorsque les trottoirs sont cartographiés en chemins séparés avec highway=footway, on trouvera également un ![way] chemin avec highway=footway et footway=crossing.

De la même manière, lorsque le passage piéton est représenté uniquement avec un ![node] nœud, des transformations géométriques sont à envisager afin de proposer une meilleure géométrie dans l'export NeTEx.

De plus, les informations liées à ce passage piéton peuvent être renseignés soit sous la forme de tag sur le ![node] nœud highway=crossing soit sous la forme de tag sur le ![way] chemin avec highway=footway, et il peut être pertinent de considérer les deux objets pour construire les attributs NeTEx.

Pour en savoir plus sur les différents schémas pour les trottoirs et les passages piétons, consulter la page du wiki [Cheminements piétons](https://wiki.openstreetmap.org/wiki/FR:Cheminements_piétons).

### Conversion des attributs

#### SitePathLink/Name

SitePathLink/Name est rempli avec la valeur du tag [name](https://wiki.openstreetmap.org/wiki/FR:Key:name).

#### SitePathLink/Distance

SitePathLink/Distance est rempli avec la distance calculée du SitePathLink, en mètres arrondis au cm.

#### SitePathLink/LineString

SitePathLink/LineString est rempli avec la géométrie du ![way] chemin, sous la forme d'une ligne.

#### SitePathLink/From

SitePathLink/From est une référence vers l'objet NeTEx correspondant au point de départ du ![way] chemin.

#### SitePathLink/To

SitePathLink/From est une référence vers l'objet NeTEx correspondant au point de destination du ![way] chemin.

#### SitePathLink/Description

SitePathLink/Description peut être rempli avec la valeur du tag [description](https://wiki.openstreetmap.org/wiki/FR:Key:description).

#### SitePathLink/AccessibilityAssessment

**ValidityCondition/Description** est construit en contaténant le contenu des tags suivants, avec un séparateur " - " :

- [wheelchair:description=\*](https://wiki.openstreetmap.org/wiki/FR:Key:wheelchair:description)
- [blind:description](https://wiki.openstreetmap.org/wiki/Disabilitydescription)
- [deaf:description](https://wiki.openstreetmap.org/wiki/Disabilitydescription)

**AccessibilityLimitation/WheelchairAccess** est renseigné avec les règles de gestion suivantes, dans l'ordre :

- false si highway=steps
- false si conveying!=no
- false si highway=elevator et maxlength:physical < 1,25 m
- false si highway=elevator et length < 1,25 m
- false si highway=elevator et est_length < 1,25 m
- false si highway=elevator et door:width < 0,8 m
- false si highway=elevator et automatic_door=no
- true si highway=elevator et automatic_door!=no et door:width ≥ 0,8 m
    et maxlength:physical (ou length ou est_length) ≥ 1,25 m
- false si incline:across > 5%
- false si incline > 8%
- false si width (ou est_width) < 0,9 m
- false si surface = gravel / fine_gravel / pebblestone / chipseal /
    mud / sand / rock / stone / bare_rock / rocks
- false si smoothness = bad / very_bad / horrible / very_horrible /
    impassable
- false si le
    ![way] chemin comprend un
    ![node] nœud avec barrier=\* ou
    obstacle:wheelchair=yes
- false si le
    ![way] chemin comprend un
    ![node] nœud avec kerb=raised
- false si le
    ![way] chemin comprend un
    ![node] nœud avec kerb:height > 0,04 m
- true si incline:across ≤ 2% et incline ≤ 5% et width (ou est_width)
    ≥ 1,4 m et
    surface!=gravel/fine_gravel/pebblestone/chipseal/mud/sand/rock/stone/bare_rock/rocks
    et smoothness=excellent/good/intermediate et le
    ![way] chemin ne comprend aucun
    ![node] nœud avec barrier=\* ou
    obstacle:wheelchair=yes ou kerb=raised ou kerb:height > 0,02 m
- partial si incline:across ≤ 5% et incline ≤ 8% et width (ou
    est_width) ≥ 0,9 m et
    surface!=gravel/fine_gravel/pebblestone/chipseal/mud/sand/rock/stone/bare_rock/rocks
    et smoothness=excellent/good/intermediate et le
    ![way] chemin ne comprend aucun
    ![node] nœud avec barrier=\* ou
    obstacle:wheelchair=yes ou kerb=raised ou kerb:height > 0,04 m
- true si wheelchair=yes
- false si wheelchair=no
- partial si wheelchair=limited/bad
- other si wheelchair=\* a une autre valeur
- non renseigné sinon

**AccessibilityLimitation/StepFreeAccess** est construit avec les règles
de gestion suivantes dans l'ordre :

- false si highway=steps

- true si conveying!=no

- true si highway=elevator

- false si un
    ![node] nœud constituant le
    ![way] chemin a
    [kerb=raised](https://wiki.openstreetmap.org/wiki/FR:Key:kerb)

- false si un
    ![node] nœud constituant le
    ![way] chemin a kerb:height > 0,04 m

- true si le
    ![way] chemin n'a aucun
    ![node] nœud avec kerb:height > 0,02 m

- true si le
    ![way] chemin n'a aucun
    ![node] nœud avec kerb=raised

- partial si le
    ![way] chemin a des
    ![node] noeuds avec kerb:height compris
    entre 0,04 m et 0,02 m ou kerb=lowered

- true sinon

**AccessibilityLimitation/EscalatorFreeAccess** vaut false si
[conveying!=no](https://wiki.openstreetmap.org/wiki/FR:Key:conveying) et
[highway=steps](https://wiki.openstreetmap.org/wiki/FR:Tag:highway%3Dsteps),
true sinon.

**AccessibilityLimitation/LiftFreeAccess** vaut false si
[highway=elevator](https://wiki.openstreetmap.org/wiki/FR:Tag:highway%3Delevator),
true sinon.

**AccessibilityLimitation/VisualSignsAvailable** est susceptible d'être
renseigné uniquement si AccessFeatureType vaut crossing. L'attribut
prend alors la même valeur que CrossingEquipment/ZebraCrossing.

**AccessibilityLimitation/AudibleSignalsAvailable** est renseigné
uniquement si AccessFeatureType vaut crossing ou elevator. L'attribut
prend alors la même valeur que CrossingEquipment/AcousticCrossingAids ou
LiftEquipment/AudioAnnouncements.

#### SitePathLink/Covered

SitePathLink/Covered est construit avec les règles de gestion suivantes
:

- indoors si
    [indoor=yes](https://wiki.openstreetmap.org/wiki/FR:Key:indoor)

- indoors si
    [tunnel=yes](https://wiki.openstreetmap.org/wiki/FR:Key:tunnel)

- covered si
    [tunnel=building_passage](https://wiki.openstreetmap.org/wiki/FR:Key:tunnel)

- covered si
    [covered=yes](https://wiki.openstreetmap.org/wiki/FR:Key:covered)

- outdoors sinon (sauf pour les highway=elevator)

#### SitePathLink/Lighting

SitePathLink/Lighting est renseigné à partir du tag
[lit](https://wiki.openstreetmap.org/wiki/FR:Key:lit) :

- unlit si lit=no

- wellLit si lit a une autre valeur, sauf cas particulier

- non renseigné si le tag est absent

Cas particulier : si le tag
[lit:perceived](https://wiki.openstreetmap.org/wiki/FR:Key:lit:perceived)
est renseigné et qu'il a une valeur différente de
good/daylike/none/minimal, alors l'élément SitePathLink/Lighting aura
la valeur poorlyLit.

#### SitePathLink/AllAreasWheelchairAccessible

SitePathLink/AllAreasWheelchairAccessible est construit à partir du tag
[wheelchair](https://wiki.openstreetmap.org/wiki/FR:Key:wheelchair) :

- true si wheelchair=yes

- false si wheelchair=no

- non renseigné sinon

#### SitePathLink/NumberOfSteps

Si le SitePathLink est un escalier (AccessFeatureType valant stairs), on
utilise les règles de gestion de l'attribut
StaircaseEquipment/NumberofSteps.

Sinon, SitePathLink/NumberOfSteps est construit à partir du nombre de
![node] nœuds OSM avec
[barrier=kerb](https://wiki.openstreetmap.org/wiki/FR:Tag:barrier%3Dkerb)
ou [kerb=raised](https://wiki.openstreetmap.org/wiki/FR:Key:kerb)
constituant le
![way] chemin.

#### SitePathLink/MinimumWidth

SitePathLink/MinimumWidth est rempli avec la valeur du tag
[width](https://wiki.openstreetmap.org/wiki/FR:Key:width) ou à défaut
[est_width](https://wiki.openstreetmap.org/w/index.php?title=FR:Key:est_width)
en mètres arrondis au cm.

**Cas particulier :** dans le cas d'un
[highway=elevator,](https://wiki.openstreetmap.org/wiki/FR:Tag:highway%3Delevator)
on utilisera plutôt
[door:width](https://wiki.openstreetmap.org/wiki/FR%3AKey%3Adoor%3Awidth).

**Cas particulier :** si le
![way] chemin a des
![node] nœuds constituant des obstacles
(c'est-à-dire avec un tag
[barrier](https://wiki.openstreetmap.org/wiki/FR:Key:barrier)), on
utilisera à la place la plus petite valeur du tag
[maxwidth:physical](https://wiki.openstreetmap.org/wiki/Key:maxwidth:physical)
(l'espace disponible restant) indiqué sur ces obstacles.

#### SitePathLink/AllowedUse

SitePathLink/AllowedUse est rempli avec la valeur "oneWay" si
[conveying=forward/backward](https://wiki.openstreetmap.org/wiki/FR:Key:conveying).

#### SitePathLink/Transition

SitePathLink/Transition est construit à partir du tag
[incline](https://wiki.openstreetmap.org/wiki/FR:Key:incline) :

- up si incline=up/un nombre positif

- down si incline=down/un nombre négatif

- level si incline=no

#### SitePathLink/Gradient

SitePathLink/Gradient est rempli avec la valeur du tag
[incline](https://wiki.openstreetmap.org/wiki/FR:Key:incline) avec les
règles suivantes :

- si incline est de la forme valeur%, on applique la conversion :
    arctan(valeur absolue(incline en %) / 100)

- si incline est de la forme valeur°, on prend la valeur absolue de
    cette valeur

- si incline=up/down, l'élément ne peut être renseigné

#### SitePathLink/GradientType

SitePathLink/GradientType est rempli à partir la valeur du tag
[incline](https://wiki.openstreetmap.org/wiki/FR:Key:incline) en %,
arrondi à l'entier supérieur et avec les règles de gestion suivantes :

- verySteep si supérieur ou égal à 9%

- steep si entre 6 et 8%

- medium si égal à 5%

- gentle si entre 1 et 4%

- level si 0

#### SitePathLink/TiltAngle

SitePathLink/TiltAngle est rempli avec la valeur du tag
[incline:across](https://wiki.openstreetmap.org/wiki/Key:incline:across)
avec les règles suivantes :

- si incline:across est de la forme valeur%, on applique la conversion
    : arctan(valeur absolue(incline en %) / 100)

- si incline:across est de la forme valeur°, on prend la valeur
    absolue de cette valeur

#### SitePathLink/TiltType

SitePathLink/TiltType est construit à partir du tag
[incline:across](https://wiki.openstreetmap.org/wiki/Key:incline:across)
en %, arrondi à l'entier supérieur avec les règles suivantes :

- strongRightTilt si inférieur ou égal à 3%

- mediumRightTilt si entre 2 et 3%

- nearlyFlat si entre -2 et 2%

- mediumLeftTilt si entre -2 et -3%

- strongLeftTilt si inférieur à -3%

#### SitePathLink/AccessFeatureType

cf [§ sélection](#selection)

#### SitePathLink/PassageType

SitePathLink/PassageType est construit avec les règles de gestion
suivantes :

- corridor si
    [indoor=yes](https://wiki.openstreetmap.org/wiki/FR:Key:indoor)

- tunnel si
    [tunnel=yes](https://wiki.openstreetmap.org/wiki/FR:Key:tunnel)

- overpass si
    [bridge=yes](https://wiki.openstreetmap.org/wiki/FR:Key:bridge)

- underpass si
    [covered=yes](https://wiki.openstreetmap.org/wiki/FR:Key:covered)

- outdoors sinon (sauf pour les highway=elevator)

#### SitePathLink/FlooringType

SitePathLink/FlooringType est construit à partir de la valeur du tag
[surface](https://wiki.openstreetmap.org/wiki/FR:Key:surface) :

- asphalt si surface=asphalt/tarmac

- concrete si surface=concrete/concrete:lanes/concrete:plates/cement

- carpet si surface=carpet

- earth si
    surface=ground/dirt/unpaved/sand/earth/clay/mud/stepping_stone/snow/soil

- fibreglassGrating si surface=grass_paver

- grass si surface=grass/artificial_turf

- gravel si surface=gravel/fine_gravel/pebblestone/chipseal

- stone si
    surface=paving_stones/sett/cobblestone/unhewn_cobblestone/paving_stones:30/bricks

- stone si surface=rock/stone/bare_rock/rocks

- platicMatting si surface=plastic/linoleum

- rubber si surface=rubber

- sand si surface=compacted

- steelPlate si surface=metal/metal_grid

- wood si surface=wood

- other si surface est renseigné avec une autre valeur

#### SitePathLink/TactileWarningStrip

SitePathLink/TactileWarningStrip est construit à partir du tag
[tactile_paving](https://wiki.openstreetmap.org/wiki/FR:Key:tactile_paving)
sur les
![node] nœuds de départ et de destination du
![way] chemin :

- tactileStripAtBothEnds si les deux
    ![node] nœuds ont
    tactile_paving=yes/contrasted

- tactileStripAtBeginning si le
    ![node] nœud de départ a
    tactile_paving=yes/contrasted et le
    ![node] nœud de destination a
    tactile_paving=no

- tactileStripAtEnd si le
    ![node] nœud de destination a
    tactile_paving=yes/contrasted et le
    ![node] nœud de départ a tactile_paving=no

- noTactileStrip si les deux
    ![node] nœuds ont tactile_paving=no

#### SitePathLink/TactileGuidingStrip

SitePathLink/TactileGuidingStrip est construit à partir de la valeur du
tag
[tactile_paving](https://wiki.openstreetmap.org/wiki/FR:Key:tactile_paving)
:

- true si tactile_paving=yes/contrasted

- false si tactile_paving=no

#### SitePathLink/equipmentPlaces

SitePathLink/equipmentPlaces contient une liste de références vers les
équipements ponctuels rencontrés sur le au
![way] chemin, ainsi que leurs positions
respectives.

Les équipements concernés sont :

- EntranceEquipment si le
    ![way] chemin contient au moins un
    ![node] nœud avec
    [railway=subway_entrance](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Arailway%3Dsubway_entrance)
    ou [railway=train_station
    entrance](https://wiki.openstreetmap.org/wiki/Tag%3Arailway%3Dtrain_station_entrance)

- LiftEquipment si le
    ![way] chemin contient au moins un
    ![node] nœud avec
    [highway=elevator](https://wiki.openstreetmap.org/wiki/FR:Tag:highway%3Delevator)

- ShelterEquipment si le
    ![way] chemin contient au moins un
    ![node] nœud avec
    [amenity=shelter](https://wiki.openstreetmap.org/wiki/FR:Tag:amenity%3Dshelter)
    ou un arrêt de transport avec
    [shelter=yes](https://wiki.openstreetmap.org/wiki/FR:Key:shelter)

- SanitaryEquipment si le
    ![way] chemin contient au moins un
    ![node] nœud avec
    [amenity=toilets](https://wiki.openstreetmap.org/wiki/FR:Tag:amenity=toilets)
    ou
    [amenity=shower](https://wiki.openstreetmap.org/wiki/Tag:amenity=shower)

- SeatingEquipment si le
    ![way] chemin contient au moins un
    ![node] nœud avec
    [amenity=bench](https://wiki.openstreetmap.org/wiki/FR:Tag:amenity%3Dbench)
    ou un arrêt de transport avec
    [bench=yes](https://wiki.openstreetmap.org/wiki/FR:Key:bench)

- GeneralSign si le
    ![way] chemin contient au moins un
    ![node] nœud avec
    [information!=office/visitor_centre](https://wiki.openstreetmap.org/wiki/Key:information)

- TicketValidatorEquipment si le
    ![way] chemin contient au moins un
    ![node] nœud avec
    [barrier=turnstile](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Abarrier%3Dturnstile)
    ou
    [amenity=ticket_validator](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Aamenity%3Dticket_validator)

- RubbishDisposalEquipment si le
    ![way] chemin contient au moins un
    ![node] nœud avec
    [amenity=waste_basket](https://wiki.openstreetmap.org/wiki/FR:Tag:amenity%3Dwaste_basket)
    ou un arrêt de transport avec
    [bin=yes](https://wiki.openstreetmap.org/wiki/Key:bin)

#### SitePathLink/placeEquipments

SitePathLink/placeEquipments contient une liste de références vers des
équipements de cheminement.

Les équipements de cheminement concernés sont :

- CrossingEquipment si AccessFeatureType vaut crossing

- RampEquipment si AccessFeatureType vaut ramp

- StaircaseEquipment si AccessFeatureType vaut stairs

- EscalatorEquipment si AccessFeatureType vaut escalator

- TravelatorEquipment si AccessFeatureType vaut travelator

- LiftEquipment si AccessFeatureType vaut lift

## Les équipements de cheminement

### CrossingEquipment

Un objet NeTEx CrossingEquipment est créé pour chaque objet NeTEx SitePathLink avec AccessFeatureType valant crossing, en complément de celui-ci.

#### CrossingEquipment/Width

CrossingEquipment/Width est rempli avec la valeur du tag tag
[width](https://wiki.openstreetmap.org/wiki/FR:Key:width), en mètres
arrondis au cm.

#### CrossingEquipment/CrossingType

CrossingEquipment/CrossingType est construit avec les règles de gestion
suivantes :

- levelCrossing si le
    ![way] chemin comprend un
    ![node] nœud avec
    railway=[crossing](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Arailway%3Dcrossing)/[level_crossing](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Arailway%3Dlevel_crossing)
    et
    [crossing:barrier](https://wiki.openstreetmap.org/wiki/Key:crossing:barrier)
    non renseigné

- levelCrossing si le
    ![way] chemin comprend un
    ![node] nœud avec
    railway=[crossing](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Arailway%3Dcrossing)/[level_crossing](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Arailway%3Dlevel_crossing)/[tram_crossing](https://wiki.openstreetmap.org/wiki/Tag%3Arailway%3Dtram_crossing)/[tram_level_crossing](https://wiki.openstreetmap.org/wiki/Tag%3Arailway%3Dtram_level_crossing)
    et
    [crossing:barrier](https://wiki.openstreetmap.org/wiki/Key:crossing:barrier)!=no

- barrowCrossing si le
    ![way] chemin comprend un
    ![node] nœud avec
    [tram_crossing](https://wiki.openstreetmap.org/wiki/Tag%3Arailway%3Dtram_crossing)/[tram_level_crossing](https://wiki.openstreetmap.org/wiki/Tag%3Arailway%3Dtram_level_crossing)
    et
    [crossing:barrier](https://wiki.openstreetmap.org/wiki/Key:crossing:barrier)
    non renseigné

- barrowCrossing si le
    ![way] chemin comprend un
    ![node] nœud avec
    railway=[crossing](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Arailway%3Dcrossing)/[level_crossing](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Arailway%3Dlevel_crossing)/[tram_crossing](https://wiki.openstreetmap.org/wiki/Tag%3Arailway%3Dtram_crossing)/[tram_level_crossing](https://wiki.openstreetmap.org/wiki/Tag%3Arailway%3Dtram_level_crossing)
    et
    [crossing:barrier](https://wiki.openstreetmap.org/wiki/Key:crossing:barrier)=no

- roadCrossingWithIsland si
    [crossing:island=yes](https://wiki.openstreetmap.org/wiki/FR%3AKey%3Acrossing%3Aisland)
    et si le
    ![way] chemin comprend un
    ![node] nœud avec highway=crossing

- roadCrossing si le
    ![way] chemin comprend un
    ![node] nœud avec
    [highway=crossing](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Ahighway%3Dcrossing)

#### CrossingEquipment/ZebraCrossing

CrossingEquipment/ZebraCrossing est construit avec les règles de gestion
suivantes :

- false si
    [crossing=unmarked](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Acrossing%3Dunmarked)

- false si
    [crossing:markings](https://wiki.openstreetmap.org/wiki/FR:Key:crossing:markings)=no

- true si
    [crossing:markings](https://wiki.openstreetmap.org/wiki/FR:Key:crossing:markings)~zebra

#### CrossingEquipment/PedestrianLights

CrossingEquipment/PedestrianLights est construit avec les règles de
gestion suivantes :

- false si
    [crossing=uncontrolled](https://wiki.openstreetmap.org/wiki/Tag%3Acrossing%3Duncontrolled)

- true si
    [crossing=traffic_signals](https://wiki.openstreetmap.org/wiki/Tag%3Acrossing%3Dtraffic_signals)

- false si
    [flashing_lights=no](https://wiki.openstreetmap.org/wiki/Key%3Aflashing_lights)

- false si
    [crossing:light=no](https://wiki.openstreetmap.org/wiki/Key%3Acrossing%3Alight)

- false si
    [crossing:signals=no](https://wiki.openstreetmap.org/wiki/Key%3Acrossing%3Asignals)

- true si
    [flashing_lights=yes](https://wiki.openstreetmap.org/wiki/Key%3Aflashing_lights)

- true si
    [crossing:light=yes](https://wiki.openstreetmap.org/wiki/Key%3Acrossing%3Alight)

- true si
    [crossing:signals=yes](https://wiki.openstreetmap.org/wiki/Key%3Acrossing%3Asignals)

- true si
    [button_operated=yes](https://wiki.openstreetmap.org/wiki/Key%3Abutton_operated)

#### CrossingEquipment/AcousticCrossingAids

CrossingEquipment/AcousticCrossingAids est construit avec les règles de
gestion suivantes :

- false si
    [traffic_signals:sound=no](https://wiki.openstreetmap.org/wiki/FR%3AKey%3Atraffic_signals%3Asound)

- true si
    [traffic_signals:sound!=no](https://wiki.openstreetmap.org/wiki/FR%3AKey%3Atraffic_signals%3Asound)

- false si
    [crossing:bell=no](https://wiki.openstreetmap.org/wiki/Key%3Acrossing%3Abell)

- true si
    [crossing:bell!=no](https://wiki.openstreetmap.org/wiki/Key%3Acrossing%3Abell)

#### CrossingEquipment/TactileGuidanceStrips

CrossingEquipment/TactileGuidanceStrips est construit à partir de la
valeur du tag
[tactile_paving](https://wiki.openstreetmap.org/wiki/FR:Key:tactile_paving)
sur le
![way] chemin uniquement :

- true si tactile_paving=yes/contrasted

- false si tactile_paving=no

#### CrossingEquipment/TactileWarningStrip

CrossingEquipment/TactileWarningStrip est construit à partir du tag
[tactile_paving](https://wiki.openstreetmap.org/wiki/FR:Key:tactile_paving)
sur les
![node] nœuds de départ et de destination du
![way] chemin, ou à défaut sur le
![node] nœud highway=crossing lorsqu'il n'y en
a qu'un :

- tactileStripAtBothEnds si les deux
    ![node] nœuds de départ et de destination
    ont tactile_paving=yes/contrasted

- tactileStripAtBeginning si le
    ![node] nœud de départ a
    tactile_paving=yes/contrasted et le
    ![node] nœud de destination a
    tactile_paving=no

- tactileStripAtEnd si le
    ![node] nœud de destination uniquement a
    tactile_paving=yes/contrasted et le
    ![node] nœud de départ a tactile_paving=no

- noTactileStrip si les deux
    ![node] nœuds de départ et de destination
    ont tactile_paving=no

- tactileStripAtBothEnds si le
    ![node] nœud avec highway=crossing a
    tactile_paving=yes/contrasted

- noTactileStrip si le
    ![node] nœud avec highway=crossing a
    tactile_paving=no

#### CrossingEquipment/VisualGuidanceBands

CrossingEquipment/VisualGuidanceBands est construit avec les règles de
gestion suivantes :

- false si tactile_paving_no et
    [crossing:markings](https://wiki.openstreetmap.org/wiki/FR:Key:crossing:markings)=no
    ou non renseigné

- true si tactile_paving=yes/contrasted

- true si le tag
    [crossing:markings](https://wiki.openstreetmap.org/wiki/FR:Key:crossing:markings)
    contient un des valeurs suivantes :
    surface/lines/zebra:double/ladder/ladder:skewed/ladder:paired/dashes/dots/pictograms

#### CrossingEquipment/DroppedKerb

CrossingEquipment/DroppedKerb est construit à partir à partir du tag
[kerb](https://wiki.openstreetmap.org/wiki/FR:Key:kerb) sur les
![node] nœuds de départ et de destination du
![way] chemin, ou à défaut sur le
![node] nœud highway=crossing lorsqu'il n'y en
a qu'un :

- true si kerb=lowered/flush/no sur les deux
    ![node] nœuds de départ et de destination

- false si kerb=raised/rolled sur les deux
    ![node] nœuds de départ et de destination

- true si kerb=lowered/flush/no sur le
    ![node] nœud avec highway=crossing

- false si kerb=raised/rolled sur le
    ![node] nœud avec highway=crossing

#### CrossingEquipment/MarkingStatus

CrossingEquipment/MarkingStatus est rempli à partir de la valeur du tag [crossing:markings:condition](https://wiki.openstreetmap.org/wiki/FR:Key:crossing:markings:condition) avec les règles de gestion suivantes :

- good si crossing:markings:condition=excellent/good
- worn si crossing:markings:condition=intermediate/bad/very_bad
- hazardous si crossing:markings:condition=horrible/very_horrible/impassable

#### CrossingEquipment/VibratingCrossingAids

CrossingEquipment/VibratingCrossingAids est construit avec les règles de
gestion suivantes :

- false si
    [traffic_signals:vibration](https://wiki.openstreetmap.org/wiki/Key%3Atraffic_signals%3Avibration)=no

- true si traffic_signals:vibration!=no

- false si
    [traffic_signals:floor_vibration](https://wiki.openstreetmap.org/wiki/Key%3Atraffic_signals%3Afloor_vibration)=no

- true si traffic_signals:floor_vibration!=no

#### CrossingEquipment/VisualObstacle

CrossingEquipment/VisualObstacle est construit avec les règles de
gestion suivantes :

- none si crossing:buffer_marking = both/left/right/yes
- none si les deux noeuds aux extrémités ont crossing:buffer_marking = both/left/right/yes

#### CrossingEquipment/BollardCrossing

CrossingEquipment/BollardCrossing est rempli à partir de la valeur du tag [crossing:bollard](https://wiki.openstreetmap.org/wiki/Key:crossing:bollard) avec les règles de gestion suivantes :

- yes si crossing:bollard=yes
- partial si crossing:bollard=partial
- limited si crossing:bollard=bad
- none si crossing:bollard=no

### RampEquipment

Un objet NeTEx RampEquipment est créé pour les
![way] chemins OSM avec le tag
[incline](https://wiki.openstreetmap.org/wiki/FR:Key:incline), en
complément de l'objet NeTEx SitePathLink.

#### RampEquipment/Width

RampEquipment/Width est rempli avec la valeur du tag
[width](https://wiki.openstreetmap.org/wiki/FR:Key:width), en mètres
arrondis au cm.

#### RampEquipment/Length

RampEquipment/Length est rempli avec la distance calculée du chemin, en
mètres arrondis au cm.

#### RampEquipment/Gradient

RampEquipment/Gradient est rempli avec la valeur du tag
[incline](https://wiki.openstreetmap.org/wiki/FR:Key:incline) avec les
règles suivantes :

- si incline=up/down, l'élément n'est pas renseigné

- si incline est de la forme valeur%, on applique la conversion :
    arctan(valeur absolue(incline en %) / 100)

- si incline est de la forme valeur°, on prend la valeur absolue de
    cette valeur

#### RampEquipment/GradientType

RampEquipment/GradientType est rempli à partir la valeur du tag
[incline](https://wiki.openstreetmap.org/wiki/FR:Key:incline) en %,
arrondi à l'entier supérieur et avec les règles de gestion suivantes :

- verySteep si supérieur ou égal à 9%

- steep si entre 6 et 8%

- medium si égal à 5%

- gentle si entre 1 et 4%

- level si 0

#### RampEquipment/HandrailType

RampEquipment/HandrailType est rempli avec les règles de gestion
suivantes :

- bothSides si handrail=yes/both

- oneSide si handrail=left/rigth/center

- oneSide si handrail:left!=no

- oneSide si handrail:right!=no

- oneSide si handrail:center!=no

- non renseigné sinon

#### RampEquipment/TactileGuidanceStrips

RampEquipment/TactileGuidanceStrips est construit à partir du tag
[tactile_paving](https://wiki.openstreetmap.org/wiki/FR:Key:tactile_paving),
avec les mêmes règles de gestion que pour l'élément SitePathLink.

#### RampEquipment/VisualGuidanceBands

RampEquipment/VisualGuidanceBands a la même valeur que
RampEquipment/TactileGuidanceStrips.

#### RampEquipment/Temporary

RampEquipment/Temporary est rempli avec la valeur fixe "false", sauf
si [temporary=yes](https://wiki.openstreetmap.org/wiki/Key%3Atemporary).

### EscalatorEquipment

Un objet NeTEx EscalatorEquipment est créé pour les
![way] chemins OSM avec les tags
[conveying!=no](https://wiki.openstreetmap.org/wiki/FR:Key:conveying) et
[highway=steps](https://wiki.openstreetmap.org/wiki/FR:Tag:highway%3Dsteps),
en complément de l'objet NeTEx SitePathLink.

#### EscalatorEquipment/PublicCode

EscalatorEquipment/PublicCode est rempli avec la valeur du tag OSM
[ref](https://wiki.openstreetmap.org/wiki/FR:Key:ref) ou à défaut
[local_ref](https://wiki.openstreetmap.org/wiki/Key%3Alocal_ref).

#### EscalatorEquipment/Width

EscalatorEquipment/Width est rempli avec la valeur du tag
[width](https://wiki.openstreetmap.org/wiki/FR:Key:width), en mètres
arrondis au cm.

#### EscalatorEquipment/DirectionOfUse

EscalatorEquipment/DirectionOfUse est construit à partir des valeurs des
tags [incline](https://wiki.openstreetmap.org/wiki/FR:Key:incline) et
[conveying](https://wiki.openstreetmap.org/wiki/FR:Key:conveying) avec
les règles de gestion suivantes :

- both, si conveying=reversible

- up si conveying=forward et incline=up/un nombre positif

- down si conveying=backward et incline=up/un nombre positif

- up si conveying=backward et incline=down/un nombre négatif

- down si conveying=forward et incline=down/un nombre négatif

### TravelatorEquipment

Un objet NeTEx EscalatorEquipment est créé pour les
![way] chemins OSM avec les tags
[conveying!=no](https://wiki.openstreetmap.org/wiki/FR:Key:conveying) et
[highway=footway](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Ahighway%3Dfootway),
en complément de l'objet NeTEx SitePathLink.

#### TravelatorEquipment/PublicCode

TravelatorEquipment/PublicCode est rempli avec la valeur du tag OSM
[ref](https://wiki.openstreetmap.org/wiki/FR:Key:ref) ou à défaut
[local_ref](https://wiki.openstreetmap.org/wiki/Key%3Alocal_ref).

#### TravelatorEquipment/Width

TravelatorEquipment/Width est rempli avec la valeur du tag tag
[width](https://wiki.openstreetmap.org/wiki/FR:Key:width), en mètres
arrondis au cm.

#### TravelatorEquipment/DirectionOfUse

TravelatorEquipment/DirectionOfUse est rempli à partir de la valeur du
tag conveying:

- both si conveying=reversible

- up sinon

#### TravelatorEquipment/Length

TravelatorEquipment/Length est rempli avec la distance calculée du
SitePathLink, en mètres arrondis au cm.

#### TravelatorEquipment/Gradient

TravelatorEquipment/Gradient est rempli avec la valeur du tag
[incline](https://wiki.openstreetmap.org/wiki/FR:Key:incline), en
degrés, sur le même modèle que l'élément SitePathLink/Gradient.

### LiftEquipment

Un objet NeTEx LiftEquipment est créé pour les
![node] nœuds et les
![way] chemins OSM avec les tags
[highway=elevator](https://wiki.openstreetmap.org/wiki/FR:Tag:highway%3Delevator),
en complément de l'objet NeTEx SitePathLink.

#### LiftEquipment/PublicCode

LiftEquipment/PublicCode est rempli avec la valeur du tag OSM
[ref](https://wiki.openstreetmap.org/wiki/FR:Key:ref) ou à défaut
[local_ref](https://wiki.openstreetmap.org/wiki/Key%3Alocal_ref).

#### LiftEquipment/Depth

LiftEquipment/Depth est rempli avec le premier tag suivant rencontré, en
mètres arrondi au cm :

- [maxlength:physical](https://wiki.openstreetmap.org/wiki/Key:maxlength:physical)

- [length](https://wiki.openstreetmap.org/wiki/FR:Key:length)

#### LiftEquipment/MaximumLoad

LiftEquipment/MaximumLoad est rempli avec la valeur du tag
[maxweight](https://wiki.openstreetmap.org/wiki/FR:Key:maxweight),
converti en kilogrammes arrondis au kilogramme.

#### LiftEquipment/WheelchairPassable

LiftEquipment/WheelchairPassable est construit à partir du tag
[wheelchair](https://wiki.openstreetmap.org/wiki/FR:Key:wheelchair) :

- true si wheelchair=yes

- false si wheelchair=no

#### LiftEquipment/InternalWidth

LiftEquipment/InternalWidth est rempli avec le premier tag suivant
rencontré, en mètres arrondi au cm :

- [maxwidth:physical](https://wiki.openstreetmap.org/wiki/Key:maxwidth:physical)

- [width](https://wiki.openstreetmap.org/wiki/FR:Key:width)

- [est_width](https://wiki.openstreetmap.org/wiki/FR:Key:est_width)

#### LiftEquipment/HandrailType

LiftEquipment/HandrailType est construit avec les règles de gestion
suivantes :

- bothSides si
    [handrail=yes/both](https://wiki.openstreetmap.org/wiki/FR:Key:handrail)

- oneSide si handrail=left/rigth/center

- oneSide si handrail:left!=no

- oneSide si handrail:right!=no

- oneSide si handrail:center!=no

- non renseigné sinon

#### LiftEquipment/RaisedButtons

LiftEquipment/RaisedButtons est rempli avec les tags
[tactile_writing](https://wiki.openstreetmap.org/wiki/FR:Key:tactile_writing)
:

- false si tactile_writing=no

- true si tactile_writing=yes

- true si tactile_writing:braille=yes

- true si tactile_writing:embossed_printed_letters=yes

- true si tactile_writing:engraved_printed_letters=yes

#### LiftEquipment/BrailleButtons

LiftEquipment/BrailleButtons est rempli avec les tags
[tactile_writing:braille](https://wiki.openstreetmap.org/wiki/FR:Key:tactile_writing)
:

- false si tactile_writing:braille=no

- true si tactile_writing:braille=yes

#### LiftEquipment/AudioAnnouncements

LiftEquipment/AudioAnnouncements est rempli à partir de la valeur du tag
[speech_output](https://wiki.openstreetmap.org/wiki/Key:speech_output) :

- false si speech_output=no

- true si speech_output=yes

#### LiftEquipment/MagneticInductionLoop

LiftEquipment/MagneticInductionLoop est construit avec les règles de
gestion suivantes :

- true si audio_loop=yes

- true si hearing_loop=yes

- false si audio_loop=no

- false si hearing_loop=no

### StaircaseEquipment

Un objet NeTEx StaircaseEquipment est créé pour les
![way] chemins OSM avec les tags
[highway=steps](https://wiki.openstreetmap.org/wiki/FR:Tag:highway%3Dsteps),
en complément de l'objet NeTEx SitePathLink.

#### StaircaseEquipment/Width

StaircaseEquipment/Width est rempli avec la valeur du tag tag
[width](https://wiki.openstreetmap.org/wiki/FR:Key:width), en mètres
arrondis au cm.

#### StaircaseEquipment/NumberofSteps

StaircaseEquipment/NumberofSteps est avec la valeur du tag
[step_count](https://wiki.openstreetmap.org/wiki/FR:Key:step_count) (ou
à défaut
[est_step_count](https://wiki.openstreetmap.org/wiki/FR:Key:step_count)).

#### StaircaseEquipment/StepHeight

StaircaseEquipment/StepLength est rempli avec la valeur du tag
step:height, converti en mètres arrondi au centimètre.

#### StaircaseEquipment/StepLength

StaircaseEquipment/StepLength est rempli avec la valeur du tag
step:length, converti en mètres arrondi au centimètre.

#### StaircaseEquipment/StepColourContrast

StaircaseEquipment/StepColourContrast est rempli à partir de la valeur du tag [step:stair_nosing](https://wiki.openstreetmap.org/wiki/FR:Key:step:stair_nosing) avec les règles de gestion suivantes :

- true si step:stair_nosing=yes
- false si step:stair_nosing=false

#### StaircaseEquipment/StepCondition

StaircaseEquipment/StepCondition est rempli avec la valeur du tag
[step:condition](https://wiki.openstreetmap.org/wiki/FR%3AKey%3Astep%3Acondition),
avec les règles de gestion suivantes :

- even si step:condition=even

- uneven si step:condition=uneven

- rough si step:condition=rough

#### StaircaseEquipment/HandrailType

StaircaseEquipment/HandrailType est construit avec les règles de gestion
suivantes :

- bothSides si
    [handrail=yes/both](https://wiki.openstreetmap.org/wiki/FR:Key:handrail)

- oneSide si handrail=left/rigth/center

- oneSide si handrail:left!=no

- oneSide si handrail:right!=no

- oneSide si handrail:center!=no

- non renseigné sinon

#### StaircaseEquipment/TactileWriting

StaircaseEquipment/TactileWriting est rempli avec les tags
[tactile_writing](https://wiki.openstreetmap.org/wiki/FR%3AKey%3Atactile_writing)
:

- false si tactile_writing=no

- true si tactile_writing=yes

- true si tactile_writing:braille=yes

- true si tactile_writing:embossed_printed_letters=yes

- true si tactile_writing:engraved_printed_letters=yes

#### StaircaseEquipment/StairRamp

StaircaseEquipment/StairRamp est rempli avec la valeur du tag
[ramp](https://wiki.openstreetmap.org/wiki/FR%3AKey%3Aramp), avec les
règles de gestion suivantes :

- none si ramp=no/separate

- bicycle si ramp:bicycle!=no

- luggage si ramp:luggage!=no

- stroller si ramp:stroller!=no

- other si ramp=yes et aucun des tags suivants ne sont renseignés :
    ramp:bicycle, ramp:luggage et ramp:stroller

#### StaircaseEquipment/TopEnd/VisualContrast

StaircaseEquipment/TopEnd/VisualContrast est rempli avec la valeur du
tag
[step:contrast](https://wiki.openstreetmap.org/wiki/FR:Key:step:contrast)
:

- true si step:contrast=yes

- false si step:contrast=no

#### StaircaseEquipment/TopEnd/TexturedSurface

StaircaseEquipment/TopEnd/TexturedSurface est rempli avec la valeur du
tag
[tactile_paving](https://wiki.openstreetmap.org/wiki/FR:Key:tactile_paving)
sur le
![node] nœud situé en haut de l'escalier :

- true si tactile_paving=yes/contrasted

- false si tactile_paving=no

Le
![node] nœud situé en haut de l'escalier est

- le point de départ du
    ![way] chemin si incline=down

- le point de destination du
    ![way] chemin si incline=up ou n'est pas
    renseigné

#### StaircaseEquipment/BottomEnd/VisualContrast

StaircaseEquipment/BottomEnd/VisualContrast est rempli avec la valeur du
tag
[step:contrast](https://wiki.openstreetmap.org/wiki/FR:Key:step:contrast)
:

- true si step:contrast=yes

- false si step:contrast=no

#### StaircaseEquipment/BottomEnd/TexturedSurface

StaircaseEquipment/BottomEnd/TexturedSurface est rempli avec la valeur
du tag
[tactile_paving](https://wiki.openstreetmap.org/wiki/FR:Key:tactile_paving)
sur le
![node] nœud situé en bas de l'escalier :

- true si tactile_paving=yes/contrasted

- false si tactile_paving=no

## Les équipements ponctuels

### EntranceEquipment

Les
![node] nœuds avec
[railway=subway_entrance](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Arailway%3Dsubway_entrance)
ou [railway=train_station
entrance](https://wiki.openstreetmap.org/wiki/Tag%3Arailway%3Dtrain_station_entrance)
sont convertis en EntranceEquipment, en complément de l'objet NeTEx
Entrance.

#### EntranceEquipment/Width

EntranceEquipment/Width est rempli avec la valeur du tag tag
[width](https://wiki.openstreetmap.org/wiki/FR:Key:width), en mètres
arrondis au cm.

#### EntranceEquipment/Door

EntranceEquipment/Door est contruit à partir du tag
[door](https://wiki.openstreetmap.org/wiki/FR:Key:door) :

- false si door=no

- true si door!=no

#### EntranceEquipment/DoorHandleOutside

EntranceEquipment/DoorHandleOutside est rempli à partir de
door:handle:outside ou à défaut door:handle :

- none si door:handle:outside=no ou door:handle=no

- lever si door:handle:outside=lever ou door:handle=lever

- button si door:handle:outside=button ou door:handle=button

- knob si door:handle:outside=knob ou door:handle=knob

- other si door:handle:outside / door:handle est renseigné avec une
    autre valeur

#### EntranceEquipment/DoorHandleInside

EntranceEquipment/DoorHandleInside est rempli à partir de
door:handle:inside ou à défaut door:handle :

- none si door:handle:inside=no ou door:handle=no

- lever si door:handle:inside=lever ou door:handle=lever

- button si door:handle:inside=button ou door:handle=button

- knob si door:handle:inside=knob ou door:handle=knob

- other si door:handle:inside / door:handle est renseigné avec une
    autre valeur

#### EntranceEquipment/RevolvingDoor

EntranceEquipment/RevolvingDoor est contruit à partir du tag
[door](https://wiki.openstreetmap.org/wiki/FR:Key:door) :

- true si door=revolving

- false si door!=no/revolving

#### EntranceEquipment/Barrier

EntranceEquipment/Barrier est contruit à partir du tag
[door](https://wiki.openstreetmap.org/wiki/FR:Key:barrier) :

- true si barrier!=gate

- false si door=no

#### EntranceEquipment/DropKerbOutside

EntranceEquipment/DropKerbOutside est construit avec les règles de
gestion suivantes :

- true si la valeur du tag
    [kerb:height](https://wiki.openstreetmap.org/wiki/Key:kerb#Kerb_height)
    est inférieure ou égale à 0.02 m

- true si la valeur du tag wheelchair:step_height est inférieure ou
    égale à 0.02 m

- false si la valeur du tag kerb:height est supérieure à 0.02 m

- true si la valeur du tag wheelchair:step_height est supérieure à
    0.02 m

#### EntranceEquipment/AutomaticDoor

EntranceEquipment/AutomaticDoor est construit à partir de la valeur du
tag
[automatic_door](https://wiki.openstreetmap.org/wiki/Key:automatic_door)
:

- true si automatic_door=yes/motion/continuous/slowdown_button/floor

- false si automatic_door=no

#### EntranceEquipment/GlassDoor

EntranceEquipment/GlassDoor est construit avec les règles de gestion
suivantes :

- true si material=glass

- false si material!=glass

#### EntranceEquipment/WheelchairPassable

EntranceEquipment/WheelchairPassable est construit à partir du tag
[wheelchair](https://wiki.openstreetmap.org/wiki/FR:Key:wheelchair) :

- true si wheelchair=yes

- false si wheelchair=no

#### EntranceEquipment/DoorstepMark

EntranceEquipment/DoorstepMark est rempli avec la valeur du tag
[tactile_paving](https://wiki.openstreetmap.org/wiki/FR:Key:tactile_paving)
:

- true si tactile_paving=yes/contrasted

- false si tactile_paving=no

### ShelterEquipment

Les objets
![node]
![area] OSM avec les attributs suivants sont
convertis en ShelterEquipment :

- [amenity=shelter](https://wiki.openstreetmap.org/wiki/FR:Tag:amenity%3Dshelter)

- arrêts de transport avec
    [shelter=yes](https://wiki.openstreetmap.org/wiki/FR:Key:shelter)

Une transformation géométrique est nécessaire pour les
![area] polygones afin d'obtenir une géométrie
ponctuelle.

#### ShelterEquipment/Seats

ShelterEquipment/Seats est rempli avec la valeur du tag
[seats](https://wiki.openstreetmap.org/wiki/Key:seats) (ou à défaut
[capacity](https://wiki.openstreetmap.org/wiki/FR:Key:capacity) ou
[capacity:seats](https://wiki.openstreetmap.org/wiki/Key:capacity:seats)).

#### ShelterEquipment/Width

L'élément ShelterEquipment/Width est rempli avec valeur du tag
[width](https://wiki.openstreetmap.org/wiki/FR:Key:width), en mètres
arrondis au cm.

Si le tag est absent et que l'objet OSM est une
![area] zone, la plus grande longueur de la zone
peut être utilisée à la place.

#### ShelterEquipment/BackRest

SeatingEquipment/BackRest est rempli avec la valeur du tag
[backrest](https://wiki.openstreetmap.org/wiki/FR%3AKey%3Abackrest) :

- true si backrest=yes

- false si backrest=no

- non renseigné sinon

### SanitaryEquipment

Les objets
![node]
![area] OSM avec les attributs suivants sont
convertis en SanitaryEquipment :

- [amenity=toilets](https://wiki.openstreetmap.org/wiki/FR:Tag:amenity=toilets)

- [amenity=shower](https://wiki.openstreetmap.org/wiki/Tag:amenity=shower)

- [toilets=yes](https://wiki.openstreetmap.org/wiki/FR:Key:toilets)

Une transformation géométrique est nécessaire pour les
![area] polygones afin d'obtenir une géométrie
ponctuelle.

#### SanitaryEquipment/AccessibilityAssessment

**ValidityCondition/Description** est construit en contaténant le
contenu des tags suivants, avec un séparateur " - " :

- [wheelchair:description=\*](https://wiki.openstreetmap.org/wiki/FR:Key:wheelchair:description)

- [blind:description](https://wiki.openstreetmap.org/wiki/Disabilitydescription)

- [deaf:description](https://wiki.openstreetmap.org/wiki/Disabilitydescription)

**AccessibilityLimitation/WheelchairAccess** est renseigné avec le tag
[wheelchair](https://wiki.openstreetmap.org/wiki/FR:Key:wheelchair) :

- true si wheelchair=yes

- false si wheelchair=no

- partial si wheelchair=limited/bad

- other si wheelchair=\* a une autre valeur

- non renseigné si le tag est absent

#### SanitaryEquipment/SanitaryFacilityList

L'élément SanitaryEquipment/SanitaryFacilityList comprend une liste de
clefs séparées par des espaces :

- toilet est toujours présente

- wheelchairAccessToilet est présente si
    [wheelchair=yes](https://wiki.openstreetmap.org/wiki/FR:Key:wheelchair)

- shower est présente si
    [amenity=shower](https://wiki.openstreetmap.org/wiki/Tag:amenity=shower)
    ou si [shower!=no](https://wiki.openstreetmap.org/wiki/Key%3Ashower)

- babyChange est présente si
    [changing_table=yes](https://wiki.openstreetmap.org/wiki/Key%3Achanging_table)

- wheelchairBabyChange est présente si changing_table:wheelchair=yes

#### SanitaryEquipment/FreeToUse

SanitaryEquipment/FreeToUse est construit avec les règles de gestion
suivantes :

- true si [fee=no](https://wiki.openstreetmap.org/wiki/FR%3AKey%3Afee)

- false si fee!=no

- false si
    [charge=\*](https://wiki.openstreetmap.org/wiki/FR%3AKey%3Acharge)

#### SanitaryEquipment/Staffing

SanitaryEquipment/Staffing est construit à partir du tag
[supervised](https://wiki.openstreetmap.org/wiki/Key:supervised) :

- unmanned si supervised=no

- fullTime si supervised=yes

- partTime si supervised a une autre valeur

#### SanitaryEquipment/HandWashing

SanitaryEquipment/HandWashing est rempli avec la valeur du tag
toilets:handwashing :

- true si toilets:handwashing=yes

- false si toilets:handwashing=no

#### SanitaryEquipment/DrinkingWater

SanitaryEquipment/DrinkingWater est rempli avec la valeur du tag
[drinking_water](https://wiki.openstreetmap.org/wiki/FR%3AKey%3Adrinking_water)
:

- true si drinking_water=yes

- false si drinking_water=no

#### SanitaryEquipment/ToiletsType

SanitaryEquipment/ToiletsType est rempli avec la valeur du tag toilets:position :

- seated si toilets:position=seated
- urinal si toilets:position=urinal
- squat si toilets:position=squat
- seatedAndUrinal si toilets:position comprend à la fois les valeurs seated et urinal

### GeneralSign

Les
![node] nœuds OSM avec
[information!=office/visitor_centre](https://wiki.openstreetmap.org/wiki/Key:information)
sont convertis en GeneralSign.

### SeatingEquipment

Les
![node] nœuds OSM avec les attributs suivants
sont convertis en SeatingEquipment :

- [amenity=bench](https://wiki.openstreetmap.org/wiki/FR:Tag:amenity%3Dbench)

- arrêts de transport avec
    [bench=yes](https://wiki.openstreetmap.org/wiki/FR:Key:bench)

#### SeatingEquipment/Seats

SeatingEquipment/Seats est rempli avec la valeur du tag
[seats](https://wiki.openstreetmap.org/wiki/Key:seats) (ou à défaut
[capacity](https://wiki.openstreetmap.org/wiki/FR:Key:capacity) ou
[capacity:seats](https://wiki.openstreetmap.org/wiki/Key:capacity:seats)).

#### SeatingEquipment/Width

L'élément SeatingEquipment/Width est rempli avec valeur du tag
[width](https://wiki.openstreetmap.org/wiki/FR:Key:width), en mètres
arrondis au cm.

### TicketingEquipment

Les objets
![node]![area] OSM avec les attributs suivants sont
convertis en TicketingEquipment :

- [shop=ticket](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Ashop%3Dticket)
    et
    [tickets:public_transport](https://wiki.openstreetmap.org/wiki/FR%3AKey%3Atickets%3Apublic_transport)

- [vending=public_transport_tickets](https://wiki.openstreetmap.org/wiki/Tag%3Avending%3Dpublic_transport_tickets)
    et
    [amenity=vending_machine](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Aamenity%3Dvending_machine)

Une transformation géométrique est nécessaire pour les
![area] polygones afin d'obtenir une géométrie
ponctuelle.

#### TicketingEquipment/TicketOffice

TicketingEquipment/TicketOffice est rempli avec la valeur fixe true si
[shop=ticket](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Ashop%3Dticket)
et
[tickets:public_transport](https://wiki.openstreetmap.org/wiki/FR%3AKey%3Atickets%3Apublic_transport).

#### TicketingEquipment/TicketMachines

TicketingEquipment/TicketMachines est rempli avec la valeur fixe true si
[vending=public_transport_tickets](https://wiki.openstreetmap.org/wiki/Tag%3Avending%3Dpublic_transport_tickets)
et
[amenity=vending_machine](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Aamenity%3Dvending_machine).

#### TicketingEquipment/InductionLoops

TicketingEquipment/InductionLoops est construit avec les règles de
gestion suivantes :

- true si audio_loop=yes

- true si hearing_loop=yes

- false si audio_loop=no

- false si hearing_loop=no

#### TicketingEquipment/WheelchairSuitable

TicketingEquipment/WheelchairSuitable est est construit à partir du tag
[wheelchair](https://wiki.openstreetmap.org/wiki/FR:Key:wheelchair) :

- true si wheelchair=yes

- false si wheelchair=no

### TicketValidatorEquipment

Les
![node] nœuds OSM avec les attributs suivants
sont convertis en TicketValidatorEquipment :

- [barrier=turnstile](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Abarrier%3Dturnstile)

- [amenity=ticket_validator](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Aamenity%3Dticket_validator)

### LuggageLocker

Les
![node] nœuds OSM avec
[amenity=luggage_locker](https://wiki.openstreetmap.org/wiki/Tag:amenity=luggage_locker)
sont convertis en LuggageLocker.

### TrolleyStandEquipment

Les objets
![node]
![area] OSM avec
[amenity=trolley_bay](https://wiki.openstreetmap.org/wiki/FR:Tag:amenity%3Dtrolley_bay)
sont convertis en TrolleyStandEquipment.

Une transformation géométrique est nécessaire pour les
![area] polygones afin d'obtenir une géométrie
ponctuelle.

#### TrolleyStandEquipment/FreeToUse

TrolleyStandEquipment/FreeToUse est construit avec les règles de gestion
suivantes :

- true si [fee=no](https://wiki.openstreetmap.org/wiki/FR%3AKey%3Afee)

- false si fee!=no

- false si
    [charge=\*](https://wiki.openstreetmap.org/wiki/FR%3AKey%3Acharge)

### PassengerSafetyEquipment

Les
![node] nœuds OSM avec
[emergency=phone](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Aemergency%3Dphone)
sont convertis en PassengerSafetyEquipment.

#### PassengerSafetyEquipment/PublicCode

PassengerSafetyEquipment/PublicCode est rempli avec la valeur du tag OSM
[ref](https://wiki.openstreetmap.org/wiki/FR:Key:ref) ou à défaut
[local_ref](https://wiki.openstreetmap.org/wiki/Key%3Alocal_ref).

#### PassengerSafetyEquipment/SosPanel

PassengerSafetyEquipment/SosPanel est rempli avec la valeur fixe true.

### RubbishDisposalEquipment

Les
![node] nœuds OSM avec les attributs suivants
sont convertis en RubbishDisposalEquipment :

- [amenity=waste_basket](https://wiki.openstreetmap.org/wiki/FR:Tag:amenity%3Dwaste_basket)

- arrêts de transport avec
    [bin=yes](https://wiki.openstreetmap.org/wiki/Key:bin)

### CommunicationService

Les
![node] nœuds OSM avec les attributs
[amenity=post_box](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Aamenity%3Dpost_box)
sont convertis en CommunicationService.

#### CommunicationService/PublicCode

CommunicationService/PublicCode est rempli avec la valeur du tag
[ref](https://wiki.openstreetmap.org/wiki/FR%3AKey%3Aref).

#### CommunicationService/ServiceList

CommunicationService/ServiceList est rempli avec la valeur fixe postbox.

### AssistanceService

Les objets
![node]
![area] OSM avec les attributs suivants sont
convertis en AssistanceService :

- [amenity=reception_desk](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Aamenity%3Dreception_desk)

- [tourism=information](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Atourism%3Dinformation)
    et
    [information=office/visitor_centre](https://wiki.openstreetmap.org/wiki/FR%3AKey%3Ainformation)

Une transformation géométrique est nécessaire pour les
![area] polygones afin d'obtenir une géométrie
ponctuelle.

#### AssistanceService/AssistanceFacilityList

AssistanceService/AssistanceFacilityList est rempli avec la valeur fixe
information.

#### AssistanceService/Staffing

AssistanceService/Staffing est construit à partir du tag
[supervised](https://wiki.openstreetmap.org/wiki/Key:supervised) :

- unmanned si supervised=no

- fullTime si supervised=yes

- partTime si supervised a une autre valeur
