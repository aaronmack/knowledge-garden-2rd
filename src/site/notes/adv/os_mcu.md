---
{"dg-publish":true,"permalink":"/adv/os-mcu/","title":"OS - MCU","noteIcon":""}
---

# Attiny 85

参考: https://oshwhub.com/15653072438poiu/ji-yu-attiny85-de-tang-guo-he-you-xi-ji

需要Arduino，Attiny85,杜邦线

1. Arduino IDE中选择 示例-> ArduinoISP 打开示例
2. Arduino IDE中在Tools中Board->Arduino Uno，选择端口，Programmer选择ArduinoISP。（实际测试中选择了Arduino As ISP，不知有和影响）
3. 面包板上连接Arduino与Attiny85 （要参考Attiny85的pinout针脚图）

| Arduino Uno | Attiny85 |
| ----------- | -------- |
| 5V          | VCC      |
| GND         | GND      |
| Pin13       | PB2      |
| Pin12       | PB1      |
| Pin 11      | PB0      |
| Pin 10      | PB5      |
4. Arduino IDE中Tools->Burnbootloader （烧录一次成功即可）
5. Arduino IDE中打开游戏代码。在Sketch中选择，Upload using programmer。然后编译上传即可。
6. 简单测试：将OLED与Attiny85连接。

<img src="https://github.com/aaronmack/picx-images-hosting/raw/master/e/image.3k7w3h0dmk.webp" alt="image" width=500/>


# STM32


# ESP-32S Cam

在Arduino -> File -> Examples -> ESP32 -> Camera -> CameraWebServer

```c
# 1. 在CameraWebServer.ino 文件中 取消下面注释

#define CAMERA_MODEL_AI_THINKER // Has PSRAM

# 2. 修改 ssid与密码
const char *ssid = "**********";
const char *password = "**********";
```


# Micropython

## ESP32-WROOM-32

操作系统 Windows

```bash
# 1. 在网站 https://micropython.org/download/ 下载对应MCU的固件 .bin
# 2. 在设备管理器中找到插入的ESP32的COM端口是多少，例如 COM5
# 3. 新建一个python venv环境，安装 pip install esptool

esptool.exe --chip esp32 --port COM5 erase_flash

esptool.exe --chip esp32 --port COM5 --baud 460800 write_flash -z 0x1000 c:\tmp\esp32-20190125-v1.10.bin

```

下载Mu Editor https://codewith.mu/ 编写python代码。

* LED Blink

```python
from machine import Pin, Timer
import time
led = Pin(2, Pin.OUT)
timer = Timer(0)
toggle = 1
def blink(timer):
    global toggle
    if toggle == 1:
        led.value(0)
        toggle = 0
    else:
        led.value(1)
        toggle = 1
timer.init(freq=3, mode=Timer.PERIODIC, callback=blink)
```

* Web Server

```python
# Complete project details at https://RandomNerdTutorials.com

try:
  import usocket as socket
except:
  import socket

from machine import Pin
import network

import esp
esp.osdebug(None)

import gc
gc.collect()

ssid = '****'
password = '****'

station = network.WLAN(network.STA_IF)

station.active(True)
station.connect(ssid, password)

while station.isconnected() == False:
  pass

print('Connection successful')
print(station.ifconfig())

# LED Value 1 is close.
led = Pin(2, Pin.OUT)
print("led value", led.value())
led.value(0)

def web_page():
  if led.value() == 0:
    gpio_state="ON"
  else:
    gpio_state="OFF"
  
  html = """<html><head> <title>ESP Web Server</title> <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="icon" href="data:,"> <style>html{font-family: Helvetica; display:inline-block; margin: 0px auto; text-align: center;}
  h1{color: #0F3376; padding: 2vh;}p{font-size: 1.5rem;}.button{display: inline-block; background-color: #e7bd3b; border: none; 
  border-radius: 4px; color: white; padding: 16px 40px; text-decoration: none; font-size: 30px; margin: 2px; cursor: pointer;}
  .button2{background-color: #4286f4;}</style></head><body> <h1>ESP Web Server</h1> 
  <p>GPIO state: <strong>""" + gpio_state + """</strong></p><p><a href="/?led=on"><button class="button">ON</button></a></p>
  <p><a href="/?led=off"><button class="button button2">OFF</button></a></p></body></html>"""
  return html

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.bind(('', 80))
s.listen(5)

while True:
  conn, addr = s.accept()
  print('Got a connection from %s' % str(addr))
  request = conn.recv(1024)
  request = str(request)
  print('Content = %s' % request)
  led_on = request.find('/?led=on')
  led_off = request.find('/?led=off')
  if led_on == 6:
    print('LED ON')
    led.value(0)
  if led_off == 6:
    print('LED OFF')
    led.value(1)
  response = web_page()
  conn.send('HTTP/1.1 200 OK\n')
  conn.send('Content-Type: text/html\n')
  conn.send('Connection: close\n\n')
  conn.sendall(response)
  conn.close()

```
## ESP 32 C3 Super/Mini

```bash
esptool.exe --chip esp32c3 --port COM13 erase_flash

esptool.exe --chip esp32c3 --port COM13 --baud 460800 write_flash -z 0x0 <BIN_FILE>.bin
```


* LED Blink

```python
from machine import Pin
import time
led = Pin(8, Pin.OUT)
for i in range(10):
  time.sleep_ms(500)  
  led.off()
  time.sleep_ms(500)
  led.on()
  print("Blink ", i+1)
```

IDE可以用Thonny。


# ATINY85


# Arduino

Additional Boards Manager URLs

