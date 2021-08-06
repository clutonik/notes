# Container Health-check

- Healthcheck commands can be configured to run as regular healthchecks or while spinning
up a new container. 
- These commands can be configured in dockerfile or through docker CLI. 
- Container health-check allows us to have an extra gate which needs to be passed before
docker can mark a container running with healthy status.


## dockerfile

To add a healthcheck command to your dockerfile, follow the steps mentioned below:

- Add HEALTHCHECK directive to your dockerfile
  ```
  HEALTHCHECK --interval=5m --timeout=3s \
  CMD curl -f http://localhost/ || exit 1
  ```


## docker commands

To add a healthcheck command, follow the steps mentioned below:

- add healthcheck using docker run command:
  ```
    docker run \
    --health-cmd="curl -f localhost:9200/_cluster/health || false" \
    --health-interval=5s \
    --health-retries=3 \
    --health-timeout=2s \
    --health-start-period=15s \
    elasticsearch:2
   ```