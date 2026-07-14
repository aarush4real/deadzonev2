# Deadzone v2 - Multiplayer FPS Game

A multiplayer 3D first-person shooter browser game inspired by Deadshot.io, built with HTML5, JavaScript, Three.js, Node.js, Socket.io, and MongoDB.

## Roadmap

### Phase 1: Project Setup
- [x] Initialize repository structure
- [x] Set up client (Three.js) and server (Node.js/Socket.io) directories
- [x] Configure basic HTTP and WebSocket servers
- [x] Create shared constants for game mechanics

### Phase 2: Core Game Mechanics
- [ ] Implement player movement (walking, jumping, sliding) with collision detection
- [ ] Implement aiming and shooting (hit-scan and projectile weapons)
- [ ] Create dynamic crosshairs and HUD
- [ ] Implement instant respawn system
- [ ] Develop basic 3D map with environmental collision

### Phase 3: Multiplayer Synchronization
- [ ] Implement client-side prediction for smooth movement
- [ ] Implement server reconciliation for authority
- [ ] Add lag compensation (interpolation and extrapolation)
- [ ] Synchronize player states (position, rotation, health, ammo)
- [ ] Implement hit detection and damage application

### Phase 4: Persistence and Progression
- [ ] Set up MongoDB database for player profiles
- [ ] Implement account system (registration, login, JWT auth)
- [ ] Save match statistics, level progression, and unlocked cosmetics
- [ ] Create cosmetic system (skins, weapons, accessories)

### Phase 5: Polish and Deployment
- [ ] Refine UI/UX with modern aesthetics (main menu, leaderboards, kill feed)
- [ ] Optimize performance for various browsers and devices
- [ ] Implement anti-cheat measures (basic validation)
- [ ] Deploy client to Vercel, server to a WebSocket-compatible platform (e.g., Railway, AWS)
- [ ] Set up CI/CD pipeline for automated builds

### Phase 6: Post-Launch
- [ ] Add game modes (free-for-all, team deathmatch, capture the flag)
- [ ] Implement matchmaking and lobbies
- [ ] Add social features (friends, clans,compatible platform (e.g., Railway, AWS)
- [ ] Regular content updates and balance patches

## Project Structure

```
deadzonev2/
├── client/                 # Client-side code (runs in browser)
│   ├── public/             # Static assets served to browser
│   │   ├── index.html      # Main HTML file
│   │   ├── style.css       # CSS styles
│   │   └── client.js       # Entry point for client JavaScript
│   ├── src/                # Source code (if using build step)
│   │   ├── game/           # Game logic (movement, shooting, etc.)
│   │   ├── networking/     # Socket.io client wrapper
│   │   ├── rendering/      # Three.js scene setup and rendering
│   │   └── ui/             # HUD, menus, overlays
│   └── package.json        # Client dependencies and scripts
├── server/                 # Server-side code (runs on Node.js)
│   ├── index.js            # Server entry point
│   ├── networking/         # Socket.io event handlers
│   ├── game/               # Game state management and physics
│   ├── auth/               # Authentication and authorization
│   ├── database/           # MongoDB connection and models
│   └── package.json        # Server dependencies and scripts
├── shared/                 # Code shared between client and server
│   └── constants.js        # Game constants (speeds, sizes, etc.)
├── vercel.json             # Vercel configuration for client deployment
├── .gitignore              # Git ignore rules
└── README.md               # This file
```

## Lag Compensation Techniques

To ensure a seamless, lag-free gameplay experience, Deadzone v2 implements several lag compensation techniques:

### 1. Client-Side Prediction
- The client predicts the outcome of its own actions (movement, shooting) immediately without waiting for server confirmation
- This creates a responsive feel for local actions
- When the server's authoritative state arrives, the client reconciles any differences

### 2. Server Reconciliation
- The server is the ultimate authority for game state
- When there's a discrepancy between client prediction and server state, the client corrects its position
- This prevents cheating while maintaining smooth gameplay

### 3. Interpolation
- For rendering other players, the client interpolates between known positions to create smooth movement
- Instead of jumping directly to new positions, players appear to glide between updates
- This masks the effects of network latency

### 4. Extrapolation (with limits)
- For very short periods when updates are delayed, the client may extrapolate based on last known velocity
- This is used sparingly to avoid "rubber-banding" effects

### 5. Lag Compensation for Hit Detection
- When determining if a shot hit another player, the server rewinds time to check positions at the moment the shot was fired
- This ensures that players with high ping aren't disadvantaged when shooting

## Getting Started

### Prerequisites
- Node.js (v18+)
- MongoDB (local instance or MongoDB Atlas)
- Git

### Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/aarush4real/deadzonev2.git
   cd deadzonev2
   ```

2. Install client dependencies:
   ```bash
   cd client
   npm install
   cd ..
   ```

3. Install server dependencies:
   ```bash
   cd server
   npm install
   cd ..
   ```

4. Set up environment variables:
   - Create `.env` in the server directory:
     ```
     PORT=3000
     MONGODB_URI=mongodb://localhost:27017/deadzone
     JWT_SECRET=your_jwt_secret_here
     ```
   - Adjust values as needed for your environment.

5. Start MongoDB (if running locally):
   ```bash
   mongod
   ```

6. Start the server:
   ```bash
   cd server
   npm start
   # Server will run on http://localhost:3000
   ```

7. Start the client (in a new terminal):
   ```bash
   cd client
   npm start
   # Client will be available at http://localhost:3000 (if using Vercel dev server)
   # Or open client/public/index.html directly in browser
   ```

## Deployment

### Client (Vercel)
The client is configured for deployment to Vercel using the provided `vercel.json`.
To deploy:
1. Install Vercel CLI: `npm i -g vercel`
2. Login: `vercel login`
3. Deploy: `vercel` (from the project root)

### Server
The server requires a platform that supports persistent WebSocket connections.
Recommended options:
- Railway.app
- Render.com
- AWS Elastic Beanstalk
- DigitalOcean App Platform
- Traditional VPS (Ubuntu/Debian with PM2)

Deploy the server code to your chosen platform, set the environment variables, and ensure MongoDB is accessible.

## License

This project is licensed under the MIT License.

## Acknowledgments

- Inspired by Deadshot.io
- Built with Three.js, Socket.io, Node.js, and MongoDB
