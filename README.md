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

  <img width="1536" height="1024" alt="ChatGPT Image 8 Nis 2026 15_55_03" src="https://github.com/user-attachments/assets/2b77f275-391c-4282-8031-32b7c3ba8d64" />


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
  4. Partikül Ölçümü: SPS30 sensörü piyasadaki en profesyonel olanın bir altı. Ama ayrı güç kaynağı gerekecek. Kutu içine konulmamalı, dışarıda bir tray içine yerleşmeli. I2C de pullup'a dikkat et. Gerçek ve hassas partikül ölçümü verir. Diğer sensör analizleri ile yorumlanıp havada polen mi, sigara dumanı mı, yemek dumanı mı tespit eder. (2.000 TL Civarı)
  5. Biyolojik Kontaminasyon: Bunu bu cihazla maalesef yapamayacağız. Ama polen ve küf partiküllerinin olup olmadığını yorumlayabiliriz.
  6. NO2 (Trafik hava kirliliği) sensörü: ZE07-NO2 çok iyi bir sensör. (Çin fiyatı 500 TL gibi ama nasıl gelecek bilmem)  ppm olarak veriyor. 2 sene sonra kalibre edilmeli. Daha basit olan MICS-6814 modeli rahatça bulunabilir (2.000 TL) ama profesyonel değil.Trend algılar ölçüm vermez. Bunu da ekleyince sistem mükemmel bir hava kalite ölçüm istasyonu olacak.

### Su Kontaminasyon Sensörleri
1. pH sensörü: Gravity Analog pH Sensörü V2 (2.500 TL civarı) Alt segment bir ölçüm cihazı. Çok daha iyisi ve işimize yarayanı 200$'lık Atlas'ın "Atlas Scientific PH Sensor Kit" ürünü. Olmazsa olmaz bir ölçüm için gerekli. Diğer sensörlerin yorumunu da etkiliyor. Uzun vadede stabil. Atlas ürünlerini "Creasis.shop" internet sitesinden bulabilirsin.
2. EC / İletkenlik Sensörü: Atlas Scientific EZO EC Kit. (215 $) Su içindeki çözünmüş iyonları ölçer.Profesyonel sistemlerin temelidir. TDS buradan hesaplanır.
3. Sıcaklık Sensörü: Diğer tüm sensör okumalarına etki eder. Ucuz ve stabil. DS18B20 Waterproof Temperature Sensor. ( 53 TL)
4. ORP (oksidasyon / dezenfeksiyon) Sensörü: Atlas Scientific EZO ORP Kit (253 $) Su temiz mi okside mi görür. Klor etkisini ortaya çıkarır. Biyolojik risk analizinde kritiktir.
5. Bulanıklık (fiziksel kirlilik) Sensörü: DFRobot Gravity Turbidity Sensor (730 TL) Pas, tortu, çamur, biyofilm tespiti yapar. Musluk filtrelerinin ve boruların kirliliğini verir.
6. Klor ölçümü (reaktif + optik sistem): Bu sensör değil bir test kiti olacak. Testler suda çözülen haplarla kolayca yapılıyor. Bu haplar 400 TL ve 50 ölçüm yapılabilir. TCS34725 renk sensörüyle birlikte klor miktarını düzgün ölçem bir sistem yapılabilir. Profesyonel cihazlar da böyle çalışıyor.
7. Nitrat ve Nitrit (Tarım ilacı / Gübre Testi): Bu da test olarak Kolorimetrik test (reaktif + optik) standartlarındadır. Bunu detaylı yazmalıyım. Her ikisi için de farklı rektif katılır, sonra TCS34725 renk sensörüyle renk okunarak değer hesaplanır. Özel koşullar gerekli. Test ortamı hiç ışık almamalı, aydınlatma ledi iyi seçilmeli. Mini manyetik karıştırıcı eklenmeli, Sensör stabil olmalı. Nitrit ölçümünde (NO2-) Griess Reaksiyonu yapılır. Bunun için kullanılan reaktifler: sulfanilamide N-(1-naphthyl)ethylenediamine dihydrochloride (NED). Bunlar karışınca nitrit varsa pembe renk çıkar. Konsantrasyon ile miktarı artar. Renk sensörü ile
* RGB veya absorbans okur
* Kalibrasyon eğrisi uygular
* ppm değer hesaplar
  Nitrat (NO3-) direkt hesaplanamaz. Önce indirgenir ve nitrite döndürülür. İndirgeme için cadmium reduction column kullanılır. Ardından nitrit'teki reaktiflerle aktive edilip rengi yorumlanır. Daha kolay olarak:
* API nitrate test kit
* JBL nitrate kit
* Hach nitrate reagent kullanılabilir.
  <br> Kutu tasarımında (çekmece) sabit dalga boylu led kullanılmalı. (520–540 nm). Okumalar küçük gelirse op-amp devresi ile yükseltilmelidir. Test iş akışı da şöyle olmalıdır:
  * Kullanıcı suyu koyar
  *  Cihaz kapağı kapatır
  *  Reaktif otomatik eklenir
  *  2–5 dk beklenir
  *  LED yanar
  *  sensör okur
  *  sonuç hesaplanır
8. Çözünmüş oksijen (DO) Sensörü: Atlas Scientific EZO DO Kit ile okunabilir. Suyun bir nevi biyolojik analizini yapar.(461 $)
9. Fosfat ve Amonyak testleri için de kolorimetrik uygulama kullanılır.
10. Sertlik (Ca, Mg): Sensör yerine reaktif test (EDTA titrasyon kitleri) ile sonuca ulaşılır.

