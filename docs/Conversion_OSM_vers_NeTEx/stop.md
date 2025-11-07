# Fichier stop.xml

[node]: ../img/node.png
[way]: ../img/way.png
[area]: ../img/area.png
[relation]: ../img/relation.png

Cette partie de la documentation explique comment générer le fichier `stop.xml` de l'export NeTEx, qui contient les informations sur les arrêts, les quais, etc.

## Quay

### Sélection

Les
![node]![way]![area] objets OSM représentant des arrêts ou
quais de transport sont susceptibles d'être convertis en Quay. Les tags
utilisés pour la sélection déterminent également l'attribut NeTEx
TransportMode.

Les
![node] nœuds suivants sont convertis en Quay
avec TransportMode valant bus :

- [highway=bus_stop](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Ahighway%3Dbus_stop)

- membre d'une
    ![relation] relation avec
    [route=bus](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Aroute%3Dbus)
    avec le rôle platform

Les
![node] nœuds suivants sont convertis en Quay
avec TransportMode valant trolleyBus :

- [highway=bus_stop](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Ahighway%3Dbus_stop)
    et
    [trolleybus=yes](https://wiki.openstreetmap.org/wiki/Key:trolleybus)

- membre d'une
    ![relation] relation avec
    [route=trolleybus](https://wiki.openstreetmap.org/wiki/Tag%3Aroute%3Dtrolleybus)
    avec le rôle platform

Les
![node]![way]![area] objets OSM membres d'une
![relation] relation avec
[route=ferry](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Aroute%3Dferry)
avec le rôle platform sont convertis en Quay avec TransportMode valant
water.

#### 

Les
![node]![way]![area] objets OSM suivants sont convertis en
Quay avec TransportMode valant tram :

- [railway=platform](https://wiki.openstreetmap.org/wiki/Tag:railway%3Dplatform)
    et [tram=yes](https://wiki.openstreetmap.org/wiki/Key:tram)

- [railway=platform](https://wiki.openstreetmap.org/wiki/Tag:railway%3Dplatform)
    et
    [light_rail=yes](https://wiki.openstreetmap.org/wiki/Key:light_rail)

- membre d'une
    ![relation] relation avec
    [route=tram](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Aroute%3Dtram)
    avec le rôle platform

- membre d'une
    ![relation] relation avec
    [route=light_rail](https://wiki.openstreetmap.org/wiki/Tag%3Aroute%3Dlight_rail)
    avec le rôle platform

Les
![way]![area] objets OSM suivants sont convertis en
Quay avec TransportMode valant rail :

- [railway=platform](https://wiki.openstreetmap.org/wiki/Tag:railway%3Dplatform)
    et [train=yes](https://wiki.openstreetmap.org/wiki/Key:train)

- membre d'une
    ![relation] relation avec
    [route=train](https://wiki.openstreetmap.org/wiki/Tag%3Aroute%3Dtrain)
    avec le rôle platform

Les
![way]![area] objets OSM suivants sont convertis en
Quay avec TransportMode valant metro :

- [railway=platform](https://wiki.openstreetmap.org/wiki/Tag:railway%3Dplatform)
    et [subway=yes](https://wiki.openstreetmap.org/wiki/Key:subway)

- [railway=platform](https://wiki.openstreetmap.org/wiki/Tag:railway%3Dplatform)
    et [monorail=yes](https://wiki.openstreetmap.org/wiki/Key:monorail)

- membre d'une
    ![relation] relation avec
    [route=subway](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Aroute%3Dsubway)
    avec le rôle platform

- membre d'une
    ![relation] relation avec
    [route=monorail](https://wiki.openstreetmap.org/wiki/Tag%3Aroute%3Dmonorail)
    avec le rôle platform

Les
![node]![way]![area] objets OSM membres d'une
![relation] relation avec
[route=funicular](https://wiki.openstreetmap.org/wiki/Tag%3Aroute%3Dfunicular)
avec le rôle platform sont convertis en Quay avec TransportMode valant
funicular.

Les
![node]![way]![area] objets OSM membres d'une
![relation] relation avec route=aerialway avec le
rôle platform sont convertis en Quay avec TransportMode valant cableway.

Dans tous les cas, une transformation géométrique est nécessaire pour
les
![way] chemins afin d'obtenir une géométrie
ponctuelle ou polygonale.

### Conversion des attributs

#### Quay/Name

Quay/Name est rempli avec la valeur du tag
[name](https://wiki.openstreetmap.org/wiki/FR:Key:name).

#### Quay/Description

Quay/Description peut être rempli avec la valeur du tag
[description](https://wiki.openstreetmap.org/wiki/FR:Key:description).

#### Quay/Centroid/Location

Quay/Centroid/Location est construit avec la géométrie de l'objet, sous
forme de point.

#### Quay/Polygon

Quay/Polygon est construit avec la géométrie polygonale de l'objet
lorsqu'elle existe.

#### Quay/AccessibilityAssessment

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

**AccessibilityLimitation/VisualSignsAvailable** est renseigné avec les
règles de gestion suivantes :

- true si
    [departures_board!=no](https://wiki.openstreetmap.org/wiki/Key%3Adepartures_board)
    ou
    [passenger_information_display!=no](https://wiki.openstreetmap.org/wiki/Key:passenger_information_display)

- false si
    [departures_board=no](https://wiki.openstreetmap.org/wiki/Key%3Adepartures_board)
    ou
    [passenger_information_display=no](https://wiki.openstreetmap.org/wiki/Key:passenger_information_display)

**AccessibilityLimitation/AudibleSignalsAvailable** est renseigné avec
les règles de gestion suivantes :

- true si
    [departures_board:speech_output!=no](https://wiki.openstreetmap.org/wiki/Key%3Adepartures_board%3Aspeech_output)
    ou
    [passenger_information_display:speech_output!=no](https://wiki.openstreetmap.org/wiki/Key:passenger_information_display:speech_output)
    ou
    [announcement!=no](https://wiki.openstreetmap.org/wiki/Key%3Aannouncement)
    ou
    [speech_output!=no](https://wiki.openstreetmap.org/wiki/Key%3Aspeech_output)

- false si
    [departures_board:speech_output=no](https://wiki.openstreetmap.org/wiki/Key%3Adepartures_board%3Aspeech_output)
    ou
    [passenger_information_display:speech_output=no](https://wiki.openstreetmap.org/wiki/Key:passenger_information_display:speech_output)
    ou
    [announcement=no](https://wiki.openstreetmap.org/wiki/Key%3Aannouncement)
    ou
    [speech_output=no](https://wiki.openstreetmap.org/wiki/Key%3Aspeech_output)

#### Quay/Covered

Quay/Covered vaut covered si
[covered=yes](https://wiki.openstreetmap.org/wiki/FR:Key:covered).

#### Quay/Lighting

Quay/Lighting est renseigné à partir du tag
[lit](https://wiki.openstreetmap.org/wiki/FR:Key:lit) :

- unlit si lit=no

- wellLit si lit a une autre valeur, sauf cas particulier

- non renseigné si le tag est absent

Cas particulier : si le tag
[lit:perceived](https://wiki.openstreetmap.org/wiki/FR:Key:lit:perceived)
est renseigné et qu'il a une valeur différente de
good/daylike/none/minimal, alors l'élément Quay/Lighting aura la valeur
poorlyLit.

#### Quay/SiteRef

Quay/SiteRef référence l'objet NeTEx StopPlace qui comprend le quai ou
l'arrêt (voir le [§ sur la sélection des StopPlace](#selection_1)).

#### Quay/equipmentPlaces

Quay/equipmentPlaces contient une liste de références vers les
équipements du quai ainsi que leurs positions respectives.

Les équipements concernés sont :

- ceux créés à partir des objets amenity=\* situés à l'intérieur de
    la
    ![area] zone du Quay, dans le cas d'un quai
    polygonal

- générés à partir des attributs shelter/bench/bin du quai ponctuel

Voir le § sur les équipements ponctuels

#### Quay/TransportMode

cf [§ sélection](#selection)

#### Quay/PublicCode

Quay/PublicCode est rempli avec la valeur du tag OSM
[ref](https://wiki.openstreetmap.org/wiki/FR:Key:ref) ou à défaut
[local_ref](https://wiki.openstreetmap.org/wiki/Key%3Alocal_ref).

## StopPlace

### Sélection

Les
![node]![area] objets OSM représentant des gares ou
stations de transport sont susceptibles d'être convertis en StopPlace.
Les tags utilisés pour la sélection déterminent également les attributs
NeTEx TransportMode et StopPlaceType.

Les
![node]![area] objets OSM suivants sont convertis en
StopPlace avec TransportMode valant bus et StopPlaceType valant
busStation :

- [amenity=bus_station](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Aamenity%3Dbus_station)

- [public_transport=station](https://wiki.openstreetmap.org/wiki/FR:Tag:public_transport%3Dstation)
    et station=bus

Les
![node]![area] objets OSM suivants sont convertis en
StopPlace avec TransportMode valant metro et StopPlaceType valant
metroStation :

- [railway=station](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Arailway%3Dstation)
    et
    [station=subway/monorail](https://wiki.openstreetmap.org/wiki/Key%3Astation)

- [railway=station](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Arailway%3Dstation)
    et [subway=yes](https://wiki.openstreetmap.org/wiki/Key:subway)

- [railway=station](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Arailway%3Dstation)
    et [monorail=yes](https://wiki.openstreetmap.org/wiki/Key:monorail)

- [public_transport=station](https://wiki.openstreetmap.org/wiki/FR:Tag:public_transport%3Dstation)
    et
    [station=subway/monorail](https://wiki.openstreetmap.org/wiki/Key%3Astation)

Les
![node]![area] objets OSM suivants sont convertis en
StopPlace avec TransportMode valant tram et StopPlaceType valant
tramStation :

- [railway=station](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Arailway%3Dstation)
    et
    [station=tram/light_rail](https://wiki.openstreetmap.org/wiki/Key%3Astation)

- [railway=station](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Arailway%3Dstation)
    et [tram=yes](https://wiki.openstreetmap.org/wiki/Key:tram)

- [railway=station](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Arailway%3Dstation)
    et
    [light_rail=yes](https://wiki.openstreetmap.org/wiki/Key:light_rail)

- [public_transport=station](https://wiki.openstreetmap.org/wiki/FR:Tag:public_transport%3Dstation)
    et
    [station=tram/light_rail](https://wiki.openstreetmap.org/wiki/Key%3Astation)

Les
![node]![area] objets OSM suivants sont convertis en
StopPlace avec TransportMode valant rail et StopPlaceType valant
railStation :

- [railway=station/halt](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Arailway%3Dstation)
    et
    [station=train](https://wiki.openstreetmap.org/wiki/Key%3Astation)

- [railway=station](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Arailway%3Dstation)
    et [train=yes](https://wiki.openstreetmap.org/wiki/Key:train)

- [public_transport=station](https://wiki.openstreetmap.org/wiki/FR:Tag:public_transport%3Dstation)
    et
    [station=train](https://wiki.openstreetmap.org/wiki/Key%3Astation)

Les
![node]![area] objets OSM avec
[public_transport=station](https://wiki.openstreetmap.org/wiki/FR:Tag:public_transport%3Dstation)
et [station=ferry](https://wiki.openstreetmap.org/wiki/Key%3Astation)
sont convertis en StopPlace avec TransportMode valant water et
StopPlaceType valant ferryPort.

Les
![node]![area] objets OSM avec
[public_transport=station](https://wiki.openstreetmap.org/wiki/FR:Tag:public_transport%3Dstation)
et
[station=aerialway](https://wiki.openstreetmap.org/wiki/Key%3Astation)
sont convertis en StopPlace avec TransportMode valant cableway et
StopPlaceType valant liftStation.

Les
![node]![area] objets OSM avec
[aeroway=aerodrome](https://wiki.openstreetmap.org/wiki/Tag%3Aaeroway%3Daerodrome)
et
[aerodrome:type=international/regional](https://wiki.openstreetmap.org/wiki/Key%3Aaerodrome%3Atype)
sont convertis en StopPlace avec TransportMode valant air et
StopPlaceType valant airport.

### Conversion des attributs

#### StopPlace/Name

StopPlace/Name est rempli avec la valeur du tag
[name](https://wiki.openstreetmap.org/wiki/FR:Key:name).

#### StopPlace/Description

StopPlace/Description peut être rempli avec la valeur du tag
[description](https://wiki.openstreetmap.org/wiki/FR:Key:description).

#### StopPlace/Centroid/Location

StopPlace/Centroid/Location est construit avec la géométrie de l'objet,
sous forme de point.

#### StopPlace/Polygon

Quay/Polygon est construit avec la géométrie polygonale de l'objet
lorsqu'elle existe.

#### StopPlace/placeTypes

StopPlace/placeTypes contient une référence vers un TypeOfPlaceRef :

- monomodalStopPlace si le StopPlace contient uniquement des quais de
    même mode

- multimodalStopPlace dans le cas contraire.

#### StopPlace/AccessibilityAssessment

**ValidityCondition/Description** est construit en contaténant le
contenu des tags suivants, avec un séparateur " - " :

- [wheelchair:description=\*](https://wiki.openstreetmap.org/wiki/FR:Key:wheelchair:description)

- [blind:description](https://wiki.openstreetmap.org/wiki/Disabilitydescription)

- [deaf:description](https://wiki.openstreetmap.org/wiki/Disabilitydescription)

**AccessibilityLimitation/WheelchairAccess** est renseigné avec le tag
[wheelchair](https://wiki.openstreetmap.org/wiki/FR:Key:wheelchair) :

- true si wheelchair=yes

- false si wheelchair=no

- false s'il y a des quais avec wheelchair=no à l'intérieur de la gare
    ou station

- partial si wheelchair=limited/bad

- other si wheelchair=\* a une autre valeur

- non renseigné si le tag est absent

**AccessibilityLimitation/VisualSignsAvailable**

- true si
    [departures_board!=no](https://wiki.openstreetmap.org/wiki/Key%3Adepartures_board)
    ou
    [passenger_information_display!=no](https://wiki.openstreetmap.org/wiki/Key:passenger_information_display),
    ou s'il y a au moins un objet avec ces tags à l'intérieur de la
    gare ou station

- false si
    [departures_board=no](https://wiki.openstreetmap.org/wiki/Key%3Adepartures_board)
    ou
    [passenger_information_display=no](https://wiki.openstreetmap.org/wiki/Key:passenger_information_display),
    ou s'il n'y a aucun objet avec
    [departures_board!=no](https://wiki.openstreetmap.org/wiki/Key%3Adepartures_board)
    ou
    [passenger_information_display!=no](https://wiki.openstreetmap.org/wiki/Key:passenger_information_display)
    à l'intérieur de la gare ou station

**AccessibilityLimitation/AudibleSignalsAvailable**

- true si
    [departures_board:speech_output!=no](https://wiki.openstreetmap.org/wiki/Key%3Adepartures_board%3Aspeech_output)
    ou
    [passenger_information_display:speech_output!=no](https://wiki.openstreetmap.org/wiki/Key:passenger_information_display:speech_output)
    ou
    [announcement!=no](https://wiki.openstreetmap.org/wiki/Key%3Aannouncement),
    ou s'il y a au moins un objet avec ces tags à l'intérieur de la
    gare ou station

- false si
    [departures_board:speech_output=no](https://wiki.openstreetmap.org/wiki/Key%3Adepartures_board%3Aspeech_output)
    ou
    [passenger_information_display:speech_output=no](https://wiki.openstreetmap.org/wiki/Key:passenger_information_display:speech_output)
    ou
    [announcement=no](https://wiki.openstreetmap.org/wiki/Key%3Aannouncement),
    ou s'il n'y a aucun objet avec
    [departures_board:speech_output!=no](https://wiki.openstreetmap.org/wiki/Key%3Adepartures_board%3Aspeech_output)
    ou
    [passenger_information_display:speech_output!=no](https://wiki.openstreetmap.org/wiki/Key:passenger_information_display:speech_output)
    ou
    [announcement!=no](https://wiki.openstreetmap.org/wiki/Key%3Aannouncement)
    à l'intérieur de la gare ou station

#### StopPlace/Lighting

StopPlace/Lighting est renseigné à partir du tag
[lit](https://wiki.openstreetmap.org/wiki/FR:Key:lit) :

- unlit si lit=no

- wellLit si lit a une autre valeur, sauf cas particulier

- non renseigné si le tag est absent

Cas particulier : si le tag
[lit:perceived](https://wiki.openstreetmap.org/wiki/FR:Key:lit:perceived)
est renseigné et qu'il a une valeur différente de
good/daylike/none/minimal, alors l'élément StopPlace/Lighting aura la
valeur poorlyLit.

#### StopPlace/facilities/SiteFacilitySet/AccessibilityInfoFacilityList

AccessibilityInfoFacilityList est rempli avec une liste de valeurs parmi
les suivantes :

- audioInformation si
    [departures_board:speech_output!=no](https://wiki.openstreetmap.org/wiki/Key%3Adepartures_board%3Aspeech_output)
    ou
    [passenger_information_display:speech_output!=no](https://wiki.openstreetmap.org/wiki/Key:passenger_information_display:speech_output)
    ou
    [announcement!=no](https://wiki.openstreetmap.org/wiki/Key%3Aannouncement),
    ou s'il y a au moins un objet avec ces tags à l'intérieur de la
    gare ou station

- audioForHearingImpaired si les conditions de audioInformation sont
    réunies et qu'il y a au moins un objet avec audio_loop=yes ou
    hearing_loop=yes à l'intérieur de la gare ou station

- visualDisplays si
    [departures_board!=no/timetable](https://wiki.openstreetmap.org/wiki/Key%3Adepartures_board)
    ou
    [passenger_information_display!=no](https://wiki.openstreetmap.org/wiki/Key:passenger_information_display),
    ou s'il y a au moins un objet avec ces tags à l'intérieur de la
    gare ou station

- largePrintTimetables si
    [departures_board=timetable](https://wiki.openstreetmap.org/wiki/Key%3Adepartures_board)
    ou s'il y a au moins un objet avec
    [departures_board=timetable](https://wiki.openstreetmap.org/wiki/Key%3Adepartures_board)
    ou avec
    [information=board](https://wiki.openstreetmap.org/wiki/FR:Tag:information%3Dboard)
    et
    [board_type=public_transport](https://wiki.openstreetmap.org/wiki/FR%3AKey%3Aboard_type)
    à l'intérieur de la gare ou station

#### StopPlace/facilities/SiteFacilitySet/AssistanceFacilityList

AssistanceFacilityList est rempli avec une liste de valeurs parmi les
suivantes :

- boardingAssistance si service:SNCF:acces_plus=yes

- information s'il y a au moins un objet avec
    [amenity=reception_desk](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Aamenity%3Dreception_desk)
    à l'intérieur de la gare ou station

#### StopPlace/facilities/SiteFacilitySet/MedicalFacilityList

MedicalFacilityList est rempli avec une liste de valeurs parmi les
suivantes :

- defibrillator s'il y a au moins un objet avec
    [emergency=defibrillator](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Aemergency%3Ddefibrillator)
    à l'intérieur de la gare ou station

#### StopPlace/facilities/SiteFacilitySet/MobilityFacilityList

MobilityFacilityList est rempli avec une liste de valeurs parmi les
suivantes :

- stepFreeAccess s'il n'y a ni
    [highway=steps](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Ahighway%3Dsteps),
    ni
    [barrier=kerb](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Abarrier%3Dkerb)
    à l'intérieur de la gare ou station

- suitableForWheelchairs si
    [wheelchair=yes](https://wiki.openstreetmap.org/wiki/FR:Key:wheelchair)

- boardingAssistance si service:SNCF:acces_plus=yes

- tactilePlatformEdges si tous les quais ou arrêts à l'intérieur de
    la gare ou station ont un tag
    [tactile_paving!=no](https://wiki.openstreetmap.org/wiki/FR%3AKey%3Atactile_paving)

- tactileGuidingStrips s'il y a au moins un
    [highway=footway](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Ahighway%3Dfootway)
    avec
    [tactile_paving!=no](https://wiki.openstreetmap.org/wiki/FR%3AKey%3Atactile_paving)
    à l'intérieur de la gare ou station

#### StopPlace/facilities/SiteFacilitySet/PassengerCommsFacilityList

PassengerCommsFacilityList est rempli avec une liste de valeurs parmi
les suivantes :

- freeWifi si
    [internet_access=wlan/yes/wifi](https://wiki.openstreetmap.org/wiki/Key%3Ainternet_access)
    et
    [internet_access:fee=no](https://wiki.openstreetmap.org/wiki/Key%3Ainternet_access%3Afee),
    ou s'il y a au moins un objet avec ces tags à l'intérieur de la
    gare ou station

- publicWifi si internet_access=wlan/yes/wifi, ou s'il y a au moins
    un objet avec ce tag à l'intérieur de la gare ou station

- internet si internet_access=wlan/yes/wifi, ou s'il y a au moins un
    objet avec ce tag à l'intérieur de la gare ou station

- postBox s'il y a au moins un objet avec
    [amenity=post_box](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Aamenity%3Dpost_box)
    à l'intérieur de la gare ou station

#### StopPlace/facilities/SiteFacilitySet/PassengerInformationEquipmentList

PassengerInformationEquipmentList est rempli avec une liste de valeurs
parmi les suivantes :

- informationDesk s'il y a au moins un objet avec
    [amenity=reception_desk](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Aamenity%3Dreception_desk)
    à l'intérieur de la gare ou station

- realTimeDepartures si
    [departures_board=realtime](https://wiki.openstreetmap.org/wiki/FR%3AKey%3Adepartures_board)
    ou s'il y a au moins un objet avec ce tag à l'intérieur de la gare
    ou station

#### StopPlace/facilities/SiteFacilitySet/SanitaryFacilityList

SanitaryFacilityList est rempli avec une liste de valeurs parmi les
suivantes :

- none si toilets=no et qu'il n'y a pas d'objet avec
    amenity=toilets/shower à l'intérieur de la gare ou station

- toilet si toilets=yes ou s'il y a au moins un objet avec
    [amenity=toilets](https://wiki.openstreetmap.org/wiki/FR:Tag:amenity=toilets)
    à l'intérieur de la gare ou station

- wheelChairAccessToilet si
    [toilets:wheelchair=yes](https://wiki.openstreetmap.org/wiki/FR:Key:toilets:wheelchair)
    ou s'il y a au moins un objet avec amenity=toilets et
    [wheelchair=yes](https://wiki.openstreetmap.org/wiki/FR:Key:wheelchair)
    à l'intérieur de la gare ou station

- shower si
    [shower!=no](https://wiki.openstreetmap.org/wiki/Key%3Ashower) ou
    s'il y a au moins un objet avec ce
    [shower!=no](https://wiki.openstreetmap.org/wiki/Key%3Ashower) ou
    [amenity=shower](https://wiki.openstreetmap.org/wiki/Tag:amenity=shower)
    à l'intérieur de la gare ou station

- babyChange s'il y a au moins un objet avec
    [changing_table=yes](https://wiki.openstreetmap.org/wiki/Key%3Achanging_table)
    à l'intérieur de la gare ou station

- wheelchairBabyChange s'il y a au moins un objet avec
    changing_table=yes et changing_table:wheelchair=yes à l'intérieur
    de la gare ou station

#### StopPlace/facilities/SiteFacilitySet/TicketingFacilityList

TicketingFacilityList est rempli avec une liste de valeurs parmi les
suivantes :

- ticketMachines s'il y a au moins un objet avec
    [vending=public_transport_tickets](https://wiki.openstreetmap.org/wiki/Tag%3Avending%3Dpublic_transport_tickets)
    et
    [amenity=vending_machine](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Aamenity%3Dvending_machine)
    à l'intérieur de la gare ou station

- ticketOffice s'il y a au moins un objet avec
    [shop=ticket](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Ashop%3Dticket)
    et
    [tickets:public_transport](https://wiki.openstreetmap.org/wiki/FR%3AKey%3Atickets%3Apublic_transport)
    à l'intérieur de la gare ou station

#### StopPlace/facilities/SiteFacilitySet/AccessFacilityList

AccessFacilityList est rempli avec une liste de valeurs parmi les
suivantes :

- lift s'il y a au moins un objet avec
    [highway=elevator](https://wiki.openstreetmap.org/wiki/FR:Tag:highway%3Delevator)
    à l'intérieur de la gare ou station

- wheelchairLift s'il y a au moins un objet avec
    [highway=elevator](https://wiki.openstreetmap.org/wiki/FR:Tag:highway%3Delevator)
    et
    [wheelchair=yes](https://wiki.openstreetmap.org/wiki/FR:Key:wheelchair)
    à l'intérieur de la gare ou station

- escalator s'il y a au moins un objet avec
    [conveying!=no](https://wiki.openstreetmap.org/wiki/FR:Key:conveying)
    et
    [highway=steps](https://wiki.openstreetmap.org/wiki/FR:Tag:highway%3Dsteps)
    à l'intérieur de la gare ou station

- travelator s'il y a au moins un objet avec
    [conveying!=no](https://wiki.openstreetmap.org/wiki/FR:Key:conveying)
    et
    [highway=footway](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Ahighway%3Dfootway)
    à l'intérieur de la gare ou station

- ramp s'il y a au moins un objet avec
    [incline](https://wiki.openstreetmap.org/wiki/FR:Key:incline) à
    l'intérieur de la gare ou station

- steps s'il y a au moins un objet avec
    [kerb=raised](https://wiki.openstreetmap.org/wiki/FR:Key:kerb) à
    l'intérieur de la gare ou station

- stairs s'il y a au moins un objet avec
    [highway=steps](https://wiki.openstreetmap.org/wiki/FR:Tag:highway%3Dsteps)
    à l'intérieur de la gare ou station

- validator s'il y a au moins un objet avec
    [barrier=turnstile](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Abarrier%3Dturnstile)
    ou
    [amenity=ticket_validator](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Aamenity%3Dticket_validator)
    à l'intérieur de la gare ou station

#### StopPlace/facilities/SiteFacilitySet/EmergencyServiceList

EmergencyServiceList est rempli avec une liste de valeurs parmi les
suivantes :

- police s'il y a au moins un objet avec
    [amenity=police](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Aamenity%3Dpolice)
    à l'intérieur de la gare ou station

- fire s'il y a au moins un objet avec
    [amenity=fire_station](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Aamenity%3Dfire_station)
    à l'intérieur de la gare ou station

- sosPoint s'il y a au moins un objet avec
    [emergency=phone](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Aemergency%3Dphone)
    à l'intérieur de la gare ou station

#### StopPlace/facilities/SiteFacilitySet/LuggageLockerFacilityList

LuggageLockerFacilityList est rempli avec une liste de valeurs parmi les
suivantes :

- lockers s'il y a au moins un objet avec
    [amenity=luggage_locker](https://wiki.openstreetmap.org/wiki/Tag%3Aamenity%3Dluggage_locker)
    à l'intérieur de la gare ou station

#### StopPlace/facilities/SiteFacilitySet/LuggageServiceFacilityList

LuggageServiceFacilityList est rempli avec une liste de valeurs parmi
les suivantes :

- freeTrolleys s'il y a au moins un objet avec
    [amenity=trolley_bay](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Aamenity%3Dtrolley_bay)
    et [fee=no](https://wiki.openstreetmap.org/wiki/FR%3AKey%3Afee) à
    l'intérieur de la gare ou station

- paidTrolleys s'il y a au moins un objet avec amenity=trolley_bay et
    fee!=no à l'intérieur de la gare ou station

- other s'il y a au moins un objet avec amenity=trolley_bay et fee
    non renseigné à l'intérieur de la gare ou station

#### StopPlace/facilities/SiteFacilitySet/ParkingFacilityList

ParkingFacilityList est rempli avec une liste de valeurs parmi les
suivantes :

- carPark s'il y a un objet OSM avec
    [amenity=parking](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Aamenity%3Dparking)
    à moins de 100 mètres de la gare ou station

- cyclePark s'il y a un objet OSM avec
    [amenity=bicycle_parking](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Aamenity%3Dbicycle_parking)
    à moins de 100 mètres de la gare ou station

#### StopPlace/facilities/SiteFacilitySet/Staffing

Staffing est construit à partir du tag
[supervised](https://wiki.openstreetmap.org/wiki/Key:supervised) avec
une liste de valeurs parmi les suivantes :

- unmanned si supervised=no ou si tous les objets avec shop=tickets ou
    amenity=reception_desk ont ce tag à l'intérieur de la gare ou
    station

- fullTime si supervised=yes ou s'il y a au moins un objet avec
    shop=tickets ou amenity=reception_desk avec ce tag à l'intérieur de
    la gare ou station

- partTime si supervised a une autre valeur ou s'il y a au moins un
    objet avec shop=tickets ou amenity=reception_desk avec ce tag à
    l'intérieur de la gare ou station

#### StopPlace/entrances

StopPlace/entrances contient une liste de références vers les entrées de
l'objet StopPlace.

#### StopPlace/equipmentPlaces

StopPlace/equipmentPlaces contient une liste de références vers les
équipements ponctuels du StopPlace ainsi que leurs positions
respectives.

#### StopPlace/placeEquipments

StopPlace/equipmentPlaces contient une liste de références vers les
équipements de cheminement du StopPlace.

#### StopPlace/TransportMode

cf [§ sélection](#selection_1)

#### StopPlace/StopPlaceType

cf [§ sélection](#selection_1)

#### StopPlace/quays

StopPlace/quays contient une liste de références vers les Quay que
l'objet StopPlace englobe.

#### StopPlace/pathLinks

StopPlace/pathLinks contient une liste de références vers les
SitePathLink que l'objet StopPlace englobe

## Entrance

### Sélection

Les
![node] nœuds avec
[railway=subway_entrance](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Arailway%3Dsubway_entrance)
ou [railway=train_station
entrance](https://wiki.openstreetmap.org/wiki/Tag%3Arailway%3Dtrain_station_entrance)
sont convertis en Entrance.

### Conversion des attributs

#### Entrance/Description

Entrance/Description peut être rempli avec la valeur du tag
[description](https://wiki.openstreetmap.org/wiki/FR:Key:description).

#### Entrance/Centroid/Location

Entrance/Centroid/Location est construit avec la géométrie de l'objet,
sous forme de point.

#### Entrance/PostalAddress/AddressLine1

Entrance/PostalAddress/AddressLine1 est construit de la manière suivante
:

- la concaténation des valeurs des tags
    [addr:housenumber](https://wiki.openstreetmap.org/wiki/FR:Key:addr:*)
    et [addr:street](https://wiki.openstreetmap.org/wiki/FR:Key:addr:*),
    avec un espace pour séparateur

- la concaténation des valeurs des tags
    [contact:housenumber](https://wiki.openstreetmap.org/wiki/Key:contact:*)
    et
    [contact:street](https://wiki.openstreetmap.org/wiki/Key:contact:*),
    avec un espace pour séparateur

- la concaténation des valeurs du tags addr:housenumber et du tag name
    de la
    ![relation] relation
    [associatedStreet](https://wiki.openstreetmap.org/wiki/FR:Relation:associatedStreet)
    dont l'objet fait partie, avec un espace pour séparateur

- la concaténation des valeurs du tags addr:housenumber et du tag name
    de la
    ![relation] relation
    [street](https://wiki.openstreetmap.org/wiki/FR:Relation:street)
    dont l'objet fait partie, avec un espace pour séparateur

Remarque : l'adresse peut également être obtenue par géocodage inverse
à partir des coordonnées de l'objet.

#### Entrance/PostalAddress/PostCode

Entrance/PostalAddress/PostCode est rempli avec la valeur du tag OSM
[addr:postcode](https://wiki.openstreetmap.org/wiki/FR:Key:addr:*) ou
[contact:postcode](https://wiki.openstreetmap.org/wiki/Key:contact:*) à
défaut

Remarque : le code postal peut également être obtenu par géocodage
inverse à partir des coordonnées de l'objet.

#### Entrance/AccessibilityAssessment

**ValidityCondition/Description** est construit en contaténant le
contenu des tags suivants, avec un séparateur " - " :

- [wheelchair:description=\*](https://wiki.openstreetmap.org/wiki/FR:Key:wheelchair:description)

- [blind:description](https://wiki.openstreetmap.org/wiki/Disabilitydescription)

- [deaf:description](https://wiki.openstreetmap.org/wiki/Disabilitydescription)

**AccessibilityLimitation/WheelchairAccess** est renseigné avec les
règles de gestion suivantes, dans l'ordre :

- false si width < 0,8 m ou est_width < 0,8 m ou door:width < 0,8 m

- false si kerb:height > 0,04 m ou wheelchair:step_height > 0,04 m

- true si width (ou est_width ou door:width) ≥ 0,8 m et kerb:height
    (ou wheelchair:step_height) < 0,02 m et door=no

- true si wheelchair=yes

- false si wheelchair=no

- partial si wheelchair=limited/bad

- other si wheelchair=\* a une autre valeur

- non renseigné sinon

**AccessibilityLimitation/StepFreeAccess** est renseigné avec les règles
de gestion suivantes, dans l'ordre :

- true si la valeur du tag
    [kerb:height](https://wiki.openstreetmap.org/wiki/Key:kerb#Kerb_height)
    est inférieure ou égale à 0.02 m

- true si la valeur du tag wheelchair:step_height est inférieure ou
    égale à 0.02 m

- false si la valeur du tag kerb:height est supérieure à 0.04 m

- false si la valeur du tag wheelchair:step_height est supérieure à
    0.04 m

- partial si la valeur du tag kerb:height est inférieure à 0.04 m

- partial si la valeur du tag wheelchair:step_height est inférieure à
    0.04 m

#### Entrance/SiteRef

Entrance/SiteRef référence l'objet StopPlace dont c'est une entrée.

#### Entrance/placeEquipments

Entrance/placeEquipments contient une référence vers
l'EntranceEquipment créé en complément de l'objet Entrance.

#### Entrance/PublicCode

Entrance/PublicCode est rempli avec la valeur du tag OSM
[ref](https://wiki.openstreetmap.org/wiki/FR:Key:ref).

#### Entrance/Label

Entrance/Label est rempli avec la valeur du tag
[name](https://wiki.openstreetmap.org/wiki/FR:Key:name).

#### Entrance/EntranceType

Entrance/EntranceType est rempli avec les règles de gestion suivantes :

- opening si
    [door=no](https://wiki.openstreetmap.org/wiki/FR%3AKey%3Adoor)

- gate si
    [barrier=\*](https://wiki.openstreetmap.org/wiki/FR%3AKey%3Abarrier)

- automaticDoor si
    [automatic_door=yes](https://wiki.openstreetmap.org/wiki/Key%3Aautomatic_door)

- revolvingDoor si
    [door=revolving](https://wiki.openstreetmap.org/wiki/FR%3AKey%3Adoor)

- swingDoor si
    [door=swinging](https://wiki.openstreetmap.org/wiki/FR%3AKey%3Adoor)

- door si
    [door!=no/revolving/swinging](https://wiki.openstreetmap.org/wiki/FR%3AKey%3Adoor)

#### Entrance/IsEntry

Entrance/IsEntry est rempli avec les règles de gestion suivantes :

- false si
    [entrance=emergency](https://wiki.openstreetmap.org/wiki/FR:Key:entrance)

- false si
    [entrance=exit](https://wiki.openstreetmap.org/wiki/Tag:entrance%3Dexit)

- false si membre d'une
    ![relation] relation
    [public_transport=stop_area](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Apublic_transport%3Dstop_area)
    avec le rôle exit_only

- true sinon

#### Entrance/IsExit

Entrance/IsExit est rempli avec les règles de gestion suivantes :

- false si
    [entrance=entrance](https://wiki.openstreetmap.org/wiki/Tag%3Aentrance%3Dentrance)

- false si membre d'une
    ![relation] relation
    [public_transport=stop_area](https://wiki.openstreetmap.org/wiki/FR%3ATag%3Apublic_transport%3Dstop_area)
    avec le rôle entry_only

- true sinon

#### Entrance/Width

Entrance/Width est rempli avec la valeur du tag
[width](https://wiki.openstreetmap.org/wiki/FR:Key:width) ou à défaut
[est_width](https://wiki.openstreetmap.org/w/index.php?title=FR:Key:est_width)
ou
[door:width](https://wiki.openstreetmap.org/wiki/FR%3AKey%3Adoor%3Awidth)
en mètres arrondis au cm.

#### Entrance/Height

Entrance/Height est rempli avec la valeur du tag
[height](https://wiki.openstreetmap.org/wiki/FR%3AKey%3Aheight) en
mètres arrondis au cm.

