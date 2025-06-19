<!DOCTYPE html>
<html lang="th"><head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ดาวน์โหลดวิดีโอ 4K 1080P HD</title>
    <link rel="stylesheet" href="style.css">
    <link rel="manifest" href="manifest.json">
    <meta name="theme-color" content="#0072ff">
    <script>
        if ('serviceWorker' in navigator) {
            navigator.serviceWorker.register('service-worker.js')
                .then(reg => console.log('Service Worker registered', reg))
                .catch(err => console.log('Service Worker registration failed', err));
        }async function fetchVideo() {
        const url = document.getElementById('videoUrl').value;
        const res = await fetch(`/api/getVideo?url=${encodeURIComponent(url)}`);
        const data = await res.json();
        if (data.formats) {
            let html = '<h3>เลือกรูปแบบและคุณภาพ:</h3>';
            data.formats.forEach(format => {
                if (format.ext === 'mp4' && format.url) {
                    html += `<a href="${format.url}" target="_blank" class="download-btn">${format.format_note || ''} - ${format.height || ''}p</a><br>`;
                }
            });
            document.getElementById('qualityOptions').innerHTML = html;
        } else {
            document.getElementById('qualityOptions').innerHTML = '<p>ไม่สามารถดึงวิดีโอได้</p>';
        }
    }
</script>

</head><body>
    <div class="container">
        <h1>ดาวน์โหลดวิดีโอออนไลน์</h1>
        <input type="text" id="videoUrl" placeholder="วางลิงก์วิดีโอที่นี่...">
        <button onclick="fetchVideo()">ดึงลิงก์วิดีโอ</button>
        <div id="qualityOptions"></div>
        <div class="ad">โฆษณาของคุณที่นี่</div>
    </div>
</body></html><!-- style.css --><style>
body {
    font-family: 'Arial', sans-serif;
    background: linear-gradient(135deg, #00c6ff, #0072ff);
    color: white;
    text-align: center;
    padding: 20px;
}
.container {
    max-width: 600px;
    margin: auto;
    background: rgba(255, 255, 255, 0.1);
    padding: 20px;
    border-radius: 20px;
    box-shadow: 0 0 15px rgba(0,0,0,0.3);
}
input {
    width: 80%;
    padding: 10px;
    border: none;
    border-radius: 10px;
    margin-bottom: 10px;
}
button {
    padding: 10px 20px;
    background-color: #ffcc00;
    border: none;
    border-radius: 10px;
    cursor: pointer;
    font-size: 16px;
}
.download-btn {
    display: inline-block;
    margin: 5px;
    padding: 10px;
    background-color: #00ffcc;
    color: #000;
    border-radius: 10px;
    text-decoration: none;
}
.ad {
    margin-top: 20px;
    padding: 15px;
    background: white;
    color: black;
    border-radius: 10px;
  {
</style>
<!-- manifest.json -->
<script type="application/json" id="manifest">
{
  "name": "ดาวน์โหลดวิดีโอ 4K 1080P HD",
  "short_name": "VideoDL",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#00c6ff",
  "theme_color": "#0072ff",
  "description": "เว็บไซต์ดาวน์โหลดวิดีโอออนไลน์ รองรับ 4K 1080P HD พร้อมโฆษณา",
  "icons": [
    {
      "src": "icon-192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "icon-512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ]
}
</script>
<!-- service-worker.js -->
<script type="text/javascript" id="service-worker">
const cacheName = 'video-downloader-cache-v1';
const assetsToCache = [
    '/',
    '/index.html',
    '/style.css',
    '/manifest.json',
    '/icon-192.png',
    '/icon-512.png'
];
self.addEventListener('install', event => {
    event.waitUntil(
        caches.open(cacheName).then(cache => {
            return cache.addAll(assetsToCache);
        })
    );
});
self.addEventListener('fetch', event => {
    event.respondWith(
        caches.match(event.request).then(response => {
            return response || fetch(event.request);
        })
    );
});
</script>
