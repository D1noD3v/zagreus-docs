# Minecraft server

Whenever me & my friends have the urge to start a 2 week minecraft phase, I spin one up in docker. Docker works really well for this as it's way easier to keep track of all the configuration files and its easier to delete everything when people become bored of the server.

## Vanilla minecraft
If you are looking to setup the same server, you can use this compose file. This setup includes world backups built-in.

=== "minecraft_compose.yaml"
    ```yaml
    services:
      minecraft:
        image: itzg/minecraft-server:latest
        container_name: minecraft # change to whatever
        restart: unless-stopped
        ports:
          - "25565:25565"
        volumes:
          - ./minecraft_data:/data
        environment:
          WORLD: "2025"
          EULA: "TRUE"
          TYPE: "FABRIC"
          VERSION: "1.21.5" 
          OPS: "<your-minecraft-username>"
          MEMORY: "6G" # change accordingly
          MODRINTH_MODPACK: "BYfVnHa7" # this is the ID for this modpack: https://modrinth.com/modpack/sop, change if you want
          VIEW_DISTANCE: "24" # optional
          SIMULATION_DISTANCE: "8" # optional
          SPAWN_PROTECTION: "0"
          SNOOPER_ENABLED: "false"
          ALLOW_NETHER: "true" # optional, as it's true by default
          ALLOW_FLIGHT: "true" # set to true if you encounter issues with getting kicked
          DIFFICULTY: "hard" # options: peaceful, creative, easy, hard or hardcore
          MOTD: "your-motd" # optional
          ENABLE_RCON: "true"
          RCON_PASSWORD: "${RCON_PASSWORD}" # create .env file for this
    
      # backup entire world
      minecraft-backup: 
        image: itzg/mc-backup
        container_name: minecraft-backup # change to match your server container e.g. <something>-backup
        user: "1000"
        restart: unless-stopped
        depends_on:
          minecraft:
            condition: service_healthy
        volumes:
          - ./minecraft_data:/data:ro
          - ./backups:/backups
        environment:
          INITIAL_DELAY: 0
          BACKUP_METHOD: "rsync"
          SRC_DIR: "/data"
          DEST_DIR: "/backups"
          RESTIC_VERBOSE: "true"
          BACKUP_INTERVAL: "30m"
          PAUSE_IF_NO_PLAYERS: "true"
          PLAYERS_ONLINE_CHECK_INTERVAL: "20s"
          BACKUP_NAME: "world"
          RCON_HOST: minecraft
          RCON_PASSWORD: "${RCON_PASSWORD}" # create .env file for this
          PRUNE_BACKUPS_DAYS: "30"
          PRUNE_BACKUPS_COUNT: "200"
    ```
    
    
## Modded minecraft
This method would be used if you want a modded minecraft server. In this compose I use the itzg/minecraft-server images' built-in type called `AUTO_CURSEFORGE`.
This will take any modpack from the curseforge website and automatically install it and set it up for use. To use this method you need a API key from curseforge which can be obtained [here](https://console.curseforge.com).

=== "modded_compose.yaml"
    ``` yaml
    services:
      minecraft:
        image: itzg/minecraft-server:latest
        container_name: minecraft # change to whatever
        restart: unless-stopped
        ports:
          - "25565:25565"
        volumes:
          - ./minecraft_data:/data
        environment:
          WORLD: "2025"
          EULA: "TRUE"
          TYPE: "AUTO_CURSEFORGE"
          CF_API_KEY: "${CF_API_KEY}" # get your own API key from: https://console.curseforge.com
          VERSION: "<modpack-minecraft-version>" # the version of minecraft that the modpack is built on
          OPS: "<your-minecraft-username>"
          MEMORY: "6G" # change accordingly
          SPAWN_PROTECTION: "0"
          SNOOPER_ENABLED: "false"
          ALLOW_NETHER: "true" # optional, as it's true by default
          ALLOW_FLIGHT: "true" # set to true if you encounter issues with getting kicked
          DIFFICULTY: "hard" # options: peaceful, creative, easy, hard or hardcore
          MOTD: "your-motd" # optional
          ENABLE_RCON: "true"
          RCON_PASSWORD: "${RCON_PASSWORD}" # create .env file for this
    
      # backup entire world
      minecraft-backup: 
        image: itzg/mc-backup
        container_name: minecraft-backup # change to match your server container e.g. <something>-backup
        user: "1000"
        restart: unless-stopped
        depends_on:
          minecraft:
            condition: service_healthy
        volumes:
          - ./minecraft_data:/data:ro
          - ./backups:/backups
        environment:
          INITIAL_DELAY: 0
          BACKUP_METHOD: "rsync"
          SRC_DIR: "/data"
          DEST_DIR: "/backups"
          RESTIC_VERBOSE: "true"
          BACKUP_INTERVAL: "30m"
          PAUSE_IF_NO_PLAYERS: "true"
          PLAYERS_ONLINE_CHECK_INTERVAL: "20s"
          BACKUP_NAME: "world"
          RCON_HOST: minecraft
          RCON_PASSWORD: "${RCON_PASSWORD}" # create .env file for this
          PRUNE_BACKUPS_DAYS: "30"
          PRUNE_BACKUPS_COUNT: "200"
    ```
