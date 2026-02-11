# Northflank setup (1 service = 1 container)

## AIOStreams
Image: ghcr.io/viren070/aiostreams:latest
Port: 11451 (HTTP, public)
Env: BASE_URL, SECRET_KEY, LOG_LEVEL (și opțional ADDON_PASSWORD)
Volume: mount /app/data (optional, recommended)

## AIOMetadata
Image: ghcr.io/cedya77/aiometalatest
Port: 3232 (HTTP, public)
Redis: create Redis Addon, then set env REDIS_URL from addon connection string (use alias/secret group)
Env: HOST_NAME, TMDB_API_KEY, DATABASE_URI, REDIS_URL
Volume: mount /app/addon/data

## EasyProxy
Image: justsml/easy-proxy:latest
Port: 5050 (HTTP, public)
Env: PROXY_USERNAME, PROXY_PASSWORD, PROXY_PORT, PROXY_HOST
