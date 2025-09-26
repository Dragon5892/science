
🛠️ === دليل إصلاح مشكلة gunicorn ===

📌 المشكلة الأساسية:
- رسالة الخطأ: ModuleNotFoundError: No module named 'main'
- السبب: إعدادات النشر تشير لملف main.py بدلاً من app.py

🎯 === الحل حسب المنصة ===

🔷 Koyeb:
1. في إعدادات النشر (Deploy settings)
2. Start Command: غيّر من "gunicorn main:app" إلى "gunicorn app:app"
3. أو: "python app.py" للاختبار

🔷 Render:
1. في Build & Deploy settings
2. Start Command: "gunicorn app:app"
3. Build Command: "pip install -r requirements.txt"

🔷 Railway:
1. في Variables section
2. أضف: PORT=8080
3. Start Command: "gunicorn app:app --host=0.0.0.0 --port=$PORT"

🔷 Heroku:
1. الـ Procfile موجود بالفعل: "web: gunicorn app:app"
2. تأكد من وجود requirements.txt
3. تأكد من متغيرات البيئة

🔷 PythonAnywhere:
1. في Web tab → WSGI configuration file
2. غيّر: from main import app
3. إلى: from app import app

🔷 Vercel (Serverless):
1. إنشاء ملف vercel.json:
{
  "functions": {
    "app.py": {
      "runtime": "python3.9"
    }
  },
  "routes": [
    { "src": "/(.*)", "dest": "/app.py" }
  ]
}

💡 === نصائح إضافية ===
✅ تأكد من أن Flask app instance موجود في app.py
✅ تأكد من أن الملف الرئيسي يسمى app.py بالضبط
✅ تأكد من وجود: if __name__ == '__main__': app.run() في نهاية app.py
✅ استخدم PORT من متغيرات البيئة: os.environ.get('PORT', 5000)

🔧 === كمان للأختبار السريع ===
# في نهاية ملف app.py:
if __name__ == '__main__':
    port = int(os.environ.get('PORT', 5000))
    app.run(host='0.0.0.0', port=port, debug=False)
