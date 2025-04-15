# 🚕 Taksi Ortamı ile Q-Öğrenme Eğitimi
---

## 🗺️ Harita ve Engeller
- Harita görselden (`taksi.png`) alınır.
- Kırmızı hücreler 🚫 (geçilemez alanlar)
- Kahverengi tuğla duvarlar 🧱 (geçilemez duvarlar)
- Bu hücreler otomatik olarak tespit edilerek eğitim ortamı hazırlanır.

---

## 🧩 Ortamın Yapısı (`CustomTaxiEnv`)
- **Izgara:** 10x10 hücre (her biri 50x50 px)
- **Özellikler:**
  - Taksi: 🟡 özel ikonla
  - Yolcu: 🧍 özel ikonla
  - Hedef: 🟩 yeşil kare
  - Geçilemez alanlar: 🔴 kırmızı/kahverengi
- **Durum (state):**
  `(taksi_x, taksi_y, yolcu_x, yolcu_y, yolcu_taksidemi)`

---

## 🎮 Aksiyonlar
| Aksiyon Kodu | Açıklama              |
|--------------|-----------------------|
| 0            | Yukarı git            |
| 1            | Aşağı git             |
| 2            | Sola git              |
| 3            | Sağa git              |
| 4            | Yolcuyu al (pick up)  |
| 5            | Yolcuyu bırak (drop)  |

---

## 🏆 Ödül Sistemi
| Olay                          | Ödül   |
|-------------------------------|--------|
| Yolcuyu doğru yerde alma      | +10    |
| Yolcuyu doğru yerde bırakma   | +20    |
| Yanlış pick/drop              | -10    |
| Her adım                      | -1     |

---

## 👁️ Görselleştirme (Render)
- **Yolcu taksideyse:**  
  🔴 Taksinin etrafına **kırmızı çember** çizilir  
  ❌ Yolcunun alındığı yere **kırmızı "X"** çizilir

- **Yolcu dışarıdaysa:**  
  Yolcu ikonu gösterilir

- **Hedef:**  
  🟩 Yeşil kutu olarak görünür

- **Taksi:**  
  Taksi ikonu her zaman görünür

---

## 🤖 Q-learning Eğitimi
- Toplam **500 epoch**
- Aksiyonlar rastgele keşfedilir veya Q-table’dan seçilir
- Her adımda Q-table güncellenir:
  ```python
  Q(s, a) ← (1 - α) * Q(s, a) + α * [r + γ * max Q(s', a')]
