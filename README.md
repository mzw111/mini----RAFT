This project implements a Mini‑RAFT style distributed backend for a real‑time drawing board.

 Services
Frontend (static UI): http://localhost:8080

Gateway (WebSocket + leader routing): http://localhost:5000

Replica 1: http://localhost:6001

Replica 2: http://localhost:6002

Replica 3: http://localhost:6003

 RAFT‑Lite Endpoints
On each replica
POST /request-vote

POST /append-entries

POST /heartbeat

POST /sync-log

POST /stroke (leader only)

GET /status

GET /log

On gateway
POST /commit (leader notifies committed stroke)

GET /health

Run With Docker Compose
From project root:

bash
docker compose up --build
Open:

UI → http://localhost:8080

Stop:

bash
docker compose down
Run Without Docker (Local)
Run each terminal separately:

bash
cd replica1; node server.js
cd replica2; node server.js
cd replica3; node server.js
cd gateway; node server.js
Open frontend/index.html in browser.

 Quick Health Checks
powershell
Invoke-RestMethod http://localhost:6001/status
Invoke-RestMethod http://localhost:6002/status
Invoke-RestMethod http://localhost:6003/status
Invoke-RestMethod http://localhost:5000/health
 Failover Demo
Start all services.

Check current leader from /status.

Stop leader container/process.

Wait ~1 second.

Re‑check /status on remaining replicas → a new leader should appear.

Continue drawing; clients remain connected through gateway.
