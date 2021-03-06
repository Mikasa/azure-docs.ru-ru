---
title: "Использование Twilio для голосовой связи, VoIP и SMS сообщений в Azure"
description: "Узнайте, как осуществлять телефонные вызовы и отправку SMS-сообщений с помощью службы Twilio API в Azure. Примеры кода написаны на Node.js."
services: 
documentationcenter: nodejs
author: devinrader
manager: wpickett
editor: 
ms.assetid: f558cbbd-13d2-416f-b9b1-33a99c426af9
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 11/25/2014
ms.author: wpickett
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 83c8f126073f62b4d0b2165a57eb1b9ee6bdcc57


---
# <a name="using-twilio-for-voice-voip-and-sms-messaging-in-azure"></a>Использование Twilio для голосовой связи, VoIP и SMS сообщений в Azure
В этом руководстве демонстрируется создание приложений, взаимодействующих с Twilio и node.js в Azure.

<a id="whatis"/>

## <a name="what-is-twilio"></a>Что такое Twilio?
Twilio представляет собой платформу API, облегчающую разработчикам процессы осуществления и приема вызовов, отправки и получения текстовых сообщений и внедрения функций VoIP-вызовов в приложения на основе браузеров и в собственные приложения.  Сначала вкратце рассмотрим принцип работы.

### <a name="receiving-calls-and-text-messages"></a>Прием вызовов и получение текстовых сообщений
Twilio позволяет разработчикам [покупать программируемые телефонные номера][purchase_phone], которые можно использовать для осуществления и приема вызовов, а также для отправки и получения текстовых сообщений.  При получении на номер Twilio входящего вызова или текстового сообщения Twilio отправит на веб-приложение запрос HTTP POST или GET на получение инструкций по обработке вызова или текстового сообщения.  Сервер отвечает на HTTP-запрос от Twilio с использованием [TwiML][twiml], простого набора XML-тегов, которые содержат инструкции по обработке вызова или текстового сообщения.  Чуть позже мы увидим примеры TwiML.

### <a name="making-calls-and-sending-text-messages"></a>Осуществление вызовов и отправка текстовых сообщений
Выполняя HTTP-запросы к API веб-службы Twilio, разработчики могут отправлять текстовые сообщения или совершать исходящие вызовы.  Для исходящих вызовов разработчику необходимо также указать адрес URL, который возвращает инструкции TwiML по обработке исходящего вызова после его подключения.

### <a name="embedding-voip-capabilities-in-ui-code-javascript-ios-or-android"></a>Внедрение возможностей VoIP в код пользовательского интерфейса (JavaScript, iOS и Android)
Twilio предоставляет клиентский пакет SDK, который может превратить в VoIP-телефон любой веб-браузер, приложение iOS или Android.  В данной статье основное внимание уделяется использованию VoIP-вызова в веб-браузере.  В дополнение к SDK Twilio JavaScript, исполняемому в обозревателе, должно использоваться серверное приложение (приложение node.js) для выдачи маркера "Возможность" для клиента JavaScript.  Дополнительные сведения об использовании VoIP с Node.js см. в [блоге разработчиков Twilio][voipnode].

<a id="signup"/>

## <a name="sign-up-for-twilio-microsoft-discount"></a>Подпишитесь на Twilio (скидка Microsoft)
Перед использованием службы Twilio необходимо сначала [зарегистрироваться для получения учетной записи][signup].  Клиенты Microsoft Azure получают специальную скидку — [не забудьте зарегистрироваться здесь][signup]!

<a id="azuresite"/>

## <a name="create-and-deploy-a-nodejs-azure-website"></a>Создание и развертывание веб-сайта node.js
Затем необходимо создать на Azure веб-сайт node.js.  [Официальная документация с описанием этого процесса находится здесь][azure_new_site].  В общих чертах будут выполняться следующие действия:

* Регистрация для получения учетной записи Azure в случае ее отсутствия
* Создание нового веб-сайта с помощью консоли администрирования Azure
* Добавление поддержки системы управления версиями (подразумевается, что используется git)
* Создание файла `server.js` с простым веб-приложением node.js
* Развертывание простого приложения в Azure

