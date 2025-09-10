 الصحيح للملفات لمشروع Social Activity Tracker سيكون كما يلي:

Code
social_activity_tracker/
│       └── app.js
├── server/
│   ├── server.js
│   ├── package.json
│   └── render.yaml
└── README.md
وإليك تفاصيل كل ملف ومحتواه المناسب (بناءً على ما قدمته):


server/render.yaml
v2
services:
  - type: web
    name: social-activity-api
    env: node
    buildCommand: cd server && npm install
    startCommand: cd server && node server.js

server/package.json
v2
{
  "name": "social-activity-api",
  "version": "1.0.0",
  "description": "Backend API for Social Activity Finder",
  "main": "server.js",
  "scripts": {

server/server.js
v2
const express = require('express');
const cors = require('cors');
const app = express();
const PORT = process.env.PORT || 10000;

// Middleware متوافق مع Render

public/index.html
v2
<!DOCTYPE html>
<html lang="ar">
<head>
  <meta charset="UTF-8">
  <title>Social Activity Tracker</title>
  <link rel="stylesheet" href="css/style.css">

public/css/style.css
v2
body {
  font-family: 'Cairo', Arial, sans-serif;
  direction: rtl;
  margin: 0;
  padding: 20px;
  background: #fafafa;

public/js/app.js
v2
// دالة للبحث في API
async function searchActivities(searchData) {
  try {
    const API_URL = 'https://your-render-app.onrender.com'; // عدل هذا لاحقاً لرابط تطبيقك بعد النشر
    const response = await fetch(`${API_URL}/api/search`, {
      method: 'POST',

README.md
v2
# Social Activity Tracker

نظام بسيط للبحث عن النشاط الاجتماعي لمستخدم أو وسم وعرض النتائج بشكل زمني.

## خطوات العمل

هل تريد أي شرح إضافي أو تجهيز الملفات للتحميل بشكل مباشر؟
