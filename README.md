# Eind project API

## Beschrijving gekozen thema
Het onderwerp dat ik gekozen heb is een spel genaamd Tom Clancy's Rainbow Six Siege. Ik heb hiermee een aantal GET requests gemaakt waardoor je alle spelers en operators kunt weergeven. Ook kun je door middel van een player ID alle informatie van een speler opvragen (het wachtwoord wordt niet getoond). Door het ID van je favoriete map mee te geven komt de naam van de map te staan onder de input. Je kunt ook met een map ID de naam van een map zoeken. Met mijn laatste GET request kun je een willekeurig cijfer meegeven. Het cijfer dat je meegeeft is het aantal maps met hun ID die getoond worden. Als je een getal meegeeft dat groter is dan het aantal maps die er zijn dan worden de maps onder elkaar in volgorde weergegeven. Ik heb ook 3 POST requests gemaakt zodat je een player, operator en een map kunt aanmaken. Ik heb ook gebruik gemaakt van een PUT en DELETE zodat je de player informatie kunt wijzigen of verwijderen. De wachtwoorden worden gehashed en sommige requests vereisen een authenticatie.

## Deployment
Ik heb mijn API door middel van Okteto Cloud in de cloud gedeployed. Dit heb ik gedaan door gebruik te maken van GitHub Workflow die een image op Dockerhub zet, zodat de docker compose file in de cloud de image kan downloaden en gedeployed kan laten worden.

## Uitbreiding:
Ik heb een front end gemaakt waar alle GET en POST requests opstaan en deze gehost op Netlify (zie Links). Ik heb deze ook een style gegeven.

<br />

### Links:
### website (hosted op netlify): [Klik hier](https://meek-basbousa-ebf34a.netlify.app/)

### hosted API: [Klik hier](https://cloud.okteto.com/?_gl=1*1jwnjs*_ga*MTUyMzAzNjk3OS4xNjY2OTU0NTIw*_ga_KSKZWJHTJZ*MTY3MzAyMDQxMS4xNS4wLjE2NzMwMjA0MTEuMC4wLjA.#/spaces/dennisraaijmakers?resourceId=c4923ad4-f201-4f7e-8779-1321a22736b5)

### Front end op github: [Klik hier](https://github.com/DennisRaaijmakers/api_eindproject_webpagina)

<br />

## OpenAPI screenshot:
Hier zie je een screenshot van OpenAPI docs:
![OpenAPI image](images/OpenAPI.png)


## GET requests:
1. **Get player by id** <br />
Als je een player ID meegeeft krijg je de informatie over een player.
Dit heb ik gedaan door deze link in te geven: ```https://system-service-dennisraaijmakers.cloud.okteto.net/get/player/1```<br />
![postman get player image](images/get_player_by_id_postman.PNG)
Zo ziet het eruit als ik het op de website doe:<br />
![website get player image](images/get_player_by_id_web.PNG)

2. **Get all players** <br />
Dit wordt beveiligd met authenticatie. Je kunt pas alle players zien als je een token ingeeft.
Dit heb ik gedaan door deze link in te geven: ```https://system-service-dennisraaijmakers.cloud.okteto.net/players/```

Zonder authenticatie:<br />
![postman get all player image zonder authenticatie](images/all_players_NA_postman.PNG)
Met authenticatie:<br />
![postman get all player image met authenticatie](images/all_players_A_postman.PNG)
Op de website:<br />
![postman get all player image](images/all_players_A_web.PNG)

3. **Get all operators** <br />
Het principe is hetzelfde als "Get all players". Door een token mee te geven krijg je alle operators te zien; anders krijg je een "not authenticated" error.

De link die ik gebruikt heb hiervoor is : ```https://system-service-dennisraaijmakers.cloud.okteto.net/operators/```<br />
Zonder authenticatie:<br />
![postman get all player image zonder authenticatie](images/all_operators_NA_postman.PNG)<br />
Met authenticatie:<br />
![postman get all player image met authenticatie](images/all_operators_A_postman.PNG)<br />
Op de website:<br />
![postman get all player image](images/all_operators_A_web.PNG)<br />

4. **Get map by id** <br />
Door het ID van de map in te geven krijg je de naam van de map te zien.

De link die ik gebruikt heb hiervoor is : ```https://system-service-dennisraaijmakers.cloud.okteto.net/get/map/1```<br />

Op postman: <br />
![get map by id postman](images/get_map_by_id_postman.PNG)<br />
Op de website: <br />
![get map by id postman](images/get_map_by_id_web.PNG)<br />
 
5. **Get favorite map of player** <br />
Deze GET request geeft de favoriete map van een speler weer door het ID van de speler in te geven.<br />
De link die ik hiervoor gebruik is: ```https://system-service-dennisraaijmakers.cloud.okteto.net/get/player/favoritemap/2``` <br />

Op postman: <br />
![get fav map postman](images/get_fav_map_postman.PNG)<br />
Op de website: <br />
![get fav map web](images/get_fav_map_web.PNG)<br />

**Verduidelijking** <br />

In dit voorbeeld heb ik player 2 gebruikt.
Zoals je op de foto kunt zien is de favoriete map ID van player 2 het nummer 3.<br />
![verduidelijking 1](images/verduidelijking_get5.PNG)<br />

