# HomeContamination
Evde tespit edilebilecek kontaminasyonlar:
## Hava Kaynaklı Kontaminasyonlar:
- Karbon Monoksit (Kombi, soba, şofben, gazlı ocak)
- Karbon Dioksit (insan solunumu)
- VOC (Volatile Organic Compounds) (Boya, mobilya, temizlik ürünleri, benzen vb.)
- Partikül Kirliliği (PM10/PM2.5, Sigara, mum, yemek pişirme, dış ortam kirililiği)
- Biyolojik kontaminasyon (Küf sporları, bakteri, polen)
Kullanılacak Sensörler:
Partikül ---> SDS011 / PMS5003 (SDS011 = 1.600 - 2.000 TL arası)
VOC---> BME680 (sıcaklık da ölçer) (200-1.100 TL)
CO----> MQ-7 (105 TL)
CO2---> MH-Z19C / SCD30 (390-2.000 TL)
## Su Kaynaklı kontaminasyonlar:
- Kimyasal (Kurşun, Klor, Nitrat/nitrit, Tarım ilaçları)
- Fiziksel (TDS-Toplam Çözünmüş Madde-, Sertlik -Ca,MG-)
- Biyolojik (E.coli, Legionella)
Kullanılacak Sensörler:
TDS --> Gravity TDS sensor (1.000 TL Civarı)
PH ---> pH prob (1.500 - 3.200 TL)
İletkenlik ----> EC Sensör (600 - 1.500 TL)
Sıcaklık ---> DS18B20 (40 TL)
## Radyasyon Kaynaklı Kontaminasyon:
- Radon (önemli, akciğer kanseri riski)
- Doğal Gamma Radyasyonu (Beton, granit, seramik, tuğla)
Kullanılacak Sensörler:
Radon---> Radon Detektörü (10.000 - 20.000 TL)
Gamma---> Geiger-Muller sayacı veya scintillation detector (Geiger Tüpü - SBM20 veya SI-29BG) (SBM20: 950 - 1.900 TL)
### Elektronik Cihaz Radyasyonu Kontaminasyonu: (iyonlaştırıcı radyasyon değil)
- EMF (Elektromanyetik alan) (Wifi, cep telefonu, adaptör, elektrik tesisatı)
- Eski Cihaz Radyasyonu (CRT TV, saat, cam, eski lensler)
Kullanılacak sensörler:
EMF---> Gaussmeter, RF Meter (Gigahertz Solutions, Cornet, HMC5883L) (4.000 - 13.500 TL)

Özetlersek evde ölçülebilecek kntaminasyon listesi:
-- Hava (CO, CO2, VOC, PM2.5)
-- Su (pH, TDS, ağır metal)
-- Radyasyon (Radon, gamma)
-- EMF (RF, Manyetik Alan)

## Cihaz Fiziksel Tasarım Düşünceleri:
Küçük kutu içinde modüler tasarım:
- Üst: Hava sensörleri (doğru ölçüm için avalandırma delikleri olmalı)
- Alt: Su prob girişleri
- Yan : Gaiger Tüpü
- İç: ESP32 ve güç kaynağı

## Zorluklar:
1. Radon ölçümü haftalarca tekrarlanmalı
2. Geiger Tüpü ve gama: Analog sinyal darbeleri üretir. Saymak gerekir.
3. RF/EMF Ölçümü: Sensörler genelde belirli bir frekans aralığında hassas, tüm frekansı kapsamak zor.
4. Kalibrasyon: CO, CO2 ve VOC sensörleri kalibreli gelir ama zamanla drift yapar. pH ve TDS sensörleri ise düzenli kalibrasyon ister.

## Sensör Seçimleri
### Hava Kontaminasyon Sensörleri
  1. CO2 sensörü : SCD41. Çünkü küçük, modern, hata payı çok az, hızlı, SCD30 kadar olmasa da stabil.I2C kullanması avantaj. MH-Z19C UART kullanıyor. Proje için basit kalıyor çünkü kalibrasyonu zamanla bozuluyor. (1.500 TL civarı)
  2. CO sensörü: ZE07-CO sensörü diğerinden (MQ-7) den kat kat daha iyi. Hata vermiyor, MQ7 yanlış okuma yapabiliyor. Bu sensör, tak-çalıştır ve en güzeli profesyonele çok yakın okuma veriyor. Karbonmonoksit zehirli olduğundan riske atmayalım. (Tanesi 408 TL aliexpresste ancak nasıl getirteceğiz bilmiyorum)
  3. VOC (Havadaki uçucu bileşenler Alkoller (etanol,alkol, metanol, kolonya, dezenfektan), solventler (aseton, toluen, benzen, ksilen),  Aldehitler (Formaldehit, Asetaldehit - yeni eşyalardan), Temizlik kimyasalları (Deterjan, sprey, parfüm), Yemek buharı ve yanıkları, Duman (sigara,mum) ve insan kaynaklı VOC'lerin ölçümü için): En iyi sensör SGP40 Sensörü. (yaklaşık 750 TL) VOC indexini veriyor. I2C işlemcili. 0-100 Harika, 100-200 İyi, 200-300 Az Kirli, Havalandırma gerekir, 300-400 Orta Kirli, Temiz hava ile havalandırılşmalı, 400-500 Çok kirli, havalandırma iyi optimize edilmeli. İnsan kaynaklı VOC ölçümü CO2 ölçümü ile birlikte yorumlanmalı. (örn:
     ```
     VOC ↑ + PM ↑ → sigara / yanma
     VOC ↑ + CO2 ↑ → insan / kapalı ortam
     VOC ↑ + PM normal → kimyasal gaz (parfüm, temizlik)
     ```
  4. Partikül Ölçümü: SPS30 sensörü piyasadaki en profesyonel olanın bir altı. Ama ayrı güç kaynağı gerekecek. Kutu içine konulmamalı, dışarıda bir tray içine yerleşmeli. I2C de pullup'a dikkat et. Gerçek ve hassas partikül ölçümü verir. Diğer sensör analizleri ile yorumlanıp havada polen mi, sigara dumanı mı, yemek dumanı mı tespit eder.
  5. Biyolojik Kontaminasyon: Bunu bu cihazla maalesef yapamayacağız. Ama polen ve küf partiküllerinin olup olmadığını yorumlayabiliriz.
