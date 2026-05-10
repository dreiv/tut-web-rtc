# WebRTC Lab: Peer-to-Peer Communication

A hands-on exploration of decentralized, real-time communication using **WebRTC** and **PeerJS**. This project demonstrates how to bypass a central message broker to establish direct, low-latency "data pipes" between browsers.

---

## 🚀 Getting Started

### Prerequisites

- [Node.js](https://nodejs.org/) (v20+ recommended)
- [npm](https://www.npmjs.com/)

### Installation

```bash
# Install the local static server
npm install

```

### Running the Project

```bash
# Launch the local environment
npm run dev

```

1. Open the URL provided (usually `http://localhost:3000`) in two different browser windows.
2. Enter a display name in both (this is saved to your browser's **LocalStorage** for persistence).
3. Copy the **Peer ID** from Window A and paste it into the connection field in Window B.
4. Click **Connect** to establish the direct P2P link.

---

## 📚 The Evolution from WebSockets to P2P

While our previous project relied on a central `server.ts` to broadcast messages, this project moves to a **Mesh Topology**.

| Feature         | WebSocket Project               | WebRTC Project (Current)                 |
| --------------- | ------------------------------- | ---------------------------------------- |
| **Server Role** | Central Hub & Message Broker    | Initial "Matchmaker" (Signaling) only.   |
| **Data Path**   | Client → Server → Client        | **Client ↔ Client (Direct).**            |
| **Latency**     | Medium (Server processing time) | **Ultra-Low** (Direct transmission).     |
| **Persistence** | Server stores last 100 messages | **None.** History exists only in memory. |

---

## 🛠️ Key Technical Features

### 1. PeerJS Signaling

We utilize the PeerJS cloud to handle the "Signaling" phase. The WebSocket is used only to exchange the **Offer** and **Answer** required to "handshake" two browsers. Once connected, the WebSocket sits idle.

### 2. Identity Handshake & LocalStorage

Because WebRTC connections are anonymous by default, we implemented a custom identity layer:

- **Persistence**: Your chosen name is saved to `localStorage`, so it persists across page reloads.
- **P2P Names**: Upon connection, peers exchange a `{ type: 'identity', name: '...' }` packet to map random Peer IDs to human-readable nicknames.

### 3. Mesh Networking

The application maintains an internal `connections` array. When you send a message, the app iterates through every active "pipe" and sends the data directly to each peer.

---

## 💡 Learning Objectives

Observe the difference in networking behavior compared to standard WebSockets:

- **The Signaling/Data Split**: In the Network Tab (`F12`), you will see a WebSocket to `0.peerjs.com`. This is the "Librarian" that helps you find your friend. The actual chat data does not go through this connection.
- **NAT Traversal**: The library uses **STUN/TURN** servers to navigate through home routers and firewalls to find a direct path to the other browser.
- **Serverless Scaling**: Notice that you no longer need complex server-side logic (like `wss.on('message')`) to handle chat routing. Each browser acts as its own server.

---

### Pro-Tip: Local Development Workflow

Even though the project is decentralized, we use the `serve` dev-dependency to provide a standard `npm run dev` experience. This replaces the `tsx watch` workflow used in the previous WebSocket lab.
