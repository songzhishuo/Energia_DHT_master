# Energia_DHT_master
Energia 支持的MSP430 的DHT11库



# 关于Energia

Energia 是一个TI适配的类Arduino的IDE，使用的Arduino的语法和关键字，支持部分Arduino的库。相关介绍可以参考下文：

[ENERGIA IDE、配置、编译器或调试器 | TI.com.cn](https://www.ti.com.cn/tool/cn/ENERGIA)

# DHT11 库

官方并没有对DHT11进行适配，Git上寻找了很久找到了一个支持MSP430全系列的DHT11的第三方库。连接如下：

[songzhishuo/Energia_DHT_master: Energia 支持的MSP430 的DHT11库 (github.com)](https://github.com/songzhishuo/Energia_DHT_master)

Energia 项目主页：

https://energia.nu

## 库安装

### STEP1

首先打开上述github连接，将其download下来。

![image-20220514162717567](C:\Users\songz\Pictures\博客截图\image-20220514162717567-16525281407401.png)

### STEP2

打开Energia，若未下载可以去官网下载（https://energia.nu/download/）。目前已知1.8.10及以上版本存在编译失败的问题，可以考虑使用老版本进行操作。

点击：`文件->首选项`，即可看到`项目文件夹位置`字段，这个就是Energia 的库安装路径。在电脑的文件资源管理器中打开此路径并且进入其中的libraries 目录。

![image-20220514162404610](C:\Users\songz\Pictures\博客截图\image-20220514162404610-16525281407413.png)



![image-20220514162432019](C:\Users\songz\Pictures\博客截图\image-20220514162432019-16525281407415.png)

![image-20220514162532704](C:\Users\songz\Pictures\博客截图\image-20220514162532704-16525281407417.png)

### STEP3

将下载的Energia_DHT_master 库解压到此位置即可，然后重启Energia软件。即可在示例中看到库已经加载成功，然后参考Demo进行使用即可。

![image-20220514163314232](C:\Users\songz\Pictures\博客截图\image-20220514163314232-16525281407419.png)

# 已知Bug和解决方法

目前已知在使用DHT11 & MSP430-EXP430G2ET w 板子运行Demo时出现程序获取一次数据之后卡死的现象，具体原因待查。解决方法如下：

```c++
// Example testing sketch for various DHT humidity/temperature sensors
// Written by ladyada, public domain

#include "DHT.h"

#define DHTPIN 6    // what pin we're connected to

// Uncomment whatever type you're using!
#define DHTTYPE DHT11   // DHT 11 
//#define DHTTYPE DHT22   // DHT 22  (AM2302)
//#define DHTTYPE DHT21   // DHT 21 (AM2301)

// Connect pin 1 (on the left) of the sensor to +5V
// NOTE: If using a board with 3.3V logic like an Arduino Due connect pin 1
// to 3.3V instead of 5V!
// Connect pin 2 of the sensor to whatever your DHTPIN is
// Connect pin 4 (on the right) of the sensor to GROUND
// Connect a 10K resistor from pin 2 (data) to pin 1 (power) of the sensor

// Initialize DHT sensor for normal 16mhz Arduino
DHT dht(DHTPIN, DHTTYPE);
// NOTE: For working with a faster chip, like an Arduino Due or Teensy, you
// might need to increase the threshold for cycle counts considered a 1 or 0.
// You can do this by passing a 3rd parameter for this threshold.  It's a bit
// of fiddling to find the right value, but in general the faster the CPU the
// higher the value.  The default for a 16mhz AVR is a value of 6.  For an
// Arduino Due that runs at 84mhz a value of 30 works.
// Example to initialize DHT sensor for Arduino Due:
//DHT dht(DHTPIN, DHTTYPE, 30);

void setup() {
  Serial.begin(9600); 
  Serial.println("DHTxx test!");
 
}

void loop() {
  // Wait a few seconds between measurements.
  delay(2000);
  dht.begin();			//每次使用前从新初始化一下dht对象
  // Reading temperature or humidity takes about 250 milliseconds!
  // Sensor readings may also be up to 2 seconds 'old' (its a very slow sensor)
  float h = dht.readHumidity();
  // Read temperature as Celsius
  float t = dht.readTemperature();
  // Read temperature as Fahrenheit
  float f = dht.readTemperature(true);
  
  // Check if any reads failed and exit early (to try again).
  if (isnan(h) || isnan(t) || isnan(f)) {
    Serial.println("Failed to read from DHT sensor!");
    return;
  }

  // Compute heat index
  // Must send in temp in Fahrenheit!
  float hi = dht.computeHeatIndex(f, h);

  Serial.print("Humidity: "); 
  Serial.print(h);
  Serial.print(" %\t");
  Serial.print("Temperature: "); 
  Serial.print(t);
  Serial.print(" *C ");
  Serial.print(f);
  Serial.print(" *F\t");
  Serial.print("Heat index: ");
  Serial.print(hi);
  Serial.println(" *F");
}
```

