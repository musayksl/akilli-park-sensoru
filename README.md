# 🚗 Arduino Akıllı Park Sensörü Projesi

Bu proje, Arduino ile yapılmış temel bir **park yardım sistemi** örneğidir. HC-SR04 ultrasonik sensör ile aracın önündeki mesafe ölçülür ve mesafeye bağlı olarak:

- LED'ler ile **görsel uyarı** verilir (Yeşil, Sarı, Kırmızı)
- Buzzer ile **sesli uyarı** sağlanır

Bu sistem, hem öğrenmek isteyenler için güzel bir başlangıç projesidir hem de robotik veya IoT projeleri için temel oluşturabilir.

## 🧰 Kullanılan Malzemeler

- 1x Arduino Uno 
- 1x HC-SR04 ultrasonik mesafe sensörü
- 1x Yeşil LED
- 1x Sarı LED
- 1x Kırmızı LED
- 1x Buzzer
- 3x 220Ω direnç
- Jumper kablolar + Breadboard

## ⚙️ Sistem Mantığı

| Mesafe (cm) | LED     | Buzzer Davranışı        |
|-------------|---------|--------------------------|
| > 20 cm     | Yeşil   | Her 1 saniyede bir ötme  |
| 10–20 cm    | Sarı    | 0.5 saniyede bir ötme    |
| 5–10 cm    | Kırmızı | 0.2 saniyede bir ötme    |
| < 5 cm     | Kırmızı | Sürekli ötme             |

## 💡 Özellikler

- Gerçek zamanlı mesafe algılama
- Duruma göre değişen LED uyarısı
- Dinamik buzzer kontrolü
- Geliştirilebilir, modüler yapı

## 🔌 Bağlantı Bilgileri

HC-SR04:
- Vcc → 5V
- Trig → D9
- Echo → D10
- Gnd → Gnd
  
LED’ler:
- Yeşil → D2  
- Sarı → D3  
- Kırmızı → D4  

Buzzer:
- + → D8  
- - → GND

Kod: 
```cpp
#define trigPin 9
#define echoPin 10
#define buzzer 8
#define greenLED 2
#define yellowLED 3
#define redLED 4

void setup() {
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(buzzer, OUTPUT);
  pinMode(greenLED, OUTPUT);
  pinMode(yellowLED, OUTPUT);
  pinMode(redLED, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  long duration;
  float distance;

  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);

  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  duration = pulseIn(echoPin, HIGH);
  distance = duration * 0.034 / 2;

  Serial.print("Mesafe: ");
  Serial.print(distance);
  Serial.println(" cm");

  if (distance > 20) {
    showLED(greenLED);
    buzzWithDelay(1000);
  } 
  else if (distance > 10 && distance <= 20) {
    showLED(yellowLED);
    buzzWithDelay(500);
  } 
  else if (distance > 5 && distance <= 10) {
    showLED(redLED);
    buzzWithDelay(200);
  } 
  else if (distance <= 5) {
    showLED(redLED);
    digitalWrite(buzzer, HIGH);  // Sürekli öter
    delay(100);
  }
}

void showLED(int ledPin) {
  digitalWrite(greenLED, LOW);
  digitalWrite(yellowLED, LOW);
  digitalWrite(redLED, LOW);
  digitalWrite(ledPin, HIGH);
}

void buzzWithDelay(int delayTime) {
  digitalWrite(buzzer, HIGH);
  delay(100);
  digitalWrite(buzzer, LOW);
  delay(delayTime);
}
```

## 🚀 Nasıl Başlanır?

1. Breadboard üzerinde bağlantıları yap.
2. Kodları yükle.
3. Seri monitörden mesafeyi takip et, sistemi gözlemle.

## 📄 Lisans

Bu proje MIT lisansı ile paylaşılmıştır. İsteyen herkes kullanabilir, geliştirebilir ve paylaşabilir.

---

🧠 Fikir sahibi & geliştirici: **[Musa Yüksel]**  
