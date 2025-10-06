# Arsitektur Multiplayer Tetris Game Online dengan Ebiten

## Overview
Arsitektur ini dirancang untuk mendukung permainan Tetris multiplayer real-time yang dapat berjalan di berbagai platform menggunakan Ebiten game engine. Sistem ini mengadopsi pendekatan cross-platform dengan satu codebase Go yang dapat di-deploy ke desktop, web, dan mobile.

## Komponen Utama

### 1. **External Actors**
- **Player 1 & Player 2**: Pengguna yang bermain game Tetris secara bersamaan dalam sesi multiplayer

### 2. **Platform Layer**
- **Desktop Platform**: Windows, macOS, Linux - menjalankan executable native
- **Web Platform**: Browser dengan dukungan WebAssembly untuk menjalankan game
- **Mobile Platform**: Android/iOS devices dengan aplikasi native

### 3. **Client Layer (Ebiten-based)**

#### **Desktop Game Client** (Go/Ebiten)
- Native desktop client dengan full Tetris logic dan rendering
- Performa optimal karena berjalan langsung di OS
- Mendukung keyboard dan mouse input
- Executable tunggal tanpa dependencies eksternal

#### **Web Game Client** (Go/Ebiten/WASM)
- WebAssembly-compiled client untuk browser
- Same codebase dengan desktop client
- Cross-browser compatibility
- Akses mudah tanpa instalasi

#### **Mobile Game Client** (Go/Ebiten/Mobile)
- Mobile-optimized client untuk Android/iOS
- Touch input support
- Native performance dengan Go mobile bindings
- Distribusi melalui app store

### 4. **Network Layer**
#### **Network Client** (Go/WebSocket)
- Modul komunikasi multiplayer yang terintegrasi dalam semua client
- Handles real-time communication dengan server
- Protocol abstraction untuk berbagai platform
- Connection management dan reconnection logic

### 5. **Backend Services**

#### **API Gateway** (Go/Gin)
- Entry point untuk semua HTTP requests
- Authentication dan authorization
- Rate limiting untuk mencegah abuse
- Request routing ke service yang appropriate

#### **WebSocket Server** (Go/Gorilla WebSocket)
- Real-time bidirectional communication
- Handles game state synchronization
- Low-latency message broadcasting
- Connection lifecycle management

#### **Game Server** (Go)
- Core game logic implementation
- Tetris rules enforcement
- Game state validation
- Matchmaking algorithms
- Session management

#### **Room Manager** (Go)
- Manages game rooms dan player sessions
- Room creation, joining, leaving
- Player capacity management
- Room lifecycle (creation to termination)

### 6. **Data Storage**

#### **Redis Cache**
- Stores active game sessions
- Real-time player states
- Room data dan metadata
- Fast access untuk game state

#### **PostgreSQL Database**
- User profiles dan authentication data
- Game history dan statistics
- Leaderboards dan rankings
- Persistent data storage

#### **Monitoring Service** (Prometheus/Grafana)
- System metrics collection
- Performance monitoring
- Real-time dashboards
- Alerting untuk system issues

## Flow Komunikasi

### 1. **Player Connection Flow**
- Player memilih platform (desktop, web, atau mobile)
- Client application diluncurkan di platform masing-masing
- Client menggunakan Network Client untuk koneksi ke backend

### 2. **Authentication Flow**
- Network Client mengirim request ke API Gateway
- API Gateway memverifikasi credentials dengan PostgreSQL
- JWT token diberikan untuk subsequent requests

### 3. **Game Session Flow**
- Player request untuk join game melalui API Gateway
- Room Manager creates atau assigns player ke room
- Game Server initializes game state
- WebSocket connection established untuk real-time communication

### 4. **Real-time Gameplay Flow**
- Player input (move, rotate, drop) dikirim via WebSocket
- Game Server validates dan updates game state
- Updated state di-broadcast ke semua players dalam room
- Network Client menerima updates dan mengupdate local game state
- Ebiten client renders updated game state

### 5. **Data Persistence Flow**
- Game events disimpan di Redis untuk fast access
- Game completion data disimpan di PostgreSQL
- Statistics dan leaderboards diupdate secara asynchronous

## Keunggulan Arsitektur

### **Cross-Platform Compatibility**
- Single codebase untuk semua platform
- Consistent gameplay experience
- Reduced development dan maintenance effort

### **Real-time Performance**
- WebSocket untuk low-latency communication
- Redis caching untuk fast data access
- Optimized game state synchronization

### **Scalability**
- Microservices architecture untuk independent scaling
- Redis clustering untuk session data
- PostgreSQL untuk reliable data persistence

### **Monitoring & Observability**
- Comprehensive metrics collection
- Real-time performance monitoring
- Proactive issue detection

### **Technology Stack Benefits**
- **Go**: High performance, concurrent programming
- **Ebiten**: Cross-platform 2D game engine dengan excellent performance
- **WebSocket**: Real-time bidirectional communication
- **Redis**: High-speed caching dan session storage
- **PostgreSQL**: Reliable relational database

## Deployment Strategy

### **Desktop**
- Single executable file per platform
- No external dependencies required
- Distribution melalui website atau package managers

### **Web**
- WebAssembly files hosted di web server
- CDN distribution untuk global access
- Progressive loading untuk optimal user experience

### **Mobile**
- Native apps untuk Android dan iOS
- App store distribution
- Platform-specific optimizations

Arsitektur ini memberikan foundation yang solid untuk mengembangkan Tetris multiplayer yang responsive, scalable, dan dapat diakses di berbagai platform dengan maintenance effort yang minimal.