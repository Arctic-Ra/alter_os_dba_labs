## Отчет по лабораторной работе №1
**Группа ИС-21, Мавлютов Р. Э.**

 *1. Подготовка среды*
 
> Для проведения виртуализации была выбрана среда VirtualBox  
> В качестве базового образа операционной системы был использован актуальный дистрибутив, поддерживающий все необходимые команды и пакеты для выполнения требуемых операций.
> 
> После запуска виртуальной машины была произведена установка ОС и дальнейшая проверка и установка всех необходимых репозиториев и пакетов:  
> Команда `apt-update` обновляет данные о доступных актуальных пакетах и репозиториях  
> Команда `apt-upgrade` в свою очередь производит установку этих пакетов.
> 
> Также после установки была изменена среда рабочего стола на Pantheon, с которым обычно поставляется ElementaryOS
> 
> <img src="/Lab 1/screens/1.png" title="Рабочее окружение среды Linux" width="auto" height="592.5"/> 

 *2. Установка PostgreSQL*

> Был установлен пакет postgresql  
> Команда `apt install` `названия пакетов` устанавливает необходимые пакеты и их зависимости из репозиториев конфигурации. 
> 
> <img src="/Lab 1/screens/2.png" title="Установка PostgreSQL" width="auto" height="592.5"/> 

 *3. Проверка служебной учетной записи*

> Учетная запись postgres — это системный пользователь, создаваемый автоматически при установке PostgreSQL. Она используется для управления сервером базы данных и обладает максимальными правами внутри PostgreSQL. Однако её права в операционной системе ограничены, и она не имеет привилегий администратора (root).
> 
> Проверяем наличие учетной записи командой `id postgres`  
> `id` выводит информацию о пользователе, включая его UID (идентификатор пользователя), GID (идентификатор группы) и группы, к которым он принадлежит.
> 
> <img src="/Lab 1/screens/3.png" title="Проверка служебной учетной записи" width="auto" height="592.5"/> 

 *4. Первичная настройка конфигурационных файлов*

> После установки postgres необходимо инициализировать базу данных для создания и генерации необходимых для ее функционирования файлов, в том числе файлов конфигурации.  
> После инициализации был отредактирован файл pg_hba.conf, который отвечает за взаимодействие с доступами и типами подключения. На соединение с ipv6 было установлено требование пароля.  
> В дальнейшем будет представлен пример редактирования и взаимодействия для хоста.
> 
> <img src="/Lab 1/screens/4.png" title="Первичная настройка конфигурационных файлов" width="auto" height="592.5"/>
>
> Далее в файле postgresql.conf, который является кофигурацией, хранящей в себе почти все возможные настройки базы данных, был изменен порт, на котором будет работать postgre.  
> Произведено подключение к postgre через обновленный порт.
> 
> <img src="/Lab 1/screens/5.png" title="Первичная настройка конфигурационных файлов" width="auto" height="592.5"/>

 *5. Управление сервисом*

> С помощью `systemctl status postgresql` было определно состояние - 'active (running)'  
> Сервис был добавлен в автозагрузку командой `systemctl enable postgresql` и проверен аргументом `is-enabled`
>
> <img src="/Lab 1/screens/6.png" title="Управление сервисом" width="auto" height="592.5"/>

 *6. Создание тестовой базы данных*

> Был создан пользователь mavlyutovre и база данных dbmavlyutovre.  
> Произведено подключение под пользователем через psql.
>
> <img src="/Lab 1/screens/7.png" title="Создание тестовой базы данных" width="auto" height="592.5"/>

 *7. Знакомство со схемами*

> Схему в postgresql можно сравнить с классами в ООП. Так, в базе данных мы можем иметь несколько классов (схем) со своими функциями, таблицами и прочим.
> 
> Была создана схема lab_1. Созданному ранее пользователю были предоставлены права на пользование схемой.
> 
> `search_path` определяет, в каких схемах по умолчанию без явного указания будут исполнены команды.
>   
> Был приведен пример обращения к схеме, а также смены search_path, результатом чего стало то, что выводятся только таблицы схемы lab_1
>
> <img src="/Lab 1/screens/8.png" title="Знакомство со схемами" width="auto" height="592.5"/>

 *8. Использование утилиты psql для базовых операций*

> Обращение к схеме идет через префиксную форму с точкой: `схема`.`таблица`
> 
> Была создана таблица в public схеме, над ней были выполнены все основные SQL-запросы  
> Так как в search_path прописана схема lab_1, то обращение по префиксу не требуется.
> 
> `\dt` демонстрирует все таблицы с привязкой к схеме.
> 
> <img src="/Lab 1/screens/9.png" title="Использование утилиты psql для базовых операций" width="auto" height="592.5"/>

 *9. Настройка локальных и сетевых подключений*

> Для доступа с другого хоста, был прописан сетевой интерфейс в listen_addresses, через который будут проходить соединения к postgre.
> Был разрешен коннект по следующим адресам: `10.0.2.15, localhost, 127.0.0.1`
>
> Для пула, находящегося в подсети 10.0.2.xx через pg_hba было задано условие подключения по паролю md5.
> Для всех пользователей, для всех баз данных, для конкретного типа подключений.
> 
> <img src="/Lab 1/screens/10.png" title="Настройка локальных и сетевых подключений" width="auto" height="592.5"/>
>
> После было произведено подключение по данному хосту, для установления соединения СУБД потребовала пароль.

 *10. Журналирование (logging)*

> В конфиге базы данных было включено логгирование, а также установлен формат логов json.
> Сервис был перезапущен. После ввода команд отобразились записи в лог. Выведен файл с логом.
>
> <img src="/Lab 1/screens/11.png" title="Журналирование (logging)" width="auto" height="592.5"/>

 *11. Назначение ролей и прав*

> Была создана роль limited_user с правами на чтение одной единственной таблицы.  
> Операция SELECT выполняется успешно, но при этом ограниченный пользователь не видит других таблиц в схеме, а также не может проводить операции редактирования в доступной ему таблице.
>
> <img src="/Lab 1/screens/12.png" title="Назначение ролей и прав" width="auto" height="592.5"/>
>
> Далее, ограниченной ролью были унаследованы права от mavlyutovre, после чего ему стали доступны все схемы, таблицы и возможность их редактирования.
>
> Выдача прав через GRANT может осуществляться только ролью, у которой есть на это право.
>
> <img src="/Lab 1/screens/13.png" title="Назначение ролей и прав" width="auto" height="592.5"/>
