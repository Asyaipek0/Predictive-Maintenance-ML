# 🛠️ Machine Predictive Maintenance (Yapay Zeka Tabanlı Kestirimci Bakım)

Bu proje, bir fabrikadaki makine sensörlerinden alınan ham verileri analiz ederek, makinelerin ne zaman ve neden arıza yapabileceğini önceden tahmin eden uçtan uca (end-to-end) bir **Makine Öğrenmesi** çalışmasıdır.

## 📊 Projede Neler Yapıldı?

1. **Keşifçi Veri Analizi (EDA):** - Veri setindeki eksik ve düzensiz yapılar incelendi.
   - Korelasyon analizi (Heatmap) ve Boxplot görselleştirmeleri ile arızayı en çok tetikleyen fiziksel faktörler tespit edildi. Özellikle **Torque [Nm]** ve **Rotational Speed [rpm]** değerlerinin arıza üzerindeki doğrudan etkisi kanıtlandı.

2. **Özellik Seçimi (Feature Selection):**
   - Modelin ezber yapmasını (overfitting) önlemek amacıyla `UDI`, `Product ID` gibi kimlik kolonları veri setinden elendi.
   - Veri sızıntısını (data leakage) engellemek için spesifik arıza türü kolonları düşürülerek sadece saf sensör verileri girdi ($X$) olarak alındı.

3. **Modelleme ve Dengesiz Veri (Imbalanced Data) Çözümü:**
   - Sektörde güçlü bir klasik makine öğrenmesi algoritması olan **Random Forest Classifier** kullanıldı.
   - Sağlam makinelerin arızalı makinelere oranla çok fazla olmasından kaynaklanan dengesizlik (`support: 1932` vs `68`) fark edildi.
   - Sadece ağırlıklandırma yapmanın yeterli olmadığı görülerek kıdemli mühendis yaklaşımı olan **Threshold Tuning (Eşik Değeri Ayarı)** uygulandı.

## 📈 Model Sonuçları ve Mühendislik Kararı (Trade-off)

Modelin varsayılan arıza karar eşiği ($0.50$)'den **$0.20$'ye** düşürülmüştür. Bu stratejik kararın sonuçları şu şekildedir:

* **Eski Durum:** Gerçek arızaların sadece %63'ü yakalanabiliyordu (Recall: 0.63).
* **Yeni Durum (Eşik: 0.20):** Gerçek arızaları yakalama oranı **%81'e fırlatıldı** (Recall: 0.81).

**Mühendislik Yorumu:** Fabrika ortamında gözden kaçan bir arızanın maliyeti (üretim durması, parça kırılması) devasadır. Buna karşın, yanlış alarm durumunda bir bakım mühendisinin makineyi kontrol etmesinin maliyeti göz ardı edilebilir. Bu nedenle **Precision** değerinden bilinçli olarak fedakarlık edilerek **Recall** metriği optimize edilmiştir.

## 🛠️ Kullanılan Teknolojiler
* Python 3.x
* Pandas & NumPy (Veri Manipülasyonu)
* Matplotlib & Seaborn (Veri Görselleştirme)
* Scikit-Learn (Makine Öğrenmesi & Metrikler)
