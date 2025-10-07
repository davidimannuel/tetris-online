# Arsitektur Multiplayer Tetris Game Online dengan Ebiten

## Overview
Arsitektur sederhana untuk permainan Tetris multiplayer real-time menggunakan Ebiten game engine dengan Redis sebagai database utama.

## Komponen Utama

### 1. **Client Layer (Ebiten-based)**
- **Game Client** (Go/Ebiten)
- Cross-platform: Desktop, Web, Mobile
- WebSocket connection untuk multiplayer

### 2. **Backend Service**
#### **Tetris Server** (Single Unified Backend)
- Real-time WebSocket communication (`/ws`)
- Room creation & management  
- Core game logic & validation
- Attack mechanics (25 damage per line cleared)
- Single binary deployment pada port 8080

### 3. **Data Storage**
#### **Redis Cache**
- Active game sessions & real-time game state
- Player data (username, health, score)  
- Room data & metadata

## Flow Komunikasi

### **Room Creation/Join Flow**
- Player 1 connects → auto-creates room → gets room code
- Player 2 connects → joins room with code
- When 2 players → game auto-starts

### **Game Session Flow**  
- Both players start with 100 health
- Line clears → 25 damage to opponent
- Game ends when: health = 0 OR traditional game over

### **Data Flow**
- All data stored in Redis (temporary)
- Real-time game state synchronization

## Technology Stack

- **Go**: Backend server language
- **Ebiten**: Cross-platform 2D game engine
- **WebSocket**: Real-time communication
- **Redis**: Database
- **Docker**: Deployment

## Deployment

```bash
# Start everything
make setup

# Run multiplayer client  
make run-multi
```

## Game Flow

1. **Player opens game**
2. **Creates room → gets room code OR joins with code**
3. **When 2 players → game auto-starts**
4. **Play Tetris + attack system (line clears = damage)**
5. **Winner displayed → back to main menu**

**Total Services:** 2 (Tetris Server + Redis)  
**Total Ports:** 2 (8080 + 6379)