
# Documentation du package aRest

Version : 0.4.0

Le package aRest permet de communiquer avec le module aREST d'Arduino pour envoyer des requêtes vers Arduino.

## Installation

Pour installer le package, exécutez la commande suivante :


`npm i https://github.com/tidd-africa/node-aREST.git` 

## Utilisation

Pour utiliser le package aRest, suivez les étapes ci-dessous :



    const express = require("express");
    const app = express();
    const port = 3000;
    
    const type = "serial"; // Type de connexion
    const path = "COM3"; // Port de connexion de l'Arduino au PC
    const speed = 9600; // Vitesse en bits/seconde définie dans le code (Serial.begin)
    
    // Rest
    var rest = require("arest")(app); // Appel à aRest et assignation de l'application Express
    rest.addDevice(type, path, speed); // Ajout du device à aREST
    
    app.get("/", (req, res) => res.send("Hello World!"));
    
    app.listen(port, () => console.log(`Example app listening on port ${port}!`));

## Routes disponibles

<br>
<br>

> ### Récupérer tous les devices

**URL :** `http://localhost:3000/devices`

Cette route permet de récupérer la liste de tous les devices connectés.

Exemple de requête :

`GET /devices` 

<br>
<br>

> ### Commande

**URL :** `http://localhost:3000/:device/:command`

Cette route permet d'envoyer une commande spécifique à un device.

-   `:device` : Nom du device auquel envoyer la commande.
-   `:command` : Commande à envoyer.

Exemple de requête :

`GET /device1/command1` 

<br>
<br>

> ### Écriture digitale

**URL :** `http://localhost:3000/:device/digital/:pin/:value`

Cette route permet d'écrire une valeur digitale sur une broche spécifique d'un device.

-   `:device` : Nom du device sur lequel effectuer l'écriture digitale.
-   `:pin` : Numéro de la broche à contrôler.
-   `:value` : Valeur à écrire (0 ou 1).

Exemple de requête :

`GET /device1/digital/3/1` 

<br>
<br>

> ### Lecture analogique

**URL :** `http://localhost:3000/:device/analog/:pin`

Cette route permet de lire la valeur analogique d'une broche spécifique d'un device.

-   `:device` : Nom du device sur lequel effectuer la lecture analogique.
-   `:pin` : Numéro de la broche à lire.

Exemple de requête :

`GET /device1/analog/0` 

<br>
<br>

> ### Écriture analogique

**URL :** `http://localhost:3000/:device/analog/:pin/:value`

Cette route permet d'écrire une valeur analogique sur une broche spécifique d'un device.

-   `:device` : Nom du device sur lequel effectuer l'écriture analogique.
-   `:pin` : Numéro de la broche à contrôler.
-   `:value` : Valeur à écrire (entre 0 et 255).

Exemple de requête :

`GET /device1/analog/5/120` 

<br>
<br>

> ### Lecture digitale

**URL :** `http://localhost:3000/:device/digital/:pin`

Cette route permet de lire la valeur digitale d'une broche spécifique d'un device.

-   `:device` : Nom du device sur lequel effectuer la lecture digitale.
-   `:pin` : Numéro de la broche à lire.

Exemple de requête :

`GET /device1/digital/2` 

<br>
<br>

> ### Mode

**URL :** `http://localhost:3000/:device/mode/:pin/:value`

Cette route permet de définir le mode d'une broche spécifique d'un device.

-   `:device` : Nom du device sur lequel définir le mode.
-   `:pin` : Numéro de la broche à configurer.
-   `:value` : Mode de la broche (INPUT, OUTPUT ou INPUT_PULLUP).

Exemple de requête :

`GET /device1/mode/4/OUTPUT` 

<br>
<br>

> ### Capture d'image (pour RPi)

**URL :** `http://localhost:3000/:device/camera/snapshot`

Cette route permet de capturer une image à partir d'un appareil photo (pour Raspberry Pi).

-   `:device` : Nom du device sur lequel effectuer la capture d'image.

Exemple de requête :

    enter code here
    GET /device1/camera/snapshot

 

Ceci conclut la documentation du package aRest. Utilisez les différentes routes pour interagir avec les devices connectés à Arduino

