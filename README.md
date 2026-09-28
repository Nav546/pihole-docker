# Pi-hole in Docker

Network-wide DNS ad/tracker blocker, run as a container from the official
`pihole/pihole` image. No custom code: this project is about **pulling and
configuring an existing image** (Compose, environment variables, ports,
persistent volumes, secrets via `.env`).

## What it demonstrates

* Pulling and configuring a third-party image from Docker Hub
* DNS in practice: a container answering DNS queries on port 53
* Secrets kept out of Git (`.env` + `.env.example`)
* A **named volume** for persistent state, with a way to inspect it
* Port mapping choices (web UI on 8080 to avoid conflicts)

## Run it

```bash
cp .env.example .env        # PowerShell: Copy-Item .env.example .env
# edit .env and set PIHOLE\_PASSWORD
docker compose up -d
```

Admin UI: http://localhost:8080/admin

## Test that it works

Query Pi-hole directly as the DNS server:

```powershell
nslookup google.com 127.0.0.1          # should resolve normally
nslookup doubleclick.net 127.0.0.1     # ad domain: should return 0.0.0.0 (blocked)
```

Then open the dashboard and watch the queries appear in the query log.

## Inspect the persistent volume

```powershell
docker volume ls
docker run --rm -v pihole-docker-project\_pihole\_data:/data alpine ls -la /data
```

(The volume name is prefixed with the project folder name. Confirm it with
`docker volume ls`.)

## Troubleshooting

* **Port 53 already in use:** change `"53:53/tcp"` and `"53:53/udp"` to
`"5353:53/tcp"` and `"5353:53/udp"`, then test with
`nslookup -port=5353 google.com 127.0.0.1`.
* **Forgot the password:** `docker exec -it pihole pihole setpassword`

## Stop / clean up

```bash
docker compose down        # stops and removes the container, keeps the volume
docker compose down -v     # also deletes the volume (config + stats)
```

## Note

This runs Pi-hole for local testing. Pointing your router or other devices at
it is a separate step; only do that once you're happy it works, because if the
container is down, those devices lose DNS.

