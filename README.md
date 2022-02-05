#!/usr/bin/python3

### Импорт библиотек ### 
import pandas as pd
import telebot
import re

### Константы ### 
# Имя файла с базой
BASE_FILE_NAME = 'base_cargo.csv'
# Файл, содержащий api-key
AUTH_FILE_NAME = 'auth_info.txt'
# Сообщение-приветствие
WELCOME_MESSAGE = f'''Вас приветствует информационный сервис службы доставки Che-logistic.
Здесь Вы можете узнать статус Вашего отправления. Для этого просто введите его номер в поле ввода сообщения.
Напоминаем, что номер отправления был отправлен Вам смс-сообщением и начинается с цифр 38.
<b>Che-logistic - доставим даже слона!</b>'''
# Сообщение об ошибке
ERROR_MESSAGE = "Для получения информации о статусе отправления введите, пожалуйста, верный номер отправления."

### Функции ###
def cheking_code(code):
    '''
    cheking_code - функция для проверки кода посылки
        вход:
            code - код, полученный от пользователя
        выход:
            Булевое значение
    '''
    def get_control_sum(code):
        '''
        get_control_sum - рекурсивная функция получения контрольной суммы
            вход:
                code - код, полученный от пользователя
            выход:
                n - контрольная сумма
        '''
        code = str(code)
        n = sum([int(i) for i in code])
        if n > 9:
            n = get_control_sum(n)
        return str(n)
        
    if (code[:2] == '38') and (get_control_sum(code) == code[3]):
        return True
    else:
        return False
    
def answer(code):
    '''
    answer - функция для формирования ответа пользователю
        вход:
            code - код, полученный от пользователя
        выход:
            answer_message - сформированный ответ пользователю
    '''
    base_code = base[base.id==code]
    if base_code.status.iloc[0] == 'sorting':
        answer_message = f'Ваше отправление с номером {code} находиться в сортировочном центре в {base_code.current_location.iloc[0]}\
 и пребудет в пункт назначения через {base_code.delivery_time.iloc[0]}.'
    elif base_code.status.iloc[0] == 'waiting for recipient':
        answer_message = f'Ваше отправление с номером {code} ждет Вас в пункте выдачи в {base_code.current_location.iloc[0]}.'
    elif base_code.status.iloc[0] == 'delivered':
        answer_message = f'Ваше отправление с номером {code} было получено в {base_code.current_location.iloc[0]}.'
    else:
        answer_message = f'К сожалению, нам не удалось найти отправление с номером {code}.\
 Дополнительную информацию вы можете получить по телефону 9-900-900-99-99.'
    return answer_message
  
### Создание и описание работы бота ###
# Читаем файл с базой
base = pd.read_csv(BASE_FILE_NAME, dtype = {'id':str})

# Читаем файл с ключем
with open(AUTH_FILE_NAME, 'r', encoding='utf-8') as f:
    auth_info = f.readline()
    
# Создание бота
bot = telebot.TeleBot(auth_info)

# Обработка ввода команды "/start"
@bot.message_handler(commands=['start'])
def welcome(message):
    '''
    welcome - реакция бота на команду "/start"
        выход:
            исходящее приветственное сообщение
    '''
    # Создаем привествие для конкретного пользователя
    user_welcom = f'Здравствуйте, <b>{message.from_user.username}</b>!\n'
    # Отправляем приветственное сообщение
    bot.send_message(message.from_user.id, user_welcom + WELCOME_MESSAGE, parse_mode="HTML")

# Обработка сообщения от пользователя
@bot.message_handler(content_types=['text'])
def get_text_messages(message):
    '''
    get_text_messages - функция ответа пользователю
        вход: 
            message - входящее боту сообщение
        выход:
            Исходящее пользователю сообщение с информацией из базы
    '''
    # Фильтрация всех символов кроме цифр
    if bool(re.search( r'\D', message.text)):
        # Сообщение пользователю об ошибке номера
        bot.send_message(message.from_user.id, ERROR_MESSAGE)
    elif cheking_code(message.text): # Проверка введенного пользователем номера
        # Формирование ответа
        answer_message = answer(message.text)
        # Отправка ответа
        bot.send_message(message.from_user.id, answer_message)
    else:
        # Если номер не прошел проверку, сообщаем об ошибке
        bot.send_message(message.from_user.id, ERROR_MESSAGE)
        
# Непрерывное получение ботом сообщения с сервера
bot.polling(none_stop=True, interval=0)
