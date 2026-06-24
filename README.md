# OWN AMLI - Complete Setup Instructions

## ❌ Current Issue
**MongoDB is NOT running** on `localhost:27017`. This is why user registration fails.

---

## 📋 Dependencies Overview

| Component | Port | Status | Required Files |
|-----------|------|--------|-----------------|
| **MongoDB** | 27017 | ❌ NOT RUNNING | - |
| **Redis** | 19474 (Cloud) | ✅ Connected | `.env` (backend) |
| **Backend (Node.js/Express)** | 8080 | ✅ Running | `backend/` |
| **Frontend (Vite/React)** | 5173 | ✅ Running | `frontend/` |

---

## 🔧 Setup Steps

### **Step 1: Install MongoDB Community Edition**

#### **Windows Installation:**

1. **Download MongoDB**
   - Go to: https://www.mongodb.com/try/download/community
   - Select version: **6.0 or latest LTS**
   - Select OS: **Windows (MSI)**
   - Click Download

2. **Install MongoDB**
   - Run the `.msi` installer
   - Choose "Complete" installation
   - Check "Install MongoDB as a Service" ✅
   - Installation path: Keep default `C:\Program Files\MongoDB\Server\6.0\`
   - Finish installation

3. **Verify Installation**
   ```powershell
   # In any terminal/PowerShell, run:
   mongosh --version
   ```
   Should output version number like: `1.x.x`

---

### **Step 2: Start MongoDB Service**

#### **Option A: Automatic (Recommended)**
MongoDB should start automatically as a Windows Service.

```powershell
# Verify MongoDB is running:
Get-Service MongoDB | Select-Object Status
# Should show: Status : Running
```

#### **Option B: Manual Start**
```powershell
# Start MongoDB service:
Start-Service MongoDB

# Verify:
Get-Service MongoDB
```

#### **Option C: Using MongoDB CLI**
```powershell
# In a new PowerShell window:
mongod
# Should show: "waiting for connections on port 27017"
```

---

### **Step 3: Verify Database Connection**

```powershell
# In a new terminal, run:
mongosh
```

You should see:
```
MongoDB shell version v1.x.x
connecting to: mongodb://127.0.0.1:27017/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongosh
...
test>
```

If successful, type `exit` to quit.

---

### **Step 4: Run All Servers**

#### **Terminal 1: Backend Server**
```powershell
cd "c:\Users\Akhil Anand\Documents\OWN_AMLI\backend"
npm start
```
Expected output:
```
✅ ENV LOADED: mongodb://127.0.0.1:27017/OWN_AMLI
Server is running on port 8080
Redis Connected
```

#### **Terminal 2: Frontend Server**
```powershell
cd "c:\Users\Akhil Anand\Documents\OWN_AMLI\frontend"
npm run dev
```
Expected output:
```
VITE v8.0.7 ready in XXXXms
  ➜  Local:   http://localhost:5173/
```

---

## 🚀 Full Terminal Setup Sequence

### **Terminal 1 - MongoDB**
```powershell
# Start MongoDB (if not running as service)
mongod
# Keeps running in background
```

### **Terminal 2 - Backend**
```powershell
cd "c:\Users\Akhil Anand\Documents\OWN_AMLI\backend"
npm start
```

### **Terminal 3 - Frontend**
```powershell
cd "c:\Users\Akhil Anand\Documents\OWN_AMLI\frontend"
npm run dev
```

---

## ✅ How to Test Registration

1. Open browser: **http://localhost:5173**
2. Click "Create one" link (or go to /register)
3. Enter:
   - Email: `test1@example.com`
   - Password: `password123`
   - Confirm Password: `password123`
4. Click "Create Account"

**Success indicators:**
- ✅ No error messages
- ✅ Redirects to Home page
- ✅ Shows email in header
- ✅ Can see "Your Projects" section

---

## 🐛 Troubleshooting

### **"Connection refused" Error**
**Solution:** MongoDB is not running
```powershell
# Start MongoDB:
Start-Service MongoDB
# Wait 5 seconds, then refresh browser
```

### **"Address already in use" on port 8080**
**Solution:** Another process is using port 8080
```powershell
# Find and kill process on port 8080:
netstat -ano | findstr ":8080"
taskkill /PID <PID_NUMBER> /F
```

### **"Address already in use" on port 5173**
**Solution:** Another process is using port 5173
```powershell
# Kill process:
netstat -ano | findstr ":5173"
taskkill /PID <PID_NUMBER> /F
```

### **Redis Connection Error**
**Solution:** This is expected if cloud Redis is down. Backend still works locally.
The cloud Redis is used for session storage but has fallback.

---

## 📦 Dependencies Summary

### **Backend** (`backend/package.json`)
- Express.js - HTTP server
- Mongoose - MongoDB ODM
- JWT - Authentication
- Bcrypt - Password hashing
- Redis (ioredis) - Session storage
- Socket.io - Real-time chat
- Google Generative AI - AI integration

### **Frontend** (`frontend/package.json`)
- React - UI framework
- Vite - Build tool
- Tailwind CSS - Styling
- React Router - Navigation
- Axios - HTTP client
- Socket.io-client - Real-time chat
- Markdown-to-JSX - Markdown rendering
- Highlight.js - Code syntax highlighting

---

## 🔑 Environment Variables

### **Backend** (`backend/.env`)
```
PORT=8080
MONGODB_URI=mongodb://127.0.0.1:27017/OWN_AMLI
JWT_SECRET=own_amli_secret
CLIENT_URL=http://localhost:5173

REDIS_HOST=redis-19474.crce179.ap-south-1-1.ec2.cloud.redislabs.com
REDIS_PORT=19474
REDIS_PASSWORD=qcxVDgvzUKuG0EO06fyUHelPV7ZEQYop

GOOGLE_AI_KEY=AIzaSyBZM8b9hDv3HTUmb7ku-E-nBDzbm5jqekU
```

### **Frontend** (`frontend/.env`)
```
VITE_API_URL=http://localhost:8080
```

---

## ✨ Everything Should Work Once MongoDB is Running!

After starting all three services (MongoDB, Backend, Frontend):
- ✅ Register new accounts
- ✅ Login with credentials
- ✅ Create projects
- ✅ Add collaborators
- ✅ Chat with AI
- ✅ Edit code files
- ✅ Real-time collaboration

---

## 📞 Quick Commands Reference

```powershell
# Check MongoDB status:
Get-Service MongoDB | Select-Object Status

# Start MongoDB:
Start-Service MongoDB

# Stop MongoDB:
Stop-Service MongoDB

# View MongoDB logs:
Get-EventLog Application -Source MongoDB | Select-Object TimeGenerated, Message | head -20

# Test connection:
mongosh --eval "db.version()"

# Kill process on port:
netstat -ano | findstr ":PORT_NUMBER"
taskkill /PID <PID> /F
```

---

**Last Updated:** April 17, 2026  
**MongoDB Status Required:** ✅ RUNNING on port 27017