## Ara Değerlendirme
Başlandıçta bu kadar karmaşık bir sistem olacağını düşündüysem de özellikle su kontaminasyonu tayini için gereken kullanıcı taraflı testlerin zorluğu karşısında, en azından projenin bu ilk versiyonunda Su, radyasyon ve Elektromagnetik kontaminasyon tespitine ilişkin sensörleri çıkarmak zorunda kaldım. Bu durumda Hava kalitesini ölçen, log tutan, alarm veren ve yorum yapan bir sistemle yola devam etmeye karar verdim. Bu doğrultuda seçtiğim hava kontaminasyonu unsurları ve sensörleri şu şekilde belirlendi:
| Parametre             | Sensör                           |
| --------------------- | -------------------------------- |
| CO₂                   | SCD41                            |
| VOC                   | SGP40                            |
| PM1/PM2.5/PM4/PM10    | SPS30                            |
| CO                    | SPEC 3SP CO 0–3V + ADS1115       |
| NO₂                   | ZE07-NO2                         |
| Sıcaklık/Nem          | SHT31/SHT35                      |
| Radon                 | RD200M / RadonEye tabanlı modül  |
| **Metan / doğal gaz** | **TGS2611-E00 veya MPS Methane** |
Bu durumda, sadece hava kalitesi gösteren ve piyasada 7.000 - 22.000 TL aralığında 2026 fiyatları ile satılan Çin menşeili ürünlere benzer bir sistem hazırlayacağım.

# SİSTEMDE KULLANILACAK MİKROKONTROLCÜ VE EKRAN
## Microcontroller 
Uzun araştırmalarım sonucunda fiyat / performans oranını da düşünerek ESP32S3 Devkit C-1 N16R8 modelinde karar kıldım. İlk izlenimlerime göre bütün işi kaldıracak kapasitede. Wifi ve bluetooth bağlatıları da mevcut. Güvenlik sertifikası yüklenebiliyor. Küçük hacimli, pin sayısı 47. Sistemin omurgası için yeterli görünmekte. Sadece sistemde güç dağılımını iyi yapmak gerekiyor. Bunu ilerleyen safhalarda PSU değerlendirmesi ile birlikte ele alacağım.Bu anlamda aldığım işlemci oldukça ucuz yaklaşık 400 TL 2026 fiyatı ile ancak usb mikro ile çalışıyor. Bir daha alırken USB-C arabirimli olan modelini almak gerektiğini anladım.
<img width="1024" height="1024" alt="YEUNULJKED8122025122743_ESP32-S3-DevKitC-1_N16R8_WiFi_ve_Bluetooth_gelisti" src="https://github.com/user-attachments/assets/6ff9ffa8-7afc-4c07-b7e5-ad8ad2b44c3b" />

## Display
Ekran konusunda, güzel görünmesini istediğimden ve özellikle ev kullanıcılarına yönelik User Friendly bir bilgi ekranı yapmak istediğimden, LCD ve Oled seçeneklerini eledim. Geriye kalan seçeneklerden fiyat olarak en uygun olanı TFT SPI ekranlar oldu. Öncelikle Ili9488 modeli ile deneme yaptım. Cihaz ekrana bilgileri istediğim gibi verdi. ancak benim istediğim kalitede bir çıktı almayı başaramadım. Bunun sebebinin LVGL altyapısını kullanamaması ve SPI olmaması olduğunu anladım. Tüm çabalarıma rağmen 8 bitte düzgün çalışan ekran 16 bit ve yüksek renk kalitesine, custom fontlara geçince abuk subuk görüntüler vermeye başladı. Bunun üzerine öncelikle arabirimimin yapay zeka yardımı ile neye benzeyeceği konusunda bir çalışma yaparak ilk taslağı ortaya çıkardım. Şöyle bir şey istediğime karar verdim:
<img width="1536" height="1024" alt="ChatGPT Image 13 May 2026 13_28_11" src="https://github.com/user-attachments/assets/89ac4f35-2391-4635-98d6-1cda95b68a2d" />
Böylece ekranı dokunmatik bir ekranla değiştirdim. Yine TFT 4" kullandım. Bu kez ST7796 SPI sipariş ettim. Hem ekran büyüdü, hem de 20 pin yerine sadece 4 pin kullanacağından mikro işlemcideki pin kontrolü daha kolay hale geldi. Siprarişim geldikten sonra Ekranın LVGL olarak nasıl çalıştığını test edeceğim. SPI'ın paralelden daha yavaş olduğunu biliyoruz, ama başka çarem kalmadı. İleride, TFT ekran yerine amoled ekran kullanmak istiyorum. Ancak fiyat farkı o kadar çok ki. 2026 TFT fiyatı 1.400 TL civarı iken Amoled ekranlar 4-7.000 TL ciavrı alınabiliyor. Daha büyük ve pahalı olmasının yanı sıra HDMI girişleri olduğundan ESP ile bağlantısını baştan düşünmek gerekecek.Bunun yerine belki mikroişlenciyi bile değiştirmeyi düşünebiliriz. Öncelikle TFT ekran ile sonuç alabilecek miyim ona bakacağım. İlk hedef ST7796 SPI
<img width="300" height="300" alt="300px-MSP4021-002" src="https://github.com/user-attachments/assets/12fbe210-6683-482d-84a1-8bfe7c68e7eb" />.


