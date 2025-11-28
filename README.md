# caddy reverse proxy template

a template for a self-contained caddy reverse proxy setup, tested for ubuntu/debian VPS 

designed so that it is "self-contained" and for "seperation of concerns", 
ie. other repos does not need to know about the reverse proxy, and repos dont know about each other,
only the reverse proxy knows about the domains and repos it needs to serve, each repo just need to be concerned about its own port and domain,
although that "concern" does sort of "leak" out, cuz repos do need to somewhat "coordinate" in not clashing domains

designed so that a VPS can serve different types of services, 
like services running on docker, on a native node/bun/go/python/dotnet server, 
as long as it can be accessed via localhost, then it can be configured to be accessed via the reverse proxy

designed to be as simple and minimal as possible, and only require `docker` as the dependency to run

## prerequisites

### installed software

- docker

### DNS record setup

configure your DNS A record to point to your server IP
```
your-domain.com    A    YOUR_SERVER_IP
```
make sure that it has already propogated

### UFW firewall configuration
```bash
# allow SSH (if not already configured)
sudo ufw allow 22/tcp

# allow HTTP - required for Let's Encrypt certificate challenges
sudo ufw allow 80/tcp

# allow HTTPS - main traffic port for all domains
sudo ufw allow 443/tcp

# enable UFW
sudo ufw enable
```

## setup

edit the `Caddyfile` with target domain and the port

```caddyfile
your-domain.com {
   reverse_proxy :3000
}
```

## usage 

```bash
# start Caddy reverse proxy in detached mode
docker compose up -d

# stop Caddy reverse proxy
docker compose down

# check container status
docker compose ps

# view logs
docker compose logs -f caddy

# reload configuration without downtime
docker compose exec caddy caddy reload --config /etc/caddy/Caddyfile

# restart Caddy (brief downtime)
docker compose restart caddy
```

