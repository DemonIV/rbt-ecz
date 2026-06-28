# CLAUDE.md — EBOT Eczane Robotu (3D Simülasyon)

Bu dosyayı her oturum başında oku. Proje küçük ve niyet nettir; sadeliği koru.

## ÜRÜN
EBOT eczane robotunun **parametrik 3B simülasyonu**. Kabin/pay/kanal/ilaç ölçüleri
canlı slider'larla değişir, metrikler (kat, kanal, toplam ilaç) anında hesaplanır.
Mobil-öncelikli, dokunmatik kontrollü. "İlaç getir" butonu robot animasyonu oynatır.
Dil: Türkçe (UI). Hedef: hızlı görsel keşif / sunum aracı.

**Referans:** EBOT, Türkiye'de patentli bir eczane (ilaç dispenser) robotudur. Bu proje
o gerçek sistemin görünüm ve çalışma mantığını birebir benzetmeyi (eğimli kanal rafları,
köprü-tipi alıcı robot, konveyörle ilaç tahliyesi) hedefler. Geometri/oranlar gerçeğe
yaklaştıkça doğru kabul edilir.

## STACK (DEĞİŞMEZ — basit tut)
- **Tek dosya uygulama:** `index.html` (HTML + CSS + vanilla JS, hepsi gömülü).
- **3B:** `three.min.js` — YEREL ve gömülü (internet olmadan da çalışır). CDN ekleme.
- Build yok, paket yöneticisi yok, framework yok. Sade vanilla JS (ES5 uyumlu IIFE).
- Deploy: **Render.com statik site** (`render.yaml`). Publish dir `.`, build command boş.

## DOSYALAR
```
index.html      # tüm uygulama: stil, DOM, three.js sahnesi, kontroller, animasyon
three.min.js    # Three.js (yerel kopya — sürüm değiştirme, dokunma)
render.yaml     # Render statik site config (cache + SPA rewrite)
README.md       # kullanım + deploy talimatları
```

## index.html YAPISI (script IIFE içinde)
- `P` = parametre durumu (W,H,D,COL, TOP/BOT/SIDE/CUT, tilt, chH, chW, med, box, wall).
- `build()` — sahne/kamera/ışık/zemin bir kez kurulur.
- `rebuild()` — `root` grubunu temizleyip parametrelerden geometriyi yeniden üretir.
  Slider her değişimde `rebuild()` çağrılır. Metrik/fizik-notu burada güncellenir.
- Kamera: küresel koordinat (theta/phi/rad) + dokunmatik (1 parmak döndür, 2 parmak zoom/pan).
- `dispense()/fall()/flow()` — robot animasyon zinciri (requestAnimationFrame ease).
- `link()` — slider'ı `P`'ye bağlayan yardımcı. Yeni slider eklerken bunu kullan.

## KODLAMA KURALLARI
- Mevcut vanilla/ES5 stilini koru: `var`, IIFE, inline event listener. Yeni pattern icat etme.
- Geometri değişiklikleri **sadece `rebuild()` içinde**; tek kaynak orası.
- Yeni parametre = `P`'ye alan + HTML slider + `link()` bağı + `btnPreset` reset değeri. Dördünü birden ekle.
- Bellek: `clear()` geometry/material `dispose()` ediyor — yeni mesh türü eklersen temizlendiğinden emin ol.
- Performans: mobil hedef. Pixel ratio max 2, ilaç kutuları kanal başına max 12 ile sınırlı — bu sınırları koru.
- Hata yönetimi: script `try/catch` içinde, `fail()` ile ekranda gösterir. Bunu bozma.
- Türkçe UI metinleri; kod/yorum İngilizce ya da kısa Türkçe (mevcut karışık stil kabul).

## ÇALIŞMA DİSİPLİNİ
- Tek dosya — gereksiz tarama yok. Değişikliği `index.html` içinde yap.
- Cevapta tüm dosyayı yapıştırma; sadece değişen bloğu göster + 2-3 cümle özet.
- Açıklama isteme; kararları uygula, sonra kısa özet geç.
- Yerel test: `python3 -m http.server 8000` → `http://localhost:8000`.

## YAPILDI / SIRADA (bu bölümü güncel tut)
- [x] Parametrik 3B sahne + canlı slider'lar + metrikler
- [x] Dokunmatik kamera + görünüm butonları (Yandan/Önden/Sıfırla)
- [x] Robot "ilaç getir" animasyonu (raf → konveyör akışı)
- [x] Render.com statik deploy config
- [x] Yan boşluk → Sol/Sağ ayrı kontrol (asimetrik usable alan: `xL`/`xC`)
- [x] Performans: gölge map 1024, ilaç/raf gölge kapalı, on-demand render (`dirty` bayrağı)
- [x] Pan sınırlama (görünüm kaçmıyor) + animasyon hızlandırma
- [ ] (sırada — kullanıcı belirleyecek)
