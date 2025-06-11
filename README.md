# 自走車專案
電子電路學期末報告，用arduino版與arduino程式做出一個會沿著黑線走的自走車
# 一、專案概述
本專案旨在開發一款 Arduino 自走車，具備 紅外線循跡 及 超音波避障 功能，以提升自動導航能力。透過精確的感測器數據與智能控制演算法，使車輛能夠自主運行，適應各種環境。
# 二、專案目標
1.設計具備 循跡與避障 能力的自走車模型。

2.利用 Arduino 與 L298N 馬達驅動模組 控制車輛運動。

3.透過 HC-SR04 超音波感測器 精確偵測障礙物距離。

4.研究 PWM 速度調控 以優化行進狀態。
# 三、硬體設備

| **元件**             | **數量** | **說明**           |
|----------------------|---------|------------------|
| Arduino Uno         | 1       | 主控制器         |
| L298N 馬達驅動模組  | 1       | 控制車輛的雙馬達  |
| 直流馬達           | 2       | 車輛驅動         |
| HC-SR04 超音波感測器 | 1       | 障礙物偵測       |
| 紅外線循跡感測器   | 2       | 軌跡追蹤         |

# 四、系統架構
車輛運作流程如下：

1.循跡模式：透過紅外線感測器偵測路徑，調整左右馬達以穩定行進。

2.避障機制：當前方障礙物低於設定距離時，車輛停下或轉向。

3.PWM 速度控制：調節馬達轉速，以適應不同環境需求。
# 五、程式碼示範
// 馬達與感測器腳位設定
const int ENA = 3, IN1 = 7, IN2 = 6;

const int ENB = 5, IN3 = 2, IN4 = 4;

const int IR_L = 8, IR_R = 9;


void setup() {

pinMode(ENA, OUTPUT);

pinMode(IN1, OUTPUT);

pinMode(IN2, OUTPUT);

pinMode(ENB, OUTPUT);

pinMode(IN3, OUTPUT);

pinMode(IN4, OUTPUT);

pinMode(IR_L, INPUT);

pinMode(IR_R, INPUT);

Serial.begin(9600);

}



void loop() {

int leftIR = digitalRead(IR_L);

int rightIR = digitalRead(IR_R);



// 前進
if (leftIR == LOW && rightIR == LOW) {

moveForward(80);

}

// 偵測右邊是黑線，需左轉修正

else if (leftIR == LOW && rightIR == HIGH) {

adjustLeft();

}

// 偵測左邊是黑線，需右轉修正

else if (leftIR == HIGH && rightIR == LOW) {

adjustRight();

}

// 停止

else {

stopMotors();

}

}



// 馬達控制函式

void moveForward(int speed) {

digitalWrite(IN1, HIGH);

digitalWrite(IN2, LOW);

digitalWrite(IN3, HIGH);

digitalWrite(IN4, LOW);

analogWrite(ENA, speed);

analogWrite(ENB, speed);

}



void stopMotors() {

analogWrite(ENA, 0);

analogWrite(ENB, 0);

}


void adjustLeft() {

stopMotors();

delay(250);

reverse();

delay(250);

turnLeft();

delay(750);

}


void adjustRight() {

stopMotors();

delay(250);

reverse();

delay(250);

turnRight();

delay(750);

}



void reverse() {

digitalWrite(IN1, LOW);

digitalWrite(IN2, HIGH);

digitalWrite(IN3, LOW);

digitalWrite(IN4, HIGH);

analogWrite(ENA, 80);

analogWrite(ENB, 80);

}



void turnLeft() {

digitalWrite(IN1, HIGH);

digitalWrite(IN2, LOW); // 左輪正轉

digitalWrite(IN3, LOW);

digitalWrite(IN4, HIGH); // 右輪反轉

analogWrite(ENA, 150);

analogWrite(ENB, 150);

}



void turnRight() {

digitalWrite(IN1, LOW);

digitalWrite(IN2, HIGH); // 左輪反轉

digitalWrite(IN3, HIGH);

digitalWrite(IN4, LOW); // 右輪正轉

analogWrite(ENA, 150);

analogWrite(ENB, 150);

}

# 六、預期成果
1.成功開發 可循跡與避障 的自走車。

2.提升 自動導航穩定性，確保運作準確。

3.提供開放原始碼，讓使用者能夠改進與擴展功能。
# 七、遇到的困難
❶ 馬達時轉時不轉，控制程式未變

❷ 第二次偵測時，馬達有聲音但無法轉動

❸ 有時連起步都無法轉動，但更換硬體後仍無解

❹ 馬達僅能轉幾秒鐘後就沒力，或完全停止

❺ 所有硬體（電池、驅動、馬達）測試正常，但問題仍出現
# 七、外觀設計
❶ 黑色防撞條
❷ 粉色備胎

