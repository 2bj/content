---
tags:
  - practice
permalink: false
---

## Мониторинг сети

### Обзор сетевых устройств

Если компьютер имеет подключённый к сети сетевой интерфейс или несколько интерфейсов, то в первую очередь необходимо получить их список. Это можно сделать через графический интерфейс или с помощью терминала:

- `ifconfig` — для Unix-подобных операционных систем;
- `ipconfig /all` — для операционной системы Windows.

В списке сетевых интерфейсов можно будет посмотреть и IP-адреса, к которым они будут привязаны.

С помощью консольной утилиты `ping`, как правило, можно получить информацию о том, присутствует ли сетевое устройство в сети или нет. Например, проверить доступность известного прокси-сервера от Google можно так:

```bash
ping 8.8.8.8
```

### Мониторинг соединений

Существует масса утилит для мониторинга соединений, которые собирают и отображают информацию об открытых соединениях, портах и приложениях, которые используют эти соединения. Среди прочих утилита `netstat` присутствует практически в любой операционной системе. Например, эта утилита есть в Windows, на MacOS или в Linux. В Linux эта утилита не всегда предустановленна, но поставляет с составе пакета [net-tools](https://wiki.linuxfoundation.org/networking/net-tools).

Чтобы просмотреть все соединения, которые работаю в настоящее время, необходимо в Терминале выполнить команду:

```bash
netstat
```

Для фильтрации списка соединений необходимо использовать ключи. Например, для анализа активных сетевых соединений, использующих определённые протоколы, надо использовать ключ `-p`. Например, для анализа соединений, работающих по протоколу TCP:

```bash
netstat -p tcp
```

Если воспользоваться ключом `-n`, то можно вывести все IP-адреса с указанием портов для каждого приложения на компьютере, которое использует сетевой интерфейс:

```bash
netstat -n
```

Для анализа безопасности компьютера важно знать, какие существуют входящие соединения. Это  возможно с помощью ключа `-L`:

```bash
netstat -L
```

Если воспользоваться ключом `-s`, можно вывести статистику по протоколам:

```bash
netstat -s
```

Ключ `-i` позволяет посмотреть доступные сетевые интерфейсы:

```bash
netstat -i
```

Перед использованием утилиты, изучите руководство с помощью команды:

```bash
man netstat
```

Использование команды `man` подробно описано в статье «[Интерфейс командной строки](/tools/articles/cli)».

Операционная система Linux постоянно обновляется, обновляются и утилиты. Например, вместо `netstat` лучше использовать `ss`, а вместо `ifconfig` — утилиту `ip`.

<details>
  <summary>Подробности...</summary>

  Старые команды         | Новые команды           | Применение
  :----------------------|:------------------------|:------------------------------------------------------
  `ifconfig -a`          |`ip a`                   | Вывод списка всех IP-адресов всех сетевых интерфейсов
  `ifconfig enp6s0 down` |`ip link set enp6s0 down`| Выключить сетевой интерфейс
  `ifconfig enp6s0 up`   |`ip link set enp6s0 up`  | Включить сетевой интерфейс
  `netstat`              |`ss`                     | Вывод всех активных соединений
  `netstat <keys>`       |`ss <keys>`              | Ключи у команд практически совпадают, подробнее: `man ss`
</details>

## Настройка сетевых служб

### Управление брандмауэром, фильтрация пакетов

Для обеспечения безопасности в сети используют брандмауэр. Эта программа, которая позволяет фильтровать входящие и исходящие пакеты, которые проходят через определённый порт с учётом IP-адреса. В Unix-подобных системах используются утилиты типа `iptables` или `nftables` для анализа трафика и перенаправления его. Поскольку Unix-подобные системы являются сетевыми, сама фильтрация пакетов осуществляется на уровне ядра системы, утилиты позволяют лишь поработать с настройкой фильтрации.

В настройках описываются правила фильтрации пакетов, которые применяются по цепочке. существуют правила для входящих, исходящих и транзитных пакетов. Рассмотрим простой пример: запрет приёма пакетов, которые приходят по протоколу [ICMP](https://ru.wikipedia.org/wiki/ICMP). Это не позволит воспользоваться утилитой `ping` для того, чтобы обнаружить компьютер в сети. Реализовать данное правило можно очень просто:

```bash
iptables -A INPUT -p icmp --icmp-type echo-request - j REJECT
```

Ключ `-A` означает, что необходимо добавить правило в конец цепочки правил `INPUT`, которые обрабатывают входящие пакеты. По ключу `-p` можно задать протокол, у нас это ICMP. `--icmp-type` — специальный ключ для этого протокола, который определяет тип пакетов для этого протокола. `-j` определяет действие, у нас это — отклонить. То есть входящие пакеты по протоколу ICMP типа `echo-request` необходимо отклонить.

Чтобы посмотреть все правила выполняем команду:

```bash
iptables -L
```