# 🚀 Netlify Deployment Guide for Antibiotic Resistance Predictor

## ⚠️ Important Note

**Streamlit applications are NOT directly compatible with Netlify's static hosting.**

Netlify is designed for static sites (HTML, CSS, JS) and serverless functions. Streamlit requires a Python server running continuously.

## 📌 Recommended Hosting Platforms for Streamlit

### 1. **Streamlit Community Cloud** (Recommended)
- ✅ **FREE** for public repositories
- ✅ Native Streamlit support
- ✅ Automatic deployment from GitHub
- ✅ Easy setup (3 minutes)
- 🔗 https://streamlit.io/cloud

**Setup Steps:**
1. Go to https://share.streamlit.io/
2. Sign in with GitHub
3. Click "New app"
4. Select repository: `adrenolitik/Antibiotic-Resistance-Predictor`
5. Main file path: `main.py`
6. Click "Deploy"

### 2. **Hugging Face Spaces** (Already deployed)
- ✅ FREE
- ✅ GPU support available
- ✅ Public & private spaces
- 🔗 Your current deployment: https://huggingface.co/spaces/MrSanjay/Antibiotic-Resistance-Predictor

### 3. **Render.com**
- ✅ FREE tier available
- ✅ Easy Python app deployment
- ✅ Auto-deploy from GitHub
- 🔗 https://render.com

**Setup Steps:**
1. Go to https://dashboard.render.com/
2. Click "New +" → "Web Service"
3. Connect GitHub repository
4. Build Command: `pip install -r requirements.txt`
5. Start Command: `streamlit run main.py --server.port $PORT --server.address 0.0.0.0`

### 4. **Railway.app**
- ✅ FREE tier ($5 credit/month)
- ✅ Simple deployment
- ✅ GitHub integration
- 🔗 https://railway.app

### 5. **Heroku**
- 💰 Paid (starting $7/month)
- ✅ Reliable and scalable
- ✅ Add-ons available

## 🔧 Alternative: Netlify with Custom Backend

If you must use Netlify, you'll need:

1. **Host Streamlit separately** (Render, Railway, etc.)
2. **Create a static frontend** on Netlify that embeds the Streamlit app via iframe
3. Use Netlify for landing page only

### Example `index.html` for Netlify:
```html
<!DOCTYPE html>
<html>
<head>
    <title>Antibiotic Resistance Predictor</title>
    <style>
        body, html { margin: 0; padding: 0; height: 100%; overflow: hidden; }
        iframe { width: 100%; height: 100%; border: none; }
    </style>
</head>
<body>
    <iframe src="YOUR_STREAMLIT_URL_HERE"></iframe>
</body>
</html>
```

## 📊 Comparison Table

| Platform | Cost | Streamlit Support | Setup Time | Performance |
|----------|------|-------------------|------------|-------------|
| **Streamlit Cloud** | FREE | Native | 3 min | ⭐⭐⭐⭐⭐ |
| **Hugging Face** | FREE | Native | 5 min | ⭐⭐⭐⭐ |
| **Render** | FREE tier | Full | 10 min | ⭐⭐⭐⭐ |
| **Railway** | $5/month | Full | 8 min | ⭐⭐⭐⭐⭐ |
| **Netlify** | FREE | ❌ Static only | N/A | ⚠️ Not compatible |

## 🎯 My Recommendation

**Deploy to Streamlit Community Cloud:**
1. Fastest setup (< 5 minutes)
2. Free forever for public repos
3. Native Streamlit support
4. Automatic SSL certificate
5. Custom domain support
6. Direct GitHub integration

Would you like me to help you deploy to Streamlit Cloud or another platform?

## 📝 Files Ready for Deployment

This repository includes:
- ✅ `main.py` - Main application
- ✅ `requirements.txt` - Dependencies
- ✅ `README.md` - Documentation
- ✅ `.gitignore` - Git configuration
- ✅ Sample genome files (GCF_*.fna.gz)
- ✅ Antibiotic database (antibiotic_compounds.json)

All ready for deployment on supported platforms! 🚀
