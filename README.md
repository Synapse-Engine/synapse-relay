# Synapse Relay

REST API for Synapse

* [API Documentation | Swagger](https://petstore.swagger.io/?url=https://raw.githubusercontent.com/Synapse-Engine/synapse-relay/main/src/main/resources/synapse-openapi.yml)

## Information

* The relay passes graph data between a game server and the web editor.
* Run it as a standalone application.
* The API is still a work in progress. Please submit bugs and suggestions in the issues section.

## Usage

1. Clone the repository.
2. Compile with Gradle (`./gradlew build`).
3. Run `java -jar build/libs/synapse-relay-0.1.0-SNAPSHOT.jar`.
4. The API is accessible (by default) at `http://127.0.0.1:8080`.

## Configuration

The app can be configured using environment variables.

| Environment Variable         | Description                                          | Default Value |
|------------------------------|------------------------------------------------------|---------------|
| `SYNAPSE_RELAY_PORT`         | The port that the HTTP server should listen on       | `8080`        |
| `SYNAPSE_RELAY_AUTH`         | If API key authorization is enabled                  | `false`       |
| `SYNAPSE_RELAY_AUTH_KEYS`    | A comma-separated list of accepted API keys          | *none*        |
| `SYNAPSE_RELAY_MAX_BYTES`    | Maximum JSON body size, in bytes                     | `1048576`     |
| `SYNAPSE_RELAY_IDLE_TTL`     | How long a session can sit idle before it is deleted | `PT30M`       |
| `SYNAPSE_RELAY_ABSOLUTE_TTL` | The longest a session is kept                        | `PT2H`        |
| `SYNAPSE_RELAY_ACK_TIMEOUT`  | How long a save waits for the game server to respond | `PT10S`       |

## Security

By default, run the API so it is only reachable on the host machine.
For this reason, authentication is disabled by default.

If you make the API available over a wider network, configure authentication using API keys and enable HTTPS by exposing the API behind a reverse proxy.

Enable auth by setting `SYNAPSE_RELAY_AUTH` to true, and setting `SYNAPSE_RELAY_AUTH_KEYS` to a comma separated list of allowed API keys.

```yml
SYNAPSE_RELAY_AUTH: "true"
SYNAPSE_RELAY_AUTH_KEYS: "secureapikey,anothersecurekey"
```

Once enabled, API keys should be sent as a `Bearer` token inside the `Authorization` header of API requests.

```
Authorization: Bearer myverysecureapikey
```
