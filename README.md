# baculum15
baculum15 settings on altlinux

# Установка Baculum

1. Baculum- web интерфейс Bacula. Установим необходимые пакеты на сервере:

```
apt-get install baculum9-postgresql
apt-get install baculum9-apache2
```

2. Далее включим сайты baculum-api.conf и baculum-web.conf в apache2:

```
a2ensite baculum-api
a2ensite baculum-web
```

3. Включаем следующие модули для работы baculum web c помощью команд:

```
a2enmod auth_basic
a2enmod authn_core
a2enmod authn_file
a2enmod authz_core
a2enmod authz_host
a2enmod authz_user
a2enmod mod_php7
a2enmod negotiation
a2enmod rewrite
a2enmod setenvif
```

4. Добавляем пользователя apache2 в группу bacula:

```
usermod -a -G bacula apache2
```

5. Запустим сервис httpd2:

```
systemctl start httpd2
```

6. Для работы с bconsole веб серверу даём право запускать необходимые бинарники без пароля:

```
control sudo public
echo "Defaults:apache2 "'!'"requiretty
> apache2 ALL=NOPASSWD: /usr/bin/bconsole
> apache2 ALL=NOPASSWD: /usr/sbin/bdirjson
> apache2 ALL=NOPASSWD: /usr/sbin/bsdjson
> apache2 ALL=NOPASSWD: /usr/sbin/bfdjson
> apache2 ALL=NOPASSWD: /usr/sbin/bbconsjson" > /etc/sudoers.d/baculum
```

7. Перезапустим apache:

```
systemctl restart httpd2.service 
```

8. Переходим по ссылке http://IPадрес:9096 и попадаем в интерфейс настройки baculum-api:
Логин по-умолчанию: ```admin```
Пароль по-умолчанию: ```admin```

Выбираем язык --> Настраиваем доступ к базе данных

Database type: PostgreSQL
Database name: bacula
Login: bacula
Password: пароль от базы(по умолчанию пустой)
IP address (or hostname): localhost
Port:5432

##Внимание! Необходимо поставить галочку Use sudo где это возможно.


Настраиваем интерфейс для выполнения команд в bconsole:

Bconsole binary file path: /usr/bin/bconsole
Bconsole admin config file path:/etc/bacula/bconsole.conf


Настраиваем Config API

General configuration

Directory path for new config files: /etc/bacula/new - перед применением настроек нужно создать эту директорию и установить права пользователю и группе apache2.

# mkdir /etc/bacula/new
# chown apache2:apache2 /etc/bacula/new

Director

bdirjson binary file path:/usr/sbin/bdirjson 
Main Director config file path (usually bacula-dir.conf): /etc/bacula/bacula-dir.conf


Storage Daemon

bsdjson binary file path:/usr/sbin/bsdjson
Main Storage Daemon config file path (usually bacula-sd.conf): /etc/bacula/bacula-sd.conf


File Daemon/Client

bfdjson binary file path:/usr/sbin/bfdjson
Main File Daemon config file path (usually bacula-fd.conf): /etc/bacula/bacula-fd.conf


Bconsole

bbconsjson binary file path: /usr/sbin/bbconsjson
Admin Bconsole config file path (usually bconsole.conf): /etc/bacula/bconsole.conf<br>

Далее выберите Use HTTP Basic authentication введите логин пароль. Save.

Переходим по ссылке http://IPадрес:9095 попадаем в интерфейс настройки baculum-web:
Выбираем язык --> Вводим API логин пароль Use HTTP Basic authentication остальное оставим по умолчанию --> Далее вводим логин пароль
