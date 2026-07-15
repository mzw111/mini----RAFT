Distributed Drawing Board (Mini-RAFT)
This project implements a Mini-RAFT style distributed backend for a real-time drawing board.

Services
frontend (static UI): http://localhost:8080
gateway (WebSocket + leader routing): http://localhost:5000
replica1: http://localhost:6001
replica2: http://localhost:6002
replica3: http://localhost:6003
Implemented RAFT-lite Endpoints
On each replica:

POST /request-vote
POST /append-entries
POST /heartbeat
POST /sync-log
POST /stroke (leader only)
GET /status
GET /log
On gateway:

POST /commit (leader notifies committed stroke)
GET /health
Run With Docker Compose
From project root:

docker compose up --build
Open:

UI: http://localhost:8080
Stop:

docker compose down
Run Without Docker (Local)
Run each terminal separately:

cd replica1; node server.js
cd replica2; node server.js
cd replica3; node server.js
cd gateway; node server.js
Open fronted/index.html in browser.

Quick Health Checks
Invoke-RestMethod http://localhost:6001/status
Invoke-RestMethod http://localhost:6002/status
Invoke-RestMethod http://localhost:6003/status
Invoke-RestMethod http://localhost:5000/health
Failover Demo
Start all services.
Check current leader from /status.
Stop leader container/process.
Wait ~1 second.
Re-check /status on remaining replicas. A new leader should appear.
Continue drawing; clients should remain connected through gateway.
Notes
Election timeout is randomized in 500-800ms.
Heartbeat interval is 150ms.
Stroke is broadcast to clients only after majority commit.
