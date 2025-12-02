# Portainer

To keep track of all my current running containers I use Portainer. It works quite well and shows all the information I need. You can also build new containers or stacks within the app that you can manage more than if you created them in the command-line.

=== "portainer_compose.yaml"
    ``` yaml
    services:
      portainer:
        container_name: portainer
        image: portainer/portainer-ce:lts
        restart: always
        volumes:
          - /var/run/docker.sock:/var/run/docker.sock
          - portainer_data:/data
        ports:
          - 9443:9443
          - 8000:8000  # Remove if you do not intend to use Edge Agents
    
    volumes:
      portainer_data:
        name: portainer_data
    
    networks:
      default:
        name: portainer_network
    ```
