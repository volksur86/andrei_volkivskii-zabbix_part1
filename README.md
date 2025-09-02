# Домашнее задание к занятию "Система мониторинга Zabbix" - Волкивский Андрей

### Задание 1
Установите Zabbix Server с веб-интерфейсом.

Процесс выполнения
Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.
Установите PostgreSQL. Для установки достаточна та версия, что есть в системном репозитороии Debian 11.
Пользуясь конфигуратором команд с официального сайта, составьте набор команд для установки последней версии Zabbix с поддержкой PostgreSQL и Apache.
Выполните все необходимые команды для установки Zabbix Server и Zabbix Web Server.
Требования к результатам
Прикрепите в файл README.md скриншот авторизации в админке.
Приложите в файл README.md текст использованных команд в GitHub.

`Задание выполнял на виртуальной машине Ubuntu 24.04`
Вначале даем себе привилегии root:  sudo -s

1. `apt update   обновляем apt кэш`
2. `apt install postgresql   перед началом установки Zabix ставим PostgreSQL`
3. `wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.0+ubuntu24.04_all.deb скачиваем deb пакет, по ссылке которой нам подготовил конфигуратор`
4. `dpkg -i zabbix-release_latest_7.0+ubuntu24.04_all.deb  устанавливаем скачанный пакет`
5. `apt update`
6. `apt install zabbix-server-pgsql zabbix-frontend-php php8.3-pgsql zabbix-apache-conf zabbix-sql-scripts устанавливаем Zabbix сервер, веб-интерфейс`
7. `sudo -u postgres createuser --pwprompt zabbix  создаем пользователя zabbix`
8. `sudo -u postgres createdb -O zabbix zabbix  устанавливаем и запускаем сервер БД`
9. `zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix  импортируем начальную схему`
10. `DBPassword=password Отредактируем файл /etc/zabbix/zabbix_server.conf`
11. `systemctl restart zabbix-server apache2 ; systemctl enable zabbix-server apache2 запускаем процессы Zabbix сервера`
12. `После заходим на Zabbix через веб интерфейс http://localhost/zabbix/index.php`

<img width="1282" height="863" alt="image" src="https://github.com/user-attachments/assets/1b93d0ca-dafa-4f61-9aa6-5fefd7479bb9" />

<img width="1400" height="881" alt="image" src="https://github.com/user-attachments/assets/bfa32756-be98-426f-8a7e-71b82a17a370" />

---

### Задание 2
Установите Zabbix Agent на два хоста.

Процесс выполнения
Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.
Установите Zabbix Agent на 2 вирт.машины, одной из них может быть ваш Zabbix Server.
Добавьте Zabbix Server в список разрешенных серверов ваших Zabbix Agentов.
Добавьте Zabbix Agentов в раздел Configuration > Hosts вашего Zabbix Servera.
Проверьте, что в разделе Latest Data начали появляться данные с добавленных агентов.
Требования к результатам
Приложите в файл README.md скриншот раздела Configuration > Hosts, где видно, что агенты подключены к серверу
Приложите в файл README.md скриншот лога zabbix agent, где видно, что он работает с сервером
Приложите в файл README.md скриншот раздела Monitoring > Latest data для обоих хостов, где видны поступающие от агентов данные.
Приложите в файл README.md текст использованных команд в GitHub


`Перед началом работ склонировал первую виртуальную машину и установил на них Zabbix агента`

Список команд для установки агента:

1. `sudo -s  даем себе привилегии root`
2. `wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.0+ubuntu24.04_all.deb  устанавливаем репозиторий Zabbix`
3. `dpkg -i zabbix-release_latest_7.0+ubuntu24.04_all.deb  после обновляем apt кэш командой apt update`
4. `apt install zabbix-agent  устанавливаем Zabbix агент`
5. `systemctl restart zabbix-agent ; systemctl enable zabbix-agent  запускаем процесс zabbix агента `

Делаем то же самое на второй машине.
Редактируем файл zabbix_agentd.conf 
<img width="1335" height="820" alt="image" src="https://github.com/user-attachments/assets/0a2db143-c1cb-4d9d-960e-8124007c24af" />

<img width="1700" height="791" alt="image" src="https://github.com/user-attachments/assets/8ef32ada-bae9-41ba-891b-e9101dff1da2" />





---

