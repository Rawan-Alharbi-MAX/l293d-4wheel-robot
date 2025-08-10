# مشروع تحكم بـ 4 محركات DC باستخدام Arduino و L293D

## 📌 وصف المشروع
هذا المشروع يوضح كيفية التحكم في أربعة محركات DC باستخدام شريحتين L293D مع Arduino، لتنفيذ حركات محددة:
1. التحرك للأمام لمدة 30 ثانية.
2. التحرك للخلف لمدة 60 ثانية.
3. الالتفاف يمينًا ويسارًا بالتناوب لمدة 60 ثانية.

تم تنفيذ المشروع على منصة **Tinkercad** ويمكن تطبيقه على أرض الواقع.

---

## 🛠 المكونات المطلوبة
- لوحة Arduino UNO
- شريحتين L293D Motor Driver
- 4 محركات DC (M1, M2, M3, M4)
- بطارية 9V أو 12V (لتغذية المحركات)
- أسلاك توصيل (Jumper Wires)


---

## 🔌 التوصيلات

### الشريحة الأولى (المحرك الأمامي الأيسر + الأمامي الأيمن)
| رجل L293D | يوصل بـ |
|-----------|---------|
| Pin 1 (EN1)  | 5V من Arduino أو مخرج PWM |
| Pin 2 (IN1)  | Arduino Pin 2 |
| Pin 3 (OUT1) | طرف محرك M1 (أمامي يسار) |
| Pin 4 (GND)  | GND مشترك |
| Pin 5 (GND)  | GND مشترك |
| Pin 6 (OUT2) | طرف محرك M1 الآخر |
| Pin 7 (IN2)  | Arduino Pin 3 |
| Pin 8 (Vcc2) | بطارية 9V/12V (موجب) |
| Pin 16 (Vcc1)| 5V من Arduino |
| Pin 15 (IN3) | Arduino Pin 4 |
| Pin 14 (OUT3)| طرف محرك M2 (أمامي يمين) |
| Pin 13 (GND) | GND مشترك |
| Pin 12 (GND) | GND مشترك |
| Pin 11 (OUT4)| طرف محرك M2 الآخر |
| Pin 10 (IN4) | Arduino Pin 5 |
| Pin 9 (EN2)  | 5V أو مخرج PWM |

### الشريحة الثانية (المحرك الخلفي الأيسر + الخلفي الأيمن)
| رجل L293D | يوصل بـ |
|-----------|---------|
| Pin 1 (EN1)  | 5V |
| Pin 2 (IN1)  | Arduino Pin 6 |
| Pin 3 (OUT1) | طرف محرك M3 (خلفي يسار) |
| Pin 4 (GND)  | GND مشترك |
| Pin 5 (GND)  | GND مشترك |
| Pin 6 (OUT2) | طرف محرك M3 الآخر |
| Pin 7 (IN2)  | Arduino Pin 7 |
| Pin 8 (Vcc2) | بطارية 9V/12V (موجب) |
| Pin 16 (Vcc1)| 5V من Arduino |
| Pin 15 (IN3) | Arduino Pin 8 |
| Pin 14 (OUT3)| طرف محرك M4 (خلفي يمين) |
| Pin 13 (GND) | GND مشترك |
| Pin 12 (GND) | GND مشترك |
| Pin 11 (OUT4)| طرف محرك M4 الآخر |
| Pin 10 (IN4) | Arduino Pin 9 |
| Pin 9 (EN2)  | 5V |

⚠️ **مهم**: توصيل السالب من البطارية مع GND Arduino لضمان المرجع المشترك للإشارات.

---

## 💻 الكود البرمجي

```cpp
// المحرك الأمامي الأيسر
int M1_IN1 = 2;
int M1_IN2 = 3;
// المحرك الأمامي الأيمن
int M2_IN3 = 4;
int M2_IN4 = 5;
// المحرك الخلفي الأيسر
int M3_IN1 = 6;
int M3_IN2 = 7;
// المحرك الخلفي الأيمن
int M4_IN3 = 8;
int M4_IN4 = 9;

void setup() {
  pinMode(M1_IN1, OUTPUT);
  pinMode(M1_IN2, OUTPUT);
  pinMode(M2_IN3, OUTPUT);
  pinMode(M2_IN4, OUTPUT);
  pinMode(M3_IN1, OUTPUT);
  pinMode(M3_IN2, OUTPUT);
  pinMode(M4_IN3, OUTPUT);
  pinMode(M4_IN4, OUTPUT);
}

void forward() {
  digitalWrite(M1_IN1, HIGH); digitalWrite(M1_IN2, LOW);
  digitalWrite(M2_IN3, HIGH); digitalWrite(M2_IN4, LOW);
  digitalWrite(M3_IN1, HIGH); digitalWrite(M3_IN2, LOW);
  digitalWrite(M4_IN3, HIGH); digitalWrite(M4_IN4, LOW);
}

void backward() {
  digitalWrite(M1_IN1, LOW); digitalWrite(M1_IN2, HIGH);
  digitalWrite(M2_IN3, LOW); digitalWrite(M2_IN4, HIGH);
  digitalWrite(M3_IN1, LOW); digitalWrite(M3_IN2, HIGH);
  digitalWrite(M4_IN3, LOW); digitalWrite(M4_IN4, HIGH);
}

void rightTurn() {
  digitalWrite(M1_IN1, HIGH); digitalWrite(M1_IN2, LOW);
  digitalWrite(M2_IN3, LOW);  digitalWrite(M2_IN4, HIGH);
  digitalWrite(M3_IN1, HIGH); digitalWrite(M3_IN2, LOW);
  digitalWrite(M4_IN3, LOW);  digitalWrite(M4_IN4, HIGH);
}

void leftTurn() {
  digitalWrite(M1_IN1, LOW);  digitalWrite(M1_IN2, HIGH);
  digitalWrite(M2_IN3, HIGH); digitalWrite(M2_IN4, LOW);
  digitalWrite(M3_IN1, LOW);  digitalWrite(M3_IN2, HIGH);
  digitalWrite(M4_IN3, HIGH); digitalWrite(M4_IN4, LOW);
}

void stopMotors() {
  digitalWrite(M1_IN1, LOW); digitalWrite(M1_IN2, LOW);
  digitalWrite(M2_IN3, LOW); digitalWrite(M2_IN4, LOW);
  digitalWrite(M3_IN1, LOW); digitalWrite(M3_IN2, LOW);
  digitalWrite(M4_IN3, LOW); digitalWrite(M4_IN4, LOW);
}

void loop() {
  forward(); delay(30000); stopMotors(); delay(1000);
  backward(); delay(60000); stopMotors(); delay(1000);
  unsigned long startTime = millis();
  while (millis() - startTime < 60000) {
    rightTurn(); delay(1000);
    leftTurn(); delay(1000);
  }
  stopMotors(); delay(2000);
}


