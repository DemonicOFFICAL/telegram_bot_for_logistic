Демонстрационный телеграм-бот для нужд транспортной компании
Цель проекта
Реализация простейшего телеграм-бота на языке программирование python с использованием библиотеки pyTelegramBotAPI

Цель программы
Информировать клиента вымышленной транспортной компании Che-logistic о месте нахождения отправления (посылки) клиента в конкретный момент времени.

Как начать
Скачать и установить python версии 3.10.2;
Установить библиотеку pandas;
Установить библиотеку pyTelegramBotAPI (тестирование проводилось на версии 3.8.2);
Установить библиотеку telebot (тестирование проводилось на версии 0.0.4);
Создать телеграм-бота с помощью BotFather и получить API-key;
Скачать с репозитория файлы Che_inform.py и base_cargo.csv (проверить в скрипте Che_inform.py правильность пути к файлу base_cargo.csv, путь указан в переменной BASE_FILE_NAME);
Создать файл с именем auth_info.txt (или указать свое имя файла и путь в скрипте Che_inform.py в переменной AUTH_FILE_NAME) и записать в него полученный API-key (использовать кодировку файла UTF-8);
Запустить на исполнение файл Che_inform.py.
О проекте
Бот информирует любого спросившего о месте нахождения отправления (посылки) с определенным номером. Данные об отправлениях находятся в файле base_cargo.csv. Номера отправлений сгенерированы автоматически и имеют ключ (4-я цифра номера).

Задача бота:

получить номер отправления от пользователя;
проверить с помощью регулярного выражения, что пользователь отправил именно цифры;
проверить значение ключа номера отправления с контрольной суммой;
проверить наличие номера отправления в базе;
по результатам проверки вернуть пользователю сообщение об ошибке или место нахождение отправления.
Demonstration telegram-bot for the needs of a transport company
Project objective
Implementation of the simplest telegram-bot in the programming language python using the pyTelegramBotAPI library

Program objective
Inform the customer of the fictional transport company Che-logistic about the location of the parcel

Getting Started
Download and install python ver 3.9.7;
Install the library pandas;
Install the library pyTelegramBotAPI (testing on the ver 3.8.2);
Install the library telebot (testing on the ver 0.0.4);
Create a telegram-bot using BotFather and get API-key;
Download files from the repository Che_inform.py and base_cargo.csv (check in the script Che_inform.py the correct path to the file base_cargo.csv, the path is specified in the variable BASE_FILE_NAME);
Create a file named auth_info.txt (or specify your file name and path in the script Che_inform.py in the AUTH_FILE_NAME variable) and write the received API-key to it (use the UTF-8 file encoding);
Run Che_inform.py.
About the project
This project was developed as a demo guide for creating a telegram bot for the needs of the STEP Computer Academy. The bot informs any user about the location of a parcel with a certain number. The data about the parcels is located in the base_cargo.csv file. The parcel numbers are generated automatically and have a key (the 4th digit of the number).

The bot's task:

get the parcel number from the user;
check with a regular expression that the user sent exactly the numbers;
check the value of the parcel number key with the checksum;
check the availability of the parcel number in the database;
return an error message or the location of the parcel to the user.