```json
http://dan.drown.org/stm32duino/package_STM32duino_index.json
https://arduino.esp8266.com/stable/package_esp8266com_index.json
https://dl.espressif.com/dl/package_esp32_index.json
https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
https://raw.githubusercontent.com/damellis/attiny/ide-1.6.x-boards-manager/package_damellis_attiny_index.json
https://raw.githubusercontent.com/digistump/arduino-boards-index/master/package_digistump_index.json
```

Libraries

```bash
WiFiManager
WebSockets

ArduinoJson
AccelStepper
Adafruit_NeoPixel
Adafruit_NeoMatrix
Arduino-PID-Library
ArduPID

FastLED
PubSubClient
```

## 摇杆读取

```c
int JoyStick_X = A1; // X轴连接到A1引脚
int JoyStick_Y = A0; // Y轴连接到A0引脚
int JoyStick_Z = 3;  // Z轴连接到数字引脚D3
 
void setup() {
  pinMode(JoyStick_Z, INPUT);
  Serial.begin(9600); // 设置串口波特率为9600
}
 
void loop() {
  int x, y, z;
  x = analogRead(JoyStick_X); // 读取X轴的模拟值
  y = analogRead(JoyStick_Y); // 读取Y轴的模拟值
  z = digitalRead(JoyStick_Z); // 读取Z轴的数字值
 
  Serial.print("X: ");
  Serial.print(x);
  Serial.print(" Y: ");
  Serial.print(y);
  Serial.print(" Z: ");
  Serial.println(z);
 
  delay(100); // 延迟1秒
}
```

# OLED 0.96 IIC 128x64

开发板 UNO R3
1. SCL连接 开发板A5
2. SDA连接开发板A4

```c
// Ref: http://www.taichi-maker.com/homepage/reference-index/display-reference-index/arduino-oled-application/
// 引入IIC通讯所需的Wire库文件
// 教程参考http://www.taichi-maker.com/homepage/reference-index/arduino-library-index/wire-library/
#include <Wire.h>

// 引入驱动OLED0.96所需的库
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

#define SCREEN_WIDTH 128 // 设置OLED宽度,单位:像素
#define SCREEN_HEIGHT 64 // 设置OLED高度,单位:像素

// 自定义重置引脚,虽然教程未使用,但却是Adafruit_SSD1306库文件所必需的
#define OLED_RESET 4
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);

void setup()
{
  // 初始化Wire库
  //  Wire.begin();

  // 初始化OLED并设置其IIC地址为 0x3C
  display.begin(SSD1306_SWITCHCAPVCC, 0x3C);
}

void loop()
{
  words_display();
  display.display();
}

void words_display()
{
  // 清除屏幕
  display.clearDisplay();

  // 设置字体颜色,白色可见
  display.setTextColor(WHITE);

  //设置字体大小
  display.setTextSize(1.5);

  //设置光标位置
  display.setCursor(0, 0);
  display.print("TaichiMaker");

  display.setCursor(0, 20);
  display.print("time: ");
  //打印自开发板重置以来的秒数：
  display.print(millis() / 1000);
  display.print(" s");

  display.setCursor(0, 40);
  display.print("Author: ");
  display.print("Dapenson");
}

```


# 超声波雷达

* 开发板 ESP32 - S2 -Mini，IDE是 Thonny

1.超声波雷达端口VCC连接开发板VBUS，GND连接开发板GND
2.超声波雷达端口Trig连接1，Echo连接12.

```python
from machine import Pin
import time
 
trig=Pin(1,Pin.OUT,value=0)
echo=Pin(12,Pin.IN,value=0)
 
def measure():
    #发出开始测距信号
    trig.value(1)
    time.sleep_us(10)
    trig.value(0)
    #定时器计时
    while echo.value()==0:
        t1 = time.ticks_us()
    while echo.value()==1:
        t2 = time.ticks_us()
    t3 = time.ticks_diff(t2, t1) / 10000
    return t3 * 170
 
try:
    while True:
        print("当前距离:%0.2f cm" % measure())
        time.sleep(1)
except KeyboardInterrupt:
    pass
```

# 步进电机控制

## 28BYJ-48

开发板UNO R3， 驱动板的IN1/2/3/4分别连接开发板的8/9/10/11

```c

//https://blog.csdn.net/sxstj/article/details/132106677

//Includes the Arduino Stepper Library
#include <Stepper.h>
 
// Defines the number of steps per rotation
const int stepsPerRevolution = 2038;
 
// Creates an instance of stepper class
// Pins entered in sequence IN1-IN3-IN2-IN4 for proper step sequence
Stepper myStepper = Stepper(stepsPerRevolution, 8, 10, 9, 11);
 
void setup() {
    // Nothing to do (Stepper Library sets pins as outputs)
}
 
void loop() {
	// Rotate CW slowly at 5 RPM
	myStepper.setSpeed(5);
	myStepper.step(stepsPerRevolution);
	delay(1000);
	
	// Rotate CCW quickly at 10 RPM
	myStepper.setSpeed(10);
	myStepper.step(-stepsPerRevolution);
	delay(1000);
}
```


```c

// Include the AccelStepper Library
#include <AccelStepper.h>
 
// Define step constant
#define MotorInterfaceType 4
 
// Creates an instance
// Pins entered in sequence IN1-IN3-IN2-IN4 for proper step sequence
AccelStepper myStepper(MotorInterfaceType, 8, 10, 9, 11);
 
void setup() {
	// set the maximum speed, acceleration factor,
	// initial speed and the target position
	myStepper.setMaxSpeed(1000.0);
	myStepper.setAcceleration(50.0);
	myStepper.setSpeed(200);
	myStepper.moveTo(2038);
}
 
void loop() {
	// Change direction once the motor reaches target position
	if (myStepper.distanceToGo() == 0) 
		myStepper.moveTo(-myStepper.currentPosition());
 
	// Move the motor one step
	myStepper.run();
}
```

