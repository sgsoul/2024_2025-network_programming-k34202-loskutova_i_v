## Лабораторная работа №1 "Установка CHR и Ansible, настройка VPN"

### Описание
Данная работа предусматривает обучение развертыванию виртуальных машин (VM) и системы контроля конфигураций Ansible а также организации собственных VPN серверов.
### Цель работы
Целью данной работы является развертывание виртуальной машины на базе платформы Microsoft Azure с установленной системой контроля конфигураций Ansible и установка CHR в VirtualBox
### Ход работы

<details>
<summary> Создаём ВМ в Yandex Cloud на образе Ubuntu 22.04</summary>

![alt text](image-2.png)
![alt text](image-4.png)

Проверим, что все хорошо и нам не нужно обновляться

![alt text](image-5.png)


Теперь необходимо убедиться, что python3 и Ansible установлены

![alt text](image-6.png)

</details>

---

<details>
<summary>Устанавлаиваем CHR (RouterOS) на VirtualBox </summary>

![mikrotik](image.png)

Узнаем айпи адрес нашешо CHR:

![alt text](image-1.png)

И подключаемся к нему через терминал:

![alt text](<Снимок экрана 2024-09-23 222527.png>)

</details>

---

После этого  необходимо создать свой Wireguard сервер для организации VPN туннеля между  сервером автоматизации где был установлена система контроля конфигураций Ansible и  локальным CHR.

```bash
sudo apt update
sudo apt install wireguard iptables
```

Генерируем ключи.

```bash
umask 077
sgsoul@net-prog:~$ wg genkey | tee private.key | wg pubkey > public.key
sudo mv private.key /etc/wireguard/private.key
sudo mv public.key /etc/wireguard/public.key
```
Выводим их значения для дальнейшей работы: 
![alt text](image-12.png)

![alt text](image-11.png)

![alt text](image-7.png)


После всех манипуляций вам необходимо будет поднять VPN туннель между вашим VPN сервером на Ubuntu 18 и VPN клиентом на RouterOS (CHR)

> Что искать? («Название вашего протокола» клиент RouterOS).


### Результаты лабораторной работы

![alt text](<Снимок экрана 2024-09-24 015017.png>) 
![alt text](<Снимок экрана 2024-09-24 014947.png>)

![alt text](image-10.png)

В результате лабораторной работы у вас должна получиться схема связи следующего вида:



Также у вас будет:

- Виртуальная машина (**далее - сервер автоматизации**) с установленной системой контроля конфигураций Ansible в облачном сервисе Microsoft Azure и ваш компьютер с установленной на нем RouterOS в VirtualBox.
- VPN туннель между сервером автоматизации и вашим локальным CHR
- IP связность (это значит что вы можете попинговать свой CHR с сервера автоматизации)