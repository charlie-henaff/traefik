# Traefik proxy
Reverse proxy for development uses

## How to use 
- Add traefik network and configuration to services you want to redirect to
  ```yml
  # other project docker-compose.overide.yml
  services:  
    myservice:  
      [...]
       labels:
        - "traefik.enable=true"
        - "traefik.docker.network=[TRAEFIK_NETWORK_NAME]"
        - "traefik.http.routers.[ROUTER].rule=[RULES]"
        - "traefik.http.routers.[ROUTER].entrypoints=[ENTRYPOINTS]"
        - "traefik.http.services.[ROUTER].loadbalancer.server.port=[LOADBALANCER_PORT]"    
      networks:
        - [TRAEFIK_NETWORK_NAME]
    ```  
    Variables to replace : 
    ```
    [TRAEFIK_NETWORK_NAME]  : traefik network name (in .env file)
    [ROUTER]                : router custom name for service like 'myservicerouter'
    [RULES]                 : router rules like 'http://myservice.localhost'
    [ENTRYPOINTS]           : router entrypoints like 'secure, insecure'
    [LOADBALANCER_PORT]     : router loadbalancer port like '80' (running port of the targeted service)
    ```

    Other configuration samples : 
    ```
    # [RULES] for subdomains like *.myservice.localhost
    HostRegexp(`myservice.localhost`, `{subdomain:[A-Za-z0-9-]+}.myservice.localhost`)
    ```
- Start traefix  
  ```sh
  docker-compose up -d
  ```
- Live hosts available at http://traefik.localhost/dashboard/#/http/routers