# 舵机

## 180°

```c
#include <Servo.h>

Servo mg996r;
int pos = 0;

void setup() {
  mg996r.attach(4, 500, 2500);
}

void loop() {
  // mg996r.write(90);
  // delay(1000);
  // mg996r.write(0);
  // delay(1000);

  for(pos = 0; pos < 90; pos += 1)  
  {  
    mg996r.write(pos);  
    delay(5);
  }
  for(pos = 90; pos>=1; pos-=1)
  {                              
    mg996r.write(pos);
    delay(5);
  }
}
```

## 360°

```c
#include <Servo.h>
Servo myservo;
void setup()
{
  myservo.attach(4, 500, 2500);
}
void loop()
{
    myservo.write(45);  delay(3000); //占空比为1.0ms的PWM信号旋转约3秒时间
    myservo.write(90);  delay(1500); //占空比为1.5ms的PWM信号停止1.5秒
    
    myservo.write(0);   delay(2000); //占空比为0.5ms的PWM信号旋转约2秒时间
    myservo.write(90);  delay(1000); //占空比为1.5ms的PWM信号停止1秒
    
    myservo.write(135); delay(1000); //占空比为2.0ms的PWM信号旋转约1秒时间
    myservo.write(90);  delay(500); //占空比为1.5ms的PWM信号停止0.5秒
    
    myservo.write(180); delay(500); //占空比为2.5ms的PWM信号旋转约0.5秒时间
    myservo.write(90);  delay(250); //占空比为1.5ms的PWM信号停止0.25秒
}
```

# FOC控制

## 开环

连接与闭环一样，只不过不用连接编码器。

```c
// Deng's FOC 开环速度控制例程 测试库：SimpleFOC 2.1.1 测试硬件：灯哥开源Mini FOC
// 串口中输入"T+数字"设定两个电机的转速，如设置电机以 10rad/s 转动，输入 "T10"，电机上电时默认会以 5rad/s 转动
// 在使用自己的电机时，请一定记得修改默认极对数，即 BLDCMotor(7) 中的值，设置为自己的极对数数字
// 程序默认设置的供电电压为 12V,用其他电压供电请记得修改 voltage_power_supply , voltage_limit 变量中的值

#include <SimpleFOC.h>
#define pwmA 9 
#define pwmB 10
#define pwmC 11
#define enable_pin 6  //使能引脚

BLDCMotor motor = BLDCMotor(7);
BLDCDriver3PWM driver = BLDCDriver3PWM(pwmA, pwmB, pwmC, enable_pin);


//目标变量
float target_velocity = 10;

//串口指令设置
Commander command = Commander(Serial);
void doTarget(char* cmd) {
  command.scalar(&target_velocity, cmd);
}

void setup() {
  pinMode(LED_BUILTIN, OUTPUT);

  driver.voltage_power_supply = 12;
  driver.init();
  motor.linkDriver(&driver);
  motor.voltage_limit = 3;    // [V]
  motor.velocity_limit = 30;  // [rad/s]

  //开环控制模式设置
  motor.controller = MotionControlType::velocity_openloop;

  //初始化硬件
  motor.init();
  
  //增加 T 指令
  command.add('T', doTarget, "target velocity");

  Serial.begin(115200);
  Serial.println("Motor ready!");
  Serial.println("Set target velocity [rad/s]");
  _delay(1000);
}

void loop() {
  motor.move(target_velocity);

  //用户通讯
  command.run();

  // digitalWrite(LED_BUILTIN, HIGH);  // turn the LED on (HIGH is the voltage level)
  // delay(500);                      // wait for a second
  // digitalWrite(LED_BUILTIN, LOW);   // turn the LED off by making the voltage LOW
  // delay(500);                      // wait for a second
}
```

## 闭环

这里使用2208云台电机，极对数为7，开发板为Uno R3

1.编码器4根线，红黑连接开发板3.3v与GND，黄白分别连接开发板SCL与SDA。
2.驱动板IN1/2/3分别连接开发板9/10/11，驱动板EN(使能)连接开发板6端口，驱动板VCC与GND连接开发板3.3v/5v与GND
3.电机三根相线连接驱动板ABC-OUT接口。
4.供电使用稳压电源供电，12V

```c
/**
Deng's FOC 闭环位置控制例程 测试库：SimpleFOC 2.1.1 测试硬件：灯哥开源Mini FOC
在串口窗口中输入：T+位置，就可以使得两个电机闭环转动
比如让两个电机都转动180°，则输入其弧度制：T3.14
在使用自己的电机时，请一定记得修改默认极对数，即 BLDCMotor(7) 中的值，设置为自己的极对数数字
程序默认设置的供电电压为 12V,用其他电压供电请记得修改 voltage_power_supply , voltage_limit 变量中的值
默认PID针对的电机是 2208，使用自己的电机需要修改PID参数，才能实现更好效果
*/

#include <SimpleFOC.h>
#define pwmA 9 
#define pwmB 10
#define pwmC 11
#define enable_pin 6  //使能引脚

#define ControlType MotionControlType::angle //可以选择 MotionControlType::torque | MotionControlType::velocity | MotionControlType::angle


MagneticSensorI2C sensor = MagneticSensorI2C(AS5600_I2C);
TwoWire I2Cone = TwoWire();

//电机参数
BLDCMotor motor = BLDCMotor(7);
BLDCDriver3PWM driver = BLDCDriver3PWM(pwmA, pwmB, pwmC, enable_pin);

//命令设置
float target = 0;
Commander command = Commander(Serial);
void doTarget(char* cmd) {
  command.scalar(&target, cmd);
}

void setup() {
  //I2Cone.begin(5, 6, 400000UL);  //修改自己接的引脚
  I2Cone.begin();
  sensor.init(&I2Cone);

  //连接motor对象与传感器对象
  motor.linkSensor(&sensor);

  //供电电压设置 [V]
  driver.voltage_power_supply = 12;
  driver.init();

  //连接电机和driver对象
  motor.linkDriver(&driver);

  //FOC模型选择
  motor.foc_modulation = FOCModulationType::SpaceVectorPWM;
  //运动控制模式设置
  motor.controller = ControlType;

  //速度PI环设置
  motor.PID_velocity.P = 0.021;
  motor.PID_velocity.I = 0.12;

  //角度P环设置
  motor.P_angle.P = 20;

  //最大电机限制电机
  motor.voltage_limit = 8;

  //速度低通滤波时间常数
  motor.LPF_velocity.Tf = 0.01;

  //设置最大速度限制
  motor.velocity_limit = 40;

  Serial.begin(115200);
  motor.useMonitoring(Serial);

  //初始化电机
  motor.init();

  //初始化 FOC
  motor.initFOC();

  command.add('T', doTarget, "target velocity");

  Serial.println(F("Motor ready."));
  Serial.println(F("Set the target velocity using serial terminal:"));
}

void loop() {
  Serial.print(sensor.getAngle());
  Serial.println();

  motor.loopFOC();
  motor.move(target);
  
  command.run();
}
```

