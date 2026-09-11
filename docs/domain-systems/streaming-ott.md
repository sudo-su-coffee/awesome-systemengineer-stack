# OTT & Video Streaming Architecture (Netflix/Spotify)

If you are building a streaming platform, you are dealing with massive bandwidth costs and piracy. You cannot simply serve an `.mp4` file from a normal Go server.

## 1. Adaptive Bitrate Streaming (ABR)
If a user is watching a movie on a train, their 4G connection will fluctuate wildly. 
- You do not send them a 4K `.mp4` file. If the network drops, the video buffers forever.
- **The Architecture**: Your Go backend triggers an FFmpeg worker to slice the master video into thousands of 2-second chunks across 5 different qualities (4K, 1080p, 720p, 480p).
- As the user watches, the video player (like ExoPlayer) constantly measures their internet speed. If the train enters a tunnel, the player seamlessly requests the 480p chunks. When they exit the tunnel, it switches back to 1080p. There is ZERO buffering.

## 2. Multi-CDN Edge Caching
Netflix accounts for 15% of all global internet traffic. If everyone pulled video directly from AWS US-East, the internet would crash.
- **Open Connect**: Streaming giants put physical server boxes (Edge Nodes) inside your local ISP's data center (e.g., inside the Airtel/Jio building in Bangalore).
- When a user in Bangalore clicks "Play" on *Stranger Things*, the video doesn't come from America. It comes from a server located 2 kilometers away from their house, ensuring blazing fast startup times.

## 3. DRM (Digital Rights Management)
If you just serve video chunks, someone will write a Python script to download and steal them.
- You must encrypt the video streams using **Widevine (Google)** or **FairPlay (Apple)**. 
- Even if a hacker intercepts the video chunks, they are mathematically scrambled. Only the secure hardware chip inside the user's phone or Smart TV can decrypt and display the video.
