<img src="https://user-images.githubusercontent.com/57194638/201707251-5aa29404-2494-4e16-be4a-0cd821a1c0d9.png" width="800" height="300">

# Основные материалы для работы с роботом

```
Введение в ROS - http://docs.voltbro.ru/starting-ros/
Образовательный портал с уроками ROS -  http://learn.voltbro.ru
Базовый мануал по старту работы с роботом - https://manual.turtlebro.ru/
Основной репозиторий - https://github.com/voltbro    
Параметры и настройка через launch -  https://manual.turtlebro.ru/paket-turtlebro/params
Подключение робота к Сети - https://manual.turtlebro.ru/pervoe-vklyuchenie-i-nastroika-robota/networking
```
## Основные команды для проверки роботоспособности робота:

Имя робота в сети можно узнать введя в терминал:	
```
echo $ROS_HOSTNAME
```
Для вывода IP-адреса робота в сети:	
```
hostname -I (ip a)
```
С помощью веб-интерфейса, доступного по ссылке http://<IP-адрес робота>:8080 можно проверить работоспособность камеры, корректность одометрии при линейном движении робота и угловом вращении робота. Веб-интерфейс позволяет управлять роботом с помощью всем известным WASD. В случае неработоспособности камеры стоит сделать reset на ней. 

Название дистрибутива Linux и кодовое имя сборки выводится с помощью:	
```
lsb_release -a
```
Не забудь проверить размер оперативной памяти, он выводится в мегабайтах:
```
free -h
```
Часовой пояс на роботе в данный момент также узнать несложно. Он выводится в формате “Time zone:Continent/City (XXX, +XXXX)”:
```
timedatectl | grep zone
```
Для управления роботом может потребоваться написание кода на питоне. Для этого следует проверить следующее:
Версия интерпретатора Python3:	
```
python3 -V (python3 --version)
```
Версия библиотеки rospy:	
```
pip3 show rospy
```
Также интересная для проверки информация:
Версия пакета turtlebro:
```
rosversion turtlebro
```
Версия прошивки микроконтроллера материнской платы и серийный номер системной платы робота (mcu_id):	
```
rosservice call /board_info "request: {}"
```
Версия образа ОС, установленной на Raspberry Pi:
```
cat /boot/version
```

Наличие необходимых топиков на роботе проверяется просто:	
```
rostopic list
```
Так же, как и наличие сервисов:
```
rosservise list
```

Температура процессора в градусах (С):	
```
vcgencmd measure_temp
```

Максимальное разрешение камеры (пикселей):	
```
v4l2-ctl --list-formats-ext
```

Работоспособность датчиков проверить можно таким образом - "rostopic echo /<датчик> -n 1".
Например, для IMU-датчика:	
```
rostopic echo /imu -n 1
```
Или для лидара:	
```
rostopic echo /scan -n 1
```
Для топика батареи, помимо уже известного:	
```
rostopic echo /bat -n 1
```
Есть еще один способ:
```
grep voltage
```

С помощью библиотеки FastLED, которую необходимо предварительно загрузить,  можно проверить работоспособность светодиодов на роботе. Сделать это можно с помощью представленного скетча:
```
#include <FastLED.h> // подключаем библиотеку

#define NUM_LEDS 24 // указываем количество светодиодов на ленте
#define PIN 30                    // указываем пин для подключения ленты

CRGB leds[NUM_LEDS];

void setup() {
   // основные настройки для адресной ленты
   FastLED.addLeds <WS2812, PIN, GRB>(leds, NUM_LEDS).setCorrection(TypicalLEDStrip);
   FastLED.setBrightness(50);
}
//Необходимо указать количество светодиодов и пин к которому подключены светодиоды, он указан на самой плате.
```
Работоспособность кнопок D22-D25 можно проверисть с помощью примера библиотеки digital в программе Arduino IDE. 

В случае положительных результатов при каждой проверке, робот полностью доступен и готов к эксплуатации!