# 直流有刷电机

## TB6612驱动板 (D24A)


参数：输入电压5.5-15V 单路1.2A最大3.2A

### 电机JGB37-520  (霍尔编码器)

* 驱动板输出: AO1 GND E1A E1B 5V AO2

其中AO1与AO2是电机正负。5V与GND是编码器传感器正负。E1A与E1B是电机AB相。

* 电机输出：M1 GND C1 C2 VCC M2
与TB6612驱动板输出一一对应。

接线与下方对应。


```c

///////////////////TB6612引脚接线///////////////////////////
//直流电机----------TB6612丝印标识----------ArduinoUNO主板引脚
//                     PWMA-----------------3
//                     AIN2-----------------4
//                     AIN1-----------------5
//                     STBY-----------------7
//                     BIN1-----------------8
//                     BIN2-----------------9
//                     PWMB-----------------10
//                     GND------------------GND
//                     ADC------------------A0 (只有带TB6612带稳压模块版才有ADC测量电池电压功能)    
//                     VM-------------------12V电池
//                     VCC------------------5V  （使用带有稳压模块版时，接线变动如下：  5V---------5V  板子可向arduino供电）
//                     GND------------------GND  
//电机A正极-------------AO1
//电机A负极-------------A02
//电机B负极-------------BO2
//电机B正极-------------BO1
//                     GND-----------------GND
//直流电机----------TB6612丝印标识----------ArduinoUNO主板引脚

//定义引脚名称
#define PWMA 3  //3为模拟引脚，用于PWM控制
#define AIN1 5
#define AIN2 4
#define PWMB 10  //10为模拟引脚，用于PWM控制
#define BIN1 8
#define BIN2 9
#define STBY 7  //2、4、8、12、7为数字引脚，用于开关量控制
#define Voltage A0 //使用模拟引脚

int PwmA, PwmB;
double V;

void setup() {
  //TB6612电机驱动模块控制信号初始化
  pinMode(AIN1, OUTPUT);//控制电机A的方向，(AIN1, AIN2)=(1, 0)为正转，(AIN1, AIN2)=(0, 1)为反转
  pinMode(AIN2, OUTPUT);
  pinMode(BIN1, OUTPUT);//控制电机B的方向，(BIN1, BIN2)=(0, 1)为正转，(BIN1, BIN2)=(1, 0)为反转
  pinMode(BIN2, OUTPUT);
  pinMode(PWMA, OUTPUT);//A电机PWM
  pinMode(PWMB, OUTPUT);//B电机PWM
  pinMode(STBY, OUTPUT);//TB6612FNG使能, 置0则所有电机停止, 置1才允许控制电机
  
  //初始化TB6612电机驱动模块
  digitalWrite(AIN1, 1);
  digitalWrite(AIN2, 0);
  digitalWrite(BIN1, 1);
  digitalWrite(BIN2, 0);
  digitalWrite(STBY, 1);
  analogWrite(PWMA, 0);
  analogWrite(PWMB, 0);

 //初始化串口，用于输出电池电压
  Serial.begin(9600);
  pinMode(Voltage,INPUT); //初始化作为输入端
}

/**************************************************************************
函数功能：设置指定电机转速
入口参数：指定电机motor，motor=1（2）代表电机A（B）； 指定转速pwm，大小范围为0~255，代表停转和满速
返回  值：无
**************************************************************************/
void SetPWM(int motor, int pwm)
{
  if(motor==1&&pwm>=0)//motor=1代表控制电机A，pwm>=0则(AIN1, AIN2)=(1, 0)为正转
  {
    digitalWrite(AIN1, 1);
    digitalWrite(AIN2, 0);
    analogWrite(PWMA, pwm);
  }
  else if(motor==1&&pwm<0)//motor=1代表控制电机A，pwm<0则(AIN1, AIN2)=(0, 1)为反转
  {
    digitalWrite(AIN1, 0);
    digitalWrite(AIN2, 1);
    analogWrite(PWMA, -pwm);
  }
  else if(motor==2&&pwm>=0)//motor=2代表控制电机B，pwm>=0则(BIN1, BIN2)=(0, 1)为正转
  {
    digitalWrite(BIN1, 0);
    digitalWrite(BIN2, 1);
    analogWrite(PWMB, pwm);
  }
  else if(motor==2&&pwm<0)//motor=2代表控制电机B，pwm<0则(BIN1, BIN2)=(1, 0)为反转
  {
    digitalWrite(BIN1, 1);
    digitalWrite(BIN2, 0);
    analogWrite(PWMB, -pwm);
  }
}

void loop() 
{
  SetPWM(1, 255);//电机AB同时满速正转
  SetPWM(2, 255);
  V=analogRead(Voltage); //读取模拟引脚A0模拟量
  Serial.print(V*0.05371);  //对模拟量转换并通过串口输出
  Serial.println("V");
  delay(500);//正转3s
  
 SetPWM(1, 0);//电机AB停止
 SetPWM(2, 0);
 delay(1000);//停止1s
 
 SetPWM(1, 128);//电机AB同时半速正转
 SetPWM(2, 128);
 delay(3000);//半速正转3s
 
 SetPWM(1, 0);//电机AB停止
 SetPWM(2, 0);
 delay(1000);//停止1s
 
 SetPWM(1, -255);//电机AB同时满速反转
 SetPWM(2, -255);
 delay(3000);//反转3s
 
 SetPWM(1, 0);//电机AB停止
 SetPWM(2, 0);
 delay(1000);//停止1s
 
 SetPWM(1, 255);//电机A满速正转
 SetPWM(2, -255);//电机B满速反转
 delay(3000);//持续3s
 
 SetPWM(1, 0);//电机AB停止
 SetPWM(2, 0);
 delay(1000);//停止1s
}
```


