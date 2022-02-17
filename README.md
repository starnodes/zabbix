# zabbix
## quick start

Подготовте ваш узел на линукс для мониторинга температуры cpu

apt update && apt install lm-sensors

or

yum install lm_sensors

## Варианты мониторинга

Вывод минимального значения температуры

sensors | grep Core | awk -F'[:+°]' '{if(min==""){min=$3}; if($3<min) {min=$3};} END {print min}'

Вывод максимального значения температуры

sensors | grep Core | awk -F'[:+°]' '{if(max==""){max=$3}; if(max<$3) {max=$3};} END {print max}'

Вывод среднего значения температуры

sensors | grep Core | awk -F'[:+°]' '{avg+=$3}END{print avg/NR}'

## Настроим заббикс агент
```sh
sudo tee -a /etc/zabbix/zabbix_agentd.conf > /dev/null <<EOF
UnsafeUserParameters=1
UserParameter=temp.core1,sensors coretemp-isa-0000 | awk -F'[:+°]' '{if(max==""){max=$3}; if(max<$3) {max=$3};} END {print max}'
UserParameter=temp.core0,sensors coretemp-isa-0001 | awk -F'[:+°]' '{if(max==""){max=$3}; if(max<$3) {max=$3};} END {print max}'
EOF
```
systemctl restart zabbix-agent

## Теперь создайте новый элемент данных с плавающей точкой у нужного узла

![Создание элемента данных на заббикс сервере](https://github.com/toxi42/zabbix/raw/main/screenshots/data.png)

## Добавим триггер для оповещения

![Создание триггера на заббикс сервере](https://github.com/toxi42/zabbix/raw/main/screenshots/trigger.png)

## Осталось добавить график

![Создание графика на заббикс сервере](https://github.com/toxi42/zabbix/raw/main/screenshots/chart.png)

## Готово!
