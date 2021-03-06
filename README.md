# Keycloak export Module

This module allows you to perform a full export from the REST-API, while keycloak is still running.

## Install keycloak-export

You need Java-8-x Java environment

### Manually
Run

```
mvn clean install
```

You can deploy as a module by running:

    $KEYCLOAK_HOME/bin/jboss-cli.sh --command="module add --name=io.cloudtrust.keycloak-export --resources=target/keycloak-export-0.1-SNAPSHOT.jar --dependencies=org.keycloak.keycloak-core,org.keycloak.keycloak-server-spi,org.keycloak.keycloak-server-spi-private,org.keycloak.keycloak-services,javax.ws.rs.api"

Then registering the provider by editing `standalone/configuration/standalone.xml` and adding the module to the providers element:

    <providers>
        ...
        <provider>module:io.cloudtrust.keycloak-export</provider>
    </providers>

### Automatically

Simply call the install.sh script with the base directory of Keycloak as parameter.


Then start (or restart) the server. To use this module, the client (i.e. admin-cli) must have full scope allowed in the master realm.

## Using the module

The module is used as for other REST-API endpoints (see [here](https://www.keycloak.org/docs/1.9/server_development_guide/topics/admin-rest-api.html)):

1) Call the api to get an access token

```
curl \
  -d "client_id=admin-cli" \
  -d "username=admin" \
  -d "password=password" \
  -d "grant_type=password" \
  "http://localhost:8080/auth/realms/master/protocol/openid-connect/token"
```

Then call the `http://localhost:8080/auth/realms/master/export/realm` endpoint, using the token

```
curl \
  -H "Authorization: bearer eyJhbGciOiJSUz..." \
  "http://localhost:8080/auth/realms/master/export/realm"
```

You should see a json with the exported content.
You can also invoke the endpoint for other realms by replacing `master` with the realm name in the above url.


## Testing

Tests run with arquillian, as standard unit tests, similar to what is done on the keycloak project.