### Exemple complète 
Voici un exemple pour chaque partie de l'API :


    <!DOCTYPE html>
    <html>
    <head>
      <title>Contrôle d'Arduino</title>
      <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    </head>
    <body>
      <h1>Contrôle d'Arduino</h1>
    
      <h2>Liste des devices</h2>
      <button onclick="getDevices()">Obtenir les devices</button>
      <div id="devices"></div>
    
      <h2>Commande</h2>
      <label for="deviceCommand">Nom du device :</label>
      <input type="text" id="deviceCommand">
      <br>
      <label for="command">Commande :</label>
      <input type="text" id="command">
      <br><br>
      <button onclick="sendCommand()">Envoyer la commande</button>
    
      <h2>Écriture digitale</h2>
      <label for="deviceDigital">Nom du device :</label>
      <input type="text" id="deviceDigital">
      <br>
      <label for="pinDigital">Numéro de la broche :</label>
      <input type="number" id="pinDigital" min="0" max="13">
      <br>
      <label for="valueDigital">Valeur (0 ou 1) :</label>
      <input type="number" id="valueDigital" min="0" max="1">
      <br><br>
      <button onclick="writeDigital()">Écrire la valeur digitale</button>
    
      <h2>Lecture analogique</h2>
      <label for="deviceAnalog">Nom du device :</label>
      <input type="text" id="deviceAnalog">
      <br>
      <label for="pinAnalog">Numéro de la broche :</label>
      <input type="number" id="pinAnalog" min="0" max="5">
      <br><br>
      <button onclick="readAnalog()">Lire la valeur analogique</button>
    
      <h2>Écriture analogique</h2>
      <label for="deviceAnalogWrite">Nom du device :</label>
      <input type="text" id="deviceAnalogWrite">
      <br>
      <label for="pinAnalogWrite">Numéro de la broche :</label>
      <input type="number" id="pinAnalogWrite" min="0" max="5">
      <br>
      <label for="valueAnalogWrite">Valeur (0-255) :</label>
      <input type="number" id="valueAnalogWrite" min="0" max="255">
      <br><br>
      <button onclick="writeAnalog()">Écrire la valeur analogique</button>
    
      <h2>Lecture digitale</h2>
      <label for="deviceDigitalRead">Nom du device :</label>
      <input type="text" id="deviceDigitalRead">
      <br>
      <label for="pinDigitalRead">Numéro de la broche :</label>
      <input type="number" id="pinDigitalRead" min="0" max="13">
      <br><br>
      <button onclick="readDigital()">Lire la valeur digitale</button>
    
      <h2>Mode</h2>
      <label for="deviceMode">Nom du device :</label>
      <input type="text" id="deviceMode">
      <br>
      <label for="pinMode">Numéro de la broche :</label>
      <input type="number" id="pinMode" min="0" max="13">
      <br>
      <label for="valueMode">Mode (INPUT, OUTPUT, INPUT_PULLUP) :</label>
      <input type="text" id="valueMode">
      <br><br>
      <button onclick="setMode()">Définir le mode</button>
    
      <h2>Capture d'image (RPi)</h2>
      <label for="deviceCamera">Nom du device :</label>
      <input type="text" id="deviceCamera">
      <br><br>
      <button onclick="takeSnapshot()">Prendre une photo</button>
    
      <script> function getDevices() {
          var url = "http://localhost:8000/devices";
    
          $.get(url, function(data) {
            $("#devices").text(data);
          });
        }
    
        function sendCommand() {
          var device = $("#deviceCommand").val();
          var command = $("#command").val();
          var url = "http://localhost:8000/" + device + "/" + command;
    
          $.get(url, function(data) {
            console.log(data);
          });
        }
    
        function writeDigital() {
          var device = $("#deviceDigital").val();
          var pin = $("#pinDigital").val();
          var value = $("#valueDigital").val();
          var url = "http://localhost:8000/" + device + "/digital/" + pin + "/" + value;
    
          $.get(url, function(data) {
            console.log(data);
          });
        }
    
        function readAnalog() {
          var device = $("#deviceAnalog").val();
          var pin = $("#pinAnalog").val();
          var url = "http://localhost:8000/" + device + "/analog/" + pin;
    
          $.get(url, function(data) {
            console.log(data);
          });
        }
    
        function writeAnalog() {
          var device = $("#deviceAnalogWrite").val();
          var pin = $("#pinAnalogWrite").val();
          var value = $("#valueAnalogWrite").val();
          var url = "http://localhost:8000/" + device + "/analog/" + pin + "/" + value;
    
          $.get(url, function(data) {
            console.log(data);
          });
        }
    
        function readDigital() {
          var device = $("#deviceDigitalRead").val();
          var pin = $("#pinDigitalRead").val();
          var url = "http://localhost:8000/" + device + "/digital/" + pin;
    
          $.get(url, function(data) {
            console.log(data);
          });
        }
    
        function setMode() {
          var device = $("#deviceMode").val();
          var pin = $("#pinMode").val();
          var value = $("#valueMode").val();
          var url = "http://localhost:8000/" + device + "/mode/" + pin + "/" + value;
    
          $.get(url, function(data) {
            console.log(data);
          });
        }
    
        function takeSnapshot() {
          var device = $("#deviceCamera").val();
          var url = "http://localhost:8000/" + device + "/camera/snapshot";
    
          $.get(url, function(data) {
            console.log(data);
          });
        } </script>
    </body>
    </html>` 

Cet exemple utilise jQuery pour effectuer des requêtes GET vers les URLs correspondantes pour tester chaque fonctionnalité de l'API aRest. Les champs de saisie permettent de spécifier les paramètres nécessaires à chaque requête.

> **NB :** Assurez-vous de mettre à jour l'URL avec l'adresse et le port appropriés correspondant à votre configuration de serveur aRest.



N'oubliez pas de connecter votre Arduino avec le module aREST et de configurer le port et le type de connexion dans le code du serveur Express pour qu'il corresponde à votre configuration.

Vous pouvez utiliser cet exemple comme point de départ pour créer une interface utilisateur plus avancée pour interagir avec les différentes fonctionnalités du package aRest.

| D |O  |
|--|--|
| C |  P|

