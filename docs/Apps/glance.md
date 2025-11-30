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

Glance also has a bunch of configuration options. Here is what I run on my configs:

=== "glance.yaml"
    ``` yaml
    theme:
      background-color: 50 1 8
      primary-color: 126 100 45
      negative-color: 214 93 38
      contrast-multiplier: 1.3
    pages:
    - name: Start
      width: slim
      hide-desktop-navigation: false
      center-vertically: true
      columns:
        - size: full
          widgets:
            - type: custom-api
              title: Random Fact
              cache: 6h
              url: https://uselessfacts.jsph.pl/api/v2/facts/random
              template: |
                <p class="size-h4 color-paragraph">{{ .JSON.String "text" }}</p>
            - type: search
              autofocus: true
    
            - type: monitor
              cache: 1m
              title: Services
              sites:
                - title: Jellyfin
                  url: https://jelly.dinodev.org
                  icon: di:jellyfin
                - title: Immich
                  url: https://immich.dinodev.org
                  icon: di:immich
                - title: Pi-Hole
                  url: http://192.168.69.149/admin/login
                  icon: di:pi-hole
                - title: Vaultwarden
                  url: https://vault.dinodev.org
                  icon: di:vaultwarden
            - type: docker-containers
              hide-by-default: true
    
            - type: bookmarks
              groups:
                - title: General
                  links:
                    - title: Gmail
                      url: https://mail.google.com/mail/u/0/
                    - title: ProtonMail
                      url: https://mail.proton.me/u/0/inbox
                    - title: Github
                      url: https://github.com/D1noD3v
                - title: Entertainment
                  links:
                    - title: YouTube
                      url: https://www.youtube.com/
                    - title: Jellyfin
                      url: https://jelly.dinodev.org
                - title: Social
                  links:
                    - title: Reddit
                      url: https://www.reddit.com/
                    - title: Bluesky
                      url: https://bsky.app/
                    - title: Instagram
                      url: https://www.instagram.com/
    
    - name: News
      columns:
        - size: small
          widgets:
            - type: dns-stats
              service: pihole-v6
              url: http://192.168.1.149/
              password: ${pihole_pass}
              token: ${pihole_token}
              hour-format: 24h
            - type: calendar
              first-day-of-week: monday
            - type: clock
              hour-format: 24h
              timezones:
                - timezone: Europe/Stockholm
                  label: Stockholm
                - timezone: Europe/Vilnius
                  label: Vilnius
            - type: twitch-channels
              channels:
                - theprimeagen
                - christitustech
                - drututt
    
        - size: full
          widgets:
            - type: group
              widgets:
              - type: rss
                title: News
                limit: 10
                collapse-after: 3
                cache: 12h
                style: detailed-list
                feeds:
                  - url: https://www.unt.se/rss/lokalt/uppsala
                    title: Upsala Nya Tidning
                  - url: https://www.unt.se/rss/senaste-nytt
                    title: UNT Senaste Nytt
                  - url: https://www.svt.se/rss.xml
                    title: SVT Nyheter
                  - url: https://feeds.expressen.se/nyheter/
                    title: Expressen
                  - url: https://rss.aftonbladet.se/rss2/small/pages/sections/senastenytt/
                    title: Aftonbladet
              - type: rss
                title: Tech News & Blogs
                limit: 10
                collapse-after: 5
                cache: 12h
                style: detailed-list
                feeds:
                  - url: https://www.gamingonlinux.com/article_rss.php
                    title: Gaming on Linux
                  - url: https://hnrss.org/newest?q=linux
                    title: Hacker News - Linux
                  - url: https://daniel.haxx.se/blog/feed/
                    title: Daniel Stenberg Blog
              - type: hacker-news
    
    
            - type: videos
              channels:
                - UCld68syR8Wi-GY_n4CaoJGA # Brodie Robertson
                - UCXuqSBlHAE6Xw-yeJA0Tunw # Linus Tech Tips
                - UCR-DXc1voovS8nhAvccRZhg # Jeff Geerling
                - UCsBjURrPoezykLs9EqgamOA # Fireship
                - UCHnyfMqiRRG1u-2MsSQLbXA # Veritasium
                - UCEIwxahdLz7bap-VDs9h35A # Steve Mould
                - UCAMu6Dso0ENoNm3sKpQsy0g # Nir Lichtman
                - UCJ0-OtVpF0wOKEqT2Z1HEtA # ElectroBOOM
                - UC7YOGHUfC1Tb6E4pudI9STA # Mental Outlaw
            - type: group
              widgets:
                - type: reddit
                  subreddit: selfhosted
                  show-thumbnails: true
                - type: reddit
                  subreddit: linux
                  show-thumbnails: true
                - type: reddit
                  subreddit: homelab
                  show-thumbnails: true
                - type: reddit
                  subreddit: sysadmin
                  show-thumbnails: true
    
        - size: small
          widgets:
            - type: weather
              location: Uppsala, Sweden
              units: metric # alternatively "imperial"
              hour-format: 24h # alternatively "24h"
              # Optionally hide the location from being displayed in the widget
              # hide-location: true
            - type: custom-api
              title: Steam Specials
              cache: 12h
              url: https://store.steampowered.com/api/featuredcategories?cc=se
              template: |
                <ul class="list list-gap-10 collapsible-container" data-collapse-after="5">
                {{ range .JSON.Array "specials.items" }}
                  <li>
                    <a class="size-h4 color-highlight block text-truncate" href="https://store.steampowered.com/app/{{ .Int "id" }}/">{{ .String "name" }}</a>
                    <ul class="list-horizontal-text">
                      <li>{{ div (.Int "final_price" | toFloat) 100 | printf "€%.2f" }}</li>
                      {{ $discount := .Int "discount_percent" }}
                      <li{{ if ge $discount 40 }} class="color-positive"{{ end }}>{{ $discount }}% off</li>
                    </ul>
                  </li>
                {{ end }}
                </ul>
    
            - type: monitor
              cache: 1m
              title: Services
              sites:
                - title: Jellyfin
                  url: https://jelly.dinodev.org
                  icon: di:jellyfin
                - title: Immich
                  url: https://immich.dinodev.org
                  icon: di:immich
                - title: Pi-hole
                  url: http://192.168.69.149
                  icon: di:pi-hole
                - title: Vaultwarden
                  url: https://vault.dinodev.org
                  icon: di:vaultwarden
                - title: Dinodev Portfolio
                  url: https://www.dinodev.org
                  icon: di:next.js
    - name: Gaming
      columns:
        - size: small
          widgets:
            - type: twitch-top-games
              limit: 20
              collapse-after: 13
              exclude:
                - just-chatting
                - pools-hot-tubs-and-beaches
                - music
                - art
                - asmr
    
        - size: full
          widgets:
            - type: group
              widgets:
                - type: reddit
                  show-thumbnails: true
                  subreddit: pcgaming
                - type: reddit
                  subreddit: games
    
            - type: videos
              style: grid-cards
              collapse-after-rows: 3
              channels:
                - UCNvzD7Z-g64bPXxGzaQaa4g # gameranx
                - UCZ7AeeVbyslLM_8-nVy2B8Q # Skill Up
                - UCHDxYLv8iovIbhrfl16CNyg # GameLinked
                - UC9PBzalIcEQCsiIkq36PyUA # Digital Foundry
    
        - size: small
          widgets:
            - type: reddit
              subreddit: gamingnews
              limit: 7
              style: vertical-cards
    ```
