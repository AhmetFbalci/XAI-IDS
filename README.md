# 🛡️ CICIDS2017 Veri Seti ile Açıklanabilir Yapay Zeka (XAI) Destekli 1D-CNN Ağ Saldırı Tespit Sistemi (NIDS)

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)
[![TensorFlow](https://img.shields.io/badge/TensorFlow-2.x-orange.svg)](https://tensorflow.org/)
[![Keras](https://img.shields.io/badge/Keras-Deep%20Learning-red.svg)](https://keras.io/)
[![Scikit-Learn](https://img.shields.io/badge/scikit--learn-1.6%2B-F7931E.svg)](https://scikit-learn.org/)
[![XAI](https://img.shields.io/badge/XAI-SHAP%20%7C%20LIME-green.svg)](https://github.com/slundberg/shap)
[![Accuracy](https://img.shields.io/badge/Accuracy-99.78%25-brightgreen.svg)]()
[![Model Size](https://img.shields.io/badge/Model%20Size-0.54%20MB-blueviolet.svg)]()

Bu çalışma; modern bilgisayar ağlarında siber saldırıların tespiti amacıyla geliştirilmiş, **hafif (lightweight)**, **yüksek başarımlı** ve **açıklanabilir (interpretable)** bir **1-Boyutlu Evrişimsel Sinir Ağı (1D-CNN)** tabanlı Ağ Saldırı Tespit Sistemi (NIDS) tez/araştırma projesidir.

Modelin karar mekanizmasının şeffaflığı **SHAP (SHapley Additive exPlanations)** ve **LIME (Local Interpretable Model-agnostic Explanations)** gibi küresel ve yerel Açıklanabilir Yapay Zeka (XAI) teknikleriyle incelenmiş, iki yaklaşım arasındaki karar örtüşmesi **Jaccard Benzerlik Analizi** ile değerlendirilmiştir. Ayrıca, modelin gerçek zamanlı (streaming) ağ trafiğindeki paket gecikmesi ve sistem kapasitesi simüle edilmiştir.

---

## 📌 Proje Özeti ve Temel Katkılar

1. **Gelişmiş Veri Ön İşleme:**
   - 2.5 milyonu aşkın akış (flow) kaydından oluşan **CICIDS2017** veri seti temizlenmiş; çok nadir sınıflar (Heartbleed, Infiltration) ayıklanmış, DoS ve Web Attack alt türleri standartlaştırılarak 8 ana sınıfa indirgenmiştir.
   - Tam korelasyonlu (corr = 1.0) 6 gereksiz/artık öznitelik elenmiş, **QuantileTransformer (Normal Dağılım)** ile uç değerlere karşı dayanıklı ölçekleme yapılmıştır.
2. **SHAP Tabanlı Matematiksel Öznitelik Seçimi:**
   - 64 öznitelik arasından küresel mutlak SHAP eşik değeri ($\ge 0.010$) kullanılarak en ayırt edici **37 kritik öznitelik** seçilmiş, 27 gürültülü öznitelik elenmiştir.
3. **Kompakt ve Güçlü 1D-CNN Mimarisi:**
   - Yalnızca **41,278 parametre** (~0.54 MB disk boyutu) içeren 5 evrişimli (Conv1D) ve 4 yoğun (Dense) katmandan oluşan özel mimari tasarlanmıştır.
4. **10-Katlı Tabakalı Çapraz Doğrulama (Stratified 10-Fold CV):**
   - Modelin genelleme başarısı 10-Fold CV ile doğrulanmış: **%99.78 Ortalama Doğruluk (Accuracy)** ve **%93.25 Ortalama Macro F1-Skoru** elde edilmiştir.
5. **Çift Yönlü Açıklanabilirlik (Global SHAP & Yerel LIME):**
   - Tüm sınıflar için küresel öznitelik önem dağılımları ve tekil paket bazında yerel açıklamalar üretilmiştir.
   - Tabakalı 400 örnek üzerinde SHAP ve LIME arasındaki örtüşme **Jaccard Benzerlik Skoru (ort. 0.28)** ile matematiksel olarak incelenmiştir.
6. **Gerçek Zamanlı Çıkarım (Streaming Latency Simulation):**
   - Modelin donanım üzerinde tekil paketleri işleme süresi `tf.function` grafiği ile test edilmiş; paket başına **1.458 ms gecikme** ve saniyede **~686 paket işleme kapasitesi** ölçülmüştür.

---

## 📊 Model Başarım ve Doğrulama Sonuçları

### 1. 10-Katlı Çapraz Doğrulama (10-Fold Cross Validation)

| Metrik | Değer | Standart Sapma |
| :--- | :---: | :---: |
| **Ortalama Doğruluk (Mean Accuracy)** | **%99.78** | $\pm 0.0001$ |
| **Ortalama Macro F1-Skoru** | **%93.25** | $\pm 0.0111$ |
| **Ortalama Duyarlılık (Mean Recall)** | **%90.71** | $\pm 0.0216$ |
| **Ortalama Kesinlik (Mean Precision)** | **%98.98** | $\pm 0.0148$ |
| **En İyi Kat (Best Fold - Fold 9)** | **Macro F1: %95.36** | **Acc: %99.80** |
| **Toplam CV Eğitim Süresi** | ~83.35 dakika | (5001.2 saniye) |

---

### 2. Test Seti Sınıf Bazlı Performans Raporu (756,695 Örnek)

| Sınıf ID | Sınıf Adı | Precision | Recall | F1-Score | Test Örnek Sayısı (Support) |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **0** | **BENIGN (Normal Trafik)** | 1.00 | 1.00 | **1.00** | 628,945 |
| **1** | **Bot** | 0.93 | 0.61 | **0.74** | 586 |
| **2** | **DDoS** | 1.00 | 1.00 | **1.00 (0.9993)** | 38,405 |
| **3** | **DoS** | 0.99 | 0.99 | **0.99** | 58,124 |
| **4** | **FTP-Patator** | 1.00 | 0.99 | **0.99** | 1,780 |
| **5** | **PortScan** | 0.99 | 1.00 | **0.99** | 27,246 |
| **6** | **SSH-Patator** | 1.00 | 0.93 | **0.96** | 966 |
| **7** | **Web Attacks** | 0.99 | 0.93 | **0.96** | 643 |
| **-** | **Genel Doğruluk (Accuracy)** | - | - | **%99.78** | **756,695** |
| **-** | **Macro Average** | 0.99 | 0.93 | **0.95** | 756,695 |
| **-** | **Weighted Average** | 1.00 | 1.00 | **1.00** | 756,695 |

---

### 3. Gerçek Zamanlı Üretim Ortamı (Streaming) Gecikme Testi

| Parametre | Ölçülen Değer |
| :--- | :--- |
| **Model Disk Boyutu** | **0.5380 MB** (Keras formatında) |
| **Toplam Eğitilebilir Parametre** | **41,278** |
| **Simüle Edilen Paket Sayısı** | 10,000 bağımsız paket |
| **Ortalama Paket Çıkarım Gecikmesi** | **1.4579 ms** / paket |
| **Sistem Verim Kapasitesi (Throughput)** | **685.93 paket / saniye** |
| **Çalışma Zamanı RAM Kullanımı** | ~1878 MB |

---

## 🏗️ 1D-CNN Model Mimarisi

Ağ akış dizilerini işlemek üzere tasarlanan evrişimli ağ mimarisi:

```text
Giriş: (Batch Size, 37, 1)
│
├── Conv1D (32 filtre, kernel=2, ReLU, padding='same')    ──> Parametre: 96
├── Conv1D (16 filtre, kernel=2, ReLU, padding='valid')   ──> Parametre: 1,040
├── Conv1D (16 filtre, kernel=2, ReLU, padding='valid')   ──> Parametre: 528
├── Conv1D (16 filtre, kernel=2, ReLU, padding='valid')   ──> Parametre: 528
├── Conv1D (16 filtre, kernel=2, ReLU, padding='valid')   ──> Parametre: 528
├── Spatial Dropout (0.50)
├── MaxPooling1D (pool_size=2)
├── Flatten ()                                            ──> Çıkış: (None, 256)
├── Dense (100 nöron, ReLU)                               ──> Parametre: 25,700
├── Dense (75 nöron, ReLU)                                ──> Parametre: 7,575
├── Dense (50 nöron, ReLU)                                ──> Parametre: 3,800
├── Dense (25 nöron, ReLU)                                ──> Parametre: 1,275
└── Dense (8 nöron, Softmax)                              ──> Parametre: 208
                                                              ────────────────
                                                              Toplam: 41,278 Parametre
```

- **Kayıp Fonksiyonu (Loss):** `sparse_categorical_crossentropy`
- **Optimizasyon:** `Adam(learning_rate=0.001)`
- **Eğitim Stratejisi:** EarlyStopping & ModelCheckpoint (En iyi ağırlıkların saklanması).

---

## 🔍 Açıklanabilir Yapay Zeka (XAI) Analizleri

### 1. Küresel Açıklanabilirlik (Global SHAP)
- Tüm sınıflar için ağ akış özniteliklerinin modele etkisi Game Theory (Shapley değerleri) ile hesaplanmıştır.
- `shap_outputs_2/06_shap_importance_summary.csv` çıktısı ile belirlenen ortalama mutlak SHAP katkısı 0.010'un üzerindeki 37 öznitelik modele dahil edilmiştir.
- Sınıf bazında (BENIGN, DDoS, PortScan, DoS vb.) özniteliklerin pozitif/negatif etkileri özet grafiklerle (beeswarm/summary plot) görselleştirilmiştir.

### 2. Yerel Açıklanabilirlik (Local LIME)
- Belirli saldırı tiplerinde (örneğin PortScan veya Web Attack) tek bir paket için modelin neden o kararı verdiği LIME doğrusal yaklaşımı ile incelenmiştir.

### 3. SHAP ve LIME Karşılaştırmalı Tutarlılık Analizi (Jaccard)
- Küresel ölçekte en önemli ilk 30 SHAP özniteliği ile tekil tahminlerde LIME'ın öne çıkardığı öznitelikler arasındaki kesişim **Jaccard Benzerlik Katsayısı** ile hesaplanmıştır:
  $$J(A, B) = \frac{|A \cap B|}{|A \cup B|}$$
- 8 sınıftan tabakalı olarak seçilen 400 test örneği üzerinde **Ortalama Jaccard Skoru: 0.28** olarak ölçülmüştür. Bu durum, küresel önem hiyerarşisi ile yerel vaka bazlı karar sınırlarının birbirini tamamladığını göstermektedir.

---

## 📁 Dizin ve Dosya Yapısı

```bash
├── 1d-cnn_best_2_shap/                # 1D-CNN Eğitim, Değerlendirme ve Raporlama
│   ├── models/
│   │   ├── best_overall_model.keras    # Test setinde en yüksek skoru alan nihai model
│   │   ├── best_weights.weights.h5     # En iyi ağırlıklar
│   │   ├── fold_1_best_model.keras ... # 10-Katlı CV modelleri (Fold 1-10)
│   │   └── model_architecture.txt      # Keras mimari özeti
│   ├── accuracy_plot.png               # Eğitim/Doğrulama doğruluk grafiği
│   ├── loss_plot.png                   # Eğitim/Doğrulama kayıp (loss) grafiği
│   ├── confusion_matrix_normalized.png # Normalleştirilmiş Hata Matrisi
│   ├── roc_curves.png                  # Çok sınıflı ROC-AUC eğrileri
│   ├── precision_recall_curves.png     # Precision-Recall eğrileri
│   ├── class_metrics_bar.png           # Sınıf bazlı metrik grafikleri
│   ├── fold_metrics.png                # 10 katın karşılaştırmalı analizi
│   ├── final_report.txt                # Nihai test seti sınıflandırma raporu
│   └── metrics_summary.txt             # 10-Fold CV özet istatistikleri
│
├── shap_lime_yeni_analiz/             # XAI Çıktıları ve Görselleri
│   ├── shap_global_ilk30.png           # En önemli ilk 30 öznitelik (Global SHAP)
│   ├── shap_summary_sinif_0.png .. 7   # 8 sınıf için ayrı ayrı SHAP özet grafikleri
│   ├── lime_yerel_sinif_0_idx2273.png  # Sınıf bazlı LIME yerel açıklama grafikleri
│   └── shap_lime_jaccard_stratified.png# SHAP-LIME Jaccard benzerlik dağılımı
│
├── notebooks/                          # Jupyter / Google Colab Çalışma Defterleri
│   ├── veri_onsileme_v1.ipynb          # Veri seti ilk keşif ve filtreleme
│   ├── verionisleme_2(1).ipynb         # Etiketleme, korelasyon elemesi, Quantile Scaling
│   ├── ozellik_secimi.ipynb            # SHAP eşik değerine göre 37 özniteliğin seçilmesi
│   ├── 1_dcnn_best_2_shap.ipynb        # 1D-CNN 10-Fold CV eğitimi ve test değerlendirmesi
│   ├── SHAP_analizi_2(1).ipynb         # Derin SHAP küresel analizleri
│   ├── Shap_lime_analizi.ipynb         # SHAP, LIME ve Jaccard tutarlılık analizi
│   └── çıkarım_süresi.ipynb            # Gerçek zamanlı gecikme (latency) ve verim testi
│
└── README.md                           # Proje dokümantasyonu
```

---

## 🚀 Kurulum ve Kullanım

### 1. Gereksinimler
Proje Python 3.10+ ve modern derin öğrenme kütüphaneleri ile uyumludur:

```bash
git clone https://github.com/kullanici-adi/proje-adi.git
cd proje-adi
pip install -r requirements.txt
```

*(Eğer `requirements.txt` yoksa temel paketler:)*
```bash
pip install tensorflow>=2.15 scikit-learn pandas numpy matplotlib seaborn shap lime psutil
```

### 2. Adım Adım İş Akışı

1. **Ön İşleme:** `notebooks/verionisleme_2(1).ipynb` çalıştırılarak ham CICIDS2017 verisi temizlenir ve `QuantileTransformer` ile ölçeklenir.
2. **Öznitelik Seçimi:** `notebooks/ozellik_secimi.ipynb` ile SHAP önem derecesine göre $d=37$ öznitelik belirlenir ve kaydedilir.
3. **Model Eğitimi:** `notebooks/1_dcnn_best_2_shap.ipynb` çalıştırılarak 10-Fold CV eğitimi gerçekleştirilir, metrikler ve `.keras` modelleri kaydedilir.
4. **XAI Açıklanabilirlik:** `notebooks/Shap_lime_analizi.ipynb` çalıştırılarak SHAP ve LIME görselleştirmeleri ile Jaccard skorları üretilir.
5. **Gecikme Simülasyonu:** `notebooks/çıkarım_süresi.ipynb` ile üretim ortamı tekil paket çıkarım süresi test edilir.

### 3. Kayıtlı Modeli Yükleme ve Çıkarım Yapma (Örnek Python Kodu)

```python
import tensorflow as tf
import pandas as pd
import numpy as np

# Modeli yükleme
model_path = "1d-cnn_best_2_shap/models/best_overall_model.keras"
model = tf.keras.models.load_model(model_path)

# 37 öznitelikli yeni ağ paketi (Örnek rastgele veri)
sample_packet = np.random.randn(1, 37, 1).astype(np.float32)

# Tahmin
predictions = model.predict(sample_packet)
predicted_class = np.argmax(predictions, axis=1)[0]

siniflar = ['BENIGN', 'Bot', 'DDoS', 'DoS', 'FTP-Patator', 'PortScan', 'SSH-Patator', 'Web Attacks']
print(f"Tahmin Edilen Sınıf: {siniflar[predicted_class]} (Güven Skoru: %{predictions[0][predicted_class]*100:.2f})")
```

---

## 📈 Örnek Sonuç Grafikleri

Proje dizininde yer alan görsel analizler:
- `1d-cnn_best_2_shap/confusion_matrix_normalized.png`: Çok sınıflı tespit oranları hata matrisi.
- `1d-cnn_best_2_shap/roc_curves.png`: Her bir saldırı sınıfı için ROC eğrisi ve AUC değerleri.
- `shap_lime_yeni_analiz/shap_global_ilk30.png`: Saldırı ayrımında küresel olarak en kritik 30 ağ trafiği özniteliği.
- `shap_lime_yeni_analiz/shap_lime_jaccard_stratified.png`: Karar tutarlılığı Jaccard benzerlik dağılım grafiği.

---

## 📜 Lisans ve Atıf

Bu çalışma açık kaynak ve akademik araştırma amaçlı geliştirilmiştir. Projeyi tez veya araştırmalarınızda referans göstermek isterseniz:

```bibtex
@misc{cicids2017_xai_1dcnn,
  title={Explainable Deep Learning-Based Intrusion Detection System Using 1D-CNN and SHAP-LIME on CICIDS2017},
  author={Ahmet},
  year={2026},
  howpublished={\url{https://github.com/ahmet/github-tez}}
}
```