<a id="twiliomodule"/>

## <a name="configure-the-twilio-module"></a>Настройка модуля Twilio
Теперь начнем писать простое приложение node.js, которое использует интерфейс API Twilio.  Прежде чем начать, необходимо настроить учетные данные учетной записи Twilio.  

### <a name="configuring-twilio-credentials-in-system-environment-variables"></a>Настройка учетных данных Twilio в системных переменных среды
Для выполнения проверенных запросов к серверной части Twilio понадобится SID учетной записи и маркер проверки подлинности, которые будут использоваться как имя пользователя и пароль для учетной записи Twilio. Наиболее безопасный способ их настройки для использования с модулем узла в Azure – через системные переменные среды, которые можно установить непосредственно в консоли администрирования Azure.

Выберите веб-сайт node.js и щелкните ссылку "НАСТРОИТЬ".  Если немного прокрутить экран, появится область, где можно задать свойства конфигурации для приложения.  Введите данные учетной записи Twilio ([их можно найти на панели мониторинга Twilio][twilio_dashboard]), как показано на рисунке. Назовите их TWILIO_ACCOUNT_SID и TWILIO_AUTH_TOKEN соответственно:

![Консоль администрирования Azure][azure-admin-console]

После настройки этих переменных перезапустите приложение на консоли Azure.

### <a name="declaring-the-twilio-module-in-packagejson"></a>Объявление модуля Twilio в package.json
Далее нам нужно создать package.json для управления зависимостями модуля узла через [npm].  На том же уровне, что и файл "server.js", созданный в учебнике Azure/node.js, создайте файл с именем "package.json".  Содержимое файла должно быть следующим:

  { "name": "application-name", "version": "0.0.1", "private": true, "scripts": { "start": "node server" }, "dependencies": { "express": "3.1.0", "ejs": "*", "twilio":"*" } }

Этот код объявляет модуль Twilio как зависимость, а также популярную [веб-платформу express][express] и подсистему шаблонов EJS.  Итак, теперь все готово. Давайте напишем код!

<a id="makecall"/>

## <a name="make-an-outbound-call"></a>Осуществление исходящего вызова
Давайте создадим простую форму, которая будет осуществлять вызов на выбранный номер.  Откройте server.js и введите следующий код.  Обратите внимание на поле CHANGE_ME, в котором нужно указать имя веб-сайта Azure:

    // Module dependencies
    var express = require('express'), 
      path = require('path'), 
      http = require('http'), 
      twilio = require('twilio');

    // Create Express web application
    var app = express();

    // Express configuration
    app.configure(function(){
      app.set('port', process.env.PORT || 3000);
      app.set('views', __dirname + '/views');
      app.set('view engine', 'ejs');
      app.use(express.favicon());
      app.use(express.logger('dev'));
      app.use(express.bodyParser());
      app.use(express.methodOverride());
      app.use(app.router);
      app.use(express.static(path.join(__dirname, 'public')));
    });
    app.configure('development', function(){
      app.use(express.errorHandler());
    });

    // Render an HTML user interface for the application's home page
    app.get('/', function(request, response) {
      response.render('index');
    });

    // Handle the form POST to place a call
    app.post('/call', function(request, response) {
      var client = twilio();
      client.makeCall({
          // make a call to this number
          to:request.body.number,

          // Change to a Twilio number you bought - see:
          // https://www.twilio.com/user/account/phone-numbers/incoming
          from:'+15558675309',

          // A URL in our app which generates TwiML
          // Change "CHANGE_ME" to your app's name
          url:'https://CHANGE_ME.azurewebsites.net/outbound_call'
      }, function(error, data) {
          // Go back to the home page
          response.redirect('/');
      });
    });

    // Generate TwiML to handle an outbound call
    app.post('/outbound_call', function(request, response) {
      var twiml = new twilio.TwimlResponse();

      // Say a message to the call's receiver 
      twiml.say('hello - thanks for checking out Twilio and Azure', {
          voice:'woman'
      });

      response.set('Content-Type', 'text/xml');
      response.send(twiml.toString());
    });

    // Start server
    http.createServer(app).listen(app.get('port'), function(){
      console.log("Express server listening on port " + app.get('port'));
    });

