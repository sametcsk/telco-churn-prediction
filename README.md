# Telco Müşteri Kayıp Tahmini (Customer Churn Prediction)



Bu proje, bir telekomünikasyon şirketindeki müşterilerin hizmeti terk etme (churn) olasılıklarını tahmin etmeyi amaçlayan uçtan uca bir Makine Öğrenmesi çalışmasıdır. Projenin temel hedefi, sadece "kimin" gideceğini tahmin etmek değil, "neden" gideceğini tespit ederek iş birimlerine aksiyon alınabilir içgörüler sunmaktır.

## Proje Notebook Dosyası
Kodları, grafikleri ve analiz adımlarını interaktif olarak görüntülemek için aşağıdaki bağlantıyı kullanabilirsiniz:



Projeyi [nbviewer üzerinden görüntüleyin](https://nbviewer.org/github/sametcsk/telco-churn-prediction/blob/main/telco-churn-analysis.ipynb).

## Proje Hedefleri
- Müşteri terkine yol açan temel faktörleri (fiyatlandırma, kontrat türü, teknik altyapı vb.) belirlemek.
- Sınıf dengesizliği (Class Imbalance) olan bir veri setinde en uygun sınıflandırma modellerini kurmak ve karşılaştırmak.
- Model kararlarını SHAP (SHapley Additive exPlanations) kullanarak şeffaf ve açıklanabilir hale getirmek.

## Metodoloji

### 1. Veri Ön İşleme ve Keşifçi Veri Analizi (EDA)
- Eksik veriler (`TotalCharges`) doğrudan silinmek yerine, verinin doğasına uygun olarak (Aylık Ücret * Müşterilik Süresi) mantıksal bir yaklaşımla doldurulmuştur.
- Kategorik değişkenler için One-Hot Encoding ve Binary Encoding işlemleri uygulanmıştır.
- Veri sızıntısını (Data Leakage) önlemek amacıyla ölçeklendirme işlemleri Pipeline içerisine entegre edilmiştir.

### 2. Modelleme ve Sınıf Dengesizliği Yönetimi
Müşteri terk veri setlerindeki tipik dengesizliği çözmek için:
- Algoritmaların içine `class_weight='balanced'` (Lojistik Regresyon, Random Forest) ve `scale_pos_weight` (XGBoost) parametreleri tanımlanmıştır.
- Modellerin kararlılığı Stratified 5-Fold Cross Validation ile test edilmiştir.

### 3. Model Değerlendirmesi ve Eşik Değeri (Threshold) Analizi
- Modeller; ROC AUC, Precision, Recall ve F1 metrikleriyle karşılaştırılmıştır.
- İş mantığı gereği, müşteri kaybetmenin maliyeti (False Negative), yanlış promosyon sunmanın maliyetinden (False Positive) daha yüksek olduğu için farklı eşik değerlerinde (0.3, 0.4, 0.5) Recall-Precision takası analiz edilmiştir.

### 4. Model Açıklanabilirliği (SHAP)
Lojistik Regresyon ana model olarak seçilmiş ve "Kara Kutu" olmaktan çıkarılmıştır.
- **SHAP Summary Plot:** Genel müşteri kitlesinin hangi özellikler (Örn: Aydan aya kontrat, Fiber optik internet) sebebiyle kaybedildiğini göstermiştir.
