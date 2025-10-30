# 🚀 Quick Vercel Deployment Checklist

## ✅ Pre-Deployment Checklist

### 1. Frontend Ready?
- [ ] `npm run build` works without errors
- [ ] Environment variable configured (`.env.local`)
- [ ] API URL uses environment variable
- [ ] All dependencies in `package.json`

### 2. Backend Ready?
- [ ] All dependencies in `requirements.txt`
- [ ] CORS settings include production URL
- [ ] Environment variables documented
- [ ] Gemini API key ready

### 3. GitHub Ready?
- [ ] Code pushed to GitHub
- [ ] `.gitignore` excludes sensitive files
- [ ] `.env` files NOT committed
- [ ] README updated

---

## 🎯 Deployment Steps (10 minutes)

### Step 1: Deploy Frontend to Vercel (5 min)
1. Go to https://vercel.com → Login
2. "New Project" → Import your GitHub repo
3. Settings:
   - Root Directory: `frontend`
   - Framework: Vite
   - Build Command: `npm run build`
   - Output: `dist`
4. Environment Variable:
   - `VITE_API_URL` = `http://localhost:8000` (temporary)
5. Deploy!

Result: `https://your-app.vercel.app` ✅

---

### Step 2: Deploy Backend (Choose ONE)

#### Option A: Railway (Best for ML) - 5 min
1. https://railway.app → Sign up
2. "New Project" → "Deploy from GitHub"
3. Select repo
4. Settings:
   - Root: `backend`
   - Start: `uvicorn main:app --host 0.0.0.0 --port $PORT`
5. Add variables:
   - `gemini_api_key` = YOUR_KEY
   - `PORT` = 8000
6. Deploy!

Result: `https://your-backend.railway.app` ✅

#### Option B: Use ngrok (Easiest for Demo) - 2 min
```bash
# Terminal 1: Start backend
cd backend
python3 -m uvicorn main:app --host 0.0.0.0 --port 8000

# Terminal 2: Expose via ngrok
ngrok http 8000
```

Result: `https://xxxx-xx-xxx.ngrok.io` ✅

---

### Step 3: Connect Frontend to Backend (2 min)
1. Copy your backend URL
2. Go to Vercel → Your Project → Settings → Environment Variables
3. Update `VITE_API_URL` to your backend URL
4. Redeploy (automatic)

---

### Step 4: Update CORS (1 min)
In `backend/main.py`:
```python
app.add_middleware(
    CORSMiddleware,
    allow_origins=[
        "https://your-app.vercel.app",  # ← Add this
        "http://localhost:5173",
        "*"  # Or allow all
    ],
    ...
)
```

---

## 🧪 Test Deployment

1. Visit: `https://your-app.vercel.app`
2. Upload 2 images
3. Click "Compare"
4. ✅ Should show results!

---

## 🆘 Common Issues

### ❌ "Network Error" or "CORS Error"
- Check CORS settings in backend
- Verify backend is running
- Check browser console for actual error

### ❌ Backend takes forever (first request)
- **Cold start** on free tiers (30-60 seconds)
- Model loading takes time
- Wake up backend before demo!

### ❌ Build fails on Vercel
- Check `package.json` scripts
- Ensure all dependencies listed
- Check build logs in Vercel dashboard

---

## 💡 Pro Tips for Professor Demo

1. **Wake up backend 5 min before demo**
   - Visit backend URL to trigger cold start
   
2. **Have test images ready**
   - Pre-download 2-3 image pairs
   
3. **Show GitHub repo**
   - Demonstrates version control
   
4. **Explain architecture**
   - Frontend (Vercel) → Backend (Railway) → Gemini API
   
5. **Backup plan**
   - Keep local version running
   - Have screenshots of results

---

## 📊 What to Tell Your Professor

> "I deployed the frontend on Vercel for its excellent React/Vite support and automatic CI/CD from GitHub. The backend is on Railway because it supports Python ML frameworks like TensorFlow. The Gemini integration uses environment variables for secure API key management. The entire deployment is automated via Git push."

---

## 🔗 Useful Links

- Vercel Dashboard: https://vercel.com/dashboard
- Railway Dashboard: https://railway.app/dashboard
- ngrok Dashboard: https://dashboard.ngrok.com

---

## ⏱️ Total Time: ~15 minutes
- Frontend: 5 min
- Backend: 5 min  
- Configuration: 5 min

**Good luck! 🎓**
