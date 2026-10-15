# synapse-relay

Ephemeral session service between a Synapse game server and the web editor. The process keeps sessions in memory and drops them on restart.

## Build

```
./gradlew check
./gradlew shadowJar
```

The runnable jar is `build/libs/synapse-relay-0.1.0-SNAPSHOT.jar`. Its main class is `io.synapseproject.relay.RelayMain`.

```
java -jar build/libs/synapse-relay-0.1.0-SNAPSHOT.jar
```

## Configuration

- `SYNAPSE_RELAY_PORT` — listen port. Default `8080`.
- `SYNAPSE_RELAY_MAX_BYTES` — maximum JSON body size. Default `1048576`.
- `SYNAPSE_RELAY_IDLE_TTL` — idle session lifetime, ISO-8601 duration. Default `PT30M`.
- `SYNAPSE_RELAY_ABSOLUTE_TTL` — maximum session lifetime, ISO-8601 duration. Default `PT2H`.
- `SYNAPSE_RELAY_ACK_TIMEOUT` — how long a save waits for the plugin socket, ISO-8601 duration. Default `PT10S`.

## HTTP API

`POST /v1/sessions` with a JSON object creates a session and returns `sessionId`, `serverToken`, `editorToken`, and `expiresAt`.

`GET /v1/sessions/{id}` reads the latest document. Send `Authorization: Bearer` with either token. The response includes `X-Synapse-Revision`.

`PUT /v1/sessions/{id}` stores a new revision. Only the editor token can write. The response status is `applied`, `rejected`, or `stored`.

`GET /v1/revisions/{revision}` reads one revision. The revision id is the read capability. It does not grant writes.

The plugin keeps one WebSocket open at `/v1/sessions/{id}/server` using the server token.