### 电机JGB37-545B (霍尔编码)

* 驱动板输出: AO1 GND E1A E1B 5V AO2

* 电机输出：
	* 红（电机电源+） AO1
	* 黑（电机电源-） AO2
	* 绿（传感器地线）GND
	* 蓝（传感器电源） 5V
	* 黄（信号A输出） E1A
	* 白（信号B输出）E1B



> [!WARNING] 注意传感器千万不能接反。

与JGB37-520电机线序是不一样的，需要手动一一对应。

## TB6612驱动板 (芯片)

### F1CT 拆机 (光耦编码)

* 电机参数：输入6-12V

* 电机输出：
	* 灰 电机- AO2
	* 蓝 电机+ AO1
	* 红 编码器+
	* 黑 编码器 GND
	* 棕 A相
	* 白 B相

1.驱动板PWMA，AIN2，AIN1，STBY连接开发板11，4，5，7
2.编码器线束 红（编码器+）黑（编码器-）连接开发板5V与GND 
3.编码器线束 棕（A相）白（B相）连接开发板2，3.
4.编码器线束 灰（电机-）蓝（电机+）连接驱动板AO2，AO1
5.驱动板 VM与GND连接电源


```c
//定义引脚名称
#define PWMA 11  //11 用于PWM控制
#define AIN1 5
#define AIN2 4
#define PWMB 10  //10 用于PWM控制
#define BIN1 8
#define BIN2 9
#define STBY 7  //2、4、8、12、7为数字引脚，用于开关量控制
#define Voltage A0 //使用模拟引脚

int PwmA, PwmB;
double V;

int motor_A = 2;//中端口是0
int motor_B = 3;//中断口是1
volatile int count = 0;//如果是正转，那么每计数一次自增1，如果是反转，那么每计数一次自减1 

int reducation = 90;//减速比，根据电机参数设置，比如 15 | 30 | 60
int pulse = 11; //编码器旋转一圈产生的脉冲数该值需要参考商家电机参数
int per_round = pulse * reducation * 4;//车轮旋转一圈产生的脉冲数 
long start_time = millis();//一个计算周期的开始时刻，初始值为 millis();
long interval_time = 50;//一个计算周期 50ms
double current_vel;


void count_A(){
  //2倍频计数实现
  //手动旋转电机一圈，输出结果为 一圈脉冲数 * 减速比 * 2
  if(digitalRead(motor_A) == HIGH){
    if(digitalRead(motor_B) == HIGH){//A 高 B 高
      count++;  
    } else {//A 高 B 低
      count--;  
    }
  } else {
    if(digitalRead(motor_B) == LOW){//A 低 B 低
      count++;  
    } else {//A 低 B 高
      count--;  
    }  
  }
}

//与A实现类似
//4倍频计数实现
//手动旋转电机一圈，输出结果为 一圈脉冲数 * 减速比 * 4
void count_B(){
  if(digitalRead(motor_B) == HIGH){
    if(digitalRead(motor_A) == LOW){//B 高 A 低
      count++;
    } else {//B 高 A 高
      count--;
    }

  } else {
    if(digitalRead(motor_A) == HIGH){//B 低 A 高
      count++;
    } else {//B 低 A 低
      count--;
    }
  }
}

//获取当前转速的函数
void get_current_vel(){
  long right_now = millis();  
  long past_time = right_now - start_time;//计算逝去的时间
  if(past_time >= interval_time){//如果逝去时间大于等于一个计算周期
    //1.禁止中断
    noInterrupts();
    //2.计算转速 转速单位可以是秒，也可以是分钟… 自定义即可
    current_vel = (double)count / per_round / past_time * 1000 * 60;
    //3.重置计数器
    count = 0;
    //4.重置开始时间
    start_time = right_now;
    //5.重启中断
    interrupts();

    Serial.println(current_vel);

  }
}

/**************************************************************************
函数功能：设置指定电机转速
入口参数：指定电机motor，motor=1（2）代表电机A（B）； 指定转速pwm，大小范围为0~255，代表停转和满速
返回  值：无
**************************************************************************/
void SetPWM(int motor, int pwm)
{
  if(motor==1 && pwm>=0)//motor=1代表控制电机A，pwm>=0则(AIN1, AIN2)=(1, 0)为正转
  {
    digitalWrite(AIN1, 1);
    digitalWrite(AIN2, 0);
    analogWrite(PWMA, pwm);
  }
  else if(motor==1 && pwm<0)//motor=1代表控制电机A，pwm<0则(AIN1, AIN2)=(0, 1)为反转
  {
    digitalWrite(AIN1, 0);
    digitalWrite(AIN2, 1);
    analogWrite(PWMA, -pwm);
  }
  else if(motor==2 && pwm>=0)//motor=2代表控制电机B，pwm>=0则(BIN1, BIN2)=(0, 1)为正转
  {
    digitalWrite(BIN1, 0);
    digitalWrite(BIN2, 1);
    analogWrite(PWMB, pwm);
  }
  else if(motor==2 && pwm<0)//motor=2代表控制电机B，pwm<0则(BIN1, BIN2)=(1, 0)为反转
  {
    digitalWrite(BIN1, 1);
    digitalWrite(BIN2, 0);
    analogWrite(PWMB, -pwm);
  }
}

void setup() {
  //TB6612电机驱动模块控制信号初始化
  pinMode(AIN1, OUTPUT);//控制电机A的方向，(AIN1, AIN2)=(1, 0)为正转，(AIN1, AIN2)=(0, 1)为反转
  pinMode(AIN2, OUTPUT);
  pinMode(BIN1, OUTPUT);//控制电机B的方向，(BIN1, BIN2)=(0, 1)为正转，(BIN1, BIN2)=(1, 0)为反转
  pinMode(BIN2, OUTPUT);
  pinMode(PWMA, OUTPUT);//A电机PWM
  pinMode(PWMB, OUTPUT);//B电机PWM
  pinMode(STBY, OUTPUT);//TB6612FNG使能, 置0则所有电机停止, 置1才允许控制电机
  
  //初始化TB6612电机驱动模块
  digitalWrite(AIN1, 1);
  digitalWrite(AIN2, 0);
  digitalWrite(BIN1, 1);
  digitalWrite(BIN2, 0);
  digitalWrite(STBY, 1);
  analogWrite(PWMA, 0);
  analogWrite(PWMB, 0);

 //初始化串口，用于输出电池电压
  Serial.begin(9600);
  pinMode(Voltage,INPUT); //初始化作为输入端

  pinMode(motor_A,INPUT);
  pinMode(motor_B,INPUT);
  attachInterrupt(0,count_A,CHANGE);//当电平发生改变时触发中断函数
  //四倍频统计需要为B相也添加中断
  attachInterrupt(1,count_B,CHANGE);
}

void loop() 
{
  //测试计数器输出
  delay(300);
  // Serial.println(count);
  get_current_vel();

  // V=analogRead(Voltage); //读取模拟引脚A0模拟量
  // Serial.print(V*0.05371);  //对模拟量转换并通过串口输出
  // Serial.println("V");

  // SetPWM(1, 128);//电机AB同时满速正转
  // delay(3000);//正转3s
  // SetPWM(1, 0);//电机AB停止
  // delay(1000);//停止1s
  
  // SetPWM(1, -64);
  // delay(3000);//反转3s
  // SetPWM(1, 0);//电机AB停止
  // delay(1000);//停止1s

  // SetPWM(1, 32);
  // delay(3000);//正转3s
  // SetPWM(1, 0);//电机AB停止
  // delay(1000);//停止1s
}
```


