# Plinko Lab - Provably Fair Gaming

A deterministic, provably fair Plinko game implementation following the Daphnis Labs assignment specifications.

## 🎮 Live Demo

- **Game**: http://localhost:3001 (frontend)
- **Backend API**: http://localhost:5000 (backend)
- **Verifier Page**: http://localhost:3001/verify

## ✨ Features

### Core Functionality
- **Provably Fair**: Full commit-reveal protocol with SHA-256 hashing
- **Deterministic Engine**: XORShift32 PRNG with reproducible outcomes
- **Interactive UI**: Smooth animations, sound effects, responsive design
- **Accessibility**: Keyboard controls, reduced motion support
- **Verification System**: Public verifier page to validate any round

### Easter Eggs
- **Tilt Mode**: Press 'T' to rotate the board with vintage arcade filter
- **Dark Theme**: Type "open sesame" to toggle torchlight/dungeon theme for one round
- **Keyboard Controls**: Arrow keys to select drop column, Space to drop

## 🏗 Architecture

```
plinko-mern/
├── backend/           # Express.js API server (Port 5000)
│   ├── lib/          # Game engine & PRNG
│   ├── routes/       # API endpoints
│   ├── prisma/       # Database schema
│   └── tests/        # Unit tests
├── frontend/         # React client (Port 3001)  
│   ├── src/
│   │   ├── components/   # Game & UI components
│   │   └── utils/        # Helper functions
│   └── public/
└── package.json      # Root scripts for dev workflow
```

### Technology Stack
- **Frontend**: React 18, JavaScript (converted from Next.js)
- **Backend**: Express.js, Node.js
- **Database**: SQLite + Prisma ORM
- **Testing**: Jest for unit tests
- **PRNG**: XORShift32 algorithm
- **Hashing**: SHA-256 (Node.js crypto module)

## 🚀 Quick Start

### Prerequisites
- Node.js 16+ 
- npm

### Installation & Setup

```bash
# Clone and navigate to project
cd plinko-mern

# Install all dependencies
npm run install:all

# Initialize database
cd backend
npx prisma generate
npx prisma db push
cd ..

# Start both servers in development mode
npm run dev
```

### Individual Server Commands

```bash
# Backend only (port 5000)
npm run server:dev

# Frontend only (port 3001)  
npm run client:dev

# Production builds
npm run build
npm start
```

### Running Tests

```bash
# Backend unit tests
npm test

# Frontend tests  
npm run test:frontend
```

## 🎯 Assignment Compliance

### Provably Fair Protocol ✅

**Commit-Reveal Implementation:**
```javascript
// 1. Server generates commitment
commitHex = SHA256(serverSeed + ":" + nonce)

// 2. Client provides seed
clientSeed = "user-chosen-string"

// 3. Combined seed drives all randomness
combinedSeed = SHA256(serverSeed + ":" + clientSeed + ":" + nonce)
```

**Test Vectors (All Passing):**
- serverSeed: `b2a5f3f32a4d9c6ee7a8c1d33456677890abcdeffedcba0987654321ffeeddcc`
- nonce: `42`
- clientSeed: `candidate-hello`
- **Expected commitHex**: `bb9acdc67f3f18f3345236a01f0e5072596657a9005c7d8a22cff061451a6b34` ✅
- **Expected combinedSeed**: `e1dddf77de27d395ea2be2ed49aa2a59bd6bf12ee8d350c16c008abd406c07e0` ✅
- **Expected binIndex for dropColumn=6**: `6` ✅

### Deterministic Engine ✅

**Peg Map Generation:**
- Formula: `leftBias = 0.5 + (rand() - 0.5) * 0.2` → range [0.4, 0.6]
- 6 decimal place rounding for stable hashing
- Row r has r+1 pegs (triangular layout)

**Ball Physics:**
- Discrete model: track position as "number of right moves"
- Drop column influence: `adj = (dropColumn - 6) * 0.01`
- Decision per row: `if rand() < (leftBias + adj) then LEFT else RIGHT`

**PRNG Sequence (XORShift32):**
- First 5 values from test vector: `0.1106166649, 0.7625129214, 0.0439292176, 0.4578678815, 0.3438999297` ✅

### API Endpoints ✅

