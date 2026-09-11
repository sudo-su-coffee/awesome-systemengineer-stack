# Real-Time Collaboration & EdTech (Figma/Zoom)

Building apps where multiple users edit or interact at the exact same time (like Figma, Google Docs, or Zoom) requires solving extreme mathematical concurrency problems.

## 1. Multiplayer Editing (CRDTs)
If User A and User B are editing the exact same paragraph in a Google Doc, how does the server prevent them from deleting each other's words?
- **The Old Way**: Operational Transformation (OT). It requires a central server to constantly resolve conflicts, which introduces lag.
- **The Modern Way**: **CRDTs (Conflict-free Replicated Data Types)**. Figma uses this. It is a mathematical algorithm where every single keystroke is assigned a unique vector clock. The users' computers sync these vectors directly via WebSockets. If two people type at the exact same millisecond, the math guarantees that both screens will eventually look exactly the same without the central server having to "guess" who was right.

## 2. WebRTC (Video Conferencing)
When you build a telemedicine app or Zoom clone, you do NOT send video through your Go backend (HTTP/TCP).
- HTTP/TCP guarantees packet delivery. If a video frame drops, the network freezes the whole stream to re-send that frame, causing extreme lag.
- **The Architecture**: You use **WebRTC over UDP**. UDP just throws packets at the other person as fast as possible. If a frame drops, the video glitches for a millisecond, but the conversation doesn't lag.
- The Go backend acts only as a **Signaling Server**: It introduces User A's IP address to User B's IP address. Once introduced, the video stream goes directly from phone-to-phone (Peer-to-Peer), meaning your AWS bandwidth costs remain at $0.

## 3. The SFU (Selective Forwarding Unit)
Peer-to-Peer is great for 2 people. But what if it's a 50-person classroom?
- A phone cannot upload 50 video streams simultaneously; it will melt.
- **The Fix**: You deploy an **SFU Server**. The teacher uploads exactly 1 video stream to the SFU, and the SFU clones and routes that stream down to the 50 students.
