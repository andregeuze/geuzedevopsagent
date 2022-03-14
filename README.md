# Geuze Devops Agent
A docker image that will run an Azure DevOps Agent, configured with your own PAT and DevOps Organization.

# How to run
Docker compose (With watchtower for automatic updates!!!):
```
version: "3"
services:

  geuzedevopsagent:
    image: ghcr.io/andregeuze/geuzedevopsagent
    container_name: geuzedevopsagent
    restart: unless-stopped
    environment:
      - AZP_URL=https://dev.azure.com/<MY_ORG>
      - AZP_TOKEN=<MY_PAT>
      - AZP_AGENT_NAME=<MY_AGENT_NAME>
      - AZP_WORK=/_work
    volumes:
      - /my/local/work/folder:/_work

  watchtower:
    image: containrrr/watchtower:latest
    container_name: watchtower
    restart: unless-stopped
    environment:
      - TZ=Europe/Amsterdam
      - WATCHTOWER_CLEANUP=true
      - WATCHTOWER_SCHEDULE=0 */30 * * * *
      - WATCHTOWER_NOTIFICATIONS=shoutrrr
      - 'WATCHTOWER_NOTIFICATION_URL=discord://token@id'
      - 'WATCHTOWER_NOTIFICATION_TEMPLATE={{range .}}({{.Level}}): {{.Message}}{{println}}{{end}}'
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - /my/watchtower/config.json:/config.json
```