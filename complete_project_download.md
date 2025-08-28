# 🛒 Voice Shopping Assistant - Complete Project Package

## 📥 Quick Setup Instructions

### Method 1: Automated Setup (Recommended)
```bash
# 1. Create project directory
mkdir voice-shopping-assistant && cd voice-shopping-assistant

# 2. Copy and paste the files below into their respective locations

# 3. Run the setup script
chmod +x start.sh && ./start.sh
```

### Method 2: Manual Setup
```bash
# 1. Create project structure
mkdir -p voice-shopping-assistant/{client/{src,public},server,docs}

# 2. Copy all files from sections below

# 3. Install dependencies
cd voice-shopping-assistant
npm install && cd client && npm install && cd ../server && npm install && cd ..

# 4. Start development
npm run dev
```

---

## 📁 Complete File Structure

```
voice-shopping-assistant/
├── 📄 README.md
├── 📄 package.json
├── 🔧 start.sh
├── 🔧 deploy.sh
├── 📄 .env.example
├── 📄 .gitignore
├── 📄 Dockerfile
├── 📂 client/
│   ├── 📄 package.json
│   ├── 📂 public/
│   │   ├── 📄 index.html
│   │   ├── 📄 manifest.json
│   │   └── 🖼️ favicon.ico
│   └── 📂 src/
│       ├── 📄 index.js
│       ├── 📄 App.js
│       └── 📄 index.css
├── 📂 server/
│   ├── 📄 package.json
│   └── 📄 server.js
└── 📂 docs/
    ├── 📄 API.md
    └── 📄 DEPLOYMENT.md
```

---

## 🔧 Root Files

### start.sh
```bash
#!/bin/bash
echo "🛒 Voice Shopping Assistant - Quick Setup"
echo "========================================"

# Colors for output
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
BLUE='\033[0;34m'
NC='\033[0m' # No Color

# Function to print colored output
print_status() {
    echo -e "${GREEN}✅ $1${NC}"
}

print_error() {
    echo -e "${RED}❌ $1${NC}"
}

print_warning() {
    echo -e "${YELLOW}⚠️  $1${NC}"
}

print_info() {
    echo -e "${BLUE}ℹ️  $1${NC}"
}

# Check for Node.js
if ! command -v node &> /dev/null; then
    print_error "Node.js is not installed!"
    print_info "Please install Node.js from https://nodejs.org/"
    exit 1
fi

# Check for npm
if ! command -v npm &> /dev/null; then
    print_error "npm is not installed!"
    exit 1
fi

print_status "Node.js and npm are installed"

# Check Node version
NODE_VERSION=$(node -v | cut -d'v' -f2 | cut -d'.' -f1)
if [ "$NODE_VERSION" -lt 16 ]; then
    print_warning "Node.js version $NODE_VERSION detected. Recommended: 18+"
fi

# Create project structure if needed
print_info "Creating project structure..."
mkdir -p client/{src,public}
mkdir -p server
mkdir -p docs

# Install dependencies
print_info "Installing dependencies..."

# Root package.json if not exists
if [ ! -f "package.json" ]; then
    cat > package.json << 'EOF'
{
  "name": "voice-shopping-assistant",
  "version": "1.0.0",
  "description": "Voice Command Shopping Assistant",
  "scripts": {
    "install:all": "npm install && cd client && npm install && cd ../server && npm install",
    "dev": "concurrently \"npm run server\" \"npm run client\"",
    "client": "cd client && npm start",
    "server": "cd server && npm run dev",
    "build": "cd client && npm run build",
    "start": "cd server && npm start",
    "deploy": "./deploy.sh"
  },
  "keywords": ["voice", "shopping", "react", "express"],
  "license": "MIT",
  "devDependencies": {
    "concurrently": "^8.2.0"
  }
}
EOF
fi

# Install root dependencies
npm install

# Client setup
if [ -d "client" ]; then
    print_info "Setting up client..."
    cd client
    if [ ! -f "package.json" ]; then
        print_info "Creating client package.json..."
        # Client dependencies will be created by the files below
    fi
    npm install 2>/dev/null || print_warning "Client install failed - check package.json"
    cd ..
fi

# Server setup
if [ -d "server" ]; then
    print_info "Setting up server..."
    cd server
    if [ ! -f "package.json" ]; then
        print_info "Creating server package.json..."
        # Server dependencies will be created by the files below
    fi
    npm install 2>/dev/null || print_warning "Server install failed - check package.json"
    cd ..
fi

# Create .env file
if [ ! -f ".env" ]; then
    print_info "Creating .env file..."
    cat > .env << 'EOF'
NODE_ENV=development
PORT=5000
CLIENT_URL=http://localhost:3000
EOF
fi

# Set permissions
chmod +x start.sh
chmod +x deploy.sh 2>/dev/null || true

print_status "Setup complete!"
echo ""
echo "🚀 Available Commands:"
echo "   npm run dev      - Start development servers"
echo "   npm run build    - Build for production"  
echo "   npm start        - Start production server"
echo "   ./deploy.sh      - Deploy to cloud"
echo ""
echo "📱 Application will be available at:"
echo "   Frontend: http://localhost:3000"
echo "   Backend:  http://localhost:5000"
echo ""

# Ask user if they want to start dev servers
read -p "🔥 Start development servers now? (y/n): " -n 1 -r
echo
if [[ $REPLY =~ ^[Yy]$ ]]; then
    print_info "Starting development servers..."
    npm run dev
else
    print_info "Run 'npm run dev' when you're ready to start!"
fi
```

### package.json
```json
{
  "name": "voice-shopping-assistant",
  "version": "1.0.0",
  "description": "Voice Command Shopping Assistant - AI-powered shopping list with voice recognition",
  "main": "server/server.js",
  "scripts": {
    "install:all": "npm install && cd client && npm install && cd ../server && npm install",
    "dev": "concurrently --kill-others \"npm run server\" \"npm run client\"",
    "client": "cd client && npm start",
    "server": "cd server && npm run dev",
    "build": "cd client && npm run build",
    "start": "cd server && npm start",
    "test": "cd client && npm test",
    "test:server": "cd server && npm test",
    "deploy": "./deploy.sh",
    "heroku-postbuild": "cd client && npm install && npm run build && cd ../server && npm install"
  },
  "keywords": [
    "voice-recognition",
    "shopping-list",
    "react",
    "express",
    "nlp",
    "speech-to-text",
    "ai-assistant"
  ],
  "author": "Your Name <your.email@example.com>",
  "license": "MIT",
  "repository": {
    "type": "git",
    "url": "https://github.com/yourusername/voice-shopping-assistant.git"
  },
  "engines": {
    "node": ">=16.0.0",
    "npm": ">=8.0.0"
  },
  "devDependencies": {
    "concurrently": "^8.2.0"
  }
}
```

### deploy.sh
```bash
#!/bin/bash
echo "🚀 Deploying Voice Shopping Assistant"
echo "===================================="

# Colors
GREEN='\033[0;32m'
BLUE='\033[0;34m'
YELLOW='\033[1;33m'
NC='\033[0m'

print_status() {
    echo -e "${GREEN}✅ $1${NC}"
}

print_info() {
    echo -e "${BLUE}ℹ️  $1${NC}"
}

print_warning() {
    echo -e "${YELLOW}⚠️  $1${NC}"