Затем в этом каталоге создайте каталог с именем views и создайте файл с именем index.ejs, содержащий следующий код:

    <!DOCTYPE html>
    <html>
    <head>
      <title>Twilio Test</title>
      <style>
        input { height:20px; width:300px; font-size:18px; margin:5px; padding:5px; }
      </style>
    </head>
    <body>
      <h1>Twilio Test</h1>
      <form action="/call" method="POST">
          <input placeholder="Enter a phone number" name="number"/>
          <br/>
          <input type="submit" value="Call the number above"/>
      </form>
    </body>
    </html>

Теперь разверните веб-сайт в Azure и откройте домашнюю страницу.  Теперь вы можете ввести номер телефона в текстовом поле и получить вызов с вашего номера Twilio.

<a id="sendmessage"/>

## <a name="send-an-sms-message"></a>Отправка SMS-сообщения
Теперь попробуем настроить пользовательский интерфейс и логику обработки формы для отправки текстовых сообщений.  Откройте server.js и добавьте следующий код после последнего вызова app.post:

    app.post('/sms', function(request, response) {
      var client = twilio();
      client.sendSms({
          // send a text to this number
          to:request.body.number,

          // A Twilio number you bought - see:
          // https://www.twilio.com/user/account/phone-numbers/incoming
          from:'+15558675309',

          // The body of the text message
          body: request.body.message

      }, function(error, data) {
          // Go back to the home page
          response.redirect('/');
      });
    });

В views/index.ejs добавьте еще одну форму для указания номера и текстового сообщения:

    <form action="/sms" method="POST">
      <input placeholder="Enter a phone number" name="number"/>
      <br/>
      <input placeholder="Enter a message to send" name="message"/>
      <br/>
      <input type="submit" value="Send text to the number above"/>
    </form>

Повторно разверните приложение в Azure; теперь вы можете отправить форму, а также отправлять себе (или своим близким друзьям) текстовые сообщения.

<a id="nextsteps"/>

## <a name="next-steps"></a>Дальнейшие действия
Вы узнали основные сведения об использовании node.js и Twilio для создания приложений, поддерживающих связь.  Но эти примеры открывают лишь некоторые возможности Twilio и node.js.  Для получения дополнительных сведений об использовании Twilio с node.js обратитесь к следующим ресурсам:

* [Официальные документы по модулю][docs]
* [Руководство по использованию VoIP с приложениями Node.js][voipnode]
* [Votr — приложение для SMS-голосования в реальном времени с Node.js и CouchDB (три части)][votr]
* [Парное программирование в браузере с использованием Node.js][pair]

Надеемся, вам понравится работать с node.js и Twilio в Azure!

[purchase_phone]: https://www.twilio.com/user/account/phone-numbers/available/local
[twiml]: https://www.twilio.com/docs/api/twiml
[signup]: http://ahoy.twilio.com/azure
[azure_new_site]: /app-service-web/web-sites-nodejs-develop-deploy-mac.md
[twilio_dashboard]: https://www.twilio.com/user/account
[npm]: http://npmjs.org
[express]: http://expressjs.com
[voipnode]: http://www.twilio.com/blog/2013/04/introduction-to-twilio-client-with-node-js.html
[docs]: http://twilio.github.io/twilio-node/
[votr]: http://www.twilio.com/blog/2012/09/building-a-real-time-sms-voting-app-part-1-node-js-couchdb.html
[pair]: http://www.twilio.com/blog/2013/06/pair-programming-in-the-browser-with-twilio.html
[azure-admin-console]: ./media/partner-twilio-nodejs-how-to-use-voice-sms/twilio_1.png






<!--HONumber=Nov16_HO3-->


