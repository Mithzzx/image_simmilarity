# Vercel Deployment Guide for Image Similarity Analyzer

## 📋 Prerequisites
- GitHub account
- Vercel account (free) - sign up at https://vercel.com
- Backend hosted separately (Railway, Render, or Hugging Face)

---

## 🚀 Step-by-Step Deployment

### Part 1: Deploy Frontend to Vercel

#### Step 1: Push to GitHub
```bash
# In your project root
git init
git add .
git commit -m "Initial commit for Vercel deployment"
git branch -M main
git remote add origin YOUR_GITHUB_REPO_URL
git push -u origin main
```

#### Step 2: Deploy to Vercel

1. **Go to Vercel**: https://vercel.com
2. **Sign up/Login** with GitHub
3. Click **"Add New Project"**
4. **Import your GitHub repository**
5. Configure project:
   - **Framework Preset**: Vite
   - **Root Directory**: `frontend`
   - **Build Command**: `npm run build`
   - **Output Directory**: `dist`
   
6. **Add Environment Variable**:
   - Key: `VITE_API_URL`
   - Value: `YOUR_BACKEND_URL` (e.g., `https://your-backend.railway.app`)

7. Click **"Deploy"**

#### Step 3: Your frontend is live! 🎉
- You'll get a URL like: `https://your-project.vercel.app`

---

### Part 2: Deploy Backend (Choose One)

#### Option A: Railway (Recommended for ML/Python)

1. **Go to**: https://railway.app
2. **Sign up** with GitHub
3. Click **"New Project"** → **"Deploy from GitHub repo"**
4. Select your repository
5. Configure:
   - **Root Directory**: `backend`
   - **Start Command**: `uvicorn main:app --host 0.0.0.0 --port $PORT`
6. **Add Environment Variables**:
   - `gemini_api_key`: Your Gemini API key
   - `PORT`: 8000
7. Click **"Deploy"**

**Note**: Railway has a free tier with limitations. For ML models, you may need a paid plan (~$5/month).

#### Option B: Render

1. **Go to**: https://render.com
2. **Sign up** with GitHub
3. Click **"New +"** → **"Web Service"**
4. Connect your GitHub repo
5. Configure:
   - **Name**: image-similarity-backend
   - **Root Directory**: `backend`
   - **Environment**: Python 3
   - **Build Command**: `pip install -r requirements.txt`
   - **Start Command**: `uvicorn main:app --host 0.0.0.0 --port $PORT`
6. **Add Environment Variables**:
   - `gemini_api_key`: Your Gemini API key
7. Choose **Free tier** (sleeps after inactivity)
8. Click **"Create Web Service"**

#### Option C: Hugging Face Spaces

1. **Go to**: https://huggingface.co/spaces
2. Click **"Create new Space"**
3. Choose **"Gradio"** or **"Streamlit"** SDK
4. Upload your backend code
5. Add your API key to secrets

---

## 🔧 Update Frontend with Backend URL

After deploying your backend, update the Vercel environment variable:

1. Go to your Vercel project
2. Click **"Settings"** → **"Environment Variables"**
3. Update `VITE_API_URL` to your backend URL
4. **Redeploy** (Vercel will auto-redeploy)

---

## 🌐 Update CORS in Backend

Update `main.py` to allow your Vercel domain:

```python
app.add_middleware(
    CORSMiddleware,
    allow_origins=[
        "http://localhost:5173",
        "http://localhost:3000",
        "https://your-project.vercel.app",  # Add your Vercel URL
        "*"  # Or allow all (not recommended for production)
    ],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

---

## 📝 Important Notes

### Backend Challenges:
- **Large Model Size**: ResNet50 + TensorFlow is ~500MB
- **Cold Starts**: Free tiers sleep after inactivity
- **Memory**: ML models need 2GB+ RAM
- **Cost**: Free tiers are limited; production may need paid plans

### Alternatives for Backend:
1. **Keep it Local**: Run backend on your computer, demo via ngrok
2. **Use Docker**: Deploy to AWS/GCP with Docker
3. **Simplify Model**: Use lighter alternatives like MobileNet

---

## 🧪 Testing Deployment

1. Visit your Vercel URL: `https://your-project.vercel.app`
2. Upload two test images
3. Click "Compare Images"
4. Should see results if backend is running

---

## 🆘 Troubleshooting

### Frontend can't reach backend:
- Check CORS settings in `main.py`
- Verify `VITE_API_URL` environment variable
- Check browser console for errors

### Backend deployment fails:
- Check build logs
- Verify `requirements.txt` is correct
- May need to increase memory/timeout limits

### "Module not found" errors:
- Ensure all dependencies in `requirements.txt`
- Check Python version compatibility

---

## 💰 Cost Estimate

| Service | Free Tier | Paid (if needed) |
|---------|-----------|------------------|
| Vercel (Frontend) | ✅ Free | Free (generous) |
| Railway (Backend) | ⚠️ Limited | $5-10/month |
| Render (Backend) | ⚠️ Sleeps | $7/month |
| Hugging Face | ✅ Free | $9/month |

**Recommendation for Demo**: Use free tiers, accept cold start delays for professor demo.

---

## 🎓 For Your Professor Demo

**Best Setup:**
1. **Frontend**: Deployed on Vercel (always fast)
2. **Backend**: 
   - Option 1: Run locally + use ngrok for public URL
   - Option 2: Deploy to Railway/Render (wake it up before demo)

**Demo Tips:**
- Test 5-10 minutes before presentation
- Have backup images ready
- Explain cold start if backend is slow
- Show GitHub repo and deployment configs

---

## 🔗 Quick Commands

```bash
# Test build locally
cd frontend
npm run build
npm run preview

# Deploy backend locally with ngrok (alternative)
cd backend
uvicorn main:app --host 0.0.0.0 --port 8000
# In another terminal:
ngrok http 8000
# Use ngrok URL as VITE_API_URL
```

---

Good luck with your deployment! 🚀
