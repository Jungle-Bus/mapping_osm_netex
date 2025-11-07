# Fichier parking.xml

[node]: ../img/node.png
[area]: ../img/area.png

Cette partie de la documentation explique comment générer le fichier `parking.xml` de l'export NeTEx, qui contient les informations sur les places de stationnement.

## ParkingBay

Dans le cadre de cette documentation, on ne traitera que les places de stationnement réservées aux personnes à mobilité réduite, bien que NeTEx et OpenStreetMap permettent de représenter d'autres types de places de stationnement.

### Sélection

Les objets
![node]
![area] OSM avec les attributs suivants sont
convertis en ParkingBay :

- [amenity=parking_space](https://wiki.openstreetmap.org/wiki/FR:Tag:amenity%3Dparking_space)
    et
    [parking_space=disabled](https://wiki.openstreetmap.org/wiki/FR:Key:parking_space)

- [amenity=parking_space](https://wiki.openstreetmap.org/wiki/FR:Tag:amenity%3Dparking_space)
    et
    [capacity:disabled!=0](https://wiki.openstreetmap.org/wiki/FR:Key:capacity)

- [amenity=parking_space](https://wiki.openstreetmap.org/wiki/FR:Tag:amenity%3Dparking_space)
    et
    [wheelchair=designated](https://wiki.openstreetmap.org/wiki/FR:Key:wheelchair)

Puisqu'il s'agit de place de stationnement PMR, les attributs NeTEx
suivants ont une valeur fixe :

- PublicUse : disabledPublicOnly

- ParkingVehicleType: car

### Conversion des attributs

#### ParkingBay/Centroid/Location

La géométrie de l'objet OSM est exportée sous forme de point.

#### ParkingBay/AccessibilityAssessment

**ValidityCondition/Description** est construit en contaténant le
contenu des tags suivants, avec un séparateur " - " :

- [wheelchair:description=\*](https://wiki.openstreetmap.org/wiki/FR:Key:wheelchair:description)

- [blind:description](https://wiki.openstreetmap.org/wiki/Disabilitydescription)

- [deaf:description](https://wiki.openstreetmap.org/wiki/Disabilitydescription)

**AccessibilityLimitation/WheelchairAccess** est renseigné les règles de
gestion suivantes, dans l'ordre :

- false si width < 3,3 m

- false si parking_space:width < 3,3 m

- true si wheelchair=yes

- false si wheelchair=no

- partial si wheelchair=limited/bad

- other si wheelchair=\* a une autre valeur

- non renseigné sinon

**AccessibilityLimitation/StepFreeAccess** vaut true si
[wheelchair=yes](https://wiki.openstreetmap.org/wiki/FR:Key:wheelchair),
et est non renseigné sinon.

#### ParkingBay/PublicUse

cf [§ sélection](#selection)

#### ParkingBay/Lighting

L'élément ParkingBay/Lighting est renseigné à partir du tag
[lit](https://wiki.openstreetmap.org/wiki/FR:Key:lit) :

- unlit si lit=no

- wellLit si lit a une autre valeur, sauf cas particulier

- non renseigné si le tag est absent

Cas particulier : si le tag
[lit:perceived](https://wiki.openstreetmap.org/wiki/FR:Key:lit:perceived)
est renseigné et qu'il a une valeur différente de
good/daylike/none/minimal, alors l'élément ParkingBay/Lighting aura la
valeur poorlyLit.

#### ParkingBay/ParkingVehicleType

cf [§ sélection](#selection)

#### ParkingBay/BayGeometry

L'élément ParkingBay/BayGeometry est rempli avec la valeur du tag
[orientation](https://wiki.openstreetmap.org/wiki/Key:orientation#In_context_of_street_parking) :

- orthogonal si orientation=perpendicular
- angled si orientation=diagonal
- parallel si orientation=parallel
- other si orientation a une autre valeur
- non renseigné si le tag est absent

#### ParkingBay/ParkingVisibility

L'élément ParkingBay/ParkingVisibility est rempli à partir du tag [markings](https://wiki.openstreetmap.org/wiki/FR:Key:markings) :

- unmarked si markings=no
- signageOnly si markings=traffic_sign
- demarcated si markings~traffic_sign et markings~pictograms
- other si markings a une autre valeur
- non renseigné si le tag est absent

#### ParkingBay/Length

L'élément ParkingBay/Length est rempli avec valeur du tag
[length](https://wiki.openstreetmap.org/wiki/FR:Key:length) ou à défaut
[parking_space:length](https://wiki.openstreetmap.org/wiki/Key:parking_space:length),
en mètres arrondis au cm.

Si le tag est absent et que l'objet OSM est une
![area] zone avec
[capacity=1](https://wiki.openstreetmap.org/wiki/FR:Key:capacity) ou le
tag capacity n'est pas renseigné ou
[capacity:disabled=1](https://wiki.openstreetmap.org/w/index.php?title=FR:Key:capacity:disabled)
ou le tag capacity:disabled n'est pas renseigné, la plus grande
longueur de la zone peut être utilisée à la place.

#### ParkingBay/Width

L'élément ParkingBay/Width est rempli avec valeur du tag
[width](https://wiki.openstreetmap.org/wiki/FR:Key:width) ou à défaut
[parking_space:width](https://wiki.openstreetmap.org/wiki/Key:parking_space:width),
en mètres arrondis au cm.

Si le tag est absent et que l'objet OSM est une
![area] zone avec
[capacity=1](https://wiki.openstreetmap.org/wiki/FR:Key:capacity) ou le
tag capacity n'est pas renseigné ou
[capacity:disabled=1](https://wiki.openstreetmap.org/w/index.php?title=FR:Key:capacity:disabled)
ou le tag capacity:disabled n'est pas renseigné, la plus grande
longueur de la zone peut être utilisée à la place.

#### ParkingBay/RechargingAvailable

L'élément ParkingBay/RechargingAvailable est rempli à partir de la
valeur du tag
[capacity:charging](https://wiki.openstreetmap.org/wiki/Key:capacity:charging) :

- true si capacity:charging!=0
- false si capacity:charging=0
- non renseigné si le tag est absent
