const CACHE_NAME = 'zenstudy-cache-v1';
const ASSETS_TO_CACHE = [
  '/',
  '/index.html',
  '/jeeee.html',
  '/style.css',   // your CSS file
  '/main.js',     // your JS file
  '/Picsart_25-11-28_15-47-25-082.png',
  '/Picsart_25-11-28_15-47-16-095.png'
];

// Install SW
self.addEventListener('install', event => {
  event.waitUntil(
    caches.open(CACHE_NAME).then(cache => cache.addAll(ASSETS_TO_CACHE))
  );
  self.skipWaiting();
});

// Activate SW
self.addEventListener('activate', event => {
  event.waitUntil(
    caches.keys().then(keys =>
      Promise.all(keys.map(key => {
        if (key !== CACHE_NAME) return caches.delete(key);
      }))
    )
  );
  self.clients.claim();
});

// Fetch from cache first
self.addEventListener('fetch', event => {
  event.respondWith(
    caches.match(event.request).then(res => res || fetch(event.request))
  );
});

// Push Notification
self.addEventListener('push', event => {
  const data = event.data?.json() || { title: 'ZenStudy', body: 'You have a new task!' };
  const options = {
    body: data.body,
    icon: '/Picsart_25-11-28_15-47-25-082.png',
    badge: '/Picsart_25-11-28_15-47-16-095.png',
    vibrate: [100, 50, 100],
    data: { url: '/jeeee.html' }
  };
  event.waitUntil(self.registration.showNotification(data.title, options));
});

// Click notification
self.addEventListener('notificationclick', event => {
  event.notification.close();
  event.waitUntil(
    clients.matchAll({ type: 'window', includeUncontrolled: true }).then(clientList => {
      for (const client of clientList) {
        if (client.url.includes('/jeeee.html') && 'focus' in client) return client.focus();
      }
      if (clients.openWindow) return clients.openWindow('/jeeee.html');
    })
  );
});