# PID控制理论

* 举个例子

比如:现有一辆行驶中的无人车，要求将车速调整至100KM/h，那么应该如何向电机输出PWM值？或换言之，如何控制油门？


http://www.autolabor.com.cn/book/ROSTutorials/di-8-zhang-gou-jian-lun-shi-cha-fen-ji-qi-ren/83-di-pan-kong-zhi-ji-chu-arduino-yu-dian-ji-qu-dong/835-dian-ji-diao-su.html

<img src="https://github.com/aaronmack/picx-images-hosting/raw/master/e/image.2yy8eqv8d8.webp" alt="image" />

* 代码示例

```c
#include <PID_v1.h> 

int motor_A = 21;//中端口是2
int motor_B = 20;//中断口是3
int motor_C = 18;//中断口是5
int motor_D = 19;//中断口是4
volatile int count = 0;//如果是正转，那么每计数一次自增1，如果是反转，那么每计数一次自减1 

#define PWMA 3  //3为模拟引脚，用于PWM控制
#define AIN1 5
#define AIN2 4
#define PWMB 10  //10为模拟引脚，用于PWM控制
#define BIN1 8
#define BIN2 9
#define STBY 7  //2、4、8、12、7为数字引脚，用于开关量控制

//Vel
int reducation = 90;//减速比，根据电机参数设置，比如 15 | 30 | 60
int pulse = 11; //编码器旋转一圈产生的脉冲数该值需要参考商家电机参数
int per_round = pulse * reducation * 4;//车轮旋转一圈产生的脉冲数 
long start_time = millis();//一个计算周期的开始时刻，初始值为 millis();
long interval_time = 50;//一个计算周期 50ms
double current_vel;

//PID
//1.当前转速 2.计算输出的pwm 3.目标转速 4.kp 5.ki 6.kd 7.当输入与目标值出现偏差时，向哪个方向控制
double pwm;//电机驱动的PWM值
double target = 80;
double kp=1.5, ki=3.0, kd=0.1;
PID pid(&current_vel, &pwm, &target, kp, ki, kd, DIRECT);

void SetPWM_v2(int motor, int pwm, int dir)
{
  // motor=1代表控制电机A，(AIN1, AIN2)=(1, 0)为正转
  // motor=2代表控制电机B，(AIN1, AIN2)=(0, 1)为反转
  if(motor==1 && dir==0)
  {
    digitalWrite(AIN1, 1);
    digitalWrite(AIN2, 0);
    analogWrite(PWMA, pwm);
  }
  else if(motor==1 && dir==1)
  {
    digitalWrite(AIN1, 0);
    digitalWrite(AIN2, 1);
    analogWrite(PWMA, pwm);
  }
  else if(motor==2 && dir==0)
  {
    digitalWrite(BIN1, 0);
    digitalWrite(BIN2, 1);
    analogWrite(PWMB, pwm);
  }
  else if(motor==2 && dir==1)
  {
    digitalWrite(BIN1, 1);
    digitalWrite(BIN2, 0);
    analogWrite(PWMB, pwm);
  }
}

void count_A(){
  //2倍频计数实现
  //手动旋转电机一圈，输出结果为 一圈脉冲数 * 减速比 * 2
  if(digitalRead(motor_A) == HIGH){
    if(digitalRead(motor_B) == HIGH){//A 高 B 高
      count++;  
    } else {//A 高 B 低
      count--;  
    }
  } else {
    if(digitalRead(motor_B) == LOW){//A 低 B 低
      count++;  
    } else {//A 低 B 高
      count--;  
    }  
  }
}

//与A实现类似
//4倍频计数实现
//手动旋转电机一圈，输出结果为 一圈脉冲数 * 减速比 * 4
void count_B(){
  if(digitalRead(motor_B) == HIGH){
    if(digitalRead(motor_A) == LOW){//B 高 A 低
      count++;
    } else {//B 高 A 高
      count--;
    }
  } else {
    if(digitalRead(motor_A) == HIGH){//B 低 A 高
      count++;
    } else {//B 低 A 低
      count--;
    }
  }
}

//获取当前转速的函数
void get_current_vel(){
  long right_now = millis();  
  long past_time = right_now - start_time;//计算逝去的时间
  if(past_time >= interval_time){//如果逝去时间大于等于一个计算周期
    //1.禁止中断
    noInterrupts();
    //2.计算转速 转速单位可以是秒，也可以是分钟… 自定义即可
    current_vel = (double)count / per_round / past_time * 1000 * 60;
    //3.重置计数器
    count = 0;
    //4.重置开始时间
    start_time = right_now;
    //5.重启中断
    interrupts();
    Serial.println(current_vel);
  }
}

//速度更新函数
void update_vel(){
  //获取当前速度
  get_current_vel();
  pid.Compute();//计算需要输出的PWM
  
  SetPWM_v2(2, pwm, DIRECT);
}

void setup() {
  Serial.begin(57600);//设置波特率  

  //TB6612电机驱动模块控制信号初始化
  pinMode(AIN1, OUTPUT);//控制电机A的方向，(AIN1, AIN2)=(1, 0)为正转，(AIN1, AIN2)=(0, 1)为反转
  pinMode(AIN2, OUTPUT);
  pinMode(BIN1, OUTPUT);//控制电机B的方向，(BIN1, BIN2)=(0, 1)为正转，(BIN1, BIN2)=(1, 0)为反转
  pinMode(BIN2, OUTPUT);
  pinMode(PWMA, OUTPUT);//A电机PWM
  pinMode(PWMB, OUTPUT);//B电机PWM
  pinMode(STBY, OUTPUT);//TB6612FNG使能, 置0则所有电机停止, 置1才允许控制电机
  
  //初始化TB6612电机驱动模块
  digitalWrite(AIN1, 1);
  digitalWrite(AIN2, 0);
  digitalWrite(BIN1, 1);
  digitalWrite(BIN2, 0);
  digitalWrite(STBY, 1);
  analogWrite(PWMA, 0);
  analogWrite(PWMB, 0);

  pinMode(motor_C, INPUT);
  pinMode(motor_D, INPUT);
  pinMode(motor_A, INPUT);
  pinMode(motor_B, INPUT);

  attachInterrupt(2, count_A, CHANGE);//当电平发生改变时触发中断函数
  //四倍频统计需要为B相也添加中断
  attachInterrupt(3, count_B, CHANGE);

  pid.SetMode(AUTOMATIC);
}

void loop() {
  delay(10);
  update_vel();
}
```

