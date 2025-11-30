# Glance

[Glance](https://github.com/glanceapp/glance) is a RSS feed aggregator that you can use to collect RSS feeds and display them. Useful if you are subscribed to many different articles and want to have it all centralized.

=== "docker-compose.yaml"
    ``` yaml
    services:
      glance:
        container_name: glance
        image: glanceapp/glance
        volumes:
          - ./config:/app/config
          - /var/run/docker.sock:/var/run/docker.sock # if you want glance to be able to show your running containers
        ports:
          - 8079:8080 # nginx proxy manager runs on 8080 so switch to other external port
    ```