```bash
POST /api/rounds/commit          # Create round commitment
POST /api/rounds/:id/start       # Start round with client parameters  
POST /api/rounds/:id/reveal      # Reveal server seed
GET  /api/rounds/:id             # Get round details
GET  /api/verify                 # Public verifier endpoint
```

## 🧪 Testing Results

```bash
✅ 12 unit tests passing
✅ Provably Fair Protocol validation
✅ PRNG deterministic sequence testing  
✅ Engine path reproducibility
✅ Assignment test vectors verification
✅ Integration workflow testing
```

**Key Test Coverage:**
- Commit-reveal hash validation
- PRNG sequence determinism  
- Peg map generation format
- Edge case handling (drop columns 0-12)
- Symmetric payout table validation
- Complete round workflow verification

## 🤖 AI Usage Documentation

### Where AI Was Used
1. **Code Conversion**: Converting Next.js to MERN stack structure
2. **Algorithm Implementation**: XORShift32 PRNG and deterministic engine
3. **Testing Framework**: Unit test generation and edge case coverage
4. **UI Components**: React component structure and animation logic
5. **Documentation**: README structure and API documentation

### What Was Modified
- **Kept**: Core game logic, provably fair protocol, deterministic engine
- **Adapted**: Component structure for standalone React, API endpoints for Express
- **Enhanced**: Error handling, test coverage, documentation completeness

### Time Investment
- **Setup & Architecture**: ~2 hours  
- **Provably Fair Implementation**: ~2 hours
- **Deterministic Engine**: ~2 hours
- **UI/UX & Animation**: ~1.5 hours
- **Testing & Documentation**: ~0.5 hours
- **Total**: ~8 hours

## 🔍 Verification Example

Test the verifier with assignment vectors:

```
Server Seed: b2a5f3f32a4d9c6ee7a8c1d33456677890abcdeffedcba0987654321ffeeddcc
Client Seed: candidate-hello  
Nonce: 42
Drop Column: 6
Expected Bin: 6 ✅
```

Visit http://localhost:3001/verify to validate any round interactively.

## 📝 License

MIT License - Built for Daphnis Labs Technical Assessment

### Frontend (React.js)
- Interactive game interface
- Real-time ball animation
- Responsive design
- Game state management

## Quick Start

### Prerequisites
- Node.js 16+ 
- npm or yarn

### Installation

1. Clone and install dependencies:
```bash
cd plinko-mern
npm run install:all
```

2. Set up the database:
```bash
cd backend
npx prisma generate
npx prisma db push
```

3. Start development servers:
```bash
npm run dev
```

This will start:
- Backend server on http://localhost:5000
- Frontend React app on http://localhost:3000

## Development

### Available Scripts

- `npm run dev` - Start both frontend and backend in development mode
- `npm run server:dev` - Start only the backend server
- `npm run client:dev` - Start only the frontend
- `npm run build` - Build both frontend and backend for production
- `npm run test` - Run backend tests

### Project Structure

```
plinko-mern/
├── backend/                 # Express.js API server
│   ├── routes/             # API route handlers
│   ├── lib/               # Game engine and utilities
│   ├── prisma/            # Database schema
│   └── server.js          # Express server entry point
├── frontend/               # React.js application
│   ├── src/
│   │   ├── components/    # React components
│   │   └── utils/         # Frontend utilities
│   └── public/            # Static assets
└── package.json           # Root package with scripts
```

## API Endpoints

- `POST /api/rounds/commit` - Create round commitment
- `POST /api/rounds/:id/start` - Start game round
- `POST /api/rounds/:id/reveal` - Reveal round for verification
- `GET /api/rounds/:id` - Get round details
- `GET /api/verify` - Verify game round

## Game Logic

The game implements a provably fair system:

1. **Commit Phase**: Server generates seed and commitment hash
2. **Input Phase**: Player provides client seed and bet
3. **Execution Phase**: Deterministic outcome calculated
4. **Reveal Phase**: Server seed revealed for verification

## Technologies Used

- **Backend**: Node.js, Express.js, Prisma, SQLite
- **Frontend**: React.js, HTML5, CSS3, JavaScript
- **Game Engine**: Custom deterministic Plinko simulation
- **Cryptography**: SHA-256 for provably fair verification

## License

MIT License