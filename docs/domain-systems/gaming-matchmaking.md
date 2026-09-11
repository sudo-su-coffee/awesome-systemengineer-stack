# Multiplayer Gaming Architecture (PUBG/Valorant)

If you are building a real-time multiplayer game or a highly competitive E-Sports app, the architecture is entirely about latency, cheating prevention, and tick rates.

## 1. UDP Networking & State Interpolation
Like video conferencing, games use **UDP**, not TCP, because speed is more important than perfect packet delivery.
- **Tick Rates**: A server running at 64-tick calculates the entire game world 64 times every single second.
- **Interpolation**: If a user has a bad connection and misses a packet, their app mathematically "guesses" where the enemy player should be moving based on their last known velocity, ensuring the game looks smooth even on 3G internet.

## 2. Authoritative Servers (Anti-Cheat)
If the client (the mobile app) tells the server, *"I just shot the enemy in the head"*, the server must **never** trust it. Hackers easily modify the app to send fake "headshot" commands.
- **The Architecture**: The mobile app only sends *inputs* (e.g., "I clicked the mouse"). The **Authoritative Server** calculates the bullet trajectory. The server decides if it was a headshot, and broadcasts the result back to everyone. The client is just a "dumb screen".

## 3. Matchmaking Queues (Redis Sorted Sets)
When 100,000 players click "Find Match", how do you instantly group them by skill level (ELO/MMR)?
- You do not use Postgres `ORDER BY mmr`. It's too slow.
- You use a **Redis Sorted Set (`ZSET`)**.
- As players search, they are dumped into a Redis ZSET scored by their MMR. A Go worker constantly scans this set in memory, instantly slices off 10 players who have similar scores, removes them from the queue, and spawns a dedicated Game Server for them in milliseconds.
