# Тестовое задание для компании «OPTIMAL CITY Technologies»
## Задача
Необходимо написать приложение-чат бота. В основном окне должен быть расположен чат-виджет, где пользователь сможет набирать сообщение боту и получать от него ответы. При открытии окна бот должен поприветствовать пользователя, после чего вывести ему кнопки с вариантами продолжения дальнейшего общения. Например “Привет! Что я могу для Вас сделать?” и кнопки с вариантами “Заказать пиццу”, “Установить будильник”, “Вывести погоду”. При нажатии на кнопку реплика (со стороны пользователя) должна появиться в виджете так, как если бы пользователь набрал текст кнопки вручную. Бот должен ответить что то вроде “Хорошо, я закажу пиццу. Что еще могу сделать?”.

Как это примерно может выглядеть: https://lucasbassetti.com.br/react-simple-chatbot/#/

Бота нет необходимости делать сколь-либо интеллектуальным. Также нет необходимости использовать внешние сервисы или сетевые запросы. Весь интеллект бота можно захардкодить блоками if прямо в приложении.

Общение только текстом, без картинок, емодзи и т.п.

В задании, надо сконцентрироваться на визуальной части виджета, интерактивном процессе взаимодействия, кросс-браузерной верстке, удовлетворяющей современным требованиям (responsiveness и т.п.). Необходимо использовать фреймворке Vue.js, можно использовать любые css фреймворк, UI kit, и любые другие внешние библиотеки.

## Cтек
+ Vue
+ Scss
+ Bootstrap

## Развертывание проекта
- Скопируйте репозиторий
    - `https://github.com/eugened503/optimal-city-test.git`
- Установите пакеты
     - `npm install`
- Запустите версию для разработки (ее можно увидеть на локальном сервере)
    - `npm run dev`

## Деплой проекта
[Проект находится здесь](https://optimal-city-test-deploy.vercel.app/)

## Варианты добавление виджета на сторонние сайты
1. Использование сборки проекта. Команда npm run build создаст папку dist. Данную папку можно загрузить на хост или использовать для интеграции на сайт. Мы также можем подключить виджет к html-разметке через теги `<script>` и `<link>`.
2. Создание  npm-пакета. Для фреймворков можно сделать npm-пакет, т.е. чат-бот станет модулем, который можно импортировать, настраивать и т.д.
3. Модернизация под CRM. Виджет можно модернизировать под CRM сайта. Таким образом, появится возможность управлять сценариями (команда пользователя, действия бота).
