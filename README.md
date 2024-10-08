## BOT pour les CTFs de Cyrhades

Exemple d'utilisation du BOT en PHP, prochainement intégré dans Yoop
```javascript
$client = new \WebSocket\Client("ws://<NOM_DU_CONTAINER_BOT>:8282");
$client->text(json_encode([
    "debug" => true,
    "host" => 'http://<NOM_DU_CONTAINER_HTTP>',
    "actions" => [
        [
            'url'       => 'http:/<NOM_DU_CONTAINER_HTTP>/url',
            'script'    => '
                document.querySelector("#username").value = "admin";
                document.querySelector("#password").value = "password";
                document.querySelector("#submit").click(); 
            '
        ],
        "sleep" => 1000,
        [
            "url" => $_POST['url']
        ]
    ],
]));
```

*La partie script est facultative et le sleep également*
Pour permettre l'utilisation et le fonctionnement de fetch il est recommandé d'ajouter des sleep, pour obtenir un résultat.
Dans cette nouvelle version il est possible d'activer un mode debug permettant de simplifier la mise en place dans un challenge.


# Intégration dans votre docker-compose.yml

```yaml
version: '3.0'
services:
  botserver:
    build: bot/
    restart: always
    container_name: botserver
    healthcheck:
      test: ["CMD", "wscat", "-c", "ws://localhost:8282"]
      interval: 5s
      timeout: 3s
      retries: 10
    command: npm start
```