En zoals je op deze foto kunt zien is de map met het ID 3 de map Clubhouse:
![verduidelijking 2](images/verduidelijking_get5_mapid.PNG)<br />

6. **Get random map** <br />
Bij deze GET request wordt er gebruik gemaakt van de query parameter. Met deze GET request vraag je een random aantal maps waarvan je het aantal zelf kunt kiezen. Als je niks ingeeft dan zal er altijd 1 map getoond worden omdat de default waarde 1 is.<br />
De link die ik hiervoor gebruik is: ```https://system-service-dennisraaijmakers.cloud.okteto.net/map/random?amount=3``` <br />

Op postman: <br />
![get random postman](images/get_random_map_postman.PNG)<br />
Op de website: <br />
![get random web](images/get_random_map_web.PNG)<br />

hier zie je een voorbeeld als je een hoger getal meegeeft dan dat er maps in de database zijn. <br />
![get random te veel](images/get_random_map_web_teveel.PNG)<br />



## POST requests:
1. **Create player** <br />
Met deze POST request kun je een player aanmaken. Er zijn wel een paar eisen: je mag niet dezelfde username hebben als iemand anders en ook niet het zelfde e-mailadres. Na het maken worden de waardes terug gegeven behalve het wachtwoord.<br />

Voorbeeld JSON voor als body in postman: <br />

<pre>
{
    "fav_map_id": 2,
    "username": "player",
    "name": "player",
    "email": "player@test.com",
    "password": "abc123!",
    "region": "Europe",
    "mmr": 1234
}
</pre>
(Het player ID wordt automatisch genummerd) <br />

Op postman: <br />
![post player postman](images/post_player_postman.PNG)<br />

Op de website: <br />
(in de inputs moet je de gegevens invullen die gevraagd worden) <br />
![post player web](images/post_player_web.PNG)<br />
    
2. **Create map** <br />
Bij deze POST request kun je een map aanmaken. Hier is ook een eis: de map die je aanmaakt mag nog niet bestaan. <br />
Voorbeeld JSON voor als body in postman: <br />
<pre>
{
    "map_name": "border"
}
</pre>

Op postman: <br />
![post map postman](images/post_map_postman.PNG) <br />

Op de website: <br />
(in de inputs moet je de gegevens invullen die gevraagd worden) <br />
![post map web](images/post_map_web.PNG) <br />


3. **Create operator** <br />
Met deze POST request kun je een operator aanmaken. Er is hier ook een eis en dat is dat de naam van de operator niet al mag bestaan in de database. <br />

Voorbeeld JSON voor als body in postman: <br />
<pre>
{
    "operator_name":"Twitch",
    "primary_weapon":"F2",
    "secondary_weapon":"P9",
    "side": "attack"
}
</pre>

Op postman: <br />
![post op postman](images/post_op_postman.PNG) <br />

Op de website: <br />
(in de inputs moet je de gegevens invullen die gevraagd worden) <br />
![post op web](images/post_op_web.PNG) <br />

Als ik vervolgens nog eens op "send" klik met dezelfde gegevens dan krijg ik een error: <br />
![error op create](images/post_op_postman_bestaat.PNG) <br />

4. **Login token** <br />
Met deze POST request kun je een token aanvragen zodat je bepaalde dingen wel kunt zien. Indien je die token meegeeft dan ben je geauthenticeerd. De token krijg je enkel als je een player hebt aangemaakt en dan met het e-mailadres en password inlogt van die speler.

![inlog /token](images/post_token.PNG) <br />

Nadat deze inlog succesvol is krijg je een token teruggestuurd. Als je deze token ingeeft dan werken de requests, waar je eerst niet geauthenticeerd voor was, wel.<br />

![token](images/post_token_key.PNG) <br />

![get success](images/get_with_token.PNG) <br />

## PUT request:
In de PUT request kun je een player aanpassen.<br />
In dit voorbeeld heb ik de favoriete map, username en het aantal mmr aangepast:<br />
Before: <br />
![get success](images/put_before.PNG) <br />
Put request: <br />
![get success](images/put_request.PNG) <br />
After: <br />
![get success](images/put_after.PNG) <br />

De gebruikte link: <br />
```https://system-service-dennisraaijmakers.cloud.okteto.net/update/player/1``` <br />

## DELETE request:
Met deze DELETE request kun je een player deleten door in de link het player ID van de speler die je wilt deleten weg te doen.<br />
Gebruikte link: <br />
```https://system-service-dennisraaijmakers.cloud.okteto.net/delete/player/1``` <br />
Hier zie je dat ik player 1 gedelete heb: <br />
![delete player](images/delete_player.PNG) <br />
Checken of de player weg is in postman: <br />
![check players postman](images/delete_player_check_postman.PNG) <br />
Checken of de player weg is op de website: <br />
![check player web](images/delete_player_check_web.PNG) <br />


## Slot
Ik vond het een heel interessante opdracht om te maken. Ik heb ook aan alle basistaken gewerkt en als uitbreiding heb ik een front end met alle GET en POST requests gemaakt.Ik heb de front end ook nog een beetje gestyled. Deze front end wordt gehost op Netlify (zie Links).

