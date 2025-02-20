# Домашнее задание к занятию " `Уязвимости и атаки на информационные системы` " - `Сулименков Алексей`

---

## Задание 1

Скачайте и установите виртуальную машину Metasploitable: https://sourceforge.net/projects/metasploitable/.

Это типовая ОС для экспериментов в области информационной безопасности, с которой следует начать при анализе уязвимостей.

Просканируйте эту виртуальную машину, используя nmap.

Попробуйте найти уязвимости, которым подвержена эта виртуальная машина.

Сами уязвимости можно поискать на сайте https://www.exploit-db.com/.

Для этого нужно в поиске ввести название сетевой службы, обнаруженной на атакуемой машине, и выбрать подходящие по версии уязвимости.

Ответьте на следующие вопросы:

- Какие сетевые службы в ней разрешены?
- Какие уязвимости были вами обнаружены? (список со ссылками: достаточно трёх уязвимостей)

Приведите ответ в свободной форме.

### Ответ

Сканирование nmap metasploitable

<details> <summary>Результат Сканирование</summary>

![nmap](https://github.com/biparasite/DB-13-01HW/blob/main/task-1.1.png "nmap")

</details>

- Поиск уязвимостей, исходя из версий приложений.

<details> <summary>Уязвимости</summary>

- vsftpd 2.3.4 - Backdoor Command Execution
- vsftpd 2.3.4 - Backdoor Command Execution (Metasploit)
- MySQL 5.0.x - Single Row SubSelect Remote Denial of Service
- MySQL 5.0.x - IF Query Handling Remote Denial of Service
- Samba 3.0.21 < 3.0.24 - LSA trans names Heap Overflow (Metasploit)

</details>

---

## Задание 2

Задание 2

Проведите сканирование Metasploitable в режимах SYN, FIN, Xmas, UDP.

Запишите сеансы сканирования в Wireshark.

Ответьте на следующие вопросы:

Чем отличаются эти режимы сканирования с точки зрения сетевого трафика?
Как отвечает сервер?

### Ответ

- Cканирование Metasploitable в режимах SYN, FIN, Xmas, UDP

<details> <summary>Команды</summary>

1. SYN `nmap -sS`
2. FIN `nmap -sA`
3. Xmas `nmap -sX`
4. Xmas `nmap -sU`

</details>

- Запишите сеансы сканирования в Wireshark.

Для записи в pcap, можно использовать tcpdump, а потом открыть уже в wireshark.

Записать сеаныс сканирования можно так:

<details> <summary>Команды</summary>

1. запускаем tcpdump с фильтром host, в котором указан ip сканированной VM
2. запуск nmap
3. остановка tcpdump

`tcpdump -i eth0@if28 host 172.17.0.2 -w 172.17.0.2.pcap && nmap -sS 172.17.0.2 && killall tcpdump`

</details>

1. SYN сканирование - метод также известен как «полуоткрытое» сканирование. В этом режиме отправляющий компьютер посылает пакет с установленным флагом SYN (Synchronize), который является первым шагом при установлении TCP-соединения. Если порт открыт, то сервер отвечает пакетом с установленными флагами SYN/ACK. После этого клиент может отправить пакет с флагом RST, чтобы закрыть соединение до завершения полного трёхэтапного рукопожатия (TCP handshake).

2. FIN сканирование - при использовании этого метода отправляются пакеты с установленным флагом FIN, который обычно используется для закрытия TCP-соединения. Если порт закрыт, система должна ответить пакетом с установленным флагом RST. Однако, если порт открыт, никакого ответа не будет.

3. Xmas сканирование - в этом случае отправляются пакеты с установленными сразу несколькими флагами: URG, PSH и FIN. Название происходит от того, что флаги горят как рождественская ёлка («Christmas tree»). Как и в случае с FIN-сканированием, закрытый порт ответит пакетом с флагом RST, а открытый останется без ответа.

4. UDP сканирование - сканирование портов протокола UDP отличается от TCP тем, что нет механизма подтверждения связи. Когда отправляется запрос к UDP-порту, возможны два варианта развития событий. Если порт открыт, сервер может вернуть ответный пакет (например, ICMP-сообщение о доступности сервиса), если порт закрыт, сервер вернёт сообщение об ошибке типа ICMP «Destination Unreachable».
