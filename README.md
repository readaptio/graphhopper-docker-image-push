# Graphhopper docker provider repository
This repository holds the very basic things in order to make sure there's an updated graphhopper docker image which we use in our production server.
Images can be found here:
https://hub.docker.com/r/readaptio/graphhopper

Thank the [graphhopper](https://www.graphhopper.com/) team for their hard work and amazing product!
This repository is extremely simple, all it does is the following:
1. Every night at 1 AM it builds the latest code using Github actions from the [graphhopper repository](https://github.com/graphhopper/graphhopper) and uploads the image to docker hub with the `latest` tag
2. It checks if there's a new version tag, and if so builds it and upload it as well with the relevant tag
3. Adds a graphhopper.sh file for ease of use

That's all.

Feel free to submit issues or pull requests if you would like to improve the code here

This docker image uses the following default environment setting:
```
JAVA_OPTS: "-Xmx1g -Xms1g"
```

For a quick startup you can run the following command to create the andorra routing:
```
docker run -p 8989:8989 readaptio/graphhopper --url https://download.geofabrik.de/africa/cameroon-latest.osm.pbf --host 0.0.0.0
```
Then surf to `http://localhost:8989/`

You can also completely override the entry point and use this for example:
```
docker run --entrypoint /bin/bash readaptio/graphhopper -c "wget https://download.geofabrik.de/africa/cameroon-latest.osm.pbf -O /data/cameroon.osm.pbf && java -Ddw.graphhopper.datareader.file=/data/cameroon.osm.pbf -Ddw.graphhopper.graph.location=cameroon-gh -jar *.jar server config-example.yml"
```

Checkout `graphhopper.sh` for more usage options such as import.

In order to build the docker image locally, please run [`.github/build.sh`](.github/build.sh).
