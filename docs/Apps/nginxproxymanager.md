# Nginx Proxy manager (NPM)

[Nginx Proxy Manager](https://nginxproxymanager.com/) is a app that simplifies setting up a reverse proxy for any app that you want exposed to the internet.

!!! warning

    In order to make sure nginx can serve your app you need to make sure you have a public IP address and have port forwarding rules set up on your router. Getting a hold of a public IP address is something you can ask your ISP about. Port forwarding depends on your router and you most likely find information about this in the routers manual or if your ISP provided you a router you may ask them.

=== "docker-compose.yaml"
    ``` yaml
    services:
      app:
        image: 'docker.io/jc21/nginx-proxy-manager:latest'
        restart: unless-stopped
        ports:
          - '80:80'
          - '81:81'
          - '443:443'
        volumes:
          - $HOME/nginxproxymanager:/data
          - $HOME/nginxproxymanager:/etc/letsencrypt
    ```
