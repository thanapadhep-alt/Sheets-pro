const CACHE_NAME = "sheets-pro-v3";

const ASSETS = [
  "./",
  "./index.html",
  "./manifest.json",
  "./service-worker.js",
  "./ป.58พื้นที่.html",
  "./icon-192.png",
  "./icon-512.png",
];

// ติดตั้ง + cache ไฟล์ทั้งหมด
self.addEventListener("install", (event) => {
  event.waitUntil(
    caches.open(CACHE_NAME).then((cache) => cache.addAll(ASSETS))
  );
  self.skipWaiting();
});

// ใช้งาน cache ทันที + เคลียร์ cache เก่า
self.addEventListener("activate", (event) => {
  event.waitUntil(
    caches.keys().then((keys) =>
      Promise.all(keys.map((k) => (k !== CACHE_NAME ? caches.delete(k) : null)))
    )
  );
  self.clients.claim();
});

// Offline: cache-first + รองรับ navigation
self.addEventListener("fetch", (event) => {
  const req = event.request;

  // เปิดหน้าเว็บ (HTML Navigation)
  if (req.mode === "navigate") {
    event.respondWith(
      fetch(req).catch(() => caches.match("./index.html"))
    );
    return;
  }

  // ไฟล์อื่นๆ
  event.respondWith(
    caches.match(req).then((cached) => {
      return (
        cached ||
        fetch(req).then((res) => {
          const copy = res.clone();
          caches.open(CACHE_NAME).then((cache) => cache.put(req, copy));
          return res;
        })
      );
    }).catch(() => caches.match("./index.html"))
  );
});
