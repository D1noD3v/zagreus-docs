# Nextcloud

Nextcloud is my alternative to google drive. It offers the same functionality and has extensions you can add to make it more suitable for business like collaboration, document editors and much more.

=== "nextcloud_compose.yaml"
    ``` yaml
    services:
      nextcloud:
        image: lscr.io/linuxserver/nextcloud:latest
        container_name: nextcloud
        environment:
          - PUID=1000
          - PGID=1000
          - TZ=America/Anchorage # change according to your need
        volumes:
          - $HOME/docker_configs/config:/config
          - <dir-for-data>:/data
        ports:
          - 8065:443
        restart: unless-stopped
    ```
