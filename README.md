# social_activity_tracker
تطبيق node. js
services:
  - type: web
    name: social-activity-api
    env: node
    buildCommand: cd server && npm install
    startCommand: cd server && node server.js
    envVars:
      - key: NODE_ENV
        value: production
    plan: free
{
  "name": "social-activity-api",
  "version": "1.0.0",
  "description": "Backend API for Social Activity Finder",
  "main": "server.js",
  "scripts": {
    "start": "node server.js",
    "dev": "nodemon server.js"
  },
  "dependencies": {
    "express": "^4.18.2",
    "cors": "^2.8.5",
    "axios": "^1.4.0"
  },
  "engines": {
    "node": "18.x"
  }
}
const express = require('express');
const cors = require('cors');
const app = express();
const PORT = process.env.PORT || 10000;

// Middleware متوافق مع Render
app.use(cors());
app.use(express.json({ limit: '10mb' }));

// routes الأساسية
app.post('/api/search', async (req, res) => {
  try {
    const { identifier, type, startDate, endDate } = req.body;
    
    // محاكاة بيانات للاختبار (يتم استبدالها بالوظيفة الفعلية)
    const mockData = [
      {
        platform: "twitter",
        content: `نشاط اختباري لـ ${identifier}`,
        date: new Date().toISOString(),
        engagement: { likes: 25, shares: 5 }
      }
    ];
    
    res.json({ 
      success: true, 
      data: mockData,
      count: mockData.length 
    });
  } catch (error) {
    res.status(500).json({ 
      success: false, 
      error: error.message 
    });
  }
});

// health check إلزامي لـ Render
app.get('/health', (req, res) => {
  res.json({ 
    status: 'OK', 
    timestamp: new Date().toISOString() 
  });
});

app.listen(PORT, '0.0.0.0', () => {
  console.log(`✅ Server running on port ${PORT}`);
});
<!DOCTYPE html>
<html lang="ar">
<head>
  <meta charset="UTF-8">
  <title>Social Activity Tracker</title>
  <link rel="stylesheet" href="css/style.css">
</head>
<body>
  <h1>متتبع النشاط الاجتماعي</h1>
  <div>
    <input type="text" id="identifier" placeholder="اسم المستخدم أو الوسم">
    <button id="searchBtn">بحث</button>
  </div>
  <div id="results"></div>
  <script src="js/app.js"></script>
  <script>
    // التكوين التلقائي للربط مع Render
    const RENDER_URL = 'https://your-render-app.onrender.com';

    // اختبار الاتصال عند التحميل
    document.addEventListener('DOMContentLoaded', async () => {
      try {
        const response = await fetch(`${RENDER_URL}/health`);
        if (response.ok) {
          console.log('✅ Connected to Render server');
        }
      } catch (error) {
        console.log('❌ Render server not available');
      }

      // ربط زر البحث
      document.getElementById('searchBtn').onclick = async () => {
        const identifier = document.getElementById('identifier').value;
        const searchData = { identifier, type: 'user', startDate: '', endDate: '' };
        const data = await searchActivities(searchData);
        if (data.success) {
          displayResults(data.data);
        } else {
          document.getElementById('results').innerHTML = '<p>حدث خطأ أثناء البحث</p>';
        }
      };
    });
  </script>
</body>
</html>
body {
  font-family: 'Cairo', Arial, sans-serif;
  direction: rtl;
  margin: 0;
  padding: 20px;
  background: #fafafa;
}
h1 {
  color: #3b5998;
}
input, button {
  padding: 10px;
  font-size: 1rem;
}
button {
  background: #3b5998;
  color: #fff;
  border: none;
  cursor: pointer;
}
#results {
  margin-top: 30px;
}
.activity-card {
  background: #fff;
  border: 1px solid #ddd;
  margin-bottom: 15px;
  padding: 15px;
  border-radius: 7px;
  box-shadow: 0 1px 4px rgba(60,60,60,.07);
}
.activity-card h3 {
  margin: 0 0 5px 0;
  color: #1da1f2;
}
// استخدم هذه التعليقات لإرشاد GitHub Copilot

// دالة للبحث في API
async function searchActivities(searchData) {
  try {
    // ✅ GitHub Copilot: املأ عنوان Render التلقائي هنا
    const API_URL = 'https://your-render-app.onrender.com';
    
    const response = await fetch(`${API_URL}/api/search`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(searchData)
    });
    
    const data = await response.json();
    return data;
  } catch (error) {
    console.error('Error:', error);
    return { success: false, error: error.message };
  }
}

// دالة لعرض النتائج
function displayResults(activities) {
  // ✅ GitHub Copilot: أنشئ جدولاً زمنياً للنتائج
  const resultsContainer = document.getElementById('results');
  resultsContainer.innerHTML = activities.map(activity => `
    <div class="activity-card">
      <h3>${activity.platform}</h3>
      <p>${activity.content}</p>
      <small>${new Date(activity.date).toLocaleDateString('ar-EG')}</small>
    </div>
  `).join('');
}
# Social Activity Tracker

نظام بسيط للبحث عن النشاط الاجتماعي لمستخدم أو وسم وعرض النتائج بشكل زمني.

## خطوات العمل

1. ضع جميع الملفات في المستودع بنفس الهيكل الموضح.
2. اربط المستودع مع [Render](https://render.com) واتبع الإعدادات التالية:
   - Build Command: `cd server && npm install`
   - Start Command: `cd server && node server.js`
   - Plan: Free
3. بعد النشر، عدل رابط Render في `public/js/app.js` و `public/index.html` (`your-render-app.onrender.com`) إلى رابط تطبيقك الفعلي.

## الهيكل

```bash
مشروعك/
├── public/
│   ├── index.html
│   ├── css/
│   │   └── style.css
│   └── js/
│       └── app.js
├── server/
│   ├── server.js
│   ├── package.json
│   └── render.yaml
└── README.md
```
