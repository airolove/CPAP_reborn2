# CPAP_reborn2

Инструкция по установке и настройки CPAP

Сам CPAP (https://aliexpress.ru/item/1005003822117604.html).

Для начала нужно делать сам хост, в нашем случае это может быть Orange PI 3 LTS, Raspberry PI или любой другой, обозначив его как klipper_host_mcu в printer.cfg.
- Устанавливаем RC-скрипт для нашего клиппера, для этого нужно подключиться с помощью PuTTY к нашему одноплатнику, после авторизации пользователем вводим:

cd ~/klipper/

sudo cp ./scripts/klipper-mcu.service /etc/systemd/system/

sudo systemctl enable klipper-mcu.service

- Далее нужно сделать прошивку нашему микроконтроллеру, для этого вводим:

cd ~/klipper/

make menuconfig

- открывается окно и выбираем Linux Process

![](https://github.com/airolove/CPAP_reborn2/blob/main/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202023-04-29%20231704.png)

![](https://github.com/airolove/CPAP_reborn2/blob/main/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202023-04-29%20231722.png)

- Жмем Q и нажимаем сохранить (Y).
- Далее:

make clean

make

sudo service klipper stop

make flash

sudo service klipper start

- В printer.cfg добавляем:

[mcu host]

serial: /tmp/klipper_host_mcu

[temperature_sensor host]

sensor_type: temperature_host

min_temp: 10

max_temp: 100

- Для подключения самого CPAP мы будем использовать пин PWM (PD22, gpio2) на одноплатнике PI 3 LTS и пин VSR на самом CPAP.

![](https://github.com/airolove/CPAP_reborn2/blob/main/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202023-04-29%20232913.png)

![](https://github.com/airolove/CPAP_reborn2/blob/main/photo1680981407.jpeg)

- В самом printer.cfg добавляем (Секцию [fan] для обычного обдува вентилятора придется закомментировать):

[fan]

pin: host:gpio2

cycle_time: 0.005

hardware_pwm: False

shutdown_speed: 0

- Сохраняем. Все должно работать.

