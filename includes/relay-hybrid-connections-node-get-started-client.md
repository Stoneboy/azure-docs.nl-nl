### <a name="create-a-nodejs-application"></a>Een Node.js-toepassing maken
* Maak een nieuw JavaScript-bestand met de naam `sender.js`.

### <a name="add-the-relay-npm-package"></a>Het pakket Relay NPM toevoegen
* Voer `npm install hyco-ws` uit vanaf een Node-opdrachtprompt in de projectmap.

### <a name="write-some-code-to-send-messages"></a>Code schrijven om berichten te verzenden
1. Plaats de volgende `constants` boven aan het bestand `sender.js`.
   
    ```js
    const WebSocket = require('hyco-ws');
    const readline = require('readline')
        .createInterface({
            input: process.stdin,
            output: process.stdout
        });;
    ```
2. Voeg de volgende Relay `constants` toe aan `sender.js` voor de verbindingsgegevens van de hybride verbinding. Vervang de tijdelijke aanduidingen tussen punthaken door de juiste waarden die zijn verkregen bij het maken van de hybride verbinding.
   
   1. `const ns`: de Relay-naamruimte
   2. `const path`: de naam van de hybride verbinding
   3. `const keyrule`: de naam van de SAS-sleutel
   4. `const key`: de waarde van de SAS-sleutel
3. Voeg de volgende code toe aan het bestand `sender.js`:
   
    ```js
    WebSocket.relayedConnect(
        WebSocket.createRelaySendUri(ns, path),
        WebSocket.createRelayToken('http://'+ns, keyrule, key),
        function (wss) {
            readline.on('line', (input) => {
                wss.send(input, null);
            });
   
            console.log('Started client interval.');
            wss.on('close', function () {
                console.log('stopping client interval');
                process.exit();
            });
        }
    );
    ```
    Zo zou het bestand listener.js er moeten uitzien:
   
    ```js
    const WebSocket = require('hyco-ws');
    const readline = require('readline')
        .createInterface({
            input: process.stdin,
            output: process.stdout
        });;
   
    const ns = "{RelayNamespace}";
    const path = "{HybridConnectionName}";
    const keyrule = "{SASKeyName}";
    const key = "{SASKeyValue}";
   
    WebSocket.relayedConnect(
        WebSocket.createRelaySendUri(ns, path),
        WebSocket.createRelayToken('http://'+ns, keyrule, key),
        function (wss) {
            readline.on('line', (input) => {
                wss.send(input, null);
            });
   
            console.log('Started client interval.');
            wss.on('close', function () {
                console.log('stopping client interval');
                process.exit();
            });
        }
    );
    ```



<!--HONumber=Nov16_HO2-->