# LD14P雷达

1. 连接转接板: 转接板上电蓝灯常亮为CH9102，转接板上电绿灯常亮为CP2102


# USB-Cam

1. 安装： sudo apt-get install ros-humble-usb-cam，地址：[GitHub - ros-drivers/usb\_cam: A ROS Driver for V4L2 USB Cameras](https://github.com/ros-drivers/usb_cam)

>[!INFO] 如果在Windows系统中插入USB-Camera，再使用usbipd attach到WSL中，名称不再是`/dev/video0`，可以使用`lsusb`去检查，例如最后可能在 `/dev/bus/usb/001/009` 不知道是不是在这。


# 问题解决

## 树莓派连接Arduino开发板，但不显示设备

1. 使用`sudo dmesg`显示

```bash
ch341 1-1.1:1.0: ch341-uart converter detected
usb 1-1.1: ch341-uart converter now attached to ttyUSB0
usb 1-1.1: usbfs: interface 0 claimed by ch341 while 'brltty' sets config #1
ch341-uart ttyUSB0: ch341-uart converter now disconnected from ttyUSB0
ch341 1-1.1:1.0: device disconnected
```

2. 可以看到设备本来被`attached to ttyUSB0`，但是后面断开了。

```bash
# 这一段不知有没有用，但前面测试时执行过了。
for f in /usr/lib/udev/rules.d/*brltty*.rules; do
    sudo ln -s /dev/null "/etc/udev/rules.d/$(basename "$f")"
done
sudo udevadm control --reload-rules
# 执行了这一句命令。
sudo systemctl mask brltty.path

```

## 树莓派与Arduino开发板串口通信，报错权限不足

```bash
# 使用
ls -l /dev/ttyUSB0
# 或者，查看端口信息
stat /dev/ttyUSB0

# 可以看到设备显示在这个组，添加进去
sudo usermod -a -G dialout $USER

# 登出，再重新登录即可
logout
```

## ros的usb_cam运行报错，权限不足

```bash
# 下方命令只能临时，重启失效
chmod 777 /dev/video0

# 使用下方命令，需要logout再登入
sudo usermod -aG video <your_username>
```

# ESP32 与 ROS2 交互

将已经刷入Micropython的ESP32恢复到可以刷入Arduino的状态 `esptool --port COM15 erase_flash`，

根据 https://github.com/micro-ROS/micro_ros_arduino 用Arduino刷入ESP32模块，Example使用 `micro-ros_publisher-wifi`

上位机需要启动 `ros2 run micro_ros_agent micro_ros_agent udp4 --port 8888` 需要安装micro_ros_agent

# Micro ROS

## micro ros 搭配摇杆控制

```c
#include <micro_ros_arduino.h>

#include <stdio.h>
#include <rcl/rcl.h>
#include <rcl/error_handling.h>
#include <rclc/rclc.h>
#include <rclc/executor.h>

#include <geometry_msgs/msg/twist.h>

#if !defined(ESP32) && !defined(TARGET_PORTENTA_H7_M7) && !defined(ARDUINO_NANO_RP2040_CONNECT) && !defined(ARDUINO_WIO_TERMINAL)
#error This example is only avaible for Arduino Portenta, Arduino Nano RP2040 Connect, ESP32 Dev module and Wio Terminal
#endif

rcl_publisher_t publisher;
geometry_msgs__msg__Twist msg;
rclc_support_t support;
rcl_allocator_t allocator;
rcl_node_t node;

int VRx = 35;
int VRy = 34;
int SW = 32;
int xValue = 0; 
int yValue = 0;
int xVoltsValue = 0; 
int yVoltsValue = 0;
int swState = 1;

#define LED_PIN 13

#define RCCHECK(fn) { rcl_ret_t temp_rc = fn; if((temp_rc != RCL_RET_OK)){error_loop();}}
#define RCSOFTCHECK(fn) { rcl_ret_t temp_rc = fn; if((temp_rc != RCL_RET_OK)){}}

void error_loop(){
  while(1){
    digitalWrite(LED_PIN, !digitalRead(LED_PIN));
    delay(100);
  }
}

void timer_callback(rcl_timer_t * timer, int64_t last_call_time)
{
  RCLC_UNUSED(last_call_time);
  if (timer != NULL) {
    RCSOFTCHECK(rcl_publish(&publisher, &msg, NULL));
  }
}

void setup() {
  set_microros_wifi_transports("MERCURY_051E", "23456789", "192.168.31.200", 8888);
  Serial.begin(115200);

  analogReadResolution(12);
  //analogReadResolution(12);  // Defalut
  //analogSetAttenuation(ADC_11db);

  pinMode(LED_PIN, OUTPUT);
  digitalWrite(LED_PIN, HIGH);
  pinMode(VRx, INPUT);
  pinMode(VRy, INPUT);
  pinMode(SW, INPUT_PULLUP);

  delay(2000);

  allocator = rcl_get_default_allocator();

  //create init_options
  RCCHECK(rclc_support_init(&support, 0, NULL, &allocator));

  // create node
  RCCHECK(rclc_node_init_default(&node, "micro_ros_arduino_wifi_node", "my_qos_label", &support));

  // create publisher
  RCCHECK(rclc_publisher_init_default(
    &publisher,
    &node,
    ROSIDL_GET_MSG_TYPE_SUPPORT(geometry_msgs, msg, Twist),
    "/cmd_vel"));
  
  msg.linear.x = 0.0;
  msg.angular.z = 0.0;
}

float transform(float x, float v) {
    float result;
    if (x >= 0) {
        result = pow(x, v);
    } else {
        result = pow(-x, v);
        result = -result;
    }
    return result;
}

void processJoyValue()
{
    xValue = analogRead(VRx);
    yValue = analogRead(VRy);
    // xVoltsValue = analogReadMilliVolts(VRx);
    // yVoltsValue = analogReadMilliVolts(VRy);
    swState = digitalRead(SW);

    // FIXME the offset.
    int offsetX = 109;
    int offsetY = 65;
    
    // Remap
    int linearX = map(xValue + offsetX, 0, 4095, 0, 200);
    int angleY = map(yValue + offsetY, 0, 4095, 0, 200);
    linearX = max(0, linearX); linearX = min(200, linearX);
    angleY = max(0, angleY); angleY = min(200, angleY);

    // Reduce noise
    if(linearX < 103 && linearX > 97)
      linearX =  100;
    if(angleY < 103 && angleY > 97)
      angleY =  100;
    
    // Normalize
    float linear_x = (float(linearX) - 100.0) / 100.0;
    float angle_y = (float(angleY) - 100.0) / 100.0;
    linear_x = -transform(linear_x, 2.3);
    angle_y = -transform(angle_y, 2.3) * 3.0;

    // Serial.print("X-axis: ");
    // Serial.print(xValue);
    // Serial.print(" | Y-axis: ");
    // Serial.print(yValue);
    // Serial.print(" | X-volts: ");
    // Serial.print(linearX);
    // Serial.print(" | Y-volts: ");
    // Serial.print(angleY);
    // Serial.print(" | Switch: ");
    // Serial.println(swState);

    msg.linear.x = linear_x;
    msg.angular.z = angle_y;
    if(swState == 0)
    {
      msg.linear.x = 0.0;
      msg.angular.z = 0.0;
    }
}

void loop() {
    processJoyValue();
    RCSOFTCHECK(rcl_publish(&publisher, &msg, NULL));
    delay(20);
}
```