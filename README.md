# Stremio monorepo (Northflank)

Repo-ul conține template-uri ⁠ .env.example ⁠ pentru:
•⁠  ⁠AIOStreams
•⁠  ⁠AIOMetadata (+ Redis)
•⁠  ⁠EasyProxy

Northflank: creezi câte 1 service per container (nu rulează direct docker-compose). Configurezi Ports (HTTP) + Runtime variables (ENV) din UI.

---

## Regula pentru URL (Northflank)
Când expui un port HTTP public, Northflank îți generează un domeniu în format:

⁠ [port-name]--[service-name]--[random].code.run ⁠

De aceea:
•⁠  ⁠alege service name-uri simple (fără spații)
•⁠  ⁠alege port name-uri scurte (ex. ⁠ http ⁠)

---

## Servicii (recomandare nume)

### 1) AIOStreams
Service name: ⁠ aiostreams ⁠  
Image: ⁠ ghcr.io/viren070/aiostreams:latest ⁠  
Public port:
•⁠  ⁠Port: ⁠ 11451 ⁠
•⁠  ⁠Protocol: ⁠ HTTP ⁠
•⁠  ⁠Port name: ⁠ http ⁠
•⁠  ⁠Public: ⁠ true ⁠

Env (Runtime variables):
•⁠  ⁠⁠ BASE_URL=https://REPLACE_WITH_NORTHFLANK_URL ⁠
•⁠  ⁠⁠ SECRET_KEY=REPLACE_WITH_RANDOM ⁠
•⁠  ⁠⁠ LOG_LEVEL=info ⁠ (optional)

Storage (optional, recommended):
•⁠  ⁠Mount volume at ⁠ /app/data ⁠

După deploy: pune URL-ul generat în ⁠ aiostreams/.env ⁠ la ⁠ BASE_URL ⁠.

---

### 2) AIOMetadata
Service name: ⁠ aiometadata ⁠  
Image: ⁠ ghcr.io/cedya77/aiometalatest ⁠  
Public port:
•⁠  ⁠Port: ⁠ 3232 ⁠
•⁠  ⁠Protocol: ⁠ HTTP ⁠
•⁠  ⁠Port name: ⁠ http ⁠
•⁠  ⁠Public: ⁠ true ⁠

Redis:
•⁠  ⁠Creează un Redis Addon în Northflank.
•⁠  ⁠Setează ⁠ REDIS_URL ⁠ în service (din connection string / secret group alias).

Env:
•⁠  ⁠⁠ HOST_NAME=https://REPLACE_WITH_NORTHFLANK_URL ⁠
•⁠  ⁠⁠ REDIS_URL=REPLACE_FROM_REDIS_ADDON ⁠
•⁠  ⁠⁠ DATABASE_URI=sqlite:///app/addon/data/aiometadata.db ⁠
•⁠  ⁠⁠ TMDB_API_KEY=REPLACE_WITH_TMDB_KEY ⁠

Storage (recommended):
•⁠  ⁠Mount volume at ⁠ /app/addon/data ⁠

După deploy: pune URL-ul generat în ⁠ aiometadata/.env ⁠ la ⁠ HOST_NAME ⁠.

---

### 3) EasyProxy
Service name: ⁠ easyproxy ⁠  
Image: ⁠ justsml/easy-proxy:latest ⁠  
Public port:
•⁠  ⁠Port: ⁠ 5050 ⁠
•⁠  ⁠Protocol: ⁠ HTTP ⁠
•⁠  ⁠Port name: ⁠ http ⁠
•⁠  ⁠Public: ⁠ true ⁠

Env:
•⁠  ⁠⁠ PROXY_USERNAME=admin ⁠
•⁠  ⁠⁠ PROXY_PASSWORD=REPLACE_WITH_PASSWORD ⁠
•⁠  ⁠⁠ PROXY_PORT=5050 ⁠
•⁠  ⁠⁠ PROXY_HOST=REPLACE_WITH_PUBLIC_DOMAIN_OR_IP ⁠

---

## Notițe
•⁠  ⁠Public ports pe Northflank trebuie să fie HTTP/HTTP2 (nu TCP) ca să primești domeniu + TLS.
•⁠  ⁠Nu include ⁠ :11451 ⁠ / ⁠ :3232 ⁠ / ⁠ :5050 ⁠ în URL-urile HTTPS publice; folosește doar ⁠ https://<domain> ⁠.
