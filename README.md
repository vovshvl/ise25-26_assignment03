# CampusCoffee (WS 25/26)

## Prerequisites

* Install [Docker Desktop](https://www.docker.com/products/docker-desktop/) or a compatible open-source alternative such as [Rancher Desktop](https://rancherdesktop.io/).
* Install the [Temurin JDK 21](https://adoptium.net/temurin/releases/?version=21&os=any&arch=any) and [Maven 3.9](https://maven.apache.org/install.html) either via the provided [`mise.toml`](mise.toml) file (see [getting started guide](https://mise.jdx.dev/getting-started.html) for details) or directly via your favorite package manager.
* Install a Java IDE. We recommend [IntelliJ](https://www.jetbrains.com/idea/), but you are free to use alternatives such as [VS Code](https://code.visualstudio.com/) with suitable extensions.

## Build application

First, make sure that the Docker daemon is running.
Then, to build the application, run the following command in the command line (or use the Maven integration of your IDE):

```shell
mvn clean install
```
**Note:** In the `dev` profile, all repositories are cleared before startup, the initial data is loaded (see [`LoadInitialData.java`](application/src/main/java/de/seuhd/campuscoffee/LoadInitialData.java)).

You can use the quiet mode to suppress most log messages:

```shell
mvn clean install -q
```

## Start application (dev)

First, make sure that the Docker daemon is running.
Before you start the application, you first need to start a Postgres docker container:

```shell
docker run -d -e POSTGRES_USER=postgres -e POSTGRES_PASSWORD=postgres -p 5432:5432 postgres:17-alpine
```

Then, you can start the application:

```shell
cd application
mvn spring-boot:run -Dspring-boot.run.profiles=dev
```
**Note:** The data source is configured via the [`application.yaml`](application/src/main/resources/application.yaml) file.

## REST API

You can use `curl` in the command line to send HTTP requests to the REST API.

### POS endpoint

#### Get POS

All POS:
```shell
curl http://localhost:8080/api/pos
```
POS by ID:
```shell
curl http://localhost:8080/api/pos/1 # add valid POS id here
```

#### Create POS

```shell
curl --header "Content-Type: application/json" --request POST --data '{"name":"New Café","description":"Description","type":"CAFE","campus":"ALTSTADT","street":"Hauptstraße","houseNumber":"100","postalCode":69117,"city":"Heidelberg"}' http://localhost:8080/api/pos
```

#### Update POS

Update title and description:
```shell
 curl --header "Content-Type: application/json" \
     --request POST \
     --data '{"name":"# Bäckerei Kohlmann","description":"Do not use unless mensa is closed","type":"CAFE","campus":"INF","street":"INF","houseNumber":"370","postalCode":69120,"city":"Heidelberg"}' \
     http://localhost:8080/api/pos
{"id":6,"createdAt":"2025-11-05T20:25:36.8906479","updatedAt":"2025-11-05T20:25:36.8906479","name":"# Backerei Kohlmann","description":"Do not use unless mensa is closed","type":"CAFE","campus":"INF","street":"INF","houseNumber":"370","postalCode":69120,"city":"Heidelberg"}
~curl http://localhost:8080/api/pos/6                                                                                                            
                                                                                                                        
                                                                                                                                                                                  
StatusCode        : 200             letWebResponseException,Microsoft.PowerShell.Commands.In 
StatusDescription : mand
Content           : {"id":6,"createdAt":"2025-11-05T20:25:36.890648","updatedAt":"2025-11-05T20:25:36.890648","name":"# Backerei Kohlmann","description":"Do not use unless mensa is 
                    closed","type":"CAFE","campus":"INF","s...
RawContent        : HTTP/1.1 200 
                    Transfer-Encoding: chunked
                    Keep-Alive: timeout=60
                    Connection: keep-alive
                    Content-Type: application/json
                    Date: Wed, 05 Nov 2025 20:29:58 GMT
                    
                    {"id":6,"createdAt":"2025-11-05T20:25:...
Forms             : {}
Headers           : {[Transfer-Encoding, chunked], [Keep-Alive, timeout=60], [Connection, keep-alive], [Content-Type, application/json]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 272


```
